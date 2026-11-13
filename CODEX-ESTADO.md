# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 16:28** · publicado em **13/11/2026 16:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 16:28 · sessao 2177abea7475 · - · - · origem=Ronald (tmux)

Leitura concluída. Não localizei AGENTS.md na busca por `rg --files -g AGENTS.md`; instruções em ancestrais fora da árvore [não verificado].

**Incompatibilidade central entre anexo e código atual**

- `diff_e3_sombra.py` não escreve `PAR_DIF` nem arrays `[copia, base]`: calcula agregado e o imprime como JSON (`app/chamados/management/commands/diff_e3_sombra.py:108`, `:118`, `:148`). Define `atual` como C17 e `futuro` como tarefa (`:127-130`), mas essa definição não confirma a ordem da versão do instrumento que produziu o anexo.
- A tarefa atual não pode escrever fio `chamado`: escolhe somente `colab`, `admin`, `sistema`, `nao_sei`, `nada` (`app/chamados/services/tarefa_do_dia.py:553-554`); a projeção pública documenta porta com fio `colab` ou `admin` (`:559-565`). O anexo atribui `chamado` à cópia; procedência da execução [não verificado].
- Na mesma alimentação, a fila da tarefa é subconjunto da fila C17: tarefa toma portas inteiras das ações ∩ fila global, depois ∩ chamados da célula, e filtra visíveis (`app/chamados/services/tarefa_do_dia.py:548-551`); C17 usa todos os chamados da célula e filtra visíveis (`app/chamados/juizes.py:1677-1688`). Isso impede justificar ampliações do anexo como uma identidade comprovada destes dois corpos sobre a mesma alimentação; alimentações efetivamente executadas [não verificado].

**Quem escreve e lê**

| Campo | Escritor lógico atual | Leitor real |
|---|---|---|
| fio | `_portas_da_tarefa`, prioridade de donos das ações (`app/chamados/services/tarefa_do_dia.py:552-556`); `porta_da_tarefa` só projeta (`:565`) | folha `_fio_da_porta` (`app/folha/fora_da_folha.py:86-89`); vigia `censo_alcance` (`app/chamados/services/vigia_fio.py:648-655`) |
| fila | ações com porta, pertencimento à fila, vínculo com célula e visibilidade (`app/chamados/services/tarefa_do_dia.py:548-556`) | folha `o_que_segura` converte para lista de chamados mostrados (`app/folha/fora_da_folha.py:233-242`) |
| rota | tarefa: tem porta exigível da célula → `porta_fila`, só âncora exigível → `porta_pergunta`, nenhuma → `None` (`app/chamados/services/tarefa_do_dia.py:548-556`) | vigia soma por rota e lista (`app/chamados/services/vigia_fio.py:652-655`) |
| sem_porta | não é coluna persistida: o comparador fabrica `porta is None` (`app/chamados/management/commands/diff_e3_sombra.py:56`) | CXR aplica diretamente `porta_da_tarefa(...) is not None` para excluir o dia (`app/escala/services/cadastro_realidade.py:492-498`) |
| sem_fio | não é coluna persistida: comparador fabrica `porta is None` (`app/chamados/management/commands/diff_e3_sombra.py:59`) | pauta lê `_fio_do_dia` e compara ao `FIO_SEM_PORTA` (`app/ponto/services/pautas_dos_discordantes.py:64`, `:79-84`) |

Escritor lógico C17 conservado no HEAD: fio por verbo, prioridade `cobrar→colab`, `validar→admin`, demais→`chamado` (`app/chamados/juizes.py:1624-1639`); fila/rota/verbos/fio retornados juntos (`:1677-1688`). Esses são bytes da cópia, não comprovação dos bytes de `main` antes da E3 [não verificado].

Os nomes de leitores no comparador não significam execução deles: a função diz expressamente “não executa os censos nem suas outras decisões” (`app/chamados/management/commands/diff_e3_sombra.py:47`).

**Discordância na mesma cópia**

Nenhum grupo do anexo demonstra a contradição exemplificada pelo usuário [não verificado quanto à execução real]. Neste comparador, todos os “leitores” são projeções da mesma variável `porta`: fio da folha e do vigia vêm do mesmo valor (`app/chamados/management/commands/diff_e3_sombra.py:49`, `:54`); `sem_porta` e `sem_fio` são exatamente o mesmo teste (`:56`, `:59`). Se a porta tem `fio='chamado'`, sua rota é projetada da mesma porta, não `None` (`:49-55`).

Leitores reais também chamam a mesma autoridade: folha `porta_da_tarefa(carga.tarefa(...))` (`app/folha/fora_da_folha.py:71-74`), vigia (`app/chamados/services/vigia_fio.py:640-641`), CXR (`app/escala/services/cadastro_realidade.py:492-497`), pauta via folha (`app/ponto/services/pautas_dos_discordantes.py:64`, `:79-82`). Ainda faltariam execuções comparáveis, mesma entrada/instante/estado, para afirmar igualdade efetiva entre cargas independentes [não decidível]. Lei aplicável: L-002 (`app/docs/LEIS.md:60`) e propriedade “todo leitor lê o mesmo número” de L-110 (`app/docs/LEIS.md:136`).

O vigia real, quando não há porta, ainda pode classificar dia de hoje/futuro ou balde de lei, em vez de contar fio morto (`app/chamados/services/vigia_fio.py:656-668`). O comparador não mede esses ramos (`app/chamados/management/commands/diff_e3_sombra.py:47`).

**Semânticas**

- Dia da tarefa é **dia do turno** (`app/chamados/services/tarefa_do_dia.py:573-583`); `data_evento` é explicitamente esse dia, sem reinterpretar como dia civil (`:219-222`, `:238`).
- `data_civil` é separado, calculado pelo instante local do marco (`app/chamados/services/tarefa_do_dia.py:94-105`). Logo uma mudança turno/civil não pode ser inferida de um `PAR_DIF` sem perguntas/marcos [não decidível].
- A tarefa busca perguntas por `data_evento` na janela e agrupa por `(colaborador, data_evento)` (`app/chamados/services/tarefa_do_dia.py:375-382`). C17 lê perguntas associadas por `celula_id` (`app/chamados/juizes.py:1599-1604`). Célula/pergunta mal associadas poderiam ser relevantes, mas o anexo não traz identidades dessas perguntas nem seus dias [não decidível].
- `fila_ids` é global: itera `qs_na_fila()` sem recorte de datas; só `fila_cel` é restrita às células fornecidas (`app/chamados/juizes.py:1557-1567`). A partição de verbos é sobre todos os vivos, também sem janela (`:1558`). Isso é deliberado para preservar crédito do painel (`:1577-1583`).
- A tarefa primeiro restringe `_fila` às âncoras de suas perguntas (`app/chamados/services/tarefa_do_dia.py:415-418`) e depois usa a alimentação global para expor as portas efetivamente exigíveis (`:529-534`, `:548-556`). Não é fila global de um lado versus fila temporal de outro.
- Fila significa vivos cobrando e não silenciados no instante (`app/chamados/services/painel.py:158-190`), enquanto a tupla pública significa portas visíveis da própria célula (`app/chamados/services/tarefa_do_dia.py:550-551`; `app/chamados/juizes.py:1654-1657`). Confundir tupla pública com todas as âncoras seria significado incorreto; o código da folha preserva essa distinção (`app/folha/fora_da_folha.py:235-239`).

**Classes**

Atribuidor é `casos_do_par`, não os leitores (`app/chamados/management/commands/diff_e3_sombra.py:63-70`):

- `todos`: sempre;
- `so_fila_sem_pergunta`: fila da célula não vazia e nenhuma pergunta viva C6;
- `fila_A_pergunta_B` e `verbos_A_B`: sempre juntos, se há fila da célula e âncora de pergunta viva na fila fora daquele conjunto;
- `isento_fila_residual`: tarefa com razão `isento_ponto` e fila da célula.

São etiquetas sobrepostas; o laço contabiliza uma linha em cada etiqueta aplicável (`app/chamados/management/commands/diff_e3_sombra.py:131-139`). A classe `so_fila_sem_pergunta` não prova ausência de perguntas brutas/decisões nem identifica dono L-099: “vivas” vem do C6 (`app/chamados/juizes.py:1599-1604`).

## 13/11 16:28 · sessao 5c944161d1b8 · - · - · origem=Ronald (tmux)

O código disponível não confirma o formato nem a ordem declarados no anexo: `diff_e3_sombra.py` escreve uma mensagem de limite e JSON agregado, sem emitir `PAR_DIF`, `PAR_TOTAL` ou listas `[valor_copia, valor_base]` (`app/chamados/management/commands/diff_e3_sombra.py:107-108,118-148`). Seu lado `antes` é C17; `depois` é a projeção da tarefa (`:126-130`). A versão da sonda que produziu o anexo e sua associação ao HEAD/base ficam [não verificado].

Há incompatibilidade concreta: `tarefa_do_dia` calcula `fio` entre `colab`, `admin`, `sistema`, `nao_sei`, `nada`, nunca `chamado` (`app/chamados/services/tarefa_do_dia.py:547-556`). O anexo atribui `chamado` à posição declarada “cópia”; portanto essa posição não é confirmável como saída da tarefa desta árvore. `porta_da_tarefa` apenas retorna os campos quando existe rota (`:559-565`).

| Aspecto | C17 preservado na árvore | Tarefa da cópia |
|---|---|---|
| Existência | Todo chamado da célula em `fila_cel` entra; perguntas acrescentam âncoras se vivas, com texto válido e no app (`app/chamados/juizes.py:1677-1684`). | Somente portas inteiras de ações exigíveis, intersectadas com `fila_ids` (`app/chamados/services/tarefa_do_dia.py:547-548`). |
| Fila | Todos os chamados da célula visíveis na lista padrão (`app/chamados/juizes.py:1685-1688`). | Portas das ações exigíveis pertencentes à célula, visíveis na lista padrão (`app/chamados/services/tarefa_do_dia.py:550-551`). |
| Rota | `porta_fila` quando há qualquer `_fila`; senão `porta_pergunta` (`app/chamados/juizes.py:1687`). | `porta_fila` quando há porta exigível da célula; senão `porta_pergunta` quando há outra porta exigível (`app/chamados/services/tarefa_do_dia.py:555`). |
| Fio | Escada dos verbos: cobrar→colab, validar→admin, demais→chamado (`app/chamados/juizes.py:1624-1639,1688`). | Donos das ações: colaborador vence admin, que vence sistema (`app/chamados/services/tarefa_do_dia.py:552-556`). |

A implementação C17 acima é a cópia preservada do caminho antigo, não prova da versão exata de `main` anterior à E3 [não verificado].

As regras escritas são explícitas: somente fila sem pergunta não cria porta humana; fila A com pergunta sob B publica B e descarta A residual; verbo é lido do painel; isento com fila residual não tem ação ou porta (`app/chamados/tests/test_e3_portas_tarefa.py:1-8`). Os testes documentam:

- Só fila: tarefa `rota=None`, `fila=()`, fio `sistema`, ação `perguntar/emissor` (`:32-38`). O contraste explícito mostra C17=`porta_fila` versus tarefa sem porta (`app/chamados/tests/test_e3_leitores_c17_chamadores.py:93-103`).
- A/B: tarefa `porta_pergunta`, fila vazia e verbo somente de B (`app/chamados/tests/test_e3_portas_tarefa.py:40-47`).
- Isento: sem rota, fila, verbos ou ações (`:57-65`).

Essas direções são contrárias à leitura “cópia→base” declarada para exemplos do anexo. Não se deve inverter silenciosamente as colunas: falta a implementação que emitiu os pares [não decidível].

As classes são atribuídas por `casos_do_par` (`app/chamados/management/commands/diff_e3_sombra.py:63-70`):

| Classe | Predicado |
|---|---|
| `todos` | Sempre (`:68`). |
| `so_fila_sem_pergunta` | Fila da célula não vazia e nenhuma pergunta viva C6 (`:64-68`). |
| `fila_A_pergunta_B` | Existe fila da célula e âncora de pergunta viva na fila global fora dessa fila da célula (`:66-69`). |
| `verbos_A_B` | Exatamente o mesmo predicado da anterior; sempre são emitidas juntas (`:69`). |
| `isento_fila_residual` | `tarefa.razao == isento_ponto` e fila da célula não vazia (`:70`). |

São classes sobrepostas; a soma de ocorrências por classe não é a quantidade de colab-dias distintos (`:68-70`).

O instrumento projeta uma mesma variável `porta` para os quatro consumidores; não executa censos nem suas outras decisões (`app/chamados/management/commands/diff_e3_sombra.py:46-59`). Assim, o anexo mede projeções declaradas, não comprova execução concordante ou discordante dos leitores reais. Campos ausentes da diferença tampouco significam `None`: a comparação registra somente campos diferentes (`:133-139`).

A alimentação lê fila global para `fila_ids`, mas recorta `fila_cel` às células pedidas (`app/chamados/juizes.py:1557-1567`). Verbos são particionados no universo vivo global, pois créditos dependem do conjunto (`:1575-1583`), e `qs_na_fila` exclui estados fora de cobrança e silêncio vigente (`app/chamados/services/painel.py:158-190`).

A tarefa utiliza `data_evento` exclusivamente como dia do turno (`app/chamados/services/tarefa_do_dia.py:219-241`), carregando perguntas da janela (`:375-382`). O C17 recolhe perguntas por vínculo `celula_id` (`app/chamados/juizes.py:1599-1604`). Seu teste “no app” não recebe `agora` e chama `perguntas_do_app` (`:1610-1620`); o próprio comparador declara que esse lado usa relógio da máquina (`app/chamados/management/commands/diff_e3_sombra.py:107`).

A lei de julgamento é L-110: comparar com a regra aplicada ao cadastro e batidas; DIFF mede impacto e não certifica o caminho novo (`CLAUDE.md:45-58`). Sem fatos de entrada individuais e sem o emissor dos pares, classificar cada assinatura como cópia certa/regressão fica [não decidível]. [nao verificado]

## 13/11 16:27 · sessao e3b2fb8631ff · - · - · origem=Ronald (tmux)

O dono L-099 é **[não decidível] para todos os grupos do anexo**: o juiz exige dados que nenhuma projeção comparada fornece. A ordem efetiva é:

| Condição | Dono | Referência |
|---|---|---|
| Folga não definida; sem vínculo no dia; sem vínculo em dia de trabalho | CADASTRO | `app/ponto/management/commands/e6_oraculo.py:98` |
| Batida fora de turno | BATIDA | `app/ponto/management/commands/e6_oraculo.py:104` |
| Número ímpar de batidas recebidas pelo dia | BATIDA | `app/ponto/management/commands/e6_oraculo.py:106` |
| Fase deslocada lavrada | CADASTRO | `app/ponto/management/commands/e6_oraculo.py:108` |
| Dia em cadastro × realidade do motor | CADASTRO | `app/ponto/management/commands/e6_oraculo.py:110` |
| Resíduo | ESTRUTURA | `app/ponto/management/commands/e6_oraculo.py:110` |

Faltam esses insumos **por colab-dia**; o carregador real lê batidas, turnos, cadastro, lavra de fase e cadastro × realidade em `app/ponto/management/commands/e6_oraculo.py:149`–`:195`. Zero batidas tem contagem par e não implica BATIDA (`app/ponto/management/commands/e6_oraculo.py:72`).

**Bloqueio de proveniência:** a ordem `[cópia, base]` dos registros PAR_DIF não pode ser confirmada pelo arquivo fornecido da cópia. `diff_e3_sombra.py` monta `antes=projecoes(atual)` e `depois=projecoes(futuro)` (`app/chamados/management/commands/diff_e3_sombra.py:127`–`:130`), mas não emite PAR_DIF; publica JSON agregado (`app/chamados/management/commands/diff_e3_sombra.py:107`–`:108`). O serializador do anexo falta **[não verificado]**.

Há contradição objetiva entre anexo e implementação caso se mantenha a ordem declarada: o primeiro elemento contém `fio="chamado"`, mas a tarefa só produz `colab/admin/sistema/nao_sei/nada` (`app/chamados/services/tarefa_do_dia.py:553`). A projeção só publica porta quando existe rota (`app/chamados/services/tarefa_do_dia.py:565`), e não transforma o fio. `chamado` é precisamente a alternativa do **C17 retido**, em `app/chamados/juizes.py:1628`–`:1639`. Isso não autoriza inverter silenciosamente o anexo.

As regras específicas existentes decidem cenários completos, não as assinaturas resumidas:

- Só fila sem pergunta: nenhuma porta humana e fila vazia (`app/chamados/tests/test_e3_portas_tarefa.py:3`; verificação `:32`–`:38`).
- Fila residual A e pergunta ancorada sob B: porta da pergunta, fila da célula vazia, verbo de B (`app/chamados/tests/test_e3_portas_tarefa.py:4`–`:7`; verificação `:40`–`:55`).
- Isento com fila residual: nenhuma ação, rota ou fio (`app/chamados/tests/test_e3_portas_tarefa.py:8`; verificação `:57`–`:65`).

A implementação escolhe portas das ações exigíveis, intersectadas com a fila, e o fio pelos donos das ações (`app/chamados/services/tarefa_do_dia.py:547`–`:556`); o C17 retido abre porta por chamado da célula na fila, independentemente dessa exigibilidade, e acrescenta âncoras de perguntas (`app/chamados/juizes.py:1677`–`:1688`).

Não atribuir “cópia certa” apenas porque mudou: L-110 diz que o código antigo não é gabarito e que o DIFF é impacto (`app/docs/LEIS.md:136`). L-114 repete que impacto não certifica, enquanto a igualdade entre leitores vale para qualquer dado (`app/docs/LEIS.md:152`). L-099 separa dono do dado da coerência estrutural (`app/docs/LEIS.md:122`).

## 13/11 16:22 · sessao c2fe3b531de7 · LEI-DESLIGADO-NAO-TRAVA · censo · origem=Code

As referências `q.q` e `r.r` abaixo usam linhas contadas em cada bloco fornecido, sem o cabeçalho do anexo.

1. **Bloqueante 1 — (e) para `juizes novos=0`; (a) para o censo incompleto dos leitores crus.**

   **Declaração `juizes novos=0`: (e), NÃO PERGUNTADO.** A pergunta trata de concorrência com juiz registrado; conferir a declaração de nascimento é outra verificação. O código contém a função e seu registro. (`q.q`, volta 1:4; `app/colaboradores/services/desligamento.py:23`; `app/core/juizes.py:267`)

   - Pergunta literal: “2o juiz que duplica juiz registrado (esta_desligado x algum juiz existente de situacao/ativo)”. (`q.q`, volta 1:4)
   - Resposta literal: “Não encontrei juiz **registrado** concorrente para ‘está desligado?’.” (`r.r`, volta 1:75)
   - Resposta apontando especificamente o **zero falso**: **ausente**. (`r.r`, volta 1:75; volta 2, item 5; localização original deste último: [nao verificado])

   **Censo dos leitores crus: (a), tema presente, levantamento incompleto.** A pergunta inclui expressamente `situacao`; a resposta identifica uma comparação crua, mas não apresenta o censo referido pelo veredito. (`q.q`, volta 1:3; `r.r`, volta 1:75; `veredito.md:2`)

   - Pergunta literal: “QUEM ESCREVE e QUEM LE cada campo/valor que o diff le ou muda (situacao, data_desligamento, o_que_segura, informativo_rescisao, retidos, C5 colabs_nunca_bateu, barrados_desligado)”. (`q.q`, volta 1:3)
   - Resposta literal: “Há comparação crua duplicada no formatador de situação exibida, mas isso não comprova uma segunda autoridade registrada.” (`r.r`, volta 1:75)

2. **Bloqueante 2 — (e), NÃO PERGUNTADO, quanto à presença do corte no commit.** [nao verificado]

   - Pergunta literal sobre conferir o corte em `CORTES.md` na cópia certificada: **ausente**. A pergunta próxima pede conferir o cabeçalho de bolha. (`q.q`, volta 3:5)
   - Resposta literal identificando a ausência desse corte: **ausente**; a resposta examina a inscrição da bolha e o predicado. (Anexo `r.r`, volta 3; linha original: [nao verificado])

   O selo exige a frase especificamente em `CORTES.md`; o comentário no registro não satisfaz essa busca. A cópia termina esse documento na linha 447. (`bin/tests/test_juiz_novo_tem_corte.sh:41`; `bin/tests/test_juiz_novo_tem_corte.sh:44`; `app/core/juizes.py:264`; `app/docs/CORTES.md:447`)

   A **execução VERMELHA** e o estado da árvore viva são a parcela **(c)**: constam do veredito fornecido; comprovação independente: **[nao verificado]**. (`veredito.md:3`)

3. **Bloqueante 3 — (e) para a auditoria financeira não perguntada; (c) para a comprovação produtiva.**

   - Pergunta literal sobre impacto `fora → entra`, reversão, hashes ou regeneração do TXT: **ausente**. (Anexos `q.q`, voltas 1–3; localização original: [nao verificado])
   - Resposta literal identificando esse bloqueante financeiro: **ausente**. (Anexos `r.r`, voltas 1–3; localização original: [nao verificado])
   - Trecho relacionado, sem apontar o bloqueante: “a montagem aceita fechamentos `aberto` e `aprovado`, selecionados pelo resultado `entra`.” (`r.r`, volta 1:15)

   A possibilidade de mudança era inspecionável no código: desligado recebe motivos vazios; motivos determinam `fora/furo_espelho`; o TXT seleciona `entra`. (`app/folha/export.py:1207`; `app/folha/export.py:970`; `app/folha/export.py:716`)

   Os **18 casos em outubro, um em novembro, competências exportada/aberta e ausência de reversão/hash** constituem a parcela **(c)**, sustentada pelo veredito fornecido; não aparecem demonstrados nas verificações anteriores. (`veredito.md:4`; comprovação independente: [nao verificado])

## 13/11 16:20 · sessao da8fd0b57352 · - · - · origem=Ronald (tmux)

Para o bloqueante 1, recomendo **(e) para a declaração falsa `juizes novos=0`**, distinguindo **(a) para o censo incompleto dos leitores crus**. A exigência do campo está em `CLAUDE.md:63`; a função consta em `app/colaboradores/services/desligamento.py:23` e seu registro em `app/core/juizes.py:264-267`.

**Declaração `juizes novos=0`: (e).** As perguntas fornecidas tratam duplicação de juiz registrado, sem pedir conferência desse campo da LEI-AKITA. O trecho mais próximo da pergunta é:

> “2o juiz que duplica juiz registrado (esta_desligado x algum juiz existente de situacao/ativo)”

A resposta correspondente é:

> “Não encontrei juiz **registrado** concorrente para ‘está desligado?’.”

O trecho da resposta que aponta a falsidade de `juizes novos=0` é **ausente**; descarte desse achado pela triagem também é **ausente**. Essa distinção se sustenta nos anexos fornecidos; referências de linha dos anexos: `[nao verificado]`.

**Censo de leitores crus: (a).** Pergunta literal da volta 1:

> “QUEM ESCREVE e QUEM LE cada campo/valor que o diff le ou muda (situacao, data_desligamento, o_que_segura, informativo_rescisao, retidos, C5 colabs_nunca_bateu, barrados_desligado): arquivo:linha”

Resposta literal:

> “Há comparação crua duplicada no formatador de situação exibida, mas isso não comprova uma segunda autoridade registrada.”

A resposta identifica uma comparação crua, porém o levantamento das 32 ocorrências referido pelo veredito é **ausente** na resposta fornecida. Contagem histórica de 32: `[nao verificado]`.

A referência `app/docs/RELATO.md:74` do veredito **não corresponde à obra nesta cópia**: essa linha trata outra obra; a busca literal por `LEI-DESLIGADO-NAO-TRAVA` no arquivo não retornou ocorrência. O conteúdo daquela linha no hash histórico alegado permanece `[nao verificado]`.

## 13/11 16:20 · sessao bced962f87f4 · - · - · origem=Ronald (tmux)

B3 deve separar duas parcelas para não atribuir ao Codex dados produtivos que ele não recebeu. A regra permite distinguir “fora do alcance” de “não perguntado”. (`CLAUDE.md:385`; `CLAUDE.md:386`)

- **(c), parcela produtiva:** os 18 casos de 10/2026, o caso de 11/2026, a situação exportada/aberta e a ausência de reversão/hash só constam do veredito fornecido; não foram demonstrados pelas perguntas/respostas anteriores. (`veredito.md`, item 3 fornecido; comprovação independente: **[nao verificado]**)
- **(e), pergunta sobre DINHEIRO:** nenhuma das três perguntas pede apuração da mudança `fora → entra`, reversão, hash antes/depois ou regeneração do TXT. Trecho literal da pergunta sobre esses pontos: **“ausente”**. Trecho literal da resposta identificando esse bloqueante financeiro: **“ausente”**. (Anexos `q.q`/`r.r`, voltas 1–3; **[nao verificado]** como arquivos locais.)
- **Não sustenta (b):** a resposta da volta 1 distingue trava de aprovação e emissão do TXT, mas não identifica a mudança de universo com consequências financeiras: “Isso **não demonstra impedimento de emissão da linha no TXT**: a montagem aceita fechamentos `aberto` e `aprovado`, selecionados pelo resultado `entra`.” (`r.r`, volta 1, achado 2 fornecido; `app/folha/export.py:687`; `app/folha/export.py:716`)

