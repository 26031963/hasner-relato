# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 19:52** · publicado em **31/10/2026 19:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 19:52 · sessao 4bd4d009df1b · PADRAO-DE-CIMA · verificador · origem=Code

**Não aprovaria o pre-pouso:** há um caminho de LIVRE sem prova sobre o histórico e um mutante aceito cuja morte vem da exceção artificial que ele próprio injeta. (`bin/disjuntor.py:1498`; `bin/disjuntor.py:1514`; `bin/tests/test_disjuntor.sh:1762`; `bin/tests/_mordida.sh:159`)

Execução integral dos selos, ocorrência em produção e confirmação Git do hash: **[nao verificado]**.

1. **LIVRE, julgamento e fontes ilegíveis**

   **Há uma lacuna no fallback.** Considere uma obra anteriormente disparada, com eventos ainda dentro das 24 horas, cujas linhas saíram do RELATO. O estado conserva esses eventos independentemente da permanência no texto. Se o estado estiver corrompido e o escopo ausente ou ilegível, `estado_pdc` aceita apenas a ausência da obra no RELATO atual e retorna LIVRE. Essa ausência não prova que inexista evento histórico na janela. (`bin/disjuntor.py:1386`; `bin/disjuntor.py:1288`; `bin/disjuntor.py:272`; `bin/disjuntor.py:1484`; `bin/disjuntor.py:1506`; `bin/disjuntor.py:1514`)

   Nesse caminho houve julgamento pelo fallback nesta chamada, mas **não houve decisão completa nem prova sobre o histórico**. O teste sem escopo mantém a obra com evento no RELATO; não exercita a combinação “evento histórico retirado do texto + estado corrupto + escopo ilegível”. (`bin/disjuntor.py:1494`; `bin/disjuntor.py:1495`; `bin/tests/test_padrao_de_cima.sh:253`; `bin/tests/test_padrao_de_cima.sh:259`; `bin/tests/test_padrao_de_cima.sh:260`)

   Para fontes efetivamente abertas, a proteção está correta: `OSError`, `SyntaxError` ou `UnicodeDecodeError` chegam ao tratamento que acrescenta a obra a `duvida` e pula a criação do evento novo. RELATO ausente/ilegível também não acrescenta eventos e produz NAO_SEI fora das disparadas. (`bin/disjuntor.py:1236`; `bin/disjuntor.py:1262`; `bin/disjuntor.py:1263`; `bin/disjuntor.py:1380`; `bin/disjuntor.py:1384`; `bin/disjuntor.py:712`; `bin/disjuntor.py:1373`; `bin/disjuntor.py:1454`; `bin/disjuntor.py:1518`)

   **Ressalva anterior à abertura:** `isfile=False` é tratado como ausência. Se um caminho existente, porém inacessível, produzir esse resultado, ele será pulado; o resolvedor poderá devolver `[]`, gravar `res=2` e permitir LIVRE. Reprodução dessa condição de permissões: **[nao verificado]**. (`bin/disjuntor.py:1233`; `bin/disjuntor.py:1259`; `bin/disjuntor.py:1284`; `bin/disjuntor.py:1386`; `bin/disjuntor.py:1520`)

   No fluxo normal de `main_pretool`, a marca é limpa na entrada e acrescentada somente depois de `_nega_pdc` confirmar LIVRE; não encontrei reaproveitamento serial de julgamento anterior. Já JSON inválido no payload pode liberar a ferramenta sem julgamento PDC: o envelope recebe `{}`, não reconhece obra e termina na falha aberta. Isso é liberação da ferramenta, não retorno literal `PDC_LIVRE`; aceitação desse payload pela ferramenta real: **[nao verificado]**. (`bin/disjuntor.py:2016`; `bin/disjuntor.py:1991`; `bin/disjuntor.py:1995`; `bin/disjuntor.py:1539`; `bin/disjuntor.py:2019`; `bin/disjuntor.py:2043`; `bin/disjuntor.py:2052`)

2. **Escritores, leitores e trava**

   | Dado | Quem escreve | Quem lê |
   |---|---|---|
   | Escopo: `disparadas`, `duvida`, `obras_com_evento` | `_pdc_grava_escopo` monta e instala o arquivo por `replace`. (`bin/disjuntor.py:1467`; `bin/disjuntor.py:1473`) | `_pdc_escopo_lido`; `estado_pdc` consulta os três campos. (`bin/disjuntor.py:1480`; `bin/disjuntor.py:1500`; `bin/disjuntor.py:1502`) |
   | `_PDC_LIVRE_NESTA` | Inicialização, limpeza na entrada e inclusão após julgamento. (`bin/disjuntor.py:1559`; `bin/disjuntor.py:2016`; `bin/disjuntor.py:1995`) | `_ramo_pdc`. (`bin/disjuntor.py:1571`) |
   | Eventos do resolvedor | `decide_pdc._fn` cria eventos e atualiza `entidades/res` herdados. (`bin/disjuntor.py:1386`; `bin/disjuntor.py:1389`) | Cache de resolução, janela/agrupamento, nomeação e painel. (`bin/disjuntor.py:1375`; `bin/disjuntor.py:1288`; `bin/disjuntor.py:1297`; `bin/disjuntor.py:1427`; `bin/disjuntor.py:1580`) |
   | `visto_em` | Atribuído na criação; a reclassificação preserva o campo. (`bin/disjuntor.py:1386`; `bin/disjuntor.py:1389`) | Filtro da janela. (`bin/disjuntor.py:1289`) |

   Não identifiquei segundo escritor persistente desses dados no fluxo auditado. A chamada ao escritor do escopo ocorre em `_anuncia`; `_com_estado` mantém `LOCK_EX` durante a decisão, a gravação do estado e o anúncio. A marca em memória é escrita fora dessa trava, mas pertence ao envelope da chamada. (`bin/disjuntor.py:1444`; `bin/disjuntor.py:260`; `bin/disjuntor.py:275`; `bin/disjuntor.py:281`; `bin/disjuntor.py:293`; `bin/disjuntor.py:296`; `bin/disjuntor.py:1995`)

3. **As três provas de `_mordida.sh` ainda admitem falsos positivos**

   | Prova | Contraprova |
   |---|---|
   | Marca do assert | Um caso sintético `(return 1); false` deixa a marca do `return` da subshell e termina pelo `false`. O harness aceita porque compara apenas última marca e rc. Para expressões contendo `$`, nem compara o valor nem comprova o “último ato” anunciado no comentário. (`bin/tests/_mordida.sh:67`; `bin/tests/_mordida.sh:114`; `bin/tests/_mordida.sh:116`; `bin/tests/_mordida.sh:118`) |
   | Exceções por `sys.monitoring` | Um mutante que execute `os._exit(1)` antes da decisão não levanta exceção Python. `i14_mesma` receberia rc 1 e executaria seu `return 1`: marca válida, nenhuma exceção nova, regra sequer executada. Execução desse mutante: **[nao verificado]**. (`bin/tests/test_padrao_de_cima.sh:90`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:113`; `bin/tests/_mordida.sh:161`) |
   | Declaração da guarda | Basta declarar exatamente uma exceção plantada pelo próprio patch. O harness verifica presença e depois a retira da comparação; não verifica que o patch removeu uma guarda. `P17-passa-por-excecao` é um exemplo existente. (`bin/tests/_mordida.sh:150`; `bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:159`; `bin/tests/test_disjuntor.sh:1762`) |

   A sonda também perde **contexto e multiplicidade**: transforma ocorrências em conjuntos de tipo/arquivo/função/texto. Um mutante que redirecionasse toda leitura do estado para um caminho ausente produziria repetidamente o mesmo `FileNotFoundError` já esperado na primeira leitura do controle; reiniciaria o estado e cairia no contador de anúncios, mas a assinatura estaria autorizada pela base. Execução desse mutante: **[nao verificado]**. (`bin/tests/_mordida.sh:131`; `bin/tests/_mordida.sh:136`; `bin/tests/_mordida.sh:160`; `bin/orcamento_contexto.py:74`; `bin/disjuntor.py:269`; `bin/tests/test_padrao_de_cima.sh:120`)

   Os meta-casos cobrem exceções não declaradas, tipo errado, função errada no selo geral, prefixo, declaração ausente, `ValueError` e morte **sem marca alguma**. Não cobrem marca anterior de subshell, expressão sem último ato, repetição indevida de assinatura da base ou declaração exata de exceção artificial. (`bin/tests/test_disjuntor.sh:1799`; `bin/tests/test_disjuntor.sh:1803`; `bin/tests/test_disjuntor.sh:1807`; `bin/tests/test_disjuntor.sh:1809`; `bin/tests/test_disjuntor.sh:1812`; `bin/tests/test_padrao_de_cima.sh:687`; `bin/tests/test_padrao_de_cima.sh:705`)

4. **Mutantes reancorados e declarados**

   **Sim: `P17-passa-por-excecao` é vacuoso para a propriedade anunciada.** Ele substitui o cálculo por `1/0` e declara essa mesma `ZeroDivisionError`. O caso cai já na primeira exigência da mensagem orçamentária. Como a chamada é de verificador, o envelope pode responder B3-NAO-SEI e negar a ferramenta; portanto a mordida nem demonstra o anunciado “passa por exceção”. (`bin/tests/test_disjuntor.sh:1762`; `bin/tests/test_disjuntor.sh:525`; `bin/tests/test_disjuntor.sh:529`; `bin/disjuntor.py:1946`; `bin/disjuntor.py:2034`; `bin/disjuntor.py:2041`)

   Nos demais examinados há vínculo estático com a costura:

   | Mutantes | Evidência |
   |---|---|
   | `resolvedor-vira-vazio`, `resolvedor-sem-duvida`, `escopo-fora-da-trava` | Os casos exigem NAO_SEI, ausência de evento gravado e trava tomada. (`bin/tests/test_padrao_de_cima.sh:631`; `bin/tests/test_padrao_de_cima.sh:636`; `bin/tests/test_padrao_de_cima.sh:637`; `bin/tests/test_padrao_de_cima.sh:361`; `bin/tests/test_padrao_de_cima.sh:364`; `bin/tests/test_padrao_de_cima.sh:408`) |
   | `B3-ref-fora-da-pergunta`, `B3-symlink` | Há referência ruim fora da pergunta e link em diretório, respectivamente; o segundo conserva uma mordida comportamental além da exceção do link final. (`bin/tests/test_disjuntor.sh:1659`; `bin/tests/test_disjuntor.sh:1661`; `bin/tests/test_disjuntor.sh:740`; `bin/tests/test_disjuntor.sh:739`) |
   | `B3-foto-parcial`, `B3-foto-parcial-engolida` | Reintroduzem leitura textual parcial; o caso verifica que a linha anterior ao disparo não libere depois. (`bin/tests/test_disjuntor.sh:1663`; `bin/tests/test_disjuntor.sh:1664`; `bin/tests/test_disjuntor.sh:1023`; `bin/tests/test_disjuntor.sh:1031`) |
   | `P5`, `P7`, `P9`, `b3-sem-DESLIGADO` | Exercitam, respectivamente, B4 durante DIFF, entrada de tipo inválido, colisão do temporário fixo e chave desligada diante de estado corrupto. (`bin/tests/test_disjuntor.sh:475`; `bin/tests/test_disjuntor.sh:462`; `bin/tests/test_disjuntor.sh:653`; `bin/tests/test_padrao_de_cima.sh:326`; `bin/tests/test_padrao_de_cima.sh:327`) |
   | `P11-fifo-prende` | Prova preservação da decisão com FIFO presente; não prova que retirar o filtro necessariamente prende, pois a abertura ainda rejeita arquivo não regular. (`bin/tests/test_disjuntor.sh:1740`; `bin/tests/test_disjuntor.sh:580`; `bin/orcamento_contexto.py:77`) |

   Resultados executados desses mutantes e controles: **[nao verificado]**.

5. **Classes já reprovadas**

   | Classe | Conferência |
   |---|---|
   | Segundo juiz | As portas PDC convergem em `estado_pdc/decide_pdc`; o vocabulário é compartilhado com LEI-AKITA. Não identifiquei duplicação nesses consumidores. (`bin/disjuntor.py:1536`; `bin/disjuntor.py:1549`; `bin/disjuntor.py:1494`; `bin/disjuntor.py:1333`; `bin/tests/test_lei_akita.sh:89`) |
   | Segundo escritor fora da porta | Não identificado nos campos persistentes auditados; escopo e eventos passam pela transição sob trava. (`bin/disjuntor.py:1386`; `bin/disjuntor.py:1444`; `bin/disjuntor.py:293`) |
   | Relógio fora da borda | **Permanece:** o wrapper não injeta `agora`; `decide_pdc` lê `time.time()` internamente. O PreToolUse injeta o instante na entrada. (`bin/disjuntor.py:1549`; `bin/disjuntor.py:1353`; `bin/disjuntor.py:2022`; `bin/disjuntor.py:2025`) |
   | Derivação paralela | Agrupamento e reconhecimento são compartilhados; porém o fallback deriva LIVRE da ausência no RELATO, sem reconstruir a janela histórica — lacuna do item 1. (`bin/disjuntor.py:1292`; `bin/disjuntor.py:1181`; `bin/disjuntor.py:1508`; `bin/disjuntor.py:1514`) |
   | Selo sem mordida | **Reprovado pelas contraprovas do harness e pelo P17.** (`bin/tests/_mordida.sh:116`; `bin/tests/_mordida.sh:159`; `bin/tests/test_disjuntor.sh:1762`) |
   | Ordem de trava | Escopo sob trava do estado; wrapper adquire slot, depois trava da resposta, depois consulta B3/PDC, antes do Codex. (`bin/disjuntor.py:1444`; `bin/disjuntor.py:293`; `bin/codex_pergunta.sh:145`; `bin/codex_pergunta.sh:163`; `bin/codex_pergunta.sh:168`; `bin/codex_pergunta.sh:177`) |
   | Pouso antes do pré-requisito | Contrato aprovado, certificações e sequência real de pousos: **[nao verificado]**. | [nao verificado]

## 31/10 19:51 · sessao 35b9665cca65 · - · - · origem=Ronald (tmux)

Achados do harness, por leitura estática:

- **Marca não prova último ato.** `_mordida_marca` registra `return` dentro de subshell do corpo; `_mordida_morre` compara somente a última marca e o rc, sem verificar o comando final. Caso sintético `(return 1); false` deixa marca `return 1` e termina fora do assert com rc 1, aceito pelo ramo literal. Expressões com `$` são aceitas sem comparar sequer seu valor. (`bin/tests/_mordida.sh:67`; `bin/tests/_mordida.sh:114`; `bin/tests/_mordida.sh:118`)
  
  Reproduzi somente a trap em memória, sem arquivos: saída `marca=return 1` / `rc-final=1`. Execução integral do harness: [nao verificado].

- **Sonda não prova que a regra rodou.** Um mutante que execute `os._exit(1)` antes da decisão mata o processo sem exceção Python; `i14_mesma` recebe rc 1 e executa seu `return 1`, cuja marca satisfaz o harness. (`bin/tests/test_padrao_de_cima.sh:90`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:113`; `bin/tests/_mordida.sh:160`)

- **Linha de base perde contexto e multiplicidade.** As exceções viram conjuntos de tipo, arquivo, função e texto da linha; mensagem, argumento e quantidade desaparecem. (`bin/tests/_mordida.sh:131`; `bin/tests/_mordida.sh:136`; `bin/tests/_mordida.sh:160`)
  
  Contraprova estática concreta: inserir antes de `os.open` uma alteração de `caminho` para acrescentar `.ausente` quando termina em `/disjuntor_estado.json`, preservando o texto da linha do `os.open`. O controle já produz `FileNotFoundError` nessa linha na primeira leitura do estado ausente. O mutante produziria a mesma assinatura em todas as leituras, recriaria `{}` e reanunciaria os disparos; `i14_mesma` cairia no contador de anúncios. A sonda absorveria a repetição na base. (`bin/orcamento_contexto.py:74`; `bin/disjuntor.py:267`; `bin/disjuntor.py:269`; `bin/tests/test_padrao_de_cima.sh:38`; `bin/tests/test_padrao_de_cima.sh:120`) Execução desse mutante: [nao verificado].

- **Declaração prova presença, não legitimidade causal.** A exceção declarada é retirada de `vistas` antes da comparação com a base; nada verifica que o patch removeu uma guarda. O mutante existente `P17-passa-por-excecao` planta `o = 1 / 0` e declara precisamente sua própria `ZeroDivisionError`. Isso autoriza o mecanismo que deveria distinguir contaminação de mordida. (`bin/tests/_mordida.sh:142`; `bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:159`; `bin/tests/test_disjuntor.sh:1762`)

Os meta-casos cobrem exceção não declarada, tipo errado, função errada no selo geral, declaração por prefixo, declaração ausente, exceção fora dos cinco tipos e morte sem marca alguma. Não cobrem marca anterior de subshell, expressão sem último ato, repetição de assinatura já presente na base ou declaração exata de exceção plantada. (`bin/tests/test_disjuntor.sh:1799`; `bin/tests/test_disjuntor.sh:1817`; `bin/tests/test_padrao_de_cima.sh:687`; `bin/tests/test_padrao_de_cima.sh:707`)

## 31/10 19:50 · sessao f390df4bc2f0 · - · - · origem=Ronald (tmux)

Achado principal: **há LIVRE sem prova sobre o histórico quando estado e escopo estão ilegíveis/ausentes**. `_com_estado` levanta para JSON corrupto (`bin/disjuntor.py:268`, `bin/disjuntor.py:272`); `_pdc_escopo_lido` transforma escopo ausente/corrupto em `None` (`bin/disjuntor.py:1480`, `bin/disjuntor.py:1484`, `bin/disjuntor.py:1486`); `estado_pdc` então ignora as provas de `duvida`/`obras_com_evento` e retorna LIVRE se a obra não aparece no RELATO atual (`bin/disjuntor.py:1498`, `bin/disjuntor.py:1502`, `bin/disjuntor.py:1506`, `bin/disjuntor.py:1514`). Um evento ainda na janela pode ter saído do RELATO: o estado retém eventos por SHA e usa `visto_em`, independentemente da permanência no texto (`bin/disjuntor.py:1375`, `bin/disjuntor.py:1386`, `bin/disjuntor.py:1288`). Portanto, obra anteriormente disparada + eventos retirados do RELATO + estado corrupto + escopo ilegível → LIVRE; ocorrência em produção [não verificado].

As fontes efetivamente abertas pelo resolvedor propagam OSError/SyntaxError/UnicodeDecodeError até o `except` que adiciona a obra a `duvida` e não cria evento novo (`bin/disjuntor.py:1236`, `bin/disjuntor.py:1262`, `bin/disjuntor.py:1263`, `bin/disjuntor.py:1379`, `bin/disjuntor.py:1381`, `bin/disjuntor.py:1384`). Porém, os testes `isfile` anteriores transformam `False` em ausência e pulam a abertura (`bin/disjuntor.py:1233`, `bin/disjuntor.py:1259`); se um caminho inacessível resultar em `False`, o resolvedor poderá gravar `entidades=[]`, `res=2` e liberar (`bin/disjuntor.py:1284`, `bin/disjuntor.py:1386`, `bin/disjuntor.py:1520`). Reprodução dessa condição de permissões [não verificado].

RELATO ausente ou com erro de leitura retorna `None` (`bin/disjuntor.py:707`, `bin/disjuntor.py:712`); encoding inválido também vira texto `None` (`bin/disjuntor.py:1364`, `bin/disjuntor.py:1365`); nesse caminho não entram eventos novos e o resultado fica NAO_SEI para todas as obras fora das disparadas (`bin/disjuntor.py:1373`, `bin/disjuntor.py:1453`, `bin/disjuntor.py:1518`).

Inventário:

- **Escopo:** escritor `_pdc_grava_escopo`, conteúdo em `bin/disjuntor.py:1467`, instalação atômica em `bin/disjuntor.py:1473`; única chamada em `bin/disjuntor.py:1444`. Leitor `_pdc_escopo_lido` em `bin/disjuntor.py:1480`; consumidores de disparadas/duvida/obras_com_evento em `bin/disjuntor.py:1500`, `bin/disjuntor.py:1502`.
- **Eventos/resolvedor:** escritor novo em `bin/disjuntor.py:1386`; atualização de entidades/res herdados em `bin/disjuntor.py:1389`. Leitores de evento já resolvido em `bin/disjuntor.py:1375`; agrupamento/janela em `bin/disjuntor.py:1288`, `bin/disjuntor.py:1297`; nomeação em `bin/disjuntor.py:1427`; painel em `bin/disjuntor.py:1580`.
- **`visto_em`:** única atribuição em `bin/disjuntor.py:1386`; leitura em `bin/disjuntor.py:1289`; reclassificação preserva o campo porque só atualiza entidades/res (`bin/disjuntor.py:1389`).
- **`_PDC_LIVRE_NESTA`:** criação em `bin/disjuntor.py:1559`; limpeza em `bin/disjuntor.py:2016`; escrita após `_nega_pdc` não negar em `bin/disjuntor.py:1991`, `bin/disjuntor.py:1995`; leitura em `bin/disjuntor.py:1571`. `_nega_pdc` só retorna `None` para LIVRE (`bin/disjuntor.py:1536`, `bin/disjuntor.py:1539`).

As escritas persistentes do PDC ocorrem no callback `_fn` ou no anúncio, ambos sob `_com_estado`: trava exclusiva em `bin/disjuntor.py:260`, callback em `bin/disjuntor.py:275`, gravação em `bin/disjuntor.py:281`, anúncio em `bin/disjuntor.py:293`, fechamento em `bin/disjuntor.py:296`. Não identifiquei segundo escritor desses campos no código pesquisado; uso externo não presente na cópia [não verificado].

No fluxo serial de `main_pretool`, a marca não libera obra julgada em chamada anterior porque a limpeza precede a leitura do payload (`bin/disjuntor.py:2016`, `bin/disjuntor.py:2018`); antes da prova, `_ramo_pdc` nega a obra reconhecida (`bin/disjuntor.py:1571`, `bin/disjuntor.py:1573`). Payload Agent malformado pode escapar da classificação: o `except` de `_ramo_pdc` retorna `None`, permitindo a liberação do envelope (`bin/disjuntor.py:1568`, `bin/disjuntor.py:1569`, `bin/disjuntor.py:1570`, `bin/disjuntor.py:2052`); aceitação desse payload pela ferramenta real [não verificado].

**Relógio fora da borda permanece:** o caminho CLI wrapper → `padrao_parada` → `estado_pdc` não injeta `agora` (`bin/disjuntor.py:2265`, `bin/disjuntor.py:1549`), e `decide_pdc` lê `time.time()` internamente (`bin/disjuntor.py:1353`). O PreToolUse injeta o relógio da entrada corretamente (`bin/disjuntor.py:2022`, `bin/disjuntor.py:2025`, `bin/disjuntor.py:1991`, `bin/disjuntor.py:1536`).

