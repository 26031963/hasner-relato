# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 21:12** · publicado em **30/10/2026 21:12** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 21:12 · sessao 2792b753efd0 · O261-E-CELULA-20 · verificador · origem=Code

**1. A extração não altera os contadores de `censo()`**, considerando os mesmos dados e respostas dos leitores. A cadeia preserva a precedência do pai transcrito no diff: decisão → ausência → folga validada → admin. ([vigia_fio.py:194](/home/ronald/codex_ws/6a8c5300a64ff11b7fa84c1584c27615dfe38cd9/app/chamados/services/vigia_fio.py:194))

| Comparação pai × novo | Resultado |
|---|---|
| Decisão | Mesmo `resolver_dia_oferecido_na_data(colab, data)`; incrementa o contador, acrescenta `c.pk` a `pendentes` e continua. `app/chamados/services/vigia_fio.py:194`, `app/chamados/services/vigia_fio.py:329` |
| Ausência | Mesmo `ausencia_cobre(..., para='cobranca')`, teste de não nulo e `status in ABERTAS`; mesmo incremento e `continue`. `app/chamados/services/vigia_fio.py:196`, `app/chamados/services/vigia_fio.py:333` |
| Folga | Mesmo `any`, estado `VALIDADA` e `_e_aceite_de_folga`; preservados contador, lista `celulas_escala_nao_descreve_o_dia`, limite e `continue`. `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:336` |
| Admin | Continua sendo o último teste; preservados contador, lista `celulas_fio_do_admin`, limite e `continue`. `app/chamados/services/vigia_fio.py:201`, `app/chamados/services/vigia_fio.py:341` |

Os argumentos são o mesmo colaborador, data e **todas** as perguntas da célula (`ps`). `lambda c=c: _fio_do_admin(c)` captura a célula corrente e mantém a chamada adiada; a falha da lista continua incrementando `fio_do_admin_sem_resposta` uma vez por dia que chega a esse teste. `app/chamados/services/vigia_fio.py:304`, `app/chamados/services/vigia_fio.py:328`, `app/chamados/services/vigia_fio.py:284`, `app/chamados/services/vigia_fio.py:297`.

Permanecem os filtros anteriores, os incrementos de morto, `celulas_do_fio_morto`, `classes_do_fio_morto` e a classificação posterior de `pendentes`; portanto, os contadores derivados também conservam suas entradas. `app/chamados/services/vigia_fio.py:315`, `app/chamados/services/vigia_fio.py:346`, `app/chamados/services/vigia_fio.py:358`, `app/chamados/services/vigia_fio.py:380`.

**2. Não encontrei contraexemplo residual no delta de `(d)`, dentro do contrato declarado.** O teste anterior de véu impede `(d)` com chamado vivo; `not ps` exige ausência de pergunta viva; a nova condição exige colaborador listado **e dia ausente**. `app/chamados/services/vigia_fio.py:497`, `app/chamados/services/vigia_fio.py:557`, `app/chamados/services/vigia_fio.py:569`.

Sem porta e vencidas as leis anteriores:

- Dia listado com BATIDA/CADASTRO sai para `alcance_fio_do_admin`. `app/chamados/services/vigia_fio.py:548`.
- Dia listado com ESTRUTURA/sem dono, sem pergunta viva, recebe `(e, dia_na_lista_dono_estrutura|nenhum)`. A presença da chave distingue “sem dono” de “dia ausente”. `app/chamados/services/vigia_fio.py:573`, `app/escala/services/cadastro_realidade.py:670`.
- Sem célula recebe `(e, sem_celula)`; com pergunta viva segue as causas da pergunta. `app/chamados/services/vigia_fio.py:560`, `app/chamados/services/vigia_fio.py:564`, `app/chamados/services/vigia_fio.py:579`.

Nenhum dia do universo some ou duplica no processamento normal: `setdefault((colab, data), ...)` deduplica a identidade; cada chave passa uma vez pelo laço; cada saída incrementa um balde e continua, ou incrementa exatamente um morto e uma causa. `limite` trunca somente a lista. `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:529`, `app/chamados/services/vigia_fio.py:532`, `app/chamados/services/vigia_fio.py:551`, `app/chamados/services/vigia_fio.py:587`.

O teste cobre explicitamente ESTRUTURA, mas falta a asserção correspondente a `dia_na_lista_dono_nenhum`. `app/chamados/tests/test_o261e_f5_alcance.py:174`.

**3. Sim: o mutante que antecipa hoje sobrevive à cobertura do arquivo, por análise estática.** Os outros três possuem asserções que os detectariam. `app/chamados/tests/test_o261e_f5_alcance.py:114`, `app/chamados/tests/test_o261e_f5_alcance.py:141`, `app/chamados/tests/test_o261e_f5_alcance.py:185`, `app/chamados/tests/test_o261e_f5_alcance.py:193`.

| Mutante | Resultado pela leitura dos testes | [nao verificado]
|---|---|
| Hoje antes das portas | **Sobrevive:** portas são testadas no dia passado; o caso `hoje=DIA` não cria porta. Falta cruzar hoje com cada porta e conferir o balde. `app/chamados/tests/test_o261e_f5_alcance.py:25`, `:114`, `:121`, `:206` |
| Remover filtro do app | Detectado pelo negativo fora do app: surgiria uma porta indevida. `app/chamados/tests/test_o261e_f5_alcance.py:185` |
| Remover exclusão da âncora na fila do véu | Detectado pela resposta vazia: mudaria `(b)` para `(a)`; também quebra o caso fora do app. `app/chamados/tests/test_o261e_f5_alcance.py:141`, `:185`; `app/chamados/services/vigia_fio.py:557` |
| Contar `alcance_lista_sem_resposta` duas vezes | Detectado pelo `assertEqual(1, ...)` específico; as somas gerais não incluem esse contador. `app/chamados/tests/test_o261e_f5_alcance.py:197`, `:92` |

Execução da suíte, execução dos mutantes e ocorrência desses cenários em produção: **[nao verificado]**. [nao verificado]

## 30/10 21:11 · sessao 4caceba3830f · - · - · origem=Ronald (tmux)

Não encontrei erro residual na correção de (d), considerando o contrato declarado e as fontes da cópia:

- **Sem pergunta viva + colaborador listado + dia fora da lista:** cai em (d). O `elif veu` anterior impede atribuir (d) quando há chamado vivo; `ps` contém somente perguntas vivas. `app/chamados/services/vigia_fio.py:494-498`, `:557-570`.
- **Dia listado, dono BATIDA/CADASTRO:** sai para `alcance_fio_do_admin`; **ESTRUTURA ou None**, sem pergunta viva, permanece em (e), detalhe `dia_na_lista_dono_estrutura` ou `dia_na_lista_dono_nenhum`. `app/chamados/services/vigia_fio.py:543-553`, `:573-576`. A leitura conserva presença e dono lavrado do dia, sem derivação nova. `app/escala/services/cadastro_realidade.py:655-670`.
- **Com pergunta viva:** não entra em (d) nem no novo detalhe do dia listado; segue as causas e detalhes da pergunta. **Sem célula:** recebe `(e, sem_celula)`, antes de (d). São precedências expressas no código. `app/chamados/services/vigia_fio.py:560-586`.
- **Nenhum dia do universo some ou duplica no processamento normal:** o universo é um dicionário por `(colab, data)`; cada chave é percorrida uma vez; as portas, hoje e quatro leis incrementam um balde e continuam; o restante incrementa exatamente um morto e uma causa. O limite trunca apenas a amostra. `app/chamados/services/vigia_fio.py:465-475`, `:529-553`, `:587-592`.
- **A competência não duplica dias:** as janelas consecutivas têm início no corte e fim no corte menos um do mês seguinte; as datas vêm da mesma janela usada por `fora_da_folha`. `app/ponto/janelas.py:10-21`, `:54-72`; `app/folha/fora_da_folha.py:119-139`, `:168`.
- **Detalhe literal `outra`:** não identifiquei entrada regular que o alcance. Sem véu, terminal ou âncora ausente, as âncoras têm de estar na fila; se alguma pergunta aparece no app e tem resposta válida, já seria porta; se tem resposta vazia, cai em (b). Pergunta viva nunca tem `validada_em` preenchida. `app/chamados/catalogo/motor.py:19-31`; `app/chamados/juizes.py:1005-1007`; `app/chamados/services/vigia_fio.py:535`, `:557-586`. Estado fora do catálogo ou inconsistência de FK em produção: [nao verificado].

O teste já verifica explicitamente ESTRUTURA contra (d), mas não inclui o ramo `None`/`dia_na_lista_dono_nenhum`. `app/chamados/tests/test_o261e_f5_alcance.py:174-183`.

## 30/10 21:10 · sessao a3d62278a9a7 · - · - · origem=Ronald (tmux)

**Q1: a extração preserva os valores de `censo()` para os mesmos dados e respostas dos leitores.** As quatro condições do pai no diff anexo aparecem na mesma ordem e com os mesmos argumentos no helper: `resolver_dia_oferecido_na_data(colab, data)`; `ausencia_cobre(colab, data, para='cobranca')` seguida do teste `status in ABERTAS`; `any(p.estado == p.VALIDADA and _e_aceite_de_folga(p)...)`; chamada do admin. Cada retorno encerra a cadeia no primeiro resultado verdadeiro. (`app/chamados/services/vigia_fio.py:194`, `:196`, `:199`, `:201`.)

| Item comparado ao pai do diff anexo | Resultado na cópia |
|---|---|
| Ponto da extração | Continua depois dos filtros de outro vocabulário, fio vivo e hoje/futuro; esses filtros mantêm seus incrementos e `continue`. (`app/chamados/services/vigia_fio.py:315`, `:320`, `:323`.) |
| Argumentos | São o mesmo colaborador, a mesma data e a lista completa `ps`, obtida por célula. (`app/chamados/services/vigia_fio.py:304`, `:314`, `:328`.) |
| Lambda do admin | `lambda c=c: _fio_do_admin(c)` captura a célula corrente e só chama `_fio_do_admin` se as três leis anteriores falharem; não antecipa a consulta da lista. (`app/chamados/services/vigia_fio.py:328`, `:201`.) |
| Decisão pendente | Incrementa o mesmo contador, acrescenta `c.pk` a `pendentes` e continua. (`app/chamados/services/vigia_fio.py:329`.) |
| Ausência aberta | Incrementa o mesmo contador e continua. (`app/chamados/services/vigia_fio.py:333`.) |
| Folga validada | Incrementa o mesmo contador; preserva `len(lista) < limite`, o mesmo `c.pk` e o `continue`. (`app/chamados/services/vigia_fio.py:336`.) |
| Fio do admin | Incrementa o mesmo contador; preserva `len(lista) < limite`, o mesmo `c.pk` e o `continue`. (`app/chamados/services/vigia_fio.py:341`.) |
| Falha da lista | Continua armazenada uma vez em `_na_lista`, e cada dia que efetivamente chega ao admin incrementa `fio_do_admin_sem_resposta` uma vez. (`app/chamados/services/vigia_fio.py:284`, `:296`, `:297`.) |
| Nenhuma lei satisfeita | Cai nos mesmos incrementos de morto, abertura por ausência/presença de perguntas, cobrança e listas. (`app/chamados/services/vigia_fio.py:346`.) |
| Contadores derivados | `mortos` segue alimentando a classificação; `pendentes` segue alimentando `decisao_do_dia_pendente_com_alvo`, sem mudança nas operações segundo o diff. (`app/chamados/services/vigia_fio.py:358`, `:380`.) |

Os testes antigos contêm verificações para precedência decisão/ausência, BATIDA/CADASTRO contra ESTRUTURA/sem dono, falha da lista uma vez por rodada e a lista da folga validada; a execução deles nesta análise não foi realizada **[nao verificado]**. (`app/chamados/tests/test_o261_vigia_fio.py:531`, `:553`, `:581`, `:626`.)

A equivalência com o pai foi examinada pelo **diff fornecido**, não por checkout ou execução da revisão pai **[nao verificado]**.

## 30/10 21:10 · sessao 295113302444 · - · - · origem=Ronald (tmux)

O mutante **“hoje antes das portas” sobrevive à cobertura deste arquivo por análise estática**: as portas são plantadas em `DIA=01/09`, enquanto `HOJE=25/09`; o único caso que chama `hoje=DIA` não cria porta. Falta cruzar **dia de hoje + porta existente**, conferindo qual balde recebe o dia (`app/chamados/tests/test_o261e_f5_alcance.py:25`, `:26`, `:114`, `:121`, `:206`; ordem atual em `app/chamados/services/vigia_fio.py:532`, `:535`, `:538`).

| Mutante | Resultado estático | Evidência |
|---|---|---|
| Mover teto de hoje antes das portas | Sobrevive à cobertura do arquivo; nenhum caso cruza hoje com porta. | `app/chamados/tests/test_o261e_f5_alcance.py:114`, `:121`, `:206` |
| Remover `p.pk in no_app(cid)` da porta | Morto pelo negativo fora do app: o mutante produz porta e perde a linha esperada em `(e)`. | `app/chamados/services/vigia_fio.py:535`; `app/chamados/tests/test_o261e_f5_alcance.py:185`, `:190`, `:191` |
| Remover `chamado_que_lista(p) not in fila_pk` do véu | Morto pelo caso de resposta vazia: a âncora `aberto`, sem célula e na fila, passa a compor o véu; `(a)` ganha precedência sobre `(b)`. Também quebra o negativo fora do app, de `(e)` para `(a)`. | `app/chamados/tests/test_o261e_f5_alcance.py:141`, `:142`, `:146`, `:185`, `:191`; `app/chamados/services/vigia_fio.py:557`, `:562`, `:564`; `app/chamados/catalogo/motor.py:24` |
| Incrementar `alcance_lista_sem_resposta` duas vezes por dia | Morto: teste da lista falhando exige exatamente `1`. | `app/chamados/services/vigia_fio.py:543`, `:546`; `app/chamados/tests/test_o261e_f5_alcance.py:193`, `:197` |

As somas auxiliares não detectam duplicação de `alcance_lista_sem_resposta`, pois esse contador não participa delas; quem detecta é a asserção específica `==1` (`app/chamados/tests/test_o261e_f5_alcance.py:90`, `:92`, `:95`, `:97`, `:197`).

Resultado de execução dos testes/mutantes: **[nao verificado]**. [nao verificado]

## 30/10 21:04 · sessao 05e1da98864e · O261-E-CELULA-20 · verificador · origem=Code

**Não daria aval de fechamento da F5:** há porta real que o censo pode perder, causa `(d)` mal atribuída e o alarme continua governado pelo contador antigo. (`app/chamados/services/vigia_fio.py:495`, `app/chamados/services/vigia_fio.py:569`, `app/chamados/management/commands/vigiar_fio_morto.py:163`.)

Limite da verificação: a cópia não contém `.git`; `git show eede1619` falhou. Diff efetivo, comparação com `21b9db38` e resultados de testes executados: **[não verificado]**. A análise abaixo é da árvore fornecida; ocorrência dos contraexemplos em produção: **[não verificado]**.

**1. Contagem, universo, portas e causas**

- **Porta real não reconhecida.** Contraexemplo: dia retido com célula; pergunta do mesmo colaborador/data, `celula=NULL`, respondida com `07:00`, não validada, sem tranca; destino aberto, sem célula, presente na fila. A FK nula é permitida, o juiz mantém essa resposta viva, o app busca por colaborador e a âncora usa o destino. Entretanto, o vigia busca perguntas exclusivamente pela FK da célula e perde essa porta. (`app/chamados/models.py:1943`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1032`, `app/chamados/services/pergunta_no_app.py:42`, `app/chamados/services/fio.py:117`, `app/chamados/services/vigia_fio.py:495`.)

- **Causa `(d)` mal atribuída.** Contraexemplo: nenhuma pergunta/chamado vivo; lista Cadastro x Realidade contém assinatura **daquele dia**, com dono `ESTRUTURA`; os outros baldes não se aplicam. O dia não sai pelo balde do admin, pois este exige `BATIDA/CADASTRO`; depois recebe `(d)` apenas porque o colaborador aparece na lista. Isso contradiz a definição local de `(d)`, que exige o **dia fora da lista**. Falta conferir a ausência de `(cid, dt)` no mapa dos dias. (`app/chamados/services/vigia_fio.py:418`, `app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:569`.)

- **Competências omitidas ou indevidamente incluídas.** O enumerador considera somente a atual e a anterior; verifica “entregue” apenas para a anterior. Assim, competência mais antiga ainda não entregue desaparece; a atual já entregue continua elegível. Isso contraria a formulação ampla “competência não entregue”, embora seja o recorte do enumerador escolhido. (`app/ponto/services/he_pendente_lavrado.py:220`, `app/ponto/services/he_pendente_lavrado.py:223`, `app/chamados/services/vigia_fio.py:467`.)

- **Outros limites herdados do universo:** entram somente colaboradores cujo motivo principal é `furo_espelho`. Um colaborador com dia acusando e `sem_codigo_dominio` fica fora; quem não tem `FechamentoMensal` também fica fora. Portanto, não é um censo de todos os impedimentos da folha: é a fatia por dias publicada em `fora_da_folha()['colabs']`. (`app/folha/fora_da_folha.py:129`, `app/folha/export.py:766`, `app/folha/export.py:846`, `app/chamados/services/vigia_fio.py:469`.)

- **Porta aceita sem conferir sua pergunta.** Qualquer chamado da célula na fila encerra a análise antes de verificar resposta vazia, app ou âncora. Repetir o teste `(b)` com `celula=True` produz `porta_fila`, não `(b)`. Isso segue a definição local de porta direta, mas não demonstra que existe pergunta utilizável; é uma limitação diante do critério mais estrito de pergunta alcançável. (`app/chamados/services/vigia_fio.py:412`, `app/chamados/services/vigia_fio.py:532`, `app/chamados/services/vigia_fio.py:535`, `app/chamados/tests/test_o261e_f5_alcance.py:142`.)

- **Não encontrei duplicação lógica de dia:** a chave é `(colaborador, data)`, com `setdefault`, e cada ramo termina em `continue`. Várias perguntas/chamados não multiplicam o dia; eventual sobreposição entre competências conserva apenas o primeiro rótulo. A unicidade da célula também é por colaborador/data. (`app/chamados/services/vigia_fio.py:465`, `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:529`, `app/escala/models.py:1788`.)

**2. Quem escreve e quem lê**

A tabela identifica os escritores encontrados e os leitores relevantes ao caminho auditado. Exaustividade de **todos** os leitores da aplicação: **[não verificado]**.

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.veredito` | `lavrar_veredito` delega à porta; atribuição real na porta da célula. (`app/escala/models.py:1775`, `app/ponto/portas/celula.py:692`.) | Retenção da folha, censo antigo e juiz de vitalidade. (`app/folha/export.py:1215`, `app/chamados/services/vigia_fio.py:267`, `app/chamados/juizes.py:1054`.) |
| `CelulaDia.colaborador`, `data` | Nascimento/regeneração pela porta da célula. (`app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:593`.) | Alcance monta identidade `(colaborador,data)`; censo antigo usa colaborador/data nos baldes. (`app/chamados/services/vigia_fio.py:480`, `app/chamados/services/vigia_fio.py:328`.) |
| `PerguntaDisputa.respondida_em`, `resposta_colab` | Resposta humana; resposta automática; reabertura limpa ambos; materialização também pode carimbar resposta. (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1392`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/materializacao.py:513`.) | Juiz distingue carimbo com texto; alcance detecta vazio e causas; folha lê vitalidade/estado. (`app/chamados/juizes.py:1020`, `app/chamados/services/vigia_fio.py:526`, `app/chamados/services/vigia_fio.py:564`, `app/folha/fora_da_folha.py:66`.) |
| `PerguntaDisputa.validada_em` | Validação humana; devolução/correção/reabertura limpam; materialização autovalida. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/services/disputa_emissao.py:1260`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/materializacao.py:515`.) | Juiz mata pergunta validada; alcance distingue `(b)/(c)`; estado declarado também lê o campo. (`app/chamados/juizes.py:1006`, `app/chamados/services/vigia_fio.py:564`, `app/chamados/services/vigia_fio.py:566`, `app/chamados/models.py:2204`.) |
| `PerguntaDisputa.chamado_destino` | Nascimento/reapontamento pela identidade e religação pelo serviço do fio; exclusão do destino pode produzir `NULL`. (`app/chamados/models.py:2165`, `app/chamados/models.py:2182`, `app/chamados/services/fio.py:797`, `app/chamados/models.py:2379`.) | `chamado_que_lista`: destino, senão container da disputa; alcance usa essa autoridade. (`app/chamados/services/fio.py:116`, `app/chamados/services/vigia_fio.py:500`, `app/chamados/services/vigia_fio.py:535`.) |
| `PerguntaDisputa.celula` | Nascimento/reapontamento pela identidade; FK permite `NULL` e `SET_NULL`. (`app/chamados/models.py:2168`, `app/chamados/models.py:2182`, `app/chamados/models.py:1943`.) | Agrupamento do alcance e juiz de vitalidade. (`app/chamados/services/vigia_fio.py:495`, `app/chamados/juizes.py:1017`.) |
| `ChamadoColaborador.status_local` | `_gravar_estado`; nascimento recebe status. (`app/chamados/models.py:192`, `app/chamados/models.py:851`.) | Fila usa `COBRANCA`; alcance usa `VIVOS`, status das âncoras e `TERMINAIS`. (`app/chamados/services/painel.py:187`, `app/chamados/services/vigia_fio.py:491`, `app/chamados/services/vigia_fio.py:501`, `app/chamados/services/vigia_fio.py:566`.) |
| `ChamadoColaborador.celula` | Nascimento resolve a célula; existentes passam por `apontar_celula`. (`app/chamados/models.py:830`, `app/chamados/services/vinculos.py:98`.) | Fila é projetada em células; essa associação basta para `porta_fila`. (`app/chamados/services/vigia_fio.py:486`, `app/chamados/services/vigia_fio.py:532`.) |
| `ChamadoColaborador.silenciado_ate` | Porta `silenciar` escreve; `levantar` limpa. (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:201`.) | `qs_na_fila` exclui prazo futuro; alcance consome seu resultado. (`app/chamados/services/painel.py:188`, `app/chamados/services/vigia_fio.py:486`.) |

A afirmação documental “escritor único de `validada_em`” não descreve todas as atribuições existentes: há autovalidação e limpeza em outros arquivos, inclusive enumerados pelo contrato. Sua introdução neste commit é **[não verificado]**. (`app/docs/JUIZES.md:38`, `app/chamados/services/materializacao.py:515`, `app/core/tests/test_contract_escritores_estado.py:183`.)

Quanto ao carimbo:

- **Escritor:** o command compõe `censo.fio_morto_alcance`, o dict completo `alcance` e `esperado`; `_carimbar` grava `.novo` e faz `os.replace`. Só carimba frota, antes da saída por alarme. (`app/chamados/management/commands/vigiar_fio_morto.py:201`, `app/chamados/management/commands/vigiar_fio_morto.py:211`, `app/chamados/management/commands/vigiar_fio_morto.py:130`, `app/chamados/management/commands/vigiar_fio_morto.py:218`, `app/chamados/management/commands/vigiar_fio_morto.py:222`.)
- **Leitor afetado:** `bin/fio_estado.py` percorre `esperado`; logo o novo contador passa automaticamente a aparecer em “FORA DO ESPERADO”. O título e o aviso CEGO continuam antigos; o dict completo `alcance` não é consumido por esse leitor. (`bin/fio_estado.py:69`, `bin/fio_estado.py:101`, `bin/fio_estado.py:105`, `bin/fio_estado.py:109`.)
- **O retorno de `censo()` não recebe a chave nova:** ela é acrescentada somente à cópia para JSON. O materializador continua consultando `censo()` antigo. (`app/chamados/management/commands/vigiar_fio_morto.py:211`, `app/chamados/management/commands/materializar_passivo_validado.py:201`.)
- **Alarme permanece antigo:** `gritou` é calculado antes de `censo_alcance`; nem `--alarme-passivo` inclui o novo contador. O teste exige essa exclusão. (`app/chamados/management/commands/vigiar_fio_morto.py:163`, `app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/tests/test_o261e_f5_alcance.py:230`.)

**3. Extração de `dono_fora_do_fio` versus pai**

**Equivalência linha a linha e ausência de alteração para qualquer entrada: [não verificado]**, pela indisponibilidade do pai.

Na árvore atual, a ordem é: decisão pendente → ausência aberta → folga contestada validada → lista com dono BATIDA/CADASTRO → nenhum balde. `censo()` chama a função depois de excluir fio vivo e hoje/futuro; cada retorno incrementa seu contador correspondente. Isso permite descrever a implementação atual, mas não provar extração literal. (`app/chamados/services/vigia_fio.py:194`, `app/chamados/services/vigia_fio.py:196`, `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:201`, `app/chamados/services/vigia_fio.py:320`, `app/chamados/services/vigia_fio.py:328`.)

**4. Checklist das classes de reprovação**

| Classe | Verificação |
|---|---|
| **Segundo juiz** | Vitalidade, vida/cobrança e Resolver dia continuam delegados às autoridades existentes; os registros apontam para elas. Os defeitos encontrados são de seleção/alcance e classificação do instrumento. (`app/chamados/services/vigia_fio.py:441`, `app/chamados/services/vigia_fio.py:194`, `app/core/juizes.py:233`, `app/core/juizes.py:238`, `app/core/juizes.py:684`.) |
| **Segundo escritor fora da porta** | Não identifiquei atribuição aos campos de negócio no censo novo; ele consulta e monta estruturas Python. A escrita durável identificada é o carimbo. (`app/chamados/services/vigia_fio.py:464`, `app/chamados/services/vigia_fio.py:583`, `app/chamados/management/commands/vigiar_fio_morto.py:130`.) |
| **Relógio fora da borda** | **Presente no caminho:** command captura um instante, mas export relê `localdate`, fila relê `now` e calendário relê `now`. `hoje` injetado não controla toda a medição; passagem da meia-noite pode misturar datas. (`app/chamados/management/commands/vigiar_fio_morto.py:161`, `app/folha/export.py:1179`, `app/chamados/services/painel.py:188`, `app/colaboradores/services/calendario.py:258`.) |
| **Derivação paralela** | Retenção vem de `fora_da_folha`, porta administrativa de `qs_na_fila`, app de `perguntas_do_app`, âncora de `chamado_que_lista`. Contudo, a junção exclusivamente por célula deixa de reconhecer portas admitidas por essas fontes. (`app/chamados/services/vigia_fio.py:469`, `app/chamados/services/vigia_fio.py:486`, `app/chamados/services/vigia_fio.py:506`, `app/chamados/services/vigia_fio.py:495`.) |
| **Selo/RED sem mordida** | Há par positivo real sem porta/com porta. Porém o caso ESTRUTURA já existente só confere total, deixando passar a causa errada; o teste `(b)` evita o atalho da célula. Pela leitura, também falta negativo que exija recusar pergunta fora do app. Execução de mutantes: **[não verificado]**. (`app/chamados/tests/test_o261e_f5_alcance.py:105`, `app/chamados/tests/test_o261e_f5_alcance.py:114`, `app/chamados/tests/test_o261e_f5_alcance.py:179`, `app/chamados/tests/test_o261e_f5_alcance.py:142`, `app/chamados/tests/test_o261e_f5_alcance.py:121`.) |
| **Ordem de trava** | Não há trava explícita no censo novo; há leituras sucessivas de universo, fila, perguntas e lista. Consistência durante alterações concorrentes: **[não verificado]**. (`app/chamados/services/vigia_fio.py:464`, `app/chamados/services/vigia_fio.py:486`, `app/chamados/services/vigia_fio.py:495`, `app/chamados/services/vigia_fio.py:517`.) |
| **Pouso antes do pré-requisito** | A ordem documentada é F1→F2→F3→F4→F5, e a cópia ainda registra F1 em construção. O código condiciona ativar o alarme novo ao zeramento por F1–F4. Sequência real de pousos/cumprimento: **[não verificado]**. (`app/docs/BACKLOG.md:776`, `app/chamados/management/commands/vigiar_fio_morto.py:91`.) |

Há ainda **cegueira nova sem cobrança no painel/saída**: `alcance_cego` é calculado e impresso, mas não integra os grupos de alarme nem o CEGO lido pelo painel. (`app/chamados/services/vigia_fio.py:473`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/management/commands/vigiar_fio_morto.py:80`, `bin/fio_estado.py:91`.)

