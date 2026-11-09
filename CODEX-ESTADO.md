# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 12:02** · publicado em **09/11/2026 12:05** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 12:02 · sessao 03267d961e14 · - · - · origem=Ronald (tmux)

3. **Fotografia e identidade.** `_censo_nega` monta `SNAPSHOT <obra> :: N=<máximo dos briefs> ... <sha12>:N=...`, grava pela porta `trilha` no CLI e acumula eventos no Agent; `pretool` grava o acumulado depois que `_pretool` passa (`bin/disjuntor.py:1878`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:1885`, `bin/disjuntor.py:1967`). Caminho = `logs/sessao/disjuntor.log`; chave = obra literal; leitor `_snapshot_etapas` aceita a última linha dessa obra e retorna **somente N**, ignorando hashes/pid/briefs (`bin/disjuntor.py:131`, `bin/disjuntor.py:869`, `bin/disjuntor.py:874`, `bin/disjuntor.py:882`).

**Defeito de ordem comprovado:** o wrapper grava a fotografia no `--censo` **antes** de obter a vaga e validar o HEAD; nova tentativa com mesmo ID pode mudar a última fotografia e depois ser recusada pelo `flock`, sem nova abertura bem-sucedida (`bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:239`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:241`; leitura última foto `bin/disjuntor.py:882`). O selo testa reabertura bem-sucedida; não cobre tentativa que morre depois do censo (`bin/tests/test_etapas_no_brief.sh:234`, `bin/tests/test_etapas_no_brief.sh:237`).

Na via wrapper, abertura recebe `$CODEX_OBRA`; no final, B3 extrai a primeira obra do primeiro prompt do transcript. Não há vínculo entre esses dois IDs (`bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:234`, `bin/disjuntor.py:622`, `bin/disjuntor.py:666`). Formas também diferem: censo aceita `[A-Za-z0-9-]+`; B3 admite identidades com cauda, como `O9 (C)` e `O9 NOTURNO-X` (`bin/disjuntor.py:172`, `bin/disjuntor.py:174`, `bin/disjuntor.py:634`). IDs distintos produzem ausência de foto correspondente e `SEM-SNAPSHOT`; ID não reconhecido pelo B3 entra antes em final sem obra (`bin/disjuntor.py:666`, `bin/disjuntor.py:882`, `bin/disjuntor.py:917`). Frequência real desses casos = `[nao verificado]`.

4. **MEDIDA / DIETA.** Formato efetivo: linha após `strip().lstrip('*_`> -')`, regex `^ETAPA (\S+) (\d+):(.*)->\s*(.*?)\s*$`; obra por igualdade exata, K único em `1..N`, trecho medido com ASCII alfanumérico e que não comece, após `upper()`, por `RESULTADO OBSERVAVEL` (`bin/disjuntor.py:871`, `bin/disjuntor.py:896`, `bin/disjuntor.py:897`, `bin/disjuntor.py:900`, `bin/disjuntor.py:902`). Isso reconhece a presença textual; não valida execução, valor esperado ou correspondência do resultado à etapa (`bin/disjuntor.py:900`, `bin/disjuntor.py:902`).

DIETA manda mover conteúdo com mais de três dias para `RELATO-ARQUIVO.md`; `_etapas_medidas` abre exclusivamente `RELATO.md` (`CLAUDE.md:439`, `app/docs/LEIS.md:135`, `bin/disjuntor.py:135`, `bin/disjuntor.py:892`). Se o movimento ocorre **antes da primeira conferência daquele final**, as linhas desaparecem do universo lido, `n_rel=0`, `faltam=N`, e entra em METADE mesmo com handback N/N (`bin/disjuntor.py:902`, `bin/disjuntor.py:922`, `bin/disjuntor.py:930`, `bin/disjuntor.py:936`). Se o final já foi conferido, memo impede releitura; portanto DIETA não derruba retroativamente todo final (`bin/disjuntor.py:913`).

Outro leitor que já una RELATO e RELATO-ARQUIVO: `[nao verificado]`. Busca literal em `.py/.sh`, inclusive ocultos/ignorados, encontrou apenas referências em prosa e testes; prosa não prova autoridade leitora (`app/core/esteira_vigia.py:823`, `bin/gerar_cortes.py:131`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:193`).

5. **SEM-SNAPSHOT.** É fail-open demonstrável: ausência de foto grava memo e aviso, depois `continue`, sem conferir sequer o RELATO e sem adicionar reprovado; o final nunca é reavaliado se a fotografia surgir depois (`bin/disjuntor.py:917`, `bin/disjuntor.py:918`, `bin/disjuntor.py:921`, `bin/disjuntor.py:913`). O selo exige que esse caso passe (`bin/tests/test_etapas_no_brief.sh:207`, `bin/tests/test_etapas_no_brief.sh:210`).

O percentual exclui SEM-SNAPSHOT do denominador `finais`, enquanto seu contador separado não declara esperado 0 (`bin/disjuntor.py:2333`, `bin/disjuntor.py:2335`, `bin/disjuntor.py:2336`, `bin/disjuntor.py:2353`, `bin/disjuntor.py:2355`). Logo pode publicar 100% entre conferidos com finais escapando da conferência (`bin/disjuntor.py:2333`, `bin/disjuntor.py:2353`).

Conflita com a exigência literal I-3 fornecida de reprovar pouso sem cada etapa medida `[nao verificado: anexo não existe na cópia]` e com origem/escopo literal de `CLAUDE.md:17`, `CLAUDE.md:37`. **Nuance:** o cabeçalho histórico do próprio disjuntor ainda diz “FALHA ABERTO”; não afirmar que todo contrato dele exige fail-closed (`bin/disjuntor.py:34`). O B3 atual exige negar quando não sabe se a obra está parada (`bin/disjuntor.py:1937`, `bin/disjuntor.py:1941`, `bin/disjuntor.py:1245`).

Universo real de finais APROVADO vivos e sua contagem: `[nao verificado]`. Em código, universo = arquivos `agent-*.jsonl` no diretório de subagentes cujo meta seja verificador, primeiro prompt seja final, obra legível e último veredito APROVADO; cai no escape quando falta fotografia exata (`bin/disjuntor.py:847`, `bin/disjuntor.py:852`, `bin/disjuntor.py:855`, `bin/disjuntor.py:863`, `bin/disjuntor.py:917`).

10. **Briefs vivos.** O diretório `logs` não está disponível nesta cópia `[nao verificado]`; não se pode certificar lista/estado de fila. Lista da entrega é afirmação do construtor, não censo observado: `akmed/brief_v5`, `codexneg/brief_v4`, `esminst`, `painel_um`, `plantio_mais1`, `BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS/brief.md`, `FIO-E0-V4-E4-ESCRITORES/brief.md` `[nao verificado]`. Condição mecânica comprovada: se forem da fila 1 e sem ETAPAS, próxima abertura recusa; fora da fila 1 retorna antes do juiz (`bin/disjuntor.py:1853`, `bin/disjuntor.py:1869`, `bin/disjuntor.py:1871`, `bin/disjuntor.py:1887`).

## 09/11 12:02 · sessao 882c97d3b33a · - · - · origem=Ronald (tmux)

**2 — Juiz único.** A forma tem um juiz, `etapas_do_brief`, chamado para cada brief pela porta comum `_censo_nega` (`bin/disjuntor.py:1703`, `bin/disjuntor.py:1869`). CENSO e ETAPAS compartilham o recorte `_bloco_do_titulo`; seus validadores de conteúdo são distintos (`bin/disjuntor.py:1626`, `bin/disjuntor.py:1653`, `bin/disjuntor.py:1710`).

Há **derivação paralela no verificador**: sua instrução manda obter N do brief e n das citações no RELATO; B3 obtém N da última fotografia e calcula n por conjunto de K válidos, cruzando os dois valores (`.claude/agents/verificador.md:18`, `bin/disjuntor.py:874`, `bin/disjuntor.py:894`, `bin/disjuntor.py:928`). Um brief alterado pode produzir denominador diferente e reprovação integral (`bin/disjuntor.py:930`).

Stop, wrapper, ESTADO e handoff não implementam parser próprio de ETAPAS: delegam respectivamente a B3, `--censo`, `ler/linha` e ao reconhecedor de PAREI (`bin/hook_stop_fila1.py:1204`, `bin/codex_pergunta.sh:234`, `bin/gerar_estado.py:280`, `bin/handoff_sessao.sh:130`). Juiz registrado de negócio duplicado por esta mudança: `[nao verificado]`.

**6 — ESMERIL-SEMANTICA.** Há dois motivos sob a mesma chave: `NEG_CENSO` afirma literalmente “sem CENSO”, inclusive quando só ETAPAS falha; ambos escrevem `PAREI: censo-<obra> | espera Codex` (`bin/disjuntor.py:1612`, `bin/disjuntor.py:1616`, `bin/disjuntor.py:1887`). O medidor lê essa negativa como censo e publica “aberturas negadas por censo”/horas esperando censo (`bin/disjuntor.py:2261`, `bin/disjuntor.py:2308`, `bin/disjuntor.py:2371`). Stop/handoff reconhecem apenas o mesmo literal e a existência de wrapper vivo com papel censo/leitura; não distinguem o defeito original (`bin/hook_stop_fila1.py:730`, `bin/hook_stop_fila1.py:763`, `bin/hook_stop_fila1.py:773`, `bin/handoff_sessao.sh:130`).

**8 — Escritores e leitores.**

| Estado/linha | Escritor | Leitor |
|---|---|---|
| `logs/sessao/disjuntor.log` | `trilha`, append, carimbo interno e detalhe limitado a 300 caracteres: `bin/disjuntor.py:203` | Snapshot: `bin/disjuntor.py:879`; medidas censo/ETAPAS: `bin/disjuntor.py:2249`, `bin/disjuntor.py:2322` |
| `ETAPAS SNAPSHOT` | Wrapper: `bin/disjuntor.py:1882`; Agent, depois de passar demais verificações: `bin/disjuntor.py:1967` | Última linha com obra exatamente igual: `bin/disjuntor.py:881` |
| `ETAPAS PROVADAS/METADE/SEM-SNAPSHOT` | Preparadas em `bin/disjuntor.py:919`, `bin/disjuntor.py:937`, `bin/disjuntor.py:939`; anunciadas após salvar estado: `bin/disjuntor.py:1147` | `_etapas_24h`: `bin/disjuntor.py:2324` |
| `est['etapas'][aid]` | `_confere_etapas`: `bin/disjuntor.py:910`, `bin/disjuntor.py:918`, `bin/disjuntor.py:931` | Memo impede nova conferência: `bin/disjuntor.py:913` |
| `b3[obra]['metade'][aid]` | Integração na fila já existente: `bin/disjuntor.py:1096` | Mensagem do disparo: `bin/disjuntor.py:1113` |
| `ETAPAS: n/N` no handback | Agente instruído por `.claude/agents/verificador.md:18`; execução real `[nao verificado]` | Extração de handback: `bin/disjuntor.py:670`; parser/cruzamento: `bin/disjuntor.py:928` |
| `ETAPA <obra> K: ... -> ...` no RELATO | Escritor concreto da sessão/linhas reais `[nao verificado]` | Leitura só de RELATO vivo: `bin/disjuntor.py:892` |
| `PAREI censo` | `_parei_censo_no_relato` chama porta canônica `no_topo`: `bin/disjuntor.py:334`; a porta usa trava do diretório: `app/core/esteira_vigia.py:846`, `app/core/esteira_vigia.py:892` | Stop: `bin/hook_stop_fila1.py:773`; handoff: `bin/handoff_sessao.sh:130`; alarme: `bin/alarme_sessao_ociosa.py:137` |
| `ler()['etapas']` / texto ESTADO | Medidor e composição: `bin/disjuntor.py:2204`, `bin/disjuntor.py:2348`, `bin/disjuntor.py:2447` | `gerar_estado` chama `ler/linha`, inclui `dj_txt`, grava destino: `bin/gerar_estado.py:280`, `bin/gerar_estado.py:378`, `bin/gerar_estado.py:390` |

O memo/metade passa pela trava única do estado, gravação atômica e anúncio ainda sob trava (`bin/disjuntor.py:357`, `bin/disjuntor.py:376`, `bin/disjuntor.py:382`, `bin/disjuntor.py:392`). Não encontrei segundo escritor de máquina introduzido para RELATO; as negativas reutilizam `no_relato`/`no_topo` (`bin/disjuntor.py:304`, `bin/disjuntor.py:334`).

**9 — Classes presentes.**

- **Derivação paralela:** verificador calcula N do brief atual enquanto B3 lê fotografia (`.claude/agents/verificador.md:18`, `bin/disjuntor.py:916`).
- **Campo com dois significados:** negativa de ETAPAS contabilizada/rotulada como CENSO (`bin/disjuntor.py:1887`, `bin/disjuntor.py:2261`, `bin/disjuntor.py:2371`).
- **Ordem de trava/snapshot antes da abertura efetiva:** wrapper grava snapshot dentro de `--censo` antes de tentar prender a vaga; vaga ocupada ou HEAD inválido recusam depois. Assim uma tentativa sem abertura pode substituir o denominador vigente (`bin/disjuntor.py:1882`, `bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:239`, `bin/codex_pergunta.sh:241`, `bin/disjuntor.py:883`).
- **Fail-open na persistência:** `trilha` engole falha e não informa sucesso; a porta retorna aprovação mesmo que não tenha gravado snapshot; ausência depois passa como SEM-SNAPSHOT (`bin/disjuntor.py:210`, `bin/disjuntor.py:1882`, `bin/disjuntor.py:1886`, `bin/disjuntor.py:917`).
- **Pouso antes da conferência:** Bash principal sem `codex_pergunta` sai antes de B3; B3 é consultado em próximas chamadas de verificador ou Stop. Um merge/deploy Bash após o handback pode ocorrer antes dessa consulta (`bin/disjuntor.py:1977`, `bin/disjuntor.py:1989`, `bin/disjuntor.py:2005`, `bin/hook_stop_fila1.py:1204`). O wrapper chama B3 sem transcript, portanto não descobre sozinho um novo handback opus (`bin/disjuntor.py:1238`, `bin/disjuntor.py:1241`, `bin/codex_pergunta.sh:378`).
- **Relógio fora da borda:** `_etapas_24h` nova recebe `agora` e deriva a janela sem ler relógio (`bin/disjuntor.py:2315`, `bin/disjuntor.py:2319`). `trilha` lê `datetime.now` internamente e `decide_b3` tem fallback `time.time`, ambos preexistentes pelo diff fornecido (`bin/disjuntor.py:208`, `bin/disjuntor.py:1067`). Violação nova de relógio independente: `[nao verificado]`.

## 09/11 12:02 · sessao 57a45d0fa874 · - · - · origem=Ronald (tmux)

Leitura estática confirma asserções concretas para os 20 mutantes; execução/20 mortos permanece `[nao verificado]`, pois o selo cria, altera e apaga arquivos (`bin/tests/test_etapas_no_brief.sh:23`, `:294`, `:301`).

| Mutantes | Asserção que deve matar |
|---|---|
| e1-sem-chamada; e1-sem-sequencia; e1-ruim-some; e1-resultado-vazio | RC das formas inválidas, sequência, última etapa inválida e resultado vazio (`bin/tests/test_etapas_no_brief.sh:99`, `:104`, `:116`, `:101`). |
| e2-sem-foto-wrapper; e2-sem-foto-agent | Snapshot presente e anterior ao APROVADO (`bin/tests/test_etapas_no_brief.sh:93`, `:139`). |
| e2-b3-le-o-brief; e2-primeira-foto | Brief editado mantém N=3; reabertura muda para N=5 (`bin/tests/test_etapas_no_brief.sh:232`, `:237`). |
| e3-metade-passa; e3-so-o-verificador; e3-so-o-relato | Negação efetiva no próximo verificador e disparo esperado; RELATO inferior e ausência da linha do verificador (`bin/tests/test_etapas_no_brief.sh:167`, `:180`, `:192`, `:196`). |
| e3-copiada-conta | Expectativa copiada não completa etapa (`bin/tests/test_etapas_no_brief.sh:200`, `:203`). |
| e3-sem-memo; e3-sem-snapshot-sem-memo | Exatamente um evento em chamadas repetidas (`bin/tests/test_etapas_no_brief.sh:188`, `:214`). |
| e3-ilegivel-livre | RELATO ilegível precisa produzir deny e depois permitir nova conferência (`bin/tests/test_etapas_no_brief.sh:220`, `:222`). |
| e3-sem-aviso-de-metade | Texto literal do disparo (`bin/tests/test_etapas_no_brief.sh:180`). |
| e4-finais-nao-contam; e4-sem-janela | Dict exato, incluindo exclusão de evento velho/futuro (`bin/tests/test_etapas_no_brief.sh:252`, `:253`, `:257`). |
| e4-fora-da-linha; e4-ilegivel-zero | Texto em `--linha`/ESTADO e resultado None (`bin/tests/test_etapas_no_brief.sh:260`, `:265`, `:272`). |

A mordida tem limite: o harness aceita **qualquer RC diferente de zero**, sem conferir se caiu pela asserção pretendida; a mensagem FALHA apenas é impressa (`bin/tests/test_etapas_no_brief.sh:304`, `:307`). Existe controle sem mutação (`bin/tests/test_etapas_no_brief.sh:289`).

Casos pedidos:

- Aceitar bloco vazio ou etapa sem RESULTADO é detectado pelas asserções existentes (`bin/tests/test_etapas_no_brief.sh:107`, `:100`, `:116`).
- Contar obra por prefixo deve ser detectado: o caso inclui `OBRA-E3X` como única outra testemunha de K=3; a comparação atual é igualdade (`bin/tests/test_etapas_no_brief.sh:201`, `:203`; `bin/disjuntor.py:897`).
- **n>N já passa no código atual**: provei em memória N=3, RELATO 3 etapas e `ETAPAS: 4/3` → PROVADAS; não há guarda n≤N (`bin/disjuntor.py:929`, `:930`, `:939`). O selo não apresenta caso n>N (`bin/tests/test_etapas_no_brief.sh:178`, `:207`).
- Mutante simples sobrevivente por inspeção: substituir `_ALNUM_RE.search(med)` por `True`, preservando o restante da condição. Aceita resultado vazio; todas as linhas candidatas usadas pelo teste têm resultado não vazio, sem caso dedicado ao vazio no RELATO (`bin/disjuntor.py:900`; `bin/tests/test_etapas_no_brief.sh:162`, `:200`, `:222`). Execução desse mutante: `[nao verificado]`.
- “Medida” é filtro lexical: `pendente`, `nao medido` e `**RESULTADO OBSERVAVEL rc 0**` contam; confirmei em memória resultado 3. Só se exige ASCII alfanumérico e ausência do prefixo literal sem Markdown (`bin/disjuntor.py:896`, `:900`, `:1700`).
- Snapshot posterior substitui o anterior: leitor toma último N da obra, sem vincular abertura/aid ou verificar hash (`bin/disjuntor.py:869`, `:879`, `:883`). O teste cobre reabertura sequencial, não final antigo após nova abertura concorrente (`bin/tests/test_etapas_no_brief.sh:229`, `:237`).
- Hash registrado não fixa semanticamente as etapas: leitor extrai somente N; alteração com o mesmo N não é comparada ao snapshot (`bin/disjuntor.py:869`, `:883`, `:916`).

## 09/11 11:55 · sessao ccfc3330625e · O10-D-PERMUTA-RECIPROCA · censo · origem=Code

**O ponto novo decisivo: duas chamadas cabem no mesmo `atomic`, mas o rollback integral não vem automaticamente.** A porta captura sua recusa e retorna `'recusada'`; se o compositor deixar o `atomic` externo terminar normalmente, a primeira troca pode ser confirmada (`app/ponto/portas/celula.py:652`; `app/ponto/portas/celula.py:694`).

1. **Duas chamadas no mesmo `atomic`**

   **(a) Travas e espera.** Cada chamada trava suas duas cartas por `pk`, exclusivamente nas linhas de `Ausencia`; depois percorre suas células por `(colaborador_id, data)`, adquirindo a trava dentro de `editar_dia` (`app/ponto/portas/celula.py:657`; `app/ponto/portas/celula.py:673`; `app/ponto/portas/celula.py:593`).

   Essa ordem é **por chamada, não pelas quatro células**. Para A<B, a sequência composta pode ser `A/d1 → B/d1 → A/d2 → B/d2`. Duas composições concorrentes começando pelos pares opostos podem adquirir conjuntos distintos e depois esperar circularmente pelas cartas/células restantes — dedução estática dessas aquisições; deadlock efetivamente ocorrido: **[nao verificado]** (`app/ponto/portas/celula.py:657`; `app/ponto/portas/celula.py:673`).

   Dentro da mesma transação, a segunda chamada não precisa aguardar a própria transação liberar suas travas; espera causada por outra transação continua possível. Ensaio concorrente dessa composição: **[nao verificado]**; o teste existente inspeciona AST, sem executar essa concorrência (`app/ponto/tests/test_o10b_editar_dia_troca.py:314`).

   **(b) Lavra.** Cada edição alcançada consulta novamente a empresa atual e as competências lavradas, **antes inclusive de decidir `'igual'`**. Portanto, são até quatro avaliações, interrompidas na primeira recusa; `apesar_da_lavra` pula a guarda (`app/ponto/portas/celula.py:513`; `app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:676`).

   Isso não constitui uma pré-validação das quatro: a consulta do lado seguinte ocorre depois da escrita do anterior. Tampouco há trava dos registros de exportação nessa consulta; o TOCTOU já aparece na fila documental (`app/ponto/portas/celula.py:675`; `app/ponto/management/commands/gerar_celulas.py:161`; `app/docs/BACKLOG.md:1876`).

   **(c) Carimbos e efeitos.** Cada célula alterada recebe seu próprio `timezone.now()`, autor e trilha; não nasce um carimbo comum de permuta. Com quatro alterações, o caminho grava quatro trilhas `editar_dia` e duas `trocar_plantao`, sem uma identificação comum (`app/ponto/portas/celula.py:598`; `app/ponto/portas/celula.py:611`; `app/ponto/portas/celula.py:686`).

   | Efeito | Resultado da composição |
   |---|---|
   | Recálculo | A deduplicação por competência é local a cada execução do efeito posterior. Com quatro alterações e sem falhas nos trechos protegidos por `try/except`, são quatro agendamentos diretos; se d1/d2 pertencem à mesma competência, cada colaborador/competência pode ser recalculado duas vezes. Não há chave de cancelamento ou consolidação entre chamadas (`app/ponto/portas/celula.py:450`; `app/ponto/portas/celula.py:458`; `app/ponto/portas/celula.py:627`; `app/ponto/services/fechamento.py:1824`). |
   | Rejunção e reavaliação | Cada alteração agenda um callback contendo ambas. A rejunção repete a busca por colaborador, mas só alcança FK nula; a reavaliação recorta os dias e chamados vivos. Repetição de execução não significa necessariamente repetição de escrita (`app/ponto/portas/celula.py:490`; `app/ponto/portas/celula.py:502`; `app/chamados/rejuncao.py:60`; `app/chamados/services/feriado_reavaliar.py:17`; `app/chamados/services/feriado_reavaliar.py:76`). |
   | Rejulgamento | É síncrono, após cada edição; pode observar a composição ainda incompleta. Pula futuro/vereditos humanos e captura falhas. Não há um rejulgamento final agregado das quatro células nesse caminho (`app/ponto/portas/celula.py:427`; `app/ponto/portas/celula.py:434`; `app/ponto/portas/celula.py:437`; `app/ponto/portas/celula.py:627`). |
   | Cobrança órfã | É marcada dentro da transação da edição, por pessoa/dia, consultando os carimbos recém-escritos. O marcador atualiza contexto, não cancela os outros efeitos da composição (`app/ponto/portas/celula.py:625`; `app/ponto/management/commands/gerar_celulas.py:241`; `app/ponto/management/commands/gerar_celulas.py:255`). |

   Colisões concretas entre os efeitos transitivos das quatro células: **[nao verificado]**.

   **(d) Visibilidade e cache.** A segunda chamada usa os argumentos apenas para obter os `pk` e relê cartas e colaboradores; cada edição relê sua célula com trava e carrega novamente o vínculo. Pelo caminho de leitura, encontra as escritas anteriores da mesma transação, sem reutilizar as instâncias da primeira chamada (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:657`; `app/ponto/portas/celula.py:593`; `app/escala/alimentacao.py:355`).

   O cartório também relê a célula. O identity map encontrado pertence às instâncias explicitamente incluídas em `celulas_do_ato` e é restaurado ao sair; não é um cache global das duas trocas. Objetos ou mapas que um futuro compositor mantenha por conta própria: **[nao verificado]** (`app/ponto/services/cartorio.py:1976`; `app/escala/alimentacao.py:43`; `app/escala/alimentacao.py:47`).

   **(e) `'igual'`.** Um lado igual não impede a edição do outro: a porta visita ambos e só retorna `'igual'` quando ambos o são. Um lado igual preserva carimbo e não produz sua trilha individual nem seus efeitos; se o outro mudar, a trilha do par registra os dois resultados (`app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:596`; `app/ponto/portas/celula.py:673`; `app/ponto/portas/celula.py:679`; `app/ponto/portas/celula.py:690`).

   Essa igualdade compara estado da célula, não pedido/cartas/motivo/autor novos. Quatro células distintas não compartilham essa chave; reutilizar as mesmas duas células em sentido inverso pode reescrevê-las novamente (`app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:675`).

   **(f) Rollback.** Há dois resultados diferentes:

   - **Segunda retorna `'recusada'`, externo termina normalmente:** o savepoint da segunda chamada é desfeito; a primeira permanece elegível ao commit, inclusive suas trilhas e callbacks (`app/ponto/portas/celula.py:652`; `app/ponto/portas/celula.py:694`).
   - **Compositor provoca rollback externo:** desfaz as escritas transacionais de ambas, incluindo auditoria, marca órfã e gravações síncronas do rejulgamento. O savepoint de auditoria não é commit independente; os callbacks registrados por `on_commit` dependem do commit externo (`app/core/observ.py:133`; `app/ponto/portas/celula.py:625`; `app/ponto/portas/celula.py:434`; `app/ponto/portas/celula.py:502`; `app/ponto/services/fechamento.py:1802`).

   **Há um limite adicional ao rollback:** o rejulgamento pode alcançar abertura de cobrança e envio imediato de push de supervisão, antes do commit. Rollback SQL não recolhe uma notificação já enviada; envio concreto nesse cenário: **[nao verificado]** (`app/ponto/services/cartorio.py:1740`; `app/ponto/management/commands/detectar_ausencias.py:778`; `app/chamados/push_utils.py:40`).

