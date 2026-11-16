# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 14:14** · publicado em **16/11/2026 14:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 14:14 · sessao 05339093263a · MIUDOS-REABRE-O-ITEM-3 · leitura · origem=Code

| Entrada | (0) Causa nesta cópia | (1) Classe e alcance de produção | (2) Dinheiro | (3) Pede `!` | (4) Lei aberta? | (5) Cruza obra em voo¹ |
|---|---|---|---|---|---|---|
| **BO-ADMIN-2710/3** | Persiste: justificativas aprovadas viram datas inteiras; o crédito parcial soma Ausência; motor conta abono pelo previsto dos dias. `app/ponto/services/efeito_ausencia.py:40–50`; `app/ponto/services/fechamento.py:660–665`; `app/ponto/motor_calculo_v2.py:2606–2622`. | **OBRA**: produtores e consumidores em arquivos distintos, incluindo motor; não cabe no limite de um arquivo sem motor. Mesmos sítios. | **s** — falta, abono e DSR. `app/ponto/motor_calculo_v2.py:2600–2630`. | **s** — motor é zona inviolável. `CLAUDE.md:142`. | **n para a regra desta causa**: pagamento completa o previsto descontando trabalho do dia; pergunta já respondida. `app/docs/RELATO.md:491`. | **n** nos sítios localizados, frente ao manifesto declarado. `anexo em_voo_1611.txt:2–43`. |
| **BO2910/01-a** | Persiste: `vincular_posto` atribui posto e salva sem acompanhar empresa; a transferência escreve ambos. `app/colaboradores/views.py:325–336,1405–1407`. | **MIUDO — 1 arquivo**, `app/colaboradores/views.py`: as duas portas e as atribuições correspondentes estão nele; não exige modelo, migration ou juiz novo. `:325–336,1405–1407`. | **s** — empresa seleciona fechamentos do TXT. `app/folha/export.py:719–722`. | **n no mecanismo localizado** — trata cadastro posto/empresa; a alteração do posto cadastral não muda o vínculo que responde pelo dia. `app/colaboradores/services/colaborador.py:118–123`; `app/colaboradores/views.py:1405–1407`. | **n** — comportamento já estabelecido pela outra porta. `app/colaboradores/views.py:1397–1407`. | **s — painel do vínculo**, `app/colaboradores/views.py`. O pouso da R3 não elimina este cruzamento atual. `anexo em_voo_1611.txt:37–40`. |
| **BO2910/01-b** | Persiste a ausência de registro da exceção; **mudou o ramo**: agora relê o banco antes de devolver linha/toast, mas continua sem registrar a falha. `app/colaboradores/views.py:337–361`. | **MIUDO — 1 arquivo**, `app/colaboradores/views.py`: tratamento de exceção local, sem alteração de modelo, motor ou autoridade. `:337–361`. | **n no ramo causal** — releitura, renderização e resposta de erro. `app/colaboradores/views.py:337–361`. | **n** — o ramo causal não altera escala, vínculo, motor ou gravado de exportada. `app/colaboradores/views.py:337–361`. | **n** — falta de registro já identificada como defeito, sem decisão de negócio pendente nesta causa. `app/docs/BACKLOG.md:1133`. | **s — painel do vínculo**, `app/colaboradores/views.py`, apesar do pouso da R3. `anexo em_voo_1611.txt:37–40`. |
| **BO2910/02** | Persiste: a retratação pela célula filtra somente os três módulos de turno e exclui texto livre. `app/chamados/reconciliador.py:337–338,997–999`. | **MIUDO no mecanismo localizado — 1 arquivo**, `app/chamados/reconciliador.py`: seleção, predicado e retratação estão nele; já usa a autoridade de dia e abstém sem dia, sem exigir juiz novo. `:997–1032`. Alcance sobre nominal sem dia/célula: **[nao verificado]**; `app/docs/RELATO.md:3404`. | **n no mecanismo** — retrata chamado/perguntas e rejeita apenas justificativa pendente; disputa/chamado não determina a elegibilidade do cálculo. `app/chamados/reconciliador.py:523–541`; `app/ponto/services/fechamento.py:576–579`. | **s** — ampliar o universo altera limite de negócio expressamente mantido até corte do Ronald. `app/chamados/reconciliador.py:977–981`. | **s** — quais chamados de texto livre têm o dia como premissa suficiente para absolvição? O código mantém os demais fora até corte. `app/chamados/reconciliador.py:977–981`. | **n** no arquivo causal necessário localizado. `anexo em_voo_1611.txt:2–43`. |
| **BO2910/05** | Persiste, com linhas deslocadas: formulário permite omitir ambas as horas; view passa `None`; serviço só valida quando alguma hora veio. `app/templates/ponto/partials/painel_ausencia.html:325–330`; `app/ponto/views.py:2700–2715`; `app/ponto/services/ausencia.py:1068–1070`. | **MIUDO quanto à aceitação indevida — 1 arquivo**, `app/ponto/services/ausencia.py`: concentra decisão e validador já existente; template e view participarem do caminho não exige alterar os três para impedir a aceitação. `:1039–1048,1057–1081`. | **s** — decisão/período alimentam folha e invalidam a projeção. `app/ponto/services/ausencia.py:1112–1124`. | **n no escopo registrado** — obrigatoriedade na aprovação já está escrita; não exige regra nova, motor ou alteração de vínculo. `app/docs/BACKLOG.md:1133`; `app/ponto/services/ausencia.py:1039–1081`. | **n para novas aprovações** — período obrigatório já registrado; tratamento de estoque sem período também existe. `app/docs/BACKLOG.md:1133`; `app/templates/ponto/partials/painel_ausencia.html:310–317`. | **n no arquivo causal mínimo**, `app/ponto/services/ausencia.py`. `anexo em_voo_1611.txt:2–43`. |
| **BO2910/06** | Persiste a não reabertura, mas ela é também contrato explícito atual: reativação restaura cadastro/acesso e manda o admin revincular. `app/colaboradores/services/desligamento.py:235–259`. | **Tamanho [nao verificado]**. Há **1 produtor localizado**, `desligamento.py`, mas o número **exigido** depende da escolha de vínculo/início; a autoridade existente já consegue reativar vínculo pela mesma data inicial, portanto não há prova de dois arquivos obrigatórios. `app/colaboradores/services/vinculo.py:100–124,259–283`. | **s pelo encadeamento existente de vínculo → regeneração → fechamento**; impacto nominal **[nao verificado]**. `app/colaboradores/services/vinculo.py:315–348`; `app/ponto/portas/celula.py:483–496`. | **s** — dado de vínculo e mudança do automatismo explícito “admin revincula”. `app/colaboradores/services/desligamento.py:237–238`. | **s** — reabrir qual vínculo, com qual início, preservando ou preenchendo o intervalo desligado? Contrato atual não define reabertura automática. `app/colaboradores/services/desligamento.py:235–238`; escolha por início: `app/colaboradores/services/vinculo.py:100–124`. | **n nos produtores localizados** (`desligamento.py`/`vinculo.py`); envelope final **[nao verificado]**. `anexo em_voo_1611.txt:2–43`. |
| **BO2910/08** | Persiste para decisão humana; **mudou para futuras automáticas**: DNA reescrito já desfaz julgamento futuro, mas conserva expressamente o administrativo e exclui humanos do rejulgamento. `app/ponto/portas/celula.py:297–315,465–470,1135–1139,1155–1164`. | **MIUDO técnico — 1 arquivo**, `app/ponto/portas/celula.py`: reescrita, preservação e autoridade de veredito estão nele, com cartório existente; não exige juiz novo. `:297–315,462–472,970–987,1060–1067`. | **s** — recalcula fechamento por competência. `app/ponto/portas/celula.py:483–496`. | **s** — DNA de escala, inclusive passado, e alteração da preservação administrativa explícita. `CLAUDE.md:149–154`; `app/ponto/portas/celula.py:973–980`. | **s** — quando DNA novo invalida decisão humana, hoje expressamente preservada? `app/ponto/portas/celula.py:973–980,1135–1139`. | **n** no arquivo causal localizado. `anexo em_voo_1611.txt:2–43`. |
| **BO-ADMIN-2710/5** | Persiste: `_marcos_faltantes` pula célula não `missing` sem confrontar `tipo_real`, que o leitor já entrega. `app/chamados/services/declaracao_texto.py:130–139`; `app/escala/services/leitor_celula.py:122–133`. | **MIUDO — 1 arquivo**, `app/chamados/services/declaracao_texto.py`: dados necessários já chegam ao leitor; há teste próprio desse leitor. `:130–139`; `app/chamados/tests/test_hx_marcos_faltantes_celula.py:42–64`. | **s** — registro causal do intervalo e exigência de medir a hora de intrajornada indenizada no DIFF. `app/docs/RELATO.md:3984`; `app/docs/BACKLOG.md:848`. | **n no leitor localizado** — não altera escala/vínculo, motor ou gravado de exportada; consome dados já fornecidos. `app/chamados/services/declaracao_texto.py:100–139`; `app/escala/services/leitor_celula.py:122–133`. | **n na causa localizada** — defeito de leitura já identificado, sem escolha nova de negócio demonstrada. `app/docs/RELATO.md:3984`. | **n** no produtor e teste citados. `anexo em_voo_1611.txt:2–43`. |
| **HJ-017-U1** | Causa atual **[nao verificado]**: código já trata saída por posição e reconciliação da ata; pendência documental não prova defeito remanescente. `app/ponto/turnos.py:1067–1100`; `app/escala/utils.py:1945–2040`; `app/docs/BACKLOG.md:1270`. | — | — | — | — | — |
| **HJ-074** | Produtor atual **[nao verificado]**: default continua verdadeiro, mas nascimento consulta o juiz; nominal não identifica quem gravou a flag. `app/escala/models.py:896–917`; `app/colaboradores/services/vinculo.py:287–296`; `app/docs/BACKLOG.md:2652`. | — | — | — | — | — |
| **HJ-005-U1** | Causa **[nao verificado]**: registro ainda exige censo e declara cobertura parcial não demonstrada para o dia. `app/docs/BACKLOG.md:849`. | — | — | — | — | — |
| **HJ-012-U2** | Causa **[nao verificado]**: classificação da ingestão e medição anterior como cobrança legítima seguem sem reconciliação demonstrada. `app/docs/BACKLOG.md:1572`; `app/docs/RELATO.md:5603`. | — | — | — | — | — |
| **HJ-005-U2** | Produtor **[nao verificado]**: `sem_periodo` demonstra falta de hora, sem identificar sua porta de origem. `app/ponto/turnos.py:2632–2637,2646–2647`. | — | — | — | — | — |
| **HJ-005-U4** | Produtor **[nao verificado]**: mesmo limite; resposta `sem_periodo` não comprova escritor defeituoso. `app/ponto/turnos.py:2646–2647`. | — | — | — | — | — |
| **BO-ADMIN-2710/1** | Produtor atual **[nao verificado]**: causa histórica era cobertura crua; leitor atual distingue `sem_periodo` e classifica cadastro. `app/docs/RELATO.md:3980`; `app/ponto/supra_juiz.py:395–402,453–455`. | — | — | — | — | — |
| **BO-ADMIN-2710/8.1** | Defeito atual **[nao verificado]**: escritor já fotografa antes, distingue editar/criar e registra diferenças; view delega à porta. `app/colaboradores/services/vinculo.py:187–188,368–383`; `app/colaboradores/views.py:247–249`. | — | — | — | — | — |

¹ Cruzamentos restritos às listas fornecidas de BUG-PAR, painel e editor. **O10-D nao verificavel**, pois não tem lista declarada; portanto os `n` dessa coluna não certificam ausência de cruzamento com O10-D. (`anexo em_voo_1611.txt:2–44`.)

**Contagem por classe:**

- **6 MIUDO:** /01-a, /01-b, /02, /05, /08 e BO-ADMIN-2710/5. São os seis mecanismos concentrados em um arquivo identificados acima. (`app/colaboradores/views.py:325–361`; `app/chamados/reconciliador.py:997–1032`; `app/ponto/services/ausencia.py:1039–1081`; `app/ponto/portas/celula.py:297–315,970–987`; `app/chamados/services/declaracao_texto.py:130–139`.)
- **1 OBRA:** BO-ADMIN-2710/3. (`app/ponto/services/efeito_ausencia.py:40–50`; `app/ponto/services/fechamento.py:660–665`; `app/ponto/motor_calculo_v2.py:2606–2622`.)
- **1 tamanho [nao verificado]:** /06. Das quatro pendências, /02, /05 e /08 fecham em **1 arquivo de produção cada**; /06 não permite afirmar número exigido sem decidir vínculo/início. A porta existente impede concluir que dois arquivos sejam obrigatórios. (`app/colaboradores/services/desligamento.py:235–259`; `app/colaboradores/services/vinculo.py:100–124,259–283`.)
- **8 sem causa atual comprovada**, sem classificação posterior: as oito últimas linhas, com os limites de prova nelas citados.

**ELEGÍVEL pela regra literal: nenhuma entrada demonstrada.** Os seis MIUDO têm dinheiro, `!`/lei aberta ou cruzamento declarado; O10-D também permanece não verificável. (`app/folha/export.py:719–722`; `app/chamados/reconciliador.py:977–981`; `app/ponto/services/ausencia.py:1112–1124`; `app/ponto/portas/celula.py:483–496,973–980`; `app/docs/BACKLOG.md:848`; `anexo em_voo_1611.txt:39,44`.)

Motivo por não elegível, uma linha por entrada:

- **BO-ADMIN-2710/3:** OBRA, dinheiro e motor sujeito a `!`. (`app/ponto/motor_calculo_v2.py:2606–2622`; `CLAUDE.md:142`.)
- **BO2910/01-a:** dinheiro/TXT e cruzamento com painel em `app/colaboradores/views.py`. (`app/folha/export.py:719–722`; `anexo em_voo_1611.txt:39`.)
- **BO2910/01-b:** cruza painel em `app/colaboradores/views.py`; o pouso da R3 não o libera deste cruzamento. (`anexo em_voo_1611.txt:37–40`.)
- **BO2910/02:** ampliação do universo depende de corte de negócio, portanto `!` e pergunta de lei. (`app/chamados/reconciliador.py:977–981`.)
- **BO2910/05:** toca insumo e projeção de fechamento. (`app/ponto/services/ausencia.py:1112–1124`.)
- **BO2910/06:** tamanho não demonstrado, vínculo sujeito a `!` e decisão de reabertura ausente. (`app/colaboradores/services/desligamento.py:235–259`; `app/colaboradores/services/vinculo.py:100–124`.)
- **BO2910/08:** dinheiro, DNA de escala e mudança de regra explícita sobre decisão humana. (`app/ponto/portas/celula.py:483–496,973–980,1135–1139`.)
- **BO-ADMIN-2710/5:** envolve intrajornada indenizada no DIFF. (`app/docs/BACKLOG.md:848`; causa: `app/docs/RELATO.md:3984`.)
- **HJ-017-U1:** causa atual **[nao verificado]**, diante dos tratamentos já presentes. (`app/ponto/turnos.py:1067–1100`; `app/escala/utils.py:1945–2040`.)
- **HJ-074:** escritor causal da flag **[nao verificado]**. (`app/colaboradores/services/vinculo.py:287–296`; `app/docs/BACKLOG.md:2652`.)
- **HJ-005-U1:** causa **[nao verificado]**; censo ainda requerido. (`app/docs/BACKLOG.md:849`.)
- **HJ-012-U2:** causa **[nao verificado]**; classificações documentais não reconciliadas. (`app/docs/BACKLOG.md:1572`; `app/docs/RELATO.md:5603`.)
- **HJ-005-U2:** origem da omissão **[nao verificado]**. (`app/ponto/turnos.py:2646–2647`.)
- **HJ-005-U4:** origem da omissão **[nao verificado]**. (`app/ponto/turnos.py:2646–2647`.)
- **BO-ADMIN-2710/1:** defeito atual **[nao verificado]**; leitor já distingue cadastro sem período. (`app/ponto/supra_juiz.py:395–402,453–455`.)
- **BO-ADMIN-2710/8.1:** persistência atual do defeito **[nao verificado]**; escritor já registra edição e diferenças. (`app/colaboradores/services/vinculo.py:368–383`.)

## 16/11 14:11 · sessao 8d2c783a5f2e · - · - · origem=Ronald (tmux)

