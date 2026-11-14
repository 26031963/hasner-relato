# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 15:00** · publicado em **14/11/2026 15:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 15:00 · sessao 4e2677a2198a · - · - · origem=Ronald (tmux)

Sugestões para os testes ORM, sem substituir os escritores: [nao verificado]

| Chamador | Menor caminho |
|---|---|
| Gate de encerramento | Chamar `I._gate_pergunta_viva('fechado', registrar=True, plano=([p], [(p, G)]))`; a recusa da porta levanta `ValueError`. `chamados/models.py:421`, `chamados/models.py:437` |
| Reconciliador direto | Substituir somente `_derivar_chamado_destino` por `(G, D)` e chamar `reatar_pergunta_orfa(p, colab)`. `chamados/reconciliador.py:1557`, `chamados/reconciliador.py:1655` |
| Reconciliador histórico | Derivador retorna `(None, None)`; criar histórico terminal único, fora do alcance novo. Usar motivo não intervalo como contraexemplo. `chamados/reconciliador.py:1594`, `chamados/reconciliador.py:1642` |
| Reemissão c2 | G vivo com `modulo_origem='batida_ausente'` e `data_turno=D` evita gates de nascimento. A porta `abrir` e o reaponte permanecem reais. `chamados/reconciliador.py:838`, `chamados/reconciliador.py:954` |
| Reata ao LIMBO | `reatar_perguntas_ao_limbo([p.pk], G)` consulta e escreve pelas portas reais. `chamados/services/limbo_folgas.py:239`, `chamados/services/limbo_folgas.py:244` |
| Agrupamento de furo | `agrupar_furo(I, G)`; para isolar o chamador, substituir apenas `_vivas_ancoradas` por `[p]` e `reconciliar` por `False`. `chamados/services/limbo_folgas.py:279`, `chamados/services/limbo_folgas.py:282` |
| Migração do fio | `migrar_resolvedoras_vivas(I, G)`, com P única viva em I e G sem perguntas; conferir `recusadas`. `chamados/services/fio.py:2130`, `chamados/services/fio.py:2174` |
| Reversão do beco | Executar `Command._reverter` com `builtins.open` fornecendo `StringIO` do JSON; ORM e reaponte reais. `chamados/management/commands/reatar_beco_folga_ao_limbo.py:119`, `chamados/management/commands/reatar_beco_folga_ao_limbo.py:127` |

Decisão sugerida para reversão: aplicar a mesma guarda, sem exceção. LIMBO→I eleito passa; I eleito vivo→G recusa. O reaponte para `None` atualmente não consulta `_costura_nao_mata_o_fio`; protegê-lo exige chamada incondicional e suporte a destino nulo, ou declarar explicitamente esse caso fora do delta. `chamados/services/fio.py:910`

Existem fixtures úteis de migração, LIMBO e encerramento em `chamados/tests/test_e1_v5_chave_familia.py:169`, `chamados/tests/test_o261e_f1_codex_v5.py:106`, `chamados/tests/test_o261e_f1_porta_encerramento.py:46`. Execução ORM e resultados: [não verificado].

## 14/11 15:00 · sessao 9046ca1fcf8d · - · - · origem=Ronald (tmux)

**PROCEDENTE — reclassificar o descarte do integrador.** Continuar o push não isola a fatia recusada: `_commitar` falso apenas deixa de acrescentá-la a `subiram`; não chama `_desfazer` nem retira seus arquivos da árvore (`app/core/integrador_lote.py:719-720`). Os arquivos já foram copiados (`app/core/integrador_lote.py:317-324`), e o deploy posterior segue (`app/core/integrador_lote.py:739,753`), importando a árvore do disco (`bin/deploy.sh:327,360`). Portanto a fatia recusada pode ser publicada sem commit; ocorrência real [nao verificado].

Há ainda um reload anterior ao julgamento: `deploy.sh --reload-copia` roda antes da suite e dos commits (`app/core/integrador_lote.py:638-639`). A recusa posterior não desfaz esse efeito (`app/core/integrador_lote.py:719-720`).

A verificação de índice chamada pelo pre-push não fecha esse caminho: `--conferir` verifica somente existência e chamada do pre-commit, depois retorna 0 (`bin/index_vs_arvore.sh:35-58`).

## 14/11 14:59 · sessao d50aab665e87 · - · - · origem=Ronald (tmux)

Item 3 — dois PROCEDENTE:

- **Bateria 5/5 sem execução do caso real.** `_bateria` define cinco verificações, mas só é chamada contra o mutante que aceita tudo; o instalado passa por dois casos do 1b. O repo `$P` é plantado e nunca submetido a `_bateria`. Recusar termo legítimo, recusar `n/a` em `bin/` ou aceitar `n/a` em `app/ponto` pode permanecer verde, embora a saída anuncie “bateria 5/5”. Citações do diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:68`, `:80`, `:85`, `:97`, `:112`.

- **Hooks de `core.hooksPath` continuam com delegação apenas textual.** A varredura usa `_delega "$hp"`; o 1b executa exclusivamente o hook do common dir. Assim, uma worktree cujo hooksPath contenha hook executável que cite `bin/commit_msg.sh` em comentário e termine em `exit 0` passa na varredura. O MORDE(c) demonstra exatamente esse falso, mas exercita `_roda`, sem aplicar essa prova aos alvos alternativos da varredura. Citações do diff: `bin/tests/test_hook_mensagem_em_toda_copia.sh:23`, `:40`, `:90`, `:99`. Existência desse desvio na máquina real: [nao verificado].

O `while read ... <<< "$lista"` não introduz, sozinho, uma entrada vazia adicional: a substituição de comando remove os delimitadores finais e a here-string acrescenta o delimitador para a última entrada; não identifico achado nesse ponto do diff `bin/tests/test_hook_mensagem_em_toda_copia.sh:31` e `:41`. Worktree registrada sem pasta agora gera diagnóstico deliberadamente; ocorrência na máquina real: [nao verificado].

## 14/11 14:58 · sessao 51aec7f36248 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — cura incompleta do ambiente herdado na régua.** `bin/regua.sh:138-140` executa o selo sem remover `LEI_AKITA_COMMIT`. A nova guarda só recusa quando também há HEAD/MORDE; COMMIT sozinho ainda seleciona julgamento isolado e retorna antes de universo/MORDE (`bin/tests/test_lei_akita.sh:25-27,1000-1003`, linhas novas do diff). O pre-push está saneado (`bin/pre-push.sh:38`, diff); ocorrência real desse ambiente na régua [nao verificado].

A guarda não derruba o MORDE interno normal: ele chama diretamente `python3 -c`, sem reentrar na guarda shell (`bin/tests/test_lei_akita.sh:1015`, linha nova; base `:979`). A régua normal não acrescenta COMMIT/HEAD/MORDE (`bin/regua.sh:138-140`); o pre-push remove COMMIT (`bin/pre-push.sh:38`, diff); o hook remove HEAD/MORDE (`bin/commit_msg.sh:18`, diff).

4. **nada.** O selo lê os dois textos, mas exige somente a presença de `LEI-AKITA` (`bin/tests/test_lei_akita.sh:30,58-59`, linhas novas; base `:24,52-53`). Outro selo exige `registro.baixa`, fora do trecho alterado (`bin/tests/test_baixa_diferida_host.sh:34`; `bin/fabricante_prompt.md:49-58`, diff). O fabricante entrega o prompt inteiro ao modelo (`bin/fabricante.sh:158-162`); o gerador lê apenas `esteira.sh`, `rodar.sh` e `cadeia.sh` (`bin/molde_fatia/gerar.py:29-35`). O efeito nas próximas saídas do modelo [nao verificado].

## 14/11 14:54 · sessao 9e692c4352d3 · HOOKMSG · verificador · origem=Code

Referências **B** = cópia/base; **P** = linhas resultantes do patch fornecido. HEAD efetivo, instalação dos hooks e censo das 113 worktrees: **[nao verificado]**.

**1. Extração de `julgar_commit`: nada isoladamente; PROCEDENTE na integração.**

A extração preserva o julgamento: merge continua pelo hunk próprio; raiz pelo `show`; demais commits pelo diff contra primeiro pai, com `--no-renames` e `-z`. `medida_do_commit` continua recebendo o SHA julgado. Duplicatas continuam dispensadas antes do julgamento e `EXCECAO_NOMINAL` aplicada depois. Há releitura de mensagem/pais, sem alteração demonstrada do predicado. (`B bin/tests/test_lei_akita.sh:458–486`; `P bin/tests/test_lei_akita.sh:408–430,490–505`)

**PROCEDENTE:** o modo novo pode desviar o juiz do push. O pre-push define somente `LEI_AKITA_HEAD`; um `LEI_AKITA_COMMIT=<SHA válido aprovado>` herdado ativa a saída antecipada, força esse SHA como alvo e pula universo, catraca e verificações gerais. Isso contradiz “nenhum env de bypass nasce”; as outras guardas do pre-push continuam existindo. (`B bin/pre-push.sh:38`; `P bin/tests/test_lei_akita.sh:23,507–512,996–1002`; `contrato.txt:1`, cláusula 5)

**2. Objeto sintético: PROCEDENTE quanto à equivalência geral.**

**`GIT_INDEX_FILE`: nada contra a ordem apresentada.** O `write-tree` ocorre antes da remoção das variáveis `GIT_*`, portanto recebe o índice herdado do hook. O cálculo de `CASA` acontece em subshell e não muda o diretório corrente. Não há fundamento no script para afirmar que ele troca prematuramente o índice temporário pelo índice normal. (`P bin/commit_msg.sh:15,18–19,27`)

A entrega efetiva do índice temporário pelo Git e o ciclo interno dos comandos abaixo não foram reproduzidos nesta cópia: **[nao verificado]**.

| Caso | Classificação |
|---|---|
| **Merge direto, `git merge -m`** | **Julga a errada** no fluxo automático em que o hook antecede a gravação de `MERGE_HEAD` **[nao verificado quanto ao ciclo interno do Git]**. Sem esse arquivo, o script monta somente o pai HEAD: passa a julgar todo o delta recebido, em vez do hunk próprio do merge. Pode recusar `n/a (instrumento)` num merge limpo que o juiz do push aceitaria. (`P bin/commit_msg.sh:20–25`; `P bin/tests/test_lei_akita.sh:416–429`) |
| **`merge --no-ff --no-commit` seguido de `commit`** | **Julga a estrutura certa**, desde que `MERGE_HEAD` contenha os pais finais: usa árvore do índice, HEAD e os pais desse arquivo. Continua sujeito à divergência de limpeza da mensagem abaixo. (`P bin/commit_msg.sh:19–25`) |
| **`--amend`** | **Julga a errada relativamente ao commit final:** usa HEAD como pai, portanto julga o delta da emenda. Emenda apenas de mensagem pode ter lista de arquivos vazia e aceitar `n/a` embora o commit final contenha negócio. É limite expressamente declarado, coberto posteriormente pelo push. (`P bin/commit_msg.sh:10–11,21`; `P bin/tests/test_lei_akita.sh:427–430`; `contrato.txt:1`, cláusula 3) |
| **Commit inicial** | **Julga a estrutura certa:** sem HEAD, monta raiz; o juiz enumera os arquivos pelo `show`. Se faltar vocabulário na árvore, recusa antes do julgamento. (`P bin/commit_msg.sh:19–25`; `P bin/tests/test_lei_akita.sh:91–131,427–429`) |
| **`commit -a` / `commit <paths>`** | **Julga a árvore certa se o Git entregar o índice preparado:** o script preserva `GIT_INDEX_FILE` até concluir a montagem. A comparação efetiva com um commit desses: **[nao verificado]**. (`P bin/commit_msg.sh:19–27`) |
| **`cleanup=verbatim` / `scissors`** | **Pode julgar mensagem errada.** O script aplica sempre `stripspace -s`, sem consultar o modo de cleanup. Em scissors, pode julgar uma linha inválida abaixo do corte e recusar indevidamente; em verbatim, não preserva necessariamente os bytes que serão gravados. Com `core.commentChar=L`, por exemplo, a limpeza pode eliminar a própria linha LEI-AKITA. Aplicação efetiva desses modos pelo Git: **[nao verificado]**. (`P bin/commit_msg.sh:17,24–25`) |
| **Rebase / cherry-pick** | **Sem invocação do hook, não julga.** Se chamado para um commit novo de um pai, a montagem usa HEAD; se envolver emenda/reword, existe o problema de `--amend`. A generalização “sem `--edit` não chama” aparece somente como comentário, sem prova na cópia: **[nao verificado]**. (`P bin/commit_msg.sh:10–11,21–28`) |

O objeto também não reproduz explicitamente todos os metadados e assinatura do commit final; o juiz usa mensagem, árvore e pais, portanto identidade exata do SHA não é demonstrada nem necessária para esse predicado. (`P bin/commit_msg.sh:25`; `P bin/tests/test_lei_akita.sh:413–430`)

**3. Modo `LEI_AKITA_COMMIT`: nada quanto aos itens 1–3 decidirem o resultado; vocabulário decide.**

Os itens 1–3 **rodam antes**, mas apenas acumulam `falhas`; não há `errexit`. O modo commit oculta seu stdout e retorna antes de consultar esse contador. Assim, esses itens não recusam nem aprovam por conta própria nesse caminho. (`B bin/tests/test_lei_akita.sh:16,24–53,983–987`; `P bin/tests/test_lei_akita.sh:23,996–1002`)

O vocabulário é carregado efetivamente antes do ramo commit; GLOSSARIO ou NORTE ilegíveis recusam. `producao` e a catraca são definidos, mas não chamados nesse modo; o ramo também sai antes do MORDE. (`P bin/tests/test_lei_akita.sh:91–131,394–400,507–513`)

**ALVO correto no caminho normal:** o shell passa `LEI_AKITA_HEAD="$LEI_AKITA_COMMIT"` ao Python, que lê o vocabulário da árvore sintética. Entretanto, `LEI_AKITA_MORDE=1` herdado força o ALVO Python para HEAD e pode trocar essa fonte: **PROCEDENTE condicional**. (`P bin/tests/test_lei_akita.sh:70–78,507–513,998`)

**4. QUEM ESCREVE / QUEM LÊ: PROCEDENTE no contrato das mensagens fabricadas.**

| Elemento | Escritor → leitor |
|---|---|
| `.git/hooks/commit-msg` | `hooks.sh install` escreve o wrapper no caminho fixo da principal; o conteúdo executa `commit_msg.sh` com os argumentos recebidos. (`P bin/hooks.sh:9,14–15,70–76`) |
| `bin/commit_msg.sh` | Novo arquivo do patch, chamado pelo wrapper; monta o objeto e chama o selo existente. (`P bin/hooks.sh:74`; `P bin/commit_msg.sh:19–28`) |
| `LEI_AKITA_R` | Escrito pelo hook com `TOPO`; lido para definir `R` no selo. (`P bin/commit_msg.sh:18,28`; `B bin/tests/test_lei_akita.sh:17`) |
| `LEI_AKITA_COMMIT` | Escrito pelo hook com o SHA sintético; lido no seletor Bash, ramo Python e execução do juiz. (`P bin/commit_msg.sh:25,28`; `P bin/tests/test_lei_akita.sh:23,507–510,996–998`) |
| `LEI_AKITA_HEAD` / `ALVO` | Pre-push escreve HEAD do push; modo commit substitui pela revisão sintética. `_ler` e `_show` usam ALVO como revisão-fonte. (`B bin/pre-push.sh:38`; `P bin/tests/test_lei_akita.sh:21–24,76–78,998`) |

Quanto aos chamadores:

- **`pos_push.sh`: passa pela ausência de linha**, pois sua mensagem automática não contém LEI-AKITA. O mesmo ocorre nas mensagens de `relato`, `codex_estado` e `reverter_o122`. (`B bin/pos_push.sh:30–38`; `B bin/relato.sh:136`; `B bin/codex_estado.sh:83`; `B bin/reverter_o122.sh:30`; `P bin/commit_msg.sh:17`)
- **Fabricante: PROCEDENTE.** O prompt exige formato “exato” sem `padrao=`, e o LEIAME repete esse formato. Se seguido literalmente, o juiz recusa por campo ausente. (`B bin/fabricante_prompt.md:49–56`; `B bin/molde_fatia/LEIAME:44–46`; `B bin/tests/test_lei_akita.sh:319–321`)
- **Esteira/integrador:** copia a mensagem, substitui RED/Suite e commita com `-F ... -- caminhos`; não acrescenta `padrao=`. As cadeias antigas também usam diretamente o arquivo de mensagem. (`B bin/molde_fatia/esteira.sh:14,22,26`; `B app/core/integrador_lote.py:481`; `B bin/molde_fatia/cadeia.sh:68`; `B bin/molde_autocura/cadeia.sh:65`)
- **`codex_pergunta --construtor`:** proíbe commit; **disjuntor:** os comandos Git encontrados consultam worktrees, histórico e status. Não há mensagem automática desses caminhos demonstrada como vítima do hook. (`B bin/codex_pergunta.sh:343–346`; `B bin/disjuntor.py:1234–1247`)

Recusa não demonstra travamento: no integrador, commit malsucedido não entra em `subiram`, mas o fluxo segue ao push. Incidência em pacotes reais: **[nao verificado]**. (`B app/core/integrador_lote.py:719–720,739`)

**5. Selo: PROCEDENTE — há verdes vazios.**

**Reconhecedor divergente:** para `titulo\rLEI-AKITA: padrao=Inventado`, o `grep` inicial não encontra a linha e o hook retorna 0; `tem_linha` Python reconhece a linha após o CR. Conferido em memória, sem arquivos. Logo existe mensagem que o hook dispensa e o juiz do push examinaria. (`P bin/commit_msg.sh:17`; `B bin/tests/test_lei_akita.sh:339–355`)

**Delegação apenas textual:** `_delega` exige executável e substring. Um hook com `exit 0` e a referência somente em comentário satisfaz essa condição. A bateria executa outro wrapper, plantado pelo teste, sem provar a execução do instalado ou do alvo efetivo dele. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:24,40–42,55,74`)