O efeito estático era inspecionável: o item nasce `entra`; os motivos o tornam `fora/furo_espelho`; desligado retorna motivos vazios; a montagem inclui os ids `entra`. Isso demonstra possibilidade condicional de entrada, sem provar quantidade produtiva ou conteúdo financeiro efetivamente alterado. (`app/folha/export.py:841`; `app/folha/export.py:968`; `app/folha/export.py:1207`; `app/folha/export.py:716`; `app/folha/export.py:726`)

As exigências documentadas de reversão, hash e frota estão em `CLAUDE.md:406–414`; isso é evidência da regra, sem provar que os artefatos existiam ou faltavam. (`CLAUDE.md:407`; `CLAUDE.md:408`; `CLAUDE.md:411`; `CLAUDE.md:412`; `CLAUDE.md:414`)

## 13/11 16:20 · sessao e475d03ac1aa · - · - · origem=Ronald (tmux)

Bloqueante 2: **(e) NÃO PERGUNTADO**, quanto à conferência do corte em `CORTES.md` na cópia certificada. A pergunta próxima trata do cabeçalho de bolha, enquanto o selo exige a frase em outro arquivo. (`bin/tests/test_juiz_novo_tem_corte.sh:38`; `bin/tests/test_juiz_novo_tem_corte.sh:41`; `app/colaboradores/services/desligamento.py:6`)

- **Pergunta sobre corte presente no commit:** ausente nas três perguntas fornecidas. `[nao verificado]` [nao verificado]
- **Resposta apontando corte ausente do commit:** ausente nas três respostas fornecidas. `[nao verificado]` [nao verificado]
- **Trecho próximo, pergunta da volta 3, item 3:** “O cabecalho de bolha de desligamento.py (@bolha/@entra/@sai/@invariante/@juiz-unico-de) diz a verdade sobre quem escreve/le (arquivo:linha)?” `[nao verificado]`
- **Trecho próximo, resposta da volta 3:** “`esta_desligado` corresponde ao predicado inscrito: lê somente `situacao`.” Isso verifica o predicado, sem apontar a falta do corte em `CORTES.md`. `[nao verificado]`

A cópia contém o registro e seu comentário em `app/core/juizes.py:264` e `app/core/juizes.py:267`; `app/docs/CORTES.md` termina na linha 447, sem a entrada mencionada no veredito. (`app/docs/CORTES.md:447`)

O selo consulta especificamente `CORTES.md` e acusa falha quando não encontra a frase; o comentário em `juizes.py` não substitui essa busca. (`bin/tests/test_juiz_novo_tem_corte.sh:41`; `bin/tests/test_juiz_novo_tem_corte.sh:44`)

**Distinção para a consolidação:** a execução VERMELHA é prova de suíte relatada pelo veredito, classe **(c)** quanto à execução; isso não transforma automaticamente a ausência estática do corte em assunto fora do alcance. Execução e estado da árvore viva externa: `[nao verificado]`. [nao verificado]

## 13/11 15:57 · sessao 07b93e1ddafe · CONTRATOS-ENTRADA · censo · origem=Code

**O censo não ficou fechado para TODO o módulo.** Abaixo estão as costuras rastreadas e os desencontros confirmados; total exaustivo de costuras, desencontros e ausência de selo permanece **[não verificado]**. Não usei como evidência de produção os números mencionados em comentários do código. **[não verificado]**

Método: busca textual e inspeção AST de `JSONField`, atribuições de dict, mutações posteriores, `contexto_json`, contexto de disputa, portas com dict/kwargs, eventos, auditoria e renderização; depois leitura dos consumidores e dos testes localizados. As raízes persistidas incluem `DiaPago.detalhe`, os dois JSON do fechamento e `Advertencia.detalhe_json`; portas e renderização ampliam esse universo. (`app/ponto/models.py:378`, `app/ponto/models.py:451`, `app/ponto/models.py:452`, `app/ponto/models.py:1173`, `app/ponto/registro_batida.py:94`, `app/ponto/views.py:193`)

A unidade adotada é **estrutura × módulo gravador × módulo leitor**, reunindo os sítios do mesmo módulo. Leitura integral, cópia e hash contam como consumo opaco; não equivalem à interpretação de uma chave. Há exemplos desses consumos no contexto de chamados, na emissão de disputa e no hash do fechamento. (`app/chamados/services/contexto.py:70`, `app/chamados/services/disputa_emissao.py:54`, `app/ponto/portas/regen_exportada.py:56`)

O método não demonstrou todos os aliases dinâmicos, os kwargs indiretos, todos os includes transitivos nem todos os escritores complementares externos. Também não demonstrou inexistência global de testes. **[não verificado]** [nao verificado]

Nas tabelas, os caminhos começam em `app/`; `P` significa `app/ponto/`, `C` significa `app/chamados/` e `T` significa `app/templates/`. `NV` significa **selo da costura não verificado**, e não prova de que nenhum teste exista. O dono indicado como **ESTRUTURA provável** é inferência sobre o desencontro entre contratos, pela distinção da L-099; não é classificação do dado de produção. (`CLAUDE.md:179`) [nao verificado]

**Desencontros comprovados nos pares abaixo.** Defaults e caminhos alternativos estão registrados; ocorrência ou consequência em produção permanece **[não verificado]**.

| Estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas | DESENCONTRO | Selo que prende | Dono provável | Classe |
|---|---|---|---|---|---|---|---|---|
| Disputa de selfie retroativa | `P/management/commands/marcar_foto_ausente_retro.py:98` | `foto_ausente.{tipo_batida,batida_id,batida_em,_por}` | `C/services/disputa_emissao.py:372`, `:387` | `[motivo].data`; `ultima_batida.id`, ambos com ausência tolerada | A referência gravada fica em outra posição; o leitor não consulta `foto_ausente.batida_id` | NV; o teste verifica a gravação, não este consumidor: `P/tests/test_bug126_selfie_invisivel.py:65` | ESTRUTURA | sim |
| Mesma disputa | mesmo emissor `:98` | mesmas chaves | `C/models.py:3464` | `ultima_batida.id`, default `None` | A identidade do nascimento não consulta a referência gravada | NV; limite do teste acima | ESTRUTURA | sim |
| Mesma disputa | mesmo emissor `:98` | mesmas chaves | `api/foto_recebida.py:67` | `ultima_batida={}`; `id=None` | O fechamento por upload separado procura outra posição | NV | ESTRUTURA | sim |
| Contexto `turno_aberto` | `P/management/commands/processar_alertas_turno.py:348` | `data_turno,entrada_timestamp,posto` | `C/services/disputa_emissao.py:54`, `:59`, `:68` | Funde os contextos; procura `entrada_sem_saida.entrada_id/entrada_aberta_em` ou `ultima_batida.id`; outra função lê `entrada_timestamp` em `:145` | `_data_furo_da_grade` não usa `entrada_timestamp`; as referências alternativas não vêm deste emissor | NV | ESTRUTURA | sim |
| Contexto `turno_aberto_24h` | `P/management/commands/processar_alertas_avancados.py:126` | `data_turno,entrada_timestamp` | mesmo consumidor `:54`, `:59`, `:68`, `:145` | mesmas chaves | Mesmo desencontro no helper de data | NV | ESTRUTURA | sim |
| Contexto `turno_aberto_24h` retroativo | `P/management/commands/emitir_turno_aberto_retroativo.py:128`, persistência `:156` | `data_turno,entrada_timestamp` | mesmo consumidor `:54`, `:59`, `:68`, `:145` | mesmas chaves | Mesmo desencontro no helper de data | NV; teste da presença de `entrada_timestamp`: `P/tests/test_passe_turno_aberto_retroativo.py:102` | ESTRUTURA | sim |
| `sem_previsao_escala.data_inicio` | `P/management/commands/detectar_ausencias.py:231`, `:532` | Primeiro `ec.data_inicio`; segundo `data_ref`, com causa `dia_sem_grade` | `C/catalogo/modulos.py:126`, `:225` | `data_inicio`, declarado início de período; default `None` | Mesmo nome para início do vínculo e dia da lacuna | NV | ESTRUTURA | sim |
| Entrada bloqueada | `P/views.py:280`, renovação `:288`, nascimento `:301` | `pendentes,total` | `C/catalogo/modulos.py:182`, `:225` | `data_turno`, default `None` | O catálogo pede uma chave que esse emissor não grava | NV | ESTRUTURA | sim |
| WebPush central | `P/webpush.py:44` | `title,body,url` | `static/sw.js:88`, `:102` | `title`/`body` com defaults; `urgente=false`; clique abre `/` | `urgente` não é gravada; o clique não usa `url` | NV | ESTRUTURA | sim |
| WebPush avançado N3 e DP | `P/management/commands/processar_alertas_avancados.py:207`, `:240` | `title,body,url,requireInteraction` | `static/sw.js:88`, `:102` | `title,body,urgente`; clique abre `/` | Emissor usa `requireInteraction`, leitor usa `urgente`; clique não usa `url` | NV | ESTRUTURA | sim |
| WebPush de turno | `P/management/commands/processar_alertas_turno.py:279` | `title,body,url,requireInteraction` | `static/sw.js:88`, `:102` | mesmas leituras | Mesmo desencontro | NV | ESTRUTURA | sim |
| Contexto do lote de fechamento | `P/views.py:1087` | `resumos,proximo_offset,restantes,qs_lotes` | `T/ponto/_fech_linha.html:13`, `:42`, `:47`, via include `T/ponto/_fech_lote.html:8` | `empresa_id,periodo_fechado`, além de `r` local | As duas raízes não vêm do render do lote; a página principal as fornece | NV; testes de lote conferem quantidade/ordem: `P/tests/test_selo_lotes_fechamento.py:57` | ESTRUTURA | sim |
| Contexto da triagem no seletor | `P/services/triagem_batida.py:206` | `tem_bloqueio_turno` | `P/views.py:229` | `.get('entrada_bloqueada_turno_aberto')` | Nomes diferentes para selecionar o ramo de regularização | NV; teste afirma a chave do produtor: `P/tests/test_f7_triagem_caracterizacao.py:78` | ESTRUTURA | sim |
| Contexto da triagem apta | `P/services/triagem_batida.py:758` | `sucesso,batida_id,tipo_batida,hora_registro,aviso_do_dia` | `T/ponto/triagem/apto.html:39` | `saida_sem_entrada` | Esse nome é gravado no contexto da disputa, não no contexto de tela inspecionado: `triagem_batida.py:738` | NV | ESTRUTURA | sim |
| Contexto do painel de ausência | `P/views.py:2079`, helpers `:2032`, `:2046` | `a,raiz,irmaos,cobre_parte_do_dia,tipos,parciais,pode_editar,escala_rotulo,status_fundo,status_cor,historico,resumo_historico,periodo_aquisitivo,pdf_historico_url` | `T/ponto/partials/painel_ausencia.html:282` | `volta_url`, além das raízes fornecidas | `volta_url` não é fornecida por esses escritores | NV; teste parcial do histórico: `P/tests/test_drawer_ausencia_no_molde.py:112` | ESTRUTURA | sim |

O exemplo informado, portanto, confere nas **três** costuras da selfie acima. Isso não permite dizer que `foto_ausente.batida_id` nunca é transportada: a emissão copia o contexto inteiro antes de interpretar suas referências. (`P/management/commands/marcar_foto_ausente_retro.py:98`, `C/services/disputa_emissao.py:54`, `C/services/disputa_emissao.py:387`, `C/models.py:3464`, `api/foto_recebida.py:67`)

Os esquemas persistidos de célula encontrados foram estes:

| Estrutura | Escritores e chaves |
|---|---|
| **ATA** | Construção em `P/services/cartorio.py:738`: `tipo_dia,lampadas,minutos_previstos,minutos_realizados,sem_ancora,n_orfas,orfas,n_missing,n_celulas`; cobertura parcial em `:772`; transformações de dispensa em `:808` e teto em `:915`. Persistência pela porta em `P/portas/celula.py:1064`, `:1091`; precedência em `:1213`; variante `ata=None` em `:1165`. |
| **L**, item de `ATA.lampadas` | `tipo,hora,acesa,luz,tipo_real`, nas variantes de `P/services/cartorio.py:672`, `:676`, `:678`, `:711`, `:722`; opcionais `isencao` em `:590`, `dispensada` em `:808`, `pendente` em `:915`. |
| **DNA** | Construtor `P/management/commands/gerar_celulas.py:83`, `:104`: `dna_versao,vinculo_id,tipo_escala_id,tipo_ciclo,marcos,minutos_jornada,intervalo_livre,lampadas`; `marcos` contém `hi,hf,hii,hfi`, com pausas adicionais condicionais; `extra_declarada.{antes,depois}` em `:131`. |
| Persistência do DNA | Mesmo módulo gravador, funções distintas: `P/portas/celula.py:261`, `:278`, `:294`, `:306`, `:638`, `:642`, `:827`, `:917`, `:925`. A reversão em `:827` restaura um blob histórico; esquema de todos esses históricos **[não verificado]**. |
| **DNA anterior** | Blob anterior gravado em `P/portas/celula.py:262`, `:307`; restauração e leitura de `marcos,vinculo_id` em `:824`. |
| **Precedência na ATA** | `P/services/cartorio.py:2000`: `tipo_do_dia,faixa,rotulo,suprimidos,era_dia_previsto,texto`; escrita pela porta `P/portas/celula.py:1213`. Leitor de negócio em outro módulo **[não verificado]**. |

Para as costuras de ATA abaixo, o gravador persistente é `P/portas/celula.py:1064`, `:1091`, com os escritores e variantes descritos acima. `—` significa nenhum desencontro nominal confirmado **nessa leitura**, não certificação global. Selo integral dos pares não indicado permanece **NV**.

| Estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas/defaults | DESENCONTRO | Selo | Dono provável | Classe |
|---|---|---|---|---|---|---|---|---|
| ATA | `P/portas/celula.py:1091` | ATA/L | `escala/services/leitor_celula.py:385`, `:436`; item `:119` | Campos da ATA; L, inclusive `isencao,dispensada`; default de `acesa` pela existência de `luz` | Opcionais de L não vêm de todas as variantes; `acesa=None` tem tratamento próprio | Parcial: `P/tests/test_carimbo_isencao_lampada.py:144`; `escala/tests/test_cobparc7_leitor_transporta_a_lampada.py:29` | ESTRUTURA | sim, formal |
| ATA | mesmo `:1091` | ATA/L | `P/juiz_batida.py:157` | `lampadas=[]`, `orfas=[]`; L `hora,tipo,acesa,luz,tipo_real` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/worklist.py:22` | `lampadas` ou `[]` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/services/diagnostico_escala.py:154` | L `hora,tipo,acesa,luz`; `bool(acesa)` | `None` perde a distinção de falso nesse retrato; consequência **[não verificado]** | NV | ESTRUTURA | não verificada |
| ATA | mesmo `:1091` | ATA/L | `P/services/esmeril_espelho.py:125` | `orfas=[]`; L `luz,hora,acesa`, defaults vazios | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/services/vigia_pareador_celula.py:74` | `n_orfas=0,orfas=(),lampadas=[]`; L `tipo,hora,acesa` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/services/cluster_espurio.py:27` | `orfas,lampadas`, ausência retorna `None`; L `acesa` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/services/dia_do_colab.py:30` | L `hora,tipo,acesa,luz`; `bool(acesa)` | Distinção `None/False` não aparece nesse booleano; consequência **[não verificado]** | NV | ESTRUTURA | não verificada |
| ATA | mesmo `:1091` | ATA/L | `P/services/vigia_entrada.py:50`, `:80`, `:123` | `lampadas`; L `tipo,acesa` | Usa `bool(acesa)`; consequência **[não verificado]** | NV | ESTRUTURA | não verificada |
| ATA | mesmo `:1091` | ATA/L | `P/management/commands/tripwire_celulas.py:264`, `:342` | `lampadas,orfas,tipo_dia,n_celulas`; L `hora,acesa` em `:172` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/management/commands/detectar_intervalo_ausente.py:64` | `lampadas`; L `tipo,hora,acesa,luz`; default de `acesa` por `luz` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/management/commands/ausencias_sem_efeito_na_celula.py:61` | `lampadas`; L `acesa` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/management/commands/curar_residuo_bug115.py:45`, `:148` | Existência de `lampadas` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `C/juizes.py:450`, `:965`, `:1354` | `tipo_dia,cobertura_parcial,lampadas,orfas,n_orfas`; L `hora,tipo,acesa,luz` | `cobertura_parcial` é condicional; ausência é tratada pelo leitor | NV | ESTRUTURA | sim, formal |
| ATA | mesmo `:1091` | ATA/L | `C/services/materializacao.py:172` | `lampadas=[],orfas=[]`; L `tipo,hora,luz,acesa` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `C/reconciliador.py:701`, `:879` | `lampadas`; repassa ATA inteira | Leitura opaca adicional | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `C/services/fabrica_da_celula.py:123` | Repassa ATA ao julgamento da parcial | Consumo transitivo; todas as chaves desse percurso **[não verificado]** | NV | ESTRUTURA | não verificada |
| ATA | mesmo `:1091` | ATA/L | `C/services/disputa_emissao.py:940` | Repassa ATA ao julgamento da parcial | Mesmo limite | NV | ESTRUTURA | não verificada |
| ATA | mesmo `:1091` | ATA/L | `pautas/services.py:432` | L `hora,acesa`; lista default `[]` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `relatorios/dia_da_frota.py:20`, `:61` | `lampadas`; L `tipo,hora,acesa` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `relatorios/cartao_pela_celula.py:287` | `orfas`; L `hora,isencao`; lista default `[]` | `isencao` é omitida na variante comum | NV | ESTRUTURA | sim, formal |
| ATA | mesmo `:1091` | ATA/L | `colaboradores/services/feriado_do_posto.py:97`, `:115` | `lampadas`; L `acesa` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `folha/previa.py:83` | `lampadas`; L `acesa,hora,tipo` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `folha/validacao_pdf.py:47` | `minutos_realizados=0` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `folha/services/sem_furo.py:20` | `lampadas`; L `acesa` | — | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/services/lastro.py:89`, `:128`, `:148` | L `hora,tipo,acesa,pendente` | `pendente` é condicional; portanto não é chave globalmente sem leitor | NV | ESTRUTURA | sim, formal |
| ATA | mesmo `:1091` | ATA/L | `P/management/commands/vigia_b5_ausencia.py:79` | Repassa ATA; destino `cartorio.insumos_da_ata` lê `lampadas,n_orfas` e L `tipo,hora,acesa` em `cartorio.py:834` | — no percurso identificado | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/management/commands/backfill_b5_insumos.py:73` | Mesmo repasse ao leitor `cartorio.py:834` | — no percurso identificado | NV | ESTRUTURA | não constatada |
| ATA | mesmo `:1091` | ATA/L | `P/turnos.py:1582`, `:1586`, `:1633` | Carrega célula e delega ao juiz da batida | Consumo transitivo da ATA | NV | ESTRUTURA | não constatada |

Para DNA, todos os escritores persistentes encontrados estão nas funções da porta citadas no inventário. A coluna de gravador abaixo aponta a regeneração; edição, reversão e geração do período também escrevem a mesma estrutura. (`P/portas/celula.py:261`, `:638`, `:827`, `:917`)

| Estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas/defaults | DESENCONTRO | Selo | Dono provável | Classe |
|---|---|---|---|---|---|---|---|---|
| DNA | `P/portas/celula.py:261` | DNA | `escala/models.py:1124`, `:1189`, `:1235` | `marcos`, todos os pares de pausa; `extra_declarada.antes/depois=0` | Extras são condicionais no produtor | NV | ESTRUTURA | sim, formal |
| DNA | mesmo `:261` | DNA | `escala/utils.py:53`, `:104`, `:137`, `:153` | `marcos`; `lampadas=[]`, itens `hora,instante` | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/services/cartorio.py:484`, `:615`, `:840` | `dna_versao,marcos`; `hi,hf` e pares de pausa | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/juiz_batida.py:158` | `marcos={}`, todos os pares de pausa | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/services/esmeril_espelho.py:116` | `marcos`, via `marcos_presentes` | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/services/veredito_celula.py:31` | `marcos`, todos os pares | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/services/diagnostico_escala.py:153`, `:172` | `marcos.hi/hf` | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/services/vigia_cadastro_futuro.py:60`, `:79` | `vinculo_id`; compara blob inteiro com `montar_dna` | Consumo nominal e opaco | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/services/dia_do_colab.py:46` | Todos os marcos | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/services/vigia_entrada.py:126` | `marcos.hi/hf,tipo_ciclo` | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/services/faltas_de_hoje.py:43` | `marcos.hi` | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/management/commands/detectar_intervalo_ausente.py:53` | Marcos de todos os pares | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/management/commands/tripwire_celulas.py:30`, `:99`, `:259` | Blob inteiro e `marcos` | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/management/commands/censo_col900.py:119` | `marcos` | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/management/commands/aplicar_col900.py:174` | `marcos.hi`; DNA anterior em `:175` | Esquema histórico completo **[não verificado]** | NV | ESTRUTURA | não verificada |
| DNA | mesmo `:261` | DNA | `P/management/commands/censo_fase_12x36.py:392` | Blob inteiro | Leitura opaca | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `pautas/services.py:424` | Todos os marcos | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `C/juizes.py:1348` | Marcos das pausas e chave do motivo | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `C/services/adesao.py:35` | `marcos`, via `marcos_presentes` | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `escala/management/commands/celulas_contra_o_template.py:49` | Existência de `marcos` | — | NV | ESTRUTURA | não constatada |
| DNA | mesmo `:261` | DNA | `P/turnos.py:1582`, `:1586`, `:1633` | Delega a `periodos_do_dia` | Consumo transitivo | NV | ESTRUTURA | não constatada |

Os dois JSON do fechamento têm escritor localizado em `services/fechamento.py`; não confundi o campo persistido `motivos_espelho` com outro dict transitório produzido pelo exportador. (`P/services/fechamento.py:1067`, `:1080`, `:1186`, `folha/export.py:972`)

| Estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas | DESENCONTRO | Selo | Dono provável | Classe |
|---|---|---|---|---|---|---|---|---|
| `causa_espelho` | `P/services/fechamento.py:1080`, `:1082`, `:1187` | `{causa,dias_furo,dias_previstos,evidencia}` ou `None`; origem `folha/causas.py:31` | `P/services/fechamento_lista.py:122` | Dict inteiro ou `None` | — | NV | ESTRUTURA | não constatada |
| mesma | mesmo escritor | mesmas chaves | `T/ponto/_fech_linha.html:16` | `causa`, default `zzz`; `evidencia` | Variante `None` é possível e a apresentação a admite | NV | ESTRUTURA | não constatada |
| mesma | mesmo escritor | mesmas chaves | `folha/retrato_folha.py:1113` | `get('causa')=None`, com guarda de tipo | — | Parcial: `folha/tests/test_retrato_folha_quadros.py:1356`; usa mock da leitura em `:1340` | ESTRUTURA | não constatada |
| mesma | mesmo escritor | blob inteiro | `P/portas/regen_exportada.py:56` | Campo inteiro no hash | Leitura opaca | NV | ESTRUTURA | não constatada |
| mesma | mesmo escritor | blob inteiro | `P/management/commands/lavrar_dias_pagos.py:70` | Campo inteiro no hash | Leitura opaca | NV | ESTRUTURA | não constatada |
| `motivos_espelho` | `P/services/fechamento

[truncado em 20000 de 59955 caracteres]

## 13/11 15:53 · sessao 475ff71b94a6 · CONTRATOS-ENTRADA · censo · origem=Code