O gate do wrapper está após aquisição do slot e da trava de resposta, antes da chamada Codex (`bin/codex_pergunta.sh:140`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`).

Os casos novos cobrem estado corrupto com escopo legível e evento removido do RELATO (`bin/tests/test_padrao_de_cima.sh:500`, `bin/tests/test_padrao_de_cima.sh:502`), além de chave `duvida` ausente num escopo ainda legível (`bin/tests/test_padrao_de_cima.sh:505`, `bin/tests/test_padrao_de_cima.sh:507`). O caso sem escopo mantém no RELATO a obra com evento (`bin/tests/test_padrao_de_cima.sh:253`, `bin/tests/test_padrao_de_cima.sh:259`, `bin/tests/test_padrao_de_cima.sh:260`), deixando descoberta a combinação histórica descrita acima.

## 31/10 19:50 · sessao 4639519e8746 · - · - · origem=Ronald (tmux)

**Q4 — há um vacuamente aceito para a regra que o nome anuncia:** `P17-passa-por-excecao` injeta literalmente `o = 1 / 0` e declara esse `ZeroDivisionError`; o harness remove a exceção declarada antes de comparar com o controle. (`bin/tests/test_disjuntor.sh:1762`; `bin/tests/_mordida.sh:154`; `bin/tests/_mordida.sh:159`)

O caso `codex` exige a mensagem orçamentária já na primeira chamada; a exceção artificial impede essa mensagem e provoca `return 1`. Como a chamada tem `CODEX_OBRA=X CODEX_PAPEL=verificador`, o envelope produz **B3-NAO-SEI**, recusando a ferramenta; portanto o mutante pode “morder” sem demonstrar o anunciado “passa por exceção”. A declaração só autoriza a falha que o próprio patch fabricou. (`bin/tests/test_disjuntor.sh:525`; `bin/tests/test_disjuntor.sh:528`; `bin/tests/test_disjuntor.sh:529`; `bin/disjuntor.py:1946`; `bin/disjuntor.py:2034`; `bin/disjuntor.py:2041`)

Os demais examinados possuem ligação estática com a regra:

- `B3-ref-fora-da-pergunta` agora valida apenas `perg.group(1)`, deixando passar a referência ruim anterior à pergunta; o caso 76 contém precisamente `/etc/passwd:1` nessa posição. Evita a antiga quebra por `sv=None` porque continua validando a pergunta antes de acessar `sv[0]`. (`bin/tests/test_disjuntor.sh:1659`; `bin/tests/test_disjuntor.sh:740`; `bin/disjuntor.py:653`; `bin/disjuntor.py:655`)
- `B3-symlink`: a exceção declarada vem do link no componente final, ainda barrado por `O_NOFOLLOW`; a mordida comportamental é o **diretório** simbólico, também presente no caso 74, que passa quando se remove `realpath`. Não depende somente da exceção declarada. (`bin/tests/test_disjuntor.sh:1661`; `bin/tests/test_disjuntor.sh:738`; `bin/tests/test_disjuntor.sh:739`; `bin/orcamento_contexto.py:74`; `bin/disjuntor.py:599`)
- `B3-foto-parcial` e `B3-foto-parcial-engolida` voltam à leitura textual que tolera prefixo parcial; o caso diferencia leitura binária íntegra de leitura textual quebrada e verifica se a linha antiga libera indevidamente. As exceções declaradas são consequências dessa leitura, não um `raise` gratuito introduzido pelo patch. (`bin/tests/test_disjuntor.sh:1663`; `bin/tests/test_disjuntor.sh:1664`; `bin/tests/test_disjuntor.sh:1023`; `bin/tests/test_disjuntor.sh:1031`; `bin/hook_stop_fila1.py:429`; `bin/hook_stop_fila1.py:441`)
- `P5-diff-cala-tudo` consulta a trava e retorna antes do B4; o caso mantém a trava tomada e exige a negativa B4. O `BlockingIOError` declarado é a observação esperada da trava tomada. (`bin/tests/test_disjuntor.sh:1723`; `bin/tests/test_disjuntor.sh:471`; `bin/tests/test_disjuntor.sh:475`; `bin/disjuntor.py:326`; `bin/disjuntor.py:329`)
- `P7-tipo-solto` remove uma guarda de tipo; o caso passa `"rate_limits": "x"` e exige rc 0 e `-`. A exceção declarada corresponde à operação desprotegida. (`bin/tests/test_disjuntor.sh:1731`; `bin/tests/test_disjuntor.sh:461`; `bin/tests/test_disjuntor.sh:462`; `bin/statusline_cota.py:41`)
- `P9-novo-fixo` troca temporário único pelo nome ocupado propositalmente por diretório; o assert exige que a linha ainda seja gravada. O `IsADirectoryError` representa exatamente a colisão que a regra evita. (`bin/tests/test_disjuntor.sh:1735`; `bin/tests/test_disjuntor.sh:650`; `bin/tests/test_disjuntor.sh:653`)
- `P11-fifo-prende` tira o filtro de arquivo regular; a segunda proteção continua evitando bloqueio, mas levanta `OSError` e impede a decisão orçamentária exigida pelo caso. Assim prova a preservação da decisão com FIFO presente, não que remover esse filtro necessariamente prenda o processo. (`bin/tests/test_disjuntor.sh:1740`; `bin/tests/test_disjuntor.sh:577`; `bin/tests/test_disjuntor.sh:580`; `bin/codex_limite_estado.py:146`; `bin/orcamento_contexto.py:74`; `bin/orcamento_contexto.py:77`)
- `b3-sem-DESLIGADO` retira a saída antecipada e força julgamento de JSON sabidamente corrompido; o caso exige sucesso com a chave desligada. As exceções declaradas são o efeito esperado de atravessar indevidamente essa porta. (`bin/tests/test_padrao_de_cima.sh:608`; `bin/tests/test_padrao_de_cima.sh:611`; `bin/tests/test_padrao_de_cima.sh:326`; `bin/tests/test_padrao_de_cima.sh:327`)
- Os reancorados `resolvedor-vira-vazio`, `resolvedor-sem-duvida` e `escopo-fora-da-trava` preservam alvos concretos: evento não gravado, resultado NAO_SEI e gravação sob trava. (`bin/tests/test_padrao_de_cima.sh:631`; `bin/tests/test_padrao_de_cima.sh:636`; `bin/tests/test_padrao_de_cima.sh:637`; `bin/tests/test_padrao_de_cima.sh:361`; `bin/tests/test_padrao_de_cima.sh:364`; `bin/tests/test_padrao_de_cima.sh:408`)

Execução dos mutantes, controles e resultados dos selos: **[não verificado]**.

## 31/10 19:44 · sessao 7a3217b254f8 · GRADE-FOLGA-JUIZ · censo · origem=Code

A cópia contém uma regra compartilhada pela precedência e pela grade: **folga manual pertence à pessoa; folga de calendário só vale quando pertence ao vínculo do dia**. O carregamento preserva todas as candidatas da pessoa, inclusive quando há duas folgas na mesma data; o predicado decide qual vale. Outros leitores usam universos diferentes, descritos abaixo. (`app/ponto/precedencia.py:195`, `app/ponto/precedencia.py:213`, `app/ponto/precedencia.py:223`, `app/escala/utils.py:1119`.)

Este é um censo estático. Resultado de execução dos testes, correspondência Git do HASH e ocorrência dos casos em produção: **[nao verificado]**. As referências a testes abaixo indicam o comportamento escrito neles. [nao verificado]

**1. Precedência, seleção da folga e grade**

| Sítio | Universo, vínculo do dia e calendário de outro vínculo | Chamadores e decisão movida | Testes | [nao verificado]
|---|---|---|---|
| `_folga_avulsa` — `app/ponto/precedencia.py:161`; filtro em `app/ponto/precedencia.py:165` | **Pessoa + data**; retorna `.first()`. Não resolve EC nem lê origem. Portanto encontra também calendário de outro vínculo. | `veto_de_lancamento`, em `app/ponto/precedencia.py:501`: veta folga duplicada e outros lançamentos humanos; férias atravessam a folga. (`app/ponto/precedencia.py:492`, `app/ponto/precedencia.py:502`, `app/ponto/precedencia.py:505`.) | Atestado recusado: `app/ponto/tests/test_a2_precedencia.py:402`; férias permitidas: `app/ponto/tests/test_a2_precedencia.py:725`; duplicidade recusada: `app/ponto/tests/test_a2_precedencia.py:740`. Caso específico de calendário de outro EC no veto: **[nao verificado]**. |
| `folgas_lancadas_do_periodo` — `app/ponto/precedencia.py:195`; filtro em `app/ponto/precedencia.py:207` | **Pessoa + intervalo inclusivo**, qualquer vínculo, ordenado por PK; preserva uma lista por data. Carrega calendário alheio como **candidato**, sem decidir validade. | `fatos_do_periodo`, em `app/ponto/precedencia.py:282`; overlay da grade, em `app/escala/utils.py:1119`. Alimenta precedência e grade. | Duas candidatas, com inválida de PK menor: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`; manual em vínculo fora da janela: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:91`. |
| `folga_lancada_do_dia` — `app/ponto/precedencia.py:213` | Recebe candidatas e EC; retorna a primeira aceita pelo predicado. Não filtra pessoa/data por conta própria nem escolhe EC. (`app/ponto/precedencia.py:217`.) | `fatos_do_periodo`, em `app/ponto/precedencia.py:358`; overlay, em `app/escala/utils.py:1124`. Escolhe o fato FolgaDia usado no dia. | Calendário alheio, manual antigo, calendário próprio e duas candidatas: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:75`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:80`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`. |
| `folga_lancada_vale_no_dia` — `app/ponto/precedencia.py:223` | `None` → falso; origem nula/atributo ausente → verdadeiro; calendário → exige `folga.escala_colaborador_id == ec.pk`. Não verifica pessoa/data. **Rejeita calendário de outro EC**, inclusive quando EC é `None`; manual continua aceita. (`app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:238`, `app/ponto/precedencia.py:240`.) | Chamado pelo seletor em `app/ponto/precedencia.py:218`; afeta precedência e grade pelos dois consumidores acima. | Integração: `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:45`, `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:50`; cinco casos da grade nas referências acima. Teste unitário direto de `None`/objeto sem origem: **[nao verificado]**. |
| `fatos_do_periodo` — `app/ponto/precedencia.py:260` | **Pessoa/período**. Carrega candidatas da pessoa; EC vem de `vinculo_do_dia`, com células e vínculos da janela, completando geradoras faltantes. Aplica o seletor e registra `fatos.folga_lancada`. **Rejeita calendário alheio como fato lançado**. (`app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:315`, `app/ponto/precedencia.py:319`, `app/ponto/precedencia.py:327`, `app/ponto/precedencia.py:351`, `app/ponto/precedencia.py:358`, `app/ponto/precedencia.py:408`.) | `fatos_do_dia`, em `app/ponto/precedencia.py:257`, e consumidores por período listados adiante. Folga entra na precedência junto com previsão falsa; outro fato pode vencê-la. (`app/ponto/precedencia.py:380`, `app/ponto/precedencia.py:392`.) | Origem/vínculo: `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:45`, `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:50`. Geradora versus ordem: `app/ponto/tests/test_precedencia_pela_celula.py:82`. Vínculo encerrado e período/dia: `app/ponto/tests/test_precedencia_vinculo_encerrado.py:74`, `app/ponto/tests/test_precedencia_vinculo_encerrado.py:84`. |
| `fatos_do_dia` — `app/ponto/precedencia.py:243` | É o período de um dia; herda integralmente universo, EC e regra de origem. (`app/ponto/precedencia.py:257`.) | Consumidores relacionados na tabela 5. | Igualdade dia/período: `app/ponto/tests/test_precedencia_vinculo_encerrado.py:84`; casos de calendário/manual: `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:45`, `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:50`. |
| `folga_humana_ou_feriado` — `app/ponto/precedencia.py:179` | Lê o resultado já julgado: feriado vencedor, ou faixa folga com FolgaDia lançada/subtipo de ausência. Não escolhe EC nem lê origem novamente. (`app/ponto/precedencia.py:187`.) | Status, em `app/ponto/status_dia.py:139`; calendário, em `app/colaboradores/services/calendario.py:817`. Move natureza/status do dia. | Manual vence dúvida de cadastro: `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:82`; calendário/manual da pessoa: `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:56`, `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:66`. |
| `veto_de_lancamento` — `app/ponto/precedencia.py:426` | Para FolgaDia usa `_folga_avulsa`: **pessoa/data, sem validade por origem/EC**. Assim, pode vetar por calendário alheio que o seletor da precedência rejeitaria. Para `falta`, também pergunta a previsão ao EC de `escala_vigente`. (`app/ponto/precedencia.py:501`, `app/ponto/precedencia.py:505`, `app/ponto/precedencia.py:528`.) | Ausência: `app/ponto/services/ausencia.py:380`; declaração: `app/ponto/services/declarar_dia.py:114`; folga: `app/escala/folgas.py:258`, `app/escala/folgas.py:283`; férias: `app/ferias/models.py:428`, `app/ferias/views.py:253`. Move **veto de escrita**. | `app/ponto/tests/test_a2_precedencia.py:392`, `app/ponto/tests/test_a2_precedencia.py:402`, `app/ponto/tests/test_a2_precedencia.py:725`, `app/ponto/tests/test_a2_precedencia.py:733`, `app/ponto/tests/test_a2_precedencia.py:740`. Divergência específica com calendário alheio: **[nao verificado]**. |
| `_aplicar_folgas_foto` — `app/escala/utils.py:1103` | **Pessoa**, carregada pela mesma função da precedência. EC recebido por `esc_do_dia(d)`. Aplica o mesmo seletor: calendário alheio rejeitado; manual antiga aceita. (`app/escala/utils.py:1119`, `app/escala/utils.py:1124`.) | Builder simples em `app/escala/utils.py:770` passa EC fixo; builder por turno em `app/escala/utils.py:1794` passa mapa diário. Marca folga e limpa células/órfãs. (`app/escala/utils.py:1126`.) | Ambos builders: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:75`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:80`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:91`; classes/flags em `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:97`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:102`. |

O EC usado por esses consumidores segue esta implementação: havendo geradora, procura seu PK e exige que a data esteja dentro de início/fim; geradora ausente da lista ou fora desses limites produz `None`. Sem geradora, usa candidatos que cobrem a data, desempata por ativa/início. (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:247`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:253`.)

Na grade por turno, o mapa diário chama essa autoridade; no legacy, a mesma autoridade determina os segmentos, e cada segmento chama o builder simples. (`app/escala/utils.py:1263`, `app/escala/utils.py:1268`, `app/escala/utils.py:973`, `app/escala/utils.py:975`, `app/escala/utils.py:995`, `app/escala/utils.py:999`.)

**2. Outros leitores diretos de FolgaDia e de `calendario_origem`**

“Somente o EC recebido” significa que o sítio não procura linhas dos demais vínculos; a seleção correta desse EC depende do chamador indicado na linha. (`app/ponto/services/declarar_dia.py:70`, `app/escala/models.py:1281`, `app/escala/folgas.py:254`.)

| Sítio | Universo e critério de EC/origem | Chamadores e decisão movida | Testes | [nao verificado]
|---|---|---|---|
| `_folga_do_dia` — `app/ponto/services/declarar_dia.py:61` | **EC recebido + data**; `.first()`. Não encontra manual nem calendário de outro EC. (`app/ponto/services/declarar_dia.py:70`.) | `o_dia_ja_e_folga`, em `app/ponto/services/declarar_dia.py:91`; `_recusa_do_dia`, em `app/ponto/services/declarar_dia.py:108`. Existência/idempotência e veto. | Manual: `app/ponto/tests/test_declarar_dia.py:79`; calendário: `app/ponto/tests/test_declarar_dia.py:87`. Outro EC: **[nao verificado]**. |
| `o_dia_ja_e_folga` / `_recusa_do_dia` — `app/ponto/services/declarar_dia.py:73`, `app/ponto/services/declarar_dia.py:94` | EC de `_ec_do_dia` → `escala_vigente` → `vinculo_do_dia`. Origem não nula da linha encontrada veta declaração manual; manual acusa duplicidade. (`app/ponto/services/declarar_dia.py:56`, `app/ponto/services/declarar_dia.py:90`, `app/ponto/services/declarar_dia.py:105`, `app/ponto/services/declarar_dia.py:109`, `app/escala/servico_jornada.py:57`.) | Aceite de folga: `app/chamados/services/validacao.py:261`, `app/chamados/services/validacao.py:316`; preview/escrita: `app/ponto/services/declarar_dia.py:141`, `app/ponto/services/declarar_dia.py:154`. Move aceite idempotente, veto e declaração. | `app/ponto/tests/test_declarar_dia.py:79`, `app/ponto/tests/test_declarar_dia.py:87`, `app/ponto/tests/test_declarar_dia.py:196`, `app/ponto/tests/test_declarar_dia.py:208`; aceite de quatro perguntas para um fato: `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:167`. |
| `folgas_do_periodo` — `app/escala/alimentacao.py:49`; query em `app/escala/alimentacao.py:66` | **Lista de ECs recebida**, datas arredondadas para meses inteiros; saída por EC. Não lê origem nem escolhe EC do dia. Calendário de outro EC permanece no balde daquele EC. (`app/escala/alimentacao.py:60`, `app/escala/alimentacao.py:69`.) | Alimentações listadas na tabela 4; precedência em `app/ponto/precedencia.py:333`. Deriva previsão, não a regra pessoa/manual. | Mês inteiro: `app/escala/tests/test_selo_alimentacao_dias.py:96`; equivalência/custo alimentado: `app/escala/tests/test_selo_alimentacao_dias.py:73`. |
| `defeito_folga_nao_definida` — `app/escala/models.py:1212`; query em `app/escala/models.py:1220` | **Próprio EC + mês**. No 6x1 sem folga semanal, qualquer FD do próprio EC elimina o defeito mensal. Não distingue origem; outro EC não serve. | Detector: `app/ponto/management/commands/detectar_ausencias.py:144`, `app/ponto/management/commands/detectar_ausencias.py:187`; supra: `app/ponto/supra_juiz.py:200`; limbo: `app/chamados/services/limbo_folgas.py:199`, `app/chamados/services/limbo_folgas.py:281`; plano: `app/escala/views_plano_folgas.py:89`; faltas hoje: `app/ponto/services/faltas_de_hoje.py:39`; régua: `app/escala/regua_defesa.py:145`, `app/escala/regua_defesa.py:176`; diagnóstico: `app/ponto/services/diagnostico_escala.py:104`. Move **defeito/cobrança de cadastro**. | `app/escala/tests/test_t1_ligado_por_padrao.py:34`, `app/escala/tests/test_t1_ligado_por_padrao.py:39`, `app/escala/tests/test_t1_ligado_por_padrao.py:45`, `app/escala/tests/test_t1_ligado_por_padrao.py:55`; ligação ao EC do juiz: `app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:177`. |
| `eh_dia_trabalho_calculado` — `app/escala/models.py:1223` | **Próprio EC**: consulta foto mensal em `app/escala/models.py:1257`, data exata em `app/escala/models.py:1281`, anterior em `app/escala/models.py:1327` e posterior em `app/escala/models.py:1330`; também aceita mapa por EC. **Nenhuma leitura distingue origem**. Foto do próprio EC no dia gera folga; fora dela usa ciclo ou paridade da foto próxima. Outro EC não entra. (`app/escala/models.py:1251`, `app/escala/models.py:1282`, `app/escala/models.py:1290`, `app/escala/models.py:1335`.) | `eh_dia_trabalho`, em `app/escala/models.py:1073`, e chamadas diretas da tabela 4. Move geração de célula/previsão. | Foto vence âncora: `app/escala/tests/test_foto_vence_ancora.py:38`, `app/escala/tests/test_foto_vence_ancora.py:44`; preserva ciclo: `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:50`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:60`; foto parcial/paridade: `app/escala/tests/test_o37_foto_parcial.py:69`, `app/escala/tests/test_o37_foto_parcial.py:87`, `app/escala/tests/test_o37_foto_parcial.py:106`, `app/escala/tests/test_o37_foto_parcial.py:114`. |
| `medir` / `_foto_diz` — `app/escala/services/fase_conflitante.py:61`, `app/escala/services/fase_conflitante.py:51` | **Cada EC ativo**, todas as suas datas de FD, sem recorte na query; não escolhe EC diário nem lê origem. `_foto_diz` reconhece presença e extrapola paridade da foto próxima. (`app/escala/services/fase_conflitante.py:71`, `app/escala/services/fase_conflitante.py:74`, `app/escala/services/fase_conflitante.py:55`.) | Command em `app/escala/management/commands/lavrar_fase_conflitante.py:31`; snapshot em `app/escala/management/commands/lavrar_fase_conflitante.py:35`; cron em `app/config/crons.py:479`. Move contador de conflito foto/âncora. | `app/escala/tests/test_fase_conflitante.py:60`, `app/escala/tests/test_fase_conflitante.py:72`, `app/escala/tests/test_fase_conflitante.py:80`, `app/escala/tests/test_fase_conflitante.py:84`, `app/escala/tests/test_fase_conflitante.py:88`, `app/escala/tests/test_fase_conflitante.py:93`. |
| `divergencia_foto_x_calendario` — `app/escala/folgas.py:83` | **EC recebido + mês**; separa manual/calendário pela origem. Compara a foto com calendário atual do EC, sem exigir igualdade entre origem de cada FD e calendário atual. Outro EC não entra. (`app/escala/folgas.py:99`, `app/escala/folgas.py:101`, `app/escala/folgas.py:104`.) | Agregador em `app/escala/folgas.py:143`; command em `app/escala/management/commands/lavrar_foto_folgas.py:113`, `app/escala/management/commands/lavrar_foto_folgas.py:167`. `sobra` impede reconciliação do mês. (`app/escala/management/commands/lavrar_foto_folgas.py:168`.) | `app/escala/tests/test_foto_folga_continua.py:136`, `app/escala/tests/test_foto_folga_continua.py:158`, `app/escala/tests/test_foto_folga_continua.py:179`. |
| `gerar_foto_folgas` — `app/escala/folgas.py:150` | **EC recebido + mês**; lê manuais para preservá-las e seleciona calendário para reconciliar. Não procura outro EC. (`app/escala/folgas.py:165`, `app/escala/folgas.py:167`, `app/escala/folgas.py:169`, `app/escala/folgas.py:171`.) | Views em `app/escala/views.py:414`, `app/colaboradores/views.py:245`; serviço em `app/colaboradores/services/vinculo.py:163`; command em `app/escala/management/commands/lavrar_foto_folgas.py:170`. Materializa foto e chama relavra em `app/escala/folgas.py:192`. | `app/escala/tests/test_gerador_folgas.py:39`, `app/escala/tests/test_gerador_folgas.py:47`, `app/escala/tests/test_gerador_folgas.py:54`, `app/escala/tests/test_gerador_folgas.py:63`; preservação manual: `app/escala/tests/test_foto_folga_continua.py:179`; relavra: `app/escala/tests/test_torneira_folgas.py:87`. |
| `lancar_folga` — `app/escala/folgas.py:228` | **EC recebido + dia**; linha própria com calendário veta escrita manual. Depois consulta o veto por **pessoa**, que pode encontrar FD de outro EC. (`app/escala/folgas.py:254`, `app/escala/folgas.py:255`, `app/escala/folgas.py:258`.) | Plano em `app/escala/views_plano_folgas.py:131`; declaração em `app/ponto/services/declarar_dia.py:183`. Cria/obtém foto e relavra. (`app/escala/folgas.py:259`, `app/escala/folgas.py:261`.) | `app/escala/tests/test_torneira_folgas.py:68`; calendário/veto da declaração: `app/ponto/tests/test_declarar_dia.py:87`. |
| `_foto_do_dia` — `app/escala/folgas.py:267` | **EC recebido + dia**; origem não nula veta alteração/remoção. Não lê outro EC. (`app/escala/folgas.py:274`, `app/escala/folgas.py:275`.) | `alterar_folga`, em `app/escala/folgas.py:302`; lote, em `app/escala/folgas.py:334`; views em `app/escala/views_plano_folgas.py:136`, `app/escala/views_plano_folgas.py:185`. Move veto, alteração/remoção e relavra. | `app/escala/tests/test_bug136_plano_em_lote_relavra.py:68`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:75`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:93`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:114`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:124`. |
| `reparentar_folga` — `app/escala/folgas.py:392` | FD individual: calendário retorna sem migrar; manual fica se EC atual cobre a data; caso contrário, alvo vem de `vinculo_do_dia_carregado`. FD de qualquer origem já existente no alvo impede migração. (`app/escala/folgas.py:405`, `app/escala/folgas.py:408`, `app/escala/folgas.py:414`, `app/escala/folgas.py:418`.) | Coletor em `app/escala/folgas.py:459`; command em `app/escala/management/commands/reparentar_folgas.py:34`. Move titularidade; relavra da competência corrente em diante. (`app/escala/folgas.py:435`, `app/escala/folgas.py:441`.) | `app/escala/tests/test_folga_segue_a_pessoa.py:53`, `app/escala/tests/test_folga_segue_a_pessoa.py:66`, `app/escala/tests/test_folga_segue_a_pessoa.py:80`, `app/escala/tests/test_folga_segue_a_pessoa.py:92`, `app/escala/tests/test_folga_segue_a_pessoa.py:104`, `app/escala/tests/test_folga_segue_a_pessoa.py:113`. |
| `reparentar_folgas_do_colaborador` — `app/escala/folgas.py:447` | **Pessoa inteira, somente manuais**; seleciona vínculos que não cobrem a data e delega destino à porta acima. (`app/escala/folgas.py:453`, `app/escala/folgas.py:457`, `app/escala/folgas.py:459`.) | Signal em `app/escala/signals.py:190`; move a mesma migração/relavra. | Troca de escala leva folga ao novo vínculo: `app/escala/tests/test_folga_segue_a_pessoa.py:53`. |
| Command `reparentar_folgas.handle` — `app/escala/management/commands/reparentar_folgas.py:22` | **Todas as manuais**; ignora as cujo EC cobre a data e delega à porta individual. (`app/escala/management/commands/reparentar_folgas.py:28`, `app/escala/management/commands/reparentar_folgas.py:31`, `app/escala/management/commands/reparentar_folgas.py:34`.) | Entrada management command; `apply` controla aplicação. (`app/escala/management/commands/reparentar_folgas.py:26`.) | Porta individual: testes acima. Teste específico do command: **[nao verificado]**. |
| `_grid_ctx` — `app/escala/views_plano_folgas.py:51` | **ECs ativos da empresa/posto + mês**. Mapa por EC/data; não escolhe vínculo diário nem distingue origem. FD de outro EC não pinta a linha deste EC. FD presente precede a previsão. (`app/escala/views_plano_folgas.py:55`, `app/escala/views_plano_folgas.py:60`, `app/escala/views_plano_folgas.py:63`, `app/escala/views_plano_folgas.py:70`, `app/escala/views_plano_folgas.py:76`.) | View em `app/escala/views_plano_folgas.py:46`; template usa tipo pronto em `app/templates/escala/plano_folgas.html:81`. Move grade de cadastro. | `app/escala/tests/test_plano_folgas_filtro.py:53`, `app/escala/tests/test_plano_folgas_filtro.py:58`, `

[truncado em 20000 de 47238 caracteres]

## 31/10 19:42 · sessao 453b70466694 · V3-P5-PRORROGACAO-POR-INTERVALO · censo · origem=Code

**O booleano é o mesmo, mas a elegibilidade da prorrogação é recalculada por intervalo.** A função recebe `entrada/saida`, recorta cada pausa e chama `minutos_noturnos` com as pontas desse pedaço; esta só prorroga quando a própria chamada começa antes das 05:00 e termina depois. Assim, a pausa 05:30–06:30 não herda a prorrogação do turno 22:00–07:00. (`app/ponto/motor_calculo_v2.py:109-111`, `app/ponto/motor_calculo_v2.py:141-145`.)

**1. Chamadores da função alvo e destino do resultado**

O censo estático encontrou **dois sítios de chamada direta**. As ocorrências do nome em testes são texto explicativo e mensagem de asserção; não são chamadas. (`app/ponto/motor_calculo_v2.py:392`, `app/ponto/motor_calculo_v2.py:1865`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:18`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:54`.) Chamadas construídas por reflexão sem referência estática identificável: [nao verificado].

| Chamador direto | Valor de `prorrogacao` | Origem de `entrada/saida` | Destino imediato |
|---|---|---|---|
| `he_noturna` | Seu argumento `prorrogacao`, cujo padrão é `True`. | Passa `inicio_extra`, calculado por `_inicio_do_trecho_extra`, e `fim=saida`: é a cauda de HE, não necessariamente o turno inteiro. | Subtrai de `minutos_extra_not`; reparte proporcionalmente entre `not_50/not_100` e retorna a dupla. (`app/ponto/motor_calculo_v2.py:359`, `app/ponto/motor_calculo_v2.py:385-401`.) |
| `MotorBase._noturno_dos_intervalos` | `self.prorrogacao_noturna_pos5h`. | Repassa seus parâmetros `entrada/saida/intervalos` sem alterar. | Retorna o desconto aos leitores abaixo. (`app/ponto/motor_calculo_v2.py:1852`, `app/ponto/motor_calculo_v2.py:1865-1866`.) |

Os caminhos que chegam a esses dois sítios são:

| Caminho | Pontas, flag e destino |
|---|---|
| **Motor → adicional noturno** | `calcular_noturnas` calcula o bruto e desconta `_noturno_dos_intervalos` com o mesmo par e o mesmo flag; aplica o fator e retorna relógio/legais. `_escrever_noturnas` grava `minutos_noturnos_relogio/legais`. (`app/ponto/motor_calculo_v2.py:921-925`, `app/ponto/motor_calculo_v2.py:941-943`.) |
| **12x36 → `_escrever_noturnas`** | `e/s` vêm de `_janela_do_par(localizar(entrada), localizar(saida))`; os intervalos passam por `_intervalos_na_janela`. (`app/ponto/motor_calculo_v2.py:2697-2702`, `app/ponto/motor_calculo_v2.py:2730`.) |
| **Turno partido → `_escrever_noturnas`** | Mesma preparação de pontas efetivas e intervalos recortados. (`app/ponto/motor_calculo_v2.py:2824-2834`.) |
| **Comercial → `_escrever_noturnas`** | Mesma preparação de pontas efetivas e intervalos recortados. (`app/ponto/motor_calculo_v2.py:3149-3150`, `app/ponto/motor_calculo_v2.py:3162`, `app/ponto/motor_calculo_v2.py:3185`.) |
| **Motor → HE noturna** | `_aplicar_he_noturna` passa `periodo.entrada/saida`, extras 50/100, flag do motor e `intervalos_do_motor`; grava `minutos_extra_100_noturna` e `minutos_extra_50_noturna`. Aqui as pontas são as originais localizadas, diferentemente das pontas efetivas do adicional acima. (`app/ponto/motor_calculo_v2.py:952-960`, `app/ponto/motor_calculo_v2.py:2689`, `app/ponto/motor_calculo_v2.py:2816`, `app/ponto/motor_calculo_v2.py:3141`.) |
| **Calculador → adicional, por método emprestado** | `_ContextoNoturno` guarda `bool(prorrogacao_pos5h)` e atribui `MotorBase._noturno_dos_intervalos` como método da classe. `noturnas_do_dia` calcula cada par e desconta por esse método. (`app/ponto/calculador/regras.py:60-63`, `app/ponto/calculador/regras.py:80-89`.) |
| **Calculador → HE, por apelido de import** | Importa `he_noturna as _he_not`; chama por ponta de período, com `prorrogacao_pos5h` e os intervalos daquela ponta; soma e grava `horas_extras_50_noturna/horas_extras_100_noturna`. (`app/ponto/calculador/regras.py:868`, `app/ponto/calculador/regras.py:880-898`.) |
| **Releitura v3 → adicional por par** | `noturno_no_par` passa um par inteiro, intervalos e `cct.prorrogacao_pos5h` a `noturnas_do_dia`; `reler` grava os minutos no período. Esse caminho se declara de ensaio, não de produção. (`app/ponto/calculador/periodo.py:508-511`, `app/ponto/calculador/periodo.py:544`, `app/ponto/calculador/periodo.py:752-754`.) |

As cinco chamadas de `_aplicar_he_noturna` estão no 12x36 por período, no partido após HE diária e semanal, e no comercial por período e após HE diária mensal. (`app/ponto/motor_calculo_v2.py:2793`, `app/ponto/motor_calculo_v2.py:3085`, `app/ponto/motor_calculo_v2.py:3116`, `app/ponto/motor_calculo_v2.py:3219`, `app/ponto/motor_calculo_v2.py:3337`.)

Plantão reutiliza o cálculo do 12x36; `Motor12x36ComEscala` chama `super`; partido noturno e intermitente herdam os caminhos correspondentes. (`app/ponto/motor_calculo_v2.py:3448-3461`, `app/ponto/motor_calculo_v2.py:3474-3495`.)

Na alimentação de produção do calculador, as pontas vêm de `efetivo_entrada/saida`, e as pausas de `intervalos_do_motor`; são montados `he_pontas`, pares e intervalos por dia. O flag vem da CCT daquele dia e segue para `regras.do_dia`. (`app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/alimentacao.py:127-135`, `app/ponto/calculador/alimentacao.py:185-190`, `app/ponto/calculador/alimentacao.py:572-596`.)

Os leitores seguintes dos minutos são as propriedades do período/mês, a lavratura diária e o fechamento mensal: `horas_noturnas` soma períodos comuns **e folgas**; `horas_extra_100_noturna` soma períodos comuns. (`app/ponto/motor_calculo_v2.py:210-211`, `app/ponto/motor_calculo_v2.py:271`, `app/ponto/motor_calculo_v2.py:292-294`, `app/ponto/services/dia_pago.py:179-184`, `app/ponto/services/dia_pago.py:224`, `app/ponto/services/fechamento.py:893-899`.)

Nos testes, as chamadas explícitas a `he_noturna` usam a grade de pontas com `True/False`, sem pausas, ou `True` com pausas anteriores às 05:00; o P5 alcança a função por `regras.do_dia`, com pausa pós05. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:90-123`, `app/ponto/tests/test_noturno_desconta_intervalo.py:249-267`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:40-47`.)

