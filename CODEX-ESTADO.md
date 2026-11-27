# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 02:37** · publicado em **27/11/2026 02:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 02:37 · sessao a1bc05e461b2 · CODEX-CONSTROI-MOTOR · censo · origem=Code

### Auditoria de quem perguntou

A volta 10 perguntou expressamente quem escreve e quem lê a cerca e os REDs. Também pediu três mutações isoladas da pergunta. Para a admissão do Opus, o pedido apresentou a triagem do achado 4 da v9 e perguntou se bastava tocar **um** caminho da lista. A citação usada nessa pergunta ainda incluía `models` e `motor`, que já tinham saído da redação vigente. `q.txt:2–4` · `CLAUDE.md:325–330`

Os pares `q.txt` e `r.txt` de `verif1` a `verif10` estão indisponíveis no caminho indicado da cópia. A tabela classifica **a volta 10 pelos anexos raspados**, com linhas contadas a partir do início de cada anexo. As classes para a obra inteira e a triagem das voltas anteriores ficam [nao verificado]. `CLAUDE.md:434–438`

| Achado | Classe na volta 10 | Evidência |
|---|---|---|
| **A — Construtor pode alterar a cerca e os REDs** | **(a) Tema presente, achado não visto** | A pergunta pediu **“QUEM ESCREVE e QUEM LE”** a função e os REDs. A resposta apontou `labels_locais()` como produtora e descreveu as mutações em memória dos testes. Ficou sem resposta quem pode editar os arquivos. O deploy importa a função diretamente, e os caminhos da função e dos REDs ficam fora dos padrões de `PROIBIDOS`. `q.txt:4` · `r.txt:25–27` · `bin/deploy.sh:194–196` · `bin/codex_pergunta.sh:162–176` |
| **B — `MIGRATION_MODULES` fora da guarda de caminho** | **(e) Não perguntado na v10** | **“Ha outro chamador de `call_command`”** pediu o censo de chamadas. A resposta identificou **“a única chamada Python”**. O pedido ficou sem a pergunta sobre quem controla a configuração que o Django usa para localizar migrations. `#G:p_migr` só reconhece o componente `migrations`, e a escrita em settings fica fora dessa guarda. O efeito completo no deploy está relatado no anexo. `q.txt:2–5` · `r.txt:29` · `bin/codex_pergunta.sh:162–176,223–233` · `opus.md:4` |
| **C — Referência `:644` errada** | **(e) Não perguntado na v10** | **“O descarte procede pela letra?”** tratava do requisito de tocar um caminho. **“O `any(...)` implementa essa condição”** foi a resposta. O checklist também citou **“selo sem mordida”**, limitado ao delta, e a resposta examinou os REDs da cerca. A referência foi mantida em `:644`, onde está `REGRAS`. `PROIBIDO` está em `:645`, e o selo exige a presença da string `:644`. `q.txt:3,5` · `r.txt:17,33` · `CLAUDE.md:403` · `bin/codex_pergunta.sh:644–645` · `bin/tests/test_agente_no_modelo.sh:286–288` |
| **D — `models/` vedado ao Codex e insuficiente para admitir Opus** | **(e) Não perguntado na v10** | **“O texto exige que toque UM”** perguntou quantos caminhos bastavam. A resposta confirmou **“pelo menos um caminho”** da lista vigente. Ficou sem pergunta a cobertura dos dois lados para `models/`. `p_models_pkg` barra o Codex e está ausente de `OPUS_CONSTROI_TAGS`. Assim, um pedido Opus cujo `ARQUIVOS:` só toque `models/` é recusado. `q.txt:3` · `r.txt:17–19` · `bin/codex_pergunta.sh:166` · `bin/disjuntor.py:1961,2036–2039,2105–2112` |

O descarte documentado na v10 é o da exigência de tocar todos os caminhos. Ele não sustenta **(b)** para A–D. Os quatro achados têm evidência legível no código, então a ausência de banco ou logs de produção não os põe em **(c)**. `q.txt:3,6` · `r.txt:17–19` · `opus.md:3–6` · `CLAUDE.md:431–438`

**MODO-COMUM [nao verificado].** O Opus final registrou A–D. Para propor a linha de classe **(d)**, falta evidência de que Codex e Opus já tinham deixado passar o mesmo achado. `opus.md:1–6` · `CLAUDE.md:434–442`

## 27/11 02:36 · sessao 74c18fa945ea · E4C-F5-INSTANTE · verificador · origem=Code

Os blobs dos dois arquivos de teste da cópia coincidem com o `diff.txt` anexado (`458df7fc3` e `eb5af3188`). O diff fornecido toca apenas esses arquivos. A identidade do commit `d37d7a1c…` e a comparação integral com `2baf51086` ficam [nao verificado]. O recorte informado foi `Ran 74`, `126 failures`, `0 errors`, com a c1 verde **na base do ensaio**. O verde da c1 sobre a cura e a mordida dos mutantes sobre uma cura verde ficam [nao verificado]. [REDs](/home/ronald/codex_ws/d37d7a1c09882fc6861a0f25f960da101ee5ea02/app/chamados/tests/test_e4c_f5_c4_red.py:1) · [Mutantes](/home/ronald/codex_ws/d37d7a1c09882fc6861a0f25f960da101ee5ea02/app/chamados/tests/e4c_f5_c4red2_mutantes.py:1)

## 1–2. O que fechou e por que cada RED cai hoje?

A coluna **Falha na base** usa `falhas.txt` anexado. Os números antigos vêm de `r.txt`. As referências na tabela apontam para os testes da cópia atual. [nao verificado]

| Achado anterior | Teste e contraprova | Falha na base e fechamento | [nao verificado]
|---|---|---|
| `:36`, par do carimbo | M01 e M02 | Os quatro subcasos obtêm `(True, None)` onde esperam `(False, 'par_ato_coluna')`. Cobertos uma e duas validações, resposta divergente com validação e resposta divergente sem validação. `test_e4c_f5_c4_red.py:86`, `:113` |
| `:59`, falhas das fotos | M03 | As duas exceções injetadas chegam ao `assertRaisesMessage`. A falha vem depois, na comparação do estado em memória e da leitura. Agora cobre também a foto da lavra. `test_e4c_f5_c4_red.py:118`, `:135` |
| `:93`, absorção | M22, M23 e M24 | Nos dois REDs, o PK da irmã falta na lista de recusas. Um diverge só na resposta. O outro adultera a validação no banco depois do carimbo e exige reversão integral. O subcaso íntegro compara o carimbo com `ABSORCAO.ato_em` em T1 após mover o relógio para T2. Só M24 mostra esse desvio de relógio no anexo. `test_e4c_f5_c4_red.py:163`, `:173`, `:195`, `:211` |
| `:117`, horizonte | M04, M05 e controle M28 | Na validação candidata, muda a decisão de `aguarda_validacao` para `devolvida_ao_colaborador`. Na resposta candidata, a decisão pública passa pela comparação e a falha é na **leitura interna** de `respondida_em` futuro. Logo, o RED da resposta ainda prende a prova a essa leitura e ao ponto `foto_da_lavra`. A validação persistida e a compensação têm controles separados. `test_e4c_f5_c4_red.py:222`, `:247`, `:264`, `:291`, `:297` |
| `:160`, falso zero e fixture c1 | M06, M12 e M13 | O comparador erra seis PKs. Vigia retorna A=0/B=0 e estoque A=4/B=3, o alarme só acusa `cego`, e o DRY só lista estoque. O novo RED cobre retrodatação, limpeza, escrita e nascimento em disputa antiga e PKs com dois campos da mesma classe. **A colisão da c1 continua.** Na c1 a pergunta positiva de estoque nasce em 13/11, após o marco de 01/11. O caso `b_nascida` exige novo para a mesma combinação relevante, nascimento pós-marco com instante retrodatado. `test_e4c_f5_c4_red.py:518`, `:556`, `:574`, `:590`, `:602`, `test_e4c_f5_instante_c1.py:49`, `:83`, `cortes_f2_f9_apoio.py:77` |
| `:173` e `:183`, fronteira e classe `a` | M07 e M08 | Faltam quatro recusas de novidade, resposta e validação no marco e depois. Falta também a recusa de reprojeção na classe `a`. A contraprova positiva agora exige que a resposta compensada de outro instante permita inserir a foto histórica da classe `b`. `test_e4c_f5_c4_red.py:336`, `:354`, `:358`, `:371` |
| `:196` e `:205`, conversa e motivo | M09 e M10 | A conversa criada pela porta real tem o dia civil da abertura e entra sem a recusa esperada. Os dois motivos falsos aparecem na trilha gravada. O RED aceita recusa ou reconstrução canônica. A representação `FakeDatetime` no diff de tuplas, por si só, não explica a falha de proveniência. `test_e4c_f5_c4_red.py:386`, `:401`, `:410`, `models.py:4785` |
| `:215`, elegibilidade, origem e emissão | M11, M26 e M20 | O RED de elegibilidade hoje cai porque o carimbo deixa uma linha antiga em competência entregue. O RED dos três escritores cai na foto antiga gravada como `ato`. A resposta pública grava uma linha e emite **dois** eventos. A cobertura ainda permite um carimbo que avance a validação sem gravar a foto histórica, pois o caso entregue só compara a ausência da linha antiga e o instante da resposta. O RED de origem verifica apenas os quatro primeiros campos da linha nova, sem conferir seu `entrada_em`. `test_e4c_f5_c4_red.py:419`, `:443`, `:450`, `:489`, `:495`, `signals.py:252`, `models.py:4755` |
| `:234`, concorrência com entrega | M14, M25 e M27 | Estoque primeiro mostra Colaborador sem Empresa e entrega que conclui. Entrega primeiro mostra a trava errada, linha de estoque gravada e, na tentativa seguinte, recusa por “não reproduz a coluna” em vez de `ENTREGUE`. Essa última recusa é compatível com a linha que o primeiro worker já inseriu. **Ainda passa uma guarda que lê K6 antes da trava e usa essa foto depois.** O teste força o primeiro worker a expirar, depois inicia outra tentativa. Nenhuma tentativa continua após esperar pelo commit. `test_e4c_f5_c4_red.py:675`, `:682`, `:709`, `:723`, `models.py:4473`, `:4801` | [nao verificado]
| `:298`, item 13 | M15–M19 e controle M29 | Na base, as falhas são as fotos de auditoria ausentes na validação vetada, materialização e passivo, por PK. O instante da `RECUSA` falha sob M19. Agora há TXT vigente com FM aberto, TXT invalidado, foto integral do TXT e das batidas e verificação do ator, motivo e antes/depois. Resta o controle inverso, **FM exportado sem TXT vigente**, e a prova da sequência pela entrada pública: M29 parte de validação plantada pela fixture e chama a devolução diretamente. `test_e4c_f5_c4_red.py:758`, `:767`, `:787`, `:818`, `:850`, `:884` |
| 0058 e L-130 | M21 e controle c1 | A 0058 agora cobre empate, `+1 µs` e repetição após marcação correta. O RED independente da 0058, o de compensação e o de devolução ao admin são três controles sem falha própria visível no anexo. O teste da L-130 na c1 já cobra a `REABERTURA` marcada, a limpeza do par e o efeito na tarefa. Reparar uma marcação ampla anterior continua sem prova. `test_e4c_f5_c4_red.py:297`, `:884`, `:914`, `test_e4c_f5_instante_c1.py:234`, `0058_decisao_entrada_do_ato_historico.py:16` | [nao verificado]

Há **mais dois conflitos com a c1**. A c1 exige `ColaboradorNaoTravado` para estoque chamado diretamente sem trava prévia. Os testes de concorrência chamam a mesma `registrar` e esperam que ela adquira Empresa antes do dono. A c1 também exige que `carimbar` conclua de primeira com uma resposta histórica sem linha, enquanto o RED novo aceita recusa íntegra seguida de aplicação explícita e nova tentativa. A c1 verde na base não resolve nenhum dos dois conflitos. `test_e4c_f5_instante_c1.py:173`, `:317`, `test_e4c_f5_c4_red.py:478`, `:631`, `:699`, `:715` [nao verificado]

## 3. Os 29 mutantes mordem sobre a cura?

A tabela é uma previsão estática, condicionada a um RED verde sem mutante e à execução do caminho indicado. `falhas.txt` contém falhas da própria base também sob mutação. M12 e M13, por exemplo, repetem o mesmo placar residual da base. Já M08 acrescenta a falha do controle `b`, M24 mostra T2 onde se esperava T1 e M25 mostra o conflito depois de concluir a trava de Empresa. A execução diferencial dos 29 sobre a cura fica [nao verificado]. `test_e4c_f5_c4_red.py:211`, `:371`, `:583`, `:675`, `e4c_f5_c4red2_mutantes.py:27` [nao verificado]

| Mutantes | Efeito que o RED detectaria e limite da prova |
|---|---|
| **M01, M02** | Alinham em memória a única validação ou a resposta divergente, respectivamente. O subcaso alvo perde a recusa. São contraprovas de sensibilidade por **alteração do insumo**. M01 não implementa a condição direta “só confira quando há duas validações”; o RED cobre essa condição pelo caso de validação única. `e4c_f5_c4red2_mutantes.py:41`, `test_e4c_f5_c4_red.py:90` |
| **M03, M04, M05** | M03 repõe a validação no `finally` se a cura continuar chamando `foto_da_lavra`. M04 e M05 injetam validação ou resposta futura no carregador, desde que a candidata chegue ali com o campo e o carimbo preenchidos. Uma cura que isole o candidato da carga pública pode tornar o ponto inatingível ou deixar o próprio RED vermelho por depender da foto. `e4c_f5_c4red2_mutantes.py:61`, `:83`, `test_e4c_f5_c4_red.py:127`, `:229` |
| **M06, M12, M13** | M06 derruba as classificações retrodatadas e limpas. Com o comparador curado, M12 faria o vigia contar só dois `a` novos e dois de estoque. M13 faria a mesma troca no DRY. M12 e M13 exigem que o trecho literal de agregação continue existindo. Uma cura que reutilize uma agregação comum pode fazer o mutante falhar na preparação, antes da asserção de negócio. `e4c_f5_c4red2_mutantes.py:104`, `:195`, `:208`, `:215`, `test_e4c_f5_c4_red.py:574` |
| **M07–M11** | M07 insere validação nova, M08 barra a foto `b` elegível, M09 insere a conversa, M10 regrava o motivo falso e M11 insere payloads indevidos. Esses efeitos atingem as asserções se chegarem à porta. M07/M09/M11 pulam a porta com `bulk_create`. M10 depende de a foto canônica ser aceita. Como o teste monta o payload pela própria `linhas_da_projecao_sem_linha`, filtrar o caso proibido na origem pode deixar a lista vazia e derrubar o RED sem mutante. `e4c_f5_c4red2_mutantes.py:124`, `:140`, `:153`, `:168`, `:182`, `test_e4c_f5_c4_red.py:56`, `:330`, `:386` | [nao verificado]
| **M14, M25, M27** | M14 antecipa a trava do dono. M25 solta Empresa via rollback do savepoint e fabrica a tentativa no dono no worker de entrega. M27 é **a mesma função de M11**, com outro teste alvo. Ela pula inserção protegida e K6 por inteiro. As asserções de trava e de gravação pegam esses efeitos, mas nenhum dos três injeta especificamente K6 lido cedo e reutilizado depois da espera. `e4c_f5_c4red2_mutantes.py:224`, `:370`, `:446`, `:493`, `test_e4c_f5_c4_red.py:689`, `:715` | [nao verificado]
| **M15–M21** | M15 desliga a guarda com FM aberto, mas conserva K6 quando o FM é exportado. M16 esvazia auditoria via `LogAuditoria.save`, portanto precisa que a cura passe por `save`. M17 e M18 alteram TXT e batida após o retorno bloqueado. M19 desloca a `RECUSA`. M20 duplica chamadas de `acordar_celulas`, se esse ponto continuar no caminho. M21 exclui o empate se o literal da 0058 continuar presente. São efeitos distintos que as asserções visadas conseguem observar. `e4c_f5_c4red2_mutantes.py:239`, `:252`, `:264`, `:281`, `:290`, `:300`, `test_e4c_f5_c4_red.py:767`, `:798`, `:873`, `:939` |
| **M22–M24, M26, M28, M29** | M22 alinha a resposta divergente no banco. M23 neutraliza a adulteração injetada no `carimbar`, o que deve tirar o PK das recusas. M24 força T2 no carimbo e no ato. M26 troca a origem histórica após as três portas. M28 remove o avanço do compensador. M29 apaga a resposta na devolução. Cada efeito tem asserção correspondente, mas M22/M23 alteram a **prova disponível à guarda** e M24 força o parâmetro, sem testar uma mudança real do ponto de leitura do relógio. `e4c_f5_c4red2_mutantes.py:309`, `:327`, `:352`, `:397`, `:498`, `:507`, `test_e4c_f5_c4_red.py:163`, `:216`, `:309`, `:487`, `:905` |

**Arnes.** `test_mordida` chama o método original com a mesma instância de teste. As falhas de `subTest` chegam ao resultado, e as exceções inesperadas sobem. O arnes também deixa passar qualquer falha da base como resultado vermelho e não exige que tenha falhado a asserção visada. M23 restaura as linhas só no retorno normal. M25 e M26 entram nos patches antes de entregar o `ExitStack` externo, o que pode deixar patch ativo se a instalação falhar a meio. Em M11, uma inserção indevida fica visível aos subcasos seguintes do mesmo método até o rollback do `TestCase`. Portanto, a restauração completa em todos os caminhos e a atribuição automática da mordida falham na inspeção. `e4c_f5_c4red2_mutantes.py:27`, `:182`, `:317`, `:337`, `:343`, `:391`, `:409`, `test_e4c_f5_c4_red.py:419` [nao verificado]

## 4. Quem escreve, quem lê e qual relógio vale?

| Campo ou saída | Escrita, leitura e significado observado |
|---|---|
| `DecisaoPergunta.ato_em` | As portas de resposta, validação, carimbo, ausência e compensação fornecem o instante. `registrar` insere e ajusta os compensadores pelo último ato. `leitura` reduz por instante e PK, com precedência própria do passivo sintético. A tarefa inclui `ato_em <= agora`. `absorcao_vigente` exige igualdade com o carimbo para identificar a absorção e usa **PK de inserção** para saber se uma recusa ou reabertura veio depois. Trocar essa ordem por `ato_em` mudaria o significado desse leitor. `disputa_emissao.py:1529`, `:1565`, `validacao.py:171`, `models.py:3477`, `:4558`, `:4663`, `:4710`, `materializacao.py:1129`, `tarefa_do_dia.py:390` |
| `entrada_origem`, `entrada_em` | 0057 dá origem padrão `ato` e `Now()` do banco à entrada. A porta exige `entrada_em=marco` explícito para estoque e veda entrada explícita no ato novo. 0058 reclassifica linhas com `entrada_em <= marco`. A entrada do estoque é uma data convencional da migração. O comparador usa a **origem** da linha efetiva para `a` e usa o instante projetado com `>= marco` para novidade da coluna. As duas fronteiras são inclusivas e recaem sobre campos diferentes. `0057_decisao_entrada_do_ato.py:16`, `0058_decisao_entrada_do_ato_historico.py:16`, `models.py:4395`, `:4589`, `:4612`, `:4770` |
| `respondida_em`, `validada_em` | Resposta humana, automática, pela trilha e carimbo projetam resposta. Validação pública e carimbo projetam validação. Nova resposta, devolução, recusa, reabertura e L-130 limpam os campos conforme o ato. C21 pede instante **e** texto; C6 lê a projeção temporal; a face SQL de validação usa instante e PK para linhas reais e `>` para limpar passivo sintético. O comparador usa linhas reais, enquanto `leitura` sintetiza passivo. `verbos` também lê a coluna para contar o **dia civil local** da resposta. Há uma ambiguidade concreta: o cadastro de juízes reserva o ato de validar à porta pública, mas `carimbar` também emite `VALIDACAO` e a tarefa trata ambas como validação. `JUIZES.md:38`, `:102`, `disputa_emissao.py:1531`, `:1661`, `:1767`, `validacao.py:171`, `:307`, `:481`, `models.py:3484`, `:3726`, `:4530`, `:4593`, `juizes.py:1476`, `verbos.py:333`, `tarefa_do_dia.py:173` |
| Carimbo e lavra | `_escrever_carimbo` atribui `materializada_em`, autor e via; as portas de carimbo e descarimbo persistem. C6 corta carimbo posterior ao horizonte e preserva identidade nas vias previstas. `lavrar` escreve veredito, `julgado_em` e `janela_expira_em`; o backfill também preenche a lavra. Tarefa e raio-X leem a vigência. `julgado_em` é instante, aceito no empate com `agora`; expiração é **dia local**, vencido na igualdade se a lavra for de dia anterior. `models.py:3420`, `:3524`, `:3635`, `juizes.py:1486`, `veredito_lavrado.py:107`, `:213`, `:254`, `backfill_veredito_lavrado.py:139`, `tarefa_do_dia.py:436` |
| Vigia, DRY e `tarefa_do_dia` | São leitores. Vigia e DRY usam o mesmo comparador e hoje repetem a regra “algum campo novo” por PK e classe. O alarme publica A/B novos. O DRY usa K6 pela janela do **dia do fato**. `competencia_do_fato` aceita qualquer `data_evento` preenchida, inclusive a data civil de conversa, enquanto a tarefa delimita fato por `conversa=False`. A tarefa e a exigência por pergunta usam a mesma carga com `agora` explícito, último ato e lavra vigente; folha, pauta, vigia e beco leem `porta_da_tarefa`. `vigia_ato_x_coluna.py:17`, `:24`, `vigiar_fio_morto.py:201`, `:310`, `estoque_ato_x_coluna.py:22`, `:35`, `:44`, `materializacao.py:803`, `tarefa_do_dia.py:461`, `:545`, `JUIZES.md:81` |

## 5. A lei escrita resolve as três perguntas do construtor?

1. **Qual registro prova a escrita ou limpeza de cada coluna?** A regra do zero novo está registrada no resumo das respostas 1–2. O modelo declara expressamente a falta de `entrada_em` da coluna. O instante da linha, o `applied` da 0057 e a abertura da disputa têm significados próprios. Fonte durável por campo e lei que a eleja ficam [nao verificado]. `test_e4c_f5_instante_c1.py:1`, `RELATO.md:243`, `models.py:1921`, `:4580`, `:4600`
2. **Recusar ou reconstruir o motivo? O carimbo pode aplicar estoque sozinho?** L-007 exige motivo fiel, ator e antes/depois. A descrição escrita do estoque pede aval próprio e competência aberta, e a porta vigente recusa lote misto. Escolha entre recusa e reconstrução fiel, e autorização específica para aplicação automática pelo carimbo, ficam [nao verificado]. A exigência de sucesso imediato na c1 é uma expectativa de teste. `LEIS.md:65`, `estoque_ato_x_coluna.py:1`, `models.py:4761`, `test_e4c_f5_instante_c1.py:317` [nao verificado]
3. **Ato de fato pode nascer no futuro? Como reparar uma 0058 ampla?** L-073 e E2/0514 resolvem a leitura no horizonte. O Contrato V2 resolve o avanço de compensadores. A permissão de nascer no futuro segue como pergunta da E4d. A 0058 define empate e reexecução do recorte correto, com reversão `noop`. Autorização, dados recuperáveis e ocorrência de um UPDATE amplo no deploy ficam [nao verificado]. `LEIS.md:100`, `tarefa_do_dia.py:390`, `PROMPTS.md:2789`, `RELATO.md:698`, `BACKLOG.md:2171`, `0058_decisao_entrada_do_ato_historico.py:16`, `:29` [nao verificado]

## 6. Relógio solto, `except`, juiz novo e allowlist?