| Entrada | Resultado da leitura |
|---|---|
| **BO2910/06** | A reativação continua restaurando cadastro/acesso sem escala; isso também é o contrato explícito atual: “admin revincula” (`app/colaboradores/services/desligamento.py:235-259`). **Não há prova de OBRA ≥2 arquivos**: o juiz existente identifica vínculo pela data inicial (`app/colaboradores/services/vinculo.py:100-124`) e o executor já reativa a mesma linha (`app/colaboradores/services/vinculo.py:259-283`). **Um arquivo de produção pode bastar por composição**, mas a escolha de vínculo/data/âncora não está definida pelo dado; número exigido e certificação de MIUDO ficam **[nao verificado]**. **! s**, por vínculo e mudança do automatismo contratual (`app/colaboradores/services/desligamento.py:237-238`). **Lei s**: reabrir a vigência original, incluindo intervalo desligado, ou iniciar novo trecho; qual vínculo/data? **[nao verificado]**. Dinheiro da reabertura **s**, se passar pela autoridade existente que regenera células e provoca recálculo (`app/colaboradores/services/vinculo.py:315-348`; `app/ponto/portas/celula.py:483-496`); alcance nominal **[nao verificado]**. Cruzamento direto de `desligamento.py`/`vinculo.py` com manifestos declarados: **n** (`em_voo_1611.txt:1-44`, anexo fornecido); **O10-D nao verificavel**. |
| **BO2910/08** | A troca do DNA permanece e agora apaga julgamento automático de futuras, conservando admin; a causa humana permanece (`app/ponto/portas/celula.py:297-315,1135-1138,1155-1163`). **MIUDO técnico: 1 arquivo de produção**, `celula.py`, que contém a reescrita e a autoridade que conserva/escreve o veredito; não exige juiz novo, porque o cartório atual já é chamado nesse caminho (`app/ponto/portas/celula.py:462-472,970-986,1060-1067`). A exclusão em `pos_vinculo.py` lê o estado gravado, sem obrigatoriedade demonstrada de editar esse consumidor (`app/colaboradores/services/pos_vinculo.py:62-73`). **Dinheiro s**, recalcula por competência (`app/ponto/portas/celula.py:483-496`). **! s / lei s**: substituir a preservação de decisão humana diante de DNA novo altera a regra explícita “admin decidiu vence o cartório” (`app/ponto/portas/celula.py:973-980,1135-1138`). Cruzamento direto do produtor com manifestos declarados: **n** (`em_voo_1611.txt:1-44`, anexo fornecido); **O10-D nao verificavel**. |

**Cuidado com as citações do manifesto:** não conferi sua numeração linha a linha; use a contagem que o agente principal atribuir ao anexo. **[nao verificado]**

## 16/11 14:11 · sessao 8bc91becb95e · - · - · origem=Ronald (tmux)

| Entrada | Causa atual | Tamanho / arquivos de produção necessários | Dinheiro | ! / lei | Cruzamento |
|---|---|---|---|---|---|
| BO2910/02 | O universo da retratação pela célula continua limitado aos três módulos de turno, excluindo texto livre (`app/chamados/reconciliador.py:337-338`, `:997-999`). | **MIUDO quanto ao mecanismo localizado: 1 produção**, `app/chamados/reconciliador.py`; seleção, predicado e chamada de retratação estão nele (`:997-1032`). **Não exige juiz novo para chamados datados:** já importa `dia_do_fato` (`:45`), cuja leitura genérica suporta célula, contexto, batida e disputa (`app/chamados/juizes.py:88-139`). O alcance sobre o nominal sem data/célula permanece **[nao verificado]** (`app/docs/RELATO.md:3404`). | **n no mecanismo localizado:** retrata chamado/perguntas e rejeita apenas justificativa ainda pendente (`app/chamados/reconciliador.py:523-541`); chamado/disputa não determina elegibilidade do cálculo (`app/ponto/services/fechamento.py:576-579`). | **s / pergunta aberta:** a exclusão dos outros módulos é declarada “até Ronald cortar”, pois neles o dia não é a premissa inteira (`app/chamados/reconciliador.py:977-981`). Pergunta: qual subconjunto de texto livre a célula absolve? | **n nos arquivos necessários localizados**, comparando `app/chamados/reconciliador.py` com o manifesto fornecido; referência numerada do anexo **[nao verificado]**. **O10-D nao verificavel.** |
| BO2910/05 | Ambas as horas podem faltar no formulário; a view conserva `None`, e `decidir` só valida período se alguma hora vier (`app/templates/ponto/partials/painel_ausencia.html:325-330`; `app/ponto/views.py:2700-2715`; `app/ponto/services/ausencia.py:1068-1070`). | **MIUDO quanto à causa de aprovação indevida: 1 produção**, `app/ponto/services/ausencia.py`, escritor único da decisão, com validador de período já existente (`:1039-1048`, `:1057-1081`). A presença de template, view e serviço não prova necessidade de alterar os três; a view já encaminha a omissão à autoridade (`app/ponto/views.py:2700-2715`). Exigir também alteração visual do formulário seria escopo adicional da fila UI registrada em `app/docs/BACKLOG.md:1133`; sua necessidade técnica para impedir a aprovação não está demonstrada. | **s:** decisão e período são insumos da folha e acordam a projeção do fechamento (`app/ponto/services/ausencia.py:1112-1124`). | **Não chamar automaticamente de lei aberta:** o corte de 27/10 admite falta de período com destino ao DP (`app/docs/CORTES.md:385`), enquanto o registro posterior de 29/10 pede período obrigatório na tela (`app/docs/BACKLOG.md:1133`). Se o segundo registro for tomado como escopo decidido, a obrigação já está escrita; sua autoridade como alteração do corte anterior é **[nao verificado]**. | **n no arquivo causal mínimo**, `app/ponto/services/ausencia.py`; os arquivos do manifesto fornecido não o incluem; referência numerada do anexo **[nao verificado]**. **O10-D nao verificavel.** |

Para `/02`, não confundir **tamanho físico localizado de um arquivo** com autorização para ampliar o universo: há autoridade de dia reutilizável, mas o limite semântico foi preservado expressamente (`app/chamados/juizes.py:88-139`; `app/chamados/reconciliador.py:977-981`).

## 16/11 14:10 · sessao 765252e3b7ae · - · - · origem=Ronald (tmux)

| Entrada | (0) Causa atual e limite da prova | (1)–(5) |
|---|---|---|
| HJ-017-U1 | **[nao verificado]**: o produtor histórico já trata a saída pela posição e fecha o par; a ata também reconcilia as bordas. Não há dado atual que demonstre defeito remanescente. (`app/ponto/turnos.py:1067-1100`; `app/escala/utils.py:1945-2040`; pendência documental: `app/docs/BACKLOG.md:1270`.) | — |
| HJ-074 | **[nao verificado]**: o campo conserva default `True`, mas o nascimento pela porta consulta o juiz do tipo/posto; o nominal não demonstra qual escritor gravou sua flag. (`app/escala/models.py:896-917`; `app/colaboradores/services/vinculo.py:287-296`; nominal: `app/docs/BACKLOG.md:2652`.) | — |
| HJ-005-U1 | **[nao verificado]**: o registro ainda pede censo para reconciliar dono/destino e declara COBERTURA-PARCIAL não demonstrada em 01/10. (`app/docs/BACKLOG.md:849`.) | — |
| HJ-012-U2 | **[nao verificado]**: o registro contrapõe ingestão ESTRUTURA à medição anterior como cobrança legítima, sem reconciliação demonstrada. (`app/docs/BACKLOG.md:1572`; `app/docs/RELATO.md:5603`.) | — |
| HJ-005-U2 | **[nao verificado]**: `sem_periodo` é a resposta prevista quando falta alguma hora; isso não identifica a porta que originou a omissão. (`app/ponto/turnos.py:2632-2637`; `app/ponto/turnos.py:2646-2647`.) | — |
| HJ-005-U4 | **[nao verificado]**: mesmo limite de U2; `sem_periodo` não comprova escritor defeituoso. (`app/ponto/turnos.py:2646-2647`.) | — |
| BO-ADMIN-2710/1 | **[nao verificado]**: a causa histórica era cobertura lida sem período; o leitor atual possui os ramos de `sem_periodo` que classificam o cadastro. (`app/docs/RELATO.md:3980`; `app/ponto/supra_juiz.py:395-402`; `app/ponto/supra_juiz.py:453-455`.) | — |
| BO-ADMIN-2710/8.1 | **[nao verificado]** como defeito atual: o escritor já fotografa o antes, distingue edição/criação e registra antes/depois; a view delega a trilha à porta. (`app/colaboradores/services/vinculo.py:187-188`; `app/colaboradores/services/vinculo.py:368-383`; `app/colaboradores/views.py:247-249`.) | — |

Não elegíveis: cada uma das oito entradas carece de causa atual comprovada, pelos limites documentados nas respectivas linhas; demais critérios não classificados.

## 16/11 14:09 · sessao 34d827b27435 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · censo · origem=Code

O código sustenta o corte: o evento de cadastro seleciona futuras existentes somente até o fim de `_janela_default(hoje)`; o gerador diário pula as futuras já existentes. Porém, o efeito visual varia: Minha Escala segue `trabalha=True`, enquanto o calendário pode mostrar folga pela precedência. (`app/ponto/services/cadastro_futuro.py:26-45`; `app/ponto/portas/celula.py:930-935`; `app/escala/views.py:40-54`; `app/ponto/status_dia.py:296-319`)

O anexo informa 436 folgas além da competência, 37 contradições, 18 colaboradores; passado 274 e futuro dentro da competência 22, ambos sem contradições; EC 1038 com cinco folgas e cinco células `True`. São números fornecidos, sem reprodução das sondas; deduplicação por colaborador×dia, origem individual das células e efeito financeiro: **[não verificado]**.

**(a) Chamadores por AST**

Denomino **J** a janela retornada por `_janela_default(hoje)`: menor início e maior fim das competências correntes de **todas as empresas ativas**. Portanto, o corte herdado não é necessariamente o fim da competência da empresa do vínculo. (`app/ponto/management/commands/gerar_celulas.py:290-300`)

Todos os chamadores abaixo herdam J: o suplemento restringe a seleção a amanhã em diante, `origem='gerada'`, células existentes e `data__range=(ini,fim)`; `desde` eleva o início e `datas` restringe as datas. Cada dia segue à porta com `estender_horizonte=False`. (`app/ponto/services/cadastro_futuro.py:26-45`)