**O exemplo confere:** o retro grava `foto_ausente.batida_id`; os leitores citados procuram `ultima_batida.id` (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`, `app/chamados/services/disputa_emissao.py:387`, `app/chamados/models.py:3464`).

**Não fechei o censo total solicitado.** Abaixo estão 23 costuras com desencontro nominal identificado, mais os escritores, leitores e contratos adicionais rastreados. A enumeração completa de todos os pares do módulo e sua contagem global permanecem **[nao verificado]**.

Método executado: busca dos JSONFields, construções e mutações de dicionários, parâmetros `**kwargs`, chamadas de portas, retornos compartilhados, eventos, auditoria, outbox, renderizações e consumidores Python/JS/templates; análise AST somente em memória e leitura dos corpos encontrados. Execução desse procedimento, ausência de alterações em arquivos e ausência de execução de testes: **[nao verificado]**. Os pontos de partida persistidos estão em `app/chamados/models.py:47`, `:1742`, `:1746`, `:2444` e `:3408`.

O método não comprova os conteúdos históricos, os caminhos efetivamente executados, reflexão ou todas as chaves calculadas. Há portas que recebem estruturas abertas: `app/chamados/services/contexto.py:62`, `:82`, `:131`, `app/chamados/services/disputa_emissao.py:548` e `app/chamados/models.py:3944`. Incidência e impacto em produção: **[nao verificado]**.

Nas tabelas, caminhos sem `app/` ou `bin/` são relativos a `app/chamados/`. `N*` significa **selo específico do par não localizado [nao verificado]**, e não prova de inexistência. “Classe sim” identifica repetição pelo formato/caminho do código; erro de negócio e incidência real continuam **[nao verificado]**. O dono provável ESTRUTURA decorre dessa natureza do desencontro; a separação dos donos está em `CLAUDE.md:179`.

| # / estrutura | Emissor arquivo:linha | Chaves gravadas | Consumidor arquivo:linha | Chaves lidas, inclusive defaults | DESENCONTRO | Selo que prende | Dono provável / é classe |
|---|---|---|---|---|---|---|---|
| D01 — contexto da disputa / foto retro | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; entrada pela porta `services/disputa_emissao.py:666` | `foto_ausente.{batida_id,batida_em,tipo_batida,_por}` | `services/disputa_emissao.py:372`, `:387` | `[motivo].data=None`; `ultima_batida.id=None`, com intermediários `{}` | A identidade gravada sob `foto_ausente` não é procurada nessas leituras | N*. O contexto canônico é exercitado em `tests/test_a2_data_evento_batida.py:22`; o retro específico, [nao verificado] | ESTRUTURA / sim |
| D02 — mesma estrutura | Mesmo emissor de D01 | Mesmas | `models.py:3464` | `ultima_batida.id=None`, intermediário `{}` | Mesmo desencontro de identidade | N* | ESTRUTURA / sim |
| D03 — mesma estrutura | Mesmo emissor de D01 | Mesmas | `app/api/foto_recebida.py:67` | `ultima_batida.id=None`, intermediário `{}` | A chave do retro não é procurada pelo consumidor do recebimento da foto | N* | ESTRUTURA / sim |
| D04 — dia do chamado humano | `services/abertura.py:183`, `:191`; chave resolvida em `catalogo/modulos.py:315` | Chave catalogada; para `ausencia_colab`, `data`, declarada em `catalogo/modulos.py:120` | `juizes.py:187`, `:197`, `:226` | `data_turno=None`, `batida_id=None`; depois contexto fraco | O lote não consulta a chave catalogada que o singular consulta em `juizes.py:114` | N*. `tests/test_form_catalogo.py:76` prende módulo/data, não essa paridade | ESTRUTURA / sim |
| D05 — dia da adesão | `services/adesao.py:79` | `adesao,data_inicio,periodo` | `juizes.py:187`, `:197`, `:226` | Mesmas de D04 | `data_inicio` não participa da leitura do lote | N* | ESTRUTURA / sim |
| D06 — referência da ausência humana | `services/abertura.py:183`, `:191` | Metadados da abertura e `data`; não fornece `ausencia_id` | `views.py:1512` | `ausencia_id=None` | O leitor pede a referência ausente nessa forma de emissão; sem ID, segue a lista geral em `views.py:1517` | N* | ESTRUTURA / sim |
| D07 — mesma referência | Mesmo emissor de D06 | Mesmas | `juizes.py:2331` | `ausencia_id=None` | Referência ausente nessa forma humana | N* | ESTRUTURA / sim |
| D08 — anexo da ausência humana | Mesmo emissor de D06 | Mesmas; não fornece `tem_documento` | `app/templates/chamados/partials/_card_atendimento.html:65`, `:70` | `tem_documento`, `ausencia_id` | O cartão oferece o anexo usando o ID que essa forma de emissão não fornece | N* | ESTRUTURA / sim |
| D09 — marco emitido pelo C2 | `reconciliador.py:708`; formato em `app/ponto/services/lastro.py:134` | `marcos_faltantes[].{hora,tipo,motivo='lampada_apagada'}` | `juizes.py:551`, `:576` | `motivo=None,hora=None,tipo=None`; qualquer motivo preenchido impede alternativa por tipo | Mesmo nome com dois significados: etiqueta da lâmpada e motivo da pergunta | N*. Escritor: `tests/test_emissor_nomeia_o_marco.py:54`; leitor: `tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:33` | ESTRUTURA / sim |
| D10 — marco reconstruído pelo comando | `management/commands/nomear_marco_por_lampada.py:109`; formato confirmado em `tests/test_emissor_nomeia_o_marco.py:159` | Mesmo formato de D09 | `juizes.py:551`, `:576` | Mesmas de D09 | Mesma colisão semântica, por outro emissor | N* | ESTRUTURA / sim |
| D11 — WebPush | `push_utils.py:28` | `title,body,url,requireInteraction=True` | `app/static/sw.js:89`, `:97`, `:102` | `title || 'Hasner Ponto'`; `body || 'Nova notificação'`; `urgente || false`; clique abre `/` | Emite `requireInteraction`, lê `urgente`; `url` não governa o clique | N*. `tests/test_push_fcm.py:106` confere chamada de envio | ESTRUTURA / sim |
| D12 — ticket formatado GLPI | `glpi_client.py:138` | `id,titulo,status_code,status_label,status_css,categoria,criado_em,atualizado_em`; `descricao` no detalhado | `app/templates/chamados/partials/tabela_chamados.html:19` | `id,assunto` com alternativa `titulo`; `colaborador_nome` com padrão `—`; `urgencia`; status/data | `urgencia` não é emitida; a apresentação termina no ramo “Normal”. Outras ausências têm alternativas explícitas | N* | ESTRUTURA / sim |
| D13 — followup GLPI | `glpi_client.py:209`; transporte em `views.py:246`, `:262` | `id,data,conteudo` | `app/templates/chamados/detalhe.html:80` | `autor_tipo,autor_user,criado_em,conteudo` | Grava `data`, lê `criado_em`; autor/tipo pedidos não são emitidos pelo formatador | N* | ESTRUTURA / sim |
| D14 — resposta desarquivar | `views_arquivar.py:62`, `:71`, `:89` | Recusa de área: `ok,erro`; outra recusa: `ok,erros`; sucesso: `ok` | `app/templates/chamados/partials/_lista_chamados.html:85`, `:134` | `ok`; `erros || ['Falha ao arquivar.']` | A recusa de área escreve singular `erro`; leitor procura plural `erros` | N*. `tests/test_f3_arquivar_porta.py:64` prende sucesso/trilha | ESTRUTURA / sim |
| D15 — contexto parcial do modal | `services/fio.py:461` | Núcleo do modal e `pode_conferir_data`; não fornece os contadores nem `cor_disputas_map` | `app/templates/chamados/partials/modal_fio.html:14`, `:76` | `contador_ativos,contador_msgs_mes,contador_disputas_abertas,cor_disputas_map` | O mesmo template pede campos fornecidos apenas pelo construtor completo, em `views.py:945` | N*. O GET é exercitado em `tests/test_paridade_fio_modal_f8.py:62` | ESTRUTURA / sim |
| D16 — contexto completo do modal | `views.py:927` | Núcleo, contadores e mapa de cores; omite `pode_conferir_data` | `app/templates/chamados/partials/modal_fio.html:172` | `pode_conferir_data` | O parcial fornece essa chave em `services/fio.py:474`; o completo a omite | N* | ESTRUTURA / sim |
| D17 — resultado do lote JSON | `views_cobrar.py:265` | `ok,validadas,recusados,nao_gravados` | `app/templates/core/_copiloto.html:252` | `validadas`; `recusados && recusados.length` | Esse consumidor omite `nao_gravados` e escolhe “Todos entraram” quando `recusados` está vazio. A chave tem outro leitor em `app/templates/chamados/partials/_lote_preview.html:9` | N* | ESTRUTURA / sim |
| D18 — diagnóstico de reuso | `services/fio.py:1114` | `fonte,n,esperado,motivo,pks,universo,universo_rotulo` | `management/commands/backfill_decisoes_pergunta.py:553` | Todas as listadas, exceto `fonte` | `fonte` sem leitor nominal de produção localizado nesse retorno; ausência global [nao verificado] | N*. Contador exercitado em `tests/test_e1_v15d_um_ponto_de_escrita.py:320` | ESTRUTURA / sim, literal |
| D19 — diagnóstico sem dia | `services/fio.py:1181` | Mesmas sete de D18 | `management/commands/backfill_decisoes_pergunta.py:559` | Todas, exceto `fonte` | Mesmo desencontro nominal | N*. Contador: `tests/test_e1_v15e_contrato_de_saida.py:293` | ESTRUTURA / sim, literal |
| D20 — diagnóstico de destino alheio | `services/fio.py:1206` | Mesmas sete de D18 | `management/commands/backfill_decisoes_pergunta.py:565` | Todas, exceto `fonte` | Mesmo desencontro nominal | N* | ESTRUTURA / sim, literal |
| D21 — diagnóstico de batida retratada | `services/fio.py:1902` | `fonte,n,esperado,motivo,pks` | `management/commands/backfill_decisoes_pergunta.py:545` | Todas, exceto `fonte` | Mesmo desencontro nominal | N*. Contador: `tests/test_e1_v15b.py:165` | ESTRUTURA / sim, literal |
| D22 — resultado da retratação | `services/fio.py:1342`, `:1343` | `retratadas,merito_recusado,episodios_unidos,sem_episodio_intocada,canonica_sem_episodio` | `management/commands/backfill_decisoes_pergunta.py:331` | `retratadas` | Quatro metadados sem leitura nominal localizada desse retorno; ausência global [nao verificado] | N* | ESTRUTURA / sim, literal |
| D23 — resultado do consumidor outbox | `reconciliador.py:1340` | `eventos,colabs,rejuntados,perguntas,retratados,premissa,orfas,lastro,disputas_barradas,adiados,falhas` | `management/commands/consumir_eventos_celula.py:31` | Todas, exceto `rejuntados,perguntas,lastro` | Três contadores emitidos sem leitura nominal localizada no consumidor de produção | N* | ESTRUTURA / sim, literal |

As ausências de D06–D08 distinguem dois formatos: a abertura humana acima e o emissor especializado, que fornece `ausencia_id,data,tipo,tem_documento` em `app/ponto/services/ausencia.py:672`. A equivalência de negócio entre essas formas é **[nao verificado]**.

O inventário dos escritores de `contexto_json` encontrado na cópia é o seguinte; as gravações genéricas efetivas estão em `services/contexto.py:76`, `:102`, `:125`, `:161`, além do nascimento pelas portas de `models.py:834` e `:1055`.

| Emissor | Chaves construídas/alteradas |
|---|---|
| `services/abertura.py:58` | `sistema,nome,empresa,posto,pendente_glpi=True` |
| `services/abertura.py:116` | `pendente_glpi=False` |
| `services/abertura.py:183` | `sistema,nome,empresa,posto,aberto_por_user_id,aberto_por,pendente_glpi`; chave catalogada de data em `:191` |
| `services/adesao.py:79`, `:113` | `adesao,data_inicio,periodo`; depois `agrupado_em` |
| `reconciliador.py:691`, `:708`, `:709`, `:921` | `data_turno,origem`; condicionalmente `marcos_faltantes,_marco_por` |
| `services/disputa_emissao.py:580`, `:586`, `:630`, `:666` | Contexto arbitrário do nascimento/reuso; variante de declaração fornece `origem` |
| `services/disputa_emissao.py:1116`, `:1122` | `chamados_orfaos_ids` |
| `services/disputa_emissao.py:1351`, `:1357` | `origem,chamado_limbo_id`; reuso acrescenta `chamados_limbo_ids` |
| `services/limbo_folgas.py:68` | `sugestao.{classe,dias,ndias,semanas}` |
| `services/limbo_folgas.py:276` | `agrupado_em,defeito_cadastro` |
| `services/declaracao_texto.py:208` | `escape_colab,escape_colab_em` |
| `management/commands/nomear_marco_por_lampada.py:109` | `marcos_faltantes,_marco_por` |
| `management/commands/carimbar_por_batida_coincidente.py:98` | `batida_id,data_turno,_dia_por` |
| `management/commands/smoke_chamados.py:97`, `:102` | `{motivo:{data}}` quando o template exige data; senão `{}` |
| `services/nota.py:110`, `:123` → `services/contexto.py:131` | `contexto_novo` arbitrário; listas unem, escalares existentes permanecem |
| `app/colaboradores/views.py:2376` | `agenda_dia_ids` |
| `app/colaboradores/services/vinculo.py:419` | `snapshot_ecs,ec_nova,template_id,template_nome,data_inicio` |
| `app/colaboradores/services/desligamento.py:182` | `data_demissao,motivo_informado,executado_por_username,executado_por_id,executado_em` |
| `app/ponto/utils.py:120` | `agendamento_pk,data_inicio,data_fim` |
| `app/ponto/views.py:280`, `:288`, `:301` | `pendentes,total` |
| `app/escala/regua_defesa.py:72` | `tipo_escala_id,tipo_escala_codigo,causa,data_exemplo` |
| `app/api/views.py:714`, `app/api/views_core.py:708` | Contexto do detector acrescido de `batida_id,data_turno` |
| `app/api/views.py:1978`, `:3120` | `data_turno`; atualização `tem_documento=True` |
| `app/ponto/services/justificativa.py:301` | `data_turno,tipo,motivo_rejeicao,rejeitado_por` |
| `app/ponto/services/cluster_espurio.py:211` | `data_turno,orfas,n_missing` |
| `app/ponto/services/geofence.py:375` | `ocorrencias,alerta_geofence_id,batida_id,tipo_alerta,distancia_metros,raio_metros,posto_nome,posto_endereco,posto_lat,posto_lon,batida_lat,batida_lon,tipo_batida` |
| `app/ponto/services/ausencia.py:672` | `ausencia_id,data,tipo,tem_documento` |
| `app/ponto/services/triagem_batida.py:117`, `:347`, `:353`, `:470` | Variantes de `motivo,situacao,posto,tipo_batida,hora_tentativa,motivo_anuencia,foto_b64,foto_nome,hora_prevista,diferenca_minutos,escala` |
| `app/ponto/management/commands/detectar_par_relampago.py:383` | `data_turno,par,duracao_min` |
| `app/ponto/management/commands/processar_alertas_avancados.py:126` | `data_turno,entrada_timestamp` |
| `app/ponto/management/commands/reconciliar_grade.py:473`, `:516` | `data_turno,origem,marcos_faltantes`; atualização `orfa_regeneracao_curada`; retirada de `orfa_regeneracao` em `:454`, `:474` |
| `app/ponto/management/commands/detectar_ausencias.py:224`, `:294`, `:532`, `:538`, `:559`, `:706` | Variantes de identificação de escala/ciclo, `data_inicio,causa,colabs_mesmo_template,dias_sem_grade,data_turno,horario_previsto,atraso_min,marcos_faltantes,_marco_por` |
| `app/ponto/management/commands/processar_alertas_turno.py:348` | `data_turno,entrada_timestamp,posto` |
| `app/ponto/management/commands/gerar_celulas.py:264` | `orfa_regeneracao.{em,marcos_na_emissao}` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:64`, `:138`, `:163`, `:179`, `:195` | `orfao_detectado,escalado_n2_1h,escalado_n3_4h,escalado_n4_comercial,escalado_n5_24h` |
| `app/ponto/management/commands/curar_residuo_bug115.py:124` | `marcos_candidatos,_marco_por`; demais companheiras [nao verificado] |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:156` | `_ctx`; formato completo [nao verificado] |

Os leitores nominais desse JSON encontrados estão abaixo. Este é um inventário de leitores, **não a expansão completa de cada escritor × leitor em costuras**; essa expansão permanece **[nao verificado]**.

| Consumidor | Chaves lidas/defaults |
|---|---|
| `catalogo/modulos.py:225` | Chave dinâmica `m['chave_data']`, `.get(...)=None` |
| `juizes.py:114`, `:118`, `:129`, `:159`, `:187`, `:197`, `:226` | Chave catalogada no singular; `data_turno=None,batida_id=None,ultima_batida.timestamp=None,entrada_timestamp=None`; lote conforme D04 |
| `juizes.py:549`, `:551`, `:576`, `:579` | `marcos_faltantes or []`; itens `motivo=None,hora=None,tipo=None` |
| `juizes.py:1723`, `:1747`, `:2163`, `:2331` | `agrupado_em=None,entrada_timestamp=None,ausencia_id=None` |
| `models.py:816`, `:924`, `:931`, `:955`, `:1068`, `:1235`, `:1435` | `agrupado_em=None`; contexto de `defaults or {}`; `marcos_faltantes=None`; filtros `contexto_json__*`; contexto integral para dia/célula; `entrada_timestamp=None` |
| `reconciliador.py:146`, `:198`, `:243`, `:836`, `:1589` | `entrada_timestamp=None,par=None,chamados_orfaos_ids=None`; filtros `data_turno` |
| `data_evento.py:27` | `data_turno=None,data_batida=None,entrada_timestamp=None,pendentes=None,horario_previsto=None` |
| `services/disputa_emissao.py:57`, `:68`, `:145`, `:733`, `:930`, `:1074`, `:1143`, `:1170`, `:1232` | Cópia integral por `items`; `entrada_sem_saida.{entrada_id,entrada_aberta_em}`; `ultima_batida.id`; `entrada_timestamp`; marcos e data catalogada |
| `services/acoes_chamado.py:30`, `:57`, `:96`, `:144`, `:185`, `:247`, `:267`, `:371`, `:403` | `tipo_batida='E',hora_tentativa='',motivo_anuencia='',foto_b64=None,foto_nome='selfie.jpg',agenda_dia_ids=[]`; `snapshot_ecs or []`; campos geo; `batida_id=None,ocorrencias or []` |
| `services/limbo_folgas.py:64`, `:82`, `:193`, `:253` | `tipo_escala_id=None,sugestao or {},sugestao.dias or [],sugestao.classe=None,agrupado_em=None` |
| `services/adesao.py:111` | `agrupado_em=None` |
| `services/declaracao_texto.py:205` | `escape_colab=None` |
| `services/abertura.py:168` | Filtro `aberto_por_user_id` |
| `services/contexto.py:70`, `:97`, `:118`, `:144` | Chaves dinâmicas: `.get(k)=None`, pertencimento e listas |
| `services/feriado_reavaliar.py:78` | Filtro `data_turno__in` |
| `services/pergunta_no_app.py:114`, `services/cobranca_folha.py:32`, `rejuncao.py:83` | Data catalogada |
| `services/materializacao.py:135` | Estrutura entregue a `data_evento_do_contexto` |
| `services/celula_carimbada.py:45`, `:149` | Campo completo entregue ao juiz do dia |
| `views.py:441`, `:1512` | `tipo_escala_id=None,ausencia_id=None` |
| `signals.py:361` | Filtro `chamados_orfaos_ids__contains` |
| `management/commands/nomear_marco_por_lampada.py:50`, `:65`, `:70` | `marcos_faltantes or [],horario_previsto=None,origem=None` |
| `management/commands/auditar_invariantes_chamados.py:73`, `:93`, `:104`, `:617`, `:686`, `:1016` | Filtros de `data_turno`; `entrada_timestamp=None`; chave catalogada |
| `app/templates/chamados/partials/painel_chamado_turno.html:35` | **Todas as chaves/valores**, por `items` |
| `app/templates/chamados/partials/modal_fio.html:481`, `:529`, `:544`, `:561`, `:588` | Coordenadas, distância, raio padrão `200`, distância padrão `0`, textos padrão `''`; `batida_id=''`; `sugestao.classe`; `classe,evidencia,destino.{codigo,n_colabs}`; anuência/tipo/hora |
| `app/templates/chamados/partials/_relatorio_geo.html:18` | Tipo padrão `'-'`, alerta, distância, raio/posto padrão `'-'`, endereço e coordenadas |
| `app/templates/chamados/detalhe_local.html:227`, `:245` | `tem_documento,motivo_anuencia,tipo_batida,hora_tentativa`; últimos dois padrão `'—'` |
| `app/templates/chamados/partials/_card_atendimento.html:65`, `:232` | `tem_documento,ausencia_id,data_turno` |
| `app/escala/utils.py:1062`, `:1066` | `agrupado_em=None,marcos_faltantes or []`; itens `tipo=None,hora=None` |
| `app/colaboradores/services/calendario.py:438`, `:448` | Filtros `data_turno`, presença de `agrupado_em`; `.get('agrupado_em')=None` |
| `app/colaboradores/services/pos_vinculo.py:38` | `periodo or []`, usado como intervalo quando tem dois itens |
| `app/escala/services/regeneracao.py:149`, `:196` | `marcos_faltantes=None`; item `hora=None` |
| `app/ponto/services/lastro.py:75` | `marcos_faltantes or []`; itens `hora=None,tipo=None` |
| `app/ponto/services/entrada_sincronizada.py:51`, `:55` | Filtro e leitura `chamados_orfaos_ids=None` |
| `app/ponto/services/cartorio.py:136`, `app/ponto/services/cobertura.py:230`, `app/ponto/management/commands/supra_juiz.py:200`, `app/ponto/management/commands/mapa_divergencia.py:293` | Data catalogada |
| `app/ponto/worklist.py:54`, `app/ponto/management/commands/reconciliar_fantasmas.py:183` | `pendentes=None` |
| `app/ponto/management/commands/reconciliar_grade.py:376`, `:445`, `:465` | `orfao_14h.{data,entradas_pendentes[].data}`; `pendentes`; `orfa_regeneracao`; `marcos_faltantes[].hora`, todos opcionais |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:63`, `:112`, `:147`, `:172`, `:188` | Os cinco carimbos de escalada, `.get(...)=None` |
| `app/ponto/management/commands/curar_residuo_bug115.py:76`, `:83`, `:86`, `:121` | `marcos_faltantes or [],horario_previsto=None,origem=None,marcos_candidatos=None` |
| `app/ponto/management/commands/

[truncado em 20000 de 41581 caracteres]

## 13/11 15:51 · sessao 331cef45869d · CONTRATOS-ENTRADA · censo · origem=Code

O desencontro mais direto confirmado em escala é este: `aplicar()` devolve `{decisao_id, saida}`, mas o fio lê `ok` com default `True` e `msg`; o roteamento resolve o escritor diretamente, sem adaptar o retorno (`app/escala/services/propositor.py:788`; `app/chamados/services/proposta_no_fio.py:151-153`; `app/core/intake.py:49-52,481-487`).

**Não fecho este levantamento como o censo exaustivo pedido.** A busca encontrou outras estruturas e consumidores, mas não demonstrou **todos os escritores, todos os leitores e todos os selos de cada estrutura**; publicar um total global como fechado seria `[nao verificado]`.

Método utilizado: busca dos `JSONField`, atribuições e construtores de dict; rastreamento de chamadas, imports, subscritos, `.get`, serializações, templates e testes, com AST analisada em memória. Não executei testes, banco ou logs. A resolução geral de aliases, chamadas dinâmicas, repasses transitivos e consumidores de APIs não ficou fechada `[nao verificado]`. [nao verificado]

Nos quadros, **“não localizado” não significa inexistente** `[nao verificado]`. Dono **ESTRUTURA provável** é uma inferência sobre o protocolo; a classificação de qualquer ocorrência de produção pelo juiz L-099 permanece `[nao verificado]` (`app/ponto/management/commands/e6_oraculo.py:43`; `CLAUDE.md:179`).

| Estrutura | Emissor arquivo:linha | Chaves gravadas | Consumidor arquivo:linha | Chaves lidas | DESENCONTRO | Selo que prende | Dono L-099 provável | É classe |
|---|---|---|---|---|---|---|---|---|
| Resultado de aplicar proposta | `app/escala/services/propositor.py:788` | `decisao_id, saida` | `app/chamados/services/proposta_no_fio.py:151-153` | `ok→True`, `msg→None` | `ok/msg` não são emitidos por esse escritor; ausência de `ok` é aceita como sucesso | Não localizado `[nao verificado]`; os testes de fio usam proposta substituída em `app/chamados/tests/test_proposta_no_fio.py:131-146` | ESTRUTURA provável `[nao verificado]` | **Sim** |
| Mesmo resultado | `app/escala/services/propositor.py:788` | `decisao_id, saida` | `app/escala/management/commands/propostas_de_escala.py:68-72` | `saida, decisao_id` | Nenhum demonstrado nesse par | Não localizado `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |
| Opções da correção retroativa | `app/escala/services/propositor.py:765-771` | `colab,vinculo,template,desde,ate,ancora,novo_trecho,regenerar_desde,apesar_da_lavra,apply,schema` | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:63-66,81-86,109-110,158,172,192-194,225,233` | As mesmas, exceto `schema` | `_rodar` não lê `schema`; quem o lê é `handle`, entrada evitada pelo emissor (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:48-51`; `app/escala/services/propositor.py:760-764`) | Parcial: aplicação real e decisão em `app/escala/tests/test_porta_da_proposta.py:148-166`; descarte de `schema` não preso `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, chave sem leitura nessa entrada |
| Payload do wizard | `app/escala/views_wizard.py:20-59` | Nome, apelido, horários, duas pausas, duração, flags, extras HE, calendário, folgas, dias da semana; criação acrescenta ciclo/base/referência; HPD condicional | `app/escala/services/cadastro_tipo.py:148-267` | Campos emitidos; presença de `descricao` em `:185`; HPD por `.get` em `:217` | Wizard não emite `descricao`; HPD pode faltar. São omissões opcionais, sem dano demonstrado | **Sim, parcial**: POST→service→modelo em `app/escala/tests/test_wizard_views.py:28-67`; duração/HPD em `app/escala/tests/test_wizard_intervalo.py:76-89` | ESTRUTURA provável `[nao verificado]` | **Sim**, pelo critério literal de chave opcional |
| Ajustes de template | `app/escala/services/escala_auto_executor.py:65-84` | A7: `_delta_blocos` e horários; A4: pausa/duração; FOLGA: `folga_dia_semana` | `app/escala/services/cadastro_tipo.py:443-445` | `_delta_blocos→0`; demais itens integralmente | A4/FOLGA omitem `_delta_blocos`; default explícito | **Sim**: executor→derivação em `app/escala/tests/test_escala_auto_executor.py:66-84` | ESTRUTURA provável `[nao verificado]` | **Sim**, opcional |
| Proposta automática A4 | `app/escala/services/escala_auto.py:92-93,115,183-192` | Envelope da proposta; `valores.{hii,hfi}` | `app/escala/services/escala_auto_executor.py:67-84,97,109` | Envelope; A4 lê `hii` e calcula o fim com duração | `valores.hfi` emitido não é consumido pelo executor | Caminho executor em `app/escala/tests/test_escala_auto_executor.py:66-84`; descarte específico de `hfi` não preso `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, no par |
| Resultado do executor | `app/escala/services/escala_auto_executor.py:114` | `ec_antigo,ec_novo,template_derivado,inicio,celulas,historico` | `app/escala/management/commands/ajustar_escalas_auto.py:97-99` | Todas, exceto `ec_antigo` | `ec_antigo` sem leitura nesse consumidor; outro leitor não localizado `[nao verificado]` | Não localizado `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, nominal |
| Resultado de desfazer | `app/escala/services/escala_auto_executor.py:152` | `volta,celulas,historico` | `app/escala/management/commands/ajustar_escalas_auto.py:42` | `volta,celulas` | `historico` sem leitura nesse consumidor; outro leitor não localizado `[nao verificado]` | Não localizado `[nao verificado]`; teste isolado em `app/escala/tests/test_escala_auto_executor.py:86` | ESTRUTURA provável `[nao verificado]` | **Sim**, nominal |
| Linha de pauta da proposta | `app/escala/services/propositor.py:644-650` | `chave,n,frase,url` | `app/pautas/semanal.py:146,163-168` | `alavanca→n→0`, `frase→''`, `chave,n,url` | Não emite `alavanca`; consumidor usa `n` | Não localizado `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, opcional |
| Linha de pauta da fase | `app/escala/services/fase_12x36.py:229-234,267-280` | `chave,n,frase,url` | `app/pautas/semanal.py:146,163-168` | `alavanca→n→0`, `frase→''`, `chave,n,url` | Mesma omissão opcional de `alavanca` | Pauta pela lavra em `app/escala/tests/test_o261_v_fase_na_lista.py:254`; chave exata não presa `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, opcional |
| Partes do nome canônico | `app/escala/models.py:486-522` | `tipo,apelido,dias,horario,ciclico` | `app/core/templatetags/hasner_filters.py:72-123,207-217` | `tipo,apelido,dias,horario`; segundo leitor usa defaults `''` | `ciclico` sem leitor de produção localizado `[nao verificado]` | Produtor em `app/escala/tests/test_nome_canonico.py:77-95`; uso de `ciclico` no consumidor não preso `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, nominal |
| Mesmas partes | `app/escala/models.py:486-522` | As mesmas cinco chaves | `app/colaboradores/services/ficha.py:70-71,82-90` | `tipo,apelido,dias,horario`, defaults `''` | Mesmo campo sem leitor nominal localizado `[nao verificado]` | Cabeçalho em `app/colaboradores/tests/test_ficha_cabecalho_compacto.py:28-40`; não prende `ciclico` | ESTRUTURA provável `[nao verificado]` | **Sim**, nominal |
| Rótulo da régua | `app/escala/regua_defesa.py:632` | `classe,faixa,escore,texto` | `app/escala/services/furos_diarios.py:36,305-309` | Existência e `classe`, inclusive default `''` | `faixa/escore/texto` sem consumo de produção localizado `[nao verificado]` | Não localizado `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, nominal |
| Mesmo rótulo | `app/escala/regua_defesa.py:632` | As mesmas quatro chaves | `app/chamados/juizes.py:514-516` | Apenas existência/`None` | Mesmo restante sem leitura nominal localizada `[nao verificado]` | Não localizado `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, nominal |
| POST de realocação | `app/escala/regua_defesa.py:313` | `template_id,data_inicio,modo` | `app/colaboradores/services/vinculo.py:52-59,146` | As três; `data_ancora→None`, `confirm_txt→''` | Emissor omite os dois campos opcionais | Não localizado para esse emissor `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, opcional |
| Contexto de calendário — lista | `app/escala/views.py:294-310` | Itens `cal,tamanhos,semana_dia_rotulo,colabs,n_colabs`; tamanho `tamanho,dias,canonico` | `app/templates/escala/folga_calendarios_lista.html:12-38` | Campos do item; dos tamanhos, somente `canonico` | `tamanhos.tamanho/dias` sem leitor desse contexto localizado `[nao verificado]` | Contexto em `app/escala/tests/test_editor_folga_modo.py:66-71`; uso HTML dessas chaves não preso `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, nominal |
| Contexto do editor de calendário | `app/escala/views.py:363-371` | `cal,tamanhos,tipos,modo,celulas`; tamanho `tamanho,rotulo,dias_range,marcados` | `app/templates/escala/folga_calendario_editar.html:6-21,67-68` | Todas, exceto `tipos` e `tamanhos.dias_range` | Duas chaves sem leitura nesse template; outro leitor não localizado `[nao verificado]` | Contexto em `app/escala/tests/test_editor_folga_modo.py:58-64`; órfãs não presas `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, nominal |
| JSON de aplicação do plano | `app/escala/views_plano_folgas.py:158,171-190` | Erro inicial `{erro}`; sucesso `ok,gravadas,erros,celulas_relavradas,barrados`; erro de item `ec,dia,erro` | `app/templates/escala/plano_folgas.html:174-185` | `ok`; `erro||'erro'`; `erros||[]`; itens `ec,dia,erro` | Erro inicial omite `ok`; ausência significa falso. JS não lê `gravadas/celulas_relavradas/barrados` | API em `app/escala/tests/test_bug136_plano_em_lote_relavra.py:75-81`; JS não executado pelo selo `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | **Sim**, opcional e nominal |

