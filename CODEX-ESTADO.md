# CODEX-ESTADO

> Ultima resposta processada: **21/11/2026 22:02** · publicado em **21/11/2026 22:03** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 21/11 22:02 · sessao e24f7350c635 · WRAPPER-N-VAGAS · verificador · origem=Code

BLOQUEIA

**NOVO contra `e1ac53dce`.** Uma vaga livre pode recusar todas as tentativas por causa de um JSON v1 deixado pela versão anterior. Uma vaga ocupada pelo lançador legado pode fazer o portão comparar os arquivos da chamada errada. E a espera nova do gate pode girar indefinidamente se a abertura do lock falhar. São contraexemplos de código. Ocorrência em produção e os resultados informados de 38 casos, 55 mutantes e 82 mutantes ficam **[nao verificado]**. A classificação do delta usa o patch fornecido. Conferi a sintaxe dos scripts e do helper em leitura; a identidade Git da cópia fica **[nao verificado]**. [`bin/codex_pergunta.sh:342–356,522–543,554–558,667–678`](bin/codex_pergunta.sh#L342) · [`bin/codex_admissao.py:139–162`](bin/codex_admissao.py#L139)

### 1. Decisões que mudam

| Estado | Decisão |
|---|---|
| **PREEXISTENTE** · Meta de admissão ocupada ou capacidade esgotada | Espera a meta até `meta_timeout_s`. No vencimento, `75 PAUSADO` se a flag estiver presente, senão `75 META_OCUPADA`. Capacidade esgotada dá `75 SEM_VAGA` sem esperar vaga. `bin/codex_pergunta.sh:500–506,511–524` |
| **NOVO** · Há outra vaga viva | O portão soma o delta commitado aos previstos de ambas as partes e mantém o `git status` da admitida. Conflito ou `COMUM-NAO-SEI` dá `2 PORTAO`. Sem outra vaga viva, pula M3, como na base. `bin/codex_pergunta.sh:279–320,525–543` |
| **NOVO** · Vaga escolhida, antes de `abriu` | Lê a geração da pausa sob a meta. Valor não numérico dá `2 PRE_CHECK`. Publica JSON v2 e arquiva o anterior. JSON anterior não vazio sem ID válido, histórico divergente ou erro na publicação dá `3 FALHA_TRILHA`, sem `abriu`. O `1` do projetor é interno. `bin/codex_pergunta.sh:342–362,392,501–507,552–558` |
| **NOVO** · Admitida esperando gate, slot ou resposta | Gate e slot repetem fatias de 1 s, sem prazo total. A construtora solta o gate para esperar o slot. Pausa viva dá `75 PAUSADO`. Na resposta, esgotar as tentativas sem pausa dá `3` e a chamada preserva o `.rc` alheio enquanto não pegou a trava. Os outros papéis seguem pelos ramos anteriores. `bin/codex_pergunta.sh:570–577,667–699,708–723` |
| **NOVO** · Com slot e resposta, antes do executor | Readquire a meta. Vencimento dá `75 PAUSADO` ou `75 META_OCUPADA`. Abertura da meta falha em `2 PRE_CHECK`. Pausa ou geração diferente dá `75 PAUSADO`. N inválido, lease recusada, worktree alterada ou insumo divergente dão `2 REVALIDACAO`. Falha na projeção de `iniciou` dá `3 FALHA_TRILHA`. Sucesso publica `iniciou`, libera a meta e envia `GO`. `bin/codex_pergunta.sh:739–778` |
| **NOVO** · Fecho | Persiste `fechou/rc/motivo` no JSON do mesmo ID e acrescenta o motivo no 10º campo de `chamadas.log` para o construtor. Falha de trilha converte `0` em `3` e preserva um rc já não zero. A normalização de `2` ou `75` bruto do executor para `1 FALHA_EXECUCAO`, sem evidência de limite, é **PREEXISTENTE**. `bin/codex_pergunta.sh:372–376,407–416,568–583,820–827` |

**Admissão simultânea indevida.** **NOVO** · A termina e deixa seu JSON na vaga 1. O lançador legado pega só a trava gêmea. B vê a vaga 1 ocupada, mas compara contra o JSON de A. Se a worktree de A continua legível e é disjunta de B, B pode entrar mesmo com arquivo em comum com o legado. O selo de estado alheio limpa os JSONs antes do cenário. **PREEXISTENTE** · Duas chamadas da mesma obra podem usar a mesma worktree limpa com briefs citados disjuntos. O código confere o nome e o registro Git da worktree, mas não compara sua identidade com a da ocupante. Também aceita `app/folha/../ponto/a.py` e `app/ponto/a.py` como declarações disjuntas quando os deltas estão vazios. `bin/codex_admissao.py:139–162` · `bin/codex_pergunta.sh:266–278,298–320,407–415,454–474,519–549` · `bin/disjuntor.py:1526,1612–1645` · `bin/tests/test_codex_n_vagas.sh:212–215,346–352`

**Recusa ou espera sem saída própria.** **NOVO** · Um JSON v1 não vazio deixado por morte abrupta fica preservado. A próxima tentativa escolhe a mesma menor vaga livre e volta a sair `3` até intervenção. O selo exige essa recusa para registro sem ID. Um `slot.gate` que virou diretório deixa o `exec 9>` falhar; o laço interpreta o erro de `flock` como contenção e repete com a lease presa. Um FIFO em `pausa.seq` ou no JSON anterior também pode bloquear leitura sob a meta. **PREEXISTENTE** · Gate ou slots legitimamente retidos sem soltura já podiam deixar a espera sem prazo. `bin/codex_admissao.py:146–151` · `bin/codex_pergunta.sh:72,329–356,397–398,501–507,554–556,667–698` · `bin/tests/test_codex_n_vagas.sh:574–577`

### 2. Pulso durante a espera do gate

**NOVO** · Com o gate ainda preso, a primeira fatia malsucedida depois da publicação vê a flag ou a geração alterada e sai `75 PAUSADO`. Em contenção normal, isso leva até aproximadamente **1 s mais o custo de execução e leitura**, contado do ato efetivo sobre a flag ou a geração. Se o `flock` adquire o gate na fatia, o ramo de sucesso pula essa consulta. A chamada segue e pode detectar a pausa na espera do slot, na resposta ou em M6. Ao passar do slot de volta para um gate ocupado, pode gastar outra fatia. M6 só consulta a pausa depois de adquirir a meta ou ao vencer seu prazo. `bin/codex_pergunta.sh:397–398,672–692,714–718,744–748`

**Trilha e relógio.** O prazo de cada fatia vem de `flock -w 1`. `date +%s` mede a espera e a duração para exibição. Num cancelamento no gate, `vagas.log` registra `abriu/fechou` sem causa. O JSON registra `75/PAUSADO` com `iniciou` nulo, `chamadas.log` registra `fim` com o motivo, e stderr identifica a espera do gate e emite `CODEX_RESULTADO`. Sem a trava da resposta, não há `.rc` novo. O leitor de `vagas.log` continua limitado aos cinco campos e o leitor de `chamadas.log` aceita o décimo, mas só consome os nove anteriores. `bin/codex_pergunta.sh:372–376,391–393,407–415,557–558,568–583,611–612,669–700` · `bin/codex_vagas_estado.py:20–49` · `bin/codex_limite_estado.py:49–68`

### 3. M3 e o “não sei”

| Caso | Saída |
|---|---|
| **NOVO** · `merge-base` ausente, ou diff falha, com outra vaga viva | `COMUM-NAO-SEI`, portanto `2 PORTAO`. Na própria worktree, timeout ou erro de decodificação pode derrubar o Python sem linha de resultado, e o wrapper também recusa. Na outra worktree, a exceção vai para o `except` preexistente. `bin/codex_pergunta.sh:279–313,540–543` |
| **NOVO** · `HEAD` destacado | Funciona quando `refs/heads/main` existe no repositório compartilhado e há ancestral comum. Os casos do selo usam worktrees destacadas. `bin/codex_pergunta.sh:279–288,469–472` · `bin/tests/test_codex_n_vagas.sh:149–151,323–343` |
| **PREEXISTENTE / NOVO** · Worktree sem `main` | Estar destacada em vez de estar na branch `main` é válido. Se a ref compartilhada está ausente na viva já no pre-check, dá `2 PRE_CHECK`, **PREEXISTENTE**. Se a ref deixa de estar disponível quando M3 roda, dá `2 PORTAO`. Se o blob de configuração fica ilegível na autorização, dá `2 REVALIDACAO`, **NOVO**. `bin/codex_pergunta.sh:279–288,425–430,540–543,749–752` |
| **PREEXISTENTE / NOVO** · Previstos vazios | Para quem entra, `if not meus` fecha antes do delta, **PREEXISTENTE**. Para a admitida, `decl` guarda o tamanho da lista original, de modo que status e delta não preenchem o vazio declarado, **NOVO**. Em concorrência, ambos dão `2 PORTAO`. Sozinha, a chamada pode entrar sem previstos ou sem ancestral comum, pois M3 é pulado. O CENSO mantém sua decisão própria para obra da fila 1. `bin/codex_pergunta.sh:289–315,483,540–549` · `bin/disjuntor.py:1723–1747` |

### 4. Escritor e leitores da pausa

- **NOVO** · `pausar.sh` tenta publicar e retirar sob a meta derivada do caminho fornecido. `--soltar` avança `pausa.seq` antes de remover a flag. **PREEXISTENTE** · `--ler` exige `-f`, exibe o conteúdo e calcula idade pelo mtime. `SAIDA` é texto obrigatório na publicação. A volta depende da retirada manual. `PAUSOU` e `SOLTOU` vão para `logs/pausas.log` da raiz **do script**, mesmo quando o alvo pertence a outra raiz. `bin/pausar.sh:14–50,53–72`
- **PREEXISTENTE** · O wrapper aponta para a viva pelo `git-common-dir`. Lê a presença no pre-check, no vencimento da meta de admissão e ao obtê-la. **NOVO** · Lê a sequência em M4 e usa flag ou diferença de geração no gate, slot, resposta e M6. Um pulso inteiro antes da amostra de M4 pode passar despercebido. O PreToolUse configurado trata B3, cota e CENSO, sem ler essa flag. O vigia que interpreta `QUEM/SAIDA` enumera as duas flags da esteira. `bin/codex_pergunta.sh:90–98,397–398,424,440,501–507,674–675,689–692,715–718,744–748` · `.claude/settings.json:62–69` · `bin/disjuntor.py:1851–1863` · `app/core/esteira_vigia.py:467–486`
- **ESMERIL-SEMANTICA.** **PREEXISTENTE** · Para o wrapper um diretório conta como pausa por `-e`, enquanto `--ler` e `--soltar` exigem `-f`. Os selos legados gravam só `QUEM`, inclusive diretamente enquanto a meta está retida. **NOVO** · O escritor pode resolver `pausas/../vagas` através de um symlink e ordenar em uma meta diferente da viva usada pelo wrapper. Dois `--soltar` podem passar pelo teste de presença antes da meta e incrementar duas vezes por uma única retirada, cancelando uma chamada admitida entre eles. Os selos novos leem presença e geração; não verificam `SAIDA` nem `pausas.log`. `bin/pausar.sh:23–50` · `bin/codex_pergunta.sh:397–398,424,440,491,501–507` · `bin/tests/test_codex_pergunta.sh:170–182,261–278,311–317` · `bin/tests/test_codex_n_vagas.sh:398–413,597–639,666–679`
- **PREEXISTENTE, ampliado pela espera nova.** A preparação usa o mesmo `$f.novo` antes da meta. Duas publicações podem trocar o conteúdo atribuído ao autor. O script também pode anunciar `PAUSOU` sem validar o `mv`, ou registrar `SOLTOU` e sair `0` mesmo se o `rm` falhar. `bin/pausar.sh:39–50,61–72`

### 5. Selos

**NOVO** · São **21 mutantes novos**, um removido (`limpa_estado`) e um preexistente reaplicado (`publica`). A tabela abaixo identifica a asserção de efeito disponível. Qual asserção matou cada mutante na execução informada fica **[nao verificado]**. O motor aceita qualquer falha com o prefixo esperado, inclusive uma falha de diagnóstico sob o mesmo rótulo. `bin/tests/test_codex_n_vagas.sh:184–198,691–707,735–745,757–781`

| Mutantes | Efeito que o caso exige |
|---|---|
| `m3_decl`, `m3_minha`, `m3_outra`, `m3_pontas` | `2` e Codex não invocado, com controle disjunto que entra. `bin/tests/test_codex_n_vagas.sh:189–198,323–343` |
| `pausa_seq`, `pausa_gate`, `gate_solta`, `pausa_slot`, `pausa_resp`, `rc_sidecar` | Saída com a trava ainda retida, gate adquirível por outro, `75 PAUSADO` ou bytes do `.rc` alheio preservados, conforme o mutante. `bin/tests/test_codex_n_vagas.sh:598–638,695–700` |
| `m6_meta`, `m6_confere`, `m6_inicia`, `adm:confere_ino`, `adm:confere_n` | Recusa com rc de negócio e sem Codex nas respectivas condições de meta, N, insumo e inode. `bin/tests/test_codex_n_vagas.sh:641–663,701–705` |
| `pz:meta`, `pz:seq` | Flag muda somente depois de soltar a meta e retirada produz sequência `1`. `bin/tests/test_codex_n_vagas.sh:605–610,666–679,706–707` |
| `hist_arquiva`, `proj_fecha`, `proj_dono`, `fim_motivo` | Histórico anterior legível, fecho persistido, rc `3` por dono divergente e 10º campo com o motivo. `proj_dono` verifica apenas a presença do ID antigo, que pode permanecer num JSON indevidamente alterado. `#V:publica` pode ser contado morto pelo erro de lançamento de `_prende` sob o mesmo rótulo da asserção de conteúdo. `bin/tests/test_codex_n_vagas.sh:169–176,438–458,546–577,735–744` |

**Vacuidade parcial nova.** `m5_gate_preso` pode passar com a flag ainda presente porque descarta as duas saídas de `pausar.sh` e não prova retirada nem avanço da sequência. `_ate_abriu` pode retornar sem um novo `abriu`; os casos M5 podem, assim, observar a recusa em outra fase. No selo legado `pausa_reval`, a espera da resposta agora pode detectar a pausa antes de M6, dar o `75` correto e falhar só pelo texto esperado. M3 usa o CLI `--censo` falso e não exercita ref ou merge-base ausente. `bin/tests/test_codex_n_vagas.sh:34–47,323–343,589–623` · `bin/tests/test_codex_pergunta.sh:261–278` · `bin/codex_pergunta.sh:715–718,748`

### 6. Checklist

- **Except e fallback.** **NOVO** · `0` cláusulas Python `except`, `0` `|| true` literais e `1` `raise OSError` capturado pelo `except` **PREEXISTENTE**. Há **1** `2>/dev/null` novo em produção (`_N6`), além dos dois sítios preexistentes movidos com `_COM` e `fim`. No selo são **21** literais adicionados e **3** retirados, mais **7** chamadas a `pausar.sh` que descartam stdout e stderr. Entre os tratamentos novos, os dois retornos `None` do Git fecham M3, sequência ausente vira `0`, e os três `flock -w 1` tratam qualquer falha como contenção. A checagem de `CONFERE` usa o conteúdo e abandona o status do `read`. Os `rev-parse` na publicação também podem deixar metadado vazio. `bin/codex_pergunta.sh:279–313,397–398,540,554–555,573–574,674,689–692,715,749–752` · `bin/pausar.sh:25–26,44–47` · `bin/tests/test_codex_n_vagas.sh:441–458,551–573,593–596,605–645,669–675`
- **Segundo escritor.** **PREEXISTENTE** · A reserva de caminhos filtra `RESPOSTA`, mas `CODEX_LOGD` pode criar diretório em um caminho reservado sob os logs da própria raiz. A interface genérica de `pausar.sh` pode substituir ou remover inode e JSON de vaga fora da porta. **NOVO** · Essas vias também alcançam os artefatos novos `pausa.seq` e `historico`. `bin/codex_pergunta.sh:98–99,122,442–443,475,562` · `bin/pausar.sh:23–25,39–50,53–69`
- **Ordem de trava.** **NOVO** · M6 pega meta com lease, slot e resposta já presos. O caminho de admissão vai de meta para vaga, mas tenta a vaga com `LOCK_NB`. Não há ciclo bloqueante entre esses caminhos normais. `CONFERE` aplica `outras + 1 <= N`; se a chamada ficou em `k > N` mas o total baixou até N, ela inicia. A seleção inicial exige `k <= N`. `bin/codex_pergunta.sh:500–524,699–723,739–766` · `bin/codex_admissao.py:55–60,84–103,139–151`
- **Relógio e significado.** **NOVO** · `iniciou` carimba a projeção da autorização, antes do `GO` e do `Popen`. O `abriu` do JSON usa `time.strftime` antes dos hashes e o `abriu` da trilha usa outro `date` depois da publicação. `QUANDO` da pausa é escrito antes da meta e pode anteceder a ativação, que tem outro carimbo em `PAUSOU`. A sequência é texto comparado por desigualdade no shell, inteiro no JSON e aritmética Bash no escritor. Por exemplo, `08` passa a regex e dá `rc 1` em `--soltar` antes da remoção. A janela do portão continua lendo `date +%H%M` na admissão, **PREEXISTENTE**. `bin/codex_pergunta.sh:335,357–371,397–398,507,534–537,557,760–778` · `bin/codex_admissao.py:164–177` · `bin/pausar.sh:44–49,61–70`
- **Insumo autorizado.** **NOVO** · M4 guarda o hash dos caminhos já resolvidos. O prompt lê os argumentos originais, e M6 confere os caminhos gravados. Se um symlink de pergunta ou anexo mudar de alvo entre M4 e a montagem do prompt, M6 pode validar o arquivo antigo intacto e autorizar bytes do novo alvo. Também não relê os briefs usados para formar `arq`, salvo a conferência de bytes quando forem anexos. `bin/codex_pergunta.sh:221–256,357–371,482–490,555,634,661,760–765`

## 21/11 22:02 · sessao 2c512c616764 · - · - · origem=Ronald (tmux)

BLOQUEIA

- **NOVO — espera sem saída.** Se a abertura de `slot.gate` falhar, o Bash continua com FD inválido. `_gate` repete qualquer erro do `flock` como se fosse contenção, segurando a vaga até observar pausa. `pausa.seq` FIFO também pode bloquear indefinidamente a consulta inicial sob a meta, o polling e o `--soltar`. `bin/codex_pergunta.sh:72,397–398,501–507,667–678`, `bin/pausar.sh:42–45`.
- **NOVO — retirada duplicada.** Dois `--soltar` podem observar a mesma flag antes da meta. O primeiro remove e incrementa. Uma chamada pode ser admitida depois, e o segundo incrementa sem reconferir a flag, cancelando essa chamada por uma retirada que já ocorreu. **PREEXISTENTE — publicação concorrente.** O `.novo` compartilhado é escrito antes da trava, de modo que uma publicação pode renomear o conteúdo da outra com autor diferente na trilha. `bin/pausar.sh:39–50,61–72`, `bin/codex_pergunta.sh:397–398,501–507`.
- **NOVO — M5 em condições normais.** Gate ocupado continuamente implica consulta a cada falha genuína de `flock -w 1`, cerca de uma fatia mais custo do processo. A aquisição bem-sucedida pula a leitura e segue para slot/resposta/M6. A resposta soma tentativas falhas, não mede tempo decorrido. Gate/slot têm espera sem prazo, **PREEXISTENTE**. `bin/codex_pergunta.sh:667–700,711–723,744–748`.
- **Trilha e relógio.** `vagas.log` só tem `abriu/fechou`, JSON fecha com `75/PAUSADO` e `iniciou` nulo antes de M6, `chamadas.log` tem o novo motivo no `fim`. Sem trava da resposta, nenhum `.rc` novo. `iniciou` é autorização, gravada antes de liberar a meta e antes do GO. `date` lê a janela, carimbos e métricas, Python lê outro relógio para o registro, e `flock` governa as fatias. `bin/codex_pergunta.sh:335,361–376,408–415,534–537,554–557,563,570–577,611–612,669–723,760–781`.
- **Semântica.** A flag é a da viva. Wrapper lê presença com `-e` e pulso pela geração; `--ler/--soltar` exigem `-f`; mero diretório pausa para o wrapper e não pode ser solto pela ferramenta. `SAIDA` é declaração humana, sem avaliação automática. `pausas.log` fica na raiz do script mesmo para flag de outra raiz, `SOLTOU` é escrito antes de um `rm` cujo erro é ignorado. Hooks usam portas distintas e não consomem flag/seq na cópia examinada. `bin/codex_pergunta.sh:90–98,397–398,424,440,504–507,675,690,717,746–748`, `bin/pausar.sh:14–16,33–50,53–72`, `.claude/settings.json:3–9,62–69`, `bin/disjuntor.py:1851–1862,1920–1925`, `bin/hook_stop_fila1.py:726–744,788–838`.
- **Selos.** `pausa_gate` morde saída enquanto gate preso, `gate_solta` morde aquisição efetiva, `pausa_slot/resp` mordem rc e motivo, `rc_sidecar` morde bytes, `pz:meta` morde existência e `pz:seq` morde geração/cancelamento. Mas `_ate_abriu` não exige abertura ao vencer prazo. O pulso pode coincidir com a leitura da flag e o teste da geração ficar inconclusivo. O selo legado de M6 exige mensagem específica, mas M5 pode primeiro devolver o mesmo `75 PAUSADO`. `proj_dono` altera campos do registro mantendo ID, de modo que a asserção de preservação por ID passa vazia, embora rc/marcador/mensagem matem o mutante. `bin/tests/test_codex_n_vagas.sh:439–444,591–639,666–679,695–707,740,770–775`, `bin/tests/test_codex_pergunta.sh:261–278,577`, `bin/codex_pergunta.sh:374–376,715–718,748`.

## 21/11 22:02 · sessao 3758c9c9e020 · - · - · origem=Ronald (tmux)

**BLOQUEIA.** O novo loop do gate transforma erro permanente de `flock` em espera sem saída. Se `slot.gate` for diretório, `exec 9>` falha e o shell prossegue. Com fd 9 inválido, `flock -w 1 9` retorna imediatamente e o loop retém a vaga até observar pausa. A abertura sem guarda é preexistente. O loop infinito é novo. `bin/codex_pergunta.sh:72,668–676`.

**Selos.** São 21 mutantes novos, um removido e `#V:publica` reaplicado. Reproduzi em memória a aplicação dos 55 mutantes e a validação sintática. As mortes efetivas estão [nao verificado]. `bin/tests/test_codex_n_vagas.sh:691–707,735–745,757–780`.

- `m3_decl`, `m3_minha`, `m3_outra`, `m3_pontas` testam admissão indevida via rc, captura e marcador. `bin/tests/test_codex_n_vagas.sh:189–193,323–343`.
- `pausa_seq`, `pausa_gate` testam saída com a trava ainda presa. `gate_solta` testa disponibilidade do gate. `pausa_slot`, `pausa_resp` testam `75 PAUSADO`. `rc_sidecar` testa preservação dos bytes do `.rc` alheio. `bin/tests/test_codex_n_vagas.sh:598–638`.
- `m6_meta`, `m6_confere`, `m6_inicia`, `adm:confere_ino`, `adm:confere_n` testam recusa antes do Codex. `pz:meta` testa publicação e retirada sob a meta. `pz:seq` testa a geração resultante. `bin/tests/test_codex_n_vagas.sh:607,641–679`.
- `hist_arquiva` testa preservação do registro anterior. `proj_fecha` testa `fechou/rc/motivo`. `proj_dono` testa rc 3 e diagnóstico, mas o `grep` preservaria o verde se os bytes alheios fossem modificados mantendo o ID. `fim_motivo` testa o campo 10, mas seleciona a última linha por obra, sem vínculo com a chamada corrente. `publica` testa os campos do registro e seu fecho. `bin/tests/test_codex_n_vagas.sh:440–458,546–577`.
- `m5_gate_preso` pode passar com flag persistente. Silencia `pausar.sh`, não exige que a flag tenha sumido e não verifica a geração. `_ate_abriu` também encerra no timeout ou no rc sem exigir o incremento. O motor aceita o primeiro `FALHA:` com o prefixo esperado, inclusive mensagem de fixture. O selo preexistente `test_codex_pergunta.sh` aceita qualquer ocorrência de `FALHA`. `bin/tests/test_codex_n_vagas.sh:589–592,613–623,769–780`; `bin/tests/test_codex_pergunta.sh:657–659`.

**Contagem literal do delta.** Zero cláusulas `except` novas, zero `|| true` novo e um novo `raise OSError` sob o `except Exception` preexistente. Há um `2>/dev/null` novo em produção e dois preservados em linhas alteradas. No selo há 21 adições e três remoções de `2>/dev/null`, mais sete chamadas que descartam ambos os canais. `bin/codex_pergunta.sh:305–313,540,574,749`; `bin/tests/test_codex_n_vagas.sh:441–458,551–573,593–607,619–645,669–675`.

**Outros fallbacks e escritas.** M3 traduz erro do Git em `None` e recusa, com stderr capturado. O teste de geração trata ausência como zero e em espera usa somente o texto de `_seq`. `pausa.seq` FIFO pode travar sob a meta. Revalidação lê somente o conteúdo do pipeline N e o token de `read`, e as três chamadas `flock -w 1` tratam qualquer erro como contenção. O `flock -u` não testa rc. A projeção retorna 1 internamente para registro anterior sem ID, e o wrapper converte para 3. `bin/codex_pergunta.sh:279–315,334–355,397–398,507,555–556,674–692,715–718,749–752`.

`pausar.sh` genérico ainda aceita caminho de vaga, fora da porta. A mesma via alcança os novos `pausa.seq` e histórico. A confirmação de existência de `--soltar` ocorre antes da meta, permitindo que dois soltas da mesma flag incrementem duas vezes e cancelem uma chamada admitida no intervalo. A publicação prepara um `.novo` compartilhado antes da meta e o `mv` e a trilha não são validados. `pausar.sh` aceita `0008` no regex, mas a soma Bash aborta por octal. `bin/pausar.sh:23–30,39–50,53–72`; `bin/codex_pergunta.sh:506–507`.

## 21/11 22:01 · sessao e1e882bdbf25 · - · - · origem=Ronald (tmux)

**BLOQUEIA.** A projeção persistida deixa o lançador legado herdar o conjunto de arquivos de uma chamada encerrada. Uma projeção v1 sobrevivente impede indefinidamente o reuso da vaga. O gate novo pode girar indefinidamente com fd inválido, e a conferência de insumos pode autorizar um prompt diferente do que foi registrado. `bin/codex_admissao.py:139–151`, `bin/codex_pergunta.sh:298–320,334–371,522–555,634,661,668–678`.

- **NOVO — estado alheio.** Depois de um fechamento normal, um legado segura só a trava gêmea de k=1. A nova chamada conta k=1 ocupada, mas compara o JSON persistido da ocupante anterior. Se a WT antiga existir e for disjunta, o candidato passa embora possa tocar o mesmo arquivo que o legado. A base truncava o JSON no fecho. O selo limpa os JSONs antes de testar o ocupante legado. `bin/codex_admissao.py:139–146`, `bin/codex_pergunta.sh:298–320,408–415,519–543`, `bin/tests/test_codex_n_vagas.sh:212–215,346–352`.
- **NOVO — reuso bloqueado.** Um JSON v1 não vazio, deixado por encerramento abrupto na base, falha por ausência de `id_chamada`. A vaga continua livre, mas é selecionada de novo no próximo lançamento e retorna novamente `3 FALHA_TRILHA` antes do `abriu`. Um histórico prévio de conteúdo divergente produz o mesmo efeito. O caso de JSON sem ID está explícito no selo. `bin/codex_admissao.py:146–151`, `bin/codex_pergunta.sh:342–356,554–558`, `bin/tests/test_codex_n_vagas.sh:574–577`.
- **NOVO — esperas sem saída.** A falha de abrir `slot.gate`, por exemplo se for diretório, deixa fd 9 inválido e o novo `until flock` interpreta o erro imediato como contenção. Um FIFO em `pausa.seq` prende `cat`, inclusive com meta em mãos. Um FIFO no JSON anterior de uma vaga livre prende a leitura do novo publicador, com meta e lease em mãos. **PREEXISTENTE:** segurar de fato o gate indefinidamente, sem pausa, não tinha prazo total. `bin/codex_pergunta.sh:72,334,397–398,501–507,555,668–678,748`, `bin/pausar.sh:42–47`.
- **NOVO — insumos M6.** O registro guarda hashes dos caminhos canônicos, mas o CAB lê os argumentos originais e a M6 reabre os caminhos gravados. Trocar um symlink de entrada entre M4 e CAB pode mudar o prompt sem mudar os arquivos conferidos. Trocar A→B antes de CAB e B→A antes de M6 também passa. Os briefs descobertos pelo coletor ficam fora dos hashes salvo anexação explícita. `bin/codex_pergunta.sh:221–256,363–371,482,489,555,634,661,760`.
- **PREEXISTENTE — admissão em comum.** Duas declarações como `app/folha/../ponto/a.py` e `app/ponto/a.py`, com HEADs sem delta e WT admitida limpa, apontam para o mesmo arquivo; a comparação não normaliza `..`. Também não há exclusão explícita pela identidade da WT. A mesma obra sem brief próprio pode citar um brief de outra pasta de `logs` em cada pergunta e apresentar listas disjuntas a partir da mesma WT limpa. `bin/disjuntor.py:1526,1612–1645,1723–1747`, `bin/codex_pergunta.sh:249–256,266–320,454–471,522–549`.
- **NOVO — demais decisões.** M3 soma os dois deltas e conserva as duas pontas do rename. HEAD destacado funciona com ref compartilhada. Sem merge-base na concorrência ou com lista declarada vazia, o portão retorna rc 2. Ausência inicial de main na viva já causava `2 PRE_CHECK`; primeira vaga continua sem M3. M6 relê N e confere o inode próprio; `k>N` ainda pode autorizar se a cardinalidade couber. Sob meta, a busca de vaga é não bloqueante, por isso a ordem `meta→vaga` e `vaga→meta` não forma ciclo nas rotas normais. `bin/codex_pergunta.sh:279–320,426–429,540–544,744–766`, `bin/codex_admissao.py:55–60,84–103,139–151`.
- **Checklist.** Zero `except` novo e zero `|| true` literal novo. Há um novo `2>/dev/null` em produção, em `_N6`. O selo introduz 19 usos literais em novos trechos, três substituindo supressões anteriores, e sete `>/dev/null 2>&1` adicionais. O `except Exception` antigo cobre as novas falhas do delta alheio. A ferramenta de pausa genérica continua podendo escrever em um nome de vaga por caminho explícito sem passar pela meta, que só é tomada para o basename `codex-construtor`. `bin/codex_pergunta.sh:297–313,749`, `bin/pausar.sh:23–30,39–50,53–69`, `bin/tests/test_codex_n_vagas.sh:441–674`.
- **Selos.** M3/M6 aferem rc e não chamada pelo fake além de texto. O motor aceita qualquer falha com o prefixo esperado, então a prova fornecida não identifica qual asserção matou cada mutante. `#V:publica` reaplicado para antes de conferir o JSON, pois M6 rejeita registro ausente e `_prende` acusa que não entrou no Codex. `proj_dono` muda o resultado, mas a suposta preservação verifica só o ID que permanece mesmo quando o mutante altera os demais campos. `bin/tests/test_codex_n_vagas.sh:169–193,438–458,646–663,691–705,735–740,757–775`, `bin/codex_pergunta.sh:334–340,760–764`.

## 21/11 21:40 · sessao b7bed28275bf · E4C-F5-CONSTROI · construtor · origem=Code

**INCOMPLETO**

T1 a T4 estão no diff da cópia. Os commits por pedaço ficaram pendentes por causa da regra «nunca git commit» do brief. O diff contém apenas os três arquivos de teste abaixo, com **167 inserções e 55 remoções**. [nao verificado]

| Pedaço | Mudança | Diff |
|---|---|---:|
| T1 | Os dois A2 exigem que o `SELECT` do PK alvo execute depois do `SELECT FOR UPDATE` do colaborador, no mesmo `atomic`. [Guarda do reconciliador](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:256) | |
| T2 | Os três cenários de C20 `True` exigem zero entradas em `carimbar`. O espião registra a entrada antes de chamar a porta. [RED do C20](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/chamados/tests/test_e2_pergunta_so_ao_c6.py:194) | +21/−2 |
| T3 | A candidata chega com `validada_em` nula e `VALIDACAO` vigente só na sequência. O RED exige que o C6 aceite o carimbo mesmo com a resposta discordante. [RED do prefetch velho](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:341) | T1 + T3 +132/−33 |
| T4 | As duas notas registram `N + K + D`, com um K7 por sobrevivente com empresa e dia de turno, compartilhado por C20 e C6, mais leitura de toque se houver tranca. Os tetos seguem em 238 e 302, com folga zero registrada. [Nota de 238](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/ponto/tests/test_selo_chokepoint_escrita.py:206) · [Nota de 302](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/ponto/tests/test_selo_chokepoint_escrita.py:242) | +14/−20 |

A checagem estática confirmou a compilação dos três mutantes, a ativação por tag e a restauração da função original. A AST do selo é idêntica à da base. Verde, vermelhos e selo na pista [nao verificado]. [Ativação dos mutantes](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:44) · [Nota do selo](/home/ronald/wt-codex-e4c-f5-constroi-213412/app/ponto/tests/test_selo_chokepoint_escrita.py:206) [nao verificado]

**Comandos para o Code na pista**. A suite repassa `-k` e `--tag` ao Django e define `PYTHONDONTWRITEBYTECODE=1`. [bin/suite.sh:198](/home/ronald/wt-codex-e4c-f5-constroi-213412/bin/suite.sh:198) · [bin/suite.sh:319](/home/ronald/wt-codex-e4c-f5-constroi-213412/bin/suite.sh:319) [nao verificado]

```bash
COPIA=/home/ronald/wt-codex-e4c-f5-constroi-213412

# T1 verde e vermelho esperado nos dois A2 [nao verificado]
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_A2_evento_celula_absolveu
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_A2_evento_celula_absolveu --tag f5_e2_sql_antes_da_trava

# T2 verde e vermelho esperado nos três vetos C20 [nao verificado]
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_f5_e2_tranca_
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_f5_e2_tranca_ --tag f5_e2_carimbo_sob_veto_c20

# T3 verde e vermelho esperado na asserção do retorno da porta [nao verificado]
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_RED_prefetch_velho_esconde_validacao_so_na_sequencia
bin/suite.sh --dir "$COPIA" --only chamados --espera 7200 -- -k test_RED_prefetch_velho_esconde_validacao_so_na_sequencia --tag f5_e2_prefetch_velho

# T4 selo
bin/suite.sh --dir "$COPIA" --only ponto --espera 7200 -- -k test_selo_chokepoint_escrita
```

**Autoconferência** — except/fallback novos no diff: **0**. `git diff | grep -n except` na cópia retornou zero ocorrências.

## 21/11 21:32 · sessao 58b27750b928 · E4C-F5-CONSTROI · verificador · origem=Code

BLOQUEIA

**NOVO contra `b0935c1f1`.** O A2 ainda pode aceitar um SELECT executado antes da trava, porque observa o retorno de `.first()` e o savepoint, sem observar a execução do SQL. A nota do chokepoint também mantém uma atribuição incorreta de **dois K7 por sobrevivente**. O código faz **um K7 direto por sobrevivente com empresa e dia de turno**, compartilhado pelo veto C20 e pelo C6 da porta. A correção de `237/301` para `238/302` é compatível com a medição relatada na triagem. A contagem bruta permanece [nao verificado]. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:213–258` · `app/chamados/reconciliador.py:644–657` · `app/chamados/juizes.py:991–997,1027–1085,1144–1157` · `app/ponto/tests/test_selo_chokepoint_escrita.py:206–221,245–260`

## 1. Achados da volta 2 e triagem

| Achado | Estado em `3d8aa2c89` |
|---|---|
| Folga de uma instrução e atribuição do custo | **NOVO. Folga descartada na triagem, de forma condicional à medição corrigida. Atribuição aberta.** O código confirma os tetos `238/302` e a comparação `<=`. Se os totais corrigidos são `238/302`, a folga é zero. Os dumps e o vermelho em `237/301` são relatados na triagem e ficam [nao verificado]. A correção numérica manteve a afirmação de dois K7 diretos por sobrevivente. `app/ponto/tests/test_selo_chokepoint_escrita.py:206–221,245–260,349–360` · `app/chamados/juizes.py:991–997,1067–1075,1144–1157` |
| A2 podia passar sem releitura e o espião perdia chamadas que lançavam | **NOVO, curado em parte.** Uma releitura observada pelo PK é obrigatória. No positivo, uma única entrada bem-sucedida deve receber a mesma instância retornada. No negativo, a releitura deve retornar `None` e a porta deve receber zero chamadas. Agora o espião anota a entrada antes de invocar a porta real, portanto uma exceção dela deixa o registro. A ordem do SQL ainda escapa. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:83–96,217–273` |
| C20 entre emissão e trava | **NOVO, curado nessa janela.** Os REDs exigem emissão, mudança de tranca ou corte, um C20 `True` com o colaborador registrado como travado, trio vazio e contador zero. Cobrem execução direta, request e consulta externa. A entrada na porta sob veto ainda escapa. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:173–248` |
| Selo sem variar candidatas e competências, e `commit <a preencher>` | **Custo linear NOVO, aberto. Fixture PREEXISTENTE. Identificação NOVA, curada.** A nota identifica `701096390`, `78c9af575` e `4fec34a7c`. A fixture cria 1 ou 3 chamados no mesmo dia e confere quantos fecharam. Ela não conta candidatas emitidas, sobreviventes nem competências. `app/ponto/tests/test_selo_chokepoint_escrita.py:206–220,245–259,468–505` |
| Mutante sem `tranca_em_lote(sistema_pk=...)` | **NOVO, descarte procedente para o resultado funcional das fixtures.** A consulta privada continua fria para empresa, K7 e toque. Sem a semente, ela pode aproveitar o PK do lote externo ou buscar `get_user_sistema()` quando houver toque para conferir. Com o mesmo usuário de sistema, o predicado é igual. A possível leitura adicional faz a equivalência de custo cair. Execução do mutante [nao verificado]. `app/chamados/reconciliador.py:638–657` · `app/chamados/juizes.py:1101–1117,1144–1171,1217–1234` · `app/core/utils.py:6–18` |
| Mutante com prefetch velho de `decisoes` | **NOVO, descartado para fila pela triagem. O motivo é insuficiente.** O vigia conta divergências, mas C6 e C7 também consomem a composta. No RED da resposta, a projeção recém-gravada recompõe uma resposta ausente no prefetch velho. No A2 negativo, a coluna preenchida exclui a candidata antes de se lerem os atos. Esses testes podem passar com prefetch velho. Com uma `VALIDACAO` vigente só na sequência, a coluna nula deixa a candidata entrar e o C6 fresco a dá por morta no primeiro degrau. O C6 com o cache velho pode cair no degrau da resposta e recusá-la. Incidência do par no acervo e execução do mutante [nao verificado]. `app/chamados/models.py:4304–4322,4361–4399,4436–4452` · `app/chamados/juizes.py:1406–1440` · `app/chamados/services/vigia_ato_x_coluna.py:8–24` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:135–161` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:247–270` |
| C6 ainda depende da pré-seleção | **PREEXISTENTE, cura parcial, aberto no comportamento.** O motivo histórico da triagem se sustenta pelo anexo. A lacuna ainda incide no trecho tocado pelo 3.3. Uma pergunta muda com `data_evento=NULL` passa pela emissão com ata definitiva. Se, antes da trava, a ata ganha um marco pendente e a lâmpada perguntada fica sem luz, mantendo `concorde`, o SQL ainda aceita a pergunta. Na foto relida, C6 sem carimbo daria viva. A porta aplica o carimbo antes de consultar C6, que então aceita `celula_absolveu` pelo degrau de lastro com dia de turno `None`. O RED da lâmpada começa com a ata já provisória, deixando a pré-seleção barrar o caso. A coluna admite nulo e a porta de nascimento datado o recusa. Ocorrência em produção [nao verificado]. `app/chamados/reconciliador.py:647–657,689–703,724–735` · `app/chamados/models.py:2631–2636,2993–3000,3361–3383,4088–4094` · `app/chamados/juizes.py:337–341,684–699,1455–1496` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:27–33,48–86` |
| K7 ou relavra depois da leitura, e fecho posterior | **PREEXISTENTE, descarte de escopo sustentado para a janela residual.** A trava do colaborador serializa resposta e validação pelas portas citadas. O corte, `PeriodoFechado` e a lavra têm caminhos que podem escrever após a respectiva leitura. No fecho, a consulta privada já acabou, o E2 refresca só `fechada_em` e `fechar()` julga antes de adquirir sua trava. O contexto novo protege a foto usada no carimbo e termina antes do fecho. Concorrência observada em produção [nao verificado]. `app/chamados/services/disputa_emissao.py:1478–1485` · `app/chamados/services/validacao.py:746–760` · `app/colaboradores/services/estrutura.py:18–22` · `app/ponto/services/fechamento.py:1400–1448` · `app/ponto/portas/celula.py:957–965,1181–1202` · `app/chamados/reconciliador.py:654–678` · `app/chamados/models.py:2071–2097` |

## 2. Mutantes que escapam

- **A2, NOVO.** Dentro do mesmo `atomic`, montar o queryset relido com `.order_by('pk')` e avaliá-lo com `list(qs)` **antes** de `trava_do_colaborador`. Depois da trava, chamar `qs.first()` sobre o queryset já avaliado e entregar o resultado à porta. Por inspeção, `.first()` devolveria a instância do cache, ou `None` no negativo. O observador registraria a chamada depois da trava, e o `assertIs` do positivo continuaria satisfeito. A validação interposta já ocorreu antes do `yield`, portanto a fixture continuaria vendo a validação mesmo com o SELECT adiantado. Zero chamadas da consulta observada falha nos dois A2. Zero chamadas da porta é o resultado esperado apenas no negativo. Execução do mutante [nao verificado]. `app/chamados/reconciliador.py:644–657` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:174–193,213–270`
- **C20, NOVO.** No ramo em que C20 devolve `True`, inserir uma chamada a `carimbar` com `resposta` inválida antes do `continue`. A porta lança antes de chamar C6, o E2 engole a exceção e o RED ainda observa `[True]`, trio vazio e zero carimbos. O espião corrigido do A2 pegaria essa entrada se estivesse instalado nesse cenário. Execução do mutante [nao verificado]. `app/chamados/reconciliador.py:654–664` · `app/chamados/models.py:3353–3376` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:197–213` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:83–96`
- **Mordida preservada, NOVA.** Por inspeção, retirar os imports usados no corpo derruba o controle positivo. Retirar a releitura ou pular a candidata antes dela derruba a contagem A2. Carimbar a instância emitida derruba a identidade do positivo. Retirar o contexto privado ou recolocar o colaborador antigo expõe, respectivamente, os REDs de request e de corte. O RED da resposta também exige uma recusa real de C6. Resultados de execução [nao verificado]. `app/chamados/reconciliador.py:627–630,644–664` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:247–266` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:130–171,197–248`

## 3. Quanto custa e o que a nota explica

**NOVO.** Em fluxo sem falha, para `N` candidatas emitidas, `K` sobreviventes ao SQL e `D` sobreviventes com empresa e dia de turno, o trecho acrescenta **`N + K + D` SELECTs diretos**. São `N` releituras da pergunta com JOIN, `K` prefetches da sequência e `D` consultas de K7. Uma tranca encontrada pode acrescentar leituras de toque. O memo nasce a cada iteração, então duas sobreviventes na mesma competência fazem dois K7 no total. O custo cresce com as candidatas mesmo com uma única competência. C6 reutiliza o resultado local. C7 e o fecho podem consultar em outro escopo. `app/chamados/reconciliador.py:635–678` · `app/chamados/juizes.py:991–997,1027–1085,1132–1157,1217–1332,1414` · `app/ponto/services/fechamento.py:1464–1470` · `app/chamados/models.py:2017–2035,2071–2081`

**NOVO.** As duas notas explicam corretamente `N + K` e registram a correção aritmética, com `234→236→238` e `298→300→302` anotados na cópia. O `+2` total relatado por cenário não prova `+2` por sobrevivente. A origem do segundo K7 de cada cenário, o `N/K/D` de cada execução e a composição dos SQLs capturados ficam [nao verificado]. O selo tem mordida para excesso no total da fixture. A atribuição por sítio exigida pelo corte continua incorreta. `app/ponto/tests/test_selo_chokepoint_escrita.py:206–221,245–260,349–360,468–505` · `app/docs/RELATO.md:1292` · `app/chamados/reconciliador.py:647–657`

## 4. Escritores, leitores e semântica

| Campo ou valor | Quem escreve, quem lê e o que significa |
|---|---|
| PK, `disputa_id`, dono e candidata emitida | **PREEXISTENTE.** A abertura grava o dono e o chamado da disputa. O nascimento vincula a pergunta, e a chave de identidade da pergunta deriva o colaborador da disputa. O gerador emite pelo seletor e por C6/C20. O vigia conta emissões sem evento. **NOVO.** O E2 reaplica o seletor para o mesmo PK e a mesma disputa e exige o dono relido no registro da trava. Emitida, sobrevivente e gravada são etapas distintas. `app/chamados/services/disputa_emissao.py:601–606` · `app/chamados/models.py:3224–3234,3766–3776` · `app/chamados/reconciliador.py:635–662,682–735` · `app/chamados/management/commands/reconciliar_chamados.py:89–93,109` |
| `validada_em`, `validada_por`, `decisoes` | **PREEXISTENTE.** Validação grava coluna, autor e ato. Devolução, resposta humana e reabertura podem tirar a validação. `DecisaoPergunta.registrar` grava a sequência e invalida o prefetch e o memo de C6 na instância recebida. C6/C7 leem a composta, o vigia compara ato e coluna, e a porta recusa `(False, True)` quando a coluna está preenchida. **NOVO.** A releitura repete o filtro físico. `(False, True)` fica fora e `(True, False)` pode entrar e receber carimbo. Coluna física e validação vigente podem divergir. `app/chamados/services/validacao.py:161–175,277–296` · `app/chamados/services/disputa_emissao.py:1499–1537,1632–1684` · `app/chamados/models.py:2022–2035,3384–3393,4382–4399,4436–4452` · `app/chamados/reconciliador.py:647–649,689–703` · `app/chamados/juizes.py:1406–1411` |
| `respondida_em`, `resposta_colab`, `motivo`, `tipo_resposta` | **PREEXISTENTE.** Nascimento recebe motivo e tipo. As portas humana, automática, pela trilha e de reabertura operam a resposta e seus atos. C6/C7 entregam ao C21 instante vigente da sequência e texto da projeção. A face SQL usa as colunas físicas. Motivo e tipo guiam a interpretação da resposta e do marco. **NOVO.** O C6 do carimbo recebe esses insumos da instância relida. `app/chamados/models.py:2631–2659,3224–3234,4382–4390,2022–2027` · `app/chamados/services/disputa_emissao.py:1449–1473,1499–1537,1632–1737` · `app/chamados/juizes.py:927–953,1347–1380,1426–1440,1551–1584` · `app/chamados/reconciliador.py:647–657` |
| `celula_id`, `data_evento`, `conversa`, `veredito`, `veredito_via`, DNA e ata | **PREEXISTENTE.** Nascimento e reuso vinculam célula e dia. `gravar_dia_do_fato` atribui o dia. A porta da célula grava DNA e lavra com insumos do cartório ou do admin. O seletor lê o veredito, C6 lê via e lâmpada do marco, e C20 lê o dia via `conversa`. O outbox lê FK e dia. `data_evento` é dia do turno na pergunta datada e dia civil na conversa, com discriminador explícito. `concorde` no seletor e prova do marco na ata são condições distintas. **NOVO.** A célula para o carimbo vem da releitura. `app/chamados/models.py:2993–3012,3178–3234,3472–3496` · `app/chamados/services/fio.py:843–854` · `app/ponto/portas/celula.py:289–311,1050–1098` · `app/ponto/services/cartorio.py:925–935,1898–1922` · `app/ponto/services/veredito_celula.py:346–358` · `app/chamados/juizes.py:337–341,935–976,1423–1474` |
| Empresa, corte, competência, tranca, toque e `_user.pk` | **PREEXISTENTE.** Cadastro e transferência gravam a empresa. Cadastro de empresa grava o corte. A janela deriva mês e ano da competência. `trancar_periodo` e `reabrir_periodo` gravam a tranca. K7 lê `reaberto_em IS NULL`, e C20 usa `fechado_em` como limite do toque. Cobrar, resolver e desarquivar escrevem as assinaturas que C20 lê na trilha. `get_user_sistema()` fornece a identidade usada para autoria automática e para excluir o sistema do toque humano. **NOVO.** O JOIN relê empresa e corte. A consulta privada guarda o resultado por instância e compartilha o mesmo predicado entre C20 e C6. `app/colaboradores/views.py:538–557,1333–1365,1413–1418` · `app/ponto/janelas.py:36–63` · `app/ponto/services/fechamento.py:1400–1441,1464–1470` · `app/chamados/services/cobrar_dia.py:31–41` · `app/ponto/services/veredito_celula.py:352–358` · `app/chamados/views_arquivar.py:74–86` · `app/core/observ.py:133–138` · `app/core/utils.py:6–18` · `app/chamados/juizes.py:991–997,1067–1085,1144–1157,1217–1332` |
| `chamado_destino`, `status_local`, trio e recusa | **PREEXISTENTE.** Nascimento/reuso e costura escrevem o destino, e a porta de transição escreve o estado do chamado. C6 lê o destino e usa `status_local` na chave do memo. `carimbar` escreve `materializada_em`, `materializada_por` e `via_resolucao`. As portas de identidade e descarimbo também operam no trio. `recusa_do_carimbo` é diagnóstico em memória. O trio registra processamento, inclusive absolvição sem plantar batida, apesar do texto de ajuda da coluna. **NOVO.** O E2 entrega à porta o destino relido. `app/chamados/models.py:213–240,3197–3232,3317–3405,3425–3460,3537–3539,3691–3709,4088–4094` · `app/chamados/services/fio.py:857–876` · `app/chamados/juizes.py:784–805,846–864` · `app/chamados/reconciliador.py:647–657` |
| `_gravou`, `n`, `_disputas`, fecho e evento | **PREEXISTENTE.** A porta devolve o booleano de escrita. O E2 conta os sucessos, C7 lê a disputa, `fechar()` grava fecho, autor, resolução e motivo, e a reabertura os limpa. O signal lê o fecho e acorda o pai. A porta do outbox cria o evento e o consumidor grava `processado_em`. **NOVO.** O E2 enfileira a disputa da instância relida. `out['perguntas']` conta carimbos e `processado_em` marca processamento do evento, mesmo com zero carimbos. `app/chamados/reconciliador.py:657–679,1428–1433,1453–1474` · `app/chamados/models.py:2029–2035,2118–2127,2335–2366` · `app/chamados/signals.py:19–46,252–259` · `app/ponto/portas/celula.py:945–954` |
| Valores do espião e do selo | **NOVO.** O wrapper escreve `ENTROU_SEM_RETORNO` em `chamadas` e substitui a entrada pelo booleano, recusa e foto final se a porta voltar. As asserções leem essa lista e as fotos. A foto também lê `recusa_motivo`, texto persistido pelas portas de recusa, e o trio da lavra da pergunta, escrito por `veredito_lavrado.lavrar`. O selo lê `TETOS` em `_cobrar`, comparando com o total capturado. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:39–44,83–96,247–270` · `app/chamados/services/materializacao.py:1397–1400,1723–1737` · `app/chamados/veredito_lavrado.py:254–281` · `app/ponto/tests/test_selo_chokepoint_escrita.py:349–360,488–505` |

## 5. Checklist do pouso [nao verificado]

- **Except/fallback, NOVO:** no patch final fornecido, produção **0 `except`, 0 fallbacks** e testes **0 `except`, 0 fallbacks**. O delta 3.3 descrito no anexo também tem **0 novos** nas duas classes. Os dois `except Exception` do E2 são **PREEXISTENTES**. É **NOVO** o primeiro engolir também falhas na releitura ou no preparo de C20, permitindo ao consumidor marcar o evento como processado. `app/chamados/reconciliador.py:644–678,1428–1433,1453–1469` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:83–96` · `app/ponto/tests/test_selo_chokepoint_escrita.py:218–221,257–260`
- **Segundo juiz, derivação paralela e segundo escritor fora da porta, 0 NOVOS.** O E2 reutiliza o seletor existente e as autoridades C20, C6, K7 e C7. O trio e o fecho passam por `carimbar()` e `fechar()`. O problema do C6 é o estado em que ele é consultado. `app/core/juizes.py:290–305,343–344` · `app/chamados/reconciliador.py:647–703` · `app/chamados/models.py:3361–3405,2118–2126`
- **Relógio solto, 0 NOVOS nos REDs.** Ambos congelam o relógio e mantêm o congelamento nas intercalações. O selo SQL usa `timezone.localdate()` solto, **PREEXISTENTE**. Em produção, `carimbar` obtém o instante sob a trava. O C6 ainda pode usar `date.today()` e acionar `classificar_falta` sem `agora`, **PREEXISTENTE**. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:32–48` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:36,142–151,189–195` · `app/ponto/tests/test_selo_chokepoint_escrita.py:334–347` · `app/chamados/models.py:3361–3376` · `app/chamados/juizes.py:589–598,1475–1482` · `app/escala/servico_jornada.py:385–389`
- **Ordem de trava e mordida.** Na implementação, colaborador primeiro e depois leituras simples, com **zero inversões NOVAS**. O C7 julga antes da própria trava, **PREEXISTENTE**. São **NOVAS** as lacunas de prova do SELECT no A2 e da entrada na porta sob veto C20. `app/chamados/reconciliador.py:644–676` · `app/chamados/models.py:2071–2097,2830–2848,2874–2878` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:217–258` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:197–213`
- **Campo com dois significados.** Coluna de validação versus validação vigente, dia civil versus dia de turno e processamento versus lastro são distinções **PREEXISTENTES**. A repetição do filtro da coluna física sob a trava é **NOVA** e conserva a assimetria dos pares. `app/chamados/reconciliador.py:647–649,689–703` · `app/chamados/models.py:3004–3012,3699–3709,4382–4399` · `app/chamados/juizes.py:337–341,684–699`
- **Pré-requisito de pouso [nao verificado].** Os blobs dos dois arquivos conferem com o patch anexado. A identidade Git da cópia e a comparação direta com `b0935c1f1` ficam [nao verificado], por se tratar de uma exportação. Usei o anexo da volta 2 para a classificação cumulativa e a cópia para conferir o comportamento. O `Ran 31` é informado no enunciado. Execução dos testes, suite inteira, ordem do Opus, SQL bruto e ocorrência em produção [nao verificado]. `CLAUDE.md:344–351,369–381`

## 21/11 20:47 · sessao cbb6edf614fc · MEDIDA-FOLHA11-2111 · sonda · origem=Code

## (a) Retrato, prévia e reconstrução em memória

As referências `dado:N` numeram as linhas JSON do bloco fornecido, começando em `MEDIDA`. As chaves identificam os registros da sonda.

| PK | Retrato gravado | Prévia | Em memória | Datas do furo | Classificação | Fonte |
|---|---|---|---|---|---|---|
| 956 | `entra / null`, lavrado em `2026-11-21T09:45:30.477Z` | `fora / furo_espelho` | `fora / furo_espelho` | Gravado `null`. Em memória `2026-11-20`, com motivo `espelho_furo` e veredito `furo`. Lista própria da prévia [nao verificado]. | **DEFASAGEM**. O par gravado difere e a prévia coincide com a reconstrução. | `dado:2` `A/956` e `dado:3` `A_DIA/956/2026-11-20` |

## (b) Retenção diária, rotas e donos

Datas em 2026. `""` é a razão vazia da tarefa e `[]` é a lista vazia devolvida em `dono_r1`. A tabela mantém os dois campos de rotas que a sonda devolveu, `rotas_diarias` e `tela_retrato.rotas`.

| Chave `B/pk/dia` | Razão / fio / rota da tarefa | Retenção do dia | Rotas diárias | Rotas da tela do retrato | Marca “Sem porta” | Juiz / R1 | Fonte |
|---|---|---|---|---|---|---|---|
| 246 / 01/11 | `"" / nada / null`, verbo `cobrar` | `espelho_cobranca_indevida` | “Validar no fio”, `chamados:modal_fio`, `existe=true` | `[]` | `true` | `CADASTRO / []` | `dado:15` |
| 327 / 10/11 | `"" / nada / null` | `espelho_indefinida` | “Validar no fio”, `chamados:modal_fio`, `existe=true` | `[]` | `true` | `ESTRUTURA / []` | `dado:55` |
| 476 / 24/10 | `"" / sistema / null`, `fio_folha=nada`. A instância `int_volta` oferece `perguntar` no `emissor`, com `porta_com_chamado=false`. | `null` | `[]` | “Vínculo da escala”, `colaboradores:painel_vinculo`, `existe=true` | `false` | `CADASTRO / []` | `dado:69` |
| 476 / 25/10 | `"" / nada / null`. As instâncias `aguarda_resposta` oferecem `responder` com `porta=null` e `porta_com_chamado=false`. | `espelho_indefinida` | “Validar no fio”, `chamados:modal_fio`, `existe=true` | “Vínculo da escala”, `colaboradores:painel_vinculo`, `existe=true` | `true` | `CADASTRO / []` | `dado:70` |
| 476 / 06/11 | `"" / admin / porta_fila`, ação `validar`, `nao_sei=true` e `fio_folha=admin` | `null` | `[]` | “Vínculo da escala”, `colaboradores:painel_vinculo`, `existe=true`. O texto do retrato diz `SEM PORTA`. | `false` | `BATIDA / [BATIDA]` | `dado:82` |
| 97 / 01, 03, 05, 07, 09, 11, 13, 15, 17 e 19/11 | `"" / nada / null` em cada dia. Só em 03/11 consta o verbo `cobrar`. | `espelho_indefinida` em cada dia | “Validar no fio”, `chamados:modal_fio`, `existe=true` em cada dia | `[]` | `true` | `CADASTRO / []` | `dado:108–126`, linhas pares |

**Candidatos a BUG por retenção sem porta funcional.** São 246 em 01/11, 327 em 10/11, 476 em 25/10 e 97 em cada data retida da tabela. Em cada chave, `retencao_dia.motivo` prova a retenção, `fio_folha="nada"` e `tarefa.rota=null` provam a falta de porta publicada, e `tela_retrato.marca_sem_porta=true` confirma a sinalização. A rota genérica “Validar no fio” tem `existe=true`. Para 476 em 25/10, `tarefa.instancias[].acoes[].porta=null` também comprova a falta de porta para responder. Em 24/10 e 06/11, `retencao_dia=null` deixa os dois dias fora desse critério. `Dado:15,55,69,70,82,108–126`; [app/folha/fora_da_folha.py:86](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/folha/fora_da_folha.py:86).

**PK 97. A tela diz “Sem porta”.** Nos dias retidos, a marca é `true`, o dono exibido é “Sem ação disponível” e a lista de rotas do retrato é `[]`. `CADASTRO` vem de `dono_juiz`. A checagem separada `cadastro_97` registra `escala_a_definir=true` e confirma a existência do GET `colaboradores:painel_vinculo` e do POST `colaboradores:vincular_escala`. Envio do formulário e efeito sobre a retenção [nao verificado]. `Dado:108–126`, `B/97` nas datas retidas; [app/templates/folha/_retrato_o_que_fazer.html:46](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/templates/folha/_retrato_o_que_fazer.html:46) e [app/colaboradores/urls.py:12](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/colaboradores/urls.py:12).

## (c) PK 265 em 31/10

| Pergunta | Resultado | Fonte |
|---|---|---|
| Célula e vínculo do dia | Célula presente, `trabalha=true`, com geradora e lâmpadas. Vínculo vigente `12x36`, `isento_grade=false`. | `dado:128` `C/265/2026-10-31` |
| Grade e previsto pelo juiz | Grade `trabalho`, `sem_ancora=false`, **660 min**. `previsto_min_juiz=660`. Ata com **4** células e `n_missing=0`. | `dado:128` `C/265/2026-10-31`, `grade`, `previsto_min_juiz` e `celula.ata` |
| Vereditos | Célula e grade `cobrado`, via `cartorio`. D4 `irregular`, fonte `lavrado`. Retenção `espelho_cobrado`. No espelho visual, a linha existe, tem **4** marcos canônicos, `grade_pelo_builder=false`, veredito `trabalhou` e `dono_juiz=CADASTRO`. | `dado:128–129` `C` e `C_ESPELHO/265/2026-10-31` |
| Leitura da tarefa | `tarefa.nao_sei=true`, `fio_folha=nao_sei`, `tarefa.rota=null`. A instância `batida_fora_escala_grave` está `passivo_sem_linha`, com `acoes=[]`. O bloco D publica “SEM LEITURA (sistema)”. | `dado:128,248` `C` e `D/265/2026-10-31` |

**Candidato a bug estrutural na leitura da tarefa**, pela chave `265/2026-10-31`. O ramo `passivo_sem_linha` detecta projeção de resposta ou validação sem ato correspondente na sequência. A exposição do motivo fora dos marcos propaga `nao_sei=true` sem publicar ação, e a seleção do fio devolve `nao_sei` com rota nula. Qual ato faltou e por que a sequência ficou assim [nao verificado]. O espelho visual, por sua vez, tem linha e marcos previstos e usa um veredito visual próprio. `Dado:128–129,248`; [app/chamados/models.py:4238](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/models.py:4238), [app/chamados/services/tarefa_do_dia.py:154](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/services/tarefa_do_dia.py:154), [app/chamados/services/tarefa_do_dia.py:273](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/services/tarefa_do_dia.py:273), [app/chamados/services/tarefa_do_dia.py:350](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/services/tarefa_do_dia.py:350), [app/chamados/services/tarefa_do_dia.py:761](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/chamados/services/tarefa_do_dia.py:761) e [app/ponto/services/espelho.py:329](/home/ronald/codex_ws/ab351aa9bfa6ca96e314a46b4e9e48032b4a33a5/app/ponto/services/espelho.py:329).

## (d) Empresa 2, competência 11/2026

### Motivo × fio × guarda da porta

A contagem cobre **271 colab-dias com linha `D` completa**. O registro `D/349/2026-11-12` termina antes das guardas. A distribuição dos **1.131 colab-dias** do total por motivo, fio e guarda fica [nao verificado]. Cada célula abaixo conta as chaves `D/pk/data` com o `motivo` da linha e o filtro de campos da coluna. Os zeros valem para o trecho completo fornecido. `Dado:1,130–402`, `MEDIDA`, `D_PONTE/emp=2` e `D`.

| `D.motivo` | `colab` app oferece | `admin` calendário oferece | `admin` ausência libera | `admin` validar libera | `admin` guarda [nao verificado] | `nada` sem oferta | `nao_sei` oferta [nao verificado] | Total |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| `espelho_cobrado` | 101 | 0 | 1 | 0 | 1 | 0 | 1 | 104 |
| `espelho_furo` | 31 | 8 | 0 | 1 | 2 | 0 | 0 | 42 |
| `espelho_indefinida` | 48 | 0 | 0 | 0 | 0 | 11 | 0 | 59 |
| `espelho_nunca_bateu` | 25 | 34 | 0 | 0 | 0 | 0 | 0 | 59 |
| `espelho_discordante` | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 2 |
| `espelho_cobranca_indevida` | 0 | 0 | 0 | 0 | 0 | 2 | 0 | 2 |
| `espelho_fato_em_ausencia` | 0 | 0 | 0 | 0 | 3 | 0 | 0 | 3 |
| **Total no trecho** | **206** | **43** | **1** | **1** | **6** | **13** | **1** | **271** |

- **App e calendário.** Os filtros são `fio=colab`, `rota_tarefa=porta_app`, `guardas_da_porta.resposta_oferecida_no_app=true`, e `fio=admin`, `acao=resolver_dia`, `guardas_da_porta.oferta_calendario=true`, respectivamente. `Dado:131–401`, bloco `D`, filtrado por esses campos e `motivo`.
- **Ausência e validação com leitura positiva.** `D/220/19-11` traz `decidir_ausencia`, com `oferta_aprovar`, `aprovar_raiz_sem_confirmacao` e `rejeitar_raiz_aberta` verdadeiros. `D/334/17-11` traz `validar`, com **4** entradas em `guardas_de_validar` e `libera_guardas_lidas_sem_confirmacao=true` em todas, contadas como **1** colab-dia. `Dado:202,356`.
- **Guardas administrativas sem resultado de leitura.** `validar` em `D/227/02-11`, `D/254/29-10` e `D/255/31-10`. `decidir` em `D/258/30-10`, `D/258/06-11` e `D/258/09-11`. Os campos são `guardas_de_validar` e `libera_acao_especifica`, respectivamente. `Dado:209,233,235,242–244`.
- **Sem oferta e sem leitura.** `oferta_publicada=false` nas chaves `D/97` das dez datas retidas, `D/327/10-11`, `D/246/01-11` e `D/296/10-11`. Em `D/265/31-10`, `fio=nao_sei` e `oferta_publicada=[nao verificado]`. Para todos os registros completos, o resultado da execução do ato é [nao verificado]. `Dado:131–401`, campos `guardas_da_porta` e `resultado_do_ato`.

### Ponte com o R1

| Medida | Sonda | Referência do anexo | Chave em `D_PONTE/emp=2` |
|---|---:|---:|---|
| Colabs pendentes | 240 | 240 | `pendentes`, `pendentes_anexo` |
| União de colabs do R1 | 128 | 128 | `ponte.uniao_r1`, `uniao_r1_anexo` |
| Colabs em comum | 94 | 94 | `ponte.em_comum_colabs`, `em_comum_anexo` |
| Só pendência | 146 | 146 | `ponte.so_pendencia_colabs`, confere com `240 − 94` |
| Só R1 | 34 | 34 | `ponte.so_uniao_r1_colabs`, confere com `128 − 94` |
| Colabs em comum por dono do R1 | `ESTRUTURA` 11, `CADASTRO` 26, `BATIDA` 73 | [nao verificado] | `ponte.por_dono_colabs_em_comum` |
| Colab-dias em comum | 35 | [nao verificado] | `ponte.em_comum_colab_dias` |
| Colab-dias de retenção | 1.131 | [nao verificado] | `colab_dias` |
| Pendentes sem dia | 0 | [nao verificado] | `pendentes_sem_dia` |
| Universo de colabs do R1 | 460 | [nao verificado] | `ponte.universo_colabs_r1` |

Os cinco totais comparáveis coincidem em `D_PONTE`, `dado:130`. Os totais por dono somam **110** ocorrências para **94** pks em comum, com sobreposição entre donos. A interseção exata entre donos [nao verificado]. A interseção por pk e dia é **35**, uma unidade diferente da interseção por pk. `Dado:130`, `ponte`.

**Universo e instante.** O trecho diário fornecido cobre **62** pks e **5** coincidências no mesmo dia, pelas chaves `D/174/01-11`, `D/218/14-11`, `D/288/22-10`, `D/296/02-11` e `D/320/20-11`. A ponte agrega a competência inteira. O instante da sonda é `2026-11-21T23:31:09.766Z`. O instante do R1 e a igualdade dos conjuntos de pks entre as medições ficam [nao verificado]. Entre os cinco números comparáveis, a sonda registra diferença numérica zero. `Dado:1,130,131–401`, em especial `143,199,269,274,342`.

O trecho fornecido não registra `ERRO_TIPO`. O restante da saída após o corte está [nao verificado]. `Dado:1–402`.

## 21/11 20:29 · sessao 47380138b4db · MEDIDA-FOLHA11-2111 · leitura · origem=Code

## A. Fontes e recortes

| Item | Fonte real | Unidade, universo e exclusões |
|---|---|---|
| **(a) 956** | A prévia chama `classificar_export`. O retrato lê `linhas_do_retrato` via `linha_do_colab`. Para comparar os dois critérios no instante da leitura, `montar_linhas` percorre em memória o mesmo caminho do lavrador até antes da persistência. `_dias_que_retem` fornece as datas e os motivos diários. `app/folha/views.py:350–351` · `app/folha/retrato_folha.py:98–111,930–953,1212–1243` · `app/folha/management/commands/lavrar_retrato_folha.py:65–79` · `app/folha/fora_da_folha.py:115–122` | **Colab-competência** para presença e `status/motivo`, **colab-dia** para o furo. Prévia e reconstrução partem dos fechamentos da empresa e competência, qualquer que seja o status. A foto parte das linhas gravadas e tem carimbo próprio. O detalhe diário exclui isentos, desligados e dias dispensados pelo retentor. `app/folha/export.py:765–788,1205–1213,1238–1296` · `app/folha/models.py:350–360` |
| **(b) 246, 327, 476, 97** | `tarefa_do_dia` e `porta_da_tarefa` publicam razão, ações, fio e rota. `o_que_segura` dá o motivo de retenção e traduz a porta diária. O drawer usa `linha_do_colab` e `portas_da_linha`. `InsumosDoDono.dono` chama `dono_da_divergencia` com os insumos reais. `app/chamados/services/tarefa_do_dia.py:804–835` · `app/folha/fora_da_folha.py:257–305` · `app/colaboradores/views.py:994–1006` · `app/ponto/management/commands/e6_oraculo.py:182–195` | **Colab-dia**, em **todos os dias inclusivos da competência 11/2026** para cada PK, pelo corte de `Empresa.dia_inicio_competencia` via `janela_atual`. Os dias da CSV estão [nao verificado]. A sonda mostra o parecer do juiz em cada dia. O R1 só registra dono para divergências acima de 10 minutos que passaram pelos seus filtros. `app/ponto/janelas.py:24–63` · `app/ponto/management/commands/e6_oraculo.py:331–378` |
| **(c) 265 em 31/10** | `celulas_do_periodo`, `vinculo_do_dia_carregado`, a `grade_da_celula` consumida pela tarefa, `minutos_previstos_do_dia`, `tarefa_do_dia`, `status_do_dia_lavrado` e `espelho_do_colab`. O resultado lavrado do supra-juiz fica em `CelulaDia.veredito` e `protestos`. `app/escala/alimentacao.py:54–76,269–317` · `app/chamados/services/tarefa_do_dia.py:635–643,825–835` · `app/escala/utils.py:2241–2282` · `app/ponto/status_dia.py:201–218` · `app/ponto/services/espelho.py:1210–1245` · `app/escala/models.py:1748–1778` | **Um colab-dia**, com janela pelo juiz `janela_atual`. A tarefa distingue fora do vínculo, isenção, tranca, célula ausente, marco ainda a vencer e ata sem lâmpadas. O espelho tem guardas próprias de escala, recorte e montagem da grade. Qual ramo explica o caso 265 está [nao verificado]. `app/ponto/janelas.py:60–63` · `app/chamados/services/tarefa_do_dia.py:214–246` · `app/ponto/services/espelho.py:489–501,523–585,687–706` |
| **(d) emp2** | `dias_que_seguram` seleciona o motivo principal `furo_espelho` e devolve seus dias. A porta vem da tarefa e a tela de `portas_da_linha`. Para liberação, a sonda consulta a oferta de Resolver dia, as guardas de validação do fio, o gate de aprovação da ausência e a lista pendente do app, conforme a ação. A ponte lê `Command.handle` via `ler_r1`. `app/folha/fora_da_folha.py:125–174,279–295` · `app/folha/retrato_folha.py:1024–1076` · `app/colaboradores/services/calendario.py:97–104,125–167` · `app/chamados/services/validacao.py:681–706,740` · `app/ponto/services/ausencia.py:762–780` · `app/chamados/services/pergunta_no_app.py:68–90` · `app/ponto/management/commands/e6_oraculo.py:284–404` | **Colab-dia** para pendência e coincidência diária, **colaboradores distintos** para a ponte com o R1. Outros motivos principais do export ficam fora da lista de pendentes. O R1 parte de fechamentos de empresas ativas e exclui dia sem linha, isento, com ausência, sem valor no espelho, zero nos dois lados ou diferença explicada pela pausa; diferenças de até 10 minutos concordam. `app/folha/fora_da_folha.py:146–174` · `app/folha/export.py:1205–1213,1238–1296` · `app/ponto/management/commands/e6_oraculo.py:303–307,331–378` |

O anexo registra **uma** troca de `entra` para `fora/furo_espelho` na `emp2`, com concordância entre prévia e reconstrução em memória no par `status/motivo`. A atribuição ao **956**, a data do furo e o evento que mudou o resultado estão [nao verificado]. `tabelas.txt:154,158` · `app/folha/retrato_folha.py:1212–1243`

Na `emp2`, o anexo registra **240 pendentes** na prévia e **128** colaboradores na união do R1. A ponte por pessoa dá **94 em comum**, **146 só na pendência** e **34 só no R1**. A coincidência por dia está [nao verificado]. `tabelas.txt:22,39`

Para o **97**, o juiz atribui `CADASTRO` quando o insumo aponta ausência de vínculo no dia. A tela de vínculo mostra **A definir** quando falta escala ativa e registra o GET `colaboradores:painel_vinculo` e o POST `colaboradores:vincular_escala`. O rótulo diário **Sem porta** vem da data gravada no retrato e pode coexistir com um link geral da linha. A sonda expõe os dois testemunhos para verificar o caso. As ocorrências individuais e os bugs em produção estão [nao verificado]. `app/ponto/management/commands/e6_oraculo.py:98–110` · `app/colaboradores/views.py:262–268` · `app/colaboradores/urls.py:12–13` · `app/templates/colaboradores/partials/painel_vinculo.html:42–48,128–137` · `app/templates/folha/_retrato_o_que_fazer.html:42–46` · `app/folha/retrato_folha.py:1066–1075`

## B. `sonda.py`

O script sai no chat. A execução em produção, a conferência Git do hash e as permissões de um admin concreto estão [nao verificado]. O envelope usa a rede dedicada ao Postgres e, na própria inicialização, reescreve a credencial, reafirma privilégios e grava trilha. Nesta volta de leitura, ficou sem execução. `bin/sonda_leitura.sh:121–143,153–193,216–240,245–250`

```python
import json
import logging
import socket
import sys
from collections import defaultdict
from contextlib import ExitStack, redirect_stderr, redirect_stdout
from datetime import date, timedelta
from io import StringIO
from unittest.mock import patch
from urllib.parse import urlsplit

ANO, MES, EMP2 = 2026, 11, 2
REFERENCIA = date(ANO, MES, 1)
DESCONHECIDO = "[nao verificado]"


class EfeitoProibido(BaseException):
    pass


def proibir_efeito(*args, **kwargs):
    raise EfeitoProibido()


def ler_r1(Command, mes):
    # Anexo sonda.py. Os pontos de coleta são do handle real do E6.
    comando = Command(stdout=StringIO(), stderr=StringIO())
    codigo = comando.handle.__func__.__code__
    universo, div, falhas = {}, [], []

    def coletar(quadro, evento, argumento):
        # app/ponto/management/commands/e6_oraculo.py:307,316,401
        if evento == "line" and quadro.f_lineno == 307:
            dados = quadro.f_locals
            universo[dados["e"].pk] = set(dados["ids"])
        if evento == "exception" and quadro.f_lineno == 316 and not falhas:
            falhas.append(argumento[1:])
        if evento == "return":
            div.extend(quadro.f_locals.get("div", ()))
        return coletar

    def observar(quadro, evento, argumento):
        if evento == "call" and quadro.f_code is codigo:
            return coletar

    anterior = sys.gettrace()
    sys.settrace(observar)
    try:
        comando.handle(mes=mes, ano=ANO, hoje=None, so_txt=False, csv=None)
    finally:
        sys.settrace(anterior)
    if falhas:
        erro, pilha = falhas[0]
        raise erro.with_traceback(pilha)
    return universo, div


def selecionar(linha, *campos):
    return None if linha is None else {k: linha.get(k) for k in campos}


def medir(saida):
    from django.core.serializers.json import DjangoJSONEncoder
    from django.db import DatabaseError, connection, transaction
    from django.db.models import Model
    from django.urls import NoReverseMatch, Resolver404, get_urlconf, resolve, reverse, set_urlconf
    from django.utils import timezone

    from chamados.juizes import (
        FIO_SEM_PORTA, motivo_implausivel_validacao, pergunta_respondida, pergunta_viva,
    )
    from chamados.models import ChamadoColaborador, PerguntaDisputa
    from chamados.services.fio import chamado_que_lista
    from chamados.services.materializacao import (
        estado_aplicacao_pergunta, espera_declarado_x_gravado,
    )
    from chamados.services.pergunta_no_app import consulta_do_app
    from chamados.services.tarefa_do_dia import (
        carga_da_tarefa, porta_da_tarefa, tarefa_do_dia,
    )
    from chamados.services.validacao import _resposta_de_hora, entrega_que_barra_a_validacao
    from colaboradores.models import Colaborador, Empresa
    from colaboradores.services.calendario import resolver_dia_oferecido_nas_datas
    from escala.alimentacao import celulas_do_periodo, vinculo_do_dia_carregado
    from escala.models import EscalaColaborador
    from escala.utils import minutos_previstos_do_dia
    from folha.export import classificar_export
    from folha.fora_da_folha import DESTRAVA, _dias_que_retem, _fio_da_porta, dias_que_seguram, o_que_segura
    from folha.models import RetratoFolha
    from folha.retrato_folha import linha_do_colab, montar_linhas, o_que_fazer, portas_da_linha
    from ponto.janelas import janela_atual
    from ponto.management.commands.e6_oraculo import Command, InsumosDoDono
    from ponto.models import Ausencia
    from ponto.services import espelho as servico_espelho
    from ponto.services.ausencia import gate_aprovacao_ausencia
    from ponto.status_dia import status_do_dia_lavrado

    def emitir(bloco, pk=None, dia=None, **dados):
        registro = {"bloco": bloco, "pk": pk, "data": dia, **dados}
        print(json.dumps(registro, cls=DjangoJSONEncoder, ensure_ascii=False), file=saida)

    def erro_tipo(bloco, classe):
        print("ERRO_TIPO", bloco, classe, file=saida)

    def executar(bloco, leitor):
        falhas_sql = []

        def so_leitura(execute, sql, params, many, context):
            comando = sql.lstrip().upper()
            permitidos = ("SELECT ", "SAVEPOINT ", "RELEASE SAVEPOINT ", "ROLLBACK TO SAVEPOINT ")
            if many or not comando.startswith(permitidos):
                raise EfeitoProibido()
            try:
                return execute(sql, params, many, context)
            except DatabaseError as erro:
                falhas_sql.append(erro)
                raise

        try:
            with transaction.atomic():
                with connection.execute_wrapper(so_leitura):
                    valor = leitor()
                    if falhas_sql:
                        raise falhas_sql[0]
            return valor
        except (Exception, EfeitoProibido) as erro:
            erro_tipo(bloco, type(erro).__name__)
            return None

    def colab(pk):
        return Colaborador.objects.select_related("empresa").get(pk=pk)

    def foto(c):
        return RetratoFolha.objects.filter(empresa=c.empresa, mes=MES, ano=ANO).first()

    def resolver_link(href):
        try:
            rota = resolve(urlsplit(href).path).view_name
        except Resolver404:
            return {"rota": None, "existe": False}
        return {"rota": rota, "existe": True}

    def rota_registrada(rota, pk):
        try:
            endereco = reverse(rota, args=[pk])
        except NoReverseMatch:
            return {"rota": rota, "existe": False}
        return {"rota": rota, "existe": resolver_link(endereco)["rota"] == rota}

    def links(portas):
        return [
            {"rotulo": p["rotulo"], **resolver_link(p["href"])}
            for p in portas
        ]

    def tarefa_publicada(t):
        campos = ("razao", "nao_sei", "fio", "rota", "acao")
        instancias = []
        for i in t["instancias"]:
            instancias.append({
                **selecionar(i, "instancia", "tipo", "razao", "motivo", "vencido"),
                "acoes": [
                    {**selecionar(a, "acao", "de"),
                     "porta": a["porta"] if isinstance(a["porta"], str) else None,
                     "porta_com_chamado": isinstance(a["porta"], int)}
                    for a in i["acoes"]
                ],
            })
        return {
            **selecionar(t, *campos),
            "tem_celula_na_tarefa": t["celula_id"] is not None,
            "instancias": instancias,
            "verbos": sorted({v for v in t["verbos"].values() if v is not None}),
        }

    def ler_a():
        c = colab(956)
        ini, fim = janela_atual(REFERENCIA, c.empresa)
        retrato = foto(c)
        gravado = linha_do_colab(retrato, c.pk)
        previa = next(
            (i for i in classificar_export(c.empresa, MES, ANO) if i["colaborador_id"] == c.pk),
            None,
        )
        atual = next(
            (i for i in montar_linhas(c.empresa, MES, ANO, agora=agora, hoje=hoje)
             if i["colab"] == c.pk),
            None,
        )

        def par(linha):
            return None if linha is None else (linha["status"], linha.get("motivo") or None)

        if par(previa) != par(atual):
            leitura = "leitores_atuais_divergem"
        elif previa is None:
            leitura = "sem_linha_nos_leitores_atuais"
        elif gravado is None:
            leitura = "sem_linha_na_foto"
        elif par(gravado) != par(previa):
            leitura = "defasagem_do_par_gravado"
        else:
            leitura = "pares_concordam"

        dias_gravados = None if gravado is None else gravado.get("datas")
        dias = _dias_que_retem(c, ini, fim, hoje=hoje)[1]
        return {
            "ini": ini, "fim": fim, "foto_existe": retrato is not None,
            "lavrado_em": None if retrato is None else retrato.lavrado_em,
            "retrato": par(gravado), "previa": par(previa), "em_memoria": par(atual),
            "comparacao": leitura, "datas_gravadas": dias_gravados,
            "datas_em_memoria": None if atual is None else atual.get("datas"),
        }, [
            (d["data"], selecionar(d, "motivo", "veredito")) for d in dias
        ]

    def ler_b(pk, r1):
        c = colab(pk)
        ini, fim = janela_atual(REFERENCIA, c.empresa)
        carga = carga_da_tarefa([c], ini, fim, agora)
        retencao = o_que_segura(c, MES, ANO, agora=agora)
        por_dia = {d["data"]: d for d in retencao["dias"]}
        insumos = InsumosDoDono(c, ini, fim, hoje, agora=agora)
        linha = linha_do_colab(foto(c), pk)
        tela = None
        if linha is not None:
            tela = {
                "campos": {r["chave"]: r["valor"] for r in o_que_fazer(linha, hoje=hoje)},
                "rotas": links(portas_da_linha(linha)),
            }
        cadastro = None
        if pk == 97:
            tem_escala_ativa = EscalaColaborador.objects.filter(colaborador=c, ativa=True).exists()
            cadastro = {
                "escala_a_definir": not tem_escala_ativa,
                "get": rota_registrada("colaboradores:painel_vinculo", pk),
                "post": rota_registrada("colaboradores:vincular_escala", pk),
                "envio_do_formulario": DESCONHECIDO,
            }

        linhas = []
        dia = ini
        while dia <= fim:
            t = tarefa_do_dia(c, dia, agora, carga=carga)
            fio = _fio_da_porta(porta_da_tarefa(t))
            retido = por_dia.get(dia.isoformat())
            marca = (
                None if linha is None or "dias_sem_porta" not in linha
                else dia.isoformat() in (linha["dias_sem_porta"] or ())
            )
            dono_dia = insumos.dono(dia)
            tela_dia = None
            if tela is not None:
                tela_dia = {**tela, "marca_sem_porta": marca}
            linhas.append((dia, {
                "escopo": "todos os dias da janela 11/2026; dias da CSV [nao verificado]",
                "ini": ini, "fim": fim,
                "tarefa": tarefa_publicada(t), "fio_folha": fio,
                "destrava": DESTRAVA.get(fio, DESCONHECIDO),
                "status_export": retencao["entra"], "motivo_export": retencao["motivo"],
                "retencao_dia": selecionar(retido, "motivo", "veredito"),
                "rotas_diarias": links(retido["portas"]) if retido is not None else [],
                "tela_retrato": tela_dia, "cadastro_97": cadastro,
                "dono_juiz": dono_dia,
                "dono_r1": (
                    DESCONHECIDO if r1 is None
                    else sorted(r1[1].get((c.empresa_id, pk, dia.isoformat()), ()))
                ),
            }))
            dia += timedelta(days=1)
        return linhas

    def ler_c():
        c, dia = colab(265), date(2026, 10, 31)
        ini, fim = janela_atual(dia, c.empresa)
        cel = celulas_do_periodo([c.pk], dia, dia).get((c.pk, dia))
        vinculo = vinculo_do_dia_carregado(c.pk, dia)
        carga = carga_da_tarefa([c], ini, fim, agora)
        grade, isento = carga.grade(c)
        gd = grade.get(dia)
        t = tarefa_do_dia(c, dia, agora, carga=carga)
        ata = {} if cel is None else cel.ata or {}
        return {
            "ini": ini, "fim": fim,
            "celula": None if cel is None else {
                "trabalha": cel.trabalha, "veredito": cel.veredito,
                "via": cel.veredito_via, "veredito_em": cel.veredito_em,
                "protestos": cel.protestos, "tem_geradora": bool(cel.escala_geradora_id),
                "tem_lampadas": bool(ata.get("lampadas")),
                "ata": selecionar(ata, "tipo_dia", "minutos_previstos", "n_celulas", "n_missing"),
            },
            "vinculo_do_dia": vinculo is not None,
            "ciclo_do_vinculo": None if vinculo is None else vinculo.tipo_escala.tipo_ciclo,
            "isento_grade": isento,
            "grade": selecionar(gd, "tipo_dia", "minutos_previstos", "veredito", "sem_ancora"),
            "previsto_min_juiz": minutos_previstos_do_dia(c, dia),
            "estado_d4": selecionar(status_do_dia_lavrado(c, dia, agora=agora), "status", "fonte"),
            "tarefa": tarefa_publicada(t),
            "fio_folha": _fio_da_porta(porta_da_tarefa(t)),
            "retencao_dia": next(
                (selecionar(d, "motivo", "veredito")
                 for d in _dias_que_retem(c, ini, fim, hoje=hoje)[1] if d["data"] == dia),
                None,
            ),
        }

    def ler_c_espelho():
        c, dia = colab(265), date(2026, 10, 31)
        ini, fim = janela_atual(dia, c.empresa)
        falhas_grade = []

        def observar_aviso(mensagem, *args, **kwargs):
            classe = sys.exc_info()[0]
            if mensagem.startswith("montar_dias: grade prevista falhou") and classe is not None:
                falhas_grade.append(classe.__name__)

        # app/ponto/services/espelho.py:583–585
        with patch.object(servico_espelho.logger, "warning", observar_aviso):
            esp = servico_espelho.espelho_do_colab(c, hoje, ini, fim, agora=agora)
        linha = next((d for d in esp["dias"] if str(d["data"])[:10] == dia.isoformat()), None)
        escala = esp["esc"]
        tipo = escala.tipo_escala if escala is not None else None
        insumos = InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
        return {
            "ini_visual": esp["data_ini"], "fim_visual": esp["data_fim"],
            "ciclo_espelho": None if tipo is None else tipo.tipo_ciclo,
            "template_tem_hora_inicio": None if tipo is None else bool(tipo.hora_inicio),
            "grade_pelo_builder": bool(esp["resumo"].get("grade_pelo_builder")),
            "linha_existe": linha is not None,
            "marcos_previstos_canonicos": None if linha is None else len(linha["previsto_canon"]),
            "veredito_espelho": None if linha is None else linha["veredito_dia"],
            "dono_juiz": insumos.dono(dia),
        }, falhas_grade

    def indexar_r1():
        universo, div = ler_r1(Command, MES)
        por_dia = defaultdict(set)
        for eid, pk, dia, dono, *_ in div:
            por_dia[eid, pk, dia].add(dono)
        return universo, por_dia

    def

[truncado em 20000 de 30354 caracteres]

## 21/11 20:25 · sessao 9e691ebcaa22 · - · - · origem=Ronald (tmux)

O recorte de (d) sai de `dias_que_seguram` e `_dias_que_retem`, com porta diária publicada por `_porta` e traduzida por `portas_da_linha`. O leitor de referência por pessoa é `o_que_segura`. `app/folha/fora_da_folha.py:61,70,86,115,125,146,257,275,289` · `app/folha/retrato_folha.py:1024`

- **Validar no fio**. Ler `pergunta_respondida`, `espera_declarado_x_gravado`, `motivo_implausivel_validacao`, `entrega_que_barra_a_validacao` e `estado_aplicacao_pergunta`. O efeito no espelho e a oferta do botão têm respostas próprias. F6 pode impedir registro e conservar o botão, enquanto K6 e a espera do intervalo tiram o botão. O GET do modal marca follow-ups como lidos e deve ficar fora da execução. `app/chamados/services/validacao.py:681,697,703,740` · `app/chamados/services/materializacao.py:865,897` · `app/templates/chamados/partials/modal_fio.html:274` · `app/chamados/views.py:877`
- **Resolver dia**. `resolver_dia_oferecido_nas_datas` consulta o campo da tela e pode devolver `True`, `False` ou `None`. O POST depende de permissão, veredito e insumos adicionais, portanto seu resultado fica [nao verificado]. `app/colaboradores/services/calendario.py:97,125,157,804` · `app/ponto/views.py:3466,3482,3494`
- **Decidir ausência**. O painel lê `esta_aberta` e `pode_aprovar`. O POST aponta à raiz documental e consulta `gate_aprovacao_ausencia(raiz, confirmado=False)`. É possível publicar esse parecer e a abertura para rejeição, com resultado do ato [nao verificado]. `app/ponto/models.py:711,732` · `app/templates/ponto/partials/painel_ausencia.html:320,344` · `app/ponto/views.py:2749,2833` · `app/ponto/services/ausencia.py:762`
- **Fio residual ou app**. A publicação informa rumo e dono. O ato concreto de Decidir depende do tipo de chamado. Liberação de um POST sem ação e ator identificados [nao verificado]. `app/chamados/services/tarefa_do_dia.py:768,793,804` · `app/templates/chamados/partials/modal_fio.html:573,588,617`

Os agregados fornecidos para novembro e `emp2` somam 1.131 pares colab-dia pendentes, 25 sem porta e três sem leitura. A interseção por colaborador é 94 entre 240 pendentes e a união R1 de 128, com 146 só na pendência e 34 só na união. A identidade e a igualdade por dia estão [nao verificado]. A sonda pode recompô-las pela lista `div` do E6 capturada por `ler_r1`. `tabelas.txt` chaves C.P e B.P · `app/ponto/management/commands/e6_oraculo.py:378,401`

O envelope tem efeito próprio ao atualizar credencial, grants, rede e trilha. Ele deve ficar sem execução nesta volta de leitura. `bin/sonda_leitura.sh:121,140,153,184,216,248`

## 21/11 20:24 · sessao 36075f8a256b · - · - · origem=Ronald (tmux)

**(b)** Ler todos os dias inclusivos de `janela_atual(date(2026, 11, 1), empresa)`, com o corte de cada empresa. Consultar `tarefa_do_dia` com uma carga por colaborador e o mesmo `agora`. Usar `o_que_segura` para obter a retenção diária, o fio e o texto literal de `DESTRAVA`. Consultar `InsumosDoDono.dono` para cada dia e manter explícito que o R1 só registra esse dono quando o dia passou por suas exclusões e apresentou divergência. `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`, `app/chamados/services/tarefa_do_dia.py:804`, `app/chamados/services/tarefa_do_dia.py:825`, `app/folha/fora_da_folha.py:46`, `app/folha/fora_da_folha.py:257`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:331`, `app/ponto/management/commands/e6_oraculo.py:378`.

O drawer da Folha usa `linha_do_colab` no retrato gravado e `portas_da_linha`. O rótulo diário **Sem porta** vem de `dias_sem_porta` e pode coexistir com links complementares da linha. Para o pk 97, a tela de vínculo exibe **A definir** quando falta a escala ativa. A rota de cadastro GET `colaboradores:painel_vinculo` contém o formulário para `colaboradores:vincular_escala` POST, condicionado ao posto e à permissão. O dono diário por ausência de vínculo é `CADASTRO` quando o juiz recebe esse insumo. A ocorrência e o estado da tela para os pks pedidos em produção estão [nao verificado]. `app/colaboradores/views.py:994`, `app/templates/folha/_retrato_o_que_fazer.html:46`, `app/folha/retrato_folha.py:1003`, `app/folha/retrato_folha.py:1066`, `app/templates/colaboradores/partials/painel_vinculo.html:42`, `app/templates/colaboradores/partials/painel_vinculo.html:128`, `app/colaboradores/urls.py:12`, `app/colaboradores/views.py:401`, `app/ponto/management/commands/e6_oraculo.py:98`.

Resolver nomes de rota sem chamar views. O GET do fio marca follow-ups como lidos. Para Resolver dia, o leitor de oferta é `resolver_dia_oferecido_nas_datas`, que preserva desconhecimento como `None`. Para Validar, `entrega_que_barra_a_validacao` é uma guarda real, mas a execução ainda pode recusar por confirmação, materialização ou outros ramos. `app/chamados/views.py:869`, `app/colaboradores/services/calendario.py:125`, `app/chamados/services/tarefa_do_dia.py:697`, `app/chamados/services/validacao.py:209`, `app/chamados/services/validacao.py:693`, `app/chamados/services/validacao.py:740`, `app/chamados/services/validacao.py:769`.

## 21/11 20:24 · sessao 7832b08c923a · E4C-F5-CONSTROI · verificador · origem=Code

BLOQUEIA

**NOVO — o selo tem uma instrução de folga em cada cenário.** O anexo mede **237 e 301** na cura, mas a cópia aceita **238 e 302**. A anotação também atribui **+2 SELECTs a cada sobrevivente**, embora a consulta nova faça **um** SELECT direto de K7 por sobrevivente com empresa e dia de turno. A origem do segundo SELECT de cada cenário está **[nao verificado]**. `medida_c8.txt:2–5` · `app/ponto/tests/test_selo_chokepoint_escrita.py:209–218,245–254,343–354` · `app/chamados/reconciliador.py:647–657` · `app/chamados/juizes.py:991–997,1027–1037,1067–1075,1144–1157`

**PREEXISTENTE contra a base, com cura parcial — a elegibilidade pelo C6 ainda depende da pré-seleção.** Sob a trava, o E2 reaplica o filtro SQL e lê a ata atual, mas só chama o C6 com o carimbo já aplicado em memória. Contraexemplo por leitura: uma pergunta muda e sem `data_evento` é emitida com ata definitiva. Antes da trava, a ata vira provisória, com a lâmpada perguntada apagada, e o veredito continua `concorde`. O filtro SQL ainda aceita a pergunta. O C6 da instância relida daria **viva** antes do carimbo. Com o carimbo aplicado, cai no degrau de lastro e aceita `celula_absolveu` porque o dia do turno é `None`. O teste da lâmpada apagada monta a ata provisória desde o início, de modo que a pré-seleção barra esse caso. Ocorrência em produção **[nao verificado]**. `app/chamados/reconciliador.py:647–657,689–703,724–735` · `app/chamados/models.py:3361–3376,4080–4094` · `app/chamados/juizes.py:337–341,684–699,1455–1496` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:27–33,48–86`

Conferi os quatro arquivos alterados pelos identificadores de blob do `diff.patch` e fiz a análise sintática em memória. A cópia exportada permite conferir o conteúdo e o diff fornecido, conforme `CLAUDE.md:344–348`. A identidade e a cadeia dos commits por Git, a execução dos testes e mutantes, os SQLs brutos e a ocorrência dos casos em produção ficam **[nao verificado]**. [nao verificado]

### 1. Achados da volta 1

| Achado | Estado em `e933fc7da` |
|---|---|
| A2 podia passar sem provar releitura | **Curado, NOVO.** Exige uma chamada de `.first()` filtrada pelo PK alvo, após a trava e no mesmo conjunto de savepoints. O caso negativo exige resultado `None`, validação e ato gravados. O controle positivo exige por identidade que a porta receba a instância relida. O limite do espião de exceções está no item 4. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:170–189,209–269,271–279` |
| C20 sem mordida entre emissão e trava | **Curado nessa janela, NOVO.** Os testes intercalam tranca real em execução direta e em request, e mudança real de corte dentro de consulta externa. Exigem emissão, leitura de C20 `True` sob a trava, trio vazio e contador zero. A corrida depois da leitura de K7 continua possível, conforme item 2. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:173–248` |
| Selo linear por candidata sem variar candidatas | **Aberto.** O desenho da fixture é **PREEXISTENTE**. A releitura e o K7 por candidata são custos **NOVOS**. A fixture varia e confere quantos chamados fecharam. A quantidade de candidatas emitidas, sobreviventes e competências fica **[nao verificado]** nesses cenários. `app/ponto/tests/test_selo_chokepoint_escrita.py:462–499` · `app/chamados/reconciliador.py:636–657` |
| `commit <a preencher>` | **Curado quanto à identificação, NOVO.** A anotação nomeia `701096390` para as duas releituras e `78c9af575`/`4fec34a7c` para a comparação da cura4. A exatidão numérica e a atribuição por sobrevivente continuam abertas. `app/ponto/tests/test_selo_chokepoint_escrita.py:206–218,242–254` · `medida_c8.txt:1–5` |

### 2. O que a trava cobre

| Leitura | Instante e cobertura |
|---|---|
| **Trava e dono** | **PREEXISTENTE:** `atomic` e `trava_do_colaborador`, com `SELECT FOR UPDATE` da linha do colaborador e registro transacional. **NOVO:** confere o dono da disputa relida. O `colab` original delimita a consulta pela FK, e `p.pk`/`p.disputa_id` identificam a candidata original. `app/chamados/reconciliador.py:635–652,689–703` · `app/chamados/models.py:2830–2848,2874–2878` · `app/chamados/services/trava_do_colaborador.py:29–63` |
| **Universo, célula, lâmpada e decisões** | **NOVO:** depois da trava, o SELECT reaplica dono, carimbo nulo, validação física nula, célula vinculada e veredito elegível. Traz a célula com via, DNA e ata, o destino, e faz prefetch de `decisoes`. O C6 usa a composta e as lâmpadas dessa instância; o `_pv_memo` do objeto emitido fica na pré-seleção. A decisão de emitir ainda usa C6 e C20 anteriores à trava, e o único C6 posterior julga com o carimbo aplicado. `app/chamados/reconciliador.py:647–657,689–735` · `app/chamados/models.py:3361–3376,4361–4390` · `app/chamados/juizes.py:895–907,935–976,1406–1496` |
| **Empresa, corte, tranca e toque** | **NOVO:** o JOIN relê colaborador e empresa sob a trava. `tranca_da_consulta` abre um memo novo, com apenas o PK de `sistema` fornecido pelo lote local. Lê o dia da pergunta, deriva a competência pelo corte, consulta K7 e, quando há tranca, lê a trilha de toque. O veto explícito e o C6 usam o mesmo resultado por identidade da instância. Os contextos de fora voltam ao sair. O PK de `sistema` é obtido antes da trava e depois usado tanto para autoria quanto para excluir o sistema do toque humano. `app/chamados/reconciliador.py:638–657` · `app/chamados/juizes.py:991–997,1027–1085,1101–1117,1132–1171,1217–1266,1271–1332,1414` · `app/ponto/janelas.py:36–63` · `app/ponto/services/fechamento.py:1464–1470` · `app/core/utils.py:6–18` |
| **Mudança durante a tentativa** | **PREEXISTENTE:** a trava do colaborador serializa as portas citadas de resposta e validação. A escrita de `PeriodoFechado` ocorre antes do gancho da fila e a alteração do corte e a lavra da célula têm seus próprios caminhos de gravação. Portanto, K7 pode ler aberto, outra transação pode gravar a tranca e o C6 ainda consumir o `False` daquela tentativa. Uma relavra também pode ocorrer entre o SELECT da célula e o `save`. São janelas possíveis pelo código. Concorrência observada **[nao verificado]**. `app/chamados/services/disputa_emissao.py:1478–1485` · `app/chamados/services/validacao.py:746–760` · `app/ponto/services/fechamento.py:1400–1429,1432–1448` · `app/colaboradores/services/estrutura.py:18–22` · `app/ponto/services/cartorio.py:1898–1922` · `app/ponto/portas/celula.py:957–965,1181–1202` · `app/chamados/reconciliador.py:644–657` |

O **fecho posterior da disputa** tem outra janela, **PREEXISTENTE**. O E2 agora enfileira a disputa relida, mas a consulta local de C20 já terminou quando refresca apenas `fechada_em` e chama C7. `fechar()` volta a ler perguntas e julgar antes de adquirir sua própria trava. O caminho pode voltar ao memo ordinário de C20. O carimbo da pergunta e o fecho da disputa têm instantes e guardas distintos. `app/chamados/reconciliador.py:654–678` · `app/chamados/models.py:2017–2035,2071–2097` · `app/chamados/juizes.py:1101–1117`

### 3. Escritores, leitores e significado

| Campo ou valor | Escritores, leitores e contrato |
|---|---|
| `pk`, `disputa_id`, dono, universo e emissão | **PREEXISTENTE:** a abertura grava o dono da disputa, o nascimento vincula a pergunta à disputa, e o `save` de inserção deriva a coluna de identidade do dono da disputa. O gerador estreita pelo SQL e julga por C6/C20. O vigia conta emissões sem evento. **NOVO:** o E2 reaplica o mesmo seletor para o PK e a disputa originais, e confere a FK do dono no registro da trava. Emissão e gravação são contagens de etapas distintas. `app/chamados/services/disputa_emissao.py:601–606` · `app/chamados/models.py:1892–1896,3224–3234,3766–3775` · `app/chamados/reconciliador.py:635–662,682–735` · `app/chamados/management/commands/reconciliar_chamados.py:89–93,109` |
| `validada_em`, `validada_por`, atos e cache de `decisoes` | **PREEXISTENTE:** validação grava coluna, autor e ato; resposta e devolução retiram a validação pelos respectivos atos e projeções; reabertura limpa pela porta de lote. `registrar` grava a sequência e invalida o prefetch e o memo de C6 da instância recebida. C6 e C7 leem a composta; o vigia compara ato e coluna. O seletor lê a coluna física. Assim `(ato=False, coluna=True)` fica fora e `(ato=True, coluna=False)` entra e pode receber carimbo: C6 retorna morta pela validação e a guarda de `carimbar` depende da coluna preenchida. **NOVO:** o E2 repete esse filtro físico sob a trava. Incidência desses pares nesta obra **[nao verificado]**. `app/chamados/services/validacao.py:161–175,284–296` · `app/chamados/services/disputa_emissao.py:1499–1537,1630–1684` · `app/chamados/models.py:2022–2035,3367–3393,4382–4399,4428–4453` · `app/chamados/services/vigia_ato_x_coluna.py:8–24` · `app/chamados/reconciliador.py:647–649,689–703` · `app/chamados/juizes.py:1406–1411` |
| `respondida_em`, `resposta_colab`, `motivo`, `tipo_resposta` | **PREEXISTENTE:** nascimento define motivo e tipo. As portas humana, automática, pela trilha e de reabertura operam resposta e projeção. A composta entrega instante vigente do ato e texto da projeção. C6/C21 e C7 consomem a composta. Motivo e tipo guiam a comparação com a lâmpada e a porta de resposta. A face SQL de resposta usa as colunas físicas. `app/chamados/models.py:2631–2659,3224–3234,2022–2027,3649–3650,4382–4390` · `app/chamados/services/disputa_emissao.py:1449–1473,1499–1537,1630–1684,1689–1737` · `app/chamados/juizes.py:927–953,1347–1380,1426–1440,1551–1583` |
| `celula_id`, `data_evento`, `conversa`, veredito, via, DNA e ata | **PREEXISTENTE:** nascimento, reuso e rechaveamento vinculam célula e dia. `gravar_dia_do_fato` atribui o dia; `conversa` distingue dia civil de dia do turno para C20/C6. A porta da célula escreve DNA e lavra, e cartório/admin fornecem a ata ou o veredito. O seletor lê o veredito; C6 e lastro leem via e lâmpadas, inclusive `pendente`; o outbox lê FK e dia. Veredito elegível no SQL e lâmpada que prova o marco são condições distintas. `app/chamados/models.py:3004–3012,3178–3234,3472–3496` · `app/chamados/services/fio.py:843–854,1084–1096` · `app/chamados/juizes.py:337–341,935–976,1423–1474` · `app/ponto/portas/celula.py:289–311,1050–1098` · `app/ponto/services/cartorio.py:925–935,1898–1922` · `app/ponto/services/veredito_celula.py:346–358` · `app/ponto/services/lastro.py:138–150` |
| `empresa_id`, corte, `fechado_em`, `reaberto_em`, toque e `sistema` | **PREEXISTENTE:** cadastro e transferência gravam a empresa, cadastro de empresa grava o corte, fechamento grava a tranca e reabertura. K7 lê `reaberto_em IS NULL`; C20 usa `fechado_em` como limite para as trilhas de cobrar, resolver e desarquivar. A atribuição de `sistema` vem de `get_user_sistema`; C20 exclui esse ator ao ler a trilha. Fechamento mensal e fila também leem K7. **NOVO:** o E2 usa esses mesmos significados com dados relidos e memo local. `app/colaboradores/views.py:538–557,1333–1365,1413–1418` · `app/colaboradores/services/estrutura.py:13–22` · `app/ponto/services/fechamento.py:1400–1441,1464–1470` · `app/chamados/services/cobrar_dia.py:31–41` · `app/ponto/services/veredito_celula.py:352–358` · `app/chamados/views_arquivar.py:74–86` · `app/core/utils.py:6–18` · `app/chamados/juizes.py:1067–1085,1217–1332` · `app/ponto/views.py:1030` · `app/chamados/services/competencia_trancada.py:88–121` |
| `chamado_destino`, `status_local` | **PREEXISTENTE:** nascimento/reuso e costura gravam a FK; exclusão do destino aplica `SET_NULL`. A porta de transição grava `status_local`. O C6 lê o destino em seus degraus e usa o estado na chave de memo. O outbox acompanha a FK. **NOVO:** o destino carregado para o carimbo vem da releitura. `app/chamados/models.py:213–240,3197–3232,3463–3476,3691–3698` · `app/chamados/services/fio.py:857–876` · `app/chamados/juizes.py:784–805,846–864` · `app/chamados/reconciliador.py:647–649` |
| Trio do carimbo, retorno, recusa, `n`, `_disputas` e fecho | **PREEXISTENTE:** o modelo atribui e salva o trio pela porta, e as portas de identidade e descarimbo também operam nele. `materializada_em` delimita fila; a via e o juiz distinguem o tipo de resolução. Há carimbo `celula_absolveu` sem plantio, embora o texto de ajuda descreva batida criada e chamado fechado. `recusa_do_carimbo` é diagnóstico em memória. **NOVO:** o E2 entrega `fresca`, conta o `True` e enfileira essa disputa. O consumidor soma `n`; o cartório apenas chama o E2. C7/`fechar` leem as perguntas, escrevem o fecho e acordam o pai; reabertura limpa o fecho. O outbox da pergunta é escrito no `save`; `EventoCelula.processado_em` é marca do consumidor. `app/chamados/models.py:3320–3405,3425–3460,3537–3539,3699–3709,4088–4094,2071–2127,2335–2366` · `app/chamados/reconciliador.py:657–679,1428–1433,1453–1474,1497–1507` · `app/chamados/signals.py:19–46,252–259` · `app/ponto/services/cartorio.py:1018–1033` |

**O C20 mantém o mesmo predicado.** Dentro do carimbo, muda o instante da foto. A consulta privada restaura os contextos externos e deixa de fora o memo da pré-seleção. Gerador, vigia, app, C7, S143 e tarefa do dia continuam com seus escopos. Em particular, S143 ainda pode copiar o memo do colaborador do cartório, aquecido pela pré-seleção, enquanto seu fecho abre outra consulta. Portanto o frescor novo vale no carimbo do E2, sem garantia de que um leitor posterior veja a mesma foto. `app/chamados/juizes.py:979–997,1101–1117,1132–1171` · `app/chamados/reconciliador.py:654–657,713–735` · `app/chamados/services/pergunta_no_app.py:45–65` · `app/chamados/services/disputa_emissao.py:1983–2014,2038–2043,2149–2164` · `app/chamados/services/tarefa_do_dia.py:550–555` · `app/ponto/services/cartorio.py:109–116,990–994`

### 4. Mordida dos REDs

| Reversão isolada | Resultado por inspeção |
|---|---|
| Hunk de imports de produção | **Morre.** O caminho sobrevivente chega a nomes indefinidos. O laço engole a exceção, mas o controle exige `n == 1` e trio gravado, e os REDs de C20 exigem `[True]` sob trava. `app/chamados/reconciliador.py:627,647–664` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:163–171,197–213` |
| Hunk do corpo do E2 | **Morre.** A reversão elimina a releitura exigida pelo A2 e entrega `p` antigo à porta no RED da resposta interposta. O RED exige exatamente uma recusa de C6 após conferir resposta e ato reais. `app/chamados/reconciliador.py:644–662` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:221–254` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:130–161` |
| Quatro mutações descritas no relato | **Mordem por inspeção:** tirar a consulta interna é exposto pelo request; reinstalar `colab` antigo é exposto pela mudança de corte; pular a respondida quebra a contagem de releituras do A2; carimbar `p` quebra a identidade do controle positivo. Os quatro `rc 1` são informados. Execução e asserção exata que falhou **[nao verificado]**. `app/chamados/reconciliador.py:645–657` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:181–248` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:201–204,243–254` |
| Mutações menores que podem sobreviver | **NOVO:** remover só `tranca_em_lote(sistema_pk=...)` preserva os resultados dos REDs. Sem a semente, o juiz busca o mesmo usuário de sistema. Também é possível reutilizar o prefetch velho de `decisoes` sobre as colunas novas e passar os casos focados: o RED da resposta grava a projeção junto com o ato, a leitura recompõe a resposta ausente a partir da projeção e o A2 negativo já exclui a linha pelo filtro físico. Execução dessas variantes **[nao verificado]**. `app/chamados/reconciliador.py:647–657` · `app/chamados/juizes.py:1144–1154,1228–1234` · `app/chamados/models.py:4304–4317,4361–4367` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:135–161` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:243–266` |

**Passagem vazia.** A2 e C20 falham com zero releituras ou zero julgamentos observados, respectivamente. O RED da resposta exige tentativa com retorno de recusa. Resta uma lacuna no A2 negativo: o espião registra a entrada **depois** do retorno de `carimbar`. Uma chamada indevida que lance após a releitura observada, seja revertida e engolida pelo E2 pode deixar a lista e o contador zerados e o evento processado. O RED de C20 também registra o resultado do juiz e o efeito persistido, sem observar a entrada na porta. `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:83–92,243–269` · `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:130–133,157–161,197–213` · `app/chamados/reconciliador.py:644–664,1453–1469`

**Relógio.** Os REDs estão congelados, inclusive no `yield` das intercalações. No selo SQL, `timezone.localdate()` da fixture continua solto, **PREEXISTENTE**. Em produção, o carimbo lê o instante depois da trava. O C6 ainda pode ler `date.today()` e acionar o relógio avulso de `classificar_falta`, ambos **PREEXISTENTES**. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:36,142–151,189–195` · `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:32–48` · `app/ponto/tests/test_selo_chokepoint_escrita.py:324–341` · `app/chamados/models.py:3361–3376` · `app/chamados/juizes.py:589–598,1475–1482` · `app/escala/servico_jornada.py:385–389`

### 5. Teto e custo real

- **NOVO, N+1 por candidata.** Para `N` emitidas e `K` sobreviventes ao SQL, há `N` SELECTs da pergunta e `K` SELECTs de atos. Entre as sobreviventes, cada uma com empresa e dia de turno paga um K7 direto, mesmo se várias compartilharem a competência, pois o memo nasce dentro da iteração. Sem empresa ou dia, esse K7 custa zero. Com tranca encontrada, pode haver leituras de trilha. C6 reutiliza o resultado local. C7 e os ganchos de fecho podem consultar K7 em outros escopos. `app/chamados/reconciliador.py:636–678` · `app/chamados/juizes.py:991–997,1027–1085,1132–1157,1217–1332,1414` · `app/chamados/models.py:2022–2035,2071–2081`
- **NOVO, duas chaves alteradas e uma instrução de folga em cada.** Contra a base do diff, apenas `234→238` e `298→302` sobem. O C8 mede `235→237` e `299→301`, com `ponto_periodofechado` de `3→5` nos dois cenários. O comentário soma o delta aos tetos intermediários `236/300`, que estão uma unidade acima dos totais do RED. O selo usa `<=`, então aceita os dois excessos de uma instrução. Variar 1 e 3 chamados prova que o caminho de fecho andou, e deixa aberta a variação por candidata e competência. Pilha por SELECT e validação independente da composição SQL **[nao verificado]**. `medida_c8.txt:1–5` · `app/ponto/tests/test_selo_chokepoint_escrita.py:206–218,242–254,343–354,462–499`

### 6. Checklist do pouso [nao verificado]

- **Segundo juiz e derivação paralela — zero NOVOS.** O diff reaplica o seletor existente e compõe C20, C6 e C7 registrados. O veto explícito de C20 impede converter tranca em carimbo, enquanto C6 usa a tranca para decidir vitalidade. `app/core/juizes.py:290–304,343` · `app/chamados/reconciliador.py:647–675` · `app/chamados/juizes.py:1412–1418`
- **Segundo escritor fora da porta — zero NOVOS em produção.** O trio continua em `carimbar`; o fecho continua em `fechar`. Os limites preexistentes de serialização estão no item 2. `app/chamados/reconciliador.py:652–676` · `app/chamados/models.py:3324–3328,3361–3405,2118–2126`
- **Ordem de trava — nenhuma inversão NOVA no hunk.** Colaborador primeiro, depois leituras simples. C7 julga antes da sua própria trava, **PREEXISTENTE**. `app/chamados/reconciliador.py:644–657,670–676` · `app/chamados/models.py:2071–2097,2830–2848,2874–2878`
- **Campo com dois significados — PREEXISTENTE.** Validação física e vigente podem divergir; a reaplicação do filtro físico é **NOVA**. Dia civil e dia do turno já têm o discriminador `conversa`. O carimbo indica processamento; o lastro depende também da via e do fato. `app/chamados/reconciliador.py:647–649,689–703` · `app/chamados/models.py:3699–3709,4382–4399` · `app/chamados/juizes.py:337–341,684–699,1455–1496`
- **Pouso antes do pré-requisito — [nao verificado].** A regra exige pré-verificação, suíte inteira verde e depois Opus final. O `GREEN rc 0`, os quatro `rc 1` e o `Ran 14` informa

[truncado em 20000 de 20796 caracteres]

## 21/11 20:24 · sessao b1414da7abbd · - · - · origem=Ronald (tmux)

**(a)** A prévia lê `classificar_export` sobre os fechamentos da empresa e competência, sem filtro de status. O retrato lê `linhas_do_retrato` e o carimbo `lavrado_em` da foto. `montar_linhas` reconstrói em memória pelo mesmo classificador e permite comparar `status/motivo` em um snapshot comum. As datas gravadas vêm de `datas` e os motivos diários atuais vêm de `_dias_que_retem`. A única troca agregada de novembro em `emp2` consta de `tabelas.txt`, seção (e), mas sua atribuição ao pk 956, a data do furo e o evento que mudou o veredito estão [nao verificado]. `app/folha/views.py:350`, `app/folha/export.py:765`, `app/folha/views_retrato.py:194`, `app/folha/retrato_folha.py:98`, `app/folha/retrato_folha.py:1212`, `app/folha/retrato_folha.py:1237`, `app/folha/models.py:356`, `app/folha/fora_da_folha.py:115`.

**(c)** Use `janela_atual(31/10/2026, empresa)`, `celulas_do_periodo([265], dia, dia)`, `vinculo_do_dia_carregado`, a grade de `carga_da_tarefa(...).grade(colab)` e `tarefa_do_dia`. A razão distingue célula ausente de ata sem lâmpadas, marco ainda sem vencimento e outras guardas. O espelho também tem uma guarda de template, recorte da grade e fallback identificado por `grade_pelo_builder`. `InsumosDoDono(..., espelho=espelho).dono(dia)` lê o juiz do E6 com os insumos reais. O resultado e a causa específica de 31/10 no pk 265 estão [nao verificado]. `app/ponto/janelas.py:60`, `app/escala/alimentacao.py:54`, `app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:316`, `app/chamados/services/tarefa_do_dia.py:214`, `app/chamados/services/tarefa_do_dia.py:635`, `app/chamados/services/tarefa_do_dia.py:825`, `app/ponto/services/espelho.py:489`, `app/ponto/services/espelho.py:523`, `app/ponto/services/espelho.py:551`, `app/ponto/management/commands/e6_oraculo.py:182`.

**Integração** A marca literal `Sem porta` vem da data em `dias_sem_porta` do retrato. O link complementar de cadastro pode coexistir com essa marca, e `o_que_segura` usa `tipo='falta'` na projeção diária, então compare também o resultado de `portas_da_linha` aplicado à linha real. A tela de vínculo é `colaboradores:painel_vinculo`, e o formulário usa `colaboradores:vincular_escala`. `app/templates/folha/_retrato_o_que_fazer.html:46`, `app/folha/fora_da_folha.py:286`, `app/folha/retrato_folha.py:1003`, `app/folha/retrato_folha.py:1066`, `app/colaboradores/urls.py:12`, `app/templates/colaboradores/partials/painel_vinculo.html:136`.

O envelope executa a sonda no tenant `juliani` em uma rede dedicada ao Postgres. A própria inicialização altera credencial, privilégios e arquivos da infraestrutura e grava uma trilha. A inspeção desta volta não executou o envelope. `bin/sonda_leitura.sh:121`, `bin/sonda_leitura.sh:153`, `bin/sonda_leitura.sh:184`, `bin/sonda_leitura.sh:235`, `bin/sonda_leitura.sh:245`.

## 21/11 19:55 · sessao 7a747065f61e · E4C-F5-CONSTROI · verificador · origem=Code

**BLOQUEIA — E4C-F5, item 1.2, volta 5.** A trava compartilhada amplia a cobertura e detecta a mudança de escopo antes do carimbo. Ficam três bloqueios na leitura da cópia. `app/chamados/services/materializacao.py:2130–2160` · `app/chamados/services/validacao.py:713–746`

1. A guarda de declarado × gravado ainda elege a irmã sem consumir a eleição confirmada. O plantio elege de novo e pode recusar o par por outro caminho, inclusive cair no plantio avulso. `app/chamados/services/validacao.py:727–731,772–778` · `app/chamados/services/materializacao.py:200–207,783,938`
2. A exceção sai da porta pública sem tradução nos chamadores. Correção, declaração em texto e upload já fizeram gravações em transações separadas quando ela sobe. `app/chamados/services/validacao.py:713–718` · `app/chamados/services/acoes_disputa.py:359–387` · `app/chamados/services/declaracao_texto.py:656–699` · `app/api/foto_recebida.py:38–54`
3. O RED força a janela de concorrência por `UPDATE` direto, usa um clone terminal e verifica a recusa, mas não observa o conjunto de linhas travadas pela porta pública. Execução no RED, na cura, com M1/M2 e na suíte da árvore [nao verificado]. `app/chamados/tests/test_e4c_f5_validacao_plantio.py:77–149` · `app/chamados/services/materializacao.py:1314–1322` · `CLAUDE.md:372–381`

A comparação com a base usa o diff fornecido. A identidade Git e a ancestralidade da cópia estão [nao verificado]. A ocorrência e a frequência da corrida em produção estão [nao verificado].

### 1. Quem escreve e quem lê a eleição

- **Quem elege.** `metade_irma_do_intervalo` é a função de eleição. Busca, por PK, as perguntas de hora do motivo complementar, na mesma disputa e no mesmo `data_evento`, ainda sem materialização. Entre essas linhas, elege somente se houver uma única candidata dentro do fato, fora de voo e respondida pelo C21 sobre `DecisaoPergunta.composta`. `irma_da_validacao` acrescenta a guarda de hora declarada e delega ao mesmo seletor. Na cura, a porta pública deixou de chamá-la. `app/chamados/services/materializacao.py:136–157` · `app/chamados/models.py:4382–4390` · `app/chamados/services/validacao.py:191–212,713–716`
- **Quem publica e consome.** `travar_o_ato_do_plantio` elege antes do primeiro `FOR UPDATE`, reelege depois e devolve um mapa em memória indexado pelo PK da principal. `usar_irmas_confirmadas_no_plantio` instala o mapa no `ContextVar` e o restaura no `finally`. `par_do_intervalo` consulta o banco de novo e compara a candidata com o mapa, quando este está instalado. `_validar_o_par` chama esse leitor e carimba a irmã pelo escritor canônico. A guarda da escrita chama o mesmo leitor com `irma_validada=True` e materializa a irmã pela mesma unidade. `app/chamados/services/materializacao.py:200–215,1768–1788,1832–1842,2116–2160` · `app/chamados/services/validacao.py:161–175,643–659`
- **Leitores no mesmo ato.** A espera U163 pode eleger via testemunha antes de o mapa ser instalado. Mais tarde elegem `_validar_o_par`, a testemunha consultada por `espera_na_escrita` e `_gravar_irma_do_par`, conforme o ramo. Com insumos estáveis, usam o mesmo seletor. Se o conjunto mudar, o leitor sem mapa pode aceitar outra irmã, enquanto o leitor com mapa a rejeita. O teste do helper demonstra essa diferença ao tornar uma candidata única dentro do próprio `atomic`. Se a releitura já retornar `None`, `par_do_intervalo` sai por “falta a outra metade” antes de comparar o mapa. Fora do ato, card, fio, explicador e contador também consultam a testemunha. `app/chamados/services/validacao.py:727–731,772–778` · `app/chamados/services/materializacao.py:200–212,356,776–799,934–959,1776` · `app/chamados/tests/test_e4c_f5_plantio_concorrente.py:175–187` · `app/chamados/templatetags/chamados_tags.py:120–136` · `app/chamados/services/fio.py:378` · `app/chamados/services/explicador.py:120–132`
- **Quem escreve os insumos.** A emissão preenche motivo, tipo, disputa, destino, dia e clones no nascimento. O reapontamento e o reuso podem trocar o destino, e o rechaveamento altera o dia. Resposta, reabertura e carimbo alteram a elegibilidade. Os consumidores dos vínculos incluem a trava e o fechamento do destino e dos clones. A foto da trava compara identidade e conjunto de chamados. Uma troca de texto que conserva a mesma eleição passa por essa comparação e a porta usa a resposta relida. `app/chamados/services/disputa_emissao.py:837–860,1478–1537,1642–1684` · `app/chamados/models.py:3194–3234,3324–3380` · `app/chamados/services/fio.py:978–981,1090–1096` · `app/chamados/services/materializacao.py:1314–1322,2116–2127` · `app/chamados/services/validacao.py:716–735`
- **K6.** O default `_IRMA_A_PROCURAR` resulta em `irma=None`, sem eleição. A porta e a testemunha usam a mesma guarda de entrega. Para uma irmã elegível, disputa, dia do fato e competência coincidem, e K6 consulta toda metade de intervalo, inclusive U163. `app/chamados/services/validacao.py:203–231,739–743,779–783` · `app/chamados/services/materializacao.py:149–157,802–842,916–919`

### 2. Universo e ordem das travas

- **Comparação com a trava antiga.** Em estado estável, nenhum chamado ou disputa do conjunto antigo sai. Continuam a disputa da principal, o pai, o destino, seus clones, os vínculos da irmã eleita e `abertas_do_colaborador=True`. A nova inclui os vínculos de **todas** as candidatas SQL, mesmo sem eleição ou sem resposta, e trava explicitamente a principal e essas candidatas. A flag inclui disputas abertas, os pais delas e os chamados vivos do colaborador. Se o escopo relido mudou, a tentativa é abortada. `app/chamados/tests/test_e4c_f5_validacao_plantio.py:27–38` · `app/chamados/services/materializacao.py:149–157,2116–2160` · `app/chamados/models.py:2875–2904`
- **Ordem da aquisição inicial.** Os três chamadores usam a mesma sequência `Colaborador → Disputa → Chamado → Pergunta`, com PK crescente em cada consulta. A porta pública e o laço da disputa passam uma principal por chamada. `reparar` passa todas as principais do grupo colaborador–competência antes de escrever. Inline, lote web, copiloto e comando iteram na ordem de seus itens, com a trava por ato, sem uma transação abrangendo o lote. `app/chamados/models.py:2862–2904` · `app/chamados/services/validacao.py:713–716,1060–1082` · `app/chamados/services/materializacao.py:2205–2226` · `app/chamados/services/passivo_validado.py:375–410` · `app/chamados/views_cobrar.py:202–216,374–388` · `app/chamados/management/commands/validar_classe_a.py:100–108`
- **A ordem inicial não certifica a transação inteira.** No aceite de folga, a nova trava já tomou a principal quando `fechar_o_fio_do_dia` pode pedir outra disputa fechada com pergunta viva ou um destino terminal de outra pergunta do dia. O aceite antigo já podia fazer esse pedido depois do `UPDATE` da principal. O reconciliador também pede a ordem global na retratação, enquanto `registrar_batida` grava a batida antes dos receivers síncronos e não adquire por si a trava do colaborador. No plantio, o colaborador já está travado quando esse receiver roda. A serialização cobre escritores que tomam primeiro o mesmo colaborador. Ausência de inversão ou deadlock entre **todos** os escritores [nao verificado]. `app/chamados/services/validacao.py:527–543,561–576,713–738` · `app/ponto/services/veredito_celula.py:174–190` · `app/chamados/models.py:3664–3668` · `app/ponto/registro_batida.py:162–181` · `app/ponto/signals.py:206–218` · `app/chamados/reconciliador.py:538–547,639–646`

### 3. Recusas, trilha e `on_commit`

- **Agora há trava nas recusas de leitura.** `sem_resposta`, `ja_validada`, declarado × gravado, implausível e a prévia de folga vêm depois da trava e da releitura. Em estado estável, os retornos dessas guardas conservam estado e mensagem. Sob concorrência, a releitura pode mudar a decisão, e `AtoDoPlantioMudou` pode sair antes de qualquer uma dessas respostas. A busca inválida continua sendo tratada na entrada. `app/chamados/services/validacao.py:697–743` · `app/chamados/services/materializacao.py:2145–2158`
- **K6 já rodava dentro do `atomic` antigo.** Continua antes do carimbo e, no ramo de hora, depois do plantio, onde a recusa marca rollback. Cai a consulta redundante da irmã em cada verificação. A trava cobre as quatro tabelas do ato, enquanto a entrega é lida em `ExportacaoDominio`, cuja gravação e invalidação têm portas próprias. A trilha de devolução continua condicionada à diferença entre as fotos. A foto inicial agora é tirada sob a trava e reaproveita a leitura composta. `app/chamados/services/validacao.py` base, hunk `:694` do diff fornecido · `app/chamados/services/validacao.py:178–188,298–318,739–746,779–786` · `app/chamados/models.py:2875–2904` · `app/folha/export.py:580–594` · `app/folha/models.py:143–188`
- **Folga tem mudança de efeito.** Sem transação externa da requisição, o ramo “já era folga” antes confirmava o bloco de fechamento e disparava seus callbacks antes de `val_perg_folga_ja`. Agora essa trilha entra na transação da validação e precede os callbacks. No ramo “folga declarada”, o recálculo é enfileirado antes do commit externo, onde antes era chamado após o commit interno e executava de imediato em autocommit. O beco da pergunta e a retomada da disputa usam `on_commit`. O outbox da pergunta é síncrono e fica no mesmo rollback da escrita. O rollback do ramo `folga_recusada` continua desfazendo o carimbo tentado. `app/chamados/services/validacao.py:527–551,561–617,713–738` · `app/chamados/signals.py:122–146,229–259` · `app/ponto/services/fechamento.py:1798–1827` · `app/core/observ.py:124–139`

### 4. Quem levanta, quem captura e o que fica gravado

`travar_o_ato_do_plantio` levanta por mudança no escopo da principal, na irmã eleita ou no conjunto de candidatas. O passivo também levanta na reconferência da classificação e captura fora do `atomic`, contabilizando e seguindo para o próximo grupo. O laço da disputa captura dentro do `atomic` por item, acrescenta a mensagem a `erros` e segue. A chamada inicial de `validar_pergunta` propaga a exceção. `app/chamados/services/materializacao.py:2145–2158,2210–2215` · `app/chamados/services/passivo_validado.py:392–415,457–468` · `app/chamados/services/validacao.py:700–718`

| Entrada | Efeito se a validação levantar |
|---|---|
| Detalhe | A exceção sobe antes de a casca produzir o retorno do card. `app/chamados/views.py:483–501` · `app/chamados/services/acoes_disputa.py:13–30` |
| Fio e modal | Sobe antes do parcial e da trilha específica de validação em disputa fechada. `app/chamados/views.py:667–674,951–954` · `app/chamados/services/fio.py:634–667` |
| Inline | Interrompe o laço antes do JSON. Validações anteriores já confirmadas ficam gravadas. `app/chamados/views_cobrar.py:202–216` · `app/chamados/services/validacao.py:713–789` |
| Correção de horário | A nota do contraditório, a resposta corrigida e a trilha da correção precedem a validação. A transação da resposta já terminou. A exceção sobe e a correção fica sem a validação desta tentativa. O retorno “Corrigi a resposta, mas a gravação recusou” só existe se a porta devolver um dict. `app/chamados/services/acoes_disputa.py:359–389` · `app/chamados/services/disputa_emissao.py:1478–1538` |
| Declaração em texto | O bloco que pode emitir a pergunta e registrar a resposta termina antes de validar. A exceção sobe ao detalhe, preservando o que esse bloco gravou. `app/chamados/services/declaracao_texto.py:656–699` · `app/chamados/views.py:478–481` |
| Upload de foto | Os dois endpoints anexam a foto antes da resolução. `Batida.foto`, `FotoBatida` e eventual resposta automática já gravada sobrevivem. A exceção interrompe o processamento antes do retorno de sucesso. `foto_ausente` fica exposta à mudança de escopo **da principal**, independentemente de eleição de irmã. `app/api/views.py:839–863` · `app/api/views_core.py:823–847` · `app/ponto/registro_batida.py:316–332` · `app/api/foto_recebida.py:38–54,65–87` · `app/chamados/services/disputa_emissao.py:1689–1716` |
| Lote web e copiloto | O item interrompe os lotes antes do resumo. Itens anteriores já confirmados persistem. No chat, a proposta é removida da sessão em memória antes de aplicar. Persistência dessa remoção na resposta de erro [nao verificado]. `app/chamados/services/validacao.py:1060–1092` · `app/chamados/views_cobrar.py:258–269,374–388` · `app/core/views_ia.py:142–176,870–881` |
| Comando | Interrompe `--apply` antes do contador final e da mensagem `APLICADO`. Os itens anteriores podem já ter confirmado. `app/chamados/management/commands/validar_classe_a.py:88–108` |

A separação entre gravação prévia e validação é **preexistente**. O caminho novo é a propagação de **`AtoDoPlantioMudou` pela primeira trava**. A trava antiga da porta pública não levantava essa classe, embora já pudesse propagar falha de banco na aquisição. O laço posterior já convertia essa classe em erro por item. `app/chamados/tests/test_e4c_f5_validacao_plantio.py:27–38` · `app/chamados/models.py:2875–2904` · `app/chamados/services/validacao.py:713–718,789–790` · `app/chamados/services/materializacao.py:2210–2215`

### 5. RED, mutantes, controle e limpeza

- **RED.** O teste chama a porta pública. Após observar a eleição, o gancho espera o worker sair do `atomic(durable=True)` antes de chamar a trava real. Exige dois PIDs PostgreSQL distintos, a exceção esperada, o vínculo concorrente persistido e ausência de batidas e atos de validação novos. Isso especifica um interleaving real de duas transações. O escritor é sintético e o clone resolvido é excluído pelo fechamento dos clones. O teste vizinho exercita a porta real de reabertura concorrente pelo **laço da disputa**, em vez da entrada pública. Correspondência com um incidente de produção [nao verificado]. `app/chamados/tests/test_e4c_f5_validacao_plantio.py:77–142` · `app/chamados/services/materializacao.py:1314–1322` · `app/chamados/tests/test_e4c_f5_plantio_concorrente.py:134–173`
- **M1.** Move a guarda `espera_declarado_x_gravado` para fora do `atomic`. A mordida esperada é `all(leituras_em_atomic) == False`, desde que essa guarda eleja a volta no cenário. Preserva a trava compartilhada e deixa a corrida produzir a exceção esperada. **M2.** Recoloca os argumentos da trava antiga dentro do `atomic`, retira a reconferência e instala `None` no contexto. A mordida esperada é o retorno normal com par gravado, falhando no `assertRaisesRegex`. M2 desliga as duas proteções juntas, portanto não isola cada uma. As duas transformações aplicaram e compilaram em memória. O resultado das asserções em PostgreSQL [nao verificado]. `app/chamados/tests/test_e4c_f5_validacao_plantio.py:19–58,106–142` · `app/chamados/tests/cortes_f2_f9_apoio.py:82–107`
- **Controle e limpeza.** O controle exige o PK da irmã no retorno, os dois carimbos e via, duas batidas com origem nas perguntas, `S 01:00` e `E 02:00` no D+1 civil, e sequência `E–S–E–S`. A classe usa `TransactionTestCase` para limpar dados confirmados, fecha a conexão do worker em `finally` e registra a retirada dos patches da fixture com `addCleanup`. O relógio Python e o fuso estão fixados. O SQL do worker usa timeout, PID e atualização de vínculo, sem consultar a hora do servidor. A limpeza em execução [nao verificado]. `app/chamados/tests/test_e4c_f5_validacao_plantio.py:61–87,144–149` · `app/chamados/tests/test_bug_par_projetado.py:44–55,82–115`

### 6. Checklist 5(a) e ESMERIL-SEMÂNTICA

| Checagem | Resultado |
|---|---|
| `except` e fallback | **0 `except` adicionados em produção e 0 em teste** no diff fornecido. O `finally` do worker só fecha a conexão. A primeira exceção sobe na porta pública. O fallback avulso já existia, mas ganha um gatilho efetivo nessa porta: uma irmã diferente da confirmada vira impedimento retornado pelo leitor e pode levar a `_materializar_uma` avulsa. `app/chamados/services/validacao.py:700–718,772–778` · `app/chamados/services/materializacao.py:202–207` · `app/chamados/tests/test_e4c_f5_validacao_plantio.py:77–87,122–130` |
| Segundo juiz, escritor e derivação | O diff reutiliza C21, K6, carimbo e derivador de horário. A duplicidade encontrada é de **leitura da eleição** com escopos distintos. O `UPDATE` direto do vínculo está no teste. `app/chamados/services/validacao.py:687–695,716–746,773–778` · `app/chamados/services/materializacao.py:202–207,223–248,802–842,925–938` · `app/chamados/tests/test_e4c_f5_validacao_plantio.py:77–87` |
| Relógio fora da borda | Zero leitura nova de relógio no diff de produção. O carimbo continua sob a trava. A prévia de folga mantém sua leitura preexistente de `localdate()` e pode rodar novamente dentro da declaração. O `localdate(ts)` em `reparar` converte o instante recebido para medir divergência. `app/chamados/services/validacao.py:161–175,713–746` · `app/ponto/services/declarar_dia.py:126–168` · `app/chamados/services/passivo_validado.py:325–327,353–358` |
| Selo e ordem | A mudança `356 → 357` no registro B1 aponta para a ocorrência real de `localdate(ts)` e o selo compara as linhas por igualdade. O teste vizinho verifica SQL, ordem e PKs das perguntas no helper. O RED público verifica reconferência, mas deixa sem asserção direta os PKs de chamados e disputas travados. `app/core/tests/test_medida_turno_do_par.py:270–274,558–562` · `app/chamados/services/passivo_validado.py:353–358` · `app/chamados/tests/test_e4c_f5_plantio_concorrente.py:105–117,189–202` · `app/chamados/tests/test_e4c_f5_validacao_plantio.py:112–142` |
| Unidade, dia e bordas | A unidade é uma pergunta com eventual par complementar do mesmo turno. K6 usa `data_evento` e a janela da empresa, com corte e véspera do próximo corte inclusivos. O plantio pode cair em D+1 civil, e F6 usa a data local do instante. Declarado × gravado compara `HH:MM` local e tipo. A saída deve anteceder estritamente a volta. O reuso inclui as duas pontas da tolerância, e a busca de vizinhas exclui o instante central. `data_evento` já tem o sentido de dia civil nas conversas, discriminadas por `conversa`, e a emissão recusa pergunta de hora sem dia do turno. Nenhum segundo significado foi introduzido pelo diff. `app/chamados/services/materializacao.py:109–122,139–157,234–244,701–720,750–759,802–861` · `app/chamados/juizes.py:238–250` · `app/ponto/janelas.py:10–33,60–63,75–85` · `app/chamados/models.py:2993–3012` · `app/chamados/services/disputa_emissao.py:430–450` |

## 21/11 19:46 · sessao f3277b50dc51 · E4C-F5-CONSTROI · construtor · origem=Code

**INCOMPLETO — E4C-F5 1.2, volta 5.** A cópia contém o RED, a cura, os dois mutantes e o ajuste da catraca. O RED foi escrito primeiro e pode ser aplicado sozinho sobre `4ab051ebd`. A entrega está em diff local, sem commits, conforme o veto da obra. A certificação da corrida, dos mutantes e dos nove testes herdados pela pista segue [nao verificado]. `chamados/tests/test_e4c_f5_validacao_plantio.py:89`, `chamados/services/validacao.py:713`, `core/tests/test_medida_turno_do_par.py:274`.

### Leituras e trava

| Insumo | Na base `4ab051ebd` | Na cura |
|---|---|---|
| Irmã | A prévia podia eleger a irmã e havia uma leitura explícita antes do `atomic`. A trava direta recebia os chamados dessa leitura. `validacao.py:714`, `validacao.py:743`, `validacao.py:752` | A trava compartilhada elege e reconfere dentro do `atomic`. O plantio usa a eleição confirmada. `validacao.py:713`, `validacao.py:773` |
| Resposta vigente | `DecisaoPergunta.composta` decidia resposta e validação antes do `atomic`. `validacao.py:697` | A principal é relida depois da trava e a resposta vigente é consultada antes do carimbo. `validacao.py:716` |
| Slot, conferência de data e batidas | A prévia anterior ao `atomic` consultava a derivação do slot, `data_conferida_em`, batida existente e vizinhança. O plantio voltava a consultar essas condições dentro do `atomic`. `validacao.py:714`, `materializacao.py:171`, `materializacao.py:920`, `materializacao.py:1947` | A prévia, a plausibilidade e o plantio consultam os juízes existentes após a trava. `validacao.py:725`, `validacao.py:772` |
| Competência e exportação | A prévia consultava K6 e F6 antes do `atomic`. O K6 explícito rodava sob a trava direta, antes e depois do plantio de hora. `materializacao.py:916`, `materializacao.py:940`, `validacao.py:757`, `validacao.py:794` | O K6 explícito roda sob a trava compartilhada e usa a competência da principal. A eleição já exige o mesmo dia do fato para a irmã. `validacao.py:739`, `validacao.py:779`, `materializacao.py:149` |

`travar_o_ato_do_plantio` reconfere o escopo da principal, a irmã eleita e todas as candidatas, incluindo seus vínculos com disputa, colaborador, chamado pai, destino e clones, além de motivo, dia e tipo de resposta. A ordem é Colaborador → Disputa → Chamado → Pergunta, por PK crescente em cada grupo. A comparação cobre a elegibilidade que muda a eleição. Texto ou horário que preserve a eleição, slot derivado, competência, exportação, conferência e batidas ficam fora dessa comparação. A cura consulta esses insumos depois da trava pelos leitores existentes. `chamados/services/materializacao.py:139`, `chamados/services/materializacao.py:2116`, `chamados/models.py:2839`, `chamados/services/validacao.py:716`.

### Chamadores

- **Detalhe, fio/modal e validação inline.** A casca traduz o dicionário de retorno. `AtoDoPlantioMudou` sobe antes dessa tradução, com 500 esperado [nao verificado]. `chamados/services/acoes_disputa.py:13`, `chamados/views.py:501`, `chamados/views.py:954`, `chamados/services/fio.py:658`, `chamados/views_cobrar.py:210`.
- **Correção de horário, declaração em texto e upload de foto.** A exceção também sobe, com 500 esperado [nao verificado]. Esses caminhos escrevem antes de chamar a validação, portanto a recusa do plantio pode chegar depois de a resposta ou foto já ter sido gravada. `chamados/services/acoes_disputa.py:374`, `chamados/services/declaracao_texto.py:689`, `api/foto_recebida.py:48`, `api/views.py:850`, `api/views_core.py:834`.
- **Lote web, copiloto e comando.** A exceção interrompe a iteração. O lote web e o copiloto ficam sem o resumo, com 500 esperado [nao verificado]. O comando sai antes do balanço final. `chamados/services/validacao.py:1063`, `chamados/services/validacao.py:1082`, `chamados/views_cobrar.py:263`, `chamados/views_cobrar.py:383`, `core/views_ia.py:169`, `chamados/management/commands/validar_classe_a.py:100`.
- **Testes.** Os existentes exercitam a chamada direta e a view. O novo RED exige `AtoDoPlantioMudou` e verifica batidas, carimbos e atos de validação intactos. O controle exige as duas batidas e os dois carimbos. `chamados/tests/test_bug_par_projetado.py:136`, `chamados/tests/test_guard_s116_detalhe_local.py:63`, `chamados/tests/test_e4c_f5_validacao_plantio.py:122`.

Proposta para a borda, pendente de aval: um `process_exception` tipado pode traduzir `AtoDoPlantioMudou` em HTTP 409 com mensagem para recarregar o caso, sem `except` novo. Para lotes, a mensagem deve pedir releitura do estado antes do reenvio, pois itens anteriores podem ter sido aplicados. `config/settings/base.py:53`, `chamados/services/validacao.py:1063`, `chamados/services/validacao.py:1082`.

### Provas para a sessão

Use `copia_red` com a base `4ab051ebd` e apenas o novo arquivo de teste. O RED deve falhar porque o par gravou e a exceção esperada não subiu. Na cura, RED e controle devem passar. O m1 deve falhar em `a irmã foi lida fora do atomic`. O m2 deve falhar pela ausência de `AtoDoPlantioMudou` após a gravação do par. Resultados na pista [nao verificado]. `chamados/tests/test_e4c_f5_validacao_plantio.py:19`, `chamados/tests/test_e4c_f5_validacao_plantio.py:27`, `chamados/tests/test_e4c_f5_validacao_plantio.py:122`, `bin/suite.sh:317`.

```bash
copia_red=/caminho/da/copia-red
copia=/home/ronald/wt-codex-e4c-f5-constroi-193405
modulo=chamados.tests.test_e4c_f5_validacao_plantio
red=test_RED_validacao_recusa_irma_com_escopo_alterado_em_outra_conexao

bin/suite.sh --dir "$copia_red" --only chamados --espera 7200 -- -k test_e4c_f5_validacao_plantio
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k test_e4c_f5_validacao_plantio
bin/suite.sh --dir "$copia" --only chamados --espera 7200 --parallel 1 -- --testrunner "$modulo.RunnerMutanteM1" -k "$red"
bin/suite.sh --dir "$copia" --only chamados --espera 7200 --parallel 1 -- --testrunner "$modulo.RunnerMutanteM2" -k "$red"
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k test_e4c_f5_passivo_releitura
bin/suite.sh --dir "$copia" --only core --espera 7200 -- -k test_medida_turno_do_par
```

**Verificação local.** Conferi as 94 entradas da catraca. A única divergência era `passivo_validado.py::reparar` B1, de `[356]` para `[357]`. Após o ajuste, os 35 testes da catraca passaram no modo em memória. As duas mutações também compilaram em memória. `core/tests/test_medida_turno_do_par.py:20`, `core/tests/test_medida_turno_do_par.py:274`, `core/tests/test_medida_turno_do_par.py:558`, `chamados/services/passivo_validado.py:357`.

**Diff e autoconferência.** 3 arquivos, **+195/−60**. Cura **+45/−59**, RED com mutantes **+149/−0**, catraca **+1/−1**. `except` novos no diff **0 em produção, 0 em teste**. Fallbacks novos **0**. `chamados/services/validacao.py:677`, `chamados/tests/test_e4c_f5_validacao_plantio.py:1`, `core/tests/test_medida_turno_do_par.py:274`.

## 21/11 19:34 · sessao 8b7779573fa6 · FERIADO-POR-POSTO · leitura · origem=Code

# FERIADO-POR-POSTO — matriz da porta da marca

**Na cópia `302ca7e77`, (i) está aberto. (ii), (iii) e (iv) estão curados por inspeção do código.** Quatro testes do anexo ainda acusam falhas nos caminhos adjacentes de presença, vínculo e sincronização. A matriz identifica esses caminhos separadamente. [celula.py:392,464] [marcar_feriado_do_posto.py:118–127] [feriado_do_posto.py:305–306,379–383,450–475] [feriado_do_posto.py:74–89] [vinculo.py:896–908] [sincronizar_feriado_admin.py:52–72]

Li as duas cópias nomeadas pelos hashes, em somente leitura. O main `ab351aa9b` já tem o comando antigo, que grava o posto e retrata diretamente. A porta de serviço que propaga a chave e a API de prévia aparecem na cópia da obra. `celula.py` e `cadastro_futuro.py` são idênticos nas duas cópias. A releitura já existia no main, e o que nasce nesta obra é o caminho **marca → propagação → releitura**. A cadeia Git `1c3647461` → `302ca7e77`, o pouso, a execução dos testes e os efeitos em produção ficam **[nao verificado]**. [main/app/colaboradores/management/commands/marcar_feriado_do_posto.py:62,129–149] [main/app/colaboradores/services/feriado_do_posto.py:136–158] [main/app/ponto/portas/celula.py:392,464] [feriado_do_posto.py:288–406]

**Referências da matriz.** `CLI` = `app/colaboradores/management/commands/marcar_feriado_do_posto.py`, `PORTA` = `app/colaboradores/services/feriado_do_posto.py`, `VÍNCULO` = `app/colaboradores/services/vinculo.py`, `CÉLULA` = `app/ponto/portas/celula.py`, `FUTURO` = `app/ponto/services/cadastro_futuro.py`, `SYNC` = `app/colaboradores/management/commands/sincronizar_feriado_admin.py`. Os arquivos de prova abreviados a seguir ficam em `app/colaboradores/tests/`: `FPP` = `test_feriado_por_posto.py`, `PRÉVIA` = `test_feriado_previa_da_marca.py`, `A5` = `test_feriado_a5_volta3.py`, `V4` = `test_feriado_volta4.py`, `V5` = `test_feriado_volta5.py`. **SEM PROVA** indica que não localizei teste que fixe aquela transição específica.

## Matriz — CLI e aplicação

| Linha | Estado de entrada | Juiz consultado | Operação | Persistido | Retorno ao chamador | Prova existente |
|---|---|---|---|---|---|---|
| C1 | Sem `--posto`, inclusive com `--apply` | `PORTA:74,137::presenca_em_feriado/sugestoes`, com `vinculo_do_dia` em `app/escala/alimentacao.py:228` e calendário em `app/core/feriados.py:70::feriados_de` | Lê presença e lista sugestões. Com `--sugestoes` e posto, lista e segue para a ação. `CLI:46–68` | Nada. `CLI:49–68` | Sem posto, aviso `Nada marcado` e retorno normal. `CLI:64–68` | A passagem com posto e a data da borda são fixadas por `A5::RelogioDoLote.test_sugestoes_e_propagacoes_usam_o_dia_da_borda_ao_virar_meia_noite:245`. Saída sem posto **SEM PROVA** |
| C2 | Posto informado, ambas as ações ou nenhuma | Guarda de argumentos, `CLI:70–72::Command.handle` | Imprime `Diga --fecha OU --opera, um so.` | Nada. `CLI:70–72` | Retorno normal, apesar do estilo de erro. `CLI:71–72` | **SEM PROVA** |
| C3 | Ação `--opera` com `--retomar-retratacao` | Guarda de argumentos, `CLI:73–75::Command.handle` | Recusa antes de buscar o posto | Nada. `CLI:73–75` | `CommandError('--retomar-retratacao exige --fecha')` | **SEM PROVA** |
| C4 | PK de posto inexistente | Busca do cadastro, `CLI:111–115::Command._marcar` | Imprime `posto … nao existe.` | Nada. `CLI:111–115` | Retorno normal, com ou sem `--apply`. `CLI:113–119` | **SEM PROVA** |
| C5 **(ii)** | `--fecha` ou `--opera`, `--apply`, motivo ausente ou só espaços, posto encontrado | `PORTA:288,305–306::aplicar_marca` | Recusa o motivo antes da trava e do plano | Nada. `PORTA:305–315` | `MarcaInvalida('aplicar_marca: motivo e obrigatorio')` → `CommandError` com causa, antes de imprimir o plano. `CLI:118–130` | `FPP::CascaDoComando.test_apply_sem_motivo_recusa_sem_escrever:645`, com `--opera` e o mesmo ramo de `--fecha`. `FPP::R1TrilhaDaTrocaDaChave.test_R1_motivo_e_obrigatorio:172` fixa a porta |
| C6 | `--fecha --apply` com motivo, posto sem município | `PORTA:261,269–271::plano_da_marca` | Relê posto sob trava e recusa FECHA | Nada. `PORTA:307–323` | `MarcaInvalida('Posto SEM MUNICIPIO …')` → `CommandError`. `CLI:126–127` | `V4::MarcaInvalidaSaiComErro.test_fecha_em_posto_sem_municipio_levanta_command_error:17` e `FPP::R3FechaRetrataEPropaga.test_R3_fecha_sem_municipio_recusa_sem_escrever:250` |
| C7 | Porta Python com `novo` fora de `True`/`False`, `hoje=None`, ou retomada sem posto já FECHA. Reaplicação de posto sem marca | `PORTA:261–275::plano_da_marca` e `PORTA:308–315,357–359::aplicar_marca/reaplicar_marca` | Recusa após reler o posto, antes de observar ou gravar o plano | Nada. `PORTA:307–323` | `MarcaInvalida` na API. Se a entrada alcançar a CLI, C6 dá a tradução. `CLI:126–127` | `FPP::R3FechaRetrataEPropaga.test_R3_voltar_a_sem_marca_nao_existe:261`, `test_R3_hoje_e_obrigatorio:271`, `FPP::R4ReaplicarAMarcaVigente.test_R4_reaplicar_sem_marca_recusa:357`. Retomada inválida **SEM PROVA** |
| C8 | `--fecha` ou `--opera`, sem `--apply`, com ou sem `--motivo` | `PORTA:370–406::previa_da_marca`, P1–P4 | Chama a prévia com `hoje` e `agora`. O motivo da CLI só é lido no `--apply`. `CLI:117–127` | Nada após a prévia. `PORTA:397–406` | Sucesso na sombra imprime plano, grupos e `DRY-RUN`. Fora da sombra ou marca inválida, `CommandError`. `CLI:124–146` | `PRÉVIA::PreviaNaSombra.test_dry_run_do_comando_le_a_previa_e_nao_escreve:217` e `PRÉVIA::PreviaForaDaSombraRecusa.test_PROVA_a_comando_sai_com_erro_sem_chamar_a_porta_e_sem_escrever:277`. Variante FECHA com motivo no dry-run **SEM PROVA** |
| C9 | `--apply` válido | `PORTA:288::aplicar_marca`, M1–M7 | Busca autor por username e aplica. A busca pode entregar `None` à porta. `CLI:119–122` | Os segmentos concluídos de M1–M7. `PORTA:308–354` | Imprime plano, `MARCADO`, retratados e propagados, e retorna normalmente. `CLI:128–150` | `FPP::CascaDoComando.test_apply_chama_a_porta:658`, `PRÉVIA::PreviaForaDaSombraRecusa.test_apply_segue_valendo_fora_da_sombra:317`, e FECHA repetido em `A5::SegmentosDaMarca.test_fecha_ja_fecha_e_reaplicar_preservam_chamado_vivo:105` |

| Linha | Estado de entrada | Juiz consultado | Operação | Persistido | Retorno ao chamador | Prova existente |
|---|---|---|---|---|---|---|
| M1 | Pedido válido, marca muda | `PORTA:237–254,261–285::_chamados_do_posto/plano_da_marca`. `app/chamados/services/painel.py:135::qs_vivos`, `app/chamados/juizes.py:175::dias_do_fato`, `app/escala/alimentacao.py:334::vinculos_dos_dias`, `app/core/feriados.py:70::feriados_de` via `PORTA:47`. Para propagar, `app/escala/alimentacao.py:219::q_vinculo_cobre_a_data` e `PORTA:175,203::segue_a_marca/vinculos_que_seguem_a_marca` | Trava posto, lê o plano e grava a marca. FECHA novo seleciona vivos do posto **do dia do fato** em feriado que o alcança. OPERA tem alvo de retratação vazio. A propagação seleciona chaves divergentes de vínculos weekday vigentes, fora cargos administrativos. `PORTA:277–323` | `Posto.opera_em_feriado` pelo escritor `app/colaboradores/services/estrutura.py:90–95`. `LogAuditoria.marcar_feriado_do_posto` com antes/depois, motivo e ator, em savepoint best-effort. Falha só de auditoria pode deixar a marca gravada. `PORTA:318–323` [app/core/observ.py:94–105,124–139] | Segue para retratação e propagação. O plano é o calculado sob a trava inicial. `PORTA:325–354` | `FPP::R1TrilhaDaTrocaDaChave.test_R1_porta_da_marca_um_evento_no_posto_uma_linha_por_vinculo_2a_rodada_zero:151`, `FPP::R2IntactosNoMesmoAto.test_R2_admin_12x36_outro_posto_e_encerrado_ficam_e_o_irmao_muda:185`, `A5::SegmentosDaMarca.test_fecha_alcanca_chamado_vivo_silenciado:120`, `test_fecha_alcanca_feriado_vivo_anterior_a_180_dias:123` |
| M2 | Mesma marca, com ou sem divergências. Retomada explícita de posto já FECHA | `PORTA:261–285::plano_da_marca` | Mesma marca comum ou reaplicação só propaga divergências. Retomada de FECHA volta a selecionar vivos. `PORTA:272–285,357–359` | Sem novo evento do posto. Se houver trabalho, persistem os segmentos M3–M6 correspondentes. `PORTA:318–354` | `marca_muda=False`, contagens do que de fato avançou. Sem divergência e sem retomada, `0/0`. `PORTA:325–354` | `FPP::R4ReaplicarAMarcaVigente.test_R4_reaplicar_propaga_so_aos_divergentes_sem_evento_do_posto:321`, `A5::SegmentosDaMarca.test_fecha_ja_fecha_e_reaplicar_preservam_chamado_vivo:105`, `FPP::FalhaNoMeioDaPortaRetoma.test_falha_na_retratacao_nao_deixa_nota_sem_chamado_fechado:533` |
| M3 | Alvo de FECHA continua vivo, pertence ao posto do fato e o feriado ainda o alcança. Fechamento autorizado | `app/chamados/models.py:654,676::reconciliar/_decisao_do_fechamento`, `app/chamados/catalogo/motor.py:125::decidir`, guardas de premissa, conversa e pergunta viva em `app/chamados/models.py:776,711,422` | Revalida o alvo sob a trava do posto. Reconcilia via `app/chamados/services/fecho_runtime.py:146::transicionar_com_pauta` e anexa nota da marca no mesmo segmento. `PORTA:325–341` | `ChamadoColaborador.status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`, urgência quando aplicável, auditoria de estado e `FollowupChamado` da marca, com chave de evento por chamado/tipo/dia. [app/chamados/models.py:207–252,286–302] [app/chamados/services/nota.py:75–109] | `reconciliar=True`, soma um a `retratados`. `PORTA:335–341` | `FPP::R3FechaRetrataEPropaga.test_R3_dia_ja_folga_pela_celula_retrata_pela_nota_da_marca:217` |
| M4 | Alvo saiu do recorte sob a trava, ou o gate de fechamento recusa | `PORTA:237–254,331–335::_chamados_do_posto/eh_feriado_do_posto`, ou os gates de M3 | Pula alvo inelegível ou recebe `False` do reconciliador e continua. `PORTA:331–341` | Sem nota da marca e sem fechamento nesse passo. Uma recusa material pode gravar `LogAuditoria.fechamento_recusado` e, na guarda de conversa, nota de retenção no fio. A coleta S143 pode gravar pauta quando houver recusas coletadas. [app/chamados/models.py:409–443,734–752,821–830] [app/chamados/services/fecho_runtime.py:127–142,157–169] | Sem incremento em `retratados`. A propagação pode fechar o mesmo chamado mais tarde pelo evento da célula. `PORTA:335–354` [CÉLULA:528–540] | `FPP::R3FechaRetrataEPropaga.test_R3_dia_trabalho_a_premissa_recusa_e_a_celula_propagada_fecha:232`. Alvo que perde elegibilidade entre leituras **SEM PROVA** |
| M5 | Outra aplicação trocou a marca entre segmentos | `PORTA:328–330,346–348::aplicar_marca`, comparação com a marca relida sob trava | Interrompe o laço afetado ao ver marca diferente | Os segmentos já concluídos permanecem. Nenhuma escrita daquele segmento pela aplicação superada. `PORTA:308–354` | Retorno normal com plano inicial e contagens parciais. `PORTA:354` | `A5::SegmentosDaMarca.test_opera_superada_por_fecha_nao_grava_chave_velha:50`, `test_fecha_superada_por_opera_nao_retrata:73` |
| M6 | Vínculo ainda elegível e divergente, ou já igual | `PORTA:203–219,344–352::vinculos_que_seguem_a_marca/aplicar_marca`, `VÍNCULO:870–925::marcar_trabalha_em_feriado` | Relê elegibilidade e chave sob trava. Se mudou, grava chave e regenera passado/hoje e futuras. Se igual, pula na marca ou retorna `ec` na chamada direta. `VÍNCULO:896–925` | `EscalaColaborador.trabalha_em_feriado`, auditoria `editar` best-effort, invalidação de `FechamentoMensal.previsto_em` com `atualizado_em` pelo signal, e efeitos R1–R6. Tudo no segmento do vínculo. [app/core/auditoria.py:8–30] [app/escala/signals.py:137–151] [app/ponto/services/fechamento.py:1607–1613] | A porta do vínculo devolve `ec`. A marca conta um vínculo após a chamada completar, independentemente de quantas células mudaram. `VÍNCULO:915–925` [PORTA:349–354] | `FPP::R1TrilhaDaTrocaDaChave.test_R1_porta_do_vinculo_grava_UMA_linha_da_troca_com_autor_e_motivo:133`, `FPP::A4ContadorDosDivergentes.test_A4_vigencia_e_inclusiva_nas_duas_pontas_e_so_nelas:609` |
| M7 | Exceção que escapa durante retratação ou propagação | O gate e escritor do segmento em curso, `PORTA:325–353`, `VÍNCULO:870` | Desfaz o segmento atual. Em `--apply` ordinário, a marca e os segmentos anteriores já foram confirmados. Dentro da prévia, P3/P4 abrangem todos os segmentos. `PORTA:308–353,397–406` | Falha na nota desfaz o fechamento daquele chamado. Falha na propagação desfaz a chave, a trilha e a regeneração daquele vínculo. O que o segmento anterior confirmou fica. `PORTA:326–352` [VÍNCULO:870–925] | Exceção sobe. Um `ValueError` puro, por exemplo do invariante em `app/chamados/models.py:227–233`, passa pela CLI sem tradução. `CLI:124–127` | `FPP::FalhaNoMeioDaPortaRetoma.test_falha_na_2a_propagacao_fica_denunciada_e_a_porta_retoma:503`, `test_falha_na_retratacao_nao_deixa_nota_sem_chamado_fechado:533`. `ValueError` tardio específico **SEM PROVA** |

## Matriz — prévia e classificação

| Linha | Estado de entrada | Juiz consultado | Operação | Persistido | Retorno ao chamador | Prova existente |
|---|---|---|---|---|---|---|
| P1 | `HASNER_SOMBRA` ausente ou diferente do booleano `True` | Guarda, `PORTA:370–381::previa_da_marca` | Recusa antes da validação de `agora`, do plano e da marca | Nada. `PORTA:379–398` | `PreviaForaDaSombra` direto na API, `CommandError` na CLI. `CLI:124–125` | `PRÉVIA::PreviaForaDaSombraRecusa.test_PROVA_a_a_funcao_recusa_antes_da_porta:291`, `test_chave_que_nao_e_o_booleano_True_recusa:299`, `test_settings_sem_a_chave_tambem_recusa:310` |
| P2 **(iv)** | Sombra, `agora` omitido ou `None` | Guarda, `PORTA:370,382–383::previa_da_marca` | Recusa antes de chamar a porta da marca | Nada. `PORTA:382–399` | `ValueError('previa_da_marca: agora e obrigatorio')`. Omitir `hoje` dá `TypeError` pela assinatura. `hoje=None` explícito com `agora` válido cai em C7. `PORTA:267–268,370–383` | `V5::RelogioDesceDaBorda.test_previa_da_marca_sem_agora_recusa:18`. Omissão de `hoje` na prévia **SEM PROVA** |
| P3 | Sombra, entrada válida | `PORTA:288::aplicar_marca` e `PORTA:420:: _classificar_previa` | Fotografa os colaboradores de `alvo ∪ propagar` no plano sob trava, executa a porta real, fotografa e classifica dentro do `atomic`. `_DesfazAPrevia` força rollback. `PORTA:390–406` | Nada no banco após a saída. Os `on_commit` da simulação são descartados. `PORTA:397–406` | Dict com `marca_muda`, `retratados`, `propagados`, `plano`, `a`, `b`, `outras`. Contagens da simulação. `PORTA:354,406` | `PRÉVIA::PreviaNaSombra.test_o_plano_devolvido_e_o_do_ATO:127`, `test_PROVA_b_banco_identico_por_chave_e_valor:158`, `test_a_instancia_do_chamador_nao_sai_marcada:185`, `test_PROVA_a_nenhum_on_commit_sobra_da_previa:190`, `PRÉVIA::PreviaDoFechaComChamadoVivo.test_PROVA_b_banco_identico_no_ramo_da_retratacao:253` |
| P4 | Entrada inválida após a guarda, ou falha tardia na simulação/classificação | `PORTA:261–275::plano_da_marca`, ou a função que falhou em `PORTA:397–404` | O `atomic` exterior reverte a execução. Só a sentinela de rollback bem-sucedido é capturada. `PORTA:397–406` | Nada da simulação. `PORTA:397–406` | Exceção original na API. CLI traduz `MarcaInvalida` e `PreviaForaDaSombra`. Demais exceções sobem. `CLI:124–127` | Rollback normal em P3. Erro tardio específico na prévia **SEM PROVA** |
| P5 | Foto igual, ou foto mudou sem passar a trabalho num feriado deste posto | `PORTA:420–449::_classificar_previa`, `PORTA:47::eh_feriado_do_posto`, `app/escala/alimentacao.py:334::vinculos_dos_dias` | Compara `trabalha`, `dna`, `escala_geradora`, `ata`, `veredito`, `julgada_em`, `impressao`, `veredito_em`. Sem diferença, omite. Se mudou e não atende ao recorte, manda para `outras`. `PORTA:409–449` | Nada pelo classificador | `outras` com colaborador, data, antes/depois, campos mudados e indicador de feriado, ou ausência de linha. `PORTA:431–449,478` | `PRÉVIA::PreviaNaSombra.test_celula_que_muda_de_geradora_sem_mudar_trabalha_aparece_em_outras:112`, `A5::ClassificacaoDaPrevia.test_ata_mudada_sem_troca_de_trabalho_aparece:166`. Foto igual isolada **SEM PROVA** |
| P6 **(iii)** | Feriado vira trabalho, há turno do dia, mas o par ainda está antes do fim previsto ou há turno sem par completo | `app/ponto/turnos.py:1850::turnos_do_colab`, `:2133::dias_encerrados`, `:2195::preparar_par_completo_da_janela`, `:2207::par_completo` | Usa os turnos pós-escrita, a mesma janela e `agora` injetado. Entrada e saída formadas pelo pareador ainda precisam passar no juiz. `PORTA:450–473` | Nada pelo classificador | `outras` com `pareamento='sem par completo'`. `PORTA:472–473` | `V5::ParDoDiaPeloJuiz.test_par_antes_do_fim_previsto_nao_e_a:50`, `A5::ClassificacaoDaPrevia.test_entrada_sozinha_fica_fora_de_a_e_de_b:185` |
| P7 | Feriado vira trabalho e o juiz confirma par completo | Os juízes de P6 e `app/ponto/calculador/regras.py:302::feriado_em_dobra_do_ciclo` | Classifica em `a` e formata batidas em hora local. `PORTA:465–471` | Nada pelo classificador | Colaborador, data, nome do feriado, `batidas`, `regra` (`feriado_em_dobra` ou `feriado_simples`) e `valor='sem apuracao ainda'`. `PORTA:465–471` | `V5::ParDoDiaPeloJuiz.test_par_depois_do_fim_previsto_e_a:62`, `PRÉVIA::PreviaNaSombra.test_previa_reparte_a_com_batida_e_b_sem_batida:99` |
| P8 | Feriado vira trabalho e não há turno do dia na janela carregada, ou a data é futura | `app/ponto/janelas.py:60,66::janela_atual/janela_anterior` e os turnos de P6. Para hoje, `app/ponto/turnos.py:2133::dias_encerrados` | Classifica em `b`. A saída de turno da véspera pertence à véspera. Para `d > hoje`, o classificador usa lista vazia de turnos. `PORTA:450–477` | Nada pelo classificador | `quando=passado/futuro`, ou `quando=hoje` com booleano `encerrado`. `PORTA:474–477` | `PRÉVIA::PreviaNaSombra.test_previa_reparte_a_com_batida_e_b_sem_batida:99`, `test_saida_de_turno_da_vespera_nao_e_batida_do_feriado:142`, `A5::ClassificacaoDaPrevia.test_sem_batida_hoje_usa_encerramento_do_turno:232` fixa `True`. Caso `False` dedicado **SEM PROVA** |

## Matriz — regeneração disparada pela marca

| Linha | Estado de entrada | Juiz consultado | Operação | Persistido | Retorno ao chamador | Prova existente |
|---|---|---|---|---|---|---|
| R1 | Chave mudou e a janela anterior/início do vínculo alcança `hoje` | `app/ponto/janelas.py:66::janela_anterior` e `CÉLULA:112::regenerar_celulas_vinculo` | Regenera de `max(início do vínculo, início da anterior)` até `hoje`, sem extensão. Depois chama R2. `VÍNCULO:909–924` | Efeitos R3–R6, no mesmo segmento da chave. `VÍNCULO:870,915–924` | Regenerador devolve número de células alteradas. Vínculo descarta esse número. `CÉLULA:395–401` [VÍNCULO:915–925] | `FPP::R3FechaRetrataEPropaga.test_R3_dia_trabalho_a_premissa_recusa_e_a_celula_propagada_fecha:232`, `FPP::R5CompetenciaExportadaIntacta.test_R5_dia_da_comp_exportada_fica_e_o_da_aberta_muda:368` |
| R2 | Futuras já geradas, inclusive fora da competência. `hoje` presente | `FUTURO:8,28::alcance_celulas_geradas/regenerar_futuras_do_cadastro`, `app/escala/alimentacao.py:334::vinculos_dos_dias` | Trava as geradas `data > hoje`, recorta por `desde` e passa cada dia com dono igual ao vínculo ou sem dono. Marca de atributo usa `substitui_dono=False`. Cada chamada recebe o mesmo `hoje` e `estender_horizonte=False`. `FUTURO:21–25,43–60` [VÍNCULO:920–924] | Só futuras selecionadas pelas guardas da célula. Sem futuras elegíveis, nada. `FUTURO:50–60` | Soma dos retornos, inclusive `0`. Vínculo descarta a soma. `FUTURO:56–60` [VÍNCULO:921–925] | `app/ponto/tests/test_cadastro_futuro_c5b.py::C5bTest.test_d2_flag_de_outro_vinculo_nao_toma_o_dia:122`, `test_d2_juiza_calada_segue_para_a_porta:133`, `test_d2_contra_exemplo_o_dono_segue_reescrevendo:141` |
| R3 | `hoje=None` nas portas de regeneração, ou vínculo sem vigência declarada | `FUTURO:28,41–42::regenerar_futuras_do_cadastro`, `CÉLULA:112,138–162::regenerar_celulas_vinculo`, `app/escala/alimentacao.py:189::tem_vigencia_declarada` | `None` é erro antes da escrita. Sem vigência declarada, recusa normal da regeneração direta. A seleção da marca já exclui esse vínculo. `PORTA:203–219` | `None` → nada. Sem vigência → tentativa de auditoria `regenerar_recusado_sem_vigencia`, sem gravar célula. `CÉLULA:138–162` | `ValueError` para `None`, `TypeError` por 

[truncado em 20000 de 38827 caracteres]

## 21/11 19:32 · sessao e161a5ec7905 · E4C-F5-CONSTROI · construtor · origem=Code

**INCOMPLETO para certificação.** A opção B está aplicada em `_slot_esperado`. Em `hora` sem resposta vigente e sem carimbo legado, o derivador recebe texto vazio. Se o marco fixo fornece instante, sai `SLOT`. Se não fornece, sai `SEM_RESPOSTA_VIGENTE`. `PLANTIO_SEM_TS` fica reservado à falha de derivação com texto elegível. O B3 segue aberto. `app/chamados/juizes.py:409–423` · `app/chamados/services/materializacao.py:691–710` · `aval_1930.txt:1` (anexo)

### V1 e provas locais

| Prova | Base `f3d23021f` | Cura |
|---|---|---|
| r1 · Batida vinculada às 05:24, a 66 minutos do slot original de 06:30 | `True` após a reabertura | `False` · `app/chamados/tests/test_e3_c8b_sem_banco.py:499–507` |
| r2 · Só destino encerrado | `True` | `False` · `app/chamados/tests/test_e3_c8b_sem_banco.py:509–515` |
| r3 · Hora desfeita com marco fixo e batida no marco | `SLOT` às 13:00, `True` | `SLOT` às 13:00, `True` · `app/chamados/tests/test_e3_c8b_sem_banco.py:517–527` |
| r4 · HX com lei calada e fora da janela viva | `True` nos dois | `False` nos dois, `SEM_RESPOSTA_VIGENTE` · `app/chamados/tests/test_e3_c8b_sem_banco.py:529–538` |
| r5 · Texto residual presente e ausente | `[True, True]` | `[False, False]` · `app/chamados/tests/test_e3_c8b_sem_banco.py:540–551` |

Rodei os REDs antes da edição de produção. Na cura, passaram 34 testes C8b, 18 da catraca de respondedores e 10 da A1. O `v3_cru` falhou na catraca com duas leituras onde se espera uma. Um teste adicional de cartório parou na importação de `django.utils` com a fonte da base e com a cura. `app/chamados/tests/test_e3_c8b_sem_banco.py:465–558` · `app/chamados/tests/test_e3_catraca_respondedores.py:544–557` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:188–293` · `app/chamados/tests/test_e3_pos_suite_sem_banco.py:174–181`

### Cinco leitores

| Leitor | Efeito da opção B |
|---|---|
| `carga_pergunta_viva` | Guarda o novo motivo. Os dois motivos sem instante recebem o mesmo tratamento na montagem dos limites. O marco fixo continua contribuindo com a janela de `SLOT`. `app/chamados/services/carga_pergunta_viva.py:119–145` |
| `classe_do_lote` | C21 já manda a candidata sem vigência para B antes da leitura do slot. A distinção entre os motivos é repassada em `porque` quando o lote alcança a leitura. `app/chamados/services/validacao.py:1214–1232` |
| Explicador | Distingue os motivos. Quando chega à explicação do slot, passa a mostrar “sem resposta vigente”. `app/chamados/services/explicador.py:52–61,122–150` |
| `vigia_carimbo` | Agrupa os dois motivos em `slot_nao_derivavel` após reprovação. Um caso antes aceito só por vínculo ou destino pode sair de `com_registro` para essa classe. `app/chamados/services/vigia_carimbo.py:97–123` |
| Juiz de fato → C6 | Distingue os motivos. `SEM_RESPOSTA_VIGENTE` vai direto à prova de retração com carimbo e trilha, fechando os atalhos de vínculo e destino. C6 inverte o veredito ao alcançar o degrau 6. `app/chamados/juizes.py:287–298,801–848,1535–1542` |

### Comandos de prova

Os três mutantes locais saíram com `rc=1` por asserção. m1 atingiu r1 e r2, m2 atingiu r3 e m3 atingiu a igualdade de r5 com `False != True`. O gerador também emite cada patch com `--patch`. `app/chamados/tests/sentinela_volta5_mutantes.py:18–45`

```sh
# A partir de app/
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest -v chamados.tests.test_e3_c8b_sem_banco.SentinelaVolta5
PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.sentinela_volta5_mutantes m1
PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.sentinela_volta5_mutantes m2
PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.sentinela_volta5_mutantes m3
```

Na pista, rode o primeiro comando sobre `f3d23021f` com o recorte de testes para o RED, e sobre a cura para o GREEN. Para os três comandos seguintes, aplique isoladamente o patch correspondente emitido com `--patch` e reverta-o após a prova. A pista usa Docker e banco. `bin/suite.sh:197–209,316–325` · `app/chamados/tests/sentinela_volta5_mutantes.py:26–38`

```sh
copia=/home/ronald/wt-codex-e4c-f5-constroi-141636
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k test_RED_sentinela_volta5
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k test_RED_sentinela_volta5_r1_ -k test_RED_sentinela_volta5_r2_
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k test_RED_sentinela_volta5_r3_
bin/suite.sh --dir "$copia" --only chamados --espera 7200 -- -k test_RED_sentinela_volta5_r5_
```

**Diff e autoconferência.** Zero commits pela proibição de `git commit`. O RED precedeu a cura na cópia, com `HEAD` em `f3d23021f`. Diff total de 5 arquivos, `+161/−16`, incluindo o gerador novo. `except` adicionados: **0 em produção e 0 em teste**. Fallbacks novos: **0**. `app/chamados/juizes.py:409–423` · `app/chamados/tests/test_e3_c8b_sem_banco.py:465–558` · `app/chamados/tests/test_hx_fallback_sem_lei.py:61–80` · `app/chamados/tests/test_marco_por_motivo.py:271–282` · `app/chamados/tests/sentinela_volta5_mutantes.py:1–50`

**Pendente** · ORM e mordidas anteriores `s1`, `s2a`, `s2b`, `s3i`, `s3ii`, vínculo, destino e trilha, suíte inteira pelo Code, opus final e incidência em produção [nao verificado]. [nao verificado]

## 21/11 19:32 · sessao e7d6683e92e8 · - · - · origem=Ronald (tmux)

Na cópia, `--apply` sem motivo ou com espaços está curado. `aplicar_marca` levanta `MarcaInvalida` antes da primeira transação. `_marcar` entrega `CommandError` encadeado antes do plano. O teste cobre os dois motivos e verifica a marca nula. `app/colaboradores/services/feriado_do_posto.py:305-308`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:118-128`, `app/colaboradores/tests/test_feriado_por_posto.py:645::test_apply_sem_motivo_recusa_sem_escrever`.

| Entrada | Juiz → operação | Persistência | Saída e prova |
|---|---|---|---|
| Sem `--posto` | `sugestoes` e `presenca_em_feriado` recebem o dia capturado no `handle`. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:46-68` | Nada. Mesmo arquivo, `:49-68`. | Sugestões e retorno normal. A passagem do dia antes da virada está em `app/colaboradores/tests/test_feriado_a5_volta3.py:245::test_sugestoes_e_propagacoes_usam_o_dia_da_borda_ao_virar_meia_noite`. |
| Ambas ou nenhuma ação, com posto | `Command.handle` recusa pela igualdade dos argumentos. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:70-72` | Nada. Mesmo trecho. | Mensagem e retorno normal. **SEM PROVA**. |
| `--opera --retomar-retratacao` | `Command.handle` exige `--fecha`. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:73-75` | Nada. Mesmo trecho. | `CommandError`. **SEM PROVA**. |
| Posto inexistente | `_marcar` faz busca por PK. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:111-115` | Nada. Mesmo trecho. | Mensagem e retorno normal. **SEM PROVA**. |
| Dry-run, com ou sem motivo, em sombra | `_marcar` chama `previa_da_marca` com `hoje` e `agora`, sem repassar motivo. A prévia usa motivo próprio, roda a porta e força rollback. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:117-127,144-147`, `app/colaboradores/services/feriado_do_posto.py:397-406` | Nenhuma linha do ato persiste. Mesmos trechos. | Plano, grupos e `DRY-RUN`, retorno `None`. Prova em `app/colaboradores/tests/test_feriado_previa_da_marca.py:217::test_dry_run_do_comando_le_a_previa_e_nao_escreve` e `:253::test_PROVA_b_banco_identico_no_ramo_da_retratacao`. |
| Dry-run fora da sombra | `previa_da_marca` recusa antes da porta. `app/colaboradores/services/feriado_do_posto.py:379-383` | Nada. Mesmo trecho. | `PreviaForaDaSombra` vira `CommandError` pela CLI. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:124-125`, `app/colaboradores/tests/test_feriado_previa_da_marca.py:277::test_PROVA_a_comando_sai_com_erro_sem_chamar_a_porta_e_sem_escrever`. |
| `--fecha --apply` com motivo, sem município | `plano_da_marca` valida após reler o posto travado, antes das escritas. `app/colaboradores/services/feriado_do_posto.py:265-275,305-323` | Nada. Mesmo trecho. | `MarcaInvalida` vira `CommandError`. Prova `app/colaboradores/tests/test_feriado_volta4.py:17::test_fecha_em_posto_sem_municipio_levanta_command_error`. O teste fixa a exceção de `call_command`, e o cabeçalho documenta rc 1 de `manage.py`. Mesmo arquivo, `:1-4,17-25`. |
| Marca válida nova | `plano_da_marca` sob trava, seguido de `marcar_opera_em_feriado`. `app/colaboradores/services/feriado_do_posto.py:307-323` | `Posto.opera_em_feriado` e antes/depois em `LogAuditoria`, com trilha best-effort sob savepoint próprio. `app/colaboradores/services/estrutura.py:90-95`, `app/core/observ.py:94-139`. | Serviço entrega `marca_muda`, `retratados`, `propagados`, `plano`. CLI imprime contagens e retorna `None`. `app/colaboradores/services/feriado_do_posto.py:354`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:75,148-150`, `app/colaboradores/tests/test_feriado_por_posto.py:151::test_R1_porta_da_marca_um_evento_no_posto_uma_linha_por_vinculo_2a_rodada_zero` e `:658::test_apply_chama_a_porta`. |
| FECHA, chamado vivo no posto do fato e em feriado | `_chamados_do_posto` consulta `qs_vivos`, `dias_do_fato`, `vinculos_dos_dias`. `eh_feriado_do_posto` delega a `feriados_de`. Cada chamado revalida marca, pertencimento e alcance sob trava e chama `transicionar_com_pauta(... ato=ch.reconciliar)`. `app/colaboradores/services/feriado_do_posto.py:47-57,237-254,277-285,325-341`, `app/core/feriados.py:70`. | Se o gate autoriza, `ChamadoColaborador.status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`, urgência quando couber, `LogAuditoria.estado_chamado` e `FollowupChamado` de tipo lógico `feriado_do_posto`. `app/chamados/models.py:234-252,286-302,654-668`, `app/chamados/services/nota.py:75-109`. | `reconciliar=True`, `retratados += 1`. Prova `app/colaboradores/tests/test_feriado_por_posto.py:217::test_R3_dia_ja_folga_pela_celula_retrata_pela_nota_da_marca`. |
| Chamado candidato, gate recusa | `_decisao_do_fechamento` consulta `decidir`, premissa, conversa do pai e pergunta viva. `app/chamados/models.py:676-709` | Status e nota da marca intactos no segmento. Conforme gate, `LogAuditoria.fechamento_recusado` deduplicado, e na conversa do pai o `FollowupChamado` de tipo `retido_pela_conversa`. `app/chamados/models.py:409-456,735-752,821-830`, `app/colaboradores/services/feriado_do_posto.py:335-341`. | `False`, sem incremento, laço segue. A propagação pode fechar a falta em etapa posterior. Prova `app/colaboradores/tests/test_feriado_por_posto.py:232::test_R3_dia_trabalho_a_premissa_recusa_e_a_celula_propagada_fecha`. |
| Marca superada entre segmentos | `aplicar_marca` relê travado e quebra o laço correspondente. `app/colaboradores/services/feriado_do_posto.py:325-353` | Ficam os segmentos já concluídos. Segmento superado sem escrita. Mesmo trecho. | Contagens parciais e retorno normal. Provas `app/colaboradores/tests/test_feriado_a5_volta3.py:50::test_opera_superada_por_fecha_nao_grava_chave_velha` e `:73::test_fecha_superada_por_opera_nao_retrata`. |
| FECHA já vigente, sem retomada | `plano_da_marca` deixa de montar alvos de retratação e ainda seleciona divergências. `app/colaboradores/services/feriado_do_posto.py:272-285` | Sem novo evento do posto e sem retratação direta. Eventual propagação de divergência. Mesmo arquivo, `:318-323,343-354`. | Resultado normal. Prova `app/colaboradores/tests/test_feriado_a5_volta3.py:105::test_fecha_ja_fecha_e_reaplicar_preservam_chamado_vivo`. |
| Vínculo divergente elegível | `vinculos_que_seguem_a_marca` consulta `q_vinculo_cobre_a_data`, `segue_a_marca` e cargo. Sob trava, revalida e chama `marcar_trabalha_em_feriado`. `app/colaboradores/services/feriado_do_posto.py:175-185,203-219,343-354` | `EscalaColaborador.trabalha_em_feriado`, `LogAuditoria.editar` best-effort e regeneração por vínculo atômico. O signal invalida `FechamentoMensal.previsto_em` e carimba `atualizado_em`. `app/colaboradores/services/vinculo.py:870-925`, `app/escala/signals.py:137-151`, `app/ponto/services/fechamento.py:1607-1613`. | Retorna `ec`, soma propagado. Provas `app/colaboradores/tests/test_feriado_por_posto.py:133::test_R1_porta_do_vinculo_grava_UMA_linha_da_troca_com_autor_e_motivo` e `:185::test_R2_admin_12x36_outro_posto_e_encerrado_ficam_e_o_irmao_muda`. |
| Exceção em segmento da retratação ou propagação | `aplicar_marca` usa `atomic` por chamado e por vínculo. Um `ValueError` puro é possível no reaponte recusado e não é convertido pela CLI. `app/colaboradores/services/feriado_do_posto.py:325-353`, `app/chamados/models.py:449-451`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:118-127`. | Desfaz o segmento em falha. Em execução sem transação envolvente, ficam marca e segmentos anteriores. O `atomic` externo da prévia desfaz todos. `app/colaboradores/services/feriado_do_posto.py:308-353,397-406`. | Exceção sobe. O caso específico do `ValueError` puro na porta tem **SEM PROVA**. Falha injetada no segundo vínculo e na nota têm provas em `app/colaboradores/tests/test_feriado_por_posto.py:503::test_falha_na_2a_propagacao_fica_denunciada_e_a_porta_retoma` e `:533::test_falha_na_retratacao_nao_deixa_nota_sem_chamado_fechado`. |

**Diferença para main.** `ab351aa9b:app/colaboradores/management/commands/marcar_feriado_do_posto.py:23-31,86-94,128-149` não oferece `--motivo`, imprime e retorna normal para FECHA sem município, lê o dia em `_marcar` e faz escrita/retratação diretamente, sem propagação pela CLI. A API Python nova da marca está em `app/colaboradores/services/feriado_do_posto.py:288-406`. A releitura da célula já existe no main em `ab351aa9b:app/ponto/portas/celula.py:464`. O caminho novo é a CLI/preview acima de uma data capturada na borda. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:46-47,117-123`, `app/colaboradores/services/feriado_do_posto.py:349-352`, `app/ponto/portas/celula.py:392-405,464`.

**Extras do anexo.** `presenca_em_feriado()` ainda lê `localdate()` se faltar `hoje` e algum limite. `marcar_trabalha_em_feriado` ainda lê `localdate` depois de salvar campo e trilha. A sincronização consulta a marca na listagem e grava a seleção antiga sem `hoje` e sem a trava do posto. Os respectivos REDs continuam prevendo falha pela leitura da fonte. Os testes correntes de sync cobrem marca declarada antes da listagem e regra de cargo, não a corrida. `app/colaboradores/services/feriado_do_posto.py:74-89`, `app/colaboradores/services/vinculo.py:896-908`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:43-72`, `app/colaboradores/tests/test_feriado_admin_sync.py:80-103`.

**Proibição e mutantes.** A cura na origem requer `app/ponto/portas/celula.py:392,404-405,464`, com passagem nas outras duas chamadas `:665,858`. O wrapper proíbe esse arquivo ao construtor Codex e descarta a volta com rc 3. A integração fica com opus/sessão no rascunho. Os mutantes já presentes de motivo, par e `agora` estão em `app/colaboradores/tests/test_feriado_a5_volta3.py:342-356`; o teste de meia-noite `:245-267` fixa só a chegada do `hoje` ao escritor de vínculo. Para comprovar o corte da célula, o novo RED deve dirigir a CLI antes da meia-noite, avançar o relógio após a captura e observar uma célula de amanhã efetivamente regenerada e fora do julgamento síncrono enquanto a de hoje está incluída. Proposta de teste e mutantes ainda [nao verificado]. `bin/codex_pergunta.sh:346`, `app/ponto/services/cadastro_futuro.py:43-59`, `app/ponto/portas/celula.py:460-474`.

## 21/11 19:31 · sessao e5f10b72d3d3 · - · - · origem=Ronald (tmux)

O achado (i) está aberto. `Command.handle` captura `agora` e deriva `hoje` uma vez. O dia passa por `_marcar`, `aplicar_marca` e `marcar_trabalha_em_feriado`, tanto para a regeneração até hoje quanto para as futuras geradas já existentes. Os dois caminhos chegam a `regenerar_celulas_vinculo` com o dia correto. A perda ocorre na chamada de `_o_chao_do_dia_mudou`, cuja assinatura omite `hoje`. Ela lê `timezone.localdate()` e usa esse segundo dia para decidir quais células tocadas chegam ao cartório. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:46-47,117-123`, `app/colaboradores/services/feriado_do_posto.py:315,349-352`, `app/colaboradores/services/vinculo.py:909-924`, `app/ponto/services/cadastro_futuro.py:41-60`, `app/ponto/portas/celula.py:112-140,193,392-405,460-474`.

A releitura tem o mesmo código no snapshot de `main`. O alcance pela porta da marca é novo. O comando antigo gravava a marca e retratava chamados diretamente e já tinha leitura própria de dia para esse recorte. `main@ab351aa9b:app/ponto/portas/celula.py:392-405,464-469`, `main@ab351aa9b:app/colaboradores/management/commands/marcar_feriado_do_posto.py:93-105,128-149`.

A cura na origem exige `app/ponto/portas/celula.py::_o_chao_do_dia_mudou` com `hoje` explícito e os três chamadores `::regenerar_celulas_vinculo`, `::editar_dia` e `::reverter_regeneracao_dia`. Regeneração e reversão já recebem o valor. A edição já captura `agora` para o carimbo e pode derivar a data desse mesmo instante. Se a sessão exigir um único dia para as duas edições do ato de troca, `::trocar_plantao` passa o mesmo dia aos dois lados. A mudança vai ao Opus/sessão porque `celula.py` está proibida ao construtor Codex. Juiz novo = 0. `app/ponto/portas/celula.py:112-140,392-405,464-472,572,636-647,665-666,670-713,779,858-859`. A injeção corrige o corte do despacho. O cartório, depois da fronteira pedida, ainda consulta dia e instante próprios. `app/ponto/services/cartorio.py:2059-2089`.

RED a acrescentar pela CLI ou pela porta pública, com feriados de 5×2 no dia injetado D e em D+1, ambos materializados e efetivamente alterados, enquanto o relógio ambiente já está em D+1. A asserção pede despacho de D e exclui D+1. O mutante de restauração de `localdate()`, os mutantes que omitem o repasse e a troca de `>` por `>=` devem falhar. O teste de meia-noite existente espia só a chegada em `marcar_trabalha_em_feriado`. O teste estrutural existente sobre o cadastro futuro não inclui o helper que relê. Prova específica para (i) na cópia = SEM PROVA. `app/colaboradores/tests/test_feriado_a5_volta3.py:245-267`, `app/ponto/tests/test_cadastro_futuro_c5d.py:24-35,131-136,152-191`, `app/ponto/portas/celula.py:464-474`.

A regeneração passada/hoje pode criar `CelulaDia` gerada ou reescrever trabalho, geradora e DNA de célula gerada e carimbar `dna_anterior`, `regenerada_em` e `regeneracoes`. O futuro do evento de atributo seleciona só geradas já existentes e deixa de fora as cujo dono é outro vínculo. Lavrada pula, editada fica e desativado sem fim devolve `0` e registra a recusa. O contador da marca conta vínculos com chamada concluída, mesmo quando a regeneração devolve zero para todas as células. `app/ponto/portas/celula.py:153-162,218-229,284-318,392-401`, `app/ponto/services/cadastro_futuro.py:44-60`, `app/colaboradores/services/vinculo.py:912-925`, `app/colaboradores/services/feriado_do_posto.py:344-354`. Provas `app/colaboradores/tests/test_feriado_por_posto.py::test_R5_dia_da_comp_exportada_fica_e_o_da_aberta_muda` e `::test_R6_editada_fica_e_a_gerada_do_irmao_muda`, `app/ponto/tests/test_cadastro_futuro_c5b.py::test_d2_flag_de_outro_vinculo_nao_toma_o_dia`, `app/escala/tests/test_e4_regenerar_recusa_sem_vigencia.py::test_MORDE_desativado_sem_fim_e_recusado_com_motivo`.

A marca, cada retratação e cada vínculo usam blocos próprios. Exceção na nota desfaz a retratação corrente. Exceção na segunda propagação deixa os segmentos anteriores confirmados. A trilha comum é best-effort, com savepoint próprio. Faltar a trilha obrigatória da limpeza/conservação do julgamento futuro levanta `RuntimeError` e desfaz a propagação corrente. A prévia põe os segmentos sob um atomic externo que ela desfaz. `app/colaboradores/services/feriado_do_posto.py:305-354,397-406`, `app/core/observ.py:94-105,124-139`, `app/core/auditoria.py:8-30`, `app/ponto/portas/celula.py:1146-1175`. Provas `app/colaboradores/tests/test_feriado_por_posto.py::test_falha_na_2a_propagacao_fica_denunciada_e_a_porta_retoma`, `::test_falha_na_retratacao_nao_deixa_nota_sem_chamado_fechado` e `app/ponto/tests/test_cadastro_futuro_c5d_banco.py::test_MORDE_trilha_que_nao_grava_DESFAZ_a_limpeza`, `::test_conservado_sem_trilha_recusa`.

## 21/11 19:31 · sessao 822c2d88a55e · - · - · origem=Ronald (tmux)

A prévia tem os achados (iii) e (iv) curados por inspeção. Os testes foram lidos, sem execução. [feriado_do_posto.py:379](./app/colaboradores/services/feriado_do_posto.py#L379), [feriado_do_posto.py:450](./app/colaboradores/services/feriado_do_posto.py#L450), [test_feriado_volta5.py:18](./app/colaboradores/tests/test_feriado_volta5.py#L18)

| Estado de entrada | Juiz ou guarda → operação | Persistência → retorno | Prova |
|---|---|---|---|
| Fora da sombra | `previa_da_marca` exige `HASNER_SOMBRA is True` antes de qualquer plano, inclusive com `agora=None`. `feriado_do_posto.py:370-383` | Nada → `PreviaForaDaSombra`; CLI converte em `CommandError`. `marcar_feriado_do_posto.py:123-127` | `test_feriado_previa_da_marca.py::PreviaForaDaSombraRecusa.test_PROVA_a_comando_sai_com_erro_sem_chamar_a_porta_e_sem_escrever` :277 |
| Sombra sem `agora` | `previa_da_marca` recusa antes de abrir transação. `feriado_do_posto.py:382-386` | Nada → `ValueError('previa_da_marca: agora e obrigatorio')`. | `test_feriado_volta5.py::RelogioDesceDaBorda.test_previa_da_marca_sem_agora_recusa` :18 |
| Sombra válida | `aplicar_marca` consulta `plano_da_marca` sob trava, fotografa as células dos alvos e propagados, executa realmente e classifica a foto seguinte dentro do `atomic`. `feriado_do_posto.py:307-317,390-404` | Rollback integral, inclusive callbacks `on_commit`. Devolve `marca_muda`, `retratados`, `propagados`, `plano`, `a`, `b`, `outras`. `feriado_do_posto.py:354,397-406` | `test_feriado_previa_da_marca.py::PreviaNaSombra.test_PROVA_b_banco_identico_por_chave_e_valor` :158, `test_PROVA_a_nenhum_on_commit_sobra_da_previa` :190; FECHA `PreviaDoFechaComChamadoVivo.test_PROVA_b_banco_identico_no_ramo_da_retratacao` :253 |
| Execução simulada dá erro | `previa_da_marca` captura apenas `_DesfazAPrevia`, após sair do `atomic`. `feriado_do_posto.py:397-406` | Rollback integral → exceção original. `MarcaInvalida` é `ValueError` e CLI a converte em `CommandError`. `feriado_do_posto.py:257,265-275` e `marcar_feriado_do_posto.py:126-127` | SEM PROVA específica de falha tardia dentro da prévia. |
| Campos iguais; ou mudança sem virar trabalho em feriado deste posto | `_classificar_previa` encontra diferenças nos oito campos da foto, usa `eh_feriado_do_posto` → `feriados_de` e `vinculos_dos_dias` → `vinculo_do_dia`. `feriado_do_posto.py:47-57,409-449`, `core/feriados.py:70`, `escala/alimentacao.py:228,334-358` | Nada próprio → sem linha se igual, ou `outras` com id/data, antes/depois, campos mudados e feriado booleano. `feriado_do_posto.py:431-449` | `test_feriado_previa_da_marca.py::PreviaNaSombra.test_celula_que_muda_de_geradora_sem_mudar_trabalha_aparece_em_outras` :112, `test_feriado_a5_volta3.py::ClassificacaoDaPrevia.test_ata_mudada_sem_troca_de_trabalho_aparece` :166. SEM PROVA isolada da igualdade. |
| Virou trabalho e tem par completo | `turnos_do_colab` → `dias_encerrados(agora=agora,turnos=pareados)` → `preparar_par_completo_da_janela` → `par_completo`. `feriado_do_posto.py:450-464`, `turnos.py:1850,2113-2130,2133,2195-2219` | Nada próprio → `a`, nome do feriado, batidas em hora local, regra por ciclo e literal `sem apuracao ainda`. `feriado_do_posto.py:465-471`, `ponto/calculador/regras.py:302` | `test_feriado_volta5.py::ParDoDiaPeloJuiz.test_par_depois_do_fim_previsto_e_a` :62 |
| Par formado antes do fim ou turno órfão | Mesmo juiz; turno do dia existe, par não passou. `feriado_do_posto.py:450-473` | Nada próprio → `outras` com `pareamento='sem par completo'`. | `test_feriado_volta5.py::ParDoDiaPeloJuiz.test_par_antes_do_fim_previsto_nao_e_a` :50, `test_feriado_a5_volta3.py::ClassificacaoDaPrevia.test_entrada_sozinha_fica_fora_de_a_e_de_b` :185 | [nao verificado]
| Virou trabalho, sem turno do dia | Datas são comparadas com `hoje`; para hoje, `dias_encerrados` decide `encerrado`. `feriado_do_posto.py:459-462,474-477` | Nada próprio → `b` com `passado`, `futuro` ou `hoje` e booleano `encerrado`. | `test_feriado_previa_da_marca.py::PreviaNaSombra.test_previa_reparte_a_com_batida_e_b_sem_batida` :99 e `test_feriado_a5_volta3.py::ClassificacaoDaPrevia.test_sem_batida_hoje_usa_encerramento_do_turno` :232. SEM PROVA dedicada para `encerrado=False`. |

O CLI lê `timezone.now()` uma vez, deriva `timezone.localdate(agora)` e passa ambos à prévia. O classificador usa `hoje` para a janela anterior + atual e `agora` no encerramento. `turnos_do_colab` usa a janela e as batidas vivas, e `par_completo(janela=...)` lê o veredito já preparado. O fallback `timezone.now()` de `dias_encerrados` fica fora desse caminho. O leitor dos baldes é `_imprimir_previa`. `marcar_feriado_do_posto.py:46-47,75,78-100,117-127,145`, `feriado_do_posto.py:450-477`, `ponto/turnos.py:1867-1871,2148-2163,2207-2219`.

A API localizada é a função Python. Endpoint HTTP, JSON, status ou chamador view [não verificado]. A marca está na lista de comandos excluídos do cron como ato humano. O texto menciona a pauta das sugestões, mas a agregação efetiva nem lê `feriado_do_posto`; há um produtor e um rótulo declarados. `config/crons.py:1350-1355`, `escala/services/fase_12x36.py:254-280`, `feriado_do_posto.py:147-159`, `pautas/semanal.py:43`. O `main` tem dry-run por listagem e marca escrita pelo comando, sem `previa_da_marca` no serviço que acaba em `linhas_para_a_pauta`. `ab351aa9b/app/colaboradores/management/commands/marcar_feriado_do_posto.py:93-149`, `ab351aa9b/app/colaboradores/services/feriado_do_posto.py:146-158`.

Os REDs anexos de município, falta de `agora` e par antes/depois do fechamento têm equivalentes na árvore e passam pela lógica atual, execução [não verificado]. Os de ausência de `hoje` continuam vermelhos por inspeção no fallback de `presenca_em_feriado` e no fallback após a escrita de `marcar_trabalha_em_feriado`. Os dois de sincronização continuam vermelhos por inspeção: aplica lista velha que foi decidida antes do `stdout` e não transmite a kwarg `hoje`, de modo que a asserção do spy dá `KeyError`. `test_feriado_volta4.py:17`, `test_feriado_volta5.py:18,50,62`, `feriado_do_posto.py:74-90`, `vinculo.py:896-908`, `sincronizar_feriado_admin.py:52-72`.

Para o rascunho, preserve os mutantes existentes `volta5_par` e `volta5_agora`. Para os REDs anexos auxiliares, restaurar o fallback é o mutante de relógio e aplicar decisão pré-listagem é o mutante de sincronização. O teste por `stdout` mata a decisão antecipada, mas uma releitura tardia sem `select_for_update` ainda o satisfaz, logo a prova do lock exige outro instrumento caso a sincronização seja aprovada. Testar a ausência de `hoje` no mesmo processo requer restaurar a função real nos dois testes existentes que hoje a sobrescrevem e apagam. O juiz de par já está registrado e tem outro leitor no cron de par-relâmpago. `test_feriado_a5_volta3.py:351-356`, `sincronizar_feriado_admin.py:52-72`, `test_feriado_do_posto.py:113-117,126-132`, `core/juizes.py:243-248`, `ponto/management/commands/detectar_par_relampago.py:289-300,349-353`.

## 21/11 19:16 · sessao e3fb8629437a · FERIADO-POR-POSTO · construtor · origem=Code

**FERIADO-POR-POSTO, volta 5.** A cura está na cópia com `HEAD d72b72a76`. A regra explícita de `git commit` impediu os dois commits pedidos. O RED foi escrito e exercitado em memória antes da cura. O RED isolado está nos dois arquivos de teste indicados abaixo.

- **N1.** `--apply` sem motivo ou com só espaços usa a validação da porta. O comando propaga `CommandError` com `MarcaInvalida` como causa. O teste exige o erro e relê a marca vazia nos dois casos. `colaboradores/management/commands/marcar_feriado_do_posto.py:118–127` · `colaboradores/services/feriado_do_posto.py:305–309` · `colaboradores/tests/test_feriado_por_posto.py:645–656`.
- **N2.** A prévia consulta o juiz registrado `par_completo`, reutilizando os turnos e os encerramentos da mesma janela. O teste coloca o par com saída às 16h55 em `outras` às 16h56 e em `(a)` às 17h01. `core/juizes.py:243–248` · `ponto/turnos.py:2185–2219` · `colaboradores/services/feriado_do_posto.py:450–475` · `colaboradores/tests/test_feriado_volta5.py:31–67`.
- **N3.** Na sombra, a prévia recusa `agora` ausente ou `None` antes de chamar a porta. `hoje` é argumento obrigatório, e o plano recusa `None`. O comando já entrega o instante e a data local derivados da mesma leitura. `colaboradores/services/feriado_do_posto.py:261–268,370–403` · `colaboradores/management/commands/marcar_feriado_do_posto.py:41–47,117–123` · `colaboradores/tests/test_feriado_volta5.py:15–27`.

### Origem e uso dos valores

| Valor | Fonte e escritor | Instante | Leitores e significado |
|---|---|---|---|
| `motivo` | A supervisão fornece `--motivo`. `evento` grava `LogAuditoria.descricao`. `colaboradores/management/commands/marcar_feriado_do_posto.py:27–37` · `core/observ.py:133–137`. | A porta valida antes da trava. As descrições são gravadas quando muda a marca ou a chave. `colaboradores/services/feriado_do_posto.py:305–323,344–352` · `colaboradores/services/vinculo.py:896–904`. | A porta, a trilha do vínculo e a tela de auditoria leem a justificativa humana da mudança. `colaboradores/services/feriado_do_posto.py:305–323,349–352` · `templates/core/log_auditoria.html:73`. |
| `agora` | `handle` lê `timezone.now()` e passa o valor em memória. `colaboradores/management/commands/marcar_feriado_do_posto.py:41–47,75,123`. | Na entrada do comando. `colaboradores/management/commands/marcar_feriado_do_posto.py:41–47`. | A prévia entrega ao juiz o instante de referência para o encerramento. `colaboradores/services/feriado_do_posto.py:397–404,459–461` · `ponto/turnos.py:2113–2130`. |
| `hoje` | `handle` deriva `timezone.localdate(agora)` e passa a data em memória. `colaboradores/management/commands/marcar_feriado_do_posto.py:46–47,75,117–123`. | Na mesma passagem pela borda. `colaboradores/management/commands/marcar_feriado_do_posto.py:41–47`. | O plano seleciona vínculos vigentes. A prévia delimita as competências e distingue passado, hoje e futuro pela data local do ato. `colaboradores/services/feriado_do_posto.py:261–283,450–477`. |
| Par e encerramento | `turnos_do_colab` monta os turnos com batidas apuráveis e células da janela. `dias_encerrados` produz o mapa em memória. `ponto/turnos.py:1850–1893,1920,2133–2182`. | Dentro do savepoint, depois da aplicação simulada. `colaboradores/services/feriado_do_posto.py:397–404,450–461`. | O preparador do juiz lê as batidas e o encerramento. A prévia também lê o encerramento para `(b)` de hoje. Pareamento identifica o turno das batidas. Encerramento considera turno aberto vivo e fim previsto. `ponto/turnos.py:2113–2130,2195–2204` · `colaboradores/services/feriado_do_posto.py:462–475`. |
| Veredito do par | `par_completo` devolve em memória o booleano da consulta preparada. `ponto/turnos.py:2195–2219`. | Na classificação, antes do rollback. `colaboradores/services/feriado_do_posto.py:397–406,462–473`. | A prévia e o detector de par-relâmpago leem a resposta para a mesma pergunta, se as duas batidas pertencem a um mesmo turno encerrado. `core/juizes.py:243–248` · `colaboradores/services/feriado_do_posto.py:462–473` · `ponto/management/commands/detectar_par_relampago.py:285–300`. |

### Provas para a pista

Com dependências simuladas em memória, os três REDs falharam na base, o controle das 17h01 passou, os quatro casos passaram na cura e cada mutante fez seu RED falhar na asserção esperada. A pista com Django e PostgreSQL fica [nao verificado]. `colaboradores/tests/test_feriado_por_posto.py:645–656` · `colaboradores/tests/test_feriado_volta5.py:15–67` · `colaboradores/tests/test_feriado_a5_volta3.py:342–357,367–401`.

Da raiz de `app/`, com `BASE_RED` apontando para a raiz de uma cópia de ensaio da sessão em `d72b72a76`, copie apenas os dois arquivos do RED. O primeiro `prova` deve falhar nos três itens. O segundo, executado na cura, deve passar. As configurações de banco de ensaio são as da pista. `config/settings/ci.py:6–19,40–41`.

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python -B manage.py test \
    colaboradores.tests.test_feriado_por_posto.CascaDoComando.test_apply_sem_motivo_recusa_sem_escrever \
    colaboradores.tests.test_feriado_volta5 \
    --settings=config.settings.ci --noinput
}

cp -- colaboradores/tests/test_feriado_por_posto.py \
      colaboradores/tests/test_feriado_volta5.py \
      "$BASE_RED/app/colaboradores/tests/"
(cd "$BASE_RED/app" && prova)
prova

PYTHONDONTWRITEBYTECODE=1 python -B manage.py test \
  $(printf '%s\n' colaboradores/tests/test_feriado_*.py | sed 's/\.py$//; s#/#.#g') \
  ponto.tests.test_cadastro_futuro_c5e \
  ponto.tests.test_par_relampago_pelo_turno \
  --settings=config.settings.ci --noinput
```

Os três comandos abaixo restauram, em memória, o retorno normal do comando, a classificação sem consultar o juiz e a leitura implícita do relógio, respectivamente. `colaboradores/tests/test_feriado_a5_volta3.py:342–357,391–401`.

```bash
mutante() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci \
    python -B -c '
import sys
import django
django.setup()
from colaboradores.tests.test_feriado_a5_volta3 import rodar_mutante
raise SystemExit(rodar_mutante(sys.argv[1]))
' "$1"
}
mutante volta5_motivo
mutante volta5_par
mutante volta5_agora
```

### Diff e INCOMPLETO

O diff tem **7 arquivos, +139/−40**, incluindo `colaboradores/tests/test_feriado_volta5.py:1–67`, novo na cópia. Os seis arquivos rastreados somam +72/−40. Para reproduzir o stat completo:

```bash
git diff --stat HEAD
git diff --no-index --stat /dev/null colaboradores/tests/test_feriado_volta5.py
```

- O selo contou **44 → 44**. Saiu `timezone.now()` de `previa_da_marca`, em `d72b72a76:app/colaboradores/services/feriado_do_posto.py:385`, fora de `UNIVERSO_RELOGIO`. `ponto/tests/test_cadastro_futuro_c5e.py:327–334,376,576–599`.
- Pelo escopo do aval, ficaram pendentes a releitura na célula, os testes de omissão de `hoje` em presença e na porta do vínculo, e os dois testes de `SincronizacaoDecideSobATrava`. `MarcaInvalidaSaiComErro` pertence à volta 4. `ponto/portas/celula.py:460–474` · `colaboradores/services/feriado_do_posto.py:74–89` · `colaboradores/services/vinculo.py:871,905–908` · `colaboradores/management/commands/sincronizar_feriado_admin.py:52–72` · `colaboradores/tests/test_feriado_volta4.py:15–25`.
- Também ficaram fora do recorte os retornos para flags iguais e posto inexistente, a auditoria best-effort e o alcance dela na trilha do vínculo. `colaboradores/management/commands/marcar_feriado_do_posto.py:70–75,111–115` · `core/observ.py:85–98,124–139` · `core/auditoria.py:17–30` · `colaboradores/services/vinculo.py:896–904`.
- Suíte da pista, `rc` em subprocesso real, certificação Code, Opus final e produção [nao verificado]. [nao verificado]

**Autoconferência BRIEF-SEM-EXCEPT.** `except` adicionados **0**, fallbacks adicionados **0**. A conversão de `MarcaInvalida` continua no handler existente. `colaboradores/management/commands/marcar_feriado_do_posto.py:118–127` · `colaboradores/services/feriado_do_posto.py:379–406` · `ponto/turnos.py:2185–2219`.

_7760 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
