# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 17:21** · publicado em **29/10/2026 17:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 17:21 · sessao f3ab77b5af6d · grupo1 · verificador · origem=Code

- **Classe 7 — contrato alterado antes de migrar o leitor.** `app/ponto/management/commands/e6_oraculo.py:118` torna `agora` obrigatório em `InsumosDoDono`, mas `app/ponto/management/commands/pautas_discordantes.py:82` continua chamando sem esse argumento. Quando essa chamada é alcançada, ocorre `TypeError`, interrompendo a apuração antes da captura de exceções (`app/ponto/management/commands/pautas_discordantes.py:96`).

Total por classe: 1=0; 2=0; 3=0; 4=0; 5=0; 6=0; 7=1.

## 29/10 17:20 · sessao e01d04a5d1b8 · - · - · origem=Ronald (tmux)

- Classe 7: `app/ponto/management/commands/e6_oraculo.py:118` exige `agora` em `InsumosDoDono`, mas `app/ponto/management/commands/pautas_discordantes.py:82` chama sem esse argumento. Havendo colaborador no universo, `apurar` levanta `TypeError` antes de julgar/sincronizar; a captura de exceção começa apenas em `app/ponto/management/commands/pautas_discordantes.py:96`.

Censo AST dos contratos alterados encontrou somente essa chamada de produção sem `agora`; reprodução isolada da assinatura em memória confirmou o `TypeError`. O selo limita seus alvos aos três builders em `app/escala/tests/test_selo_grade_pede_o_instante.py:21`, portanto não cobre esse contrato intermediário.

## 29/10 17:20 · sessao 3beb801728fe · - · - · origem=Ronald (tmux)

Nenhum achado verificável nas classes 3, 6 ou 7.

O encadeamento com duas bordas alteradas preserva um único instante: `app/folha/porta_export.py:318` captura o relógio e `app/folha/porta_export.py:667` o repassa ao comando; `app/relatorios/management/commands/cartao_x_txt_divergentes.py:30` só lê o relógio quando esse argumento falta.

As outras coexistências encontradas envolvem leituras preexistentes, excluídas expressamente pelo escopo: `app/ponto/management/commands/reconciliar_grade.py:75`, `app/ponto/management/commands/mapa_divergencia.py:36`, `app/ponto/management/commands/detectar_par_relampago.py:328`, `app/ponto/services/teto_na_lista.py:50` e `app/folha/porta_export.py:318`.

## 29/10 17:20 · sessao 0e3be25a71fe · - · - · origem=Ronald (tmux)

Nenhum achado verificável de classe 5 na revisão:

- A regressão `turno=None` é distinguida pelo caso col923, que exige `('grave', '07:30', 119.0)`: app/chamados/tests/test_detector_pergunta_ao_juiz_do_par.py:264.
- A retirada da condição de dia encerrado é distinguida pelas expectativas diferentes à meia-noite e às 07:30: app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:408 e app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:413.
- O selo verifica origem do argumento `turno`: app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:455.
- A reversão da linha do cartório, removendo `agora=agora`, é detectada pelo selo AST; confirmado em memória, sem modificar arquivos: app/ponto/services/cartorio.py:874; app/escala/tests/test_selo_grade_pede_o_instante.py:39.

## 29/10 17:18 · sessao dc32db77ba24 · grupo1 · verificador · origem=Code

- **Classe 3 — duas leituras novas de relógio no mesmo ato:** `medir()` captura `now()` em `app/folha/porta_export.py:318`, mas chama `cartao_x_txt_divergentes` sem repassar esse instante (`app/folha/porta_export.py:667`). O comando captura outro `now()` (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:26`) e o usa na coleta (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:59`). As duas leituras foram criadas pelo diff.

Total por classe: 1=0; 2=0; 3=1; 4=0; 5=0; 6=0; 7=0.

## 29/10 17:17 · sessao 8aa190a14236 · - · - · origem=Ronald (tmux)

- **Classe 3:** `app/folha/porta_export.py:318` cria uma leitura de relógio em `medir`; o mesmo ato chama `cartao_x_txt_divergentes` sem transmitir o instante (`app/folha/porta_export.py:667`), e esse comando cria outra leitura (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:26`). Os leitores recebem instantes distintos: `app/folha/porta_export.py:476`, `app/folha/porta_export.py:496` e `app/relatorios/management/commands/cartao_x_txt_divergentes.py:59`. **Ambas as leituras foram adicionadas pelo diff fornecido.**

Total verificável desta auditoria: classe 3 = 1.

## 29/10 17:17 · sessao 5708d1a7d527 · - · - · origem=Ronald (tmux)

Sem achado verificável da classe 5: restaurar DP no detector contraria a asserção de `app/chamados/tests/test_detector_pergunta_ao_juiz_do_par.py:270`; retirar a trava temporal contraria `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:407`; tornar `agora` opcional contraria `app/escala/tests/test_selo_grade_pede_o_instante.py:78`.

## 29/10 17:09 · sessao de55e477697d · justif · verificador · origem=Code

**Reprovaria esta versão:** ainda há um caminho que conserva a justificativa aprovada e desfaz somente o fechamento do chamado. A nova costura entra nesse caminho pelos signals do `save()`. (`app/ponto/services/justificativa.py:159`; `app/chamados/models.py:226`; `app/chamados/signals.py:83`)

Os contraexemplos abaixo decorrem da leitura do código; reprodução em PostgreSQL e ocorrência em produção: **[não verificado]**.

1. **Defeitos encontrados**

   - **D1 — Meio-estado por erro de banco engolido no signal. Classe: outra — atomicidade.** A aprovação é salva antes do fechamento; o núcleo do chamado abre outro savepoint, contendo chamado e trilha. O receiver `resolve_perguntas_ao_resolver` captura exceções sem savepoint próprio, embora seu resolvedor execute `p.save()`. (`app/ponto/services/justificativa.py:150`; `app/chamados/models.py:226`; `app/chamados/signals.py:83`; `app/chamados/services/disputa_emissao.py:1583`)

     **Entrada → resultado errado:** J pendente ligada nos dois sentidos a C vivo; P destinada a C, disputa aberta, `validada_em` preenchido, `materializada_em=NULL`, resposta vazia e dia não trancado. P entra no resolvedor e é considerada morta; um erro SQL no UPDATE de P é engolido pelo receiver. O savepoint do núcleo pode então desfazer **C e sua trilha**, preservando a aprovação anterior de J; `reconciliar()` retorna `True`. Resultado: **J aprovada, C vivo, zero trilha de fechamento**. (`app/chamados/services/disputa_emissao.py:1536`; `app/chamados/juizes.py:1005`; `app/chamados/services/disputa_emissao.py:1583`; `app/chamados/signals.py:87`; `app/chamados/models.py:226`; `app/chamados/models.py:445`)

     Não há uma propagação obrigatória posterior que salve esse caso: o cartório também captura suas consultas à conexão abortada; o receiver de isentos retorna pelo estado encerrado da instância; a relavra posterior exclui P já validada. O mecanismo de rollback silencioso por erro engolido já está documentado na própria casa. (`app/chamados/signals.py:166`; `app/chamados/signals.py:224`; `app/chamados/models.py:246`; `app/core/observ.py:124`)

   - **D2 — Pré-trava menor que a cascata efetivamente executada. Classe: ordem de trava.** `_cascata_do_turno` cobre quatro módulos daquele dia, mas a criação da batida dispara sincronamente o reconciliador de **todos os chamados vivos do colaborador**. Portanto, ordenar os IDs dessa pré-trava não estabelece uma ordem global. (`app/ponto/services/justificativa.py:43`; `app/ponto/services/justificativa.py:72`; `app/ponto/registro_batida.py:157`; `app/ponto/signals.py:206`; `app/chamados/reconciliador.py:1203`)

     **Entrada → resultado errado:** mesmo colaborador, J1 de 07/10→C1 e J2 de 17/10→C2, C1<C2; chamados vivos com turnos já encerrados e premissas regularizadas, justificativas pendentes sem batida gerada. T1 segura C1/J1; T2 segura C2/J2. Ao registrar as saídas, T2 tenta C1; T1 percorre o reconciliador e tenta C2. Forma-se **C1→C2 contra C2→C1**, podendo abortar uma aprovação por deadlock. Os UPDATEs dos chamados permanecem sob os atomics externos. (`app/ponto/services/justificativa.py:85`; `app/ponto/services/justificativa.py:288`; `app/chamados/reconciliador.py:1205`; `app/chamados/reconciliador.py:1214`; `app/chamados/reconciliador.py:455`; `app/chamados/models.py:231`)

   - **D3 — Origem lida antes da trava continua mutável. Classe: ordem de trava.** A pré-leitura de `chamado_origem_id` ocorre antes de travar J; há caminhos reais que preenchem essa origem em justificativa existente. A docstring reconhece a janela, mas não a elimina. (`app/ponto/services/justificativa.py:66`; `app/ponto/services/justificativa.py:86`; `app/ponto/views.py:1631`; `app/api/views.py:2055`; `app/ponto/services/justificativa.py:61`)

     **Entrada → resultado errado:** T1 lê J sem origem; uma resposta concorrente grava origem C; T1 trava J; o reconciliador segura C e espera atualizar J; T1 chega ao helper e espera C. Retorna o ciclo **J→C contra C→J**, com possível aborto da aprovação. (`app/ponto/views.py:1633`; `app/ponto/services/justificativa.py:32`; `app/chamados/reconciliador.py:455`; `app/chamados/reconciliador.py:468`)

   - **D4 — Savepoint de invalidação começa depois de uma possível consulta SQL. Classe: atomic mal posicionado.** `competencias_do_fato()` executa antes do novo `atomic` e acessa a relação potencialmente preguiçosa `colaborador.empresa`. Erro nessa consulta continua sendo capturado fora de qualquer savepoint restaurador local. (`app/ponto/services/fechamento.py:1609`; `app/ponto/services/fechamento.py:1567`; `app/ponto/services/fechamento.py:1615`; `app/ponto/services/fechamento.py:1626`)

     **Entrada → resultado errado:** J avulsa, sem saída, sem célula e com empresa não carregada; o signal retorna sem carregar a empresa; o SELECT da empresa falha dentro da transação da aprovação. `invalidar_fechamento` devolve 0 e o helper retorna cedo por ausência de origem, admitindo novamente aprovação desfeita com resposta de sucesso. Com origem, a consulta seguinte pode revelar a transação abortada e derrubar o ato. Execução desses resultados: **[não verificado]**. (`app/ponto/services/cartorio.py:1547`; `app/ponto/services/fechamento.py:1630`; `app/ponto/services/justificativa.py:29`; `app/ponto/services/justificativa.py:32`; `app/ponto/services/justificativa.py:209`)

   - **D5 — O selo aceita um save que não persiste a aprovação. Classe: selo/RED sem mordida.** `_saves` aceita qualquer receptor e quaisquer `update_fields`; `_chama` exige somente uma chamada posterior ao primeiro save encontrado. Os dois mutantes abaixo foram aceitos pela análise AST em memória. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:63`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:76`)

     ```python
     with tx.atomic():
         j.status = 'aprovada'
         outro.save()  # também passa: j.save(update_fields=['descricao'])
         encerrar_chamado_de_origem(j)
         j.save()
     ```

     **Entrada → resultado errado:** J ainda pendente no banco e dia furado; o fechamento consulta essa pendência, pode recusar, e o último save aprova deixando a origem viva. O selo permanece verde. As chamadas atuais estão depois dos saves corretos; o defeito é a garantia anunciada pelo selo. (`app/chamados/models.py:937`; `app/chamados/models.py:495`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:77`; `app/ponto/services/justificativa.py:150`; `app/ponto/services/justificativa.py:329`; `app/ponto/views.py:1798`)

   - **D6 — Função interna pode esconder escritor externo do selo. Classe: selo/RED sem mordida.** Se uma função externa aprova sem fechamento, mas contém outra função que também aprova, o `continue` descarta a externa inteira. Contraexemplo em módulo novo: função externa salva J aprovada sem costura; função interna, nunca chamada, tem uma aprovação corretamente costurada. O censo aceita ambas, embora a execução externa deixe C vivo; o teste das três portas examina somente dois arquivos. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:88`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:125`)

2. **Escritores não alcançados**

   **Sim: o admin genérico de Justificativa permanece editável**, com `status='aprovada'` disponível e sem passar pelo novo helper. É caminho preexistente, **FORA-DO-ESCOPO** como alteração proposta; uso efetivo em produção: **[não verificado]**. (`app/ponto/admin.py:18`; `app/ponto/models.py:132`; `app/ponto/models.py:159`; `app/config/urls.py:24`)

   Não encontrei outro escritor explícito de aprovação além dos três alcançados. No universo AST do selo de `status_local`, não encontrei escrita fora das portas; o helper chama `reconciliar`, que escreve pelo núcleo. Isso é uma conclusão limitada às formas reconhecidas pelo censo. (`app/ponto/services/justificativa.py:147`; `app/ponto/services/justificativa.py:326`; `app/ponto/views.py:1795`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:317`; `app/chamados/models.py:444`)

   Também não identifiquei novo juiz de fechamento: o helper delega à porta e ao coletor existente. A lista da cascata foi centralizada, embora suas duas avaliações não congelem o mesmo conjunto de linhas. (`app/ponto/services/justificativa.py:35`; `app/chamados/models.py:495`; `app/ponto/services/justificativa.py:72`; `app/ponto/services/justificativa.py:337`)

3. **Mordida dos testes** [nao verificado]

   Por leitura, os positivos de aprovação e o teste da materialização perderiam o fechamento esperado sem a costura. A materialização usa um wrapper excluído da cascata e fora do catálogo do reconciliador, evitando que esses caminhos alternativos satisfaçam sua assertiva. Execução Django: **[não verificado]**. (`app/ponto/tests/test_justificativa_encerra_origem.py:72`; `app/ponto/tests/test_justificativa_encerra_origem.py:84`; `app/ponto/tests/test_justificativa_encerra_origem.py:211`; `app/ponto/tests/test_justificativa_encerra_origem.py:229`; `app/ponto/services/justificativa.py:43`; `app/chamados/catalogo/premissa.py:123`)

   **A vacuidade anterior dos rollbacks foi corrigida:** ambos exigem `assert_called_once()`. O negativo da rejeição confirma que houve rejeição; o de ponteiro distinto confirma aprovação e trilha de recusa. (`app/ponto/tests/test_justificativa_encerra_origem.py:170`; `app/ponto/tests/test_justificativa_encerra_origem.py:197`; `app/ponto/tests/test_justificativa_encerra_origem.py:136`; `app/ponto/tests/test_justificativa_encerra_origem.py:157`; `app/ponto/tests/test_justificativa_encerra_origem.py:159`)

   Os controles sem origem e de rejeição passam sem a cura por desenho. Já o teste de universo também passa sem a costura, pois conta atribuições; os defeitos D5/D6 permitem verde estrutural indevido. Os mocks `RuntimeError` não exercitam o erro SQL engolido de D1, e o positivo simples pode passar sem os efeitos dos signals porque a justificativa aprovada basta ao coletor. (`app/ponto/tests/test_justificativa_encerra_origem.py:129`; `app/ponto/tests/test_justificativa_encerra_origem.py:176`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:128`; `app/ponto/tests/test_justificativa_encerra_origem.py:168`; `app/chamados/models.py:937`)

