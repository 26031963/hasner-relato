# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 10:42** · publicado em **06/11/2026 10:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 10:42 · sessao 481eb41296f1 · - · - · origem=Ronald (tmux)

Recomendo manter `F.perguntar_pela_celula` como autoridade da sonda e declarar que a medição descreve a recusa atual da fábrica sobre a célula lavrada; o ingresso histórico pelo cartório continua [nao verificado]. A fábrica efetivamente recebe a célula em `julgar_colab`; o comando é outra camada e não é obrigatório para chamar essa autoridade real. (`app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:145`)

`julgar_colab(apply_=False, forcar=True)` não cobre esse ingresso: a fábrica está dentro de `if apply_`, e o ramo `nunca_bateu` executa classificação/lavra somente dentro de outro `if apply_`. Portanto, adicioná-lo não elimina a limitação e cria uma segunda medição incompleta. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:1207`, `app/ponto/services/cartorio.py:1226`)

O emissor separado de chamado é `detectar_ausencias.Command._abrir_chamado`; seus parâmetros reais vêm de dois ramos específicos do cartório. Chamá-lo com parâmetros montados à mão mudaria a pergunta da sonda. Não encontrei modo somente leitura que atravesse integralmente esse emissor e depois a fábrica. (`app/ponto/management/commands/detectar_ausencias.py:570`, `app/ponto/services/cartorio.py:1717`, `app/ponto/services/cartorio.py:1740`, `app/ponto/services/cartorio.py:1768`, `app/ponto/services/cartorio.py:1778`)

Se o contrato for interpretado como exigência de medir também o chamado, seria necessário executar `apply_=True` com interceptação de todas as portas reais de escrita e lidar com as releituras posteriores; interromper na primeira escrita não chega à fábrica e não satisfaz essa interpretação. (`app/ponto/services/cartorio.py:1802`, `app/ponto/services/cartorio.py:997`, `app/ponto/services/cartorio.py:1038`)

## 06/11 10:41 · sessao 829ee648074e · - · - · origem=Ronald (tmux)

- A porta histórica é `diff_calculador.Command.handle`, com `--pares-da-autoridade`; ela chama `insumos_do_calculador` e `rubricas_por_dia` reais. (`app/ponto/management/commands/diff_calculador.py:287`, `:309`, `:312`, `:417`)
- Neste hash o instrumento ainda omite `minutos_extra_100_feriado` e soma indiscriminadamente `periodos_ft` como folga. Não serve para atribuir essas duas diferenças ao v2 sem confrontar o emissor real. (`app/ponto/management/commands/diff_calculador.py:253`, `:263`, `:268`; diagnóstico histórico: `app/docs/RELATO.md:560`)
- É possível observar o comando sem reconstruir cálculos: trace restrito ao `Command.handle.__code__`; na entrada do ramo divergente, linha 539, recolher `c.pk`, `k`, `r`, `a`, `b`; no retorno recolher `S`, `div`, `colabs_div`, `T`, `linhas`. Isso mantém valores sem arredondamento e respeita o limiar escolhido pelo comando. (`app/ponto/management/commands/diff_calculador.py:535`, `:538`, `:539`, `:542`)
- Soma dos deltas divergentes não equivale necessariamente ao delta de `S`: `S` recebe também diferenças inferiores ou iguais a 0,02 h. Para reconciliação completa, conservar uma parcela explícita residual/sub-limiar. (`app/ponto/management/commands/diff_calculador.py:535`, `:536`, `:537`, `:538`)
- Os erros capturados silenciosamente pelo comando podem ser registrados por eventos `exception` no frame do comando e nas funções locais do mesmo arquivo, sem alterar suas decisões. Sítios de captura: `app/ponto/management/commands/diff_calculador.py:148`, `:226`, `:314`, `:350`, `:368`, `:405`.
- O emissor real `dia_pago.lavrar` distribui HE100 de feriado e as parcelas de folga; o fechamento chama esse mesmo emissor nas versões `motor` e `oraculo`. (`app/ponto/services/dia_pago.py:135`, `:183`, `:223`, `:250`; `app/ponto/services/fechamento.py:1185`, `:1261`)
- Escritas exatas do emissor: `DiaPago.objects.filter(...).delete()` e `DiaPago.objects.bulk_create(novas)`. Dentro de um wrapper que chama exclusivamente o `lavrar` original, os patches podem limitar-se ao manager usado por essas duas operações: `filter` devolve objeto com `delete` simulado; `bulk_create` recolhe as instâncias em memória. O retorno original continua sendo `len(novas)`. (`app/ponto/services/dia_pago.py:163`, `:357`, `:358`, `:359`)
- Para alcançar esse emissor por `--periodos-do-fechamento`, o wrapper de `fechamento.recalcular_fechamento_mes` precisa restaurar temporariamente o emissor real instrumentado: o comando substitui `dia_pago.lavrar` por `_nada` antes de chamar o fechamento. (`app/ponto/management/commands/diff_calculador.py:123`, `:137`, `:140`, `:142`)
- Os totais v2 podem vir do retorno real de `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)`, que contém o próprio objeto destacado do fechamento. (`app/ponto/services/fechamento.py:439`, `:1112`, `:1119`, `:1293`)
- O fechamento também engole falhas do emissor e por colaborador; é necessário observá-las para cumprir `INCOMPLETO`. Seu log por colaborador contém nome, portanto não deve ir diretamente ao stdout. (`app/ponto/services/fechamento.py:1222`, `:1281`, `:1287`, `:1290`)
- Tempo de execução e universo atual não podem ser certificados sem execução/dados adicionais. [nao verificado]

## 06/11 10:41 · sessao 481eb41296f1 · - · - · origem=Ronald (tmux)

Montagem e reconciliação:

- Use exatamente `V.censo_alcance(limite=10**7)`; filtre `x[4] == 'sem_porta' and x[5] == 'd'`; agrupe por `x[1]`; monte `InsumosDoDono(c, min(datas), max(datas), hoje, agora=agora)` e retenha somente `ins.dono(data) == 'ESTRUTURA'`. Esta é a montagem do anexo; a autoridade do primeiro total é `r['alcance_d']`, e a do subconjunto é o retorno de `ins.dono`. (`app/chamados/services/vigia_fio.py:542`, `app/ponto/management/commands/e6_oraculo.py:128`, `app/ponto/management/commands/e6_oraculo.py:172`)
- A lista contém `[celula_id, colab_id, data, competencia, rota, causa, detalhe, papel]`; carregue a célula por `x[0]`, conferindo colaborador/data. Há unicidade colaborador/data na montagem do censo. (`app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:593`, `app/chamados/services/vigia_fio.py:629`)
- `200` e `182/13/5` devem ser expectativas históricas, nunca totais impostos ao processamento. Valores atuais na sombra: [nao verificado].

Caminho verdadeiro do evento:

`_cartorio_post_save_batida` → `_julgar_dias` → `cartorio.julgar_celula` → `cartorio.julgar_colab` → `fabrica_da_celula.perguntar_pela_celula`. (`app/ponto/signals.py:131`, `app/ponto/signals.py:85`, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:1960`, `app/ponto/services/cartorio.py:1989`, `app/ponto/services/cartorio.py:997`)

O comando `processar_cartorio` usa o mesmo `julgar_colab`, com `detectar_ausencias.Command` como emissor do chamado do dia. (`app/ponto/management/commands/processar_cartorio.py:36`, `app/ponto/management/commands/processar_cartorio.py:42`, `app/ponto/management/commands/processar_cartorio.py:65`)

Não confundir isso com `post_save` de `CelulaDia`: a porta documenta expressamente que o gatilho é o ato de corrigir cadastro, e recusa adicionar sinal à entidade. (`app/ponto/portas/celula.py:362`)

Instrumentação sem copiar predicados:

- `patch(..., wraps=real)` nas funções `dia_sem_trabalho`, `vencidos_no_instante`, `parcial_so_lista`, `_ata_do_dia`, `motivos_pela_ata`, `limbo_vai_a_pauta`.
- Para os retornos inline, `sys.settrace` limitado aos `__code__` reais de `motivos_a_perguntar`, `motivos_pela_ata`, `perguntar_pela_celula`; registrar `frame.f_lineno` no evento `return`. Assim a classificação decorre da linha executada, sem reproduzir `situacao`, `data`, `veredito` ou os filtros.
- Guardas de `motivos_a_perguntar`, na ordem: dia sem trabalho/inativo (`91–92`); futuro (`93–94`); vencimento calculado (`98`); veredito absolvente (`99–100`); parcial só lista (`102–104`); deduplicação após motivos da ata (`118–126`). (`app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:93`, `app/chamados/services/fabrica_da_celula.py:98`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:126`)
- Guardas internas dos motivos: vencidos vazio (`45–46`), ata não autoriza (`50–51`), faltantes `None` com vencidos não `None` (`52–54`), classificador sem motivo (`56–57`), exclusão de marcos não vencidos (`60–61`), ausência de correspondência de motivo (`62–64`). (`app/chamados/services/fabrica_da_celula.py:45`, `app/chamados/services/fabrica_da_celula.py:50`, `app/chamados/services/fabrica_da_celula.py:52`, `app/chamados/services/fabrica_da_celula.py:56`, `app/chamados/services/fabrica_da_celula.py:60`, `app/chamados/services/fabrica_da_celula.py:62`)
- `parcial_so_lista` deve ter a própria linha de retorno registrada: `399` distingue parcial sem período; `400` distingue a regra de julgamento anterior/ausência de fato novo. A instrumentação não precisa copiar esses testes. (`app/ponto/services/cartorio.py:398`, `app/ponto/services/cartorio.py:400`)
- Depois dos motivos, limbo entregue segue para Pauta DP e retorna `[]`; isso é destino alternativo, não ausência de emissão. (`app/chamados/services/fabrica_da_celula.py:171`, `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:173`)

Armadilhas:

- `_ata_do_dia` captura `Exception` e devolve `(True, None)`. O contrato exige observar essa captura; envolver `core.observ.registrar_engolido` para contar a exceção recebida preservando/repropagando conforme desenho da sonda. Contar só erros que escapam do emissor perderia estes casos. (`app/chamados/services/disputa_emissao.py:828`, `app/chamados/services/disputa_emissao.py:830`, `app/chamados/services/disputa_emissao.py:832`)
- Chamar diretamente `perguntar_pela_celula` prova a resposta atual do emissor para o estado lido. Não demonstra que o evento histórico efetivamente o chamou: `julgar_colab` só chega ali com `apply_` e célula presente no `cods` do ato. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:986`, `app/ponto/services/cartorio.py:995`)
- Para não vazar conteúdo, represente o último frame como `arquivo:linha:funcao`, com até 200 caracteres; não imprima `str(exc)`, SQL nem linha de código contendo dados. A existência de dados pessoais em mensagens concretas permanece [nao verificado].
- Tempo de execução na sombra: [nao verificado]. O custo amplo é do censo e do motor solicitado uma vez por colaborador pelo carregador; as 200 chamadas diretas da fábrica evitam rejulgar a frota. (`app/chamados/services/vigia_fio.py:582`, `app/ponto/management/commands/e6_oraculo.py:154`, `app/ponto/management/commands/e6_oraculo.py:159`)

## 06/11 10:41 · sessao faa68ab99b0c · - · - · origem=Ronald (tmux)

R1 chama apenas o juiz do dono; cartório e emissor são caminhos separados:

- `app/ponto/management/commands/e6_oraculo.py:304`: chama `espelho_do_colab`; `:316` cria `InsumosDoDono`; `:366` chama seu `dono`; `:183` delega a `dono_da_divergencia`.
- `app/ponto/services/espelho.py:1227`: `autoridade_do_periodo`; `:1262` chama `montar_dias`; `:1284` chama `folha_manda`. Não identifiquei escrita nesse caminho. [não verificado quanto à totalidade transitiva dos métodos de modelos]
- `app/ponto/services/cartorio.py:895`: `julgar_colab` é outra entrada. Seu corpo chama `classificar_dia` em `:1413`; emissor `_abrir_chamado` é chamado somente com `apply_=True`, em `:1725`–`:1740` e `:1777`–`:1778`. Portanto `apply_=False` não mede o emissor.
- `app/ponto/management/commands/detectar_ausencias.py:693`: emissor chama a porta `ChamadoColaborador.abrir`; depois pode escrever marcos (`:744`), contexto/nota (`:765`, `:768`), enviar push (`:778`) e criar fio (`:801`). Não basta interceptar só a criação para fazer uma sondagem desse caminho.

O problema mais importante do contrato é erro engolido:

- `app/ponto/management/commands/e6_oraculo.py:305`: exceção do espelho vira apenas contador e `continue`.
- `app/ponto/services/espelho.py:411`: falha lendo DiaPago vira `{}`.
- `app/ponto/services/espelho.py:569`: falha da grade vira grade vazia.
- `app/ponto/services/espelho.py:650`: falha de fatos/palavra vira ausência da palavra.
- `app/ponto/motor_calculo_v2.py:1387`: falha de carregar empresa vira `None`; `:1419`: falha das decisões HE vira `{}`.
- `app/escala/services/leitor_celula.py:27`: erro de conversão vira `None`.

