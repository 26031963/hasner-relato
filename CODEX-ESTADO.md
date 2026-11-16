# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 15:33** · publicado em **16/11/2026 15:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 15:33 · sessao bdfad5118ce7 · UI-EDITOR-CALENDARIO-TRES-GRADES · verificador · origem=Code

1. **PROCEDE — curas do item 3; NÃO PROCEDE — cura completa do item 5.**

O `finally` cobre o reenvio síncrono por `requestSubmit()`: o guarda encontra `confirmado=true`, e a autorização é zerada ao retornar, inclusive se o `required` impedir o evento ou ocorrer exceção. Também cobre o retorno de `form.submit()`; esse fallback dispensa validação e evento `submit`, mas só é chamado após resposta afirmativa. Não identifiquei novo caminho do editor que envie grade vazia em `dias_mes` sem pergunta. (`app/templates/escala/folga_calendario_editar.html:138`, `app/templates/escala/folga_calendario_editar.html:143`, `app/templates/escala/folga_calendario_editar.html:147`, `app/templates/escala/folga_calendario_editar.html:151`)

Editar o nome agora apenas redesenha, sem repor `dias_*` após `fcModo()` esvaziá-los. (`app/templates/escala/folga_calendario_editar.html:128`, `app/templates/escala/folga_calendario_editar.html:180`)

**Resta uma lacuna no selo das grades:** ele verifica somente a abertura das `section`, sem medir sua visibilidade após o JS. Contraexemplo por inspeção: acrescentar ao final da inicialização a ocultação de `fc-tam-30` e `fc-tam-28` preservaria as asserções estáticas; os cliques programáticos do smoke continuariam alcançando seus dias. O delta protege `hidden/display:none` diretamente na seção inicial, mas ainda não protege integralmente “três grades visíveis ao mesmo tempo”. (`app/escala/tests/test_editor_calendario_tres_grades.py:37`, `app/escala/tests/test_editor_calendario_tres_grades.py:89`, `app/escala/tests/test_editor_calendario_tres_grades.py:92`, `app/escala/tests/test_smoke_clique_folga_calendario.py:51`)

Execução desse mutante: **[não verificado]**.

2. **PROCEDE — leitores e escritores depois do delta.**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `confirmado` | Inicialização em falso; resposta afirmativa põe verdadeiro; `finally` limpa. (`app/templates/escala/folga_calendario_editar.html:136`, `:150`, `:151`) | Guarda de `submit`. (`app/templates/escala/folga_calendario_editar.html:138`) |
| `dias_<tam>` no formulário | `syncHidden()` serializa `sel`; inicialização, alternar dia, repetir e voltar a `dias_mes` o acionam. `fcModo()` limpa em `semana_dia`. (`app/templates/escala/folga_calendario_editar.html:109`, `:110`, `:119`, `:125`, `:180`, `:181`) | Navegador/sonda serializam; view lê, filtra e entrega à porta; porta persiste `dias`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:56`; `app/escala/views.py:342`, `:347`, `:356`; `app/escala/folgas.py:594`) |
| Canônico do editor | `renderGrade()` usa seleção ordenada e nome atual; input do nome chama esse redesenho. (`app/templates/escala/folga_calendario_editar.html:80`, `:94`, `:106`, `:128`) | Exibição em `fc-canonico-*`; não é campo enviado à view. (`app/templates/escala/folga_calendario_editar.html:57`, `:106`; `app/escala/views.py:321`, `:342`) |
| Canônico da lista | View monta a partir de nome, tamanho e dias persistidos. (`app/escala/views.py:293`, `:297`) | Template da lista imprime `t.canonico`. (`app/templates/escala/folga_calendarios_lista.html:22`) |

3. **PROCEDE — o novo teste morde a autorização velha, por inspeção.** [nao verificado]

A sonda apaga o nome antes do primeiro Sim e restaura antes do segundo Salvar. Sem a limpeza no `finally`, o primeiro reenvio barrado pelo `required` deixa a autorização verdadeira; o segundo clique passa sem perguntar, faltando `modal2` e aparecendo POST. As duas asserções pertinentes exigem segundo modal e `SEM-POST`. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:151`; `app/escala/tests/test_smoke_clique_folga_calendario.py:209`, `:210`, `:212`, `:213`)

**NÃO PROCEDE — reabrir os descartes como bloqueadores:**

- Trocar para `semana_dia` enquanto o modal está pendente não demonstra envio indevido em `dias_mes`. Contudo, a justificativa “é overlay” não garante bloqueio do teclado: o componente foca o Sim, mas não implementa contenção de foco. (`app/templates/escala/folga_calendario_editar.html:139`; `app/templates/core/_dialogo.html:160`, `:195`)
- A contagem sem `_reenvia()` continua redundante; `SEM-POST` é a asserção efetiva. (`app/escala/tests/test_smoke_clique_folga_calendario.py:175`, `:192`, `:199`, `:200`)
- O smoke contém leitura real do modal no Chromium, além da regex; resultado efetivamente executado não foi fornecido. (`app/escala/tests/test_smoke_clique_folga_calendario.py:69`, `:71`, `:124`, `:187`)

GREEN, RED e mutante executados: **[não verificado]**.

4. **NÃO PROCEDE — nova infração identificada de LEIS-UI ou dos selos estruturais.**

A ajuda apontada na volta 1 foi removida, curando aquele item 6; o trecho agora entra diretamente no laço das grades. O delta mantém `hxConfirmar` e não introduz novo markup nas alterações do guarda. (`app/docs/LEIS-UI.md:77`; `app/templates/escala/folga_calendario_editar.html:39`, `:127`, `:146`, `:151`)

Execução dos selos de template: **[não verificado]**.

**VEREDITO: DEVOLVE 5.**

## 16/11 15:31 · sessao 20b59d9b1e2c · - · - · origem=Ronald (tmux)

**PROCEDE — cura da autorização.** `confirmado=true` agora vale apenas durante o reenvio e o `finally` o limpa mesmo quando `requestSubmit()` retorna sem gerar `submit` por causa do `required`, ou quando a chamada lança exceção. O guarda síncrono do reenvio encontra `true`. (`app/templates/escala/folga_calendario_editar.html:138`, `:150`, `:151`)

**PROCEDE, com limite explícito — fallback.** O `finally` também limpa após `form.submit()`. Esse fallback não passa pelo evento `submit` nem pela validação HTML, mas só é alcançado depois de `ok=true`; portanto não cria POST sem pergunta. A garantia de bloqueio pelo `required` não se aplica a esse fallback. (`app/templates/escala/folga_calendario_editar.html:146`, `:147`, `:151`)

**NÃO PROCEDE — novo caminho demonstrado de POST vazio sem pergunta em `dias_mes`.** No fluxo do formulário, o guarda consulta o modo, identifica seleção vazia e cancela antes de abrir a pergunta; Cancelar retorna sem reenvio. (`app/templates/escala/folga_calendario_editar.html:139`, `:140`, `:143`, `:146`, `:147`)

**PROCEDE — novo teste tem mordida por inspeção.** A sonda apaga o nome antes do primeiro Sim e o restaura antes de clicar novamente. Sem o `finally` — inclusive restaurando o código anterior que limpava somente no evento `submit` — o primeiro `requestSubmit()` é barrado pelo `required`, a autorização sobra, o segundo clique passa sem modal e a sonda registra POST. Assim falham `modal2` e/ou `SEM-POST`. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:151`; `app/escala/tests/test_smoke_clique_folga_calendario.py:209`, `:210`, `:212`, `:213`, `:52`, `:57`) Execução efetiva e mutante executado: **[não verificado]**.

**NÃO PROCEDE — descarte do modal/troca de modo como bloqueador desta obra.** O overlay existe, mas o componente não implementa contenção de foco; só trata Escape e Enter no campo do próprio modal. A possibilidade de troca por teclado não está eliminada. Contudo, confirmar e reenviar em `semana_dia` segue a regra desse modo, enquanto voltar a `dias_mes` e submeter normalmente encontra o guarda. (`app/templates/core/_dialogo.html:112`, `:160`, `:163`, `:195`; `app/templates/escala/folga_calendario_editar.html:139`, `:176`) Reprodução humana: **[não verificado]**.

**NÃO PROCEDE — contagem redundante como defeito bloqueador.** A comparação antes/depois não demonstra ausência de gravação HTTP: o POST Django só ocorre em `_reenvia()`, ausente nesse teste. A asserção efetiva é `SEM-POST`, alimentada pela sonda que respeita `defaultPrevented`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:53`, `:165`, `:175`, `:192`, `:199`, `:200`)

**PROCEDE — smoke vai além da regex de `hxConfirmar`.** O código do smoke lê texto e botões do modal aberto no DOM do Chromium e exige sua presença; isso protege comportamento quando executado. Não há, no dado recebido, relatório de execução que permita afirmar GREEN/RED observado. (`app/escala/tests/test_smoke_clique_folga_calendario.py:69`, `:71`, `:124`, `:187`, `:188`) Execução observada: **[não verificado]**.

## 16/11 15:31 · sessao fa2574325564 · - · - · origem=Ronald (tmux)

**Pergunta 4 — NÃO PROCEDE: nova infração de LEIS-UI ou dos selos estruturais não identificada no delta.** A ajuda questionada desapareceu; o trecho agora passa diretamente do contêiner ao laço de grades. A L7 está em `app/docs/LEIS-UI.md:77`; trecho atual em `app/templates/escala/folga_calendario_editar.html:39`. As adições restantes são comentários e JS, sem diálogo nativo ou novo markup interpolado (`app/templates/escala/folga_calendario_editar.html:127`, `app/templates/escala/folga_calendario_editar.html:146`, `app/templates/escala/folga_calendario_editar.html:151`). Os selos examinam diálogo nativo, atributos inválidos, fonte Python, comentários vazados e interpolação crua (`app/core/tests/test_contract_l8_dialogo.py:42`; `app/core/tests/test_contract_template_bem_formado.py:162`, `:169`, `:174`, `:180`). Execução desses selos: **[não verificado]**.

**Cura das grades ocultas — PROCEDE para o mutante declarado.** `_secoes()` devolve somente a tag de abertura de cada seção, portanto a regex nova não acusa a classe `hidden` legítima dos spans internos; acusa `hidden` ou `display:none` na própria seção (`app/escala/tests/test_editor_calendario_tres_grades.py:37`, `:40`, `:92`; `app/templates/escala/folga_calendario_editar.html:47`).

A prova continua limitada: o selo não mede visibilidade calculada, ancestrais, CSS por seletor ou o próprio `fc-grade` escondido; verifica apenas ID da grade e atributos da seção. O smoke também usa `.click()` programático (`app/escala/tests/test_editor_calendario_tres_grades.py:89`, `:92`; `app/escala/tests/test_smoke_clique_folga_calendario.py:51`). Contraexemplo estático: adicionar `hidden` ao `div#fc-grade-*` preservaria essas asserções; execução do mutante: **[não verificado]**.

## 16/11 15:28 · sessao a6729505dd53 · O50-CELULA-APOS-DESLIGAMENTO · verificador · origem=Code

**O item 1 precisa voltar para fechar condições literais**, principalmente pagamento, células editadas e universo de batidas; aval condicional não autoriza completar essas condições por inferência. (`app/docs/LEIS.md:61`; `app/docs/CORTES.md:470`.)

Revisão estática da cópia. Identidade Git do HASH, comportamento em execução e impacto em produção: [nao verificado].

1. **As portas existem, mas não fazem hoje a remoção proposta.**

   `executar_desligamento` grava situação, data e motivo da demissão, bloqueia acesso, registra histórico e chama `fechar_vinculos_ativos`; também executa a cascata de chamados/disputas e cria o chamado de auditoria ao DP. O ramo agendado grava a intenção e retorna antes dessa cascata. (`app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:147`; `app/colaboradores/services/desligamento.py:164`; `app/colaboradores/services/desligamento.py:195`.)

   O desligamento fecha apenas vínculos ativos cujo início não seja posterior ao fim informado; vínculos futuros recusados permanecem. Esse caminho usa `update()` e regenera somente células futuras, com `recalculo=False`. Portanto, não equivale à proposta de remover células e atualizar os valores do fechamento. (`app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:652`; `app/colaboradores/services/vinculo.py:662`; `app/colaboradores/services/vinculo.py:855`.)

   `encerrar_vinculo` valida a vigência, grava `ativa=False` e `data_fim` por `save()` e registra antes/depois. Esse `save()` dispara regeneração de células passadas e futuras, além da invalidação do previsto do fechamento. **Já há efeito indireto sobre célula e fechamento nessa porta.** (`app/colaboradores/services/vinculo.py:835`; `app/colaboradores/services/vinculo.py:842`; `app/escala/signals.py:72`; `app/escala/signals.py:80`; `app/escala/signals.py:145`.)

   A **porta da célula** é `ponto/portas/celula.py`, registrada para `CelulaDia`; gerar, regenerar, editar, lavrar e carimbar pertencem a esse módulo. `gerar_celulas.py` reexporta as funções, sem constituir outro escritor. Não há operação declarada de remoção de `CelulaDia`; fora da vigência, o código conserva a linha e altera trabalho/DNA. (`app/core/portas.py:107`; `app/ponto/portas/celula.py:74`; `app/ponto/management/commands/gerar_celulas.py:10`; `app/ponto/portas/celula.py:239`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`.)

   “Ninguém remove” seria absoluto demais: excluir o colaborador tem cascata sobre células; excluir o vínculo apenas anula sua geradora. (`app/escala/models.py:1681`; `app/escala/models.py:1692`.)

   A **porta do fechamento** é `ponto/services/fechamento.py`. `recalcular_por_evento` agenda, após commit, `recalcular_fechamento_mes` para o colaborador e a competência do fato; esta grava `FechamentoMensal` e também lavra `DiaPago`. Apenas invalidar carimbos não equivale a atualizar seus valores. (`app/ponto/services/fechamento.py:1315`; `app/ponto/services/fechamento.py:1798`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/fechamento.py:1144`; `app/ponto/services/fechamento.py:1191`; `app/ponto/services/fechamento.py:1652`.)

2. **A direção da obra é compatível, mas precisa conservar restrições vigentes.**

   A célula é dado congelado; a exceção formal para passado errado por cadastro exige denúncia da reescrita. LU-13/LU-14 autorizam a direção de remover com trilha, preservando **exportada, editada e com batida**, com medição prévia e `!` próprio para retroativo. O item 1 fornecido não repete a exceção “editada”; revogação dessa proteção: [nao verificado]. (`CLAUDE.md:149`; `app/docs/CORTES.md:470`; `app/docs/CORTES.md:471`.)

   DESLIGADO-NAO-TRAVA já faz o desligado retornar sem motivos de retenção e entrega os dias como informativo ao DP. Isso não depende de apagar células; a O50 não deve ser apresentada como condição para aquela liberação. (`app/folha/export.py:1197`; `app/folha/export.py:1207`; `app/folha/export.py:1216`.)

   A cura nas portas é coerente com L-099; usar código para escolher cadastro correto, apagar batida ou resolver vínculo futuro extrapolaria a separação entre ESTRUTURA e CADASTRO/BATIDA. (`app/docs/LEIS.md:122`; `app/colaboradores/services/vinculo.py:855`.)

   L-113 alterou as recusas históricas de L-092, mas o texto consolidado mantém alteração genérica de exportada fora da preaprovação, com porta e `!` específicos. Portanto, a rotina genérica de recálculo não pode servir como garantia automática de “exportada intacta”. (`app/docs/LEIS.md:155`; `CLAUDE.md:411`; `app/ponto/portas/celula.py:436`.)

3. **Há condições que ainda não fecham no literal.**

   **“Não paga”:** `competencia_entregue` reconhece TXT vigente; `competencia_fechada_para_celula` acrescenta holerite publicado da pessoa; `marcos_da_competencia['paga']` também consulta holerite publicado. Isso não comprova ausência de pagamento externo. A obra do juiz `competencia_ja_paga`, incluindo cadastro de pagamento externo, está registrada com pouso condicionado à espinha. (`app/folha/export.py:588`; `app/folha/export.py:618`; `app/folha/export.py:622`; `app/ponto/services/fechamento.py:1539`; `app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)

   **“Tenha batida”:** o universo apurável exclui retratadas, enquanto a regra distingue apuração de exibição crua. A associação ao dia também distingue batida reclamada pelo turno de órfã no dia civil. A proibição de remover precisa escolher explicitamente o universo e proteger a passagem da meia-noite. (`CLAUDE.md:158`; `CLAUDE.md:167`; `app/ponto/turnos.py:2888`; `app/ponto/turnos.py:2898`.)

   **“Afastado”:** há juiz registrado, `afastado_hoje`, com data e argumento obrigatório `para`; cobrança considera ausência não rejeitada, folha considera aprovada. Consultar apenas `situacao='afastado'` não representa esse contrato. Falta definir data e efeito do veto. (`app/core/juizes.py:272`; `app/ponto/turnos.py:2737`; `app/colaboradores/services/situacao_exibida.py:18`; `app/colaboradores/models.py:444`.)

   **“Posterior ao fim”:** a célula pertence à pessoa/dia, com geradora opcional; pode existir sucessor ou readmissão. A própria regeneração atual preserva dia coberto por outro vínculo. Remover indiscriminadamente toda célula após o fim de um vínculo perderia essa proteção. (`app/escala/models.py:1681`; `app/escala/models.py:1685`; `app/escala/models.py:1692`; `app/ponto/portas/celula.py:254`; `app/docs/BACKLOG.md:1411`.)

4. **Dinheiro está no escopo; seus efeitos ainda precisam ser delimitados.**

   Atualizar `FechamentoMensal` toca valores financeiros e pode alcançar a competência inteira e `DiaPago`. Valor efetivamente alterado ou garantia de nenhuma falta nova: [nao verificado]. A preaprovação exige reversão anterior, exportada intacta comprovada por hash e prova de frota no mesmo marco; paga e vínculo/escala retroativo continuam sujeitos a `!`. (`app/ponto/services/fechamento.py:902`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1191`; `CLAUDE.md:404`; `CLAUDE.md:406`.)

   A remoção também anula referências de chamados e perguntas; ausência da célula faz leitores recorrerem ao cálculo/template. Logo, “a linha sumiu” não prova sozinho que previsto, cobrança e dinheiro ficaram coerentes. (`app/chamados/models.py:50`; `app/chamados/models.py:2519`; `app/escala/models.py:1094`; `app/escala/models.py:1098`.)

