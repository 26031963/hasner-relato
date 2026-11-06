# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 09:42** · publicado em **06/11/2026 09:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 09:42 · sessao c5a68eae791b · AKITA-MERGE-DE-VOLTA · verificador · origem=Code

**Não aprovaria como está: a interseção por nomes admite um furo com renomeação.** O filtro novo presume que ausência no diff significa igualdade naquele caminho; essa premissa pode falhar com `--name-only` e renames (`bin/tests/test_lei_akita.sh:183`; anexo → `bin/tests/test_lei_akita.sh:196–197`).

Abaixo, **“novo” indica a numeração resultante do delta fornecido**.

1. **Há um contraexemplo que deve continuar vermelho.**

   Considere este merge hipotético: primeiro pai contém `app/x.py=B`; segundo pai, publicado, contém `app/x.py=A`; resultado remove `app/x.py` e cria `app/docs/x.py=A`. Escolha B suficientemente diferente de A para não haver rename detectado contra o primeiro pai. Reprodução desse cenário com Git: **[nao verificado]**.

   Com detecção de rename ativa, o diff contra o primeiro pai pode listar a deleção `app/x.py` e a adição documental; contra o publicado, o rename exato pode listar somente o destino `app/docs/x.py`. A interseção elimina a deleção de negócio, embora **ambos os pais ainda tenham `app/x.py`**. O juiz antigo reprova pelo caminho de negócio; o novo aceita `n/a` porque sobra apenas documentação (`bin/tests/test_lei_akita.sh:123–135`, `:183–185`; novo `bin/tests/test_lei_akita.sh:196–198`).

   A correção indicada é usar `--no-renames` nos dois diffs, ou contabilizar origem e destino. Há precedente no repo que inclui ambas as pontas ao tratar renames (`bin/tests/test_lei_protege_sitio.sh:104–109`).

   Para caminhos contabilizados integralmente, os demais casos são coerentes com a exceção proposta:

   - **Pouso main←raia e primeiro pai publicado:** o diff inicial já representa a comparação com esse pai; intersectá-lo novamente seria redundante. Pais adicionais não publicados não alteram a lista (`bin/tests/test_lei_akita.sh:183`; novo `bin/tests/test_lei_akita.sh:190–197`).
   - **Octopus:** percorre todos os pais adicionais; basta igualdade com um publicado para retirar o caminho. Não exige igualdade com todos eles (novo `bin/tests/test_lei_akita.sh:190–197`).
   - **Resolução de conflito:** resultado diferente de todos os pais publicados permanece julgado; escolher integralmente uma versão publicada fica dispensado, conforme a exceção proposta (novo `bin/tests/test_lei_akita.sh:186–198`).
   - **Raia publicada por outro caminho:** a ancestralidade é consultada contra `origin/main`, sem restringir ao primeiro pai dessa história (novo `bin/tests/test_lei_akita.sh:191–195`).

   Reintroduzir uma versão antiga de pai publicado também fica dispensado. Isso decorre expressamente do critério proposto; o cabeçalho não estabelece uma exigência adicional de igualdade com o **main vigente** (`bin/tests/test_lei_akita.sh:58–62`; novo `bin/tests/test_lei_akita.sh:186–197`).

2. **QUEM ESCREVE e QUEM LÊ.**

   `falhas_do_universo` produz a lista; `julgar → _julgar_linha → negocio` a consome. Externamente saem defeitos e contadores, sem exportação da lista (`bin/tests/test_lei_akita.sh:180–187`, `:122–146`, `:317–321`).

   O pre-push chama esse mesmo selo com `LEI_AKITA_HEAD=PUSH_SHA` e barra retorno não zero; a régua executa o mesmo arquivo pelo glob de selos (`bin/pre-push.sh:33–38`; `bin/regua.sh:138–145`). O instalador atual escreve um wrapper que delega a `bin/pre-push.sh`; conteúdo do hook efetivamente instalado: **[nao verificado]** (`bin/hooks.sh:49–56`).

   Existe a cópia antiga `app/core/hooks/pre-push.sh`, identificada pelo próprio repo como migração que não pousou (`bin/tests/test_recursos_sourced.sh:26–30`). Não encontrei outro juiz desse predicado; inexistência absoluta de implementação equivalente: **[nao verificado]**.

3. **O novo MORDE morde os dois mutantes pedidos, por leitura.**

   | Mutante | Por que falha |
   |---|---|
   | “Merge não é julgado” | Falta a volta maligna no conjunto exigido. |
   | “Nenhum pai é publicado” | A volta limpa também reprova, sobrando um hash indevido. | [nao verificado]

   A expectativa é exatamente `{maligna}` (novo `bin/tests/test_lei_akita.sh:335–338`).

   Sobrevive um mutante plausível: **examinar apenas o segundo pai**, ignorando terceiro e seguintes; o plantio usa merges de dois pais. Também não há nesse plantio rename nem injeção de erro do `merge-base` (novo `bin/tests/test_lei_akita.sh:317–338`). O caso antigo já exige um pouso vermelho quando a raia traz negócio, protegendo contra dispensar merges indiscriminadamente (`bin/tests/test_lei_akita.sh:280–293`).

4. **Sim: rc do `merge-base` fora de 0/1 gera `PADRAO_ERRO` e impede verde silencioso nesse caminho.** [nao verificado]

   O delta lança `SystemExit` explicitamente; os demais comandos novos passam pelo wrapper estrito (novo `bin/tests/test_lei_akita.sh:190–196`; base `bin/tests/test_lei_akita.sh:153–159`). A interrupção impede `PADRAO_FIM`, cuja ausência incrementa falhas e leva à saída vermelha (`bin/tests/test_lei_akita.sh:317–326`, `:333–337`).

Execução do MORDE: **[nao verificado]**; seu preparo cria arquivos e repositórios, incompatível com a regra de somente leitura (`bin/tests/test_lei_akita.sh:237–255`).

## 06/11 09:40 · sessao 076be9594384 · - · - · origem=Ronald (tmux)

Achado principal: **renomeação pode invalidar a inferência “sumiu do diff = igual ao pai publicado”**. Hipótese: P1 contém `app/x.py=B`; P2 publicado contém `app/x.py=A`; H remove `app/x.py` e cria `app/docs/x.py=A`. Sem detectar rename contra P1, a lista contém deleção de negócio; detectando rename exato contra P2, `--name-only` pode listar apenas o destino documental. A interseção então elimina a deleção e permite `n/a`. A execução dessa hipótese está `[nao verificado]`; o risco decorre da interseção por nomes no delta proposto `bin/tests/test_lei_akita.sh:196-197`, da classificação em `bin/tests/test_lei_akita.sh:122-135` e da ausência de `--no-renames` em `bin/tests/test_lei_akita.sh:183`. Correção sugerida: desligar renames em **ambos** os diffs, ou considerar ambas as pontas.

Os demais casos têm leitura consistente com a regra proposta:

- Primeiro pai publicado: já está representado pelo diff inicial; intersectá-lo outra vez seria idempotente. Pouso com raia não publicada mantém o julgamento atual. `bin/tests/test_lei_akita.sh:180-185`; delta proposto `:190-197`.
- Octopus: examina todos os pais adicionais e mantém apenas caminhos diferentes de cada pai publicado. Resolução diferente de todos eles permanece; escolha integral de versão publicada sai deliberadamente. Delta proposto `:190-197`. [nao verificado]
- Raia publicada por outro caminho: `merge-base --is-ancestor` não depende do nome do ramo nem de primeiro pai. Delta proposto `:191-195`. [nao verificado]
- Ressuscitar versão antiga de pai publicado é **ambiguidade de política**, não contraexemplo inequívoco à exceção declarada: a versão coincide integralmente com pai publicado, precisamente o critério do delta. O cabeçalho define universo de commits, sem definir atualidade de blobs, em `bin/tests/test_lei_akita.sh:58-62`; o juiz não verifica semanticamente a cura, conforme `:7-8`.

O novo MORDE mata “merges não são julgados” porque faltaria `maligna`, e “nenhum pai é publicado” porque sobraria também `limpa`; compara exatamente `{maligna}` no delta proposto `:335-338`. Sobrevivem “examinar só o segundo pai/parar no primeiro publicado”, ausência de tratamento de erro do `merge-base`, e o problema de renomeação: não há octopus, injeção desse erro ou rename no novo plantio, delta proposto `:317-338`. O mutante “todos os pais são publicados” é morto pelo MORDE **já existente**, cujo `pouso` deve continuar vermelho: `bin/tests/test_lei_akita.sh:280` e `:288-293`.

RC do novo `merge-base` fora de 0/1 gera explicitamente `PADRAO_ERRO`, delta proposto `:193-194`. A interrupção impede `PADRAO_FIM`, exigido pelo envelope em `bin/tests/test_lei_akita.sh:317-326`; portanto não fica verde silencioso nesse caminho. Os demais novos comandos passam pelo wrapper estrito em `bin/tests/test_lei_akita.sh:153-159`.

## 06/11 09:39 · sessao 4b900836e2b1 · PEP-REENTRADA · verificador · origem=Code

**A v2 protege a P corrente nos caminhos síncronos demonstrados de S143 e S144. Não encontrei outro escritor automático dessa P nesses caminhos; isso não prova exclusão em chamadas arbitrárias, pois a proteção está nos chamadores, não na porta.** As guardas estão em `app/chamados/services/disputa_emissao.py:1586`, `:1671`, `:1710` e `app/chamados/services/materializacao.py:1399`; `carimbar` não consulta o sinal (`app/chamados/models.py:2562`).

**Os testes melhoraram, mas ainda deixam passar mutantes relevantes, especialmente S144 desligado e S144 “pula todas”.** O controle posterior chama a função diretamente e não inclui irmã fora de voo (`app/chamados/tests/test_pep_reentrada.py:223–229`). Resultados de execução dos testes e mutantes: **[nao verificado]**.

1. **Quem escreve, quem lê e quais caminhos alcançam o bloco**

O censo abaixo trata dos caminhos de aplicação; os testes novos são examinados no item 4. Chamadas dinâmicas externas à cópia: **[nao verificado]**. [nao verificado]

| Estado | Escritores encontrados | Leitores relevantes |
|---|---|---|
| Trio da P: `materializada_em`, `materializada_por`, `via_resolucao` | A atribuição conjunta está em `_escrever_carimbo`; persistem `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote` (`app/chamados/models.py:2556`, `:2609`, `:2644`, `:2652`, `:2663`). | Lastro e C6 leem materialização/via; `.estado`, seleção do materializador, S143 e S144 também (`app/chamados/juizes.py:647`, `:673`, `:1171`, `:1207`; `app/chamados/models.py:2501`; `app/chamados/services/materializacao.py:1343`, `:1392`; `app/chamados/services/disputa_emissao.py:1572`). |
| Somente `via_resolucao` | Há escritores preexistentes fora de `carimbar`: `encerrar_por_fato_no_espelho` e `relavra_via.relavrar` (`app/chamados/models.py:2673`; `app/chamados/services/relavra_via.py:123`). | C6, explicador e apresentação leem a via (`app/chamados/juizes.py:1171`; `app/chamados/services/explicador.py:95`; `app/templates/chamados/partials/_card_atendimento.html:186`). |
| Autor da materialização | Além da escrita conjunta, é lido para procedência, fotografia anterior e auditoria (`app/chamados/models.py:2559`, `:2583`; `app/ponto/services/procedencia_batida.py:71`; `app/chamados/reconciliador.py:1134`). | A procedência escolhe `validada_por` ou `materializada_por` (`app/ponto/services/procedencia_batida.py:72`). |
| D: `fechada_em`, `resolucao` | `fechar` preenche ambos; `reabrir_admin` e `reabrir_sistema` zeram ambos; limpeza residual zera somente resolução (`app/chamados/models.py:1861–1865`, `:2063–2075`, `:2087–2091`, `:2173–2174`). | `.aberta`, idempotência de `fechar`, receivers, caudas e apresentação (`app/chamados/models.py:1765`, `:1820`; `app/chamados/signals.py:106`, `:130`; `app/chamados/services/materializacao.py:1290`, `:1302`; `app/templates/chamados/partials/modal_fio.html:374`). |
| `_EM_VOO` | Declaração no módulo, `set` na entrada e `reset(token)` no `finally` (`app/chamados/services/materializacao.py:1106`, `:1120`, `:1124`). | `pergunta_em_voo` e, por ele, as três guardas S143 e a guarda S144; o teste da cauda lê diretamente o conjunto (`app/chamados/services/materializacao.py:1129`, `:1399`; `app/chamados/services/disputa_emissao.py:1586`, `:1671`, `:1710`; `app/chamados/tests/test_pep_reentrada.py:109`). |

**Cada chamador de `PerguntaDisputa.carimbar`:**

| Chamador | Alcance dentro do bloco e efeito sobre a P corrente |
|---|---|
| Materializador, `app/chamados/services/materializacao.py:1251` | **Sim:** é o escritor esperado, após lastro aprovado, usando declaração ou via derivada do fato (`:1207`, `:1235–1252`). |
| S143, `app/chamados/services/disputa_emissao.py:1612` | **Sim**, pelo fecho do destino e receiver; pula a P antes do julgamento/carimbo (`app/chamados/services/materializacao.py:949`; `app/chamados/signals.py:91–92`; `app/chamados/services/disputa_emissao.py:1586`). |
| S143 sem credor, `app/chamados/services/disputa_emissao.py:1686` | A varredura é alcançável, mas exclui perguntas validadas e ainda tem guarda explícita de voo (`:1665–1672`). |
| S144, `app/chamados/services/materializacao.py:1405` | **Sim**, por qualquer fecho síncrono de disputa; pula a P corrente (`app/chamados/signals.py:139–140`; `app/chamados/services/materializacao.py:1399`). |
| Reconciliador sem turno, `app/chamados/reconciliador.py:405` | **Sim para irmãs**, pela batida. A P-hora corrente tem texto obrigatório e é barrada nesse reconciliador (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1417`, `:1603`, `:364`; `app/chamados/services/materializacao.py:804`). |
| Reconciliador por célula, `app/chamados/reconciliador.py:586` | **Sim para irmãs**, pela batida; sua seleção exclui a P validada corrente (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1416`, `:620`; `app/chamados/services/materializacao.py:1345`). |
| Absorção de ausência, `app/chamados/services/materializacao.py:549` | Vem da porta de resposta explícita. Não foi demonstrada chamada dessa porta pelos receivers síncronos do plantio; alcance adicional **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1327–1330`). |
| Wrapper fantasma, `app/chamados/models.py:2685` | Chamado pelo consumidor de eventos, não pela cadeia síncrona de batida examinada (`app/chamados/reconciliador.py:1136`, `:1363`; `app/config/crons.py:394`). |

**Cada chamador de `DisputaSupervisao.fechar`:**

| Chamador | Alcance durante o bloco / tratamento da P em voo |
|---|---|
| S143, `app/chamados/services/disputa_emissao.py:1713` | Síncrono; **não fecha disputa que contenha P em voo**, pela guarda `:1710`. |
| Reconciliador, `app/chamados/reconciliador.py:412` | Pode fechar por uma irmã no signal da batida; o S144 resultante pula a P corrente (`app/ponto/signals.py:215`; `app/chamados/services/materializacao.py:1399`). |
| Reconciliador, `app/chamados/reconciliador.py:602` | Mesmo alcance, por irmã absolvida pela célula; S144 pula a P corrente (`app/chamados/reconciliador.py:1416`; `app/chamados/services/materializacao.py:1399`). |
| Cauda do materializador, `app/chamados/services/materializacao.py:1307` | Executa **depois** da unidade e da saída do sinal (`:1350–1353`). |
| Absorção, `app/chamados/services/materializacao.py:568` | Porta de resposta explícita; alcance pelo plantio **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1330`). |
| Becos, `app/chamados/services/becos.py:52` | O gancho da pergunta usa `on_commit`, portanto executa depois do bloco normal (`app/chamados/signals.py:197–202`). |
| Fantasmas, `app/chamados/reconciliador.py:1160` | Consumidor de eventos separado (`:1363`; `app/config/crons.py:394`). |
| Emissão manual, `app/chamados/services/disputa_emissao.py:1118` | Alcance síncrono pelo plantio **[nao verificado]**. A fábrica chama outra porta, `criar_ou_atualizar_disputa` (`app/chamados/services/fabrica_da_celula.py:139`). |
| Tranca, `app/chamados/services/competencia_trancada.py:204` | Alcance pelo plantio **[nao verificado]**; se fechasse dentro dele, o S144 ainda passaria pela guarda (`app/chamados/services/materializacao.py:1399`). |
| Desligamento, `app/colaboradores/services/desligamento.py:142` | Alcance pelo plantio **[nao verificado]**; mesmo tratamento S144 (`app/chamados/services/materializacao.py:1399`). |
| Entrada sincronizada, `app/ponto/services/entrada_sincronizada.py:59` | Chamador API; alcance inverso pelo materializador **[nao verificado]** (`app/api/views.py:805`). |
| Foto recebida, `app/api/foto_recebida.py:80` | Chamador API; alcance inverso pelo materializador **[nao verificado]** (`app/api/views.py:871`). |
| Comando, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Execução separada; chamada pelo plantio **[nao verificado]**. |
| Wrapper `admin_encerra`, `app/chamados/models.py:1879` | Alcance pelo plantio **[nao verificado]**. |
| Wrapper `remover_motivo`, `app/chamados/models.py:1986` | Associado aos eventos de posto e âncora; alcance pelo plantio **[nao verificado]** (`app/colaboradores/signals.py:77`; `app/escala/signals.py:161`). |

O preenchimento efetivo de `fechada_em` encontrado na aplicação continua sendo o save da porta `fechar`; as outras escritas encontradas o zeram nas reaberturas (`app/chamados/models.py:1865`, `:2075`, `:2091`).

**Ressalva sobre “pela porta `carimbar`”:** o próprio materializador já possui encerramentos por identidade — paridade e colisão — usando `carimbar_identidade`. Não são um segundo escritor automático introduzido pela obra, mas impedem afirmar que *todo* encerramento durante o voo usa literalmente `carimbar` (`app/chamados/services/materializacao.py:899`, `:1074`).

Também existe caminho do cartório para descarimbo, mas ele exige C6 vivo; a P corrente validada é morta no primeiro degrau do C6 e não passa nessa condição (`app/chamados/services/fabrica_da_celula.py:166`; `app/chamados/services/relavra_via.py:227–231`; `app/chamados/juizes.py:1136–1137`).

2. **S144: outros chamadores e eventual falta de carimbo**