**2. Mesmo valor do turno? Alguém já passa a janela do turno?**

- **Via `calcular_noturnas`: sim, o flag é igual** no bruto do par e no desconto das pausas. Também são passadas as pontas do par inteiro ao desconto. Entretanto, a função alvo usa essas pontas apenas para recortar cada pausa; não transporta a janela noturna já reconhecida para o par. (`app/ponto/motor_calculo_v2.py:921-923`, `app/ponto/motor_calculo_v2.py:141-145`, `app/ponto/motor_calculo_v2.py:1865-1866`.)
- **Via `he_noturna`: o flag também é igual**, mas as pontas passadas ao desconto são as da cauda extra. Tanto a elegibilidade do bruto da HE quanto a das pausas é avaliada sobre suas respectivas chamadas. (`app/ponto/motor_calculo_v2.py:390-393`.)
- **Via calculador: o mesmo flag da CCT do dia** alimenta bruto, desconto e split de HE. Isso não comprova igualdade com o flag de um motor montado para outra data/fatia. (`app/ponto/calculador/alimentacao.py:448`, `app/ponto/calculador/alimentacao.py:572-596`, `app/ponto/calculador/regras.py:80-86`, `app/ponto/calculador/regras.py:886-888`.)
- Portanto, **há caminhos que já passam o envelope inteiro do par**, mas **nenhum dos dois chamadores diretos passa uma janela noturna elegível previamente decidida pelo turno**; a assinatura só recebe pontas, intervalos e flag. (`app/ponto/calculador/periodo.py:508-510`, `app/ponto/motor_calculo_v2.py:121`, `app/ponto/motor_calculo_v2.py:392-393`, `app/ponto/motor_calculo_v2.py:1865-1866`.)

O flag do motor é recebido explicitamente ou resolvido pelo ciclo quando chega `None`; a fábrica CCT ativa pode substituí-lo pelo valor da régua. Valores cadastrados efetivos em produção: [nao verificado]. (`app/ponto/motor_calculo_v2.py:708-712`, `app/core/regua_cct.py:182`, `app/core/regua_cct.py:458-466`.)

**3. Outras funções: turno ou pedaço**

| Função | Como decide a prorrogação |
|---|---|
| `minutos_noturnos` | Decide pela geometria **da própria chamada**: início antes das 05:00 e fim depois. Não recebe contexto de turno externo. (`app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:109-111`.) |
| `minutos_noturnos_dos_intervalos` | Mede **cada pausa recortada separadamente**, repassando o flag. (`app/ponto/motor_calculo_v2.py:141-145`.) |
| `MotorBase._noturno_dos_intervalos` | Só delega; portanto o desconto continua decidido por pedaço. (`app/ponto/motor_calculo_v2.py:1865-1866`.) |
| `MotorBase.calcular_noturnas` | Bruto pelo par recebido; desconto por pausa isolada. Usa o mesmo flag em ambos. (`app/ponto/motor_calculo_v2.py:921-925`.) |
| `he_noturna` | Bruto pela cauda extra, cujo início recua por minutos trabalhados; desconto por pausa recortada nessa cauda. (`app/ponto/motor_calculo_v2.py:328-356`, `app/ponto/motor_calculo_v2.py:390-393`.) |
| `regras._ContextoNoturno` | Não deriva uma janela: guarda o booleano e empresta o método do motor. (`app/ponto/calculador/regras.py:60-63`.) |
| `regras.noturnas_do_dia` | Bruto por par; desconto por pausa, via contexto emprestado; aplica fator após a soma. (`app/ponto/calculador/regras.py:80-89`.) |
| `periodo.noturno_no_par` | Encaminha um único par inteiro e suas pausas à porta acima; conserva a mesma assimetria entre bruto do par e desconto por pedaço. (`app/ponto/calculador/periodo.py:508-511`.) |

**4. Escritores e leitores de `horas_noturnas` e `horas_extras_100_noturna`**

Os dois campos existem em `DiaPago` e `FechamentoMensal`; o resultado do motor usa o nome singular `horas_extra_100_noturna`, convertido para o plural no fechamento. (`app/ponto/models.py:327`, `app/ponto/models.py:332`, `app/ponto/models.py:413`, `app/ponto/models.py:419`, `app/ponto/services/fechamento.py:899`.)

| Escritor/derivador | Campos e operação |
|---|---|
| Motor | `_escrever_noturnas` grava minutos noturnos relógio/legais; `_aplicar_he_noturna` grava minutos de HE100 noturna. As propriedades convertem e somam em horas. (`app/ponto/motor_calculo_v2.py:941-943`, `app/ponto/motor_calculo_v2.py:958`, `app/ponto/motor_calculo_v2.py:271`, `app/ponto/motor_calculo_v2.py:294`.) |
| Fechamento mensal | Atribui os dois campos a partir de `resultado`; persiste por `save` ou `update`. (`app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1112`, `app/ponto/services/fechamento.py:1131`, `app/ponto/services/fechamento.py:1137`.) |
| Calculador v3 | Escreve noturnas nos ramos folga/comum e HE100 noturna no split. (`app/ponto/calculador/regras.py:739`, `app/ponto/calculador/regras.py:898`, `app/ponto/calculador/regras.py:912`.) |
| `dia_pago.lavrar` | Soma ambos dos períodos; acrescenta noturnas das folgas; substitui campos pelos `valores` do calculador quando cabível; persiste por `bulk_create`. (`app/ponto/services/dia_pago.py:179-184`, `app/ponto/services/dia_pago.py:224`, `app/ponto/services/dia_pago.py:320-328`, `app/ponto/services/dia_pago.py:343-358`.) |
| Integração do fechamento | Lavra a versão `motor`; calcula as rubricas v3 e lavra separadamente a versão `oraculo`. (`app/ponto/services/fechamento.py:1169-1195`, `app/ponto/services/fechamento.py:1239-1262`.) |
| Releitura v3 de ensaio | Reescreve minutos noturnos no período, posteriormente lidos pelas propriedades do resultado. (`app/ponto/calculador/periodo.py:544`, `app/ponto/calculador/periodo.py:752-754`, `app/ponto/motor_calculo_v2.py:271`.) |
| `restaurar_fechamento` | Escritor genérico: aceita campos existentes e os persiste por `**_upd`, podendo atingir ambos. (`app/ponto/services/fechamento.py:1342-1361`.) |
| `retratar_exportada` | Pode criar linhas diárias e ajuste com ambos zerados por compreensão sobre `CAMPOS`. (`app/ponto/services/dia_pago.py:470-477`, `app/ponto/services/dia_pago.py:508-515`, `app/ponto/services/dia_pago.py:543`.) |
| Restauros em `bin` | Reescrevem campos mensais por `setattr/save` e recriam `DiaPago`: `bin/restore_relavratura_10_2026.py:79-86`, `bin/restore_o263_comp10.py:48-56`. O restauro de badge recria os campos diários presentes no snapshot: `bin/restore_badge_linha_09_10.py:27`, `bin/restore_badge_linha_09_10.py:38-41`, `bin/restore_badge_linha_09_10.py:63`. |

Os leitores funcionais e de apresentação são:

| Leitor | Uso |
|---|---|
| Agregações de `DiaPago` | Ambos por listas de campos: soma da competência e soma do período. (`app/ponto/services/dia_pago.py:34-36`, `app/ponto/services/dia_pago.py:560-561`, `app/ponto/services/dia_pago.py:782-787`.) |
| `pago_do_dia` | Lê ambos para detectar existência de dinheiro lavrado. (`app/ponto/services/dia_pago.py:859-862`, `app/ponto/services/dia_pago.py:890`.) |
| `rubricas_do_dia` | Lê `horas_noturnas` para tela/papel; consumido por espelho, calendário e PDF. (`app/ponto/services/dia_pago.py:922`, `app/ponto/services/espelho.py:824`, `app/colaboradores/services/calendario.py:785`, `app/relatorios/pdf_espelho.py:840`, `app/relatorios/pdf_espelho.py:906`.) |
| `folha/porta_export` | Compara ambos genericamente, lendo cada campo da soma e do fechamento. (`app/folha/porta_export.py:451-458`.) |
| **`folha/export`** | Lê `horas_noturnas` → evento `adicional_noturno`. Não emite HE100 noturna como linha adicional no retorno de eventos. (`app/folha/export.py:413-450`.) |
| TXT e homologação | Consomem `eventos_do_fechamento`. O código da rubrica sai do cadastro, por `_rub_map`/`codigo_fopag`; “25” aparece no comentário, não como código fixo dessa emissão. Cadastro real: [nao verificado]. (`app/folha/export.py:419`, `app/folha/export.py:694`, `app/folha/export.py:711-719`, `app/folha/export.py:976`.) |
| Simulação de folha | Lê ambos por `CAMPOS_HORAS/getattr`. (`app/folha/management/commands/simular_folha.py:59-60`, `app/folha/management/commands/simular_folha.py:84`.) |
| Ranking | Lê efetivamente `horas_noturnas`. (`app/folha/services/ranking_he.py:225`.) |
| Relatórios e espelho | Leem noturnas para CSV, totais, extrato e cartão. (`app/relatorios/views.py:94`, `app/relatorios/views.py:108`, `app/relatorios/views.py:524`, `app/relatorios/cartao_pela_celula.py:176`, `app/ponto/services/espelho.py:99`.) |
| Templates | Leem noturnas. (`app/templates/relatorios/horas_mes.html:68`, `app/templates/ponto/_fech_linha.html:31`, `app/templates/ponto/partials/painel_fechamento.html:33`.) |

Também há acessos de manutenção, comparação e snapshot — inclusive dinâmicos, que uma busca apenas por atribuição literal perderia:

- `aplicar_09_he_intra` fotografa ambos e pode alterar HE100 noturna pela porta de restauração; `aplicar_09_corte_b` fotografa ambos, recalcula e restaura campos; `desvio_o68b` fotografa e restaura campos selecionados. (`app/ponto/management/commands/aplicar_09_he_intra.py:39-45`, `app/ponto/management/commands/aplicar_09_he_intra.py:128-152`, `app/ponto/management/commands/aplicar_09_corte_b.py:184`, `app/ponto/management/commands/aplicar_09_corte_b.py:212`, `app/ponto/management/commands/aplicar_09_corte_b.py:265-268`, `app/ponto/management/commands/desvio_o68b.py:149`, `app/ponto/management/commands/desvio_o68b.py:195-202`.)
- Hash/snapshot de ambos por introspecção ou lista canônica: `carimbo_gravado`, `lavrar_dias_pagos`, `recalcular_fechamento`, `regen_exportada` e a serialização do modo leitura do fechamento. (`app/ponto/management/commands/carimbo_gravado.py:36`, `app/ponto/management/commands/carimbo_gravado.py:54-55`, `app/ponto/management/commands/lavrar_dias_pagos.py:70-77`, `app/ponto/management/commands/recalcular_fechamento.py:54-67`, `app/ponto/portas/regen_exportada.py:72-84`, `app/ponto/services/fechamento.py:1102-1104`.)
- `diff_frota` lê os campos recebidos; seus chamadores passam a lista que contém ambos. `diff_calculador` também monta o valor diário de noturnas do motor. (`app/ponto/services/diff_frota.py:63`, `app/ponto/management/commands/diff_col900.py:45`, `app/ponto/management/commands/diff_passivo_s84.py:50`, `app/ponto/management/commands/diff_janela_he_total.py:101-115`, `app/ponto/management/commands/diff_calculador.py:260`.)
- Outros leitores de noturnas: `celula_veredito_velho`, `diff_reclassificar_partido` e a lista de rubricas de autorização de HE. (`app/ponto/management/commands/celula_veredito_velho.py:70-85`, `app/ponto/management/commands/celula_veredito_velho.py:155`, `app/ponto/management/commands/diff_reclassificar_partido.py:67`, `app/ponto/management/commands/diff_reclassificar_partido.py:146-156`, `app/ponto/services/autorizacao_he_periodo.py:48-50`.)
- Scripts de fotografia/diff leem ambos: `bin/snapshot_relavratura_10_2026.py:31-37`, `bin/snapshot_o263_comp10.py:57-63`, `bin/snapshot_badge_linha_09_10.py:50-60`, `bin/diff_o118_col221.py:23-27`, `bin/diff_o119_frota.py:27-46`, `bin/diff_o124_frota.py:31`, `bin/aplicar_o263_comp10.py:44-46`.
- Os scripts de relavratura também fotografam ambos genericamente antes de chamar o recálculo: `bin/relavrar_o118_col221.py:26-40`, `bin/relavrar_o119_col221.py:25-39`, `bin/relavrar_o120_col221_09.py:29-41`, `bin/relavrar_o120_col221_09.py:62-69`.

Fixtures e asserções dos campos aparecem, entre outros, nos testes de exportação, soma diária e v3; são usos de teste, não novos derivadores de produção. (`app/folha/tests/test_export_invariante_he.py:27`, `app/folha/tests/test_export_59a.py:14`, `app/folha/tests/test_export_dsr_reflexo.py:25`, `app/ponto/tests/test_dia_pago_soma.py:231`, `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:69-74`, `app/ponto/tests/test_v3_periodos_da_celula.py:116-176`.)

**5. Testes do intervalo pós05 com prorrogação** [nao verificado]

**Existe o caso exato, mas ele exige a correção: não fixa 9 horas defeituosas como resultado esperado.** O módulo declara ser vermelho de propósito e sem `expectedFailure`; monta turno 22:00–07:00, pausa 05:30–06:30, flag `True` e fator 1. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:4-6`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:40-47`.)

