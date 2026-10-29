# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 17:50** · publicado em **29/10/2026 17:50** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 17:50 · sessao 5ecb2ed6553f · - · - · origem=Ronald (tmux)

Nenhum achado verificável de classe 7 no censo dos chamadores das assinaturas alteradas.

O aparente chamador sem `agora=` em `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:53` recebe o argumento via `**kw`, preenchido em `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:52`.

Os chamadores de `Command.apurar` passam `agora` em `app/ponto/management/commands/pautas_discordantes.py:112` e `app/ponto/tests/test_o320_pauta_dos_discordantes.py:189`.

## 29/10 17:48 · sessao edaf1101b86e · - · - · origem=Ronald (tmux)

Refinamento confirmado: a célula do contraexemplo deve estar elegível para julgamento, por exemplo `julgada_em=None` imediatamente antes do POST; assim o pulo por impressão igual não ocorre (`app/ponto/services/cartorio.py:1092`).

Entrada concreta: escala E07/S19, E07 real, C vivo cobrando S19, J própria pendente e pergunta S19 respondida sem validação/materialização; célula de trabalho criada após os saves preparatórios. Ao aprovar, o signal de J julga (`app/ponto/signals.py:161`–`:165`), a grade marca justificado (`app/escala/utils.py:1198`–`:1203`), a ata acende S19 por dispensa (`app/ponto/services/cartorio.py:602`–`:610`) e o cartório chama `reconciliar()` diretamente (`:1163`). O helper encontra C terminal e não consulta S128 (`app/ponto/services/justificativa.py:37`–`:39`). A proteção das irmãs fica contornada.

O savepoint S143 inclui corretamente `get_user_sistema()` (`app/chamados/signals.py:91`–`:92`), com captura após a saída do atomic (`:93`–`:94`).

Existe risco estrutural residual de banco engolido no cartório: o receiver usa `engolir` sem savepoint (`app/chamados/signals.py:171`–`:180`; `app/core/observ.py:85`–`:91`), e pode alcançar `ressuscitar_perguntas` (`app/ponto/services/cartorio.py:1271`), cujo `update` não tem atomic próprio (`app/chamados/models.py:1162`–`:1163`). Uma reprodução de aprovação retornando sucesso com C vivo nesse caminho específico ficou [nao verificado]; não acrescentaria como defeito demonstrado do diff.

Não identifiquei segundo juiz autônomo na guarda J2: ela impede decidir outra justificativa (`app/ponto/services/justificativa.py:40`–`:45`), enquanto S128 usa “pendente” para executar auto-rejeição (`app/chamados/reconciliador.py:462`–`:468`). Não há no dado contrato exigindo trilha para esse veto [nao verificado].

## 29/10 17:47 · sessao 1e7f75cfae7d · grupo1p1 · verificador · origem=Code

- **Classe 3 — espelho perde o instante na fonte dos totais.** `app/ponto/services/espelho.py:1288` chama `folha_manda` sem repassar `agora`. Ela consulta turno vivo e véspera com novos relógios em `app/relatorios/cartao_pela_celula.py:398` e `app/relatorios/cartao_pela_celula.py:412`, acionando `app/ponto/turnos.py:2255` e `app/relatorios/cartao_pela_celula.py:40`.

- **Classe 3 — fila e simulação usam relógios diferentes.** `app/ponto/services/flip_auto.py:263` captura `agora`, mas a fila chamada em `app/ponto/services/flip_auto.py:268` lê novamente em `app/ponto/services/flip_auto.py:34` e `app/ponto/services/flip_auto.py:126`.

- **Classe 3 — ficha ainda lê novamente para classificar pendências.** Após capturar o instante em `app/colaboradores/services/ficha.py:117`, chama `particionar_verbos` em `app/colaboradores/services/ficha.py:155`; essa função lê outro instante em `app/chamados/services/verbos.py:32`, usado para silêncio e idade em `app/chamados/services/verbos.py:48` e `app/chamados/services/verbos.py:54`.

- **Classe 3 — PDF auditável mantém segundo relógio.** O instante capturado em `app/relatorios/pdf_espelho.py:1088` não acompanha a chamada em `app/relatorios/pdf_espelho.py:1099`. O pipeline captura outro em `app/relatorios/pdf_base.py:133`, usado no rodapé em `app/relatorios/pdf_base.py:139`.

- **Classe 3 — reconciliação relê no ramo de aplicação.** O comando captura o instante em `app/ponto/management/commands/reconciliar_grade.py:75`, mas chama `_relavrar_orfa` em `app/ponto/management/commands/reconciliar_grade.py:228`, que lê novamente para `relavrada_em` em `app/ponto/management/commands/reconciliar_grade.py:458`.

- **Classe 5 — selo aceita restaurar leitura interna.** O detector em `app/escala/tests/test_selo_grade_pede_o_instante.py:57` apenas exige `agora=` diferente de literal `None`. Trocar a propagação em `app/escala/utils.py:2277` por `agora=timezone.now()` mantém essa verificação aprovada; a asserção em `app/escala/tests/test_selo_grade_pede_o_instante.py:78` não distingue os dois casos.

- **Classe 8 — migração muda os cenários temporais dos testes.** `app/escala/tests/test_f1c_contrato.py:36` injeta 2100 numa janela construída em torno do hoje real em `app/escala/tests/test_f1c_contrato.py:34`; o contrato usa 2100 como hoje em `app/escala/utils.py:2272`, retirando o cenário futuro que a asserção pretende verificar. Também `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:206` injeta 2100 no cenário congelado em julho/2026 de `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:328`: o teto do coletor em `app/relatorios/pdf_espelho.py:287` passa a admitir a competência inteira, embora `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:330` exija excluir o dia corrente.

Total por classe: 1=0; 2=0; 3=5; 4=0; 5=1; 6=0; 7=0; 8=1.

## 29/10 17:47 · sessao b48a37d3b9a3 · - · - · origem=Ronald (tmux)

Achados estáticos para consolidar; execução concorrente em banco `[nao verificado]`.

1. **F3 permanece — ordem de trava C → TurnoMaterializado contra TurnoMaterializado → C.** A aprovação trava chamados em `app/ponto/services/justificativa.py:89`, trava J em `:101`, e registra saída em `:147`; o signal recomputa turnos em `app/ponto/signals.py:34`, com DELETE/INSERT de `TurnoMaterializado` em `app/ponto/nucleo.py:57`. O concorrente `veredito_celula` mantém atomic em `app/ponto/services/veredito_celula.py:203`, registra batidas em `:300`, recomputa em `:307`, e depois fecha chamados em `:324`, via `:182`. Escalonamento: B segura TM; A segura C; A espera TM; B espera C. A pretrava nova não abrange TM (`app/ponto/services/justificativa.py:84`).

2. **F5 permanece no ramo que registra saída — guarda J2 chega depois do efeito.** Entrada concreta: C `turno_aberto` do dia D, sem CelulaDia, aponta J2 pendente de D+1; J1 de D aponta C e informa entrada/saída. A saída registrada em `app/ponto/services/justificativa.py:147` recomputa TM e dispara reconciliador (`app/ponto/signals.py:214`); sem célula, cartório retorna em `app/ponto/services/cartorio.py:1547`; o reconciliador verifica turno fechado em `app/chamados/reconciliador.py:264`, gate aceita pelo fallback dos turnos em `app/chamados/models.py:978`, e `_retratar` rejeita J2 em `app/chamados/reconciliador.py:462`. A guarda nova só aparece depois, em `app/ponto/services/justificativa.py:44`, mas C já encerrado retorna antes em `:38`. Resultado: aprovar J1 rejeita J2. O teste novo usa wrapper sem batida e não cobre esse ramo (`app/ponto/tests/test_justificativa_encerra_origem.py:277`, `:284`).

3. **A pretrava não elimina novos chamados ou novo ponteiro depois da consulta.** O ponteiro é lido sem trava em `app/ponto/services/justificativa.py:80`; o conjunto é travado em `:89`. Produção pode preencher origem nula depois: `app/ponto/views.py:1631` ou `app/api/views.py:2055`; a API pode criar wrapper em `app/api/views.py:2032` e apontar reverso em `:2067`. Escalonamento possível: A conclui pretrava; B cria C e liga J; A trava J; R retrata C e espera J (`app/chamados/reconciliador.py:455`, `:468`); A espera C (`app/ponto/services/justificativa.py:37`). O comentário que promete cobrir nascimento concorrente não corresponde ao conjunto fixado pela consulta (`app/ponto/services/justificativa.py:74`). Reprodução `[nao verificado]`.

