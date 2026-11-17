# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 23:01** · publicado em **16/11/2026 23:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 23:01 · sessao 64ed77f64ee2 · REVISAO-AVAL-2300 · verificador · origem=Code

**CORTE-U163-NO-K6: PROCEDE nos itens lista/contador e pré-condições do RED.** Retirar a exclusão no K6 não inclui automaticamente U163 na lista, e uma U163 com irmã fora163 entregue já pode bloquear hoje. (`app/chamados/services/materializacao.py:795`; `app/chamados/management/commands/bug_par_103.py:72`; `app/chamados/tests/test_bug_par_volta.py:145`)

**PUSH-DOS-TRES-MARCOS: PROCEDE no item 1 quanto à garantia de ordem; item 2 depende de apuração.** A trava oferece exclusão mútua, sem prioridade; a causa da demora e a ausência de tentativas posteriores são [nao verificado]. (`bin/trava_teste.sh:25`; `bin/trava_teste.sh:35`)

**CORTE-U163-NO-K6**

1. **Premissas: conferem quanto ao pulo e às autoridades.**

   `k6_consulta_a_pergunta` retorna verdadeiro somente para pergunta existente, metade de intervalo e **fora U163**; `_k6_que_barra` pula quando recebe falso. (`app/chamados/services/materializacao.py:792`; `app/chamados/services/materializacao.py:806`)

   O pulo está documentado na docstring do predicado (“as 163 nao passam pelo K6”), na docstring da guarda (“o ramo que pula as 163 nao muda”), na composição K6/F6, na docstring do escritor e no comentário de sua guarda inicial. Todos precisam acompanhar a mudança. (`app/chamados/services/materializacao.py:793`; `app/chamados/services/materializacao.py:799`; `app/chamados/services/materializacao.py:819`; `app/chamados/services/materializacao.py:1818`; `app/chamados/services/materializacao.py:1829`)

   Há **um predicado de entregue nessa composição**: `competencia_do_fato` deriva a competência pelo dia do fato e chama `competencia_entregue`; esse juiz verifica exportação vigente não invalidada e está registrado como K6. F6 responde outra pergunta. (`app/chamados/services/materializacao.py:776`; `app/chamados/services/materializacao.py:786`; `app/folha/export.py:556`; `app/folha/export.py:588`; `app/core/juizes.py:343`)

   Hoje, F6 também alcança U163: consulta a competência do **instante local do plantio**, barrando FM `aprovado` ou `exportado`; FM aberto passa por essa guarda. O materializador chama a porta com `fato_de_chao=False`, que aplica F6. (`app/ponto/janelas.py:75`; `app/ponto/janelas.py:84`; `app/ponto/registro_batida.py:154`; `app/chamados/services/materializacao.py:1455`)

2. **Colisões: extensão autorizada, mas exige atualizar a exceção e a matriz.**

   O corte anterior exige K6 nos três caminhos **fora163**; o COMPLEMENTO-4 registra explicitamente U163 entregue sob F6, com aberto seguindo. A lacuna da matriz original — L1 fora163 e L2 não entregue — já consta no RELATO. O novo corte pode suceder essa exceção, mas deve ampliar L1/L5 e registrar essa sucessão. (`app/docs/BACKLOG.md:2082`; `app/docs/RELATO.md:22`; `app/docs/RELATO.md:30`)

   A dependência da B2 está registrada: oito arquivos compartilhados, abertura por cima da BUG-PAR c4. Construir U163 depois da B2 é compatível com essa dependência; B2 efetivamente “em voo” no horário do anexo é [nao verificado]. (`app/docs/RELATO.md:37`; `app/docs/BACKLOG.md:821`)

   CORTES mantém uma condição adicional para a futura F4: pousar atrás da F1/F2 ou deixar a costura J1 sob contador esperado zero. O encaixe precisa respeitá-la. (`app/docs/CORTES.md:487`) Os avais anteriores têm remissões em PROMPTS; seu registro literal em CORTES.md fica [nao verificado]. (`app/docs/PROMPTS.md:2382`; `app/docs/PROMPTS.md:2422`)

3. **Literal: há três pendências.**

   - **PROCEDE — “vai na lista da Pauta DP”.** A lista existe como **saída textual do comando** `bug_par_103`: inclui U103 entregue/sem dia e, em outro laço, metades respondidas fora U163; depois imprime `PAUTA DP`. Não há nessa construção criação de uma `Pauta`. (`app/chamados/management/commands/bug_par_103.py:53`; `app/chamados/management/commands/bug_par_103.py:63`; `app/chamados/management/commands/bug_par_103.py:70`; `app/chamados/management/commands/bug_par_103.py:85`)

     A guarda apenas devolve estado e texto “preservada para Pauta DP”. Retirar o `not no_universo_163` não altera o laço que exclui U163; portanto não garante incluir U60 nessa lista. O ajuste do comando precisa entrar no escopo futuro. (`app/chamados/services/materializacao.py:812`; `app/chamados/management/commands/bug_par_103.py:72`)

   - **PROCEDE — classificação do contador.** Entregue/sem dia têm baldes separados no ramo das perguntas carimbadas elegíveis à devolução U103. Uma U163 não materializada que passe a receber L1/L5 cai hoje em `outras` e continua somando `pendentes_aval`. Registrar a matriz F4 precisa explicitar o destino dessas perguntas e a conservação das contagens. (`app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:347`; `app/chamados/services/materializacao.py:351`)

   - **PROCEDE — RED insuficientemente condicionado.** “U163 + entregue + FM aberto” não basta para garantir “hoje valida e grava”: quando pula a pergunta, a composição procura a irmã; uma irmã fora163 entregue já bloqueia. Existe teste desse cenário. (`app/chamados/services/validacao.py:219`; `app/chamados/services/validacao.py:224`; `app/chamados/tests/test_bug_par_volta.py:145`)

     O caminho que pode gravar é `validar_pergunta` → carimbo → `_validar_o_par`/`_materializar_uma` → `registrar_batida`. Exige resposta, ausência de validação anterior, passagem pelo declarado × gravado, confirmação de implausível quando necessária e demais guardas do plantio. (`app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:609`; `app/chamados/services/validacao.py:615`; `app/chamados/services/validacao.py:652`; `app/chamados/services/validacao.py:681`; `app/chamados/services/materializacao.py:1455`)

     O número **1** está declarado no registro da sonda; ele não demonstra, sozinho, que essa pergunta atravessa todas essas condições e cria batida. Essa conclusão e a nova medição permanecem [nao verificado]. (`app/docs/RELATO.md:19`)

4. **Exportada, dinheiro e `!`: não há autorização adicional implícita.**

   O bloqueio proposto atua antes do carimbo na validação e antes de recusa/pendência no escritor, compatível com preservar o gravado. Seu efeito declarado é impedir plantio futuro em competência entregue. (`app/chamados/services/validacao.py:652`; `app/chamados/services/materializacao.py:1829`)

   Há recalculo por evento no caminho atual de validação, já apontado como herdado no RELATO. Isso não autoriza apply, retificação ou alteração de FM exportado; essa escrita continua sob `!`. Impacto monetário efetivo é [nao verificado]. (`app/chamados/services/validacao.py:664`; `app/docs/RELATO.md:30`; `CLAUDE.md:411`)

**PUSH-DOS-TRES-MARCOS** [nao verificado]

1. **Premissas: a regra existe; push disputa a trava.** [nao verificado]

   “PUSH: um por MARCO” está literal. `push.sh` chama `git push`, preserva seu retorno e chama `pos_push.sh` após sucesso; o pós-push atualiza e commita o derivado TICKETS. (`CLAUDE.md:336`; `bin/push.sh:6`; `bin/pos_push.sh:18`; `bin/pos_push.sh:29`)

   O pre-push adquire `/tmp/regua.lock`, com espera ou herança; cada `_teste_irmao` disputa separadamente a trava do banco de teste. O arsenal pode ser dispensado por certificação da mesma árvore; o control-plane permanece chamado. (`bin/pre-push.sh:80`; `bin/pre-push.sh:86`; `bin/pre-push.sh:142`; `bin/pre-push.sh:66`; `bin/pre-push.sh:198`)

   Os três marcos têm registros documentais, mas SHA remoto, distância de 34 commits, hook instalado e ausência efetiva de push são [nao verificado]. (`app/docs/RELATO.md:5`; `app/docs/RELATO.md:6`; `app/docs/RELATO.md:13`; `app/docs/RELATO.md:15`)

2. **Colisões: a finalidade confere; a prioridade precisa ser despachada.**

   CORTES reserva a VM para espinha, pouso e push; lateral usa a pista e só volta à VM, atrás da espinha, se a pista não responder. O novo aval não deve ser interpretado como trazer lateral para disputar normalmente a VM. (`app/docs/CORTES.md:489`)

   A continuidade da B2 é compatível com a dependência registrada; sua execução e eventual interferência operacional são [nao verificado]. (`app/docs/RELATO.md:37`) O COMPLEMENTO-2 fornecido exclui push de seu alcance; conferência do texto integral em arquivo local é [nao verificado].

3. **Literal: item 1 precisa de mecanismo operacional; item 2 não tem causa provada.**

   - **PROCEDE, item 1:** `flock -w` oferece exclusão mútua, sem ticket, posição ou prioridade. Apenas lançar o push em espera não garante “logo depois da atual, antes da lateral”. Arsenal e control-plane ainda fazem aquisições separadas, permitindo intercalação. A ordem precisa ser assegurada pelo despacho dos chamadores. (`bin/trava_teste.sh:25`; `bin/trava_teste.sh:35`; `bin/pre-push.sh:142`; `bin/pre-push.sh:186`; `bin/pre-push.sh:198`)
   - **Item 2:** há registro de push em pre-push às 17:1x. A causa da demora e a ausência de tentativas posteriores são [nao verificado]; a linha pedida não pode inventá-las. (`app/docs/RELATO.md:44`)
   - **Item 3:** pedir push no fechamento acompanha a regra vigente; os scripts examinados não implementam esse agendamento automático. (`CLAUDE.md:332`; `CLAUDE.md:336`; `bin/push.sh:6`; `bin/pre-push.sh:142`)
   - **Item 4:** as duas partials existem. L5 mostra o detalhe “sem dia do fato” e oculta Validar; L2 conserva Validar e preserva `COMPETENCIA_APROVADA` na recusa, devolução e nota. O roteiro pode cobrar esses resultados; disponibilidade de casos reais e execução do smoke são [nao verificado]. (`app/chamados/tests/test_bug_par_volta.py:200`; `app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/modal_fio.html:274`; `app/chamados/tests/test_bug_par_volta.py:247`; `app/chamados/tests/test_bug_par_volta.py:258`; `app/chamados/tests/test_bug_par_volta.py:267`; `app/chamados/tests/test_bug_par_volta.py:313`)

4. **Exportada, dinheiro e `!`: nenhum toque direto aparece nesses caminhos.**

   O wrapper executa push e pós-push documental; os testes usam árvore do commit e configurações de teste. Não há chamada explícita de deploy/apply nesses trechos. Efeitos efetivos do ambiente e do hook instalado são [nao verificado]. (`bin/push.sh:6`; `bin/pos_push.sh:19`; `bin/pos_push.sh:29`; `bin/pre-push.sh:144`; `bin/pre-push.sh:186`; `bin/pre-push.sh:198`)

**Quem escreve e quem lê**

| Valor | Quem produz/escreve | Leitores diretos de produção |
|---|---|---|
| `no_universo_163(p)` | É função, sem campo persistido: produz booleano consultando a PK no universo congelado. O carregador lê o JSON e forma conjuntos imutáveis. (`app/chamados/services/universo_bug_par.py:7`; `app/chamados/services/universo_bug_par.py:12`; `app/chamados/services/universo_bug_par.py:25`; `app/chamados/services/universo_bug_par.py:33`) | `_espera_do_aval_163`, para aplicar declarado × gravado; `k6_consulta_a_pergunta`, para excluir U163. (`app/chamados/services/materializacao.py:742`; `app/chamados/services/materializacao.py:795`) |
| Retorno de `k6_consulta_a_pergunta(p)` | A própria função produz **booleano de consulta**, não veredito de entrega. (`app/chamados/services/materializacao.py:792`) | `_k6_que_barra`, decidindo pular a pergunta; `entrega_que_barra_a_validacao`, decidindo procurar a irmã quando a pergunta é pulada. (`app/chamados/services/materializacao.py:806`; `app/chamados/services/validacao.py:224`) |

Os leitores diretos de teste de pertencimento estão em `test_universo_bug_par`, `test_bug_par_volta`; há substituições do predicado nos testes sem banco e mutantes de pertencimento. A função K6 também é carregada no ambiente do teste sem banco. Isso é evidência de código, sem comprovação de execução nesta revisão. (`app/chamados/tests/test_universo_bug_par.py:65`; `app/chamados/tests/test_bug_par_volta.py:33`; `app/chamados/tests/test_bug_par_volta.py:100`; `app/chamados/tests/test_bug_par_sem_banco.py:88`; `app/chamados/tests/test_bug_par_sem_banco.py:134`; `app/chamados/tests/bug_par_mutantes.py:55`; `app/chamados/tests/test_bug_par_sem_banco.py:140`)

## 16/11 22:58 · sessao 3a1b9f7f1505 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **Premissas do pulo conferem.** `k6_consulta_a_pergunta` devolve `False` para U163 (`app/chamados/services/materializacao.py:793-795`); `_k6_que_barra` obedece esse retorno (`:805-808`). Documentação a atualizar no ato: docstrings `:793-794`, `:799-803`, `:819`; comentário sobre procurar a irmã somente quando a pergunta é pulada em `app/chamados/services/validacao.py:220-224`.

2. **Um predicado de entregue nessa composição.** `competencia_do_fato` compõe competência pela data do fato com `competencia_entregue` (`app/chamados/services/materializacao.py:776-789`); registro do juiz em `app/core/juizes.py:343`; implementação do predicado único em `app/folha/export.py:556-594`. F6 consulta outra pergunta: `FechamentoMensal` aprovado/exportado para a competência dos instantes (`app/ponto/janelas.py:75-89`, `app/chamados/services/materializacao.py:818-835`).

3. **Mudança autorizada sucede a exceção vigente; não é colisão impeditiva por si.** O corte anterior registrado no BACKLOG exige K6 nos três caminhos **fora163** (`app/docs/BACKLOG.md:2082`). COMPLEMENTO-4 registra que U163 entregue atualmente cai em F6: aprovado/exportado bloqueia; aberto segue (`app/docs/RELATO.md:22`). A matriz original deixou lacuna para U163 entregue, registrada em `app/docs/RELATO.md:30`. É necessário ampliar L1/L5 e registrar a sucessão dessa exceção; atualizar só F4 deixa o texto anterior coexistindo (`app/docs/RELATO.md:22,30`; matriz anexada pelo usuário [nao verificado quanto a arquivo local]).

4. **PROCEDE — condição literal “vai na lista da Pauta DP”.** A lista existe como saída do comando: U103 entra pelo laço de `app/chamados/management/commands/bug_par_103.py:53-63`; fora163 entra pelo laço `:71-77`; a saída é impressa em `:85`. Esse segundo laço exclui U163 inteira (`:72`), portanto retirar a exclusão da guarda K6 não põe U60 nessa lista. O contador também coloca bloqueadas por entregue/sem dia em `outras` e conserva a soma em `pendentes_aval` (`app/chamados/services/materializacao.py:339-356`); só o ramo das já materializadas separa pauta/sem dia (`:326-336`). Registrar uma matriz não implementa esse encaminhamento [nao verificado quanto à implementação futura].

5. **PROCEDE — RED insuficientemente condicionado.** “Uma das 163 + entregue + FM aberto” não garante que hoje valida e grava: ao pular a pergunta U163, a porta procura a irmã e consulta seu K6 (`app/chamados/services/validacao.py:220-225`); há teste de U163 com irmã fora163 entregue já bloqueando hoje (`app/chamados/tests/test_bug_par_volta.py:145-160`). Também há portão declarado × gravado (`app/chamados/services/materializacao.py:922-937`). O RED deve explicitar irmã igualmente pulada/ausente e declaração compatível, além das demais condições de plantio (`app/chamados/services/validacao.py:220-225`; `app/chamados/services/materializacao.py:895-937`). O caso real da sonda efetivamente gravar permanece **[nao verificado]**.

6. **B2.** O registro disponível diz que cruza oito arquivos com BUG-PAR e abre por cima da c4 (`app/docs/RELATO.md:37`; `app/docs/BACKLOG.md:821`). “B2 em voo” na hora do aval não está demonstrado pelo dado anexado **[nao verificado]**. Respeitar sua continuidade e encaixar U163 depois dela é compatível com a dependência registrada (`app/docs/RELATO.md:37`).

7. **CORTES.md.** Não localizei registro literal de `CORTE-BUG-PAR-FORA163`, `COMPLEMENTO-2 ESPINHA-ABRE-A-VOLTA-BUG-PAR` ou `CORTE-U163-NO-K6` no arquivo **[nao verificado quanto a registro ausente]**. Há referências aos dois primeiros em `app/docs/PROMPTS.md:2382,2422`; o corte fora163 é reproduzido em `app/docs/BACKLOG.md:2082`.

**Veredito recomendado U163: PROCEDE nos itens literais de encaminhamento à lista/contador e pré-condições do RED**, pelas referências dos itens 4–5; a extensão do K6 em si confere com a estrutura existente (`app/chamados/services/materializacao.py:792-835`).

## 16/11 22:58 · sessao d0594a09bc3a · - · - · origem=Ronald (tmux)

1. **Pulo atual confirmado.** `k6_consulta_a_pergunta` retorna bool e exclui `no_universo_163`; a docstring diz literalmente “as 163 nao passam pelo K6” (`app/chamados/services/materializacao.py:792-795`). `_k6_que_barra` documenta “o ramo que pula as 163 nao muda” e consome aquele bool (`:798-807`). Outros textos a atualizar no mesmo ato: composição “metades fora163” (`:818-824`), docstring de `_materializar_uma` (`:1813-1819`) e comentário da guarda inicial (`:1829-1833`).

2. **Um juiz de entregue neste caminho.** `competencia_do_fato` compõe janela do dia do fato com `folha.export.competencia_entregue`, devolvendo `None` sem dia ou `(mes, ano, entregue)` (`app/chamados/services/materializacao.py:776-789`). O juiz verifica Exportação vigente não invalidada; não usa `FechamentoMensal` (`app/folha/export.py:556-594`). A F6 é outra pergunta: por instante local, busca FM e barra somente status `aprovado`/`exportado` (`app/ponto/janelas.py:75-89`); `registrar_batida` a aplica para `fato_de_chao=False` (`app/ponto/registro_batida.py:147-161`), exatamente a flag passada pelo materializador (`app/chamados/services/materializacao.py:1454-1462`). FM aberto portanto não basta para barrar U163 entregue por esse caminho (`app/ponto/janelas.py:86-89`).