Os JSONs persistidos abaixo tiveram escritores e leitores localizados, mas esse quadro **também não certifica a relação completa de consumidores indiretos** `[nao verificado]`.

| Estrutura | Emissor arquivo:linha | Chaves gravadas | Consumidor arquivo:linha | Chaves lidas | DESENCONTRO | Selo que prende | Dono L-099 provável | É classe |
|---|---|---|---|---|---|---|---|---|
| `horarios_por_dia` | `app/escala/services/cadastro_tipo.py:265-267` | Dict recebido integralmente; inválido vira `{}` | `app/escala/models.py:224-239,326-342,462-471,667-695,862-866` | Weekday; `hi/hf/hii/hfi`; defaults `None`, `''` ou escalares conforme função | Nenhum demonstrado para as chaves canônicas; extras arbitrários `[nao verificado]` | Persistência em `app/escala/tests/test_cadastro_tipo_service.py:47-56`; leitores em `app/escala/tests/test_horarios_por_dia.py:37-93` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |
| Mesmo HPD | Mesmo escritor `app/escala/services/cadastro_tipo.py:265-267` | Dict integral | `app/escala/utils.py:274-283,735-741` | `hi/hf/hii/hfi→None`; presença do mapa | Nenhum demonstrado | Leitor em `app/escala/tests/test_marcos_def_hpd.py:27-45`; travessia específica do escritor não fechada `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |
| Mesmo HPD | Mesmo escritor `app/escala/services/cadastro_tipo.py:265-267` | Dict integral | `app/escala/views_wizard.py:80` | Serialização integral, default `{}` | Nenhum demonstrado | Não localizado `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |
| Mesmo HPD | Mesmo escritor `app/escala/services/cadastro_tipo.py:265-267` | Dict integral | `app/colaboradores/services/drawer.py:99-100` | Verdade/falsidade do JSON | Nenhum demonstrado | Não localizado `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |
| Mesmo HPD | Mesmo escritor `app/escala/services/cadastro_tipo.py:265-267` | Dict integral | `app/ponto/selecao_periodo.py:223-229` | Weekday string; `hi/hf`; erro vira `False` | Leitor aceita chave string; `_override_do_dia` também admite inteiro. Efeito após persistência JSON `[nao verificado]` (`app/escala/models.py:231`) | Não localizado `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | `[nao verificado]` |
| `FolgaCalendario.celulas` | **Dois escritores:** `app/escala/folgas.py:584-591,654-656` | Lista de pares `[semana,weekday]`, ou `[]` | `app/escala/views.py:371` | Lista integral, default `[]` | Nenhum demonstrado | Escrita e GET em `app/escala/tests/test_editor_folga_modo.py:27-64`; ida e volta única não demonstrada `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |
| `FolgaCalendarioTamanho.dias` | **Dois escritores:** `app/escala/folgas.py:593-595,658-661` | Lista de dias por tamanho | `app/escala/models.py:1560,1579` | Lista integral | Nenhum demonstrado | Getter em `app/escala/tests/test_folga_models.py:13-23`; ponte específica dos escritores `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |
| Mesmo `dias` | Mesmos escritores `app/escala/folgas.py:593-595,658-661` | Lista de dias | `app/escala/views.py:362` | Lista integral convertida em set | Nenhum demonstrado | Não localizado `[nao verificado]` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |
| `dias_por_tamanho()` | `app/escala/models.py:1558-1560` | `{tamanho_mes:list(dias)}` | `app/escala/folgas.py:12-15,56` | Chaves `28/30/31`; fevereiro de 29 usa 28; default `[]` | Nenhum demonstrado | **Sim:** produtor→resolvedor em `app/escala/tests/test_folga_models.py:19-23` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |
| Ata efêmera | `app/escala/utils.py:1886-1898` | Somente `lampadas,orfas`; omissão de minutos explícita | `app/ponto/juiz_batida.py:157-161,190-194,215-217,230-246`, por `app/ponto/turnos.py:1631-1633` | `lampadas/orfas→[]`; lâmpada `hora,tipo,acesa,luz,tipo_real` | Nenhum demonstrado; consumidor indicado não exige minutos | **Sim:** convergência grade→ata→turno em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:193-205` | ESTRUTURA provável `[nao verificado]` | Não demonstrada |

Há estruturas gravadas sem par de leitura demonstrado nesta busca; elas **não podem entrar numa contagem de costuras comprovadas**:

| Estrutura gravada | Escritores localizados | Chaves | Leitor / selo |
|---|---|---|---|
| `DecisaoProposta.resumo` | `app/escala/services/propositor.py:717-727,773-776` | `tipo,template,template_id,ancora,ancora_atual,template_atual,casa_hoje,casaria,sobra,aplicar_desde,dias_batidos` | Leitor não localizado `[nao verificado]`; finalidade forense declarada em `app/escala/models.py:1927-1930` |
| Snapshot de calendário vazio | `app/escala/management/commands/lavrar_foto_folgas.py:173-183` | Mês→`vinculos,calendarios,vinculos_ids` | Leitor específico e selo não localizados `[nao verificado]` |
| Snapshot de sobra da foto | `app/escala/management/commands/lavrar_foto_folgas.py:175-183` | Mês→`vinculos,datas,lista` | Leitor específico e selo não localizados `[nao verificado]` |
| Fila de cobranças órfãs | `app/escala/services/regeneracao.py:189-196` | `chamado_id,colaborador_id,colaborador,data_turno,emitido_em,dna_reescrito_em,marcos_cobrados` | Consumidor de produção não localizado `[nao verificado]` |

A localização do model não basta para atribuir o emissor a escala: DNA e DNA anterior são atribuídos pela porta em ponto; ata persistida é atribuída nessa porta; o model de escala declara o escritor externo do outbox (`app/ponto/portas/celula.py:261-307,629-642,827-828,917-926,1003,1064,1091`; `app/escala/models.py:1819-1828,1851`). Isso não exclui a ata **efêmera** montada em escala, registrada no quadro anterior (`app/escala/utils.py:1886-1898`).

O exemplo fornecido foi conferido: o retroativo grava `foto_ausente.batida_id`, enquanto os dois leitores indicados procuram `ultima_batida.id`; não o contei como emissor de escala (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98-103`; `app/chamados/services/disputa_emissao.py:372-392`; `app/chamados/models.py:3464`).

**Contagem do módulo escala:** costuras totais **`[nao verificado]`**; com desencontro **`[nao verificado]`**; sem selo **`[nao verificado]`**. Os quadros acima contêm **29 pares explicitados**, mas não cobrem o universo solicitado: permanecem sem consolidação integral as grades produzidas por `leitor_celula`, os mapas de alimentação, os snapshots, os contextos de cadastro e os demais retornos de serviços (`app/escala/services/leitor_celula.py:390-445`; `app/escala/alimentacao.py:73-78,97-102,145-160,329-358`; `app/escala/services/cadastro_realidade.py:970-976`).

## 13/11 15:47 · sessao f90fd80293fd · - · - · origem=Ronald (tmux)

Nova partição: **31 costuras nominais**, normalizadas por `(estrutura retornada, módulo emissor, módulo consumidor)`, com **6 desencontros literais de chaves emitidas sem leitura nominal de produção encontrada**. Não executei código da aplicação nem testes; existência do selo não significa resultado verde [nao verificado]. [nao verificado]

Prefixos: `S=app/chamados/services/`, `C=app/chamados/management/commands/`, `T=app/chamados/tests/`.

| # | Estrutura / emissor | Chaves emitidas | Consumidor / leitura | Desencontro literal | Selo localizado |
|---|---|---|---|---|---|
| 1 | Foto, `S/foto_e4b.py:252`, `:329` | índice `tabela:pk → {campos,alcance[{dia,comp,entregue}]}` | `C/backfill_fio_morto_e4b.py:283`, `:289`, `:302`, `:318-320`, `:346-348`: foto inteira para reversão/diff; itera nomes `dinheiro_online:*` | nenhum identificado | `T/test_e4b_c4_orm.py:509` verifica fotografia sob trava e reversão antes do ato; `T/test_e4b_foto_memoria.py:18-48` sela shape da cerca/relato |
| 2 | Delta, `S/foto_e4b.py:118`, `:141` | índice dinâmico `→ {antes,depois}`, cada lado foto anterior | `C/backfill_fio_morto_e4b.py:320-323`, `:387-393`: índices dinâmicos, lados diretos, `alcance`, `comp`, `.get('dia')=None` | nenhum identificado | `T/test_e4b_c4_orm.py:189`; cobertura precisa de toda chave do par [nao verificado] |
| 3 | Relato, `S/foto_e4b.py:199-206` | `nascimentos,push,dinheiro,linhas`; linhas têm `campos_alterados,alcance` | `C/backfill_fio_morto_e4b.py:322`, `:348`, `:397-401`: guarda tentativa e serializa **todo** relato com `json.dumps` | nenhum identificado | `T/test_e4b_foto_memoria.py:48-71` sela conteúdo do retorno; selo do par completo com comando [nao verificado] |
| 4 | Reparo passivo, `S/passivo_validado.py:319-322`, `:359-361`, `:411` | `universo,com_registro,alvo,plantada,recusada,fora_do_escopo,competencias,reversao,pauta,pauta_sem_movimento,apply,comp_pelo_instante_diverge,erros`; `disputas_fechadas` só quando há fechamento. Competência: `empresa_id,colab_id,mes,ano,exportada,alvo,plantada,regen,pauta` | `C/materializar_passivo_validado.py:104-115`, `:129`, `:244-272`, `:286-288`, `:305-308`: serializa todos recursivamente; lê contadores diretos, `pauta_sem_movimento` default `0`; nested regen `hash_depois` default `'-'`, `mudou` default `None` | nenhum identificado | `T/test_o317_passivo_pela_porta.py:288`, `:526` selam partição/reversão do retorno; par com comando [nao verificado] |
| 5 | Censo fio, `S/vigia_fio.py:356-372`, `:374`, `:488-490` | todos contadores/listas explicitados no bloco; `porta_` + classes dinâmicas; `cego` em ambos retornos | `C/vigiar_fio_morto.py:175`, `:185`, `:214-215`, `:239`: alarme por `d[k]`, `celulas_do_fio_morto`, **todo** dict preservado em carimbo | nenhum identificado | **Sim**, `T/test_o261_vigia_fio.py:787`, compara contadores reais com carimbo |
| 6 | Mesmo censo fio | idem #5 | `C/materializar_passivo_validado.py:205-211`: itera todas chaves, retém valores `int/bool` via `f.get(k)` | nenhum global: outras chaves são lidas por #5 | selo do par [nao verificado] |
| 7 | Censo alcance, `S/vigia_fio.py:571-575`, `:589`, `:629` | `alcance_universo,alcance_competencias,alcance_porta_fila,alcance_porta_pergunta,alcance_de_hoje_pra_frente,fio_morto_alcance,alcance_porta_exige_o_admin,alcance_e_detalhe,alcance_por_rota_papel,alcance_dias,alcance_dias_omitidos,alcance_cego`, e `alcance_` + causas/baldes | `C/vigiar_fio_morto.py:176`, `:189`, `:240`, `:243`: encaminha para linhas, lê `fio_morto_alcance`, persiste **todo** alcance | nenhum identificado | `T/test_o261_vigia_fio.py:787` também confere `fio_morto_alcance` em `:817`; demais shape [nao verificado] |
| 8 | Mesmo alcance | idem #7; linhas `alcance_dias=[celula_id,colab_id,data,competencia,rota,causa,detalhe,papel]`, `S/vigia_fio.py:629` | `C/backfill_fio_morto_e4b.py:78-79`: `alcance_dias`, índice `4` para `sem_porta`; `:412`: `alcance_universo,fio_morto_alcance` diretos | nenhum identificado | selo do par [nao verificado] |
| 9 | Vivas em disputa fechada, `S/vigia_fio.py:759` | `perguntas,disputas` | `C/vigiar_fio_morto.py:177`, `:194`, `:245`: ambas diretas e dict inteiro em carimbo | nenhum identificado | selo específico do par [nao verificado] |
| 10 | Materializada viva, `S/vigia_fio.py:851`; chaves da origem `:810` | `aberta,exportada`, valores contagens | `C/vigiar_fio_morto.py:179`, `:202`, `:239`, `:244`: ambas diretas | nenhum identificado | selo específico do par [nao verificado] |
| 11 | Censo carimbo, `S/vigia_carimbo.py:121-123`, `:132` | `universo,com_registro,resposta_enviada_sem_gravacao,sem_plantio_nenhum,plantio_fora_do_slot,slot_nao_derivavel,perguntas_sem_gravacao,cego` | `C/vigiar_carimbo_sem_registro.py:63-74`: lista `perguntas_sem_gravacao`, alarme por `d[k]`, encaminha dict inteiro a `linhas` | nenhum identificado | `T/test_o306_vigia_carimbo.py:148`, `:196`, `:225` selam partições, linhas, alarmes; contrato completo do comando [nao verificado] |
| 12 | Mesmo censo carimbo | idem #11 | `C/materializar_passivo_validado.py:205-210`: lê por subscript todas chaves **menos** lista de perguntas; lista consumida por #11 | nenhum identificado | selo do par [nao verificado] |
| 13 | Diagnóstico reuso, `S/fio.py:1114-1117` | `fonte,n,esperado,motivo,pks,universo,universo_rotulo` | `C/backfill_decisoes_pergunta.py:553-556`: lê diretamente todas **menos `fonte`** | **`fonte` sem leitura nominal de produção encontrada** | contador selado em `T/test_e1_v15d_um_ponto_de_escrita.py:320`; selo da leitura/omissão de `fonte` [nao verificado] |
| 14 | Diagnóstico sem dia, `S/fio.py:1181-1184` | mesmas 7 chaves #13 | `C/backfill_decisoes_pergunta.py:559-562`: todas menos `fonte` | **`fonte` sem leitura nominal de produção encontrada** | `T/test_e1_v15e_contrato_de_saida.py:293` sela contador; par [nao verificado] |
| 15 | Diagnóstico destino alheio, `S/fio.py:1206-1208` | mesmas 7 chaves #13 | `C/backfill_decisoes_pergunta.py:565-568`: todas menos `fonte` | **`fonte` sem leitura nominal de produção encontrada** | selo específico do par [nao verificado] |
| 16 | Diagnóstico batida retratada, `S/fio.py:1902-1903` | `fonte,n,esperado,motivo,pks` | `C/backfill_decisoes_pergunta.py:545-547`: todas menos `fonte` | **`fonte` sem leitura nominal de produção encontrada** | `T/test_e1_v15b.py:165` sela contador; par [nao verificado] |
| 17 | Fora do índice, `S/fio.py:2001-2007` | `marco_sem_dia,batida_sem_origem,batida_sem_dia` | `C/backfill_decisoes_pergunta.py:486`, `:489`, `:574`: três chaves diretas | nenhum identificado | selo do par [nao verificado] |
| 18 | Plano retratação, `S/fio.py:1281-1282`, `:1287` | `retrata,merito_recusado,episodios_unidos,sem_episodio_intocada,canonica_sem_episodio` | `C/backfill_decisoes_pergunta.py:290`, `:396`, `:510-514`: `retrata,merito_recusado` e categorias de `NAO_RETRATA` por chaves dinâmicas; leitura de `canonica_sem_episodio` neste módulo não localizada [nao verificado] | nenhum global: `canonica_sem_episodio` lida por #19 | `T/test_e1_v10.py:142-167` sela plano; par com backfill [nao verificado] |
| 19 | Mesmo plano retratação | idem #18 | `C/dedup_perguntas_fato.py:81-88`: categorias `NAO_RETRATA`, `canonica_sem_episodio,retrata`, todos subscripts | nenhum identificado | **Sim**, `T/test_e1_v10.py:183-201`, executa comando e confere contagem do plano |
| 20 | Resultado retratação, `S/fio.py:1343` | `retratadas`, mais `merito_recusado,episodios_unidos,sem_episodio_intocada,canonica_sem_episodio` do plano após `pop('retrata')` em `:1342` | `C/backfill_decisoes_pergunta.py:331`: só `['retratadas']`; chamadas internas em `S/fio.py:2181` também só leem essa chave, `:922` ignora retorno | **quatro metadados restantes sem leitura nominal de produção encontrada desse retorno** | testes do ato existem `T/test_e1_v15g.py`; selo dessas chaves do retorno [nao verificado] |
| 21 | Resultado migração, `S/fio.py:2080`; enriquecido por `_migrar_sob_trava` em `:2121` | `migradas,duplicatas,recusadas`; listas | `app/chamados/models.py:1416`, `:1430`, `:1436`: lê `migradas` para mensagem; espalha **todo** resultado em payload de auditoria | nenhum identificado | `T/test_e1_v5_chave_familia.py:114` sela resumo/trilha; verificar alcance exato do reabrir [nao verificado] |
| 22 | Resultado consumidor outbox, `app/chamados/reconciliador.py:1340-1341` | `eventos,colabs,rejuntados,perguntas,retratados,premissa,orfas,lastro,disputas_barradas,adiados,falhas` | `C/consumir_eventos_celula.py:31-35`: interpolação lê `eventos,colabs,retratados,premissa,orfas,disputas_barradas,adiados,falhas` | **`rejuntados,perguntas,lastro` sem leitura nominal de produção encontrada no único caller** | `T/test_c20_juiz3_outbox.py` e `T/test_orfas_nao_morre_na_guarda_da_porta.py:146-152` leem resultado em testes; selo desse par de saída [nao verificado] |
| 23 | Alimentação fila, `app/chamados/juizes.py:1549` | `verbo,fila_ids,fila_cel` | `S/tarefa_do_dia.py:529-532`, `:546-549`: três diretas; `fila_cel.get(celula_id,())` | nenhum identificado | **Sim**, `T/test_e3_portas_tarefa.py:113-127` exige uma alimentação e confere verbos; outros cenários em `:32-100` |
| 24 | Alimentação portas, `app/chamados/juizes.py:1585-1587` | anteriores + `vivas,colab,no_app,instancias` | `S/vigia_fio.py:604-621`: `fila_ids,vivas,instancias`, e `_no_app` recebe todo mapa; porta também recebe mapa em `:635` | nenhum identificado | `T/test_n1_alimentacao_unica.py:74-86`; `T/test_n1_perguntas_do_app.py:395-418` |
| 25 | Mesma alimentação portas | idem #24 | `app/folha/fora_da_folha.py:62`, `:67`: repassa estrutura inteira ao juiz da porta, sem acesso direto às chaves | nenhum identificado | `T/test_o261e_f5b_juiz_porta.py:180-189` sela paridade lote/avulso; par folha em `:114-119` |
| 26 | Mesma alimentação portas | idem #24 | `app/escala/services/cadastro_realidade.py:484-488`: `vivas` direto, `.get(pk) → ()`; repassa mapa inteiro à porta | nenhum identificado | selo específico desse par [nao verificado] |
| 27 | Mesma alimentação portas | idem #24 | `C/diff_e3_sombra.py:114`, `:124`, `:128`, `:62-64`: `fila_cel.get(cel_id,())`, `vivas.get(cel_id,())`, `fila_ids`; repassa à porta | nenhum identificado | `T/test_e3_diff_sombra.py`; cobertura específica de chaves [nao verificado] |
| 28 | Porta alcançável, `app/chamados/juizes.py:1667-1668` | `rota,fila,verbos,fio`, ou `None` em `:1655/:1664` | `app/folha/fora_da_folha.py:78`, `:225`: `fio` direto; `(porta or {}).get('fila') or ()` | nenhum identificado | **Sim**, `T/test_o261e_f5b_juiz_porta.py:114-119`, `:167` |
| 29 | Mesma porta | idem #28 | `S/vigia_fio.py:641-646`: `verbos,rota` diretos; guarda `is not None` | nenhum identificado | **Sim**, `T/test_o261e_f5b_juiz_porta.py:119`, `:125` |
| 30 | Mesma porta | idem #28 | `app/escala/services/cadastro_realidade.py:488`: só compara estrutura com `None`; não lê chave | nenhum identificado | selo específico desse par [nao verificado] |
| 31 | Mesma porta | idem #28 | `C/diff_e3_sombra.py:47-57`, `:124`, `:139-140`: `fio,fila,rota,verbos`; se não há porta usa `'nada',(),None,[]/False`; compara estrutura com `None` | nenhum identificado | `T/test_e3_diff_sombra.py`; cobertura precisa das quatro chaves [nao verificado] |