| Teste | O que exige | [nao verificado]
|---|---|
| `test_P5_HE100_noturna_pela_regra` | HE100 = 8 h e HE100 noturna = 8 h. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:49-54`.) |
| `test_P5_PROPRIEDADE_parte_nao_passa_do_todo` | HE100 noturna ≤ HE100. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:56-59`.) |
| `test_P5_noturnas_pela_regra` | `horas_noturnas` = 8 h. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61-63`.) |

O docstring registra 9 h nas duas parcelas como comportamento defeituoso atual; essa descrição é coerente com a avaliação por pedaço presente no código. Resultado de execução dos testes nesta auditoria: [nao verificado]. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:17-24`, `app/ponto/motor_calculo_v2.py:109-111`, `app/ponto/motor_calculo_v2.py:145`.)

Há guardas relacionadas, com geometria diferente:

- Pausa **04:30–05:30**, atravessando as 05:00: C3 exige 8 h e verifica que a porta entrega par inteiro e pausas. (`app/ponto/tests/test_v3_periodos_da_celula.py:123-148`.)
- F7f fixa dois pares e 390 minutos; F7g, marcado `expectedFailure`, pede um par e 480 minutos. Outra guarda compara explicitamente par inteiro = 480 versus segmentos = 390. (`app/ponto/tests/test_o280_bateria_da_lei.py:1562-1616`, `app/ponto/tests/test_o280_bateria_da_lei.py:4132-4171`.)
- As guardas de desconto da pausa 00:00–01:00 e de prorrogação do turno sem pausa são testes separados; não cobrem a pausa inteiramente pós05. (`app/ponto/tests/test_s5b_regra_noturnas.py:75-93`.)
- As guardas de HE descontam pausas 23:30–00:30 e 23:00–23:30; também não cobrem a pausa inteiramente pós05. (`app/ponto/tests/test_noturno_desconta_intervalo.py:245-269`.)

Quantidade de colaboradores atingidos, valores persistidos, impacto financeiro e estado efetivo de produção: **[nao verificado]**.

## 31/10 19:41 · sessao 9af78461416a · - · - · origem=Ronald (tmux)

Censo complementar de grade e consumidores indiretos, somente pelo código:

| Sítio | Universo / EC do dia / calendário de outro vínculo | Chamador e decisão |
|---|---|---|
| `app/escala/utils.py::_aplicar_folgas_foto:1103` | Carrega FolgaDia de toda a pessoa por `folgas_lancadas_do_periodo` em `:1119`; julga por `folga_lancada_do_dia(..., esc_do_dia(d))` em `:1124`. Portanto calendário de outro EC é recusado; manual de outro EC é aceito pela autoridade delegada. | `montar_grade_prevista:770` passa EC fixo recebido; `montar_grade_prevista_periodo_por_turno:1794` passa mapa do vínculo do dia. Marca `tipo_dia='folga'`, apaga células e órfãs em `:1126-1128`. |
| `app/escala/utils.py::_montar_grade_prevista_periodo_legacy:955` | Todas escalas da pessoa em `:966`; EC por `vinculo_do_dia` em `:973`, com células em `:970`; segmenta por EC em `:975-982`. | Chama builder simples em `:995`, retém só datas do segmento em `:999`; wrapper seleciona legacy em `:2038`. Herda overlay acima. |
| `app/escala/utils.py::montar_grade_prevista_periodo_por_turno:1229` | Todas escalas da pessoa em `:1248`; células carregadas em `:1260`; EC por `vinculo_do_dia:1263`, mapa em `:1268`. | Wrapper `montar_grade_prevista_periodo:2036`; overlay em `:1794`. Wrapper é chamado por cartório em `app/ponto/services/cartorio.py:946`, reconciliação em `app/ponto/management/commands/reconciliar_grade.py:118` e contrato de grade em `app/escala/utils.py:2286`. |
| `app/colaboradores/services/calendario.py::contexto_calendario:252` | Foto para previsão carrega só EC disponível em `:606`; fatos A2 usam `fatos_do_periodo` em `:617-619`, **sem** sobrescrever `folgas_lancadas`. Otimização passa `[ec]` se cobre as duas pontas, pelo juiz de cobertura em `:614-619`. Origem/vínculo válido são julgados pela precedência, com universo pessoa, conforme ligação em `:617-623`. | `folga_humana_ou_feriado` em `:817-820` alimenta `_classificar:839` e `trabalho_do_dia:840`; esta última força False para folga humana em `:103-104`. Chamadores: view `app/colaboradores/views.py:1946`, drawer `app/colaboradores/services/drawer.py:121`, relatório `app/relatorios/furos.py:184`, portas locais de status/resolução em `calendario.py:211,223,234`. |
| `app/ponto/status_dia.py::fatos_do_status:124` | Consulta A2 pela pessoa/data somente com veredito `indefinida`, em `:135-139`; herda regra de vínculo/origem. | Cartório `app/ponto/services/cartorio.py:749,1062,1583`; resultado `folga_humana` entra nos fatos do status em `status_dia.py:149`. |
| `app/ponto/services/cartorio.py::aplicar_precedencia_na_celula:1681` | Consulta `fatos_do_dia(cel.colaborador,cel.data)` em `:1694`, herdando universo e EC da precedência. | `julgar_celula:1743`; carimba faixa, tipo, suprimidos e previsto na ata em `:1695-1702`. |
| `app/ponto/services/espelho.py::montar_dias`, chamada A2 em `:659` | Pessoa/período, sem carga própria de FolgaDia/origem; herda regra da precedência. | Transporta fato em `:881`; `aplicar_palavra_do_dia` o lê em `:342`, chamada ao final em `:1295`. Move palavra/cor do espelho e cartão; PDF delega desenho e palavra à tela, ligação documentada em `app/relatorios/pdf_espelho.py:371-378`. |
| `app/folha/services/sem_furo.py::dias_com_furo:16` | Pessoa, intervalo mínimo/máximo das células com lâmpada apagada; A2 em `:25`; calendário de outro EC recusado por delegação. | Só mantém apagados cujo `tipo_do_dia == trabalho` em `:26`. `colabs_sem_furo:51` usa a resposta no contador; command `app/folha/management/commands/colabs_sem_furo.py:13`. |
| `app/ferias/situacao.py::_dia_de_descanso:72` | Consulta A2 em `:94`, mas lê `fatos.folga_lancada` em `:97`, preservando repouso mesmo com férias vencendo a precedência; também lê `era_dia_previsto` em `:99`. | `_vespera:110`, usada em `:246,320`; move aviso de início de férias junto de repouso/feriado, descrito em `:102-105`. |
| `app/escala/services/semana_do_posto.py::_fato_do_dia:168` | Pessoa/data pela A2 em `:177`; herda regra do EC e da origem. | `grade_do_posto:151-158` classifica previsto sem batida como explicado ou buraco. Grade chamada na API `app/api/views_mensageria.py:2106`, propostas em `semana_do_posto.py:190,234`. |
| `app/chamados/services/feriado_reavaliar.py::espurios:23` | Pessoa e dia do chamado pelo juiz `dia_do_fato:30`; A2 em `:33`. Exige previsão conhecida em `:34`, faixa feriado/folga em `:36` e premissa morta em `:38`. | `app/chamados/models.py:1253`, command `app/ponto/management/commands/reavaliar_ausencias_feriado.py:24`, serviço `reavaliar_dias:82`; move reconciliação em `feriado_reavaliar.py:47`, anotação em `:49-55`. Evento da porta da célula chama `reavaliar_dias` em `app/ponto/portas/celula.py:432`. |
| `app/ponto/services/dia_do_colab.py::dia_do_colab:6` | Pessoa/data pela A2 em `:14`; herda regra. | Ficha em `app/colaboradores/services/ficha.py:280`, API em `app/api/views_mensageria.py:1963`; publica fato vencedor em `dia_do_colab.py:84-89` e contradição com célula em `:130-136`. |
| `app/ponto/services/abriu_nao_bateu.py::com_nomes:94` | Pessoa/hoje pela A2 em `:123`; herda regra. | Command `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:30`; exclui quem não tem faixa trabalho da lista em `abriu_nao_bateu.py:123-124`. |
| `app/ponto/services/deslogados.py::apurar`, consulta em `:166` | Pessoa/dias candidatos pela A2; remove dias cujo tipo não é trabalho/None em `:166-168`. | `app/ponto/management/commands/lavrar_deslogados.py:38` e pautas em `app/ponto/services/pautas_dos_deslogados.py:62`; move corrida de plantões e lista de deslogados em `deslogados.py:168-190`. |
| `app/ponto/services/faltas_de_hoje.py::faltas_de_hoje_sem_cobranca:13` | Pessoa/hoje pela A2 em `:61`; seleção inicial usa EC ativo recente em `:35-40`, antes da pergunta A2. | Command `app/ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`; A2 exclui dias sem trabalho em `faltas_de_hoje.py:61-62`, move contador em `:67-70`. |
| `app/ponto/management/commands/detectar_ausencias.py`, consulta em `:616` | Pessoa/data pela A2; trecho testa exclusivamente faixa feriado em `:616`. | Não é um novo filtro de FolgaDia: lê resultado amplo da A2, mas o ramo mostrado só suprime feriado, conforme `:610-616`. |

Todos os chamadores de `folgas_do_periodo` fora models/precedência encontrados:

| Local | Seleção e decisão |
|---|---|
| `app/escala/utils.py:1384` | Todos ECs da pessoa em `rows`; previsão por EC do dia, dado pelo juiz em `:1263,1268`. Overlay FolgaDia usa autoridade separada em `:1794`. |
| `app/colaboradores/services/calendario.py:606` | Foto só do `ec` disponível; `eh_dia_trabalho` em `:786`; fato humano amplo é consultado separadamente em `:617`. |
| `app/api/views.py::api_espelho_v2:1609` | `esc` vem da autoridade do espelho em `:1371`; `eh_dia_trabalho:1627`; move falta/previsto/neutro em `:1636-1655`. |
| `app/core/services/painel_op.py::montar_painel_operacional:105` | ECs do mapa `escalas:74`; `_previsto_agora:109` pergunta `ec.eh_dia_trabalho:118`; move previsão de turno agora em `:123`. |
| `app/ponto/management/commands/censo_fase_12x36.py:176` | ECs dos segmentos escolhidos por `vinculo_do_dia:161`; previsão por segmento em `:179`; paridade com realizado em `:184`. |
| `app/ponto/management/commands/diff_espelho_alimentacao.py:101` | ECs de `escalas_do_periodo:96`; compara previstas e motores com alimentações distintas em `:103-122`. |
| `app/relatorios/pdf_espelho.py::_coletar_dados_espelho:361` | Escalas do período e geradoras fora de vigência adicionadas em `:354-359`; alimenta `previstas_do_periodo:368-370` e motor por fatia em `:383`. |
| `app/ponto/services/espelho.py::autoridade_do_periodo:1071` | Escalas da janela mais geradoras em `:1057-1059`; EC do fim pelo juiz em `:1069`; alimenta previstas em `:1073`. |

Esses mapas preservam chave EC/data e não examinam `calendario_origem`; a decisão é delegada ao modelo e a `previstas_do_periodo` nas chamadas indicadas acima. Não estabelecem por si a regra “manual da pessoa / calendário do EC do dia”. (`app/escala/utils.py:1384`; `app/colaboradores/services/calendario.py:606`; `app/api/views.py:1609`; `app/core/services/painel_op.py:105`; `app/ponto/management/commands/censo_fase_12x36.py:176`; `app/ponto/management/commands/diff_espelho_alimentacao.py:101`; `app/relatorios/pdf_espelho.py:361`; `app/ponto/services/espelho.py:1071`.)

Chamadores diretos de `eh_dia_trabalho_calculado` fora models/precedência:

- `app/ponto/portas/celula.py::regenerar_celulas_vinculo:243`: EC recebido; monta DNA em `:244`, cria/atualiza célula em `:247,251`.
- `app/ponto/portas/celula.py::gerar_celulas_periodo:590`: EC ativa em `:568`, cobertura pelo juiz em `:588`; cria DNA/célula em `:593-596`, preserva editada em `:598`.
- `app/ponto/management/commands/censo_col900.py:115`: EC do dia por `vinculo_do_dia:111`, todas escalas em `:108`; compara DNA vivo com congelado em `:118-121`.
- `app/ponto/management/commands/tripwire_celulas.py::medir_stale:27`: EC do mapa por pessoa vindo de `ativa=True`/pessoa ativa em `:17-19`; não consulta geradora para selecionar esse EC; compara trabalha e DNA em `:29-33`.
- `app/colaboradores/management/commands/corrigir_escala_retroativa.py:120`: projeta EC de mesmo PK em `:107`, tipo novo em `:108` e âncora opcional em `:110`; compara previsão em `:118,128-130`.

Testes específicos que fixam os comportamentos principais: [nao verificado]

- Grade: calendário alheio recusado, manual antigo aceito, calendário próprio aceito, candidata alheia de PK menor não esconde própria, manual fora da janela: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70,75,80,85,91`.
- Overlay básico FolgaDia: `app/escala/tests/test_grade_folga_overlay.py:37,43`.
- Correção aplicável a competências antigas e simulação, mantendo manual: `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:74,79,84,90`.
- Rejulgar ata antiga com força, preservar fechamento/TXT, ausência de rejulgamento automático sem força: `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:101,106,121`.
- Calendário: manual do antigo aceito e `eh_trabalho=False`, calendário antigo recusado, calendário ativo recusado quando geradora é outra: `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:56,66,102`.
- Status lavrado com previsão indefinida: FolgaDia manual produz status folga: `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:82`.
- Folha sem furo: mesma lâmpada apagada não conta em folga/ausência; contador: `app/folha/tests/test_sem_furo.py:67,75`.
- Foto carregada em lote cobre mês completo, mesmo folga fora da janela pedida: `app/escala/tests/test_selo_alimentacao_dias.py:96`; custo constante: `:73`.
- Regeneração de célula por FolgaDia e idempotência: `app/ponto/tests/test_porta_celula.py:46`; recorte só do dia: `app/ponto/tests/test_porta_celula_dia.py:79,85`; mesma fixture regenerada por vínculo toca mais dias: `:99`.
- Painel: 12x36 de folga não é previsto, DNA vence template, turno iniciado ontem permanece previsto: `app/core/tests/test_painel_previsto_celula.py:58,70,79`.
- Palavra única tela/cartão/calendário/template: `app/relatorios/tests/test_palavra_do_dia.py:205,228,307`; esses testes não fixam especificamente calendário de outro EC [nao verificado].
- Semana do posto: atestado remove buraco em `app/escala/tests/test_semana_do_posto.py:125`; teste específico de FolgaDia/origem nesse consumidor [nao verificado].
- Corrida de deslogados: cobertura não rompe nem estica em `app/ponto/tests/test_lista_viva_dos_deslogados.py:38`; teste específico de calendário de outro EC [nao verificado].
- Vigia de faltas de hoje e contador: `app/ponto/tests/test_vigia_falta_de_hoje.py:62,68,74`; teste específico de calendário de outro EC [nao verificado].

Templates/tags que consultem diretamente FolgaDia ou `calendario_origem`: [nao verificado]. O template de grade é fixado como consumidor de palavra pronta, pelo teste `app/relatorios/tests/test_palavra_do_dia.py:307`.

## 31/10 19:40 · sessao f567806d1e0f · - · - · origem=Ronald (tmux)

Censo de campos e v3, somente por leitura:

- **Definições dos campos persistidos:** `DiaPago.horas_noturnas` e `.horas_extras_100_noturna`: `app/ponto/models.py:327`, `app/ponto/models.py:332`; correspondentes do `FechamentoMensal`: `app/ponto/models.py:413`, `app/ponto/models.py:419`.
- **Origem no motor:** `PeriodoCalculo.horas_noturnas` converte minutos legais em horas: `app/ponto/motor_calculo_v2.py:210`; `ResultadoMes.horas_noturnas` soma `periodos + periodos_ft`: `app/ponto/motor_calculo_v2.py:271`; `ResultadoMes.horas_extra_100_noturna` soma apenas `periodos`: `app/ponto/motor_calculo_v2.py:292`.
- **Escritor mensal canônico:** atribui ambos do `resultado`: `app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:899`; persiste com `queryset.update`: `app/ponto/services/fechamento.py:1131`, `app/ponto/services/fechamento.py:1137`.
- **Outro escritor mensal genérico:** `restaurar_fechamento` aceita campos existentes do modelo e persiste `**_upd`, portanto pode escrever ambos: `app/ponto/services/fechamento.py:1342`, `app/ponto/services/fechamento.py:1348`, `app/ponto/services/fechamento.py:1360`.

**DiaPago/v3**

- Ambos pertencem a `CAMPOS_DIA`: `app/ponto/services/dia_pago.py:34`.
- `lavrar` soma noturnas e HE100 noturna dos períodos comuns em `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:184`; folgas acrescentam somente noturnas na rotina compartilhada em `app/ponto/services/dia_pago.py:224`.
- Quando recebe rubricas v3, substitui genericamente os campos declarados em `valores`, exceto `do_motor`: `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:327`; monta e grava os objetos em `app/ponto/services/dia_pago.py:343`, `app/ponto/services/dia_pago.py:357`.
- `retratar_exportada` pode criar linhas e ajuste com ambos zerados por compreensão sobre `CAMPOS`: `app/ponto/services/dia_pago.py:470`, `app/ponto/services/dia_pago.py:476`, `app/ponto/services/dia_pago.py:508`, `app/ponto/services/dia_pago.py:513`; persiste em `app/ponto/services/dia_pago.py:543`.
- v3 escreve `horas_noturnas` no ramo folga em `app/ponto/calculador/regras.py:739` e no ramo comum em `app/ponto/calculador/regras.py:912`; escreve HE100 noturna em `app/ponto/calculador/regras.py:898`.
- Os dados v3 chegam de `rubricas_por_dia`; flags CCT por dia são extraídos em `app/ponto/calculador/alimentacao.py:572`, `app/ponto/calculador/alimentacao.py:573` e passados em `app/ponto/calculador/alimentacao.py:593`, `app/ponto/calculador/alimentacao.py:596`.
- Na integração do fechamento, o motor é lavrado como `versao='motor'`: `app/ponto/services/fechamento.py:1170`, `app/ponto/services/fechamento.py:1195`; rubricas v3 são calculadas e lavradas separadamente como `versao='oraculo'`: `app/ponto/services/fechamento.py:1239`, `app/ponto/services/fechamento.py:1245`, `app/ponto/services/fechamento.py:1256`, `app/ponto/services/fechamento.py:1262`.

**_ContextoNoturno**

- Converte o cadastro em `bool` e empresta o método do motor, por alias de atributo de classe: `app/ponto/calculador/regras.py:60`, `app/ponto/calculador/regras.py:63`.
- `noturnas_do_dia` passa o mesmo flag ao cálculo do par e ao contexto do desconto: `app/ponto/calculador/regras.py:80`, `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:86`.
- O desconto executa o método emprestado; a função final calcula cada intervalo recortado com seu próprio início/fim, sem transportar uma janela noturna previamente decidida pelo turno: `app/ponto/motor_calculo_v2.py:145`, `app/ponto/motor_calculo_v2.py:1865`.
- HE v3 chama o alias `_he_not` por período, com as pontas e intervalos desse período: `app/ponto/calculador/regras.py:868`, `app/ponto/calculador/regras.py:880`, `app/ponto/calculador/regras.py:886`.

**Leitores de produção, inclusive dinâmicos**

- `soma_da_competencia` agrega ambos via `CAMPOS`: `app/ponto/services/dia_pago.py:560`; `soma_do_periodo` via `CAMPOS_DIA`: `app/ponto/services/dia_pago.py:782`, `app/ponto/services/dia_pago.py:785`.
- `pago_do_dia` lê ambos genericamente para verificar existência de dinheiro: `app/ponto/services/dia_pago.py:859`, `app/ponto/services/dia_pago.py:890`.
- `rubricas_do_dia` lê noturnas para tela/papel: `app/ponto/services/dia_pago.py:922`; consumidores: `app/ponto/services/espelho.py:824`, `app/colaboradores/services/calendario.py:785`, PDF por chave `noturnas` em `app/relatorios/pdf_espelho.py:840`, `app/relatorios/pdf_espelho.py:906`.
- `folha/porta_export` compara todos os campos retornados pela soma com `getattr` do fechamento, abrangendo ambos: `app/folha/porta_export.py:451`, `app/folha/porta_export.py:452`.
- `folha/export` lê `horas_noturnas` e emite `adicional_noturno`: `app/folha/export.py:436`; não emite uma linha própria de HE100 noturna no retorno: `app/folha/export.py:428` a `:450`. A justificativa dos subsets está em `app/folha/export.py:413`. O TXT resolve o código pelo cadastro de rubricas, não por `25` cravado: `app/folha/export.py:694`, `app/folha/export.py:716`, `app/folha/export.py:719`; “rubrica 25” está em comentário: `app/folha/export.py:419`. Código cadastrado real de produção: [nao verificado].
- `eventos_do_fechamento` é consumido pelo TXT em `app/folha/export.py:711` e pela homologação em `app/folha/export.py:976`.
- Relatórios: CSV mensal `app/relatorios/views.py:94`, soma mensal `app/relatorios/views.py:108`, extrato `app/relatorios/views.py:524`, cartão pela soma diária `app/relatorios/cartao_pela_celula.py:176`, espelho pela property do período `app/ponto/services/espelho.py:99`.
- Templates leem noturnas: `app/templates/relatorios/horas_mes.html:68`, `app/templates/ponto/_fech_linha.html:31`, `app/templates/ponto/partials/painel_fechamento.html:33`.
- Ranking lê efetivamente noturnas em `app/folha/services/ranking_he.py:225`. A tupla `CAMPOS` inclui ambos em `app/folha/services/ranking_he.py:33`, mas não tem uso naquele módulo.
- Simulação de folha lê ambos via `CAMPOS_HORAS/getattr`: `app/folha/management/commands/simular_folha.py:59`, `app/folha/management/commands/simular_folha.py:84`.

**Comandos e scripts: não omitir do censo**

- `aplicar_09_he_intra`: lê ambos no snapshot em `app/ponto/management/commands/aplicar_09_he_intra.py:128`; HE100 noturna é alvo em `:39`–`:40`, e recebe gravado + delta pela porta em `:141`–`:152`; noturnas está só no snapshot em `:43`.
- `aplicar_09_corte_b`: lê ambos via `CAMPOS` em `app/ponto/management/commands/aplicar_09_corte_b.py:184`; recalcula em `:212`; pode restaurar ambos dentre todos os campos em `:265`–`:268`.
- `desvio_o68b`: lê snapshot em `app/ponto/management/commands/desvio_o68b.py:149`, `:202`; restaura todos os campos selecionados em `:195`–`:198`.
- `carimbo_gravado`: introspecção dos campos em `app/ponto/management/commands/carimbo_gravado.py:36`, leitura em `:54`; `lavrar_dias_pagos` faz hash por introspecção em `app/ponto/management/commands/lavrar_dias_pagos.py:70`, `:76`.
- `celula_veredito_velho` lê noturnas via tupla em `app/ponto/management/commands/celula_veredito_velho.py:70`, `:85`, `:155`.
- `diff_reclassificar_partido` lê noturnas em tupla `app/ponto/management/commands/diff_reclassificar_partido.py:67` e getters em `:146`, `:156`, `:212`; sua restauração explícita é só banco em `:224`–`:225`.
- `diff_frota` lê campos recebidos via getter em `app/ponto/services/diff_frota.py:63`; recebe a lista canônica que inclui ambos dos comandos `app/ponto/management/commands/diff_col900.py:45`, `app/ponto/management/commands/diff_passivo_s84.py:50`, `app/ponto/management/commands/diff_janela_he_total.py:101`, `:110`.
- `recalcular_fechamento` agrega ambos pela lista canônica em `app/ponto/management/commands/recalcular_fechamento.py:54`, `:67`.
- `regen_exportada` lê ambos por hash/snapshot em `app/ponto/portas/regen_exportada.py:72`, `:84`.
- **Escritores diretos fora da porta:** `bin/restore_relavratura_10_2026.py:79`–`:81` e `bin/restore_o263_comp10.py:48`–`:50` fazem `setattr/save` com `VALOR` derivado de `CAMPOS`, abrangendo ambos; recriam DiaPago em `bin/restore_relavratura_10_2026.py:86`, `bin/restore_o263_comp10.py:56`.
- Snapshots bin leem ambos: `bin/snapshot_relavratura_10_2026.py:31`, `:36`–`:37`; `bin/snapshot_o263_comp10.py:57`–`:58`, `:63`; diff pontual em `bin/diff_o118_col221.py:23`, `:27`.
- O modo leitura do fechamento também serializa ambos pela lista canônica: `app/ponto/services/fechamento.py:1102`–`:1104`.

Testes que usam os campos podem ser agrupados como fixtures e assertions; evidências úteis: `app/folha/tests/test_export_invariante_he.py:27`, `app/folha/tests/test_export_59a.py:14`, `app/folha/tests/test_export_dsr_reflexo.py:25`, `app/folha/tests/test_sm1_monster_export.py:92`, `app/ponto/tests/test_dia_pago_soma.py:231`, `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:69`, `:74`, `:107`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52`, `:58`, `:62`, `app/ponto/tests/test_v3_periodos_da_celula.py:116`, `:134`, `:170`, `:176`.