4. **Possível bypass adicional das irmãs, para confirmação no consolidado.** O `save()` de J precede helper (`app/ponto/services/justificativa.py:167`, `:353`), dispara cartório (`app/ponto/signals.py:161`) e esse chama `reconciliar()` diretamente em `app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`, sem unidade S128. Os testes novos de irmãs não criam CelulaDia (`app/ponto/tests/test_justificativa_encerra_origem.py:299`, `:340`); portanto não medem esse fechamento anterior. Contraexemplo completo com os protestos calculados `[nao verificado]`.

`rejeitar_lote_justificativas` não usa atomic próprio (`app/ponto/views.py:1868`), então não afirmo inversão persistente J → C ali em autocommit. `JustificativaAdmin` continua ModelAdmin editável (`app/ponto/admin.py:18`); atomic interno do framework não está na cópia consultada `[nao verificado]`.

## 29/10 17:47 · sessao 1842696154ae · - · - · origem=Ronald (tmux)

- **Escritor não alcançado: Django admin, FORA-DO-ESCOPO.** `JustificativaAdmin` é um `admin.ModelAdmin` genérico, sem bloqueio/override do save; `status` é editável e aceita `aprovada`. Entrada concreta: admin altera J pendente para aprovada → save genérico sem `encerrar_chamado_de_origem`. O selo admite expressamente essa lacuna. (`app/ponto/admin.py:18`, `app/ponto/models.py:130`, `app/ponto/models.py:159`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`)

- **Censo de atribuições de aprovação:** os escritores de Justificativa encontrados são um a um, materialização e lote; os três chamam o fechamento dentro do atomic. A atribuição de férias é outro modelo. (`app/ponto/services/justificativa.py:164`, `app/ponto/services/justificativa.py:176`, `app/ponto/services/justificativa.py:350`, `app/ponto/services/justificativa.py:377`, `app/ponto/views.py:1795`, `app/ponto/views.py:1838`, `app/ferias/services.py:419`)

- **`status_local`:** executei em memória o censo AST do selo, que percorreu 739 arquivos e encontrou zero violações; isso cobre as formas declaradas pelo próprio selo, com os limites de tipagem/dinamismo registrados nele. As portas permitidas estão na allowlist. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:10`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:322`)

- **Ausência de sinal no teste de J1/J2:** `test_aprovar_J1_nao_rejeita_a_J2_pendente_que_o_chamado_aponta` passaria retirando a chamada do fechamento da aprovação: ele só exige J1 aprovada, J2 pendente e chamado aberto, precisamente o resultado de nunca tentar fechar. Como teste específico da guarda, morde removendo a guarda e mantendo a chamada; como teste da costura, não morde. Classe: **selo/RED sem mordida**, limitada a esse cenário. (`app/ponto/tests/test_justificativa_encerra_origem.py:272`, `app/ponto/tests/test_justificativa_encerra_origem.py:286`, `app/ponto/tests/test_justificativa_encerra_origem.py:289`)

- **Irmãs:** o teste da aprovação exige a trilha `retratacao_retida_irma_orfa`, portanto não passa simplesmente por ausência do fechamento. O teste da cascata exige aprovação efetiva e chamado vivo; removendo só a guarda da cascata, o `encerrar()` passa a fechar e contraria a asserção. Ele ainda passaria removendo todo o fechamento da cascata e da origem. (`app/ponto/tests/test_justificativa_encerra_origem.py:313`, `app/ponto/tests/test_justificativa_encerra_origem.py:315`, `app/ponto/tests/test_justificativa_encerra_origem.py:349`, `app/ponto/tests/test_justificativa_encerra_origem.py:352`, `app/ponto/services/justificativa.py:367`, `app/ponto/services/justificativa.py:369`)

- **Erro de banco S143:** o teste direto exige que o mock seja chamado, retorno verdadeiro, chamado encerrado e uma trilha; está desenhado para distinguir o rollback silencioso. O teste da falha repetida exige chamada e rollback integral, mas, isoladamente, também passaria sem o savepoint S143, pois esse resultado continuaria sendo rollback integral. (`app/ponto/tests/test_justificativa_encerra_origem.py:241`, `app/ponto/tests/test_justificativa_encerra_origem.py:250`, `app/ponto/tests/test_justificativa_encerra_origem.py:254`, `app/ponto/tests/test_justificativa_encerra_origem.py:261`, `app/ponto/tests/test_justificativa_encerra_origem.py:267`)

- **Teste UPDATE do carimbo:** injeta uma data inválida, não uma falha emitida pelo servidor; só exige que `timezone.now` seja chamado e o ato sobreviva. Ausência de `FechamentoMensal` não basta para declarar vacuidade, pois o UPDATE precisa preparar os valores mesmo sem linhas correspondentes. Se a preparação ocorre dentro de `mark_for_rollback_on_error` na versão instalada, o teste morde sem savepoint; essa implementação Django e a execução com banco ficaram **[nao verificado]**. (`app/ponto/tests/test_justificativa_encerra_origem.py:379`, `app/ponto/tests/test_justificativa_encerra_origem.py:382`, `app/ponto/tests/test_justificativa_encerra_origem.py:385`)

- **Tetos:** o custo acrescentado é por execução do receiver em `post_save` de chamado terminal, pois ele não verifica transição nem `update_fields`; portanto “por chamado que fecha” é uma descrição incompleta. O próprio comentário prevê dois disparos no cenário de um chamado. A alteração é compatível com SAVEPOINT+RELEASE por execução; os números medidos permanecem **[nao verificado]** sem banco. Os tetos usam `<=`, portanto remover o savepoint não os reprovaria; a garantia funcional depende do RED separado. (`app/chamados/signals.py:75`, `app/chamados/signals.py:81`, `app/chamados/signals.py:91`, `app/ponto/tests/test_selo_chokepoint_escrita.py:148`, `app/ponto/tests/test_selo_chokepoint_escrita.py:150`, `app/ponto/tests/test_selo_chokepoint_escrita.py:210`)

- **F7 corrigido:** o selo agora compara o receptor da atribuição, do save e do primeiro argumento do fechamento; há mutação explícita para outro objeto. Executei os dois selos em memória, substituindo somente `SimpleTestCase` por `unittest.TestCase`: 22 testes passaram, sem Django nem banco; execução Django funcional **[nao verificado]**. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:81`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:102`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:103`, `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:207`)

## 29/10 17:46 · sessao edaf1101b86e · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Guarda das irmãs pode chegar tarde.** `j.save()` ocorre antes de `encerrar_chamado_de_origem` (`app/ponto/services/justificativa.py:167`, `:176`; lote `app/ponto/views.py:1798`, `:1833`). O signal da justificativa julga a célula imediatamente (`app/ponto/signals.py:161`–`:165`, `:88`–`:93`). A grade reconhece a justificativa aprovada e marca o dia justificado (`app/escala/utils.py:1157`–`:1159`, `:1198`–`:1203`); o cartório dispensa as lâmpadas (`app/ponto/services/cartorio.py:602`–`:610`) e fecha chamados diretamente por `reconciliar`, sem a guarda S128 (`:1163`, `:1175`, `:1196`).

   Contraexemplo: célula de trabalho com saída S19 apagada; chamado vivo com `marcos_faltantes=[{'hora':'19:00','tipo':'S'}]`; justificativa própria pendente; pergunta respondida “19:00”, não validada/materializada. Aprovar a justificativa acende a lâmpada por dispensa; `marcos_acesos` retorna verdadeiro (`app/ponto/services/lastro.py:170`–`:181`), o gate aceita a justificativa própria aprovada (`app/chamados/models.py:937`–`:938`) e fecha antes da guarda. O helper encontra chamado terminal e retorna (`app/ponto/services/justificativa.py:37`–`:39`). O resolver preserva a resposta ainda não consumida (`app/chamados/services/disputa_emissao.py:1558`–`:1560`). **Classe: pouso antes do pré-requisito / guarda contornada.**

2. **Guarda J2 pendente também pode chegar tarde.** As portas unitária e materialização registram a saída antes de chamar o helper (`app/ponto/services/justificativa.py:147`–`:153`, `:328`–`:335`, `:176`, `:377`). O signal da batida chama o reconciliador (`app/ponto/signals.py:206`–`:215`); ele encontra a premissa de turno morta e chama `_retratar` (`app/chamados/reconciliador.py:264`–`:289`, `:1203`–`:1214`), que auto-rejeita a justificativa relacionada pendente (`:462`–`:468`).

   Contraexemplo: J1 aprovada com saída que fecha o turno; origem C aponta J2 pendente; sem irmãs. A batida resolve C e rejeita J2 antes da guarda `app/ponto/services/justificativa.py:44`. Depois o helper retorna porque C já está encerrado (`:38`). **Classe: pouso antes do pré-requisito / outra: decisão colateral de J2.**

