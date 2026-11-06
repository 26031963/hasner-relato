# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 03:38** · publicado em **06/11/2026 03:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 03:38 · sessao 873fb725b2fe · O261-ITEM3-PEP · verificador · origem=Code

1. **OBSERVAÇÃO — os PROCEDENTES da v1 foram fechados parcialmente.** Uso **“v2 virtual”** para a numeração após incorporar o anexo em memória; as demais referências são da cópia. Execução dos testes, dos mutantes e ocorrência dos casos em produção: **[não verificado]**. [nao verificado]

   | Procedente da triagem | O v2 fecha? |
   |---|---|
   | **1a — reentrada S143 na mesma transação** | **Fecha a recusa específica:** encontrando o carimbo anterior aceito pelo C6, retorna `True` sem sobrescrevê-lo. Isso evita `_Reprovada` naquele caminho. Porém estende o mesmo sucesso a atos independentes, com efeitos indevidos nos consumidores descritos abaixo. (`app/chamados/models.py:2598`, **v2 virtual**; `app/chamados/services/materializacao.py:1204`, cópia) |
   | **1b — falha no commit próprio** | **Fecha o posicionamento do tratamento:** o `except` agora envolve a saída do `with` e restaura as fotografias existentes. O teste novo provoca falha de commit, mas verifica somente carimbo, sem solicitar lavra. (`app/chamados/models.py:2595`, **v2 virtual**; `app/chamados/models.py:2628`, **v2 virtual**; `app/chamados/tests/test_o261_item3_pep.py:307`, **anexo**) |
   | **2b — resposta nova na ordem inversa** | **Não fecha.** O escritor continua decidindo limpar validação a partir da instância recebida e pode salvar apenas resposta/instante, preservando uma validação gravada depois daquela leitura. Ser anterior ao patch sustenta a classificação como dívida existente; não sustenta dizer que a proteção é bilateral. (`app/chamados/services/disputa_emissao.py:1284`; `app/chamados/services/disputa_emissao.py:1286`; `app/chamados/services/disputa_emissao.py:1301`) |
   | **3 — textos atribuíam toda recusa ao C6** | **Fecha a causa “mudança humana”, mas não toda a veracidade.** Acrescentou essa alternativa; permanecem “segue pendente”, nome de evento “viva” e a causa omitida “linha sumiu”. (`app/chamados/services/materializacao.py:552`, **v2 virtual**; `app/chamados/services/materializacao.py:1206`, **v2 virtual**; `app/chamados/models.py:2661`, **v2 virtual**) |
   | **5 — retirar um campo isolado da guarda sobrevivia** | **Fecha por inspeção dos testes:** o novo `subTest` altera separadamente texto, instante da resposta, instante e autor da validação. A execução dos quatro mutantes continua **[não verificado]**. (`app/chamados/tests/test_o261_item3_pep.py:99`, **anexo**) |

   **OBSERVAÇÃO — a janela da validação também permanece:** lê e julga antes do `atomic`, que começa somente para gravar a assinatura. (`app/chamados/services/validacao.py:359`; `app/chamados/services/validacao.py:374`; `app/chamados/services/validacao.py:395`)

2. **PROCEDENTE-candidato — dois motivos de DESCARTE são abrangentes demais.**

   | Descarte | Conferência do motivo escrito |
   |---|---|
   | **1c e 7b — conteúdo das relações** | **A ressalva sobre julgamento em curso se sustenta.** Existe projeção explícita de célula, e a ressuscitação a utiliza. Mas isso não torna qualquer relação carregada uma fotografia mais nova que o banco. O E2 consulta normalmente célula/disputa/destino, e o cartório também tem caminho que chama E2 **depois** da lavra. Preservar indistintamente essas relações mantém o contraexemplo de cache obsoleto. (`app/chamados/juizes.py:383`; `app/chamados/models.py:1450`; `app/chamados/reconciliador.py:641`; `app/ponto/services/cartorio.py:953`; `app/ponto/services/cartorio.py:959`) |
   | **1c — disputa/destino incluídos no descarte** | **A justificativa da célula não os cobre.** O juiz lê contexto da disputa e status do destino; o laço novo copia apenas campos concretos da pergunta, preservando relações com a mesma FK. (`app/chamados/juizes.py:364`; `app/chamados/juizes.py:743`; `app/chamados/models.py:2663`, **v2 virtual**) |
   | **1c — reler célula exigiria inverter trava** | **Não demonstrado.** Releitura e aquisição de trava são operações distintas; a relavra de referência carrega célula, mas seu `of` declara somente pergunta/disputa. Ciclo concreto com a porta da célula: **[não verificado]**. (`app/chamados/services/relavra_via.py:221`; `app/chamados/services/relavra_via.py:222`) |
   | **1d — campo deferido** | **Sustenta rebaixar a prioridade, não a justificativa geral.** Os chamadores conferidos carregam perguntas completas. Entretanto, decidir por filtro SQL sobre um campo não exige carregar seu atributo: o hipotético `filter(validada_em__isnull=True).only('pk')` já decide sobre ele. Chamador atual com essa projeção: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1572`; `app/chamados/reconciliador.py:641`; `app/chamados/services/materializacao.py:1343`; `app/chamados/models.py:2657`, **v2 virtual**) |
   | **5b — sempre abrir `atomic()` só mudaria SAVEPOINT** | **Não se sustenta como equivalência comportamental.** Savepoint muda a contenção de erro quando alguém captura a exceção. O repositório documenta justamente rollback externo provocado por erro engolido sem savepoint. S144 captura exceção do materializador sem criar seu próprio savepoint. Isso não obriga mudar a candidata escolhida, mas invalida o motivo do descarte. (`app/core/observ.py:124`; `app/chamados/signals.py:83`; `app/chamados/signals.py:139`; `app/chamados/signals.py:141`) |

3. **PROCEDENTE-candidato — o novo `True` mistura “estado aceito” com “ato gravado”.** O contrato escrito continua `bool (gravou?)`, mas o ramo já carimbado retorna antes de escrever, completar resposta/validação ou lavrar. Também precede a comparação dos campos humanos. (`app/chamados/models.py:2563`; `app/chamados/models.py:2602`, **v2 virtual**; `app/chamados/models.py:2665`, **v2 virtual**; `app/chamados/models.py:2667`, **v2 virtual**)

   Para os oito chamadores, considerando carimbo de **outra via, autor ou instante**:

   | Chamador | Consequência de `True` e avaliação |
   |---|---|
   | **`lavrar_sem_turno_aberto`** | Pode fechar a disputa, grava resolução “sem turno aberto” e retorna sucesso. O fechamento continua condicionado a `pode_fechar`; o retorno já não comprova que esta chamada lavrou aquela via. (`app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:411`; `app/chamados/reconciliador.py:412`; `app/chamados/reconciliador.py:414`) |
   | **`retratar_perguntas_absolvidas_por_celula`** | Incrementa `n`, agenda disputa e pode fechá-la atribuindo resolução à célula. O número documentado como “perguntas carimbadas” passa a incluir carimbo alheio encontrado. (`app/chamados/reconciliador.py:551`; `app/chamados/reconciliador.py:588`; `app/chamados/reconciliador.py:590`; `app/chamados/reconciliador.py:603`) |
   | **S143, ramo principal** | Incrementa `perguntas_resolvidas`, inclui disputa e pode fechá-la. Aceitar a reentrada própria resolve 1a; contabilizar ato independente não prova resolução efetuada nesta chamada. A lavra solicitada também é pulada nesse retorno. (`app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/services/disputa_emissao.py:1607`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/models.py:2602`, **v2 virtual**) |
   | **S143, ramo sem credor** | **Emite trilha incorreta:** `fecho_sem_credor`, ator atual, fotografia anterior sem carimbo e `depois.materializada_em` igual ao horário da tentativa. O banco conserva o instante alheio. Esse ramo não incrementa `n_pq`; ainda participa da cauda de fechamento. (`app/chamados/services/disputa_emissao.py:1666`; `app/chamados/services/disputa_emissao.py:1678`; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/services/disputa_emissao.py:1689`) |
   | **`_materializar_ausencia_de_resposta`** | Não registra recusa; conserva a Ausência já criada, reconcilia destino e pode fechar disputa. Preservar decisão alheia é adequado; isso não significa que a irmã foi absorvida neste ato: o agrupamento da absorção depende do mesmo `materializada_em`. (`app/chamados/services/materializacao.py:520`; `app/chamados/services/materializacao.py:549`; `app/chamados/services/materializacao.py:564`; `app/chamados/services/materializacao.py:568`; `app/chamados/services/materializacao.py:593`) |
   | **`_materializar_uma`** | Conserva plantio/reuso anterior ao carimbo, marca sucesso e aumenta `materializadas`. Correto para a reentrada S143 do próprio plantio; para ato independente, a morte pelo C6 não demonstra que o plantio baseado na leitura anterior continua válido. Nesse ramo nem se aplica a comparação humana. (`app/chamados/services/materializacao.py:1145`; `app/chamados/services/materializacao.py:1147`; `app/chamados/services/materializacao.py:1207`; `app/chamados/services/materializacao.py:1302`; `app/chamados/models.py:2665`, **v2 virtual**) |
   | **`materializar_pendentes_aprovadas`** | Acrescenta PK a `feitas`; não emite evento nesse trecho, e o receiver ignora a lista. Preserva o carimbo, mas a lista deixa de representar exatamente as perguntas carimbadas pela chamada. (`app/chamados/services/materializacao.py:1317`; `app/chamados/services/materializacao.py:1349`; `app/chamados/services/materializacao.py:1350`; `app/chamados/signals.py:140`) |
   | **`encerrar_fantasma`** | Propaga `True`; o consumidor emite evento de encerramento pela lavra, incrementa `encerradas` e agenda disputa. **Contraria o contrato local de trilha somente quando escreveu**, embora o carimbo seja de outro ato. (`app/chamados/models.py:2685`; `app/chamados/reconciliador.py:1107`; `app/chamados/reconciliador.py:1138`; `app/chamados/reconciliador.py:1145`; `app/chamados/reconciliador.py:1146`) |

   **PROCEDENTE-candidato — os textos de `False` ainda não dizem toda a verdade:**

   - Se `validada_em` mudou de nulo para preenchido, a guarda retorna `False` sem chamar C6; o C6 real já considera essa pergunta morta. Portanto “segue pendente” e a ação `irma_viva_na_ausencia` não decorrem do retorno. (`app/chamados/models.py:2667`, **v2 virtual**; `app/chamados/juizes.py:1136`; `app/chamados/services/materializacao.py:553`, **v2 virtual**; `app/chamados/services/materializacao.py:573`, cópia)
   - Linha desaparecida também retorna `False`, sem provar nenhuma das duas causas escritas nos novos textos. (`app/chamados/models.py:2661`, **v2 virtual**; `app/chamados/services/materializacao.py:552`, **v2 virtual**; `app/chamados/services/materializacao.py:1206`, **v2 virtual**)
   - A frase sobre a batida sair do espelho continua sustentada pelo `_Reprovada` dentro do `atomic` da unidade. (`app/chamados/services/materializacao.py:1145`; `app/chamados/services/materializacao.py:1204`; `app/chamados/services/materializacao.py:1208`)

4. **OBSERVAÇÃO — delta de escritores e leitores em relação à v1.**

   | Campos | Quem escreve / quem lê no delta |
   |---|---|
   | **`materializada_em`, `materializada_por`, `via_resolucao`** | Nenhum escritor persistente novo. A atribuição continua em `_escrever_carimbo`; o v2 passa a preservar os três quando encontra carimbo existente. Esse estado agora alimenta sucesso nos oito consumidores, inclusive suas trilhas. (`app/chamados/models.py:2556`; `app/chamados/models.py:2602`, **v2 virtual**; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/reconciliador.py:1143`) |
   | **`resposta_colab`** | Continua escrita pelo core; não é escrita por `carimbar`. A comparação humana já existia na proposta v1; no v2 ela fica subordinada à ausência de carimbo. O C6 passa a lê-la também no ramo já carimbado. (`app/chamados/services/disputa_emissao.py:1282`; `app/chamados/models.py:2665`, **v2 virtual**; `app/chamados/models.py:2667`, **v2 virtual**; `app/chamados/juizes.py:1150`) |
   | **`respondida_em`, `validada_em`, `validada_por`** | Escritores permanecem core, validação e complemento da própria porta. O v2 deixa de completar esses campos no retorno antecipado. Logo `_CAMPOS_DO_HUMANO` não significa exclusividade humana de escrita. (`app/chamados/services/disputa_emissao.py:1283`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/validacao.py:131`; `app/chamados/models.py:2611`, **v2 virtual**; `app/chamados/models.py:2602`, **v2 virtual**) |
   | **`veredito_lavrado`, `julgado_em`, `janela_expira_em`** | Escritor/restaurador continuam na porta da lavra. A fotografia mudou para depois da aprovação do C6; a recuperação passou a alcançar o commit próprio. Já carimbada retorna sem lavrar, mesmo com `lavrar_veredito=True`. (`app/chamados/veredito_lavrado.py:227`; `app/chamados/veredito_lavrado.py:235`; `app/chamados/veredito_lavrado.py:240`; `app/chamados/models.py:2622`, **v2 virtual**; `app/chamados/models.py:2602`, **v2 virtual**) |
   | **`_pv_memo`** | Escritor/leitor continuam em `pergunta_viva`. O novo ramo já carimbado executa C6 após o `pop` e pode deixar um memo novo. A chave continua sem cobrir ata/DNA/batidas; invalidar o memo não renova relações preservadas. (`app/chamados/juizes.py:811`; `app/chamados/juizes.py:835`; `app/chamados/juizes.py:847`; `app/chamados/models.py:2660`, **v2 virtual**; `app/chamados/models.py:2664`, **v2 virtual**) |

5. **OBSERVAÇÃO — a recuperação não usa fotografia inexistente, mas seu alcance transacional tem limites.**

   **Antes da fotografia:** `antes = lavra_antes = None` e as guardas do `except` evitam restauração inválida. Releitura, julgamento de carimbo existente, relógio e obtenção do autor precedem o carimbo novo; a lavra é fotografada antes de sua alteração. (`app/chamados/models.py:2594`, **v2 virtual**; `app/chamados/models.py:2607`, **v2 virtual**; `app/chamados/models.py:2622`, **v2 virtual**; `app/chamados/models.py:2629`, **v2 virtual**)

   **Commit do chamador:** quando há transação externa, a porta retorna antes desse commit. Uma falha posterior não atravessa seu `except`; a instância pode conservar estado posteriormente revertido. Essa limitação permanece. (`app/chamados/models.py:2596`, **v2 virtual**; `app/chamados/models.py:2627`, **v2 virtual**)

   **Depois de SQL efetivado:** se um `save()` concluir o UPDATE e algum hook lançar exceção Python depois, `nullcontext` não desfaz SQL. A porta restauraria memória e um chamador que capturasse a exceção poderia confirmar o banco. **Não encontrei esse caminho ordinário demonstrado nos receivers atuais**: o receiver da pergunta agenda trabalho protegido por `engolir`. Ocorrência concreta desse cenário: **[não verificado]**. (`app/chamados/models.py:2596`, **v2 virtual**; `app/chamados/models.py:2626`, **v2 virtual**; `app/chamados/models.py:2632`, **v2 virtual**; `app/chamados/signals.py:197`; `app/core/observ.py:85`)

6. **PROCEDENTE-candidato — os testes mordem, mas ainda há mutante plausível sobrevivente.** [nao verificado]

   **Mordidas por inspeção:** os quatro campos isolados defendem a guarda; o caso já carimbado/vivo exige consultar C6; a reentrada exige aceitar o carimbo anterior; a captura SQL exige trava antes do juiz; o teste transacional exige recuperação após falha de commit. Execução dessas mordidas: **[não verificado]**. (`app/chamados/tests/test_o261_item3_pep.py:99`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:144`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:163`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:263`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:307`, **anexo**)

   **Mutante:** restaurar lavra no `except` apenas quando `_cx.in_atomic_block`, mantendo recuperação do carimbo incondicional. Os testes antigos de falha de lavra/save herdam `TestCase`; o novo de commit não pede lavra nem verifica seus três campos. Assim, a cobertura apresentada deixa passar lavra residual em memória após commit próprio falho com `lavrar_veredito=True`. Execução do mutante: **[não verificado]**. (`app/chamados/models.py:2629`, **v2 virtual**; `app/chamados/tests/test_o261e_f2_fila.py:143`; `app/chamados/tests/test_o261e_f2_fila.py:190`; `app/chamados/tests/test_o261e_f2_fila.py:200`; `app/chamados/tests/test_o261_item3_pep.py:307`, **anexo**)

   **OBSERVAÇÃO — a reentrada nova é um teste de costura simulada:** substitui plantio e julgamento; não percorre o S143 real nem verifica os eventos dos outros consumidores. Também permanece sem defesa a diferença entre projeção legítima e relação obsoleta com a mesma FK. (`app/chamados/tests/test_o261_item3_pep.py:170`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:176`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:233`, **anexo**)

7. **OBSERVAÇÃO — enquadramento nas sete classes.**

   | Classe | Resultado |
   |---|---|
   | **(a) Segundo juiz** | **Não demonstrado.** Reutiliza o C6 registrado; comparar fotografia humana é pré-condição de concorrência. (`app/core/juizes.py:266`; `app/chamados/models.py:2602`, **v2 virtual**; `app/chamados/models.py:2667`, **v2 virtual**) |
   | **(b) Segundo escritor** | **Não há nova porta persistente demonstrada.** Há cópia dinâmica em memória; o selo declara não alcançar nomes vindos de atributo. (`app/chamados/models.py:2664`, **v2 virtual**; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113`) |
   | **(c) Relógio fora da borda** | **Dívida mantida:** fallback `now()` e `encerrar_fantasma` sem repassar o instante recebido pelo consumidor. (`app/chamados/models.py:2605`, **v2 virtual**; `app/chamados/models.py:2685`, cópia; `app/docs/BACKLOG.md:1154`) |
   | **(d) Derivação paralela** | **Não demonstrada para vitalidade.** O problema encontrado é fornecer insumo relacionado obsoleto ao juiz existente e interpretar sua resposta como autoria do ato. (`app/chamados/models.py:2664`, **v2 virtual**; `app/chamados/juizes.py:1181`; `app/chamados/services/disputa_emissao.py:1683`) |
   | **(e) Selo/RED sem mordida** | **Cobertura parcial, não vacuidade total.** Há testes discriminantes e o mutante residual do item 6. (`app/chamados/tests/test_o261_item3_pep.py:99`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:307`, **anexo**) |
   | **(f) Ordem de trava** | **Incompleto.** Igualdade de `FOR UPDATE OF` e precedência ao C6 não provam ordem física entre P/D nem ausência de ciclo com chamado. Há gate que trava chamado antes de S143 e fechamento de disputa que alcança chamado. Deadlock concreto: **[não verificado]**. (`app/chamados/services/relavra_via.py:221`; `app/chamados/tests/test_o261_item3_pep.py:250`, **anexo**; `app/chamados/models.py:674`; `app/chamados/signals.py:42`; `app/chamados/signals.py:91`) |
   | **(g) Pouso antes do pré-requisito** | Cronologia de medição, RED/GREEN, suite e pouso: **[não verificado]**. A presença dos testes no anexo não comprova sua execução. (`app/chamados/tests/test_o261_item3_pep.py:250`, **anexo**) |

8. **PROCEDENTE-candidato — contraexemplo novo: `True` produz instante falso na trilha.**

   Considere o ramo sem credor com disputa aberta, pai/destinos fora de `VIVOS`, pergunta sem resposta nem carimbo e célula que permite ao C6 considerá-la morta. Esse estado alcança a tentativa de carimbo. (`app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1653`; `app/chamados/services/disputa_emissao.py:1656`; `app/chamados/services/disputa_emissao.py:1663`; `app/chamados/juizes.py:1195`)

   - B já leu a pergunta e prepara a fotografia com `materializada_em=None`. (`app/chamados/services/disputa_emissao.py:1666`)
   - Antes de B entrar na porta, A grava `admin_decidiu` em `tA`. A porta de identidade grava instante, autor e via. (`app/chamados/models.py:2639`; `app/chamados/models.py:2644`, cópia)
   - B tenta carimbar em `tB`. O v2 relê o carimbo de A, não o altera e devolve `True`, pois o C6 aceita a identidade existente. (`app/chamados/models.py:2665`, **v2 virtual**; `app/chamados/models.py:2602`, **v2 virtual**; `app/chamados/juizes.py:1171`)
   - B emite `fecho_sem_credor` com ator B e **`depois.materializada_em=tB`**, enquanto a pergunta conserva **`materializada_em=tA` e autor A**. (`app/chamados/services/disputa_emissao.py:1678`; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/models.py:2602`, **v2 virtual**)

   **O carimbo preservado está correto; o

