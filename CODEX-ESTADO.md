# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 10:35** · publicado em **06/11/2026 10:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 10:35 · sessao 63c4f7796b6e · PEP-REENTRADA · verificador · origem=Code

Parecer estático: execução dos testes, dos mutantes e comportamento em produção `[nao verificado]`. [nao verificado]

**1. P1–P4: cura e mutantes sobreviventes**

| Item | Parecer |
|---|---|
| **P1** | **Cura implementada para observar entrada no serviço durante o voo:** o espia registra `pergunta_em_voo(p.pk)` e exige pelo menos um `True`. Ele não identifica exclusivamente o receiver que originou a chamada. `app/chamados/tests/test_pep_reentrada.py:90`, `app/chamados/tests/test_pep_reentrada.py:97`, `app/chamados/tests/test_pep_reentrada.py:99`. |
| **P2** | **Curado quanto ao estado persistido do trio nos dois alvos**, mas **parcial quanto à exigência “dentro do voo”**: sem credor verifica depois de sair do `with`; S144 verifica ainda dentro. `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/tests/test_pep_reentrada.py:256`, `app/chamados/tests/test_pep_reentrada.py:259`, `app/chamados/tests/test_pep_reentrada.py:274`, `app/chamados/tests/test_pep_reentrada.py:278`. |
| **P3** | **O mutante original passa a ser distinguido:** ambas sem destino atravessariam `if q.chamado_destino_id: continue`, inclusive a P protegida; a exigência de P sem carimbo o reprovaria. A alteração, porém, muda o ingresso da disputa em `_cands`. `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/tests/test_pep_reentrada.py:259`, `app/chamados/services/disputa_emissao.py:1652`. |
| **P4** | **Curado:** C7 e C9 agora coincidem com registro e mapa. `app/docs/JUIZES.md:54`, `app/docs/JUIZES.md:55`, `app/core/juizes.py:267`, `app/core/juizes.py:269`, `app/docs/MAPA-JUIZES.md:97`, `app/docs/MAPA-JUIZES.md:105`. |

Mutantes de produção que os testes curativos ainda não distinguem — sobrevivência inferida pela leitura; execução `[nao verificado]`: [nao verificado]

- **P1:** substituir o corpo do resolver por retorno de contadores zerados. O espia continuaria registrando entrada em voo; o próprio materializador fecharia o destino e produziria seu carimbo. O teste exige entrada, destino fechado, batida e um `carimbar` bem-sucedido pela via da declaração, sem exigir efeito exclusivo do serviço observado. `app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:101`, `app/chamados/tests/test_pep_reentrada.py:103`, `app/chamados/tests/test_pep_reentrada.py:105`, `app/chamados/services/materializacao.py:949`, `app/chamados/services/materializacao.py:1251`.
- **P2:** nas guardas de voo dos dois alvos, executar `descarimbar()` antes de `continue`. Haveria escrita indevida do trio, mas os valores continuariam exatamente os vazios esperados. Portanto, conferir estado intacto não prova ausência de escritor. `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`, `app/chamados/models.py:2646`, `app/chamados/models.py:2652`, `app/chamados/tests/test_pep_reentrada.py:260`, `app/chamados/tests/test_pep_reentrada.py:279`.
- **P3:** trocar a guarda por `if pergunta_em_voo(q.pk) and not q.chamado_destino_id: continue`. O novo caso permanece protegido, mas perde-se a proteção da P em voo **com destino** no laço sem credor. O laço admite destinos encerrados; só um destino vivo barra a disputa. `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/services/disputa_emissao.py:1662`, `app/chamados/services/disputa_emissao.py:1671`.

**2. O espia de P1 está no nome correto?**

**Sim.** O receiver faz import local de `chamados.models.resolver_perguntas_por_chamado_resolvido` e chama esse nome; é precisamente o atributo substituído pelo teste. Não há outro ramo do próprio receiver que chame o resolver por um nome diferente. `app/chamados/signals.py:89`, `app/chamados/signals.py:92`, `app/chamados/tests/test_pep_reentrada.py:97`.

Há outros **chamadores do serviço**, também pelo nome observado: `_retratar`, reatamento de órfã e fechamento de beco. Eles não escapam do espia; podem, contudo, tornar ambígua a atribuição “foi o receiver S143”. Quais efetivamente chamam durante esse fixture, inclusive com S143 desligado, `[nao verificado]`. `app/chamados/reconciliador.py:508`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1528`, `app/chamados/reconciliador.py:1631`, `app/chamados/services/becos.py:40`, `app/chamados/services/becos.py:49`.

O registro ocorre **antes** de executar o serviço, e o receiver engole exceções: o `True` comprova entrada, não conclusão bem-sucedida. `app/chamados/tests/test_pep_reentrada.py:91`, `app/chamados/tests/test_pep_reentrada.py:92`, `app/chamados/signals.py:93`.

**3. P3 mantém o caminho anterior?**

**Mantém o laço, mas muda o caminho de entrada.** Antes, a P destinada também colocava a disputa no conjunto derivado de `perguntas`; agora ambas estão sem destino, o filtro inicial fica vazio para elas e a disputa entra pela consulta das abertas do colaborador. `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/services/disputa_emissao.py:1572`, `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1653`.

Pai resolvido e validações nulas permitem alcançar `_abertas`; o controle da irmã exige que alguma escrita realmente aconteça nesse laço. Portanto, a nova configuração não torna a guarda inatingível. `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:251`, `app/chamados/tests/test_pep_reentrada.py:258`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1665`.

O limite de realismo continua relevante: o fixture mantém texto, apaga resposta/validação, declara voo manualmente e força C6 a “morta”. A porta normal seleciona perguntas com resposta e validação gravadas; com C6 real, o próprio docstring declara que a irmã não receberia o carimbo. Isso prova a guarda isolada e perde cobertura do ingresso anterior por destino; não demonstra a cadeia real nesse estado. `app/chamados/tests/test_pep_reentrada.py:238`, `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/services/materializacao.py:1343`.

**4. P4: correspondência documental**

| Autoridade | JUIZES | Registro | MAPA |
|---|---|---|---|
| C7: `DisputaSupervisao.pode_fechar` | `app/docs/JUIZES.md:54` | `app/core/juizes.py:267` | `app/docs/MAPA-JUIZES.md:97` |
| C9: `catalogo/motor.py::prazo_estourou` | `app/docs/JUIZES.md:55` | `app/core/juizes.py:269` | `app/docs/MAPA-JUIZES.md:105` |

**5. Classes de reprovação no delta**

| Classe | Resultado da leitura |
|---|---|
| **Segundo juiz** | O voo é pertencimento por PK condicionado a atomic aberto; não recalcula vitalidade nem fechamento. C6 continua na porta do carimbo e C7 na cauda. `app/chamados/services/materializacao.py:1129`, `app/chamados/models.py:2596`, `app/chamados/services/disputa_emissao.py:1712`. |
| **Segundo escritor fora da porta** | As guardas novas apenas pulam os candidatos. Entretanto, o espia principal conta somente chamadas de `carimbar` que retornam `True`: uma escrita direta seguida de sobrescrita pelo materializador pode escapar dessa contagem. `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/materializacao.py:1399`, `app/chamados/tests/test_pep_reentrada.py:69`, `app/chamados/tests/test_pep_reentrada.py:105`. |
| **Relógio fora da borda** | O sinal novo não usa relógio; o fixture congela o instante. `app/chamados/services/materializacao.py:1115`, `app/chamados/services/materializacao.py:1129`, `app/chamados/tests/test_pep_reentrada.py:35`. |
| **Derivação paralela** | O sinal guarda PKs; os carimbos continuam delegando aos juízes existentes. `app/chamados/services/materializacao.py:1120`, `app/chamados/services/materializacao.py:1207`, `app/chamados/models.py:2596`. |
| **Selo sem mordida** | Há controles positivos, mas persistem os limites de origem do espia, estado versus escrita e mudança de caminho de P3 descritos acima. `app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:258`, `app/chamados/tests/test_pep_reentrada.py:281`. |
| **Ordem de trava** | A mudança do bloco acrescenta o contexto depois do atomic; o contexto só manipula `ContextVar`. Validação concorrente da ordem de travas `[nao verificado]`. `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1120`. |
| **Pouso antes do pré-requisito** | O atomic entra antes do voo; declarar voo fora de atomic levanta erro. O carimbo normal permanece depois do plantio e do juiz do lastro. `app/chamados/services/materializacao.py:1118`, `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1251`. |
| **Estado de módulo que sobrevive** | Existe `ContextVar` de módulo, mas o valor anterior é restaurado por token em `finally`; há testes de rollback, exceção e aninhamento. Passagem desses testes `[nao verificado]`. `app/chamados/services/materializacao.py:1106`, `app/chamados/services/materializacao.py:1124`, `app/chamados/tests/test_pep_reentrada.py:150`, `app/chamados/tests/test_pep_reentrada.py:167`, `app/chamados/tests/test_pep_reentrada.py:179`. |

**6. Quem escreve e quem lê o trio dentro do voo?**

**Nos caminhos atuais examinados, não identifiquei escritor da P além do materializador. Essa garantia está nos caminhos, não nas portas genéricas.** `app/chamados/services/materializacao.py:1251`, `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`.

| Papel | Caminho |
|---|---|
| **Escritor normal** | `_materializar_uma → carimbar → _escrever_carimbo → save`, atribuindo o trio junto. `app/chamados/services/materializacao.py:1251`, `app/chamados/models.py:2556`, `app/chamados/models.py:2609`. |
| **Escritor alternativo do próprio materializador** | A recusa por paridade usa `carimbar_identidade` e retorna `None`, impedindo o carimbo normal posterior. `app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:900`, `app/chamados/services/materializacao.py:1201`. |
| **S143/S144** | Os dois laços de S143 pulam a P; sua cauda não fecha disputa com P em voo; S144 também pula antes de carimbar. `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`, `app/chamados/services/materializacao.py:1399`. |
| **Reconciliador** | A batida o aciona sincronamente. A absolvição por célula pode carimbar, mas exige validação nula, excluindo a P selecionada pelos chamadores atuais; a lavra sem turno exige ausência de texto. `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1345`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/reconciliador.py:364`. |
| **Dedup/descarimbo** | Migração considera perguntas vivas, enquanto C6 elimina imediatamente a validada; ressuscitação exige carimbo existente e texto vazio. `app/chamados/services/fio.py:935`, `app/chamados/juizes.py:1136`, `app/chamados/models.py:1434`. |
| **Leitores** | A porta fotografa o trio anterior; C6 e lastro leem materialização/via; S143, S144 e irmãs sem consumo usam materialização nos filtros. O autor é preservado no snapshot. `app/chamados/models.py:2583`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:647`, `app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/materializacao.py:1393`, `app/chamados/juizes.py:1748`. |

A recusa por colisão no `finally` pode escrever identidade **depois da saída do voo**; ela não constitui outro escritor dentro desse bloco. Já `carimbar_identidade` e `descarimbar` continuam sem consultar o voo: ampliar os chamadores para estados como o fixture de P3 exigiria nova análise de alcançabilidade. `app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1261`, `app/chamados/services/materializacao.py:1271`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`, `app/chamados/tests/test_pep_reentrada.py:234`.

## 06/11 10:33 · sessao 083c8ea8b2b0 · - · - · origem=Ronald (tmux)

No bloco atual, não encontrei escritor do trio da P além do materializador; a garantia depende dos caminhos atuais, pois as portas genéricas não consultam o voo (`app/chamados/services/materializacao.py:1194`, `app/chamados/models.py:2562`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`).

**Quem escreve:**

- O carimbo normal sai de `_materializar_uma → p.carimbar`, após o juiz do lastro; a porta atribui os três campos e grava (`app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1251`, `app/chamados/models.py:2556`, `app/chamados/models.py:2609`).
- O próprio ramo hora pode escrever identidade por paridade e retornar imediatamente, impedindo o carimbo normal; continua sendo o materializador (`app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:900`, `app/chamados/services/materializacao.py:1201`).
- A recusa `COLISAO` pode escrever identidade no `finally`, mas esse `finally` roda após a saída de `materializacao_em_voo`; portanto não é outro escritor **dentro** do voo (`app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1261`, `app/chamados/services/materializacao.py:1271`).

**Outros candidatos e por que não escrevem a P atual:**

- S143 pula a P nos dois laços e bloqueia o fecho da disputa com qualquer P em voo (`app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`).
- S144 pula antes do juiz e do carimbo (`app/chamados/services/materializacao.py:1399`, `app/chamados/services/materializacao.py:1405`).
- A batida chama sincronamente `reconciliar_chamados_colab`; sua absolvição por célula carimba sem consultar voo, mas exige `validada_em IS NULL`. A porta normal seleciona validação não nula; a validação individual grava antes de materializar; o passivo também seleciona validação não nula (`app/ponto/signals.py:206`, `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1343`, `app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:424`, `app/chamados/services/vigia_carimbo.py:98`).
- `lavrar_sem_turno_aberto` exige ausência de texto, incompatível com hora que chega ao plantio e com `data_hora` parseável (`app/chamados/reconciliador.py:364`, `app/chamados/reconciliador.py:405`, `app/chamados/services/materializacao.py:804`, `app/chamados/services/materializacao.py:970`).
- Dedup só processa perguntas vivas; o C6 dá por morta imediatamente a P validada (`app/chamados/services/fio.py:931`, `app/chamados/services/fio.py:935`, `app/chamados/juizes.py:1136`).
- Ressuscitação/descarimbo filtra pergunta já materializada e sem texto; a P hora em voo tem texto (`app/chamados/models.py:1434`, `app/chamados/models.py:1437`, `app/chamados/models.py:1504`, `app/chamados/services/materializacao.py:804`).
- Re-lavra de via não está ligada ao sinal da batida; `_gravar_estado` re-lavra somente o veredito, não o trio (`app/ponto/signals.py:94`, `app/chamados/models.py:291`, `app/chamados/veredito_lavrado.py:227`).

**Quem lê:**

- `carimbar` fotografa o trio anterior e escreve pela porta única; C6 e juiz do lastro leem materialização/via; a lavra delega ao juiz do lastro (`app/chamados/models.py:2583`, `app/chamados/models.py:2586`, `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1207`, `app/chamados/juizes.py:647`, `app/chamados/juizes.py:673`, `app/chamados/veredito_lavrado.py:219`).
- Os universos de S143, S144, irmãs sem consumo e reconciliador filtram `materializada_em`; o autor é preservado no snapshot e atribuído pela porta, sem participar desses predicados (`app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/disputa_emissao.py:1666`, `app/chamados/services/materializacao.py:1393`, `app/chamados/juizes.py:1748`, `app/chamados/reconciliador.py:619`, `app/chamados/models.py:2583`, `app/chamados/models.py:2559`).

O limite estrutural merece constar: uma chamada nova de `_materializar_uma` com validação nula poderia entrar no universo do reconciliador, porque ele não consulta voo; os chamadores atuais evitam isso pelos filtros acima, e o próprio teste P3 declara esse limite (`app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1132`, `app/chamados/tests/test_pep_reentrada.py:234`).

## 06/11 10:33 · sessao d90ff57f7b2e · - · - · origem=Ronald (tmux)

- **Item 2:** o patch de P1 entra no nome correto: o teste troca `chamados.models.resolver_perguntas_por_chamado_resolvido` em `app/chamados/tests/test_pep_reentrada.py:97`; o receiver faz import local desse mesmo nome e o chama em `app/chamados/signals.py:89` e `app/chamados/signals.py:92`; o reexport vem de `app/chamados/models.py:3281`. Não há outro ramo do próprio receiver que execute o resolver sem passar por esse alias (`app/chamados/signals.py:81`, `app/chamados/signals.py:88`).