3. **A cascata antecede a guarda J2.** Ela chama `encerrar()` em cada alvo (`app/ponto/services/justificativa.py:361`–`:369`), antes do helper (`:377`). `encerrar` força fechamento sem gate da premissa (`app/chamados/models.py:1192`–`:1202`). Se a origem pertence à cascata e aponta outra justificativa pendente, termina fechada antes da guarda de `:44`; a cascata não auto-rejeita J2, mas contorna o resultado prometido pela guarda. **Classe: pouso antes do pré-requisito.**

4. **Residual de duas trilhas na materialização:** `NAO_FECHADOS` contém `resolvido` (`app/chamados/catalogo/motor.py:32`). A saída pode fazer o signal resolver C antes da cascata; `_cascata_do_turno` volta a selecionar esse C (`app/ponto/services/justificativa.py:61`, `:361`) e `encerrar()` faz `resolvido → fechado` (`app/chamados/catalogo/motor.py:114`–`:115`). São duas transições e duas linhas `estado_chamado` (`app/chamados/models.py:227`–`:231`). Esse mecanismo da cascata já existia no trecho fornecido; apontar como residual preexistente, não atribuir sua introdução ao v6.  

Sobre os efeitos:

- Rebaixamento não captura exceções (`app/chamados/signals.py:68`–`:72`). No caminho normal `_gravar_estado` já normaliza a urgência antes do save (`app/chamados/models.py:210`), de modo que `_rebaixar_sem_post_save` retorna sem update (`:424`–`:426`). Uma exceção propagada nessa execução atravessa o atomic da aprovação (`app/ponto/services/justificativa.py:99`–`:181`).
- S143 agora tem savepoint próprio e captura fora dele (`app/chamados/signals.py:88`–`:94`), portanto o erro direto do resolver fica isolado. S128 chama o resolver novamente sem captura (`app/chamados/reconciliador.py:455`–`:473`); erro repetido desfaz a aprovação inteira.
- O cartório usa `engolir` (`app/chamados/signals.py:171`–`:180`), cujo corpo somente captura, sem savepoint próprio (`app/core/observ.py:85`–`:91`). Suas portas de persistência da célula têm atomic próprio (`app/ponto/portas/celula.py:634`–`:642`). Uma garantia geral de isolamento de todo erro de banco no cartório não foi demonstrada [nao verificado].
- O risco concreto demonstrável pela leitura é **ordem de efeitos síncronos** antes das guardas, acima; ler as próprias alterações ainda não commitadas é necessário para que o gate veja a justificativa aprovada (`app/chamados/models.py:937`–`:938`), não constitui sozinho erro.
- `invalidar_fechamento` protege a coleta da competência e cada update com atomic separado (`app/ponto/services/fechamento.py:1616`–`:1624`), capturando fora (`:1629`–`:1633`). Se falhar na segunda competência, a primeira permanece invalidada e a função devolve zero; é o comportamento documentado no novo comentário (`:1614`–`:1615`), não evidência de aprovação parcialmente gravada.

Não executei banco nem testes; ocorrências reais desses contraexemplos em produção estão [nao verificado]. [nao verificado]

## 29/10 17:46 · sessao c8fc8330748e · - · - · origem=Ronald (tmux)

Achados de classe 3:

- `app/ponto/services/flip_auto.py:263`: `avaliar_fila` captura `agora`, mas `fila_flip` chamado em `:268` provoca outras duas leituras: `pares_mesmo_tipo` em `:34` e `fila_flip_recusas` em `:126`, pelas chamadas em `:146` e `:148`. O universo da fila e as simulações em `:303-304` usam instantes diferentes.
- `app/ponto/services/espelho.py:1288`: não repassa o instante obrigatório a `folha_manda`. Essa fonte chama `turno_aberto_de` sem instante em `app/relatorios/cartao_pela_celula.py:398` e `ate_ontem` sem data em `:412`, acionando relógios em `app/ponto/turnos.py:2255` e `app/relatorios/cartao_pela_celula.py:40`.
- `app/relatorios/pdf_espelho.py:1088`: captura o instante do PDF, mas chama `gerar_pdf_auditavel` em `:1099` sem repassá-lo; o pipeline lê novamente em `app/relatorios/pdf_base.py:133` e usa esse segundo instante nos rodapés em `:139` e `:170`.
- `app/colaboradores/services/ficha.py:117`: captura o instante da ficha, mas a chamada `particionar_verbos(vivos)` em `:155` faz outra leitura em `app/chamados/services/verbos.py:32`, usada para classificar silêncio e idade em `:48` e `:54`.
- `app/colaboradores/services/ficha.py:275`: `ficha_para_o_copiloto` chama a ficha, cuja leitura ocorre em `:117`, e depois lê novamente por `timezone.localdate()` em `:280`.
- `app/ponto/management/commands/reconciliar_grade.py:75`: captura instante do comando, mas `_relavrar_orfa`, chamado em `:228`, lê novamente em `:458` no ramo `apply`. Esse caso afeta o carimbo `relavrada_em`.

Possível classe 5, comprovada em memória:

- `app/escala/tests/test_selo_grade_pede_o_instante.py:58`: o detector só exige a presença de `agora=` e rejeita literal `None` (`:59-60`). `chamadas_sem_instante('grade_espelho_janela(c, bs, i, f, agora=timezone.now())')` retorna `[]`; portanto trocar uma propagação por nova leitura do relógio mantém esse selo aprovado. A execução foi apenas da função AST extraída, com `python3 -B`, sem Django ou escrita.

## 29/10 17:46 · sessao c22e52762040 · - · - · origem=Ronald (tmux)

- **Classe 8 — migração do teste remove seu cenário futuro.** `app/escala/tests/test_f1c_contrato.py:36` passa `DEPOIS_DE_TUDO`, definido como 01/01/2100 em `app/escala/tests/instante.py:12`, embora o dia examinado seja o hoje real + cinco dias (`app/escala/tests/test_f1c_contrato.py:34`). O contrato passa a considerar hoje=2100 (`app/escala/utils.py:2272`), portanto esse dia deixa de ser futuro para o clip (`app/escala/utils.py:942`), contrariando a asserção do teste (`app/escala/tests/test_f1c_contrato.py:39`). Na data fornecida, 03/11/2026 é terça, dia de trabalho da fixture (`app/escala/tests/test_f1c_contrato.py:27`); a montagem mantém as células (`app/escala/utils.py:1749`).

- **Classe 8 — migração do selo corrente muda o período perguntado.** `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:206` passa 2100 ao cartão, inclusive no cenário congelado em 23/07/2026 (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:328`), que exige excluir o dia corrente (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:330`). O coletor usa esse argumento para determinar a véspera (`app/relatorios/pdf_espelho.py:287`) e entrega 31/12/2099 como hoje ao espelho (`app/relatorios/pdf_espelho.py:594`); o espelho monta até essa data (`app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:1266`). O cenário deixou de medir um cartão emitido em 23/07/2026.

Não identifiquei introdução verificável das classes 1, 2, 4 ou 6 nas alterações examinadas. [não verificado]

## 29/10 17:46 · sessao 7aa4ace3aa4f · - · - · origem=Ronald (tmux)

- **Classe 5 (candidato):** `app/escala/tests/test_selo_grade_pede_o_instante.py:57-59` aceita qualquer expressão não literal `None` como instante. Uma chamada com `agora=timezone.now()` passa no selo mesmo após outra leitura de relógio no ato; reprodução em memória de `chamadas_sem_instante` retornou `[]`. O teste de produção apenas exige essa lista vazia (`:74-78`), e os demais verificam assinatura (`:100-111`). Isso permite restaurar relógio interno, por exemplo em `app/escala/utils.py:2277`, sem este selo detectar.
- **Classe 3, se o escopo inclui testes:** `app/ponto/tests/test_par_relampago_pelo_turno.py:55` combina `timezone.localdate()` e `timezone.now()` na mesma chamada. Os testes chamadores congelam o relógio (`:58`), mas o helper continua lendo duas vezes.
- **Problema de migração temporal de teste:** `app/escala/tests/test_f1c_contrato.py:34-36` monta janela em torno do relógio real e injeta o ano 2100. O contrato deriva `hoje` do argumento (`app/escala/utils.py:2272`) e o entrega ao clip (`:2278`); portanto a janela denominada futura (`test_f1c_contrato.py:38-40`) é passada para o clip como passada.

Não encontrei chamadores Python sem migração de `agora`: varredura AST de `app` e `bin` incluiu aliases e testes. A única chamada sem keyword explícita usa `**kw`, previamente preenchido com `agora` (`app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:52-53`).