2. **Validação antes da primeira escrita**

   | Controle | Reutilização sem gravar |
   |---|---|
   | Interjornada de 11h | Validador pronto localizado: **[nao verificado]**. O helper de disponibilidade declara expressamente que não valida 11h (`app/ponto/utils.py:152`). |
   | Sobreposição de turnos | Juiz pronto da colisão geométrica da permuta: **[nao verificado]**. `marcos_turno` fornece datetimes crescentes, incluindo meia-noite, e `fim_turno` fornece o término; são insumos, não o veredito (`app/escala/servico_jornada.py:61`; `app/escala/servico_jornada.py:89`). |
   | Permissão | `tem_acao(user, *acoes)` responde por ação sem gravação; `pode_agir_chamado` responde pelo departamento dono do chamado. A ação específica e a autorização contextual das quatro células: **[nao verificado]** (`app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68`). |
   | Competência exportada/lavrada | `competencia_fechada_para_celula` já é juiz separado, registrado como K9, retornando bool ou fontes. `_competencias_exportadas` enumera suas janelas e `autoridade_da_lavra` fornece a frase da recusa, sem gravar (`app/folha/export.py:589`; `app/core/juizes.py:322`; `app/ponto/management/commands/gerar_celulas.py:126`; `app/ponto/management/commands/gerar_celulas.py:208`). |

   Não há, entre esses quatro controles, um predicado existente exclusivamente preso à escrita que precise ser extraído: a lavra já está separada; permissão existe fora, mas não é chamada pelas portas; 11h e colisão não aparecem como guardas nelas (`app/ponto/portas/celula.py:559`; `app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:675`).

   `ausencias_sobrepostas` responde sobre períodos de **Ausencia**, não sobre colisão horária dos plantões; usá-la como resposta à segunda checagem mudaria a pergunta (`app/ponto/turnos.py:2795`).

3. **ID próprio na trilha**

   `evento` não possui argumento `correlacao`, `lote` ou `grupo`; aceita `antes/depois`, persistidos nos JSONs da auditoria. `LogAuditoria` tem um `objeto_id` inteiro e nenhuma coluna específica de correlação. Portanto, há espaço técnico no JSON para o identificador, quatro células e pedido (`app/core/observ.py:94`; `app/core/observ.py:133`; `app/core/models.py:298`; `app/core/models.py:300`).

   Atenção ao adaptador: passar simplesmente `correlacao=...` a `evento_kw` não funciona, pois ele só encaminha os campos conhecidos (`app/core/observ.py:142`).

   Existe precedente de metadado `lote` dentro do JSON, mas seu valor é o rótulo fixo `'arquivar_lote'`, sem identificar exclusivamente cada execução. Padrão existente que já ligue quatro células e pedido de permuta: **[nao verificado]** (`app/chamados/views_arquivar.py:146`; `app/chamados/services/arquivo.py:133`).

   Hoje `trocar_plantao` não recebe esse ID: audita a carta cedente como objeto e dois lados no JSON. `Ausencia.chamado_origem` pode apontar para o pedido, mas não é copiado para essa trilha (`app/ponto/portas/celula.py:632`; `app/ponto/portas/celula.py:682`; `app/ponto/portas/celula.py:686`; `app/ponto/models.py:786`).

4. **Duas cartas por lado**

   Para representar literalmente quatro coordenadas distintas pela porta atual, a composição precisa destas quatro `Ausencia`; pessoa/data vêm exclusivamente das cartas relidas (`app/ponto/portas/celula.py:659`; `app/ponto/portas/celula.py:673`):

   | Chamada | Cedente | Substituto |
   |---|---|---|
   | Primeira | A/d1 | B/d1 |
   | Segunda | B/d2 | A/d2 |

   São quatro registros, mas o código não exige quatro arquivos físicos diferentes. Cada chamada exige argumentos presentes, registros encontrados, colaboradores distintos e `documento` preenchido; não compara nomes nem abre os arquivos (`app/ponto/portas/celula.py:653`; `app/ponto/portas/celula.py:660`; `app/ponto/portas/celula.py:662`; `app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:684`).

   Não há consumo ou exclusividade global da carta entre chamadas. Reutilizar as mesmas duas invertidas volta às mesmas duas células, em vez de produzir as quatro; a porta também não exige igualdade de datas dentro de cada chamada (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:673`).

   Tipo, status, aprovação, período e vínculo com pedido não integram a elegibilidade documental verificada pela troca; lavra e vínculo do dia são avaliados separadamente por `editar_dia` (`app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:675`; `app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:584`).

A referência documental diverge: nesta cópia, `BACKLOG.md:1879` trata `BO-0511-G2-TRIAGEM`; a correção mencionada no aval não está nessa linha (`app/docs/BACKLOG.md:1879`).

Do censo v1, ficam marcadas como **respondidas pelo aval fornecido**: **v1.1**, quanto à exigência de validar 11h; **v1.2**, quanto à validação impeditiva de sobreposição; **v1.4**, quanto à unidade de quatro células — que exige duas datas distintas para A/B. Numeração física do aval na cópia: **[nao verificado]**.

perguntas de lei: 4

1. **v1.1 — residual:** quais turnos entram na checagem das 11h, inclusive no retroativo? Esse universo não foi explicitado no aval fornecido; juiz específico localizado: **[nao verificado]** (`app/ponto/utils.py:152`).
2. **v1.3 — não respondida:** há restrição entre empresas, vínculos ou tipos de escala diferentes? **[nao verificado]**; a porta consulta vínculo individual, sem comparar esses atributos (`app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:673`).
3. **v1.5 — não respondida:** quais condições de tipo, status e período tornam as quatro cartas elegíveis? **[nao verificado]**; a exigência documental atual é apenas presença de `documento` (`app/ponto/portas/celula.py:664`).
4. **v1.6 — não respondida:** eventual resultado `folga_trabalhada` deve pedir decisão do admin? Continua registrada como pergunta aberta (`app/docs/RELATO.md:449`; `app/docs/BACKLOG.md:1876`).

## 09/11 11:53 · sessao 1d0e367e27e2 · - · - · origem=Ronald (tmux)

Complemento:

- **O cache encontrado não reaproveita automaticamente a fotografia da primeira troca.** `julgar_celula` relê a célula e cria contadores novos; seus caches de regra/veto/afastamento são locais, e o `finally` restaura ou limpa o estado da thread (`app/ponto/services/cartorio.py:1976`, `app/ponto/services/cartorio.py:1988`, `app/ponto/services/cartorio.py:1138`, `app/ponto/services/cartorio.py:1274`, `app/ponto/services/cartorio.py:1296`, `app/ponto/services/cartorio.py:1074`). O identity map de EC é atributo das instâncias explicitamente envolvidas em `celulas_do_ato`, instalado/restaurado pelo contexto; os carregadores usados pela edição e pelo cartório fazem queries novas (`app/escala/models.py:1063`, `app/escala/alimentacao.py:43`, `app/escala/alimentacao.py:47`, `app/escala/alimentacao.py:355`, `app/escala/alimentacao.py:148`, `app/escala/servico_jornada.py:46`). Cache de objetos mantidos por um futuro compositor: **[nao verificado]**.

- **Há efeito externo possível antes do commit: push de supervisão.** O rejulgamento síncrono pode alcançar `_abrir_chamado`, cujo caminho chama `push_supervisao` imediatamente; este chega a `webpush`, sem `on_commit` nesse encadeamento (`app/ponto/portas/celula.py:434`, `app/ponto/services/cartorio.py:1740`, `app/ponto/management/commands/detectar_ausencias.py:774`, `app/chamados/push_utils.py:89`, `app/chamados/push_utils.py:110`, `app/chamados/push_utils.py:40`). Portanto, rollback das escritas SQL não promete recolher uma notificação eventualmente enviada; ocorrência real, destinatários e subscriptions: **[nao verificado]**.

## 09/11 11:52 · sessao 1d0e367e27e2 · - · - · origem=Ronald (tmux)

Novidades para consolidação, por leitura estática:

- **Recusa retornada pela 2ª chamada não desfaz automaticamente a 1ª.** `trocar_plantao` abre seu `atomic`, lança `_TrocaRecusada` internamente, captura após sair dele e retorna `'recusada'`; portanto, um `atomic` externo que termine normalmente preserva a primeira troca. O compositor precisa tratar a recusa fazendo o rollback externo; exceções não capturadas, como falha obrigatória da trilha, já propagam (`app/ponto/portas/celula.py:652`, `app/ponto/portas/celula.py:676`, `app/ponto/portas/celula.py:692`, `app/ponto/portas/celula.py:694`). Ensaio específico com duas trocas externas: **[nao verificado]**.

- **A ordenação atual é por par, não pelas quatro células/cartas.** Cada chamada trava cartas por `pk`, depois edita células por `(colaborador_id, data)`; o lock da célula ocorre dentro de `editar_dia` (`app/ponto/portas/celula.py:656`, `app/ponto/portas/celula.py:657`, `app/ponto/portas/celula.py:673`, `app/ponto/portas/celula.py:593`). Com A<B, a composição faz A/d1,B/d1,A/d2,B/d2: A/d2 vem depois de B/d1, contrariando uma ordenação global das quatro chaves; duas composições concorrentes que iniciem pelos pares opostos podem esperar circularmente pelos lotes de cartas/células — dedução das duas aquisições locais (`app/ponto/portas/celula.py:657`, `app/ponto/portas/celula.py:673`). Deadlock efetivamente ocorrido em produção: **[nao verificado]**. O teste de concorrência declarado consulta AST, e o teste de ordem mede apenas uma troca; não demonstram concorrência real da composição de quatro células (`app/ponto/tests/test_o10b_editar_dia_troca.py:314`, `app/ponto/tests/test_o10b_editar_dia_troca.py:442`).

- **Lavra é reconsultada em cada `editar_dia`, inclusive antes de decidir `'igual'`.** A empresa vem de query própria; `_competencias_exportadas` consulta os candidatos e o juiz, sem cache compartilhado entre chamadas; `apesar_da_lavra` pula a guarda (`app/ponto/portas/celula.py:513`, `app/ponto/portas/celula.py:568`, `app/ponto/management/commands/gerar_celulas.py:154`, `app/ponto/management/commands/gerar_celulas.py:161`, `app/ponto/management/commands/gerar_celulas.py:173`, `app/ponto/portas/celula.py:594`). Não há lock de exportação nessa consulta, nem validação antecipada das quatro células no corpo atual (`app/ponto/management/commands/gerar_celulas.py:161`, `app/ponto/portas/celula.py:675`).

- **A 2ª chamada relê instâncias.** Os objetos `Ausencia` recebidos servem para extrair `pk`; as cartas e seus colaboradores são recarregados, e a célula é relida com trava em cada edição (`app/ponto/portas/celula.py:656`, `app/ponto/portas/celula.py:657`, `app/ponto/portas/celula.py:593`). O vínculo também é alimentado novamente por queries de células e escalas (`app/escala/alimentacao.py:316`, `app/escala/alimentacao.py:329`, `app/escala/alimentacao.py:355`, `app/escala/alimentacao.py:357`; query de células em `app/escala/alimentacao.py:73`). Assim, não há reaproveitamento das instâncias anteriores nesse caminho; visibilidade concreta em execução: **[nao verificado]**.

- **`'igual'` é por célula e não aborta o outro lado.** A igualdade exige origem editada, mesmo `trabalha`, DNA e geradora; ignora motivo/autor novos, preserva carimbo e retorna antes da trilha, marca órfã e efeitos posteriores (`app/ponto/portas/celula.py:594`, `app/ponto/portas/celula.py:596`, `app/ponto/portas/celula.py:598`, `app/ponto/portas/celula.py:611`, `app/ponto/portas/celula.py:625`, `app/ponto/portas/celula.py:627`). A troca só retorna `'igual'` após visitar ambos; com apenas um lado igual, grava a trilha do par incluindo o resultado de cada lado (`app/ponto/portas/celula.py:673`, `app/ponto/portas/celula.py:679`, `app/ponto/portas/celula.py:683`, `app/ponto/portas/celula.py:690`). As quatro células distintas não compartilham essa chave de igualdade; reaproveitar a mesma pessoa/data com sentido inverso altera novamente `trabalha`, não vira no-op (`app/ponto/portas/celula.py:594`, `app/ponto/portas/celula.py:675`).

- **Carimbos são independentes por edição.** Cada célula alterada recebe seu próprio `timezone.now`; não existe carimbo comum da permuta; `gerada_em` permanece o de criação, e a edição só salva os seis campos indicados (`app/ponto/portas/celula.py:598`, `app/ponto/portas/celula.py:609`, `app/escala/models.py:1702`). Coincidência temporal concreta: **[nao verificado]**.

- **Não há deduplicação entre chamadas para os efeitos.** A deduplicação financeira `_por_competencia` é uma variável local de `_o_chao_do_dia_mudou`; `editar_dia` chama esse rabo separadamente, com um dia (`app/ponto/portas/celula.py:450`, `app/ponto/portas/celula.py:456`, `app/ponto/portas/celula.py:627`). Se as quatro células mudarem, o caminho direto agenda quatro recálculos e quatro callbacks de rejunção+reavaliação; se d1/d2 caírem na mesma competência, o mesmo colaborador/competência é recalculado duas vezes. Não há cancelamento ou chave de coalescência nesse registro (`app/ponto/portas/celula.py:458`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1824`, `app/ponto/portas/celula.py:490`, `app/ponto/portas/celula.py:502`). Contagem total incluindo signals e efeitos transitivos: **[nao verificado]**.

- **Rejunção repetida é limitada por FK nula; reavaliação, por chamados vivos.** A rejunção procura `celula__isnull=True`; a reavaliação usa `universo` com `status_local__in=VIVOS`, recorta os dias e encerra somente os considerados espúrios pelo juiz (`app/chamados/rejuncao.py:60`, `app/chamados/rejuncao.py:98`, `app/chamados/services/feriado_reavaliar.py:17`, `app/chamados/services/feriado_reavaliar.py:33`, `app/chamados/services/feriado_reavaliar.py:76`, `app/chamados/services/feriado_reavaliar.py:82`). Portanto, são consultas repetidas, sem equivaler necessariamente a novas escritas; colisões concretas: **[nao verificado]**.

- **Rejulgamento acontece antes do commit externo e vê estados intermediários da composição.** Cada edição chama o juiz imediatamente após escrever sua célula; pula futuro e vereditos humanos, e engole falhas (`app/ponto/portas/celula.py:627`, `app/ponto/portas/celula.py:427`, `app/ponto/portas/celula.py:430`, `app/ponto/portas/celula.py:434`, `app/ponto/portas/celula.py:437`). O caminho direto não adia os quatro rejulgamentos para o estado final; eventual interferência entre seus resultados: **[nao verificado]**.

- **Cobrança órfã é marcada na transação, separadamente por célula/dia.** O marcador lê os carimbos recém-escritos, filtra cobranças vivas e grava `orfa_regeneracao` com instante próprio e marcos de emissão, usando a porta de contexto (`app/ponto/portas/celula.py:625`, `app/ponto/management/commands/gerar_celulas.py:241`, `app/ponto/management/commands/gerar_celulas.py:243`, `app/ponto/management/commands/gerar_celulas.py:255`, `app/chamados/services/contexto.py:101`). Quatro células distintas não sobrescrevem diretamente o mesmo carimbo de célula; alcance concreto dos chamados associados: **[nao verificado]**.