O único chamador direto de aplicação encontrado é o receiver; existem chamadas diretas nos testes de porta e no teste novo (`app/chamados/signals.py:139–140`; `app/chamados/tests/test_porta_desfecho_pergunta.py:226`, `:244`, `:248`, `:263`, `:292`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:691`; `app/chamados/tests/test_pep_reentrada.py:228`).

A premissa **“S144 roda só uma vez por fecho” está errada**: o receiver aceita qualquer `post_save` de disputa fechada, sem comparar estado anterior ou `update_fields`. Uma gravação posterior pode acioná-lo novamente; que ela ocorrerá é **[nao verificado]** (`app/chamados/signals.py:124–140`).

No cenário proposto — fecho durante o plantio, S144 pula, materializador reprova — **o fecho ocorrido dentro daquele atomic também volta**. Tanto a reprovação do lastro quanto a recusa de `carimbar` levantam `_Reprovada` dentro do mesmo bloco (`app/chamados/services/materializacao.py:1194`, `:1208–1217`, `:1251–1258`).

Os desfechos demonstráveis são:

- **Sucesso:** o materializador carimba pela via dele antes de sair (`app/chamados/services/materializacao.py:1235–1256`).
- **Rollback com lastro anterior que permaneça aprovado:** a cauda pode fechar depois do sinal e o S144 carimbar por `VIA_JUIZ_APROVOU`, condicionado aos juízes aceitarem (`app/chamados/services/materializacao.py:1287–1307`, `:1352`, `:1401–1405`).
- **Recusa persistente:** não há garantia de carimbo eventual. O `continue` não agenda repetição; novas tentativas existem pelas portas humanas, mas sua execução futura é **[nao verificado]** (`app/chamados/services/materializacao.py:1399–1400`; `app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/fio.py:666`; `app/chamados/services/validacao.py:430`).

Portanto, **não há demonstração de orfandade permanente causada pelo exemplo de rollback**, nem base para prometer “alguém sempre carimba depois”. A própria validação também pode devolver a validação recusada e reabrir a disputa, em vez de esconder a recusa com um carimbo (`app/chamados/services/validacao.py:424–427`).

3. **Escopo do sinal**

| Situação | Resultado pela implementação |
|---|---|
| Sucesso/commit | O atomic entra primeiro; o sinal entra depois e é restaurado antes da saída do atomic (`app/chamados/services/materializacao.py:1194`, `:1124`). |
| `_Reprovada`, exceção ou retorno antecipado | Todos atravessam o `finally` do contexto; a restauração ocorre antes do tratamento externo (`app/chamados/services/materializacao.py:1121–1124`, `:1201`, `:1257–1260`). |
| Savepoint interno desfeito e exceção absorvida | A P externa **continua em voo**, porque seu contexto externo ainda está ativo. O S143 tem precisamente savepoint interno com exceção absorvida (`app/chamados/signals.py:88–94`; `app/chamados/services/materializacao.py:1124`). |
| Outra P aninhada | A união mantém ambas; o token restaura o conjunto externo na saída (`app/chamados/services/materializacao.py:1120–1124`). |
| Mesma P aninhada | O token restaura a presença externa da mesma PK; não há remoção simples que a derrube (`app/chamados/services/materializacao.py:1120–1124`). |
| `on_commit` real depois do bloco | Encontra o sinal restaurado pela ordem dos managers (`app/chamados/services/materializacao.py:1194`, `:1124`; gancho em `app/chamados/signals.py:202`). |
| Outra thread/request | Não há armazenamento explícito do conjunto em variável global mutável ou thread-local: há `ContextVar` com conjunto imutável e reset. Comportamento efetivo entre requests/threads: **[nao verificado]** (`app/chamados/services/materializacao.py:1106`, `:1120–1124`). |

A garantia é **lexical**, não uma identidade de transação armazenada: o valor contém PKs, e a consulta verifica apenas se há atomic naquele momento (`app/chamados/services/materializacao.py:1111`, `:1120`, `:1129`). Uma cópia de contexto não seria invalidada pelo reset do contexto original; caminho concreto que faça essa cópia durante o voo: **[nao verificado]**.

Não encontrei sobrevivência no contexto corrente após saída normal, rollback ou exceção; afirmar ausência universal de sobrevivência em contexto transportado excederia o código demonstrado (`app/chamados/services/materializacao.py:1124`).

4. **Os testes mordem?** [nao verificado]

A tabela apresenta **mutantes previstos por leitura**, não mutantes executados. “Passa” abaixo é **[nao verificado]**; alguns sobrevivem apenas ao teste isolado e são mortos por outro teste do arquivo. [nao verificado]

| Teste | Mordida e mutante que pode escapar | [nao verificado]
|---|---|
| `UM_escritor`, `app/chamados/tests/test_pep_reentrada.py:83` | Conta carimbos bem-sucedidos e exige uma declaração. Uma segunda escrita crua da P por `save/update` escapa do spy, que observa somente `carimbar` (`:69–72`, `:90–92`). |
| `via_gravada`, `app/chamados/tests/test_pep_reentrada.py:94` | Verifica via/autor finais. Retirar a guarda S143 pode permitir escrita antecipada seguida de sobrescrita correta pelo materializador, preservando essas asserções (`:98–99`; `app/chamados/services/materializacao.py:1251`). |
| `S143_nao_fecha`, `app/chamados/tests/test_pep_reentrada.py:101` | Agora exige exatamente uma chamada fora de voo: ausência de fecho também falha. Desligar o receiver S143 ainda pode deixá-lo verde, pois não há prova positiva da passagem pelo receiver (`:108–117`; `app/chamados/signals.py:81`). |
| `DURANTE_o_plantio`, `app/chamados/tests/test_pep_reentrada.py:122` | Prova sinal na entrada do ramo e ausência no retorno. Restringir o contexto apenas ao ramo de plantio, retirando-o antes do julgamento/carimbo, preserva essas observações (`:126–134`; `app/chamados/services/materializacao.py:1207`, `:1251`). |
| `ROLLBACK`, `app/chamados/tests/test_pep_reentrada.py:136` | A v2 prova nascimento do sinal e desaparecimento da batida. Limpar apenas em `_Reprovada`, vazando no sucesso, passa neste teste isolado (`:141–151`). |
| `EXCECAO`, `app/chamados/tests/test_pep_reentrada.py:153` | Agora tem assert positivo. Limpar somente na exceção passa neste teste isolado (`:159–163`). |
| `aninhado`, `app/chamados/tests/test_pep_reentrada.py:165` | Cobre outra P e mesma P. Restaurar token somente após `yield`, sem `finally`, passa nas saídas normais testadas e falha nas excepcionais não presentes aqui (`:169–177`; `app/chamados/services/materializacao.py:1121–1124`). |
| `fora_de_voo`, `app/chamados/tests/test_pep_reentrada.py:189` | Controle positivo útil. `pergunta_em_voo` sempre falso passa neste teste isolado (`:193–196`). |
| `S143_pula`, `app/chamados/tests/test_pep_reentrada.py:198` | A irmã nova mata “pula todas” no primeiro laço. Retirar a guarda do laço sem credor escapa: ambas estão validadas, enquanto esse laço exige validação nula (`:207–215`; `app/chamados/services/disputa_emissao.py:1665–1671`). |
| `qualquer_fecho/S144`, `app/chamados/tests/test_pep_reentrada.py:219` | Trocar a guarda por “se existe qualquer voo, pule todas” preserva a única P pulada e o controle fora de voo. Falta irmã fora de voo no S144 (`:223–229`; `app/chamados/services/materializacao.py:1399`). |
| `fora_de_atomic`, `app/chamados/tests/test_pep_reentrada.py:234` | Exige recusa fora de atomic. Um contexto que sempre lance `RuntimeError` passa neste teste isolado (`:238–241`). |

**O teste S144 não é intrinsecamente vazio:** chama `fechar` real e exige depois que a mesma P seja carimbável. Porém, desligar o receiver S144 mantém o assert interno nulo e o controle direto posterior funcionando; logo ele não prova sozinho que houve tentativa pelo receiver (`app/chamados/tests/test_pep_reentrada.py:225–229`; `app/chamados/signals.py:130–140`).

**O spy da cauda resolveu a fraqueza do estado final**, mas observa apenas chamadas de `fechar`; um fecho antecipado por `update` não aparece nele. A cauda também usa a instância recebida sem atualização prévia (`app/chamados/tests/test_pep_reentrada.py:108–117`; `app/chamados/services/materializacao.py:1290–1307`).

Faltam demonstrações de commit real, rollback de savepoint interno, callback observando o sinal depois do bloco e isolamento entre threads/requests. Além disso, no teste S144 a captura de callbacks é o último manager e executa sua saída **antes** da saída do contexto de voo (`app/chamados/tests/test_pep_reentrada.py:36`, `:160–177`, `:224`).

5. **Triagem da volta 1**

- **S144 “procedente e curado”:** a guarda cura o escritor identificado, mas o selo ainda não prova receiver vivo nem poupa irmã fora de voo. “Curado no código” e “coberto integralmente pelo teste” não são equivalentes aqui (`app/chamados/services/materializacao.py:1399`; `app/chamados/tests/test_pep_reentrada.py:223–229`).
- **Multi-alias:** descarte sustentado pelas configurações da cópia, que declaram somente `default`; configuração efetivamente carregada em produção: **[nao verificado]** (`app/config/settings/saas.py:83–95`; `app/config/settings/prod.py:9–18`).
- **Contextos copiados:** aceitável descartar como defeito concreto não demonstrado; errado converter isso em prova de impossibilidade. O reset restaura apenas o contexto corrente; transporte concreto: **[nao verificado]** (`app/chamados/services/materializacao.py:1120–1124`).
- **Absorção / `:405` / `:586`:** os descartes têm fundamento no alcance encontrado: absorção vem da resposta explícita; `:405` barra texto, obrigatório no plantio de hora; `:586` exclui validada (`app/chamados/services/disputa_emissao.py:1330`; `app/chamados/reconciliador.py:364`, `:620`; `app/chamados/services/materializacao.py:804`).
- **Custo da cauda “menor”:** há leitura adicional por disputa, mas custo desprezível não foi medido no dado fornecido: **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1703–1710`).
- **Duplo fecho:** é possibilidade preexistente por instância desatualizada, não segundo escritor da P. O reconciliador fecha outra instância; a cauda e a porta consultam o estado em memória (`app/chamados/reconciliador.py:600–602`; `app/chamados/services/materializacao.py:1290`; `app/chamados/models.py:1820`). Ocorrência concreta: **[nao verificado]**.
- **JUIZES.md “observação”: descarte insuficiente.** A própria documentação manda corrigir divergência no mesmo commit; “disputa fecha … SEM juiz” contradiz o registro e a implementação de C7 (`app/docs/JUIZES.md:14`, `:54`; `app/core/juizes.py:267`; `app/chamados/models.py:1789–1794`).

6. **Classes de reprovação, uma a uma**

| Classe | Avaliação neste diff |
|---|---|
| Segundo juiz duplicando registrado | Não identificado: o predicado novo decide posse temporária; lastro, C6 e C7 continuam nas autoridades existentes (`app/chamados/services/materializacao.py:1129`, `:1207`; `app/chamados/models.py:2596`; `app/chamados/services/disputa_emissao.py:1712`; `app/core/juizes.py:266–267`). |
| Segundo escritor fora da porta | As adições não escrevem o trio: pulam concorrentes. Há escritores preexistentes de identidade e somente de via, que precisam permanecer distintos da afirmação de exclusividade da P

[truncado em 20000 de 21751 caracteres]

## 06/11 09:38 · sessao b8b07d486070 · - · - · origem=Ronald (tmux)

- **Quem escreve/lê a lista:** `falhas_do_universo` produz a lista contra o primeiro pai e a entrega diretamente a `julgar`; este chama `_julgar_linha`, cujo ramo `n/a` consulta `negocio`. A lista é local, sem arquivo ou protocolo externo: [bin/tests/test_lei_akita.sh:180](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_lei_akita.sh:180), [bin/tests/test_lei_akita.sh:141](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_lei_akita.sh:141), [bin/tests/test_lei_akita.sh:122](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_lei_akita.sh:122). A saída externa contém falhas e contadores, não a lista: [bin/tests/test_lei_akita.sh:317](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_lei_akita.sh:317).

- **Consumidores do veredito:** `bin/pre-push.sh` passa `PUSH_SHA` como `LEI_AKITA_HEAD` e interrompe no retorno não zero; a régua executa o mesmo script pelo glob `tests/test_*.sh`. Ambos recebem a alteração automaticamente, sem predicado duplicado nesses chamadores: [bin/pre-push.sh:33](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/pre-push.sh:33), [bin/pre-push.sh:38](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/pre-push.sh:38), [bin/regua.sh:138](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/regua.sh:138).

- **Hook previsto:** o instalador atual escreve `.git/hooks/pre-push` como wrapper com `exec bash "$(git rev-parse --show-toplevel)/bin/pre-push.sh" "$@"`; portanto a cadeia prevista usa o mesmo juiz: [bin/hooks.sh:49](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/hooks.sh:49). O conteúdo do hook efetivamente instalado é `[nao verificado]`.

- **Cópias antigas:** há `app/core/hooks/pre-push.sh`, reconhecido no próprio censo como migração que não pousou; esse arquivo não chama o selo e não contém outro predicado PADRAO-NA-CURA: [bin/tests/test_recursos_sourced.sh:26](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/bin/tests/test_recursos_sourced.sh:26), [app/core/hooks/pre-push.sh:6](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/app/core/hooks/pre-push.sh:6). Há também instalador antigo que copia o `bin/pre-push.sh`, mas a fonte continua sendo `bin/`: [app/core/hooks/hooks.sh:10](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/app/core/hooks/hooks.sh:10), [app/core/hooks/hooks.sh:35](/home/ronald/codex_ws/858b5d7adfc797252076767a10f8f2dd82f1b7db/app/core/hooks/hooks.sh:35).

- A busca por `test_lei_akita`, `PADRAO_FALHA`, `PADRAO_FIM`, `falhas_do_universo`, `_julgar_linha`, `LEI_AKITA_HEAD` e literais `n/a (instrumento)` encontrou o predicado somente neste selo; inexistência absoluta de outra implementação equivalente: `[nao verificado]`.

## 06/11 09:36 · sessao b2c4c9acd30d · LEI-FIO-MUDO-B · verificador · origem=Code

**Parecer: a remoção atende à cura literal, mas o R08f ainda está incompleto como selo.** Saíram a função e o agendamento de reabertura; os problemas encontrados estão na cobertura do teste. [app/chamados/models.py:516; app/chamados/services/fio.py:899; app/chamados/tests/test_c20_fio_mudo.py:283; app/chamados/tests/test_c20_fio_mudo.py:290]

**1. Errado ou incompleto na volta 2**

- **O POST fotografa os pushes antes dos callbacks.** `_no_post` é copiado dentro de `captureOnCommitCallbacks(execute=True)`, antes de sua saída. Um POST que agendasse `on_commit(lambda: push_colaborador(...))` escaparia da comparação: o callback executaria depois da fotografia. Os cinco alvos diferentes de `pushes[0]` ficam descobertos nesse intervalo; o primeiro ainda é conferido indiretamente pelo total final igual a 1. [app/chamados/tests/test_c20_fio_mudo.py:287; app/chamados/tests/test_c20_fio_mudo.py:290; app/chamados/tests/test_c20_fio_mudo.py:293; app/chamados/tests/test_c20_fio_mudo.py:298; app/chamados/tests/test_c20_fio_mudo.py:299]
- **“Corrigir” continua com asserção vazia para o ramo relevante.** A pergunta é `escolha`; o botão exige `hora`. Liberar indevidamente Corrigir para perguntas de hora em disputa fechada, mantendo Rejeitar oculto, não seria detectado. [app/chamados/tests/test_c20_fio_mudo.py:68; app/chamados/tests/test_c20_fio_mudo.py:283; app/templates/chamados/partials/modal_fio.html:284; app/templates/chamados/partials/modal_fio.html:295]
- **O POST novo não exercita a proteção `sem_reabrir` na recusa da materialização.** Esse parâmetro impede reabertura no ramo de resposta de hora recusada; a fixture `escolha` não entra nele. Retirar sua passagem em `fio.py:605` escaparia deste caso. [app/chamados/services/fio.py:605; app/chamados/services/validacao.py:152; app/chamados/services/validacao.py:199; app/chamados/services/validacao.py:422; app/chamados/services/validacao.py:427]

Para fechar essas lacunas, a observação dos seis mocks precisa ocorrer depois dos callbacks, separada da chamada artificial; Corrigir precisa de cenário `hora`; e a proteção contra reabertura no Validar precisa de cenário de hora cuja materialização seja recusada. [app/chamados/tests/test_c20_fio_mudo.py:287; app/chamados/tests/test_c20_fio_mudo.py:290; app/templates/chamados/partials/modal_fio.html:295; app/chamados/services/validacao.py:425]

**2. Descartes e causalidade do REATAR**

**Não encontrei descarte indevido pelo critério perguntado. Retifico a abrangência do parecer v1:** pertencer ao fluxo amplo de `abrir/reabrir` não demonstra ser efeito do REATAR. `reabrir` chama a ressuscitação antes da adoção. [app/chamados/models.py:1495; app/chamados/models.py:1496]

O caminho causal lido é: adoção → reapontamento/lavra → save da pergunta → callback de beco. A classificação do beco exclui imediatamente disputa FECHADA; a adoção também pode retratar duplicatas, sem chamar ressuscitação. [app/chamados/models.py:502; app/chamados/models.py:515; app/chamados/services/fio.py:845; app/chamados/services/fio.py:847; app/chamados/signals.py:202; app/chamados/services/becos.py:27]

- **HX-RESSUSCITA:** seleciona MATERIALIZADAS sem texto, descarimba e reabre suas disputas. Continua sendo ato distinto, anterior à adoção, e não reabertura causada pela reatada. [app/chamados/models.py:1528; app/chamados/models.py:1531; app/chamados/models.py:1598; app/chamados/models.py:1625]
- **F2:** diagnostica a viva não materializada em fechada; esse trecho não reabre nem envia push. Seu filtro também não depende do reapontamento. [app/chamados/management/commands/auditar_invariantes_chamados.py:1761] [nao verificado]
- **Reabertura administrativa:** exige ação própria, permissão, motivo e justificativa; procura a disputa pelo chamado-container, reabre e envia FCM. A adoção não chama essa ação. [app/chamados/services/fio.py:577; app/chamados/services/acoes_disputa.py:242; app/chamados/services/acoes_disputa.py:245; app/chamados/services/acoes_disputa.py:252; app/chamados/services/acoes_disputa.py:256; app/chamados/services/acoes_disputa.py:283]
- **RELATO:** a passagem da cópia já começa com `RESPONDIDA`; não sustento o apontamento anterior de decisão ainda pendente nessa passagem. [app/docs/RELATO.md:65]

Há fábrica posterior à abertura que pode notificar por outra disputa ABERTA. A exceção para dia passado com marcos nomeados independe da reatada e também funciona sem ela; não demonstra reabertura da fechada original nem push causado pelo REATAR. [app/ponto/management/commands/detectar_ausencias.py:790; app/chamados/services/disputa_emissao.py:658; app/chamados/services/disputa_emissao.py:876; app/chamados/services/disputa_emissao.py:925; app/chamados/services/disputa_emissao.py:935; app/chamados/services/disputa_emissao.py:1123]

**3. O que o R08f efetivamente morde**

| Parte | Alcance por leitura estática |
|---|---|
| Disputa | Exige pergunta viva, cobrança criada e destino reapontado; executa callbacks antes de comparar fechamento e resolução. Morde a volta do callback antigo nesse cenário respondido. [app/chamados/tests/test_c20_fio_mudo.py:255; app/chamados/tests/test_c20_fio_mudo.py:257; app/chamados/tests/test_c20_fio_mudo.py:262; app/chamados/tests/test_c20_fio_mudo.py:264] |
| Push no reatar | Corrigiu os guardas: patcha os wrappers e confere os seis alvos depois dos callbacks da adoção. [app/chamados/tests/test_c20_fio_mudo.py:221; app/chamados/tests/test_c20_fio_mudo.py:257; app/chamados/tests/test_c20_fio_mudo.py:266] |
| Card | Corrigido: exige a pergunta especificamente sob `ch.pk` e confere a flag de fechada. [app/chamados/tests/test_c20_fio_mudo.py:270; app/chamados/tests/test_c20_fio_mudo.py:272] |
| Modal | Confere presença do ID, presença de Validar e ausência de Rejeitar. ID e ação são procurados separadamente, sem conferir o mesmo formulário; Corrigir conserva a vacuidade descrita acima. [app/chamados/tests/test_c20_fio_mudo.py:279; app/chamados/tests/test_c20_fio_mudo.py:283] |
| POST Validar | Agora faz POST real e exige `validada_em` persistido, preservando fechamento/resolução após callbacks. Permanecem as lacunas de push tardio e de recusa de hora. [app/chamados/tests/test_c20_fio_mudo.py:288; app/chamados/tests/test_c20_fio_mudo.py:294; app/chamados/tests/test_c20_fio_mudo.py:298] |