4. **Ordem das travas**

   A inversão direta foi tratada para uma origem estável: as três aprovações agora pré-travam C antes de J, compatível com o reconciliador, que escreve C e depois a justificativa pendente. **A garantia completa continua falsa pelos D2/D3**, inclusive com `cascata=True`. (`app/ponto/services/justificativa.py:85`; `app/ponto/services/justificativa.py:288`; `app/ponto/views.py:1792`; `app/chamados/reconciliador.py:456`; `app/chamados/reconciliador.py:468`)

5. **Signals dentro da aprovação e alcance da invalidação**

   Uma exceção que escape do fechamento atravessa o atomic da aprovação e desfaz o ato; porém, **capturar uma exceção SQL não garante essa atomicidade**, como mostra D1. O rebaixamento não captura exceções, mas normalmente já foi realizado pelo núcleo e seu receiver não precisa escrever novamente. (`app/ponto/services/justificativa.py:160`; `app/chamados/signals.py:72`; `app/chamados/models.py:210`; `app/chamados/models.py:278`; `app/chamados/models.py:424`)

   Não identifiquei um contraexemplo causado exclusivamente pela leitura das próprias escritas ainda não commitadas. O problema demonstrável é o recorte dos savepoints; a gravação de célula e seu evento possuem atomic próprio. Ausência universal de outros problemas nessa integração: **[não verificado]**. (`app/ponto/portas/celula.py:639`; `app/chamados/signals.py:166`; `app/chamados/models.py:226`)

   O novo savepoint protege os UPDATEs de `FechamentoMensal`, mas também alcança os consumidores de ausência e regeneração de template, **FORA-DO-ESCOPO** como fluxos de negócio. Agora, falha na segunda competência desfaz também a invalidação da primeira; a função devolve 0. Essa mudança amplia a unidade de rollback do carimbo e não resolve D4. (`app/ponto/services/fechamento.py:1615`; `app/ponto/services/fechamento.py:1621`; `app/ponto/services/fechamento.py:1630`; `app/ponto/services/ausencia.py:930`; `app/escala/services/cadastro_tipo.py:358`)

## 29/10 17:07 · sessao 8816b60bd46e · - · - · origem=Ronald (tmux)

Dois achados relevantes para a obra:

1. **O savepoint de `invalidar_fechamento` não cobre toda consulta ao banco.** `competencias_do_fato()` roda antes dele (`app/ponto/services/fechamento.py:1609`, `:1615`) e acessa `colaborador.empresa` (`:1567`). Contraexemplo: J pendente avulsa, empresa não cacheada, sem célula e sem saída; o signal de J retorna por ausência de célula (`app/ponto/signals.py:165`; `app/ponto/services/cartorio.py:1547-1549`), então a leitura de Empresa acontece fora do savepoint. Erro de banco nessa leitura é engolido (`app/ponto/services/fechamento.py:1626-1630`), deixando a transação externa abortada; como a origem é nula, o helper retorna cedo (`app/ponto/services/justificativa.py:29-30`). Continua possível desfazer a aprovação silenciosamente e anunciar sucesso. O mecanismo de rollback silencioso por erro engolido está descrito também em `app/core/observ.py:124-132`; ocorrência real neste caso: **[não verificado]**. Classe: **ordem de trava / atomic mal posicionado**.

2. **Um erro de banco engolido pelo signal pode desfazer apenas o fechamento e conservar a aprovação.** J é salva antes do helper (`app/ponto/services/justificativa.py:147-159`; lote em `app/ponto/views.py:1795-1829`); `_gravar_estado` cria um savepoint interno envolvendo trilha e chamado (`app/chamados/models.py:226-231`). `resolve_perguntas_ao_resolver` engole exceções sem savepoint próprio (`app/chamados/signals.py:83-88`), mas o resolvedor executa `p.save()` (`app/chamados/services/disputa_emissao.py:1583`).

   Contraexemplo concreto: C vivo ligado à J; P aponta para C, pertence a disputa aberta, tem `validada_em` preenchido, `materializada_em=NULL`, resposta vazia e dia não trancado. P entra no filtro (`app/chamados/services/disputa_emissao.py:1536-1540`) e está morta pela regra (`app/chamados/juizes.py:1005-1007`). Um `statement_timeout` no UPDATE de P marca rollback; o receiver engole. O cartório seguinte também engole suas consultas à conexão quebrada (`app/chamados/signals.py:166-174`), e o receiver de isentos retorna pelo estado encerrado da instância (`:224-225`). O savepoint interno desfaz C e trilha, enquanto a relavra posterior exclui P já validada (`app/chamados/models.py:244-249`). `reconciliar()` retorna `True` (`:444-445`): **J aprovada, C vivo, nenhuma trilha de fechamento**. Classe: **outra — falha de atomicidade na nova integração**. Reprodução executada: **[não verificado]**.

Os cinco chamadores diretos de `invalidar_fechamento` são as duas portas de J (`app/ponto/services/justificativa.py:157`, `:333`), o lote (`app/ponto/views.py:1804`), a decisão de ausência (`app/ponto/services/ausencia.py:930`) e a regeneração de template (`app/escala/services/cadastro_tipo.py:358`). Os dois últimos são **FORA-DO-ESCOPO** como fluxos de negócio, mas recebem a mudança compartilhada do savepoint (`app/ponto/services/fechamento.py:1615`).

Sobre os três efeitos pedidos:

- **Rebaixa:** não captura exceções (`app/chamados/signals.py:72`); no caminho normal desta porta, porém, `_gravar_estado` já rebaixou a urgência (`app/chamados/models.py:210`), então `_rebaixar_sem_post_save` retorna sem UPDATE (`:424-426`; ritual em `:278-283`). Falha propagada pode desfazer a aprovação pelo `except` externo (`app/ponto/services/justificativa.py:160-164`).
- **Resolve perguntas:** captura exceções, mas possui a brecha transacional descrita acima (`app/chamados/signals.py:83-88`; `app/chamados/services/disputa_emissao.py:1583`).
- **Cartório:** usa `engolir`, que apenas captura e registra (`app/chamados/signals.py:166-174`; `app/core/observ.py:85-91`). O save e evento da célula possuem atomic próprio (`app/ponto/portas/celula.py:639-642`); isso não protege automaticamente cada outra operação do julgamento (`app/ponto/services/cartorio.py:739-770`).

Não identifiquei um contraexemplo sustentado apenas por “ler estado ainda não commitado”; os problemas concretos encontrados decorrem dos limites dos savepoints e das exceções engolidas, nos sítios acima. Uma conclusão universal de ausência de leituras erradas permanece **[não verificado]**. [nao verificado]

O teste novo de rollback injeta `RuntimeError` diretamente em `reconciliar`; agora exige que a chamada ocorra, corrigindo a vacuidade anterior, mas não exercita o erro de banco engolido dentro do signal (`app/ponto/tests/test_justificativa_encerra_origem.py:164-174`).

## 29/10 17:07 · sessao 1b1e2c389147 · - · - · origem=Ronald (tmux)

Achados de travas e transação, todos por inspeção estática:

1. **A ordem global de travas continua invertível pela cascata implícita dos signals — classe “ordem de trava”.** A pré-trava cobre a origem e, na materialização, somente quatro módulos daquele dia; depois trava J (`app/ponto/services/justificativa.py:43`, `:66`, `:72`, `:74`, `:85`, `:86`, `:288`, `:289`). Contudo, criar a saída dispara sincronamente o reconciliador de **todos** os chamados vivos do colaborador, ordenados por pk (`app/ponto/registro_batida.py:157`; `app/ponto/signals.py:206`; `app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1203`; `:1206`; `:1214`).

   Contraexemplo: mesmo colaborador, J1 de 07/10→C1 e J2 de 17/10→C2, C1<C2, ambos vivos embora seus turnos já estejam encerrados e a premissa regularizada; ambas J pendentes, sem saída gerada. T1 pré-trava C1/J1; T2 pré-trava C2/J2. T2 chega ao reconciliador e espera C1; T1 fecha C1 e depois espera C2. O UPDATE de C ocorre dentro do atomic do reconciliador, com locks retidos até terminar o atomic externo (`app/chamados/reconciliador.py:455`; `:456`; `app/chamados/models.py:226`; `:231`). O filtro de pré-trava por dia não alcança o outro chamado (`app/ponto/services/justificativa.py:45`). Uma aprovação pode abortar por deadlock; reprodução em PostgreSQL **[não verificado]**.

2. **O savepoint não cobre a preparação que também pode consultar o banco — classe “atomic mal posicionado”.** `competencias_do_fato` é chamado antes do novo atomic; ele acessa `colaborador.empresa`, que pode ser uma relação ainda não carregada (`app/ponto/services/fechamento.py:1609`; `:1615`; `:1567`). Contraexemplo: colaborador com empresa fora do cache e erro SQL no SELECT dessa relação. O except devolve 0 sem restaurar a transação externa; o fechamento da origem faz outra consulta e a aprovação pode voltar inteira (`app/ponto/services/fechamento.py:1626`; `:1630`; `app/ponto/services/justificativa.py:159`; `:32`; `:160`). O savepoint resolve os erros nos UPDATEs de FechamentoMensal, mas não esse caminho anterior (`app/ponto/services/fechamento.py:1615`; `:1621`). Injeção/reprodução **[não verificado]**.

3. **A leitura da origem continua sem estabilidade até travar J — limitação explicitamente reconhecida pelo código.** A origem é consultada antes da trava de J; os formulários de resposta podem atribuí-la e salvar J nesse intervalo (`app/ponto/services/justificativa.py:66`; `:74`; `:86`; `app/ponto/views.py:1631`; `:1633`; `app/api/views.py:2055`; `:2057`). Portanto ainda existe a sequência leitura sem origem→atribuição concorrente→trava de J→trava tardia de C, contra C→J do reconciliador (`app/ponto/services/justificativa.py:32`; `app/chamados/reconciliador.py:455`; `:468`). A docstring declara esse limite, sem eliminá-lo (`app/ponto/services/justificativa.py:61`).

Consequência nova do savepoint para outros consumidores: um único atomic engloba todas as competências; falha na segunda desfaz também a invalidação bem-sucedida da primeira, e a função retorna 0 (`app/ponto/services/fechamento.py:1615`; `:1621`; `:1626`; `:1630`). Isso alcança ausência com intervalo indeterminado e regeneração de escala (`app/ponto/services/ausencia.py:930`; `:931`; `app/escala/services/cadastro_tipo.py:358`). Em autocommit, o comportamento anterior preservava os primeiros UPDATEs bem-sucedidos; agora pode ampliar o conjunto que permanece carimbado de fresco após uma falha **[não verificado quanto à execução]**. Como a própria função já promete operação best effort, considero consequência a registrar, não defeito bloqueante independente (`app/ponto/services/fechamento.py:1602`).

## 29/10 17:07 · sessao bba42898d7e2 · - · - · origem=Ronald (tmux)

Achado confirmado: **selo ainda aceita “pouso antes do pré-requisito” com outro `.save()`**. `_saves` aceita qualquer receptor e não verifica `update_fields`; `_chama` usa o primeiro save após a atribuição, sem ligá-lo a `j.status` (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:63`, `:76`). Contraexemplos avaliados em memória, ambos retornando `violacoes_em=[]`:

```python
with tx.atomic():
    j.status = 'aprovada'
    outro.save()
    encerrar_chamado_de_origem(j)
    j.save()
```

```python
with tx.atomic():
    j.status = 'aprovada'
    j.save(update_fields=['descricao'])
    encerrar_chamado_de_origem(j)
    j.save()
```

Nesses mutantes, o gate lê a justificativa persistida ainda pendente; pode recusar o fechamento, e o último save aprova deixando a origem viva (`app/chamados/models.py:937`; selo `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:76`). Classe: **selo/RED sem mordida**; não é posição errada das chamadas atuais, que estão depois dos saves corretos (`app/ponto/services/justificativa.py:150`, `:159`, `:329`, `:348`; `app/ponto/views.py:1798`, `:1829`).

Outra lacuna reproduzida: função externa que aprova sem fechamento é ignorada se contiver função interna que também aprova corretamente; o `continue` elimina a externa inteira (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:88`). Classificar junto à fragilidade do selo, evitando tratar como defeito runtime atual.

Censo só de leitura: executei as funções AST dos selos com `python3 -B`, retirando imports Django e classes de teste apenas em memória. O selo de status percorreu 739 arquivos e retornou zero violações; o de aprovada retornou somente a entrada nominal `ferias/services.py::aprovar_solicitacao`, outro modelo (`app/chamados/tests/test_selo_status_so_pelas_portas.py:317`, `:322`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:25`). Não encontrei escritor explícito de `Justificativa.status='aprovada'` além das três portas alcançadas (`app/ponto/services/justificativa.py:147`, `:326`; `app/ponto/views.py:1795`).

**Admin genérico continua sendo escritor não alcançado, preexistente/FORA-DO-ESCOPO**: `JustificativaAdmin` herda diretamente `ModelAdmin`, sem restrição de edição; `status` é campo com choice `aprovada`; a rota admin existe (`app/ponto/admin.py:18`; `app/ponto/models.py:130`, `:159`; `app/config/urls.py:24`). Uso efetivo em produção: **[não verificado]**. O próprio selo declara essa exclusão (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`).