- **Relógio.** Os cenários congelam o Python, fixam o marco e passam `agora` para a tarefa. O `Now()` do banco permanece independente. A origem verifica apenas parte da linha nova e a 0058 injeta os instantes de entrada manualmente. Também não há prova nova da virada do dia local na lavra. `test_e4c_f5_c4_red.py:66`, `:489`, `:668`, `:918`, `models.py:4406`, `veredito_lavrado.py:107`
- **Exceções.** Os `suppress(ValueError)` da classe `b` e da proveniência têm controles posteriores de gravação. Em competência entregue, o `suppress` do carimbo aceita qualquer `ValueError` e só verifica parte do efeito. A checagem de `55P03` confere a tabela, o PK e se o SELECT terminou. Os `except` amplos de produção que já existem no carimbo e na lavra são tratados em pontos diferentes. `test_e4c_f5_c4_red.py:380`, `:410`, `:445`, `:675`, `models.py:3490`, `:3528`, `veredito_lavrado.py:261`
- **Juiz e allowlist.** O diff anexado só altera testes, sem registro novo de juiz ou edição de allowlist. As autoridades em uso são as já registradas. O item 13 ainda substitui U60/U103/U163 por conjuntos vazios na fixture, de modo que este ensaio prova o caminho de fora da lista. Um juiz novo na cura exigiria o corte da L-011. `test_e4c_f5_c4_red.py:733`, `:738`, `e4c_f5_c4red2_mutantes.py:1`, `core/juizes.py:311`, `:350`, `LEIS.md:69`

**REPROVADO** — a c1 ainda colide com a novidade por nascimento, a aquisição das travas e a recusa íntegra do carimbo; a concorrência deixa passar K6 lido antes da trava; o RED da resposta futura cai na leitura interna; a mordida diferencial fica [nao verificado] e o arnes tem restauração incompleta. `test_e4c_f5_instante_c1.py:83`, `:173`, `:317`, `test_e4c_f5_c4_red.py:291`, `:556`, `:715`, `e4c_f5_c4red2_mutantes.py:27`, `:343`

## 27/11 02:34 · sessao 7e98225111c0 · - · - · origem=Ronald (tmux)

Conferi os casos em leitura. A execução e a mordida dos mutantes ficam [não verificadas].

- 21/23 devem usar `carimbar_na_fixture`, que adquire e retira do registro a trava da assinatura concorrente. O mock do 23 pertence a `validacao.validar_pergunta`, importado dentro da ação. Os controles elegíveis devem conseguir recusar e corrigir, respectivamente. `chamados/tests/_ato_da_fixture.py:15–27,47–51` · `chamados/services/validacao.py:974–1006` · `chamados/services/acoes_disputa.py:373–376,418–466`
- 27 pode trocar `celula_id` durante a aquisição da trava e exigir que o juiz receba a mudança. O mutante conserva a trava e o SELECT fresco, mas entrega ao juiz a instância anterior. 28 precisa manter a `declaracao` com `ABSORCAO` e usar uma irmã `legado_sem_lastro` como controle alcançável pelo comando atual. `chamados/reconciliador.py:1265–1298` · `chamados/management/commands/encerrar_perguntas_com_fato.py:31–44,66–77`
- 06/07/31/32/37 têm efeitos verificáveis pelas portas existentes. O formato e a superfície pública da lista de lápides são pendências de desenho. `core/portas.py` declara arquivos por entidade. Para 32, a porta pública ainda aceita assinaturas via `**campos` e as grava ao nascer, o que permite um contraste concreto com a E4D quando o contrato alcançar esse caminho. `docs/PROMPTS.md:2873,2950` · `docs/RELATO.md:292` · `core/portas.py:396–447` · `chamados/models.py:3088–3103,3330–3336,3703–3709`
- 15/19/22 precisam de controles elegíveis que passem pelo mesmo escritor. 24 deve conferir também o estado de E e aceitar qualquer ordem na lista de recusadas. 36 precisa de irmã viva fora da competência, em D ligada ao chamado-pai. 43 pode capturar ausência de reescrita no retry feito em T2. 46 deve usar a relavra real da célula sobre batidas apuráveis. `chamados/services/validacao.py:417–487,742–787` · `chamados/services/disputa_emissao.py:1741–1764` · `chamados/services/materializacao.py:1119–1149` · `chamados/services/competencia_trancada.py:162–183` · `chamados/models.py:2094–2100` · `ponto/services/cartorio.py:1200–1209,2078–2115`
- O selo de ordem precisa vincular a DP pelo par pergunta/ato e a escrita à mesma pergunta. O selo de escritor deve observar a origem de todas as escritas daquela operação, inclusive uma segunda atualização do mesmo valor. O censo existente entende aliases e gerentes reversos. `chamados/tests/test_e4d_matriz_reds.py:49–87` · `core/censo_escritas.py:372–439,451–466`

## 27/11 02:34 · sessao 1c3d1ffa49e8 · PAR-PELO-TURNO · leitura · origem=Code

# BRIEF ao OPUS construtor · PAR-PELO-TURNO · CODEX 5(d)

**Cópia indicada** `c1v5a2`, hash `5b5d5f9781b0ce5294bab63d22e512da00558108`, base `794ab3332`. Conferi os arquivos da cópia entregue. Identidade Git de HEAD e base, contagem das sete voltas e execução do recorte de 6.935 testes [nao verificado]. O escalonamento após mais de três voltas está na seção CODEX 5(d). `CLAUDE.md:421`, `CLAUDE.md:428`

**Regra da volta.** O corte V6 mantém o separador de pausa por classe, transforma o excedente a previsto + 4 h em C9 e conserva o limite absoluto de 26 h. A LEI1 mantém no mesmo juiz a atribuição da órfã que pertence à véspera. Os REDs abaixo dão o resultado pela regra antes da construção. O DIFF mede impacto por rubrica. `app/docs/BACKLOG.md:2027`, `app/docs/PROMPTS.md:2852`, `app/docs/PROMPTS.md:3025`, `app/docs/LEIS.md:136`, `app/docs/LEIS.md:154`

**Pergunta de lei antes do RED 4c.** `lei: com previsão comercial, sem ausência e sem fato pertencente à competência, qual saldo deve ser lavrado: 0,00 h ou o saldo da previsão, −193,60 h no ensaio?` A L-085 decide o dia da jornada. A L-095 decide a fonte do valor lavrado. O valor devido nessa situação fica [nao verificado]. O RED já pode exigir que uma marca exclusivamente de contexto deixe o resultado da competência idêntico. `app/docs/LEIS.md:113`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:136`, `app/ponto/services/fechamento.py:645`, `app/ponto/services/fechamento.py:1103`

## 1. O dia do mesmo PK varia com a janela — confirmado

- **Conferência e origem.** A carga chega a `fim+2`, mas `_turnos_do_colab_puro` só cria segmentos até `fim` e só conserva julgamentos cujo `data_turno` cai no segmento. A projeção inclui as marcas da direita, restringe quais turnos podem reclamar seus PKs e preenche as chaves restantes pelo dia civil. Há ainda a variante `turnos=`, que substitui os julgamentos a partir de `ini`. Reproduzi em memória a saída com dia **22/10** na consulta do dia 20 e **21/10** nas consultas dos dias 21 e 22. A origem é a combinação entre o alcance do julgamento e a publicação do mapa. `app/ponto/turnos.py:1858`, `app/ponto/turnos.py:1990`, `app/ponto/turnos.py:2023`, `app/ponto/turnos.py:3027`, `app/ponto/turnos.py:3066`, `app/ponto/turnos.py:3072`, `app/ponto/turnos.py:3077`
- **Regra e RED 1a.** L-085 para o dia julgado, L-037/L-038 para o recorte de apresentação. Em vínculo único com DNA `21:30–05:42`, `E 21/10 21:30` e `S 22/10 05:42`, os dois PKs valem **21/10** nas consultas de 20, 21 e 22, quando ambas as marcas estão carregadas. Cobrir casca, lote, consulta individual da saída e variante `turnos=`. O cartão encerrado em 20/10 fica sem essas duas marcas. `app/docs/LEIS.md:85`, `app/docs/LEIS.md:86`, `app/docs/LEIS.md:117`, `app/docs/CORTES.md:328`, `app/docs/CORTES.md:341`, `app/ponto/turnos.py:3048`, `app/ponto/turnos.py:3173`
- **RED 1b.** Manter a contraprova col297. No 12x36 `22:00–06:00`, `E 20/09 22:56` fica em 20/09 e aberta. `E 21/09 02:26` e `S 21/09 03:26` têm chave em **21/09** e ficam fora do cartão encerrado em 20/09. Esse teste sozinho aceita a resposta civil para as duas pontas. O RED 1a exige que a saída de outro dia civil siga a jornada. A O73 trata de volta de pausa da mesma jornada. `app/relatorios/tests/test_pdf_sem_regra_propria.py:111`, `app/relatorios/tests/test_pdf_sem_regra_propria.py:127`, `app/relatorios/tests/test_pdf_sem_regra_propria.py:135`, `app/ponto/tests/test_par_pelo_turno_bordas.py:44`, `app/docs/BACKLOG.md:1496`
- **Quem lê.** Espelho e API mensal filtram pelo valor do PK. O PDF recebe as linhas do espelho. Painel, férias e vigia consultam PKs específicos. A consulta individual abastece disputa, fio, serviço de jornada e par-relâmpago. `app/ponto/services/espelho.py:465`, `app/ponto/services/espelho.py:588`, `app/api/views.py:1444`, `app/api/views.py:1459`, `app/api/views.py:2042`, `app/relatorios/pdf_espelho.py:596`, `app/core/services/painel_op.py:330`, `app/ferias/management/commands/listar_ferias_com_batida.py:54`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`, `app/chamados/services/disputa_emissao.py:283`, `app/chamados/services/fio.py:1827`, `app/escala/servico_jornada.py:509`, `app/ponto/services/par_relampago_na_lista.py:101`

## 2. A pré-passagem troca a entrada que governa a pausa — confirmado

- **Conferência e origem.** Sem marcos de borda, `_marca_intra_duracao` reinicia as 26 h em qualquer `E` gravada, inclusive a marca de intervalo. A pré-passagem roda antes da leitura da ata. O laço principal conserva o relógio da entrada efetiva e descarta as marcas carimbadas fora desse limite. Reproduzi em memória o carimbo indevido em `08:00→10:00` e a perda do par `10:00→14:00`. `app/ponto/turnos.py:407`, `app/ponto/turnos.py:419`, `app/ponto/turnos.py:1223`, `app/ponto/turnos.py:1318`, `app/ponto/turnos.py:1381`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1459`
- **Regra e RED 2.** L-087 dá prioridade ao papel da ata, a seção 6 exige teto por instante e o V6 preserva o separador por classe e o limite de 26 h. No modo duração da classe Base, sem bordas, usar `E 17/10 20:30`, `E 18/10 22:00` marcada como intervalo, `S 19/10 08:00`, `E 19/10 10:00`, `S 19/10 14:00`. O limite do turno de 17/10 é **18/10 22:30**. O turno de 17 fica aberto, a saída das 08:00 fica órfã e `10:00→14:00` fecha em 19/10. Cobrir também a marca de intervalo indicada pela ata e o controle de pausa legítima `E 20/10 19:00`, `S 21/10 01:00`, `E 21/10 02:00`, `S 21/10 07:00`, uma jornada com 60 min de pausa. `app/docs/LEIS.md:119`, `CLAUDE.md:192`, `app/docs/PROMPTS.md:2852`, `app/ponto/tests/test_par_pelo_turno.py:419`
- **Quem lê.** O laço principal consome `_intra_dur`. Os leitores de intervalo e realizado consomem os carimbos no turno. O corpo abastece casca, lote e pergunta viva. O motor também alcança o pareador por `turnos_de_batidas`, com sua própria alimentação. O provedor de última batida apurável pode provar fecho posterior fora da janela e precisa entrar no controle. `app/ponto/turnos.py:1334`, `app/ponto/turnos.py:551`, `app/ponto/turnos.py:713`, `app/ponto/turnos.py:804`, `app/ponto/turnos.py:1161`, `app/ponto/turnos.py:1920`, `app/ponto/turnos.py:1545`, `app/ponto/turnos.py:3157`, `app/ponto/motor_calculo_v2.py:909`, `app/chamados/services/carga_pergunta_viva.py:165`

## 3. PDF, leitor noturno e testemunhas conservam recortes próprios — confirmado

- **Conferência e origem.** A fonte da carga é `janela_com_pad` com `batidas_apuraveis_na_janela`. O espelho e o fechamento a usam. O coletor do PDF cria outra lista, encerrada ao meio-dia de `fim+1`, e a entrega à mesma `apurar_por_fatia`. Ela ainda alimenta o total do PDF em período livre. O leitor noturno consulta desde D−1 e só aceita `data_turno` ou `data_turno+1`, mesmo com `cross_meianoite=True`. A testemunha da folha pede à autoridade até `fim+1` e recorta só depois. O resumo do espelho volta à data civil da entrada em `somar_periodos`. Esses desvios nascem nos respectivos recortes. O dia da jornada segue nascendo no juiz. `app/ponto/turnos.py:1858`, `app/ponto/turnos.py:2989`, `app/ponto/services/espelho.py:1032`, `app/ponto/services/fechamento.py:638`, `app/relatorios/pdf_espelho.py:264`, `app/relatorios/pdf_espelho.py:419`, `app/relatorios/pdf_espelho.py:502`, `app/relatorios/pdf_espelho.py:628`, `app/escala/servico_jornada.py:249`, `app/escala/servico_jornada.py:258`, `app/folha/porta_export.py:77`, `app/folha/porta_export.py:114`, `app/ponto/services/espelho.py:83`, `app/ponto/services/espelho.py:453`
- **Regra e REDs.** L-002, L-025, L-037/L-038, L-085 e L-095 regem a fonte, o universo e o recorte. **3a:** vínculo noturno único, DNA `23:30–01:30`, `E 20/10 23:30 → S 22/10 01:30`. No PDF de período livre encerrado em 20/10, ambas as pontas chegam à apuração e o total lê a única jornada de 20/10. **3b:** na consulta noturna pela saída de 22/10, o par completo é reconhecido e a própria saída fica sem cobrança órfã. **3c:** no mesmo caso, a testemunha da folha pede o período até 20/10, recebe a saída pelo contexto e compara o par no dia 20. **3d:** DNA de 20/09 `23:50–07:50`, `E 21/09 00:00 → S 21/09 07:50`. Com o motor como fonte do resumo, a soma encerrada em 20/09 inclui uma vez o período julgado para 20/09. `app/docs/LEIS.md:60`, `app/docs/LEIS.md:76`, `app/docs/LEIS.md:85`, `app/docs/LEIS.md:86`, `app/docs/LEIS.md:113`, `app/docs/LEIS.md:117`, `app/docs/CORTES.md:328`, `app/ponto/turnos.py:247`, `app/escala/servico_jornada.py:364`, `app/relatorios/cartao_pela_celula.py:321`
- **Quem lê.** `apurar_por_fatia` recebe a carga de fechamento, espelho e PDF. A passada dos órfãos também consulta o helper. `classificar_falta` lê o resultado noturno. `TurnoMaterializado` lê as pontas, o dia e `cross_meianoite`. `DiaPago`, a testemunha do export e o calculador de período consomem o dia publicado por período. `app/ponto/services/fechamento.py:756`, `app/ponto/services/fechamento.py:811`, `app/ponto/services/espelho.py:1171`, `app/relatorios/pdf_espelho.py:419`, `app/escala/servico_jornada.py:364`, `app/ponto/nucleo.py:47`, `app/ponto/services/dia_pago.py:174`, `app/folha/porta_export.py:106`, `app/ponto/calculador/periodo.py:596`

## 4. A fatia perde o par e a carga pode mover horas e saldo — confirmado na leitura

- **4a · Origem na fatia.** `apurar_por_fatia` classifica as marcas por dia civil local. Na sobreposição, esse filtro pode retirar a saída da fatia antes de o motor receber a lista. No anexo, A perde a saída, B recebe as pontas e o motor de B exclui o dia 20. A composição da sonda foi de **0 h**, com **26 h** no controle de uma fatia. **RED:** dois vínculos noturnos sobrepostos, células de 20 e 23/10 geradas por A e de 21 e 22/10 por B, `E 20/10 23:30 → S 22/10 01:30`. Pela L-085, a jornada inteira pertence a A no dia 20. Exigir um par fechado de 1.560 min de geometria, uma única apropriação no dia 20 e totais iguais ao controle de uma fatia para o mesmo cadastro e régua. O teste de carga atual confere presença de PKs antes do descarte. O teste de 26 h roda com um vínculo. O teste com motor falso preserva a invariância UTC/−03 e mede a lista recebida. `app/docs/LEIS.md:117`, `app/docs/LEIS.md:136`, `app/ponto/services/fechamento.py:228`, `app/ponto/services/fechamento.py:270`, `app/ponto/services/fechamento.py:276`, `app/ponto/services/fechamento.py:302`, `app/ponto/motor_calculo_v2.py:2542`, `app/ponto/tests/test_par_pelo_turno.py:225`, `app/ponto/tests/test_par_pelo_turno.py:646`, `app/ponto/tests/test_composicao_de_fatias_e_total.py:279`, `app/ponto/tests/test_composicao_de_fatias_e_total.py:316`
- **4b · Origem no motor.** A herança de até 6 h sobrescreve o dia já obtido do juiz, antes do filtro da competência. A carga maior passa a acionar essa herança no ensaio do anexo. **RED:** corte 21, `E 19/10 22:00 → S 20/10 22:00`, depois `E 21/10 03:00 → S 21/10 11:00`, DNA `03:00–11:00` no dia 21. Pela L-085 e pelo separador Base do V6, são jornadas distintas. O par de oito horas fica em **21/10, competência 11**, com as mesmas oito horas nas condições de cálculo da sonda, com ou sem o primeiro par inteiro na carga. O teste deve preservar a cauda de uma pausa efetiva do RED 2. `app/docs/LEIS.md:117`, `app/docs/PROMPTS.md:2852`, `app/ponto/janelas.py:15`, `app/ponto/services/fechamento.py:638`, `app/ponto/motor_calculo_v2.py:2516`, `app/ponto/motor_calculo_v2.py:2521`, `app/ponto/motor_calculo_v2.py:2542`
- **4c · Origem no fechamento.** O próprio fechamento escreve `sem_movimento` a partir da lista com contexto e usa esse sinal para o saldo e para uma inconsistência. Na sonda do anexo, o motor devolveu zero períodos e `−193,60 h` nos dois alcances. A atribuição do fechamento mudou de **0,00 h para −193,60 h**. **RED:** comercial seg–sex, corte 21, período `21/09–20/10`, sem ausência. Comparar lista vazia com somente `E 22/10 12:00`, sem jornada reclamada pelo período. Exigir zero períodos nos dois braços e igualdade do saldo e do indicador de movimento da competência. O valor absoluto segue a pergunta de lei do topo. `app/docs/LEIS.md:113`, `app/docs/LEIS.md:117`, `app/ponto/services/fechamento.py:638`, `app/ponto/services/fechamento.py:645`, `app/ponto/services/fechamento.py:1103`, `app/ponto/motor_calculo_v2.py:3542`, `app/ponto/motor_calculo_v2.py:3572`
- **Quem lê.** A composição mescla `dia_da_jornada`. O fechamento usa o resultado para escrever `FechamentoMensal` e passa os períodos, o mapa e o saldo à lavra de `DiaPago`. O banco entra na linha de ajuste de `DiaPago`. O cartão lê os totais do fechamento ou da lavratura conforme a fonte disponível. `app/ponto/motor_calculo_v2.py:2573`, `app/ponto/services/fechamento.py:317`, `app/ponto/services/fechamento.py:1159`, `app/ponto/services/fechamento.py:1179`, `app/ponto/services/fechamento.py:1200`, `app/ponto/services/fechamento.py:1207`, `app/ponto/services/fechamento.py:1216`, `app/ponto/services/dia_pago.py:352`, `app/relatorios/cartao_pela_celula.py:321`

## 5. O selo aceita ata vazia — confirmado

- **Conferência e origem.** `papel_por_minuto_da_ata` produz o mapa e `_turnos_do_colab_puro` o entrega na chamada que pareia. O selo AST cobra o nome do argumento e a forma da chamada direta. O mutante oficial troca a chamada da casca por um alias e mantém a entrega da ata. Reproduzi em memória a diferença que importa: o mapa entregue fecha o par e o mapa vazio deixa dois abertos. O resultado agregado dos nove alvos pertence ao anexo [nao verificado]. `app/ponto/turnos.py:1575`, `app/ponto/turnos.py:1969`, `app/ponto/turnos.py:2023`, `app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:183`, `app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:199`, `app/ponto/tests/test_par_pelo_turno_bordas.py:63`
- **Regra e RED 5.** L-087 e L-110. DNA `21:30–05:42`, `E 20/10 21:30` e marca de `21/10 05:42` gravada como `E`, com papel `S` na ata. Pela entrada normal da casca e do lote, exigir exatamente um turno fechado em 20/10 com os dois PKs e a saída às 05:42. Preservar o tipo gravado da segunda marca. O mutante de ata vazia deve falhar nessa asserção. O alias que preserva a entrega serve de controle semântico. `app/docs/LEIS.md:119`, `app/docs/LEIS.md:136`, `CLAUDE.md:28`, `app/ponto/turnos.py:896`, `app/ponto/turnos.py:1318`, `app/ponto/turnos.py:3157`
- **Quem mais lê.** `_papel_da_ata` resolve o papel por minuto local. `_turno_aberto_puro` e o motor também consomem o mapa produzido a partir da ata. O ponto a provar neste achado é a entrega da alimentação pelo corpo comum. `app/ponto/turnos.py:896`, `app/ponto/turnos.py:2333`, `app/ponto/motor_calculo_v2.py:524`

## 6. Col155 em 08/11 — geometria coberta, laudo nominal em aberto

- **Conferência.** O registro HJ-026 inclui a série de domingos de 18/10 a 15/11. A tabela de fixtures e o ensaio de 08/11 são distintos. O teste de 08/11 usa explicitamente PKs `1` e `2` e pontas de ensaio. PKs, instantes, papéis e DNA congelado do dia real, comprovação nominal dos 29 casos e antes/depois por rubrica [nao verificado]. `app/docs/BACKLOG.md:745`, `app/docs/BACKLOG.md:2027`, `app/ponto/tests/test_par_pelo_turno.py:55`, `app/ponto/tests/test_par_pelo_turno.py:80`
- **Regra e RED 6.** No ensaio com DNA `21:30–05:42`, `E 08/11 17:29 → S 09/11 06:31` dá **782 min**, teto de **732 min**, entrada 241 min adiantada e saída 49 min prorrogada. Só uma ponta excede 180 min, logo a L-084 mantém o cadastro aplicável. L-085 e V6 dão **um par fechado em 08/11**, com uma C9. A L-097 rege a classificação e a autorização das pontas. Para fechar o RED nominal, usar o cadastro congelado e os PKs, instantes e papéis reais do laudo, com gabarito pela regra, negativos e comparação de impacto por rubrica. Esse insumo fica [nao verificado]. `app/docs/LEIS.md:115`, `app/docs/LEIS.md:116`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:136`, `app/docs/BACKLOG.md:2027`, `app/ponto/tests/test_par_pelo_turno.py:80`
- **Origem e leitores.** `_mk` publica o turno e chama `_c9_do_par`. `pares_separados_pelo_teto` lê o carimbo. `teto_na_lista` resolve a competência pelo dia julgado e lavra o snapshot consumido por Cadastro x Realidade. A Gestão de HE tem fluxo próprio de pendências e decisões, já exercitado em outro ensaio da cópia. `app/ponto/turnos.py:171`, `app/ponto/turnos.py:188`, `app/ponto/turnos.py:311`, `app/ponto/services/teto_na_lista.py:51`, `app/ponto/services/teto_na_lista.py:91`, `app/escala/services/cadastro_realidade.py:425`, `app/ponto/tests/test_par_pelo_turno.py:656`

## Escopo final da volta

