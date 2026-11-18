# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 15:15** · publicado em **18/11/2026 15:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 15:15 · sessao 4335686bde57 · - · - · origem=Ronald (tmux)

Leitura de X4, A9b, X11 e do clique mudo concluída e enviada ao agente principal, com referências ao código e aos testes localizados. Os testes não foram executados. [app/chamados/tests/test_e4_expand_orm.py:63](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/tests/test_e4_expand_orm.py:63)

## 18/11 15:15 · sessao 156e23fc3fa6 · - · - · origem=Ronald (tmux)

Levantamento enviado ao agente principal, com os pontos de escrita e leitura localizados, inclusive consumidores fora de `app`, e com a ressalva de que o censo completo permanece **[não verificado]**.

## 18/11 15:14 · sessao 1a24b3e4db43 · - · - · origem=Ronald (tmux)

Leitura de P8, P9 e P16 enviada ao agente principal, com estados de entrada, juízes, operações, campos, retornos e testes identificados. Os testes foram apenas lidos, não executados [não verificado]. [nao verificado]

A cópia não contém `.git`; portanto, a identidade exata do HEAD ficou **[não verificado]**.

## 18/11 15:11 · sessao 0d43c2e84e99 · E4C-F6F7 · construtor · origem=Code

Os dois selos foram alterados para rejeitar chamadas reflexivas indicadas, reatribuições e mutações sintáticas de `perguntas`, e exigir seu uso no campo `perguntas` da resposta. Também foi preservada a rejeição de leitura de `consulta` antes de `pendentes`. [test_pergunta_no_app.py:29](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_pergunta_no_app.py:29), [test_contract_pergunta_viva.py:26](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:26), [test_e4c_f6f7_c7_sem_banco.py:124](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:124).

A view atual lê `consulta['pendentes']` e usa `perguntas` na resposta; não foram encontradas as chamadas reflexivas indicadas em seu corpo. [views.py:2526](/home/ronald/wt-codex-e4c-f6f7-132608/app/api/views.py:2526), [views.py:2542](/home/ronald/wt-codex-e4c-f6f7-132608/app/api/views.py:2542), [views.py:2583](/home/ronald/wt-codex-e4c-f6f7-132608/app/api/views.py:2583).

O comando abaixo executa **cada mutante como subteste**, contra cada selo. A lista inclui reflexão, reatribuição, exclusão, alteração de item e atributo, métodos e substituição do campo da resposta. [test_e4c_f6f7_c7_sem_banco.py:88](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:88)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.CosturasSemDjango.test_reflexao_e_mutacao_de_perguntas_MORDEM
```

Antes da cura, a bateria inicial produziu o RED abaixo; quatro variantes já eram rejeitadas. As variantes adicionadas posteriormente passaram na bateria final, mas seu RED individual anterior à cura é **[não verificado]**.

```text
AssertionError: True is not false
Ran 1 test in 0.650s
FAILED (failures=30)
```

Após a cura, o módulo completo sem banco passou, incluindo os mutantes antigos e novos: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco
```

```text
Ran 6 tests in 1.540s
OK
```

A detecção de mutação indireta por aliases ou por funções chamadas com `perguntas` é **[não verificado]**; os selos implementam as verificações sintáticas descritas. [test_pergunta_no_app.py:49](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_pergunta_no_app.py:49)

LEI-AKITA: padrao=Architecture Conformance Test.

## 18/11 15:09 · sessao 626cbd9f7e08 · E4C-F6F7 · verificador · origem=Code

**PROCEDE.** O c9 fecha os dois exemplos da volta 2 e rejeita os usos diretos testados entre a consulta e a leitura. Ainda passam, porém, mutantes que quebram a ligação entre a consulta e o que o endpoint entrega. Trata-se de **selo sem mordida suficiente**, não de uma derivação paralela ou de um segundo juiz introduzido pelo delta. [test_pergunta_no_app.py:46](app/chamados/tests/test_pergunta_no_app.py:46), [test_contract_pergunta_viva.py:43](app/chamados/tests/test_contract_pergunta_viva.py:43), [views.py:2541](app/api/views.py:2541).

1. **Bloqueia — os selos ainda aceitam costura quebrada.** Executei ambos os helpers extraídos em memória. Eles aceitam a view atual e rejeitam os mutantes diretos testados: `del consulta`, alteração de item ou atributo, chamada de método, passagem como argumento, alias e uso em bloco aninhado. A view atual faz a leitura de `pendentes` imediatamente após a consulta; seu uso de `historico` vem depois. [test_pergunta_no_app.py:50](app/chamados/tests/test_pergunta_no_app.py:50), [test_contract_pergunta_viva.py:47](app/chamados/tests/test_contract_pergunta_viva.py:47), [views.py:2541](app/api/views.py:2541).

   Ambos, entretanto, retornam `True` quando se insere esta linha imediatamente após a consulta:

   ```python
   locals()['consulta']['pendentes'].clear()
   ```

   O acesso indireto altera a lista antes da leitura, mas a árvore sintática dessa linha não contém um `ast.Name` chamado `consulta`: o nome está numa string. Assim, a afirmação de que os selos rejeitam **qualquer uso** é ampla demais. Também retornaram `True` para uma sobrescrita de `perguntas = []` logo após a leitura; a view usa `perguntas` para montar a resposta. Essa segunda lacuna já está fora do intervalo especificamente corrigido pelo c9; não há evidência de que tenha sido introduzida pelo delta. [test_pergunta_no_app.py:51](app/chamados/tests/test_pergunta_no_app.py:51), [test_contract_pergunta_viva.py:48](app/chamados/tests/test_contract_pergunta_viva.py:48), [views.py:2574](app/api/views.py:2574).

2. **Ressalva — uso legítimo é rejeitado.** Inserir `historico = consulta['historico']` entre a consulta e a leitura de `pendentes` fez ambos os helpers retornarem `False`, embora essa leitura, por si só, não altere as pendentes. A view **atual** passa; uma reorganização com essa leitura legítima antes de `pendentes` falharia o selo. A restrição decorre do teste de qualquer `ast.Name` intermediário, inclusive em contexto `Load`. [test_pergunta_no_app.py:57](app/chamados/tests/test_pergunta_no_app.py:57), [test_contract_pergunta_viva.py:54](app/chamados/tests/test_contract_pergunta_viva.py:54), [views.py:2541](app/api/views.py:2541).

3. **Dente e classes anteriores.** Pelo delta fornecido, os mutantes existentes foram preservados e 11 foram acrescentados; a mudança na atribuição reconhecida também ficou mais restritiva. Não identifiquei remoção de um caso existente nem nova derivação paralela ou segundo juiz nesse delta. A comparação independente com o ancestral e a confirmação do hash por Git são **[não verificado]**: a cópia não contém repositório Git. [test_e4c_f6f7_c7_sem_banco.py:87](app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:87), [test_pergunta_no_app.py:51](app/chamados/tests/test_pergunta_no_app.py:51), [test_contract_pergunta_viva.py:48](app/chamados/tests/test_contract_pergunta_viva.py:48).

Executei os cinco testes do módulo sem banco: **5 passaram**, incluindo a bateria dos dois selos. A suíte completa, os testes sob Django e o comportamento em produção permanecem **[não verificado]**. [test_e4c_f6f7_c7_sem_banco.py:29](app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:29), [test_e4c_f6f7_c7_sem_banco.py:87](app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:87).

## 18/11 15:07 · sessao e3ccba44da01 · E4C-F6F7 · construtor · origem=Code

Os dois selos agora rejeitam qualquer ocorrência da variável da consulta entre sua atribuição e a leitura de `['pendentes']`. Eles continuam aceitando a view atual, que lê `consulta['historico']` depois dessa leitura. [test_pergunta_no_app.py:43](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_pergunta_no_app.py:43), [test_contract_pergunta_viva.py:40](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:40), [views.py:2541](/home/ronald/wt-codex-e4c-f6f7-132608/app/api/views.py:2541).

O teste MORDE ganhou 11 mutantes: `del consulta`, atribuição, `+=` e `del` no item, atribuição e `del` em atributo, `update`, `setdefault`, `pop`, `clear` e `setattr`. Cada um é aplicado aos dois selos. [test_e4c_f6f7_c7_sem_banco.py:87](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:87) [nao verificado]

**RED**, antes da cura, para todos os mutantes novos:

```text
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.CosturasSemDjango.test_selos_e_mutantes_MORDEM

Ran 1 test in 0.716s
FAILED (failures=22)
```

As 22 falhas foram `AssertionError: True is not false`: cada selo aceitava os 11 mutantes. [test_e4c_f6f7_c7_sem_banco.py:106](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:106)

**GREEN**, após a cura:

```text
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v chamados.tests.test_e4c_f6f7_c7_sem_banco

Ran 5 tests in 0.875s
OK
```

O teste GREEN verifica a view atual e exige `False` para cada mutante em cada selo; uma execução diagnóstica também mostrou `view=True` e `mutante=False` individualmente para ambos. [test_e4c_f6f7_c7_sem_banco.py:92](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:92) [nao verificado]

LEI-AKITA: padrao=Architecture Conformance Test. [test_e4c_f6f7_c7_sem_banco.py:87](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:87)

Certificação pelo Code e opus final: **[não verificado]**.

## 18/11 15:06 · sessao 105247fcdbcc · E4C-F6F7 · verificador · origem=Code

**PROCEDE.** A sobrescrita simples da variável foi fechada, mas os dois selos ainda aceitam mutantes que quebram a mesma costura. [test_pergunta_no_app.py:46](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_pergunta_no_app.py:46), [test_contract_pergunta_viva.py:43](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_contract_pergunta_viva.py:43).

1. **Bloqueia — os selos ainda passam com a costura quebrada.** Executei os dois helpers extraídos em memória contra a view atual e contra mutantes: ambos aceitaram a view e rejeitaram atribuição simples, atribuição dentro de `if` e `try`, `augassign`, walrus, destinos de `for` e `with` e tupla-destino. Ambos, porém, aceitaram estes dois mutantes inseridos imediatamente após a consulta: [test_pergunta_no_app.py:29](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_pergunta_no_app.py:29), [test_contract_pergunta_viva.py:26](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_contract_pergunta_viva.py:26), [views.py:2541](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/api/views.py:2541).

   ```python
   del consulta
   ```

   ```python
   consulta['pendentes'] = outra_fonte(colab)
   ```

   O primeiro deixa a leitura seguinte sem a variável; o segundo substitui o conteúdo que a leitura usa. A contagem procura apenas `ast.Name` com contexto `Store`: `del consulta` usa `Del`, e a atribuição ao item não grava o nome `consulta`. Assim, a correção cobre o mutante específico da volta 1, mas não fecha o requisito solicitado para `del` nem garante que as pendentes continuem vindas da consulta. [test_pergunta_no_app.py:46](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_pergunta_no_app.py:46), [test_contract_pergunta_viva.py:43](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_contract_pergunta_viva.py:43), [views.py:2542](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/api/views.py:2542).

2. **Achado 2 da volta 1 — esclarecido.** A frase restante no teste é verdadeira: a autoridade só acrescenta uma linha `VALIDACAO` quando `validada_em is not None`. Portanto, para o contador que seleciona apenas essas linhas, excluir previamente perguntas sem `validada_em` não muda o número contado; o mutante apagado não poderia demonstrar uma diferença nesse resultado pela inclusão dessas perguntas. [test_e4c_f6f7_c7_sem_banco.py:64](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:64), [models.py:4265](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/models.py:4265), [backfill_decisoes_pergunta.py:107](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/management/commands/backfill_decisoes_pergunta.py:107).

3. **Dente dos testes e classes anteriores.** No delta fornecido, o único caso removido é o mutante do pré-filtro discutido acima; os mutantes existentes dos selos permanecem, e o mutante que também conta respostas permanece com sua asserção. Não identifiquei no delta uma nova derivação paralela de produção; a lacuna do item 1 é um **selo sem mordida suficiente**, com dois exemplos que passam. A comparação com o ancestral por Git é **[não verificado]**, pois esta cópia não contém repositório Git; a comparação se baseia no delta fornecido. [test_e4c_f6f7_c7_sem_banco.py:53](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:53), [test_e4c_f6f7_c7_sem_banco.py:87](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:87).

Os três testes sem banco do contador e dos selos passaram nesta inspeção, sem escrita de bytecode. A suíte completa e a execução dos casos sob Django permanecem **[não verificado]**. [test_e4c_f6f7_c7_sem_banco.py:53](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:53), [test_e4c_f6f7_c7_sem_banco.py:87](/home/ronald/codex_ws/383f193dc21e3cda78325bef7a25b71fd15cb552/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:87).

## 18/11 15:05 · sessao 90caa6bf5b85 · ESCALA-NAO-DESCREVE · verificador · origem=Code

**AVAL-DEVOLVIDO — ITEM 1.** A cópia contém um “desde” por linha **opcional**, mas ainda não contém a frase agregada do topo descrita no aval; o literal não define como obter o mínimo quando uma pessoa listada está sem data. A condição de parada para uma lista que não carrega o campo por pessoa, em absoluto, **não se aplica**: o campo existe, embora possa ser `None`. [cadastro_realidade.py:960](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:960) [cadastro_realidade.py:1042](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:1042) [não verificado]

1. **Premissas.**

   - **Lista e frase:** `lista()` monta cada `linha` com `carga_desde` e `carga_texto`, inclui as linhas com assinaturas e retorna `dados['linhas']` ordenado por carga. [cadastro_realidade.py:961](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:961) [cadastro_realidade.py:1009](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:1009) [cadastro_realidade.py:1042](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:1042) A linha 785 escreve a frase **de uma pessoa**, sem `X pessoa(s)`; o topo atual do template mostra outros contadores, sem a frase agregada pedida. [cadastro_realidade.py:779](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:779) [cadastro_x_realidade.html:8](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/templates/escala/cadastro_x_realidade.html:8)
   - **Escritor e leitores:** `janela()` calcula o início da janela; `anomalias_da_frota()` chama essa função e escreve `orfas_desde` quando há medida. Nesta cópia, a linha 238 **é** essa escrita; `p = r['proposta']` está na linha 244, contrariando a premissa do anexo para esta cópia. [esmeril_espelho.py:38](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/ponto/services/esmeril_espelho.py:38) [esmeril_espelho.py:233](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/ponto/services/esmeril_espelho.py:233) [esmeril_espelho.py:244](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/ponto/services/esmeril_espelho.py:244) `lista()` lê o mapa e produz o campo e o texto; tela e PDF leem `carga_texto`. [cadastro_realidade.py:949](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:949) [cadastro_x_realidade.html:28](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/templates/escala/cadastro_x_realidade.html:28) [cadastro_realidade.py:1170](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:1170)
   - **L-040:** existe e diz: “LISTA UNICA CADASTRO x REALIDADE: uma so lista de ‘o cadastro diz uma coisa e o ponto mostra outra’, por posto -> vinculo -> assinatura -> acao, com o destino sugerido do propositor; o PDF e a MESMA lista”. A linha atual não cita `END-L3-1800`. [LEIS.md:88](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/docs/LEIS.md:88)
   - **END-L3-1800:** existe nos dois arquivos; registra que a lista é ordenada por carga de órfã, para todos. [CORTES.md:536](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/docs/CORTES.md:536) [CORTES.json:2008](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/docs/CORTES.json:2008)

2. **Colisão com avais:** nos textos fornecidos, a sessão principal construir sem Agent não ocupa o Codex construtor; a exceção ao “depois das 18:00” é nominal; e continuam declarados a precedência da espinha e o teto de um Codex construtor. Não identifico colisão entre essas condições, desde que sejam cumpridas. A execução efetiva e o estado das vagas são [não verificado].

3. **Literal que não fecha:** uma linha pode ter `carga_desde=None`, inclusive para pessoa sem medida; o aval não diz se o mínimo deve ignorar essa pessoa ou se a frase deve ficar incompleta. [cadastro_realidade.py:960](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:960) [test_escala_nao_descreve.py:199](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/tests/test_escala_nao_descreve.py:199) Também se exige incluir o texto da L-040 citando o corte, mas o texto exato a inserir no commit não foi fornecido. [LEIS.md:88](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/docs/LEIS.md:88) [não verificado]