- **Limite de P1:** o espia comprova entrada no serviço durante o voo, mas não identifica qual chamador entrou nem comprova conclusão do serviço: só registra `pergunta_em_voo(p.pk)` antes de chamar o original (`app/chamados/tests/test_pep_reentrada.py:90`); o receiver engole exceções (`app/chamados/signals.py:93`). Há chamadas diretas pelo mesmo alias, também vistas pelo espia, em `app/chamados/reconciliador.py:508`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1528`, `app/chamados/reconciliador.py:1631`, `app/chamados/services/becos.py:40` e `app/chamados/services/becos.py:49`. Quais efetivamente acontecem no fixture: `[nao verificado]`.

- **Mutante sobrevivente de P1, no teste que o cura:** substituir o serviço por retorno `{'perguntas_resolvidas': 0, 'disputas_fechadas': 0}` preservaria a chamada observada em voo e deixaria o materializador produzir seu carimbo único; o método exige apenas ocorrência de `True`, destino fechado, batida presente e uma gravação com `VIA_DECLARACAO` (`app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:101`, `app/chamados/tests/test_pep_reentrada.py:103`, `app/chamados/tests/test_pep_reentrada.py:105`; carimbo do materializador em `app/chamados/services/materializacao.py:1251`). Resultado executado do mutante: `[nao verificado]`.

- **Item 3:** P3 continua atingindo o mesmo laço-alvo, mas **não pelo mesmo ingresso em `_cands`**: antes P apontava para `destino`; agora remove a FK em `app/chamados/tests/test_pep_reentrada.py:250`. Como a irmã também nasce sem destino (`app/chamados/tests/test_pep_reentrada.py:244`), o filtro `chamado_destino=chamado` fica sem essas perguntas (`app/chamados/services/disputa_emissao.py:1572`). A disputa entra pela busca de todas as abertas do colaborador (`app/chamados/services/disputa_emissao.py:1653`), enquanto antes também entrava pelo conjunto derivado de `perguntas` (`app/chamados/services/disputa_emissao.py:1652`). Pai resolvido e validações nulas satisfazem os filtros do laço (`app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:251`; filtros em `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1665`).

- **P3 mata o mutante original:** trocar a guarda por `if q.chamado_destino_id: continue` deixaria ambas passarem, inclusive P em voo, contrariando a exigência de P sem carimbo (`app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/tests/test_pep_reentrada.py:259`; ponto de mutação em `app/chamados/services/disputa_emissao.py:1671`). Execução: `[nao verificado]`.

- **Mutante sobrevivente de P3:** `if pergunta_em_voo(q.pk) or q.chamado_destino_id: continue` mantém o teste verde porque ambas estão sem destino, mas passa a pular indevidamente irmãs fora de voo com destino morto; esse universo é aceito pelo laço de produção, que só exclui a disputa quando existe destino **vivo** (`app/chamados/services/disputa_emissao.py:1662`, `app/chamados/services/disputa_emissao.py:1671`; fixture em `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`). Execução: `[nao verificado]`.

- **Realismo de P3:** o limite de C6 mockado está agora declarado explicitamente (`app/chamados/tests/test_pep_reentrada.py:238`); a modificação adicional perde cobertura do ingresso por pergunta com destino, mas não impede atingir a guarda do laço sem credor (`app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1671`). O caso continua sintético: remove carimbos de resposta/validação mantendo o texto e declara voo manualmente (`app/chamados/tests/test_pep_reentrada.py:245`, `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:254`); portanto sua evidência é da guarda, não da cadeia real de materialização (`app/chamados/tests/test_pep_reentrada.py:239`).

## 06/11 10:33 · sessao 8b3b6fa3ee26 · - · - · origem=Ronald (tmux)

P4 está curado: `app/docs/JUIZES.md:54` aponta C7 para `DisputaSupervisao.pode_fechar`, como `app/core/juizes.py:267` e `app/docs/MAPA-JUIZES.md:97`; SLA/C9 em `JUIZES.md:55` coincide com `core/juizes.py:269` e `MAPA-JUIZES.md:105`.

P2 está curado quanto ao **estado persistido do trio** nos dois cenários pedidos: sem credor verifica `materializada_em` e `(via_resolucao, materializada_por_id)` em `app/chamados/tests/test_pep_reentrada.py:259` e `:260`; S144 verifica em `:278` e `:279`. Contudo, sem credor lê depois de sair do bloco (`:253`, `:256`); somente S144 lê dentro (`:274`, `:276`). A afirmação literal “trio intacto dentro do voo nos dois testes” é mais forte que o observado no primeiro (`:253`, `:256`).

Mutante substantivo sobrevivente aos **dois testes curativos de P2**: estreitar as respectivas guardas para `if pergunta_em_voo(pk) and p.tipo_resposta == 'hora': continue`. Os fixtures são todos `hora` (`app/chamados/tests/test_pep_reentrada.py:57`, `:245`, `:270`), mas produção põe também `data_hora` e `escolha` em voo (`app/chamados/services/materializacao.py:1194`, `:1197`, `:1199`); as guardas atuais não restringem tipo (`app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`). Sobrevivência inferida estaticamente, sem executar mutação: `[nao verificado]`.

Outro limite: assertar o trio vazio não prova ausência de escrita. Um `descarimbar()` na guarda grava o mesmo trio vazio (`app/chamados/models.py:2646`, `:2652`), já esperado pelos testes (`app/chamados/tests/test_pep_reentrada.py:260`, `:279`); os valores iniciais vêm dos defaults (`app/chamados/models.py:2323`, `:2791`, `:2795`). Isso é uma limitação do selo por estado, sem demonstrar defeito no código atual (`app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`).

Nas classes solicitadas, o delta v4 acrescenta observação/assertions e corrige lookup, sem introduzir decisão de produção: `app/chamados/tests/test_pep_reentrada.py:90`, `:97`, `:250`, `:260`, `:279`; `app/docs/JUIZES.md:54`. No diff completo, o sinal não replica C6/C7: consulta apenas pertencimento e transação (`app/chamados/services/materializacao.py:1127`), enquanto carimbo e fecho continuam lendo os juízes existentes (`app/chamados/models.py:2596`, `app/chamados/services/disputa_emissao.py:1712`). O reset está em `finally`, com token (`app/chamados/services/materializacao.py:1120`, `:1123`); há cobertura explícita de rollback, exceção e aninhamento (`app/chamados/tests/test_pep_reentrada.py:150`, `:167`, `:179`). O bloco entra primeiro no atomic e depois declara voo (`app/chamados/services/materializacao.py:1194`); a declaração exige atomic aberto (`:1118`). Não há evidência fornecida de execução dos testes ou de validação concorrente de ordem de travas: `[nao verificado]`.

## 06/11 10:32 · sessao 6d8557c6b3d4 · H12H06-B · sonda · origem=Code

Transcrição literal do dado fornecido [nao verificado]. Não executei o wrapper: ele escreve arquivos (`bin/sonda_leitura.sh:132`, `bin/sonda_leitura.sh:250`).

```text
sonda_leitura: pago_x_devido.py [b04491167a9f3c6b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

col|status_fm|entregue|grav_ok|grav_perd|grav_reflexo|antes_diff(ok,perd,refl)|devido(ok,perd,refl)|grav==antes
col159|aberto|False|4|1|0.9943|(4, 1, None)|(5, 0, None)|True
col284|aprovado|True|4|1|0.1914|(4, 1, None)|(5, 0, None)|True
col342|aprovado|True|4|1|2.4046|(4, 1, 1.9195)|(5, 0, 3.2714)|False
col400|aprovado|True|4|1|1.2723|(4, 1, 0.9864)|(5, 0, 1.2125)|False
col433|aberto|True|2|3|0.0000|(2, 3, None)|(4, 1, None)|True
col842|aberto|True|3|1|1.3746|(3, 1, 1.3849)|(4, 0, 1.7164)|False
col905|aberto|True|0|1|0.0000|(0, 1, None)|(1, 0, None)|True
UNIVERSO 7 esperado 7
```

(1) UNIVERSO fecha: **sim, 7 esperado 7**, conforme impresso. [nao verificado]

(2) e (3), conforme impresso:

| col | grav==antes | entregue | Referência |
|---|---|---|---|
| col159 | True | False | [nao verificado] |
| col284 | True | True | [nao verificado] |
| col342 | False | True | [nao verificado] |
| col400 | False | True | [nao verificado] |
| col433 | True | True | [nao verificado] |
| col842 | False | True | [nao verificado] |
| col905 | True | True | [nao verificado] |

## 06/11 10:29 · sessao cf215076862f · H12H06-B · sonda · origem=Code

Não executei `dry_comp11.py`: o arquivo não foi localizado na cópia. Segue a saída literal fornecida no pedido. [nao verificado] [nao verificado]

```text
sonda_leitura: dry_comp11.py [6be44cbafd75e60b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

pk|col|tipo|data|data_fim|status|grav|decl|jornada|novo|novo==jornada(falta)/0(atestado)|cerca|veredito
5096|col940|atestado|2026-10-21|-|aguardando_documento|720|-|480|0|True|-|mudaria
5124|col940|atestado|2026-10-22|-|aguardando_documento|720|-|480|0|True|-|mudaria
5184|col723|atestado|2026-10-23|-|aguardando_documento|720|-|440|0|True|-|mudaria
5206|col558|atestado|2026-10-24|-|aguardando_documento|720|-|720|0|True|-|mudaria
5305|col493|atestado|2026-10-28|-|aguardando_documento|720|-|530|0|True|-|mudaria
5332|col735|atestado|2026-10-29|-|aguardando_decisao|720|-|540|0|True|-|mudaria
5334|col371|atestado|2026-10-30|-|aguardando_documento|720|-|420|0|True|-|mudaria
5336|col363|atestado|2026-10-22|-|aguardando_documento|720|-|480|0|True|-|mudaria
5345|col288|atestado|2026-10-30|-|aguardando_decisao|720|-|660|0|True|-|mudaria
5348|col723|atestado|2026-10-31|-|aguardando_documento|720|-|440|0|True|-|mudaria
5349|col723|atestado|2026-10-30|-|aguardando_documento|720|-|440|0|True|-|mudaria
5366|col455|atestado|2026-11-01|-|rejeitada|720|-|720|0|True|-|pulada:identidade_mudou
5128|col967|atestado|2026-10-21|-|aprovada|720|-|440|0|True|-|mudaria
5182|col200|atestado|2026-10-24|2026-10-25|aprovada|720|-|840|0|True|-|mudaria
5197|col701|atestado|2026-10-23|-|aprovada|720|-|480|0|True|-|mudaria
5236|col891|atestado|2026-10-27|-|aprovada|720|-|720|0|True|-|mudaria
5198|col444|falta|2026-10-24|-|aprovada|720|-|420|420|True|-|mudaria
5335|col905|falta|2026-10-30|-|aprovada|720|-|660|660|True|-|mudaria
5339|col304|falta|2026-10-29|-|aprovada|720|-|555|555|True|-|mudaria
5340|col304|falta|2026-10-28|-|aprovada|720|-|555|555|True|-|mudaria
5351|col823|falta|2026-11-02|-|aprovada|720|-|240|240|True|-|mudaria
5355|col869|falta|2026-11-02|-|aprovada|720|-|480|480|True|-|mudaria
5357|col924|falta|2026-10-26|-|aprovada|720|-|440|440|True|-|mudaria
5358|col924|falta|2026-11-01|-|aprovada|720|-|440|440|True|-|mudaria
5359|col924|falta|2026-10-28|-|aprovada|720|-|440|440|True|-|mudaria
5360|col924|falta|2026-10-27|-|aprovada|720|-|440|440|True|-|mudaria
5369|col924|falta|2026-10-29|-|aprovada|720|-|440|440|True|-|mudaria
5333|col145|atestado|2026-10-30|2026-12-28|rejeitada|720|-|19800|0|True|-|pulada:identidade_mudou
5353|col769|falta|2026-11-01|-|aprovada|720|-|660|660|True|-|mudaria
5311|col33|atestado|2026-10-29|-|aguardando_decisao|720|-|660|0|True|-|mudaria
UNIVERSO 30 esperado 30
RESUMO mudaria=28 ja_aplicada=0 pulada=2 parcial_dp=0 erros=0
SOMA 30 == 30: True
```

1. No dado fornecido, SOMA e UNIVERSO fecham: `30 == 30: True` e `30 esperado 30`. [nao verificado]
2. Vereditos diferentes de `mudaria`, por pk: [nao verificado]
   - `5333`: `pulada:identidade_mudou`. [nao verificado]
   - `5366`: `pulada:identidade_mudou`. [nao verificado]
3. Nenhuma linha fornecida tem `novo==jornada(falta)/0(atestado)` igual a `False`. [nao verificado]

## 06/11 10:26 · sessao faf686f2cce0 · CAUSA-D-ESTRUTURA · sonda · origem=Code

**O raspado fecha os 200 dias; os três grupos chegam a ESTRUTURA pelo mesmo caminho residual. Ele não demonstra três causas materiais distintas.** A diferença medida entre os grupos é a cardinalidade par; os outros predicados informados são iguais. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`; valores recebidos: [nao verificado])

**1. Saída e conferência**

Esta é a saída recebida, não uma reexecução minha. O conteúdo de `sonda_causa.py`, sua correspondência com o hash `688ae6ab76dea90a` e a execução pelo envelope não foram conferidos independentemente. [nao verificado]

| Conferência do raspado | Resultado | Evidência |
|---|---:|---|
| `SEM_PORTA_CLASSE_D` | 217 | Anexo [nao verificado] |
| `alcance_dias_omitidos` | 0 | Anexo [nao verificado] |
| `UNIVERSO_DONO_ESTRUTURA` | **200 / esperado 200 — CONFERE** | Anexo [nao verificado] |
| Fora do universo | BATIDA 3; CADASTRO 14 | Anexo [nao verificado] |
| Reconciliação da classe d | 200 + 3 + 14 = **217** | Cálculo sobre o anexo [nao verificado] |
| `SOMA_DOS_GRUPOS` | 182 + 13 + 5 = **200 — True** | Cálculo sobre o anexo [nao verificado] |
| `rc_sombra` | **0** | Anexo [nao verificado] |
| `INCOMPLETO` / erros impressos | Nenhum consta no trecho recebido | Anexo [nao verificado] |

| Grupo | n | Cadastro: folga indefinida / sem vínculo / sem vínculo em trabalho | Turno | Batidas | Fase lavrada | CXR do motor | Evidência |
|---|---:|---|---|---|---|---|---|
| 1 | **182** | False / False / False | não aberto | 0, par | False | False | Anexo [nao verificado] |
| 2 | **13** | False / False / False | não aberto | 4+, par | False | False | Anexo [nao verificado] |
| 3 | **5** | False / False / False | não aberto | 2, par | False | False | Anexo [nao verificado] |
| **SOMA** | **200** | | | | | | Cálculo sobre o anexo [nao verificado] |

| MARGINAL | Distribuição | Evidência |
|---|---|---|
| P1 — cadastro | três flags False: **200** | Anexo [nao verificado] |
| P2 — batida fora de turno | `turno_nao_aberto`: **200** | Anexo [nao verificado] |
| P3 — paridade | 0: **182**; 4+ pares: **13**; 2: **5** | Anexo [nao verificado] |
| P4 — fase | `fase_lavrada=False`: **200** | Anexo [nao verificado] |
| P5 — CXR | `em_cxr_do_motor=False`: **200** | Anexo [nao verificado] |

**2. Quais predicados deixaram os dias no resíduo**

A montagem real é `InsumosDoDono.dono`: carrega cadastro, consulta `batida_fora_de_turno`, lê a fase e pergunta a pertença à CXR do motor. A paridade usa `oraculo.horas`. ([e6_oraculo.py:172](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/ponto/management/commands/e6_oraculo.py:172), `app/ponto/management/commands/e6_oraculo.py:144`)

Esta cadeia vale **para cada um dos três grupos**, com a variação de P3 explicitada:

| Predicado real | Valor recebido | Por que não captura o dia |
|---|---|---|
| P1 — `folga_nao_definida or sem_vinculo_no_dia or sem_vinculo_em_dia_de_trabalho` | False / False / False [nao verificado] | O OR é falso; não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:98`) |
| P2 — `batida_fora_de_turno` | `turno_nao_aberto` [nao verificado] | `.aberto=False` faz a função retornar False imediatamente; a consulta das entradas soltas nas batidas apuráveis não é alcançada. (`app/ponto/management/commands/e6_oraculo.py:196`, `:198`) |
| P3 — `len(horas_do_dia or ()) % 2` | G1: 0; G2: 4+ par; G3: 2 [nao verificado] | O resto é zero nos três grupos; não retorna BATIDA. (`app/ponto/management/commands/e6_oraculo.py:106`) |
| P4 — `fase_deslocada` | False [nao verificado] | Não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:108`) |
| P5 — `em_cadastro_x_realidade` | False [nao verificado] | O ternário final escolhe **ESTRUTURA**. ([e6_oraculo.py:110](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/ponto/management/commands/e6_oraculo.py:110)) |

Há quatro limites para interpretar esses negativos:

- **Cadastro False não certifica todo o cadastro.** Os flags vêm do vínculo do dia e de `defeito_folga_nao_definida`; este último acusa especificamente o 6x1 sem folga semanal no template e sem `FolgaDia` no mês. (`app/ponto/supra_juiz.py:193`, `:200`; `app/escala/models.py:1241`, `:1247`)
- **“Turno não aberto” não prova turno fechado.** Sem qualquer turno do dia, a autoridade também retorna `aberto=False`, junto de `sem_turno=True`. ([turnos.py:717](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/ponto/turnos.py:717), `app/ponto/turnos.py:596`)
- **Fase False não prova fase correta.** Dia sem nenhuma lavra cobrindo também fica fora do conjunto de fase deslocada. (`app/escala/services/fase_12x36.py:123`)
- **CXR False significa ausência na lista lida.** A leitura admite resultado/lista ausente como vazio e descarta itens sem `dia`; o raspado não distingue essas situações. (`app/ponto/management/commands/e6_oraculo.py:160`; situação dos casos: [nao verificado])

**Lei do desenho e limite por grupo**

Existe lei escrita comum ao beco: **“Emissao, pergunta e chamado nascem do sinal da celula”**, e o corte explicita **“lampada acesa no marco = fio vivo obrigatorio”**. ([CLAUDE.md:168](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/CLAUDE.md:168), [CORTES.md:263](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/docs/CORTES.md:263))

Contudo, a classe **d atual** é definida por ausência de pergunta viva, dia fora da lista e colaborador presente na lista, depois de recusadas as rotas anteriores. Ela não identifica qual emissor falhou nem equivale automaticamente ao GRUPO-D histórico. (`app/chamados/services/vigia_fio.py:635`, `:654`, [vigia_fio.py:677](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/chamados/services/vigia_fio.py:677), `app/docs/CORTES.md:263`)

| Grupo | O que fica explicado | Lei escrita e o que não fica demonstrado |
|---|---|---|
| **1 — 182, zero par** | P1/P2/P4/P5 negativos; P3 é `0 % 2 = 0`. O código declara expressamente zero como par e mantém o dia no resíduo ESTRUTURA. (`app/ponto/management/commands/e6_oraculo.py:72`, `:98`, `:106`, `:110`; valores: [nao verificado]) | Lei comum do fio: `CLAUDE.md:168` / `CORTES.md:263`. Atribuição errada à meia-noite aparece como explicação no comentário, mas sua ocorrência nesses 182 dias não foi mostrada. (`app/ponto/management/commands/e6_oraculo.py:73`; ocorrência: [nao verificado]) |
| **2 — 13, quatro ou mais pares** | P1/P2/P4/P5 negativos; P3 tem resto zero. Nenhum desses testes retira o dia do resíduo. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`; valores: [nao verificado]) | Mesma lei do fio. Quantidade par não demonstra casamento correto com marcos, intervalo ou valor; horários, ata e valores desses casos estão [nao verificado]. |
| **3 — 5, duas pares** | P1/P2/P4/P5 negativos; P3 é `2 % 2 = 0`. O retorno final continua ESTRUTURA. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`; valores: [nao verificado]) | Mesma lei do fio. Duas batidas do oráculo e `aberto=False` não demonstram um turno real fechado; o realizado também admite ausência de turno com esse flag. (`app/ponto/turnos.py:717`; situação dos cinco casos: [nao verificado]) |

A L-099 exige coerência com o cadastro existente e nomeia esse juiz; o contrato estrutural exige **“1 JUIZ por pergunta”**. O raspado não mostra dois juízes discordando, nem valores divergentes entre leitores, portanto não prova uma quebra específica desses contratos em cada grupo. (`app/docs/LEIS.md:122`, [ESTRUTURAL.md:4](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/docs/ESTRUTURAL.md:4), `app/docs/ESTRUTURAL.md:29`; divergências específicas: [nao verificado])