**VEREDITO: AVAL-DEVOLVIDO**

- A execução aguardará `competencia_ja_paga` ou qual autoridade provará ausência de pagamento externo? (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)
- Permanecem excluídas células editadas, dias de sucessor/readmissão e todo passivo reservado ao `!`? (`app/docs/CORTES.md:470`; `app/docs/BACKLOG.md:1411`.)
- “Tenha batida” inclui retratada e órfã, com associação ao dia por qual autoridade? (`CLAUDE.md:158`; `CLAUDE.md:167`; `app/ponto/turnos.py:2898`.)
- “Afastado” será consultado em qual data e com qual `para`? (`app/ponto/turnos.py:2737`; `app/colaboradores/services/situacao_exibida.py:18`.)
- “Exportada → nada muda” vale para todo o ato ou apenas para o efeito novo da O50, considerando que o `save()` atual invalida previsto sem filtro de competência? (`app/escala/signals.py:145`; `app/ponto/services/fechamento.py:1607`.)

## 16/11 15:27 · sessao 982166043b41 · - · - · origem=Ronald (tmux)

- A porta canônica é `app/ponto/portas/celula.py`, registrada para `CelulaDia` em `app/core/portas.py:112`; declara gerar, lavrar, carimbar e editar, sem operação de remover célula (`app/ponto/portas/celula.py:74`).
- O censo estático real `core.censo_escritas.varrer_arvore` encontrou 10 escritas atribuídas a `CelulaDia`, todas nessa porta, sem `delete`; o instrumento declara limite para operações não atribuíveis (`app/core/censo_escritas.py:43`). Os sítios são `app/ponto/portas/celula.py:265`, `:279`, `:293`, `:310`, `:638`, `:647`, `:836`, `:914`, `:926`, `:1237`.
- Fora da vigência, hoje a célula permanece: futuro zera previsto, limpa geradora, guarda DNA anterior e desfaz julgamento (`app/ponto/portas/celula.py:251`); passado grava `trabalha=False` e DNA sem marcos (`app/ponto/portas/celula.py:272`). O comentário explicita “A celula NAO se apaga” porque há perguntas apontando para ela (`app/ponto/portas/celula.py:239`).
- O `delete` da porta remove `FuroDiario`, não `CelulaDia` (`app/ponto/portas/celula.py:1253`, `:1266`).
- A guarda vigente da célula é `competencia_fechada_para_celula`: TXT vigente entregue OU holerite com lote publicado da pessoa; não representa pagamento externo declarado (`app/folha/export.py:597`, `:618`, `:622`). O atual `marcos_da_competencia()['paga']` também só consulta holerite com lote publicado (`app/ponto/services/fechamento.py:1539`). O corte COMPETENCIA-PAGA consta “construindo”, condicionado à espinha (`app/docs/CORTES.md:495`).
- “Tenha batida” não fecha por inferência: o universo apurável exclui retratadas (`app/ponto/turnos.py:2868`, `:2888`); associação ao dia distingue batida reclamada por turno e órfã no dia civil (`app/ponto/turnos.py:2898`, `:2957`). É preciso dizer se a proibição inclui retratadas e como protege órfãs e turnos que cruzam meia-noite; o literal do anexo não decide isso [nao verificado].
- Excluir a célula anula referências de chamados e perguntas (`app/chamados/models.py:50`, `:2519`). Excluir o colaborador causa cascata para células (`app/escala/models.py:1681`); excluir vínculo apenas anula `escala_geradora` (`app/escala/models.py:1692`). A restauração de snapshot apaga ECs e protege células editadas contra essa anulação (`app/colaboradores/services/vinculo.py:1022`).
- A seleção deve preservar dia de sucessor: o gerador trabalha por vínculo vigente e, se a célula não existe, cria uma (`app/ponto/portas/celula.py:906`, `:914`); a regeneração também cria célula ausente no dia coberto (`app/ponto/portas/celula.py:289`). A célula pertence à pessoa e ao dia, enquanto a geradora é proveniência opcional (`app/escala/models.py:1681`, `:1685`, `:1692`).
- Remoção precisa conferir leitores: sem célula, `eh_dia_trabalho` recai no cálculo do vínculo (`app/escala/models.py:1094`); `marcos_do_dia` declara retorno ao template vivo quando falta célula/DNA (`app/escala/models.py:1098`). Ausência da linha não prova ausência de previsto.
- O dinheiro deve ser acionado pelo ato, não por sinal de `CelulaDia` (`app/ponto/portas/celula.py:432`). A própria porta registra que o caminho ordinário hoje pode recalcular exportada, após L-113 (`app/ponto/portas/celula.py:436`); O50 necessita guarda explícita do seu alcance antes de chamar esse caminho.

## 16/11 15:27 · sessao c81e9bf48af4 · - · - · origem=Ronald (tmux)

Achados para consolidar, só item 1:

- **Restrição omitida no aval novo:** LU-13 manda O50 “só DIREÇÃO”, define futuras como após D e exclui **exportada/editada/com batida**, exige medir frota antes e `!` próprio para retroativo. A condição **editada** precisa continuar explícita; o item 1 fornecido não a nomeia. (`app/docs/CORTES.md:470`; cláusula do anexo novo sem arquivo local: [nao verificado].)
- **Irretroatividade não se resolve só por “posterior ao fim”:** célula é dado congelado e passado só se reescreve pela exceção formal de cadastro com denúncia; “posterior ao fim” pode alcançar datas já passadas. Precisa separar evento futuro, célula futura já existente e passivo reservado ao `!`, sem autorizar por inferência a remoção de passado. (`CLAUDE.md:149-156`; `app/docs/BACKLOG.md:1410-1411`; `app/docs/CORTES.md:470`.)
- **Paga é dependência real, sem juiz pousado nesta cópia:** a obra `COMPETENCIA-PAGA-UM-JUIZ` declara o futuro `competencia_ja_paga`, cuja definição é entregue/exportação vigente OU cadastro “paga fora do sistema”, e proíbe meia migração; ainda está registrada/construindo, com pouso depois da espinha. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.) A busca em Python de produção não achou `competencia_ja_paga`, `CompetenciaPaga`, `paga_fora` ou `competencia_paga`: ausência absoluta além desses nomes [nao verificado].
- **Exportação tem juiz, pagamento não pode ser inferido dele:** `competencia_entregue` pergunta por TXT vigente não invalidado; `competencia_fechada_para_celula` compõe essa resposta com holerite publicado do próprio colaborador. Nenhum desses contratos diz representar pagamento externo. (`app/folha/export.py:556-594`, especialmente `:564-578`; `:597-628`; `app/core/juizes.py:343-353`.)
- **Afastado exige pergunta mais precisa:** `situacao='afastado'` permanece nas choices, mas o modelo declara que afastamento deixou de ser valor de cadastro e é lido do juiz. (`app/colaboradores/models.py:295-299`; `:444-449`.) O juiz registrado é `afastado_hoje`, que aceita data e exige `para`; tipos atualmente estreitados a `afastamento_inss`. (`app/core/juizes.py:272`; `app/ponto/turnos.py:2716-2744`.) `para='folha'` significa aprovada, `para='cobranca'` não rejeitada. (`app/ponto/turnos.py:2523-2532`.) Logo falta dizer se “tocar afastado” veta pela data do ato ou da célula e qual efeito/status; a apresentação dá precedência ao desligado, portanto não basta consultá-la depois de demitir. (`app/colaboradores/services/situacao_exibida.py:18-25`; `:56-63`.)
- **DESLIGADO-NAO-TRAVA é retentor, não licença de remoção:** hoje retorna vazio para desligado em qualquer dia da competência; informativo da rescisão só lê e declara proibido mexer em célula/batida e lançar falta automática naquela lei. O item 1 novo é autorização específica futura, não deve ser descrito como reparo de trava ainda vigente. (`app/folha/export.py:1197-1213`; `:1216-1228`; `app/docs/CORTES.md:440`.) O juiz do desligado lê só estado gravado e exclui agendado/afastado. (`app/colaboradores/services/desligamento.py:23-32`; `app/core/juizes.py:273-276`.)
- **Dinheiro está expressamente no escopo:** atualizar `FechamentoMensal` não é operação neutra. Regra consolidada exige `!` para paga e vínculo/escala retroativo; competência não exportada tem preaprovação condicionada a reversão antes, exportada intacta com hash e prova de frota no mesmo marco. Exportada só porta REGEN-EM-EXPORTADA com `!`, DIFF e trilha. (`CLAUDE.md:404-417`.) L-113 alterou as recusas históricas da L-092, mas não autoriza ignorar o item 1 que restringe expressamente exportada/paga; o texto consolidado atual é o acima. (`app/docs/LEIS.md:155`; `CLAUDE.md:411-422`.)
- **L-099 favorece cura na origem e lista para dado:** separar ESTRUTURA de CADASTRO/BATIDA está literalmente na lei; células com batida não viram caso especial de código. (`app/docs/LEIS.md:122`.)
- **Universo da guarda “tenha batida” continua ambíguo:** apuração canônica exclui retratadas, exibição usa cru. `batidas_apuraveis` confirma que pendente de DP conta, retratada não. Qual desses universos governa a proibição de apagar célula precisa ser fechado; não é lícito assumir que a lei de apuração responde pergunta de preservação de dado. (`CLAUDE.md:158-167`; `app/ponto/turnos.py:2868-2895`.)
- **Registro anterior de escopo maior:** O50-DESLIGAR-ENCERRA-TUDO absorve O50-CELULA e inclui encerrar vínculos vigentes/futuros; não transportar automaticamente esse escopo maior para esta revisão do item 1. (`app/docs/BACKLOG.md:1409`.) A preservação de último dia, turno noturno e readmissão já está declarada no complemento O50 e deve orientar os cenários do PREPARA. (`app/docs/BACKLOG.md:1411`.)

Perguntas mais fortes para AVAL-DEVOLVIDO: dependência do juiz paga; manutenção da exclusão editada e do limite direção/futuro; universo de batida e dia do turno; data/efeito do veto afastado. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:470`; `CLAUDE.md:149-167`; `app/ponto/turnos.py:2523-2532`, `:2716-2744`, `:2868-2895`.)

## 16/11 15:16 · sessao ed7650d71c30 · UI-EDITOR-CALENDARIO-TRES-GRADES · verificador · origem=Code

Análise por leitura da cópia e do diff. Execução da suíte, navegador e mutantes: **[não verificado]**. Os números de produção citados nos comentários não têm demonstração no dado fornecido. (`app/escala/tests/test_editor_calendario_tres_grades.py:6`)

1. **PROCEDE — leitores e escritores identificados.**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `dias_<tam>` | `syncHidden()` serializa `sel` em inputs. (`app/templates/escala/folga_calendario_editar.html:113`) | A view lê, filtra `1..tam` e entrega à porta; a porta persiste. (`app/escala/views.py:342`, `app/escala/views.py:347`, `app/escala/views.py:356`, `app/escala/folgas.py:594`) |
| `data-fc-vazia` | HTML inicial usa `t.marcados`; JS atualiza pela seleção atual. (`app/templates/escala/folga_calendario_editar.html:44`, `app/templates/escala/folga_calendario_editar.html:100`) | `_vazia()` nos testes. O guarda consulta diretamente `sel.size`. (`app/escala/tests/test_editor_calendario_tres_grades.py:44`, `app/templates/escala/folga_calendario_editar.html:140`) |
| `fc-marca-vazia` | Template cria o texto e a classe; `renderGrade()` atualiza a visibilidade. (`app/templates/escala/folga_calendario_editar.html:50`, `app/templates/escala/folga_calendario_editar.html:99`) | Usuário recebe o texto; teste procura o span fora dos scripts. (`app/templates/escala/folga_calendario_editar.html:51`, `app/escala/tests/test_editor_calendario_tres_grades.py:51`) |
| `sel[tam]` | Inicialização pelo seed, `fcTog()` e `fcRepetir()`. (`app/templates/escala/folga_calendario_editar.html:82`, `app/templates/escala/folga_calendario_editar.html:122`, `app/templates/escala/folga_calendario_editar.html:127`) | Grade, contagem, marca, chips, inputs, repetir e confirmação. (`app/templates/escala/folga_calendario_editar.html:89`, `:97`, `:116`, `:127`, `:140`) |
| `modo` | Radios; contexto vindo do calendário; persistência pela porta. (`app/templates/escala/folga_calendario_editar.html:20`, `app/escala/views.py:370`, `app/escala/folgas.py:585`, `app/escala/folgas.py:590`) | View, porta, guarda e alternância visual. (`app/escala/views.py:325`, `app/escala/folgas.py:584`, `app/templates/escala/folga_calendario_editar.html:139`, `:175`) |
| `t.marcados` | View projeta `cal.tamanhos.all()`/`t.dias`; ausência vira conjunto vazio. (`app/escala/views.py:362`, `app/escala/views.py:366`) | Estado inicial, contagem, marca e seed do JS. (`app/templates/escala/folga_calendario_editar.html:44`, `:49`, `:50`, `:77`) |

A origem persistida também pode nascer pelo minicadastro já existente; ele permanece em `folgas.py`. Isso não foi criado pelo diff. (`app/escala/folgas.py:645`, `app/escala/folgas.py:659`)

2. **NÃO PROCEDE — segundo escritor, juiz novo ou diálogo nativo.**

A view continua chamando `salvar_calendario_de_folga`; a gravação permanece nessa porta. As duas entidades têm `escala/folgas.py` como arquivo autorizado de escrita. (`app/escala/views.py:354`, `app/escala/folgas.py:594`, `app/core/portas.py:472`)

A seleção editável produz tanto a apresentação quanto os campos enviados e a pergunta sobre vazio; não surgiu outra derivação das datas efetivas de folga. Essa derivação continua em `datas_que_o_calendario_manda()`. (`app/templates/escala/folga_calendario_editar.html:97`, `:116`, `:140`; `app/escala/folgas.py:43`)

O diálogo utilizado é `hxConfirmar`, sem `confirm/alert/prompt/hx-confirm`. O próprio selo distingue essas chamadas. (`app/templates/escala/folga_calendario_editar.html:146`, `app/core/tests/test_contract_l8_dialogo.py:41`)

3. **PROCEDE COM RESSALVA — existe confirmação que pode ficar latente.**

No fluxo normal, clique, Enter e `requestSubmit()` encontram o guarda do formulário. Em `dias_mes`, grade vazia executa `preventDefault()` antes de chamar o modal; Cancelar não reenvia. (`app/templates/escala/folga_calendario_editar.html:137`, `:139`, `:143`, `:146`, `:147`)

Modal indisponível não libera esse envio: o cancelamento já aconteceu; quando o componente consegue responder à falta de elementos, devolve falso sem `naoBloquear`. (`app/templates/escala/folga_calendario_editar.html:143`, `:146`; `app/templates/core/_dialogo.html:184`)

**Contraexemplo por inspeção:** abrir a pergunta com nome válido, apagar o nome enquanto ela está pendente e confirmar. O `requestSubmit()` pode ser impedido pelo `required`, deixando `confirmado=true`; preencher novamente e salvar consome essa autorização antiga sem perguntar novamente. O booleano só é limpo no próximo evento `submit`. Reprodução em navegador: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:148`, `:149`; `app/templates/core/_dialogo.html:160`, `:195`)

