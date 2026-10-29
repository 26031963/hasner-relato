# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 17:43** · publicado em **29/10/2026 17:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 29/10 17:35 · sessao c9b56617d3ee · - · - · origem=Ronald (tmux)

Achados de classe 3, por sítio tocado:

1. `app/escala/utils.py:1234`: builder por turno ainda lê `localdate()` quando `ref` é omitido, embora receba `agora` obrigatório em `:1221`.
2. `app/escala/utils.py:2270`: contrato ainda lê `localdate()` quando `hoje` é omitido; PDF efetivamente omite `hoje` em `app/relatorios/pdf_espelho.py:472` e `:539`.
3. `app/colaboradores/services/ficha.py:199`: repassa `agora` lido em `:155`, mas o mesmo ato também lê relógio em `:117` e `:249`.
4. `app/folha/porta_export.py:318`: lê `now()` e depois chama `_hoje()` em `:338`, cuja implementação lê `localdate()` em `:23`.
5. `app/ponto/management/commands/detectar_par_relampago.py:328`: lê `now()` e volta a ler `localdate()` em `:335`.
6. `app/ponto/management/commands/e6_oraculo.py:245`: lê `now()` e, sem `--hoje`, lê `localdate()` em `:257`.
7. `app/ponto/management/commands/mapa_divergencia.py:36`: lê `now()` e `localdate()` em `:45`.
8. `app/ponto/management/commands/pautas_discordantes.py:71`: `apurar()` lê relógio internamente; `handle()` já lê `localdate()` em `:111` antes de chamá-lo em `:112`.
9. `app/ponto/management/commands/reconciliar_grade.py:75`: captura instante, mas relê em `:78`, `:80` e `:81`.
10. `app/ponto/services/teto_na_lista.py:50`: captura instante depois da leitura independente de `localdate()` em `:49`.
11. `app/relatorios/management/commands/diff_pdf_x_espelho.py:76`: usa `dt.date.today()` dentro do laço apesar do instante capturado em `:51`.
12. `app/relatorios/pdf_espelho.py:593`: coletor recebe `agora` em `:187`, mas lê `datetime.date.today()` e chama `_ate_ontem` sem `hoje`; também chama `_ate_ontem` sem `hoje` em `:287`. Esse helper lê `localdate()` em `app/relatorios/cartao_pela_celula.py:40`.
13. `app/ponto/views.py:182`: `espelho_ponto` relê `now()` apesar da captura em `:97`.
14. `app/api/views.py:1635`: `api_espelho_v2` relê `now()` apesar da captura em `:1350`.
15. `app/ponto/services/flip_auto.py:268`: `avaliar_fila` capturou `now()` em `:263`, mas chama `fila_flip`, que chama `pares_mesmo_tipo` em `:146` e `fila_flip_recusas` em `:148`; estes leem novamente em `:34` e `:126`.
16. `app/ponto/services/espelho.py:1288`: `espelho_do_colab` recebe `agora` em `:1198`, mas não o propaga a `folha_manda`. Esta sempre chama `ate_ontem` sem `hoje` em `app/relatorios/cartao_pela_celula.py:412` e, havendo turno aberto, chama `turno_aberto_de` sem instante em `:398`; os relógios são lidos em `:40` e `app/ponto/turnos.py:2255`. PDF também chama `folha_manda` sem instante em `app/relatorios/pdf_espelho.py:623`.
17. `app/relatorios/pdf_espelho.py:1098`: `gerar_pdf_espelho_bytes` captura instante em `:1087`, mas chama `gerar_pdf_auditavel`, que lê outro instante em `app/relatorios/pdf_base.py:133`.

Não confirmei achado independente de classe 8; o efeito em produção da troca de `date.today()` para `timezone.localtime(...).date()` em `app/ponto/views.py:98` não está demonstrado pelo dado disponível. [não verificado]

## 29/10 17:25 · sessao 76db267e245a · grupo1 · leitura · origem=Code

1) **Na cópia, `turno_do_juiz_no_dia(...)` devolve `None`.** O turno fechado passa pela primeira guarda, mas o encerramento temporal ainda é falso. [app/escala/utils.py:571–578]

O caminho, aplicando as entradas fornecidas:

| Etapa | Valor / ramo tomado | Fonte |
|---|---|---|
| `len(turnos_do_dia) != 1 or turnos_do_dia[0].aberto` | `False`; continua. | `app/escala/utils.py:571–572` |
| `_hii, _hfi` | `01:00, 02:00`; `not (_hii and _hfi)` é falso; continua. | `app/escala/utils.py:573–575` |
| `turno_aberto_vivo(...)` | `abertos = []`; retorna `None`. A expiração por margem **não é consultada**. | `app/ponto/turnos.py:1987–1991` |
| `_aberta is not None` | `False`; não retorna nem `'turno_aberto_relogio'` nem `'turno_aberto'`. | `app/ponto/turnos.py:2097–2101` |
| `marcos_turno(..., ec=ec)` | Lê o DNA e encadeia: `09-28 19:00 → 09-29 01:00 → 09-29 02:00 → 09-29 07:00`. | `app/escala/servico_jornada.py:68–85` |
| `fim_turno(...)` | Retorna o último marco: **2026-09-29 07:00 −03:00**. | `app/escala/servico_jornada.py:89–92` |
| `fim is None` | `False`; não retorna `'sem_previsto'`. | `app/ponto/turnos.py:2102–2104` |
| `agora >= fim` | `00:02:34 >= 07:00` é `False`; `_dia_encerrado_motivo_puro` retorna **`'relogio'`**. | `app/ponto/turnos.py:2105`; valor da constante em `app/ponto/turnos.py:2066` |
| `_dia_encerrado_puro(...)` | `'relogio' is None` → **`False`**. | `app/ponto/turnos.py:2083–2085` |
| `not _dia_encerrado_puro(...)` | `True`; retorna **`None`**, sem alcançar `return turnos_do_dia[0]`. | `app/escala/utils.py:576–578` |

2) **Na cópia, `_match_marcos` recebe `turno=None` e `derivar=False`; portanto usa o alinhamento DP.** A closure repassa a decisão acima; sem turno, `_papeis=None`, e `derivar=False` também impede derivar papéis por alternância. O caminho chega a `_alinhar(M, B)`. [app/escala/utils.py:1565; app/escala/utils.py:1670–1672; app/escala/utils.py:592–595; app/escala/utils.py:621–629]

3) **No `main`, a closure devolve o próprio turno `[18:05:15, 23:01:04]`.** Ela verifica somente quantidade e `aberto`: a condição é falsa e executa `return _ts[0]`; não consulta `agora`, `fim_turno` ou `dia_encerrado`. Como há `hii` e `hfi`, esse objeto é passado em `turno=`, habilitando a leitura do papel do juiz. [main:app/escala/utils.py:1528–1536; main:app/escala/utils.py:1641–1643; main:app/escala/utils.py:562]

4) **Para esse instante, a cópia respeita o teto temporal; o `main` libera o papel prematuramente.** Às `00:02:34`, o fim previsto das `07:00` ainda não passou; a cópia bloqueia por `'relogio'`, enquanto o `main` aceita o fechamento do par isolado como suficiente. A seção 6 exige proteção por instante ou fato encerrado, e a documentação do juiz explicita que ausência de turno aberto, sozinha, pode antecipar o encerramento da jornada. [CLAUDE.md:330–334; app/ponto/turnos.py:2033–2037; app/ponto/turnos.py:2105; main:app/escala/utils.py:1533–1536]

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

_182 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