| Chamador de `regenerar_futuras_do_cadastro` | Evento representado | Herda corte |
|---|---|---|
| `relavrar_celulas_da_foto` | Foto de folgas materializada; alcança geração mensal, lançamento manual, alteração/remoção, lote e reparentamento. | Sim, J. (`app/escala/folgas.py:215-218`, `:298-299`, `:337`, `:380-382`, `:418-421`, `:523`) |
| `sincronizar_foto_futura_do_calendario` | Troca do calendário do vínculo ou alteração da definição do calendário. | Sim; também seleciona suas próprias datas dentro de J. (`app/colaboradores/services/vinculo.py:450-465`; `app/escala/folgas.py:599-601`, `:619-642`) |
| `_ec_vigencia_regenera_celulas` | Alteração de início/fim de vínculo existente por `save`; ignora criação/raw e vigência inalterada. Declara `substitui_dono=True`. | Sim, J. (`app/escala/signals.py:54-83`) |
| `_ec_ancora_regenera_celulas` | Âncora alterada para valor não nulo, em vínculo existente. | Sim, J. (`app/escala/signals.py:111-134`) |
| `_propagar_futuras_template` | Edição do TipoEscala, inclusive pelo admin; percorre vínculos daquele tipo. | Sim, J. (`app/escala/services/cadastro_tipo.py:328`, `:370-388`) |
| `executar_vinculo` | Criação, atualização ou reativação do vínculo salvo; desde `data_inicio`, com substituição de dono. | Sim, J. (`app/colaboradores/services/vinculo.py:259-299`, `:335-340`) |
| `fechar_vigencia` | Fechamento/alteração do fim por `update`; começa após o menor fim anterior/novo; substitui dono. | Sim, J. (`app/colaboradores/services/vinculo.py:641`, `:652-664`) |
| `reabrir_vigencia_impossivel` | Reparo que reativa vínculo com vigência impossível; substitui dono. | Sim, J. (`app/colaboradores/services/vinculo.py:730`, `:744-752`) |
| `marcar_trabalha_em_feriado` | Mudança da opção de trabalhar em feriado. | Sim, J. (`app/colaboradores/services/vinculo.py:890-913`) |
| `reavaliar_pela_troca_de_municipio` | Município do posto alterado; percorre vínculos do posto. | Sim, J. (`app/colaboradores/services/vinculo.py:953-963`) |
| `reverter_para_snapshot` | Restauração dos vínculos do snapshot; substitui dono. | Sim, J. (`app/colaboradores/services/vinculo.py:1029-1035`) |
| `corrigir_escala_retroativa.Command._aplicar` | Correção retroativa, com suplemento futuro desde o piso solicitado; substitui dono. | Sim, J. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:225-245`) |
| `regenerar_futuras_por_feriado` | Criação/edição/exclusão de feriado; alteração considera data antiga e nova. | Sim; corta as datas em J antes de chamar o suplemento. (`app/core/signals.py:28-43`; `app/ponto/services/cadastro_futuro.py:57-78`) |

Sem `substitui_dono`, o suplemento preserva dias cujo vínculo responsável é outro; com ele, declara a intenção de substituir o responsável na linha do tempo. Isso não amplia J. (`app/ponto/services/cadastro_futuro.py:9-15`, `:37-45`)

| Chamador direto de `_janela_default` | Uso e consequência |
|---|---|
| `regenerar_futuras_do_cadastro`, linha 26 | Define o teto das relavras futuras dos eventos acima. (`app/ponto/services/cadastro_futuro.py:26-32`) |
| `regenerar_futuras_por_feriado`, linha 57 | Descarta eventos de feriado fora de J antes da seleção das células. (`app/ponto/services/cadastro_futuro.py:57-64`) |
| `sincronizar_foto_futura_do_calendario`, linha 619 | Seleciona células futuras em J; a foto mensal pode avançar até o último dia dos meses envolvidos. (`app/escala/folgas.py:619-642`) |
| `meses_da_janela`, linhas 75 e 76 | Consulta J de hoje e novamente no dia posterior ao seu fim; inclui corrente e seguinte para a foto. (`app/escala/management/commands/lavrar_foto_folgas.py:74-82`) |
| `gerar_celulas.Command.handle`, linha 344 | Usa J quando não recebe o par explícito `--ini/--fim`. (`app/ponto/management/commands/gerar_celulas.py:337-350`) |

Também aparecem chamadas em testes: contrato de DNA; janela corrente+seguinte; fechamento/reabertura/geradora sem vigência; relógio obrigatório, mutante e wrapper `real_futuro`. Não representam eventos adicionais de produção, e parte usa serviços carregados dinamicamente ou código mutado. (`app/escala/tests/test_celula_dia.py:165`; `app/escala/tests/test_foto_folga_continua.py:116-117`; `app/ponto/tests/test_cadastro_futuro_c4.py:58`, `:126`, `:191`; `app/ponto/tests/test_cadastro_futuro_c5d.py:121`, `:127`, `:135`, `:145`; `app/ponto/tests/test_cadastro_futuro_c5e.py:46-53`)

**(b) Até onde cria células**

Sem argumentos, `gerar_celulas` cria ausentes até o **maior fim das competências correntes das empresas ativas**. A data concreta depende desses cadastros: **[não verificado]**. Se todos os cortes fossem 21, em 16/11/2026 o fim seria 20/11/2026; isso é hipótese, não identificação do corte de produção. (`app/ponto/management/commands/gerar_celulas.py:299-300`, `:337-350`; `app/ponto/janelas.py:10-33`)

Os pontos de criação de produção encontrados estão na porta da célula:

| Criador | Alcance |
|---|---|
| `regenerar_celulas_vinculo` | Cria ausentes em `[desde,ate]`; a extensão automática só aumenta `ate` até o fim corrente, nunca reduz um fim solicitado mais distante. (`app/ponto/portas/celula.py:178-199`, `:289-294`) |
| `editar_dia` | Cria célula humana na data solicitada, sujeito a vínculo responsável e guardas; não impõe teto na competência corrente. (`app/ponto/portas/celula.py:597-639`) |
| `gerar_celulas_periodo` | Cria ausentes no intervalo recebido, respeitando cobertura do vínculo e a seleção adicional dos inativos. (`app/ponto/portas/celula.py:883-917`) |

Há caminhos concretos para criar além da competência: a CLI aceita intervalo explícito; ESCALA-AUTO começa normalmente na próxima competência e regenera pelo menos seu primeiro dia, podendo ir até a última célula existente. Isso demonstra possibilidades de origem, sem atribuir a elas as células da sonda. (`app/ponto/management/commands/gerar_celulas.py:333-344`; `app/escala/services/escala_auto_executor.py:24-31`, `:52-58`, `:99-108`; origem da sonda **[não verificado]**)

O próprio comentário do gerador registra um algoritmo anterior que podia cobrir duas competências em janeiro; sua participação no resíduo fornecido é **[não verificado]**. (`app/ponto/management/commands/gerar_celulas.py:293-295`)

**(c) Relavra na virada de competência**

**Não existe relavra geral de futuras existentes disparada pela simples virada**, no fluxo agendado examinado: o gerador diário seleciona somente `data=hoje` para regeneração; depois, a geração pula células existentes de hoje em diante. Entrar na nova janela não remove esse bloqueio. (`app/config/crons.py:425-426`; `app/ponto/management/commands/gerar_celulas.py:303-325`; `app/ponto/portas/celula.py:919-935`)

Há relavra futura **condicionada a outro evento**: o cron de foto pode gravar uma foto faltante, cuja escrita chama a relavra. Quando a foto já está completa, ele pula o vínculo-mês; portanto esse caminho não constitui revisão do acervo contraditório existente. (`app/escala/management/commands/lavrar_foto_folgas.py:123-129`, `:163-170`; `app/escala/folgas.py:215-218`, `:298-299`)

A correção “na manhã do próprio dia” é uma tentativa condicionada a célula gerada e vínculo responsável encontrado; o código não permite garantir o resultado dos 37 casos fornecidos. (`app/ponto/management/commands/gerar_celulas.py:316-325`; resultado individual **[não verificado]**)

**(d) Corrente + seguinte na foto**

O motivo registrado é antecipar a foto e a identificação de calendário vazio: com apenas a corrente, novembro ficava invisível durante parte de outubro, justamente quando ainda havia tempo para cadastrar. A rotina consulta a janela duas vezes e cobre os meses civis envolvidos. (`app/escala/management/commands/lavrar_foto_folgas.py:59-82`)

A ordem configurada é foto às 05:47, depois geração às 05:50, dependente da foto: a intenção declarada é a folga chegar antes da célula. Execução efetiva desses horários em produção: **[não verificado]**. (`app/config/crons.py:425-426`; `app/escala/management/commands/lavrar_foto_folgas.py:24-29`)

Em `cadastro_futuro`, o impedimento técnico é o próprio seletor: consulta somente J de hoje, filtra existentes nessa janela e desliga extensão do horizonte na porta. Não há nesses corpos uma recusa genérica de competência seguinte pela porta inferior, que aceita fim mais distante; eventual proibição normativa adicional: **[não verificado]**. (`app/ponto/services/cadastro_futuro.py:26-45`; `app/ponto/portas/celula.py:178-199`)

Há ainda alcance diferente dentro da sincronização: seleciona células apenas em J, mas materializa FolgaDia até o fim dos meses civis envolvidos, porque uma folga posterior pode definir a fase de um dia anterior. A relavra final continua restrita às datas selecionadas. (`app/escala/folgas.py:619-642`)

**(e) Leitores e decisões com `True` em dia de FolgaDia**

A distinção central é esta: `EC.eh_dia_trabalho` devolve imediatamente o booleano da célula; já a precedência pode conservar `era_dia_previsto=True` e fazer uma FolgaDia válida vencer como fato. Folga de calendário precisa pertencer ao vínculo do dia; manual vale para a pessoa. (`app/escala/models.py:1092-1096`; `app/ponto/precedencia.py:235-239`, `:356-363`, `:379-407`)

Nos resultados abaixo que dependem da precedência, “folga” pressupõe FolgaDia válida e ausência de fato de prioridade superior. (`app/ponto/precedencia.py:379-407`)

| Leitor | O que mostra ou decide |
|---|---|
| **Minha Escala HTML**, hoje +13 dias | `True` mostra previsão de trabalho e horários do DNA, quando presentes. (`app/escala/views.py:38-60`; `app/escala/models.py:1124-1133`; `app/templates/escala/minha_escala.html:66-81`) |
| **API escala/v2**, hoje +13 dias | Publica `trabalho=True`; os horários vêm do **template**, por `te.marcos_do_dia`. Renderização do aplicativo nativo: **[não verificado]**. (`app/api/views.py:918-945`) |
| **API espelho/v2**, mês solicitado | No ramo sem card/batida, pode publicar status de folga pelo D4 e, simultaneamente, horários previstos preenchidos pelo template porque a célula diz `True`. (`app/api/views.py:1587-1618`; `app/ponto/status_dia.py:296-319`) |
| **Calendário de ponto do colaborador no admin**, inclusive janela futura | Lê status do D4; no ramo sem batida/lavra aplicável, mostra folga pela precedência, apesar do booleano `True`. (`app/colaboradores/services/calendario.py:227-239`, `:634-639`, `:694-696`; `app/ponto/status_dia.py:286-319`) |
| **Painel do vínculo e modal da agenda** | Seguem `eh_dia_trabalho` e marcos do vínculo: `True` entrega trabalho/horários. AgendaDia, quando presente, tem prioridade visual. O modal aceita semana navegável. (`app/colaboradores/views.py:280-294`, `:2239-2268`; `app/templates/colaboradores/partials/painel_vinculo.html:218-230`; `app/templates/colaboradores/partials/agenda_modal.html:97-133`) |
| **Plano de Folgas** | A FolgaDia gravada ganha antes da consulta à célula; mostra seu tipo de folga. (`app/escala/views_plano_folgas.py:60-82`) |
| **Disponibilidade / retorno de folga**, próximos sete dias | Se hoje é `False`, o primeiro futuro `True` vira data de retorno, sem consultar FolgaDia nessa decisão. (`app/colaboradores/views.py:2903-2921`) |
| **Semana do posto** | Mantém `previsto=True`; sem batida, consulta precedência e pode registrar `explicado_por='folga'`, sem contar buraco. (`app/escala/services/semana_do_posto.py:132-158`) |
| **Prévia de fase** | Antes de `desde`, lê a célula do vínculo atual; depois, projeta pelo template proposto. Portanto o trecho anterior à troca pode publicar plantão para uma célula `True`. (`app/escala/services/fase_na_tela.py:179-201`) |

A grade também aplica FolgaDia válida como overlay: muda `tipo_dia` para folga e limpa marcos e órfãs, mesmo após a montagem inicial baseada na célula. (`app/escala/utils.py:1128-1153`, `:1844`)

Algumas telas não alcançam normalmente a unidade pedida: o drawer termina no fim da corrente; o espelho normal termina em hoje, embora seu cabeçalho possa ler o restante da semana; o PDF chama o espelho com ontem e recorta as linhas impressas. (`app/colaboradores/services/drawer.py:115-123`; `app/ponto/services/espelho.py:222-229`, `:1225-1227`, `:1262`; `app/relatorios/pdf_espelho.py:598-603`)

Para perguntas e cobranças:

| Leitor/porta | Decisão |
|---|---|
| **Fábrica de perguntas** | Recusa `cel.data > hoje`; `True` sozinho não cria pergunta futura. Também consulta dispensa e vencimento dos marcos. (`app/chamados/services/fabrica_da_celula.py:110-121`) |
| **Cartório avulso** | Recusa data futura; hoje, sem teto por marco, pode esperar o fim do turno. (`app/ponto/services/cartorio.py:2025-2038`) |
| **`dia_sem_trabalho`** | Não consulta FolgaDia diretamente: dispensa com célula `False` ou ata preenchida de outro tipo, ressalvada ausência parcial. `True` sem essa ata não dispensa. (`app/chamados/juizes.py:445-460`) |
| **Gate `celula_veta_cobranca`** | Consulta booleano e veredito; `False` veta como folga. FolgaDia isolada não integra esse gate. (`app/ponto/marcos.py:154-163`) |
| **Perguntas no app / cobrança manual** | O app lê perguntas existentes filtradas por `pergunta_viva`; cobrança usa essa mesma lista pendente e o prazo dos pais. Não deriva pendência diretamente de `trabalha=True`. (`app/chamados/services/pergunta_no_app.py:59-85`; `app/chamados/services/cobranca.py:45-48`, `:82-94`) |
| **Pergunta futura preexistente** | Não há garantia de invisibilidade: o C6 pode devolver viva quando não materializada. Existência disso no dado fornecido: **[não verificado]**. (`app/chamados/juizes.py:1482-1491`) |

Há critérios diferentes no caminho da cobrança: o contador de faltas de hoje exclui fatos classificados como folga pela precedência; o trecho do funil emissor consulta precedência para veto de feriado e depois o gate da célula. Isso não prova emissão futura, diante das guardas temporais anteriores. (`app/ponto/services/faltas_de_hoje.py:39-62`; `app/ponto/management/commands/detectar_ausencias.py:607-634`; `app/chamados/services/fabrica_da_celula.py:114-115`)

Quanto aos crons e instrumentos:

| Leitor | Alcance e efeito |
|---|---|
| **Vigia de cadastro futuro**, dentro de `gerar_celulas` | Mede futuras existentes apenas em J; compara booleano/DNA com cadastro do vínculo do dia. Além de J fica fora dessa medição padrão; não relavra células. (`app/ponto/management/commands/gerar_celulas.py:344-353`; `app/ponto/services/vigia_cadastro_futuro.py:31`, `:52-80`, `:99-108`) |
| **`tripwire_celulas`**, configurado às 06:05 | Mede geradas existentes até hoje+45. Compara com vínculo **ativo de colaborador ativo**; `True` contra cálculo `False` acusa STALE. Apenas imprime. (`app/config/crons.py:429`; `app/ponto/management/commands/tripwire_celulas.py:17-33`, `:357-370`) |
| **Auditor C5**, configurado às 07:18 | Percorre FolgaDia sem teto de data, cruza pessoa+data com célula, acusa `trabalha is True` e exclui exportadas. Alcança o resíduo além da corrente. (`app/config/crons.py:542-543`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1536-1554`) |
| **`celulas_contra_o_template`**, chamado pelo placar | Pode ler todo o acervo gerado `True`; compara com template do vínculo ativo, sem consultar FolgaDia. Só acusa esse conflito se o template também responder folga. (`app/escala/management/commands/celulas_contra_o_template.py:25-40`; `bin/placar_code.sh:261-263`) |
| **`previsto_cego` / censo 12x36** | Leem previsão até o fim corrente da empresa; não cobrem o acervo posterior por suas janelas padrão. (`app/escala/services/previsto_cego.py:94-110`, `:137-141`; `app/ponto/management/commands/censo_fase_12x36.py:88-89`, `:151-179`) |
| **Cartório diário, detector de ausências, vigia de hora** | Respectivamente: passado; hoje/ontem; células `True` de hoje/ontem com marco vencido. Não julgam antecipadamente as células-dia futuras da unidade pedida. (`app/ponto/management/commands/processar_cartorio.py:49-65`; `app/ponto/management/commands/detectar_ausencias.py:137-145`; `app/ponto/services/vigia_de_hora.py:20-23`, `:126-134`) |
| **Vigia de perguntas / furos sem cobrança / reconciliar grade** | Trabalham sobre passado: perguntas com `data<hoje`, furos até ontem e grade descartando hoje/futuro antes da avaliação. (`app/chamados/management/commands/disparar_perguntas_competencia.py:177-189`; `app/ponto/services/furos_sem_cobranca.py:53-63`; `app/ponto/management/commands/reconciliar_grade.py:118-135`) |

O C5 existente **não equivale ao contador descrito no anexo**: não aplica piso na competência corrente nem filtro explícito de vínculo ativo; exclui exportadas, aceita qualquer origem de célula e conta ocorrências de FolgaDia. Seu total usa `len(lista)`, sem deduplicar colaborador×dia. Resultados e execução efetiva desses instrumentos em produção: **[não verificado]**. (`app/chamados/management/commands/auditar_invariantes_chamados.py:560-567`, `:1536-1554`)

**Script AST usado**

O script abaixo faz leitura sintática, resolve imports locais, imports com apelido, atributos e aliases simples por atribuição; separa referências nominais/dinâmicas. Identidade de chamadas produzidas por reflexão ou `exec`: **[não verificado]**.

```python
import ast
from pathlib import Path
T = {'ponto.services.cadastro_futuro.regenerar_futuras_do_cadastro',
     'ponto.management.commands.gerar_celulas._janela_default'}
S = (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef, ast.Lambda)
rows, errors = [], []
def name(n):
    if isinstance(n, ast.Name): return n.id
    if isinstance(n, ast.Attribute):
        return (name(n.value) or '<dinamico>') + '.' + n.attr
    if isinstance(n, ast.Subscript) and isinstance(n.slice, ast.Constant):
        return '<indice>.' + str(n.slice.value)
def resolve(n, env):
    s = name(n)
    if s:
        h, *r = s.split('.')
        return '.'.join([env.get(h, h), *r])
def scan(node, outer, mod, path, owner='<module>'):
    nodes, children = [], []
    def collect(n):
        if isinstance(n, S): children.append(n); return
        nodes.append(n)
        for c in ast.iter_child_nodes(n): collect(c)
    for c in ast.iter_child_nodes(node): collect(c)
    env = dict(outer)
    for n in nodes:
        if isinstance(n, ast.Import):
            for a in n.names:
                env[a.asname or a.name.split('.')[0]] = a.name if a.asname else a.name.split('.')[0]
        if isinstance(n, ast.ImportFrom):
            base = n.module or ''
            if n.level: base = '.'.join(mod.split('.')[:-n.level] + ([base] if base else []))
            for a in n.names: env[a.asname or a.name] = base + '.' + a.name
    for c in children:
        

[truncado em 20000 de 23948 caracteres]

## 16/11 14:05 · sessao 2675e78cc9e0 · - · - · origem=Ronald (tmux)

Complemento dos leitores futuros:

| Leitor | Alcance real e decisão atual |
|---|---|
| `tripwire_celulas` | **Lê além da competência:** padrão hoje até hoje+45 dias (`app/ponto/management/commands/tripwire_celulas.py:357-362`), células existentes `origem='gerada'`; compara `trabalha` e DNA com o **vínculo ativo de colaborador ativo**, usando `eh_dia_trabalho_calculado` (`:17-33`). `True` quando o cadastro calcula `False` vira `STALE`, apenas impresso; não relavra (`:365-370`). Cron 06:05 (`app/config/crons.py:429`). |
| Auditor `C5` | **Lê todo o acervo, inclusive além da competência:** percorre todas FolgaDia, sem filtro de data; aceita a do vínculo que cobre ou se outro vínculo cobre; cruza pessoa+data com CelulaDia; registra a que tem `trabalha is True`, excluindo competências exportadas (`app/chamados/management/commands/auditar_invariantes_chamados.py:1536-1554`). Cron 07:18 `--html` (`app/config/crons.py:542-543`). |
| `celulas_contra_o_template` | **Lê todo o acervo, inclusive além da competência**, salvo `--desde` opcional: células geradas `True` de pessoas com vínculo ativo; acusa quando o **template do vínculo ativo** responde `False` (`app/escala/management/commands/celulas_contra_o_template.py:25-40`). Não lê FolgaDia; `True`+FolgaDia só acusa se template também disser folga (`:27-37`). Chamado por `bin/placar_code.sh:261-263`; cadastro o declara sem cron próprio (`app/config/crons.py:1156-1163`). |
| `fase_conflitante` | **Não lê CelulaDia**; conta dias dos últimos 60 dias até hoje, comparando foto FolgaDia com âncora (`app/escala/services/fase_conflitante.py:65-83`). Porém carrega **todas as FolgaDia do vínculo ativo**, inclusive futuras, e a foto mais próxima influencia a resposta de cada dia contado (`:51-58`, `:71-80`). Escreve somente snapshot no comando (`app/escala/management/commands/lavrar_fase_conflitante.py:30-38`); cron 07:14 (`app/config/crons.py:498-499`). |
| `previsto_cego` | **Limita-se à competência corrente de cada empresa**, não ao acervo além dela (`app/escala/services/previsto_cego.py:137-141`). Percorre dias até fim e lê `eh_dia_trabalho`; no primeiro dia com minutos previstos positivos deixa de considerar o colaborador cego (`:94-110`). Unidade é colaborador com ao menos duas batidas, não conflito célula/FolgaDia (`:165-175`, `:193-205`). Cron 07:33 (`app/config/crons.py:514-515`). |
| `fase_na_tela` | Não é cron. O calendário admite mês/ano informado, inclusive futuro; antes de `desde` lê `ec_atual.eh_dia_trabalho`, depois de `desde` usa o template proposto; publica `plantao` e `futuro` (`app/escala/services/fase_na_tela.py:179-201`). Portanto `True` pode marcar plantão no trecho “antes da troca”, mesmo com FolgaDia; o trecho proposto vem do template (`:193-201`). Contagem de furos antes/depois fica nos últimos 28 dias até ontem (`:181-189`). |
| `reconciliar_grade` | Padrão termina hoje; lê grade e lâmpadas DNA da janela, mas **descarta hoje e futuro** antes de avaliar (`app/ponto/management/commands/reconciliar_grade.py:78-80`, `:118-135`). Cron 06:20 sem `--apply` (`app/config/crons.py:460-462`). |
| `censo_fase_12x36` | Padrão anterior+atual por empresa (`app/ponto/management/commands/censo_fase_12x36.py:88-89`), carrega células até fim (`:151-157`); lê `eh_dia_trabalho` em todos esses dias (`:178-179`), mas furos e folgas trabalhadas contam **antes de ontem** (`:185-192`). Cron `--min-plantoes 6 --lavrar`, sem `--curar` (`app/config/crons.py:638-639`). |
| `vigia_b5_ausencia` | Exige célula já julgada e `data__lte=hoje`; exclui futuro (`app/ponto/management/commands/vigia_b5_ausencia.py:53-55`). |
| `vigiar_motor_autoridade` | Janela hoje−31 até hoje, motor calcula apenas esse intervalo (`app/ponto/management/commands/vigiar_motor_autoridade.py:32-43`, `:57-59`); não mede resíduo futuro além corrente. |
| `tripwire_vinculo_orfao` | Não lê CelulaDia nem FolgaDia; consulta vínculo ativo de pessoa não ativa (`app/colaboradores/management/commands/tripwire_vinculo_orfao.py:44-51`). |
| `mapa_divergencia` | Janela da competência indicada por `--mes/--ano`; compara renders e separa futuro de passado nas contagens (`app/ponto/management/commands/mapa_divergencia.py:30-32`, `:93-98`, `:158-164`). Não é contador específico True×FolgaDia; separa divergências futuras por serem diferenças de apresentação, segundo comentário (`:83-87`). |

**Diferenças relevantes para as DECISÕES:**

- **Já existe um detector C5 de FolgaDia×célula True**, mas não tem o contrato do contador solicitado no anexo: não limita `data >= início da competência corrente`, não exige vínculo ativo, não distingue origem, exclui exportadas e percorre uma linha por FolgaDia (`app/chamados/management/commands/auditar_invariantes_chamados.py:1536-1554`). Seu total é `len(lista)`, podendo repetir pessoa+data quando houver duas FolgaDia nessa chave (`:560-567`); a possibilidade de duas fotos por pessoa/data é reconhecida pelo carregador (`app/ponto/precedencia.py:201-202`).
- **Dois vigias alcançam horizontes diferentes:** vigia de cadastro futuro do gerador mede apenas sua janela corrente (`app/ponto/management/commands/gerar_celulas.py:344-346`; `app/ponto/services/vigia_cadastro_futuro.py:31`), enquanto `tripwire_celulas` mede as geradas existentes até hoje+45 (`app/ponto/management/commands/tripwire_celulas.py:357-362`, `:21-33`).
- **Vínculos de comparação diferem:** vigia cadastro futuro consulta o vínculo do dia com a geradora da célula (`app/ponto/services/vigia_cadastro_futuro.py:52-59`); tripwire e contador contra template escolhem o ativo da pessoa (`app/ponto/management/commands/tripwire_celulas.py:17-27`; `app/escala/management/commands/celulas_contra_o_template.py:28-36`).
- **Ausência de célula além corrente não é defeito para `medir_ausentes`:** ele pode carregar datas até hoje+45, mas só acusa se `ec.data_inicio <= d <= fim_da_competência` (`app/ponto/management/commands/tripwire_celulas.py:49-68`); já célula **existente** divergente além corrente entra em `medir_stale` (`:21-33`).
- Execução efetiva, resultados e alarmes desses crons em produção: `[não verificado]`; o levantamento acima prova configuração e código, não sua execução.

## 16/11 14:05 · sessao 79ab2edd0d65 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**VEREDITO: REPROVA.** Existe carregador real pelo drawer que o teste de exclusividade não encontra. Isso invalida a premissa usada para limitar o smoke a `vincular.html`. (`app/folha/retrato_folha.py:1004`, `app/templates/folha/_retrato_o_que_fazer.html:73`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:278`)