- **REDs numerados:** **1a–1b** dia por PK e cartão, **2** relógio da pausa, **3a–3d** PDF, noturno, porta e resumo, **4a–4c** composição, herança e saldo, **5** entrega da ata, **6** geometria sintética e lacuna nominal. Evidenciar os vermelhos antes da cura e repetir a verificação após a construção. `CLAUDE.md:28`, `CLAUDE.md:421`, `app/docs/LEIS.md:136`, `app/docs/LEIS.md:154`
- **Turnos e leitores.** Arquivos da volta para 1, 2, 3a–3b e 5: `app/ponto/turnos.py`, `app/relatorios/pdf_espelho.py`, `app/escala/servico_jornada.py`. Testes de referência: `app/ponto/tests/test_par_pelo_turno_bordas.py`, `app/ponto/tests/test_par_pelo_turno.py`, `app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py`, `app/relatorios/tests/test_pdf_sem_regra_propria.py`. `turnos.py` é compartilhado com DIA-DA-BATIDA. O relato registra o rebase da F4e sobre PAR-PELO-TURNO. Estado Git dessa integração [nao verificado]. `app/ponto/turnos.py:1223`, `app/ponto/turnos.py:1990`, `app/relatorios/pdf_espelho.py:264`, `app/escala/servico_jornada.py:249`, `app/ponto/tests/test_par_pelo_turno_bordas.py:13`, `app/ponto/tests/test_par_pelo_turno.py:195`, `app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:166`, `app/relatorios/tests/test_pdf_sem_regra_propria.py:111`, `app/docs/RELATO.md:13678`
- **Fechamento e competência, mão única.** `app/ponto/services/fechamento.py` para 4a e 4c, `app/ponto/services/espelho.py` para 3d e `app/folha/porta_export.py` para 3c. Cobrir a composição em `app/ponto/tests/test_composicao_de_fatias_e_total.py` e a passagem financeira em `app/ponto/tests/test_par_pelo_turno.py`. A **11 é aberta** nesta volta. Para o ato de dinheiro da 11 valem reversão antes, prova de integridade da exportada, prova de frota depois e pacote HOMOLOGA. A **09 é guardada** nas empresas que a exportaram. O recálculo genérico barra a escrita, inclusive a lavra de `DiaPago` pedida junto de `somente_leitura`. Estado efetivo do gravado e do TXT [nao verificado]. `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:555`, `app/ponto/services/fechamento.py:576`, `app/ponto/services/fechamento.py:645`, `app/ponto/services/espelho.py:453`, `app/folha/porta_export.py:77`, `app/ponto/tests/test_composicao_de_fatias_e_total.py:270`, `app/ponto/tests/test_par_pelo_turno.py:636`, `app/docs/LEIS.md:156`, `app/docs/PROMPTS.md:3032`, `CLAUDE.md:255`, `CLAUDE.md:453`, `CLAUDE.md:458`
- **Motor, zona com aval.** O **4b** exige tocar `app/ponto/motor_calculo_v2.py`, na origem da sobrescrita de `data_jornada`. A seção 4 exige aval explícito para esse toque. Aval específico para a herança de 6 h nesta volta [nao verificado]. Os sítios de origem de 4a e 4c estão em `fechamento.py`. `CLAUDE.md:145`, `CLAUDE.md:274`, `app/ponto/motor_calculo_v2.py:2516`, `app/ponto/motor_calculo_v2.py:2521`, `app/ponto/services/fechamento.py:270`, `app/ponto/services/fechamento.py:645`
- **Juízes novos: zero.** Já existem o juiz do turno e do intervalo, a juíza do vínculo do dia, o motor de cálculo e a autoridade da janela de folha. C9 já tem origem e lavra. O item 6 pede evidência nominal. `app/core/juizes.py:216`, `app/core/juizes.py:222`, `app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:29`, `app/docs/JUIZES.md:35`, `app/docs/JUIZES.md:142`, `app/ponto/services/teto_na_lista.py:91`, `CLAUDE.md:74`, `CLAUDE.md:182`
- **Proibidos nesta volta:** fallback, `except` para mascarar divergência, juiz ou porta nova, zerar HE para igualar leitores, autorizar HE automaticamente, alterar batida original, pag

[truncado em 20000 de 20216 caracteres]

## 27/11 02:33 · sessao cf389dff1207 · - · - · origem=Ronald (tmux)

- **18** Use o restore para o fato futuro e congele cada horizonte. O C6 hoje reduz a sequência completa antes do recorte e perde o carimbo anterior. O I23 deve medir o caso no auditor real, com um controle que conte uma compensação futura de ato normal. A c4 já cobre o efeito temporal da `RECUSA` na tarefa. [C6](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/juizes.py:1476) · [I23](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/auditar_invariantes_chamados.py:1122) · [Controle anterior](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319) · [c4](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:297)
- **30** Grave a resposta pelo escritor público e introduza a divergência depois. Com carimbo cru nulo, a fila real omite a pergunta, a API envia carimbo nulo e o raio-x credita o chamado ao grupo sem resposta. O modal exige `disputa_obj`, lista de perguntas e `pode_agir_map`. A composta conserva o texto da projeção sob o contrato c4. [Fila](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/juizes.py:1863) · [API](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/api/views.py:1161) · [Raio-x](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/raiox.py:135) · [Modal](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/templates/chamados/partials/modal_fio.html:123) · [Composta c4](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4635)
- **42** O carimbo público pela trilha permite registrar depois da lavra, com `ato_em` anterior ao corte, e mudar de fato a projeção. O marco passado atualmente sobe porque o leitor toma o máximo de `respondida_em` na coluna. A referência à migração e ao caso de empate/reexecução é da c4. [Porta](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:1767) · [Marco](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/veredito_lavrado.py:206) · [c4](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:914)
- **45** Afirme classes exatas. Divergência de texto ou autor com linha vigente entra em A, passivo sem linha entra em B, e estoque ocupa seu próprio quadro. Exercite autor nulo e exceção histórica com ABSORÇÃO vigente. A c4 já cobre instante, classificação e contagem por PK. [Comparador c4](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4589) · [Teste c4](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:574) · [Exceção histórica](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/docs/PROMPTS.md:2806) [nao verificado]
- **46** Vincule a escala de 480 minutos e deixe o sinal da batida relavrar a ata depois da validação. Afirme `480/0 → 480/480`, com prontidão `0 → 100`. Os mutantes são o denominador realizado e a omissão da relavratura. [Sinal](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/ponto/signals.py:136) · [Ata](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/ponto/services/cartorio.py:749) · [Prontidão](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/folha/export.py:1104)

Execução dos testes e mordida dos mutantes [nao verificado]. [nao verificado]

## 27/11 02:32 · sessao 43c32c8f4f58 · - · - · origem=Ronald (tmux)

Leitura concluída. Execução dos testes e dos mutantes [nao verificado]. [nao verificado]

- **01/02/04/07/44** têm entrada existente em `call_command('backfill_decisoes_pergunta', aplicar=True)`. Hoje o comando só publica a projeção sem linha no DRY e aplica colisões e silêncios. Isso permite cobrar o efeito ausente de R/V em 01/02/04/44 e observar a preservação de `celula_absolveu` em 07. As datas históricas devem preceder o `applied` da 0057. Na c4, a fixture comum fixa esse marco em 2026-01-01. `chamados/management/commands/backfill_decisoes_pergunta.py:459,489,631` · `chamados/tests/cortes_f2_f9_apoio.py@c154a8d4e:36,61`
- **01** deve verificar silêncio no caminho do estoque. A c4 já verifica proveniência e o evento único da resposta normal. `chamados/tests/test_e4c_f5_c4_red.py@c154a8d4e:401,495` · `chamados/models.py@c154a8d4e:4755`
- **02/03** devem preservar a foto completa dos atos anteriores. O controle do 03 restaura R após REABERTURA compensadora e exige V apenas nesse cartão. `chamados/models.py:4437,4533` · `docs/PROMPTS.md:2806`
- **11** dá RED pela ausência de REABERTURA na pergunta vazia usando a assinatura atual. **12** pede usuários independentes para as quatro FKs. `chamados/services/disputa_emissao.py:1628,1703` · `chamados/models.py:3804,3824,4347,4352`
- **14 — lei:** a sessão prioriza `REABERTURA` marcada da c4. A exigência E4D de `SANEAMENTO` em campo próprio e preservação de outra resposta no empate ou posterior pede decisão explícita sobre seleção do ato atingido. `docs/PROMPTS.md:2950` · `docs/LEIS.md:151` · `chamados/tests/test_e4c_f5_instante_c1.py@c154a8d4e:234` · `chamados/services/validacao.py@c154a8d4e:476`
- **42** usa a referência da migração real e testa o resíduo pela porta pública de resposta, com lavra em T1 e nova resposta efetivamente projetada com tempo de negócio anterior a T1. Hoje `marcos_do_lote` lê o `respondida_em` sobrescrito. O selo F3 veda consultar DP diretamente nesse leitor. `chamados/tests/test_e4c_f5_c4_red.py@c154a8d4e:914` · `chamados/services/disputa_emissao.py:1529,1562` · `chamados/veredito_lavrado.py:205` · `chamados/tests/test_f3_lavra_foto_sem_decisao.py:312` · `docs/CORTES.md:609,610`
- **Lei/desenho:** por qual entrada se reverte exatamente um lote de uma competência em 04, transporta-se o `!` e comprova-se hash real do gravado em 08, identifica-se o pedido em 11 e lê-se autoria explicitamente desconhecida em 12? A via comum de 08 segue a recusa da c4. `chamados/management/commands/backfill_decisoes_pergunta.py:459,514` · `ponto/portas/regen_exportada.py:62` · `chamados/tests/test_e4c_f5_c4_red.py@c154a8d4e:419` · `chamados/services/disputa_emissao.py:1628` · `chamados/models.py:4347`

## 27/11 02:29 · sessao 45fdf9242303 · E4C-F5-INSTANTE · construtor · origem=Code

**c4red3 — 29 testes coletados**, todos `test_mordida`, em coleta isolada do `unittest` com dependências substituídas em memória. A coleta no runner Django e a mordida dos REDs ficam para o Code [não verificado]. [Fábrica das contraprovas](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:27) [nao verificado]

1. **M12, M13 e M21.** Os mutantes substituem as referências com `patch.object(..., new=...)`, e o `ExitStack` restaura os alvos. M12 cobre também os aliases usados pelo RED e pelo comando de alarme. M21 usa uma cópia da operação em memória. [M12](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:194), [M13](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:208), [M21](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:300)
2. **M14, M25 e M27.** As três contraprovas herdam diretamente da classe RED de concorrência, que é `TransactionTestCase`. A edição no RED foi necessária para aplicar `freeze_time` ao ciclo de vida da subclasse executada, incluindo `setUpClass` e `tearDownClass`. [Hooks do relógio](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:66), [classe de concorrência](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:617)
3. **Coleta.** A fábrica mascara os métodos de teste herdados na classe gerada e executa o método RED visado por `test_mordida`. [Filtro de coleta](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:27) [nao verificado]

**Recorte para o Code**, a partir de `app/`. [Entrada do runner](/home/ronald/wt-codex-e4c-f5-instante-012038/app/manage.py:7)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_instante_c1 \
  chamados.tests.test_e4c_f5_c4_red

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.e4c_f5_c4red2_mutantes

for classe in M12Vigia M13Dry M14OrdemDaTrava M21Empate0058 M25ConflitoErrado M27EntregaPrimeiro; do
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    "chamados.tests.e4c_f5_c4red2_mutantes.${classe}.test_mordida"
done
```

## 27/11 02:27 · sessao efdf484bf0a2 · E4D-CONTRATO-ROTA-B · verificador · origem=Code

A pista tem **43 ocorrências em 36 métodos**. São 12 erros e 31 falhas, das quais sete são ocorrências adicionais de subtestes: 18 tem duas, 30 tem cinco e 45 tem três. Há **nove métodos sem ocorrência listada**. O resultado individual desses nove na pista fica [nao verificado]. `T:438–462,513–534,1076–1117`

**Escopo.** Os dois commits da E4D, `145fccbe8` e `d43b0dfe0`, acrescentam somente `T` e `M`, com zero `except`. O delta solicitado `de65d64f4..d43b0dfe0` tem 68 arquivos, inclui código de produção e testes existentes herdados do merge `eb76e7be7`, e contém uma ocorrência textual de `except`, numa docstring. Cláusulas `except` acrescentadas nesse delta: zero. O teste antigo do caso 20 está intacto. A verificação foi somente leitura. Reexecução, mordida em execução e dados de produção [nao verificado]. `T:1` · `M:1` · `app/chamados/models.py:4598–4607` · `app/chamados/tests/_ato_da_fixture.py:9` · `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:364–380`

**Referências.** [T, testes da matriz](/home/ronald/codex_ws/d43b0dfe014ee97a3a400a8fed5eb6eea6eaec0e/app/chamados/tests/test_e4d_matriz_reds.py:1) · [M, runner de mutantes](/home/ronald/codex_ws/d43b0dfe014ee97a3a400a8fed5eb6eea6eaec0e/app/chamados/tests/e4d_matriz_mutantes.py:1) · [C4, REDs da c4red2 em `c154a8d4e`](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1). `A` indica asserção do contrato e `I` indica interface nomeada no brief. As regras foram cotejadas com o brief do construtor e as decisões registradas. `app/docs/PROMPTS.md:2873,2950,2974`

### As 43 ocorrências

**29 certas, sendo 27 A e 2 I. 14 erradas.** Cada linha abaixo corresponde a uma ocorrência da pista. As curas erradas são contraexemplos por leitura estática. `T:167–200,213–534,547–857,936–1269`

| Caso | Classificação e motivo | Cura errada que ficaria verde | [nao verificado]
|---|---|---|
| 01 | **Certo A.** O backfill grava a resposta e emite um evento. `T:220–228` · `app/chamados/models.py:4608–4623` | Suprimir o despertar de toda `RESPOSTA`, inclusive a normal. `T:220–228` · `C4:479–491` |
| 02 | **Certo A.** Sai a `VALIDACAO` esperada, sem `ABSORCAO`, mas com um evento. `T:230–243` · `app/chamados/models.py:4622` | Alterar a `RESPOSTA` preexistente mantendo seu tipo. Só os valores da validação nova são conferidos. `T:238–243` |
| 03 | **Certo A.** Entra `VALIDACAO` retroativa na absorvida. `T:245–260` · `app/chamados/models.py:4437–4447` | Suprimir validação para qualquer histórico com `ABSORCAO`, inclusive depois de uma compensação. `T:253–260` |
| 04 | **Errado.** Para no símbolo inventado `aplicar_backfill`, antes de testar a reversão. O `aplicar` existente atende grupos de colisão e silêncio, com outro contrato. `T:280–300` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:193–219` | Devolver dois SHA-256 fixos e fotos com os PKs certos e valores falsos. O teste confere formato, diferença e chaves. `T:281–286` |
| 05 | **Certo A.** Falta `VALIDACAO` física. O absorvedor grava `ABSORCAO` após carimbar. `T:943–957` · `app/chamados/services/materializacao.py:1107–1132` | Inserir os dois atos juntos e gravar V/M diretamente em outro bloco. Só os `INSERT`s têm seus savepoints comparados. `T:944–957` |
| 06 | **Errado.** O brief exige exceção com lápide. O erro cobra sua exposição como `DP.lista_de_excecoes`, com shape escolhido pelo teste. `T:167–178,560–565` | Cadastrar o escritor observado com lápide genérica e manter outro escritor de M fora da lista. `T:173–178,562–565` | [nao verificado]
| 07 | **Errado.** A foto, a ausência de ato e o zero evento já foram verificados quando cai no atributo inventado. `T:302–311` | Preencher a lista e deixar o comando efetivo fabricar `CONCLUSAO`. O teste passa uma lista vazia do helper a `registrar`. `T:304–311` · `app/chamados/models.py:4437–4448` |
| 08 | **Errado.** Tropeça em `aplicar_backfill` antes de demonstrar o veto da exportada. O teste exige 64 hex no gravado, enquanto a função existente de hash do gravado retorna 12. `T:327–345` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:505–516` · `app/ponto/portas/regen_exportada.py:62–74` | Retornar o mesmo hash falso do gravado em ambas as chaves. A foto da tabela é comparada à parte e o hash reportado do gravado nunca é comparado a ela. `T:148–152,330–340` |
| 09 | **Certo A.** O ramo grava V sem `VALIDACAO` física. `T:567–585` · `app/chamados/services/disputa_emissao.py:2112–2122` | Inserir a linha e manter V no escritor direto. Para M, um juiz que sempre devolvesse a mesma via também poderia passar, pois o esperado vem desse juiz. `T:578–585` |
| 10 | **Certo A.** O texto legado ganha carimbo sem `RESPOSTA` física. `T:587–606` · `app/chamados/services/disputa_emissao.py:2189–2202` | Corrigir só perguntas de `escolha`. A fixture com texto usa esse tipo e deixa de fora uma `hora` com o mesmo meio-par. `T:590–605` |
| 11 | **Certo I.** O identificador do pedido está na regra. A assinatura atual rejeita o argumento `id_do_pedido`. `T:347–368` · `app/chamados/services/disputa_emissao.py:1628,1695–1715` | Deduplicar para sempre pela pergunta, recusando também um pedido novo. O teste repete um único UUID. `T:354–368` |
| 12 | **Certo A.** A exclusão não levanta `ProtectedError`. A marca de autoria desconhecida fica atrás dessa primeira asserção. `T:370–388` · `app/chamados/models.py:3804–3829,4347–4353` | Proteger só `validada_por`. O mesmo U ocupa todas as referências e uma única FK protegida impede sua exclusão. `T:370–382` |
| 14 | **Certo I.** `SANEAMENTO` é o choice literalmente pedido. O erro ocorre antes das variantes de empate e posterioridade. `T:390–413` · `app/chamados/models.py:4326–4337` | Sanear sempre a `RESPOSTA` de menor PK. A resposta histórica que deve ser atingida sempre ocupa essa posição nas fixtures. `T:393–406` |
| 16 | **Certo A.** A porta com o dono travado aceita a resposta futura. `T:415–424` · `app/chamados/models.py:4598–4609` | Fazer o `INSERT` e só então levantar `ValueError`. O rollback externo esconde a violação da ordem. `T:420–424` |
| 17 | **Certo A.** `save(force_insert=True)` com PK vazio entra. `T:426–436` · `app/chamados/models.py:4405–4408` | Vetar só `force_insert` e criar por `Model.save` com alias `DP`. O selo AST reaproveitado não rastreia esse caminho. `T:429–436` · `app/chamados/tests/test_e1_entradas.py:190–214` |
| 18 · horizonte | **Certo A.** Em T1, a tarefa ainda vê a resposta de T0 e o C6 perde o par após reduzir a sequência inteira. O modal tem um defeito adicional de fixture. `T:449–459,160–165` · `app/chamados/services/tarefa_do_dia.py:389–429` · `app/templates/chamados/partials/modal_fio.html:123` | Especializar os leitores para o cenário de `REABERTURA` testado e deixar outros compensadores com horizontes diferentes. `T:445–459` |
| 18 · contador | **Errado.** Procura `ato_futuro` no contador A/B. O I23 existente, no relógio T1, contaria a compensação T2. A regra exige reconciliar esse recorte com o zero esperado. `T:438–462` · `app/chamados/services/vigia_ato_x_coluna.py:8–25` · `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134` | Acrescentar `ato_futuro=0` fixo ao retorno de A/B. `T:460–462` |
| 20 | **Certo A.** A recusa humana acontece, mas a passagem retorna `[]` em vez de recusar Q. O teste também pede que a irmã anterior fique e a seguinte prossiga. `T:959–995` · `app/chamados/services/materializacao.py:1089–1132` | Usar só `RECUSA.ato_em > T1` e um ID constante por ausência. Passa com T2 e uma passagem, perde empate e pedidos distintos. `T:966–990` |
| 21 | **Errado.** A fixture chama `_carimbar_validacao` antes da trava real. `registrar` exige o dono travado. O traceback do erro da pista foi cortado [nao verificado]. `T:187–200,544–545,674–683` · `app/chamados/services/validacao.py:164–178` · `app/chamados/models.py:4606–4607` | Com a fixture travada, recusar toda pergunta daquele motivo com `ja_validada`, sem reler a elegibilidade. `T:674–683` |
| 23 | **Errado.** O `patch` aponta para `acoes_disputa.validar_pergunta`, mas o import é local. Corrigido o alvo, sobra a mesma fixture sem trava do 21. `T:544–545,700–713` · `app/chamados/services/acoes_disputa.py:373–376` | Com a fixture corrigida, recusar toda correção depois da trava com o mesmo texto. Falta um controle de correção elegível nesse teste. `T:700–713` |
| 25 | **Certo A.** O seletor exige âncora de hora e deixa Q carimbada apesar da `ABSORCAO` vigente. `T:1024–1050` · `app/chamados/services/materializacao.py:1195–1212,1235–1243` | Aceitar qualquer `ABSORCAO` física, inclusive uma já vencida por `RECUSA`, ou continuar exigindo o texto de E que esta fixture conserva. `T:1027–1043` |
| 26 | **Certo A.** A coincidência de colunas devolve o legado e quebra a preservação. O leitor admin fica por verificar em execução. `T:1052–1072` · `app/chamados/services/materializacao.py:1195–1212,1267–1279` | Encaminhar todas as perguntas sem vínculo daquele dia, mesmo de outra passagem. Só a entrada de Q no único grupo é cobrada. `T:1052–1072` |
| 28 | **Errado como RED de trava e compensação.** A fixture de `declaracao` segue a matriz, mas o seletor a exclui antes de consultar o espelho. Trocar para `legado_sem_lastro` alcançaria o comando e tiraria H4 da entrada. `T:749–777` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44,66–77` · `app/chamados/models.py:4533–4541` | Alargar o seletor para toda declaração e sempre registrar `REABERTURA`, inclusive com H4 já compensado. `T:753–771` |
| 29 | **Certo A.** M é limpo sem `REABERTURA` física. `T:779–797` · `app/chamados/services/relavra_via.py:230–261` | Inserir a reabertura e continuar descarimbando diretamente, ou reler a elegibilidade antes da trava. `T:790–797` |
| 30 · SQL | **Errado.** A fixture injeta a divergência sem projetor e a própria consulta de teste seleciona as colunas cruas. `q_respondida_com_texto` é um filtro. Migrar o leitor SQL real ainda deixaria essa consulta vermelha. `T:117–121,1087–1095` · `app/chamados/juizes.py:1676–1681` | Fazer só essa leitura de coluna devolver o par do ato, mantendo o leitor SQL de produção no par cru. `T:1093–1095` |
| 30 · C6 | **Certo A.** Entrega 10:05 com o texto cru `08:00`. `T:1096–1097` · `app/chamados/models.py:4507–4515` | Corrigir só a resposta desse recorte temporal, deixando as demais chamadas da composta divergirem. `T:1096–1097` |
| 30 · API | **Certo A.** O payload serializa R cru e o display já mostra `09:00`. `T:1098–1107` · `app/api/views.py:1161–1165` | Corrigir a serialização com a lista fornecida pelo mock e deixar a seleção real buscar perguntas erradas. `T:1101–1107` |
| 30 · modal | **Errado.** Faltam `disputa_obj` e `pode_agir_map`. Mesmo com o par correto, o bloco fica oculto e o helper devolve `(None, None)`. `T:160–165,1108–1109` · `app/templates/chamados/partials/modal_fio.html:123,157–158,217–220` | Renderizar a resposta também para quem está fora do portão de autorização. A fixture passaria. `T:160–165` · `app/templates/chamados/partials/modal_fio.html:123` |
| 30 · raio-x | **Certo A.** A tupla entregue ao juiz contém o par cru. `T:1110–1117` · `app/chamados/services/raiox.py:135–158` | Corrigir a tupla passada ao juiz e deixar a seleção real de `q_vivas` errada, pois o teste a substitui. `T:1110–1117` |
| 31 | **Errado.** Cai no atributo inventado da lista. O selo vê apenas os campos da D escritos pela emissão exercitada. `T:167–178,1127–1141` | Cadastrar esse escritor e deixar nascimento manual ou campos do chamado fora do selo. `T:1127–1141` |
| 32 | **Errado.** Mesmo atributo inventado. Também exige `tipo_resposta` da P sob a chave `raiz`, embora a exceção do brief cubra D/chamado e a regra da filha exija R/V/M vazios e zero DP. `T:167–178,1143–1157` · `app/docs/PROMPTS.md:2950` | Cadastrar só `tipo_resposta` e deixar outros campos de identidade fora da lista. `T:1152–1157` |
| 34 | **Certo A.** M é escrito antes de `SILENCIO`. `T:1159–1172` · `app/chamados/models.py:2184–2207` | Antecipar o ato e continuar gravando M diretamente. A fixture sem chamado-pai também deixa escapar uma cascata quebrada. `T:1160–1172` · `app/chamados/signals.py:20–46` |
| 35 | **Certo A.** A emissão retorna outra D. A fixture distingue `fechada_em` de PK e cobre o empate. `T:1177–1204` · `app/chamados/services/disputa_emissao.py:539–542,582–605` · `app/docs/CORTES.md:618` | Eleger a mais recente de todo o colaborador, inclusive a de outro chamado. A fixture cria um único chamado. `T:1177–1189` |
| 37 | **Errado.** O comportamento de reabrir preservando R passa até o atributo inventado da lista. `T:1225–1242` | Listar só `materializada_em` e deixar a alteração de autoria e via fora do selo. `T:1239–1242` |
| 38 | **Certo A.** O desligamento grava M antes de `SILENCIO`. O recorte usa a pergunta muda prevista. `T:1244–1269` · `app/chamados/models.py:2184–2207` | Inserir o ato primeiro e manter o carimbo direto, ou perder a cascata do pai, que a fixture não cria. `T:1250–1269` |
| 39 | **Certo A.** V precede a `VALIDACAO` física. `T:799–818` · `app/chamados/services/validacao.py:164–178` | Antecipar a DP e conservar dois escritores diretos de V/M. Uma via M errada também passa pelo recorte `[:6]`. `T:807–818` |
| 40 | **Certo A.** `recusa_motivo` precede a `RECUSA` física. `T:820–836` · `app/chamados/services/validacao.py:995–1000` | Mover a linha antes do `save` legado e manter a limpeza em outro escritor. `T:826–836` |
| 41 | **Certo A.** A hora corrigida é escrita antes da `RESPOSTA`. A asserção de `RESULTADO` ainda fica atrás desse RED. `T:838–858` · `app/chamados/services/disputa_emissao.py:1562–1567` | Gravar `RESULTADO` só no caminho da exceção injetada. A recusa normal por `ok=False` poderia perder a trilha. `T:843–857` · `app/chamados/services/acoes_disputa.py:452–465` |
| 42 | **Errado.** `DP.marcar_estoque` é uma API inventada. Recebe a hora pronta e contorna a operação de migração pedida. O trecho que exige projeção alterada e lavra preservada está depois do erro. `T:464–494` · `app/docs/PROMPTS.md:2974` · `C4:900–931` | Fazer o helper funcionar só quando chamado à mão, enquanto a migração deixa o estoque sem marca ou remarca linhas novas. `T:468–473` |
| 44 | **Certo A.** A segunda entrega insere outra linha com o mesmo fato. A diferença é a duplicata, não a representação de `FakeDatetime`. `T:499–511` · `app/chamados/models.py:4608–4609` | Deduplicar pela identidade da lista em memória ou regravar o instante no retry. A lista e o relógio são os mesmos. `T:500–511` |
| 45 · autoria | **Certo A.** O mesmo instante com autor diferente retorna zero. `T:514–516,523–528` · `app/chamados/services/vigia_ato_x_coluna.py:8–25` | Contar autoria só quando os dois lados têm autor, deixando passar `NULL` indevido. `T:514–516` |
| 45 · lastro | **Certo A.** V sem `VALIDACAO` física retorna zero. `T:517,523–528` · `app/chamados/models.py:4473–4483` | Contar toda ausência física como nova, inclusive a exceção histórica do 03. A fixture não traz essa exceção. `T:517,523–534` |
| 45 · texto | **Certo A.** Texto diferente com o mesmo instante retorna zero. `T:518–528` · `app/chamados/services/vigia_ato_x_coluna.py:8–25` | Comparar só texto e autoria e esquecer diferenças exclusivas de instante. A soma também aceita jogar tudo na mesma classe. `T:514–534` |