**5. Custo**

**Não existe quantidade constante de queries por dia.** A contagem executada e o tempo na frota são **[não verificado]**; o código mostra os seguintes componentes:

- **Empresa × competência:** uma chamada de `fora_da_folha`, que classifica o export e consulta novamente os dias dos retidos. O trabalho inclui colaboradores que não acabam no universo final. (`app/chamados/services/vigia_fio.py:467`, `app/folha/fora_da_folha.py:120`, `app/folha/fora_da_folha.py:139`.)
- **Após montar o universo:** seis consultas explícitas em lote — células, fila inteira, chamados vivos, perguntas, colaboradores e âncoras; conjuntos vazios ou particionamento do ORM podem mudar o número executado. A busca de células usa produto de colaboradores × datas, não somente os pares retidos. (`app/chamados/services/vigia_fio.py:480`, `app/chamados/services/vigia_fio.py:486`, `app/chamados/services/vigia_fio.py:491`, `app/chamados/services/vigia_fio.py:495`, `app/chamados/services/vigia_fio.py:499`, `app/chamados/services/vigia_fio.py:501`.)
- **Por pergunta:** já existe N+1 na formação do universo: `_alimentar` carrega somente disputa; `.viva` acessa a célula na chave do memo. O juiz ainda pode carregar colaborador/empresa para a tranca. Essas avaliações reaparecem sobre novas instâncias no alcance/app. (`app/folha/fora_da_folha.py:51`, `app/folha/fora_da_folha.py:66`, `app/chamados/juizes.py:757`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:896`.)
- **Por colaborador que chega ao ramo do app:** uma busca de todas as perguntas candidatas dele, seguida do juiz; cache por colaborador, sem recorte aos dias do universo. (`app/chamados/services/vigia_fio.py:504`, `app/chamados/services/pergunta_no_app.py:42`.)
- **Por dia passado sem porta:** montagem do calendário completo de um dia; se não oferecer Resolver dia, consulta adicional de ausência. A lista Cadastro x Realidade é carregada no máximo uma vez por censo, com custo interno de frota. (`app/chamados/services/vigia_fio.py:194`, `app/colaboradores/services/calendario.py:213`, `app/chamados/services/vigia_fio.py:196`, `app/ponto/turnos.py:2525`, `app/chamados/services/vigia_fio.py:514`.)

## 30/10 21:03 · sessao db7f861d99cf · - · - · origem=Ronald (tmux)

Achados consolidados, apenas por leitura:

- **Porta real perdida:** pergunta com `celula_id=None`, `data_evento` igual ao dia retido, resposta preenchida, não validada, destino aberto sem célula na fila. O modelo permite essa FK nula; a verdade temporal declarada é `data_evento`. O app seleciona por colaborador, sem exigir célula; a âncora usa o destino. O censo, porém, busca perguntas exclusivamente por `celula_id__in`, perde a pergunta e pode contar morto. Use resposta `07:00` para garantir vida pelo ramo da resposta, sem depender do fallback de janela. Ocorrência em produção [não verificado]. (`app/chamados/models.py:1943`, `app/chamados/models.py:1952`, `app/chamados/models.py:1965`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1032`, `app/chamados/services/pergunta_no_app.py:42`, `app/chamados/services/fio.py:117`, `app/chamados/services/vigia_fio.py:495`.)

- **Causa `(d)` errada:** lista contém assinatura do próprio DIA com dono `ESTRUTURA`, nenhuma pergunta viva, nenhum chamado vivo. `_do_admin` não isenta, mas a presença do colaborador na lista basta para classificar `(d)`, embora o comentário exija DIA fora da lista. O teste existente já monta essa entrada, mas só exige total morto=1; não verifica a causa. (`app/chamados/services/vigia_fio.py:418`, `app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:569`, `app/chamados/tests/test_o261e_f5_alcance.py:179`.)

- **Atalho de porta a conferir contra o aval:** qualquer chamado ligado à célula na fila encerra a classificação antes de conferir perguntas/app/resposta vazia. Repetir o teste `(b)` com `celula=True` produziria porta_fila=1 e morto=0. Isso segue o contrato local, que aceita chamado direto como porta, mas é mais amplo que exigir pergunta viva listada e visível no app. (`app/chamados/services/vigia_fio.py:412`, `app/chamados/services/vigia_fio.py:532`, `app/chamados/services/vigia_fio.py:535`, `app/chamados/tests/test_o261e_f5_alcance.py:141`, `app/chamados/tests/test_o261e_f5_alcance.py:65`.)

- **Duplicação de dia:** o mapa usa `(colab_id, data)` com `setdefault`; múltiplas perguntas, chamados ou competências enumeradas não multiplicam esse dia. A competição pode apenas manter o primeiro rótulo de competência. (`app/chamados/services/vigia_fio.py:465`, `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:529`.)

- **Escopo de universo:** só competência atual e anterior não entregue; a atual entra sem perguntar se foi entregue. Só colaboradores cujo motivo principal do export é `furo_espelho`; sem código, férias com batida e cadastro zero têm precedência e ficam fora dessa lista. Sem fechamento mensal também não entra no universo do export. São limites herdados das fontes escolhidas; eventual violação depende da interpretação de “universo do retrato”, cujo módulo inclui todos os itens do export. (`app/ponto/services/he_pendente_lavrado.py:220`, `app/ponto/services/he_pendente_lavrado.py:223`, `app/folha/fora_da_folha.py:129`, `app/folha/export.py:763`, `app/folha/export.py:846`, `app/folha/export.py:850`, `app/folha/export.py:876`, `app/folha/retrato_folha.py:9`.)

Custo:

- Após formar o universo, há **6 consultas explícitas em lote**: células, fila inteira, chamados vivos das células, perguntas das células, colaboradores e status das âncoras. Consulta com conjunto vazio pode ser eliminada pelo ORM; contagem executada [não verificado]. (`app/chamados/services/vigia_fio.py:480`, `:486`, `:491`, `:495`, `:499`, `:501`.)
- `no_app` roda uma vez por colaborador que chega ao ramo, carregando **todas** suas perguntas candidatas e avaliando o juiz; não se limita aos dias do universo. (`app/chamados/services/vigia_fio.py:504`, `app/chamados/services/pergunta_no_app.py:42`.)
- Cada dia passado sem porta monta calendário de um dia; se não resolver, consulta ausência com `.first()`. Calendário faz leituras de turnos, retratadas, escala, DiaPago, células, grade e vínculos. Portanto não existe número fixo de queries por dia. (`app/chamados/services/vigia_fio.py:194`, `:196`, `app/colaboradores/services/calendario.py:213`, `:316`, `:332`, `:351`, `:385`, `:410`, `:428`, `:440`, `app/ponto/turnos.py:2525`.)
- Há **N+1 por pergunta já na formação do universo**: `_alimentar` faz `select_related('disputa')`, `_fio_do_dia` avalia todas as `.viva`, e a chave do memo acessa `p.celula`, não carregada. Para pergunta não validada com data, a tranca ainda acessa colaborador/empresa ausentes do `select_related`. Depois o novo censo e `perguntas_do_app` criam outras instâncias e repetem avaliações. (`app/folha/fora_da_folha.py:51`, `:66`, `app/chamados/models.py:2369`, `app/chamados/juizes.py:757`, `:888`, `:896`, `app/chamados/services/vigia_fio.py:495`, `app/chamados/services/pergunta_no_app.py:43`.)
- A lista Cadastro x Realidade é chamada no máximo uma vez por censo, com empresas visíveis; seu custo interno é de frota. (`app/chamados/services/vigia_fio.py:514`, `:517`, `:523`.)
- O relógio recebido em `hoje` não chega às dependências: export lê `localdate()`, fila lê `timezone.now()`, calendário lê outro `now()`. Uma execução histórica ou que atravesse a meia-noite pode misturar retratos temporais; ocorrência real [não verificado]. (`app/chamados/services/vigia_fio.py:453`, `app/folha/export.py:1179`, `app/chamados/services/painel.py:188`, `app/colaboradores/services/calendario.py:258`.)

## 30/10 21:02 · sessao 5fdabf740032 · - · - · origem=Ronald (tmux)

Rastreio dos campos, por escrita real e leitura relevante ao alcance:

| Campo | Quem escreve | Quem lê no caminho auditado |
|---|---|---|
| `CelulaDia.veredito` | `CelulaDia.lavrar_veredito` delega à porta; atribuição real em `ponto/portas/celula.py:692`; delegação em `escala/models.py:1775`. Cartório chama a porta em `ponto/services/cartorio.py:1409`. | Juiz de vitalidade em `chamados/juizes.py:1054`; `dono_fora_do_fio` lê indiretamente via resolver/estado, `chamados/services/vigia_fio.py:194` e `:199`. |
| `CelulaDia.colaborador`, `data` | Nascimento na regeneração, `ponto/portas/celula.py:247`, e geração por período, `:593`; dupla única em `escala/models.py:1788`. | Alcance monta o mapa `(colaborador,data)→pk`, `chamados/services/vigia_fio.py:480`; porta da identidade busca a célula da pergunta, `chamados/models.py:2156`. |
| `PerguntaDisputa.respondida_em`, `resposta_colab` | Resposta humana em `chamados/services/disputa_emissao.py:1247`; automática em `:1392`; reconstrução do carimbo pela trilha em `:1409`; reabertura zera ambos em `:1358`, salva em `:1377`. `respondida_em` também recebe carimbo condicionado a texto em `chamados/services/materializacao.py:513`, `:1192`, `chamados/services/disputa_emissao.py:1572`, `:1657`. Ressuscitação limpa carimbo em `chamados/models.py:1258`. | Alcance detecta vazio em `chamados/services/vigia_fio.py:526` e atribui causas em `:564`; juiz exige carimbo+texto em `chamados/juizes.py:1020`; SQL generoso do app usa ambos em `chamados/models.py:2342`. |
| `PerguntaDisputa.validada_em` | Validação humana em `chamados/services/validacao.py:131`; devolução limpa em `:188`; correção de resposta limpa em `chamados/services/disputa_emissao.py:1260`; reabertura em `:1358`; autovalidação em `chamados/services/materializacao.py:515`, `:1195`, `chamados/services/disputa_emissao.py:1575`; ressuscitação em `chamados/models.py:1258`. Os quatro arquivos são enumerados no contrato em `core/tests/test_contract_escritores_estado.py:183`. | Juiz mata a pergunta validada em `chamados/juizes.py:1006`; alcance consulta em `chamados/services/vigia_fio.py:564` e `:566`; estado declarado consulta em `chamados/models.py:2204`. |
| `PerguntaDisputa.chamado_destino` | Nascimento/identidade em `chamados/models.py:2165` e `:2182`; reapontamento na porta em `chamados/services/fio.py:797`, com relavra e save em `:798`. FK `SET_NULL` também permite limpeza quando o destino é apagado, `chamados/models.py:2379`. | Âncora única `chamado_que_lista`, `chamados/services/fio.py:116`; alcance usa a âncora em `chamados/services/vigia_fio.py:500`, `:535`, `:557`, `:566`. |
| `PerguntaDisputa.celula` | Identidade reaponta em `chamados/models.py:2168`; nascimento em `:2182`; FK nullable em `:1943`. | Alcance agrupa exclusivamente por essa FK, `chamados/services/vigia_fio.py:495`; juiz lê célula em `chamados/juizes.py:1017`; folha consulta `.viva`, `folha/fora_da_folha.py:66`. |
| `ChamadoColaborador.status_local` | Núcleo de escrita `_gravar_estado`, `chamados/models.py:185`, atribui em `:192`; nascimento passa `status_local=status`, `:851`. | Alcance mede vivos em `chamados/services/vigia_fio.py:491` e status das âncoras em `:501`; fila lê `COBRANCA`, `chamados/services/painel.py:187`. |
| `ChamadoColaborador.celula` | Nascimento resolve em `chamados/models.py:830`; existente passa por `apontar_celula`, `chamados/services/vinculos.py:92`, atribui e salva em `:98`; vínculo existente é preservado por `:95`. | Alcance considera qualquer chamado da fila com essa célula uma porta em `chamados/services/vigia_fio.py:486` e `:532`; folha usa vivos por célula em `folha/fora_da_folha.py:198`. |
| `ChamadoColaborador.silenciado_ate` | Porta `silenciar`, `chamados/services/arquivo.py:141`, escreve em `:166`; `levantar`, `:176`, limpa em `:201`; contrato enumera somente esse arquivo em `core/tests/test_contract_escritores_estado.py:35`. | Alcance não lê diretamente: pergunta à fila, `chamados/services/vigia_fio.py:486`; fila exclui prazo futuro usando relógio real em `chamados/services/painel.py:188`. |

Não reduzir `validada_em` a “escritor único `validar_pergunta`”: existem escritores de autovalidação e limpeza no código, além da validação humana (`chamados/services/materializacao.py:515`, `chamados/services/disputa_emissao.py:1260`, `chamados/models.py:1258`).

`perguntas_do_app` usa o SQL generoso e confirma cada pergunta pelo juiz, `chamados/services/pergunta_no_app.py:42`; `_fio_do_dia` da folha usa `.viva`, `folha/fora_da_folha.py:66`; a property delega ao mesmo juiz, `chamados/models.py:2369`. O alcance não substitui esses leitores: consome seus resultados em `chamados/services/vigia_fio.py:469`, `:497`, `:506`.

A afirmação de que esses são **todos** os leitores de cada campo em toda a aplicação permanece [não verificado]; são os leitores e escritores identificados no caminho auditado, com o contrato de escritores como apoio em `core/tests/test_contract_escritores_estado.py:15`.

## 30/10 21:02 · sessao 0a20a2e83285 · - · - · origem=Ronald (tmux)

- **Leitor muda automaticamente:** `bin/fio_estado.py:69-73` percorre `esperado`, comparando cada chave com `censo`; portanto `fio_morto_alcance != 0` passa a aparecer em “FORA DO ESPERADO” (`bin/fio_estado.py:105-108`). `bin/gerar_estado.py:260-261` publica essa composição. O cabeçalho continua exibindo `fio_morto` antigo e o CEGO continua sendo o antigo (`bin/fio_estado.py:101-111`).

- **Alarme continua antigo:** `gritou` deriva exclusivamente de `censo()` e das constantes antigas (`app/chamados/management/commands/vigiar_fio_morto.py:80-81`, `:163-165`); `censo_alcance()` roda depois (`:181`) e só entra na impressão/carimbo (`:182-183`, `:211-213`). Nem `--alarme-passivo` cobra `fio_morto_alcance`; o teste exige sua exclusão (`app/chamados/tests/test_o261e_f5_alcance.py:230-231`). O cron já executa com `--alarme --alarme-passivo` (`app/config/crons.py:1654-1656`). Assim, o observável novo é o painel; o código de saída permanece guiado pelo contador antigo.

- **CEGO novo sem cobrança:** `alcance_cego` nasce quando universo novo é vazio (`app/chamados/services/vigia_fio.py:472-475`) e só é impresso (`:594-599`); não está em `ESPERADO_DO_CARIMBO` nem nos grupos de alarme (`app/chamados/management/commands/vigiar_fio_morto.py:80-94`). Contraexemplo estrutural: universo antigo com lâmpadas, universo novo vazio ⇒ novo CEGO apenas no dict/stdout, sem aviso CEGO do painel (`bin/fio_estado.py:91`, `:109-111`).

- **Escritor do carimbo:** command compõe `censo.fio_morto_alcance`, `alcance` e `esperado` (`app/chamados/management/commands/vigiar_fio_morto.py:201-214`), `_carimbar()` escreve `.novo` e faz `os.replace` (`:125-132`), somente em frota (`:200`, `:218-220`), antes do `SystemExit` (`:222-223`). O novo contador não é adicionado ao retorno de `censo()`; é adicionado à cópia usada no JSON (`:211-212`). O outro consumidor de `censo()` encontrado, `materializar_passivo_validado._juizes()`, continua lendo a função antiga e selecionando seus inteiros/bools (`app/chamados/management/commands/materializar_passivo_validado.py:198-208`).

- **Relógio não fica único na rodada:** command captura `agora/hoje` (`app/chamados/management/commands/vigiar_fio_morto.py:157-162`), mas `censo_alcance` chama `qs_na_fila()` sem instante (`app/chamados/services/vigia_fio.py:486`) e esta lê `timezone.now()` (`app/chamados/services/painel.py:186-188`). `dono_fora_do_fio` chama o calendário sem `hoje` (`app/chamados/services/vigia_fio.py:194`); a porta curta monta contexto (`app/colaboradores/services/calendario.py:208-216`) que relê relógio (`:254-259`). Uma rodada atravessando meia-noite ou vencimento de silêncio pode misturar instantes; ocorrência real [não verificado]. Freezegun fixa todos os testes F5 no mesmo instante (`app/chamados/tests/test_o261e_f5_alcance.py:30`).

- **Juízes existentes são consultados:** VIVOS/C6/fio_vivo estão registrados (`app/core/juizes.py:233-247`); alcance importa VIVOS/TERMINAIS, `pergunta_viva`, `chamado_que_lista`, `qs_na_fila`, `perguntas_do_app` e `fora_da_folha` (`app/chamados/services/vigia_fio.py:441-451`). `dono_fora_do_fio` consulta calendário, ausência e aceite de folga, sem gravar aqueles modelos (`:190-203`). O arquivo de consulta também nomeia validação, fio, vida e cobrança (`app/docs/JUIZES.md:37-54`). Não identifiquei segundo escritor dos campos de negócio nesse trecho; o escritor visível é o carimbo (`app/chamados/management/commands/vigiar_fio_morto.py:125-132`).

- **Mordida e lacunas:** F5 tem +1 real e par com porta 0 (`app/chamados/tests/test_o261e_f5_alcance.py:105-127`), causas a–e (`:129-172`), universo vazio e competência entregue (`:201-217`), publicação com valor literal 1 (`:219-229`). Porém o teste do carimbo antigo compara resultado com nova chamada da mesma função, demonstrando transporte, não correção da medida (`app/chamados/tests/test_o261_vigia_fio.py:780-782`). Não existe caso F5 com pergunta viva cuja âncora está na fila **e que não aparece no app**: o único caso de porta por pergunta é positivo (`app/chamados/tests/test_o261e_f5_alcance.py:121-127`). A mutação que remove `p.pk in no_app(cid)` da condição (`app/chamados/services/vigia_fio.py:535`) parece escapar dessa suíte; execução da mutação [não verificado].

- **Ordem de trava/pouso:** o trecho novo faz consultas e incrementa estruturas Python (`app/chamados/services/vigia_fio.py:464-589`), sem ordem nova de travas explícita; coerência sob alterações concorrentes [não verificado]. A condição de ativar o novo alarme somente após F1–F4 zerarem está escrita (`app/chamados/management/commands/vigiar_fio_morto.py:91-94`); cumprimento do pré-requisito em produção e sequência real de pousos [não verificado]. Comparação literal com pai/diff [não verificado], pois não há repositório Git acessível nesta cópia.