Capturar somente exceções que escapam do `call_command` ou somente registros de logging não atende “erro nunca engolido”, porque os dois últimos fallbacks do motor não registram logger. A alternativa instrumental sem reproduzir lógica é ler os módulos com `ast`, mapear a primeira linha executável de cada `ExceptHandler` e usar `sys.settrace` nesses frames; quando essa linha executar, registrar `sys.exc_info()` com contexto col/data da pilha. Deduplicar a mesma instância de exceção preservando referências evita contá-la novamente quando for tratada em mais de um nível. A execução usa Python 3.13 segundo `app/Dockerfile:1`.

## 06/11 10:40 · sessao 79d40633b87e · - · - · origem=Ronald (tmux)

O “resto dos 38” tem três blocos no registro: cenários B17–B33; fechamento/export para a propriedade “todo leitor lê o mesmo número”; e dimensionamentos de intervalo/tolerância. (`app/docs/BACKLOG.md:1621`)

**Oito cenários sem execução naquela costura, segundo o registro atual da bateria complementar:**

| Caso | O que falta / destino declarado | Evidência |
|---|---|---|
| B18 | Batida 00:00 UTC−3: pergunta dos leitores e do dia da jornada; `onde='fora'`. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:69` |
| B20 | Segundo vínculo vigente sobreposto ao primeiro; `onde='cadastro'`. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:76` |
| B24 | Só duas batidas da pausa: falta exercício pelos leitores/gravação; `onde='fora'`. O corte posterior manda EM ABERTO, soma dos pares fechados. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:100`; `app/docs/BACKLOG.md:1521` |
| B25 | Colaborador sem vínculo: precisa exercitar a porta de fechamento, pois a costura exige `ec`. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:104` |
| B29 | Retratação pertence ao universo apurável antes do motor; aponta para tripwire próprio, não para caso de período. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:122` |
| B30 | Registro ainda pede vínculo com âncora futura; corte posterior descreve extrapolação máxima cadastrada por TipoEscala, inicialmente 180 minutos. Essa diferença impede declarar o caso atualizado apenas pelo rótulo. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:126`; `app/docs/BACKLOG.md:1521` |
| B31 | Falta exercício da porta de cadastro e lista dos existentes; registro ainda usa pergunta antiga sobre início=fim. Corte posterior manda recusar na porta e listar existentes. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:129`; `app/docs/BACKLOG.md:1521` |
| B33 | Atestado aprovado em folga com par fechado; fixture de cadastro ausente e metade da lei declarada sem dono. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:136` |

O selo de cobertura só exige caso executável para entradas `onde='roda'`; para `fora`/`cadastro` exige explicação textual. Portanto os oito itens acima não são oito testes vermelhos nem oito bugs da v3. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:503`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:514`)

**Casos já presentes que não devem ser apresentados como ainda ausentes:**

- B17: virada da competência, teste e asserções 720 na competência inicial e zero na seguinte. O `660+0` do backlog está desatualizado frente à correção explicitada pela bateria. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:252`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:266`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:44`)
- B19, B22, B23 e B32: casos tabelados com respostas numéricas. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:204`; `:213`; `:220`; `:226`)
- B21: teste presente, mas com redação anterior “abono se cancela”; corte posterior diz batidas contam e abono cobre apenas previsto descoberto. Não certificar aderência ao corte novo sem conferir esse comportamento. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:276`; `app/docs/BACKLOG.md:1521`)
- B26 e B28: testes presentes. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:319`; `:478`)
- B27: presente; B27b e B27c são pinos explícitos do defeito v2 ligado à ordem/deduplicação. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:340`; `:371`; `:423`)

**Demais pendências:**

- Propriedade de quatro leitores: o backlog declara só 2/4 pontas e deixa fechamento/export de fixture pendentes. O teste de export explica por que compara a função isolada: montar entrada no TXT exigia catálogo, fechamento e célula julgada em todo dia, e as tentativas caíram em `cadastro_zero`/`furo_espelho`. Isso demonstra o obstáculo, não a cobertura integral dos quatro leitores. (`app/docs/BACKLOG.md:1522`; `app/docs/BACKLOG.md:1621`; `app/folha/tests/test_porta_do_export.py:216`; `:224`)
- Art.71 §4 pós-2017: dimensionamento suprimido versus “INTEGRAL” continua pergunta nomeada; comentário v2 contém “indeniza INTEGRAL”. (`app/docs/BACKLOG.md:1621`; `app/ponto/motor_calculo_v2.py:68`)
- Art.58 §1: dimensionamento 5 por marcação versus 10 por ponta; C13 declara explicitamente a contradição, e F9/F9b são indicados como exercícios da lei. (`app/docs/BACKLOG.md:1621`; `app/ponto/tests/test_o280_bateria_da_lei.py:605`; `:611`)
- Degrau HE >2h→100% continua explicitamente aberto no backlog; os casos só afirmam total e conservação das parcelas, sem certificar 120/restante como lei. (`app/docs/BACKLOG.md:1621`; `app/ponto/tests/test_o280_bateria_da_lei.py:1954`; `:5742`)

Há divergência documental de contagem: o backlog diz “15 células B17–B33” e chama B33 verde; a tabela atual contém **17 IDs**, o selo exige 17 e B33 está `cadastro`. (`app/docs/BACKLOG.md:1621`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:64`; `:136`; `:527`)

Nenhum resultado de execução desta bateria foi produzido nesta auditoria; estado verde no ambiente atual: [nao verificado]. [nao verificado]

## 06/11 10:40 · sessao f6ffc8ffebe3 · - · - · origem=Ronald (tmux)

Sítios de escrita reais, nesta cópia:

| Ramo | Sítio |
|---|---|
| Disputa existente: agrega contexto/motivos antes de gerar perguntas | `app/chamados/services/disputa_emissao.py:501` — `disputa_aberta.save(...)` |
| Disputa nova: abre/reutiliza pai | `app/chamados/services/disputa_emissao.py:504` — `ChamadoColaborador.abrir(...)` |
| Pai novo | `app/chamados/models.py:932` — `cls.objects.get_or_create(...)` |
| Disputa nova | `app/chamados/services/disputa_emissao.py:523` — `DisputaSupervisao.objects.create(...)` |
| Pergunta existente reapontada | `app/chamados/models.py:2474` — `ex.save(...)` |
| Pergunta nova | `app/chamados/models.py:2476` — `cls.objects.create(...)` |
| Descarimbo anterior à emissão | `app/chamados/services/relavra_via.py:231` → `app/chamados/models.py:2652` — `PerguntaDisputa.save(...)` |
| Pauta do limbo exportado | `app/pautas/services.py:180` — `Pauta.objects.create(...)` |
| Usuário sistema ausente, no ramo de pauta | `app/core/utils.py:18` — `User.objects.get_or_create(...)` |

Atenções para a sonda:

- Patch apenas em `ChamadoColaborador.objects.create` **não intercepta necessariamente** o `get_or_create`: este usa o `QuerySet.create`; patch de `ChamadoColaborador.save` cobre o INSERT real. (`app/chamados/models.py:932`; funcionamento interno do Django: [nao verificado])
- A disputa existente sempre recebe `merge_contexto=contexto or {}` neste caminho, salva o contexto e só depois chama a geração da pergunta. Interromper nesse `save` prova entrada no escritor, não que a pergunta passaria pelas guardas posteriores. (`app/chamados/services/disputa_emissao.py:479`, `:501`, `:603`, `:618`)
- Uma sentinela derivada diretamente de `BaseException` evita que os `except Exception` internos a engulam. Há captura desse tipo na emissão/reatação e na leitura da ata. (`app/chamados/services/disputa_emissao.py:411`, `:828`)
- Se houver candidata de descarimbo, o emissor pode tentar essa escrita **antes** da emissão. Não deve contabilizar o descarimbo como pergunta/chamado emitível. (`app/chamados/services/fabrica_da_celula.py:164`, `:166`, `:174`)
- `SELECT FOR UPDATE` ocorre antes das escritas na disputa e no descarimbo. Permissão do papel LEITOR para executá-lo: [nao verificado]. (`app/chamados/services/disputa_emissao.py:467`; `app/chamados/services/relavra_via.py:221`)
- O ramo real da pauta chama `autor_sistema` antes do `Pauta.objects.create`; logo também existe possível escrita na obtenção do usuário sistema. (`app/chamados/services/fabrica_da_celula.py:236`; `app/pautas/services.py:87`; `app/core/utils.py:18`)

Há outro emissor do chamado no cartório: `julgar_celula` instancia `detectar_ausencias.Command` e o corpo chama seu `_abrir_chamado`. A fábrica da pergunta roda depois da lavra, apenas nas células julgadas naquele ato. (`app/ponto/services/cartorio.py:1964`, `:1990`, `:1740`, `:1778`, `:981`, `:997`)

## 06/11 10:40 · sessao 963cba183f13 · - · - · origem=Ronald (tmux)

A contagem declarada é **8 `EM_CURSO` + 1 `ESPERA_RONALD` = 9 itens não `FEITO`**. O placar efetivo rebaixa `FEITO` sem prova, mas os itens `FEITO` lidos têm provas preenchidas; a regra está em `app/core/espelho_verdade.py:199-203`.

| Item | Estado/local | Número do R1 que atualiza; limite |
|---|---|---|
| E1-passivo-53 | `EM_CURSO`, `app/core/espelho_verdade.py:36-47` | Não mede. `escala/services/vigencia_impossivel.py:23-24` oferece `medir()` somente leitura e devolve vínculos/colabs/ativos/lavrados em `:48-53`. |
| E1-sem-previsao | `EM_CURSO`, `app/core/espelho_verdade.py:48-49` | Classe `esp_zero_e6_trabalho` (`app/ponto/management/commands/e6_oraculo.py:358-361`), proxy de zero no espelho com trabalho no oráculo. Não certifica previsão válida; a medição específica é `escala/services/previsto_cego.py:114-115`, que usa `janela_atual` em `:138` e cobra ativo com par e nenhum previsto (`:142-174`). |
| E3-turno-aberto | `EM_CURSO`, `app/core/espelho_verdade.py:81-82` | `dia_batida_impar`, batem/divergem e `impar_FORA_da_faixa` (`e6_oraculo.py:347-349`, `:402-412`). São paridade/faixa do oráculo, não censo exato de turnos abertos. |
| E4-rotulo-do-desenho | `EM_CURSO`, `app/core/espelho_verdade.py:130-131` | R1 não mede; verificar leitor da pílula/selo de UI. **Prova do placar envelhecida:** `app/templates/escala/tipos_lista.html:55` já usa `{{ t\|desenho_do_turno }}` e cita O74 e seu selo. |
| E5-le-da-celula | `EM_CURSO`, `app/core/espelho_verdade.py:138-139` | R1 não mede proveniência do fechamento nem conclusão da troca de leitores; a pendência declarada é “falta status encerrado” (`:139`). |
| E6-sistema-bate | `EM_CURSO`, `app/core/espelho_verdade.py:168-169` | `BATE`/dias comparados (`e6_oraculo.py:398-399`) e ESTRUTURA dias/horas/colabs (`:422-423`). |
| E6-exportavel | `EM_CURSO`, `app/core/espelho_verdade.py:172-173` | `colabs com divergencia` (`e6_oraculo.py:429`) e colabs por dono (`:423`). R1 10/11 não mede elegibilidade real do export nem a 09 citada no título do item. |
| E6-piso-185 | `EM_CURSO`, `app/core/espelho_verdade.py:178-179` | `SOMA` donos/classes (`e6_oraculo.py:424-427`) e ESTRUTURA dias/horas/colabs. **Não fecha causa:** o próprio placar exige “falta causa nomeada nos ... ESTRUTURA” (`espelho_verdade.py:179`). |
| E3-separados-30 | `ESPERA_RONALD`, `app/core/espelho_verdade.py:83-84` | Não mede: é universo separado da 09 e situação de migração/aplicação; o placar declara a 09 sob responsabilidade externa (`:84`). |

O backlog pede explicitamente R1 para **cinco itens**: E1-sem-previsao, E3-turno-aberto, E6-sistema-bate, E6-exportavel e E6-piso-185, e pede confirmar E6-batida-impar; este último já é `FEITO` (`app/docs/BACKLOG.md:1478`; `app/core/espelho_verdade.py:176-177`). A confirmação usa as linhas de ímpares e paridade (`app/ponto/management/commands/e6_oraculo.py:402-412`).

`app/docs/ESTRUTURAL.md:34-42` estabelece o dono único, o juiz `dono_da_divergencia`, o destino CADASTRO/BATIDA no admin e a separação do placar principal R1–R6 e ESPELHO-VERDADE. `app/core/placar_estrutural.py:115-138` declara a fonte `logs/e6_cauda2c/r1_dono_*.txt`, cadência, dias/horas/colabs e tabela impressa pelo próprio comando. `:105-113` exige a soma obtida no comando, sem sonda paralela.