3. **RED possível, porém existência real não demonstrada.** Porta exige resposta, ausência de validação anterior (`app/chamados/services/validacao.py:594-599`), passa pelo portão declarado x gravado e pela confirmação de implausível (`:605-618`), consulta K6 de pergunta+irmã e só depois carimba (`:638-655`); segue por `_validar_o_par` ou `_materializar_uma` (`:681-687`), e o escritor chama `registrar_batida` (`app/chamados/services/materializacao.py:1454-1462`). Uma U163 com irmã fora163 entregue **já é bloqueada hoje**: a composição consulta também a irmã quando pula a pergunta (`app/chamados/services/validacao.py:219-225`), cenário explicitamente coberto no código do teste (`app/chamados/tests/test_bug_par_volta.py:145-160`; execução [nao verificado]). Portanto o RED precisa uma U163 realmente gravável, sem irmã fora163 que já a barre e sem L3/paridade/data/reuso impeditivos (`app/chamados/services/materializacao.py:889-940`). O “1” da sonda e qual pergunta satisfaz todas essas condições: [nao verificado].

4. **PROCEDE — literal “a pergunta vai na lista da Pauta DP” / escopo MUDA incompleto.** A lista existe como saída textual do comando `bug_par_103`, não como escrita de `Pauta`: constrói `pauta=[]`, inclui U103 entregue/sem dia, acrescenta metades respondidas hora **fora U163** e imprime stdout (`app/chamados/management/commands/bug_par_103.py:54-85`). `_k6_que_barra` apenas retorna estado e texto “preservada para Pauta DP” (`app/chamados/services/materializacao.py:808-815`). Retirar a exclusão no bool não faz o comando incluir U60/U163 não materializadas, pois o filtro atual continua excluindo U163 (`app/chamados/management/commands/bug_par_103.py:70-77`). Deve declarar ajuste desse leitor ou deixar explícito que o encaminhamento ainda não estará implementado.

5. **PROCEDE — F4 exige completar classificação.** Contador só separa entregues/sem dia no ramo das U103 carimbadas (`app/chamados/services/materializacao.py:325-337`). Pergunta U163 não materializada que ganhar L1/L5 cairá hoje em `outras` (`:339-347`), continuará em `universo` e `pendentes_aval` (`:351-356`); não irá automaticamente aos baldes `bug_par_103_pauta_dp`/`bug_par_103_sem_dia`. Isso precisa entrar na matriz F4 que o aval manda registrar (`app/chamados/services/materializacao.py:311-316`).

6. **Escopo e preservação.** Bloqueio K6 precede qualquer carimbo na validação (`app/chamados/services/validacao.py:652-655`) e precede recusa/pendência/materialização no escritor (`app/chamados/services/materializacao.py:1829-1833`), compatível com “nada gravado”. O delta muda a possibilidade futura de plantar batidas em competência entregue, cuja criação passa pelo escritor indicado (`app/chamados/services/materializacao.py:1454-1462`); impacto monetário efetivo e dados exportados de produção [nao verificado]. As condições de L3 continuam antes da escrita (`app/chamados/services/validacao.py:605-613`), e L1/L5 permanecem estados próprios consumidos pela tela (`app/templates/chamados/partials/_pergunta_card.html:44-47`, `app/templates/chamados/partials/modal_fio.html:274`).

**Veredito do recorte: PROCEDE nos itens lista Pauta DP e classificação F4; RED e contagem precisam ser explicitamente tratados como cenário a demonstrar, sem afirmar prova de produção [nao verificado].**

## 16/11 22:58 · sessao e0112d968bb3 · - · - · origem=Ronald (tmux)

Veredito PUSH: **PROCEDE, item 1** — a posição na fila não é garantida pelos scripts; os demais itens conferem como instruções de conduta/documentação, com limites abaixo. (`bin/trava_teste.sh:35-42`; `CLAUDE.md:332-336`)

1. **Premissas.** “PUSH: um por MARCO” existe literalmente. (`CLAUDE.md:336`) `push.sh` chama `git push`, preserva seu rc e, se passar, chama `pos_push.sh`. (`bin/push.sh:6-9`) O pós-push atualiza e commita somente o derivado do TICKETS. (`bin/pos_push.sh:18-30`) O pre-push possui trava da régua, espera até 30 minutos ou herda a trava do chamador; cada execução de testes disputa também a trava do banco de teste. (`bin/pre-push.sh:80-89`, `bin/pre-push.sh:142-145`) O arsenal pode ser dispensado pelo carimbo da mesma árvore; o control-plane continua chamado. (`bin/pre-push.sh:65-70`, `bin/pre-push.sh:159`, `bin/pre-push.sh:190-199`) Existe bypass `SKIP_TESTS=1`; o aval não o autoriza. (`bin/pre-push.sh:5`; [nao verificado] quanto ao ambiente efetivo)

2. **Colisão.** A orientação de push por marco está alinhada com CLAUDE. (`CLAUDE.md:332-336`) A B2 tinha dependência expressa de pousar BUG-PAR antes de abrir, por oito arquivos em comum; isso é registro da cópia, não prova de seu estado atual em execução. (`app/docs/RELATO.md:37`) O COMPLEMENTO-2 separa push do próprio alcance no dado fornecido; interferência operacional real na B2/suite em curso fica [nao verificado]. Os cortes de dinheiro preservam `!` para alterar gravado exportado. (`CLAUDE.md:411-417`)

3. **Literal que não fecha.**
   - **Item 1, PROCEDE:** a trava implementa somente `flock -w`, execução e liberação; não tem posição, ticket, prioridade nem reserva para “push depois da atual e antes da lateral”. (`bin/trava_teste.sh:21-42`) Além disso, arsenal e control-plane são duas aquisições separadas da trava: outra execução pode intercalar. (`bin/pre-push.sh:142-145`, `bin/pre-push.sh:186-199`) Precisa declarar o despacho dos chamadores e impedir que a lateral seja lançada antes do push; atribuir essa ordem à trava seria falso. (`bin/trava_teste.sh:25-42`)
   - **Item 2:** não há prova da causa da demora nem da ausência de tentativa posterior; [nao verificado]. Há registro de push em pre-push às 17:1x. (`app/docs/RELATO.md:44`) Logo a linha deve relatar uma causa apurada, sem inventar histórico; [nao verificado].
   - **Item 3:** pedir o push no fechamento está alinhado à regra existente, mas não há agendador automático nos dois scripts. (`CLAUDE.md:332-336`; `bin/push.sh:1-9`; `bin/pre-push.sh:80-89`)
   - **Item 4:** as duas partials existem, identificadas também no teste de matriz. (`app/chamados/tests/test_bug_par_volta.py:200-202`) L2 conserva Validar e cobra motivo/nota `COMPETENCIA_APROVADA`; L5 mostra “sem dia do fato” e oculta Validar. (`app/chamados/tests/test_bug_par_volta.py:247-267`, `app/chamados/tests/test_bug_par_volta.py:311-318`) O RELATO apenas remete o smoke ao responsável, sem roteiro ali. (`app/docs/RELATO.md:5`) Disponibilidade de casos reais para executar esses cliques: [nao verificado].
   - SHA remoto `71e9576bf`, 34 commits e ausência de push dos três marcos não puderam ser confirmados por git: cópia sem repositório utilizável; [nao verificado]. Há registros dos pousos e do uso de `343565cf6` como base, que não demonstram estado remoto. (`app/docs/RELATO.md:6`, `app/docs/RELATO.md:13-16`)

4. **Exportada, dinheiro, `!`.** O wrapper apresentado faz push e atualização documental do TICKETS; o pre-push executa testes com configurações de teste e árvore do commit. (`bin/push.sh:6-8`; `bin/pos_push.sh:18-30`; `bin/pre-push.sh:142-145`, `bin/pre-push.sh:186-199`) Nesses caminhos não há chamada explícita de apply/deploy/retificação de produção. (`bin/push.sh:1-9`; `bin/pos_push.sh:18-30`; `bin/pre-push.sh:186-199`) A escrita real em produção, efeitos das credenciais e estado do hook instalado são [nao verificado]; o wrapper esperado do hook está documentado no instalador. (`bin/hooks.sh:60-65`)

## 16/11 22:30 · sessao c014cdc3a87b · E4C-F1-B2 · verificador · origem=Code

**Não aprovaria B2 como “todo leitor lê o juiz” nem como garantia de ausência de efeitos nas competências entregues.** Restam leitores operacionais próprios e existem caminhos de escrita alcançados pela mudança da retenção. `app/chamados/services/verbos.py:335`, `app/chamados/canal.py:65`, `app/chamados/models.py:1925`, `app/ponto/management/commands/supra_juiz.py:286`.

A cópia não contém `.git`: identidade do HEAD e comparação integral `520d7b557..HEAD` **[nao verificado]**. Testes, PostgreSQL, estado efetivo das duas perguntas e efeitos nas 141 entregues **[nao verificado]**.

**ACHADOS**

1. **Grave — a lista fechada do saneamento não isola os efeitos dos leitores nas entregues.** A retenção deixa de segurar ambos os pares ilegais; isso libera o CSF a tentar retratação. O gate posterior de pergunta viva exclui expressamente competência entregue, portanto não substitui essa proteção. Há caminho para resolver chamado, rejeitar justificativa e executar o resolver de perguntas. `app/chamados/models.py:1923`, `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:588`, `app/chamados/models.py:364`, `app/chamados/reconciliador.py:525`, `app/chamados/reconciliador.py:531`, `app/chamados/reconciliador.py:538`.  
   Isso comprova **alcance possível**, não ocorrência nas 141 perguntas: faltam suas datas, âncoras, lâmpadas e trilhas **[nao verificado]**.

2. **Médio — a migração dos leitores ficou incompleta.** O placar continua chamando de “respondidos” os registros selecionados somente por instante; o canal partido continua definindo “sem resposta” por texto exatamente `''`; a ressuscitação mantém seleção por texto NULL/`''`; cobrança/cartão ainda recebem uma recomposição local de `not J`. São resíduos preexistentes que impedem certificar a conclusão “todo leitor”. `app/chamados/services/verbos.py:325`, `app/chamados/services/verbos.py:335`, `app/chamados/canal.py:53`, `app/chamados/canal.py:65`, `app/chamados/models.py:1494`, `app/chamados/models.py:1533`, `app/chamados/services/fio.py:187`.

3. **Médio — a lista fechada não é uma restrição da porta.** `ESTOQUE_L130_ALVOS` existe, mas `impedimento_estoque_l130` e a porta aceitam qualquer pk elegível. Somente o comando compara o conjunto com a lista, antes das travas; depois aplica uma pergunta por transação. Assim, a igualdade é garantida na fotografia inicial, não como condição atômica do lote inteiro. Pode haver aplicação parcial por perda de elegibilidade ou falha de trilha da outra pergunta. `app/chamados/services/validacao.py:378`, `app/chamados/services/validacao.py:381`, `app/chamados/services/validacao.py:432`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:53`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:70`.

4. **Médio — novo caminho de julgamento sem relógio recebido da borda.** O predicado novo chama `pergunta_viva(perg)` sem `agora`/`hoje`; esse caminho pode ler `date.today()` e, adiante, `timezone.now()`. A leitura do comando acontece depois da seleção e serve apenas ao arquivo de reversão. Contraria a consequência documentada de Clock Injection. `app/chamados/services/validacao.py:398`, `app/chamados/juizes.py:596`, `app/chamados/juizes.py:1475`, `app/escala/servico_jornada.py:387`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:61`, `app/docs/PLANO-PADROES.md:101`.

5. **Médio — o teste da property não prova delegação.** Comparar resultados com o juiz permite passar uma property que copie integralmente sua expressão. Os casos distinguem as regras antigas B/C/D, mas não demonstram a mordida de um mutante que substitua a delegação por um segundo juiz equivalente. `app/chamados/tests/test_f1nova_b2_leitores.py:61`, `app/chamados/tests/test_f1nova_b2_leitores.py:75`, `app/chamados/tests/test_f1nova_b2_leitores.py:142`. Mordidas executadas **[nao verificado]**.

6. **Menor — documentação permanece na situação anterior.** O lookup ainda diz que B/C/D migrarão no B2, e a LEIS declara B2 “por construir”; o teste O168 conserva docstring dizendo que a retenção usa I OU T e “nunca solta”, embora seu corpo agora teste o contrário para os pares ilegais. `app/docs/JUIZES.md:105`, `app/docs/LEIS.md:151`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:278`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:290`.

**Conferido e conforme, com as ressalvas acima**

**1. Escritores, leitores e significado dos campos**

O contrato correto é J = instante presente E texto restante após trim; os carimbos são `DateTimeField`, não o dia do fato. `app/chamados/juizes.py:1552`, `app/chamados/models.py:2553`, `app/chamados/models.py:2554`, `app/chamados/models.py:3531`.

| Campo | Quem escreve em produção |
|---|---|
| `respondida_em` | Nascimento com campos recebidos; resposta humana; resposta automática; recuperação pela trilha; preenchimento de simetria em `carimbar`; limpeza por reabertura/ressuscitação e pela nova porta, via lote. `app/chamados/models.py:3124`, `app/chamados/services/disputa_emissao.py:1482`, `app/chamados/services/disputa_emissao.py:1675`, `app/chamados/services/disputa_emissao.py:1696`, `app/chamados/models.py:3243`, `app/chamados/services/disputa_emissao.py:1648`, `app/chamados/models.py:1629`, `app/chamados/services/validacao.py:440`. |
| `resposta_colab` | Nascimento; resposta humana/automática; limpeza integral na reabertura, ressuscitação e saneamento L-130. `app/chamados/models.py:3124`, `app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/services/disputa_emissao.py:1609`, `app/chamados/models.py:1630`, `app/chamados/services/validacao.py:440`. |
| `validada_em` | Validação administrativa; preenchimento de simetria em `carimbar(resposta='validar')`; limpeza por nova resposta, devolução de validação, BUG-PAR-103, reabertura e ressuscitação. `app/chamados/services/validacao.py:168`, `app/chamados/models.py:3246`, `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/validacao.py:273`, `app/chamados/services/validacao.py:349`, `app/chamados/services/disputa_emissao.py:1609`, `app/chamados/models.py:1629`. |
| `materializada_em` | Atribuição central em `_escrever_carimbo`, usada por `carimbar`, `carimbar_identidade` e `descarimbar`; limpeza de lote em `descarimbar_em_lote`. `app/chamados/models.py:3210`, `app/chamados/models.py:3240`, `app/chamados/models.py:3293`, `app/chamados/models.py:3319`, `app/chamados/models.py:3394`. |
| `via_resolucao` | Mesmas portas de carimbo; adicionalmente, encerramento por fato no espelho e relavra da via derivada. `app/chamados/models.py:3212`, `app/chamados/models.py:3394`, `app/chamados/models.py:3403`, `app/chamados/services/relavra_via.py:125`. |
| `DecisaoPergunta` | Escritor físico `registrar`. Recebe atos de nascimento, resposta, reabertura/recusa, validação/devolução, silêncio, retratação, absorção e reconstrução da projeção. `app/chamados/models.py:4124`, `app/chamados/models.py:3129`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1631`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:354`, `app/chamados/models.py:2005`, `app/chamados/models.py:2137`, `app/chamados/services/materializacao.py:1048`, `app/chamados/services/validacao.py:439`. |
| Evento novo | A porta chama `evento('l130_instante_sem_texto_devolvida')`; o escritor de `LogAuditoria` é `core.observ.evento`. `app/chamados/services/validacao.py:449`, `app/core/observ.py:134`. |

Os leitores ficam distribuídos assim:

| Campo | Quem lê |
|---|---|
| Par instante/texto para decidir resposta | Juiz Python, faces SQL, vitalidade temporal, property, estado, retenção, corrigíveis e fila a validar. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1423`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1588`, `app/chamados/models.py:3669`, `app/chamados/models.py:3156`, `app/chamados/models.py:1925`, `app/chamados/juizes.py:1753`, `app/chamados/juizes.py:1760`. |
| Par instante/texto nos consumidores migrados | API/web, regularização/reabertura, declaração de hora, partição do app, validação/recusa, becos, canônica/mérito, materialização, reconciliador, cartório, ficha, calendário, prévia e auditoria. `app/api/views.py:2577`, `app/api/views.py:2620`, `app/chamados/views.py:1280`, `app/chamados/views.py:1417`, `app/chamados/services/acoes_disputa.py:98`, `app/chamados/services/acoes_disputa.py:133`, `app/chamados/services/fio.py:706`, `app/chamados/services/declaracao_texto.py:417`, `app/chamados/services/declaracao_texto.py:687`, `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:900`, `app/chamados/services/becos.py:30`, `app/chamados/services/fio.py:1281`, `app/chamados/services/fio.py:1294`, `app/chamados/services/materializacao.py:872`, `app/chamados/services/materializacao.py:1315`, `app/chamados/services/materializacao.py:2069`, `app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:1085`, `app/ponto/services/cartorio.py:1642`, `app/colaboradores/services/ficha.py:171`, `app/colaboradores/services/calendario.py:505`, `app/folha/previa.py:142`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1658`. |
| `respondida_em` como tempo bruto | Reconstrução da projeção, marcos de invalidação, histórico, idade da fila e serialização API; também o placar divergente. `app/chamados/models.py:4095`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/historico.py:99`, `app/chamados/services/validacao.py:1254`, `app/api/views.py:2563`, `app/chamados/services/verbos.py:335`. |
| `resposta_colab` como conteúdo | Display, parsing do horário/data, opções/escapes, classificação da resposta e testemunha declarado × gravado. `app/chamados/models.py:3692`, `app/chamados/juizes.py:364`, `app/chamados/juizes.py:1788`, `app/chamados/juizes.py:1935`, `app/chamados/juizes.py:2039`, `app/chamados/services/materializacao.py:625`, `app/chamados/services/materializacao.py:718`, `app/chamados/services/materializacao.py:953`, `app/chamados/services/materializacao.py:1562`, `app/chamados/services/validacao.py:1181`. |
| `validada_em` | Gates de chamado e retenção, estado/vitalidade/SQL, validação/devolução/recusa/lote, materialização/irmã, declaração, fila de flip, vigias, placares, histórico e auditorias. `app/chamados/models.py:310`, `app/chamados/models.py:344`, `app/chamados/models.py:1924`, `app/chamados/models.py:3151`, `app/chamados/models.py:3497`, `app/chamados/juizes.py:1389`, `app/chamados/services/validacao.py:269`, `app/chamados/services/validacao.py:684`, `app/chamados/services/validacao.py:897`, `app/chamados/services/validacao.py:1062`, `app/chamados/services/materializacao.py:199`, `app/chamados/services/materializacao.py:2070`, `app/chamados/services/declaracao_texto.py:227`, `app/ponto/services/flip_auto.py:130`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/services/verbos.py:264`, `app/chamados/services/painel.py:270`, `app/chamados/services/historico.py:101`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`. |
| `materializada_em` | Ressuscitação, retenção, estado, portas de carimbo, vitalidade/lastro, reconciliador, materialização/passivo, validação, API/web, emissão, procedência, marcos temporais, histórico e comandos de estoque/auditoria. `app/chamados/models.py:1531`, `app/chamados/models.py:1924`, `app/chamados/models.py:3153`, `app/chamados/models.py:3293`, `app/chamados/juizes.py:683`, `app/chamados/juizes.py:1502`, `app/chamados/reconciliador.py:368`, `app/chamados/reconciliador.py:631`, `app/chamados/services/materializacao.py:862`, `app/chamados/services/materializacao.py:2071`, `app/chamados/services/passivo_validado.py:534`, `app/chamados/services/validacao.py:264`, `app/chamados/views.py:1257`, `app/chamados/services/disputa_emissao.py:776`, `app/ponto/services/procedencia_batida.py:74`, `app/chamados/veredito_lavrado.py:203`, `app/chamados/services/historico.py:103`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`. |
| `via_resolucao` | Identidade/terminalidade, vitalidade e projeção temporal, ressuscitação, estado/carimbo, resolver, canônica/mérito/dedup, materialização/recusas/absorção, tarefa, relavra, Pauta/tranca, passivo, explicador e templates. `app/chamados/models.py:3312`, `app/chamados/models.py:3484`, `app/chamados/juizes.py:683`, `app/chamados/juizes.py:1390`, `app/chamados/models.py:1535`, `app/chamados/models.py:3166`, `app/chamados/services/disputa_emissao.py:2072`, `app/chamados/services/fio.py:1293`, `app/chamados/services/fio.py:1400`, `app/chamados/services/materializacao.py:868`, `app/chamados/services/materializacao.py:1111`, `app/chamados/services/tarefa_do_dia.py:256`, `app/chamados/services/relavra_via.py:118`, `app/chamados/services/competencia_trancada.py:276`, `app/chamados/services/passivo_validado.py:141`, `app/chamados/services/explicador.py:118`, `app/templates/chamados/partials/_card_atendimento.html:192`. |
| Decisões/evento | Decisões alimentam projeção temporal, tarefa, respostas anteriores e reconstrução da projeção; o evento fica disponível na consulta genérica da auditoria. `app/chamados/juizes.py:1389`, `app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:421`, `app/chamados/models.py:3679`, `app/chamados/models.py:4098`, `app/core/views.py:705`. |