## 30/10 20:53 · sessao 49a3dd0c868b · O261-E-CELULA-20 · leitura · origem=Code

A medida pedida **não coincide com o `fio_morto` atual**: a folha parte dos colaboradores classificados como `furo_espelho`; o censo parte de chamados vivos com célula, sem filtrar competência entregue. Além disso, “pergunta viva”, “chamado na fila” e “porta de validação no fio” têm seleções diferentes. (`app/folha/fora_da_folha.py:120`, `app/folha/fora_da_folha.py:129`, `app/chamados/services/vigia_fio.py:185`, `app/chamados/services/painel.py:186`, `app/chamados/services/fio.py:594`)

Os custos abaixo são inferidos do código. Quantidades observadas de queries, estado do banco e implantação em produção: **[nao verificado]**.

**1) Assinaturas, retornos, custos e chamadores**

Para as fórmulas: `C` é o custo de `classificar_export(empresa, mes, ano)`; `R(c)` é o custo de `motivos_retencao_celula(c, ..., com_dias=True)`; `J` é o custo adicional dos juízes de vitalidade. Esses caminhos são chamados pelas ferramentas da folha. (`app/folha/fora_da_folha.py:93`, `app/folha/fora_da_folha.py:120`, `app/folha/fora_da_folha.py:66`)

| Função | Retorno | Custo por chamada |
|---|---|---|
| `fora_da_folha(empresa, mes, ano, com_itens=False)` | Dict com `competencia, empresa, total, entram, fora_por_motivo, furo_espelho, grupos, dias_por_fio, colabs`; opcionalmente `itens`. `colabs` contém somente classificados por `furo_espelho`. | `C + carga dos colaboradores + Σ R(c) + duas cargas de fio + J`. Depois da classificação, relê os dias de cada colaborador retido por furo. (`app/folha/fora_da_folha.py:97`, `app/folha/fora_da_folha.py:129`, `app/folha/fora_da_folha.py:132`, `app/folha/fora_da_folha.py:139`, `app/folha/fora_da_folha.py:142`, `app/folha/fora_da_folha.py:170`) |
| `o_que_segura(colab, mes, ano)` | Dict com `competencia, colab, nome, entra, motivo, grupo, motivos_retencao, dias`; cada dia traz `data, veredito, motivo, fio, destrava, chamado`. | `C da empresa inteira + R(colab) + duas cargas de fio + outra carga de chamados + J`; pode carregar `colab.empresa`. Chamá-la para N colaboradores repete N vezes a classificação inteira da empresa. (`app/folha/fora_da_folha.py:179`, `app/folha/fora_da_folha.py:188`, `app/folha/fora_da_folha.py:191`, `app/folha/fora_da_folha.py:198`, `app/folha/fora_da_folha.py:209`) |
| `chamado_que_lista(p, disputa=None)` | ID do destino; na ausência, ID do chamado da disputa; na ausência de ambos, `None`. | Zero queries com disputa fornecida/carregada ou sem `disputa_id`; uma para carregar `p.disputa` caso necessário. Carrega a disputa **antes** de retornar o destino, mesmo quando ele existe. (`app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:116`) |
| `qs_na_fila(f_urgencia='', f_q='', f_colab='', f_empresa='', f_praca='', _base=None)` | `QuerySet` lazy: `VIVOS ∩ COBRANCA`, excluindo silêncio futuro. | Zero ao construir; `.count()` normalmente uma query; materialização padrão não vazia normalmente duas: principal e prefetch de `followups`. `_base` e projeções `values/values_list` alteram esse custo. Não há laço interno por colaborador. (`app/chamados/services/painel.py:98`, `app/chamados/services/painel.py:153`, `app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:186`) |
| `perguntas_do_app(colab)` | Lista de `PerguntaDisputa`, ordenada por `id`, após `q_vivas()` e juiz Python. | Uma query inicial **mais custo variável por pergunta**; só `disputa` vem por `select_related`. Célula, colaborador, empresa, tranca e outros fatos podem gerar consultas adicionais. (`app/chamados/services/pergunta_no_app.py:39`, `app/chamados/juizes.py:757`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:1066`) |

`R(c)` normalmente tem duas consultas: células e vínculos do período; as saídas por isenção ou janela colapsada antecedem essas consultas. (`app/folha/export.py:1174`, `app/folha/export.py:1184`, `app/folha/export.py:1188`, `app/folha/export.py:1194`, `app/escala/alimentacao.py:131`, `app/escala/alimentacao.py:153`)

`C` não é constante: percorre os fechamentos da empresa e pode consultar batidas, escala, previsto, retenção e homologação por colaborador. O cruzamento de férias também pode carregar `periodo_aquisitivo` por agendamento e consultar batidas por colaborador com férias. (`app/folha/export.py:766`, `app/folha/export.py:787`, `app/folha/export.py:888`, `app/folha/export.py:912`, `app/folha/export.py:942`, `app/folha/export.py:948`, `app/folha/export.py:480`, `app/folha/export.py:488`, `app/ferias/services.py:51`)

As duas cargas de `_alimentar` **não tornam o custo total igual a duas queries**: `_fio_do_dia` chama `p.viva` e `p.estado`; o juiz acessa relações e pode consultar tranca e outros juízes. Há memo por instância de pergunta; a tranca usa memo por request, ou por instância do colaborador fora de request. (`app/folha/fora_da_folha.py:51`, `app/folha/fora_da_folha.py:66`, `app/chamados/juizes.py:791`, `app/chamados/juizes.py:893`, `app/chamados/juizes.py:919`)

Chamadores diretos localizados fora dos testes — execução efetiva em produção: **[nao verificado]**: [nao verificado]

- `fora_da_folha`: `montar_linhas`, com `com_itens=True`; o command `lavrar_retrato_folha` chama esse montador. (`app/folha/retrato_folha.py:1120`, `app/folha/retrato_folha.py:1157`, `app/folha/management/commands/lavrar_retrato_folha.py:65`)
- `o_que_segura`: nenhum chamador direto de produção encontrado por busca estática; chamadas dinâmicas e ausência absoluta: **[nao verificado]**.
- `chamado_que_lista`: `data_do_fato_de`, montagem do fio, validar/recusar/corrigir no fio/modal e no detalhe. (`app/chamados/services/fio.py:77`, `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:629`, `app/chamados/views.py:499`, `app/chamados/views.py:516`, `app/chamados/views.py:534`)
- `perguntas_do_app`: API do colaborador, `medir` — uma vez por colaborador — e `do_dia`. (`app/api/views.py:2604`, `app/chamados/services/pergunta_no_app.py:113`, `app/chamados/services/pergunta_no_app.py:115`, `app/chamados/services/pergunta_no_app.py:138`)
- `qs_na_fila`, telas/contadores: painel, contador do fio, três caminhos da navbar, dashboard e painel operacional. (`app/chamados/services/painel.py:210`, `app/chamados/views.py:901`, `app/core/context_processors.py:93`, `app/core/context_processors.py:150`, `app/core/context_processors.py:187`, `app/core/views.py:211`, `app/core/views.py:1193`, `app/core/services/painel_op.py:86`)
- `qs_na_fila`, serviços: fila/gestão, fila por causa, SLA, lastro, furos diários e propositor. (`app/chamados/services/fila.py:92`, `app/chamados/services/fila.py:150`, `app/chamados/services/fila_por_causa.py:158`, `app/chamados/services/sla.py:62`, `app/ponto/services/lastro.py:216`, `app/escala/services/furos_diarios.py:202`, `app/escala/services/propositor.py:579`)
- `qs_na_fila`, commands: alerta SLA, feriado do posto, nomeação de marco e cura de resíduo. (`app/chamados/management/commands/alertar_chamados_sla.py:30`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:95`, `app/chamados/management/commands/nomear_marco_por_lampada.py:47`, `app/ponto/management/commands/curar_residuo_bug115.py:72`)

**2) Mesmos dias, ausência de célula e unicidade**

São a **mesma fonte e regra**, mas listas reconstruídas em chamadas distintas: ambas usam `_dias_que_retem → motivos_retencao_celula(com_dias=True)`. `datas` projeta as datas dessa lista; `o_que_segura['dias']` acrescenta fio, ação e chamados. (`app/folha/fora_da_folha.py:90`, `app/folha/fora_da_folha.py:139`, `app/folha/fora_da_folha.py:168`, `app/folha/fora_da_folha.py:193`, `app/folha/fora_da_folha.py:203`)

A igualdade depende de mesmo colaborador, janela e estado dos dados/relógio. Há também diferença de universo: `fora_da_folha['colabs']` só inclui `furo_espelho`; `o_que_segura` calcula os dias mesmo quando a classificação do colaborador aponta outro motivo. (`app/folha/export.py:1179`, `app/folha/fora_da_folha.py:129`, `app/folha/fora_da_folha.py:191`, `app/folha/fora_da_folha.py:193`)

**Sim, existe dia retido sem `celula_id`**: dia passado sem célula, com vínculo cobrindo a data e diferente do dia de demissão, entra como `sem_celula`, com ID/veredito `None`. Sem célula e sem vínculo, o dia é ignorado. Nas duas ferramentas, ausência de ID recebe fio `nada`. (`app/folha/export.py:1203`, `app/folha/export.py:1209`, `app/folha/export.py:1211`, `app/folha/export.py:1214`, `app/folha/fora_da_folha.py:154`, `app/folha/fora_da_folha.py:204`)

`CelulaDia` declara unicidade por `(colaborador, data)` no modelo e na migração. Constraint efetivamente aplicada no banco de produção: **[nao verificado]**. (`app/escala/models.py:1788`, `app/escala/migrations/0030_celuladia.py:35`)

**3) Competências “abertas”, entendidas como não entregues**

O predicado correspondente é **`not competencia_entregue(empresa_id, mes, ano)`**: K6 exige exportação vigente e não invalidada. Default retorna bool; `registro=True` retorna o registro ou `None`. Custa uma query sem registros de exportação e duas quando existem, por causa de `anotar_vigencia`. (`app/folha/export.py:545`, `app/folha/export.py:570`, `app/folha/export.py:572`, `app/folha/export.py:573`, `app/folha/models.py:207`, `app/folha/models.py:211`)

| Função real | O que responde |
|---|---|
| `periodo_apuracao`, `periodo_apuracao_atual`, `corte_da_empresa`, `janela_fechamento`, `janela_atual`, `janela_anterior` | Geometria da competência pelo corte da empresa, clampado em 2–28; não decidem entrega. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`, `app/ponto/janelas.py:66`) |
| `folha.export.janela_competencia` | Delega a `janela_fechamento`. (`app/folha/export.py:47`) |
| `passivo_s84.competencia_de(dia, empresa)` | Mês/ano do fim da janela que contém o dia. (`app/ponto/services/passivo_s84.py:38`, `app/ponto/services/passivo_s84.py:45`) |
| `vigia_fio.competencia_do_dia_entregue(dia, empresa, memo=None)` | Compõe `janela_atual` com K6; retorna `None` sem dia; memo opcional por empresa/mês/ano. (`app/chamados/services/vigia_fio.py:455`, `app/chamados/services/vigia_fio.py:467`, `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:475`) |
| `esmeril_espelho.janela(colab, hoje=None)` | Retrocede da atual enquanto a anterior não foi entregue e existe célula do colaborador até seu fim; para na primeira entregue ou ausência de histórico; retorna intervalo até ontem. (`app/ponto/services/esmeril_espelho.py:38`, `app/ponto/services/esmeril_espelho.py:55`, `app/ponto/services/esmeril_espelho.py:58`, `app/ponto/services/esmeril_espelho.py:60`, `app/ponto/services/esmeril_espelho.py:63`) |
| `he_pendente_lavrado.competencias_a_lavrar` | Enumeração parcial: atual incondicionalmente, anterior somente se não entregue; não retrocede além dela. (`app/ponto/services/he_pendente_lavrado.py:209`, `app/ponto/services/he_pendente_lavrado.py:220`, `app/ponto/services/he_pendente_lavrado.py:223`) |
| `vigia_pausa._competencias` | Anterior e atual, sem filtrar entrega. (`app/folha/services/vigia_pausa.py:77`, `app/folha/services/vigia_pausa.py:94`) |
| `dia_pago.competencias_do_intervalo`; `fechamento.competencias_do_fato` | Enumeram janelas tocadas por intervalo; a segunda adapta os dados e delega; não filtram K6. (`app/ponto/services/dia_pago.py:700`, `app/ponto/services/dia_pago.py:712`, `app/ponto/services/fechamento.py:1541`, `app/ponto/services/fechamento.py:1567`) |

Não identifiquei enumerador geral de **todas e somente** as competências não entregues da empresa; ausência absoluta: **[nao verificado]**.

Os outros sentidos de “fechada/aberta” não são equivalentes a K6:

- `janelas.competencia_fechada` lê `FechamentoMensal` individual aprovado/exportado. (`app/ponto/janelas.py:75`, `app/ponto/janelas.py:85`, `app/ponto/janelas.py:87`)
- `periodo_trancado`/`competencia_trancada` leem tranca viva; `marcos_da_competencia` reúne aprovada, exportada, trancada e paga. (`app/ponto/services/fechamento.py:1432`, `app/ponto/services/fechamento.py:1441`, `app/ponto/services/fechamento.py:1486`)
- K9, `competencia_fechada_para_celula`, significa **K6 ou holerite publicado da pessoa**; `_competencias_exportadas` usa essa regra para regeneração. (`app/folha/export.py:578`, `app/folha/export.py:599`, `app/folha/export.py:603`, `app/ponto/management/commands/gerar_celulas.py:126`, `app/ponto/management/commands/gerar_celulas.py:179`)

**4) Pergunta visível/acionável, leitores e escritores**

Há três seleções distintas:

- **Fila administrativa:** `qs_na_fila` cobra abertos/em análise sem silêncio futuro. O crédito de pergunta para “Validar” usa `q_vivas`, exclui materializada com `VL_SIM` e separa respondidas pelo carimbo. Destino pertencente ao universo vivo ganha; senão, o crédito cai no chamado pai da disputa. (`app/chamados/services/painel.py:186`, `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:144`)
- **Fio/modal administrativo:** começa com todos os chamados do colaborador; carrega perguntas de disputas abertas e perguntas vivas de fechadas; agrupa pelo destino preenchido, ou pelo pai quando não há destino. Sem âncora, descarta a pergunta da montagem. (`app/chamados/services/fio.py:174`, `app/chamados/services/fio.py:213`, `app/chamados/services/fio.py:222`, `app/chamados/services/fio.py:236`)
- **API do colaborador:** seleciona perguntas vivas sem exigir disputa aberta, destino vivo ou ausência de silêncio. Estado `RESPONDIDA` vai a `respondidas`, com `aguarda_admin=True`; os demais vão a `perguntas`. O endereço de cada item é o destino cru; apenas o envelope tem fallback ao pai. (`app/chamados/services/pergunta_no_app.py:42`, `app/api/views.py:2622`, `app/api/views.py:2624`, `app/api/views.py:2626`, `app/api/views.py:2642`)

No modal, apresentação de validação depende da disputa, permissão e condições da pergunta. No POST, exige permissão, mesma âncora e mesmo colaborador; disputa fechada exige pergunta viva e permite validar sem reabri-la. O detalhe ainda exige disputa aberta. (`app/templates/chamados/partials/modal_fio.html:120`, `app/templates/chamados/partials/modal_fio.html:210`, `app/templates/chamados/partials/modal_fio.html:221`, `app/chamados/services/fio.py:560`, `app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:594`, `app/chamados/views.py:493`)

A validação exige `respondida_em` preenchido e `validada_em` vazio; pode exigir confirmação de implausibilidade. **O guard inicial não exige texto não vazio**. Já a API de resposta rejeita texto vazio. (`app/chamados/services/validacao.py:356`, `app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:367`, `app/api/views.py:2688`)

A vitalidade é decidida por `pergunta_viva`, não apenas pelos carimbos: validação e tranca vêm primeiro; resposta carimbada **com texto após `strip()`** pode continuar viva para o admin; depois entram decisão humana, identidade, marco no espelho, veredito, falta e materialização. `.viva` delega a esse juiz; `q_vivas` é prefiltro generoso. (`app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1039`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1066`, `app/chamados/models.py:2328`, `app/chamados/models.py:2359`)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `respondida_em`, `resposta_colab` | Resposta canônica; automática; recuperação pela trilha; reabertura limpa ambos e guarda versão anterior. Materialização/ausência/fecho também têm escritas condicionadas a texto. (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/disputa_emissao.py:1370`, `app/chamados/services/disputa_emissao.py:1392`, `app/chamados/services/disputa_emissao.py:1407`, `app/chamados/services/materializacao.py:511`, `app/chamados/services/materializacao.py:1190`, `app/chamados/services/disputa_emissao.py:1570`, `app/chamados/services/disputa_emissao.py:1655`) | Vitalidade, estado, crédito administrativo, modal e API. A fila global de validação exige carimbo e texto. (`app/chamados/juizes.py:1020`, `app/chamados/models.py:2207`, `app/chamados/services/raiox.py:135`, `app/templates/chamados/partials/modal_fio.html:210`, `app/api/views.py:2632`, `app/chamados/juizes.py:1121`) |
| `validada_em` | Carimbo canônico; devolução recusada limpa; nova resposta e reabertura limpam; simetrias de materialização também carimbam. (`app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:187`, `app/chamados/services/disputa_emissao.py:1251`, `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/materializacao.py:515`, `app/chamados/services/materializacao.py:1195`, `app/chamados/services/disputa_emissao.py:1575`) | Vitalidade, estado, prefiltro, template, validação e marcador do placar. (`app/chamados/juizes.py:1006`, `app/chamados/models.py:2204`, `app/chamados/models.py:2353`, `app/templates/chamados/partials/modal_fio.html:221`, `app/chamados/services/validacao.py:359`, `app/chamados/services/painel.py:66`) |
| `chamado_destino` | `PerguntaDisputa.abrir`, no nascimento/reapontamento; `reapontar_resolvedora`, com validação e relavra. (`app/chamados/models.py:2124`, `app/chamados/models.py:2165`, `app/chamados/models.py:2182`, `app/chamados/services/fio.py:792`, `app/chamados/services/fio.py:797`) | Âncora do fio, crédito administrativo, API e materialização. (`app/chamados/services/fio.py:116`, `app/chamados/services/raiox.py:144`, `app/api/views.py:2626`, `app/chamados/services/materializacao.py:502`) |
| `ChamadoColaborador.status_local` | Núcleo `_gravar_estado`; nascimento por `abrir/criar`; portas de transição, reconciliação, reabertura e encerramento. (`app/chamados/models.py:185`, `app/chamados/models.py:599`, `app/chamados/models.py:818`, `app/chamados/models.py:851`, `app/chamados/models.py:1115`, `app/chamados/models.py:1288`) | Fila, universo de crédito, desenlace e julgamento da materialização pelo destino. (`app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:187`, `app/chamados/services/raiox.py:54`, `app/chamados/services/desenlace.py:48`, `app/chamados/juizes.py:699`) |
| `silenciado_ate` | `arquivo.silenciar` grava; `arquivo.levantar` limpa. (`app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:176`, `app/chamados/services/arquivo.py:201`) | Fila, partição por verbo e `esta_silenciado`; a seleção do app não o consulta. (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:43`, `app/chamados/models.py:304`, `app/chamados/services/pergunta_no_app.py:39`) |

Conjuntos do catálogo:

```text
VIVOS       = aberto, em_analise, registrado
COBRANCA    = aberto, em_analise
ENCERRADOS  = resolvido, fechado
TERMINAIS   = resolvido, fechado, cancelado, superado
NAO_FECHADOS = VIVOS + resolvido
```

Fonte: `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`, `app/chamados/catalogo/motor.py:32`.

Para as causas propostas:

- **(a) Vivo com véu:** silêncio futuro tira da fila; qualquer silêncio preenchido sobrepõe o verbo para arquivado/futuro ou histórico/passado. Isso não elimina automaticamente a pergunta da API nem o chamado do fio administrativo. (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:45`, `app/chamados/services/pergunta_no_app.py:42`, `app/chamados/services/fio.py:174`)
- **(b) Carimbo com resposta vazia:** pode continuar viva, mas `.estado` a chama de `RESPONDIDA`; o app a coloca em espera pelo admin. O contador específico só mede carimbos desde o marco de 29/10/2026 20:20 UTC. (`app/chamados/juizes.py:1020`, `app/chamados/models.py:2208`, `app/api/views.py:2622`, `app/chamados/services/vigia_fio.py:405`, `app/chamados/services/vigia_fio.py:417`)
- **(c) Respondida não validada com destino fechado:** isso sozinho não pro

[truncado em 20000 de 32915 caracteres]

## 30/10 20:51 · sessao c48c20b624b7 · - · - · origem=Ronald (tmux)

**Q1 — contratos e custos, por leitura estática; contagens em execução não foram medidas [não verificado].**

| Função | Retorno e custo |
|---|---|
| `chamado_que_lista(p, disputa=None)` | Retorna `p.chamado_destino_id` ou `disputa.chamado_id` ou `None`. **0 queries** se disputa passada/FK em cache/sem `disputa_id`; **1 query** se precisa carregar `p.disputa`. Atenção: carrega a disputa **antes** de avaliar o `or`, mesmo quando já existe destino. Não tem memo próprio. (`app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:116`) |
| `qs_na_fila(f_urgencia='', f_q='', f_colab='', f_empresa='', f_praca='', _base=None)` | Retorna `QuerySet` lazy: VIVOS ∩ COBRANCA menos `silenciado_ate > now`. **0 queries ao chamar**; `.count()` normalmente **1**; materialização da base padrão normalmente **2**: principal + prefetch de followups, quando há resultados. `_base` pode mudar o prefetch; `values/values_list` evita materializar followups. Sem loop por colaborador interno. (`app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:186`, `app/chamados/services/painel.py:98`, `app/chamados/services/painel.py:153`) |
| `perguntas_do_app(colab)` | Retorna **lista** de `PerguntaDisputa`, ordenada por `id`: seleção do colaborador, `q_vivas()` e filtro Python `pergunta_viva(p)`. **1 query inicial + custo variável por pergunta**: só `disputa` está em `select_related`; juiz pode carregar célula, colaborador, empresa, tranca, trilha e outros fatos. Não há custo constante seguro. (`app/chamados/services/pergunta_no_app.py:39`, `app/chamados/juizes.py:757`, `app/chamados/juizes.py:875`, `app/chamados/juizes.py:1005`) |

No custo de `perguntas_do_app`, a tranca tem memo por **request**; fora de request, por instância do colaborador. Empresa e `(empresa, mês, ano)` são memoizadas, bem como toque humano por `(colaborador,dia,fechado_em)`. O juiz da tranca consulta uma linha por competência não memoizada; havendo tranca, busca células/chamados e até três consultas de existência à auditoria. (`app/chamados/juizes.py:893`, `app/chamados/juizes.py:919`, `app/chamados/juizes.py:937`, `app/chamados/juizes.py:947`, `app/ponto/services/fechamento.py:1432`)

Chamadores de produção localizados:

- `chamado_que_lista`: `data_do_fato_de` (`app/chamados/services/fio.py:77`), montagem do fio (`app/chamados/services/fio.py:236`), validar e recusar/corrigir no POST do fio/modal (`app/chamados/services/fio.py:592`, `app/chamados/services/fio.py:629`), validar/recusar/corrigir no detalhe (`app/chamados/views.py:499`, `app/chamados/views.py:516`, `app/chamados/views.py:534`).
- `perguntas_do_app`: API nativa (`app/api/views.py:2604`), `medir` por colaborador (`app/chamados/services/pergunta_no_app.py:115`), `do_dia` (`app/chamados/services/pergunta_no_app.py:138`).
- `qs_na_fila`: painel (`app/chamados/services/painel.py:210`), contador do fio (`app/chamados/views.py:901`), três caminhos da navbar (`app/core/context_processors.py:93`, `app/core/context_processors.py:150`, `app/core/context_processors.py:187`), dashboard (`app/core/views.py:211`, `app/core/views.py:1193`), painel operacional (`app/core/services/painel_op.py:86`), fila/gestão (`app/chamados/services/fila.py:92`, `app/chamados/services/fila.py:150`), fila por causa (`app/chamados/services/fila_por_causa.py:158`), SLA serviço/command (`app/chamados/services/sla.py:62`, `app/chamados/management/commands/alertar_chamados_sla.py:30`), lastro (`app/ponto/services/lastro.py:216`), furos diários (`app/escala/services/furos_diarios.py:202`), propositor (`app/escala/services/propositor.py:579`), commands feriado/marco/resíduo (`app/colaboradores/management/commands/marcar_feriado_do_posto.py:95`, `app/chamados/management/commands/nomear_marco_por_lampada.py:47`, `app/ponto/management/commands/curar_residuo_bug115.py:72`).

**Q4 — admin, app, estados e campos**

**Há três seleções diferentes: fila administrativa, fio/modal administrativo e API do colaborador.** A fila exclui silêncio futuro e `registrado`; o fio administrativo começa com todos os chamados do colaborador e carrega perguntas de disputas abertas mais perguntas vivas de disputas fechadas; a API começa diretamente nas perguntas vivas do colaborador. (`app/chamados/services/painel.py:186`, `app/chamados/services/fio.py:174`, `app/chamados/services/fio.py:213`, `app/chamados/services/pergunta_no_app.py:42`)

- **Fila/Validar:** `creditos_perguntas` procura perguntas pelo destino ou chamado-pai dentro dos chamados vivos; aplica `q_vivas`, exclui materializadas com `veredito_lavrado=VL_SIM`, e separa respondidas pelo **carimbo** `respondida_em`. Destino vivo recebe crédito primeiro; caso contrário, recebe o pai. Destino fechado sozinho não impede crédito; ambos fora do universo vivo deixam a pergunta sem crédito nessa fila. (`app/chamados/services/raiox.py:49`, `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:140`)
- **Véu:** `particionar_verbos` sobrepõe o verbo por `ARQUIVADO` com silêncio futuro, ou `HISTORICO` com silêncio passado. Isso é outra regra além do filtro da fila; não muda `status_local`. (`app/chamados/services/verbos.py:41`, `app/chamados/services/arquivo.py:154`)
- **Fio/modal admin:** perguntas são agrupadas por `chamado_que_lista`; quando nenhuma âncora existe, são descartadas da montagem. Disputa fechada só fornece perguntas vivas, e o bloco permite validação sem reabrir a disputa. (`app/chamados/services/fio.py:222`, `app/chamados/services/fio.py:233`, `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:594`)
- **Botão no modal:** exige `disputa_obj` e permissão em `pode_agir_map`; dentro da pergunta, `respondida_em` leva ao bloco de resposta, `pendencia_admin` e `validada_em` precedem o formulário Validar. Sem esses dois, o formulário aparece, inclusive com confirmação de implausibilidade quando aplicável. (`app/templates/chamados/partials/modal_fio.html:120`, `app/templates/chamados/partials/modal_fio.html:210`, `app/templates/chamados/partials/modal_fio.html:215`, `app/templates/chamados/partials/modal_fio.html:221`, `app/templates/chamados/partials/modal_fio.html:243`, `app/templates/chamados/partials/modal_fio.html:272`)
- **Gate POST fio/modal:** exige `pode_agir_chamado`, mesma âncora e mesmo colaborador; em disputa fechada exige pergunta viva. O detalhe antigo ainda exige `disputa.fechada_em IS NULL`; portanto os dois caminhos não têm exatamente o mesmo universo. (`app/chamados/services/fio.py:560`, `app/chamados/services/fio.py:589`, `app/chamados/services/fio.py:594`, `app/chamados/views.py:493`)
- **Permissão:** contextual por departamento dono do módulo; gestor geral pode agir em tudo. A função declara que visualização não depende dela. (`app/chamados/utils.py:32`, `app/chamados/utils.py:47`, `app/chamados/utils.py:52`, `app/chamados/utils.py:68`)
- **Validação:** serviço exige `respondida_em` preenchido e `validada_em` vazio; pode exigir confirmação de implausibilidade; carimba e materializa. A tranca não barra o toque humano nessa versão. Não há guard inicial exigindo texto não vazio, de modo que `respondida_em` com texto vazio passa pelo guard “sem resposta”. (`app/chamados/services/validacao.py:348`, `app/chamados/services/validacao.py:356`, `app/chamados/services/validacao.py:362`, `app/chamados/services/validacao.py:367`, `app/chamados/services/validacao.py:388`)
- **API app:** não filtra disputa aberta, status do destino ou silêncio. Perguntas com estado RESPONDIDA vão para `respondidas`, com `aguarda_admin=True`; demais ficam em `perguntas`. `chamado_id` de cada item é destino **cru**, sem fallback; no envelope há fallback ao chamado da disputa. (`app/chamados/services/pergunta_no_app.py:42`, `app/api/views.py:2620`, `app/api/views.py:2626`, `app/api/views.py:2642`)
- **Resposta vazia com carimbo:** se a pergunta continuar viva, `.estado` retorna RESPONDIDA por `respondida_em`, mesmo sem texto; logo ela pode aparecer no conjunto `respondidas` da API. Para responder, a API exige pergunta do próprio colaborador e viva, e rejeita resposta vazia. (`app/chamados/models.py:2207`, `app/api/views.py:2622`, `app/api/views.py:2680`, `app/api/views.py:2688`)

A vitalidade não equivale a `respondida_em IS NULL`: validação e tranca matam primeiro; resposta com carimbo **e texto após strip** continua viva, exceto decisão humana no dia ou concordância da resposta com luz em dia coerente; depois entram vias de identidade, fato do marco, veredito da célula, classificação da falta e prova da materialização. (`app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1039`, `app/chamados/juizes.py:1050`, `app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1082`)

`q_vivas()` é explicitamente um **prefiltro generoso**, não o valor exato; `.viva` delega ao juiz Python. Portanto SQL sozinho não decide se uma pergunta é acionável. (`app/chamados/models.py:2328`, `app/chamados/models.py:2353`, `app/chamados/models.py:2359`)

Escritores e leitores relevantes:

| Campo | Escritores | Leitores |
|---|---|---|
| `respondida_em` / `resposta_colab` | Resposta canônica do colaborador (`app/chamados/services/disputa_emissao.py:1247`); resposta automática com texto (`app/chamados/services/disputa_emissao.py:1381`); recuperação do carimbo pela trilha, somente com texto (`app/chamados/services/disputa_emissao.py:1398`); reabertura zera ambos e guarda versão anterior (`app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/disputa_emissao.py:1370`); simetrias de materialização/ausência/fecho só carimbam com texto (`app/chamados/services/materializacao.py:511`, `app/chamados/services/materializacao.py:1190`, `app/chamados/services/disputa_emissao.py:1570`, `app/chamados/services/disputa_emissao.py:1655`). | Juiz (`app/chamados/juizes.py:1020`), estado (`app/chamados/models.py:2207`), crédito administrativo por carimbo (`app/chamados/services/raiox.py:135`), template (`app/templates/chamados/partials/modal_fio.html:210`), API (`app/api/views.py:2632`), fila global para validar exige carimbo e texto (`app/chamados/juizes.py:1121`). |
| `validada_em` | Carimbo canônico (`app/chamados/services/validacao.py:127`); devolução de validação recusada zera (`app/chamados/services/validacao.py:187`); resposta nova zera (`app/chamados/services/disputa_emissao.py:1251`); reabertura zera (`app/chamados/services/disputa_emissao.py:1358`); três simetrias com texto nos pontos acima. | Mata vitalidade (`app/chamados/juizes.py:1006`); estado (`app/chamados/models.py:2204`); prefiltro (`app/chamados/models.py:2353`); template (`app/templates/chamados/partials/modal_fio.html:221`); gate de validação (`app/chamados/services/validacao.py:359`); marcador do placar (`app/chamados/services/painel.py:66`). |
| `chamado_destino` | Nascimento e reapontamento em `PerguntaDisputa.abrir` (`app/chamados/models.py:2124`, `app/chamados/models.py:2165`, `app/chamados/models.py:2182`); `reapontar_resolvedora`, com resolvedora validada e relavra no mesmo save (`app/chamados/services/fio.py:792`, `app/chamados/services/fio.py:797`). | Âncora do fio (`app/chamados/services/fio.py:116`); crédito administrativo (`app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:144`); API (`app/api/views.py:2626`); materialização de ausência (`app/chamados/services/materializacao.py:502`). |
| `status_local` | Núcleo `_gravar_estado` (`app/chamados/models.py:185`); nascimento por `abrir`/`criar` (`app/chamados/models.py:599`, `app/chamados/models.py:818`, `app/chamados/models.py:851`). Portas incluem transicionar, reconciliar, superar, reabrir, encerrar (`app/chamados/models.py:122`, `app/chamados/models.py:434`, `app/chamados/models.py:543`, `app/chamados/models.py:1115`, `app/chamados/models.py:1288`). | Fila (`app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:187`); crédito via universo vivo (`app/chamados/services/raiox.py:54`); desenlace (`app/chamados/services/desenlace.py:48`); memo do juiz quando destino já carregado (`app/chamados/juizes.py:759`, `app/chamados/juizes.py:774`). |
| `silenciado_ate` | `arquivo.silenciar` grava prazo/autor/motivo; `arquivo.levantar` limpa (`app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:176`, `app/chamados/services/arquivo.py:201`). | Filtro da fila (`app/chamados/services/painel.py:188`); partição por verbo (`app/chamados/services/verbos.py:43`); `esta_silenciado(agora)` (`app/chamados/models.py:304`). Não há leitura desse campo na seleção `perguntas_do_app`. (`app/chamados/services/pergunta_no_app.py:39`) |

Conjuntos reais:

- `VIVOS = ('aberto','em_analise','registrado')`. (`app/chamados/catalogo/motor.py:24`)
- `COBRANCA = ('aberto','em_analise')`. (`app/chamados/catalogo/motor.py:29`)
- `ENCERRADOS = ('resolvido','fechado')`. (`app/chamados/catalogo/motor.py:30`)
- `TERMINAIS = ('resolvido','fechado','cancelado','superado')`. (`app/chamados/catalogo/motor.py:31`)
- `NAO_FECHADOS = VIVOS + ('resolvido',)`. (`app/chamados/catalogo/motor.py:32`)

## 30/10 20:51 · sessao c4a97bdd103a · - · - · origem=Ronald (tmux)

**Q1 — assinaturas, retorno, custo e chamadores**

- `fora_da_folha(empresa, mes, ano, com_itens=False)` retorna dict com `competencia`, `empresa`, `total`, `entram`, `fora_por_motivo`, `furo_espelho`, `grupos`, `dias_por_fio`, `colabs`; `com_itens=True` acrescenta a classificação crua inteira em `itens`. Cada `colabs` contém `colab,nome,grupo,motivos,dias,datas,por_fio`. Somente os classificados com motivo `furo_espelho` recebem linhas em `colabs`; outros retidos entram nas contagens, mas não nessa lista. Referências: `app/folha/fora_da_folha.py:97`, `:119`, `:129`, `:160`, `:170`, `:174`.
- Chamador direto de produção encontrado: `folha.retrato_folha.montar_linhas(empresa,mes,ano)` chama com `com_itens=True` em `app/folha/retrato_folha.py:1120`, `:1157`; seu chamador é o command `lavrar_retrato_folha`, em `app/folha/management/commands/lavrar_retrato_folha.py:65`. Ausência de outros chamadores, após busca textual no repositório: **[não verificado para chamadas dinâmicas]**.
- Custo estático de `fora_da_folha`: defina `C=queries de classificar_export(empresa,mes,ano)`, `R(c)=queries de motivos_retencao_celula(c,...)`, `J=queries dos juízes de vitalidade das perguntas`. A estrutura é **`C + carga dos colaboradores furo (normalmente 1) + Σ R(c) + duas cargas de fio + J`**; consultas com `__in=[]` podem não executar SQL. Cada colaborador retido por furo tem seus dias lidos novamente, depois da classificação. Referências: `app/folha/fora_da_folha.py:120`, `:132`, `:135`, `:139`, `:142`, `:46`, `:51`, `:53`, `:66`.
- `R(c)` normalmente custa **2 queries**: células da janela e vínculos da janela; custa zero nas saídas de isento/janela colapsada. A consulta de vínculo singular delega à plural; sem `mais`, só executa sua consulta principal. Referências: `app/folha/export.py:1174`, `:1184`, `:1188`, `:1194`; `app/escala/alimentacao.py:131`, `:153`, `:116`, `:123`.
- As “duas consultas” de alimentação **não são o custo total do julgamento do fio**: `_alimentar` carrega apenas `disputa` por join, enquanto `_fio_do_dia` chama `p.viva` e `p.estado`; `viva` delega a `pergunta_viva`, cuja chave já acessa `p.celula`, e o julgamento pode carregar colaborador/empresa, consultar tranca e descer a outros juízes. Há memo por instância de pergunta, portanto `estado` não repete necessariamente o julgamento já feito por `viva`. Referências: `app/folha/fora_da_folha.py:51`, `:66`, `:67`; `app/chamados/models.py:2207`, `:2359`, `:2369`; `app/chamados/juizes.py:757`, `:791`, `:793`, `:1005`, `:1010`, `:1068`, `:1083`, `:888`, `:896`, `:900`.
- `C` não é constante: `classificar_export` consulta todos os fechamentos da empresa/competência, catálogo, rubricas, férias e âncoras, depois percorre cada fechamento; dentro do laço pode contar batidas, consultar escala, perguntar previsto por dia, ler retenção e homologação. Referências: `app/folha/export.py:746`, `:766`, `:770`, `:771`, `:772`, `:787`, `:795`, `:888`, `:912`, `:915`, `:942`, `:948`.
- Há custos adicionais no cruzamento férias/batidas: o queryset de férias não usa `select_related`; cada agendamento acessa `periodo_aquisitivo`, e cada colaborador com férias recebe consulta de batidas. Referências: `app/ferias/services.py:47`, `:51`, `:54`; `app/folha/export.py:480`, `:481`, `:484`, `:488`.
- `o_que_segura(colab, mes, ano)` retorna `competencia,colab,nome,entra,motivo,grupo,motivos_retencao,dias`; cada dia contém `data,veredito,motivo,fio,destrava,chamado`. Referências: `app/folha/fora_da_folha.py:179`, `:209`, `:211`.
- Seu custo é **`C de toda a empresa + R(colab) + duas cargas de fio + uma carga extra dos IDs dos chamados + J`**, com eventual carregamento lazy de `colab.empresa`; portanto chamá-la para N colaboradores repete N vezes a classificação inteira da empresa. Referências: `app/folha/fora_da_folha.py:188`, `:191`, `:193`, `:194`, `:198`. Nenhum chamador direto de produção encontrado na busca textual; somente testes **[não verificado para chamadas dinâmicas]**.

**Q2 — identidade dos dias, ausência de célula, unicidade**

- São a **mesma fonte e regra**, não a mesma lista compartilhada entre chamadas: ambas chamam `_dias_que_retem`, que delega a `motivos_retencao_celula(...,com_dias=True)`; `datas` projeta essa lista em strings e `o_que_segura['dias']` a transforma em descrições. Referências: `app/folha/fora_da_folha.py:90`, `:93`, `:139`, `:168`, `:193`, `:203`, `:209`.
- A equivalência exige mesmo colaborador, mesma janela e mesmo estado/“hoje”; cada chamada refaz consultas e o retentor usa `timezone.localdate()` por padrão. Além disso, `fora_da_folha['colabs']` só inclui `furo_espelho`, mas `o_que_segura` calcula dias mesmo se o colaborador estiver fora por motivo anterior da classificação. Referências: `app/folha/export.py:1179`; `app/folha/fora_da_folha.py:129`, `:191`, `:193`.
- **Sim, há dia retido sem `celula_id`**: dia passado sem célula, coberto por vínculo e diferente do dia de demissão, vira `sem_celula` com ID e veredito `None`; dia sem célula e sem vínculo é ignorado. Referências: `app/folha/export.py:1200`, `:1203`, `:1209`, `:1211`, `:1214`. Esse dia recebe fio `nada` em ambas as ferramentas. Referências: `app/folha/fora_da_folha.py:154`, `:204`.
- `CelulaDia` declara unicidade `(colaborador,data)` no modelo e na migração de criação. Referências: `app/escala/models.py:1785`, `:1788`; `app/escala/migrations/0030_celuladia.py:35`. Aplicação real da migração/constraint na base de produção: **[não verificado]**.

**Q3 — competências abertas/não entregues**

- Para o significado solicitado, o predicado existente é `not competencia_entregue(empresa_id,mes,ano)`: K6 busca o TXT vigente pela tripla e exige `invalidada_em is None`; `registro=True` retorna o registro ou `None`, default retorna bool. Referências: `app/folha/export.py:545`, `:569`, `:570`, `:572`, `:573`, `:575`.
- K6 custa **1 query sem registros ED; 2 quando existem registros**, porque `anotar_vigencia` faz uma segunda consulta se a lista não está vazia; seleciona mais recente por `(gerado_em,pk)`. Referências: `app/folha/export.py:570`; `app/folha/models.py:203`, `:207`, `:211`, `:220`.
- Funções reais do calendário: `periodo_apuracao(mes,ano,dia_corte=21)`, `periodo_apuracao_atual(data,dia_corte=21)`, `corte_da_empresa(empresa)`, `janela_fechamento(mes,ano,empresa=None)`, `janela_atual(data,empresa=None)`, `janela_anterior(data,empresa=None)`; não consultam entrega e usam corte clampado de 2 a 28. Referências: `app/ponto/janelas.py:10`, `:15`, `:24`, `:36`, `:51`, `:54`, `:60`, `:66`.
- `folha.export.janela_competencia` delega a `janela_fechamento`. `ponto.services.passivo_s84.competencia_de(dia,empresa)` devolve mês/ano do fim da `janela_atual`. Referências: `app/folha/export.py:47`, `:50`; `app/ponto/services/passivo_s84.py:38`, `:45`, `:47`.
- `chamados.services.vigia_fio.competencia_do_dia_entregue(dia,empresa,memo=None)` compõe `janela_atual` com K6, retorna `None` sem dia e memoiza por `(empresa,mes,ano)` quando recebe dict. Referências: `app/chamados/services/vigia_fio.py:455`, `:467`, `:471`, `:472`, `:475`.
- **Enumeração parcial existente**: `he_pendente_lavrado.competencias_a_lavrar(empresa,hoje)` retorna atual **incondicionalmente** e anterior só quando não entregue; não retrocede além da anterior e não verifica se a atual já foi entregue. Referências: `app/ponto/services/he_pendente_lavrado.py:209`, `:220`, `:221`, `:222`, `:223`, `:225`.
- **Janela contínua existente**: `esmeril_espelho.janela(colab,hoje=None)` começa na atual e retrocede enquanto a anterior não foi entregue e existe alguma célula do colaborador até o fim dela; retorna intervalo até ontem. É por colaborador e pode parar na primeira entregue, não enumera todas as competências não entregues da empresa. Referências: `app/ponto/services/esmeril_espelho.py:38`, `:55`, `:57`, `:58`, `:60`, `:62`, `:63`.
- **Enumerações de recorte sem filtrar entrega**: `vigia_pausa._competencias(hoje,empresa)` retorna anterior e atual; `dia_pago.competencias_do_intervalo(ini,fim,empresa=None)` retorna as competências tocadas pelo intervalo, janela a janela; `fechamento.competencias_do_fato(colaborador,de,ate=None)` apenas adapta datas e delega a essa última. Referências: `app/folha/services/vigia_pausa.py:77`, `:94`; `app/ponto/services/dia_pago.py:700`, `:712`, `:714`, `:716`; `app/ponto/services/fechamento.py:1541`, `:1562`, `:1567`.
- `_competencias_exportadas(empresa_id,colaborador_id=None)` enumera o lado fechado para regeneração: candidatos com ED ou holerite publicado e valida por `competencia_fechada_para_celula`; K9 é **entregue OU holerite publicado da pessoa**. Referências: `app/ponto/management/commands/gerar_celulas.py:126`, `:161`, `:169`, `:173`, `:179`; `app/folha/export.py:578`, `:599`, `:603`, `:609`.
- Outros “fechados” são perguntas distintas: `ponto.janelas.competencia_fechada(colaborador,timestamp)` lê status individual aprovado/exportado de `FechamentoMensal`; `fechamento.periodo_trancado`/`competencia_trancada` leem tranca viva de `PeriodoFechado`; `marcos_da_competencia` reúne aprovada/exportada/trancada/paga e seu `marco='aberta'` exige ausência de todos, não equivale a “não entregue”. Referências: `app/ponto/janelas.py:75`, `:85`, `:86`, `:87`; `app/ponto/services/fechamento.py:1432`, `:1438`, `:1441`, `:1448`, `:1486`, `:1491`, `:1500`.
- Não identifiquei função que enumere genericamente **todas e somente** as competências ainda não entregues de uma empresa; a ausência absoluta é **[não verificado]**.

## 30/10 20:50 · sessao 868d11dd3d12 · - · - · origem=Ronald (tmux)

**5. `cadastro_realidade`**

- Assinatura: `lista(hoje=None, empresas=None) -> dict | None`; `hoje` assume `timezone.localdate()`, `empresas=None` abrange todas, e retorna `None` quando não há lavra por colaborador nem assinaturas de cadastro (`app/escala/services/cadastro_realidade.py:502`, `:508`, `:575`, `:577`).
- O universo combina assinaturas A da lavra com C1–C9, depois carrega colaboradores/vínculos atuais; exclui colaborador inexistente ou de empresa fora do filtro e agrupa por posto, incluindo “sem vinculo vigente” (`app/escala/services/cadastro_realidade.py:513`, `:527`, `:535`, `:542`, `:549`, `:556`, `:563`, `:570`, `:579`, `:588`, `:621`).
- A tela chama `lista(empresas={e.pk for e in empresas_visiveis()})`; `empresas_visiveis()` exige `ativa=True` e exclui CNPJ iniciado em `11222333` (`app/escala/views.py:490`; `app/colaboradores/queries.py:78`, `:85`).
- C3/C7/C8/C9 leem snapshots da frota antes do filtro final por empresa; C8 ainda consulta se o colaborador continua no posto acusado pela lavra (`app/escala/services/cadastro_realidade.py:527`, `:276`, `:294`, `:306`, `:588`; `app/ponto/services/vinculo_na_lista.py:146`, `:151`).
- `universo` e a parte A de `por_assinatura` vêm da lavra, sem recálculo por empresas visíveis; os contadores de cadastro são montados pelas linhas efetivamente aceitas (`app/escala/services/cadastro_realidade.py:626`, `:636`, `:646`).

Custo SQL estático, sem exceções e desconsiderando cache frio:

| Parte | Consultas | Evidência |
|---|---:|---|
| C1 | 1 | `app/colaboradores/services/vinculo.py:472`, `:477` |
| C2 | 1 vínculos + 1 células se há tipos elegíveis | `app/escala/services/cadastro_realidade.py:190`, `:193`, `:195` |
| C3 | 1 snapshot | `app/escala/services/previsto_cego.py:217`, `:220` |
| C4 | 2 conjuntos de IDs + 1 colaboradores se há IDs + N células + M coberturas | `app/escala/services/cadastro_realidade.py:347`, `:350`, `:351`, `:359`, `:361`, `:366`; `app/colaboradores/services/situacao_exibida.py:46`; `app/ponto/turnos.py:2734`, `:2740` |
| C6 | 1 | `app/escala/services/cadastro_realidade.py:258`, `:261` |
| C7 | 1 snapshot | `app/escala/services/regua_na_lista.py:57`, `:60` |
| C8 | 2 leituras do mesmo snapshot + 1 colaboradores se há IDs | `app/escala/services/cadastro_realidade.py:297`, `:299`; `app/ponto/services/vinculo_na_lista.py:128`, `:146`, `:151` |
| C9 | 1 snapshot | `app/ponto/services/teto_na_lista.py:120`, `:123` |
| C5 | 1 empresas + E células | `app/escala/services/cadastro_realidade.py:126`, `:128`, `:130` |
| Lavra A | 1 snapshot | `app/ponto/services/esmeril_espelho.py:345`, `:346` |
| Enriquecimento | 2 consultas de lote se há IDs | `app/escala/services/cadastro_realidade.py:483`, `:490` |

No caminho normal não vazio: **17 + E + N + M**, com E empresas, N afastados examinados e M afastados com dias candidatos; consultas com `__in=[]` podem ser eliminadas pelo ORM [não verificado]. O catálogo vivo de ausências pode acrescentar consulta quando o cache de 60 segundos está frio/expirado (`app/ponto/models.py:689`; `app/ponto/catalogo/ausencias.py:249`, `:257`, `:267`, `:276`). Custo observado em execução: [não verificado].

- C2 percorre as células elegíveis, mas `periodos_do_dia` trabalha com `ata`, `dna` e o tipo de escala já carregado, sem consulta explícita em seu corpo (`app/escala/services/cadastro_realidade.py:195`, `:200`; `app/ponto/juiz_batida.py:149`, `:157`, `:174`, `:250`, `:314`).
- `dias_na_lista(dados)` retorna `{(colab_id, 'AAAA-MM-DD'): dono_do_dia}`; apenas percorre os dados fornecidos, portanto custa zero queries (`app/escala/services/cadastro_realidade.py:655`, `:665`, `:669`, `:670`).
- Colaborador aparecer na lista **não prova que um dia esteja listado**: só entra no mapa a assinatura com campo estruturado `data`; assinaturas do vínculo inteiro não nomeiam o dia (`app/escala/services/cadastro_realidade.py:660`, `:669`).
- No código atual, C9 fornece `data` e `dono_do_dia`; C4/C5 mencionam datas apenas em `detalhe`, portanto não entram por isso em `dias_na_lista` (`app/escala/services/cadastro_realidade.py:312`, `:313`, `:372`, `:374`, `:141`, `:142`).
- Mesmo C9 presente pode não fornecer porta: `acao` retorna URL para BATIDA/CADASTRO, mas retorna `None` para ESTRUTURA ou dono ainda não lavrado (`app/escala/services/cadastro_realidade.py:445`, `:451`, `:453`, `:457`, `:459`).
- O vigia carrega essa lista uma vez, sob demanda, com empresas visíveis intersectadas com seu filtro; considera fio do admin somente quando o mapa contém exatamente o dia com dono BATIDA/CADASTRO; falha da lista mantém o alarme (`app/chamados/services/vigia_fio.py:244`, `:250`, `:256`, `:259`, `:263`, `:266`).

**7. Composição existente**

Não encontrei função de produção que componha `fora_da_folha`/`o_que_segura` com `qs_na_fila`/`perguntas_do_app`; conclusão negativa por busca estática [não verificado].

O caminho existente de folha usa outro conjunto: `_dias_que_retem` chama `motivos_retencao_celula`; `_alimentar` consulta perguntas por célula e chamados em `VIVOS`; `_fio_do_dia` consulta `p.viva`, sem aplicar a seleção da fila ou a lista do app (`app/folha/fora_da_folha.py:46`, `:51`, `:53`, `:59`, `:66`, `:90`, `:93`, `:139`, `:142`, `:193`, `:194`).

Há instrumentos próximos, mas diferentes: `dias_sem_fio` compõe `_alimentar`/`_fio_do_dia` com células `discordante`, enquanto `vigia_retido_calado.censo` lê retratos gravados e considera roteamento de C5 por colaborador (`app/ponto/services/pautas_dos_discordantes.py:45`, `:59`, `:67`, `:73`, `:75`; `app/folha/vigia_retido_calado.py:27`, `:31`, `:40`, `:41`, `:44`, `:46`).

## 30/10 20:17 · sessao a717d30b276f · O261-E-D3 · censo · origem=Code

O outbox encontrado é **depois da lavra da célula**: registra alterações de `veredito`/`ata` e alimenta a reconciliação de chamados e perguntas. O rejulgamento reativo do cartório ocorre por chamadas síncronas, principalmente nos signals de insumos. (`app/ponto/portas/celula.py:622`, `app/chamados/reconciliador.py:1300`, `app/chamados/reconciliador.py:1303`, `app/ponto/signals.py:85`)

**1. Definição, chamadas e composição de `impressao_insumos`**

A definição está em `app/ponto/services/cartorio.py:314`. Há duas chamadas de produção, ambas em `_julgar_colab_corpo`: ramo sem adesão, em `app/ponto/services/cartorio.py:941`, e ramo normal, em `app/ponto/services/cartorio.py:1083`. `julgar_colab` chama esse corpo e pode repeti-lo para reconciliação/releitura. (`app/ponto/services/cartorio.py:744`, `app/ponto/services/cartorio.py:746`, `app/ponto/services/cartorio.py:811`)

O comando `processar_cartorio` **reexporta** a função, mas chama `julgar_colab`: busca colaboradores com células passadas, carrega suas células ordenadas por data e entrega a lista ao serviço. A porta unitária `julgar_celula` busca uma célula e chama o mesmo serviço com `[cel]`. (`app/ponto/management/commands/processar_cartorio.py:20`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1547`, `app/ponto/services/cartorio.py:1560`)