## 29/10 17:43 · sessao cc944dd21722 · grupo1p1 · verificador · origem=Code

- **Classe 3** — A sonda calcula `HOJE` com `localtime()` sem argumento e depois lê outro instante para `agora`, passando ambos ao mesmo espelho. `bin/r3_turno_aberto_alcanca.py:34`, `bin/r3_turno_aberto_alcanca.py:42`, `bin/r3_turno_aberto_alcanca.py:48`.

- **Classe 3** — O comando captura o instante, mas chama `_ate_ontem` sem `hoje=`, provocando outra leitura de relógio. `app/relatorios/management/commands/diff_pdf_x_espelho.py:50`, `app/relatorios/management/commands/diff_pdf_x_espelho.py:75`; releitura em `app/relatorios/cartao_pela_celula.py:40`.

- **Classe 3** — `avaliar_fila` captura `agora`, mas `fila_flip` consulta duas fontes que leem seus próprios relógios. `app/ponto/services/flip_auto.py:263`, `app/ponto/services/flip_auto.py:268`; chamadas em `app/ponto/services/flip_auto.py:146`, `app/ponto/services/flip_auto.py:148`; releituras em `app/ponto/services/flip_auto.py:34`, `app/ponto/services/flip_auto.py:126`.

- **Classe 3** — A ficha captura `agora`, mas a partição das pendências lê outro instante para vencimento e idade. `app/colaboradores/services/ficha.py:117`, `app/colaboradores/services/ficha.py:155`; releitura e usos em `app/chamados/services/verbos.py:32`, `app/chamados/services/verbos.py:48`, `app/chamados/services/verbos.py:54`.

- **Classe 3** — O PDF formal captura o instante para coleta e corpo, mas não o repassa ao pipeline, que lê outro para o rodapé. `app/relatorios/pdf_espelho.py:1088`, `app/relatorios/pdf_espelho.py:1099`; releitura em `app/relatorios/pdf_base.py:133` e uso em `app/relatorios/pdf_base.py:139`.

- **Classe 3** — O espelho recebe `agora`, mas não o repassa a `folha_manda`; esta consulta o teto e o turno vivo com relógios próprios. `app/ponto/services/espelho.py:1198`, `app/ponto/services/espelho.py:1288`; chamadas em `app/relatorios/cartao_pela_celula.py:398`, `app/relatorios/cartao_pela_celula.py:412`; releituras em `app/ponto/turnos.py:2255`, `app/relatorios/cartao_pela_celula.py:40`.

- **Classe 7** — `Command.apurar` passou a exigir `agora`, mas o chamador de teste continua sem o argumento, causando `TypeError` antes das verificações. `app/ponto/management/commands/pautas_discordantes.py:56`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:188`.

- **Classe 5** — O selo omite `Command.apurar` de `ALVOS`. Retirar `agora=` do chamador de produção não seria detectado pelo censo nem pela inspeção das assinaturas. `app/escala/tests/test_selo_grade_pede_o_instante.py:25`, `app/escala/tests/test_selo_grade_pede_o_instante.py:51`, `app/escala/tests/test_selo_grade_pede_o_instante.py:92`; chamador em `app/ponto/management/commands/pautas_discordantes.py:112`.

Total por classe: 1=0; 2=0; 3=6; 4=0; 5=1; 6=0; 7=1; 8=0.

## 29/10 17:42 · sessao 5694306ca989 · - · - · origem=Ronald (tmux)

- **Classe 5 — censo do selo incompleto:** `app/escala/tests/test_selo_grade_pede_o_instante.py:25` omite `pautas_discordantes.Command.apurar`, embora `app/ponto/management/commands/pautas_discordantes.py:56` torne `agora` obrigatório. Retirar `agora=` do chamador em `app/ponto/management/commands/pautas_discordantes.py:112` passa pelo filtro do selo (`app/escala/tests/test_selo_grade_pede_o_instante.py:51`) e pela inspeção de assinaturas (`app/escala/tests/test_selo_grade_pede_o_instante.py:92`). Confirmei executando somente o AST em memória: `chamadas_sem_instante('self.apurar(mes, ano, empresas, hoje)')` retorna `[]`.

Os casos F1c (`app/escala/tests/test_f1c_contrato.py:36`) e O197 (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:206`) passam 2100 para cenários de dia corrente. Isso contradiz as expectativas de recorte em `app/escala/tests/test_f1c_contrato.py:39` e `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:330`; são testes quebrados pela migração, não exemplos fortes de classe 5 (“passaria com o defeito”).

## 29/10 17:42 · sessao 846b1fa92892 · - · - · origem=Ronald (tmux)

Achados de classe 3:

- `bin/r3_turno_aberto_alcanca.py:34,42,48`: `HOJE` vem de `timezone.localtime()` sem argumento; depois outro relógio fornece `_AGORA_DO_ATO`. A mesma chamada de espelho recebe os dois.
- `app/relatorios/management/commands/diff_pdf_x_espelho.py:50,75`: captura `_agora_do_ato`, mas chama `_ate_ontem` sem `hoje=`. O helper relê `timezone.localdate()` em `app/relatorios/cartao_pela_celula.py:40`.
- `app/ponto/services/flip_auto.py:263,268`: captura `agora`, depois chama `fila_flip`; esta chama `pares_mesmo_tipo` e `fila_flip_recusas` (`:146,148`), que releem relógio (`:34,126`).
- `app/relatorios/pdf_espelho.py:1088,1099`: captura instante para coleta/corpo, depois chama `gerar_pdf_auditavel` sem repasse. O pipeline lê outro instante em `app/relatorios/pdf_base.py:133`, usado no rodapé (`:139,170`).
- `app/colaboradores/services/ficha.py:117,155`: captura `agora`, depois chama `particionar_verbos`. Esta relê relógio em `app/chamados/services/verbos.py:32`, usado em vencimento e idade (`:48,54`).

Cadeia adicional compartilhada, para avaliar o alcance do escopo:

- `app/ponto/services/espelho.py:1288` chama `folha_manda` sem repassar `agora`; `app/relatorios/cartao_pela_celula.py:323,73` chama `totais_da_folha → fechamento_lido`; `app/ponto/services/fechamento.py:1718` chama `recalcular_fechamento_mes`, que lê relógio em `:595`, utilizado no teto dos furos em `:1085`. Isso continua acontecendo dentro do espelho que agora recebe instante obrigatório (`app/ponto/services/espelho.py:1198`).

Um carimbo adicional:

- `app/ponto/management/commands/reconciliar_grade.py:75,228` captura instante e chama `_relavrar_orfa`, que relê relógio em `:458`. É carimbo de escrita; a relevância para o teto do julgamento é [não verificado].

## 29/10 17:41 · sessao 9abcd1c52e8b · - · - · origem=Ronald (tmux)

Classe 7 — `app/ponto/management/commands/pautas_discordantes.py:56` exige `agora` keyword obrigatório, mas `app/ponto/tests/test_o320_pauta_dos_discordantes.py:188` chama `Command().apurar(...)` sem esse argumento. Os quatro testes que passam por `_apurar` (`:197`, `:204`, `:210`, `:216`) recebem `TypeError` antes das respectivas verificações.

Censo por AST de todos os arquivos `.py`, resolvendo aliases de `ImportFrom`, não revelou outro chamador não migrado das assinaturas indicadas. O `**kw` de `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:53` recebe `agora` em `:52`.

## 29/10 17:41 · sessao afda6525cce7 · justif · verificador · origem=Code

**Reprovo a v5:** a costura mantém inversões de trava e o teste novo de erro nas perguntas contradiz o caminho executado. (`app/ponto/services/justificativa.py:94`; `app/chamados/reconciliador.py:469`; `app/ponto/tests/test_justificativa_encerra_origem.py:247`)

Análise estática da cópia e do patch fornecido. Execução dos testes Django, reprodução dos deadlocks, identidade commit/base e ocorrência dos contraexemplos em produção: **[nao verificado]**. [nao verificado]

Para a pergunta **1**, encontrei estes defeitos:

- **F1 — O resolvedor protegido é chamado novamente sem proteção. Classe: outra.** Entrada: aprovação cuja resolução de perguntas sofre erro de banco repetível. O receiver S143 isola e engole a primeira falha, mas `retratar()` chama o mesmo resolvedor novamente, sem captura. Essa segunda falha desfaz a aprovação: J permanece pendente e C vivo, contrariando o comportamento exigido pelo teste novo de best-effort. (`app/chamados/signals.py:88`; `app/chamados/signals.py:91`; `app/chamados/reconciliador.py:455`; `app/chamados/reconciliador.py:469`; `app/ponto/services/justificativa.py:169`; `app/ponto/tests/test_justificativa_encerra_origem.py:247`)