**Não há significado uniforme em todos os leitores.** Os resíduos do achado 2 ainda confundem presença de carimbo ou de string com resposta. Além disso, `validada_em` tem assinatura administrativa e preenchimento de simetria; `materializada_em` também registra encerramentos sem plantio de batida. Esses significados já existem nas portas, não foram criados pelo descarimbo B2. `app/chamados/services/verbos.py:335`, `app/chamados/canal.py:65`, `app/chamados/models.py:3245`, `app/chamados/models.py:3175`, `app/chamados/models.py:3415`.

O instante da resposta não é o dia do fato: a recuperação usa o instante testemunhado pela nota; a resposta automática usa instante recebido; K6 usa o dia do fato. Não encontrei conversão nova desses carimbos em “dia de competência”. `app/chamados/services/disputa_emissao.py:1687`, `app/chamados/services/disputa_emissao.py:1675`, `app/chamados/services/materializacao.py:784`. Ausência integral de tal conversão fora dos caminhos examinados **[nao verificado]**.

**2. Leitores residuais: migrar ou preservar**

A classificação formal E/F depende do censo citado, que não veio no dado nem está disponível na cópia **[nao verificado]**. A classificação abaixo é pela finalidade observável.

| Residual | Avaliação |
|---|---|
| `fio.espera_o_texto_do_colaborador` | **Deveria delegar.** Recompõe `I ausente OU instante sem texto`, equivalente hoje a `not J`; alimenta cobrança e cartão. `app/chamados/services/fio.py:187`, `app/chamados/services/cobranca.py:227`, `app/chamados/services/fio.py:359`. |
| `canal.perguntas_mudas_com_resposta` | **Deveria migrar**, se “sem resposta” mantém o sentido declarado: exige texto exatamente vazio e perde brancos/NBSP/texto sem instante. Consumido pelo cobrar. `app/chamados/canal.py:53`, `app/chamados/canal.py:65`, `app/chamados/views_cobrar.py:181`. |
| `verbos.placar_aceite` | **Deveria migrar.** “Respondidos” depende somente de I após cobrança; chega ao painel e à Folha. `app/chamados/services/verbos.py:325`, `app/chamados/services/verbos.py:335`, `app/chamados/services/painel.py:298`, `app/folha/views_retrato.py:515`. |
| `_universo_da_ressuscitacao` | **Regra operacional própria**, não exibição: declara “SEM resposta”, mas seleciona NULL/`''`. Brancos/NBSP ficam fora; T sem I conserva proteção pelo texto. `app/chamados/models.py:1494`, `app/chamados/models.py:1530`. |
| `bin/censo_folha.py` | **Também residual**, fora de `app/`: publica `PERG_VIVAS`/`AGUARDA_ADMIN` por I nulo/não nulo. É instrumento de diagnóstico; uso efetivo em produção **[nao verificado]**. `bin/censo_folha.py:28`, `bin/censo_folha.py:29`, `bin/censo_folha.py:37`. |

Preservações legítimas por finalidade:

- **Trilha, projeção e cache temporal:** leem o carimbo bruto, inclusive ilegal, para não apagar evidência. `app/chamados/models.py:4095`, `app/chamados/models.py:4102`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `app/chamados/services/historico.py:98`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/painel.py:67`.
- **Integridade/estoque específico:** detectam meio-par, não julgam resposta operacional. I20 e curar limbo ainda usam `!= ''`, portanto seu diagnóstico também não acompanha o trim. `app/chamados/services/vigia_fio.py:737`, `app/chamados/services/disputa_emissao.py:1692`, `app/chamados/services/disputa_emissao.py:1861`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`.
- **Preservação de voz e reparo do par:** impedir sobrescrita de texto bruto na resposta automática; preencher carimbo faltante na porta de simetria. `app/chamados/services/disputa_emissao.py:1663`, `app/chamados/models.py:3241`.
- **Parse e apresentação de conteúdo/testemunha:** não são, por si, outro julgamento de J. `app/chamados/services/materializacao.py:625`, `app/chamados/services/materializacao.py:718`, `app/chamados/services/materializacao.py:770`, `app/chamados/templatetags/chamados_tags.py:139`, `app/chamados/templatetags/chamados_tags.py:166`, `app/chamados/services/fio.py:377`.
- **API, ponto e templates:** serialização, seleção de opção, preenchimento e exibição do cru. As condições operacionais dos dois templates alterados usam `.respondida`. `app/api/views.py:1163`, `app/api/views.py:2563`, `app/api/views_mensageria.py:567`, `app/ponto/services/dia_do_colab.py:70`, `app/templates/chamados/partials/_card_atendimento.html:114`, `app/templates/chamados/partials/_card_atendimento.html:141`, `app/templates/chamados/partials/_card_atendimento.html:148`, `app/templates/chamados/partials/_card_atendimento.html:154`, `app/templates/chamados/partials/_card_atendimento.html:182`, `app/templates/chamados/partials/_lote_preview.html:24`, `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/modal_fio.html:217`.

Não apareceu negação executável `~q_respondida_com_texto()` nos leitores examinados; a nova face escreve os ramos explicitamente. `app/chamados/juizes.py:1588`. Ausência por execução **[nao verificado]**.

**3. Complemento SQL e whitespace**

**Conforme por inspeção.** O conjunto de brancos vem de `str.isspace()`, e não de `\s` do PostgreSQL; Python usa `(t or '').strip()`. `app/chamados/juizes.py:1526`, `app/chamados/juizes.py:1560`.

| Valores | Positiva | Negada |
|---|---:|---:|
| Sem I, qualquer texto | Falso | Verdadeiro |
| Com I, texto NULL, `''`, somente espaços, NBSP, tabs ou LF | Falso | Verdadeiro |
| Com I e algum caractere não branco | Verdadeiro | Falso |

A tabela decorre dos ramos NULL/vazio, regex positiva e negação escrita. `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1588`.

O teste B2 contém seis pares e não testa NULL/tab/LF; a bateria anterior cobre tab/LF, NBSP e outros brancos. NULL SQL não é exercitado porque o campo é NOT NULL; `None` aparece apenas no teste Python. `app/chamados/tests/test_f1nova_b2_leitores.py:127`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:11`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:29`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:118`. Concordância executada no banco desta cópia **[nao verificado]**.

**4. Pergunta viva com I e sem T**

| Leitor | Resultado atual |
|---|---|
| App/consulta | Pendente se a disputa estiver aberta; em disputa fechada, histórico. `app/chamados/services/pergunta_no_app.py:81`. |
| Fila a validar | Não entra. `app/chamados/juizes.py:1760`. |

[truncado em 20000 de 33679 caracteres]

## 16/11 22:23 · sessao 833c837a69a0 · - · - · origem=Ronald (tmux)

Achados:

- **Médio — relógio fora da borda por novo caminho.** `impedimento_estoque_l130` não recebe `agora` e chama `pergunta_viva(perg)` sem relógio (`app/chamados/services/validacao.py:381`, `:398`). O juiz pode cair em `date.today()` (`app/chamados/juizes.py:1471`, `:596`) e passar `agora=None` a `classificar_falta` (`:1475`), que lê `_tz.now()` (`app/escala/servico_jornada.py:387`). O comando lê `timezone.localtime()` somente depois da seleção, para o arquivo de reversão, e não o transmite (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:61`). A consequência documentada exige um relógio na borda até o julgamento (`app/docs/PLANO-PADROES.md:99`–`:103`).