- **Rollback externo efetivo inclui trilhas e escritas dos efeitos imediatos, mas os savepoints não são commits independentes.** Auditoria usa outro `transaction.atomic` e cria o `LogAuditoria` dentro dele; falta de trilha causa `RuntimeError` na edição/troca (`app/core/observ.py:124`, `app/core/observ.py:133`, `app/ponto/portas/celula.py:619`, `app/ponto/portas/celula.py:692`). Os efeitos posteriores estão registrados em `on_commit`, não executados diretamente nessa parte do caminho (`app/ponto/services/fechamento.py:1824`, `app/ponto/portas/celula.py:502`). Se o compositor realmente desfizer o atomic externo, a intenção documentada é que callbacks só rodem com fato gravado (`app/ponto/services/fechamento.py:1802`); descarte efetivo da fila por Django em execução: **[nao verificado]**. Mensagens de log e contadores em memória não são gravações transacionais e podem permanecer após falha (`app/core/observ.py:73`, `app/ponto/portas/celula.py:439`).

## 09/11 11:52 · sessao d8d8f4da4ae5 · - · - · origem=Ronald (tmux)

3) **Correlação disponível por JSON; nenhuma API própria de correlação.**

- `evento` aceita `acao/modelo/objeto_id`, descrição, `antes/depois`, ator e IP; não tem `correlacao/lote/grupo`. Os JSONs são gravados sem transformação em `valor_antes/valor_depois` (`app/core/observ.py:94`; `app/core/observ.py:133`). `evento_kw` encaminha somente os campos conhecidos: passar um kw adicional de correlação não o propaga (`app/core/observ.py:142`).
- `LogAuditoria` possui um único `objeto_id` inteiro e dois JSONs; não possui coluna própria para correlação (`app/core/models.py:295`; `app/core/models.py:298`; `app/core/models.py:300`; `app/core/models.py:307`). Assim, há lugar técnico para incluir um identificador e as quatro chaves de célula + pedido dentro de `depois`; contrato/padrão específico já existente para permuta: **[nao verificado]** (`app/core/observ.py:136`).
- Há precedente de **metadado livre de lote**: `extra={'lote': 'arquivar_lote', ...}` é acrescentado ao JSON de trilha junto a `chamado_pk/colaborador_id`; esse `lote` é rótulo fixo, não identificador exclusivo de cada execução (`app/chamados/views_arquivar.py:146`; `app/chamados/services/arquivo.py:130`; `app/chamados/services/arquivo.py:133`).
- Há também precedente de **objeto agrupador**: eventos de holerite usam `modelo='LoteHolerite'` e `objeto_id=lote_id` (`app/holerite/management/commands/processar_lote_holerite.py:148`).
- Hoje a troca não recebe nem propaga tal identificador: sua assinatura não o contém; registra `modelo='Ausencia'`, `objeto_id=a_c.pk` e JSON com dois lados, motivo e override, sem pedido/chamado/correlação (`app/ponto/portas/celula.py:632`; `app/ponto/portas/celula.py:682`; `app/ponto/portas/celula.py:686`). As trilhas individuais de edição também não recebem correlação (`app/ponto/portas/celula.py:611`).
- A `Ausencia.chamado_origem` já pode ligar cada carta a um chamado, mas `trocar_plantao` não lê nem copia esse campo para sua trilha (`app/ponto/models.py:786`; `app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:682`).

4) **Quatro células distintas exigem quatro registros de carta para a composição literal atual; não necessariamente quatro documentos físicos diferentes.**

| Chamada | Carta cedente | Carta substituto |
|---|---|---|
| Primeira | `Ausencia(A,d1)` | `Ausencia(B,d1)` |
| Segunda | `Ausencia(B,d2)` | `Ausencia(A,d2)` |

A correspondência decorre de a porta escolher pessoa/data exclusivamente de cada carta relida e editar essas duas coordenadas; não recebe pessoa/data alternativas (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:673`). Uma mesma `Ausencia` tem uma pessoa e uma `data`; reutilizá-la na segunda chamada não passa a representar outra data (`app/ponto/models.py:743`; `app/ponto/models.py:750`). Reusar as mesmas duas cartas invertidas é aceito pelo corpo, mas volta a editar as mesmas duas células, não constitui quatro coordenadas distintas (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:673`).

Para cada chamada, a porta exige:

- Argumentos não nulos, registros encontrados pelos `pk`, colaboradores distintos dentro do par e `documento` verdadeiro em ambos (`app/ponto/portas/celula.py:653`; `app/ponto/portas/celula.py:660`; `app/ponto/portas/celula.py:662`; `app/ponto/portas/celula.py:664`).
- Não compara nomes dos documentos nem abre os arquivos; o mesmo caminho de documento pode satisfazer mais de uma carta (`app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:684`). O helper de teste inclusive permite repetir seu default `o10b/carta.pdf` (`app/ponto/tests/test_o10b_editar_dia_troca.py:106`).
- Não testa tipo, status, aprovação, `data_fim`, horários parciais, `documento_referencia` ou `chamado_origem`; não valida existência física ou conteúdo do anexo (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:673`). Os campos existem no modelo (`app/ponto/models.py:749`; `app/ponto/models.py:751`; `app/ponto/models.py:768`; `app/ponto/models.py:786`; `app/ponto/models.py:791`; `app/ponto/models.py:799`).
- Vínculo do dia e lavra são checados por `editar_dia` para cada coordenada, independentemente da carta (`app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:675`).

**Perguntas antigas e aval:** o aval fornecido responde que insuficiência de 11h e sobreposição devem participar da validação das quatro células antes de escrever; não detalha universo de turnos, elegibilidade entre empresas/vínculos/escalas, tipo/status/período documental, nem a pergunta `folga_trabalhada` (**anexo `aval_reciproca.txt`, conteúdo fornecido no enunciado; arquivo físico não localizado [nao verificado]**). A pergunta de `folga_trabalhada` continua registrada literalmente como aberta na cópia (`app/docs/RELATO.md:449`; `app/docs/BACKLOG.md:1876`).

**Divergência documental relevante:** o aval fornecido diz “BACKLOG:1879 corrigido”, mas a linha 1879 desta cópia trata `BO-0511-G2-TRIAGEM`; não trata permuta (`app/docs/BACKLOG.md:1879`). A presença do aval novo em arquivo da cópia é **[nao verificado]**.

## 09/11 11:52 · sessao 299fbae42c6c · - · - · origem=Ronald (tmux)

Pré-checagens reutilizáveis encontradas:

| Checagem | Código sem escrita e limite |
|---|---|
| Competência exportada/lavrada | `competencia_fechada_para_celula(empresa_id, mes, ano, colaborador_id=None, fontes=False)` retorna bool ou fontes, combinando TXT vigente e holerite do próprio colaborador com lote publicado; executa consultas, sem gravação (`app/folha/export.py:589`; `app/folha/export.py:609`). Está registrado como K9 (`app/core/juizes.py:315`). |
| TXT entregue | `competencia_entregue` retorna bool ou registro vigente, excluindo o vigente invalidado; chama `anotar_vigencia`/`so_vigentes`, que consultam e anotam instâncias em memória, sem persistir (`app/folha/export.py:556`; `app/folha/export.py:580`; `app/folha/models.py:192`; `app/folha/models.py:203`). |
| Janela lavrada do dia | `_competencias_exportadas` enumera competências candidatas, pergunta K9 e converte em janelas; pode ser reutilizada antes das quatro escritas, como já faz `editar_dia` (`app/ponto/management/commands/gerar_celulas.py:126`; `app/ponto/management/commands/gerar_celulas.py:161`; `app/ponto/management/commands/gerar_celulas.py:172`; `app/ponto/portas/celula.py:568`). |
| Recusa falante da lavra | `autoridade_da_lavra` escolhe a competência intersectada e `fonte_da_lavra` traduz as fontes retornadas pelo mesmo K9; não gravam (`app/ponto/management/commands/gerar_celulas.py:183`; `app/ponto/management/commands/gerar_celulas.py:208`). |
| Permissão por ação | `tem_acao(user, *acoes)` retorna bool por autenticação, superuser e permissões `colaboradores.rbac_*`; não grava (`app/colaboradores/mixins.py:322`). `editar_escalas` já é ação usada nas views de escala; sua eleição específica para confirmar permuta: **[nao verificado]** (`app/escala/views.py:140`). |
| Permissão contextual de chamado | `pode_agir_chamado` retorna a permissão do departamento dono do chamado; é reutilizável para esse contexto, sem julgar os quatro dias nem o par (`app/chamados/utils.py:32`; `app/chamados/utils.py:52`; `app/chamados/utils.py:68`). |
| Interjornada 11h | Validador pronto localizado: **[nao verificado]**. O helper de disponibilidade declara expressamente que **não** valida descanso interjornada de 11h (`app/ponto/utils.py:141`; `app/ponto/utils.py:152`). |
| Sobreposição geométrica de turnos | Validador pronto do par localizado: **[nao verificado]**. Há componentes de geometria e leitura, abaixo, mas eles não retornam esse veredito (`app/escala/servico_jornada.py:61`; `app/core/juizes.py:195`). |

Componentes reaproveitáveis de geometria: `EC.eh_dia_trabalho` lê célula ou fallback do vínculo; `EC.marcos_do_dia` lê DNA ou template; `marcos_turno(..., ec=..., celulas=...)` encadeia marcos em datetimes crescentes, inclusive virada da meia-noite; `fim_turno` retorna o último marco. São insumos para calcular descanso e colisão, não um juiz que valide a permuta (`app/escala/models.py:1087`; `app/escala/models.py:1098`; `app/escala/models.py:1117`; `app/escala/servico_jornada.py:61`; `app/escala/servico_jornada.py:89`).

Cuidado com domínio: `ausencias_sobrepostas`, `veto_de_lancamento` e `checar_ausencia` são consultas/validações reutilizáveis de **Ausência/fato humano**, não sobreposição horária de plantões; `checar_ausencia` já é prévia explicitamente sem escrita (`app/ponto/turnos.py:2795`; `app/ponto/precedencia.py:430`; `app/ponto/services/checar_ausencia.py:15`).

No caminho de escrita, a lavra já é bloqueada por `editar_dia` antes do seu `atomic`; não existe necessidade de extrair o juiz da escrita, pois K9 e a enumeração de janelas já estão separados (`app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:592`; `app/folha/export.py:589`). Identificação do autor e motivo são guardas locais, não autorização; `trocar_plantao` apenas exige `autor.pk`/motivo e chama `editar_dia`, sem `tem_acao`/`pode_agir_chamado` (`app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:668`). Interjornada e colisão geométrica tampouco aparecem como guardas locais nessas portas; implementação pronta dessas duas verificações: **[nao verificado]** (`app/ponto/portas/celula.py:559`; `app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:673`).

`core/permissoes` nesta cópia: **[nao verificado]**; os endereços concretos de permissão encontrados são os acima (`app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68`).

## 09/11 11:50 · sessao d03007e09ff6 · RETRATO-POR-EVENTO · censo · origem=Code

O frescor atual pode deixar passar mudanças nas fontes do retrato: ele observa somente o carimbo do fechamento, enquanto o lavrador também lê cadastro, células, vínculos, férias, ausências, rubricas e o fio dos chamados. Porém, “escala e ausência não invalidam” não vale para todos os caminhos desta cópia: existem invalidações indiretas do fechamento. (`app/folha/retrato_folha.py:1298`; `app/escala/signals.py:124`; `app/ponto/services/ausencia.py:941`)

Execução atual dos crons, conteúdo atual da fila e custo atual de produção: **[nao verificado]**.

**1. Fontes do lavrador**

O caminho começa em `handle → montar_linhas → fora_da_folha → dias_que_seguram → classificar_export`; acrescenta `_causas_gravadas` e, para o dono do bloqueio, `alimentar_portas/porta_alcancavel_do_dia`. (`app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/retrato_folha.py:1157`; `app/folha/retrato_folha.py:1160`; `app/folha/fora_da_folha.py:119`; `app/folha/fora_da_folha.py:166`; `app/folha/fora_da_folha.py:178`)

O fecho inclui os seguintes caminhos transitivos:

- Retenção → célula → protestos → juiz dos períodos/pausas. (`app/folha/export.py:960`; `app/folha/export.py:1266`; `app/folha/export.py:1253`; `app/folha/export.py:1142`)
- Homologação → eventos do fechamento, ciclo do vínculo, régua CCT e datas de ausência. (`app/folha/export.py:994`; `app/folha/export.py:995`; `app/folha/export.py:996`; `app/folha/export.py:1005`)
- Cadastro zero → previsto → métodos de vínculo/template → folgas e feriados. (`app/folha/export.py:934`; `app/escala/utils.py:2266`; `app/escala/utils.py:2269`; `app/escala/models.py:1286`; `app/escala/models.py:1395`)
- Dono → fila/verbos/pergunta viva → tranca, trilha humana, materialização e batidas. (`app/chamados/juizes.py:1307`; `app/chamados/juizes.py:1310`; `app/chamados/juizes.py:1322`; `app/chamados/juizes.py:1157`; `app/chamados/juizes.py:1088`; `app/chamados/juizes.py:1234`)

A enumeração abaixo distingue **leitura ORM da linha inteira** de consumo individual em Python: consultas sem `only/defer/values` carregam os campos concretos declarados, inclusive campos que a decisão não usa. É por isso que `Colaborador.cpf` aparece no inventário, embora o classificador use nome/código e não consulte o atributo CPF. (`app/folha/export.py:777`; `app/folha/export.py:831`; `app/colaboradores/models.py:319`)

Nas relações, o nome do campo inclui a coluna correspondente `*_id`; `pk` designa a identidade usada nas consultas. As declarações desses campos estão nos respectivos modelos. (`app/colaboradores/models.py:321`; `app/escala/models.py:872`; `app/chamados/models.py:2424`)

Campos carregados por ORM, um por linha:

```text
Empresa.pk -> handle (app/folha/management/commands/lavrar_retrato_folha.py:51)
Empresa.razao_social -> empresas_visiveis (app/colaboradores/queries.py:87)
Empresa.nome_fantasia -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.cnpj -> empresas_visiveis (app/colaboradores/queries.py:86)
Empresa.ativa -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.em_rollout -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.dia_inicio_competencia -> corte_da_empresa (app/ponto/janelas.py:49)
Empresa.janela_he_ativa -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_piso_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.limite_decisao_he_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_teto_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_saida_ativa -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_saida_teto_min -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.he_pendente_trava_export -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.janela_he_desde -> empresas_visiveis (app/colaboradores/queries.py:85)
Empresa.regime_trabalhista -> regua_para (app/core/regua_cct.py:353)

FechamentoMensal.pk -> classificar_export (app/folha/export.py:777)
FechamentoMensal.colaborador -> classificar_export (app/folha/export.py:779)
FechamentoMensal.mes -> classificar_export (app/folha/export.py:778)
FechamentoMensal.ano -> classificar_export (app/folha/export.py:778)
FechamentoMensal.status -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_trabalhadas -> classificar_export (app/folha/export.py:823)
FechamentoMensal.horas_noturnas -> eventos_do_fechamento (app/folha/export.py:447)
FechamentoMensal.horas_extras -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_extras_50 -> eventos_do_fechamento (app/folha/export.py:441)
FechamentoMensal.horas_extras_50_noturna -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_extras_100 -> eventos_do_fechamento (app/folha/export.py:421)
FechamentoMensal.horas_extras_100_feriado -> eventos_do_fechamento (app/folha/export.py:420)
FechamentoMensal.horas_extras_100_noturna -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_folga_trabalhada -> eventos_do_fechamento (app/folha/export.py:446)
FechamentoMensal.horas_atraso -> eventos_do_fechamento (app/folha/export.py:452)
FechamentoMensal.horas_saida_antecipada -> eventos_do_fechamento (app/folha/export.py:452)
FechamentoMensal.horas_falta -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_intra_indenizada -> eventos_do_fechamento (app/folha/export.py:448)
FechamentoMensal.turnos_abertos -> classificar_export (app/folha/export.py:825)
FechamentoMensal.minutos_previstos -> classificar_export (app/folha/export.py:777)
FechamentoMensal.dias_incertos -> classificar_export (app/folha/export.py:777)
FechamentoMensal.minutos_abonados -> classificar_export (app/folha/export.py:777)
FechamentoMensal.minutos_realizados -> classificar_export (app/folha/export.py:777)
FechamentoMensal.dias_previstos -> classificar_export (app/folha/export.py:777)
FechamentoMensal.motivos_espelho -> classificar_export (app/folha/export.py:777)
FechamentoMensal.causa_espelho -> _causas_gravadas (app/folha/retrato_folha.py:1115)
FechamentoMensal.previsto_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.derivado_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.semanas_dsr_ok -> classificar_export (app/folha/export.py:777)
FechamentoMensal.semanas_dsr_perdido -> classificar_export (app/folha/export.py:777)
FechamentoMensal.horas_reflexo_dsr -> classificar_export (app/folha/export.py:777)
FechamentoMensal.saldo_banco_horas -> classificar_export (app/folha/export.py:777)
FechamentoMensal.inconsistencias -> classificar_export (app/folha/export.py:960)
FechamentoMensal.aprovado_por -> classificar_export (app/folha/export.py:777)
FechamentoMensal.aprovado_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.observacao -> classificar_export (app/folha/export.py:777)
FechamentoMensal.criado_em -> classificar_export (app/folha/export.py:777)
FechamentoMensal.atualizado_em -> classificar_export (app/folha/export.py:777)

Colaborador.pk -> dias_que_seguram (app/folha/fora_da_folha.py:131)
Colaborador.isento_ponto -> isento_ponto (app/core/constants.py:18)
Colaborador.matricula_hasner -> classificar_export (app/folha/export.py:779)
Colaborador.matricula_nexti -> classificar_export (app/folha/export.py:779)
Colaborador.codigo_dominio -> classificar_export (app/folha/export.py:832)
Colaborador.cod_esocial -> classificar_export (app/folha/export.py:779)
Colaborador.cpf -> classificar_export (app/folha/export.py:779)
Colaborador.nome -> classificar_export (app/folha/export.py:831)
Colaborador.empresa -> classificar_export (app/folha/export.py:778)
Colaborador.posto -> classificar_export (app/folha/export.py:779)
Colaborador.situacao -> classificar_export (app/folha/export.py:802)
Colaborador.data_admissao -> motivos_retencao_celula (app/folha/export.py:1198)
Colaborador.data_demissao -> motivos_retencao_celula (app/folha/export.py:1200)
Colaborador.cargo -> classificar_export (app/folha/export.py:779)
Colaborador.data_nascimento -> classificar_export (app/folha/export.py:779)
Colaborador.genero -> classificar_export (app/folha/export.py:779)
Colaborador.estado_civil -> classificar_export (app/folha/export.py:779)
Colaborador.pis -> classificar_export (app/folha/export.py:779)
Colaborador.rg -> classificar_export (app/folha/export.py:779)
Colaborador.rg_emissor -> classificar_export (app/folha/export.py:779)
Colaborador.telefone -> classificar_export (app/folha/export.py:779)
Colaborador.telefone2 -> classificar_export (app/folha/export.py:779)
Colaborador.email_pessoal -> classificar_export (app/folha/export.py:779)
Colaborador.end_rua -> classificar_export (app/folha/export.py:779)
Colaborador.end_numero -> classificar_export (app/folha/export.py:779)
Colaborador.end_complemento -> classificar_export (app/folha/export.py:779)
Colaborador.end_bairro -> classificar_export (app/folha/export.py:779)
Colaborador.end_cidade -> classificar_export (app/folha/export.py:779)
Colaborador.end_uf -> classificar_export (app/folha/export.py:779)
Colaborador.end_cep -> classificar_export (app/folha/export.py:779)
Colaborador.motivo_desligamento -> classificar_export (app/folha/export.py:779)
Colaborador.usuario -> classificar_export (app/folha/export.py:779)
Colaborador.perfil -> classificar_export (app/folha/export.py:779)
Colaborador.deve_trocar_senha -> classificar_export (app/folha/export.py:779)
Colaborador.fcm_token -> classificar_export (app/folha/export.py:779)
Colaborador.foto -> classificar_export (app/folha/export.py:779)
Colaborador.fcm_removido_em -> classificar_export (app/folha/export.py:779)
Colaborador.notificacao_negada_em -> classificar_export (app/folha/export.py:779)
Colaborador.fcm_removido_motivo -> classificar_export (app/folha/export.py:779)
Colaborador.onboarding_completo -> classificar_export (app/folha/export.py:779)
Colaborador.wk_em_operacao -> classificar_export (app/folha/export.py:779)
Colaborador.onboarding_pendente -> classificar_export (app/folha/export.py:779)
Colaborador.aceite_termos_em -> classificar_export (app/folha/export.py:779)
Colaborador.aceite_termos_versao -> classificar_export (app/folha/export.py:779)
Colaborador.gps_concedido_em -> classificar_export (app/folha/export.py:779)
Colaborador.camera_concedida_em -> classificar_export (app/folha/export.py:779)
Colaborador.notif_concedida_em -> classificar_export (app/folha/export.py:779)
Colaborador.telefone_confirmado_em -> classificar_export (app/folha/export.py:779)
Colaborador.email_confirmado_em -> classificar_export (app/folha/export.py:779)
Colaborador.criado_em -> classificar_export (app/folha/export.py:779)
Colaborador.atualizado_em -> classificar_export (app/folha/export.py:779)
Colaborador.plataforma_principal -> classificar_export (app/folha/export.py:779)

CelulaDia.pk -> motivos_retencao_celula (app/folha/export.py:1221)
CelulaDia.colaborador -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.data -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.trabalha -> motivos_retencao_celula (app/folha/export.py:1255)
CelulaDia.origem -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.escala_geradora -> vinculo_do_dia (app/escala/alimentacao.py:270)
CelulaDia.dna -> periodos_do_dia (app/ponto/juiz_batida.py:158)
CelulaDia.gerada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.regenerada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.regeneracoes -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.dna_anterior -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.editada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.editada_por -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.julgada_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.impressao -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.veredito -> motivos_retencao_celula (app/folha/export.py:1233)
CelulaDia.veredito_via -> _pergunta_viva_calc (app/chamados/juizes.py:1168)
CelulaDia.veredito_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.protestos -> motivos_retencao_celula (app/folha/export.py:1246)
CelulaDia.ata -> periodos_do_dia (app/ponto/juiz_batida.py:157)
CelulaDia.ausencia -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.tem_entrada -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.tem_saida -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.insumos_em -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.status_dia -> motivos_retencao_celula (app/folha/export.py:1206)
CelulaDia.status_rotulo -> motivos_retencao_celula (app/folha/export.py:1206)

EscalaColaborador.pk -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:155)
EscalaColaborador.colaborador -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:149)
EscalaColaborador.posto -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
EscalaColaborador.tipo_escala -> classificar_export (app/folha/export.py:930)
EscalaColaborador.data_inicio -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:149)
EscalaColaborador.data_fim -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:151)
EscalaColaborador.ativa -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:150)
EscalaColaborador.regua_confianca -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_classe -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_avaliada_em -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_dias_baixo -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_corroborada_em -> classificar_export (app/folha/export.py:930)
EscalaColaborador.regua_corroborada_por -> classificar_export (app/folha/export.py:930)
EscalaColaborador.folga_calendario -> classificar_export (app/folha/export.py:930)
EscalaColaborador.trabalha_em_feriado -> eh_dia_trabalho_calculado (app/escala/models.py:1390)
EscalaColaborador.data_ancora_colaborador -> classificar_export (app/folha/export.py:801)
EscalaColaborador.origem_ancora -> classificar_export (app/folha/export.py:930)
EscalaColaborador.chamado_ancora -> classificar_export (app/folha/export.py:930)