1. **Carregador real: sim; cobertura completa da interação: não.**

   A sonda chama `carregarPainel(cid)`, entrega pelo stub o HTML obtido da view e percorre a implementação real de inserção do fragmento. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:83`, `:203`, `:216`; `app/templates/colaboradores/vincular.html:143`)

   Ainda permitem verde com funcionalidade quebrada: [nao verificado]

   - **Clique na linha quebrado:** a chamada direta pula o `onclick` e `abrirPainelVinculo`, inclusive a escrita de `_painelAtualCid`. (`app/templates/colaboradores/partials/vinculo_row.html:4`; `app/templates/colaboradores/vincular.html:136`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:216`)
   - **HTMX sem processar o formulário:** a alternância depende do `onchange` nativo; o smoke não envia o formulário. Retirar `htmx.process(p)` pode preservar o resultado observado e comprometer as ações HTMX. (`app/templates/colaboradores/vincular.html:161`; `app/templates/colaboradores/partials/painel_vinculo.html:192`, `:196`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:221`)
   - **Conteúdo invisível com área positiva:** a medição não exige `visibility:visible` nem opacidade positiva; essas regressões podem conservar o veredito. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:209`)

   **O MORDE morde a ausência do `onchange`, por inspeção:** remove exatamente uma ocorrência e exige o calendário inicial nas três medições, incompatível com a expectativa principal de alternância. Sua execução efetiva está **[não verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:243`, `:261`, `:265`)

2. **A busca não cobre todas as formas, e omite um caminho existente.**

   Só examina `.html`/`.js` em `templates` e `static`. A regex reconhece uma concatenação específica e o nome terminado em aspa simples; não cobre `reverse()` em Python, nome com aspas duplas ou todas as strings montadas. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:276`, `:278`)

   O caminho omitido é concreto:

   - `PORTA_DO_TIPO` oferece `colaboradores:painel_vinculo` para `escala`, `folga` e `sem_escala`. (`app/folha/retrato_folha.py:1004`)
   - `portas_da_linha` resolve a rota por `reverse()` e publica `href`. (`app/folha/retrato_folha.py:1066`, `:1071`, `:1075`)
   - A view entrega `retrato_portas`; o painel inclui o bloco que chama `abrirDrawer(..., p.href, ...)`. (`app/colaboradores/views.py:1006`, `:1016`; `app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/folha/_retrato_o_que_fazer.html:73`)
   - O drawer faz `fetch(url)`, injeta o HTML e processa HTMX. (`app/templates/core/_drawer_generico.html:19`, `:26`, `:33`)

   As duas cascas incluem esse drawer; um fluxo de produto efetivamente alcançável na `base_app`, com permissões e registros suficientes, está **[não verificado]**. (`app/templates/base.html:399`; `app/templates/base_app.html:154`)

3. **A sentinela distingue descarte e troca de datas, mas não comprova a associação ao mês exibido.**

   O teste exige as quatro chamadas exatas, incluindo multiplicidade, e a sequência esperada de datas dentro de cada calendário. Assim, descartar os retornos e mostrar as datas normais, inverter os retornos mensais ou trocar calendários com sentinelas distintas viola as asserções. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:154`, `:159`, `:163`)

   A lacuna é que `_DIA.findall(corpo)` achata todos os `<time>` do calendário. Trocar os títulos dos meses ou colocar ambos os retornos sob um único título pode preservar essa sequência: o teste não verifica cada par **rótulo do mês → retorno**. A distinção entre calendários também usa `pk % 12`, sem garantir unicidade para IDs arbitrários. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:38`, `:154`, `:163`; `app/templates/colaboradores/partials/painel_vinculo.html:206`)

4. **Escritores e leitores dos valores auditados:**

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | Fragmento e resposta do fetch stub | `_html` obtém a resposta real; `_mede` serializa; a sonda instala o stub e devolve `FRAG`. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:86`, `:250`, `:199`, `:201`) | `carregarPainel` chama fetch, lê `.text()` e insere o HTML. (`app/templates/colaboradores/vincular.html:147`, `:148`, `:152`) |
   | `data-folgas-cal` e `hidden` | O template escreve o PK e a ocultação inicial; o `onchange` atualiza `hidden`. (`app/templates/colaboradores/partials/painel_vinculo.html:205`, `:196`) | O mesmo handler compara o atributo ao valor selecionado; a sonda lê atributo, estilo e geometria. (`app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:209`) |
   | `folga_calendario_id` no select | O template escreve opções/seleção; a sonda atribui outro PK e vazio. (`app/templates/colaboradores/partials/painel_vinculo.html:198`, `:200`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:222`, `:224`) | O handler lê `this.value`; a view lê o campo no POST. O smoke não faz esse POST. (`app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/views.py:230`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:221`) |
   | Calendário persistido do vínculo | Fixture: criação do vínculo. Produção: cópia do padrão, troca explícita e cópia em vínculos derivados. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:80`; `app/colaboradores/services/vinculo.py:161`, `:461`, `:1092`, `:1113`) | A view busca a escala ativa; o template usa seu ID para seleção e ocultação inicial. (`app/colaboradores/views.py:268`; `app/templates/colaboradores/partials/painel_vinculo.html:200`, `:205`) |
   | Datas projetadas / sentinelas | A fonte produz as datas; o mutante substitui seu retorno. (`app/escala/folgas.py:43`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:153`) | A view monta a projeção; o template renderiza; o teste compara os `<time>`. (`app/colaboradores/views.py:304`; `app/templates/colaboradores/partials/painel_vinculo.html:206`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:163`) |
   | `erros` | `abre` coleta somente linhas de stderr contendo `:ERROR:CONSOLE`. (`app/core/tests/test_smoke_chromium.py:124`) | O teste principal exige lista vazia; o MORDE descarta essa lista. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:263`, `:266`) |

Execução dos testes e estado de produção: **[não verificado]**. [nao verificado]

## 16/11 14:04 · sessao 8d94586da715 · BUG-PAR-O-PAR · construtor · origem=Code

Cura c2 escrita: contador e U103 consultam o carregador pelo módulo dono; o aviso preserva o contrato existente; `bloqueada_competencia` entrou em `ESTADOS`. ([materializacao.py:259](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:259), [materializacao.py:313](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:313), [validacao.py:58](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/validacao.py:58), [validacao.py:696](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/validacao.py:696))

Na tabela, **D = defeito da cura**. Nenhuma das 13 exigiu inverter a expectativa antiga; a análise individual, com linhas da BASE e da c2, está em [BUG_PAR_C2.md:9](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2.md:9).

| # | Falha | Causa | Classe | Cura — arquivo:linha |
|---|---|---|---|---|
| 1 | Contador das recusadas / IndexError | Loader capturado ignora U60 injetada. | D | `chamados/services/materializacao.py:313` |
| 2 | Aviso das irmãs | `outra(s)` rompe o contrato `1 pergunta(s)`. | D | `chamados/services/validacao.py:696` |
| 3 | K6 falso / retry | Elegibilidade ignora U103 injetada; porta retorna antes do ato. | D | `chamados/services/materializacao.py:259` |
| 4 | 103 elegível no contador | Recorte ignora U163 injetada. | D | `chamados/services/materializacao.py:313` |
| 5 | Situação da irmã | Contador não percorre a PK da fixture. | D | `chamados/services/materializacao.py:313` |
| 6 | 103 devolvida espera | Elegibilidade impede alcançar a devolução. | D | `chamados/services/materializacao.py:259` |
| 7 | Laço / contador de espera | Escrita e contador enxergam bindings diferentes do loader. | D | `chamados/services/materializacao.py:313` |
| 8 | Recusa legítima | Contador ignora a PK declarada dentro163. | D | `chamados/services/materializacao.py:313` |
| 9 | Rótulo 103 no modal | Retorno precoce impede gerar DEVOLUÇÃO. | D | `chamados/services/materializacao.py:259` |
| 10 | Foto com `materializada_por` | Retorno precoce impede gerar foto/evento. | D | `chamados/services/materializacao.py:259` |
| 11 | Trilha não gravada desfaz | Retorno precoce impede alcançar a verificação da trilha. | D | `chamados/services/materializacao.py:259` |
| 12 | Universo/contador F1–F2 | Contador usa o loader capturado e retorna total zero. | D | `chamados/services/materializacao.py:313` |
| 13 | Vocabulário | Estado devolvido pela porta não estava em `ESTADOS`. | D | `chamados/services/validacao.py:58` |

As fixtures deixaram de semear `recusa_motivo`; texto e instante continuam escritos juntos. A catraca registra o deslocamento da mesma consulta de 403 para 404. ([fixture:319](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/tests/test_bug_par_projetado.py:319), [catraca:271](/home/ronald/wt-codex-bug-par-o-par-131207/app/core/tests/test_medida_turno_do_par.py:271))

Resultados locais observados:

- **RED sobre a BASE:** `Ran 5 tests` / `FAILED (failures=5)`. [Registro:66](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:66)
- **Provas sem banco:** `Ran 14 tests` / `OK`. [Registro:102](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:102)
- **Cinco mutantes c2:** mortos por asserção, sem erros. [Registro:107](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:107)
- **Catraca adaptada em memória:** `Ran 35 tests` / `OK`. [Registro:794](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:794)

As tentativas com `--settings=config.settings.ci` pararam antes da descoberta por ausência de `django.core`. Não emitiram `Ran N` nem veredito de testes: [nao verificado]

| Módulo | Resultado |
|---|---|
| `chamados.tests.test_bug_par_projetado` | **[nao rodado]** — [registro:695](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:695) |
| `chamados.tests.test_bug29_30_log_validacao` | **[nao rodado]** — [registro:711](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:711) |
| `chamados.tests.test_bug_par_103` | **[nao rodado]** — [registro:727](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:727) |
| `chamados.tests.test_bug_par_volta` | **[nao rodado]** — [registro:743](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:743) |
| `chamados.tests.test_porta_validacao_pergunta` | **[nao rodado]** — [registro:759](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:759) |
| `core.tests.test_medida_turno_do_par` | **[nao rodado]** via CI — [registro:775](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2_VERIFICACOES.txt:775) |

Comandos dos REDs, dos mutantes e de cada módulo CI estão completos em [BUG_PAR_C2.md:36](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2.md:36). Execução conjunta dos cinco mutantes:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.bug_par_c2_mutantes --mutante todos
```

Arquivos tocados: `materializacao.py`, `validacao.py`, `test_bug_par_projetado.py`, `test_medida_turno_do_par.py`, os dois arquivos novos de provas/mutantes c2 e três documentos. A lista com caminhos e linhas está em [BUG_PAR_C2.md:91](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2.md:91).

A matriz **QUEM ESCREVE / QUEM LÊ**, por campo e função alcançados pelo diff, está em [BUG_PAR_C2.md:72](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/BUG_PAR_C2.md:72). Resultado integrado dos 13 casos e certificação Code/opus final: **[nao verificado]**.

LEI-AKITA: origem=universo congelado e porta canonica do carregador; testemunha=estado_aplicacao_pergunta e competencia_entregue existentes; RED=cinco provas c2 e mutantes em memoria; quem-mais-le=docs/BUG_PAR_C2.md:72; juizes novos=0; padrao=Single Writer Principle.

## 16/11 14:04 · sessao d2148e39a772 · - · - · origem=Ronald (tmux)

Complemento de amplitude:

| Caminho | Alcança célula-dia futura além da competência corrente? | Evidência |
|---|---|---|
| **Espelho do admin (`espelho_do_colab`)** | A janela visual normal termina em hoje: autoridade e montagem dos dias recebem `hoje` como fim. Há uma leitura auxiliar da semana seg–dom no cabeçalho para escala semanal, que pode alcançar datas futuras da semana. | `app/ponto/services/espelho.py:1225`, `app/ponto/services/espelho.py:1227`, `app/ponto/services/espelho.py:1262`, `app/ponto/services/espelho.py:222`, `app/ponto/services/espelho.py:224` |
| **Cartão/PDF espelho** | Não imprime linhas futuras: chama o espelho com ontem e recorta suas linhas ao período. A coleta interna usa `max(data_ini, ate_ontem(...))`; se todo período pedido for futuro, pode ainda consultar a primeira data futura internamente, sem imprimi-la no conjunto de dias. | `app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:354`, `app/relatorios/pdf_espelho.py:598`, `app/relatorios/pdf_espelho.py:602` |
| **`cartao_pela_celula`** | O helper `orfas_e_isencao_da_ata` aceita qualquer intervalo fornecido, inclusive futuro, e lê ata; não decide trabalho/folga. A parcela “dias em aberto” recebe teto de ontem, mesmo quando o período pedido é futuro. | `app/relatorios/cartao_pela_celula.py:277`, `app/relatorios/cartao_pela_celula.py:286`, `app/relatorios/cartao_pela_celula.py:288`, `app/relatorios/cartao_pela_celula.py:37`, `app/relatorios/cartao_pela_celula.py:41`, `app/relatorios/cartao_pela_celula.py:413`, `app/relatorios/cartao_pela_celula.py:419` |
| **`api/views_core.py`, bloco escala de `/api/me/`** | Só hoje: o endpoint calcula a data local atual e passa a `_escala_de_hoje`; True publica `trabalha_hoje=True` com horários do template. | `app/api/views_core.py:281`, `app/api/views_core.py:282`, `app/api/views_core.py:354`, `app/api/views_core.py:248`, `app/api/views_core.py:249`, `app/api/views_core.py:257` |
| **Painel operacional admin** | Lê células de ontem e hoje; previsão é de hoje e “previsto agora” compara os marcos desses dois dias ao instante atual. Não alcança a unidade futura além da competência por essa janela. | `app/core/services/painel_op.py:136`, `app/core/services/painel_op.py:137`, `app/core/services/painel_op.py:149`, `app/core/services/painel_op.py:151`, `app/core/services/painel_op.py:156`, `app/core/services/painel_op.py:231` |
| **Painel situacional `core/views.py`: ausentes** | Só células de hoje; True sem entrada inclui o colaborador em `ausentes_hoje`. | `app/core/views.py:396`, `app/core/views.py:397`, `app/core/views.py:399`, `app/core/views.py:404`, `app/core/views.py:405` |
| **Prévia de fase do cadastro** | Aceita mês/ano futuros, mas a projeção do vínculo proposto vem do template + âncora, não da célula futura existente; a célula é lida apenas para datas anteriores a `desde`, ou para a pergunta “mesma fase” na âncora. | `app/colaboradores/views_fase.py:94`, `app/colaboradores/views_fase.py:95`, `app/colaboradores/views_fase.py:98`, `app/colaboradores/views_fase.py:101`, `app/escala/services/fase_na_tela.py:191`, `app/escala/services/fase_na_tela.py:194`, `app/escala/services/fase_na_tela.py:195`, `app/escala/services/fase_na_tela.py:196` |
| **Sugestão de vigência da fase** | Procura datas até hoje+28, porém calcula fase pelo template; batidas só até hoje. Não lê célula futura nesse cálculo. | `app/escala/services/fase_na_tela.py:19`, `app/escala/services/fase_na_tela.py:109`, `app/escala/services/fase_na_tela.py:110`, `app/escala/services/fase_na_tela.py:112`, `app/escala/services/fase_na_tela.py:113` |
| **Cadastro × Realidade: leitores diretos abertos** | C2 consulta células até hoje; C5 “nunca bateu”, C4 “afastado sem registro” e C11 “folga não definida sem porta” terminam em ontem. Não detectam por essas consultas o resíduo da unidade futura além da competência corrente. | `app/escala/services/cadastro_realidade.py:224`, `app/escala/services/cadastro_realidade.py:226`, `app/escala/services/cadastro_realidade.py:233`, `app/escala/services/cadastro_realidade.py:166`, `app/escala/services/cadastro_realidade.py:167`, `app/escala/services/cadastro_realidade.py:603`, `app/escala/services/cadastro_realidade.py:604`, `app/escala/services/cadastro_realidade.py:553`, `app/escala/services/cadastro_realidade.py:555` |