4. **Exportada, dinheiro e `!`:** o escopo declarado é frase e documentação; não autoriza alterar dados de escala ou vínculo, nem gravado de competência exportada, que exigem `!`. [CLAUDE.md:265](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/CLAUDE.md:265) Se uma implementação futura toca dinheiro, exportada ou outro `!` fora desse escopo depende do diff: [não verificado].

**Fecho: PROCEDE** — o mínimo não está definido para linhas sem data, e o texto exato da alteração da L-040 não foi fornecido; a referência à frase agregada também não corresponde ao código atual. [cadastro_realidade.py:960](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/escala/services/cadastro_realidade.py:960) [cadastro_x_realidade.html:8](/home/ronald/codex_ws/a2fc6de40479e933887fa2e89361d61b3ef3d133/app/templates/escala/cadastro_x_realidade.html:8) [não verificado]

## 18/11 15:04 · sessao 976f16fdbf32 · E4C-F6F7 · construtor · origem=Code

Os dois selos agora exigem uma única atribuição à variável da consulta no corpo da view e rejeitam a sobrescrita antes da leitura de `pendentes`. O mutante foi acrescentado aos dois arquivos, preservando os existentes. [test_pergunta_no_app.py:43](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_pergunta_no_app.py:43), [test_pergunta_no_app.py:195](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_pergunta_no_app.py:195), [test_contract_pergunta_viva.py:40](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:40), [test_contract_pergunta_viva.py:116](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_contract_pergunta_viva.py:116).

O teste sem Django ficou **vermelho nos dois selos antes da cura** para `consulta = outra_fonte(colab)` entre a consulta e a leitura. Após a cura, passou também com mutantes de `augassign`, walrus, `for`, `with` e atribuição em bloco aninhado. [test_e4c_f6f7_c7_sem_banco.py:87](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:87). Comando para ambos os selos e seus mutantes: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.CosturasSemDjango.test_selos_e_mutantes_MORDEM
```

O mutante do pré-filtro foi removido: a autoridade só produz `VALIDACAO` com `validada_em` preenchido, então esse filtro não muda o número contado. A razão ficou registrada no teste. [models.py:4265](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/models.py:4265), [test_e4c_f6f7_c7_sem_banco.py:64](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:64). O caso do contador e o mutante existente que também conta respostas passaram com: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c7_sem_banco.ContadorSemBanco.test_autoridade_conta_so_validacao_sem_linha_correspondente chamados.tests.test_e4c_f6f7_c7_sem_banco.ContadorSemBanco.test_mutante_conta_tambem_respostas_MORDE
```

O `py_compile` passou nos três arquivos tocados, com a escrita de bytecode desativada pelo comando abaixo. A suíte e a execução dos casos diretamente sob Django permanecem **[não verificado]**. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import importlib._bootstrap_external
import py_compile
from pathlib import Path

importlib._bootstrap_external._write_atomic = lambda *args, **kwargs: None
for name in ('test_pergunta_no_app.py', 'test_contract_pergunta_viva.py',
             'test_e4c_f6f7_c7_sem_banco.py'):
    path = Path('chamados/tests') / name
    py_compile.compile(str(path), cfile=str(path.parent / (name + '.pyc')), doraise=True)
    print('compilado:', path)