## 06/11 10:40 · sessao 27a8fa81a908 · - · - · origem=Ronald (tmux)

- **V3-MOTOR-FERIADO-HE100-NOTURNA** está na fila e exige `!`; o registro relata HE100 noturna zero em feriado com HE100 e noturnas positivas. Isso é relato do backlog, sem reexecução nesta auditoria. (`app/docs/BACKLOG.md:937`)
  - No `MotorTurnoPartido`, as HE noturnas são zeradas antes do cálculo mensal; o ramo de dobra preenche HE100 e sai por `continue`, enquanto `_aplicar_he_noturna` aparece depois. (`app/ponto/motor_calculo_v2.py:2937`, `:3092`, `:3101`, `:3147`)
  - No `MotorComercial`, ocorre a mesma estrutura: zera os splits, preenche a dobra e sai antes da chamada de HE noturna. (`app/ponto/motor_calculo_v2.py:3323`, `:3337`, `:3352`, `:3399`)
  - Qual desses ramos explica cada um dos dez colaboradores registrados: **[nao verificado]**.

- **V3-NOTURNO-RECORTE-LONGO** espera `!` e está explicitamente fora do escopo V3-P5. (`app/docs/BACKLOG.md:939`)
  - `minutos_noturnos` devolve zero acima de 26 horas. (`app/ponto/motor_calculo_v2.py:93`)
  - Com prorrogação, estende `s1` até `fim` e soma separadamente `s2` de 22–24; portanto pode contar a sobreposição duas vezes. (`app/ponto/motor_calculo_v2.py:110`, `:112`, `:116`)
  - `MotorPlantao.calcular_periodo` delega a `Motor12x36`, que escreve noturnas pela autoridade que chama `minutos_noturnos`. (`app/ponto/motor_calculo_v2.py:3510`, `:2792`, `:983`)

- **Rubricas diagnósticas para a lista:** `horas_extras_100_noturna`, contextualizada por `horas_extras_100`, `horas_extras_100_feriado` e `horas_noturnas`, pode localizar a assinatura conhecida do feriado. (`app/docs/BACKLOG.md:937`; nomes dos campos: `app/ponto/services/dia_pago.py:35`)
  - `horas_noturnas`, `horas_extras_50_noturna` e `horas_extras_100_noturna` podem expor efeitos do recorte longo; HE50/100 também podem mudar quando a régua legal inclui o ganho noturno na base do excedente. (`app/ponto/motor_calculo_v2.py:983`, `:441`, `:3123`)
  - Há **cegueira compartilhada**: a v3 também chama `minutos_noturnos`; igualdade no diff não elimina o defeito comum. (`app/ponto/calculador/regras.py:85`; `app/ponto/calculador/periodo.py:510`)
  - Divergências fora dessas assinaturas são candidatos a investigação, não comprovação de bug novo. L-110 define a comparação com o código velho como **impacto**, nunca validação. (`app/docs/LEIS.md:136`)

## 06/11 10:40 · sessao fb0ce3409b50 · - · - · origem=Ronald (tmux)

Lei geral: `CLAUDE.md:166–170` faz célula/ata a lâmpada, evento o emissor, consumidor leitor. `app/docs/CORTES.md:263` explicita “lampada acesa no marco = fio vivo obrigatorio”; não transforma toda recusa em violação, porque há exceções escritas posteriores.

Guardas de `motivos_a_perguntar`, em ordem:

1. `dia_sem_trabalho(cel)` e colaborador inativo: `app/chamados/services/fabrica_da_celula.py:91`, retorno `:92`. O juiz é `app/chamados/juizes.py:413`, predicado `:426–428`. Dia sem trabalho tem cobertura expressa em `app/docs/CORTES.md:416`: `trabalha=False` ou tipo lavrado diferente de trabalho; `None` cobra. Não viola 4a quando a própria lâmpada diz ausência de trabalho. Inativo tem piso por domínio RH documentado/testado em `app/chamados/tests/test_selo_f3_celula_soberana.py:19–25`, `:86–103`; não encontrei corte nomeado nesta leitura que autorize especificamente **todo** status diferente de `ativo`, então ampliação além de desligado [nao verificado].

2. Dia futuro: fábrica `:93–94`. Teto temporal escrito em `CLAUDE.md:185`; `CORTES.md:263` exclui hoje por teto na formulação histórica. A fábrica atual trabalha por instante e inclui hoje quando marco venceu: docstring fábrica `:8–11` e autoridade `vencidos_no_instante :15–31`. Recusa de marco ainda não vencido é coberta; não há violação demonstrada.

3. Veredito preenchido fora de `VEREDITOS_ACUSAM`: fábrica `:99–100`. Obedece à lâmpada/consumidor em `CLAUDE.md:167–170`; não há obrigação de perguntar célula absolvida.

4. `parcial_so_lista`: fábrica `:101–104`; juiz real `app/ponto/services/cartorio.py:370–400`. Duas hipóteses documentadas `:378–387`: parcial sem período sempre lista do admin; parcial com período sem fato novo desde julgamento também lista. Corte explícito `app/docs/CORTES.md:319`, complemento da resposta `nao_sei :334`; aval literal “parcial sem periodo ... nunca chamado” está no código `cartorio.py:381–382`. Portanto não imputar quebra geral da 4a a esta recusa: é exceção escrita. Causa dos 200 por ela [nao verificado].

5. Construção dos motivos e dedup: fábrica `:118–126`. Dedup elimina todo motivo com pergunta histórica que não seja `VIA_DUPLICATA`, sem testar aqui se viva. Se essa for a causa medida de célula acusadora sem fio vivo, a lei que confronta é `CLAUDE.md:168–170` + `CORTES.md:263`; não há nesta leitura lei que autorize substituir **fio vivo** por mera existência histórica.

Guardas internas de `motivos_pela_ata`:

- Vencidos vazio: fábrica `:45–46`, coberto pelo teto `CLAUDE.md:185`.
- `_ata_do_dia` nega: fábrica `:49–51`. Autoridade real `disputa_emissao.py:818–844`; nega somente sem célula com escala vigente (`:833–843`). Docstring `:819–823` exige lavra antes de questionário. Se lâmpada acusadora existente e emissor negar por leitura inconsistente, confronto com `CLAUDE.md:167–170`; caso concreto [nao verificado].
- Sem ata e teto por marcos: fábrica `:52–54`; aguarda porque não há marco para comparar; lei específica além de teto/lâmpada [nao verificado].
- Sem ata e sem marcos passados: chama `classificar_falta` e pode obter nenhum motivo, fábrica `:55–57`. Se isso cala célula/ata que acusa, é consumidor rejulgando contra `CLAUDE.md:170`; ocorrência [nao verificado].
- Filtra faltantes por vencimento, data, mapeamento e unicidade: fábrica `:59–65`. Vencimento tem teto; identidade/data preservam marco do dia. Mapeamento ausente que silencie marco acusador confronta `CORTES.md:263`, ocorrência [nao verificado].

Depois dos motivos, `perguntar_pela_celula`:

- Limbo exportado vai à pauta em vez de pergunta: fábrica `:171–173`; juiz `limbo_vai_a_pauta :180–188`; pauta `:225–242`. Exceção expressa comentada fábrica `:167–170` e aval em `app/docs/PROMPTS.md:1719`. Não confundir com fronteira geral exportada, abolida por `app/docs/CORTES.md:219` e `app/docs/LEIS.md:91`.
- Pauta já existente impede duplicata: fábrica `:208–222`, `:234–236`; idempotência escrita em `CLAUDE.md:175` e fábrica `:228–230`.

Atenção ao contrato de erros: `_ata_do_dia` **engole** erro real de `_marcos_faltantes` em `app/chamados/services/disputa_emissao.py:828–832`, registrando por `core.observ.registrar_engolido` e retornando `(True, None)`. Sonda deve observar essa chamada/registrador por mock, contar o erro e marcar incompleto; retorno normal do emissor não garante ausência de erro.

## 06/11 10:35 · sessao 63c4f7796b6e · PEP-REENTRADA · verificador · origem=Code

Parecer estático: execução dos testes, dos mutantes e comportamento em produção `[nao verificado]`. [nao verificado]

**1. P1–P4: cura e mutantes sobreviventes**

| Item | Parecer |
|---|---|
| **P1** | **Cura implementada para observar entrada no serviço durante o voo:** o espia registra `pergunta_em_voo(p.pk)` e exige pelo menos um `True`. Ele não identifica exclusivamente o receiver que originou a chamada. `app/chamados/tests/test_pep_reentrada.py:90`, `app/chamados/tests/test_pep_reentrada.py:97`, `app/chamados/tests/test_pep_reentrada.py:99`. |
| **P2** | **Curado quanto ao estado persistido do trio nos dois alvos**, mas **parcial quanto à exigência “dentro do voo”**: sem credor verifica depois de sair do `with`; S144 verifica ainda dentro. `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/tests/test_pep_reentrada.py:256`, `app/chamados/tests/test_pep_reentrada.py:259`, `app/chamados/tests/test_pep_reentrada.py:274`, `app/chamados/tests/test_pep_reentrada.py:278`. |
| **P3** | **O mutante original passa a ser distinguido:** ambas sem destino atravessariam `if q.chamado_destino_id: continue`, inclusive a P protegida; a exigência de P sem carimbo o reprovaria. A alteração, porém, muda o ingresso da disputa em `_cands`. `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/tests/test_pep_reentrada.py:259`, `app/chamados/services/disputa_emissao.py:1652`. |
| **P4** | **Curado:** C7 e C9 agora coincidem com registro e mapa. `app/docs/JUIZES.md:54`, `app/docs/JUIZES.md:55`, `app/core/juizes.py:267`, `app/core/juizes.py:269`, `app/docs/MAPA-JUIZES.md:97`, `app/docs/MAPA-JUIZES.md:105`. |

Mutantes de produção que os testes curativos ainda não distinguem — sobrevivência inferida pela leitura; execução `[nao verificado]`: [nao verificado]

- **P1:** substituir o corpo do resolver por retorno de contadores zerados. O espia continuaria registrando entrada em voo; o próprio materializador fecharia o destino e produziria seu carimbo. O teste exige entrada, destino fechado, batida e um `carimbar` bem-sucedido pela via da declaração, sem exigir efeito exclusivo do serviço observado. `app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:101`, `app/chamados/tests/test_pep_reentrada.py:103`, `app/chamados/tests/test_pep_reentrada.py:105`, `app/chamados/services/materializacao.py:949`, `app/chamados/services/materializacao.py:1251`.
- **P2:** nas guardas de voo dos dois alvos, executar `descarimbar()` antes de `continue`. Haveria escrita indevida do trio, mas os valores continuariam exatamente os vazios esperados. Portanto, conferir estado intacto não prova ausência de escritor. `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`, `app/chamados/models.py:2646`, `app/chamados/models.py:2652`, `app/chamados/tests/test_pep_reentrada.py:260`, `app/chamados/tests/test_pep_reentrada.py:279`.
- **P3:** trocar a guarda por `if pergunta_em_voo(q.pk) and not q.chamado_destino_id: continue`. O novo caso permanece protegido, mas perde-se a proteção da P em voo **com destino** no laço sem credor. O laço admite destinos encerrados; só um destino vivo barra a disputa. `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/services/disputa_emissao.py:1662`, `app/chamados/services/disputa_emissao.py:1671`.

**2. O espia de P1 está no nome correto?**

**Sim.** O receiver faz import local de `chamados.models.resolver_perguntas_por_chamado_resolvido` e chama esse nome; é precisamente o atributo substituído pelo teste. Não há outro ramo do próprio receiver que chame o resolver por um nome diferente. `app/chamados/signals.py:89`, `app/chamados/signals.py:92`, `app/chamados/tests/test_pep_reentrada.py:97`.

Há outros **chamadores do serviço**, também pelo nome observado: `_retratar`, reatamento de órfã e fechamento de beco. Eles não escapam do espia; podem, contudo, tornar ambígua a atribuição “foi o receiver S143”. Quais efetivamente chamam durante esse fixture, inclusive com S143 desligado, `[nao verificado]`. `app/chamados/reconciliador.py:508`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1528`, `app/chamados/reconciliador.py:1631`, `app/chamados/services/becos.py:40`, `app/chamados/services/becos.py:49`.

O registro ocorre **antes** de executar o serviço, e o receiver engole exceções: o `True` comprova entrada, não conclusão bem-sucedida. `app/chamados/tests/test_pep_reentrada.py:91`, `app/chamados/tests/test_pep_reentrada.py:92`, `app/chamados/signals.py:93`.

**3. P3 mantém o caminho anterior?**

**Mantém o laço, mas muda o caminho de entrada.** Antes, a P destinada também colocava a disputa no conjunto derivado de `perguntas`; agora ambas estão sem destino, o filtro inicial fica vazio para elas e a disputa entra pela consulta das abertas do colaborador. `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/services/disputa_emissao.py:1572`, `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1653`.

Pai resolvido e validações nulas permitem alcançar `_abertas`; o controle da irmã exige que alguma escrita realmente aconteça nesse laço. Portanto, a nova configuração não torna a guarda inatingível. `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:251`, `app/chamados/tests/test_pep_reentrada.py:258`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1665`.