O resultado é `sha256('|'.join(partes).encode()).hexdigest()[:64]`; as partes são construídas na ordem abaixo. (`app/ponto/services/cartorio.py:323`, `app/ponto/services/cartorio.py:380`)

| Argumento | O que efetivamente entra no hash | Origem no chamador normal |
|---|---|---|
| `bs_janela` | Para cada batida: **`pk`, `tipo`, `timestamp.isoformat()`**, separados por `:`; batidas separadas por vírgula. (`app/ponto/services/cartorio.py:324`) | `bj`, recorte por data local da lista `bs`, carregada por `batidas_apuraveis`. (`app/ponto/services/cartorio.py:871`, `app/ponto/services/cartorio.py:1042`) |
| `cob_status` | Apenas `str(cob_status)`, inclusive `"None"` quando não há cobertura. (`app/ponto/services/cartorio.py:325`) | `getattr(cob, 'status', None)`, com `cob = ausencia_cobre(c, data, para='cobranca')`. (`app/ponto/services/cartorio.py:1041`, `app/ponto/services/cartorio.py:1083`) |
| `chamados` | Para cada chamado: **`pk`, `status_local`**; nenhum outro campo diretamente. (`app/ponto/services/cartorio.py:326`) | `lista = chs.get(data.isoformat(), [])`; `chs` vem de `chamados_que_falam_de_batida(c)`. (`app/ponto/services/cartorio.py:1004`, `app/ponto/services/cartorio.py:1040`) |
| `dna` | Somente **`str(dna_versao) + str(marcos)`**. Não é o JSON inteiro. (`app/ponto/services/cartorio.py:327`) | `cel.dna`, da célula recebida pelo serviço. (`app/ponto/services/cartorio.py:1083`) |
| `veto` | `veto:<valor>`, somente quando o valor é verdadeiro. (`app/ponto/services/cartorio.py:329`) | `_veto_do_dia(data)` → `motivo_do_veto(c, vinculo_do_emissor(c,data),data)`. (`app/ponto/services/cartorio.py:1011`, `app/ponto/services/cartorio.py:1013`) |
| `teto` | `teto:<valor>`, quando diferente de `None`. (`app/ponto/services/cartorio.py:334`) | Quantidade de marcos vencidos retornados por `marcos_vencidos`, nos caminhos de `teto_de_hoje`; normalmente `None`. (`app/ponto/services/cartorio.py:1048`, `app/ponto/services/cartorio.py:1053`, `app/ponto/services/cartorio.py:1067`, `app/ponto/services/cartorio.py:1085`) |
| `regra` | `regra:<valor>`, quando diferente de `None`. (`app/ponto/services/cartorio.py:342`) | `_regra_do_dia(data)` → `versao_para(data,c.empresa)`. A versão vem de `REGRA_VERSOES`, escolhida pela competência de `janela_atual`. (`app/ponto/services/cartorio.py:100`, `app/ponto/services/cartorio.py:183`, `app/ponto/services/cartorio.py:888`) |
| `fio` | `fio:<valor>`, quando diferente de `None`; **zero entra**. (`app/ponto/services/cartorio.py:367`) | Número de chamados vivos da lista que possuem pergunta viva: `_com_fio`. Sem chamados vivos, recebe `None`. (`app/ponto/services/cartorio.py:1069`, `app/ponto/services/cartorio.py:1074`, `app/ponto/services/cartorio.py:1087`) |
| `cob_parcial` | Quando verdadeiro: `cobparcial:1`; sendo string, acrescenta `:<estado>`. (`app/ponto/services/cartorio.py:375`) | `dia['periodo_parcial']` ou o booleano `dia['cobertura_parcial']`, lidos da grade. (`app/ponto/services/cartorio.py:1079`, `app/ponto/services/cartorio.py:1088`) |
| `enc` | `enc:<valor>`, quando diferente de `None`. (`app/ponto/services/cartorio.py:352`) | `0` para dia passado cujo turno permanece em curso e não está sendo julgado até um teto; caso contrário, `None`. (`app/ponto/services/cartorio.py:1082`, `app/ponto/services/cartorio.py:1084`) |

Detalhes da alimentação desses argumentos:

- `chamados_que_falam_de_batida` consulta `ChamadoColaborador` do colaborador, filtra `modulo_origem__in=FURO_MODULES`, faz prefetch de `perguntas_que_resolvem` e agrupa pela data obtida por `data_do_chamado`. Os módulos são `batida_ausente`, `turno_aberto`, `intervalo_pendente` e `entrada_bloqueada_turno_aberto`. (`app/ponto/services/cartorio.py:85`, `app/ponto/services/cartorio.py:93`, `app/ponto/services/cartorio.py:96`, `app/chamados/catalogo/furo_modulos.py:15`)
- `ausencia_cobre` consulta `Ausencia`, exclui rejeitadas para cobrança, considera dia único/range fechado/range aberto permitido e escolhe a vencedora por `-data,-pk`. (`app/ponto/turnos.py:2489`, `app/ponto/turnos.py:2525`, `app/ponto/turnos.py:2530`)
- A grade escreve `cobertura_parcial` usando `cobre_parte_do_dia` e escreve `periodo_parcial` usando `periodo_da_parcial_no_dia`. A parcialidade depende de tipo habilitado para horas, dia único e `0 < minutos < 480`; o estado do período pode ser `sem_periodo`, `dentro`, `fora` ou `nao_sei`. (`app/escala/utils.py:1147`, `app/escala/utils.py:1190`, `app/escala/utils.py:1194`, `app/ponto/turnos.py:2571`, `app/ponto/turnos.py:2587`)
- `fio_vivo` reduz as perguntas recebidas a `any(pergunta_viva(p) ...)`. (`app/chamados/juizes.py:1102`, `app/chamados/juizes.py:1111`)
- `marcos_vencidos` obtém o vínculo por `escala_vigente`, lê `marcos_turno` e compara cada instante com `agora`, acrescentando a margem de 30 minutos. (`app/ponto/services/cartorio.py:662`, `app/ponto/services/cartorio.py:675`, `app/ponto/services/cartorio.py:679`)
- `_turno_encerrado` vem de `dias_encerrados(c,d0,d1,agora=...,celulas=...)`. (`app/ponto/services/cartorio.py:850`)

No DNA produzido hoje, `marcos` contém `hi`, `hf`, `hii`, `hfi` e, quando houver segunda pausa, `hii2`, `hfi2`; as horas são convertidas para `HH:MM`. Como entra `str(marcos)`, entra a representação do dicionário inteiro nessa parcela. **Ficam fora dessa parcela** `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `minutos_jornada`, `intervalo_livre`, `lampadas` e `extra_declarada`. (`app/ponto/management/commands/gerar_celulas.py:32`, `app/ponto/management/commands/gerar_celulas.py:83`, `app/ponto/management/commands/gerar_celulas.py:92`, `app/ponto/management/commands/gerar_celulas.py:95`, `app/ponto/management/commands/gerar_celulas.py:121`, `app/ponto/services/cartorio.py:327`)

No ramo **`adesao is None`**, a chamada passa `bs_janela=[]`, `chamados=[]`, a cobertura obtida por `ausencia_cobre`, `cel.dna`, `regra` e a parcialidade da grade. `veto`, `teto`, `fio` e `enc` permanecem nos defaults `None`. A adesão é obtida pela primeira batida não retratada da vida do colaborador. (`app/ponto/services/cartorio.py:899`, `app/ponto/services/cartorio.py:907`, `app/ponto/services/cartorio.py:924`, `app/ponto/services/cartorio.py:933`, `app/ponto/services/cartorio.py:941`)

**Janela de batidas:** para a célula de **D**, o hash normal recebe batidas de **D−1, D e D+1**, pela data local — intervalo equivalente a `[D−1 00:00, D+2 00:00)`. A carga geral do lote é maior: `[d0−1 00:00, d1+3 00:00)`, depois recortada por célula. São excluídas batidas retratadas, e a ordenação é por `timestamp`. (`app/ponto/services/cartorio.py:855`, `app/ponto/services/cartorio.py:856`, `app/ponto/services/cartorio.py:871`, `app/ponto/services/cartorio.py:1042`, `app/ponto/turnos.py:2845`, `app/ponto/turnos.py:2852`)

Também há chamadas diretas em testes: [nao verificado]

| Arquivo | Linhas das chamadas |
|---|---|
| `app/ponto/tests/test_cartorio.py` | 16, 17, 22, 23, 29, 30, 34, 35 |
| `app/ponto/tests/test_veto_que_cai_rejulga.py` | 136, 137, 138, 141 |
| `app/ponto/tests/test_vigia_de_hora.py` | 144, 145 |
| `app/ponto/tests/test_o219_versao_da_regra.py` | 97, 102, 110, 111, 112, 118, 119, 146 |
| `app/ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py` | 40, 41, 47, 48, 129 |
| `app/ponto/tests/test_o146_extra_declarada_da_escala.py` | 423, 424 |
| `app/ponto/tests/test_o261_cobrado_le_o_fio.py` | 209, 210, 211, 212 |
| `app/ponto/tests/test_celula_concorde_meio_turno.py` | 56, 57, 86, 87, 88 |
| `app/escala/tests/test_cobertura_parcial_periodo.py` | 203, 204, 205, 206, 207 |

**2. Os nove insumos: hash, portas e disparos**

Primeiro, o mecanismo comum:

- `EventoCelula` é o outbox encontrado; contém identificadores, data, criação, processamento, tentativas e erro. `celula_id` é inteiro, **sem FK**. (`app/escala/models.py:1800`, `app/escala/models.py:1808`)
- `salvar_e_emitir` salva a célula e insere evento no mesmo `atomic` quando os campos lavrados incluem `veredito` ou `ata`. Alterar apenas DNA ou campos de insumos não satisfaz esse critério. (`app/ponto/portas/celula.py:623`, `app/ponto/portas/celula.py:634`, `app/ponto/portas/celula.py:638`)
- O consumidor lê o evento e chama `retratar_chamados_absolvidos_por_celula` e `encerrar_orfas_pela_lampada`. Uma alteração de status de chamado feita nessa reconciliação pode, por sua vez, chamar o cartório pelo signal do chamado. (`app/chamados/reconciliador.py:1300`, `app/chamados/reconciliador.py:1303`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`)
- O consumidor está configurado a cada minuto; o cartório periódico roda com `--apply`, sem `--forcar`. A “fila” deste último é o queryset de células passadas, e o hash igual permite pular o julgamento. (`app/config/crons.py:375`, `app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/services/cartorio.py:1092`)
- `out['rejulgar']` é uma lista em memória para segunda passada, com descarte contado das sobras; não é outbox persistente. (`app/ponto/services/cartorio.py:745`, `app/ponto/services/cartorio.py:751`)
- Não localizei outra tabela/função de fila persistente para rejulgar `CelulaDia`, pesquisando `Outbox`, `Pendencia`, `Fila`, `enfileirar`, `agendar`, `rejulgar`, `julgar_celula` e `julgar_colab` nos modelos, signals, services e comandos de `app/`. Infraestrutura externa ao código disponível: [nao verificado].

**(i) Batidas do dia**

**Hash:** entram diretamente por `bs_janela`, apenas `pk/tipo/timestamp`. `retratada_em` atua indiretamente, retirando ou recolocando a batida no universo apurável; `aprovada` isoladamente não é filtro desse universo. (`app/ponto/services/cartorio.py:324`, `app/ponto/turnos.py:2845`)

**Portas:** criação por `registrar_batida`; alteração por `anotar`; retratação por `retratar_batida`; exclusão por `apagar`. `flip_tipo` usa `anotar`; retratação/restauração humana usa `retratar_humano.retratar/desfazer`. (`app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:235`, `app/ponto/registro_batida.py:267`, `app/ponto/registro_batida.py:285`, `app/ponto/services/flip_batida.py:30`, `app/ponto/services/retratar_humano.py:179`, `app/ponto/services/retratar_humano.py:207`)

Entradas que chegam à criação incluem API/PWA, materialização de justificativa, resposta de pergunta, regularização externa e decisão do dia. (`app/api/views.py:683`, `app/api/views_core.py:680`, `app/ponto/services/triagem_batida.py:708`, `app/ponto/services/justificativa.py:173`, `app/ponto/services/justificativa.py:346`, `app/chamados/services/materializacao.py:872`, `app/chamados/services/materializacao.py:952`, `app/chamados/services/regularizacao_ext.py:77`, `app/ponto/services/veredito_celula.py:301`)

**Disparo:** save/delete chama sincronicamente `julgar_celula` para a data local da batida e sua véspera. Não enfileira julgamento em outbox; a lavra resultante pode produzir o evento posterior. (`app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/signals.py:93`, `app/ponto/portas/celula.py:638`)

A interface de importação AEJ retorna HTTP 410. O bulk de `semear` exige banco de ensaio e recusa o banco de produção indicado no código. (`app/ponto/views.py:57`, `app/ponto/views.py:70`, `app/ponto/registro_batida.py:326`, `app/ponto/registro_batida.py:331`)

**(ii) Batidas do dia seguinte**

**Hash e portas:** são as mesmas batidas/portas do item anterior; D+1 faz parte expressamente do recorte do hash de D. (`app/ponto/services/cartorio.py:1042`, `app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:235`)

**Disparo:** uma batida de D+1 aciona D pelo signal que julga a véspera. Há uma assimetria: uma batida de D−1 também pertence ao hash de D, mas seu signal julga D−2/D−1, **não D**. (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/services/cartorio.py:1042`)

**(iii) `CelulaDia.dna`**

**Hash:** diretamente, mas **parcialmente**: somente `dna_versao` e `marcos`; modificar outra chave isoladamente não muda essa parcela. (`app/ponto/services/cartorio.py:327`)

**Portas e disparos:**

| Porta | Escrita e julgamento |
|---|---|
| `gerar_celulas_periodo` | Cria, completa DNA nulo passado e atualiza células presentes/futuras; termina sem chamada ao cartório/outbox. (`app/ponto/portas/celula.py:593`, `app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:610`, `app/ponto/portas/celula.py:617`) |
| `regenerar_celulas_vinculo` | Reescreve DNA; com `recalculo=True`, força julgamento dos dias tocados até hoje, excetuando vereditos humanos. (`app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:365`, `app/ponto/portas/celula.py:372`) |
| `regenerar_celulas_dia` | Delega ao regenerador de vínculo com `[dia,dia]` e sem extensão do horizonte. (`app/ponto/portas/celula.py:474`) |
| `reverter_regeneracao_dia` | Repõe `dna_anterior`, salva e registra trilha; termina sem julgamento/outbox. (`app/ponto/portas/celula.py:530`, `app/ponto/portas/celula.py:538`, `app/ponto/portas/celula.py:552`) |

Não há emissão direta do outbox apenas pela escrita de DNA: o critério é `veredito/ata`. Não localizei receiver de `CelulaDia` nos signals de `app/`; a ausência de signal nessa cadeia também está declarada no código. (`app/ponto/portas/celula.py:623`, `app/ponto/portas/celula.py:332`; busca negativa: [nao verificado])

**(iv) Vínculo vigente / escala**

**Hash:** o vínculo não é serializado. Pode entrar indiretamente pelos marcos do DNA regenerado, pelo `veto` e pelos valores derivados `teto/enc`. O julgamento ainda recebe cadastro vivo do vínculo, carregado separadamente, sem serializá-lo integralmente no hash. (`app/ponto/services/cartorio.py:327`, `app/ponto/services/cartorio.py:839`, `app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`, `app/ponto/services/cartorio.py:1118`)

O escritor declarado de `EscalaColaborador` é `colaboradores/services/vinculo.py`. (`app/core/portas.py:465`)

| Portas/caminho | Disparo efetivo |
|---|---|
| `executar_vinculo` | Cria/atualiza vínculo, regenera células e chama `reconciliar_apos_vinculo`, que força julgamento, com exclusões de dias humanos/já julgados. (`app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:278`, `app/colaboradores/services/vinculo.py:320`, `app/colaboradores/services/vinculo.py:328`, `app/colaboradores/services/pos_vinculo.py:50`, `app/colaboradores/services/pos_vinculo.py:72`) |
| `encerrar_vinculo`, `corrigir_vinculo_retroativo` | Usam save; mudanças de vigência/âncora acionam regeneração pelos signals correspondentes. (`app/colaboradores/services/vinculo.py:799`, `app/colaboradores/services/vinculo.py:845`, `app/escala/signals.py:71`, `app/escala/signals.py:117`) |
| `fechar_vigencia`, `reabrir_vigencia_impossivel`, `absorver_vigencias_posteriores`, `invalidar_ancoras_incoerentes`, `reverter_para_snapshot` | Fazem `update()` sem o signal de save; esses corpos não chamam cartório/outbox. O chamador pode complementar o ato. (`app/colaboradores/services/vinculo.py:615`, `app/colaboradores/services/vinculo.py:691`, `app/colaboradores/services/vinculo.py:753`, `app/colaboradores/services/vinculo.py:857`, `app/colaboradores/services/vinculo.py:874`) |
| `trocar_calendario_de_folga`, `marcar_trabalha_em_feriado` | Salvam essas chaves, que não são vigência/âncora; não há rejulgamento direto nesses corpos. (`app/colaboradores/services/vinculo.py:445`, `app/colaboradores/services/vinculo.py:825`, `app/escala/signals.py:57`, `app/escala/signals.py:107`) |
| `criar_trecho_retroativo`, `ajustar_vinculo_pelo_sistema`, `desfazer_ajuste_do_sistema` | Criam/fecham/restauram vínculos; os receivers de regeneração pulam `created`, e há atualizações sem signal nesses caminhos. (`app/colaboradores/services/vinculo.py:832`, `app/colaboradores/services/vinculo.py:928`, `app/colaboradores/services/vinculo.py:940`, `app/escala/signals.py:54`, `app/escala/signals.py:104`) |
| `salvar_tipo` → `_propagar_regeneracao_template` | Regenera com **`recalculo=False`**; portanto não executa o julgamento da regeneração. Também invalida o fechamento. (`app/escala/services/cadastro_tipo.py:320`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/cadastro_tipo.py:350`, `app/escala/services/cadastro_tipo.py:357`, `app/ponto/portas/celula.py:360`) |