[truncado em 20000 de 20265 caracteres]

## 06/11 03:34 · sessao 90e685ecd2b5 · - · - · origem=Ronald (tmux)

- **PROCEDENTE-candidato — descarte 5b exagera ao dizer que `atomic()` sempre aberto não muda comportamento.** Savepoint muda a contenção de erro de banco quando o chamador captura a exceção: sem ele, o atomic externo pode terminar em rollback; com ele, pode continuar e confirmar outras escritas. O repositório documenta exatamente essa classe em `app/core/observ.py:124` e `app/chamados/signals.py:83`. S144 chama o materializador e captura exceções sem savepoint próprio em `app/chamados/signals.py:139`; o materializador chama `carimbar` em `app/chamados/services/materializacao.py:1349`. Ocorrência concreta desse problema no v2: **[não verificado]**.

- **OBSERVAÇÃO — fotografia ainda inexistente não é restaurada.** No patch, `antes = lavra_antes = None` e os dois `if ... is not None` impedem desempacotar/restaurar fotografia inexistente. Falha de releitura, julgamento da linha já carimbada, relógio ou obtenção do autor acontece antes da escrita nova; o primeiro carimbo vem depois de `antes`. Referência: `app/chamados/models.py:2596` **v2 virtual**, bloco imediatamente posterior. A lavra é fotografada antes de sua alteração; a foto e restauração reais cobrem três campos em `app/chamados/veredito_lavrado.py:235` e `app/chamados/veredito_lavrado.py:240`.

- **OBSERVAÇÃO — restauração de memória não equivale a rollback do SQL.** Se `save()` efetivar o UPDATE e um hook levantar exceção Python depois, o `nullcontext` permite que um chamador capture a exceção e confirme o SQL, embora o `except` da porta restaure a instância. Não encontrei esse caminho ordinário comprovado nos receivers atuais: o único receiver de Pergunta registra callback em `app/chamados/signals.py:191`, e seu trabalho fica sob `engolir` em `app/chamados/signals.py:199`, cuja captura está em `app/core/observ.py:85`. Portanto, cenário possível sob falha/hook, mas ocorrência no código atual **[não verificado]**.

- **OBSERVAÇÃO — commit externo permanece fora do alcance.** Quando a porta herda transação, retorna antes do commit do chamador; uma falha posterior desse commit não atravessa seu `except`. A promessa de recuperar memória no commit vale para o atomic aberto pela própria porta, conforme a seleção do contexto no patch de `app/chamados/models.py:2598` **v2 virtual**; cobertura de commit externo: **[não verificado]**.

- **PROCEDENTE-candidato — mutante de lavra no commit sobrevive à cobertura apresentada.** Mutante: restaurar a lavra no `except` somente se `_cx.in_atomic_block`, mantendo a recuperação do carimbo incondicional. Os testes antigos de erro de lavra/save continuam passando porque herdam `TestCase` por `app/chamados/tests/test_o261e_f2_fila.py:143` e exercitam suas falhas em `app/chamados/tests/test_o261e_f2_fila.py:179`, `:190`, `:200`. O teste novo de commit chama a porta sem `lavrar_veredito=True` e verifica somente o carimbo; não denuncia lavra persistente na memória após commit falho com lavra solicitada: `app/chamados/tests/test_o261_item3_pep.py:307` **anexo**. Execução do mutante: **[não verificado]**.

- **OBSERVAÇÃO — testes anteriores simulam falha antes de SQL.** Os mocks de `PerguntaDisputa.save` lançam diretamente `RuntimeError`; não fazem o save real antes de lançar. Assim não cobrem a divergência “SQL confirmado / memória restaurada”: `app/chamados/tests/test_o261e_f2_fila.py:184`, `:211`. O v2 acrescenta cobertura útil do commit próprio e dos quatro campos humanos isolados, mas não comprova comportamento após save real seguido de exceção Python: `app/chamados/tests/test_o261_item3_pep.py:99`, `:307` **anexo**.

## 06/11 03:33 · sessao 41021277887b · - · - · origem=Ronald (tmux)

1. **OBSERVAÇÃO — 1c/7b: o descarte sustenta uma ressalva específica, não elimina o achado de frescor.** A célula do julgamento em curso existe como projeção explícita: `celula_deste_julgamento()` obtém ata/par do cartório e delega a `celula_como_o_julgamento_deixa()`; esta preserva a instância fora desse contexto e produz cópia quando há projeção. A ressuscitação usa expressamente essa função. (`app/chamados/juizes.py:383`; `app/chamados/juizes.py:394`; `app/ponto/portas/celula.py:709`; `app/chamados/models.py:1448`)

   **PROCEDENTE-candidato — preservar qualquer relação carregada não distingue projeção legítima de cache obsoleto.** O C6 lê veredito/via/ata/DNA da célula e também conteúdo de outras relações: destino/status e disputa/contexto. A justificativa sobre a célula não sustenta preservar indistintamente destino e disputa. (`app/chamados/juizes.py:1147`; `app/chamados/juizes.py:1181`; `app/chamados/juizes.py:893`; `app/chamados/juizes.py:910`; `app/chamados/juizes.py:743`; `app/chamados/juizes.py:361`)

   **OBSERVAÇÃO — o próprio E2 tem leitura comum do banco.** `perguntas_absolvidas_pela_celula()` carrega célula/disputa/destino via `select_related`; o cartório chama E2 depois da lavra da primeira passada. Portanto “cartório emite antes de lavrar” é verdadeiro para outros caminhos, mas não torna toda célula recebida por `carimbar` uma projeção mais nova que o banco. (`app/chamados/reconciliador.py:641`; `app/chamados/reconciliador.py:586`; `app/ponto/services/cartorio.py:948`; `app/ponto/services/cartorio.py:953`; `app/ponto/services/cartorio.py:959`; `app/ponto/services/cartorio.py:1802`)

   **OBSERVAÇÃO — reler e travar célula são decisões diferentes.** A porta da célula salva e emite outbox dentro de atomic; nessas operações não há demonstração de ordem concorrente com P/D. A afirmação “travar célula inverteria a ordem” continua **[não verificado]** quanto ao ciclo concreto. (`app/ponto/portas/celula.py:676`; `app/ponto/portas/celula.py:681`)

2. **OBSERVAÇÃO — 1d: ausência de chamador atual sustenta rebaixar a observação, mas o argumento geral é excessivo.** A busca por `.only()`/`.defer()` na produção de `chamados` e `ponto` não localizou projeção de PerguntaDisputa; os exemplos encontrados projetam outras entidades. (`app/chamados/rejuncao.py:78`; `app/chamados/services/juizes_do_dia.py:93`; `app/chamados/services/disputa_emissao.py:868`; carregamento parcial de PerguntaDisputa: **[não verificado]**)

   **OBSERVAÇÃO — “não carregou atributo, logo não decidiu sobre ele” não é implicação válida.** Uma decisão poderia decorrer de filtro SQL sobre campo omitido pela projeção; exemplo hipotético `filter(validada_em__isnull=True).only('pk')`: **[não verificado]**, nenhum chamador atual localizado. Os chamadores efetivamente conferidos carregam perguntas completas. (`app/chamados/services/disputa_emissao.py:1572`; `app/chamados/reconciliador.py:641`; `app/chamados/services/materializacao.py:1343`)

3. **OBSERVAÇÃO — classe (f): comparar a cláusula não fecha o grafo.** A referência relavra trava pergunta/disputa no mesmo SELECT, mas carrega célula fresca; o v2 mantém os mesmos alvos declarados, com relações preservadas. A igualdade de `FOR UPDATE OF` prova alvos, não a ordem física entre eles nem ausência de ciclo com chamado. (`app/chamados/services/relavra_via.py:217`; `app/chamados/services/relavra_via.py:221`; `app/chamados/services/relavra_via.py:227`; teste novo `OrdemDaTrava`, anexo)

   **OBSERVAÇÃO — há caminhos anteriores envolvendo chamado e disputa.** O gate do chamado trava CH antes de seu save/receiver S143; o fechamento de disputa salva D e seu receiver pode reconciliar CH; o novo carimbo acrescenta D à trava e antecipa P. Ocorrência concreta de deadlock e plano de aquisição: **[não verificado]**. (`app/chamados/models.py:674`; `app/chamados/signals.py:91`; `app/chamados/signals.py:92`; `app/chamados/models.py:1865`; `app/chamados/signals.py:41`; `app/chamados/signals.py:42`; `app/chamados/models.py::_reler_travada`, v2 anexo)