**Não há lacuna de lei demonstrada para o retorno do dono: o resíduo já está escrito.** O que falta é evidência da causa material. Chamar essa falta de evidência de “sem lei” inventaria uma necessidade de critério novo. (`app/ponto/management/commands/e6_oraculo.py:69`, `:110`; causa material por grupo: [nao verificado])

**3. Dois exemplos por grupo**

| Grupo | Exemplo 1 | Exemplo 2 | Evidência |
|---|---|---|---|
| 1 | col144 — 2026-09-21 | col144 — 2026-09-23 | Anexo [nao verificado] |
| 2 | col137 — 2026-11-03 | col137 — 2026-11-05 | Anexo [nao verificado] |
| 3 | col222 — 2026-10-28 | col292 — 2026-10-22 | Anexo [nao verificado] |

“Zero batidas” foi preservado como rótulo recebido: na montagem do dono, a contagem vem de `oraculo.horas`, agrupada pela data inicial do turno do oráculo, enquanto `brutas` conta por data civil. Zero nessa chave não prova ausência de marcações civis naquela data. (`app/ponto/management/commands/e6_oraculo.py:144`, `:183`; `app/ponto/calculador/nucleo.py:235`, `:237`)

## 06/11 10:24 · sessao 5064473cb077 · - · - · origem=Ronald (tmux)

Os três grupos percorrem os mesmos cinco testes; só muda a cardinalidade usada na paridade. A função não tem outro juiz depois deles: todos negativos chegam ao retorno residual `ESTRUTURA`. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`)

| Predicado | Valor raspado comum aos grupos | Por que não captura |
|---|---|---|
| `folga_nao_definida or sem_vinculo_no_dia or sem_vinculo_em_dia_de_trabalho` | `False, False, False` [nao verificado] | OR falso; não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:98`) |
| `batida_fora_de_turno` | `turno_nao_aberto` [nao verificado] | `.aberto=False` faz o carregador retornar `False` imediatamente, antes de consultar entradas soltas e sua presença em `batidas_apuraveis`. (`app/ponto/management/commands/e6_oraculo.py:195`) |
| `len(horas_do_dia or ()) % 2` | Grupo 1: `0`; grupo 2: `4+`, par; grupo 3: `2` [nao verificado] | Resto zero nos três grupos; não retorna BATIDA. (`app/ponto/management/commands/e6_oraculo.py:106`) |
| `fase_deslocada` | `False` [nao verificado] | Não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:108`) |
| `em_cadastro_x_realidade` | `False` [nao verificado] | A expressão final escolhe ESTRUTURA. (`app/ponto/management/commands/e6_oraculo.py:110`) |

A montagem real carrega `oraculo.horas`, os turnos da janela ampliada, o cadastro do dia, o conjunto de fase e a lista CXR; chama `dono_da_divergencia` com esses insumos. (`app/ponto/management/commands/e6_oraculo.py:142`, `:148`, `:154`, `:164`, `:172`)

Limites essenciais:

- **“Turno não aberto” não prova turno fechado.** `RealizadoDoDia` tem os campos `minutos, sem_turno, aberto...`; quando não há turno do dia, retorna `None, True, False...`. Portanto, ausência de turno também produz `.aberto=False`. (`app/ponto/turnos.py:596`, `:717`)
- **“0 batidas” é zero na lista `horas` daquela chave do oráculo, não necessariamente zero marcações civis.** `InsumosDoDono` usa `oraculo.horas`; o núcleo agrega essa lista pela data da primeira batida de cada turno, enquanto `brutas` conta por data civil. (`app/ponto/management/commands/e6_oraculo.py:144`; `app/ponto/calculador/nucleo.py:235`, `:237`)
- **`fase_lavrada=False` não prova fase correta.** Um dia sem lavra cobrindo também fica fora do conjunto retornado; o código documenta “não tem medida e não é fase”. (`app/escala/services/fase_12x36.py:117`, `:123`, `:132`)
- **`em_cxr=False` significa ausência na lista lida, não demonstra distâncias pequenas.** A leitura tolera resultado/lista ausente como vazia e descarta itens sem `dia`. (`app/ponto/management/commands/e6_oraculo.py:160`)
- O juiz L-084 exige ambas as distâncias presentes e com módulo maior que 180 minutos; a acusação registra o dia pelo mapa da entrada, podendo obter `None`. Esses mecanismos existem, mas sua ocorrência nos 200 dias não foi demonstrada pelo raspado. (`app/ponto/motor_calculo_v2.py:1946`, `:1988`, `:1997`; ocorrência: [nao verificado])
- Os três flags de cadastro vêm de `_insumos_do_vinculo`: `ec=None` produz `sem_vinculo_no_dia=True`; com vínculo, os dois flags de vínculo são falsos e `folga_nao_definida` lê `ec.defeito_folga_nao_definida(data)`. (`app/ponto/supra_juiz.py:193`, `:198`)
- Esse último defeito é específico: ciclo 6x1 sem `folga_dia_semana` e sem `FolgaDia` no mês; `False` não certifica todo o cadastro. (`app/escala/models.py:1241`, `:1247`)

## 06/11 10:24 · sessao 5b6247571eb3 · - · - · origem=Ronald (tmux)

Lei comum aos três grupos: a emissão deriva da célula — “Emissao, pergunta e chamado nascem do sinal da celula” — e o consumidor lê a lâmpada; o corte do GRUPO-D explicita “lampada acesa no marco = fio vivo obrigatorio”. Esse é o lastro escrito para o beco sem porta, condicionado ao universo fornecido. (`CLAUDE.md:168`, `CLAUDE.md:170`, `app/docs/CORTES.md:263`; medição fornecida [nao verificado])

Não há prova de três mecanismos diferentes de quebra: os grupos variam somente entre 0, 4+ e 2 batidas pares; todos os demais predicados fornecidos são iguais. Os horários, marcos, ata, valores de realizado e atribuição entre dias não aparecem no dado. [nao verificado]

- **182 / zero:** o código declara zero como par e o deixa em ESTRUTURA; sua explicação sobre atribuição a outro dia não demonstra que ocorreu nos 182 casos. (`app/ponto/management/commands/e6_oraculo.py:72`; ocorrência [nao verificado]) Pergunta específica: **“sem lei: qual fato da célula, com zero batidas atribuídas pelo oráculo, deve decidir o dono além dos predicados negativos medidos?”** [nao verificado]
- **13 / 4+ pares:** a cardinalidade par evita BATIDA pela paridade, mas não prova cumprimento nem quebra da geometria dos marcos. (`app/ponto/management/commands/e6_oraculo.py:106`; geometria dos casos [nao verificado]) Pergunta específica: **“sem lei: qual propriedade observada desses dias demonstra erro estrutural de valor, além do resíduo do dono?”** [nao verificado]
- **5 / duas pares:** mesma limitação; duas batidas do oráculo não demonstram um turno real fechado ou uma ata correta. (`app/ponto/management/commands/e6_oraculo.py:106`; turnos/atas dos casos [nao verificado]) Pergunta específica: **“sem lei: qual propriedade observada desses pares decide a causa estrutural de valor?”** [nao verificado]

Não atribuir L-084: ela requer as duas pontas a mais de três horas dos marcos, informação ausente. (`app/docs/LEIS.md:116`; horários [nao verificado]) Não atribuir L-102/L-103: elas tratam de ausência de turno pareado; `turno_nao_aberto` não demonstra ausência de turno. (`app/docs/LEIS.md:128`, `app/docs/LEIS.md:129`, `app/ponto/management/commands/e6_oraculo.py:196`)

A classe `d` atual significa: sem pergunta viva, dia fora da lista e colaborador presente na lista; não equivale automaticamente ao GRUPO-D histórico de zero chamado e zero pergunta. (`app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:230`, `app/chamados/services/vigia_fio.py:248`, `app/docs/CORTES.md:263`)

`ESTRUTURAL.md` contém os contratos de juiz único/escritor único e L-099, mas nenhuma etiqueta literal `C11`; identidade dessa referência: [nao verificado]. (`app/docs/ESTRUTURAL.md:4`, `app/docs/ESTRUTURAL.md:29`)

## 06/11 10:24 · sessao accbea19d370 · CODEX-BANCO-DIRETO · leitura · origem=Code

1. Comando: `id; uname -a; pwd`  
rc: `0`. Saída literal completa: [nao verificado]
```text
uid=1001(ronald) gid=1001(ronald) groups=1001(ronald),65534(nogroup)
Linux saas-hasner-sp 6.8.0-139-generic #139-Ubuntu SMP PREEMPT_DYNAMIC Sat Aug  1 03:52:05 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux
/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d
```

2. Comando: `python3 -c "import socket; s=socket.create_connection(('127.0.0.1',5433),3); print('tcp ok'); s.close()"`  
rc: `1`. Saída literal, últimas 5 linhas: [nao verificado]
```text
    sock = socket(af, socktype, proto)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/socket.py", line 233, in __init__
    _socket.socket.__init__(self, family, type, proto, fileno)
PermissionError: [Errno 1] Operation not permitted
```

3. Comando: `python3 -c "import socket; s=socket.create_connection(('172.18.0.9',5432),3); print('tcp ok'); s.close()"`  
rc: `1`. Saída literal, últimas 5 linhas: [nao verificado]
```text
    sock = socket(af, socktype, proto)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/socket.py", line 233, in __init__
    _socket.socket.__init__(self, family, type, proto, fileno)