**Censo vazio por erro:** falha de `worktree list` não é conferida pelo chamador; falha de configuração vira `continue`; diretório ausente é pulado. Só a saída textual vazia determina aprovação. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:28–33,44–45`)

Os MORDEs provam menos que a promessa:

- **(a)** detecta o script mutante que aceita tudo na fixture; não testa o wrapper instalado. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:76–81`)
- **(b)** detecta o desvio plantado por worktree e sua retirada; não prova que o censo real enumerou alguma worktree. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:83–88`)

**Global/sistema e relativo: nada quanto à omissão explícita desses escopos.** A consulta não restringe a configuração a `--local`; caminhos relativos são ancorados em cada worktree. A cobertura continua sujeita aos erros silenciosos e ao teste textual de delegação. Configurações efetivas do host: **[nao verificado]**. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:30–32`)

**Selo órfão: nada.** Seu nome entra no glob executado pela régua. (`B bin/regua.sh:138–145`)

**6. Classes de reprovação.**

| Classe | Achado |
|---|---|
| **Segundo juiz duplicando o registrado** | **nada no predicado principal:** `julgar_commit` chama o `julgar` existente. (`P bin/tests/test_lei_akita.sh:408–430`; `B bin/tests/test_lei_akita.sh:336–344`) |
| **Derivação paralela de fonte única** | **PROCEDENTE:** “tem linha” é recalculado pelo `grep`, com semântica divergente do reconhecedor Python. (`P bin/commit_msg.sh:17`; `B bin/tests/test_lei_akita.sh:354–355`) |
| **Selo sem mordida** | **PROCEDENTE na ligação com o instalado:** a mordida funcional está na fixture; o wrapper real passa por inspeção textual. (`P bin/tests/test_hook_mensagem_em_toda_copia.sh:24,40,55,74–81`) |
| **ALVO com dois significados** | **nada no caminho normal:** significa revisão-fonte, seja ponta do push ou objeto sintético. O defeito demonstrado é o seletor ambiental que pode trocar indevidamente o caminho de julgamento. (`P bin/tests/test_lei_akita.sh:21–24,76–78,507–512,998`) |

## 14/11 14:50 · sessao b39e0829dc2b · - · - · origem=Ronald (tmux)

Referências **B** = arquivo da cópia/base; **P** = linhas resultantes do diff fornecido.

- **1 — nada na extração do PUSH, isoladamente.** `julgar_commit` conserva os comandos de merge, raiz, `--no-renames`, `-z` e a chamada de `medida_do_commit`; o laço conserva os filtros de duplicatas antes e exceção nominal depois. Apenas relê mensagem/pais do SHA. [B `bin/tests/test_lei_akita.sh:458`; B `bin/tests/test_lei_akita.sh:468`; P `bin/tests/test_lei_akita.sh:408`; P `bin/tests/test_lei_akita.sh:490`; P `bin/tests/test_lei_akita.sh:500`]

- **3 — itens 1–3 executam, mas não decidem o modo commit.** O script não tem `errexit`; essas verificações só incrementam `falhas`. O modo commit redireciona stdout e retorna antes do tratamento final desse contador. Vocabulário executa efetivamente antes do ramo commit e rejeita GLOSSARIO/NORTE ilegíveis; `producao`, catraca e MORDE não são chamados nesse caminho. [B `bin/tests/test_lei_akita.sh:16`; B `bin/tests/test_lei_akita.sh:24`; B `bin/tests/test_lei_akita.sh:35`; B `bin/tests/test_lei_akita.sh:100`; B `bin/tests/test_lei_akita.sh:129`; P `bin/tests/test_lei_akita.sh:23`; P `bin/tests/test_lei_akita.sh:507`; P `bin/tests/test_lei_akita.sh:996`]

- **ALVO: nada quanto a campo com dois significados, no caminho normal.** É a revisão que fornece o vocabulário: ponta do push ou objeto sintético do commit. O shell força `LEI_AKITA_HEAD="$LEI_AKITA_COMMIT"` para o Python. `medida_do_commit` continua consultando `h`, independentemente do ALVO global. [B `bin/tests/test_lei_akita.sh:74`; B `bin/tests/test_lei_akita.sh:76`; B `bin/tests/test_lei_akita.sh:296`; P `bin/tests/test_lei_akita.sh:998`]

- **PROCEDENTE — nasce seletor ambiental que pode desviar o juiz do PUSH.** O pre-push força apenas `LEI_AKITA_HEAD`; um `LEI_AKITA_COMMIT=<SHA válido aprovado>` herdado ativa o novo modo, troca o alvo e evita universo, catraca e itens 1–3. Não há evidência de uso em produção. [B `bin/pre-push.sh:38`; P `bin/tests/test_lei_akita.sh:23`; P `bin/tests/test_lei_akita.sh:507`; P `bin/tests/test_lei_akita.sh:998`; produção: **[nao verificado]**]

- **PROCEDENTE — segundo reconhecedor divergente produz verde vazio.** O gate `grep` retorna “sem linha”, mas `tem_linha` do juiz retorna verdadeiro para `titulo\rLEI-AKITA: padrao=Inventado\n`, `titulo\vLEI-AKITA: ...` e prefixo NBSP. Verifiquei as expressões em memória, sem arquivos; `git stripspace -s` preserva esses bytes. O hook sai antes de chamar o juiz; o juiz do PUSH reconhece tais linhas. [P `bin/commit_msg.sh:17`; B `bin/tests/test_lei_akita.sh:354`; B `bin/tests/test_lei_akita.sh:339`]

- **PROCEDENTE condicional — `LEI_AKITA_MORDE=1` herdado troca o vocabulário.** O Python fixa ALVO em HEAD quando MORDE está ativo; o ramo commit roda antes do ramo MORDE. Assim pode julgar o SHA sintético com vocabulário do HEAD. A ocorrência desse ambiente em produção não foi demonstrada. [B `bin/tests/test_lei_akita.sh:68`; B `bin/tests/test_lei_akita.sh:74`; P `bin/tests/test_lei_akita.sh:507`; P `bin/tests/test_lei_akita.sh:513`; produção: **[nao verificado]**]

Coordenadas P conferidas por contagem das alterações: início `+2`, extração `+30`, substituição de 14 linhas por uma `−13`, ramo commit Python `+6`, total `+25`; `julgar_commit:408–430`, ramo Python `507–512`, ramo shell `996–1002`. [diff fornecido: **[nao verificado por arquivo físico]**] [nao verificado]

## 14/11 14:50 · sessao ff798a325404 · - · - · origem=Ronald (tmux)

Q4: **PROCEDENTE condicional nos geradores de mensagens; nenhum travamento demonstrado.**

- `bin/hooks.sh install` escreve o wrapper no caminho fixo da principal, delegando para `bin/commit_msg.sh`; os destinos novos são `FONTE_MSG`/`ALVO_MSG` e o `exec` está no bloco adicionado do diff. Não há instalação realizada nem hooks reais no dado fornecido. (`diff_v1.patch`, hunk de `bin/hooks.sh:9` e hunk `+65`; instalação atual [nao verificado].)
- Escritor de `LEI_AKITA_COMMIT` e `LEI_AKITA_R`: a nova chamada de `bin/commit_msg.sh:28`; leitores: modo Bash e ramo Python acrescentados em `bin/tests/test_lei_akita.sh`. `LEI_AKITA_HEAD` já é escrito pelo pre-push e lido pelo selo; `ALVO` Bash alimenta `_ler`, e o Python possui sua própria variável `ALVO`. (`bin/pre-push.sh:38`; `bin/tests/test_lei_akita.sh:17`; `bin/tests/test_lei_akita.sh:21`; `bin/tests/test_lei_akita.sh:22`; `bin/tests/test_lei_akita.sh:74`; `diff_v1.patch`, hunks `+19`, `+497`, `+993`.)
- **Possível bypass novo:** `LEI_AKITA_COMMIT=<SHA sem linha>` herdado no push ativa o modo que julga apenas aquele SHA e sai antes do universo/catraca; o pre-push atribui somente `LEI_AKITA_HEAD`, sem limpar a variável nova. Outras guardas do push continuam aplicáveis. Isso contradiz a promessa “nenhum env de bypass nasce”. (`bin/pre-push.sh:38`; `diff_v1.patch`, ramo `if os.environ.get('LEI_AKITA_COMMIT')` e bloco Bash do hunk `+993`; `contrato.txt:1`.)

Chamadores:

| Sítio | Resultado pelo código |
|---|---|
| `pos_push.sh` | Mensagem automática não contém LEI-AKITA; passa pelo retorno inicial do hook. (`bin/pos_push.sh:30`; `bin/pos_push.sh:31`; `bin/pos_push.sh:37`; proposta `bin/commit_msg.sh:17`.) |
| `codex_pergunta --construtor` | Proíbe commit; inventário verifica HEAD e descarta a saída quando alterado. Não monta mensagem de commit. (`bin/codex_pergunta.sh:345`; `bin/codex_pergunta.sh:148`; `bin/codex_pergunta.sh:181`; `bin/codex_pergunta.sh:437`; `bin/codex_pergunta.sh:441`.) |
| `disjuntor.py` | Os subprocessos Git encontrados consultam worktrees, log e status, sem commit; ele não depende de mensagem automática LEI-AKITA. (`bin/disjuntor.py:1234`; `bin/disjuntor.py:1237`; `bin/disjuntor.py:1244`; `bin/disjuntor.py:1247`.) |
| Fabricante | Prompt exige formato “exato” de LEI-AKITA **sem `padrao=`** e proíbe commit direto; se seguido literalmente, o novo hook recusará no integrador. (`bin/fabricante_prompt.md:49`; `bin/fabricante_prompt.md:52`; `bin/fabricante_prompt.md:70`; `bin/tests/test_lei_akita.sh:319`; `bin/tests/test_lei_akita.sh:321`.) |
| Esteira/integrador | Copia `msg_commit.molde` para `.txt`, substitui somente RED/Suite e entrega ao integrador; este executa `git commit -F msg_commit.txt -- caminhos`, sem completar o campo ausente. (`bin/molde_fatia/esteira.sh:14`; `bin/molde_fatia/esteira.sh:22`; `bin/molde_fatia/esteira.sh:26`; `app/core/integrador_lote.py:481`.) |
| Cadeias antigas | Também commitam o arquivo de mensagem diretamente; mensagem sem `padrao=` será recusada. (`bin/molde_fatia/cadeia.sh:68`; `bin/molde_autocura/cadeia.sh:65`; `bin/tests/test_lei_akita.sh:321`.) |
| Outros automáticos | `relato`, `codex_estado` e `reverter_o122` geram mensagens sem LEI-AKITA, portanto passam pelo retorno inicial. (`bin/relato.sh:136`; `bin/codex_estado.sh:83`; `bin/reverter_o122.sh:30`; proposta `bin/commit_msg.sh:17`.) |

A recusa de `_commitar` apenas impede adicionar a fatia em `subiram`; o integrador segue ao push. Incidência em pacotes reais e produção: [nao verificado]. (`app/core/integrador_lote.py:719`; `app/core/integrador_lote.py:720`; `app/core/integrador_lote.py:739`.)

Q6, juiz registrado: **nada quanto a duplicação de juiz de domínio**. O mapa descreve juízes de domínio gerados de `core/juizes.py`; a proposta reutiliza `julgar` existente por meio de `julgar_commit`. Há, porém, reconhecimento paralelo de “tem linha” no grep do hook e em `tem_linha` Python, sujeito a divergência de whitespace Unicode. (`app/docs/MAPA-JUIZES.md:3`; `app/docs/MAPA-JUIZES.md:7`; `bin/tests/test_lei_akita.sh:336`; `bin/tests/test_lei_akita.sh:354`; proposta `bin/commit_msg.sh:17`; `diff_v1.patch`, definição nova `julgar_commit`.)

## 14/11 14:49 · sessao 1b363f83bb2d · - · - · origem=Ronald (tmux)

**Item 5 — PROCEDENTE: selo pode ficar verde com hook instalado inoperante.** `_delega` verifica somente executável e ocorrência textual de `bin/commit_msg.sh`; um wrapper executável contendo `exit 0` e `# /caminho/existente/bin/commit_msg.sh` satisfaz a inspeção, inclusive existência do alvo. A bateria executa outro wrapper, criado no repositório plantado, e não o instalado. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:24`, `:40`, `:41`, `:55`, `:74`.

**Item 5 — PROCEDENTE: censo vazio por erro pode passar.** Falha de `worktree list` não é checada pelo chamador; erro de `config --get` vira `continue`; caminhos inexistentes também são pulados. `DESV` vazio é aprovado independentemente do status da função. O MORDE usa outro repositório e não exige uma worktree sequer enumerada no repositório real. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:28`, `:29`, `:30`, `:44`, `:45`, `:83`. Ocorrência disso em produção: **[nao verificado]**.

**MORDE (a): prova estreita, válida por inspeção.** O mutante ganha `exit 0`; sua bateria precisa denunciar aceitação do padrão inventado. Isso testa o script mutante na fixture, não a execução efetiva do hook instalado. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:76`, `:78`, `:81`.

**MORDE (b): prova estreita, válida por inspeção.** Planta configuração por worktree apontando para pasta vazia, exige denúncia e depois ausência de denúncia ao remover a configuração. Não cobre configuração global/sistema plantada nem enumeração real vazia. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:82`, `:83`, `:85`, `:86`.