TipoEscala.pk -> classificar_export (app/folha/export.py:930)
TipoEscala.codigo -> classificar_export (app/folha/export.py:930)
TipoEscala.descricao -> classificar_export (app/folha/export.py:930)
TipoEscala.minutos_jornada -> classificar_export (app/folha/export.py:930)
TipoEscala.hora_inicio -> marcos_do_dia (app/escala/models.py:343)
TipoEscala.hora_fim -> marcos_do_dia (app/escala/models.py:343)
TipoEscala.hora_inicio_intervalo -> marcos_do_dia (app/escala/models.py:344)
TipoEscala.hora_fim_intervalo -> marcos_do_dia (app/escala/models.py:345)
TipoEscala.hora_inicio_intervalo2 -> pausas_cadastradas (app/escala/models.py:276)
TipoEscala.hora_fim_intervalo2 -> pausas_cadastradas (app/escala/models.py:276)
TipoEscala.noturno -> classificar_export (app/folha/export.py:930)
TipoEscala.permite_hora_extra -> classificar_export (app/folha/export.py:930)
TipoEscala.he_extra_antes_min -> classificar_export (app/folha/export.py:930)
TipoEscala.he_extra_depois_min -> classificar_export (app/folha/export.py:930)
TipoEscala.folga_calendario_padrao -> classificar_export (app/folha/export.py:930)
TipoEscala.intervalo_modo -> _intervalo_livre (app/escala/models.py:250)
TipoEscala.intervalo_indenizavel -> periodos_do_dia (app/ponto/juiz_batida.py:310)
TipoEscala.intervalo_duracao_min -> intervalo_cadastrado (app/ponto/juiz_batida.py:143)
TipoEscala.apelido -> classificar_export (app/folha/export.py:930)
TipoEscala.tipo_ciclo -> classificar_export (app/folha/export.py:800)
TipoEscala.ciclo_dias -> classificar_export (app/folha/export.py:930)
TipoEscala.is_template -> classificar_export (app/folha/export.py:930)
TipoEscala.template_nome -> classificar_export (app/folha/export.py:930)
TipoEscala.data_ancora -> classificar_export (app/folha/export.py:930)
TipoEscala.dias_trabalho_semana -> eh_dia_trabalho (app/escala/models.py:831)
TipoEscala.template_pai -> classificar_export (app/folha/export.py:930)
TipoEscala.nivel -> classificar_export (app/folha/export.py:930)
TipoEscala.tipo_base -> classificar_export (app/folha/export.py:930)
TipoEscala.editavel -> classificar_export (app/folha/export.py:930)
TipoEscala.perfil_apuracao -> classificar_export (app/folha/export.py:930)
TipoEscala.folga_dia_semana -> eh_dia_trabalho (app/escala/models.py:838)
TipoEscala.horarios_por_dia -> _override_do_dia (app/escala/models.py:227)

Posto.pk -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.nome -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.empresa -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.tolerante_offline -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.praca -> regua_para (app/core/regua_cct.py:330)
Posto.municipio -> lugar_do_vinculo (app/core/feriados.py:122)
Posto.endereco -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.numero -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.complemento -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.bairro -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.cidade -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.cep -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.opera_em_feriado -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.lotacao_minima -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.lotacao_maxima -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.latitude -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.longitude -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.raio_metros -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.ativo -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Posto.revisar -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)

Praca.pk -> regua_para (app/core/regua_cct.py:330)
Praca.nome -> regua_para (app/core/regua_cct.py:330)
Praca.cidade -> regua_para (app/core/regua_cct.py:330)
Praca.estado -> regua_para (app/core/regua_cct.py:330)
Praca.sindicato -> regua_para (app/core/regua_cct.py:330)
Praca.obs -> regua_para (app/core/regua_cct.py:330)
Praca.adicional_noturno_percentual -> regua_para (app/core/regua_cct.py:330)
Praca.tolerancia_minutos -> regua_para (app/core/regua_cct.py:330)
Praca.banco_horas_prazo_dias -> regua_para (app/core/regua_cct.py:330)
Praca.ativa -> regua_para (app/core/regua_cct.py:330)

Municipio.pk -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Municipio.codigo_ibge -> escalas_do_periodo_por_colab (app/escala/alimentacao.py:152)
Municipio.nome -> _lugar (app/core/feriados.py:53)
Municipio.uf -> _lugar (app/core/feriados.py:52)

Feriado.pk -> feriados_de (app/core/feriados.py:83)
Feriado.nome -> feriados_de (app/core/feriados.py:83)
Feriado.data -> _qs (app/core/feriados.py:29)
Feriado.abrangencia -> alcanca (app/core/feriados.py:60)
Feriado.uf -> alcanca (app/core/feriados.py:63)
Feriado.municipio -> alcanca (app/core/feriados.py:66)
Feriado.fonte -> feriados_de (app/core/feriados.py:83)

AplicacaoConvencao.pk -> _aplicacao_vigente (app/core/regua_cct.py:259)
AplicacaoConvencao.sindicato -> _aplicacao_vigente (app/core/regua_cct.py:255)
AplicacaoConvencao.empresa -> _aplicacao_vigente (app/core/regua_cct.py:254)
AplicacaoConvencao.praca -> _aplicacao_vigente (app/core/regua_cct.py:256)
AplicacaoConvencao.ativo -> _aplicacao_vigente (app/core/regua_cct.py:254)
AplicacaoConvencao.criado_em -> _aplicacao_vigente (app/core/regua_cct.py:259)
AplicacaoConvencao.criado_por -> _aplicacao_vigente (app/core/regua_cct.py:259)
AplicacaoConvencao.motivo -> _aplicacao_vigente (app/co

[truncado em 20000 de 69455 caracteres]

## 09/11 11:41 · sessao 6d93743673c9 · - · - · origem=Ronald (tmux)

Fecho confirmado por AST em memória e inspeção de métodos:

- `classificar_export → motivos_retencao_espelho → motivos_retencao_celula → nao_retem_pela_pausa → periodos_do_dia`. (`app/folha/export.py:960`, `:1266`, `:1253`, `:1165`, `:1142`)
- `classificar_export → _codigos_nao_homologados → eventos_do_fechamento / _ciclo_do_colab / feriado_12x36_em_dobra / datas_falta → regua_para / alimentacao / TipoAusencia / Ausencia`. (`app/folha/export.py:966`, `:994`, `:995`, `:996`, `:1005`, `:380`, `:359`, `:180`)
- `classificar_export → minutos_previstos_do_dia → EscalaColaborador.eh_dia_trabalho / marcos_do_dia / pausas_do_dia → TipoEscala / FolgaDia / Feriado`. (`app/folha/export.py:934`; `app/escala/utils.py:2266`, `:2269`, `:2291`; `app/escala/models.py:1096`, `:1133`, `:1208`, `:1286`, `:1395`)
- `fora_da_folha → dias_que_seguram → classificar_export`; depois `alimentar_portas → particionar_verbos / qs_na_fila / pergunta_viva`; `porta_alcancavel_do_dia → chamado_que_lista / perguntas_do_app`. (`app/folha/fora_da_folha.py:161`, `:119`, `:166`; `app/chamados/juizes.py:1307`, `:1310`, `:1322`, `:1399`, `:1400`, `:1339`)
- `pergunta_viva → dia_da_pergunta_trancado → periodo_trancado / _toque_humano_no_dia → LogAuditoria / User`; outros ramos chegam a `classificar_falta`, materialização e `turnos_do_colab`, lendo batidas, células, vínculos e templates. (`app/chamados/juizes.py:1157`, `:964`, `:971`, `:1087`, `:1088`, `:1221`, `:1234`, `:384`)

A resolução sintática preliminar alcançou 178 funções; esse número exclui expansão posterior de métodos de instância, reexports e callbacks. Portanto, não equivale a prova de 178 funções no fecho completo. `[nao verificado]`

`prontidao`, `grade_do_fechamento`, `get_motor_cct`, `EmpresaIntegracao`, `ExportacaoDominio` e `Holerite` não pertencem aos caminhos inspecionados dessas raízes; `grade_da_celula` entra por `_tipo_pelo_marco`, separadamente. (`app/folha/export.py:757`, `:986`; `app/chamados/services/materializacao.py:282`)

Foram enviados ao principal os inventários detalhados de campos, o mapa de leituras de linhas completas e os relógios. Há uma escrita potencial dentro desse fecho: `get_user_sistema()` usa `User.objects.get_or_create(username='sistema')`; nenhum código da aplicação foi executado nesta inspeção. (`app/core/utils.py:18`; `app/chamados/juizes.py:1087`)

## 09/11 11:41 · sessao 8a265e5c5b41 · - · - · origem=Ronald (tmux)

O-OUTBOX verificado na cópia:

- Modelo `EventoCelula`: campos `celula_id`, `colaborador_id`, `data`, `criado_em`, `processado_em`, `tentativas`, `erro`; sem payload e sem índice único de pendente. Não contém empresa nem competência gravadas (`app/escala/models.py:1845-1871`).
- Escritor único: `emitir_evento_celula`; faz `EventoCelula.objects.create(celula_id=..., colaborador_id=..., data=...)` (`app/ponto/portas/celula.py:882-891`).
- `salvar_e_emitir` grava célula e evento no mesmo `atomic`; somente mudanças de `veredito` ou `ata` acordam o consumidor (`app/ponto/portas/celula.py:878-902`).
- Consumidor único de produção: comando `consumir_eventos_celula`, com advisory lock por schema; cron a cada minuto (`app/chamados/management/commands/consumir_eventos_celula.py:17-31`; `app/chamados/reconciliador.py:1215-1242`; `app/config/crons.py:393-394`).
- Agrupa eventos por colaborador dentro de cada lote de até 500; não é debounce por empresa×competência. Roda lotes até esvaziar ou ultrapassar 45 s (`app/chamados/reconciliador.py:1209-1211`; `app/chamados/reconciliador.py:1342-1352`; `app/chamados/reconciliador.py:1402-1404`).
- Consumidores funcionais: rejunção, retratação de perguntas absolvidas, retratação de chamados absolvidos, premissa morta, órfãs pela lâmpada e fechamento por lastro (`app/chamados/reconciliador.py:1363-1383`).
- Falha mantém pendente, incrementa tentativas e guarda erro; após cinco tentativas sai do universo vivo; processados são apagados após sete dias (`app/chamados/reconciliador.py:1209-1210`; `app/chamados/reconciliador.py:1246-1255`; `app/chamados/reconciliador.py:1388-1404`).
- Semeadura evita pendente vivo já existente por célula; a emissão ordinária não deduplica (`app/chamados/reconciliador.py:1278-1310`; `app/ponto/portas/celula.py:891`).

Emissores já existentes:

- `CelulaDia.lavrar_veredito`, `carimbar` e `carimbar_precedencia`: emitem condicionalmente pela interseção com veredito/ata; alteração apenas de status não emite (`app/ponto/portas/celula.py:1003`; `app/ponto/portas/celula.py:1013-1020`; `app/ponto/portas/celula.py:1035`; `app/ponto/portas/celula.py:1067`; `app/ponto/portas/celula.py:1082`).
- `ChamadoColaborador.abrir` e `criar`: emitem quando nasce chamado de módulo de turno com célula ligada (`app/chamados/models.py:971-980`; `app/chamados/models.py:1093-1099`).
- `recompute_turnos`: emite para turnos anteriormente abertos que fecharam, se existir célula para o dia (`app/ponto/nucleo.py:57-66`; `app/ponto/nucleo.py:86-93`).
- `PerguntaDisputa.save`: atualização existente emite via receiver quando toca `CAMPOS_QUE_ACORDAM`; nascimento não emite. Save e receiver estão no mesmo `atomic` (`app/chamados/models.py:3220-3222`; `app/chamados/signals.py:237-244`; `app/chamados/models.py:3413-3423`).
- `reabrir_para_correcao`: update em lote emite explicitamente depois da escrita; `descarimbar_em_lote`, isoladamente, não emite (`app/chamados/services/disputa_emissao.py:1602-1630`; `app/chamados/models.py:3252-3266`).
- `PerguntaDisputa.acordar_celulas` deduplica as células apenas dentro da chamada, pela PK; usa FK da pergunta ou busca pelo par colaborador/data (`app/chamados/models.py:3231-3248`).
- Batidas e ausências podem emitir indiretamente: seus saves/deletes chamam cartório, que lavra a célula; somente mudança de ata/veredito gera evento (`app/ponto/signals.py:85-93`; `app/ponto/signals.py:131-152`; `app/ponto/portas/celula.py:878-902`).
- Mudança do status do chamado pode emitir indiretamente pelo rejulgamento da célula; alterações apenas de contexto/texto/urgência não passam nessa guarda (`app/chamados/signals.py:187-203`).
- `DisputaSupervisao.fechar` pode emitir indiretamente ao fechar chamado-pai/materializar perguntas; não possui INSERT próprio no outbox (`app/chamados/signals.py:18-45`; `app/chamados/signals.py:147-163`).

Correções relevantes ao aval:

- Escala não pode ser classificada genericamente como “não invalida”: pós-save de `EscalaColaborador` chama `invalidar_previsto`, que atualiza `FechamentoMensal.atualizado_em` quando `previsto_em` não é nulo (`app/escala/signals.py:124-133`; `app/ponto/services/fechamento.py:1607-1613`).
- `salvar_tipo` também invalida fechamento após regeneração de frota (`app/escala/services/cadastro_tipo.py:351-359`).
- Ausência também não pode ser classificada genericamente como “não invalida”: edição e decisão chamam `invalidar_fechamento`; o update só alcança registros com `derivado_em` não nulo (`app/ponto/services/ausencia.py:518-524`; `app/ponto/services/ausencia.py:941-948`; `app/ponto/services/fechamento.py:1652-1656`).
- `TipoEscalaAdmin` é escritor fora da porta: usa `admin.ModelAdmin` editável; `EscalaColaboradorAdmin` usa `SoLeitura`. O admin não chama o serviço `salvar_tipo` que faz a propagação/invalidação (`app/escala/admin.py:7-13`; `app/escala/services/cadastro_tipo.py:306-359`).
- `CatalogoDominio`: escritor Python não encontrado; escritor de produção `[nao verificado]`. Modelo/leitor existem (`app/folha/models.py:248-258`; `app/folha/export.py:259-260`).

Inventário de escritores enviado nas mensagens anteriores: Empresa/Colaborador/EscalaColaborador; FolgaDia/calendários/agenda; Ausencia/TipoAusencia; AgendamentoFerias/PeriodoAquisitivo; ChamadoColaborador/PerguntaDisputa/DisputaSupervisao; FechamentoMensal/PeriodoFechado/RubricaDominio. O censo AST da cópia foi executado sem escrita; ele próprio declara que saves de parâmetros, forms e modelos dinâmicos podem permanecer não atribuíveis, portanto não prova completude sozinho (`app/core/censo_escritas.py:43-56`).

## 09/11 11:39 · sessao 9d3a5d3f52b3 · - · - · origem=Ronald (tmux)

Resultado das questões 4–6, somente leitura:

**4. Frescor e invalidação**

- O juiz compara `Max(FechamentoMensal.atualizado_em)` por empresa/mês/ano com `RetratoFolha.lavrado_em`; fechamento posterior significa `desatualizado`. O universo consultado contém apenas competências atual e anterior das empresas visíveis (`app/folha/retrato_folha.py:1287-1306`).
- A view lê esse juiz e recorta avisos para a competência e empresas exibidas, sem comparar carimbos por conta própria (`app/folha/views_retrato.py:199-210`).
- Estados do juiz: `nunca lavrado`, `desatualizado`, `atualizado`; suas frases vêm de `_rotulo_admin` (`app/folha/retrato_folha.py:1276-1277`, `app/folha/retrato_folha.py:1323-1329`).
- A barra mostra o aviso de desatualização ou `atualizado HH:MM`; sem retrato, a lista mostra “Sem retrato lavrado”. Nesse fluxo não há estado `atualizando` ou `falhou` (`app/templates/folha/_retrato_barra.html:19-22`, `app/templates/folha/_retrato_lista.html:17-20`).
- `FechamentoMensal.atualizado_em` é `auto_now=True` (`app/ponto/models.py:479`).
- Escritores que movem o carimbo: criação/lavra por `recalcular_fechamento_mes` (`app/ponto/services/fechamento.py:898-900`, `app/ponto/services/fechamento.py:1143-1161`); `aprovar` com save completo (`app/ponto/services/fechamento.py:1335-1340`); `restaurar_fechamento` (`app/ponto/services/fechamento.py:1392-1393`); `reabrir_periodo` (`app/ponto/services/fechamento.py:1449-1452`); `invalidar_previsto` (`app/ponto/services/fechamento.py:1610-1613`); `invalidar_fechamento` (`app/ponto/services/fechamento.py:1652-1656`).
- Escritores que escrevem fechamento **sem mover esse carimbo**: `aprovar_lote` faz update sem `atualizado_em`; `reabrir_lote` também; `reabrir` faz save com `update_fields` que o omite (`app/ponto/services/fechamento.py:1329-1332`, `app/ponto/services/fechamento.py:1344-1354`). Portanto, o comentário “sobe em TODO save” precisa dessa ressalva (`app/folha/retrato_folha.py:1264`).
- A afirmação genérica do aval de que escala/veredito/ausência não invalidam não descreve todos os caminhos desta cópia: save de `EscalaColaborador` chama `invalidar_previsto` por signal (`app/escala/signals.py:124-133`); edição e decisão de ausência chamam `invalidar_fechamento` (`app/ponto/services/ausencia.py:509-524`, `app/ponto/services/ausencia.py:935-948`); regeneração de células pode recalcular por competência (`app/ponto/portas/celula.py:445-458`).
- Essas invalidações são condicionais: `invalidar_previsto` só atualiza linhas com `previsto_em` preenchido; `invalidar_fechamento` só atualiza linhas com `derivado_em` preenchido (`app/ponto/services/fechamento.py:1611-1613`, `app/ponto/services/fechamento.py:1652-1656`).
- `recalcular_por_evento` agenda no `on_commit` a lavra do fechamento do colaborador na competência do fato; falha gera warning, sem propagar exceção (`app/ponto/services/fechamento.py:1798-1827`).
- Chamadores dessa rederivação incluem batida, decisão de HE, folga aceita e validação de disputa (`app/ponto/registro_batida.py:177-178`, `app/ponto/portas/he.py:201-202`, `app/chamados/services/validacao.py:360-361`, `app/chamados/services/validacao.py:445-446`).
- A família declara `ponto/services/fechamento.py` como porta única; o teste de censo AST tem allowlist e pendentes vazios e verifica escritores externos (`app/folha/tests/test_chokepoint_folha_gate.py:6-9`, `app/folha/tests/test_chokepoint_folha_gate.py:28-31`, `app/folha/tests/test_chokepoint_folha_gate.py:58-62`, `app/folha/tests/test_chokepoint_folha_gate.py:72`).

**5. Custo**

- O comando aceita uma empresa e competência explícitas; sem os filtros, percorre todas as empresas visíveis e competências anterior/atual. Portanto, suporta a unidade empresa × competência, mas a execução padrão é frota × duas competências (`app/folha/management/commands/lavrar_retrato_folha.py:38-40`, `app/folha/management/commands/lavrar_retrato_folha.py:48-65`).
- A configuração do cron usa `--apply` sem `--empresa`; o comentário confirma uma corrida após todas as empresas do gatilho (`app/config/crons.py:1606-1609`, `app/config/crons.py:1627-1629`).
- Cada `montar_linhas(empresa, mes, ano)` chama `fora_da_folha` uma vez; esta chama `dias_que_seguram`, que chama `classificar_export` uma vez. Assim, **uma chamada a `classificar_export` por empresa × competência**, oito no exemplo de quatro empresas × duas competências (`app/folha/retrato_folha.py:1156-1160`, `app/folha/fora_da_folha.py:160-162`, `app/folha/fora_da_folha.py:118-119`).
- Produção histórica documentada: **17,8 s para oito retratos**, quatro empresas × duas competências, em 11/10; os subtotais são por empresa com duas competências: emp1 1,31 s; emp2 11,04 s; emp3 3,77 s; emp4 1,65 s. Não constituem medição por empresa × competência (`app/config/crons.py:1591-1594`; repetido na docstring `app/folha/management/commands/lavrar_retrato_folha.py:21-22`).
- Outra execução registrada informa **12 s para oito retratos**, atuais/anterior, às 02:52 (`app/docs/RELATO.md:5193`); outra passagem registra oito retratos em 12 s pela via homologada, carimbo de 21/10 18:49:53 (`app/docs/RELATO.md:5533-5534`).
- Há medição **unitária de sombra**, não produção nem tempo integral do lavrador: `fora_da_folha` real de emp2 × competência 11 passou de 7,3 s para 32,0 s no ensaio F5C de 30/10 (`app/docs/RELATO.md:1132`).
- Há ensaio de sombra que registra **10,8 s para o ATO3 `lavrar_retrato_folha`**, com aviso de ambiente de teste em quatro núcleos; não é medida unitária de produção (`app/docs/RELATO.md:4764`).
- `crons_duracao.json` contém **39 s** para o comando (`app/config/crons_duracao.json:49`). Isso é valor de agendamento calculado por **p95 das corridas dos últimos sete dias × 1,5, piso de 30 s**, não duração medida de uma unidade (`bin/crons_duracao.py:9-13`, `bin/crons_duracao.py:69-76`). O arquivo informa fonte `app/logs/placar.jsonl`, geração em 31/10, janela sete dias (`app/config/crons_duracao.json:106-110`).
- O teste comprova uma linha por empresa/competência e idempotência, não benchmark de produção (`app/folha/tests/test_retrato_folha_contador.py:265-280`).
- Custo atual medido em produção por empresa × competência: **[nao verificado]**. Não dividir os totais de oito retratos e apresentar a média como medição unitária.

**6. Lei escrita**

- O48 está registrada como fechamento = leitura da célula soberana em qualquer período; “recalcular deixa de existir”, somente atos persistem (`app/docs/CORTES.md:174`, `app/docs/PROMPTS.md:1135`).
- A etapa do retrato declara fonte existente, universo por empresa e competência, retrato gravado em lote e proibição de calcular ao abrir a tela; chama isso de complemento da O48, sem lei nova (`app/docs/PROMPTS.md:1156`).
- Rodar fora do horário do cron já é permitido pela regra de cron idempotente que “pode ser adiantado na mão” (`CLAUDE.md:246`); há aval específico para relavrar imediatamente pela porta e conferir com `classificar_export` (`app/docs/PROMPTS.md:1339`).
- O padrão de evento/outbox e projeções já inclui retrato: `PLANO-PADROES` nomeia espelho, DiaPago, fechamento, retrato e pílulas como projeções reconstruíveis, além do outbox fora da pilha da batida (`app/docs/CORTES.md:288`).
- O contrato outbox aprovado prescreve evento na mesma transação do veredito e consumidor idempotente a cada minuto; isso é a autorização escrita do mecanismo existente, não prova de que já consome eventos para relavrar retrato (`app/docs/CORTES.md:351`).
- O aval de 22/10 registra invalidação por evento, depois recolocada após 20/20; é registro de ordem, não proibição permanente de lavra fora do cron (`app/docs/PROMPTS.md:1339`, `app/docs/PROMPTS.md:1341`).
- Substituir o anterior está implementado pelo escritor único com `update_or_create(empresa, mes, ano)` (`app/folha/management/commands/lavrar_retrato_folha.py:74-76`), restrição única no banco (`app/folha/models.py:362-364`) e teste de segunda lavra preservando a mesma PK e uma linha (`app/folha/tests/test_retrato_folha_contador.py:270-280`).
- A docstring explicita “SUBSTITUI, um por empresa+competência” (`app/folha/models.py:314-317`); a lei geral exige um escritor por entidade e idempotência “2x = mesmo estado, 1 linha de trilha, 0 duplicata” (`CLAUDE.md:173-175`).
- A L-121 manda reconstruir projeções pela porta quando pousa cura de juiz, observando competência exportada e DIFF/reversão (`app/docs/LEIS.md:142`).
- Para as duas perguntas delimitadas — execução fora do cron e substituição por empresa/competência — não identifiquei pergunta adicional de lei: as regras acima já cobrem ambas. A exigência de não propor lei nova por iniciativa própria está expressa (`app/docs/LEIS.md:138`, `CLAUDE.md:266-270`).

Sugestão de fechamento do laudo consolidado: `perguntas de lei: 0`, seguido de lista vazia ou “Nenhuma”, com as referências acima.

## 09/11 11:15 · sessao 8db4566726fa · O10-D-PERMUTA-NA-TELA · censo · origem=Code

1) **Há entradas genéricas pelo app/PWA; pedido próprio de permuta, com o par estruturado, não foi confirmado [nao verificado].** `Ausencia` guarda uma pessoa e um período; `ChamadoColaborador` guarda solicitação textual e contexto JSON, sem campos próprios para os dois lados da troca (`app/ponto/models.py:743`; `app/ponto/models.py:786`; `app/chamados/models.py:38`).