Folgas também alteram esse desenho: `lancar_folga`, `alterar_folga`, `aplicar_folgas_em_lote` e `gerar_foto_folgas` chamam `relavrar_celulas_da_foto`, que regenera com o default de julgamento ligado. Salvar o calendário isoladamente apenas grava o cadastro. (`app/escala/folgas.py:192`, `app/escala/folgas.py:220`, `app/escala/folgas.py:261`, `app/escala/folgas.py:303`, `app/escala/folgas.py:340`, `app/escala/folgas.py:468`)

A régua **de defesa do vínculo**, distinta da CCT, é gravada por `gravar_regua` com update sem signal. `desfazer_corroboracao` possui chamada explícita a `rejulgar_furos_do_vinculo`, que usa `julgar_colab`. (`app/colaboradores/services/vinculo.py:889`, `app/colaboradores/services/vinculo.py:911`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:725`, `app/escala/regua_defesa.py:692`)

Nenhum desses caminhos é enqueue de julgamento em outbox; os que julgam chamam o cartório diretamente, e a eventual lavra pode emitir `EventoCelula`. (`app/ponto/portas/celula.py:372`, `app/ponto/portas/celula.py:638`)

**(v) Ausências/coberturas: férias, atestado, afastamento, abono**

**Hash:** entra o **status da ausência vencedora** por `cob_status`, mais a classificação de parcialidade/período por `cob_parcial`. Não entram integralmente `pk`, tipo, datas, minutos ou horários da ausência. Datas e tipo influenciam a seleção da cobertura; tipo/minutos/horários podem influenciar a classificação parcial. Duas ausências diferentes podem produzir os mesmos componentes de hash. (`app/ponto/services/cartorio.py:325`, `app/ponto/services/cartorio.py:375`, `app/ponto/services/cartorio.py:1088`, `app/ponto/turnos.py:2525`, `app/ponto/turnos.py:2571`, `app/ponto/turnos.py:2605`)

**Portas:** `criar_ausencia`/`garantir_ausencia`; `editar_ausencia_registro`; `mudar_estado`/`decidir`; `anexar_documento`; `reverter_rejeicao`/`_reabrir`; `rederivar_fim`. A declaração/edição do período parcial passa por `decidir` → `_aplicar_periodo`. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:453`, `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:792`, `app/ponto/services/aus

[truncado em 20000 de 39882 caracteres]

## 30/10 20:14 · sessao ab5275193aec · - · - · origem=Ronald (tmux)

- **`ferias.Afastamento` é outra entidade:** cadastro de estado com início/retorno, editável pelo `AfastamentoAdmin`; não possui sync para Ausencia no modelo. O julgamento operacional consulta **`ponto.Ausencia`, tipo `afastamento_inss`**, por `afastamentos_no_dia`, não `ferias.Afastamento`. Não achei receiver de Afastamento nos `app/*/signals.py`, nem chamada cartório/outbox nesse admin/modelo. (`app/ferias/models.py:488`, `app/ferias/models.py:499`, `app/ferias/admin.py:4`, `app/ferias/admin.py:10`, `app/ponto/turnos.py:2683`, `app/ponto/turnos.py:2693`, `app/ponto/turnos.py:2694`; busca negativa nos arquivos citados).

- **Cobertura por substituição não é `cob_status`:** `designar_cobertura` cria `OrdemSubstituicao`; `declarar_cobertura_passada` cria ordem concluída, quando há substituto. Não escrevem DNA/agenda/Ausencia e a ordem não é serializada no hash. Podem mudar hash **via status dos chamados**, pois tentam `ch.reconciliar()`; mudança de status dispara o cartório por signal. O gate pode recusar o fechamento porque a célula/ata não lê essa cobertura. Não há signal de OrdemSubstituicao nos `app/*/signals.py`, nem enqueue direto de rejulgamento/outbox nessas portas. (`app/ponto/services/cobertura.py:22`, `app/ponto/services/cobertura.py:100`, `app/ponto/services/cobertura.py:122`, `app/ponto/services/cobertura.py:246`, `app/ponto/services/cobertura.py:289`, `app/ponto/services/cobertura.py:315`, `app/ponto/services/cobertura.py:318`, `app/ponto/services/cartorio.py:325`, `app/ponto/services/cartorio.py:326`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`; busca negativa nos signals).

- **Folgas têm gatilho efetivo de regeneração/julgamento:** `lancar_folga`, `alterar_folga`, `aplicar_folgas_em_lote` e `gerar_foto_folgas` chamam `relavrar_celulas_da_foto`, que chama `regenerar_celulas_vinculo` com o default `recalculo=True`; esta força `julgar_celula` dos dias tocados até hoje, exceto humanos. Entram indiretamente no hash pelos marcos do DNA que a regeneração altera, sem enqueue direto. (`app/escala/folgas.py:192`, `app/escala/folgas.py:198`, `app/escala/folgas.py:220`, `app/escala/folgas.py:228`, `app/escala/folgas.py:261`, `app/escala/folgas.py:293`, `app/escala/folgas.py:303`, `app/escala/folgas.py:308`, `app/escala/folgas.py:340`, `app/ponto/portas/celula.py:102`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:372`, `app/ponto/services/cartorio.py:327`).

- **Salvar calendário isoladamente não relavra:** `salvar_calendario_de_folga` apenas grava calendário/tamanhos. Já as portas que o **aplicam ao vínculo** chamam `gerar_foto_folgas` e chegam à cadeia acima: `vincular_folga` e aplicação em lote do tipo. (`app/escala/folgas.py:468`, `app/escala/folgas.py:479`, `app/escala/folgas.py:490`, `app/colaboradores/views.py:212`, `app/colaboradores/views.py:239`, `app/colaboradores/views.py:245`, `app/escala/views.py:412`, `app/escala/views.py:414`, `app/escala/views.py:419`).

## 30/10 20:13 · sessao ab5275193aec · - · - · origem=Ronald (tmux)

Consolidação dos insumos sob minha exploração, somente leitura:

**Mecanismo de rejulgamento e outbox**

- Os receivers de Batida e Ausencia chamam diretamente `cartorio.julgar_celula`, pelo helper `_julgar_dias`; não enfileiram julgamento. O helper evita reentrada quando `cartorio.em_julgamento()` é verdadeiro. (`app/ponto/signals.py:85`, `app/ponto/signals.py:90`, `app/ponto/signals.py:93`, `app/ponto/signals.py:131`, `app/ponto/signals.py:143`).
- O outbox encontrado é **posterior à lavra**: `EventoCelula` registra alterações de veredito/ata; `salvar_e_emitir` salva e cria o evento no mesmo `atomic`. (`app/escala/models.py:1800`, `app/ponto/portas/celula.py:623`, `app/ponto/portas/celula.py:626`, `app/ponto/portas/celula.py:634`).
- Seu consumidor chama `retratar_chamados_absolvidos_por_celula` e `encerrar_orfas_pela_lampada`, sem chamar o cartório. Portanto, esse outbox não é uma fila de rejulgamento de `CelulaDia`. (`app/chamados/reconciliador.py:1257`, `app/chamados/reconciliador.py:1300`, `app/chamados/reconciliador.py:1303`).
- A “fila” do comando `processar_cartorio` é uma consulta a colaboradores com células passadas; o comando carrega todas as células passadas e chama `julgar_colab`. (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/management/commands/processar_cartorio.py:65`).
- Busca negativa: procurei `outbox|Outbox|Fila|Pendencia|enfileirar|agendar|rejulgar` nos modelos, signals e services de `app/`; não encontrei tabela/função que enfileire rejulgamento da célula. O outbox encontrado tem o comportamento acima; o comando tem a consulta acima. [não verificado quanto a infraestrutura externa ao repositório].

**(i) Batidas do dia e (ii) do seguinte**

- Entram diretamente por `bs_janela`: para cada batida, `pk`, `tipo`, `timestamp.isoformat()`, unidos por vírgulas. Não entram diretamente `aprovada`, `origem`, foto, justificativa, GPS ou `retratada_em`. (`app/ponto/services/cartorio.py:324`).
- O ramo normal seleciona batidas com data local **D−1, D e D+1**, inclusivamente; a carga é ordenada por timestamp e exclui retratadas. (`app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:871`, `app/ponto/turnos.py:2845`, `app/ponto/turnos.py:2852`).
- `retratada_em` entra indiretamente pela inclusão/exclusão da batida nessa lista. `aprovada=False` isoladamente não exclui a batida, pois o filtro de apuração é `retratada_em__isnull=True`. (`app/ponto/turnos.py:2845`).
- Portas centrais: `registrar_batida` cria; `anotar` muda campos existentes; `retratar_batida` chama `anotar` com `aprovada=False` e `retratada_em=now`; `apagar` faz delete. (`app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:235`, `app/ponto/registro_batida.py:254`, `app/ponto/registro_batida.py:267`, `app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:285`, `app/ponto/registro_batida.py:291`).
- A porta de anotação permite `tipo`, aprovação/autoria/data, origem, retratação, foto, justificativa e pergunta de origem; não permite alterar timestamp. (`app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`).
- `flip_tipo` altera E↔S usando `anotar`; `retratar_humano.retratar` usa `retratar_batida`; `retratar_humano.desfazer` restaura `aprovada=True,retratada_em=None` usando `anotar`. (`app/ponto/services/flip_batida.py:10`, `app/ponto/services/flip_batida.py:30`, `app/ponto/services/retratar_humano.py:150`, `app/ponto/services/retratar_humano.py:179`, `app/ponto/services/retratar_humano.py:190`, `app/ponto/services/retratar_humano.py:207`).
- As views de retratar/desfazer são `retratar_batida_view` e `desfazer_retratacao_view`; ambas chamam esses services. (`app/ponto/views.py:3264`, `app/ponto/views.py:3270`, `app/ponto/views.py:3276`, `app/ponto/views.py:3282`).
- Criadores de produção via `registrar_batida`: API `api_bater_ponto`; PWA `ramo_registrar_es`; aprovação/materialização de justificativa; materialização de resposta hora e data/hora; regularização externa; Resolver dia; ações de chamado; lote de justificativas. (`app/api/views.py:400`, `app/api/views.py:683`, `app/api/views_core.py:387`, `app/api/views_core.py:680`, `app/ponto/services/triagem_batida.py:616`, `app/ponto/services/triagem_batida.py:708`, `app/ponto/services/justificativa.py:117`, `app/ponto/services/justificativa.py:173`, `app/ponto/services/justificativa.py:316`, `app/ponto/services/justificativa.py:346`, `app/chamados/services/materializacao.py:724`, `app/chamados/services/materializacao.py:872`, `app/chamados/services/materializacao.py:923`, `app/chamados/services/materializacao.py:952`, `app/chamados/services/regularizacao_ext.py:36`, `app/chamados/services/regularizacao_ext.py:77`, `app/ponto/services/veredito_celula.py:301`, `app/chamados/services/acoes_chamado.py:44`, `app/ponto/views.py:1764`, `app/ponto/views.py:1816`).
- Save/delete de batida julga **a véspera da data da batida e a própria data**. Assim, batida de D+1 dispara julgamento de D; batida de D−1, embora esteja no hash de D, não dispara D por esse receiver. (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/signals.py:131`, `app/ponto/signals.py:135`, `app/ponto/signals.py:138`, `app/ponto/signals.py:140`).
- Essas portas não inserem diretamente no outbox; o julgamento disparado pode emitir `EventoCelula` se alterar veredito/ata. (`app/ponto/signals.py:93`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:642`).
- Importação AEJ via interface está desativada: `importar_aej` devolve HTTP 410; a URL ainda aponta para essa view; código legado está em `.disabled`. (`app/ponto/views.py:57`, `app/ponto/views.py:65`, `app/ponto/views.py:70`, `app/ponto/urls.py:9`, `app/ponto/legacy/aej_importer.py.disabled:9`).
- Único `bulk_create` de Batida encontrado nos `.py` de `app/` fora testes/migrations: `semear`, restrito a banco de ensaio e recusado sem `BANCO_DE_ENSAIO` ou se o banco se chama `saas_hasner`; não é importação de produção. (`app/ponto/registro_batida.py:316`, `app/ponto/registro_batida.py:326`, `app/ponto/registro_batida.py:331`, `app/ponto/registro_batida.py:335`). Existência de sincronização SQL externa: [não verificado]; o comentário da view apenas a menciona (`app/ponto/views.py:61`).

**(iv) Vínculo vigente/escala**

- O objeto vínculo não é serializado no hash. Do DNA entram somente `dna_versao` e `marcos`; `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `minutos_jornada`, `intervalo_livre` e `lampadas` ficam fora dessa parcela. (`app/ponto/services/cartorio.py:327`, `app/ponto/management/commands/gerar_celulas.py:95`).
- Influência indireta: regeneração transforma o vínculo/template em DNA; alteração de marcos muda o hash. O veto também entra: `_veto_do_dia` resolve vínculo do emissor e `motivo_do_veto`; este testa régua degenerada/suspeita. (`app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:260`, `app/ponto/services/cartorio.py:1011`, `app/ponto/services/cartorio.py:1013`, `app/ponto/services/furos_vetados.py:18`, `app/ponto/services/furos_vetados.py:27`, `app/ponto/services/furos_vetados.py:40`, `app/ponto/services/furos_vetados.py:42`).
- O julgamento ainda lê dados vivos do vínculo via `cadastro_do_dia`, alimentado por todos os vínculos do colaborador e pelas células em julgamento; essa informação não é serializada integralmente no hash. (`app/ponto/services/cartorio.py:839`, `app/ponto/services/cartorio.py:842`, `app/ponto/services/cartorio.py:1118`).
- Módulo de escrita declarado para EscalaColaborador: `colaboradores/services/vinculo.py`. (`app/core/portas.py:465`).
- `executar_vinculo` cria/atualiza o vínculo, regenera células e chama `reconciliar_apos_vinculo`; esta força `julgar_celula` para os dias até hoje, excetuando vereditos humanos e dias já julgados pela regeneração. (`app/colaboradores/services/vinculo.py:167`, `app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:274`, `app/colaboradores/services/vinculo.py:278`, `app/colaboradores/services/vinculo.py:320`, `app/colaboradores/services/vinculo.py:328`, `app/colaboradores/services/pos_vinculo.py:50`, `app/colaboradores/services/pos_vinculo.py:61`, `app/colaboradores/services/pos_vinculo.py:72`).
- `encerrar_vinculo` e `corrigir_vinculo_retroativo` usam save; receivers de alteração de vigência e âncora chamam regeneração. (`app/colaboradores/services/vinculo.py:780`, `app/colaboradores/services/vinculo.py:799`, `app/colaboradores/services/vinculo.py:839`, `app/colaboradores/services/vinculo.py:845`, `app/escala/signals.py:34`, `app/escala/signals.py:71`, `app/escala/signals.py:79`, `app/escala/signals.py:117`).
- A regeneração força julgamento dos dias efetivamente tocados até hoje, sem vereditos humanos, **apenas quando `recalculo=True`**. (`app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:365`, `app/ponto/portas/celula.py:369`, `app/ponto/portas/celula.py:372`).
- Alteração de template: `salvar_tipo` → `_propagar_regeneracao_template` → regeneração com `recalculo=False`; essa porta não força o rejulgamento anterior e não emite outbox só por mudar DNA. (`app/escala/services/cadastro_tipo.py:140`, `app/escala/services/cadastro_tipo.py:306`, `app/escala/services/cadastro_tipo.py:320`, `app/escala/services/cadastro_tipo.py:327`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/cadastro_tipo.py:350`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:623`).
- Portas que fazem `update()` sem signal e não chamam cartório/outbox dentro de seus corpos: `fechar_vigencia`; `reabrir_vigencia_impossivel`; `absorver_vigencias_posteriores`; `invalidar_ancoras_incoerentes`; `reverter_para_snapshot`. Chamadores podem executar ações posteriores, como faz `executar_vinculo`. (`app/colaboradores/services/vinculo.py:561`, `app/colaboradores/services/vinculo.py:615`, `app/colaboradores/services/vinculo.py:635`, `app/colaboradores/services/vinculo.py:691`, `app/colaboradores/services/vinculo.py:710`, `app/colaboradores/services/vinculo.py:753`, `app/colaboradores/services/vinculo.py:849`, `app/colaboradores/services/vinculo.py:857`, `app/colaboradores/services/vinculo.py:860`, `app/colaboradores/services/vinculo.py:874`).
- Outras portas: `trocar_calendario_de_folga`, `marcar_trabalha_em_feriado`, `criar_trecho_retroativo`, `ajustar_vinculo_pelo_sistema`, `desfazer_ajuste_do_sistema`. As duas primeiras salvam chaves que não são vigência/âncora; os receivers de regeneração só detectam as mudanças que seus nomes indicam e pulam `created`. (`app/colaboradores/services/vinculo.py:433`, `app/colaboradores/services/vinculo.py:445`, `app/colaboradores/services/vinculo.py:822`, `app/colaboradores/services/vinculo.py:825`, `app/colaboradores/services/vinculo.py:829`, `app/colaboradores/services/vinculo.py:832`, `app/colaboradores/services/vinculo.py:925`, `app/colaboradores/services/vinculo.py:930`, `app/colaboradores/services/vinculo.py:936`, `app/colaboradores/services/vinculo.py:940`, `app/escala/signals.py:54`, `app/escala/signals.py:58`, `app/escala/signals.py:104`, `app/escala/signals.py:108`).
- O escore/corroborração da régua é escrito por `gravar_regua`, com update sem signal, e pode mudar o `veto` do hash. (`app/colaboradores/services/vinculo.py:889`, `app/colaboradores/services/vinculo.py:911`, `app/ponto/services/furos_vetados.py:42`).

**(v) Ausências/coberturas, inclusive férias**

- Entram pelo **status da ausência vencedora**, argumento `cob_status`; não entram diretamente pk, tipo, minutos ou datas. A vencedora vem de `ausencia_cobre(c,D,para='cobranca')`, que exclui rejeitadas, seleciona cobertura de dia/range aberto/fechado e desempata por `-data,-pk`. (`app/ponto/services/cartorio.py:325`, `app/ponto/services/cartorio.py:1041`, `app/ponto/services/cartorio.py:1083`, `app/ponto/turnos.py:2487`, `app/ponto/turnos.py:2490`, `app/ponto/turnos.py:2525`, `app/ponto/turnos.py:2530`).
- Datas/range entram indiretamente por determinar se há cobertura e qual vence; tipos/minutos/horários parciais podem alterar `cob_parcial`/`periodo_parcial`, lidos da grade e adicionados como `cobparcial:1[:estado]`. Isso não serializa os horários/minutos integralmente. (`app/ponto/services/cartorio.py:1079`, `app/ponto/services/cartorio.py:1088`, `app/ponto/services/cartorio.py:375`, `app/ponto/services/cartorio.py:379`).
- Portas principais: `criar_ausencia` cria com colaborador/tipo/datas/minutos/status; `editar_ausencia_registro` altera tipo/range/minutos; `mudar_estado` altera status e salva; `decidir` passa por `mudar_estado` e pode escrever `hora_ini/hora_fim`. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:508`, `app/ponto/services/ausencia.py:792`, `app/ponto/services/ausencia.py:819`, `app/ponto/services/ausencia.py:875`, `app/ponto/services/ausencia.py:881`, `app/ponto/services/ausencia.py:895`, `app/ponto/services/ausencia.py:908`, `app/ponto/services/ausencia.py:911`).
- Essas escritas por save/delete disparam cartório diretamente pelo receiver; dias vêm de `_dias_ausencia`, usando as datas **atuais** da instância, aberto até ontem e limitado a hoje, filtrando células existentes quando o range tem mais de três dias. Não há captura do range anterior nesse receiver/helper. (`app/ponto/signals.py:115`, `app/ponto/signals.py:123`, `app/ponto/signals.py:124`, `app/ponto/signals.py:127`, `app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/signals.py:150`, `app/ponto/signals.py:152`).
- Views usuárias dessas portas incluem lançar/submeter, aprovar/rejeitar/cancelar individual e lote, editar registro e editar período parcial. (`app/ponto/views.py:2260`, `app/ponto/views.py:2280`, `app/ponto/views.py:2294`, `app/ponto/views.py:2309`, `app/ponto/views.py:2323`, `app/ponto/views.py:2341`, `app/ponto/views.py:2358`, `app/ponto/views.py:2405`, `app/ponto/views.py:2457`, `app/ponto/views.py:2526`, `app/ponto/views.py:2589`, `app/ponto/views.py:2652`, `app/ponto/views.py:2663`, `app/ponto/views.py:2670`, `app/ponto/views.py:2710`, `app/ponto/views.py:2741`, `app/ponto/views.py:2784`, `app/api/views.py:1738`, `app/api/views.py:1872`).
- Férias: `AgendamentoFerias.save()` chama `_sync_ausencia` → `espelhar_ferias`; este cria uma Ausencia aprovada por dia via `criar_ausencia` e apaga as que deixam de valer; o delete do agendamento também apaga ausências. São essas criações/deleções que acionam o cartório. (`app/ferias/models.py:384`, `app/ferias/models.py:434`, `app/ferias/models.py:437`, `app/ferias/models.py:439`, `app/ponto/services/ausencia.py:1144`, `app/ponto/services/ausencia.py:1172`, `app/ponto/services/ausencia.py:1186`, `app/ponto/services/ausencia.py:1195`, `app/ponto/services/ausencia.py:1246`, `app/ponto/services/ausencia.py:1254`).
- Cancelamento DP salva status cancelado; cancelamento admin primeiro rejeita ausências por update sem signal, depois salva o agendamento cancelado, cujo sync apaga as ausências e dispara post_delete. Correção de datas apaga o range antigo e salva/sincroniza o novo. (`app/ferias/services.py:330`, `app/ferias/services.py:337`, `app/ferias/services.py:341`, `app/ferias/services.py:356`, `app/ferias/services.py:362`, `app/ponto/services/ausencia.py:1257`, `app/ponto/services/ausencia.py:1267`, `app/ferias/services.py:366`, `app/ferias/services.py:385`, `app/ferias/services.py:387`).
- Nenhuma dessas portas enfileira rejulgamento em outbox; os saves/deletes chamam diretamente cartório e a eventual lavra emite o outbox posterior descrito acima. (`app/ponto/signals.py:93`, `app/ponto/signals.py:147`, `app/ponto/signals.py:152`, `app/ponto/portas/celula.py:638`).

**(vi) Feriados da praça**