4. **OBSERVAÇÃO — classes restantes.**

   - **(a)/(d):** o novo ramo delega vida ao C6; comparação de fotografia humana não demonstra juiz paralelo. O risco identificado é insumo obsoleto para a autoridade existente. (`app/chamados/juizes.py:822`; `app/chamados/juizes.py:1135`; `app/chamados/models.py::carimbar`, v2 anexo)
   - **(b):** cópia dinâmica em memória não cria nova porta persistente; o selo declara não alcançar nome obtido por atributo, portanto não demonstra exclusividade dessa cópia. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113`; `app/chamados/models.py:2556`)
   - **(c):** dívida existente: `encerrar_fantasma` continua sem repassar `agora`, e há fallback de relógio. (`app/chamados/models.py:2685`; `app/docs/BACKLOG.md:1154`)
   - **(e):** os testes de trava e fotografia são discriminantes, mas não exercitam alteração de relação com mesma FK; o teste de identity map apenas conserva identidade. (`app/chamados/tests/test_o261_item3_pep.py`, anexo, `test_relacoes_carregadas_ficam_identity_map`; `test_MORDE_carimbar_trava_pergunta_e_disputa_como_a_relavra`)
   - **(g):** execução, cronologia “medido antes do verde” e pouso: **[não verificado]**; o patch contém os testes propostos, não evidência de sua execução. (`app/chamados/tests/test_o261_item3_pep.py`, anexo)

## 06/11 03:33 · sessao c3aa33033a39 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — `True` já carimbada não preserva contrato “gravou?”.** O docstring continua `-> bool (gravou?)`, mas v2 retorna `not pergunta_viva(self)` sem escrever. Isso é seguro para detectar estado encerrado; não prova autoria, instante ou causalidade do ato corrente. (`app/chamados/models.py:2563`, v2 virtual ramo `_RELIDA_JA_CARIMBADA` do anexo).

2. **Efeitos nos oito chamadores**, considerando outra via/autor/instante encontrado na releitura:

| Chamador | Efeito de `True` | Avaliação |
|---|---|---|
| `lavrar_sem_turno_aberto` | Pode fechar a disputa com resolução “sem turno aberto”; retorna `True`. (`app/chamados/reconciliador.py:405`, `:411`, `:412`, `:414`) | Fechar pelo juiz pode ser correto; atribuir a resolução atual à via sem turno não decorre do retorno, pois a pergunta foi encerrada por outro ato. O consumidor trata `True` como trabalho concluído. (`app/chamados/reconciliador.py:1598`) |
| `retratar_perguntas_absolvidas_por_celula` | Incrementa `n`; agenda disputa e pode fechá-la com resolução que atribui absolvição à célula. (`app/chamados/reconciliador.py:588`, `:590`, `:601`, `:603`) | Contar estado encerrado é defensável; contar retratação própria e atribuir causa à célula não é garantido pelo carimbo alheio. O número entra em `out['perguntas']`. (`app/chamados/reconciliador.py:1353`) |
| S143, ramo principal | Incrementa `perguntas_resolvidas`, inclui disputa para fechamento; não re-lavra porque a porta retorna antes. (`app/chamados/services/disputa_emissao.py:1603`, `:1606`, `:1607`, `:1696`, `:1698`; v2 virtual ramo já carimbada) | Resolve reentrada quando S143 pertence à mesma materialização; o retorno genérico também contabiliza ato de outra pessoa/processo. O comentário “carimbou ⇒ re-lavra” deixa de valer nessa chamada. (`app/chamados/services/disputa_emissao.py:1602`) |
| S143, sem credor | Emite `fecho_sem_credor`; não incrementa `n_pq` nesse ramo. (`app/chamados/services/disputa_emissao.py:1678`, `:1686`) | **Erro concreto de trilha:** `antes.materializada_em=None`, `ator=user`, `depois.materializada_em=_agora`, apesar de a linha preservar instante/autor alheios. (`app/chamados/services/disputa_emissao.py:1666`, `:1678`, `:1683`) |
| `_materializar_ausencia_de_resposta` | Não acrescenta recusa nem evento de recusa; reconcilia destino e pode fechar disputa. Ausência já foi criada. (`app/chamados/services/materializacao.py:520`, `:549`, `:550`, `:564`, `:568`) | Preservar outra decisão é correto; ela não integra automaticamente o grupo desta absorção, identificado pelo mesmo instante. Falta distinguir “já resolvida por outro ato” de “absorvida aqui”. (`app/chamados/services/materializacao.py:593`, `:608`, `:609`, `:614`) |
| `_materializar_uma` | Conserva o plantio/reuso feito antes da porta; marca `_materializou=True`; chamador incrementa materializadas. (`app/chamados/services/materializacao.py:1145`, `:1147`, `:1207`, `:1301`, `:1302`) | Corrige a reentrada própria. Para carimbo de ato independente, morte pelo C6 não prova que o plantio já realizado era autorizado pelo estado atualizado. A guarda humana é pulada nesse ramo do v2. (v2 virtual `_reler_travada`: materializada antes da comparação humana) |
| `materializar_pendentes_aprovadas` | Acrescenta PK em `feitas`; receiver ignora a lista. (`app/chamados/services/materializacao.py:1349`, `:1350`; `app/chamados/signals.py:140`) | A lista deixa de significar exatamente “foram carimbadas” por esta chamada. Sem evento extra nesse trecho; carimbo alheio preservado. (`app/chamados/services/materializacao.py:1317`) |
| `encerrar_fantasma` | Devolve `True`; consumidor grava evento fantasma, incrementa `encerradas`, inclui disputa para fechamento. (`app/chamados/models.py:2685`; `app/chamados/reconciliador.py:1136`, `:1138`, `:1145`, `:1146`) | **Trilha indevida:** contrato local exige uma trilha “só quando a porta escreveu”; evento diz “Encerrada no evento da lavra” e usa ator corrente mesmo quando o encerramento foi alheio. (`app/chamados/reconciliador.py:1107`, `:1108`, `:1139`, `:1144`) |

3. **PROCEDENTE-candidato — contraexemplo novo e direto de `True` errado/trilha falsa.** No ramo sem credor, Q foi lida sem carimbo; outro ato chama `carimbar_identidade(VIA_ADMIN_DECIDIU, A, quando=tA)`; Q sem resposta permanece morta pelo degrau de identidade; B chama `carimbar(..., quando=tB)` sobre sua instância velha e recebe `True` sem salvar. O chamador grava `fecho_sem_credor` com `depois.materializada_em=tB` e ator B, enquanto Q continua com instante tA/autor A. O erro de conteúdo do evento decorre dos argumentos explícitos, independentemente de plano de locks. (`app/chamados/models.py:2639`, `:2644`; `app/chamados/juizes.py:1171`; `app/chamados/services/disputa_emissao.py:1666`, `:1675`, `:1678`, `:1683`; v2 virtual ramo já carimbada). Execução concreta: **[não verificado]**.

4. **PROCEDENTE-candidato — os dois textos de `False` ainda têm limites.** “C6 viva ou humano mudou” cobre a nova comparação, mas não cobre `fresca is None`, que recusa sem executar C6 e sem demonstrar mudança humana. Além disso, “ela segue pendente” não decorre da recusa: se `validada_em` passou de `None` para instante, a comparação recusa, mas o C6 real já a considera morta e `.estado` retorna `VALIDADA`. (v2 virtual `_reler_travada`, ramo linha sumida/comparação humana; `app/chamados/juizes.py:1136`; `app/chamados/models.py:2499`). Permanecem também nome de ação `irma_viva_na_ausencia` e docstring que promete lista apenas de irmãs vivas. (`app/chamados/services/materializacao.py:510`, `:573`).

5. **OBSERVAÇÃO — testes do anexo não defendem esses consumidores.** Os testes `LinhaJaCarimbadaPorOutroAto` verificam retorno e preservação do banco diretamente; `ReentradaNaMesmaTransacao` cobre `_materializar_uma` com plantio e C6 mockados. Nenhum dos testes novos chama os ramos reais de evento `fecho_sem_credor` ou `encerrar_orfas_pela_lampada`; portanto não confrontam instante/autor da trilha com a linha preservada. (`app/chamados/tests/test_o261_item3_pep.py:114`, `:123`, `:151`, `:157`, `:173`, `:175`, anexo). Execução: **[não verificado]**.

## 06/11 03:23 · sessao 8f44185a9a9d · O261-ITEM3-PEP · verificador · origem=Code

1. **PROCEDENTE-candidato — não aprovaria a v1 como está.** A guarda nova pode recusar um carimbo legítimo produzido pela própria transação e provocar rollback do plantio; além disso, reler a pergunta não atualiza o conteúdo das relações preservadas. São problemas distintos da intercalação coberta pelo RED. (`app/chamados/services/materializacao.py:944`, `app/chamados/services/materializacao.py:1204`; `app/chamados/models.py:2653` **proposto**; `app/chamados/juizes.py:1181`)

   **OBSERVAÇÃO — referências e alcance.** Abaixo, “proposto” significa a numeração resultante da aplicação mental do patch; referências ao teste novo pertencem ao anexo. Execução dos testes, ocorrência dos contraexemplos em produção e planos de execução do banco: **[não verificado]**. [nao verificado]

   **OBSERVAÇÃO — a estrutura transacional atende à escolha declarada:** abre `atomic()` quando não há bloco ativo; dentro do bloco do chamador usa `nullcontext`, sem savepoint adicional. A trava permanece até terminar a transação correspondente. A escolha corresponde à primeira candidata registrada. (`app/chamados/models.py:2590` **proposto**; `app/docs/RELATO.md:131`)

   **OBSERVAÇÃO — “sem condenar o atomic externo” precisa da distinção entre exceções.** Uma exceção Python não implica, por si, rollback obrigatório; erro de banco no `save()` pode deixar a transação externa marcada para rollback. O próprio código documenta esse problema e usa savepoint no receiver S143 para contê-lo. (`app/chamados/models.py:2621` **proposto**; `app/chamados/signals.py:83`, `app/chamados/signals.py:91`)

   | Saída | Estado da instância do chamador |
   |---|---|
   | `True` | Recebe todos os campos concretos da releitura; depois recebe carimbo, complemento de resposta/validação e, opcionalmente, lavra. Dentro de transação externa, esse sucesso ainda está sujeito ao rollback dela. (`app/chamados/models.py:2653` **proposto**, `app/chamados/models.py:2598` **proposto**, `app/chamados/models.py:2619` **proposto**) |
   | `False`, linha já carimbada ou humano mudou | Campos concretos ficam como a linha relida; `_pv_memo` é removido; relações com a mesma FK permanecem carregadas. (`app/chamados/models.py:2650` **proposto**, `app/chamados/models.py:2653` **proposto**, `app/chamados/models.py:2655` **proposto**) |
   | `False`, linha sumiu | Remove `_pv_memo`, mas conserva os campos antigos: não existe linha fresca para copiar. Portanto a promessa de “toda recusa” devolver o estado do banco tem essa exceção. (`app/chamados/models.py:2578` **proposto**, `app/chamados/models.py:2651` **proposto**) |
   | `False`, C6 viva | Desfaz os seis campos fotografados sobre a base relida; remove novamente o memo. (`app/chamados/models.py:2595` **proposto**, `app/chamados/models.py:2612` **proposto**; `app/chamados/models.py:2618` da cópia) |
   | Exceção no C6 | Restaura aqueles seis campos e propaga. (`app/chamados/models.py:2607` **proposto**) |
   | Exceção na lavra ou no `save()` | Restaura lavra e os seis campos, propagando a exceção. (`app/chamados/models.py:2622` **proposto**; `app/chamados/veredito_lavrado.py:240`) |

   **PROCEDENTE-candidato — o encerramento do atomic próprio ficou fora da recuperação de memória.** Se o `save()` terminar, mas a saída do `atomic()` falhar antes de confirmar a gravação, o tratamento interno já terminou: a instância conserva carimbo/lavra sem confirmação no banco. Os testes existentes provocam falha na lavra e no `save`, não no encerramento da transação. (`app/chamados/models.py:2590` **proposto**, `app/chamados/models.py:2626` **proposto**; `app/chamados/tests/test_o261e_f2_fila.py:179`, `app/chamados/tests/test_o261e_f2_fila.py:200`; ocorrência: **[não verificado]**)

   **PROCEDENTE-candidato — frescor das relações não está garantido.** A consulta fresca carrega `disputa`, mas a cópia transfere somente campos concretos de **PerguntaDisputa**. Mantida a FK, conserva-se a disputa antiga, sua instância de colaborador, a célula antiga e o chamado-destino antigo. O C6 pode, portanto, executar novamente sobre `veredito`, `veredito_via`, ata/DNA ou status antigo. Preservar a identidade do colaborador é requisito válido; preservar conteúdo obsoleto das relações é outro efeito. (`app/chamados/models.py:2648` **proposto**, `app/chamados/models.py:2653` **proposto**; `app/chamados/juizes.py:1006`, `app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:893`, `app/chamados/juizes.py:743`)

   **OBSERVAÇÃO — campos deferidos enfraquecem a fotografia recebida.** `_recebido` usa `getattr` antes da consulta travada: um campo humano deferido será lido naquele momento, não representa necessariamente a leitura que fundamentou a decisão do chamador. Depois, o laço carrega todos os campos concretos. Os testes novos usam instâncias completas e não defendem essa situação. (`app/chamados/models.py:2647` **proposto**, `app/chamados/models.py:2653` **proposto**; `app/chamados/tests/test_o261_item3_pep.py:35`, anexo; reprodução com deferimento: **[não verificado]**)

   **OBSERVAÇÃO — retirar `_pv_memo` é necessário, mas insuficiente.** A chave cobre campos da pergunta, veredito/via da célula e status do destino quando carregado; não cobre ata, DNA, batidas nem todos os demais insumos do julgamento. O `pop` força nova execução, mas não renova as relações preservadas. (`app/chamados/juizes.py:811`, `app/chamados/juizes.py:835`, `app/chamados/juizes.py:893`, `app/chamados/juizes.py:910`; `app/chamados/models.py:2650` **proposto**)

2. **OBSERVAÇÃO — o mapa de donos não equivale a um único método escrevendo todos os campos.** DOSSIES separa identidade/carimbo, resposta, materialização, reconciliação, lavra e validação. JUIZES aponta `validar_pergunta` como escritor de validação, mas o código e o selo reconhecem as exceções existentes. (`app/docs/DOSSIES.md:42`, `app/docs/DOSSIES.md:46`; `app/docs/JUIZES.md:38`; `app/chamados/tests/test_contract_dono_validada.py:26`)

   | Campo | Quem escreve | Quem lê relevantemente |
   |---|---|---|
   | `materializada_em` | Atribuição comum em `_escrever_carimbo`; persistência por `carimbar`, identidade, descarimbo e lote. (`app/chamados/models.py:2558`, `app/chamados/models.py:2609`, `app/chamados/models.py:2644`, `app/chamados/models.py:2652`, `app/chamados/models.py:2663`) | C6, estado da pergunta, reabertura por resposta e marco de envelhecimento. (`app/chamados/juizes.py:1177`, `app/chamados/models.py:2499`, `app/chamados/models.py:2711`, `app/chamados/veredito_lavrado.py:162`) |
   | `materializada_por` | Mesmas portas do carimbo, pela atribuição comum do ID. (`app/chamados/models.py:2559`, `app/chamados/models.py:2644`, `app/chamados/models.py:2663`) | Fotografias/restauração e trilha do encerramento. (`app/chamados/models.py:2583`, `app/chamados/models.py:2618`, `app/chamados/reconciliador.py:1134`) |
   | `via_resolucao` | Portas do carimbo; também troca isolada por fato no espelho e relavra. (`app/chamados/models.py:2560`, `app/chamados/models.py:2673`, `app/chamados/services/relavra_via.py:123`) | C6, juiz do lastro, reabertura e seleção da relavra. (`app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:658`, `app/chamados/models.py:2711`, `app/chamados/services/relavra_via.py:116`) |
   | `resposta_colab` | Core da resposta, reabertura e resposta automática. (`app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1426`) | Validação, C6, derivação do plantio e guarda humana proposta. (`app/chamados/services/validacao.py:374`, `app/chamados/juizes.py:1150`, `app/chamados/services/materializacao.py:356`; `app/chamados/models.py:2647` **proposto**) |
   | `respondida_em` | Core, reabertura, resposta automática/complemento; `carimbar` preenche se houver texto; ressuscitação pode limpar. (`app/chamados/services/disputa_emissao.py:1283`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1426`, `app/chamados/services/disputa_emissao.py:1443`, `app/chamados/models.py:2588`, `app/chamados/models.py:1504`) | Validação, C6, estado exibido e marco de envelhecimento. (`app/chamados/services/validacao.py:363`, `app/chamados/juizes.py:1150`, `app/chamados/models.py:2503`, `app/chamados/veredito_lavrado.py:166`) |
   | `validada_em` | `_carimbar_validacao`; devolução; resposta nova condicionalmente limpa; reabertura/ressuscitação; complemento de `carimbar`. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1504`, `app/chamados/models.py:2592`) | Primeiro degrau do C6, pré-condição da validação, estado e decisão de limpar a assinatura na resposta nova. (`app/chamados/juizes.py:1136`, `app/chamados/services/validacao.py:366`, `app/chamados/models.py:2499`, `app/chamados/services/disputa_emissao.py:1286`) |
   | `validada_por` | Acompanha a validação nas portas anteriores. (`app/chamados/services/validacao.py:132`, `app/chamados/services/validacao.py:189`, `app/chamados/services/disputa_emissao.py:1296`, `app/chamados/models.py:2593`) | Foto da validação, trilha da assinatura desfeita e comparação humana proposta. (`app/chamados/services/validacao.py:139`, `app/chamados/services/disputa_emissao.py:1312`; `app/chamados/models.py:2630` **proposto**) |
   | `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar`, restauração em memória e gravação em lote; o backfill também atribui. Chamadores persistem a lavra com seus atos. (`app/chamados/veredito_lavrado.py:227`, `app/chamados/veredito_lavrado.py:240`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `app/chamados/models.py:2471`, `app/chamados/services/fio.py:845`, `app/chamados/services/relavra_via.py:125`) | Verificação de stale, crédito de tela, auditoria e fotografia para recuperação. (`app/chamados/veredito_lavrado.py:179`, `app/chamados/services/raiox.py:182`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1170`, `app/chamados/veredito_lavrado.py:235`) |
   | `_pv_memo` | `pergunta_viva` grava; desfazimento/descarimbo e a releitura proposta invalidam. (`app/chamados/juizes.py:847`, `app/chamados/models.py:2623`, `app/chamados/models.py:2649`; `app/chamados/models.py:2650` **proposto**) | A própria `pergunta_viva`, comparando `_chave_memo_pv`. (`app/chamados/juizes.py:835`) |

   **PROCEDENTE-candidato — a corrida na ordem inversa continua possível.** O respondente pode ler `validada_em=None`; `carimbar(resposta='validar')` então grava a validação; depois o respondente salva resposta nova usando sua instância antiga. Como o core só acrescenta a limpeza da validação quando a instância recebida já a tinha, esse UPDATE pode preservar a assinatura sobre R2. A trava serializa os UPDATEs, mas não refaz a decisão anterior do escritor da resposta. Isso já era possível antes; limita a proteção anunciada pela cura. (`app/chamados/views.py:1239`, `app/chamados/views.py:1283`; `app/chamados/services/disputa_emissao.py:1284`, `app/chamados/services/disputa_emissao.py:1286`, `app/chamados/services/disputa_emissao.py:1297`, `app/chamados/services/disputa_emissao.py:1301`; ocorrência: **[não verificado]**)

   **OBSERVAÇÃO — existe também uma janela anterior à nova porta:** `validar_pergunta` lê e julga sem trava, entrando no atomic somente para gravar a validação. Uma resposta pode mudar entre esses pontos; a releitura posterior já encontra a resposta nova assinada. Identidade, relavra e lavra preguicosa também não passam pela nova comparação humana. (`app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:374`, `app/chamados/services/validacao.py:395`; `app/chamados/models.py:2644`; `app/chamados/services/relavra_via.py:115`; `app/chamados/management/commands/lavrar_perguntas_stale.py:85`)

3. **OBSERVAÇÃO — os oito chamadores diretos foram encontrados.** Todos passam a poder receber `False` por linha já carimbada, mudança de campo humano ou C6 que reprove os dados concretos relidos, mesmo quando o julgamento sobre a instância antiga permitiria `True`. (`app/chamados/models.py:2591` **proposto**, `app/chamados/models.py:2608` **proposto**, `app/chamados/models.py:2655` **proposto**)

   | Chamador | Tratamento do novo `False` e veracidade |
   |---|---|
   | `lavrar_sem_turno_aberto` | Retorna `False`; não chega ao fechamento da disputa. Não emite texto falso nesse ramo. (`app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:412`) |
   | `retratar_perguntas_absolvidas_por_celula` | Continua; não incrementa contador nem agenda aquela disputa para fechamento. (`app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:588`) |
   | Resolver perguntas do chamado, ramo principal | Continua; não incrementa perguntas resolvidas nem adiciona a disputa por esse sucesso. A cauda do serviço continua executando. (`app/chamados/services/disputa_emissao.py:1603`, `app/chamados/services/disputa_emissao.py:1606`, `app/chamados/services/disputa_emissao.py:1620`) |
   | Mesmo serviço, ramo sem credor | Continua sem emitir `fecho_sem_credor`; não há evento de sucesso indevido nesse `False`. (`app/chamados/services/disputa_emissao.py:1675`, `app/chamados/services/disputa_emissao.py:1678`) |
   | `_materializar_ausencia_de_resposta` | Conta/trilha a recusa e segue para reconciliar destino e possivelmente fechar disputa. (`app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:564`, `app/chamados/services/materializacao.py:568`) |
   | `_materializar_uma` | Levanta `_Reprovada`, desfaz o atomic, acrescenta erro e grava recusa. É o ponto da regressão por reentrada descrita no item 7. (`app/chamados/services/materializacao.py:1202`, `app/chamados/services/materializacao.py:1204`, `app/chamados/services/materializacao.py:1222`) |
   | `materializar_pendentes_aprovadas` | Não adiciona o PK aos materializados; sem texto próprio de recusa. (`app/chamados/services/materializacao.py:1349`) |
   | `encerrar_fantasma` | Devolve o booleano; o consumidor continua sem evento nem incremento de encerradas. (`app/chamados/models.py:2685`, `app/chamados/reconciliador.py:1136`, `app/chamados/reconciliador.py:1145`) |

   **PROCEDENTE-candidato — dois textos deixam de dizer a verdade.** “C6 manteve … VIVA”/“segue pendente” e “C6 … VIVA com o carimbo” passam a ser emitidos também quando o C6 nem foi executado, inclusive para linha já carimbada. (`app/chamados/services/materializacao.py:552`, `app/chamados/services/materializacao.py:1205`; `app/chamados/models.py:2591` **proposto**)