**O “MORDE” do mock prova apenas que o import explícito do próprio teste recebe o primeiro símbolo patchado.** Ele chama diretamente esse mock; não exercita um chamador de produção nem dá controle positivo dos outros cinco alvos. [app/chamados/tests/test_c20_fio_mudo.py:230; app/chamados/tests/test_c20_fio_mudo.py:291; app/chamados/tests/test_c20_fio_mudo.py:299]

A generalização “todo sítio de produção importa localmente” também é inexata: `views_cobrar` importa FCM no nível do módulo e usa esse nome depois. Isso limita a promessa do comentário, sem demonstrar alcance desse outro ato pela adoção. [app/chamados/tests/test_c20_fio_mudo.py:218; app/chamados/views_cobrar.py:11; app/chamados/views_cobrar.py:44; app/chamados/models.py:502]

A variante **sem resposta** também permanece fora do R08f: a adoção não exige resposta, o teste a preenche, e a interface/validação atuais mostram “Aguardando resposta” e recusam validar sem `respondida_em`. A cobertura dessa variante pelo novo selo é ausente. [app/chamados/models.py:478; app/chamados/models.py:492; app/chamados/tests/test_c20_fio_mudo.py:252; app/templates/chamados/partials/modal_fio.html:314; app/chamados/services/validacao.py:363]

**4. Escritores e leitores adicionais da volta 2**

| Alvo adicional | Quem define/escreve | Quem lê/chama |
|---|---|---|
| `_CANOS_DE_PUSH` / `_push_gravado` | Tupla e montagem dos seis patches no teste. [app/chamados/tests/test_c20_fio_mudo.py:217; app/chamados/tests/test_c20_fio_mudo.py:230] | Blocos de adoção/POST e comparações das chamadas. [app/chamados/tests/test_c20_fio_mudo.py:256; app/chamados/tests/test_c20_fio_mudo.py:266; app/chamados/tests/test_c20_fio_mudo.py:286; app/chamados/tests/test_c20_fio_mudo.py:298] |
| Novo POST | Teste fornece ação e IDs. [app/chamados/tests/test_c20_fio_mudo.py:288] | View → `processar_post_fio` → `acao_validar_pergunta` → `validar_pergunta`. [app/chamados/views.py:951; app/chamados/services/fio.py:581; app/chamados/services/fio.py:605; app/chamados/services/acoes_disputa.py:18] |
| `sem_reabrir` agora exercitado pelo POST | Fio calcula pela disputa da pergunta e passa à casca. [app/chamados/services/fio.py:596; app/chamados/services/fio.py:605] | Casca encaminha; validação usa na devolução recusada. [app/chamados/services/acoes_disputa.py:22; app/chamados/services/validacao.py:427] |
| `validada_em` agora observado | No ato testado, `_carimbar_validacao` grava instante e autor. [app/chamados/services/validacao.py:127; app/chamados/services/validacao.py:133] | Novo assert, juiz de vida, estado do modelo, seleção do materializador e modal. [app/chamados/tests/test_c20_fio_mudo.py:296; app/chamados/juizes.py:1136; app/chamados/models.py:2593; app/chamados/services/materializacao.py:1296; app/templates/chamados/partials/modal_fio.html:223] |

`validada_em` possui outras escritas preexistentes: devolução recusada, nova resposta, reabertura para correção, ressuscitação e preenchimento pela porta `carimbar`. Portanto, “dono do ato de validar” não equivale a atribuição exclusiva da coluna; essas escritas não nasceram na volta 2. [app/chamados/services/validacao.py:188; app/chamados/services/disputa_emissao.py:1295; app/chamados/services/disputa_emissao.py:1393; app/chamados/models.py:1598; app/chamados/models.py:2686]

**5. Órfãos, segundo escritor e relógio**

Não encontrei chamador executável sobrevivente da função apagada: as referências restantes identificadas são lápides e registros documentais. [app/chamados/models.py:517; app/chamados/services/fio.py:899; app/docs/PROMPTS.md:1895; app/docs/RELATO.md:65; app/docs/BACKLOG.md:1778]

Não há novo escritor ou relógio de produção introduzido pela volta 2: as adições exercitam portas existentes; o teste mantém o relógio congelado e a validação usa seu carimbo existente. [app/chamados/tests/test_c20_fio_mudo.py:31; app/chamados/tests/test_c20_fio_mudo.py:269; app/chamados/tests/test_c20_fio_mudo.py:288; app/chamados/services/validacao.py:131]

Correspondência da cópia com HASH/HEAD, RED→GREEN, suite, sobrevivência experimental dos mutantes e efeitos efetivos em produção: **[nao verificado]**. [nao verificado]

## 06/11 09:36 · sessao b60541119bfe · - · - · origem=Ronald (tmux)

Censo de produção, excluídos testes/migrações: [nao verificado]

- `PerguntaDisputa.carimbar`: `materializacao.py:549`, `:1251`, `:1405`; `disputa_emissao.py:1612`, `:1686`; `reconciliador.py:405`, `:586`; wrapper `models.py:2685`. Os sítios S143/S144 pulam a P em voo em `disputa_emissao.py:1586`, `:1671` e `materializacao.py:1399`; o materializador escreve em `materializacao.py:1251`.
- `reconciliador.py:405` é alcançável sincronamente pela batida: `ponto/signals.py:215` → `reconciliador.py:1417` → `:1510` → `:1603`. Porém não alcança a P-hora corrente: exige ausência de texto em `reconciliador.py:364`, e o plantio de hora exige texto em `materializacao.py:804`. `MOTIVOS_C1` só contém dois motivos (`reconciliador.py:332`), ambos `hora` no catálogo (`catalogo/perguntas.py:48`, `:56`); escolha sem texto com motivo C1 seria dado fora desse catálogo [nao verificado].
- `reconciliador.py:586` também é síncrono, via `ponto/signals.py:215` → `reconciliador.py:1416`, e alcança irmãs, mas exclui a P validada pelo filtro `reconciliador.py:620`.
- A absorção `materializacao.py:549` vem da resposta explícita em `disputa_emissao.py:1330`; nenhum chamador desse responder foi encontrado nos receivers síncronos do plantio [nao verificado para alcance absoluto]. O fantasma `models.py:2685` vem de `reconciliador.py:1136`, chamado pelo consumidor EventoCelula em `reconciliador.py:1363`, com cron próprio em `config/crons.py:394`.

Escritores adicionais dos mesmos campos:

- Atribuição tríplice: `models.py:2558`–`:2560`; persistência por `carimbar` em `models.py:2609`, identidade em `:2644`, descarimbo em `:2652` e descarimbo em lote em `:2663`.
- Identidade: `models.py:2522` (`fechar_por_admin`); `materializacao.py:899` (paridade), `:1074` (colisão); `fio.py:891` (duplicata). Paridade e colisão podem escrever durante o bloco, mas são ramos do próprio materializador, por `carimbar_identidade`, preexistentes ao diff (`materializacao.py:899`, `:1074`, `:1194`).
- Só troca da via: `models.py:2673`–`:2674`, chamada pelo comando `encerrar_perguntas_com_fato.py:68`; `relavra_via.py:123`–`:126`, chamada pelo comando `relavrar_via_i6.py:102`. `relavrar_dia` delega em `relavra_via.py:155`, mas está sem chamador de produção [nao verificado por execução].
- Volta do carimbo: `models.py:2696` (fantasma), `:2713` (resposta), `:2720` (competência), `:1504` (ressuscitação); `disputa_emissao.py:1412` (desfazer absorção); `relavra_via.py:231` (carimbo sem lastro).
- O cartório pode atingir `relavra_via.py:231` durante o bloco por `cartorio.py:985` → `fabrica_da_celula.py:166`, mas a porta exige que C6 considere a pergunta viva (`relavra_via.py:227`); a P corrente está validada e C6 a considera morta [pai está verificando a referência exata do degrau].
- A ressuscitação exige carimbo presente e ausência de texto (`models.py:1434`–`:1438`); a migração/deduplicação exige C6 vivo (`fio.py:931`–`:936`), portanto não selecionam a P-hora validada corrente nesses estados (`materializacao.py:804`, `:1343`–`:1346`).

`DisputaSupervisao.fechar`, todos os sítios de produção:

| Sítio | Alcance durante voo |
|---|---|
| `materializacao.py:568` | Absorção da resposta explícita, chamador em `disputa_emissao.py:1330`; alcance pelo plantio não demonstrado [nao verificado]. |
| `materializacao.py:1307` | Cauda depois de `_materializar_uma`, portanto após saída do contexto (`materializacao.py:1350`, `:1352`). |
| `disputa_emissao.py:1713` | S143 síncrono, barrado quando há P em voo em `disputa_emissao.py:1710`. |
| `reconciliador.py:412` | Síncrono por irmãs via batida; P corrente com texto não passa por `reconciliador.py:364`. S144 acionado por esse fecho pula a P em `materializacao.py:1399`. |
| `reconciliador.py:602` | Síncrono por irmã absolvida; P corrente excluída por `reconciliador.py:620`; S144 pula a P em `materializacao.py:1399`. |
| `reconciliador.py:1160` | Consumidor EventoCelula (`reconciliador.py:1363`), cron separado (`config/crons.py:394`). |
| `becos.py:52` | Evento registrado em `chamados/signals.py:202`, por `on_commit`, portanto depois do bloco normal. |
| `disputa_emissao.py:1118` | Pertence a `criar_disputa_a_partir_de_chamados_orfaos`; fábrica chama outra porta, `criar_ou_atualizar_disputa` (`fabrica_da_celula.py:139`), não prova esse alcance [nao verificado]. |
| `competencia_trancada.py:204` | Porta da tranca, nenhum caminho síncrono pelo plantio demonstrado [nao verificado]. |
| `colaboradores/services/desligamento.py:142` | Porta de desligamento, nenhum caminho síncrono pelo plantio demonstrado [nao verificado]. |
| `ponto/services/entrada_sincronizada.py:59` | Chamadores API em `api/views.py:805`, `api/views_core.py:805`; nenhum caminho inverso do plantio demonstrado [nao verificado]. |
| `api/foto_recebida.py:80` | Chamadores API em `api/views.py:871`, `api/views_core.py:861`; nenhum caminho inverso do plantio demonstrado [nao verificado]. |
| `management/commands/encerrar_perguntas_com_fato.py:88` | Comando separado, sem chamada do plantio demonstrada [nao verificado]. |
| `models.py:1879` | Wrapper `admin_encerra`; sem chamada do plantio demonstrada [nao verificado]. |
| `models.py:1986` | Wrapper `remover_motivo`, associado a eventos de posto/âncora (`models.py:1911`–`:1913`); sem chamada do plantio demonstrada [nao verificado]. |

O fechamento efetivo escreve `fechada_em`/`resolucao` e salva ambos em `models.py:1861`–`:1865`. Reaberturas zeram ambos em `models.py:2063`–`:2075` e `:2087`–`:2091`; limpeza residual só zera resolução em `models.py:2173`–`:2174`. Não encontrei outro escritor de produção que ponha `fechada_em` preenchida [nao verificado por execução].

Correção relevante da hipótese do usuário: S144 **não** roda apenas uma vez por fecho; o receiver aceita qualquer `post_save` de disputa já fechada, porque a guarda verifica somente `raw`, `created` e ausência de `fechada_em` (`chamados/signals.py:130`). O único chamador direto de produção encontrado de `materializar_pendentes_aprovadas` é esse receiver (`chamados/signals.py:139`–`:140`).

## 06/11 09:35 · sessao e8e99bc140d8 · - · - · origem=Ronald (tmux)

Auditoria estática, sem execução dos testes; resultados reais de verde/vermelho e dos mutantes são [nao verificado]. Abaixo, “sobrevive” significa previsão pela leitura das asserções citadas. [nao verificado]

**Sinal e transações**

- `_EM_VOO` é um objeto `ContextVar` de módulo; seu valor é um conjunto imutável de PKs. A entrada exige `connection.in_atomic_block`, une a PK ao conjunto e guarda token; a saída sempre restaura o token em `finally` — não usa `clear()` que quebraria a reentrada da mesma P (`app/chamados/services/materializacao.py:1106`, `:1118`, `:1120`, `:1124`).
- No uso produtivo, o atomic entra primeiro e o sinal depois; na saída, o sinal é restaurado **antes** de o atomic confirmar ou desfazer. Isso cobre sucesso, `_Reprovada`, exceção e retorno antecipado (`app/chamados/services/materializacao.py:1194`, `:1201`, `:1213`, `:1253`, `:1257`).
- Savepoint interno desfeito, com exceção absorvida dentro do bloco externo, deve preservar a P externa em voo: o manager externo ainda não saiu. O S143 possui precisamente atomic/savepoint interno com exceção absorvida; não altera `_EM_VOO` (`app/chamados/signals.py:88`, `:91`, `:93`; `app/chamados/services/materializacao.py:1124`). Falta teste desse caso (`app/chamados/tests/test_pep_reentrada.py:136`, `:153`, `:165`).
- Aninhamento de P diferente preserva a anterior; mesma P usa token para restaurar o conjunto anterior. Há asserções positivas e negativas para ambos, mas só com saída normal (`app/chamados/tests/test_pep_reentrada.py:169`, `:171`, `:173`, `:174`, `:176`, `:177`).
- `on_commit` realmente executado depois da saída de `_materializar_uma` encontra o sinal restaurado, pela ordem dos managers (`app/chamados/services/materializacao.py:1194`, `:1124`). **Os testes não demonstram esse caso**: `_Base` herda `TestCase`, e `captureOnCommitCallbacks(execute=True)` é o último manager em `:224`, portanto a execução capturada ocorre antes de sair do manager do sinal (`app/chamados/tests/test_pep_reentrada.py:36`, `:224`).
- O teste da materialização também executa callbacks capturados, mas só verifica o sinal na função de plantio e no retorno, não dentro de callback posterior ao bloco (`app/chamados/tests/test_pep_reentrada.py:74`, `:122`, `:129`, `:134`).
- Não há identidade de transação/token de conexão no valor: apenas PK e a pergunta “há atomic agora?”; a garantia vem do escopo lexical e do reset (`app/chamados/services/materializacao.py:1111`, `:1120`, `:1129`). Migração de contexto para outra thread/request não está exercitada: [nao verificado]. A imagem declara Python 3.13 (`app/Dockerfile:1`).
- Multi-alias não é bug demonstrado nesta configuração: `saas.py` declara somente `default`, com backend django-tenants e router (`app/config/settings/saas.py:83`, `:84`, `:85`, `:93`). Troca de schema com sinal ativo não foi demonstrada: [nao verificado].

**Teste S144: útil, mas não suficiente** [nao verificado]

- Não considero o teste `qualquer_fecho...` intrinsecamente vazio: ele cria batida real, mantém a P sem carimbo, chama `d.fechar` real e exige que, fora do voo, a **mesma função** carimbe exatamente aquela P (`app/chamados/tests/test_pep_reentrada.py:179`, `:181`, `:186`, `:225`, `:227`, `:228`).
- O caminho do receiver é síncrono: `fechar` salva `fechada_em`, e o receiver chama imediatamente `materializar_pendentes_aprovadas`; não é o callback capturado (`app/chamados/models.py:1861`, `:1865`; `app/chamados/signals.py:124`, `:139`, `:140`).
- A P está validada no fixture; o C6 a considera morta imediatamente por `validada_em`. Logo o fecho normal não é barrado por pergunta viva e `carimbar` também aceita a P validada (`app/chamados/tests/test_pep_reentrada.py:59`, `:60`; `app/chamados/juizes.py:1135`, `:1136`, `:1137`; `app/chamados/models.py:2596`, `:2609`).
- Mutante sobrevivente grave: receiver S144 retornar sempre em `signals.py:130`. O assert interno continua vendo NULL e o controle final chama a função diretamente, portanto não prova que o receiver foi alcançado (`app/chamados/signals.py:130`; `app/chamados/tests/test_pep_reentrada.py:225`, `:227`, `:228`).
- Outro mutante sobrevivente: trocar `if pergunta_em_voo(p.pk)` por `if bool(_EM_VOO.get())` no S144. A única P do teste continua pulada; fora do voo continua carimbada. Falta uma irmã **fora** de voo no mesmo S144, como a irmã já adicionada para S143 (`app/chamados/services/materializacao.py:1399`; `app/chamados/tests/test_pep_reentrada.py:223`, `:228`, `:204`, `:214`).

**Teste que espia a cauda** [nao verificado]

- Ficou mais forte: registra cada chamada de `DisputaSupervisao.fechar` antes da execução e exige exatamente uma, com conjunto vazio; não depende apenas da resolução final (`app/chamados/tests/test_pep_reentrada.py:108`, `:109`, `:113`, `:115`, `:117`).
- Também falha se nenhuma chamada ocorrer; portanto não é vazio por lista vazia (`app/chamados/tests/test_pep_reentrada.py:113`).
- Continua observando apenas a porta `fechar`. Mutante que escrevesse cedo `fechada_em/resolucao` via `QuerySet.update` e deixasse a instância original do materializador chamar `fechar` ao final poderia escapar do spy; a instância da cauda não recebe `refresh_from_db` (`app/chamados/tests/test_pep_reentrada.py:111`; `app/chamados/services/materializacao.py:1290`, `:1307`).
- Mutante mais simples: desativar o receiver S143. O destino ainda pode terminar fechado, a única chamada final permanece fora de voo e o teste continua verde; não existe assert de que o S143 foi realmente chamado dentro do plantio (`app/chamados/tests/test_pep_reentrada.py:101`, `:112`, `:113`; `app/chamados/signals.py:81`).

**Um mutante sobrevivente por teste** [nao verificado]

| Teste | O que observa / mutante que pode escapar | [nao verificado]
|---|---|
| `:83 UM_escritor` | Conta somente chamadas **bem-sucedidas de `carimbar`** e via. Uma segunda escrita crua `save/update` da P durante S143 não entra em `gravados`; o carimbo posterior do materializador deixa a lista esperada (`:69-72`, `:90-92`). |
| `:94 via_gravada` | Remover a guarda S143 e permitir carimbo antecipado seguido de sobrescrita pelo materializador pode deixar via/autor finais corretos; não conta escritas (`:98-99`; materializador `:1251`). |
| `:101 cauda` | Desativar S143 preserva única chamada final fora de voo; falta assert positivo de passagem pelo receiver (`:108-117`; signals `:81`). |
| `:122 durante/saída` | Reduzir o manager do sinal à chamada `_materializar_resposta_hora`, retirando-o antes das etapas posteriores/carimbo, ainda produz `[True]` e `False` ao final; não espia a duração inteira (`:126-134`; materializador `:1207`, `:1251`). |
| `:136 rollback` | Resetar apenas no caminho `_Reprovada` e vazar no sucesso ainda satisfaz este teste isolado; agora ele prova nascimento do sinal e rollback da batida, corrigindo a fraqueza da v1 (`:141-151`). | [nao verificado]
| `:153 exceção` | Resetar somente quando há exceção ainda passa este teste; não exige limpeza na saída normal (`:159-163`). | [nao verificado]
| `:165 aninhamento` | Retirar `finally` e restaurar token só após `yield` na saída normal passa este teste; rollback/exceção aninhados não estão presentes (`:169-177`; manager `:1121-1124`). | [nao verificado]
| `:189 fora_de_voo` | Retornar sempre `False` em `pergunta_em_voo` mantém esse controle verde; ele só exige carimbo normal (`:193-196`). | [nao verificado]
| `:198 S143_pula` | Retirar somente a guarda do laço sem credor (`disputa_emissao.py:1671`) escapa: ambas as perguntas do fixture estão validadas e esse laço filtra `validada_em is None` (`test :207-208`; serviço `:1665-1666`). |
| `:219 S144` | Guarda global “se existe qualquer voo, pula todas” sobrevive; falta irmã fora de voo no mesmo S144 (`materializacao.py:1399`; teste `:223-229`). | [nao verificado]
| `:234 fora_atomic` | Manager que sempre lance `RuntimeError`, mesmo em atomic, passa este teste isolado; ele só exerce recusa (`:238-241`). | [nao verificado]