**hooksPath global/sistema/relativo: nada quanto à omissão explícita de escopo.** A consulta usa `git config --get`, sem restringir a `--local` ou `--worktree`; valores relativos são ancorados na raiz de cada worktree. São usadas as mesmas condições textuais fracas de delegação. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:30`, `:31`, `:32`. Configurações efetivamente presentes no host: **[nao verificado]**.

**Selo órfão: nada.** A régua descobre `bin/tests/test_*.sh` e executa cada arquivo por Bash; o novo nome entra no glob. `bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`.

**Item 6 — selo sem mordida: PROCEDENTE na ligação com o instalado.** A mordida funcional existe na fixture; a ligação com o wrapper real permanece um grep que aceita comentário ou código inalcançável. Referências propostas: `bin/tests/test_hook_mensagem_em_toda_copia.sh:24`, `:40`, `:55`, `:74`.

## 14/11 13:55 · sessao 0911a0f79119 · HJ-091 · sonda · origem=Code

| Campo | Valor |
|---|---|
| universo_admin_encerrado_colab_com_ferias | `845` [nao verificado] |
| com_dia_do_fato | `419` [nao verificado] |
| dia_coberto_por_ferias | `27` [nao verificado] |
| veto_admin_sem_fato_novo_ativo_hoje | `16` [nao verificado] |
| erros | `0` [nao verificado] |
| pks_veto | `[3331, 3444, 3934, 4533, 4879, 5968, 6184, 7081, 7084, 7805, 7809, 8640, 11141, 11776, 12526, 12801]` [nao verificado] |
| DEPOIS | `{'pk': 3977, 'tipo': 'ferias', 'status': 'rejeitada', 'agendamento_ferias_id': 103, 'data': datetime.date(2026, 11, 7)}` [nao verificado] |
| DEPOIS | `{'pk': 3985, 'tipo': 'ferias', 'status': 'rejeitada', 'agendamento_ferias_id': 103, 'data': datetime.date(2026, 11, 5)}` [nao verificado] |
| DEPOIS | `{'pk': 3995, 'tipo': 'ferias', 'status': 'rejeitada', 'agendamento_ferias_id': 103, 'data': datetime.date(2026, 11, 6)}` [nao verificado] |
| DEPOIS | `{'pk': 5592, 'tipo': 'abono', 'status': 'aprovada', 'agendamento_ferias_id': None, 'data': datetime.date(2026, 11, 5)}` [nao verificado] |
| col344_0507 | `[(3977, 'ferias', 'rejeitada', 103), (3985, 'ferias', 'rejeitada', 103), (3995, 'ferias', 'rejeitada', 103), (5592, 'abono', 'aprovada', None)]` [nao verificado] |
| rejeitadas_sem_origem_em_agendamento_vivo | `0 []` [nao verificado] |
| ferias_sem_origem_total | `60` [nao verificado] |
| Ocorrências literais de ERRO no dado raspado | `0` [nao verificado] |
| Ocorrências literais de Traceback no dado raspado | `0` [nao verificado] |

## 14/11 13:53 · sessao 3423c9c36e81 · HJ-091 · sonda · origem=Code

| Campo | Valor |
|---|---|
| universo_admin_encerrado_colab_com_ferias [nao verificado] | 845 |
| com_dia_do_fato [nao verificado] | 419 |
| dia_coberto_por_ferias [nao verificado] | 27 |
| veto_admin_sem_fato_novo_ativo_hoje [nao verificado] | 16 |
| erros [nao verificado] | 0 |
| pks_veto [nao verificado] | [3331, 3444, 3934, 4533, 4879, 5968, 6184, 7081, 7084, 7805, 7809, 8640, 11141, 11776, 12526, 12801] |
| TRES [nao verificado] | 3977 col 344 data 2026-11-07 tipo ferias status rejeitada marcas [103] janelas [103] |
| TRES [nao verificado] | 3985 col 344 data 2026-11-05 tipo ferias status rejeitada marcas [103] janelas [103] |
| TRES [nao verificado] | 3995 col 344 data 2026-11-06 tipo ferias status rejeitada marcas [103] janelas [103] |
| coluna_fk_existe [nao verificado] | False |
| Ocorrências literais de ERRO no bloco fornecido [nao verificado] | 0 |
| Ocorrências literais de Traceback no bloco fornecido [nao verificado] | 0 |

O bloco transcrito informa `erros 0`; a afirmação “erro NAO e zero” não encontra suporte nesse bloco. O total real de erros da execução permanece [nao verificado].

## 14/11 13:50 · sessao af94840d4a0c · FRAC5 · verificador · origem=Code

**Os dois bypasses centrais da volta 2 estão fechados na remarcação regular: ausência de fração ≥14 e divergência entre `dias` e intervalo. Não encontrei novo bypass de gravação introduzido pelo delta.** Permanecem limitações nos testes e caminhos anteriores que não passam por `clean()`. (`app/ferias/services.py:680`; `app/ferias/services.py:686`; `app/ferias/services.py:718`; `app/ferias/models.py:380`; `app/ferias/models.py:396`)

Execução de RED contra `63ff6e88b`, suíte, navegador, pouso e impacto em produção: **[nao verificado]**. Os resultados contra o pai abaixo são contrafactuais pelo delta fornecido.

**1. NÃO PROCEDE [bloqueia] — julgamento sobre foto velha ou antes da atribuição.**

`vivo` vem de `select_for_update()`. O período também é relido sob trava e atribuído à relação de `vivo`. Depois são atribuídos os novos `data_inicio`, `data_fim` e `dias`; somente então roda `vivo.clean()`. O juiz exclui o próprio PK e usa o intervalo novo para julgar a fração substituta. (`app/ferias/services.py:599`; `app/ferias/services.py:602`; `app/ferias/services.py:603`; `app/ferias/services.py:680`; `app/ferias/services.py:686`; `app/ferias/models.py:371`; `app/ferias/models.py:377`)

**NÃO PROCEDE [bloqueia] — suposta aplicação de saldo, abono, período e sobreposição pelo `clean()`.** O corpo desse método contém somente fracionamento, ordem das datas e igualdade entre quantidade e intervalo. A chamada acrescentada é `clean()`, não `full_clean()`. (`app/ferias/models.py:363`; `app/ferias/models.py:399`; `app/ferias/services.py:686`)

| Validação | Onde ocorre; pertinência à remarcação |
|---|---|
| Máximo de três frações | `clean()` conta os irmãos ativos e a fração substituta. Pode recusar a remarcação de um período que já tenha quatro frações. É o mesmo julgamento registrado, agora aplicado à substituição. (`app/ferias/models.py:368`; `app/ferias/models.py:372`; `app/core/juizes.py:271`) |
| Alguma fração ≥14 | Usa dias gravados dos irmãos e duração nova do próprio objeto. Fecha o caso único **14→5**. (`app/ferias/models.py:377`; `app/ferias/models.py:380`) |
| Fração escrita ≥5 | Julga o intervalo novo; frações curtas históricas dos irmãos não bloqueiam essa cláusula. (`app/ferias/models.py:384`; `app/ferias/models.py:387`) |
| Ordem e equivalência das datas | Recusa fim anterior ao início e `dias` diferente do intervalo inclusivo. Ambas fazem sentido numa troca de janela. Fracionamento tem precedência sobre essas mensagens. (`app/ferias/models.py:380`; `app/ferias/models.py:387`; `app/ferias/models.py:392`; `app/ferias/models.py:396`) |
| Sobreposição | Continua no `save()`, pelo juiz de precedência. A limpeza anterior e a gravação estão no mesmo `atomic`; o veto desfaz ambas. (`app/ferias/services.py:669`; `app/ferias/services.py:694`; `app/ferias/services.py:699`; `app/ferias/models.py:450`; `app/ferias/models.py:454`; `app/ferias/models.py:463`) |
| Período aquisitivo/concessivo | `clean()` usa o período para selecionar irmãos; não exige status `adquirido` nem enquadramento das novas datas no ciclo ou limite concessivo. A troca salva somente concedidos/status do PA. (`app/ferias/models.py:363`; `app/ferias/models.py:368`; `app/ferias/services.py:701`) |
| Saldo | É julgado **antes** da atribuição: saldo atual mais os dias já reservados pelo agendamento. Isso permite substituir a reserva sem cobrá-la duas vezes. O novo `clean()` não refaz essa conta. (`app/ferias/services.py:675`; `app/ferias/services.py:679`; `app/ferias/models.py:220`; `app/ferias/models.py:225`) |
| Abono | A troca não vende dias; vendidos já são descontados pelo saldo. O teto de venda é aplicado nas portas que incluem abono, separadamente. (`app/ferias/models.py:220`; `app/ferias/services.py:312`; `app/ferias/services.py:277`; `app/ferias/services.py:699`; `app/ferias/services.py:701`) |

**NÃO PROCEDE [bloqueia] — ausência de `clean()` significar gravação regular liberada.** Ele não roda quando:

- A linha está `cancelado`: a trava recusa antes, impedindo ressuscitar a reserva. (`app/ferias/models.py:313`; `app/ferias/services.py:600`; `app/ferias/services.py:670`)
- Está `em_gozo`, `concluido`, outro status diferente de `agendado`, ou `agendado` com início já alcançado: `pode_editar_em(hoje)` recusa antes. (`app/ferias/models.py:340`; `app/ferias/models.py:344`; `app/ferias/services.py:671`)
- Falta saldo: a recusa também antecede o julgamento. (`app/ferias/services.py:675`; `app/ferias/services.py:677`)
- A entrada é a correção administrativa: o wrapper mantém `so_nao_iniciado=False`, independentemente do status não cancelado. É a exceção A3 expressa, com aviso pela view. (`app/ferias/services.py:657`; `app/ferias/services.py:681`; `app/ferias/services.py:708`; `app/ferias/views.py:508`; `app/ferias/views.py:552`)

O status novo só é atribuído depois do `clean()`. Isso não altera seu resultado atual: ele filtra o status dos irmãos, mas não consulta `self.status`. (`app/ferias/services.py:691`; `app/ferias/models.py:363`; `app/ferias/models.py:370`)

**2. NÃO PROCEDE [bloqueia] — perda de cobertura ao remover o recorte do wrapper.**

Toda remarcação regular que chega à gravação passa `so_nao_iniciado=True` e atravessa `clean()`. Para `novos_dias=1–4`, um intervalo curto é recusado pelo fracionamento; um intervalo maior é recusado pela desigualdade com `dias`. Chamadas inválidas por status/início/saldo agora podem receber essas recusas primeiro, sem gravar. (`app/ferias/services.py:718`; `app/ferias/services.py:671`; `app/ferias/services.py:676`; `app/ferias/services.py:686`; `app/ferias/services.py:699`; `app/ferias/models.py:387`; `app/ferias/models.py:396`)

| Caminho encontrado | Tratamento de existente e de fração `<5` |
|---|---|
| `remarcar_agendamento` | Converge no escritor com julgamento obrigatório. (`app/ferias/services.py:718`; `app/ferias/services.py:686`) |
| `corrigir_datas_agendamento` | Converge no mesmo escritor sem `clean()`; admite curto sujeito a status, saldo e colisão. A view exige quantidade positiva, calcula o fim e acrescenta avisos. (`app/ferias/services.py:708`; `app/ferias/services.py:670`; `app/ferias/services.py:675`; `app/ferias/views.py:503`; `app/ferias/views.py:507`; `app/ferias/views.py:517`; `app/ferias/views.py:552`) |
| `_trocar_janela` diretamente | O padrão é administrativo, sem `clean()`. Os dois wrappers encontrados escolhem explicitamente suas modalidades. (`app/ferias/services.py:657`; `app/ferias/services.py:708`; `app/ferias/services.py:718`) |
| Django admin | O registro local contém somente `Afastamento`; não oferece edição de `AgendamentoFerias`. (`app/ferias/admin.py:2`; `app/ferias/admin.py:4`) |
| `sincronizar_status_ferias` | Usa a porta que altera somente status; não julga nem corrige duração curta. (`app/ferias/management/commands/sincronizar_status_ferias.py:48`; `app/ferias/services.py:928`; `app/ferias/services.py:929`) |
| `listar_ferias_dp` | Simula cancelamento e validação, depois força rollback; não remarca datas/dias. (`app/ferias/management/commands/listar_ferias_dp.py:100`; `app/ferias/management/commands/listar_ferias_dp.py:107`; `app/ferias/management/commands/listar_ferias_dp.py:109`) |
| Realinhamento de concedidos | Altera o contador do PA, não dias/datas do agendamento. (`app/ferias/services.py:156`; `app/ferias/services.py:160`; `app/ferias/services.py:162`) |
| API do colaborador | POST retorna 405; GET serializa datas, dias e status existentes. (`app/api/views.py:2223`; `app/api/views.py:2244`; `app/api/views.py:2246`) |
| API de mensageria | Consulta situação e pedido hipotético; entrega avisos. (`app/api/views_mensageria.py:812`; `app/api/views_mensageria.py:824`; `app/api/views_mensageria.py:847`) |

**PROCEDE [fila] — pontos anteriores não fechados por este delta:** histórico cria fração curta sem fracionamento; cancelar a fração de 14 pode deixar somente a de 5; `save()` não chama `clean()` universalmente; chamada direta à correção ainda aceita `dias` e intervalo independentes. Nenhum desses mecanismos foi acrescentado pelo delta. (`app/ferias/services.py:492`; `app/ferias/services.py:510`; `app/ferias/services.py:616`; `app/ferias/services.py:620`; `app/ferias/models.py:461`; `app/ferias/services.py:680`; `app/ferias/services.py:681`; `app/ferias/services.py:708`)

O helper ainda ignora zero por `if f`; isso permanece uma limitação dele, mas não permite gravar zero pela remarcação regular com intervalo coerente: igualdade e ordem das datas também são verificadas. (`app/ferias/models.py:271`; `app/ferias/models.py:392`; `app/ferias/models.py:396`)

**3. Quem escreve e quem lê os campos/valores envolvidos no delta.**

| Campo/valor | Quem escreve ou monta | Quem lê |
|---|---|---|
| `so_nao_iniciado` | Padrão `False`; remarcação passa `True`; correção conserva o padrão. (`app/ferias/services.py:657`; `app/ferias/services.py:708`; `app/ferias/services.py:718`) | Guarda de edição e condição do `clean()`. (`app/ferias/services.py:671`; `app/ferias/services.py:681`) |
| `hoje` | Borda lê uma vez por request; testes fornecem datas explícitas. (`app/ferias/views.py:15`; `app/ferias/views.py:695`; `app/ponto/tests/test_ferias_fracao_demais_5.py:145`) | Guarda de início, aviso, status do agendamento e do PA. (`app/ferias/services.py:671`; `app/ferias/services.py:687`; `app/ferias/services.py:691`; `app/ferias/services.py:700`) |
| `nova_ini`, `novos_dias`, `nova_fim` | Views convertem POST e derivam fim; testes também chamam o serviço diretamente. (`app/ferias/views.py:499`; `app/ferias/views.py:507`; `app/ferias/views.py:708`; `app/ferias/views.py:718`; `app/ponto/tests/test_ferias_fracao_demais_5.py:154`) | Saldo, atribuição da janela e, agora, `clean()`. (`app/ferias/services.py:676`; `app/ferias/services.py:680`; `app/ferias/services.py:686`) |
| PK e vínculo ao PA | Criações associam o agendamento ao período; o próprio banco atribui a identidade na inserção. (`app/ferias/services.py:544`; `app/ferias/services.py:545`) | Releitura/travas; seleção dos irmãos; exclusão do próprio PK. (`app/ferias/services.py:599`; `app/ferias/services.py:602`; `app/ferias/models.py:369`; `app/ferias/models.py:371`) |
| `AgendamentoFerias.data_inicio/data_fim/dias` | Escritor da troca atribui e persiste; criação e histórico inicializam. (`app/ferias/services.py:680`; `app/ferias/services.py:699`; `app/ferias/services.py:484`; `app/ferias/services.py:544`) | `clean()`, colisão, espelho, situação e API. (`app/ferias/models.py:377`; `app/ferias/models.py:396`; `app/ferias/models.py:450`; `app/ponto/services/ausencia.py:1185`; `app/ferias/situacao.py:122`; `app/api/views.py:2244`) |
| `AgendamentoFerias.status` | Criação/status por data, troca, cancelamentos e sincronização. (`app/ferias/services.py:546`; `app/ferias/services.py:691`; `app/ferias/services.py:619`; `app/ferias/services.py:651`; `app/ferias/services.py:928`) | Guardas, seleção de irmãos, espelho e API. (`app/ferias/services.py:600`; `app/ferias/models.py:344`; `app/ferias/models.py:370`; `app/ponto/services/ausencia.py:1181`; `app/api/views.py:2247`) |
| `PeriodoAquisitivo.dias_concedidos` | Troca substitui a reserva; criação/histórico somam; cancelamentos subtraem; realinhamento grava a soma. (`app/ferias/services.py:679`; `app/ferias/services.py:548`; `app/ferias/services.py:511`; `app/ferias/services.py:616`; `app/ferias/services.py:643`; `app/ferias/services.py:160`) | Saldo, saldo para remarcar, status do PA e contador de divergência. (`app/ferias/models.py:220`; `app/ferias/models.py:225`; `app/ferias/models.py:98`; `app/ferias/services.py:125`) |
| `dias_direito` e `dias_vendidos` do PA | Art.130/edição escrevem direito; portas de abono/histórico escrevem vendidos. (`app/ferias/services.py:373`; `app/ferias/services.py:824`; `app/ferias/services.py:277`; `app/ferias/services.py:512`; `app/ferias/services.py:906`) | Saldo utilizado antes do novo `clean()`; teto de venda. (`app/ferias/models.py:220`; `app/ferias/models.py:238`; `app/ferias/services.py:675`) |
| `PeriodoAquisitivo.status` | `aplicar_status()` recebe o veredito e a troca o persiste. (`app/ferias/models.py:215`; `app/ferias/services.py:700`; `app/ferias/services.py:701`) | Elegibilidade para agendar e apresentação da API. (`app/ferias/models.py:234`; `app/api/views_mensageria.py:841`) |
| `aviso_emitido`, antecedência 30 | A troca deriva o flag usando a constante; criação também o inicializa. (`app/ferias/situacao.py:56`; `app/ferias/services.py:687`; `app/ferias/services.py:547`) | View, situação e API. (`app/ferias/views.py:733`; `app/ferias/situacao.py:265`; `app/api/views_mensageria.py:845`) |
| Irmãos: `dias`, status e PA | Portas acima; fixtures novas inserem irmã `concluido`, 20 dias, de 05–24/01/2026, com PA e usuário existentes. (`app/ferias/test_cancelar_2x.py:247`; `app/ferias/test_remarcar_limpa_janela_f346.py:35`) | Consultas de irmãos do `clean()` e da situação; o novo `clean()` não lê `agendado_por`. (`app/ferias/models.py:363`; `app/ferias/models.py:368`; `app/ferias/situacao.py:117`) |
| Duração calculada, frações e `caput/par1/par1_demais` | `clean()` monta duração/listas; helper compara com constantes 3/14/5. (`app/ferias/models.py:251`; `app/ferias/models.py:273`; `app/ferias/models.py:275`; `app/ferias/models.py:277`; `app/ferias/models.py:377`; `app/ferias/models.py:379`) | Ramos de recusa do `clean()` e ramos de aviso; wrapper regular deixou de escolher somente `par1_demais`. (`app/ferias/models.py:372`; `app/ferias/models.py:380`; `app/ferias/models.py:387`; `app/ferias/situacao.py:227`; `app/ferias/services.py:718`) |
| Mensagens de `ValidationError` | Ramos do `clean()`; colisão traduzida no `save()`. (`app/ferias/models.py:373`; `app/ferias/models.py:381`; `app/ferias/models.py:388`; `app/ferias/models.py:397`; `app/ferias/models.py:454`) | View apresenta `e.messages`; testes conferem trechos. (`app/ferias/views.py:730`; `app/ponto/tests/test_ferias_fracao_demais_5.py:137`; `app/ponto/tests/test_ferias_fracao_demais_5.py:146`; `app/ponto/tests/test_ferias_fracao_demais_5.py:155`) |
| Avisos `{codigo,texto,nivel,ancora}` | `_aviso()` e situação montam. (`app/ferias/situacao.py:64`; `app/ferias/situacao.py:229`; `app/ferias/situacao.py:238`) | Índice preserva listas, API entrega, correção mostra textos e novo teste inspeciona a saída. (`app/ferias/situacao.py:302`; `app/api/views_mensageria.py:847`; `app/ferias/views.py:552`; `app/ponto/tests/test_ferias_fracao_demais_5.py:183`) |

**4. PROCEDE [fila] — as fixtures perderam coerência agregada; NÃO PROCEDE [bloqueia] — terem eliminado a mordida da limpeza e do veto.**

Na F346, o contador continua **10**, mas as linhas passam a somar **30**. Após reduzir para cinco, a asserção exige contador **5**, enquanto os agendamentos somam **25**. No teste da foto velha, contador **30** e agendamento **30** recebem uma irmã **20**; após remarcar para dez, espera-se contador **10**, mas as linhas somam **30**. Essas fixtures deixam de provar saldo agregado íntegro; a divergência é precisamente a condição medida pelo contador existente. (`app/ferias/test_remarcar_limpa_janela_f346.py:28`; `app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_remarcar_limpa_janela_f346.py:83`; `app/ferias/test_cancelar_2x.py:51`; `app/ferias/test_cancelar_2x.py:55`; `app/ferias/test_cancelar_2x.py:247`; `app/ferias/test_cancelar_2x.py:253`; `app/ferias/services.py:117`)

A irmã elimina o impedimento de falta de fração ≥14 para manter os cenários de limpeza. O R06 continua com três frações, alcança a colisão no `save()` e confere rollback de ausências, contador e início. A adulteração da foto para 999 também continua exercitada. (`app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_remarcar_limpa_janela_f346.py:99`; `app/ferias/test_remarcar_limpa_janela_f346.py:105`; `app/ferias/test_remarcar_limpa_janela_f346.py:108`; `app/ferias/test_remarcar_limpa_janela_f346.py:110`; `app/ferias/test_cancelar_2x.py:251`)

Cada asserção nova/alterada, contra `63ff6e88b`, pelo trecho anterior fornecido no delta:

| Asserção | Falharia no pai? |
|---|---|
| Quatro dias: `assertRaises` | **Não:** o recorte anterior já recusava quatro. (`app/ponto/tests/test_ferias_fracao_demais_5.py:135`) |
| Quatro dias: contém `art. 134` | **Não:** a mensagem anterior também continha isso. A troca de `5 dias` por esse trecho reduz a especificidade: agora a fração única de quatro cai primeiro na exigência de 14. (`app/ponto/tests/test_ferias_fracao_demais_5.py:137`; `app/ferias/models.py:380`; `app/ferias/models.py:387`) |
| Quatro dias: permanece 14 | **Não:** já havia recusa antes de escrever. (`app/ponto/tests/test_ferias_fracao_demais_5.py:139`) |
| Única 14→5: `assertRaises` | **Sim:** cinco passava no recorte anterior. (`app/ponto/tests/test_ferias_fracao_demais_5.py:144`) |
| Única 14→5: mensagem `14 dias` | Não seria alcançada, pois faltaria a exceção. (`app/ponto/tests/test_ferias_fracao_demais_5.py:146`) |
| Única 14→5: permanece 14 | Não seria alcançada; isoladamente falharia, pois seria gravado cinco. (`app/ponto/tests/test_ferias_fracao_demais_5.py:148`; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Dias 14/intervalo 15: `assertRaises` | **Sim:** o parâmetro 14 passava e não havia conferência do intervalo. (`app/ponto/tests/test_ferias_fracao_demais_5.py:153`) |
| Divergência: mensagem `intervalo real` | Não seria alcançada, pois faltaria a exceção. (`app/ponto/tests/test_ferias_fracao_demais_5.py:155`) |
| Divergência: `(14, fim14)` preservado | Não seria alcançada; isoladamente falharia pelo fim alterado para dia 15. (`app/ponto/tests/test_ferias_fracao_demais_5.py:157`; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Sucesso: dias 15 | **Não:** controle positivo que passa antes e depois. (`app/ponto/tests/test_ferias_fracao_demais_5.py:161`; `app/ponto/tests/test_ferias_fracao_demais_5.py:163`) |
| Correção: ausência de substring | **Não:** permanece inspeção textual do wrapper; não executa correção nem verifica aviso. (`app/ponto/tests/test_ferias_fracao_demais_5.py:169`) |
| Aviso contém §1 | **Não:** esse ramo já existia. (`app/ponto/tests/test_ferias_fracao_demais_5.py:184`; `app/ferias/situacao.py:228`) |
| Aviso contém caput | **Não:** o `if caput` já estava no pai deste delta. O teste melhora a prova comportamental da cura anterior. (`app/ponto/tests/test_ferias_fracao_demais_5.py:185`; `app/ferias/situacao.py:237`) |

**NÃO PROCEDE [bloqueia] — conjunto sem mordida:** há dois casos que distinguem este delta do pai. **PROCEDE [fila] — cobertura incompleta:** o selo administrativo continua textual, e os novos casos de remarcação não exercitam quatro frações nem quatro dias ao lado de uma irmã ≥14; este último existe apenas diretamente sobre `clean()`. (`app/ponto/tests/test_ferias_fracao_demais_5.py:144`; `app/ponto/tests/test_ferias_fracao_demais_5.py:153`; `app/ponto/tests/test_ferias_fracao_demais_5.py:169`; `app/ponto/tests/test_ferias_fracao_demais_5.py:128`; `app/ponto/tests/test_ferias_fracao_demais_5.py:67`)

**5. NÃO PROCEDE [bloqueia] — `ValidationError` engolida ou rebatizada pela view.**

As exceções anteriores não são superclasses de `ValidationError`: não remarcável deriva da exceção de domínio; saldo deriva de `Exception`. A captura final entrega `'; '.join(e.messages)`, preservando inclusive mensagens de coerência e colisão. O comentário “juiz do fracionamento” não vira texto de tela. (`app/ferias/services.py:555`; `app/ferias/services.py:576`; `app/ferias/services.py:586`; `app/ferias/views.py:725`; `app/ferias/views.py:727`; `app/ferias/views.py:729`; `app/ferias/views.py:730`)

A precedência efetiva passou a ser **status → início → saldo → `clean()` → colisão no `save()`**. Dentro do `clean()`, uma única fração de quatro dias recebe primeiro a mensagem da exigência de 14; com irmã ≥14, recebe a de cinco. Ambas vêm dos respectivos ramos do juiz. (`app/ferias/services.py:670`; `app/ferias/services.py:671`; `app/ferias/services.py:675`; `app