4. **OBSERVAÇÃO — a ordem precisa considerar a transação do chamador.** A nova porta trava pergunta e disputa; quando abre seu próprio atomic, essas travas terminam antes da cauda do chamador. Quando herda atomic, permanecem durante a cauda. A relavra de referência usa os mesmos alvos, mas uma instância nova, incluindo célula nova. (`app/chamados/models.py:2590` **proposto**, `app/chamados/models.py:2648` **proposto**; `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`)

   Na tabela, **P** = pergunta, **D** = disputa e **CH** = chamado; são as travas explícitas/UPDATEs identificados nos caminhos citados. Efeitos adicionais e ordem efetiva do plano do banco: **[não verificado]**.

   | Chamador | Antes de `carimbar`, se ainda na mesma transação | Depois, se ainda na mesma transação |
   |---|---|---|
   | `reconciliador.py:405` | O método não abre atomic nem trava explicitamente nesse trecho; pode herdar transação do chamador. (`app/chamados/reconciliador.py:340`, `app/chamados/reconciliador.py:405`) | UPDATE de D pelo fechamento; signal pode travar/atualizar CH pai. Sem atomic externo, isso ocorre após terminar o novo atomic da porta. (`app/chamados/reconciliador.py:412`, `app/chamados/models.py:1865`, `app/chamados/signals.py:42`) |
   | `reconciliador.py:586` | Sem atomic próprio; pode vir do cartório. (`app/chamados/reconciliador.py:579`, `app/ponto/services/cartorio.py:958`) | Outras perguntas do laço; fechamento de D somente depois do laço, seguido do CH pai. (`app/chamados/reconciliador.py:582`, `app/chamados/reconciliador.py:602`, `app/chamados/signals.py:42`) |
   | `disputa_emissao.py:1603` | Via S143, CH já foi travado pelo gate e atualizado; receiver abre atomic. (`app/chamados/models.py:674`, `app/chamados/models.py:279`, `app/chamados/signals.py:91`) | Outras P; possível reabertura do pai; fechamento de D e reconciliação do pai. (`app/chamados/services/disputa_emissao.py:1628`, `app/chamados/services/disputa_emissao.py:1675`, `app/chamados/services/disputa_emissao.py:1696`, `app/chamados/signals.py:42`) |
   | `disputa_emissao.py:1675` | Mesmo contexto; podem estar retidas travas das perguntas anteriores e da primeira chamada. (`app/chamados/services/disputa_emissao.py:1603`, `app/chamados/services/disputa_emissao.py:1661`) | Outras P e fechamento de D/CH pai. (`app/chamados/services/disputa_emissao.py:1661`, `app/chamados/services/disputa_emissao.py:1696`, `app/chamados/signals.py:42`) |
   | `materializacao.py:549` | Resposta escape já pode ter atualizado sua P; criação/reabertura de Ausência vem antes. Só permanecem retidos se houver transação externa abrangente. (`app/chamados/services/disputa_emissao.py:1301`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:417`) | Reconciliação trava CH; fechamento atualiza D e alcança CH pai. (`app/chamados/services/materializacao.py:564`, `app/chamados/services/materializacao.py:568`, `app/chamados/models.py:674`, `app/chamados/signals.py:42`) |
   | `materializacao.py:1202` | Atomic obrigatório; escrita/anotação de Batida; reconciliação de destino/clones e seus signals. Via validação, também UPDATE prévio de P. (`app/chamados/services/materializacao.py:1145`, `app/chamados/services/materializacao.py:897`, `app/chamados/services/materializacao.py:918`, `app/chamados/services/materializacao.py:944`, `app/chamados/services/validacao.py:396`) | Termina o atomic da unidade; fechamento de D pelo laço vem depois, salvo atomic externo. (`app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1303`) |
   | `materializacao.py:1349` | Pelo receiver de fechamento, D já sofreu UPDATE; receiver anterior pode ter reconciliado CH pai. (`app/chamados/models.py:1865`, `app/chamados/signals.py:42`, `app/chamados/signals.py:140`) | Outras perguntas do laço; não há fechamento próprio posterior nesse método. (`app/chamados/services/materializacao.py:1349`) |
   | `models.py:2685` | Consumidor abre atomic imediatamente antes; o trecho anterior coleta/julga sem trava explícita. (`app/chamados/reconciliador.py:1123`, `app/chamados/reconciliador.py:1135`) | INSERT da trilha; D fecha depois do atomic local, salvo transação externa. (`app/chamados/reconciliador.py:1138`, `app/chamados/reconciliador.py:1160`) |

   **OBSERVAÇÃO — há ordens opostas que já existiam.** CH→P ocorre no fechamento do chamado seguido do S143; P→D→CH ocorre quando o carimbo e o fechamento da disputa compartilham transação externa. D→CH→P também aparece nos receivers de fechamento da disputa. O `save()` antigo de `carimbar` já fazia UPDATE de P; não se pode atribuir ao diff a origem de todas essas inversões. (`app/chamados/models.py:674`, `app/chamados/models.py:2609`, `app/chamados/models.py:1865`; `app/chamados/signals.py:42`, `app/chamados/signals.py:92`, `app/chamados/signals.py:140`)

   **OBSERVAÇÃO — o diff amplia a superfície de bloqueio:** acrescenta D à trava explícita e antecipa P para antes do C6, inclusive em recusas que antes não salvavam P. Comparar `FOR UPDATE OF` demonstra alvos e posição da consulta, não uma ordem temporal comprovada de aquisição entre P e D nem ausência de ciclos com CH. (`app/chamados/models.py:2648` **proposto**; `app/

[truncado em 20000 de 29410 caracteres]

## 06/11 03:19 · sessao 05bd765022f0 · - · - · origem=Ronald (tmux)

Leis: origem e testemunha em `CLAUDE.md:17` e `CLAUDE.md:20`; escritor único em `CLAUDE.md:32`; contrato 2/idempotência em `CLAUDE.md:175`; proteção do sítio em `CLAUDE.md:187`. A metade-trava e as duas candidatas constam de `app/docs/BACKLOG.md:1154` e `app/docs/RELATO.md:131`.

Tabela conferida contra as 238 linhas do anexo `app/chamados/tests/test_o261_item3_pep.py`:

| Linha | Teste abreviado | Falha sem cura? | [nao verificado]
|---:|---|---|
| 52 | resposta nova após validação | Sim: original carimba e mantém validação antiga |
| 74 | resposta chega sem validação anterior | Sim: C6 mockado permite gravação |
| 85 | outro validador | Sim: original não percebe alteração de autor |
| 102 | admin decidiu no meio | Sim: original sobrescreve carimbo |
| 114 | repetir não renova instante | Sim: original reescreve instante/via |
| 128 | dado de máquina mudou | Sim: juiz recebe `data_evento` antigo |
| 143 | memo antigo | Sim: memo forjado responde “viva” |
| 160 | sem intercalação | Não: controle de compatibilidade |
| 169 | identity map | Não: original já preserva relações |
| 186 | trava pergunta e disputa | Sim: SQL original não contém trava |
| 199 | trava antes do juiz | Sim: juiz encontra zero SELECTs com trava |
| 209 | extrator rejeita SQL sem trava | Não: testa auxiliar |
| 217 | fora de atomic | Sim: original grava, mas não emite SELECT com trava |

**PROCEDENTE-candidato — cobertura incompleta da guarda humana.** Retirar somente `respondida_em`, somente `validada_em`, ou ambos de `_CAMPOS_DO_HUMANO` deixa os novos testes passarem: os casos das linhas 52 e 74 também mudam `resposta_colab`; o caso da linha 85 muda apenas `validada_por_id`. Nenhum novo caso isola mudança em `respondida_em` ou `validada_em`. (`app/chamados/tests/test_o261_item3_pep.py:52`, `:74`, `:85`, anexo.)

**OBSERVAÇÃO — sem-savepoint não foi defendido.** Um mutante que sempre usa `atomic()` também passa os testes novos: nenhum verifica SAVEPOINT dentro do atomic do chamador, nem captura uma exceção Python e continua a transação externa. (`app/chamados/tests/test_o261_item3_pep.py:186`, `:199`, `:217`, anexo.)

**PROCEDENTE-candidato — relações antigas sobrevivem à releitura.** Limpar `_pv_memo` obriga recalcular, mas não atualiza `celula`, `disputa` e `chamado_destino` quando a FK permanece igual. O C6 lê `celula.veredito`, `veredito_via`, DNA e ata desses objetos; pode recalcular com conteúdo antigo. O teste de identity map não muda conteúdo relacionado. (`app/chamados/juizes.py:1147`, `:1177`, `:1181`, `:1195`, `:889`, `:910`; `app/chamados/tests/test_o261_item3_pep.py:169`, anexo.)

**OBSERVAÇÃO — o selo não vê a cópia dinâmica.** O novo `setattr(self, _f.attname, ...)` também atribui os campos do carimbo fora de `_escrever_carimbo`, mas copia estado do banco; não constitui sozinho outro escritor persistente. O selo declara não alcançar nome vindo de atributo e seu tratamento de `setattr` depende de reconhecer o nome do campo. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`, `:179`; método `_reler_travada`, diff anexo.)

**PROCEDENTE-candidato — falha ao sair do atomic próprio.** O tratamento de exceção envolve C6/lavra/save, mas o `__exit__` do atomic próprio fica fora desses tratamentos. Se o COMMIT falhar depois do `save`, a memória conserva o carimbo e a lavra sem gravação confirmada; ocorrência em produção **[não verificado]**. Os testes existentes provocam erro no `save` e na lavra, não no encerramento da transação. (`app/chamados/tests/test_o261e_f2_fila.py:179`, `:190`, `:200`; bloco `with` de `carimbar`, diff anexo.)

Classes: não nasce outro C6; permanece o chamado à autoridade existente, cuja separação entre memo e cálculo está em `app/chamados/juizes.py:822` e `:1135`. Não há outro escritor persistente demonstrado pelo método novo; há a limitação de selo acima. O relógio implícito já existia em `app/chamados/models.py:2581`; sua dívida está expressa em `app/docs/BACKLOG.md:1154`. O teste SQL prova alvos e momento do SELECT, mas não prova ausência de ciclo com travas anteriores/posteriores dos chamadores (`app/chamados/tests/test_o261_item3_pep.py:186`, `:199`, anexo). Pouso ou execução dos testes nesta verificação: **[não verificado]**.

## 06/11 03:18 · sessao 7ffc81c85454 · - · - · origem=Ronald (tmux)

**Achados para consolidar**

- **PROCEDENTE-candidato — recusa de reentrada legítima.** `_materializar_uma` abre atomic, planta/reusa batida e reconcilia destino antes de chamar `carimbar` (`app/chamados/services/materializacao.py:1145`, `:897`, `:918`, `:944`, `:1202`). A reconciliação salva CH e dispara S143, que pode carimbar a mesma pergunta enquanto a instância externa permanece velha (`app/chamados/models.py:279`; `app/chamados/signals.py:91`; `app/chamados/services/disputa_emissao.py:1603`). Com o diff, a chamada externa recebe False por já carimbada e levanta `_Reprovada`, desfazendo batida e primeiro carimbo (`app/chamados/services/materializacao.py:1204`, `:1208`; anexo diff, `_reler_travada`). **Execução concreta não verificada.** Construção: D aberta, P validada, última batida correta no slot tornando premissa do CH regularizada; S143 não filtra validada nula (`app/chamados/services/disputa_emissao.py:1572`), C6 mata validada (`app/chamados/juizes.py:1136`), guard de resposta admite lastro existente (`app/chamados/services/disputa_emissao.py:1596`; `app/chamados/juizes.py:718`), e gate admite premissa regularizada (`app/chamados/models.py:701`). A própria P validada não retém fechamento (`app/chamados/juizes.py:1747`).

- **PROCEDENTE-candidato — mensagens falsas.** Os novos False podem ocorrer sem executar C6 ou com P já resolvida; textos continuam afirmando “C6 manteve ... VIVA”, “segue pendente” e “C6 ... VIVA” (`app/chamados/services/materializacao.py:552`, `:1205`; anexo diff, guardas de `_reler_travada`).

**Oito chamadores e travas**

| Chamada | Antes na mesma transação | Depois na mesma transação / False |
|---|---|---|
| `reconciliador.py:405` | Método sem atomic próprio; guarda só lê (`:340`, `:362`). Travas herdadas dependem da entrada. | False retorna False (`:406`); True pode fechar D (`:412`) e CH pai pelo signal (`signals.py:41`). Sem atomic externo, a nova transação da porta termina antes disso. |
| `reconciliador.py:586` | Método sem atomic próprio (`:579`); cartório também o chama (`ponto/services/cartorio.py:958`). | False continua (`:587`), não conta nem agenda D. D fecha somente depois do laço (`:602`), fora do atomic novo da porta salvo externo. |
| `disputa_emissao.py:1603` | Método sem atomic próprio (`:1550`). Via S143: CH já travado pelo gate e atualizado (`models.py:638`, `:674`, `:279`; `signals.py:91`). Via `_retratar`: atomic e possível UPDATE de justificativa (`reconciliador.py:522`, `:535`). | False continua (`:1605`); depois pode reabrir pai (`:1628`), carimbar outras P e fechar D (`:1696`), cujo signal trava CH pai. |
| `disputa_emissao.py:1675` | Mesmo contexto; pode já deter travas das P anteriores do laço e da primeira chamada (`:1603`, `:1661`). | False continua, sem evento (`:1676`). Fechamento D depois (`:1696`). |
| `materializacao.py:549` | Sem atomic próprio. Resposta escape já salvou P (`disputa_emissao.py:1301`); criação/reabertura de Ausência vem antes (`materializacao.py:520`; `ponto/services/ausencia.py:291`, `:417`). Esses locks persistem somente se houver transação externa. | False conta e trilha (`:550`), mas segue reconciliando CH (`:564`) e pode fechar D (`:568`). Texto falso em `:552`. |
| `materializacao.py:1202` | Atomic obrigatório (`:1145`); Batida escrita/anotada, sinais, reconciliação de destino/clones (`:897`, `:918`, `:944`, `:1029`, `:1049`). Na validação humana, UPDATE P já retido (`validacao.py:395`, `:396`; escritor `:133`). | False levanta e rollback (`:1204`), registra erro e recusa (`:1208`, `:1222`). Fecho D do laço ocorre depois do atomic interno (`:1303`), salvo atomic externo. |
| `materializacao.py:1349` | Sem atomic local (`:1315`). Pelo receiver, D já sofreu UPDATE (`models.py:1865`; `signals.py:140`) e primeiro receiver pode ter reconciliado CH pai (`signals.py:42`). | False não adiciona pk (`:1349`); sem texto próprio. Fora de atomic, porta abre o seu; por `D.fechar`, herda transação. |
| `models.py:2685` | Wrapper `encerrar_fantasma`; entrada produtiva abre atomic explícito antes (`reconciliador.py:1135`). Nenhuma trava explícita no trecho anterior (`:1123`). | False continua (`:1136`); True insere evento (`:1138`). Fecho D ocorre após esse atomic (`:1160`), salvo externo. |

**Ordem oposta**

- **OBSERVAÇÃO — CH→P preexistia.** Gate trava CH, salva e S143 atualiza P (`app/chamados/models.py:674`, `:279`; `app/chamados/signals.py:91`; `app/chamados/services/disputa_emissao.py:1603`; save antigo `app/chamados/models.py:2609`).
- **OBSERVAÇÃO — P→D→CH também preexistia quando há atomic externo.** Carimbo já atualizava P, depois `D.fechar` atualiza D e signal reconcilia CH (`app/chamados/models.py:2609`, `:1865`; `app/chamados/signals.py:41`).
- **OBSERVAÇÃO — diff acrescenta lock explícito de D e antecipa lock de P para antes do C6, incluindo recusas.** Igualdade de `FOR UPDATE OF` não comprova ordem temporal de aquisição das duas linhas ou ausência de deadlock; teste só compara cláusulas (anexo diff, `OrdemDaTrava`; referência da relavra `app/chamados/services/relavra_via.py:221`). Plano e execução concorrente: **[não verificado]**.

## 06/11 03:17 · sessao 5a58e2b84bcc · - · - · origem=Ronald (tmux)

Escritores/leitores e alcance da trava, todos conferidos somente na cópia:

- **OBSERVAÇÃO — mapa de donos:** DOSSIES divide PerguntaDisputa em Q1 identidade/carimbo, Q2 resposta, Q3 materialização, Q4 reconciliação, Q5 veredito e Q6 validação; admite várias portas por dono de regra. Referências: `app/docs/DOSSIES.md:42`, `app/docs/DOSSIES.md:46`. JUIZES chama `validar_pergunta` de escritor único de `validada_em`, mas o código e o selo reconhecem exceções: `app/docs/JUIZES.md:38`, `app/chamados/tests/test_contract_dono_validada.py:26`, `app/chamados/tests/test_contract_dono_validada.py:30`.

| Campo | Escritores relevantes | Leitores relevantes |
|---|---|---|
| `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição comum `_escrever_carimbo`: `models.py:2556-2560`; portas `carimbar`: `2586`, `carimbar_identidade`: `2636-2644`, `descarimbar`: `2648-2653`; lote: `2663-2664`. Via isolada: `encerrar_por_fato_no_espelho` `2673-2674`; re-lavra: `services/relavra_via.py:123-126`. | C6: `juizes.py:1145`, `1171`, `1177`, `1207`; status: `models.py:2499-2506`; resposta pode reabrir: `models.py:2711-2713`; agregador de stale: `veredito_lavrado.py:162-165`. |
| `resposta_colab`, `respondida_em` | Resposta humana: `services/disputa_emissao.py:1282-1301`; reabertura: `1393-1413`; automática: `1426-1430`; complemento pela trilha: `1443-1446`; carimbar com `resposta`: `models.py:2587-2590`; ressuscitar limpa instante: `models.py:1504`; desfazer somente memória: `models.py:2620`. | Validação: `services/validacao.py:363`, `374`, `382`; C6: `juizes.py:1150-1162`; último marco: `veredito_lavrado.py:166-169`; respostas exibidas: `models.py:2818`, `2823-2845`. |
| `validada_em`, `validada_por` | Validar: `services/validacao.py:127-133`; devolver validação: `187-190`; nova resposta remove condicionalmente: `services/disputa_emissao.py:1285-1297`; reabertura: `1393-1413`; carimbar: `models.py:2591-2594`; ressuscitar: `models.py:1504`; desfazer em memória: `models.py:2621-2622`. | C6 imediatamente mata: `juizes.py:1136-1137`; validar verifica pré-condição: `services/validacao.py:363-368`; fila/status: `models.py:2499-2501`; SQL de vivas: `models.py:2757`; resposta decide limpar lendo instante: `services/disputa_emissao.py:1285-1297`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | Porta `lavrar`: `veredito_lavrado.py:211-231`; restore memória: `240-245`; lote: `197-207`; backfill atribui: `management/commands/backfill_veredito_lavrado.py:138-140`. Persistências da porta também por carimbar `models.py:2606-2609`, nascimento/reapontamento `models.py:2471-2474`, costura `services/fio.py:845-848`, re-lavra `services/relavra_via.py:123-126`, destino `models.py:291-295`, cron `management/commands/lavrar_perguntas_stale.py:85-87`. | Stale: `veredito_lavrado.py:179-190`; crédito de tela: `services/raiox.py:182-185`; comparação auditor: `management/commands/auditar_invariantes_chamados.py:1170`; foto restore: `veredito_lavrado.py:235-237`. |
| `_pv_memo` | C6 grava: `juizes.py:835-847`; invalidações existentes: `models.py:2623`, `2649`; diff invalida também na releitura, conforme anexo. | C6 lê/cache hit: `juizes.py:835-838`; chave: `juizes.py:811-819`. |

Os caminhos acima usam prefixo `app/chamados/`; as referências em intervalos são para consolidação, podendo citar uma linha âncora individualmente no relatório final.

**PROCEDENTE-candidato — a corrida inversa sobrevive.** A porta web lê a pergunta sem trava em `app/chamados/views.py:1239`, julga a instância em `:1242`, e depois passa a mesma instância ao core em `:1283`; a API faz o mesmo em `app/api/views.py:2628` e `:2643`. O core decide se deve zerar validação pela instância recebida, apenas quando `p.validada_em is not None`, em `app/chamados/services/disputa_emissao.py:1285`, `:1286`, `:1295`, `:1297`, e salva em `:1301`. Portanto:

1. Respondente lê R1, `validada_em=None`.
2. `carimbar(resposta='validar')` obtém a trava nova, aplica validação e salva.
3. O respondente, com a instância anterior, grava R2. Seu `update_fields` contém apenas resposta/instante (`disputa_emissao.py:1284`), preservando a validação aplicada enquanto ele esperava. [nao verificado]
4. R2 fica validada por quem não a viu. O C6 a mata imediatamente (`juizes.py:1136`).

É contraexemplo lógico do código; ocorrência real ou intercalamento real em produção: **[não verificado]**. O novo RED testa resposta anterior à releitura; esta ordem é distinta e permanece fora da trava da decisão do escritor.

**OBSERVAÇÃO — outra janela já existente fica antes da nova porta.** `validar_pergunta` busca R1 sem trava em `app/chamados/services/validacao.py:359`, verifica resposta/validação em `:363` e `:366`, julga em `:374`, só depois entra no atomic em `:395` e grava validação em `:396` pela função `:131-133`. Se R2 chegar entre leitura e escrita, a própria validação já é sobre texto não visto. O materializador poderá ler R2 já validada; sua nova guarda não tem a foto original de R1. O diff não cria essa janela nem a fecha.

**OBSERVAÇÃO — outros escritores não entram na trava nova.** Identidade e descarimbo continuam sem releitura própria (`app/chamados/models.py:2636`, `:2644`, `:2648`, `:2653`); re-lavra atribui via de instância recebida e salva sem trava própria (`app/chamados/services/relavra_via.py:115`, `:123`, `:125`); lavra cron calcula e salva sobre instâncias coletadas sem trava (`app/chamados/management/commands/lavrar_perguntas_stale.py:85`, `:87`; `app/chamados/veredito_lavrado.py:230`). UPDATE ainda serializa a escrita, mas decisões desses caminhos não passam pela foto nova. Tratar como alcance da cura, sem afirmar ocorrência em produção.

**OBSERVAÇÃO — memo.** A chave cobre campos da pergunta, `celula.veredito/veredito_via` e, se já carregado, `chamado_destino.status_local` (`app/chamados/juizes.py:811-819`), mas não inclui ata/DNA/batidas; a remoção no diff é necessária para obrigar o C6 a executar outra vez. Remover o memo não atualiza os objetos de relação mantidos no cache — esse problema fica com a análise principal de cache.

**OBSERVAÇÃO — LEI-PROTEGE-SITIO.** A regra distingue explicitamente dono de sítio protegido e proíbe deduzir proteção (`app/docs/LEIS.md:43-48`). O grep exato de `chamados/models.py|carimbar` não encontrou proteção nominal atual de `PerguntaDisputa.carimbar`: L050 cita sítio morto (`app/docs/LEIS.md:91`); DOSSIES registra a porta/donos (`app/docs/DOSSIES.md:46`); CORTES declara outro método, `DisputaSupervisao.pode_fechar` (`app/docs/CORTES.md:83`, `:272`), e o corte sobre resposta vazia é outro ramo, cuja guarda o diff preserva (`app/docs/CORTES.md:384`; `app/chamados/models.py:2587`). Não se pode concluir, por esse grep, que leis gerais da casa deixem de valer.

## 06/11 02:26 · sessao a4b5542bebe9 · H12H06-B · verificador · origem=Code

**REPROVA quanto à abrangência da invalidação:** há janela que o leitor alcança e a nova chamada deixa de fora. (`app/ponto/services/ausencia.py:519`; `app/ponto/turnos.py:2762`)

1. **Alta — ausência aberta deixa projeções futuras carimbadas.**  
   O fim vazio vira `max(data, hoje)`, enquanto a cobertura de `TIPOS_EM_ABERTO` se estende até o fim da competência consultada. (`app/ponto/services/ausencia.py:515–520`; `app/ponto/turnos.py:2753–2767`)

   Contraexemplo: em 06/11, corte 21, INSS aprovado desde 01/10, sem fim, com projeção de dezembro existente; editar para atestado de um dia em 01/10 invalida outubro/novembro, mas dezembro perde a supressão sem perder `derivado_em`. O escritor permite calcular e gravar dezembro; não garante a premissa “competência futura não tem projeção”. Existência desse caso em produção: **[não verificado]**. (`app/ponto/janelas.py:10–33`; `app/ponto/services/ausencia.py:496–520`; `app/ponto/services/ausencia.py:940`; `app/ponto/services/fechamento.py:430–436`, `:610–630`, `:674–692`, `:1127–1128`)

   Para ranges finitos válidos, o envelope cobre antigo/novo, deslocamentos para trás/para frente e competências intermediárias. `NULL` de tipo não aberto pode invalidar competências extras, pois sua cobertura efetiva é um dia. (`app/ponto/services/ausencia.py:516–520`; `app/ponto/services/fechamento.py:1578–1583`; `app/ponto/services/dia_pago.py:712–716`; `app/ponto/turnos.py:2762–2767`)

2. **Alta — permanece caminho de alteração de aprovada sem invalidação.**  
   `curar_ausencia_sem_fim` inclui aprovadas e chama `rederivar_fim`, que apenas salva/relê. O modelo então pode preencher `data_fim`, alterando a cobertura de várias competências sem invalidá-las. É caminho legado; existência de alvos atuais em produção: **[não verificado]**. (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29–31`, `:46`; `app/ponto/services/ausencia.py:1289–1294`; `app/ponto/models.py:902–908`)

   Os sinais não fecham esse buraco: julgam células e reconciliam chamados, sem invalidar o fechamento nesses caminhos. (`app/ponto/signals.py:85–93`, `:143–147`, `:250–272`)

3. **Média — “dentro do atomic do chamador” não vale para a tela.**  
   No comando, edição e invalidação estão no `atomic` de `tratar_linha`; `_Desfaz` desfaz também o carimbo. Na view, não há `atomic` envolvendo a chamada; os blocos internos de `invalidar_fechamento` começam depois do `save` da ausência. As configurações copiadas tampouco habilitam `ATOMIC_REQUESTS`. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137–191`; `app/ponto/views.py:2750–2803`; `app/ponto/services/ausencia.py:509–520`; `app/ponto/services/fechamento.py:1632–1640`; `app/config/settings/saas.py:83–92`; `app/config/settings/prod.py:9–18`)

   Portanto, rollback do comando restaura o carimbo; falha posterior na tela não desfaz automaticamente os atos anteriores. Além disso, falha da invalidação é engolida, retorna zero e não impede confirmar a edição — limite herdado do helper utilizado. (`app/ponto/services/fechamento.py:1645–1649`; `app/ponto/services/ausencia.py:520`)

4. **Média, condicional — contesto descartar a corrida com exportação incondicionalmente.**  
   O apply trava somente a ausência e consulta “entregue” antes de editar; a exportação aceita competência solicitada, calcula sua fotografia e registra o TXT sem compartilhar essa trava. É possível consultar “não entregue”, exportar a fotografia anterior e confirmar a edição concorrentemente. Ocorrência em produção: **[não verificado]**. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137–170`; `app/folha/views.py:109–120`, `:207–209`, `:297–300`; `app/folha/models.py:174–188`)

   “Antes de 21/11” não constitui garantia pelo código: o portão verifica carimbo do dia/competência e falhas, sem exigir encerramento temporal. A ausência operacional de exportação concorrente continua **[não verificado]**. (`app/folha/porta_export.py:800–867`)

5. **Baixa — teste de rollback pode passar vazio isoladamente.**   [nao verificado]
   `test_linha_desfeita_nao_vence_a_projecao` exige apenas carimbo preenchido; também passa se o comando pular antes de editar. O irmão da trilha verifica resumo e minutos restaurados, mas o teste novo não demonstra que chegou a invalidar antes do rollback. (`app/ponto/tests/test_h12h06b_rederivar_minutos.py:254–259`, `:163–169`)

   O teste de range cobre somente avanço de um atestado aguardando documento, de outubro para novembro — competências 11→12. Não cobre aberto, retorno para trás, fim antigo mais distante nem exportada/barrada. (`app/ponto/tests/test_h12h06b_rederivar_minutos.py:72–76`, `:261–272`)

**Rastreio de `derivado_em`:**

| Papel | Quem / comportamento | Evidência |
|---|---|---|
| Preenche | Recalcular fechamento: `save` para aberto, `update` para demais status | `app/ponto/services/fechamento.py:1121–1128`, `:1144–1173` |
| Esvazia | `invalidar_fechamento`; `restaurar_fechamento`, salvo valor explicitamente fornecido | `app/ponto/services/fechamento.py:1635–1640`, `:1364–1377` |
| Lê `NULL` | Queryset/contador de projeções vencidas e comando que conta/lista; não rederivam | `app/ponto/services/fechamento.py:1675–1683`, `:1705`; `app/ponto/management/commands/projecoes_vencidas.py:47–68` |
| `decidir` | Chama invalidação; não consulta o carimbo | `app/ponto/services/ausencia.py:942–944` |
| Evento | Recalcula após commit, independentemente do carimbo | `app/ponto/services/fechamento.py:1791–1808` |
| `fechamento_lido` | Deriva em memória, independentemente do carimbo | `app/ponto/services/fechamento.py:1743–1745` |
| Leitores do gravado | Os caminhos padrão de classificação/export carregam valores sem testar o carimbo | `app/folha/export.py:709–724`, `:775–780` |
| TXT pela view | Injeta fechamentos derivados em memória | `app/folha/views.py:207–212` |

O que fica vazio é **o carimbo**, não as rubricas. A invalidação alcança também exportadas/barradas dentro da janela; a guarda da 09 pode impedir reconstrução, deixando valores antigos com carimbo nulo. (`app/ponto/services/fechamento.py:1635–1640`, `:549–555`)

Os testes positivos têm mordida: começam com carimbos preenchidos e exigem `None`; o DSR consulta `FechamentoMensal` gravado, usa motor real e exige delta exato `(+1, −1)` para 720/0, com contraste 479. O teste de 720 exige perda prévia; há mock da lavra como `{}`, limitando a integração comprovada. Execução GREEN/RED/M16/M17: **[não verificado]**. (`app/ponto/tests/test_h12h06b_rederivar_minutos.py:241–252`, `:261–272`; `app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:130–178`)

Nos leitores examinados, a decisão de parcial continua delegada ao juiz existente; não identifiquei nova derivação paralela nesses sítios. O relógio é lido dentro da porta de edição; classificá-lo sozinho como “fora da borda” não está demonstrado. (`app/ponto/services/fechamento.py:681–685`; `app/ponto/services/efeito_ausencia.py:33–55`; `app/ponto/services/credito_parcial.py:20–27`; `app/ponto/turnos.py:2583–2600`; `app/ponto/services/ausencia.py:513–519`)

O DIFF de impacto anterior ao pouso permanece **[não verificado]**; “medida em curso” não demonstra cumprimento do pré-requisito. A exigência continua registrada, e eventos continuam podendo regravar competências exportadas fora da guarda da 09. (`app/docs/LEIS.md:112`, `:142`; `app/ponto/services/fechamento.py:530–555`, `:1793–1794`)

## 06/11 02:26 · sessao b2671fcd93c3 · - · - · origem=Ronald (tmux)

Achados principais:

1. **Alta — atomicidade prometida não existe na tela.** `app/ponto/services/ausencia.py:509` salva a ausência antes da invalidação em `:520`; a função começa em `:465` sem `atomic`. O chamador de tela tem apenas `@login_required` (`app/ponto/views.py:2750`) e chama a porta em `:2794`. A configuração copiada não ativa `ATOMIC_REQUESTS` (`app/config/settings/saas.py:83`). Portanto, o comentário “Dentro do atomic do chamador” (`app/ponto/services/ausencia.py:512`) só corresponde ao comando: a tela deixa escrita e carimbo em transações separadas. Configuração efetiva de produção: `[não verificado]`.

2. **Alta, caminho legado — outro escritor altera insumo de aprovada sem invalidar.** `curar_ausencia_sem_fim` seleciona todas as não rejeitadas, incluindo aprovadas (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`), e chama `rederivar_fim` em `:46`. Essa função apenas salva e relê (`app/ponto/services/ausencia.py:1289`), sem invalidar. O modelo preenche `data_fim` (`app/ponto/models.py:902`) e também pode preencher minutos de falta aprovada zero (`:877`). É outro caminho de escrita dos campos perguntados, embora dentro do arquivo declarado como porta. O comando é classificado como cura de acervo, uma vez (`app/config/crons.py:1257`). Existência atual de alvos: `[não verificado]`.