Há uma falha comum nos testes de ordem. `insercoes` procura o literal do ato em qualquer parâmetro SQL e `primeira_escrita` identifica a coluna sem vincular as duas operações ao mesmo PK. Antecipar a DP e manter o escritor direto satisfaz as asserções de ordem. A lista de exceções tem limitação semelhante: o helper verifica os escritores observados naquela execução e aceita campos adicionais na lista. `T:52–90,167–185,944–957,1163–1172` [nao verificado]

O teste antigo que cai com a cura do 20 é `test_RED_recusa_datada_depois_mas_inserida_antes_da_absorcao_recusa`. Ele exige `recusadas == []`, a `ABSORCAO` posterior por PK e o bloqueio H4 da nova resposta. A inversão é deliberada no brief, mas o teste novo omite o nome da caracterização que ficará vermelha. `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:364–380` · `T:959–995`

### Cobertura e guardas

Existem **45 métodos numerados distintos**, cobrindo **01–46 com 33 excluído**. A cobertura dos números existe. A cobertura da regra fica incompleta nos erros acima e nas mordidas abaixo. Das 11 guardas pedidas, 21 e 23 aparecem como `ERROR`. As nove restantes têm observáveis de negócio, com os limites indicados. `T:213–534,547–857,936–1321` · `M:17–57`

| Guarda sem ocorrência na pista | O que observa e o que ainda escaparia |
|---|---|
| 13 | Refresh funciona antes da desativação e depois devolve `401` por inatividade, com autoria preservada. Alterar texto ou motivo da DP mantendo tipo e autor escaparia. `T:608–631` · `app/api/views.py:76–100` |
| 15 | A entrega entra na espera pela trava e bloqueia o saneamento. Uma porta que sempre recuse após travar também passaria. `T:633–655` |
| 19 | A resposta humana realmente entra no empate e fica. Uma automática que sempre retorne `False` após travar também passaria. `T:657–672` |
| 22 | A reabertura realmente entra durante a espera e o teste exige zero validação e zero batida. Retornar sempre `sem_resposta` para esse motivo passaria. `T:685–698` | [nao verificado]
| 24 | Exige Q intacta, ocorrência auditada e ausência de cobrança. Aceita carimbar E indevidamente se Q e a tarefa preservarem os resultados verificados. A ordem literal `[E,Q]` também excede a regra sobre Q. `T:997–1022` |
| 27 | Observa trava, SELECT, chamada ao juiz e escrita, com veredito forçado. Reler para cumprir o SELECT e julgar a instância antiga do mesmo PK passaria. O mutante fornecido move a trava para depois de uma releitura da empresa que já exige essa trava, podendo cair por `ColaboradorNaoTravado` antes da asserção. `T:715–743` · `M:40–43` · `app/chamados/reconciliador.py:1268–1279` · `app/chamados/services/trava_do_colaborador.py:87–113` |
| 36 | C6 muda de vivo para fora da fila com R e DP preservados. Um encerrador que feche o pai indevidamente com outra filha viva passaria nesta fixture de uma filha. `T:1206–1223` |
| 43 | Compara os PKs de ato e trilha, o estado e os eventos. Uma segunda chamada que regrave os mesmos valores de M/F passa com o relógio congelado. `T:1271–1286` |
| 46 | Exercita o denominador de `prontidao` com 480 minutos previstos e mudança no realizado. O realizado da segunda ata é escrito pela fixture. Plantar a batida sem atualizar o realizado passaria. `T:1288–1321` · `app/folha/export.py:1104–1116` |

### Conflitos com E4C-F5-INSTANTE

- **01/02/44 e 42, mesma entrada com dois contratos de teste.** A c4 usa `entrada_origem` (`ato` ou `estoque`) e `entrada_em`. Para a entrada normal, o relógio é `Now()` do banco. O estoque usa o `applied` da 0057. Os REDs 01/02/44 passam payload sem marca de estoque e esperam `origem='backfill'` e `instante_de_entrada=T1` pelo relógio Python. A c4 interpreta esse payload como ato normal e acorda a célula. A c4red2 também exige um evento na resposta normal, então silenciar todas as respostas conflita diretamente. `T:96,220–243,499–511` · `app/chamados/models.py@c154a8d4e:4392–4406,4728–4777` · `C4:399–417,479–491`
- **42, o marco do estoque coincide com a opção B para as linhas existentes.** A c4red2 roda a operação real da 0058, cobre empate, preserva a linha um microssegundo depois e repete a migração. A E4D inventa outro ponto de escrita e não cobre a fronteira. A parte nova da E4D é provar a lavra mesmo quando a projeção muda. Uma implementação que consulte DP diretamente em `marcos_do_lote` também derruba o selo F3 existente, que proíbe essa tabela no SQL do leitor. `C4:900–931` · `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py@c154a8d4e:9–20` · `T:464–494` · `app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:312–326`
- **02/04/08/44, separar a autorização e a identidade da operação.** Na c4, o estoque comum recusa linha nova 

[truncado em 20000 de 21948 caracteres]

## 27/11 02:27 · sessao 9d8478fd954a · PAR-PELO-TURNO · verificador · origem=Code

## Item 1 · Dia por PK e bordas

**O mesmo PK ainda muda de dia.** Na sonda em memória, com DNA `21:30–05:42`, `E 21/10 21:30` e `S 22/10 05:42`, a saída recebeu **22/10** em `[20/10,20/10]` e **21/10** em `[21/10,21/10]` e `[22/10,22/10]`. As duas marcas estavam carregadas nas três consultas. O corpo só devolve segmentos até `fim`, mas o projetor inclui as marcas da borda direita e preenche com o dia civil quando perde o turno que as reclama. A mesma chave passa a significar dia da jornada ou dia civil conforme o recorte. Isso contraria a L-085. `app/docs/LEIS.md:117` · `app/ponto/turnos.py:1858`, `:1990`, `:2026`, `:3066`, `:3077`

**O relógio da pausa ainda pode partir de outra entrada.** Na sonda sem marcos de borda, `E 17/10 20:30`, `E 18/10 22:00` marcada como intervalo, `S 19/10 08:00`, `E 19/10 10:00`, `S 19/10 14:00` deixam o dia 17 aberto e a última saída órfã. A pré-passagem reinicia as 26 h na `E` de intervalo e carimba `08:00→10:00` como pausa. O laço principal mantém o relógio da entrada real do dia 17, descarta as duas marcas carimbadas e perde o par `10:00→14:00`. A pré-passagem também roda pelo tipo gravado antes de ler a ata. `app/ponto/turnos.py:407`, `:419`, `:1223`, `:1318`, `:1381`, `:1430`, `:1459` · `app/docs/LEIS.md:119`

**O caso col297 pede as chaves no mapa e o recorte na apresentação.** O teste existente exige `21/09` para as duas marcas da madrugada e sua ausência no cartão encerrado em `20/09`. O docstring diz que elas formam outra jornada. A O73 trata da volta de pausa do mesmo dia. Restaurar as chaves atende esse caso, mas o teste aceita justamente a resposta civil que mascara o erro quando a jornada seguinte também cruza a meia-noite. `app/relatorios/tests/test_pdf_sem_regra_propria.py:111`, `:127`, `:135` · `app/docs/BACKLOG.md:1496` · `app/ponto/tests/test_par_pelo_turno_bordas.py:24`, `:44` · `app/docs/LEIS.md:85`, `:86`, `:117`

## Quem escreve e quem lê

| Valor | Quem escreve | Quem lê e onde o contrato diverge |
|---|---|---|
| Batidas, células, escalas e provedores de última batida e DNA | `_carregar_turnos_do_colab` devolve os cinco insumos. A casca carrega por data local e mantém a última batida como `Max(timestamp)` de toda a vida apurável. `app/ponto/turnos.py:1876`, `:1884`, `:1906`, `:1920`, `:1930`, `:1944` | `turnos_do_colab` e `dia_das_batidas` consomem a tupla. O lote e a carga da pergunta viva montam seus próprios insumos e chamam o mesmo corpo. O provedor da última batida pode provar encerramento fora da janela pela O95. `app/ponto/turnos.py:1871`, `:3023`, `:3133`, `:3157`, `:1161`, `:1176` · `app/chamados/services/carga_pergunta_viva.py:165` |
| Papel, `Turno.data_turno`, pontas, `batidas`, `aberto`, `cross_meianoite` e C9 | O corpo carrega o papel da ata, segmenta por vínculo e chama `parear_turnos`. `_mk` publica a geometria e a C9. `app/ponto/turnos.py:1969`, `:1989`, `:2023`, `:300` | Casca e lote publicam por dia do turno. O núcleo materializa a geometria e a lavra C9 usa o dia para escolher a competência. O serviço de jornada ainda procura no máximo um dia para trás e só aceita `data_turno` ou `data_turno+1`. Para `E 20/10 23:30 → S 22/10 01:30`, ele falha na consulta pela data da saída, mesmo com `cross_meianoite=True`. `app/ponto/turnos.py:2028`, `:3163` · `app/ponto/nucleo.py:47` · `app/ponto/services/teto_na_lista.py:51` · `app/escala/servico_jornada.py:249`, `:258` |
| Mapa `{pk: dia}` | `_dia_das_batidas_puro` escreve o dia do turno e completa as demais chaves com `data_local`. A casca aceita `turnos=` e substitui a parte julgada a partir de `ini`. `app/ponto/turnos.py:3027`, `:3058` | Espelho e API mensal buscam por PK e filtram o dia devolvido. O PDF usa as linhas do espelho. Painel, férias e vigia buscam PKs específicos. A consulta individual usa a janela do dia civil da marca e alimenta disputa, fio e serviço de jornada. No contraexemplo, a consulta individual da saída devolve 21 e o mapa da janela do dia 20 devolve 22. `app/ponto/services/espelho.py:465`, `:588` · `app/api/views.py:1444`, `:1459`, `:2042` · `app/relatorios/pdf_espelho.py:596` · `app/core/services/painel_op.py:330` · `app/ferias/management/commands/listar_ferias_com_batida.py:54` · `app/ponto/management/commands/vigia_plantio_overnight.py:87` · `app/ponto/turnos.py:3046` · `app/chamados/services/disputa_emissao.py:283` · `app/chamados/services/fio.py:1827` · `app/escala/servico_jornada.py:509` |
| Carga de apuração, células e feriados | `janela_com_pad` dá `ini−4..fim+2`. O helper consulta até o último microssegundo inclusive. O espelho carrega células e feriados nesse intervalo. O fechamento usa o envelope dos cortes e repete a carga de batidas para os órfãos. `app/ponto/turnos.py:1858`, `:2978`, `:2989` · `app/ponto/services/espelho.py:1032`, `:1056` · `app/ponto/services/fechamento.py:609`, `:619`, `:638`, `:811` | **O coletor de cálculo do PDF ficou fora.** Ele entrega à mesma `apurar_por_fatia` uma lista própria que termina ao meio-dia de `fim+1`. A `S 22/10 01:30` de um turno iniciado em `20/10 23:30` nunca chega a esse motor. O PDF ainda soma esse resultado em período livre. A testemunha da porta da folha também segue pedindo `fim+1` à autoridade, ampliando o período julgado antes de recortar a comparação. `app/relatorios/pdf_espelho.py:264`, `:419`, `:498`, `:628` · `app/folha/porta_export.py:77`, `:106`, `:114` |

**A fonte única ainda se rompe depois da carga.** O juiz de turno é o registrado em `core/juizes.py`. A fatia pode excluir batidas pelo **dia civil** antes de perguntar ao motor. O motor pode sobrescrever o **dia do juiz** pela herança de 6 h. E o resumo do espelho volta ao **dia civil da entrada** para recortar os totais. Exemplo deste último: `E 21/09 00:00 → S 21/09 07:50`, atribuída a 20/09 pelo juiz, fica fora da soma do motor exibida para um período encerrado no dia 20 quando essa soma é a fonte do resumo. `app/core/juizes.py:216` · `CLAUDE.md:17`, `:20` · `app/ponto/services/fechamento.py:270` · `app/ponto/motor_calculo_v2.py:2501`, `:2516` · `app/ponto/services/espelho.py:83`, `:453` · `app/relatorios/cartao_pela_celula.py:321`

## Testes e dinheiro [nao verificado]

- **Mutante sobrevivente.** Troquei em memória `papel_por_minuto=_papel` por `papel_por_minuto={}` na chamada do corpo. Os nove alvos executados com dependências de banco simuladas deram `OK` também no mutante. Com `E 20/10 21:30` e saída de `21/10 05:42` gravada como `E`, a ata entregue como `S` fecha um par na cópia e o mutante deixa dois abertos. O selo AST exige o nome do argumento. O mutante oficial da chamada indireta derruba esse selo mesmo preservando a entrega da ata e o par no ensaio. `app/ponto/turnos.py:2023` · `app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:187`, `:199` · `app/ponto/tests/test_par_pelo_turno_bordas.py:63`, `:69` · `app/ponto/tests/test_par_pelo_turno.py:195`, `:225`, `:246`, `:253`, `:266`
- **26 h chegam à carga e viram 0 h na fatia.** Na sonda com dois vínculos sobrepostos, A é dono de `20 e 23/10` e B de `21 e 22/10`. Para `E 20/10 23:30 → S 22/10 01:30`, o juiz fecha no dia 20. A fatia de A descarta a saída por cair no dia civil de B. B recebe as pontas e exclui o período por pertencer ao dia 20. A composição deu **0 h**, contra **26 h** no controle com uma fatia. O teste financeiro confere os PKs antes desse filtro e o teste de 26 h usa um só vínculo. `app/ponto/turnos.py:2003`, `:2026` · `app/ponto/services/fechamento.py:228`, `:258`, `:270`, `:302` · `app/ponto/motor_calculo_v2.py:2542` · `app/ponto/tests/test_par_pelo_turno.py:225`, `:646`, `:652`
- **A carga nova pode tirar 8 h de um par já contado na comp. 11.** Isolei a mudança de entrada com o mesmo `Motor12x36` da cópia. Batidas `E 19/10 22:00 → S 20/10 22:00` e `E 21/10 03:00 → S 21/10 11:00`, com DNA `03:00–11:00` no dia 21. Com a borda anterior antiga, o motor só recebe a `S` do primeiro par e conta **8 h** na 11. Com `ini−4`, o primeiro par chega inteiro, o vão de 5 h faz o segundo herdar 20/10 e a 11 fica com **0 h**. A herança é o mecanismo que já estava no motor. A ampliação passa a acioná-lo para esta classe. O fechamento usa esse resultado para o mensal e o `DiaPago`. `app/ponto/services/fechamento.py:262`, `:638`, `:1161`, `:1200` · `app/ponto/motor_calculo_v2.py:2516`, `:2521`, `:2542`
- **Só contexto também muda o saldo.** Na sonda comercial de `21/09–20/10`, com previsão de segunda a sexta, sem ausência e com uma única `E 22/10 12:00`, o motor devolve zero períodos nos dois alcances. O saldo calculado é `−193,60 h` nos dois. A atribuição do fechamento muda de **0,00 h para −193,60 h** porque a marca de `fim+2` desliga `sem_movimento`. A mesma atribuição alimenta o banco no `DiaPago`. `app/ponto/services/fechamento.py:638`, `:645`, `:1103`, `:1207` · `app/ponto/motor_calculo_v2.py:3542`, `:3572`
- **O teste existente desloca corretamente os instantes do filtro local**, para `05/10 23:30` dentro e `26/09 22:00` fora na fatia `01–03/10`. Ele continua conferindo a invariância UTC/−03. O motor falso registra somente as marcas recebidas. A alteração sela o alcance da lista e deixa passar as perdas de horas e a mudança de saldo acima. `app/ponto/tests/test_composicao_de_fatias_e_total.py:268`, `:279`, `:306`, `:316` · `app/ponto/services/fechamento.py:270`, `:645`, `:1103`

## Item 5 · Laudo col155/08-11

A série HJ-026 inclui os domingos de 18/10 a 15/11. O ensaio de 08/11 usa pontas e PKs sintéticos. Com DNA `21:30–05:42`, `17:29→06:31` dá **782 min** contra teto de **732 min**. A entrada está 241 min adiantada e a saída 49 min prorrogada. Só uma ponta passa de 180 min, portanto a L-084 não dispensa o cadastro nesse ensaio, e a L-085 atribui o par a 08/11. O teste cobre essa geometria. PKs, instantes e papéis do dia real, prova nominal dos 29 casos e antes/depois por rubrica **[nao verificado]**. `app/docs/BACKLOG.md:745`, `:2027` · `app/docs/LEIS.md:116`, `:117` · `app/ponto/turnos.py:82`, `:171`, `:311` · `app/ponto/tests/test_par_pelo_turno.py:80`

As sondas foram feitas em memória, com dados sintéticos e dependências de banco simuladas. A execução independente do recorte de 6.935 testes, a identidade Git de HEAD/base e os efeitos no gravado ou no TXT de produção ficam **[nao verificado]**. [nao verificado]

**REPROVADO — dia por PK varia com a janela (`turnos.py:1990`, `:3077`), relógio da pausa usa a entrada errada (`turnos.py:411`), PDF e leitor noturno conservam bordas próprias (`pdf_espelho.py:264`, `servico_jornada.py:258`), a fatia perde o par e a carga pode mover horas e saldo (`fechamento.py:270`, `:645`, `:1103`; `motor_calculo_v2.py:2521`), o selo deixa passar ata vazia (`test_selo_motor_nao_pareia_pelo_tipo_gravado.py:199`) e o laudo nominal de col155/08-11 segue [nao verificado].**

## 27/11 02:25 · sessao b4efc12a9134 · - · - · origem=Ronald (tmux)