O limite de realismo continua relevante: o fixture mantém texto, apaga resposta/validação, declara voo manualmente e força C6 a “morta”. A porta normal seleciona perguntas com resposta e validação gravadas; com C6 real, o próprio docstring declara que a irmã não receberia o carimbo. Isso prova a guarda isolada e perde cobertura do ingresso anterior por destino; não demonstra a cadeia real nesse estado. `app/chamados/tests/test_pep_reentrada.py:238`, `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/services/materializacao.py:1343`.

**4. P4: correspondência documental**

| Autoridade | JUIZES | Registro | MAPA |
|---|---|---|---|
| C7: `DisputaSupervisao.pode_fechar` | `app/docs/JUIZES.md:54` | `app/core/juizes.py:267` | `app/docs/MAPA-JUIZES.md:97` |
| C9: `catalogo/motor.py::prazo_estourou` | `app/docs/JUIZES.md:55` | `app/core/juizes.py:269` | `app/docs/MAPA-JUIZES.md:105` |

**5. Classes de reprovação no delta**

| Classe | Resultado da leitura |
|---|---|
| **Segundo juiz** | O voo é pertencimento por PK condicionado a atomic aberto; não recalcula vitalidade nem fechamento. C6 continua na porta do carimbo e C7 na cauda. `app/chamados/services/materializacao.py:1129`, `app/chamados/models.py:2596`, `app/chamados/services/disputa_emissao.py:1712`. |
| **Segundo escritor fora da porta** | As guardas novas apenas pulam os candidatos. Entretanto, o espia principal conta somente chamadas de `carimbar` que retornam `True`: uma escrita direta seguida de sobrescrita pelo materializador pode escapar dessa contagem. `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/materializacao.py:1399`, `app/chamados/tests/test_pep_reentrada.py:69`, `app/chamados/tests/test_pep_reentrada.py:105`. |
| **Relógio fora da borda** | O sinal novo não usa relógio; o fixture congela o instante. `app/chamados/services/materializacao.py:1115`, `app/chamados/services/materializacao.py:1129`, `app/chamados/tests/test_pep_reentrada.py:35`. |
| **Derivação paralela** | O sinal guarda PKs; os carimbos continuam delegando aos juízes existentes. `app/chamados/services/materializacao.py:1120`, `app/chamados/services/materializacao.py:1207`, `app/chamados/models.py:2596`. |
| **Selo sem mordida** | Há controles positivos, mas persistem os limites de origem do espia, estado versus escrita e mudança de caminho de P3 descritos acima. `app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:258`, `app/chamados/tests/test_pep_reentrada.py:281`. |
| **Ordem de trava** | A mudança do bloco acrescenta o contexto depois do atomic; o contexto só manipula `ContextVar`. Validação concorrente da ordem de travas `[nao verificado]`. `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1120`. |
| **Pouso antes do pré-requisito** | O atomic entra antes do voo; declarar voo fora de atomic levanta erro. O carimbo normal permanece depois do plantio e do juiz do lastro. `app/chamados/services/materializacao.py:1118`, `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1251`. |
| **Estado de módulo que sobrevive** | Existe `ContextVar` de módulo, mas o valor anterior é restaurado por token em `finally`; há testes de rollback, exceção e aninhamento. Passagem desses testes `[nao verificado]`. `app/chamados/services/materializacao.py:1106`, `app/chamados/services/materializacao.py:1124`, `app/chamados/tests/test_pep_reentrada.py:150`, `app/chamados/tests/test_pep_reentrada.py:167`, `app/chamados/tests/test_pep_reentrada.py:179`. |

**6. Quem escreve e quem lê o trio dentro do voo?**

**Nos caminhos atuais examinados, não identifiquei escritor da P além do materializador. Essa garantia está nos caminhos, não nas portas genéricas.** `app/chamados/services/materializacao.py:1251`, `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`.

| Papel | Caminho |
|---|---|
| **Escritor normal** | `_materializar_uma → carimbar → _escrever_carimbo → save`, atribuindo o trio junto. `app/chamados/services/materializacao.py:1251`, `app/chamados/models.py:2556`, `app/chamados/models.py:2609`. |
| **Escritor alternativo do próprio materializador** | A recusa por paridade usa `carimbar_identidade` e retorna `None`, impedindo o carimbo normal posterior. `app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:900`, `app/chamados/services/materializacao.py:1201`. |
| **S143/S144** | Os dois laços de S143 pulam a P; sua cauda não fecha disputa com P em voo; S144 também pula antes de carimbar. `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`, `app/chamados/services/materializacao.py:1399`. |
| **Reconciliador** | A batida o aciona sincronamente. A absolvição por célula pode carimbar, mas exige validação nula, excluindo a P selecionada pelos chamadores atuais; a lavra sem turno exige ausência de texto. `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1345`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/reconciliador.py:364`. |
| **Dedup/descarimbo** | Migração considera perguntas vivas, enquanto C6 elimina imediatamente a validada; ressuscitação exige carimbo existente e texto vazio. `app/chamados/services/fio.py:935`, `app/chamados/juizes.py:1136`, `app/chamados/models.py:1434`. |
| **Leitores** | A porta fotografa o trio anterior; C6 e lastro leem materialização/via; S143, S144 e irmãs sem consumo usam materialização nos filtros. O autor é preservado no snapshot. `app/chamados/models.py:2583`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:647`, `app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/materializacao.py:1393`, `app/chamados/juizes.py:1748`. |

A recusa por colisão no `finally` pode escrever identidade **depois da saída do voo**; ela não constitui outro escritor dentro desse bloco. Já `carimbar_identidade` e `descarimbar` continuam sem consultar o voo: ampliar os chamadores para estados como o fixture de P3 exigiria nova análise de alcançabilidade. `app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1261`, `app/chamados/services/materializacao.py:1271`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`, `app/chamados/tests/test_pep_reentrada.py:234`.

## 06/11 10:33 · sessao 083c8ea8b2b0 · - · - · origem=Ronald (tmux)

No bloco atual, não encontrei escritor do trio da P além do materializador; a garantia depende dos caminhos atuais, pois as portas genéricas não consultam o voo (`app/chamados/services/materializacao.py:1194`, `app/chamados/models.py:2562`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`).

**Quem escreve:**

- O carimbo normal sai de `_materializar_uma → p.carimbar`, após o juiz do lastro; a porta atribui os três campos e grava (`app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1251`, `app/chamados/models.py:2556`, `app/chamados/models.py:2609`).
- O próprio ramo hora pode escrever identidade por paridade e retornar imediatamente, impedindo o carimbo normal; continua sendo o materializador (`app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:900`, `app/chamados/services/materializacao.py:1201`).
- A recusa `COLISAO` pode escrever identidade no `finally`, mas esse `finally` roda após a saída de `materializacao_em_voo`; portanto não é outro escritor **dentro** do voo (`app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1261`, `app/chamados/services/materializacao.py:1271`).

**Outros candidatos e por que não escrevem a P atual:**

- S143 pula a P nos dois laços e bloqueia o fecho da disputa com qualquer P em voo (`app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`).
- S144 pula antes do juiz e do carimbo (`app/chamados/services/materializacao.py:1399`, `app/chamados/services/materializacao.py:1405`).
- A batida chama sincronamente `reconciliar_chamados_colab`; sua absolvição por célula carimba sem consultar voo, mas exige `validada_em IS NULL`. A porta normal seleciona validação não nula; a validação individual grava antes de materializar; o passivo também seleciona validação não nula (`app/ponto/signals.py:206`, `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1343`, `app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:424`, `app/chamados/services/vigia_carimbo.py:98`).
- `lavrar_sem_turno_aberto` exige ausência de texto, incompatível com hora que chega ao plantio e com `data_hora` parseável (`app/chamados/reconciliador.py:364`, `app/chamados/reconciliador.py:405`, `app/chamados/services/materializacao.py:804`, `app/chamados/services/materializacao.py:970`).
- Dedup só processa perguntas vivas; o C6 dá por morta imediatamente a P validada (`app/chamados/services/fio.py:931`, `app/chamados/services/fio.py:935`, `app/chamados/juizes.py:1136`).
- Ressuscitação/descarimbo filtra pergunta já materializada e sem texto; a P hora em voo tem texto (`app/chamados/models.py:1434`, `app/chamados/models.py:1437`, `app/chamados/models.py:1504`, `app/chamados/services/materializacao.py:804`).
- Re-lavra de via não está ligada ao sinal da batida; `_gravar_estado` re-lavra somente o veredito, não o trio (`app/ponto/signals.py:94`, `app/chamados/models.py:291`, `app/chamados/veredito_lavrado.py:227`).

**Quem lê:**

- `carimbar` fotografa o trio anterior e escreve pela porta única; C6 e juiz do lastro leem materialização/via; a lavra delega ao juiz do lastro (`app/chamados/models.py:2583`, `app/chamados/models.py:2586`, `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1207`, `app/chamados/juizes.py:647`, `app/chamados/juizes.py:673`, `app/chamados/veredito_lavrado.py:219`).
- Os universos de S143, S144, irmãs sem consumo e reconciliador filtram `materializada_em`; o autor é preservado no snapshot e atribuído pela porta, sem participar desses predicados (`app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/disputa_emissao.py:1666`, `app/chamados/services/materializacao.py:1393`, `app/chamados/juizes.py:1748`, `app/chamados/reconciliador.py:619`, `app/chamados/models.py:2583`, `app/chamados/models.py:2559`).

O limite estrutural merece constar: uma chamada nova de `_materializar_uma` com validação nula poderia entrar no universo do reconciliador, porque ele não consulta voo; os chamadores atuais evitam isso pelos filtros acima, e o próprio teste P3 declara esse limite (`app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1132`, `app/chamados/tests/test_pep_reentrada.py:234`).

## 06/11 10:33 · sessao d90ff57f7b2e · - · - · origem=Ronald (tmux)

- **Item 2:** o patch de P1 entra no nome correto: o teste troca `chamados.models.resolver_perguntas_por_chamado_resolvido` em `app/chamados/tests/test_pep_reentrada.py:97`; o receiver faz import local desse mesmo nome e o chama em `app/chamados/signals.py:89` e `app/chamados/signals.py:92`; o reexport vem de `app/chamados/models.py:3281`. Não há outro ramo do próprio receiver que execute o resolver sem passar por esse alias (`app/chamados/signals.py:81`, `app/chamados/signals.py:88`).