- Não há argumento/chave de feriado no hash. Pode entrar indiretamente **se uma geração/regeneração atualizar os marcos**: `eh_dia_trabalho_calculado` aplica `feriado_vira_folga`, que consulta o feriado pelo município do posto; DNA só monta marcos quando `trabalha is True`. (`app/ponto/services/cartorio.py:314`, `app/ponto/services/cartorio.py:323`, `app/escala/models.py:1338`, `app/escala/models.py:1340`, `app/escala/models.py:1344`, `app/escala/models.py:1362`, `app/escala/models.py:1363`, `app/ponto/management/commands/gerar_celulas.py:79`, `app/ponto/management/commands/gerar_celulas.py:100`, `app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:260`).
- O alcance é nacional/UF/município; o lugar do colaborador vem de `colaborador.posto.municipio`. (`app/core/feriados.py:57`, `app/core/feriados.py:60`, `app/core/feriados.py:62`, `app/core/feriados.py:64`, `app/core/feriados.py:106`, `app/core/feriados.py:113`).
- Portas de calendário: `form_feriado` cria/edita; `_importar_do_payload`, chamado por `importar_feriados_nacionais`, faz get_or_create; `excluir_feriado` apaga. (`app/core/views_config.py:39`, `app/core/views_config.py:62`, `app/core/views_config.py:68`, `app/core/views_config.py:81`, `app/core/views_config.py:124`, `app/core/views_config.py:141`, `app/core/views_config.py:153`, `app/core/views_config.py:164`, `app/core/views_config.py:182`, `app/core/views_config.py:198`).
- Alteração do lugar também muda a aplicabilidade: portas `atualizar_posto`/`definir_municipio_posto`; a segunda usa update sem signal. (`app/colaboradores/services/estrutura.py:47`, `app/colaboradores/services/estrutura.py:50`, `app/colaboradores/services/estrutura.py:67`, `app/colaboradores/services/estrutura.py:70`).
- O signal de Feriado roda `reavaliar_dia(data)` em on_commit; não regenera células nem enfileira julgamento. Ele busca chamados `batida_ausente` vivos, consulta `fatos_do_dia` e reconcilia os espúrios. A reconciliação pode provocar rejulgamento **indireto** pela mudança de status do chamado. (`app/chamados/signals.py:265`, `app/chamados/signals.py:274`, `app/chamados/signals.py:276`, `app/chamados/services/feriado_reavaliar.py:14`, `app/chamados/services/feriado_reavaliar.py:33`, `app/chamados/services/feriado_reavaliar.py:43`, `app/chamados/services/feriado_reavaliar.py:47`, `app/chamados/services/feriado_reavaliar.py:59`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`).
- Não há post_delete de Feriado: o código declara isso expressamente, porque essa reavaliação só fecha chamados e não reabre retratados quando o feriado é removido. (`app/chamados/signals.py:261`, `app/chamados/signals.py:262`).
- O eventual efeito sobre chamados muda a parcela `pk:status_local` do hash; o feriado em si continua ausente. (`app/ponto/services/cartorio.py:326`).

**(ix) Isenção art. 62**

- Flag real: `Colaborador.isento_ponto`. A flag não entra diretamente no hash, nem no DNA produzido por `montar_dna`; contudo é entregue diretamente ao juiz como `isento_ponto=bool(c.isento_ponto)`. (`app/colaboradores/models.py:294`, `app/ponto/services/cartorio.py:323`, `app/ponto/services/cartorio.py:327`, `app/ponto/management/commands/gerar_celulas.py:95`, `app/ponto/services/cartorio.py:1113`).
- Não encontrei caminho que codifique a flag indiretamente no hash: o signal da isenção silencia chamados, conservando seu status, e o hash lê status, não `silenciado_ate`. (`app/chamados/signals.py:213`, `app/chamados/signals.py:222`, `app/chamados/services/silencio_isentos.py:34`, `app/chamados/services/silencio_isentos.py:43`, `app/chamados/services/silencio_isentos.py:46`, `app/ponto/services/cartorio.py:326`).
- Porta humana `passe_livre` chama `colaboradores.services.colaborador.gravar(colab,'cadastro',{'isento_ponto':novo_valor})`; `gravar` atribui e salva. (`app/colaboradores/views.py:145`, `app/colaboradores/views.py:173`, `app/colaboradores/views.py:176`, `app/colaboradores/services/colaborador.py:34`, `app/colaboradores/services/colaborador.py:47`, `app/colaboradores/services/colaborador.py:53`).
- Essa porta não chama cartório nem emite outbox. O recei

[truncado em 20000 de 20717 caracteres]

## 30/10 20:13 · sessao 82751e7ef508 · - · - · origem=Ronald (tmux)

Censo concluído, somente leitura. Não carreguei Django nem executei testes; usei leitura estática, `rg` e o varredor AST puro com `PYTHONDONTWRITEBYTECODE=1`. Resultado de execução vivo de `_meta` permanece [não verificado]. [nao verificado]

**Inventário: 24 campos concretos + 2 relações reversas = 26 entradas de `get_fields()` pela leitura estática.** A classe herda diretamente `models.Model` (`app/escala/models.py:1603`); são 23 declarações de campos (`app/escala/models.py:1649` até `:1773`) e `id` implícito BigAutoField, registrado na migration (`app/escala/migrations/0030_celuladia.py:20`; configuração `app/escala/apps.py:5`).

Nas referências abaixo, **P = `app/ponto/portas/celula.py`**. O save comum de lavratura/carimbo é `salvar_e_emitir`, P:634–642.

| Campo | Definição | Escritores de produção |
|---|---|---|
| `id` | migration 0030:20 | Automático nos creates de `regenerar_celulas_vinculo` P:247 e `gerar_celulas_periodo` P:593. |
| `colaborador` | models:1649 | Creates P:247 e P:594. |
| `data` | models:1653 | Creates P:247 e P:594. |
| `trabalha` | models:1654 | Creates P:247/P:595; regenerar vínculo P:231/P:260, saves P:233/P:264; gerar período P:612/P:615; reverter P:532/P:538. |
| `origem` | models:1659 | Somente `'gerada'` nos creates P:248/P:595. Nenhum escritor de produção de `'editada'` localizado. |
| `escala_geradora` | models:1660 | Creates P:248/P:595; regenerar P:260/P:264; gerar P:613/P:615; reverter P:535/P:538. `SET_NULL` declarado em models:1661. |
| `dna` | models:1665 | Creates P:248/P:596; regenerar P:232–233/P:260–265; gerar P:604–605/P:614–615; reverter P:530/P:538. |
| `gerada_em` | models:1670 | `auto_now_add` nos creates P:247/P:593. |
| `regenerada_em` | models:1686 | Regenerar P:262/P:264; reverter P:536/P:538. |
| `regeneracoes` | models:1691 | Default 0; regenerar P:263/P:264; reverter P:537/P:538. |
| `dna_anterior` | models:1694 | Regenerar P:261/P:264; reverter limpa P:531/P:538. |
| `editada_em` | models:1700 | Nenhum escritor de produção localizado. |
| `editada_por` | models:1701 | Nenhum escritor de produção localizado; `SET_NULL` declarado em models:1702. |
| `julgada_em` | models:1713 | `carimbar`, P:721/P:726. |
| `impressao` | models:1714 | `carimbar`, P:722/P:726. |
| `veredito` | models:1738 | `lavrar_veredito`, P:692/P:695. |
| `veredito_via` | models:1739 | `lavrar_veredito`, P:693/P:695. |
| `veredito_em` | models:1740 | `lavrar_veredito`, P:694/P:695. |
| `protestos` | models:1741 | `lavrar_veredito`, P:691/P:695. |
| `ata` | models:1746 | `lavrar_veredito`, P:674/P:690/P:697; `carimbar_precedencia`, P:739–743. |
| `ausencia` | models:1764 | `carimbar`, P:723/P:726; `carimbar_insumos_em_lote`, P:759/P:764. `SET_NULL` models:1765. |
| `tem_entrada` | models:1769 | `carimbar`, P:724/P:726; lote P:760/P:764. |
| `tem_saida` | models:1770 | `carimbar`, P:724/P:726; lote P:760/P:764. |
| `insumos_em` | models:1773 | `carimbar`, P:725/P:727; lote P:761/P:764. |
| `chamados` reverso | `app/chamados/models.py:44–45` | Não é coluna da célula: nasce pela FK no create de chamado, `models.py:876`/`:905`; associação posterior por `services/vinculos.py:92–99`. |
| `perguntas` reverso | `app/chamados/models.py:1943–1944` | Não é coluna da célula: `PerguntaDisputa.abrir` reaponta FK em `models.py:2167–2179` ou cria em `:2181–2182`. |

`regenerar_celulas_dia` apenas delega ao regenerador de vínculo com `[dia,dia]`, sem extensão (`P:474–477`). O wrapper de modelo `CelulaDia.lavrar_veredito` delega para a porta (`app/escala/models.py:1775–1783`).

**Não existe relação reversa `eventos_consumo` localizada.** `EventoCelula.celula_id` é `IntegerField`, explicitamente sem FK (`app/escala/models.py:1806–1809`).

**Selos encontrados:**

- Escritor único por módulo, allowlist vazia: `app/ponto/tests/test_chokepoint_celula_gate.py:20–22`, teste da árvore `:49–53`, entidades `:58–60`, operações declaradas e cobertura AST `:62–84`, contraprovas `:86–96`.
- Cadastro da família/porta: `app/core/portas.py:44`, `:111`.
- Varredor AST: `app/core/censo_escritas.py:6–45`; as classes de inferência estão documentadas ali, inclusive balde não atribuível.
- Contraprovas do censo envolvendo CelulaDia: `app/core/tests/test_censo_escritas.py:15–16`, `:72–90`; gerente reverso `:143–157`; produção exclui testes e inclui management `:230–261`.
- Idempotência das operações: `app/ponto/tests/test_porta_celula.py:36–60`, lavrar `:63–76`, carimbar `:79–92`, precedência/ata `:94–139`, lote `:142–150`.
- Veredito verifica conjunto dos **quatro campos retornados pela lavratura**, não conjunto de fields do modelo: `app/escala/tests/test_b5_veredito.py:37–42`.
- **Nenhum teste do conjunto completo de fields de CelulaDia localizado** ao buscar `CelulaDia._meta`, `_meta`, `get_fields`, `escritor unico`, `escritor_unico`, `escritores` em `app/*/tests`. Os selos acima são de módulo/operacão ou comportamento, não um mapa completo campo→escritores. [nao verificado]

**(iii) DNA:**

- Hash só usa `dna_versao` e `str(marcos)`, não o JSON inteiro (`app/ponto/services/cartorio.py:327`). Logo alterar outra chave isoladamente não muda diretamente hash.
- Portas que realmente gravam: gerar período, regenerar vínculo/dia e reverter, nos sítios da tabela.
- Regenerar vínculo, com `recalculo=True`, julga diretamente os dias tocados não futuros e sem veredito humano, com `forcar=True` (`P:360–379`). O wrapper por dia usa esse default (`P:474–477`).
- Gerar período termina sem chamada ao cartório/outbox (`P:590–617`). Reverter termina após save/trilha, sem julgamento/outbox (`P:529–552`).
- Não encontrei receiver de `CelulaDia` em `app/`; comentário declara ausência intencional do signal (`P:332–334`).
- Regeneração não insere diretamente `EventoCelula`; uma lavra posterior pode inserir se mudar `veredito` ou `ata` (`P:622–642`).

**(viii) Decisões humanas:**

1. **Justificativa**: `status`, aprovação, tipo etc. não entram diretamente na função hash; não há parâmetro/chave de justificativa (`cartorio.py:314–380`). Pode mudar hash por batida criada, por status dos chamados fechados e por `fio`; a justificativa sem esses efeitos pode manter impressão idêntica, apesar de signal chamar julgamento (`cartorio.py:324–327`, `:367–368`; sinais abaixo).
   - Individual: `aprovar_justificativa_dp` (`app/ponto/services/justificativa.py:117`, grava `:190–193`), `rejeitar_justificativa_dp` (`:247`, `:268–272`), `materializar_saida_dp` (`:316`, `:368–371`).
   - Lote: `aprovar_lote_justificativas` (`app/ponto/views.py:1764`, grava `:1797–1800`), rejeitar lote (`:1860`, grava `:1876–1880`).
   - Admin genérico editável: `JustificativaAdmin(admin.ModelAdmin)` (`app/ponto/admin.py:18–22`); dívida reconhecida pelo selo em `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:12–15`.
   - Todo save aciona julgamento **síncrono**, não fila de rejulgamento: `app/ponto/signals.py:161–165` → `:85–93`.
   - Aprovação pode criar batida (`services/justificativa.py:172–179`) e fecha origem (`:202`); lote cria batida (`views.py:1816–1822`) e fecha origem (`:1838`).

2. **Edição de célula `origem='editada'`**: **nenhuma porta de produção localizada**, embora modelo reconheça a origem (`app/escala/models.py:1658–1659`) e gerador a preserve (`P:598–599`). `editada_em/editada_por` sem escritores, como acima. Não afirmar que uma porta humana de edição existe só por comentários do modelo (`models.py:1608–1611`).

3. **Veredito humano diretamente no dia**: view `app/ponto/views.py:3302–3304` → serviço `veredito_celula` (`app/ponto/services/veredito_celula.py:191`).
   - `trabalhou` pode flipar/gravar batidas (`:291–305`); `faltou/justificado` cria Ausencia (`:310–317`); fecha fio (`:324–328`); escreve veredito admin (`:329–330`).
   - `veredito_via`, veredito e autor não entram diretamente hash (`cartorio.py:323–380`). Seus fatos materiais entram pelas batidas/cobertura/chamados/fio.
   - A lavra admin chama `salvar_e_emitir` (`P:682–697`) e **gera EventoCelula se houver mudança**, pelo mesmo atomic (`P:634–642`). Esse outbox reconcilia chamados, não enfileira rejulgamento do próprio cartório (`app/escala/models.py:1803–1807`).

4. **Decisão em chamado**: `ChamadoColaborador.fechar` → `transicionar` → `_gravar_estado` (`app/chamados/models.py:260–264`), save em `:231`.
   - Entra no hash por `pk:status_local` (`cartorio.py:326`).
   - Criar/mudar status aciona cartório síncrono: pre/post-save `app/chamados/signals.py:155–180`, sem outbox de rejulgar.

5. **Decisão em pergunta**:
   - `validar_pergunta` (`app/chamados/services/validacao.py:336`) carimba (`:389`, escritor `:127–133`) e materializa (`:415–423`).
   - `recusar_pergunta` (`:487`) salva recusa e chama reabertura (`:538–540`).
   - Fecho admin explícito `PerguntaDisputa.fechar_por_admin` (`app/chamados/models.py:2213–2230`).
   - Pergunta muda hash **indiretamente via contagem de chamados com fio vivo** (`cartorio.py:1074`, `:1087`).
   - Receiver direto da pergunta só fecha beco da disputa, `on_commit`, **sem chamada a julgar_celula e sem inserir outbox** (`app/chamados/signals.py:191–202`).
   - Validação pode disparar julgamento pelos fatos materializados (Batida/Ausencia/Justificativa); o `recalcular_por_evento` da validação (`validacao.py:399–401`) é do fechamento financeiro, não fila do cartório.

## 30/10 20:12 · sessao b72efd86392b · - · - · origem=Ronald (tmux)

CCT / item 2(vii)

- **Não entra no hash diretamente nem indiretamente pelo DNA**: `impressao_insumos` só concatena batidas, status da cobertura, chamados, `dna_versao`, `marcos`, veto, teto, versão da regra, encerramento, fio e parcialidade; nenhum campo da CCT aparece (`app/ponto/services/cartorio.py:314-380`). O produtor do DNA não lê CCT: usa `EscalaColaborador`, `TipoEscala`, marcos, jornada, intervalos e extras declaradas (`app/ponto/management/commands/gerar_celulas.py:73-123`).
- **O argumento `regra` não é a régua CCT**: vem de `versao_para`/`REGRA_VERSOES`, declaração de versão do código do cartório (`app/ponto/services/cartorio.py:100`, `app/ponto/services/cartorio.py:168-187`, `app/ponto/services/cartorio.py:888-891`). Alterar `Empresa.regime_trabalhista`, Sindicato ou AplicacaoConvencao não altera essa versão (`app/ponto/services/cartorio.py:168-187`).
- A régua efetiva é decidida por `regua_para`: vínculo do dia → praça e ciclo (`app/core/regua_cct.py:328-339`); `Empresa.regime_trabalhista` decide CLT/CCT (`app/core/regua_cct.py:352-366`); `_aplicacao_vigente` busca aplicações ativas da empresa, praça ou empresa inteira, confere vigência do Sindicato, prioriza praça e recusa empate (`app/core/regua_cct.py:253-275`); os valores são extraídos do Sindicato (`app/core/regua_cct.py:374-380`).
- `get_motor_cct` lê o switch global `ParametroSistema.regua_cct_ativa` e injeta a régua no motor de cálculo; trata-se do caminho do motor/folha (`app/core/regua_cct.py:428-470`). A régua expõe sete campos declarados sem efeito no cálculo e quatro que chegam ao motor (`app/core/regua_cct.py:72-89`).

**Portas que mudam CCT/regime e seus disparos**

| Estado | Porta de produção | Rejulgamento / outbox |
|---|---|---|
| Campos/vigência de Sindicato | `form_sindicato`: coleta régua (`app/core/views_config.py:258`, `app/core/views_config.py:268-297`), atualiza e salva (`app/core/views_config.py:303-321`) ou cria (`app/core/views_config.py:323-336`). | Não chama juiz nem emite evento: termina em trilha, atualização de `VinculoSindicatoPraca` e resposta (`app/core/views_config.py:320-343`). |
| AplicacaoConvencao | `form_aplicacao` chama `cadastrar` ou `desativar` (`app/core/views_config.py:387`, `app/core/views_config.py:402-417`). `semear_aplicacao_convencao.handle` chama as mesmas portas (`app/core/management/commands/semear_aplicacao_convencao.py:105-111`). Escritor: criação/reativação/desativação em `cadastrar` (`app/core/services/aplicacao_convencao.py:101-134`) e desativação em `desativar` (`app/core/services/aplicacao_convencao.py:138-157`). | Não chama juiz nem outbox: salva e registra auditoria, depois retorna (`app/core/services/aplicacao_convencao.py:120-134`, `app/core/services/aplicacao_convencao.py:149-157`). |
| Empresa.regime_trabalhista | `form_empresa` valida o valor recebido (`app/colaboradores/views.py:485`, `app/colaboradores/views.py:512-515`) e passa às portas `atualizar_empresa` ou `criar_empresa` (`app/colaboradores/views.py:519-540`); escritores genéricos em `app/colaboradores/services/estrutura.py:12-20`. | Não chama juiz nem outbox: as portas só fazem create/setattr/save (`app/colaboradores/services/estrutura.py:12-21`). |

- O admin não é porta alternativa para esses dois estados: regime é readonly (`app/colaboradores/admin.py:19`); AplicacaoConvencao usa `SoLeitura` (`app/core/admin.py:99-102`), que proíbe adicionar, mudar e apagar (`app/core/admin.py:59-66`).
- Não encontrei receivers de `Empresa`, `Sindicato` ou `AplicacaoConvencao` buscando `sender` em todos os `.py` de `app/`; os signals inspecionados não contêm esses ganchos (`app/core/signals.py:1-48`, `app/colaboradores/signals.py:1-119`, `app/ponto/signals.py:85-165`, `app/chamados/signals.py:155-202`).
- Não encontrei escritor de produção do switch `regua_cct_ativa` pesquisando o nome e `ParametroSistema` em `app/core/views_config.py`, `app/core/services/` e `app/core/management/commands/`; a tela de parâmetros é somente leitura e recusa POST (`app/core/views_config.py:480-492`). [não verificado fora desse escopo]

**Mecanismo de outbox — distinção essencial**

- O outbox real é `escala.models.EventoCelula`, com `celula_id`, `colaborador_id`, data, criação, processamento, tentativas e erro (`app/escala/models.py:1800-1815`).
- Ele **não é uma fila de rejulgamento do cartório**: registra a lavra da célula e acorda o consumidor de chamados (`app/escala/models.py:1801-1807`). O consumidor chama `retratar_chamados_absolvidos_por_celula` e `encerrar_orfas_pela_lampada`, não o juiz do cartório (`app/chamados/reconciliador.py:1257-1268`, `app/chamados/reconciliador.py:1295-1306`).
- A emissão é centralizada em `emitir_evento_celula` (`app/ponto/portas/celula.py:626-631`). `salvar_e_emitir` grava célula e evento no mesmo atomic, somente se os campos lavrados incluem `veredito` ou `ata` (`app/ponto/portas/celula.py:622-642`). A mera gravação de DNA ou dos insumos do carimbo não aciona esse critério (`app/ponto/portas/celula.py:623`, `app/ponto/portas/celula.py:708-728`).
- O nascimento de chamado de turno ligado à célula também emite esse evento (`app/chamados/models.py:735-744`); há semeadura pela mesma porta (`app/chamados/reconciliador.py:1223-1253`).
- O consumidor roda a cada minuto (`app/config/crons.py:375`), seleciona pendentes com menos de cinco tentativas (`app/chamados/reconciliador.py:1154`, `app/chamados/reconciliador.py:1191-1200`), mantém pendências por falha/adiamento ou marca processamento (`app/chamados/reconciliador.py:1311-1320`).
- O rejulgamento reativo dos insumos é **chamada síncrona direta**: `_julgar_dias` chama `cartorio.julgar_celula`, com guarda antirrecursão (`app/ponto/signals.py:85-93`); os receivers de Batida, Ausencia e Justificativa chamam esse helper (`app/ponto/signals.py:131-165`). Mudança/criação de chamado chama diretamente o mesmo juiz (`app/chamados/signals.py:155-180`).
- A “fila” do comando `processar_cartorio` é um queryset de colaboradores com células passadas, não tabela de pendências (`app/ponto/management/commands/processar_cartorio.py:45-65`); o cron o chama com `--apply`, sem `--forcar` (`app/config/crons.py:597`). O pulo usa igualdade da impressão (`app/ponto/services/cartorio.py:1092-1094`).
- `out['rejulgar']` também não é outbox: é lista em memória consumida na segunda passada; sobras são descartadas e contadas (`app/ponto/services/cartorio.py:745-753`).
- Não encontrei modelo próprio `*Outbox*`, `*Pendencia*` ou `*Fila*` de rejulgamento do cartório no censo de classes/modelos e ocorrências `outbox|Outbox|enfileirar|julgar_celula|julgar_colab` em `app/**/*.py`, excluindo migrations e testes; o mecanismo concreto encontrado é o acima (`app/ponto/management/commands/processar_cartorio.py:49-65`, `app/ponto/signals.py:85-93`, `app/escala/models.py:1800-1815`, `app/chamados/reconciliador.py:1295-1306`).

## 30/10 19:54 · sessao 138b78f0be18 · O261-E-CELULA-20 · censo · origem=Code

**O bloqueio nasce da ordem das operações:** F1 exige que a pergunta deixe de ser viva antes de fechar o chamado; nesse caminho, a célula só deixa de acusar depois do fechamento e do rejulgamento. O C6 continua vendo a célula acusadora durante a nova guarda (`app/chamados/models.py:601`; `app/ponto/services/cartorio.py:1163`; `app/ponto/services/cartorio.py:1166`; `app/chamados/juizes.py:1056`).

Uso `B/` para os arquivos da base no diretório `e54eae6ed8c46bc652434308bf918485dd3ac23e`; caminhos sem esse prefixo são da cópia. Nos anexos, numero as linhas desde `Found 29 test(s).`.

**(1) Escritor e leitores de `cobrado`**

O cartório escolhe `_vd='cobrado'` quando, simultaneamente:

- `cods` está vazio;
- existe chamado vivo naquele dia;
- algum desses chamados tem pergunta viva entre `perguntas_que_resolvem`. O predicado do fio termina em `pergunta_viva` (`app/ponto/services/cartorio.py:1069`; `app/ponto/services/cartorio.py:1074`; `app/ponto/services/cartorio.py:1122`; `app/ponto/services/cartorio.py:1141`; `app/chamados/juizes.py:1111`).

A escolha entra em `cel.lavrar_veredito`; a atribuição efetiva é `cel.veredito = v` na porta da célula. O cartório persiste pelo `carimbar`. Uma decisão anterior com `veredito_via='admin'` impede essa substituição pelo cartório (`app/ponto/services/cartorio.py:1409`; `app/ponto/portas/celula.py:672`; `app/ponto/portas/celula.py:692`; `app/ponto/services/cartorio.py:1424`; `app/ponto/portas/celula.py:728`).

`VEREDITOS_ACUSAM` contém `furo`, `discordante`, `indefinida`, `cobranca_indevida`, **`cobrado`**, `nunca_bateu` e `fato_em_ausencia`. O C6 retorna `True` ao encontrar esse pertencimento. A saída anterior por marco aceso exige `materializada_em` preenchido (`app/escala/models.py:1735`; `app/chamados/juizes.py:1050`; `app/chamados/juizes.py:1056`).

Censo das leituras diretas do literal ou da constante, fora de testes e migrations: [nao verificado]

| Uso | Arquivo:linha |
|---|---|
| C6 e consulta de perguntas vivas | `app/chamados/juizes.py:1002`, `app/chamados/juizes.py:1056`, `app/chamados/models.py:2523`, `app/chamados/models.py:2524` |
| Absolvição de perguntas e chamados | `app/chamados/reconciliador.py:598`, `app/chamados/reconciliador.py:978` |
| Emissão, silêncio, resposta tardia e LIMBO | `app/chamados/services/fabrica_da_celula.py:86`, `app/chamados/services/disputa_emissao.py:705`, `app/chamados/services/silencio_ausencia.py:41`, `app/chamados/services/resposta_tardia.py:45`, `app/chamados/services/limbo_folgas.py:194` |
| Folha e apresentação | `app/folha/export.py:1215`, `app/relatorios/dia_da_frota.py:55`, `app/relatorios/furos.py:25`, `app/colaboradores/services/calendario.py:70` |
| Vigias | `app/chamados/services/furo_sem_canal.py:35`, `app/chamados/services/vigia_fio.py:182`, `app/folha/services/vigia_pausa.py:155`, `app/ponto/services/vigia_ausencia_veredito.py:41` |
| Comandos | `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `app/chamados/management/commands/disparar_perguntas_competencia.py:86`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:133`, `app/chamados/management/commands/auditar_invariantes_chamados.py:197`, `:342`, `:1387`, `:1601`, `:1643` |

**(2) Sequência da base**

1. `registrar_batida` cria a batida; o receiver do cartório chama `_julgar_dias`, que chama `julgar_celula` (`B/app/ponto/registro_batida.py:157` → `B/app/ponto/signals.py:132` → `B/app/ponto/signals.py:135` → `B/app/ponto/signals.py:93`).

2. `julgar_celula` chama `julgar_colab`; este executa `_julgar_colab_corpo`. O corpo monta a ata atual, disponibiliza-a no contexto do julgamento e chama `x.reconciliar()` quando os marcos cobrados acenderam (`B/app/ponto/services/cartorio.py:1560` → `B/app/ponto/services/cartorio.py:744` → `B/app/ponto/services/cartorio.py:1148` → `B/app/ponto/services/cartorio.py:1152` → `B/app/ponto/services/cartorio.py:1163`).

3. `reconciliar` consulta motor e premissa. Com a premissa regularizada, chama `_gravar_estado`, que atribui o novo status e salva (`B/app/chamados/models.py:440` → `B/app/chamados/models.py:467` → `B/app/chamados/models.py:470` → `B/app/chamados/models.py:495` → `B/app/chamados/models.py:444` → `B/app/chamados/models.py:192` → `B/app/chamados/models.py:231`).

4. **É após esse save que existe o intervalo “chamado terminal, pergunta ainda viva”.** A célula ainda pode apresentar o veredito acusador anterior, e a pergunta sem materialização não passa pela exceção do marco aceso. O C6 retorna viva antes de chegar ao fallback que examina o destino terminal. O receiver do chamado evita rejulgamento recursivo durante o cartório (`B/app/chamados/models.py:231`; `B/app/chamados/juizes.py:1050`; `B/app/chamados/juizes.py:1056`; `B/app/chamados/juizes.py:1083`; `B/app/chamados/signals.py:174`).

5. O fechamento enfileira a célula para rejulgamento. A primeira lavra ainda pode usar `_vd='cobrado'`, calculado antes do fechamento; a segunda passada relê os vivos. Sem protestos e sem aquela cobrança viva, deriva `concorde` (`B/app/ponto/services/cartorio.py:1142` → `B/app/ponto/services/cartorio.py:1166` → `B/app/ponto/services/cartorio.py:1409` → `B/app/ponto/services/cartorio.py:746` → `B/app/ponto/services/cartorio.py:1069`; `B/app/escala/models.py:1596`).

6. O receiver posterior da batida chama `reconciliar_chamados_colab`. Seu primeiro consumidor de absolvição seleciona células não acusadoras, confirma C6 falso e grava `materializada_em`, autor e `via_resolucao='celula_absolveu'` (`B/app/ponto/signals.py:207` → `B/app/ponto/signals.py:215` → `B/app/chamados/reconciliador.py:1350` → `B/app/chamados/reconciliador.py:598` → `B/app/chamados/reconciliador.py:613` → `B/app/chamados/reconciliador.py:617` → `B/app/chamados/reconciliador.py:619`).

O resultado fornecido confirma um chamado fechado e a pergunta materializada por `celula_absolveu`, célula `concorde`, C6 falso (`anexo debug_base.log:4`; `anexo debug_base.log:5`).

**(3) Dependências que retêm a cópia**

- **Fechamento → pergunta:** `_decisao_do_fechamento` consulta primeiro `_plano_pergunta_viva`. Havendo viva sem reata válida, retorna `None` antes do gate da premissa (`app/chamados/models.py:601`; `app/chamados/models.py:604`; `app/chamados/models.py:605`).
- **Pergunta → célula:** `_vivas_ancoradas` simula somente o status novo do chamado. Ela não simula a futura lavra da célula. O C6 continua lendo o veredito acusador atual (`app/chamados/models.py:304`; `app/chamados/models.py:310`; `app/chamados/models.py:311`; `app/chamados/juizes.py:1056`).
- **Marco aceso → materialização prévia:** a luz acesa só encerra a pergunta nesse degrau se ela já tiver `materializada_em`. A pergunta da sonda não tem esse carimbo (`app/chamados/juizes.py:1050`; `anexo debug_cura.log:7`).
- **Chamado vivo + pergunta viva → `cobrado`:** recusado o fechamento, ambos permanecem no cálculo do fio. Sem protestos, o cartório escolhe `cobrado`, que também acusa (`app/ponto/services/cartorio.py:1074`; `app/ponto/services/cartorio.py:1122`; `app/ponto/services/cartorio.py:1142`; `app/escala/models.py:1736`).
- **Célula acusadora → ausência de `celula_absolveu`:** o consumidor que grava esse carimbo exclui `VEREDITOS_ACUSAM` (`app/chamados/reconciliador.py:598`).

O pós-estado simulado do destino não rompe essa dependência: o ramo da célula acusadora vem antes do fallback de materialização que considera destino encerrado (`app/chamados/juizes.py:1056`; `app/chamados/juizes.py:1083`; `app/chamados/juizes.py:700`).

Na sonda, a tentativa vê pergunta #1 `saida_sem_entrada`, célula `furo`, e recusa; depois ficam chamado aberto, célula `cobrado` e pergunta viva (`anexo debug_cura.log:4`; `anexo debug_cura.log:5`; `anexo debug_cura.log:7`).

**(4) Os três testes** [nao verificado]

**PORTA-2ATO / MORDE.** O chamado identificado pelo teste é `ch`, módulo `batida_ausente`, subcategoria `marco_furado`, dia `self.seg`; a exigência é sair de `VIVOS` depois da correção (`app/colaboradores/tests/test_contract_porta_2ato.py:150`; `app/colaboradores/tests/test_contract_porta_2ato.py:158`).

A regeneração altera `trabalha` e DNA sem limpar o veredito. O cartório tenta reconciliar cobranças do dia sem trabalho antes de lavrar o novo veredito. Assim, **uma pergunta ancorada que ainda leia célula acusadora pode barrar essa tentativa**, embora o previsto já tenha virado folga (`app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:264`; `app/ponto/services/cartorio.py:1188`; `app/ponto/services/cartorio.py:1196`; `app/ponto/services/cartorio.py:1409`; `app/chamados/juizes.py:1056`; `app/chamados/models.py:601`).

A fixture não cria explicitamente a pergunta retentora; existe geração automática pelo save do chamado e pela fábrica. **Qual pergunta efetivamente o segura — PK, motivo e ramo C6 observado — é [nao verificado] nos anexos fornecidos** (`app/colaboradores/tests/test_contract_porta_2ato.py:147`; `app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:770`; `app/chamados/services/fabrica_da_celula.py:124`).

**T34 / furo sob LIMBO.** Quem deixa de fechar é `self.furo`, `batida_ausente`. A pergunta explícita é `p`, motivo `orfao_14h`, tipo `hora`, destino `self.furo`. O próprio teste exige furo resolvido **e essa pergunta viva, sem materialização** (`app/chamados/tests/test_t34_furo_sob_limbo.py:23`; `app/chamados/tests/test_t34_furo_sob_limbo.py:42`; `app/chamados/tests/test_t34_furo_sob_limbo.py:47`; `app/chamados/tests/test_t34_furo_sob_limbo.py:52`; `app/chamados/tests/test_t34_furo_sob_limbo.py:53`).

`agrupar_furo` grava `agrupado_em` antes de reconciliar. A premissa aceita agrupamento sob pai vivo, mas F1 pode recusar pela pergunta antes de consultar essa exceção. O pai LIMBO não integra os módulos de cobrança pesquisados para reata (`app/chamados/services/limbo_folgas.py:219`; `app/chamados/services/limbo_folgas.py:221`; `app/chamados/models.py:644`; `app/chamados/models.py:601`; `app/chamados/models.py:331`; `app/chamados/catalogo/modulos.py:235`).

Sem absolvição anterior, pergunta sem materialização permanece viva; célula acusadora também retorna viva. **Qual desses ramos foi percorrido nessa execução é [nao verificado] nos anexos** (`app/chamados/juizes.py:1056`; `app/chamados/juizes.py:1076`).

**O199 / quatro irmãs.** O chamado é `ch`, módulo `turno_aberto`. As quatro perguntas são `saida_sem_entrada`, respondidas `FOLGA_CONTESTA`, destinadas a `ch`, inicialmente sem validação/materialização (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:106`; `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:121`; `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:175`).