Os mutantes das linhas `:136`, `:153`, `:165`, `:189`, `:234` seriam mortos por outros testes do arquivo; não os apresentaria como buracos da **suíte inteira**. Os buracos coletivos mais claros são: escrita crua invisível aos spies, receiver não demonstrado, S144 “pula todas”, ausência de caso sem credor não validado em voo e ausência dos cenários savepoint/on_commit/thread (`app/chamados/tests/test_pep_reentrada.py:69`, `:111`, `:204`, `:224`, `:228`; `app/chamados/services/disputa_emissao.py:1665`; `app/chamados/services/materializacao.py:1399`).

## 06/11 09:35 · sessao 690d0fbfa393 · - · - · origem=Ronald (tmux)

- **S144 fora do receiver:** não há outro chamador de produção encontrado; a chamada está em `app/chamados/signals.py:139-140`. Há chamadas diretas em testes: `app/chamados/tests/test_porta_desfecho_pergunta.py:226`, `:244`, `:248`, `:263`, `:292`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:691`; `app/chamados/tests/test_pep_reentrada.py:228`.
- **Não roda estritamente “uma vez por fecho”:** o receiver aceita qualquer `post_save` de disputa já fechada, excluindo somente `raw`, criação e ausência de `fechada_em`; não compara estado anterior nem `update_fields`. Uma nova gravação da disputa fechada pode acioná-lo novamente. `app/chamados/signals.py:124-140`.
- **A guarda não agenda recuperação:** apenas dá `continue`; nenhuma callback ou fila nasce nesse trecho. `app/chamados/services/materializacao.py:1399-1400`.
- **No sucesso normal**, o próprio materializador carimba antes de sair do sinal, com `VIA_DECLARACAO` quando houve declaração; sem declaração, deriva a via por `via_pelo_fato`, ou usa `VIA_CHAMADO_ENCERRADO` sem destino. `app/chamados/services/materializacao.py:1235-1255`.
- **O exemplo “fechou no meio, S144 pulou, materializador reprovou” não deixa aquele fecho confirmado:** reprovação do lastro ou da porta levanta `_Reprovada` dentro do mesmo `atomic` que contém o plantio e os signals síncronos; o fecho ocorrido ali também volta. `app/chamados/services/materializacao.py:1194`, `:1207-1217`, `:1251-1258`.
- **Se o fato aprovado já existia antes**, ele pode persistir ao rollback; a cauda roda depois da saída do sinal, consulta C7 e pode fechar a disputa, fazendo S144 carimbar por `VIA_JUIZ_APROVOU`. Isso depende de C7 e do lastro aprovarem. `app/chamados/services/materializacao.py:1287-1307`, `:1352-1353`, `:1401-1405`.
- **Não há garantia demonstrada de carimbo eventual para toda recusa:** recusas não terminais ficam pendentes com motivo; novas tentativas existem pelas ações de regularização, pelo fio e pela validação, mas sua execução futura é [nao verificado]. `app/chamados/services/materializacao.py:1058-1062`, `:1076-1078`; `app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/fio.py:666`; `app/chamados/services/validacao.py:430`.
- **A validação humana tem recuperação própria:** depois da unidade recusada, devolve validação ao admin e pode reabrir disputa fechada; não depende de um carimbo posterior do S144 para esconder a recusa. `app/chamados/services/validacao.py:424-427`, `:155-200`.

Triagem:

- **Multi-alias:** descarte sustentado pelas configurações da cópia: SaaS declara somente `default`, backend tenant e `TenantSyncRouter`; produção convencional também declara só `default`. Configuração efetivamente carregada em produção: [nao verificado]. `app/config/settings/saas.py:83-95`; `app/config/settings/prod.py:9-18`.
- **Duplo fecho:** permanece possibilidade estrutural por instância desatualizada: reconciliador fecha uma instância; cauda do materializador verifica a instância recebida sem `refresh_from_db`; a porta também checa `self.fechada_em` em memória. Isso antecede as linhas alteradas nos anexos; execução concreta em produção: [nao verificado]. `app/chamados/reconciliador.py:598-604`; `app/chamados/services/materializacao.py:1290-1307`; `app/chamados/models.py:1820-1824`.
- **JUIZES.md:** “só observação” é insuficiente perante a própria regra documental: a tabela diz “disputa fecha … SEM juiz”, mas C7 está registrado e implementado, e o documento exige corrigir divergência no mesmo commit. A divergência é preexistente; não prova juiz duplicado introduzido pela obra. `app/docs/JUIZES.md:14`, `:54`; `app/core/juizes.py:267`; `app/chamados/models.py:1789-1794`.

Classes no diff:

- **Segundo juiz:** a consulta nova responde posse temporária da escrita; os julgamentos de lastro/C6/C7 continuam nas autoridades existentes. `app/chamados/services/materializacao.py:1127-1129`, `:1207`, `:1251`; `app/chamados/services/disputa_emissao.py:1710-1713`.
- **Relógio:** as adições não introduzem leitura de hora; os instantes de escrita permanecem nos pontos já existentes. `app/chamados/services/materializacao.py:1218`, `:1390`; `app/chamados/services/disputa_emissao.py:1669`.
- **Ordem de trava:** nenhuma nova aquisição explícita de lock aparece nas adições; a cauda acrescenta leitura de perguntas. Ausência de deadlock em execução concorrente: [nao verificado]. `app/chamados/services/disputa_emissao.py:1710`; `app/chamados/services/materializacao.py:1118-1124`, `:1399-1400`.
- **Pouso antes do pré-requisito:** o contexto verifica transação aberta, é instalado após entrar no `atomic` e antes dos ramos de plantio. `app/chamados/services/materializacao.py:1118-1122`, `:1194-1200`.
- **Estado de módulo:** há objeto `ContextVar` no módulo, com valor inicial imutável e restauração por token; não é prova de sobrevivência do valor após o bloco. Contextos copiados/thread herdada exigem análise separada. `app/chamados/services/materializacao.py:1106`, `:1120-1124`.

## 06/11 09:34 · sessao bcd698cc91fd · - · - · origem=Ronald (tmux)

Achado concreto introduzido no teste: `_no_post` copia as chamadas **antes da execução dos callbacks de commit**. A captura ocorre na linha 290, dentro do `captureOnCommitCallbacks(execute=True)` iniciado em 287; o contexto só termina após a chamada artificial em 293. Assim, um push Web Push agendado no POST escapa da comparação em 298. O FCM principal fica parcialmente protegido pelo `len(...)=1` em 299, porque a chamada artificial acrescenta uma chamada nesse alvo. [app/chamados/tests/test_c20_fio_mudo.py:287; app/chamados/tests/test_c20_fio_mudo.py:290; app/chamados/tests/test_c20_fio_mudo.py:293; app/chamados/tests/test_c20_fio_mudo.py:298; app/chamados/tests/test_c20_fio_mudo.py:299]

O controle “MORDE” importa o símbolo já substituído e chama diretamente o mock. Prova que o endereço patchado responde ao import local; não demonstra passagem por um chamador real de produção, nem controle positivo dos outros cinco alvos. [app/chamados/tests/test_c20_fio_mudo.py:221; app/chamados/tests/test_c20_fio_mudo.py:224; app/chamados/tests/test_c20_fio_mudo.py:230; app/chamados/tests/test_c20_fio_mudo.py:291]

Censo adicional do delta da volta 2:

| Alvo | Quem escreve/define | Quem lê/chama |
|---|---|---|
| `_CANOS_DE_PUSH` | Tupla de seis endereços no teste. [app/chamados/tests/test_c20_fio_mudo.py:217] | `_push_gravado` cria os patches; comparações usam seu tamanho. [app/chamados/tests/test_c20_fio_mudo.py:230; app/chamados/tests/test_c20_fio_mudo.py:266; app/chamados/tests/test_c20_fio_mudo.py:298] |
| `_push_gravado` | Retorna `ExitStack` e mocks. [app/chamados/tests/test_c20_fio_mudo.py:226] | Adoção e POST Validar. [app/chamados/tests/test_c20_fio_mudo.py:256; app/chamados/tests/test_c20_fio_mudo.py:286] |
| Novo POST `validar_pergunta` | O teste fornece ação e IDs. [app/chamados/tests/test_c20_fio_mudo.py:288] | View delega a `processar_post_fio`; este resolve pergunta, listagem e disputa e delega com `sem_reabrir`. [app/chamados/views.py:951; app/chamados/services/fio.py:581; app/chamados/services/fio.py:594; app/chamados/services/fio.py:605] |
| `validada_em` agora observado | O ato continua em `_carimbar_validacao`, que salva timestamp/autor; a casca encaminha à porta existente. [app/chamados/services/validacao.py:127; app/chamados/services/acoes_disputa.py:18; app/chamados/services/validacao.py:396] | Novo assert; juiz considera validada morta; propriedade traduz estado; materializador seleciona validadas/respondidas; modal apresenta “Validada”. [app/chamados/tests/test_c20_fio_mudo.py:296; app/chamados/juizes.py:1136; app/chamados/models.py:2593; app/chamados/services/materializacao.py:1294; app/templates/chamados/partials/modal_fio.html:223] |
| Wrappers FCM/Web Push | FCM verifica token e chama `push_fcm`; `push_colaborador` verifica usuário e chama `_enviar_push_usuario`; `push_web_colaborador` verifica assinaturas e chama `pywebpush.webpush`. [app/chamados/fcm_utils.py:101; app/chamados/push_utils.py:114; app/ponto/webpush.py:24] | Os novos patches interceptam wrappers antes desses guardas e os níveis inferiores. [app/chamados/tests/test_c20_fio_mudo.py:221; app/chamados/tests/test_c20_fio_mudo.py:224] |

`validada_em` já tem portas anteriores de preenchimento por simetria, limpeza e reversão; o novo POST apenas exercita a validação existente. Exemplos persistentes: carimbo de validação, devolução recusada, `carimbar(resposta='validar')` e descarimbo em lote. Isso não constitui segundo escritor introduzido pela volta 2. [app/chamados/services/validacao.py:131; app/chamados/services/validacao.py:188; app/chamados/models.py:2686; app/chamados/models.py:2703; app/chamados/models.py:1598]

A função removida não conserva referência executável encontrada: restam lápides nos dois sítios e menções documentais em PROMPTS, BACKLOG e RELATO. O RELATO da cópia já prefixa a passagem histórica com `RESPONDIDA`; portanto, o apontamento de “topo ainda pendente” da v1 não corresponde à passagem atual. [app/chamados/models.py:517; app/chamados/services/fio.py:899; app/docs/PROMPTS.md:1895; app/docs/BACKLOG.md:1778; app/docs/RELATO.md:65]

Nenhum relógio ou escritor de produção novo aparece no delta volta1→2 fornecido: as adições ficam no teste; seus instantes usam o congelamento existente e o POST chama a porta existente, cujo relógio permanece `_carimbar_validacao`. [app/chamados/tests/test_c20_fio_mudo.py:269; app/chamados/tests/test_c20_fio_mudo.py:287; app/chamados/services/validacao.py:131]

Execução dos testes, sobrevivência experimental dos mutantes e efeitos em produção: [nao verificado]. [nao verificado]

## 06/11 09:34 · sessao c5ab57b1181c · - · - · origem=Ronald (tmux)

**Q2: não encontrei descarte errado segundo o critério causal pedido.** A adoção chama `reapontar_resolvedora`, registra o evento e retrata duplicatas; não chama ressuscitação ou reabertura. O reapontamento salva destino e lavra; o signal da pergunta agenda fechamento de beco, cuja classificação retorna imediatamente quando a disputa já está FECHADA. [app/chamados/models.py:502; app/chamados/models.py:507; app/chamados/models.py:515; app/chamados/services/fio.py:845; app/chamados/services/fio.py:847; app/chamados/signals.py:191; app/chamados/signals.py:202; app/chamados/services/becos.py:27]

- **HX-RESSUSCITA:** `reabrir` chama `ressuscitar_perguntas` **antes** da adoção. A ressuscitação seleciona perguntas MATERIALIZADAS e sem texto, descarimba e reabre suas disputas pela porta do sistema. É outro ato, preexistente e independente de executar o REATAR subsequente. [app/chamados/models.py:1495; app/chamados/models.py:1496; app/chamados/models.py:1528; app/chamados/models.py:1531; app/chamados/models.py:1598; app/chamados/models.py:1625]
- **Cartório:** ao encontrar perguntas e fio vivo, pula a ressuscitação; `fio_vivo` é `any(pergunta_viva(...))`. Assim, a reatada ainda viva pelo C6 não causa ressuscitação nesse bloco. [app/ponto/services/cartorio.py:1594; app/ponto/services/cartorio.py:1595; app/ponto/services/cartorio.py:1622; app/chamados/juizes.py:1242]
- **F2:** o trecho descartado apenas agrega IDs ao diagnóstico; não chama reabertura ou push. Seu filtro já alcança viva não materializada em fechada independentemente do destino estar reatado. [app/chamados/management/commands/auditar_invariantes_chamados.py:1761] [nao verificado]
- **Reabertura administrativa:** exige ação HTTP, administrador, motivo válido e justificativa; encontra a disputa pelo próprio chamado-container, reabre e envia FCM. Não é chamada pela adoção. [app/chamados/services/fio.py:577; app/chamados/services/acoes_disputa.py:242; app/chamados/services/acoes_disputa.py:245; app/chamados/services/acoes_disputa.py:252; app/chamados/services/acoes_disputa.py:256; app/chamados/services/acoes_disputa.py:283]

**Ressalva causal da fábrica posterior:** o detector chama `criar_disputa_a_partir_de_chamados_orfaos` depois da abertura, com `notificar=True` por default. Para pergunta reatada não materializada de dia atual, o filtro elimina o chamado; para dia passado com marcos nomeados, a exceção `cobra_marcos_da_ata` permite processamento mesmo com pergunta existente. A fábrica então seleciona/cria uma disputa ABERTA e pode enviar push. Essa exceção depende do chamado/data/marcos, não da reatada; também existe sem ela, e não reabre a disputa FECHADA original. [app/ponto/management/commands/detectar_ausencias.py:790; app/ponto/management/commands/detectar_ausencias.py:801; app/chamados/services/disputa_emissao.py:876; app/chamados/services/disputa_emissao.py:658; app/chamados/services/disputa_emissao.py:680; app/chamados/services/disputa_emissao.py:925; app/chamados/services/disputa_emissao.py:935; app/chamados/services/disputa_emissao.py:962; app/chamados/services/disputa_emissao.py:1123]

A primeira passagem da fábrica também pode complementar perguntas usando a disputa FECHADA encontrada pela pergunta reatada; os atos lidos criam/deduplicam perguntas e salvam motivos, sem reabrir a disputa nem enviar push nesse ramo. [app/chamados/services/disputa_emissao.py:909; app/chamados/services/disputa_emissao.py:920; app/chamados/services/disputa_emissao.py:923; app/chamados/services/disputa_emissao.py:756; app/chamados/services/disputa_emissao.py:770; app/chamados/models.py:2525; app/chamados/models.py:2568]

**Cron posterior:** o lembrete diário seleciona somente disputas ABERTAS. A reatada cuja disputa permanece FECHADA não habilita esse lembrete; uma reabertura por outro ato pode habilitá-lo. [app/ponto/management/commands/alertar_disputas_pendentes.py:33; app/ponto/management/commands/alertar_disputas_pendentes.py:59]

Execução dos caminhos, testes, callbacks e efeitos efetivos em produção: **[nao verificado]**. [nao verificado]

## 06/11 09:34 · sessao e07c2466b6fa · - · - · origem=Ronald (tmux)

Achados no R08f, por leitura estática; execução e sobrevivência experimental de mutantes: **[nao verificado]**.

1. **O POST perde pushes de callbacks em cinco dos seis alvos.** `_no_post` copia as listas **dentro** do `with`, antes da saída que executa `captureOnCommitCallbacks(execute=True)`. A comparação posterior usa essa cópia antiga. Um callback que chame `pushes[1]` a `pushes[5]` escaparia dessa asserção; o primeiro alvo FCM ainda é verificado indiretamente pelo total final igual a 1, após a chamada de controle. [app/chamados/tests/test_c20_fio_mudo.py:287; app/chamados/tests/test_c20_fio_mudo.py:290; app/chamados/tests/test_c20_fio_mudo.py:293; app/chamados/tests/test_c20_fio_mudo.py:298; app/chamados/tests/test_c20_fio_mudo.py:299]

2. **A ausência de Corrigir continua passando vazia para o ramo relevante.** A fixture nasce com `tipo_resposta='escolha'`; Corrigir exige `tipo_resposta == 'hora'`, além da guarda de disputa aberta. A nova `assertNotIn` não detectaria uma alteração que liberasse Corrigir para perguntas de hora em disputa fechada, preservando Rejeitar oculto. [app/chamados/tests/test_c20_fio_mudo.py:65; app/chamados/tests/test_c20_fio_mudo.py:68; app/chamados/tests/test_c20_fio_mudo.py:283; app/templates/chamados/partials/modal_fio.html:284; app/templates/chamados/partials/modal_fio.html:295; app/templates/chamados/partials/modal_fio.html:298]

3. **O POST é real, mas ainda não morde a proteção `sem_reabrir` na recusa de materialização.** O teste passa pela porta HTTP e exige `validada_em` preenchida. Entretanto, sua pergunta `escolha` não entra em `_resposta_de_hora`, que exige `hora`; somente esse ramo chama `devolver_validacao_recusada(..., reabrir=not sem_reabrir)`. Portanto retirar a passagem de `sem_reabrir` em `fio.py:605` não é detectado por esse caso. [app/chamados/tests/test_c20_fio_mudo.py:288; app/chamados/tests/test_c20_fio_mudo.py:296; app/chamados/services/validacao.py:143; app/chamados/services/validacao.py:152; app/chamados/services/validacao.py:422; app/chamados/services/validacao.py:425; app/chamados/services/fio.py:605]

4. **O “MORDE” do mock prova somente a interceptação da chamada explícita do teste ao primeiro alvo.** O próprio teste importa o símbolo já patchado e o chama; isso confirma que esse import recebe o mock, mas não exercita um chamador de produção nem certifica os outros cinco alvos. Também é falsa a generalização de que todos os chamadores importam dentro da função: `views_cobrar` importa `push_fcm_colaborador` no nível do módulo e depois usa esse alias. O alcance desse caminho pela adoção é **[nao verificado]** nesta exploração. [app/chamados/tests/test_c20_fio_mudo.py:218; app/chamados/tests/test_c20_fio_mudo.py:230; app/chamados/tests/test_c20_fio_mudo.py:291; app/chamados/tests/test_c20_fio_mudo.py:299; app/chamados/views_cobrar.py:11; app/chamados/views_cobrar.py:44]

O que efetivamente melhorou:

| Parte | Avaliação |
|---|---|
| Disputa no reatar | Exige pergunta viva, cobrança criada e destino reapontado; executa callbacks antes de conferir fechamento e resolução. Morde a volta do callback antigo nesse cenário respondido. [app/chamados/tests/test_c20_fio_mudo.py:255; app/chamados/tests/test_c20_fio_mudo.py:257; app/chamados/tests/test_c20_fio_mudo.py:260; app/chamados/tests/test_c20_fio_mudo.py:262; app/chamados/tests/test_c20_fio_mudo.py:264] |
| Push no reatar | Agora patcha wrappers antes dos guardas de token/usuário e confere todos os alvos depois dos callbacks. Corrige o vazio anterior para chamadas que resolvam esses símbolos patchados. [app/chamados/tests/test_c20_fio_mudo.py:221; app/chamados/tests/test_c20_fio_mudo.py:257; app/chamados/tests/test_c20_fio_mudo.py:266; app/chamados/fcm_utils.py:104; app/chamados/push_utils.py:116] |
| Card | Agora exige a pergunta sob `ch.pk`, além da flag de disputa fechada; não depende apenas da presença global no fio. [app/chamados/tests/test_c20_fio_mudo.py:270; app/chamados/tests/test_c20_fio_mudo.py:272] |
| Modal | Exige o ID da pergunta e presença de Validar; proíbe Rejeitar globalmente. O ID e Validar são buscados separadamente no HTML, sem comprovar que pertencem ao mesmo formulário; Corrigir conserva a lacuna acima. [app/chamados/tests/test_c20_fio_mudo.py:279; app/chamados/tests/test_c20_fio_mudo.py:283; app/templates/chamados/partials/modal_fio.html:274; app/templates/chamados/partials/modal_fio.html:278] |
| POST Validar | Exercita a ação e comprova mudança persistida em `validada_em`, preservando fechamento/resolução após callbacks. A cobertura de push e da recusa de hora tem as limitações acima. [app/chamados/tests/test_c20_fio_mudo.py:288; app/chamados/tests/test_c20_fio_mudo.py:294; app/chamados/tests/test_c20_fio_mudo.py:297] |

O código produtivo consultado já guarda os dois verbos: o template oculta Rejeitar/Corrigir quando `em_disputa_fechada`; o backend busca essas ações somente em disputa aberta. Validar aceita a pergunta viva em fechada e passa `sem_reabrir=True` à porta canônica. Portanto os achados acima são lacunas de certificação do novo teste, sem demonstração de violação produtiva nesses trechos. [app/templates/chamados/partials/modal_fio.html:284; app/chamados/services/fio.py:596; app/chamados/services/fio.py:602; app/chamados/services/fio.py:605; app/chamados/services/fio.py:629]

## 06/11 09:28 · sessao 61eb5622041b · E-SEM-VIVA-CACA · sonda · origem=Code

**O dado permite explicar os bloqueios atuais do col574 e apontar uma diferença de universo para o col950; não permite provar uma causa histórica comum aos 21 novos.** Os valores de produção abaixo vêm exclusivamente do texto fornecido, sem conferência independente: `[nao verificado]`.

**1. A sonda usa as chamadas reais?**

**Não é possível certificar os argumentos da sonda:** o texto contém a saída, mas não o conteúdo de `sonda_esemviva.py`; esse script também não foi localizado na cópia examinada. Portanto, divergências concretas **script × código** permanecem `[nao verificado]`.

Este é o contrato real contra o qual conferir o script:

| Função | Assinatura e chamada do sistema |
|---|---|
| `censo_alcance` | Assinatura `(empresas=None, hoje=None, limite=50, lista=None)`. O comando chama `censo_alcance(empresas, hoje=hoje, lista=lista)`, dentro de `tranca_em_lote()`. `app/chamados/services/vigia_fio.py:542`; `app/chamados/management/commands/vigiar_fio_morto.py:171-173`. |
| `ListaDoAdmin` | Recebe `(hoje, empresas=None, *, agora=None)`. O comando fornece o mesmo instante da rodada: `ListaDoAdmin(hoje, empresas, agora=agora)`. `app/chamados/services/vigia_fio.py:199`; `app/chamados/management/commands/vigiar_fio_morto.py:163-167`. |
| `motivos_retencao_celula` | Recebe `(colab, ini, fim, hoje=None, com_dias=False)`. O leitor de dias retidos usa `com_dias=True`, sem fornecer `hoje` nesse chamador. `app/folha/export.py:1168`; `app/folha/fora_da_folha.py:95-99`. |
| `InsumosDoDono` | Recebe `(colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`. O comando E6 fornece `espelho=esp, agora=_agora_do_ato`. `app/ponto/management/commands/e6_oraculo.py:128`; `app/ponto/management/commands/e6_oraculo.py:316`. |
| `pergunta_viva` | Recebe o **objeto da pergunta**, cujo estado e relações são consultados pelo cálculo. A alimentação das portas chama esse juiz para as perguntas da célula. `app/chamados/juizes.py:822-848`; `app/chamados/juizes.py:1300-1303`. |

Passar `limite` maior explica uma listagem mais extensa, mas não muda os contadores nem omite dias sem porta: esses dias entram sempre. Isso, isoladamente, não seria divergência de classificação. `app/chamados/services/vigia_fio.py:551-553`; `app/chamados/services/vigia_fio.py:628-631`.

**2. Quem poderia emitir e por que não emite?**

A distinção decisiva é: **col574 tem julgamento `nunca_bateu`; col950 tem célula física sem julgamento**, conforme a saída fornecida. `[nao verificado]`

O motivo `sem_celula` da folha também designa **célula existente sem veredito**, quando `trabalha` está preenchido; portanto ele não prova ausência física de `CelulaDia`. `app/folha/export.py:1255-1256`.

| Candidato existente | Bloqueio demonstrável ou limite da conclusão |
|---|---|
| **Cartório — col574** | A primeira batida apurável define adesão. Sem nenhuma, entra no ramo explicitamente descrito como **“carimba e não emite”**, lavra `nunca_bateu` sem cobertura integral e retorna antes da emissão normal. Os campos fornecidos — zero batidas da vida, `nunca_bateu`, protestos vazios — correspondem a esse mecanismo. `app/ponto/services/cartorio.py:1154-1162`; `app/ponto/services/cartorio.py:1232-1242`; `app/ponto/services/cartorio.py:1268`. |
| **Cobrança normal do cartório — col574** | Além do retorno anterior, o caminho normal exclui desligados. O estado atual fornecido basta para barrá-lo; não prova qual era a situação na data de cada julgamento. `app/ponto/services/cartorio.py:1696-1697`; `app/ponto/services/cartorio.py:1762`. Histórico: `[nao verificado]`. |
| **Cartório — col950** | É o responsável existente por julgar células passadas: a fila contém todas elas, sem filtro de situação. Entretanto, o cron declarado só expande empresas **2, 3 e 4**; a empresa 1 fornecida está fora dessa expansão. Isso prova o não alcance pela configuração declarada, não qual execução histórica ocorreu. `app/ponto/management/commands/processar_cartorio.py:45-65`; `app/config/crons.py:17`; `app/config/crons.py:616`; `app/config/crons.py:1835`. |
| **Fábrica de perguntas pela célula** | Exige colaborador ativo e dia que o juiz não considere sem trabalho: col574 desligado é barrado; suas folgas também. Para col950, a fábrica depende dos motivos extraídos da ata; sem ata, o questionário se abstém quando existe vínculo. O retorno efetivo do vínculo e de `_marcos_faltantes` não foi impresso. `app/chamados/services/fabrica_da_celula.py:89-104`; `app/chamados/services/fabrica_da_celula.py:45-57`; `app/chamados/services/disputa_emissao.py:818-843`. Aplicação completa ao col950: `[nao verificado]`. |
| **Pergunta por chamados órfãos** | Recebe chamados, não dias retidos. Retorna se a entrada estiver vazia ou sem chamados regularizáveis. A contagem de chamados sem célula criados perto do dia não informa módulo, data canônica, status ou destino suficientes para provar que alcançariam aquele dia. `app/chamados/services/disputa_emissao.py:876-896`; `app/chamados/services/disputa_emissao.py:925-933`. Conteúdo desses chamados: `[nao verificado]`. |
| **Detector de ausência/batida** | Seleciona colaboradores em operação, exigindo ativo, empresa ativa, identidade operacional, rollout e vínculo vigente hoje. Col574 é excluído pela situação atual. Para col950, apenas estar ativo não comprova todas essas guardas. O detector percorre hoje/ontem e, depois do encerramento do dia, atribui a cobrança ao cartório. `app/ponto/management/commands/detectar_ausencias.py:98-99`; `app/colaboradores/queries.py:25-29`; `app/colaboradores/queries.py:65`; `app/ponto/management/commands/detectar_ausencias.py:136-138`; `app/ponto/management/commands/detectar_ausencias.py:460-473`. |
| **Funil de cobrança de furo** | O veto da célula cala `trabalha=False` e `veredito=nunca_bateu`. Isso cobre as folgas e os dias de trabalho do col574 descritos no dado, mesmo numa chamada direta ao funil. `app/ponto/marcos.py:119`; `app/ponto/marcos.py:155-162`; `app/ponto/management/commands/detectar_ausencias.py:630-633`. |
| **Adesão** | O universo exige **ativo + `nunca_bateu`**; também exclui quem já possui batida apurável. Col574 é desligado; col950 já bateu e não apresenta esse veredito. Há ainda a regra expressa “desligado não adere a nada”. `app/chamados/services/adesao.py:32-34`; `app/chamados/services/adesao.py:60-67`. |
| **Lista DP/Cadastro x Realidade — C5** | É o encaminhamento escrito para quem nunca bateu, mas o carregador exclui desligados. Além disso, C5 é uma assinatura do colaborador, sem campo `data`; não insere seus dias em `dias_na_lista`. `app/escala/services/cadastro_realidade.py:72-79`; `app/escala/services/cadastro_realidade.py:151-164`; `app/escala/services/cadastro_realidade.py:955-970`. |
| **Turno aberto, corrente ou retroativo** | Exige turno materializado aberto e entrada; o retroativo exige também timestamp da entrada. Zero batidas na vida do col574 não fornece esse fato. Os turnos materializados do col950 não foram impressos; a batida da vizinhança de 09/10 não prova sua atribuição ao turno. `app/ponto/management/commands/processar_alertas_turno.py:76-93`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88-103`. Turnos efetivos: `[nao verificado]`. |
| **Chamado de ausência** | Recebe uma `Ausencia` já existente; não converte automaticamente dia sem batida em ausência. `ausencia_id=None` na célula não prova inexistência de registro bruto ou submissão. `app/ponto/services/ausencia.py:621-646`. Ausências não lavradas: `[nao verificado]`. |
| **Consumidor de `EventoCelula`** | Rejunta, retrata, encerra órfãs e fecha cobrança por lastro. O processamento do evento não significa que deveria criar uma porta para um dia vazio. `app/chamados/reconciliador.py:1346-1371`. |
| **`sem_batidas_mes` / `reconciliar_grade`** | O primeiro emissor está aposentado por escrito; o segundo é medidor na rotina, com escrita reservada à bancada dirigida. Encontrá-los no grep não os torna responsáveis atuais pelos dias. `app/ponto/services/fechamento.py:1089-1093`; `app/ponto/management/commands/reconciliar_grade.py:1-11`. |

Para o **Resolver dia**, a competência corrente não recebe `pode_ultimo_recurso`; a função também exige passado e status `falta`, `irregular`, `aberto` ou `sem_certidao`. Isso explica o bloqueio dos dias desde 21/10 e, pelo status de folga, das quatro folgas antigas do col574, contra os campos fornecidos. `app/colaboradores/services/calendario.py:94-104`; `app/colaboradores/services/calendario.py:738-739`.

Para os dias antigos do col950, `status_dia=None` bruto não basta para explicar o `False`: falta o item efetivo do calendário, incluindo presença do dia, `status` e `pode_ultimo_recurso`. A porta também responde `False` quando o calendário não devolve o dia. `app/colaboradores/services/calendario.py:97-104`; `app/colaboradores/services/calendario.py:132-136`.

**3. Quem escreve e quem lê os campos decisivos?**

“Pergunta viva”, “dono fora do fio” e “motivo de retenção” são resultados produzidos por funções; não são campos independentes que algum emissor simplesmente preenche. `app/chamados/juizes.py:822-848`; `app/chamados/services/vigia_fio.py:274-315`; `app/folha/export.py:1218-1222`.

| Campo ou resultado | Escritor/produtor | Leitor que decide a classe |
|---|---|---|
| Existência da pergunta, `data_evento`, `celula`, destino | `PerguntaDisputa.abrir` localiza/reaponta célula e destino e cria a pergunta. `app/chamados/models.py:2428-2477`. | `alimentar_portas` agrupa perguntas da célula e testa vitalidade; o censo recebe esse mapa. `app/chamados/juizes.py:1300-1303`; `app/chamados/services/vigia_fio.py:610-616`. |
| `resposta_colab`, `respondida_em` | Porta de resposta. `app/chamados/services/disputa_emissao.py:1282-1301`. | Vitalidade exige carimbo e texto para o ramo de resposta. `app/chamados/juizes.py:1150-1162`. |
| `validada_em` | Validação escreve; nova resposta desfaz validação anterior. `app/chamados/services/validacao.py:127-133`; `app/chamados/services/disputa_emissao.py:1285-1297`. | Primeiro degrau da vitalidade. `app/chamados/juizes.py:1135-1137`. |
| `materializada_em`, `via_resolucao` | Portas de carimbo, identidade e descarimbo. `app/chamados/models.py:2532-2543`; `app/chamados/models.py:2625-2644`. | Vitalidade lê via, carimbo e testemunho do marco/dia. `app/chamados/juizes.py:1145-1146`; `app/chamados/juizes.py:1170-1214`. |
| “Pergunta viva” | `pergunta_viva` delega ao cálculo; a property `viva` delega ao mesmo juiz. `app/chamados/juizes.py:822-848`; `app/chamados/models.py:2763-2773`. | Porta alcançável e mapa usado pelo censo. `app/chamados/juizes.py:1373-1383`; `app/chamados/services/vigia_fio.py:610`. |
| Colaborador e dia na lista do admin | `cadastro_realidade.lista` monta linhas/assinaturas; `dias_na_lista` exige assinatura com `data` e resolve seu dono. `app/escala/services/cadastro_realidade.py:792-857`; `app/escala/services/cadastro_realidade.py:950-970`. | `ListaDoAdmin` guarda separadamente conjunto de colaboradores e mapa de dias; a classificação consulta ambos. `app/chamados/services/vigia_fio.py:216-218`; `app/chamados/services/vigia_fio.py:222-251`; `app/chamados/services/vigia_fio.py:661-686`. |
| “Resolver dia” | Calendário produz último recurso; juiz exige passado/status permitido. `app/colaboradores/services/calendario.py:738-739`; `app/colaboradores/services/calendario.py:97-104`. | Porta plural e `dono_fora_do_fio`. `app/colaboradores/services/calendario.py:132-136`; `app/chamados/services/vigia_fio.py:306-307`. |
| Dono fora do fio | Ordem: Resolver oferecido → ausência cobrindo em `ABERTAS` → aceite de folga validado → dia na lista do admin. `app/chamados/services/vigia_fio.py:306-315`. | `censo_alcance` consulta esse resultado antes de atribuir causa. `app/chamados/services/vigia_fio.py:654-659`. |
| Dono E6 mostrado na sonda | `InsumosDoDono.dono` monta insumos e chama `dono_da_divergencia`. `app/ponto/management/commands/e6_oraculo.py:172-183`. | **Não entra diretamente na classe:** o vigia lê o dono presente na lista. `app/chamados/services/vigia_fio.py:313-314`. |
| Veredito, protestos, ata e status da célula | Porta de lavra grava esses dados. `app/ponto/portas/celula.py:809-817`. | Retenção lê veredito/protestos; vitalidade também lê veredito e marco da ata. `app/folha/export.py:1233-1256`; `app/chamados/juizes.py:1177-1196`. |
| Motivo e dias de retenção | `motivos_retencao_celula` produz ambos na mesma decisão. `app/folha/export.py:1218-1256`. | `dias_que_seguram` fornece o universo ao censo. `app/folha/fora_da_folha.py:95-99`; `app/chamados/services/vigia_fio.py:582-587`. |

**`ESTRUTURA` não identifica um emissor faltante:** no E6 é o resíduo das outras condições, e zero batidas é expressamente contagem par. `app/ponto/management/commands/e6_oraculo.py:69-75`; `app/ponto/management/commands/e6_oraculo.py:98-110`.

**4. Existe causa comum aos 21 novos?**

**Não foi demonstrada.** O dado divide os novos em **20 do col574 e 1 do col950**, mas não fornece o estado anterior de cada porta nem o evento responsável pela mudança. `[nao verificado]`

Os mecanismos atuais são distintos:

- **col574:** retenção por veredito acusador com protestos vazios, combinada com silêncio de cobrança/adesão e exclusão da C5 por desligamento. `app/folha/export.py:1233-1254`; `app/ponto/services/cartorio.py:1157-1162`; `app/escala/services/cadastro_realidade.py:151-154`.
- **col950:** retenção por célula sem veredito; seu novo dia 05/11 também passou a ser passado em 06/11, condição que o retentor e o vigia usam. Isso torna o avanço de data compatível com esse único novo, sem provar o histórico da execução. `app/folha/export.py:1255-1256`; `app/chamados/services/vigia_fio.py:648-650`. Causa histórica: `[nao verificado]`.

Não se pode afirmar que col574 foi desligado entre as duas sondas, nem que o cancelamento de `sem_batidas_mes` provocou a entrada dos vinte: faltam situação anterior, trilha e conteúdo do chamado. `[nao verificado]`

**5. Classificação**

| Colaborador | Classificação sustentada |
|---|---|
| **col574 — 20 dias** | **LEI FALTANDO quanto à atribuição específica de encaminhamento do desligado que nunca bateu.** As regras existentes mandam calar cobrança, excluem desligado da adesão e da C5, enquanto a retenção continua lendo seus dias até a demissão. Não encontrei regra específica que determine qual emissor deve abrir uma porta substituta nessa combinação. `app/ponto/services/cartorio.py:1157-1162`; `app/chamados/services/adesao.py:66-67`; `app/escala/services/cadastro_realidade.py:134-154`; `app/folha/export.py:1198-1201`. Ausência de outra atribuição escrita: `[nao verificado]`. |
| **col950 — 19 dias** | **INCONCLUSIVO sobre o mecanismo histórico.** Há responsável escrito: cartório para células passadas. Há diferença comprovada de universos: cron somente 2/3/4 e censo por empresas visíveis. Falta estabelecer a obrigação operacional da empresa 1 e se houve chamada efetiva do cartório para ela; se houve, falta saber se a grade devolveu esses dias ou ocorreu falha. `app/ponto/management/commands/processar_cartorio.py:45-65`; `app/config/crons.py:17`; `app/config/crons.py:1835`; `app/chamados/services/vigia_fio.py:567-585`; `app/ponto/services/cartorio.py:1365-1367`. |

Essa classificação **não significa ausência de regra geral de alcance**: há corte escrito exigindo fio para lâmpada acusadora, encaminhando dia sem fio ao Admin e qualificando o contador positivo como bug. A falha de alcance está registrada no dado fornecido; o que não ficou provado, pelo critério mais específico da pergunta, é **um emissor individual desobedecendo sua própria obrigação neste caso**. `app/docs/CORTES.md:263`; `app/docs/CORTES.md:265`; dado atual `[nao verificado]`.

Para fechar o diagnóstico faltam: **o script da sonda; os argumentos/estado da rodada de 05/11; a execução efetiva do cartório para empresa 1; e, por dia do col950, a saída da grade e do calendário usados pelos juízes.** Os pontos que consomem essas informações estão em `app/chamados/management/commands/vigiar_fio_morto.py:163-173`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1365-1367` e `app/colaboradores/services/calendario.py:132-136`. Disponibilidade dessas medições: `[nao verificado]`.

## 06/11 09:26 · sessao 984f6712ccf6 · G1-PORTA · sonda · origem=Code

**Os dados não provam que seguir as portas libera os cinco casos.** O juiz da porta responde por onde alcançar a célula; o export decide a situação do **colaborador na competência**, e não de cada dia isoladamente. ([folha/export.py:757](app/folha/export.py:757), [chamados/juizes.py:1341](app/chamados/juizes.py:1341))

**1. A sonda reproduz as chamadas reais?**

Não é possível certificar: o corpo de `sonda_g1porta.py` não consta do material recebido/localizado; os argumentos efetivamente usados permanecem **[não verificado]**.

Os pontos concretos de comparação são:

| Função | Chamada/contrato do sistema |
|---|---|
| `motivos_retencao_celula` | `_dias_que_retem` passa colaborador, início e fim, com `com_dias=True`, **sem `hoje` explícito**; a janela vem de `janela_fechamento`. ([fora_da_folha.py:95](app/folha/fora_da_folha.py:95), [fora_da_folha.py:118](app/folha/fora_da_folha.py:118)) |
| Alimentação e porta | Alimenta IDs de células e colaboradores; calcula verbos sobre **todos os `qs_vivos()`**, não somente sobre os casos selecionados; consulta a porta com `alimentacao=alim`. Recortar previamente os chamados pode mudar o crédito das perguntas. ([chamados/juizes.py:1267](app/chamados/juizes.py:1267), [chamados/juizes.py:1287](app/chamados/juizes.py:1287), [vigia_fio.py:635](app/chamados/services/vigia_fio.py:635)) |
| `InsumosDoDono` | E6 passa colaborador, janela, `HOJE`, `espelho=esp` e `agora` comum. A classe também aceita carregar o espelho sob demanda. ([e6_oraculo.py:128](app/ponto/management/commands/e6_oraculo.py:128), [e6_oraculo.py:154](app/ponto/management/commands/e6_oraculo.py:154), [e6_oraculo.py:316](app/ponto/management/commands/e6_oraculo.py:316)) |
| `censo_alcance` | O comando compartilha `ListaDoAdmin(hoje, empresas, agora=agora)` e chama o censo dentro de `tranca_em_lote()`. ([vigiar_fio_morto.py:163](app/chamados/management/commands/vigiar_fio_morto.py:163), [vigiar_fio_morto.py:167](app/chamados/management/commands/vigiar_fio_morto.py:167), [vigiar_fio_morto.py:171](app/chamados/management/commands/vigiar_fio_morto.py:171)) |

Há uma diferença de apresentação a conferir: o censo tem `limite=50`; a saída recebida informa lista integral. Ampliar esse limite altera a listagem, não o universo nem o contador. O argumento usado pela sonda é **[não verificado]**. ([vigia_fio.py:542](app/chamados/services/vigia_fio.py:542), [vigia_fio.py:625](app/chamados/services/vigia_fio.py:625))

A diferença semântica importante é o uso do dono: **o E6 real só consulta `.dono(data)` depois de identificar divergência**, passando pelos descartes anteriores. A sonda recebida não apresenta essa comparação; equivalência com esse fluxo é **[não verificado]**. ([e6_oraculo.py:330](app/ponto/management/commands/e6_oraculo.py:330), [e6_oraculo.py:353](app/ponto/management/commands/e6_oraculo.py:353), [e6_oraculo.py:366](app/ponto/management/commands/e6_oraculo.py:366))

**2. Quem escreve e quem lê?**

| Objeto | Quem produz/escreve | Quem lê |
|---|---|---|
| **Datas retentoras** | `motivos_retencao_celula`, pelo `_retem`, acrescenta data, motivo, célula e veredito ao retorno. Seu insumo persistido é a célula; o escritor único de veredito/protestos é `lavrar_veredito`. ([export.py:1218](app/folha/export.py:1218), [escala/models.py:1819](app/escala/models.py:1819)) | `_dias_que_retem` → `dias_que_seguram`; o censo enumera essas datas. `fora_da_folha` também as entrega ao retrato. ([fora_da_folha.py:98](app/folha/fora_da_folha.py:98), [vigia_fio.py:585](app/chamados/services/vigia_fio.py:585), [fora_da_folha.py:192](app/folha/fora_da_folha.py:192)) |
| **Porta de cada data** | `alimentar_portas` carrega fila, verbos e perguntas; `porta_alcancavel_do_dia` produz em memória `rota`, `fila`, `verbos` e `fio`. ([chamados/juizes.py:1282](app/chamados/juizes.py:1282), [chamados/juizes.py:1386](app/chamados/juizes.py:1386)) | `_fio_do_dia` da folha e `censo_alcance`. ([fora_da_folha.py:77](app/folha/fora_da_folha.py:77), [vigia_fio.py:635](app/chamados/services/vigia_fio.py:635)) |
| **Dono E6** | `InsumosDoDono.dono` monta os insumos e delega a `dono_da_divergencia`; o juiz devolve CADASTRO/BATIDA/ESTRUTURA. ([e6_oraculo.py:172](app/ponto/management/commands/e6_oraculo.py:172), [e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py:98)) | E6 contabiliza o retorno; pautas e teto também o consomem. ([e6_oraculo.py:366](app/ponto/management/commands/e6_oraculo.py:366), [pautas_discordantes.py:109](app/ponto/management/commands/pautas_discordantes.py:109), [teto_na_lista.py:86](app/ponto/services/teto_na_lista.py:86)) |

**O `dono` do retrato é outro conceito:** guarda o **fio agregado** da pessoa, não o dono E6. As datas e esse fio são persistidos pelo lavrador do retrato. ([retrato_folha.py:1183](app/folha/retrato_folha.py:1183), [lavrar_retrato_folha.py:74](app/folha/management/commands/lavrar_retrato_folha.py:74))

**3. Por caso, o admin consegue liberar?**

`cobrar` significa aguardar resposta do colaborador e depois validação; `validar` oferece ato do admin; `decidir` exige julgamento humano. Nenhum desses verbos equivale a liberação garantida. ([fora_da_folha.py:40](app/folha/fora_da_folha.py:40), [verbos.py:80](app/chamados/services/verbos.py:80))

A tabela aplica esses significados aos dados recebidos; sucesso dos atos e resultado posterior são **[não verificado]**.

| Caso / dia indicado | Portas das retentoras informadas | O que se pode concluir |
|---|---|---|
| **col504 — 21/10** | 29/10: fila, **cobrar** | Depende de resposta do colaborador; o admin sozinho não conclui o fluxo indicado. **[não verificado]** |
| **col709 — 22/10** | 30/10: fila, **validar**; 02, 03 e 04/11: fila, **cobrar** | Há uma validação e três dependências do colaborador. O dono ESTRUTURA de 02/11 não transforma seu verbo em ação de liberação. **[não verificado]** |
| **col204 — 21/09** | 29/09: fila, **validar** | Há ato disponível ao admin; aceitar a validação e eliminar a retenção não foram demonstrados. **[não verificado]** |
| **col43 — 26/10** | 01/11: pergunta, **cobrar** | Depende de resposta do colaborador e posterior validação. **[não verificado]** |
| **col502 — 09/10** | 28/09: pergunta, **cobrar**; 07/10: fila, **cobrar**; 10 e 14/10: fila, **decidir**; 18/10: fila, **cobrar** | Três cobranças e dois julgamentos; seguir uma porta não resolve as demais. **[não verificado]** |

Além disso, a porta não recebe usuário e não certifica autorização individual; a validação real pode recusar a operação. ([chamados/juizes.py:1341](app/chamados/juizes.py:1341), [views_cobrar.py:24](app/chamados/views_cobrar.py:24), [validacao.py:363](app/chamados/services/validacao.py:363))

Mesmo removidas todas as retenções de espelho, o export ainda verifica homologação; `turno_aberto` pertence à pergunta separada de aprovação/tranca. ([export.py:788](app/folha/export.py:788), [export.py:825](app/folha/export.py:825), [export.py:966](app/folha/export.py:966))

**Tela/leitor existente:** o drawer do colaborador, aberto com competência, lê a linha gravada do retrato e mostra **“Datas (ISO)”**. Portanto, existe apresentação **colaborador/competência → datas retentoras**. ([colaboradores/views.py:975](app/colaboradores/views.py:975), [colaboradores/views.py:982](app/colaboradores/views.py:982), [_retrato_o_que_fazer.html:43](app/templates/folha/_retrato_o_que_fazer.html:43))

Esse bloco não apresenta uma relação explícita **dia afetado X → datas Y/Z**; seus links recebem o colaborador, não cada retentora. A presença e atualização dos cinco casos nesse retrato são **[não verificado]**. ([retrato_folha.py:1177](app/folha/retrato_folha.py:1177), [retrato_folha.py:1020](app/folha/retrato_folha.py:1020))

A ferramenta `o_que_segura` fornece cada retentora, fio, ação e chamados, também por colaborador/competência. Uma negativa sobre **todas** as telas mostrarem a relação específica entre dias permanece **[não verificado]**. ([fora_da_folha.py:203](app/folha/fora_da_folha.py:203), [fora_da_folha.py:227](app/folha/fora_da_folha.py:227))

**4. Por que fora do censo e ESTRUTURA? Qual classificação?**

- **Fora do censo:** ele enumera exclusivamente as datas que **causam** retenção, retornadas por `dias_que_seguram`; não expande o bloqueio mensal para todos os dias da janela. Isso corresponde à regra escrita “dias que seguram a folha”. ([vigia_fio.py:585](app/chamados/services/vigia_fio.py:585), [CORTES.md:379](app/docs/CORTES.md:379))
- **ESTRUTURA:** é o retorno residual do juiz após excluir os ramos de cadastro/batida/fase. A função não exige previamente uma diferença de valores; essa exigência pertence ao chamador E6. Logo, obter ESTRUTURA numa data não comprova defeito nela. ([e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py:98), [e6_oraculo.py:172](app/ponto/management/commands/e6_oraculo.py:172), [e6_oraculo.py:353](app/ponto/management/commands/e6_oraculo.py:353))

**Classificação: INCONCLUSIVO quanto à liberação; BUG não provado por essa combinação.** A exclusão do dia afetado é compatível com o universo escrito; o dono isolado não demonstra incoerência contra a L-099. ([CORTES.md:379](app/docs/CORTES.md:379), [CLAUDE.md:176](CLAUDE.md:176), [e6_oraculo.py:110](app/ponto/management/commands/e6_oraculo.py:110))

Para concluir faltam: script integral da sonda; resultado dos atos nas retentoras; nova leitura das retentoras/export; e, para acusar divergência ESTRUTURA, comparação dos valores do mesmo dia. Tudo isso está **[não verificado]**.

Não há **LEI FALTANDO demonstrada**: o corte existente define alcance das causas. Uma obrigação adicional de exibir cada dia afetado ligado às suas retentoras precisaria de regra específica, cuja existência permanece **[não verificado]**. ([CORTES.md:379](app/docs/CORTES.md:379), [CORTES.md:401](app/docs/CORTES.md:401))

## 06/11 09:25 · sessao 892efc794fa0 · - · - · origem=Ronald (tmux)

Achados consolidados, todos somente leitura:

| Candidato | Condição no código e aplicação aos casos |
|---|---|
| Cartório, ramo sem adesão | A primeira batida apurável define `adesao`; nenhuma batida entra no ramo cuja regra escrita é “carimba e nao emite”. Esse ramo lavra `nunca_bateu` sem cobertura integral e retorna antes da emissão normal. Aplica-se ao `col574` conforme o dado fornecido. `app/ponto/services/cartorio.py:1154-1162`, `:1232-1242`, `:1268`. |
| Cobrança normal do cartório | Exclui expressamente `situacao == 'desligado'`, tanto para furo inteiro quanto parcial. Também exige código de furo, ausência de chamado vivo/piso e vínculo/dia de trabalho/marco inicial. O desligamento basta para impedir `col574`; os demais gates não devem ser inventados. `app/ponto/services/cartorio.py:1684`, `:1696-1697`, `:1714-1742`, `:1762`. |
| Adesão | `universo_adesao` exige situação `ativo` e célula com `veredito='nunca_bateu'`. O sincronizador reforça “desligado nao adere a nada”. Assim, `col574` é excluído pela situação; `col950` não tem o veredito exigido e já bateu. `app/chamados/services/adesao.py:25-40`, `:58-67`, `:74-86`. |
| Detector de ausência/batida | Universo passa por `colaboradores_em_operacao`, que exige ativo, empresa ativa, username operacional, rollout e vínculo vigente hoje. Examina hoje/ontem e só cobra antes de o dia encerrar; depois a atribuição escrita é do cartório. `col574` é excluído por desligamento; para `col950`, rollout/identidade/vínculo hoje não foram impressos. `app/colaboradores/queries.py:25-28`, `:50-65`; `app/ponto/management/commands/detectar_ausencias.py:98-99`, `:136-138`, `:305-306`, `:460-473`. |
| Funil de cobrança | A célula veta cobrança por `trabalha=False` ou `veredito='nunca_bateu'`; o funil real lê esse juiz. Isso impede cobrar as folgas e os nunca-bateu do `col574`, mesmo se chamado diretamente. `app/ponto/marcos.py:119`, `:155-162`; `app/ponto/management/commands/detectar_ausencias.py:630-633`. |
| Fábrica de perguntas por chamados | Não percorre dias retidos autonomamente: recebe chamados órfãos, retorna se lista vazia ou sem regularizáveis. Mesmo havendo chamado, sem ata e com vínculo existente abstém-se de perguntar. Para os chamados sem célula que a sonda apenas conta, faltam módulo, contexto/data canônica, status e destino para determinar se alcançariam os dias. `app/chamados/services/disputa_emissao.py:876-896`, `:925-933`, `:818-843`. |
| Turno aberto | Emissor corrente exige turno materializado aberto com entrada; retroativo exige turno aberto e timestamp da entrada. Dias sem batida não criam tal fato. Para `col950` em 09/10 a única batida da vizinhança é de 08/10: atribuição ao turno exige medir a autoridade, não usar a vizinhança como prova. `app/ponto/management/commands/processar_alertas_turno.py:76-98`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88-103`. |
| Ausência do colaborador | O emissor recebe uma `Ausencia` real já criada; não transforma automaticamente silêncio de batida em documento/ausência. API e formulário o chamam após criar registro. `ausencia_id=None` na célula não prova inexistência de ausência crua ou submissão. `app/ponto/services/ausencia.py:621-628`, `:643-646`; `app/api/views.py:1848-1857`; `app/ponto/views.py:2535-2547`. |
| `sem_batidas_mes` | Emissor aposentado por escrito desde 02/07, por duplicata sem dono; painel de folha registra inconsistências. O único cancelado de `col574` não pode ser identificado como causa histórica dos 20 dias. `app/ponto/services/fechamento.py:1089-1093`. |
| `reconciliar_grade` | O grep encontra um escritor de bancada, mas módulo declara comando como medidor sem escritor rotineiro; `--apply` ficou para sombra dirigida. Ainda exigiria ativo e leria o veto da célula. `app/ponto/management/commands/reconciliar_grade.py:1-11`, `:106`, `:501-504`. |
| Consumidor `EventoCelula` | Rejunta chamados, retrata perguntas/chamados/premissas, encerra órfãs e fecha por lastro. Não constitui emissor autônomo de porta para dia vazio. Evento processado não prova que havia obrigação de criar chamado/pergunta. `app/chamados/reconciliador.py:1346-1371`. |

Ponto decisivo para `col950`: o cartório declarado no cron é `por_empresa=True`, mas o universo estático é somente `(2,3,4)`. Portanto a configuração da cópia não agenda o cartório para a empresa 1. A expansão usa essa tupla. Isso explica um caminho estrutural de não alcance; não prova qual execução histórica ocorreu. O próprio contrato nomeia esse universo como empresas em operação, então a mera existência de `col950` na empresa 1 não basta para provar violação de uma regra que a inclua. `app/config/crons.py:10-17`, `:616`, `:1835`.

Se o cartório for chamado para empresa 1, sua fila é toda célula passada, sem filtro de situação. Célula sem julgamento não é pulada pela idempotência; ainda pode ser pulada se o dia não aparecer na grade carregada. Para fechar o mecanismo de `col950`, faltam o resultado dessa grade, vínculo alimentado e execução real do comando. `app/ponto/management/commands/processar_cartorio.py:45-65`; `app/ponto/services/cartorio.py:1358-1367`.

Para classificação: em `col574`, há regra explícita para calar cobrança e adesão; não encontrei regra específica atribuindo a algum emissor a dívida de pessoa desligada que nunca bateu, embora o dia permaneça retido. Isso sustenta **LEI FALTANDO quanto à atribuição de emissão**, sem converter o “esperado 0” em emissor inventado. `app/ponto/services/cartorio.py:1157-1162`, `:1268`; `app/chamados/services/adesao.py:66-67`; `app/chamados/services/vigia_fio.py:532-534`.

Em `col950`, há atribuição existente ao cartório para dia encerrado, mas seu cron declarado exclui empresa 1; falta regra incluindo a empresa 1 no universo operacional, além de execução/grade para estabelecer a causa concreta. **INCONCLUSIVO**, com divergência de universos comprovada na configuração. `app/ponto/management/commands/detectar_ausencias.py:460-464`; `app/ponto/management/commands/processar_cartorio.py:45-65`; `app/config/crons.py:17`, `:616`, `:1835`.

## 06/11 09:25 · sessao dd1bfbb8a56f · - · - · origem=Ronald (tmux)

**1. Paridade da sonda**

O arquivo `sonda_esemviva.py` não foi encontrado na cópia pelo levantamento de nomes; o prompt contém a saída, mas não o script. Portanto, argumentos efetivamente usados pela sonda e qualquer divergência script × código são **[nao verificado]**.

O contrato real a conferir no script é:

- `censo_alcance(empresas=None, hoje=None, limite=50, lista=None)` em `app/chamados/services/vigia_fio.py:542`.
- O command captura um instante, deriva `hoje`, cria `ListaDoAdmin(hoje, empresas, agora=agora)` e chama `censo_alcance(empresas, hoje=hoje, lista=lista)` dentro de `tranca_em_lote()`: `app/chamados/management/commands/vigiar_fio_morto.py:163-173`.
- `ListaDoAdmin.__init__(hoje, empresas=None, *, agora=None)`: `app/chamados/services/vigia_fio.py:199-203`. Carrega visibilidade e passa `lista(hoje=self.hoje, empresas=vis, agora=self.agora)`: `app/chamados/services/vigia_fio.py:211-218`.
- `motivos_retencao_celula(colab, ini, fim, hoje=None, com_dias=False)`: `app/folha/export.py:1168`; produção usa `com_dias=True`, sem passar `hoje`, em `app/folha/fora_da_folha.py:95-99`. Universo de retenção vem de janela do juiz e `classificar_export`: `app/folha/fora_da_folha.py:118-137`. Assim, passar `hoje` explicitamente na sonda pode ser legítimo para congelar medição, mas não é a mesma forma literal desse chamador.
- `InsumosDoDono(colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`: `app/ponto/management/commands/e6_oraculo.py:128-146`; `dono(data)` carrega abertura, fase e cadastro e pergunta ao juiz: `:172-183`.
- `pergunta_viva(p)` recebe **objeto PerguntaDisputa**, não ID/estado textual: `app/chamados/juizes.py:822-848`. `alimentar_portas` usa todas as perguntas da célula e chama esse juiz: `:1300-1303`.

**2. Por que a classe é exatamente `e/sem_pergunta_viva`**

A porta exige chamado **da célula** na fila, ou pergunta viva daquela célula, no app e ancorada na fila: `app/chamados/juizes.py:1341-1349`, implementação `:1373-1383`. Chamados sem célula contados por proximidade de criação não satisfazem essa regra por esse único fato: `:1344-1348`.

Depois de ausência de porta e dia passado, o vigia consulta os donos externos: `app/chamados/services/vigia_fio.py:635-659`. Na ausência deles, não havendo pergunta viva, nem dia na lista, nem colaborador na lista, ele termina em `e/sem_pergunta_viva`: `:661-686`. Não há regra aqui que deduza qual emissor deixou de agir; esse vigia apenas classifica: `:542-553`, `:695-699`.

**3. Escritores e leitores dos campos decisivos**

| Campo/resultado | Quem escreve/produz | Quem lê |
|---|---|---|
| Pergunta existe; `celula`, `data_evento`, destino | Gate `PerguntaDisputa.abrir`: procura célula e identidade, reaponta e cria: `app/chamados/models.py:2398-2477` | `alimentar_portas` agrupa por célula e chama `pergunta_viva`: `app/chamados/juizes.py:1300-1303`; `censo_alcance` usa esse mapa em `app/chamados/services/vigia_fio.py:610-616` |
| `resposta_colab` + `respondida_em` | `responder_pergunta_disputa_core`: `app/chamados/services/disputa_emissao.py:1282-1301` | `app/chamados/juizes.py:1150-1162` |
| `validada_em` | Validação humana: `app/chamados/services/validacao.py:127-133`; resposta nova desfaz: `app/chamados/services/disputa_emissao.py:1285-1297`; auto-validação é porta do carimbo conforme `app/core/tests/test_contract_escritores_estado.py:133-147` | Primeiro degrau da vitalidade: `app/chamados/juizes.py:1135-1137` |
| `materializada_em` + `via_resolucao` | Portas `carimbar`, `carimbar_identidade`, `descarimbar`; contrato e propósito: `app/chamados/models.py:2532-2543`; identidade escreve e salva em `:2625-2644` | `app/chamados/juizes.py:1145-1146`, `:1170-1179`, `:1207-1214` |
| “pergunta viva” | É resultado de leitura, não campo gravado: memo delega ao cálculo em `app/chamados/juizes.py:822-848`; regras `:1135-1214` | Property `viva` delega ao mesmo juiz: `app/chamados/models.py:2763-2773`; `estado` traduz a leitura em `:2492-2506` |
| Dia e dono na lista do admin | `cadastro_realidade.lista` junta assinaturas e monta as linhas: `app/escala/services/cadastro_realidade.py:792-857`; harmoniza dono via juiz em `:897-935`; `dias_na_lista` só aceita assinatura com **data** e resolve dono: `:950-970` | `ListaDoAdmin._carregar` lê mapas e conjunto de colabs: `app/chamados/services/vigia_fio.py:216-218`; dia/admin em `:222-236`; colab em `:248-251`; classe lê ambos em `:661-686` |
| Dono L-099 exibido pela sonda | `InsumosDoDono.dono` compõe insumos: `app/ponto/management/commands/e6_oraculo.py:172-183`; juiz tem ordem cadastro explícito → turno aberto real → ímpar → fase → CxR → estrutura: `:98-110` | Esse `dono=ESTRUTURA` não é argumento direto de `censo_alcance`; o vigia lê dono **da lista**: `app/chamados/services/vigia_fio.py:313-314`, `:654-656` |
| Dono fora do fio | Composição ordenada de Resolver dia, ausência aberta, folga validada e lista: `app/chamados/services/vigia_fio.py:274-315` | `censo_alcance`: `:654-659` |
| “Resolver dia” | Campo montado pelo calendário: último recurso se competência encerrada e sem pergunta viva respondida: `app/colaboradores/services/calendario.py:455-475`, `:738-739`; juiz exige passado e status permitido: `:94-104` | Porta plural lê o mesmo campo ou devolve False se dia não veio no calendário: `:117-136`; vigia usa em `app/chamados/services/vigia_fio.py:258-271`, `:306-307` |
| Retenção | Função produz motivo e dia: `app/folha/export.py:1218-1222`; decide por veredito/protestos e cegueira em `:1230-1256` | `dias_que_seguram`: `app/folha/fora_da_folha.py:95-99`, `:118-137`; vigia toma esse universo: `app/chamados/services/vigia_fio.py:582-587` |
| `veredito`, protestos, ata, status | `CelulaDia.lavrar_veredito` delega: `app/escala/models.py:1819-1828`; porta escreve protestos/veredito/via/em e ata/status: `app/ponto/portas/celula.py:809-817`; status aplica em `:743-748` | Retenção: `app/folha/export.py:1233-1256`; vitalidade: `app/chamados/juizes.py:1180-1196` |

**4. Achados particulares relevantes**

**col574:** há lei explícita para nunca bateu: encaminhar à pauta/lista do DP por adesão/cadastro, pois perguntar a quem nunca usou o app não chega a ninguém: `app/escala/services/cadastro_realidade.py:72-79`. Porém o carregador C5 exclui expressamente desligados: `:151-154`, e sua docstring afirma isso como regra do portão: `:134-136`. A retenção não exclui por situação; corta apenas por admissão/demissão: `app/folha/export.py:1198-1205`, e `nunca_bateu` sem protestos continua retendo em `:1233-1254`.

Mesmo para um ativo, a C5 que esse código fabrica é assinatura de **colaborador**, sem `data` e `dono_do_dia`: `app/escala/services/cadastro_realidade.py:159-164`. Ela pode pôr o colaborador na lista, mas não põe cada dia no mapa `dias_na_lista`, porque esse mapa exige `data`: `:955-969`. Não confundir o dono adesão da C5 com o dono L-099 ESTRUTURA; são respostas de funções distintas: `:72-79` e `app/ponto/management/commands/e6_oraculo.py:69-75`.

Os dias de novembro de ambos não oferecem Resolver dia por serem da competência corrente (o calendário escreve que aí o caminho é chamado → pergunta → colaborador): `app/colaboradores/services/calendario.py:455-458`, `:738-739`. As folgas de outubro de col574 também não têm status em `STATUS_QUE_PEDEM_DECISAO`: `:94-104`.

**col950:** `sem_celula` na retenção não significa necessariamente ausência física de CelulaDia; uma célula existente sem veredito e `trabalha` preenchido também recebe esse motivo: `app/folha/export.py:1255-1256`. Isso corresponde aos campos apresentados pelo usuário **[nao verificado: saída fornecida sem arquivo:linha]**.

Para seus dias antigos de outubro, não se deve explicar `resolver_dia_oferecido=False` apenas por `status_dia=None`: esse é campo bruto; o calendário usa status calculado/lavrado do D4 e último recurso, não só aquele campo: `app/colaboradores/services/calendario.py:97-104`, `:122-136`. Faltaria medir, por dia, o item de `contexto_calendario`: `status`, `pode_ultimo_recurso`, presença do dia, `_ini_comp_atual` e perguntas respondidas que o calendário considera: `:132-136`, `:455-475`, `:738-739`.

**5. Limite da causa comum aos novos**

A atual saída distingue dois mecanismos: col574 é retido por veredito acusador sem protestos (`app/folha/export.py:1233-1254`), col950 por ausência de julgamento (`:1255-1256`). A ausência de porta é efeito comum (`app/chamados/services/vigia_fio.py:635-686`), mas uma causa histórica comum da entrada dos 21 entre 05 e 06/11 não é demonstrável sem o estado/argumentos da medição anterior e histórico das mudanças **[nao verificado]**. Em particular, situação atual “desligado” não prova que col574 mudou de situação nesse intervalo **[nao verificado]**.

## 06/11 09:25 · sessao d93523c2419c · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Dono L-099 não é atribuição de emissor.** `dono_da_divergencia` retorna ESTRUTURA como resíduo; zero batidas é explicitamente contagem par e pode ser ESTRUTURA. Portanto `dono=ESTRUTURA` não prova que faltou cobrança. `app/ponto/management/commands/e6_oraculo.py:69-75`, `:98-110`.
- **Contrato de `InsumosDoDono`:** `(colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`; a chamada real do comando passa `espelho=esp, agora=_agora_do_ato`. Carrega batidas `[ini−12h, fim+1d+14h]`, turnos `[ini−1d,fim+1d]`, cadastro, fase e CxR; `dono(data)` chama o juiz. `app/ponto/management/commands/e6_oraculo.py:128-183`, `:316`. Sem script da sonda, equivalência de seus argumentos é `[nao verificado]`.

**Resolver dia / dono fora do fio**

- A regra escrita proíbe “Resolver dia” na competência corrente; seu caminho declarado é chamado → pergunta → colaborador. O calendário escreve `pode_ultimo_recurso` somente quando `data < início_competência_atual` e não existe pergunta viva respondida. O juiz exige também passado e status `falta|irregular|aberto|sem_certidao`. `app/colaboradores/services/calendario.py:455-467`, `:738-739`, `:94-104`.
- Isso explica, contra os campos fornecidos, as quatro folgas antigas col574: `status_dia=folga` não satisfaz o juiz; seus dezesseis dias 21/10–05/11 e o novo 05/11 col950 estão na competência corrente fornecida, bloqueando último recurso. A identidade entre `status_dia` da célula e status efetivo do calendário deve ser validada pelo resultado real já impresso `resolver_dia_oferecido=False`; os dois campos não devem ser simplesmente equiparados. `app/colaboradores/services/calendario.py:103-104`, `:738-739`; campos medidos sem arquivo da sonda `[nao verificado]`.
- `dono_fora_do_fio` lê, nesta ordem: Resolver oferecido; ausência cobrindo em ABERTAS; pergunta VALIDADA de aceite de folga; dia presente na lista do admin. Sem nenhum, devolve None. Nunca consulta diretamente o dono E6; o dono vem da lista. `app/chamados/services/vigia_fio.py:274-315`.

**Emissores candidatos**

| Candidato | Guarda / aplicação |
|---|---|
| `detectar_ausencias` | Seleciona `colaboradores_em_operacao`; esta consulta exige ativo, empresa ativa, usuário CPF, empresa em rollout e vínculo cobrindo hoje. Col574 desligado é excluído hoje; col950 ativo só comprova uma das condições, demais `[nao verificado]`. `app/ponto/management/commands/detectar_ausencias.py:98-99`; `app/colaboradores/queries.py:25-29`, `:50-65`. |
| Mesmo detector | Só percorre hoje e ontem; folga é pulada; não cobra após `dia_encerrado`, pois dali em diante atribui ao cartório. Portanto não repara retrospectivamente os dias antigos. `app/ponto/management/commands/detectar_ausencias.py:136-141`, `:305-306`, `:460-473`. |
| Funil `_abrir_chamado` | Celula veto/nunca_bateu é perguntado em `celula_veta_cobranca`; sem célula física tem outro aviso `dia_sem_grade`. Col950 **tem** CelulaDia, então motivo folha `sem_celula` não autoriza afirmar que este ramo de aviso dispararia. `app/ponto/management/commands/detectar_ausencias.py:630-668`. |
| Adesão | Seu universo requer ativo + célula `nunca_bateu`; exclui quem já bateu/isento e tem guarda expressa desligado. Col574 é desligado; col950 não tem veredito nunca_bateu e tem batidas. `app/chamados/services/adesao.py:25-40`, `:60-70`. |
| Turno aberto corrente | Lê turnos materializados de até 36h, exige aberto e entrada; col574 zero batidas não pode produzir entrada real. A presença/ausência de turno materializado do col950 não foi impressa `[nao verificado]`. `app/ponto/management/commands/processar_alertas_turno.py:46-79`, `:88-93`. |
| Turno aberto avançado | Só ontem, exige em_uso, vínculo e horas de turno aberto; não é detector de dia inteiro sem nenhuma batida. `app/ponto/management/commands/processar_alertas_avancados.py:20-38`, `:49-65`. |
| Turno aberto retroativo | Exige turno aberto e entrada com timestamp, depois ≥24h. Nada disso foi medido como turno no anexo; inexistência de batidas no recorte não deve substituir o juiz do turno. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88-103`. |
| Furo retroativo | Filtra ativo e exige alguma batida na janela; col574 excluído pelas duas condições. No col950 exige que `classificar_dia` devolva FURO_SEM_COBRANCA; sem executar sua classificação, emissão concreta `[nao verificado]`. Também exige dia trabalho sabido e marco inicial. `app/ponto/management/commands/emitir_furo_retroativo.py:52-56`, `:101-112`, `:130-145`. |
| Chamado de ausência | Recebe registro de ausência concreto; é aviso persistente de ausência criada pelo colaborador, não criador automático de falta por ausência de batida. Célula `ausencia_id=None` não é prova de inexistência de toda ausência cobrindo o período `[nao verificado]`. `app/ponto/services/ausencia.py:621-646`. |
| Disparo de perguntas, fio mudo | Universo é chamado VIVO ligado à célula; com zero chamados da célula não entra. Não-ativo também é barrado expressamente. `app/chamados/management/commands/disparar_perguntas_competencia.py:141-150`, `:169-171`. |

**Mandatos a enfrentar na classificação**

- O cartório declara julgar toda célula passada sem carimbo, sem filtro de situação; o lote seleciona todos colaboradores com células passadas e passa suas células a `julgar_colab`. Logo há regra escrita responsável por julgar col950; não é defensável dizer apenas “nenhuma lei manda julgar”. Falta saber execução, limite, retornos e exceções para provar por que ele permaneceu com tudo NULL. `app/ponto/management/commands/processar_cartorio.py:1-13`, `:45-65`.
- Há corte explícito “lâmpada acesa no marco = fio vivo obrigatório”, que manda censar o emissor que pulou e curar nele; há também corte incluindo dias sem fio no Admin e declarando o alarme >0 bug provado. Entretanto o caso desligado/nunca_bateu possui guardas expressas de silêncio e RH, então é necessário confrontar escopo antes de escolher “lei faltando”. `app/docs/CORTES.md:263`, `:265`; `app/chamados/services/adesao.py:66-67`; `app/chamados/reconciliador.py:804-811`.
- **Causa histórica comum aos 21:** o retrato atual permite identificar que 17 são da competência corrente e quatro são folgas antigas, com impedimentos distintos de Resolver dia; não demonstra o evento que fez 20 col574 entrarem desde 05/11. Estado anterior, data efetiva do desligamento e execução do emissor anterior não foram fornecidos `[nao verificado]`. As guardas atuais estão em `app/colaboradores/services/calendario.py:103-104`, `:738-739`.

## 06/11 09:25 · sessao b33ecbc3dc1f · - · - · origem=Ronald (tmux)

- **O censo enumera causas de retenção, não todo dia afetado pelo bloqueio da competência.** Ele chama `dias_que_seguram`, insere exclusivamente as datas retornadas e só consulta portas para essas chaves; por isso um dia afetado, mas ausente das retentoras, não aparece no universo nem entra no contador sem porta. (`app/chamados/services/vigia_fio.py:577`, `:585`, `:587`, `:635`, `:638`)

- **Isso corresponde ao contrato explícito local:** o comentário define a pergunta como “o dia que SEGURA a folha tem uma porta”; a autoridade das datas é `dias_que_seguram`, e a soma das rotas fecha esse universo. A lei escrita da porta é chamado da célula na fila OU pergunta viva no app com âncora na fila. (`app/chamados/services/vigia_fio.py:493`, `:502`, `:517`, `:531`; `app/docs/CORTES.md:401`)

- **Quem produz e lê a porta:** `alimentar_portas` lê `qs_na_fila`, particiona verbos sobre **todos** `qs_vivos`, carrega perguntas da célula e aplica `pergunta_viva`; a leitura do app é sob demanda. `porta_alcancavel_do_dia` monta uma resposta em memória com rota, fila visível, verbos e fio; seus leitores incluem `fora_da_folha._fio_do_dia` e o censo. (`app/chamados/juizes.py:1282`, `:1287`, `:1290`, `:1300`, `:1302`, `:1319`, `:1375`, `:1380`, `:1386`; `app/folha/fora_da_folha.py:77`; `app/chamados/services/vigia_fio.py:635`)

- **A alimentação deve preservar o universo global dos verbos.** Particionar somente os chamados/células dos casos pode mudar o crédito da pergunta; o comentário explica que destino vivo fora da fila pode vencer o pai e que a autoridade é a lista padrão sem filtros. Não há código da sonda disponível aqui para conferir se ela respeitou isso. (`app/chamados/juizes.py:1267`, `:1271`, `:1287`; disponibilidade integral da sonda: **[não verificado]**)

- **Produção chama o censo com lista e contexto compartilhados:** o command calcula `agora/hoje`, instancia `ListaDoAdmin(hoje, empresas, agora=agora)` e executa `censo_alcance(empresas, hoje=hoje, lista=lista)` dentro de `tranca_em_lote()`. O padrão do censo é `limite=50`, que só limita apresentação de dias com porta/balde legal, sem limitar o universo ou dias sem porta. A saída fornecida com 2.127 linhas/zero omitidas exige limite ampliado ou outra apresentação; a chamada exata da sonda permanece **[não verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:159`, `:163`, `:167`, `:171`, `:173`; `app/chamados/services/vigia_fio.py:542`, `:551`, `:625`, `:628`)