- **Limite de P1:** o espia comprova entrada no serviço durante o voo, mas não identifica qual chamador entrou nem comprova conclusão do serviço: só registra `pergunta_em_voo(p.pk)` antes de chamar o original (`app/chamados/tests/test_pep_reentrada.py:90`); o receiver engole exceções (`app/chamados/signals.py:93`). Há chamadas diretas pelo mesmo alias, também vistas pelo espia, em `app/chamados/reconciliador.py:508`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1528`, `app/chamados/reconciliador.py:1631`, `app/chamados/services/becos.py:40` e `app/chamados/services/becos.py:49`. Quais efetivamente acontecem no fixture: `[nao verificado]`.

- **Mutante sobrevivente de P1, no teste que o cura:** substituir o serviço por retorno `{'perguntas_resolvidas': 0, 'disputas_fechadas': 0}` preservaria a chamada observada em voo e deixaria o materializador produzir seu carimbo único; o método exige apenas ocorrência de `True`, destino fechado, batida presente e uma gravação com `VIA_DECLARACAO` (`app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:101`, `app/chamados/tests/test_pep_reentrada.py:103`, `app/chamados/tests/test_pep_reentrada.py:105`; carimbo do materializador em `app/chamados/services/materializacao.py:1251`). Resultado executado do mutante: `[nao verificado]`.

- **Item 3:** P3 continua atingindo o mesmo laço-alvo, mas **não pelo mesmo ingresso em `_cands`**: antes P apontava para `destino`; agora remove a FK em `app/chamados/tests/test_pep_reentrada.py:250`. Como a irmã também nasce sem destino (`app/chamados/tests/test_pep_reentrada.py:244`), o filtro `chamado_destino=chamado` fica sem essas perguntas (`app/chamados/services/disputa_emissao.py:1572`). A disputa entra pela busca de todas as abertas do colaborador (`app/chamados/services/disputa_emissao.py:1653`), enquanto antes também entrava pelo conjunto derivado de `perguntas` (`app/chamados/services/disputa_emissao.py:1652`). Pai resolvido e validações nulas satisfazem os filtros do laço (`app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:251`; filtros em `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1665`).

- **P3 mata o mutante original:** trocar a guarda por `if q.chamado_destino_id: continue` deixaria ambas passarem, inclusive P em voo, contrariando a exigência de P sem carimbo (`app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/tests/test_pep_reentrada.py:259`; ponto de mutação em `app/chamados/services/disputa_emissao.py:1671`). Execução: `[nao verificado]`.

- **Mutante sobrevivente de P3:** `if pergunta_em_voo(q.pk) or q.chamado_destino_id: continue` mantém o teste verde porque ambas estão sem destino, mas passa a pular indevidamente irmãs fora de voo com destino morto; esse universo é aceito pelo laço de produção, que só exclui a disputa quando existe destino **vivo** (`app/chamados/services/disputa_emissao.py:1662`, `app/chamados/services/disputa_emissao.py:1671`; fixture em `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`). Execução: `[nao verificado]`.

- **Realismo de P3:** o limite de C6 mockado está agora declarado explicitamente (`app/chamados/tests/test_pep_reentrada.py:238`); a modificação adicional perde cobertura do ingresso por pergunta com destino, mas não impede atingir a guarda do laço sem credor (`app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1671`). O caso continua sintético: remove carimbos de resposta/validação mantendo o texto e declara voo manualmente (`app/chamados/tests/test_pep_reentrada.py:245`, `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:254`); portanto sua evidência é da guarda, não da cadeia real de materialização (`app/chamados/tests/test_pep_reentrada.py:239`).

## 06/11 10:33 · sessao 8b3b6fa3ee26 · - · - · origem=Ronald (tmux)

P4 está curado: `app/docs/JUIZES.md:54` aponta C7 para `DisputaSupervisao.pode_fechar`, como `app/core/juizes.py:267` e `app/docs/MAPA-JUIZES.md:97`; SLA/C9 em `JUIZES.md:55` coincide com `core/juizes.py:269` e `MAPA-JUIZES.md:105`.

P2 está curado quanto ao **estado persistido do trio** nos dois cenários pedidos: sem credor verifica `materializada_em` e `(via_resolucao, materializada_por_id)` em `app/chamados/tests/test_pep_reentrada.py:259` e `:260`; S144 verifica em `:278` e `:279`. Contudo, sem credor lê depois de sair do bloco (`:253`, `:256`); somente S144 lê dentro (`:274`, `:276`). A afirmação literal “trio intacto dentro do voo nos dois testes” é mais forte que o observado no primeiro (`:253`, `:256`).

Mutante substantivo sobrevivente aos **dois testes curativos de P2**: estreitar as respectivas guardas para `if pergunta_em_voo(pk) and p.tipo_resposta == 'hora': continue`. Os fixtures são todos `hora` (`app/chamados/tests/test_pep_reentrada.py:57`, `:245`, `:270`), mas produção põe também `data_hora` e `escolha` em voo (`app/chamados/services/materializacao.py:1194`, `:1197`, `:1199`); as guardas atuais não restringem tipo (`app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`). Sobrevivência inferida estaticamente, sem executar mutação: `[nao verificado]`.

Outro limite: assertar o trio vazio não prova ausência de escrita. Um `descarimbar()` na guarda grava o mesmo trio vazio (`app/chamados/models.py:2646`, `:2652`), já esperado pelos testes (`app/chamados/tests/test_pep_reentrada.py:260`, `:279`); os valores iniciais vêm dos defaults (`app/chamados/models.py:2323`, `:2791`, `:2795`). Isso é uma limitação do selo por estado, sem demonstrar defeito no código atual (`app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`).

Nas classes solicitadas, o delta v4 acrescenta observação/assertions e corrige lookup, sem introduzir decisão de produção: `app/chamados/tests/test_pep_reentrada.py:90`, `:97`, `:250`, `:260`, `:279`; `app/docs/JUIZES.md:54`. No diff completo, o sinal não replica C6/C7: consulta apenas pertencimento e transação (`app/chamados/services/materializacao.py:1127`), enquanto carimbo e fecho continuam lendo os juízes existentes (`app/chamados/models.py:2596`, `app/chamados/services/disputa_emissao.py:1712`). O reset está em `finally`, com token (`app/chamados/services/materializacao.py:1120`, `:1123`); há cobertura explícita de rollback, exceção e aninhamento (`app/chamados/tests/test_pep_reentrada.py:150`, `:167`, `:179`). O bloco entra primeiro no atomic e depois declara voo (`app/chamados/services/materializacao.py:1194`); a declaração exige atomic aberto (`:1118`). Não há evidência fornecida de execução dos testes ou de validação concorrente de ordem de travas: `[nao verificado]`.

## 06/11 10:32 · sessao 6d8557c6b3d4 · H12H06-B · sonda · origem=Code

Transcrição literal do dado fornecido [nao verificado]. Não executei o wrapper: ele escreve arquivos (`bin/sonda_leitura.sh:132`, `bin/sonda_leitura.sh:250`).

```text
sonda_leitura: pago_x_devido.py [b04491167a9f3c6b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

col|status_fm|entregue|grav_ok|grav_perd|grav_reflexo|antes_diff(ok,perd,refl)|devido(ok,perd,refl)|grav==antes
col159|aberto|False|4|1|0.9943|(4, 1, None)|(5, 0, None)|True
col284|aprovado|True|4|1|0.1914|(4, 1, None)|(5, 0, None)|True
col342|aprovado|True|4|1|2.4046|(4, 1, 1.9195)|(5, 0, 3.2714)|False
col400|aprovado|True|4|1|1.2723|(4, 1, 0.9864)|(5, 0, 1.2125)|False
col433|aberto|True|2|3|0.0000|(2, 3, None)|(4, 1, None)|True
col842|aberto|True|3|1|1.3746|(3, 1, 1.3849)|(4, 0, 1.7164)|False
col905|aberto|True|0|1|0.0000|(0, 1, None)|(1, 0, None)|True
UNIVERSO 7 esperado 7
```

(1) UNIVERSO fecha: **sim, 7 esperado 7**, conforme impresso. [nao verificado]

(2) e (3), conforme impresso:

| col | grav==antes | entregue | Referência |
|---|---|---|---|
| col159 | True | False | [nao verificado] |
| col284 | True | True | [nao verificado] |
| col342 | False | True | [nao verificado] |
| col400 | False | True | [nao verificado] |
| col433 | True | True | [nao verificado] |
| col842 | False | True | [nao verificado] |
| col905 | True | True | [nao verificado] |

## 06/11 10:29 · sessao cf215076862f · H12H06-B · sonda · origem=Code

Não executei `dry_comp11.py`: o arquivo não foi localizado na cópia. Segue a saída literal fornecida no pedido. [nao verificado] [nao verificado]

```text
sonda_leitura: dry_comp11.py [6be44cbafd75e60b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

pk|col|tipo|data|data_fim|status|grav|decl|jornada|novo|novo==jornada(falta)/0(atestado)|cerca|veredito
5096|col940|atestado|2026-10-21|-|aguardando_documento|720|-|480|0|True|-|mudaria
5124|col940|atestado|2026-10-22|-|aguardando_documento|720|-|480|0|True|-|mudaria
5184|col723|atestado|2026-10-23|-|aguardando_documento|720|-|440|0|True|-|mudaria
5206|col558|atestado|2026-10-24|-|aguardando_documento|720|-|720|0|True|-|mudaria
5305|col493|atestado|2026-10-28|-|aguardando_documento|720|-|530|0|True|-|mudaria
5332|col735|atestado|2026-10-29|-|aguardando_decisao|720|-|540|0|True|-|mudaria
5334|col371|atestado|2026-10-30|-|aguardando_documento|720|-|420|0|True|-|mudaria
5336|col363|atestado|2026-10-22|-|aguardando_documento|720|-|480|0|True|-|mudaria
5345|col288|atestado|2026-10-30|-|aguardando_decisao|720|-|660|0|True|-|mudaria
5348|col723|atestado|2026-10-31|-|aguardando_documento|720|-|440|0|True|-|mudaria
5349|col723|atestado|2026-10-30|-|aguardando_documento|720|-|440|0|True|-|mudaria
5366|col455|atestado|2026-11-01|-|rejeitada|720|-|720|0|True|-|pulada:identidade_mudou
5128|col967|atestado|2026-10-21|-|aprovada|720|-|440|0|True|-|mudaria
5182|col200|atestado|2026-10-24|2026-10-25|aprovada|720|-|840|0|True|-|mudaria
5197|col701|atestado|2026-10-23|-|aprovada|720|-|480|0|True|-|mudaria
5236|col891|atestado|2026-10-27|-|aprovada|720|-|720|0|True|-|mudaria
5198|col444|falta|2026-10-24|-|aprovada|720|-|420|420|True|-|mudaria
5335|col905|falta|2026-10-30|-|aprovada|720|-|660|660|True|-|mudaria
5339|col304|falta|2026-10-29|-|aprovada|720|-|555|555|True|-|mudaria
5340|col304|falta|2026-10-28|-|aprovada|720|-|555|555|True|-|mudaria
5351|col823|falta|2026-11-02|-|aprovada|720|-|240|240|True|-|mudaria
5355|col869|falta|2026-11-02|-|aprovada|720|-|480|480|True|-|mudaria
5357|col924|falta|2026-10-26|-|aprovada|720|-|440|440|True|-|mudaria
5358|col924|falta|2026-11-01|-|aprovada|720|-|440|440|True|-|mudaria
5359|col924|falta|2026-10-28|-|aprovada|720|-|440|440|True|-|mudaria
5360|col924|falta|2026-10-27|-|aprovada|720|-|440|440|True|-|mudaria
5369|col924|falta|2026-10-29|-|aprovada|720|-|440|440|True|-|mudaria
5333|col145|atestado|2026-10-30|2026-12-28|rejeitada|720|-|19800|0|True|-|pulada:identidade_mudou
5353|col769|falta|2026-11-01|-|aprovada|720|-|660|660|True|-|mudaria
5311|col33|atestado|2026-10-29|-|aguardando_decisao|720|-|660|0|True|-|mudaria
UNIVERSO 30 esperado 30
RESUMO mudaria=28 ja_aplicada=0 pulada=2 parcial_dp=0 erros=0
SOMA 30 == 30: True
```

1. No dado fornecido, SOMA e UNIVERSO fecham: `30 == 30: True` e `30 esperado 30`. [nao verificado]
2. Vereditos diferentes de `mudaria`, por pk: [nao verificado]
   - `5333`: `pulada:identidade_mudou`. [nao verificado]
   - `5366`: `pulada:identidade_mudou`. [nao verificado]
3. Nenhuma linha fornecida tem `novo==jornada(falta)/0(atestado)` igual a `False`. [nao verificado]

## 06/11 10:26 · sessao faf686f2cce0 · CAUSA-D-ESTRUTURA · sonda · origem=Code

**O raspado fecha os 200 dias; os três grupos chegam a ESTRUTURA pelo mesmo caminho residual. Ele não demonstra três causas materiais distintas.** A diferença medida entre os grupos é a cardinalidade par; os outros predicados informados são iguais. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`; valores recebidos: [nao verificado])

**1. Saída e conferência**

Esta é a saída recebida, não uma reexecução minha. O conteúdo de `sonda_causa.py`, sua correspondência com o hash `688ae6ab76dea90a` e a execução pelo envelope não foram conferidos independentemente. [nao verificado]

| Conferência do raspado | Resultado | Evidência |
|---|---:|---|
| `SEM_PORTA_CLASSE_D` | 217 | Anexo [nao verificado] |
| `alcance_dias_omitidos` | 0 | Anexo [nao verificado] |
| `UNIVERSO_DONO_ESTRUTURA` | **200 / esperado 200 — CONFERE** | Anexo [nao verificado] |
| Fora do universo | BATIDA 3; CADASTRO 14 | Anexo [nao verificado] |
| Reconciliação da classe d | 200 + 3 + 14 = **217** | Cálculo sobre o anexo [nao verificado] |
| `SOMA_DOS_GRUPOS` | 182 + 13 + 5 = **200 — True** | Cálculo sobre o anexo [nao verificado] |
| `rc_sombra` | **0** | Anexo [nao verificado] |
| `INCOMPLETO` / erros impressos | Nenhum consta no trecho recebido | Anexo [nao verificado] |

| Grupo | n | Cadastro: folga indefinida / sem vínculo / sem vínculo em trabalho | Turno | Batidas | Fase lavrada | CXR do motor | Evidência |
|---|---:|---|---|---|---|---|---|
| 1 | **182** | False / False / False | não aberto | 0, par | False | False | Anexo [nao verificado] |
| 2 | **13** | False / False / False | não aberto | 4+, par | False | False | Anexo [nao verificado] |
| 3 | **5** | False / False / False | não aberto | 2, par | False | False | Anexo [nao verificado] |
| **SOMA** | **200** | | | | | | Cálculo sobre o anexo [nao verificado] |

| MARGINAL | Distribuição | Evidência |
|---|---|---|
| P1 — cadastro | três flags False: **200** | Anexo [nao verificado] |
| P2 — batida fora de turno | `turno_nao_aberto`: **200** | Anexo [nao verificado] |
| P3 — paridade | 0: **182**; 4+ pares: **13**; 2: **5** | Anexo [nao verificado] |
| P4 — fase | `fase_lavrada=False`: **200** | Anexo [nao verificado] |
| P5 — CXR | `em_cxr_do_motor=False`: **200** | Anexo [nao verificado] |

**2. Quais predicados deixaram os dias no resíduo**

A montagem real é `InsumosDoDono.dono`: carrega cadastro, consulta `batida_fora_de_turno`, lê a fase e pergunta a pertença à CXR do motor. A paridade usa `oraculo.horas`. ([e6_oraculo.py:172](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/ponto/management/commands/e6_oraculo.py:172), `app/ponto/management/commands/e6_oraculo.py:144`)

Esta cadeia vale **para cada um dos três grupos**, com a variação de P3 explicitada:

| Predicado real | Valor recebido | Por que não captura o dia |
|---|---|---|
| P1 — `folga_nao_definida or sem_vinculo_no_dia or sem_vinculo_em_dia_de_trabalho` | False / False / False [nao verificado] | O OR é falso; não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:98`) |
| P2 — `batida_fora_de_turno` | `turno_nao_aberto` [nao verificado] | `.aberto=False` faz a função retornar False imediatamente; a consulta das entradas soltas nas batidas apuráveis não é alcançada. (`app/ponto/management/commands/e6_oraculo.py:196`, `:198`) |
| P3 — `len(horas_do_dia or ()) % 2` | G1: 0; G2: 4+ par; G3: 2 [nao verificado] | O resto é zero nos três grupos; não retorna BATIDA. (`app/ponto/management/commands/e6_oraculo.py:106`) |
| P4 — `fase_deslocada` | False [nao verificado] | Não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:108`) |
| P5 — `em_cadastro_x_realidade` | False [nao verificado] | O ternário final escolhe **ESTRUTURA**. ([e6_oraculo.py:110](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/ponto/management/commands/e6_oraculo.py:110)) |