Não expandi as fontes de lavra de todas as assinaturas de Cadastro × Realidade; o alcance temporal integral dessas fontes é **[não verificado]**. A C10 aqui apenas lê `ultimo_lavrado(hoje)`, e a lista agrega também C7/C8/C9/C12 por outras fontes; logo não usar esta amostra para afirmar que a lista inteira nunca recebe datas futuras. `app/escala/services/cadastro_realidade.py:393`, `app/escala/services/cadastro_realidade.py:843`, `app/escala/services/cadastro_realidade.py:857`

## 16/11 14:04 · sessao a2fc8078c176 · - · - · origem=Ronald (tmux)

Q2: **REPROVA o censo de carregadores.** O teste só percorre `templates/` e `static/`, em `.html`/`.js`; não acompanha links produzidos em Python (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:278-284`).

Existe um carregador real omitido:

1. O mapa oferece `colaboradores:painel_vinculo` para tipos `escala`, `folga` e `sem_escala` (`app/folha/retrato_folha.py:1004-1006`).
2. `portas_da_linha` resolve essa rota por `reverse` e publica `href` (`app/folha/retrato_folha.py:1066-1075`).
3. A view entrega `retrato_portas` ao painel do colaborador, condicionada a competência, permissão e retrato existente (`app/colaboradores/views.py:996-1016`).
4. O painel inclui `_retrato_o_que_fazer.html`, cujo botão chama `window.abrirDrawer(..., p.href, ...)` (`app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/folha/_retrato_o_que_fazer.html:73`).
5. `abrirDrawer` pede a rota por `fetch`, injeta HTML e processa HTMX (`app/templates/core/_drawer_generico.html:19-33`).

Há entrada pela lista do retrato e pelo Raio-X (`app/templates/folha/_retrato_lista.html:61`; `app/templates/folha/_retrato_raiox.html:68-70`). Logo, o comentário “único carregador” e a lista esperada não representam o grafo real (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:271-285`).