- **Médio — lista fechada não pertence à porta.** A porta aceita qualquer pk que satisfaça o predicado; nem o predicado nem a porta consultam `ESTOQUE_L130_ALVOS` (`app/chamados/services/validacao.py:378`, `:381`, `:408`, `:432`). O comando compara a lista antes das travas (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:53`), depois chama a porta separadamente por pergunta (`:70`–`:76`). Assim, garante igualdade na fotografia inicial, não igualdade do conjunto durante as escritas; também permite aplicação parcial se outra pergunta perder elegibilidade ou falhar. A porta individual continua segura contra perda de elegibilidade daquela pergunta porque relê o predicado travado (`app/chamados/services/validacao.py:431`–`:433`).

- **Médio — teste não prova delegação da property.** O teste compara resultados nos casos listados; uma property que copie integralmente a expressão do juiz passa por ele (`app/chamados/tests/test_f1nova_b2_leitores.py:75`–`:85`). Há casos que, por inspeção, distinguem os mutantes antigos de B, C e D (`:61`–`:69`, `:75`–`:91`, `:142`–`:147`), porém o arquivo não contém injeção de mutantes, patch do juiz ou teste que force a delegação. Mordida executada: [não verificado].

Conforme:

- Ordem C → Disputa → Chamado → Pergunta pela porta canônica: chamada em `app/chamados/services/validacao.py:429`; aquisições em `app/chamados/models.py:2771`, `:2780`, `:2793`, `:2797`.
- Releitura sob trava e uma escrita pelo descarimbo canônico: `app/chamados/services/validacao.py:431`–`:440`; único `update` do descarimbo em `app/chamados/models.py:3392`–`:3394`. Não nasce segundo escritor de descarimbo nesse trecho.
- Linha RESPOSTA registrada antes do `update`, no mesmo `atomic` (`app/chamados/services/validacao.py:428`, `:439`, `:440`). O evento vem depois, mas `None` levanta exceção dentro do mesmo `atomic`, revertendo pergunta e sequência (`:449`–`:457`). `evento` retorna registro ou `None` (`app/core/observ.py:133`–`:139`); `DecisaoPergunta.registrar` não engole falhas (`app/chamados/models.py:4123`–`:4127`).
- Idempotência individual decorre de retirar `respondida_em`: segunda chamada encontra `nao_e_instante_sem_texto` (`app/chamados/services/validacao.py:390`, `:440`). A projeção evita duplicar RESPOSTA pelo mesmo instante (`app/chamados/models.py:4098`–`:4105`). O teste verifica segunda execução do comando, não duas chamadas diretas nem rollback por falha do evento (`app/chamados/tests/test_f1nova_b2_leitores.py:238`–`:244`).
- Complemento SQL correto por inspeção para NULL, vazio, espaços, NBSP, tab e LF: Python usa `(t or '').strip()` (`app/chamados/juizes.py:1526`–`:1528`), regex deriva os caracteres de `isspace()` (`:1560`–`:1562`), vazio SQL inclui NULL (`:1572`), positivo exige I e algum não branco (`:1580`), negativo é ausência de I ou I com vazio (`:1588`). Execução no PostgreSQL desta cópia: [não verificado].
- O teste B2 da face negada tem seis pares, sem NULL/tab/LF (`app/chamados/tests/test_f1nova_b2_leitores.py:127`–`:129`). A bateria anterior cobre tab/LF, NBSP e outros brancos (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:29`–`:38`, `:89`–`:120`). NULL SQL não é exercitado porque o campo é NOT NULL; há teste de `None` somente em Python (`:11`–`:15`, `:118`–`:120`).
- `test_o168` preserva o sujeito e adiciona controle positivo legal antes das duas metades ilegais; a inversão corresponde à L-130 (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:287`–`:297`). Restou docstring histórica que afirma “nunca solta”, agora contrariada pelo próprio teste (`:276`–`:280`): **menor**, documentação.
- `test_e3_10` inverte exatamente a retenção de instante sem texto, conserva controle positivo e mantém `pode_fechar=False`; não enfraquece essa proteção (`app/chamados/tests/test_e3_10_cura_no_escritor.py:127`–`:148`).
- `test_estado_aplicacao` completa o objeto sintético com instante fixo, adequando a fixture respondida ao novo contrato; as asserções permanecem específicas (`app/chamados/tests/test_estado_aplicacao.py:37`–`:44`, `:49`–`:77`).
- K6 usa dia do fato, sem relógio implícito: `app/chamados/services/materializacao.py:784`–`:789` → `app/chamados/services/passivo_validado.py:86`–`:95`.

Nenhum teste foi executado, nenhum banco acessado e nenhum arquivo escrito. Execução e resultados de produção: [não verificado]. [nao verificado]

## 16/11 22:23 · sessao ba4d04fd8d7d · - · - · origem=Ronald (tmux)

Q2 — resíduos que deveriam ler o juiz:

- `espera_o_texto_do_colaborador` ainda recompõe `not J`: `I ausente OU respondida_sem_texto`. Hoje é equivalente, mas é derivação paralela; alimenta cobrança e cartão. `app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:187`, `app/chamados/services/cobranca.py:227`, `app/chamados/services/fio.py:359`.
- Canal partido chama seu universo de “Perguntas sem resposta” e exige `resposta_colab=''`: perde espaços, NBSP e texto sem instante. O cobrar consome esse detector. `app/chamados/canal.py:53`, `app/chamados/canal.py:65`, `app/chamados/views_cobrar.py:181`.
- Placar de aceite conta “respondidos” por `respondida_em__gte`, sem texto. É consumido pelo painel e pela Folha. `app/chamados/services/verbos.py:277`, `app/chamados/services/verbos.py:325`, `app/chamados/services/verbos.py:335`, `app/chamados/views.py:1489`, `app/chamados/services/painel.py:298`, `app/folha/views_retrato.py:515`.
- Ressuscitação seleciona materializadas com texto NULL/`''`, embora descreva seu universo como perguntas “SEM resposta”. Espaços/NBSP ficam fora; texto sem instante recebe a proteção reservada à resposta. É decisão operacional, não exibição. `app/chamados/models.py:1494`, `app/chamados/models.py:1530`, `app/chamados/models.py:1533`. Sua classificação formal E/F no censo é [não verificado].

E/F legítimos por sua função:

- Trilha/projeção: `app/chamados/models.py:4095`, `app/chamados/models.py:4102`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `app/chamados/services/historico.py:98`.
- Marcos temporais/cache: `app/chamados/veredito_lavrado.py:202`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/painel.py:64`.
- Integridade/estoque específico: `app/chamados/services/vigia_fio.py:737`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1104`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/services/disputa_emissao.py:1692`, `app/chamados/services/disputa_emissao.py:1861`. Auditor I20 e curar limbo usam `!= ''`, portanto não espelham trim, mas julgam integridade, não estado operacional. `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`.
- Preservar texto bruto/escrever metades: `app/chamados/services/disputa_emissao.py:1663`, `app/chamados/models.py:3241`.
- Parse/classificação de conteúdo: `app/chamados/services/validacao.py:467`, `app/chamados/services/validacao.py:1181`, `app/chamados/services/materializacao.py:546`, `app/chamados/services/materializacao.py:625`, `app/chamados/services/materializacao.py:718`, `app/chamados/services/materializacao.py:770`, `app/chamados/services/materializacao.py:953`, `app/chamados/services/materializacao.py:1562`.
- Fallback da testemunha por conteúdo bruto: `app/chamados/templatetags/chamados_tags.py:139`, `app/chamados/templatetags/chamados_tags.py:166`, `app/chamados/services/fio.py:377`.
- Exibição API/ponto: `app/api/views.py:1163`, `app/api/views.py:2561`, `app/api/views.py:2637`, `app/api/views_mensageria.py:567`, `app/chamados/views_cobrar.py:292`, `app/ponto/services/dia_do_colab.py:70`, `app/ponto/services/veredito_celula.py:197`. A API de validação recebe seu universo do juiz. `app/api/views_mensageria.py:539`.
- Templates: seleção/exibição do cru em `_card_atendimento.html:114`, `:141`, `:148`, `:154`, `:182`; datas sob `resposta_enviada` em `:106` e `:391`; preview cru em `_lote_preview.html:24`; os dois templates migrados guardam a resposta com `.respondida`. `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/modal_fio.html:217`.

Q4 — pergunta viva I sem T:

| Leitor | Resultado |
|---|---|
| App | Pendente se disputa aberta. `app/chamados/services/pergunta_no_app.py:81` |
| Fila validar | Não entra. `app/chamados/juizes.py:1760` |
| Estado | Pendente. `app/chamados/models.py:3156` |
| Retenção | Não integra respostas sem veredito. `app/chamados/models.py:1923` |
| Pode fechar | Não fecha enquanto fio vivo. `app/chamados/models.py:1934` |
| CSF | Essa pergunta não segura pelo predicado de conversa. `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:588` |
| Órfã | Ausência de resposta permite continuar; ainda exige fato=True, C6 e teto. `app/chamados/reconciliador.py:1085`, `app/chamados/reconciliador.py:1087`, `app/chamados/reconciliador.py:1091` |
| Pauta DP | Não entra por resposta sem veredito; só mede trancada, não mera exportada. `app/chamados/services/competencia_trancada.py:95`, `app/chamados/services/competencia_trancada.py:126`, `app/chamados/services/competencia_trancada.py:137` |
| Cobrar | Conta como esperando colaborador, sujeito ao C6/canal/prazo. `app/chamados/services/cobranca.py:241`, `app/chamados/services/cobranca.py:252`, `app/chamados/services/cobranca.py:255` |
| Vigia | Pode classificar causa b, depois de precedências anteriores. `app/chamados/services/vigia_fio.py:628`, `app/chamados/services/vigia_fio.py:634` |

Tarefa/dono não decorre apenas de I/T: última linha RESPOSTA vazia exige responder/colaborador; projeção sem linha gera desconhecimento; linhas posteriores podem mudar o próximo ato. `app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:159`, `app/chamados/services/tarefa_do_dia.py:163`. O dono colab exige também presença entre pendentes do app e ação responder. `app/chamados/services/tarefa_do_dia.py:651`.

Existe oposição condicional: placar diz “respondido” e app diz “pendente” para I sem T cujo instante esteja na janela pós-cobrança. `app/chamados/services/verbos.py:335`, `app/chamados/services/verbos.py:342`, `app/chamados/services/pergunta_no_app.py:81`. A ocorrência dessa janela nas pk reais é [não verificado].

## 16/11 22:23 · sessao 00869dd13935 · - · - · origem=Ronald (tmux)

Q6 — há caminhos de escrita que alcançam competência entregue; o comando novo não protege os demais consumidores. A ocorrência nas 141 perguntas do dado não pode ser afirmada sem conhecer suas datas, âncoras e lâmpadas [não verificado].

| Consumidor | Mudança e escrita possível | Entregue |
|---|---|---|
| **CSF / supra_juiz** | `sem_veredito_entre` deixa de reter I sem T/T sem I (`app/chamados/models.py:1923`). A lista compartilhada libera CSF (`app/ponto/supra_juiz.py:573`, `:588`); o executor chama `retratar` (`app/ponto/management/commands/supra_juiz.py:261`, `:286`). `_retratar` fecha chamado, pode rejeitar justificativa e chama resolver (`app/chamados/reconciliador.py:520`, `:525`, `:531`, `:538`). | **Sim, potencial.** O gate de pergunta viva exclui entregue explicitamente (`app/chamados/models.py:364`); portanto não substitui a retenção removida. O cron `--executar` está configurado (`app/config/crons.py:599`), mas sua janela padrão é atual (`app/ponto/management/commands/supra_juiz.py:90`). Alcance nas 141 [não verificado]. |
| **Pauta após fechamento** | S143 passa I sem T para `reabrir_credor` (`app/chamados/services/disputa_emissao.py:1843`, `:1969`), que recusa entregue por K6 (`:1857`, `:1867`) e escreve nota de recusa (`:1879`). O resolver coleta (`:1973`); `transicionar_com_pauta` escreve Pauta no mesmo ato (`app/chamados/services/fecho_runtime.py:161`, `:168`, `:139`). | **Sim, potencial**, após fechamento agora liberado pela retenção B; nenhum descarimbo é necessário para nascer essa Pauta. |
| **Cartório** | A isenção D passa de T para J (`app/ponto/services/cartorio.py:1641`); T sem I deixa de isentar o chamado e pode liberar dedup/fecho (`:1652`, `:1667`) ou tentativa de ressuscitação (`:1674`). | **Não há veto K6 nesse bloco.** A ressuscitação continua selecionando somente texto NULL/`''` (`app/chamados/models.py:1530`), então T sem I isolado não é descarimbado; irmãs vazias no mesmo chamado podem ser atingidas (`:1600`, `:1629`). O comentário reconhece alcance exportado histórico (`app/ponto/services/cartorio.py:1626`). |
| **Reconciliador — lavrar_sem_turno_aberto** | D passa a J (`app/chamados/reconciliador.py:365`); T sem I pode chegar ao carimbo `sem_turno_aberto` e ao fechamento da disputa (`:407`, `:413`). | **Não existe K6 neste caminho**; depende de ausência de turno e C6 morto (`:373`, `:399`). O carimbo escreve materialização/via (`app/chamados/models.py:3239`, `:3263`), sem tocar o par quando `resposta=None` (`:3241`). |
| **Reconciliador — veredito_orfa** | D passa a J (`app/chamados/reconciliador.py:1084`); T sem I pode produzir `ENCERRA` após lâmpada, C6 e teto (`:1087`, `:1093`). O consumidor chama `encerrar_fantasma` e evento (`:1138`, `:1150`, `:1152`), podendo fechar disputa (`:1173`). | **Sem veto K6 neste caminho**. `encerrar_fantasma` grava pela porta de carimbo (`app/chamados/models.py:3415`). Para I sem T a condição D já era falsa antes: a troca D não cria delta próprio nesse caso (`app/chamados/reconciliador.py:1085`). |
| **Becos** | O leitor C usa property (`app/chamados/services/becos.py:29`); I sem T torna-se visível quando destino não terminal, reduzindo candidatos. Havendo destino terminal, `pode_fechar` ainda decide (`:33`). O ato resolve perguntas e fecha disputa (`:54`, `:56`). | A retirada da retenção B pode liberar disputa cuja pergunta esteja morta pelo C6, inclusive trancada (`app/chamados/models.py:1934`). **O cron instalado usa `--alarme`, sem escrita** (`app/config/crons.py:663`); a porta por evento continua escrevendo (`app/chamados/services/becos.py:74`, `:78`). |
| **Materialização** | A seleção C agora exclui I sem T (`app/chamados/services/materializacao.py:2067`); reduz plantios. D `_materializar_resposta_hora` agora recusa T sem I com escrita de recusa (`:1314`, `:1322`). | A guarda K6 é específica para metades de intervalo fora163 (`:792`, `:805`), não geral. A fila pública não traz mais esses pares (`:2069`), mas a unidade `_materializar_uma` também tem consumidor no passivo (`app/chamados/services/passivo_validado.py:39`). Não afirmar novo plantio: a migração endurece anuência. |
| **Vigia fio** | Usa J no subtipo c, mantendo antes o subtipo b para I sem T (`app/chamados/services/vigia_fio.py:634`, `:636`). | Esse trecho somente conta/lista e retorna (`:656`, `:660`). Não é escritor. |
| **views_cobrar** | I sem T sai do lote validar e entra em aguardando colaborador (`app/chamados/views_cobrar.py:169`, `:175`, `:197`). | **Sem nova escrita para o par ilegal**; validação só é chamada para `pendentes` J (`:202`, `:209`), em POST humano (`:151`). |
| **Dedup / backfill decisões** | A escolha canônica e mérito mudam T para J (`app/chamados/services/fio.py:1281`, `:1294`). O apply do dedup delega à porta do backfill (`app/chamados/management/commands/dedup_perguntas_fato.py:105`). | **O apply preserva grupos exportados**, com lista e saída PAREI (`:112`, `:132`); o contrato do veto aparece no próprio chamador (`:102`). |

Pauta da tranca: `medir` só trabalha se `periodo_trancado` existir (`app/chamados/services/competencia_trancada.py:95`); pede `respostas_sem_veredito(ignorar_tranca=True)` (`:126`). I sem T sai de `decidir_pelo_dp` (`:137`), podendo liberar `pode_fechar` se C6 também disser morto (`:178`); `encerrar` escreve chamado/disputa (`:209`, `:229`) e Pauta somente se sobrar lista (`:232`). **Exportada e trancada são estados distintos no próprio contrato** (`:5`); o dado não prova quantas das 141 são trancadas [não verificado].

Push: a migração de `recobrar_questionario` altera a contagem de pendentes (`app/chamados/services/disputa_emissao.py:1735`); o mesmo serviço envia push e grava nota (`:1748`, `:1758`), sem veto K6 nesse caminho. É ação de recobrança, não consequência demonstrada de cron [não verificado].

## 16/11 21:54 · sessao ff8736dc078a · E4C-F1-B2 · censo · origem=Code

A cópia contém o juiz da B1, mas o corpo de retenção ainda usa **OU**, a property `respondida` ainda usa **só instante**, e as entradas de validar/recusar também continuam por instante. A B2 permanece registrada como construção dos leitores B/C/D. (`app/chamados/juizes.py:1551`; `app/chamados/models.py:1924`; `app/chamados/models.py:3665`; `app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:813`; `app/docs/BACKLOG.md:821`.)

O pouso c4+c4c, merge `41f5648af`, está registrado na documentação da cópia. Correspondência independente da árvore com `520d7b557`, diff Git contra `71e9576bf`, execução dos testes e confirmação de produção: **[nao verificado]**. (`app/docs/BACKLOG.md:2081`.)

**1. CENSO**

`I` = instante presente; `T` = texto não vazio após trim; `J` = `I E T`, pelo juiz `pergunta_respondida`, com face SQL `q_respondida_com_texto`. (`app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1580`.)

O fechamento abaixo cobre os **66 usos B/C/D identificados pelos R do anexo**, mais **14 delegações complementares** explicitadas depois das tabelas. São ocorrências nomeadas, não 80 predicados independentes. Cobertura de despacho dinâmico arbitrário: **[nao verificado]**.

| R | B — leitura atual | Evidência no HEAD fornecido |
|---|---|---|
| 24 | Irmãs delegam ao corpo sem veredito | `app/chamados/juizes.py:2105` |
| 27 | Auditor I12 delega às irmãs | `app/chamados/management/commands/auditar_invariantes_chamados.py:984` |
| 38 | Comando consulta respostas sem veredito | `app/chamados/management/commands/validar_classe_a.py:54` |
| 42 | **Corpo:** sem validação/materialização, `I OU T`, com tranca opcional | `app/chamados/models.py:1922`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926` |
| 43 | Lista da disputa delega ao corpo | `app/chamados/models.py:1906` |
| 44 | `pode_fechar`: lista vazia e ausência de fio vivo | `app/chamados/models.py:1934` |
| 45 | `fechar` consulta a lista | `app/chamados/models.py:1967` |
| 60 | Retenção da retratação consulta irmãs | `app/chamados/reconciliador.py:448` |
| 62 | Encerramento de órfãs consulta a lista | `app/chamados/reconciliador.py:1164` |
| 69 | Medição da tranca consulta lista ignorando tranca | `app/chamados/services/competencia_trancada.py:126` |
| 80 | Resolvedor consulta irmãs antes de reabrir pai | `app/chamados/services/disputa_emissao.py:2031` |
| 85 | Gaveta consulta respostas sem veredito | `app/chamados/services/fila_por_causa.py:226` |
| 104 | Reconciliação do destino consulta irmãs | `app/chamados/services/materializacao.py:1244` |
| 133 | Desligamento consulta a lista | `app/colaboradores/services/desligamento.py:165` |
| 134 | Ficha consulta a lista dentro das vivas | `app/colaboradores/services/ficha.py:169` |
| 142 | CSF consulta o mesmo corpo nos universos pai/destino, ignorando tranca | `app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574` |

| R | C — leitura atual: instante | Evidência no HEAD fornecido |
|---|---|---|
| 4 | Respondidas no histórico da API | `app/api/views.py:2577` |
| 5 | `ja_respondida` na resposta da API | `app/api/views.py:2620` |
| 18 | Corrigíveis, com lastro/vitalidade adicionais | `app/chamados/juizes.py:1744` |
| 25 | Contraprova I21a | `app/chamados/management/commands/auditar_invariantes_chamados.py:177` |
| 26 | I2 | `app/chamados/management/commands/auditar_invariantes_chamados.py:592` |
| 30 | C2 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1605` |
| 49 | Saídas pendente/respondida de `estado` | `app/chamados/models.py:3155`; `app/chamados/models.py:3158` |
| 56 | **Property ainda não delega:** instante não nulo | `app/chamados/models.py:3665` |
| 58 | Reconstrução histórica permite ato RESPOSTA pelo instante | `app/chamados/models.py:4091`; `app/chamados/models.py:4098` |
| 64 | Contagem de pendentes na reabertura | `app/chamados/services/acoes_disputa.py:97` |
| 65 | Viva sem instante impede regularizar | `app/chamados/services/acoes_disputa.py:132` |
| 67 | Classificação de beco | `app/chamados/services/becos.py:30` |
| 71 | Instante informa reenvio da declaração | `app/chamados/services/declaracao_texto.py:419` |
| 72 | Ausência de instante dispara gravação da declaração | `app/chamados/services/declaracao_texto.py:687` |
| 76 | Contador da recobrança | `app/chamados/services/disputa_emissao.py:1735` |
| 77 | Instante presente impede reabertura do credor por essa porta | `app/chamados/services/disputa_emissao.py:1843` |
| 89 | Viva sem instante impede regularização pelo fio | `app/chamados/services/fio.py:706` |
| 94 | Instante e limite `desde` selecionam evento histórico | `app/chamados/services/historico.py:105`; `app/chamados/services/historico.py:106` |
| 107 | Seleção da materialização: instante, validação e ausência de materialização | `app/chamados/services/materializacao.py:2064`; `app/chamados/services/materializacao.py:2065`; `app/chamados/services/materializacao.py:2066` |
| 111 | Partição pendente do app | `app/chamados/services/pergunta_no_app.py:81` |
| 116 | Validar retorna `sem_resposta` sem instante | `app/chamados/services/validacao.py:594` |
| 117 | Recusar retorna `ja_pendente` sem instante | `app/chamados/services/validacao.py:813` |
| 121 | Contador temporal: sete dias e posterior à cobrança | `app/chamados/services/verbos.py:335`; `app/chamados/services/verbos.py:342`; `app/chamados/services/verbos.py:345` |
| 123 | Causa `c` do vigia sob âncora terminal | `app/chamados/services/vigia_fio.py:636` |
| 128 | `ja_respondida` na view | `app/chamados/views.py:1280` |
| 129 | Reabertura seleciona instante nulo e depois aplica C6 | `app/chamados/views.py:1416`; `app/chamados/views.py:1417` |
| 130 | Instante presente para validar; nulo para aguardar colaborador | `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176` |
| 137 | Relavrar órfã seleciona instante presente | `app/ponto/management/commands/reconciliar_grade.py:461` |
| 143 | Contadores operacionais nulo/presente | `bin/censo_folha.py:28`; `bin/censo_folha.py:29` |
| 148 | Instante governa rótulo e caixa do cartão | `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29` |
| 150 | Instante governa correção da cega e caixa no modal | `app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217` |

| R | D — leitura atual: texto | Evidência no HEAD fornecido |
|---|---|---|
| 8 | SQL com vazio exato, sem trim | `app/chamados/canal.py:65` |
| 31 | Texto após strip interrompe F1 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1657` |
| 34 | Texto carregado alimenta eleição da canônica | `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:506` |
| 37 | Texto após strip; depois eleição da canônica | `app/chamados/management/commands/dedup_perguntas_fato.py:75`; `app/chamados/management/commands/dedup_perguntas_fato.py:76` |
| 40 | Ressuscitação seleciona texto NULL ou vazio exato | `app/chamados/models.py:1533` |
| 59 | Texto impede lavrar sem turno | `app/chamados/reconciliador.py:365` |
| 61 | Texto impede ramo de órfã | `app/chamados/reconciliador.py:1083` |
| 74 | Texto existente impede resposta automática | `app/chamados/services/disputa_emissao.py:1663` |
| 81 | `_tem_resp` pelo strip | `app/chamados/services/disputa_emissao.py:2079` |
| 90 | Canônica favorece texto | `app/chamados/services/fio.py:1279` |
| 91 | Mérito perdedor exige texto e via admin | `app/chamados/services/fio.py:1290`; `app/chamados/services/fio.py:1291` |
| 99 | Testemunha exige tipo hora e texto, **depois das vias de recusa** | `app/chamados/services/materializacao.py:862`; `app/chamados/services/materializacao.py:869` |
| 105 | Plantio de hora recusa texto vazio | `app/chamados/services/materializacao.py:1311` |
| 126 | Leitura da resposta sai vazia sem texto após strip | `app/chamados/templatetags/chamados_tags.py:166` |
| 132 | Calendário exclui vazio exato | `app/colaboradores/services/calendario.py:504` |
| 135 | Pendência da ficha pelo texto vazio, fora de `falou` | `app/colaboradores/services/ficha.py:171` |
| 136 | Evidência de fala na prévia | `app/folha/previa.py:142` |
| 138 | Evidência de fala no cartório | `app/ponto/services/cartorio.py:1641` |
| 145 | Verdade crua do texto no cartão do colaborador | `app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:182` |

As **14 delegações complementares** fecham o mapa nominal. As quatro marcadas abaixo faltavam também no b2.r; não atribuo seu nascimento à c4 sem o diff histórico, **[nao verificado]**.

| Classe | Delegações complementares |
|---|---|
| B transitivo — 1 | Gate do pai → CSF: `app/chamados/models.py:719` |
| C — 4 | Contador: `app/chamados/templatetags/chamados_tags.py:226`; **acréscimos ao b2.r:** ação `app/chamados/services/acoes_disputa.py:99`, reabertura pelo colaborador `app/chamados/models.py:2309`, view `app/chamados/views.py:1420` |
| D — 9 | Canônica em declaração: `app/chamados/services/declaracao_texto.py:253`; `app/chamados/services/declaracao_texto.py:275` |
| D — continuação | Canônica no fio: `app/chamados/services/fio.py:1003`; `app/chamados/services/fio.py:1900`; `app/chamados/services/fio.py:2223`; mérito: `app/chamados/services/fio.py:1335` |
| D — continuação | Canônica no backfill: `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:393`; **acréscimo ao b2.r:** nascimento `app/chamados/models.py:3031` |

No código da BUG-PAR há **cinco consumidores que já usam J ou sua face SQL**. Eles entram na conferência dos efeitos, sem nova troca de predicado nesses sítios. (`app/chamados/services/materializacao.py:146`; `app/chamados/services/materializacao.py:192`; `app/chamados/services/materializacao.py:259`; `app/chamados/services/materializacao.py:290`; `app/chamados/management/commands/bug_par_103.py:71`.)

| Consumidor | Leitura atual |
|---|---|
| Seleção da irmã respondida | SQL J — `app/chamados/services/materializacao.py:146` |
| Resposta da metade no julgamento do par | J — `app/chamados/services/materializacao.py:192` |
| Elegibilidade da devolução U103 | J, pertencimento congelado, via/carimbo/corte/lâmpada — `app/chamados/services/materializacao.py:259` |
| Situação da irmã | J — `app/chamados/services/materializacao.py:290` |
| Lista fora163 no comando | SQL J — `app/chamados/management/commands/bug_par_103.py:71` |

As leituras novas de texto para **disponibilidade da testemunha**, **escopo da espera** e **interpretação/comparação do declarado** têm finalidade E; não são cópias alternativas de “está respondida?”. A troca indiscriminada dessas guardas por J não corresponde à migração B/C/D descrita no juiz. (`app/chamados/juizes.py:1551`; `app/chamados/services/fio.py:377`; `app/chamados/templatetags/chamados_tags.py:139`; `app/chamados/services/materializacao.py:770`; `app/chamados/services/materializacao.py:718`; `app/chamados/services/materializacao.py:730`.)

O delta relevante dos **oito arquivos cruzados** é:

| Arquivo | O que muda no brief em relação ao b2.r |
|---|---|
| `acoes_disputa.py` | C64/C65 agora em `app/chamados/services/acoes_disputa.py:97` e `app/chamados/services/acoes_disputa.py:132`. A mensagem de sucesso agora vem do serviço e pode nomear a irmã gravada com o par. (`app/chamados/services/acoes_disputa.py:60`; `app/chamados/services/acoes_disputa.py:64`.) |
| `fio.py` | C89 agora em `app/chamados/services/fio.py:706`; canônica/mérito em `app/chamados/services/fio.py:1279` e `app/chamados/services/fio.py:1291`. A testemunha ausente ou que falha passa a produzir `indisponivel`. (`app/chamados/services/fio.py:377`; `app/chamados/services/fio.py:384`.) |
| `materializacao.py` | B104/D99/D105/C107 agora em `app/chamados/services/materializacao.py:1244`, `app/chamados/services/materializacao.py:869`, `app/chamados/services/materializacao.py:1311` e `app/chamados/services/materializacao.py:2064`. A B2 encontra par, K6, espera e vias de recusa antes de chegar às antigas decisões simples. (`app/chamados/services/materializacao.py:192`; `app/chamados/services/materializacao.py:810`; `app/chamados/services/materializacao.py:812`; `app/chamados/services/materializacao.py:862`.) |
| `validacao.py` | Entradas C agora em `app/chamados/services/validacao.py:594` e `app/chamados/services/validacao.py:813`. Validar compõe K6 da pergunta/irmã, pode validar o par e preserva o motivo F6 após rollback. (`app/chamados/services/validacao.py:225`; `app/chamados/services/validacao.py:554`; `app/chamados/services/validacao.py:562`.) |
| `chamados_tags.py` | D126 agora em `app/chamados/templatetags/chamados_tags.py:166`; contador delegado em `app/chamados/templatetags/chamados_tags.py:226`. O cartão ganhou adaptador da testemunha com fallback restritivo. (`app/chamados/templatetags/chamados_tags.py:136`; `app/chamados/templatetags/chamados_tags.py:139`.) |
| `test_estado_aplicacao.py` | A fixture continua sem `respondida_em`; agora fornece também campos do gate de data. A chamada direta de J ainda exigirá completar essa fixture. (`app/chamados/tests/test_estado_aplicacao.py:38`; `app/chamados/tests/test_estado_aplicacao.py:41`; `app/chamados/juizes.py:1552`.) |
| `_pergunta_card.html` | Os C de instante permanecem; dentro deles há agora testemunha, estados K6 próprios, indisponibilidade e espera sem Validar. (`app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/_pergunta_card.html:43`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/_pergunta_card.html:49`.) |
| `modal_fio.html` | Os C de instante permanecem; os estados próprios e a espera agora participam da decisão de esconder Validar. (`app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217`; `app/templates/chamados/partials/modal_fio.html:274`.) |