PermissionError: [Errno 1] Operation not permitted
```

4. Comando: `python3 -c "import django; print(django.__version__)"`  
rc: `1`. Saída literal completa: [nao verificado]
```text
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'django'
```

5. Comando: `python3 -c "import psycopg2; print(psycopg2.__version__)" ; python3 -c "import psycopg; print(psycopg.__version__)"`  
rc: `1`. Saída literal, últimas 5 linhas: [nao verificado]
```text
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'psycopg2'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'psycopg'
```

6. Comando: `which psql pg_isready docker; ls -la /var/run/docker.sock`  
rc: `0`. Saída literal completa: [nao verificado]
```text
/usr/bin/docker
srw-rw---- 1 nobody nogroup 0 Sep 22 10:49 /var/run/docker.sock
```

7. Comando: `env | grep -i -E 'sandbox|codex' | sed 's/=.*/=<omitido>/'`  
rc: `0`. Saída literal, primeiras 5 linhas: [nao verificado]
```text
CODEX_VERSION=<omitido>
PWD=<omitido>
CODEX_SESSION_ID=<omitido>
CODEX_OBRA=<omitido>
CODEX_THREAD_ID=<omitido>
```

TCP local: não nos dois destinos testados; criação do socket bloqueada com `Operation not permitted`. [nao verificado]  
Django: não neste `python3`; módulo ausente. [nao verificado]  
Docker: cliente e socket presentes; acesso ao daemon não foi testado. [nao verificado]

## 06/11 10:23 · sessao 1b70f051c81b · CXR-TELA-LE-O-MOTOR · leitura · origem=Code

HASH LIDO = 0fca15818137d882b634bdf8261780370e63582d

| id | regra de origem (L-NNN, corte literal do aval ou arquivo:linha da regra) | entrada (model e campos, cada campo com arquivo:linha da definicao) | esperado OBSERVAVEL (o que um teste ve: retorno, linha gravada, status, texto) | mutante que tem de morder (a alteracao de codigo que este caso pega) | tipo golden/borda/adversarial | [nao verificado]
|---|---|---|---|---|---|
| CXR-01 | Aval fornecido: “os 7 dias de col841 aparecem na tela”; app/docs/BACKLOG.md:775 | `ResultadoMes.colaborador_id=841` (app/ponto/motor_calculo_v2.py:271), `mes=11` (:272), `ano=2026` (:273), `dias_cadastro_x_realidade` com 7 entradas (:289); `dia=20/10/2026`, `causa=l084_pausa` (:1671). Datas das outras seis entradas: [nao verificado]. | Os sete dias publicados aparecem na tela. A associação fornecida de 20/10 à competência 11 conflita com L-090; qual referência corrigir: regra nao decide. | Filtrar `l084_pausa`; mostrar somente dias reconhecidos pela lista antiga. | golden |
| CXR-02 | Aval fornecido: “tela x dias_cadastro_x_realidade = 0 na frota, comp 10 e 11” | Universo de ensaio não vazio, mesmo escopo; `ResultadoMes.mes=10` (app/ponto/motor_calculo_v2.py:272), `ano=2026` (:273), `colaborador_id` (:271), lista publicada (:289). Frota real dessa competência: [nao verificado]. | Na competência 10, zero dias publicados ausentes e zero dias extras na assinatura coberta pela obra. | Omitir um colaborador ou acrescentar um dia derivado pela tela. | golden |
| CXR-03 | Mesmo corte literal do aval: “comp 10 e 11” | Universo de ensaio não vazio; `ResultadoMes.mes=11` (app/ponto/motor_calculo_v2.py:272), `ano=2026` (:273), `colaborador_id` (:271), lista publicada (:289). Frota real: [nao verificado]. | Na competência 11, zero dias publicados ausentes e zero dias extras na assinatura coberta pela obra. | Implementar a leitura publicada somente para a competência 10. | golden |
| CXR-04 | Aval fornecido: “0 chamada ao motor por request”; app/docs/BACKLOG.md:775, C3 | Request da tela e request PDF; entrada do leitor `lista(empresas=...)` (app/escala/services/cadastro_realidade.py:710); publicação disponível: entidade/campos [nao verificado]. | Cada request faz zero chamadas ao motor, inclusive chamadas indiretas. | Chamar o motor dentro de `lista`, da view ou da geração do PDF. | adversarial |
| CXR-05 | L-090, app/docs/LEIS.md:123; app/ponto/janelas.py:11 | `Empresa.dia_inicio_competencia=21` (app/colaboradores/models.py:46); item publicado com `dia=20/10/2026` (app/ponto/motor_calculo_v2.py:1671). | O dia 20/10 pertence à competência 10/2026, encerrada em 20/10; não à 11. | Usar `dia >= 20` para avançar a competência. | borda |
| CXR-06 | L-090, app/docs/LEIS.md:123; app/ponto/janelas.py:11 | `Empresa.dia_inicio_competencia=21` (app/colaboradores/models.py:46); item publicado com `dia=21/10/2026` (app/ponto/motor_calculo_v2.py:1671). | O dia 21/10 pertence à competência 11/2026, iniciada em 21/10. | Recortar por mês civil ou manter o dia 21 na competência anterior. | borda |
| CXR-07 | L-085, app/docs/LEIS.md:117; L-090, app/docs/LEIS.md:123 | `TipoEscala.hora_inicio=19:00`, `hora_fim=07:00` (app/escala/models.py:50, :51); `Batida.timestamp` em 20/10 e 21/10 (app/ponto/models.py:23); item publicado com `dia=20/10` (app/ponto/motor_calculo_v2.py:1671). | A jornada publicada pertence ao dia 20. A cauda depois da meia-noite não cria outro dia de trabalho nem outra entrada por esse motivo. | Agrupar pela data civil da saída e criar também o dia 21. | borda |
| CXR-08 | Aval fornecido: “tela x dias_cadastro_x_realidade = 0”; L-040, app/docs/LEIS.md:88 | `ResultadoMes.dias_cadastro_x_realidade=[]` (app/ponto/motor_calculo_v2.py:289), publicação concluída; armazenamento da publicação: [nao verificado]. | Zero dias dessa publicação na tela. Texto de vazio, estrutura do retorno e status HTTP: regra nao decide. | Interpretar lista vazia como autorização para carregar a lista antiga. | borda |
| CXR-09 | Aval fornecido: “ler o que o motor publicou”; “PROIBIDO fallback”; L-040, app/docs/LEIS.md:88 | Duas publicações sucessivas: primeira contém um `dia`, segunda não o contém; `ResultadoMes.dias_cadastro_x_realidade` (app/ponto/motor_calculo_v2.py:289), chave `dia` (:1671). | Depois da segunda publicação concluída, o dia removido deixa de aparecer nessa assinatura. | Acrescentar publicações sem retirar os dias que desapareceram da fonte. | adversarial |
| CXR-10 | Aval fornecido: “ler o que o motor publicou”; “PROIBIDO fallback”; app/docs/BACKLOG.md:775 | Dia presente somente na lista antiga; `ResultadoMes.dias_cadastro_x_realidade=[]` (app/ponto/motor_calculo_v2.py:289). Definição do registro antigo específico: [nao verificado]. | Para a mesma acusação coberta pela obra, vence a publicação do motor: o dia não aparece por reaproveitamento da lista antiga. | Fazer união da publicação com o resultado antigo como fallback. | adversarial |
| CXR-11 | Aval fornecido: “lavra 2x = mesmo estado”; requisito fornecido: “1 linha, 0 duplicata” | Mesmos `ResultadoMes.colaborador_id`, `mes`, `ano`, lista (app/ponto/motor_calculo_v2.py:271, :272, :273, :289); duas lavras no mesmo dia. Entidade/chave da publicação: [nao verificado]. | Estado final igual ao de uma lavra; uma linha da publicação diária, zero duplicatas. Retorno e carimbo de atualização: regra nao decide. | Inserir nova linha incondicionalmente em cada lavra. | adversarial |
| CXR-12 | L-099, app/docs/LEIS.md:122; aval fornecido: “lavra 2x = mesmo estado” | Duas lavras concorrentes com os mesmos insumos; lista definida em app/ponto/motor_calculo_v2.py:289. Entidade/chave da publicação: [nao verificado]. | Ao terminarem, mesmo estado de uma lavra e nenhuma duplicata. Qual chamada espera, falha ou retorna primeiro: regra nao decide. | Separar consulta e inserção de modo que ambas gravem uma publicação equivalente. | adversarial |
| CXR-13 | Aval fornecido: “nunca motor por request”; “PROIBIDO fallback”; L-099, app/docs/LEIS.md:122 | Request durante uma lavra ainda em andamento; lista-fonte (app/ponto/motor_calculo_v2.py:289). Campos de conclusão/versão da publicação: [nao verificado]. | Regra nao decide qual versão a tela serve durante a escrita. Permanecem proibidos motor no request, fallback e decisão própria da tela. | Diante da publicação incompleta, recalcular no request. Escolha entre versões intermediárias: sem gabarito escrito. | adversarial |
| CXR-14 | Aval fornecido: “nunca motor por request”; “PROIBIDO fallback” | Lavra de hoje ainda não executada; publicação anterior ou ausência dela: entidade/campos [nao verificado]; leitor `lista(hoje=...)` (app/escala/services/cadastro_realidade.py:710). | Regra nao decide entre última publicação, vazio, aviso ou indisponibilidade. Zero chamadas ao motor e nenhum fallback. | Usar ausência da lavra diária para disparar motor ou lista antiga. | borda |
| CXR-15 | Aval fornecido: “tela x dias_cadastro_x_realidade = 0”; L-099, app/docs/LEIS.md:122 | `Colaborador.situacao=desligado`, `data_demissao` preenchida (app/colaboradores/models.py:323, :325); seu dia permanece publicado no mesmo escopo, lista (app/ponto/motor_calculo_v2.py:289). | Se o dia integra a publicação comparada, aparece na tela. Uma exclusão adicional de desligados no universo da publicação: regra nao decide. | Excluir apenas na tela todos os colaboradores atualmente desligados. | borda |
| CXR-16 | L-020, app/docs/LEIS.md:71; L-040, app/docs/LEIS.md:88 | Dois `EscalaColaborador`, com `data_inicio`, `data_fim` e `tipo_escala` diferentes (app/escala/models.py:875, :876, :874); `CelulaDia.escala_geradora` antigo e `dna` congelado (:1692, :1697); dias publicados antes/depois da troca (:1685). | Os dias publicados permanecem visíveis e o DNA congelado não muda pela publicação. Qual vínculo a nova assinatura apresenta quando a fonte não o identifica: regra nao decide. | Resolver todo o passado pelo vínculo atual e reescrever a célula durante a lavra da lista. | borda |
| CXR-17 | L-020, app/docs/LEIS.md:71 | `CelulaDia.origem=editada`, `dna`, `trabalha`, `escala_geradora` (app/escala/models.py:1691, :1697, :1686, :1692); publicar acusação desse dia. | A publicação não altera esses campos da célula. | Regenerar célula editada como efeito colateral de publicar CXR. | adversarial |
| CXR-18 | Aval fornecido: “ler o que o motor publicou”; L-040, app/docs/LEIS.md:88 | `FechamentoMensal.mes`, `ano`, `status=exportado` (app/ponto/models.py:408, :409, :410); dia presente na lista publicada (app/ponto/motor_calculo_v2.py:289). | Havendo publicação no escopo comparado, a tela a espelha. Regra nao decide uma política específica de lavra CXR em exportada. | Acrescentar somente no leitor um filtro que esconde competência exportada. | borda |
| CXR-19 | Aval fornecido, PRONTO (5); regras de causas em app/chamados/services/vigia_fio.py:523; conservação em :531 | Dia passado retido, sem pergunta/chamado vivo; `CelulaDia.colaborador`, `data` (app/escala/models.py:1681, :1685). Antes: colaborador na lista, dia fora. Depois: dia na lista, dono ESTRUTURA; formato de presença/dono (app/escala/services/cadastro_realidade.py:951). | `d` diminui 1; `e/dia_na_lista_dono_estrutura` aumenta 1; `fio_morto_alcance` não diminui por essa transição. `a+b+c+d+e` continua fechando. | Retirar o dia de `d` sem colocá-lo em `e`; considerar somente presença do colaborador. | golden |
| CXR-20 | Corte literal “Dono ESTRUTURA nunca sai”, app/chamados/services/vigia_fio.py:292; causas em :525 | Mesmo cenário de entrada em lista, mas dono lavrado CADASTRO; presença/dono por dia definidos em app/escala/services/cadastro_realidade.py:951. | O dia vai para `fio_do_admin`, conforme esse corte; não se exige `e/dia_na_lista_dono_cadastro`. As partes do universo continuam fechando (:531). | Enviar todos os donos para `e` ou retirar todos do alarme sem distinguir o dono lavrado. | adversarial |
| CXR-21 | L-040, app/docs/LEIS.md:88; app/escala/services/cadastro_realidade.py:974 | Mesma lista publicada (app/ponto/motor_calculo_v2.py:289), entregue à tela e ao PDF; parâmetro `dados` do PDF (app/escala/services/cadastro_realidade.py:973). | Tela e PDF contêm os mesmos dias e agrupamentos da lista. | Fazer o PDF consultar a lista antiga ou derivar seus próprios dias. | golden |
| O10-01 | Contrato TROCA-DE-PLANTAO, app/docs/CORTES.md:187 | Cedente com `TipoEscala.tipo_ciclo=12x36` (app/escala/models.py:117); `CelulaDia.data`, `trabalha=True` (:1685, :1686). Par completo, duas cartas e confirmação administrativa: model/campos [nao verificado]. | Dia cedido passa a folga trocada, sem desconto decorrente da cessão. | Aplicar o efeito apenas ao substituto e deixar falta/desconto no cedente. | golden |
| O10-02 | Contrato TROCA-DE-PLANTAO, app/docs/CORTES.md:187 | Cedente com `TipoEscala.tipo_ciclo=6x1` (app/escala/models.py:117); `CelulaDia.data`, `trabalha=True` (:1685, :1686). Par, cartas e confirmação: [nao verificado]. | Dia cedido é folga trocada, sem desconto decorrente da cessão. | Restringir o efeito da troca ao ciclo 12x36. | golden |
| O10-03 | Contrato TROCA-DE-PLANTAO, app/docs/CORTES.md:187 | Substituto com `CelulaDia.trabalha=False`, `data` (app/escala/models.py:1686, :1685); jornada noturna com `TipoEscala.hora_inicio`, `hora_fim` (:50, :51). Par confirmado e cartas: [nao verificado]. | Dia assumido é normal, com AN/intra conforme a regra aplicável, sem HE100 decorrente de ser originalmente folga. Quantidades de AN/intra sem cadastro completo: [nao verificado]. | Manter o dia como folga trabalhada com HE100 ou suprimir AN/intra por ser troca. | golden |
| O10-04 | “troca confirmada sem 2 anexos = vermelho”, app/docs/CORTES.md:187 | Tentativa de confirmação com zero cartas. Model/campos do par e dos anexos: [nao verificado]. | Recusa da confirmação; nenhum par válido produzido. Código, status e texto exatos: regra nao decide. | Remover a exigência dos documentos. | borda |
| O10-05 | Duas cartas, uma de cada lado, app/docs/CORTES.md:187 | Tentativa com somente carta do cedente; campos dos anexos: [nao verificado]. | Não confirma a troca. Código/status/texto: regra nao decide. | Aceitar qualquer documento como suficiente. | borda |
| O10-06 | Duas cartas, uma de cada lado, app/docs/CORTES.md:187 | Tentativa com somente carta do substituto; campos dos anexos: [nao verificado]. | Não confirma a troca. Código/status/texto: regra nao decide. | Validar apenas a carta do substituto. | borda |
| O10-07 | “troca e ATO com par”; ausência vira pedido, app/docs/CORTES.md:187 | `Ausencia.tipo=troca_de_plantao`, `data` (app/ponto/models.py:749, :750); somente um lado. Referência à contraparte: [nao verificado]. | Continua pedido; não produz o efeito de troca confirmada. | Tratar a ausência isolada como confirmação do par. | borda |
| O10-08 | L-099, app/docs/LEIS.md:122; contrato em app/docs/CORTES.md:187 | Reconfirmar o mesmo par, sem mudança de insumos; model/chave do par: [nao verificado]. | Mesmo estado e mesmos efeitos de uma confirmação; nenhum efeito duplicado. Retorno e repetição de registros de tentativa: regra nao decide. | Inserir novamente os efeitos em toda confirmação. | adversarial |
| O10-09 | L-085, app/docs/LEIS.md:117; contrato em app/docs/CORTES.md:187 | Jornada assumida em 19/10, 19:00–20/10, 07:00; `TipoEscala.hora_inicio`, `hora_fim` (app/escala/models.py:50, :51); `CelulaDia.data=19/10` (:1685). Par confirmado: [nao verificado]. | Jornada pertence ao dia 19, normal por troca, sem HE100 decorrente da folga original; a cauda não cria trabalho independente no dia 20. | Dividir a troca em dois dias pela meia-noite. | borda |
| O10-10 | L-090, app/docs/LEIS.md:123; contrato em app/docs/CORTES.md:187 | `Empresa.dia_inicio_competencia=21` (app/colaboradores/models.py:46); `CelulaDia.data` em 20/10 e 21/10 (app/escala/models.py:1685). Datas/ligação do par: [nao verificado]. | Dia 20 pertence à comp 10; dia 21 à comp 11. O contrato não restringe o par a uma única competência. Sem outra pendência, os efeitos de cada lado respeitam seu dia. | Exigir mesmo mês/competência para o par ou perder um dos lados no recorte. | borda |
| O10-11 | S5b-TROCA-NAO-CRIA-DESCONTO, app/docs/BACKLOG.md:1248 | Cenário de troca motor→calculador, classe `Motor12x36` (app/ponto/motor_calculo_v2.py:2730); `pontualidade_fora_por_lei` (app/ponto/calculador/regras.py:583); marcos e pares de pontualidade (:590). | A classe que não julgava pontualidade permanece `NAO_DECIDE`; a substituição não começa a cobrar atraso/antecipada. Esse corte trata da troca do calculador. | Habilitar pontualidade da classe durante a substituição e criar desconto novo. | adversarial |
| O10-12 | L-099, app/docs/LEIS.md:122; contrato em app/docs/CORTES.md:187 | Duas confirmações concorrentes do mesmo par e documentos; model/chave do par: [nao verificado]. | Estado final equivalente a uma confirmação, sem duplicar os efeitos. Resposta da operação perdedora: regra nao decide. | Ambas as confirmações gravarem efeitos independentes para o mesmo ato. | adversarial |
| O10-13 | “o par so vale quando o admin confirma”, app/docs/CORTES.md:187 | Pedido em andamento, um lado preparado e confirmação ainda não concluída; estado/model do par: [nao verificado]. | O pedido ainda não vale como troca confirmada. Isolamento de leitura durante a gravação: regra nao decide. | Tornar o dia cedido folga efetiva antes de existir par confirmado. | adversarial |
| O10-14 | Contrato em app/docs/CORTES.md:187; L-020, app/docs/LEIS.md:71 | Colega sem vínculo cobrindo o dia: `EscalaColaborador.data_inicio`, `data_fim`, `ativa` (app/escala/models.py:875, :876, :877); `CelulaDia.data` (:1685). | Regra nao decide se a troca é recusada, aceita ou exige regularização do vínculo. | Aceitar versus recusar não tem mutante discriminável sem regra adicional. | borda |
| O10-15 | Contrato em app/docs/CORTES.md:187 | Dia cedido com trabalho já registrado: `Batida.colaborador`, `timestamp` (app/ponto/models.py:22, :23); `CelulaDia.data` (app/escala/models.py:1685). | Regra nao decide o conflito entre trabalho já realizado e cessão do mesmo dia. | Aceitar, recusar ou reclassificar o trabalho não tem gabarito escrito para este conflito. | adversarial |
| O10-16 | L-020, app/docs/LEIS.md:71; contrato em app/docs/CORTES.md:187 | `CelulaDia.origem=editada`, `dna`, `editada_em` (app/escala/models.py:1691, :1697, :1732); nova troca solicitada para esse dia, campos do par [nao verificado]. | Regra nao decide a precedência entre edição humana anterior e novo override de troca. A soberania da célula não autoriza reescrita incidental pelo gerador. | Regerar silenciosamente a célula editada. Substituir versus preservar o override humano: sem gabarito específico. | adversarial |
| O10-17 | Contrato em app/docs/CORTES.md:187; restrição O10 em app/docs/BACKLOG.md:1781 | `FechamentoMensal.status=exportado`, `mes`, `ano` (app/ponto/models.py:410, :408, :409); tentativa de conversão em lote sem DRY/aval. Dados do par: [nao verificado]. | A conversão descrita exige DRY→aval; não efetiva esse lote sem cumprir a condição. Status/texto de recusa: regra nao decide. | Efetivar automaticamente o lote legado por a ausência já estar aprovada. | adversarial |
| O10-18 | Contrato em app/docs/CORTES.md:187; legado descrito em app/docs/BACKLOG.md:1224 | Fixture legada: `Ausencia.tipo=troca_de_plantao`, `status=aprovada`, `data_fim=NULL` (app/ponto/models.py:749, :799, :751), sem contraparte e sem duas cartas; respectivos campos [nao verificado]. | A aprovação isolada não constitui par confirmado segundo o contrato. Não produz automaticamente os efeitos da troca. Existência desse registro hoje em produção: [nao verificado]. | Promover qualquer ausência legada aprovada a troca válida, sem verificar par e documentos. | adversarial |

- **Regras que voce NAO encontrou escritas:** conciliação entre “col841 20/10, comp 11” fornecido e a janela 21→20 de L-090 (app/docs/LEIS.md:123); datas dos outros seis dias: [nao verificado].
- **Regras que voce NAO encontrou escritas:** conteúdo servido antes da primeira lavra diária, durante publicação incompleta ou quando lavras concorrentes publicam conteúdos diferentes; procurado contra o aval fornecido e o contexto C8 (app/ponto/services/vinculo_na_lista.py:111): [nao verificado].
- **Regras que voce NAO encontrou escritas:** exclusões específicas CXR por desligamento/exportação e escolha do vínculo exibido quando a publicação não o identifica; procurado em app/docs/BACKLOG.md:775, app/docs/LEIS.md:71 e :88: [nao verificado].
- **Regras que voce NAO encontrou escritas:** A2 literal de O10 no anexo raspado; o registro da obra em app/docs/BACKLOG.md:1781 não transcreve esse aval: [nao verificado].
- **Regras que voce NAO encontrou escritas:** validação O10 para colega sem vínculo, dia cedido já trabalhado, datas vazias e precedência entre troca e edição humana anterior; procurado no contrato de app/docs/CORTES.md:187: [nao verificado].
- **Regras que voce NAO encontrou escritas:** códigos HTTP, textos exatos de recusa, retorno da reconfirmação e política de isolamento/desempate das confirmações O10; procurado no contrato de app/docs/CORTES.md:187 e na idempotência de app/docs/LEIS.md:122: [nao verificado].

## 06/11 10:20 · sessao cffa6b326684 · - · - · origem=Ronald (tmux)

Regras e definições O10 lidas:

- Contrato: `app/docs/CORTES.md:187`; reprodução no backlog: `app/docs/BACKLOG.md:1224`.
- S5b: `app/docs/BACKLOG.md:1248`. A “troca” desse corte é a substituição do motor pelo calculador: classes que não julgavam pontualidade não passam a cobrar. Não atribuir essa origem exclusivamente à troca de plantão.
- Idempotência/coerência: L-099, `app/docs/LEIS.md:122`.
- Competência: L-026, `app/docs/LEIS.md:77`; L-090, `app/docs/LEIS.md:123`.
- Meia-noite: L-085, `app/docs/LEIS.md:117`.
- Célula soberana: L-020, `app/docs/LEIS.md:71`.
- Exportada: L-113, `app/docs/LEIS.md:150`; restrição específica O10 registrada em `app/docs/BACKLOG.md:1781` (“motor_v2/exportada sem !”).

Definições para entradas:

| Model | Campos com definição |
|---|---|
| TipoEscala | `tipo_ciclo` `app/escala/models.py:117`; valores 12x36 `:26`, 6x1 `:30`; `hora_inicio` `:50`; `hora_fim` `:51`; intervalo `:52` e `:53` |
| EscalaColaborador | `colaborador` `app/escala/models.py:872`; `tipo_escala` `:874`; `data_inicio` `:875`; `data_fim` `:876`; `ativa` `:877` |
| CelulaDia | `colaborador` `app/escala/models.py:1681`; `data` `:1685`; `trabalha` `:1686`; `origem` `:1691`; `escala_geradora` `:1692`; `dna` `:1697`; `editada_em` `:1732`; `editada_por` `:1733` |
| Ausencia | `colaborador` `app/ponto/models.py:743`; `tipo` `:749`; `data` `:750`; `data_fim` `:751`; `documento` `:777`; `status` `:799`; `aprovado_por` `:805`; `aprovada_em` `:810` |
| TipoAusencia | `codigo` `app/ponto/models.py:577`; `efeito` `:581` |
| Batida | `colaborador` `app/ponto/models.py:22`; `timestamp` `:23` |
| Empresa | `dia_inicio_competencia` `app/colaboradores/models.py:46` |
| DiaPago, observáveis | `horas_trabalhadas` `app/ponto/models.py:326`; `horas_noturnas` `:327`; HE100 `:330`, `:331`, `:332`; `horas_atraso` `:334`; `horas_saida_antecipada` `:335`; `horas_falta` `:336`; `horas_intra_indenizada` `:337` |

Não achei definição de model/campos próprios do par, do dia cedido e das duas cartas: substituir essas entradas por `[nao verificado]`. O registro do estado atual também diz não haver escritor humano de célula (`app/docs/RELATO.md:3`); isso não define o esperado.

Casos sustentados:

| Caso | Origem | Entrada e esperado pela regra | Mutante |
|---|---|---|---|
| Cedente 12x36 | `app/docs/CORTES.md:187` | Par completo, duas cartas, confirmação admin, cedente 12x36; dia cedido vira folga trocada, sem desconto. Valores concretos de AN/intra não definidos pelo contrato. | Override aplicado só ao substituto. |
| Cedente 6x1 | `app/docs/CORTES.md:187` | Mesmo cenário, cedente 6x1; mesmo efeito de folga sem desconto. | Restringir troca a 12x36. |
| Substituto em folga | `app/docs/CORTES.md:187` | Dia originalmente de folga assumido por par confirmado; dia normal com AN/intra, HE100=0. | Manter folga trabalhada/HE100 ou suprimir AN/intra. |
| Zero anexos | `app/docs/CORTES.md:187` | Pedido de confirmação sem cartas; não vale como troca confirmada. Código/status/texto exato: regra não decide. | Remover exigência de documentos. |
| Só carta do cedente | `app/docs/CORTES.md:187` | Um documento; não confirma. | Trocar exigência dos dois por qualquer um. |
| Só carta do substituto | `app/docs/CORTES.md:187` | Mesmo resultado, em sentido contrário. | Validar apenas documento do substituto. |
| Par incompleto | `app/docs/CORTES.md:187` | Ausencia `tipo='troca_de_plantao'`, só um lado; continua pedido, sem efeito de par. Mesmo status legado “aprovada” não fornece o segundo lado. | Aprovação da ausência isolada basta para efetivar. |
| Origem cedente versus substituto | `app/docs/CORTES.md:187` | Pedido pode nascer de qualquer lado; completado e confirmado, resultado equivalente. | Aceitar apenas pedidos do cedente. |
| Segunda confirmação | L-099, `app/docs/LEIS.md:122` | Repetir confirmação do mesmo par sem alterar entradas; mesmo estado e efeito, sem duplicar desconto, pagamento ou override. Retorno/trilha repetida: regra não decide. | Inserção incondicional na reconfirmação. |
| Duas confirmações concorrentes | L-099, `app/docs/LEIS.md:122` | Mesmo par e entradas em duas confirmações; estado final equivalente a uma confirmação. Status HTTP, perdedor e número de tentativas de trilha: regra não decide. | Duas operações gravam duas trocas/efeitos. |
| Estado intermediário | `app/docs/CORTES.md:187` | Pedido ainda sem par confirmado; nenhum lado deve ser tratado como troca válida. Atomicidade de armazenamento e isolamento de leitura: regra não decide. | Aplicar cedido antes de completar/confirmar o par. |
| Meia-noite | L-085 `app/docs/LEIS.md:117` + contrato | Jornada assumida 20/10 19h–21/10 07h; jornada pertence ao dia 20, sem criar segundo trabalho dia 21; dia assumido normal sem HE100. | Classificar a cauda como trabalho independente dia 21. |
| Competência 20/21 | L-026 `app/docs/LEIS.md:77` + contrato | Empresa com corte 21, dias cedido/assumido em 20/10 e 21/10; dia 20 pertence à comp 10, dia 21 à comp 11. Efeitos da troca não desaparecem na fronteira. | Recortar por mês civil ou exigir ambos na mesma competência. |
| Vínculo trocado | L-020 `app/docs/LEIS.md:71` | Célula com `escala_geradora` antigo e vínculo atual diferente; preservar DNA congelado fora do ato autorizado. Qual vínculo habilita troca: regra não decide. | Resolver passado pelo vínculo atual e reescrever DNA incidentalmente. |
| Colega sem vínculo no dia | Contrato + L-020 | Ausência de vínculo ativo no dia; regra não decide se recusa ou como aceita. | Não há mutante de aceitação/recusa justificável sem regra adicional. |
| Cedido já trabalhado | `app/docs/CORTES.md:187` | Batidas existentes no dia cedido; regra não decide conflito entre trabalho já realizado e folga trocada. | Não há gabarito para escolher aceitar/recusar. |
| Célula editada | L-020 `app/docs/LEIS.md:71` + contrato | `origem='editada'`, DNA já humano; regra não decide precedência entre edição anterior e troca confirmada. | Não há gabarito para substituir/preservar override. |
| Legado solo | `app/docs/CORTES.md:187` | Ausencia `troca_de_plantao`, aprovada, `data_fim=NULL`, sem contraparte; não constitui par válido. Conversão em lote depende de DRY→aval. | Migrar toda aprovação antiga como troca confirmada. |
| S5b contraexemplo | `app/docs/BACKLOG.md:1248` | Classe MotorComercial/Motor12x36/MotorIntermitente que não julgava pontualidade; troca do calculador não começa a cobrar atraso/antecipada. | Habilitar pontualidade dessas classes durante substituição. |

Lacunas procuradas:

- A2 literal O10: não consta no `aval.txt` raspado; `app/docs/BACKLOG.md:1781` só registra execução/prontidão.
- Validação do colega sem vínculo, dia cedido já trabalhado, conflito com célula editada e datas vazias: `[nao verificado]`.
- Status HTTP e texto de recusa, chave do par, armazenamento das duas cartas, retorno de reconfirmação e política de concorrência: `[nao verificado]`.
- O dado legado de produção O10 não foi fornecido no bloco raspado; números históricos do backlog são relato de código/documentação, não nova leitura de produção (`app/docs/BACKLOG.md:1224`).

## 06/11 10:20 · sessao ef523e07fb21 · - · - · origem=Ronald (tmux)

Evidências e casos sustentados:

| Caso | Regra escrita | Entrada com definições | Esperado pela regra | Mutante |
|---|---|---|---|---|
| CXR-vigia-d-e | Aval fornecido PRONTO (5); causas e soma em `app/chamados/services/vigia_fio.py:519`, `app/chamados/services/vigia_fio.py:531` | Dia retido passado; `CelulaDia.colaborador` `app/escala/models.py:1681`, `data` `app/escala/models.py:1685`; sem pergunta/chamado vivo; colaborador já na lista, dia antes ausente; após lavra dia presente com dono ESTRUTURA, definição do item/dono novo `[nao verificado]` | O dia sai de d e entra em e com detalhe `dia_na_lista_dono_estrutura`; d diminui 1, e aumenta 1, mesmo universo; a+b+c+d+e continua igual a `fio_morto_alcance`. ESTRUTURA continua no alarme pela regra literal `app/chamados/services/vigia_fio.py:292`. | Continuar classificando por presença apenas do colaborador; omitir o dia de ambas as classes; abater d sem acrescentar e. |
| CXR-vigia-admin | Corte literal registrado em `app/docs/PROMPTS.md:1670`; transcrição em `app/chamados/services/vigia_fio.py:292` | Mesmo cenário, dia publicado com dono BATIDA ou CADASTRO `[nao verificado]` | Vai para `fio_do_admin`, com dono lido da lista; não é correto exigir e para esses donos. Se sair da lista sem resolver, volta ao fio morto. | Encaminhar todo dono para e; recalcular o dono no vigia; manter fora do alarme depois de sair da lista. |
| CXR-dia-x-colab | Regra documentada em `app/escala/services/cadastro_realidade.py:953` | Lista contém assinatura de vínculo/colaborador, mas nenhuma assinatura com data do dia `[nao verificado]`; `CelulaDia.data` `app/escala/models.py:1685` | Presença do colaborador não equivale a presença do dia; o dia continua fora de `dias_na_lista`. | Marcar todos os dias de um colaborador como presentes porque existe alguma linha dele. |
| CXR-lista-falhou | Regra explícita em `app/chamados/services/vigia_fio.py:191` | Carregamento da lista falha; mesmo dia consultado nos dois universos; parâmetros `hoje`, `empresas`, `agora` definidos em `app/chamados/services/vigia_fio.py:199` | Falha não retira dia do contador; `fio_do_admin_sem_resposta` conta uma vez o par colaborador/data na rodada. | Tratar exceção como lista vazia válida/alarme resolvido; contar duas vezes o mesmo dia. |
| CXR-lavra-nao-reescreve-celula | L-020 `app/docs/LEIS.md:71` | `CelulaDia.origem='editada'` `app/escala/models.py:1691`; DNA congelado `app/escala/models.py:1697`; lavra/publicação de CXR | A publicação da lista não autoriza alterar dia ou marcos congelados; `origem`, `dna`, `trabalha` e proveniência permanecem. | Regerar célula como efeito colateral de publicar lista. |

Definições úteis adicionais:

- `EscalaColaborador`: colaborador `app/escala/models.py:872`; posto `:873`; tipo_escala `:874`; data_inicio `:875`; data_fim `:876`; ativa `:877`.
- `Colaborador.situacao`: `app/colaboradores/models.py:323`; data_demissao: `:325`.
- `CelulaDia.escala_geradora`: `app/escala/models.py:1692`; regenerada_em `:1718`; regeneracoes `:1723`; dna_anterior `:1726`.
- Formato observável do vigia: `[celula_id, colab_id, data, competencia, rota, causa, detalhe, papel]`, documentado em `app/chamados/services/vigia_fio.py:548`.
- Universo e partes: `porta_fila + porta_pergunta + de_hoje_pra_frente + quatro baldes + fio_morto_alcance = universo`; `a+b+c+d+e=fio_morto_alcance`, regra escrita em `app/chamados/services/vigia_fio.py:531`.
- `limite` nunca omite dia sem porta; contagem por rota/papel cobre universo inteiro, regra escrita em `app/chamados/services/vigia_fio.py:551`.

Lacunas encontradas:

- Regra específica de CXR para lavra ainda não rodada hoje: não encontrada. C8 define retorno sem medida em `app/ponto/services/vinculo_na_lista.py:125`, mas isso é contexto de outra assinatura, não gabarito CXR.
- Regra de CXR para exposição durante escrita intermediária e desempate entre publicações concorrentes de conteúdos diferentes: não encontrada; aval apenas exige mesmo estado ao lavrar duas vezes.
- Regra específica CXR para desligado ou competência exportada: não encontrada além da autoridade única do aval; não importar exclusões C8/C11. C8 exclui desligado em `app/ponto/services/vinculo_na_lista.py:151`; C11 tem escopo próprio de competência aberta em `app/escala/services/cadastro_realidade.py:119`.
- Esquema da publicação CXR: `[nao verificado]`. O modelo C8 não foi imposto pelo aval como modelo de dados; `MetricaSnapshot` está definido em `app/inteligencia/models.py:81`.
- A garantia declarada de uma linha no C8 está em `app/ponto/services/vinculo_na_lista.py:111`; não é evidência de comportamento concorrente CXR.

## 06/11 10:18 · sessao 279f5e84cfaa · O10-B · censo · origem=Code

**1. Sítios que escrevem em `CelulaDia`**

| Sítio físico | Escrita | Pela porta ou fora |
|---|---|---|
| `regenerar_celulas_vinculo` | Fora da vigência: `save` de `trabalha=False` e DNA, somente para origem `gerada`. `app/ponto/portas/celula.py:232`, `app/ponto/portas/celula.py:239` | Pela porta. |
| `regenerar_celulas_vinculo` | `create` da célula ausente, com colaborador, data, trabalha, origem `gerada`, geradora e DNA. `app/ponto/portas/celula.py:253` | Pela porta. |
| `regenerar_celulas_vinculo` | `save` de trabalha, geradora, DNA, DNA anterior, momento e contador de regeneração; exige origem `gerada` e diferença. `app/ponto/portas/celula.py:257`, `app/ponto/portas/celula.py:270` | Pela porta. |
| `reverter_regeneracao_dia` | `save` de DNA restaurado, DNA anterior limpo, trabalha, geradora e trilha de regeneração. `app/ponto/portas/celula.py:570`, `app/ponto/portas/celula.py:578` | Pela porta. |
| `gerar_celulas_periodo` | `create` da célula ausente. `app/ponto/portas/celula.py:633` | Pela porta. |
| `gerar_celulas_periodo` | `save` somente do DNA: enriquecimento de célula passada, `gerada`, com DNA nulo. `app/ponto/portas/celula.py:643`, `app/ponto/portas/celula.py:645` | Pela porta. |
| `gerar_celulas_periodo` | `save` de trabalha, geradora e DNA para hoje/futuro; a editada foi pulada antes. `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:650`, `app/ponto/portas/celula.py:655` | Pela porta. |
| `salvar_e_emitir` | `cel.save(update_fields=campos)`, compartilhado pela lavra e pelos carimbos. `app/ponto/portas/celula.py:678`, `app/ponto/portas/celula.py:684` | Pela porta. |
| `carimbar_insumos_em_lote` | `bulk_update` de ausência, entrada, saída e `insumos_em`. `app/ponto/portas/celula.py:887` | Pela porta. |
| Chamadas lógicas do `save` compartilhado | Lavra sobre decisão administrativa: ata/status; atualização somente de status; lavra completa; carimbo; precedência na ata. `app/ponto/portas/celula.py:787`, `app/ponto/portas/celula.py:804`, `app/ponto/portas/celula.py:819`, `app/ponto/portas/celula.py:851`, `app/ponto/portas/celula.py:866` | Todas pela porta. |
| Delegação do modelo e chamadores da lavra | O método do modelo delega; decisão humana e cartório chamam a lavra. Não são novos pontos físicos de `save`. `app/escala/models.py:1826`, `app/ponto/services/veredito_celula.py:330`, `app/ponto/services/cartorio.py:889`, `app/ponto/services/cartorio.py:1242`, `app/ponto/services/cartorio.py:1802` | Pela porta. |
| Outros métodos de DML | Nenhum sítio adicional de `update`, `update_or_create`, `bulk_create` ou SQL direto contra produção foi localizado na cópia; completude além dos sítios identificados: **[nao verificado]**. |
| SQL de cópia da sombra | Inclui `escala_celuladia` e executa `DELETE`/`COPY`, porém o destino declarado é `sombra_db`/`sombra`; produção fornece `SELECT`/`COPY TO`. `bin/sombra_incremental.sh:20`, `bin/sombra_incremental.sh:35`, `bin/sombra_incremental.sh:55`, `bin/sombra_incremental.sh:61`, `bin/sombra_incremental.sh:68` | Fora da porta, destinado à sombra. |
| Migration `0030` | `CreateModel`: cria o esquema da tabela. `app/escala/migrations/0030_celuladia.py:17` | Fora da porta; DDL. |
| Migration `0031` | Adiciona DNA. `app/escala/migrations/0031_celuladia_dna.py:13` | Fora da porta; DDL. |
| Migration `0032` | Adiciona impressão e momento do julgamento. `app/escala/migrations/0032_celuladia_impressao_celuladia_julgada_em.py:13`, `app/escala/migrations/0032_celuladia_impressao_celuladia_julgada_em.py:18` | Fora da porta; DDL. |
| Migration `0036` | Adiciona protestos e campos do veredito. `app/escala/migrations/0036_b5_celula_veredito.py:13`, `app/escala/migrations/0036_b5_celula_veredito.py:18`, `app/escala/migrations/0036_b5_celula_veredito.py:23`, `app/escala/migrations/0036_b5_celula_veredito.py:28` | Fora da porta; DDL. |
| Migration `0037` | Adiciona ata. `app/escala/migrations/0037_hx_ata_celula.py:13` | Fora da porta; DDL. |
| Migration `0038` | Adiciona DNA anterior, contador e momento de regeneração; contador tem default zero. `app/escala/migrations/0038_celuladia_dna_anterior_celuladia_regeneracoes_and_more.py:13`, `app/escala/migrations/0038_celuladia_dna_anterior_celuladia_regeneracoes_and_more.py:18`, `app/escala/migrations/0038_celuladia_dna_anterior_celuladia_regeneracoes_and_more.py:23` | Fora da porta; DDL. |
| Migration `0039` | Adiciona ausência, `insumos_em`, entrada e saída. `app/escala/migrations/0039_b5f2_celula_insumos.py:15`, `app/escala/migrations/0039_b5f2_celula_insumos.py:20`, `app/escala/migrations/0039_b5f2_celula_insumos.py:25`, `app/escala/migrations/0039_b5f2_celula_insumos.py:30` | Fora da porta; DDL. |
| Migration `0044` | Adiciona status e rótulo. `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13`, `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:18` | Fora da porta; DDL. |

**2. Leitores de origem editada, `editada_em`, `editada_por` ou motivo**

| Âmbito / leitor | O que decide |
|---|---|
| Produção — regeneração fora da vigência | Só desliga origem `gerada`; preserva trabalha/DNA da editada. `app/ponto/portas/celula.py:232` |
| Produção — regeneração normal | Só reescreve origem `gerada`. `app/ponto/portas/celula.py:257` |
| Produção — reversão | Retorna `False` se origem diferente de `gerada`. `app/ponto/portas/celula.py:558` |
| Produção — geração periódica | Origem `editada` incrementa `puladas`; enriquecimento passado exige `gerada`. `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:643` |
| Produção — `medir_stale` | Consulta somente `gerada`; editada fica fora dessa acusação de divergência com o cadastro. `app/ponto/management/commands/tripwire_celulas.py:21` |
| Produção — `contra_o_template` | Consulta somente `gerada`; editada não conta como erro do gerador. `app/escala/management/commands/celulas_contra_o_template.py:29` |
| Produção — `houve_fato_novo` | `editada_em` em `(desde, ate]` retorna `True`. `parcial_so_lista` usa esse resultado para decidir se o restante fica somente na lista. `app/ponto/services/cartorio.py:365`, `app/ponto/services/cartorio.py:400` |
| Produção — `marcos_do_lote` / `esta_stale` | Inclui `editada_em` no maior timestamp por colaborador; marco posterior ao julgamento torna a pergunta stale. `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:154`, `app/chamados/veredito_lavrado.py:187` |
| Produção — `janelas_reabertas` | Inclui `Max(editada_em)`; fato posterior à lavra reabre a janela passada para nova lavra do censo de fase. `app/escala/services/fase_12x36.py:188`, `app/escala/services/fase_12x36.py:192` |
| Produção — apresentação da origem | Expõe a origem no retrato do dia, representação textual e detalhe do contador; sem decisão especial por `editada` nesses trechos. `app/ponto/services/dia_do_colab.py:90`, `app/escala/models.py:1842`, `app/escala/management/commands/celulas_contra_o_template.py:74` |
| Produção — `editada_por` | Nenhum leitor de negócio localizado **[nao verificado]**; o campo é declarado como FK opcional. `app/escala/models.py:1733` |
| Produção/testes — motivo da edição | Não existe campo de motivo de edição no catálogo da célula; o catálogo é confrontado com os campos concretos do modelo. `app/escala/tests/test_d3_guarda_da_linha_celula.py:38`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:146`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:171` |
| Teste — preservação da origem | Lê origem após gerar novamente e exige `editada`, com trabalha preservado. `app/escala/tests/test_celula_dia.py:83` |
| Teste — contrato do DNA gerado | Filtra origem `gerada` para verificar vocabulário, versão e vínculo do DNA; editadas ficam fora desse universo. `app/escala/tests/test_celula_dia.py:116` |
| Teste — regeneração não é edição humana | Exige `editada_em is None` após regeneração. `app/escala/tests/test_cobranca_orfa_regeneracao.py:166` |
| Teste — isolamento do relógio de regeneração | Exige `editada_em is None` no caso que prova reabertura por `regenerada_em`. `app/escala/tests/test_vfase_dono_as_of.py:279` |
| Teste — guarda da linha | Classifica os dois campos, mantém a exceção `SEM_ESCRITOR` e procura escritas sintáticas deles. `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:72`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179` |
| Testes com fixture editada — autoridade | Escreve origem editada e verifica que `eh_dia_trabalho` acompanha a célula, divergindo da aritmética. `app/escala/tests/test_celula_dia.py:98`, `app/escala/tests/test_celula_dia.py:100` |
| Testes com fixture editada — tripwire | Verifica que editada não acusa stale. `app/escala/tests/test_celula_dia.py:358`, `app/escala/tests/test_celula_dia.py:361` |
| Testes com fixture editada — fim do vínculo | Verifica que célula humana permanece trabalhando fora da vigência. `app/escala/tests/test_selo_celula_alem_do_vinculo.py:103`, `app/escala/tests/test_selo_celula_alem_do_vinculo.py:106` |
| Testes com fixture editada — foto de folgas | Verifica que a foto não reescreve trabalha humano. `app/escala/tests/test_selo_foto_folgas_relavra.py:106`, `app/escala/tests/test_selo_foto_folgas_relavra.py:109` |
| Testes com fixture editada — contadores | Contra-template ignora editada; folga com marcos inclui editada, pois esse segundo contador vale para qualquer origem. `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:98`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:99`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:128`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:129` |
| Testes com fixture editada — universo de furos | Folga materializada prevalece sobre template sem fase e resulta `sem_dia`. `app/escala/tests/test_sem_dia_pelo_juiz.py:46`, `app/escala/tests/test_sem_dia_pelo_juiz.py:65` |
| Testes com fixture editada — troca de template | Verifica preservação da célula humana após executar mudança do vínculo. `app/colaboradores/tests/test_vinculo_regenera_celulas.py:39`, `app/colaboradores/tests/test_vinculo_regenera_celulas.py:45` |
| Testes com fixture editada — reversão | Exige recusa da reversão mesmo havendo DNA anterior. `app/ponto/tests/test_porta_celula_dia.py:247`, `app/ponto/tests/test_porta_celula_dia.py:250` |
| Testes com fixture editada — reconciliação | Usam folga editada para verificar reconciliação/deduplicação, sobrevivência do novo e absolvição da pergunta; não comparam diretamente origem nos asserts. `app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:157`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:180`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:203`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:225`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:249` |
| Teste com fixture `editada_em` — impressão | Atualiza ata e `editada_em`; exige hash inalterado. `app/ponto/tests/test_d3_impressao_cobre_insumos.py:140`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:141` |
| Teste com fixture de edição e DNA novo | Atualiza origem, `editada_em` e marcos; exige hash alterado. A entrada no hash é pelo DNA. `app/ponto/tests/test_d3_impressao_cobre_insumos.py:239`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:242` |
| Teste com fixture somente `editada_em` | Verifica reabertura da competência no censo de fase. `app/escala/tests/test_vfase_dono_as_of.py:291`, `app/escala/tests/test_vfase_dono_as_of.py:297` |

**3. Quem escreve hoje `editada_em` / `editada_por`**

| Campo / âmbito | Resposta |
|---|---|
| `editada_em`, edição em produção | **Nenhum escritor declarado**; catálogo marca `None`, e o selo exige ausência de escritor nos arquivos de produção que varre. `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179` |
| `editada_por`, edição em produção | **Nenhum escritor declarado**, sob a mesma guarda. `app/escala/tests/test_d3_guarda_da_linha_celula.py:51`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179` |
| `editada_em`, testes | Há escritas explícitas nas fixtures de impressão e reabertura de fase. `app/ponto/tests/test_d3_impressao_cobre_insumos.py:140`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:243`, `app/escala/tests/test_vfase_dono_as_of.py:291` |
| Declaração da FK | `editada_por` possui `on_delete=SET_NULL`; isso é a política declarada da relação, distinta de um escritor de edição humana. `app/escala/models.py:1733` |
| Ocorrências reais em produção | **[nao verificado]**; o dado fornecido não contém linhas de células nem auditorias de edição. |

**4. O que a porta e seus selos exigem**

| Aspecto | Contrato e implementação |
|---|---|
| Operações existentes | `OPERACOES` contém `gerar`, `lavrar_veredito` e `carimbar`; não contém `editar_dia`. `app/ponto/portas/celula.py:66` |
| Gate de nomes | O teste exige exatamente esses três nomes e que todas as funções declaradas sejam chamáveis. Acrescentar a quarta operação altera essa expectativa. `app/ponto/tests/test_chokepoint_celula_gate.py:62` |
| Gate de localização | Para entidades identificadas da família, escrita deve estar na porta; allowlist e pendências são vazias. `app/ponto/tests/test_chokepoint_celula_gate.py:21`, `app/ponto/tests/test_chokepoint_celula_gate.py:25`, `app/ponto/tests/test_chokepoint_celula_gate.py:49` |
| Gate dentro da porta | Escritas detectadas devem estar dentro de funções declaradas em `OPERACOES`. `app/ponto/tests/test_chokepoint_celula_gate.py:69` |
| Limite do gate | Ignora achados com entidade não inferida; o varredor exclui migrations e testes. Portanto o gate não substitui a leitura do `save` genérico e das migrations. `app/ponto/tests/test_chokepoint_celula_gate.py:29`, `app/core/censo_escritas.py:66`, `app/ponto/portas/celula.py:684` |
| Guarda de competência | Na regeneração, carrega janelas lavradas e pula seus dias. O carregador consulta o juiz `competencia_fechada_para_celula`, considerando candidatas com exportação ou holerite publicado. `app/ponto/portas/celula.py:202`, `app/ponto/portas/celula.py:244`, `app/ponto/management/commands/gerar_celulas.py:161`, `app/ponto/management/commands/gerar_celulas.py:169`, `app/ponto/management/commands/gerar_celulas.py:173` |
| `barrados` | Regeneração conta dias barrados, guarda extremos e devolve `{dias,de,ate,texto}` se a lista foi fornecida. Também devolve recusa de vínculo sem vigência declarada. `app/ponto/portas/celula.py:139`, `app/ponto/portas/celula.py:244`, `app/ponto/portas/celula.py:474` |
| `apesar_da_lavra` | Valor verdadeiro esvazia as janelas impeditivas; o texto entra na trilha da regeneração. O código não faz validação adicional desse texto nesse ramo. `app/ponto/portas/celula.py:202`, `app/ponto/portas/celula.py:336`, `app/ponto/portas/celula.py:344` |
| Reversão | Repete a guarda de competência, mas devolve `False` ao bloquear; sua assinatura não oferece `barrados`. `app/ponto/portas/celula.py:520`, `app/ponto/portas/celula.py:560` |
| Alcance das guardas | Essas verificações estão nos corpos de regeneração/reversão. `OPERACOES` é uma declaração de funções, sem invólucro que aplique automaticamente guarda, auditoria ou idempotência a uma nova operação. `app/ponto/portas/celula.py:66`, `app/ponto/portas/celula.py:202`, `app/ponto/portas/celula.py:560` |
| Idempotência | Contrato: mesma entrada, mesmo estado e uma linha de trilha. Regeneração compara conteúdo; reversão limpa DNA anterior; lavra e carimbos retornam sem salvar quando iguais. `app/ponto/portas/celula.py:19`, `app/ponto/portas/celula.py:257`, `app/ponto/portas/celula.py:558`, `app/ponto/portas/celula.py:571`, `app/ponto/portas/celula.py:797`, `app/ponto/portas/celula.py:840`, `app/ponto/portas/celula.py:860` |
| Selos de idempotência | Exercitam geração, regeneração, lavra, carimbo e lote; há contagem adicional de chamados/perguntas para detectar duplicação. `app/ponto/tests/test_porta_celula.py:36`, `app/ponto/tests/test_porta_celula.py:46`, `app/ponto/tests/test_porta_celula.py:63`, `app/ponto/tests/test_porta_celula.py:79`, `app/ponto/tests/test_porta_celula.py:142`, `app/ponto/tests/test_porta_celula.py:165` |
| Trilha da regeneração | Emite `regenerar_celulas_vinculo` quando `n` é verdadeiro; inclui motivo, contagem de barrados e abertura da guarda. `app/ponto/portas/celula.py:327` |
| Trilha da lavra | Emite `veredito_celula`; o carimbo guarda sua trilha na própria célula. `app/ponto/portas/celula.py:33`, `app/ponto/portas/celula.py:805`, `app/ponto/portas/celula.py:820`, `app/ponto/portas/celula.py:844` |
| Auditoria e atomicidade | `evento` é escritor de `LogAuditoria`, com savepoint e comportamento best-effort; falha de auditoria não derruba o ato. Já célula e outbox são gravados no mesmo `atomic` de `salvar_e_emitir`. `app/core/observ.py:94`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/ponto/portas/celula.py:682` |
| Autor/motivo | A documentação exige autor para a trilha, mas regeneração aceita `autor=None`, `motivo=''`; ausência de autor não é recusada pela assinatura/caminho mostrado. `app/ponto/portas/celula.py:41`, `app/ponto/portas/celula.py:100`, `app/ponto/portas/celula.py:330` |