Há quatro limites para interpretar esses negativos:

- **Cadastro False não certifica todo o cadastro.** Os flags vêm do vínculo do dia e de `defeito_folga_nao_definida`; este último acusa especificamente o 6x1 sem folga semanal no template e sem `FolgaDia` no mês. (`app/ponto/supra_juiz.py:193`, `:200`; `app/escala/models.py:1241`, `:1247`)
- **“Turno não aberto” não prova turno fechado.** Sem qualquer turno do dia, a autoridade também retorna `aberto=False`, junto de `sem_turno=True`. ([turnos.py:717](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/ponto/turnos.py:717), `app/ponto/turnos.py:596`)
- **Fase False não prova fase correta.** Dia sem nenhuma lavra cobrindo também fica fora do conjunto de fase deslocada. (`app/escala/services/fase_12x36.py:123`)
- **CXR False significa ausência na lista lida.** A leitura admite resultado/lista ausente como vazio e descarta itens sem `dia`; o raspado não distingue essas situações. (`app/ponto/management/commands/e6_oraculo.py:160`; situação dos casos: [nao verificado])

**Lei do desenho e limite por grupo**

Existe lei escrita comum ao beco: **“Emissao, pergunta e chamado nascem do sinal da celula”**, e o corte explicita **“lampada acesa no marco = fio vivo obrigatorio”**. ([CLAUDE.md:168](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/CLAUDE.md:168), [CORTES.md:263](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/docs/CORTES.md:263))

Contudo, a classe **d atual** é definida por ausência de pergunta viva, dia fora da lista e colaborador presente na lista, depois de recusadas as rotas anteriores. Ela não identifica qual emissor falhou nem equivale automaticamente ao GRUPO-D histórico. (`app/chamados/services/vigia_fio.py:635`, `:654`, [vigia_fio.py:677](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/chamados/services/vigia_fio.py:677), `app/docs/CORTES.md:263`)

| Grupo | O que fica explicado | Lei escrita e o que não fica demonstrado |
|---|---|---|
| **1 — 182, zero par** | P1/P2/P4/P5 negativos; P3 é `0 % 2 = 0`. O código declara expressamente zero como par e mantém o dia no resíduo ESTRUTURA. (`app/ponto/management/commands/e6_oraculo.py:72`, `:98`, `:106`, `:110`; valores: [nao verificado]) | Lei comum do fio: `CLAUDE.md:168` / `CORTES.md:263`. Atribuição errada à meia-noite aparece como explicação no comentário, mas sua ocorrência nesses 182 dias não foi mostrada. (`app/ponto/management/commands/e6_oraculo.py:73`; ocorrência: [nao verificado]) |
| **2 — 13, quatro ou mais pares** | P1/P2/P4/P5 negativos; P3 tem resto zero. Nenhum desses testes retira o dia do resíduo. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`; valores: [nao verificado]) | Mesma lei do fio. Quantidade par não demonstra casamento correto com marcos, intervalo ou valor; horários, ata e valores desses casos estão [nao verificado]. |
| **3 — 5, duas pares** | P1/P2/P4/P5 negativos; P3 é `2 % 2 = 0`. O retorno final continua ESTRUTURA. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`; valores: [nao verificado]) | Mesma lei do fio. Duas batidas do oráculo e `aberto=False` não demonstram um turno real fechado; o realizado também admite ausência de turno com esse flag. (`app/ponto/turnos.py:717`; situação dos cinco casos: [nao verificado]) |

A L-099 exige coerência com o cadastro existente e nomeia esse juiz; o contrato estrutural exige **“1 JUIZ por pergunta”**. O raspado não mostra dois juízes discordando, nem valores divergentes entre leitores, portanto não prova uma quebra específica desses contratos em cada grupo. (`app/docs/LEIS.md:122`, [ESTRUTURAL.md:4](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/docs/ESTRUTURAL.md:4), `app/docs/ESTRUTURAL.md:29`; divergências específicas: [nao verificado])

**Não há lacuna de lei demonstrada para o retorno do dono: o resíduo já está escrito.** O que falta é evidência da causa material. Chamar essa falta de evidência de “sem lei” inventaria uma necessidade de critério novo. (`app/ponto/management/commands/e6_oraculo.py:69`, `:110`; causa material por grupo: [nao verificado])

**3. Dois exemplos por grupo**

| Grupo | Exemplo 1 | Exemplo 2 | Evidência |
|---|---|---|---|
| 1 | col144 — 2026-09-21 | col144 — 2026-09-23 | Anexo [nao verificado] |
| 2 | col137 — 2026-11-03 | col137 — 2026-11-05 | Anexo [nao verificado] |
| 3 | col222 — 2026-10-28 | col292 — 2026-10-22 | Anexo [nao verificado] |

“Zero batidas” foi preservado como rótulo recebido: na montagem do dono, a contagem vem de `oraculo.horas`, agrupada pela data inicial do turno do oráculo, enquanto `brutas` conta por data civil. Zero nessa chave não prova ausência de marcações civis naquela data. (`app/ponto/management/commands/e6_oraculo.py:144`, `:183`; `app/ponto/calculador/nucleo.py:235`, `:237`)

## 06/11 10:24 · sessao 5064473cb077 · - · - · origem=Ronald (tmux)

Os três grupos percorrem os mesmos cinco testes; só muda a cardinalidade usada na paridade. A função não tem outro juiz depois deles: todos negativos chegam ao retorno residual `ESTRUTURA`. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`)

| Predicado | Valor raspado comum aos grupos | Por que não captura |
|---|---|---|
| `folga_nao_definida or sem_vinculo_no_dia or sem_vinculo_em_dia_de_trabalho` | `False, False, False` [nao verificado] | OR falso; não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:98`) |
| `batida_fora_de_turno` | `turno_nao_aberto` [nao verificado] | `.aberto=False` faz o carregador retornar `False` imediatamente, antes de consultar entradas soltas e sua presença em `batidas_apuraveis`. (`app/ponto/management/commands/e6_oraculo.py:195`) |
| `len(horas_do_dia or ()) % 2` | Grupo 1: `0`; grupo 2: `4+`, par; grupo 3: `2` [nao verificado] | Resto zero nos três grupos; não retorna BATIDA. (`app/ponto/management/commands/e6_oraculo.py:106`) |
| `fase_deslocada` | `False` [nao verificado] | Não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:108`) |
| `em_cadastro_x_realidade` | `False` [nao verificado] | A expressão final escolhe ESTRUTURA. (`app/ponto/management/commands/e6_oraculo.py:110`) |

A montagem real carrega `oraculo.horas`, os turnos da janela ampliada, o cadastro do dia, o conjunto de fase e a lista CXR; chama `dono_da_divergencia` com esses insumos. (`app/ponto/management/commands/e6_oraculo.py:142`, `:148`, `:154`, `:164`, `:172`)

Limites essenciais:

- **“Turno não aberto” não prova turno fechado.** `RealizadoDoDia` tem os campos `minutos, sem_turno, aberto...`; quando não há turno do dia, retorna `None, True, False...`. Portanto, ausência de turno também produz `.aberto=False`. (`app/ponto/turnos.py:596`, `:717`)
- **“0 batidas” é zero na lista `horas` daquela chave do oráculo, não necessariamente zero marcações civis.** `InsumosDoDono` usa `oraculo.horas`; o núcleo agrega essa lista pela data da primeira batida de cada turno, enquanto `brutas` conta por data civil. (`app/ponto/management/commands/e6_oraculo.py:144`; `app/ponto/calculador/nucleo.py:235`, `:237`)
- **`fase_lavrada=False` não prova fase correta.** Um dia sem lavra cobrindo também fica fora do conjunto retornado; o código documenta “não tem medida e não é fase”. (`app/escala/services/fase_12x36.py:117`, `:123`, `:132`)
- **`em_cxr=False` significa ausência na lista lida, não demonstra distâncias pequenas.** A leitura tolera resultado/lista ausente como vazia e descarta itens sem `dia`. (`app/ponto/management/commands/e6_oraculo.py:160`)
- O juiz L-084 exige ambas as distâncias presentes e com módulo maior que 180 minutos; a acusação registra o dia pelo mapa da entrada, podendo obter `None`. Esses mecanismos existem, mas sua ocorrência nos 200 dias não foi demonstrada pelo raspado. (`app/ponto/motor_calculo_v2.py:1946`, `:1988`, `:1997`; ocorrência: [nao verificado])
- Os três flags de cadastro vêm de `_insumos_do_vinculo`: `ec=None` produz `sem_vinculo_no_dia=True`; com vínculo, os dois flags de vínculo são falsos e `folga_nao_definida` lê `ec.defeito_folga_nao_definida(data)`. (`app/ponto/supra_juiz.py:193`, `:198`)
- Esse último defeito é específico: ciclo 6x1 sem `folga_dia_semana` e sem `FolgaDia` no mês; `False` não certifica todo o cadastro. (`app/escala/models.py:1241`, `:1247`)

## 06/11 10:24 · sessao 5b6247571eb3 · - · - · origem=Ronald (tmux)

Lei comum aos três grupos: a emissão deriva da célula — “Emissao, pergunta e chamado nascem do sinal da celula” — e o consumidor lê a lâmpada; o corte do GRUPO-D explicita “lampada acesa no marco = fio vivo obrigatorio”. Esse é o lastro escrito para o beco sem porta, condicionado ao universo fornecido. (`CLAUDE.md:168`, `CLAUDE.md:170`, `app/docs/CORTES.md:263`; medição fornecida [nao verificado])

Não há prova de três mecanismos diferentes de quebra: os grupos variam somente entre 0, 4+ e 2 batidas pares; todos os demais predicados fornecidos são iguais. Os horários, marcos, ata, valores de realizado e atribuição entre dias não aparecem no dado. [nao verificado]

- **182 / zero:** o código declara zero como par e o deixa em ESTRUTURA; sua explicação sobre atribuição a outro dia não demonstra que ocorreu nos 182 casos. (`app/ponto/management/commands/e6_oraculo.py:72`; ocorrência [nao verificado]) Pergunta específica: **“sem lei: qual fato da célula, com zero batidas atribuídas pelo oráculo, deve decidir o dono além dos predicados negativos medidos?”** [nao verificado]
- **13 / 4+ pares:** a cardinalidade par evita BATIDA pela paridade, mas não prova cumprimento nem quebra da geometria dos marcos. (`app/ponto/management/commands/e6_oraculo.py:106`; geometria dos casos [nao verificado]) Pergunta específica: **“sem lei: qual propriedade observada desses dias demonstra erro estrutural de valor, além do resíduo do dono?”** [nao verificado]
- **5 / duas pares:** mesma limitação; duas batidas do oráculo não demonstram um turno real fechado ou uma ata correta. (`app/ponto/management/commands/e6_oraculo.py:106`; turnos/atas dos casos [nao verificado]) Pergunta específica: **“sem lei: qual propriedade observada desses pares decide a causa estrutural de valor?”** [nao verificado]

Não atribuir L-084: ela requer as duas pontas a mais de três horas dos marcos, informação ausente. (`app/docs/LEIS.md:116`; horários [nao verificado]) Não atribuir L-102/L-103: elas tratam de ausência de turno pareado; `turno_nao_aberto` não demonstra ausência de turno. (`app/docs/LEIS.md:128`, `app/docs/LEIS.md:129`, `app/ponto/management/commands/e6_oraculo.py:196`)

A classe `d` atual significa: sem pergunta viva, dia fora da lista e colaborador presente na lista; não equivale automaticamente ao GRUPO-D histórico de zero chamado e zero pergunta. (`app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:230`, `app/chamados/services/vigia_fio.py:248`, `app/docs/CORTES.md:263`)

`ESTRUTURAL.md` contém os contratos de juiz único/escritor único e L-099, mas nenhuma etiqueta literal `C11`; identidade dessa referência: [nao verificado]. (`app/docs/ESTRUTURAL.md:4`, `app/docs/ESTRUTURAL.md:29`)

## 06/11 10:24 · sessao accbea19d370 · CODEX-BANCO-DIRETO · leitura · origem=Code

1. Comando: `id; uname -a; pwd`  
rc: `0`. Saída literal completa: [nao verificado]
```text
uid=1001(ronald) gid=1001(ronald) groups=1001(ronald),65534(nogroup)
Linux saas-hasner-sp 6.8.0-139-generic #139-Ubuntu SMP PREEMPT_DYNAMIC Sat Aug  1 03:52:05 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux
/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d
```

2. Comando: `python3 -c "import socket; s=socket.create_connection(('127.0.0.1',5433),3); print('tcp ok'); s.close()"`  
rc: `1`. Saída literal, últimas 5 linhas: [nao verificado]
```text
    sock = socket(af, socktype, proto)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/socket.py", line 233, in __init__
    _socket.socket.__init__(self, family, type, proto, fileno)