A regex captura o nome `painel_vinculo` terminado por aspa simples e uma concatenação específica com variável `\w+`; não cobre nome com aspas duplas, template literal nem todas as strings montadas (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:276`).

As duas cascas incluem o drawer (`app/templates/base.html:399`; `app/templates/base_app.html:154`). Um fluxo efetivamente alcançável de produto na `base_app` para esse botão: **[não verificado]**; existência de registros/permissões em produção para acionar o caminho: **[não verificado]**.

## 16/11 14:04 · sessao 18084491e621 · - · - · origem=Ronald (tmux)

- **Percorre `carregarPainel` real:** a sonda chama a função diretamente; o HTML vem da view pelo `Client`, e a função executa fetch → wrapper → scripts → `htmx.process`. `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:83`, `:215`, `:240`; `app/templates/colaboradores/vincular.html:143`.
- **Pode ficar verde com clique quebrado:** o teste pula o `onclick` da linha e `abrirPainelVinculo`, que também escreve `_painelAtualCid`. `app/templates/colaboradores/partials/vinculo_row.html:4`; `app/templates/colaboradores/vincular.html:136`; teste `:216`.
- **Pode ficar verde sem processamento HTMX:** a alternância usa `onchange` nativo, e a sonda não envia o formulário; remover `htmx.process` não é diretamente observado pelas asserções. `app/templates/colaboradores/partials/painel_vinculo.html:191`, `:196`; `app/templates/colaboradores/vincular.html:161`; teste `:221`.
- **Visibilidade incompleta:** conta `display` e área, sem exigir `visibility`, `opacity`, ausência de recorte ou oclusão. `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:207`.
- **CSS não comprovado:** o helper aponta para `STATIC_ROOT`, sem validar recursos carregados; só recolhe linhas `:ERROR:CONSOLE`. `app/core/tests/test_smoke_chromium.py:93`, `:124`.
- **MORDE é válido para a alternância:** remove exatamente um `onchange` e exige o calendário inicial repetido nas três medições, resultado incompatível com a asserção principal. `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:243`, `:261`, `:265`. Execução efetiva: [não verificado].

## 16/11 14:03 · sessao 2675e78cc9e0 · - · - · origem=Ronald (tmux)

Censo de perguntas, cobranças e crons:

- **Célula futura `trabalha=True` não gera automaticamente pergunta de falta:** a fábrica chama `dia_sem_trabalho` e retorna `[]` para `cel.data > hoje`; antes de perguntar exige marcos vencidos por instante e ata com faltantes. Fontes: `app/chamados/services/fabrica_da_celula.py:110-121`, `app/chamados/services/fabrica_da_celula.py:52-73`.
- **Cartório avulso recusa futuro:** `julgar_celula` retorna `None` se `data > hoje`; para hoje com `trabalha is not False`, sem `teto_de_hoje`, espera fim do turno. Fontes: `app/ponto/services/cartorio.py:2025-2038`.
- **Cartório diário não lê futuro:** as duas consultas do lote usam `data__lt=hoje`; lê as células passadas e delega para `julgar_colab`. Fonte: `app/ponto/management/commands/processar_cartorio.py:49-65`.
- **Vigia de hora não alcança futuro:** candidatas são células `trabalha=True` de hoje e ontem; ontem precisa estar na janela do turno; sem marco vencido pula; julga com `teto_de_hoje=True`. Fontes: `app/ponto/services/vigia_de_hora.py:20-23`, `app/ponto/services/vigia_de_hora.py:123-134`.
- **`detectar_ausencias` examina hoje e ontem:** lê `ec.eh_dia_trabalho(data_ref)`; não há futura na lista de passadas. Fonte: `app/ponto/management/commands/detectar_ausencias.py:137-145`.
- **Vigia de perguntas da competência só conta passado:** consulta `data__lt=hoje`; aplica `motivos_que_o_evento_deixou` e exclui ontem ainda na janela do vigia. Fonte: `app/chamados/management/commands/disparar_perguntas_competencia.py:177-189`.
- **Vigia “furos sem cobrança” só chega até ontem:** exige veredito `furo`, protesto que cobra, início da competência corrente até ontem, colaborador ativo e empresa em rollout. Fonte: `app/ponto/services/furos_sem_cobranca.py:53-63`.
- **Vigia de cadastro futuro alcança futuras da janela passada pelo gerador:** filtra `data__gt=hoje, data__range=(ini,fim)`; compara `trabalha` e DNA com o cadastro da própria geradora; não escreve células. Fontes: `app/ponto/services/vigia_cadastro_futuro.py:24-31`, `app/ponto/services/vigia_cadastro_futuro.py:75-87`. Na execução padrão `gerar_celulas`, a medida recebe `_janela_default(hoje)` e ocorre antes da regeneração de hoje e geração; portanto o resíduo além dessa janela não entra na medida padrão. Fonte: `app/ponto/management/commands/gerar_celulas.py:337-353`.

**O que `trabalha=True` significa para os juízes de cobrança:**

- `dia_sem_trabalho` não consulta FolgaDia: retorna verdadeiro com `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de `trabalho`; ausência parcial mantém cobrança do resto, exceto célula `False`. Portanto `True` + ata ausente não constitui dispensa nesse juiz. Fonte: `app/chamados/juizes.py:445-460`.
- O gate `celula_veta_cobranca` também não consulta FolgaDia: lê apenas `trabalha` e `veredito`; `False` veta como `FOLGA`; `True` depende dos vereditos de veto. Fonte: `app/ponto/marcos.py:154-163`.
- `_celula_furada` lê a célula/ata; com `True` e sem `lampadas` devolve `None` (“não sei”), não `True` (“furada”). Fonte: `app/chamados/juizes.py:491-510`.
- A tarefa do dia consome `grade_da_celula`; sem dispensa e sem instâncias, se nenhum marco venceu retorna `nao_venceu`; sem lâmpadas e sem essa confirmação retorna `nao_sei=True, ata_sem_lampadas`; instância não vencida também retorna `nao_venceu`. Fontes: `app/chamados/services/tarefa_do_dia.py:194-203`, `app/chamados/services/tarefa_do_dia.py:230-240`, `app/chamados/services/tarefa_do_dia.py:282-308`.
- O app consulta perguntas existentes e filtra pelo C6 `pergunta_viva`; divide pendentes por `respondida_em is None` e disputa aberta; badge “Responder DP” conta essas pendentes. Não deriva uma pergunta de `trabalha=True`. Fontes: `app/chamados/services/pergunta_no_app.py:45-61`, `app/chamados/services/pergunta_no_app.py:64-85`, `app/chamados/services/pergunta_no_app.py:100-119`.
- A cobrança manual usa exatamente `consulta_do_app(colaborador)['pendentes']`; permite cobrar quando existem pendentes e nenhum pai tem prazo SLA futuro. Fontes: `app/chamados/services/cobranca.py:45-48`, `app/chamados/services/cobranca.py:75-94`.
- **Limite da conclusão:** o C6 não possui uma recusa geral por `data_evento > hoje` no corpo aberto; pergunta existente sem materialização pode retornar viva. Logo não afirmar que uma pergunta futura preexistente seria necessariamente invisível; existência de tal pergunta no dado fornecido: `[não verificado]`. Fontes: `app/chamados/juizes.py:1403-1495`, especialmente `:1482-1483`.

**Divergência verificável na leitura de FolgaDia:**

- `fatos_do_periodo` lê a célula `True` como `previsto=True`, mas também lê FolgaDia válida para o vínculo do dia; adiciona **folga e trabalho** e escolhe folga pela precedência. Assim pode devolver `era_dia_previsto=True`, `tipo_do_dia='folga'` e trabalho suprimido para a mesma célula. Fontes: `app/ponto/precedencia.py:348-363`, `app/ponto/precedencia.py:379-413`; ordem em `app/ponto/precedencia.py:46`.
- Folga de calendário só vale no vínculo do dia; a seleção percorre todas as FolgaDia da pessoa por dia, não apenas uma linha. Fontes: `app/ponto/precedencia.py:194-214` e `app/ponto/precedencia.py:350-356`.
- O vigia “faltas de hoje sem cobrança” começa por `eh_dia_trabalho(hoje)` e DNA, mas exclui o dia se `fatos_do_dia.tipo_do_dia` não for `trabalho`/`None`; portanto uma FolgaDia válida pode excluí-lo apesar da célula `True`. Fonte: `app/ponto/services/faltas_de_hoje.py:39-62`.
- O funil emissor de falta consulta precedência apenas para veto de **feriado**, depois consulta o gate da célula; não há veto FolgaDia nessa parte do funil. Fonte: `app/ponto/management/commands/detectar_ausencias.py:607-634`.
- `_protesta` do cartório continua usando `trabalha is not False` e marcos não acesos, enquanto a classificação recebe o juiz de dia sem trabalho com ata do julgamento atual. Fontes: `app/ponto/services/cartorio.py:1458-1468`.
- A precedência é lavrada **depois** do julgamento avulso e o comentário declara que descreve o dia, não decide o veredito do cartório. Fonte: `app/ponto/services/cartorio.py:2043-2048`.

**Crons configurados no código, execução efetiva em produção `[não verificado]`:**

- `detectar_ausencias` a cada 5 min; `vigia_de_hora --apply` a cada 10 min: `app/config/crons.py:364-367`.
- `lavrar_foto_folgas --apply` 05:47; `gerar_celulas` 05:50 dependente da foto: `app/config/crons.py:425-426`.
- `reconciliar_grade` 06:20, sem `--apply`; comentário o mantém como sombra: `app/config/crons.py:460-462`.
- `processar_cartorio --apply` 06:27, por empresa: `app/config/crons.py:614-616`.
- `disparar_perguntas_competencia --comp-corrente --alarme` encadeado após apurar furos: `app/config/crons.py:1539`.
- `reavaliar_ausencias_feriado --alarme` 07:02: `app/config/crons.py:608`; comando só mede chamados abertos que precedência considera feriado/folga e alarma, não relavra célula: `app/ponto/management/commands/reavaliar_ausencias_feriado.py:27-45`.

**DECISÕES atuais para consolidação, sem propor lei:**

1. Quem decide dispensa prevista: célula/DNA para gates de cobrança versus FolgaDia válida + precedência para fato vencedor; a mesma combinação `True` + FolgaDia pode ser “previsto trabalho” e “fato folga”. Fontes: `app/ponto/marcos.py:154-163`, `app/ponto/precedencia.py:348-413`.
2. Quem decide quando nasce cobrança/pergunta: relógio do cartório/fábrica, com recusa de futuro e vencimento de marco; `True` sozinho não autoriza perguntar antes do fato. Fontes: `app/ponto/services/cartorio.py:2025-2038`, `app/chamados/services/fabrica_da_celula.py:110-121`.
3. Quem decide o universo do alarme de cadastro futuro: janela do gerador, não todo acervo futuro já existente; além da competência padrão fica fora. Fontes: `app/ponto/management/commands/gerar_celulas.py:344-346`, `app/ponto/services/vigia_cadastro_futuro.py:31`.
4. Quem decide se FolgaDia cala cobrança: o contador de falta de hoje consulta precedência completa; gate emissor consulta só feriado e célula; são portas diferentes com critérios observavelmente distintos. Fontes: `app/ponto/services/faltas_de_hoje.py:61-62`, `app/ponto/management/commands/detectar_ausencias.py:616-634`.

## 16/11 14:03 · sessao d2148e39a772 · - · - · origem=Ronald (tmux)

Censo dos leitores de tela, somente pelo código aberto:

| Leitor | Janela | Resultado para célula `trabalha=True` em dia com FolgaDia |
|---|---|---|
| **Minha Escala HTML** | Hoje e próximos 13 dias. `app/escala/views.py:38` | Lê `EC.eh_dia_trabalho`, que retorna a célula antes da foto; mostra trabalho verde com horários do DNA da célula. `app/escala/views.py:40`, `app/escala/views.py:43`, `app/templates/escala/minha_escala.html:66`, `app/templates/escala/minha_escala.html:76` |
| **API escala/v2 do colaborador** | Hoje e próximos 13 dias. `app/api/views.py:918` | Publica `trabalho=True`; horários vêm **do template**, por `te.marcos_do_dia`, não do DNA. `app/api/views.py:924`, `app/api/views.py:929`, `app/api/views.py:940` |
| **API espelho/v2** | Mês civil solicitado, inclusive futuro. `app/api/views.py:1344`, `app/api/views.py:1347` | No dia sem card/batida, consulta a célula para horários, mas usa D4 para status: pode publicar **`status='folga'` e horários previstos preenchidos ao mesmo tempo**, se a FolgaDia for válida e o dia chegar ao ramo sem lavra do D4. `app/api/views.py:1587`, `app/api/views.py:1589`, `app/api/views.py:1591`, `app/api/views.py:1611`, `app/api/views.py:1615`, `app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:312` |
| **Calendário de ponto do colaborador, exibido ao gestor/admin** | Mês, semana, quinzena, competência ou custom; custom aceita futuro e limita intervalo a 366 dias. `app/colaboradores/services/calendario.py:7`, `app/colaboradores/services/calendario.py:227`, `app/colaboradores/services/calendario.py:234`, `app/colaboradores/services/calendario.py:239` | Lê D4, não usa diretamente `eh_dia_trabalho` para a palavra do dia; em futuro sem batida/lavra com FolgaDia válida, mostra **folga**, apesar da célula True. `app/colaboradores/services/calendario.py:634`, `app/colaboradores/services/calendario.py:638`, `app/colaboradores/services/calendario.py:695`, `app/templates/colaboradores/partials/_calendario_grade.html:106`, `app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:312` |
| **Drawer do colaborador** | Competência anterior e corrente até o fim da corrente, respeitando piso visual; não alcança além do fim da corrente por essa rota. `app/colaboradores/services/drawer.py:115`, `app/colaboradores/services/drawer.py:116`, `app/colaboradores/services/drawer.py:123` | Usa o mesmo `contexto_calendario`, logo o mesmo D4. `app/colaboradores/services/drawer.py:122` |
| **Painel do vínculo: agenda** | Hoje e próximos 13 dias carregados; template mostra os primeiros sete. `app/colaboradores/views.py:280`, `app/colaboradores/views.py:285`, `app/colaboradores/views.py:287`, `app/templates/colaboradores/partials/painel_vinculo.html:217` | True vira trabalho/horário do DNA; havendo AgendaDia, seu conteúdo tem prioridade visual. `app/colaboradores/views.py:289`, `app/colaboradores/views.py:292`, `app/templates/colaboradores/partials/painel_vinculo.html:218`, `app/templates/colaboradores/partials/painel_vinculo.html:230` |
| **Modal da agenda do admin** | Semana navegável por parâmetro, inclusive futura. `app/colaboradores/views.py:2239`, `app/colaboradores/views.py:2250` | Mesmo True e horários DNA; AgendaDia tem prioridade; sem agenda, mostra horário/“Trabalho”. `app/colaboradores/views.py:2256`, `app/colaboradores/views.py:2265`, `app/colaboradores/views.py:2266`, `app/templates/colaboradores/partials/agenda_modal.html:97`, `app/templates/colaboradores/partials/agenda_modal.html:123` |
| **Plano de Folgas do admin** | Mês solicitado, inclusive futuro. `app/escala/views_plano_folgas.py:33`, `app/escala/views_plano_folgas.py:52` | A FolgaDia gravada ganha antes da leitura da célula: mostra seu tipo de folga como gravado; só sem FolgaDia consulta `eh_dia_trabalho`. `app/escala/views_plano_folgas.py:60`, `app/escala/views_plano_folgas.py:70`, `app/escala/views_plano_folgas.py:71`, `app/escala/views_plano_folgas.py:76`, `app/templates/escala/plano_folgas.html:81` |
| **Disponibilidade: voltando de folga** | Hoje até hoje+7. `app/colaboradores/views.py:2903`, `app/colaboradores/views.py:2905` | Exige célula de hoje False; primeiro futuro True vira data de retorno, sem consulta a FolgaDia nessa decisão. `app/colaboradores/views.py:2912`, `app/colaboradores/views.py:2914`, `app/colaboradores/views.py:2916`, `app/colaboradores/views.py:2917` |
| **Semana do posto / mensageria** | Semana recebida por `inicio`; grade consulta célula em toda a semana. `app/escala/services/semana_do_posto.py:126`, `app/escala/services/semana_do_posto.py:132`, `app/escala/services/semana_do_posto.py:85` | Publica `previsto=True`; se não bateu, pergunta a A2 e FolgaDia válida vira `explicado_por='folga'`, não buraco. `app/escala/services/semana_do_posto.py:143`, `app/escala/services/semana_do_posto.py:151`, `app/escala/services/semana_do_posto.py:152`, `app/escala/services/semana_do_posto.py:157` |

Autoridades que explicam as diferenças:

- **`EC.eh_dia_trabalho` ignora a FolgaDia quando a célula existe:** devolve `cel.trabalha` imediatamente; a foto só participa da aritmética de fallback quando não há célula. `app/escala/models.py:1092`, `app/escala/models.py:1094`, `app/escala/models.py:1096`
- **`EC.marcos_do_dia` lê DNA congelado:** usa `dna.marcos` quando preenchido; sem marcos no DNA, cai no template. `app/escala/models.py:1124`, `app/escala/models.py:1125`, `app/escala/models.py:1131`, `app/escala/models.py:1133`
- **A2 pode dizer folga e manter `era_dia_previsto=True`:** lê o True da célula como previsto, admite a FolgaDia válida como fato, acrescenta trabalho também e ordena pela precedência. `app/ponto/precedencia.py:356`, `app/ponto/precedencia.py:357`, `app/ponto/precedencia.py:363`, `app/ponto/precedencia.py:384`, `app/ponto/precedencia.py:386`, `app/ponto/precedencia.py:396`, `app/ponto/precedencia.py:407`
- **FolgaDia de calendário precisa ser do vínculo do dia; manual vale para a pessoa:** esses são os predicados que determinam “válida” nas linhas anteriores. `app/ponto/precedencia.py:235`, `app/ponto/precedencia.py:237`, `app/ponto/precedencia.py:239`
- **D4 não equivale ao booleano de previsão:** sem batida e sem status lavrado aplicável, faixa A2 `folga` vira status `folga`; com lavra e dia não retido pelo relógio, usa o status persistido. `app/ponto/status_dia.py:286`, `app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:296`, `app/ponto/status_dia.py:312`
- **Grade também aplica overlay FolgaDia válida:** transforma `tipo_dia='folga'`, limpa marcos e órfãs mesmo após montar por célula True; aplicação acontece no fim do builder. `app/escala/utils.py:1128`, `app/escala/utils.py:1149`, `app/escala/utils.py:1151`, `app/escala/utils.py:1152`, `app/escala/utils.py:1844`

Consequência para a redação do censo: a frase raspada “colaborador e admin veem trabalho em dia de folga futura” **não pode ser universalizada para todas as telas desta cópia**, porque Minha Escala/painel de vínculo seguem o True, enquanto calendário D4 e Plano de Folgas seguem a folga válida nos ramos descritos. `app/escala/views.py:40`, `app/colaboradores/views.py:289`, `app/colaboradores/services/calendario.py:638`, `app/ponto/status_dia.py:312`, `app/escala/views_plano_folgas.py:71`

Renderização efetiva do app nativo e telas observadas em produção: **[não verificado]**; aqui há prova do payload das APIs e dos templates HTML citados. `app/api/views.py:935`, `app/api/views.py:1595`, `app/templates/escala/minha_escala.html:62`, `app/templates/colaboradores/partials/_calendario_grade.html:106`

DECISÕES constatáveis para consolidação: horizonte da previsão versus horizonte da atualização pertence aos escritores (outro agente); booleano da célula versus fato FolgaDia na palavra da tela é decidido hoje por `EC.eh_dia_trabalho` versus A2/D4; horários DNA versus template são decididos pelo leitor HTML versus APIs. `app/escala/models.py:1094`, `app/ponto/precedencia.py:384`, `app/ponto/status_dia.py:312`, `app/escala/views.py:43`, `app/api/views.py:929`, `app/api/views.py:1589`

## 16/11 14:02 · sessao 4319b5e98d3c · - · - · origem=Ronald (tmux)

(b) O `gerar_celulas` sem argumentos cria as ausentes entre `min(inícios)` e `max(fins)` das competências correntes de todas as empresas ativas; sem empresa ativa, usa o default do juiz. Portanto, seu horizonte padrão **não é a competência seguinte**, mas a união das correntes. (`app/ponto/management/commands/gerar_celulas.py:290-300`, `:337-350`)

- A data concreta do fim em produção não pode ser extraída sem os cortes das empresas ativas. Com corte 21, a competência que contém 16/11/2026 termina em **20/11/2026**; a aplicação dessa hipótese à produção é [não verificado]. (`app/ponto/janelas.py:10-33`, `:36-63`)
- A CLI aceita `--ini` e `--fim` explícitos, substituindo o default e permitindo criar além da competência corrente. (`app/ponto/management/commands/gerar_celulas.py:332-344`)
- A criação depende de vínculo que cubra a data; o inativo com fim declarado entra somente em dia futuro em que o juiz o nomeia dono. (`app/ponto/portas/celula.py:883-917`)

Os três pontos de criação encontrados no código de produção são:

| Criador | Horizonte/condição |
|---|---|
| `regenerar_celulas_vinculo` | Percorre `[desde, ate]`, cria ausentes e reescreve geradas divergentes; a extensão até o fim da competência apenas aumenta `ate`, nunca reduz horizonte solicitado maior. (`app/ponto/portas/celula.py:112-119`, `:178-199`, `:226`, `:289-311`) |
| `editar_dia` | Cria célula humana para a data solicitada, se existe vínculo responsável e passam autor, motivo e guarda de competência lavrada; não tem limite superior pela competência corrente. (`app/ponto/portas/celula.py:572-588`, `:597-639`) |
| `gerar_celulas_periodo` | Cria ausentes em todo `[ini,fim]` recebido. (`app/ponto/portas/celula.py:863-886`, `:901-918`) |

O censo executado da própria cópia, `core/censo_escritas.py::varrer_arvore('app')`, também retornou somente esses três `create` atribuídos a `CelulaDia`; seu algoritmo considera construtores, gerentes, apelidos/inferências e exclui testes/migrações, tendo as limitações documentadas para escrita não atribuível. (`app/core/censo_escritas.py:6-55`, `:61-82`; pontos encontrados em `app/ponto/portas/celula.py:293`, `:638`, `:914`)

Um caminho concreto além da competência é ESCALA-AUTO: seu início padrão é o primeiro dia da próxima competência; `_ate` retorna pelo menos esse início ou a última célula já gerada, e a aplicação chama regeneração explícita que cria ausentes. O desfazer também regenera até a última célula existente. Isso demonstra uma origem possível, **não demonstra que criou as células da sonda**. (`app/escala/services/escala_auto_executor.py:24-31`, `:52-58`, `:87-108`, `:143-148`; origem das células da sonda [não verificado])

(c) **Não existe relavra geral de células futuras existentes disparada pela virada de competência no fluxo agendado examinado**: o cron chama `gerar_celulas` diariamente; `_regenerar_hoje_existentes` seleciona somente `data=hoje` e regenera somente `[hoje,hoje]`; o gerador pula as geradas existentes de hoje em diante. (`app/config/crons.py:425-429`, `app/ponto/management/commands/gerar_celulas.py:303-325`, `app/ponto/portas/celula.py:919-935`)

A mudança da janela pode fazer a célula entrar na medição, mas essa medição não a relavra: `medir` filtra futuras na janela, conta divergências, e `lavrar` escreve `MetricaSnapshot`. (`app/ponto/services/vigia_cadastro_futuro.py:24-38`, `:75-87`, `:99-108`)

Há uma ressalva: o cron de foto pode provocar uma relavra futura **como efeito de gravar foto faltante**, não como varredura geral na virada; ele pula vínculo-mês sem `falta`, chama `gerar_foto_folgas` apenas nos alvos e essa escrita chama a relavra. Se a foto já está completa, esse gatilho não existe. (`app/escala/management/commands/lavrar_foto_folgas.py:123-129`, `:163-170`, `app/escala/folgas.py:213-218`, `:298-299`)

(d) `lavrar_foto_folgas` pergunta a `_janela_default(hoje)` e novamente no dia posterior ao fim, cobre os meses civis até o maior fim e assim inclui corrente + seguinte. O motivo registrado é antecipar a foto e tornar o template vazio de novembro visível ainda em outubro, enquanto há tempo para cadastrar; a foto deve chegar antes da célula. (`app/escala/management/commands/lavrar_foto_folgas.py:59-82`, `:24-29`)

O cron está declarado às 05:47 com `--apply`; o gerador vem às 05:50 e depende dele. (`app/config/crons.py:425-426`)

A lavra de foto é aditiva: bloqueia quando há `sobra`, quando não falta foto ou quando a competência está fechada; seu corpo reconsulta a divergência antes de gravar. (`app/escala/management/commands/lavrar_foto_folgas.py:119-129`, `:163-170`)

O que impede tecnicamente a mesma abrangência no suplemento é o seu próprio filtro: ele faz **uma** chamada a `_janela_default(hoje)`, limita o início a amanhã e seleciona existentes apenas em `data__range=(ini,fim)`; os dias selecionados são enviados à porta com `estender_horizonte=False`. (`app/ponto/services/cadastro_futuro.py:26-45`)

O tratamento de feriado também corta as datas na corrente antes de delegar ao mesmo suplemento. (`app/ponto/services/cadastro_futuro.py:57-60`, `:76-78`)

Não há, nesses corpos, recusa técnica genérica da competência seguinte: a porta de regeneração aceita `ate` maior e não o encurta; a janela mais curta nasce no seletor de cadastro. Eventual proibição normativa adicional de usar corrente + seguinte em cadastro_futuro é [não verificado]. (`app/ponto/portas/celula.py:178-199`, `app/ponto/services/cadastro_futuro.py:26-45`)

Outro descompasso verificável é que a sincronização de calendário seleciona células apenas na corrente, mas materializa a foto futura **até o fim dos meses civis envolvidos**; o comentário declara que a foto mensal pode precisar do dia 22 para definir a fase do dia 13. A relavra final continua limitada ao conjunto de células selecionado. (`app/escala/folgas.py:619-642`)

## 16/11 13:56 · sessao f6c24406d92e · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**VEREDITO: REPROVA.** P1/P2 ficaram parcialmente cobertos: o navegador testa o fragmento em um hospedeiro artificial, sem percorrer o carregamento real do painel. O selo pode permanecer verde com esse carregamento quebrado (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:247`; `app/templates/colaboradores/vincular.html:143`).

1. **P1/P2 — a alternância é medida, mas a integração real continua descoberta.** O teste confirma a casca renderizada, dispara `change` e compara o resultado; remover `onchange` produz o estado antigo. Há mordida para esse comportamento isolado (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:237`, `:201`, `:255`, `:262`).

   Porém, inserir o fragmento antes de `</body>` **não representa sua chegada real**:

   - Na página de vínculos, que estende `base.html`, ele chega por `fetch`, entra via `innerHTML` em `.vinc-painel`, tem scripts executados explicitamente e passa por `htmx.process` (`app/templates/colaboradores/vincular.html:1`, `:147`, `:150`, `:156`, `:161`).
   - No drawer, o destino é `#drawer-geral-corpo`; a abertura altera a transformação do ancestral, e o processamento HTMX acontece depois da inserção (`app/templates/core/_drawer_generico.html:8`, `:16`, `:26`, `:33`).
   - O teste usa apenas `<div id="uvc-painel">`, como HTML inicial. Não chama nenhum desses carregadores. Uma regressão que impeça `carregarPainel` de inserir o fragmento não interfere nessa medição (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:240`; `app/templates/colaboradores/vincular.html:143`).

   Os ancestrais também importam: a tela real possui largura limitada, `overflow:hidden` e área de rolagem; o drawer nasce deslocado para fora da tela. A sonda consulta somente `display` do próprio bloco, sem verificar abertura, área ou recorte por ancestrais. Portanto, “bloco com display” não certifica “painel visível e utilizável” (`app/templates/colaboradores/vincular.html:5`; `app/templates/core/_drawer_generico.html:3`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:194`).

2. **A prova da cascata também admite falso verde.** O helper religa os estáticos, mas o teste não confirma que o CSS carregou e descarta os erros retornados pelo navegador. Como a alternância usa `hidden` e um handler inline, o resultado esperado pode sobreviver à ausência da folha externa (`app/core/tests/test_smoke_chromium.py:93`, `:123`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:250`; `app/templates/colaboradores/partials/painel_vinculo.html:196`, `:205`). Carregamento efetivo nesta execução: [nao verificado].

3. **P3 — respondido no ponto levantado.** Exigir HTTP 200 e dois/sete blocos elimina o verde por duas respostas de erro iguais. A medição continua sendo quantidade de queries; bytes são apenas impressos, sem limite ou medição de tempo (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:174`, `:177`, `:179`).

4. **P4 — agora confere argumentos, mas não associa cada retorno à renderização.** A asserção exige que as duas chamadas corretas estejam presentes; permite chamadas adicionais. Como todas recebem a mesma sentinela, esse teste isolado aceita chamadas corretas descartadas e renderização alimentada por outra chamada. Os testes de datas acrescentam cobertura, mas a nova asserção não distingue os retornos por calendário/mês (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:150`, `:154`, `:157`, `:107`, `:114`).

Quanto aos descartes:

- **D1:** a linha citada confirma o problema semântico: vazio resolve o padrão do tipo no POST, enquanto a caixa vazia oculta todas as projeções. A fixture não configura padrão no `TipoEscala`, deixando descoberta a combinação “vazio com padrão não nulo” (`app/colaboradores/views.py:236`; `app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:55`). A restrição de alterar somente contexto está registrada; já a afirmação de que qualquer solução necessariamente duplicaria regra ou alteraria o POST não está demonstrada pelas linhas citadas (`app/docs/BACKLOG.md:820`; [nao verificado]).
- **D2:** manter os demais links dentro de blocos ocultos corresponde ao desenho implementado; isso não é, por si, link visível indevido. A alegação de aprovação literal depende do anexo, sem o documento original conferido (`app/templates/colaboradores/partials/painel_vinculo.html:196`, `:205`, `:207`; [nao verificado]).
- **D3:** a projeção reutiliza o mesmo `hoje` da agenda. O código configura São Paulo, mas isso não demonstra a igualdade incondicional alegada entre os dois relógios (`app/colaboradores/views.py:280`, `:302`; `app/config/settings/base.py:93`). Inicialização efetiva de `TZ` e igualdade em produção: [nao verificado].
- **D4:** motivo consistente: a fonte calcula a definição do calendário, e essa é a pergunta registrada para a obra; não pretende mostrar a foto individual (`app/escala/folgas.py:43`, `:54`, `:56`; `app/docs/BACKLOG.md:820`).
- **D5:** motivo consistente como descrição de snapshot: um novo GET recalcula agenda e projeção; trocar a caixa apenas alterna blocos já recebidos (`app/colaboradores/views.py:280`, `:294`, `:304`; `app/templates/colaboradores/partials/painel_vinculo.html:196`).
- **D6:** “custo constante” só se sustenta como quantidade de queries. Processamento e HTML crescem por calendário; o teste não limita esse crescimento (`app/colaboradores/views.py:299`, `:304`; `app/templates/colaboradores/partials/painel_vinculo.html:204`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:177`). A medição do log e a extrapolação fornecida não foram revalidadas: [nao verificado].

O mapa dos quatro valores é:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `folgas_projecao` | View consulta a fonte, monta e publica o contexto: `app/colaboradores/views.py:304`, `:308`. | Template percorre calendários, meses e datas: `app/templates/colaboradores/partials/painel_vinculo.html:204`, `:206`. |
| `pode_abrir_calendario` | View consulta `tem_acao(..., 'editar_escalas')`: `app/colaboradores/views.py:309`. | Template condiciona o link: `app/templates/colaboradores/partials/painel_vinculo.html:207`. Destino exige a mesma ação: `app/escala/views.py:315`. |
| `data-folgas-cal` | Template grava o PK: `app/templates/colaboradores/partials/painel_vinculo.html:205`. | Handler compara com a caixa e escreve `hidden`: `app/templates/colaboradores/partials/painel_vinculo.html:196`. Testes leem atributo/conteúdo/display: `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:35`, `:41`, `:194`. |
| `folga_calendario_id`, formulário | Template produz opções e seleção inicial; sonda altera `.value`: `app/templates/colaboradores/partials/painel_vinculo.html:195`, `:200`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:201`, `:203`. | Handler lê a seleção; POST lê o campo e resolve calendário ou padrão: `app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/views.py:230`, `:236`. |

O `folga_calendario_id` persistente pertence à FK nullable `EscalaColaborador.folga_calendario`, com `SET_NULL` (`app/escala/models.py:890`).

- **Escritores:** cópia do padrão ao vincular; troca explícita usada pela tela individual e pelo lote; criação de vínculos nos ajustes automáticos. A fixture também grava a FK diretamente (`app/colaboradores/services/vinculo.py:161`, `:284`, `:298`, `:461`, `:1092`, `:1113`; `app/colaboradores/views.py:239`; `app/escala/views.py:409`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:78`).
- **Leitores:** seleção/ocultação inicial, retorno do POST e trilha (`app/templates/colaboradores/partials/painel_vinculo.html:200`, `:205`; `app/colaboradores/views.py:237`, `:258`; `app/colaboradores/services/vinculo.py:42`, `:459`, `:471`).
- **Outros consumidores:** universo de vínculos, divergência, vigias, materialização e sincronização futura; lista de usuários do calendário pela relação reversa; diagnóstico de vínculos sem plano; comando de lavra (`app/escala/folgas.py:75`, `:99`, `:127`, `:157`, `:600`, `:627`; `app/escala/views.py:300`; `app/api/views_mensageria.py:1223`; `app/escala/management/commands/lavrar_foto_folgas.py:113`, `:170`).

Execução da suíte e resultados de produção: [nao verificado]. [nao verificado]

## 16/11 13:54 · sessao 1062dd8ea6bd · - · - · origem=Ronald (tmux)

D1–D6, com distinção entre código confirmado e justificativas históricas:

- **D1:** o fato citado está correto: vazio no POST resolve `tipo_escala.folga_calendario_padrao`, e passa o resultado ao escritor (`app/colaboradores/views.py:230`, `:236`, `:239`). A tela, por outro lado, associa vazio à opção “padrão do template” e oculta todos os blocos (`app/templates/colaboradores/partials/painel_vinculo.html:198`, `:205`). A fixture não define calendário padrão no tipo; portanto o caso “vazio com padrão não nulo” permanece descoberto (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:55`). A restrição “view só contexto” consta do escopo registrado (`app/docs/BACKLOG.md:820`). Ser pré-existente e ser impossível corrigir sem duplicar regra ou violar o aval são justificativas históricas/de desenho, não demonstradas só pelas linhas citadas [nao verificado].

- **D2:** o preload de todos os blocos e a alternância por valor existem (`app/colaboradores/views.py:301`, `:304`; `app/templates/colaboradores/partials/painel_vinculo.html:196`, `:204`). Os links permanecem nos blocos ocultos quando há permissão (`app/templates/colaboradores/partials/painel_vinculo.html:205`, `:207`). A aprovação literal desse desenho é relatada no anexo; o documento original da proposta/revisão não foi localizado [nao verificado].

- **D3:** `hoje` vem de `date.today()` e alimenta tanto a agenda anterior quanto a projeção nova (`app/colaboradores/views.py:280`, `:285`, `:302`). O fuso configurado é São Paulo, com `USE_TZ=True` (`app/config/settings/base.py:93`, `:95`). Isso sustenta a alegação de reutilização do relógio existente; a afirmação universal de igualdade com `timezone.localdate()` e a inicialização de `TZ` pelo Django não estão demonstradas pelo código da cópia [nao verificado]. Não há evidência fornecida de divergência em produção [nao verificado].

- **D4:** correto quanto à pergunta implementada: a fonte recebe calendário/ano/mês e calcula a definição, sem ler a foto individual (`app/escala/folgas.py:43`, `:54`, `:56`). A view usa exatamente essa fonte (`app/colaboradores/views.py:304`), coerente com o escopo registrado (`app/docs/BACKLOG.md:820`).

- **D5:** correto como descrição de snapshot: agenda e projeção são montadas durante o GET e entregues no mesmo fragmento (`app/colaboradores/views.py:280`, `:294`, `:304`, `:306`). A alternância apenas escolhe os blocos já entregues (`app/templates/colaboradores/partials/painel_vinculo.html:196`). O painel da página de vínculos pode ser recarregado por novo `fetch` (`app/templates/colaboradores/vincular.html:143`, `:147`).

- **D6:** crescimento linear do trabalho e do HTML é confirmado pelos loops por calendário e por mês (`app/colaboradores/views.py:304`; `app/templates/colaboradores/partials/painel_vinculo.html:204`, `:206`). O teste mede igualdade da quantidade de queries entre duas e sete opções; imprime bytes, sem impor limite de bytes, tempo ou processamento (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:174`, `:177`, `:179`). “~581 bytes” é aritmeticamente coerente com os números do anexo, mas a execução e `green_modulo.log` não foram verificadas [nao verificado]. “140 calendários” não é contagem de produção demonstrada pelo dado fornecido [nao verificado].

Não identifiquei linha citada que contradiga frontalmente o fato central de D1, D4 ou D5; D2 e D3 contêm premissas adicionais sem prova suficiente, e D6 só sustenta custo constante **em queries**, não custo total (`app/colaboradores/views.py:299`, `:304`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:179`).

## 16/11 13:54 · sessao 18019f974ac0 · - · - · origem=Ronald (tmux)

Censo dos quatro campos/contextos:

| Campo | Escritor | Leitor |
|---|---|---|
| `folgas_projecao` | `painel_vinculo` monta `(calendário, [(mês, dias ordenados)])` consultando a fonte e publica no contexto — `app/colaboradores/views.py:304`, `app/colaboradores/views.py:308`. | Template percorre calendários/meses e imprime datas — `app/templates/colaboradores/partials/painel_vinculo.html:204`, `app/templates/colaboradores/partials/painel_vinculo.html:206`. |
| `pode_abrir_calendario` | `painel_vinculo` consulta `tem_acao(request.user, 'editar_escalas')` — `app/colaboradores/views.py:309`. | Template condiciona o link — `app/templates/colaboradores/partials/painel_vinculo.html:207`. O destino exige a mesma ação — `app/escala/views.py:314`, `app/escala/views.py:315`. |
| `data-folgas-cal` | Template grava o PK de cada calendário no atributo e determina `hidden` inicial pelo FK salvo — `app/templates/colaboradores/partials/painel_vinculo.html:205`. | `onchange` lê o atributo, compara com o valor da caixa e grava `e.hidden` — `app/templates/colaboradores/partials/painel_vinculo.html:196`. Regex do teste lê PK/hidden/conteúdo — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:35`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:41`; sonda lê atributo e `display` — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:193`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:194`. |
| `folga_calendario_id` como formulário | Template produz `select`, marca opção pelo FK salvo e permite vazio — `app/templates/colaboradores/partials/painel_vinculo.html:195`, `app/templates/colaboradores/partials/painel_vinculo.html:198`, `app/templates/colaboradores/partials/painel_vinculo.html:200`. Usuário/sonda muda `.value` — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:201`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:203`. | POST lê e resolve PK; vazio resolve `tipo_escala.folga_calendario_padrao` — `app/colaboradores/views.py:230`, `app/colaboradores/views.py:232`, `app/colaboradores/views.py:236`. Sonda seleciona o campo — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:190`. |

`folga_calendario_id` persistente corresponde à FK nullable `EscalaColaborador.folga_calendario`, com `on_delete=SET_NULL` — `app/escala/models.py:890`, `app/escala/models.py:891`, `app/escala/models.py:892`.

Escritores persistentes relevantes:

- `_copiar_folga_padrao` preenche somente vínculo sem calendário, atribui a relação e salva — `app/colaboradores/services/vinculo.py:157`, `app/colaboradores/services/vinculo.py:158`, `app/colaboradores/services/vinculo.py:161`, `app/colaboradores/services/vinculo.py:162`; chamadas após atualizar/criar vínculo — `app/colaboradores/services/vinculo.py:284`, `app/colaboradores/services/vinculo.py:298`.
- `trocar_calendario_de_folga` atribui/salva FK — `app/colaboradores/services/vinculo.py:461`, `app/colaboradores/services/vinculo.py:462`; chamado pela tela individual — `app/colaboradores/views.py:239` — e aplicação em lote — `app/escala/views.py:409`.
- Ajuste automático e desfazimento criam vínculos copiando calendário — `app/colaboradores/services/vinculo.py:1089`, `app/colaboradores/services/vinculo.py:1092`, `app/colaboradores/services/vinculo.py:1110`, `app/colaboradores/services/vinculo.py:1113`.

Leitores persistentes relevantes:

- UI: seleção e bloco inicial — `app/templates/colaboradores/partials/painel_vinculo.html:200`, `app/templates/colaboradores/partials/painel_vinculo.html:205`.
- POST: rótulo anterior e JSON de retorno — `app/colaboradores/views.py:237`, `app/colaboradores/views.py:258`.
- Trilha: snapshot e antes/depois da troca — `app/colaboradores/services/vinculo.py:42`, `app/colaboradores/services/vinculo.py:459`, `app/colaboradores/services/vinculo.py:463`, `app/colaboradores/services/vinculo.py:471`.
- Folgas: universo com calendário — `app/escala/folgas.py:75`, `app/escala/folgas.py:79`; divergência — `app/escala/folgas.py:99`; contador de calendário sem datas — `app/escala/folgas.py:126`, `app/escala/folgas.py:129`; materialização — `app/escala/folgas.py:157`, `app/escala/folgas.py:158`; sincronização futura — `app/escala/folgas.py:627`, `app/escala/folgas.py:629`.
- Salvar definição do calendário procura vínculos por essa relação e sincroniza — `app/escala/folgas.py:600`, `app/escala/folgas.py:601`.

A consequência de D1 está no código: mudar a caixa para vazio oculta todos os blocos imediatamente, mas salvar pode persistir o calendário padrão do tipo — `app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/colaboradores/views.py:236`, `app/colaboradores/views.py:239`. A fixture “sem calendário” não configura calendário padrão no tipo, logo não cobre essa combinação — `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:55`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:58`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:69`.

## 16/11 13:54 · sessao 1d6a3422f25b · - · - · origem=Ronald (tmux)

- **P1/P2 parcialmente atendidos.** O teste executa `change` e mede `display` nas duas cascas, com mutante que remove `onchange`; portanto comprova a alternância do fragmento naquele hospedeiro artificial (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:188`, `:201`, `:255`, `:262`). Não comprova a chegada real: injeta HTML inicial em `#uvc-painel` antes de `</body>` (`:240-250`); a tela real usa `fetch`, `innerHTML`, wrapper `.vinc-painel`, execução dos scripts e `htmx.process` (`app/templates/colaboradores/vincular.html:143-161`). O drawer usa `#drawer-geral-corpo`, abre por transformação e processa HTMX após injeção (`app/templates/core/_drawer_generico.html:3`, `:16-34`).
- **Selo pode ficar verde com integração quebrada.** A sonda só verifica `display` do próprio bloco (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:193-195`); não mede ancestrais, posição, área, clipping, abertura nem processamento do fragmento real. O hospedeiro real limita largura e tem `overflow:hidden`/rolagem, ausentes do wrapper artificial (`app/templates/colaboradores/vincular.html:5-11`; teste `:247`). O requisito local exige duas cascas e afirma que o helper é piso, não teto (`CLAUDE.md:202-203`).
- **Cascata sem garantia de CSS carregado.** O helper apenas liga `STATIC_ROOT` e reescreve URLs (`app/core/tests/test_smoke_chromium.py:93-100`); o novo teste descarta os erros retornados (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:250`). Como `hidden` já funciona pelo navegador e `onchange` é inline, CSS externo ausente pode manter o resultado esperado (`app/templates/colaboradores/partials/painel_vinculo.html:195-205`). Carregamento efetivo em execução: [nao verificado].
- **P3 atendido no ponto relatado.** Exigir HTTP 200 e dois/sete blocos fecha o verde por duas respostas de erro iguais (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:174-176`). A métrica continua sendo quantidade de queries: bytes são impressos, sem limite, e CPU/tempo não são medidos (`:177-179`).
- **P4 melhora, mas não fecha o vínculo argumento→retorno→tela.** O teste exige que as duas chamadas corretas estejam presentes no conjunto de chamadas, mas permite chamadas adicionais erradas (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:154-156`). Todas recebem a mesma sentinela, e a comparação agrega datas do bloco, permitindo consultar corretamente sem usar esses retornos e usar retorno de outra chamada (`:150-158`). Não valida quantidade, exclusividade nem correspondência de cada mês/calendário com sua resposta (`:154-158`).
- **Execução dos testes nesta auditoria: [nao verificado].** O próprio caminho de teste cria diretório e arquivos temporários e os apaga; executá-lo contrariaria a restrição somente leitura (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:248-252`; `app/core/tests/test_smoke_chromium.py:94-100`).

## 16/11 13:47 · sessao 25e4b8416f04 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**Parecer: não fecharia esta versão.** O caminho “vazio → calendário padrão” deixa a projeção divergente do calendário salvo; o teste de alternância não executa a interação e o selo novo não cobre as duas cascas. (`app/colaboradores/views.py:235-257`; `app/templates/colaboradores/partials/painel_vinculo.html:193-205`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120`; `CLAUDE.md:203`)

Execução dos testes, smoke, números efetivos de queries e confirmação Git do hash/base: **[nao verificado]**. [nao verificado]

1. **Quem escreve, quem lê e significado**

   | Campo/valor | Escritor → leitores |
   |---|---|
   | `folga_calendarios` | A view produz uma lista ordenada, com `tamanhos` prefetched; a própria view percorre essa lista para projetar e o template percorre para montar opções. (`app/colaboradores/views.py:301-307`; `app/templates/colaboradores/partials/painel_vinculo.html:199-200`) |
   | `folgas_projecao` | A view produz pares calendário/meses, contendo datas ordenadas da fonte; o template consome meses, datas e calendário do link. (`app/colaboradores/views.py:302-308`; `app/templates/colaboradores/partials/painel_vinculo.html:204-207`) |
   | `pode_abrir_calendario` | A view chama `tem_acao('editar_escalas')`; o template condiciona o link. O destino exige a mesma ação por um decorator que chama o mesmo juiz. (`app/colaboradores/views.py:309`; `app/templates/colaboradores/partials/painel_vinculo.html:207`; `app/escala/views.py:314-316`; `app/colaboradores/mixins.py:334-346`) |
   | `escala_ativa.folga_calendario_id` | A view carrega a primeira escala `ativa=True`; o template usa o FK para seleção e visibilidade inicial. O POST escreve pela porta `trocar_calendario_de_folga`; a herança inicial também escreve no serviço de vínculo, e operações de cópia preservam o FK. (`app/colaboradores/views.py:268`; `app/templates/colaboradores/partials/painel_vinculo.html:200-205`; `app/colaboradores/services/vinculo.py:151-162`; `app/colaboradores/services/vinculo.py:450-462`; `app/colaboradores/services/vinculo.py:1092`; `app/colaboradores/services/vinculo.py:1113`) |
   | `FolgaCalendario.modo/celulas/tamanhos` | As portas de salvar/criar calendário escrevem esses dados. A fonte lê `modo`, depois `celulas` ou `dias_por_tamanho()`, que percorre `tamanhos.all()`. Editor e listagem também leem a definição. (`app/escala/folgas.py:571-595`; `app/escala/folgas.py:645-661`; `app/escala/folgas.py:52-56`; `app/escala/models.py:1558-1560`; `app/escala/views.py:292-307`; `app/escala/views.py:362-371`) |
   | `hoje` | É variável local, **não contexto enviado ao template**. Já vinha de `date.today()` para a agenda; agora também determina os dois meses da projeção. (`app/colaboradores/views.py:280-288`; `app/colaboradores/views.py:302-318`) |

   Os meses projetados são **meses civis completos**, inclusive dias anteriores a hoje; a fonte retorna `date`, sem hora. Não há recorte por competência 21–20 nesse caminho. (`app/colaboradores/views.py:302-305`; `app/escala/folgas.py:6-15`; `app/escala/folgas.py:18-36`)

   **Não há uniformidade explícita de fuso:** painel e POST usam `date.today()`, enquanto a porta de troca e o salvamento do calendário usam `timezone.localdate()`. Os settings declaram `America/Sao_Paulo` e `USE_TZ=True`; o fuso efetivo do processo e eventual divergência em produção são **[nao verificado]**. (`app/colaboradores/views.py:243`; `app/colaboradores/views.py:280`; `app/colaboradores/services/vinculo.py:465`; `app/escala/folgas.py:599`; `app/config/settings/base.py:93-95`)

   Há duas interpretações do **vazio**: na apresentação significa “nenhuma projeção”; no POST significa “resolver o padrão do tipo”, que pode ser um calendário concreto. (`app/templates/colaboradores/partials/painel_vinculo.html:196-205`; `app/colaboradores/views.py:230-239`)

2. **Derivação fora da fonte**

   Não identifiquei derivação paralela no trecho novo: a view chama `datas_que_o_calendario_manda` e apenas ordena; o template itera e formata. A escolha entre os dois modos continua dentro da fonte. (`app/colaboradores/views.py:300-305`; `app/templates/colaboradores/partials/painel_vinculo.html:204-206`; `app/escala/folgas.py:43-56`)

   No GET, cada projeção contém todas as datas retornadas para aquele calendário/mês. Ela representa **a definição do calendário**, sem filtrar vigência do vínculo ou consultar a foto efetiva da pessoa; portanto não certifica os dias materializados ou julgados para ela. (`app/colaboradores/views.py:304-305`; `app/templates/colaboradores/partials/painel_vinculo.html:206`; `app/escala/folgas.py:157-158`; `app/escala/models.py:1069-1096`)

3. **Quando fica velha ou divergente; o que o admin vê**

   | Caminho | Resultado pelo código |
   |---|---|
   | Trocar a caixa sem salvar | Mostra imediatamente a projeção e o link do candidato selecionado; o vínculo só muda no POST. Isso atende à prévia do calendário **selecionado**. (`app/templates/colaboradores/partials/painel_vinculo.html:195-207`; `app/colaboradores/views.py:230-239`) |
   | Salvar calendário explícito | Permanece a projeção calculada no GET; a resposta é somente toast. Não há atualização da definição após salvar. (`app/templates/colaboradores/partials/painel_vinculo.html:191-196`; `app/colaboradores/views.py:251-257`) |
   | Salvar vazio, com padrão não nulo | **Falha:** a caixa continua vazia e todos os blocos ficam ocultos, embora o POST tenha salvo o calendário padrão. Novo GET seleciona esse FK e mostra sua projeção. (`app/templates/colaboradores/partials/painel_vinculo.html:196-205`; `app/colaboradores/views.py:235-239`; `app/colaboradores/views.py:251-257`) |
   | Salvar vazio, sem padrão | Continua sem calendário e sem projeção visível, coerente com o valor salvo. (`app/colaboradores/views.py:235-239`; `app/templates/colaboradores/partials/painel_vinculo.html:205`) |
   | Sem escala ativa | Exibe “vincule a escala primeiro”, bloco esmaecido e `pointer-events:none`; ainda calcula todas as projeções, inicialmente ocultas. POST retorna 400. Os controles não recebem `disabled`; comportamento por teclado: **[nao verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:184-211`; `app/colaboradores/views.py:226-229`; `app/colaboradores/views.py:301-305`) |
   | Calendário editado depois | Um painel ainda aberto conserva a definição antiga; alternar a caixa apenas alterna HTML existente. Novo GET recalcula. O editor salva e redireciona à lista; retorno pelo histórico/cache do navegador: **[nao verificado]**. (`app/templates/colaboradores/partials/painel_vinculo.html:196`; `app/colaboradores/views.py:301-305`; `app/escala/views.py:354-361`) |
   | Virada de mês com painel aberto | Conserva os dois meses do GET; o handler não consulta relógio nem servidor. (`app/colaboradores/views.py:280`; `app/colaboradores/views.py:302-305`; `app/templates/colaboradores/partials/painel_vinculo.html:196`) |

   Na leitura literal de **“sem calendário = sem link”**, também há desvio: com permissão, os links dos calendários oferecidos continuam no HTML, dentro de blocos `hidden`. O teste exige somente ausência de blocos visíveis. (`app/templates/colaboradores/partials/painel_vinculo.html:204-209`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:123-129`)

4. **Duas cascas e raiz da busca**

   O endpoint entrega um fragmento, sem renderizar uma casca. O caminho direto vem de `vincular.html`, que estende `base.html`: a linha chama `abrirPainelVinculo`, depois `carregarPainel`, que busca **`/colaboradores/vincular/<cid>/painel/` a partir da raiz do host**, injeta o HTML e processa HTMX. (`app/colaboradores/views.py:306`; `app/templates/colaboradores/vincular.html:1`; `app/templates/colaboradores/partials/vinculo_row.html:4`; `app/templates/colaboradores/vincular.html:136-161`; `app/config/urls.py:26`; `app/colaboradores/urls.py:12`)

   Existe outro caminho pelo retrato: tipos escala/folga/sem_escala geram a URL do painel e o bloco chama `window.abrirDrawer`. Esse carregador faz `fetch(url)` e está incluído nas duas cascas. Isso demonstra infraestrutura compartilhada; sessão real oferecendo e abrindo o novo painel em `base_app.html`: **[nao verificado]**. (`app/folha/retrato_folha.py:1003-1006`; `app/folha/retrato_folha.py:1066-1075`; `app/templates/folha/_retrato_o_que_fazer.html:73`; `app/templates/core/_drawer_generico.html:11-33`; `app/templates/base.html:399`; `app/templates/base_app.html:154`)

5. **Leis de UI e conduta de front**

   - **L2:** a derivação de datas e a autorização do link usam autoridades existentes. A inconsistência está na resolução do vazio descrita acima. (`app/docs/LEIS-UI.md:23-25`; `app/colaboradores/views.py:304-309`; `app/colaboradores/views.py:235-239`)
   - **L4:** o novo link só é renderizado com `editar_escalas`, mesma autorização exigida pelo destino. (`app/docs/LEIS-UI.md:39-49`; `app/templates/colaboradores/partials/painel_vinculo.html:207`; `app/escala/views.py:315`; `app/colaboradores/mixins.py:342`)
   - **L7/L8 e JS por posição:** o trecho acrescentado não traz ajuda explicativa, `title` cru ou diálogo nativo. O handler busca pelo atributo `data-folgas-cal`, dentro do formulário, sem índice ou posição no DOM. (`app/templates/colaboradores/partials/painel_vinculo.html:195-209`; `app/docs/LEIS-UI.md:77-89`; `app/core/selo_js.py:3-7`)
   - **L1/L3/L6/L9/L10/L11:** o trecho novo não acrescenta contador, classificação de estado, filtro, PDF ou ícone; não identifiquei violação nova dessas leis nesse bloco. (`app/templates/colaboradores/partials/painel_vinculo.html:195-209`; `app/docs/LEIS-UI.md:12`; `app/docs/LEIS-UI.md:31`; `app/docs/LEIS-UI.md:68`; `app/docs/LEIS-UI.md:95`; `app/docs/LEIS-UI.md:120`; `app/docs/LEIS-UI.md:174`)
   - **L5:** o botão existente continua sem indicador/trava, mas duração superior a um segundo é **[nao verificado]**; não há base para declarar nova violação por lentidão. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/docs/LEIS-UI.md:56-60`)
   - **Duas cascas:** o selo novo só pede o fragmento; não renderiza ambas nem prova seu grafo, contrariando a conduta explícita. Smoke de clique e eventual execução de verificações JS: **[nao verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:78-83`; `CLAUDE.md:202-203`)

   O `title` cru da agenda e o emoji de “Ver agenda completa” já existem fora das adições apresentadas; não são introduzidos por esta obra. (`app/templates/colaboradores/partials/painel_vinculo.html:226`; `app/templates/colaboradores/partials/painel_vinculo.html:249`)

6. **O teste morde?** [nao verificado]

   - **Fonte: sim, parcialmente.** Exige sentinela não vazia na saída e ausência do dia antigo. Porém o retorno igual para todas as chamadas e a conversão em conjuntos não verificam argumentos, separação por mês, ordem ou duplicações. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:41-42`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:90-91`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:143-150`)
   - **Alternância: não prova funcionamento.** Verifica estado inicial e uma regex do `onchange`; um handler quebrado contendo o texto esperado pode passar. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120`)
   - **Sem calendário:** tem proteção contra ausência total de blocos, mas verifica visibilidade, não ausência literal de links. A fixture tem escala ativa e tipo sem padrão; não alcança o caso vazio com padrão concreto. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:52-75`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:123-129`)
   - **Sem ação:** preserva datas não vazias e exige ausência da URL; existe também teste positivo do link. Testa o consumo de um juiz mockado, sem validar navegação de uma conta real sem permissão. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:101-103`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:131-140`)
   - **Custo:** isoladamente pode passar com duas respostas iguais de erro/redirect, pois não exige status 200 nem projeções nas respostas medidas. Não fixa teto absoluto. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:153-167`)
   - **Relógio:** a classe está congelada; não depende do dia da execução. O horário escolhido não testa divergência na fronteira de dia/mês entre fusos. RED/GREEN efetivamente executados: **[nao verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:45`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:101-108`)

7. **Lista, outros leitores e queries**

   O consumidor do painel apenas itera, portanto não depende de métodos de queryset. O wizard usa o mesmo nome de contexto, mas recebe querysets produzidos por suas próprias views; a mudança local não altera esses produtores. (`app/templates/colaboradores/partials/painel_vinculo.html:199`; `app/templates/escala/_wizard_cal_select.html:4`; `app/escala/views_wizard.py:86`; `app/escala/views_wizard.py:176`; `app/escala/views_wizard.py:197`)

   **Contagem estrutural esperada do carregamento de calendários:** uma consulta de calendários mais uma de `tamanhos` quando há calendários; sem calendários, somente a primeira. O prefetch atende `tamanhos.all()` usado pela fonte. Frente ao queryset anterior das opções, acrescenta uma consulta quando a lista não está vazia. (`app/colaboradores/views.py:299-305`; `app/escala/models.py:1558-1560`; `app/escala/folgas.py:56`)

   A quantidade dessas consultas é constante em relação ao número de calendários, mas há **duas chamadas à fonte por calendário**, além de memória e HTML proporcionais à quantidade oferecida. (`app/colaboradores/views.py:304-305`; `app/templates/colaboradores/partials/painel_vinculo.html:204-209`)

   **Número total efetivo por request: [nao verificado].** O endpoint também executa a agenda existente, com consultas dependentes dos caminhos de células/fotos; o teste fornecido apenas compara dois totais e imprime números cuja saída não foi fornecida. (`app/colaboradores/views.py:283-294`; `app/escala/models.py:1092-1096`; `app/escala/models.py:1122-1123`; `app/escala/models.py:1286-1288`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:165-167`)