3. **Alta, contestação do descarte — executar antes de 21/11 não elimina estruturalmente a corrida com exportação.** O comando trava somente Ausencia (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`), consulta a entrega em `:160` e depois escreve em `:169`. O export recebe mês/ano sem exigir competência encerrada (`app/folha/views.py:109`); `conferir` verifica carimbo do dia, competência e falhas (`app/folha/porta_export.py:800`), sem barreira de data. Lê a projeção online (`app/folha/views.py:207`) e registra o TXT posteriormente (`:297`), usando `exists/create`, sem trava comum (`app/folha/models.py:174`). Interleaving possível: cerca passa → export lê dados antigos → apply confirma → export registra TXT antigo. Ocorrência real e carimbo verde atual: `[não verificado]`.

Conferências:

- **Comando:** `transaction.atomic()` em `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137` contém porta e invalidação. `_Desfaz` por diferença de campos/valor ou auditoria ausente (`:175`, `:178`, `:186`) desfaz também o carimbo.
- **Signals não cobrem o segundo escritor:** o receiver da Ausencia chama cartório (`app/ponto/signals.py:143`); `_julgar_dias` julga células (`:85`). O consumidor apenas reconcilia chamados após commit (`:250`).
- **Admin não é escritor adicional:** AusenciaAdmin usa SoLeitura (`app/ponto/admin.py:37`), que nega adicionar/editar/apagar (`app/core/admin.py:59`).
- **Form não é escritor adicional:** AusenciaForm deriva de `forms.Form` (`app/ponto/forms.py:41`).
- **Férias não edita esses campos de Ausencia existente:** o modelo delega o espelho (`app/ferias/models.py:379`); o espelho mantém existentes e cria ausentes (`app/ponto/services/ausencia.py:1194`).

## 06/11 02:25 · sessao 78bcf75af9d3 · - · - · origem=Ronald (tmux)

**Achado: janela incompleta para ausência aberta com projeção futura existente — gravidade alta, condicional à existência desse gravado.** `editar_ausencia_registro` substitui `data_fim=NULL` pelo máximo entre início e hoje, limitando a invalidação (`app/ponto/services/ausencia.py:515-520`); o leitor de cobertura estende ausência de `TIPOS_EM_ABERTO` até o fim de qualquer janela consultada (`app/ponto/turnos.py:2753-2767`).

Contraexemplo por código: em 06/11, corte21, ausência aprovada INSS iniciada em 01/10, sem fim, e projeção de 12/2026 existente; editar para atestado de um dia em 01/10 invalida somente competências10/11, mas a12 perde toda a supressão antiga sem perder seu carimbo (`app/ponto/services/ausencia.py:496-520`; `app/ponto/janelas.py:10-33`; `app/ponto/services/fechamento.py:674-692`). O escritor aceita competência futura: seu universo não limita mês/ano por hoje e a gravação usa a janela pedida (`app/ponto/services/fechamento.py:430-436`, `:610-630`, `:1121-1128`). Existência desse caso em produção: `[não verificado]`. A premissa “competência futura não tem projeção gravada” aparece no comentário de `decidir`, mas não é garantida pelo escritor (`app/ponto/services/ausencia.py:938-943`; `app/ponto/services/fechamento.py:430-436`).

Para ranges finitos válidos, o envelope mínimo dos inícios/máximo dos fins cobre antigo e novo, inclusive deslocamentos nos dois sentidos; sua tradução em competências percorre todas as janelas pelo corte da empresa (`app/ponto/services/ausencia.py:496-520`; `app/ponto/services/fechamento.py:1578-1583`; `app/ponto/services/dia_pago.py:700-716`). Para `NULL` de tipo não aberto, o limite hoje apenas pode invalidar competências extras, pois a cobertura efetiva é um dia (`app/ponto/services/ausencia.py:519`; `app/ponto/turnos.py:2762-2767`).

**Quem escreve/lê `derivado_em`.**

- Escritores com carimbo preenchido: recalcular fechamento aberto por `save`, e demais status por `update` (`app/ponto/services/fechamento.py:1121-1128`, `:1144-1173`).
- Escritores que apagam: `invalidar_fechamento` (`app/ponto/services/fechamento.py:1632-1640`); `restaurar_fechamento`, salvo se o chamador fornecer explicitamente outro valor (`:1364-1377`). O default do modelo é `NULL` (`app/ponto/models.py:454-464`).
- Leitores explícitos do carimbo: `projecoes_vencidas_qs`, contador e comando de listagem; detectam `NULL` e imprimem/contam, sem rederivar (`app/ponto/services/fechamento.py:1675-1683`, `:1705`; `app/ponto/management/commands/projecoes_vencidas.py:47-68`). O default observa somente a competência corrente (`app/ponto/services/fechamento.py:1676-1679`; `app/ponto/janelas.py:102-108`).
- `decidir` escreve invalidação, sem ler o carimbo (`app/ponto/services/ausencia.py:942-944`); `recalcular_por_evento` recalcula incondicionalmente após commit (`app/ponto/services/fechamento.py:1791-1808`); `fechamento_lido` deriva incondicionalmente em memória (`:1743-1745`).
- A folha continua carregando o gravado sem filtro de `derivado_em`: `classificar_export` (`app/folha/export.py:775-780`) e `montar_linhas_export` (`:709-724`). Portanto, carimbo vazio alarma, mas não força atualização dos números consumidos nesses caminhos (`app/ponto/services/fechamento.py:1675-1683`; `app/folha/export.py:709-724`).
- O carimbo é excluído das comparações de fotografia/hash (`app/ponto/portas/regen_exportada.py:35-38`; `app/ponto/management/commands/carimbo_gravado.py:27-30`).

**Exportada/barrada:** a invalidação não filtra status, exportação nem guarda da09, portanto também apaga esses carimbos (`app/ponto/services/fechamento.py:1635-1640`). A reconstrução da09 exportada é barrada pelo universo do escritor (`:549-555`), enquanto a leitura online segue disponível pelo modo `somente_leitura=True` (`:530-531`, `:1743-1745`). O gravado barrado pode permanecer `NULL`, com os números antigos ainda disponíveis ao export (`app/folha/export.py:709-724`). Isso decorre do desenho existente; introdução desse comportamento pelo delta `[não verificado]`.

## 06/11 02:25 · sessao 9456bae2f482 · - · - · origem=Ronald (tmux)

1. **Não bloqueante — lacuna de cobertura da janela.** O teste de range cobre somente mover `27/10` para `23/11`, com `data_fim=None` e status `aguardando_documento`; não cobre fim antigo posterior ao novo, edição para trás, intervalo aberto nem exportada/barrada. O nome promete mais que os casos exercitados. `app/ponto/tests/test_h12h06b_rederivar_minutos.py:72`, `:261-272`.

2. **Não bloqueante — rollback pode passar vazio isoladamente.** `test_linha_desfeita_nao_vence_a_projecao` verifica apenas que `derivado_em` continua preenchido: passa também se o comando pular antes do escritor. Não afirma resumo, minutos restaurados ou que a invalidação aconteceu dentro da transação. O teste irmão da trilha cobre resumo `puladas=2` e minutos restaurados; portanto a suíte tem contraparte, mas este teste sozinho não comprova reversão do carimbo após invalidar. `app/ponto/tests/test_h12h06b_rederivar_minutos.py:254-259`, `:163-169`. O caminho real invalida antes da falha de trilha, que lança `_Desfaz` fora do `atomic`. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`, `:169-191`; `app/ponto/services/ausencia.py:509-520`.

3. **Testes positivos têm mordida estrutural.** O apply começa com duas projeções preenchidas e exige ambas nulas; o teste direto começa com comp11/12 preenchidas e exige ambas nulas. Suprimir a invalidação deixa esses valores iniciais vivos, salvo outro efeito ainda não demonstrado. `app/ponto/tests/test_h12h06b_rederivar_minutos.py:241-252`, `:261-272`. Execução dos RED/M16/M17: **[não verificado]**; os logs citados no commit não foram encontrados na listagem de `logs`/`app/logs` **[não verificado quanto à existência fora desses diretórios]**.

4. **A cura do GRAVADO é efetiva pelo código do teste.** O helper chama `recalcular_fechamento_mes`, espia `get_motor_cct` devolvendo o motor real, exige que o motor tenha sido chamado e lê `FechamentoMensal` persistido. Exige delta exato `(ok+1, perdido−1)` para 720 e 0; 720 ainda exige semana previamente perdida. Isso não passa por saída vazia. `app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:130-139`, `:152-178`. O escritor persiste os campos do resultado real do motor, cuja regra usa presença/justificação dos dias obrigatórios. `app/ponto/services/fechamento.py:1085-1086`, `:1127-1128`; `app/ponto/motor_calculo_v2.py:2595-2608`, `:2629-2630`. Limite: `esmeril_espelho.ler_lavra` é mockado como `{}`, portanto estes testes não demonstram convivência com lavra de produção. `app/ponto/tests/test_h12h06b_parcial_pelo_juiz.py:136`.

5. **Classes arquiteturais:** os leitores de dinheiro consultam o mesmo juiz `cobre_parte_do_dia`, cujo corpo centraliza tipo parcial, dia único e limite 480; não identifiquei juiz paralelo novo nesses sítios. `app/ponto/services/fechamento.py:681-685`; `app/ponto/services/efeito_ausencia.py:33-55`; `app/ponto/services/credito_parcial.py:20-27`; `app/ponto/turnos.py:2583-2600`. A composição `TIPOS_PAGOS and cobre_parte_do_dia` continua repetida nos quatro sítios, como no apontamento anterior. Mesmas referências.

6. **Relógio:** o delta lê `localdate()` dentro da porta, sem entrada `hoje/agora`; os testes congelam globalmente o relógio. `app/ponto/services/ausencia.py:513-519`; `app/ponto/tests/test_h12h06b_rederivar_minutos.py:36`. A classificação “relógio lido fora da borda” consta do checklist, mas regra específica que faça essa leitura, isoladamente, bloquear a obra: **[não verificado]**. `CLAUDE.md:365-367`.

## 06/11 02:18 · sessao e1c59897b38c · FILA1-ESTADO · leitura · origem=Code

“NO AR” abaixo significa **deploy registrado nos anexos**, sem confirmação independente de produção [nao verificado]. Os hashes são os citados nesses registros; a cópia não permite conferir o histórico Git, e o `git log --oneline -60` não veio no dado fornecido [nao verificado]. [nao verificado]

**MANHA-0511 — estado por item**, conforme a enumeração de `app/docs/BACKLOG.md:1738`:

| Item | Estado atual com prova |
|---|---|
| **1 — F3-BECO-1** | **NO AR**, árvore certificada `3b8d4a3e`, deploy `9c0fd9d3`, rc=0. Permanecem os itens de fila do opus e a lei **F3-PERGUNTAS-DO-APP-463**; portanto, pouso comprovado não equivale a encerramento integral. `app/docs/RELATO.md:6441`, `app/docs/RELATO.md:6444`, `app/docs/RELATO.md:6446`, `app/docs/RELATO.md:59`. |
| **2 — JUIZ-1 vinculo_do_dia** | **NO AR**, v5, deploy `8ece7834` às 13:49, com instalação do crontab registrada. O hash foi depois amendado para `98ce09f8`; outra reescrita preservou as árvores e terminou no HEAD `630131b1`. Correspondência individual final após essa reescrita: [nao verificado]. `app/docs/RELATO.md:78`, `app/docs/RELATO.md:79`, `app/docs/RELATO.md:80`. |
| **3 — 12x36 (B)** | **Incluído na v5 do JUIZ-1 NO AR**: a medida registra os casos col334/col727 passando de feriado para trabalho; o aval vincula expressamente a frota (B) à volta 5. Mesmo deploy `8ece7834` do item 2. `app/docs/RELATO.md:94`, `app/docs/RELATO.md:97`, `app/docs/RELATO.md:78`. |
| **4 — JUIZ-3 (a), outbox** | **NO AR `de9fd69b`**, suite da árvore `0f918814`, deploy rc=0. O registro posterior mede a rodada das 20:30; ainda declara que o crontab ficou sem `--alarme`. `app/docs/RELATO.md:6517`, `app/docs/RELATO.md:6522`, `app/docs/RELATO.md:6523`, `app/docs/RELATO.md:6525`. |
| **5 — JUIZ-4 (i), FIO MUDO** | **Em construção**, como **C20-FIO-MUDO**: abertura do construtor registrada e posteriormente mantida entre os trabalhos ativos. **Hash da implementação e pouso: [nao verificado]**. O `8238da4c` comprova somente **JUIZ-4A**, não este ramo (i). `app/docs/RELATO.md:6512`, `app/docs/RELATO.md:6570`, `app/docs/RELATO.md:109`; distinção de escopo em `app/docs/PROMPTS.md:1875`. |
| **6 — JUIZ-2, retido no TOTAL** | **NO AR `0cd6261a`**, merge da cópia `0c99a2cf`, contendo o produto `78b755f8` e a confirmação de que o retido fica no TOTAL; deploy rc=0. Isso comprova esta parte, não o rebaixamento completo do cron, tratado depois como LASTRO-PARTE-2. `app/docs/RELATO.md:111`, `app/docs/BACKLOG.md:1740`. |
| **7 — O261 item 3, PEP** | **Sem implementação comprovada nesta retomada**: aguardava vaga; depois recebeu pré-código e casos para o futuro construtor. **Hash próprio de cura/pouso: [nao verificado]**. `9c0fd9d3` e `3a514a4e` são bases das leituras registradas. `app/docs/RELATO.md:6427`, `app/docs/RELATO.md:6507`, `app/docs/RELATO.md:6599`, `app/docs/RELATO.md:6602`. |
| **8 — OTIMIZA-3** | **Em raia `wt-qc-224741 @ d1354f77`, sem construtor na última indicação explícita**. A reprovação/B3 antiga está em `RELATO:126`, mas MANHA-0511 declara essa parada respondida; posteriormente a obra continuava aguardando abertura. **Novo hash ou pouso: [nao verificado]**. `app/docs/RELATO.md:126`, `app/docs/RELATO.md:104`, `app/docs/RELATO.md:6472`, `app/docs/RELATO.md:6507`. |

**NOITE-0411 — cobertura da lista que permanece no BACKLOG**, incluindo seus desdobramentos posteriores (`app/docs/BACKLOG.md:1737`):

| Item | Estado atual com prova |
|---|---|
| **1 — E4-8-SAIDA-I** | **NO AR `115196f4`**. O `!` por pouso fora da janela foi **respondido: manter**, pelo aval POUSO-E4-8-MANTEM. `app/docs/RELATO.md:6316`, `app/docs/RELATO.md:212`. |
| **2 — OTIMIZA 1-3** | **OTIMIZA-1:** em raia `wt-si-2235 @ 2d91d590`; faltavam suite, replay, verificador e pouso. PRONTO 5 ficou em fila. **OTIMIZA-2:** registrado parado pela cota, sem retorno à ordem da noite; execução posterior [nao verificado]. **OTIMIZA-3:** estado descrito no item 8 acima. `app/docs/RELATO.md:179`, `app/docs/RELATO.md:169`, `app/docs/BACKLOG.md:1791`, `app/docs/RELATO.md:6472`. |
| **3 — espelho.py::_status_dia** | **NO AR `7f275efd`**, deploy rc=0; catraca viva caiu para 1. `app/docs/RELATO.md:6327`. |
| **4 — E4-APP-MAPA** | **NO AR `b051f0ae`**, deploy rc=0. `app/docs/RELATO.md:6338`. |
| **5 — catraca=0** | **FECHADO por medida viva**, no pouso `b051f0ae`: número 0, itens e novos vazios. `app/docs/RELATO.md:6338`. |
| **6 — E4-1/E4-6 SAIDA-II + JUIZ-1/JUIZ-2** | **E4-1/E4-6 NO AR `fb84b97d`**; **JUIZ-1 NO AR**, conforme item 2 da manhã; **JUIZ-2 parte 1 NO AR `0cd6261a`**. **LASTRO-PARTE-2** permanece em investigação de origem, com cura+vigia aguardando construtor; hash dessa parte [nao verificado]. `app/docs/RELATO.md:210`, `app/docs/RELATO.md:78`, `app/docs/RELATO.md:111`, `app/docs/RELATO.md:48`. |
| **6b — F3-DORMENTE, beco 1** | **NO AR**, deploy `9c0fd9d3`, com a lei residual indicada no item 1 da manhã. `app/docs/RELATO.md:6446`, `app/docs/RELATO.md:59`. |
| **6c — juízes 3 e 4** | **JUIZ-3 NO AR `de9fd69b`**. **JUIZ-4A NO AR `8238da4c`**; **FIO MUDO em construção**, sem hash/pouso comprovado. `app/docs/RELATO.md:6517`, `app/docs/RELATO.md:109`, `app/docs/RELATO.md:6512`. |
| **7 — OTIMIZA 4-5** | Último estado explícito: **aguardavam contrato ao Codex/cota**. Construção, hash ou pouso posterior: **[nao verificado]**. `app/docs/RELATO.md:152`. |
| **9 — motor v3 pela bateria** | **Sem abertura comprovada desta retomada**: o registro diz que não abre sem Codex. Hash ou conclusão desse item da noite: **[nao verificado]**. `app/docs/RELATO.md:134`. |

Nos desdobramentos da noite também há **N1-PERGUNTAS-DO-APP NO AR `26ee6979`**, **LAVRAR-HE NO AR `95544d22`**, **BUG-SW NO AR `4d5fd8df`** e **N1-ALIMENTACAO-UNICA NO AR `e44d7efa`**. `app/docs/RELATO.md:193`, `app/docs/PROMPTS.md:1867`, `app/docs/RELATO.md:184`, `app/docs/RELATO.md:149`.

**Substituições e alterações posteriores:**

- **NOITE-NON-STOP**, de 05/11 00:3x, reordena os pousos da noite; **MANHA-0511**, de 08:3x, declara expressamente substituir as versões anteriores e a ordem da NOITE-0411. O marcador continua literalmente em NOITE-0411. `app/docs/PROMPTS.md:1851`, `app/docs/PROMPTS.md:1853`, `app/docs/BACKLOG.md:1738`, `app/docs/BACKLOG.md:8`.
- Após MANHA, há alterações explícitas: **CRON-MARCA-COBRE-INSTALL** insere (c)(1) antes do item 4; **F3-PORTA-E-CRON-PRAZO** substitui o encerramento previsto no item 1; **LASTRO-PARTE-2** ganha encaixe após JUIZ-1; **CONTRATO-20-DECISOES** libera e redefine JUIZ-3/FIO MUDO numa sequência local. `app/docs/PROMPTS.md:1854`, `app/docs/PROMPTS.md:1857`, `app/docs/PROMPTS.md:1859`, `app/docs/PROMPTS.md:1875`.
- Uma **substituição integral posterior de MANHA-0511** não está comprovada [nao verificado]. O complemento do piloto de 16:4x diz expressamente **“ordem e fila intocados”**. `app/docs/PROMPTS.md:1881`.

## 06/11 02:13 · sessao ef5279bc9e8c · H12H06-B · censo · origem=Code

Nas referências aos anexos, conto as linhas do conteúdo colado, começando em 1 e incluindo linhas vazias.

1. **BLOQUEANTE 1 — (b), com ressalva: visto e adiado para medição; não houve descarte técnico documentado.** O alcance sobre exportadas foi apontado pelo Codex. A triagem deixou o problema sem tratamento nesta obra e encaminhou a medição à sessão. (`codex_v1.r:5`; `codex_v2.q:7`)

   **Pergunta literal:**
   > “O complemento move dinheiro em quem? (classes de ausencia/tipo com minutos>=480 ou range com minutos), e quais leitores de  
   > tela/PDF mudam de numero.” (`codex_v1.q:23-24`)

   **Resposta literal:**
   > “A cerca do comando não protege esse caminho do complemento.” (`codex_v1.r:5`)

   **Motivo registrado na triagem:**
   > “#2 recalculo de exportada pelo complemento: nao curado nesta obra; vai como pergunta a sessao (medir na frota exportada).” (`codex_v2.q:7`)

   A quantificação efetiva do DIFF pertence a **(c), dado de produção**. O Codex delimitou isso expressamente:
   > “Pessoas e valores efetivamente alterados em produção: **[não verificado]**.” (`codex_v1.r:49`)

   Portanto, o risco foi visto; a exigência específica de DIFF com filtro e sentido não aparece formulada na resposta. (`codex_v1.r:5`; `codex_v1.r:49`; `opus_final_4e9380e5.md:2`)

2. **BLOQUEANTE 2 — (a): não apontado, com o tema presente na pergunta.** A pergunta cobrava efeitos da edição que escapassem à conferência local; a resposta enumerou efeitos externos, mas não apontou a falta de invalidação do fechamento. (`codex_v1.q:14-16`; `codex_v1.r:39-41`)

   **Pergunta literal:**
   > “Ha efeitos colaterais do editar_ausencia_registro + Ausencia.save (signals, rejulgar, cartorio,  
   > prazo_documento, aprovada_em) que a conferencia "so minutos mudou" nao pega, ou que pegaria e desfaria indevidamente?” (`codex_v1.q:15-16`)

   **Resposta literal:**
   > “A comparação não alcança efeitos em `CelulaDia`, eventos, chamados ou perguntas.” (`codex_v1.r:41`)

   A cópia permite conferir a omissão: `editar_ausencia_registro` salva e retorna sem chamar `invalidar_fechamento`; a chamada aparece em `decidir`. (`app/ponto/services/ausencia.py:508-515`; `app/ponto/services/ausencia.py:925-932`)

3. **NÃO BLOQUEANTE — comentário desatualizado: (e), NÃO PERGUNTADO.** As perguntas fornecidas não cobram atualização de comentários; as respostas não registram esse comentário como achado. Não há trecho literal específico de pergunta ou resposta para reproduzir. (`codex_v1.q:10-28`; `codex_v2.q:17-24`; `codex_v3.q:14-20`; `codex_v4.q:13-19`; `codex_v1.r:1-53`; demais respostas fornecidas)

   O comentário citado pelo opus existe na cópia:
   > “somam `minutos` de `TIPOS_PAGOS` aprovados” (`app/ponto/services/ausencia.py:920`)

   Atribuir à triagem um descarte desse comentário seria **[não verificado]**.

4. **NÃO BLOQUEANTE — predicado composto repetido: (a), pelo tema amplo perguntado.** A pergunta cobrou duplicação de juiz e derivação paralela. A resposta verificou a chamada ao juiz existente, sem registrar como achado a repetição da combinação com `TIPOS_PAGOS`. (`codex_v1.q:25-26`; `codex_v1.r:51`; `opus_final_4e9380e5.md:7`)

   **Pergunta literal:**
   > “2o juiz que duplica juiz registrado” (`codex_v1.q:25`)
   >
   > “derivacao paralela de fonte unica” (`codex_v1.q:26`)

   **Resposta literal:**
   > “não encontrei juiz novo nos três leitores migrados: todos chamam `cobre_parte_do_dia`” (`codex_v1.r:51`)

   A pergunta não nomeou expressamente a repetição do **predicado composto**; a classe (a) decorre da abrangência do tema cobrado. (`codex_v1.q:25-26`)

5. **NÃO BLOQUEANTE — teste não afirma o resultado em FechamentoMensal: VISTO, mas sem descarte documentado.** Aqui a classificação oferecida não fecha: não cabe (a), pois o Codex descreveu precisamente a limitação; para afirmar (b), falta o registro do descarte e seu motivo. (`codex_v1.r:53`; `triagem_v4.md:2-6`) [nao verificado]

   **Pergunta literal:**
   > “selo/RED sem mordida (mock que nao morde, teste vacuo)” (`codex_v1.q:26`) [nao verificado]

   **Resposta literal:**
   > “Porém ele verifica a alimentação `datas_justificadas`, não os valores financeiros persistidos ou o PDF.” (`codex_v1.r:53`)

   **Motivo de eventual descarte pela triagem: [não verificado].** Classificá-lo como “Codex não viu” ou “NENHUM viu” contrariaria esse trecho explícito. (`codex_v1.r:53`)

## 06/11 02:12 · sessao 08d6671f0bc7 · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE 1: visto; encaminhado à medição.** Aproxima-se de **(b)**, mas “descarte” não está documentado: foi mantido aberto. `codex_v1.q`, pergunta 6: “O complemento move dinheiro em quem? [...] e quais leitores de tela/PDF mudam de numero.” `codex_v1.r`, achado 2: “o complemento pode mudar exportada por recálculo genérico.” `codex_v2.q`, triagem: “#2 recalculo de exportada pelo complemento: nao curado nesta obra; vai como pergunta a sessao (medir na frota exportada).” A medição efetiva do DIFF é **(c)**; resultado não fornecido. Linhas dos anexos: [não verificado].

2. **BLOQUEANTE 2: (a).** Tema presente em `codex_v1.q`, pergunta 2: “Ha efeitos colaterais do editar_ausencia_registro + Ausencia.save [...] que a conferencia ‘so minutos mudou’ nao pega”. O próprio texto do commit anexado diz “FechamentoMensal fica stale ate recalculo”. A resposta percorre efeitos, mas não aponta ausência de invalidação: “A comparação não alcança efeitos em `CelulaDia`, eventos, chamados ou perguntas.” (`codex_v1.r`). Linhas dos anexos: [não verificado]. [nao verificado]

3. **NB comentário: (e).** Nenhuma pergunta fornecida cobra comentário desatualizado ou sincronização da documentação de `ausencia.py:920-921`. Não existe trecho positivo de `.q` ou `.r` para esse achado; ausência constatada nos anexos, linhas [não verificado].

4. **NB predicado composto: (a), pelo tema amplo; fronteira com (e).** `codex_v1.q`, pergunta 7: “2o juiz que duplica juiz registrado; [...] derivacao paralela de fonte unica”. `codex_v1.r`: “não encontrei juiz novo nos três leitores migrados: todos chamam `cobre_parte_do_dia`”. A resposta conferiu o componente chamado, sem registrar a repetição da combinação. A pergunta não nomeia explicitamente duplicação do predicado composto; se exigir literalidade estrita do objeto, seria (e). Linhas dos anexos: [não verificado].

5. **NB teste FechamentoMensal: visto, sem destino documentado — taxonomia incompleta.** `codex_v1.q`, pergunta 7: “selo/RED sem mordida (mock que nao morde, teste vacuo)”. `codex_v1.r`: “Porém ele verifica a alimentação `datas_justificadas`, não os valores financeiros persistidos ou o PDF.” Isso impede (a), (d) e (e); a triagem fornecida não registra descarte nem motivo, portanto **(b) não está comprovada**. Linhas dos anexos e motivo do eventual descarte: [não verificado]. [nao verificado]

Não classificaria nenhum dos cinco como **(d)**: há achados vistos pelo Codex e omissões de pergunta; “nenhum viu” também seria incompatível com o próprio achado do opus. [não verificado]

## 06/11 02:07 · sessao 657abac16215 · H12H06-B · sonda · origem=Code

**Parte A — funções reais e limite da conferência.** O conteúdo de `sonda_h12h06b.py` não foi localizado na cópia nem consta do anexo transcrito; portanto, seus imports, argumentos, filtros e eventuais reconstruções de lógica ficam **[nao verificado]**. A vinculação da cópia ao HEAD informado também fica **[nao verificado]**. O identificador entre colchetes no cabeçalho do envelope é o SHA-256 abreviado **do script**, calculado em `bin/sonda_leitura.sh:219` e impresso em `bin/sonda_leitura.sh:221`.

As quatro funções existem com estas assinaturas:

| Função | Assinatura real | Fonte |
|---|---|---|
| `parciais_vigentes` | `parciais_vigentes()` | `app/ponto/catalogo/ausencias.py:336` |
| `cobre_parte_do_dia` | `cobre_parte_do_dia(a, parciais=None)` | `app/ponto/turnos.py:2583` |
| `janela_atual` | `janela_atual(data, empresa=None)` | `app/ponto/janelas.py:60` |
| `competencia_entregue` | `competencia_entregue(empresa_id, mes, ano, registro=False)` | `app/folha/export.py:556` |

`parciais_vigentes()` seleciona a flag de parcial do catálogo vigente; `cobre_parte_do_dia()` exige tipo nesse conjunto, registro de um único dia e **`0 < minutos < 480`**. `janela_atual()` recebe a data primeiro e aplica o corte da empresa. `competencia_entregue()` consulta a exportação vigente e exige que não esteja invalidada. (`app/ponto/catalogo/ausencias.py:357`; `app/ponto/turnos.py:2580`; `app/ponto/turnos.py:2595`; `app/ponto/turnos.py:2598`; `app/ponto/janelas.py:63`; `app/folha/export.py:580`)

**O filtro `status='aprovada', minutos>0` não é o universo integral dos leitores.** Para folha, o carregador filtra aprovadas, mas não exige minutos positivos; para cobrança, exclui apenas rejeitadas. Os carregadores também consideram interseção de datas e os intervalos abertos permitidos. (`app/ponto/turnos.py:2514`; `app/ponto/turnos.py:2516`; `app/ponto/turnos.py:2529`; `app/ponto/turnos.py:2753`)

Quem lê e como trata minutos:

| Leitor | Leitura efetiva | Fonte |
|---|---|---|
| Fechamento mensal | Carrega ausências para folha; soma minutos de tipos pagos e pula o efeito de dia inteiro quando esses minutos são não zero | `app/ponto/services/fechamento.py:630`, `:665`, `:685` |
| Efeitos da ausência | Usa os carregadores para folha e o mesmo teste de tipo pago com minutos não zero | `app/ponto/services/efeito_ausencia.py:43`, `:44`, `:49`, `:51` |
| Crédito parcial | Acumula minutos de tipos pagos pela data inicial | `app/ponto/services/credito_parcial.py:23`, `:24` |
| Exportação | Filtra aprovadas, tipos solicitados e interseção da janela, sem exigir minutos positivos | `app/folha/export.py:87`, `:88`, `:90` |

Nos três primeiros leitores, esses testes locais usam **tipo pago + verdade lógica de `minutos`**, sem chamar `cobre_parte_do_dia()` nesses pontos; portanto, não aplicam ali o limite de 480 nem a condição de dia único do juiz. São tratamentos locais distintos do predicado real. Reconstruções dentro **da sonda** permanecem indeterminadas. (`app/ponto/services/fechamento.py:665`; `app/ponto/services/fechamento.py:685`; `app/ponto/services/efeito_ausencia.py:44`; `app/ponto/services/efeito_ausencia.py:51`; `app/ponto/services/credito_parcial.py:24`; `app/ponto/turnos.py:2598`) **[nao verificado — conteúdo da sonda]**

**Parte B — saída fornecida.** Nela, **`falta` não está em `parciais_vigentes()`**: o resultado é `false`, e a lista contém `abon_hr`, `atestado`, `atraso`, `declaracao`, `saida_antecipada`. **[nao verificado — dado fornecido, sem nova consulta]**

A tabela abaixo preserva `n` e os identificadores recebidos, inclusive quando há vários registros para um mesmo identificador. **[nao verificado — dado fornecido]**

| Empresa | Competência | Exportada | Tipo | n | Colaboradores | Evidência |
|---:|:---:|:---:|---|---:|---|---|
| 2 | 06/2026 | false | falta | 1 | col43 | [nao verificado] |
| 4 | 06/2026 | true | falta | 3 | col39, col40, col47 | [nao verificado] |
| 3 | 07/2026 | true | abono | 1 | col148 | [nao verificado] |
| 3 | 07/2026 | true | falta | 2 | col64 | [nao verificado] |
| 4 | 07/2026 | true | falta | 1 | col676 | [nao verificado] |
| 2 | 08/2026 | true | abono | 3 | col666, col819, col905 | [nao verificado] |
| 2 | 08/2026 | true | atestado | 7 | col284, col342, col400, col433, col707, col842 | [nao verificado] |
| 2 | 08/2026 | true | falta | 46 | col187, col200, col228, col237, col272, col328, col335, col338, col344, col347, col366, col406, col433, col442, col489, col493, col552, col568, col718, col760, col783, col823, col874 | [nao verificado] |
| 2 | 08/2026 | true | saida_antecipada | 1 | col649 | [nao verificado] |
| 3 | 08/2026 | false | atestado | 1 | col159 | [nao verificado] |
| 3 | 08/2026 | false | falta | 8 | col58, col60, col122, col128, col743, col769 | [nao verificado] |
| 4 | 08/2026 | false | abono | 1 | col712 | [nao verificado] |
| 4 | 08/2026 | false | atestado_inss | 1 | col42 | [nao verificado] |
| 4 | 08/2026 | false | falta | 3 | col46, col50 | [nao verificado] |
| 2 | 09/2026 | true | atestado | 3 | col518, col885, col906 | [nao verificado] |
| 2 | 09/2026 | true | falta | 114 | col174, col200, col268, col304, col325, col327, col331, col338, col370, col406, col417, col437, col443, col492, col493, col558, col566, col572, col576, col584, col594, col622, col659, col666, col667, col673, col697, col704, col726, col735, col782, col786, col814, col823, col824, col855, col877, col894, col905, col906 | [nao verificado] |
| 2 | 09/2026 | true | suspensao | 1 | col417 | [nao verificado] |
| 3 | 09/2026 | true | atestado | 2 | col111, col643 | [nao verificado] |
| 3 | 09/2026 | true | falta | 16 | col92, col154, col161, col173, col502, col515, col643, col746, col876 | [nao verificado] |
| 3 | 09/2026 | true | suspensao | 1 | col90 | [nao verificado] |
| 4 | 09/2026 | true | atestado | 1 | col40 | [nao verificado] |
| 4 | 09/2026 | true | falta | 2 | col50, col510 | [nao verificado] |
| 1 | 10/2026 | false | falta | 1 | col950 | [nao verificado] |
| 2 | 10/2026 | false | atestado | 17 | col204, col252, col354, col389, col446, col511, col518, col558, col566, col618, col626, col889, col905, col920 | [nao verificado] |
| 2 | 10/2026 | false | falta | 83 | col55, col200, col206, col207, col248, col304, col307, col335, col370, col373, col427, col432, col444, col507, col569, col576, col577, col622, col655, col666, col697, col699, col718, col736, col833, col885, col889, col893, col924, col928, col929, col944, col951, col954, col966, col968 | [nao verificado] |
| 3 | 10/2026 | false | falta | 6 | col113, col173, col643, col752, col769 | [nao verificado] |
| 4 | 10/2026 | false | falta | 2 | col510, col857 | [nao verificado] |
| 2 | 11/2026 | false | atestado | 4 | col200, col701, col891, col967 | [nao verificado] |
| 2 | 11/2026 | false | falta | 20 | col298, col304, col364, col444, col566, col570, col723, col823, col869, col885, col905, col924, col928, col966 | [nao verificado] |
| 3 | 11/2026 | false | falta | 2 | col769, col984 | [nao verificado] |
| 2 | 09/2029 | false | falta | 1 | col667 | [nao verificado] |

**As linhas detalhadas somam 355, não 448.** Os 93 de `parcial_pelo_juiz` aparecem somente no agregado; sua distribuição por empresa, competência, exportada, tipo e colaboradores é **indeterminada**. **[nao verificado — cálculo e limites do dado fornecido]**

| Parcela | n | Evidência |
|---|---:|---|
| Linhas detalhadas com `exportada=false` — coincide numericamente com `aberta` | 151 | [nao verificado] |
| Linhas detalhadas com `exportada=true` — coincide com `exportada` | 204 | [nao verificado] |
| Subtotal detalhado | 355 | [nao verificado] |
| `parcial_pelo_juiz`, sem detalhamento fornecido | 93 | [nao verificado] |
| **Universo: 151 + 204 + 93** | **448** | [nao verificado] |

Combinações ausentes não foram preenchidas com zero; a composição individual dos 93 permanece indeterminada. **[nao verificado — ausência de detalhamento no dado]**

**Parte C(A) — competência exportada grava, com exceção explícita para 09/2026.** No modo de escrita, exportação gera aviso; os fechamentos abertos são salvos com `save()`, e os demais recebem `update()` dos campos calculados. Entretanto, `GUARDA_COMPETENCIA` contém **09/2026**, e os colaboradores das empresas exportadas nessa competência são excluídos do processamento, preservando seus fechamentos nessa chamada. (`app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:530`; `app/ponto/services/fechamento.py:532`; `app/ponto/services/fechamento.py:551`; `app/ponto/services/fechamento.py:555`; `app/ponto/services/fechamento.py:1121`; `app/ponto/services/fechamento.py:1128`; `app/ponto/services/fechamento.py:1144`)

Com `somente_leitura=True`, não grava `FechamentoMensal`; a combinação com `lavrar_dias_pagos=True` pode escrever `DiaPago`. (`app/ponto/services/fechamento.py:1112`; `app/ponto/services/fechamento.py:1175`)

**Parte C(B) — sim, há caminho indireto de cron para competência anterior.** `detectar_par_relampago` está agendado com `--dias 2 --apply --retratar`; a busca retrocede dois dias e pode atravessar a virada da competência. (`app/config/crons.py:605`; `app/ponto/management/commands/detectar_par_relampago.py:83`; `app/ponto/management/commands/detectar_par_relampago.py:335`)

A cadeia é: comando chama `_retratar(alvo)` → `retratar_batida()` → `_fechamento_por_evento()` com o timestamp da batida → `recalcular_por_evento()` → `recalcular_fechamento_mes()` na competência **do fato**, após commit. (`app/ponto/management/commands/detectar_par_relampago.py:363`; `app/ponto/management/commands/detectar_par_relampago.py:224`; `app/ponto/registro_batida.py:281`; `app/ponto/registro_batida.py:178`; `app/ponto/services/fechamento.py:1793`; `app/ponto/services/fechamento.py:1794`; `app/ponto/services/fechamento.py:1808`)

O comando mensal `recalcular_fechamento` está declarado fora do pipeline; isso não elimina o caminho indireto acima. (`app/config/crons.py:767`; `app/config/crons.py:1107`) Ocorrência efetiva desse recálculo em produção: **[nao verificado]**.

**Parte C(C) — outros caminhos capazes de recalcular o passado, além do botão:**

| Caminho | Chamada e evidência |
|---|---|
| Registro e retratação de batida | `app/ponto/registro_batida.py:155`, `:160`, `:178`, `:281` |
| Decisão de HE referente ao dia informado | `app/ponto/portas/he.py:202` |
| Folga aceita e resposta validada | `app/chamados/services/validacao.py:331`, `:407` |
| Regeneração de células do vínculo, por competência tocada | `app/ponto/portas/celula.py:414`, `:422`, `:427` |
| Regeneração de exportada, chamada pelo passivo validado | `app/chamados/services/passivo_validado.py:429`, `:438`; `app/ponto/portas/regen_exportada.py:179` |
| Comando `recalcular_fechamento --mes --ano --apply` | `app/ponto/management/commands/recalcular_fechamento.py:78`, `:79`, `:110`, `:118` |
| Comando `aplicar_09_corte_b` | `app/ponto/management/commands/aplicar_09_corte_b.py:212` |
| Comando `diff_reclassificar_partido`, com `--aplicar` | `app/ponto/management/commands/diff_reclassificar_partido.py:206`, `:213`, `:232` |
| Comando `aplicar_janela_he_total`, delegando ao comando mensal | `app/ponto/management/commands/aplicar_janela_he_total.py:145` |
| Scripts para 10/2026, nos respectivos ramos de aplicação | `bin/aplicar_o263_comp10.py:26`, `:48`, `:51`; `bin/relavrar_o118_col221.py:19`, `:52`, `:53`; `bin/relavrar_o119_col221.py:18`, `:51`, `:52`; `bin/r5_idempotencia_frota.py:26`, `:117` |

Esses caminhos continuam sujeitos à guarda de 09/2026 no escritor. (`app/ponto/services/fechamento.py:551`) Já `fechamento_lido()` também recalcula competências solicitadas, mas usa `somente_leitura=True`, sem persistir `FechamentoMensal`. (`app/ponto/services/fechamento.py:1743`)

`bin/relavrar_o120_col221_09.py:75` passa `permitir_exportada=True`, argumento ausente da assinatura atual; essa chamada é incompatível com a cópia. (`app/ponto/services/fechamento.py:439`)

A execução efetiva desses caminhos sobre os registros do dado fornecido permanece **[nao verificado]**.

## 06/11 02:04 · sessao a2114b554fe1 · - · - · origem=Ronald (tmux)

C(A): **exportada em geral grava `FechamentoMensal`**, mas **09/2026 exportada é preservada pela guarda ativa**: competência declarada em `app/ponto/services/fechamento.py:36`; empresas exportadas detectadas em `:530`; colaboradores dessas empresas excluídos do universo em `:551–555`. Para os demais casos, `status='aberto'` usa `save()` em `:1121–1128`; outros status usam `update()` dos campos calculados em `:1144–1174`. `somente_leitura=True` não grava o fechamento (`:1112–1120`), embora a combinação com `lavrar_dias_pagos=True` possa gravar `DiaPago` (`:1175`).

C(C): além do botão, recalculam competências passadas pelos seguintes caminhos do código:

- **Eventos retroativos:** `recalcular_por_evento` determina a competência pela data do fato e chama o escritor (`app/ponto/services/fechamento.py:1793–1794`), após commit (`:1808`). Chamadores: batida (`app/ponto/registro_batida.py:155`, `:160`, `:178`); decisão HE (`app/ponto/portas/he.py:202`); folga aceita (`app/chamados/services/validacao.py:331`); resposta validada (`:407`); regeneração de vínculo, para cada competência tocada (`app/ponto/portas/celula.py:414–429`).
- **Retificação de exportada:** `regenerar_em_exportada` chama diretamente o escritor (`app/ponto/portas/regen_exportada.py:179`); o serviço de passivo validado chama essa porta quando houve plantio em exportada (`app/chamados/services/passivo_validado.py:429–439`).
- **Comando operacional:** `recalcular_fechamento --mes --ano --apply` (`app/ponto/management/commands/recalcular_fechamento.py:77–81`, `:110–120`).
- **Outros comandos que podem persistir:** `aplicar_09_corte_b.py:212`; `diff_reclassificar_partido.py:206` com commit no ramo `--aplicar` (`:213–214`, `:232–238`); `aplicar_janela_he_total.py:145` delega ao comando operacional. [nao verificado]
- **Scripts existentes:** `bin/aplicar_o263_comp10.py:26`, `:48–52`; `bin/relavrar_o118_col221.py:19`, `:52–53`; `bin/relavrar_o119_col221.py:18`, `:51–52`; `bin/r5_idempotencia_frota.py:26`, `:117`.

`bin/relavrar_o120_col221_09.py:75` passa `permitir_exportada=True`, argumento ausente da assinatura atual (`app/ponto/services/fechamento.py:439–442`); essa chamada está incompatível com a cópia.

Medidores com recálculo seguido de rollback: `desvio_o68b.py:93–95`, `folga_que_sumiu.py:69–71`, `plano_b_no_dinheiro.py:109–112`. `celula_veredito_velho.py` desliga recálculo no apply (`:95–99`); no ensaio, recalcula e reverte (`:146–157`).

Ocorrência efetiva desses caminhos em produção: **[não verificado]**.

## 06/11 02:04 · sessao 573d59eacfa9 · - · - · origem=Ronald (tmux)

**C(B): sim, existe caminho indireto de cron para recálculo de competência anterior.** O cron `detectar_par_relampago` roda com `--dias 2 --apply --retratar`; o recorte usa hoje menos dois dias, sem limitar pela competência corrente. Assim, na virada da janela, pode alcançar a anterior. (`app/config/crons.py:605`; `app/ponto/management/commands/detectar_par_relampago.py:335`; `app/ponto/management/commands/detectar_par_relampago.py:343`)

Cadeia executável:

- O comando chama `_retratar(alvo)` ou `_retratar` nas duas batidas do par. (`app/ponto/management/commands/detectar_par_relampago.py:363`; `app/ponto/management/commands/detectar_par_relampago.py:399`)
- `_retratar` chama `retratar_batida`. (`app/ponto/management/commands/detectar_par_relampago.py:224`)
- `retratar_batida` chama `_fechamento_por_evento` com o **timestamp da batida retratada**. (`app/ponto/registro_batida.py:281`)
- O helper chama `recalcular_por_evento`; este resolve a competência daquele fato e chama `recalcular_fechamento_mes`, após commit. (`app/ponto/registro_batida.py:177`; `app/ponto/registro_batida.py:178`; `app/ponto/services/fechamento.py:1793`; `app/ponto/services/fechamento.py:1794`; `app/ponto/services/fechamento.py:1808`)
- A competência é obtida pela data do fato, através de `competencia_do_fato` → `competencias_do_fato` → `competencias_do_intervalo`. (`app/ponto/services/fechamento.py:1588`; `app/ponto/services/fechamento.py:1578`; `app/ponto/services/fechamento.py:1583`)

A declaração de que `recalcular_fechamento` está fora do pipeline refere-se ao comando mensal direto; ela não elimina essa cadeia indireta. (`app/config/crons.py:767`; `app/config/crons.py:1107`)

O cron `pre_fechamento --apply` escreve pautas e apenas consulta os marcos da competência anterior; seu corpo não chama o recálculo mensal. (`app/config/crons.py:406`; `app/folha/management/commands/pre_fechamento.py:59`; `app/folha/management/commands/pre_fechamento.py:61`; `app/folha/management/commands/pre_fechamento.py:73`; `app/folha/management/commands/pre_fechamento.py:74`)

Ocorrência efetiva desse recálculo indireto em produção, frequência e colaboradores atingidos: **[não verificado]**.

## 06/11 02:04 · sessao 07f1d044bfe0 · - · - · origem=Ronald (tmux)

- `status='aprovada'` é o universo por status da folha: `_pelo_efeito(..., para='folha')` filtra aprovada; cobrança lê todas exceto rejeitada. `minutos>0` não integra esse carregador: `ausencias_do_periodo` e `cobertura_ausencia_periodo` leem interseção de datas, incluindo ranges abertos permitidos, sem filtro de minutos. Portanto aprovada + minutos>0 é recorte dos leitores, não seu universo integral. (`app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2529`, `app/ponto/turnos.py:2753`)

- Quem lê esse universo: fechamento carrega `ausencias_do_periodo(..., para='folha')`; efeitos da ausência e crédito parcial fazem a mesma chamada. (`app/ponto/services/fechamento.py:629`, `app/ponto/services/fechamento.py:630`, `app/ponto/services/efeito_ausencia.py:43`, `app/ponto/services/credito_parcial.py:23`)

- As três implementações de dinheiro usam **tipo pago e minutos não zero**, sem perguntar `cobre_parte_do_dia`: fechamento soma minutos e pula o efeito de dia inteiro; efeitos da ausência faz o mesmo; crédito parcial acumula por data. Isso difere do juiz, que exige tipo em parciais, registro de um só dia e `0 < minutos < 480`. Ressalva: o código usa verdade lógica de `minutos`, não comparação literal `>0`; o campo é `IntegerField`. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2580`, `app/ponto/turnos.py:2595`, `app/ponto/turnos.py:2598`, `app/ponto/models.py:755`)

- A tela do espelho e o PDF leem `efeitos_da_ausencia`; o fechamento também usa `creditos_por_dia`. (`app/ponto/services/espelho.py:1048`, `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:332`, `app/relatorios/pdf_espelho.py:333`, `app/ponto/services/fechamento.py:978`, `app/ponto/services/fechamento.py:983`)

- Exportação tem outro leitor: `ausencias_na_janela` filtra aprovadas, tipos pedidos e interseção da janela, sem filtro de minutos; `minutos_de_falta_por_dia` usa tipos que descontam e rateia minutos pelos dias. (`app/folha/export.py:87`, `app/folha/export.py:88`, `app/folha/export.py:90`, `app/folha/export.py:138`, `app/folha/export.py:140`, `app/folha/export.py:141`)

- Não localizei `sonda_h12h06b.py` nem com `rg --files -uuu`; ausência do script impede conferir as chamadas que ele efetivamente fez ou identificar reconstruções dentro dele. [não verificado]

## 06/11 01:57 · sessao 43ff11e3f048 · H12H06-B · verificador · origem=Code

**Alta — a cura de #2 não fecha.** `mudar_estado` grava status, minutos e range sem mover `atualizado_em`; depois tenta registrar a trilha. Essa tentativa pode falhar sem desfazer a ausência, e `registrar_log` não verifica o retorno. (`app/ponto/services/ausencia.py:818`, `app/ponto/services/ausencia.py:820`, `app/core/auditoria.py:22`, `app/core/observ.py:105`, `app/core/observ.py:139`)

Contraexemplo: depois do apply, rejeitar e reabrir um atestado, com falha nas duas trilhas, devolve status/minutos/identidade/carimbo aos valores esperados. A reversão não encontra log posterior e restaura 720 sobre o ato posterior. Há também uma janela sem falha definitiva: duas chamadas podem concluir os saves da ida/volta e ficar suspensas antes dos respectivos logs. (`app/ponto/services/ausencia.py:819`, `app/ponto/services/ausencia.py:1125`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:154`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`) Ocorrência em produção: [não verificado].

**Média — #3 fecha o relato da linha, mas permanece parcialmente aberto para recuperação.** O resultado recebe a linha antes da confirmação, conforme pretendido. Porém, se uma falha de escrita deixar a confirmação final truncada, o leitor executa `json.loads` sobre todas as linhas e aborta antes de montar qualquer reversão, inclusive das linhas anteriores corretamente confirmadas. Falta tolerância ao fragmento final do diário. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:270`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:274`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:254`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:222`) Reprodução dessa falha: [não verificado].

**Alta — contesto o descarte categórico da corrida com exportação.** O comando trava somente a ausência, consulta a cerca e depois escreve. A consulta de competência entregue não estabelece trava compartilhada com o exportador, que registra `ExportacaoDominio` independentemente. Portanto, um export concorrente pode entregar a competência entre a conferência e a alteração. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:160`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`, `app/folha/export.py:581`, `app/folha/models.py:182`, `app/folha/views.py:297`) Ocorrência e fundamento do descarte anterior: [não verificado].

Quanto às demais curas e hipóteses:

- **#1 fecha o caso descrito:** o objeto é recarregado e o carimbo retornado dentro da transação que mantém a trava. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:173`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:189`)
- **PK repetido:** a confirmação consome a última foto pendente daquele PK; não reutiliza indiscriminadamente todas as fotos anteriores. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:233`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:235`)
- **PK de log versus commit:** o filtro compara um identificador automático, não uma sequência de commits. Não identifiquei escritor concreto com log menor e alteração posterior da mesma ausência que atravesse todas as demais guardas; esse contraexemplo permanece [não verificado]. (`app/core/migrations/0002_log_auditoria.py:19`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`)
- **Trilha própria da reversão:** ela também grava evento e confirma seu próprio PK; o filtro estritamente `pk__gt` não conta essa mesma trilha como posterior. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:274`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`)
- **Push do apply desfeito:** existe caminho síncrono entre save, julgamento e envio antes das verificações que podem provocar rollback. Entretanto, alcançar esse envio nas 30 linhas depende de condições não fornecidas; não o promovo a achado específico desta obra. (`app/ponto/services/ausencia.py:508`, `app/ponto/signals.py:147`, `app/ponto/services/cartorio.py:1740`, `app/ponto/management/commands/detectar_ausencias.py:778`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:175`) Alcance nas 30: [não verificado].

**Quem escreve LogAuditoria com `modelo='Ausencia'`:**

| Origem | Arquivo:linha |
|---|---|
| Criação, estado, documento e período | `app/ponto/services/ausencia.py:441`, `:820`, `:854`, `:915` |
| Cancelamento em lote, aprovação apesar do anexo, cancelamento individual e edição | `app/ponto/views.py:2357`, `:2614`, `:2739`, `:2808` |
| Lançamento pela IA | `app/core/views_ia.py:991` |
| Devolução de perguntas absorvidas | `app/chamados/services/materializacao.py:663` |
| Cura do fim | `app/ponto/management/commands/curar_ausencia_sem_fim.py:47` |
| Apply e reversão desta obra | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:182` |
| Migrações históricas, diretamente | `app/ponto/migrations/0057_a4_backfill_pendente.py:44`, `app/ponto/migrations/0058_a4_relogio_recomeca.py:43`, `app/ponto/migrations/0059_a4_d0_no_primeiro_dia_do_cron.py:37` |

O escritor físico atual é `evento`; `registrar_log` é adaptador. (`app/core/observ.py:134`, `app/core/auditoria.py:22`)

**Quem escreve Ausencia sem trilha própria:**

- `editar_ausencia_registro` e `rederivar_fim` fazem save completo; os chamadores identificados registram a trilha. (`app/ponto/services/ausencia.py:508`, `app/ponto/services/ausencia.py:1280`, `app/ponto/views.py:2808`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`)
- A rejeição de férias não registra trilha da ausência, mas move explicitamente `atualizado_em`. (`app/ponto/services/ausencia.py:1267`)
- Há exclusões sem trilha própria; a reversão detecta que a linha desapareceu. (`app/ponto/services/ausencia.py:1174`, `app/ponto/services/ausencia.py:1197`, `app/ponto/services/ausencia.py:1254`, `app/ponto/services/ausencia.py:1274`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:140`)
- Há saves de metadados sem carimbo/log próprio: prazo; decisão sem mudança de estado e sem período; documento/descrição/prazo na reabertura. Eles não demonstram, isoladamente, alteração invisível de minutos/identidade/status. (`app/ponto/services/ausencia.py:536`, `app/ponto/services/ausencia.py:911`, `app/ponto/services/ausencia.py:1114`, `app/ponto/services/ausencia.py:1124`, `app/ponto/services/ausencia.py:1132`)

**Modelo/objeto diferentes:** o cancelamento de férias altera ausências e registra o ato em `AgendamentoFerias`, com o PK do agendamento; a alteração das ausências carimba `atualizado_em`. O aviso de recusa registra `Colaborador`, mas não altera uma ausência. Nenhum desses caminhos fornece o contraexemplo invisível procurado. (`app/ferias/services.py:356`, `app/ferias/views.py:410`, `app/ponto/services/ausencia.py:1267`, `app/api/views.py:1937`)

_2088 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