PermissionError: [Errno 1] Operation not permitted
```

3. Comando: `python3 -c "import socket; s=socket.create_connection(('172.18.0.9',5432),3); print('tcp ok'); s.close()"`  
rc: `1`. Saída literal, últimas 5 linhas: [nao verificado]
```text
    sock = socket(af, socktype, proto)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/socket.py", line 233, in __init__
    _socket.socket.__init__(self, family, type, proto, fileno)
PermissionError: [Errno 1] Operation not permitted
```

4. Comando: `python3 -c "import django; print(django.__version__)"`  
rc: `1`. Saída literal completa: [nao verificado]
```text
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'django'
```

5. Comando: `python3 -c "import psycopg2; print(psycopg2.__version__)" ; python3 -c "import psycopg; print(psycopg.__version__)"`  
rc: `1`. Saída literal, últimas 5 linhas: [nao verificado]
```text
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'psycopg2'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'psycopg'
```

6. Comando: `which psql pg_isready docker; ls -la /var/run/docker.sock`  
rc: `0`. Saída literal completa: [nao verificado]
```text
/usr/bin/docker
srw-rw---- 1 nobody nogroup 0 Sep 22 10:49 /var/run/docker.sock
```

7. Comando: `env | grep -i -E 'sandbox|codex' | sed 's/=.*/=<omitido>/'`  
rc: `0`. Saída literal, primeiras 5 linhas: [nao verificado]
```text
CODEX_VERSION=<omitido>
PWD=<omitido>
CODEX_SESSION_ID=<omitido>
CODEX_OBRA=<omitido>
CODEX_THREAD_ID=<omitido>
```

TCP local: não nos dois destinos testados; criação do socket bloqueada com `Operation not permitted`. [nao verificado]  
Django: não neste `python3`; módulo ausente. [nao verificado]  
Docker: cliente e socket presentes; acesso ao daemon não foi testado. [nao verificado]

## 06/11 10:23 · sessao 1b70f051c81b · CXR-TELA-LE-O-MOTOR · leitura · origem=Code

HASH LIDO = 0fca15818137d882b634bdf8261780370e63582d

| id | regra de origem (L-NNN, corte literal do aval ou arquivo:linha da regra) | entrada (model e campos, cada campo com arquivo:linha da definicao) | esperado OBSERVAVEL (o que um teste ve: retorno, linha gravada, status, texto) | mutante que tem de morder (a alteracao de codigo que este caso pega) | tipo golden/borda/adversarial | [nao verificado]
|---|---|---|---|---|---|
| CXR-01 | Aval fornecido: “os 7 dias de col841 aparecem na tela”; app/docs/BACKLOG.md:775 | `ResultadoMes.colaborador_id=841` (app/ponto/motor_calculo_v2.py:271), `mes=11` (:272), `ano=2026` (:273), `dias_cadastro_x_realidade` com 7 entradas (:289); `dia=20/10/2026`, `causa=l084_pausa` (:1671). Datas das outras seis entradas: [nao verificado]. | Os sete dias publicados aparecem na tela. A associação fornecida de 20/10 à competência 11 conflita com L-090; qual referência corrigir: regra nao decide. | Filtrar `l084_pausa`; mostrar somente dias reconhecidos pela lista antiga. | golden |
| CXR-02 | Aval fornecido: “tela x dias_cadastro_x_realidade = 0 na frota, comp 10 e 11” | Universo de ensaio não vazio, mesmo escopo; `ResultadoMes.mes=10` (app/ponto/motor_calculo_v2.py:272), `ano=2026` (:273), `colaborador_id` (:271), lista publicada (:289). Frota real dessa competência: [nao verificado]. | Na competência 10, zero dias publicados ausentes e zero dias extras na assinatura coberta pela obra. | Omitir um colaborador ou acrescentar um dia derivado pela tela. | golden |
| CXR-03 | Mesmo corte literal do aval: “comp 10 e 11” | Universo de ensaio não vazio; `ResultadoMes.mes=11` (app/ponto/motor_calculo_v2.py:272), `ano=2026` (:273), `colaborador_id` (:271), lista publicada (:289). Frota real: [nao verificado]. | Na competência 11, zero dias publicados ausentes e zero dias extras na assinatura coberta pela obra. | Implementar a leitura publicada somente para a competência 10. | golden |
| CXR-04 | Aval fornecido: “0 chamada ao motor por request”; app/docs/BACKLOG.md:775, C3 | Request da tela e request PDF; entrada do leitor `lista(empresas=...)` (app/escala/services/cadastro_realidade.py:710); publicação disponível: entidade/campos [nao verificado]. | Cada request faz zero chamadas ao motor, inclusive chamadas indiretas. | Chamar o motor dentro de `lista`, da view ou da geração do PDF. | adversarial |
| CXR-05 | L-090, app/docs/LEIS.md:123; app/ponto/janelas.py:11 | `Empresa.dia_inicio_competencia=21` (app/colaboradores/models.py:46); item publicado com `dia=20/10/2026` (app/ponto/motor_calculo_v2.py:1671). | O dia 20/10 pertence à competência 10/2026, encerrada em 20/10; não à 11. | Usar `dia >= 20` para avançar a competência. | borda |
| CXR-06 | L-090, app/docs/LEIS.md:123; app/ponto/janelas.py:11 | `Empresa.dia_inicio_competencia=21` (app/colaboradores/models.py:46); item publicado com `dia=21/10/2026` (app/ponto/motor_calculo_v2.py:1671). | O dia 21/10 pertence à competência 11/2026, iniciada em 21/10. | Recortar por mês civil ou manter o dia 21 na competência anterior. | borda |
| CXR-07 | L-085, app/docs/LEIS.md:117; L-090, app/docs/LEIS.md:123 | `TipoEscala.hora_inicio=19:00`, `hora_fim=07:00` (app/escala/models.py:50, :51); `Batida.timestamp` em 20/10 e 21/10 (app/ponto/models.py:23); item publicado com `dia=20/10` (app/ponto/motor_calculo_v2.py:1671). | A jornada publicada pertence ao dia 20. A cauda depois da meia-noite não cria outro dia de trabalho nem outra entrada por esse motivo. | Agrupar pela data civil da saída e criar também o dia 21. | borda |
| CXR-08 | Aval fornecido: “tela x dias_cadastro_x_realidade = 0”; L-040, app/docs/LEIS.md:88 | `ResultadoMes.dias_cadastro_x_realidade=[]` (app/ponto/motor_calculo_v2.py:289), publicação concluída; armazenamento da publicação: [nao verificado]. | Zero dias dessa publicação na tela. Texto de vazio, estrutura do retorno e status HTTP: regra nao decide. | Interpretar lista vazia como autorização para carregar a lista antiga. | borda |
| CXR-09 | Aval fornecido: “ler o que o motor publicou”; “PROIBIDO fallback”; L-040, app/docs/LEIS.md:88 | Duas publicações sucessivas: primeira contém um `dia`, segunda não o contém; `ResultadoMes.dias_cadastro_x_realidade` (app/ponto/motor_calculo_v2.py:289), chave `dia` (:1671). | Depois da segunda publicação concluída, o dia removido deixa de aparecer nessa assinatura. | Acrescentar publicações sem retirar os dias que desapareceram da fonte. | adversarial |
| CXR-10 | Aval fornecido: “ler o que o motor publicou”; “PROIBIDO fallback”; app/docs/BACKLOG.md:775 | Dia presente somente na lista antiga; `ResultadoMes.dias_cadastro_x_realidade=[]` (app/ponto/motor_calculo_v2.py:289). Definição do registro antigo específico: [nao verificado]. | Para a mesma acusação coberta pela obra, vence a publicação do motor: o dia não aparece por reaproveitamento da lista antiga. | Fazer união da publicação com o resultado antigo como fallback. | adversarial |
| CXR-11 | Aval fornecido: “lavra 2x = mesmo estado”; requisito fornecido: “1 linha, 0 duplicata” | Mesmos `ResultadoMes.colaborador_id`, `mes`, `ano`, lista (app/ponto/motor_calculo_v2.py:271, :272, :273, :289); duas lavras no mesmo dia. Entidade/chave da publicação: [nao verificado]. | Estado final igual ao de uma lavra; uma linha da publicação diária, zero duplicatas. Retorno e carimbo de atualização: regra nao decide. | Inserir nova linha incondicionalmente em cada lavra. | adversarial |
| CXR-12 | L-099, app/docs/LEIS.md:122; aval fornecido: “lavra 2x = mesmo estado” | Duas lavras concorrentes com os mesmos insumos; lista definida em app/ponto/motor_calculo_v2.py:289. Entidade/chave da publicação: [nao verificado]. | Ao terminarem, mesmo estado de uma lavra e nenhuma duplicata. Qual chamada espera, falha ou retorna primeiro: regra nao decide. | Separar consulta e inserção de modo que ambas gravem uma publicação equivalente. | adversarial |
| CXR-13 | Aval fornecido: “nunca motor por request”; “PROIBIDO fallback”; L-099, app/docs/LEIS.md:122 | Request durante uma lavra ainda em andamento; lista-fonte (app/ponto/motor_calculo_v2.py:289). Campos de conclusão/versão da publicação: [nao verificado]. | Regra nao decide qual versão a tela serve durante a escrita. Permanecem proibidos motor no request, fallback e decisão própria da tela. | Diante da publicação incompleta, recalcular no request. Escolha entre versões intermediárias: sem gabarito escrito. | adversarial |
| CXR-14 | Aval fornecido: “nunca motor por request”; “PROIBIDO fallback” | Lavra de hoje ainda não executada; publicação anterior ou ausência dela: entidade/campos [nao verificado]; leitor `lista(hoje=...)` (app/escala/services/cadastro_realidade.py:710). | Regra nao decide entre última publicação, vazio, aviso ou indisponibilidade. Zero chamadas ao motor e nenhum fallback. | Usar ausência da lavra diária para disparar motor ou lista antiga. | borda |
| CXR-15 | Aval fornecido: “tela x dias_cadastro_x_realidade = 0”; L-099, app/docs/LEIS.md:122 | `Colaborador.situacao=desligado`, `data_demissao` preenchida (app/colaboradores/models.py:323, :325); seu dia permanece publicado no mesmo escopo, lista (app/ponto/motor_calculo_v2.py:289). | Se o dia integra a publicação comparada, aparece na tela. Uma exclusão adicional de desligados no universo da publicação: regra nao decide. | Excluir apenas na tela todos os colaboradores atualmente desligados. | borda |
| CXR-16 | L-020, app/docs/LEIS.md:71; L-040, app/docs/LEIS.md:88 | Dois `EscalaColaborador`, com `data_inicio`, `data_fim` e `tipo_escala` diferentes (app/escala/models.py:875, :876, :874); `CelulaDia.escala_geradora` antigo e `dna` congelado (:1692, :1697); dias publicados antes/depois da troca (:1685). | Os dias publicados permanecem visíveis e o DNA congelado não muda pela publicação. Qual vínculo a nova assinatura apresenta quando a fonte não o identifica: regra nao decide. | Resolver todo o passado pelo vínculo atual e reescrever a célula durante a lavra da lista. | borda |
| CXR-17 | L-020, app/docs/LEIS.md:71 | `CelulaDia.origem=editada`, `dna`, `trabalha`, `escala_geradora` (app/escala/models.py:1691, :1697, :1686, :1692); publicar acusação desse dia. | A publicação não altera esses campos da célula. | Regenerar célula editada como efeito colateral de publicar CXR. | adversarial |
| CXR-18 | Aval fornecido: “ler o que o motor publicou”; L-040, app/docs/LEIS.md:88 | `FechamentoMensal.mes`, `ano`, `status=exportado` (app/ponto/models.py:408, :409, :410); dia presente na lista publicada (app/ponto/motor_calculo_v2.py:289). | Havendo publicação no escopo comparado, a tela a espelha. Regra nao decide uma política específica de lavra CXR em exportada. | Acrescentar somente no leitor um filtro que esconde competência exportada. | borda |
| CXR-19 | Aval fornecido, PRONTO (5); regras de causas em app/chamados/services/vigia_fio.py:523; conservação em :531 | Dia passado retido, sem pergunta/chamado vivo; `CelulaDia.colaborador`, `data` (app/escala/models.py:1681, :1685). Antes: colaborador na lista, dia fora. Depois: dia na lista, dono ESTRUTURA; formato de presença/dono (app/escala/services/cadastro_realidade.py:951). | `d` diminui 1; `e/dia_na_lista_dono_estrutura` aumenta 1; `fio_morto_alcance` não diminui por essa transição. `a+b+c+d+e` continua fechando. | Retirar o dia de `d` sem colocá-lo em `e`; considerar somente presença do colaborador. | golden |
| CXR-20 | Corte literal “Dono ESTRUTURA nunca sai”, app/chamados/services/vigia_fio.py:292; causas em :525 | Mesmo cenário de entrada em lista, mas dono lavrado CADASTRO; presença/dono por dia definidos em app/escala/services/cadastro_realidade.py:951. | O dia vai para `fio_do_admin`, conforme esse corte; não se exige `e/dia_na_lista_dono_cadastro`. As partes do universo continuam fechando (:531). | Enviar todos os donos para `e` ou retirar todos do alarme sem distinguir o dono lavrado. | adversarial |
| CXR-21 | L-040, app/docs/LEIS.md:88; app/escala/services/cadastro_realidade.py:974 | Mesma lista publicada (app/ponto/motor_calculo_v2.py:289), entregue à tela e ao PDF; parâmetro `dados` do PDF (app/escala/services/cadastro_realidade.py:973). | Tela e PDF contêm os mesmos dias e agrupamentos da lista. | Fazer o PDF consultar a lista antiga ou derivar seus próprios dias. | golden |
| O10-01 | Contrato TROCA-DE-PLANTAO, app/docs/CORTES.md:187 | Cedente com `TipoEscala.tipo_ciclo=12x36` (app/escala/models.py:117); `CelulaDia.data`, `trabalha=True` (:1685, :1686). Par completo, duas cartas e confirmação administrativa: model/campos [nao verificado]. | Dia cedido passa a folga trocada, sem desconto decorrente da cessão. | Aplicar o efeito apenas ao substituto e deixar falta/desconto no cedente. | golden |
| O10-02 | Contrato TROCA-DE-PLANTAO, app/docs/CORTES.md:187 | Cedente com `TipoEscala.tipo_ciclo=6x1` (app/escala/models.py:117); `CelulaDia.data`, `trabalha=True` (:1685, :1686). Par, cartas e confirmação: [nao verificado]. | Dia cedido é folga trocada, sem desconto decorrente da cessão. | Restringir o efeito da troca ao ciclo 12x36. | golden |
| O10-03 | Contrato TROCA-DE-PLANTAO, app/docs/CORTES.md:187 | Substituto com `CelulaDia.trabalha=False`, `data` (app/escala/models.py:1686, :1685); jornada noturna com `TipoEscala.hora_inicio`, `hora_fim` (:50, :51). Par confirmado e cartas: [nao verificado]. | Dia assumido é normal, com AN/intra conforme a regra aplicável, sem HE100 decorrente de ser originalmente folga. Quantidades de AN/intra sem cadastro completo: [nao verificado]. | Manter o dia como folga trabalhada com HE100 ou suprimir AN/intra por ser troca. | golden |
| O10-04 | “troca confirmada sem 2 anexos = vermelho”, app/docs/CORTES.md:187 | Tentativa de confirmação com zero cartas. Model/campos do par e dos anexos: [nao verificado]. | Recusa da confirmação; nenhum par válido produzido. Código, status e texto exatos: regra nao decide. | Remover a exigência dos documentos. | borda |
| O10-05 | Duas cartas, uma de cada lado, app/docs/CORTES.md:187 | Tentativa com somente carta do cedente; campos dos anexos: [nao verificado]. | Não confirma a troca. Código/status/texto: regra nao decide. | Aceitar qualquer documento como suficiente. | borda |
| O10-06 | Duas cartas, uma de cada lado, app/docs/CORTES.md:187 | Tentativa com somente carta do substituto; campos dos anexos: [nao verificado]. | Não confirma a troca. Código/status/texto: regra nao decide. | Validar apenas a carta do substituto. | borda |
| O10-07 | “troca e ATO com par”; ausência vira pedido, app/docs/CORTES.md:187 | `Ausencia.tipo=troca_de_plantao`, `data` (app/ponto/models.py:749, :750); somente um lado. Referência à contraparte: [nao verificado]. | Continua pedido; não produz o efeito de troca confirmada. | Tratar a ausência isolada como confirmação do par. | borda |
| O10-08 | L-099, app/docs/LEIS.md:122; contrato em app/docs/CORTES.md:187 | Reconfirmar o mesmo par, sem mudança de insumos; model/chave do par: [nao verificado]. | Mesmo estado e mesmos efeitos de uma confirmação; nenhum efeito duplicado. Retorno e repetição de registros de tentativa: regra nao decide. | Inserir novamente os efeitos em toda confirmação. | adversarial |
| O10-09 | L-085, app/docs/LEIS.md:117; contrato em app/docs/CORTES.md:187 | Jornada assumida em 19/10, 19:00–20/10, 07:00; `TipoEscala.hora_inicio`, `hora_fim` (app/escala/models.py:50, :51); `CelulaDia.data=19/10` (:1685). Par confirmado: [nao verificado]. | Jornada pertence ao dia 19, normal por troca, sem HE100 decorrente da folga original; a cauda não cria trabalho independente no dia 20. | Dividir a troca em dois dias pela meia-noite. | borda |
| O10-10 | L-090, app/docs/LEIS.md:123; contrato em app/docs/CORTES.md:187 | `Empresa.dia_inicio_competencia=21` (app/colaboradores/models.py:46); `CelulaDia.data` em 20/10 e 21/10 (app/escala/models.py:1685). Datas/ligação do par: [nao verificado]. | Dia 20 pertence à comp 10; dia 21 à comp 11. O contrato não restringe o par a uma única competência. Sem outra pendência, os efeitos de cada lado respeitam seu dia. | Exigir mesmo mês/competência para o par ou perder um dos lados no recorte. | borda |
| O10-11 | S5b-TROCA-NAO-CRIA-DESCONTO, app/docs/BACKLOG.md:1248 | Cenário de troca motor→calculador, classe `Motor12x36` (app/ponto/motor_calculo_v2.py:2730); `pontualidade_fora_por_lei` (app/ponto/calculador/regras.py:583); marcos e pares de pontualidade (:590). | A classe que não julgava pontualidade permanece `NAO_DECIDE`; a substituição não começa a cobrar atraso/antecipada. Esse corte trata da troca do calculador. | Habilitar pontualidade da classe durante a substituição e criar desconto novo. | adversarial |
| O10-12 | L-099, app/docs/LEIS.md:122; contrato em app/docs/CORTES.md:187 | Duas confirmações concorrentes do mesmo par e documentos; model/chave do par: [nao verificado]. | Estado final equivalente a uma confirmação, sem duplicar os efeitos. Resposta da operação perdedora: regra nao decide. | Ambas as confirmações gravarem efeitos independentes para o mesmo ato. | adversarial |
| O10-13 | “o par so vale quando o admin confirma”, app/docs/CORTES.md:187 | Pedido em andamento, um lado preparado e confirmação ainda não concluída; estado/model do par: [nao verificado]. | O pedido ainda não vale como troca confirmada. Isolamento de leitura durante a gravação: regra nao decide. | Tornar o dia cedido folga efetiva antes de existir par confirmado. | adversarial |
| O10-14 | Contrato em app/docs/CORTES.md:187; L-020, app/docs/LEIS.md:71 | Colega sem vínculo cobrindo o dia: `EscalaColaborador.data_inicio`, `data_fim`, `ativa` (app/escala/models.py:875, :876, :877); `CelulaDia.data` (:1685). | Regra nao decide se a troca é recusada, aceita ou exige regularização do vínculo. | Aceitar versus recusar não tem mutante discriminável sem regra adicional. | borda |
| O10-15 | Contrato em app/docs/CORTES.md:187 | Dia cedido com trabalho já registrado: `Batida.colaborador`, `timestamp` (app/ponto/models.py:22, :23); `CelulaDia.data` (app/escala/models.py:1685). | Regra nao decide o conflito entre trabalho já realizado e cessão do mesmo dia. | Aceitar, recusar ou reclassificar o trabalho não tem gabarito escrito para este conflito. | adversarial |
| O10-16 | L-020, app/docs/LEIS.md:71; contrato em app/docs/CORTES.md:187 | `CelulaDia.origem=editada`, `dna`, `editada_em` (app/escala/models.py:1691, :1697, :1732); nova troca solicitada para esse dia, campos do par [nao verificado]. | Regra nao decide a precedência entre edição humana anterior e novo override de troca. A soberania da célula não autoriza reescrita incidental pelo gerador. | Regerar silenciosamente a célula editada. Substituir versus preservar o override humano: sem gabarito específico. | adversarial |
| O10-17 | Contrato em app/docs/CORTES.md:187; restrição O10 em app/docs/BACKLOG.md:1781 | `FechamentoMensal.status=exportado`, `mes`, `ano` (app/ponto/models.py:410, :408, :409); tentativa de conversão em lote sem DRY/aval. Dados do par: [nao verificado]. | A conversão descrita exige DRY→aval; não efetiva esse lote sem cumprir a condição. Status/texto de recusa: regra nao decide. | Efetivar automaticamente o lote legado por a ausência já estar aprovada. | adversarial |
| O10-18 | Contrato em app/docs/CORTES.md:187; legado descrito em app/docs/BACKLOG.md:1224 | Fixture legada: `Ausencia.tipo=troca_de_plantao`, `status=aprovada`, `data_fim=NULL` (app/ponto/models.py:749, :799, :751), sem contraparte e sem duas cartas; respectivos campos [nao verificado]. | A aprovação isolada não constitui par confirmado segundo o contrato. Não produz automaticamente os efeitos da troca. Existência desse registro hoje em produção: [nao verificado]. | Promover qualquer ausência legada aprovada a troca válida, sem verificar par e documentos. | adversarial |

- **Regras que voce NAO encontrou escritas:** conciliação entre “col841 20/10, comp 11” fornecido e a janela 21→20 de L-090 (app/docs/LEIS.md:123); datas dos outros seis dias: [nao verificado].
- **Regras que voce NAO encontrou escritas:** conteúdo servido antes da primeira lavra diária, durante publicação incompleta ou quando lavras concorrentes publicam conteúdos diferentes; procurado contra o aval fornecido e o contexto C8 (app/ponto/services/vinculo_na_lista.py:111): [nao verificado].
- **Regras que voce NAO encontrou escritas:** exclusões específicas CXR por desligamento/exportação e escolha do vínculo exibido quando a publicação não o identifica; procurado em app/docs/BACKLOG.md:775, app/docs/LEIS.md:71 e :88: [nao verificado].
- **Regras que voce NAO encontrou escritas:** A2 literal de O10 no anexo raspado; o registro da obra em app/docs/BACKLOG.md:1781 não transcreve esse aval: [nao verificado].
- **Regras que voce NAO encontrou escritas:** validação O10 para colega sem vínculo, dia cedido já trabalhado, datas vazias e precedência entre troca e edição humana anterior; procurado no contrato de app/docs/CORTES.md:187: [nao verificado].
- **Regras que voce NAO encontrou escritas:** códigos HTTP, textos exatos de recusa, retorno da reconfirmação e política de isolamento/desempate das confirmações O10; procurado no contrato de app/docs/CORTES.md:187 e na idempotência de app/docs/LEIS.md:122: [nao verificado].

_2216 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