Quanto à mordida dos funcionais:

- Os dois rollbacks agora provam chegada ao ponto de falha com `m.assert_called_once()`, eliminando a passagem por retorno cedo descrita na leitura anterior (`app/ponto/tests/test_justificativa_encerra_origem.py:168`, `:170`, `:195`, `:197`).
- O negativo da rejeição verifica que a decisão aconteceu; o de ponteiro distinto verifica aprovação e trilha da recusa. Portanto não dependem apenas de ausência de fechamento (`app/ponto/tests/test_justificativa_encerra_origem.py:136`, `:157`, `:159`).
- `test_MATERIALIZAR_encerra_a_origem_de_outro_modulo` tem mordida por leitura: sua origem é `justificativa_colab`, excluída da cascata e ausente das classes do reconciliador; o catálogo devolve fora de escopo e o consumidor pula (`app/ponto/tests/test_justificativa_encerra_origem.py:211`; `app/ponto/services/justificativa.py:43`; `app/chamados/catalogo/premissa.py:45`, `:123`; `app/chamados/reconciliador.py:257`, `:1208`). O setup não cria célula/escala, e o cartório retorna sem célula; retirando a costura da materialização, a assertiva de origem resolvida falharia (`app/ponto/tests/test_justificativa_encerra_origem.py:40`, `:211`, `:229`; `app/ponto/services/cartorio.py:1547`; `app/ponto/services/justificativa.py:348`).
- `test_sem_chamado_de_origem_aprova_como_sempre` e `test_MORDE_rejeitar_NAO_encerra` passam sem a cura por serem controles; isso não é passagem vacuosa (`app/ponto/tests/test_justificativa_encerra_origem.py:129`, `:176`).
- Não executei testes Django/banco; os testes novos usam `TestCase` e mocks `RuntimeError`, não demonstram concorrência ou recuperação de erro real de banco no savepoint (`app/ponto/tests/test_justificativa_encerra_origem.py:38`, `:168`, `:195`).

## 29/10 17:01 · sessao 9a870d76713d · justif · verificador · origem=Code

**Não aprovaria ainda:** há dois contraexemplos de deadlock e dois caminhos de rollback silencioso relevantes para esta obra (`app/ponto/services/justificativa.py:47`, `:263`; `app/chamados/signals.py:83`; `app/ponto/services/fechamento.py:1620`). Reprodução no banco e ocorrência em produção: **[nao verificado]**.

**1. Defeitos encontrados**

- **D1 — a pré-trava usa uma origem que pode mudar. Classe: ordem de trava.**  
  `travar_origem_antes` lê `chamado_origem_id` sem travar J; o fluxo de resposta pode preencher uma origem anteriormente nula (`app/ponto/services/justificativa.py:47`; `app/ponto/views.py:1631`).

  Contraexemplo: aprovador lê origem `NULL` → colaborador vincula C e salva → aprovador trava J → reconciliador escreve C e tenta atualizar J → aprovador tenta travar C. Reaparece **J→C contra C→J**, com possibilidade de deadlock (`app/ponto/views.py:1633`, `:1648`; `app/ponto/services/justificativa.py:61`, `:134`, `:32`; `app/chamados/reconciliador.py:455`, `:468`).

- **D2 — a pré-trava introduz ciclo entre materializações. Classe: ordem de trava.**  
  Cada materialização prende sua origem antes de J, mas depois percorre uma cascata que pode incluir as origens das outras materializações (`app/ponto/services/justificativa.py:263`, `:315`, `:324`).

  Contraexemplo: mesmo colaborador/dia; C1 `turno_aberto` e C2 `turno_aberto_24h` vivos; J1/J2 aprovadas, entradas distintas, origens respectivas e sem batida gerada — configuração permitida pelas restrições (`app/ponto/models.py:186`, `:189`). T1 prende C1/J1; T2 prende C2/J2; ambas percorrem C1/C2: T1 espera C2 e T2 espera C1. `encerrar()` efetivamente escreve pelo núcleo (`app/ponto/services/justificativa.py:263`, `:324`; `app/chamados/models.py:1201`, `:231`).

  O cenário independe de novos signals de batida: saídas preexistentes e desvinculadas podem ser devolvidas pelo dedup; sem célula, o julgamento disparado pelo save de J retorna sem agir (`app/ponto/registro_batida.py:113`; `app/ponto/services/cartorio.py:1547`).

- **D3 — permanece aprovação sem fechamento por rollback silencioso interno. Classe: outra — meio-estado.**  
  O receiver de resolução captura exceções sem savepoint próprio; o resolver salva perguntas diretamente dentro do savepoint de `_gravar_estado` (`app/chamados/signals.py:83`; `app/chamados/services/disputa_emissao.py:1583`; `app/chamados/models.py:226`).

  Contraexemplo: J pendente, C vivo, pergunta elegível → aprovação salva J → fechamento salva C → erro de banco no `p.save()` → receiver engole → savepoint desfaz C e sua trilha → `reconciliar()` retorna `True`. O atomic externo pode conservar **J aprovada e C vivo** (`app/ponto/services/justificativa.py:122`, `:134`; `app/chamados/models.py:227`, `:231`, `:445`). O mecanismo de rollback silencioso está documentado na própria cópia (`app/core/observ.py:124`).

- **D4 — novo falso sucesso no lote após rollback integral. Classe: outra.**  
  `invalidar_fechamento` captura erro de banco sem savepoint próprio; agora essa operação está dentro do atomic do item (`app/ponto/services/fechamento.py:1607`, `:1615`, `:1620`; `app/ponto/views.py:1791`, `:1804`).

  Contraexemplo: J pendente, sem origem e sem saída informada; fechamento derivado elegível → erro de banco no `update` da invalidação → exceção engolida → ramo da saída não executa → helper retorna antes de consultar o banco → atomic desfaz J silenciosamente, mas o lote incrementa `aprovadas` e responde sucesso. Resultado: **“1 aprovada”, J ainda pendente** (`app/ponto/views.py:1807`, `:1830`, `:1841`, `:1847`; `app/ponto/services/justificativa.py:29`). É a mesma mecânica documentada em `app/core/observ.py:124`.

- **D5 — selo ainda mede atribuição, não persistência. Classe: selo/RED sem mordida.**  
  `_gravacoes` reconhece `j.status = 'aprovada'`; `_chama` exige somente uma chamada posterior à atribuição dentro de algum `atomic` ancestral (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:36`, `:63`).

  Contraexemplo aceito pelo analisador:

  ```python
  with transaction.atomic():
      j.status = 'aprovada'
      encerrar_chamado_de_origem(j)
      j.save()
  ```

  Com o dia ainda furado, o chamado consulta J pendente no banco, recusa o fechamento, e depois J é persistida aprovada. Portanto, a garantia anunciada de chamada depois da **gravação** continua incompleta (`app/chamados/models.py:495`, `:937`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:6`, `:66`).

A ponte em si delega ao juiz e ao escritor existentes; não encontrei nela segundo juiz, escrita direta de `status_local` ou derivação paralela. Os carimbos de aprovação mostrados também já estão dentro dos respectivos atomics; não identifiquei nova leitura de relógio introduzida fora dessas bordas (`app/ponto/services/justificativa.py:29`, `:35`, `:124`, `:303`; `app/ponto/views.py:1797`).

**2. Escritores não alcançados**

**Sim: o admin genérico de Justificativa. FORA-DO-ESCOPO, preexistente.** Ele herda `ModelAdmin` sem restringir a edição nem encaminhar a aprovação à ponte; `status` é editável e aceita `aprovada`. Entrada: salvar aprovação pelo admin → fechamento da origem não é garantido pelo diff. Uso efetivo desse caminho em produção: **[nao verificado]** (`app/ponto/admin.py:18`; `app/ponto/models.py:130`, `:159`).

Os três escritores explícitos de aprovação encontrados estão alcançados: individual, materialização e lote (`app/ponto/services/justificativa.py:122`, `:301`; `app/ponto/views.py:1795`). O próprio selo declara que não cobre admin, `update` nem `create` (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:12`).

Não encontrei escrita explícita de `status_local` fora das portas no universo do selo existente: os sítios encontrados são `_gravar_estado`, `abrir` e `criar`. Isso é uma conclusão limitada pelas formas reconhecidas pelo analisador, não prova sobre escrita dinâmica genérica (`app/chamados/models.py:192`, `:730`, `:834`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `:37`, `:337`).

**3. Mordida dos testes e revisão de (a)–(d)** [nao verificado]

- **(a) Corrigido para exceção propagada da saída:** aprovação, saída e fechamento estão no atomic do item. O teste `test_LOTE_falha_da_saida_desfaz_o_item_inteiro` diferencia esse caso da versão com saída fora do atomic; D3/D4 mostram que a garantia geral de falha ainda não está completa (`app/ponto/views.py:1791`, `:1814`, `:1829`; `app/ponto/tests/test_justificativa_encerra_origem.py:184`).

- **(b) Corrigido apenas para origem estável e o par simples C/J:** a pré-trava antecede J nas três portas. D1 e D2 impedem considerar a ordem global resolvida (`app/ponto/services/justificativa.py:60`, `:263`; `app/ponto/views.py:1792`).

- **(c) Parcialmente corrigido:** o selo agora rejeita chamada fora do atomic e antes da atribuição, mas aceita o contraexemplo D5, antes do `save()` (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:126`, `:134`, `:66`).

- **(d) Os dois negativos citados agora provam o ato:** rejeição exige J rejeitada; ponteiro de outra justificativa exige J aprovada e trilha de recusa. O segundo passa a detectar a retirada da consulta à porta no caminho exercitado (`app/ponto/tests/test_justificativa_encerra_origem.py:136`, `:157`, `:159`).

**Ainda há testes que passam por ausência de sinal — classe selo/RED sem mordida:** os dois testes de rollback só verificam J pendente/C vivo, sem exigir que o mock tenha sido chamado. Um retorno antecipado antes do ato satisfaz ambos (`app/ponto/tests/test_justificativa_encerra_origem.py:164`, `:168`, `:172`, `:184`, `:194`, `:198`).

O teste da materialização nasce com J já aprovada: detecta falta de fechamento da origem de outro módulo, mas não prova a transição pendente→aprovada nem sua atomicidade; mover a ponte para antes do `save()` pode continuar passando nesse caso (`app/ponto/tests/test_justificativa_encerra_origem.py:215`, `:226`; `app/ponto/services/justificativa.py:301`, `:304`).

**4. Ordem contra o reconciliador**

Com origem estável, o novo caminho prende **C→J**, acompanhando o reconciliador, que escreve C antes de atualizar J. Porém, a origem lida sem proteção pode restaurar **J→C**, e a cascata acrescenta o ciclo **C1→C2 contra C2→C1** descrito em D2 (`app/ponto/services/justificativa.py:47`, `:60`, `:263`, `:324`; `app/chamados/reconciliador.py:455`, `:468`).

**5. Efeitos dos receivers dentro da aprovação**

- **Rebaixa pode propagar e desfazer a aprovação:** não há captura no receiver. No fechamento normal, o núcleo já normalizou a urgência, fazendo o receiver retornar sem nova escrita (`app/chamados/signals.py:68`; `app/chamados/models.py:210`, `:424`; `app/ponto/services/justificativa.py:135`).
- **Resolve perguntas pode produzir o meio-estado D3:** captura ampla, saves sem savepoint próprio e rollback silencioso do savepoint do chamado (`app/chamados/signals.py:83`; `app/chamados/services/disputa_emissao.py:1583`; `app/chamados/models.py:226`).
- **Julgar célula engole exceções; célula e evento têm savepoint próprio.** Isso protege aquele par, mas não constitui garantia sobre todos os efeitos do receiver (`app/chamados/signals.py:166`; `app/core/observ.py:86`; `app/ponto/portas/celula.py:639`).
- Ler J aprovada e C encerrado neste fluxo é compatível com a ordem das escritas da mesma transação. Um estado errado causado exclusivamente pela ausência de commit: **[nao verificado]** (`app/ponto/services/justificativa.py:125`, `:134`; `app/chamados/models.py:231`).

Resultados das suítes Django, reprodução concorrente e os números de produção mencionados nos comentários: **[nao verificado]**.

## 29/10 16:59 · sessao 5238bb2f1bf4 · - · - · origem=Ronald (tmux)

Confirmado como defeito novo da reestruturação do lote, por análise estática (`app/ponto/views.py:1791-1805`).

Contraexemplo: J pendente, sem `chamado_origem`, sem saída a gerar; existe fechamento derivado elegível. O `FechamentoMensal.update()` sofre `DatabaseError`, capturado sem savepoint próprio (`app/ponto/services/fechamento.py:1607-1624`). O atomic do item fica marcado para rollback; o ramo de saída não executa (`app/ponto/views.py:1807`) e o helper retorna sem consultar o banco (`app/ponto/services/justificativa.py:29-30`). A saída do atomic desfaz J silenciosamente, mas incrementa `aprovadas` e responde sucesso (`app/ponto/views.py:1830`, `:1841-1848`). O mecanismo de rollback silencioso está documentado na cópia (`app/core/observ.py:124-132`).

Estado errado: resposta “1 aprovada”, banco com J ainda pendente. Classe: **outra — sucesso falso por rollback silencioso**. A ocorrência real em produção é [nao verificado].

## 29/10 16:59 · sessao 1ee182181f58 · - · - · origem=Ronald (tmux)

HEAD lido: `2cc71cc8ac027b864963daf221d770a4a2a2f191`. Ocorrências reais no banco e comportamento final do APK: **[nao verificado]**.

**(1) Materialização sem resposta e sem `respondida_em`**