- **Há uma sutileza de relógio:** embora `censo_alcance` receba `hoje`, chama `dias_que_seguram` sem esse argumento; `_dias_que_retem` também chama `motivos_retencao_celula` sem `hoje`. Não concluir divergência da sonda sem seu script; comparar se usou a mesma janela e data efetiva. (`app/chamados/services/vigia_fio.py:585`; `app/folha/fora_da_folha.py:98`, `:118`, `:137`)

- **`porta=s` não prova liberação pelo admin.** A função não recebe usuário e não aplica autorização individual; os endpoints exigem `gerir_chamados` e `recusa_de_area`. Cobrar envia push e registra nota, aguardando resposta do colaborador; Validar pode recusar por implausibilidade ou falta de resposta, e depois materializa/recalcula pela data da pergunta. (`app/chamados/juizes.py:1341`, `:1375`, `:1386`; `app/chamados/views_cobrar.py:24`, `:25`, `:31`, `:44`, `:55`, `:145`, `:149`, `:157`; `app/chamados/services/validacao.py:363`, `:374`, `:396`, `:407`, `:430`)

- **Leitura dos cinco casos:** cobrar significa depender do colaborador e depois validar; validar indica uma oportunidade de ato do admin; decidir exige julgamento humano. Assim col504 e col43 dependem de resposta; col709 mistura validar com três cobrar; col204 tem validar; col502 mistura três cobrar com dois decidir. A conclusão sobre os atos necessários segue os verbos fornecidos, mas sucesso após execução, autorização do admin e reclassificação final são **[não verificado]**. (`app/chamados/services/verbos.py:20`, `:22`, `:80`; `app/folha/fora_da_folha.py:40`)