| Entrada existente | Endpoint, view e gravação |
|---|---|
| Ausência pelo app | `GET/POST /api/ausencias/` → `api_ausencias`; o POST chama `criar_ausencia`, identificando o colaborador e `registrado_por=request.user` (`app/api/urls.py:82`; `app/api/views.py:1700`; `app/api/views.py:1836`). |
| Ausência pelo PWA | `/ponto/ausencias/submeter/` → `submeter_ausencia`; chama a mesma porta e renderiza `ponto/submeter_ausencia.html` (`app/ponto/urls.py:64`; `app/ponto/views.py:2467`; `app/ponto/views.py:2536`; `app/ponto/views.py:2560`). |
| Solicitação textual pelo app | `POST /api/chamados/abrir/` → `api_abrir_chamado`; grava assunto, descrição, classificação, urgência e `modulo_origem='app_colaborador'` (`app/api/urls.py:87`; `app/api/views.py:2153`; `app/api/views.py:2201`). |
| Solicitação textual pelo PWA | `/chamados/abrir/` → `abrir_chamado`, serviço `abrir_chamado_local`, template `chamados/form_abertura.html` (`app/chamados/urls.py:10`; `app/chamados/views.py:147`; `app/chamados/views.py:196`). |

A taxonomia dos chamados contém **“Escala / Turno” → “Troca de turno”**, permitindo um pedido textual dessa natureza; isso não constitui ligação estruturada entre duas cartas ou efetivação da troca (`app/chamados/classificador.py:38`; `app/api/views.py:2201`).

`Ausencia` contém colaborador, tipo, `data/data_fim`, minutos, dias corridos, horários de cobertura parcial, descrição, documento, prazo, chamado de origem, documento de referência, status, autor e carimbos de aprovação/criação (`app/ponto/models.py:743`; `app/ponto/models.py:768`; `app/ponto/models.py:776`; `app/ponto/models.py:799`). O tipo aceito vem do catálogo vivo; a disponibilidade real de `troca_de_plantao` nesse cadastro é **[nao verificado]**, e esse código não consta das choices estáticas do modelo (`app/ponto/services/ausencia.py:193`; `app/api/views.py:1883`; `app/ponto/models.py:661`).

| Estado de `Ausencia` | Quem escreve no fluxo encontrado |
|---|---|
| `aguardando_documento` | `criar_ausencia` usa `estado_inicial`: tipo exige documento e não há anexo (`app/ponto/services/ausencia.py:333`; `app/ponto/services/ausencia.py:790`). |
| `aguardando_decisao` | Criação com documento ou sem exigência dele; `anexar_documento` também transfere para esse estado; o escalonador faz isso quando vence o prazo documental (`app/ponto/services/ausencia.py:790`; `app/ponto/services/ausencia.py:873`; `app/ponto/services/ausencia.py:999`). |
| `aprovada` / `rejeitada` | `decidir`, ato do DP, chama o escritor de status `mudar_estado` (`app/ponto/services/ausencia.py:808`; `app/ponto/services/ausencia.py:897`). |

App e PWA também abrem o chamado `ausencia_colab`, com `ausencia_id`, data, tipo e presença de documento; ele avisa o DP e leva ao registro, sem decidir a ausência (`app/api/views.py:1871`; `app/ponto/views.py:2546`; `app/ponto/services/ausencia.py:637`; `app/ponto/services/ausencia.py:659`).

Os chamados possuem os estados genéricos `aberto`, `em_analise`, `resolvido`, `fechado`, `cancelado`, `registrado` e `superado`; nascem pela fábrica `criar`, e a escrita posterior de `status_local` converge em `_gravar_estado`, incluindo ações de gestão por `transicionar` (`app/chamados/models.py:28`; `app/chamados/models.py:1054`; `app/chamados/models.py:201`; `app/chamados/services/acoes_chamado.py:313`).

| Quem lê hoje no código | Registros consumidos |
|---|---|
| App | GET das últimas 30 ausências do próprio colaborador, com status e descrição; API de chamados visíveis do colaborador (`app/api/views.py:1876`; `app/api/views.py:1031`). |
| Admin/DP | Lista de ausências, com filtros de tipo/status e ações; painel de chamados (`app/ponto/views.py:2155`; `app/ponto/urls.py:53`; `app/chamados/services/painel.py:96`). |
| PWA “Meu Atendimento” | Solicitações e pendências do colaborador; a abertura de novas solicitações depende de `app_solicitacoes`, cujo valor real é **[nao verificado]** (`app/chamados/views.py:612`; `app/chamados/services/solicitacoes_no_app.py:1`). |
| API mensageria/DP | Ausências, contagens por tipo/status e fila aguardando decisão (`app/api/views_mensageria.py:736`). |
| Automação documental | `escalonar_documentos_ausencia` lê ausências aguardando documento; fechamento de chamados consulta a ausência associada (`app/ponto/management/commands/escalonar_documentos_ausencia.py:25`; `app/ponto/services/ausencia.py:992`; `app/ponto/services/ausencia.py:686`). |
| Automação de chamados | `alertar_chamados_sla` lê a fila de chamados e trata vencimentos (`app/chamados/management/commands/alertar_chamados_sla.py:20`; `app/chamados/management/commands/alertar_chamados_sla.py:44`). |

A API de perguntas permite responder uma `PerguntaDisputa` já existente do colaborador; um caminho que transforme essa resposta em par de permuta é **[nao verificado]** (`app/api/views.py:2616`; `app/api/views.py:2643`). Pedidos efetivamente existentes e execução desses crons em produção são **[nao verificado]**.

2) **A porta efetiva duas edições de dia; não faz uma permutação de quatro células.** Ela põe `(cedente, data da carta cedente)` como folga e `(substituto, data da carta substituto)` como trabalho; não grava automaticamente o movimento inverso nas outras duas combinações pessoa/data (`app/ponto/portas/celula.py:673`).

A assinatura é (`app/ponto/portas/celula.py:632`):

```python
trocar_plantao(
    ausencia_cedente, ausencia_substituto, *,
    autor, motivo, barrados=None, apesar_da_lavra=''
)
```

| Argumento | Significado demonstrado |
|---|---|
| `ausencia_cedente` | Instância persistida de `Ausencia`; sua pessoa/data serão editadas para `trabalha=False` (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:673`). |
| `ausencia_substituto` | Outra `Ausencia`; sua pessoa/data serão editadas para `trabalha=True` (`app/ponto/portas/celula.py:659`; `app/ponto/portas/celula.py:673`). |
| `autor` | Objeto com `pk`, usado como autor da edição e da trilha (`app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:668`; `app/ponto/portas/celula.py:686`). |
| `motivo` | Texto obrigatório após `strip`, incluído nas trilhas (`app/ponto/portas/celula.py:648`; `app/ponto/portas/celula.py:668`; `app/ponto/portas/celula.py:691`). |
| `barrados` | Lista opcional que recebe motivos de recusa, incluindo lavra e falta de vínculo (`app/ponto/portas/celula.py:571`; `app/ponto/portas/celula.py:586`; `app/ponto/portas/celula.py:694`). |
| `apesar_da_lavra` | Valor repassado aos dois lados; quando verdadeiro, pula a guarda de lavra e fica registrado na trilha (`app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:614`; `app/ponto/portas/celula.py:668`). |

As cartas são relidas do banco com trava, usando seus `pk`; a porta exige `documento` preenchido, mas não verifica conteúdo ou existência física do arquivo (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:664`). Nos chamadores de teste, vêm de `Ausencia.objects.create`, com tipo `declaracao` e status `aguardando_decisao`; a montagem do par por um chamador de produção presente nesta cópia é **[nao verificado]** (`app/ponto/tests/test_o10b_editar_dia_troca.py:106`).

| Condição | Comportamento atual da porta |
|---|---|
| Autor ausente / motivo vazio | Levanta `ValueError`; não retorna `recusada` nesses casos (`app/ponto/portas/celula.py:646`). |
| Um lado ausente, registro apagado, mesma pessoa ou falta de uma carta | Retorna `recusada`, com motivo em `barrados` quando fornecido (`app/ponto/portas/celula.py:653`; `app/ponto/portas/celula.py:694`). |
| Interjornada de 11h | **Não há checagem de descanso ou consulta aos turnos vizinhos** no corpo da troca/edição (`app/ponto/portas/celula.py:559`; `app/ponto/portas/celula.py:646`). |
| Sobreposição de turnos | **Não há checagem geométrica de sobreposição** antes das edições (`app/ponto/portas/celula.py:668`; `app/ponto/portas/celula.py:675`). |
| Competência exportada/lavrada | Cada lado passa pela guarda de `editar_dia`; se barrar, o par retorna `recusada` e desfaz o outro lado (`app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:675`). |
| Alcance da lavra | Inclui TXT vigente **ou holerite publicado do próprio colaborador**; não equivale simplesmente a `FechamentoMensal.status` (`app/ponto/management/commands/gerar_celulas.py:126`; `app/folha/export.py:589`). |
| Permissão | Exige identificação de autor, mas não verifica `is_staff`, perfil, `tem_acao` ou permissão contextual (`app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:668`). |
| Vínculo | Recusa se o juiz do vínculo do dia não devolver vínculo; não exige que os dois tenham o mesmo vínculo, tipo de escala ou empresa (`app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:673`). |
| Datas iguais/diferentes | Não compara as datas. Há testes escritos esperando sucesso tanto na mesma data quanto em datas/competências diferentes (`app/ponto/portas/celula.py:673`; `app/ponto/tests/test_o10b_editar_dia_troca.py:178`; `app/ponto/tests/test_o10b_editar_dia_troca.py:260`). |
| Tipo/status/período das cartas | Não exige tipo específico ou aprovação; usa somente `Ausencia.data`, sem recusar `data_fim > data` (`app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:675`; `app/docs/BACKLOG.md:1876`). |

Portanto, dos quatro controles citados no aval, **interjornada, sobreposição e permissão do autor não são recusas implementadas**; há somente identificação do autor. A guarda de competência lavrada existe, com a abertura explícita por `apesar_da_lavra` (`app/ponto/portas/celula.py:646`; `app/ponto/portas/celula.py:568`; `app/ponto/portas/celula.py:675`).

O substituto recebe os marcos do **próprio vínculo/template naquela data**, não os horários copiados do cedente; o produtor `montar_dna` congela esses marcos (`app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:591`; `app/ponto/management/commands/gerar_celulas.py:73`).

A transação principal do par é **um `atomic` externo**, contendo as duas chamadas de `editar_dia`; cada edição possui seu `atomic` aninhado, e cada gravação de auditoria usa outro savepoint. Não são dois commits independentes para os lados (`app/ponto/portas/celula.py:652`; `app/ponto/portas/celula.py:592`; `app/core/observ.py:133`). As cartas e células são travadas; as células seguem ordem por `(colaborador_id, data)` (`app/ponto/portas/celula.py:657`; `app/ponto/portas/celula.py:593`; `app/ponto/portas/celula.py:673`).

Quando ambos mudam, grava duas edições e suas trilhas `editar_dia`, mais uma trilha `trocar_plantao`; se ambos já estiverem iguais, retorna `igual` sem essas novas gravações. Falha obrigatória de trilha levanta exceção e desfaz o ato (`app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:611`; `app/ponto/portas/celula.py:619`; `app/ponto/portas/celula.py:679`; `app/ponto/portas/celula.py:686`).

Há efeitos adicionais condicionais: marca de cobrança órfã no mesmo ato; rejulgamento de dias já alcançados pelo relógio, salvo vereditos humanos; agendamento do recálculo financeiro, rejunção e reavaliação de falta após commit. Assim, **o conjunto inteiro de efeitos não cabe numa quantidade fixa de transações**, embora o par editado tenha uma transação principal (`app/ponto/portas/celula.py:625`; `app/ponto/portas/celula.py:422`; `app/ponto/portas/celula.py:445`; `app/ponto/portas/celula.py:490`; `app/ponto/services/fechamento.py:1824`).

Os chamadores concretos encontrados estão no teste O10-B: helper e chamadas diretas para confirmação, incompletude, repetição e par vazio (`app/ponto/tests/test_o10b_editar_dia_troca.py:112`; `app/ponto/tests/test_o10b_editar_dia_troca.py:193`; `app/ponto/tests/test_o10b_editar_dia_troca.py:226`; `app/ponto/tests/test_o10b_editar_dia_troca.py:246`; `app/ponto/tests/test_o10b_editar_dia_troca.py:307`). O relato menciona um lote `trocas_plantao_lote_o10c.py:160`, mas esse arquivo/chamador não foi localizado nesta cópia: **[nao verificado]**; o próprio relato registra ausência de chamador de UI (`app/docs/RELATO.md:426`).

3) **Os leitores principais consultam a célula, mas há leituras diretas dos horários da base.** `EC.eh_dia_trabalho` retorna `cel.trabalha`; `EC.marcos_do_dia` lê `cel.dna.marcos` antes do fallback ao template (`app/escala/models.py:1087`; `app/escala/models.py:1117`).

| Leitor | Caminho comprovado |
|---|---|
| Espelho admin | View → `espelho_do_colab`; previsto da competência por `grade_da_celula` (`app/ponto/views.py:157`; `app/ponto/services/espelho.py:506`). |
| Espelho app | Usa `espelho_do_colab`; há exceção nos horários dos dias sem card, abaixo (`app/api/views.py:1379`; `app/api/views.py:1611`). |
| PDF do espelho | Dias por `espelho_do_colab`; coluna “Previsto” usa template diretamente, abaixo (`app/relatorios/pdf_espelho.py:596`; `app/relatorios/pdf_espelho.py:903`). |
| Calendário admin | Grade por `grade_da_celula`, status por `status_dos_dias_lavrados`, rubricas por `DiaPago` (`app/colaboradores/services/calendario.py:332`; `app/colaboradores/services/calendario.py:602`; `app/colaboradores/services/calendario.py:655`). |
| Calendário/escala app | `eh_dia_trabalho` para trabalho/folga; template direto para horários (`app/api/views.py:950`; `app/api/views.py:955`). |
| Escala PWA e agenda admin | Usam `EC.eh_dia_trabalho` e `EC.marcos_do_dia` (`app/escala/views.py:38`; `app/colaboradores/views.py:2223`; `app/colaboradores/views.py:2232`). |
| Folha/motor | Fechamento consulta `eh_dia_trabalho`, fornece células e chama apuração com EC; o motor consulta os métodos do EC (`app/ponto/services/fechamento.py:729`; `app/ponto/services/fechamento.py:750`; `app/ponto/services/fechamento.py:278`; `app/ponto/motor_calculo_v2.py:2290`; `app/ponto/motor_calculo_v2.py:2447`). |
| TXT | Verbas vêm de `FechamentoMensal`; retenção consulta células e a grade delega a `grade_da_celula`, sem recalcular os valores pelo template (`app/folha/export.py:703`; `app/folha/export.py:1206`; `app/folha/export.py:329`). |