Há, portanto, **interseção comportamental nova comprovada**: a mudança em R99 alcança a testemunha consumida pelo fio, pelo cartão e pelo explicador; a entrada C de validar encontra uma operação capaz de validar/materializar também a irmã. Isso amplia a conferência além das linhas antigas, sem demonstrar necessidade de um nono arquivo de produção no diff. (`app/chamados/services/fio.py:376`; `app/chamados/templatetags/chamados_tags.py:136`; `app/chamados/services/explicador.py:124`; `app/chamados/services/validacao.py:554`; `app/chamados/services/validacao.py:555`.)

**2. TABELA DE ESTADOS**

A tabela é do **código atual**, antes da B2. Validar testa instante antes de validação existente; recusar testa validação existente antes de instante. A apresentação de `estado` prioriza validação, depois vitalidade e materialização. (`app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:597`; `app/chamados/services/validacao.py:810`; `app/chamados/services/validacao.py:813`; `app/chamados/models.py:3151`; `app/chamados/models.py:3154`; `app/chamados/models.py:3156`.)

| Situação | Quem julga | O que a porta faz | O que persiste | Estado | Tela |
|---|---|---|---|---|---|
| Validar: ID inválido/fora da disputa | Busca de `validar_pergunta`, `app/chamados/services/validacao.py:586` | Recusa | Nenhuma escrita nesse ramo | `nao_encontrada`, `app/chamados/services/validacao.py:592` | “Pergunta inválida”, `app/chamados/services/acoes_disputa.py:24` |
| Validar: **sem I**, com ou sem T | Guarda C, `app/chamados/services/validacao.py:594` | Recusa antes de conferir validação existente | Nenhuma escrita nesse ramo | `sem_resposta` | “Aguardando resposta do colab”, `app/chamados/services/acoes_disputa.py:26` |
| Validar: I e validação existente | `app/chamados/services/validacao.py:597` | Não repete validação | Conserva registro | `ja_validada` | Informação de já validada, `app/chamados/services/acoes_disputa.py:28` |
| U163 de intervalo: gravaria hora/tipo diferente do declarado, onde a testemunha diria aplicável | Testemunha/espera, `app/chamados/services/materializacao.py:741`; `app/chamados/services/validacao.py:611` | Recusa antes do implausível e das escritas | Conserva pergunta | `espera_declarado_x_gravado`, `app/chamados/services/validacao.py:613` | Rótulo de espera; sem Validar nos dois cartões, `app/templates/chamados/partials/_pergunta_card.html:49`; `app/templates/chamados/partials/modal_fio.html:274` |
| Implausível sem confirmação, após as saídas anteriores | `motivo_implausivel_validacao`, `app/chamados/services/validacao.py:615` | Recusa antes do carimbo | Nenhuma escrita nesse ramo | `implausivel` | Aviso do serviço, `app/chamados/services/acoes_disputa.py:30` |
| Metade fora163, **sem dia do fato**, após as saídas anteriores | `entrega_que_barra_a_validacao` → K6, `app/chamados/services/validacao.py:652`; `app/chamados/services/materializacao.py:810` | Recusa antes do carimbo; K6 não é consultado sem dia | Conserva pergunta/atos | `bloqueada_sem_dia_do_fato`, `app/chamados/services/validacao.py:233` | Mostra motivo; sem Validar, `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/modal_fio.html:274` |
| Metade fora163 com competência do fato **ENTREGUE**, inclusive irmã abrangida pelo ato | Mesma composição, `app/chamados/services/validacao.py:225`; `app/chamados/services/materializacao.py:812` | Recusa antes do carimbo | Conserva pergunta/atos | `bloqueada_competencia_entregue`, `app/chamados/services/validacao.py:234` | Mostra preservação para Pauta DP; sem Validar, `app/chamados/services/materializacao.py:813`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/modal_fio.html:274` |
| Competência apenas fechada/aprovada, F6, sem K6 impeditivo | Testemunha consulta F6; porta conserva o toque, `app/chamados/services/materializacao.py:830`; `app/chamados/services/validacao.py:212` | Oferece toque; plantio pode recusar no chokepoint | No caso do par recusado, savepoint volta e motivo F6 é gravado pela recusa canônica | Testemunha `bloqueada_competencia`; porta pode retornar `nao_gravou` com devolução, `app/chamados/services/validacao.py:562`; `app/chamados/services/validacao.py:727` | **Conserva Validar**; informa motivo, `app/templates/chamados/partials/_pergunta_card.html:48`; prova estática em `app/chamados/tests/test_bug_par_volta.py:249` |
| Testemunha ausente/falhou na montagem da tela | Adaptadores do fio/cartão, `app/chamados/services/fio.py:377`; `app/chamados/services/fio.py:384`; `app/chamados/templatetags/chamados_tags.py:139` | A apresentação não oferece Validar | Nenhuma escrita pelos adaptadores | `indisponivel` **da testemunha** | Aviso e ausência de botão, `app/templates/chamados/partials/_pergunta_card.html:45`; `app/templates/chamados/partials/modal_fio.html:274` |
| Folga sem dia | `_aceitar_a_folga`, `app/chamados/services/validacao.py:414` | Recusa | Nenhuma escrita nesse ramo | `folga_sem_dia` | Mensagem do serviço pela casca, `app/chamados/services/validacao.py:416`; `app/chamados/services/acoes_disputa.py:50` |
| Folga já declarada | `_aceitar_a_folga`, `app/chamados/services/validacao.py:424` | Valida, encerra pergunta e pede fecho do fio | Validação/ato e carimbo; não redeclara o dia, `app/chamados/services/validacao.py:432`; `app/chamados/services/validacao.py:440` | `folga_ja_declarada` | Mensagem de dia já declarado, `app/chamados/services/validacao.py:446`; `app/chamados/services/acoes_disputa.py:56` |
| Folga recusada na checagem ou escrita | `app/chamados/services/validacao.py:450`; `app/chamados/services/validacao.py:480` | Recusa; falha da escrita reverte atomic | Conserva pergunta após recusa | `folga_recusada`, `app/chamados/services/validacao.py:453`; `app/chamados/services/validacao.py:494` | Motivo do serviço, `app/chamados/s

[truncado em 20000 de 36092 caracteres]

## 16/11 21:51 · sessao 09356da2f63b · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**PROCEDENTE a ressalva ao cumprimento integral:** a contagem facetada está implementada, mas o literal “qualquer requisição” tem contraexemplo nos hrefs; também entrou um selo AST adicional apesar de “nenhum instrumento novo”. (`app/ponto/views.py:2281-2291`; `app/templates/ponto/ausencias.html:48`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:492-546`; `complemento3_ausencia_efeito_1611.txt:8`.)

Execução da suíte Django, morte efetivamente executada dos mutantes, SQL medido e ocorrências produtivas: **[nao verificado]**. Os dois métodos do selo AST passaram quando extraídos e executados em memória, sem Django/BD; essa verificação alcança somente suas asserções estáticas. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:526-546`.)

**1. CHECKLIST**

“Cumpre” abaixo significa conformidade demonstrável na implementação ou nos asserts; não certifica execução verde. **[nao verificado]** [nao verificado]

| Cláusula | Veredito | Onde e por quê |
|---|---|---|
| C3 MUDA (a) | **Cumpre**, no âmbito lista/chips | As duas portas concentram os cortes e atendem pares em memória e queryset. Existe uma consulta separada de alertas com `status='aprovada'`, tratada no item 3(e). (`app/ponto/views.py:2251-2271,2296-2299`.) |
| C3 MUDA (b) | **Cumpre** | Todos e chips de status somam sobre `_sob_efeito`. (`app/ponto/views.py:2281-2287`.) |
| C3 MUDA (c) | **Cumpre** | Chips de efeito somam sobre `_sob_status`. (`app/ponto/views.py:2282,2288-2289`.) |
| C3 MUDA (d) | **Cumpre** | A lista recebe exatamente `por_efeito(por_status(_base, status_filtro), efeito_filtro)`. (`app/ponto/views.py:2291`.) |
| C3 MUDA (e) | **Cumpre no delta fornecido** | Os hrefs conservam a outra dimensão e `q_universo`; o template não integra o delta apresentado. (`app/templates/ponto/ausencias.html:48-67`; `app/ponto/views.py:2220-2230,2339`.) |
| C3 RESULTADO 5 | **Cumpre em parte** | Fecha para os valores usuais; falha para valor desconhecido contendo separador de query, conforme contraexemplo do item 4. (`app/ponto/views.py:2260-2262,2281-2283`; `app/templates/ponto/ausencias.html:48`.) |
| C3 RED 1 | **Cumpre no teste escrito** | Exige Suprime `(0,0)` na origem Rejeitada. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:389-391`.) |
| C3 RED 2 | **Cumpre no teste escrito** | Exige Rejeitada `(1,1)` na origem Aprovada. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:393-395`.) |
| C3 RED 3 | **Cumpre a enumeração pedida** | São 24 origens, sete chips obrigatórios e navegação pelo href de cada um, com datas válidas. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:423-440`.) |
| M-a, corrigido pelo C4 | **Cumpre logicamente** | Contar efeito sobre base crua produz Suprime 1 onde o RED 1 exige 0. Execução mutante **[nao verificado]**. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-391`; `complemento4_ausencia_efeito_1611.txt:3`.) |
| M-b | **Cumpre logicamente** | Conservar o corte Aprovada no contador Rejeitada produz 0 onde o RED 2 exige 1. Execução mutante **[nao verificado]**. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-395`.) |
| C3 SELO | **Cumpre em parte** | RED 3 existe, mas também foi acrescentado `SeloPortasDaFacetadaTest`, instrumento distinto do laço. No literal, contraria “nenhum instrumento novo”. (`complemento3_ausencia_efeito_1611.txt:8`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:418-440,492-546`.) |
| C3 PROIBIDO, após C4 | **Cumpre quanto aos cortes e contagens examinados** | Cortes das dimensões permanecem nas portas; `pendentes` e orçamento das contagens seguem as substituições expressas do C4. A ressalva do instrumento adicional permanece acima. (`app/ponto/views.py:2251-2291`; `complemento4_ausencia_efeito_1611.txt:4-5`.) |
| C4 (a) | **Cumpre** | Todos conta sob efeito, sem conservar status; a partição anterior continua exigida sem cortes. O exemplo literal possui asserts próprios. (`app/ponto/views.py:2281-2289`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:402-415`.) |
| C4 (b) | **Cumpre no teste escrito** | Acrescentado RED M-c: sob Suprime, Rejeitada `(0,0)` e Todos `(1,1)`. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:397-400`.) |
| C4 (c) | **Cumpre** | `pendentes = por_status(_base, 'pendente').count()`. (`app/ponto/views.py:2277`.) |
| C4 (d) | **Cumpre na estrutura do código** | Uma consulta agrupa por `(tipo,status)` e classifica pelo juiz; todos os chips somam `n`. O juiz usa esses dois campos e catálogo vigente. (`app/ponto/views.py:2175-2178,2273-2289`; `app/ponto/services/dia_decidido.py:467-495`.) |

**Algo não pedido no delta — PROCEDENTE para o selo adicional.** Não há alteração apresentada de template, juiz, catálogo, modelo/status, CSS ou dados persistidos; as alterações funcionais examinadas ficam na montagem dos pares e facetas. O acréscimo estrutural não solicitado é o varredor e sua classe de selo. (`app/ponto/views.py:2165-2291`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:477-546`; `complemento3_ausencia_efeito_1611.txt:8`.) Correspondência independente entre o delta transcrito e o hash anunciado: **[nao verificado]**.

**2. QUEM ESCREVE E QUEM LÊ**

| Valor | Escritor/origem | Leitor, incluindo fora do delta |
|---|---|---|
| GET `status` | Chips de status escrevem os valores; chips de efeito e formulário preservam o recebido. (`app/templates/ponto/ausencias.html:52-67,75`.) | Leitura inicial em `status_filtro`; usada na faceta de efeito, lista e contexto. (`app/ponto/views.py:2191,2282,2291,2337`.) |
| GET `efeito` | Chips de efeito escrevem `abona/desconta/suprime`; chips de status preservam o recebido. (`app/templates/ponto/ausencias.html:48-67`.) | Leitura em `efeito_filtro`; usada na faceta de status, lista e contexto. (`app/ponto/views.py:2236,2281,2291,2335`.) |
| `_pares.tipo/status` | Saem de `values('tipo','status')` sobre `_base`. (`app/ponto/views.py:2177-2178,2240`.) | O juiz lê ambos; `por_status` lê status; `por_efeito` usa ambos para reconstruir os pares SQL. (`app/ponto/views.py:2177,2256,2270`.) |
| `_pares.n` | `Count('pk')` do agrupamento. (`app/ponto/views.py:2178`.) | `_soma`, chamada por todos os chips. (`app/ponto/views.py:2273-2289`.) |
| `_pares.veredito` | `DD.veredito_do_tipo(tipo,status)`. (`app/ponto/views.py:2177`.) | `por_efeito`, nos dois ramos. (`app/ponto/views.py:2263-2271`.) |
| `_status_do_filtro` | Mapa local; Pendente deriva de `_A4_ABERTAS`, importada da autoridade das filas. (`app/ponto/views.py:79,2245-2246`; `app/ponto/services/ausencia.py:912`.) | Somente `por_status` resolve esse mapa. (`app/ponto/views.py:2252`.) |
| `_veredito_do_filtro` | Deriva do campo `filtro` do mapa de apresentação do juiz. (`app/ponto/views.py:2242`; `app/ponto/services/dia_decidido.py:80-84`.) | `por_efeito` e laço dos contadores de efeito. (`app/ponto/views.py:2260,2288-2289`.) |
| `pendentes` | Contagem separada da caixa aberta sobre `_base`; entregue ao contexto. (`app/ponto/views.py:2277,2325`.) | Leitor explícito encontrado: `C4cPendentesTest`. O chip usa `count_pendente`, outro valor. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:443-448`; `app/templates/ponto/ausencias.html:54`.) |
| `_cnt` / `cnt` / `count_*` | Somas facetadas; aliases saem do mesmo `_cnt`. (`app/ponto/views.py:2281-2289,2335-2336`.) | Template lê quatro aliases de status e três `cnt.*`; testes anteriores leem contexto e HTML. (`app/templates/ponto/ausencias.html:50-67`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:146-152,179-210`.) |
| `filtro_status` / `filtro_efeito` | Contexto recebe os valores crus dos GET. (`app/ponto/views.py:2335-2337`.) | Template usa nos hrefs, marcação ativa e hidden de status. (`app/templates/ponto/ausencias.html:48-67,75`.) |
| Lista `ausencias` | Portas produzem `qs`; ele vira lista e recebe atributos de apresentação. (`app/ponto/views.py:2291,2313-2324`.) | Template percorre as linhas; testes anteriores inspecionam os registros. (`app/templates/ponto/ausencias.html:142-179`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:128-129,223-224`; `app/ponto/tests/test_lista_ausencias.py:19-31`.) |
| Retorno de `_pares_por_veredito` | Agora é lista de dicionários com quatro chaves. (`app/ponto/views.py:2177-2178`.) | Única chamada produtiva localizada: atribuição a `_pares`; o selo anterior inspeciona AST, sem consumir baldes. (`app/ponto/views.py:2240`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:297-307`.) |

**Dependência externa do antigo dict — NAO PROCEDENTE na cópia examinada.** Não foi localizado consumidor produtivo adicional; os testes anteriores que mencionam a função verificam sua presença e chamada única. (`app/ponto/views.py:2240`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:297-307`.)

**Dependência do universo antigo dos chips — NAO PROCEDENTE nos leitores localizados.** O template apenas apresenta os números; a partição antiga permanece testada na requisição sem cortes, compatível com C4(a). (`app/templates/ponto/ausencias.html:50-67`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:179-186`; `complemento4_ausencia_efeito_1611.txt:2`.)

**Significados — coerentes no caminho válido.** Pendente é caixa das duas filas, não status persistido; `pendentes` ignora as dimensões pedidas, enquanto `cnt.pendente` respeita o efeito pedido. Essa diferença é explícita, autorizada e não é divergência de predicado. (`app/ponto/services/ausencia.py:912`; `app/ponto/models.py:690-705`; `app/ponto/views.py:2277,2281-2285`; `complemento4_ausencia_efeito_1611.txt:4`.)

O veredito empregado é o efeito visual do juiz: abertas conservam o efeito do tipo e rejeitada o sobrepõe. A função que responde PENDENTE para lançamento aberto é outra pergunta, `veredito_do_lancamento`, que esta lista não chama. (`app/ponto/services/dia_decidido.py:453-495`; `app/ponto/views.py:2177,2321`.)

**3. CLASSES QUE JÁ REPROVARAM**

**(a) Segundo juiz — NAO PROCEDENTE no delta.** Agrupamento chama `veredito_do_tipo`; coluna chama `veredito_de`, que delega à mesma função. A regra de rejeição continua no juiz. O `ANEXO!` consulta sua autoridade própria sobre documento e tipo, sem determinar o veredito da coluna. (`app/ponto/views.py:2177,2321`; `app/ponto/services/dia_decidido.py:475-495`; `app/templates/ponto/ausencias.html:158`; `app/ponto/services/ausencia.py:745-759`.)

**(b) Derivação paralela — NAO PROCEDENTE para os cortes usuais; PROCEDENTE a possibilidade de chip diferente do clique.** Os dois ramos usam os mesmos estados e pares julgados; o contraexemplo encontrado vem da serialização do href, detalhada no item 4. (`app/ponto/views.py:2251-2271`; `app/templates/ponto/ausencias.html:48-67`.)

**(c) RED/selo sem mordida — PROCEDENTE em parte:**