Nos **seis desencontros** (#13–16, #20, #22), o dono provável é **ESTRUTURA** pela natureza do contrato no código, sem conclusão de impacto de produção [nao verificado]. Há repetição em todos os caminhos desses retornos, portanto são **classe sim como desencontro literal**; isso não demonstra erro de negócio [nao verificado].

Estruturas examinadas sem costura externa nominal encontrada: `S/foto_e4b.py:169` (`nascimentos`, só consumida internamente por `relato` em `:199`), `S/vigia_fio.py:810` (`carimbos_sem_lastro_por_dono`, só reduzida internamente em `:850-851`), `S/fio.py:2032-2033` (`batida_sem_origem_no_horizonte`, nenhum caller nominal de produção encontrado [nao verificado]). O último emite `fonte,n,esperado,dono,universo,novas,acervo,por_dono`; não contei um par inexistente.

Quanto à partição anterior: `nomear_marco_por_lampada.py:109` de fato repete o shape com `motivo='lampada_apagada'`, confirmado por `T/test_emissor_nomeia_o_marco.py:159-162`; é origem distinta do mesmo escritor físico `services/contexto.py:102`, para a normalização que você escolheu.

## 13/11 15:47 · sessao bcbd0a8413ab · - · - · origem=Ronald (tmux)

Censo adicional por AST das chamadas `JsonResponse`, `render`, `resposta_render`, `render_to_string`, seguido das rotas e dos leitores JS/templates. Nenhum teste executado. `app/chamados/urls.py:24-45` prende a identidade das rotas JSON; templates emitem as URLs concretas em `app/templates/chamados/partials/_lista_chamados.html:82-85`, `app/templates/core/_copiloto.html:16`, `app/templates/chamados/painel_gestao.html:59`, `:302`.

**JSON: 8 pares com leitura de corpo comprovada.** Agrupei ramos sucesso/erro da mesma função e todos os handlers do mesmo arquivo consumidor.

| Estrutura | Emissor | Chaves gravadas | Consumidor | Chaves lidas/defaults | Desencontro | Selo que prende |
|---|---|---|---|---|---|---|
| Resposta cobrança individual | `app/chamados/views_cobrar.py:33`, `:43`, `:53`, `:57` | `ok`; erro `erros[]`; sucesso `hora` | `app/templates/chamados/partials/_js_cobranca.html:51-56` | `ok`, `hora`; `erros || ['Falha na cobranca.']` | Não localizado | Par JS/schema: não localizado [nao verificado] |
| Resposta cobrança em massa | `app/chamados/views_cobrar.py:109`, `:139` | Prévia: `ok,previa,colabs,chamados,ja_cobrados,sem_app,fora_da_folha_exportada,fora_da_folha_sem_data,sem_data_por_modulo,a_enviar`; execução: `ok,enviados,ja_cobrados,sem_app,falhas,colabs,fora_da_area` | `app/templates/chamados/painel_gestao.html:59-69` | Prévia: `a_enviar,chamados,colabs,ja_cobrados,sem_app`; execução: `enviados,ja_cobrados,sem_app,falhas` | Consumidor omite exclusões da folha/área e `ok`; ausência de outros consumidores desses campos [nao verificado] | Par exato: não localizado [nao verificado] |
| Resposta validar inline | `app/chamados/views_cobrar.py:159`, `:167`, `:196-198`, `:213` | `ok,erros`; conforme ramo `aguardando_colab,resposta_no_fio[]`; sucesso `validadas,avisos,pendentes` | `app/templates/chamados/partials/_lista_chamados.html:145-161` | `ok,validadas,pendentes,aguardando_colab`; `erros[0] || avisos[0] || fallback`; `resposta_no_fio[].data || ''`, `.texto_do_fio || ''` | Não localizado | `app/chamados/tests/test_e3_validar_inline.py:33`, `:40`, `:48` cobre endpoint; par JS exato [nao verificado] |
| Resposta validar lote JSON | `app/chamados/views_cobrar.py:251-267` | `ok,validadas,recusados`; erros `erros[]`; sucesso `nao_gravados[]` | `app/templates/core/_copiloto.html:248-255` | `recusados && recusados.length`, `validadas`; não lê `ok`, `erros`, `nao_gravados` | `nao_gravados` não informado por este consumidor; `recusados=[]` produz “Todos entraram” mesmo que existam `nao_gravados`. Leitor HTML separado existe em `_lote_preview.html:9`, portanto não é chave órfã global | Par exato: não localizado [nao verificado] |
| Proposta lote JSON | `app/chamados/views_cobrar.py:285`, `:290-302` | `ok,refs,elegiveis,erros` na recusa; sucesso `elegiveis_chamados,rotulo_lote,amostra[{ref,colaborador,pergunta,resposta,hora_prevista,diferenca_min}],fora_do_lote,sem_classificacao` | `app/templates/core/_copiloto.html:228-242` | `fora_do_lote[*],elegiveis,sem_classificacao,amostra[].{colaborador,resposta,hora_prevista},amostra.length,refs` | Não lê `elegiveis_chamados,rotulo_lote,amostra[].{ref,pergunta,diferenca_min}`; ausência global desses leitores [nao verificado] | Par exato: não localizado [nao verificado] |
| Resposta desarquivar | `app/chamados/views_arquivar.py:62`, `:71`, `:89` | Recusa área `{ok:false,erro}`; outra recusa `{ok:false,erros[]}`; sucesso `{ok:true}` | `app/templates/chamados/partials/_lista_chamados.html:85`, `:120-134` | `ok`; `erros || ['Falha ao arquivar.']`; sucesso com `resgatar=true` não lê `ate` | **Confirmado: recusa área emite `erro`, leitor só procura `erros` e mostra fallback** | `test_f3_arquivar_porta.py:64-75` prende sucesso/trilha, não o par de erro |
| Resposta colaborador | `app/chamados/views.py:1045`, `:1055`, `:1068-1102`, `:1124` | `erro`; conforme ramo `estado`; sucesso `status,estado,msg,exige_doc` ou `status,aviso` | `app/templates/chamados/meu_atendimento.html:404-416`, `:432-444`; handler livre `:217-225` | Escapes/hora: `status`, `msg || fallback`, `erro || msg || fallback`. Handler livre não lê corpo nem status HTTP | `exige_doc`, `estado`, `aviso` não lidos por estes handlers. Em resposta livre, `.then()` mostra mensagem enviada sem inspecionar recusa HTTP (`:218-224`) | Par exato: não localizado [nao verificado] |
| Placar JSON | `app/chamados/views.py:846-862`; payload calculado em `app/chamados/services/painel.py:291-299` | Canônico calculado: `verbos_contagens,registros_n,cobranca,arquivados,equipe_janelas,resolvidos_auto,aceite`; ramo lavrado copia payload e acrescenta `lavrado_em` | `app/templates/chamados/painel_gestao.html:302-321` | `verbos_contagens.{validar,cobrar,decidir,arquivado}.n`; `equipe_janelas.{hoje,h48,h72,d7}`; `resolvidos_auto.{hoje,d7}`; se `aceite`, `{enviados,abertos,respondidos}.{hoje,d7}`. Sem default para subchaves obrigatórias | `lavrado_em,registros_n,cobranca,arquivados` não lidos por este JS; writer fora chamados do payload lavrado não foi recenseado nesta partição | Par schema exato: não localizado [nao verificado] |

Nos dois desencontros fortes de interpretação do resultado (`validar_lote→copiloto`, `desarquivar→JS`), dono provável **ESTRUTURA**, classe **sim**, pelos pares acima. Incidência em produção: [nao verificado].

Cinco emissores JSON diretos adicionais ficaram **sem leitor de corpo localizado**, portanto não formei pares artificiais:

- `app/chamados/views.py:1011`, `:1016`, `:1036`, `atribuir_chamado`: `{erro,area}` ou `{status,atribuido_a}`. Forms fazem POST normal em `app/templates/chamados/fio_colaborador.html:212`, `:220`; não foi localizado JS que leia o JSON.
- `app/chamados/views.py:1132-1136`, `marcar_lido_colaborador`: `{erro}` ou `{status}`; leitor nominal não localizado [nao verificado].
- `app/chamados/views.py:1144-1152`, `marcar_lidos_colaborador`: `{erro}` ou `{status,n}`. `app/templates/chamados/meu_atendimento.html:165-167` dispara fetch e ignora resposta.
- `app/chamados/views_arquivar.py:34`, `:48`, `arquivar_chamado`: `{ok,erro}` ou `{ok,ate}`. Existe função JS que suportaria o caso, mas a única chamada concreta localizada usa **desarquivar**, em `_lista_chamados.html:85`; par arquivar→JS [nao verificado].
- `app/chamados/views_arquivar.py:106`, `:148-149`, `arquivar_lote`: `{ok,erros}` ou `{ok,arquivados,fora_da_area,ate}`; consumidor UI nominal não localizado [nao verificado].

**Render: pares principais localizados.** Todos os campos abaixo são chaves de contexto; acesso de variável Django ausente nos corpos indicados não contém `.get` explícito. Defaults escritos nos templates são assinalados.

| Estrutura | Emissor/ponte | Gravadas | Consumidor e lidas | Desencontro forte / selo |
|---|---|---|---|---|
| Form abertura | `views.py:156-162`, `:187-214`, `abrir_chamado` | `modulo,titulo_pagina,taxonomia_json,taxonomia,tipos_para_colab,pode_abrir_para_colab` | `app/templates/chamados/form_abertura.html:2`, `:8`, `:21`, `:32`, `:39`, `:145`, `:179`: todas | Nenhum localizado; selo do par [nao verificado] |
| Form abertura para colaborador | `views.py:131-134`, `_abrir_para_colaborador` | Mesmas, recebidas por `ctx_taxonomia` | Mesmo template/linhas | Nenhum localizado; selo [nao verificado] |
| Detalhe GLPI | `views.py:262-269`, `detalhe_chamado` | `ticket,followups,local,chamado,erro,is_gestor,titulo_pagina` | `app/templates/chamados/detalhe.html:4`, `:23-32`, `:73-80`: `chamado,is_gestor,followups` | `ticket,local,erro,titulo_pagina` sem leitura nominal no template; `chamado=local` pode ser `None` apesar de `ticket` existir (`views.py:258-266`), mas template usa só chamado. Efeito sem local [nao verificado]; selo [nao verificado] |
| Tabela parcial GLPI | `views.py:340`, `partial_lista_chamados` | `tickets,is_gestor` | `app/templates/chamados/partials/tabela_chamados.html:1`, `:7`, `:17`, `:23`: ambas | Nenhum localizado; selo [nao verificado] |
| Painel chamado turno | `views.py:353`, `painel_chamado_turno` | `chamado,followups` | `app/templates/chamados/partials/painel_chamado_turno.html:8-13`, `:42-45`: ambas | Nenhum localizado; selo [nao verificado] |
| Detalhe local | `app/chamados/services/acoes_chamado.py:409-424`; ponte `views.py:550-551` | `agenda_proposta,chamado,followups,is_gestor,pode_agir,area_label,titulo_pagina,disputa_aberta,perguntas_disputa,todas_validadas,folha_julga_pontualidade,advertencias_ativas` | `app/templates/chamados/detalhe_local.html:5-6`, `:45-47`, `:105`, `:184`, `:289`, `:309-314`: todas exceto `titulo_pagina` e folha, esta repassada ao include | Folha é lida no include `_pergunta_card.html:43`; selo `app/chamados/tests/test_ui_resposta_diz_o_que_e.py:229`, `:258` |
| Meu atendimento | `views.py:633-651`, `meu_atendimento` | `colaborador,chamados,justificativas,pendentes_just,nao_lidos,pendencias,solicitacoes,nova_solicitacao_no_app,aba_solicitacoes,nao_lidos_pendencias,nao_lidos_solicitacoes,titulo_pagina,cor_disputas_map,regularizacoes_pendentes,ferias_pendentes,is_admin_ti,escapes_colab` | `app/templates/chamados/meu_atendimento.html:6`, `:48`, `:63`, `:68-81`, `:117`, `:134`: regularizações, flags, contadores, justificativas, pendências, solicitações; outros consumidos pelos includes abaixo | Campos extras sem leitura própria não bastam para ausência global; selo [nao verificado] |
| Fio colaborador | `views.py:697`, `fio_colaborador` | `colaborador,ficha,ultima_geo,geo_historico,colab_posto,chamados,resumo,voltar_url,is_gestor,is_admin_ti,pode_agir_map,area_label_map,cor_disputas_map,titulo_pagina` | `app/templates/chamados/fio_colaborador.html:5`, `:10`, `:15-22`, `:79`, `:112-126`, `:140`, `:164`, `:211`, `:225`: conjunto salvo flags/título sem leitura nominal própria | Selo contexto `app/chamados/tests/test_paridade_fio_modal_f8.py:73-83`; não cobre cada chave/template |
| Painel completo | `app/chamados/services/painel.py:413-445`, `:530-548`; `views.py:727`, `:762-767` | `chamados_data,total,abertos,arquivados,em_analise,resolvidos,emergencias,page_obj,paginator,f_status,f_colab,f_verbo,verbos_contagens,registros_n,resolvidos_janelas,resolvidos_auto,validacoes_janelas,equipe_janelas,dias_atraso,f_urgencia,f_q,f_empresa,f_praca,empresa_fora_rollout,empresas,pracas,fechados,ativos,historico,cancelados,titulo_pagina`; resumo acrescenta `modo_resumo,grupos,total_colabs`; view acrescenta `he_central` ou `None` | `app/templates/chamados/painel_gestao.html:41-44`, `:78-82`, `:105-120`, `:127`, `:136-191`, `:239`: verbos, contagens, HE, filtros, empresas, praças, modo; encaminha lista aos includes | Schema grande com chaves sem consumo direto; total de ausência global [nao verificado]; selo [nao verificado] |
| Lista detalhada painel | Mesmo serviço; ponte `views.py:740-743` e include `painel_gestao.html:239` | Mesmo contexto painel; `chamados_data[]` shape em `services/painel.py:385-405` | `app/templates/chamados/partials/_lista_chamados.html:3-8`, `:30-97`: filtros/page/rollout e itens `{obj,verbo,rotulo_linha,origem_rotulo,origem_destaque,estado_title,cobrado_hoje,n_followups,ultimo_fu,nao_lido,n_nao_lidos,horas_sem_resposta,sla_estourado,sla_alerta,sla_limite}` | Desencontro de chaves forte não localizado; cobertura integral [nao verificado] |
| Lista resumo painel | `app/chamados/services/painel.py:469-489`, `:493-494`, `:529-548`; ponte `views.py:740-743` | Contexto resumo; grupos `{cid,colab,chamados,cont,urgente,mais_antigo,n,acao,tem_proposta}`; itens `{id,obj,criado_em,verbo,ord,rotulo_linha,estado_title,origem_rotulo,origem_destaque}` | `app/templates/chamados/partials/_lista_resumo.html:5-28`, `:33-71`: filtros/page/paginator/contagens/rollout/grupos e seus itens | Internos de ordenação não lidos no template; ausência global [nao verificado]; selo [nao verificado] |
| Modal completo | `views.py:927-948`, `_resp_modal_full` | `colaborador,folha_julga_pontualidade,ficha,chamados,proposta_viva,pode_agir_map,area_label_map,is_gestor,is_admin_ti,modo_beneficio,chamados_orfaos_count,proximo_fio_pk,encerrados_juiz,contador_ativos,contador_msgs_mes,contador_disputas_abertas,cor_disputas_map` | `app/templates/chamados/partials/modal_fio.html:9-19`, `:26`, `:40`, `:74-76`, `:123`, `:172`, `:250`, `:382`, `:500`, `:606-651` | **Lê `pode_conferir_data` em `:172`, omitida por este emissor.** Parcial a grava em `fio.py:474`. Selo GET `test_paridade_fio_modal_f8.py:62-71` não prende essa chave |
| Modal parcial | `app/chamados/services/fio.py:461-482`; ponte `views.py:1398-1400`, e `services/fio.py:497` | Mesmo núcleo do completo, mais `pode_conferir_data`; **sem** três contadores nem `cor_disputas_map` | Mesmo template, especialmente `modal_fio.html:14-19`, `:76`, `:172` | **Lê contadores e `cor_disputas_map` que este emissor omite; completo grava `views.py:945-948`.** Selo do par/paridade de construtores não localizado [nao verificado] |
| Mensagens rápidas | `views.py:1000-1002`, `abrir_chamado_rapido` | `chamados` | `app/templates/colaboradores/partials/msgs_lista.html:1-9`: itera chamados/followups | Nenhum localizado; selo schema [nao verificado] |
| Verificação PDF | `views.py:1203`, `:1218`, `verificar_pdf` | Erro `erro,hash_consultado`; sucesso `registro,hash_consultado,autentico` | `app/templates/chamados/verificar_pdf.html:30-42`, `:54`: todas segundo ramo | Nenhum localizado; selo [nao verificado] |
| Lente raio-X | `views.py:1482-1489`, `raiox_lente` | `grupos,total,verbo,rotulo_verbo,cor_verbo,placar,empresas,aceite,empresa_id` | `app/templates/chamados/partials/_raiox_lente.html:3-8`, `:18`, `:27-40`: todas | Nenhum localizado; selo [nao verificado] |
| Regularização painel | `app/chamados/views_regularizacao.py:34`, `regularizacao_painel` | `colaborador,lotes` | `app/templates/chamados/regularizacao_painel.html:2-14`, `:23-31`, `:45`: ambas | Nenhum localizado; selo [nao verificado] |
| Regularização edição | `app/chamados/views_regularizacao.py:67`, `regularizacao_editar` | `reg,itens,editavel` | `app/templates/chamados/regularizacao_editar.html:2-7`, `:17-29`, `:42`: todas | Nenhum localizado; selo [nao verificado] |
| Busca arquivados | `app/chamados/views_arquivar.py:178`, `busca_arquivados` | `chamados,total,q,de,ate,agora` | `app/templates/chamados/partials/_busca_arquivados.html:7`, `:12`, `:17`, `:26`, `:29-46`: todas | Nenhum localizado; selo [nao verificado] |
| Meus arquivados | `app/chamados/views_arquivar.py:205`, `meus_arquivados` | `chamados,total,de,ate,agora` | `app/templates/chamados/partials/_meus_arquivados.html:9`, `:14`, `:23-35`: todas exceto `agora` | `agora` sem leitura nominal neste template; selo [nao verificado] |
| Prévia lote HTML | `app/chamados/views_cobrar.py:358`, `:362-368`, `lote_preview` | Recusa `recusa`; sucesso `linhas,n,rotulo_lote,total,fora,sem_marco,sem_marco_rotulo` | `app/templates/chamados/partials/_lote_preview.html:2-3`, `:13-26`: todas exceto `n`; linhas `{ref,pergunta,detalhe}` com `detalhe.esperada|default:'—'`, `delta_min` condicional; sem_marco `{nome,matricula|default:'-',perguntas,cadastro}` | `n` gravada sem leitura nominal; schema dos subitens vem do serviço de validação, não inventado pela view. Selo par exato [nao verificado] |
| Resultado lote HTML | `app/chamados/views_cobrar.py:378`, `:386`, `validar_lote_tela` | Recusa `recusa`; execução `resultado,quem` | `app/templates/chamados/partials/_lote_preview.html:2-9`: `resultado.{validadas,recusados[].{ref,motivo},nao_gravados[].ref}`, `quem` | Não lê `resultado.erros` mesmo quando o serviço devolve erro. Esquema/efeito desse ramo precisa consolidar com responsável por `validacao.py`; selo par [nao verificado] |

Nos dois pares modal, dono provável **ESTRUTURA**, classe **sim**, pelas omissões concretas acima; dados reais ou impacto de produção [nao verificado]. Acrescentar **`cor_disputas_map`** ao achado do parcial, além dos três contadores: comparação de `services/fio.py:461-482` com `views.py:948`, consumo `modal_fio.html:76`.

Leitores em includes que devem ser contados separadamente caso “módulo consumidor” inclua cada template:

| Emissor da estrutura | Consumidor adicional | Chaves |
|---|---|---|
| `views.py:633-651`, meu atendimento | `app/templates/ferias/_meu_atendimento_ferias.html:1-9` | `ferias_pendentes` |
| `views.py:633-651`, meu atendimento | `app/templates/chamados/partials/_card_atendimento.html:12`, `:219`, `:398` | `chamado` é alias do laço de `pendencias/solicitacoes` do pai (`meu_atendimento.html:117-144`); `escapes_colab` e `is_admin_ti` aparecem no corpo do cartão; linhas exatas de cada acesso além das citadas [nao verificado] |
| `views.py:697`, fio | `app/templates/core/_voltar.html:3-6` | `voltar_url` |
| `views.py:697`, fio; `views.py:930`, modal completo; `services/fio.py:464`, modal parcial | `app/templates/colaboradores/partials/_cabecalho_ficha.html:7-9` | `ficha` — são três emissores para um leitor |
| `services/acoes_chamado.py:422`, detalhe local | `app/templates/chamados/partials/_pergunta_card.html:43` | `folha_julga_pontualidade`; `pode_agir` em `:42` |
| `services/painel.py:413-445`, painel | `app/templates/chamados/partials/_paginacao_chamados.html:3-11` | `paginator,page_obj,f_status,f_urgencia,f_q,f_empresa,f_praca,f_verbo,total` |

A tabela principal contém **22 pares render**, além dos **8 pares JSON**; os includes acima são expansão adicional, não estão embutidos nesses números. Não some os totais aos outros agentes antes de deduplicar pares `services/painel→template`, `services/fio→template`, `services/acoes_chamado→template`, que posso ter sobreposto ao recorte deles.

Limites: JavaScript de terceiros/minificado foi excluído depois de uma busca inicialmente ruidosa; não atribuí consumo a formas HTML que só transportam POST; `resposta_acao`/`resposta_render` recebem parâmetros de envelope e têm escritor físico em `core/respostas.py`, fora do recorte próprio desta tarefa. Ainda há essas portas a consolidar se o responsável do núcleo não as tiver inventariado (`app/chamados/views.py:19`, `views_regularizacao.py:16`, `views_cobrar.py:311`).

## 13/11 15:47 · sessao 823646fecf8a · - · - · origem=Ronald (tmux)

Complemento de renderização, somente leitura. Fiz inventário AST de `render`/`resposta_render` nos três módulos pedidos e busca lexical de referências Django nos templates, removendo `{% comment %}`, separando variáveis de loop, aliases de `{% url ... as ... %}` e parâmetros locais de includes. Os caminhos dinâmicos da triagem vêm de `app/ponto/views.py:225-235`; os aliases de espelho e formulário estão em `:192` e `:1654`. Esquemas aninhados e todos os includes transitivos não ficaram fechados; portanto este complemento não constitui censo exaustivo de costuras. [não verificado]

As 28 linhas abaixo são pares de módulo renderizador × template direto; não são uma contagem completa de gravadores, pois `triagem_batida.py` também muta o mesmo contexto entregue por `views.py` (`app/ponto/services/triagem_batida.py:200-223`, `:299-300`, `:378-387`, `:478-486`, `:758-766`).

Abreviações: `V=app/ponto/views.py`; `T=app/templates/ponto/`. `R` lista raízes lidas no template, sem globais e variáveis locais. A ausência de leitura declarada abaixo é limitada ao template direto e aos includes expressamente examinados; ausência global permanece [não verificado].

| Estrutura / emissor | Chaves gravadas | Consumidor / raízes lidas | Desencontro / selo |
|---|---|---|---|
| Contexto espelho, `V:193` | `colab_param,colaborador,dias,semanas,hoje,escala_resumo,resumo_mes,carimbo_convencao,apur_ini,apur_fim,disputa_aberta,ultimas_batidas,mes,ano,mes_nome,mes_ant,ano_ant,mes_prox,ano_prox` | `T/espelho.html:7`: `colaborador,disputa_aberta,carimbo_convencao,apur_ini,apur_fim,resumo_mes,ultimas_batidas,escala_resumo,semanas,hoje,dias` | Navegação `mes/ano/...` e `colab_param` não são lidos diretamente; todos os aninhados [não verificado]. Teste parcial `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:46` |
| Contexto triagem apto, `V:249`, `:330`, `:337`, `:358` | Base `colaborador`; união dos enriquecimentos abaixo | `T/triagem/apto.html:4`: `sucesso,tipo_batida,hora_registro,colaborador,saida_sem_entrada,tem_regularizacao_pendente,qtd_regularizacao_pendente,turno_aberto` | `saida_sem_entrada` lida em `:39` não é escrita no contexto pelos escritores inspecionados; o serviço grava esse nome somente no contexto da disputa em `triagem_batida.py:737-739`. Selo do desencontro [não verificado] |
| Contexto triagem regularizar, mesmos renders | mesma união | `T/triagem/turno_regularizar.html:3`: `regularizacao_enviada,entradas_sem_saida,regularizacao_erros` | Campos existem nos escritores `triagem_batida.py:205`, `:299-300`; porém o seletor usa outra chave para chegar a esse template, descrita adiante. Teste dos dados `app/ponto/tests/test_f7_ramos_frios.py:62-75` |
| Contexto triagem confirmar, mesmos renders | base + `aguardando_confirmacao=True` em `V:329`; `turno_aberto` do serviço | `T/triagem/turno_confirmar.html:6`: `turno_aberto.entrada` | Compatibilidade da raiz constatada; selo do ramo `app/ponto/tests/test_f7_ramos_frios.py:128-142` |
| Contexto fora horário, `V:343` e seletor | base + `bloqueio_horario`, `anuencia_enviada` ou `anuencia_erro` do serviço | `T/triagem/fora_horario.html:3`: `anuencia_enviada`; `bloqueio_horario.{hora_prevista,hora_tentativa,diferenca_min,escala,chamado_pk,tipo_batida}` em `:15-25` | `anuencia_erro` escrita em `triagem_batida.py:384-387` não é lida pelo template nem pelo `base_triagem.html` examinado. `test_f7_ramos_frios.py:121-125` só confere a presença no contexto |
| Contexto bater V2, `V:357` | mesma união da triagem | `T/bater_ponto_v2.html:39`: `colaborador,sucesso,tipo_batida,hora_registro,aviso_do_dia,bloqueio_horario,aguardando_confirmacao,tipo_sugerido`; `colaborador.posto.{latitude,longitude,raio_metros}` em `:139-140` | Nenhuma ausência entre essas raízes constatada; selo integral [não verificado] |
| Contexto turnos abertos, `V:409` | `alertas,total,criticos,altos,normais,nivel_filtro` | `T/turnos_abertos.html:34-58`: todas as seis raízes | Sem desencontro de raiz; selo integral [não verificado] |
| Contexto offline, `V:488` | `pendentes,total_pendentes,com_gps,sem_gps` | `T/batidas_offline_pendentes.html:19-31`: todas | Sem desencontro de raiz; selo integral [não verificado] |
| Contexto prévia HE, `V:712` | `empresa,mes,ano,colab,motivo,voltar,r,q,linhas,itens,mostrado` | `T/gestao_he_periodo_previa.html:21-91`: todas; `r.periodo.0/.1,r.dias,r.exportada,r.erros`; `linhas[].{rotulo,antes,depois,diff}` | Esquema `linhas` produzido no mesmo render (`V:717-718`) coincide com leitura `:50-54`; demais aninhados [não verificado] |
| Contexto HE, `V:951` | `empresas,empresa,mes,ano,meses,anos,retrato,calculado_em,colabs,pracas,postos,totais,filtrou,teto_tira,q,f_praca,f_posto,f_estado,estados,pode_autorizar` | `T/gestao_he.html:23-325`: `pode_autorizar,empresas,empresa,meses,mes,anos,ano,q,retrato,calculado_em,totais,teto_tira,colabs,filtrou` | `pracas,postos,f_praca,f_posto,f_estado,estados` não são lidas no template direto examinado; aninhados [não verificado]. Teste parcial `app/ponto/tests/test_tela_gestao_he.py:82` |
| Contexto painel fechamento, `V:1006` | `colab,fechamento,mes,ano,justificativas,ausencias,fechamento_pk,periodo_fechado` | `T/partials/painel_fechamento.html:6-100`: todas | A referência `fechamento.pk` em `:92` está dentro de comentário Django e não deve contar. Guarda usa `fechamento_pk` em `:80`; hidden em `:98`. Selo de tranca `app/ponto/tests/test_m4_painel_e_reavaliar_leem_o_juiz.py:93-111` |
| Contexto lote fechamento, `V:1087` | `resumos,proximo_offset,restantes,qs_lotes` | `T/_fech_lote.html:8-9`: todas; inclui `_fech_linha.html` | **Faltam `empresa_id` e `periodo_fechado` exigidas pelo include**, conforme linha adicional abaixo. Testes de lotes `app/ponto/tests/test_selo_lotes_fechamento.py:57-94` cobrem quantidade/ordem, não essas duas chaves |
| Contexto fechamento principal, `V:1179` | `empresas,empresa_id,periodo_fechado,resumos,proximo_offset,restantes,qs_lotes,mes,ano,mes_nome,mes_ant,ano_ant,mes_prox,ano_prox,total_aprovados,total_pendentes,pode_aprovar,total,sem_calculo,sem_escala,n_ativos_u,n_deslig_u,n_outros_u,n_sem_ancora,n_turno_aberto,n_rescisao,n_fx0,n_fx50,n_fx80,n_fxok,n_fxcego` | `T/fechamento.html:2-266`: todas as raízes fornecidas | Sem ausência de raiz constatada; `resumos` inclui `_fech_linha.html`. Testes parciais de lotes acima |
| Contexto formulário justificativa, `V:1652`, `:1655` | `turnos_abertos,titulo_pagina` | `T/justificativa_form.html:16`, `:29`: `turnos_abertos` como pares `(entrada,saida)` | `titulo_pagina` não é lida diretamente; a navegação `voltar_url_tmp` é variável local criada por `{% url ... as ... %}` em `:4`, não chave ausente. Selo [não verificado] |
| Contexto painel justificativa, `V:1685` | `j,colab,historico,ausencias_dia,chamado,followups` | `T/partials/painel_justificativa.html:4-131`: todas | Sem ausência de raiz constatada; objetos/aninhados [não verificado] |
| Contexto fila justificativas, `V:1749` | `justificativas,status_filtro,empresa_filtro,q,total_pendentes,total_sla,total_hoje,total_backlog,empresas,titulo_pagina` | `T/fila_justificativas.html:40-98`: todas, salvo `titulo_pagina` | `titulo_pagina` não lida diretamente; selo integral [não verificado] |
| Contexto painel ausência, `V:2079` | `a,raiz,irmaos,cobre_parte_do_dia,tipos,parciais,pode_editar,escala_rotulo`; expansão `status_fundo,status_cor` de `V:2046-2049`; expansão `historico,resumo_historico,periodo_aquisitivo,pdf_historico_url` de `V:2032-2036` | `T/partials/painel_ausencia.html:23-309`: todas, mais `volta_url` em `:282` | **`volta_url` não é gravada pelo render nem pelos helpers inspecionados.** Os processors configurados estão em `app/config/settings/base.py:76-84`; nenhum processor próprio inspecionado fornece essa chave. Selo da chave [não verificado]. Teste parcial de histórico `app/ponto/tests/test_drawer_ausencia_no_molde.py:112-121` |
| Contexto lista ausências, `V:2244` | `ausencias,pendentes,colaboradores,tipos,ids_alerta_atestado,mes,ano,mes_nome,mes_ant,ano_ant,mes_prox,ano_prox,filtro_colaborador,filtro_tipo,filtro_efeito,cnt,count_total,count_pendente,count_aprovada,count_rejeitada,filtro_status,de,ate` | `T/ausencias.html:2-142`: todas, exceto `pendentes`; `cnt.{abona,desconta,suprime}` em `:65-67` | `pendentes` não lida diretamente; aninhados [não verificado] |
| Contexto lançar ausência, `V:2424`, `:2459`, `:2460` | `colaboradores,tipos,parciais` | `T/lancar_ausencia.html:19-93`: todas | Sem ausência de raiz; `voltar_url_tmp` é alias local do template `:4`. Selo [não verificado] |
| Contexto submeter ausência, `V:2559`, `:2560` | `tipos,tipos_pagos,parciais,titulo_pagina` | `T/submeter_ausencia.html:24-77`: `tipos,tipos_pagos,parciais` | `titulo_pagina` não lida diretamente; selo [não verificado] |
| Contexto histórico atestados, `V:2882` | `colaborador,atestados,total_dias,empresa_paga,inss_assume,ano,alerta_15dias` | `T/historico_atestados.html:2-37`: todas | Sem ausência de raiz; selo [não verificado] |
| Contexto saída retroativa, `V:3124` | `justificativa,titulo_pagina` | `T/materializar_saida_retroativa.html:21-32`: `justificativa` | `titulo_pagina` não lida diretamente; selo [não verificado] |
| Contexto regularizar retorno, `V:3235` | `batida_entrada,data_turno,horas_aberto,dias_ausente,turnos_orfaos,titulo_pagina` | `T/regularizar_retorno.html:18-20`: `batida_entrada,horas_aberto,dias_ausente` | `data_turno,turnos_orfaos,titulo_pagina` não lidas diretamente; selo [não verificado] |
| Contexto geo, `V:3254` | `batida,resumo` | `T/partials/geo_detalhe_turno.html:4-17`: `batida.{colaborador.nome,timestamp}`, `resumo.{total_pings,pings_dentro,pings_fora,pings_mock,distancia_maxima,amostras}`; amostras `.{mock,dentro,timestamp,distancia,provider}` em `:27-34` | Raízes compatíveis; esquema do produtor de `resumo` [não verificado] |
| Contexto fila flip, `V:3307` | `itens,placar,dias,n_auto` | `T/flip_fila.html:5-15`: todas; `itens[].{cid,nome,dia,hora,bid,de,para,motivo}` | Raízes compatíveis; esquema de `itens` [não verificado] |
| Contexto tipos ausência, `app/ponto/views_tipos_ausencia.py:37` | `grupos,usos,total,titulo_pagina`; `grupos[].{efeito,rotulo,itens}` em `:36` | `T/tipos_ausencia.html:24-45`: `total,grupos[].{rotulo,itens},usos`; `itens` são objetos `TipoAusencia` | `grupos[].efeito` e `titulo_pagina` não lidas diretamente; selo [não verificado] |
| Contexto painel tipo ausência, `app/ponto/views_tipos_ausencia.py:48` | `t,usos,efeitos` | `T/partials/painel_tipo_ausencia.html:1-40`: todas; `t.{pk,nome,codigo,efeito,exige_documento,bloqueia_ponto,parcial,medico,ativo}` | Sem ausência de raiz; atributos de modelo não tratados como chaves de dict. Selo [não verificado] |
| Contexto motivos, `app/ponto/views_motivos_retratacao.py:38` | `motivos,usos,total_usos,titulo_pagina` | `T/motivos_retratacao.html:12-31`: `motivos,total_usos,usos.items` | `titulo_pagina` não lida diretamente; selo [não verificado] |

Duas costuras transitivas que merecem linha própria na consolidação:

| Estrutura | Emissor | Consumidor | Chaves lidas / desencontro | Selo / dono / classe |
|---|---|---|---|---|
| Contexto do lote herdado pelo include | `app/ponto/views.py:1087-1092`: `resumos,proximo_offset,restantes,qs_lotes` | `app/templates/ponto/_fech_linha.html:13`, `:42`, `:47`, alcançado por `_fech_lote.html:8` | Lê `empresa_id`, `periodo_fechado`, além de `r` local a `resumos`. Os dois primeiros faltam no contexto do lote. A página principal os grava em `views.py:1181-1182` | Selo específico não localizado [não verificado]; ESTRUTURA provável; classe sim por ausência entre gravador e leitor |
| Contexto da triagem no seletor | `app/ponto/services/triagem_batida.py:206`: `tem_bloqueio_turno` | `app/ponto/views.py:229`: `.get('entrada_bloqueada_turno_aberto')` | Escritor e seletor usam nomes diferentes. O ramo regularizar depende da segunda chave (`views.py:230`). Nenhum escritor da segunda no corpo inspecionado; total global [não verificado] | `app/ponto/tests/test_f7_triagem_caracterizacao.py:78` afirma `tem_bloqueio_turno`; não prende correspondência com o seletor. ESTRUTURA provável; classe sim |

Globais não devem virar falsos desencontros: `request` e `messages` vêm dos processors configurados em `app/config/settings/base.py:78-80`; `base_tpl` vem de `app/core/context_processors.py:210`. Nomes de loops (`r`, `dia`, `c`, `h` etc.), aliases de URL e parâmetros locais de includes não são exigências do dict raiz. Os atributos dentro de objetos Django também não foram confundidos com chaves de dict.

As lacunas precisas são: todos os campos aninhados de `dias/semanas/resumo_mes/escala_resumo`, `resumos[].{pront,folha,causa}`, `colabs` da HE, `historico/resumo_historico` e todas as referências herdadas dos layouts/includes comuns. Seus escritores e consumidores completos não foram fechados neste complemento. Total de costuras do módulo e quantidade completa sem selo permanecem [não verificado].

## 13/11 15:46 · sessao 3a3451fa21d5 · - · - · origem=Ronald (tmux)

Consolidado por **estrutura × módulo emissor × módulo consumidor**, incluindo auxiliares que efetivamente leem o mesmo dict e excluindo caches que apenas transportam:

- `celulas_do_periodo`: **15 módulos consumidores identificados**; referências enviadas na mensagem anterior. Escritor `escala/alimentacao.py:73-78`. Ausência de consumidores indiretos adicionais `[nao verificado]`.
- `folgas_do_periodo`: **1 módulo consumidor efetivo**, `escala/models.py:1281`, chave dinâmica `ec.pk`, default `frozenset()`. Escritor `escala/alimentacao.py:97-102`. Selos de cadeia `escala/tests/test_selo_alimentacao_dias.py:59-73,96-108`.
- `escalas_do_periodo_por_colab`: **6 módulos consumidores**: `escala/folgas.py:79`; `escala/services/cadastro_realidade.py:511,528`; `escala/services/propositor.py:442,446`; `ponto/turnos.py:2361,3038,3058`; `ponto/portas/regen_exportada.py:147`; `folha/previa.py:81`. Escritor `escala/alimentacao.py:145-160`. Defaults `[]` ou `()` para chave cid ausente. Selo explícito de cadeia para turnos: `escala/tests/test_o131_um_carregador_de_candidatos.py:192-224`; demais selos específicos `[nao verificado]`.
- `vinculos_do_dia_carregado`: **3 módulos consumidores**, `escala/proposta_pdf.py:36`; `chamados/services/validacao.py:902-903`; `ponto/management/commands/gerar_celulas.py:321`. Escritor `escala/alimentacao.py:329-331`, chave cid, valor EC ou None. Selos específicos `[nao verificado]`.
- `vinculos_dos_dias`: **4 módulos consumidores**, `colaboradores/management/commands/marcar_feriado_do_posto.py:110`; `chamados/services/fila_por_causa.py:176`; `ponto/services/cadastro_futuro.py:40-41`; `ponto/services/vigia_cadastro_futuro.py:56`. Escritor `escala/alimentacao.py:350-358`, chave `(cid,data)`; primeiro consumidor usa `.get` default None, demais `[]`. Selos específicos `[nao verificado]`.
- kwargs de alimentação utils → models: **1 costura**, emissores `escala/utils.py:2215,2218,2265,2269,2291`; chaves opcionais `celulas,folgas`, somente valores não None; consumidores `escala/models.py:1069,1098,1166`, defaults None. Selo da cadeia específica `[nao verificado]`.

São **30 costuras adicionais identificadas** nesse bloco. Nenhum desencontro de chave demonstrado nesse bloco; exaustividade dos aliases indiretos `[nao verificado]`.

Para `grade_da_celula`/`remapear_na_regua`, considerei o mesmo contrato de grade produzido pelo módulo `escala/services/leitor_celula.py`, agrupando as duas funções na costura de espelho. O inventário entregue tem **23 módulos consumidores efetivos**:

| Módulo consumidor | Leitura | Selo da cadeia ou seletor |
|---|---|---|
| `relatorios/furos.py` | `:132-139,194-206` | sim: `escala/tests/test_f1c_contrato.py:61-62` |
| `escala/services/furos_diarios.py` | `:18-22,45-54` | sim: `escala/tests/test_cobparc_rotulo_furos_diarios.py:58-67` |
| `inteligencia/calculadores.py` | `:48-64,147-161,333` | sim: `inteligencia/tests/test_t42_calculadores_celula.py:25-30` |
| `ponto/worklist.py` | `:18-21` | sim: `ponto/tests/test_worklist_ata.py:44-68` |
| `colaboradores/services/calendario.py` | `:353-385` | sim: `colaboradores/tests/test_relogio_riscado_calendario.py:86-106` |
| `ponto/services/bordas_realizado.py` | `:49-51,71-87` | cadeia específica `[nao verificado]`; testes do predicado `ponto/tests/test_bordas_realizado.py:119-152` |
| `chamados/services/materializacao.py` | `:234-243,284-300` | sim: `chamados/tests/test_p71_marco_sem_hora.py:31-46` |
| `chamados/services/declaracao_texto.py` | `:104-133` | sim: `chamados/tests/test_hx_ata_nao_sabe.py:57-99` |
| `chamados/services/disputa_emissao.py` | `:84-85` | `[nao verificado]` |
| `chamados/services/tarefa_do_dia.py` | `:88-89,461-467,499-501` | sim: `chamados/tests/test_e2_tarefa_do_dia.py:603,1380-1391` |
| `ponto/management/commands/propor_reorganizacao.py` | `:66,91-112` | `[nao verificado]` |
| `ponto/management/commands/emitir_furo_retroativo.py` | `:90-102` | sim: `ponto/tests/test_t42_furo_retroativo_celula.py:20-25` |
| `ponto/management/commands/supra_juiz.py` | `:205-224` | sim: `ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:164,215-230` |
| `ponto/supra_juiz.py` | `:70-71,222-530` | sim: `ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:41-70` |
| `folha/export.py` | `:1095-1106`, wrapper `:338` | sim: `folha/tests/test_borda_celula.py:62-68` |
| `folha/causas.py` | `:35-43,58,84-100` | cadeia específica `[nao verificado]`; teste de contrato sintético `folha/tests/test_causas_sem_previsao.py:24-67` |
| `ponto/services/fechamento.py` | `:1020,1043-1050` | sim: `ponto/tests/test_o48_sem_celula_nao_e_zero.py:96-185` |
| `ponto/services/dia_pago.py` | `:387-400` | cadeia específica `[nao verificado]`; leis do consumidor `ponto/tests/test_retrato_exportada.py:46-65` |
| `ponto/management/commands/mapa_divergencia.py` | `:99-155,174-194` | `[nao verificado]` |
| `ponto/propositor.py` | `:209-213` | `[nao verificado]` |
| `chamados/juizes.py` | `:438` | cadeia específica `[nao verificado]` |
| `escala/utils.py` | `:845-849,957-965,1086-1110` | sim para isenção: `escala/tests/test_leitor_isento.py:64-90`; demais trechos `[nao verificado]` |
| `ponto/services/espelho.py` | `:519-524,677-685,856,892-893` | sim: `escala/tests/test_fronteira_remap.py:64-91` |

Nesse bloco de grade: **23 costuras identificadas; 14 com selo de cadeia/seletor localizado e 9 sem selo específico localizado** — “sem selo” como ausência de localização nesta leitura, não prova de inexistência `[nao verificado]`.

Nenhum desencontro novo foi fechado aqui. As variantes sem célula omitem campos específicos como `celula_id/n_ok/n_casadas`, enquanto os consumidores localizados usam guardas ou `.get`; produtores e variantes estão em `escala/services/leitor_celula.py:390-445`. `futuro/fora_janela` lidos no espelho são acrescentados pelo mutador `escala/utils.py:951-965`, chamado em `ponto/services/espelho.py:521`; não são falta demonstrada no contrato.

**Total dos dois blocos adicionais: 53 costuras identificadas**, além das 13 do levantamento anterior; unicidade com as costuras já enumeradas pelo root exige consolidação `[nao verificado]`.

## 13/11 15:45 · sessao e4cbbc1b1738 · - · - · origem=Ronald (tmux)

Inventário entregue nas quatro mensagens, com emissores, chaves, consumidores, defaults e selos localizados. Correções posteriores prevalecem: `cadastro_realidade.lista` possui também leitores em `vigia_fio`, `fechar_chamados_vinculo_na_lista` e `fechar_avisos_regua_na_lista`; o diagnóstico `n_dias` é consumido em `ponto/services/diagnostico_escala.py:240`.

Limite principal: a busca estática não prova resolução completa de aliases, chamadas dinâmicas nem leitores JavaScript de APIs; completude global `[nao verificado]`. Nenhum arquivo foi alterado e nenhum banco, log ou teste foi executado. [nao verificado]

## 13/11 15:45 · sessao 2c14b553a604 · - · - · origem=Ronald (tmux)

Censo local entregue nas mensagens anteriores: 28 pares nominais de `CelulaDia.ata`, 20 de DNA, 7 de JSON do fechamento e 17 linhas de métricas antes da deduplicação com o censo principal; completude global e total final [não verificado].

Ajustes essenciais para consolidar:

- Incluir `desfazer_julgamento_da_futura` como escritor da variante `ata=None`. `app/ponto/portas/celula.py:1165`.
- Agrupar os leitores de `chamados/juizes.py` numa única costura por estrutura; há leituras em `:450`, `:965` e `:1354`.
- Se repasses indiretos contam, acrescentar ATA/DNA → `ponto/turnos.py`, que carrega células e delega a `periodos_do_dia`. `app/ponto/turnos.py:1582`, `app/ponto/turnos.py:1586`.
- `DiaPago.detalhe`: escritores confirmados em `app/ponto/services/dia_pago.py:346`, `:358`, `:474`, `:512`; leitor nominal confirmado somente no mesmo módulo, em `:897`. Leitor externo da estrutura [não verificado].
- `Advertencia.detalhe_json`: emissor encontrado fora de ponto, em `app/chamados/services/acoes_chamado.py:203`; não entra como estrutura escrita por ponto.
- `ata.precedencia`: escritor em `app/ponto/services/cartorio.py:2000` e `app/ponto/portas/celula.py:1213`; leitor de negócio em outro módulo [não verificado].
- `diff_calculador.py:546` lê `Rubricas.detalhe`, não `DiaPago.detalhe`: tipo e atribuição em `app/ponto/management/commands/diff_calculador.py:427`, `:502`.
- Não tratar chaves diagnósticas como absolutamente órfãs quando existe leitura opaca do blob; hash do fechamento inclui ambos os JSON em `app/ponto/portas/regen_exportada.py:56`, `app/ponto/management/commands/lavrar_dias_pagos.py:70`.

Selos conferidos incluem transporte de isenção, cobertura parcial e métricas C8/C12; vários outros testes só cobrem fixtures ou leitores isolados, portanto não demonstram a costura completa. `app/ponto/tests/test_carimbo_isencao_lampada.py:144`; `app/escala/tests/test_cobparc7_leitor_transporta_a_lampada.py:29`; `app/ponto/tests/test_c2_vinculo_vira_lista.py:73`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:84`; `app/folha/tests/test_retrato_folha_quadros.py:1464`.

## 13/11 15:45 · sessao 73e2ed51da70 · - · - · origem=Ronald (tmux)

Complemento dos retornos, com correções para a consolidação:

| Estrutura | Emissor/chaves | Consumidor/chaves | Desencontro | Selo |
|---|---|---|---|---|
| `com_nomes` | `ponto/services/abriu_nao_bateu.py:117`: inicia `mudos,recusados`; `:147-149` acrescenta `total,abriram,janela_horas`; retorna em `:150` | `ponto/management/commands/lavrar_abriu_nao_bateu.py:35,36,40,49,51`: `total,mudos,recusados` | **Não** quanto a `total`: o coletor de dict literal perderia a mutação incremental. | Selo desta costura **[não verificado]**. |
| diagnóstico | `ponto/services/diagnostico_escala.py:221-247`: `colaborador,periodo,dias,so_os_dias_que_discordam,paridade_real,horario_mediano,dias_fora_do_horario,troca_de_ritmo,termometro,destino_sugerido,template,pausa_nao_cadastrada,acoes,fonte` | `api/views_mensageria.py:2008-2010`: recebe inteiro, acrescenta `ano_assumido`, serializa | Não há leitura nominal incompatível nesse consumidor. | **Sim**, `api/tests/test_diagnostico_escala.py:122-128` verifica endpoint e campos do diagnóstico. |
| turno órfão | `ponto/utils_turno_orfao.py:37-42`: itens `pk,data,hora,horas`; envelope `tem_bloqueio,tem_regularizacao_pendente,entradas_pendentes,qtd_pendentes` em `:60-65` | `chamados/detector_anomalias.py:33-43`: flags default `None`; `entradas_pendentes=[]`; itens `horas=0,hora=None,pk=None` | Não quanto aos nomes lidos. `data` do item é civil (`utils_turno_orfao.py:39`); o consumidor deriva o dia do turno pelo `pk`, em `detector_anomalias.py:43-48`. | Selo entre estes dois módulos **[não verificado]**. Testes do produtor: `ponto/tests/test_bloqueio_orfao_nucleo.py:33-36`, `test_o247_retratada_fora_do_pareamento.py:86-114`. |
| turno órfão | mesmo produtor | `api/views.py:327,353` | `_bloq['entradas_pendentes']` aparece sintaticamente, mas está no ramo falso do ternário: `regularizacao_obrigatoria=False` em `:330`. Leitura efetiva dessa chave nesse caminho **não acontece pelo corpo mostrado**. | `api/tests/test_api_me_nao_bloqueia.py:55-66` verifica órfão existente e resposta `entradas_pendentes=[]`. |
| turno órfão | mesmo produtor | `api/views_core.py:315,341` | Mesma leitura nominal inativa: flag fixada em `False` em `:318`. | Selo específico deste kernel **[não verificado]**. |
| resumo do espelho | `ponto/services/espelho.py:568`: `grade_pelo_builder=True` somente no fallback | `folha/porta_export.py:514`: `r.get('grade_pelo_builder')`, default `None` | Presença condicional; nenhum nome divergente. Se a régua contar ausência opcional como desencontro, registrar como **opcionalidade**, não como incidente. | **Sim**, `ponto/tests/test_o81_celula_da_competencia.py:59-72` prende os nomes dos dois lados. |
| resumo do espelho | `ponto/services/espelho.py:910-912`: acumula `total_relogio` somente havendo dia dentro da janela; não inicializado em `:1230-1232` | `folha/porta_export.py:511`: `r.get('total_relogio') or 0` | Presença condicional, default explícito no leitor. Efeito problemático **[não verificado]**. | Selo exato produtor–consumidor **[não verificado]**; teste do campo no produtor em `ponto/tests/test_e5_topo_soma_das_linhas.py:30`. |
| mesmo resumo | mesmo escritor `:910-912` | `templates/ponto/espelho.html:47-49`: testa e imprime `resumo_mes.total_relogio` | Presença condicional, condição explícita de exibição. | Selo exato desta costura **[não verificado]**. |
| resumo do espelho | `relatorios/cartao_pela_celula.py:321-345`: acrescenta `fonte_dos_totais` só quando `_comp` existe; recebe o dict de ponto em `ponto/services/espelho.py:1283-1284` | `api/views.py:1660`: `.get('fonte_dos_totais')`, default `None` | Presença condicional; nenhuma incompatibilidade de nome. | **Sim**, `api/tests/test_bug139_espelho_app_mesma_fonte.py:109-118` sela chaves do payload; `api/tests/test_espelho_app_fonte_e_geometria.py:85-105` sela valores alternativos. |
| mesmo resumo | mesmos escritores | `colaboradores/services/ficha.py:200-209`: `total_trabalhadas,total_extras,turnos,turnos_abertos`, todos com `or 0`; `fonte_dos_totais` default `None` | Presença condicional da fonte; nenhuma chave incompatível encontrada nesse corpo. | Selo específico da ficha **[não verificado]**. |

Para o dicionário do espelho não limitar o inventário ao literal do `return`: o envelope está em `ponto/services/espelho.py:1294-1296`; `resumo` recebe mutações em `:1249-1270`, por `montar_dias` em `:1262-1266`, e por `folha_manda` em `:1283-1284`. `montar_dias` ainda grava `datas_turno_aberto` (`:471`), `datas_saida_orfa` (`:617`), somas pelas chaves de `somar_periodos` (`:441`, esquema `:77-79`) e os campos condicionais acima.

Nenhum desencontro novo inequívoco confirmado nesse complemento; preservo D1–D7 do relatório anterior. Ausência global de outros desencontros **[não verificado]**.

## 13/11 15:44 · sessao 3432109c0e42 · - · - · origem=Ronald (tmux)

Censo do recorte enviado em mensagens com todos os sítios e chaves. Nenhum arquivo criado, editado ou apagado; nenhum teste executado. [nao verificado]

Desencontros confirmados:

| Estrutura | Emissor | Consumidor | Desencontro | Selo | Dono / classe |
|---|---|---|---|---|---|
| WebPush | `app/chamados/push_utils.py:28` | `app/static/sw.js:89` | Emite `requireInteraction`; lê `urgente \|\| false`. Emite `url`; clique abre `/` sem lê-la (`app/static/sw.js:102`). | Não localizado [nao verificado]; `app/chamados/tests/test_push_fcm.py:106` verifica somente envio chamado. | ESTRUTURA provável; sim. |
| Ticket GLPI | `app/chamados/glpi_client.py:138` | `app/templates/chamados/partials/tabela_chamados.html:19` | Template lê `urgencia`, ausente no formatador; termina em “Normal”. `assunto` e `colaborador_nome` também ausentes, com alternativas explícitas (`:21`, `:24`, `:30`). | Não localizado [nao verificado]. | ESTRUTURA provável; sim. |
| Followup GLPI | `app/chamados/glpi_client.py:209` | `app/templates/chamados/detalhe.html:80` | Grava `id,data,conteudo`; template lê `autor_tipo,autor_user,criado_em,conteudo`. Transporte confirmado em `app/chamados/views.py:246` e `:262`. | Não localizado [nao verificado]. | ESTRUTURA provável; sim. |

Portas auditadas sem incompatibilidade demonstrada: entradas `defaults/**lookup/**campos` dos chamados (`app/chamados/models.py:834`, `:1055`); entradas das perguntas (`:2777`, `:2795`); decisões (`:3944`); descarimbo em lote (`:3253`); texto (`app/chamados/services/texto.py:21`); leitura de notas (`app/chamados/services/nota.py:126`); anotação de batidas (`app/ponto/registro_batida.py:235`).

Carimbo do fio possui dois módulos leitores comprovados: `bin/fio_estado.py:121` e `app/core/management/commands/pipeline_placar.py:92`, alimentados por `app/chamados/management/commands/vigiar_fio_morto.py:228`. Selos encontrados: `app/chamados/tests/test_o261_vigia_fio.py:787`, `app/core/tests/test_fio_mudo_placar.py:147`, `:197` e `:227`.

Logs: inventário enviado de 47 sítios iniciais mais seis sítios descobertos por resolução de aliases, com consumidores integrais `core.observ.evento` (`app/core/observ.py:94`), `evento_kw` (`:142`) e `registrar_log` (`app/core/auditoria.py:8`). Normalizar pela unidade final solicitada pelo coordenador: estrutura × módulo emissor × módulo consumidor; não somar sítios como costuras.

A persistência do outbox fica fora de `chamados`: `app/ponto/portas/celula.py:954`. As origens em chamados e o payload intermediário foram enumerados; o primeiro grava `{celula_id,colaborador_id,data}`, enquanto `app/chamados/models.py:3229` produz as tuplas intermediárias. Considerar essa fronteira explicitamente na consolidação.

Completude global, execução dos selos e efeitos em produção: [nao verificado].

## 13/11 15:44 · sessao 219ef396477d · - · - · origem=Ronald (tmux)

Recorte concluído por busca estática de chamadas, retornos dict, `render`, chaves `.get`/subscritas e testes. Nenhum arquivo alterado. Testes não executados. A busca não demonstra exaustividade de chamadas dinâmicas, consumidores externos ao repositório ou dados de produção `[nao verificado]`. [nao verificado]

Nos quadros abaixo, **“sem selo” significa teste produtor+consumidor não localizado `[nao verificado]`**, e não prova de inexistência. **Classe** está indicada como “sim” apenas para desencontro de chave demonstrado; diferenças opcionais aparecem nomeadas sem afirmar defeito operacional. [nao verificado]

### Schemas compartilhados

**W — payload wizard.** Emissor `app/escala/views_wizard.py:20-59`: `template_nome,apelido,hora_inicio,hora_fim,hora_inicio_intervalo,hora_fim_intervalo,hora_inicio_intervalo2,hora_fim_intervalo2,intervalo_duracao_min,noturno,permite_hora_extra,he_extra_antes_min,he_extra_depois_min,folga_calendario_padrao_id,folgas,dias_trabalho_semana`; apenas criação acrescenta `tipo_ciclo,tipo_base,tipo_base_ref`; apenas POST com `horarios_por_dia` preenchido acrescenta essa chave (`:49-59`). Outro emissor da mesma porta, fora do recorte, grava apenas `template_nome,folgas`: `app/chamados/services/limbo_folgas.py:92`.

**P — envelope e proposta viva.** Emissor `app/escala/services/propositor.py:465-467,585-589`: envelope `fonte,n,propostas,universo,janela_dias,ritmo_discordante,ritmo_discordante_vinculos,pausa_nao_cadastrada,pausa_nao_cadastrada_vinculos`. Escritores internos das propostas: `:184-195,299-305,319-323,339-342,363-375`, chaves comuns `tipo,template_id,template,ancora,casa_hoje,casaria,previsto_novo`; ramos aplicáveis acrescentam `sobra`; alguns ramos acrescentam `motivo`. Enriquecimento `:539-561`: `colaborador_id,colaborador,posto,vinculo_id,template_atual,ancora_atual,razao_fora_dentro,dias_discordantes,entrou_por,dias_batidos,dias_previstos,aplicar_desde`; `motivo` pode ser reescrito em `:568-570`; `chamados_que_morreriam` nasce em `:572,575-576`, contendo `{chamado_id,dia}` escrito em `:634-635`. `previsto_novo` é removido em `:577`, salvo ramo `so_existencia=True`, que retorna antes da remoção (`:571-574`).

**D — detector v0.** Emissor `app/escala/detector_proposta.py:123-132,153-156`: `padrao,proposta,confianca,porque`; proposta preenchida contém `tipo_escala_id,codigo,descricao,ciclo,horario`. `padrao` vem de `:63-65,82-92`: `dias,marcos_moda,entrada,saida,duracao_h,dispersao_entrada_min,dows,padrao_dias`; ramo com dados também grava `marcos_freq`.

### Portas e propostas: 17 pares identificados

| ID / estrutura | Emissor / gravadas | Consumidor / lidas | DESENCONTRO | Selo que prende | Dono provável / classe |
|---|---|---|---|---|---|
| P01 W | `views_wizard.py:20-59`, W | `services/cadastro_tipo.py:148-267`: W por `.get`; `template_nome→''`, ciclo/base→`12x36`, campos texto→`''`, bool via `bool`; duração só altera se dígitos; HPD inválido→`{}`; lê presença de `descricao` em `:185` | `descricao` nunca emitida pelo wizard; HPD pode faltar e é lido por `.get` em `:217`; demais omissões condicionais têm guardas. Trata-se de protocolo opcional, sem dano demonstrado | **sim**, `tests/test_wizard_views.py:28-49,51-67` percorre POST→payload→service→modelo; ponte duração em `tests/test_wizard_intervalo.py:76-89` | ESTRUTURA provável; classe: sim, ausência opcional pelo critério literal |
| P02 ajustes `**aj` | `services/escala_auto_executor.py:65-84`: `_delta_blocos`, seis horários no A7; pausa/duração no A4; `folga_dia_semana` no FOLGA | `services/cadastro_tipo.py:443-445`: `pop('_delta_blocos',0)` e todos demais itens via `setattr` | A4/FOLGA não emitem `_delta_blocos`; default explícito 0. Sem chave de ajuste descartada | **sim**, `tests/test_escala_auto_executor.py:66-84` percorre executor→derivação e confirma pausa do novo template | ESTRUTURA provável; classe: sim, opcional |
| P03 P | `services/propositor.py:465-589`, P | `management/commands/propostas_de_escala.py:39-98`: `propostas,colaborador_id,vinculo_id→None,universo,n,colaborador,tipo,template_atual,ancora_atual,casa_hoje,dias_batidos,motivo` no ramo grade; `template,ancora,casaria,sobra` no outro; `aplicar_desde,chamados_que_morreriam[].chamado_id` | Guardas dos ramos respeitam a ausência de `sobra` na grade. Chaves órfãs globais abaixo | Sem selo real localizado; `tests/test_o261_v_fase_na_lista.py:527,539` usa mocks do produtor | ESTRUTURA provável; classe não demonstrada |
| P04 P | mesmos escritores P | `management/commands/ritmo_discordante.py:20-22`: `ritmo_discordante_vinculos→[]`, itens `colaborador_id,ciclo,ritmo,proposta`; `ritmo_discordante→0` | Chaves emitidas nos dois retornos do envelope | Sem selo localizado | ESTRUTURA provável; classe não demonstrada |
| P05 P | mesmos escritores P | `app/chamados/services/painel.py:522-524`: `propostas[].colaborador_id`; produtor chamado com `so_existencia=True` | Esse ramo preserva `previsto_novo` no dict (`propositor.py:571-577`), consumidor não lê; nenhum dano demonstrado | Sem selo produtor real localizado | ESTRUTURA provável; classe não demonstrada |
| P06 P | mesmos escritores P | `app/colaboradores/services/ficha.py:176-184`: `propostas,tipo`; `.get(template_atual,template,ancora,casa_hoje,casaria,dias_batidos,aplicar_desde)→None`; `chamados_que_morreriam→[]` | Chaves lidas são emitidas; consumidor substitui janela por literal `28` (`:181`) embora envelope tenha `janela_dias` | Sem selo produtor real localizado | ESTRUTURA provável; classe não demonstrada: nome/significado de janela não provado divergente |
| P07 P | mesmos escritores P | `app/chamados/services/proposta_no_fio.py:41-42,68-92,107-114,141-146`: `n→None,propostas`; `dias_batidos,casa_hoje,casaria,aplicar_desde`; `chamados_que_morreriam→[]`, `chamado_id`; `.get(vinculo_id,tipo,ancora,ancora_atual,template,template_atual,motivo,entrou_por)→None`; assinatura lê `tipo,template_id,ancora,aplicar_desde` em `propositor.py:663-665` | `motivo` falta em ramos `:339-342,363-367`; consumidor aceita None | Sem selo produtor real localizado; testes fio substituem propostas, `app/chamados/tests/test_proposta_no_fio.py:131-146` | ESTRUTURA provável; classe sim, opcional |
| P08 P | mesmos escritores P | `app/core/views_ia.py:575-582,598-636,664`: `propostas→[]`, `colaborador_id→None`; `.get(tipo,entrou_por,dias_discordantes,vinculo_id,ancora,ancora_atual,template_atual,template,aplicar_desde,casa_hoje,casaria)`, `dias_discordantes→0`, `chamados_que_morreriam→[]`; assinatura por autoridade `:578` | Chaves lidas emitidas; ramos respeitados | Sem selo produtor real localizado; `app/core/tests/test_ia_escala_com_sim.py:59,104-145` fabrica envelope | ESTRUTURA provável; classe não demonstrada |
| P09 P | mesmos escritores P | `app/api/views_mensageria.py:2034-2049`: `propostas→[]`, `.get(colaborador,posto,ancora,template_atual,template,casa_hoje,casaria,aplicar_desde,tipo)`, `chamados_que_morreriam→[]`, `n→0` | Chaves lidas emitidas | Sem selo produtor real localizado | ESTRUTURA provável; classe não demonstrada |
| P10 linhas da pauta — transporte | `services/propositor.py:644-650`: `chave,n,frase,url` | `services/fase_12x36.py:267-280`: concatena lista e passa `linhas` a `escrever_semanal`; não lê chaves internas | Nenhum desencontro demonstrado; é transporte, excluir da contagem se “leitor” exigir acesso a chave | Sem selo específico localizado | ESTRUTURA provável; classe não |
| P11 linhas da pauta | mesmo emissor `propositor.py:644-650` | `app/pautas/semanal.py:146,163-168`: `alavanca→n→0`, `frase→''`, `chave,n` no fallback, `url` obrigatório | Produtor não grava `alavanca`; usa-se `n`, apesar de frase mencionar chamadas que morreriam (`propositor.py:643-648`) | Sem selo do par localizado | ESTRUTURA provável; classe sim, opcional |
| P12 opções da porta retroativa | `services/propositor.py:765-771`: `colab,vinculo,template,desde,ate,ancora,novo_trecho,regenerar_desde,apesar_da_lavra,apply,schema` | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:63-66,81-86,109-110,158,172,192-194,225,233`: todos salvo `schema`; opcionais `.get` sem default ou `apesar_da_lavra→''` | `schema` gravado mas `_rodar` não lê; só `handle` lê em `:48-51`, deliberadamente evitado pelo emissor (`propositor.py:760-764`) | **sim parcial**, `tests/test_porta_da_proposta.py:148-166` chama aplicar real e confirma trilha/decisão; não prende especificamente `schema` | ESTRUTURA provável; classe sim, chave transportada sem leitura nessa entrada |
| P13 resultado aplicar | `services/propositor.py:788`: `decisao_id,saida` | `management/commands/propostas_de_escala.py:68-72`: `saida,decisao_id` | Sem desencontro de chave | Sem selo real CLI→produtor localizado | ESTRUTURA provável; classe não |
| **P14 resultado aplicar** | **`services/propositor.py:788`: apenas `decisao_id,saida`** | **`app/chamados/services/proposta_no_fio.py:151-153`: `r.get('ok',True)` e `(r or {}).get('msg')`** | **`ok` e `msg` nunca emitidos por aplicar. `ok` ausente vira sucesso True.** Mapeamento não adapta resultado: `app/core/intake.py:49-52,481-487` resolve diretamente `escala.services.propositor.aplicar` | **Sem selo dessas chaves localizado**; mocks de fio em `app/chamados/tests/test_proposta_no_fio.py:131-146` não atravessam produtor real | **ESTRUTURA provável; classe sim** |
| P15 D | `detector_proposta.py:63-92,123-156`, D | `proposta_pdf.py:75-80,121-138`: `proposta,confianca,padrao.dias,marcos_moda,entrada,saida,duracao_h,dispersao_entrada_min,padrao_dias,dows`; `proposta.codigo,horario,ciclo`; `porque` | Todas chaves lidas são emitidas em seus ramos; `marcos_freq` não é lido pelo PDF, mas é lido pelo próprio detector `:133-149` | **sim**, `tests/test_detector_proposta.py:141-165,168-181` monta resultados pelo produtor real e gera PDF | ESTRUTURA provável; classe não |
| P16 D | mesmos escritores D | `management/commands/propor_escala_limbo.py:37-57`: `confianca,proposta,padrao` com as mesmas oito chaves de padrão do PDF; proposta `codigo,horario,ciclo`; `porque` | Todas chaves lidas emitidas nos ramos respectivos | Sem selo real CLI localizado | ESTRUTURA provável; classe não |
| P17 D | mesmos escritores D | `services/escala_auto.py:120-125`: `proposta→None,confianca→None,padrao.dias,proposta.tipo_escala_id,codigo,descricao,horario,porque` | Chaves emitidas; ramo `proposta` guarda acesso à proposta e ao mínimo de dados | Sem selo do par real localizado | ESTRUTURA provável; classe não |

### Render e resposta JSON: 13 pares identificados

| ID / estrutura | Emissor / gravadas | Consumidor / lidas | DESENCONTRO | Selo que prende | Dono provável / classe |
|---|---|---|---|---|---|
| P18 contexto minha escala | `views.py:15,23` grava só `sem_vinculo`; ramo normal `:55-70` grava `escala,dias,hoje,ancora_ausente,titulo_pagina`; dias `{data,trabalho,hora_inicio,hora_fim,status_rotulo}` | `app/templates/escala/minha_escala.html:10,24-49,62-84`: `sem_vinculo,escala,ancora_ausente,dias,hoje` e todas chaves dos dias. `base_tpl|default:"base.html"` em `:1` pode vir de contexto externo | Ramo normal não grava `sem_vinculo`, consumido como condição falsa. `titulo_pagina` sem leitura direta localizada. Bases/includes globais não fechados `[nao verificado]` | **sim**, `tests/test_e4_6_minha_escala_e_previsao.py:50-73` atravessa view/template e confirma rótulo HTML | ESTRUTURA provável; classe sim, ausência opcional |
| P19 contexto tipos | `views.py:114-115`: `tipos,uso,quadros_barra`; uso mapa `{tipo_id:n}` em `:85-90` | `app/templates/escala/tipos_lista.html:21,42,89-91`: as três chaves; `uso|get_item` por pk | Sem desencontro nominal demonstrado | **sim parcial**, `tests/test_o122_etapa1_barra_em_tipos.py:56-82` atravessa rota/render e confirma quadro | ESTRUTURA provável; classe não |
| P20 quadros barra | `views.py:105-112`: quadros `{titulo,itens}`; item `{atalho,rotulo,icone,href,onclick,attrs,title}` | `app/templates/core/_barra_gestao.html:65,67` via `tipos_lista.html:21`: lê `titulo,title,itens`; item `atalho,botao,attrs,onclick,ativo,title,icone,rotulo,href|default:'javascript:void(0)'`; ramo não atalho também `classe,cor,negrito_escuro,numero` | `q.title,it.botao,it.ativo` faltam neste emissor; opcionais. Ramo não atalho é inalcançável para o item emitido | **sim**, `tests/test_o122_etapa1_barra_em_tipos.py:56-82` prende título, ícone, href e attrs htmx | ESTRUTURA provável; classe sim, opcionais |
| P21 contexto opções busca | `views.py:229`: `escalas` | `app/templates/colaboradores/partials/_opcoes_escala.html:1-7`: `escalas` | Sem desencontro de chave | Teste específico real busca→partial não lido; `[nao verificado]` | ESTRUTURA provável; classe não |
| P22 contexto lista calendários | `views.py:294-310`: `itens[].{cal,tamanhos,semana_dia_rotulo,colabs,n_colabs}`, `tamanhos[].{tamanho,dias,canonico}` | `app/templates/escala/folga_calendarios_lista.html:12-38`: todos campos item; tamanhos lê só `canonico` (`:21-22`) | `tamanhos.tamanho,dias` gravados sem leitura neste consumidor; sem outro leitor desse dict localizado `[nao verificado]` | `tests/test_editor_folga_modo.py:66-71` verifica contexto, não HTML; **sem selo das chaves órfãs** | ESTRUTURA provável; classe sim, órfãs no par |
| P23 contexto editor calendário | `views.py:363-371`: `cal,tamanhos,tipos,modo,celulas`; tamanho `{tamanho,rotulo,dias_range,marcados}` | `app/templates/escala/folga_calendario_editar.html:6-21,67-68`: `cal,modo,celulas,tamanhos.{tamanho,rotulo,marcados}` | `tipos` e `tamanhos.dias_range` sem leitura no template; sem outro leitor desse dict localizado `[nao verificado]` | `tests/test_editor_folga_modo.py:58-64` verifica contexto, não uso HTML; **sem selo das órfãs** | ESTRUTURA provável; classe sim, órfãs |
| P24 wrapper cadastro realidade | `views.py:492`: `{dados}`; conteúdo vem de `services/cadastro_realidade.lista`, coberto pelo agente contextos | `app/templates/escala/cadastro_x_realidade.html:8-29`: `dados` e chaves internas conforme serviço | Wrapper em si confere; não duplicar contrato interno de `lista` | Selo não conferido aqui `[nao verificado]` | ESTRUTURA provável; classe não demonstrada |
| P25 contexto plano | `views_plano_folgas.py:38-47`: `empresas,mes,ano,meses,anos,empresa_id,posto_id`, condicional `postos,linhas,cab`; `:72-93` linhas `{ec,cels,sem_previsao,escala_nome}`, cels `{dia,tipo,proj}`, cab `{dia,dow,domingo}` | `app/templates/escala/plano_folgas.html:14-90,173,192-193`: todas chaves descritas; `cab|length|default:0` em `:193` | `postos,linhas,cab` omitidos sem empresa; consumidor consulta dentro de guardas/iterações vazias/default | Testes locais de `_grid_ctx` em `tests/test_plano_folgas_filtro.py:53-64`; **selo HTML dessas chaves não localizado** | ESTRUTURA provável; classe sim, omissões opcionais |
| P26 contexto wizard | `views_wizard.py:75-91`: `tipo,criar,ciclos,bases,hpd_json,folgas_atuais,pai,folga_calendarios,perso_valor,drawer,base_tpl` | `app/templates/escala/wizard_tipo_escala.html:1-37,146,225-234`: todos salvo `folga_calendarios`, lido em include P27 | Sem desencontro no conjunto template+include | **sim parcial**, `tests/test_wizard_drawer.py:25-34` atravessa `drawer/base_tpl` e verifica HTML; calendários em `tests/test_wizard_folga_padrao.py:83-89` verifica contexto | ESTRUTURA provável; classe não |
| P27 contexto wizard herdado pelo select | mesmo emissor P26; `selecionado=tipo.folga_calendario_padrao_id` acrescentado pelo include em `wizard_tipo_escala.html:146` | `app/templates/escala/_wizard_cal_select.html:4-5`: `folga_calendarios,selecionado` | `selecionado` nasce no template intermediário, não na view; cadeia completa confere | Sem selo que prenda seleção real localizado | ESTRUTURA provável; classe não |
| P28 contexto select calendário criado | `views_wizard.py:175-178,196-199`: `folga_calendarios,selecionado` | `app/templates/escala/_wizard_cal_select.html:4-5`: ambas | Confere | **sim parcial**, `tests/test_wizard_folga_padrao.py:41-54` cria pela rota e confirma nome do calendário no fragmento; seleção por pk não assertada | ESTRUTURA provável; classe não |
| P29 contexto preview | `views_wizard.py:263-265`: `tipo,folgas_rotulos,eh_perso,perso_dias` | `app/templates/escala/_wizard_preview.html:4-12`: as quatro | Confere | **sim**, `tests/test_wizard_views.py:140-152` atravessa POST→preview→HTML e confirma apelido/HE | ESTRUTURA provável; classe não |
| P30 JSON aplicação lote plano | `views_plano_folgas.py:158,171-190`: erro inicial `{erro}` com 400; sucesso `{ok,gravadas,erros,celulas_relavradas,barrados}`; erros itens `{ec,dia,erro}` | JS `app/templates/escala/plano_folgas.html:174-185`: lê `ok`, `erro||'erro'` se falsy; `erros||[]`, erro item `ec,dia,erro` | Resposta de erro inicial não tem `ok`, JS usa ausência como false. `gravadas,celulas_relavradas,barrados` não são lidas por esse JS; leitura por outros consumidores não localizada `[nao verificado]` | Teste `tests/test_bug136_plano_em_lote_relavra.py:75-81` verifica JSON de erro na API, sem executar o JS; **selo da costura com JS não localizado** | ESTRUTURA provável; classe sim, omissão opcional e campos sem leitor no par |

### Pontos a incorporar na consolidação

- **30 linhas identificadas**, das quais **P10 é mero transporte de lista**; se o critério exigir acesso a chave interna, são **29 costuras** deste recorte. P24 é wrapper e deve ser distinguido da costura de conteúdo `cadastro_realidade.lista→template` do outro agente.
- **Desencontro mais forte: P14**, comprovado com produtor, consumidor e roteamento dinâmico, sem hipótese sobre banco.
- Proposta tem possíveis chaves órfãs `razao_fora_dentro` (`propositor.py:547`), `dias_previstos` (`:559`) e `chamados_que_morreriam[].dia` (`:634-635`). Nenhum leitor nominal do objeto proposta foi localizado; orfandade global permanece `[nao verificado]`.
- **Assimetria de schema em P:** `previsto_novo` é retirado no retorno normal (`propositor.py:577`), mas fica no retorno de existência (`:571-574`). O consumidor atual do ramo de existência lê apenas `colaborador_id` (`app/chamados/services/painel.py:522-524`); dano não demonstrado.
- Há respostas JSON de `views.py:127-133,245-250,266-279,422` e `views_plano_folgas.py:138-139` cujo consumidor não foi localizado neste recorte `[nao verificado]`. Não inventei costura sem leitor.
- `services/agenda.py:11-45` recebe parâmetros escalares/modelos/lista de IDs e grava campos ORM; não identifiquei dict customizado emitido por ela e lido por outro módulo. Não contei kwargs internos de `objects.create/update` como contrato customizado.
- Todos os donos acima são **prováveis pelo protocolo de código**, não aplicação do juiz L-099 por dia de produção `[nao verificado]`.

## 13/11 15:43 · sessao f90fd80293fd · - · - · origem=Ronald (tmux)

Inventário estático de `ChamadoColaborador.contexto_json`, por leitura de referências nominais, AST em memória e seguimento dos aliases e portas. Campo persistido: `app/chamados/models.py:47`. As portas aceitam chaves arbitrárias; portanto reflexão, chamadas dinâmicas, dados históricos e execução real permanecem [nao verificado] (`app/chamados/services/contexto.py:62`, `:82`, `:131`). Não executei testes nem escrevi arquivos.

**Escritores internos encontrados, com shapes**

`app/chamados/services/contexto.py:76`, `:102`, `:125`, `:161` são os saves efetivos de anotar/renovar/retirar/fundir; os emissores semânticos encontrados são:

| Emissor | Chaves gravadas |
|---|---|
| `services/abertura.py:58` | `sistema,nome,empresa,posto,pendente_glpi=True` |
| `services/abertura.py:116` | `pendente_glpi=False` |
| `services/abertura.py:183` | `sistema,nome,empresa,posto,aberto_por_user_id,aberto_por,pendente_glpi=False`, mais chave dinâmica de data em `:191`; resolve por `catalogo/modulos.py:315` |
| `services/adesao.py:79` | `adesao=True,data_inicio,periodo=[ini,fim]` |
| `services/adesao.py:113` | `agrupado_em` |
| `reconciliador.py:691`, `:708`, `:709`, persistido em `:921` | `data_turno,origem='reconciliador_c2'`, condicionalmente `marcos_faltantes,_marco_por='lampada_da_celula'`; marcos têm `{hora,tipo,motivo:'lampada_apagada'}` por `app/ponto/services/lastro.py:134` |
| `services/disputa_emissao.py:580`, reuso em `:586` | `contexto_nascimento` arbitrário; wrapper `:630` passa `{origem}`, wrapper `:666` repassa `contexto or {}` |
| `services/disputa_emissao.py:1116`, reuso `:1122` | `chamados_orfaos_ids` |
| `services/disputa_emissao.py:1351` | `origem='ancora_limbo',chamado_limbo_id` |
| `services/disputa_emissao.py:1357` | `chamados_limbo_ids`, em reuso |
| `services/limbo_folgas.py:68` | `sugestao={classe,dias,ndias,semanas}` |
| `services/limbo_folgas.py:276` | `agrupado_em,defeito_cadastro='folga_nao_definida'`; guarda só por `agrupado_em` |
| `services/declaracao_texto.py:208` | `escape_colab,escape_colab_em`; guarda só por `escape_colab` |
| `management/commands/nomear_marco_por_lampada.py:109` | `marcos_faltantes,_marco_por='reconstruido_pela_lampada'` |
| `management/commands/carimbar_por_batida_coincidente.py:98` | `batida_id,data_turno,_dia_por` |
| `management/commands/smoke_chamados.py:97`, via porta em `:102` | `{motivo:{data}}` quando pergunta do catálogo pede data; caso contrário `{}`; também nasce no contexto do chamado pela porta |

A ponte adicional de fusão está em `services/nota.py:110` → `:123` → `services/contexto.py:131`: `contexto_novo` é arbitrário, listas unem, escalares existentes ficam (`services/contexto.py:146`).

**Leitores nominais e shapes/defaults**

Caminhos abaixo têm prefixo `app/chamados/`, salvo os explicitamente iniciados por `app/`.

| Leitor | Chaves lidas |
|---|---|
| `catalogo/modulos.py:225` | `ctx.get(m['chave_data'])`, default `None`; chave vem de `MOTIVOS`, `:105-198` |
| `juizes.py:114`, `:118`, `:129-130` | chave catalogada; fallback `data_turno=None`; `batida_id=None`, seguido de acesso direto |
| `juizes.py:159-162` | `ultima_batida=None → {}`, `ultima_batida.timestamp=None`; fallback `entrada_timestamp=None` |
| `juizes.py:187`, `:197`, `:226` | lote: `data_turno=None`, `batida_id=None`; depois contexto fraco anterior; **não consulta chave catalogada** |
| `juizes.py:549-551`, `:576`, `:579` | `marcos_faltantes=None → []`; por marco `motivo=None,hora=None,tipo=None`; motivo nomeado impede fallback por tipo |
| `juizes.py:1723`, `:1747` | `agrupado_em=None` |
| `juizes.py:2163` | `entrada_timestamp=None` |
| `juizes.py:2331` | `ausencia_id=None` |
| `models.py:816` | `agrupado_em=None` |
| `models.py:924` | `defaults.contexto_json=None → {}`; `marcos_faltantes=None` |
| `models.py:931` | ORM `contexto_json__has_key='agrupado_em'` |
| `models.py:955-958` | `defaults.contexto_json`; chaves dinâmicas extraídas de `contexto_json__*` |
| `models.py:1068`, `:1235-1236` | contexto inteiro para `_celula_de`; `_modulo_origem=None → ''`; delega dia ao juiz |
| `models.py:1435` | `entrada_timestamp=None` |
| `reconciliador.py:146` | `entrada_timestamp=None` |
| `reconciliador.py:198` | `par=None`, espera lista/tupla de horários |
| `reconciliador.py:243` | `chamados_orfaos_ids=None`; ausência retorna `None`, não `False` |
| `reconciliador.py:836`, `:1589` | filtro ORM `data_turno` |
| `data_evento.py:27-39` | ordem `data_turno=None,data_batida=None,entrada_timestamp=None,pendentes=None,horario_previsto=None`; datas ISO ou texto BR para pendentes |
| `services/disputa_emissao.py:57-59` | todas as chaves por `.items()` e `setdefault`; usa `entrada_sem_saida=None → {}` |
| `services/disputa_emissao.py:68-73` | `entrada_sem_saida.entrada_id=None`, `.entrada_aberta_em=None`, `ultima_batida=None → {}`, `.id=None` |
| `services/disputa_emissao.py:145` | `entrada_timestamp=None` |
| `services/disputa_emissao.py:733`, `:739` | delega marcos a `_marcos_cobrados`; data catalogada |
| `services/disputa_emissao.py:930` | data catalogada |
| `services/disputa_emissao.py:1074`, `:1232` | delega contexto a `data_evento_do_contexto` |
| `services/disputa_emissao.py:1143` | `marcos_faltantes=None → []`; subchaves dos marcos em `:1146-1151` |
| `services/disputa_emissao.py:1170` | data catalogada |
| `services/acoes_chamado.py:30-32` | `tipo_batida='E',hora_tentativa='',motivo_anuencia=''` |
| `services/acoes_chamado.py:57`, `:61` | `foto_b64=None,foto_nome='selfie.jpg'` |
| `services/acoes_chamado.py:96`, `:403` | `agenda_dia_ids=[]` |
| `services/acoes_chamado.py:144-146` | presença de `snapshot_ecs`; `.get(None) → []` |
| `services/acoes_chamado.py:185`, `:191`, `:195-210` | `distancia_metros,alerta_geofence_id,tipo_batida,posto_nome,raio_metros,posto_lat,posto_lon,batida_lat,batida_lon`, todos `.get` sem default; texto usa `or` para vazios |
| `services/acoes_chamado.py:247` | `batida_id=None,ocorrencias=None → []`; em ocorrência `batida_id=None` |
| `services/acoes_chamado.py:267`, `:275-276` | `batida_id=None`, `distancia_metros=None → '?'`, `posto_nome=None → '-'`, `raio_metros=None → '-'` |
| `services/acoes_chamado.py:371` | `hora_tentativa=None → None` |
| `services/limbo_folgas.py:64`, `:84`, `:101` | `tipo_escala_id=None`, também filtro ORM |
| `services/limbo_folgas.py:82-85` | `sugestao=None → {}`, `.dias=None → []`, `.classe=None` |
| `services/limbo_folgas.py:193` | filtro ORM `agrupado_em` |
| `services/limbo_folgas.py:253` | `agrupado_em=None`, guarda |
| `services/adesao.py:111` | `agrupado_em=None`, guarda |
| `services/declaracao_texto.py:205` | `escape_colab=None`, guarda |
| `services/abertura.py:168` | filtro ORM `aberto_por_user_id` |
| `services/contexto.py:70-71`, `:97-100`, `:118-123`, `:144-156` | todas as chaves dinâmicas do ato: `.get(k)=None`, pertencimento e listas; ler e escrever no mesmo módulo |
| `services/feriado_reavaliar.py:78` | filtro ORM `data_turno__in` |
| `services/pergunta_no_app.py:114` | data catalogada |
| `services/cobranca_folha.py:32` | data catalogada |
| `services/materializacao.py:135` | contexto delegado a `data_evento_do_contexto` |
| `services/celula_carimbada.py:45`, `:149` | carrega campo completo e delega dia ao juiz |
| `rejuncao.py:83` | data catalogada |
| `views.py:441` | `tipo_escala_id=None` |
| `views.py:1512` | `ausencia_id=None` |
| `signals.py:361` | ORM `chamados_orfaos_ids__contains` |
| `management/commands/nomear_marco_por_lampada.py:50`, `:65`, `:70` | `marcos_faltantes=None → []`, `horario_previsto=None`, `origem=None` |
| `management/commands/auditar_invariantes_chamados.py:73`, `:93`, `:104`, `:617`, `:686`, `:1016` | ORM data_turno; `entrada_timestamp=None`; chave catalogada por `MOTIVOS[m]['chave_data']` |
| `app/templates/chamados/partials/painel_chamado_turno.html:35-36` | **todas as chaves e valores por `.items`** |
| `app/templates/chamados/partials/modal_fio.html:481-488` | coordenadas batida/posto, distância, raio; raio default `200`, distância `0`, posto_nome/endereço `''` |
| `app/templates/chamados/partials/modal_fio.html:529` | `batida_id`, default `''` |
| `app/templates/chamados/partials/modal_fio.html:544` | `sugestao.classe` |
| `app/templates/chamados/partials/modal_fio.html:561-578` | `classe,evidencia,destino.codigo,destino.n_colabs` |
| `app/templates/chamados/partials/modal_fio.html:588-594` | `motivo_anuencia,tipo_batida,hora_tentativa` |
| `app/templates/chamados/partials/_relatorio_geo.html:18-33` | tipo_batida default `'-'`; tipo_alerta; distância; raio default `'-'`; posto_nome default `'-'`; endereço e coordenadas |
| `app/templates/chamados/detalhe_local.html:227`, `:245-253` | `tem_documento,motivo_anuencia,tipo_batida,hora_tentativa`, últimos dois default `'—'` |
| `app/templates/chamados/partials/_card_atendimento.html:65-70`, `:232-235` | `tem_documento,ausencia_id,data_turno` |

**Leitores externos efetivamente encontrados**

Estes são consumidores do mesmo JSON persistido e entram conforme a fronteira do usuário:

- `app/escala/utils.py:1062`, `:1066-1070`: `agrupado_em=None`; `marcos_faltantes=None → []`; marcos `tipo=None,hora=None`.
- `app/colaboradores/services/calendario.py:438`, `:448-451`: ORM `data_turno` não nulo, presença de `agrupado_em`; `.get('agrupado_em')=None`.
- `app/colaboradores/services/pos_vinculo.py:38-41`: `periodo=None → []`, só restringe datas se tamanho 2.
- `app/escala/services/regeneracao.py:149`, `:196`: `marcos_faltantes=None`; por marco `hora=None`.
- `app/ponto/services/lastro.py:75-78`: `marcos_faltantes=None → []`; `hora=None`, `tipo=None → None`. Entradas pelos consumidores de `:110`, `:170`, `:273`, `:346`.
- `app/ponto/services/entrada_sincronizada.py:51`, `:55`: ORM `chamados_orfaos_ids__contains`; `.get(None)` comparado com lista unitária.
- `app/ponto/services/cartorio.py:136`, `app/ponto/services/cobertura.py:230`, `app/ponto/management/commands/supra_juiz.py:200`, `app/ponto/management/commands/mapa_divergencia.py:293`: data catalogada.
- `app/ponto/worklist.py:54-55`: `pendentes=None`.
- `app/ponto/management/commands/reconciliar_grade.py:376-383`: `orfao_14h=None`; nested `data=None,entradas_pendentes=None → []`, entradas `data=None`; `pendentes=None`.
- `app/ponto/management/commands/reconciliar_grade.py:445-449`, `:465-467`: `orfa_regeneracao=None`, `marcos_faltantes=None → []`, marcos `hora=None`.
- `app/ponto/management/commands/escalonar_chamados_supervisao.py:63`, `:112`, `:147`, `:172`, `:188`: `orfao_detectado,escalado_n2_1h,escalado_n3_4h,escalado_n4_comercial,escalado_n5_24h`, todos `.get(None)`.
- `app/ponto/management/commands/reconciliar_fantasmas.py:183-184`: `pendentes=None`.
- `app/ponto/management/commands/curar_residuo_bug115.py:76`, `:83`, `:86`, `:121`: `marcos_faltantes=None → []`, `horario_previsto=None`, `origem=None`, `marcos_candidatos=None`.
- `app/ponto/management/commands/gerar_celulas.py:259-260`: `marcos_faltantes=None → []`; marcos `hora=None`.
- `app/ponto/management/commands/detectar_ausencias.py:557`: `marcos_faltantes=None`.
- `app/ponto/services/faltas_de_hoje.py:64`, `:85`; `app/ponto/services/furos_sem_cobranca.py:69`, `:130`; `app/ponto/services/vigia_de_hora.py:112-113`; `app/ponto/services/veredito_celula.py:150`; `app/ponto/management/commands/flip_automatico.py:76`; `app/ponto/management/commands/processar_alertas_turno.py:166`, `:181`, `:338`: filtros/projeções ORM por `data_turno`.
- `app/api/views.py:1045`, `:3114`: `ausencia_id=None`, filtro ORM pelo mesmo ID; `:1956-1957` filtro data_turno.
- `app/ponto/services/ausencia.py:662`, `:712`, `:715`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:32`: `ausencia_id`, `.get(None)`.
- `app/ponto/services/justificativa.py:57-58`: filtro data_turno.
- `app/escala/regua_defesa.py:54`, `:328`, `:334`: filtro tipo_escala_id; `destino=None → {}`, nested `codigo=None`; `tipo_escala_codigo=None`.
- `app/escala/management/commands/fechar_avisos_regua_na_lista.py:42`: `classe=None`.
- `app/ponto/services/geofence.py:403-406`, chamado em `:425`: `ocorrencias=None`, ocorrências `batida_id=None,batida_lat=None,batida_lon=None`; fallback das três chaves no topo.

Leitores da **resposta derivada** de `dias_do_fato`, além do próprio JSON: `app/chamados/management/commands/arquivar_competencia_encerrada.py:102`, `services/juizes_do_dia.py:94`, `services/fila_por_causa.py:162`, `:319`, `:367`, e `management/commands/nomear_marco_por_lampada.py:51`; os consumidores externos do juiz são chamados em `app/colaboradores/management/commands/marcar_feriado_do_posto.py:106`, `corrigir_escala_retroativa.py:142`, `app/ponto/management/commands/curar_residuo_bug115.py:77`, `app/escala/services/propositor.py:633`.

**Escritores externos da mesma estrutura**

Para completar TODOS os emissores do JSON, estes também foram encontrados; avaliar na consolidação se unidade trata nascimento delegado à porta interna como emissão aqui:

- `app/colaboradores/views.py:2376`: `agenda_dia_ids`.
- `app/colaboradores/services/vinculo.py:419`: `snapshot_ecs,ec_nova,template_id,template_nome,data_inicio`.
- `app/colaboradores/services/desligamento.py:182`: `data_demissao,motivo_informado,executado_por_username,executado_por_id,executado_em`.
- `app/ponto/utils.py:120`: `agendamento_pk,data_inicio,data_fim`.
- `app/ponto/views.py:280`, persistência `:288` ou `:301`: `pendentes,total`.
- `app/escala/regua_defesa.py:72`: `tipo_escala_id,tipo_escala_codigo,causa,data_exemplo`.
- `app/api/views.py:714-726`, gêmeo `app/api/views_core.py:708-720`: dict do detector mais `batida_id,data_turno`.
- `app/api/views.py:1978`: `data_turno`; `:3120`: `tem_documento=True`.
- `app/ponto/services/justificativa.py:301`: `data_turno,tipo,motivo_rejeicao,rejeitado_por`.
- `app/ponto/services/cluster_espurio.py:211`: `data_turno,orfas,n_missing`.
- `app/ponto/services/geofence.py:375`: `ocorrencias,alerta_geofence_id,batida_id,tipo_alerta,distancia_metros,raio_metros,posto_nome,posto_endereco,posto_lat,posto_lon,batida_lat,batida_lon,tipo_batida`; agregação de ocorrências via nota em `:359`.
- `app/ponto/services/ausencia.py:672`: `ausencia_id,data,tipo,tem_documento`.
- `app/ponto/services/triagem_batida.py:117`: `motivo,situacao,posto`; `:347`: `tipo_batida,hora_tentativa`; `:353-371` renovação `motivo_anuencia,tipo_batida,hora_tentativa`, opcional `foto_b64,foto_nome`; `:470`: `tipo_batida,hora_tentativa,hora_prevista,diferenca_minutos,escala`.
- `app/ponto/management/commands/detectar_par_relampago.py:383`: `data_turno,par,duracao_min`.
- `app/ponto/management/commands/processar_alertas_avancados.py:126`: `data_turno,entrada_timestamp`.
- `app/ponto/management/commands/reconciliar_grade.py:516`: `data_turno,origem,marcos_faltantes[{tipo,hora,motivo}]`; `:473`: `marcos_faltantes,orfa_regeneracao_curada`; retira `orfa_regeneracao` em `:454`, `:474`.
- `app/ponto/management/commands/detectar_ausencias.py:224`: `escala_id,tipo_escala_id,tipo_escala_codigo,tipo_ciclo,colabs_mesmo_template,causa,data_inicio`; `:294`: `escala_id,tipo_ciclo,data_inicio`; `:532`: `data_inicio,causa,escala_id`; `:538`: soma `dias_sem_grade`; `:559`: `marcos_faltantes,_marco_por`; `:706`: `data_turno,horario_previsto,atraso_min`, mais `marcos_faltantes` pelo argumento ou helper; `:766`: `folga_nao_declarada` e companheiras a conferir [nao verificado].
- `app/ponto/management/commands/processar_alertas_turno.py:348`: `data_turno,entrada_timestamp,posto`.
- `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:156`: `_ctx`, shape não concluído [nao verificado].
- `app/ponto/management/commands/gerar_celulas.py:264`: `orfa_regeneracao={em,marcos_na_emissao}`.
- `app/ponto/management/commands/escalonar_chamados_supervisao.py:64`, `:138`, `:163`, `:179`, `:195`: carimbos de escalada listados acima.
- `app/ponto/management/commands/curar_residuo_bug115.py:124`: `marcos_candidatos,_marco_por`, demais companheiras [nao verificado].

**Desencontros literais comprovados e selos**

1. `abertura.py:183-191` → `juizes.py:187-197`: emissor grava chave catalogada (`data` para ausência em `catalogo/modulos.py:120`; `data_inicio` aparece também em `adesao.py:86`), mas o leitor em lote só lê `data_turno` antes de procurar batida/disputa/carimbo. O singular lê catálogo em `juizes.py:114`, e `_dia_do_contexto_fraco` só lê timestamps em `:159-162`. A falta das outras âncoras pode produzir singular com data e lote sem data pela leitura estática. ESTRUTURA provável; classe sim. Selo específico da paridade de **chaves** não localizado [nao verificado]. `tests/test_e1_v9.py:357-368` cobre conversa versus fato, não essas chaves.

2. `abertura.py:183-191` → `views.py:1512`: abertura humana de `ausencia_colab` grava `data`, mas não `ausencia_id`; leitor pede `ausencia_id=None`. ESTRUTURA provável; classe sim; impacto de produção [nao verificado]. `tests/test_form_catalogo.py:76-83` sela módulo e data, sem selar ID.

3. Mesmo emissor → `juizes.py:2331`: pede `ausencia_id` ausente nessa forma humana. ESTRUTURA provável; classe sim; selo específico não localizado [nao verificado].

4. Mesmo emissor → `app/templates/chamados/partials/_card_atendimento.html:65-70`: pede `tem_documento` e usa `ausencia_id` no endpoint; ambas ausentes na forma humana. ESTRUTURA provável; classe sim; selo específico não localizado [nao verificado]. O emissor externo especializado **grava ambas** em `app/ponto/services/ausencia.py:672`: distinguir formas, sem assumir prejuízo observado.

5. `reconciliador.py:708` → `juizes.py:551`, `:576-577`: `marcos_faltantes[].motivo` vale `'lampada_apagada'` no emissor (`app/ponto/services/lastro.py:134`), mas o leitor compara com o motivo da Pergunta e recusa fallback se houver qualquer motivo nomeado. Mesmo nome para etiqueta da lâmpada e identidade da pergunta. ESTRUTURA provável; classe sim. Existem selos separados do escritor e leitor, sem selo encontrado do par com esse shape: escritor `tests/test_emissor_nomeia_o_marco.py:54-74`; leitor `tests/test_selo_juizes_orfaos.py:84-123`, `tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:33`. Cobertura específica da costura [nao verificado].

6. `nomear_marco_por_lampada.py:109` → `juizes.py:551` repete o shape com `'lampada_apagada'`, confirmado por `tests/test_emissor_nomeia_o_marco.py:159-162`. Mesma classe anterior; selo do par [nao verificado].

**Selos positivos utilizáveis**

- `services/limbo_folgas.py:68` → template `modal_fio.html:544`: `tests/test_limbo_folgas.py:97` renderiza o botão, com integração de sugestão/template em `:47-64`; conferir assertions completas para marcar par exato.
- `services/adesao.py:113` → `models.py:816`: `tests/test_agrupado_em.py:32`, `:42` prende bloqueio por pai vivo e liberação após fechamento.
- `services/limbo_folgas.py:276` → reata/fecho/agrupamento: `tests/test_t34_furo_sob_limbo.py:37`.
- `services/disputa_emissao.py:1116/:1122` → fusão: `tests/test_3b_reuso_899.py:31`; consumidor semântico `_filhos_vivos` não necessariamente selado por esse teste [nao verificado].
- `services/disputa_emissao.py:1351/:1357` → fusão: `tests/test_3b_reuso_1061.py:17`; não demonstra leitor semântico dos IDs.
- `services/contexto.py:62/:82/:131` regras de escrita: `tests/test_porta_contexto.py:92-116`, `:132-156`, `:220-257`, `:273-279`.
- `retirar_contexto` e renovação de órfãs: `tests/test_porta_chamado_e2.py:39-69`.
- fusão por nota: `tests/test_anexar_ocorrencia.py:22-48`.
- semântica de `entrada_timestamp`: `tests/test_cq_c8.py:14`.
- nascimento/junção pelo contexto: `tests/test_f1b_celula.py:17-48`, `tests/test_emissor_carimba_celula.py:68-106`.

**Atenção para consolidar contagem**

`app/templates/chamados/partials/painel_chamado_turno.html:35-36` é leitor válido de **todo** o JSON. Portanto, `escape_colab_em`, `defeito_cadastro`, `chamados_limbo_ids`, metadados e quaisquer outras chaves não podem ser declaradas “gravadas que ninguém lê” globalmente: o template as lê. Pode-se registrar ausência de leitor semântico específico como [nao verificad

[truncado em 20000 de 20392 caracteres]

_4667 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