PY
```

LEI-AKITA: padrao=Architecture Conformance Test. [GLOSSARIO.md:15](/home/ronald/wt-codex-e4c-f6f7-132608/app/docs/GLOSSARIO.md:15)

## 18/11 15:02 · sessao daba2766e516 · E4C-F6F7 · verificador · origem=Code

**PROCEDE — há um bloqueio no selo C3 e uma limitação relevante no teste de C4.** O fechamento dos 18 vermelhos e o resultado dos dois testes de folga permanecem **[não verificado]**; a inspeção abaixo é estática, sem acesso a banco ou logs. [nao verificado]

1. **Bloqueia — C3: os mutantes apresentados mordem apenas duas formas de quebrar a costura, e o selo aceita uma fonte sobrescrita.** Os dois novos verificadores procuram uma atribuição de `consulta_do_app(colab, ...)` e, depois, `perguntas = <mesma variável>['pendentes']`, mas não verificam o que acontece com essa variável entre os dois pontos. Assim, inserir `consulta = outra_fonte(colab)` entre as linhas atuais da view preservaria os padrões que os verificadores aceitam, embora `perguntas` passasse a vir de outra fonte. Os mutantes novos substituem a chamada original inteira; não testam essa sobrescrita. Isso também era uma lacuna do selo literal anterior, portanto não demonstra uma regressão funcional introduzida na view; demonstra que a alegação de mordida da costura não cobre esse defeito. [test_contract_pergunta_viva.py:26](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_contract_pergunta_viva.py:26), [test_pergunta_no_app.py:29](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_pergunta_no_app.py:29), [test_pergunta_no_app.py:189](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_pergunta_no_app.py:189), [views.py:2541](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/api/views.py:2541).

   A view atual passa o mesmo `agora` à consulta e à exigência, e a cadeia adicional de `consulta_do_app` até `q_vivas` e `pergunta_viva` continua verificada pelo segundo selo. A substituição da exigência literal por AST aceita a chamada atual, mas sua execução com Django é **[não verificado]**. [views.py:2540](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/api/views.py:2540), [test_contract_pergunta_viva.py:90](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_contract_pergunta_viva.py:90).

2. **Fila — C4: o mutante do filtro cru detecta a grafia pelo defeito da fixture, não por uma diferença no resultado do censo.** A `Query` sintética não implementa `filter`; o mutante é considerado detectado quando provoca `AttributeError`. Com um queryset real, a autoridade já descarta perguntas sem resposta e sem validação, de modo que esse teste não demonstra que remover o pré-filtro altere o número contado. O outro mutante, que passa a contar respostas, encontra dados preparados para distinguir resposta de validação. O selo AST original continua em vigor e identifica `validada_em__isnull` fora da allowlist; sua execução é **[não verificado]**. [test_e4c_f6f7_c7_sem_banco.py:15](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:15), [test_e4c_f6f7_c7_sem_banco.py:47](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:47), [test_e4c_f6f7_c7_sem_banco.py:56](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:56), [models.py:4251](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4251), [test_contract_leitores_vitalidade.py:329](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_contract_leitores_vitalidade.py:329).

3. **Fila — C1 e C2 removem as causas identificadas sem alterar as asserções mostradas no delta; o fechamento integral ainda é [não verificado].** Em C1, `False` corresponde ao default real e mantém o ramo de resposta humana que exige validação; os testes próprios também distinguem `True`, `False` e `True` sem validação anterior. O mutante novo confirma que retirar o atributo volta a causar erro em uma fixture, mas esse erro não prova, sozinho, todas as asserções dos demais casos. [models.py:4195](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4195), [tarefa_do_dia.py:159](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/services/tarefa_do_dia.py:159), [test_f6_t_resposta_mantem_validacao.py:12](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_f6_t_resposta_mantem_validacao.py:12), [test_e4c_f6f7_c7_sem_banco.py:74](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:74).

   Em C2, a fixture injeta a função extraída no namespace e conserva as asserções sobre âncoras e relações; o mutante que omite a extração espera o `NameError` original. O resultado posterior dos testes e da matriz inteira é **[não verificado]**. [test_e3_r3_c7_sem_banco.py:190](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_e3_r3_c7_sem_banco.py:190), [test_e3_r3_c7_sem_banco.py:209](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_e3_r3_c7_sem_banco.py:209), [test_e4c_f6f7_c7_sem_banco.py:82](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:82).

4. **C4 — autoridade, campos e significado.** O contador chama `linhas_da_projecao_sem_linha` e conta apenas os dicionários de ato `VALIDACAO` devolvidos. Ele mede **validações projetadas sem linha correspondente**, não a quantidade de perguntas atualmente validadas. A leitura de validação vigente é `DecisaoPergunta.leitura`, com face SQL `q_validada_pelo_ato`; ambas consideram atos posteriores que podem limpar a validação. Portanto, o contador delega a correspondência ao produtor do passivo, sem criar outro juiz de validação vigente no trecho alterado. [backfill_decisoes_pergunta.py:94](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/management/commands/backfill_decisoes_pergunta.py:94), [models.py:4238](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4238), [models.py:4295](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4295), [models.py:3539](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:3539).

   | Campo ou valor | Quem escreve / produz | Quem lê no caminho de C4 |
   |---|---|---|
   | `pk` / `pergunta_id` | Identidade da pergunta; a autoridade produz `pergunta_id` nas linhas propostas. [models.py:4262](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4262) | A autoridade usa o ID para buscar e comparar linhas por pergunta. [models.py:4254](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4254) |
   | `respondida_em`, `resposta_colab` | A porta humana grava resposta e instante; a automática e a trilha também têm caminhos declarados. [disputa_emissao.py:1480](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/services/disputa_emissao.py:1480), [disputa_emissao.py:1681](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/services/disputa_emissao.py:1681), [disputa_emissao.py:1692](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/services/disputa_emissao.py:1692) | A autoridade usa o instante para correspondência e o texto para compor a linha proposta; o contador descarta linhas de resposta. [models.py:4260](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4260), [backfill_decisoes_pergunta.py:107](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/management/commands/backfill_decisoes_pergunta.py:107) |
   | `validada_em`, `validada_por_id` | `_carimbar_validacao` grava instante e autor junto à decisão; há outros caminhos de projeção, portanto a lista completa de escritores é **[não verificado]**. [validacao.py:161](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/services/validacao.py:161), [models.py:3331](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:3331) | A autoridade compara o instante com a linha existente e coloca o autor na linha proposta. [models.py:4265](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4265) |
   | `ato`, `ato_em`, linhas existentes | As portas entregam linhas a `DecisaoPergunta.registrar`; a validação registra o mesmo instante do carimbo. [models.py:4328](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4328), [validacao.py:170](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/services/validacao.py:170) | A autoridade compara `(pergunta_id, ato, ato_em)`; o contador seleciona o `ato` produzido. [models.py:4254](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/models.py:4254), [backfill_decisoes_pergunta.py:107](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/management/commands/backfill_decisoes_pergunta.py:107) |

   A remoção do filtro amplia a varredura para todas as perguntas; custo e volume em produção são **[não verificado]**. [backfill_decisoes_pergunta.py:102](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/management/commands/backfill_decisoes_pergunta.py:102).

5. **Alcance e classes de reprovação.** No delta fornecido, a mudança de produção é de leitura no contador; não identifiquei nela novo escritor, relógio, trava, mudança de ordem de pouso ou campo com segundo significado. O resultado dessa inspeção não prova ausência dessas classes no restante da obra. [backfill_decisoes_pergunta.py:102](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/management/commands/backfill_decisoes_pergunta.py:102).

   O **comando que contém** o contador consulta `folha.export.competencia_entregue`; seu `aplicar` também é chamado pelo comando de deduplicação. Ele está declarado em `FORA_DE_PIPELINE`, como ato de uma vez, e seu DRY chama a porta dentro de transação revertida. Isso é alcance do módulo/comando, não evidência de que o contador alterado seja chamado por esses caminhos: na busca da cópia, seus usos encontrados foram os próprios testes. Execução efetiva por cron externo, impacto em retenção ou `DiaPago`: **[não verificado]**. [backfill_decisoes_pergunta.py:478](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/management/commands/backfill_decisoes_pergunta.py:478), [backfill_decisoes_pergunta.py:604](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/management/commands/backfill_decisoes_pergunta.py:604), [dedup_perguntas_fato.py:100](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/management/commands/dedup_perguntas_fato.py:100), [crons.py:809](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/config/crons.py:809), [test_f7_contador_validada_sem_linha.py:28](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/chamados/tests/test_f7_contador_validada_sem_linha.py:28).

6. **G5 — a causa específica do teto da competência deixou de ser demonstrável nesta árvore; verde dos dois testes é [não verificado].** A seleção agora alcança células geradas existentes com `data > hoje`, sem teto de competência, e a relavra passa por verificações de vínculo e pela porta da célula. Os dois testes continuam escolhendo `hoje + 3`; os caminhos manual e de calendário chegam à regeneração futura. Isso sustenta que a data além do corte, por si só, já não exclui a célula, mas não prova que todas as demais condições e asserções dos testes passem. [cadastro_futuro.py:8](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/ponto/services/cadastro_futuro.py:8), [cadastro_futuro.py:43](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/ponto/services/cadastro_futuro.py:43), [folgas.py:289](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/escala/folgas.py:289), [folgas.py:605](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/escala/folgas.py:605), [test_torneira_folgas.py:68](/home/ronald/codex_ws/b63dd29f72fa3ea8a66eebbe986584fee332d986/app/escala/tests/test_torneira_folgas.py:68).

**Fecho: PROCEDE.** C3 precisa de mordida para a sobrescrita da fonte; o fechamento da suíte e os resultados dependentes de banco seguem **[não verificado]**. [nao verificado]

## 18/11 14:59 · sessao 7f5ddcf1c760 · ESPINHA-NAO-ESPERA · verificador · origem=Code

**AVAL-DEVOLVIDO — item 2: PROCEDE.** O brief contém uma premissa expressamente marcada `[nao verificado]`, e o nome e a assinatura da face SQL que ele manda ler não existem no literal pedido. O próprio item 2 determina a devolução nessas condições. [não verificado — anexo `espinha_nao_espera_1811_1455.txt`, item 2, fornecido sem numeração de linhas; [models.py:3538](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3538)]

Os anexos foram fornecidos no texto da pergunta, sem linhas numeradas. Suas referências abaixo identificam o anexo e a seção, acompanhadas de **[não verificado]**; os links com linha apontam para arquivos efetivamente lidos nas cópias.

### Item 1 — E4C-F6F7 c7

1. **Premissas:** conferem no registro disponível. A c7 foi encaminhada ao construtor na cópia indicada; o RELATO registra depois a entrega do construtor e o verificador à espera. Isso não prova o estado atual de processos. ([RELATO.md:14](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:14), [RELATO.md:8](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:8)).

2. **Colisão com aval vigente:** a liberação antecipada do Codex é a substituição expressa da frase do complemento; a RAIA continua estabelecendo F9 antes de F6F7 na ordem posterior às 18h. A antecipação do verificador da c7 antes das 18h, por si só, não exige abrir outra vaga de construtor. Se ainda houver disputa pela vaga após as 18h, permanece a ordem da RAIA e o teto de um Codex construtor. [não verificado — anexos `espinha_nao_espera_1811_1455.txt`, cabeçalho e item 1; `complemento1_pousos_comboio_1811_1336.txt`, item 1c; `raia_espurias_1811_1410.txt`, item 1; [BACKLOG.md:2345](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/BACKLOG.md:2345); [CLAUDE.md:353](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/CLAUDE.md:353)]

3. **Condições no literal:** a sequência verificador → cura e nova verificação quando procedente → suite inteira verde → opus final é compatível com o pré-verificador; o aval mantém o opus para depois das 18h. O RELATO registra `merge-tree` rc 0 com a c9, mas isso não prova GREEN da c9 nem certifica a árvore do eventual comboio. [não verificado — anexo `espinha_nao_espera_1811_1455.txt`, item 1; [CLAUDE.md:372](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/CLAUDE.md:372); [RELATO.md:5](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:5)]

4. **Exportada, dinheiro ou `!`:** o *diff anexado da c7* lista um comando e oito arquivos de teste, abaixo; não lista arquivo de folha, export, retenção ou DiaPago. Isso **não comprova o alcance completo da obra F6F7 nem seus efeitos indiretos**, pois o anexo é o diff da c7, não uma prova apresentada do diff integral do pouso. A condição da janela deve ser conferida sobre a árvore e o diff final antes do ato. [não verificado — anexo `f6f7_c7_wt.diff` e anexo `espinha_nao_espera_1811_1455.txt`, item 4; exemplo da alteração em [backfill_decisoes_pergunta.py:94](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/management/commands/backfill_decisoes_pergunta.py:94)] [nao verificado]

Arquivos do diff c7, todos sob `app/chamados/`:

- `management/commands/backfill_decisoes_pergunta.py`;
- `tests/test_contract_pergunta_viva.py`;
- `tests/test_e3_c8b_sem_banco.py`;
- `tests/test_e3_portas_tarefa.py`;
- `tests/test_e3_pos_suite_sem_banco.py`;
- `tests/test_e3_r3_c7_sem_banco.py`;
- `tests/test_e3_tarefa_admin_sem_banco.py`;
- `tests/test_pergunta_no_app.py`;
- `tests/test_e4c_f6f7_c7_sem_banco.py`, novo.

Inventário transcrito do anexo `f6f7_c7_wt.diff`, sem numeração de linhas: **[não verificado]**; os arquivos e exemplos das alterações estão presentes na cópia, como em [test_e4c_f6f7_c7_sem_banco.py:29](/home/ronald/wt-codex-e4c-f6f7-132608/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:29).

**Fecho item 1: CONFERE** — o registro sustenta a c7 entregue e aguardando verificador; a elegibilidade do pouso e o GREEN da c9 permanecem por verificar. ([RELATO.md:8](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:8); [não verificado — anexo do aval, itens 1 e 4])

### Item 2 — E4C-F9

1. **Premissas:** a P2 está registrada como pousada em `8ae23ad62`. O literal `PerguntaDisputa._q_validada_pelo_ato()` como `staticmethod` privado **não confere**: existe `PerguntaDisputa.q_validada_pelo_ato(cls)`, público e `@classmethod`. A leitura por ato existe como `DecisaoPergunta.leitura(cls, p)`, também `@classmethod`, com conservação do passivo descrita no próprio método. O brief fornecido também contém a ressalva literal de que os casos com ato não foram separados por último ato, marcada `[nao verificado]`; o RELATO registra censo F9 fechado, mas isso não elimina essa ressalva textual. ([RELATO.md:4](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:4), [models.py:3538](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3538), [models.py:4294](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:4294), [RELATO.md:89](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/docs/RELATO.md:89); [não verificado — anexo `brief.md`, seções 2a e 3])

2. **Colisão com aval vigente:** a abertura antecipada do Codex pretende substituir a espera do complemento, mas mantém a vaga única do wrapper e a ordem da RAIA; a cláusula “quando a F6F7 não estiver usando o construtor” respeita o teto de um. Disponibilidade efetiva da vaga **[não verificado]**. [não verificado — anexos `espinha_nao_espera_1811_1455.txt`, item 2; `complemento1_pousos_comboio_1811_1336.txt`, item 1c; `raia_espurias_1811_1410.txt`, item 1; [CLAUDE.md:353](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/CLAUDE.md:353)]

3. **Condições no literal:** **não fecham duas condições expressas do próprio item 2**: o sítio nomeado pelo brief não existe com aquele nome e assinatura, e o brief contém uma premissa `[nao verificado]`. A existência de uma face pública funcionalmente relacionada não autoriza substituir silenciosamente o literal exigido. O brief ainda declara dependência de RELAVRA-11; seu pouso **[não verificado]** nos dados disponíveis. [não verificado — anexos `espinha_nao_espera_1811_1455.txt`, item 2, e `brief.md`, seções 2a, 3 e 5; [models.py:3538](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3538)]

4. **Exportada, dinheiro ou `!`:** o brief declara F9 como mudança de leitura/seleção e declara que nenhum comando com `--apply` será executado no pouso. Isso não é um diff construído nem prova de ausência de efeito indireto sobre folha, export, retenção ou DiaPago; a elegibilidade do pouso permanece **[não verificado]**. [não verificado — anexo `brief.md`, seções 1, 5 e 7; anexo `espinha_nao_espera_1811_1455.txt`, item 4] [nao verificado]

Os **12 comandos identificados pelo brief** são os seguintes; a lista é de escopo proposto, não de arquivos comprovadamente alterados:

- `auditar_invariantes_chamados.py`, `backfill_decisoes_pergunta.py`, `reabrir_orfas_enterradas.py`, `curar_perguntas_em_limbo.py`;
- `auditar_disputas.py`, `encerrar_perguntas_com_fato.py`, `dedup_perguntas_fato.py`, `migrar_data_evento_dia_do_turno.py`;
- `validar_classe_a.py`, `relavrar_via_i6.py`, `reconciliar_fantasmas.py`, `reconciliar_grade.py`.

O brief também cita, como dependências, arquivos comuns ou possíveis pontos de teste: `chamados/juizes.py`, `services/atos_pergunta.py`, `chamados/models.py`, `config/crons.py`, `app/core/juizes.py`, `app/chamados/tests/test_reabrir_orfas_enterradas.py`, `test_e4_backfill.py`, `test_e4_passivo.py` e `test_n1_perguntas_do_app.py`. [não verificado — anexo `brief.md`, seções 1, 2b e 5]

**Fecho item 2: PROCEDE** — devolver a abertura da F9: falham as duas condições literais acima, independentemente do registro de pouso da P2. ([models.py:3538](/home/ronald/codex_ws/d6da9850935f039f0c5e45b1d4d8a6974d6c35d6/app/chamados/models.py:3538); [não verificado — anexos `brief.md`, seção 3, e aval, item 2])

## 18/11 14:48 · sessao 2ff881a349be · PRE-MAPA-E4D · leitura · origem=Code

A matriz abaixo é uma **leitura parcial**: onde não confirmei o juiz registrado, o ato ou um teste que exerça a porta, marquei `[nv]`. A numeração das decisões do item 2 não consta no dado fornecido; por isso, “decisão N” identifica uma dependência ainda sem número comprovado, sem escolher o destino do campo. O anexo situa A03, A05, A06, A17, A18 e A20 como exceções FATO posteriores à E4d; a abrangência exata de cada exceção por campo permanece `[nv]`. [app/docs/RELATO.md:2233](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/RELATO.md:2233)

As contagens na última coluna são **as do anexo `leitores_tabela.txt` fornecido na mensagem**, sem arquivo:linha local comprovado `[nv]`. São leitores **detectados**, não uma prova de completude; o próprio dado informa 67 `getattr` dinâmicos não atribuídos e possíveis correspondências de campo em outro modelo `[nv]`. Não somei contagens entre campos, pois um mesmo sítio pode ler mais de um campo `[nv]`.

Abreviações na tabela: `M` = `materializada_em`, `materializada_por`, `via_resolucao`; `V` = `validada_em`, `validada_por`; `R` = `respondida_em`, `resposta_colab`; `L` = `veredito_lavrado`, `julgado_em`, `janela_expira_em`; `F` = `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`. Os números de leitores são apresentados na mesma ordem dos campos dessas abreviações.

| Porta (função e fonte) | Estado de entrada | Juiz que decide | Operação | Grava hoje: ato + projeção | Depois da E4d | Retorno | Prova existente | Leitores NÃO cobertos dos campos gravados |
|---|---|---|---|---|---|---|---|---|
| A01 [`_gravar_estado`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:207) | Estado anterior e novo; terminal exige via válida e não admite pergunta viva ancorada. | `chamados.juizes::pergunta_viva` pela guarda; outros juízes `[nv]`. [models.py:228](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:228) | Transição do chamado. | Ato: evento `estado_chamado`; projeção: `status_local`, `resolvido_em`, `via_encerramento`, condicionalmente `respondido_por`; também urgência e relavra delegada. [models.py:234](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:234), [models.py:298](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:298) | Ato indicado; destino de cada campo e da relavra: decisão N `[nv]`. | `None` `[nv]`. | [`test_hx_b7_trilha.py::test_gravar_estado_emite`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_hx_b7_trilha.py:27) | `status_local` 454; `resolvido_em` 27; `via_encerramento` 19; `respondido_por` 8. |
| A02 [`fechar`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2017) | Disputa aberta, usuário; não pode haver resposta sem veredito. | `chamados.juizes::pergunta_viva` para perguntas vivas. [models.py:2040](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2040) | Fecha disputa; em exceções, encerra perguntas mudas. | Ato: evento `disputa_fechada` e, condicionalmente, `SILENCIO` por pergunta; projeção: `F` e carimbo delegado. [models.py:2082](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2082) | Atos indicados; `F` e campos delegados → decisão N `[nv]`. | `True`; `False` na repetição idêntica; entrada diferente pode levantar `ValueError`. [models.py:2045](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2045) | [`test_disputa_transicoes.py::test_01_fechar_grava_tres_campos`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_disputa_transicoes.py:60) | `F`: 190, 2, 24, 17; carimbo delegado: `M` 220, 1, 149. |
| A03 [`reabrir_admin`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2289) | Fechada, usuário presente e menos de duas reaberturas administrativas. | `[nv]` | Reabre administrativamente. | Ato próprio `[nv]`; limpa `F`, incrementa `reaberturas_admin`, altera `motivos` se fornecido. [models.py:2312](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2312) | FATO declarado; ato e destino de `F`, contador e `motivos` → decisão N `[nv]`. | `None`; condições inválidas levantam `ValueError`. [models.py:2301](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2301) | [`test_disputa_transicoes.py::test_01_reabrir_zera_fechada_em_e_incrementa`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_disputa_transicoes.py:131) | `F`: 190, 2, 24, 17; `reaberturas_admin` 6; `motivos` 26. |
| A04 [`reabrir_sistema`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2326) | Disputa fechada; aberta é no-op. | `[nv]` | Reabre pelo sistema. | Ato: ocorrência `disputa_reaberta_sistema`, se houver chamado; limpa `F`. [models.py:2336](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2336) | Ocorrência indicada; `F` → decisão N `[nv]`. | `True` se reabriu; `False` se já aberta. | `[nv]` | `F`: 190, 2, 24, 17. |
| A05 [`carimbar`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3299) | Via não identidade, pergunta dentro do fato; resposta opcional válida. | `chamados.juizes::pergunta_viva` e `texto_vazio`. [models.py:3313](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3313) | Carimba se a pergunta deixa de estar viva. | Ato próprio de decisão `[nv]`; `M`, condicionalmente `respondida_em`, `V` e `L`. [models.py:3324](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3324), [models.py:3345](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3345) | FATO declarado; ato e destino de `M`, `respondida_em`, `V`, `L` → decisão N `[nv]`. | `True` se gravou; `False` se recusou; pode levantar exceção. | `[nv]` | `M`: 220, 1, 149; `respondida_em` 68; `V`: 170, 7; `L`: 13, 5, 5. |
| A06 [`carimbar_identidade`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3364) | Via de identidade; fora do fato restringe mudança de via. | O código atribui o julgamento de identidade ao chamador; juiz registrado específico `[nv]`. [models.py:3365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3365) | Carimbo de identidade. | Ato próprio de decisão `[nv]`; `M` e, se fornecido, `recusa_motivo`. [models.py:3378](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3378) | FATO declarado; ato e destino de `M` e `recusa_motivo` → decisão N `[nv]`. | `True`/`False`; via inválida levanta `ValueError`. | [`test_e1_volta2.py::test_RED_retratada_nao_esta_no_dominio_do_lote`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_volta2.py:128) | `M`: 220, 1, 149; `recusa_motivo` 20. |
| A07 [`descarimbar`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3399) | Não estar fora do fato. | `[nv]` | Limpa carimbo; pode só preparar em memória. | Ato próprio `[nv]`; limpa `M`, persistindo se `salvar=True`. | Ato `[nv]`; `M` → decisão N `[nv]`. | Lista de campos ou `[]` se recusado. | [`test_e4c_f2_leitura_do_ato.py::test_MORDE_descarimbo_volta_com_resposta_so_na_projecao`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:403) | `M`: 220, 1, 149. |
| A08 [`descarimbar_em_lote`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3448) | Lote com colaboradores travados; exclui fora do fato e verifica par de resposta quando fornecido. | `[nv]` | Limpeza em lote com `**outros`. | Ato próprio `[nv]`; limpa `M` e grava os campos passados em `**outros`, incluindo `R`/`V` nos chamadores comprovados. [models.py:3485](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3485), [models.py:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1702) | Ato `[nv]`; `M` e cada campo efetivo de `**outros` → decisão N `[nv]`. | Número de linhas atualizadas. | [`test_e3_10_cura_no_escritor.py`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e3_10_cura_no_escritor.py:207)::teste `[nv]`. | `M`: 220, 1, 149; `R`: 68, 135; `V`: 170, 7; demais dinâmicos `[nv]`. |
| A09 [`encerrar_por_fato_no_espelho`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3489) | Não estar fora do fato nem já na via `fato_materializado`. | `chamados.juizes::fato_do_marco_no_espelho`, chamado pelo chamador. [models.py:3491](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3491) | Troca a via. | Ato próprio `[nv]`; `via_resolucao`. | Ato `[nv]`; `via_resolucao` → decisão N `[nv]`. | `True`/`False`. | [`test_porta_chamado_e3.py::test_encerrar_por_fato_no_espelho_2x`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_porta_chamado_e3.py:70) | `via_resolucao` 149. |
| A10 [`_carimbar_validacao`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:161) | Pergunta e autor; condições de mérito vêm dos chamadores. | `[nv]` | Valida. | `DecisaoPergunta.VALIDACAO`, mais linhas legadas faltantes; `V`. [validacao.py:167](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:167) | Ato indicado; `V` → decisão N `[nv]`. | `None`. | [`test_e1_entradas.py::test_MORDE_validacao_e_linha_com_o_relogio_do_carimbo`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_entradas.py:94) | `V`: 170, 7. |
| A11 [`devolver_validacao_recusada`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:237) | Resposta de hora, não materializada, com motivo de recusa; validação pode já estar vazia. | `chamados.juizes::declarou_hora`, pela função auxiliar. [validacao.py:185](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:185) | Devolve validação; pode reabrir disputa. | `DEVOLUCAO` se havia validação e evento `validacao_devolvida` se houve mudança; limpa `V`, com possível A04 delegada. [validacao.py:269](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:269) | Atos indicados; `V` e campos delegados → decisão N `[nv]`. | `False`, `None` ou `True`, conforme guarda e mudança. | [`test_e1_volta2.py::test_MORDE_devolucao_guarda_a_validacao_legada`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_volta2.py:101) | `V`: 170, 7; se A04, `F`: 190, 2, 24, 17. |
| A12 [`responder_pergunta_disputa_core`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1439) | Resposta válida para o tipo; recebe indicador `ja_respondida`. | `[nv]` | Resposta humana e eventual descarimbo. | `RESPOSTA` e linhas legadas faltantes; `R`, possível limpeza de `V` e `M`, e `recusa_motivo`. [disputa_emissao.py:1481](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1481), [disputa_emissao.py:1532](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1532) | Ato indicado; `R`, `V`, `M` e `recusa_motivo` → decisão N `[nv]`. | `(True, None)` ou `(False, mensagem)`. | [`test_e4c_f2_leitura_do_ato.py::test_resposta_nova_depois_da_validacao_volta_ao_admin`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:239) | `R`: 68, 135; `V`: 170, 7; `M`: 220, 1, 149; `recusa_motivo` 20. |
| A13 [`reabrir_para_correcao`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1584) | Queryset de perguntas corrigíveis; ato deve aceitar foto. | `chamados.juizes::perguntas_corrigiveis` seleciona nos chamadores que o usam; não é chamado pela função em todos os casos comprovados `[nv]`. [disputa_emissao.py:1604](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1604) | Reabre em lote. | `REABERTURA` por padrão, ou ato recebido, para pergunta da qual algo é retirado; limpa `R`, `V`, `M` por A08. [disputa_emissao.py:1639](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1639) | Ato indicado; `R`, `V`, `M` → decisão N `[nv]`. | Número de linhas atualizadas. | [`test_e4c_f2_leitura_do_ato.py::test_resposta_validacao_devolucao_reabertura`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:215) | `R`: 68, 135; `V`: 170, 7; `M`: 220, 1, 149. |
| A14 [`responder_automaticamente`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1662) | Sem texto anterior; texto e instante devem formar par válido. | `[nv]` | Registra resposta automática. | `RESPOSTA` e linhas legadas faltantes; `R`. [disputa_emissao.py:1681](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1681) | Ato indicado; `R` → decisão N `[nv]`. | `True` se escreveu; `False` se já havia texto. | [`test_porta_chamado_e3.py::test_responder_automaticamente_2x`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_porta_chamado_e3.py:43) | `R`: 68, 135. |
| A15 [`carimbar_resposta_pela_trilha`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1692) | Texto presente e `respondida_em` vazio; instante fornecido pela trilha. | `[nv]` | Recupera instante da resposta. | `RESPOSTA` e evento `carimbar_resposta_pela_trilha`; `respondida_em`. [disputa_emissao.py:1705](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1705) | Ato indicado; `respondida_em` → decisão N `[nv]`. | `True`/`False`. | [`test_e4c_f2_leitura_do_ato.py::test_resposta_pela_trilha_e_validacao`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e4c_f2_leitura_do_ato.py:295) | `respondida_em` 68. |
| A16 [`relavrar`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:106) | Via derivada e nova via diferente e não nula. | `chamados.juizes::via_pelo_fato` no ramo correspondente de `via_de_hoje`; outros ramos `[nv]`. [relavra_via.py:95](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:95) | Relavra via e veredito. | Evento de relavra por pergunta alterada; `via_resolucao` e `L`. [relavra_via.py:125](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:125) | Evento indicado; `via_resolucao` e `L` → decisão N `[nv]`. | Tupla `(n_vias, n_vereditos)`. | `[nv]` | `via_resolucao` 149; `L`: 13, 5, 5. |
| A17 [`_materializar_resposta_hora`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1310) | Ramo de hora; exige anuência e cálculo de plantio. | `[nv]` | Materializa ou registra pendência. | Ato de pendência `[nv]`; condicionalmente `pendencia_admin`, `recusa_motivo`; demais efeitos e atos do plantio `[nv]`. [materializacao.py:1355](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1355), [materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365) | FATO declarado; ato e destino dos dois campos → decisão N `[nv]`; demais efeitos `[nv]`. | `True` ou `None`, conforme ramo. | `[nv]` | `pendencia_admin` 5; `recusa_motivo` 20; delegados `[nv]`. |
| A18 [`_carimbar_recusa`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1683) | Motivo de recusa; colisão e ausência de carimbo determinam o ramo. | `[nv]` | Registra recusa; colisão delega carimbo de identidade. | Ocorrência `recusa_materializacao` quando aplicável; `recusa_motivo` e, na colisão, `M` por A06. [materializacao.py:1693](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1693) | FATO declarado; ocorrência indicada; `recusa_motivo` e `M` delegado → decisão N `[nv]`. | `None`. | `[nv]` | `recusa_motivo` 20; `M`: 220, 1, 149. |
| A19 [`recusar_pergunta`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:858) | Motivo, pergunta encontrada, respondida, não validada e dentro do fato. | `chamados.juizes::pergunta_respondida`. [validacao.py:901](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:901) | Recusa e devolve ao colaborador. | `RECUSA` via A13, ocorrência e auditoria; `recusa_motivo` e limpeza delegada de `R`, `V`, `M`. [validacao.py:916](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:916) | Atos indicados; campos citados → decisão N `[nv]`. | Dicionário `ok`, `estado`, `msg`. | [`test_e1_entradas.py::test_MORDE_recusa_e_linha_com_quem_e_o_texto_que_tirou`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_entradas.py:103) | `recusa_motivo` 20; `R`: 68, 135; `V`: 170, 7; `M`: 220, 1, 149. |
| A20 [`conferir_data_do_fato`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:957) | Pergunta encontrada, justificativa, data divergente e ainda não conferida. | Juiz registrado de `data_do_fato_de` `[nv]`. [validacao.py:990](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:990) | Confere data. | `LogAuditoria` `conferir_data`; `data_conferida_em`, `data_conferida_por`. [validacao.py:996](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:996) | FATO declarado; ato indicado; os dois campos → decisão N `[nv]`. | Dicionário `ok`, `estado`, `msg`. | [`test_data_do_fato_divergente.py::test_GOLDEN_conferir_carimba_e_deixa_trilha`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_data_do_fato_divergente.py:185) | `data_conferida_em` 5; `data_conferida_por` 1. |
| A21 [`remover_motivo`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2134) | Disputa aberta, motivo presente e ator; fato encerrado é julgado pelo evento chamador. | `[nv]` | Remove motivo e retrata perguntas correspondentes. | `RETRATACAO` por pergunta elegível; `motivos`, `contexto`, `M` delegado e possível fechamento A02. [models.py:2217](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2217) | Ato indicado; `motivos`, `M` e eventual `F` → decisão N `[nv]`; classificação de `contexto` `[nv]`. | Dicionário com `ok`, `estado`, `motivos`. [models.py:2229](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2229) | [`test_e1_entradas.py::test_MORDE_a_retratada_morre_e_tem_linha`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/tests/test_e1_entradas.py:142) | `motivos` 26; `M`: 220, 1, 149; possível `F`: 190, 2, 24, 17. |
| A22 [`_nascer`](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3012) | Disputa, motivo, data e identidade válidos; aceita `**campos`. | `chamados.juizes::resolvedora_valida` em ramo de destino; demais julgamentos `[nv]`. [mod

[truncado em 20000 de 24659 caracteres]

## 18/11 14:47 · sessao baf79de3bdbc · PRE-MAPA-E4D · leitura · origem=Code

As etiquetas abaixo distinguem **LEI**, quando falta corte de Ronald, de **DESENHO**, quando o desenho ou corte escrito enquadra a pergunta. A escolha de implementação permanece formulada como pergunta em todos os itens. O censo de escritores admite escritas dinâmicas não resolvidas; sua completude é **[nao verificado]**. O resumo de leitores é um tamanho detectado, sujeito aos limites informados no anexo; cobertura real de cada leitor é **[nao verificado]**.

1. **DESENHO — `status_local`:** na A01, parar significa deixar de gravar, derivar de qual ato ou retirar o campo por migration? Como preservar seus leitores e o nascimento? Fonte: [models.py:234](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:234), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

2. **DESENHO — `resolvido_em`:** na A01, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration? Fonte: [models.py:235](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:235), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

3. **DESENHO — `via_encerramento`:** na A01, deixa de ser gravada, deriva de qual ato ou sai do modelo por migration? Fonte: [models.py:234](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:234), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

4. **DESENHO — `respondido_por`:** na A01, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration? Fonte: [models.py:237](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:237), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

5. **DESENHO — `fechada_em`:** nas A02–A04, deixa de ser gravada, deriva de qual ato ou sai do modelo por migration, considerando a exceção A03? Fonte: [models.py:2095](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2095), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

6. **DESENHO — `fechada_por`:** nas A02–A04, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration, considerando A03? Fonte: [models.py:2096](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2096), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

7. **DESENHO — `resolucao`:** nas A02–A04 e A24, deixa de ser gravada, deriva de qual ato ou sai do modelo por migration, considerando A03? Fonte: [models.py:2097](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2097), [models.py:2428](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2428), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

8. **DESENHO — `motivo_fechamento`:** nas A02–A04, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration, considerando A03? Fonte: [models.py:2098](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2098), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

9. **DESENHO — `reaberturas_admin`:** na A03, qual é o tratamento do contador até o fato próprio e depois dele: gravação, derivação de qual ato ou retirada por migration? Fonte: [models.py:2316](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2316), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

10. **DESENHO — `reaberturas_colab`:** na A23, deixa de ser gravado, deriva de qual ato ou sai do modelo por migration? Fonte: [models.py:2402](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2402), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

11. **DESENHO — `motivos`:** na A03 e nos demais sítios, é projeção a parar, dado derivado de qual ato ou campo que permanece no modelo? Como incide a exceção A03? Fonte: [models.py:2319](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2319), [disputa_emissao.py:550](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:550), [BACKLOG.md:2091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2091).

12. **DESENHO — `materializada_em`:** nas portas que carimbam ou descarimbam, como passar da escrita dupla ao ato de conclusão e tratar as exceções A05/A06, inclusive os leitores ainda detectados? Fonte: [models.py:3324](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3324), [BACKLOG.md:2015](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2015), [BACKLOG.md:2092](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2092).

13. **DESENHO — `materializada_por`:** deixa de ser gravado, deriva de qual ato e autor, ou sai por migration, respeitadas A05/A06? Fonte: [models.py:3324](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3324), [BACKLOG.md:2015](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2015), [BACKLOG.md:2093](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2093).

14. **DESENHO — `via_resolucao`:** deixa de ser gravada, deriva de qual ato ou sai por migration, distinguindo vias de decisão e vias derivadas e as exceções A05/A06? Fonte: [models.py:3378](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3378), [relavrar_via_i6.py:58](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/relavrar_via_i6.py:58), [BACKLOG.md:2092](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2092).

15. **DESENHO — `respondida_em`:** deixa de ser gravado, deriva de qual ato de resposta e de seus atos posteriores, ou sai por migration? Fonte: [disputa_emissao.py:1481](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1481), [models.py:4262](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4262), [PLANO-PADROES.md:44](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/PLANO-PADROES.md:44).

16. **DESENHO — `resposta_colab`:** deixa de ser gravada, deriva de qual ato e texto, ou sai por migration, preservando os leitores e a exceção A05? Fonte: [disputa_emissao.py:1481](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1481), [CORTES.md:521](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:521), [BACKLOG.md:2092](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2092).

17. **DESENHO — `validada_em`:** deixa de ser gravado, deriva de qual ato de validação e de seus atos posteriores, ou sai por migration? Fonte: [validacao.py:168](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:168), [models.py:4267](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4267), [CORTES.md:543](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:543).

18. **DESENHO — `validada_por`:** deixa de ser gravado, deriva de qual ato e autor, ou sai por migration? Fonte: [validacao.py:168](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:168), [models.py:4267](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:4267), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

19. **DESENHO — `recusa_motivo`:** quais escritas deixam de gravar, qual ato fornece o motivo e como ficam as exceções A06/A17/A18 até seus fatos próprios? Fonte: [validacao.py:916](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:916), [BACKLOG.md:2093](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2093), [BACKLOG.md:2094](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2094).

20. **DESENHO — `pendencia_admin`:** como permanece a fonte lida de A17 até o fato próprio e qual derivação ou migration fica para depois? Fonte: [materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365), [BACKLOG.md:2094](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2094).

21. **DESENHO — `data_conferida_em`:** como permanece a fonte de A20 até o fato próprio e de qual ato e par de dias será derivado o instante depois? Fonte: [validacao.py:996](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:996), [BACKLOG.md:2096](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2096).

22. **DESENHO — `data_conferida_por`:** como permanece a fonte de A20 até o fato próprio e de qual ato será derivado o autor depois? Fonte: [validacao.py:997](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:997), [BACKLOG.md:2096](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2096).

23. **DESENHO — `veredito_lavrado`:** o cache deixa de ser gravado, continua derivado de qual fonte, ou sai por migration; como seus leitores passam pelo Contract? Fonte: [veredito_lavrado.py:270](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:270), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

24. **DESENHO — `julgado_em`:** o instante do cache deixa de ser gravado, deriva de qual fonte ou sai por migration? Fonte: [veredito_lavrado.py:281](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:281), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

25. **DESENHO — `janela_expira_em`:** o prazo do cache deixa de ser gravado, deriva de qual fonte ou sai por migration? Fonte: [veredito_lavrado.py:228](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:228), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

26. **DESENHO — competência exportada:** que tratamento recebe cada gravado alcançado pelo Contract e pelo backfill do ato de conclusão, distinguindo projeção, gravado de cálculo e TXT entregue, à luz da L-092 como alterada pela L-113 e do `!` declarado para backfill que alcance exportada? O estoque efetivamente atingido é **[nao verificado]**. Fonte: [LEIS.md:125](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/LEIS.md:125), [LEIS.md:155](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/LEIS.md:155), [BACKLOG.md:2015](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2015).

27. **DESENHO — CLASSE-D:** quais entregas da cura pela porta, da lista do admin e da leitura de empresas pelo cadastro precisam pousar antes da E4d, e como comprovar o cruzamento com B2 + delta e F2? O pouso atual é **[nao verificado]**. Fonte: [BACKLOG.md:2025](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2025), [CORTES.md:532](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:532), [CORTES.md:544](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:544).

28. **LEI — CLASSE-D:** qual dono responde por `FATO_EM_AUSENCIA`: CADASTRO, BATIDA ou ESTRUTURA? Falta o corte de Ronald sobre o dono dessa classe; o encaminhamento dia a dia à lista do admin já consta do corte. Fonte: [HANDOFF-SESSAO.md:25](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/HANDOFF-SESSAO.md:25), [CORTES.md:544](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:544).

29. **DESENHO — RAIZ-DA-DISPUTA F5:** qual fronteira e qual ordem de pouso unem a F5 da raiz à E4d, depois das fases anteriores, para que os escritores e leitores não sejam migrados duas vezes? Fonte: [BACKLOG.md:1904](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:1904), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109).

30. **DESENHO — D1:** como `devolver_paridade_bug_par_103` deixa de gravar os três campos de materialização e os dois de validação, usando a trilha `DEVOLUCAO` e preservando a operação? Fonte: [validacao.py:346](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:346), [CORTES.md:543](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:543).

31. **DESENHO — D2:** como `ressuscitar_perguntas` deixa de gravar os sete campos de resposta, validação e materialização, com a `REABERTURA` marcada e a fila conservada? Fonte: [models.py:1694](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1694), [models.py:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1702), [CORTES.md:544](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:544).

32. **LEI — D2, efeito para o colaborador:** ressuscitar devolve a pergunta ao colaborador? Falta o corte **RESSUSCITAR-DEVOLVE-AO-COLABORADOR**; sua relação com a E4d e com a fila deve ser explicitada sem presumir resposta. Fonte: [CORTES.md:543](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:543), [CORTES.md:560](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:560).

33. **DESENHO — D3, sem ato:** como `devolver_instante_sem_texto_l130` deixará de limpar resposta e carimbo sem deixar mudança de estado sem ato, e como será tratado o estoque entregue sujeito a `!` próprio? Qual ato existente ou desenho autorizado representa essa operação é **[nao verificado]**. Fonte: [validacao.py:414](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:414), [validacao.py:440](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:440), [CORTES.md:560](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/CORTES.md:560).

34. **DESENHO — D4:** como `desfazer_carimbo_sem_lastro` deixará de gravar os três campos por A07, e qual ato representa o desfazimento? A equivalência do ato é **[nao verificado]**. Fonte: [relavra_via.py:246](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/relavra_via.py:246), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

35. **DESENHO — L1:** que passa a fazer `lavrar`, inclusive quando chamado pelo comando de perguntas stale, se os três campos da lavra deixarem de ser gravados? Fonte: [veredito_lavrado.py:270](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:270), [lavrar_perguntas_stale.py:89](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/lavrar_perguntas_stale.py:89), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

36. **DESENHO — L2:** que passa a fazer o `bulk_update` de `gravar_lavras_em_lote` e seu comando de backfill? Fonte: [veredito_lavrado.py:250](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/veredito_lavrado.py:250), [backfill_veredito_lavrado.py:138](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/management/commands/backfill_veredito_lavrado.py:138), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

37. **DESENHO — L3:** como `reapontar_resolvedora` preserva a leitura após deixar de salvar os três campos da lavra? Fonte: [fio.py:975](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:975), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

38. **DESENHO — L4:** como `rechavear_data_evento` preserva a leitura após deixar de salvar os três campos da lavra? Fonte: [fio.py:1091](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:1091), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

39. **DESENHO — L5:** como tratar a lavra no ramo de reuso de `_nascer`, inclusive sua fronteira com A22? A separação de L5 da porta é **[nao verificado]**. Fonte: [models.py:3195](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3195), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

40. **DESENHO — `motivos`, aparar:** como `aparar_motivo_respondido` deixa de gravar a lista e de qual ato ou fonte passa a obter seu efeito? Fonte: [models.py:2129](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2129), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

41. **DESENHO — `motivos`, abrir ou agregar:** como `_abrir_ou_agregar_disputa` passa a registrar e ler a inclusão de motivos? Fonte: [disputa_emissao.py:550](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:550), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109).

42. **DESENHO — `motivos`, órfãos:** como `criar_disputa_a_partir_de_chamados_orfaos` trata suas escritas de criação e atualização, e qual ato ou fonte as substitui? Fonte: [disputa_emissao.py:1097](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1097), [disputa_emissao.py:1269](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1269), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109).

43. **DESENHO — `motivos`, âncora:** como `criar_disputa_ancora_de_chamado` trata a gravação da lista e qual ato ou fonte a substitui? Fonte: [disputa_emissao.py:1372](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1372), [BACKLOG.md:2109](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2109).

44. **DESENHO — nascimento por `abrir`:** como tratar o default de `status_local` no nascimento e qual ato ou fonte sustentará a primeira leitura? Fonte: [models.py:1000](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1000), [BACKLOG.md:2089](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2089).

45. **DESENHO — nascimento por `criar`:** como tratar o default de `status_local` nesse sítio e qual ato ou fonte sustentará a primeira leitura? Fonte: [models.py:1154](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/mo

[truncado em 20000 de 22193 caracteres]

## 18/11 14:47 · sessao 5475c33f062c · ESPURIAS-PRE-MAPA · censo · origem=Code

O lote contém **29 linhas**, todas reproduzidas abaixo. O fechamento usa `[nv]` como categoria de incerteza: não consegui provar todas as classes, chamadores e fatias, portanto a lista final é **um mínimo confirmado**, não o tamanho real completo da obra. A unidade continua sendo a função do anexo, mesmo quando ela contém vários sítios. [app/core/medida_turno_do_par.py:10]

Convenções: **JP** = JUÍZO PARALELO; **EP** = ENTRADA DO PAR; **EX** = EXIBIÇÃO. Em (b), “cron” significa declarado em `config/crons.py`; a execução efetiva em produção é `[não verificado]`. Em (c), o efeito indicado é o alcance comprovado do caminho, sujeito às condições do código. Referências `ponto/...`, `escala/...` e `folha/...` são relativas a `app/`. [app/docs/BACKLOG.md:1846; app/config/crons.py:428]

| chave | linha(s) | (a) | (b) | (c) | (d) | (e) arquivo:linha conferido |
|---|---:|---|---|---|---|---|
| escala/regua_defesa.py::entradas_fora_do_inicio | 521 | JP | VIVO cron; chamador `escala/management/commands/termometro_regua.py:68`, cron `config/crons.py:428` | GRAVA lista de régua | DIA-DA-BATIDA-JUIZ-UNICO | [escala/regua_defesa.py:521](app/escala/regua_defesa.py#L521); [termometro_regua.py:93](app/escala/management/commands/termometro_regua.py#L93) |
| escala/utils.py::montar_grade_prevista | 807 | EX | VIVO cron, via `escala/utils.py:1027`; [nv] quanto à condição de ativação do caminho legacy | EXIBE órfãs | DIA-DA-BATIDA-JUIZ-UNICO | [escala/utils.py:807](app/escala/utils.py#L807); [reconciliar_grade.py:118](app/ponto/management/commands/reconciliar_grade.py#L118) |
| escala/utils.py::montar_grade_prevista_periodo_por_turno | 1491, 1511, 1521, 1534, 1545, 1831 | JP | VIVO cron, via `escala/utils.py:2098` e `reconciliar_grade.py:118`, condicionado à flag | [nv] | DIA-DA-BATIDA-JUIZ-UNICO | [escala/utils.py:1491](app/escala/utils.py#L1491); [escala/utils.py:1545](app/escala/utils.py#L1545); [escala/utils.py:1831](app/escala/utils.py#L1831) |
| folha/export.py::dias_ferias_com_batida | 500 | JP | VIVO request, via `folha/export.py:791`, `folha/views.py:214` | PAGA: classificação da exportação | DIA-DA-BATIDA-JUIZ-UNICO | [folha/export.py:500](app/folha/export.py#L500); [folha/export.py:791](app/folha/export.py#L791) |
| folha/porta_export.py::dias_fora_do_dia_pago | 104 | JP | [nv]; chamador `folha/porta_export.py:597` | EXIBE divergência para a porta | [nv] | [folha/porta_export.py:104](app/folha/porta_export.py#L104) |
| folha/porta_export.py::medir | 538 | JP | [nv] | EXIBE/mede contador da porta | [nv] | [folha/porta_export.py:538](app/folha/porta_export.py#L538) |
| ponto/calculador/nucleo.py::minutos_do_oraculo | 117, 141, 235 | JP | [nv]; chamadores `ponto/calculador/periodo.py:356` e `ponto/calculador/alimentacao.py:449` | [nv] | [nv]; o BACKLOG nomeia ESMERIL-ORÁCULO-NOTURNA, mas não provei que estes sítios sejam ele | [ponto/calculador/nucleo.py:141](app/ponto/calculador/nucleo.py#L141); [ponto/calculador/nucleo.py:235](app/ponto/calculador/nucleo.py#L235); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/janelas.py::competencia_fechada | 84 | JP | VIVO request, via `ponto/registro_batida.py:159` | GRAVA: guarda de ajuste de batida | DIA-DA-BATIDA-JUIZ-UNICO | [ponto/janelas.py:84](app/ponto/janelas.py#L84); [ponto/registro_batida.py:159](app/ponto/registro_batida.py#L159) |
| ponto/management/commands/desvio_o68b.py::Command._autopsia | 112, 121, 136 | JP | INSTRUMENTO; chamador `desvio_o68b.py:181` | EXIBE autópsia | [nv] | [desvio_o68b.py:112](app/ponto/management/commands/desvio_o68b.py#L112) |
| ponto/management/commands/detectar_intervalo_ausente.py::furos_intervalo | 82 | EX | VIVO cron, chamador `detectar_intervalo_ausente.py:120`, com `--alarme` em `config/crons.py:491` | EXIBE/alarma; esse cron força dry | [nv] | [detectar_intervalo_ausente.py:82](app/ponto/management/commands/detectar_intervalo_ausente.py#L82); [detectar_intervalo_ausente.py:108](app/ponto/management/commands/detectar_intervalo_ausente.py#L108) |
| ponto/management/commands/detectar_par_relampago.py::varrer_pares_embutidos | 303 | EP | VIVO cron, chamador `detectar_par_relampago.py:392`, cron `config/crons.py:605` | GRAVA: retrata batidas candidatas | DIA-DA-BATIDA-JUIZ-UNICO | [detectar_par_relampago.py:303](app/ponto/management/commands/detectar_par_relampago.py#L303); [detectar_par_relampago.py:399](app/ponto/management/commands/detectar_par_relampago.py#L399) |
| ponto/management/commands/emitir_furo_retroativo.py::Command.handle | 75, 100 | JP | AVULSO, comando sob ordem `config/crons.py:1414` | GRAVA com `--apply` | DIA-DA-BATIDA-JUIZ-UNICO | [emitir_furo_retroativo.py:100](app/ponto/management/commands/emitir_furo_retroativo.py#L100); [emitir_furo_retroativo.py:111](app/ponto/management/commands/emitir_furo_retroativo.py#L111) |
| ponto/management/commands/flip_automatico.py::Command.handle | 64 | JP | VIVO cron `config/crons.py:707` | GRAVA batida e nota; o dia civil também seleciona chamados para a nota | DIA-DA-BATIDA-JUIZ-UNICO | [flip_automatico.py:64](app/ponto/management/commands/flip_automatico.py#L64); [flip_automatico.py:76](app/ponto/management/commands/flip_automatico.py#L76) |
| ponto/management/commands/marcar_abandono_provisorio.py::Command.handle | 56 | JP | AVULSO, comando sob ordem `config/crons.py:1422` | GRAVA ausências com `--apply` | DIA-DA-BATIDA-JUIZ-UNICO | [marcar_abandono_provisorio.py:56](app/ponto/management/commands/marcar_abandono_provisorio.py#L56); [marcar_abandono_provisorio.py:76](app/ponto/management/commands/marcar_abandono_provisorio.py#L76) |
| ponto/management/commands/plano_b_no_dinheiro.py::Command.handle._espiao._cm_espiao | 88 | EX | INSTRUMENTO; instalado no método em `plano_b_no_dinheiro.py:101` | EXIBE; ensaio revertido | [nv] | [plano_b_no_dinheiro.py:88](app/ponto/management/commands/plano_b_no_dinheiro.py#L88); [plano_b_no_dinheiro.py:110](app/ponto/management/commands/plano_b_no_dinheiro.py#L110) |
| ponto/management/commands/reconciliar_fantasmas.py::_parse_pendentes | 66 | [nv] | VIVO cron, chamador `reconciliar_fantasmas.py:184`, com `--alarme` em `config/crons.py:392` | EXIBE/alarma no cron; `--apply` manual existe | [nv] | [reconciliar_fantasmas.py:66](app/ponto/management/commands/reconciliar_fantasmas.py#L66); [reconciliar_fantasmas.py:27](app/ponto/management/commands/reconciliar_fantasmas.py#L27) |
| ponto/management/commands/recusar_ponta_pequena.py::Command.handle | 105 | [nv]: data de cálculo do retrato, não provei classe de dia de batida | VIVO cron encadeado `config/crons.py:1585` | GRAVA decisão de HE com `--apply`, condicionada à foto do dia | [nv] | [recusar_ponta_pequena.py:105](app/ponto/management/commands/recusar_ponta_pequena.py#L105); [recusar_ponta_pequena.py:170](app/ponto/management/commands/recusar_ponta_pequena.py#L170) |
| ponto/management/commands/supra_juiz.py::Command.handle | 135, 222 | JP | VIVO cron `config/crons.py:599` | EXIBE/alarma para esses sítios; a escrita lê a lavra da célula | DIA-DA-BATIDA-JUIZ-UNICO | [supra_juiz.py:222](app/ponto/management/commands/supra_juiz.py#L222); [supra_juiz.py:241](app/ponto/management/commands/supra_juiz.py#L241) |
| ponto/management/commands/vigia_plantio_overnight.py::batidas_fora_do_turno_da_pergunta | 86 | EP | VIVO cron, chamador `vigia_plantio_overnight.py:110`, cron `config/crons.py:646` | EXIBE/alarma | DIA-DA-BATIDA-JUIZ-UNICO | [vigia_plantio_overnight.py:86](app/ponto/management/commands/vigia_plantio_overnight.py#L86) |
| ponto/motor_calculo_v2.py::Motor12x36.calcular_periodo | 2796, 2809 | JP | VIVO request, via `motor_calculo_v2.py:2458`, fechamento `ponto/services/fechamento.py:286` e request `ponto/views.py:1255` | PAGA: feriado e HE | f2 DIA: feriado cruzando meia-noite | [motor_calculo_v2.py:2796](app/ponto/motor_calculo_v2.py#L2796); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/motor_calculo_v2.py::MotorBase._intervalo_descontado_no_dia | 1313 | JP | [nv]; chamador `motor_calculo_v2.py:1803` | PAGA: limite de intervalo | [nv] | [motor_calculo_v2.py:1313](app/ponto/motor_calculo_v2.py#L1313); [motor_calculo_v2.py:1803](app/ponto/motor_calculo_v2.py#L1803) |
| ponto/motor_calculo_v2.py::MotorBase._janela_do_dia | 1528 | JP | [nv]; chamadores em `motor_calculo_v2.py:1691`, `:1733`, `:1760` | PAGA: janela do período | [nv] | [motor_calculo_v2.py:1528](app/ponto/motor_calculo_v2.py#L1528) |
| ponto/motor_calculo_v2.py::MotorBase._julgar_pontualidade | 2053, 2093, 2118, 2198, 2200, 2210 | JP | VIVO request, via `motor_calculo_v2.py:3306` e fechamento `ponto/services/fechamento.py:286` | PAGA: pontualidade | [nv] | [motor_calculo_v2.py:2053](app/ponto/motor_calculo_v2.py#L2053); [motor_calculo_v2.py:2198](app/ponto/motor_calculo_v2.py#L2198) |
| ponto/motor_calculo_v2.py::MotorBase._previsto_do_dia | 1227 | JP | [nv]; chamadores `motor_calculo_v2.py:1170`, `:1273`, `:1453`, `:1825` | PAGA: previsto do dia | [nv] | [motor_calculo_v2.py:1227](app/ponto/motor_calculo_v2.py#L1227) |
| ponto/motor_calculo_v2.py::MotorBase.calcular_mes | 2403, 2579, 2639, 2643, 2644 | JP | VIVO request, chamador `ponto/services/fechamento.py:286`, request `ponto/views.py:1255` | PAGA: jornada e reflexo DSR | f1 PAR **e** f2 DIA: a função também contém a herança por gap nomeada; não se atribui cada sítio automaticamente às duas | [motor_calculo_v2.py:2403](app/ponto/motor_calculo_v2.py#L2403); [motor_calculo_v2.py:2643](app/ponto/motor_calculo_v2.py#L2643); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/motor_calculo_v2.py::MotorComercial.calcular_mes | 3326, 3447, 3465, 3490, 3494, 3495 | JP | VIVO request, chamador `ponto/services/fechamento.py:286`, request `ponto/views.py:1255` | PAGA: jornada, banco e DSR | [nv] | [motor_calculo_v2.py:3326](app/ponto/motor_calculo_v2.py#L3326); [motor_calculo_v2.py:3490](app/ponto/motor_calculo_v2.py#L3490) |
| ponto/motor_calculo_v2.py::MotorComercial.calcular_periodo | 3264, 3268 | JP | VIVO request, via `motor_calculo_v2.py:2458` e fechamento `ponto/services/fechamento.py:286` | PAGA: feriado e HE | f2 DIA: feriado cruzando meia-noite | [motor_calculo_v2.py:3264](app/ponto/motor_calculo_v2.py#L3264); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/motor_calculo_v2.py::MotorTurnoPartido.calcular_mes | 2942, 2955 | JP | VIVO request, chamador `ponto/services/fechamento.py:286`, request `ponto/views.py:1255` | PAGA: jornada | f2 DIA: partido, sítio nomeado | [motor_calculo_v2.py:2942](app/ponto/motor_calculo_v2.py#L2942); [app/docs/BACKLOG.md:2051](app/docs/BACKLOG.md#L2051) |
| ponto/motor_calculo_v2.py::_periodos_pelo_marco | 578, 686, 687 | JP | [nv]; chamador `motor_calculo_v2.py:531` | PAGA: seleção de períodos | [nv] | [motor_calculo_v2.py:578](app/ponto/motor_calculo_v2.py#L578); [motor_calculo_v2.py:686](app/ponto/motor_calculo_v2.py#L686) |

A atribuição de f2 aos sítios de feriado identifica o tema registrado, **não prova que toda execução dessas linhas cruze meia-noite**. A classificação de efeito também não afirma que houve pagamento ou escrita em produção. [app/docs/BACKLOG.md:2051; app/ponto/motor_calculo_v2.py:2796; app/ponto/motor_calculo_v2.py:3264]

**Fecho (a) × (b), deste lote:**

| (a) | VIVO | AVULSO | INSTRUMENTO | SEM CHAMADOR | [nv] | Total |
|---|---:|---:|---:|---:|---:|---:|
| EXIBIÇÃO | 2 | 0 | 1 | 0 | 0 | 3 |
| ENTRADA DO PAR | 2 | 0 | 0 | 0 | 0 | 2 |
| JUÍZO PARALELO | 12 | 2 | 1 | 0 | 7 | 22 |
| [nv] | 2 | 0 | 0 | 0 | 0 | 2 |
| **Total** | **18** | **2** | **2** | **0** | **7** | **29** |

A soma é uma contagem das classificações da tabela acima; os sete `[nv]` em (b) não são prova de ausência de chamador. [app/core/medida_turno_do_par.py:33]

**JUÍZO PARALELO + VIVO + GRAVA/PAGA comprovados no lote:**

- `escala/regua_defesa.py::entradas_fora_do_inicio` — grava lista de régua. [termometro_regua.py:68](app/escala/management/commands/termometro_regua.py#L68); [termometro_regua.py:93](app/escala/management/commands/termometro_regua.py#L93)
- `folha/export.py::dias_ferias_com_batida` — participa da classificação da exportação. [folha/export.py:500](app/folha/export.py#L500); [folha/export.py:791](app/folha/export.py#L791)
- `ponto/janelas.py::competencia_fechada` — guarda usada no registro de batida. [ponto/janelas.py:84](app/ponto/janelas.py#L84); [ponto/registro_batida.py:159](app/ponto/registro_batida.py#L159)
- `ponto/management/commands/flip_automatico.py::Command.handle` — grava batida e seleciona chamados por dia para nota. [flip_automatico.py:64](app/ponto/management/commands/flip_automatico.py#L64)
- Motor: `Motor12x36.calcular_periodo`, `MotorBase._julgar_pontualidade`, `MotorBase.calcular_mes`, `MotorComercial.calcular_mes`, `MotorComercial.calcular_periodo` e `MotorTurnoPartido.calcular_mes`. [ponto/services/fechamento.py:286](app/ponto/services/fechamento.py#L286); [ponto/views.py:1255](app/ponto/views.py#L1255); [motor_calculo_v2.py:2458](app/ponto/motor_calculo_v2.py#L2458); [motor_calculo_v2.py:3306](app/ponto/motor_calculo_v2.py#L3306)

A função de grade por turno permanece fora dessa lista porque seu efeito de gravação ou pagamento ficou `[nv]`; as demais linhas com caminho `[nv]` também impedem tratar os **10 itens comprovados** como contagem final. [escala/utils.py:1545](app/escala/utils.py#L1545)

**Não visto pelo método:** outros pareadores por estado; outras regras de meia-noite e heranças; chamadas dinâmicas e parte da composição interprocedural; HTML/JS, código gerado e pastas excluídas. Não há número para esses pontos cegos. [app/core/medida_turno_do_par.py:33](app/core/medida_turno_do_par.py#L33); [app/core/medida_turno_do_par.py:44](app/core/medida_turno_do_par.py#L44); [app/core/medida_turno_do_par.py:46](app/core/medida_turno_do_par.py#L46)

## 18/11 14:46 · sessao 7be273638f9b · ESPURIAS-PRE-MAPA · censo · origem=Code

O lote contém **32 funções**. A conferência abaixo não fecha a classificação de todos os caminhos: **12 funções classificadas como JUÍZO PARALELO ficaram com o caminho de execução [nv]**. A lista curta, portanto, é um mínimo comprovado nesta leitura, não o tamanho final da obra. [anexo lote_00: não possui arquivo:linha; app/core/medida_turno_do_par.py:46]

Usei **J** = JUÍZO PARALELO, **E** = ENTRADA DO PAR, **X** = EXIBIÇÃO, **V** = VIVO, **A** = AVULSO e **I** = INSTRUMENTO. `[nv]` significa não verificado. Em (c), o efeito é o alcançado pelo caminho citado; uma função auxiliar pode apenas retornar um valor usado pela escrita. Em (d), `D` significa DIA-DA-BATIDA-JUIZ-UNICO. A atribuição de fatia que não consegui provar ficou [nv]. [app/docs/BACKLOG.md:1846] [app/docs/BACKLOG.md:2051]

| Chave | Linha(s) | (a) | (b) Caminho e chamador | (c) Efeito | (d) Fatia | (e) Conferido |
|---|---:|---|---|---|---|---|
| `escala/utils.py::_alinhar_por_papel` | 529 | J | [nv]; chamada interna em `escala/utils.py:623` | [nv] | [nv] | [app/escala/utils.py:511] |
| `escala/utils.py::_papeis_por_alternancia` | 496 | J | [nv]; chamada interna em `escala/utils.py:608` | [nv] | [nv] | [app/escala/utils.py:477] |
| `ponto/calculador/nucleo.py::minutos_do_oraculo` | 246 | J, no ramo que pareia por sequência | [nv]; também chamado por `ponto/calculador/alimentacao.py:449` | [nv] | [nv] | [app/ponto/calculador/nucleo.py:65] |
| `ponto/management/commands/e6_piso_autopsia.py::Command.handle` | 94 | J | I: autópsia | EXIBE/mede | [nv] | [app/ponto/management/commands/e6_piso_autopsia.py:80] |
| `ponto/management/commands/vigiar_motor_autoridade.py::Command.handle` | 85 | [nv] | I; cron em `config/crons.py:596` | EXIBE/mede | [nv] | [app/ponto/management/commands/vigiar_motor_autoridade.py:80] |
| `ponto/turnos.py::_pares_marcados` | 812 | J | [nv]; chamada interna em `ponto/turnos.py:545` | [nv] | [nv] | [app/ponto/turnos.py:798] |
| `ponto/motor_calculo_v2.py::turnos_via_autoridade` | 503 | E | [nv]; chamada interna em `ponto/motor_calculo_v2.py:530` | PAGA: integra cálculo do motor; caminho até fechamento em `ponto/services/fechamento.py:286` | f1 PAR | [app/ponto/motor_calculo_v2.py:503] |
| `ponto/motor_calculo_v2.py::MotorBase._turnos_do_juiz` | 882 | E | [nv]; chamada interna em `ponto/motor_calculo_v2.py:2346` | PAGA: integra cálculo do motor; fechamento em `ponto/services/fechamento.py:286` | f1 PAR | [app/ponto/motor_calculo_v2.py:867] |
| `ponto/motor_calculo_v2.py::MotorBase.calcular_mes` | 2408 | J | [nv]; fechamento chama o motor em `ponto/services/fechamento.py:286` | PAGA: cálculo do motor | f1 PAR e f2 DIA, conforme complemento do caso | [app/ponto/motor_calculo_v2.py:2403] [app/docs/BACKLOG.md:2051] |
| `api/views.py::api_bater_ponto` | 719 | J | V request: `api/urls.py:67` | GRAVA chamado com `data_turno` no contexto | D | [app/api/views.py:716] |
| `api/views_core.py::api_bater_ponto` | 713 | J | [nv]; rota desta cópia aponta para `views`, em `api/urls.py:2` e `:67` | GRAVA chamado com `data_turno` no contexto, se executada | D | [app/api/views_core.py:710] |
| `chamados/juizes.py::_dia_da_batida` | 54 | J | [nv]; chamada por `chamados/juizes.py:131` | [nv] | D | [app/chamados/juizes.py:51] |
| `chamados/juizes.py::_dia_do_contexto_fraco` | 170 | J | [nv]; chamada por `chamados/juizes.py:139` e `:230` | [nv] | D | [app/chamados/juizes.py:162] |
| `chamados/juizes.py::_plantio_na_hora_do_slot` | 275 | J | [nv]; chamada por `chamados/juizes.py:764` | [nv] | D | [app/chamados/juizes.py:254] |
| `chamados/juizes.py::dias_do_fato` | 204 | J | [nv]; há chamadores em `chamados/services/fila_por_causa.py:162` e `escala/services/propositor.py:633` | [nv] | D | [app/chamados/juizes.py:201] |
| `chamados/management/commands/auditar_disputas.py::Command.handle` | 111 | J | A: relatório sob demanda, `config/crons.py:1465` | EXIBE relatório | D | [app/chamados/management/commands/auditar_disputas.py:107] |
| `chamados/management/commands/carimbar_por_batida_coincidente.py::Command.handle` | 81 | J | A: comando, `config/crons.py:1321` | GRAVA contexto e vínculo de célula com `--apply` | D | [app/chamados/management/commands/carimbar_por_batida_coincidente.py:80] |
| `chamados/management/commands/migrar_data_evento_dia_do_turno.py::_fonte_viva` | 102 | E | A: migração; chamada por `migrar_data_evento_dia_do_turno.py:154` | GRAVA pergunta na migração com `--aplicar` [nv quanto a este ramo específico] | [nv] | [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:94] |
| `chamados/management/commands/migrar_data_evento_dia_do_turno.py::classificar` | 158 | E | A: migração; chamada por `migrar_data_evento_dia_do_turno.py:212` | GRAVA pergunta na migração com `--aplicar` [nv quanto a este ramo específico] | [nv] | [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:150] |
| `chamados/services/desvio_escala.py::medir_desvio` | 65 | J | V cron: `config/crons.py:707` → `ponto/management/commands/flip_automatico.py:66` → `desvio_escala.py:103` | GRAVA causa do flip de batida e nota em chamado | D | [app/chamados/services/desvio_escala.py:58] [app/ponto/management/commands/flip_automatico.py:64] |
| `chamados/services/disputa_emissao.py::_orfao_resolvido_cross_meianoite` | 729, 730 | J | V cron: `config/crons.py:362` → `ponto/management/commands/processar_alertas_turno.py:382` → `disputa_emissao.py:797` | GRAVA/PERGUNTA: a guarda interfere no universo da fábrica de disputa | D | [app/chamados/services/disputa_emissao.py:721] |
| `chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` | 1213 | J | V cron: `config/crons.py:362` → `ponto/management/commands/processar_alertas_turno.py:385` | GRAVA disputa/pergunta | D | [app/chamados/services/disputa_emissao.py:1207] |
| `chamados/services/explicador.py::_explicar_validada` | 70 | X | [nv]; chamada por `chamados/services/explicador.py:137` | EXIBE explicação; `_d0` é calculado nesse sítio | [nv] | [app/chamados/services/explicador.py:65] |
| `chamados/services/materializacao.py::_plantio_preenche_furo_na_data` | 434, 455 | J | [nv]; chamadas internas em `chamados/services/materializacao.py:220` e `:920` | GRAVA plantio indiretamente, conforme resultado da guarda; caminho vivo [nv] | D | [app/chamados/services/materializacao.py:404] |
| `chamados/services/passivo_validado.py::_dia_do_plantio` | 113 | J | A: `materializar_passivo_validado.py:113` → `passivo_validado.py:344`; não agendado conforme `config/crons.py:859` | GRAVA plantio no modo apply | D | [app/chamados/services/passivo_validado.py:98] |
| `chamados/services/passivo_validado.py::_plantar_o_dia` | 547, 549 | J | A: `materializar_passivo_validado.py:113` → `passivo_validado.py:443` | GRAVA plantio no modo apply; a guarda pode reverter o ato | D | [app/chamados/services/passivo_validado.py:537] |
| `chamados/services/passivo_validado.py::reparar` | 348 | X | A: `materializar_passivo_validado.py:113` | EXIBE/conta divergência de competência nesse sítio | [nv] | [app/chamados/services/passivo_validado.py:344] |
| `colaboradores/services/calendario.py::contexto_calendario` | 273 | X | V request: `colaboradores/views.py:1990` | EXIBE batidas retratadas por dia civil | [nv] | [app/colaboradores/services/calendario.py:258] |
| `core/esteira_vigia.py::instante_de_ontem` | 1082 | [nv] | I: chamado para congelar testes em `core/esteira_vigia.py:1358` | EXIBE/fornece instante para teste | [nv] | [app/core/esteira_vigia.py:1080] |
| `escala/detector_proposta.py::medir_padrao` | 58 | J | [nv]; chamada por `escala/detector_proposta.py:120` | EXIBE proposta no caminho read-only citado; outros efeitos [nv] | D | [app/escala/detector_proposta.py:49] [app/escala/detector_proposta.py:118] |
| `escala/management/commands/hub_smoke.py::Command.handle.win_map` | 95 | [nv] | I: smoke | EXIBE/mede [nv quanto ao consumo completo do mapa] | [nv] | [app/escala/management/commands/hub_smoke.py:91] |
| `escala/regua_defesa.py::diagnostico_regua` | 157 | J | V cron: `config/crons.py:428` → `escala/management/commands/termometro_regua.py:43` → `escala/regua_defesa.py:735` | GRAVA escore no vínculo | D | [app/escala/regua_defesa.py:153] [app/escala/regua_defesa.py:730] |

A contagem abaixo inclui **[nv] como balde de conferência**, não como nova classe do aval. Soma: **32**. Os rótulos e as classificações correspondem às linhas da tabela acima. [app/docs/BACKLOG.md:1846]

| (a) × (b) | VIVO | AVULSO | INSTRUMENTO | Caminho [nv] | Total |
|---|---:|---:|---:|---:|---:|
| JUÍZO PARALELO | 5 | 4 | 1 | 12 | 22 |
| ENTRADA DO PAR | 0 | 2 | 0 | 2 | 4 |
| EXIBIÇÃO | 1 | 1 | 0 | 1 | 3 |
| Classe [nv] | 0 | 0 | 3 | 0 | 3 |
| **Total** | **6** | **7** | **4** | **15** | **32** |

**JUÍZO PARALELO + VIVO + GRAVA/PAGA comprovados nesta leitura: 5 funções.** As duas primeiras influenciam uma escrita pela decisão que retornam; a terceira decide a emissão de pergunta; a quarta grava o contexto do chamado; a quinta influencia o escore gravado. [app/ponto/management/commands/flip_automatico.py:66] [app/ponto/management/commands/processar_alertas_turno.py:379] [app/api/views.py:717] [app/escala/regua_defesa.py:730]

- `chamados/services/desvio_escala.py::medir_desvio` — causa do flip e nota. [app/chamados/services/desvio_escala.py:65] [app/ponto/management/commands/flip_automatico.py:66]
- `chamados/services/disputa_emissao.py::_orfao_resolvido_cross_meianoite` — guarda da fábrica de pergunta. [app/chamados/services/disputa_emissao.py:729] [app/chamados/services/disputa_emissao.py:797] [app/ponto/management/commands/processar_alertas_turno.py:382]
- `chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` — emissão de disputa/pergunta. [app/chamados/services/disputa_emissao.py:1213] [app/ponto/management/commands/processar_alertas_turno.py:385]
- `api/views.py::api_bater_ponto` — chamado com data no contexto. [app/api/views.py:719] [app/api/urls.py:67]
- `escala/regua_defesa.py::diagnostico_regua` — escore gravado no vínculo. [app/escala/regua_defesa.py:157] [app/escala/regua_defesa.py:735] [app/config/crons.py:428]

O medidor declara como **não vistos pelo método**, entre outros, pareadores por estado fora das portas nomeadas, outras regras de meia-noite e heranças, além de certas chamadas dinâmicas e conteúdo em HTML/JS. Não há contagem desses pontos cegos no dado fornecido. [app/core/medida_turno_do_par.py:35] [app/core/medida_turno_do_par.py:44] [app/core/medida_turno_do_par.py:46]

## 18/11 14:46 · sessao 9dc0fd09c600 · ESPURIAS-PRE-MAPA · censo · origem=Code

A conferência abaixo cobre as **33 linhas fornecidas no anexo**. Ela não fecha todas as classificações: `[nv]` indica célula que não consegui provar. A linha do BACKLOG define as três classes e registra as fatias; ela nomeia `_dia_de` na fatia 2 e a herança por gap como caso das duas fatias. [app/docs/BACKLOG.md:1846; app/docs/BACKLOG.md:2051]

Na tabela, os caminhos são relativos a `app/`; **E** = ENTRADA DO PAR, **J** = JUÍZO PARALELO, **I** = INSTRUMENTO e **V** = caminho vivo. Em (c), o efeito é o do caminho indicado em (b), não necessariamente uma escrita feita pela própria função. A indicação de cron é a declaração na cópia; sua instalação ou execução efetiva em produção é [não verificado]. [app/config/crons.py:1; app/config/crons.py:5]

| chave | linha(s) | (a) | (b) caminho e chamador | (c) | (d) fatia | (e) conferido |
|---|---:|---|---|---|---|---|
| `ponto/motor_calculo_v2.py::minutos_noturnos` | 97,98 | [nv] | V, motor: `ponto/motor_calculo_v2.py:983` | PAGA | [nv] | `ponto/motor_calculo_v2.py:97` |
| `ponto/motor_calculo_v2.py::turnos_via_autoridade` | 470 | E | V, motor: `ponto/motor_calculo_v2.py:530` | PAGA | [nv] | `ponto/motor_calculo_v2.py:470` |
| `ponto/propositor.py::dia_posicional_completo` | 364 | J | [nv]; chamada em `chamados/reconciliador.py:113` | [nv] | [nv] | `ponto/propositor.py:364` |
| `ponto/registro_batida.py::_casa_marco_proprio` | 43 | J | V, porta de batida: `ponto/registro_batida.py:113`; request: `ponto/services/triagem_batida.py:700` | GRAVA batida, condicional | [nv] | `ponto/registro_batida.py:43` |
| `ponto/registro_batida.py::_janela_oposto` | 28 | J | V, triagem: `ponto/services/triagem_batida.py:715` | GRAVA batida, condicional | [nv] | `ponto/registro_batida.py:28` |
| `ponto/services/cartorio.py::_julgar_colab_corpo` | 1205,1358 | E | V, cron declarado `processar_cartorio --apply`: `config/crons.py:616`; chamada: `ponto/services/cartorio.py:996` | GRAVA [nv] | [nv] | `ponto/services/cartorio.py:1205`; `ponto/services/cartorio.py:1358` |
| `ponto/services/credito_parcial.py::dia_do_periodo` | 36 | J | V, fechamento: `ponto/services/credito_parcial.py:43`; `ponto/services/fechamento.py:998` | PAGA | [nv] | `ponto/services/credito_parcial.py:36` |
| `ponto/services/dia_pago.py::_dia_de` | 132 | J | V, lavratura: `ponto/services/dia_pago.py:174` | PAGA, DiaPago | f2 DIA | `ponto/services/dia_pago.py:132`; `docs/BACKLOG.md:2051` |
| `ponto/services/dias_em_aberto.py::passivo_da_competencia` | 123 | [nv] | AVULSO, comando: `pautas/management/commands/pauta_dia_em_aberto.py:98` | EXIBE | [nv] | `ponto/services/dias_em_aberto.py:123` |
| `ponto/services/esmeril_espelho.py::assinaturas_do_colab` | 170,177 | J | V, cron declarado: `config/crons.py:640`; chamada: `ponto/services/esmeril_espelho.py:195` | [nv] | [nv] | `ponto/services/esmeril_espelho.py:170` |
| `ponto/services/espelho.py::autoridade_do_periodo` | 1029 | E | V, espelho: `ponto/services/espelho.py:1227`; request: `ponto/views.py:161` | EXIBE | [nv] | `ponto/services/espelho.py:1029` |
| `ponto/services/espelho.py::montar_dias` | 426 | J | V, espelho: `ponto/services/espelho.py:1262`; request: `ponto/views.py:161` | EXIBE | [nv] | `ponto/services/espelho.py:426` |
| `ponto/services/espelho.py::somar_periodos` | 84 | J | V, espelho: `ponto/services/espelho.py:439`; request: `ponto/views.py:161` | EXIBE | [nv] | `ponto/services/espelho.py:84` |
| `ponto/services/fechamento.py::_so_a_data` | 1569 | [nv] | [nv]; chamada: `ponto/services/fechamento.py:1595` | [nv] | [nv] | `ponto/services/fechamento.py:1569` |
| `ponto/services/fechamento.py::apurar_por_fatia` | 204 | E | V, fechamento: `ponto/services/fechamento.py:754`; request: `ponto/views.py:1255` | PAGA | [nv] | `ponto/services/fechamento.py:204`; `ponto/services/fechamento.py:253` |
| `ponto/services/fechamento.py::recalcular_fechamento_mes` | 655,657,663,683,754,793,839,851,935,947 | J | V, request: `ponto/views.py:1255` | PAGA, fechamento | [nv] | `ponto/services/fechamento.py:793`; `ponto/services/fechamento.py:935` |
| `ponto/services/flip_auto.py::fila_flip_recusas` | 140 | J | V, cron declarado: `config/crons.py:707`; chamada: `ponto/services/flip_auto.py:148` | [nv] | [nv] | `ponto/services/flip_auto.py:140` |
| `ponto/services/regularizacao.py::regularizar_turno_orfao` | 60,73 | J | V, request: `ponto/views.py:3369` | GRAVA justificativa e ausência, condicional | [nv] | `ponto/services/regularizacao.py:73`; `ponto/services/regularizacao.py:91` |
| `ponto/services/triagem_batida.py::_bloqueio_horario_clt` | 424 | [nv] | V, triagem: `ponto/services/triagem_batida.py:657` | GRAVA chamado, condicional | [nv] | `ponto/services/triagem_batida.py:424`; `ponto/services/triagem_batida.py:459` |
| `ponto/services/triagem_batida.py::ramo_regularizar_saida` | 247,254,260 | J | V, request: `ponto/views.py:339` | GRAVA justificativa | [nv] | `ponto/services/triagem_batida.py:258` |
| `ponto/signals.py::_dias_batida` | 111 | E | V, signal: `ponto/signals.py:135` | GRAVA [nv] | [nv] | `ponto/signals.py:111` |
| `ponto/signals.py::_recompute_around` | 23 | E | V, signal: `ponto/signals.py:36` | GRAVA [nv] | [nv] | `ponto/signals.py:23` |
| `ponto/turnos.py::_dia_das_batidas_puro` | 2967 | J | V, espelho: `ponto/services/espelho.py:451`; chamada: `ponto/turnos.py:2927` | EXIBE; outros efeitos [nv] | DIA-DA-BATIDA-JUIZ-UNICO [nv] | `ponto/turnos.py:2967` |
| `ponto/turnos.py::dia_do_turno_da_batida` | 2945 | E | V, chamada: `chamados/services/disputa_emissao.py:283` | [nv] | [nv] | `ponto/turnos.py:2945` |
| `ponto/turnos.py::par_completo` | 2212 | E | V, cron declarado: `config/crons.py:605`; chamada: `ponto/management/commands/detectar_par_relampago.py:353` | GRAVA batida, condicional | [nv] | `ponto/turnos.py:2212`; `ponto/management/commands/detectar_par_relampago.py:362` |
| `ponto/views.py::regularizar_retorno` | 3386 | J | V, request: `ponto/urls.py:45` | EXIBE | [nv] | `ponto/views.py:3386` |
| `relatorios/management/commands/diff_extrato_parcial.py::Command._antes` | 45,51,58,61,67,68 | E | I, chamado pelo comando: `relatorios/management/commands/diff_extrato_parcial.py:117` | EXIBE | [nv] | `relatorios/management/commands/diff_extrato_parcial.py:68` |
| `relatorios/management/commands/diff_extrato_parcial.py::Command.handle` | 102,106,140 | [nv] | I, comando `diff_extrato_parcial` | EXIBE | [nv] | `relatorios/management/commands/diff_extrato_parcial.py:102`; `relatorios/management/commands/diff_extrato_parcial.py:140` |
| `relatorios/pdf_espelho.py::_coletar_dados_espelho` | 298 | E | V, PDF: `relatorios/views.py:1098`; chamada: `relatorios/pdf_espelho.py:1046` | EXIBE | [nv] | `relatorios/pdf_espelho.py:298` |
| `relatorios/views.py::extrato_parcial` | 486,593 | [nv] | V, request: `relatorios/urls.py:17` | EXIBE | [nv] | `relatorios/views.py:486`; `relatorios/views.py:593` |
| `ponto/motor_calculo_v2.py::turnos_via_autoridade` — porta nomeada | 503 | E | V, motor: `ponto/motor_calculo_v2.py:530` | PAGA | f1 PAR [nv] | `ponto/motor_calculo_v2.py:503` |
| `ponto/motor_calculo_v2.py::MotorBase._turnos_do_juiz` — porta nomeada | 882 | E | V, motor: `ponto/motor_calculo_v2.py:2346` | PAGA | f1 PAR [nv] | `ponto/motor_calculo_v2.py:882` |
| `ponto/motor_calculo_v2.py::MotorBase.calcular_mes` — herança por gap | 2408 | J | V, fechamento: `ponto/services/fechamento.py:754`; request: `ponto/views.py:1255` | PAGA | f1 PAR e f2 DIA | `ponto/motor_calculo_v2.py:2408`; `docs/BACKLOG.md:2051` |

**Ressalva da linha `par_completo`:** o chamador citado passa uma janela pronta; a derivação na linha 2212 só roda quando `janela is None`. Portanto, o cron prova o caminho da função, mas não prova que execute esse sítio específico. [app/ponto/turnos.py:2211; app/ponto/management/commands/detectar_par_relampago.py:353]

A contagem abaixo é da **classificação parcial desta tabela**, não um fechamento sem lacunas do censo. As células `[nv]` entram explicitamente na soma:

| (a) \ (b) | VIVO | AVULSO | INSTRUMENTO | [nv] | Total |
|---|---:|---:|---:|---:|---:|
| EXIBIÇÃO | 0 | 0 | 0 | 0 | 0 |
| ENTRADA DO PAR | 11 | 0 | 1 | 0 | 12 |
| JUÍZO PARALELO | 15 | 0 | 0 | 1 | 16 |
| [nv] | 3 | 1 | 1 | 1 | 6 |
| **Total** | **29** | **1** | **2** | **2** | **34** |

A soma acima revela uma inconsistência na própria consolidação: **não fecha as 33 linhas**. A contagem correta por inspeção da tabela é **[nv]**; não a apresento como censo fechado. [não verificado]

A lista **provisória**, pelas células J + V + GRAVA/PAGA, é:

- `_casa_marco_proprio` e `_janela_oposto`: caminho de registro de batida, condicionado às guardas. [app/ponto/registro_batida.py:105; app/ponto/registro_batida.py:113; app/ponto/services/triagem_batida.py:700]
- `dia_do_periodo`: participa do cálculo de atraso no fechamento. [app/ponto/services/credito_parcial.py:43; app/ponto/services/fechamento.py:998]
- `_dia_de`: atribuição de dia na lavratura de DiaPago, com fallback pela data local da entrada. [app/ponto/services/dia_pago.py:132; app/ponto/services/dia_pago.py:174]
- `recalcular_fechamento_mes`: caminho de fechamento acionado pela view. [app/ponto/services/fechamento.py:793; app/ponto/views.py:1255]
- `regularizar_turno_orfao` e `ramo_regularizar_saida`: gravam justificativa nos caminhos indicados. [app/ponto/services/regularizacao.py:91; app/ponto/services/triagem_batida.py:258]
- `MotorBase.calcular_mes`, na herança por gap: caso registrado nas duas fatias. [app/ponto/motor_calculo_v2.py:2408; app/docs/BACKLOG.md:2051]

Essa lista não é o tamanho real fechado da obra: há efeitos, chamadores e classes marcados `[nv]`. O método também declara pontos cegos, entre eles pareadores por estado, outras regras de meia-noite e heranças não reconhecidas, além das limitações da resolução estática; não há número para o que ele não viu. [app/core/medida_turno_do_par.py:40; app/core/medida_turno_do_par.py:46]

## 18/11 14:41 · sessao 0f40a97e3846 · PRE-MAPA-E4D · censo · origem=Code

A busca literal encontrou **52 hits de escrita**, correspondentes a **26 pares campo × função**. Destes, **um par está fora das portas**: duas atribuições de `reaberturas_colab` em `montar_chamados_fio`, em memória sobre `ChamadoColaborador`, e não sobre o campo de `DisputaSupervisao`. A classificação mecânica inclui essas atribuições; a distinção de modelo está documentada em [contratos_estruturais.py:719](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/contratos_estruturais.py:719).

A unidade das tabelas é **campo × função**; as linhas listadas incluem tanto atribuições quanto itens de listas usadas em `update_fields`. As portas seguem o mapeamento fornecido nos anexos e os respectivos corpos nesta cópia. A busca também encontrou uma atribuição múltipla de `pendencia_admin` ao examinar o contexto, embora a linha não seja um hit do padrão pedido: [materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365).

### `resolvido_em`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:_gravar_estado:235](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:235), [250](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:250), [254](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:254) | A01 |

**0 fora das portas (rg acima, 9 hits lidos).**

### `respondido_por`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:_gravar_estado:237](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:237), [238](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:238) | A01 |

**0 fora das portas (rg acima, 8 hits lidos).**

### `fechada_em`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:fechar:2095](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2095), [2099](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2099) | A02 |
| [models.py:reabrir_admin:2312](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2312), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |
| [models.py:reabrir_sistema:2336](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2336), [2340](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2340) | A04 |

**0 fora das portas (rg acima, 23 hits lidos).**

### `fechada_por`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:fechar:2096](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2096), [2099](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2099) | A02 |
| [models.py:reabrir_admin:2313](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2313), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |
| [models.py:reabrir_sistema:2337](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2337), [2340](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2340) | A04 |

**0 fora das portas (rg acima, 8 hits lidos).**

### `resolucao`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:fechar:2097](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2097), [2099](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2099) | A02 |
| [models.py:reabrir_admin:2314](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2314), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |
| [models.py:reabrir_sistema:2338](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2338), [2340](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2340) | A04 |
| [models.py:limpar_resolucao_residual:2428](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2428), [2429](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2429) | A24 |

**0 fora das portas (rg acima, 42 hits lidos).**

### `motivo_fechamento`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:fechar:2098](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2098), [2099](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2099) | A02 |
| [models.py:reabrir_admin:2315](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2315), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |
| [models.py:reabrir_sistema:2339](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2339), [2340](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2340) | A04 |

**0 fora das portas (rg acima, 11 hits lidos).**

### `reaberturas_admin`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:reabrir_admin:2316](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2316), [2317](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2317) | A03 |

**0 fora das portas (rg acima, 4 hits lidos).**

### `reaberturas_colab`

| Escrita: arquivo, função e linha | Porta ou tipo |
|---|---|
| [models.py:reabrir_por_colab:2402](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2402), [2403](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2403) | A23 |
| [fio.py:montar_chamados_fio:390](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:390), [394](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/fio.py:394) | **Fora das portas; outro:** anotação em memória em outro modelo. [contratos_estruturais.py:719](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/contratos_estruturais.py:719) |

### `recusa_motivo`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [models.py:carimbar_identidade:3384](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3384), [3385](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:3385) | A06 |
| [disputa_emissao.py:responder_pergunta_disputa_core:1511](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1511), [1513](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/disputa_emissao.py:1513) | A12 |
| [materializacao.py:_materializar_resposta_hora:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365), [1366](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1366) | A17 |
| [materializacao.py:_carimbar_recusa:1702](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1702), [1703](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1703) | A18 |
| [validacao.py:recusar_pergunta:916](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:916), [922](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:922) | A19 |

**0 fora das portas (rg acima, 31 hits lidos).**

### `pendencia_admin`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [materializacao.py:_materializar_resposta_hora:1366](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1366), item de `update_fields` | A17 |
| [materializacao.py:_materializar_resposta_hora:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365), atribuição múltipla encontrada no contexto, **fora dos hits do padrão literal** | A17 |

**0 fora das portas (rg acima, 7 hits lidos).**

### `data_conferida_em`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [validacao.py:conferir_data_do_fato:996](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:996), [998](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:998) | A20 |

**0 fora das portas (rg acima, 4 hits lidos).**

### `data_conferida_por`

| Escrita: arquivo, função e linha | Porta |
|---|---|
| [validacao.py:conferir_data_do_fato:997](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:997), [998](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/validacao.py:998) | A20 |

**0 fora das portas (rg acima, 4 hits lidos).**

### Classificação dos demais hits

As definições de campo e migration são **DEFINIÇÃO**:

- `resolvido_em`: [models.py:66](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:66), [migration 0006:28](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0006_followup_sem_glpi.py:28); `respondido_por`: [models.py:64](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:64), [migration 0006:33](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0006_followup_sem_glpi.py:33).
- `fechada_em`: [models.py:1894](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1894), índice em [1943](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1943), campo e índice em [migration 0013:24](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0013_disputasupervisao.py:24) e [34](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0013_disputasupervisao.py:34).
- `fechada_por`: [models.py:1903](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1903), [migration 0013:28](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0013_disputasupervisao.py:28); `resolucao`: [models.py:1910](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1910), [migration 0013:25](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0013_disputasupervisao.py:25).
- `motivo_fechamento`: [models.py:1930](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1930), [migration 0049:14](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0049_disputa_motivo_fechamento.py:14).
- `reaberturas_admin`: [models.py:1899](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1899), [migration 0018:15](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0018_disputasupervisao_reaberturas_admin.py:15); `reaberturas_colab`: [models.py:1895](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:1895), [migration 0015:13](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0015_disputasupervisao_reaberturas_colab.py:13).
- `recusa_motivo`: [models.py:2747](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2747), [migration 0040:15](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0040_a27_recusa_motivo.py:15); `pendencia_admin`: [models.py:2743](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2743), [migration 0043:15](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0043_hx_pendencia_admin.py:15).
- `data_conferida_em`: [models.py:2645](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2645), [migration 0044:18](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0044_perguntadisputa_data_conferida_em_and_more.py:18); `data_conferida_por`: [models.py:2646](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/models.py:2646), [migration 0044:23](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/migrations/0044_perguntadisputa_data_conferida_em_and_more.py:23).

Os demais hits compreendem **LEITURAS** e também texto que não é operação sobre o campo: comentários, documentação, parâmetros de função, variáveis locais e argumentos de chamadas comuns. Por exemplo, os `resolucao=` nos signals são argumentos de `remover_motivo`, não keywords de `update` ou `create`: [escala/signals.py:177](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/escala/signals.py:177), [colaboradores/signals.py:80](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/colaboradores/signals.py:80). **A classificação individual, com arquivo:linha, de todos esses demais hits é [nao verificado].**

**Contagens:** 160 hits lidos; 52 hits de escrita na busca, agrupados em 26 pares campo × função; **1 par fora das portas**, com 2 hits de atribuição em memória. A atribuição adicional de `pendencia_admin` em [materializacao.py:1365](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/services/materializacao.py:1365) não integra os 160 nem os 52 hits. Escritas não capturadas pelo padrão literal e execução em produção: **[nao verificado]**.

## 18/11 14:40 · sessao 08fcba64cfc5 · ESPURIAS-PRE-MAPA · censo · origem=Code

O censo identificou **quatro pontos de apresentação que afirmam texto sobre a HE sem ler a decisão do dia**: o aviso e o relógio do espelho, o relógio do calendário e a observação do PDF. Os demais consumidores de estado identificados usam `estado_por_dia` ou `DecisaoHE.objects.vigentes()`; a contagem e sua unidade estão no fecho. [app/templates/ponto/espelho.html:189; app/templates/ponto/espelho.html:237; app/templates/colaboradores/partials/_calendario_grade.html:127; app/templates/core/_relogio_riscado.html:15; app/relatorios/pdf_espelho.py:932; app/ponto/services/gestao_he.py:86; app/ponto/motor_calculo_v2.py:1416]

A tabela de juízes consultada se apresenta como lookup, aponta `app/core/juizes.py` como registro vivo e diz que o código prevalece em caso de divergência. Uma entrada específica para decisão de HE nessa tabela ou no registro: **[não verificado]**. Para a pergunta “a decisão vale hoje?”, o próprio modelo nomeia `DecisaoHE.objects.vigentes()`; `estado_por_dia` publica o mapa por colaborador e dia a partir dessa consulta. [app/docs/JUIZES.md:3; app/docs/JUIZES.md:9; app/ponto/models.py:1251; app/ponto/services/gestao_he.py:63]

Apenas `autorizado` e `nao` são estados armazenados; `nao` registra decisão e ciência, enquanto a ausência de decisão vigente é apresentada como `sem_decisao` pela Gestão. O retrato conserva dias e minutos, além do estado fotografado na lavratura; a Gestão sobrepõe a decisão viva para apresentar o estado atual. [app/ponto/models.py:1283; app/ponto/models.py:1287; app/ponto/services/he_pendente_lavrado.py:132; app/ponto/services/gestao_he.py:71; app/ponto/services/gestao_he.py:271]

| Sítio | O que imprime ou decide | Autoridade lida para o estado; texto fixo |
|---|---|---|
| [Espelho — montador](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/services/espelho.py:720) | Monta `he_fora_da_janela` a partir das pontas marcadas e de relatos do motor, inclusive dia sem previsto. É a entrada dos avisos da tela e do PDF. | Marcador de janela e relato do motor para **ocorrência e minutos**; não há leitura de decisão de HE nesse trecho. O comentário associa as ocorrências a “sem decisão”. [app/ponto/services/espelho.py:727; app/ponto/services/espelho.py:740; app/ponto/services/espelho.py:773] |
| [Espelho — aviso](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/templates/ponto/espelho.html:189) | Mostra minutos e “sem decisão” para cada ocorrência. | Lê `dia.he_fora_da_janela`, sem leitor de estado na linha. **Texto fixo:** “sem decisão”; o título também afirma que os minutos não viram HE ou não contam sem decisão. |
| [Espelho — relógio](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/templates/ponto/espelho.html:237) | Mostra relógio riscado nas células marcadas fora da janela. | Lê a marca da célula e inclui o componente com minutos, sentido e marco. **Texto fixo:** “não conta”, sem estado de HE no contexto passado ao componente. [app/templates/core/_relogio_riscado.html:15] |
| [Calendário — relógio](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/templates/colaboradores/partials/_calendario_grade.html:127) | Mostra o mesmo relógio riscado para batida marcada fora da janela. | Lê a marca da batida e inclui o mesmo componente. **Texto fixo:** “não conta”, sem estado de HE no contexto passado. [app/templates/core/_relogio_riscado.html:15] |
| [PDF do espelho](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/relatorios/pdf_espelho.py:927) | Imprime observação por ocorrência, com minutos e, quando existe, sentido e marco. | Lê `dia.he_fora_da_janela` do espelho. **Texto fixo:** “não conta sem decisão” para dia sem jornada prevista; “não conta” para ponta com marco. [app/relatorios/pdf_espelho.py:932; app/relatorios/pdf_espelho.py:937] |
| [Gestão de HE — tela](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/views.py:932) | Mostra e filtra dias sem decisão, autorizados e com ciência; apresenta também os totais. | `he_pendente_lavrado.ler` fornece o retrato; `enriquecer` sobrepõe `estado_por_dia` e consulta diretamente `DecisaoHE` vigente para os totais de minutos da decisão. Os rótulos são condicionados ao estado. [app/ponto/services/gestao_he.py:105; app/ponto/services/gestao_he.py:245; app/ponto/services/gestao_he.py:278; app/templates/ponto/gestao_he.html:259; app/templates/ponto/gestao_he.html:314] |
| [Gestão de HE — PDF](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/views.py:804) | Imprime por colaborador e em total dias sem decisão, minutos autorizados e minutos “não”. | Mesmo retrato e `enriquecer` da tela: `estado_por_dia` para estado dos dias e `DecisaoHE` vigente diretamente para totais da foto da decisão. [app/ponto/views.py:807; app/ponto/views.py:841; app/ponto/services/gestao_he.py:105; app/ponto/services/gestao_he.py:250] |
| [Gestão — seleção para atos em lote](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/services/gestao_he.py:370) | Seleciona dias ainda sem decisão para ciência ou autorização por período; `so_acima` restringe o segundo universo. | Retrato mais `enriquecer` e, portanto, `estado_por_dia`; a seleção testa `sem_decisao` e o limite do dia. [app/ponto/services/gestao_he.py:399; app/ponto/services/gestao_he.py:404] |
| [Central de relatórios](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/relatorios/views.py:37) | Mostra quantidade de dias sem decisão, indisponibilidade do contador e empresas sem retrato. | `pendentes_na_central` combina dias do retrato com `estado_por_dia`; o template condiciona as frases ao resultado. [app/ponto/services/gestao_he.py:461; app/ponto/services/gestao_he.py:475; app/templates/relatorios/index.html:125] |
| [Painel de chamados — atalho HE](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/chamados/views.py:761) | Mostra contador de dias aguardando ciência ou traço quando falta retrato. | Mesmo `pendentes_na_central`: retrato mais `estado_por_dia`. [app/ponto/services/gestao_he.py:466; app/ponto/services/gestao_he.py:475; app/templates/chamados/painel_gestao.html:105] |
| [Folha — portão de exportação](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/folha/porta_export.py:675) | Conta dias acima do limite e sem decisão para a trava configurável; a mensagem de recusa nomeia os dias. Qualquer estado presente, inclusive `nao`, sai dessa lista. | Ocorrências do espelho, `estado_por_dia` para decisão e `acima_do_limite` para o limite. A mensagem é condicionada à lista e à configuração da trava. [app/folha/porta_export.py:223; app/folha/porta_export.py:230; app/folha/porta_export.py:681; app/folha/porta_export.py:858] |
| [Motor — cálculo](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/motor_calculo_v2.py:1397) | Lê se o dia está autorizado para decidir o tratamento das pontas; também consulta o mapa para dia sem previsto. | `DecisaoHE.objects.vigentes()` diretamente, em mapa interno; a comparação é com `autorizado`. Não imprime rótulo de estado nesses pontos. [app/ponto/motor_calculo_v2.py:1416; app/ponto/motor_calculo_v2.py:1455; app/ponto/motor_calculo_v2.py:1653; app/ponto/motor_calculo_v2.py:1694; app/ponto/motor_calculo_v2.py:1739] |

No **app**, a API chama `espelho_do_colab`, mas o payload de dias examinado publica batidas, status, palavra do dia e horas; o resumo publica totais de extras. Não foi demonstrada ali a publicação do estado da decisão de HE ou de `he_fora_da_janela`; a apresentação pelo cliente nativo é **[não verificado]**. Os totais têm suas próprias fontes declaradas, incluindo folha ou ausência de apuração, e não provam o estado da decisão de cada HE. [app/api/views.py:1355; app/api/views.py:1535; app/api/views.py:1595; app/api/views.py:1648; app/api/views.py:1667]

**Fecho da contagem:** a unidade é o **sítio consumidor ou apresentador na tabela**, excluindo o montador, que é alimentação, e excluindo o app por não ter sido demonstrado leitor de estado. São **11 sítios**: seis leem `estado_por_dia` direta ou indiretamente, um lê `DecisaoHE` diretamente no motor e quatro apresentam texto sem leitor de estado. A tela e o PDF da Gestão também fazem leitura direta de `DecisaoHE` para os totais de minutos, portanto essa leitura é uma sobreposição e não acrescenta sítios à soma. [app/ponto/services/gestao_he.py:105; app/ponto/services/gestao_he.py:250; app/ponto/services/gestao_he.py:401; app/ponto/services/gestao_he.py:475; app/folha/porta_export.py:681; app/ponto/motor_calculo_v2.py:1416]

A lista de texto fixo é: **aviso do espelho**, **relógio do espelho**, **relógio do calendário** e **observação do PDF do espelho**. O componente de relógio é um único texto compartilhado por dois pontos de inclusão. [app/templates/ponto/espelho.html:189; app/templates/ponto/espelho.html:237; app/templates/colaboradores/partials/_calendario_grade.html:127; app/templates/core/_relogio_riscado.html:15; app/relatorios/pdf_espelho.py:932]

A completude fora dos caminhos examinados e qualquer ocorrência ou quantidade em produção permanecem **[não verificado]**. O arquivo de saída solicitado não foi criado, em respeito à instrução de somente leitura.

## 18/11 14:39 · sessao 498b353cf973 · ESPURIAS-PRE-MAPA · censo · origem=Code

O medidor retornou **9 pareadores e 85 derivações de dia: 94 parcelas**, correspondentes a **88 funções distintas**. A contagem soma os itens nomeados aos dois contadores; por isso, algumas funções aparecem mais de uma vez. A entrada usada foi `censo('app')`, com `python3 -I`, sem importar Django. ([medida_turno_do_par.py:10](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:10), [medida_turno_do_par.py:426](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:426), [medida_turno_do_par.py:448](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:448))

**O censo solicitado está incompleto:** não conferi classe, cadeia de chamada, efeito e fatia para cada função. Portanto, a tabela de totais por classe × caminho e a lista curta de juízos paralelos vivos que gravam ou pagam ficam **[não verificado]**. Não seria seguro preencher esses totais a partir apenas da detecção lexical, que o próprio medidor diz não ser prova semântica. ([medida_turno_do_par.py:18](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:18))

Um ponto conferido é `MotorBase.calcular_mes`: o medidor identifica a herança de dia por intervalo entre períodos; a função usa `data_jornada = _prev_dj` sob a condição de até seis horas. O fechamento chama `motor.calcular_mes`, e o BACKLOG registra essa herança na fatia 2 DIA de MOTOR-LE-O-TURNO. Sua classificação completa no recorte pedido e a prova do caminho vivo específico permanecem **[não verificado]**. ([motor_calculo_v2.py:2400](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/motor_calculo_v2.py:2400), [fechamento.py:286](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/ponto/services/fechamento.py:286), [BACKLOG.md:2051](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/docs/BACKLOG.md:2051))

**Não visto pelo método**, sem número atribuído: outros pareadores por estado fora das formas reconhecidas; outras regras de meia-noite e heranças fora das formas reconhecidas; e novos sítios A1/B1 dentro de função já contada como aumento separado da catraca. A2 e B2 são aproximações para classificação, não prova de derivação da jornada. ([medida_turno_do_par.py:46](/home/ronald/codex_ws/ca53ea0e77eaed4009be1c42105f21367ccc9700/app/core/medida_turno_do_par.py:46))

## 18/11 14:39 · sessao 10dcfb4f1a51 · ESPURIAS-PRE-MAPA · leitura · origem=Code

A classificação mais sustentada pelo estado c9 é **FIXTURE nos dois testes identificados**, incluindo os dois subcasos de M8. A explicação causal é uma inferência a partir dos dubles atuais e do contrato: o diff c8→c9 e os erros da execução c8 não estão disponíveis para verificar a mudança e o traceback exatos. [não verificado] [nao verificado]

| Teste | Classificação e evidência | [nao verificado]
|---|---|
| `test_MORDE_quando_a_FOLHA_cobre_o_pendente_e_IGNORADO` | **FIXTURE.** O teste passa `_Aus` como cobertura e verifica que ela prevalece sobre o pedido pendente ([teste:93](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_palavra_parcial_e_pendente.py:93)). O duble atual fornece `pk` ([teste:35](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/tests/test_palavra_parcial_e_pendente.py:35)); o ramo de cobertura de `do_dia` lê `cobertura.pk` para retornar `ausencia_id` ([dia_decidido.py:473](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:473)). Isso sustenta a leitura de duble defasado, sem demonstrar o erro histórico exato. [não verificado] |
| `test_nao_perguntado_intencional_preserva_o_juiz_da_oferta` | **FIXTURE.** O teste usa `calendario_inteiro` e espera oferta verdadeira com `em_aberto_perguntado=False` ([teste:788](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/test_e3_degrau_dono_e_porta.py:788)). O `do_dia` dublado agora devolve `quem`, `quando` e `ausencia_id` como `None` ([e3_lu3_apoio.py:100](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:100)); o calendário acessa as três chaves diretamente ([calendario.py:748](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:748)). |
| `test_M8_carga_inteira_nao_engole_relogio`, **`aberto=False`** | **FIXTURE**, pela mesma costura: o subcaso usa `calendario_inteiro`, cujo retorno dublado precisa das três chaves lidas pelo calendário ([teste:752](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/test_e3_degrau_dono_e_porta.py:752); [duble:100](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:100); [leitor:757](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/colaboradores/services/calendario.py:757)). |
| `test_M8_carga_inteira_nao_engole_relogio`, **`aberto=True`** | **FIXTURE**, pelo mesmo duble; o laço executa separadamente os estados de turno fechado e aberto ([teste:754](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/test_e3_degrau_dono_e_porta.py:754); [duble:100](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/chamados/tests/e3_lu3_apoio.py:100)). |

O contrato atual de `do_dia` associa `quem`, `quando` e `ausencia_id` à ausência que deu a palavra; quando o dia não foi decidido por ausência, os três campos são `None` ([dia_decidido.py:434](/home/ronald/codex_ws/6e8aa01ca5482369e6d71a5c5cabb78b9d8e1b33/app/ponto/services/dia_decidido.py:434)). A mudança do contrato é o contexto da defasagem; as evidências atuais apontam para os dubles como causa, sem provar defeito no código de produção nem o histórico exato do vermelho. [não verificado]

Quanto ao merge, o anexo fornecido informa `rc 0`, uma árvore `cda458298117a0463f3b86addd90f5846cc1c7f2` e ausência de conflito entre as pontas c9 e F6F7 c7; pela informação relatada, **não houve conflito textual no merge, inclusive em `app/api/views.py`**. O merge não foi reproduzido nem suas entradas foram verificadas nesta leitura. [não verificado]

_6996 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