## 31/10 19:40 · sessao f9695340e72e · - · - · origem=Ronald (tmux)

Censo complementar dos leitores diretos, somente leitura:

| Sítio e filtro | Universo / ec / calendário de outro vínculo | Chamador e decisão | Testes | [nao verificado]
|---|---|---|---|
| `app/escala/alimentacao.py:49` `folgas_do_periodo`; query `:66` | Lista de vínculos fornecida; agrupa datas por `ec_id`; arredonda início/fim para meses inteiros (`:60`). Não escolhe ec do dia nem lê origem: calendário de outro vínculo fica no balde daquele outro vínculo. | Alimenta cálculo do vínculo: `app/ponto/precedencia.py:333`; grade `app/escala/utils.py:1384`; calendário `app/colaboradores/services/calendario.py:606`; painel `app/core/services/painel_op.py:105`; PDF `app/relatorios/pdf_espelho.py:361`; censos `app/ponto/management/commands/censo_fase_12x36.py:176` e `diff_espelho_alimentacao.py:101`. | `app/escala/tests/test_selo_alimentacao_dias.py:73` custo constante e `:96` mês inteiro; contrato lista de vínculos em `app/escala/tests/test_carregadores_contrato.py:6`. |
| `app/escala/models.py:1212` `defeito_folga_nao_definida`; query `:1220` | Próprio `self` e mês da data. Para 6x1 sem `folga_dia_semana`, ausência de qualquer FD no mês acusa defeito. Não lê origem; outro vínculo não serve. | Plano `app/escala/views_plano_folgas.py:89`; limbo `app/chamados/services/limbo_folgas.py:199`, `:281`; detector `app/ponto/management/commands/detectar_ausencias.py:144`, `:187`; supra juiz `app/ponto/supra_juiz.py:200`; faltas hoje `app/ponto/services/faltas_de_hoje.py:39`; régua `app/escala/regua_defesa.py:145`, `:176`; diagnóstico `app/ponto/services/diagnostico_escala.py:104`. Move defeito/cobrança de cadastro, visibilidade e guardas. | `app/escala/tests/test_t1_ligado_por_padrao.py:34`, `:39`, `:45`, `:55`; `app/escala/tests/test_plano_folgas_filtro.py:53`, `:58`, `:61`. |
| `app/escala/models.py:1223` `eh_dia_trabalho_calculado`: foto mensal `:1257`, existência no dia `:1281`, anterior `:1327`, posterior `:1330`; ou mapa alimentado `:1251` | Próprio vínculo (`self`); **nenhuma dessas leituras distingue manual/calendário**. Outro vínculo não entra. Foto no dia => `False` (`:1280`); fora da foto usa ciclo se ele sabe (`:1290`), senão paridade da foto mais próxima (`:1322`); sem foto mensal pode usar âncora (`:1260`). | `eh_dia_trabalho` retorna célula se existe e só cai aqui sem célula: `app/escala/models.py:1048`, `:1071`. Geradora/regeneradora chama diretamente em `app/ponto/portas/celula.py:243`, `:590` e grava `trabalha`/DNA (`:244`, `:594`); precedência em `app/ponto/precedencia.py:368`; projeção retroativa `app/colaboradores/management/commands/corrigir_escala_retroativa.py:120`; sentinelas `app/ponto/management/commands/tripwire_celulas.py:27`, `censo_col900.py:115`. Move célula, grade e decisões downstream. | `app/escala/tests/test_foto_vence_ancora.py:34`, `:38`, `:44`, `:52`; `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:50`, `:60`, `:67`, `:73`; continuação da foto parcial em `app/escala/tests/test_o37_foto_parcial.py` [linhas de métodos não verificadas]; alimentação acima. |
| `app/escala/services/fase_conflitante.py:61` `medir`; query `:74`; derivador `_foto_diz` `:51` | Itera **cada ec ativo** (`:71`) e todas suas FD, sem recorte de data na query. Não seleciona ec por dia/vigência; não lê origem. Foto de outro ec não entra. `_foto_diz` marca data presente como folga e extrapola paridade a partir da mais próxima (`:55`). | `app/escala/management/commands/lavrar_fase_conflitante.py:31` mede e `:35` grava snapshot; cron `app/config/crons.py:479`; API lê snapshot em `app/api/views_mensageria.py:632`, `:643`. Conta conflito com âncora; não altera veredito (`app/escala/services/fase_conflitante.py:80`). | `app/escala/tests/test_fase_conflitante.py:60`, `:72`, `:80`, `:84`, `:88`, `:93`. |
| `app/escala/folgas.py:83` `divergencia_foto_x_calendario`; query `:101`, origem `:104` | Próprio ec + mês; separa FD manual e calendário; compara com calendário **atual** de `ec` (`:99`), sem exigir que a origem de cada FD seja esse calendário atual. Outro vínculo não entra. Não escolhe ec do dia. | Contador `foto_que_o_calendario_de_hoje_nao_manda` em `app/escala/folgas.py:133`, chamada `:143`; comando `app/escala/management/commands/lavrar_foto_folgas.py:113`, `:167`. `falta` permite lavra; `sobra` impede reconciliar o mês (`:123`, `:168`). | `app/escala/tests/test_foto_folga_continua.py:136` sobra bloqueia; `:158` vazio/contador; `:179` manual preservado. |
| `app/escala/folgas.py:150` `gerar_foto_folgas`; leitura manuais `:165`, filtro origem `:167`; filtro calendário `:169`/`:171` | Próprio ec + mês; manual preservada; FD com origem não nula é reconciliada com calendário atual. Outro vínculo não entra; ec é argumento. | `app/escala/views.py:414`, `:415`; `app/colaboradores/views.py:245`, `:246`; `app/colaboradores/services/vinculo.py:163`, `:164`; lavra `app/escala/management/commands/lavrar_foto_folgas.py:170`. Cria foto e chama re-lavra `app/escala/folgas.py:192`. | `app/escala/tests/test_gerador_folgas.py:39`, `:47`, `:54`, `:63`; `app/escala/tests/test_torneira_folgas.py:87`; `app/escala/tests/test_foto_folga_continua.py:179`; lista de chamadores é fixada por `:260`. |
| `app/escala/folgas.py:228` `lancar_folga`; query `:254`/origem `:255` | Próprio ec + dia; calendário no próprio vínculo veta lançamento manual. FD de outro vínculo não é procurada. Ec fornecido pelo chamador. | Plano `app/escala/views_plano_folgas.py:131`; declaração `app/ponto/services/declarar_dia.py:183`; veto precedência em `app/escala/folgas.py:258`, foto `:259`, re-lavra `:261`. | `app/escala/tests/test_torneira_folgas.py:68`; `app/escala/tests/test_bug136_plano_em_lote_relavra.py:114` para veto equivalente no helper; testes da porta declarar com outro agente. |
| `app/escala/folgas.py:267` `_foto_do_dia`; query `:274`/origem `:275` | Próprio ec + dia; origem não nula veta alteração/remoção; outro vínculo não entra. | `alterar_folga` `app/escala/folgas.py:302` e lote `:334`; alterar chama re-lavra `:303`; lote recusa dias anteriores à competência `:330`, calendário `:335`, re-lavra `:340`. Portas views `app/escala/views_plano_folgas.py:136`, `:185`. | `app/escala/tests/test_bug136_plano_em_lote_relavra.py:68`, `:75`, `:84`, `:93`, `:105`, `:114`, `:124`. |
| `app/escala/folgas.py:392` `reparentar_folga`; origem `:405`; ocupação destino `:418` | Folga individual; calendário retorna `'calendario'` sem migrar; manual do próprio ec é mantida se cobre o dia (`:408`), senão destino vem de `vinculo_do_dia_carregado` (`:414`). Se destino já tem FD de **qualquer origem**, não migra (`:418`). | `app/escala/folgas.py:459` e comando `app/escala/management/commands/reparentar_folgas.py:34`. Move titularidade e re-lavra só competência corrente em diante (`app/escala/folgas.py:435`, `:441`). | `app/escala/tests/test_folga_segue_a_pessoa.py:53`, `:66`, `:80`, `:92`, `:104`, `:113`. |
| `app/escala/folgas.py:447` `reparentar_folgas_do_colaborador`; query `:453` | Pessoa inteira, **somente manuais** (`:454`); filtra as cujo vínculo não cobre data (`:457`); destino delegado acima. Calendário outro vínculo excluído. | Signal `app/escala/signals.py:190`, `:191` após mudança do vínculo. Mesmo efeito da porta individual. | `app/escala/tests/test_folga_segue_a_pessoa.py:53`; casos individuais acima. |
| `app/escala/management/commands/reparentar_folgas.py:22` `handle`; query `:28` | Todas FD manuais, ordenadas por pessoa/data; não requer ec ativo; ignora as cujo ec cobre (`:31`); destino pela porta `:34`. | Entrada management command (`:16`); flag decide dry/apply `:26`; migração/re-lavra pela porta. | Teste específico do command [nao verificado]; porta fixada acima. |
| `app/escala/views_plano_folgas.py:51` `_grid_ctx`; query `:60` | Vínculos **ativos** da empresa/posto (`:55`), mês pedido; mapa por ec/dia (`:63`). Não escolhe vínculo por dia nem vigência. Não lê origem, outro vínculo não pinta linha deste ec. FD presente tem prioridade sobre previsão (`:70`); sem FD usa `ec.eh_dia_trabalho` (`:76`). | View `plano_folgas` chama em `app/escala/views_plano_folgas.py:46`; URL `app/escala/urls.py:8`. Pinta grid/defeito de cadastro. | `app/escala/tests/test_plano_folgas_filtro.py:53`, `:58`, `:61`, `:64`; `app/escala/tests/test_folga_cadastro.py:26`. |
| `app/escala/views_plano_folgas.py:98` `plano_folgas_toggle`; query `:112`/origem `:113` | Ec ativo pelo ID do POST (`:109`), dia do POST; calendário próprio gera 409. Não verifica ec do dia; outro vínculo não entra. | URL `app/escala/urls.py:9`; chama lançar/alterar `app/escala/views_plano_folgas.py:131`, `:136`. Veto ou gravação/re-lavra. | `app/escala/tests/test_bug136_plano_em_lote_relavra.py:124`; `app/escala/tests/test_torneira_folgas.py:68`. |
| `app/ponto/services/diagnostico_escala.py:81` `template_do_vinculo`; query `:102` | Próprio ec + mês; origem ignorada; outro vínculo não conta. **Ec aqui é `atual = vinculos[0]`** (`:182`), não escolhido por cada dia. | `diagnostico_escala` `app/ponto/services/diagnostico_escala.py:193`; sem folga cadastrada gera ação Plano (`:194`); API `app/api/views_mensageria.py:1995`. Diagnóstico/contador. | `app/api/tests/test_diagnostico_template.py:36`, `:45`; teste específico de count FD [nao verificado]. |
| `app/relatorios/furos.py:78` `escalas_em_limbo`; query `:112` | Cada ec ativo sem fim, colaborador ativo (`:86`), após dois dias sem previsão (`:93`); FD do mês atual e ec próprio. Não lê origem/outro vínculo; ec por queryset, não juiz do dia. | `app/relatorios/views.py:1552`; inclui peça explicativa “sem foto FolgaDia do mes” (`app/relatorios/furos.py:115`) na fila de cadastro. | `app/relatorios/tests/test_selo_ancora_do_vinculo.py:51` fixa diagnóstico da âncora; teste específico origem/FD [nao verificado]. |

Outros auxiliares relevantes da lavra: universo ativo/calendário/mês vem de `app/escala/folgas.py:59`, usando carregador de vínculos em `:78`; `app/escala/management/commands/lavrar_foto_folgas.py:101` mede falta/sobra, e `:144` handle reconsulta antes de gravar (`:167`). Testes em `app/escala/tests/test_foto_folga_continua.py:80`, `:99`, `:125`, `:136`, `:158`, `:199`, `:208`.

Atenção à escolha de ec no diagnóstico: `app/ponto/services/diagnostico_escala.py:49` `_vigente` pega o **primeiro vínculo que cobre a data** usando `vinculo_cobre_a_data` (`:53`), chamado em `:150`; não chama `vinculo_do_dia` nem toma a geradora da célula nessa escolha. Isso afeta previsão diagnóstica em `:152`, não a contagem mensal que usa `vinculos[0]` (`:182`, `:193`).

Não identifiquei uso operacional de reverse `folgas_dia` além da definição em `app/escala/models.py:1554`; ausência total além dos sítios pesquisados [nao verificado].

## 31/10 19:40 · sessao b38a17c024b7 · - · - · origem=Ronald (tmux)

Censo somente por leitura; nenhum teste executado. [nao verificado]

**Alvo: dois chamadores diretos, nenhum em teste ou por alias de import.** A varredura AST de todos os `.py` encontrou somente os dois `Load` abaixo; nenhuma ocorrência semântica como import, atributo, atribuição ou string exata para `getattr`. Os usos do nome em testes são texto explicativo/mensagem, em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:18` e `:54`.

| Chamador direto | `prorrogacao` | `entrada` / `saida` recebidas pelo alvo | Destino |
|---|---|---|---|
| `he_noturna`, `app/ponto/motor_calculo_v2.py:392` | Mesmo argumento `prorrogacao` de `he_noturna`, padrão `True` (`:359`, `:393`) | `inicio_extra` calculado por `_inicio_do_trecho_extra(entrada,fim,total_extra,intervalos)` em `:390`; `fim=saida` em `:385`. Portanto recorte da cauda extra, não necessariamente turno inteiro. | Subtraído de `minutos_extra_not`; dividido proporcionalmente em `not_50/not_100` e retornado (`:394`, `:398`, `:400`, `:401`). |
| `MotorBase._noturno_dos_intervalos`, `app/ponto/motor_calculo_v2.py:1865` | `self.prorrogacao_noturna_pos5h` (`:1866`) | Encaminha seus próprios parâmetros sem mudar (`:1852`, `:1865`). | Retorna desconto para `calcular_noturnas` (`:923`) ou para o método emprestado pelo calculador (`app/ponto/calculador/regras.py:63`, `:86`). |

**Bool igual não equivale a janela igual.** O alvo encaminha o mesmo bool a cada intervalo, mas recorta cada pedaço e chama `minutos_noturnos` separadamente em `app/ponto/motor_calculo_v2.py:145`. A elegibilidade efetiva da prorrogação depende do início/fim da chamada: `inter_s1_ini=max(inicio,s1_ini)` e condição `inter_s1_ini < 05h` com `fim > 05h`, em `:109-111`. Logo, um intervalo iniciado às 05:30 não herda a elegibilidade de um turno iniciado às 22h. Nenhum dos dois chamadores fornece ao alvo uma janela noturna elegível ou o contexto temporal do turno além do recorte `entrada/saida`; assinatura e encaminhamentos em `:121`, `:392-393`, `:1865-1866`.

**Cadeias de produção para o desconto normal:**

- `MotorBase.calcular_noturnas` calcula o bruto com `self.prorrogacao_noturna_pos5h` e o mesmo par e desconta `_noturno_dos_intervalos(entrada,saida,intervalos)`, em `app/ponto/motor_calculo_v2.py:921-923`. O resultado multiplica pelo fator e retorna (`:924-925`); `_escrever_noturnas` grava `PeriodoCalculo.minutos_noturnos_relogio/legais` (`:941-943`).
- Há três chamadas de `_escrever_noturnas`: Motor12x36 `:2730`, MotorTurnoPartido `:2834`, MotorComercial `:3185`. Em cada uma, `e/s` vêm de `_janela_do_par(localizar(entrada),localizar(saida))` (`:2697-2698`, `:2824-2825`, `:3149-3150`), e os intervalos são recortados por `_intervalos_na_janela(e,s,intervalos)` (`:2702`, `:2829`, `:3162`).
- MotorPlantao reutiliza explicitamente o cálculo do Motor12x36 (`:3448-3449`); Motor12x36ComEscala chama `super().calcular_periodo` (`:3460-3461`); MotorTurnoPartidoNoturno herda sem implementação adicional (`:3474-3481`); MotorIntermitente herda Motor12x36 (`:3483`, `:3491-3495`).
- Os minutos legais são lidos pela property `PeriodoCalculo.horas_noturnas` (`:210-211`) e pela soma mensal `ResultadoMes.horas_noturnas`, que inclui `periodos + periodos_ft` (`:253`, `:271`). MotorTurnoPartido também lê o ganho `legais - relogio` para excedente em régua legal (`:3063-3066`).

**Cadeias do calculador, incluindo apelidos:**

- `_ContextoNoturno` armazena `bool(prorrogacao_pos5h)` (`app/ponto/calculador/regras.py:60-61`) e empresta exatamente `MotorBase._noturno_dos_intervalos` como atributo de classe (`:63`).
- `noturnas_do_dia` percorre cada par recebido, usa o mesmo bool no bruto e no contexto do desconto (`:80-86`), limita a soma a zero e aplica fator (`:87-89`): bruto por par, desconto efetivamente por intervalo.
- `regras.do_dia` chama `noturnas_do_dia` na folga trabalhada (`:736-738`) e grava `_vf['horas_noturnas']` (`:739`); no caminho normal chama em `:908-911` e grava `val['horas_noturnas']` (`:912`).
- `periodo.noturno_no_par` chama `noturnas_do_dia` com um único par inteiro e intervalos julgados; flag `cct.prorrogacao_pos5h` (`app/ponto/calculador/periodo.py:508-510`). Seu chamador lê o par `e/s_` em `:752` e grava `p.minutos_noturnos_relogio/legais` em `:753-754`.
- Para o split de HE, `regras.py` importa `he_noturna as _he_not` em `:868` e chama esse alias para cada ponta de período em `:886-888`, passando `prorrogacao_pos5h` e intervalos daquela ponta. Soma retornos (`:889-890`) e grava `horas_extras_50_noturna` / `horas_extras_100_noturna` (`:895-898`).
- A produção V3 obtém os pares dos carimbos `p.efetivo_entrada/saida` (`app/ponto/calculador/alimentacao.py:106`), lê `intervalos_do_motor` (`:127`), monta `he_pontas` (`:135`) e pares/intervalos por dia (`:185-186`); entrega tudo a `regras.do_dia` (`:593-596`). O bool vem da CCT daquele dia (`:572-574`), cujo motor é obtido por `get_motor_cct(c,_k,tipo_escala=_te,...)` (`:448`). `_cct_do_motor` transcreve o flag do motor (`app/ponto/calculador/nucleo.py:347-351`).

**Cadeias de produção para `he_noturna`:**

- `_aplicar_he_noturna` passa `periodo.entrada/saida`, extras 50/100, `self.prorrogacao_noturna_pos5h` e `periodo.intervalos_do_motor`, em `app/ponto/motor_calculo_v2.py:952-956`. Grava `minutos_extra_100_noturna` em `:958` e `minutos_extra_50_noturna` em `:960`; soma mensal lê esses campos em `:290` e `:294`.
- Os pares gravados em `periodo.entrada/saida` são localizações das pontas originais, em `:2689`, `:2816`, `:3141`; portanto esse caminho usa pontas cruas, enquanto o adicional normal recebe `e/s` efetivos, conforme referências anteriores.
- Chamadas exatas de `_aplicar_he_noturna`: Motor12x36 por período `:2793`; MotorTurnoPartido na HE diária `:3085` e semanal `:3116`; MotorComercial por período `:3219` e na HE diária mensal `:3337`. Na agregação diária o portador é `periodos_dia[-1]` (`:3021-3025`, `:3269-3273`); na semanal é `pds[-1]` (`:3106-3110`).

**Origem do bool do motor:** explicitamente recebido ou, quando `None`, lido de `prorrogacao_pos5h_legal(tipo_ciclo)` em `app/ponto/motor_calculo_v2.py:708-712`. A função legal retorna `tipo_ciclo != '12x36'` (`app/core/regua_cct.py:182`); a fábrica ativa injeta o valor da régua cadastrada em `:458`, `:466`, e no ramo desligado/CLT usa o ciclo em `:452-457`. O valor cadastrado de qualquer colaborador real é [nao verificado].

**Testes diretos de `he_noturna`, úteis para a cadeia:** `test_s5b_split_noturno_tem_um_dono.py:111-112` percorre bools `True/False` definidos em `:108`, sem intervalos; `:122-123` usa `True`, também sem intervalos. `test_noturno_desconta_intervalo.py:249-250`, `:263-267` usa `True` com intervalos noturnos antes das 05h. O teste P5 monta explicitamente turno 22–07, intervalo 05:30–06:30 e flag `True` (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:40-47`), exige 8h nas rubricas (`:51-54`, `:62-63`) e parte ≤ todo (`:58-59`); é teste da correção desejada, não aprovação do resultado atual descrito em seu docstring (`:4-6`, `:17-24`).

## 31/10 19:40 · sessao b04758375e64 · - · - · origem=Ronald (tmux)

Censo dos sítios sob minha responsabilidade, somente por leitura:

| Sítio | Universo e EC do dia | Calendário de outro vínculo | Chamadores e decisão | Testes | [nao verificado]
|---|---|---|---|---|
| `app/ponto/services/declarar_dia.py:61` `_folga_do_dia` | Filtra exatamente `escala_colaborador=ec,data=data`; EC fornecido pelo chamador. `:69–70`. | Não entra nessa consulta; também não entra a manual de outro vínculo. `:70`. | `o_dia_ja_e_folga` em `:91`; `_recusa_do_dia` em `:108`. | Existência manual e recusa: `app/ponto/tests/test_declarar_dia.py:79`; calendário impede declaração manual: `:87`. Caso de outro vínculo nessa porta: `[nao verificado]`. |
| `app/ponto/services/declarar_dia.py:73` `o_dia_ja_e_folga` | EC via `_ec_do_dia(:90)` → `escala_vigente(:56–58)`; esta carrega célula/geradora ou vínculos e chama `vinculo_do_dia`: `app/escala/servico_jornada.py:38`, `:46`, `:55–58`. | Só vê a linha do EC escolhido. `declarar_dia.py:91`, `:70`. | `app/chamados/services/validacao.py:261`, `:316`: aceite já satisfeito, carimbo, fechamento do fio sem declarar novamente (`:262–279`), ou repetição do ramo idempotente (`:316–319`). | Quatro perguntas dão uma folga/uma trilha/um chamado resolvido: `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:167`. |
| `app/ponto/services/declarar_dia.py:94` `_recusa_do_dia` | Para tipo folga, escolhe EC pela porta acima `:105`; lê a linha em `:108`, origem em `:109`. | Não vê linha de outro EC; calendário do EC escolhido produz recusa “edite o calendario”; manual produz “ja e folga”. `:109–112`. | `checar_declaracao(:141)`; `declarar_dia` consulta novamente `:154`. Move veto/preview e escrita do lote `:159–168`; folga escrita por `lancar_folga(:183)` e fio fechado `:198`. Entrada IA registrada `app/core/intake.py:76–93`, resolvida `:481–494`, executada `app/core/views_ia.py:729`, `:775`. Aceite de resposta chama preview e escrita em `app/chamados/services/validacao.py:280`, `:291`; despacho para aceite em `:382–383`. | Recusas manual/calendário: `test_declarar_dia.py:79`, `:87`; precedência: `:99`; relavra: `:144`; fecha fio: `:157`; lote parcial: `:196`; rechecagem: `:208`. Aceite decide dia: `test_o199_aceite_de_folga_decide_o_dia.py:136`; fecha e deixa trilha: `:151`; recusa sem carimbar: `:194`. |
| `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:45` `Command.handle` | Todas as FolgaDia da pessoa na janela da empresa: filtro por empresa da pessoa e data, saída `(colaborador_id,data)`. `:53–57`. Não escolhe EC. | Sim, como candidato ao rejulgamento: não há filtro de EC/origem `:55–57`. O julgamento final é delegado a `julgar_celula(...,forcar=True)` `:69–71`. | Execução por command; classificado como ato único sob demanda em `app/config/crons.py:1400–1401`. `handle(:60)` chama `dias_de_ausencia_em_folga(:24)`; ausência não-falta aprovada cruzada com conjunto pessoa/data `:28–35`; dry-run `:63–65`; recusa se conjunto divergir de `MEDIDAS(:66–68)`. Move seleção de dias para nova lavra de ausência/folga. | `app/ponto/tests/test_rejulgar_ausencia_em_folga.py:23` ausência em folga; `:26` falta fica fora; `:29` aberta; `:58` dry-run; `:64` universo mudou; `:70` força porta. `app/ponto/tests/test_aus_f6_vencidas_pelo_juiz.py:27` usa expansão do juiz. Teste calendário de outro vínculo: `[nao verificado]`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1523` bloco C5 | Itera todas as FolgaDia, converte em pessoa/data; inclui se próprio vínculo cobre ou se `vinculo_que_cobre(...,exceto=...)` não retorna “nenhum” `:1530–1536`. Não escolhe EC soberano do dia. | Sim: não consulta origem nem compara dono da FolgaDia com vínculo soberano `:1532–1536`. | Corpo do command; compara célula da pessoa/data `:1537–1542`, acusa apenas `trabalha is True`, exclui competências exportadas `:1544–1547`, registra contador C5 `:1549–1550`. Move alarme de contradição folha/grade; não corrige aqui. | Teste específico desse C5: `[nao verificado]`. |

O gate solicitado **não lê FolgaDia diretamente nesta cópia**; ele lê a premissa do chamado e só chega à precedência num fallback. `app/chamados/models.py:699`, `:1200`, `:1252–1254`.

| Sítio indireto | Universo/EC e origem | Quem chama e efeito | Testes | [nao verificado]
|---|---|---|---|
| `app/chamados/models.py:678` `_gate_premissa_do_fechamento` | Pessoa e dia do fato pelo juiz `:690–692`; chama `premissa_regularizada(para='folha')` `:699`. Não escolhe EC nem lê `calendario_origem`. | `_decisao_do_fechamento(:646)`, usada por `reconciliar(:615)` e `reconciliaria(:625)`. Regularizada ou agrupada deixa fechar `:699–700`; dedup com sobrevivente também `:709–722`; caso contrário recusa `:723–732`. | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:70`, `:80`, `:95`, `:130`, `:143`; folga: `:196`, `:212`; trabalho com furo recusa: `:217`. |
| `app/chamados/models.py:1200` `premissa_regularizada` | Primeiro célula/ata atual da pessoa/data `:1227–1228`; `dia_sem_trabalho` regulariza chamado que não é turno aberto `:1232–1233`. Depois `_celula_furada(:1247)`; só se resposta indeterminada chama `espurios(:1253)`. Origem FolgaDia não é insumo do ramo célula; portanto não verifica se a célula veio de calendário alheio `[nao verificado]` quanto à origem efetiva. | Gate acima `:699`; reabertura também consome o coletor (contrato descrito em `:1203–1208`, implementação em `recusa_da_reabertura` a partir de `:1266`). Move regularização, fechamento/reabertura de cobrança de furo. | `test_reconciliar_pergunta_a_premissa.py:212` célula folga com ata velha apagada fecha; `:244` gate e reabertura concordam. `app/chamados/tests/test_f1c4_3_none_cobra.py:105` None não regulariza; `:111` ata folga regulariza. |
| `app/chamados/juizes.py:398` `dia_sem_trabalho`; `:411` `_celula_furada` | Lê `trabalha is False` ou `ata.tipo_dia` preenchido diferente de trabalho `:407–408`. `_celula_furada` trabalha em pessoa/data `:417`, ausência `:426`, célula `:436`; dia sem trabalho recursa à véspera com mesmo `para` `:443–449`. EC só é consultado para confiabilidade de órfãs `:464–465`; não lê FolgaDia/origem. | `premissa_regularizada` em `models.py:1232`, `:1247`. Move cobrança/regularização do dia/turno. | `test_f1c4_3_none_cobra.py:39`, `:42`, `:45`, `:50`, `:93`, `:97`, `:101`, `:117`, `:133`. |
| `app/chamados/services/feriado_reavaliar.py:23` `espurios` | Pessoa e dia do chamado por `dia_do_fato(:30)`; chama `fatos_do_dia(:33)`. Herda EC e regra de origem desse juiz. Não aceita candidato de calendário alheio se o juiz o descartar; ainda exige `era_dia_previsto != None(:34–35)` e faixa feriado/folga `:36`. | `models.py:1253`; `reavaliar_dias(:82)`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:24`. `premissa_morta(:38)` seleciona falta espúria; `encerrar(:43)` chama `reconciliar(:47)`. Move retirada de cobrança/chamado. Evento feriado: `app/chamados/signals.py:274`; evento da porta de célula: `app/ponto/portas/celula.py:431`; cron `app/config/crons.py:589`. | `app/chamados/tests/test_varredura_feriado_evento.py:60` evento encerra; `:70` outro dia não muda; `:78` idempotência; `:87` cron e receiver mesma porta; `:94` cron registrado. Teste específico de FolgaDia/calendário alheio nesse serviço: `[nao verificado]`. |

Nenhum teste foi executado por este agente; resultados de execução são `[nao verificado]`. [nao verificado]

## 31/10 19:39 · sessao cf3a0f71ee37 · - · - · origem=Ronald (tmux)

O teste específico do intervalo **inteiramente depois das 05:00** existe, mas exige a correção; não preserva o resultado defeituoso como expectativa: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:4` declara vermelho de propósito e sem `expectedFailure`; o cenário está em `:40-47`: período 22:00–07:00, intervalo 05:30–06:30, prorrogação `True`, hora reduzida afastada.

Seus três testes são: [nao verificado]

- `test_P5_HE100_noturna_pela_regra`: HE100 = 8 h, HE100 noturna = 8 h — `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:49-54`.
- `test_P5_PROPRIEDADE_parte_nao_passa_do_todo`: HE100 noturna ≤ HE100 — `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:56-59`.
- `test_P5_noturnas_pela_regra`: horas noturnas = 8 h — `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61-63`.

O docstring registra 9 h no código atual para ambas as parcelas e atribui o erro ao desconto do intervalo isolado — `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:17-24`. Execução nesta auditoria: [nao verificado].

Testes relacionados, com pausa **atravessando** as 05:00: [nao verificado]

- C3 fixa 22:00–07:00, intervalo 04:30–05:30, prorrogação `True` e hora normal: `horas_noturnas = 8` — `app/ponto/tests/test_v3_periodos_da_celula.py:123-136`.
- A porta deve entregar o período inteiro e os intervalos, inclusive dentro de `he_pontas` — `app/ponto/tests/test_v3_periodos_da_celula.py:138-148`.
- F7f fixa o comportamento geométrico de dois pares, 390 minutos noturnos de relógio e 445,71 legais — `app/ponto/tests/test_o280_bateria_da_lei.py:1562-1592`.
- F7g está em `@unittest.expectedFailure` e pede um par, 480/548,57 — `app/ponto/tests/test_o280_bateria_da_lei.py:1594-1616`.
- v3 F7f compara expressamente par inteiro = 480 com segmentos = 390 — `app/ponto/tests/test_o280_bateria_da_lei.py:4132-4171`; asserção efetiva em `:4168-4171`.

Outras guardas não cobrem o intervalo inteiramente pós05:

- Desconto de pausa noturna com prorrogação desligada — `app/ponto/tests/test_noturno_desconta_intervalo.py:41-61`, `:123-126`, `:170-200`.
- HE noturna recua por trabalho e desconta pausa 23:30–00:30 — `app/ponto/tests/test_noturno_desconta_intervalo.py:245-257`; outra pausa 23:00–23:30 em `:259-269`.
- Testa prorrogação em turno inteiro, sem pausa — `app/ponto/tests/test_s5b_regra_noturnas.py:86-93`.
- Testa desconto de pausa 00:00–01:00 — `app/ponto/tests/test_s5b_regra_noturnas.py:75-84`.
- Grade de equivalência de `he_noturna` inclui atravessar05, mas sem intervalos passados — `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:90-115`.
- Selo estrutural verifica encanamento de intervalos, sem caso temporal pós05 — `app/ponto/tests/test_noturno_sitio_unico.py:130-155`.

A busca textual em todos os `.py`, inclusive ocultos, encontrou chamadas diretas à função alvo apenas em `app/ponto/motor_calculo_v2.py:392` e `:1865`; nos testes, as ocorrências do nome são docstring/mensagem de P5 (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:18`, `:54`), não import nem chamada direta. Nenhum apelido de import da própria função apareceu nessa busca; reflexão/import dinâmico fora dessas ocorrências: [nao verificado].

Também existe o wrapper `noturno_no_par`, que passa o par inteiro à porta comum com os intervalos e a CCT — `app/ponto/calculador/periodo.py:487`, `:508`; convém incluí-lo no item 3 do censo.

## 31/10 19:04 · sessao 6638e332e36a · GRADE-FOLGA-DO-VINCULO · censo · origem=Code

As referências abreviadas abaixo pertencem a **`obra_qr_triagem.txt`, blocos `logs/grade_folga/…`**. A numeração começa na primeira linha após cada separador `=====` e inclui linhas em branco.

**O item 8 não cabe nas cinco classes propostas:** o Codex viu os dois leitores, mas não há descarte documentado pela triagem. Forçar uma classe apagaria essa diferença. (`censo.r:85–86`; descarte: [nao verificado])

1. **Pré-requisito de ensaio — (c), quanto à comprovação material. O requisito foi visto.**

   **Pergunta:** “pouso antes do pre-requisito” (`v5/pv/v1.q:8`).

   **Resposta:** “**Não daria o pouso como comprovado:** a v3 já está declarada, mas exige antes ensaio `--apply` com dias rejulgados, dias que movem minuto e `FORA-DO-HORIZONTE 0`.” (`v3.r:27`). [nao verificado]

   A triagem registrou: “ensaio --apply v3 (rejulgados, minutos, FORA-DO-HORIZONTE 0) antes do pouso”. Portanto, houve alerta e reconhecimento; a suficiência do ensaio posterior depende da evidência da sombra. (`triagem_v3.txt:5`; `opus_final.txt:2`) [nao verificado]

2. **Reversão, prova de frota e hashes FM/TXT — (c).**

   **Pergunta:** “FechamentoMensal e TXT ficam intactos por construcao ou so no teste?” (`v5/pv/v1.q:5`).

   **Resposta:** “**FechamentoMensal e TXT ficam fora das escritas diretas desse caminho por construção**, com testemunha adicional no teste”, seguida da ressalva: “Isso não constitui imunidade universal”. (`v5/pv/v1.r:46`).

   A resposta também apontou que o teste observava menos que o registro inteiro. Isso é análise do caminho e da fixture; não comprova reversão em `logs/`, execução na frota ou hashes reais da 09. Esses são os artefatos ausentes relatados pelo opus. (`v5/pv/v1.r:48`; `opus_final.txt:3`; comprovação independente: [nao verificado])

3. **`delta_chamados=0` antigo não certifica a alteração posterior — (c).**

   **Pergunta:** “Quem le `ata['tipo_dia']` gravada na CelulaDia, e para que?” (`censo.q:6`).

   **Resposta:** `dia_sem_trabalho` “Retorna verdadeiro se `cel.trabalha is False` **ou** se a ata tem tipo preenchido diferente de `'trabalho'`.” (`censo.r:99`).

   Portanto, o consumidor foi visto; não cabe atribuir ao Codex desconhecimento desse elo. A validade do delta depois de `836c57b7` exige nova comparação material, que não é demonstrada por essa leitura estática. (`censo.r:99`; `opus_final.txt:4`; delta posterior: [nao verificado])

4. **“Sem `--forcar` não rejulga a 09” — (b), alerta conhecido afastado pela decisão posterior.**

   **Pergunta:** “E sem `--forcar` (o cron de prod), em que condicoes um dia de competencia 2026-09 e rejulgado (impressao muda? quais insumos compoem a impressao)?” (`v4_fora/q1.q:5`).

   **Resposta:** “**Sim, pelo componente `cobparcial`, mesmo mantendo `regra:1` e o mesmo `tipo_dia` final.**” E: “O cartório incorpora esse estado como `cobparcial:1:<estado>`, sem filtro por versão.” (`v4_fora/q1.r:48,53`).

   **Triagem e motivo:** “o pouso nao rejulga a 09 (horizonte 2026-10, provado por mutante)”. A decisão tomou o mutante como prova geral apesar do mecanismo anteriormente identificado. As sete células concretas são dado do opus, não reprodução do Codex. (`v5/pv/triagem_v1.txt:3`; `opus_final.txt:5`)

5. **Pauta DP da 09 — (b), explicitamente adiada.**

   **Pergunta:** “Quem gera a Pauta DP da diferenca pago x devido e onde?” (`v5/pv/v1.q:5`).

   **Resposta:** “Não encontrei chamada correspondente no rejulgamento desta cura; a declaração da v3 apenas promete a Pauta.” (`v5/pv/v1.r:50`).

   **Triagem e motivo:** “DECIDIDO PELA LEI (3, Pauta DP nao encadeada)”, invocando “a Pauta DP da 09 e ato proprio, depois”, e destinando a `BACKLOG GRADE-09-PAUTA`. Logo, a lacuna foi apontada e conscientemente adiada. (`v5/pv/triagem_v1.txt:3`)

6. **`_folga_avulsa` mantém outra regra de validade — (b), literalmente FILA.**

   **Pergunta:** “Algum outro sitio decide "esta FolgaDia vale neste dia" com regra diferente da de folga_lancada_vale_no_dia e alimenta a mesma lavra/ata/chamado?” (`v1.q:11`).

   **Resposta:** “o veto consulta **qualquer FolgaDia da pessoa/data**, sem validar proveniência/vínculo”, permitindo que calendário antigo rejeitado pelo predicado ainda impeça lançamento. (`v1.r:109`).

   **Triagem e motivo:** “veto de _folga_avulsa […] -> FILA: respondem "pode lancar?", nao a lavra”. O mesmo destino foi repetido na volta 2. (`triagem_v1.txt:7`; `triagem_v2.txt:5`)

7. **`_folga_do_dia` ignora manual do vínculo antigo — (b), literalmente FILA.**

   **Pergunta:** “Algum outro sitio decide "esta FolgaDia vale neste dia" com regra diferente da de folga_lancada_vale_no_dia e alimenta a mesma lavra/ata/chamado?” (`v1.q:11`).

   **Resposta:** “Uma manual presa ao velho vale pelo predicado proposto, mas não é reconhecida ali. Esse booleano decide o caminho que carimba/fecha a pergunta e o fio.” (`v1.r:107`).

   **Triagem e motivo:** `declarar_dia.o_dia_ja_e_folga` foi enviado à FILA porque responderia “pode lancar?”, “nao a lavra”. Portanto, o Codex apontou inclusive a consequência sobre chamado; a triagem afastou o bloqueio. (`triagem_v1.txt:7`; `triagem_v2.txt:5`)

8. **Leitores com regra velha — sem encaixe estrito nas classes.**

   **Pergunta:** “Que outros sitios do codigo decidem se uma `FolgaDia` vale como folga de um dia”; a pergunta exige listar cada sítio e seu tratamento de calendário e manual de outro vínculo. (`censo.q:5`).

   **Resposta sobre `fase_conflitante`:** “Lê fotos de cada vínculo ativo, sem filtro de origem/vigência por data”. (`censo.r:85`).

   **Resposta sobre `rejulgar_ausencia_em_folga`:** “Seleciona pares pessoa/data por foto da empresa/janela, sem distinguir origem ou vínculo diário.” (`censo.r:86`).

   Assim, **o Codex viu ambos**. Descarte pela triagem: **[nao verificado]**. A situação documentada é “visto no censo, sem destino demonstrado”; classificá-la como (a), (d) ou (e) negaria os trechos acima, e classificá-la como (b) inventaria uma decisão. (`censo.q:5`; `censo.r:85–86`)

9. **Predicado novo sem juiz/corte e completude indevida — (a), quanto à falha estrutural específica.**

   **Pergunta:** “com JUIZES.md anexo. Qual e o juiz unico de "este dia e folga?" e quantos sitios respondem essa pergunta depois do diff?” (`v5/pv/v1.q:3`).

   **Resposta:** “**Um predicado:** `folga_lancada_vale_no_dia`”; “**Um seletor**”; “**Dois sítios consumidores:** precedência e overlay da grade.” (`v5/pv/v1.r:26–28`).

   A resposta apresentou essa centralização sem apontar a falta específica de registro/corte cobrada pelo opus. O tema autoridade/juiz estava expressamente perguntado. Os resíduos 6 e 7, entretanto, já haviam sido vistos e enfileirados; essa parte do item composto não é descoberta nova. (`v5/pv/v1.q:3`; `v5/pv/v1.r:26–28`; `triagem_v2.txt:5`; `opus_final.txt:10`)

   A correspondência da LEI-AKITA ao commit `362c6d8a`: **[nao verificado]**.

10. **`red_aval.log` era verde; RED real em `red0_aval.log` — (c).** [nao verificado]

    **Pergunta:** “Os RED mordem? Que mutante simples […] cada teste novo deixaria passar?” (`v5/pv/v1.q:6`).

    **Resposta:** “A tabela descreve **mordida prevista por inspeção**, não execução de mutantes. Resultado RED/GREEN: **[não verificado]**.” (`v5/pv/v1.r:56`).

    A resposta delimitou a inspeção; não certificou os arquivos de execução. A troca concreta dos rótulos é informação do opus, cuja conferência independente permanece **[nao verificado]**. (`v5/pv/v1.r:56`; `opus_final.txt:11`)

Contagem das atribuições acima:

| Classe | Itens | Total |
|---|---|---:|
| **(a)** | 9, na falha estrutural específica | **1** |
| **(b)** | 4, 5, 6, 7 | **4** |
| **(c)** | 1, 2, 3, 10, na comprovação material | **4** |
| **(d)** | Nenhum identificado | **0** |
| **(e)** | Nenhum item integral identificado | **0** |
| **Sem encaixe nas definições** | 8: visto, sem descarte demonstrado | **1** |

A exceção que impede fechar os dez nas cinco classes está documentada no próprio censo. (`censo.r:85–86`; descarte correspondente: [nao verificado])

## 31/10 19:04 · sessao ad8bd6de03f7 · PADRAO-DE-CIMA · verificador · origem=Code

**Pré-veredito: REPROVA.** O diff corrige os defeitos originais de fotografia e publicação do escopo, mas ainda há caminhos de dúvida para LIVRE e mortes acidentais aceitas como mordida. **Certeza alta sobre esses fluxos; ocorrência em produção [não verificado].** (`bin/disjuntor.py:1493`, `bin/disjuntor.py:1261`, `bin/tests/test_disjuntor.sh:83`)

Não executei as suítes, pois criam e removem arquivos. Fiz leitura estática e sondas com funções isoladas em memória, sem persistência. Resultado integral dos selos: **[não verificado]**. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_padrao_de_cima.sh:29`, `bin/tests/test_padrao_de_cima.sh:30`)

Os itens A–F ficam assim; os graus abaixo são de leitura do código.