- **C3FacetadaTest:** tem mordida contra M-a, M-b e M-c pelos resultados absolutos exigidos; não depende apenas de comparar dois resultados possivelmente errados. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-405`.)
- **C3Red3LacoTest:** não é vazio: exige os sete chips, zero observado e número maior que um. Entretanto, não cobre valores desconhecidos/vazios no laço, valores contendo separadores, registro nem tipo fora do catálogo; o contraexemplo de href passa fora desse domínio. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:423-440`.)
- **C4cPendentesTest:** verifica valor e independência dos filtros. Isoladamente, passaria com a antiga contagem direta das duas filas, porque ela também produz seis; a exigência de passar pela porta depende do selo estrutural. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:443-448,526-534`; `app/ponto/services/ausencia.py:912`.)
- **C4dCatalogoUnicoTest:** morde leitura congelada do catálogo, mas não prova identidade do juiz: uma regra duplicada que lesse o mesmo catálogo poderia passar. O filtro também é conferido apenas por inclusão de um registro; isoladamente, esse assert passaria se o filtro devolvesse registros a mais. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:460-474`.)
- **SeloPortasDaFacetadaTest:** tem escape comprovado. Ignora corpos das portas; uma `.count()` extra dentro delas deixa o resultado do detector igual a `([], ['por_status'])`. Também ignora cortes por aliases fora das raízes enumeradas e leitura por `.get('veredito')`. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:504-516,529-534`.)

A suíte contém outras verificações capazes de pegar parte dessas regressões comportamentais; isso não amplia o alcance do selo nem cobre o contraexemplo de serialização. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:154-161,423-440`.)

**(d) Campo com dois significados — NAO PROCEDENTE como regressão introduzida pelo delta; há ressalva preexistente.** A coluna Status chama qualquer estado diferente de aprovada/rejeitada de Pendente. Para os quatro estados declarados isso coincide com as duas filas; para um estado persistido inválido ou legado, a coluna diria Pendente e o chip não o incluiria. Existência desse dado: **[nao verificado]**. (`app/templates/ponto/ausencias.html:176-179`; `app/ponto/models.py:690-705`; `app/ponto/views.py:2245-2257`.)

**(e) Predicado fora das portas — NAO PROCEDENTE para repetição dos cortes da lista/chips.** Existe, porém, `status='aprovada'` fora delas na consulta dos alertas médicos. Portanto, “nenhum filtro de status em lugar algum da view” seria falso; essa consulta responde outra pergunta e não corta a lista nem seus chips. O selo a deixa passar por sua seleção de raízes. (`app/ponto/views.py:2295-2299`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:508-509`.)

**(f) Contagens — NAO PROCEDENTE a permanência das contagens SQL dos chips.** Os sete números saem de somas do agrupamento; permanece uma `.count()` separada para `pendentes`. Isso não torna a view inteira uma consulta: há lista, alertas, colaboradores e possíveis leituras de catálogo. Total SQL real: **[nao verificado]**. (`app/ponto/views.py:2177-2178,2273-2289,2295-2300,2313`; `app/ponto/catalogo/ausencias.py:249-251,267-281`.)

**4. BORDAS**

| Requisição/caso | Veredito e comportamento pelo código |
|---|---|
| `?status=xyz` | **NAO PROCEDENTE** corte indevido: desconhecido retorna o alvo inteiro. Todos não fica visualmente ativo, porque o template testa a existência do valor, embora não haja corte. (`app/ponto/views.py:2252-2254`; `app/templates/ponto/ausencias.html:49`.) |
| `?status=` | **NAO PROCEDENTE** divergência: sem corte; omitir esse vazio no href conserva a mesma seleção. (`app/ponto/views.py:2252-2254`; `app/templates/ponto/ausencias.html:65-67`.) |
| `?efeito=abona&status=aguardando_documento` | **NAO PROCEDENTE** divergência entre portas: lista é a interseção da fila com os pares ABONADO. O juiz mantém ABONADO para tipo abonador aberto. (`app/ponto/views.py:2251-2271,2291`; `app/ponto/services/dia_decidido.py:475-487`.) |
| Efeito simples fora do mapa ou vazio | **NAO PROCEDENTE** corte indevido: retorna o alvo. Há teste anterior para `xyz`, vazio, `rejeitado` e `abonado`. (`app/ponto/views.py:2260-2262`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:212-215`.) |
| Efeito válido sem par | **NAO PROCEDENTE** retorno de tudo: contador soma zero e queryset recebe `.none()`. (`app/ponto/views.py:2263-2271,2289`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:203-210`.) |
| `registro=<pk>` com status incompatível | **NAO PROCEDENTE** escape do corte: PK substitui o universo antes das portas; status incompatível deixa a lista vazia. Os chips conservam o PK reconhecido e podem remover/substituir sua própria dimensão. (`app/ponto/views.py:2207-2214,2225-2226,2291`; `app/templates/ponto/ausencias.html:48-67`.) |
| Tipo fora do catálogo | **NAO PROCEDENTE** erro de classificação entre grupo e coluna: catálogo retorna `None`; juiz cai em ABONADO, salvo rejeitada. A apresentação contém ambas as chaves. Trata-se do comportamento existente, não de validação desse cadastro. (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:80-84,475-495`; `app/ponto/views.py:2177,2321-2322`.) |
| `de/ate` válidos | **NAO PROCEDENTE** perda do universo nos hrefs: valores normalizados entram em `q_universo`; os testes seguem os links com recorte. (`app/ponto/views.py:2202-2206,2220-2224,2339`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:336-347,430-436`.) |
| `alvo` lista × queryset | **NAO PROCEDENTE** predicados diferentes para os alvos usados: status usa o mesmo `_st`; efeito seleciona os mesmos pares julgados. O OR sobre `_pares` completos é intersectado com o queryset já cortado. (`app/ponto/views.py:2251-2271,2291`.) |

**Contraexemplo PROCEDENTE ao RESULTADO 5:** origem `?mes=4&ano=2026&efeito=xyz%26status%3Drejeitada`. O efeito recebido é `xyz&status=rejeitada`, desconhecido, portanto Todos conta toda a base. O href renderizado de Todos contém `&efeito=xyz&status=rejeitada`; ao clicar, aparece um filtro de status que não existia na origem. No cenário de abril já definido no teste, a derivação é **chip 2, clique 1**. É falha preexistente do template alcançada pelo contrato novo; execução HTTP desse caso: **[nao verificado]**. (`app/ponto/views.py:2236,2260-2262,2281-2283,2291`; `app/templates/ponto/ausencias.html:48`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-383`.)

Há o simétrico com `status=xyz%26efeito%3Dsuprime`: o href Abona passa a conter dois parâmetros `efeito`, introduzindo outra seleção no clique. Os valores preservados recebem escape HTML, mas não codificação de componente de URL; somente `q_universo` é montado com `urlencode`. (`app/ponto/views.py:2191,2252-2254,2282,2289,2339`; `app/templates/ponto/ausencias.html:65`.)

**500 nas bordas simples enumeradas: NAO PROCEDENTE como defeito demonstrado pelo trecho; resposta HTTP efetiva [nao verificado].** Igualdade temporal absoluta entre grupo e coluna também não está certificada: ambos consultam a mesma autoridade, mas o catálogo tem cache expirável e invalidável, sem fotografia explícita por requisição. Mudança concorrente ou durante expiração: **[nao verificado]**. (`app/ponto/views.py:2177,2321`; `app/ponto/catalogo/ausencias.py:257-281`.)

## 16/11 21:49 · sessao aed6460a09d2 · - · - · origem=Ronald (tmux)

| Situação | Quem julga → porta | Persistência / retorno |
|---|---|---|
| ID inválido | `validar_pergunta` busca pergunta/disputa | Nenhuma escrita; `nao_encontrada`. (`app/chamados/services/validacao.py:586-593`) |
| Sem instante, com ou sem texto | Guarda anterior à validação existente | Nenhuma escrita; `sem_resposta`. Continua leitor C a migrar. (`app/chamados/services/validacao.py:594-599`) |
| Já validada | Guarda após instante | Conserva registro; `ja_validada`. (`app/chamados/services/validacao.py:597-599`) |
| Pergunta ou irmã que o ato gravaria barrada por K6 | `entrega_que_barra_a_validacao` → `_k6_que_barra` | Retorno distinto para `bloqueada_sem_dia_do_fato` e `bloqueada_competencia_entregue`. A irmã integra a composição; c4c dispensa sua busca quando a própria pergunta já basta para consultar o mesmo K6. (`app/chamados/services/validacao.py:209-234`) |
| Par recusado no chokepoint por competência aprovada/F6 | `_validar_o_par` | Rollback do savepoint do par; grava motivo pela recusa canônica; sentinela impede tentar a metade sozinha e substituir o motivo por paridade. (`app/chamados/services/validacao.py:536-564`) |
| K6 muda durante ato | Reconsulta depois da materialização | Rollback integral antes da DEVOLUÇÃO; devolve estado próprio K6. (`app/chamados/services/validacao.py:688-695`) |
| Materialização recusada, resposta HORA, sem encerramento, com motivo | `devolver_validacao_recusada` | Desfaz validação, conserva resposta; pode reabrir disputa; exige motivo do escritor canônico. (`app/chamados/services/validacao.py:247-259`) |
| Recusa repetida sem mudança | `devolvida is None` | `nao_gravou`, sem novo evento de mudança; pergunta segue com admin. (`app/chamados/services/validacao.py:702-706`) |
| Devolução ou erro próprio após materialização | Separação dos erros por pergunta | `nao_gravou`; com devolução explicita que validação não ficou. (`app/chamados/services/validacao.py:716-732`) |
| Reuso | Materialização / `reusos` | `ja_no_espelho`, mensagem do serviço. (`app/chamados/services/validacao.py:739-743`) |
| Par gravado | `_validar_o_par` | `validada`, inclui PK da irmã e mensagem de gravação conjunta. (`app/chamados/services/validacao.py:744-747`) |
| Caminho comum concluído | `validar_pergunta` | `validada`. (`app/chamados/services/validacao.py:748-749`) |
| Recusar sem motivo / já validada / sem instante / fora do fato | Guardas próprias, nessa ordem | Respectivamente `sem_motivo`, `ja_validada`, `ja_pendente`, `fora_do_fato`; precedência diferente de validar. (`app/chamados/services/validacao.py:798-822`) |
| Recusar elegível | Motivo + `reabrir_para_correcao`, atomic com trava do colaborador | Zera par pela porta, ato `recusa`; depois nota, auditoria e tentativa de push; `recusada`. (`app/chamados/services/validacao.py:823-865`) |

Interseções novas relevantes: B2 pode alterar a composição do par que será validado/materializado e, por consequência, os motivos e atos de recusa F6; “nenhuma batida nova” não demonstra “nenhum efeito”. (`app/chamados/services/validacao.py:536-564`.) K6 da irmã é parte da decisão da porta e precisa constar do brief; não é apenas atributo da pergunta clicada. (`app/chamados/services/validacao.py:209-225`.)

Não encontrei decisão de lei nova nesses trechos: os estados novos compõem as guardas e distinguem seus resultados; o predicado de entrada ainda continua só instante. Uma pergunta de lei adicional necessária por esses ramos: [nao verificado]. (`app/chamados/services/validacao.py:228-234`; `app/chamados/services/validacao.py:594-599`.)

## 16/11 21:47 · sessao 1694869c524a · - · - · origem=Ronald (tmux)

- **PROCEDENTE — há contraexemplo ao “qualquer R” por serialização dos hrefs.** `R=?efeito=xyz%26status%3Drejeitada`: a porta recebe `xyz&status=rejeitada`, desconhecido, e não corta; Todos soma toda `_base`. O href Todos concatena esse valor sem `urlencode`, tornando o clique `?…&efeito=xyz&status=rejeitada`: agora corta rejeitadas. Com os dois registros de abril do teste, Todos conta 2 e o clique lista 1. O problema está no template preexistente, alcançado pelo contrato amplo C3, não numa nova derivação de efeito. (`app/ponto/views.py:2236,2260-2262,2281-2283,2291`; `app/templates/ponto/ausencias.html:48`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-382`.)

- **PROCEDENTE — caso simétrico.** `R=?status=xyz%26efeito%3Dsuprime`: status desconhecido não corta; chip Abona conta abonados, mas seu href produz `efeito=abona&status=xyz&efeito=suprime`, fazendo o último efeito prevalecer no clique. A concatenação também existe nos demais chips. Parsing demonstrado somente em memória; ocorrência produtiva **[nao verificado]**. (`app/ponto/views.py:2191,2236,2252-2254,2282,2288-2289`; `app/templates/ponto/ausencias.html:48,52,56,60,65-67`.)

- **NAO PROCEDENTE — `status=xyz` simples ou `status=` causarem corte indevido/500 pelo delta.** Ambos retornam o alvo sem corte; efeito simples desconhecido/vazio faz o mesmo. Status desconhecido não marca Todos como ativo porque o template usa verdade do valor, mas isso não altera a lista. (`app/ponto/views.py:2252-2254,2260-2262`; `app/templates/ponto/ausencias.html:49`.)

- **NAO PROCEDENTE — `efeito=abona&status=aguardando_documento` divergir pela implementação dos predicados.** A lista intersecta o status com o efeito; o juiz atual dá ABONADO ao tipo abonador mesmo em fila aberta, porque só rejeitada sobrepõe o efeito. O lançamento pendente é pergunta separada em `veredito_do_lancamento`; a lista consulta `veredito_de`. (`app/ponto/views.py:2291,2321`; `app/ponto/services/dia_decidido.py:453-464,475-487,490-495`.)

- **NAO PROCEDENTE — registro com status incompatível escapar do filtro.** O PK substitui o universo antes de colaborador/tipo e antes das portas; o corte incompatível produz lista vazia. Os hrefs conservam o PK reconhecido, e cada chip troca somente sua dimensão. (`app/ponto/views.py:2207-2214,2225-2226,2235,2291`; `app/templates/ponto/ausencias.html:48-67`.)

- **NAO PROCEDENTE — tipo fora do catálogo causar 500 ou outra classificação na coluna.** O catálogo devolve `None`; o juiz cai em ABONADO; agrupamento e coluna chamam o mesmo núcleo, e a apresentação tem essa chave. A existência produtiva de tais tipos é **[nao verificado]**. (`app/ponto/catalogo/ausencias.py:293-295`; `app/ponto/services/dia_decidido.py:80-84,482-487,495`; `app/ponto/views.py:2177,2321-2322`.)

- **NAO PROCEDENTE — predicados diferentes nos ramos lista/queryset, para os alvos usados.** Status compara pertinência ao mesmo `_st`; efeito compara o veredito agrupado, monta OR dos mesmos pares e intersecta o queryset recebido. Ausência de pares vira `.none()`. O ramo queryset usa `_pares` de toda `_base`, mas os cortes anteriores do próprio queryset conservam a interseção. (`app/ponto/views.py:2240,2251-2271,2281-2291`.)

- **NAO PROCEDENTE — segundo juiz novo no delta.** O agrupamento chama `veredito_do_tipo`; a coluna chama `veredito_de`, que delega àquela função; a regra “rejeitada vence” permanece no juiz. (`app/ponto/views.py:2177,2321`; `app/ponto/services/dia_decidido.py:475-495`.)

- **Ressalva de leitura do literal “um sítio”:** permanece `status='aprovada'` fora das portas no universo próprio dos alertas médicos, sem filtrar a lista ou os chips. Sua presença é demonstrável; tratá-la como repetição do predicado da dimensão exigiria ignorar que responde outra pergunta. (`app/ponto/views.py:2295-2299`.)

- **NAO SEI — igualdade temporal de catálogo em toda execução.** Agrupamento e coluna consultam o mesmo catálogo, mas não uma fotografia explícita: `vigente()` tem cache de 60 segundos e pode ser invalidado. Mudança entre as duas classificações pode mudar a coluna; concorrência ou expiração durante requisição produtiva é **[nao verificado]**. (`app/ponto/views.py:2177,2321`; `app/ponto/catalogo/ausencias.py:257-281`.)

## 16/11 21:47 · sessao e2b57ed680ee · - · - · origem=Ronald (tmux)

Censo de escritores/leitores, pela cópia:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| GET `status` | Chips de status escrevem `pendente/aprovada/rejeitada`; chips de efeito e formulário preservam o recebido (`app/templates/ponto/ausencias.html:52-67,75`). | `status_filtro = request.GET.get('status')`; portas de números/lista e contexto (`app/ponto/views.py:2191,2282,2291,2337`). |
| GET `efeito` | Chips de efeito escrevem `abona/desconta/suprime`; chips de status preservam o recebido (`app/templates/ponto/ausencias.html:48-67`). | `efeito_filtro = request.GET.get('efeito')`; portas de números/lista e contexto (`app/ponto/views.py:2236,2281,2291,2335`). |
| `_pares` / retorno de `_pares_por_veredito` | `tipo/status` saem de `values`; `n` de `Count('pk')`; `veredito` de `DD.veredito_do_tipo`; retorno é lista de dicionários (`app/ponto/views.py:2175-2178,2240`). | `por_status` lê `status`; `por_efeito` lê `veredito/tipo/status`; `_soma` lê `n`; facetas leem as portas (`app/ponto/views.py:2255-2274,2281-2289`). |
| `_status_do_filtro` | Mapa local, `pendente` deriva de `_A4_ABERTAS`, quatro estados individuais (`app/ponto/views.py:2245-2246`). | Só `por_status` (`app/ponto/views.py:2252`). Fonte externa das abertas: import em `app/ponto/views.py:79`; tupla em `app/ponto/services/ausencia.py:912`. |
| `_veredito_do_filtro` | Compreensão do `APRESENTACAO_AUSENCIA`, excluindo filtros vazios (`app/ponto/views.py:2242`; mapa em `app/ponto/services/dia_decidido.py:80-84`). | `por_efeito` resolve pedido; laço escreve contadores de efeito (`app/ponto/views.py:2260,2288-2289`). |
| `pendentes` | Contagem separada de `por_status(_base,'pendente')`; exportada ao contexto (`app/ponto/views.py:2277,2325`). | Leitor explícito localizado: teste C4c (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:444-448`). Nenhum consumo produtivo explícito desse contexto localizado no template; outros `pendentes` da árvore pertencem a outros contextos [nao verificado]. |
| `_cnt`, `cnt`, `count_*` | Facetas somam `n`; aliases de status saem do mesmo `_cnt` (`app/ponto/views.py:2281-2289,2335-2336`). | Template usa quatro aliases e três `cnt.*` (`app/templates/ponto/ausencias.html:50-67`); testes anteriores leem contexto e HTML (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:149-152,181-208,338,361`). |
| `filtro_status/filtro_efeito` | Contexto recebe os GET crus (`app/ponto/views.py:2335-2337`). | Hrefs, classes ativas e hidden de status (`app/templates/ponto/ausencias.html:48-67,75`). |
| Lista `ausencias` | Portas produzem `qs`; materialização e enriquecimento; publicação no contexto (`app/ponto/views.py:2291,2313-2324`). | Template percorre linhas (`app/templates/ponto/ausencias.html:142-185`); helper e testes anteriores (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:129,223-224`; `app/ponto/tests/test_lista_ausencias.py:21-31`). |