Auditoria somente leitura de Portas/Ausência concluída e enviada ao agente principal. Os erros de guarda 21 e 23 vêm da fixture e do alvo do `patch`. O caso 28 não alcança a operação de troca de via, pois a triagem exclui a fixture `declaracao`. O caso 06 usa uma assinatura de lista escolhida no teste. Os casos 05, 09, 10, 20, 25, 26, 29 e 39–41 acusam desvios contratuais e aceitam mutantes que a tabela enviada detalha. `app/chamados/tests/test_e4d_matriz_reds.py:167–185,544–606,674–713,745–858,936–1072` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44`.

A pista enumera 43 ocorrências de 36 métodos distintos. Os nove ausentes incluem os controles positivos 13, 15, 19, 22, 24 e 27. Confirmação de execução e resultado de mutantes [nao verificado]. `app/chamados/tests/test_e4d_matriz_reds.py:438–535,608–698,715–743,997–1022,1076–1108,1206–1242,1271–1321`.

C4 usa a migração 0058 real, `entrada_origem`, `entrada_em` do banco e o marco `applied` da 0057. E4D usa uma API diferente e congela só o relógio Python. A partição do vigia em novo/estoque e a emissão de um evento no ato novo também precisam sobreviver aos novos contratos. `app/chamados/tests/test_e4d_matriz_reds.py:464–535` · `app/chamados/tests/test_e4c_f5_c4_red.py:479–491,559–595,900–931` na cópia c4.

## 27/11 02:25 · sessao 4a1e225f35a5 · - · - · origem=Ronald (tmux)

Nos 19 registros do histórico, há 12 motivos certos e 7 errados. Entre os certos, 11 são falhas comportamentais e dois erros de interface nomeada, 11 e 14, o que totaliza 13 certos e 6 errados ao contar as ocorrências individuais. A lista individual abaixo é a contagem de referência. [nao verificado para reexecução]

Referências `T` = `app/chamados/tests/test_e4d_matriz_reds.py`.

| Caso | Motivo da pista | Contraexemplo de cura errada que escapa |
|---|---|---|
| 01 | Certo A. Evento físico adicional, `T:220–228`, `app/chamados/models.py:4622`. | Silenciar também atos normais. `T:220–228`. |
| 02 | Certo A. Mesmo incremento apesar da validação física correta. `T:236–243`. | Reescrever a RESPOSTA existente mantendo seu tipo. O teste inspeciona apenas os valores da VALIDACAO nova. `T:238–243`. | [nao verificado]
| 03 | Certo A. Validação retroativa excedente. `T:253–260`, `app/chamados/models.py:4443–4447`. | Pular V para qualquer ABSORCAO antiga, inclusive compensada e seguida por validação legítima. `T:253–260`. |
| 04 | Errado. `aplicar_backfill` e `reverter` com essas assinaturas e o formato de retorno foram escolhidos pelo teste. O comando tem outra porta para colisões e silêncio. `T:280–296`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:193–217,486–516`. | Retornar hashes distintos constantes e fotos de conteúdo errado com chaves corretas. `T:281–286`. |
| 07 | Errado no critério de interface nomeada. A lista e a lápide são regras, sua exposição como atributo de DP foi escolhida pelo teste. `T:167–170,302–311`. | Passar no helper vazio e no cadastro da referência, mas inventar CONCLUSAO no comando efetivo. `T:302–311`, `app/chamados/models.py:4429–4431`. |
| 08 | Errado. `aplicar_backfill` antecipa uma porta e uma assinatura que o brief não fixa, antes de mostrar o veto real da exportada. `T:327–335`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:505–516`. | Devolver dois hashes iguais e falsos para o gravado. Também escapa RESPOSTA com instante ou texto errado. `T:330–340`. |
| 11 | Certo I. Identificador do pedido expressamente exigido, embora a grafia Python seja escolha local. `T:354–368`, `app/chamados/services/disputa_emissao.py:1628,1695–1715`. | Deduplicar pela P para sempre, bloqueando inclusive pedido distinto. `T:354–368`. |
| 12 | Certo A. `ProtectedError` ausente. `T:370–388`, `app/chamados/models.py:3804–3829,4347–4353`. | Proteger só `validada_por`. O único autor tem quatro referências, uma FK PROTECT já bloqueia sua exclusão e mascara as demais SET_NULL. `T:370–382`. |
| 14 | Certo I. `SANEAMENTO` é o nome literal do choice. `T:390–405`, `app/chamados/models.py:4326–4337`. | Escolher sempre o menor PK, que é o alvo em todas as variantes da fixture. `T:393–406`. |
| 16 | Certo A. A porta travada aceita o futuro. `T:420–424`, `app/chamados/models.py:4598–4609`. | Escrever primeiro, depois levantar `ValueError`. O rollback faz passar as verificações posteriores. `T:420–424`. |
| 17 | Certo A. `save(force_insert=True)` entra sem PK. `T:429–436`, `app/chamados/models.py:4405–4408`. | Vetar só `force_insert` e criar fora da porta por alias `DP` ou `Model.save`, que o selo atual não rastreia. `app/chamados/tests/test_e1_entradas.py:190–214`. |
| 18a | Certo A. Tarefa corta os atos pelo horizonte. Composta reduz a sequência inteira e o corte do C6 fica sem a resposta de T0. `T:449–457`, `app/chamados/services/tarefa_do_dia.py:399–429`, `app/chamados/models.py:4486–4515`. | Especializar o C6 apenas para a REABERTURA e o instante do cenário, sem implementar horizonte comum. `T:449–457`. |
| 18b | Errado. O teste procura `ato_futuro` em `contar()`, comparador A/B. No relógio restaurado para T1, o I23 real contaria a compensação T2. `T:43,449–462`, `app/chamados/services/vigia_ato_x_coluna.py:8–25`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134`. | Devolver `ato_futuro=0` fixo. `T:460–462`. O modal também tem defeito latente de fixture, pois faltam as variáveis do portão. `T:160–165,458–459`, `app/templates/chamados/partials/modal_fio.html:123`. |
| 42 | Errado. Chama `DP.marcar_estoque` com a hora pronta em vez de exercitar a migração, que é a operação decidida. `T:464–494`, `app/docs/PROMPTS.md:2974`. | Helper funciona se invocado manualmente, mas a migração nunca o chama. O leitor também pode ignorar o corte de entrada das linhas de estoque, pois todas foram marcadas antes de T1. `T:468–473,491–494`. |
| 44 | Certo A. Duplica o mesmo payload físico. `T:500–511`, `app/chamados/models.py:4608–4609`. | Deduplicar pela identidade em memória, perdendo retry após recarregar o payload. Também escapa regravar a entrada no retry, pois o relógio ficou igual. `T:500–511`. |
| 45 ×3 | Certos A para autoria, falta de sustentação e texto. `T:513–534`, `app/chamados/services/vigia_ato_x_coluna.py:8–25`. A soma de A/B é uma forma escolhida pelo teste. | Ignorar diferença só de instante ou a exceção histórica do caso 03, ou classificar todas as diferenças num bucket só. `T:513–534`. |

O `c4` específico está [nao verificado]. O registro disponível só identifica a c1 aberta e os temas de estoque, comparação de instante e L-130. `app/docs/BACKLOG.md:2173`, `app/docs/RELATO.md:172`. No F5 disponível, a validação sem linha é sintetizada e o A/B espera zero, enquanto 45 exige um para o mesmo arranjo. `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:63–67`, `T:517,523–528`. O F5 disponível exige texto da coluna em `composta`, enquanto 30-C6 exige texto do ato sobre coluna intencionalmente divergente. `app/chamados/tests/test_e4c_f5_opus_models.py:59–69`, `T:1089–1097`. A opção B exige hora de migração única e `origem=estoque`; a E4D escreve o nome `instante_de_entrada`, mas a identidade com `entrada_em` do c4 e a semântica do marco seguem [nao verificado]. `app/docs/PROMPTS.md:2974`, `T:469–473`.

**Retificação da contagem inicial:** 18 ocorrências no histórico, com 13 certas e 5 erradas pela tabela. São seis ERROR e doze FAIL. [nao verificado para reexecução]

## 27/11 02:24 · sessao e42b5417b2db · - · - · origem=Ronald (tmux)

### Raiz e leitores

`T` = `app/chamados/tests/test_e4d_matriz_reds.py`.

| Ocorrência | Classificação e contraexemplo |
|---|---|
| 30 SQL | **Errado.** A fixture injeta divergência fora de `registrar`, sem passar pelo projetor. A consulta pede as colunas cruas. Migrar os leitores para o ato e projetar toda escrita feita pela porta ainda devolve `(10:00,08:00)` nessa consulta sintética. `T:116–121,1087–1095`, `app/chamados/juizes.py:1676–1681` |
| 30 C6 | **Certo.** A leitura entrega o instante do ato com texto da coluna. A cura superficial pode trocar o texto localmente e continuar servindo estado divergente por outros chamadores. `T:1096–1097`, `app/chamados/juizes.py:1476–1496`, `app/chamados/models.py:4507–4519` |
| 30 API | **Certo.** O payload expõe os dois valores crus e o display do ato. A seleção real pode continuar errada, pois o teste fornece a coleção via mock. `T:1098–1107`, `app/api/views.py:1161–1165` |
| 30 modal | **Errado.** A fixture não fornece `disputa_obj` nem `pode_agir_map`. O bloco fica oculto mesmo com a fonte do par corrigida. `T:160–165,1108–1109`, `app/templates/chamados/partials/modal_fio.html:123,157–158,217–220`, `app/chamados/services/fio.py:348–362,603` |
| 30 raio-x | **Certo.** A tupla entregue ao juiz contém R cru. Um seletor real errado fica coberto pelo mock de `q_vivas`. O retorno do raio-x contém IDs, e o spy observa o par intermediário. `T:1110–1117`, `app/chamados/services/raiox.py:135–158` |
| 31 | **Errado.** A lista é exigida pelo brief, a API `DP.lista_de_excecoes` e seu dicionário são inventados. Autoinscrever os três escritores observados deixaria caminhos manuais e campos do chamado fora do selo. `T:167–178,1127–1141` |
| 32 | **Errado.** Mesmo erro de API. O selo ainda exige listar `tipo_resposta` da filha em `raiz`, quando o brief descreve a exceção de D/chamado. Um escritor que cria duas filhas após dois pedidos passa, pois a fixture faz um pedido. `T:167–178,1143–1157` |
| 34 | **Certo.** M sai antes de `SILENCIO`. Inserir o ato e em seguida escrever M diretamente deixa o teste verde. A fixture sem chamado-pai permite perder a cascata inteira. `T:1159–1172`, `app/chamados/models.py:2184–2207`, `app/chamados/signals.py:19–46` |
| 35 | **Certo.** Elege PK diverso. O teste distingue recência e empate. Selecionar a disputa mais recente de todo o colaborador sem restringir ao mesmo chamado passa nesta fixture de um chamado. `T:1174–1204`, `app/chamados/services/disputa_emissao.py:539–542,582–605`, `app/docs/CORTES.md:618` |
| 37 | **Errado.** A função de leitura da lista é inventada. Listar só a escrita de `materializada_em` passa mesmo deixando a via e a autoria fora do selo. `T:167–178,1225–1242` |
| 38 | **Certo.** Detecta SILÊNCIO depois de M. Antecipar o ato e carimbar M diretamente passa. A fixture também não tem chamado-pai para acusar falta de cascata. `T:1244–1269`, `app/chamados/models.py:2184–2207`, `app/chamados/signals.py:29–46` |

As guardas **36, 43 e 46** têm observáveis úteis. 36 mede C6 antes/depois, R/V/M, atos e batidas. Remover a chamada ao encerrador de competência ou fechar o pai indevidamente passa, pois a guarda observa a filha. 43 exige mesmos PKs de ato e trilha. Reescrever M/F com os mesmos valores ou repetir uma trilha de outro tipo passa. 46 mede a independência de previsto na face por grade e confirma a batida, mas insere o novo realizado na ata à mão. Não propagar o realizado no plantio ou corromper o denominador da face com snapshot passa. `T:1206–1223,1271–1286,1288–1321`, `app/ponto/services/fechamento.py:1446–1453`, `app/folha/export.py:1054–1089,1104–1116`

### Escopo, contagem e c4

O delta dos dois commits E4D, `eb76e7be7..d43b0dfe0`, tem só dois arquivos novos de teste e zero ocorrências de `except`. O delta pedido `de65d64f4..d43b0dfe0` inclui 68 arquivos e um token `except` em docstring, zero cláusulas novas. Os demais arquivos são do merge herdado PORTA-CONFERE-TRAVA. `app/chamados/tests/test_e4d_matriz_reds.py:1`, `app/chamados/tests/e4d_matriz_mutantes.py:1`, `app/chamados/tests/_ato_da_fixture.py:9`, `app/chamados/models.py:4590–4623`

A pista lista 24 métodos com falhas e 12 com erros. Sete entradas adicionais são falhas de subtestes de 18, 30 e 45. Há nove métodos sem ocorrência transcrita, 13, 15, 19, 22, 24, 27, 36, 43 e 46. O status individual na execução fica [nao verificado]. `T:438–462,513–537,1076–1117`, `falhas.txt` [nao verificado quanto a linhas locais]

No commit c4 RED `2baf51086`, `DecisaoPergunta` usa `entrada_origem` (`ato` ou `estoque`), `entrada_em` (`Now()` do banco para novos, marco 0057 para estoque) e `marco_do_estoque`. A 0058 marca por `entrada_em <= applied` e preserva os pós-marco. E4D 01/02/44 exigem `origem='backfill'` e relógio Python, 42 inventa `marcar_estoque` e `instante_de_entrada`. A intenção da Opção B é compatível com a 0058 para o estoque existente. A interface e o relógio exigidos pelo teste divergem. `app/chamados/models.py@2baf51086:4392–4406,4580–4586,4761–4786`, `app/chamados/migrations/0057_decisao_entrada_do_ato.py@2baf51086:16–32`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py@2baf51086:9–20`, `T:222–243,464–494,499–511`

45 pode estender o comparador de instante a valores mantendo classes e corte novo/estoque de c4. O teste E4D permite substituí-los por um contador genérico. 08 exige um caminho autorizado e só de trilha, compatível com preservar a recusa K6 no caminho comum de c4. 30/C6 muda deliberadamente a autoridade do texto exigida nas caracterizações F5 presentes em c4. `T:313–345,513–537,1096–1097`, `app/chamados/tests/test_e4c_f5_c4_red.py@2baf51086:144–181,234–275`, `app/chamados/services/vigia_ato_x_coluna.py@2baf51086:11–34`, `app/chamados/tests/test_e4c_f5_opus_models.py:58–73`, `app/chamados/tests/test_e4c_f5_volta2_juizes.py:80–103`, `app/docs/BACKLOG.md:2163`

## 27/11 02:21 · sessao b0bdba18d7a0 · E4C-F5-INSTANTE · construtor · origem=Code

Refiz a c4red2 em dois arquivos sob `chamados/tests/`. A checagem estática conferiu a sintaxe, as docstrings dos 22 testes e os alvos dos 29 mutantes. `git diff --check` passou. A execução dos REDs na base, a mordida dos mutantes, o verde após a cura e a compatibilidade executada com a c1 ficam [nao verificado]. [Testes](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1) · [Mutantes](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:399) [nao verificado]

### Teste → mutante visado → lei [nao verificado]

| Teste | Mutante visado | Lei ou corte | [nao verificado]
|---|---|---|
| [Par do carimbo](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:68) | `M01DuasValidacoes` só confere com duas validações. `M02IgnoraResposta` ignora `respondida_em`, inclusive na pergunta sem validação. | LEIS-1-4 resposta 2, L-110 |
| [Falhas das fotos](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:100) | `M03FotoDaLavra` restaura o trio do carimbo e deixa a validação candidata na instância. O teste injeta separadamente falhas na foto da projeção e da lavra. | L-007, O261-E-F2-FILA item 3 | [nao verificado]
| [Irmã divergente](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:155) · [Releitura e relógio da absorção](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:168) | `M22SoValidacaoNaIrma` ignora a resposta. `M23SoCandidata` ignora o par persistido. `M24RelogioAposEspera` troca o relógio comum pelo instante após a espera. | LEIS-1-4 resposta 2, L-002, L-007, RESPOSTAS-2011-1135 |
| [Validação futura](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:230) · [Resposta futura](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:257) · [Compensação](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:280) | `M04ValidacaoFutura` e `M05RespostaFutura` vazam cada ato pendente à decisão pública. `M28CompensadorCru` adianta a devolução ao usar o relógio cru. | L-073, E2/0514, F5-CONTRATO-V2 item 3 |
| [Porta no marco e depois](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:320) · [Correspondência por ato e instante](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:342) | `M07ValidacaoNoMarco` deixa entrar `VALIDACAO` nova. `M08QualquerResposta` barra uma foto elegível porque já houve outra resposta, compensada. As recusas de novidade e reprojeção têm motivos distintos. | LEIS-1-4 respostas 1–2, L-110 |
| [Conversa civil](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:370) · [Proveniência](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:385) · [Elegibilidade da foto](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:403) | `M09Conversa` admite o próprio dia civil da abertura. `M10Proveniencia` aceita “validado pelo DP”. `M11Elegibilidade` aceita o rótulo de estoque sem conferir o payload inteiro nem o K6. | FIO-E1/B2, L-007, L-092, LEIS-1-4 resposta 1 |
| [Origem nos escritores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:434) · [Emissão por célula](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:479) | `M26FotoViraAto` reclassifica a foto anterior como ato novo. `M20EmissaoDupla` acorda a mesma célula duas vezes. | Q0.2, L-007, L-111, O-OUTBOX DA PERGUNTA |
| [Falso zero no comparador, vigia, alarme e DRY](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:559) | `M06DatasDaColuna` esconde escrita retrodatada, limpeza pós-marco e `b` nova em disputa antiga. `M12Vigia` e `M13Dry` dão precedência ao estoque quando o PK tem dois campos `a`. | LEIS-1-4 respostas 1–2, L-110, L-114 |
| [Estoque primeiro](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:668) · [Entrega primeiro](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:695) | `M14OrdemDaTrava` pega Colaborador antes de Empresa. `M25ConflitoErrado` produz `55P03` no Colaborador. `M27EntregaPrimeiro` deixa o estoque passar enquanto a entrega segura a Empresa. | L-092, K6, [ordem Empresa → Colaborador](/home/ronald/wt-codex-e4c-f5-instante-012038/app/folha/export.py:556) |
| [Validação com FM aberto](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:773) · [Materialização e passivo](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:804) · [Recusa e devolução](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:836) | `M15SoFM` substitui K6 pelo status do FM. `M16TrilhaVazia` guarda PK e autor sem conteúdo. `M17AlteraTXT` e `M18AlteraBatida` alteram o gravado com o hash do FM intacto. `M19RecusaFutura` desloca o instante da recusa. `M29DevolucaoApagaResposta` limpa a resposta ao devolver apenas a validação. | CORTE-BUG-PAR-FORA163, L-007, L-092, L-113, L-130 |
| [Empate e reexecução da 0058](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:901) | `M21Empate0058` troca `<=` por `<`. O controle repete o `UPDATE` com uma linha já marcada. | LEIS-1-4 resposta 1, L-110, [0058](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16) |

### Perguntas de lei que sobram

- **Qual registro prova quando cada coluna mudou ou foi limpa?** O comparador atual recebe o instante da coluna e a origem da linha. O caso novo exige reconhecer uma escrita posterior com instante retrodatado ou nulo. O controle positivo da c1, congelado em 13/11 com marco em 01/11, continua no recorte para rejeitar a heurística baseada na abertura da disputa. A fonte durável por campo fica [nao verificado]. [Comparador](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4589) · [Controle c1](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_instante_c1.py:78)
- **Payload de proveniência falsa deve ser recusado ou reconstruído? O carimbo recebeu aval para aplicar estoque automaticamente?** O teste aceita recusa ou reconstrução fiel. Para o carimbo, aceita aplicação íntegra ou recusa seguida de aplicação explícita e nova tentativa. O literal do aval para escolher uma dessas formas fica [nao verificado]. [Proveniência](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:385) · [Origem](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:434) [nao verificado]
- **Um ato de fato pode nascer no futuro? Como reparar uma 0058 anterior se um `UPDATE` amplo já tiver sobrescrito a origem e o instante de entrada?** O caso temporal cobra o efeito público no horizonte. O da 0058 caracteriza o empate e a reexecução com uma linha já marcada. A autorização de reparo e o que ocorreu no deploy ficam [nao verificado]. [Backlog E4d](/home/ronald/wt-codex-e4c-f5-instante-012038/app/docs/BACKLOG.md:2171) · [Reversão da 0058](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:29) [nao verificado]

### Comandos para o Code

O recorte inclui a c1 para conferir a fixture de estoque e a devolução especial da L-130. Os mutantes têm um rótulo individual. Execute `morde <rótulo da tabela>` para cada um, depois do verde sem mutante. [Recorte da suíte](/home/ronald/wt-codex-e4c-f5-instante-012038/bin/suite.sh:25) · [Seletores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:399) · [L-130 na c1](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_instante_c1.py:234) [nao verificado]

```bash
wt=/home/ronald/wt-codex-e4c-f5-instante-012038
PYTHONDONTWRITEBYTECODE=1 "$wt/bin/suite.sh" --dir "$wt" --parallel 1 \ [nao verificado]
  --only "chamados.tests.test_e4c_f5_c4_red chamados.tests.test_e4c_f5_instante_c1"

morde() {
  PYTHONDONTWRITEBYTECODE=1 "$wt/bin/suite.sh" --dir "$wt" --parallel 1 \ [nao verificado]
    --only "chamados.tests.e4c_f5_c4red2_mutantes.$1.test_mordida"
}
morde M01DuasValidacoes
```

## 27/11 02:04 · sessao 4caa314c9ba9 · E4C-F5-INSTANTE · verificador · origem=Code

Conferência estática da cópia. O arquivo dos REDs tem 12 métodos `test_` e o blob corresponde ao diff fornecido. A identidade do HEAD, o traceback de `failures=14`, o resultado dos mutantes e a incidência em produção ficam **[nao verificado]**. [app/chamados/tests/test_e4c_f5_c4_red.py:36](/home/ronald/codex_ws/2baf510868c5debc9fbdd6f7a67b353efd7c2bfd/app/chamados/tests/test_e4c_f5_c4_red.py:36)

## Regra e mordida dos 12 REDs

O `RELATO.md:243` registra as quatro perguntas e aponta para o aval. O teste c1 resume as respostas 1 e 2, mas o literal do aval está **[nao verificado]**. Pela L-110, esse resumo dá lastro para comparar instantes e separar novo de estoque. A escolha de um substituto para a data em que a coluna mudou exige fundamento próprio. `app/docs/LEIS.md:136`, `app/docs/RELATO.md:141`, `app/docs/RELATO.md:243`, `app/chamados/tests/test_e4c_f5_instante_c1.py:1`

| RED | Regra que cobra e contraexemplo que escaparia |
|---|---|
| `test_e4c_f5_c4_red.py:36` — carimbo | A resposta 2 de LEIS-1-4 cobra o instante vigente. O aval 1255 citado no modelo descreve a guarda antiga de presença. O teste derruba a busca por qualquer validação em T1, mas fica verde com uma guarda que só confere quando há **duas** validações. Uma única `VALIDACAO(T2)` com coluna em T1 ainda passaria. Também falta a divergência só em `respondida_em`, campo abrangido pela resposta 2. `app/chamados/models.py:3439`, `app/chamados/models.py:4590`, `app/chamados/models.py:4654` |
| `test_e4c_f5_c4_red.py:59` — falha da foto | O261-E-F2-FILA item 3 dá lastro à preservação em memória, com a L-007 como regra de integridade da escrita. O teste injeta a falha num método privado. Restaurar apenas o trio do carimbo nesse ponto deixa o teste verde e ainda permite que uma falha em `foto_da_lavra`, também fora do `try`, deixe validação e candidato na instância. `app/docs/BACKLOG.md:1327`, `app/docs/LEIS.md:65`, `app/chamados/models.py:3465`, `app/chamados/models.py:3510` |
| `test_e4c_f5_c4_red.py:93` — absorção | Resposta 2 de LEIS-1-4 e L-002 para a autoridade comum. Corrigir só a recusa dentro de `carimbar` deixa a guarda posterior da absorção comparando presença e o teste verde. Uma irmã com resposta divergente e validação coincidente ainda escapa de uma cura restrita à validação. O teste aceita as duas origens da recusa e confere só parte do estado após ela. `app/docs/LEIS.md:60`, `app/chamados/models.py:4590`, `app/chamados/services/materializacao.py:1108`, `app/chamados/services/materializacao.py:1115` |
| `test_e4c_f5_c4_red.py:117` — horizonte | O teto por instante tem lastro na L-073 e na regra E2/0514 registrada na carga. A exigência de que `D.composta(candidata)` veja o pendente por essa mesma interface prende o teste ao mecanismo atual. Um contexto privado do C6 poderia cumprir o teto e falhar na linha 139. No outro sentido, cortar só `VALIDACAO` pendente deixa passar `RESPOSTA` pendente futura e dá verde. O teste substitui o C6 e só lê `leituras`, sem conferir a decisão pública da tarefa. `app/docs/LEIS.md:100`, `app/docs/JUIZES.md:81`, `app/chamados/models.py:4549`, `app/chamados/services/tarefa_do_dia.py:390`, `app/chamados/services/tarefa_do_dia.py:429` |
| `test_e4c_f5_c4_red.py:160` — `b` retrodatada | A resposta 1 separa divergência nova de estoque. A data de abertura da disputa prova um limite: pergunta de uma disputa nascida depois do marco só pode ser nova. Ela não data a coluna em disputa antiga. O mutante `coluna >= marco OU disputa.aberta_em >= marco` passa no teste e esconde uma resposta retrodatada gravada hoje em disputa antiga. Também deixa escapar uma pergunta criada hoje dentro daquela disputa. `app/docs/LEIS.md:136`, `app/chamados/models.py:1921`, `app/chamados/models.py:2750`, `app/chamados/models.py:4600` |
| `test_e4c_f5_c4_red.py:173` — marco e depois | Resposta 1 de LEIS-1-4 e fronteira `>=` documentada. As duas datas são insumos sintéticos. Uma guarda que recuse apenas `RESPOSTA` em ou após o marco passa nos dois subcasos e deixa entrar uma `VALIDACAO` igualmente nova. O `ValueError` genérico também aceita uma recusa por motivo alheio. `app/chamados/models.py:4597`, `app/chamados/models.py:4777`, `app/chamados/tests/test_e4c_f5_c4_red.py:153` |
| `test_e4c_f5_c4_red.py:183` — classe `a` | Respostas 1 e 2. `a` pede reprojeção pelo ato vigente, enquanto a inserção histórica é para `b` elegível. O mutante “se existe alguma `RESPOSTA`, rejeite” passa. Ele perde, por exemplo, a resposta histórica vigente sem linha quando já existe uma `RESPOSTA` anterior que foi compensada. A regra de correspondência é por ato e instante. `app/chamados/models.py:4392`, `app/chamados/models.py:4476`, `app/chamados/models.py:4627` |
| `test_e4c_f5_c4_red.py:196` — conversa | FIO-E1/B2 dá lastro ao dia civil sem identidade de fato, e a L-092 fundamenta a guarda do estoque. A fixture põe **14/10** numa conversa criada em **18/10**, enquanto `abrir_conversa` gravaria a data civil do pedido. Uma cura que só rejeitasse conversa com data anterior à abertura ficaria verde e aceitaria a conversa normal de 18/10 com `data_evento=18/10`. `app/docs/LEIS.md:125`, `app/chamados/tests/cortes_f2_f9_apoio.py:31`, `app/chamados/tests/cortes_f2_f9_apoio.py:77`, `app/chamados/models.py:3097`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:74` |
| `test_e4c_f5_c4_red.py:205` — motivo | A L-007 sustenta a fidelidade da trilha. O literal `MOTIVO_DO_CARIMBO` é só um exemplo de proveniência falsa. Bloquear exclusivamente esse literal deixa passar `ato_motivo='validado pelo DP'` na mesma foto. A lei sustenta a proveniência fiel. A exigência de lançar `ValueError` em vez de reconstruir o motivo canônico está **[nao verificado]** no aval. `app/docs/LEIS.md:65`, `app/chamados/models.py:4469`, `app/chamados/models.py:4781`, `app/chamados/models.py:4785` |
| `test_e4c_f5_c4_red.py:215` — origem da foto | Q0.2 e resposta 1 distinguem a foto anterior do ato que grava a coluna hoje. Trocar o rótulo para `estoque` e pular toda a elegibilidade deixa o teste verde. Escapam `entrada_em` diferente do marco, texto errado e foto em competência entregue. Também falta lastro literal para exigir que o carimbo aplique estoque automaticamente: a descrição da origem prevê aplicação com aval em competência aberta, e a porta atual recusa lote misto. `app/chamados/tests/test_e4c_f5_c4_red.py:225`, `app/chamados/models.py:3521`, `app/chamados/models.py:4392`, `app/chamados/models.py:4761`, `app/chamados/models.py:4813` |
| `test_e4c_f5_c4_red.py:234` — entrega concorrente | L-092, K6 e ordem Empresa → Colaborador. `1s`, `8s` e `55P03` são parâmetros e diagnóstico do ensaio. Adquirir a trava correta de Empresa **depois** de já travar o Colaborador dá verde e viola a ordem. O teste também aceita `55P03` vindo de outro ponto da transação do worker e cobre apenas estoque chegando primeiro. `app/docs/LEIS.md:125`, `app/docs/CORTES.md:418`, `app/folha/export.py:556`, `app/chamados/tests/test_e4c_f5_c4_red.py:250`, `app/chamados/tests/_ato_da_fixture.py:21` |
| `test_e4c_f5_c4_red.py:298` — item 13 | FORA163/L-092 sustenta preservar a metade e o gravado, com trilha. L-007 cobra autor, motivo e antes/depois. L-130 sustenta limpar o par na devolução ao colaborador. L-113 preserva o TXT anterior e exige DIFF antes de ato de frota. Mutantes verdes: bloquear pelo status do `FechamentoMensal`, pois a fixture marca **TXT vigente e FM exportado**; gravar uma auditoria com PK e autor, mas sem motivo nem antes/depois; alterar o TXT ou uma batida preexistente sem mudar o hash do FM. Na recusa, até um `RECUSA.ato_em` futuro passa, pois o teste confere o instante **original**, não o da recusa. `app/docs/CORTES.md:564`, `app/docs/CORTES.md:574`, `app/docs/LEIS.md:65`, `app/docs/LEIS.md:151`, `app/docs/LEIS.md:155`, `app/chamados/tests/test_e4c_f5_c4_red.py:290`, `app/chamados/tests/test_e4c_f5_c4_red.py:310`, `app/chamados/tests/test_e4c_f5_c4_red.py:324`, `app/chamados/tests/test_e4c_f5_c4_red.py:349`, `app/ponto/portas/regen_exportada.py:35` |