- **F2 — Aprovação × rejeição ainda forma C → J / J → C. Classe: ordem de trava.** Contraexemplo: A inicia aprovação e trava C; B inicia rejeição e trava J; o `j.save()` de B dispara o cartório, que tenta reconciliar C quando o dia/marco está regularizado; A tenta obter J. Resultado: A espera B e B espera A. A pré-trava nova não alcança a rejeição. (`app/ponto/services/justificativa.py:83`; `app/ponto/services/justificativa.py:94`; `app/ponto/services/justificativa.py:95`; `app/ponto/services/justificativa.py:231`; `app/ponto/services/justificativa.py:242`; `app/ponto/signals.py:165`; `app/ponto/services/cartorio.py:1163`; `app/chamados/models.py:231`)

- **F3 — Aprovação × materialização de batida forma C → TurnoMaterializado / TurnoMaterializado → C. Classe: ordem de trava.** Contraexemplo: B executa `veredito_celula('trabalhou')`, registra uma saída e mantém a trava do turno T pelo atomic externo; A trava C e registra outra saída do mesmo dia, esperando o DELETE de T; B continua pelos signals e tenta fechar C, esperando A. Escolhendo timestamps separados por mais de 60 segundos, o dedup não elimina essa execução. (`app/ponto/services/veredito_celula.py:203`; `app/ponto/services/veredito_celula.py:300`; `app/ponto/signals.py:34`; `app/ponto/nucleo.py:57`; `app/ponto/nucleo.py:59`; `app/ponto/services/justificativa.py:94`; `app/ponto/services/justificativa.py:141`; `app/ponto/signals.py:135`; `app/ponto/services/cartorio.py:1163`; `app/ponto/registro_batida.py:106`)

- **F4 — A materialização fecha antes de consultar a guarda das irmãs. Classe: pouso antes do pré-requisito.** Entrada: origem `turno_aberto` com pergunta respondida, não validada e não materializada. A cascata chama `encerrar()`, que força fechamento sem essa guarda; depois, o helper encontra C terminal e retorna `False`. Resultado: aprovação com chamado fechado sobre a irmã pendente. O fechamento forçado é herdado, mas permanece no sítio de cascata explicitamente tocado pelo diff. (`app/ponto/services/justificativa.py:346`; `app/ponto/services/justificativa.py:349`; `app/ponto/services/justificativa.py:357`; `app/ponto/services/justificativa.py:38`; `app/chamados/models.py:1192`; `app/chamados/juizes.py:1471`)

- **F5 — Aprovar J1 pode rejeitar outra J2. Classe: outra.** Contraexemplo permitido pelo modelo: C vivo, módulo `justificativa_colab`, categoria `justificativa`, subcategoria `esqueci_saida`; C aponta J2 pendente no dia D2; J1 pendente no dia D1≠D2 aponta C como origem, sem saída informada. O gate desse chamado não exige regularização de furo; a nova costura fecha C e auto-rejeita J2, acrescentando uma descrição de retratação pelo reconciliador. A aprovação de J1 passa, portanto, a decidir outra justificativa. Dias diferentes respeitam a unicidade das pendentes; a porta de vínculo não valida correspondência de dia. (`app/ponto/models.py:173`; `app/ponto/models.py:189`; `app/chamados/services/vinculos.py:81`; `app/chamados/models.py:915`; `app/chamados/models.py:484`; `app/ponto/services/justificativa.py:40`; `app/chamados/reconciliador.py:462`; `app/chamados/reconciliador.py:468`)

- **F6 — O atomic do lote não trata recusa retornada como `None`. Classe: outra, meio-estado.** Entrada: J com saída informada, competência aprovada/exportada, origem viva e outra justificativa sem batida gerada. `registrar_batida()` retorna `(None, mensagem)`; a consulta `_outra`, feita com `batida_gerada=None`, encontra essa outra justificativa e pula o bloco de vinculação/conflito. O lote prossegue, fecha a origem e conta aprovação, sem saída criada. A ausência de tratamento é herdada no bloco movido; o fechamento da origem é o efeito novo. (`app/ponto/janelas.py:87`; `app/ponto/registro_batida.py:130`; `app/ponto/registro_batida.py:137`; `app/ponto/views.py:1814`; `app/ponto/views.py:1821`; `app/ponto/views.py:1829`; `app/ponto/views.py:1830`)

- **F7 — O selo aceita a costura com outro objeto. Classe: selo sem mordida.** Contraexemplo: dentro do atomic, `j.status='aprovada'; j.save(); encerrar_chamado_de_origem(k)`, sendo `k` sem origem. O selo aceita porque confere o receptor do `save()`, mas reconhece a chamada de fechamento apenas pelo nome e pela posição da linha. Resultado: selo verde com J aprovada e sua origem viva. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:61`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:79`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:102`)

Quanto à pergunta **2**, **há escritor de aprovação não alcançado: o Django admin — FORA-DO-ESCOPO, preexistente**. `JustificativaAdmin` continua editável, e `status` aceita `aprovada`. Alterar J pelo admin, com origem viva e sem célula, salva a aprovação; o signal retorna sem julgar célula inexistente e não executa a costura nova. (`app/ponto/admin.py:18`; `app/ponto/models.py:130`; `app/ponto/models.py:159`; `app/core/admin.py:7`; `app/ponto/signals.py:165`; `app/ponto/services/cartorio.py:1547`)

**Não encontrei escritor de `status_local` fora das portas no universo coberto pelo selo existente.** A costura termina em `reconciliar()` → `_gravar_estado()`, e a cascata também usa uma porta do modelo. Essa conclusão fica limitada às formas que a análise AST resolve; o próprio selo declara limites para escrita dinâmica/genérica. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:17`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:337`; `app/chamados/models.py:444`; `app/chamados/models.py:1201`)

Para a pergunta **3**, os testes têm resultados contrafactuais diferentes: [nao verificado]

- **Os casos positivos das três portas detectam a retirada da costura**, pelas asserções de aprovação e fechamento; o de idempotência também exige uma única trilha. (`app/ponto/tests/test_justificativa_encerra_origem.py:78`; `app/ponto/tests/test_justificativa_encerra_origem.py:91`; `app/ponto/tests/test_justificativa_encerra_origem.py:127`; `app/ponto/tests/test_justificativa_encerra_origem.py:227`)
- **O teste das irmãs tem mordida:** além de manter C vivo, exige J aprovada e trilha de retenção. Remover toda a costura não satisfaz essa trilha. Entretanto, ele cobre aprovação individual de `turno_aberto`, não a cascata do F4. (`app/ponto/tests/test_justificativa_encerra_origem.py:47`; `app/ponto/tests/test_justificativa_encerra_origem.py:262`; `app/ponto/tests/test_justificativa_encerra_origem.py:266`; `app/ponto/tests/test_justificativa_encerra_origem.py:269`)
- **Os testes de rollback evitam passar por retorno antecipado:** exigem chamada efetiva aos mocks. (`app/ponto/tests/test_justificativa_encerra_origem.py:170`; `app/ponto/tests/test_justificativa_encerra_origem.py:197`)
- **O teste de erro nas perguntas deveria falhar inclusive nesta versão**, pelo F1: o mock quebra em todas as chamadas e a segunda não é protegida. (`app/ponto/tests/test_justificativa_encerra_origem.py:237`; `app/ponto/tests/test_justificativa_encerra_origem.py:241`; `app/chamados/reconciliador.py:469`)
- **Os testes de leitura da competência e UPDATE do carimbo exigem aprovação e fechamento**, mas a efetiva mordida contra a retirada de cada savepoint não foi executada: **[nao verificado]**. (`app/ponto/tests/test_justificativa_encerra_origem.py:278`; `app/ponto/tests/test_justificativa_encerra_origem.py:287`; `app/ponto/tests/test_justificativa_encerra_origem.py:296`; `app/ponto/tests/test_justificativa_encerra_origem.py:302`)
- **Rejeição e aprovação sem origem passam sem a cura por serem controles.** O selo também admite explicitamente `if False`, além do falso verde do F7. (`app/ponto/tests/test_justificativa_encerra_origem.py:129`; `app/ponto/tests/test_justificativa_encerra_origem.py:176`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:8`)