O primeiro aceite valida somente a pergunta atual antes de declarar folga. O fechamento do fio carimba as irmãs por `fechar_por_admin`, mas essa porta não preenche `validada_em`. As irmãs respondidas e ainda não validadas continuam vivas: o ramo da resposta precede a morte por identidade `admin_decidiu`; `FOLGA_CONTESTA` não oferece hora comparável com a luz (`app/chamados/services/validacao.py:283`; `app/ponto/services/veredito_celula.py:174`; `app/chamados/models.py:2399`; `app/chamados/juizes.py:1020`; `app/chamados/juizes.py:1032`; `app/chamados/juizes.py:1044`; `app/chamados/juizes.py:995`).

São essas irmãs que retêm a tentativa de `reconciliar` do fechamento do fio. Os aceites seguintes encontram folga pronta, validam a pergunta e retornam sem nova tentativa de reconciliar o chamado; por isso a recusa anterior pode permanecer após os quatro aceites (`app/ponto/services/veredito_celula.py:183`; `app/chamados/models.py:601`; `app/chamados/services/validacao.py:261`; `app/chamados/services/validacao.py:270`).

**(5) O significado de `encerradas=2`**

`encerradas` conta **perguntas carimbadas por `encerrar_fantasma()`**, uma por retorno verdadeiro da porta. O incremento ocorre antes das guardas de fechamento da disputa; não conta chamados nem disputas encerrados (`app/chamados/reconciliador.py:1095`; `app/chamados/reconciliador.py:1104`; `app/chamados/reconciliador.py:1106`; `app/chamados/models.py:2438`).

O universo abrange o colaborador inteiro: perguntas sem materialização/validação, com dia, em disputa aberta e com os dois motivos previstos. **O SQL não exige `chamado_destino=NULL`**, apesar do nome `_orfas_abertas` (`app/chamados/reconciliador.py:991`; `app/chamados/reconciliador.py:999`; `app/chamados/reconciliador.py:1082`).

Para contar, a pergunta precisa ter resposta vazia, luz do próprio marco acesa, C6 vivo e dia passado encerrado. Portanto, `self.respondida`, com resposta `06:55`, não é a segunda contada (`app/chamados/reconciliador.py:1033`; `app/chamados/reconciliador.py:1035`; `app/chamados/reconciliador.py:1037`; `app/chamados/reconciliador.py:1041`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:93`).

A mudança relevante é a retenção anterior pelo novo gate: a base consultava motor e premissa; a cópia recusa primeiro por pergunta viva sem reata. A fixture cria batidas antes das perguntas explícitas, e essas batidas podem gerar perguntas automaticamente pelo cartório/fábrica (`B/app/chamados/models.py:467`; `B/app/chamados/models.py:470`; `app/chamados/models.py:601`; `app/chamados/models.py:604`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:70`; `app/ponto/signals.py:135`; `app/ponto/services/cartorio.py:770`).

A explicação estática para o aumento é a conservação de uma candidata automática que antes podia ser encerrada/carimbada durante o setup. **A identidade exata da segunda pergunta, e a confirmação dessa trajetória individual, são [nao verificado] pelo dado fornecido.**

## 30/10 19:53 · sessao ade1699d4ba3 · - · - · origem=Ronald (tmux)

Q4 — referências da cópia:

- **PORTA-2ATO / MORDE:** o chamado é `ch`, `modulo_origem='batida_ausente'`, `subcategoria='marco_furado'`, com `data_turno=self.seg`; o teste exige que saia de `VIVOS` após a correção (`app/colaboradores/tests/test_contract_porta_2ato.py:147-160`). A fixture não cria pergunta explicitamente; seu nascimento pode ocorrer no `post_save` do chamado → `julgar_celula` → fábrica (`app/chamados/signals.py:164-180`; `app/ponto/services/cartorio.py:768-770`; `app/chamados/services/fabrica_da_celula.py:112-124`). O motivo/PK exato que o segura não consta nos anexos: **[nao verificado]**.
  
  A dependência estática: a regeneração troca `trabalha`/DNA sem limpar o veredito (`app/ponto/portas/celula.py:259-265`); o cartório tenta reconciliar a cobrança em dia sem trabalho antes de lavrar o novo veredito (`app/ponto/services/cartorio.py:1188-1199`, `1406-1425`). O C6 lê o veredito persistido da célula e retorna viva para um veredito acusador (`app/chamados/juizes.py:1053-1057`). Portanto, **se a pergunta ancorada ainda vê `furo`/`cobrado`, ela bloqueia essa tentativa**, mesmo após o DNA virar folga (`app/chamados/models.py:599-608`). Marcos início/E e fim/S correspondem a `saida_sem_entrada` e `orfao_14h`, respectivamente (`app/chamados/services/declaracao_texto.py:43-44`; `app/chamados/services/fabrica_da_celula.py:58-64`).

- **T3.4 / furo sob LIMBO:** quem deixa de fechar é `self.furo`, módulo `batida_ausente`, aberto, dia `self.d`; o pai é `self.pai`, `sem_previsao_escala/folga_nao_definida` (`app/chamados/tests/test_t34_furo_sob_limbo.py:21-25`). A pergunta explícita é `p`, motivo `orfao_14h`, destino `self.furo`, tipo `hora`, aberta sem resposta (`app/chamados/tests/test_t34_furo_sob_limbo.py:41-42`). O próprio contrato exige **furo resolvido com essa pergunta ainda viva e sem materialização** (`app/chamados/tests/test_t34_furo_sob_limbo.py:47-53`).
  
  `agrupar_furo` grava `agrupado_em` e depois chama `reconciliar` (`app/chamados/services/limbo_folgas.py:214-222`). A premissa antiga permite fechar agrupado sob pai vivo (`app/chamados/models.py:644`), mas F1 consulta as perguntas antes e pode recusar antes de chegar à premissa (`app/chamados/models.py:599-608`). A pergunta sem materialização continua viva na ausência de absolvição anterior; célula acusadora também a mantém viva (`app/chamados/juizes.py:1054-1057`, `1076-1077`). O pai LIMBO não serve de sobrevivente C8 nessa composição: F1 busca apenas `CALADOS_POR_ISENCAO`, que não inclui `sem_previsao_escala` (`app/chamados/models.py:327-334`; `app/chamados/catalogo/modulos.py:235-241`; `app/chamados/juizes.py:1620-1626`). O ramo C6 efetivamente percorrido nessa execução não está nos anexos: **[nao verificado]**.

- **O199 / quatro irmãs:** quem fica retido é `ch`, `turno_aberto/saida_sem_entrada`, `aberto`, dia informado no contexto (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:106-118`). São quatro perguntas de `hora`, motivo `saida_sem_entrada`, todas respondidas `FOLGA_CONTESTA`, apontando para `ch`, inicialmente sem validação/materialização (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:120-126`, `175-178`).
  
  O primeiro aceite valida somente a pergunta atual antes de declarar folga (`app/chamados/services/validacao.py:282-285`). O fechamento do fio carimba as irmãs por `fechar_por_admin`, que escreve `via_resolucao`/`materializada_em`, sem validar (`app/ponto/services/veredito_celula.py:174-178`; `app/chamados/models.py:2399-2402`). **As irmãs respondidas e ainda não validadas seguem vivas:** no C6, resposta não vazia vem antes da morte por identidade `admin_decidiu` ou por célula absolvida (`app/chamados/juizes.py:1017-1045`, `1053-1065`); `FOLGA_CONTESTA` não fornece HH:MM comparável, logo não satisfaz a concordância com a luz (`app/chamados/juizes.py:983-997`). Elas seguram o `reconciliar` executado pelo fechamento do fio (`app/ponto/services/veredito_celula.py:182-183`; `app/chamados/models.py:599-608`).
  
  Os próximos aceites encontram a folga pronta, validam/fecham cada pergunta e retornam sem tentar reconciliar novamente o chamado (`app/chamados/services/validacao.py:261-272`). Assim, todas podem terminar validadas enquanto `ch` conserva a recusa anterior; o teste exige `ch.status_local == 'resolvido'` ao final (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:177-184`). O PK da primeira irmã validada e das retentoras em runtime não consta nos anexos: **[nao verificado]**.

## 30/10 19:53 · sessao 956521c1388b · - · - · origem=Ronald (tmux)

Q1/Q2, somente leitura:

- O cartório escolhe `cobrado` quando `cods` está vazio, há chamados em `VIVOS` e pelo menos um desses chamados tem fio vivo; o fio usa `pergunta_viva`. (`app/ponto/services/cartorio.py:1069`, `:1074`, `:1122`, `:1141`, `:1142`; `app/chamados/juizes.py:1111`).
- O valor chega à porta por `cel.lavrar_veredito(... save=False)`; a porta respeita decisão de admin, atribui `cel.veredito = v`, e `carimbar` persiste junto dos demais campos. (`app/ponto/services/cartorio.py:1409`, `:1424`; `app/ponto/portas/celula.py:672`, `:692`, `:728`).
- `cobrado` pertence a `VEREDITOS_ACUSAM`; C6 retorna vivo por esse pertencimento. A exceção anterior de marco aceso exige pergunta já materializada. (`app/escala/models.py:1735`; `app/chamados/juizes.py:1050`, `:1056`).

Base disponível em `/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e`; a cadeia abaixo usa os arquivos dessa base:

1. `registrar_batida` cria a batida → receiver do cartório → `_julgar_dias` → `julgar_celula` → `julgar_colab` → `_julgar_colab_corpo`. (`app/ponto/registro_batida.py:157`; `app/ponto/signals.py:132`, `:135`, `:93`; `app/ponto/services/cartorio.py:1560`, `:744`).
2. O corpo monta a ata nova, publica-a no contexto do julgamento e, com os marcos cobrados acesos, chama `x.reconciliar()`. (`app/ponto/services/cartorio.py:1148`, `:1152`, `:1163`).
3. Na base, `reconciliar` pede a decisão; o gate da premissa aceita `premissa_regularizada`; `_gravar_estado` atribui o terminal e salva. (`base/app/chamados/models.py:440`, `:470`, `:495`, `:444`, `:192`, `:231`).
4. **Após esse save e antes da segunda lavra, o chamado já está terminal e a pergunta sem carimbo continua viva pelo veredito anterior acusador.** C6 não possui veto geral por destino terminal antes desse ramo; o receiver do chamado evita julgamento recursivo enquanto o cartório está ativo. (`base/app/chamados/models.py:231`; `app/chamados/juizes.py:1050`, `:1054`, `:1056`; `app/chamados/signals.py:174`).
5. O fechamento enfileira a célula para nova passada; a segunda passada relê os chamados, agora sem aquele vivo. Sem códigos e sem cobrança, `derivar_veredito([])` produz `concorde`. (`app/ponto/services/cartorio.py:1166`, `:745`, `:746`, `:1069`, `:1409`; `app/escala/models.py:1596`).
6. O receiver posterior da batida chama `reconciliar_chamados_colab`, que começa por `retratar_perguntas_absolvidas_por_celula`; esse consumidor exclui células acusadoras, confirma C6 falso e grava `materializada_em`, usuário e `celula_absolveu`. (`app/ponto/signals.py:207`, `:215`; `app/chamados/reconciliador.py:1350`, `:598`, `:613`, `:617`, `:619`, `:620`).

Leitores diretos de `cobrado` / `VEREDITOS_ACUSAM`, fora de testes e migrations: [nao verificado]

- C6 e consultas de perguntas: `app/chamados/juizes.py:1002`, `:1056`; `app/chamados/models.py:2523`, `:2524`.
- Fechamento, emissão e fio: `app/chamados/reconciliador.py:598`, `:978`; `app/chamados/services/fabrica_da_celula.py:86`; `app/chamados/services/silencio_ausencia.py:41`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/limbo_folgas.py:194`; `app/chamados/services/disputa_emissao.py:705`.
- Folha e apresentação: `app/folha/export.py:1215`; `app/relatorios/dia_da_frota.py:13`, `:55`; `app/relatorios/furos.py:25`, `:46`; `app/colaboradores/services/calendario.py:70`, `:173`.
- Vigias/comandos: `app/chamados/services/furo_sem_canal.py:35`, `:72`; `app/chamados/services/vigia_fio.py:182`, `:230`; `app/folha/services/vigia_pausa.py:155`, `:157`; `app/ponto/services/vigia_ausencia_veredito.py:41`, `:42`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `:62`; `app/chamados/management/commands/disparar_perguntas_competencia.py:86`, `:120`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:133`; `app/chamados/management/commands/auditar_invariantes_chamados.py:197`, `:342`, `:1387`, `:1601`, `:1643`.

A chave de contador `cobrado` em `app/ponto/services/furos_sem_cobranca.py:131` não é leitura de `CelulaDia.veredito`.

_755 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