- **Existe UI mostrando retentoras por colaborador/competência:** o drawer com `mes/ano` busca `RetratoFolha` gravado, localiza a linha e renderiza “Datas (ISO)”. Mostra as datas retentoras da **pessoa**, sem estabelecer nesse bloco uma aresta de um dia afetado específico para suas dependências. Os links são genéricos por fio/tipo, apontando ao fio, vínculo ou cadastro. Se o retrato gravado contém as datas da medição atual, permanece **[não verificado]**. (`app/colaboradores/views.py:973`, `:975`, `:980`, `:982`, `:984`; `app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/folha/_retrato_o_que_fazer.html:43`, `:46`; `app/folha/retrato_folha.py:974`, `:986`, `:1015`, `:1020`)

- **Não afirmar “nenhum leitor mostra datas”.** `o_que_segura` é ferramenta existente que traz as retentoras e o que destrava; o drawer mostra datas gravadas. A afirmação mais estreita é: não identifiquei nesses leitores uma apresentação explícita “dia retido X depende de Y/Z”; uma negativa sobre **todas** as telas do sistema permanece **[não verificado]**. (`app/folha/fora_da_folha.py:203`, `:231`; `app/templates/folha/_retrato_o_que_fazer.html:43`)

- **Classificação sugerida:** exclusão do dia afetado do censo, isoladamente, não prova bug contra o contrato existente, pois o universo escrito é das retentoras. Se a exigência for listar também cada dia afetado e sua relação com as retentoras, essa obrigação precisa ser localizada em regra escrita; não está no corte que define a porta. A garantia de liberação dos cinco casos permanece inconclusiva sem resultado dos atos e reavaliação do export. (`app/chamados/services/vigia_fio.py:493`, `:502`, `:585`; `app/docs/CORTES.md:401`; sucesso final: **[não verificado]**)