| Item | Cura na origem | O que ainda escapa |
|---|---|---|
| **A — parcialmente resolvido; alta** | No caminho normal, RELATO ilegível produz `nao_sei` com `duvida=None`; `estado_pdc` recusa as obras não reconhecidas como PARADAS. (`bin/disjuntor.py:1449`, `bin/disjuntor.py:1502`, `bin/disjuntor.py:1505`) | Se a decisão levanta, o fallback retorna **LIVRE fora de `esc['disparadas']`**, ignorando a dúvida e até `obras_com_evento=None`. (`bin/disjuntor.py:1487`, `bin/disjuntor.py:1491`, `bin/disjuntor.py:1493`) |
| **B — defeito original resolvido; alta** | Cada entidade disparada ganha fotografia normalizada do topo; SHA presente nela não libera nem é consumido. Redisparo reinicia a fotografia. (`bin/disjuntor.py:1390`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1417`, `bin/disjuntor.py:1430`) | A fotografia representa a observação feita dentro da transição; não há exclusão dos escritores do RELATO nem reobservação antes de publicar, como existe no B3. (`bin/disjuntor.py:1361`, `bin/disjuntor.py:1431`, `bin/disjuntor.py:746`, `app/core/esteira_vigia.py:848`) |
| **C — corrida original resolvida; alta** | `_pdc_grava_escopo` passou ao anúncio executado antes de fechar a trava do estado. Transições cooperantes deixam de publicar o escopo em ordem invertida. (`bin/disjuntor.py:1439`, `bin/disjuntor.py:293`, `bin/disjuntor.py:296`) | Estado e escopo continuam duas publicações separadas. Falha do escopo mantém o anterior; o fallback não verifica seu frescor. (`bin/disjuntor.py:281`, `bin/disjuntor.py:1465`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:1490`) |
| **D — parcialmente resolvido; alta** | Exceção propagada por `entidades` marca a obra em dúvida e deixa o evento fora do estado, permitindo nova tentativa. (`bin/disjuntor.py:1376`, `bin/disjuntor.py:1379`, `bin/disjuntor.py:1383`, `bin/disjuntor.py:1451`) | Falhas internas ainda são engolidas: `_so_modelos` continua após erro; `_por_sitio` transforma fonte ilegível em ausência. Isso pode persistir `entidades=[]` ou programa alternativo. Eventos já persistidos — inclusive vazios herdados da v3 — não são reclassificados. (`bin/disjuntor.py:1234`, `bin/disjuntor.py:1261`, `bin/disjuntor.py:1283`, `bin/disjuntor.py:1376`, `bin/disjuntor.py:1384`) |
| **E — parcialmente resolvido; alta** | O mutante passa a importar `H`; a morte estrutural por `KeyError` foi substituída pela retirada da fotografia. Há sonda para cinco classes de exceção. (`bin/tests/test_disjuntor.sh:1714`, `bin/tests/test_padrao_de_cima.sh:559`, `bin/tests/test_padrao_de_cima.sh:562`, `bin/tests/test_disjuntor.sh:54`) | `_morre` ainda aceita qualquer retorno não zero sem contaminação registrada; não comprova assert do caso. Exceções fora da lista e permissões amplas por prefixo continuam aceitando mortes acidentais. (`bin/tests/test_disjuntor.sh:77`, `bin/tests/test_disjuntor.sh:79`, `bin/tests/test_disjuntor.sh:83`) |
| **F — lacunas apontadas resolvidas no selo; alta** | Compara o mapa inteiro com o BACKLOG; testa chave plantada, O261 retirada e destino trocado. Exercita cada fonte de vocabulário ausente e os dois mutantes `raise → pass`. (`bin/tests/test_padrao_de_cima.sh:486`, `bin/tests/test_padrao_de_cima.sh:500`, `bin/tests/test_padrao_de_cima.sh:503`, `bin/tests/test_padrao_de_cima.sh:451`, `bin/tests/test_padrao_de_cima.sh:582`) | A guarda do vocabulário já existia; a alteração é de cobertura. `PROGRAMAS` continua cadastro literal independente, reconciliado pelo selo segundo três formas textuais do BACKLOG. (`bin/padrao_vocab.py:46`, `bin/padrao_vocab.py:56`, `bin/disjuntor.py:1111`, `bin/tests/test_padrao_de_cima.sh:475`, `bin/tests/test_padrao_de_cima.sh:483`) |

Quanto a **quem escreve e quem lê**, esta é a cadeia localizada. Certeza alta sobre os consumidores citados; autoria humana concreta e execução em produção **[não verificado]**.

| Campo ou arquivo | Quem escreve | Quem lê |
|---|---|---|
| **`logs/sessao/disjuntor_estado.json`** | `_com_estado` lê, modifica e publica por `replace` sob flock. Compartilhado com B1, B3, rearme, triagem e B4. (`bin/disjuntor.py:260`, `bin/disjuntor.py:275`, `bin/disjuntor.py:281`, `bin/disjuntor.py:494`, `bin/disjuntor.py:845`, `bin/disjuntor.py:986`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:1992`) | Transições pela mesma porta; leitores de painel usam `ler_estado`, sem tomar essa trava. (`bin/disjuntor.py:267`, `bin/disjuntor.py:299`, `bin/disjuntor.py:1567`) |
| **`pdc.eventos[sha]`: `visto_em`, `tipo`, `obra`, `entidades`** | `decide_pdc`, somente para SHA ainda ausente. Falha propagada não insere evento. (`bin/disjuntor.py:1376`, `bin/disjuntor.py:1383`, `bin/disjuntor.py:1384`) | Janela, agrupamento, nomeação e painel. `visto_em` não é renovado na releitura. (`bin/disjuntor.py:1288`, `bin/disjuntor.py:1298`, `bin/disjuntor.py:1422`, `bin/disjuntor.py:1568`) |
| **`pdc.nomeados`, `libera_vistas`, `disparadas`** | A transição consome SHA, nomeia pares evento/entidade e recalcula disparadas. (`bin/disjuntor.py:1419`, `bin/disjuntor.py:1424`, `bin/disjuntor.py:1428`) | Disparo, proteção contra reutilização, comparação com episódio anterior e painel. (`bin/disjuntor.py:1296`, `bin/disjuntor.py:1302`, `bin/disjuntor.py:1389`, `bin/disjuntor.py:1414`, `bin/disjuntor.py:1576`) |
| **`pdc.fotos[entidade]`** | A transição cria/reinicia, fotografa os hashes das linhas e remove ao encerrar episódio. (`bin/disjuntor.py:1387`, `bin/disjuntor.py:1390`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1429`) | A própria decisão bloqueia liberação presente na fotografia. O painel não consome esse campo. (`bin/disjuntor.py:1417`, `bin/disjuntor.py:1567`, `bin/disjuntor.py:1575`) |
| **`nao_sei` / `duvida`** | Atributos **em memória** de `DecisaoPDC`, preenchidos após a transição. (`bin/disjuntor.py:1341`, `bin/disjuntor.py:1447`, `bin/disjuntor.py:1449`, `bin/disjuntor.py:1451`) | `estado_pdc`; depois `_nega_pdc` e CLI traduzem para recusa. Não são serializados no escopo. (`bin/disjuntor.py:1505`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:1540`, `bin/disjuntor.py:1460`) |
| **`logs/padrao_de_cima_escopo.json`: `em`, `disparadas`, `obras_com_evento`** | `_pdc_grava_escopo`, chamado ainda sob a trava do estado. (`bin/disjuntor.py:1439`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1465`) | Fallback de `estado_pdc`; nesse ramo usa apenas `disparadas`, sem validar `em` ou consultar `obras_com_evento`. (`bin/disjuntor.py:1474`, `bin/disjuntor.py:1489`, `bin/disjuntor.py:1493`) |
| **RELATO: topo e corpo** | Topo manual é reconhecido e preservado por `no_relato`; negativas passam por essa porta. **Também escrevem fora dela** `gerar_cortes` e `deploy_agendado`. (`app/core/esteira_vigia.py:829`, `bin/disjuntor.py:236`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:44`) | PDC varre corpo e recorta topo; B3 lê topo; Stop lê PAREI/ESPERANDO e verifica prova; handoff e publicação também leem. (`bin/disjuntor.py:1361`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1375`, `bin/disjuntor.py:677`, `bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:1176`, `bin/handoff_sessao.sh:92`, `bin/relato.sh:56`) |
| **Modelos/fontes citadas; `PROGRAMAS`; BACKLOG** | Cadastro de programas é literal no código; o selo altera cópias para testar divergências. (`bin/disjuntor.py:1111`, `bin/tests/test_padrao_de_cima.sh:499`, `bin/tests/test_padrao_de_cima.sh:506`) | Resolvedor lê modelos e fontes; programa só entra sem entidade. BACKLOG é consultado pelo selo, não pelo resolvedor. (`bin/disjuntor.py:1217`, `bin/disjuntor.py:1233`, `bin/disjuntor.py:1259`, `bin/disjuntor.py:1283`, `bin/tests/test_padrao_de_cima.sh:469`) |
| **GLOSSARIO / PLANO-PADROES** | Esses leitores não os escrevem. Autor efetivo **[não verificado]**. (`bin/disjuntor.py:1331`, `bin/padrao_vocab.py:41`, `bin/padrao_vocab.py:48`) | Ambos alimentam `padrao_vocab.vocabulario`, chamado por PDC e LEI-AKITA. PDC lê disco; LEI-AKITA lê o commit por `git show`. (`bin/disjuntor.py:1334`, `bin/tests/test_lei_akita.sh:77`, `bin/tests/test_lei_akita.sh:89`) |
| **`disjuntor.log`** | `trilha`; anúncios PDC após gravação válida; negativas do PreToolUse antes da resposta. (`bin/disjuntor.py:135`, `bin/disjuntor.py:1440`, `bin/disjuntor.py:2014`) | `ler`/`linha` apresentam contadores, incluindo PDC. (`bin/disjuntor.py:2085`, `bin/disjuntor.py:2107`, `bin/disjuntor.py:2220`) |
| **Artefatos dos selos: cópias, fixtures, `sitecustomize.py`, `.exc`** | Scripts criam cópias e mutantes; callback da sonda acrescenta tipo/arquivo/função ao `.exc`. (`bin/tests/test_disjuntor.sh:48`, `bin/tests/test_disjuntor.sh:62`, `bin/tests/test_padrao_de_cima.sh:525`, `bin/tests/test_padrao_de_cima.sh:535`) | `_excecao` lê o `.exc`; casos leem fixtures, estado, escopo e saídas das portas. (`bin/tests/test_disjuntor.sh:77`, `bin/tests/test_padrao_de_cima.sh:124`, `bin/tests/test_padrao_de_cima.sh:155`) |

As configurações `.claude` chamam **caminhos absolutos da árvore viva**, tanto no Stop quanto no PreToolUse. Que executem este hash em produção: **[não verificado]**. (`.claude/settings.json:9`, `.claude/settings.json:68`)

Sobre **NAO SEI sair como LIVRE**, há quatro situações distintas:

1. **Consulta normal: corrigida.** PARADA tem precedência; caso contrário, RELATO ilegível recusa todas, e falha propagada do resolvedor recusa a obra afetada. PreToolUse transforma NAO_SEI em negativa; CLI retorna 4; wrapper recusa qualquer rc não zero. **Alta.** (`bin/disjuntor.py:1449`, `bin/disjuntor.py:1451`, `bin/disjuntor.py:1502`, `bin/disjuntor.py:1526`, `bin/disjuntor.py:1540`, `bin/codex_pergunta.sh:168`)

2. **Fallback por escopo: ainda LIVRE.** Uma decisão com dúvida pode publicar escopo sem a obra em `disparadas`; depois, estado ilegível ou trava ocupada conduz a `return PDC_LIVRE`. Nem `obras_com_evento=None` impede isso. As sondas em memória confirmaram esse retorno para escopo com `None` e para escopo contendo a obra em `obras_com_evento`. **Alta, condicional às falhas indicadas.** (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1461`, `bin/disjuntor.py:265`, `bin/disjuntor.py:1487`, `bin/disjuntor.py:1493`)

3. **Falha interna do resolvedor: dúvida não reconhecida.** Fonte citada ilegível pode retornar `[]`, persistido como evento resolvido; depois não há nova tentativa para aquele SHA. A sonda isolada confirmou `entidades=[]` nessa condição. **Alta.** (`bin/disjuntor.py:1261`, `bin/disjuntor.py:1264`, `bin/disjuntor.py:1285`, `bin/disjuntor.py:1376`, `bin/disjuntor.py:1384`)

4. **Chamadores que não consultam ou não exibem PDC.** Agent sem obra reconhecida pula o gate; wrapper só consulta PDC no papel `verificador`; chave desligada permite explicitamente. Stop não chama PDC. Painel lê o estado persistido e pode mostrar zero disparadas sem representar a dúvida — **não imprime literalmente LIVRE**. **Alta.** (`bin/disjuntor.py:1978`, `bin/codex_pergunta.sh:164`, `bin/disjuntor.py:1534`, `bin/hook_stop_fila1.py:1048`, `bin/hook_stop_fila1.py:1058`, `bin/disjuntor.py:1567`, `bin/disjuntor.py:2220`)

Há ainda um escape condicional no **envelope excepcional do PreToolUse**: `_ramo_pdc` relê RELATO; se a obra não aparecer nessa segunda versão, pode retornar `None` e chegar a “ferramenta liberada”. **Alta sobre o fluxo; concorrência efetiva [não verificado].** (`bin/disjuntor.py:1557`, `bin/disjuntor.py:1561`, `bin/disjuntor.py:2028`, `bin/disjuntor.py:2037`)

Quanto à **foto e TOCTOU**, corpo, topo e fotografia PDC usam os mesmos bytes, obtidos dentro da trava do estado. Redisparo redefine a foto e a recompõe nessa mesma trava; estado anterior à v4 recebe fotografia tardia. **Alta.** (`bin/disjuntor.py:260`, `bin/disjuntor.py:275`, `bin/disjuntor.py:1361`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1390`, `bin/disjuntor.py:1392`, `bin/disjuntor.py:1396`)

**Existem leituras fora dessa trava:** fallback de `estado_pdc`, envelope excepcional e leitores do Stop. Podem observar outro RELATO, mas não compõem a fotografia interna de `decide_pdc`. **Alta.** (`bin/disjuntor.py:1494`, `bin/disjuntor.py:1557`, `bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:429`)

A trava do estado também não congela RELATO: o escritor central trava seu diretório, outra trava. Portanto uma escrita após o último `stat` pode deixar a fotografia anterior à publicação do estado. PDC não reobserva; B3 faz uma segunda observação e une as duas. **Alta sobre a diferença; aceitação de linha nessa janela é inferência condicional, ocorrência [não verificado].** (`bin/disjuntor.py:256`, `bin/disjuntor.py:711`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1431`, `app/core/esteira_vigia.py:844`, `app/core/esteira_vigia.py:848`, `bin/disjuntor.py:746`, `bin/disjuntor.py:748`)

Sobre **a sonda e as três exceções declaradas**:

- **`try/except` no próprio código não esconde, sozinho, uma das cinco classes monitoradas.** O callback está registrado em `RAISE`, antes do tratamento posterior, filtrando pelo arquivo da cópia. **Alta.** (`bin/tests/test_disjuntor.sh:54`, `bin/tests/test_disjuntor.sh:57`, `bin/tests/test_disjuntor.sh:60`, `bin/tests/test_disjuntor.sh:64`)
- **Outros tipos escapam.** `ValueError`, `RuntimeError`, `OSError`, entre outros, não entram na tupla; retorno não zero com `.exc` vazio é aceito. Contraexemplo estático: `raise ValueError` no começo de `estado_pdc` faz `i14_mesma` falhar no rc esperado, sem contaminação registrada. **Alta pelo fluxo; execução desse mutante [não verificado].** (`bin/tests/test_disjuntor.sh:54`, `bin/tests/test_disjuntor.sh:77`, `bin/tests/test_disjuntor.sh:84`, `bin/disjuntor.py:1481`, `bin/tests/test_padrao_de_cima.sh:150`)
- **A exceção declarada não precisa ocorrer.** Log vazio é aceito. Se o caso passar, o mutante é corretamente acusado de sobreviver; se falhar por outro motivo não monitorado, pode ser contado como morto sem levantar a declarada. Logo, falta uma obrigação positiva de observar o erro esperado. **Alta.** (`bin/tests/test_disjuntor.sh:77`, `bin/tests/test_disjuntor.sh:79`, `bin/tests/test_disjuntor.sh:84`, `bin/tests/test_disjuntor.sh:1537`)

| Mutante | Exceção perdoada pelo filtro — certeza alta |
|---|---|
| `cx8-ausente-sem-falha` | Qualquer `AttributeError` em **qualquer função** de `bin/disjuntor.py`. (`bin/tests/test_disjuntor.sh:1652`) |
| `B3-ref-fora-da-pergunta` | Qualquer `TypeError` em `linhas_libera_b3`, sem identificar a operação ou linha. (`bin/tests/test_disjuntor.sh:1706`) |
| `P7-tipo-solto` | Qualquer `AttributeError` em **qualquer função** de `bin/statusline_cota.py`. (`bin/tests/test_disjuntor.sh:1775`) |

**Sim, o prefixo do arquivo inteiro aceita exceção de outro sítio.** O log guarda tipo, arquivo e nome da função; o filtro usa substring fixa, sem identificação da linha. Os autotestes acrescentados não cobrem erro fora dos cinco tipos, ausência da exceção esperada ou mesmo tipo em outro sítio permitido. **Alta.** (`bin/tests/test_disjuntor.sh:62`, `bin/tests/test_disjuntor.sh:79`, `bin/tests/test_disjuntor.sh:1839`, `bin/tests/test_padrao_de_cima.sh:592`)

Por fim, as **oito classes históricas procuradas**:

| Classe | Resultado e grau de certeza |
|---|---|
| **Segundo juiz duplicando registrado** | **Não demonstrado no recorte.** PDC reutiliza censo e vocabulário; `_so_modelos` acrescenta filtro de herança ao catálogo amplo. Não localizei reprodução dos juízes de negócio registrados; ausência global **[não verificado]**. (`bin/disjuntor.py:1198`, `bin/disjuntor.py:1217`, `bin/disjuntor.py:1219`, `bin/disjuntor.py:1334`, `app/core/censo_escritas.py:90`, `app/core/juizes.py:156`, `app/core/juizes.py:1255`) |
| **Segundo escritor fora da porta** | **Presente; alta.** `gerar_cortes` e `deploy_agendado` leem/regravam RELATO sem o flock central; resíduo documentado pelo próprio escritor. (`bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:37`, `bin/deploy_agendado.sh:44`, `app/core/esteira_vigia.py:839`) |
| **Relógio fora da borda** | **Presente; alta.** Hooks injetam `agora`; CLI deixa decisões lerem `time.time()`. Trilha e triagem também têm relógio próprio. (`bin/disjuntor.py:2007`, `bin/disjuntor.py:1979`, `bin/disjuntor.py:1536`, `bin/disjuntor.py:1354`, `bin/disjuntor.py:904`, `bin/disjuntor.py:140`, `bin/disjuntor.py:840`) |
| **Leitura dupla com foto diferente** | **Corpo/topo PDC corrigidos; resíduos presentes, alta.** Censo e filtro de herança leem modelos separadamente; painel abre a trilha quatro vezes; fallback relê RELATO. (`bin/disjuntor.py:1369`, `app/core/censo_escritas.py:105`, `bin/disjuntor.py:1233`, `bin/disjuntor.py:2085`, `bin/disjuntor.py:2088`, `bin/disjuntor.py:1557`) |
| **Selo sem mordida** | **Classe ainda aberta; alta.** Há testes comportamentais e controles, mas o harness aceita mortes não identificadas como assert e exceções perdoáveis por prefixo amplo. (`bin/tests/test_padrao_de_cima.sh:529`, `bin/tests/test_padrao_de_cima.sh:565`, `bin/tests/test_disjuntor.sh:79`, `bin/tests/test_disjuntor.sh:83`) |
| **Ordem de trava** | **Sem inversão localizada nos percursos examinados; alta local.** Wrapper: slot → resposta → estado; triagem: resposta compartilhada → estado. C corrige publicação do escopo fora da trava. Ausência global de inversão **[não verificado]**. (`bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/disjuntor.py:813`, `bin/disjuntor.py:845`, `bin/disjuntor.py:1439`) |
| **Fallback** | **Presente e liberador; alta.** Escopo incompleto/antigo pode dar LIVRE; falha interna pode virar entidade vazia. Programa alternativo continua expressamente cadastrado. (`bin/disjuntor.py:1493`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:1261`, `bin/disjuntor.py:1283`) |
| **argv com dado sensível** | **Superfície presente; conteúdo sensível efetivo [não verificado].** Gates passam `$OBRA`; o valor só é validado como não vazio, sem raspagem. Pergunta e anexos entram no Codex por stdin. (`bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:178`) |

## 31/10 19:04 · sessao 063a033030e1 · - · - · origem=Ronald (tmux)

Classificação sugerida: **1=c; 2=c; 3=c; 4=b; 5=b; 10=c**. Numeração abaixo começa na primeira linha depois de cada `=====`.

| Item | Evidência e ressalva |
|---|---|
| **1 — c** | O requisito foi visto: `logs/grade_folga/v3.q:10` pergunta “pouso antes do pre-requisito”; `v3.r:27`: “**Não daria o pouso como comprovado:** a v3 já está declarada, mas exige antes ensaio `--apply` com dias rejulgados, dias que movem minuto e `FORA-DO-HORIZONTE 0`.” A triagem **acolheu**, não descartou: `triagem_v3.txt:5`: “devido ja registrado: ensaio --apply v3 (...) antes do pouso.” A prova material posterior pertence ao ensaio. |
| **2 — c** | `v5/pv/v1.q:5`: “FechamentoMensal e TXT ficam intactos por construcao ou so no teste?” `v5/pv/v1.r:46`: “Isso não constitui imunidade universal: outro caminho, `recalcular_fechamento_mes`, admite exportadas e atualiza o fechamento.” `r:48` denuncia preservação parcial do teste. A falta de reversão/logs/prova real de frota não pode ser suprida pela inspeção. |
| **3 — c** | O leitor foi identificado: `v5/pv/v1.r:11-12` nomeia `dia_sem_trabalho` e `app/chamados/juizes.py:408`. Mas a validade do **delta medido v4 após outra mudança** requer ensaio novo. `v5/pv/v1.r:3`: “Execução de testes, mutantes, ensaio de impacto e correspondência Git da cópia aos commits informados: **[não verificado]**.” |
| **4 — b** | `v4_fora/q1.q:5`: “E sem `--forcar` (o cron de prod), em que condicoes um dia de competencia 2026-09 e rejulgado (...)?” `q:6`: “A impressao de um dia com versao_para < 3 pode mudar (...)?” `q1.r:26` diz que impressão divergente permite julgamento inclusive da 09; `r:48`: “**Sim, pelo componente `cobparcial`, mesmo mantendo `regra:1` e o mesmo `tipo_dia` final.**”; `r:53`: “sem filtro por versão.” **Triagem contrária:** `v5/pv/triagem_v1.txt:3`: “o pouso nao rejulga a 09 (horizonte 2026-10, provado por mutante)”. Motivo dado: horizonte e mutante da fixture. O número real de sete células pertence ao opus. |
| **5 — b** | `v5/pv/v1.q:5`: “Quem gera a Pauta DP da diferenca pago x devido e onde?” `r:50`: “Não encontrei chamada correspondente no rejulgamento desta cura; a declaração da v3 apenas promete a Pauta.” `r:52`: “Entrega da Pauta DP referente à diferença específica da v5: **[não verificado]**.” **Triagem:** `v5/pv/triagem_v1.txt:3`, “DECIDIDO PELA LEI (...) precedente VERSAO 1 (...) ‘a Pauta DP da 09 e ato proprio, depois’ (...) BACKLOG GRADE-09-PAUTA.” |
| **10 — c** | `v5/pv/v1.q:6`: “Os RED mordem? Que mutante simples (...) cada teste novo deixaria passar?” `r:56`: “A tabela descreve **mordida prevista por inspeção**, não execução de mutantes. Resultado RED/GREEN: **[não verificado]**.” Os nomes `red_aval.log` e `red0_aval.log` não são discutidos em Q/R; distinguir as execuções exige os logs. |

Números adicionais conferidos de `v5/pv/v1.r`:

- `:26-28`: “Um predicado”; “Um seletor”; “Dois sítios consumidores”.
- `:107`: requisito de ensaio e evidência v5 de ensaio/suíte/pouso não fornecida. [nao verificado]
- `:110`: aceite limitado; exige comprovar Pauta e RED/ensaios.

A cópia local não deve ser tratada como árvore v5: `app/ponto/services/cartorio.py:111` contém apenas versões 1 e 2. Confirma mecanismos relevantes em `app/ponto/services/cartorio.py:386-390` (`cobparcial` sem condição de versão), `app/chamados/juizes.py:407-408` (leitura da ata) e `app/ponto/services/fechamento.py:1015-1016` (trabalho/ausência entram no previsto).

## 31/10 19:04 · sessao 6a308bf3be73 · - · - · origem=Ronald (tmux)

Numeração interna dos blocos, começando na primeira linha após `=====` e contando linhas em branco:

| Bloco | Linha | Evidência |
|---|---:|---|
| `logs/grade_folga/censo.r` | 59 | `_folga_avulsa` conta por pessoa/data sem filtro de vínculo, origem ou vigência. |
| Mesmo | 60 | `declarar_dia` não reconhece localmente manual de outro vínculo. |
| Mesmo | 85 | `fase_conflitante` inventariado explicitamente, “sem filtro de origem/vigência por data”. |
| Mesmo | 86 | `rejulgar_ausencia_em_folga` inventariado explicitamente, “sem distinguir origem ou vínculo diário”. |
| Mesmo | 99 | `dia_sem_trabalho` lê o tipo persistido na ata. |
| Mesmo | 107 | Fechamento mensal inclui trabalho/ausência no previsto e grava os totais. |
| `logs/grade_folga/v1.r` | 107 | “**Há outro sítio alimentando diretamente o mesmo chamado:** `o_dia_ja_e_folga` busca somente FolgaDia do vínculo do dia.” |
| Mesmo | 109 | “o veto consulta **qualquer FolgaDia da pessoa/data**, sem validar proveniência/vínculo.” |

Para 6/7, triagens exatas: `logs/grade_folga/triagem_v1.txt:7` e `logs/grade_folga/triagem_v2.txt:5`. Para 8, o inventário está comprovado, mas descarte pela triagem permanece **[não verificado]**.

## 31/10 19:02 · sessao 6a308bf3be73 · - · - · origem=Ronald (tmux)

6 e 7: **(b), inequívocos**. `logs/grade_folga/v1.q:11` pergunta literalmente: “Algum outro sitio decide "esta FolgaDia vale neste dia" com regra diferente da de folga_lancada_vale_no_dia e alimenta a mesma lavra/ata/chamado?” A resposta v1, §6, descreve tanto a manual antiga ignorada por `o_dia_ja_e_folga` quanto o veto que aceita calendário antigo. `logs/grade_folga/triagem_v1.txt:7`: “declarar_dia.o_dia_ja_e_folga, veto de _folga_avulsa, foto {d: True} do calendario -> FILA: respondem "pode lancar?", nao a lavra”; `logs/grade_folga/triagem_v2.txt:5` repete o destino. O código da cópia confirma os mecanismos em `app/ponto/precedencia.py:165`, `app/ponto/precedencia.py:458`, `app/ponto/services/declarar_dia.py:70` e `app/ponto/services/declarar_dia.py:91`.

8: **sem encaixe estrito nas cinco classes**. O censo viu explicitamente os dois leitores e descreveu a falta dos filtros; não há triagem nominal descartando-os. Classificar (a) ou (d) negaria esse inventário; classificar (b) atribuiria descarte não documentado. O descarte amplo em `logs/grade_folga/v5/pv/triagem_v1.txt:5` exclui outros sentidos de folga, preservando expressamente o escopo de “VALIDADE da FolgaDia”, portanto não prova descarte desses leitores. Os mecanismos estão em `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:55` e `app/escala/services/fase_conflitante.py:74`.

9: **(a) para a falha estrutural de registro/corte e a falsa completude do juiz único**, com ressalva de item composto. `logs/grade_folga/v5/pv/v1.q:3` exige “com JUIZES.md anexo. Qual e o juiz unico de "este dia e folga?" e quantos sitios respondem essa pergunta depois do diff?” A resposta afirma “Um predicado”, “Um seletor” e “Dois sítios consumidores”, sem cobrar registro/corte do predicado nem incorporar 6/7. O texto específico da LEI-AKITA de `362c6d8a` não foi fornecido: seu conteúdo integral e correspondência ao hash ficam **[não verificado]**.

A cópia é anterior ao predicado novo: `app/ponto/precedencia.py:195` contém `fatos_do_dia`, e `app/ponto/precedencia.py:236`–238 ainda seleciona uma candidata com `setdefault`. Portanto, as linhas do opus sobre o novo predicado não devem ser apresentadas como verificação direta desta cópia.

## 31/10 19:02 · sessao 010bc3109982 · - · - · origem=Ronald (tmux)

Certeza **alta por leitura estática** nos itens abaixo; execução e ocorrência em produção **[não verificado]**.

- **D cura apenas exceções que chegam ao `except` externo.** `_so_modelos` engole `OSError`, `SyntaxError`, `UnicodeDecodeError` e continua; `_por_sitio` também engole `ValueError`, guarda fonte como `None` e continua. Portanto, fonte/modelo ilegível pode resultar em entidade vazia ou programa alternativo e ser persistido; o evento já persistido não é resolvido novamente. (`bin/disjuntor.py:1233`, `bin/disjuntor.py:1234`, `bin/disjuntor.py:1259`, `bin/disjuntor.py:1261`, `bin/disjuntor.py:1264`, `bin/disjuntor.py:1283`, `bin/disjuntor.py:1376`, `bin/disjuntor.py:1384`)

- **A foto da transição PDC está sob a mesma trava do estado.** `_com_estado` toma flock antes de executar `_fn`; `_fn` lê o RELATO inteiro e recorta o topo desses mesmos bytes. Fotografia e reconhecedor usam a mesma normalização; não existe segunda abertura do RELATO para fazer a foto. (`bin/disjuntor.py:260`, `bin/disjuntor.py:275`, `bin/disjuntor.py:1361`, `bin/disjuntor.py:1369`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1308`, `bin/disjuntor.py:1314`)

- **Redisparo refaz a foto nessa mesma trava.** Nova disparada redefine `fotos[ent] = None`, fotografa o topo observado e, ao encerrar episódio, apaga a foto. Estado anterior à v4 recebe fotografia tardia. (`bin/disjuntor.py:1390`, `bin/disjuntor.py:1392`, `bin/disjuntor.py:1396`, `bin/disjuntor.py:1429`, `bin/disjuntor.py:1430`)

- **Há leituras externas à trava que podem observar outro RELATO:** fallback de `estado_pdc`, ramo de exceção do PreToolUse, Stop/PAREI/ESPERANDO e checagem da prova do RELATO. Essas leituras não compõem a foto interna de `decide_pdc`. (`bin/disjuntor.py:1494`, `bin/disjuntor.py:1557`, `bin/disjuntor.py:2028`, `bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:429`, `bin/hook_stop_fila1.py:1176`)

- **Estabilidade observada não equivale a excluir escritores.** PDC usa `disjuntor_estado.json.lock`; `no_relato` usa flock do diretório de RELATO. Um escritor pode atualizar RELATO após o segundo stat e antes da persistência; a transição usa os bytes observados, sem reabrir. (`bin/disjuntor.py:256`, `bin/disjuntor.py:260`, `bin/disjuntor.py:711`, `bin/disjuntor.py:716`, `bin/disjuntor.py:281`, `app/core/esteira_vigia.py:844`, `app/core/esteira_vigia.py:848`)

- **C corrige a inversão concorrente do escopo entre transições cooperantes.** `_pdc_grava_escopo` é chamado no anúncio; `_com_estado` executa anúncio antes de fechar o descritor da trava. Entretanto, falha de publicação apenas registra FALHA e mantém o escopo anterior. (`bin/disjuntor.py:1439`, `bin/disjuntor.py:293`, `bin/disjuntor.py:296`, `bin/disjuntor.py:1466`, `bin/disjuntor.py:1467`)

- **Escritores RELATO fora da porta persistem:** `gerar_cortes` lê e regrava integralmente; `deploy_agendado` faz o mesmo. Não tomam o flock do diretório empregado por `no_relato`. O próprio escritor central documenta esses resíduos. (`bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:37`, `bin/deploy_agendado.sh:44`, `app/core/esteira_vigia.py:839`)

- **Quem escreve/lê topo:** o código reconhece topo escrito à mão, preservado pela inserção de alarmes. Negativas do disjuntor passam por `no_relato`; os leitores incluem PDC/B3, Stop, handoff e publicação. Autoria humana concreta **[não verificado]**. (`app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:872`, `bin/disjuntor.py:235`, `bin/disjuntor.py:236`, `bin/disjuntor.py:1361`, `bin/disjuntor.py:677`, `bin/hook_stop_fila1.py:429`, `bin/handoff_sessao.sh:92`, `bin/relato.sh:56`, `bin/relato.sh:104`)

- **`nao_sei`/`duvida` são apenas memória da decisão:** defaults na classe, preenchimento no retorno de `decide_pdc`, leitura por `estado_pdc`. Não são publicados no escopo: este só contém `em`, `disparadas`, `obras_com_evento`. Consequentemente, dúvida não integra o fallback persistido. (`bin/disjuntor.py:1341`, `bin/disjuntor.py:1342`, `bin/disjuntor.py:1449`, `bin/disjuntor.py:1451`, `bin/disjuntor.py:1505`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1461`, `bin/disjuntor.py:1491`, `bin/disjuntor.py:1493`)

- **Segundo juiz:** reuso confirmado do censo e do juiz de vocabulário; `_so_modelos` acrescenta filtro de herança ao catálogo amplo do censo. Não demonstrada duplicação dos juízes de negócio registrados; ausência absoluta **[não verificado]**. (`bin/disjuntor.py:1198`, `bin/disjuntor.py:1217`, `bin/disjuntor.py:1219`, `bin/disjuntor.py:1266`, `app/core/censo_escritas.py:90`, `app/core/censo_escritas.py:109`, `app/core/juizes.py:156`, `app/core/juizes.py:1255`)

- **Vocabulário tem implementação compartilhada, mas fontes concretas podem divergir:** PDC lê disco da raiz; selo LEI-AKITA lê documentos do commit por `git show`. (`bin/disjuntor.py:1332`, `bin/disjuntor.py:1334`, `bin/tests/test_lei_akita.sh:77`, `bin/tests/test_lei_akita.sh:89`)

- **Relógio fora da borda persiste:** decisões B3/PDC usam `time.time()` quando chamador não injeta `agora`; CLIs chamam sem injeção. Trilha e triagem leem seu próprio `datetime.now()`. (`bin/disjuntor.py:904`, `bin/disjuntor.py:1354`, `bin/disjuntor.py:1073`, `bin/disjuntor.py:1536`, `bin/disjuntor.py:140`, `bin/disjuntor.py:840`)

- **Leitura dupla fora do RELATO persiste:** censo lê models; filtro de subclasses relê os mesmos arquivos, e sítios citados têm uma terceira leitura independente. Mudança concorrente pode misturar catálogo, herança e sítio. Painel abre a trilha quatro vezes para eventos distintos. (`app/core/censo_escritas.py:105`, `bin/disjuntor.py:1233`, `bin/disjuntor.py:1259`, `bin/disjuntor.py:2085`, `bin/disjuntor.py:2088`)

## 31/10 19:01 · sessao f3ba881c7f8d · - · - · origem=Ronald (tmux)

**E: parcialmente resolvido, certeza alta por inspeção.**

- O mutante `B3-foto-parcial-engolida` agora importa `H` antes de usá-lo; elimina o `NameError` acidental específico. O mutante que retirava `if ent not in disp` foi removido e substituído pelos testes da fotografia. (`bin/tests/test_disjuntor.sh:1714`; `bin/tests/test_padrao_de_cima.sh:559`; `bin/tests/test_padrao_de_cima.sh:562`)
- Capturar uma das cinco exceções no próprio código **não basta para escondê-la**: a sonda registra `RAISE`, independentemente de um `except` posterior, desde que o código esteja dentro da cópia monitorada. (`bin/tests/test_disjuntor.sh:54`; `bin/tests/test_disjuntor.sh:57`; `bin/tests/test_disjuntor.sh:64`)
- **Exceções de outros tipos escapam da sonda.** A lista contém somente `NameError`, `KeyError`, `AttributeError`, `TypeError`, `IndexError`; `_morre` aceita qualquer retorno não zero se o log não acusar contaminação. Não comprova que houve falha de assert. (`bin/tests/test_disjuntor.sh:54`; `bin/tests/test_disjuntor.sh:77`; `bin/tests/test_disjuntor.sh:83`; `bin/tests/test_padrao_de_cima.sh:43`; `bin/tests/test_padrao_de_cima.sh:65`)
- **Contraexemplo estático:** inserir `raise ValueError(...)` no começo de `estado_pdc` causa saída não zero do Python; `i14_mesma` exige rc 3 e falha, enquanto a sonda ignora `ValueError`. O harness aceita essa morte como mordida, embora seja acidente de execução. Certeza alta por fluxo; execução desse mutante **[não verificado]**. (`bin/disjuntor.py:1481`; `bin/tests/test_padrao_de_cima.sh:124`; `bin/tests/test_padrao_de_cima.sh:150`; `bin/tests/test_padrao_de_cima.sh:43`; `bin/tests/test_padrao_de_cima.sh:65`)

**As três declarações não exigem que a exceção declarada ocorra.** `_excecao` retorna sucesso com log vazio; havendo log, apenas exclui linhas que contenham o texto declarado com `grep -v -F`. Se o caso passar, `_morre` sempre acusa sobrevivência; se falhar por outro motivo não monitorado, aceita a mordida mesmo sem exceção declarada. Portanto, não existe uma obrigação positiva de observar a exceção esperada. Certeza alta. (`bin/tests/test_disjuntor.sh:77`; `bin/tests/test_disjuntor.sh:79`; `bin/tests/test_disjuntor.sh:84`; `bin/tests/test_disjuntor.sh:1537`)

**Os prefixos perdoam sítios diferentes.** `cx8-ausente-sem-falha` aceita `AttributeError` em qualquer função de `bin/disjuntor.py`; `P7-tipo-solto` aceita qualquer `AttributeError` de `bin/statusline_cota.py`; `B3-ref-fora-da-pergunta` restringe à função `linhas_libera_b3`, mas não à linha ou operação esperada. O log só grava tipo, arquivo e nome da função; o filtro é substring fixa, sem ancoragem. Certeza alta. (`bin/tests/test_disjuntor.sh:62`; `bin/tests/test_disjuntor.sh:79`; `bin/tests/test_disjuntor.sh:1652`; `bin/tests/test_disjuntor.sh:1706`; `bin/tests/test_disjuntor.sh:1775`)

Os autotestes provam, pelo desenho, somente que o erro plantado é detectado e que declaração vazia/errada não o perdoa; não cobrem exceções fora da lista, ausência da exceção esperada ou exceção do mesmo tipo em outro sítio coberto pelo prefixo. Execução **[não verificado]**. (`bin/tests/test_disjuntor.sh:1839`; `bin/tests/test_padrao_de_cima.sh:592`)

**F: resolvido no selo para as lacunas apontadas, certeza alta por inspeção.**

- Agora o selo exige igualdade integral entre `PROGRAMAS` e o mapa extraído das três formas declaradas no BACKLOG; inclui chave plantada, remoção da O261, troca de programa e controle sem mutação. (`bin/tests/test_padrao_de_cima.sh:475`; `bin/tests/test_padrao_de_cima.sh:486`; `bin/tests/test_padrao_de_cima.sh:495`; `bin/tests/test_padrao_de_cima.sh:500`; `bin/tests/test_padrao_de_cima.sh:503`; `bin/tests/test_padrao_de_cima.sh:510`)
- A guarda de vocabulário já existia; a cura acrescenta casos com cada fonte ausente e mutantes retirando os respectivos `raise VocabIlegivel`. (`bin/padrao_vocab.py:45`; `bin/padrao_vocab.py:55`; `bin/tests/test_padrao_de_cima.sh:451`; `bin/tests/test_padrao_de_cima.sh:454`; `bin/tests/test_padrao_de_cima.sh:582`)
- Permanece duplicação de **cadastro**, não demonstração de segundo juiz: `PROGRAMAS` continua literal no runtime, com reconciliação feita somente pelo selo. Novas formas textuais de declaração fora das três regex não são reconhecidas automaticamente. (`bin/disjuntor.py:1111`; `bin/tests/test_padrao_de_cima.sh:475`; `bin/tests/test_padrao_de_cima.sh:479`; `bin/tests/test_padrao_de_cima.sh:483`)

As suítes criam, modificam e apagam arquivos; não foram executadas nesta auditoria. Resultado integral dos selos **[não verificado]**. (`bin/tests/test_disjuntor.sh:39`; `bin/tests/test_disjuntor.sh:41`; `bin/tests/test_padrao_de_cima.sh:29`; `bin/tests/test_padrao_de_cima.sh:30`)

## 31/10 19:01 · sessao 85d80daa26c4 · - · - · origem=Ronald (tmux)

**Achados por inspeção estática; certeza alta nos fluxos, ocorrência em produção [não verificado].**

| Tema | Evidência |
|---|---|
| **NAO_SEI normal é recusado** | `_nega_pdc` recusa qualquer resultado diferente de LIVRE; CLI retorna 4 para NAO_SEI; wrapper recusa qualquer rc não zero. (`bin/disjuntor.py:1522`, `bin/disjuntor.py:1540`, `bin/codex_pergunta.sh:168`) |
| **Fallback ainda transforma dúvida em LIVRE** | Se `decide_pdc` levanta por estado ilegível/trava, `estado_pdc` consulta apenas `esc['disparadas']`; fora dessa lista retorna LIVRE, sem consultar `em`, `obras_com_evento`, RELATO ou dúvida. O escopo não persiste `nao_sei`/`duvida`. (`bin/disjuntor.py:1487`, `bin/disjuntor.py:1491`, `bin/disjuntor.py:1493`, `bin/disjuntor.py:1460`) |
| **Cenário concreto do fallback** | Uma decisão com resolvedor falho pode gravar escopo sem a obra em `disparadas`, embora ela esteja em `obras_com_evento`; depois uma falha na abertura/trava/estado devolve LIVRE para ela. Mesmo com RELATO ilegível, `obras_com_evento=None` não impede esse LIVRE. **Inferência condicional alta.** (`bin/disjuntor.py:1380`, `bin/disjuntor.py:1383`, `bin/disjuntor.py:1439`, `bin/disjuntor.py:1461`, `bin/disjuntor.py:1493`) |
| **Escopo ainda pode ficar antigo** | Falha na escrita do escopo só registra FALHA; estado já publicado continua válido, escopo anterior permanece. Depois, o fallback não verifica frescor. **Inferência condicional alta.** (`bin/disjuntor.py:281`, `bin/disjuntor.py:293`, `bin/disjuntor.py:1465`, `bin/disjuntor.py:1467`, `bin/disjuntor.py:1490`) |
| **PreToolUse sem obra reconhecida** | Agent só chama `_nega_pdc` se `_obra_de(prompt)` reconhecer obra; a função pode retornar None. Nesse percurso não se pergunta PDC. (`bin/disjuntor.py:512`, `bin/disjuntor.py:520`, `bin/disjuntor.py:1978`) |
| **PreToolUse: segunda leitura fora da trava** | No envelope de exceção, `_ramo_pdc` lê novamente RELATO; só devolve obra se arquivo ilegível ou obra presente no texto dessa segunda foto. Se o evento sair entre as fotos, pode retornar None e chegar a “ferramenta liberada”. Não é a fotografia usada para liberar entidade: é a classificação do fallback. **Inferência condicional alta.** (`bin/disjuntor.py:1556`, `bin/disjuntor.py:1561`, `bin/disjuntor.py:2028`, `bin/disjuntor.py:2037`) |
| **Stop não pergunta PDC** | `_disjuntor_contido` trata B4, B2, `julga_b3`, B1; não chama `estado_pdc`/`decide_pdc`. Portanto não atribui LIVRE a PDC: simplesmente não apresenta a dúvida PDC. (`bin/hook_stop_fila1.py:1043`, `bin/hook_stop_fila1.py:1048`, `bin/hook_stop_fila1.py:1053`, `bin/hook_stop_fila1.py:1058`) |
| **Painel perde a dúvida** | `_pdc_painel` lê somente estado persistido, conta eventos e disparadas; não lê RELATO nem `DecisaoPDC`. Com estado legível pode mostrar zero disparadas apesar de RELATO ilegível/resolvedor falho. Não apresenta literalmente LIVRE, mas omite NAO_SEI. (`bin/disjuntor.py:1564`, `bin/disjuntor.py:1567`, `bin/disjuntor.py:1575`, `bin/disjuntor.py:2220`) |
| **Wrapper: papéis e chave** | Gate só para `PAPEL=verificador`; leitura/censo/sonda não consultam PDC. `padrao_parada` devolve zero com chave desligada. (`bin/codex_pergunta.sh:164`, `bin/codex_pergunta.sh:168`, `bin/disjuntor.py:1534`) |
| **Hooks apontam árvore viva** | Configuração usa caminhos absolutos `/home/ronald/saas-hasner/…`, tanto Stop quanto PreToolUse. Execução do hash auditado em produção [não verificado]. (`.claude/settings.json:9`, `.claude/settings.json:68`) |

**Travas e relógio — certeza alta.**

- Wrapper: gate FIFO → slot → solta gate → resposta exclusiva → estado B3 → estado PDC. Triagem: resposta compartilhada → estado. Não localizei inversão nesses percursos; ausência absoluta em toda árvore [não verificado]. (`bin/codex_pergunta.sh:138`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:153`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/disjuntor.py:813`, `bin/disjuntor.py:845`)
- Trava de estado tenta por aproximadamente um segundo e depois levanta; esse caso chega ao fallback de escopo. (`bin/disjuntor.py:258`, `bin/disjuntor.py:263`, `bin/disjuntor.py:265`, `bin/disjuntor.py:1487`)
- PreToolUse injeta relógio de borda; CLI `padrao_parada` não injeta, fazendo `decide_pdc` ler relógio internamente. Trilha tem seu próprio relógio. (`bin/disjuntor.py:2007`, `bin/disjuntor.py:1979`, `bin/disjuntor.py:1536`, `bin/disjuntor.py:1354`, `bin/disjuntor.py:140`)
- Painel abre `disjuntor.log` quatro vezes independentes — NEGOU/FALHA/DISJUNTOR/LIBERA — e pode combinar instantes distintos. (`bin/disjuntor.py:2085`, `bin/disjuntor.py:2088`)

**Escritores/leitores fora do diff — certeza alta.**

- Estado comum: `_com_estado` publica por replace sob flock; `ler_estado` lê sem flock. A leitura ganha arquivo inteiro por replace, sem garantir mesma observação das outras fontes. (`bin/disjuntor.py:260`, `bin/disjuntor.py:275`, `bin/disjuntor.py:281`, `bin/disjuntor.py:299`)
- Negativas PreToolUse: `trilha('NEGOU')`, resposta deny e depois `relato_negacao`; este chama `esteira_vigia.no_relato`. (`bin/disjuntor.py:2014`, `bin/disjuntor.py:2017`, `bin/disjuntor.py:234`, `bin/disjuntor.py:236`)
- Escopo: `_anuncia` chama `_pdc_grava_escopo` ainda sob `_com_estado`; consumidor produtivo localizado é fallback de `estado_pdc`. (`bin/disjuntor.py:1439`, `bin/disjuntor.py:293`, `bin/disjuntor.py:1489`)
- `nao_sei`/`duvida`: atribuídos no objeto retornado; `estado_pdc` consome; painel e Stop não consomem esse objeto. (`bin/disjuntor.py:1447`, `bin/disjuntor.py:1449`, `bin/disjuntor.py:1451`, `bin/disjuntor.py:1505`, `bin/disjuntor.py:1567`, `bin/hook_stop_fila1.py:1048`)

**argv — certeza alta quanto à forma; conteúdo sensível efetivo [não verificado].**

Os gates passam `$OBRA` em argv; `CODEX_OBRA` só exige string não vazia e não recebe raspagem. Pergunta/anexos chegam ao `codex exec` por stdin, via `$CAB`. O ID também entra cru no cabeçalho e nos logs; portanto “só OBRA” limita a superfície, mas não prova ausência de dado sensível se o valor do ambiente estiver contaminado. (`bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/codex_pergunta.sh:88`, `bin/codex_pergunta.sh:121`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`, `bin/codex_pergunta.sh:178`)

_1211 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
