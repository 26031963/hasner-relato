# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 14:25** · publicado em **17/11/2026 14:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 14:25 · sessao 75c84ea53fc0 · COMPLEMENTO-1-1410 · verificador · origem=Code

**AVAL-DEVOLVIDO — somente item 1: PROCEDE o achado de premissa divergente da cópia.** A retratação existente está em `_retratar`, com transição e resolvedor; o ponto indicado como consulta de competência encerra uma docstring (`app/chamados/reconciliador.py:504`, `app/chamados/reconciliador.py:524`, `app/chamados/reconciliador.py:537`, `app/chamados/reconciliador.py:576`).

As referências aos anexos abaixo contam as linhas do texto transcrito.

1. **As premissas conferem?**

   **(a) PROCEDE a divergência.** Nesta cópia, a unidade consulta `retencao_da_retratacao`, que trata irmã pendente e justificativa alheia; não é a guarda de competência descrita. `retratar` delega diretamente a `_retratar`. A sequência `retratar_com_motivo → consulta em :576` não corresponde ao código inspecionado (`app/chamados/reconciliador.py:438`, `app/chamados/reconciliador.py:519`, `app/chamados/reconciliador.py:545`, `app/chamados/reconciliador.py:576`).

   Existem consultas nomeáveis de competência, mas locais: `_dia_fora_do_alcance_c` consulta entregue, enquanto o ramo HIST conserva o caminho anterior quando está fora desse alcance. Isso não constitui a guarda geral pressuposta (`app/chamados/reconciliador.py:743`, `app/chamados/reconciliador.py:745`, `app/chamados/reconciliador.py:1593`).

   **(b) PROCEDE a insuficiência da identificação das escritas.** AST consegue enumerar funções, métodos e chamadas explícitas: há `bulk_update` em `gravar_lavras_em_lote`, `save` em `lavrar` e persistência da costura. Entretanto, reconhecer apenas esses verbos não identifica automaticamente os modelos, campos e limites do ato protegido; `carimbar`, por exemplo, constrói `campos` por ramos antes do `save` (`app/chamados/veredito_lavrado.py:240`, `app/chamados/veredito_lavrado.py:250`, `app/chamados/veredito_lavrado.py:275`, `app/chamados/services/fio.py:974`, `app/chamados/models.py:3238`, `app/chamados/models.py:3262`).

   **(c) PROCEDE o limite da promessa por AST.** O callback aparece como referência `ato=ch.reconciliar`, sendo executado como `ato()`; existe também a aresta implícita `save → post_save → resolvedor`. Um censo de nomes/chamadas não demonstra passagem obrigatória pela guarda antes de toda escrita (`app/chamados/reconciliador.py:524`, `app/chamados/services/fecho_runtime.py:160`, `app/chamados/services/fecho_runtime.py:165`, `app/chamados/models.py:296`, `app/chamados/signals.py:91`, `app/chamados/signals.py:117`).

   **NAO PROCEDE que alias e reexport, isoladamente, impeçam análise estática.** São explícitos nestes exemplos: `retratar as _retr` e reexport do resolvedor. Identificar essas referências continua sendo diferente de provar o fluxo da guarda (`app/ponto/management/commands/supra_juiz.py:258`, `app/ponto/management/commands/supra_juiz.py:286`, `app/chamados/models.py:4259`, `app/chamados/models.py:4260`).

2. **Colide com o vigente?**

   **PROCEDE a incompatibilidade com uma expectativa existente**, caso a cerca passe a impedir a reconciliação com pergunta entregue: há teste exigindo expressamente que essa pergunta **não segure** `reconciliar()`, e o código atual exclui entregue da lista de vivas (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:113`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:117`, `app/chamados/models.py:364`).

   **NAO PROCEDE que os selos existentes já satisfaçam o pedido ou que a sobreposição seja, sozinha, colisão.** Eles verificam portas de escrita e ordem de `_vivas_entre`; a pergunta sobre competência é diferente (`app/chamados/tests/test_selo_status_so_pelas_portas.py:1`, `app/chamados/tests/test_porta_veredito_pergunta.py:94`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:5`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:24`).

   **PROCEDE a ambiguidade entre allowlist vazia e exceção nomeada:** se a exceção dispensa a guarda e deixa verde, funciona como permissão de violação; se apenas registra o sítio mantendo vermelho, não contradiz a lista vazia. O item não especifica esse efeito (`comp1_aval.txt:2`). A proibição localizada de allowlist em L-106 trata de commits de documentação, não estabelece proibição universal de declarações nominais (`app/docs/LEIS.md:132`).

3. **Alguma condição não fecha no literal?**

   **PROCEDE.** A matriz já exige consulta obrigatória **antes da primeira escrita funcional do ato**; portanto, mera presença da guarda em algum ancestral não basta. Falta fechar como o selo reconhece essa obrigação através das unidades encadeadas e callbacks (`matriz.r:17`, `app/chamados/services/becos.py:53`, `app/chamados/services/fecho_runtime.py:165`).

   Também não fica fechado se “16º chamador” inclui entrada indireta: wrappers podem apenas delegar, sem chamada local à guarda. Exigir guarda no mesmo corpo e aceitar proteção transitiva são critérios diferentes (`comp1_aval.txt:2`, `app/chamados/reconciliador.py:545`, `app/chamados/reconciliador.py:547`).

   Há ainda distinção entre atribuição em memória e persistência: `lavrar(..., salvar=False)` altera campos, mas quem salva é o chamador. Isso interfere no marco da “primeira escrita” (`app/chamados/veredito_lavrado.py:270`, `app/chamados/veredito_lavrado.py:274`, `app/chamados/services/fio.py:973`, `app/chamados/services/fio.py:974`).

4. **Toca exportada, dinheiro ou `!` fora do declarado?**

   **NAO PROCEDE afirmar esse alcance adicional pelo texto:** o complemento exclui porta do export e produção. Não autoriza apply financeiro nem mudança do gravado exportado (`comp1_aval.txt:5`, `CLAUDE.md:265`, `CLAUDE.md:406`, `CLAUDE.md:411`). Efeito material da implementação futura, selo vermelho com 16º chamador e mordida do mutante: **[nao verificado]**.

## 17/11 14:24 · sessao ae657b1ef1bd · CENSO-FECHA-DE-VERDADE · verificador · origem=Code

**AVAL-DEVOLVIDO, por item: PROCEDE em 1, 3, 4, 5, RED, CONTRATO e CONDUTA**, pelas lacunas abaixo. Item 2, RESERVA e PROIBIDO não apresentam colisão necessária com a regra vigente (`bin/disjuntor.py:1522`, `1698`, `2306`; `CLAUDE.md:394`).

“PROCEDE” abaixo significa achado contra o fechamento do aval. As reprovações B2/F2, os briefs efetivamente aprovados, os construtores atualmente abertos e o custo de 10–20 minutos permanecem **[nao verificado]**.

**1 — caminhos**