Os leitores diretos da base encontrados nesses caminhos são:

| Leitura direta | Efeito sobre o dia trocado |
|---|---|
| `/api/escala/v2/`: `te.marcos_do_dia` | Horários não vêm do DNA; trabalho/folga vem da célula (`app/api/views.py:950`; `app/api/views.py:955`; `app/api/views.py:968`). |
| Espelho app, dias sem card: `tipo_escala.marcos_do_dia` | Mesma separação: célula decide trabalho/folga, template fornece horários previstos (`app/api/views.py:1611`; `app/api/views.py:1613`; `app/api/views.py:1639`). |
| Bloco escala de `/api/me/`, nas duas cascas | Horários vêm do template; não representa uma leitura dos marcos congelados do dia (`app/api/views.py:259`; `app/api/views_core.py:248`). |
| Coluna “Previsto” do PDF | Usa `tipo_escala.hora_inicio/hora_fim` nas linhas com turnos, sem consultar o DNA daquela linha (`app/relatorios/pdf_espelho.py:903`; `app/relatorios/pdf_espelho.py:978`). |
| Resumos/cabeçalhos de escala | Tela, PDF e “minha escala” apresentam horários da base; as linhas da escala PWA continuam usando EC (`app/ponto/services/espelho.py:201`; `app/relatorios/pdf_espelho.py:730`; `app/templates/escala/minha_escala.html:33`; `app/escala/views.py:43`). |
| Caminho antigo `montar_grade_prevista`, sem mapa de DNA | Trabalho passa pelo EC, mas horários podem vir diretamente do template; existe chamador sem mapa (`app/escala/utils.py:760`; `app/escala/utils.py:767`; `app/escala/utils.py:1016`). |

Esses caminhos podem mostrar **horários incompatíveis com a célula** quando template/escala ativa e DNA/geradora divergem; não permitem afirmar que toda troca apareceria errada, pois o trabalho/folga passa pela célula e a própria troca gera o DNA do substituto a partir do template dele. Divergência concreta de um par de produção: **[nao verificado]** (`app/api/views.py:950`; `app/api/views.py:1611`; `app/ponto/portas/celula.py:591`; `app/ponto/management/commands/gerar_celulas.py:81`).

Fora desses consumidores principais, também há leitura direta da base na correção E/S retroativa, no propositor, na defesa de pergunta e no horário da cobrança “abriu e não bateu”; o efeito concreto deles sobre uma permuta é **[nao verificado]** (`app/api/views.py:619`; `app/api/views_core.py:612`; `app/ponto/propositor.py:334`; `app/chamados/juizes.py:1546`; `app/ponto/services/abriu_nao_bateu.py:193`).

4) **Os seis campos explícitos da edição são escritos por `editar_dia`, dentro da porta canônica da célula.** Gerar, regenerar e reverter são outras operações do mesmo módulo, não escritores externos (`app/ponto/portas/celula.py:600`; `app/ponto/portas/celula.py:609`; `app/core/portas.py:107`).

| Campo | Escrita e leitores encontrados |
|---|---|
| `trabalha` | Troca escreve `False/True`; outros atos da porta também o escrevem. Leitores: `EC.eh_dia_trabalho`, grade, cartório e detalhe do dia (`app/ponto/portas/celula.py:673`; `app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:788`; `app/ponto/portas/celula.py:849`; `app/escala/models.py:1096`; `app/escala/services/leitor_celula.py:381`; `app/ponto/services/cartorio.py:1408`; `app/ponto/services/dia_do_colab.py:91`). |
| `dna` | Produzido por `montar_dna`, gravado pela edição; gerar/regenerar/reverter também escrevem. Leitores: marcos, pausas, extra declarada, cartório e juiz de batida (`app/ponto/portas/celula.py:591`; `app/ponto/portas/celula.py:604`; `app/ponto/portas/celula.py:244`; `app/ponto/portas/celula.py:786`; `app/ponto/portas/celula.py:849`; `app/escala/models.py:1124`; `app/escala/models.py:1189`; `app/escala/models.py:1235`; `app/ponto/services/cartorio.py:1348`; `app/ponto/juiz_batida.py:158`). |
| `escala_geradora` | Edição escreve o vínculo escolhido para aquele dia; geração/regeneração/reversão também escrevem. O juiz `vinculo_do_dia` lê a geradora e exige sua vigência (`app/ponto/portas/celula.py:584`; `app/ponto/portas/celula.py:605`; `app/ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:789`; `app/ponto/portas/celula.py:849`; `app/escala/alimentacao.py:269`). |
| `origem` | Edição escreve `'editada'`; geração automática escreve `'gerada'`. Regeneração/reversão restringem-se a gerada; geração pula editada; vigias de template também filtram gerada (`app/ponto/portas/celula.py:606`; `app/ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:238`; `app/ponto/portas/celula.py:777`; `app/ponto/portas/celula.py:854`; `app/ponto/management/commands/tripwire_celulas.py:21`; `app/escala/management/commands/celulas_contra_o_template.py:29`). |
| `editada_em` | Edição escreve o instante atual. Leitores: carimbos de reescrita/cobrança órfã, fato novo do cartório, invalidação de perguntas, fase 12x36 e detalhe do dia (`app/ponto/portas/celula.py:598`; `app/ponto/portas/celula.py:607`; `app/escala/services/regeneracao.py:72`; `app/ponto/services/cartorio.py:365`; `app/chamados/veredito_lavrado.py:176`; `app/escala/services/fase_12x36.py:188`; `app/ponto/services/dia_do_colab.py:94`). |
| `editada_por` | Edição escreve `autor`; `_estado_da_edicao` lê para a trilha. Outro leitor de produção dessa identidade: **[nao verificado]** (`app/ponto/portas/celula.py:608`; `app/ponto/portas/celula.py:531`). |

Todos esses campos também são lidos para antes/depois e idempotência; se a célula não existir, a edição cria `colaborador/data`, e `gerada_em` nasce automaticamente. As autoridades consultam a célula por essa chave, e os relógios de invalidação leem `gerada_em` (`app/ponto/portas/celula.py:524`; `app/ponto/portas/celula.py:594`; `app/ponto/portas/celula.py:600`; `app/escala/models.py:1702`; `app/escala/models.py:1092`; `app/chamados/veredito_lavrado.py:178`).

A edição não escreve os campos de regeneração `regenerada_em/regeneracoes/dna_anterior`; tampou

[truncado em 20000 de 25142 caracteres]

## 09/11 11:12 · sessao be52b0625b15 · - · - · origem=Ronald (tmux)

Censo dos campos e leis (somente leitura).

A escrita da troca é indireta: `trocar_plantao` chama `editar_dia` uma vez por lado; o cedente recebe `trabalha=False`, o substituto `True` (`app/ponto/portas/celula.py:673-675`). A porta é o escritor declarado de `CelulaDia` (`app/core/portas.py:107-113`).

| Campo | Quem escreve | Quem lê encontrado |
|---|---|---|
| `CelulaDia.trabalha` | Na troca, `editar_dia`, create/save (`app/ponto/portas/celula.py:600-609`). Outros atos da mesma porta: regenerar (`:243-245`, `:259-276`), reverter (`:788-795`), gerar (`:849-871`). | Autoridade `EC.eh_dia_trabalho` (`app/escala/models.py:1087-1096`); leitor de grade (`app/escala/services/leitor_celula.py:381-411`); cartório (`app/ponto/services/cartorio.py:1408-1414`); detalhe do dia (`app/ponto/services/dia_do_colab.py:91`); trilha/idempotência (`app/ponto/portas/celula.py:528`, `:594`). |
| `CelulaDia.dna` | `editar_dia` constrói com `montar_dna(ec,dia,trabalha)` e grava (`app/ponto/portas/celula.py:591`, `:600-609`). Outros atos: regenerar (`:244-245`, `:256-276`), reverter (`:786-795`), gerar (`:849-871`). | `EC.marcos_do_dia` (`app/escala/models.py:1124-1133`), pausas (`:1189-1200`), extra declarada (`:1235-1238`); mapas de DNA (`app/escala/utils.py:46-53`, `:97`, `:130`); cartório/impressão/ata (`app/ponto/services/cartorio.py:1348`, `:1386`, `:1800`, `:1821`); juiz de batida (`app/ponto/juiz_batida.py:158`); detalhe do dia (`app/ponto/services/dia_do_colab.py:46`); trilha/idempotência (`app/ponto/portas/celula.py:528`, `:594`). |
| `CelulaDia.escala_geradora` | `editar_dia` grava o vínculo devolvido por `vinculo_do_dia_carregado` (`app/ponto/portas/celula.py:584`, `:600-609`). Regenerar (`:259-276`), reverter (`:789-795`), gerar (`:849-871`). | Autoridade do vínculo do dia: geradora tem precedência e deve cobrir a vigência (`app/escala/alimentacao.py:269-286`); trilha/idempotência (`app/ponto/portas/celula.py:529`, `:595`). |
| `CelulaDia.origem` | Troca grava `'editada'` em `editar_dia` (`app/ponto/portas/celula.py:600-609`); criação automática grava `'gerada'` (`:259-260`, `:849-852`). | Regeneração só reescreve gerada (`app/ponto/portas/celula.py:238`, `:263`), reversão só gerada (`:777`), geração pula editada (`:854-855`); tripwire só gerada (`app/ponto/management/commands/tripwire_celulas.py:21-22`); comparação com template só gerada (`app/escala/management/commands/celulas_contra_o_template.py:29`); detalhe do dia (`app/ponto/services/dia_do_colab.py:91`); trilha/idempotência (`app/ponto/portas/celula.py:528`, `:594`). |
| `CelulaDia.editada_em` | `editar_dia` (`app/ponto/portas/celula.py:598-609`). | Carimbos da reescrita/cobrança órfã (`app/escala/services/regeneracao.py:72-105`); detalhe do dia (`app/ponto/services/dia_do_colab.py:94`); fato novo do cartório (`app/ponto/services/cartorio.py:365`); invalidação de veredito de perguntas (`app/chamados/veredito_lavrado.py:176-180`); invalidação do censo de fase (`app/escala/services/fase_12x36.py:188-193`); trilha (`app/ponto/portas/celula.py:530`). |
| `CelulaDia.editada_por` | `editar_dia` (`app/ponto/portas/celula.py:600-609`). | Leitor explícito encontrado: `_estado_da_edicao`, para trilha (`app/ponto/portas/celula.py:531`). Outro leitor de produção da identidade desse campo: [nao verificado]. |
| `CelulaDia.colaborador/data` quando não existe célula | `editar_dia` cria a linha com a chave (`app/ponto/portas/celula.py:600`). | Seleção da própria edição (`:593`); autoridades consultam pela chave (`app/escala/models.py:1092-1093`, `:1122-1123`). |
| `CelulaDia.gerada_em` em criação | Automático do modelo (`app/escala/models.py:1702`), decorrente do create de `editar_dia` (`app/ponto/portas/celula.py:600`). | Invalidação das perguntas (`app/chamados/veredito_lavrado.py:178-179`) e fase (`app/escala/services/fase_12x36.py:190`). |

A edição não grava `regenerada_em/regeneracoes/dna_anterior`; isso é declarado e a lista de `update_fields` confirma (`app/ponto/portas/celula.py:538-539`, `:609`). O DNA do substituto usa o template do próprio vínculo, não o horário copiado do cedente (`app/ponto/portas/celula.py:510-511`, `:584-591`, `:635-637`).

Outros estados/trilha:

- Cada edição produz `LogAuditoria`: usuário, ação `editar_dia`, modelo `CelulaDia`, objeto, descrição, antes/depois dos seis campos, motivo e `apesar_da_lavra` (`app/ponto/portas/celula.py:611-618`). O par produz ação `trocar_plantao`, modelo `Ausencia`, objeto cedente, lados com colaborador/data/ausencia/documento/trabalha/resultado e motivo/override (`:682-691`). O escritor físico é `core.observ.evento`, via `evento_kw` (`app/core/observ.py:94-148`).
- Leitor genérico da trilha: `core.views.log_auditoria`, permissão `ver_saude_sistema`, filtro e leitura dos logs (`app/core/views.py:686-738`). A tela mostra usuário/ação/modelo/objeto/descrição/IP, sem renderizar o JSON antes/depois (`app/templates/core/log_auditoria.html:55-74`). Consumidor específico das ações `editar_dia/trocar_plantao`, fora a própria emissão, não encontrado na busca: [nao verificado].
- Cada lado marca cobranças órfãs (`app/ponto/portas/celula.py:625-626`): escreve `contexto_json.orfa_regeneracao={em,marcos_na_emissao}` pela porta `renovar_contexto` (`app/ponto/management/commands/gerar_celulas.py:241-255`). O julgamento dessa marca/reescrita é `cobranca_orfa`, alimentado por ambos os carimbos (`app/escala/services/regeneracao.py:87-105`, `:141-155`).
- O rabo da edição pode chamar julgamento do dia e recálculo financeiro (`app/ponto/portas/celula.py:627-628`, `:424-458`), além de rejunção e reavaliação de falta após commit (`:490-502`). Não confundir os seis campos explícitos da edição com os campos dos modelos alcançados por esses serviços (`:600-609`, `:424-502`).

Juiz:

- `JUIZES.md` é lookup e diz que registro vivo é `core/juizes.py` (`app/docs/JUIZES.md:8-14`). Lá estão dia de trabalho, minutos, precedência, ausência, status e folga (`app/core/juizes.py:170-193`), e geometria/marcos do turno (`:195-237`). Não encontrei juiz registrado para “este par pode trocar?”; esse predicado/registro é [nao verificado]. A função `trocar_plantao` é operação da porta, não aparece como juiz no registro (`app/ponto/portas/celula.py:81-82`; `app/core/juizes.py:170`).
- A tabela aponta permissão de tela para `tem_acao` e contextual para `pode_agir_chamado` (`app/docs/JUIZES.md:48-49`); a troca só exige `autor.pk` e motivo, sem chamar essas autoridades (`app/ponto/portas/celula.py:646-649`, `:652-691`).

Leis já escritas (não transformar em perguntas novas):

- Par, folga sem desconto, assumido normal com AN/intra sem HE100, duas cartas, confirmação pelo admin, pedido por qualquer lado: `app/docs/CORTES.md:187`.
- Escritor humano na mesma porta, origem editada, trilha, duas chamadas no mesmo atomic e exportada barrada: aval posterior O10-B literal em `app/docs/PROMPTS.md:1901`, também `app/docs/RELATO.md:480`; casos registram o literal em `app/ponto/tests/test_o10b_editar_dia_troca.py:4-9`.
- L-113 genérica revogou guarda exportada como trava (`app/docs/LEIS.md:151`), mas o aval O10-B de 06/11 explicitamente manda exportada barrada (`app/docs/PROMPTS.md:1901`). Convém citar ambos sem inventar nova decisão.
- Quem fornece lado substituto no SOLO: admin, nunca inferido; SOLO aplica zero pares; competência 09 só com `!` após DRY (`app/docs/PROMPTS.md:1907`; `app/docs/RELATO.md:431`).
- Fonte do dia/marcos é célula; template é gerador e leitores usam EC (`CLAUDE.md:145-154`).
- Autor/motivo/trilha em ação crítica e escritor único já estão escritos (`CLAUDE.md:32-34`).

Lacuna de lei expressamente registrada: “troca que cai em folga trabalhada pede decisão do admin?” (`app/docs/RELATO.md:449`; `app/docs/BACKLOG.md:1876`). Não reduzir o número final só às lacunas novas sugeridas: essa pergunta já consta como aberta.

Para agrupamento das demais perguntas do usuário, as decisões específicas de recusa de par por interjornada/sobreposição, empresas/vínculos diferentes, datas iguais/diferentes e tipo/status/período das cartas não aparecem na porta (`app/ponto/portas/celula.py:632-700`). Os 11h são mencionados como “NÃO valida” em outra função de cobertura (`app/ponto/utils.py:152-155`), o que comprova referência existente, não uma lei de elegibilidade da permuta. “Qual regra deve valer?” nesses pontos é [nao verificado].

## 09/11 11:12 · sessao e6a1ea29d182 · - · - · origem=Ronald (tmux)

Item 3 — censo dos leitores:

| Leitor | Fonte comprovada |
|---|---|
| Espelho admin | A view chama `espelho_do_colab` (`app/ponto/views.py:157-158`); o previsto na competência vem de `grade_da_celula` (`app/ponto/services/espelho.py:506-520`). |
| Espelho app | Chama a mesma autoridade `espelho_do_colab` (`app/api/views.py:1379-1385`), mas os dias sem card buscam os horários diretamente no template (`app/api/views.py:1611-1614`). |
| PDF espelho | Busca os dias em `espelho_do_colab` (`app/relatorios/pdf_espelho.py:596-598`), porém a coluna **Previsto** usa `tipo_escala.hora_inicio/hora_fim`, imprimindo-os em cada linha com turnos (`app/relatorios/pdf_espelho.py:903-909`, `:978`). |
| Calendário admin | Consulta `grade_da_celula` (`app/colaboradores/services/calendario.py:332-334`), lê status por `status_dos_dias_lavrados` (`:602-607`) e dinheiro por `DiaPago` (`:290`, `:655-656`). A view consumidora está em `app/colaboradores/views.py:1945-1946`. |
| Calendário/escala API app | `/api/escala/v2/` registrado em `app/api/urls.py:72`; trabalho/folga vem de `escala.eh_dia_trabalho(data)`, porém horários vêm de `te.marcos_do_dia(data)` (`app/api/views.py:934-955`), publicados em `:968-971`. |
| Escala web/PWA | Usa ambos `ec.eh_dia_trabalho` e `ec.marcos_do_dia` (`app/escala/views.py:38-44`); a agenda semanal administrativa também usa ambos (`app/colaboradores/views.py:2223`, `:2232-2234`). |
| Folha/motor | O fechamento consulta `esc.eh_dia_trabalho` (`app/ponto/services/fechamento.py:729-732`), alimenta o motor com células (`:750`) e chama `apurar_por_fatia` (`:754-755`); esta passa `escala_colaborador=esc` ao motor (`:278-290`). O motor prefere `EC.marcos_do_dia` (`app/ponto/motor_calculo_v2.py:2290-2291`) e pergunta `EC.eh_dia_trabalho` (`:2447-2448`). |
| TXT | As verbas são lidas de `FechamentoMensal`, sem novo motor (`app/folha/export.py:703-724`); retenção consulta `CelulaDia` (`:1206`, `:1233-1256`); `grade_do_fechamento` delega a `grade_da_celula` (`:329-338`). Datas de falta vêm de ausências cujo efeito vigente desconta, não de template (`:169-185`). |

As autoridades realmente passam pela célula: `EC.eh_dia_trabalho` retorna `cel.trabalha` (`app/escala/models.py:1087-1096`); `EC.marcos_do_dia` lê `cel.dna.marcos` antes do fallback de template (`:1117-1133`). `grade_da_celula` carrega `CelulaDia` e lê sua ata/trabalha (`app/escala/services/leitor_celula.py:345-381`).

**Leitores diretos da base encontrados nos caminhos pedidos:**

- Calendário API app: horários dos dias via template (`app/api/views.py:955`); o trabalho/folga continua passando pela célula (`:950`).
- Espelho app, dias sem card: horários via template (`app/api/views.py:1613`, `:1639-1642`); trabalho/folga passa pela célula (`:1611`).
- Bloco escala de `/api/me/`, nas duas cascas: horários via template (`app/api/views.py:259-274`; `app/api/views_core.py:248-263`).
- Coluna Previsto do PDF: campos crus do template, não DNA (`app/relatorios/pdf_espelho.py:903-909`, `:978`).
- Cabeçalhos da escala: resumo da tela usa horas cruas do template (`app/ponto/services/espelho.py:201-212`, `app/templates/ponto/espelho.html:142`); PDF idem (`app/relatorios/pdf_espelho.py:730-740`); “minha escala” apresenta o padrão no cabeçalho (`app/templates/escala/minha_escala.html:33-34`), mas os horários das linhas vêm do EC (`app/escala/views.py:43-44`, `app/templates/escala/minha_escala.html:78-81`).
- Na degradação antiga, `montar_grade_prevista` consulta trabalha pelo EC (`app/escala/utils.py:760-762`), mas pode consultar horas no template sem mapa de DNA (`:767-768`); há chamador sem mapa em `app/escala/utils.py:1016`. O caminho normal `grade_espelho_janela` usa o builder de período (`:2337`), que consulta trabalha e mapa DNA antes do fallback (`:1605-1607`).