Em `semana_dia`, uma nova submissão não abre o modal. Entretanto, uma pergunta aberta anteriormente em `dias_mes` não é fechada por `fcModo()` e pode permanecer após uma troca de modo pelo teclado. Reprodução: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:139`, `:174`; `app/templates/core/_dialogo.html:193`)

Há outra inconsistência na sequência **trocar para semana → editar nome**: `fcModo()` limpa `dias_*`, mas o input do nome chama `renderTudo()` e os repõe. A view ignora esses campos em `semana_dia`; isso não demonstra gravação indevida. (`app/templates/escala/folga_calendario_editar.html:112`, `:130`, `:178`; `app/escala/views.py:327`)

4. **NÃO PROCEDE — repetir fora do tamanho pelo fluxo normal.**

Os dias clicáveis nascem em `1..t.n`; repetir conserva somente `d <= t.n`, exatamente o corte do criador. (`app/templates/escala/folga_calendario_editar.html:90`, `:127`; `app/escala/folgas.py:661`)

O corte não valida o limite inferior, e a leitura do legado não saneia os dias. Portanto, garantia absoluta para calendários previamente malformados não está demonstrada; existência desses registros: **[não verificado]**. (`app/escala/views.py:362`, `app/templates/escala/folga_calendario_editar.html:127`)

5. **PROCEDE — os quatro mutantes indicados têm mordida identificável, mas há lacunas.**

As falhas abaixo são previstas pela leitura; não são resultados de execução. **[não verificado]**

| Mutante | Asserção que deve falhar |
|---|---|
| Tirar a marca | Contagem de marcas vazias; se todas ficarem visíveis, contraponto do calendário completo. (`app/escala/tests/test_editor_calendario_tres_grades.py:99`, `:109`) |
| Tirar o guarda de submit | Exigência de modal e de `SEM-POST` ao cancelar. (`app/escala/tests/test_smoke_clique_folga_calendario.py:174`, `:183`, `:186`) |
| Repetir sem corte | Comparação dos campos capturados **antes** da filtragem da view. (`app/escala/tests/test_smoke_clique_folga_calendario.py:195`, `:196`) |
| Sonda em captura | Cancelar encontrará POST capturado em vez de `SEM-POST`. A sonda atual respeita `defaultPrevented` em bolha. (`app/escala/tests/test_smoke_clique_folga_calendario.py:51`, `:57`, `:186`) |

**Lacuna relevante:** esconder as seções 30/28 pode manter os testes verdes. O teste das três grades verifica seções/IDs; a marca “visível” considera apenas `hidden` no próprio span; o smoke usa `.click()` programático sem medir visibilidade. Assim, a promessa “três grades ao mesmo tempo” não está protegida contra ocultação. (`app/escala/tests/test_editor_calendario_tres_grades.py:82`, `:48`; `app/escala/tests/test_smoke_clique_folga_calendario.py:50`)

Há vacuidade **local** na contagem de registros antes/depois de Cancelar: esse teste nunca chama `_reenvia()`, que realiza o POST Django. A asserção `SEM-POST` continua útil. (`app/escala/tests/test_smoke_clique_folga_calendario.py:162`, `:179`, `:186`, `:187`)

A regex de `hxConfirmar` não prova execução; o `wraps` prova uma chamada real no cadastro, mas não exclui um segundo update redundante nem cobre esse spy na edição. (`app/escala/tests/test_editor_calendario_tres_grades.py:118`, `app/escala/tests/test_folga_cadastro.py:19`, `:43`, `:45`)

6. **PROCEDE — infração à L7.**

A ajuda adicional “Cada tamanho de mês...” está hardcoded em um `<div>`, fora de tooltip/componente e de mapa compartilhado. Isso confronta diretamente a L7. A marca visível exigida pelo aval é questão distinta. (`app/templates/escala/folga_calendario_editar.html:40`, `app/docs/LEIS-UI.md:77`)

**NÃO PROCEDE — infração identificada aos selos estruturais examinados.** Não há diálogo nativo ou `title` cru novo; não identifiquei atributo inválido, fonte Python ou comentário vazando. Resultado executado dos selos: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:50`, `:146`; `app/core/tests/test_contract_l8_dialogo.py:42`, `app/core/tests/test_contract_template_bem_formado.py:162`, `:169`, `:174`)

7. **NÃO PROCEDE — relógio novo fora da borda ou dois significados para vazio.**

`t.marcados` é a seleção carregada; `sel[tam]`, sua versão editável. Marca e atributo significam “seleção vazia” e acompanham o mesmo conjunto. Não significam “a pessoa não terá nenhuma outra origem de folga”. (`app/escala/views.py:362`, `app/templates/escala/folga_calendario_editar.html:51`, `:82`, `:97`, `:100`, `:145`)

O relógio civil permanece na porta e é repassado aos consumidores. A classe “selo sem mordida” procede parcialmente pelas lacunas descritas no item 5. (`app/escala/folgas.py:599`, `app/escala/folgas.py:601`; `app/escala/tests/test_editor_calendario_tres_grades.py:82`)

**VEREDITO: DEVOLVE 3, 5, 6.**

## 16/11 15:15 · sessao 5db8ccbd2ba6 · - · - · origem=Ronald (tmux)

Auditoria por leitura; testes não executados. [não verificado] [nao verificado]

**5 — PROCEDE: os quatro mutantes indicados têm mordida identificável.**

| Mutante | Teste que fica vermelho | [nao verificado]
|---|---|
| Retirar a marca | `app/escala/tests/test_editor_calendario_tres_grades.py:99`; remover apenas condição e deixar todas visíveis falha em `:109`. |
| Retirar guarda do submit | Smoke exige modal em `app/escala/tests/test_smoke_clique_folga_calendario.py:174` e `:183`; Cancelar exige `SEM-POST` em `:186`. A regex isolada `test_editor_calendario_tres_grades.py:121` sobreviveria se a chamada continuasse em código morto. |
| Repetir sem corte | FormData anterior à view precisa conter dias exatos em `app/escala/tests/test_smoke_clique_folga_calendario.py:195` e `:196`; a persistência isolada esconderia esse defeito, conforme `:193`. |
| Sonda em captura | Registraria o evento antes do guarda; Cancelar encontraria JSON em vez de `SEM-POST`, falhando em `app/escala/tests/test_smoke_clique_folga_calendario.py:186`. Ordem atual: listener de documento em bolha e respeito a `defaultPrevented` em `:51–57`; guarda no form em `app/templates/escala/folga_calendario_editar.html:137`. |

**PROCEDE: existem limites e afirmações excessivas nos selos.**

- A contagem antes/depois em Cancelar é vacuamente verdadeira quanto à persistência: esse teste não chama `_reenvia`, único ponto que POSTa ao Django; `app/escala/tests/test_smoke_clique_folga_calendario.py:162`, `:179`, `:187`. O `SEM-POST` continua sendo uma asserção útil, `:186`.
- “Três grades ao mesmo tempo” não está protegido contra ocultação: teste verifica seções/IDs, `app/escala/tests/test_editor_calendario_tres_grades.py:82`; `_marcas_visiveis` considera apenas `hidden` do próprio span, `:48`. Esconder seções 30/28 com CSS pode sobreviver, pois smoke usa `.click()` programático sem medir visibilidade, `app/escala/tests/test_smoke_clique_folga_calendario.py:50`.
- `wraps` e `call_count` provam chamada real e resultado do cadastro, não ausência universal de outros escritores: um segundo update com valores iguais sobreviveria; `app/escala/tests/test_folga_cadastro.py:43`, `:45`, `:49`. Edição não passa por esse spy, `:19`.

**6 — PROCEDE: L7 merece devolução.**

A ajuda adicional é prosa fixa em `<div>`, fora de tooltip/componente/mapa: `app/templates/escala/folga_calendario_editar.html:40`; L7 proíbe ajuda hardcoded por tela em `app/docs/LEIS-UI.md:77`. O texto de confirmação usa o componente autorizado, mas continua fixo no template sem mapa compartilhado: `app/templates/escala/folga_calendario_editar.html:144`. Não conte o texto visível da marca como infração por ser visível: foi exigido expressamente pelo usuário; sua implementação está em `:50`.

**NÃO PROCEDE: diálogo nativo/title cru novos.** A chamada é `hxConfirmar`, `app/templates/escala/folga_calendario_editar.html:146`; o varredor distingue essa chamada de `confirm/alert/prompt/hx-confirm`, `app/core/tests/test_contract_l8_dialogo.py:41`. A marca tem texto e não usa `title`, `app/templates/escala/folga_calendario_editar.html:50`.

**NÃO PROCEDE: violação estrutural identificada nos varredores examinados.** Novos elementos não trazem atributos inválidos, fonte Python ou comentário vazando; critérios em `app/core/tests/test_contract_template_bem_formado.py:162`, `:169`, `:174`. A regex real de interpolação crua não encontrou correspondência neste template, critério `:123` e teste `:180`; verificação feita em memória, sem arquivo. A confirmação resolve `hxConfirmar` no handler, `app/templates/escala/folga_calendario_editar.html:146`, sem captura no load proibida por `app/core/tests/test_hx_nao_se_captura_no_load.py:21`.

**7 — NÃO PROCEDE: relógio fora da borda introduzido.** `setTimeout(200)` serve à sonda, `app/escala/tests/test_smoke_clique_folga_calendario.py:67`; não lê data/hora civil, alvo do selo em `app/core/tests/test_selo_teste_sem_relogio.py:32`.

## 16/11 15:15 · sessao 3c88f6c0f911 · - · - · origem=Ronald (tmux)

**Q3 — NÃO PROCEDE no fluxo normal; há duas ressalvas de estado.**

- Clique, Enter e `requestSubmit()` encontram o guarda instalado no formulário; grade vazia em `dias_mes` cancela o envio antes de chamar `hxConfirmar`. (`app/templates/escala/folga_calendario_editar.html:137`, `:139`, `:140`, `:143`, `:146`)
- Cancelar não reenvia; o único `form.submit()` local fica depois da resposta positiva. (`app/templates/escala/folga_calendario_editar.html:147`, `:149`)
- Modal ausente não libera envio: `preventDefault()` já aconteceu; o componente retorna falso quando faltam seus elementos e `naoBloquear` não foi passado. Se a função inteira estiver indisponível, a exceção também acontece depois do cancelamento. (`app/templates/escala/folga_calendario_editar.html:143`, `:146`; `app/templates/core/_dialogo.html:184`)
- Submeter em `semana_dia` não abre confirmação. Voltar para `dias_mes` reconstitui os campos da seleção. (`app/templates/escala/folga_calendario_editar.html:139`, `:174`, `:178`, `:179`)

**Ressalva 1 — PROCEDE: confirmação pode ficar latente.** `confirmado=true` só é consumido pelo próximo evento `submit`. Se o `requestSubmit()` não produzir esse evento por validação inválida, a autorização fica para uma tentativa posterior. Contraexemplo por inspeção: abrir o modal com nome válido, alcançar o nome pelo teclado e apagá-lo enquanto a promessa está pendente, confirmar, preencher novamente e salvar; não haverá nova pergunta. O componente não prende o foco nem torna o formulário inerte. Esse caminho já passou por uma confirmação anterior; não equivale a “nunca chamou `hxConfirmar`”. Execução em navegador: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:148`, `:149`; `app/templates/core/_dialogo.html:119`, `:160`, `:195`)

**Ressalva 2 — PROCEDE: trocar modo e depois editar nome repõe `dias_*`.** `fcModo()` limpa os campos em `semana_dia`, mas editar o nome chama `renderTudo()` → `syncHidden()` sem conferir o modo. A view ignora esses dias ao processar `semana_dia`, portanto isso não prova gravação indevida. (`app/templates/escala/folga_calendario_editar.html:112`, `:117`, `:130`, `:178`; `app/escala/views.py:327`, `:339`)

**Modal visível depois da troca para `semana_dia`:** pode permanecer um modal aberto anteriormente em `dias_mes`, pois `fcModo()` não o fecha; nova submissão em `semana_dia` não o abre. Reprodução por teclado: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:139`, `:174`; `app/templates/core/_dialogo.html:193`)

## 16/11 15:14 · sessao c243c82dc789 · - · - · origem=Ronald (tmux)

1. **NÃO PROCEDE — duplicidade de autoridade nos campos.**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `dias_<tam>` | `syncHidden`, a partir de `sel[tam]`: `app/templates/escala/folga_calendario_editar.html:113` e `:117` | View lê POST em `app/escala/views.py:342`, valida em `:347`, repassa à porta em `:356`; porta persiste em `app/escala/folgas.py:593` |
| `data-fc-vazia` | SSR com `t.marcados`: template `:44`; atualização por `arr.length`: `:100` | Teste `_vazia`: `app/escala/tests/test_editor_calendario_tres_grades.py:44`; confirmação não consulta esse atributo, consulta `sel.size`: template `:140` |
| `fc-marca-vazia` | Elemento/classe no SSR: template `:50`; visibilidade atualizada por `arr.length`: `:99` | Leitura do HTML pelo teste: `app/escala/tests/test_editor_calendario_tres_grades.py:51`; usuário recebe texto em template `:51` |
| `sel[tam]` | Inicialização do seed: template `:82`; interação: `:122`; repetir: `:127` | Grade `:89`, ordenação `:85`, campos POST `:116`, confirmação `:140`, origem do repetir `:127` |
| `modo` | Radios template `:20`; persistência na porta `app/escala/folgas.py:585` e `:590`; contexto view `:370` | View POST `:325`; guarda JS template `:139`; troca visual `:175`; porta `app/escala/folgas.py:584` |
| `t.marcados` | View projeta `cal.tamanhos.all()` em `app/escala/views.py:362` e `:366`; origem persistida `app/escala/folgas.py:595` | SSR template `:44`, `:49`, `:50`; seed `:77`, posteriormente convertido em `sel`: `:82` |

2. **NÃO PROCEDE — segundo escritor persistente, juiz novo ou derivação decisória paralela.** A view continua entrando pela porta em `app/escala/views.py:354`; escrita fica em `app/escala/folgas.py:571` e `:594`. `data-fc-vazia`, marca e contagem são projeções do mesmo conjunto em template `:97`–`:100`; o guarda consulta esse conjunto em `:140`. A distinção entre lookup e registro vivo consta em `app/docs/JUIZES.md:7` e `:11`.

4. **NÃO PROCEDE — repetir sem corte no fluxo normal da UI.** Dias clicáveis nascem em `1..t.n`, template `:90`; o repetir filtra `d <= t.n`, `:127`, igual ao criador em `app/escala/folgas.py:661`. Ambos deixam de validar o limite inferior nesse corte; seed legado é carregado sem saneamento em `app/escala/views.py:362`. Existência de legado com dia zero/negativo: **[não verificado]**.

7. **NÃO PROCEDE — campo com dois significados ou relógio novo.** `t.marcados` representa a seleção carregada, `sel[tam]` a seleção editável; atributo, marca e contagem acompanham sua cardinalidade em template `:82`, `:97`–`:100`. O relógio existente permanece na porta em `app/escala/folgas.py:599`, repassado em `:601`.

**Observação de L7:** a nova ajuda hardcoded em `app/templates/escala/folga_calendario_editar.html:41` confronta a exigência de ajuda em componente/mapa de `app/docs/LEIS-UI.md:77`–`:85`; isso é distinto da marca visível explicitamente solicitada.

## 16/11 14:45 · sessao 427f2c880889 · PRIORIDADE-EV-ITEM1 · sonda · origem=Code

1. A saída informa **`TOTAL|parte=3/3|dias=19|ok=19|erro=0`** e **`rc_sombra=0`**. A linha desses registros no arquivo não foi fornecida: [nao verificado]. **Disparo de tripwire: [nao verificado]**; não há resultado explícito desse controle no trecho apresentado.

2. Na tabela, **espelho = `minutos_realizados` e classe = `STATUS.veredito`**; o E6 é o inteiro da linha do universo, confirmado por `confere_e6_arquivo=true`. (`app/ponto/services/espelho.py:783-819,764-895`; `app/ponto/management/commands/e6_oraculo.py:263-269,401`)