## Achados do verif2 sem RED suficiente na c4red

- **Falso zero residual.** Faltam `a` retrodatada, `a` por limpeza pós-marco e `b` por escrita nova em disputa antiga. Exemplo com marco 20/10: ato de estoque em 18/10 e coluna limpa no dia 21. O comparador ainda devolve `a` de estoque. O c4red só exige `b` nova em disputa nova. Também falta levar esse caso ao alarme. Vigia e DRY continuam reduzindo por classe em dois lugares. Um PK com dois campos `a`, um novo e outro de estoque, expõe um mutante que dê precedência ao estoque num dos leitores. `app/chamados/tests/test_e4c_f5_c4_red.py:160`, `app/chamados/models.py:4600`, `app/chamados/models.py:4612`, `app/chamados/services/vigia_ato_x_coluna.py:24`, `app/chamados/management/commands/estoque_ato_x_coluna.py:44`, `app/chamados/management/commands/vigiar_fio_morto.py:310`
- **Comportamento temporal além do candidato.** Falta mover o relógio enquanto a absorção espera a trava e conferir o instante efetivamente gravado. O caso fixa T1 para toda a passagem, então uma leitura antes ou depois da trava dá a mesma hora. Também faltam a comparação entre o candidato e as faces persistidas e o efeito na tarefa de uma compensação avançada para o instante de um ato futuro. Há testes anteriores para o `max` do compensador, mas a pergunta sobre o significado desse futuro está registrada para a E4d. `app/chamados/tests/test_e4c_f5_c4_red.py:93`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:86`, `app/chamados/services/materializacao.py:1047`, `app/chamados/services/materializacao.py:1076`, `app/chamados/models.py:3739`, `app/chamados/models.py:4711`, `app/chamados/tests/test_f5_contrato_v2_itens_3_5.py:80`, `app/docs/BACKLOG.md:2171`
- **Origem e escritor único.** O carimbo ainda monta o mesmo tipo `VALIDACAO` da porta pública e a tarefa o interpreta como validação. O RED da foto aceita expressamente esse ato novo. A mesma ambiguidade de origem pode continuar em `validacao` e `disputa_emissao`, que também juntam foto anterior e ato novo. Um exemplo é recusar uma resposta antiga sem linha e inserir a foto com a origem padrão `ato`. A possível emissão dupla para a mesma célula também ficou sem asserção. `app/docs/JUIZES.md:38`, `app/chamados/models.py:3484`, `app/chamados/services/validacao.py:170`, `app/chamados/services/disputa_emissao.py:1757`, `app/chamados/services/tarefa_do_dia.py:173`, `app/chamados/signals.py:252`, `app/chamados/models.py:4755`, `app/chamados/tests/test_e4c_f5_c4_red.py:226`
- **Item 13 e entrega.** Faltam contraprovas entregues com FM aberto e sem TXT vigente, além da checagem do conteúdo da trilha e do TXT preservado. O c4red não passa pelo plantio do passivo nem pelo caminho de validação seguida de devolução. Há testes anteriores para o controle aberto e o passivo U163, e para devolução fora da guarda K6 de metades. Eles não provam a trilha deste recorte. `app/docs/CORTES.md:564`, `app/docs/CORTES.md:580`, `app/chamados/tests/test_e4c_f5_c4_red.py:298`, `app/chamados/tests/test_bug_par_volta.py:533`, `app/chamados/tests/test_bug_par_volta.py:582`, `app/chamados/tests/test_e4c_f5_instante_c1.py:291`
- **0058 e L-130.** A c4red não cobre o empate dos relógios da 0058 nem o cenário de UPDATE anterior já aplicado. O teste anterior prepara um instante de cada lado, e a reversão da migração é `noop`. A devolução especial da L-130 já tinha RED na c1 para a `REABERTURA` marcada e para o efeito na tarefa. A execução do deploy, os relógios reais e a partição atual dos 16.886/71 ficam **[nao verificado]**. `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:29`, `app/chamados/tests/test_e4c_f5_instante_c1.py:234`, `app/chamados/tests/test_e4c_f5_instante_c1.py:370`, `app/docs/RELATO.md:243`

## Fixture e concorrência

**Os cinco `ValueError not raised` são explicáveis pelo código.** São dois subcasos em `:173`, mais `:183`, `:196` e `:205`. Todos passam pela mesma asserção. Com o marco da fixture e K6 aberto, o conferidor atual aceita os payloads que reproduzem a coluna: ele não verifica a classe, consulta K6 pelo `data_evento` até em conversa e deixa `ato_motivo` fora da chave. O freeze de `aberta_em` tem asserções próprias em `:168–169`, e `entrada_em` é fornecido explicitamente nesses payloads. A data artificial da conversa enfraquece a mordida, mas a aceitação atual se explica pela guarda. A atribuição dos cinco tracebacks reais fica **[nao verificado]**. `app/chamados/tests/test_e4c_f5_c4_red.py:146`, `app/chamados/tests/test_e4c_f5_c4_red.py:153`, `app/chamados/tests/test_e4c_f5_c4_red.py:168`, `app/chamados/models.py:1921`, `app/chamados/models.py:4785`, `app/chamados/models.py:4800`, `app/chamados/models.py:4813`

Há ainda **colisão de fixture com a c1** se a cura usar `disputa.aberta_em` como no RED `:160`. A c1 cria o seu estoque positivo sob freeze de **13/11**, com marco de **01/11**. Pela própria expectativa nova, aquela disputa nasce depois do marco e deveria produzir `b` nova. A mesma fixture espera inserir estoque com sucesso. `app/chamados/tests/test_e4c_f5_c4_red.py:160`, `app/chamados/tests/test_e4c_f5_instante_c1.py:49`, `app/chamados/tests/test_e4c_f5_instante_c1.py:83`

**O entrelaçamento é forçado.** Na c3, `registrar` termina `_conferir_estoque`, entra em `_no_ultimo_ato` e só depois faz o `bulk_create`. O patch espera `Future.exception()` antes de devolver o controle. Se o worker termina normalmente, o `atomic` da entrega já comitou antes de o `INSERT` prosseguir. O vermelho esperado vem da janela do código. Atraso superior ao limite pode dar erro do ensaio. A lacuna é que o verde aceita a trava tardia: a fixture já pegou Colaborador ao entrar em `registrar`, e o worker não tenta pegá-lo. Uma transação real que pegue Empresa e depois esse mesmo Colaborador pode formar o ciclo que o RED deixa escapar. `app/chamados/models.py:4737`, `app/chamados/tests/test_e4c_f5_c4_red.py:250`, `app/chamados/tests/_ato_da_fixture.py:21`, `app/folha/export.py:556`, `app/ponto/services/par_relampago_na_lista.py:172`

**REPROVADO** — guardas por instante furáveis, falso zero residual e fixture c1 incompatível, horizonte e relógio incompletos, elegibilidade e proveniência sem prova suficiente, concorrência que aceita ordem invertida e item 13 que aceita falso K6 e trilha vazia de conteúdo. `test_e4c_f5_c4_red.py:36`, `:93`, `:117`, `:160`, `:173`, `:196`, `:205`, `:215`, `:234`, `:298`

## 27/11 01:53 · sessao bb8490509145 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

## A1–A4

| Achado | Contraprova e alcance |
|---|---|
| **A1 — a cura do `DECL_60` é parcial.** | No caso do anexo, 19h–07h, previsto 660, E19h, pausa julgada 07h20–07h40, S08h20, a regra dá **780 trabalhados, base 720, pausa descontada 20, intra 60, HE 0**. A c2l dá esses números na releitura. O mutante `intra_integral` volta a dar **intra 40 e HE50 20**, `rc=1`. Já o mutante que grava `_base_he[id(p)] = 0` passa, `rc=0`. O teste confere os 720 **injetados** em `p.minutos_base_da_he`, e a HE esperada de zero aceita a base interna zerada. Procede como **RED sem mordida sobre a base usada no excesso**. `app/ponto/tests/test_ramo2b_c2h_saida.py:196–209` · `app/ponto/tests/ramo2b_c2l_mutantes.py:33–36,67–70` · `app/ponto/calculador/periodo.py:752–759,833–837` · `app/docs/PROMPTS.md:3071`. |
| **A1 — a releitura ainda perde pausa na borda da entrada.** | 12x36, 19h–07h, previsto 660, pausa cadastrada de 60, E18h40, S18h50, E19h20, S08h20. Entregando a pausa já julgada a `calcular_periodo`, a janela corta a entrada às 19h e conserva **19h–19h20** como pausa. Regra e motor dão **780 trabalhados, base 700, pausa 20, intra 40, HE 0**. `reler` descarta a pausa cujo início coincide com a entrada efetiva e dá **800 trabalhados, base interna 720, pausa 0, intra 60, HE 0**. O campo público `minutos_base_da_he` continua em **700** no mesmo objeto. Procede contra **L-032, L-031 e L-097(b)** e demonstra a derivação paralela. Formação desse par pelas batidas cruas **[nao verificado]**. `app/ponto/motor_calculo_v2.py:2074–2082,2964–2999` · `app/ponto/calculador/periodo.py:304–310,747–759,835–837` · `app/ponto/calculador/alimentacao.py:136–146` · `app/docs/LEIS.md:79–80,115`. |
| **A2 — o repasse funciona quando o chamador entrega o mapa.** | 12x36, 19h–07h, previsto 660, E19h50, S00h, E01h, S07h20, parcial 19h20–19h50. São 630 trabalhados, base 610 e atraso bruto 50. A regra abate 30 e dá **20**. A releitura com mapa dá **20**, sem mapa dá **30** pelo teto. `descarta_parcial` cai em **30 × 20**, `rc=1`. As costuras antigas que julgam pontualidade continuam omitindo o mapa. Procede contra o **corte do abono** nesses caminhos de ensaio. `app/ponto/tests/test_ramo2b_c2h_saida.py:211–228` · `app/ponto/calculador/periodo.py:474–477,792–826` · `app/ponto/tests/test_o280_bateria_da_lei.py:3967–3968,4638–4640,5477–5483` · `app/docs/PROMPTS.md:3031`. |
| **A3 — o selo agora pega a intra zerada, mas aceita o Partido alterado.** | Partido 19h–07h, vão cadastrado 00h–03h, previsto 540, E19h, S00h20, E03h, S07h30. O mensal real recebe vão de 160 e grava **20 de intra**. `zera_intra_partido` dá **0** e derruba os dois subcasos, `rc=1`. Com S07h30, o controle aceita tanto os **560 trabalhados + 30 fora** do main quanto os **590 trabalhados + 0 fora** da cópia, porque soma as duas grandezas. A lacuna original do mensal foi coberta. A proteção do Partido intocado depende do B1, que está vermelho. Procede contra o **aval (a)**. `app/ponto/tests/test_ramo2b_c2h_saida.py:32–51,147–163` · `app/ponto/tests/ramo2b_c2l_mutantes.py:40–43` · `app/ponto/motor_calculo_v2.py:1893–1905,3103–3118,3184–3227` · `app/docs/PROMPTS.md:3087`. |
| **A4 — o mutante de chamada morta cai. A borda da competência continua errada.** | O teste entrega a `medir` um período de 05/11, E19h–S08h20, base e previsto 720, intra 60, declarado 0 e HE injetada 80. A regra dá teto **0** e **1 diagnóstico**, que entra em `falhas_proprias` e `falhas`. Com `testemunha_morta`, a lista fica vazia, `rc=1`. O mesmo selo fica verde enquanto o caso de 20/11 abaixo perde o diagnóstico pelo dia civil. A lacuna original da chamada morta foi coberta. O defeito de data procede contra **L-085/L-002**. `app/folha/tests/test_porta_do_export.py:25–77` · `app/ponto/tests/ramo2b_c2l_mutantes.py:44–46` · `app/folha/porta_export.py:340–362,585–591,713–721,788–790` · `app/docs/LEIS.md:60,117`. |

Outro desvio de **A2**, mesmo com o mapa presente. Comercial com jornada de D às 23h50–07h50, pausa 02h–03h, previsto 420 e D+1 de folga. Batidas E00h02, S02h, E03h, S07h50 em D+1, atribuídas a D. A parcial de D, 23h55–00h02, cobre 7 dos 12 minutos de atraso. Com 408 trabalhados, a regra dá **5**. Na chamada aos juízes em memória, `reler` deu **5** com mapa e **12** sem mapa. O motor deu **0** porque consulta os marcos de `e.date()` e retorna ao encontrar a folga de D+1. Procede contra **L-084/L-085** e o corte do abono. Atribuição automática dessas batidas a D no mensal **[nao verificado]**. `app/ponto/motor_calculo_v2.py:2241–2260,1242–1275,1304–1308` · `app/ponto/calculador/alimentacao.py:278–295` · `app/ponto/calculador/periodo.py:797–803` · `app/docs/LEIS.md:111,116–117` · `app/docs/PROMPTS.md:3031`.

## Números por regime

Valores em minutos, em dia comum. Nos casos com pausa entregue ao par, a comparação começa depois de a pausa ter sido julgada. `app/ponto/motor_calculo_v2.py:2964–2974,3445–3463`.

| Cadastro e batidas | Regra e resultado da cópia |
|---|---|
| **12x36, `DECL_60`**. 19h–07h, previsto 660, E19h, pausa julgada 07h20–07h40, S08h20. | **Regra, motor e `reler` nesse caso:** trabalhado **780**, base **720**, intra **60**, HE **0**. A pausa reduz o trabalhado integral em 20 e o gozo dentro da base é zero. O teto da HE é `660 + 60 = 720`. A L-084 mantém o julgamento porque as duas pontas não estão ambas a mais de 3h. `app/docs/LEIS.md:79–80,115–116` · `app/docs/PROMPTS.md:3071` · `app/ponto/motor_calculo_v2.py:2968–2999,3052–3068` · `app/ponto/tests/test_ramo2b_c2h_saida.py:196–209`. |
| **Comercial, célula congelada 19h–07h**, pausa declarada 00h–01h, previsto 660. Mesmo par do `DECL_60`. | **Regra e releitura:** trabalhado **780**, base **720**, intra **60**, HE **0**. Com esse par entregue, o mensal usa **480** como previsto para a célula que cruza. O teto vira **540** e a conta dá **180 HE**, repartidas pelo código em **120 HE50 + 60 HE100**. No RED de batidas, com E19h50, S00h, E01h, S07h20, a regra dá **630 / 610 / 0 / 0**, e o mensal dá **630 / 610 / 0 / 130**. Procede contra **L-024**. `app/escala/utils.py:2284,2322–2332,2368–2374` · `app/ponto/motor_calculo_v2.py:1978–1985,3430,3464–3485,3555–3560,3617–3653` · `app/ponto/tests/test_ramo2b_c2l_red_motor.py:116–127` · `app/docs/LEIS.md:75`. |
| **Partido, régua relógio**, 19h–07h, vão 00h–03h, previsto 540, E19h50, S00h, E03h, S07h20, parcial 19h20–19h50. | **L-097(b) em geral:** trabalhado **510**, base **490**, intra **0**, HE **0**, atraso **20**. **Aval (a) nesta obra e main:** trabalhado **490**, fora **20**, atraso **20**, intra e HE **0**. **Cópia:** trabalhado **510**, fora **0**, atraso **20**, intra e HE **0**. O desvio é de **escopo do aval**. No outro caso do Partido, E19h/S00h20/E03h/S07h30, o mensal dá **base 560, intra 20, HE 0**. Se esse resultado for passado a `reler`, a v3 mede cada par isolado, perde o vão julgado e dá **base 560, intra 0, HE50 20**. A divergência é de ensaio e também ocorre com S07h00. `app/docs/PROMPTS.md:3087` · `app/docs/LEIS.md:79–80,115` · `app/ponto/motor_calculo_v2.py:3103–3118,3184–3227,3347–3365` · `app/ponto/calculador/periodo.py:334–362,747–759,833–865` · `app/ponto/tests/test_ramo2b_c2l_red_motor.py:51–61`. |

## Escritores, leitores e significado

| Valor | Escrita e leitura na cópia |
|---|---|
| `minutos_intrajornada_indenizada` | **Escrevem:** o construtor inicializa e a rotina de bloqueio zera. 12x36 e Comercial escrevem por par, Partido escreve no segundo par do vão julgado, `periodo_do_dia` produz `DiaV3` e `reler` sobrescreve o `PeriodoCalculo` em memória, inclusive no zeramento de duplicados. `app/ponto/motor_calculo_v2.py:247,1612–1620,2997,3222–3224,3483` · `app/ponto/calculador/periodo.py:183,360–362,739–759`. **Leem em minutos:** `_limite_com_suprimido`, `reler`, a testemunha do export e o PDF para decidir se exibe a coluna. **Convertem para horas:** `ResultadoMes`, `DiaPago`, resumo do espelho e `diff_calculador`. A lavratura usa o mapa da jornada. A testemunha usa a data civil da entrada. `app/ponto/motor_calculo_v2.py:391–398,1978` · `app/ponto/calculador/periodo.py:836` · `app/folha/porta_export.py:340–351` · `app/relatorios/pdf_espelho.py:844–859` · `app/ponto/services/dia_pago.py:173–186,223–225` · `app/ponto/services/espelho.py:104` · `app/ponto/management/commands/diff_calculador.py:244–254`. |
| `_base_he` e `minutos_base_da_he` | O **mapa local** nasce por dia em `reler`, recebe minutos por `id(p)` e só a soma de trabalho o lê para excesso e dobra. O **campo do par** é escrito pela porta da tripla e lido por `_trabalhado_da_base_da_he`, que usa o trabalhado integral quando o campo é `None`. Esse leitor serve ao R3, Partido, Comercial e export. O caso da pausa na borda dá **720 internos × 700 gravados no campo**. `app/ponto/calculador/periodo.py:633,752–756,833–849` · `app/ponto/motor_calculo_v2.py:280–285,1927–1956,2747–2765,3167,3216,3320–3324,3560,3585–3589` · `app/folha/porta_export.py:339–351`. |
| `saida_da_base_da_he` | A janela calcula o **instante** do corte, já com extra declarada, e `_janela_do_par` o limita ao par efetivo. `_gravar_base_da_he` manda gravar pela porta da tripla. O bloqueio limpa pela mesma porta. O construtor gerado aceita escrever o campo diretamente. Leem **R3**, `segmentos_do_motor` para a geometria e a ponta da HE noturna, e `reler`. O corte mede duração até o instante. Se a pausa começa no marco, contribui **0** para a base. Se termina no marco, entra o trecho anterior. O R3 usa a mesma tripla para **soma de pares fechados** num portador aberto, com o instante do último par creditado que tinha cauda. Ocorrência dessa combinação no dado fornecido **[nao verificado]**. `app/ponto/janela_he.py:192–200` · `app/ponto/motor_calculo_v2.py:244–246,1612–1620,1893–1956,2747–2765` · `app/ponto/calculador/alimentacao.py:106–115,168–179` · `app/ponto/calculador/periodo.py:650–653,752–756`. |
| `parciais_por_dia` | A porta do DP escreve `hora_ini/hora_fim`. `parciais_com_ponta_por_dia` monta `{date: [Ausencia]}` por `a.data`. Motor e `rubricas_por_dia` carregam dessa fonte. Em `reler`, o único chamador que fornece o mapa é o teste novo, que o monta à mão. Também omitem o mapa as chamadas da bateria sem `marcos`, nas quais a pontualidade fica fora da releitura. O juiz usa minutos desde 00h do dia da jornada, com madrugada em **1440+**. O marco previsto usa bordas inclusivas. A batida usa direção, com **S no início** e **E no fim** fora. O abatimento mede fatias **`[k,k+1)`** e usa o período, enquanto `Ausencia.minutos` participa da classificação como parcial. `app/ponto/services/ausencia.py:1039–1058,1099–1104` · `app/ponto/services/credito_parcial.py:15–32,54–70` · `app/ponto/motor_calculo_v2.py:481–505,1326–1343` · `app/ponto/calculador/alimentacao.py:535–540,563–566` · `app/ponto/tests/test_ramo2b_c2h_saida.py:222–224` · `app/ponto/tests/test_o280_bateria_da_lei.py:3565,3665–3667,3900,3967,4306,4456,4638,5481–5483` · `app/ponto/turnos.py:2622–2639,2653–2711`. |

## REDs do motor

| RED | Regra, falha e mordida |
|---|---|
| **B1** | O Partido do caso acima dá **490/20/20/0** no main e **510/0/20/0** na cópia, na ordem trabalhado, fora, atraso, HE. Morde o mensal e pede o **aval (a)**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:51–61` · `app/ponto/motor_calculo_v2.py:1893–1905,3103–3118` · `app/docs/PROMPTS.md:3087`. |
| **B2** | 19h–07h, S08h20, pausa julgada 07h20–07h40. Em 12x36, E01h10, regra **base 350 / trabalho 410 / pausa 20 / HE 0**, código **350/430/0/0**. No Comercial, E03h10, regra **230/290/20/0**, código **230/310/0/0**. Morde a perda de desconto depois de a pausa ter sido julgada. A formação automática do par **[nao verificado]**. Procede contra **L-032/L-097(b)**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:63–85` · `app/ponto/motor_calculo_v2.py:2968–2999,3449–3485` · `app/docs/LEIS.md:80,115`. |
| **B3** | Par 19h–08h20, trabalhado 800. O construtor aceita `(720, None, None)` e o leitor numérico vê **720**, enquanto o geométrico conserva **800**. Morde a recusa ausente no construtor. O RED cobre a forma com só `minutos_base_da_he` preenchido. Procede contra **L-007**. Chamador de produção que passe tripla parcial **[nao verificado]**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:87–94` · `app/ponto/motor_calculo_v2.py:244–246,280–285,1927–1943` · `app/ponto/calculador/alimentacao.py:168–174` · `app/docs/LEIS.md:65`. |
| **B4** | Comercial 5x2, cinco turnos 22h–06h, o último até 06h30, HE 0. Pela fórmula semanal escrita no motor e pela L-097(b), **2.430 − 2.640 = −210**. O código reaproveita a base de HE como trabalhado semanal e dá **2.400 − 2.640 = −240**. Morde a troca de significado de `trab_por_dia`. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:96–110` · `app/ponto/motor_calculo_v2.py:3558–3560,3655–3685` · `app/docs/LEIS.md:115`. |
| **Herdado — limite Comercial** | O caso de 630 trabalhados e base 610 pede **HE 0** pela célula de 660. A última asserção morde **120 HE50 + 10 HE100**, na cópia e no main executado. Procede contra **L-024**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:116–127` · `app/ponto/motor_calculo_v2.py:3617–3653` · `59600163c:app/ponto/motor_calculo_v2.py:3504` · `app/docs/LEIS.md:75`. |
| **Herdado — HE noturna** | 12x36, 16h–04h, extra declarada 60, sem prorrogação pós-05h, E16h20/S05h30. A HE50 total é **40**. A regra situa os 40 em **04h20–05h**. A saída crua desloca a faixa para **04h50–05h30** e o motor conta só **10 noturnos**, na cópia e no main executado. Morde o split, contra **L-035/L-097(b)**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:129–144` · `app/ponto/motor_calculo_v2.py:1084–1094` · `59600163c:app/ponto/motor_calculo_v2.py:1051` · `app/docs/LEIS.md:83,115`. |
| **Herdado — export** | Competência fecha em 20/11. Turno de 20/11, 23h50–07h50, pausa 02h–03h sem gozo, E00h02/S08h20 em 21/11. Com HE injetada **30**, intra **60**, base **468**, previsto **420**, o teto é **0** e deve sair **1 diagnóstico**. Sai **0** na cópia e no main executado. O RED morde `medir` pelo descarte na borda, contra **L-085/L-002**. A produção da HE indevida pelo motor nesse caso **[nao verificado]**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:146–160` · `app/folha/porta_export.py:340–362,587–591` · `59600163c:app/folha/porta_export.py:544` · `app/docs/LEIS.md:60,117`. |