**5. `SEM_ESCRITOR` e o que o selo mede**

| Item | Conteúdo / medida |
|---|---|
| Linha 72 | Exatamente `SEM_ESCRITOR = {'editada_em', 'editada_por'}`. `app/escala/tests/test_d3_guarda_da_linha_celula.py:72` |
| Natureza da exceção | Ambos estão classificados como trilha, com escritor `None`. `app/escala/tests/test_d3_guarda_da_linha_celula.py:50` |
| Completude do catálogo | Compara `CATALOGO` aos campos concretos de `CelulaDia._meta`; campo sem linha e linha sem campo geram violação. `app/escala/tests/test_d3_guarda_da_linha_celula.py:124`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:146` |
| Existência do escritor | Reconhece atribuições de atributos, kwargs de criação/atualização e nomes em listas de campos; verifica também `auto_now_add`. É reconhecimento sintático por nome de campo. `app/escala/tests/test_d3_guarda_da_linha_celula.py:80`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:129` |
| Cobertura da impressão | Insumos devem aparecer como `cel.<campo>` em cada chamada de `impressao_insumos`; a exceção atual é `trabalha`. `app/escala/tests/test_d3_guarda_da_linha_celula.py:70`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:107`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:140` |
| Catracas | Os conjuntos de exceções só podem encolher. `app/escala/tests/test_d3_guarda_da_linha_celula.py:175` |
| Ausência de escritor | Varre Python de produção, excluindo inclusive `tests` e `migrations`, e exige zero achados para os campos em `SEM_ESCRITOR`. `app/escala/tests/test_d3_guarda_da_linha_celula.py:156`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179` |
| Nascer o escritor humano | O teste manda retirar o campo da catraca e declarar seu escritor no mesmo ato. `app/escala/tests/test_d3_guarda_da_linha_celula.py:180` |
| Unicidade | Reusa o gate da família; aqui verifica concordância entre a porta registrada e o catálogo por campo. `app/escala/tests/test_d3_guarda_da_linha_celula.py:191` |
| O selo não demonstra | Não consulta dados de produção nem prova propagação financeira da edição; suas entradas são AST, metadados e chamadas da impressão. `app/escala/tests/test_d3_guarda_da_linha_celula.py:167` |

**6. Leitores relevantes para falta/HE, cartório, fechamento e folha**

| Leitor | Dependência e decisão afetada |
|---|---|
| `EscalaColaborador.eh_dia_trabalho` | Havendo célula, retorna `trabalha`, sem filtro de origem. `app/escala/models.py:1087` |
| `marcos_do_dia`, `pausas_do_dia`, `extra_declarada_do_dia` | Consomem marcos, intervalos e extra declarada congelados no DNA. `app/escala/models.py:1117`, `app/escala/models.py:1182`, `app/escala/models.py:1227` |
| `minutos_previstos_do_dia` / `intervalo_declarado_do_dia` | Trabalho previsto, marcos e intervalos determinam minutos previsto

[truncado em 20000 de 29536 caracteres]

## 06/11 10:14 · sessao 4ea10ee07e5e · - · - · origem=Ronald (tmux)

| Q2 — leitor de produção | Decisão |
|---|---|
| `app/ponto/portas/celula.py:232` | Regeneração fora da vigência só desliga célula com `origem == 'gerada'`; preserva editada. |
| `app/ponto/portas/celula.py:257` | Regeneração dentro da vigência só reescreve `trabalha/dna` de gerada. |
| `app/ponto/portas/celula.py:558` | Reversão retorna `False` para origem diferente de gerada. |
| `app/ponto/portas/celula.py:638` | Gerador incrementa `puladas` para editada; o enriquecimento passado também exige gerada em `app/ponto/portas/celula.py:643`. |
| `app/ponto/management/commands/tripwire_celulas.py:22` | `medir_stale` só consulta geradas; editadas não são acusadas. |
| `app/escala/management/commands/celulas_contra_o_template.py:29` | Contador contra template só consulta geradas; exclui editadas. |
| `app/ponto/services/cartorio.py:365` | `editada_em` em `(desde, ate]` faz `houve_fato_novo` retornar `True`; `parcial_so_lista` usa esse resultado para decidir lista versus emissão em `app/ponto/services/cartorio.py:400`. |
| `app/chamados/veredito_lavrado.py:154` | Agrega `editada_em` com os outros timestamps no marco por colaborador; marco posterior ao julgamento torna pergunta stale em `app/chamados/veredito_lavrado.py:187`. |
| `app/escala/services/fase_12x36.py:190` | Agrega `editada_em`; timestamp posterior à lavra reabre janela passada em `app/escala/services/fase_12x36.py:192`. |
| `app/ponto/services/dia_do_colab.py:90`; `app/escala/models.py:1842`; `app/escala/management/commands/celulas_contra_o_template.py:74` | Leem `origem` para apresentação, sem ramo especial por editada nesses locais. |
| `editada_por` e motivo de edição | Nenhum leitor de negócio localizado; `editada_por` está declarado em `app/escala/models.py:1733`; não há campo motivo no catálogo completo de campos em `app/escala/tests/test_d3_guarda_da_linha_celula.py:38`. |

| Q2 — testes: leitura direta ou guarda | O que verificam | [nao verificado]
|---|---|
| `test_celula_dia.py::test_editada_intocavel_e_passado_imutavel` | Lê `cel.origem` e exige `'editada'` em `app/escala/tests/test_celula_dia.py:83`; exige trabalho editado preservado em `app/escala/tests/test_celula_dia.py:84`. |
| `test_cobranca_orfa_regeneracao.py::test_reescrita_deixa_marca_na_celula_e_guarda_o_dna_anterior` | Lê `editada_em` e exige `None`: regeneração não é edição humana, em `app/escala/tests/test_cobranca_orfa_regeneracao.py:166`. |
| `test_vfase_dono_as_of.py::test_MORDE_regenerada_em_reabre_a_competencia` | Lê `editada_em` e exige `None`, isolando regeneração, em `app/escala/tests/test_vfase_dono_as_of.py:279`. |
| `test_d3_guarda_da_linha_celula.py` | Classifica os dois campos como trilha sem escritor em `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`; verifica a catraca em `app/escala/tests/test_d3_guarda_da_linha_celula.py:177`; varre possíveis escritores em `app/escala/tests/test_d3_guarda_da_linha_celula.py:179`. |

| Q2 — testes que **escrevem fixtures** editadas e verificam consumidores | Efeito verificado | [nao verificado]
|---|---|
| `app/escala/tests/test_celula_dia.py:98` | Juiz lê trabalho editado e difere da aritmética em `app/escala/tests/test_celula_dia.py:100`. |
| `app/escala/tests/test_celula_dia.py:358` | Tripwire não acusa em `app/escala/tests/test_celula_dia.py:361`. |
| `app/escala/tests/test_selo_celula_alem_do_vinculo.py:103` | Célula humana permanece trabalhando após encerrar vínculo em `app/escala/tests/test_selo_celula_alem_do_vinculo.py:106`. |
| `app/escala/tests/test_selo_foto_folgas_relavra.py:106` | Foto de folgas preserva trabalho humano em `app/escala/tests/test_selo_foto_folgas_relavra.py:109`. |
| `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:98` | Contador contra template ignora editada em `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:99`. |
| `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:128` | Contador de folga com marcos inclui editada em `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:129`. |
| `app/escala/tests/test_sem_dia_pelo_juiz.py:46` | Célula de folga prevalece sobre template e resulta `sem_dia` em `app/escala/tests/test_sem_dia_pelo_juiz.py:65`. |
| `app/colaboradores/tests/test_vinculo_regenera_celulas.py:39` | Troca de template preserva célula humana em `app/colaboradores/tests/test_vinculo_regenera_celulas.py:45`. |
| `app/ponto/tests/test_porta_celula_dia.py:247` | Reversão recusa célula humana em `app/ponto/tests/test_porta_celula_dia.py:250`. |
| `app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `:180`, `:225` | Fixtures de folga editada; verificam reconciliação/dedup, sobrevivência do novo e absolvição da pergunta em `app/ponto/tests/test_fechar3_troca_escala_gate.py:157`, `:203`, `:249`. Não fazem comparação direta de origem nesses asserts. |
| `app/ponto/tests/test_d3_impressao_cobre_insumos.py:140` | Escreve `editada_em` junto com ata e exige hash inalterado em `app/ponto/tests/test_d3_impressao_cobre_insumos.py:141`. |
| `app/ponto/tests/test_d3_impressao_cobre_insumos.py:243` | Escreve origem, timestamp e DNA; verifica hash alterado via `_mudou` em `app/ponto/tests/test_d3_impressao_cobre_insumos.py:242`. |
| `app/escala/tests/test_vfase_dono_as_of.py:291` | Escreve somente `editada_em`; exige competência reaberta em `app/escala/tests/test_vfase_dono_as_of.py:297`. |