- **Q1 — Premissas:** conferem os endereços do juiz e da porta. Hoje o juiz exige leitores/escritores com referência, decisões e perguntas cortadas; não interpreta caminhos, ponto único ou lista fechada (`bin/disjuntor.py:1522`, `1565`, `1698`).
- **Q2 — Colisão:** **NAO PROCEDE** como incompatibilidade necessária: é aperto do juiz existente. Entretanto, um brief conforme hoje pode faltar aos novos requisitos: a própria fixture `CONFORME` não contém caminhos nem ataques (`bin/tests/test_censo_antes_da_vaga.sh:54`, `86`).
- **Q3 — Literal:** **PROCEDE.** Contar N linhas não comprova “TODO caminho”. Também ficam sem definição explícita N=0, duplicatas e correspondência entre destinos e “lista fechada: N sitios” **[nao verificado]**. A referência atual apenas casa aparência `arquivo.ext:numero`; não verifica existência, conteúdo ou relação chamador→sítio (`bin/disjuntor.py:1501`, `1566`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto: este juiz lê o brief e retorna motivo; não escreve dado de negócio (`bin/disjuntor.py:1525`, `1577`).

**2 — nenhuma ocorrência de “nao verificado”**

- **Q1 — Premissas:** hoje vale o **primeiro** título que case `^(#{1,6})\s*CENSO\b`, com caixa literal. O bloco termina no próximo título de nível igual ou superior; subtítulos permanecem dentro (`bin/disjuntor.py:1496`, `1530`, `1538`).
- **Q2 — Colisão:** **NAO PROCEDE** como contradição necessária. O aperto passa a recusar conteúdo antes aceito; hoje não existe teste dessa expressão no juiz (`bin/disjuntor.py:1543`, `1577`).
- **Q3 — Literal:** **NAO PROCEDE** a objeção de capturar prosa fora do bloco, preservado o recorte existente. Dentro dele, inclusive subtítulos, a proibição é literal. “Citando a linha” ainda não existe: o parser conserva texto, mas descarta os números físicos ao montar `bloco` (`bin/disjuntor.py:1537`, `1544`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto; é validação textual do brief (`bin/disjuntor.py:1525`, `1577`).

**3 — ataques cruzados**

- **Q1 — Premissas:** o juiz atual lê somente o brief; não busca ataques nem interpreta respostas a eles (`bin/disjuntor.py:1524`, `1547`, `1577`).
- **Q2 — Colisão:** **NAO PROCEDE** contra a revisão vigente: ela já exige classes anteriores de reprovação e escritores/leitores, inclusive fora do diff (`CLAUDE.md:373`, `379`).
- **Q3 — Literal:** **PROCEDE.** `ataques cruzados: 0 (busca: grep...)` declara um comando; não prova execução, resultado, universo pesquisado ou ausência de ataques. Igualmente, escrever “respondido por” não comprova que a resposta enfrenta o ataque. Essa comprovação não é feita pelo juiz atual (`bin/disjuntor.py:1525`, `1577`); buscas e respostas concretas: **[nao verificado]**.
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto. Registrar uma resposta no brief não amplia autorização sobre negócio ou competência exportada (`CLAUDE.md:265`, `268`).

**4 — complemento é abertura e exige brief próprio**

- **Q1 — Premissas:** **PROCEDE a lacuna.** A porta recebe obra, texto, via e raiz; não recebe marcador de complemento, volta ou construtor já aberto. Um brief citado de outra pasta sob `logs/` pode satisfazer a abertura sem existir brief na pasta da obra (`bin/disjuntor.py:1698`, `1607`, `1619`, `1715`).
- **Q2 — Colisão:** **PROCEDE o risco concreto de revalidar material antigo.** O coletor reúne **todos** os `brief*.md` próprios e citados, e qualquer recusado barra a chamada. Assim, brief novo conforme não salva brief antigo da mãe também coletado. Aprovação anterior não é autorização duradoura nessa decisão (`bin/disjuntor.py:1604`, `1619`, `1703`, `1717`). Obras reais atingidas: **[nao verificado]**. Exigir brief próprio não elimina, por si, a competência técnica da sessão para encaixar complemento (`CLAUDE.md:276`, `278`).
- **Q3 — Literal:** **PROCEDE.** O reconhecimento foi remetido ao contrato, mas o contrato fornecido não distingue complemento de volta **[nao verificado em arquivo local]**. Continuar um processo aberto não chama novamente este portão; uma nova invocação de construtor passa por ele, sem sinal de continuidade (`bin/disjuntor.py:1851`, `1855`; `bin/codex_pergunta.sh:247`). A ressalva sobre obras abertas não fica automaticamente assegurada para suas próximas chamadas.
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como autorização adicional. Complemento continua submetido ao escopo literal e aos casos que exigem `!` (`CLAUDE.md:265`, `268`).

**5 — fila 1, LATERAL-ESTRUTURA, tela e instrumento**

- **Q1 — Premissas:** fila 1, na conduta, é contrato 20/placar L-099 (`CLAUDE.md:291`). No código, `_fila1_viva` lê os IDs andáveis do bloco OBRAS; o hook exclui fechados/parados pela coluna de estado, e `_pertence_fila1` resolve identidade e ambiguidades (`bin/disjuntor.py:1630`, `1634`, `1688`; `bin/hook_stop_fila1.py:180`, `250`). **Não há classificação desses baldes na decisão atual**: fora da fila 1, a porta retorna imediatamente (`bin/disjuntor.py:1707`).
- **Q2 — Colisão:** **PROCEDE a interseção não resolvida.** Tela e fila 1 não são categorias necessariamente exclusivas: há registro histórico explícito de “TELA” que é “FILA 1 AGORA”; isso não prova obra aberta hoje (`app/docs/BACKLOG.md:1248`).
- **Q3 — Literal/costura:** **PROCEDE.** O CLI do wrapper também decide “fora da fila 1” e retorna **antes** de `_censo_nega`. Ampliar somente a porta deixa Codex lateral escapando por esse retorno (`bin/disjuntor.py:2306`, `2311`, `2315`). Além disso, “tela como está” preserva a cobrança atual quando ela pertence à fila 1; R6, lido como isenção universal de tela, diz outra coisa (`bin/disjuntor.py:1707`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto; ampliar universo do portão não amplia aval de negócio (`CLAUDE.md:265`, `268`).

**RED — R1 a R6**

- **Q1 — Premissas:** existem testes de juiz, CLI, pretool e costura. A fixture conforme atual passa sem os novos campos; o caso de tela usa especificamente `CONGELADA, FILA 2` (`bin/tests/test_censo_antes_da_vaga.sh:44`, `54`, `86`, `252`).
- **Q2 — Colisão:** **NAO PROCEDE** contra exigir casos e mutantes: o selo vigente já distingue testes do juiz e testes da costura (`bin/tests/test_censo_antes_da_vaga.sh:13`, `18`).
- **Q3 — Literal:** **PROCEDE em R4/R6.** R4 depende da distinção complemento/volta ainda ausente; R6 não resolve tela∩fila1. R5 também precisa continuar satisfazendo os campos antigos: o aval diz “também”, e o juiz atual os exige (`bin/disjuntor.py:1698`, `1707`, `1565`). Nenhum resultado dos novos R1–R6 está provado **[nao verificado]**.
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** pelo desenho dos testes: o selo existente usa árvore de mentira, não BACKLOG de produção (`bin/tests/test_censo_antes_da_vaga.sh:20`, `36`).

**CONTRATO**

- **Q1 — Premissas:** os dois chamadores conferem. Agent construtor chama `_censo_nega` no pretool; há também chamada na recuperação de exceção (`bin/disjuntor.py:1851`, `1855`, `1763`). Codex passa pelo wrapper→CLI→porta, antes da vaga; o pretool deliberadamente não julga essa via (`bin/codex_pergunta.sh:247`; `bin/disjuntor.py:1832`, `2315`).
- **Q2 — Colisão:** **NAO PROCEDE** contra contrato prévio único e pouso próprio: ambos já são exigidos (`CLAUDE.md:327`, `394`; `app/docs/PISTA.md:170`).
- **Q3 — Fechamento:** **PROCEDE parcialmente.** Brief ilegível e exceção recusam no ramo alcançado (`bin/disjuntor.py:1527`, `1733`). PAREI provado exige wrapper vivo da mesma raiz, obra exata e papel `censo|leitura`; não basta publicar a linha (`bin/hook_stop_fila1.py:734`, `763`). Porém faltam o reconhecimento de complemento e a medida prometida: hoje ESTADO publica negativas **agregadas**, não por motivo (`bin/disjuntor.py:2104`, `2145`, `2164`). O contrato normativo também pede contador com esperado; o esperado da nova medida não foi explicitado **[nao verificado]** (`app/docs/PISTA.md:173`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto. Desligamento já tem chave própria atribuída a Ronald; não é allowlist por obra (`bin/disjuntor.py:124`, `209`, `2291`).

**CONDUTA**

- **Q1 — Premissas:** conferir manualmente e registrar cabe à sessão; leitura/censo pertence ao despacho Codex (`CLAUDE.md:285`, `358`). A existência de publicação `CENSO-MANUAL` para qualquer obra concreta é **[nao verificado]**.
- **Q2 — Colisão:** **PROCEDE.** “Qualquer construtor ou complemento” inclui tela/instrumento, enquanto o item 5 os preserva; essa diferença está no literal fornecido **[nao verificado em arquivo local]**. O código atual não oferece classificação que resolva a diferença (`bin/disjuntor.py:1698`, `1707`).
- **Q3 — Limite:** **PROCEDE quanto à evidência.** A linha manual de contagens não prova mapas ou respostas. O Codex de leitura recebe cópia por `git archive`, e `logs/` é ignorado; os dados vivos chegam por anexos/sonda raspados, não pela cópia (`bin/codex_pergunta.sh:315`, `331`, `334`, `425`; `.gitignore:53`). Portanto não pode conferir buscas nos logs que não recebeu **[nao verificado]**.
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como autorização de escrita; conferir e publicar censo não remove os limites do aval (`CLAUDE.md:265`, `268`).

**RESERVA**

- **Q1 — Premissas:** **NAO PROCEDE** como desvio de papel: ataque adversarial, censo e mapa de escritores/leitores cabem ao Codex. A lista vigente de classes já está descrita (`CLAUDE.md:358`, `367`, `373`).
- **Q2 — Colisão:** **NAO PROCEDE** como abertura necessária de construtor: leitura pode ocorrer sem vaga; a própria preparação registrada separa leitura/desenho de construção (`app/docs/BACKLOG.md:794`).
- **Q3 — Limite:** ataques efetivos, respostas e mapas completos de F4/F7/F3/F9 são **[nao verificado]**. A cópia registra preparação de F2/F4/F7, mas não substitui seus anexos (`app/docs/BACKLOG.md:822`; `bin/codex_pergunta.sh:331`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** para a leitura reservada. Eventual escrita resultante continua sob os limites de negócio (`CLAUDE.md:265`, `268`).

**PROIBIDO**

- **Q1 — Premissas:** **NAO PROCEDE** afirmar necessidade de novo juiz ou allowlist. Já existem juiz, porta e coletor compartilhados; a aprovação percorre todos os briefs coletados (`bin/disjuntor.py:1522`, `1698`, `1717`).
- **Q2 — Colisão:** **NAO PROCEDE**: a proibição de allowlist é compatível com o corte vigente. A ressalva temporal não autoriza inventar lista de obras dispensadas (`app/docs/CORTES.md:300`; `bin/disjuntor.py:1703`).
- **Q3 — Limite:** a existência de retorno “FORA” no CLI é uma decisão de alcance, não um segundo parser do brief; permanece, porém, a costura que impede ampliar somente `_censo_nega` (`bin/disjuntor.py:2311`, `2315`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como permissão para contornar `!`; esses limites permanecem expressos (`CLAUDE.md:265`, `268`).

**QUEM ESCREVE / QUEM LÊ**

| Arquivo/campos | Quem escreve | Quem lê |
|---|---|---|
| `logs/<OBRA>/brief*.md`: leitores, escritores, decisões, perguntas | Autor concreto de cada brief: **[nao verificado]**. O portão não escreve o brief; lê e solicita correção (`bin/disjuntor.py:1525`, `1728`). | `_briefs_do_censo` coleta; `censo_do_brief` julga (`bin/disjuntor.py:1607`, `1718`). |
| Novos campos: caminhos, ponto único/lista fechada, ataques/respostas | Produtor concreto e autoridade para atestar completude: **[nao verificado]**. | Leitura futura pretendida no mesmo juiz; interpretação desses campos ainda ausente (`bin/disjuntor.py:1498`, `1547`). |
| BACKLOG: ID, descrição, estado e marcador | Sessão registra OBRAS; gerador preserva bloco/marcador escritos à mão. `backlog_estado.py` atualiza estado (`CLAUDE.md:436`; `bin/gerar_backlog.py:103`, `113`; `bin/backlog_estado.py:58`). | `_fila1_viva`→`_itens_andaveis`→`_pertence_fila1`; não lê balde semântico (`bin/disjuntor.py:1634`, `1688`; `bin/hook_stop_fila1.py:250`). |
| `chamadas.log`: hora, obra, papel, hash12, pid, `ini`, pergunta raspada | Wrapper (`bin/codex_pergunta.sh:266`, `308`). | `_linhas_chamadas` extrai hora/obra/papel/chave; `_censo_24h` casa abertura por obra+pid+hash (`bin/codex_limite_estado.py:63`; `bin/disjuntor.py:2126`, `2134`). |
| `chamadas.log`: `fim`, thread_id, rc, duração | Trap `_saida` do wrapper (`bin/codex_pergunta.sh:269`, `272`). | Parser, últimas chamadas e procedência do ESTADO (`bin/codex_limite_estado.py:66`, `82`, `92`; `bin/gerar_codex_estado.py:176`). |
| `logs/sessao/disjuntor.log`: aprovação/negativa/motivo | `trilha`, chamada pela porta e pelo CLI (`bin/disjuntor.py:193`, `1724`, `2317`). | `_censo_24h`; esta é a fonte das negativas. Recusa anterior à vaga não alcança o `ini` de `chamadas.log` (`bin/disjuntor.py:2090`, `2102`; `bin/codex_pergunta.sh:247`, `308`). |
| RELATO: `PAREI: censo-<obra> \| espera Codex` | Mecanismo de negativa via `esteira_vigia.no_topo` (`bin/disjuntor.py:302`, `324`). | Hook de parada verifica literal e produtor vivo; não relê os novos campos do brief (`bin/hook_stop_fila1.py:773`, `783`). |

O host do wrapper consulta a raiz viva; o Codex leitor trabalha na cópia e recebe anexos. São acessos distintos (`bin/codex_pergunta.sh:247`, `315`, `331`, `425`). Nenhuma identificação de construtor hoje aberto ou brief de produção foi inferida dessas referências **[nao verificado]**.

## 17/11 14:24 · sessao bd950d29c346 · - · - · origem=Ronald (tmux)

- **PROCEDE — existem selos sobre os mesmos escritores, mas com outra pergunta.** O selo de status fiscaliza quem escreve `status_local`/`fechada_em`, inclusive `save`, `update`, `bulk_update`, aliases e SQL (`app/chamados/tests/test_selo_status_so_pelas_portas.py:1`, `:10`, `:36`, `:340`). O selo do veredito fixa `lavrar` e `gravar_lavras_em_lote` e suas escritas nominais (`app/chamados/tests/test_porta_veredito_pergunta.py:94`, `:123`). Nenhum desses trechos prova passagem pela guarda de competência.

- **PROCEDE — existe tripwire de ordem, mas é outro predicado.** O tripwire existente exige `_vivas_entre` e `raise` antes da primeira atribuição a `status_local`, apenas em `_gravar_estado`; possui mutantes de chamada removida, `raise` removido e escrita antecipada (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:10`, `:24`, `:64`, `:78`). O teste funcional existente exige que pergunta de competência ENTREGUE **não impeça** `reconciliar()` (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:113`). Portanto, não equivale à cerca pedida.

- **NAO PROCEDE — sobreposição de arquivos, sozinha, não demonstra duplicação de selo.** A advertência contra terceiro varredor trata expressamente da mesma pergunta “escrita de status fora da porta”; o próprio arquivo acrescenta uma pergunta diferente sobre ordem da guarda (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:5`). A nova pergunta de competência não está demonstrada por esses selos.

- **PROCEDE — distinguir declaração de porta e exceção que perdoa violação.** O gate da família exige `ALLOWLIST == ()` e zero escrita fora de porta (`app/chamados/tests/test_chokepoint_chamado_gate.py:371`). Entretanto, portas nominais com motivos continuam declaradas no selo de status (`app/chamados/tests/test_selo_status_so_pelas_portas.py:36`). Uma lista dos 15 pontos, por si, não contradiz allowlist vazia; uma exceção que faça violação passar precisa ser confrontada com o literal do anexo [nao verificado quanto ao mecanismo ainda inexistente].

- **NAO PROCEDE — proibição universal de listas nominais não está comprovada.** A frase `proibido allowlist` em `CLAUDE.md:334` e `app/docs/LEIS.md:132` refere-se especificamente a commits só de documentação. Há inclusive exceção declarada e medida em selo de resolução (`app/chamados/tests/test_selo_resolucao_um_escritor.py:77`).

- **Limite já declarado na casa:** o selo de status admite que AST não resolve receptor/tipo genérico nem campos externos em todos os casos (`app/chamados/tests/test_selo_status_so_pelas_portas.py:17`). Logo, usá-lo como prova de completude irrestrita de chamadores seria indevido.

- A busca local não localizou o aval `RESPOSTAS-1711-1410`/`COMPLEMENTO-1-1410`; vigência adicional além do anexo fornecido: **[nao verificado]**.

## 17/11 14:23 · sessao 19d8c964bebe · - · - · origem=Ronald (tmux)

- **PROCEDE — a premissa de identidade da cópia falha.** O corpo atual é `_retratar`, com retenção em `app/chamados/reconciliador.py:519`, callback de reconciliação em `:524` e resolvedor em `:537`; `retratar` delega a ele em `:547`. A guarda `retratar_com_motivo → _retratacao_toca_entregue` descrita no anexo não corresponde a esse corpo. O reexport real está em `app/chamados/models.py:4260`; o resolvedor é definido em `app/chamados/services/disputa_emissao.py:1887`.

- **PROCEDE — AST sintático não equivale à propriedade semântica prometida.** O callback é passado como valor (`ato=ch.reconciliar`), em `app/chamados/reconciliador.py:524`, e executado como `ato()`, em `app/chamados/services/fecho_runtime.py:160` e `:165`. Há ainda ligação implícita `save → post_save → resolvedor`: `app/chamados/models.py:296`; `app/chamados/signals.py:91`; `:117`. Um censo de chamadas por nome não demonstra que a guarda domina todas as escritas desses caminhos.

- **NÃO PROCEDE — alias/reexport, isoladamente, tornariam o censo impossível.** São resolvíveis estaticamente nestes exemplos: `retratar as _retr`, em `app/ponto/management/commands/supra_juiz.py:258`, utilizado em `:286`; reexport do resolvedor em `app/chamados/models.py:4259`, `:4260`. Essa resolução identifica referências; não certifica a execução obrigatória de guarda antes da escrita, conforme a execução indireta em `app/chamados/services/fecho_runtime.py:165`.

- **PROCEDE — “qualquer ancestral” não determina passagem obrigatória anterior.** O callback pode executar por dois ramos, em `app/chamados/services/fecho_runtime.py:160` e `:165`; a reconciliação pode retornar sem transição, em `app/chamados/models.py:653`, ou escrever em `:655`. A existência de um ancestral com consulta não estabelece, sozinha, o caminho efetivamente executado nem a propagação da recusa.

- **Exportada/dinheiro/!** Existe consulta local de entregue na reabertura do credor, em `app/chamados/services/disputa_emissao.py:1857`, com veto por `motivo` antes de `credor.reabrir()`, em `:1867` e `:1873`; ela não é uma guarda universal dos quinze sítios. K6 é `competencia_entregue`, em `app/folha/export.py:556`; a composição por dia é `competencia_do_dia_entregue`, em `app/chamados/services/vigia_fio.py:785`. O limite vigente exige `!` para tocar gravado de exportada, em `CLAUDE.md:265`, e distingue apply financeiro de competência aberta, em `CLAUDE.md:406`. Impacto material futuro desse selo em exportada/dinheiro: **[nao verificado]**.

## 17/11 14:23 · sessao bbc39757284b · - · - · origem=Ronald (tmux)

- **1(a) — NÃO PROCEDE a premissa nesta cópia.** A unidade existente é `_retratar`, definida em `app/chamados/reconciliador.py:504`: consulta `retencao_da_retratacao` em `:519`, transiciona em `:524`, pode salvar justificativa em `:536` e chama o resolvedor em `:537`. `retratar` apenas delega (`app/chamados/reconciliador.py:545`, `:547`). A linha `:576` encerra a docstring de E2; não consulta competência (`app/chamados/reconciliador.py:550`, `:576`). A busca global de Python em `app` não encontrou os símbolos `retratar_com_motivo`, `_retratacao_toca_entregue` e `perguntas_alcancadas_pela_retratacao`; a matriz não corresponde integralmente à cópia inspecionada (`app/chamados/reconciliador.py:504`, `:545`, `:550`).

- **Existem predicados nomeáveis de competência, mas não equivalem à guarda geral descrita.** `_dia_fora_do_alcance_c` consulta `competencia_do_dia_entregue` e a tranca (`app/chamados/reconciliador.py:743`, `:745`, `:748`); no HIST, estar fora do alcance conserva o percurso anterior (`app/chamados/reconciliador.py:1593`, `:1594`, `:1608`). O helper `reabrir_credor_de_pergunta_viva` verifica competência da carga, porém somente considera dias diferentes de `None` (`app/chamados/services/disputa_emissao.py:1827`, `:1857`, `:1858`, `:1859`).

- **1(b) — PROCEDE listar os símbolos; NÃO PROCEDE confundir isso com identificar todas as escritas protegidas.** Há escritores explícitos para estado (`app/chamados/models.py:201`, `:296`), carimbo (`app/chamados/models.py:3207`, `:3213`, `:3262`), costura (`app/chamados/services/fio.py:972`, `:974`) e lote (`app/chamados/veredito_lavrado.py:240`, `:250`). Entretanto, `campos` é construído por ramos e passado ao `save` (`app/chamados/models.py:3238`, `:3243`, `:3247`, `:3261`, `:3262`); reconhecer somente `.save/.update/.bulk_update` não identifica modelo, campos nem unidade funcional desses atos (`app/chamados/models.py:296`, `:3262`).

- **3 — PROCEDE o achado de ambiguidade entre atribuição em memória e primeira persistência.** `lavrar(p, salvar=False)` atribui os três campos sem chamar `save`; a persistência fica no chamador (`app/chamados/veredito_lavrado.py:254`, `:270`, `:274`, `:275`; `app/chamados/services/fio.py:973`, `:974`; `app/chamados/models.py:3260`, `:3262`). Uma regra de presença/ordem de chamadas precisa distinguir esses comportamentos para sustentar o literal “antes da primeira escrita” [nao verificado].

- **A lista de 15 tem um alvo ausente e 14 nomes existentes.** As posições atuais são: `fechar_beco` (`app/chamados/services/becos.py:36`), resolvedor (`app/chamados/services/disputa_emissao.py:1887`), `reconciliar` (`app/chamados/models.py:642`), `transicionar` (`app/chamados/models.py:128`), `encerrar` (`app/chamados/models.py:1669`), `encerrar_por_competencia_trancada` (`app/chamados/models.py:1686`), `reabrir` (`app/chamados/models.py:1409`), `DisputaSupervisao.fechar` (`app/chamados/models.py:1936`), E2 (`app/chamados/reconciliador.py:550`), `reatar_pergunta_orfa` (`app/chamados/reconciliador.py:1534`), `encerrar_orfas_pela_lampada` (`app/chamados/reconciliador.py:1113`), `reapontar_resolvedora` (`app/chamados/services/fio.py:851`), `lavrar` (`app/chamados/veredito_lavrado.py:254`) e `gravar_lavras_em_lote` (`app/chamados/veredito_lavrado.py:240`).

## 17/11 14:23 · sessao 10ac03e7bce0 · CONFRONTO-PILOTO · sonda · origem=Code

Transcrição do dado fornecido, sem reexecução: `sonda_fp.py` não foi localizado na cópia e não há acesso à sombra. [nao verificado]

```text
SAIDA_BEGIN
sonda_leitura: sonda_fp.py [9b60a7ed9c0c0dce] no cpuset 4-7, lendo sombra como leitor (LEITURA)

emp2 colabs=741 com_batida_na_janela=401 codigos=11
emp3 colabs=163 com_batida_na_janela=101 codigos=5
emp4 colabs=35 com_batida_na_janela=20 codigos=4
M C001 | emp2 | nao mapeado | - | 1 pre-candidatos, 0 casam dia a dia
M C002 | emp2 | MAPEADO | col882 | 1 pre-candidatos, 1 casam dia a dia
M C003 | emp2 | MAPEADO | col719 | 1 pre-candidatos, 1 casam dia a dia
M C004 | emp2 | MAPEADO | col281 | 1 pre-candidatos, 1 casam dia a dia
M C005 | emp2 | nao mapeado | - | 0 pre-candidatos, 0 casam dia a dia
M C006 | emp2 | MAPEADO | col351 | 1 pre-candidatos, 1 casam dia a dia
M C007 | emp2 | MAPEADO | col307 | 1 pre-candidatos, 1 casam dia a dia
M C008 | emp2 | nao mapeado | - | 1 pre-candidatos, 0 casam dia a dia
M C009 | emp2 | MAPEADO | col902 | 1 pre-candidatos, 1 casam dia a dia
M C010 | emp2 | nao mapeado | - | 0 pre-candidatos, 0 casam dia a dia
M C011 | emp2 | MAPEADO | col327 | 1 pre-candidatos, 1 casam dia a dia
M C012 | emp3 | nao mapeado | - | 1 pre-candidatos, 0 casam dia a dia
M C013 | emp3 | MAPEADO | col117 | 1 pre-candidatos, 1 casam dia a dia
M C014 | emp3 | MAPEADO | col125 | 1 pre-candidatos, 1 casam dia a dia
M C015 | emp3 | MAPEADO | col89 | 1 pre-candidatos, 1 casam dia a dia
M C016 | emp3 | MAPEADO | col133 | 1 pre-candidatos, 1 casam dia a dia
M C017 | emp4 | MAPEADO | col32 | 1 pre-candidatos, 1 casam dia a dia
M C018 | emp4 | nao mapeado | - | 0 pre-candidatos, 0 casam dia a dia
M C019 | emp4 | nao mapeado | - | 0 pre-candidatos, 0 casam dia a dia
M C020 | emp4 | MAPEADO | col712 | 1 pre-candidatos, 1 casam dia a dia
ESTADO: mapearam 13 de 20
FIM_SONDA_FP
rc_sombra=0
SAIDA_END
```

O dado fornecido reporta **13/20 casamentos únicos**, nenhum ambíguo, `rc_sombra=0` e nenhuma ocorrência de `ERRO_COLETA`; sem validação independente. [nao verificado]  
C001, C008 e C012: cada um tem **0 candidatos finais**, após 1 pré-candidato; falharam na comparação diária, mas a divergência concreta não foi fornecida. [nao verificado]  
C005, C010, C018 e C019: cada um tem **0 pré-candidatos e 0 finais**; exclusão já no pré-filtro, cuja composição e causa concreta não estão demonstradas. [nao verificado]  
Dia: `str(data)` de um `date` coincide com `AAAA-MM-DD`; o PDF usa `dia['data']`, exibido como `%d/%m` + dia da semana. A implementação da comparação na sonda está [nao verificado]. [pdf_espelho.py:868](app/relatorios/pdf_espelho.py:868)  
Hora: o PDF usa `timezone.localtime(b.timestamp).strftime('%H:%M')`, somente para `orfa=False`; espúrias aparecem separadas como `*HH:MM`. [pdf_espelho.py:876](app/relatorios/pdf_espelho.py:876), [pdf_espelho.py:881](app/relatorios/pdf_espelho.py:881), [pdf_espelho.py:885](app/relatorios/pdf_espelho.py:885)  
Atribuir os sete insucessos a espúrias do auditor, multiconjunto de HH:MM ou erro de coleta, e confirmar que a sonda reproduz esses critérios: [nao verificado].

## 17/11 14:22 · sessao 64f2c72c2154 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Item 5 — PROCEDE:** a porta atual decide somente pertinência da obra à fila 1 viva, por ID e estado das células do bloco OBRAS; não lê classe tela, instrumento ou balde: `bin/disjuntor.py:1707`, `bin/disjuntor.py:1688`, `bin/hook_stop_fila1.py:231`, `bin/hook_stop_fila1.py:250`. O literal novo não resolve a obra simultaneamente fila 1 e tela [nao verificado — anexo aval.txt]. Essa interseção não é impossível na documentação: há item histórico escrito “a outra metade e a TELA, e ela e a FILA 1 AGORA”, já fechado: `app/docs/BACKLOG.md:1248`. Não afirmar construtor desse item hoje aberto [nao verificado].

- **Item 4 / CONTRATO — PROCEDE:** o aval remete a identificação de complemento ao contrato, mas suas cinco cláusulas não dão um campo, evento, ID ou critério para distinguir complemento de volta [nao verificado — anexo aval.txt]. A API atual recebe obra/texto/via, sem parâmetro de complemento ou volta: `bin/disjuntor.py:1698`; o coletor reúne todos os briefs próprios e citados: `bin/disjuntor.py:1607`. A regra vigente permite à sessão decidir tecnicamente quando obra entra como complemento: `CLAUDE.md:276`, `CLAUDE.md:278`; exigir brief próprio não revoga necessariamente esse poder.

- **CONDUTA — PROCEDE:** “qualquer construtor ou complemento”, exigindo itens 1–4, abrange também tela e instrumento, enquanto o alcance do item 5 diz deixá-los como estão [nao verificado — anexo aval.txt]. O código existente não dá um significado mais estreito a “qualquer”: `bin/disjuntor.py:1698`, `bin/disjuntor.py:1737`.

- **PROIBIDO — NAO PROCEDE, quanto a alegar conflito obrigatório:** endurecer o juiz existente e manter a porta existente é compatível com as funções únicas atuais: `bin/disjuntor.py:1522`, `bin/disjuntor.py:1698`; o contrato prévio único e pouso separado de instrumento já são exigidos: `CLAUDE.md:394`, `CLAUDE.md:327`, `app/docs/PISTA.md:170`. Não há autorização para uma allowlist implícita de obras já aprovadas [nao verificado — anexo aval.txt]. `CENSO APROVADO` atual prova brief aprovado, não vaga aberta: `bin/disjuntor.py:1703`.

- **Exportada/dinheiro/! — NAO PROCEDE como efeito direto declarado:** os pontos de mudança indicados são o parser do brief e a porta de abertura, que leem arquivos e decidem recusa: `bin/disjuntor.py:1525`, `bin/disjuntor.py:1713`, `bin/disjuntor.py:1728`. Isso não amplia autorização de negócio: `CLAUDE.md:265`, `CLAUDE.md:268`; a competência exportada segue exigindo `!`: `CLAUDE.md:411`. Implementação futura além desses pontos [nao verificado].

- **RESERVA — NAO PROCEDE quanto a colisão necessária:** leitura adversarial e mapa de escritores/leitores já pertencem ao despacho Codex: `CLAUDE.md:358`, `CLAUDE.md:359`; a lista vigente de classes inclui segundo juiz, segundo escritor, relógio, derivação paralela, selo sem mordida, ordem de trava e pouso sem pré-requisito: `CLAUDE.md:373`. Estado atual dos ataques F4/F7/F3/F9 e sua resposta no brief [nao verificado]. [nao verificado]

Regra vigente e documentação:

- Fila 1 é contrato 20 e placar L-099 pela conduta: `CLAUDE.md:291`; o código usa os itens andáveis do bloco OBRAS: `bin/disjuntor.py:1634`. UI fila 2 tem recorte próprio: `CLAUDE.md:312`; instrumento em `bin/` tem pouso próprio: `CLAUDE.md:327`.
- O registro existente do CENSO-ANTES-DA-VAGA descreve brief `logs/<obra>/brief*.md`, perguntas sem corte, recusa e PAREI: `app/docs/BACKLOG.md:802`. Defeitos já registrados do instrumento ficam na linha seguinte: `app/docs/BACKLOG.md:803`.
- CORTES.md declara ser gerado do JSON: `app/docs/CORTES.md:3`; corte ESPINHA-ANTES-DA-UI mantém prioridade estrutural e proíbe allowlist: `app/docs/CORTES.md:300`.
- Contrato precisa declarar quem chama, espera, falha/limite, medida e desligamento; procedente muda o contrato antes do código: `app/docs/PISTA.md:170`, `app/docs/PISTA.md:174`.

**BACKLOG: quem escreve / quem lê**

- **Escrevem:** sessão registra prompt em OBRAS no mesmo turno, autorizada pela rotina: `CLAUDE.md:285`, `CLAUDE.md:436`. O bloco OBRAS e o marcador ORDEM-VIVA são escritos à mão: `bin/gerar_backlog.py:103`, `bin/gerar_backlog.py:113`; `backlog_estado.py` recebe ID/texto e grava coluna de estado `[3]`: `bin/backlog_estado.py:29`, `bin/backlog_estado.py:58`. O gerador escreve o arquivo preservando esse bloco: `bin/gerar_backlog.py:119`, `bin/gerar_backlog.py:138`.
- **Leem para o portão:** `disjuntor._fila1_viva`, que usa o juiz do hook: `bin/disjuntor.py:1630`, `bin/disjuntor.py:1634`; `_itens_andaveis` lê ID, fechamento e parada: `bin/hook_stop_fila1.py:244`, `bin/hook_stop_fila1.py:250`.
- **Outros leitores comprovados:** fabricante: `bin/fabricante_alvo.py:238`; contador de interrompidas: `bin/interrompidas_estado.py:89`; publicação do relato: `bin/relato.sh:130`.

## 17/11 14:22 · sessao 35317b459d4b · - · - · origem=Ronald (tmux)

Achados do wrapper/contrato, somente por leitura:

- **Item 5 — PROCEDE:** ampliar apenas `_censo_nega` não cobre Codex lateral: `main_censo` calcula pertença à fila 1 e retorna 0 antes da porta quando está fora; só depois chama `_censo_nega` (`bin/disjuntor.py:2306`, `bin/disjuntor.py:2311`, `bin/disjuntor.py:2315`). O wrapper efetivamente chama esse CLI antes de prender a vaga, passando raiz do repositório vivo, obra, pid, hash, pergunta e anexos (`bin/codex_pergunta.sh:241`, `bin/codex_pergunta.sh:247`, `bin/codex_pergunta.sh:252`).

- **Item 4 — PROCEDE:** hoje o coletor é união de todos os briefs da pasta da obra e dos citados; não exige que exista brief próprio quando um citado já serve (`bin/disjuntor.py:1604`, `bin/disjuntor.py:1607`, `bin/disjuntor.py:1619`). O wrapper fornece somente obra/papel/WT/pergunta/anexos, sem campo de complemento ou volta de construção (`bin/codex_pergunta.sh:57`, `bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:247`). Não há nessa chamada informação que distinga complemento de nova chamada para obra aberta [nao verificado quanto a outros chamadores].

- **CONTRATO — PROCEDE na medida:** o ESTADO atual conta negativas totais, não por motivo: incrementa `negados` ao ler `NEGOU`, retornando um único número (`bin/disjuntor.py:2102`, `bin/disjuntor.py:2104`, `bin/disjuntor.py:2145`). Seu texto publica “aberturas negadas por censo nas 24 h”, também sem motivos (`bin/disjuntor.py:2164`). As negativas vêm de `logs/sessao/disjuntor.log`, não de `chamadas.log` (`bin/disjuntor.py:2090`). O censo antecede a gravação do `ini`; chamada recusada ali não alcança esse registro (`bin/codex_pergunta.sh:247`, `bin/codex_pergunta.sh:308`).

- **CONTRATO — NAO PROCEDE quanto a falta da porta no wrapper:** ela existe e qualquer rc diferente de zero recusa; raiz usada para BACKLOG/brief/RELATO é a raiz do repositório vivo, derivada do git-common-dir (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:247`). Brief ilegível retorna motivo de recusa (`bin/disjuntor.py:1524`, `bin/disjuntor.py:1528`); falha ao ler pergunta/anexo vira negativa no CLI (`bin/disjuntor.py:2295`, `bin/disjuntor.py:2308`).

- **CONTRATO — NAO PROCEDE quanto a “PAREI provado como hoje”:** a prova existente exige wrapper vivo desta raiz, mesmo uid, obra exata e papel `censo|leitura`; quando acaba, deixa de provar espera (`bin/hook_stop_fila1.py:734`, `bin/hook_stop_fila1.py:744`, `bin/hook_stop_fila1.py:763`, `bin/hook_stop_fila1.py:770`). O hook não lê o conteúdo do brief para essa prova; verifica processo e ambiente (`bin/hook_stop_fila1.py:747`, `bin/hook_stop_fila1.py:759`).

- **CONDUTA/RESERVA — PROCEDE quanto ao acesso do leitor:** os quatro papéis de leitura do Codex recebem cópia feita por `git archive` e executam em `$WS`, sob read-only (`bin/codex_pergunta.sh:312`, `bin/codex_pergunta.sh:315`, `bin/codex_pergunta.sh:425`). `logs/` é ignorado (`.gitignore:53`). Portanto esse leitor não recebe os logs vivos por esse mecanismo; recebe os anexos concatenados e raspados no prompt (`bin/codex_pergunta.sh:331`, `bin/codex_pergunta.sh:334`, `bin/codex_pergunta.sh:360`). Exigir que o Codex confirme a busca nos logs sem fornecer seu resultado deixa essa confirmação [nao verificado]. O portão em host tem acesso à raiz viva por argumento; não confundir com o leitor Codex (`bin/codex_pergunta.sh:247`).

- **Item 1/RED — prova de compatibilidade antiga:** a fixture atualmente chamada `CONFORME` contém apenas CENSO, leitores, escritores, decisões e perguntas de lei 0; não contém caminhos/ataques (`bin/tests/test_censo_antes_da_vaga.sh:54`). A forma atual aprova quando esses requisitos fecham (`bin/disjuntor.py:1565`, `bin/disjuntor.py:1577`). Logo o aperto deliberadamente torna insuficiente um brief conforme ao contrato antigo; ocorrência de brief de produção nessas condições [nao verificado].

Quem escreve/lê:

| Arquivo/campo | Escritor | Leitor |
|---|---|---|
| `logs/<OBRA>/brief*.md` e novos campos caminhos/ponto/ataques | Autor concreto não demonstrado na cópia [nao verificado]; o aval anexo atribui conferência à sessão [nao verificado como arquivo:linha] | Coletor `_briefs_do_censo`, depois `censo_do_brief` (`bin/disjuntor.py:1607`, `bin/disjuntor.py:1718`) |
| `BACKLOG` | Registrar BACKLOG é ação pré-aprovada da sessão/Code (`CLAUDE.md:282`, `CLAUDE.md:285`); prompt que pede obra deve virar item no bloco OBRAS (`CLAUDE.md:436`) | `main_censo` consulta fila 1 antes da porta (`bin/disjuntor.py:2306`); `_censo_nega` recebe as mesmas células (`bin/disjuntor.py:2315`) |
| `chamadas.log`: hora, obra, papel, hash12, pid, `ini`, pergunta raspada | Wrapper, antes da chamada (`bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:308`) | Parser `_linhas_chamadas`, que extrai t/obra/papel/chave, ignorando texto da pergunta (`bin/codex_limite_estado.py:49`, `bin/codex_limite_estado.py:63`) |
| `chamadas.log`: `fim`, thread_id, rc, duração | Trap `_saida` do wrapper (`bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:272`) | Mesmo parser; últimas chamadas/rc e procedência por thread (`bin/codex_limite_estado.py:66`, `bin/codex_limite_estado.py:82`, `bin/codex_limite_estado.py:92`); `gerar_codex_estado` usa procedência (`bin/gerar_codex_estado.py:176`) |
| Casamento `ini` com censo aprovado | Wrapper escreve ini; trilha de aprovação vem da porta (`bin/codex_pergunta.sh:308`, `bin/disjuntor.py:1722`) | `_censo_24h`, mesma obra+pid+hash em janela de 600 s (`bin/disjuntor.py:2052`, `bin/disjuntor.py:2134`) |

A mudança de instrumento em `bin/` não pode ser entregue pelo papel construtor deste wrapper: ele proíbe `bin/` e tudo fora de app (`bin/codex_pergunta.sh:128`, `bin/codex_pergunta.sh:134`); isso é compatível com instrumento/pouso próprio (`CLAUDE.md:327`). Nenhum apply de dinheiro ou exportada decorre desses caminhos do portão; operações de negócio e estado de produção citados no anexo continuam [nao verificado].

## 17/11 14:22 · sessao 12b949535d78 · E4C-F2 · verificador · origem=Code

1. **O INSERT antigo grava `False` nas duas colunas.** Os defaults estão no banco nas migrations [0054:19](./app/chamados/migrations/0054_decisao_da_ressuscitacao.py#L19) e [0055:26](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L26).

   **Nenhuma das duas constraints citadas rejeita esse INSERT, para qualquer `ato`.** Ambas têm a forma `ato=... OR campo=False`; com `False`, a segunda condição sempre satisfaz o check ([0054:24](./app/chamados/migrations/0054_decisao_da_ressuscitacao.py#L24); [0055:34](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L34)).

2. **O backfill continua correto para as linhas alcançadas pelo UPDATE.** Ele atribui explicitamente `True` às RESPOSTAS com motivo; `db_default=False` não impede essa atualização ([0055:12](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L12)).

   A ordem apresentada é coerente: adiciona a coluna, atualiza, verifica constraints diferidas antes do próximo `ALTER TABLE` e adiciona o check. O código já emprega esse mecanismo na 0051 ([0055:22–35](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L22); [0051:101–104](./app/chamados/migrations/0051_decisao_pergunta.py#L101)). Execução efetiva dessa sequência: **[nao verificado]**.

   **A ressalva é temporal:** o backfill roda uma vez; INSERTs antigos posteriores recebem `False`, mesmo quando têm `ato='resposta'` e motivo preenchido — combinação que o próprio backfill classificaria como `True` ([0055:12](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L12), [0055:26–28](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L26)).

3. **Os dois campos e respectivos checks estão consistentes por inspeção.** Modelo e migrations declaram igualmente `default=False, db_default=False`, com as mesmas condições e nomes de constraints ([models.py:4178](./app/chamados/models.py#L4178), [4185](./app/chamados/models.py#L4185), [4203–4207](./app/chamados/models.py#L4203); [0054:19–25](./app/chamados/migrations/0054_decisao_da_ressuscitacao.py#L19); [0055:26–35](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L26)).

   `makemigrations` executado e sem diferenças, inclusive fora desses campos: **[nao verificado]**.

4. **O teste morde o default do banco, por construção.** O SQL omite as duas colunas, usa `RETURNING` e exige `(False, False)`; não passa pelo default Python. O segundo teste exige os dois defaults no catálogo ([teste:25–32](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L25); [teste:34–39](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L34)). Num banco reconstruído sem os `db_default` das migrations, o primeiro INSERT deve falhar por `NOT NULL`; remover apenas os defaults do modelo, mantendo o banco já migrado, não testa essa regressão ([0054:19](./app/chamados/migrations/0054_decisao_da_ressuscitacao.py#L19); [0055:26](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L26); [teste:25](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L25)).

   **Não deixa a linha órfã para o teardown:** `_Desfaz` sai do `atomic`, desfazendo o INSERT antes da asserção; somente essa exceção é capturada ([teste:21–32](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L21)). Isso pressupõe a FK diferida declarada no comentário; configuração real da FK e execução de `check_constraints`: **[nao verificado]** ([teste:24](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L24)). A reversão de eventual avanço da sequência de IDs também não é verificada pelo teste ([teste:25](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L25)).

5. **Escritores e leitores identificados:**

   - `da_ressuscitacao`: a ressuscitação escreve `True`; o predicado exige REABERTURA e marca verdadeira; a carga da tarefa usa esse predicado para excluir a linha. `False` conserva o tratamento comum ([models.py:1695](./app/chamados/models.py#L1695), [4214](./app/chamados/models.py#L4214); [tarefa_do_dia.py:426](./app/chamados/services/tarefa_do_dia.py#L426)).
   - `resposta_mantem_validacao`: resposta humana escreve `False`; automática, carimbo pela trilha e foto da projeção escrevem `True`; o backfill escreve `True` no histórico selecionado ([disputa_emissao.py:1534](./app/chamados/services/disputa_emissao.py#L1534), [1688](./app/chamados/services/disputa_emissao.py#L1688), [1710](./app/chamados/services/disputa_emissao.py#L1710); [models.py:4253](./app/chamados/models.py#L4253); [0055:12](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L12)).
   - A porta genérica instancia os dicionários e insere em lote; os leitores Python e SQL interpretam RESPOSTA com `False` como ato que desfaz validação ([models.py:4328](./app/chamados/models.py#L4328), [4281](./app/chamados/models.py#L4281), [3547](./app/chamados/models.py#L3547), [4311–4312](./app/chamados/models.py#L4311)).

   **Há, portanto, um contraexemplo semântico para a janela:** validação em T0; RESPOSTA automática ou pela trilha em T1>T0, inserida pelo código antigo após o backfill, omitindo a coluna. Ela recebe `False`, e a leitura nova desfaz a validação. Porém esses caminhos preservam `validada_em` e exigem `True` na cópia atual ([disputa_emissao.py:1685–1688](./app/chamados/services/disputa_emissao.py#L1685), [1707–1710](./app/chamados/services/disputa_emissao.py#L1707); [models.py:4281](./app/chamados/models.py#L4281), [4311–4312](./app/chamados/models.py#L4311)). Ocorrência desse caso em produção e corpos exatos desses escritores antigos: **[nao verificado]**.

**REPROVADO** para o pouso irrestrito descrito: o delta resolve a aceitação dos INSERTs, mas o backfill único não preserva a semântica das RESPOSTAS antigas posteriores que deveriam manter validação ([0055:12,28](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L12); [models.py:4281](./app/chamados/models.py#L4281)).

## 17/11 14:22 · sessao 3e7b5d496f7c · - · - · origem=Ronald (tmux)

Achados da leitura estática, sem executar os selos que escrevem arquivos (`bin/tests/test_censo_antes_da_vaga.sh:78`).

- **Premissa do juiz/porta — NAO PROCEDE como objeção.** Existem exatamente os sítios citados: juiz `censo_do_brief` em `bin/disjuntor.py:1522`, porta `_censo_nega` em `bin/disjuntor.py:1698`. Agent chama a porta em `bin/disjuntor.py:1855`; a recuperação de exceção também chama em `bin/disjuntor.py:1763`; CLI do wrapper chama em `bin/disjuntor.py:2315`.

- **Reconhecimento atual do bloco.** Primeiro título que case `^(#{1,6})\s*CENSO\b`, com caixa literal; termina no próximo título de nível igual ou superior (`bin/disjuntor.py:1496`, `bin/disjuntor.py:1530`, `bin/disjuntor.py:1538`). Portanto títulos de nível 1–6 e sufixos depois de `CENSO` são reconhecidos; só o primeiro bloco é julgado (`bin/disjuntor.py:1534`). Prosa fora desse recorte não entra no juízo (`bin/disjuntor.py:1538`).

- **Campos atuais.** Lê leitores, escritores, decisões e perguntas de lei; exige alguma referência sintática em leitores/escritores e alguma decisão não vazia (`bin/disjuntor.py:1498`, `bin/disjuntor.py:1565`). A regex de referência só reconhece aparência `arquivo.ext:NN`; não abre o código citado nem verifica que a linha exista (`bin/disjuntor.py:1501`, `bin/disjuntor.py:1566`). Exige quantidade exata de perguntas e cada uma terminando em `? cortada por Ronald <aval>` (`bin/disjuntor.py:1572`, `bin/disjuntor.py:1574`). Não existe interpretação de `caminhos`, `ponto unico`, `lista fechada`, `ataques cruzados` ou do resultado de buscas na função atual (`bin/disjuntor.py:1543`, `bin/disjuntor.py:1577`).

- **Item 1 — PROCEDE a lacuna literal.** O aval pede N linhas e “TODO caminho”, mas não define casos como N=0, duplicatas, arquivos inexistentes e como uma lista fechada de sítios se relaciona às linhas de caminhos; o juiz atual somente faz validação sintática de referências, não confere alcance de chamadas (`bin/disjuntor.py:1501`, `bin/disjuntor.py:1566`). A completude real dos caminhos descritos no anexo é `[nao verificado]`.

- **Item 2 — NAO PROCEDE a objeção de atingir prosa externa**, se preservado o recorte atual: só conteúdo entre o primeiro título CENSO e o próximo título de nível igual/maior fica no bloco (`bin/disjuntor.py:1530`, `bin/disjuntor.py:1538`). **PROCEDE** observar que “citando a linha” não é um recurso existente: o laço de validação perde os índices ao construir `bloco`; mensagens atuais não retornam o número físico da linha (`bin/disjuntor.py:1537`, `bin/disjuntor.py:1544`).

- **Item 3 — PROCEDE a diferença entre declarar busca e comprovar busca.** O juiz lê apenas o texto do brief (`bin/disjuntor.py:1524`); não executa grep nem pesquisa outros logs; o juízo atual termina em `bin/disjuntor.py:1577`. Uma linha contendo o comando usado pode ser conferida quanto à forma; a realização e cobertura dessa busca não ficam comprovadas pelo conteúdo dessa linha `[nao verificado]`.

- **Item 4 — PROCEDE a insuficiência do reconhecimento e a colisão com brief antigo.** A porta recebe `obra,texto,via,raiz` e não recebe indicador de complemento, volta ou construtor já aberto (`bin/disjuntor.py:1698`). Coleta TODOS os `logs/<OBRA>/brief*.md` unidos a TODOS os briefs citados, inclusive brief de outra pasta sob logs (`bin/disjuntor.py:1607`, `bin/disjuntor.py:1619`). Se qualquer um falhar, a abertura é negada (`bin/disjuntor.py:1717`, `bin/disjuntor.py:1728`). Assim, adicionar um brief novo conforme não salva o brief antigo da obra-mãe citado na abertura: ele também seria julgado pela nova forma (`bin/disjuntor.py:1611`, `bin/disjuntor.py:1718`). Não há memória de aprovação anterior nem exceção para volta de construtor já aberto nessa decisão (`bin/disjuntor.py:1706`, `bin/disjuntor.py:1727`). Quais obras reais seriam atingidas está `[nao verificado]`.

- **Fila 1 hoje.** `_fila1_viva` lê `app/docs/BACKLOG.md` e pede ao hook os itens andáveis dentro de `<!-- OBRAS:INICIO -->` (`bin/disjuntor.py:1630`, `bin/disjuntor.py:1634`). Andável é o contrário de fechado/parado; a coluna de estado participa dessa decisão (`bin/hook_stop_fila1.py:171`, `bin/hook_stop_fila1.py:180`, `bin/hook_stop_fila1.py:244`). A pertença de uma obra é decidida por token exato de ID, com ambiguidades negadas (`bin/disjuntor.py:1688`, `bin/disjuntor.py:1694`); não é o marcador da obra atualmente em curso, que tem juiz diferente (`bin/hook_stop_fila1.py:192`).

- **Item 5 — PROCEDE a falta de classificação/precedência.** A porta só sabe fila 1 e libera imediatamente fora dela (`bin/disjuntor.py:1707`). O CLI também libera fora da fila 1 ANTES de chamar a porta (`bin/disjuntor.py:2306`, `bin/disjuntor.py:2311`). Portanto ampliar apenas `_censo_nega` deixa o wrapper fora do novo alcance. Nem tela, instrumento nem balde LATERAL-ESTRUTURA entram na decisão atual (`bin/disjuntor.py:1698`, `bin/disjuntor.py:1707`). Obra que seja fila 1 e tela já cai no censo hoje: não há isenção de tela (`bin/disjuntor.py:1707`). O literal novo precisa ter precedência inequívoca para essa interseção; a escolha pretendida é `[nao verificado]`.

- **RED — PROCEDE a insuficiência de R4/R6 sem essas distinções.** O selo atual tem fixture “CONFORME” que não contém nenhum dos novos campos e espera rc=0 (`bin/tests/test_censo_antes_da_vaga.sh:54`, `bin/tests/test_censo_antes_da_vaga.sh:86`); ele terá de mudar para representar o novo contrato. Não há prova executada de R1–R6 nesta revisão `[nao verificado]`. Para R4 falta o sinal de complemento na entrada atual (`bin/disjuntor.py:1698`); para R6 falta classificação de tela (`bin/disjuntor.py:1707`).

- **CONTRATO — parcialmente confere.** Agent construtor é tipo fora de `SO_LEITURA` cujo prompt contém caminho `/.../wt-*`; `_censo_nega` é chamado só nessa condição (`bin/disjuntor.py:151`, `bin/disjuntor.py:153`, `bin/disjuntor.py:1851`). Exceções e brief ilegível negam no ramo do censo (`bin/disjuntor.py:1527`, `bin/disjuntor.py:1733`). O Stop prova a espera por processo vivo do wrapper da mesma raiz, obra exata e papel censo/leitura; não julga os novos campos do brief (`bin/hook_stop_fila1.py:730`, `bin/hook_stop_fila1.py:763`). **PROCEDE a diferença na medida:** ESTADO hoje conta recusas agregadas, não por motivo (`bin/disjuntor.py:2102`, `bin/disjuntor.py:2145`, `bin/disjuntor.py:2164`).

- **Aprovação, PAREI e abertura.** `CENSO APROVADO` não registra autorização duradoura: o Agent o escreve só se o hook inteiro passar (`bin/disjuntor.py:1805`, `bin/disjuntor.py:1808`). O CLI escreve aprovação do brief; depois a medida casa essa aprovação ao `ini` de chamadas.log por obra+pid+hash e janela de 600 s (`bin/disjuntor.py:2052`, `bin/disjuntor.py:2134`). O PAREI é escrito pelo mecanismo de RELATO e lido de volta pelo leitor do Stop (`bin/disjuntor.py:312`, `bin/disjuntor.py:324`).

- **PROIBIDO — NAO PROCEDE afirmar necessidade de segundo juiz ou allowlist.** Os pontos existentes aceitam extensão do mesmo juiz e porta; não há allowlist no caminho atual de aprovação, que percorre todos os briefs coletados (`bin/disjuntor.py:1713`, `bin/disjuntor.py:1717`). Preservar a cabeça `NEG_CENSO` importa: ela alimenta identificação de obra no PAREI e na medida (`bin/disjuntor.py:1512`, `bin/disjuntor.py:1516`, `bin/disjuntor.py:309`, `bin/disjuntor.py:2103`).

- **Exportada/dinheiro/! — NAO PROCEDE afirmar toque direto pelo juiz/porta descritos.** O juiz abre brief e valida texto (`bin/disjuntor.py:1524`, `bin/disjuntor.py:1577`); a porta decide abertura e registra trilha (`bin/disjuntor.py:1706`, `bin/disjuntor.py:1724`). Impacto indireto sobre cronograma e obras que mexem nesses domínios é `[nao verificado]`.

## 17/11 14:21 · sessao 70d03b6c7fae · - · - · origem=Ronald (tmux)

Item 5: há risco semântico na janela autorizada.

- `da_ressuscitacao`: escritor de `True` é `ChamadoColaborador.ressuscitar_perguntas` (`app/chamados/models.py:1695`); omissões recebem `False` (`app/chamados/models.py:4178`). O predicado exige REABERTURA e marca verdadeira (`app/chamados/models.py:4214`); `CargaDaTarefa` exclui apenas essas linhas (`app/chamados/services/tarefa_do_dia.py:426`). `False` significa reabertura comum, coerente com a documentação do campo (`app/chamados/models.py:4171`).

- `resposta_mantem_validacao`: quatro escritores explícitos atuais: humano `False` (`app/chamados/services/disputa_emissao.py:1534`); automática `True` (`app/chamados/services/disputa_emissao.py:1688`); carimbo pela trilha `True` (`app/chamados/services/disputa_emissao.py:1710`); foto da projeção `True` (`app/chamados/models.py:4253`). Histórico é atualizado pelo `RunPython` (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`). A porta genérica instancia os dicts e faz `bulk_create`, aplicando defaults às omissões (`app/chamados/models.py:4328`).

- Leitores diretos: SQL de validação considera RESPOSTA com `False` um ato que limpa validação (`app/chamados/models.py:3547`); predicado Python faz o mesmo (`app/chamados/models.py:4281`), chamado pela leitura (`app/chamados/models.py:4299`), que zera `validada` (`app/chamados/models.py:4311`). Leitores indiretos incluem estado (`app/chamados/models.py:3231`), `respondida` (`app/chamados/models.py:3782`), texto exibido (`app/chamados/models.py:3806`) e `q_vivas` (`app/chamados/models.py:3609`).

**Contraexemplo:** depois do backfill, antes HUP, uma RESPOSTA automática ou pela trilha do código antigo omite a coluna e recebe `False`; a nova leitura passa a tratá-la como resposta que desfaz validação (`app/chamados/models.py:4185`, `4281`, `4311`). Contudo esses caminhos preservam `validada_em`: seus saves só incluem texto/instante (`app/chamados/services/disputa_emissao.py:1685`, `1707`) e o código atual exige `True` (`app/chamados/services/disputa_emissao.py:1688`, `1710`). O `RunPython` executado uma vez não alcança inserts posteriores (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `28`). Ocorrência em produção e código antigo exato: [nao verificado].

O teste nominal documenta que o default nunca deveria decidir silenciosamente o efeito de uma resposta (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:3`), e distingue os quatro escritores e seus valores (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:18`). Logo o delta resolve compatibilidade estrutural, mas não demonstra compatibilidade semântica universal da janela (`app/chamados/models.py:4281`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:28`).

Recomendação: **REPROVADO** para pouso com escritores antigos irrestritos após o backfill, pelo contraexemplo acima (`app/chamados/services/disputa_emissao.py:1685`, `1688`; `app/chamados/models.py:4281`).

## 17/11 14:21 · sessao 2e3a0a2f08fd · CONFRONTO-PILOTO · sonda · origem=Code

Transcrição literal da saída fornecida; não representa nova execução. [nao verificado]

```text
SAIDA_BEGIN
sonda_leitura: sonda_47.py [28c98d9f6041901c] no cpuset 4-7, lendo sombra como leitor (LEITURA)

SITUACOES emp2-4 {'ativo': 545, 'desligado': 383, 'afastado': 11}
ATIVOS situacao=ativo emp2-4: 545 | zero batida apuravel 21/10..16/11: 47 por empresa {2: 37, 3: 8, 4: 2}
admissao antes/durante: {'antes': 41, 'durante': 6}
estrato em 16/11: {'5x2': 6, 'personalizado': 1, '12x36_diurno': 16, '12x36_noturno': 7, '6x1': 11, 'sem_vinculo': 4, 'intermitente': 2}
no TXT (status entra + fechamento em STATUS_NO_TXT): 26 | fora: 21
status classificar_export: {'entra': 26, 'fora': 21}
status x motivo (fora do TXT): {('fora', 'furo_espelho', 'aberto'): 20, ('fora', 'sem_codigo_dominio', 'aberto'): 1}
ainda zero ate o fim da janela (20/11): 47 | bateram entre 17/11 e 20/11: 0
-- por pessoa: colNNN | emp | admissao | estrato | status | motivo | status_fech | no_txt | batidas_ate_fim
P col181 | emp2 | antes | 5x2 | entra |  | aberto | TXT | 0
P col186 | emp2 | antes | personalizado | entra |  | aberto | TXT | 0
P col209 | emp2 | antes | 12x36_diurno | fora | furo_espelho | aberto | fora | 0
P col216 | emp2 | antes | 5x2 | entra |  | aberto | TXT | 0
P col225 | emp2 | antes | 12x36_diurno | entra |  | aberto | TXT | 0
P col234 | emp2 | antes | 12x36_diurno | entra |  | aberto | TXT | 0
P col236 | emp2 | antes | 5x2 | fora | furo_espelho | aberto | fora | 0
P col257 | emp2 | antes | 5x2 | entra |  | aberto | TXT | 0
P col268 | emp2 | antes | 12x36_diurno | fora | furo_espelho | aberto | fora | 0
P col274 | emp2 | antes | 12x36_diurno | entra |  | aberto | TXT | 0
P col291 | emp2 | antes | 12x36_noturno | entra |  | aberto | TXT | 0
P col298 | emp2 | antes | 6x1 | fora | furo_espelho | aberto | fora | 0
P col303 | emp2 | antes | 12x36_diurno | entra |  | aberto | TXT | 0
P col312 | emp2 | antes | 12x36_diurno | fora | furo_espelho | aberto | fora | 0
P col317 | emp2 | antes | 6x1 | fora | furo_espelho | aberto | fora | 0
P col391 | emp2 | antes | sem_vinculo | entra |  | aberto | TXT | 0
P col440 | emp2 | antes | 6x1 | entra |  | aberto | TXT | 0
P col499 | emp2 | antes | 12x36_noturno | entra |  | aberto | TXT | 0
P col534 | emp2 | antes | 6x1 | fora | furo_espelho | aberto | fora | 0
P col607 | emp2 | antes | 6x1 | fora | furo_espelho | aberto | fora | 0
P col644 | emp2 | antes | sem_vinculo | entra |  | aberto | TXT | 0
P col647 | emp2 | antes | 12x36_diurno | entra |  | aberto | TXT | 0
P col650 | emp2 | antes | 12x36_diurno | fora | furo_espelho | aberto | fora | 0
P col667 | emp2 | antes | 6x1 | fora | furo_espelho | aberto | fora | 0
P col704 | emp2 | antes | 12x36_diurno | entra |  | aberto | TXT | 0
P col717 | emp2 | antes | 12x36_diurno | fora | furo_espelho | aberto | fora | 0
P col832 | emp2 | antes | 12x36_diurno | fora | furo_espelho | aberto | fora | 0
P col915 | emp2 | antes | 6x1 | fora | furo_espelho | aberto | fora | 0
P col921 | emp2 | antes | 6x1 | fora | furo_espelho | aberto | fora | 0
P col938 | emp2 | antes | 12x36_noturno | entra |  | aberto | TXT | 0
P col966 | emp2 | antes | 12x36_diurno | entra |  | aberto | TXT | 0
P col1009 | emp2 | durante | 6x1 | fora | furo_espelho | aberto | fora | 0
P col1016 | emp2 | durante | 12x36_noturno | fora | furo_espelho | aberto | fora | 0
P col1025 | emp2 | durante | 5x2 | entra |  | aberto | TXT | 0
P col1027 | emp2 | durante | 12x36_diurno | fora | furo_espelho | aberto | fora | 0
P col1033 | emp2 | durante | 6x1 | fora | furo_espelho | aberto | fora | 0
P col1043 | emp2 | durante | 12x36_diurno | fora | furo_espelho | aberto | fora | 0
P col66 | emp3 | antes | sem_vinculo | entra |  | aberto | TXT | 0
P col76 | emp3 | antes | intermitente | entra |  | aberto | TXT | 0
P col128 | emp3 | antes | 12x36_noturno | entra |  | aberto | TXT | 0
P col144 | emp3 | antes | 12x36_diurno | fora | furo_espelho | aberto | fora | 0
P col168 | emp3 | antes | 12x36_noturno | entra |  | aberto | TXT | 0
P col642 | emp3 | antes | 5x2 | entra |  | aberto | TXT | 0
P col751 | emp3 | antes | intermitente | entra |  | aberto | TXT | 0
P col980 | emp3 | antes | 12x36_noturno | entra |  | aberto | TXT | 0
P col41 | emp4 | antes | 6x1 | entra |  | aberto | TXT | 0
P col758 | emp4 | antes | sem_vinculo | fora | sem_codigo_dominio | aberto | fora | 0
FIM_SONDA_47
rc_sombra=0
SAIDA_END
```

1. Pelo dado fornecido, sim: `situacao=ativo`, empresas 2–4, totaliza 545. [nao verificado]
2. Zero batida apurável em 21/10–16/11: 47; empresas 2/3/4 = 37/8/2; admissões antes/durante = 41/6: esses recortes conferem com o auditor. [nao verificado]
3. Estratos não conferem: diurna 16 versus 17; noturna 7 versus 8; sem vínculo 4 versus 2; demais conferem: 6x1=11, 5x2=6, intermitente=2, personalizado=1. [nao verificado]
4. Pelo critério da sonda, 26 entram e 21 ficam fora, sendo 20 `furo_espelho` e 1 `sem_codigo_dominio`; presença em TXT efetivamente emitido não foi demonstrada. [nao verificado]
5. `entra` significa liberado pela classificação, com motivo vazio; `fora` significa excluído pelo motivo indicado (`app/folha/export.py:841`, `app/folha/export.py:970`); `aberto` é status do fechamento e é aceito no TXT, assim como `aprovado` (`app/folha/export.py:687`, `app/folha/export.py:716`).
6. `furo_espelho` significa pendência retornada pelo juiz de retenção das células, consultado por `motivos_retencao_espelho` (`app/folha/export.py:968`, `app/folha/export.py:1301`); o subtipo e os dias desses 20 casos não constam do dado. [nao verificado]
7. `sem_codigo_dominio` significa código do colaborador ausente ou vazio após remover espaços, causando exclusão antes da análise do espelho (`app/folha/export.py:840`, `app/folha/export.py:872`).
8. Não houve nova execução: não localizei `sonda_47.py` na cópia [nao verificado]; o invólucro escreve credencial, temporários e trilha (`bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:248`); `rc_sombra=0` pertence somente ao dado fornecido. [nao verificado]

## 17/11 14:20 · sessao b5b1bedae917 · - · - · origem=Ronald (tmux)

1. As duas colunas recebem `False`: ambas têm `db_default=False` nas migrations (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:19`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:26`). Nenhuma das duas constraints citadas pode rejeitar esses valores, independentemente de `ato`: cada expressão é `ato=... OR campo=False` (`0054_decisao_da_ressuscitacao.py:24`; `0055_decisao_resposta_mantem_validacao.py:34`). Outros motivos de rejeição, como constraint antiga da absorção, continuam possíveis (`app/chamados/migrations/0051_decisao_pergunta.py:79`).

2. O `UPDATE` explícito para `True` continua correto; default não o substitui, e o filtro limita a `RESPOSTA` com motivo (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`). A sequência adiciona campo, atualiza, força constraints e adiciona check; o mesmo mecanismo para eventos pendentes aparece na 0051 (`0055_decisao_resposta_mantem_validacao.py:22`; `:28`; `:30`; `:31`; `0051_decisao_pergunta.py:101`; `:104`).

   **Risco concreto da janela:** o backfill é executado uma vez (`0055...:28`). Um `INSERT` antigo posterior com `ato='resposta'` e motivo preenchido recebe `False`; a regra histórica o classificaria como `True` (`0055...:12`), enquanto os leitores novos interpretam `False` como “desfaz validação” (`app/chamados/models.py:3547`; `:4281`; `:4311`). O default resolve aceitação do INSERT, mas não preserva automaticamente essa semântica.

   Contraexemplo: validação em t0, resposta automática antiga em t1>t0 após migrate e antes HUP, omitindo campos. A linha de resposta impede criar outra foto correspondente (`app/chamados/models.py:4249`), e a leitura nova limpa a validação (`:4299`; `:4311`). Na cópia, respostas automática e pela trilha preservam `validada_em` e explicitam `True` (`app/chamados/services/disputa_emissao.py:1682`; `:1685`; `:1688`; `:1705`; `:1707`; `:1710`). Ocorrência real desse caso e corpos do commit anterior: [nao verificado].

3. Os dois campos e respectivas constraints coincidem entre modelo e migrations (`app/chamados/models.py:4178`; `:4185`; `:4203`; `:4206`; `0054...:19`; `:24`; `0055...:26`; `:34`). `makemigrations` efetivamente executado sem diferenças: [nao verificado]. A cópia fixa Django 5.2.14 (`app/requirements.txt:13`).

O delta é estruturalmente consistente, mas a condição de pouso descrita não garante a semântica de `resposta_mantem_validacao` para linhas antigas pós-backfill (`0055...:12`; `app/chamados/models.py:4281`). Minha conclusão para o pouso amplo: **REPROVADO**.

## 17/11 14:18 · sessao c3cb2be916ab · E4C-F1-B2 · leitura · origem=Code

**Resultado (b): não há ponto único existente que anteceda a primeira escrita de todos os caminhos.** A cerca atual fica em `retratar_com_motivo`, mas há entradas diretas no resolvedor, carimbos independentes, costuras anteriores ao resolvedor e transições que escrevem antes do S143 (`app/chamados/reconciliador.py:576`, `app/chamados/services/becos.py:55`, `app/chamados/reconciliador.py:652`, `app/chamados/reconciliador.py:1700`, `app/chamados/models.py:296`, `app/chamados/signals.py:124`).

A conclusão é sobre alcance estático da cópia. Ocorrência desses caminhos sobre perguntas efetivamente ENTREGUES em produção, quantidade de casos e estado atual do banco: **[nao verificado]**.

**Convenção das tabelas.** Os conjuntos abaixo nomeiam as escritas para evitar repetir os mesmos campos em todas as entradas.

| Conjunto | O que grava | Prova |
|---|---|---|
| **T — transição** | `ChamadoColaborador.status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`; conforme a transição, `respondido_por`, `urgencia`, `urgencia_original`. Antes do save, registra `LogAuditoria`. | `app/chamados/models.py:228`, `app/chamados/models.py:229`, `app/chamados/models.py:230`, `app/chamados/models.py:246`, `app/chamados/models.py:292`, `app/chamados/models.py:296`; `app/core/observ.py:134`. |
| **C — carimbo** | `PerguntaDisputa.materializada_em`, `materializada_por`, `via_resolucao`; com resposta textual e opção correspondente, também `respondida_em`, `validada_em`, `validada_por`. | `app/chamados/models.py:3220`, `app/chamados/models.py:3249`, `app/chamados/models.py:3251`, `app/chamados/models.py:3255`, `app/chamados/models.py:3273`. |
| **L — lavra S2** | `PerguntaDisputa.veredito_lavrado`, `julgado_em`, `janela_expira_em`. | `app/chamados/veredito_lavrado.py:270`, `app/chamados/veredito_lavrado.py:272`, `app/chamados/veredito_lavrado.py:273`, `app/chamados/veredito_lavrado.py:275`. |
| **A — costura** | `PerguntaDisputa.chamado_destino`, eventualmente `data_evento`, mais **L**; na colisão ao datar, pode retratar outras perguntas antes desse save. | `app/chamados/services/fio.py:927`, `app/chamados/services/fio.py:968`, `app/chamados/services/fio.py:973`, `app/chamados/services/fio.py:975`. |
| **D — disputa** | `DisputaSupervisao.fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; dispara Defesa 1 e S144. | `app/chamados/models.py:2023`, `app/chamados/models.py:2027`; `app/chamados/signals.py:26`, `app/chamados/signals.py:156`. |
| **Z — ressuscitação** | Registra decisões anteriores; limpa `PerguntaDisputa.materializada_em`, `materializada_por`, `via_resolucao`, `validada_em`, `validada_por`, `respondida_em`, `resposta_colab`; pode reabrir a disputa. | `app/chamados/models.py:1621`, `app/chamados/models.py:1639`, `app/chamados/models.py:1649`, `app/chamados/models.py:3402`. |
| **U — unidade de retratação** | **T**, efeitos do resolvedor descritos adiante, eventual `Justificativa.status/descricao`, ocorrência no fio. | `app/chamados/reconciliador.py:580`, `app/chamados/reconciliador.py:589`, `app/chamados/reconciliador.py:592`, `app/chamados/reconciliador.py:593`, `app/chamados/reconciliador.py:595`. |

“Passa pela cerca” abaixo significa que **o caminho obrigatoriamente a consulta antes da primeira escrita funcional desse ato**. A recusa pode escrever sua própria trilha; portanto, recusar não significa literalmente zero INSERTs (`app/chamados/reconciliador.py:578`, `app/chamados/reconciliador.py:491`).

**Entradas da retratação e desvios encontrados.**

| Entrada → caminho | Quem chama | Cerca/veto antes da primeira escrita? | O que grava → prova |
|---|---|---|---|
| `app/ponto/management/commands/reconciliar_fantasmas.py:281` → `retratar_com_motivo` | Comando aplicador. | **Sim**, `app/chamados/reconciliador.py:576`, antes de `app/chamados/reconciliador.py:580`. | **U**, `app/chamados/reconciliador.py:580`, `app/chamados/reconciliador.py:592`, `app/chamados/reconciliador.py:593`. |
| **Obrigatório:** `app/ponto/management/commands/reconciliar_fantasmas.py:288` → resolvedor avulso | Fallback do comando quando a retratação retorna falsa. | **Não em todos os ramos.** A recusa de competência impede o fallback; a retenção anterior à cerca pode retornar antes da consulta e ainda alcançar o fallback (`app/chamados/reconciliador.py:564`, `app/chamados/reconciliador.py:567`; `app/ponto/management/commands/reconciliar_fantasmas.py:284`, `app/ponto/management/commands/reconciliar_fantasmas.py:288`). | Ramos do resolvedor: **C/L/T/Z/D**, conforme a submatriz abaixo; entrada em `app/chamados/services/disputa_emissao.py:2028`. |
| **Obrigatório:** `app/chamados/services/becos.py:55` → resolvedor de cada credor | Signal de pergunta → `on_commit` → `fechar_beco_da_disputa`; também comando aplicador (`app/chamados/signals.py:246`, `app/chamados/signals.py:247`; `app/chamados/services/becos.py:79`; `app/chamados/management/commands/fechar_becos_disputa.py:57`). | **Não.** Trava e chama, sem veto (`app/chamados/services/becos.py:52`, `app/chamados/services/becos.py:55`). | Ramos do resolvedor; depois pode gravar **D** diretamente, `app/chamados/services/becos.py:58`. |
| `app/chamados/reconciliador.py:593` → resolvedor após a transição | Unidade `retratar_com_motivo`. | **Sim**, nesse caminho, `app/chamados/reconciliador.py:576`. | **U**; a transição já ocorreu em `app/chamados/reconciliador.py:580`. |
| **Obrigatório:** `app/chamados/reconciliador.py:1700` → costura HIST → `app/chamados/reconciliador.py:1706` → resolvedor | `reatar_pergunta_orfa`, chamado pela reconciliação e pela emissão (`app/chamados/reconciliador.py:1584`; `app/chamados/services/disputa_emissao.py:473`). | **Não.** Competência entregue conserva o caminho histórico; a guarda da costura libera entregue (`app/chamados/reconciliador.py:1650`, `app/chamados/services/fio.py:824`). | Primeiro **A**, em `app/chamados/services/fio.py:975`; somente depois chama o resolvedor em `app/chamados/reconciliador.py:1706`. |
| **Obrigatório:** `app/chamados/reconciliador.py:1685` | Nesta cópia é comentário que descreve a antiga costura direta. | **Não é entrada executável.** A operação correspondente está em `app/chamados/reconciliador.py:1700`. | Comentário em `app/chamados/reconciliador.py:1685`; escrita real em `app/chamados/services/fio.py:975`. |
| **Obrigatório:** `app/chamados/signals.py:124` → resolvedor S143 | `post_save` de chamado, quando o save escreve status encerrado (`app/chamados/signals.py:98`, `app/chamados/signals.py:104`, `app/chamados/signals.py:112`). | **Não necessariamente.** O receiver não consulta a cerca; o save de **T** já aconteceu (`app/chamados/models.py:296`, `app/chamados/signals.py:123`). | **T** antes da entrada; depois os ramos do resolvedor e, independentemente deles, possível **L** (`app/chamados/models.py:312`). |
| **Obrigatório:** `app/ponto/services/geofence.py:441`–`:448` → `retratar` | Signal de Posto e comando aplicador (`app/ponto/signals.py:306`; `app/ponto/management/commands/reconciliar_geofence.py:34`). | **Sim.** `retratar` → `_retratar` → `retratar_com_motivo` → cerca (`app/chamados/reconciliador.py:603`, `app/chamados/reconciliador.py:543`, `app/chamados/reconciliador.py:576`). | **U**, chamada efetiva em `app/ponto/services/geofence.py:444`. |
| `app/chamados/management/commands/retratar_chamados_lei_cala.py:59` → `retratar` | Comando sob `--apply`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:54`. | **Sim**, `app/chamados/reconciliador.py:576`. | **U**, `app/chamados/reconciliador.py:580`. |
| `app/ponto/management/commands/processar_alertas_turno.py:216` → `retratar` | Comando, chamado por seu `handle` (`app/ponto/management/commands/processar_alertas_turno.py:89`). | **Sim**, `app/chamados/reconciliador.py:576`. | **U**, `app/chamados/reconciliador.py:580`. |
| `app/ponto/management/commands/reconciliar_grade.py:309` → `retratar` | Comando sob aplicação, `app/ponto/management/commands/reconciliar_grade.py:305`. | **Sim**, `app/chamados/reconciliador.py:576`. | **U**, `app/chamados/reconciliador.py:580`. |
| `app/ponto/management/commands/reconciliar_vinculo.py:68` → `retratar` | Comando sob aplicação, `app/ponto/management/commands/reconciliar_vinculo.py:58`. | **Sim**, `app/chamados/reconciliador.py:576`. | **U**, `app/chamados/reconciliador.py:580`. |
| `app/ponto/management/commands/supra_juiz.py:286` → alias `_retr` | Comando com execução habilitada (`app/ponto/management/commands/supra_juiz.py:258`, `app/ponto/management/commands/supra_juiz.py:279`). | **Sim**, `app/chamados/reconciliador.py:576`. | **U**, `app/chamados/reconciliador.py:580`. |
| `app/ponto/services/justificativa.py:46` → `retratar` | Encerramento do chamado de origem pela view/aprovação (`app/ponto/views.py:1840`; `app/ponto/services/justificativa.py:217`, `app/ponto/services/justificativa.py:408`). | **Sim para a unidade de retratação**, `app/chamados/reconciliador.py:576`. A aprovação pode ter escrito a justificativa antes de chamar essa unidade (`app/ponto/services/justificativa.py:208`, `app/ponto/services/justificativa.py:386`). | **U**, `app/chamados/reconciliador.py:580`. |
| `app/chamados/reconciliador.py:1061` → `_retratar` | Absolvição de chamados pelo consumidor de eventos (`app/chamados/reconciliador.py:1426`). | **Sim**, `app/chamados/reconciliador.py:576`. | **U**, `app/chamados/reconciliador.py:580`. |
| `app/chamados/reconciliador.py:1522` → `_retratar` | Premissa morta, pelo consumidor ou reconciliação por colaborador (`app/chamados/reconciliador.py:1430`, `app/chamados/reconciliador.py:1499`). | **Sim**, `app/chamados/reconciliador.py:576`. | **U**, `app/chamados/reconciliador.py:580`. |
| `app/chamados/reconciliador.py:652` → `p.carimbar` | E2: cartório, consumidor de eventos ou reconciliação por colaborador (`app/ponto/services/cartorio.py:1007`; `app/chamados/reconciliador.py:1423`, `app/chamados/reconciliador.py:1490`). | **Não.** A seleção consulta C6 e tranca, sem veto de entrega (`app/chamados/reconciliador.py:718`, `app/chamados/reconciliador.py:724`). | **C**, save em `app/chamados/models.py:3273`; depois pode fechar **D**, `app/chamados/reconciliador.py:669`. |
| `app/chamados/reconciliador.py:1677` → `lavrar_sem_turno_aberto` → `p.carimbar` | Ramo c1 de `reatar_pergunta_orfa`. | **Não.** Guardas de motivo, resposta, turno e C6, sem K6 (`app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:373`, `app/chamados/reconciliador.py:400`). | **C**, `app/chamados/reconciliador.py:407`; depois possível **D**, `app/chamados/reconciliador.py:414`. |
| `app/chamados/reconciliador.py:1713` → costura derivável | Outro ramo de `reatar_pergunta_orfa`. | **Não**, mesmas guardas da costura, sem veto geral de entrega (`app/chamados/services/fio.py:955`, `app/chamados/services/fio.py:957`, `app/chamados/services/fio.py:824`). | **A**, `app/chamados/services/fio.py:975`. |
| `app/chamados/reconciliador.py:1679` → c2 → `app/chamados/reconciliador.py:1011` | Reemissão/costura da pergunta órfã. | **Não para todos os motivos.** K6 só é exigido fora de `MOTIVOS_C1`; os dois motivos antigos passam por outro ramo (`app/chamados/reconciliador.py:333`, `app/chamados/reconciliador.py:867`, `app/chamados/reconciliador.py:965`). | Pode julgar célula e abrir/renascer chamado antes de **A** (`app/chamados/reconciliador.py:759`, `app/chamados/reconciliador.py:967`, `app/chamados/reconciliador.py:977`, `app/chamados/reconciliador.py:1011`). |
| `app/chamados/reconciliador.py:1205` → `encerrar_fantasma` | Consumidor de eventos, `app/chamados/reconciliador.py:1433`. | **Não.** Predicado usa resposta, lâmpada, C6 e teto temporal (`app/chamados/reconciliador.py:1140`, `app/chamados/reconciliador.py:1142`, `app/chamados/reconciliador.py:1144`, `app/chamados/reconciliador.py:1146`). | **C** pela porta `app/chamados/models.py:3425`; depois possível **D**, `app/chamados/reconciliador.py:1229`. |

Os crons configurados de fantasmas, becos e geofence usam `--alarme`; os caminhos aplicadores acima continuam disponíveis por comando ou evento (`app/config/crons.py:392`, `app/config/crons.py:663`, `app/config/crons.py:486`). O consumidor de eventos está configurado por minuto (`app/config/crons.py:394`).

**Submatriz completa dos efeitos do resolvedor.** Cada entrada “resolvedor” da matriz anterior pode alcançar os ramos abaixo, conforme seus predicados; eles não são todos executados em toda chamada (`app/chamados/services/disputa_emissao.py:2086`, `app/chamados/services/disputa_emissao.py:2098`, `app/chamados/services/disputa_emissao.py:2146`, `app/chamados/services/disputa_emissao.py:2170`, `app/chamados/services/disputa_emissao.py:2208`).

| Entrada → ramo | Quem chama | Cerca/veto antes da primeira escrita? | O que grava → prova |
|---|---|---|---|
| `app/chamados/services/disputa_emissao.py:2012` → corpo privado | Todas as cinco chamadas diretas: S143, becos, retratação, HIST e fantasmas. | **Não no wrapper.** Ele trata a via trancada, pega trava e delega (`app/chamados/services/disputa_emissao.py:2021`, `app/chamados/services/disputa_emissao.py:2027`, `app/chamados/services/disputa_emissao.py:2028`). | Ramos seguintes; não há veto de entrega nesse wrapper (`app/chamados/services/disputa_emissao.py:2012`). |
| `app/chamados/services/disputa_emissao.py:2090` → `reabrir_credor_de_pergunta_viva` | Primeiro laço das ancoradas. | **Sim, somente local e parcial.** Verifica carga do credor e pergunta; ignora dia indecidível por exigir dia diferente de `None` (`app/chamados/services/disputa_emissao.py:1976`, `app/chamados/services/disputa_emissao.py:1982`, `app/chamados/services/disputa_emissao.py:1983`, `app/chamados/services/disputa_emissao.py:1998`). | Reabertura: **T/L/Z**, ou migração ao irmão; `app/chamados/models.py:1462`, `app/chamados/models.py:1489`, `app/chamados/models.py:1498`. |
| `app/chamados/services/disputa_emissao.py:2129` → carimbo S128 direto | Perguntas com destino no chamado, não materializadas e em disputa aberta (`app/chamados/services/disputa_emissao.py:1830`). | **Não.** Pula pergunta em voo, viva, trancada e resposta sem lastro; essas guardas não são veto K6 (`app/chamados/services/disputa_emissao.py:2103`, `app/chamados/services/disputa_emissao.py:2109`, `app/chamados/services/disputa_emissao.py:2113`, `app/chamados/services/disputa_emissao.py:2123`). | **C+L** no mesmo save, `app/chamados/models.py:3271`, `app/chamados/models.py:3273`. |
| `app/chamados/services/disputa_emissao.py:2154` → `_pai.reabrir()` | HX resposta visível. | **Não.** Chama diretamente `reabrir`, sem o helper de competência do primeiro laço (`app/chamados/services/disputa_emissao.py:2150`, `app/chamados/services/disputa_emissao.py:2152`, `app/chamados/services/disputa_emissao.py:2154`). | **T/L/Z**, ou migração e retratação de duplicatas; `app/chamados/models.py:1462`, `app/chamados/models.py:1489`, `app/chamados/models.py:1498`. |
| `app/chamados/models.py:1462` → migração ao irmão vivo | Recusa/redirecionamento da reabertura. | **Não há veto geral nesse percurso.** A migração seleciona vivas e aplica plano de mérito/episódios (`app/chamados/services/fio.py:2205`, `app/chamados/services/fio.py:2230`). | **A**, `app/chamados/services/fio.py:2246`; também carimbo de identidade/`recusa_motivo` em conflitantes, `app/chamados/services/fio.py:2252`, `app/chamados/services/fio.py:1410`, `app/chamados/models.py:3311`. |
| `app/chamados/models.py:1489` → reabertura normal → `ressuscitar_perguntas` | Reabertura aceita. | **Não dentro dessas portas.** O helper anterior pode ter vetado sua chamada; o HX direto não passa por ele (`app/chamados/services/disputa_emissao.py:1998`, `app/chamados/services/disputa_emissao.py:2154`). | **T**, possível **L**, depois **Z** (`app/chamados/models.py:296`, `app/chamados/models.py:312`, `app/chamados/models.py:1498`, `app/chamados/models.py:1639`). |
| **Obrigatório:** `app/chamados/services/disputa_emissao.py:1862` → seleção HX → carimbo em `:2194` | `_abertas_da_disputa_sem_credor`, chamada pelo resolvedor em `app/chamados/services/disputa_emissao.py:2173`. | **Não.** `:1862` apenas pula pai vivo; também verifica destinos vivos e exclui materializadas/validadas (`app/chamados/services/disputa_emissao.py:1862`, `app/chamados/services/disputa_emissao.py:1865`, `app/chamados/services/disputa_emissao.py:1845`). | **C**, com opção `responder`, sem S2 explícita nesse carimbo (`app/chamados/services/disputa_emissao.py:2194`; `app/chamados/models.py:3273`); trilha em `app/chamados/services/disputa_emissao.py:2197`. |
| **Obrigatório:** `app/chamados/services/disputa_emissao.py:1891` | Enumerador `_disputa_fecha_no_ato`, usado pela cerca. | **Sem escrita.** Calcula `resta` e pergunta C7 (`app/chamados/services/disputa_emissao.py:1891`, `app/chamados/services/disputa_emissao.py:1892`). | Usado para enumerar cascata/carga em `app/chamados/services/disputa_emissao.py:1911`, `app/chamados/services/disputa_emissao.py:1947`; a cauda escritora real é outra, em `app/chamados/services/disputa_emissao.py:2226`. |
| `app/chamados/services/disputa_emissao.py:2226` → `d.fechar` | Laço final do resolvedor. | **Não.** Consulta C7; não consulta entrega (`app/chamados/services/disputa_emissao.py:2225`). | **D**, `app/chamados/models.py:2027`; dispara as duas linhas seguintes. |
| `app/chamados/signals.py:50` → reconciliação do pai | Defesa 1, após save da disputa. | **Não.** Chama `ch.reconciliar`, sem unidade S128 (`app/chamados/signals.py:50`). | Pode fazer **A antes de T**, depois S143 e **L** (`app/chamados/models.py:437`, `app/chamados/models.py:655`, `app/chamados/models.py:296`, `app/chamados/signals.py:124`, `app/chamados/models.py:312`). |
| `app/chamados/signals.py:172` → `materializar_pendentes_aprovadas` | S144, após save da disputa. | **Não.** Testa pergunta em voo e lastro; não entrega (`app/chamados/services/materializacao.py:2161`, `app/chamados/services/materializacao.py:2163`). | **C**, `app/chamados/services/materializacao.py:2167` → `app/chamados/models.py:3273`. |
| `app/chamados/signals.py:266` e `app/chamados/signals.py:247` | `post_save` da pergunta. | **Não acrescentam veto.** | Acordam células e agendam avaliação de beco, realimentando o grafo (`app/chamados/models.py:3369`; `app/chamados/signals.py:246`). Save só de **L** fica fora do conjunto que acorda outbox (`app/chamados/models.py:3341`, `app/chamados/signals.py:264`). |

**Entradas adicionais de transição que alimentam S143/S2.** A rota comum é `transicionar_com_pauta` → callback `reconciliar`/`encerrar`, ou `transicionar` → `_gravar_estado` → save → S143 e eventual S2 (`app/chamados/services/fecho_runtime.py:160`, `app/chamados/services/fecho_runtime.py:165`, `app/chamados/services/fecho_runtime.py:175`, `app/chamados/models.py:450`, `app/chamados/models.py:655`, `app/chamados/models.py:1693`, `app/chamados/models.py:296`, `app/chamados/models.py:312`).

Nas linhas abaixo, **não existe passagem obrigatória pela cerca S128 na cadeia indicada**. A guarda de perguntas vivas não supre isso: ela **exclui entregues da lista de vivas**, enquanto a carga de S2 inclui todas as não validadas do destino (`app/chamados/models.py:364`, `app/chamados/models.py:365`, `app/chamados/models.py:309`, `app/chamados/models.py:310`).

| Entrada | Quem chama / gate | Cerca antes da primeira escrita? | Escritas/prova da cadeia |
|---|---|---|---|
| `app/colaboradores/services/pos_vinculo.py:43` | Serviço → `reconciliar` | Não obrigatória; rota comum acima. | **A/T/C/L/Z/D**, condicionais; `app/chamados/models.py:437`, `app/chamados/models.py:655`. |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py:141` | Comando → `reconciliar` | Não obrigatória. | Mesma rota, `app/chamados/models.py:655`. |
| `app/ponto/services/lastro.py:401` | Serviço/consumidor → `reconciliar` | Não obrigatória. | Mesma rota, `app/chamados/models.py:655`. |
| `app/ponto/services/cobertura.py:123` | Serviço → `reconciliar` | Não obrigatória. | Mesma ro

[truncado em 20000 de 40957 caracteres]

## 17/11 14:13 · sessao 643e42dfd211 · CONFRONTO-PILOTO · sonda · origem=Code

```text
SAIDA_BEGIN
sonda_leitura: sonda_mapa.py [91337151eae2675b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

TOTAL_REGISTROS 2063 ESPELHO 2030
SEM C001 prefixo16_casa=0
SEM C002 prefixo16_casa=0
SEM C003 prefixo16_casa=0
SEM C004 prefixo16_casa=0
SEM C005 prefixo16_casa=0
SEM C006 prefixo16_casa=0
SEM C007 prefixo16_casa=0
SEM C008 prefixo16_casa=0
SEM C009 prefixo16_casa=0
SEM C010 prefixo16_casa=0
SEM C011 prefixo16_casa=0
SEM C012 prefixo16_casa=0
SEM C013 prefixo16_casa=0
SEM C014 prefixo16_casa=0
SEM C015 prefixo16_casa=0
SEM C016 prefixo16_casa=0
SEM C017 prefixo16_casa=0
SEM C018 prefixo16_casa=0
SEM C019 prefixo16_casa=0
SEM C020 prefixo16_casa=0
ACHOU 0 de 20
ESPELHOS_ULT3D 1 COLABS_DISTINTOS 1
SAIDA_END
```

Saída reproduzida do dado fornecido; nova execução em produção: [nao verificado].
Segundo esse dado, casaram **0/20**, inclusive por prefixo de 16 caracteres; confirmação independente: [nao verificado].
O espelho chama o escritor compartilhado em `app/relatorios/pdf_espelho.py:1103`; ele calcula SHA-256 do PDF da primeira passagem, sem hash no rodapé, e persiste esse valor (`app/relatorios/pdf_base.py:139`, `app/relatorios/pdf_base.py:149`, `app/relatorios/pdf_base.py:152`).
O hash gravado corresponde à primeira passagem; o arquivo final entregue é reconstruído com o hash visível na segunda passagem (`app/relatorios/pdf_base.py:170`, `app/relatorios/pdf_base.py:178`, `app/relatorios/pdf_base.py:180`).
A origem e o significado dos valores da coluna Hash do auditor, e a causa das divergências, permanecem [nao verificado].

## 17/11 14:12 · sessao 2130aa0d2665 · - · - · origem=Ronald (tmux)

Censo das entradas e origens, somente pela cópia.

| Entrada | Origem e cadeia comprovada | Cerca antes do resolver |
|---|---|---|
| `app/ponto/management/commands/reconciliar_fantasmas.py:288` | Comando com `--apply` e sem `--alarme` (`:153`, `:269`). Primeiro tenta `retratar_com_motivo` (`:281`); pula fallback exclusivamente para `RECUSA_COMPETENCIA_ENTREGUE` (`:284`). Cron configurado usa `--alarme` (`app/config/crons.py:392`). | A tentativa consulta a cerca; o fallback pode ser chamado quando `rec=False` por outro motivo (`:281`, `:284`, `:288`). |
| `app/chamados/services/becos.py:55` | Signal de pergunta `app/chamados/signals.py:236` → `on_commit` (`:247`) → `fechar_beco_da_disputa` (`:246`) → `fechar_beco` (`app/chamados/services/becos.py:79`). Também comando `fechar_becos_disputa --apply` (`app/chamados/management/commands/fechar_becos_disputa.py:30`, `:57`); cron usa `--alarme` (`app/config/crons.py:663`). | Não consulta `_retratacao_toca_entregue`: apenas trava e chama resolver (`app/chamados/services/becos.py:52`, `:55`). |
| `app/chamados/signals.py:124` | S143: `post_save` de `ChamadoColaborador` (`:98`), status em ENCERRADOS (`:104`) e gravação do status reconhecida (`:112`). | Não consulta cerca; só entra em contexto de recusas e atomic (`:123`, `:124`). |
| `app/ponto/services/geofence.py:444` | Signal de Posto (`app/ponto/signals.py:291`) → `on_commit` (`:307`) → reavaliação aplicadora (`:306`). Também comando `reconciliar_geofence --apply` (`app/ponto/management/commands/reconciliar_geofence.py:26`, `:34`); cron usa `--alarme` (`app/config/crons.py:486`). | Passa por `retratar` (`app/ponto/services/geofence.py:444`) → `_retratar` (`app/chamados/reconciliador.py:603`) → `retratar_com_motivo` (`:543`) → cerca (`:576`). |
| `app/chamados/services/disputa_emissao.py:1862` | Nesta cópia é condição `_credor_vivo(d.chamado)` dentro do seletor `_abertas_da_disputa_sem_credor` (`:1848`, `:1859`). | Seletor puro, sem escrita; o resolver o chama em `:2173`. |
| `app/chamados/services/disputa_emissao.py:1891` | Nesta cópia constrói `resta` dentro de `_disputa_fecha_no_ato` (`:1881`), depois consulta `pode_fechar` (`:1892`). | Seletor puro, usado para enumerar cascata pela cerca (`:1911`). |

O resolvedor público somente trava e chama o corpo; não contém veto K6 (`app/chamados/services/disputa_emissao.py:2012`, `:2026`, `:2027`, `:2028`). Sua exceção `VIA_COMPETENCIA_TRANCADA` retorna zero no corpo (`:2048`), diferente de competência entregue, cujo juiz é consultado em `app/chamados/models.py:364`.

**Origens adicionais de S143 e da relavra S2.** `transicionar_com_pauta` chama o callback em `app/chamados/services/fecho_runtime.py:160`/`:165`, ou `chamado.transicionar` em `:175`/`:181`. A transição humana chega a `_aplicar` (`app/chamados/models.py:199`) e `_gravar_estado` (`:450`); reconciliar chega a `_gravar_estado` em `:655`; encerrar em `:1693`. O save acontece em `:296`, acordando S143; a relavra S2 pode acontecer depois, independentemente do resolver, em `:308`/`:312`.

Sites runtime encontrados de `transicionar_com_pauta(... ato=*.reconciliar)`, além de retratação/S88:

- `app/colaboradores/services/pos_vinculo.py:43`.
- `app/colaboradores/management/commands/marcar_feriado_do_posto.py:141`.
- `app/ponto/services/lastro.py:401`.
- `app/ponto/services/cobertura.py:123`, `:319`.
- `app/ponto/services/cartorio.py:1011`, `:1539`, `:1552`, `:1573`, `:1652`, `:1667`.
- `app/ponto/services/entrada_sincronizada.py:32`.
- `app/ponto/services/cluster_espurio.py:190`.
- `app/ponto/services/ausencia.py:841`.
- `app/ponto/services/veredito_celula.py:203`.
- `app/ponto/services/triagem_batida.py:598`.
- `app/ponto/management/commands/detectar_ausencias.py:159`, `:826`, `:837`, `:849`.
- `app/ponto/management/commands/curar_residuo_bug115.py:155`.
- `app/ponto/management/commands/detectar_intervalo_ausente.py:139`.
- `app/chamados/services/silencio_ausencia.py:102`.
- `app/chamados/services/materializacao.py:1067`, `:1256`.
- `app/chamados/services/limbo_folgas.py:102`, `:284`, `:287`.
- `app/chamados/services/feriado_reavaliar.py:48`.
- `app/chamados/services/adesao.py:116`, `:151`.
- `app/chamados/management/commands/auditar_chamados_orfaos.py:68`.

Sites usando `ato=*.encerrar`: `app/chamados/reconciliador.py:505`; `app/colaboradores/services/vinculo.py:404`; `app/colaboradores/services/desligamento.py:187`; `app/chamados/services/regularizacao_ext.py:206`.

Sites de transição humana que podem encerrar: `app/colaboradores/views.py:2112`; `app/chamados/services/acoes_chamado.py:71`, `:110`, `:155`, `:229`, `:286`, `:325`; `app/chamados/services/fio.py:769`; wrapper `app/chamados/models.py:455`; comandos `app/chamados/management/commands/smoke_chamados.py:135`, `:148`, `:164`; `app/ponto/management/commands/fechar_chamados_meio_periodo.py:110`.

Não incluir `acoes_chamado.py:241` como S143: pede `em_analise`. Não incluir CANCELAR: `ENCERRADOS=('resolvido','fechado')`, enquanto cancelado está apenas em TERMINAIS (`app/chamados/catalogo/motor.py:30`, `:31`). O gate analisar em `app/chamados/models.py:853` também não chega a S143. Reabrir em `:1489` pode atingir S2 pela mudança de fronteira (`:273`), mas S143 recusa seu status vivo (`app/chamados/signals.py:104`).

**Fechos externos de disputa que chegam à Defesa 1/S143/S144.** Cada `DisputaSupervisao.fechar` grava o fechamento em `app/chamados/models.py:2027`; Defesa 1 tenta reconciliar o pai em `app/chamados/signals.py:50`; S144 chama `materializar_pendentes_aprovadas` em `:172`. Sites encontrados:

- `app/api/foto_recebida.py:84`.
- `app/chamados/reconciliador.py:414`, `:669`, `:1229`.
- `app/chamados/models.py:2041` (`admin_encerra`, sem caller runtime encontrado), `:2163` (`remover_motivo`, chamado por `app/escala/signals.py:174` e `app/colaboradores/signals.py:77`).
- `app/colaboradores/services/desligamento.py:167`.
- `app/ponto/services/entrada_sincronizada.py:60`.
- `app/chamados/services/materializacao.py:1071`, `:2030`.
- `app/chamados/services/disputa_emissao.py:1298`, `:2226`.
- `app/chamados/services/competencia_trancada.py:229`.
- `app/chamados/services/becos.py:58`.
- `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`.

A guarda de perguntas vivas **não é veto de entrega**: `_vivas_entre` exclui perguntas entregues com `continue` (`app/chamados/models.py:364`, `:365`); `_ancoradas` inclui qualquer competência (`:344`), e a relavra S2 busca todas as não validadas sem filtro K6 (`:309`, `:310`, `:312`).

**Reprodução do censo:** executei `rg` dos nomes `resolver_perguntas_por_chamado_resolvido`, `_resolver_perguntas_por_chamado_resolvido`, `retratar`, `_retratar`, `retratar_com_motivo`, `reavaliar_chamados_geofence`, `fechar_beco`, `fechar_beco_da_disputa`; depois AST de todos `.py` fora de `tests`/`migrations`, coletando imports com `asname`, `Call(Name/Attribute)` e referências `Attribute` usadas como callback. Na expansão S143 acrescentei `transicionar_com_pauta`, `transicionar`, `reconciliar`, `_gravar_estado`, `fechar`, `encerrar`, `cancelar`, `superar`, `encerrar_por_competencia_trancada`, `admin_encerra`. AST é necessária porque `ato=ch.reconciliar` não é `Call`; o próprio código documenta falsos positivos de grep e alias em `app/core/management/commands/gerar_diagrama.py:69`, `:87`, `:88`, e a limitação de `getattr` em `:90`. Fechamento diante de referências dinâmicas arbitrárias: [nao verificado].

## 17/11 14:12 · sessao 4a67d85ceaa0 · RESPOSTAS-1410 · verificador · origem=Code

**Item 2**

1. **Premissas: parcialmente conferem.** A recusa do POST consulta a porta e retorna sem lavrar medição (`app/folha/views.py:124-133`). Porém, o comando só aceita `--mes` e `--ano`; não há `--tentativa` nessa interface (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`). O registro em crons declara execução sob demanda, sem agendamento (`app/config/crons.py:887-897`). Hoje se sobrescreve um carimbo por empresa/dia; exceção em `medir` termina antes de lavrá-lo (`app/folha/porta_export.py:772-796`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`).
2. **Lei:** escritor único concorda com L-007 (`app/docs/LEIS.md:65`). A existência do cron pressuposto por R9 não está demonstrada pelo registro fornecido, que declara justamente não agendar (`app/config/crons.py:887-897`). Selagem pelos commits: **[nao verificado]**.
3. **Literal incompleto:** “toda execução” precisa definir tentativa por invocação ou por empresa: o comando percorre empresas e aborta na primeira exceção, podendo já ter lavrado empresas anteriores (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`). O carimbo existente tampouco preserva tentativas separadas ou competências distintas no mesmo dia (`app/folha/porta_export.py:772-776`).
4. **Dinheiro/!：** a escrita examinada é de `MetricaSnapshot`; não altera o gravado da folha (`app/folha/porta_export.py:768-796`). Alterar `FechamentoMensal` exportado continuaria exigindo `!` e a porta própria (`CLAUDE.md:411-417`).

**ITEM 2: PROCEDE — AVAL-DEVOLVIDO: identificar o cron de R9 e definir a unidade da tentativa; a interface e o registro atuais não correspondem às premissas descritas.** (`app/config/crons.py:887-897`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`, `86-95`)

**Item 3**

1. **Premissas conferem como possibilidade de código.** `data_evento` admite NULL; C6 pode manter viva uma pergunta sem dia e sem materialização (`app/chamados/models.py:2528-2530`; `app/chamados/juizes.py:1470-1483`). O cálculo de C6 não condiciona a vida ao estado aberto/fechado da disputa (`app/chamados/juizes.py:1403-1495`). Estoque real nesse estado: **[nao verificado]**.
2. **Não demonstrei colisão do “esperado 0”.** O nascimento exige dia e já existe contador esperado zero para linhas que violam esse contrato (`app/chamados/models.py:2892-2895`; `app/chamados/services/fio.py:1199-1210`). Isso não demonstra quantidade zero em produção; número medido exige a fonte real (`CLAUDE.md:35-36`).
3. **O literal fecha usando** pergunta de fato (`conversa=False`), `data_evento is None` e vida respondida por C6; conversa tem outra semântica de data (`app/chamados/models.py:2532-2540`; `app/chamados/juizes.py:338-342`). O contador deve declarar fonte, unidade, universo e exclusões (`CLAUDE.md:192`).
4. **Dinheiro/!：** contar essas perguntas não autoriza datá-las, ressuscitá-las ou corrigir gravado exportado; esse limite permanece literal (`CLAUDE.md:265-268`). A retenção existente por célula é uma decisão própria do retentor (`app/folha/export.py:1270-1296`).

**ITEM 3: NAO PROCEDE — não há bloqueante demonstrado; “esperado 0” é compatível com o contrato existente, sem afirmar estoque zerado.** (`app/chamados/models.py:2892-2895`; `app/chamados/services/fio.py:1199-1210`)

**Item 4**

1. **A distinção de donos existe**, com ações de colaborador, admin e sistema (`app/chamados/services/tarefa_do_dia.py:157-186`). Porém, `nao_sei` não é um estado exclusivo: absorção pendente conserva ação administrativa e marca desconhecimento simultaneamente (`app/chamados/services/tarefa_do_dia.py:186`, `334-341`).
2. **4(d), isoladamente, preserva a retenção atual**, inclusive `sem_celula` independentemente de ação humana (`app/folha/export.py:1270-1296`). Mas preservar retenções não preserva necessariamente as **isenções**: L-033 manda manter aberta a pergunta de intervalo sem bloquear a folha, e o retentor aplica essa exceção (`app/docs/LEIS.md:81`; `app/folha/export.py:1287-1294`). O novo ramo precisa respeitá-la explicitamente.
3. **Há interseção sem precedência definida:** ação humana + `nao_sei=True` satisfaz 4(a) e 4(c) com resultados opostos. A porta publicada confirma que desconhecimento pode coexistir com porta humana (`app/chamados/services/tarefa_do_dia.py:669-670`, `719-722`). Também é preciso distinguir ações das instâncias da ação efetivamente alcançável: a tarefa filtra alcance antes de publicar o dono (`app/chamados/services/tarefa_do_dia.py:637-669`).
4. **Dinheiro/!：** retenção participa diretamente da decisão de entrada no TXT (`app/folha/export.py:968-973`). Isso não autoriza modificar gravado exportado; esse ato exige `!` (`CLAUDE.md:411-417`).

**ITEM 4: PROCEDE — AVAL-DEVOLVIDO: resolver ação humana junto de `nao_sei` e conservar a isenção L-033 no novo ramo.** (`app/chamados/services/tarefa_do_dia.py:334-341`; `app/docs/LEIS.md:81`; `app/folha/export.py:1287-1294`)

**Item 5**

1. **Premissa confere.** K6 está registrado como `folha/export.py::competencia_entregue`; responde pela exportação vigente não invalidada, sem consultar aprovação ou tranca (`app/core/juizes.py:343`; `app/folha/export.py:580-594`). Invalidar a última exportação não ressuscita automaticamente a anterior (`app/folha/models.py:217-225`; `app/folha/export.py:591`).
2. **Não demonstrei colisão.** TXT é fotografia, não pagamento (`CLAUDE.md:418-422`). O tratamento de competência paga fora está registrado como construção condicionada a cadastro pela tela e medida antes de valer em produção (`app/docs/CORTES.md:496`). Implantação efetiva: **[nao verificado]**.
3. **Os ramos fecham como delimitação do universo.** Trancada sem TXT passa pelo critério K6, mas isso não obriga retenção: C6 pode calar a pergunta e a tarefa pode retornar sem ações pela tranca (`app/folha/export.py:588-594`; `app/chamados/juizes.py:1407-1410`; `app/chamados/services/tarefa_do_dia.py:221-229`).
4. **Dinheiro/!：** K6 consulta exportações; não reescreve dinheiro (`app/folha/export.py:580-594`). “Sem TXT” não dispensa as condições próprias de eventual apply em competência não exportada (`CLAUDE.md:406-410`).

**ITEM 5: NAO PROCEDE — K6 responde ao critério declarado, inclusive TXT invalidado sem substituto; tranca continua sendo julgada pelas autoridades existentes.** (`app/folha/export.py:564-594`; `app/chamados/juizes.py:1407-1410`)

**Item 7**

1. **Premissas não integralmente verificadas.** O hook recebe JSON, mas só consome `transcript_path`; o fornecimento real de `stop_hook_active` é **[nao verificado]** (`bin/hook_stop_fila1.py:1339-1347`). O formato sem PID/arquivo é recusado atualmente (`bin/hook_stop_fila1.py:814-830`). Implementação e comportamento do RETOMA-AUTO: **[nao verificado]**.
2. **Há conflito a esclarecer com a regra vigente:** ESPERANDO libera com produtor vivo provado; aceitar somente um nome não apresenta essa prova (`CLAUDE.md:262-264`; `bin/hook_stop_fila1.py:527-541`). Além disso, a prova do RELATO precede qualquer liberação (`bin/hook_stop_fila1.py:1349-1358`).
3. **O literal não fecha na interseção** `stop_hook_active=true` + ESPERANDO com PID morto: 7(a) manda liberar, enquanto o RED manda continuar recusando. A recusa do PID morto existe e tem caso nominal (`bin/hook_stop_fila1.py:832-836`; `bin/tests/test_hook_esperando_com_prova.sh:95-96`). Falta definir precedência também perante RELATO sem prova (`bin/tests/test_hook_esperando_com_prova.sh:186-189`).
4. **Dinheiro/!：** a alteração proposta alcança a autorização de encerrar o turno; liberar antes da prova contornaria uma guarda vigente, e atalhos permanecem fora da pré-aprovação (`bin/hook_stop_fila1.py:1349-1358`; `CLAUDE.md:265-268`). Efeito sobre exportada ou dinheiro: **[nao verificado]**.

**ITEM 7: PROCEDE — AVAL-DEVOLVIDO: definir precedências, comprovação do subagente e localizar o RETOMA-AUTO antes de certificar 7(c).** (`bin/hook_stop_fila1.py:527-541`, `832-836`, `1349-1358`; RETOMA-AUTO **[nao verificado]**)

## 17/11 14:11 · sessao 562028e7c1f7 · - · - · origem=Ronald (tmux)

Submatriz do resolvedor e efeitos, somente código da cópia:

| Entrada / subramo | Cerca antes da escrita? | Escrita e prova |
|---|---|---|
| `resolver_perguntas_por_chamado_resolvido` | **Não**: wrapper só trata via trancada e trava; não consulta `_retratacao_toca_entregue` nem `veto_de_competencia` (`app/chamados/services/disputa_emissao.py:2021`, `:2027`, `:2028`). | Encaminha todos os ramos abaixo (`app/chamados/services/disputa_emissao.py:2031`). |
| S128 direto | **Não** no resolvedor; C6/tranca/resposta são outras guardas (`app/chamados/services/disputa_emissao.py:2103`, `:2109`, `:2113`, `:2123`). | `PerguntaDisputa.materializada_em/materializada_por/via_resolucao`, eventualmente `respondida_em/validada_em/validada_por`, e S2 `veredito_lavrado/julgado_em/janela_expira_em`: chamada `:2129`; atribuições em `app/chamados/models.py:3220`, `:3253`, `:3256`; lavra `:3271`; save `:3273`. |
| HX sem credor | **Não**: seleção por credores/vitalidade, sem competência (`app/chamados/services/disputa_emissao.py:2173`, `:2182`). | Carimbo e eventual `respondida_em`, sem S2 explícita: `app/chamados/services/disputa_emissao.py:2194` → `app/chamados/models.py:3250`, `:3253`, `:3273`. Também evento `fecho_sem_credor` (`app/chamados/services/disputa_emissao.py:2197`). |
| Reabrir credor de pergunta viva, antes de carimbar | **Sim, parcial**: guarda local de competência na carga, mas ignora dia `None` (`app/chamados/services/disputa_emissao.py:1976`, `:1982`, `:1983`, `:1992`, `:1998`). Não é a cerca `_retratacao_toca_entregue`. | `credor.reabrir` (`:1998`) pode passar pela transição/S2/ressuscitação ou migração abaixo (`app/chamados/models.py:1462`, `:1489`, `:1498`). |
| HX resposta visível | **Não**: chama `_pai.reabrir()` diretamente, sem helper acima (`app/chamados/services/disputa_emissao.py:2146`, `:2150`, `:2152`, `:2154`). | Transição/S2/ressuscitação ou migração abaixo; depois nota (`app/chamados/services/disputa_emissao.py:2156`). |
| Reabrir normal | **Não** em `reabrir`: decide recusas e chama escritor (`app/chamados/models.py:1437`, `:1489`). | `ChamadoColaborador.status_local/resolvido_em/via_encerramento/atualizado_em`, urgência conforme estado (`app/chamados/models.py:228`, `:229`, `:248`, `:251`, save `:296`); antes há evento `estado_chamado` (`:292`). |
| S2 pela transição | **Não**: ocorre após save do chamado, sem veto (`app/chamados/models.py:296`, `:305`, `:308`, `:312`). | `PerguntaDisputa.veredito_lavrado/julgado_em/janela_expira_em` (`app/chamados/veredito_lavrado.py:270`, `:272`, `:273`, save `:275`). |
| Ressuscitar após reabrir | **Não** dentro de `ressuscitar_perguntas` (`app/chamados/models.py:1601`, `:1615`, `:1639`). | Primeiro registra decisões antigas (`:1621`); limpa carimbo, validação e resposta inteira (`:1639` → `:3402`–`:3404`); acorda células (`:1642`); reabre disputa (`:1649`); registra auditoria (`:1663`). |
| Reabrir redirecionado ao irmão | **Não**: ramo `irmao_vivo` chama migração (`app/chamados/models.py:1458`, `:1462`). O plano de retratação avalia mérito/episódios, sem competência (`app/chamados/services/fio.py:1330`–`:1349`). | Migra destino e S2 (`app/chamados/services/fio.py:2246` → `:973`–`:976`); pode retratar perguntas conflitantes do novo destino (`:2225`, `:2252` → `:1392` → `:1410` → `app/chamados/models.py:3311`). |
| Fecho final do resolvedor | **Não**: C7/tranca, sem competência entregue (`app/chamados/services/disputa_emissao.py:2215`, `:2225`, `:2226`). | `DisputaSupervisao.fechada_em/fechada_por/resolucao/motivo_fechamento` (`app/chamados/models.py:2023`–`:2027`); dispara Defesa 1 e S144 (`app/chamados/signals.py:26`, `:156`). |
| Defesa 1 após fecho | **Não**: usa `ch.reconciliar`, sem passar por `retratar_com_motivo` (`app/chamados/signals.py:50`). | Pode reapontar pergunta **antes** do estado (`app/chamados/models.py:689` → `:437`), depois transição/S143/S2 (`:655`, `:296`, `app/chamados/signals.py:124`, `app/chamados/models.py:312`); nota (`app/chamados/signals.py:53`). |
| S144 após fecho | **Não**: testa pergunta em voo e lastro, sem competência (`app/chamados/services/materializacao.py:2161`, `:2163`, `:2167`). | Carimbo pela porta `carimbar`, save `app/chamados/models.py:3273`; disparador em `app/chamados/signals.py:172`. |
| `post_save` da pergunta | Cerca anterior depende do caminho; receiver não aplica veto (`app/chamados/signals.py:260`–`:266`). | Carimbo/resposta/destino acordam célula (`:266` → `app/chamados/models.py:3369` → `app/ponto/portas/celula.py:954`, `EventoCelula.objects.create`); agenda beco em `on_commit` (`app/chamados/signals.py:246`, `:247`). Save só de S2 não acorda outbox (`app/chamados/signals.py:264`, `app/chamados/models.py:3341`). |

**As linhas obrigatórias `disputa_emissao.py:1862` e `:1891` são seleção, não escritores.** A primeira verifica pai vivo dentro de `_abertas_da_disputa_sem_credor`; a segunda calcula `resta` em `_disputa_fecha_no_ato` (`app/chamados/services/disputa_emissao.py:1848`, `:1862`, `:1881`, `:1891`). O resolvedor chama a seleção HX em `:2173`; a cauda real usa `d.pode_fechar` e `d.fechar` diretamente (`:2225`, `:2226`).

**Há também incompletude independente na enumeração da cerca.** `perguntas_alcancadas_pela_retratacao` enumera perguntas dos credores `[chamado] + encerrados_pela_cascata` (`app/chamados/services/disputa_emissao.py:1937`, `:1938`, `:1942`, `:1943`). A cascata só acrescenta pai que `defesa_1_reconcilia` aceite (`:1909`, `:1911`), cujo predicado exclui pai já `ENCERRADO` (`app/chamados/signals.py:23`). Porém HX pode reabrir precisamente pai já encerrado: pula somente pai em `VIVOS` (`app/chamados/services/disputa_emissao.py:2150`) e chama `reabrir` (`:2154`). Esse pai, suas perguntas ressuscitáveis e eventuais conflitantes do irmão não são necessariamente enumerados (`app/chamados/services/disputa_emissao.py:1942`, `:1943`; `app/chamados/services/fio.py:2225`, `:2252`). Existência desses casos em produção: **[nao verificado]**.

**Conclusão de desenho:** colocar cerca somente em `carimbar`, `lavrar` ou no resolvedor chega depois de outras escritas: transição grava antes de S143/S2 (`app/chamados/models.py:292`, `:296`, `:312`), e `reconciliar` pode reapontar antes de `_gravar_estado` (`:689`, `:437`, `:655`). Portanto, essas portas isoladas não constituem ponto único antes da primeira escrita da unidade. A barreira precisa anteceder a aplicação das operações de transição/reatamento, além das entradas autônomas do resolvedor e dos escritores diretos que a matriz principal identificar; chamar isso de lista mínima global sem esse restante seria **[nao verificado]**.

Censo executado em memória com `python3`/AST, sem importar Django: arquivos de `app/**/*.py`, excluindo `tests` e `migrations`; `ast.Call` por `Name`/`Attribute`, resolvendo `ImportFrom.asname` para `lavrar`, e inspeção dos corpos encontrados. Nomes: `resolver_perguntas_por_chamado_resolvido`, `_resolver_perguntas_por_chamado_resolvido`, `reabrir_credor_de_pergunta_viva`, `_perguntas_do_fecho_direto`, `_abertas_da_disputa_sem_credor`, `_disputa_fecha_no_ato`, `perguntas_alcancadas_pela_retratacao`, `carimbar`, `lavrar`, `materializar_pendentes_aprovadas`, `migrar_resolvedoras_vivas`, `reapontar_resolvedora`, `retratar_para_canonica`, `carimbar_identidade`, `descarimbar_em_lote`. Censo de chamadas do resolvedor: `signals.py:124`, `becos.py:55`, `reconciliador.py:593/:1706`, `reconciliar_fantasmas.py:288`; alias reexportado em `models.py:4274`.

## 17/11 14:11 · sessao d10afe6d2b0e · - · - · origem=Ronald (tmux)

A cerca de S128 existe em `app/chamados/reconciliador.py:576`, antes da transição em `:580`, mas E2, HIST e c1 escrevem sem atravessá-la (`app/chamados/reconciliador.py:652`, `:1700`, `:407`).

| Entrada | Chamador | Cerca antes da escrita? | Escrita/prova |
|---|---|---|---|
| `app/chamados/management/commands/retratar_chamados_lei_cala.py:59` | comando `handle`, `--apply` (`:26`, `:54`) | Sim, S128 `app/chamados/reconciliador.py:576` | Transição `:580`, justificativa `:592`, resolver `:593`, ocorrência `:595` |
| `app/ponto/management/commands/processar_alertas_turno.py:216` | comando, `_resolver_chamado_do_turno` (`:153`), chamado por `handle` em `:89` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`app/chamados/reconciliador.py:580`, `:592`, `:593`, `:595`) |
| `app/ponto/management/commands/reconciliar_grade.py:309` | comando sob `apply` (`:305`) | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/ponto/management/commands/reconciliar_vinculo.py:68` | comando sob `apply` (`:58`) | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/ponto/management/commands/supra_juiz.py:286` | comando, alias `_retr` (`:258`), `--executar` e trava T6 (`:279`) | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/ponto/services/geofence.py:444` | comando `app/ponto/management/commands/reconciliar_geofence.py:34`; signal do Posto `app/ponto/signals.py:306` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/ponto/services/justificativa.py:46` | `encerrar_chamado_de_origem`, chamado em `app/ponto/views.py:1840` e `app/ponto/services/justificativa.py:217`, `:408` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`); aprovação já escreveu Justificativa antes (`app/ponto/services/justificativa.py:208`, `:386`) |
| `app/ponto/management/commands/reconciliar_fantasmas.py:281` | comando | Sim, S128 `app/chamados/reconciliador.py:576`; recusa de competência impede fallback (`app/ponto/management/commands/reconciliar_fantasmas.py:284`) | Fallback resolver `:288` ainda acontece para outras recusas, inclusive retenção anterior à cerca (`app/chamados/reconciliador.py:564`) |
| `app/chamados/reconciliador.py:1061` | consumidor outbox em `:1426`; comando `app/chamados/management/commands/consumir_eventos_celula.py:31` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/chamados/reconciliador.py:1522` | consumidor outbox `:1430`; reconciliador por colaborador `:1499`, signals `app/ponto/signals.py:215` e `app/chamados/signals.py:377` | Sim, S128 `app/chamados/reconciliador.py:576` | Mesmos efeitos S128 (`:580`, `:592`, `:593`, `:595`) |
| `app/chamados/reconciliador.py:652` | E2: outbox `:1423`; reconciliação por colaborador `:1490`; cartório `app/ponto/services/cartorio.py:1007` | **Não**: filtro verifica C6 e tranca, sem K6 (`app/chamados/reconciliador.py:718`, `:724`) | `PerguntaDisputa.materializada_em/materializada_por/via_resolucao`, save em `app/chamados/models.py:3273`; pode fechar disputa depois (`app/chamados/reconciliador.py:668`) |
| `app/chamados/reconciliador.py:1677` | `reatar_pergunta_orfa`; callers `:1584` e `app/chamados/services/disputa_emissao.py:473` | **Não**: c1 verifica motivo, resposta, materialização, turno e C6 (`app/chamados/reconciliador.py:363`, `:366`, `:368`, `:373`, `:400`) | carimbo em `:407` → save `app/chamados/models.py:3273`; fecho disputa em `app/chamados/reconciliador.py:414` |
| `app/chamados/reconciliador.py:1700` → `:1706` | HIST de `reatar_pergunta_orfa`; callers `:1584` e `app/chamados/services/disputa_emissao.py:473` | **Não**: entregue permanece no caminho histórico (`app/chamados/reconciliador.py:1650`); costura libera entregue em `app/chamados/services/fio.py:824` | Primeiro `PerguntaDisputa.chamado_destino`, eventualmente `data_evento`, `veredito_lavrado/julgado_em/janela_expira_em` em `app/chamados/services/fio.py:975`; só depois resolver em `app/chamados/reconciliador.py:1706` |
| `app/chamados/reconciliador.py:1713` | ramo derivável de `reatar_pergunta_orfa`; mesmos callers `:1584` / `app/chamados/services/disputa_emissao.py:473` | **Não**, costura sem veto geral K6 (`app/chamados/services/fio.py:955`, `:957`, `:974`) | Mesmos campos da costura em `app/chamados/services/fio.py:975` |
| `app/chamados/reconciliador.py:1679` → `:1011` | c2 de `reatar_pergunta_orfa` | **Parcial**: K6 somente para motivos fora `MOTIVOS_C1` (`:867`, `:965`); `orfao_14h/saida_sem_entrada` são `MOTIVOS_C1` (`:333`) | Antes da costura pode julgar célula por `_ctx_c2` (`:977` → `:759`) e abrir/renascer chamado (`:967`); costura grava em `app/chamados/services/fio.py:975` |

O wrapper público `retratar` encaminha a `_retratar` e este a `retratar_com_motivo` (`app/chamados/reconciliador.py:603`, `:543`). A primeira escrita de estado do Chamado fica em `app/chamados/models.py:296`, precedida da trilha em `:292`; pode haver reaponte antes, pois `reconciliar` chama `_decisao_do_fechamento` em `:652`, esta chama `_gate_pergunta_viva` em `:689`, e o gate costura em `:437`, antes de `_gravar_estado` em `:655`. Os candidatos desse gate excluem competência entregue (`app/chamados/models.py:364`).

O cartório também chama diretamente `reconciliar`, sem S128, em `app/ponto/services/cartorio.py:1011`; portanto `_gravar_estado` recebe caminhos que não passaram pela cerca `app/chamados/reconciliador.py:576`.

Para a lista fechada operacional, precisam ser contemplados pelo menos: unidade S128 antes da transição (`app/chamados/reconciliador.py:580`), resolver direto, primeira escrita da costura HIST/derivável (`app/chamados/services/fio.py:975`), carimbos E2/c1 (`app/chamados/reconciliador.py:652`, `:407`), e transições diretas com S143/relavra (`app/chamados/models.py:296`, `:312`). Somente cercar carimbo é tardio para a transição e insuficiente para a costura; somente cercar `_gravar_estado` não alcança resolver direto, E2 ou HIST (`app/chamados/reconciliador.py:1700`, `:1706`, `:652`).

Censo executado em leitura: `rg` dos nomes e imports, seguido de `python3 -B` com `ast.parse` sobre `app/**/*.py`, excluindo `tests/migrations/__pycache__`, coletando `ImportFrom` com alias e `Call(Name|Attribute)`. Nomes: `retratar`, `_retratar`, `retratar_com_motivo`, `retratar_perguntas_absolvidas_por_celula`, `retratar_chamados_absolvidos_por_celula`, `retratar_premissa_morta_colab`, `reconciliar_chamados_colab`, `reatar_pergunta_orfa`, `reconciliar_perguntas_orfas_colab`, `lavrar_sem_turno_aberto`, `reemitir_chamado_do_dia` [nao verificado: comando executado, sem arquivo de saída por regra de leitura]. Homônimos excluídos: `_retratar` de Batida em `app/ponto/management/commands/detectar_par_relampago.py:217` e `retratar_humano` importado em `app/ponto/views.py:3355`.

## 17/11 14:10 · sessao 65a6af02077b · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Item 3(a): não confirmei colisão do “esperado 0”.** `data_evento` permite NULL (`app/chamados/models.py:2528`), mas isso sozinho não prova legitimidade do estado. Já existe contador esperado 0 para dia nulo/divergente, com universo e exclusões declarados (`app/chamados/services/fio.py:1199`, `:1203`, `:1209`). O contador novo precisa declarar universo, unidade e exclusões (`CLAUDE.md:192`); não pode apresentar meta como número medido (`CLAUDE.md:35`). Quantidade real de perguntas vivas sem dia: **[não verificado]**. O acervo anterior ao horizonte é explicitamente congelado (`app/chamados/models.py:2500`, `:2505`; `app/docs/LEIS.md:145`).

- **Item 4(d): preservar retenções existentes é compatível com o retentor atual**, que continua retendo `sem_celula` e vereditos acusadores independentemente de tarefa humana (`app/folha/export.py:1270`, `:1273`, `:1293`, `:1295`). Portanto, “ninguém age” não libera automaticamente retenção antiga.

- **Item 4: colisão adicional com L-033.** A lei mantém pergunta de intervalo aberta **sem bloquear folha** (`app/docs/LEIS.md:81`). O retentor aplica expressamente essa isenção (`app/folha/export.py:1173`, `:1287`, `:1293`); o teste fixa `veredito='furo'`, protesto preservado e retenção vazia (`app/folha/tests/test_furo_so_intervalo_nao_retem.py:169`). Um novo ramo que retém por qualquer pergunta viva com ação humana precisa conservar essa exceção. “Só acrescenta retenção” não resolve essa precedência. **ITEM 4: PROCEDE**, além da coexistência `nao_sei` + ação humana já encontrada pelo principal.

- **Item 5: premissa K6 confere.** Autoridade registrada em `app/core/juizes.py:343`; implementação só consulta a tripla empresa/competência e exige exportação vigente não invalidada (`app/folha/export.py:580`, `:588`, `:591`). Não consulta aprovação nem tranca. O desempate pelo último `(gerado_em, pk)` fica em `app/folha/models.py:217`; invalidar o último não ressuscita o anterior (`:224`; `app/folha/export.py:591`). A competência paga fora está registrada como construção (`app/docs/CORTES.md:232`, `:496`); implantação ou cadastro real: **[não verificado]**. **ITEM 5: NAO PROCEDE** nesta revisão.

- **Limite de autorização dos itens 3–5:** mudar retenção altera quem entra no TXT, cuja decisão é `classificar_export` (`app/folha/export.py:765`, `:768`). Não autoriza tocar gravado exportado: exige `!` (`CLAUDE.md:265`); escopo literal não se amplia por alegar que dinheiro não mudou (`:268`). Apply em não exportada segue a autorização própria de `CLAUDE.md:286`.

_6583 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