Nos **tetos**, a unidade precisa é **por execução de save terminal**, inclusive save sem mudança de status e sem perguntas: S143 filtra apenas `raw` e estado. Dentro de transação, o acréscimo é SAVEPOINT + RELEASE por execução. Os aumentos 49→51, 187→191 e 233→241 pressupõem respectivamente 1, 2 e 4 execuções; o comentário reconhece a repetição, mas “por chamado que fecha” não descreve toda a incidência. Contagens efetivas: **[nao verificado]**. Retirar o savepoint reduz queries e continua passando no teto `<=`; esse selo não prova a proteção funcional. (`app/chamados/signals.py:81`; `app/chamados/signals.py:91`; `app/ponto/tests/test_selo_chokepoint_escrita.py:114`; `app/ponto/tests/test_selo_chokepoint_escrita.py:148`; `app/ponto/tests/test_selo_chokepoint_escrita.py:154`; `app/ponto/tests/test_selo_chokepoint_escrita.py:210`)

Para a pergunta **4**, **as aprovações agora antecipam as travas de chamados**, antes de travar J. No reconciliador citado, a trava de C é adquirida pelo UPDATE, não por `select_for_update`; depois vem o eventual save da justificativa pendente. A ordenação por PK melhora esse par, mas não elimina os ciclos F2/F3. (`app/ponto/services/justificativa.py:83`; `app/ponto/services/justificativa.py:95`; `app/ponto/services/justificativa.py:298`; `app/ponto/views.py:1792`; `app/chamados/reconciliador.py:1203`; `app/chamados/reconciliador.py:1214`; `app/chamados/reconciliador.py:468`)

Existe ainda caminho que **escreve J antes de C**: `regularizar_turno_orfao()` atualiza a justificativa dentro do atomic e depois percorre chamados, chamando `analisar()`. Logo, ordenar somente os três aprovadores não estabelece uma ordem global. (`app/ponto/services/regularizacao.py:89`; `app/ponto/services/regularizacao.py:106`; `app/ponto/services/regularizacao.py:137`; `app/chamados/models.py:572`)

Para a pergunta **5**, **há efeito capaz de desfazer a aprovação**: a segunda resolução de perguntas do F1 e a gravação obrigatória da nota de retratação propagam falhas pelo atomic. O savepoint novo de S143 protege apenas sua própria chamada. (`app/chamados/reconciliador.py:455`; `app/chamados/reconciliador.py:469`; `app/chamados/reconciliador.py:471`; `app/chamados/signals.py:91`; `app/ponto/services/justificativa.py:169`)

O receiver de **rebaixa** não captura exceções; no fechamento normal, porém, o núcleo já normalizou a urgência e ele retorna sem UPDATE adicional. O **cartório** captura exceções por `engolir`, que não cria savepoint próprio. Portanto, captura de exceção não basta para afirmar isolamento de toda falha de banco; ocorrência concreta desse problema nesta obra: **[nao verificado]**. (`app/chamados/signals.py:72`; `app/chamados/models.py:210`; `app/chamados/models.py:278`; `app/chamados/models.py:424`; `app/chamados/signals.py:172`; `app/core/observ.py:85`)

**Não identifiquei erro causado apenas por ler a aprovação ainda sem commit:** o save de J precede o fechamento no mesmo fluxo, e célula/evento são gravados juntos. Já `invalidar_fechamento()` agora delimita a leitura e cada competência com atomics próprios, mantendo a captura fora deles. Isso não protege o resolvedor direto de `_retratar`. (`app/ponto/services/justificativa.py:159`; `app/ponto/services/justificativa.py:168`; `app/ponto/portas/celula.py:639`; `app/ponto/services/fechamento.py:1616`; `app/ponto/services/fechamento.py:1619`; `app/ponto/services/fechamento.py:1629`; `app/chamados/reconciliador.py:469`)

**FORA-DO-ESCOPO, legado:** após o atomic da aprovação individual, o bloco de `entrada_bloqueada_turno_aberto` ainda pode forçar o fechamento da própria origem que o helper reteve por irmã pendente. Esse bloco também impede estender a garantia nova a todo o comportamento posterior da porta. (`app/ponto/services/justificativa.py:188`; `app/ponto/services/justificativa.py:193`; `app/ponto/services/justificativa.py:199`; `app/chamados/models.py:1192`)

## 29/10 17:38 · sessao edec12c9aee3 · - · - · origem=Ronald (tmux)

Achados por leitura estática; não executei testes nem operações de banco. [nao verificado] [nao verificado]

1. **Ordem de trava: aprovação × rejeição ainda inverte C/J.** A aprovação trava C antes de J (`app/ponto/services/justificativa.py:83`, `:94`, `:95`); a rejeição trava J e salva dentro do atomic (`:231`, `:232`, `:242`). Esse save chama o cartório sincronamente (`app/ponto/signals.py:161`, `:165`, `:93`), que pode reconciliar C quando seu marco acendeu (`app/ponto/services/cartorio.py:1163`) ou quando o dia ficou regular (`:1188`, `:1196`). O UPDATE do chamado acontece em `app/chamados/models.py:444`, `:231`. Escalonamento: A trava C; B trava J e tenta C pelo signal; A tenta J → ciclo. A pré-trava nova não alcança a rejeição (`app/ponto/services/justificativa.py:220`, `:232`).

2. **Ordem de trava: aprovação × materialização de batida mantém ciclo C/TurnoMaterializado.** `veredito_celula` abre atomic e registra batidas dentro dele (`app/ponto/services/veredito_celula.py:203`, `:298`, `:300`). O primeiro receiver recomputa o núcleo (`app/ponto/signals.py:19`, `:26`, `:34`), cujo DELETE mantém a trava de T até o atomic externo terminar (`app/ponto/nucleo.py:57`, `:59`). Escalonamento concreto: B materializa uma saída e trava T; A pré-trava C e tenta registrar outra saída do mesmo dia, bloqueando no DELETE de T; B continua ao cartório/reconciliador e tenta atualizar C, bloqueando em A (`app/ponto/services/justificativa.py:94`, `:141`; `app/ponto/signals.py:131`, `:135`, `:206`, `:215`; `app/ponto/services/cartorio.py:1163`; `app/chamados/models.py:231`). Use duas saídas com timestamps separados por mais de 60 segundos para evitar o dedup (`app/ponto/registro_batida.py:106`, `:113`). Uma chamada isolada em autocommit não demonstra esse ciclo; o atomic externo de `veredito_celula` demonstra (`app/ponto/services/veredito_celula.py:203`).

3. **Outra: o resolver “best-effort” roda novamente sem proteção na unidade escolhida.** O helper chama `retratar` (`app/ponto/services/justificativa.py:40`); `_retratar` chama o resolver novamente, sem catch, depois do fechamento (`app/chamados/reconciliador.py:455`, `:456`, `:469`). O savepoint novo protege somente a chamada do receiver (`app/chamados/signals.py:88`, `:91`, `:94`). Com o mock do teste `test_erro_de_banco_engolido_nas_perguntas_nao_desfaz_o_fechamento`, a primeira falha é engolida, mas a segunda chamada propaga e desfaz a aprovação pelo atomic externo (`app/ponto/services/justificativa.py:93`, `:169`, `:173`). Logo esse teste não deveria ficar verde com um erro real repetível no resolver; execução não realizada. [nao verificado]

4. **Outra: aprovação de J1 pode rejeitar outra J2.** Entrada permitida pelo schema: C vivo `justificativa_colab`, categoria `justificativa`, subcategoria `esqueci_saida`; C aponta J2 pendente no dia D2; J1 pendente no dia D1≠D2 aponta C como origem, sem saída informada (`app/chamados/catalogo/modulos.py:190`; `app/ponto/models.py:173`, `:189`). A porta de vínculo não verifica correspondência do dia (`app/chamados/services/vinculos.py:81`, `:88`). Esse C não pertence ao universo de furo pelos campos indicados, portanto seu gate aceita (`app/chamados/models.py:915`, `:920`, `:2466`, `:2478`, `:484`). Aprovar J1 fecha C e `_retratar` rejeita J2, acrescentando a mensagem de auto-rejeição (`app/ponto/services/justificativa.py:156`, `:168`, `:40`; `app/chamados/reconciliador.py:462`, `:465`, `:467`, `:468`). A existência desse formato em produção é `[nao verificado]`; o efeito novo está alcançável pela costura, sem precisar registrar batida antes (`app/ponto/services/justificativa.py:105`, `:156`, `:168`).

No par estrito aprovação/reconciliador, o reconciliador não usa `select_for_update` de chamado: percorre vivos por PK e adquire trava no UPDATE, antes de salvar a justificativa pendente (`app/chamados/reconciliador.py:1203`, `:1205`, `:1214`, `:456`, `:468`; `app/chamados/models.py:231`). A pré-trava melhora essa ordem, mas os ciclos acima impedem afirmar que ela encerrou todas as inversões (`app/ponto/services/justificativa.py:83`; `app/ponto/services/justificativa.py:232`; `app/ponto/nucleo.py:59`).