**Mostrariam errado?** Não há prova de divergência concreta do par real: **[nao verificado]**. Os caminhos diretos podem publicar horários diferentes da célula quando o template/escala ativa difere do DNA/geradora do dia; os sítios acima demonstram essa separação. Não é correto afirmar que ignoram toda a permuta: os caminhos do app consultam `trabalha` pela célula (`app/api/views.py:950`, `:1611`, `:260`).

Também não se pode assumir que o substituto receberia as horas do cedente: a porta atual faz o substituto trabalhar com o **próprio template** (`app/ponto/portas/celula.py:635-637`), chama `montar_dna` (`:591`), que congela os marcos de `ec.tipo_escala` naquela data (`app/ponto/management/commands/gerar_celulas.py:73-84`). Portanto, logo após a edição e sem diferença entre template atual e geradora, usar template pode coincidir com o novo DNA; o dano efetivo depende de dados não fornecidos **[nao verificado]**.

Outros leitores diretos encontrados, fora dos cinco consumidores principais: correção E/S retroativa nas duas APIs (`app/api/views.py:619-625`, `app/api/views_core.py:612-617`); `dia_posicional_completo` (`app/ponto/propositor.py:334-343`); defesa de pergunta `_marco_esperado_da_grade` (`app/chamados/juizes.py:1546-1563`); horário da cobrança “abriu e não bateu” (`app/ponto/services/abriu_nao_bateu.py:193-203`). Não confundir com fallbacks que primeiro consultam a célula: `veredito_celula._marcos_do_dia` (`app/ponto/services/veredito_celula.py:29-43`) e `marcos_turno` quando recebe `ec` (`app/escala/servico_jornada.py:68-71`).

## 09/11 11:11 · sessao d51be0d9df52 · - · - · origem=Ronald (tmux)

Item 1 — há canais genéricos para o colaborador pedir; não encontrei modelo/endpoint próprio que registre **o par de permuta** estruturado com dois colaboradores e duas datas. A conclusão negativa deve ficar como “[nao verificado] modelo próprio de pedido de permuta”; o que está comprovado é o modelo genérico `Ausencia`, sem campos de par, substituto ou dia assumido (`app/ponto/models.py:743-814`), e a solicitação textual em `ChamadoColaborador` (`app/chamados/models.py:38-52`).

**1) Ausência enviada pelo app/PWA**

- App: `GET/POST /api/ausencias/` → `api_ausencias`; autenticado e vinculado ao colaborador do usuário (`app/api/urls.py:82`; `app/api/views.py:1700-1706`).
- PWA: `/ponto/ausencias/submeter/` → `submeter_ausencia`, formulário `ponto/submeter_ausencia.html` (`app/ponto/urls.py:64`; `app/ponto/views.py:2466-2468`; `app/ponto/views.py:2560-2564`).
- Entrada: `tipo`, `data_inicio`, `data_fim`, `descricao`, documento e minutos; chama `criar_ausencia` com `registrado_por=request.user` (`app/api/views.py:1736`; `app/api/views.py:1788-1791`; `app/api/views.py:1830-1842`; `app/ponto/views.py:2476-2480`; `app/ponto/views.py:2536-2542`).
- Tipo é validado no catálogo vivo/cadastro `TipoAusencia`; a API também oferece as choices vigentes. Portanto, a disponibilidade de `troca_de_plantao` no app depende do cadastro que esta cópia não demonstra — **[nao verificado] no dado fornecido** (`app/ponto/services/ausencia.py:193-195`; `app/api/views.py:1883`; `app/ponto/catalogo/ausencias.py:17-23`). A lista estática de `Ausencia.TIPO_CHOICES` não contém `troca_de_plantao` (`app/ponto/models.py:661-679`).
- Campos persistidos: colaborador, tipo, data/data_fim, minutos/dias corridos, horários de cobertura parcial, hora_extra_100, descrição, documento, prazo_documento, chamado_origem, documento_referencia, status, registrado_por, aprovado_por/aprovada_em e timestamps (`app/ponto/models.py:743-814`).
- Estados: `aguardando_documento`, `aguardando_decisao`, `aprovada`, `rejeitada` (`app/ponto/models.py:701-705`). Na criação, `criar_ausencia` chama `estado_inicial`: com documento ou tipo que não exige documento → aguarda decisão; tipo que exige sem documento → aguarda documento (`app/ponto/services/ausencia.py:331-345`; `app/ponto/services/ausencia.py:790-797`).
- Escrita posterior do status é centralizada em `mudar_estado` (`app/ponto/services/ausencia.py:808-835`). `anexar_documento` muda de aguardando documento para aguardando decisão (`app/ponto/services/ausencia.py:846-875`); `decidir` aceita aprovada/rejeitada, como ato DP (`app/ponto/services/ausencia.py:897-927`); escalonamento de documento vencido também passa a aguardando decisão, sem declarar falta (`app/ponto/services/ausencia.py:992-1006`).
- App e PWA abrem chamado genérico `ausencia_colab` após criar a ausência (`app/api/views.py:1871-1874`; `app/ponto/views.py:2546-2547`). Esse chamado é aviso persistente ao DP, não decide a ausência; guarda `ausencia_id`, data, tipo e presença de documento em contexto JSON (`app/ponto/services/ausencia.py:637-644`; `app/ponto/services/ausencia.py:659-678`).

**Leitores comprovados dessas Ausencias**

- App GET devolve últimas 30 do próprio colaborador, incluindo status/descrição/minutos (`app/api/views.py:1876-1895`).
- Admin `/ponto/ausencias/`: exige `ver_ausencias`, consulta vigentes ou registro específico e filtra tipo/status; botões/endpoints de aprovar/rejeitar/anexar/editar existem (`app/ponto/views.py:2155-2187`; `app/ponto/urls.py:53-76`).
- API mensageria/DP lê `ausencias_do_dp`, conta por tipo/status e lista aguardando decisão (`app/api/views_mensageria.py:736-762`).
- Comando `escalonar_documentos_ausencia` chama o escalonador (`app/ponto/management/commands/escalonar_documentos_ausencia.py:25-26`); o escalonador lê fila aguardando documento e pode escrever aguardando decisão (`app/ponto/services/ausencia.py:992-1006`). **Execução atual desse cron em produção: [nao verificado].**
- Fechamento do aviso: `chamados_da_ausencia_decidida` seleciona `ausencia_colab` vivos e consulta ausência associada; `fechar_chamado_da_ausencia` reconcilia quando ausência sai dos estados abertos (`app/ponto/services/ausencia.py:686-716`). O lookup de crons declara `fechar_chamados_ausencia` a cada 30 minutos, mas execução real não é comprovada pelo código (`app/docs/CRONS.md:25`).
- Signals de Ausencia existem como consumidores em save/delete (`app/ponto/signals.py:144`; `app/ponto/signals.py:151`; `app/ponto/signals.py:250`; `app/ponto/signals.py:276`; `app/ponto/signals.py:283`).

**2) Solicitação textual / chamado**

- Existe taxonomia `Escala / Turno` com subcategorias “Troca de turno”, “Folga”, “Plantao” e palavras substituto/substituir (`app/chamados/classificador.py:38-49`).
- App `POST /api/chamados/abrir/` → `api_abrir_chamado`: aceita assunto, descrição, urgência, área, categoria e subcategoria; grava `ChamadoColaborador` com `modulo_origem='app_colaborador'` e nota da abertura (`app/api/urls.py:87`; `app/api/views.py:2151-2177`; `app/api/views.py:2201-2214`).
- Web/PWA `/chamados/abrir/` → `abrir_chamado`, chama `abrir_chamado_local` e renderiza `chamados/form_abertura.html` (`app/chamados/urls.py:10`; `app/chamados/views.py:147`; `app/chamados/views.py:196-215`).
- Não é modelo novo chamado “Solicitacao”: rótulo “Solicitação” decorre de `modulo_origem in ('chamados','app_colaborador')` (`app/chamados/rotulos.py:4-5`; `app/chamados/rotulos.py:26-38`).
- Campos principais: colaborador, assunto/título/descrição, área/categoria/subcategoria, módulo, contexto JSON, célula opcional, status_local, urgência, respondente/resolução, atribuição e SLA (`app/chamados/models.py:38-85`).
- Estados genéricos: aberto/em_analise/resolvido/fechado/cancelado/registrado/superado (`app/chamados/models.py:28-35`). Nascimento passa por `ChamadoColaborador.criar` e motor; escritor central posterior é `_gravar_estado` (`app/chamados/models.py:1054-1067`; `app/chamados/models.py:1093-1099`; `app/chamados/models.py:201-228`). Gestor muda estado por `transicionar` no serviço de ações (`app/chamados/services/acoes_chamado.py:313-319`).
- Leitores: painel DP consulta `ChamadoColaborador` (`app/chamados/services/painel.py:96-115`); “Meu Atendimento” separa solicitações e pendências e passa ao template (`app/chamados/views.py:612-642`); API de chamados carrega chamados visíveis do colaborador (`app/api/views.py:1031-1039`). A criação envia push para supervisão ou DP conforme área, com link painel (`app/api/views.py:2216-2228`).
- A apresentação da nova solicitação no PWA é controlada por parâmetro empresarial `app_solicitacoes`, desligado por padrão; quem tem solicitação já existente ainda vê aba (`app/chamados/services/solicitacoes_no_app.py:1-20`; `app/chamados/views.py:615-642`). **Valor real desse parâmetro: [nao verificado].**
- Leitor automático genérico de chamados: comando `alertar_chamados_sla` lê fila e registra ocorrência/push de SLA (`app/chamados/management/commands/alertar_chamados_sla.py:20`; `app/chamados/management/commands/alertar_chamados_sla.py:44-55`); execução real **[nao verificado]**.

**3) Pergunta**

Existe resposta a `PerguntaDisputa` pelo app, mas ela depende de pergunta existente da disputa do próprio colaborador; não é endpoint para abrir pedido estruturado de permuta (`app/api/views.py:2616-2619`; `app/api/views.py:2643-2655`). GET de perguntas usa `perguntas_do_app` e estados da pergunta (`app/api/views.py:2565-2568`; `app/api/views.py:2583-2585`). **Uma resposta específica de permuta materializar o par: [nao verificado].**

Não importar as contagens antigas de produção presentes em comentários/RELATO como dado atual: o anexo fornecido não contém registros de pedidos, tipos cadastrados ou flags por empresa — **[nao verificado]**.

## 09/11 10:59 · sessao 6efcaa755ca3 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

Implementei P1/P4/P5 e escrevi primeiro os REDs, incluindo P2/P3 reservados à sessão. **RED executado, GREEN, mortes dos mutantes e resultado em produção: [nao verificado].**

**Entrega por hunk**

| Arquivo:linha | Alteração |
|---|---|
| [materializacao.py:109](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/services/materializacao.py:109) | Espera exige a própria metade validada. |
| `chamados/services/materializacao.py:175` | Vizinhança usa `batidas_apuraveis`, autoridade que exclui retratadas (`ponto/turnos.py:2866`). |
| [acoes_chamado.py:348](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/services/acoes_chamado.py:348) | Detalhe inclui C6 viva **ou** metade em espera pelo mesmo predicado. |
| [test_bug_par_intervalo.py:164](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/test_bug_par_intervalo.py:164) | P1: S retratada 01:30; valida S01/E02 e cobra alternância canônica, carimbos e fechamento. |
| `chamados/tests/test_bug_par_intervalo.py:457` | Foto de estado/trilha; P2 cobra zero saves, eventos e agendamentos em duas chamadas (`:478`); P3 chama `fechar` real e exige consulta C7 após trava (`:497`). |
| `chamados/tests/test_bug_par_intervalo.py:528` | P4: duas respondidas sem aceite; explicador, declaração do colaborador e aplicação pelo administrador (`:543`, `:556`). |
| `chamados/tests/test_bug_par_intervalo.py:623` | P5: GET real do detalhe e modal, sem contexto injetado; cobra include e aviso HTML (`:627`, `:631`). |
| `chamados/tests/test_bug_par_intervalo.py:702` | P6: carimbo inicialmente vazio; primeira falha cobra 4 saves, 4 agendamentos e 2 eventos; segunda cobra zero novas escritas e foto idêntica (`:726`, `:743`). |
| [mutantes_bug_par_intervalo.py:131](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/mutantes_bug_par_intervalo.py:131) | M38 restaura a guarda exata da volta 2; acrescentados M39–M44. |
| `chamados/tests/mutantes_bug_par_intervalo.py:205` | Inclui `acoes_chamado.py` no `py_compile`; `--comandos-v4` imprime os comandos individuais com settings CI (`:226`). |

Os novos testes usam a classe `TestCase`, relógio congelado e fixture `col960` existentes (`chamados/tests/test_bug_par_intervalo.py:32`, `:45`).

**Quem escreve e quem lê**

| Campo/função | ESCRЕVE / dono | LÊ, inclusive fora do diff |
|---|---|---|
| Espera do par | Predicado sem escrita: `materializacao.py:101`. | C7 `chamados/models.py:1864`; explicador `explicador.py:74`; declaração `declaracao_texto.py:213`, `:382`, `:678`; validação `validacao.py:193`, `:483`; vigia `vigia_carimbo.py:109`; tag `chamados_tags.py:19`. |
| Vizinhança / universo | Consulta sem escrita: `materializacao.py:169`; autoridade `ponto/turnos.py:2846`. | Aplicação individual `materializacao.py:962`; julgamento posterior do par `:1364`; apresentação `:539`; reexport `chamados/models.py:4080`. |
| `perguntas_disputa` | Contexto do detalhe `acoes_chamado.py:349`; contexto independente do modal `fio.py:253`. | View `chamados/views.py:549`; detalhe `templates/chamados/detalhe_local.html:309`; modal `templates/chamados/partials/modal_fio.html:156`. |
| `validada_em/por` | Aceite `validacao.py:134`; devolução `:202`; nova resposta `disputa_emissao.py:1477`; complemento pelo carimbo `chamados/models.py:3138`. | Espera `materializacao.py:109`; C6 `chamados/juizes.py:1153`; C7 `chamados/models.py:1850`. |
| `materializada_em/por`, `via_resolucao` | `_escrever_carimbo`, `chamados/models.py:3100`; descarimbo `:3211`; lote `:3269`; relavra `relavra_via.py:125`. | Lastro `chamados/juizes.py:660`; C6 `:1164`; devolução `validacao.py:188`; lavra persistida `veredito_lavrado.py:187`. |
| `Batida.retratada_em` | Retração `ponto/registro_batida.py:276`, por `anotar` (`:255`). | Universo canônico `ponto/turnos.py:2866`; tolerância `ponto/registro_batida.py:99`; lastro `ponto/services/lastro.py:262`. |
| Fotos e definições dos testes | Fixtures/helpers `test_bug_par_intervalo.py:81`, `:88`, `:472`, `:767`. | Asserções `:495`, `:526`, `:745`; alvos do runner `mutantes_bug_par_intervalo.py:131`. | [nao verificado]
| `MUTANTES` / fontes | Definições `mutantes_bug_par_intervalo.py:131`. | Leitura/recompilação em memória `:166`, `:179`, `:249`; conferência estática `:217`. |

O save do descarimbo agenda beco e pode emitir evento pelos receivers (`chamados/signals.py:225`, `:247`; `chamados/models.py:3252`). Exaustividade transitiva deste mapa: **[nao verificado]**.

**Linhas exatas para a SESSÃO escrever em `chamados/models.py`**

P2: após o retorno de `fora_do_fato`, antes de `_escrever_carimbo` (`chamados/models.py:3209`, `:3211`):

```python
        if (self.materializada_em is None
                and self.materializada_por_id is None
                and self.via_resolucao == ''):
            return []
```

P3: antes do `if motivo is None` (`chamados/models.py:1908`):

```python
        pelo_juiz = motivo is None
```

Após a chamada completa de `trava_na_ordem` (`chamados/models.py:1923`):

```python
            if pelo_juiz and not self.pode_fechar():
                raise ValueError(
                    'Nao e possivel fechar a disputa #%s: C7 reteve o fechamento.' % self.pk)
```

É necessário conservar a modalidade original porque `motivo` recebe `FECHOU_VAZIA` ou `FECHOU_VEREDITO` antes da trava (`chamados/models.py:1914`).

**Mutantes e asserções discriminantes previstas**

| Mutante | Regressão | Asserção que deve matar |
|---|---|---|
| M38 | Guarda exata v2 | Ambas passam por `descarimbar` mesmo inicialmente vazias (`test_bug_par_intervalo.py:724`). |
| M39 | Universo cru inclui retratada | Segunda validação aceita o par (`:176`). |
| M40 | Espera sem validação própria | Predicado falso para metade apenas respondida (`:533`). |
| M41 | Detalhe volta a filtrar só C6 | Metade em espera presente no contexto GET (`:641`). |
| M42 | Aplicação retorna antes de validar | Própria metade recebe validação (`:566`). |
| M43 | Declaração prematuramente em espera | Retorna `ja_declarada`, aguardando supervisão (`:550`). |
| M44 | Tag omite espera no modal | Aviso HTML presente uma vez no GET real (`:642`). |

M38 cobra a passagem pela porta; após P2, comparar somente estado/trilha não distingue a guarda antiga que pula um no-op (`test_bug_par_intervalo.py:724`, `:745`). Mortes executadas: **[nao verificado]**.

Comandos individuais reservados ao Code; equivalentes aos impressos por `--comandos-v4` (`mutantes_bug_par_intervalo.py:226`):

```bash
export DJANGO_SETTINGS_MODULE=config.settings.ci
export PYTHONDONTWRITEBYTECODE=1

teste() { [nao verificado]
  python3 manage.py test "chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.$1" --noinput
}
mutante() {
  python3 -m chamados.tests.mutantes_bug_par_intervalo "$1"
}

teste test_V4_P1_par_aplica_com_S_retratada_entre_metades [nao verificado]
teste test_V4_P2_descarimbar_vazio_duas_vezes_nao_escreve_nem_agenda [nao verificado]
teste test_V4_P3_fechar_real_consulta_C7_sob_trava_e_retem_espera [nao verificado]
teste test_V4_P4_respondidas_sem_aceite_aguardam_validacao [nao verificado]
teste test_V4_P4_declaracao_colab_sem_aceite_aguarda_supervisao [nao verificado]
teste test_V4_P4_aplicar_declaracao_valida_a_propria_metade [nao verificado]
teste test_V4_P5_GET_detalhe_conserva_metade_em_espera [nao verificado]
teste test_V4_P5_GET_modal_conserva_metade_em_espera [nao verificado]
teste test_V4_P6_falha_com_carimbo_vazio_duas_vezes_mesmo_estado_e_trilha [nao verificado]

mutante M38
mutante M39
mutante M40
mutante M41
mutante M42
mutante M43
mutante M44
```