| Q3 | Evidência |
|---|---|
| Escritor de produção de `editada_em/editada_por`: **nenhum**. | Declaração `None` em `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`; selo que exige ausência em `app/escala/tests/test_d3_guarda_da_linha_celula.py:179`. |
| `ORIGEM_EDITADA` | Nenhuma constante localizada; o valor é literal na enumeração em `app/escala/models.py:1690`. |
| Motivo da edição da célula | Campo ausente do catálogo em `app/escala/tests/test_d3_guarda_da_linha_celula.py:38`; o catálogo é comparado aos campos concretos do modelo em `app/escala/tests/test_d3_guarda_da_linha_celula.py:171`. |

| Q5 — selo | O que mede |
|---|---|
| Conteúdo de `SEM_ESCRITOR` | Exatamente `{'editada_em', 'editada_por'}` em `app/escala/tests/test_d3_guarda_da_linha_celula.py:72`. |
| Catálogo completo | Campo vivo sem linha e linha sem campo geram violação em `app/escala/tests/test_d3_guarda_da_linha_celula.py:124`. |
| Escritor declarado | Exige escrita sintática do nome no arquivo ou `auto_now_add`; implementação em `app/escala/tests/test_d3_guarda_da_linha_celula.py:80` e `:129`. Não resolve entidade de cada atributo: reconhece nomes sintáticos. |
| Insumos | Cada chamada de `impressao_insumos` deve receber `cel.<campo>`; exceção atual `{'trabalha'}` em `app/escala/tests/test_d3_guarda_da_linha_celula.py:70`; cobrança em `:140`. |
| Catraça | Conjuntos só podem encolher em `app/escala/tests/test_d3_guarda_da_linha_celula.py:175`. |
| Ausência de escritores | Varre `.py` de produção, excluindo inclusive `tests` e `migrations`, em `app/escala/tests/test_d3_guarda_da_linha_celula.py:156`; exige zero achados para `SEM_ESCRITOR` em `:179`. |
| Unicidade | Reusa gate por entidade; aqui compara porta registrada e escritores do catálogo em `app/escala/tests/test_d3_guarda_da_linha_celula.py:191`. |
| Consequência de nascer escritor humano | Deve retirar campos da catraca e declarar escritor no mesmo ato, conforme teste em `app/escala/tests/test_d3_guarda_da_linha_celula.py:180`. |