**Consumidor do antigo dict: NAO PROCEDENTE na cópia examinada.** A única chamada produtiva localizada é `lista_ausencias`; os testes fora do delta apenas inspecionam AST da função e quantidade de chamadas, sem consumir seus baldes (`app/ponto/views.py:2240`; `app/ponto/tests/test_ausencia_efeito_pelo_estado.py:297-307`). Não localizei leitor executável exigindo o universo antigo; a partição anterior permanece testada na requisição sem cortes (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:179-186`).

**Semântica válida: NAO PROCEDENTE a alegação de confusão introduzida pelo delta.** Caixa Pendente deriva das duas filas; o veredito do grupo e da coluna vem do mesmo juiz de efeito (`app/ponto/views.py:2245-2257,2177,2321`; `app/ponto/services/ausencia.py:912`). Existe outro juiz, `veredito_do_lancamento`, que responde PENDENTE para abertas, mas a view examinada não o chama (`app/ponto/services/dia_decidido.py:453-464`; `app/ponto/views.py:2177,2321`).

**Ressalva fora do delta:** coluna Status escreve “Pendente” em qualquer `else`; para os quatro estados declarados isso coincide com ABERTAS, mas estado persistido inválido/legado seria chamado Pendente sem entrar no chip. Existência produtiva desse estado: [nao verificado] (`app/templates/ponto/ausencias.html:176-179`; `app/ponto/models.py:690-705`; `app/ponto/views.py:2245-2257`).

**Achado para RESULTADO 5 literal: PROCEDENTE.** Valores desconhecidos contendo separador de query são ignorados pelas portas, mas preservados sem codificação de URL no href; somente `_universo` recebe `urlencode` (`app/ponto/views.py:2252-2254,2260-2262,2339`; `app/templates/ponto/ausencias.html:48-67`). Exemplo derivado: `?mes=3&ano=2026&status=xyz%26tipo%3Dfalta` conta Abona sobre todos os tipos, porém href Abona renderiza status contendo `&tipo=falta`; ao navegar, `tipo=falta` passa a filtrar `_base` e Abona pode abrir zero (`app/ponto/views.py:2190-2191,2213-2214,2282,2289`; `app/templates/ponto/ausencias.html:65`). Na fixture existente isso seria **4 no chip e 0 no clique**, pois há quatro Abona e falta é DESCONTA; execução HTTP desse exemplo [nao verificado] (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:91-100,109-111,149`).

## 16/11 21:47 · sessao ccc5d48acf69 · - · - · origem=Ronald (tmux)

- **C3FacetadaTest — NAO PROCEDENTE a falta de mordida dos mutantes nomeados.** O cenário tem aprovado-suprime e rejeitado-abona, em abril; RED1 exige Suprime `(0,0)`, matando M-a; RED2 exige Rejeitada `(1,1)`, matando M-b; RED M-c exige Rejeitada `(0,0)` sob Suprime, matando contador de status na base crua. Isso é inferência direta dos asserts, sem execução Django. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:379-400`.) Execução verde e mutação real: **[nao verificado]**.

- **C3Red3LacoTest — NAO PROCEDENTE vacuidade geral.** São 24 origens, incluindo as duas filas individuais, e os sete chips precisam existir; cada href é seguido e comparado ao número. Há exigência de zero e de número maior que um. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:423-440`.) **Cumpre a cobertura enumerada do RED3**, mas não inclui status desconhecido/vazio, efeito desconhecido, registro ou tipos fora do catálogo; o conjunto está explícito em `:424-427`, e o universo usa apenas os três tipos em `:91-95`. A igualdade compara quantidade, não pertencimento exato da lista, por `_pks` e `len` (`:128-129,435-436`).

- **C4cPendentesTest — proteção funcional presente, proteção estrutural depende do selo.** Exige seis pendentes independentemente de quatro combinações de filtros; retornar à contagem antiga diretamente com `_A4_ABERTAS` preservaria esses seis e passaria neste teste isolado. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:443-448`; `app/ponto/views.py:2245-2257,2277`.) Não é assert vazio: o fixture cria ambas as filas para três tipos. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:34-35,91-95`.)

- **C4dCatalogoUnicoTest — mordida real contra catálogo congelado.** Troca `efeito_vigente(atestado)` para SUPRIME, exige coluna SUPRIMIDO, Abona zero, Suprime sete e presença do aprovado no filtro; compara também os totais por veredito na coluna. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:456-474`.) **Limite:** uma segunda implementação da regra que também consultasse `CAT.efeito_vigente` poderia passar; esse teste verifica comportamento sob a troca, não a identidade do juiz. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:460-474`.)

- **SeloPortasDaFacetadaTest — PROCEDENTE proteção incompleta.** O varredor só reconhece chamadas com raízes `qs/_base/Q/_Q`, subscritos literais `['status']/['veredito']` e chamadas `.count()`; ignora completamente os corpos das portas. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:502-517`.) Assim, predicado duplicado em alias (`outro = _base; outro.filter(status=...)`), leitura com `.get('veredito')` e contagens extras dentro das portas escapam; os asserts só exigem achados vazios, `['por_status']` e existência das duas funções. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:508-516,526-534`.) Confirmei esses três escapes executando somente as funções AST extraídas em memória, sem arquivos nem BD; o caso com contagens adicionais nas portas devolveu `([], ['por_status'])`. A limitação decorre de `:504-505,508,514-516`.

- **Contagens atuais — NAO PROCEDENTE permanência das sete contagens dos chips no corpo examinado.** Os chips somam `n`; o agrupamento é uma chamada `.values('tipo','status').annotate(n=Count('pk'))`, e `pendentes` conserva sua `.count()` separada. (`app/ponto/views.py:2177-2178,2273-2289`.) **Total SQL executado: [nao verificado]**; o selo não prova esse total pelos limites acima. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:504-513`.)

- **SELO C3 — PROCEDENTE instrumento adicional no delta.** Além do RED3 declarado selo, foi acrescentada a classe AST `SeloPortasDaFacetadaTest`; o C3 diz “o RED 3 é o selo; nenhum instrumento novo”. (`app/ponto/tests/test_ausencia_efeito_pelo_estado.py:418-440,492-546`; `complemento3_ausencia_efeito_1611.txt:8`.)

## 16/11 21:47 · sessao 89cb77cde8e2 · - · - · origem=Ronald (tmux)

Censo estático fechado pelos sítios R nomeados e chamadas nominais B/C/D com aliases de `ImportFrom`; despacho dinâmico: [nao verificado].

- Permanecem os **66 R** herdados: B16/C31/D19. O corpo B ainda usa OU, C ainda usa instante e D ainda usa texto; a property continua só instante. (`app/chamados/models.py:1924`; `app/chamados/models.py:3665`; `app/chamados/services/validacao.py:594`; `app/chamados/services/materializacao.py:869`.)
- O complemento ao anexo tem **14 sítios**: B transitivo1/C4/D9; não são 14 predicados independentes. (`app/chamados/models.py:719`; `app/chamados/services/acoes_disputa.py:99`; `app/chamados/models.py:2309`; `app/chamados/views.py:1420`; `app/chamados/templatetags/chamados_tags.py:226`; `app/chamados/models.py:3031`; `app/chamados/services/declaracao_texto.py:253`; `app/chamados/services/declaracao_texto.py:275`; `app/chamados/services/fio.py:1003`; `app/chamados/services/fio.py:1335`; `app/chamados/services/fio.py:1900`; `app/chamados/services/fio.py:2223`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:393`.)

Mudanças de linhas em relação ao b2.r:

| R | HEAD atual |
|---|---|
| B104 | `app/chamados/services/materializacao.py:1244` |
| C64/C65 | `app/chamados/services/acoes_disputa.py:97`; `:132` |
| C89 | `app/chamados/services/fio.py:706` |
| C107 | `app/chamados/services/materializacao.py:2064` |
| C116/C117 | `app/chamados/services/validacao.py:594`; `:813` |
| D90/D91 | `app/chamados/services/fio.py:1279`; `:1290`; `:1291` |
| D99/D105 | `app/chamados/services/materializacao.py:869`; `:1311` |
| D126 | `app/chamados/templatetags/chamados_tags.py:166` |
| C delegado | `app/chamados/templatetags/chamados_tags.py:226` |
| D delegados fio | `app/chamados/services/fio.py:1003`; `:1335`; `:1900`; `:2223` |

A BUG-PAR acrescenta quatro consumidores que **já usam J**; não devem virar cópias B/C/D na contagem: seleção SQL da irmã, resposta da metade, elegibilidade U103 e situação da irmã. (`app/chamados/services/materializacao.py:146`; `:192`; `:259`; `:290`.)

As três guardas textuais novas são **E técnicas**, com finalidade própria: fallback da testemunha no fio, fallback na página e escopo da proteção declarado versus gravado. Migrá-las por busca textual apagaria a distinção entre resposta e disponibilidade/escopo da testemunha. (`app/chamados/services/fio.py:377`; `app/chamados/templatetags/chamados_tags.py:139`; `app/chamados/services/materializacao.py:770`.) A interpretação HH:MM e a comparação hora/tipo também são E. (`app/chamados/services/materializacao.py:718`; `:730`.)

Na antiga D99 existe uma precedência nova que o brief deve preservar: vias materializadas de recusa retornam seu estado **antes** de testar tipo hora/texto; só depois vem o atalho `aplicada`. (`app/chamados/services/materializacao.py:862`; `:869`; `:871`.) A porta de escrita consulta K6 antes de DATA_EM_DISPUTA e a espera declarado versus gravado antes do atomic do plantio. (`app/chamados/services/materializacao.py:1830`; `:1877`; `:1887`.)

A fixture do teste continua sem `respondida_em`, embora agora forneça campos do gate DATA_EM_DISPUTA; a incompatibilidade estática com chamada direta de J permanece. (`app/chamados/tests/test_estado_aplicacao.py:38`; `:41`; `app/chamados/juizes.py:1552`.)

Novas provas estáticas da c4 para conferir na B2: separação F6/K6, competência entregue e sem dia do fato, incluindo a porta e apresentação. Aprovação em execução: [nao verificado]. (`app/chamados/tests/test_bug_par_volta.py:133`; `:233`; `:312`; `:318`.)

## 16/11 21:40 · sessao 950d9935f151 · ESPINHA-F2 · verificador · origem=Code

**Os três itens conferem como aval de conservação; os sete nomes não formam um censo fechado do trio.** O próprio C3 inclui leitores adicionais no selo, e o zero permanece limitado ao predicado investigado pelo C2 (`complemento3_respostas_f2_estados_1611.txt:3`; `complemento2_respostas_f2_estados_1611.txt:4`). Execução do selo, ocorrências em produção e correspondência independente do hash: [nao verificado].

Referências dos anexos contam desde o conteúdo fornecido, incluindo linhas vazias.

**Item 1**