Nos escritores abaixo, “preserva” significa: não preenche resposta/carimbo; se ambos estavam vazios, permanecem vazios.

| Escritor | Condição e campos gravados |
|---|---|
| `lavrar_sem_turno_aberto` — `app/chamados/reconciliador.py:340–408` | Texto vazio, materialização nula, motivo elegível, sem turno aberto no dia e juiz considera a pergunta morta. Grava materialização/autor/via; preserva `respondida_em` (`:363–408`). |
| `retratar_perguntas_absolvidas_por_celula` — `app/chamados/reconciliador.py:515–552` | Célula julgada não acusadora, sem marcos pendentes, pergunta sem materialização/validação e morta pelo juiz. Grava `celula_absolveu`, materialização e autor (`:515–552`). |
| `PerguntaDisputa.fechar_por_admin` — `app/chamados/models.py:2131–2134` | Grava `admin_decidiu`, materialização e autor; preserva resposta/carimbo. É chamada, entre outros, pela decisão administrativa do dia (`app/ponto/services/veredito_celula.py:177`). |
| `fechar_por_competencia_trancada` — `app/chamados/models.py:2143–2148` | Pergunta não validada e ainda não nessa via; grava via/materialização/autor, preservando resposta/carimbo. |
| `encerrar_fantasma` — `app/chamados/models.py:2168–2173` | Grava `fantasma`, materialização e autor. O consumidor chega à porta pelo encerramento de órfãs (`app/chamados/reconciliador.py:938–940`). |
| `_materializar_ausencia_de_resposta` — `app/chamados/services/materializacao.py:506–530` | Materializa irmãs `hora` do mesmo dia. Só preenche resposta temporal/validação quando já existe texto; irmã muda recebe via `ausencia_dia` sem esses carimbos (`:511–527`). |
| `_materializar_uma` — `app/chamados/services/materializacao.py:1129–1198` | Após materializador e juiz aprovarem, grava materialização/via; só preenche `respondida_em` se existe texto (`:1190–1193`). O materializador de escolha pode retornar sucesso sem exigir texto (`:1008–1024`). Alcançabilidade desse estado pelas filas normais: **[nao verificado]**. |
| `materializar_pendentes_aprovadas` — `app/chamados/services/materializacao.py:1335–1343` | Pergunta sem materialização, aprovada pelo juiz do fato; grava `juiz_aprovou`, materialização e autor, sem resposta/carimbo. Chamada pelo signal de disputa fechada (`app/chamados/signals.py:118–135`). |
| `resolver_perguntas_por_chamado_resolvido`, ramo principal — `app/chamados/services/disputa_emissao.py:1547–1583` | Pergunta morta pelo juiz, dia não trancado; grava materialização/via. Só preenche `respondida_em` quando há texto (`:1570–1573`). |
| `retratar_duplicata` — `app/chamados/services/fio.py:834–850` | Grava `duplicata`; preenche materialização se nula; preserva resposta/carimbo. Usada pela migração e pelo comando de deduplicação (`:892`; `app/chamados/management/commands/dedup_perguntas_fato.py:88`). |
| `_carimbar_recusa` — `app/chamados/services/materializacao.py:1042–1053` | Recusa `COLISAO` com materialização nula grava `fato_ocupado`, materialização e autor, sem resposta/carimbo. Caminho completo com ambos inicialmente vazios: **[nao verificado]**. |

Distinções:

- O ramo **sem credor vivo** do resolvedor não pertence ao recorte estrito: grava `respondida_em` se nulo, mesmo sem texto (`app/chamados/services/disputa_emissao.py:1643–1651`).
- A recusa de paridade grava materialização/via, mas seu ramo exige texto antes; não é produtor de pergunta totalmente muda (`app/chamados/services/materializacao.py:769–776`, `:858–868`).
- `encerrar_por_fato_no_espelho` escreve somente via; `relavrar` troca via/veredito preservando os demais campos (`app/chamados/models.py:2156–2159`; `app/chamados/services/relavra_via.py:115–126`).

**Via `sem_turno_aberto`**

O escritor é `lavrar_sem_turno_aberto`. As condições são cumulativas:

1. Motivo `orfao_14h` ou `saida_sem_entrada` (`app/chamados/reconciliador.py:332`, `:363–364`).
2. Texto vazio e materialização nula; **não exige `respondida_em` nulo** (`:365–368`).
3. Ausência de turno aberto no D recebido, ou em `p.data_evento` quando D não foi passado (`:369–373`). Com D, o teste delega a `turnos_abertos_de`; sem D consulta globalmente entrada sem saída (`:161–178`). [nao verificado]
4. `pergunta_viva(p)` deve ser falso (`:398–400`).
5. Grava somente materialização, autor e via; pode depois fechar a disputa se `pode_fechar()` (`:402–415`).

O chamador direto encontrado é `reatar_pergunta_orfa`: chega à lavra quando não encontra candidato vivo nem histórico correspondente (`app/chamados/reconciliador.py:1300–1342`). O laço normal exige destino nulo, disputa aberta e materialização nula (`:1248–1254`, `:1268–1272`). Esse laço é alcançado pelo reconciliador, inclusive via signal de batida e comando periódico (`:1195–1196`; `app/ponto/signals.py:214–215`; `app/chamados/management/commands/reconciliar_chamados.py:52`; `app/config/crons.py:376`).

**Depois do carimbo, pode voltar a contar como viva.** `sem_turno_aberto` não integra as vias que encerram por identidade; célula acusadora pode devolver `True`, salvo degraus anteriores como validação, tranca ou próprio marco já materializado (`app/chamados/juizes.py:1006–1057`). Sem decisão anterior, o juiz ainda considera janela e lastro (`:1062–1087`).

Para o app, `perguntas_do_app` usa `q_vivas` seguido do juiz, sem exigir materialização nula ou disputa aberta (`app/chamados/services/pergunta_no_app.py:39–44`). Portanto, a pergunta carimbada pode voltar à resposta da API se o juiz a considerar viva; as restrições do fio/PWA são diferentes, conforme item 3.

**(2) Emissores em disputa já existente**

A porta `PerguntaDisputa.abrir` não exige pai vivo nem disputa nova. Com data, deduplica globalmente por colaborador/motivo/data e pode devolver pergunta já existente; sem data explícita, trata conversa, atribui hoje e permite novo nascimento (`app/chamados/models.py:2028–2086`).

| Emissor | Como recebe/preenche `chamado_destino` | Pai já resolvido/fechado |
|---|---|---|
| `gerar_perguntas_disputa` — `app/chamados/services/disputa_emissao.py:280`, `:389–396` | Deriva para motivos elegíveis; demais podem receber `None` (`:379–396`). | Não há guarda do status do pai no nascimento. Pode criar com outro destino vivo ou sem destino. |
| `_criar_pergunta_se_couber` — `app/chamados/services/disputa_emissao.py:756–779` | Destino recebido do chamador. Barra ausência e repetição; não testa status do pai/destino (`:763–777`). | Pode adicionar pergunta à disputa existente sem reabrir seu pai nesse helper. |
| `_complementar_marcos_faltantes` — `app/chamados/services/disputa_emissao.py:783–815` | Repete o helper anterior para marcos faltantes, conservando o destino recebido (`:806–813`). | Herda o comportamento anterior. |
| `criar_disputa_a_partir_de_chamados_orfaos` — `app/chamados/services/disputa_emissao.py:847–1051` | Usa chamado do grupo/canônico; também complementa a disputa de pergunta existente (`:874–881`, `:956`, `:978`, `:1012–1018`, `:1043–1049`). | Os ramos de reutilização não exigem pai vivo; destino não é presumido igual ao pai. |
| `criar_disputa_ancora_de_chamado` — `app/chamados/services/disputa_emissao.py:1093–1167` | Destino explícito=`chamado_limbo`. Recusa limbo resolvido/fechado (`:1107–1110`). | Reutiliza disputa aberta sem testar o status de seu pai (`:1121–1124`); a pergunta nasce destinada ao limbo (`:1160–1167`). |
| `declarar_hora_colab` — `app/chamados/services/declaracao_texto.py:307–323` | Nova pergunta recebe o chamado da declaração e dia casado; reutilizada sem destino passa pelo reapontador (`:311–323`). | Carregador de disputa não exige pai vivo (`app/chamados/services/disputa_emissao.py:467–502`). |
| `aplicar_declaracao` — `app/chamados/services/declaracao_texto.py:589–612` | Destino explícito=`ch`, data da proposta. | Reutiliza o mesmo carregador; não há guarda do pai na criação (`:605–612`). |
| `acao_reabrir_disputa_admin` — `app/chamados/services/acoes_disputa.py:232–266` | Cria pergunta de confirmação **sem destino nem data explícita** (`:259–264`). | Só depois tenta `ch.reabrir()`; não verifica o retorno nesse trecho (`:265–266`). Se recusada a reabertura, a pergunta já foi criada sob pai encerrado. |

O carregador comum `_abrir_ou_agregar_disputa` escolhe por colaborador e `fechada_em IS NULL`, sem filtrar o status do pai; no ramo de reutilização apenas agrega motivos/contexto (`app/chamados/services/disputa_emissao.py:467–502`).

O derivador de destino procura chamado **vivo**, exclui categoria `disputa_supervisao`, exige motivo atendido e dia compatível; ausência/ambiguidade pode deixar destino nulo (`app/chamados/services/disputa_emissao.py:181–228`). Esse critério não transforma automaticamente o pai da disputa em destino.

Entre os gatilhos encontrados estão criar/atualizar disputa, entradas pendentes e fábrica da célula (`app/chamados/services/disputa_emissao.py:274`, `:618–621`; `app/chamados/services/fabrica_da_celula.py:98–110`). API de batida, triagem e comando retroativo chamam criar/atualizar (`app/api/views.py:778`; `app/api/views_core.py:778`; `app/ponto/services/triagem_batida.py:651`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`).

**(3) Destino nulo e pai terminal: onde aparece**

`chamado_que_lista` retorna destino ou pai, **sem testar status**. Nesse caso, devolve o pai terminal (`app/chamados/services/fio.py:97–117`). Vitalidade e localização são leituras distintas: o juiz não contém veto geral por pai terminal ou disputa fechada (`app/chamados/juizes.py:1005–1087`).

| Canal/view/endpoint | Resultado condicionado pelos filtros |
|---|---|
| **Central**, `/chamados/` e `/chamados/painel/` → `painel_gestao` | Pai terminal fica fora da lista padrão, que exige `COBRANCA`. Pode entrar no histórico/filtro de status correspondente (`app/chamados/urls.py:9`, `:22`; `app/chamados/services/painel.py:301–314`). |
| **Fila/contador** | Exige chamado vivo em cobrança e sem silêncio futuro; pergunta viva não torna pai terminal elegível (`app/chamados/services/painel.py:186–188`). O crédito também parte de destino ou pai pertencentes ao conjunto vivo (`app/chamados/services/raiox.py:49–64`). |
| **Fio administrativo**, `/chamados/colaborador/<id>/` → `fio_colaborador` | Carrega todos os chamados; destino nulo anexa a pergunta ao pai terminal (`app/chamados/urls.py:21`; `app/chamados/views.py:676–677`; `app/chamados/services/fio.py:173–176`, `:236–249`). |
| **Modal administrativo**, `/chamados/modal/<id>/` → `modal_fio_colaborador` | Usa o mesmo montador; template não exige chamado vivo para exibir perguntas (`app/chamados/urls.py:39`; `app/chamados/views.py:869–889`; `app/templates/chamados/partials/modal_fio.html:154–157`). |
| **Disputa aberta no fio administrativo** | Busca todas as perguntas daquele ramo, sem filtro de vitalidade na seleção; pode mostrar estado/histórico (`app/chamados/services/fio.py:218–228`). |
| **Disputa fechada no fio administrativo** | Inclui perguntas por `q_vivas` e confirma no juiz; viva pode aparecer sob pai terminal (`app/chamados/services/fio.py:213–249`). |
| **Retrato/fonte da folha** | Pergunta é carregada por célula; viva pendente gera fio `colab`, outra viva gera `admin`, independentemente do pai terminal/destino nulo (`app/folha/fora_da_folha.py:51–73`). Sem associação à célula retentora, não entra nesse mapa. |
| **Tela do retrato** | Lê projeção gravada, não a pergunta diretamente; linha contém colaborador/datas/dono/tipo, sem ID da pergunta (`app/colaboradores/views.py:980–985`; `app/folha/retrato_folha.py:1177–1188`). |
| **`GET /api/disputa/perguntas/` → `api_disputa_perguntas`** | Se viva, entra mesmo com disputa fechada, pai terminal e destino nulo: seleção é por perguntas do colaborador + `q_vivas` + juiz (`app/api/urls.py:92`; `app/api/views.py:2603–2604`; `app/chamados/services/pergunta_no_app.py:39–44`). |
| **Serialização desse endpoint** | A pergunta recebe `chamado_id=NULL`; o campo superior usa destino de uma pendente ou fallback para pai da primeira disputa selecionada, que pode ser terminal (`app/api/views.py:2615`, `:2622–2627`, `:2642–2644`). |
| **`GET /api/chamados/v2/` → `api_chamados_v2`** | Depende de o pai passar pelo recorte de chamados. Não há exclusão geral de terminal, mas exige janela/exceção, silêncio nulo e módulo permitido (`app/api/urls.py:73`; `app/api/views.py:1015–1016`; `app/chamados/services/janela_colab.py:61–74`). |
| **Perguntas dentro da v2** | Só disputas abertas; destino nulo indexa no pai. Se o pai não virou card, a pergunta não ganha card independente (`app/api/views.py:1028–1048`, `:1121–1122`, `:1158–1177`). |
| **PWA `/chamados/meu-atendimento/` → `meu_atendimento`** | Usa fio com janela; não carrega perguntas de disputas fechadas. Pai fora da janela não é resgatado por pergunta sem destino (`app/chamados/urls.py:16`; `app/chamados/views.py:559–569`; `app/chamados/services/fio.py:169–172`, `:213`; `app/chamados/services/janela_colab.py:61–69`). |
| **`GET /api/chamados/`** | Retorna HTTP 410; não lista o caso (`app/api/urls.py:70`; `app/api/views.py:888–893`). |

A exceção de janela usa somente **destinos não nulos** de `q_vivas`; não inclui o pai como fallback. Além disso, o filtro exige `silenciado_ate IS NULL`, não apenas silêncio vencido (`app/chamados/services/janela_colab.py:61–74`).

Como o APK renderiza a combinação “ID nulo por pergunta + pai terminal no topo”: não li, **[nao verificado]**.

**(4) Portas de reapontamento e chamadores**

**Sim, existem caminhos para reapontar a chamado vivo; a porta genérica não exige, sozinha, pergunta viva e destino vivo.** Ela verifica compatibilidade do fato e grava FK/data/veredito (`app/chamados/services/fio.py:785–801`).

| Porta/caminho | Condição e chamadores |
|---|---|
| `reapontar_resolvedora` — `app/chamados/services/fio.py:740–801` | Preenche data se ausente, consulta `resolvedora_valida`, grava destino e relavra no mesmo `save`; não testa vitalidade da pergunta nem status do destino (`:785–800`). |
| `PerguntaDisputa.abrir`, ramo de dedup — `app/chamados/models.py:2061–2084` | Também reaponta pergunta existente quando recebe destino explícito diferente; atualiza célula e relavra. Não exige pergunta/destino vivos. |
| `migrar_resolvedoras_vivas` — `app/chamados/services/fio.py:854–900` | Seleciona perguntas vivas e chama reapontador (`:882–896`). Chamador encontrado: reabertura recusada por irmão vivo cobrindo o turno (`app/chamados/models.py:1068–1072`). |
| `declarar_hora_colab` | Reutilizada sem destino recebe chamado da declaração (`app/chamados/services/declaracao_texto.py:311–317`). |
| `reemitir_chamado_do_dia` | Exige chamado resultante em `VIVOS`, depois reaponta (`app/chamados/reconciliador.py:733–750`); chamado pelo reatador (`:1344`). |
| `reatar_pergunta_orfa`, candidato vivo | Deriva candidato vivo e reaponta (`app/chamados/reconciliador.py:1300–1301`, `:1376`). |
| `reatar_pergunta_orfa`, histórico | Também pode reapontar para **encerrado**, quando há histórico único; depois chama o resolvedor (`app/chamados/reconciliador.py:1316–1319`, `:1364–1370`). Não é reparo para destino vivo nesse ramo. |

O reatador é chamado pelo laço de órfãs e por geração de pergunta nova elegível (`app/chamados/reconciliador.py:1271`; `app/chamados/services/disputa_emissao.py:404–409`). O laço normal exige **destino nulo, disputa aberta, materialização nula e motivo reatável**; portanto não alcança diretamente uma pergunta já carimbada `sem_turno_aberto` (`app/chamados/reconciliador.py:1248–1254`).

“Mesmo dia” não é igualdade civil obrigatória na porta: o juiz aceita data igual, admite diferença de um dia no cruzamento de meia-noite e pode permitir quando faltam dados para provar divergência (`app/chamados/juizes.py:1703–1737`).

Escritores/emissores dinâmicos sem referências literais, SQL externo e testes/migrations: **[nao verificado]**; testes/migrations não li integralmente. [nao verificado]

## 29/10 16:58 · sessao 96e4ba45118a · o261 · censo · origem=Code

**O estado descrito é compatível com o juiz atual: `materializada_em` preenchida não implica `pergunta_viva=False`. Porém, a lavra atual de `sem_turno_aberto` recusa uma pergunta que já esteja viva no instante da consulta.** A distinção é entre carimbo antigo, mudança posterior dos insumos e leitura concorrente/desatualizada. (`app/chamados/reconciliador.py:398`, `app/chamados/reconciliador.py:402`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1056`)