Achado adicional: embora a docstring diga que célula humana não é julgada em `app/ponto/portas/celula.py:122`, o ramo `rejulgar` coleta qualquer célula existente e não filtra origem em `app/ponto/portas/celula.py:291`; adiciona dias a `_tocadas` em `:297`. Isso não reescreve DNA nesse ramo, mas precisa ser considerado para o censo de reescrita de saídas.

Não executei testes. Executei somente parsing AST em memória com `python3 -B`, reutilizando `campos_escritos` do selo; retornou lista vazia de escritores dos dois campos [não verificado por execução da suíte]. [nao verificado]

## 06/11 10:14 · sessao 4ece0d2822aa · - · - · origem=Ronald (tmux)

| Sítio físico de escrita em `CelulaDia` | Campos/efeito | Porta |
|---|---|---|
| `app/ponto/portas/celula.py:239` — `_cel_fv.save(...)` | Fora da vigência, transforma em folga; grava `trabalha`, `dna`. Restrito a `origem == 'gerada'` em `:232`. | Pela própria porta. |
| `app/ponto/portas/celula.py:253` — `CelulaDia.objects.create(...)` | Cria célula ausente com colaborador/data/trabalha/origem gerada/geradora/DNA. | Pela própria porta. |
| `app/ponto/portas/celula.py:270` — `cel.save(...)` | Regenera trabalha/geradora/DNA e trilha `dna_anterior`, `regenerada_em`, `regeneracoes`; só gerada (`:257`). | Pela própria porta. |
| `app/ponto/portas/celula.py:578` — `cel.save(...)` | Reverte DNA anterior, trabalha/geradora e trilha. Só gerada e com DNA anterior (`:558`). | Pela própria porta. |
| `app/ponto/portas/celula.py:633` — `CelulaDia.objects.create(...)` | Geração periódica de célula ausente. | Pela própria porta. |
| `app/ponto/portas/celula.py:645` — `cel.save(update_fields=['dna'])` | Enriquece passado com DNA nulo; somente gerada (`:643`). Editada já foi pulada (`:638-639`). | Pela própria porta. |
| `app/ponto/portas/celula.py:655` — `cel.save(...)` | Atualiza trabalha/geradora/DNA hoje/futuro; editada já foi pulada (`:638-639`). | Pela própria porta. |
| `app/ponto/portas/celula.py:684` — `salvar_e_emitir` | Save genérico pelos campos recebidos; atômico com evento quando veredito/ata mudou (`:682-686`). | Pela própria porta. |
| `app/ponto/portas/celula.py:887` — `CelulaDia.objects.bulk_update(...)` | Backfill de ausência/entrada/saída/insumos_em. | Pela própria porta. |

| Sítios lógicos que usam o save genérico de `:684` | Efeito |
|---|---|
| `app/ponto/portas/celula.py:787` | Cartório sobre veredito de admin: atualiza ata/status; preserva decisão admin (`:781-788`). |
| `app/ponto/portas/celula.py:804` | Só status mudou; grava status sem mover veredito_em (`:797-810`). |
| `app/ponto/portas/celula.py:819` | Lavra veredito/via/tempo/protestos/ata/status (`:811-825`). |
| `app/ponto/portas/celula.py:851` | Carimbo julgada_em/impressão/ausência/entrada/saída/insumos_em e campos lavrados (`:844-851`). |
| `app/ponto/portas/celula.py:866` | Grava precedência dentro de ata (`:859-866`). |

O varredor próprio vê oito escritas atribuídas a CelulaDia; `salvar_e_emitir:684` fica sem entidade inferida, pois recebe `cel` como parâmetro e lista dinâmica de campos — a limitação dessa inferência está documentada em `app/core/censo_escritas.py:43-47`. Portanto a lista de nove acima exige leitura manual além do gate (`app/ponto/tests/test_chokepoint_celula_gate.py:25-35`).

| Migrations | Escrita fora da porta? / efeito sobre editada |
|---|---|
| `app/escala/migrations/0030_celuladia.py:17` | `CreateModel`, schema; declara trabalha/origem/editada_em/editada_por (`:22-28`). |
| `app/escala/migrations/0031_celuladia_dna.py:13` | `AddField dna`; nenhuma lógica por origem (`:13-17`). |
| `app/escala/migrations/0032_celuladia_impressao_celuladia_julgada_em.py:13` e `:18` | Acrescenta impressão/julgada_em; schema (`:13-22`). |
| `app/escala/migrations/0036_b5_celula_veredito.py:13`, `:18`, `:23`, `:28` | Acrescenta protestos/veredito/veredito_em/veredito_via; schema. |
| `app/escala/migrations/0037_hx_ata_celula.py:13` | Acrescenta ata; schema. |
| `app/escala/migrations/0038_celuladia_dna_anterior_celuladia_regeneracoes_and_more.py:13`, `:18`, `:23` | Acrescenta dna_anterior/regeneracoes/regenerada_em; regeneracoes tem default 0 (`:21`). |
| `app/escala/migrations/0039_b5f2_celula_insumos.py:15`, `:20`, `:25`, `:30` | Acrescenta ausência/insumos_em/entrada/saída; schema. |
| `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13` e `:18` | Acrescenta status_dia/status_rotulo; schema. |

Não encontrei migration de dados `RunPython`/`RunSQL` que reescreva `CelulaDia` na cópia; as operações encontradas são as listadas acima. Não encontrei uso de CelulaDia em admin/forms/serializers nem DML SQL de CelulaDia contra produção; universalidade fora da cópia [não verificado].

Há SQL fora da porta em `bin/sombra_incremental.sh:61-68`, com `escala_celuladia` na lista de tabelas (`:35`), mas o destino declarado é `PG=sombra_db`, `ALVO=sombra` (`:20`); a produção é fonte de SELECT/COPY TO (`:55-57`), não destino de escrita desse script.

| Contraexemplo/caminho | Respeita `origem='editada'`? |
|---|---|
| `regenerar_celulas_vinculo` | Sim para trabalha/DNA: ramo fora da vigência exige gerada (`app/ponto/portas/celula.py:232-239`); ramo normal só reescreve gerada (`:257-271`). |
| `regenerar_celulas_dia` | Delega à mesma regeneração, com intervalo de um dia e sem extensão (`app/ponto/portas/celula.py:514-517`). |
| `reverter_regeneracao_dia` | Sim: retorna False se origem não é gerada (`app/ponto/portas/celula.py:558-559`). |
| `gerar_celulas_periodo` | Sim: editada incrementa puladas e não entra nos ramos seguintes (`app/ponto/portas/celula.py:638-655`). |
| `lavrar_veredito` | Sem guarda de origem; pode relavrar derivados da editada. A proteção é decisão `veredito_via == 'admin'`, e ainda permite ata/status (`app/ponto/portas/celula.py:781-788`); grava veredito/ata/status (`:811-819`). Não escreve trabalha/DNA. |
| `carimbar` | Sem guarda de origem; escreve julgada_em/impressão/ausência/entrada/saída/insumos_em (`app/ponto/portas/celula.py:839-851`). |
| `carimbar_precedencia` | Sem guarda de origem; altera ata (`app/ponto/portas/celula.py:859-866`). |
| `carimbar_insumos_em_lote` | Sem guarda de origem; altera insumos quando diferentes (`app/ponto/portas/celula.py:878-887`). |
| `regenerar_celulas_vinculo(..., rejulgar=...)` | Nuance: a entrada em `_tocadas` exige célula existente e outras guardas, mas não origem (`app/ponto/portas/celula.py:291-298`). O julgamento posterior exclui por **veredito humano**, não por origem (`:390-402`). Assim não reescreve trabalha/DNA editados, mas esse ramo permite relavrar seus derivados. |
| Command `gerar_celulas` | Reexporta funções da porta (`app/ponto/management/commands/gerar_celulas.py:13`), chama geração periódica (`:310-312`); herda pulo da editada. |
| Command `corrigir_escala_retroativa` | Chama regeneração com barrados/motivo/apesar_da_lavra (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:229-238`); herda guarda da origem para DNA. |
| Command `aplicar_col900` | Chama reversão (`app/ponto/management/commands/aplicar_col900.py:77-78`) ou regeneração diária (`:115-119`); herda guardas. |
| Command `backfill_b5_insumos` | Seleciona células julgadas sem filtro de origem (`app/ponto/management/commands/backfill_b5_insumos.py:60-63`) e chama carimbo em lote (`:81`, `:84`); pode carimbar editada. |
| Command `processar_cartorio` | Carrega células passadas sem filtro de origem e passa a `julgar_colab` (`app/ponto/management/commands/processar_cartorio.py:61-65`); pode relavrar derivados. |
| Command `lavrar_status_competencia` | Carrega células da janela sem filtro de origem e chama julgamento com completar_status (`app/ponto/management/commands/lavrar_status_competencia.py:50-57`). |
| Command `curar_residuo_bug115` | Gera pela porta e força julgamento (`app/ponto/management/commands/curar_residuo_bug115.py:132-143`); geração protege editada, julgamento pode mudar derivados. |

Chamadores de lavra em produção: delegação `app/escala/models.py:1826-1828`; decisão humana `app/ponto/services/veredito_celula.py:330`; cartório `app/ponto/services/cartorio.py:889`, `:1242`, `:1802`. Todos chegam à porta, não são save independente.

## 06/11 10:13 · sessao 6c6818576678 · - · - · origem=Ronald (tmux)

Q6 — leitores que dependem de `trabalha`/DNA (a origem não é filtro nestes trechos).

| Leitor | Decisão afetada | Evidência |
|---|---|---|
| Autoridades `EscalaColaborador.eh_dia_trabalho`, `marcos_do_dia`, `pausas_do_dia`, `extra_declarada_do_dia` | Retornam respectivamente `cel.trabalha`, marcos congelados, todos os intervalos congelados e extra declarada. Leem independentemente de `origem`. | `app/escala/models.py:1087-1096`, `1117-1133`, `1182-1208`, `1227-1239` |
| `minutos_previstos_do_dia` / `intervalo_declarado_do_dia` | Dia não explicitamente de trabalho vale zero; DNA determina duração e intervalos. | `app/escala/utils.py:2229-2248`, `2277-2295`, `2312-2326` |
| `classificar_falta` | `trabalha=False` devolve `(None,None,None)`; DNA determina quais batidas/marcos eram esperados. | `app/escala/servico_jornada.py:279-304`, `427-435` |
| Classificador de procedência de falta | Sem batidas, pergunta ao vínculo: False → `improcedente`, True → `falta_real`, None → `ambiguo`. | `app/escala/servico_jornada.py:533-546` (nome da função deve ser confirmado ao redor da linha 500) |
| Precedência `fatos_do_periodo` | Havendo resposta de `cel.trabalha`, utiliza-a como `previsto`; afeta tratamento de feriado/trabalho. | `app/ponto/precedencia.py:357-377` (nome da função deve ser confirmado) |
| Porta de precedência para faltas/ausências | Veta lançamento de tipo que só cabe em jornada quando vínculo responde False. | `app/ponto/precedencia.py:536-543` |
| Gerador da grade `montar_grade_prevista_periodo` | Pula dia de folga, usa marcos/pausas congelados para alinhar batidas e medir previsto. Essa grade alimenta cartório. | `app/escala/utils.py:1619-1660`; chamada pelo cartório em `app/ponto/services/cartorio.py:1126` |
| Motor base, `calcular_mes` | `eh_dia_trabalho=False` manda o período para `periodos_ft`, caso contrário para `periodos`; folga também desativa janela de HE. | `app/ponto/motor_calculo_v2.py:2445-2459`, `2468-2494`, `1522-1536` |
| Motor: faltas e DSR | Dias previstos alimentam faltas injustificadas, quantidade de faltas, horas de falta e abono. | `app/ponto/motor_calculo_v2.py:2593-2622`; origem dos dias previstos em `app/ponto/services/fechamento.py:720-732` |
| Motor: jornada contínua com múltiplos pares | Marcos determinam se deve zerar atraso de pares intermediários e saída antecipada de pares anteriores ao último. | `app/ponto/motor_calculo_v2.py:2686-2716` |
| Motor turno partido, Zona 5 | DNA com os quatro marcos habilita e fornece régua de atraso/saída antecipada. | `app/ponto/motor_calculo_v2.py:3014-3049` |
| Motor: limite de HE diária | Marcos e primeiro intervalo definem limite diário; excesso vira HE50/HE100. | `app/ponto/motor_calculo_v2.py:3363-3398` |
| Motor: banco de horas | Trabalho previsto determina proporcionalização da jornada semanal e saldo. | `app/ponto/motor_calculo_v2.py:3420-3431` |
| `juiz_batida.periodos_do_dia` (usado por motor e folha) | Lê DNA e ata; DNA sem marcos, quantidade diferente ou horas divergentes retorna `desalinhado`, em vez de atribuir papéis às batidas. | `app/ponto/juiz_batida.py:149-174`, `196-204`, `226-235` |
| Alimentação do calculador | Consulta previsto/intervalo pelas autoridades e pergunta `eh_dia_trabalho` para classificar folga trabalhada. | `app/ponto/calculador/alimentacao.py:374-382`, `434-445`; regra pura em `app/ponto/calculador/regras.py:367-396` |
| Cartório | DNA compõe impressão de insumos, lampadas e ata; `trabalha=False` cala protestos de chamados; insumo trabalha+ata alimenta `classificar_dia`; resultado é lavrado. | `app/ponto/services/cartorio.py:443-456`, `565-586`, `1348-1364`, `1407-1424`, `1800-1807` |
| Cartório: emissão e furo parcial | Só examina marcos se vínculo responde trabalho=True; usa marcos do dia. | `app/ponto/services/cartorio.py:1718-1719`, `1853-1856` |
| Fechamento | Carrega células em lote, fornece mapa ao motor, monta datas previstas com `eh_dia_trabalho`, apura fatias e resultado. | `app/ponto/services/fechamento.py:595-604`, `720-746`, `269-282` |
| Fechamento: métricas derivadas da grade | Tipo do dia/previsto afetam dias previstos, minutos previstos, abonos e realizado; retentores viram `motivos_espelho`. | `app/ponto/services/fechamento.py:997-998`, `1030-1052` |
| Leitor soberano `grade_da_celula` | **Prefere ata existente**: `ata.tipo_dia` antes de trabalha e `ata.minutos_previstos` antes de consultar autoridade. Na ausência desses valores usa trabalha/DNA; transporta trabalha+ata ao suprajuiz. | `app/escala/services/leitor_celula.py:380-411` |
| Folha `grade_do_fechamento` | Abre a grade soberana da competência, portanto herda a prioridade da ata. | `app/folha/export.py:329-338` |
| Folha: retenção | Lê veredito/protestos lavrados; DNA também pode mudar resultado de `nao_retem_pela_pausa` porque esta consulta o juiz de períodos. Veredito None com trabalha não None em dia passado retém por `sem_celula`. | `app/folha/export.py:1140-1147`, `1165`, `1206`, `1233-1256` |
| Folha: rubricas | HE em TXT sai de campos já apurados do fechamento, portanto depende da reapuração; não recalcula diretamente o DNA nessa emissão. | `app/folha/export.py:420-446` |

Ressalvas indispensáveis:

1. Não se pode afirmar que todo leitor muda imediatamente quando a célula muda: grade soberana dá preferência à ata existente, e folha lê veredito/fechamento já lavrados. `app/escala/services/leitor_celula.py:380-395`; `app/folha/export.py:1233-1256`, `420-446`.
2. `impressao_insumos` recebe DNA e inclui `dna_versao`/`marcos`, mas não recebe `trabalha`; `_em_dia` pula julgamento se impressão/veredito já estão em dia e não há `forcar`. Assim, editar só `trabalha` não demonstra rejulgamento pelo cartório. `app/ponto/services/cartorio.py:443-456`, `1348-1364`.
3. A ocorrência efetiva de qualquer mudança em produção é `[não verificado]`; foram identificadas dependências no código, sem dados de banco/log.

## 06/11 09:49 · sessao a483d8cfa564 · PEP-REENTRADA · verificador · origem=Code

Execução dos testes e mutantes: **[nao verificado]**. As conclusões abaixo são por inspeção do código. [nao verificado]

1. **Os dois testes têm mordida local, com limites diferentes.** [nao verificado]

   No **laço sem credor**, retirar isoladamente a guarda deve permitir o carimbo da P e contrariar `assertIsNone`: o teste deixa sua validação nula e força o C6 a responder “morta”. A irmã sem destino comprova a passagem pelo segundo laço, pois fica fora do filtro do primeiro. (`app/chamados/tests/test_pep_reentrada.py:227`, `app/chamados/tests/test_pep_reentrada.py:229`, `app/chamados/tests/test_pep_reentrada.py:233`, `app/chamados/tests/test_pep_reentrada.py:239`; `app/chamados/services/disputa_emissao.py:1573`, `app/chamados/services/disputa_emissao.py:1665`, `app/chamados/services/disputa_emissao.py:1671`.)

   Entretanto, substituir essa guarda por **`if q.chamado_destino_id: continue`** também preservaria o teste: P tem destino; irmã não tem. Portanto, ele distingue essas duas perguntas, mas não prova sozinho que a distinção depende exclusivamente do voo. (`app/chamados/tests/test_pep_reentrada.py:58`, `app/chamados/tests/test_pep_reentrada.py:227`, `app/chamados/tests/test_pep_reentrada.py:238`; `app/chamados/services/disputa_emissao.py:1671`.)

   **O mock altera também a porta de escrita.** Seus leitores potenciais nessa chamada são:

   - **Laço 1:** importa a função mockada, mas a guarda de voo pula P antes de consultá-la. (`app/chamados/services/disputa_emissao.py:1577`, `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1592`.)
   - **`carimbar`:** importa novamente a mesma função e a consulta depois de aplicar o carimbo e os campos de resposta em memória. O mock força essa aceitação também. (`app/chamados/models.py:2576`, `app/chamados/models.py:2586`, `app/chamados/models.py:2596`.)
   - **Cauda:** C7 chega à função por `fio_vivo`, e `fechar` também a importa. Contudo, neste teste a guarda de voo impede essa avaliação; mesmo removendo-a, `respostas_sem_veredito` ainda considera o texto não validado da P. (`app/chamados/services/disputa_emissao.py:1710`; `app/chamados/models.py:1784`, `app/chamados/models.py:1794`, `app/chamados/models.py:1815`; `app/chamados/juizes.py:1242`.)

   Há diferença concreta para o código real: `resposta='responder'` preenche `respondida_em`, sem preencher `validada_em`. Na irmã criada sem célula, o C6 real não comprova concordância com a luz e mantém viva essa resposta, recusando o carimbo. O teste força o resultado contrário na porta; demonstra a guarda **sob C6 artificial**, não a integração real desse cenário. (`app/chamados/tests/test_pep_reentrada.py:227`, `app/chamados/tests/test_pep_reentrada.py:233`; `app/chamados/models.py:2587`, `app/chamados/models.py:2591`, `app/chamados/models.py:2600`; `app/chamados/juizes.py:890`, `app/chamados/juizes.py:1101`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1162`.)

   No **S144**, a irmã corrige a vacuidade apontada: remover a guarda carimbaria P; desligar o receiver ou “pular todas” deixaria a irmã sem a via exigida; continuar bloqueando P depois do voo contrariaria o controle `[p.pk]`. As verificações acontecem dentro do bloco, pelo receiver síncrono. (`app/chamados/tests/test_pep_reentrada.py:252`, `app/chamados/tests/test_pep_reentrada.py:256`, `app/chamados/tests/test_pep_reentrada.py:257`, `app/chamados/tests/test_pep_reentrada.py:259`; `app/chamados/signals.py:139`; `app/chamados/services/materializacao.py:1399`, `app/chamados/services/materializacao.py:1405`.)

   **Mutantes ainda sobreviventes:** desligar S143 deixa os dois verdes — o primeiro chama o resolver diretamente; o segundo exercita S144. Além disso, ambos verificam apenas `materializada_em` da P durante o voo: uma escrita indevida apenas de `via_resolucao` ou `materializada_por`, seguida do `continue`, escaparia desses dois testes. Isso é limite desses testes, não afirmação de que tal escritor exista na cópia. (`app/chamados/signals.py:92`; `app/chamados/tests/test_pep_reentrada.py:235`, `app/chamados/tests/test_pep_reentrada.py:239`, `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/tests/test_pep_reentrada.py:256`.)