Executei somente a conferência estática abaixo: **rc 0**, `py_compile` dos quatro Python tocados sem persistência e sintaxe/âncoras M1–M44 conferidas (`mutantes_bug_par_intervalo.py:195`, `:211`, `:217`). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo --sintaxe
```

**DECISAO-ABERTA:** P2/P3 aguardam escrita da sessão; P6 depende de P2. A inserção proposta consulta C7 sob trava, mas as leituras anteriores de respostas/perguntas permanecem antes dela (`chamados/models.py:1897`, `:1906`, `:1921`). Prova concorrente, GREEN e certificação pelo Code/opus: **[nao verificado]**.

## 09/11 10:57 · sessao 476fb91835ff · - · - · origem=Ronald (tmux)

Considerando os novos comandos com `config.settings.ci`, não encontrei bloqueador concreto dos GETs por inspeção: `ci.py:5` seleciona `config.urls_ci`, `config/urls_ci.py:9` inclui as rotas de chamados e os middlewares de sessão/autenticação/mensagens vêm de `config/settings/base.py:47`, `:50`, `:55`. O administrador da fixture tem `is_staff=True` e `is_superuser=True` (`chamados/tests/test_bug_par_intervalo.py:41`), aceitos pelo gestor e por RBAC (`chamados/views.py:35`; `colaboradores/mixins.py:328`). Execução permanece [nao verificado].

O modal recebe perguntas de disputas abertas sem filtro C6 (`chamados/services/fio.py:221`), associa a disputa em `:258` e o template entra pelo mapa real de permissão (`templates/chamados/partials/modal_fio.html:122`). O detalhe passa pelo contexto real da view (`chamados/views.py:549`) e pelo include em `templates/chamados/detalhe_local.html:310`. Os novos testes não injetam contexto (`chamados/tests/test_bug_par_intervalo.py:633`).

As contagens do P6 correspondem ao caminho inspecionado **depois da cura P2**:

- Duas recusas salvam apenas `recusa_motivo` (`chamados/services/materializacao.py:1195`), cada uma agenda um beco (`chamados/signals.py:225`) e não emite outbox porque esse campo não pertence a `CAMPOS_QUE_ACORDAM` (`chamados/models.py:3224`; `chamados/signals.py:245`).
- Duas devoluções salvam validação (`chamados/services/validacao.py:205`), agendam outros dois becos e emitem os dois eventos (`chamados/signals.py:225`, `:247`; `chamados/models.py:3252`).
- A segunda recusa idêntica não salva (`chamados/services/materializacao.py:1183`, `:1192`); validação já vazia não salva (`chamados/services/validacao.py:198`) e foto idêntica não audita (`:222`). As notas também dependem da mudança do motivo (`chamados/services/materializacao.py:1195`).

O registro de trava em `on_commit` não deve inflar a primeira contagem: `_travar_ato_materializacao` está antes da captura (`chamados/tests/test_bug_par_intervalo.py:714`), e `_registrar_travados` não adiciona novamente colaborador já registrado (`chamados/services/trava_do_colaborador.py:42`). Resultado executado: [nao verificado].

Única lacuna adicional do P6: o teste configura a falha fixa, mas não cobra que `_materializar_resposta_hora` foi realmente alcançada; uma recusa anterior estável poderia satisfazer parte das mesmas asserções (`chamados/tests/test_bug_par_intervalo.py:719`). Capturar esse mock e cobrar uma chamada por tentativa reforça a origem pretendida da falha. [nao verificado]

## 09/11 10:53 · sessao 476fb91835ff · - · - · origem=Ronald (tmux)

Mapa por leitura estática:

| Função/campo | Escritor/dono | Leitores fora do diff |
|---|---|---|
| `aguarda_outra_metade_intervalo` | Predicado sem escrita: `chamados/services/materializacao.py:101`. | C7 `chamados/models.py:1864`; explicador `chamados/services/explicador.py:74`; declaração `chamados/services/declaracao_texto.py:213`, consumida em `:382`, `:485`, `:678`; validação/devolução `chamados/services/validacao.py:193`, `:483`; vigia `chamados/services/vigia_carimbo.py:109`; tag `chamados/templatetags/chamados_tags.py:19`; estado de aplicação `chamados/services/materializacao.py:527`. |
| `_vizinha_mesmo_tipo` | Consulta sem escrita: `chamados/services/materializacao.py:169`. | Estado da aplicação `chamados/services/materializacao.py:542`; aplicação individual `:965`; par posterior `:1367`; reexport em `chamados/models.py:4080`. |
| `batidas_apuraveis` | Autoridade sem escrita, filtro de retratação em `ponto/turnos.py:2866`. | Espelho `ponto/services/espelho.py:1022`; turno órfão `ponto/utils_turno_orfao.py:28`; diagnóstico `ponto/services/diagnostico_escala.py:142`; fio `chamados/services/fio.py:1521`, `:1881`; validação `chamados/services/validacao.py:896`; consultas em lote `ponto/turnos.py:2989`, `:3009`. |
| Contexto `perguntas_disputa` do detalhe | `chamados/services/acoes_chamado.py:348`, publicado em `:409`. | View GET `chamados/views.py:549`; loop `templates/chamados/detalhe_local.html:309`; include `:310`. Modal possui escritor próprio em `chamados/services/fio.py:253`, consumido por `templates/chamados/partials/modal_fio.html:156`. |
| `validada_em/por` | Aceite `chamados/services/validacao.py:134`; devolução `:202`; nova resposta `chamados/services/disputa_emissao.py:1477`; reabertura em lote `:1590`; carimbo completando validação `chamados/models.py:3138`; reabertura da disputa `:1565`. | C6 `chamados/juizes.py:1153`; foto da validação `chamados/services/validacao.py:147`; crédito por autor `chamados/services/verbos.py:240`; C7 `chamados/models.py:1850`. |
| `materializada_em/por/via_resolucao` | Porta comum `_escrever_carimbo`, `chamados/models.py:3100`–`:3104`; descarimbo `:3211`; lote `:3269`; via por fato `:3278`; relavra `chamados/services/relavra_via.py:125`. | Lastro `chamados/juizes.py:660`, `:672`, `:687`; C6 `:1164`, `:1190`; devolução `chamados/services/validacao.py:188`; lavra persistida `chamados/veredito_lavrado.py:187`. |
| `Batida.retratada_em` | Retração pela porta `ponto/registro_batida.py:276`, escrita efetiva por `anotar` em `:255`; desfazer humano por `anotar` em `ponto/services/retratar_humano.py:207`. | Universo canônico `ponto/turnos.py:2866`; tolerância `ponto/registro_batida.py:99`; lastro `ponto/services/lastro.py:262`; vigia de carimbo `chamados/services/vigia_carimbo.py:119`. |

Efeitos do `save` de descarimbo: agendamento pelo receiver `chamados/signals.py:215`–`:225`; emissão por `chamados/signals.py:242`; resolução da célula e chamada da porta em `chamados/models.py:3235`–`:3252`. Ausência de deduplicação nessa emissão consta da porta `ponto/portas/celula.py:891`.

Texto mínimo para a SESSÃO, sem edição realizada:

P2: inserir entre o retorno de `fora_do_fato` e `_escrever_carimbo`, em `chamados/models.py:3211`:

```python
        if (self.materializada_em is None
                and self.materializada_por_id is None
                and self.via_resolucao == ''):
            return []
```

P3: guardar a modalidade original antes do `if motivo is None` de `chamados/models.py:1909`:

```python
        pelo_juiz = motivo is None
```

Após a chamada completa de `trava_na_ordem`, em `chamados/models.py:1923`, inserir:

```python
            if pelo_juiz and not self.pode_fechar():
                raise ValueError(
                    'Nao e possivel fechar a disputa #%s: C7 reteve o fechamento.' % self.pk)
```

A flag precisa conservar a entrada original: antes da trava, o código já converte `motivo=None` em `FECHOU_VAZIA` ou `FECHOU_VEREDITO`; verificar `motivo is None` somente depois disso tornaria a guarda inoperante. (`chamados/models.py:1916`)

Essa inserção consulta C7 sob trava, mas as leituras anteriores de `_pend`, `perguntas` e `vivas` permanecem anteriores à trava; proteção integral dessas leituras exigiria movê-las para dentro do bloco após a trava. (`chamados/models.py:1897`, `:1907`, `:1921`)

Exaustividade transitiva do mapa: [nao verificado]. Nenhum teste, sintaxe, banco, rede ou escrita foi executado por este explorer. [nao verificado] [nao verificado]

## 09/11 10:52 · sessao f14c3a354d98 · SONDA-SEM-GRANT · verificador · origem=Code

**Não fecharia a volta 4:** permanecem caminhos que conservam carimbo verde após alteração da sombra e uma limpeza que pode remover container alheio por colisão de nome. (bin/sombra.sh:214,225,228,430,433; bin/tests/test_sonda_sem_grant_pg.sh:41,50,93,95)

1. **Itens PROCEDENTES da triagem v3**

| Item | Fechamento e mordida |
|---|---|
| **2a — fechado; NÃO PROCEDE reabrir a lacuna original.** | SUPERUSER é aplicado; o selo exige `super>0`, rc 5, recusa nomeada e sonda não executada. O log registra precisamente essas falhas para `super=0`. (bin/tests/test_sonda_sem_grant_pg.sh:183,185,186,187,207; anexo sonda_v4_mut.log:11) |
| **2b — fechado para `$PGU → leitor`; NÃO PROCEDE reabrir esse caso.** | O estrago de schema grantable passa pelas mesmas cobranças; a porta agora revoga esse default. O mutante da medição morre na detecção; retirar o REVOKE morre na recuperação pela porta. (bin/tests/test_sonda_sem_grant_pg.sh:183,189,208; bin/sonda_provisionar.sh:132; anexo sonda_v4_mut.log:16,31) |
| **4 — guarda original fechada; residuais abaixo são PROCEDENTES.** | `carimbar` verifica as operações e `refazer` exige invalidação antes do drop. O log mata ausência da guarda e retorno do `mv` ignorado pela asserção “chamou dropdb”. (bin/sombra.sh:94,95,98,99,101,275,276; bin/tests/test_sonda_sem_grant.sh:408; anexo sonda_v4_mut.log:1,26) |
| **6 — fechado no pedido literal; NÃO PROCEDE reabrir invalidado/ontem.** | Placar retorna `?` nesses casos; invalidação também apaga INCOMPLETA. O log registra a comparação esperada matando a leitura antiga. (bin/placar_code.sh:37; bin/sombra.sh:110; bin/tests/test_sonda_sem_grant.sh:434,436,439; anexo sonda_v4_mut.log:6) |
| **10 — silêncio original corrigido; identidade ainda PROCEDENTE.** | O selo cobra rc preservado, falha nomeada e `rm -f` exato; o log mata a implementação antiga nessas cobranças. Isso não demonstra autoria do container removido. (bin/tests/test_sonda_sem_grant_pg.sh:40,44,48,76,80,82; anexo sonda_v4_mut.log:21) |

O log fornecido registra sete mortes compatíveis com as asserções indicadas; patches exatos dos mutantes, comandos completos, baseline e reexecução independente são **[nao verificado]**. (anexo sonda_v4_mut.log:1,6,11,16,21,26,31)

**PROCEDENTE — ressalva no selo 9c:** no caso `mv`, o STAMP aponta para `sem_dir/stamp`, mas a asserção de integridade compara outro arquivo, `RF/stamp`. Ela não prova preservação do carimbo existente após falha do `mv` final. Cura sugerida: provocar a falha final mantendo STAMP apontado para o carimbo verde efetivamente examinado. A mordida contra alcançar `dropdb` continua válida. (bin/tests/test_sonda_sem_grant.sh:394,396,399,408,411)

2. **T4: outros caminhos**

- **PROCEDENTE — bloco:** começando com hoje/OK, nova execução pode calcular FALHOU, falhar em `carimbar` e terminar com o `echo` bem-sucedido. O OK anterior permanece; `conferir` pode aprová-lo enquanto houver migrations. Cura sugerida: invalidar sob a trava antes de executar o bloco, exigir publicação final e testar erro do bloco combinado com falha de gravação. (bin/sombra.sh:356,407,417,429,430,433,455,459)
- **PROCEDENTE — incremental:** a transação altera a janela antes da prova e das máscaras, sem invalidar o carimbo. Falha posterior conserva o verde anterior; falha em `carimbar` também é engolida pelo `echo`. Se o carimbo anterior era completa/OK, a cobertura ainda aceita “completa”. Cura sugerida: invalidar antes da alteração, guardar explicitamente os insumos necessários e exigir publicação final. (bin/sombra_incremental.sh:61,66,68,81,85; bin/sombra.sh:214,216,218,225,228; app/core/sombra_cobertura.py:39,40,41)
- **NÃO PROCEDE — fim do refazer:** `carimbar` é o último comando da função, portanto seu rc se propaga; se falhar, a invalidação anterior permanece. `--dump-agora` converge para a mesma guarda antes do drop; `conferir` não escreve o carimbo. (bin/sombra.sh:110,259,275,276,328,331,332,436,459)

3. **Predicados: NÃO são iguais**

| Caso | Placar | `conferir`, supondo migrations positivas |
|---|---|---|
| Dia vazio ou ontem | `?` | Recusa |
| Hoje, status vazio, INCOMPLETA=0 | `0` | Recusa |
| Hoje, OK, INCOMPLETA ausente | `?` | Aceita |
| Hoje, REFEITA/DIVERGE/FALHOU, número presente | Mostra número | Recusa |
| Hoje, REFAZENDO | `?` | Recusa |

Essas diferenças decorrem de `STATUS != REFAZENDO` mais número obrigatório no placar, contra `STATUS=OK` mais existência mínima do banco em `conferir`. (bin/placar_code.sh:34,35,36,37; bin/sombra.sh:439,455,457,459)

**PROCEDENTE — derivação própria no novo leitor.** Completude medida e ensaio verde são perguntas distintas; não devem ser anunciadas como predicados iguais. Cura sugerida: uma autoridade com parser comum e predicados explicitamente distintos. A cobertura já possui terceiro julgamento: hoje, OK ou REFEITA, depois tipo/base/janela. Até o parsing diverge: última ocorrência no placar, concatenação em `conferir`, última ocorrência com normalização na cobertura. (CLAUDE.md:20,21,22; bin/placar_code.sh:34,37; bin/sombra.sh:439; app/core/sombra_cobertura.py:18,21,35,37,42,45)

Leitores operacionais encontrados:

- **Diretos:** incremental, bloco e `conferir`; placar; `core/sombra_cobertura.py`. (bin/sombra.sh:210,211,224,356,427,439,443,444; bin/placar_code.sh:34,35,36; app/core/sombra_cobertura.py:35,37,39,42,45)
- **Delegados, sem predicado próprio do stamp:** deploy, indicador verde do placar, esteira em `trava.sh`, simulação de folha e diff de janela; os dois últimos também exigem cobertura. (bin/deploy.sh:162; bin/placar_code.sh:169; bin/trava.sh:52; bin/simular_folha.sh:68,70; bin/diff_janela_he_total.sh:19,22)

4. **Porta/defaults — PROCEDENTE a cobertura desigual**

`REVOKE ... ON SCHEMAS` cobre defaults de `$PGU` para `leitor`. A conferência conta defaults de **qualquer concedente**, destinados ao leitor **ou PUBLIC**, também para tabelas e sequências. A porta não revoga todo esse universo. (bin/sonda_provisionar.sh:125,126,128,129,132; bin/sonda_leitor_comum.sh:119,120,121,122)

**A porta deixa esses privilégios, mas não deixa passar como pronto:** a leitura recusa rc 5; a porta recusa rc 1 antes de promover `.env_leitor`. Há bloqueio sem autocura, sem falso pronto nesse caminho. Quem cura é o operador autorizado, revogando o default no concedente/destinatário/escopo de origem; o próprio diagnóstico orienta REVOKE manual. Cura efetiva em produção: **[nao verificado]**. (bin/sonda_leitor_comum.sh:132,145; bin/sonda_leitura.sh:187,135; bin/sonda_provisionar.sh:148,150,151,171)

5. **`_limpa` — PROCEDENTE colisão; NÃO PROCEDE nome vazio no fluxo atual**

O trap precede a criação. Se `run --name "$PGT"` falhar por colisão, o EXIT executa `rm -f "$PGT"` sobre o container que já ocupava o nome. A guarda só exclui três nomes conhecidos; não prova autoria. Cura sugerida: remover exclusivamente o ID obtido da criação bem-sucedida, sem remover por nome após criação recusada; acrescentar caso de colisão ao selo. (bin/tests/test_sonda_sem_grant_pg.sh:35,36,41,50,93,95)

PGT não fica vazio nessa atribuição: contém prefixo literal e PID; a substituição usada pela fixture ocorre em subshell. (bin/tests/test_sonda_sem_grant_pg.sh:35,72,73,74)

**PROCEDENTE — ausência não comprovada:** o rc de `rm` é ignorado e qualquer falha de `inspect` equivale a “sumiu”. Cura sugerida: distinguir inexistência de falha da consulta e testar esta última. Falha real do Docker: **[nao verificado]**. (bin/tests/test_sonda_sem_grant_pg.sh:41,42,85,86,87,88)

6. **Classes adicionais**

- **Segundo escritor operacional do stamp — NÃO PROCEDE no código localizado:** publicação e invalidação passam por `carimbar`; os escritores adicionais dos selos usam fixtures temporárias. Escritor externo/manual: **[nao verificado]**. (bin/sombra.sh:101,110,225,328,430; bin/tests/test_sonda_sem_grant.sh:363,394,432,447)
- **Relógio — FILA para o preexistente; PROCEDENTE no novo leitor independente:** HOJE nasce antes da trava, a base relê a data e a cobertura usa `date.today()`. O placar acrescenta outra captura antes de chamar posteriormente `conferir`. Cura sugerida: referência temporal obtida na borda e passada aos julgamentos. Travessia efetiva da meia-noite: **[nao verificado]**. (anexo triagem_preverif_v3.txt:10; bin/sombra.sh:83,328,464; app/core/sombra_cobertura.py:34; bin/placar_code.sh:39,169)
- **Selo sem mordida — NÃO PROCEDE como acusação geral:** há mortes pertinentes registradas; preservação no caso `mv`, identidade do container e os caminhos incremental/bloco continuam fora dessa prova. (anexo sonda_v4_mut.log:1,6,11,16,21,26,31; bin/tests/test_sonda_sem_grant.sh:396,411; bin/tests/test_sonda_sem_grant_pg.sh:80,93)
- **Campos com dois sentidos — PROCEDENTE na interpretação de prontidão:** STATUS distingue resultado do refazer e resultado do bloco; incremental preserva o status anterior mesmo declarando bloco fora da janela. DIA sozinho não significa pronto, e INCOMPLETA é a medição do refazer. Cura sugerida: preservar essas distinções na autoridade comum, sem equiparar “medição válida” a “ensaio verde”. (bin/sombra.sh:227,319,329,331,432; bin/placar_code.sh:37; app/core/sombra_cobertura.py:37)

7. **Quem escreve / quem lê**

Todos os campos do stamp abaixo são materializados por `carimbar`; ele lê o anterior, produz temporários e promove o resultado. (bin/sombra.sh:94,95,98,99,101)

| Campo de `.sombra_stamp` | Quem escreve | Quem lê |
|---|---|---|
| DIA | Invalidação/completo: sombra.sh:110,328 | sombra.sh:210,356,439; placar_code.sh:34; sombra_cobertura.py:35 |
| STATUS | Invalidação/incremental/completo/bloco: sombra.sh:110,227,331,432 | sombra.sh:224,439; placar_code.sh:35; sombra_cobertura.py:37 |
| INCOMPLETA | Invalidação/completo: sombra.sh:110,329 | placar_code.sh:36 |
| TIPO / BASE_DIA | Incremental/completo: sombra.sh:225,328 | sombra.sh:211,441,443; sombra_cobertura.py:39,45 |
| JANELA_INI/FIM | Incremental/completo: sombra.sh:225,329 | sombra.sh:443; sombra_cobertura.py:42 |
| DIVERGE | Completo: sombra.sh:329 | sombra.sh:427,444 |
| INCREMENTAL_EM / ERROS | Incremental/bloco: sombra.sh:226,431 | sombra.sh:443,444 |
| DUMP, REFEITA, BLOCO, INCREMENTAL_S, BLOCO_NA_JANELA, COMANDOS, ALARMES, FORA_DO_ENSAIO | sombra.sh:226,227,328,330,430,431 | Leitor operacional desses campos: **[nao verificado]** |

| Outros estados/arquivos alcançados pelo delta | Quem escreve / quem lê |
|---|---|
| Defaults de schema; `super` | Porta revoga default e escreve NOSUPERUSER; consulta produz `default_escrita`/`super`; parser, porta e leitura consomem. Fixtures aplicam os estragos. (bin/sonda_provisionar.sh:105,132,148; bin/sonda_leitor_comum.sh:68,119,132,145; bin/sonda_leitura.sh:187; bin/tests/test_sonda_sem_grant_pg.sh:207,208) |
| `.env_leitor`, senha, temporário | Porta lê senha e publica; leitura valida/lê e entrega ao container. (bin/sonda_provisionar.sh:77,168,169,171; bin/sonda_leitura.sh:173,174,178,234) |
| Fontes recortadas pelos selos | Selos leem `sombra.sh` e `placar_code.sh`; escritor dessas fontes não é identificado pelo dado. (bin/tests/test_sonda_sem_grant.sh:358,359,380,422) **[nao verificado]** |
| Fixtures 9/9c/10: stamps, dumps vazios, docker falso, argv, saída e temporários | Selo monta; docker falso registra argv; asserções leem. (bin/tests/test_sonda_sem_grant.sh:363,383,384,385,387,394,403,408,411,413,432,447,450) |
| Fixtures `_limpa`: `lf/docker`, `lf/argv`, `lf/out`, `lf/t`; container PGT | Selo cria entradas; falso grava argv; asserções leem; `_limpa` remove pasta/container e consulta existência. (bin/tests/test_sonda_sem_grant_pg.sh:41,42,46,61,63,71,74,76,80,93) |
| Imagem/recursos do selo PG | Selo lê `recursos.sh` e `db_teste.sh`; autoria desses valores: **[nao verificado]**. (bin/tests/test_sonda_sem_grant_pg.sh:29,31) |
| Trilhas de provisionamento/leitura e saída da porta na sombra | Traps escrevem as trilhas; sombra redireciona e lê a falha da porta. Outros leitores operacionais: **[nao verificado]**. (bin/sonda_provisionar.sh:60,61; bin/sonda_leitura.sh:121,122; bin/sombra.sh:252,253) |

_3126 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