1. **Premissas conferem.** A lista exige ausência de validação e materialização, instante **ou** texto de resposta e tratamento da tranca. O descarimbo efetivo remove a materialização; ressuscitar também apaga instante e texto de resposta, fazendo o controle ficar fora da lista (`app/chamados/models.py:1923`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926`; `app/chamados/models.py:3318`; `app/chamados/models.py:1629`; `app/chamados/models.py:1630`).

2. **Não colide com o vigente.** Adota a correção pedida na devolução e conserva a escolha de perder o instante no descarimbo, mantendo o comportamento anterior (`app/docs/RELATO.md:4`; `app/docs/RELATO.md:31`; `complemento1_respostas_f2_estados_1611.txt:9`).

3. **Fecha no literal.** “Descarimbada” precisa significar descarimbo efetivamente realizado, mantendo a pergunta no universo passado à lista. A preparação do caso deve excluir via terminal, pois essa via faz a porta recusar o descarimbo (`app/chamados/models.py:1922`; `app/chamados/models.py:3316`; `app/chamados/models.py:3392`). “Resposta preservada” corresponde ao instante presente ou texto não branco exigido pelo predicado (`app/chamados/models.py:1924`; `app/chamados/models.py:1925`).

4. **Há alcance declarado no CSF.** A lista alimenta sua permissão de execução; qualquer diferença em competência exportada continua exigindo `PAREI !` com tabela (`app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:574`; `app/ponto/supra_juiz.py:588`; `app/docs/CORTES.md:487`). Efeito financeiro efetivo: [nao verificado].

**Item 2**

1. **Os sete pontos existem, mas são observáveis diferentes.** `respondida` lê `respondida_em`; `resposta_display` lê a resposta, não o trio. `supra_juiz` e `irmas_respondidas_sem_consumo` delegam à lista (`app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/ponto/supra_juiz.py:573`; `app/chamados/juizes.py:2105`).

2. **Não colide com o vigente:** amplia expressamente o selo anterior para os sete e para qualquer leitor adicional, preservando a obrigação geral de conservação (`complemento3_respostas_f2_estados_1611.txt:3`; `respostas_registradas.md:5`).

3. **Fecha como condição extensível, não como inventário encerrado.** Encontrar leitores adicionais obriga incluí-los no selo; não devolve automaticamente este item (`complemento3_respostas_f2_estados_1611.txt:3`). A atribuição integral dos consumidores abaixo ao escopo da matriz F2: [nao verificado].

4. **Não autoriza mudança de comportamento nesses leitores.** Continua exigindo igualdade antes/depois, com a restrição de exportadas preservada (`complemento3_respostas_f2_estados_1611.txt:2`; `complemento2_respostas_f2_estados_1611.txt:5`; `app/docs/CORTES.md:487`). Igualdade efetivamente demonstrada: [nao verificado].

**Resposta ao censo solicitado: não, os sete não esgotam os leitores do trio na cópia.** Há os seguintes consumidores adicionais identificados; inclusive portas que leem antes de escrever (`app/chamados/models.py:3236`; `app/chamados/juizes.py:1390`; `app/ponto/services/procedencia_batida.py:72`).

| Outros leitores | Arquivo:linha |
|---|---|
| `_universo_da_ressuscitacao`: seleciona por instante e via | `app/chamados/models.py:1531`, `app/chamados/models.py:1535` |
| `fechar`; `remover_motivo`: leem via e/ou instante | `app/chamados/models.py:2001`, `app/chamados/models.py:2006`, `app/chamados/models.py:2136`, `app/chamados/models.py:2141` |
| `do_fato`; `q_dos_fatos`; `fora_do_fato`: classificam pela via | `app/chamados/models.py:2829`, `app/chamados/models.py:2860`, `app/chamados/models.py:3311` |
| `fechar_por_admin`; `carimbar`; `carimbar_identidade`: idempotência, foto anterior e preservação de carimbo | `app/chamados/models.py:3165`, `app/chamados/models.py:3166`, `app/chamados/models.py:3236`, `app/chamados/models.py:3292`, `app/chamados/models.py:3293` |
| `descarimbar_em_lote`; encerramento por fato/fantasma; reaberturas | `app/chamados/models.py:3392`, `app/chamados/models.py:3400`, `app/chamados/models.py:3411`, `app/chamados/models.py:3423`, `app/chamados/models.py:3440`, `app/chamados/models.py:3447` |
| `_retratada_com_trilha`; `_pergunta_materializada_de_fato`; chave do memo; projeção temporal; `pergunta_sem_lastro_ou_viva` | `app/chamados/juizes.py:292`, `app/chamados/juizes.py:683`, `app/chamados/juizes.py:711`, `app/chamados/juizes.py:857`, `app/chamados/juizes.py:858`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1502` |
| Materialização: guardas, pendentes, grupos de absorção e foto do trio | `app/chamados/services/materializacao.py:440`, `app/chamados/services/materializacao.py:539`, `app/chamados/services/materializacao.py:631`, `app/chamados/services/materializacao.py:633`, `app/chamados/services/materializacao.py:638`, `app/chamados/services/materializacao.py:674`, `app/chamados/services/materializacao.py:677`, `app/chamados/services/materializacao.py:678`, `app/chamados/services/materializacao.py:1099`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/materializacao.py:1425` |
| Emissão: filtros de via/materialização e resolução pelo chamado | `app/chamados/services/disputa_emissao.py:228`, `app/chamados/services/disputa_emissao.py:337`, `app/chamados/services/disputa_emissao.py:409`, `app/chamados/services/disputa_emissao.py:428`, `app/chamados/services/disputa_emissao.py:446`, `app/chamados/services/disputa_emissao.py:776`, `app/chamados/services/disputa_emissao.py:1084`, `app/chamados/services/disputa_emissao.py:1104`, `app/chamados/services/disputa_emissao.py:1340`, `app/chamados/services/disputa_emissao.py:1930`, `app/chamados/services/disputa_emissao.py:2062` |
| Validação; tarefa | `app/chamados/services/validacao.py:215`, `app/chamados/services/validacao.py:486`, `app/chamados/services/validacao.py:513`, `app/chamados/services/tarefa_do_dia.py:256` |
| Reconciliação: guardas, seleção e fotos | `app/chamados/reconciliador.py:367`, `app/chamados/reconciliador.py:630`, `app/chamados/reconciliador.py:1050`, `app/chamados/reconciliador.py:1144`, `app/chamados/reconciliador.py:1145`, `app/chamados/reconciliador.py:1146`, `app/chamados/reconciliador.py:1505` |
| Re-lavra: comparação de via, seleção e foto | `app/chamados/services/relavra_via.py:118`, `app/chamados/services/relavra_via.py:121`, `app/chamados/services/relavra_via.py:155`, `app/chamados/services/relavra_via.py:181`, `app/chamados/services/relavra_via.py:244` |
| Fio: identidade, duplicatas e seleção | `app/chamados/services/fio.py:1139`, `app/chamados/services/fio.py:1206`, `app/chamados/services/fio.py:1282`, `app/chamados/services/fio.py:1389`, `app/chamados/services/fio.py:1472`, `app/chamados/services/fio.py:1913`, `app/chamados/services/fio.py:1949`, `app/chamados/services/fio.py:1971`, `app/chamados/services/fio.py:2016` |
| Histórico; marcador do painel; marcos temporais da lavra | `app/chamados/services/historico.py:103`, `app/chamados/services/historico.py:105`, `app/chamados/services/painel.py:67`, `app/chamados/veredito_lavrado.py:203`, `app/chamados/veredito_lavrado.py:204` |
| Procedência da batida: autor e instante | `app/ponto/services/procedencia_batida.py:72`, `app/ponto/services/procedencia_batida.py:74` |
| Raio X; explicador; fábrica; passivo; reabertura de competência | `app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:182`, `app/chamados/services/explicador.py:95`, `app/chamados/services/fabrica_da_celula.py:149`, `app/chamados/services/fabrica_da_celula.py:171`, `app/chamados/services/passivo_validado.py:141`, `app/chamados/services/competencia_trancada.py:276` |
| View; cartório; flip; decisão do dia; apresentação da via | `app/chamados/views.py:1257`, `app/ponto/services/cartorio.py:1650`, `app/ponto/services/flip_auto.py:129`, `app/ponto/services/veredito_celula.py:196`, `app/templates/chamados/partials/_card_atendimento.html:192` |

Também existem consumidores indiretos além dos dois nomeados: `respostas_sem_veredito`, `pode_fechar`, reconciliação, fechamento por competência, fila por causa, ficha e desligamento (`app/chamados/models.py:1906`; `app/chamados/models.py:1934`; `app/chamados/reconciliador.py:1164`; `app/chamados/services/competencia_trancada.py:126`; `app/chamados/services/fila_por_causa.py:226`; `app/colaboradores/services/ficha.py:169`; `app/colaboradores/services/desligamento.py:165`).

**Item 3**

1. **Confere como registro do censo específico do C2.** A projeção parte dos campos atuais e apenas corta valores posteriores a `agora`; não reconstrói materialização apagada pelo descarimbo (`complemento2_respostas_f2_estados_1611.txt:4`; `respostas_registradas.md:14`; `app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1396`; `app/chamados/juizes.py:1400`; `app/chamados/models.py:3318`).

2. **L1 resolvido pelo corte é compatível com a escolha vigente.** Resolve pela conservação do campo atual, mantendo a perda do instante; não declara MOOT pela F1 nem cria ato histórico (`complemento2_respostas_f2_estados_1611.txt:3`; `complemento3_respostas_f2_estados_1611.txt:4`; `app/docs/RELATO.md:31`).

3. **O zero não significa ausência de leitores temporais.** `marcos_do_lote` filtra `materializada_em <= ate` e calcula seu máximo; tarefa e raio X fornecem horizonte. Isso lê o campo atual, sem demonstrar dependência de reconstruir um instante já apagado (`app/chamados/veredito_lavrado.py:163`; `app/chamados/veredito_lavrado.py:203`; `app/chamados/veredito_lavrado.py:204`; `app/chamados/services/tarefa_do_dia.py:434`; `app/chamados/services/raiox.py:38`). Afirmação universal sobre produção: [nao verificado].

4. **Não libera exportadas nem dispensa a prova.** A mudança de classificação de L1 conserva a exigência de igualdade e a parada diante de diferença em competência exportada (`complemento3_respostas_f2_estados_1611.txt:2`; `app/docs/CORTES.md:487`). Resultado efetivo da sombra e impacto financeiro: [nao verificado].

VEREDITO por item: 1 CONFERE (`app/chamados/models.py:1923`, `app/chamados/models.py:1629`); 2 CONFERE — selo extensível, sete não exaustivos (`complemento3_respostas_f2_estados_1611.txt:3`, `app/chamados/juizes.py:1390`); 3 CONFERE — zero no escopo registrado do C2 (`complemento2_respostas_f2_estados_1611.txt:4`, `respostas_registradas.md:14`).

## 16/11 21:39 · sessao 425552aa2e88 · AUSENCIA-EFEITO-PELO-ESTADO · verificador · origem=Code

**O C4 resolve as devoluções examinadas; (d) permite agrupar por `(tipo, status)`.** O juiz depende também do catálogo atual, mas não consulta a data da ausência para escolher esse catálogo. (`complemento4_ausencia_efeito_1611.txt:2-5`; `app/ponto/services/dia_decidido.py:453-481`; `app/ponto/catalogo/ausencias.py:267-295`.)

As referências aos anexos contam linhas desde `aval Ronald`, sem o cabeçalho do anexo.

**(a)**

1. **Premissas:** conferem. Os hrefs de status conservam efeito; os de efeito conservam status. A contagem facetada acompanha essa navegação. (`app/templates/ponto/ausencias.html:48-67`; `complemento3_ausencia_efeito_1611.txt:4`.)
2. **Colisão:** o C4 revoga expressamente o universo anterior dos chips e Total e restringe o 5a à requisição sem corte nas duas dimensões. Resolve o conflito apontado com o C1. (`complemento1_ausencia_efeito_1611.txt:5-6,10`; `complemento4_ausencia_efeito_1611.txt:2`.)
3. **Literal:** o exemplo fecha: em `status=rejeitada`, nenhum efeito conta a rejeitada; Todos remove o corte de status e conta os dois registros. A preservação de datas e registro continua dependendo do C2, mantido pelo C3. (`app/ponto/services/dia_decidido.py:461-473`; `app/templates/ponto/ausencias.html:48,65-67`; `complemento2_ausencia_efeito_1611.txt:2`; `complemento3_ausencia_efeito_1611.txt:4`; `complemento4_ausencia_efeito_1611.txt:2`.)
4. **Exportada/dinheiro/`!`:** muda o universo exibido dos contadores, dentro da autorização explícita; não exige gravação financeira. (`complemento4_ausencia_efeito_1611.txt:2`; `app/ponto/views.py:2197-2226,2254-2270`.)

**(b)**

1. **Premissas:** M-a corrigido é morto pelo RED 1: contar Suprime sobre `_base` cru inclui o aprovado que suprime, embora a origem esteja em Rejeitada. (`complemento3_ausencia_efeito_1611.txt:6`; `complemento4_ausencia_efeito_1611.txt:3`.)
2. **Colisão:** substitui nominalmente M-a, acrescenta M-c e conserva M-b; não mantém a associação errada anterior. (`complemento3_ausencia_efeito_1611.txt:7`; `complemento4_ausencia_efeito_1611.txt:3`.)
3. **Literal:** M-c também fecha: com efeito Suprime, a rejeitada não pertence ao universo do contador Rejeitada; o mutante sobre a base crua conta 1. São contraexemplos lógicos, não ocorrências produtivas demonstradas. (`app/ponto/services/dia_decidido.py:461-473`; `complemento4_ausencia_efeito_1611.txt:3`.)
4. **Exportada/dinheiro/`!`:** são condições de verificação dos contadores, sem alteração declarada dos registros. (`complemento4_ausencia_efeito_1611.txt:3`.)

**(c)**

1. **Premissas:** `pendentes` hoje conta `_A4_ABERTAS`, importado de `ABERTAS`: as duas filas abertas. (`app/ponto/views.py:79,2197`; `app/ponto/services/ausencia.py:912`.)
2. **Colisão:** a ordem específica do C4 substitui a proibição anterior de tocar essa contagem. (`complemento3_ausencia_efeito_1611.txt:9`; `complemento4_ausencia_efeito_1611.txt:4`.)
3. **Literal:** preserva o valor se `por_status(..., 'pendente')` usar a união dessas duas filas. É necessário definir `_base` antes da chamada: atualmente sua atribuição vem depois de `pendentes`. (`complemento3_ausencia_efeito_1611.txt:4`; `app/ponto/views.py:2197-2201,2212-2219`.)
4. **Exportada/dinheiro/`!`:** continua sendo contagem entregue ao contexto, sem gravação. (`app/ponto/views.py:2197,2256`; `complemento4_ausencia_efeito_1611.txt:4`.)

**(d)**

1. **Premissas:** `veredito_de` lê somente `ausencia.tipo` e `ausencia.status`. Rejeitada retorna imediatamente; os demais casos consultam `CAT.efeito_vigente(tipo)`. Portanto, **não é função exclusivamente do par**, pois existe a dependência externa do catálogo. (`app/ponto/services/dia_decidido.py:453-481`.)
2. **Colisão:** a consulta agrupada substitui expressamente as sete contagens, esclarecendo o orçamento anterior; `pendentes` conserva sua contagem separada conforme (c). (`app/ponto/views.py:2197-2211`; `complemento4_ausencia_efeito_1611.txt:4-5`.)
3. **Literal:** **dá para agrupar por `(tipo, status)`** e classificar cada grupo pelo juiz. O catálogo é o atual: lê tipos ativos, com cache de 60 segundos e semente quando a leitura não fornece cadastro. Não há seleção histórica por data do lançamento. Datas delimitam `_base`, não o veredito. Isso não torna a view inteira uma única consulta. (`app/ponto/catalogo/ausencias.py:240-295`; `app/ponto/services/dia_decidido.py:468-481`; `app/ponto/views.py:2186-2196,2231-2258`.)
4. **Exportada/dinheiro/`!`:** agrupar e consultar o juiz não exige alterar catálogo, status ou projeção financeira; a gravação e o despertar da projeção pertencem à transição de estado. (`app/ponto/catalogo/ausencias.py:240-295`; `app/ponto/services/ausencia.py:976-994`.)

O `ANEXO!` permanece um predicado separado; nenhuma dessas letras exige modificá-lo. (`app/templates/ponto/ausencias.html:158`; `app/ponto/services/ausencia.py:745-759`.) Implementação futura, REDs executados, mutantes mortos, total SQL e comportamento produtivo: **[não verificado]**.

VEREDITO por letra/item: (a) CONFERE (`complemento4_ausencia_efeito_1611.txt:2`); (b) CONFERE (`complemento4_ausencia_efeito_1611.txt:3`); (c) CONFERE (`app/ponto/views.py:2197-2201,2212-2219`); (d) CONFERE (`app/ponto/services/dia_decidido.py:453-481`; `app/ponto/catalogo/ausencias.py:267-295`).

## 16/11 21:39 · sessao 201f536c64d6 · - · - · origem=Ronald (tmux)

**Item 2: CONFERE como compromisso de conservação extensível; os sete não constituem censo completo do trio.** O próprio C3 manda incluir qualquer leitor adicional no selo; portanto encontrar outro não contradiz sua condição, mas impede registrar “universo fechado = 7” (`complemento3_respostas_f2_estados_1611.txt:3`). A abrangência exata do brief/matriz F2, ausentes nesta cópia: [nao verificado].

`respondida` e `resposta_display` são observáveis da F2 citados no aval, mas não leem o trio: leem `respondida_em` e `resposta_colab`, respectivamente (`app/chamados/models.py:3665`; `app/chamados/models.py:3688`). `supra_juiz` e `irmas_respondidas_sem_consumo` são consumidores indiretos da lista (`app/ponto/supra_juiz.py:573`; `app/chamados/juizes.py:2105`).

**Leitores adicionais no mesmo arquivo de modelos**, além dos sete nomeados:

| Função/unidade | Leitura |
|---|---|
| `_universo_da_ressuscitacao` | Filtra instante e via (`app/chamados/models.py:1531`; `app/chamados/models.py:1535`). |
| `fechar` | Guarda via anterior e usa instante no SILENCIO (`app/chamados/models.py:2001`; `app/chamados/models.py:2006`). |
| `remover_motivo` | Filtra via e a registra no motivo da retratação (`app/chamados/models.py:2136`; `app/chamados/models.py:2141`). |
| `do_fato`; `q_dos_fatos` | Excluem vias fora do fato (`app/chamados/models.py:2829`; `app/chamados/models.py:2860`). |
| `fechar_por_admin` | Guarda de idempotência por via e instante (`app/chamados/models.py:3165`; `app/chamados/models.py:3166`). |
| `carimbar`; `carimbar_identidade` | Leem estado anterior do trio / preservam instante e autor (`app/chamados/models.py:3236`; `app/chamados/models.py:3292`; `app/chamados/models.py:3293`). |
| `fora_do_fato` | Classifica pela via (`app/chamados/models.py:3311`). |
| `descarimbar_em_lote` | Lê via no filtro antes de apagar o trio (`app/chamados/models.py:3392`; `app/chamados/models.py:3393`). |
| `encerrar_por_fato_no_espelho`; `encerrar_fantasma` | Guardas pela via / via com instante (`app/chamados/models.py:3400`; `app/chamados/models.py:3411`). |
| `reabrir_fantasma`; `reabrir_por_resposta`; `reabrir_por_competencia` | Guardas por instante/via (`app/chamados/models.py:3423`; `app/chamados/models.py:3440`; `app/chamados/models.py:3447`). |
| Índices de unicidade | Condições dependem da via (`app/chamados/models.py:3652`; `app/chamados/models.py:3658`). |

**Outros consumidores produtivos do trio na cópia** — atribuí-los automaticamente à F2 seria [nao verificado]:

| Arquivo/funções | Evidência |
|---|---|
| Juízes: `_retratada_com_trilha`, `_pergunta_materializada_de_fato`, `_chave_memo_pv`, `projecao_temporal_da_pergunta`, `pergunta_sem_lastro_ou_viva` | `app/chamados/juizes.py:292`; `:683`; `:711`; `:857`; `:1390`; `:1502`. |
| Materialização: estado de aplicação, seleção de pendentes, agrupamento de absorvidas, foto do trio, guarda de recusa | `app/chamados/services/materializacao.py:440`; `:539`; `:631`; `:633`; `:638`; `:674`; `:677`; `:678`; `:1099`; `:1378`; `:1425`. |
| Emissão: derivação do destino, geração de perguntas, órfãs/âncoras e resolução pelo chamado | `app/chamados/services/disputa_emissao.py:228`; `:337`; `:409`; `:428`; `:446`; `:776`; `:1084`; `:1104`; `:1340`; `:1930`; `:2062`. |
| Validação | `app/chamados/services/validacao.py:215`; `:486`; `:513`. |
| Tarefa | `app/chamados/services/tarefa_do_dia.py:256`; `:257`. |
| Reconciliação: guardas, universos e fotos | `app/chamados/reconciliador.py:367`; `:630`; `:1050`; `:1144`; `:1146`; `:1505`. |
| Re-lavra: filtros e foto anterior | `app/chamados/services/relavra_via.py:118`; `:121`; `:123`; `:155`; `:181`; `:244`. |
| Fio: identidade, duplicatas e grupos | `app/chamados/services/fio.py:1139`; `:1206`; `:1282`; `:1389`; `:1472`; `:1913`; `:1949`; `:1971`; `:2016`. |
| Procedência: **autor e instante**, além da via nos demais leitores | `app/ponto/services/procedencia_batida.py:72`; `:74`. |
| Histórico; painel; marcos do lote | `app/chamados/services/historico.py:103`; `:105`; `app/chamados/services/painel.py:67`; `app/chamados/veredito_lavrado.py:203`. |
| Raio X; explicador; fábrica; passivo; reabertura de competência | `app/chamados/services/raiox.py:135`; `:182`; `app/chamados/services/explicador.py:95`; `app/chamados/services/fabrica_da_celula.py:149`; `:171`; `app/chamados/services/passivo_validado.py:141`; `app/chamados/services/competencia_trancada.py:276`. |
| View; cartório; flip; decisão do dia; template | `app/chamados/views.py:1257`; `app/ponto/services/cartorio.py:1650`; `app/ponto/services/flip_auto.py:129`; `app/ponto/services/veredito_celula.py:196`; `app/templates/chamados/partials/_card_atendimento.html:192`. |

O trio também aparece em manutenção, testes e migrações; esses usos não devem ser contados automaticamente como leitores de pouso da F2 [nao verificado]. Não confundir com `RegularizacaoExtraordinaria`, outro modelo que possui campos homônimos (`app/chamados/models.py:4152`; `app/chamados/models.py:4153`).

## 16/11 21:38 · sessao c7d251d56727 · - · - · origem=Ronald (tmux)

**Item 3: CONFERE no sentido registrado pelo C2; a afirmação universal sobre produção permanece [nao verificado].** O C2 pede o predicado composto: leitor da F2 que chama projeção com `agora` anterior a um descarimbo **e depende do instante apagado** (`complemento2_respostas_f2_estados_1611.txt:4`). O registro fornecido mantém esse censo como zero **no código** (`respostas_registradas.md:15`).

1. **Premissas:** a projeção recebe os campos atuais e apenas elimina valores posteriores ao horizonte; não busca trilha nem recompõe instante apagado (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1391`; `app/chamados/juizes.py:1396`; `app/chamados/juizes.py:1400`). Descarimbar escreve `None` no instante (`app/chamados/models.py:3318`). Os três chamadores diretos produtivos são `_slot_esperado`, que usa resposta, `_pergunta_viva_calc`, que usa materialização/via, e `beco_em_lote`, que usa somente o instante de resposta (`app/chamados/juizes.py:351`; `app/chamados/juizes.py:352`; `app/chamados/juizes.py:1404`; `app/chamados/juizes.py:1416`; `app/chamados/juizes.py:1444`; `app/chamados/juizes.py:1482`; `app/escala/services/cadastro_realidade.py:502`). Não encontrei nesses corpos a dependência histórica composta pedida pelo C2; ocorrência real em produção: [nao verificado].

2. **Colisão:** resolver L1 pela conservação do comportamento vigente é compatível com o cabeçalho do corte e com a manutenção de descarimbo apagando o instante; é resolução pelo corte, não demonstração de MOOT pela F1 (`app/docs/CORTES.md:487`; `complemento1_respostas_f2_estados_1611.txt:9`; `complemento2_respostas_f2_estados_1611.txt:3`). A cópia ainda registra a medição anterior como RESIDUAL; isso é o estado pendente que a resposta pretende resolver (`app/docs/RELATO.md:138`).

3. **Literal/limite:** “nenhum leitor depende do instante apagado” deve conservar o significado restrito do censo registrado, sem virar afirmação de ausência de leitores temporais (`respostas_registradas.md:15`). A tarefa passa `agora` ao C6 (`app/chamados/services/tarefa_do_dia.py:537`; `app/chamados/services/tarefa_do_dia.py:541`). Há também leitor temporal fora dos sete: **`marcos_do_lote`** filtra `materializada_em <= ate` e calcula seu máximo; `esta_stale` usa esse máximo para decidir nova lavra (`app/chamados/veredito_lavrado.py:151`; `app/chamados/veredito_lavrado.py:163`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/veredito_lavrado.py:204`; `app/chamados/veredito_lavrado.py:230`; `app/chamados/veredito_lavrado.py:232`). A tarefa e o raio X fornecem horizonte a esse leitor (`app/chamados/services/tarefa_do_dia.py:434`; `app/chamados/services/raiox.py:38`). Isso é dependência do campo atual, não prova de exigência de recuperar instante já apagado (`app/chamados/veredito_lavrado.py:202`; `app/chamados/veredito_lavrado.py:204`).

4. **Exportada/dinheiro/!:** o item não revoga a proibição: qualquer diferença de `supra_juiz` em competência exportada exige `PAREI !` com tabela (`app/docs/CORTES.md:487`; `complemento2_respostas_f2_estados_1611.txt:5`). Igualdade efetiva na sombra, impacto financeiro e produção: [nao verificado].

**Recomendação para consolidar:** item 3 CONFERE como registro do censo de código delimitado no C2; não promova “zero” a prova de produção nem a inexistência de outros leitores temporais (`respostas_registradas.md:15`; `app/chamados/veredito_lavrado.py:202`).

## 16/11 21:38 · sessao e6b94b016dcf · - · - · origem=Ronald (tmux)

Item 1 — **CONFERE**.

1. **Premissas:** o caso positivo satisfaz o predicado: sem validação, sem materialização, instante ou texto de resposta preservado e tranca ausente/ignorada (`app/chamados/models.py:1922-1926`). O descarimbo efetivo apaga materialização e via (`app/chamados/models.py:3318`). Ressuscitar também apaga instante e texto de resposta; portanto, a pergunta não entra nessa lista (`app/chamados/models.py:1629-1630`; `app/chamados/models.py:1924-1925`).

2. **Compatibilidade:** o C3 adota exatamente a formulação proposta na devolução já registrada (`app/docs/RELATO.md:4`). Conservação lendo o campo atual corresponde à alternativa explicitamente oferecida anteriormente (`app/docs/RELATO.md:31`) e ao cabeçalho de conservação do corte (`app/docs/CORTES.md:487`).

3. **Literal:** fecha tomando “descarimbada” como descarimbo efetivo e mantendo a pergunta no universo passado ao leitor (`app/chamados/models.py:1922`). Para montar o caso que morde, a via não pode ser terminal: o descarimbo individual recusa `fora_do_fato`, e o lote exclui essas vias (`app/chamados/models.py:3316-3318`; `app/chamados/models.py:3391-3393`). Isso é requisito de preparação do caso, não contradição da afirmação sobre a pergunta já descarimbada (`app/chamados/models.py:1922-1926`).

4. **Exportada/dinheiro/!:** há alcance já declarado no CSF: os dois universos passam pela lista, cuja ausência permite executar o CSF (`app/ponto/supra_juiz.py:573-574`; `app/ponto/supra_juiz.py:588`). Continua valendo `PAREI !` diante de diferença em competência exportada (`app/docs/CORTES.md:487`). Execução do selo ou efeito efetivo em produção: [nao verificado].

VEREDITO por item: item 1 CONFERE (`app/chamados/models.py:1922-1926`; `app/chamados/models.py:1629-1630`).

_6197 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