## Travas e alcance

- **Juiz e escritor.** A2 delega ao mesmo juiz de pontualidade. Em A1, a releitura de ensaio deriva a base em paralelo à resposta registrada do motor e reescreve o mesmo atributo de intra em memória. Os **720 × 700** da borda mostram o efeito. A testemunha do export também responde pelo dia civil quando já tem acesso ao resultado com dia da jornada. B3 prova a escrita da tripla fora da porta pelo construtor, embora a catraca só encontre a porta na varredura de sítios de produção. `app/core/juizes.py:406,1214–1216` · `app/ponto/calculador/regras.py:112–127,274–285` · `app/ponto/calculador/periodo.py:543–548,747–759` · `app/folha/porta_export.py:344,587–591` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:605–638` · `app/docs/LEIS.md:60,65`.
- **Escopo da entrega.** O hunk c2l acrescenta **0 `except`**. Na porta, o instante continua capturado em `medir` e repassado aos leitores. Inversão de ordem de trava, certificação, pouso prematuro e efeito no gravado **[nao verificado]**. O caminho genérico por evento pode recalcular fora da guarda explícita da 09/2026. Isso dá alcance estático ao desvio do Partido e não comprova dinheiro regravado. `app/folha/porta_export.py:389–394,529,549,693` · `app/docs/PROMPTS.md:3065,3105–3106` · `CLAUDE.md:421–431` · `app/ponto/services/fechamento.py:36–50,559–595,1822–1848` · `app/docs/LEIS.md:155–156`.
- **Prova executada.** Rodei em memória, com substitutos para Django e as fronteiras de I/O, **13 testes, 8 falhas de asserção**, e o recorte de A deu **6 testes verdes**. Os quatro mutantes fornecidos deram `rc=1`. Os três herdados estão numa única classe, em desacordo com o brief que pediu uma classe por RED. A execução Django de `Ran 99`, o DIFF de frota e os números de produção ficam **[nao verificado]**. `app/ponto/tests/ramo2b_c2l_mutantes.py:50–70` · `app/ponto/tests/test_ramo2b_c2l_red_motor.py:50–160` · `app/folha/tests/test_porta_do_export.py:57–77`.

**REPROVADO** — A1 perde pausa na borda e deixa o teste verde com base zerada, A2 deixa chamadores sem a parcial e diverge do motor na virada, A3 aceita o Partido fora do aval, B1–B4 seguem procedentes, e os três herdados mordem os defeitos de limite Comercial, ponta da HE noturna e dia civil no export. [nao verificado]

## 27/11 01:43 · sessao 5872738329b7 · O30-F3-CONSTRUIR · leitura · origem=Code

O saldo informado é de **+11 registros e +1 documento**. A empresa 2 teve +7 registros e +1 documento, a 3 teve +2 e a 4 teve +2. Os totais e o ZIP × CSV 1 a 1 dentro de cada execução deixam em aberto quais PKs entraram, se algum saiu e qual explica o saldo de documentos. [nao verificado]

### O que faz uma raiz entrar na F3

- A F3 seleciona raízes com `documento_referencia_id IS NULL`. Para a requisição da sonda, a view restringe o lote à empresa pedida, aos tipos médicos do catálogo e ao status `aprovada`. A linha do tempo devolve o próprio registro raiz e informa a presença de documento com `bool(a.documento)`. [app/ponto/services/dia_decidido.py:624](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:624) [app/relatorios/views.py:382](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:382) [app/ponto/services/dia_decidido.py:679](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:679)
- A F3 aplica `data <= fim` e aceita qualquer uma destas condições: `data >= ini`, `data_fim >= ini` ou `data_fim IS NULL` com `tipo in Ausencia.TIPOS_EM_ABERTO`. O filtro sem a terceira condição está reproduzido no mutante local, que aceita `data_fim >= ini` ou, com fim nulo, `data >= ini`. **O caso que distingue os dois é a raiz com `data < 2026-10-21`, `data_fim = null` e `tipo_em_aberto = true`**, satisfeitos os demais filtros. O teste da F3 exige essa inclusão para um `afastamento_inss` e também testa o fim fechado exatamente na borda inicial. [app/ponto/services/atestados_lote.py:35](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:35) [app/ponto/turnos.py:2842](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/turnos.py:2842) [app/ponto/tests/mutantes_atestado_f3_c14.py:45](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/tests/mutantes_atestado_f3_c14.py:45) [app/ponto/tests/test_atestado_f3.py:294](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/tests/test_atestado_f3.py:294)
- O código literal do MAIN nos pontos indicados (`atestados_lote.py:46`, `dia_decidido.py:645` e `:675`) fica **[nao verificado]**. A comparação acima usa o predicado reproduzido no mutante da cópia. [app/ponto/tests/mutantes_atestado_f3_c14.py:45](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/tests/mutantes_atestado_f3_c14.py:45)

**Regra aplicável.** O registro do aval `O30-F3-CONSTRUIR-V2-2511` diz que o V2 altera a vez da obra. O corte `AUSENCIAS-DRAWER-E-LOTE` veda montagem própria da janela, e a **L-002** exige que os leitores usem a mesma autoridade. O relato da c15 registra a adoção de `_q_cobertura`, cujo conjunto de tipos em aberto vem do cadastro de fins indeterminados. Para uma raiz com a assinatura acima, a inclusão é comportamento deliberado e testado da F3. O enquadramento de cada PK adicional e do documento adicional fica **[nao verificado]**. [app/docs/PROMPTS.md:3001](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/docs/PROMPTS.md:3001) [app/docs/CORTES.md:349](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/docs/CORTES.md:349) [app/docs/LEIS.md:60](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/docs/LEIS.md:60) [app/docs/RELATO.md:13620](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/docs/RELATO.md:13620) [app/ponto/catalogo/ausencias.py:430](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/catalogo/ausencias.py:430) [app/ponto/tests/test_atestado_f3.py:294](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/tests/test_atestado_f3.py:294)

### Script

O script usa as linhas da view real, ordena por `(empresa, pk)` e imprime o JSONL após sair do `atomic` por rollback. Mantém as guardas do anexo, a 0079 condicional e o storage em memória. `dias_corridos` fica disponível para conferir a apresentação da linha, que usa esse campo quando `data_fim` é nulo. Validei a sintaxe e exercitei os dois caminhos da 0079 com objetos simulados. Execução com Django e PostgreSQL nas duas árvores **[nao verificado]**. [app/relatorios/views.py:404](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:404) [app/ponto/services/dia_decidido.py:679](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:679) [app/ponto/migrations/0079_ausencia_atestado_dados.py:4](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/migrations/0079_ausencia_atestado_dados.py:4)

```python
import json
from datetime import date
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.base import ContentFile
from django.core.files.storage import Storage
from django.db import connection, transaction
from django.db.migrations.executor import MigrationExecutor
from django.template.response import TemplateResponse
from django.test import RequestFactory

from ponto.catalogo import ausencias as catalogo
from ponto.models import Ausencia
from relatorios import views


USUARIO_PK = None
EMPRESAS = (2, 3, 4)
JANELA_ESPERADA = (date(2026, 10, 21), date(2026, 11, 20))
MIGRACAO = ("ponto", "0079_ausencia_atestado_dados")


class DesfazerEnsaio(Exception):
    pass


class MidiaEmMemoria(Storage):
    def _open(self, name, mode="rb"):
        if not name or mode != "rb":
            raise FileNotFoundError("Caminho vazio ou modo de abertura inválido.")
        return ContentFile(b"sonda de atestados em lote\n", name=name)


def preparar_0079():
    campos = {
        campo.column
        for campo in Ausencia._meta.local_fields
        if campo.name.startswith("atestado_")
    }
    if not campos:
        return

    with connection.cursor() as cursor:
        cursor.execute(
            """
            SELECT column_name
            FROM information_schema.columns
            WHERE table_schema = current_schema() AND table_name = %s
            """,
            [Ausencia._meta.db_table],
        )
        presentes = {nome for (nome,) in cursor.fetchall()}

    faltam = campos - presentes
    if not faltam:
        return
    if faltam != campos:
        raise RuntimeError("As colunas da 0079 estão parcialmente presentes.")

    executor = MigrationExecutor(connection)
    migration = executor.loader.graph.nodes[MIGRACAO]
    if executor.migration_plan([MIGRACAO]) != [(migration, False)]:
        raise RuntimeError("O plano pendente precisa conter apenas a aplicação da 0079.")

    estado_anterior = executor.loader.project_state([MIGRACAO], at_end=False)
    with connection.schema_editor(atomic=False) as editor:
        migration.apply(estado_anterior, editor)


def usuario_da_view():
    usuarios = get_user_model().objects.filter(is_active=True)
    if USUARIO_PK is None:
        usuario = usuarios.filter(is_superuser=True).order_by("pk").first()
    else:
        usuario = usuarios.filter(pk=USUARIO_PK).first()
    if usuario is None:
        raise RuntimeError("Usuário ativo da contraprova indisponível.")
    return usuario


def medir_empresa(empresa, usuario):
    request = RequestFactory().get(
        "/relatorios/atestados/lote/",
        {"empresa": empresa, "mes": 11, "ano": 2026},
    )
    request.user = usuario

    with patch.object(views, "render", new=TemplateResponse):
        resposta = views.atestados_lote(request)

    if not isinstance(resposta, TemplateResponse) or resposta.status_code != 200:
        raise RuntimeError("A view não autorizou a consulta do lote.")

    contexto = resposta.context_data
    if "previa" not in contexto:
        raise RuntimeError("Empresa indisponível no filtro da view.")
    if (contexto["ini"], contexto["fim"]) != JANELA_ESPERADA:
        raise RuntimeError("A janela da empresa diverge da competência da contraprova.")

    tipos_em_aberto = set(Ausencia.TIPOS_EM_ABERTO)
    tipos_medicos = set(catalogo.TIPOS_MEDICOS)

    for linha in sorted(contexto["linhas"], key=lambda item: item["ausencia"].pk):
        a = linha["ausencia"]
        yield {
            "empresa": empresa,
            "pk": a.pk,
            "data": a.data.isoformat(),
            "data_fim": a.data_fim.isoformat() if a.data_fim is not None else None,
            "tipo": a.tipo,
            "status": a.status,
            "tem_documento": bool(linha["documento"]),
            "empresa_do_colaborador": a.colaborador.empresa_id,
            "documento_referencia_id": a.documento_referencia_id,
            "tipo_medico": a.tipo in tipos_medicos,
            "tipo_em_aberto": a.tipo in tipos_em_aberto,
            "dias_corridos": a.dias_corridos,
        }


def medir():
    if (
        settings.SETTINGS_MODULE != "config.settings.sombra"
        or connection.vendor != "postgresql"
        or connection.settings_dict["HOST"] != "sombra_db"
        or not connection.features.can_rollback_ddl
    ):
        raise RuntimeError("A medição exige PostgreSQL da sombra com DDL reversível.")

    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        if cursor.fetchone() != ("sombra", "juliani"):
            raise RuntimeError("Banco ou schema diferente do alvo da contraprova.")

    preparar_0079()
    usuario = usuario_da_view()

    campo_documento = Ausencia._meta.get_field("documento")
    with patch.object(campo_documento, "storage", new=MidiaEmMemoria()):
        return [linha for empresa in EMPRESAS for linha in medir_empresa(empresa, usuario)]


try:
    with transaction.atomic(durable=True):
        resultados = medir()
        raise DesfazerEnsaio
except DesfazerEnsaio:
    for resultado in resultados:
        print(json.dumps(resultado, sort_keys=True))
```

## 27/11 01:43 · sessao ba76a83b5eee · PAR-PELO-TURNO · construtor · origem=Code

Corrigi as duas regressões em `ponto/turnos.py`. A conferência por AST deu **`except no diff = 0`**. `ponto/turnos.py:1871` · `ponto/turnos.py:3066` · `ponto/tests/test_par_pelo_turno_bordas.py:13`

- **Col297. O teste guarda a regra correta.** A L-084 e a O73 regem pontualidade. No mapa por PK, a marca reclamada recebe o dia do turno e a marca sem turno recebe o dia local. Na base, o recorte descartava as marcas posteriores a `fim` antes dessa atribuição. A cura devolve **21/09** para as duas marcas da madrugada. O espelho filtra pelo dia atribuído, então elas ficam fora do cartão encerrado em **20/09**. `docs/LEIS.md:116` · `docs/TICKETS.md:308` · `relatorios/tests/test_pdf_sem_regra_propria.py:111` · `f822620f2:app/ponto/turnos.py:3061` · `ponto/turnos.py:3066` · `ponto/turnos.py:3077` · `ponto/services/espelho.py:588`
- **Ata.** Na base, o carregador intermediário chamava o corpo no lugar de `turnos_do_colab`, que é a chamada direta exigida pelo selo. Agora a casca carrega os insumos e chama `_turnos_do_colab_puro` diretamente. O corpo lê a ata e passa `papel_por_minuto` a `parear_turnos`. `f822620f2:app/ponto/turnos.py:1871` · `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:187` · `ponto/turnos.py:1871` · `ponto/turnos.py:1969` · `ponto/turnos.py:2023`

### RED e mutantes

Ensaios em memória, com cargas simuladas. Cada modo executou o RED do mapa e o selo existente da ata. `ponto/tests/test_par_pelo_turno_bordas.py:34` · `ponto/tests/test_par_pelo_turno_bordas.py:79`

| Modo | Saída |
|---|---|
| `base` | `Ran 2 tests`, `FAILED (failures=3)`, rc 1. Faltaram os dois PKs de 21/09 na casca e no lote, e o selo da ata falhou. `ponto/tests/test_par_pelo_turno_bordas.py:45` · `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:187` |
| `cura` | `Ran 2 tests`, `OK`, rc 0. `ponto/tests/test_par_pelo_turno_bordas.py:90` |
| `mapa_sem_borda_direita` | `FAILED (failures=2)`, rc 1. Falharam as duas asserções do mapa. `ponto/tests/test_par_pelo_turno_bordas.py:45` |
| `ata_por_chamada_indireta` | `FAILED (failures=1)`, rc 1. O selo acusou a perda da chamada direta. `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:187` |

Comandos para o Code em `app/`. O executor monta as fontes em memória. `ponto/tests/test_par_pelo_turno_bordas.py:54` · `ponto/tests/test_par_pelo_turno_bordas.py:97`

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci
python3 -m ponto.tests.test_par_pelo_turno_bordas base
python3 -m ponto.tests.test_par_pelo_turno_bordas cura
python3 -m ponto.tests.test_par_pelo_turno_bordas mapa_sem_borda_direita
python3 -m ponto.tests.test_par_pelo_turno_bordas ata_por_chamada_indireta
```

### Recorte solicitado

O runner Django parou na importação com `ModuleNotFoundError: No module named 'django.core'`. Em memória, o módulo do selo deu `Ran 8 tests / OK`. No módulo do PDF passaram os três selos e os dois casos do juiz, col297 e col788. Passaram também os quatro alvos unitários da c1v5a e o controle de paridade entre janelas, `Ran 5 tests / OK`. `manage.py:11` · `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:63` · `relatorios/tests/test_pdf_sem_regra_propria.py:92` · `relatorios/tests/test_pdf_sem_regra_propria.py:127` · `relatorios/tests/test_pdf_sem_regra_propria.py:147` · `ponto/tests/test_par_pelo_turno.py:133` · `ponto/tests/test_par_pelo_turno.py:984`

Os dois casos integrados do cartão e o quinto alvo da c1v5a, de folha e espelho, ficam **[nao verificado]**. `relatorios/tests/test_pdf_sem_regra_propria.py:100` · `relatorios/tests/test_pdf_sem_regra_propria.py:135` · `ponto/tests/test_par_pelo_turno.py:636`

## 27/11 01:42 · sessao 46266bc6bb7d · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**RAMO2-B c2l entregue na base `7f954d921`.** A mudança de produção está na releitura v3. Em memória, A passou em 6 testes, os quatro mutantes retornaram `rc=1` por asserção, e B registrou 7 testes com 8 falhas de asserção porque B2 verifica dois motores. O diff, incluindo os arquivos novos, tem **0 `except` adicionados**. A execução usou `unittest` com dependências de Django substituídas em memória. Suíte Code e certificação pelo opus final [nao verificado]. [periodo.py:747](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:747) · [ramo2b_c2l_mutantes.py:60](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2l_mutantes.py:60) · [test_ramo2b_c2l_red_motor.py:50](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:50)

| Achado | Arquivo e linha | RED observado | GREEN de A |
|---|---|---|---|
| A1 · `DECL_60` | [periodo.py:747](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:747) · [teste:197](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:197) | Trabalhado 780, base 720, intra 40, HE50 20 | Trabalhado 780, base 720, intra 60, HE 0 | [nao verificado]
| A2 · Parcial no 12x36 | [periodo.py:798](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:798) · [teste:211](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:211) | Com parcial, atraso 30 | Com parcial, atraso 20. Sem parcial, 30 | [nao verificado]
| A3 · Partido mensal | [motor_calculo_v2.py:3216](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3216) · [teste:147](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:147) | Os controles anteriores passavam com o mutante. Os novos falham nos dois casos com intra 0 | Intra 20 para saídas às 07h30 e 07h00 | [nao verificado]
| A4 · Testemunha da porta | [porta_export.py:587](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/porta_export.py:587) · [teste:58](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/tests/test_porta_do_export.py:58) | Com a chamada sob `if False`, `medir` devolve `[]` | HE 80 gera 1 diagnóstico, com teto 0 e `falhas=1` | [nao verificado]
| B1 · Partido intocado | [teste:51](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:51) | 510 trabalhados e 0 fora. Esperado 490 trabalhados e 20 fora. `main` deu 490 e 20 | | [nao verificado]
| B2 · Pausa julgada com base curta | [teste:63](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:63) | 12x36 deu 430 trabalhados e pausa 0, esperado 410 e 20. Comercial deu 310 e 0, esperado 290 e 20 | | [nao verificado]
| B3 · Tripla parcial | [teste:87](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:87) | `AssertionError: ValueError not raised` no construtor | | [nao verificado]
| B4 · Saldo do Comercial | [teste:96](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:96) | Trabalhado 2.430, HE 0, saldo −240. Esperado −210 | | [nao verificado]
| Herdado · Limite do Comercial | [teste:116](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:116) | Grade 660, mensal paga HE50 120 e HE100 10. Esperado 0 e 0 | | [nao verificado]
| Herdado · HE noturna | [teste:129](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:129) | Dos 40 min de HE50, 10 saem noturnos. Esperado 40 | | [nao verificado]
| Herdado · Dia civil no export | [teste:146](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:146) | 0 diagnósticos. Esperado 1 para 20/11, com teto 0 | | [nao verificado]

**Herdados no `main`** — a execução das funções de `59600163c` via `git show` devolveu os mesmos números nos três casos, respectivamente `120/10`, `10` e `0` diagnósticos. `59600163c:app/ponto/motor_calculo_v2.py:3504` · `59600163c:app/ponto/motor_calculo_v2.py:1051` · `59600163c:app/folha/porta_export.py:544`

**Escritores e leitores**

- **Base, intra e HE.** O motor grava o marco em `saida_da_base_da_he`. `reler` lê o marco e mede base e intra indenizada no mesmo trecho. O excesso lê os dois valores e `reler` escreve HE total, HE50 e HE100. `ResultadoMes` soma as faixas e a intra indenizada. [motor_calculo_v2.py:1927](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1927) · [periodo.py:752](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:752) · [periodo.py:835](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:835) · [periodo.py:863](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:863) · [motor_calculo_v2.py:352](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:352) · [motor_calculo_v2.py:391](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:391)
- **Parciais e pontualidade.** `credito_parcial` fornece o mapa por dia. O chamador de `reler` o entrega pelo argumento opcional `parciais_por_dia`. O juiz existente recebe as parciais, `reler` grava atraso e saída antecipada, e `ResultadoMes` soma ambos. [credito_parcial.py:62](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/services/credito_parcial.py:62) · [periodo.py:543](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:543) · [periodo.py:474](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:474) · [periodo.py:825](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:825) · [motor_calculo_v2.py:375](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:375)