A ocorrência concreta e a cronologia da pergunta no banco não foram verificadas. [nao verificado]

**Definição de `pergunta_viva` e dependência do carimbo**

`pergunta_viva(p)` é o wrapper com memo por instância; a decisão está em `_pergunta_viva_calc(p)`. A chave do memo inclui expressamente `materializada_em` e `via_resolucao`. Portanto, **sim, ela depende de `materializada_em`**, mas também de outros campos e entidades. (`app/chamados/juizes.py:778`, `app/chamados/juizes.py:791`, `app/chamados/juizes.py:1005`, `app/chamados/juizes.py:769`)

A precedência da decisão é esta:

| Ordem | Decisão |
|---|---|
| 1 | `validada_em` preenchida → morta. (`app/chamados/juizes.py:1006`) |
| 2 | Dia efetivamente trancado → morta; via `competencia_trancada` com carimbo também → morta. (`app/chamados/juizes.py:1010`, `app/chamados/juizes.py:1015`) |
| 3 | Resposta com texto **e** `respondida_em`, sem decisão administrativa do dia → decide pela concordância resposta/luz. Esse ramo não alcança a pergunta muda descrita. (`app/chamados/juizes.py:1018`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1028`) |
| 4 | Via de identidade com carimbo → morta. O conjunto contém admin, duplicata, fato ocupado, paridade e competência trancada; **não contém `sem_turno_aberto`**. (`app/chamados/juizes.py:1039`, `app/chamados/juizes.py:1044`) |
| 5 | Carimbo preenchido **e** marco próprio comprovadamente aceso → morta. (`app/chamados/juizes.py:1050`) |
| 6 | Célula com veredito em `VEREDITOS_ACUSAM` → **viva, mesmo materializada**. (`app/chamados/juizes.py:1054`) |
| 7 | Célula julgada que não acusa, sem marcos pendentes → morta; com pendentes, continua pelos próximos degraus. (`app/chamados/juizes.py:1063`) |
| 8 | Data dentro da janela viva e `classificar_falta(...)[1]` não nulo → viva. (`app/chamados/juizes.py:1066`) |
| 9 | Sem materialização → viva; com materialização → negação de `_pergunta_materializada_de_fato`. (`app/chamados/juizes.py:1076`, `app/chamados/juizes.py:1083`) |

O marco próprio vem de `MARCO_DA_PERGUNTA`: `saida_sem_entrada → hi/E`, `orfao_14h → hf/S`, e intervalos → suas respectivas pausas. A prova consulta DNA e lâmpadas da ata da célula; ausência de informação retorna `None`, não prova positiva. (`app/chamados/juizes.py:823`, `app/chamados/juizes.py:831`, `app/chamados/juizes.py:849`, `app/chamados/juizes.py:866`)

`sem_turno_aberto` pertence a `VIAS_SEM_TESTEMUNHA`. Nesse ramo, `_pergunta_materializada_de_fato` ainda consulta a classificação do dia dentro da janela viva: classificação acusatória retorna `False`, e a negação final pode devolver a pergunta viva. (`app/chamados/models.py:2530`, `app/chamados/juizes.py:603`, `app/chamados/juizes.py:616`, `app/chamados/juizes.py:621`, `app/chamados/juizes.py:1083`)

Assim, **num cálculo fresco do estado informado**, `validada_em` precisa estar nula, nenhuma tranca efetiva pode barrá-la e o marco próprio não pode estar comprovadamente aceso; depois disso, uma célula acusatória basta para `True`. (`app/chamados/juizes.py:1006`, `app/chamados/juizes.py:1010`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1056`)

**Todos os sítios explícitos de escrita encontrados**

A tabela inclui preenchimento, limpeza e escrita somente de via. “PV” significa a função `pergunta_viva` definida acima; juiz do lastro significa `_pergunta_materializada_de_fato`, que é outra decisão. (`app/chamados/juizes.py:778`, `app/chamados/juizes.py:588`)

| Sítio | Escrita | Guarda anterior e consulta de PV |
|---|---|---|
| `lavrar_sem_turno_aberto` | `materializada_em=now`, autor e `VIA_SEM_TURNO_ABERTO`. (`app/chamados/reconciliador.py:402`) | Motivo em `MOTIVOS_C1`; texto vazio após `strip`; materialização nula; nenhum turno aberto no recorte; **PV precisa ser False**. Não exige `respondida_em` nula nem `validada_em` nula. (`app/chamados/reconciliador.py:332`, `app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:372`, `app/chamados/reconciliador.py:398`) |
| `retratar_perguntas_absolvidas_por_celula` | Materialização, autor e `VIA_CELULA_ABSOLVEU`. (`app/chamados/reconciliador.py:548`) | Colaborador, materialização/validação nulas, célula com veredito preenchido fora dos acusatórios; nenhum marco pendente; **PV=False**. Não exige disputa aberta para carimbar. (`app/chamados/reconciliador.py:515`, `app/chamados/reconciliador.py:526`, `app/chamados/reconciliador.py:538`, `app/chamados/reconciliador.py:544`, `app/chamados/reconciliador.py:554`) |
| `ChamadoColaborador.ressuscitar_perguntas` | Limpa materialização, via, validação e `respondida_em`. (`app/chamados/models.py:1162`) | Perguntas daquele destino já materializadas e sem texto; exclui marco comprovadamente aceso; exige IDs restantes. **Não consulta PV**; consulta o fato do marco. (`app/chamados/models.py:1111`, `app/chamados/models.py:1118`, `app/chamados/models.py:1153`) |
| `PerguntaDisputa.fechar_por_admin` | Materialização, autor e `admin_decidiu`. (`app/chamados/models.py:2131`) | Só recusa se já tiver essa via **e** materialização; pode sobrescrever outra via. **Não consulta PV dentro do método**. (`app/chamados/models.py:2117`, `app/chamados/models.py:2122`) |
| `fechar_por_competencia_trancada` | Materialização, autor e `competencia_trancada`. (`app/chamados/models.py:2145`) | Recusa validada ou já nessa via. **Não consulta PV**. (`app/chamados/models.py:2137`, `app/chamados/models.py:2143`) |
| `encerrar_por_fato_no_espelho` | **Somente via**, para `fato_materializado`. (`app/chamados/models.py:2158`) | Recusa se já nessa via; a prova do fato pertence ao chamador. **Não consulta PV dentro do método**. (`app/chamados/models.py:2151`, `app/chamados/models.py:2156`) |
| `encerrar_fantasma` | Materialização, autor e `fantasma`. (`app/chamados/models.py:2170`) | Recusa apenas se já fantasma e materializada. **Não consulta PV dentro do método**; depende do julgamento do chamador. (`app/chamados/models.py:2162`, `app/chamados/models.py:2168`) |
| `reabrir_fantasma` | Limpa materialização, autor e via. (`app/chamados/models.py:2185`) | Exige via fantasma e materialização preenchida. **Não consulta PV dentro do método**. (`app/chamados/models.py:2183`) |
| `reabrir_por_resposta` | Limpa materialização, autor e via **em memória**, retornando campos a salvar. (`app/chamados/models.py:2205`) | Exige materialização e via em `VIAS_SEM_LASTRO`; **não consulta PV**. O escritor da resposta inclui os campos retornados no save. (`app/chamados/models.py:2203`, `app/chamados/services/disputa_emissao.py:1265`) |
| `reabrir_por_competencia` | Limpa materialização, autor e via. (`app/chamados/models.py:2215`) | Exige via `competencia_trancada`; **não consulta PV**. (`app/chamados/models.py:2213`) |
| `relavra_via.relavrar` | **Somente via**, acompanhada da relavra do veredito. (`app/chamados/services/relavra_via.py:123`) | Via anterior em `declaracao/completude_turno`; nova via não nula e diferente. **PV não é guarda**; `lavrar` acontece depois da atribuição. (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:124`) |
| `desfazer_carimbo_sem_lastro` | Limpa materialização, autor e via. (`app/chamados/services/relavra_via.py:204`) | Materializada, via fora das identidades e **PV=True**. (`app/chamados/services/relavra_via.py:166`, `app/chamados/services/relavra_via.py:196`) |
| `fio.retratar_duplicata` | Via `duplicata`; preenche materialização/autor somente se materialização estiver nula. (`app/chamados/services/fio.py:844`) | Recusa se já duplicata. **Não consulta PV dentro da porta**. (`app/chamados/services/fio.py:830`, `app/chamados/services/fio.py:834`) |
| `disputa_emissao.reabrir_para_correcao` | Com `desfazer_absorcao=True`, limpa materialização/autor/via pelo `update`. No padrão, limpa resposta e validação, preservando esses campos. (`app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/disputa_emissao.py:1377`) | Recebe queryset escolhido pelo chamador; **não consulta PV dentro da função**. (`app/chamados/services/disputa_emissao.py:1347`, `app/chamados/services/disputa_emissao.py:1358`) |
| `resolver_perguntas_por_chamado_resolvido`, primeiro laço | Materialização e via derivada por `via_pelo_fato`. (`app/chamados/services/disputa_emissao.py:1562`) | Exclui encerramento por tranca; exige destino igual, materialização nula e disputa aberta; **PV=False**, dia não trancado; recusa resposta com carimbo sem lastro. Texto vazio não recebe `respondida_em`/validação. (`app/chamados/services/disputa_emissao.py:1531`, `app/chamados/services/disputa_emissao.py:1536`, `app/chamados/services/disputa_emissao.py:1547`, `app/chamados/services/disputa_emissao.py:1551`, `app/chamados/services/disputa_emissao.py:1558`, `app/chamados/services/disputa_emissao.py:1570`) |
| Mesmo método, segundo laço | Materialização e via pelo pai, ou `chamado_encerrado`; preenche `respondida_em` se nula. (`app/chamados/services/disputa_emissao.py:1643`, `app/chamados/services/disputa_emissao.py:1648`) | Disputa aberta, pai fora de VIVOS, nenhum destino VIVO, materialização/validação nulas e **PV=False**. (`app/chamados/services/disputa_emissao.py:1624`, `app/chamados/services/disputa_emissao.py:1630`, `app/chamados/services/disputa_emissao.py:1641`) |
| `materializacao._materializar_ausencia_de_resposta` | Materialização das irmãs de hora do dia; via `declaracao` se declarou hora, senão `ausencia_dia`. (`app/chamados/services/materializacao.py:507`, `app/chamados/services/materializacao.py:526`) | Exige data, cria/reutiliza ausência e seleciona irmãs ainda não materializadas. **Não consulta PV**. Entrada pela resposta de escape. (`app/chamados/services/materializacao.py:495`, `app/chamados/services/materializacao.py:499`, `app/chamados/services/disputa_emissao.py:1292`) |
| `_materializar_resposta_hora`, paridade | Materialização, autor e `paridade_recusada`. (`app/chamados/services/materializacao.py:858`) | Exige texto e timestamp; sem batida reutilizável; vizinha do mesmo tipo; plantio não preenche furo; marco próprio comprovadamente aceso. **Não consulta PV**; consulta o fato do marco. (`app/chamados/services/materializacao.py:769`, `app/chamados/services/materializacao.py:785`, `app/chamados/services/materializacao.py:799`, `app/chamados/services/materializacao.py:815`, `app/chamados/services/materializacao.py:840`) |
| `_carimbar_recusa` | Materialização, autor e `fato_ocupado`. (`app/chamados/services/materializacao.py:1043`) | Após refresh, materialização nula e recusa traduzida como `COLISAO`. **Não consulta PV**. (`app/chamados/services/materializacao.py:1035`, `app/chamados/services/materializacao.py:1042`) |
| `_materializar_uma` | Materialização e via `declaracao` ou derivada do fato; sem destino, `chamado_encerrado`. (`app/chamados/services/materializacao.py:1149`, `app/chamados/services/materializacao.py:1167`) | Bloqueia data divergente não conferida; exige sucesso do ramo e, para hora/data_hora, aprovação do **juiz do lastro**. **PV não é guarda**. (`app/chamados/services/materializacao.py:1096`, `app/chamados/services/materializacao.py:1124`, `app/chamados/services/materializacao.py:1136`) |
| `materializar_pendentes_aprovadas` | Materialização, autor e `juiz_aprovou`. (`app/chamados/services/materializacao.py:1339`) | Pergunta da disputa ainda não materializada e aprovada pelo **juiz do lastro**. **Não consulta PV**. (`app/chamados/services/materializacao.py:1335`) |

Há também a criação genérica em `PerguntaDisputa.abrir(..., **campos)`: os defaults são materialização nula e via vazia. Ela aceita campos adicionais, mas não identifiquei chamador runtime passando explicitamente esses dois campos. (`app/chamados/models.py:2085`, `app/chamados/models.py:2291`, `app/chamados/models.py:1952`; completude da negativa: [nao verificado])

As migrations introduzem `materializada_em` nullable e `via_resolucao` com default vazio; não identifiquei migração de dados preenchendo esses campos. (`app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:21`, `app/chamados/migrations/0033_perguntadisputa_via_resolucao.py:13`; negativa: [nao verificado])

**Crons, commands, signals e views: onde entram nessas escritas**

| Entrada | Guarda e delegação |
|---|---|
| Cron do reconciliador | Agendado `*/15`; command seleciona colaborador com chamado vivo **ou** pergunta órfã reatável e chama `reconciliar_chamados_colab`. (`app/config/crons.py:376`, `app/chamados/management/commands/reconciliar_chamados.py:36`, `app/chamados/management/commands/reconciliar_chamados.py:51`) |
| Signal de Batida | Chama o reconciliador após os receivers de recomputação/julgamento. (`app/ponto/signals.py:206`, `app/ponto/signals.py:214`) |
| Emissão de pergunta | Pergunta recém-criada, sem destino e com motivo elegível chama diretamente `reatar_pergunta_orfa`, que pode chegar a `lavrar_sem_turno_aberto`. (`app/chamados/services/disputa_emissao.py:404`, `app/chamados/services/disputa_emissao.py:409`, `app/chamados/reconciliador.py:1342`) |
| Signal de chamado encerrado | Sem `raw`, com status em ENCERRADOS, delega ao S128. (`app/chamados/signals.py:75`, `app/chamados/signals.py:81`, `app/chamados/signals.py:86`) |
| Signal de disputa fechada | Sem `raw`, não recém-criada, `fechada_em` preenchida: delega a `materializar_pendentes_aprovadas`. (`app/chamados/signals.py:118`, `app/chamados/signals.py:124`, `app/chamados/signals.py:134`) |
| Signal da própria pergunta | Sem `raw` e com disputa, agenda fechamento de beco em `on_commit`; a porta de becos também chama S128. (`app/chamados/signals.py:185`, `app/chamados/signals.py:196`, `app/chamados/services/becos.py:41`) |
| Evento da célula | Consumidor chama `encerrar_orfas_pela_lampada`; exige pergunta aberta sem carimbo/validação, texto vazio, marco aceso, **PV=True**, dia anterior a hoje e encerramento confirmado; então chama `encerrar_fantasma`. (`app/chamados/reconciliador.py:845`, `app/chamados/reconciliador.py:878`, `app/chamados/reconciliador.py:882`, `app/chamados/reconciliador.py:884`, `app/chamados/reconciliador.py:940`, `app/chamados/reconciliador.py:1148`) |
| Commands de dedup, fato e relavra | `dedup_perguntas_fato --apply` chama `retratar_duplicata`; `encerrar_perguntas_com_fato --apply` exige marco aceso e chama a escrita somente de via; `relavrar_via_i6 --apply` chama `relavrar`. (`app/chamados/management/commands/dedup_perguntas_fato.py:84`, `app/chamados/management/commands/dedup_perguntas_fato.py:88`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`, `app/chamados/management/commands/relavrar_via_i6.py:96`, `app/chamados/management/commands/relavrar_via_i6.py:102`) |
| Command de reabertura de fantasmas | Exige lâmpada apagada, **PV=True**, janela autorizada e demais filtros; em `--apply`, chama `reabrir_fantasma`. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:77`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:87`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:121`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`) |
| Views/ações de reabertura | Selecionam perguntas corrigíveis antes de `reabrir_para_correcao`. (`app/chamados/views.py:1419`, `app/chamados/views.py:1435`, `app/chamados/services/acoes_disputa.py:96`) |
| Validação/regularização humana | As ações delegam à materialização; validação de folga e decisão administrativa do dia podem chamar `fechar_por_admin`. (`app/chamados/services/acoes_disputa.py:139`, `app/chamados/services/validacao.py:264`, `app/chamados/services/validacao.py:292`, `app/ponto/services/veredito_celula.py:174`) |