2. **Há dois descartes incorretos e uma justificativa incompleta.**

   **Receiver S143 vivo:** o descarte está errado. `UM_escritor` exige destino encerrado, batida e carimbo único do materializador; tudo isso também ocorre com S143 desligado, porque o próprio materializador reconcilia o destino e carimba P. `S143_nao_fecha` igualmente observaria apenas o fechamento normal posterior, fora do voo. Compartilhar o fixture não prova que o receiver executou. (`app/chamados/tests/test_pep_reentrada.py:87`, `app/chamados/tests/test_pep_reentrada.py:91`, `app/chamados/tests/test_pep_reentrada.py:113`; `app/chamados/services/materializacao.py:746`, `app/chamados/services/materializacao.py:1251`, `app/chamados/services/materializacao.py:1352`.)

   **Divergência de documentação:** reconhecer e adiar não cumpre a instrução expressa de corrigir no mesmo commit. A tabela ainda diz “disputa fecha […] SEM juiz”, enquanto C7 está implementado e registrado. A divergência pode ser anterior à obra; o descarte como dispensável nesta entrega continua contrário à regra do próprio documento. (`app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:54`; `app/chamados/models.py:1789`; `app/core/juizes.py:267`.)

   **Censo de escrita crua:** “só `_escrever_carimbo` e descarimbos” é incompleto para `via_resolucao`: existem duas portas adicionais que escrevem somente a via, inclusive reconhecidas pelo selo AST. Isso corrige a justificativa, mas não demonstra escritor adicional durante o voo. (`app/chamados/models.py:2673`; `app/chamados/services/relavra_via.py:123`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`.)

3. **Nos caminhos atuais examinados, não sobra outro escritor da P em voo; continuam existindo leitores.**

   **Quem escreve:** o materializador chama `carimbar`; as atribuições do trio ficam em `_escrever_carimbo`, e a persistência na porta. Os carimbos de identidade por paridade/colisão também pertencem ao materializador. A chamada de recusa no `finally` da unidade ocorre depois da saída do contexto de voo. (`app/chamados/services/materializacao.py:1251`, `app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1271`; `app/chamados/models.py:2556`, `app/chamados/models.py:2609`, `app/chamados/models.py:2644`.)

   **Outros escritores alcançáveis pela cadeia:** S143 pula P nos dois laços e barra sua cauda; S144 pula P antes de carimbar. O absolvedor do reconciliador exige validação nula; o descarimbo exige C6 vivo; o dedup também seleciona perguntas vivas. Os chamadores atuais materializam com validação gravada, que faz C6 retornar morto já no primeiro degrau. (`app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`; `app/chamados/services/materializacao.py:1399`, `app/chamados/services/materializacao.py:1343`; `app/chamados/services/validacao.py:396`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/reconciliador.py:620`; `app/chamados/services/relavra_via.py:227`; `app/chamados/services/fio.py:935`; `app/chamados/juizes.py:1136`.)

   **Quem lê o trio nessa cadeia:**

   | Leitor | Campos e finalidade |
   |---|---|
   | S143 | Seleciona por `materializada_em`; o laço sem credor também lê a via para a trilha, depois da guarda de voo. (`app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/disputa_emissao.py:1665`, `app/chamados/services/disputa_emissao.py:1676`.) |
   | S144 | Seleciona por `materializada_em`, antes da guarda. (`app/chamados/services/materializacao.py:1392`.) |
   | `carimbar` | Lê os três campos para restaurá-los se houver recusa ou exceção. (`app/chamados/models.py:2583`, `app/chamados/models.py:2619`.) |
   | C6 e juiz do lastro | Leem instante/via; C6 da P validada retorna antes desses degraus. (`app/chamados/juizes.py:647`, `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1207`.) |
   | C7 e crédito de irmãs | Leem `materializada_em` nas pendências; são chamados pela cadeia de fechamento/reconciliação. (`app/chamados/models.py:1784`; `app/chamados/juizes.py:1748`; `app/chamados/services/materializacao.py:737`.) |
   | Recusa | Relê instante e via antes de decidir o carimbo de colisão. (`app/chamados/services/materializacao.py:1064`.) |

   A exclusividade é dos caminhos examinados, não um bloqueio universal na porta: `materializacao_em_voo` registra o PK, mas `carimbar_identidade` e `descarimbar` não consultam esse sinal. (`app/chamados/services/materializacao.py:1120`, `app/chamados/services/materializacao.py:1129`; `app/chamados/models.py:2625`, `app/chamados/models.py:2646`.)

4. **No delta V3, a ressalva é de cobertura do selo, não uma nova infração demonstrada no código de produção.**

   Os dois testes mordem as remoções locais descritas; o S144 ganhou controle positivo efetivo. Persistem os limites do mock amplo, da identidade confundida com presença de destino e da conexão S143 sem prova positiva. Portanto, não os classifico como totalmente “sem mordida”, nem como prova completa do contrato. (`app/chamados/tests/test_pep_reentrada.py:227`, `app/chamados/tests/test_pep_reentrada.py:233`, `app/chamados/tests/test_pep_reentrada.py:235`, `app/chamados/tests/test_pep_reentrada.py:257`.)

   O delta fornecido altera testes; nele não identifiquei novo segundo juiz/escritor de produção, relógio fora da borda, derivação paralela, inversão de trava ou estado de módulo sobrevivente. No código completo, o voo mantém `reset` em `finally` e termina antes da cauda normal de fechamento. Pouso efetivo e efeitos em produção: **[nao verificado]**. (`app/chamados/tests/test_pep_reentrada.py:219`, `app/chamados/tests/test_pep_reentrada.py:241`; `app/chamados/services/materializacao.py:1123`, `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1352`.)

## 06/11 09:48 · sessao 4f4e1062af72 · - · - · origem=Ronald (tmux)

- **O teste sem credor morde a remoção isolada da guarda**, por análise estática: a P entra em `_abertas` com validação nula; sem o `continue`, o mock permite `carimbar`, contrariando `assertIsNone`. A irmã sem destino comprova execução desse laço, pois não participa do primeiro filtro por destino. (`app/chamados/tests/test_pep_reentrada.py:227`, `:229`, `:233`, `:239`; `app/chamados/services/disputa_emissao.py:1573`, `:1665`, `:1671`, `:1686`).

- **O mock substitui mais que a consulta desse laço.** O primeiro laço importa a função mockada; `carimbar` também a importa e consulta depois de aplicar os campos; C7 chega a ela por `fio_vivo`, e `fechar` importa a mesma função. Neste teste, as guardas de voo pulam a P no primeiro laço e na cauda. (`app/chamados/services/disputa_emissao.py:1577`, `:1586`, `:1592`, `:1710`; `app/chamados/models.py:2576`, `:2596`, `:1793`, `:1815`, `:1837`; `app/chamados/juizes.py:1242`).

- **Há comportamento real removido pelo mock:** `resposta='responder'` preenche `respondida_em`, mas não valida; o C6 real prioriza essa resposta e exige concordância com a luz. A irmã foi criada sem célula/destino; o teste não demonstra concordância real e força a aceitação da porta. Portanto, prova a guarda sob C6 artificial, sem demonstrar a integração real desse cenário. (`app/chamados/models.py:2587`, `:2591`, `:2596`; `app/chamados/juizes.py:1150`, `:1162`, `:1101`; `app/chamados/tests/test_pep_reentrada.py:227`, `:233`).

- **Mutante sobrevivente do primeiro teste:** substituir a guarda de voo do laço sem credor por `if q.chamado_destino_id: continue`. A P tem destino e a irmã não, preservando exatamente os dois asserts sem consultar voo. (`app/chamados/services/disputa_emissao.py:1671`; `app/chamados/tests/test_pep_reentrada.py:58`, `:227`, `:238`, `:239`).

- **O S144 ganhou mordida adequada para os mutantes indicados:** remover sua guarda carimba P; desligar receiver ou pular todas deixa a irmã sem `VIA_JUIZ_APROVOU`; manter o bloqueio depois do voo viola o retorno `[p.pk]`. São previsões estáticas, sem execução. (`app/chamados/services/materializacao.py:1399`, `:1405`; `app/chamados/signals.py:139`; `app/chamados/tests/test_pep_reentrada.py:252`, `:256`, `:257`, `:259`).

- **Os dois novos testes deixam sobreviver um S143 desligado:** o primeiro chama o resolver diretamente; o segundo depende do receiver S144. Também deixam sem aferição a preservação de `materializada_por` e `via_resolucao` da P durante o voo: verificam somente `materializada_em`. (`app/chamados/tests/test_pep_reentrada.py:235`, `:239`, `:253`, `:256`; `app/chamados/signals.py:92`, `:140`).

- **O descarte “S143_nao_fecha sem prova de receiver vivo” está errado.** O `UM_escritor` exige destino fechado, batida e único carimbo do materializador, resultado igualmente possível com S143 desligado. O próprio materializador reconcilia o destino, carimba P e chama a cauda depois da unidade; o teste de fecho apenas observaria seu fechamento normal fora do voo. (`app/chamados/tests/test_pep_reentrada.py:83`, `:87`, `:91`, `:108`, `:113`; `app/chamados/services/materializacao.py:746`, `:1251`, `:1350`, `:1352`; `app/chamados/signals.py:92`).

## 06/11 09:47 · sessao ebc4941dec11 · - · - · origem=Ronald (tmux)

Nos chamadores atuais, não achei segundo escritor da P em voo: a validação já está gravada antes de `_materializar_uma` em `app/chamados/services/validacao.py:395-424`; o laço normal exige `validada_em` não nulo em `app/chamados/services/materializacao.py:1343-1350`; o universo do passivo também exige validação em `app/chamados/services/vigia_carimbo.py:96-98`.

Escritores alcançados pelo bloco:

- Materializador: carimbo normal em `app/chamados/services/materializacao.py:1251-1252`; identidade por paridade em `app/chamados/services/materializacao.py:899`; identidade por colisão na porta de recusa em `app/chamados/services/materializacao.py:1064-1075`. As atribuições do trio passam por `app/chamados/models.py:2556-2560`; a persistência sai por `app/chamados/models.py:2609` ou `app/chamados/models.py:2644`.
- S143: primeiro laço guardado antes de `carimbar` em `app/chamados/services/disputa_emissao.py:1586-1587`, escrita em `app/chamados/services/disputa_emissao.py:1612-1613`; segundo laço guardado em `app/chamados/services/disputa_emissao.py:1671-1672`, escrita em `app/chamados/services/disputa_emissao.py:1686`; cauda guardada em `app/chamados/services/disputa_emissao.py:1710-1711`.
- S144: receiver síncrono em `app/chamados/signals.py:139-140`; guarda em `app/chamados/services/materializacao.py:1399-1400` antes da escrita em `app/chamados/services/materializacao.py:1405`.
- Absolvedor do reconciliador: potencial escrita em `app/chamados/reconciliador.py:586`, mas a seleção exige `validada_em` nulo em `app/chamados/reconciliador.py:617-620`, excluindo a P dos chamadores atuais.
- Lavra sem turno: potencial escrita em `app/chamados/reconciliador.py:405`, mas recusa texto em `app/chamados/reconciliador.py:364-365`; a P validada também é morta pelo C6 em `app/chamados/juizes.py:1135-1137`.
- Descarimbo da fábrica: o julgamento pode coletar carimbadas e chamar a porta em `app/chamados/services/fabrica_da_celula.py:118-125` e `app/chamados/services/fabrica_da_celula.py:163-166`; a porta só descarimba se o C6 der viva em `app/chamados/services/relavra_via.py:221-231`, impossível para a P validada pelo degrau inicial em `app/chamados/juizes.py:1135-1137`.
- Dedup do fio: só considera perguntas vivas pelo C6 em `app/chamados/services/fio.py:931-943`; portanto não alcança a P validada. A escrita de identidade existe em `app/chamados/services/fio.py:891-894`.

Leitores relevantes dentro da cadeia:

- S143 seleciona por `materializada_em` em `app/chamados/services/disputa_emissao.py:1572-1575`; o segundo laço lê `materializada_em` e `via_resolucao` em `app/chamados/services/disputa_emissao.py:1665-1678`.
- S144 seleciona por `materializada_em` em `app/chamados/services/materializacao.py:1392-1393`.
- A porta `carimbar` lê o trio anterior para desfazer em memória em `app/chamados/models.py:2583-2584`; aplica-o antes de consultar o C6 em `app/chamados/models.py:2586-2596`.
- C6 lê via e instante em `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1177` e `app/chamados/juizes.py:1207`; a P validada retorna antes desses degraus em `app/chamados/juizes.py:1135-1137`.
- Juiz do lastro lê via/instante em `app/chamados/juizes.py:647-673`; o materializador o chama antes do carimbo em `app/chamados/services/materializacao.py:1206-1207`.
- C7 lê `materializada_em` para respostas sem veredito em `app/chamados/models.py:1783-1787`, depois consulta o fio em `app/chamados/models.py:1789-1794`.
- Crédito de irmãs lê instante na consulta em `app/chamados/juizes.py:1747-1753`; é chamado antes da reconciliação do destino em `app/chamados/services/materializacao.py:736-746`.
- Recusa relê instante/via em `app/chamados/services/materializacao.py:1064-1068`.

A justificativa da triagem “só `_escrever_carimbo` e descarimbos” é literalmente incompleta para `via_resolucao`: há escrita direta em `app/chamados/models.py:2673-2674`, usada pelo comando em `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; e há re-lavra direta em `app/chamados/services/relavra_via.py:123-125`. Isso não demonstra segundo escritor dentro do bloco: o primeiro é comando, e a chamada reativa de re-lavra foi retirada, conforme a cadeia documentada em `app/ponto/signals.py:94-105`.

A exclusividade demonstrada vale para os chamadores atuais; `materializacao_em_voo` não é intertravamento universal das portas de escrita, pois apenas registra o PK e verifica atomicidade em `app/chamados/services/materializacao.py:1115-1129`, enquanto `carimbar_identidade` e `descarimbar` não consultam o sinal em `app/chamados/models.py:2625-2652`.

_2207 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