## 06/11 09:25 · sessao 651fd7a6e630 · - · - · origem=Ronald (tmux)

O leitor existente mostra **colaborador + competência → datas retentoras**, não uma relação explícita **dia “retido” → outras datas**: a linha gravada contém `colab`, `status`, `motivo`, `dias`, `datas` e `dono`, sem chave para o dia afetado; `montar_linhas` obtém essas datas de `fora_da_folha`. (`app/folha/retrato_folha.py:1156`, `app/folha/retrato_folha.py:1171`, `app/folha/retrato_folha.py:1177`)

A cadeia efetiva da tela é:

- A lista do retrato abre o drawer com colaborador e `mes`/`ano`. (`app/templates/folha/_retrato_lista.html:60`)
- A view exige competência, acesso `ver_folha` e retrato acessível; busca a linha gravada, monta ações e portas. (`app/colaboradores/views.py:975`, `app/colaboradores/views.py:980`, `app/colaboradores/views.py:982`)
- O drawer inclui o bloco “O que fazer” quando existe `retrato_linha`. (`app/templates/colaboradores/partials/painel_colaborador.html:26`)
- O bloco mostra **“Datas (ISO)”** e percorre `retrato_linha.datas`; sem lista, informa que a fonte não deu lista de datas. (`app/templates/folha/_retrato_o_que_fazer.html:42`, `app/templates/folha/_retrato_o_que_fazer.html:46`, `app/templates/folha/_retrato_o_que_fazer.html:49`)
- “O que destrava” vem de `DESTRAVA`, pelo fio agregado da linha. Os links também são por colaborador, escolhidos pelo fio e tipo; não recebem cada data retentora. (`app/folha/retrato_folha.py:1057`, `app/folha/retrato_folha.py:1068`, `app/folha/retrato_folha.py:1015`, `app/folha/retrato_folha.py:1020`)