O serviço atual de competência trancada declara que **não carimba perguntas**; a tranca é lida pelo C6. O método antigo de carimbo continua definido no modelo. (`app/chamados/services/competencia_trancada.py:170`, `app/chamados/models.py:2137`, `app/chamados/juizes.py:1010`)

`passivo_validado.CAMPOS_REVERSAO` é snapshot, lido por `getattr`; a escrita efetiva delega a `_materializar_uma`. Portanto, essa lista de campos não é outra restauração dinâmica de materialização/via. (`app/chamados/services/passivo_validado.py:64`, `app/chamados/services/passivo_validado.py:83`, `app/chamados/services/passivo_validado.py:516`)

**Ordens de execução que explicam o estado**

1. **Comportamento histórico: gravação enquanto já estava viva.** O teste da O261 documenta explicitamente o código anterior: ausência de turno aberto bastava para materializar e escrever `sem_turno_aberto`, embora o juiz continuasse dando a pergunta por viva. O teste atual exige que essa situação seja barrada. Isso é evidência documental do repositório; não prova a origem desta pergunta específica. (`app/chamados/tests/test_o261_lavra_le_a_lampada.py:30`, `app/chamados/tests/test_o261_lavra_le_a_lampada.py:129`; origem concreta: [nao verificado])

2. **Código atual: morta na lavra, viva após mudança da célula.** Sequência condicional: pergunta muda, sem carimbo, célula que não acusa e sem pendentes → PV=False; sem turno aberto e demais guardas satisfeitas → grava materialização/`sem_turno_aberto`, preservando resposta e `respondida_em`; depois a célula passa a acusar, com marco próprio apagado → PV=True. A consulta atual não congela o veredito da célula no instante da materialização. (`app/chamados/juizes.py:1063`, `app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:398`, `app/chamados/reconciliador.py:407`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1056`)

   Essa mudança externa tem escritores reais: salvar/apagar Batida ou Ausência chama julgamento dos dias; o cartório recalcula ata/veredito, e a porta grava esses campos da célula. (`app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/services/cartorio.py:1406`, `app/ponto/portas/celula.py:689`)

3. **A ordem “célula primeiro” não cobre todas as entradas.** No reconciliador completo, `retratar_perguntas_absolvidas_por_celula` roda antes da reconciliação das órfãs; nesse caminho, uma absolvição elegível recebe `celula_absolveu` antes de alcançar c1. Mas a emissão chama `reatar_pergunta_orfa` diretamente, sem passar por essa ordem do orquestrador. (`app/chamados/reconciliador.py:1195`, `app/chamados/reconciliador.py:1196`, `app/chamados/services/disputa_emissao.py:404`)

4. **Mudança de vínculo da pergunta.** `PerguntaDisputa.abrir` pode encontrar a pergunta existente, inclusive materializada, e atribuir-lhe célula ou destino, preservando materialização/via/resposta. Portanto, uma pergunta anteriormente carimbada pode passar a enxergar uma célula acusatória e ficar viva, satisfeitas as precedências do juiz. (`app/chamados/models.py:2022`, `app/chamados/models.py:2058`, `app/chamados/models.py:2064`, `app/chamados/models.py:2071`, `app/chamados/models.py:2083`, `app/chamados/juizes.py:1056`)

5. **Concorrência ou relações já carregadas.** A função consulta PV e depois salva os três campos; não recarrega a célula nem a pergunta entre esses dois pontos. Assim, uma alteração externa entre a leitura e o save é uma possibilidade lógica, não uma ocorrência demonstrada. (`app/chamados/reconciliador.py:398`, `app/chamados/reconciliador.py:407`; ocorrência: [nao verificado])

Há ainda uma limitação de observação: o memo inclui veredito/via da célula, mas não ata, DNA, tranca ou batidas. Uma instância já carregada pode reutilizar resultado antigo se apenas esses insumos mu

[truncado em 20000 de 21079 caracteres]

## 29/10 16:58 · sessao 949a9b64a5bb · - · - · origem=Ronald (tmux)

1. **Defeito novo confirmado — ordem de trava na materialização.** `travar_origem_antes` prende C1 antes de J1; outra transação prende C2 antes de J2 ([justificativa.py:262-264](ponto/services/justificativa.py:262)). Depois, ambas percorrem a cascata que inclui C1 e C2, sem excluir os chamados previamente presos ([justificativa.py:313-324](ponto/services/justificativa.py:313)); `encerrar()` escreve por `_gravar_estado`, cujo `save()` executa a atualização ([models.py:1192-1201](chamados/models.py:1192), [models.py:226-231](chamados/models.py:226)).

   Contraexemplo: mesmo colaborador/dia; C1 `turno_aberto`, C2 `turno_aberto_24h`, ambos vivos; J1/J2 aprovadas, com entradas distintas e origens respectivas, ambas sem `batida_gerada` — configuração admitida pelas restrições ([ponto/models.py:186-192](ponto/models.py:186)). Duas saídas preexistentes, distintas e desvinculadas permitem que `registrar_batida` retorne pelo dedup, sem disparar novos signals de batida ([registro_batida.py:107-115](ponto/registro_batida.py:107)). Sem célula, o receiver de J não julga nada ([signals.py:161-165](ponto/signals.py:161), [cartorio.py:1547-1549](ponto/services/cartorio.py:1547)). T1 fica esperando C2; T2 fica esperando C1: deadlock. A pré-trava introduz o ciclo entre cascatas que percorrem o mesmo queryset na ordem padrão `-criado_em` ([chamados/models.py:100](chamados/models.py:100)).

2. **Correção incompleta — origem lida sem proteção.** O helper lê `chamado_origem_id` antes de travar J ([justificativa.py:47-49](ponto/services/justificativa.py:47)). Existe escritor real que preenche origem anteriormente nula ([views.py:1621-1633](ponto/views.py:1621)). Contraexemplo: aprovador lê origem NULL; colaborador vincula C e salva; aprovador trava J e depois tenta C ([justificativa.py:60-63](ponto/services/justificativa.py:60), [justificativa.py:134](ponto/services/justificativa.py:134), [justificativa.py:32](ponto/services/justificativa.py:32)); reconciliador já segura C e espera atualizar J ([reconciliador.py:455-468](chamados/reconciliador.py:455)). Reaparece J→C contra C→J.

3. **Lote: corrida continua produzindo erro para item já decidido.** Precheck pendente ocorre antes do lock ([views.py:1780](ponto/views.py:1780)); após esperar outra aprovação terminar, o `get(status='pendente')` levanta e conta erro, em vez de `ja_decididas` ([views.py:1792-1793](ponto/views.py:1792), [views.py:1831-1848](ponto/views.py:1831)). O estado continua idempotente; a resposta acusa falha. A janela check/get já existia, portanto não é um defeito exclusivamente introduzido pelo diff. Os testes de tela velha e idempotência são sequenciais ([test_bug141_lote_justificativa_ja_decidida.py:33-46](ponto/tests/test_bug141_lote_justificativa_ja_decidida.py:33), [test_justificativa_encerra_origem.py:112-123](ponto/tests/test_justificativa_encerra_origem.py:112)).

## 29/10 16:58 · sessao 28f3e1ec191c · - · - · origem=Ronald (tmux)

Achados consolidados:

- **Selo ainda não prova posição após persistência**: `_gravacoes` reconhece `Assign`, não `save`; `_chama` só compara linhas e ancestral `atomic`. Contraexemplo aceito: `j.status='aprovada'; encerrar_chamado_de_origem(j); j.save()` dentro do `atomic`. A porta lê a justificativa ainda pendente no banco e pode recusar; depois a aprovação persiste e o chamado permanece vivo. Classe: **selo/RED sem mordida**. Referências: `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:36-38`, `:63-68`; porta e gate em `app/chamados/models.py:440-444`, `:495`. Executei o analisador puro em memória: esse contraexemplo retorna `[]`. Também aceita chamada com outro objeto e chamada em savepoint cuja exceção é engolida, pelos mesmos critérios. `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:45-51`, `:63-68`.

- **Dois testes de rollback podem passar sem atingir a falha injetada**: os mocks não têm `assert_called`; antecipar retorno/erro antes da aprovação deixa J pendente e C vivo e satisfaz as asserções. São `test_falha_do_fechamento_desfaz_a_aprovacao` e `test_LOTE_falha_da_saida_desfaz_o_item_inteiro`. Classe: **selo/RED sem mordida**. `app/ponto/tests/test_justificativa_encerra_origem.py:164-173`, `:184-199`. Isso não prova que o caminho atual esteja pulado; remover somente a costura mantendo os atos anteriores faria esses casos falharem pelas asserções de J pendente. `app/ponto/tests/test_justificativa_encerra_origem.py:168-172`, `:194-198`.

- **Materialização não testa transição nem rollback dessa aprovação**: o fixture já nasce aprovado; mede fechamento da origem, mas mover a chamada para antes do `j.save()` não seria detectado por esse caso. `app/ponto/tests/test_justificativa_encerra_origem.py:215-226`; aprovação real em `app/ponto/services/justificativa.py:298-304`.

- **Negativos melhoraram**: rejeição agora exige `status='rejeitada'`; ponteiro de outra justificativa exige aprovação efetiva e `fechamento_recusado`, além do chamado vivo. `app/ponto/tests/test_justificativa_encerra_origem.py:129-137`, `:149-162`. A trilha usa apenas `.exists()` sem baseline, mas não provei trilha preexistente no fixture: vacuidade concreta por esse motivo **[nao verificado]**. `app/ponto/tests/test_justificativa_encerra_origem.py:159`.

- **Escritores alcançados**: três atribuições literais de aprovação de Justificativa: `app/ponto/services/justificativa.py:122`, `:301`, `app/ponto/views.py:1795`. A outra atribuição literal encontrada é férias, explicitamente allowlisted em `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:25-27`; escritor em `app/ferias/services.py:419`.

- **FORA-DO-ESCOPO: admin genérico continua escritor descoberto**: `JustificativaAdmin` herda diretamente `ModelAdmin`, sem readonly nem override; status tem opção aprovada e campo editável. `app/ponto/admin.py:18-22`; `app/ponto/models.py:130-159`. Seu uso real em produção **[nao verificado]**. O selo declara que não o alcança. `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:12-15`.

- **Não encontrei escritor explícito de status_local fora das portas**: executei somente as funções puras do selo existente, lendo 739 arquivos; resultado `violations=[]`. Os sítios encontrados do Chamado são `_gravar_estado`, `abrir` e `criar`. `app/chamados/models.py:192`, `:231`, `:730`, `:834`; universo e verificação em `app/chamados/tests/test_selo_status_so_pelas_portas.py:322-344`; portas permitidas em `:37-43`. Limites da prova AST continuam declarados em `app/chamados/tests/test_selo_status_so_pelas_portas.py:17-19`.

## 29/10 16:58 · sessao c03300972490 · - · - · origem=Ronald (tmux)

T22 item 1 — fonte `2cc71cc8ac027b864963daf221d770a4a2a2f191`.

**`sem_turno_aberto` não é, sozinho, garantia de pergunta morta:** a porta só carimba quando o juiz já a considera morta (`chamados/reconciliador.py:398-408`), mas o juiz pode posteriormente devolvê-la viva se a célula acusar (`chamados/juizes.py:1053-1057`). A API não exclui pergunta apenas por materialização ou disputa fechada: `chamados/services/pergunta_no_app.py:42-44`.

Censo dos escritores que podem deixar `materializada_em`/`via_resolucao` preenchidos **sem gravar resposta e sem preencher `respondida_em`**:

| Arquivo:linha | Escritor | Condição / resposta e carimbo |
|---|---|---|
| `chamados/reconciliador.py:363-408` | `lavrar_sem_turno_aberto` | Motivo permitido, texto vazio, materialização nula, sem turno aberto no D e juiz dá morta. Escreve só materialização/autor/via. Não testa `respondida_em`: pode preservar carimbo preexistente; com carimbo inicialmente nulo continua nulo. |
| `chamados/reconciliador.py:515-552` | `retratar_perguntas_absolvidas_por_celula` | Célula julgada sem acusação, pergunta sem materialização/validação, sem marcos pendentes (`537-542`), juiz dá morta (`543-544`). Só materialização/autor/via `celula_absolveu`. |
| `chamados/models.py:2131-2134` | `PerguntaDisputa.fechar_por_admin` | Grava `admin_decidiu`, materialização e autor; preserva resposta/carimbo. Chamadores: `chamados/models.py:1502`, `chamados/services/validacao.py:264,292`, `ponto/services/veredito_celula.py:177`. |
| `chamados/models.py:2143-2148` | `fechar_por_competencia_trancada` | Não validada e ainda não nessa via; grava via/materialização/autor; preserva resposta/carimbo. |
| `chamados/models.py:2170-2173` | `encerrar_fantasma` | Grava via/materialização/autor; preserva resposta/carimbo. Hoje chamador de evento em `chamados/reconciliador.py:938-940`. |
| `chamados/services/materializacao.py:509-530` | `_materializar_ausencia_de_resposta`, irmãs mudas | Todas as perguntas `hora` do mesmo dia da disputa sem materialização (`507-508`) recebem materialização e via; só preenche respondida/validada se texto existente (`511-515`); irmã muda fica sem ambos. Via da irmã sem declaração de hora = `ausencia_dia` (`525-527`). |
| `chamados/services/materializacao.py:1042-1053` | `_carimbar_recusa` | Materialização ainda nula e código de recusa `COLISAO`; grava `fato_ocupado`, materialização/autor; não grava resposta/carimbo. Alcançabilidade com texto vazio e carimbo nulo depende do erro recebido; **[nao verificado]** em execução. |
| `chamados/services/materializacao.py:1149-1198` | `_materializar_uma`, ramo sem texto | Grava materialização e via após materializador retornar sucesso e juiz aprovar (`1135-1147`); só preenche respondida se texto já existe (`1190-1193`). Ramo `escolha` chama `_materializar_resposta_escolha` (`1129-1130`), que retorna True sem exigir texto (`1008-1024`): a unidade, se chamada diretamente nesse estado, permite ambos os campos vazios. As filas normais têm guardas próprias; não confundir possibilidade da unidade com caso existente em produção. |
| `chamados/services/materializacao.py:1335-1343` | `materializar_pendentes_aprovadas` | Pergunta da disputa sem materialização, juiz de fato dá True; grava `juiz_aprovou`, materialização/autor, sem resposta/carimbo. Signal de disputa fechada a chama em `chamados/signals.py:118-135`. |
| `chamados/services/disputa_emissao.py:1562-1585` | `resolver_perguntas_por_chamado_resolvido`, ramo principal | Juiz da pergunta dá morta (`1547-1548`), dia não trancado (`1550-1551`); grava via pelo fato/materialização. Só carimba respondida se texto existe (`1570-1573`): vazia segue sem carimbo. |
| `chamados/services/fio.py:844-850` | `retratar_duplicata` | Grava via `duplicata`; se materialização nula, preenche materialização/autor. Preserva resposta/carimbo. Chamadores: `chamados/services/fio.py:892`, comando `chamados/management/commands/dedup_perguntas_fato.py:88`. |

Escritores próximos que precisam ser distinguidos:

- `chamados/services/disputa_emissao.py:1643-1651`, ramo sem credor de `resolver_perguntas_por_chamado_resolvido`, **preenche `respondida_em` se nulo mesmo com texto vazio**. Portanto produz materialização sem texto, mas não preserva `respondida_em` nulo.
- `chamados/services/materializacao.py:858-868`, recusa de paridade, grava materialização/via sem escrever resposta/carimbo naquela operação; porém o ramo `hora` exige texto antes em `769-776`, e exige próprio marco aceso em `840-857`. Não é caminho novo de pergunta totalmente muda.
- `chamados/models.py:2158-2159`, `encerrar_por_fato_no_espelho`, escreve **apenas via**; não fabrica materialização.
- `chamados/services/relavra_via.py:123-126`, `relavrar`, altera via/veredito, preservando materialização/resposta/carimbo.
- `chamados/services/regularizacao_ext.py:198` grava `materializada_em` de **outro modelo**, RegularizacaoExterna.

`sem_turno_aberto`: origem, condição e alcance

1. Motivos permitidos = `orfao_14h` e `saida_sem_entrada`, em `chamados/reconciliador.py:332`. Intervalos ficam fora por essa enumeração.
2. A porta recusa motivo diferente (`363-364`), qualquer texto `.strip()` (`365-366`), pergunta já materializada (`367-368`) e turno aberto naquele dia (`373-374`).
3. D recebido prevalece; sem D usa `p.data_evento` (`370-372`). `_tem_turno_aberto` com D delega a `turnos_abertos_de`; sem D consulta global `TurnoMaterializado` com entrada e sem saída (`161-178`).
4. Depois exige `pergunta_viva(p) == False` (`398-400`) e salva apenas os três campos `materializada_em/materializada_por/via_resolucao` (`402-408`). **Não passa por nova escrita/julgamento da célula nessa porta.**
5. Pode fechar a disputa pela porta dela, se aberta e `pode_fechar()` (`413-415`).
6. Chamador direto encontrado: `reatar_pergunta_orfa`, em `1342`. Só chega ali quando não achou candidato vivo (`1300-1302`) nem histórico correspondente (`1327`); D vem de `p.data_evento`, com fallback contexto, parseado em `1311-1318,1335-1338`.
7. Universo normal do reatador: destino nulo, disputa aberta, materialização nula e motivo reatável (`1248-1254`); laço chama unidade em `1268-1272`. Logo o uso normal é órfã; a função isolada aceita pergunta com destino, porque não tem guarda de destino (`363-374`).
8. `reconciliar_chamados_colab` roda primeiro absolvição pela célula e depois órfãs (`1195-1196`); a ordem faz célula absolvida receber `celula_absolveu` antes da alternativa `sem_turno_aberto`.
9. Chamadores efetivos de `reconciliar_chamados_colab`: signal de batida `ponto/signals.py:214-215`; comando `chamados/management/commands/reconciliar_chamados.py:52`; cron configurado a cada 15 min em `config/crons.py:376`. Portanto o comentário “HOJE SO O CRON A CHAMA” em `reconciliador.py:1283-1286` não descreve integralmente essa cadeia indireta, pois o mesmo laço é chamado pelo signal.

Vitalidade depois da lavra

- Validação existente mata primeiro (`chamados/juizes.py:1006-1008`); tranca mata antes da resposta (`1010-1016`).
- `sem_turno_aberto` não está nas vias que encerram por identidade (`1038-1045`). Célula acusadora devolve True (`1053-1057`), salvo degrau anterior de próprio marco aceso com materialização (`1050-1052`).
- Célula sem acusação e sem marcos pendentes mata (`1062-1065`); sem isso, lei de falta ainda pode vivificar (`1066-1075`).
- Último degrau consulta lastro quando há materialização (`1076-1087`).
- `sem_turno_aberto` está em `VIAS_SEM_TESTEMUNHA` (`chamados/models.py:2528-2532`), mas o juiz do lastro também pergunta à lei: **não** integra `_ABSOLVEM_SEMPRE` (`chamados/juizes.py:611-615`); dentro da janela, `classificar_falta` acusando devolve False ao lastro (`618-624`). Fora da janela, aceita a via (`616-617`). Isso explica por que “carimbada” não significa necessariamente “morta”.

Fio e API

- API `api_disputa_perguntas` usa `perguntas_do_app` em `api/views.py:2603-2604`; esse coletor busca todas as disputas do colab, pré-filtra `q_vivas` e confirma no juiz (`chamados/services/pergunta_no_app.py:42-44`). **Não filtra disputa aberta nem materialização nula.**
- `q_vivas` aceita célula acusadora mesmo com carimbo; não inclui sem_turno_aberto em identidade (`chamados/models.py:2243-2262`). Por isso legado dessa via pode aparecer na API se o juiz o der vivo.
- A API separa `RESPONDIDA` das demais em `api/views.py:2620-2622` e envia texto/carimbo em `2632-2634`; pergunta muda com carimbo de materialização não é eliminada só por isso.
- Fio do admin inclui perguntas de disputas fechadas via `q_vivas` e juiz (`chamados/services/fio.py:212-235`); do colaborador com `so_janela=True`, o universo de fechadas é excluído em `212`.
- A ação no fio exige pergunta viva e com ação possível (`chamados/services/fio.py:279-282`); `pergunta_sem_acao_possivel` consulta lastro/veredito, não carimbo cru (`chamados/services/raiox.py:151-165`). Render final dos templates não li.
- Há porta que desfaz carimbo sem lastro preservando resposta/validação: só atua em materializada não identitária que o juiz dá viva (`chamados/services/relavra_via.py:195-209`).

Buscas realizadas: `rg` de campos/constantes/chamadores e AST de atribuições em Python operacional da árvore inteira. Também procurei updates/dicts: apagadores encontrados em `chamados/models.py:1162-1163` e `chamados/services/disputa_emissao.py:1360` não são produtores de materialização. Testes/migrations e execução real não li/rodei; escritores dinâmicos sem nomes literais de campos e SQL externo: **[nao verificado]**.

## 29/10 16:58 · sessao df5ef1ecfe7e · - · - · origem=Ronald (tmux)

T22 item 3 — fonte `2cc71cc8ac027b864963daf221d770a4a2a2f191`. Caso: pergunta com `chamado_destino=NULL` e `disputa.chamado` terminal. É preciso separar **chamado terminal** de **disputa fechada**, pois os seletores não tratam ambos igualmente (`services/fio.py:181-235`; `api/views.py:1028-1048`).

**Juiz, estado e crédito**

| Camada | Evidência |
|---|---|
| Quem recebe crédito no fio | `chamados/services/fio.py:97-117`: `chamado_que_lista` retorna destino, ou chamado pai da disputa. Não verifica status do pai/destino nem fechamento da disputa. No caso proposto, devolve o pai terminal. |
| Vitalidade da pergunta | `chamados/juizes.py:1005-1087`: validação, tranca, resposta, decisão administrativa, identidade, fato do marco, célula, janela/lastro. Não há veto geral “pai terminal→morta” ou “disputa fechada→morta”. Vitalidade efetiva da pergunta nominal: [não verificado], sem banco. |
| Pré-filtro SQL | `chamados/models.py:2222-2260`: `q_vivas` é deliberadamente mais generoso que o juiz; não filtra status do chamado pai nem `disputa.fechada_em`. |
| `estado` da pergunta | `chamados/models.py:2101-2115`: traduz validação/materialização e `pergunta_viva`; pode retornar PENDENTE/RESPONDIDA mesmo no último fallback. Portanto “estado=PENDENTE” sozinho não prova `.viva`. |
| Perguntas vivas de disputa | `chamados/juizes.py:1097-1099`: percorre perguntas e chama juiz; não exige disputa aberta. |

**Onde aparece ou desaparece**

| Canal | Filtro/crédito e efeito no caso |
|---|---|
| Central administrativa `/chamados/` ou `/chamados/painel/` | Rotas `chamados/urls.py:9,22`; view `chamados/views.py:721-738` delega ao painel. Lista padrão filtra chamados `COBRANCA` (`services/painel.py:307-314`); **pai terminal não aparece na lista padrão**, independentemente da pergunta viva. Histórico inclui HISTORICO301, cancelados têm ramo próprio303, status explícito305. |
| Fila/contador central | `chamados/services/painel.py:186-188`: exige VIVOS→COBRANCA e exclui silenciamento futuro. Pai terminal não ganha crédito por carregar pergunta viva. |
| Fio administrativo por colaborador | `chamados/views.py:676-677` chama `montar_chamados_fio` sem janela; carrega **todos os chamados** do colaborador (`services/fio.py:173-176`). Pergunta destinoNULL é anexada ao pai pelo juiz236-249, inclusive pai terminal. |
| Modal fio administrativo | Rota `chamados/urls.py:39`; view `chamados/views.py:869-889` exige gestor e usa mesmo montador. Template exibe `chamado.perguntas_disputa` sem exigir status vivo (`templates/chamados/partials/modal_fio.html:154-157`). Portanto **pode aparecer sob pai terminal**. |
| Se disputa está aberta | Montador busca todas as perguntas das disputas abertas (`services/fio.py:218,222-228`), sem filtro `.viva` nesse ramo; elas são exibidas como histórico/estado. |
| Se disputa está fechada | Admin inclui disputa fechada, usa `q_vivas` e depois juiz exato (`services/fio.py:213-235`). Só pergunta viva sobrevive nesse ramo; anexa ao pai terminal236-249. `em_disputa_fechada=True`232; template retira Rejeitar/Corrigir, não a pergunta inteira (`modal_fio.html:242,282-308`). |
| Retrato da folha/ferramenta | `folha/fora_da_folha.py:51-55,66-73`: pergunta é selecionada **por célula**, e `.viva` decide fio. Pai terminal/destinoNULL não impedem fio `colab` ou `admin` se pergunta tem célula retentora. Chamado vivo só é consultado quando não há pergunta viva. Sem `celula_id`, a pergunta não entra nesse mapa. |
| `/api/disputa/perguntas/` | Rota `api/urls.py:92`; `api/views.py:2584-2604` usa `perguntas_do_app`. Serviço carrega perguntas do colaborador, `q_vivas` e juiz, **sem exigir disputa aberta, destino ou pai vivo** (`services/pergunta_no_app.py:39-44`). Se viva, inclui o caso. |
| Crédito na resposta `/api/disputa/perguntas/` | Por pergunta manda `chamado_id=p.chamado_destino_id` — **NULL** no caso (`api/views.py:2622-2627`). No topo escolhe destino de pendente, ou `disputa.chamado_id` — pode devolver **pai terminal** (`:2642-2644`). Não usa `chamado_que_lista` nesse serializador. |
| `/api/chamados/v2/` | Rota `api/urls.py:73`; `api/views.py:1015-1016` usa `chamados_visiveis_colab`. O chamado pai precisa estar na janela atual, não silenciado e não módulo interno (`services/janela_colab.py:67-74`). **Não há exclusão geral de terminal nessa query**. |
| Exceção à janela na v2 | `janela_colab.py:61-69` inclui fora da janela somente IDs de **destino não NULL** com `q_vivas`. Pergunta destinoNULL **não resgata seu pai fora da janela**, mesmo que viva. |
| Pergunta dentro da v2 | `api/views.py:1028-1048`: só disputas `fechada_em IS NULL`; destino NULL cai no pai. Assim disputa aberta+pai terminal ainda pode aparecer **se pai sobrevive ao recorte de chamados**. Disputa fechada exclui a pergunta da v2, ainda que `/disputa/perguntas/` a entregue. |
| Serialização v2 | Percorre somente chamados selecionados (`api/views.py:1121-1122`); lista perguntas por `perguntas_por_chamado[c.pk]` (`:1158-1177`). Pergunta indexada em pai fora do recorte não gera card independente. |
| `/api/chamados/` v1 | Não lista: devolve HTTP410 e redirecionamento informativo à v2 (`api/views.py:888-893`; rota `api/urls.py:70`). |
| PWA `/chamados/meu-atendimento/` | `chamados/views.py:559-569` usa `montar_chamados_fio(...,so_janela=True)`. Tem recorte do `janela_colab` (`services/fio.py:169-172`) e **não inclui perguntas de disputas fechadas** (`:213`). Em disputa aberta, destinoNULL cai no pai117/236; pai fora da janela não é resgatado pela exceção que exige destino não NULL (`janela_colab.py:61-69`). |

**Resultado comprovado da classe**

- A pergunta pode estar **viva pelo juiz** e ficar sob um **pai terminal pelo crédito**, sem contradição interna do juiz: são perguntas diferentes (`juizes.py:1005-1087`; `services/fio.py:117`).
- Pode aparecer no modal administrativo e no endpoint de perguntas, mas ficar fora da Central padrão e da lista v2/PWA por filtros de chamado/janela ou fechamento de disputa (`services/painel.py:314`; `services/fio.py:173-235`; `pergunta_no_app.py:42-44`; `janela_colab.py:61-74`).
- `/disputa/perguntas/` e `/chamados/v2/` **não têm o mesmo universo**: o primeiro seleciona por pergunta viva; o segundo exige que um card de chamado passe pelo recorte e indexa perguntas somente de disputas abertas (`pergunta_no_app.py:42-44`; `api/views.py:1016,1028-1048,1122,1177`).
- Como o APK renderiza campos `chamado_id=NULL` por pergunta e pai terminal no topo: **não li / [não verificado]**. O código servidor prova a resposta servida, não o comportamento final do cliente (`api/views.py:2626,2642-2644`).

## 29/10 16:57 · sessao 41a4d35fbf65 · - · - · origem=Ronald (tmux)

T22 itens 2/4 — fonte `2cc71cc8ac027b864963daf221d770a4a2a2f191/app`, somente leitura.

**Nascimento em disputa existente**

| emissor / linha | como preenche destino; pai terminal |
|---|---|
| `chamados/services/disputa_emissao.py:280,389-396` `gerar_perguntas_disputa` | Destino derivado por `_derivar_chamado_destino` (`:379-383`), ou None. Derivador procura **outro chamado VIVO do colaborador**, motivo atendido, dia compatível (`:181-218`); não usa automaticamente `disputa.chamado`. Assim disputa existente com pai terminal pode receber pergunta nova destinada a chamado vivo ou sem destino; não há guarda do pai no nascimento lido. |
| `chamados/services/disputa_emissao.py:756-779` `_criar_pergunta_se_couber` | Recebe destino argumento; só barra ausência e pergunta já existente (`:763-766`); não verifica status do pai da disputa nem do destino nessa função. |
| `chamados/services/disputa_emissao.py:783-815` `_complementar_marcos_faltantes` | Para cada marco faltante do mesmo dia, delega à anterior com destino recebido (`:806-813`). |
| `chamados/services/disputa_emissao.py:847-1051` agrupador de órfãos | Pode complementar disputa de pergunta existente (`:864-881`), ou reutilizar disputa aberta; destino é chamado do grupo/canônico/sem_origem (`:956,:978,:1012-1018,:1043-1049`). Não presume pai=destino. Status terminal do pai reutilizado não é conferido nos ramos lidos. |
| `chamados/services/disputa_emissao.py:1093,1121-1124,1154-1167` `criar_disputa_ancora_de_chamado` | Reutiliza disputa aberta e cria pergunta com destino=`chamado_limbo`. Barra **limbo** em ENCERRADOS (`:1107-1110`), mas não verifica pai da disputa existente. |
| `chamados/services/declaracao_texto.py:257,307-323` `declarar_hora_colab` | Reutiliza disputa via `disputa_para_declaracao`; nova pergunta recebe destino=`ch`, dia casado. Pergunta reutilizada sem destino reaponta em `:311-317`. Pai da disputa existente pode permanecer terminal: carregador só filtra `fechada_em is null` (`disputa_emissao.py:467-469`). |
| `chamados/services/declaracao_texto.py:589,605-612` `aplicar_declaracao` | Mesmo carregador; nova pergunta recebe `ch`, dia da proposta. Sem guarda de status do pai no trecho de criação. |
| `chamados/services/acoes_disputa.py:252-266` reabertura administrativa | Reabre disputa fechada do próprio chamado e cria pergunta **sem destino e sem data explícita** (`:259-264`); depois tenta reabrir chamado se ENCERRADOS (`:265-266`). Se reabrir retorna False, retorno é ignorado nesse trecho. |

O carregador comum reutiliza disputa por colaborador+`fechada_em is null`, sem filtrar `disputa.chamado.status_local`, em `chamados/services/disputa_emissao.py:466-502`; só ramo sem disputa abre/obtém pai (`:504`).

A porta `PerguntaDisputa.abrir` não exige disputa nova, pai vivo, destino vivo ou pergunta viva (`chamados/models.py:2028-2086`). Com data, deduplica fato `(colab,motivo,data)` e pode devolver existente em outra disputa (`:2061-2084`); sem data, trata conversa, coloca hoje e nasce sempre (`:2052-2057`). Confere compatibilidade dia/turno do destino explícito (`:2043-2051`), não vitalidade.

Chamadores da geração vistos na busca:

- `perguntar_entradas_pendentes` → `gerar_perguntas_disputa`, `disputa_emissao.py:240,274`.
- `criar_ou_atualizar_disputa` → geração em dois ramos, `disputa_emissao.py:570,606,618`.
- fábrica `perguntar` → criar/atualizar e gerar, `fabrica_da_celula.py:98,108-110`.
- comando de smoke também gera, `management/commands/smoke_chamados.py:102`; corpo não li.
- Complementador → helper, `disputa_emissao.py:813`; agrupador chama helper/complementador em `:881,:956,:978,:1012,:1017,:1043,:1048`.

**Portas de reapontamento e TODOS os callers encontrados pela busca textual**

1. `chamados/models.py:2028,2064-2083` `PerguntaDisputa.abrir` também reaponta pergunta existente quando destino explícito muda; atualiza célula e relavra. **Não exige destino vivo nem pergunta viva.**
2. `chamados/services/fio.py:740,785-801` `reapontar_resolvedora`:
   - se data atual ausente, recebe data antes do juiz;
   - `resolvedora_valida` confere turno;
   - grava FK destino e relavra no mesmo save;
   - **não exige pergunta viva nem chamado vivo**.
3. `chamados/services/fio.py:854,882-900` `migrar_resolvedoras_vivas` filtra origem por `pergunta_viva`; deduplica conflito por motivo+data, escolhe canônica; chama reapontador. Só caller encontrado: `chamados/models.py:1068-1072`, recusa de reabertura porque irmão vivo cobre turno.

Callers diretos do reapontador:

| arquivo:linha | situação |
|---|---|
| `chamados/services/fio.py:896` | Migração acima; novo vem do caller. |
| `chamados/services/declaracao_texto.py:311-317` | Pergunta reutilizada sem destino recebe chamado da declaração. Não há filtro `pergunta_viva` nesse trecho. |
| `chamados/reconciliador.py:750` | `reemitir_chamado_do_dia`; depois de abrir/deduplicar, **exige chamado resultante em VIVOS** em `:733-737`. |
| `chamados/reconciliador.py:1364` | Ramo histórico de `reatar_pergunta_orfa`: explicitamente destino ENCERRADO (`:1316-1319`); depois chama resolvedor das perguntas por chamado resolvido (`:1368-1370`). Não é reapontamento a vivo. |
| `chamados/reconciliador.py:1376` | Ramo candidato vivo derivado de `_derivar_chamado_destino`, `:1300-1301`. |

Callers dos caminhos de reatamento:

- `reemitir_chamado_do_dia` só chamado em `reconciliador.py:1344`.
- `reatar_pergunta_orfa` chamado no laço `reconciliador.py:1271` e no nascimento `disputa_emissao.py:409`.
- `reconciliar_perguntas_orfas_colab` chamado por `reconciliar_chamados_colab`, `reconciliador.py:1196`.
- Universo do laço: destino NULL, disputa aberta, materializada_em NULL e motivo elegível, `reconciliador.py:1248-1254`. **Não alcança pergunta já apontada para pai/destino terminal.**

Compatibilidade não é estritamente “mesmo dia civil”: `chamados/juizes.py:1721-1736` aceita dia igual ou diferença de um dia se turno cruza meia-noite; ausência de informação também pode permitir (`:1703-1716,1732-1735`). Nenhuma dessas portas, isoladamente, garante conjuntamente **pergunta viva + destino vivo + mesmo dia civil**.

Busca feita no app Python inteiro, excluindo testes/migrations, por `PerguntaDisputa.abrir`, `objects.create/get_or_create/update_or_create/bulk_create`, atribuições `chamado_destino`, `reapontar_resolvedora`, `migrar_resolvedoras_vivas`. Criação via aliases dinâmicos/managers não textuais `[nao verificado]`; testes/migrations/templates não li. [nao verificado]

_169 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