## 29/10 17:37 · sessao 796244ccb65e · - · - · origem=Ronald (tmux)

Achados dos testes e tetos, somente por leitura: [nao verificado]

1. **Teste de erro das perguntas contradiz o código atual.** O mock sempre tenta gravar `tipo='x'*40` (`app/ponto/tests/test_justificativa_encerra_origem.py:237`), mas o campo admite 20 (`app/ponto/models.py:146`). O receiver protege a primeira chamada (`app/chamados/signals.py:91`), porém `_retratar` chama novamente o mesmo resolver, sem tratamento, dentro do atomic (`app/chamados/reconciliador.py:455`, `app/chamados/reconciliador.py:469`). Essa segunda falha desfaz a aprovação; o teste exige aprovação e fechamento (`app/ponto/tests/test_justificativa_encerra_origem.py:247`). Execução efetiva: `[nao verificado]`.

2. **Selo aceita fechar a justificativa errada — classe selo sem mordida.** `_chamadas` reconhece o nome da função, sem conferir argumentos (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:61`); `_chama` só relaciona posições das linhas (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:102`). Confirmei, executando exclusivamente essas funções AST, que `j.status='aprovada'; j.save(); encerrar_chamado_de_origem(k)` dentro do atomic retorna nenhuma violação. Entrada: `j` com origem viva e `k` sem origem → selo verde, aprovação de `j` sem fechamento. A limitação `if False` já está declarada (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:8`).

3. **Asserções positivas evitam várias vacuidades.** O teste das irmãs exige aprovação e trilha de retenção (`app/ponto/tests/test_justificativa_encerra_origem.py:266`, `app/ponto/tests/test_justificativa_encerra_origem.py:269`); retirar toda a costura não satisfaz a trilha. O teste de falha da porta exige chamada efetiva ao mock (`app/ponto/tests/test_justificativa_encerra_origem.py:170`); o de rollback do lote também (`app/ponto/tests/test_justificativa_encerra_origem.py:197`). O controle negativo de rejeição e o caso sem origem passam sem a cura por serem controles (`app/ponto/tests/test_justificativa_encerra_origem.py:129`, `app/ponto/tests/test_justificativa_encerra_origem.py:176`).

4. **Cobertura das irmãs não cobre os dois desvios de integração.** O teste usa `turno_aberto` e aprovação individual (`app/ponto/tests/test_justificativa_encerra_origem.py:47`, `app/ponto/tests/test_justificativa_encerra_origem.py:262`). Entretanto:
   - Origem `entrada_bloqueada_turno_aberto` retida pelo novo helper ainda pode ser fechada pelo trecho pós-atomic legado (`app/ponto/services/justificativa.py:193`, `app/ponto/services/justificativa.py:199`).
   - Na materialização, a cascata chama `encerrar()` antes do novo helper (`app/ponto/services/justificativa.py:349`, `app/ponto/services/justificativa.py:357`). `encerrar()` força a transição sem guarda das irmãs (`app/chamados/models.py:1192`). O teste de materialização escolhe precisamente outro módulo, `justificativa_colab` (`app/ponto/tests/test_justificativa_encerra_origem.py:213`).
   
   As chamadas forçadas são legadas; o segundo sítio tem queryset alterado pelo diff (`app/ponto/services/justificativa.py:346`).

5. **Unidade real do custo: por save terminal.** S143 testa apenas `raw` e estado terminal, ignorando mudança de status e `update_fields` (`app/chamados/signals.py:76`, `app/chamados/signals.py:81`); portanto cada execução abre o atomic, inclusive sem perguntas (`app/chamados/signals.py:91`). Os aumentos dos tetos equivalem a 1, 2 e 4 execuções do receiver, respectivamente (`app/ponto/tests/test_selo_chokepoint_escrita.py:114`, `app/ponto/tests/test_selo_chokepoint_escrita.py:154`). A contagem real desses saves permanece `[nao verificado]`. O comentário “por chamado que fecha” é impreciso, embora registre a dupla execução (`app/ponto/tests/test_selo_chokepoint_escrita.py:148`). Os testes de teto usam `<=`, logo retirar savepoints reduz custo e passa; eles são tetos de desempenho, não prova funcional dos savepoints (`app/ponto/tests/test_selo_chokepoint_escrita.py:210`).

Não executei testes Django: o ambiente não possui o pacote instalado `[nao verificado]`. A execução das funções AST ocorreu com `python3 -B`, sem importar Django nem escrever arquivos `[nao verificado]`. [nao verificado]

## 29/10 17:37 · sessao cc184a0d6af7 · - · - · origem=Ronald (tmux)

Achados consolidados, somente leitura:

1. **Teste novo contradiz a implementação atual.** `test_erro_de_banco_engolido_nas_perguntas_nao_desfaz_o_fechamento` injeta falha em **todas** as chamadas ao resolver e exige aprovação persistida (`app/ponto/tests/test_justificativa_encerra_origem.py:237`, `:241`, `:247`). O receiver S143 isola e engole a primeira falha (`app/chamados/signals.py:88`), mas `_retratar` chama novamente o mesmo resolver, diretamente, sem captura (`app/chamados/reconciliador.py:424`, `:469`). Essa segunda falha atravessa o atomic e desfaz a aprovação (`app/chamados/reconciliador.py:455`; `app/ponto/services/justificativa.py:169`). Logo esse teste deve falhar **mesmo com a cura**; execução real [nao verificado]. Classe: **outra — teste incompatível com a costura**.

2. **Existe escritor de aprovação não alcançado: Django admin — FORA-DO-ESCOPO, pré-existente.** `JustificativaAdmin` herda diretamente de `ModelAdmin`, sem tranca dos campos (`app/ponto/admin.py:18`). `status` admite `aprovada` (`app/ponto/models.py:130`, `:159`); a própria casa documenta que esse admin torna campos editáveis (`app/core/admin.py:7`). Contraexemplo: alterar J pendente para aprovada no admin, com origem viva e sem célula; o signal da justificativa chama julgamento (`app/ponto/signals.py:161`), que retorna sem célula (`app/ponto/services/cartorio.py:1547`), deixando a origem viva. Não há costura com `encerrar_chamado_de_origem` nesse admin (`app/ponto/admin.py:18`).

3. **Não encontrei escritor de `status_local` fora das portas dentro do universo do selo.** Executei exclusivamente sua análise AST em memória: 739 arquivos, zero violações; o universo e a comparação ficam em `app/chamados/tests/test_selo_status_so_pelas_portas.py:317`, `:322`, `:337`. As portas admitidas estão em `:37`; os limites de resolução genérica estão declarados em `:17`. Não equivale a provar inexistência de escrita dinâmica além desses limites (`:17`).

4. **Efeitos dentro da transação:**

   - S143 novo protege sua própria invocação com savepoint e captura a exceção (`app/chamados/signals.py:88`); **não protege a segunda chamada direta** de `_retratar` (`app/chamados/reconciliador.py:469`).
   - Cartório captura exceções via `engolir` (`app/chamados/signals.py:171`; `app/core/observ.py:85`); célula e evento são persistidos juntos em atomic (`app/ponto/portas/celula.py:634`).
   - Rebaixa não captura exceções (`app/chamados/signals.py:68`), mas o fechamento normal já normaliza a urgência antes do signal (`app/chamados/models.py:210`); nessa situação o receiver retorna sem SQL (`app/chamados/models.py:278`, `:424`).
   - Não identifiquei erro causado simplesmente por ler a aprovação ainda sem commit: a gravação precede a costura no mesmo fluxo (`app/ponto/services/justificativa.py:156`, `:168`); manifestação concreta desse tipo [nao verificado].

5. **Outro best-effort anterior sem savepoint — FORA-DO-ESCOPO:** `_gravar_estado` engole erros da relavra S2 (`app/chamados/models.py:244`), cujo escritor executa `p.save()` diretamente (`app/chamados/veredito_lavrado.py:230`). Pode contaminar a transação em falha de banco; ocorrência em produção [nao verificado]. Não o classificaria como defeito introduzido pelo diff.

## 29/10 17:37 · sessao ee305bd1e0c2 · grupo1p1 · verificador · origem=Code

- **Classe 7 — chamador não migrado:** `bin/r3_turno_aberto_alcanca.py:46` chama `espelho_do_colab` sem `agora`, obrigatório em `app/ponto/services/espelho.py:1198`. A chamada levanta `TypeError` e o colaborador é pulado pelo tratamento em `bin/r3_turno_aberto_alcanca.py:48`.

- **Classe 5 — selo deixa a incompatibilidade passar:** os alvos e as raízes de busca excluem `espelho_do_colab` e `bin`, deixando o chamador acima invisível. Remover `agora` da chamada migrada da API também passa pelo detector. (`app/escala/tests/test_selo_grade_pede_o_instante.py:20`, `app/escala/tests/test_selo_grade_pede_o_instante.py:21`, `app/escala/tests/test_selo_grade_pede_o_instante.py:36`; `app/api/views.py:1369`)

- **Classe 3 — referência temporal interna no builder:** mesmo recebendo `agora`, o builder consulta `localdate()` quando `ref` é omitido. O desvio legacy também mantém essa leitura. (`app/escala/utils.py:1234`, `app/escala/utils.py:2028`, `app/escala/utils.py:962`)

- **Classe 3 — relógio interno no contrato:** `grade_espelho_janela` consulta `localdate()` quando `hoje` é omitido; as chamadas do PDF e da simulação efetivamente omitem esse argumento. (`app/escala/utils.py:2270`; `app/relatorios/pdf_espelho.py:472`, `app/relatorios/pdf_espelho.py:539`; `app/ponto/services/simular_dia.py:7`)

- **Classe 3 — ficha com múltiplos instantes:** calcula `hoje` com `localdate()`, captura outro instante para o espelho e lê novamente para `lido_em`. (`app/colaboradores/services/ficha.py:117`, `app/colaboradores/services/ficha.py:155`, `app/colaboradores/services/ficha.py:199`, `app/colaboradores/services/ficha.py:249`)

- **Classe 3 — portão de exportação:** captura `now()` e depois obtém `hoje` por outra leitura, dentro de `_hoje()`. (`app/folha/porta_export.py:318`, `app/folha/porta_export.py:338`, `app/folha/porta_export.py:23`)

- **Classe 3 — detector de pares:** captura `now()` e depois lê `localdate()` para `ref`. (`app/ponto/management/commands/detectar_par_relampago.py:328`, `app/ponto/management/commands/detectar_par_relampago.py:335`)

- **Classe 3 — oráculo:** captura `now()` e, sem `--hoje`, lê novamente por `localdate()`. (`app/ponto/management/commands/e6_oraculo.py:245`, `app/ponto/management/commands/e6_oraculo.py:257`)

- **Classe 3 — mapa de divergência:** captura `now()` e obtém `hoje` por outra leitura. (`app/ponto/management/commands/mapa_divergencia.py:36`, `app/ponto/management/commands/mapa_divergencia.py:45`)

- **Classe 3 — pautas discordantes:** sem `--hoje`, o comando lê `localdate()` e chama `apurar`, que captura outro instante quando há dias para apurar. (`app/ponto/management/commands/pautas_discordantes.py:111`, `app/ponto/management/commands/pautas_discordantes.py:112`, `app/ponto/management/commands/pautas_discordantes.py:71`)

- **Classe 3 — reconciliação:** além do instante capturado, mantém leituras independentes para `ref`, `hoje` e `_agora_local`. (`app/ponto/management/commands/reconciliar_grade.py:75`, `app/ponto/management/commands/reconciliar_grade.py:78`, `app/ponto/management/commands/reconciliar_grade.py:80`, `app/ponto/management/commands/reconciliar_grade.py:81`)

- **Classe 3 — teto na lista:** sem `hoje` explícito, lê `localdate()` e depois `now()`. (`app/ponto/services/teto_na_lista.py:49`, `app/ponto/services/teto_na_lista.py:50`)

- **Classe 3 — comparação PDF/espelho:** captura o instante, mas consulta `date.today()` novamente dentro do laço de colaboradores. (`app/relatorios/management/commands/diff_pdf_x_espelho.py:51`, `app/relatorios/management/commands/diff_pdf_x_espelho.py:76`)

- **Classe 3 — coletor PDF:** recebe `agora`, mas consulta `date.today()` e chama `ate_ontem` sem fornecer `hoje`; esse helper também lê o relógio. (`app/relatorios/pdf_espelho.py:187`, `app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:593`; `app/relatorios/cartao_pela_celula.py:40`)

- **Classe 3 — tela do espelho:** captura o instante e depois chama `now()` novamente para semanas, resumo da escala e contexto. (`app/ponto/views.py:97`, `app/ponto/views.py:182`)

- **Classe 3 — API do espelho:** captura `agora`, mas o ramo do turno previsto de hoje consulta outro `now()`. (`app/api/views.py:1350`, `app/api/views.py:1635`)

- **Classe 3 — avaliação de flips:** captura um instante, mas chama a montagem da fila sem propagá-lo; as duas fontes da fila consultam seus próprios relógios. (`app/ponto/services/flip_auto.py:263`, `app/ponto/services/flip_auto.py:268`, `app/ponto/services/flip_auto.py:146`, `app/ponto/services/flip_auto.py:148`, `app/ponto/services/flip_auto.py:34`, `app/ponto/services/flip_auto.py:126`)

- **Classe 3 — instante não chega à fonte dos totais:** espelho e PDF chamam `folha_manda` sem transmitir o instante recebido. Ela consulta `ate_ontem` com relógio próprio e, havendo candidato aberto, consulta `turno_aberto_de` sem instante. (`app/ponto/services/espelho.py:1288`; `app/relatorios/pdf_espelho.py:623`; `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:398`, `app/relatorios/cartao_pela_celula.py:40`; `app/ponto/turnos.py:2255`)

- **Classe 3 — emissão auditável do PDF:** captura o instante para os dados, mas chama o gerador auditável, que captura outro para o documento. (`app/relatorios/pdf_espelho.py:1087`, `app/relatorios/pdf_espelho.py:1098`; `app/relatorios/pdf_base.py:133`)

Total por classe: 1=0; 2=0; 3=17; 4=0; 5=1; 6=0; 7=1; 8=0.

## 29/10 17:36 · sessao 6d4a6d58dba3 · - · - · origem=Ronald (tmux)

- **Classe 7:** `bin/r3_turno_aberto_alcanca.py:46` continua chamando `espelho_do_colab(col, HOJE, ini, fim)` sem `agora`, obrigatório em `app/ponto/services/espelho.py:1198`. A chamada levanta `TypeError`; o `except` registra o erro e pula o colaborador (`bin/r3_turno_aberto_alcanca.py:47-48`). Entra no escopo pela assinatura alterada e pela inclusão explícita de chamadores não migrados.

- **Classe 5, candidato para consolidar:** o selo limita `ALVOS` aos três builders (`app/escala/tests/test_selo_grade_pede_o_instante.py:20`) e aceita qualquer expressão diferente de `None` para `agora` (`app/escala/tests/test_selo_grade_pede_o_instante.py:38-40`). Executando só seu detector, sem Django, `grade_espelho_janela(..., agora=timezone.now())` retorna `[]`: a leitura de relógio no miolo passa pelo selo. Também retorna `[]` para `espelho_do_colab(...)` sem `agora`, pois esse contrato alterado não está em `ALVOS` (`app/escala/tests/test_selo_grade_pede_o_instante.py:20`).

Censo adicional: não apareceram outros chamadores Python sem migração; `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:53` recebe o argumento por `**kw`, preenchido na linha 52.

## 29/10 17:36 · sessao d89ef60a26da · - · - · origem=Ronald (tmux)

- **Classe 5 — cobertura incompleta do selo.** `app/escala/tests/test_selo_grade_pede_o_instante.py:20` limita os alvos aos três builders; `:36` ignora qualquer outra função. Retirar `agora=agora` de `app/api/views.py:1369` restaura um chamador incompatível com a assinatura obrigatória de `app/ponto/services/espelho.py:1198`, mas `chamadas_sem_instante` continua devolvendo `[]`; a asserção de `app/escala/tests/test_selo_grade_pede_o_instante.py:59` permanece verde. Prova feita em memória, sem alterar arquivo.

- **Classe 3 — relógio interno sobrevivente.** `app/escala/utils.py:2270` consulta `localdate()` quando `hoje` não vem; isso ocorre nas chamadas tocadas de `app/relatorios/pdf_espelho.py:472`, `app/relatorios/pdf_espelho.py:539` e `app/ponto/services/simular_dia.py:7`, mesmo recebendo `agora`. O builder também consulta `localdate()` com `ref=None` em `app/escala/utils.py:1234`.

- **Classe 3 — segunda leitura na view.** `app/ponto/views.py:97` captura o instante, mas `app/ponto/views.py:182` chama `timezone.now()` novamente para semanas, resumo da escala e contexto.

_191 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