| Código | Data | Espelho: minutos; classe | E6 universo | Categoria/causa apontada pelos detalhes |
|---|---|---|---:|---|
| col220 | 2026-11-10 | 337; `trabalhou` | 430 | **Pareamento e desconto diferentes:** espelho usa turno até 04:31 e desconta 60 min; E6 inclui 06:00 e forma dois pares, com pausa de 55,643 min. (`estrutura_dias.txt:39`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-09 | 551; `trabalhou` | 597 | **Pausa abaixo do piso:** espelho não reconhece a pausa curta e desconta 60 min declarados; E6 desconta 13,556 min entre batidas. (`estrutura_dias.txt:40`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-10 | 551; `trabalhou` | 598 | **Pausa abaixo do piso:** desconto de 60 min no espelho versus 13,463 min no E6. (`estrutura_dias.txt:41`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-11 | 550; `trabalhou` | 598 | **Pausa abaixo do piso:** desconto de 60 min no espelho versus 12,814 min no E6. (`estrutura_dias.txt:42`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-13 | 551; `trabalhou` | 597 | **Pausa abaixo do piso:** desconto de 60 min no espelho versus 13,942 min no E6. (`estrutura_dias.txt:43`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col296 | 2026-11-02 | 345; `trabalhou` | 0 | **Associação ao dia divergente:** espelho atribui ao dia 02 um turno de 03/11, 00:15–07:00, calculando 405−60; E6 tem zero turnos no dia 02. (`estrutura_dias.txt:44`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col37 | 2026-11-14 | 241; `trabalhou` | 0 | **Associação ao dia divergente:** espelho atribui ao dia 14 um turno de 15/11, 02:00–07:01, calculando 301−60; E6 tem zero turnos no dia 14. (`estrutura_dias.txt:45`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col382 | 2026-11-10 | 418; `trabalhou` | 0 | **Associação ao dia divergente:** espelho usa turno de 11/11, 00:02–07:51, com pausa reconhecida de 51 min: 469−51; E6 tem zero turnos no dia 10. (`estrutura_dias.txt:46`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col516 | 2026-10-23 | 361; `folga` | 0 | **Folga com trabalho apurado:** espelho conta 01:00–07:01, sem desconto; célula registra `fato_sem_previsao` e há alerta `folga_trabalhada`. E6 tem zero turnos nesse dia; causa completa desse zero: [nao verificado]. (`estrutura_dias.txt:47`; `app/ponto/turnos.py:725-767`; `app/escala/models.py:1652-1701`; `app/ponto/services/espelho.py:450-460,767`) |
| col516 | 2026-10-28 | 372; `trabalhou` | 0 | **Associação ao dia divergente:** espelho atribui ao dia 28 o turno de 29/10, 00:43–06:55, sem desconto; E6 tem zero turnos no dia 28. (`estrutura_dias.txt:48`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col727 | 2026-11-10 | 307; `em_aberto` | 0 | **Associação ao dia e classificação divergentes:** espelho atribui ao dia 10 o turno fechado de 11/11, 01:00–07:07, calculando 367−60; mantém classe `em_aberto`. E6 tem zero turnos no dia 10; o motor também aponta `dia_sem_previsto`/`folga_trabalhada`. (`estrutura_dias.txt:49`; `app/ponto/services/espelho.py:764-895,450-460`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col874 | 2026-10-22 | 667; `trabalhou` | 383 | **Corte e pareamento diferentes:** espelho conta 10:00–22:07 menos 60 min; no E6, 10:00 fecha fragmento iniciado em 21/10, e o dia 22 recebe somente 15:45–22:07. (`estrutura_dias.txt:50`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col884 | 2026-11-08 | 679; `rejeitado` | 699 | **Desconto e estado de ausência:** espelho desconta 60 min sem pausa reconhecida; E6 desconta 39,546 min posicionais. Há atestado `rejeitada` cobrindo o dia. (`estrutura_dias.txt:51`; `app/ponto/turnos.py:725-767,2798-2814`; `app/ponto/calculador/nucleo.py:224-247`; `app/ponto/models.py:690-705,743-754`) |
| col909 | 2026-11-11 | 831; `em_aberto` | 245 | **Borda posicional versus cortes:** realizado usa 07:00–23:00 menos 129 min, embora haja um turno fechado e outro aberto. E6 conta apenas 07:00–11:04 e registra `impar=true`, `corte_que_orfanou=true`. (`estrutura_dias.txt:52`; `app/ponto/turnos.py:598-601,707-767`; `app/ponto/calculador/nucleo.py:215-254`) |
| col922 | 2026-11-01 | 1382; `trabalhou` | 68 | **Turnos e pares diferentes:** realizado soma 901+481 min, sem desconto; espelho exibe apenas o turno noturno. E6 atribui ao dia somente o par 22:01–23:09, entre uma saída e uma entrada. (`estrutura_dias.txt:53`; `app/ponto/turnos.py:725-767`; `app/ponto/services/espelho.py:450-460,767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col922 | 2026-11-04 | 676; `trabalhou` | 458 | **Turno adicional no realizado:** soma 218 min da madrugada e 458 min do turno noturno; espelho exibe só o noturno. E6 mantém a madrugada no fragmento iniciado em 03/11 e atribui ao dia 04 apenas o noturno. (`estrutura_dias.txt:54`; `app/ponto/turnos.py:725-767`; `app/ponto/services/espelho.py:450-460,767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col989 | 2026-10-23 | 721; `trabalhou` | 567 | **Pareamento deslocado:** espelho conta entrada 17:59 até saída 06:00 seguinte, sem desconto; E6 pareia a saída 08:32 com a entrada 17:59 e corta antes de 06:00. (`estrutura_dias.txt:55`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col996 | 2026-10-28 | 0; `folga` | 580 | **Saída órfã versus par posicional:** juiz apresenta apenas saída às 21:18, sem entrada, e o espelho não exibe turno; E6 pareia essa saída com a entrada de 29/10 às 06:57. (`estrutura_dias.txt:56`; `app/ponto/services/espelho.py:267-294,450-460`; `app/ponto/calculador/nucleo.py:224-247`) |
| col996 | 2026-11-10 | 443; `em_aberto` | 1026 | **Par adicional no E6:** realizado conta o turno diurno, 533−90; mantém saída órfã às 21:16. E6 acrescenta o par dessa saída com a entrada de 11/11 às 06:59. (`estrutura_dias.txt:57`; `app/ponto/turnos.py:725-767`; `app/ponto/services/espelho.py:267-294`; `app/ponto/calculador/nucleo.py:224-247`) |

3. **Dias com `ERRO`: nenhum segundo o TOTAL informado (`erro=0`); portanto, nenhuma exclusão numérica por ERRO nesta parte.** Arquivo:linha do registro TOTAL: [nao verificado].

## 16/11 14:41 · sessao c91636553c1d · PRIORIDADE-EV-ITEM1 · sonda · origem=Code

1. **TOTAL da parte:** `N=19; ok=19; erro=0; rc_sombra=0`, conforme as linhas finais fornecidas; localização dessas linhas em arquivo:linha **[nao verificado]**. **Tripwire:** disparo em algum dia **[nao verificado]**; `rc_sombra=0` não fornece, sozinho, esse resultado.

2. A classe abaixo é `STATUS.veredito`; os minutos do espelho são `minutos_realizados`. (`app/ponto/services/espelho.py:764-819`) A coluna e6 usa o inteiro conferido contra a linha do universo indicada em cada registro. (`app/ponto/management/commands/e6_oraculo.py:263-269,401`)

| Código, data | Espelho: minutos / classe | e6 do universo | Categoria/causa evidenciada na saída |
|---|---:|---:|---|
| col723, 2026-10-08 | 440 / trabalhou | 345 (`estrutura_dias.txt:20`) | **Desconto de intervalo diferente:** realizado desconta 60 min declarados, sem pausa reconhecida; e6 desconta 155,277 min entre os dois pares posicionais. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col746, 2026-09-24 | 289 / trabalhou | 193 (`estrutura_dias.txt:21`) | **Desconto de intervalo diferente:** realizado desconta 60 min declarados; e6 desconta 156,819 min entre os pares. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col841, 2026-10-20 | 681 / em_aberto | 522 (`estrutura_dias.txt:22`) | **Agrupamento e pareamento diferentes:** juiz mantém uma saída isolada às 11:00 e o turno 18:46–07:04, com desconto de 57 min; e6 pareia 11:00–18:46 e 21:44–22:41, cortando antes da saída das 07:04. (`app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col854, 2026-10-04 | 718 / trabalhou | 7 (`estrutura_dias.txt:23`) | **Corte do e6:** realizado conta 18:54–06:52, sem desconto; e6 pareia somente 18:54–19:01 e corta antes das 06:52, após um gap de 711,349 min. (`app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col864, 2026-10-12 | 509 / feriado | 449 (`estrutura_dias.txt:24`) | **Intervalo sem desconto no realizado:** nenhuma pausa reconhecida e nenhuma janela declarada usada; e6 desconta 59,804 min. A célula consta como folga, sem marcos. (`app/ponto/turnos.py:707-771`; `app/ponto/calculador/nucleo.py:224-254`; `app/escala/models.py:1652-1701,1718-1724`) |
| col868, 2026-09-22 | 0 / em_aberto | 679 (`estrutura_dias.txt:25`) | **Formação de turno diferente:** juiz tem apenas saída sem entrada e nenhum turno exibido; e6 agrupa quatro batidas entre 18:48 e 07:01 do dia seguinte. O cadastro mostrado prevê jornada diurna 6x1; dono dessa divergência **[nao verificado]**. (`app/ponto/turnos.py:502-567,719`; `app/ponto/services/espelho.py:450-460,767`; `app/ponto/calculador/nucleo.py:224-247`; `app/escala/models.py:103-138,170-175`) |
| col876, 2026-09-21 | 550 / em_aberto | 665 (`estrutura_dias.txt:26`) | **Conjunto de batidas diferente:** realizado conta 18:29–04:39 menos 60 min e mantém a saída das 06:31 sem entrada; e6 também usa 05:36 e conta o segundo par até 06:31. Motivo de 05:36 não aparecer no turno do juiz **[nao verificado]**. (`app/ponto/turnos.py:502-567,725-771`; `app/ponto/calculador/nucleo.py:224-247`) |
| col1021, 2026-11-06 | 841 / folga | 0 (`estrutura_dias.txt:27`) | **Corte que orfanou no e6:** realizado mantém 05:55–19:56, sem desconto; e6 separa as duas batidas em fragmentos unitários, com `impar=true` e `corte_que_orfanou=true`. (`app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-254`) |
| col110, 2026-11-12 | 241 / em_aberto | 0 (`estrutura_dias.txt:28`) | **Atribuição ao dia:** juiz atribui a 12/11 o trecho 01:59–07:00 de 13/11 e desconta 60 min; e6 não tem turno atribuído a 12/11. Destino dessas batidas no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col112, 2026-10-21 | 546 / trabalhou | 838 (`estrutura_dias.txt:29`) | **Pareamento noturno no e6:** realizado conta 06:57–17:03 menos 60 min; e6 pareia a saída das 17:03 com a entrada das 07:01 de 22/10. A entrada das 06:57 aparece no fragmento anterior. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:215-247`) |
| col112, 2026-10-22 | 541 / trabalhou | 835 (`estrutura_dias.txt:30`) | **Pareamento noturno no e6:** realizado conta 07:01–17:02 menos 60 min; e6 pareia 17:02 com 06:56 de 23/10. A entrada das 07:01 aparece no fragmento anterior. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:215-247`) |
| col113, 2026-10-30 | 242 / em_aberto | 0 (`estrutura_dias.txt:31`) | **Atribuição ao dia:** juiz mantém saída isolada às 02:00 e conta 03:00–07:02 de 31/10 em 30/10, sem desconto; e6 não tem turno em 30/10. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col113, 2026-11-01 | 241 / em_aberto | 0 (`estrutura_dias.txt:32`) | **Atribuição ao dia:** juiz mantém saída isolada às 02:01 e conta 03:00–07:01 de 02/11 em 01/11, sem desconto; e6 não tem turno em 01/11. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col114, 2026-11-08 | 439 / trabalhou | 429 (`estrutura_dias.txt:33`) | **Desconto de intervalo diferente:** realizado desconta 60 min declarados, sem pausa reconhecida; e6 desconta 70,666 min entre os pares. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col114, 2026-11-15 | 499 / feriado | 440 (`estrutura_dias.txt:34`) | **Intervalo sem desconto no realizado:** realizado conta o envelope sem pausa reconhecida ou janela declarada; e6 desconta 59,538 min. (`app/ponto/turnos.py:707-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col130, 2026-10-29 | 335 / trabalhou | 672 (`estrutura_dias.txt:35`) | **Conjunto e pareamento diferentes:** juiz encerra às 01:31 e desconta 60 min; e6 inclui 07:06 e forma os pares 18:56–00:33 e 01:31–07:06. Motivo da exclusão de 07:06 pelo juiz **[nao verificado]**. (`app/ponto/turnos.py:502-567,725-771`; `app/ponto/calculador/nucleo.py:224-247`) |
| col200, 2026-10-29 | 180 / em_aberto | 0 (`estrutura_dias.txt:36`) | **Atribuição ao dia:** juiz conta 02:00–06:00 de 30/10 em 29/10 e desconta 60 min; e6 não tem turno em 29/10. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col200, 2026-11-02 | 180 / em_aberto | 0 (`estrutura_dias.txt:37`) | **Atribuição ao dia:** juiz mantém uma saída isolada e conta 02:00–06:00 de 03/11 em 02/11, menos 60 min; e6 não tem turno em 02/11. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col218, 2026-11-14 | 103 / em_aberto | 0 (`estrutura_dias.txt:38`) | **Atribuição ao dia:** juiz mantém saída isolada às 02:17 e conta 03:17–05:00 de 15/11 em 14/11, sem desconto; e6 não tem turno em 14/11. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-767`; `app/ponto/calculador/nucleo.py:224-254`) |

**Dono ORACULO/SISTEMA/CADASTRO por causa:** **[nao verificado]**; os mecanismos acima não bastam para atribuir responsabilidade definitiva.

3. **Dias com ERRO:** nenhum registrado no `TOTAL` fornecido (`erro=0`); arquivo:linha desse registro **[nao verificado]**. Portanto, não há dia listado para invalidar por ERRO nessa parte; arquivo:linha **[nao verificado]**.

## 16/11 14:38 · sessao af08e3068de7 · PRIORIDADE-EV-ITEM1 · sonda · origem=Code

1. **TOTAL da parte:** `N=19 | ok=19 | erro=0 | rc_sombra=0`, conforme os registros finais fornecidos; arquivo:linha desses registros **[nao verificado]**. **Tripwire:** disparo **[nao verificado]**; não há resultado explícito dele na saída.

2. Na tabela, **minutos do espelho = `minutos_realizados`** e **classe = `STATUS.veredito`**, conforme as referências da sonda: `app/ponto/services/espelho.py:783-819,764-895`. As categorias abaixo descrevem os mecanismos apresentados, sem atribuir dono definitivo **[nao verificado]**.

| Código | Data | Espelho: minutos; classe | e6 do universo | Categoria/causa apontada |
|---|---|---|---:|---|
| col129 | 2026-10-12 | 0; `em_aberto` | 779 | **Pareamento diferente:** juiz recebeu duas saídas sem entrada; e6 pareou a saída de 17:30 com a saída de 06:30 do dia seguinte. `estrutura_dias.txt:1`; `app/ponto/turnos.py:502-567,719`; `app/ponto/calculador/nucleo.py:224-247`. |
| col218 | 2026-10-15 | 105; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 21h; espelho conta o par 03:15–05:00 do dia 16 no dia 15; e6 não apresenta turno no dia 15. `estrutura_dias.txt:2`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-23 | 548; `trabalhou` | 594 | **Desconto de intervalo:** realizado desconta 60 min declarados; pausa de 13,3619 min fica abaixo do piso reconhecido; e6 desconta essa pausa. `estrutura_dias.txt:3`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-24 | 552; `trabalhou` | 598 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 13,569383 min é descontada pelo e6. `estrutura_dias.txt:4`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-25 | 556; `trabalhou` | 604 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 12,438 min é descontada pelo e6. `estrutura_dias.txt:5`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-28 | 535; `trabalhou` | 581 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 14,634152 min é descontada pelo e6. `estrutura_dias.txt:6`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-01 | 545; `trabalhou` | 590 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 14,443733 min é descontada pelo e6. `estrutura_dias.txt:7`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-02 | 545; `trabalhou` | 593 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 13,069783 min é descontada pelo e6. `estrutura_dias.txt:8`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-06 | 551; `trabalhou` | 597 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 14,038883 min é descontada pelo e6. `estrutura_dias.txt:9`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-09 | 544; `trabalhou` | 592 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 11,989283 min é descontada pelo e6. `estrutura_dias.txt:10`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col369 | 2026-10-12 | 476; `feriado` | 416 | **Intervalo não reconhecido no realizado:** célula de folga sem marcos; batidas internas E–S, nenhuma pausa reconhecida e nenhum desconto. e6 desconta 59,582783 min entre as batidas internas. `estrutura_dias.txt:11`; `app/escala/models.py:1652-1701`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col37 | 2026-10-05 | 241; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; juiz tem saída isolada às 02h e par 03:00–07:01 do dia 6 atribuído ao dia 5; e6 não apresenta turno no dia 5. `estrutura_dias.txt:12`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col37 | 2026-10-09 | 241; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; saída isolada às 02h e par 03:00–07:01 do dia 10 atribuído ao dia 9; e6 não apresenta turno no dia 9. `estrutura_dias.txt:13`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col382 | 2026-09-30 | 364; `trabalhou` | 0 | **Atribuição do turno noturno com pausa reconhecida:** espelho atribui 01:01–07:50 do dia 1 ao dia 30 e calcula 409−45=364 min; e6 não apresenta turno no dia 30. `estrutura_dias.txt:14`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col451 | 2026-09-23 | 707; `em_aberto` | 718 | **Pareamento diferente:** espelho conta 18:58–06:45 do dia seguinte; e6 pareia a saída isolada de 07h com a entrada de 18:58 e corta antes da saída seguinte. `estrutura_dias.txt:15`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`. |
| col451 | 2026-09-25 | 705; `em_aberto` | 718 | **Pareamento diferente:** espelho conta 19:00–06:45 do dia seguinte; e6 pareia a saída isolada de 07:02 com a entrada de 19h e corta antes da saída seguinte. `estrutura_dias.txt:16`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`. |
| col478 | 2026-09-25 | 719; `trabalhou` | 8 | **Entrada adicional e corte do e6:** espelho usa 18:57–06:56, com a entrada de 19:04 marcada `X`; e6 pareia as duas entradas, obtém 7,766517 min e corta antes da saída. `estrutura_dias.txt:17`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`. |
| col515 | 2026-10-09 | 244; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; saída isolada às 02h e par 02:59–07:03 do dia 10 atribuído ao dia 9; e6 não apresenta turno no dia 9. `estrutura_dias.txt:18`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col515 | 2026-10-11 | 245; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; saída isolada às 01:50 e par 03:00–07:05 do dia 12 atribuído ao dia 11; e6 não apresenta turno no dia 11. `estrutura_dias.txt:19`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |

3. **Dias com ERRO: nenhum**, conforme `TOTAL|parte=1/3|dias=19|ok=19|erro=0`; arquivo:linha desse registro **[nao verificado]**. Portanto, nenhuma exclusão de número por ERRO foi indicada nessa parte; arquivo:linha **[nao verificado]**.

## 16/11 14:36 · sessao a12a65a6e084 · PRIORIDADE-EV-ITEM6 · sonda · origem=Code

[nao verificado]
```text
PEND|col373|2026-08-04|08/2026|emp2
PEND|col60|2026-06-23|07/2026|emp3
PEND|col152|2026-06-23|07/2026|emp3
PEND|col152|2026-06-29|07/2026|emp3
PEND|col888|2026-08-28|09/2026|emp2
PEND|col920|2026-09-02|09/2026|emp2
PEND|col616|2026-09-05|09/2026|emp2
PEND|col920|2026-09-04|09/2026|emp2
PEND|col727|2026-09-04|09/2026|emp2
PEND|col439|2026-09-03|09/2026|emp2
PEND|col212|2026-08-26|09/2026|emp2
PEND|col112|2026-09-06|09/2026|emp3
PEND|col920|2026-09-06|09/2026|emp2
PEND|col926|2026-09-07|09/2026|emp2
PEND|col152|2026-09-07|09/2026|emp3
PEND|col236|2026-09-07|09/2026|emp2
PEND|col363|2026-09-07|09/2026|emp2
PEND|col358|2026-09-07|09/2026|emp2
PEND|col928|2026-09-07|09/2026|emp2
PEND|col878|2026-09-07|09/2026|emp2
PEND|col507|2026-09-07|09/2026|emp2
PEND|col49|2026-09-07|09/2026|emp3
PEND|col112|2026-09-08|09/2026|emp3
PEND|col920|2026-09-08|09/2026|emp2
PEND|col616|2026-09-07|09/2026|emp2
PEND|col148|2026-09-10|09/2026|emp3
PEND|col112|2026-09-10|09/2026|emp3
PEND|col920|2026-09-10|09/2026|emp2
PEND|col616|2026-09-09|09/2026|emp2
PEND|col920|2026-09-12|09/2026|emp2
PEND|col616|2026-09-11|09/2026|emp2
PEND|col862|2026-09-13|09/2026|emp2
PEND|col242|2026-09-13|09/2026|emp2
PEND|col901|2026-09-14|09/2026|emp2
PEND|col920|2026-08-27|09/2026|emp2
PEND|col920|2026-08-29|09/2026|emp2
PEND|col920|2026-08-31|09/2026|emp2
PEND|col207|2026-09-05|09/2026|emp2
PEND|col869|2026-09-06|09/2026|emp2
PEND|col869|2026-09-13|09/2026|emp2
PEND|col899|2026-09-05|09/2026|emp2
PEND|col903|2026-09-14|09/2026|emp2
PEND|col129|2026-08-30|09/2026|emp3
PEND|col129|2026-09-08|09/2026|emp3
PEND|col129|2026-09-13|09/2026|emp3
PEND|col129|2026-09-15|09/2026|emp3
PEND|col382|2026-09-03|09/2026|emp2
PEND|col491|2026-08-27|09/2026|emp2
PEND|col382|2026-09-10|09/2026|emp2
PEND|col382|2026-09-17|09/2026|emp2
PEND|col616|2026-09-17|09/2026|emp2
PEND|col901|2026-09-17|09/2026|emp2
PEND|col572|2026-09-18|09/2026|emp2
PEND|col648|2026-09-15|09/2026|emp2
PEND|col148|2026-09-13|09/2026|emp3
PEND|col902|2026-09-13|09/2026|emp2
PEND|col824|2026-09-15|09/2026|emp2
PEND|col824|2026-09-19|09/2026|emp2
PEND|col902|2026-09-19|09/2026|emp2
PEND|col499|2026-09-19|09/2026|emp2
PEND|col148|2026-09-19|09/2026|emp3
PEND|col822|2026-09-20|09/2026|emp2
PEND|col572|2026-09-20|09/2026|emp2
PEND|col491|2026-09-20|09/2026|emp2
PEND|col902|2026-09-20|09/2026|emp2
PEND|col148|2026-09-20|09/2026|emp3
PEND|col491|2026-09-21|10/2026|emp2
PEND|col616|2026-09-21|10/2026|emp2
PEND|col824|2026-09-21|10/2026|emp2
PEND|col903|2026-09-21|10/2026|emp2
PEND|col704|2026-09-21|10/2026|emp2
PEND|col857|2026-09-22|10/2026|emp4
PEND|col418|2026-09-22|10/2026|emp2
PEND|col245|2026-09-22|10/2026|emp2
PEND|col203|2026-09-22|10/2026|emp2
PEND|col616|2026-09-23|10/2026|emp2
PEND|col824|2026-09-23|10/2026|emp2
PEND|col859|2026-09-23|10/2026|emp2
PEND|col951|2026-09-24|10/2026|emp2
PEND|col616|2026-09-25|10/2026|emp2
PEND|col824|2026-09-05|09/2026|emp2
PEND|col824|2026-09-25|10/2026|emp2
PEND|col369|2026-09-25|10/2026|emp2
PEND|col885|2026-09-26|10/2026|emp2
PEND|col616|2026-09-27|10/2026|emp2
PEND|col242|2026-09-27|10/2026|emp2
PEND|col457|2026-09-27|10/2026|emp2
PEND|col155|2026-09-27|10/2026|emp3
PEND|col373|2026-09-27|10/2026|emp2
PEND|col666|2026-09-27|10/2026|emp2
PEND|col859|2026-09-02|09/2026|emp2
PEND|col859|2026-09-16|09/2026|emp2
PEND|col784|2026-09-28|10/2026|emp2
PEND|col857|2026-09-29|10/2026|emp4
PEND|col373|2026-09-29|10/2026|emp2
PEND|col89|2026-09-30|10/2026|emp3
PEND|col202|2026-10-02|10/2026|emp2
PEND|col876|2026-10-03|10/2026|emp3
PEND|col373|2026-10-06|10/2026|emp2
PEND|col131|2026-10-08|10/2026|emp3
PEND|col974|2026-10-10|10/2026|emp2
PEND|col155|2026-10-11|10/2026|emp3
PEND|col924|2026-10-10|10/2026|emp2
```

| Competência | Linhas PEND |
|---|---:|
| 07/2026 | 3 [nao verificado] |
| 08/2026 | 1 [nao verificado] |
| 09/2026 | 65 [nao verificado] |
| 10/2026 | 34 [nao verificado] |
| 11/2026 | 0 [nao verificado] |

[nao verificado]
```text
TOTAL_DIAS 253 TOTAL_PENDENTE 103 ERROS 0
FIM
```

## 16/11 14:34 · sessao 34889e7d5722 · PRIORIDADE-EV-ITEM6 · sonda · origem=Code

Referência: somente o bloco colado; sem arquivo:linha disponível, os resultados levam [nao verificado].

1. **Marcadores presentes:** `READ_ONLY on`, `ERROS 0` e `FIM`. Todas as linhas O199 têm `erro=0`; a saída passa pelo critério solicitado de integridade. [nao verificado]

2. **Conferência dos valores:** [nao verificado]

| competencia | empresa | exportada | dias | ja_folga | pendente | colabs_pendentes | referência |
|---|---:|:---:|---:|---:|---:|---:|---|
| 07/2026 | 3 | s | 3 | 0 | 3 | 2 | [nao verificado] |
| 08/2026 | 2 | s | 6 | 5 | 1 | 1 | [nao verificado] |
| 09/2026 | 2 | s | 91 | 39 | 52 | 29 | [nao verificado] |
| 09/2026 | 3 | s | 19 | 6 | 13 | 5 | [nao verificado] |
| 10/2026 | 2 | n | 74 | 47 | 27 | 20 | [nao verificado] |
| 10/2026 | 3 | n | 17 | 12 | 5 | 4 | [nao verificado] |
| 10/2026 | 4 | n | 3 | 1 | 2 | 1 | [nao verificado] |
| 11/2026 | 1 | n | 1 | 1 | 0 | 0 | [nao verificado] |
| 11/2026 | 2 | n | 29 | 29 | 0 | 0 | [nao verificado] |
| 11/2026 | 3 | n | 8 | 8 | 0 | 0 | [nao verificado] |
| 11/2026 | 4 | n | 2 | 2 | 0 | 0 | [nao verificado] |
| **TOTAL** | — | — | **253** | **150** | **103** | **62** | [nao verificado] |

**Os totais fecham:** soma de `dias=253`, soma de `pendente=103` e `150+103=253`. Em todas as linhas, `dias=ja_folga+pendente`. Os 62 são a soma de `colabs_pendentes` das linhas; quantidade de colaboradores únicos não está demonstrada. [nao verificado]

3. **Pendências e Corte C:** [nao verificado]

- **Exportadas (`s`): 69 dias pendentes**, destinados à lista do DP pelo critério solicitado. [nao verificado]
- **Abertas pelo flag (`n`): 34 dias pendentes**, todos em **10/2026**. [nao verificado]
- **11/2026: zero pendentes O199**; portanto, o recorte autorizado do Corte C apresenta **zero dias O199 para apply**, que continua condicionado ao `!` de escala. [nao verificado]

**Divergência:** o anexo chama 10 de competência paga, enquanto a saída marca 10 como `exportada=n`. Os 34 dias de outubro não podem ser considerados autorizados para apply pelo Corte C, limitado à 11. A saída também não apresenta medição identificável de `SEM-CELULA-11-DEVE-TER-CELULA (col305)`. [nao verificado]

## 16/11 14:33 · sessao fbd083b15a88 · ESPELHO-VERDADE-FECHO-V0 · leitura · origem=Code

A função é `espelho_do_colab`: a tela a chama diretamente e o PDF consome seus dias (`app/ponto/views.py:160-162`; `app/relatorios/pdf_espelho.py:596-603`). O patch abaixo intercepta somente as atribuições ao cache do catálogo, preservando sua leitura e os turnos reais (`app/ponto/catalogo/ausencias.py:275-281`).

```python
# OBRA: ESPELHO-VERDADE-FECHO-V0 — R3
# Arvore solicitada: 3580b35e028267d85318cb75fd5b3d0c40e04a00
# Texto para o envelope JA inicializado em SOMBRA/juliani.
# Execucao, desempenho e compatibilidade do script: [nao verificado].
# Auxiliares deste script, biblioteca padrao, sqlparse e implementacoes
# internas do Django fora da copia: [nao verificado].
#
# PARTE recorta estrutura_dias.txt:1-57 na ordem recebida: 19/19/19.
PARTE = 1

import sys
sys.dont_write_bytecode = True

import datetime as dt
import decimal
import json
import logging
import re
from contextlib import ExitStack, redirect_stderr, redirect_stdout
from unittest.mock import patch

NV = "[nao verificado]"
SAIDA = sys.stdout

# Mesma data civil do R1 recebido; instante/snapshot original: [nao verificado].
# O instante AGORA sera real, unico por execucao e publicado. [nao verificado]
HOJE = dt.date(2026, 11, 16)

# Transcricao dos campos identificadores e numeros de estrutura_dias.txt:1-57.
# Inclui col1021 e col909: o universo aqui e 57, nao os 55 do R2.
DADO = """col129|10/2026|2026-10-12|0|779
col218|10/2026|2026-10-15|105|0
col252|10/2026|2026-09-23|548|594
col252|10/2026|2026-09-24|552|598
col252|10/2026|2026-09-25|556|604
col252|10/2026|2026-09-28|535|581
col252|10/2026|2026-10-01|545|590
col252|10/2026|2026-10-02|545|593
col252|10/2026|2026-10-06|551|597
col252|10/2026|2026-10-09|544|592
col369|10/2026|2026-10-12|476|416
col37|10/2026|2026-10-05|241|0
col37|10/2026|2026-10-09|241|0
col382|10/2026|2026-09-30|364|0
col451|10/2026|2026-09-23|707|718
col451|10/2026|2026-09-25|705|718
col478|10/2026|2026-09-25|719|8
col515|10/2026|2026-10-09|244|0
col515|10/2026|2026-10-11|245|0
col723|10/2026|2026-10-08|440|345
col746|10/2026|2026-09-24|289|193
col841|10/2026|2026-10-20|681|522
col854|10/2026|2026-10-04|718|7
col864|10/2026|2026-10-12|509|449
col868|10/2026|2026-09-22|0|679
col876|10/2026|2026-09-21|550|665
col1021|11/2026|2026-11-06|841|0
col110|11/2026|2026-11-12|241|0
col112|11/2026|2026-10-21|546|838
col112|11/2026|2026-10-22|541|835
col113|11/2026|2026-10-30|242|0
col113|11/2026|2026-11-01|241|0
col114|11/2026|2026-11-08|439|429
col114|11/2026|2026-11-15|499|440
col130|11/2026|2026-10-29|335|672
col200|11/2026|2026-10-29|180|0
col200|11/2026|2026-11-02|180|0
col218|11/2026|2026-11-14|103|0
col220|11/2026|2026-11-10|337|430
col252|11/2026|2026-11-09|551|597
col252|11/2026|2026-11-10|551|598
col252|11/2026|2026-11-11|550|598
col252|11/2026|2026-11-13|551|597
col296|11/2026|2026-11-02|345|0
col37|11/2026|2026-11-14|241|0
col382|11/2026|2026-11-10|418|0
col516|11/2026|2026-10-23|361|0
col516|11/2026|2026-10-28|372|0
col727|11/2026|2026-11-10|307|0
col874|11/2026|2026-10-22|667|383
col884|11/2026|2026-11-08|679|699
col909|11/2026|2026-11-11|831|245
col922|11/2026|2026-11-01|1382|68
col922|11/2026|2026-11-04|676|458
col989|11/2026|2026-10-23|721|567
col996|11/2026|2026-10-28|0|580
col996|11/2026|2026-11-10|443|1026"""

TODOS = []
for numero, texto in enumerate(DADO.splitlines(), 1):
    codigo, competencia, data, esp_min, e6_min = texto.split("|")
    TODOS.append((numero, codigo, competencia, data, int(esp_min), int(e6_min)))

if PARTE not in (1, 2, 3):
    raise ValueError("ParteInvalida")
if len(TODOS) != 57 or len({r[1:4] for r in TODOS}) != 57:
    raise RuntimeError("UniversoIncompativel")
UNIVERSO = TODOS[(PARTE - 1) * 19:PARTE * 19]
N = len(UNIVERSO)
if N != 19:
    raise RuntimeError("FatiaIncompativel")


def imprimir(*partes):
    print(*partes, file=SAIDA, flush=True)


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


class escrita_nao_interceptada(RuntimeError):
    pass


class CacheSomenteLeitura:
    """Proxy de escrita; a leitura vem INTEGRALMENTE do objeto original."""

    def __init__(self, original):
        self.original = original
        self.suprimidas = 0

    def __getitem__(self, chave):
        return self.original[chave]

    def __setitem__(self, chave, valor):
        if chave not in ("v", "t"):
            raise RuntimeError("EscritorDeCacheIncompativel")
        self.suprimidas += 1
        # SOMENTE as atribuicoes sao absorvidas; nenhum valor e inventado.
        # Escritor nominal: ponto.catalogo.ausencias._CACHE.__setitem__
        # nos sitios app/ponto/catalogo/ausencias.py:279-280.
        # vigente() ainda calcula v pela fonte real (:278) e retorna v (:281).


def classe_segura(exc):
    nome = type(exc).__name__
    return nome if re.fullmatch(r"[A-Za-z_][A-Za-z0-9_]{0,79}", nome) else "Excecao"


def sonda():
    ok = erro = 0
    feitos = set()
    guarda = None

    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, connections, transaction
            from django.db.models import Q
            from django.utils import timezone
            import sqlparse
            from sqlparse import tokens as SQLTOK

            # Mesmo molde de ambiente do anexo r2_script_p1.py.
            # Configuracao: app/config/settings/sombra.py:49-50,65.
            # Envelope efetivo: verificado apenas quando este texto executar.
            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.schema_name != "juliani"
                or connection.in_atomic_block
                or not connection.get_autocommit()
            ):
                raise RuntimeError("AmbienteIncompativel")

            class GuardaSQL:
                def __init__(self):
                    self.tentou = False
                    self.permite_set_inicial = True

                def bloquear(self):
                    self.tentou = True
                    raise escrita_nao_interceptada()

                def __call__(self, execute, sql, params, many, context):
                    # execute_wrapper / sqlparse: implementacoes [nao verificado].
                    # O flag e aderente ao dia: except Exception internos nao
                    # podem converter uma tentativa de escrita em dia "ok".
                    # Exemplo de captura interna:
                    # app/ponto/catalogo/ausencias.py:246-254.
                    if context["connection"].alias != "default":
                        self.bloquear()

                    comandos = [
                        s for s in sqlparse.parse(str(sql))
                        if str(s).strip()
                    ]
                    if len(comandos) != 1:
                        self.bloquear()

                    partes = []
                    for token in comandos[0].flatten():
                        if token.is_whitespace:
                            continue
                        if token.ttype in SQLTOK.Comment:
                            continue
                        if token.ttype in SQLTOK.Literal.String:
                            continue
                        # Identificador entre aspas nao e comando.
                        if token.value.startswith('"'):
                            continue
                        partes.append(token.value)
                    plano = " ".join(partes).upper().strip().rstrip(";").strip()

                    # Inclui DML dentro de WITH e SELECT ... FOR UPDATE/SHARE.
                    # TRUNCATE/DDL/COPY/CALL tambem nao sao leitura autorizada.
                    if re.search(
                        r"\b(INSERT|UPDATE|DELETE|LOCK|MERGE|TRUNCATE|"
                        r"CREATE|ALTER|DROP|GRANT|REVOKE|COPY|CALL|DO|"
                        r"VACUUM|ANALYZE|REFRESH|REINDEX|CLUSTER|SHARE)\b",
                        plano,
                    ):
                        self.bloquear()
                    if re.search(r"\bPG_(?:TRY_)?ADVISORY_", plano):
                        self.bloquear()

                    if plano == "SET TRANSACTION READ ONLY":
                        if not self.permite_set_inicial:
                            self.bloquear()
                    elif not re.match(
                        r"^(SELECT|WITH|SHOW|SAVEPOINT|RELEASE|ROLLBACK)\b",
                        plano,
                    ):
                        self.bloquear()

                    return execute(sql, params, many, context)

            guarda = GuardaSQL()

            def serializar(valor):
                if isinstance(valor, dt.datetime):
                    # Mesmo uso local do E6:
                    # app/ponto/management/commands/e6_oraculo.py:152.
                    return timezone.localtime(valor).isoformat()
                if isinstance(valor, (dt.date, dt.time)):
                    return valor.isoformat()
                if isinstance(valor, decimal.Decimal):
                    return float(valor)
                # Nunca str(modelo), repr(modelo), nome ou texto de excecao.
                raise TypeError("TipoDeSaidaNaoAutorizado")

            def local(valor):
                return None if valor is None else timezone.localtime(valor).isoformat()

            def hora(valor):
                if valor is None or isinstance(valor, dt.time):
                    return valor
                if isinstance(valor, str) and re.fullmatch(
                    r"\d{2}:\d{2}(?::\d{2}(?:\.\d+)?)?", valor
                ):
                    return dt.time.fromisoformat(valor).isoformat()
                return NV

            def batida(b):
                if b is None:
                    return None
                # Campos: app/ponto/models.py:23-26,67.
                # Papeis em memoria: app/ponto/turnos.py:890-909.
                return {
                    "id": b.pk,
                    "hora_local": local(b.timestamp),
                    "tipo": b.tipo,
                    "is_intervalo": b.is_intervalo,
                    "retratada_em": local(b.retratada_em),
                    "papel_ata": b.__dict__.get("_papel_ata", NV),
                    "intra_ata": b.__dict__.get("_intra_ata", NV),
                    "intra_dur": b.__dict__.get("_intra_dur", NV),
                    "intra_declarado": b.__dict__.get("_intra_declarado", NV),
                    "eco_flush": b.__dict__.get("_eco_flush", NV),
                }

            def alertas_sem_nomes(alertas):
                # Publica categorias e numeros, nunca o texto livre.
                # Campo real: app/ponto/motor_calculo_v2.py:253.
                # Exemplos de produtores:
                # :1484-1486,2144-2147,2220-2231,2485-2490,
                # :2561-2563,2754,2779-2782,2814-2828,3188-3190.
                prefixos = (
                    ("DIA SEM JORNADA PREVISTA", "dia_sem_previsto"),
                    ("Entrada ", "horario_deslocado"),
                    ("Atraso:", "atraso"),
                    ("Saída antecipada:", "saida_antecipada"),
                    ("FOLGA TRABALHADA", "folga_trabalhada"),
                    ("Turno aberto", "turno_aberto"),
                    ("Período aberto", "periodo_aberto"),
                    ("Periodo aberto", "periodo_aberto"),
                    ("Intrajornada suprimida", "intra_suprimida"),
                    ("Feriado em", "feriado"),
                    ("HE semanal:", "he_semanal"),
                )
                saida = []
                for texto in alertas or ():
                    if not isinstance(texto, str):
                        raise TypeError("AlertaIncompativel")
                    codigo = next(
                        (cod for inicio, cod in prefixos if texto.startswith(inicio)),
                        "texto_livre_suprimido",
                    )
                    saida.append({
                        "codigo": codigo,
                        "numeros_do_texto": re.findall(
                            r"\d+(?:[.,:/]\d+)*", texto
                        ),
                    })
                return saida

            with ExitStack() as pilha:
                # Tripwire instalado antes dos imports de codigo da aplicacao.
                pilha.enter_context(connection.execute_wrapper(guarda))
                for alias in connections:
                    if alias != "default":
                        pilha.enter_context(connections[alias].execute_wrapper(guarda))

                with transaction.atomic():
                    with connection.cursor() as cursor:
                        cursor.execute("SET TRANSACTION READ ONLY")
                        guarda.permite_set_inicial = False
                        cursor.execute(
                            "SELECT current_database(), current_user, "
                            "current_schema(), "
                            "current_setting('transaction_read_only')"
                        )
                        if cursor.fetchone() != ("sombra", "leitor", "juliani", "on"):
                            raise RuntimeError("BancoPapelOuTenantIncompativel")

                    from ponto.catalogo import ausencias as catalogo
                    from ponto.services import espelho as esp
                    from ponto import turnos as geometria
                    from ponto.management.commands.e6_oraculo import (
                        minutos_do_espelho,
                        minutos_do_oraculo,
                    )
                    from ponto.janelas import janela_fechamento
                    from escala.alimentacao import (
                        q_vinculo_cobre_a_data,
                        vinculo_cobre_a_data,
                    )

                    # Modelos: app/colaboradores/models.py:283;
                    # app/escala/models.py:871,1635;
                    # app/ponto/models.py:115,660.
                    Colaborador = apps.get_model("colaboradores", "Colaborador")
                    CelulaDia = apps.get_model("escala", "CelulaDia")
                    EC = apps.get_model("escala", "EscalaColaborador")
                    Ausencia = apps.get_model("ponto", "Ausencia")
                    Justificativa = apps.get_model("ponto", "Justificativa")

                    # Falha explicitamente se os sitios usados pelo observador
                    # deixarem de corresponder ao codigo desta copia.
                    for funcao, linha in (
                        (esp.espelho_do_colab, 1194),
                        (esp._realizado_do_dia_tela, 267),
                        (geometria.realizado_do_dia, 774),
                        (geometria.realizado_dos_turnos, 707),
                        (minutos_do_oraculo, 42),
                    ):
                        if funcao.__code__.co_firstlineno != linha:
                            raise RuntimeError("SitioDaArvoreIncompativel")

                    # Patch NOMINAL apenas do destino das atribuicoes :279-280.
                    # _CACHE original continua sendo a fonte de todos os reads.
                    # Nao troca vigente(), _do_banco(), catalogo ou turnos.
                    cache = CacheSomenteLeitura(catalogo._CACHE)
                    pilha.enter_context(patch.object(catalogo, "_CACHE", cache))

                    # Os append de :1163,1906,1917 sao listas LOCAIS Python.
                    # Fontes: SELECT agregado e leitor de marcos; nao precisam
                    # GRANT e nao sao removidos:
                    # app/ponto/turnos.py:1154-1164,1901-1918.

                    AGORA = timezone.now()
                    if guarda.tentou:
                        raise escrita_nao_interceptada()

                    def ler_dia(registro):
                        numero, codigo, comp, texto_dia, esp_r1, e6_r1 = registro
                        cid = int(codigo[3:])
                        dia = dt.date.fromisoformat(texto_dia)
                        mes, ano = map(int, comp.split("/"))
                        chave = dia.isoformat()
                        linhas = []
                        inicio_cache = cache.suprimidas

                        def fato(nome, ref, **campos):
                            campos["ref"] = ref
                            linhas.append(
                                "DET|%s|%s|%s|%s" % (
                                    codigo, texto_dia, nome,
                                    json.dumps(
                                        campos, default=serializar,
                                        ensure_ascii=True, allow_nan=False,
                                        separators=(",", ":"),
                                    ),
                                )
                            )

                        c = Colaborador.objects.select_related("empresa").get(pk=cid)
                        # Funcao real: app/ponto/janelas.py:54-57.
                        ini, fim = janela_fechamento(mes, ano, c.empresa)
                        if not ini <= dia <= fim or dia > HOJE:
                            raise RuntimeError("DiaForaDaJanela")

                        captura = {
                            "raizes": [],
                            "corpos": [],
                            "descontos": [],
                            "oraculos": [],
                            "cortes_e6": [],
                            "pares_e6": [],
                        }
                        lendo_e6 = False

                        # Observacao de frames: nao altera argumentos, retornos,
                        # regras, pareamento ou objetos da aplicacao.
                        def observar_raiz(frame, evento, retorno):
                            if evento == "return" and retorno is not None:
                                L = frame.f_locals
                                captura["raizes"].append({
                                    "resultado": retorno,
                                    "ec": L.get("ec"),
                                    "celulas": L["celulas"],
                                    "turnos": tuple(
                                        t for t in L["turnos_juiz"]
                                        if t.data_turno == dia
                                    ),
                                })
                            return observar_raiz

                        def observar_corpo(frame, evento, retorno):
                            L = frame.f_locals
                            # Le pausas/bruto/fim JA decididos pelo corpo real,
                            # imediatamente antes de liquido = bruto - pausas:
                            # app/ponto/turnos.py:756-767.
                            if evento == "line" and frame.f_lineno == 767:
                                captura["descontos"].append({
                                    "modo": "turno",
                                    "entrada": L["t"].entrada.timestamp,
                                    "fim_contado": L["fim"].timestamp,
                                    "bruto_min": L["bruto"],
                                    "pausa_descontada_min": L["pausas"],
                                    "intervalos_reconhecidos": tuple(L["_itv_t"]),
                                })
                            if evento == "return" and retorno is not None:
                                captura["corpos"].append({
                                    "resultado": retorno,
                                    "livre": L["livre"],
                                    "intervalo"

[truncado em 20000 de 54344 caracteres]

## 16/11 14:31 · sessao b6bb56501dae · PRIORIDADE-EV-ITEM4 · sonda · origem=Code

Os números abaixo são os da saída fornecida; os ponteiros são os nela indicados. **Fatos persistidos compartilhados não demonstram quantidade de chamadas de cada endpoint.** (`app/ponto/portas/he.py:94-99`; `app/chamados/models.py:2325`)

- **TOTAL:** informado **55**, contado **55**: 26 `USO`, 19 `ERRO`, 8 `RECUSA`, 2 `EVIDENCIA`. Fecha na transcrição; correspondência com a execução integral: [nao verificado].
- **ERRO da própria sonda:** [nao verificado]. As 19 linhas `ERRO` apresentadas descrevem cobertura ou métricas: 17 de `5xx_traceback` desconhecidos e duas contagens restritas de zero; não comprovam falha de execução da sonda. (`app/config/settings/saas.py:173-181`; `app/ponto/services/he_pendente_lavrado.py:94-100,177-189`; `app/chamados/services/fio.py:713-714`)
- **rc:** [nao verificado]. Porta invalidada por erro da própria sonda: [nao verificado].

Na tabela, **L** significa: atribuição HTTP, 5xx/traceback e quantidade de recusas indevidas **[nao verificado]**. Os zeros persistidos abaixo têm cobertura restrita e não demonstram zero erros HTTP. (`app/config/settings/saas.py:173-181`)

| Obra | Porta | Uso n | Erro/recusa persistidos n | O que a sonda não prova |
|---|---|---:|---|---|
| O-JANELA-HE | `relatorios:index` — `app/relatorios/views.py:33-51` | [nao verificado] | [nao verificado] | L |
| O-JANELA-HE | `ponto:gestao_he` — `app/ponto/views.py:932-966` | [nao verificado] | **0**, somente apuração nas fotos atuais — `app/ponto/services/he_pendente_lavrado.py:94-100,177-189` | L; fotos sobrescritas não constituem histórico HTTP |
| O-JANELA-HE | `ponto:decidir_he` — `app/ponto/portas/he.py:94-99` | [nao verificado] | [nao verificado]; recusa do POST não persistida — `app/ponto/views.py:535-537,621-629` | L; separação do domínio compartilhado |
| O-JANELA-HE | `ponto:recusar_he_em_lote` — `app/ponto/portas/he.py:263-268` | [nao verificado] | [nao verificado]; recusa do POST não persistida — `app/ponto/views.py:535-537,621-629` | L; separação do domínio compartilhado |
| O-JANELA-HE | `ponto:gestao_he_pdf` — `app/ponto/views.py:870-878` | [nao verificado] | [nao verificado] | L; **16** evidências candidatas por assinatura não provam chamadas HTTP |
| BO-HE-RECUSAR-LOTE-500 | `ponto:gestao_he` — `app/ponto/views.py:932-966` | [nao verificado] | [nao verificado] | L |
| BO-HE-RECUSAR-LOTE-500 | `ponto:decidir_he` — `app/ponto/portas/he.py:94-99` | [nao verificado] | [nao verificado]; recusa do POST não persistida — `app/ponto/views.py:535-537,621-629` | L; separação do domínio compartilhado |
| BO-HE-RECUSAR-LOTE-500 | `ponto:recusar_he_em_lote` — `app/ponto/portas/he.py:263-268` | [nao verificado] | [nao verificado]; recusa do POST não persistida — `app/ponto/views.py:535-537,621-629` | L; separação do domínio compartilhado |
| REABRIR-NAO-DESTROI | `chamados:reabrir_disputa_supervisao` — `app/chamados/views.py:1435-1456` | [nao verificado] | [nao verificado] | L |
| REABRIR-NAO-DESTROI | `chamados:fio_colaborador` — `app/chamados/views.py:667-674` | [nao verificado] | [nao verificado] | L |
| REABRIR-NAO-DESTROI | `chamados:modal_fio` — `app/chamados/views.py:877-880` | [nao verificado] | [nao verificado] | L; **1.630** notas marcadas lidas não são exclusivas do modal — `app/chamados/services/nota.py:126-133` |
| REABRIR-NAO-DESTROI | `chamados:modal_fio@corrigir_resposta` — `app/chamados/services/acoes_disputa.py:375-383` | [nao verificado] | [nao verificado] | L |
| REABRIR-NAO-DESTROI | `chamados:modal_fio@regularizar_disputa` — `app/chamados/services/fio.py:713-714` | [nao verificado] | [nao verificado] por endpoint | L |
| REABRIR-NAO-DESTROI | `chamados:detalhe_local` — `app/chamados/views.py:469-475` | [nao verificado] | [nao verificado] | L |
| REABRIR-NAO-DESTROI | `chamados:solicitar_correcao_disputa` — `app/chamados/models.py:2318-2325` | [nao verificado] | [nao verificado] | L; separação HTML/API |
| REABRIR-NAO-DESTROI | `api:api_solicitar_correcao_disputa` — `app/api/views.py:2686` | [nao verificado] | [nao verificado] | L; separação HTML/API |
| REABRIR-NAO-DESTROI | `chamados:cobrar_chamado` — `app/chamados/views_cobrar.py:54-56` | [nao verificado] | [nao verificado] | L; atribuição exclusiva das notas |
| O-JANELA-HE | `dominio:decidir_he[compartilhada]` — `app/ponto/portas/he.py:94-99` | **72 fatos** | **10 decisões “não”**; erro/recusa indevida [nao verificado] | L; distribuição entre endpoints |
| BO-HE-RECUSAR-LOTE-500 | `dominio:decidir_he[compartilhada]` — `app/ponto/portas/he.py:94-99` | **71 fatos** | **10 decisões “não”**; erro/recusa indevida [nao verificado] | L; distribuição entre endpoints |
| REABRIR-NAO-DESTROI | `dominio:reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1631-1648` | **471 perguntas afetadas** | [nao verificado] | L; número de chamadas e atribuição por endpoint |
| REABRIR-NAO-DESTROI | `dominio:recusar_pergunta` — `app/chamados/services/validacao.py:628-629` | [nao verificado] | **9 recusas humanas de resposta**; caráter indevido [nao verificado] | L; essas recusas não demonstram falhas técnicas |
| REABRIR-NAO-DESTROI | `dominio:reabrir_disputa` — `app/chamados/views.py:1456`; `app/chamados/services/acoes_disputa.py:113` | **8 atos** | [nao verificado] | L; URL de origem |
| REABRIR-NAO-DESTROI | `dominio:corrigir_resposta_admin` — `app/chamados/services/acoes_disputa.py:375-383` | **42 atos** | [nao verificado] | L; URL de origem |
| REABRIR-NAO-DESTROI | `dominio:regularizar_disputa_granular` — `app/chamados/services/fio.py:713-714`; `app/chamados/services/acoes_disputa.py:144-145` | **7 atos** | **0 erros retornados de materialização**; classificação indevida [nao verificado] | L; URL de origem e erros fora dessa materialização |
| REABRIR-NAO-DESTROI | `dominio:recobrar_questionario` — `app/chamados/services/disputa_emissao.py:1755-1759` | **17 atos** | [nao verificado] | L; URL de origem |
| REABRIR-NAO-DESTROI | `dominio:cobranca` — `app/chamados/views_cobrar.py:54-56` | **13 notas** | [nao verificado] | L; distribuição entre endpoints |
| REABRIR-NAO-DESTROI | `dominio:solicitacao_correcao[HTML_API]` — `app/chamados/models.py:2325` | **38 notas** | [nao verificado] | L; separação HTML/API |
| REABRIR-NAO-DESTROI | `dominio:validar_pergunta` — `app/chamados/services/validacao.py:513-521` | [nao verificado] | **138 recusas em validações compartilhadas**; caráter indevido [nao verificado] | L; atribuição por endpoint |

**Smoke pelo ITEM 4: [nao verificado] nas três obras.** Há fatos persistidos positivos, mas “uso da porta > 0 e erros = 0”, incluindo 5xx/traceback e recusa indevida, não está demonstrado pelos dados apresentados. (`app/config/settings/saas.py:173-181`; referências e limites por porta na tabela)

## 16/11 14:31 · sessao 920e00533953 · PRIORIDADE-EV-ITEM6 · sonda · origem=Code

Referências `script.py:N` indicam linhas da **saída fornecida**, contando a primeira `sonda_leitura` como linha 1; no anexo, `aval Ronald` é a linha 1.

1. **O199:** 0 em 09/2026, 0 em 10/2026 e 0 em 11/2026. Nenhum caso individual aparece listado. (`script.py:4`, `script.py:5`, `script.py:6`)

2. **SC11 (col305):** o total informado para 11/2026 é **0**. Quantidades separadas de dias sem célula e com célula julgada errada: **[nao verificado]**, pois não há detalhamento desses grupos. A cobertura informa apenas `existentes_ate_hoje_sem_juizo=0`; cobertura integral de novembro e julgamento dos dias sem célula: **[nao verificado]**. (`script.py:7`, `script.py:8`)

3. **Erros:** não aparece linha `ERRO`; os totais zero não apresentam divergência visível com o detalhamento vazio; `rc_sombra=0`. Não há erro explícito na saída que invalide os números reportados. (`script.py:4`, `script.py:5`, `script.py:6`, `script.py:7`, `script.py:8`, `script.py:9`)

O destino por competência e a exigência de **! de escala** vêm do corte C. (`prioridade_espelho_verdade_tres_cortes_1611.txt:13`)

| obra | competencia | n | exportada | destino |
|---|---|---:|---|---|
| O199 | 09/2026 | 0 (`script.py:4`) | Sim, conforme enunciado | lista DP |
| O199 | 10/2026 | 0 (`script.py:5`) | Sim, conforme enunciado | lista DP |
| O199 | 11/2026 | 0 (`script.py:6`) | Não; aberta, conforme enunciado | apply na 11 com **! de escala** |
| SC11 (col305) | 11/2026 | 0 (`script.py:7`) | Não; aberta, conforme enunciado | apply na 11 com **! de escala** |

## 16/11 14:30 · sessao 114bf448442c · PRIORIDADE-EV-ITEM4 · leitura · origem=Code

**O dado fornecido não permite fechar nenhuma das três obras por “uso > 0 e erro 0”: as contagens posteriores aos pousos permanecem [nao verificado].** A cópia permite montar a sonda, mas a auditoria não guarda URL nem status HTTP, e vários endpoints compartilham o mesmo escritor. (`app/core/models.py:295-307`, `app/ponto/portas/he.py:263-268`, `app/chamados/services/acoes_disputa.py:113-114`)

**1. Portas e evidências**

Os prefixos são `/ponto/`, `/chamados/`, `/relatorios/` e `/api/`. (`app/config/urls.py:28-37`)

| Obra / porta | Rota, view | Uso persistido e escritor | Erro ou recusa persistidos |
|---|---|---|---|
| O-JANELA-HE — listagem | `ponto:gestao_he`; `/ponto/gestao-he/`; `gestao_he`. (`app/ponto/urls.py:17`, `app/ponto/views.py:887`) | A view lê e renderiza, sem gravar abertura. O retrato existente foi escrito pelo lavrador, não pelo acesso à tela. (`app/ponto/views.py:932-966`, `app/ponto/services/he_pendente_lavrado.py:177-189`) | `MetricaSnapshot.detalhe["erros"]`: falhas de apuração na **foto atual**, não erros HTTP nem histórico desde o pouso. Incremento em `he_pendente_lavrado.py:94-100`; gravação em `:177-189`; campos em `app/inteligencia/models.py:95-96`. |
| O-JANELA-HE — Autorizar/Não diário | `ponto:decidir_he`; `/ponto/gestao-he/decidir/`; `decidir_he`. (`app/ponto/urls.py:19`, `app/ponto/views.py:499`) | `DecisaoHE.estado`, `minutos_na_decisao`, `origem`, timestamps; criação/alteração em `_gravar`. Histórico: `LogAuditoria`, ação `decidir_he`, modelo `DecisaoHE`, `valor_antes/depois`, `criado_em`. (`app/ponto/models.py:1300-1317`, `app/ponto/portas/he.py:66-99`, `app/core/models.py:295-307`) | `estado="nao"` é ciência/decisão de negócio. A exceção `DecisaoHERecusada` apenas vira resposta da view; não há gravação desse retorno nesses caminhos. (`app/ponto/models.py:1283-1288`, `app/ponto/portas/he.py:175-193`, `app/ponto/views.py:535-537`) |
| O-JANELA-HE — Não em lote | `ponto:recusar_he_em_lote`; `/ponto/gestao-he/recusar-lote/`; `recusar_he_em_lote`. (`app/ponto/urls.py:21`, `app/ponto/views.py:549`) | Chama a mesma porta diária, produzindo a mesma trilha `decidir_he`, por dia alterado. **Não identifica o lote/URL.** (`app/ponto/portas/he.py:263-273`, `app/ponto/portas/he.py:94-99`) | Recusas ficam numa lista em memória e na resposta; itens malformados também só aparecem na resposta. (`app/ponto/portas/he.py:267-268`, `app/ponto/views.py:621-629`) |
| O-JANELA-HE — PDF | `ponto:gestao_he_pdf`; `/ponto/gestao-he/pdf/`; `gestao_he_pdf`. (`app/ponto/urls.py:27`, `app/ponto/views.py:772`) | `RegistroPDFAuditavel.tipo="outro"`, `filtros_json`, `gerado_em`; escritor em `pdf_base.py:152-166`. A chamada usa filtros `empresa/mes/ano/q/praca/posto/estado`, mas não persiste a rota. (`app/ponto/views.py:870-878`, `app/relatorios/models.py:36-55`) | O registro precede a segunda montagem do PDF; sua existência não prova conclusão nem entrega HTTP. (`app/relatorios/pdf_base.py:152-180`) |
| O-JANELA-HE — atalho na Central | `relatorios:index`; `/relatorios/`; `index`. (`app/relatorios/urls.py:9`, `app/relatorios/views.py:22`) | Lê contador e renderiza, sem trilha de abertura nessa view. (`app/relatorios/views.py:33-51`) | Falha do contador chama `logger.exception`; não grava erro de acesso no banco. (`app/relatorios/views.py:44-50`) |
| BO-HE-RECUSAR-LOTE-500 | As portas de decisão diária e recusa em lote acima; a listagem é a entrada da tela. (`app/docs/BACKLOG.md:1737`, `app/ponto/urls.py:17-21`) | As mesmas evidências anteriores, recortadas pelo pouso desta obra. A transação está na porta, antes do `select_for_update`. (`app/ponto/portas/he.py:151-159`, `app/ponto/portas/he.py:195`) | O caminho não registra 5xx no banco; o tratamento das recusas permanece em retorno/UI. (`app/ponto/views.py:535-537`, `app/ponto/views.py:621-629`, `app/config/settings/saas.py:173-181`) |

O pouso da O-JANELA nomeia as quatro rotas da Gestão de HE; os “13 passos” são referidos nesse registro. A inclusão exata de outras telas nos 13 passos é [nao verificado]. (`app/docs/RELATO-ARQUIVO.md:28278-28285`)

Na REABRIR, **o modelo atual é `DecisaoPergunta`**: a migration renomeou `RespostaAnterior`, preservou o instante como `ato_em` e atribuiu `ato="reabertura"` às linhas existentes. (`app/chamados/migrations/0051_decisao_pergunta.py:41-61`)

| Porta / fluxo REABRIR-NAO-DESTROI | Rota, view | Uso persistido e escritor | Erro ou recusa |
|---|---|---|---|
| Reabrir pela supervisão | `chamados:reabrir_disputa_supervisao`; `/chamados/local/<id>/reabrir-disputa/`; `reabrir_disputa_supervisao`. (`app/chamados/urls.py:20`, `app/chamados/views.py:1372`) | `DecisaoPergunta(ato="reabertura", ato_em)`; preparação em `disputa_emissao.py:1631-1646`, INSERT em `models.py:4120`. Auditoria `reabrir_disputa/DisputaSupervisao` em `views.py:1456`. | Sem permissão/sem disputa/sem pergunta corrigível viram respostas; não são gravados como recusas nesses ramos. (`app/chamados/views.py:1403-1434`) |
| Reabrir pelo fio/modal | `chamados:fio_colaborador`, `/chamados/colaborador/<id>/`, `fio_colaborador`; `chamados:modal_fio`, `/chamados/modal/<id>/`, `modal_fio_colaborador`. (`app/chamados/urls.py:21,39`, `app/chamados/views.py:659,869`) | POST delegado ao fio; mesma reabertura e mesma auditoria, sem distinguir a URL. (`app/chamados/views.py:667-674,951-954`, `app/chamados/services/fio.py:679-682`, `app/chamados/services/acoes_disputa.py:102-114`) | As recusas anteriores ao ato só retornam mensagem. (`app/chamados/services/acoes_disputa.py:95-101`) |
| Pedir correção — HTML/API | `chamados:solicitar_correcao_disputa`, `/chamados/disputa/<pk>/solicitar-correcao/`; `api:api_solicitar_correcao_disputa`, `/api/disputa/<pk>/solicitar-correcao/`. Views em `app/chamados/views.py:1317` e `app/api/views.py:2670`; rotas em `app/chamados/urls.py:19`, `app/api/urls.py:94`. | Ambas usam `reabrir_por_colab`; grava snapshot, contador e nota `[Solicitacao de correcao - `. HTML/API ficam indistinguíveis nessa evidência. (`app/chamados/views.py:1334`, `app/api/views.py:2686`, `app/chamados/models.py:2318-2325`) | Fechada/limite/sem resposta retornam estado, sem evento de recusa nesse método. (`app/chamados/models.py:2304-2312`) |
| Corrigir administrativamente | POST do modal com `acao=corrigir_resposta`; também fio e detalhe local. (`app/templates/chamados/partials/modal_fio.html:301-312`, `app/chamados/services/fio.py:663-677`, `app/chamados/views.py:525-536`) | Auditoria `corrigir_resposta_admin/PerguntaDisputa`, após gravar a resposta, **antes da validação/materialização**. (`app/chamados/services/acoes_disputa.py:370-383`) | `LogAuditoria(acao="val_perg_nao_gravou", modelo="PerguntaDisputa")` registra recusa de materialização, compartilhada com outras validações. (`app/chamados/services/validacao.py:513-521`) |
| Cobrar resposta | `chamados:cobrar_chamado`; `/chamados/cobrar/<cid>/`; `cobrar_chamado`. (`app/chamados/urls.py:25`, `app/chamados/views_cobrar.py:24`) | Nota `FollowupChamado.conteudo`, `criado_em`, prefixo `[COBRANCA]`; chamada em `views_cobrar.py:54-56`, INSERT em `services/nota.py:103,107`; campos em `models.py:1732-1738`. | Permissão, área, limite diário e falha de push retornam antes da nota. (`app/chamados/views_cobrar.py:25-53`) |
| Recobrar questionário pendente | Mesma rota `reabrir_disputa_supervisao`, ramo pendente. (`app/chamados/views.py:1421-1425`) | Auditoria `recobrar_questionario/DisputaSupervisao` e nota `[Re-cobranca do questionario - `. (`app/chamados/services/disputa_emissao.py:1741-1759`) | Falhas de nota/push usam `engolir`, que registra logging e contador de processo. (`app/chamados/services/disputa_emissao.py:1741-1754`, `app/core/observ.py:73-91`) |
| Encerrar/regularizar | POST do modal com `acao=regularizar_disputa`; também fio e `chamados:detalhe_local`, `/chamados/local/<id>/`, `detalhe_chamado_local`. (`app/templates/chamados/partials/modal_fio.html:353-359`, `app/chamados/urls.py:14`, `app/chamados/views.py:359,469-475`) | Auditoria `regularizar_disputa_granular/DisputaSupervisao`. (`app/chamados/services/fio.py:713-714`, `app/chamados/services/acoes_disputa.py:144-145`) | A descrição guarda `Mat=N Pend=N Fechada=B Erros=N`; guardas anteriores ao ato só retornam mensagem. (`app/chamados/services/fio.py:685-714`, `app/chamados/services/acoes_disputa.py:123-145`) |
| Abrir o histórico no modal | GET `chamados:modal_fio`, acima. (`app/chamados/urls.py:39`) | Carimba `FollowupChamado.lido_dp_em`, mas somente notas ainda não lidas; não é contador de aberturas nem evidência exclusiva desta tela. (`app/chamados/views.py:877-880`, `app/chamados/services/nota.py:126-133`, `app/chamados/models.py:1736`) | A presença do histórico não prova sua renderização; o template chama o leitor de respostas anteriores. (`app/templates/chamados/partials/modal_fio.html:204-205`, `app/chamados/models.py:3667-3683`) |

Uma reabertura produz linhas **por pergunta afetada**, não por clique; pendente limpa não produz versão. Decisão HE repetida também retorna sem trilha nova. (`app/chamados/services/disputa_emissao.py:1631-1648`, `app/ponto/portas/he.py:197-204`)

**2. Texto da sonda**

O script mantém `[nao verificado]` para contagens HTTP sem atribuição; imprime contagens separadas das portas de domínio compartilhadas. Para os horários incompletos, calcula limites: se houver registros dentro do minuto/faixa incerta, informa `n_min/n_max`, sem inventar o instante do pouso. Os horários publicados são os do backlog. (`app/docs/BACKLOG.md:1208`, `app/docs/BACKLOG.md:1737`, `app/docs/BACKLOG.md:1168`)

O envelope deve estar previamente inicializado; banco PROD `saas_hasner`, role `leitor` e tenant `juliani` correspondem ao envelope da cópia. **O wrapper não deve ser executado nesta tarefa:** ele próprio contém criação de arquivos e administração da role. (`bin/sonda_leitura.sh:28-29,42-46,216-218,239-240,248-250`)

```python
# OBRA: PRIORIDADE-EV-ITEM4
# Somente TEXTO para envelope PROD previamente inicializado; nao executado.
# Execucao, desempenho, Django/PostgreSQL e compatibilidade em PROD:
# [nao verificado].
# Nao chama views, servicos, juizes, lavradores nem django.setup().
# Biblioteca padrao e implementacoes externas: [nao verificado].
#
# Ambiente esperado: bin/sonda_leitura.sh:42-46,237-240.
# Datas publicadas: app/docs/BACKLOG.md:1208,1737,1168.
# "x" nao e convertido em minuto exato: contam-se limites inferior/superior.
# Offset -03:00 aplicado ao horario local informado: [nao verificado]. [nao verificado]
# TOTAL conta todas as linhas emitidas ANTES da propria linha TOTAL.
# Falha de sonda nunca e rotulada como erro ocorrido no produto.

import sys
sys.dont_write_bytecode = True

import datetime as dt
import logging
import re
from contextlib import redirect_stdout, redirect_stderr
from unittest.mock import patch

NV = "[nao verificado]"
SAIDA = sys.stdout
LINHAS = []

JANELA = "O-JANELA-HE"
BO = "BO-HE-RECUSAR-LOTE-500"
REABRIR = "REABRIR-NAO-DESTROI"

# Rotulo do pouso, menor instante possivel, primeiro instante certamente posterior. [nao verificado]
# Segundos exatos dos pousos: [nao verificado].
CORTES = {
    JANELA: (
        "2026-10-01T09:2x-03:00",
        "2026-10-01T09:20:00-03:00",
        "2026-10-01T09:30:00-03:00",
    ),
    BO: (
        "2026-10-22T10:26-03:00",
        "2026-10-22T10:26:00-03:00",
        "2026-10-22T10:27:00-03:00",
    ),
    REABRIR: (
        "2026-10-25T07:4x-03:00",
        "2026-10-25T07:40:00-03:00",
        "2026-10-25T07:50:00-03:00",
    ),
}

# Endpoints HTTP: nao converter trilha compartilhada em acesso atribuido.
# Rotas/views:
# app/ponto/urls.py:17-27; app/ponto/views.py:499,549,772,887.
# app/relatorios/urls.py:9; app/relatorios/views.py:22-51.
# app/chamados/urls.py:14,19-21,25,39.
# app/api/urls.py:94; app/api/views.py:2670.
PORTAS = {
    JANELA: (
        ("relatorios:index", "app/relatorios/views.py:33-51"),
        ("ponto:gestao_he", "app/ponto/views.py:932-966"),
        ("ponto:decidir_he", "app/ponto/portas/he.py:94-99"),
        ("ponto:recusar_he_em_lote", "app/ponto/portas/he.py:263-268"),
        ("ponto:gestao_he_pdf", "app/ponto/views.py:870-878"),
    ),
    BO: (
        ("ponto:gestao_he", "app/ponto/views.py:932-966"),
        ("ponto:decidir_he", "app/ponto/portas/he.py:94-99"),
        ("ponto:recusar_he_em_lote", "app/ponto/portas/he.py:263-268"),
    ),
    REABRIR: (
        ("chamados:reabrir_disputa_supervisao",
         "app/chamados/views.py:1435-1456"),
        ("chamados:fio_colaborador",
         "app/chamados/views.py:667-674"),
        ("chamados:modal_fio",
         "app/chamados/views.py:877-880"),
        ("chamados:modal_fio@corrigir_resposta",
         "app/chamados/services/acoes_disputa.py:375-383"),
        ("chamados:modal_fio@regularizar_disputa",
         "app/chamados/services/fio.py:713-714"),
        ("chamados:detalhe_local",
         "app/chamados/views.py:469-475"),
        ("chamados:solicitar_correcao_disputa",
         "app/chamados/models.py:2318-2325"),
        ("api:api_solicitar_correcao_disputa",
         "app/api/views.py:2686"),
        ("chamados:cobrar_chamado",
         "app/chamados/views_cobrar.py:54-56"),
    ),
}


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


def classe_segura(exc):
    nome = type(exc).__name__
    return nome if re.fullmatch(r"[A-Za-z_][A-Za-z0-9_]{0,79}", nome) else "Excecao"


def adicionar(*campos):
    LINHAS.append("|".join(str(c) for c in campos))


def emitir_medida(tipo, obra, porta, valor, extras=(), subtipo=None):
    campos = [tipo, obra, porta]
    if subtipo is not None:
        campos.append(subtipo)
    if tipo == "USO":
        campos.extend(("desde=" + CORTES[obra][0], "n=" + str(valor)))
    elif tipo in ("ERRO", "RECUSA"):
        campos.append(str(valor))
    else:
        campos.append("n=" + str(valor))
    campos.extend(extras)
    adicionar(*campos)


def principais():
    # Ausencia de campo URL/status: app/core/models.py:295-307.
    for obra, portas in PORTAS.items():
        for porta, ref in portas:
            emitir_medida(
                "USO", obra, porta, NV,
                ("atribuicao_http=nao_disponivel", "ref=" + ref),
            )
            emitir_medida(
                "ERRO", obra, porta, NV,
                ("cobertura=banco_nao_conta_http",
                 "ref=app/config/settings/saas.py:173-181"),
                subtipo="5xx_traceback",
            )

    # Recusas do POST, distintas de decisao de negocio "Nao".
    # app/ponto/views.py:535-537,621-629.
    for obra in (JANELA, BO):
        for porta in ("ponto:decidir_he", "ponto:recusar_he_em_lote"):
            emitir_medida(
                "RECUSA", obra, porta, NV,
                ("criterio=nao_ha", "escopo=recusa_do_post_nao_persistida",
                 "ref=app/ponto/views.py:535-537,621-629"),
            )


def sonda():
    principais()
    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, transaction

            # Configuracao tenant-aware: app/config/settings/saas.py:83-95.
            # Identidade real do envelope e GRANTs em PROD: [nao verificado].
            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.saas"
                or getattr(settings, "BANCO_DE_ENSAIO", False)
                or connection.settings_dict["NAME"] != "saas_hasner"
                or connection.schema_name != "juliani"
                or connection.in_atomic_block
                or not connection.get_autocommit()
            ):
                raise RuntimeError("AmbienteIncompativel")

            # Modelos/campos efetivamente consultados:
            # LogAuditoria: acao, modelo, valor_depois, descricao, criado_em
            # app/core/models.py:295-307.
            # Escritor efetivo: app/core/observ.py:133-138.
            Log = apps.get_model("core", "LogAuditoria")

            # DecisaoPergunta: ato, ato_em.
            # app/chamados/models.py:4039-4040.
            # Rename de RespostaAnterior:
            # app/chamados/migrations/0051_decisao_pergunta.py:41-61.
            # Escritor: app/chamados/models.py:4110-4122.
            Decisao = apps.get_model("chamados", "DecisaoPergunta")

            # FollowupChamado: conteudo, criado_em, lido_dp_em.
            # app/chamados/models.py:1735-1738.
            # Escritores: app/chamados/services/nota.py:103,107,126-133.
            Nota = apps.get_model("chamados", "FollowupChamado")

            # MetricaSnapshot: chave, escopo, detalhe, calculado_em.
            # app/inteligencia/models.py:87-96.
            # Escritor: app/ponto/services/he_pendente_lavrado.py:177-189.
            Snapshot = apps.get_model("inteligencia", "MetricaSnapshot")

            # RegistroPDFAuditavel: tipo, filtros_json, gerado_em.
            # app/relatorios/models.py:36-55.
            # Escritor: app/relatorios/pdf_base.py:152-166.
            PDF = apps.get_model("relatorios", "RegistroPDFAuditavel")

            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                    )
                    cursor.execute("SET LOCAL statement_timeout = '30s'")
                    cursor.execute(
                        "SELECT current_database(), current_user, session_user, "
                        "current_schema(), "
                        "current_setting('transaction_read_only'), "
                        "CURRENT_TIMESTAMP"
                    )
                    banco, papel, sessao, schema, leitura, fim = cursor.fetchone()
                    if (banco, papel, sessao, schema, leitura) != (
                        "saas_hasner", "leitor", "leitor", "juliani", "on"
                    ):
                        raise RuntimeError("BancoPapelOuTenantIncompativel")

                def medir(tipo, obra, porta, qs, campo, ref, *,
                          subtipo=None, extras=(), reduzir=None):
                    # Conta somente fatos anteriores ao inicio desta transacao.
                    # Duas leituras na mesma fotografia transacional.
                    try:
                        with transaction.atomic():
                            _, texto_lo, texto_hi = CORTES[obra]
                            lo = dt.datetime.fromisoformat(texto_lo)
                            hi = dt.datetime.fromisoformat(texto_hi)
                            base = qs.filter(**{campo + "__lt": fim})
                            desde_lo = base.filter(**{campo + "__gte": lo})
                            desde_hi = base.filter(**{campo + "__gte": hi})
                            maior = (reduzir(desde_lo) if reduzir
                                     else desde_lo.count())
                            menor = (reduzir(desde_hi) if reduzir
                                     else desde_hi.count())
                            if menor < 0 or maior < menor:
                                raise RuntimeError("ContagemIncompativel")
                        valor = menor if menor == maior else NV
                        emitir_medida(
                            tipo, obra, porta, valor,
                            (
                                "desde=" + CORTES[obra][0]
                                if tipo != "USO" else "unidade=fatos_persistidos",
                                "n_min=" + str(menor),
                                "n_max=" + str(maior),
                                "n_janela_incerta=" + str(maior - menor),
                                "ref=" + ref,
                            ) + tuple(extras),
               

[truncado em 20000 de 31626 caracteres]

## 16/11 14:28 · sessao 8531c1e56279 · PRIORIDADE-EV-ITEM6 · leitura · origem=Code

1. **O199:** o juiz da pendência é `o_dia_ja_e_folga`, usado pelo próprio aceite; ele delega a `folga_lancada_do_dia`, registrado em JUIZES. Universo: respostas `FOLGA_CONTESTA` aceitas (`validada_em` preenchido), deduplicadas por colaborador e `data_evento`. (`app/ponto/services/declarar_dia.py:78`, `app/chamados/services/validacao.py:305`, `app/docs/JUIZES.md:127`, `app/chamados/services/validacao.py:259`, `app/chamados/services/validacao.py:159`, `app/chamados/services/validacao.py:293`.)

2. **SC11:** a obrigação de existir célula vem de `vinculo_cobre_a_data`; `vinculo_do_dia` identifica seu dono. Universo: dias de col305 na competência empresarial 11/2026 cobertos por vínculo, sem filtro por situação. (`app/docs/JUIZES.md:135`, `app/docs/JUIZES.md:137`, `app/escala/alimentacao.py:205`, `app/escala/alimentacao.py:228`, `app/ponto/portas/celula.py:870`.)

**Limite do pedido:** juiz geral de “célula errada” para toda a competência: `[nao verificado]`. O medidor existente julga futuras; comparar o template atual com células passadas não certifica erro. Por isso o script distingue casos confirmados de cobertura incompleta e não publica total integral falso. (`app/ponto/services/vigia_cadastro_futuro.py:24`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/ponto/management/commands/tripwire_celulas.py:4`.)

O estado abaixo é o **gravado na célula**; status atual pelo juiz e possibilidade de declarar ficam `[nao verificado]`: seus caminhos alcançam carregadores/cache vedados pelo molde. Exportada significa `competencia_entregue`, sem confundir com fechamento por holerite. (`app/escala/models.py:1816`, `app/ponto/status_dia.py:270`, `app/ponto/services/declarar_dia.py:120`, `app/ponto/catalogo/ausencias.py:279`, `app/folha/export.py:556`, `app/folha/export.py:597`.)

```python
# OBRA: PRIORIDADE-EV-ITEM6
# Copia solicitada: a5765647153708b3fdd1c5dae4e6003bbc8fb4bf.
# Texto para envelope JA inicializado em SOMBRA / juliani / leitor.
# Nao executado; compatibilidade, desempenho e resultados: [nao verificado].
# Auxiliares deste texto, biblioteca padrao e internos do Django:
# [nao verificado].
#
# O199: inventario por competencia; 09/10 somente lista do DP.
# Elegibilidade para eventual apply restrita a 11, conforme anexo ITEM 6.
# Nenhuma porta de escrita e chamada.
#
# SC11:
# - ausentes: obrigacao de existencia pelo juiz de cobertura;
# - existentes futuras: medidor real, chamado numa janela de um dia;
# - existentes ate hoje: juizo geral de erro [nao verificado].
# TOTAL_PARCIAL conta linhas confirmadas.
# TOTAL recebe [nao verificado] quando a cobertura nao permite total integral.
#
# NAO chamar:
# checar_declaracao -> veto_de_lancamento -> ausencias_sobrepostas
# -> TIPOS_EM_ABERTO -> vigente, que escreve cache.
# app/ponto/services/declarar_dia.py:120,147
# app/ponto/precedencia.py:461
# app/ponto/turnos.py:2843
# app/ponto/catalogo/ausencias.py:267,279-280,406,430
#
# NAO chamar status_do_dia_lavrado com carga/insumos fabricados.
# app/ponto/status_dia.py:201,221,270,273,298
#
# NAO chamar lavrar(medida).
# app/ponto/services/vigia_cadastro_futuro.py:99-108

import sys
sys.dont_write_bytecode = True

import datetime as dt
import json
import logging
from collections import Counter
from contextlib import redirect_stderr, redirect_stdout
from unittest.mock import patch

NV = "[nao verificado]"
SAIDA = sys.stdout
ALVO = (2026, 11)


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


def emitir(*campos):
    print("|".join(str(c) for c in campos), file=SAIDA, flush=True)


def compacto(valor):
    return json.dumps(
        valor, ensure_ascii=True, allow_nan=False, separators=(",", ":")
    )


def consumir(qs):
    it = qs.iterator(chunk_size=128)
    try:
        return list(it)
    finally:
        it.close()


def competencia(ano_mes):
    ano, mes = ano_mes
    return "%02d/%04d" % (mes, ano)


def sonda():
    linhas = []
    totais_o199 = Counter({(2026, 9): 0, (2026, 10): 0, ALVO: 0})
    total_sc11 = 0
    sem_data_o199 = 0
    existentes_sem_juizo = 0

    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, transaction
            from django.utils import timezone

            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.settings_dict["HOST"] != "sombra_db"
                or connection.schema_name != "juliani"
            ):
                raise RuntimeError("AmbienteIncompativel")

            # Configuracao: app/config/settings/sombra.py:47-50,65.
            # Inicializacao efetiva do envelope: [nao verificado].

            # Predicado exato do aceite:
            # app/chamados/services/validacao.py:259-261.
            from chamados.services.validacao import _e_aceite_de_folga

            # Pergunta usada pela porta real de aceite:
            # app/ponto/services/declarar_dia.py:78-95.
            # Chamador real: app/chamados/services/validacao.py:305.
            from ponto.services.declarar_dia import o_dia_ja_e_folga

            # Juiz de competencia exportada:
            # app/folha/export.py:556-594.
            from folha.export import competencia_entregue

            # Competencia empresarial, sem corte 21 duplicado:
            # app/ponto/janelas.py:54-63.
            from ponto.janelas import janela_atual, janela_fechamento

            # Carregadores reais:
            # app/escala/alimentacao.py:54-78,163-186.
            # Juizes:
            # app/escala/alimentacao.py:205-216,228-286.
            from escala.alimentacao import (
                celulas_do_periodo,
                escalas_do_periodo,
                vinculo_cobre_a_data,
                vinculo_do_dia,
            )

            # Leitor real; nao importar/chamar lavrar:
            # app/ponto/services/vigia_cadastro_futuro.py:24-87.
            from ponto.services.vigia_cadastro_futuro import medir

            # Modelos:
            # app/chamados/models.py:2511,2528,2552-2554.
            # app/colaboradores/models.py:283.
            # app/escala/models.py:1635,1681-1695,1813-1816.
            Pergunta = apps.get_model("chamados", "PerguntaDisputa")
            Colaborador = apps.get_model("colaboradores", "Colaborador")
            CelulaDia = apps.get_model("escala", "CelulaDia")

            status_permitidos = {k for k, _ in CelulaDia.STATUS_DIA_CHOICES}

            def estado_gravado(cel):
                # Colunas reais; nenhuma chamada ao juiz de status.
                # app/escala/models.py:1686-1688,1816.
                return {
                    "folga_decidida_pela_porta": False,
                    "celula_existe": cel is not None,
                    "trabalha_gravado": None if cel is None else cel.trabalha,
                    "status_gravado": (
                        None if cel is None else
                        cel.status_dia if (
                            cel.status_dia is None
                            or cel.status_dia in status_permitidos
                        ) else NV
                    ),
                    "status_atual_pelo_juiz": NV,
                    "pode_declarar_agora": NV,
                }

            def retrato_vinculo(ec):
                # Apenas identificador e vigencia; nunca __str__.
                # Campos consumidos pelo juiz:
                # app/escala/alimentacao.py:214-216.
                return {
                    "id": ec.pk,
                    "ativa": ec.ativa,
                    "inicio": (
                        ec.data_inicio.isoformat()
                        if ec.data_inicio is not None else None
                    ),
                    "fim": (
                        ec.data_fim.isoformat()
                        if ec.data_fim is not None else None
                    ),
                }

            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, "
                        "READ ONLY"
                    )
                    cursor.execute(
                        "SELECT current_database(), current_user, "
                        "current_schema(), "
                        "current_setting('transaction_read_only')"
                    )
                    if cursor.fetchone() != (
                        "sombra", "leitor", "juliani", "on"
                    ):
                        raise RuntimeError("BancoPapelOuTenantIncompativel")

                # Relogio Django/envelope: [nao verificado].
                hoje = timezone.localdate()

                # O aceite real usa disputa.colaborador e data_evento.
                # app/chamados/services/validacao.py:293-294.
                # Carimbo da aceitacao:
                # app/chamados/services/validacao.py:152-166.
                perguntas = consumir(
                    Pergunta.objects.filter(validada_em__isnull=False)
                    .select_related("disputa__colaborador__empresa")
                    .order_by("pk")
                )
                feitos = set()

                for perg in perguntas:
                    # app/chamados/services/validacao.py:259-261.
                    if not _e_aceite_de_folga(perg):
                        continue

                    c = perg.disputa.colaborador
                    dia = perg.data_evento
                    if dia is None:
                        sem_data_o199 += 1
                        linhas.append((
                            "PENDENCIA", "O199", "col%d" % c.pk,
                            NV, "aceite_sem_data_evento",
                        ))
                        continue

                    chave = (c.pk, dia)
                    if chave in feitos:
                        continue
                    feitos.add(chave)

                    if c.empresa_id is None:
                        raise RuntimeError("EmpresaAusente")

                    # app/ponto/janelas.py:60-63.
                    ini, fim = janela_atual(dia, c.empresa)
                    par = (fim.year, fim.month)
                    if not ini <= dia <= fim:
                        raise RuntimeError("CompetenciaIncompativel")
                    totais_o199.setdefault(par, 0)

                    # Juiz real de pendencia do backfill:
                    # app/ponto/services/declarar_dia.py:78-95.
                    if o_dia_ja_e_folga(c, dia):
                        continue

                    # app/folha/export.py:556-594.
                    exportada = competencia_entregue(
                        c.empresa_id, fim.month, fim.year
                    )

                    # Leitura da coluna; nao julgamento do status.
                    # app/escala/models.py:1681-1688,1816.
                    cs = consumir(CelulaDia.objects.filter(
                        colaborador_id=c.pk, data=dia
                    ))
                    if len(cs) > 1:
                        raise RuntimeError("CelulaDuplicada")
                    cel = cs[0] if cs else None

                    linhas.append((
                        "O199", "col%d" % c.pk, dia.isoformat(),
                        competencia(par), "s" if exportada else "n",
                        compacto(estado_gravado(cel)),
                    ))
                    totais_o199[par] += 1

                # SC11: exclusivamente col305 na competencia empresarial 11.
                # Modelo: app/colaboradores/models.py:283.
                c305 = Colaborador.objects.select_related("empresa").get(pk=305)
                if c305.empresa_id is None:
                    raise RuntimeError("EmpresaAusente")

                # app/ponto/janelas.py:54-63.
                ini, fim = janela_fechamento(11, 2026, c305.empresa)
                if janela_atual(hoje, c305.empresa) != (ini, fim):
                    raise RuntimeError("OnzeNaoECompetenciaCorrente")

                # app/escala/alimentacao.py:54-78.
                celulas = celulas_do_periodo([305], ini, fim)

                # Incluir geradoras nomeadas fora da candidatura da janela.
                # app/escala/alimentacao.py:163-186.
                escalas = escalas_do_periodo(
                    305, ini, fim,
                    mais={
                        cel.escala_geradora_id
                        for cel in celulas.values()
                        if cel.escala_geradora_id is not None
                    },
                )

                # app/folha/export.py:556-594.
                exp305 = competencia_entregue(c305.empresa_id, 11, 2026)

                dia = ini
                while dia <= fim:
                    cel = celulas.get((305, dia))

                    # Cobertura nao se presume pelo flag ativa.
                    # app/escala/alimentacao.py:205-216.
                    cobrem = [
                        ec for ec in escalas
                        if vinculo_cobre_a_data(ec, dia)
                    ]

                    # Nao escolher outro vinculo por fora da autoridade.
                    # app/escala/alimentacao.py:228-286.
                    dono = vinculo_do_dia(
                        305, dia, escalas, celulas=celulas
                    )

                    motivos = []
                    if cel is None:
                        if cobrem:
                            motivos.append("sem_celula_com_vinculo")
                    elif dia > hoje:
                        # Janela de UM dia: obtem casos usando o medidor
                        # existente, sem copiar seu predicado.
                        # app/ponto/services/vigia_cadastro_futuro.py:24-87.
                        medida = medir(
                            dia, dia, hoje=hoje, colaborador_id=305
                        )
                        motivos = sorted(
                            k
                            for grupo in ("contadores", "selos")
                            for k, n in medida[grupo].items()
                            if n > 0
                        )
                    else:
                        # Medidor nao possui jurisdicao neste ramo.
                        # app/ponto/services/vigia_cadastro_futuro.py:31.
                        existentes_sem_juizo += 1

                    if motivos:
                        linhas.append((
                            "SC11", "col305", dia.isoformat(), "11/2026",
                            "s" if exp305 else "n",
                            compacto({
                                "existe": cel is not None,
                                "trabalha": (
                                    None if cel is None else cel.trabalha
                                ),
                            }),
                            compacto({
                                "do_dia": (
                                    None if dono is None else dono.pk
                                ),
                                "cobrem": [
                                    retrato_vinculo(ec)
                                    for ec in sorted(cobrem, key=lambda e: e.pk)
                                ],
                                "motivos": motivos,
                            }),
                        ))
                        total_sc11 += 1

                    dia += dt.timedelta(days=1)

        # Publicar somente depois de a leitura inteira terminar.
        for linha in linhas:
            emitir(*linha)

        for par, n in sorted(totais_o199.items()):
            emitir(
                "TOTAL", "O199", competencia(par),
                NV if sem_data_o199 else n,
            )
            if sem_data_o199:
                emitir("TOTAL_PARCIAL", "O199", competencia(par), n)

        emitir(
            "TOTAL", "SC11", "11/2026",
            NV if existentes_sem_juizo else total_sc11,
        )
        if existentes_sem_juizo:
            emitir("TOTAL_PARCIAL", "SC11", "11/2026", total_sc11)

        emitir(
            "COBERTURA", "SC11", "11/2026",
            "existentes_ate_hoje_sem_juizo=%d" % existentes_sem_juizo,
            NV if existentes_sem_juizo else "medidores_nomeados",
        )
        if sem_data_o199:
            emitir(
                "COBERTURA", "O199",
                "aceites_sem_data=%d" % sem_data_o199, NV,
            )

    except Exception:
        # Nao imprimir excecao, traceback, objeto ou texto livre:
        # podem conter nomes. Nenhum total parcial vira total integral.
        emitir("ERRO", "SONDA", NV)
        emitir("TOTAL", "O199", "11/2026", NV)
        emitir("TOTAL", "SC11", "11/2026", NV)


sonda()
```

3. **Portas do eventual apply:** O199 → `declarar_dia` (`app/ponto/services/declarar_dia.py:153`); SC11 por dia → `regenerar_celulas_dia`, que delega a `regenerar_celulas_vinculo` sem estender horizonte (`app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:773`). Números atuais de produção: `[nao verificado]`.

_6083 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