Logo, seria incorreto dizer **“nenhuma tela mostra aquelas datas”**: o drawer tem leitor para elas. A presença e atualização desses cinco casos no retrato gravado não foram dadas pela sonda. **[não verificado]**

A ferramenta `o_que_segura(colab, mes, ano)` fornece dados ainda mais específicos: data, veredito, motivo, fio, ação e chamados da fila, mas continua sendo uma pergunta por colaborador/competência. (`app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:217`, `app/folha/fora_da_folha.py:225`, `app/folha/fora_da_folha.py:227`, `app/folha/fora_da_folha.py:229`) Não encontrei uma view chamando essa ferramenta; a documentação interna afirma que o drawer a lê, porém a view efetiva lê o retrato gravado. (`app/folha/retrato_folha.py:1218`, `app/colaboradores/views.py:978`, `app/colaboradores/views.py:982`)

Outros leitores relevantes:

- O calendário lê o status diário lavrado pelo D4; o código declara que retenção da folha não decide a palavra da tela. (`app/colaboradores/services/calendario.py:597`, `app/colaboradores/services/calendario.py:602`, `app/colaboradores/services/calendario.py:606`)
- O Raio-X seleciona pessoas pelo export mensal, mas obtém suas datas de `furos_por_dia`, não de `motivos_retencao_celula(com_dias=True)`; o clique abre chamados por colaborador. (`app/folha/views.py:589`, `app/folha/views.py:591`, `app/folha/views.py:616`, `app/folha/views.py:667`)
- A prévia verifica se as datas propostas cobrem **todas** as retentoras e mostra “passaria a entrar” ou “continua fora”; também é efeito por colaborador, não aresta entre dias. (`app/folha/previa.py:119`, `app/folha/previa.py:169`, `app/folha/previa.py:171`, `app/folha/previa.py:173`)

As regras escritas relevantes exigem mesma autoridade para leitores e ação, e coerência com o cadastro existente; não encontrei nelas exigência expressa de apresentar a aresta dia afetado → datas retentoras. (`app/docs/LEIS.md:60`, `app/docs/LEIS-UI.md:23`, `CLAUDE.md:176`) Declarar essa ausência específica como violação provada dessas regras seria **[não verificado]**.

## 06/11 09:25 · sessao cbf261424aa8 · BO-0511-G2-TRIAGEM · censo · origem=Code

**Padrão: n/a (medida).** Para citar o dado fornecido, uso `anexo_dados_g2.txt:N`, contando `== A) relato raspado...` como linha 1, inclusive linhas vazias. É numeração da transcrição colada.

Os status abaixo têm estes significados: `irregular` indica batida faltando, fora do horário ou em ordem inesperada; `furo` integra os vereditos que acusam essa irregularidade. `escala_suspeita`, associada a `indefinida`, expressa dúvida cadastral sobre o dia de trabalho, sem afirmar falta do colaborador. A fonte `lavrado` lê o status persistido na célula. (`app/ponto/status_dia.py:32`, `app/ponto/status_dia.py:38`, `app/ponto/status_dia.py:68`, `app/ponto/status_dia.py:129`, `app/ponto/status_dia.py:143`, `app/ponto/status_dia.py:287`.)

A retenção é outra pergunta: `dias_que_seguram` seleciona a classificação `furo_espelho` e consulta os motivos por dia, independentemente da porta. O retentor registra `espelho_` + veredito acusador quando não incidem as exceções de protestos não cobráveis ou furo somente da pausa. C.d demonstra esse resultado nos cinco dias; protestos e ata individuais não foram fornecidos [nao verificado]. (`app/folha/fora_da_folha.py:95`, `app/folha/fora_da_folha.py:102`, `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`; `app/folha/export.py:1233`, `app/folha/export.py:1253`, `app/folha/export.py:1254`.)

O dono é respondido por **`dono_da_divergencia`**, com insumos montados por **`InsumosDoDono.dono`**. C.b informa o retorno, mas não os argumentos que distinguem os ramos. BATIDA pode resultar de `batida_fora_de_turno` ou contagem ímpar das horas atribuídas pelo oráculo; CADASTRO pode resultar da prioridade folga/vínculo, fase deslocada ou Cadastro×Realidade. O ramo individual exato permanece [nao verificado]. Uma trava `turno_aberto` não demonstra o insumo `batida_fora_de_turno`, que exige também órfã real atribuída ao dia. (`app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:104`, `app/ponto/management/commands/e6_oraculo.py:106`, `app/ponto/management/commands/e6_oraculo.py:108`, `app/ponto/management/commands/e6_oraculo.py:110`, `app/ponto/management/commands/e6_oraculo.py:172`, `app/ponto/management/commands/e6_oraculo.py:196`.)

| Caso | Col | Dia | Classe: vigia / status / motivo do segura | Dono de C e ramo | Obra/lei existente: ID, estado e alcance | Arquivo:linha |
|---|---|---|---|---|---|---|
| **G2-01** | col407 | 21/10/2026 | **B, 05/11:** **a — `chamado_vivo_com_veu`**, detalhe `registrado`. **C, 06/11:** porta nula e #33290 `registrado` satisfazem o véu **se o dia chegar à cadeia a–e**; os baldes anteriores não foram expostos, portanto classificação completa atual [nao verificado]. **Lavra:** `furo` / `irregular`. **Segura:** `espelho_furo`, pela regra de retenção acima. | **BATIDA**; ramo `batida_fora_de_turno` (:104) ou paridade ímpar (:106); qual ocorreu [nao verificado]. | **O261-FIO-MORTO-PELO-RETRATO — ABSORVIDA; “curas no ar”**: escopo expresso de dia retido sem porta, incluindo causa a; alcança a medição B. **L-099 — PELA-METADE**: enquadra o dono informado, BATIDA na lista do admin. | Dado: `anexo_dados_g2.txt:18`, `anexo_dados_g2.txt:31`. Classe: `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:664`, `app/chamados/services/vigia_fio.py:669`. Escopo: `app/docs/BACKLOG.md:963`; `app/docs/LEIS.md:122`. |
| **G2-02** | col836 | 13/10/2026 | **B:** **e — `pergunta_viva_sob_chamado_terminal`**. **C:** porta nula e chamado terminal não informam vitalidade, resposta, texto ou âncora; permanência dessa classe em 06/11 [nao verificado]. **Lavra:** `indefinida` / `escala_suspeita`. **Segura:** `espelho_indefinida`. | **CADASTRO**; prioridade cadastral (:98), fase (:108) ou Cadastro×Realidade (:110); ramo exato [nao verificado]. | **O261-E-CELULA-20-UM-DONO — F0 FECHADA; F1 NO AR; F2 NO AR 01/11; F3 PARADA 02/11**: escopo explícito de pergunta viva sob chamado terminal, demonstrado por B. **L-099 — PELA-METADE**: CADASTRO na lista do admin. | Dado: `anexo_dados_g2.txt:21`, `anexo_dados_g2.txt:32`. Classe: `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:687`. Escopo: `app/docs/BACKLOG.md:859`; `app/docs/LEIS.md:122`. |
| **G2-03** | col836 | 14/10/2026 | **B:** **e — `pergunta_viva_sob_chamado_terminal`**, medido separadamente do dia 13. **C:** classe atual [nao verificado], pelos mesmos campos ausentes. **Lavra:** `indefinida` / `escala_suspeita`. **Segura:** `espelho_indefinida`. | **CADASTRO**; ramos possíveis :98, :108 ou :110; ramo exato [nao verificado]. | **O261-E-CELULA-20-UM-DONO**, com os mesmos estados escritos acima: B demonstra o escopo também neste dia. **L-099 — PELA-METADE**. | Dado: `anexo_dados_g2.txt:22`, `anexo_dados_g2.txt:33`. Classe: `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:687`. Escopo: `app/docs/BACKLOG.md:859`; `app/docs/LEIS.md:122`. |
| **G2-04** | col909 | 16/10/2026 | **B:** colaborador ausente da lista de dias sem porta; rota daquele instante [nao verificado]. **C:** **`porta_fila`**, #31891, verbo **`validar`**, fio **`admin`**; não entra nos ramos a–e, pois a porta antecede essa classificação. **Lavra:** `furo` / `irregular`. **Segura:** `espelho_furo`, mesmo havendo porta. | **BATIDA**; ramo :104 ou :106; qual ocorreu [nao verificado]. | **L-099 — PELA-METADE** alcança o dono medido. **Nenhuma obra específica encontrada com enquadramento demonstrado.** **C20-JUIZ-3 — FECHADA / NO AR** trata de evento em `criar()/recompute_turnos`; a queixa e C não demonstram esse evento causal [nao verificado]. | Dado: `anexo_dados_g2.txt:28`, `anexo_dados_g2.txt:34`. Porta: `app/chamados/services/vigia_fio.py:642`; `app/chamados/juizes.py:1327`, `app/chamados/juizes.py:1344`. Escopo: `app/docs/LEIS.md:122`; `app/docs/BACKLOG.md:1768`. |
| **G2-05** | col292 | 21/09/2026 | **B:** não contém este dia; as causas **d** de 22/10 e 28/10 não se transferem para 21/09. **C:** célula presente, porta nula e #24460 `fechado`; faltam perguntas, âncoras e lista para determinar a–e [nao verificado]. **Lavra:** `furo` / `irregular`. **Segura:** `espelho_furo`. | **BATIDA**; ramo :104 ou :106; qual ocorreu [nao verificado]. | **O261-COMPLEMENTO-[nome]**, estado escrito **“(C) = cura (2) PAI-TERMINAL em copia”; “(B) materializada sem resposta 2.970 a construir”**. A linha cita expressamente **col292 21/09** e define alcance da porta. Vincular as perguntas citadas nessa documentação ao conteúdo atual de C [nao verificado]. **L-099 — PELA-METADE**. | Dado: `anexo_dados_g2.txt:16`, `anexo_dados_g2.txt:17`, `anexo_dados_g2.txt:35`. Classe d: `app/chamados/services/vigia_fio.py:677`; demais condições: `app/chamados/services/vigia_fio.py:673`. Escopo: `app/docs/BACKLOG.md:782`; `app/docs/LEIS.md:122`. |
| **G2-06** | col207 | **sem dia** | A relata outubro retido, novembro apta e dez chamados vivos, incluindo disputa #36736 sem célula. Classe do vigia, lavra e motivo diário do segura [nao verificado]. | C não informa dono; dono e ramo [nao verificado]. | **Nenhuma encontrada com enquadramento individual demonstrado.** A queixa não demonstra ausência de pergunta viva nem evento abrangido por **C20-FIO-MUDO — ABERTA / “construtor aberto”**; aplicação [nao verificado]. | `anexo_dados_g2.txt:10`, `anexo_dados_g2.txt:36`; escopo candidato: `app/docs/BACKLOG.md:1769`. |
| **G2-07** | col332 | **sem dia** | A relata retenção 10/11 e treze chamados vivos de batida ausente/turno aberto. Classe, lavra e motivo diário do segura [nao verificado]. | C não informa dono; dono e ramo [nao verificado]. | **Nenhuma encontrada com enquadramento individual demonstrado.** **C20-FIO-MUDO — ABERTA / “construtor aberto”** tem escopo por evento; chamados vivos, sozinhos, não demonstram sua ocorrência [nao verificado]. | `anexo_dados_g2.txt:11`, `anexo_dados_g2.txt:37`; escopo candidato: `app/docs/BACKLOG.md:1769`. |
| **G2-08** | col446 | **sem dia** | A relata retenção 10/11 e seis chamados vivos de batidas ausentes, incluindo #36459–#36462. Classe, lavra e motivo diário do segura [nao verificado]. | C não informa dono; dono e ramo [nao verificado]. | **Nenhuma encontrada com enquadramento individual demonstrado.** Aplicação de **C20-FIO-MUDO — ABERTA** [nao verificado]. **O261-FIO-MORTO-PELO-RETRATO — ABSORVIDA** cita col446/#11582, mas a ligação com esta queixa sem dia [nao verificado]. | `anexo_dados_g2.txt:12`, `anexo_dados_g2.txt:38`; `app/docs/BACKLOG.md:1769`, `app/docs/BACKLOG.md:963`. |

**F3-BECO-1 não fica confirmado para os dois dias de col836:** a linha existente **F3-BECO-1-VOLTA-3 — NO AR 05/11 17:5x** exige a transição do defeito de folga para `False` e trata do reatamento a pai na fila. Esses insumos não constam de C [nao verificado]. (`app/docs/BACKLOG.md:1775`; `anexo_dados_g2.txt:32`, `anexo_dados_g2.txt:33`.)

**Contagem por dono:** BATIDA **3**; CADASTRO **2**; ESTRUTURA **0**; sem dono informado **3**. **Soma = 8.** (`anexo_dados_g2.txt:31` a `anexo_dados_g2.txt:38`.)

**Contagem “obra existente × nenhuma”, com enquadramento demonstrado:** obra específica **4**; nenhuma obra específica **4** — G2-04 possui apenas o enquadramento pela L-099. **Soma = 8.** Incluindo **obra ou lei**, como na coluna solicitada: existente **5**; nenhuma encontrada **3**. **Soma = 8.** (`app/docs/BACKLOG.md:782`, `app/docs/BACKLOG.md:859`, `app/docs/BACKLOG.md:963`, `app/docs/LEIS.md:122`; dados individuais citados na tabela.)

## 06/11 09:25 · sessao 1a1cffb2681a · - · - · origem=Ronald (tmux)

**Dono não é prova de defeito:** `dono_da_divergencia` devolve `ESTRUTURA` como resíduo, após excluir cadastro, batida e fase; aceita inclusive ausência de horas. [app/ponto/management/commands/e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py:98), [app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:143](app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:143)

O comando E6 real primeiro comprova divergência: lê minutos, descarta ausência de lavra, ambos zero, diferença explicada por intervalo e diferença ≤10 minutos; somente então chama `.dono(data)`. Portanto, chamar `.dono` para um dia meramente retido pelo export responde uma classificação residual; não comprova que esse dia esteja divergente. [app/ponto/management/commands/e6_oraculo.py:330](app/ponto/management/commands/e6_oraculo.py:330), [app/ponto/management/commands/e6_oraculo.py:366](app/ponto/management/commands/e6_oraculo.py:366)

**Argumentos e fontes:** assinatura real `InsumosDoDono(colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`; carrega batidas apuráveis de `[ini−12h, fim+1d+14h]`, turnos de `[ini−1d, fim+1d]`, cadastro do dia, fase lavrada e CxR do espelho/motor. A resposta `.dono(data)` não consulta retenção nem porta. [app/ponto/management/commands/e6_oraculo.py:128](app/ponto/management/commands/e6_oraculo.py:128), [app/ponto/management/commands/e6_oraculo.py:142](app/ponto/management/commands/e6_oraculo.py:142), [app/ponto/management/commands/e6_oraculo.py:172](app/ponto/management/commands/e6_oraculo.py:172)

O E6 real passa janela de fechamento, hoje, `espelho=esp` já lido e um instante comum `agora`; pautas usa a mesma classe sem espelho antecipado. [app/ponto/management/commands/e6_oraculo.py:292](app/ponto/management/commands/e6_oraculo.py:292), [app/ponto/management/commands/e6_oraculo.py:304](app/ponto/management/commands/e6_oraculo.py:304), [app/ponto/management/commands/e6_oraculo.py:316](app/ponto/management/commands/e6_oraculo.py:316), [app/ponto/management/commands/pautas_discordantes.py:81](app/ponto/management/commands/pautas_discordantes.py:81)

**Quem produz/quem lê dono:** o juiz produz o retorno; `InsumosDoDono.dono` monta os argumentos e delega; E6 conta o retorno; pautas grava no item; teto inclui no resultado; Cadastro x Realidade inclui como `dono_do_dia` e usa o juiz para resolver conflitos. [app/ponto/management/commands/e6_oraculo.py:110](app/ponto/management/commands/e6_oraculo.py:110), [app/ponto/management/commands/e6_oraculo.py:183](app/ponto/management/commands/e6_oraculo.py:183), [app/ponto/management/commands/e6_oraculo.py:366](app/ponto/management/commands/e6_oraculo.py:366), [app/ponto/management/commands/pautas_discordantes.py:109](app/ponto/management/commands/pautas_discordantes.py:109), [app/ponto/services/teto_na_lista.py:86](app/ponto/services/teto_na_lista.py:86), [app/escala/services/cadastro_realidade.py:459](app/escala/services/cadastro_realidade.py:459), [app/escala/services/cadastro_realidade.py:932](app/escala/services/cadastro_realidade.py:932)

**Universo causal do censo:** regra escrita exige medir “dias que seguram a folha (`o_que_segura`) sem porta”; implementação enumera precisamente as datas retornadas por `dias_que_seguram`, sem expandir para cada dia exportado como fora. Retido e retentor são perguntas distintas nesse contrato. [app/docs/CORTES.md:379](app/docs/CORTES.md:379), [app/chamados/services/vigia_fio.py:493](app/chamados/services/vigia_fio.py:493), [app/chamados/services/vigia_fio.py:585](app/chamados/services/vigia_fio.py:585)

**Classificação recomendada:** exclusão do dia retido do censo + `ESTRUTURA` isolado não constitui bug provado nem demonstra lei faltando. Para concluir sobre liberação faltam execução do ato, efeitos sobre as datas causais, nova avaliação da retenção e demais requisitos do export; a sonda apresentada não contém esse antes/depois. [não verificado]

A L-099 exige coerência dos leitores, não porta própria em cada dia afetado por bloqueio mensal; a L-120 proíbe trava sem saída por bug, mas a existência de porta causal não demonstra conclusão do fluxo. [CLAUDE.md:176](CLAUDE.md:176), [app/docs/LEIS.md:141](app/docs/LEIS.md:141)

Existe regra “a tela NUNCA difere da folha”, porém o ADR trata status/valor do dia e a sonda não mede tela; não basta citar essa frase para provar violação nos cinco casos. [app/docs/ADR/0002-celula-e-o-dia.md:8](app/docs/ADR/0002-celula-e-o-dia.md:8), [app/docs/ADR/0002-celula-e-o-dia.md:29](app/docs/ADR/0002-celula-e-o-dia.md:29), [não verificado]

_2183 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