**Mutantes** — comandos a partir de `app`, no ambiente Django da suíte. Todos retornaram `rc=1` em memória. `zera_intra_partido` derrubou os dois controles de A3. [ramo2b_c2l_mutantes.py:33](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2l_mutantes.py:33) [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci
python3 -m ponto.tests.ramo2b_c2l_mutantes intra_integral
python3 -m ponto.tests.ramo2b_c2l_mutantes descarta_parcial
python3 -m ponto.tests.ramo2b_c2l_mutantes zera_intra_partido
python3 -m ponto.tests.ramo2b_c2l_mutantes testemunha_morta
```

**Recortes para a sessão** — A com vizinhos e B com os REDs intencionais. [bin/suite.sh:25](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/bin/suite.sh:25) [nao verificado]

```bash
RAMO2_WT=/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946
PYTHONDONTWRITEBYTECODE=1 "$RAMO2_WT/bin/suite.sh" --dir "$RAMO2_WT" --only \ [nao verificado]
  "ponto.tests.test_ramo2b_c2h_saida ponto.tests.test_ramo2b_c2_base_da_he folha.tests.test_porta_do_export core.tests.test_medida_turno_do_par"

PYTHONDONTWRITEBYTECODE=1 "$RAMO2_WT/bin/suite.sh" --dir "$RAMO2_WT" --only \ [nao verificado]
  "ponto.tests.test_ramo2b_c2l_red_motor"
```

## 27/11 01:38 · sessao 9e97b3f580bf · O30-F3-CONSTRUIR · leitura · origem=Code

**O `--rodar` grava na sombra antes de chamar o script.** Ele repõe dois `MetricaSnapshot` e grava logs no host. O rollback do Python cobre a 0079 e a execução do relatório. Para cumprir zero escrita persistente na execução inteira, a sessão precisa chamar diretamente o mesmo container e `tenant_command`, com a saída em stdout. [`bin/sombra.sh:136`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/bin/sombra.sh:136), [`bin/sombra.sh:151`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/bin/sombra.sh:151), [`bin/sombra.sh:182`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/bin/sombra.sh:182), [`bin/sombra.sh:483`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/bin/sombra.sh:483)

1. **Chamadas reais.** A view é `relatorios.views.atestados_lote`, na linha 346 da F3 e 345 do main `59600163c`. Ambas exigem usuário autenticado com `ver_relatorios`, `ver_folha` e `ver_ausencias`. A view seleciona uma empresa visível, usa `janela()` e `linhas()` com os tipos médicos do catálogo e, por padrão, só `aprovada`. O script invoca a view real para obter as linhas e a prévia, passa competência 11/2026 e verifica as duas datas. A view usa `request.user`. O PK do usuário histórico ficou [nao verificado]. `USUARIO_PK` permite fixá-lo nas duas execuções. O padrão do script é o superusuário ativo de menor PK. [F3 `app/relatorios/views.py:20`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:20), [F3 `app/relatorios/views.py:345`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:345), [F3 `app/relatorios/views.py:382`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:382), [main `app/relatorios/views.py:344`](/home/ronald/saas-hasner/app/relatorios/views.py:344), [main `app/relatorios/views.py:376`](/home/ronald/saas-hasner/app/relatorios/views.py:376)

   | | F3 | Main |
   |---|---|---|
   | Prévia | `previa(linhas)` — `app/ponto/services/atestados_lote.py:64`, `app/relatorios/views.py:404` | `previa(linhas)` — `app/ponto/services/atestados_lote.py:69`, `app/relatorios/views.py:395` |
   | CSV | `csv_bytes(linhas, pode_ver_anexo=True)` — `app/ponto/services/atestados_lote.py:109`, `app/relatorios/views.py:392` | `csv_bytes(linhas)` — `app/ponto/services/atestados_lote.py:107`, `app/relatorios/views.py:386` |
   | ZIP | `zip_bytes(linhas)` — `app/ponto/services/atestados_lote.py:235`, `app/relatorios/views.py:398` | `zip_bytes(linhas)` — `app/ponto/services/atestados_lote.py:135`, `app/relatorios/views.py:389` |

2. **Registro e documento.** Nas duas versões, um registro é um lançamento raiz selecionado, contado uma vez na prévia e exportado como uma linha de dados do CSV. `com_documento` conta essas linhas quando `bool(ausencia.documento)` é verdadeiro. O inteiro devolvido pelo ZIP conta entradas gravadas após a leitura do anexo. Uma falha de leitura acrescenta uma entrada `.FALTANDO.txt` sem incrementar esse inteiro. A F3 aplica `_q_cobertura` antes de montar a linha do tempo, incluindo o caso de tipos de intervalo aberto. O main monta a linha do tempo por colaborador e usa `data_fim or data` no filtro posterior. [F3 `app/ponto/services/dia_decidido.py:624`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:624), [F3 `app/ponto/services/dia_decidido.py:679`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:679), [F3 `app/ponto/services/atestados_lote.py:35`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:35), [F3 `app/ponto/turnos.py:2842`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/turnos.py:2842), [main `app/ponto/services/atestados_lote.py:46`](/home/ronald/saas-hasner/app/ponto/services/atestados_lote.py:46), [main `app/ponto/services/dia_decidido.py:645`](/home/ronald/saas-hasner/app/ponto/services/dia_decidido.py:645), [main `app/ponto/services/dia_decidido.py:675`](/home/ronald/saas-hasner/app/ponto/services/dia_decidido.py:675)

3. **Mídia vazia.** O ZIP precisa abrir e ler o arquivo. O nome vem da matrícula, data inicial, tipo e extensão de `documento.name`. Com matrícula ausente, o ZIP usa o PK do colaborador e o CSV deixa a matrícula em branco. O script troca apenas o storage do `FileField` durante a medição. Cada caminho não vazio carregado do banco recebe bytes fixos em memória. Isso mede quantas entradas o ZIP real produz com leitura bem-sucedida e quais nomes ele emite. A disponibilidade dos arquivos originais em produção ficou [nao verificado]. [F3 `app/ponto/services/atestados_lote.py:83`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:83), [F3 `app/ponto/services/atestados_lote.py:124`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:124), [F3 `app/ponto/services/atestados_lote.py:247`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:247), [main `app/ponto/services/atestados_lote.py:88`](/home/ronald/saas-hasner/app/ponto/services/atestados_lote.py:88), [main `app/ponto/services/atestados_lote.py:147`](/home/ronald/saas-hasner/app/ponto/services/atestados_lote.py:147)

4. **0079.** Na F3, o queryset carrega instâncias completas de `Ausencia`, então o `SELECT` já precisa das seis colunas. O CSV autorizado também lê os seis atributos. Se faltarem todas as colunas, o script exige que o plano pendente seja apenas a 0079 e executa as operações dessa migration pelo `schema_editor`, sem inserir no histórico de migrations. A única captura é a sentinela fora do `atomic()`. O main segue pelo mesmo script sem aplicar a 0079. [F3 `app/ponto/services/atestados_lote.py:35`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:35), [F3 `app/ponto/services/atestados_lote.py:102`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:102), [F3 `app/ponto/models.py:787`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/models.py:787), [F3 `app/ponto/migrations/0079_ausencia_atestado_dados.py:8`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/migrations/0079_ausencia_atestado_dados.py:8)

5. **Saída.** O casamento usa apenas as linhas `tem_documento=sim` do CSV e remove só a extensão final do nome do ZIP. Uma chave ambígua ou um sufixo de colisão aparece na conferência. O SHA-256 usa todas as chaves do CSV em ordem lexicográfica, incluindo repetições. `contagens_prod_conferem` compara com os números informados. A execução do script ficou [nao verificado]. [F3 `app/ponto/services/atestados_lote.py:121`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:121), [F3 `app/ponto/services/atestados_lote.py:251`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:251), [`logs/O30-F3-CONSTRUIR/smoke_sombra/q.txt:10`](/home/ronald/saas-hasner/logs/O30-F3-CONSTRUIR/smoke_sombra/q.txt:10)

```python
import csv
import hashlib
import json
import warnings
from collections import Counter
from datetime import date, datetime
from inspect import signature
from io import BytesIO, StringIO
from os.path import splitext
from unittest.mock import patch
from zipfile import ZipFile

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.base import ContentFile
from django.core.files.storage import Storage
from django.db import connection, transaction
from django.db.migrations.executor import MigrationExecutor
from django.template.response import TemplateResponse
from django.test import RequestFactory

from ponto.models import Ausencia
from ponto.services import atestados_lote as lote
from relatorios import views


USUARIO_PK = None
ESPERADO_PROD = {2: (119, 116), 3: (7, 7), 4: (4, 4)}
JANELA_ESPERADA = (date(2026, 10, 21), date(2026, 11, 20))
MIGRACAO = ("ponto", "0079_ausencia_atestado_dados")

CSV_COM_PERMISSAO = "pode_ver_anexo" in signature(lote.csv_bytes).parameters


class DesfazerEnsaio(Exception):
    pass


class MidiaEmMemoria(Storage):
    def _open(self, name, mode="rb"):
        if not name or mode != "rb":
            raise FileNotFoundError("Caminho vazio ou modo de abertura inválido.")
        return ContentFile(b"sonda de atestados em lote\n", name=name)


def preparar_0079():
    campos = {
        campo.column
        for campo in Ausencia._meta.local_fields
        if campo.name.startswith("atestado_")
    }
    if not campos:
        return

    with connection.cursor() as cursor:
        cursor.execute(
            """
            SELECT column_name
            FROM information_schema.columns
            WHERE table_schema = current_schema() AND table_name = %s
            """,
            [Ausencia._meta.db_table],
        )
        presentes = {nome for (nome,) in cursor.fetchall()}

    faltam = campos - presentes
    if not faltam:
        return
    if faltam != campos:
        raise RuntimeError("As colunas da 0079 estão parcialmente presentes.")

    executor = MigrationExecutor(connection)
    migration = executor.loader.graph.nodes[MIGRACAO]
    if executor.migration_plan([MIGRACAO]) != [(migration, False)]:
        raise RuntimeError("O plano pendente precisa conter apenas a aplicação da 0079.")

    estado_anterior = executor.loader.project_state([MIGRACAO], at_end=False)
    with connection.schema_editor(atomic=False) as editor:
        migration.apply(estado_anterior, editor)


def usuario_da_view():
    usuarios = get_user_model().objects.filter(is_active=True)
    if USUARIO_PK is None:
        usuario = usuarios.filter(is_superuser=True).order_by("pk").first()
    else:
        usuario = usuarios.filter(pk=USUARIO_PK).first()
    if usuario is None:
        raise RuntimeError("Usuário ativo da contraprova indisponível.")
    return usuario


def contar_repeticoes(contador):
    return sum(quantidade - 1 for quantidade in contador.values())


def medir_empresa(empresa, usuario):
    request = RequestFactory().get(
        "/relatorios/atestados/lote/",
        {"empresa": empresa, "mes": 11, "ano": 2026},
    )
    request.user = usuario

    # Captura o contexto da view real sem renderizar o HTML.
    with patch.object(views, "render", new=TemplateResponse):
        resposta = views.atestados_lote(request)

    if not isinstance(resposta, TemplateResponse) or resposta.status_code != 200:
        raise RuntimeError("A view não autorizou a consulta do lote.")

    contexto = resposta.context_data
    if "previa" not in contexto:
        raise RuntimeError("Empresa indisponível no filtro da view.")
    if (contexto["ini"], contexto["fim"]) != JANELA_ESPERADA:
        raise RuntimeError("A janela da empresa diverge da competência da contraprova.")

    linhas = contexto["linhas"]
    previa = contexto["previa"]
    if CSV_COM_PERMISSAO:
        csv_real = lote.csv_bytes(linhas, pode_ver_anexo=True)
    else:
        csv_real = lote.csv_bytes(linhas)

    # O aviso da biblioteca inclui o nome. A duplicata é contada abaixo.
    with warnings.catch_warnings():
        warnings.filterwarnings("ignore", message=r"^Duplicate name:", category=UserWarning)
        zip_real, documentos_zip = lote.zip_bytes(linhas)

    leitor = csv.DictReader(StringIO(csv_real.decode("utf-8-sig"), newline=""), delimiter=";")
    chaves_csv = []
    chaves_docs_csv = Counter()
    chaves_vazias_csv = 0
    chaves_vazias_docs_csv = 0

    for linha in leitor:
        matricula, data_csv, tipo = linha["matricula"], linha["data"], linha["tipo"]
        data_iso = (
            datetime.strptime(data_csv, "%d/%m/%Y").date().isoformat()
            if data_csv.strip() else ""
        )
        chave = f"{matricula}_{data_iso}_{tipo}"
        vazia = not all(parte.strip() for parte in (matricula, data_iso, tipo))
        chaves_csv.append(chave)
        chaves_vazias_csv += vazia

        if linha["tem_documento"] == "sim":
            chaves_docs_csv[chave] += 1
            chaves_vazias_docs_csv += vazia

    with ZipFile(BytesIO(zip_real)) as arquivo_zip:
        nomes = arquivo_zip.namelist()

    nomes_vazios = sum(not nome.strip() for nome in nomes)
    nomes_duplicados = contar_repeticoes(Counter(nomes))
    chaves_zip = Counter(splitext(nome)[0] for nome in nomes)
    chaves_duplicadas_zip = contar_repeticoes(chaves_zip)
    chaves_duplicadas_docs_csv = contar_repeticoes(chaves_docs_csv)
    marcadores_faltando = sum(nome.endswith(".FALTANDO.txt") for nome in nomes)
    documentos_csv = sum(chaves_docs_csv.values())

    registros_conferem = previa["total"] == len(chaves_csv)
    documentos_conferem = (
        previa["com_documento"] == documentos_csv == documentos_zip == len(nomes)
        and marcadores_faltando == 0
    )
    zip_csv_1_a_1 = (
        registros_conferem
        and documentos_conferem
        and chaves_docs_csv == chaves_zip
        and chaves_vazias_docs_csv == chaves_duplicadas_docs_csv == 0
        and nomes_vazios == nomes_duplicados == chaves_duplicadas_zip == 0
    )
    chaves_serializadas = json.dumps(
        sorted(chaves_csv), ensure_ascii=False, separators=(",", ":")
    ).encode("utf-8")

    return {
        "empresa": empresa,
        "registros_previa": previa["total"],
        "registros_csv": len(chaves_csv),
        "documentos_previa": previa["com_documento"],
        "documentos_csv": documentos_csv,
        "documentos_zip": documentos_zip,
        "entradas_zip": len(nomes),
        "marcadores_faltando_zip": marcadores_faltando,
        "nomes_vazios_zip": nomes_vazios,
        "nomes_duplicados_zip": nomes_duplicados,
        "chaves_duplicadas_zip": chaves_duplicadas_zip,
        "chaves_vazias_csv": chaves_vazias_csv,
        "chaves_duplicadas_csv": contar_repeticoes(Counter(chaves_csv)),
        "chaves_vazias_csv_com_documento": chaves_vazias_docs_csv,
        "chaves_duplicadas_csv_com_documento": chaves_duplicadas_docs_csv,
        "registros_previa_csv_conferem": registros_conferem,
        "documentos_previa_csv_zip_conferem": documentos_conferem,
        "zip_csv_1_a_1": zip_csv_1_a_1,
        "contagens_prod_conferem": (
            registros_conferem
            and documentos_conferem
            and (previa["total"], documentos_zip) == ESPERADO_PROD[empresa]
        ),
        "sha256_chaves_csv": hashlib.sha256(chaves_serializadas).hexdigest(),
    }


def medir():
    if (
        settings.SETTINGS_MODULE != "config.settings.sombra"
        or connection.vendor != "postgresql"
        or connection.settings_dict["HOST"] != "sombra_db"
        or not connection.features.can_rollback_ddl
    ):
        raise RuntimeError("A medição exige PostgreSQL da sombra com DDL reversível.")

    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        if cursor.fetchone() != ("sombra", "juliani"):
            raise RuntimeError("Banco ou schema diferente do alvo da contraprova.")

    preparar_0079()
    usuario = usuario_da_view()

    campo_documento = Ausencia._meta.get_field("documento")
    with patch.object(campo_documento, "storage", new=MidiaEmMemoria()):
        return [medir_empresa(empresa, usuario) for empresa in ESPERADO_PROD]


try:
    with transaction.atomic(durable=True):
        resultados = medir()
        raise DesfazerEnsaio
except DesfazerEnsaio:
    for resultado in resultados:
        print(json.dumps(resultado, sort_keys=True))
```

## 27/11 01:31 · sessao d67b8bc0162f · E4C-F5-INSTANTE · construtor · origem=Code

Escrevi 12 testes em [test_e4c_f5_c4_red.py](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1). Cada caso começa pela regra, pelo cadastro e pelo resultado esperado. A alteração ficou no arquivo novo, com zero `except` novos. [nao verificado]

A execução do módulo parou no import, antes das fixtures. O Python local devolveu `ModuleNotFoundError: No module named 'django.contrib'`, rc 1. A suíte da cópia usa PostgreSQL. Em memória, executei dois métodos do arquivo novo e oito recortes das funções reais da cópia. A saída foi `Ran 10 tests`, `FAILED (failures=10)`, rc 1, todas por `AssertionError`. As três provas de integração indicadas na tabela seguem **[nao verificado]**. `app/chamados/tests/test_e4c_f5_c4_red.py:9`, `app/config/settings/ci.py:6`

| Achado | Teste em `test_e4c_f5_c4_red.py` | Saída RED | Cura esperada | [nao verificado]
|---|---:|---|---|
| Carimbo aceita validação vigente em T2 com coluna em T1 | `:36` | `(True, None) != (False, 'par_ato_coluna')` | `chamados/models.py::DecisaoPergunta.par_ato_coluna` e `PerguntaDisputa.carimbar` devem conferir o instante vigente. `models.py:3499`, `models.py:4646` |
| Absorção aceita o mesmo par divergente | `:93` | **[nao verificado]**. O teste exige recusa por PK, trilha e ausência de `ABSORCAO`. | `chamados/services/materializacao.py::_materializar_ausencia_de_resposta` deve usar a mesma comparação por instante. `materializacao.py:1108`, `materializacao.py:1115` |
| Falha ao ler a foto vaza o carimbo em memória | `:59` | Instante, autor `7` e via `juiz_aprovou` ficaram na instância. | `chamados/models.py::PerguntaDisputa.carimbar` deve ler a foto antes de alterar a instância ou incluir essa leitura na proteção existente. `models.py:3465`, `models.py:3473`, `models.py:3490` |
| `ato_em` pendente escapa ao horizonte da tarefa | `:117` | O horizonte de 12h viu a validação pendente de 13h. | `chamados/models.py::DecisaoPergunta.leitura` e `chamados/services/tarefa_do_dia.py::_fatos_das_perguntas` devem limitar o candidato ao contexto do carimbo e respeitar o horizonte da tarefa. `models.py:4549`, `tarefa_do_dia.py:416` |
| Coluna retrodatada em disputa nova conta como estoque | `:160` | `[('b', 'respondida', False)] != [('b', 'respondida', True)]` | `chamados/models.py::DecisaoPergunta.divergencia_ato_x_coluna` deve usar a abertura posterior da disputa como prova de que a pergunta nasceu depois do marco. `models.py:1921`, `models.py:4615` |
| Porta aceita estoque no marco e depois | `:173` | `ValueError not raised` nos dois instantes. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve admitir somente o campo classificado como `b` de estoque. `models.py:4615`, `models.py:4800` |
| Porta aceita foto de campo `a` com ato vigente | `:183` | `ValueError not raised`. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve recusar a foto e preservar a autoridade do ato vigente para a reprojeção. `models.py:4611`, `models.py:4801` |
| Porta aceita dia civil de conversa como dia do fato | `:196` | `ValueError not raised`. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve excluir conversa sem competência de fato e consultar o K6 existente para as elegíveis. `models.py:3191`, `models.py:4813`, `backfill_decisoes_pergunta.py:74` |
| Payload de estoque atribui a foto ao carimbo | `:205` | `ValueError not raised`. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve recusar o motivo adulterado. `models.py:4469`, `models.py:4781`, `models.py:4785` |
| Foto histórica ganha origem de ato novo | `:215` | `'ato' != 'estoque'` no payload com o default do modelo. | `chamados/models.py::PerguntaDisputa.carimbar` e `DecisaoPergunta.registrar` devem preservar a proveniência da foto e a entrada própria da validação nova. `models.py:3473`, `models.py:3526`, `models.py:4392` |
| Entrega pode intercalar a leitura do K6 e o `INSERT` | `:234` | **[nao verificado]**. A prova usa duas conexões e a trava real de entrega. | A entrada do estoque em `chamados/models.py::DecisaoPergunta.registrar` deve serializar com `folha/export.py::travar_entrega`, respeitando Empresa antes de Colaborador. `models.py:4737`, `models.py:4742`, `folha/export.py:556` |
| Item 13 por PK com entrega real pelo K6 | `:298` | **[nao verificado]**. O teste verifica as três portas, atos, colunas, trilha e hash do mês do fato. | `chamados/services/validacao.py::validar_pergunta` e `chamados/services/materializacao.py::_materializar_uma` devem registrar o bloqueio com autor e PK. A recusa serve de controle com a trilha já emitida pela porta. `validacao.py:792`, `materializacao.py:1981`, `validacao.py:1017` |

**Achados sem RED adicional**

- Regravação retrodatada ou limpeza em pergunta já existente no marco pode produzir a mesma foto persistida que um passivo legítimo. O teste usa a abertura da disputa como evidência apenas no caso em que ela resolve a ambiguidade. As fixtures antigas que pretendem simular estoque também precisam representar uma disputa anterior ao marco se a cura usar essa evidência. `app/chamados/models.py:4600`, `app/chamados/tests/test_e4c_f5_instante_c1.py:83`
- A 0058 já recorta `entrada_em <= marco`, com teste na função real. Ordenação dos relógios no deploy e recuperação de timestamps sobrescritos seguem **[nao verificado]**. `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16`, `app/chamados/tests/test_e4c_f5_instante_c1.py:370`
- O censo de `RESPOSTA`, a devolução L-130, a colisão Q0.2 e a saída DRY já têm casos próprios. A contagem e a lista de PKs reais, o efeito monetário e o DIFF de frota seguem **[nao verificado]**. `app/chamados/tests/test_f2_resposta_mantem_validacao.py:20`, `app/chamados/tests/test_e4c_f5_instante_c1.py:234`, `app/chamados/tests/test_e4c_f5_instante_c1.py:340`, `app/chamados/tests/test_e4c_f5_instante_c1.py:142`, `app/docs/LEIS.md:155`
- O relógio da passagem da ausência tem um contrato próprio de horizonte, e o `max` dos atos compensadores está previsto no contrato V2. A janela de candidato foi testada. Uma mudança na data de `ABSORCAO`, o contrato geral de ato futuro e a escolha do escritor semântico da validação automática requerem conciliar os contratos citados. `app/chamados/services/materializacao.py:1047`, `app/chamados/services/materializacao.py:1100`, `app/chamados/models.py:4691`, `app/docs/BACKLOG.md:2171`, `app/docs/JUIZES.md:38`, `app/chamados/models.py:3484`
- Vigia e DRY duplicam a consolidação por PK, com a mesma regra observável no código atual. Extrair a consolidação por si só não dá um RED de comportamento. `app/chamados/services/vigia_ato_x_coluna.py:24`, `app/chamados/management/commands/estoque_ato_x_coluna.py:44`

**Comando para a sessão**

```bash
bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-instante-012038 \
  --only "chamados.tests.test_e4c_f5_c4_red"
```

Para os mutantes, após o verde, reintroduzir o comportamento da primeira coluna uma vez por caso e rodar o seletor correspondente. A aplicação e a execução dos mutantes seguem **[nao verificado]**. A certificação fica com o Code e o opus final. `app/docs/LEIS.md:147`

```bash
red() {
  bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-instante-012038 \
    --only "chamados.tests.test_e4c_f5_c4_red.$1"
}

red PortaDoCarimboTest.test_RED_carimbo_recusa_validacao_vigente_em_outro_instante
red AbsorcaoDoParTest.test_RED_absorcao_preserva_irma_com_validacao_em_instante_divergente
red PortaDoCarimboTest.test_RED_falha_da_foto_preserva_o_carimbo_em_memoria
red HorizonteDoCarimboTest.test_RED_candidato_futuro_nao_existe_no_horizonte_da_tarefa
red EstoqueDoParTest.test_RED_coluna_retrodatada_em_disputa_nova_conta_como_b_novo
red EstoqueDoParTest.test_RED_porta_estoque_recusa_instante_no_marco_ou_depois
red EstoqueDoParTest.test_RED_porta_estoque_recusa_a_com_ato_vigente
red EstoqueDoParTest.test_RED_porta_estoque_recusa_dia_civil_de_conversa
red EstoqueDoParTest.test_RED_porta_estoque_nao_grava_motivo_de_ato_do_carimbo
red EstoqueDoParTest.test_RED_foto_anterior_carimbada_hoje_conserva_origem_historica
red EstoqueConcorrenteComEntregaTest.test_RED_entrega_nao_intercala_k6_e_insert_do_estoque
red Item13PorPKTest.test_RED_item13_portas_publicas_em_entrega_vigente_com_trilha_por_pk
```

_8549 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
