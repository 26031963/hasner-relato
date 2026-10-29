# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 17:38** · publicado em **29/10/2026 17:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

_177 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