8. **Classes que já reprovaram obras**

   - **Segundo juiz/derivação paralela:** não aparece no trecho novo; datas vêm da fonte declarada e a permissão vem do juiz existente. (`app/colaboradores/views.py:300-309`; `app/escala/folgas.py:43-56`; `app/colaboradores/mixins.py:322-342`)
   - **Segundo escritor fora da porta:** não aparece nas adições; elas consultam e montam contexto, e o JS altera somente visibilidade. (`app/colaboradores/views.py:300-309`; `app/templates/colaboradores/partials/painel_vinculo.html:196`)
   - **Relógio fora da borda:** não foi acrescentado; a view reutiliza seu `hoje`. Permanece a diferença de convenção de fuso apontada no item 1. (`app/colaboradores/views.py:280`; `app/colaboradores/views.py:302-305`; `app/colaboradores/services/vinculo.py:465`)
   - **Selo sem mordida suficiente:** presente na promessa de alternância e na verificação isolada de custo; comprovação histórica de RED: **[nao verificado]**. (`app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:112-120`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:153-167`)
   - **Campo com dois significados:** presente no vazio da caixa — ausência de projeção na UI versus resolução de calendário padrão no POST — e agravado pela manutenção do HTML após salvar. (`app/templates/colaboradores/partials/painel_vinculo.html:193-205`; `app/colaboradores/views.py:235-257`)

## 16/11 13:45 · sessao 444ae62b7f9f · - · - · origem=Ronald (tmux)

1. **Produtores/leitores e significado**

- `folga_calendarios`: escrito por `painel_vinculo` como lista ordenada de todos os `FolgaCalendario`, com prefetch de `tamanhos`; lido na própria view para produzir projeções e no template para opções `pk/nome` (`app/colaboradores/views.py:301`, `app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:199`).
- `folgas_projecao`: escrito somente pela view como `(calendário, [(primeiro dia do mês, datas ordenadas da fonte)])`; lido no template para datas e link (`app/colaboradores/views.py:302`, `app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:204`).
- `pode_abrir_calendario`: view chama `tem_acao(user, 'editar_escalas')`; template consome o booleano; destino usa `@acao_required('editar_escalas')`, cujo corpo chama o mesmo `tem_acao`. Não há segundo juiz de permissão neste trecho (`app/colaboradores/views.py:309`, `app/templates/colaboradores/partials/painel_vinculo.html:207`, `app/escala/views.py:315`, `app/colaboradores/mixins.py:342`).
- `escala_ativa.folga_calendario_id`: carregado da primeira escala `ativa=True`, usado para marcar opção e mostrar bloco inicial. Escritor do POST é `trocar_calendario_de_folga`; a herança inicial `_copiar_folga_padrao` também escreve dentro do serviço de vínculo. O novo JavaScript só troca o bloco conforme o valor escolhido, sem escrever banco (`app/colaboradores/views.py:268`, `app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/templates/colaboradores/partials/painel_vinculo.html:200`, `app/templates/colaboradores/partials/painel_vinculo.html:205`, `app/colaboradores/services/vinculo.py:161`, `app/colaboradores/services/vinculo.py:461`).
- `FolgaCalendario.modo/celulas/tamanhos`: campos definidos como modo `dias_mes` ou `semana_dia`, pares de ocorrência semanal e dias por tamanho de mês. Editor chama `salvar_calendario_de_folga`, que grava modo/células e apaga ou atualiza tamanhos; wizard chama a porta `criar_calendario_de_folga`. A fonte lê `modo/celulas`, ou `dias_por_tamanho()` que lê `tamanhos.all()` (`app/escala/models.py:1539`, `app/escala/models.py:1544`, `app/escala/models.py:1558`, `app/escala/models.py:1566`, `app/escala/views.py:354`, `app/escala/folgas.py:584`, `app/escala/folgas.py:593`, `app/escala/folgas.py:645`, `app/escala/folgas.py:654`, `app/escala/folgas.py:54`).
- `hoje` **não é contexto enviado ao template**: variável local da view, escrita por `_dt.date.today()`, usada na agenda e agora na seleção dos dois meses. As datas são civis, sem hora; a fonte produz `date(ano, mes, dia)`. A projeção cobre meses civis completos, incluindo dias anteriores a hoje, exatamente conforme item 1, e não a competência 21–20 (`app/colaboradores/views.py:280`, `app/colaboradores/views.py:285`, `app/colaboradores/views.py:302`, `app/colaboradores/views.py:306`, `app/escala/folgas.py:15`, `app/escala/folgas.py:36`). A competência tem outro contrato explícito (`app/ponto/janelas.py:10`).
- **Fuso não é uniformizado no código**: view e POST usam `date.today()`, mas troca pelo serviço e salvamento do calendário usam `timezone.localdate()`; settings declaram `America/Sao_Paulo` e `USE_TZ=True`. Se o fuso do processo diferir, a virada de mês pode divergir. Configuração real do processo e divergência em produção: **[nao verificado]** (`app/colaboradores/views.py:243`, `app/colaboradores/views.py:280`, `app/colaboradores/services/vinculo.py:465`, `app/escala/folgas.py:599`, `app/config/settings/base.py:93`).
- **O vazio tem duas leituras de interface/ação**: a seleção vazia oculta todas as projeções; no POST, vazio resolve para `ec.tipo_escala.folga_calendario_padrao`, que pode ser calendário concreto. Após sucesso HTMX só há toast, portanto a interface fica vazia embora tenha sido salvo um calendário padrão (`app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/templates/colaboradores/partials/painel_vinculo.html:198`, `app/colaboradores/views.py:230`, `app/colaboradores/views.py:236`, `app/colaboradores/views.py:251`).

2. **Fonte única**

- Não há derivação paralela de folgas no trecho novo: a view chama `datas_que_o_calendario_manda` duas vezes por calendário e só ordena; template só itera e formata. Os dois modos são interpretados exclusivamente pela fonte (`app/colaboradores/views.py:300`, `app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:206`, `app/escala/folgas.py:52`).
- A tela apresenta todas as datas devolvidas para cada mês; não filtra hoje, vigência, foto materializada ou folgas manuais. Isso é coerente com “o calendário manda”, mas não prova os dias efetivamente materializados ou julgados para a pessoa (`app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:206`, `app/escala/folgas.py:158`, `app/escala/folgas.py:184`, `app/escala/folgas.py:187`).
- O relógio continua lido na borda view, e a fonte recebe ano/mês explicitamente; o diff não acrescenta relógio dentro do resolvedor (`app/colaboradores/views.py:280`, `app/colaboradores/views.py:304`, `app/escala/folgas.py:43`).

7. **Outros leitores e custo**

- Outro leitor com o mesmo nome de contexto é `_wizard_cal_select.html`, mas ele recebe seu próprio queryset de `views_wizard`; a mudança local para lista no painel não o afeta. No painel, o leitor só itera, sem métodos de queryset (`app/templates/escala/_wizard_cal_select.html:4`, `app/escala/views_wizard.py:86`, `app/escala/views_wizard.py:176`, `app/escala/views_wizard.py:197`, `app/templates/colaboradores/partials/painel_vinculo.html:199`).
- Custo estrutural do trecho novo: **1 consulta de calendários + 1 prefetch de tamanhos quando há calendários**; sem calendários, só a primeira. Antes, a lista de opções exigia uma consulta. A fonte lê `self.tamanhos.all()` de objetos prefetched, sem `.filter()` que descartaria o cache (`app/colaboradores/views.py:301`, `app/escala/models.py:1558`, `app/escala/folgas.py:56`).
- Constante em número de consultas de calendário para `K>0`; CPU/memória/HTML crescem com `K`, pois há `2K` chamadas à fonte e um bloco por calendário (`app/colaboradores/views.py:304`, `app/templates/colaboradores/partials/painel_vinculo.html:204`).
- **Número total medido por request: [nao verificado]**. Não confundir a constância relativa a `K` com custo total fixo: a agenda existente chama `eh_dia_trabalho` até 14 vezes e `marcos_do_dia` nos dias de trabalho, cujos caminhos consultam células e, no fallback, fotos (`app/colaboradores/views.py:287`, `app/colaboradores/views.py:289`, `app/colaboradores/views.py:292`, `app/escala/models.py:1092`, `app/escala/models.py:1122`, `app/escala/models.py:1286`).

_6049 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