[truncado em 20000 de 22824 caracteres]

## 14/11 13:47 · sessao 5e3828cd3955 · - · - · origem=Ronald (tmux)

**Censo e recorte removido**

- **NÃO PROCEDE [bloqueia] — perda de julgamento de chamada regular antes coberta.** `remarcar_agendamento` sempre encaminha `so_nao_iniciado=True`; `_trocar_janela` atribui datas/dias e chama `vivo.clean()` antes de qualquer gravação. O `clean` julga a duração pelas datas e exige igualdade com `dias`; uma declaração 1–4 com intervalo ≥5 também termina recusada pela divergência. (`app/ferias/services.py:680`; `app/ferias/services.py:681`; `app/ferias/services.py:686`; `app/ferias/services.py:718`; `app/ferias/models.py:377`; `app/ferias/models.py:387`; `app/ferias/models.py:395`)
- **NÃO PROCEDE [bloqueia] — segundo escritor produtivo encontrado.** O registro permite `ferias/services.py`; o escritor material identificado é `_trocar_janela`, convergência das duas portas. As atribuições em `services.py:821` pertencem a `PeriodoAquisitivo`, não a `AgendamentoFerias`. (`app/core/portas.py:489`; `app/ferias/services.py:680`; `app/ferias/services.py:699`; `app/ferias/services.py:708`; `app/ferias/services.py:718`; `app/ferias/services.py:816`)
- **PROCEDE [fila] — correção administrativa continua permitindo fração <5.** O wrapper usa o padrão `so_nao_iniciado=False`; não chama o `clean`. Isso tem mandato explícito A3: corrigir o lançamento com aviso. A view exige dias positivos, deriva o fim, coleta os avisos e os acrescenta ao sucesso. Não é furo novo. (`app/ferias/services.py:657`; `app/ferias/services.py:681`; `app/ferias/services.py:708`; `app/ferias/views.py:503`; `app/ferias/views.py:507`; `app/ferias/views.py:508`; `app/ferias/views.py:517`; `app/ferias/views.py:552`)

| Caminho | Alteração de existente / tratamento de `<5` |
|---|---|
| `remarcar_agendamento` | Usa o escritor e `clean`; recusa nova fração curta. (`app/ferias/services.py:718`; `app/ferias/services.py:686`; `app/ferias/models.py:387`) |
| `corrigir_datas_agendamento` | Usa o mesmo escritor sem `clean`; aceita curto sujeito a saldo, status e colisão. Aviso é providenciado pela view. (`app/ferias/services.py:708`; `app/ferias/services.py:670`; `app/ferias/services.py:675`; `app/ferias/services.py:699`; `app/ferias/views.py:517`; `app/ferias/views.py:552`) |
| Django admin | Registro local somente de `Afastamento`; nenhuma edição de `AgendamentoFerias`. (`app/ferias/admin.py:2`; `app/ferias/admin.py:4`) |
| `sincronizar_status_ferias` | Encaminha para `alinhar_status_por_data`, que salva somente `status`; não reavalia duração curta. (`app/ferias/management/commands/sincronizar_status_ferias.py:48`; `app/ferias/services.py:928`; `app/ferias/services.py:929`) |
| `listar_ferias_dp` | Simula cancelamento e validação; rollback obrigatório. Não remarca. (`app/ferias/management/commands/listar_ferias_dp.py:100`; `app/ferias/management/commands/listar_ferias_dp.py:107`; `app/ferias/management/commands/listar_ferias_dp.py:109`; `app/ferias/management/commands/listar_ferias_dp.py:110`) |
| `alertar_retornos` | Consulta por fim/status e envia alerta; não muda agendamento. (`app/ferias/management/commands/alertar_retornos.py:20`; `app/ferias/management/commands/alertar_retornos.py:24`; `app/ferias/management/commands/alertar_retornos.py:45`) |
| API colaborador | POST 405; GET serializa o que está gravado. (`app/api/views.py:2223`; `app/api/views.py:2224`; `app/api/views.py:2244`; `app/api/views.py:2246`) |
| API mensageria | Consulta situação e avisos de pedido hipotético, sem escrita. (`app/api/views_mensageria.py:812`; `app/api/views_mensageria.py:824`; `app/api/views_mensageria.py:847`; `app/api/views_mensageria.py:849`) |

**Quem escreve e quem lê, para consolidar a tabela da raiz**

| Campo/valor | Escritor/montador | Leitores |
|---|---|---|
| `nova_ini`, `novos_dias`, `nova_fim` | Views leem POST e derivam fim. (`app/ferias/views.py:499`; `app/ferias/views.py:500`; `app/ferias/views.py:507`; `app/ferias/views.py:708`; `app/ferias/views.py:709`; `app/ferias/views.py:718`) | Escritor verifica saldo, atribui e passa a `clean`. (`app/ferias/services.py:676`; `app/ferias/services.py:679`; `app/ferias/services.py:680`; `app/ferias/services.py:686`) |
| `vivo.data_inicio`, `data_fim`, `dias` | `_trocar_janela`. (`app/ferias/services.py:680`; `app/ferias/services.py:699`) | `clean` calcula duração/confere dias; guarda de colisão; espelho; situação; APIs. (`app/ferias/models.py:377`; `app/ferias/models.py:395`; `app/ferias/models.py:450`; `app/ponto/services/ausencia.py:1185`; `app/ferias/situacao.py:122`; `app/api/views.py:2244`; `app/api/views.py:2246`) |
| `periodo.dias_concedidos` | Troca subtrai dias antigos e soma novos, depois salva. Criação e histórico também somam; cancelamentos subtraem. (`app/ferias/services.py:679`; `app/ferias/services.py:701`; `app/ferias/services.py:548`; `app/ferias/services.py:511`; `app/ferias/services.py:616`; `app/ferias/services.py:643`) | Juiz de saldo e saldo para remarcar; aplicação de status. (`app/ferias/models.py:220`; `app/ferias/models.py:225`; `app/ferias/services.py:700`) |
| `vivo.aviso_emitido` | Derivado da antecedência pelo escritor. (`app/ferias/services.py:687`; `app/ferias/services.py:699`) | View acrescenta aviso de antecedência; API mensageria entrega o flag. (`app/ferias/views.py:733`; `app/api/views_mensageria.py:845`) |
| `vivo.status` | `status_por_data` e escritor; cancelamentos; sincronização. (`app/ferias/services.py:691`; `app/ferias/services.py:699`; `app/ferias/services.py:619`; `app/ferias/services.py:651`; `app/ferias/services.py:928`) | Guardas sob trava, edição, seleção das frações, espelho e APIs. (`app/ferias/services.py:600`; `app/ferias/models.py:344`; `app/ferias/models.py:370`; `app/ponto/services/ausencia.py:1181`; `app/api/views.py:2247`) |
| Frações irmãs (`dias`, `status`, vínculo ao PA, `pk`) | Criações e portas acima; fixtures novas usam `bulk_create`. (`app/ferias/services.py:544`; `app/ferias/services.py:484`; `app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_cancelar_2x.py:247`) | `clean` filtra por PA/status e exclui a própria pk; situação repete a coleta. (`app/ferias/models.py:368`; `app/ferias/models.py:371`; `app/ferias/models.py:378`; `app/ferias/situacao.py:115`; `app/ferias/situacao.py:122`) |
| `caput`, `par1`, `par1_demais` | Helper e constantes 3/14/5. (`app/ferias/models.py:251`; `app/ferias/models.py:252`; `app/ferias/models.py:253`; `app/ferias/models.py:273`; `app/ferias/models.py:275`; `app/ferias/models.py:277`) | `clean` e avisos; não mais wrapper de remarcação. (`app/ferias/models.py:372`; `app/ferias/models.py:380`; `app/ferias/models.py:387`; `app/ferias/situacao.py:227`; `app/ferias/situacao.py:348`; `app/ferias/services.py:718`) |

**Classes solicitadas**

- **Relógio fora da borda: PROCEDE [fila], preexistente.** Situação e proposta mantêm fallback `date.today`; cancelamento administrativo lê a data para o carimbo. A alteração usa `hoje` explícito e não acrescenta leitura de relógio. (`app/ferias/situacao.py:156`; `app/ferias/situacao.py:326`; `app/ferias/services.py:649`; `app/ferias/services.py:671`; `app/ferias/services.py:687`; `app/ferias/services.py:691`; `app/ferias/services.py:700`)
- **Derivação paralela: NÃO PROCEDE [bloqueia] quanto ao recorte novo/removido; PROCEDE [fila] quanto à coleta duplicada antiga.** A duração da remarcação agora vem das datas no juiz e é conferida contra `dias`. Permanecem duas consultas montando as frações do período, no modelo e na situação. (`app/ferias/models.py:377`; `app/ferias/models.py:395`; `app/ferias/models.py:368`; `app/ferias/situacao.py:117`)
- **Campo com dois significados: NÃO PROCEDE [bloqueia] entre gozo e abono.** Dias vendidos são armazenados separadamente e entram separadamente no saldo. **PROCEDE [fila] divergência dias/datas ainda possível em chamada direta de correção**, que recebe ambos sem conferir equivalência; a view administrativa deriva o fim e evita esse caso. (`app/ferias/services.py:277`; `app/ferias/models.py:220`; `app/ferias/services.py:680`; `app/ferias/services.py:681`; `app/ferias/services.py:708`; `app/ferias/views.py:507`)
- **Save universal sem juiz: PROCEDE [fila], preexistente.** `save()` chama colisão e espelho, não `clean`; histórico explicitamente permite criação curta sem fracionamento. Não é alteração de linha existente nem abertura do delta. (`app/ferias/models.py:461`; `app/ferias/models.py:466`; `app/ferias/services.py:492`; `app/ferias/services.py:510`)

Outros escritores externos ou dinâmicos e efeitos em produção: **[nao verificado]**.

## 14/11 13:47 · sessao 76905d2e0dc4 · - · - · origem=Ronald (tmux)

**PROCEDE [fila] — fixtures passam a representar contadores diferentes da soma dos agendamentos.** F346 mantém `dias_concedidos=10` e acrescenta irmã concluída de 20 à linha de 10; a redução para 5 continua esperando contador 5, embora as linhas somem 25. O teste continua verificando limpeza e atualização incremental, mas não saldo agregado coerente. (`app/ferias/test_remarcar_limpa_janela_f346.py:28`; `app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_remarcar_limpa_janela_f346.py:80`; `app/ferias/test_remarcar_limpa_janela_f346.py:83`)

O teste da foto velha mantém contador 30 e linha de 30, acrescenta irmã de 20, remarca para 10 e espera contador 10: soma final das linhas 30. A adulteração da foto para 999 continua sendo exercitada; um cenário com irmã registrada e contador reconciliado não está coberto por esse teste. (`app/ferias/test_cancelar_2x.py:51`; `app/ferias/test_cancelar_2x.py:55`; `app/ferias/test_cancelar_2x.py:247`; `app/ferias/test_cancelar_2x.py:251`; `app/ferias/test_cancelar_2x.py:253`)

**NÃO PROCEDE [fila] — irmã esconde o veto do `save` no R06.** Com irmã20 e outro10, permanecem três frações; `clean` não verifica colisão, e o teste ainda pode alcançar o veto no `save`, após a limpeza. As asserções conferem restauração das ausências, contador e início. (`app/ferias/test_remarcar_limpa_janela_f346.py:35`; `app/ferias/test_remarcar_limpa_janela_f346.py:99`; `app/ferias/test_remarcar_limpa_janela_f346.py:105`; `app/ferias/test_remarcar_limpa_janela_f346.py:108`; `app/ferias/test_remarcar_limpa_janela_f346.py:110`; `app/ferias/models.py:363`; `app/ferias/models.py:399`; `app/ferias/services.py:694`; `app/ferias/services.py:699`)

Contrafactual contra 63ff, pelo delta fornecido:

| Asserção | Resultado no pai |
|---|---|
| Quatro dias: `assertRaises` | Passa: a guarda anterior já recusava quatro. (`app/ponto/tests/test_ferias_fracao_demais_5.py:135`; delta fornecido, remoção em `remarcar_agendamento`) |
| Quatro dias: contém `art. 134` | Passa: a mensagem anterior também continha esse trecho; a asserção foi ampliada de `5 dias` para `art. 134`. (`app/ponto/tests/test_ferias_fracao_demais_5.py:137`; delta fornecido) |
| Quatro dias: permanece14 | Passa: recusa anterior precedia escrita. (`app/ponto/tests/test_ferias_fracao_demais_5.py:139`; delta fornecido) |
| Única14→5: `assertRaises` | **Falha**: cinco não acionava o recorte anterior. (`app/ponto/tests/test_ferias_fracao_demais_5.py:144`; delta fornecido) |
| Única14→5: mensagem14 | Não alcançada após falha do contexto; falta a exceção que conteria a mensagem. (`app/ponto/tests/test_ferias_fracao_demais_5.py:146`) |
| Única14→5: permanece14 | Não alcançada; isoladamente falharia, pois o escritor gravaria5. (`app/ponto/tests/test_ferias_fracao_demais_5.py:148`; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Dias14/intervalo15: `assertRaises` | **Falha**: parâmetro14 passa na guarda anterior; sem `clean` o intervalo divergente é gravado. (`app/ponto/tests/test_ferias_fracao_demais_5.py:153`; delta fornecido; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Divergência: mensagem `intervalo real` | Não alcançada após falha do contexto. (`app/ponto/tests/test_ferias_fracao_demais_5.py:155`) |
| Divergência: `(14,fim14)` | Não alcançada; isoladamente falharia por fim15. (`app/ponto/tests/test_ferias_fracao_demais_5.py:157`; `app/ferias/services.py:680`; `app/ferias/services.py:699`) |
| Sucesso15: dias15 | Passa antes e depois; controle positivo, sem RED contra pai. (`app/ponto/tests/test_ferias_fracao_demais_5.py:161`; `app/ponto/tests/test_ferias_fracao_demais_5.py:163`) |
| Correção: ausência de substring | Passa antes e depois; não executa correção nem comprova aviso. (`app/ponto/tests/test_ferias_fracao_demais_5.py:165`; `app/ponto/tests/test_ferias_fracao_demais_5.py:169`; `app/ferias/services.py:706`) |
| Aviso: contém §1 | Passa antes e depois: o ramo `par1` já existia e não mudou neste delta. (`app/ponto/tests/test_ferias_fracao_demais_5.py:184`; `app/ferias/situacao.py:228`) |
| Aviso: contém caput | Passa antes e depois: o `if caput` também já existia no pai; o delta melhora o selo, substituindo inspeção textual por comportamento. (`app/ponto/tests/test_ferias_fracao_demais_5.py:185`; `app/ferias/situacao.py:237`; delta fornecido) |

**NÃO PROCEDE [fila] — conjunto novo sem mordida.** Existem dois casos comportamentais que distinguem o delta do pai: única fração reduzida a5 e divergência dias/intervalo. **PROCEDE [fila] — selo administrativo permanece fraco:** verifica apenas substring do wrapper. (`app/ponto/tests/test_ferias_fracao_demais_5.py:144`; `app/ponto/tests/test_ferias_fracao_demais_5.py:153`; `app/ponto/tests/test_ferias_fracao_demais_5.py:169`)

**PROCEDE [fila] — não há caso novo de remarcação exercitando `caput` com quatro frações nem fração4 com irmã≥14 para exigir especificamente a segunda metade.** A fixture de remarcação tem uma única14; os casos acrescentados são5, intervalo divergente e15. O caso específico quatro ao lado de14 existe no teste direto de `clean`, sem atravessar o serviço. (`app/ponto/tests/test_ferias_fracao_demais_5.py:67`; `app/ponto/tests/test_ferias_fracao_demais_5.py:128`; `app/ponto/tests/test_ferias_fracao_demais_5.py:141`; `app/ponto/tests/test_ferias_fracao_demais_5.py:150`; `app/ponto/tests/test_ferias_fracao_demais_5.py:159`)

Execução de suíte, RED real contra o pai e efeitos de produção: **[nao verificado]**. [nao verificado]

## 14/11 13:46 · sessao ae89bb590807 · - · - · origem=Ronald (tmux)

**PROCEDE fechamento do juiz parcial e da divergência dias/datas [bloqueia resolvido].** `vivo` é relido com `select_for_update`; o período também é travado e colocado no cache da relação (`app/ferias/services.py:599`, `:602`, `:603`). `_trocar_janela` atribui as novas datas e `dias` antes de `vivo.clean()` (`app/ferias/services.py:680`, `:686`). O `clean()` exclui o próprio PK e julga os irmãos vivos, além do intervalo novo do próprio objeto (`app/ferias/models.py:368`, `:371`, `:377`, `:379`). O status próprio ainda é o antigo; a derivação do novo só ocorre depois do julgamento, mas `clean()` não consulta `self.status` (`app/ferias/services.py:691`; `app/ferias/models.py:363`).

**NÃO PROCEDE dizer que `clean()` agora aplica sobreposição, período adquirido, saldo e abono.** O corpo termina em `app/ferias/models.py:399`: verifica máximo de três, alguma fração ≥14, fração escrita ≥5, ordem de datas e equivalência dias/intervalo (`app/ferias/models.py:372`, `:380`, `:387`, `:392`, `:396`). Não executa `full_clean()` nem validações de campos automaticamente: a chamada é diretamente `clean()` (`app/ferias/services.py:686`).

| Pergunta adicional | Local e efeito |
|---|---|
| Sobreposição | Continua no `save()`, depois de apagar o trecho antigo; atomic engloba ambos. O `save` chama `_guard_sobreposicao`, que delega à precedência e traduz seu veto em `ValidationError`. Faz sentido remarcando datas. (`app/ferias/services.py:669`, `:694`, `:699`; `app/ferias/models.py:450`, `:454`, `:463`) |
| Período | `clean` seleciona irmãos pelo período, mas não exige status adquirido nem novo intervalo dentro do período aquisitivo/concessivo. A guarda de sobreposição de PAs só dispara em escrita que muda datas do próprio PA; a remarcação salva somente concedidos/status. (`app/ferias/models.py:368`, `:363`, `:185`; `app/ferias/services.py:701`) |
| Saldo | Julgado separadamente, antes da mutação, com a reserva antiga devolvida ao cálculo. Não duplica contagem por chamar `clean`. (`app/ferias/services.py:675`, `:676`, `:679`; `app/ferias/models.py:222`, `:225`) |
| Abono | Remarcação não grava dias vendidos; saldo já os desconta. Não chama o teto `dias_vendaveis`, reservado ao pedido que inclui venda. (`app/ferias/models.py:220`, `:237`; `app/ferias/services.py:699`, `:701`, `:311`, `:312`, `:277`) |
| Datas | Ordem e equivalência são pertinentes ao intervalo remarcado. Fracionamento antecede a mensagem específica de datas invertidas, portanto outra mensagem pode ganhar precedência. (`app/ferias/models.py:380`, `:387`, `:392`, `:396`) |

**NÃO PROCEDE caminho regular que grave sem julgamento por ter removido o recorte.** Todo retorno do wrapper regular passa `so_nao_iniciado=True`; só há save depois do novo `clean` (`app/ferias/services.py:718`, `:681`, `:686`, `:699`). Chamadas antes recusadas pelo mínimo podem agora ser recusadas por status ou saldo primeiro; não gravam (`app/ferias/services.py:670`, `:671`, `:675`).

**Quando o `clean` não roda:** cancelado é recusado na trava antes dele; em_gozo/concluido, ou agendado com início já alcançado, são recusados pela guarda regular `pode_editar_em` (`app/ferias/services.py:600`, `:670`, `:671`; `app/ferias/models.py:313`, `:340`, `:344`). Na correção administrativa, o wrapper não passa o booleano e o default é falso, independentemente de status não cancelado; o `clean` é pulado por desenho A3 (`app/ferias/services.py:657`, `:681`, `:706`, `:708`). O próprio `clean` também retorna se falta FK/início/fim, porém o caminho com datas ausentes não demonstra gravação bem sucedida: usaria essas datas depois para aviso/status (`app/ferias/models.py:365`; `app/ferias/services.py:687`, `:691`).

**NÃO PROCEDE captura da mensagem por exceção anterior [bloqueia resolvido].** As classes são independentes: `AgendamentoNaoRemarcavel` herda `AgendamentoRecusado(Exception)`; `SaldoInsuficienteParaRemarcar` herda `Exception`; nenhuma captura uma `ValidationError` (`app/ferias/services.py:555`, `:576`, `:586`). A ordem da view é não remarcável → saldo → `ValidationError`, cuja lista de mensagens é entregue diretamente, unida por `; ` (`app/ferias/views.py:725`, `:727`, `:729`, `:730`). A prioridade efetiva do serviço é status → início → saldo → `clean` → colisão no save (`app/ferias/services.py:670`, `:671`, `:675`, `:686`, `:699`).

**Mensagem chega ao mecanismo de tela no código.** HTMX passa pelo `_resp` à seção, que põe o texto em `HX-Trigger/showToast/msg`; normal usa `resposta_acao`, que adiciona a mensagem Django antes do redirect (`app/ferias/views.py:697`, `:699`, `:700`, `:843`, `:857`; `app/core/respostas.py:68`, `:69`, `:71`). O template base entrega Django messages em `data-toast-msg` (`app/templates/base.html:334`). Execução real de navegador, suíte e produção: **[nao verificado]**.

**NÃO PROCEDE inversão de trava pelo novo clean.** A ordem material permanece agendamento → período → ausências; o `clean` lê irmãos com consulta comum, sem `select_for_update` adicional (`app/ferias/services.py:599`, `:602`, `:686`, `:694`; `app/ferias/models.py:368`). Concorrência real: **[nao verificado]**.

Observação de conferência: o decorator da view contém lambda com relação `periodo` incorreta, mas está `isento=True`, portanto esse lambda não é avaliado nesse caminho e não sustenta alegação de tela quebrada (`app/ferias/views.py:686`; `app/colaboradores/mixins.py:301`, `:302`).

## 14/11 13:00 · sessao 010db3e600ff · HJ-091 · verificador · origem=Code

**REPROVA.** Há uma quebra objetiva de contrato na árvore e um caminho que apaga a rejeitada vinculada, permitindo depois nascer aprovada para o mesmo dia. `app/escala/tests/test_contract_juiz_escala.py:358`; `app/ponto/services/ausencia.py:1344`; `app/ponto/services/ausencia.py:1277`.

1. **Aval: cura implementada, preservação posterior incompleta.**

| Exigência | Árvore e teste que a cobre | [nao verificado]
|---|---|
| Buscar férias sem origem, incluindo rejeitadas | Consulta sem filtro de status e carrega `status`: `app/ponto/services/ausencia.py:1512`. Teste: `app/ponto/tests/test_hj091_ferias_derivadas.py:891`. |
| Vincular pela janela do mesmo colaborador | Status, colaborador e duas pontas da janela são conferidos: `app/ponto/services/ausencia.py:1373`. |
| Não resolver duas janelas ou marca conflitante automaticamente | Recusas em `app/ponto/services/ausencia.py:1410` e `app/ponto/services/ausencia.py:1414`; testes em `app/ponto/tests/test_hj091_ferias_derivadas.py:907` e `app/ponto/tests/test_hj091_ferias_derivadas.py:926`. |
| Manter rejeitada e carimbo | A porta atualiza somente a FK: `app/ponto/services/ausencia.py:1459`. Asserções de status e carimbo: `app/ponto/tests/test_hj091_ferias_derivadas.py:899`. |
| Usar a porta nominal do PERMITIDOS | Apply chama a porta em `app/ponto/services/ausencia.py:1545`; autorização nominal em `app/ponto/tests/test_carimbo_acompanha_a_escrita.py:48`. |
| Manter trilha | Escrita da trilha dentro do `atomic`: `app/ponto/services/ausencia.py:1550`. Testes de conteúdo e falha: `app/ponto/tests/test_hj091_ferias_derivadas.py:786` e `app/ponto/tests/test_hj091_ferias_derivadas.py:832`. |
| Espelho imediato não criar nem reabrir | Existência pela FK interrompe a criação: `app/ponto/services/ausencia.py:1271`. Teste: `app/ponto/tests/test_hj091_ferias_derivadas.py:901`. |

**[fila] Ciência dos três dias na LISTA-DP não está demonstrada.** O comando imprime nessa lista somente `nao_resolvidas`; uma rejeitada vinculada com sucesso sai desse conjunto. A ciência por procedimento externo permanece **[nao verificado]**. `app/ponto/management/commands/vincular_ausencias_ferias.py:61`; `app/ponto/management/commands/vincular_ausencias_ferias.py:65`.

A classificação atual não contém lista nominal de pulo: decide por status, janela, colaborador e marca. `app/ponto/services/ausencia.py:1402`. No delta fornecido, não identifiquei alteração funcional alheia à cura; a totalidade do diff real entre commits é **[nao verificado]**, pois o histórico Git não está disponível nesta cópia.

A prova de sombra — zero nascidas, três PKs rejeitadas com origem 103, #5592 intacta e 16 vetos iguais — é **[nao verificado]**. Também são **[nao verificado]** a execução da suíte inteira, a posição na esteira, a revisão Opus, o pouso único e a competência exportada. [nao verificado]

2. **Escritores e leitores dos campos.**

| Campo | Quem escreve | Quem lê nos caminhos examinados |
|---|---|---|
| `Ausencia.agendamento_ferias` | Nascimento por `criar_ausencia`, via `kwargs`, em `app/ponto/services/ausencia.py:413`; persistência em `:446`. Backfill e reversão pela porta em `:1460`, chamada em `:1545` e `:1572`. | Mensagem `app/ponto/services/ausencia.py:85`; painel `:106`; busca de rejeitada própria `:312`; espelho `:1255`; apagador `:1341`; rejeição em massa `:1354`; backfill `:1513`; veto `app/ferias/models.py:431`; contador `app/ferias/services.py:149`. |
| `Ausencia.status` | Nascimento `app/ponto/services/ausencia.py:403`; transição genérica `:889`; rejeição de férias em massa `:1355`. | Busca de rejeitada `:309`; guarda/transição `:882`; contadores abertos `:1030`; reversão `:1167`; backfill novo `:1404`; cobertura folha/cobrança `app/ponto/turnos.py:2516`; contador HJ-091 `app/ferias/services.py:147`. |
| `Ausencia.atualizado_em` | `auto_now` do modelo em `app/ponto/models.py:824`; edição com save completo `app/ponto/services/ausencia.py:572`; período parcial `:960`; rejeição em massa `:1355`. Backfill deliberadamente não o escreve: `:1460`. | Fato novo `app/ponto/services/cartorio.py:383`; reversão H06 `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`; administração `app/ponto/admin.py:47`. |
| `AgendamentoFerias.status/data_inicio/data_fim` | Histórico `app/ferias/services.py:521`, persistido em `:547`; agendamento `:581`; aprovação de solicitação `:775`, persistida em `:786`; troca de datas `:716`, status em `:721`, save em `:729`; cancelamentos escrevem status em `:656` e `:688`; sincronização em `:955`. | Juiz de janela `app/ferias/services.py:53`; situação atual `:65`; divergência `:79`; contador HJ-091 `:150`; backfill `app/ponto/services/ausencia.py:1373`; espelho `:1251` e `:1266`; painel `:110`; guardas do modelo `app/ferias/models.py:335`; folha `app/folha/export.py:491`. |

**Há escrita de nascimento da FK fora de `_gravar_origem_de_ferias`: `criar_ausencia`.** Ela é explicitamente declarada como parte do espelho; a porta nominal é o único UPDATE do backfill e da reversão, não o único INSERT possível dessa FK. `app/ponto/models.py:791`; `app/ponto/services/ausencia.py:414`; `app/ponto/services/ausencia.py:1452`.

O selo AST executado em memória encontrou somente os escritores declarados; suas limitações incluem SQL cru, variáveis, parâmetros e subscrito de dict. Portanto isso não prova ausência de qualquer escrita dinâmica possível. `app/ponto/tests/selo_hj091.py:12`; `app/ponto/tests/selo_hj091.py:23`; `app/ponto/tests/selo_hj091.py:109`.

**[fila, herdada] `mudar_estado` salva sem incluir `atualizado_em`.** O campo é `auto_now`, mas a lista parcial não o contém; logo esse escritor não garante movimentar esse carimbo. Decisões também dispõem de `aprovada_em`, lido pelo cartório. `app/ponto/services/ausencia.py:888`; `app/ponto/services/ausencia.py:900`; `app/ponto/services/cartorio.py:385`.

A exaustividade de todos os leitores indiretos de `status` e `atualizado_em` na árvore inteira é **[nao verificado]**; o censo acima cobre a FK direta e os caminhos solicitados.

3. **Destino da rejeitada depois de vinculada.**

| Leitor/caminho | Efeito concreto |
|---|---|
| Espelho: criar | Reconhece a rejeitada pela FK e não cria outra naquele dia. `app/ponto/services/ausencia.py:1255`; `:1271`. |
| Espelho: atualizar mantendo o dia | Não promove nem reabre a rejeitada: simplesmente pula o dia existente. `app/ponto/services/ausencia.py:1271`. |
| Espelho: retirar o dia da janela | **Apaga a rejeitada**, pois a limpeza não filtra status. `app/ponto/services/ausencia.py:1284`. |
| Espelho: agendamento inativo | **Apaga todas as próprias**, incluindo rejeitadas. `app/ponto/services/ausencia.py:1260`. |
| `apagar_ausencias_de_ferias` | **Apaga pela FK independentemente do status**; a exceção preserva somente datas da nova janela. `app/ponto/services/ausencia.py:1341`. |
| Cancelamento DP | Salva `cancelado`; o modelo chama o espelho, que apaga. `app/ferias/services.py:656`; `app/ferias/models.py:448`; `app/ponto/services/ausencia.py:1262`. |
| Cancelamento admin | Não atualiza as já rejeitadas na etapa de rejeição, mas depois salva `cancelado` e igualmente as apaga. `app/ponto/services/ausencia.py:1355`; `app/ferias/services.py:688`. |
| Exclusão do agendamento | Chama o apagador; a FK também possui `CASCADE`. `app/ferias/models.py:452`; `app/ponto/models.py:797`. |
| Reabertura/aprovação genérica | Recusa férias antes da alteração de estado. `app/ponto/services/ausencia.py:881`; `:972`; `:1166`. |
| Veto na troca de datas | Exclui as próprias pela FK, incluindo essa rejeitada, do conjunto que pode vetar a alteração. `app/ferias/models.py:431`. |
| Contador HJ-091 | Não acusa a rejeitada no lado “ausência sem agendamento”; acusa seu dia no lado “dia sem ausência aprovada”. `app/ferias/services.py:147`; `:155`; `:162`. |

**[bloqueia] Contraexemplo de perda da decisão:** vincular a rejeitada D → corrigir datas excluindo D → corrigir novamente incluindo D. A primeira correção apaga sua PK; a segunda encontra o dia sem linha própria e materializa **nova aprovada**. Não é uma transição de status da antiga linha: é destruição seguida de novo nascimento. `app/ferias/services.py:725`; `app/ponto/services/ausencia.py:1344`; `app/ponto/services/ausencia.py:1271`; `app/ponto/services/ausencia.py:1277`.

**[fila] O mesmo dia continua férias para leitores do agendamento.** A rejeitada não cobre folha nem cobrança pela autoridade de Ausência; entretanto `em_gozo_hoje`, o bloqueio de ponto e a verificação de férias com batida consultam a janela do agendamento. Contraexemplo: D rejeitado, agendamento vivo cobrindo D e batida em D; o exportador ainda pode reter como `ferias_com_batida`. `app/ponto/turnos.py:2516`; `app/ferias/services.py:65`; `app/ponto/utils.py:89`; `app/folha/export.py:491`; `app/folha/export.py:878`.

As telas também podem apresentar essas férias como gozadas pelo status do agendamento, e o saldo concedido não desconta a rejeição diária. `app/templates/ferias/_secao_colab.html:49`; `app/templates/ferias/_secao_colab.html:140`; `app/templates/ferias/_secao_colab.html:168`; `app/ferias/services.py:105`.

4. **Janela e ESMERIL-SEMANTICA.**

As duas pontas são **inclusivas** no helper, no espelho, na limpeza com exceção, no contador e no juiz existente de janela; a duração é calculada com `+1`. Não encontrei divergência inclusivo/exclusivo nesses caminhos. `app/ponto/services/ausencia.py:1375`; `:1267`; `:1343`; `app/ferias/services.py:152`; `:161`; `:53`; `app/ferias/models.py:382`.

`STATUS_QUE_ESPELHAM = ('agendado', 'em_gozo', 'concluido')`. “Vivo” aqui inclui concluído; não significa somente férias atualmente em gozo. Os quatro status declarados deixam somente cancelado fora desse conjunto. `app/ferias/services.py:132`; `app/ferias/models.py:296`.

**[fila] Há duas formulações da pertinência:** o helper aceita uma lista positiva; `ferias_na_janela` exclui apenas cancelado. Coincidem nos quatro status declarados, mas um valor legado como `solicitado` seria incluído pelo segundo e recusado pelo primeiro. A existência desse valor em produção é **[nao verificado]**. `app/ponto/services/ausencia.py:1373`; `app/ferias/services.py:53`.

**[bloqueia] O predicado novo também quebra o contrato de censo.** O censo puro retornou:

```text
copias = ['ponto/services/ausencia.py::_janelas_vivas_do_dia']
COPIAS = {}
```

O helper é reconhecido pela comparação das duas datas; não consta nas exceções nominais. O teste exige ausência de nomes novos e igualdade com a lista congelada. `app/core/censo_pergunta2.py:87`; `app/core/censo_pergunta2.py:43`; `app/core/censo_pergunta2.py:115`; `app/escala/tests/test_contract_juiz_escala.py:358`; `app/escala/tests/test_contract_juiz_escala.py:363`.

5. **Travas e TOCTOU.**

O DRY e o apply usam o mesmo carregador e classificador, mas **não prometem a mesma fotografia**: o apply refaz sua leitura inicial e depois relê sob trava. Um DRY executado anteriormente não é um plano persistido para o apply. `app/ponto/services/ausencia.py:1513`; `:1515`; `:1528`; `:1535`.

A sequência implementada é agendamentos do DRY, com período pelo join, depois ausências. Agendamento já conhecido que muda status ou datas antes da trava é relido; os testes existentes exercitam cancelamento e remarcação nesse intervalo. `app/ponto/services/ausencia.py:1522`; `:1527`; `:1528`; `app/ponto/tests/test_hj091_ferias_derivadas.py:713`; `:721`.

Um agendamento novo elegível, ou uma nova marca, **visível na consulta de conferência**, provoca `FontesDoBackfillMudaram` antes de gravar. Ele não é travado tardiamente depois das ausências. `app/ponto/services/ausencia.py:1530`.

**Não encontrei caminho que vincule usando objeto fora de `travados`:** o classificador final recebe exclusivamente esse dicionário. Isso não prova que nenhuma janela concorrente possa nascer depois da última conferência. `app/ponto/services/ausencia.py:1535`; `:1543`.

**[fila] A janela de concorrência posterior à conferência permanece.** Contraexemplo a validar: após `faltam`, nasce outro agendamento para o mesmo colaborador, em outro período aquisitivo não travado, cobrindo D. O classificador final ainda conhece somente os objetos travados anteriormente. O código não toma trava pelo colaborador para proteger esse universo; a criação de histórico trava seu próprio período. A reprodução entre sessões e o isolamento efetivo são **[nao verificado]**. `app/ponto/services/ausencia.py:1531`; `:1535`; `app/ferias/services.py:519`; `:520`.

Garantia física da ordem das travas adquiridas pelo join e ausência de deadlock entre sessões: **[nao verificado]**. A ordem nominal declarada é agendamento → período → ausência. `app/ferias/services.py:633`; `app/ponto/services/ausencia.py:1522`.

6. **Os quatro testes novos mordem?** [nao verificado]

A comparação abaixo foi feita sobre a lógica pura extraída em memória; execução integral dos `TestCase` é **[nao verificado]**.

| Teste | Com a cura revertida | [nao verificado]
|---|---|
| Rejeitada dentro da janela | **Falha:** recebe `sem_marca`, sem vínculo. `app/ponto/tests/test_hj091_ferias_derivadas.py:897`; `app/ponto/services/ausencia.py:1419`. |
| Duas janelas vivas | **Falha:** recebe `sem_marca`, não `janela_ambigua`. `app/ponto/tests/test_hj091_ferias_derivadas.py:913`. |
| Somente cancelado | **Passa:** a descrição sem marca já recebia `sem_marca`. É regressão útil, mas não RED da cura. `app/ponto/tests/test_hj091_ferias_derivadas.py:885`; `:923`; `app/ponto/services/ausencia.py:1419`. |
| Marca de outro agendamento | **Falha:** recebe `fora_da_janela`, não `marca_x_janela`. `app/ponto/tests/test_hj091_ferias_derivadas.py:934`; `app/ponto/services/ausencia.py:1430`. |

Mutantes pedidos:

- **Ignorar status: morto.** As outras linhas mantêm a marca do cancelado, fazendo-o entrar no universo; sem o predicado de status, a rejeitada seria vinculada. `app/ponto/tests/test_hj091_ferias_derivadas.py:495`; `:887`; `:923`; `app/ponto/services/ausencia.py:1389`.
- **Trocar qualquer borda para exclusiva: morto.** O segundo agendamento é exatamente `DIA..DIA`; desaparece a ambiguidade exigida pelo teste. `app/ponto/tests/test_hj091_ferias_derivadas.py:911`; `:913`.
- **Ignorar marca conflitante: morto.** O quarto teste passa a vincular. `app/ponto/services/ausencia.py:1414`; `app/ponto/tests/test_hj091_ferias_derivadas.py:934`.
- **[fila] Ignorar colaborador: sobrevive aos quatro.** Todos usam o mesmo colaborador. Contraexemplo ausente: lote com rejeitadas de col1 e col2; somente a janela de col1 cobre D de col2. Sem a igualdade do helper, col2 ganha origem de col1. `app/ponto/services/ausencia.py:1374`; `:1392`; `app/ponto/tests/test_hj091_ferias_derivadas.py:38`; `:45`; `:50`.

Os testes novos não exercitam o ciclo “vincular → retirar dia → devolver dia” nem nascimento concorrente de janela; seus cenários terminam nas verificações indicadas. `app/ponto/tests/test_hj091_ferias_derivadas.py:891`; `:907`; `:917`; `:926`.

7. **Classes recorrentes da casa.**

- **[bloqueia] Derivação paralela/censo:** já existe `ferias_na_janela`, usado pelo juiz registrado `em_gozo_hoje`; o helper recompõe cobertura e colaborador. Além da questão de autoridade, há a falha objetiva do contrato apontada acima. `app/ferias/services.py:49`; `:65`; `app/core/juizes.py:262`; `app/ponto/services/ausencia.py:1373`.
- **Segundo UPDATE da origem fora da porta:** não encontrado pelo selo, dentro de suas limitações; nascimento possui escritor declarado. `app/ponto/tests/selo_hj091.py:12`; `:23`; `app/ponto/models.py:791`.
- **Relógio novo fora da borda:** não encontrado nos helpers novos; o comando fornece `quando`. Existem relógios internos herdados no cancelamento admin e nos fallbacks de leitura/status. `app/ponto/services/ausencia.py:1367`; `app/ponto/management/commands/vincular_ausencias_ferias.py:39`; `app/ferias/services.py:686`; `:64`; `app/ferias/models.py:283`.
- **[fila] Selo/RED incompleto:** um teste passa com a cura revertida e o mutante de colaborador sobrevive; o selo de escritores não verifica essa semântica. `app/ponto/tests/test_hj091_ferias_derivadas.py:923`; `app/ponto/tests/selo_hj091.py:72`.
- **Campo com dois significados:** a FK passa a representar também decisão humana rejeitada, mas os apagadores continuam tratando todas as linhas vinculadas como derivadas descartáveis. Esse é o contraexemplo bloqueante do item 3. `app/ponto/services/ausencia.py:1417`; `:1341`; `app/ponto/models.py:794`.
- **Pouso antes dos pré-requisitos:** sombra, suíte integral e sequência efetiva de pouso são **[nao verificado]**. [nao verificado]

8. **LEI-AKITA de `git log -1`.**

Origem, testemunha, RED, quem-mais-lê e padrão da linha do commit são **[nao verificado]**: esta cópia não oferece histórico Git consultável, e o texto dessa linha não foi fornecido. Não substituo isso pelo comentário dos testes. [nao verificado]

**REPROVA.**

## 14/11 12:58 · sessao 06748442c433 · - · - · origem=Ronald (tmux)

Achados e censo somente leitura:

- **[bloqueia, se preservação da rejeição abrange o ciclo posterior] A FK nova entrega a rejeitada aos apagadores existentes.** `espelhar_ferias` lê todas as próprias, qualquer status (`app/ponto/services/ausencia.py:1255`); preserva a rejeitada na criação/atualização enquanto o dia permanece na janela (`:1271`), porém apaga todas quando agendamento deixa de espelhar (`:1260`–`:1263`) e apaga as que saem da janela (`:1284`–`:1288`). `apagar_ausencias_de_ferias` também não protege rejeitadas (`:1341`–`:1344`). Contraexemplo: rejeitada D vinculada pelo backfill; cancelar seu agendamento ou corrigir datas excluindo D remove a PK rejeitada. O cancelamento DP salva `cancelado` (`app/ferias/services.py:656`–`:657`), cujo `save` espelha (`app/ferias/models.py:448`–`:449`). Cancelamento admin primeiro rejeita as demais (`app/ferias/services.py:683`), depois salva cancelado (`:688`–`:689`), igualmente apagando todas. Remarcação chama explicitamente o apagador fora da janela nova (`:725`). Exclusão do agendamento chama o apagador (`app/ferias/models.py:452`–`:454`), e a FK é `CASCADE` (`app/ponto/models.py:796`–`:799`).

- **[fila] O dia permanece férias nos leitores do agendamento apesar da ausência rejeitada.** Ausência rejeitada não cobre folha nem cobrança (`app/ponto/turnos.py:2516`–`:2519`), mas `ferias_na_janela` consulta exclusivamente datas do agendamento e exclui apenas cancelado (`app/ferias/services.py:49`–`:56`). `em_gozo_hoje` usa esse universo (`:65`), e o gate de ponto pergunta a ele (`app/ponto/utils.py:88`–`:89`). Contraexemplo concreto: D rejeitado, origem vinculada a agendamento vivo cobrindo D, batida em D: `dias_ferias_com_batida` ainda inclui D (`app/folha/export.py:491`–`:502`) e o classificador retém como `ferias_com_batida` (`:878`). A soma de dias concedidos também soma agendamentos não cancelados sem descontar dias rejeitados (`app/ferias/services.py:105`–`:106`). Isso não muda a linha para aprovada; são universos distintos.

Censo da FK em produção:

| Caminho | Leitura/escrita |
|---|---|
| Mensagem da linha | lê origem para mensagem `app/ponto/services/ausencia.py:85` |
| Painel | lê agendamento, datas, dias e status `app/ponto/services/ausencia.py:106`–`:111`; chamado pela view `app/ponto/views.py:2093`; renderização `app/templates/ponto/partials/painel_ausencia.html:135`–`:140` |
| Criação | lê FK ao procurar rejeitada própria `app/ponto/services/ausencia.py:312`–`:315`; escreve FK no nascimento por `kwargs` `:413`–`:414`, constrói `Ausencia` em `:446` |
| Espelho | lê próprias por FK `:1255`; passa FK ao produtor `:1280` |
| Exclusão | lê por FK e apaga `:1341`–`:1344` |
| Rejeição em massa | lê por FK, exclui já rejeitadas e escreve status/carimbo `:1354`–`:1355` |
| Backfill/reversão | lê nulas `:1513`, `:1528`; ambos escrevem pela porta `:1545`, `:1572`; único UPDATE nominal em `:1459`–`:1460` |
| Veto de remarcação | lê FK para excluir próprias do veto `app/ferias/models.py:431`–`:435` |
| Contador HJ-091 | consulta origem das não rejeitadas `app/ferias/services.py:147`–`:152`; cobertura exige aprovada `:155`–`:156`, faltantes contados em `:158`–`:163` |

Há escritor de **nascimento** fora `_gravar_origem_de_ferias`: `criar_ausencia`, explicitamente declarado no modelo (`app/ponto/models.py:791`–`:795`). Nenhum outro UPDATE/atribuição explícita da FK foi encontrado pelo censo textual e pelo selo AST; o selo declara que não vê SQL cru, variáveis, certos parâmetros e subscript de dict (`app/ponto/tests/selo_hj091.py:12`–`:15`).

Executei somente o selo AST puro em memória, sem Django, banco ou escrita de arquivo: encontrou `espelhar_ferias:1262/:1275/:1287`, `apagar_ausencias_de_ferias:1344`, `rejeitar_ausencias_de_ferias:1354`, `_gravar_origem_de_ferias:1459`; **fora de ESCRITORES = []**. Regras e universo do selo: `app/ponto/tests/selo_hj091.py:23`–`:29`, `:109`–`:125`.

Status/carimbo:

- Status nasce pelo produtor (`app/ponto/services/ausencia.py:403`), muda genericamente por `mudar_estado` (`:889`–`:900`) e em férias pela rejeição em massa (`:1355`).
- Portas genéricas recusam férias antes da decisão/status: `mudar_estado:881`, `decidir:972`, `reverter_rejeicao:1166`; edição recusa em `:536`; rederivação em `:1589`. Logo rejeitada vinculada não reabre nesses caminhos.
- `atualizado_em` é `auto_now` (`app/ponto/models.py:824`): criação e `editar_ausencia_registro.save()` completo (`app/ponto/services/ausencia.py:572`) o movem; período parcial o inclui explicitamente (`:960`, `:992`); rejeição em massa escreve `timezone.now()` (`:1355`); backfill só grava FK (`:1460`).
- `mudar_estado` salva lista sem `atualizado_em` (`:888`–`:900`); decisões ainda têm `aprovada_em`, também lido como fato novo (`app/ponto/services/cartorio.py:383`–`:385`).
- Leitores de carimbo: `houve_fato_novo` (`app/ponto/services/cartorio.py:383`–`:385`), proteção de reversão H06 (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`–`:159`), auditoria administrativa (`app/ponto/admin.py:47`).
- Folha usa `ausencias_do_periodo(..., para='folha')` (`app/ponto/services/fechamento.py:638`–`:639`) e exportação exige aprovada (`app/folha/export.py:87`–`:91`).
- Calendário pede cobertura cobrança (`app/colaboradores/services/calendario.py:484`–`:485`) e folha (`:532`–`:533`), portanto rejeitada não vira cobertura nesses mapas.
- Contadores DP selecionam somente estados abertos (`app/ponto/services/ausencia.py:1030`–`:1036`).
- Tela mantém botão “Reverter rejeição” para qualquer rejeitada, inclusive férias (`app/templates/ponto/partials/painel_ausencia.html:293`–`:307`), mas a porta recusa (`app/ponto/services/ausencia.py:1166`): **[fila] botão sem efeito permitido**, herdado.

A exaustividade de todos os leitores indiretos de `Ausencia.status` na árvore é **[nao verificado]**; os leitores de FK direta acima foram censados.

## 14/11 12:57 · sessao a474a29d30b9 · - · - · origem=Ronald (tmux)

Resultado da inspeção e da execução pura por AST, sem banco nem escrita de arquivos:

| Teste novo | Com cura revertida | Evidência | [nao verificado]
|---|---|---|
| Rejeitada dentro do agendamento | Falha: retorna `sem_marca`, falta vínculo | `app/ponto/tests/test_hj091_ferias_derivadas.py:897`; `app/ponto/services/ausencia.py:1419` |
| Duas janelas vivas | Falha: retorna `sem_marca`, esperado `janela_ambigua` | `app/ponto/tests/test_hj091_ferias_derivadas.py:913`; `app/ponto/services/ausencia.py:1410` |
| Somente agendamento cancelado | **Passa**: retorna `sem_marca` antes e depois | `app/ponto/tests/test_hj091_ferias_derivadas.py:923`; `app/ponto/services/ausencia.py:1419` |
| Marca de outro agendamento | Falha: retorna `fora_da_janela`, esperado `marca_x_janela` | `app/ponto/tests/test_hj091_ferias_derivadas.py:934`; `app/ponto/services/ausencia.py:1430` |

[**fila**] Um dos quatro testes novos não é RED da cura: a descrição da rejeitada não contém marca, então o código anterior já devolve exatamente `sem_marca`. É teste de preservação, não testemunha da implementação nova. `app/ponto/tests/test_hj091_ferias_derivadas.py:885`; `app/ponto/tests/test_hj091_ferias_derivadas.py:917`; `app/ponto/services/ausencia.py:1419`.

[**fila**] Mutante natural sobrevivente: remover a comparação de colaborador em `_janelas_vivas_do_dia`. Os quatro testes novos usam somente `self.c/self.pa`; a execução pura produziu as mesmas quatro respostas com esse predicado removido. Contraexemplo ausente: duas pessoas com rejeitadas sem origem entram no mesmo lote; uma janela de col1 cobre o dia da rejeitada de col2, mas nenhuma janela de col2 cobre esse dia. O mutante vincula a linha de col2 à origem de col1. `app/ponto/services/ausencia.py:1374`; `app/ponto/services/ausencia.py:1392`; `app/ponto/tests/test_hj091_ferias_derivadas.py:45`; `app/ponto/tests/test_hj091_ferias_derivadas.py:50`; `app/ponto/tests/test_hj091_ferias_derivadas.py:891`.

Os outros mutantes pedidos morrem:

- Ignorar status no helper: o teste cancelado passa a vincular a rejeitada. As outras quatro linhas da fixture mantêm a marca do agendamento cancelado, incluindo-o no universo mesmo após o cancelamento. `app/ponto/services/ausencia.py:1389`; `app/ponto/tests/test_hj091_ferias_derivadas.py:887`; `app/ponto/tests/test_hj091_ferias_derivadas.py:495`; `app/ponto/tests/test_hj091_ferias_derivadas.py:923`.
- Trocar qualquer borda `<=` por `<`: desaparece a segunda janela de um único dia; o teste esperava ambiguidade e passa a receber vínculo. `app/ponto/services/ausencia.py:1375`; `app/ponto/tests/test_hj091_ferias_derivadas.py:911`; `app/ponto/tests/test_hj091_ferias_derivadas.py:913`.
- Ignorar a marca conflitante: o quarto teste passa a vincular em vez de devolver `marca_x_janela`. `app/ponto/services/ausencia.py:1414`; `app/ponto/tests/test_hj091_ferias_derivadas.py:932`; `app/ponto/tests/test_hj091_ferias_derivadas.py:934`.

O selo cobre formas sintáticas de escrita e lista de escritores, não a semântica de colaborador/janela/status; esses mutantes não são sua jurisdição. Suas limitações declaradas incluem tipo por variável, SQL cru, objeto recebido por parâmetro e FK por subscrito de dict. `app/ponto/tests/selo_hj091.py:12`; `app/ponto/tests/selo_hj091.py:23`; `app/ponto/tests/test_hj091_ferias_derivadas.py:399`.

Há autoridade existente para férias por janela: `ferias_na_janela` usa interseção inclusiva e exclui cancelados; `em_gozo_hoje` delega a ela e consta no registro vivo. O helper novo repete as comparações de janela/colaborador, mas restringe ao conjunto materializável. A diferença de status é concreta: `solicitado` entra em `ferias_na_janela` e fica fora do helper. Classificar isso como duplicação proibida exige considerar o censo/contrato que você está verificando. `app/ferias/services.py:49`; `app/ferias/services.py:53`; `app/ferias/services.py:65`; `app/ferias/services.py:132`; `app/core/juizes.py:262`; `app/ponto/services/ausencia.py:1373`.

Execução integral dos `TestCase`, efeitos reais do banco e prova de sombra: **[nao verificado]**.

## 14/11 12:57 · sessao 2cb923b51ad2 · - · - · origem=Ronald (tmux)

[**bloqueia**] O selo existente reprova o helper novo: `app/core/censo_pergunta2.py:87-96` reconhece `_janelas_vivas_do_dia`; `copias()` desconta apenas os nomes registrados (`:115-117`), cuja lista não o inclui (`:43-60`). Execução somente leitura com `python3 -B`: `copias=['ponto/services/ausencia.py::_janelas_vivas_do_dia']`, esperado `[]`. Logo falha `app/escala/tests/test_contract_juiz_escala.py:363`.

[**bloqueia**, se o contrato exige preservar a rejeição nos leitores seguintes] A rejeitada vinculada pode ser apagada e posteriormente substituída por aprovada: vincular → corrigir datas excluindo o dia → corrigir datas incluindo novamente o dia. A primeira correção apaga pela FK sem filtrar status (`app/ferias/services.py:725`; `app/ponto/services/ausencia.py:1341-1344`); a volta materializa aprovada porque não há mais linha (`app/ponto/services/ausencia.py:1271-1280`). Também a apagam cancelamento DP (`app/ferias/services.py:656-657` → `app/ferias/models.py:448-449` → `app/ponto/services/ausencia.py:1260-1263`), cancelamento admin (`app/ferias/services.py:683-689`) e exclusão do agendamento (`app/ferias/models.py:451-454`; FK `CASCADE` em `app/ponto/models.py:796-797`).

[**fila**] Cobertura da Ausencia rejeitada e leitura do agendamento continuam dando respostas diferentes. A rejeitada não cobre folha nem cobrança (`app/ponto/turnos.py:2516-2519`); o contador acusa seu dia como sem ausência aprovada (`app/ferias/services.py:155-163`). Entretanto folha, situação e telas continuam lendo a janela inteira do agendamento: folha cruza batidas inclusive no dia rejeitado (`app/folha/export.py:491-501`), `em_gozo_hoje` consulta apenas agendamento (`app/ferias/services.py:59-70`), API mostra status/datas sem consultar Ausencia (`app/api/views.py:2237-2253`), e tela chama concluído de “gozadas” (`app/templates/ferias/_secao_colab.html:140-141`, `:168`), com `dias_concedidos` como “Gozo” (`:49`). Uma rejeitada vinculada não reduz `dias_concedidos` pelo backfill (`app/ponto/services/ausencia.py:1459-1460`).

Escritores de `AgendamentoFerias.status/data_inicio/data_fim`, todos em `app/ferias/services.py`:

- Gozo histórico: constrói datas/status `:521-527`, salva `:547`.
- Agendamento: `objects.create` com datas/status `:581-584`.
- Aprovação de solicitação: constrói `:775-783`, salva `:786`.
- Cancelamento DP: status `:656`, salva `:657`.
- Cancelamento admin: status `:688`, salva `:689`.
- Troca de janela existente: datas `:716`, status `:721`, salva `:729`; entradas públicas `corrigir_datas_agendamento :736-738` e `remarcar_agendamento :741-745`.
- Cron: escritor `alinhar_status_por_data :952-956`; chamado por `app/ferias/management/commands/sincronizar_status_ferias.py:48-49`.
- Default do modelo: `app/ferias/models.py:319`; persistência e espelho em `:444-449`.

A janela é inclusiva nas duas pontas: helper novo (`app/ponto/services/ausencia.py:1375`), espelho (`:1266-1269`), limpeza com exceção (`:1343`), contador (`app/ferias/services.py:152`, `:161`), juiz existente `ferias_na_janela` (`:49-55`), cálculo de dias (`app/ferias/models.py:371`, `:382`) e escritor (`app/ferias/services.py:565`). A consulta de batidas usa fim exclusivo no primeiro instante do dia seguinte, equivalente ao último dia inclusivo (`app/ferias/situacao.py:273-281`). `STATUS_QUE_ESPELHAM=('agendado','em_gozo','concluido')` (`app/ferias/services.py:132`); corresponde aos status declarados exceto cancelado (`app/ferias/models.py:296-301`).

Já existe autoridade de janela/dia em `ferias_na_janela` (`app/ferias/services.py:49-56`), usada por `em_gozo_hoje :65`, esta registrada em `app/core/juizes.py:262`. O helper repete a comparação de cobertura e colaborador (`app/ponto/services/ausencia.py:1372-1375`); nos quatro status declarados, os resultados de pertinência da janela coincidem, embora `ferias_na_janela` exclua apenas cancelado e o helper aceite uma lista positiva (`app/ferias/services.py:53`; `app/ponto/services/ausencia.py:1373`).

Há relógios internos preexistentes fora da borda: cancelamento admin lê `date.today()` para carimbo (`app/ferias/services.py:685-686`); `status_por_data` admite fallback (`app/ferias/models.py:283`), assim como `em_gozo_hoje` (`app/ferias/services.py:64`). O helper novo não lê relógio (`app/ponto/services/ausencia.py:1367-1375`).

Não executei suíte Django nem consultei banco/logs. Resultado de produção e prova de sombra: [nao verificado]. [nao verificado]

## 14/11 12:50 · sessao aaf08d3738b9 · E3-FORMA-SQL · construtor · origem=Code

**Pergunta ao topo:** o recuo deve preservar especificamente C17 nos consumidores migrados? No main, esses consumidores usam C17, mas a API `tarefa_do_dia` já devolve outro fio. O instrumento de sombra distingue expressamente “atual” de “candidata” (`folha/fora_da_folha.py@ac431c506:65–78`; `chamados/management/commands/diff_e3_sombra.py@ac431c506:124–127`).

Pela sua cláusula de divergência no main, deixei os quatro estados sem cura enquanto essa referência permanece pendente.

As referências `@ac431c506` abaixo são do segundo pai, lidas com `git show`.

| Estado | E3 v9 | Main: C17 / tarefa | Autoridades no main | Após esta volta |
|---|---|---|---|---|
| P13: responder; verbo validar | colab (`chamados/tests/test_e3_portas_v6_sem_banco.py:20`) | **admin / colab** | C17: `chamados/juizes.py@ac431c506:1608–1619,1668`; tarefa: `chamados/services/tarefa_do_dia.py@ac431c506:550–554` | Sem cura; colab |
| P14: validar; verbo cobrar | admin (`chamados/tests/test_e3_portas_v6_sem_banco.py:26`) | **colab / admin** | Mesmas autoridades acima | Sem cura; admin |
| P19: residual decidir; ação admin sob B/cobrar | admin (`chamados/tests/test_e3_portas_v6_sem_banco.py:31`) | **colab / admin** | Mesmas autoridades acima | Sem cura; admin |
| Simultâneo colab+admin; verbo validar | colab (`chamados/tests/test_e3_portas_tarefa.py:225`) | **admin / colab** | Mesmas autoridades acima | Sem cura; colab |

O simultâneo já retorna **colab na tarefa do main**; portanto a v9 coincide com essa API, mas diverge de C17 quando o único verbo publicado é validar (`chamados/services/tarefa_do_dia.py@ac431c506:550–552`; `chamados/juizes.py@ac431c506:1608`).

Quanto a portas e verbos:

- **P13/P14/simultâneo sem residual:** ambos publicam PF quando a porta pertence à célula e PP quando apenas a âncora externa abre a porta; preservam o verbo recebido (`chamados/services/tarefa_do_dia.py@ac431c506:546–554`; `chamados/juizes.py@ac431c506:1657–1668`).
- **P19 com A residual da célula e B externo:** C17 publica **PF**, `fila=(A,)`, verbos `{A: decidir, B: cobrar}`. A tarefa do main publica **PP**, `fila=()`, somente `{B: cobrar}`: ela seleciona portas pelas ações (`chamados/juizes.py@ac431c506:1657–1668`; `chamados/services/tarefa_do_dia.py@ac431c506:545–554`).

Quem lê na árvore atual:

| Leitor | Uso |
|---|---|
| Folha | Recebe a porta e devolve seu fio: `folha/fora_da_folha.py:74,89` |
| Vigia | Recebe a porta e conta admin pelo fio: `chamados/services/vigia_fio.py:648,662` |
| `furo_sem_canal` | Usa existência/rota e `nao_sei`: `chamados/services/furo_sem_canal.py:97–104` |
| Painel | Fornece fila e partição dos verbos: `chamados/services/painel.py:188–190,229–230` |

Executei o controle existente: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_portas_v6_sem_banco -v
```

Resultado local: **10 testes passaram**, incluindo as expectativas E3 ainda presentes (`chamados/tests/test_e3_portas_v6_sem_banco.py:16–42`); certificação pela suite **[não verificado]**.

Também comparei os corpos reais do main e da v9 por AST, em memória: oito combinações sintéticas confirmaram as divergências acima; alcance ORM **[não verificado]**.

**Arquivos tocados: nenhum**, incluindo os seis congelados; `git diff --name-only` e `git status --short` terminaram vazios **[não verificado]**. Não produzi cura, RED novo nem mutantes, para não fixar uma das respostas antes da definição solicitada. Permanecem incompletos esses três entregáveis; ocorrência em produção, contadores reais e execução com banco **[não verificado]**.

## 14/11 12:27 · sessao 812f3f6bae91 · COERENCIA-PELO-EFEITO · censo · origem=Code

A tabela conta **100 decisões identificadas**, voltando da cobrança exibida até batida, cadastro e célula. Ocorrência, frequência e resultado desses caminhos em produção: **[não verificado]**.

`SEM JUIZ` significa ausência de autoridade registrada para aquela decisão; chamar outra autoridade como componente não registra a composição. O registro consultado é `JUIZES`, inclusive seus acréscimos (`app/core/juizes.py:182`; `app/core/juizes.py:277`; `app/core/juizes.py:1310`). `SEM CASO` significa caso pela regra não confirmado nesta leitura; inexistência global de teste **[não verificado]**. Nos casos indicados, considerei cenário com resposta esperada escrita, sem contar somente AST, snapshot ou paridade.

| passo | decisão | arquivo::função:linha | grava em | relido por | juiz | caso pela regra |
|---|---|---|---|---|---|---|
| 01 — efeito | Pergunta viva fica pendente somente sem resposta e com disputa aberta; demais vivas vão ao histórico. | `app/chamados/services/pergunta_no_app.py::consulta_do_app:70` | — | API: `app/api/views.py:2531` | SEM JUIZ para a partição | SEM CASO |
| 02 — efeito | Havendo pendentes, badge “Responder DP” precede aviso de notas não lidas. | `app/chamados/services/pergunta_no_app.py::projecao_do_app:90` | — | App e painel recebem projeção em `app/chamados/services/pergunta_no_app.py:107` | SEM JUIZ | SEM CASO |
| 03 — efeito | Rótulo usa validação, C6 e materialização; mesmo com C6 falso, sem materialização retorna pendente/respondida. | `app/chamados/models.py::PerguntaDisputa.estado:3097` | — | API: `app/api/views.py:2543` | SEM JUIZ para essa tradução | SEM CASO |
| 04 — admin | Crédito usa `q_vivas`, exclui materializada com cache `sim`; destino vivo vence pai. Não confere C6 ao final. | `app/chamados/services/raiox.py::creditos_perguntas:56`; `:135` | — | Verbos: `app/chamados/services/verbos.py:34` | SEM JUIZ para o crédito | SEM CASO |
| 05 — admin | Crédito determina verbo; silenciamento transforma-o em arquivado ou histórico. | `app/chamados/services/verbos.py::particionar_verbos:34` | — | Badge lê placar ou recalcula: `app/core/context_processors.py:68` | SEM JUIZ para a partição | SEM CASO |
| 06 — admin | Detalhe considera disputa aberta e perguntas C6 vivas; “todas validadas” significa haver perguntas e nenhuma viva. | `app/chamados/services/acoes_chamado.py::montar_contexto_detalhe:345`; `:398` | — | Formulários validar/recusar: `app/templates/chamados/partials/_pergunta_card.html:70`; `:112` | SEM JUIZ para essa equivalência | SEM CASO |
| 07 — relatório | Sem perguntas → sem pergunta; nenhuma viva → pronta para fechar; viva muda → aguarda colaborador; restante → supervisão. | `app/chamados/relatorios_disputa.py::classificar:12` | — | Classificação devolvida em `app/chamados/relatorios_disputa.py:23` | SEM JUIZ para a taxonomia | SEM CASO |
| 08 — pauta | “Calados” usa `q_vivas`, resposta nula e disputa aberta, sem filtro C6; agrega prazo vencido por pessoa. | `app/chamados/services/cobranca.py::_calados:228` | — | Frase “pergunta segue aberta”: `app/chamados/services/cobranca.py:262` | SEM JUIZ para o conjunto; prazo chama `_C9` | SEM CASO |
| 09 — vigia | Qualquer pergunta da célula com resposta nula conta como aberta, sem conferir validação, materialização ou disputa. | `app/chamados/services/furo_sem_canal.py::medir:80` | — | Contadores `total` e `com_pergunta_aberta`: `app/chamados/services/furo_sem_canal.py:89` | SEM JUIZ | SEM CASO |
| 10 — prazo | SLA local nasce da última nota DP ou criação, mais horas da urgência; somente `COBRANCA` vencida entra. | `app/chamados/services/sla.py::referencia:19`; `::vence_em:30`; `::vencido:37` | — | Resultado de vencimento: `app/chamados/services/sla.py:39` | SEM JUIZ para esse predicado; `_C9` registra outra função (`app/core/juizes.py:285`) | SEM CASO |
| 11 — cobrança | Nova cobrança exige pendentes do app e nenhum pai com `prazo_sla > agora`. | `app/chamados/services/cobranca.py::checar_cobranca:82` | — | Porta recusa em `app/chamados/services/cobranca.py:155` | SEM JUIZ | SEM CASO |
| 12 — cobrança | Prazo informado vence o padrão; sem ele, usa agora +48h. Grava antes do envio. | `app/chamados/services/cobranca.py::cobrar_colaborador:170` | `Chamado.prazo_sla`, pela porta `definir_prazo_sla`; ocorrência: `:178` | Pauta e aviso: `app/chamados/services/cobranca.py:181`; `:246` | SEM JUIZ para escolha/48h | SEM CASO |
| 13 — cron | Toda disputa aberta recebe alerta diário se o contexto ainda não contém o dia; não consulta pergunta nem C6. | `app/ponto/management/commands/alertar_disputas_pendentes.py::Command.handle:33`; `:48` | `Disputa.contexto`, via `renovar_contexto` | Push “Situação pendente”: `app/ponto/management/commands/alertar_disputas_pendentes.py:59` | SEM JUIZ | SEM CASO |
| 14 — resposta HTML | Para `hora` com HH:MM, exige `resposta_tocou=1` antes de aceitar o envio. | `app/chamados/views.py::responder_pergunta_disputa:1274` | Não grava se recusar; delega ao core em `:1285` | API também consulta C6, mas sua entrada está em `app/api/views.py:2605` | SEM JUIZ para a guarda HTML | SEM CASO |
| 15 — resposta | Recusa vazio, opção inválida e formato incompatível com o tipo; aceita conforme opções e expressões do corpo. | `app/chamados/services/disputa_emissao.py::responder_pergunta_disputa_core:1432` | `resposta_colab`, `respondida_em`: `:1467`; ato `RESPOSTA`: `:1511` | C6: `app/chamados/juizes.py:1417`; materializador: `app/chamados/services/materializacao.py:1375` | SEM JUIZ | SEM CASO |
| 16 — resposta | Resposta nova desfaz validação anterior; carimbo sem lastro pode ser removido. | `app/chamados/services/disputa_emissao.py::responder_pergunta_disputa_core:1471` | Limpa `validada_em/por`; chama `reabrir_por_resposta`: `:1485` | Fila validada: `app/chamados/services/materializacao.py:1375` | SEM JUIZ para essa invalidação | SEM CASO |
| 17 — resposta especial | `FOLGA_CONTESTA` deixa motivo para admin; escapes catalogados seguem para ausência. | `app/chamados/services/disputa_emissao.py::responder_pergunta_disputa_core:1486`; `:1538` | `recusa_motivo`; ausência pela porta de materialização | Aceite de folga: `app/chamados/services/validacao.py:257`; ausência: `app/chamados/services/materializacao.py:502` | SEM JUIZ para o roteamento | SEM CASO |
| 18 — escritor auxiliar | Resposta automática escreve somente se ainda não houver texto. | `app/chamados/services/disputa_emissao.py::responder_automaticamente:1647` | `resposta_colab/respondida_em` e ato `RESPOSTA`: `:1652` | C6: `app/chamados/juizes.py:1417` | SEM JUIZ | SEM CASO |
| 19 — escritor auxiliar | Texto existente, sem carimbo de resposta, recebe o instante testemunhado pela trilha. | `app/chamados/services/disputa_emissao.py::carimbar_resposta_pela_trilha:1670` | `respondida_em` e ato `RESPOSTA`: `:1674` | Materializador: `app/chamados/services/materializacao.py:1376` | SEM JUIZ | SEM CASO |
| 20 — reabertura | Correção limpa resposta e validação; com `desfazer_absorcao`, também remove materialização. | `app/chamados/services/disputa_emissao.py::reabrir_para_correcao:1593` | Foto/ato: `:1615`; `descarimbar_em_lote` ou `update`: `:1630` | C6: `app/chamados/juizes.py:1399`; evento da célula: `app/chamados/services/disputa_emissao.py:1633` | SEM JUIZ para o conjunto apagado | SEM CASO |
| 21 — validação | Exige resposta, ausência de validação e confirmação quando a hora é considerada implausível. | `app/chamados/services/validacao.py::validar_pergunta:417` | `_carimbar_validacao` grava `validada_em/por` e ato: `:157` | Materialização: `app/chamados/services/materializacao.py:1375` | SEM JUIZ | SEM CASO |
| 22 — validação | Delta desconhecido não classifica; AM/PM ou delta ≥ limiar vira C; até tolerância vira A; restante B. | `app/chamados/juizes.py::classe_por_delta:1835` | — | Concordância: `app/chamados/juizes.py:1373`; validação: `app/chamados/juizes.py:1952` | SEM JUIZ para essa classificação no registro | SEM CASO |
| 23 — folga | Aceite exige dia e decisão da porta de folga; recusa da declaração desfaz o ato. | `app/chamados/services/validacao.py::_aceitar_a_folga:293`; `:328` | Validação, declaração de folga e carimbo admin: `:311`; `:343` | Fecho do fio: `app/chamados/services/validacao.py:318` | SEM JUIZ para o aceite composto | SEM CASO |
| 24 — recusa | Admin precisa informar motivo; pergunta deve estar respondida e não validada. | `app/chamados/services/validacao.py::recusar_pergunta:588` | `recusa_motivo`; reabre questionário: `:611`; `:620` | Nota e push para responder novamente: `app/chamados/services/validacao.py:625`; `:639` | SEM JUIZ | SEM CASO |
| 25 — data | Conferência exige justificativa, divergência e ausência de conferência anterior. | `app/chamados/services/validacao.py::conferir_data_do_fato:681` | `data_conferida_em/por`: `:691` | Guarda de materialização: `app/chamados/services/materializacao.py:1199` | SEM JUIZ | SEM CASO |
| 26 — materialização | Divergência não conferida barra plantio; sucesso exige resultado do ramo e, para hora/data-hora, testemunha externa. | `app/chamados/services/materializacao.py::_materializar_uma:1199`; `:1227`; `:1238` | Recusa ou carimbo; reprovação levanta dentro da transação: `:1245`; carimbo: `:1283` | Devolução de validação: `app/chamados/services/validacao.py:213` | SEM JUIZ para o conjunto de gates | SEM CASO |
| 27 — plantio de hora | Sem texto não planta; deriva tipo/data/hora, recusa escape/default sem marco, reutiliza batida ou verifica vizinhança/paridade. Intervalo pode usar hora do marco. | `app/chamados/services/materializacao.py::_ts_tipo_do_plantio:344`; `:360`; `:373`; `:390`; `:413`; `::_materializar_resposta_hora:835`; `:865`; `:881` | `pendencia_admin/recusa_motivo`: `:856`; batida via `registrar_batida`: `:933`; origem: `:952` | Juiz de testemunha: `app/chamados/juizes.py:752` | SEM JUIZ para essa derivação/seleção | SEM CASO |
| 28 — plantio data-hora | Data/hora legíveis geram entrada; conflito impede sucesso; destino recebe informação para cadastro da fase. | `app/chamados/services/materializacao.py::_materializar_resposta_data_hora:1001` | Batida `E`: `:1013`; ocorrência: `:1039` | Reconciliação: `app/chamados/services/materializacao.py:1065` | SEM JUIZ para esse resultado | SEM CASO |
| 29 — escolha | Escolha validada resolve sem batida; `declara_falta` passa pela declaração de ausência. | `app/chamados/services/materializacao.py::_materializar_resposta_escolha:1082` | Ausência, quando aplicável; carimbo final pela unidade em `:1283` | Destino/clones: `app/chamados/services/materializacao.py:1085` | SEM JUIZ | SEM CASO |
| 30 — absorção | Declaração de ausência alcança irmãs de hora do mesmo dia ainda não materializadas; C6 pode recusar o carimbo. | `app/chamados/services/materializacao.py::_materializar_ausencia_de_resposta:538`; `:567` | Ausência, vias `declaracao/ausencia_dia`, ato `ABSORCAO`: `:569` | Disputa consulta fechamento: `app/chamados/services/materializacao.py:591` | SEM JUIZ para a seleção das irmãs; usa `_C6` | SEM CASO |
| 31 — ausência rejeitada | Devolve absorvidas somente quando outra ausência não cobre o dia para cobrança. | `app/chamados/services/materializacao.py::devolver_absorvidas_da_ausencia_rejeitada:662`; `:669` | Correção com descarimbo: `:688`; reabre disputa: `:692` | App exige disputa aberta: `app/chamados/services/pergunta_no_app.py:71` | SEM JUIZ para a devolução composta | SEM CASO |
| 32 — colisão | Recusa traduzida como colisão, sem materialização, encerra por identidade; outras recusas só gravam motivo. | `app/chamados/services/materializacao.py::_carimbar_recusa:1097` | `fato_ocupado` via carimbo ou `recusa_motivo`: `:1105` | Nota no fio: `app/chamados/services/materializacao.py:1113` | SEM JUIZ para essa escolha | SEM CASO |
| 33 — devolução ao admin | Hora declarada, sem materialização e com recusa escrita perde validação; disputa fechada pode reabrir. | `app/chamados/services/validacao.py::devolver_validacao_recusada:213` | Limpa validação, ato `DEVOLUCAO`, reabertura: `:222`; `:228`; `:240` | Retorno da validação: `app/chamados/services/validacao.py:509` | SEM JUIZ | SEM CASO |
| 34 — carimbo | Aplica carimbo em memória e pergunta ao C6; se continuar viva, desfaz e não grava. | `app/chamados/models.py::PerguntaDisputa.carimbar:3175`; `:3185`; `:3195` | `materializada_em/por/via`; opcionalmente resposta/validação/cache | C6 e estado: `app/chamados/juizes.py:1399`; `app/chamados/models.py:3097` | `_C6` — `app/core/juizes.py:282` | SEM CASO |
| 35 — identidade | Via deve pertencer à lista de identidade; linha fora do fato não retorna por outra via. Não consulta C6 para gravar. | `app/chamados/models.py::PerguntaDisputa.carimbar_identidade:3236` | Carimbo e eventual `recusa_motivo`: `:3242` | C6 interpreta a via: `app/chamados/juizes.py:1412`; `:1438` | SEM JUIZ para admitir esse ato | SEM CASO |
| 36 — descarimbo | Linha fora do fato não é descarimbada; lote também exclui essas vias. | `app/chamados/models.py::PerguntaDisputa.descarimbar:3259`; `::descarimbar_em_lote:3320` | Limpa materialização, autor e via; lote aceita campos adicionais | C6: `app/chamados/juizes.py:1399` | SEM JUIZ para a elegibilidade da escrita | SEM CASO |
| 37 — relavra da via | Somente vias derivadas mudam; hora declarada vira declaração; sem destino ou conversa não deriva via pelo turno. | `app/chamados/services/relavra_via.py::via_de_hoje:97`; `::relavrar:118` | `via_resolucao` e cache: `:125` | C6 e crédito: `app/chamados/juizes.py:1399`; `app/chamados/services/raiox.py:135` | SEM JUIZ para a substituição | SEM CASO |
| 38 — reinterpretação | Carimbo elegível é relido sob trava e removido se C6 disser viva; pode reabrir disputa. | `app/chamados/services/relavra_via.py::desfazer_carimbo_sem_lastro:236`; `:242` | `descarimbar`; `reabrir_sistema`: `:246`; `:261` | Fábrica chama durante julgamento: `app/chamados/services/fabrica_da_celula.py:209` | `_C6` — `app/core/juizes.py:282` | SEM CASO |
| 39 — ressuscitação | Só candidata materializada e muda; exclui fora do fato e marco aceso; C6 julga cópia sem carimbos antes de devolver à fila. | `app/chamados/models.py::ChamadoColaborador._universo_da_ressuscitacao:1530`; `:1550` | `ressuscitar_perguntas` limpa materialização/resposta/validação: `:1613` | C6 e evento de célula: `app/chamados/models.py:1556`; `:1615` | `_C6` — `app/core/juizes.py:282` | SEM CASO |
| 40 — fantasma reaberto | Porta exige via fantasma e carimbo; comando escolhe lâmpada apagada+C6 viva e separa exportada para pauta. | `app/chamados/models.py::PerguntaDisputa.reabrir_fantasma:3345`; `app/chamados/management/commands/reabrir_orfas_enterradas.py::Command.handle:83`; `:95` | Descarimbo; comando chama porta em `:157` | App e pauta: `app/chamados/services/pergunta_no_app.py:70`; comando `:193` | SEM JUIZ para essa seleção composta | SEM CASO |
| 41 — competência reaberta | Remove carimbo somente se a via gravada for `competencia_trancada`. | `app/chamados/models.py::PerguntaDisputa.reabrir_por_competencia:3373` | Descarimbo: `:3378` | C6 também lê tranca atual: `app/chamados/juizes.py:1405` | SEM JUIZ para a inversão do carimbo | SEM CASO |
| 42 — motivo retirado | Disputa deve estar aberta e conter motivo; perguntas desse motivo recebem retratação e saem da identidade do fato. | `app/chamados/models.py::DisputaSupervisao.remover_motivo:2089`; `:2110` | Ato `RETRATACAO`, via `motivo_retratado`, motivos/contexto: `:2112` | C6 mata antes da resposta: `app/chamados/juizes.py:1412` | SEM JUIZ para retirar o motivo | SEM CASO |
| 43 — disputa | Fechamento normal exige ausência de resposta sem veredito e de pergunta viva; exceções nomeadas tratam silêncio. | `app/chamados/models.py::DisputaSupervisao.pode_fechar:1908`; `::fechar:1942`; `:1953`; `:1974` | `fechada_em/por/resolucao/motivo`; exceções carimbam mudas e registram ato: `:1977`; `:1988` | Cascata do pai: `app/chamados/signals.py:43` | `_C7` — `app/core/juizes.py:283` | SEM CASO para o predicado completo |
| 44 — portão do chamado | Saída de vivo para terminal exige nenhuma pergunta viva ancorada na competência aberta ou reata legítima de todas; caso contrário recusa. | `app/chamados/models.py::ChamadoColaborador._vivas_entre:361`; `::_plano_pergunta_viva:390`; `::_gate_pergunta_viva:417` | Reapontamento ou trilha de recusa; núcleo protege novamente em `:221` | `_gravar_estado`: `app/chamados/models.py:224`; admin lê fio: `app/chamados/services/fio.py:240` | SEM JUIZ para esse gate composto | SEM CASO |
| 45 — portão do pai | Pai com resposta sem veredito não fecha pelo sistema; examina origem e perguntas que o resolvem, ignorando tranca nessa guarda. | `app/ponto/supra_juiz.py::perguntas_que_seguram_csf:573`; `::pode_executar_csf:588`; `app/chamados/models.py::_gate_conversa_do_pai:716` | Recusa e nota `retido_pela_conversa`: `app/chamados/models.py:723`; `:735` | Contador: `app/chamados/juizes.py:2511` | Chaves literais de respostas sem veredito/CSF — `app/core/juizes.py:298`; `:299` | SEM CASO |
| 46 — leitura SQL | `q_vivas` recompõe vida por validação, resposta, identidade e veredito/pendência da célula; não executa todos os degraus do C6. | `app/chamados/models.py::PerguntaDisputa.q_vivas:3404` | — | App confere C6: `app/chamados/services/pergunta_no_app.py:58`; crédito/pauta não: `app/chamados/services/raiox.py:135`; `app/chamados/services/cobranca.py:228` | SEM JUIZ próprio para o predicado SQL | SEM CASO independente do C6 |
| 47 — C6 | Validação existente mata a pergunta antes dos demais degraus. | `app/chamados/juizes.py::_pergunta_viva_calc:1400` | — | `.viva`: `app/chamados/models.py:3431`; app: `app/chamados/services/pergunta_no_app.py:58` | `_C6` — `app/core/juizes.py:282` | SEM CASO |
| 48 — C6/tranca | Competência trancada mata por leitura, salvo toque administrativo posterior no mesmo colaborador/dia; conversa não tem dia de turno. | `app/chamados/juizes.py::dia_da_pergunta_trancado:991`; `::dias_trancados:1062`; `:1078` | —; não carimba pergunta | C6: `app/chamados/juizes.py:1405`; fechamento: `app/chamados/models.py:1902` | `_C20` — `app/core/juizes.py:296` | L-051: `app/docs/LEIS.md:92`; `app/chamados/tests/test_c7_red_minimos.py::C7_08_CompetenciaTrancadaTest::test_RED_o_C6_le_a_tranca_sem_escrever:361` e `::test_RED_destrancar_devolve_a_pergunta_viva_sozinha:369` |
| 49 — C6/identidade | Via terminal que vence resposta, com carimbo, mata antes de considerar texto. | `app/chamados/juizes.py::_pergunta_viva_calc:1412` | — | Lista de vias: `app/chamados/models.py:2579`; SQL: `app/chamados/models.py:3409` | `_C6` — `app/core/juizes.py:282` | SEM CASO |
| 50 — C6/resposta | Resposta não vazia continua viva, salvo concordância comprovada com luz, classe A e dia que não acusa. | `app/chamados/juizes.py::_pergunta_viva_calc:1417`; `::_resposta_concorda_com_a_luz:1342` | — | App e fila admin: `app/chamados/services/pergunta_no_app.py:58`; `app/chamados/juizes.py:1684` | `_C6` — `app/core/juizes.py:282` | Regra e respostas literais: `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:3`; `:7`; `::PerguntaRespondidaEMarcoAcesoTest::test_MORDE_concorda_ate_5_min_e_dia_coerente_morre:56` e `::test_MORDE_6_min_e_classe_B_segue_para_o_admin:59` |
| 51 — C6/admin | Veredito da célula via admin impede que a resposta, sozinha, mantenha a pergunta viva. | `app/chamados/juizes.py::_pergunta_viva_calc:1415`; `:1425` | — | Estado e app: `app/chamados/models.py:3100`; `app/chamados/services/pergunta_no_app.py:58` | `_C6` — `app/core/juizes.py:282` | SEM CASO |
| 52 — C6/marco | Com materialização, lâmpada do próprio marco comprovadamente acesa mata antes do veredito do dia; pausas múltiplas exigem todas as luzes correspondentes. | `app/chamados/juizes.py::_pergunta_viva_calc:1444`; `::fato_do_marco_no_espelho:955` | — | Porta troca via para fato materializado: `app/chamados/models.py:3324` | `_C6` — `app/core/juizes.py:282` | SEM CASO |
| 53 — C6/célula | Veredito da célula pertencente a `VEREDITOS_ACUSAM` mantém viva nesse degrau. | `app/chamados/juizes.py::_pergunta_viva_calc:1448` | 

[truncado em 20000 de 42185 caracteres]

_5159 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
