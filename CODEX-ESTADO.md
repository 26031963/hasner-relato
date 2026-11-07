# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 09:09** · publicado em **07/11/2026 09:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 09:09 · sessao 9c3f52998fa8 · FIO-E0-V4-E2 · verificador · origem=Code

1. **Bloqueia — #3 cura os escapes anteriores, mas o selo ainda aceita leitura do relógio.** `_sem_instante` considera qualquer primeiro argumento diferente de `None` como instante; isso confunde **fuso** com **instante** (`app/chamados/tests/test_e2_tarefa_do_dia.py:756`; `app/chamados/tests/test_e2_tarefa_do_dia.py:759`).

   **Caso executado em memória contra o detector real:**
   ```python
   def f():
       return datetime.datetime.now(datetime.timezone.utc)
   ```
   O detector devolve `[]`, embora a expressão leia o relógio. Também passa `from django.utils.timezone import now as clock; clock()`: o import usa `ast.alias`, enquanto a busca cobre `Name`/`Attribute`, e `clock` não pertence à lista reconhecida (`app/chamados/tests/test_e2_tarefa_do_dia.py:745`; `app/chamados/tests/test_e2_tarefa_do_dia.py:754`; `app/chamados/tests/test_e2_tarefa_do_dia.py:767`; `app/chamados/tests/test_e2_tarefa_do_dia.py:770`).

   Portanto, **não considero #3 encerrado**. Os mutantes novos mordem suas grafias declaradas, mas não sustentam a garantia do selo (`app/chamados/tests/test_e2_tarefa_do_dia.py:739`; `app/chamados/tests/test_e2_tarefa_do_dia.py:795`).

2. **Fila — #1 e #2 estão curados nos dois predicados; igualdade e `None` não escapam.** Para os mesmos objetos, marcos e instante aware, as comparações coincidem (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:198`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/veredito_lavrado.py:206`; `app/chamados/services/raiox.py:40`; `app/chamados/services/raiox.py:42`; `app/chamados/services/raiox.py:45`).

   | Caso, sem outro motivo de invalidação | Resultado dos dois |
   |---|---|
   | `julgado_em == agora` | Fresca: a existência é inclusiva. |
   | `julgado_em > agora`, expiração `None` | Stale: o futuro é rejeitado antes da expiração. |
   | Expiração em D+1; julgamento D 22:30 local, armazenado em UTC; consulta D+1 09h | Stale: usa a data local do julgamento. |
   | Julgamento no próprio dia da expiração | Não cai novamente pelo relógio: a comparação é `<`. |
   | Marco igual ao julgamento | Não invalida: exige `>`. |

   Esses resultados foram reproduzidos com os corpos extraídos em memória, substituindo consultas e conversão de fuso; execução integrada Django: **[nao verificado]** (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/services/raiox.py:40`).

3. **Fila — `agora` naive continua sem validação na borda.** `tarefa_do_dia` rejeita somente `None`; com julgamento aware e `agora` naive, a nova comparação levanta `TypeError`. Reproduzi esse caso no corpo isolado; não há retorno conservador de stale (`app/chamados/services/tarefa_do_dia.py:349`; `app/chamados/veredito_lavrado.py:196`).

   A cópia configura `USE_TZ=True`; chamador real passando naive: **[nao verificado]**. Isso é uma lacuna de contrato/validação, sem evidência para promovê-la a regressão de produção (`app/config/settings/base.py:95`).

4. **Fila — o teste de concordância ainda compara instantes diferentes.** O delta alinha os marcos e o juízo do segundo caminho, mas `_confere` executa a varredura antes de capturar seu próprio `agora`; a varredura captura outro relógio internamente (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:56`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:58`; `app/chamados/services/raiox.py:35`).

   **Caso:** julgamento em `t`, expiração `None`, sem marcos; relógio da varredura em `t−1µs`, relógio do juízo em `t+1µs`. A primeira responde stale; o segundo, fresca. Não demonstra divergência da lei: demonstra que o selo não fixa a mesma entrada temporal (`app/chamados/services/raiox.py:40`; `app/chamados/veredito_lavrado.py:196`).

5. **Fila — os testes funcionais novos mordem os mutantes pretendidos, com lacunas específicas.** [nao verificado]

   - **Noite:** trocar somente `localdate(p.julgado_em)` por `.date()` muda o caso isolado de stale para fresca, contrariando a expectativa do teste. Ele cobre o caminho da tarefa; não exercita a varredura (`app/chamados/tests/test_e2_tarefa_do_dia.py:650`; `app/chamados/tests/test_e2_tarefa_do_dia.py:652`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/veredito_lavrado.py:202`).
   - **Futuro na tarefa:** retirar `> agora` aceita a lavra das 22h às 21h; o contracaso das 23h impede invalidar indiscriminadamente toda lavra (`app/chamados/tests/test_e2_tarefa_do_dia.py:658`; `app/chamados/tests/test_e2_tarefa_do_dia.py:660`; `app/chamados/tests/test_e2_tarefa_do_dia.py:662`).
   - **Futuro na concordância:** retirar a guarda de uma implementação quebra a igualdade; retirar das duas deixa ambas vazias, mas quebra a exigência de exatamente um stale (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:61`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:99`).

   Falta caso específico de igualdade com `agora` e de julgamento noturno na varredura; os casos novos usam horários estritamente anteriores/posteriores (`app/chamados/tests/test_e2_tarefa_do_dia.py:659`; `app/chamados/tests/test_e2_tarefa_do_dia.py:661`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`).

6. **Fila — escritores e leitores dos dois campos: concordância decisória, dívida de significado preservada.**

   | Papel | Sítio |
   |---|---|
   | Produz valores novos | `lavrar`: instante corrente e expiração calculada pela data local desse instante (`app/chamados/veredito_lavrado.py:245`; `app/chamados/veredito_lavrado.py:246`). |
   | Exceção de backfill | Captura dia/instante no início e atribui os campos às linhas selecionadas (`app/chamados/management/commands/backfill_veredito_lavrado.py:94`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`; `app/chamados/management/commands/backfill_veredito_lavrado.py:140`). |
   | Persiste/restaura | Porta de lote e restauração da foto (`app/chamados/veredito_lavrado.py:224`; `app/chamados/veredito_lavrado.py:261`; `app/chamados/veredito_lavrado.py:262`). |
   | Decide vigência | `esta_stale` e varredura, com as mesmas desigualdades (`app/chamados/veredito_lavrado.py:196`; `app/chamados/services/raiox.py:40`). |
   | Transporta/compara | Foto da lavra, comparação do backfill e foto de reversão (`app/chamados/veredito_lavrado.py:254`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`; `app/chamados/services/passivo_validado.py:83`). |

   Os saves de abertura/carimbo, troca de via e costura persistem o resultado de `lavrar`; não calculam outra expiração (`app/chamados/models.py:2572`; `app/chamados/models.py:2709`; `app/chamados/services/relavra_via.py:124`; `app/chamados/services/fio.py:850`; `app/chamados/services/fio.py:903`).

   **Caso da dívida #9:** evento remoto recebe `hoje`, porque o cálculo começa em `max(hoje, data_evento)`. Isso contradiz “determinístico por data_evento”, mas não cria laço: após lavrar, data local do julgamento iguala a expiração e não satisfaz `<`. Também torna incorreto o comentário “data futura de novo” (`app/chamados/models.py:2402`; `app/chamados/veredito_lavrado.py:115`; `app/chamados/veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:200`; `app/chamados/veredito_lavrado.py:202`).

7. **Fila — futuro stale não cria relavra em laço pelo mesmo motivo; leitores antigos continuam diferentes da tarefa.** O cron seleciona os stale uma vez e chama `lavrar` uma vez por alvo. A lavratura substitui o timestamp futuro pelo instante corrente; a rodada seguinte captura novo `agora` (`app/chamados/management/commands/lavrar_perguntas_stale.py:58`; `app/chamados/management/commands/lavrar_perguntas_stale.py:85`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:245`; `app/chamados/services/raiox.py:35`).

   **Caso com relógio normal:** borda da tarefa em `t0`, lavratura concorrente em `t1>t0`, SELECT depois. A tarefa passa a responder `veredito_sem_lavra_vigente`, sem relavrar; ela não conserva a lavra anterior para reconstruir aquele instante (`app/chamados/services/tarefa_do_dia.py:280`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:137`).

   Raiox/fio antigos leem diretamente o veredito preenchido, sem validar os dois campos. Portanto uma lavra futura `sim` ainda pode esconder pergunta materializada nesses caminhos até a manutenção; a auditoria apenas pula o stale (`app/chamados/services/raiox.py:134`; `app/chamados/services/raiox.py:183`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`). Ocorrência e volume em produção: **[nao verificado]**.

8. **Fila — não encontrei fundamento para promover #9 ou #13 a bloqueio de produção; #3 permanece bloqueador.** #13 continua incompleto: o juiz lê `date.today()`, enquanto a expiração recebe a data local do carimbo. O efeito depende da execução e do fuso efetivo, que o dado fornecido não demonstra (`app/chamados/juizes.py:563`; `app/chamados/juizes.py:661`; `app/chamados/veredito_lavrado.py:246`). Fuso efetivo, execução da suíte Django e impacto real em produção: **[nao verificado]**.

## 07/11 09:08 · sessao 23fa3c758f64 · - · - · origem=Ronald (tmux)

1. **Bloqueia — #3 permanece incompleto: argumento de fuso é confundido com instante.** `_sem_instante` só marca chamada vazia ou primeiro argumento/keyword `value` literalmente `None`; a função chamada também é excluída da busca por nomes (`app/chamados/tests/test_e2_tarefa_do_dia.py:756`; `:759`; `:760`; `:765`; `:770`).

   **Caso executado contra o helper real, extraído por AST em memória:**
   ```python
   def f():
       return datetime.datetime.now(datetime.timezone.utc)
   ```
   Resultado do detector: `[]`. Executei também a expressão: retorna instante aware, lendo efetivamente o relógio. O argumento é fuso, não instante. Portanto o selo continua aceitando leitura banal do relógio (`app/chamados/tests/test_e2_tarefa_do_dia.py:759`; `:767`).

   Os casos `timezone.localdate(timezone=datetime.timezone.utc)` e `timezone.localtime(timezone=datetime.timezone.utc)` também retornam `[]` no detector, pela mesma omissão do `value`; execução real dessas APIs Django aqui é **[nao verificado]**, pois Django não está instalado (`app/requirements.txt:13`; detector em `app/chamados/tests/test_e2_tarefa_do_dia.py:759`).

2. **Fila — os três escapes da volta 1 foram efetivamente curados, e seus mutantes mordem.** Executei em memória o `test_MORDE_mutantes` extraído por AST, usando `unittest.TestCase`; todas as assertions passaram, incluindo `localdate(None)`, `localtime(value=None)`, alias por atribuição, helper `_hoje` e chamada no módulo. Isso confirma as grafias concretamente listadas, sem corrigir o escape do item 1 (`app/chamados/tests/test_e2_tarefa_do_dia.py:795`; `:799`; `:800`; `:801`; `:802`; `:803`; `:805`).

   **Outro escape executado:** `from django.utils.timezone import now as clock; def f(): return clock()` produz `[]`: o import usa `ast.alias`, enquanto o detector só pesquisa `ast.Name`/`ast.Attribute`, e `clock` não consta de `_RELOGIO` (`app/chamados/tests/test_e2_tarefa_do_dia.py:745`; `:754`; `:770`).

3. **Fila — `test_julgado_a_noite_e_a_data_local_da_lavra` morde o mutante `.date()` no caminho da tarefa.** O teste grava julgamento local D 22:30, expiração D+1 e consulta D+1 09:00; exige ausência de lavra vigente. A carga aplica `esta_stale`, e validação sem entrada em `_lavra` produz exatamente essa razão (`app/chamados/tests/test_e2_tarefa_do_dia.py:650`; `:651`; `:652`; `app/chamados/services/tarefa_do_dia.py:298`; `:135`; `:137`).

   **Caso reproduzido em memória com a função real extraída por AST**, `julgado_em` convertido para UTC e um adaptador de `localdate` com `ZoneInfo('America/Sao_Paulo')`: atual retorna `True`; substituindo somente `timezone.localdate(p.julgado_em)` por `p.julgado_em.date()`, retorna `False`. A configuração da cópia usa São Paulo e `USE_TZ=True` (`app/chamados/veredito_lavrado.py:202`; `app/config/settings/base.py:93`; `:95`). O teste não exercita a varredura (`app/chamados/tests/test_e2_tarefa_do_dia.py:651`).

4. **Fila — os dois testes de futuro mordem a remoção da guarda `> agora`.** No teste da tarefa, expiração permanece `None` pela fixture, julgamento passa a D 22h, consulta D 21h exige ignorância e D 23h exige validade; isso impede atribuir a primeira queda à expiração e impede um mutante que invalide toda lavra (`app/chamados/tests/test_e2_tarefa_do_dia.py:623`; `:658`; `:659`; `:660`; `:661`; `:662`).

   **Caso reproduzido em memória com a função real:** às 21h, atual `True`, remoção apenas de `or p.julgado_em > agora` dá `False`; às 23h, atual `False` (`app/chamados/veredito_lavrado.py:196`).

   O teste de concordância usa julgamento uma hora futuro e expiração 30 dias futura; exige exatamente um stale. Remover a guarda de só uma implementação quebra a igualdade; remover das duas mantém igualdade vazia, mas quebra a cardinalidade exigida (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`; `:98`; `:99`; `:61`; `app/chamados/services/raiox.py:40`; `app/chamados/veredito_lavrado.py:196`).

5. **Fila — igualdade com `agora` está correta no código, mas sem mordida específica nos testes novos.** Com expiração `None`, marcos `{}` e julgamento exatamente igual a `agora`, reproduzi `esta_stale=False`; alterando `>` para `>=`, reproduzi `True`. Os casos novos usam 22h contra 21h/23h e futuro de uma hora, sem igualdade (`app/chamados/veredito_lavrado.py:196`; `:198`; `:204`; `app/chamados/tests/test_e2_tarefa_do_dia.py:658`; `:659`; `:661`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`).

Execução efetiva da suíte Django e passagem desses testes no banco: **[nao verificado]**. [nao verificado]

## 07/11 09:07 · sessao 9cfeaba4b92d · - · - · origem=Ronald (tmux)

1. **Fila — os leitores decisórios concordam para os mesmos insumos e instante aware.** `esta_stale` e a varredura usam as mesmas quatro comparações: julgamento futuro `> agora`; expiração `<= data local de agora`; julgamento local `< expiração`; marco `> julgamento`. Igualdade de julgamento com `agora` é aceita; marco igual não invalida; `janela_expira_em=None` desliga somente o motivo relógio, não a rejeição de julgamento futuro (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:198`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/veredito_lavrado.py:206`; `app/chamados/services/raiox.py:40`; `app/chamados/services/raiox.py:42`; `app/chamados/services/raiox.py:45`).

2. **Fila — censo de quem escreve e lê os dois campos.** Escrita de valores novos: `lavrar` carimba um instante e calcula a expiração pela data local desse mesmo instante; backfill carimba o instante inicial do comando e calcula pela data local capturada no início. Restauração de foto recoloca os valores anteriores; a porta de lote persiste o trio já calculado (`app/chamados/veredito_lavrado.py:245`; `app/chamados/veredito_lavrado.py:246`; `app/chamados/management/commands/backfill_veredito_lavrado.py:94`; `app/chamados/management/commands/backfill_veredito_lavrado.py:95`; `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`; `app/chamados/management/commands/backfill_veredito_lavrado.py:140`; `app/chamados/veredito_lavrado.py:224`; `app/chamados/veredito_lavrado.py:261`; `app/chamados/veredito_lavrado.py:262`).

   Os demais escritores encontrados persistem o resultado de `lavrar`: reapontamento em `abrir`, carimbo, troca de via, costura de resolvedora e rechaveamento do dia (`app/chamados/models.py:2572`; `app/chamados/models.py:2574`; `app/chamados/models.py:2709`; `app/chamados/models.py:2711`; `app/chamados/services/relavra_via.py:124`; `app/chamados/services/relavra_via.py:125`; `app/chamados/services/fio.py:850`; `app/chamados/services/fio.py:851`; `app/chamados/services/fio.py:903`; `app/chamados/services/fio.py:904`).

   Leitores decisórios diretos são `esta_stale` e a varredura; leitores adicionais transportam/comparam valores: foto da lavra, contadores do backfill e foto para reversão do passivo (`app/chamados/veredito_lavrado.py:196`; `app/chamados/services/raiox.py:31`; `app/chamados/veredito_lavrado.py:254`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`; `app/chamados/management/commands/backfill_veredito_lavrado.py:135`; `app/chamados/services/passivo_validado.py:65`; `app/chamados/services/passivo_validado.py:83`).

3. **Fila — dívida #9 permanece, sem laço demonstrado.** O modelo promete expiração determinística por evento, mas `expira_em` parte de `max(hoje,data_evento)`: evento já fora da janela recebe a data de hoje. Após lavrar, `data_local(julgado_em)==janela_expira_em`; portanto a comparação estrita `<` impede repetir a invalidação só pelo relógio. O comentário que promete “data futura de novo” está errado nesse caso, mas a guarda continua funcional (`app/chamados/models.py:2402`; `app/chamados/veredito_lavrado.py:115`; `app/chamados/veredito_lavrado.py:119`; `app/chamados/veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:200`; `app/chamados/veredito_lavrado.py:202`).

4. **Fila — `agora` naive continua sem contrato validado na borda.** A API pública rejeita apenas `None`; um julgamento aware comparado a `agora` naive falha em `p.julgado_em > agora`. A configuração usa `USE_TZ=True`; aceitação intencional de naive não está documentada nesses contratos (`app/chamados/services/tarefa_do_dia.py:349`; `app/chamados/veredito_lavrado.py:196`; `app/config/settings/base.py:95`). Chamador de produção fornecendo naive: **[nao verificado]**.

5. **Fila — #13 não está curado, mas dano de produção não é demonstrado pelo dado.** O juiz usado por `lavrar` continua consultando `date.today()`, enquanto a expiração usa a data local do instante carimbado; configurações diferentes de fuso entre relógio do SO e Django podem produzir julgamentos e expirações baseados em dias distintos (`app/chamados/juizes.py:563`; `app/chamados/juizes.py:661`; `app/chamados/veredito_lavrado.py:237`; `app/chamados/veredito_lavrado.py:246`). A cópia configura `America/Sao_Paulo`; fuso efetivo do SO em produção: **[nao verificado]** (`app/config/settings/base.py:93`).

6. **Fila — rejeitar julgamento futuro não cria repetição no cron pelo mesmo instante antigo.** O cron seleciona uma vez os stale e chama `lavrar` uma vez por alvo; `lavrar` substitui o julgamento pelo relógio corrente. A seleção da varredura lê as linhas antes de capturar `agora`, portanto, com relógio normal e valores escritos por `lavrar`, o julgamento já lido precede a borda da varredura (`app/chamados/management/commands/lavrar_perguntas_stale.py:58`; `app/chamados/management/commands/lavrar_perguntas_stale.py:85`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:245`; `app/chamados/services/raiox.py:31`; `app/chamados/services/raiox.py:35`). Quantidade efetiva de julgamentos futuros no acervo: **[nao verificado]**.

## 07/11 09:07 · sessao 7af3b96e1193 · - · - · origem=Ronald (tmux)

1. **Fila — o teste de concordância ainda usa dois instantes.** `_confere` executa a varredura antes de capturar seu próprio `agora`; a varredura lê o relógio internamente. Portanto, o comentário “MESMA borda” vale para marcos/juízo do segundo caminho, não entre varredura e juízo (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:56`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:58`; `app/chamados/services/raiox.py:35`).

   **Caso:** sem marcos e expiração `None`, fixe `julgado_em=t`; devolva `t−1µs` para o relógio da varredura e `t+1µs` para o relógio de `_confere`. A varredura devolve stale e a lei devolve fresca, embora ambos os predicados estejam corretos para seus próprios instantes (`app/chamados/services/raiox.py:40`; `app/chamados/veredito_lavrado.py:196`). O teste novo com futuro de uma hora não exercita essa fronteira (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`).

2. **Fila — a mudança não demonstra relavra em laço com relógio normal.** O cron calcula uma lista de stale, aplica teto e chama `lavrar` uma vez para cada alvo; não reavalia a mesma lista durante a rodada. `lavrar` substitui o timestamp futuro pelo `timezone.now()` corrente, e a próxima varredura captura novo `agora` (`app/chamados/management/commands/lavrar_perguntas_stale.py:58`; `app/chamados/management/commands/lavrar_perguntas_stale.py:60`; `app/chamados/management/commands/lavrar_perguntas_stale.py:85`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:245`; `app/chamados/services/raiox.py:35`).

   **Caso:** uma linha com julgamento amanhã passa a entrar na rodada de hoje; depois da gravação, sem outro insumo posterior, deixa de ser futura e não volta por esse motivo. Comportamento efetivo de linhas futuras em produção: **[nao verificado]** (`app/chamados/services/raiox.py:40`; `app/chamados/services/raiox.py:45`; `app/chamados/veredito_lavrado.py:245`).

3. **Fila — telas antigas continuam lendo o campo mesmo stale; não acionam a nova regra nem relavram.** `creditos_perguntas` usa diretamente `veredito_lavrado='sim'` no filtro SQL, e `pergunta_sem_acao_possivel` retorna o campo preenchido sem chamar `esta_stale`. A decisão de aceitar o campo até a próxima rodada está declarada nesse módulo (`app/chamados/services/raiox.py:134`; `app/chamados/services/raiox.py:183`; `app/chamados/services/raiox.py:115`; `app/chamados/services/raiox.py:117`).

   **Caso:** uma lavra futura `sim` ainda pode esconder uma pergunta materializada nessas leituras até o cron corrigir o campo. Já a tarefa exclui essa lavra e retorna `veredito_sem_lavra_vigente`; a mudança não torna todos os leitores observadores do mesmo instante (`app/chamados/services/raiox.py:134`; `app/chamados/services/raiox.py:184`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:135`; `app/chamados/services/tarefa_do_dia.py:137`).

4. **Fila — concorrência normal pode produzir ignorância transitória na tarefa, sem escrever.** A tarefa recebe um `agora` fixo, mas consulta as versões atuais das perguntas; se uma lavra for gravada depois desse `agora` e antes do SELECT, a nova guarda a exclui (`app/chamados/services/tarefa_do_dia.py:267`; `app/chamados/services/tarefa_do_dia.py:280`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/veredito_lavrado.py:245`).

   **Caso:** borda `t0`, cron grava `julgado_em=t1>t0`, SELECT da tarefa encontra esse campo. A tarefa responde `veredito_sem_lavra_vigente`, `nao_sei=True`, e não relavra. Isso segue a regra declarada de não aceitar lavra posterior; o código não conserva a lavra anterior para reconstruí-la nesse instante (`app/chamados/services/tarefa_do_dia.py:137`; `app/chamados/services/tarefa_do_dia.py:252`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/veredito_lavrado.py:245`).

5. **Fila — auditoria reconhece futuro como stale, mas apenas pula essa amostra.** Não há relavra nesse ramo: o auditor usa um `agora` para marcos e juízo e executa `continue` quando `esta_stale` retorna verdadeiro (`app/chamados/management/commands/auditar_invariantes_chamados.py:1160`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1162`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`).

   **Caso:** lavra futura deixa de ser confrontada com o juiz real nessa execução, sendo classificada como stale conhecido. O cron só trata perguntas cujo `chamado_destino` pertence ao universo vivo; não se deve interpretar o comentário “o leitor re-lavra” como garantia para toda pergunta pulada pelo auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:1154`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1166`; `app/chamados/management/commands/lavrar_perguntas_stale.py:56`; `app/chamados/management/commands/lavrar_perguntas_stale.py:57`).

6. **Fila — a divergência de fuso do juiz antigo permanece condicional.** `_dentro_da_janela_viva` lê `date.today()`, enquanto o carimbo de expiração recebe `timezone.localdate(julgado_em)`; a cópia declara `TIME_ZONE='America/Sao_Paulo'` e `USE_TZ=True`. O fuso efetivo do processo/SO e uma divergência em produção não estão demonstrados pelo dado (`app/chamados/juizes.py:563`; `app/chamados/veredito_lavrado.py:246`; `app/config/settings/base.py:93`; `app/config/settings/base.py:95`). **[nao verificado]**

## 07/11 08:59 · sessao 9681a0007b55 · FIO-E0-V4-E1 · verificador · origem=Code

**Não daria o aval.** A cópia ainda permite reentrada de duplicatas nos índices, não resolve os fatos sem chave antes da migration e não estabelece a ordem global de travas exigida. (`app/chamados/models.py:1524`, `app/chamados/models.py:2675`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:263`, `app/ponto/services/veredito_celula.py:174`)

A cópia não contém `.git`: o diff `c2833d85..d62f5b96` e a atribuição de cada problema a esse diff ficaram **[nao verificado]**. O parecer abaixo verifica o código presente. Execução da suíte, DRY real, migration aplicada, correção efetiva de col90 e ocorrência dos cenários em produção: **[nao verificado]**.

**A. Escritores, leitores e significado**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `PerguntaDisputa.batida_origem` | `_nascer` repassa o episódio; `_chave_de_identidade` preenche na inserção. O derivador lê `ultima_batida.id` e confere o colaborador da batida. (`app/chamados/models.py:2579`, `app/chamados/models.py:3026`, `app/chamados/models.py:3041`) | Dedup por episódio e índice BATIDA; agrupamento e contagem de chave ausente no backfill. (`app/chamados/models.py:2467`, `app/chamados/models.py:2472`, `app/chamados/models.py:3064`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:83`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:87`) |
| `data_evento` | Nascimento; reapontamento de pergunta sem dia; rechaveamento. (`app/chamados/models.py:2629`, `app/chamados/services/fio.py:841`, `app/chamados/services/fio.py:863`, `app/chamados/services/fio.py:913`, `app/chamados/services/fio.py:919`) | Identidade MARCO, juízes, plantio, ausência, janela, reconciliação, apresentação e backfill. Referências detalhadas abaixo. (`app/chamados/models.py:2496`, `app/chamados/services/materializacao.py:344`, `app/chamados/veredito_lavrado.py:229`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:144`) |
| `instancia_marco` | `_chave_de_identidade`; aceita valor explícito não vazio. A migration histórica também preenche pelo motivo. (`app/chamados/models.py:3023`, `app/chamados/migrations/0051_decisao_pergunta.py:27`) | `linhas_dos_fatos`, índice MARCO e backfill. (`app/chamados/models.py:2496`, `app/chamados/models.py:3059`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:82`) |
| `colaborador` | `_chave_de_identidade`, derivado da disputa e conferido somente na inserção; preenchimento histórico pela migration. (`app/chamados/models.py:3007`, `app/chamados/models.py:3017`, `app/chamados/migrations/0051_decisao_pergunta.py:25`) | Dedup, travas, índice e competência do backfill. Outros consumidores continuam lendo o colaborador pela disputa. (`app/chamados/models.py:2497`, `app/chamados/models.py:2509`, `app/chamados/models.py:3059`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:137`, `app/ponto/services/veredito_celula.py:175`) |
| `conversa` | `abrir=False`, `abrir_conversa=True`, persistidos em `_nascer`; acervo histórico recebe `False`. (`app/chamados/models.py:2529`, `app/chamados/models.py:2540`, `app/chamados/models.py:2629`, `app/chamados/migrations/0051_decisao_pergunta.py:98`) | Dedup, liberação de identidade, derivação da chave, índices e backfill. (`app/chamados/models.py:2472`, `app/chamados/models.py:2497`, `app/chamados/models.py:2511`, `app/chamados/models.py:3023`, `app/chamados/models.py:3060`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:95`) |
| `via_resolucao` | `_escrever_carimbo`, persistido por `carimbar`, `carimbar_identidade` e `descarimbar`; `.update()` de `descarimbar_em_lote`; encerramento por fato; relavra direta. (`app/chamados/models.py:2713`, `app/chamados/models.py:2762`, `app/chamados/models.py:2797`, `app/chamados/models.py:2805`, `app/chamados/models.py:2852`, `app/chamados/models.py:2862`, `app/chamados/services/relavra_via.py:123`) | Vitalidade, lastro, identidade, materialização, fábrica, reabertura e auditorias. (`app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1173`, `app/chamados/models.py:2498`, `app/chamados/services/materializacao.py:623`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/competencia_trancada.py:251`) |

Os consumidores localizados de `data_evento`, além do próprio nascimento/dedup, são:

- **Julgamento e materialização:** `app/chamados/juizes.py:266`, `app/chamados/juizes.py:363`, `app/chamados/juizes.py:623`, `app/chamados/juizes.py:659`, `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1199`, `app/chamados/juizes.py:1525`, `app/chamados/juizes.py:1965`; `app/chamados/services/materializacao.py:279`, `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:473`, `app/chamados/services/materializacao.py:517`, `app/chamados/services/materializacao.py:621`; `app/chamados/veredito_lavrado.py:229`.
- **Emissão, declaração, validação e reconciliação:** `app/chamados/services/disputa_emissao.py:392`, `app/chamados/services/disputa_emissao.py:408`, `app/chamados/services/disputa_emissao.py:821`, `app/chamados/services/disputa_emissao.py:963`, `app/chamados/services/disputa_emissao.py:1293`; `app/chamados/services/declaracao_texto.py:237`, `app/chamados/services/declaracao_texto.py:337`, `app/chamados/services/declaracao_texto.py:550`, `app/chamados/services/declaracao_texto.py:625`; `app/chamados/services/validacao.py:268`, `app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:489`, `app/chamados/services/validacao.py:645`; `app/chamados/reconciliador.py:370`, `app/chamados/reconciliador.py:1040`, `app/chamados/reconciliador.py:1072`, `app/chamados/reconciliador.py:1293`, `app/chamados/reconciliador.py:1543`.
- **Serviços e apresentação:** `app/chamados/canal.py:65`; `app/chamados/services/aviso_pendencia.py:20`; `app/chamados/services/cobranca.py:83`; `app/chamados/services/competencia_trancada.py:107`; `app/chamados/services/explicador.py:70`; `app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/historico.py:108`; `app/chamados/services/limbo_folgas.py:202`; `app/chamados/services/passivo_validado.py:113`; `app/chamados/services/pergunta_no_app.py:92`; `app/chamados/services/relavra_via.py:101`; `app/chamados/services/vigia_fio.py:816`; `app/chamados/templatetags/chamados_extras.py:31`; `app/api/views.py:1184`; `app/api/views_mensageria.py:568`; `app/colaboradores/services/calendario.py:473`; `app/folha/previa.py:65`; `app/ponto/services/dia_do_colab.py:65`; `app/ponto/services/veredito_celula.py:176`.
- **Comandos:** `app/chamados/management/commands/backfill_decisoes_pergunta.py:58`; `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/management/commands/dedup_perguntas_fato.py:54`; `app/chamados/management/commands/disparar_perguntas_competencia.py:213`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:74`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:126`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:68`; `app/chamados/management/commands/auditar_invariantes_chamados.py:148`; `app/ponto/management/commands/mapa_divergencia.py:272`; `app/ponto/management/commands/vigia_plantio_overnight.py:86`.

Outros leitores de `via_resolucao` incluem a fotografia da reconciliação/materialização, relavra, explicador, cartório, flip e comandos de auditoria/dedup/reabertura. (`app/chamados/reconciliador.py:1133`, `app/chamados/services/materializacao.py:665`, `app/chamados/services/passivo_validado.py:141`, `app/chamados/services/relavra_via.py:179`, `app/chamados/services/explicador.py:95`, `app/ponto/services/cartorio.py:1599`, `app/ponto/services/flip_auto.py:129`, `app/chamados/management/commands/auditar_invariantes_chamados.py:202`, `app/chamados/management/commands/dedup_perguntas_fato.py:74`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:129`)

Para `DecisaoPergunta.ato/ato_em/ato_por`, o escritor físico é `registrar()` por `bulk_create`; os produtores dos valores são os seguintes. (`app/chamados/models.py:3517`)

| Produtor | Valores escritos |
|---|---|
| Fechamento da disputa | `SILENCIO`, carimbo da pergunta, usuário. (`app/chamados/models.py:1878`) |
| Remoção de motivo | `RETRATACAO`, instante do ato, ator. (`app/chamados/models.py:1998`) |
| Resposta humana | `RESPOSTA`, mesmo instante da projeção, usuário. (`app/chamados/services/disputa_emissao.py:1369`) |
| Reabertura/recusa | Ato recebido, instante único, `por`. (`app/chamados/services/disputa_emissao.py:1478`) |
| Respostas automáticas | `RESPOSTA`, instante recebido, autor omitido. (`app/chamados/services/disputa_emissao.py:1519`, `app/chamados/services/disputa_emissao.py:1540`) |
| Validação/devolução | `VALIDACAO` com `validada_em`; `DEVOLUCAO` com novo instante; autor recebido. (`app/chamados/services/validacao.py:139`, `app/chamados/services/validacao.py:205`) |
| Absorção | `ABSORCAO`, instante compartilhado com carimbo, usuário. (`app/chamados/services/materializacao.py:560`) |
| Fechamento do fio | `SILENCIO`, `materializada_em`, autor. (`app/ponto/services/veredito_celula.py:182`) |
| Backfill | `SILENCIO`, `materializada_em`, `materializada_por_id`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:181`) |
| Projeção legada | `RESPOSTA`/`VALIDACAO`, carimbos originais; autor preservado somente para validação. (`app/chamados/models.py:3505`) |

Os leitores ordenam por `(ato_em, pk)`, identificam projeção legada por igualdade exata de `(pergunta, ato, ato_em)` e montam o histórico de respostas/fotos. O template exibe ato, instante e autor. Já o detector `admin_decidiu_sem_ato` aceita **qualquer** `SILENCIO/VALIDACAO` anterior, sem correspondência de instante. São critérios diferentes. (`app/chamados/models.py:3463`, `app/chamados/models.py:3501`, `app/chamados/models.py:3082`, `app/chamados/services/fio.py:231`, `app/templates/chamados/partials/modal_fio.html:208`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:125`)

O mapa de chamadas das funções pedidas é:

| Função | Chamadores encontrados |
|---|---|
| `do_fato` | `fato_livre_para`, `_nascer`, projeção/propostas de declaração e classificador da migração do dia. (`app/chamados/models.py:2513`, `app/chamados/models.py:2582`, `app/chamados/services/declaracao_texto.py:434`, `app/chamados/services/declaracao_texto.py:594`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:152`) |
| `fato_livre_para` | Reapontamento que data; rechaveamento. (`app/chamados/services/fio.py:849`, `app/chamados/services/fio.py:908`) |
| `_nascer` | `abrir` e `abrir_conversa`. (`app/chamados/models.py:2529`, `app/chamados/models.py:2540`) |
| `_chave_de_identidade` | `save`, somente em inserção. (`app/chamados/models.py:3007`) |
| `trava_do_colaborador` | Estado/releitura do Chamado, identidade/nascimento, três entradas de emissão, costura/reapontamento/rechaveamento e apply do backfill. (`app/chamados/models.py:285`, `app/chamados/models.py:686`, `app/chamados/models.py:2510`, `app/chamados/models.py:2555`, `app/chamados/services/disputa_emissao.py:510`, `app/chamados/services/disputa_emissao.py:948`, `app/chamados/services/disputa_emissao.py:1211`, `app/chamados/services/fio.py:768`, `app/chamados/services/fio.py:853`, `app/chamados/services/fio.py:901`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:163`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:175`) |
| `_gravar_estado` | `_aplicar`, reconciliar, cancelar, analisar, reabrir, encerrar e encerrar por competência. (`app/chamados/models.py:444`, `app/chamados/models.py:631`, `app/chamados/models.py:764`, `app/chamados/models.py:782`, `app/chamados/models.py:1408`, `app/chamados/models.py:1569`, `app/chamados/models.py:1589`) |
| `_recarregar_estado_se_mudou` | Decisão de fechamento, cancelar e os dois encerramentos. (`app/chamados/models.py:649`, `app/chamados/models.py:758`, `app/chamados/models.py:1563`, `app/chamados/models.py:1579`) |
| `reapontar_resolvedora` | Gate do Chamado; reemissão/reata; declaração; limbo; migração de resolvedoras; reversão do comando de reata. (`app/chamados/models.py:431`, `app/chamados/reconciliador.py:944`, `app/chamados/reconciliador.py:1628`, `app/chamados/reconciliador.py:1641`, `app/chamados/services/declaracao_texto.py:330`, `app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/limbo_folgas.py:276`, `app/chamados/services/fio.py:1023`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:122`) |
| `rechavear_data_evento` | Apply e reversão da migração do dia. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:284`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:336`) |
| `_costura_nao_mata_o_fio` | Dedup do nascimento e reapontamento. (`app/chamados/models.py:2599`, `app/chamados/services/fio.py:856`) |
| `fechar_o_fio_do_dia` | Veredito da célula, declaração de folga e aceite de folga já declarada. (`app/ponto/services/veredito_celula.py:332`, `app/ponto/services/declarar_dia.py:204`, `app/chamados/services/validacao.py:289`) |
| Backfill `handle/aplicar` | `handle` mede/classifica e chama `aplicar`; este retrata pela porta do fio e registra decisões pela porta da sequência. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:199`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:263`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:167`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:181`) |

**Não pressupõem todos o mesmo significado.** Os desvios encontrados são:

1. **BATIDA muda de unidade entre portas.** `_nascer` deduplica passando o episódio; `fato_livre_para` omite `p.batida_origem_id` e cai em motivo/dia/instância. Dois episódios distintos permitidos pelo índice no mesmo dia podem bloquear um rechaveamento legítimo. (`app/chamados/models.py:2582`, `app/chamados/models.py:2513`, `app/chamados/models.py:2496`, `app/chamados/models.py:3064`)

2. **Dois outros consumidores conservam a identidade antiga.** `migrar_resolvedoras_vivas` considera duplicação apenas por motivo/dia e pode retratar episódios diferentes. O gerador também barra outra pergunta BATIDA pelo par disputa/motivo antes de chegar ao novo dedup. (`app/chamados/services/fio.py:1013`, `app/chamados/services/fio.py:1019`, `app/chamados/services/disputa_emissao.py:26`, `app/chamados/services/disputa_emissao.py:321`)

3. **Instância explícita e instância consultada podem divergir.** A inserção aceita instância fornecida não vazia, mas `linhas_dos_fatos` consulta a instância derivada do motivo. Nenhum emissor atual passando instância alternativa foi localizado; ocorrência produtiva **[nao verificado]**. (`app/chamados/models.py:3023`, `app/chamados/models.py:2496`)

4. **`data_evento` tem dois significados declarados:** dia do turno para fato; dia civil do nascimento para conversa. O tipo é `NewType`, sem conversão temporal. Leitores como a lavra da janela não discriminam `conversa`. (`app/chamados/models.py:2523`, `app/chamados/models.py:2534`, `app/chamados/models.py:2540`, `app/core/dia_do_turno.py:13`, `app/chamados/veredito_lavrado.py:229`)

5. **As bordas temporais localizadas:** a identidade compara datas exatas; a janela viva inclui ambas as pontas; a expiração ocorre quando `janela_expira_em <= hoje`. A atribuição da batida ao dia delega ao juiz do turno, inclusive para o noturno. (`app/chamados/models.py:2496`, `app/chamados/juizes.py:589`, `app/chamados/veredito_lavrado.py:116`, `app/chamados/veredito_lavrado.py:182`, `app/ponto/turnos.py:2917`)

6. **`SILENCIO` também recebe pergunta respondida.** O fechamento do fio percorre `q_vivas`, que inclui resposta não vazia, e grava `SILENCIO`. No fechamento da disputa, resposta sem veredito é recusada antes desse ato. O mesmo enum cobre situações diferentes. (`app/ponto/services/veredito_celula.py:174`, `app/ponto/services/veredito_celula.py:182`, `app/chamados/models.py:2935`, `app/chamados/models.py:1848`, `app/chamados/models.py:3439`)

A completude absoluta diante de SQL construído dinamicamente e aliases não atribuíveis ficou **[nao verificado]**.

**B. Há outros caminhos que podem violar os índices**

- **Ressurreição de duplicata/retratada — bloqueador.** O universo seleciona materializadas sem texto, sem excluir `duplicata/motivo_retratado`; limpa a via numa cópia para consultar o C6. Havendo substituta no fato e célula que ainda acusa, a antiga pode ser escolhida. O `.update(via_resolucao='')` reintroduz essa linha no índice, sem consultar identidade. Caminhos: reabrir, cartório e desarquivar. (`app/chamados/models.py:1449`, `app/chamados/models.py:1470`, `app/chamados/models.py:1473`, `app/chamados/models.py:1524`, `app/chamados/models.py:2852`, `app/chamados/models.py:1417`, `app/ponto/services/cartorio.py:1622`, `app/chamados/views_arquivar.py:86`)

- **Fechamento administrativo de duplicata respondida — bloqueador.** A resposta pode manter a duplicata viva; `fechar_o_fio_do_dia` a seleciona e `fechar_por_admin` troca `duplicata` por `admin_decidiu`. Essa via participa do índice, enquanto a canônica continua presente. A gravação é `save(update_fields=...)`, sem `fato_livre_para` nem dedup de nascimento. Também há chamadas diretas no aceite de folga. (`app/chamados/juizes.py:1152`, `app/chamados/juizes.py:1164`, `app/chamados/models.py:2935`, `app/ponto/services/veredito_celula.py:177`, `app/chamados/models.py:2675`, `app/chamados/models.py:2797`, `app/chamados/models.py:3061`, `app/chamados/services/validacao.py:282`, `app/chamados/services/validacao.py:317`)

Não localizei outro nascimento produtivo fora do `cls.objects.create` de `_nascer`. O `bulk_update` encontrado em `veredito_lavrado` grava campos da lavra; a relavra direta de via restringe sua entrada a vias derivadas. Isso não elimina os dois caminhos de reentrada acima. (`app/chamados/models.py:2629`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:125`)

**C. A ordem de trava “SEMPRE” continua falsa**

| Caminho | Ordem encontrada |
|---|---|
| `DisputaSupervisao.fechar` | Atomic → UPDATE Pergunta nas exceções → UPDATE Disputa → signal → reconciliar pai → Colaborador. Mesmo sem perguntas, permanece Disputa antes do Colaborador quando o pai está vivo. (`app/chamados/models.py:1866`, `app/chamados/models.py:1875`, `app/chamados/models.py:1890`, `app/chamados/signals.py:41`, `app/chamados/models.py:649`, `app/chamados/models.py:686`) |
| Aprovar justificativa DP | Atomic → Chamado `FOR UPDATE` → encerrar origem → retratar/reconciliar → Colaborador. Materializar saída DP usa a mesma entrada. (`app/ponto/services/justificativa.py:125`, `app/ponto/services/justificativa.py:114`, `app/ponto/services/justificativa.py:202`, `app/ponto/services/justificativa.py:42`, `app/chamados/reconciliador.py:522`, `app/chamados/models.py:686`, `app/ponto/services/justificativa.py:327`, `app/ponto/services/justificativa.py:393`) |
| Validar folga já declarada | Atomic → `_carimbar_validacao` → UPDATE Pergunta → fechar fio → reconciliar Chamado → Colaborador. Nova folga também grava a validação primeiro. (`app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:137`, `app/chamados/services/validacao.py:289`, `app/ponto/services/veredito_celula.py:190`, `app/chamados/models.py:686`, `app/chamados/services/validacao.py:307`) |
| `fechar_o_fio_do_dia` | Grava Perguntas antes de reconciliar Chamados, cuja porta adquire Colaborador. A função não estabelece a trava inicial; seus chamadores incluem atomics externos. (`app/ponto/services/veredito_celula.py:174`, `app/ponto/services/veredito_celula.py:189`, `app/chamados/models.py:686`, `app/ponto/services/veredito_celula.py:211`, `app/ponto/services/declarar_dia.py:188`) |
| Reatar beco ao limbo | Atomic → pai Chamado `FOR UPDATE` → reatar perguntas → reapontar → Colaborador. (`app/chamados/managem

[truncado em 20000 de 32847 caracteres]

## 07/11 08:56 · sessao 3deb80690dc1 · V2-I2 · sonda · origem=Code

**A volta 3 identifica o disparador da regeneração e mostra lavras posteriores a ela. Ainda não recupera a ata consumida em 29/10 nem prova qual ato mudou o previsto para 420.** [nao verificado]

1. **Ata na lavratura e ata atual**

Hoje, a sonda mostra **`minutos_previstos=420`, `tipo_dia='trabalho'`**, com quatro marcos 15/20/21/23, todos apagados; a grade reproduz 420/trabalho. [nao verificado]

Em **29/10 10:26**, está demonstrado no material fornecido **DP previsto=0**; **ata prevista=0 e `tipo_dia` histórico continuam [nao verificado]**. DP zero não permite reconstruí-los: o mapa aceita somente `trabalho`/`ausencia`, e uma linha criada por outra rubrica ou furo começa com zero e pode permanecer sem contribuição do mapa de previsto. (`app/ponto/services/dia_pago.py:167`, `app/ponto/services/dia_pago.py:279`, `app/ponto/services/dia_pago.py:288`, `app/ponto/services/dia_pago.py:387`)

O escritor é esta cadeia: **`ata_do_dia` monta os campos → `lavrar_veredito` substitui a ata diferente → `carimbar` persiste a lavra com os carimbos**. (`app/ponto/services/cartorio.py:704`, `app/ponto/services/cartorio.py:1800`, `app/ponto/portas/celula.py:1027`, `app/ponto/portas/celula.py:1060`)

**02/11 é compatível com a reescrita, mas não está provado como o instante de 0→420.** `julgada_em` pode avançar por outros campos do julgamento; a auditoria apresentada guarda veredito/via/protestos/status, sem ata anterior/posterior. (`app/ponto/portas/celula.py:1055`, `app/ponto/portas/celula.py:982`, `app/ponto/portas/celula.py:1037`)

2. **Ata velha sem rejulgar, ou julgamento com célula nova?**

O raspado registra, nesta ordem: regeneração em **23/10 13:04:42**, lavra **`cobrado→cobrado` às 13:04:43**, lavra **`furo→concorde` em 29/10 10:12:50**, DP às **10:26**, julgamento atual em **02/11**. Portanto, há evidência de lavras após a regeneração e antes do DP. [nao verificado]

Isso **contraria a explicação simples “foi julgada antes de regenerar e nunca rejulgada”**. Pela cópia, ata diferente provoca nova lavra mesmo mantendo o veredito; repetir `cobrado` não significa ausência de julgamento. (`app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:1013`, `app/ponto/portas/celula.py:1027`; [nao verificado])

Mas **“julgou previsto zero com a célula nova” ainda não está demonstrado**: faltam os campos da ata dessas lavras e a grade que os produziu. Os eventos não mostram quais chaves mudaram. (`app/ponto/services/cartorio.py:704`, `app/ponto/portas/celula.py:1037`; [nao verificado])

3. **Quem disparou a regeneração de 23/10**

O ato registrado é **`executar_vinculo`, `usuario=846`, `ator=None`, no vínculo 1449**, com regeneração de 61 células entre 21/09 e 20/11 às **13:04:42.483907**; em seguida, há o registro de vínculo **ASG CAB TARD** pelo mesmo usuário. Esse é o ato correspondente ao carimbo da célula fornecido. Identidade nominal do usuário: **[nao verificado]**.

A associação tem suporte no código: `executar_vinculo` passa `request.user` como autor e esse motivo à regeneração; a porta registra o autor na auditoria do vínculo. (`app/colaboradores/services/vinculo.py:320`, `app/ponto/portas/celula.py:334`)

4. **Dono L-099 ou “relógio da ata”?**

**Nenhum dos dois está fechado como explicação causal deste +420.** A cronologia comprova julgamento atual posterior ao FM/DP; sozinha, não comprova que esse julgamento alterou o previsto. [nao verificado]

“Relógio da ata” não é uma saída de `dono_da_divergencia`: o juiz retorna **CADASTRO, BATIDA ou ESTRUTURA**, sem receber datas de FM/DP/julgamento. Faltam seus insumos efetivos para classificar este caso. (`app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:172`)

A analogia com APPLY 11 também não fecha a causa: o trecho citado explica **realizado de dia em curso versus ata D+1**, e declara que o **previsto cobre a competência inteira**; não certifica este previsto de 27/09. (`app/ponto/management/commands/lavrar_dias_pagos.py:16`, `app/ponto/management/commands/lavrar_dias_pagos.py:23`)

**Falta exatamente:**

- **Ata e grade efetivamente consumidas em 29/10 10:26**, com previsto, tipo, presença da chave no mapa de previsto e carimbo do julgamento anterior. [nao verificado]
- **Antes/depois das atas nas lavras de 23/10, 29/10 e 02/11**, para localizar a mudança; para explicar eventual zero, a grade produzida naquele julgamento, seus insumos históricos e a versão executada. [nao verificado]
- **`horas_do_dia` e os seis indicadores efetivos do juiz**: folga indefinida, os dois de ausência de vínculo, batida fora de turno, fase deslocada e Cadastro × Realidade. (`app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:172`)

## 07/11 08:55 · sessao 5824c396a471 · - · - · origem=Ronald (tmux)

- **Escritor da ATA:** `ata_do_dia` copia `tipo_dia` e `minutos_previstos` da grade (`app/ponto/services/cartorio.py:704–706`); o julgamento entrega essa ATA à lavra (`app/ponto/services/cartorio.py:1800–1807`); a porta substitui `cel.ata` quando diferente (`app/ponto/portas/celula.py:995`, `1027–1028`); `carimbar` salva a lavra junto de `julgada_em` e `insumos_em` (`app/ponto/services/cartorio.py:1821–1823`; `app/ponto/portas/celula.py:1060–1067`).

- **`julgada_em=02/11` não prova que previsto mudou nesse julgamento.** Esse relógio avança se a impressão, ausência, entrada/saída ou qualquer campo lavrado mudar; pode andar sem mudar `minutos_previstos`. (`app/ponto/portas/celula.py:1055–1067`)

- **A auditoria fornecida não preserva ATA anterior/posterior.** O evento guarda veredito, via, protestos e status; não guarda `ata`, `minutos_previstos`, `tipo_dia`, `impressao` ou julgamento anterior. Assim não reconstitui o previsto/tipo consumido na lavratura do DP nem identifica precisamente o ato de 0→420. (`app/ponto/portas/celula.py:982–983`, `1037–1040`)

- **Há evidência de julgamento após a regeneração**, não apenas julgamento anterior esquecido: o raspado tem `23/10 13:04:43 cobrado→cobrado` e `29/10 10:12:50 furo→concorde`, antes do DP de 10:26. [nao verificado] Na cópia, ATA diferente com o mesmo veredito provoca nova lavra; quando ATA/veredito/via/protestos são idênticos, só mudança de status emite descrição própria “status do dia…”, não “veredito cobrado→cobrado”. (`app/ponto/portas/celula.py:995`, `1013–1025`, `1027–1041`) Logo o evento repetindo `cobrado` é compatível com mudança da ATA, sem revelar quais chaves mudaram; aplicar esse detalhe ao binário de outubro exige comprovar versão então executada. [nao verificado]

- **A cópia atual força julgamento dos dias passados tocados antes do dinheiro:** regeneração chama `_o_chao_do_dia_mudou` (`app/ponto/portas/celula.py:352–356`), que chama `julgar_celula(...forcar=True)` (`app/ponto/portas/celula.py:422–440`) antes de `recalcular_por_evento` (`app/ponto/portas/celula.py:445–458`). O próprio comentário documenta correção da ordem em 23/10, sem certificar horário de deploy nem execução desse fluxo para col270. (`app/ponto/portas/celula.py:411–421`)

- **O ato `lavrar_status_competencia` de 31/10 não explica mudança de previsto/tipo:** o caminho só passa status, sem ATA, e não move o veredito quando apenas status muda. (`app/ponto/services/cartorio.py:875–891`; `app/ponto/portas/celula.py:1013–1025`)

- **`carimbar_precedencia` também não explica troca de previsto/tipo:** altera exclusivamente a chave `precedencia` da ATA existente. (`app/ponto/portas/celula.py:1075–1082`)

Falta exatamente uma fotografia histórica/versionada da ATA de 33835 em **29/10 antes do DP**, incluindo `minutos_previstos`, `tipo_dia`, `julgada_em` e `impressao`; fotografias antes/depois dos julgamentos relevantes para datar 0→420; e os insumos/grade produzidos naquele julgamento, com versão de código então executada, para decidir por que um julgamento posterior à regeneração poderia ter produzido zero. [nao verificado]

## 07/11 08:55 · sessao ff601d32099a · - · - · origem=Ronald (tmux)

O “relógio da ata” **não é uma saída do juiz L-099**: `dono_da_divergencia` só retorna `CADASTRO`, `BATIDA` ou `ESTRUTURA`, usando flags e paridade; não recebe FM, DP, ata ou `julgada_em`. (`app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:98`)

- O juiz exige `horas_do_dia`, `em_cadastro_x_realidade`, `folga_nao_definida`, `sem_vinculo_no_dia`, `sem_vinculo_em_dia_de_trabalho`, `batida_fora_de_turno` e `fase_deslocada`; sua alimentação oficial está em `InsumosDoDono.dono`. (`app/ponto/management/commands/e6_oraculo.py:172`)
- `orfas=[]`, quatro marcos ausentes e realizado zero na ata atual não fornecem esse conjunto. Dono concreto L-099: **[nao verificado]**.
- Se todas as flags forem falsas e `horas_do_dia=[]`, o resultado será `ESTRUTURA`; é conclusão condicional, não classificação comprovada deste caso. (`app/ponto/management/commands/e6_oraculo.py:98`)

A ordem informada — FM/DP em 29/10 e julgamento atual em 02/11 — demonstra julgamento atual posterior ao gravado, **mas não demonstra por que o previsto era zero nem qual alteração mudou o número**. [nao verificado]

A explicação de `lavrar_dias_pagos.py:16–26` trata especificamente de **realizado de dia em voo**: motor já lê batidas do dia, ata só nasce em D+1; o mesmo texto diz que previsto cobre a competência inteira. Portanto, esse comentário sustenta a possibilidade geral de fontes com relógios diferentes; não certifica que este **previsto de 27/09** seja a mesma situação normal documentada. (`app/ponto/management/commands/lavrar_dias_pagos.py:16`, `app/ponto/management/commands/lavrar_dias_pagos.py:23`)

**Cuidado com o pressuposto ATA histórica = 0:** DP previsto zero **não prova** ata prevista zero nem `tipo_dia` histórico:

1. A grade prefere `ata.tipo_dia` e preserva `ata.minutos_previstos=0`, chamando a função somente quando previsto é `None`. (`app/escala/services/leitor_celula.py:380`)
2. O mapa de previsto só inclui `tipo_dia in ('trabalho','ausencia')`. (`app/ponto/services/dia_pago.py:387`)
3. A linha DP começa com todos os campos em zero; o mapa de previsto só acrescenta valores quando tem a chave do dia. Uma linha criada por outra rubrica/furo pode manter previsto zero mesmo se o tipo a excluiu do mapa. (`app/ponto/services/dia_pago.py:167`, `app/ponto/services/dia_pago.py:279`, `app/ponto/services/dia_pago.py:288`)

Falta, portanto, **ATA/grade efetivamente consumida em 29/10**, com previsto e tipo, e seu carimbo de julgamento; versões/intervenções até 02/11; e os insumos efetivos do juiz para fechar L-099. A sonda fornecida não apresenta esses estados históricos. [nao verificado]

## 07/11 08:55 · sessao 20b7ebf0bf10 · FIO-E0-V4-E2 · verificador · origem=Code

**REPROVADO para o conjunto pedido.** B3 e B4 foram corrigidos, mas permanecem falhas no relógio local, na aceitação de lavra futura e na cobertura do selo de relógio (`app/chamados/services/tarefa_do_dia.py:315`; `app/chamados/tests/test_e2_tarefa_do_dia.py:507`; `app/chamados/veredito_lavrado.py:201`; `app/chamados/tests/test_e2_tarefa_do_dia.py:735`).

1. **Bloqueia — 0514(2): a correção de fuso ficou pela metade.** `esta_stale` converte `agora` para data local, mas compara a expiração com `p.julgado_em.date()`, sem conversão local; a varredura repete essa mistura (`app/chamados/veredito_lavrado.py:198`; `app/chamados/veredito_lavrado.py:201`; `app/chamados/services/raiox.py:42`).

   **Caso reproduzido em memória:** expiração em **21/11/2026**, julgamento em **21/11 01:30 UTC = 20/11 22:30 São Paulo**, consulta em **21/11 09:00 São Paulo**, sem marcos. A função retorna `False`: mantém vigente uma lavra anterior à virada local. O novo selo testa `agora` em UTC, mas não essa virada de `julgado_em` (`app/chamados/veredito_lavrado.py:201`; `app/chamados/tests/test_e2_tarefa_do_dia.py:623`; `app/chamados/tests/test_e2_tarefa_do_dia.py:637`).

2. **Bloqueia — 0514(2): limitar os marcos não impede ler uma lavra futura.** `esta_stale` não exige `julgado_em <= agora`; a carga aceita qualquer `sim`/`nao` que passe nessa função (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:203`; `app/chamados/services/tarefa_do_dia.py:297`).

   **Caso reproduzido em memória:** `veredito_lavrado='nao'`, `julgado_em=2036`, `agora=2026`, expiração `None`, marcos `{}` ⇒ `esta_stale=False`. A fixture geral continua chamando de “fresco” um julgamento em `D+3650`, portanto legitima esse futuro em vez de testar existência da lavra no instante consultado (`app/chamados/veredito_lavrado.py:207`; `app/chamados/tests/test_e2_tarefa_do_dia.py:127`).

3. **Bloqueia — o selo “relógio só na borda” deixa passar leituras reais.** O detector reconhece somente cinco nomes e somente chamadas sem argumentos nem keywords (`app/chamados/tests/test_e2_tarefa_do_dia.py:724`; `app/chamados/tests/test_e2_tarefa_do_dia.py:735`).

   **Casos executados contra o helper AST, em memória:** `timezone.now()` foi detectado; `timezone.localdate(None)` e `clock = timezone.now; clock()` devolveram `[]`. Também ficam fora os relógios em helpers externos às três funções examinadas. Os mutantes existentes mordem as grafias simples declaradas, mas não sustentam a garantia ampla do nome do selo (`app/chamados/tests/test_e2_tarefa_do_dia.py:743`; `app/chamados/tests/test_e2_tarefa_do_dia.py:755`; `app/chamados/tests/test_e2_tarefa_do_dia.py:760`).

4. **Bloqueia a certificação — 0514(3), universo completo, permanece [nao verificado].** A grade percorre todas as datas de `[ini,fim]`, inclusive produzindo `sem_celula`; entretanto a carga recebe de fora os colaboradores e o horizonte. Isso não demonstra que a sombra selecionou **toda célula do horizonte** (`app/escala/services/leitor_celula.py:375`; `app/escala/services/leitor_celula.py:394`; `app/chamados/services/tarefa_do_dia.py:340`).

   **Caso que falta:** confronto entre o universo de células do horizonte e as chaves efetivamente avaliadas pela sombra. O dado fornecido não contém essa execução/certificação: **[nao verificado]**.

5. **Fila — B3 realizado no caminho alterado.** A carga recarrega o cadastro e alimenta `grade_da_celula`; `_tarefa_pura` recebe o dia projetado e a isenção, encerra `fora_vinculo` e isento sem ações, e pergunta “dia sem trabalho” ao juiz existente por meio do adaptador (`app/chamados/services/tarefa_do_dia.py:276`; `app/chamados/services/tarefa_do_dia.py:315`; `app/chamados/services/tarefa_do_dia.py:171`; `app/chamados/services/tarefa_do_dia.py:174`; `app/ponto/supra_juiz.py:69`).

   **Caso:** Caso13 confronta demissão anterior, admissão posterior, isenção, demissão no próprio dia com/sem fato e vínculo vigente. Esses casos distinguem o B3 antigo e também impedem uma implementação que simplesmente silencie tudo (`app/chamados/tests/test_e2_tarefa_do_dia.py:555`; `app/chamados/tests/test_e2_tarefa_do_dia.py:562`; `app/chamados/tests/test_e2_tarefa_do_dia.py:569`; `app/chamados/tests/test_e2_tarefa_do_dia.py:576`; `app/chamados/tests/test_e2_tarefa_do_dia.py:584`; `app/chamados/tests/test_e2_tarefa_do_dia.py:591`).

6. **Fila — censo de `acesa_lavrada`: todas as formas foram contempladas, mas “acesa” não significa necessariamente batida ocorrida.**

   | Valor/caminho | Quem escreve | Quem lê e caso |
   |---|---|---|
   | `acesa` original | O cartório produz `True`, `None` ou `False`; também produz lâmpadas pelo casamento posicional e pelo ramo sem DNA (`app/ponto/services/cartorio.py:638`; `:642`; `:644`; `:677`; `:689`). | `ata_do_dia` incorpora a lista à ata (`app/ponto/services/cartorio.py:702`). |
   | Dispensa | Força `acesa=True`, `luz=None`, com motivo de dispensa (`app/ponto/services/cartorio.py:774`). | **Caso:** ausência dispensada acende sem ninguém bater; a tarefa cala a cobrança pelo `True` (`app/ponto/services/cartorio.py:752`; `app/chamados/services/tarefa_do_dia.py:211`). |
   | Marco pendente | O teto transforma `False` em `None` (`app/ponto/services/cartorio.py:881`). | A tarefa conserva ignorância e marca `nao_sei` (`app/chamados/services/tarefa_do_dia.py:214`). |
   | Forma com lâmpadas | `_celulas_da_ata` publica literalmente `l_.get('acesa')`; chave ausente também vira `None` (`app/escala/services/leitor_celula.py:137`). | `_lampadas_do_dia` transporta o valor para as instâncias (`app/chamados/services/tarefa_do_dia.py:81`; `:95`). |
   | Forma agregada | Cada célula agregada recebe `acesa_lavrada=None`, com tipo/hora desconhecidos (`app/escala/services/leitor_celula.py:171`). | Sem instâncias identificáveis, dia de trabalho resulta em `ata_sem_lampadas`, `nao_sei=True` (`app/chamados/services/tarefa_do_dia.py:185`). |

   `acesa_lavrada` é projeção em memória; o overlay de isenção pode mudar `missing` para `False` sem mudar esse valor original. A tarefa usa o gate de isento antes de cobrar, portanto essa diferença não cria cobrança (`app/escala/services/leitor_celula.py:137`; `app/escala/services/leitor_celula.py:441`; `app/escala/utils.py:858`; `app/chamados/services/tarefa_do_dia.py:174`).

7. **Fila — censo de `fora_vinculo` e isenção: significados distintos, coerentes no caminho novo.**

   - **`tipo_dia='fora_vinculo'`:** escrito pela grade a partir de admissão, demissão e fato lavrado; lido pela tarefa, declaração, materialização e fechamento. Admissão é inclusiva; antes dela o fato pode preservar o dia. Na demissão, o próprio dia exige fato; depois da demissão, fato não vence a exclusão. O fato considerado é minuto realizado ou contagem lavrada, não mera existência de uma lâmpada `True` (`app/escala/services/leitor_celula.py:385`; `:389`; `:393`; `app/chamados/services/tarefa_do_dia.py:171`; `app/chamados/services/declaracao_texto.py:112`; `app/chamados/services/materializacao.py:239`; `app/ponto/services/fechamento.py:1036`).
   - **Isenção:** a fonte é a flag cadastral; o juiz `isento_ponto` lê somente essa flag. A tela Passe Livre grava pela porta de cadastro, e a migration histórica semeou a flag. A grade publica `isento_ponto`; a carga o passa como `isento` à tarefa (`app/colaboradores/models.py:294`; `app/core/constants.py:13`; `app/colaboradores/views.py:175`; `app/colaboradores/services/colaborador.py:55`; `app/colaboradores/migrations/0043_isento_ponto.py:13`; `app/escala/services/leitor_celula.py:445`; `app/chamados/services/tarefa_do_dia.py:317`).

   **Caso de significado:** `fora_vinculo` aqui trata da vida cadastral do empregado; escolher a escala do dia é outra pergunta, cuja autoridade registrada é `vinculo_do_dia`. O novo caminho não substitui essa autoridade por uma escolha própria (`app/escala/services/leitor_celula.py:389`; `app/docs/JUIZES.md:110`; `app/escala/utils.py:2194`).

8. **Fila — censo dos marcos: o limite foi aplicado a todos os campos enumerados.** São instantes de registro/alteração, agregados por colaborador; não são o dia civil nem o dia do turno da ocorrência (`app/chamados/veredito_lavrado.py:151`; `:165`; `:176`).

   | Marco | Escritor/origem | Leitor |
   |---|---|---|
   | `Batida.criado_em` | `auto_now_add`; criação pela porta de batida (`app/ponto/models.py:79`; `app/ponto/registro_batida.py:157`). | `app/chamados/veredito_lavrado.py:151` |
   | `Batida.retratada_em` | Retratação com `timezone.now()`; desfazer pode limpar (`app/ponto/registro_batida.py:274`; `app/ponto/services/retratar_humano.py:207`). | `app/chamados/veredito_lavrado.py:154` |
   | `CelulaDia.gerada_em` | `auto_now_add` (`app/escala/models.py:1702`). | `app/chamados/veredito_lavrado.py:167` |
   | `regenerada_em` | Porta de célula, regeneração/desfazer (`app/ponto/portas/celula.py:274`; `:792`). | `app/chamados/veredito_lavrado.py:167` |
   | `editada_em` | Edição humana pela porta (`app/ponto/portas/celula.py:601`; `:607`). | `app/chamados/veredito_lavrado.py:168` |
   | `veredito_em` | Porta, com `agora` recebido ou relógio interno (`app/ponto/portas/celula.py:1032`). | `app/chamados/veredito_lavrado.py:167` |
   | `julgada_em` | `carimbar`, com `agora` obrigatório recebido (`app/ponto/portas/celula.py:1047`; `:1060`). | `app/chamados/veredito_lavrado.py:168` |
   | `materializada_em` | `_escrever_carimbo`; a porta recebe `quando` ou lê o relógio; descarimbo também limpa (`app/chamados/models.py:2658`; `:2683`; `:2802`). | `app/chamados/veredito_lavrado.py:176` |
   | `respondida_em` | Resposta com relógio do ato; resposta automática/trilha com instante recebido; carimbar também pode preencher, e desfazer restaura (`app/chamados/services/disputa_emissao.py:1340`; `:1346`; `:1516`; `:1537`; `app/chamados/models.py:2691`; `:2722`). | `app/chamados/veredito_lavrado.py:180` |

   **Caso de fronteira:** com `ate`, cada marco entra por `<= ate`, inclusivo; só invalida uma lavra se for estritamente `> julgado_em`. Sem `ate`, entram todos os valores atuais. Isso filtra os campos atuais, sem reconstruir versões sobrescritas; não constitui uma fotografia histórica completa (`app/chamados/veredito_lavrado.py:137`; `:140`; `:205`).

9. **Fila — `janela_expira_em` continua com dois significados.** O modelo o descreve como data determinística por `data_evento`, mas `expira_em` começa em `max(hoje, data_evento)`: para evento já expirado, devolve o dia da consulta/lavra (`app/chamados/models.py:2399`; `app/chamados/veredito_lavrado.py:115`).

   **Caso reproduzido em memória:** evento **21/05/2026** ⇒ expiração **07/11/2026** quando `hoje=07/11`; o mesmo evento ⇒ **08/11/2026** quando `hoje=08/11`. Essa dívida já está declarada como `EXPIRA-EM-ANDA-COM-HOJE`, e o diff não a cura (`app/chamados/veredito_lavrado.py:119`; `app/chamados/management/commands/backfill_veredito_lavrado.py:24`).

   **Escrita/leitura:** `lavrar` atribui e salva o trio; o backfill calcula e atribui, persistindo pela porta de lote; desfazer restaura a foto. Os leitores decisórios são `esta_stale` e a varredura; o backfill também compara o campo. Outros caminhos persistem o resultado já lavrado no mesmo save (`app/chamados/veredito_lavrado.py:245`; `:247`; `:253`; `:261`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`; `:140`; `:143`; `app/chamados/services/raiox.py:42`; `app/chamados/models.py:2710`; `app/chamados/services/fio.py:904`).

10. **Fila — chamadores: nenhuma quebra de assinatura encontrada; duas chamadas continuam sem `ate`.**

    - `expira_em`: os chamadores executáveis fornecem `hoje`; `esta_stale`: tarefa, auditor e testes fornecem `marcos, agora`. As duas chamadas sem argumentos novos no selo são deliberadas, sob `assertRaises(TypeError)` (`app/chamados/veredito_lavrado.py:245`; `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`; `app/chamados/tests/test_e2_tarefa_do_dia.py:648`).
    - **Não migrado:** alias `_mdl_s2` no fio. Não quebra porque `ate=None` permanece válido; atualmente os marcos chegam a `pergunta_sem_acao_possivel`, que ignora o argumento. São cinco consultas sem efeito nessa decisão (`app/chamados/services/fio.py:248`; `:249`; `:284`; `app/chamados/services/raiox.py:174`; `:183`; `app/chamados/veredito_lavrado.py:125`).
    - **Não migrado:** teste de concordância. Não quebra a assinatura, mas compara varredura limitada a um instante com marcos sem limite e outro relógio; pode divergir se houver mudança entre as leituras (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:56`; `:58`; `:59`; `app/chamados/services/raiox.py:35`).
    - `grade_da_celula` preserva os três argumentos antigos e acrescenta alimentação opcional; os chamadores antigos não precisam migrar. Isso inclui folha, calendário, relatórios, inteligência, espelho, emissão e materialização (`app/escala/services/leitor_celula.py:350`; `app/folha/export.py:338`; `app/colaboradores/services/calendario.py:334`; `app/relatorios/furos.py:132`; `app/inteligencia/calculadores.py:42`; `app/ponto/services/espelho.py:519`; `app/chamados/services/disputa_emissao.py:83`; `app/chamados/services/materializacao.py:233`).

11. **Fila — equivalência da grade: nenhum contraexemplo válido encontrado com alimentação completa do mesmo estado; o selo não prova “todo caso”.** Os caminhos usam o mesmo mapa recortado, cadastro e cálculo; a carga fornece todos os vínculos. Múltiplos vínculos continuam submetidos ao juiz registrado, que prioriza a geradora e depois a vigência (`app/escala/services/leitor_celula.py:361`; `:364`; `:372`; `:411`; `app/ponto/turnos.py:2988`; `app/escala/alimentacao.py:269`; `:285`).

    **Casos:** célula fora da janela é descartada; sem célula retorna `sem_celula`; sem vínculo, o previsto calculado retorna zero; admissão/demissão passam pelo mesmo predicado antes do cálculo. Alimentação incompleta não tem igualdade garantida — `{}` significa que não existe célula carregada naquele dia (`app/escala/services/leitor_celula.py:364`; `:389`; `:394`; `app/escala/utils.py:2256`; `app/escala/alimentacao.py:240`).

    O selo tem **um vínculo**, células existentes e demissão; não exercita múltiplos vínculos, colaborador sem vínculo, admissão ou forma agregada. A igualdade e a ausência de consultas são verificações úteis, mas insuficientes para a universalidade solicitada (`app/escala/tests/test_grade_alimentada.py:26`; `:33`; `:44`; `:51`; `:57`).

12. **Fila — mordida dos demais selos, por inspeção.**

    | Selo | Caso que distingue o mutante | Limite |
    |---|---|---|
    | **Caso10 MORDE** | Injeta consulta real a `FechamentoMensal`; o mesmo detector usado na proibição exige encontrá-la. B4 foi curado (`app/chamados/tests/test_e2_tarefa_do_dia.py:507`; `:521`; `:533`; `:539`; `:544`). | Não depende de o fechamento existir: a consulta já basta para violar a proibição (`app/chamados/tests/test_e2_tarefa_do_dia.py:539`). |
    | **Caso13** | Razões específicas e contracasos cobrados impedem silêncio geral (`app/chamados/tests/test_e2_tarefa_do_dia.py:560`; `:582`; `:597`). | Prova comportamento; uma cópia equivalente da regra também passaria. Lote e casca compartilham implementação (`app/chamados/tests/test_e2_tarefa_do_dia.py:609`; `app/chamados/services/tarefa_do_dia.py:352`). |
    | **LavraNoRelogioDaBorda** | Fato das 22h não vence às 21h e vence às 23h; consulta UTC preserva o dia local (`app/chamados/tests/test_e2_tarefa_do_dia.py:630`; `:634`; `:641`). | Exercita apenas `CelulaDia.veredito_em` e a conversão de `agora`; não cobre o julgamento UTC do achado 1 (`app/chamados/tests/test_e2_tarefa_do_dia.py:623`; `:630`). |
    | **`test_grade_alimentada`** | Compara resultado, consultas e triestado explícito (`app/escala/tests/test_grade_alimentada.py:51`; `:52`; `:60`). | Cobertura limitada aos dados do achado 11 (`app/escala/tests/test_grade_alimentada.py:20`). |

    Esses testes não passam por ausência total de exercício; execução efetiva da suíte Django nesta auditoria: **[nao verificado]**. [nao verificado]

13. **Fila — classes arquiteturais e restrição E1.** A derivação paralela de staleness permanece na varredura, expressamente declarada; ela já reproduz o erro de fuso do juiz. Há também relógio fora da borda no juiz original `_dentro_da_janela_viva`, que continua lendo `date.today()`, enquanto a expiração agora usa data local recebida (`app/chamados/services/raiox.py:17`; `:42`; `app/chamados/juizes.py:563`; `app/chamados/veredito_lavrado.py:245`).

    **Caso:** se o SO estiver em UTC, às **21/11 01:30 UTC = 20/11 22:30 São Paulo**, o juiz pode usar a janela de 21/11 e o carimbo de expiração a de 20/11. O fuso do SO em produção é **[nao verificado]**; os dois contratos diferentes estão no código (`app/chamados/juizes.py:564`; `:583`; `:661`; `app/chamados/veredito_lavrado.py:117`).

    A convivência dos cinco respondedores antigos com `tarefa_do_dia` está autorizada até E3; não constitui, por si, juiz clandestino. O caminho novo delega dia sem trabalho ao juiz registrado (`app/core/juizes.py:150`; `app/docs/JUIZES.md:73`; `:80`; `app/ponto/supra_juiz.py:69`).

    `lavrar` continua lendo o relógio, mas foi explicitamente declarado borda de escrita; sua assinatura preservada atende `models.py`. O fio sem `ate` também continua executável, com o efeito descrito no achado 10. Não há nesses dois pontos quebra nova de assinatura (`app/chamados/tests/test_e2_tarefa_do_dia.py:722`; `app/chamados/veredito_lavrado.py:227`; `:244`; `app/chamados/models.py:2709`; `app/chamados/services/fio.py:249`).

    A tarefa continua dependente dos campos e da sequência da E1; pouso e pré-requisitos efetivamente aplicados são **[nao verificado]**. Não identifiquei nova escrita fora da porta nem nova aquisição de trava no caminho alterado; o backfill persiste pela porta de lote (`app/chamados/services/tarefa_do_dia.py:280`; `:289`; `app/chamados/management/commands/backfill_veredito_lavrado.py:143`; `app/chamados/veredito_lavrado.py:223`).

## 07/11 08:54 · sessao 6b0be6c97974 · - · - · origem=Ronald (tmux)

Achados confirmados na cópia, sem execução em banco:

1. **Reentrada no índice pela ressuscitação.** `_universo_da_ressuscitacao` inclui perguntas materializadas sem texto, sem excluir `duplicata` ou `motivo_retratado`; remove o carimbo na cópia e pergunta ao C6 se ela viveria. Se a célula acusa o dia e o marco está apagado, a antiga pode voltar mesmo havendo substituta para o mesmo fato. `ressuscitar_perguntas` então executa `UPDATE via_resolucao=''`, reintroduzindo a linha no índice sem `fato_livre_para` nem `_nascer`. Referências: `app/chamados/models.py:1449`, `:1470`, `:1473`, `:1524`, `:2852`, `:3060`. Caminhos: reabertura em `app/chamados/models.py:1417`; cartório em `app/ponto/services/cartorio.py:1622`; desarquivamento em `app/chamados/views_arquivar.py:86`.

2. **Reentrada no índice por `fechar_por_admin`.** Uma duplicata respondida pode continuar viva porque a resposta precede a regra da via; `fechar_o_fio_do_dia` seleciona `q_vivas` e troca a via para `admin_decidiu`, entrando novamente no índice enquanto a canônica permanece. Referências: `app/chamados/juizes.py:1152`, `:1164`, `:1173`; `app/chamados/models.py:2935`, `:2946`, `:2675`, `:2797`, `:3061`; `app/ponto/services/veredito_celula.py:174`. Também há chamadas diretas em `app/chamados/services/validacao.py:282` e `:317`.

3. **Identidade BATIDA ainda tem dois significados.** `_nascer` passa o episódio ao dedup; `fato_livre_para` não passa `p.batida_origem_id` e consulta motivo/dia/instância. Assim, dois episódios GPS distintos, permitidos pelo índice no mesmo dia, podem impedir um rechaveamento legítimo. Referências: `app/chamados/models.py:2582`, `:2513`, `:2496`, `:3064`.

4. **Outro derivador antigo mata episódios distintos.** `migrar_resolvedoras_vivas` identifica conflito somente por motivo e data; canoniza e retrata uma das linhas, mesmo quando são BATIDAS de episódios diferentes. Caminho produtivo: `ChamadoColaborador.reabrir`, ramo `irmao_vivo`. Referências: `app/chamados/services/fio.py:1013`, `:1017`, `:1019`; `app/chamados/models.py:1387`, `:1390`, `:3064`.

Inventário de escrita:

| Campo | Escritores encontrados |
|---|---|
| `batida_origem` | `_nascer` repassa o episódio, `app/chamados/models.py:2579`; `_chave_de_identidade` preenche na inserção, `:3026`; derivador lê o contexto e confere o dono, `:3041`–`:3045`. |
| `instancia_marco` | `_chave_de_identidade`, `app/chamados/models.py:3023`. Aceita valor explícito não vazio; não o reconfere com o motivo, `:3023`. |
| `colaborador` | `_chave_de_identidade`, `app/chamados/models.py:3017`. Igualdade com a disputa é conferida somente na inserção, `:3007`. |
| `conversa` | `abrir`/`abrir_conversa` passam `False`/`True`, `app/chamados/models.py:2529`, `:2540`; persistência em `:2629`. |
| `data_evento` | Nascimento em `app/chamados/models.py:2629`; datar pergunta sem dia em `app/chamados/services/fio.py:841`, `:863`; rechaveamento em `:913`, `:919`. |
| `via_resolucao` | `_escrever_carimbo`, `app/chamados/models.py:2713`; gravações por `carimbar :2762`, `carimbar_identidade :2797`, `descarimbar :2805`; lote por `.update() :2852`; encerramento por fato `:2862`; relavra direta em `app/chamados/services/relavra_via.py:123`, `:125`. |

Não encontrei criação produtiva de `PerguntaDisputa` fora de `_nascer`, cujo `cls.objects.create` está em `app/chamados/models.py:2629`; completude absoluta diante de escritores dinâmicos: **[nao verificado]**. Não encontrei `bulk_create` de perguntas; o `bulk_update` de `app/chamados/veredito_lavrado.py:207` usa os campos da lavra, não as chaves de identidade (`app/chamados/veredito_lavrado.py:57`). Buscas em `bin/`, `scripts/`, `ops/`, `eval/` e `fatias_agendadas/` não localizaram outro escritor desses campos; chamadas indiretas/genéricas: **[nao verificado]**. O leitor explícito de perguntas encontrado em `bin/` somente conta linhas: `bin/censo_folha.py:28`.

Leitores dos campos novos:

- Episódio: `app/chamados/models.py:2467`, `:2471`, `:2472`, `:3064`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:83`, `:87`.
- Instância: `app/chamados/models.py:2496`, `:3059`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:82`.
- Conversa: `app/chamados/models.py:2472`, `:2497`, `:2511`, `:3023`, `:3026`, `:3060`, `:3065`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:95`.
- Colaborador materializado: `app/chamados/models.py:2497`, `:2509`; `app/chamados/services/fio.py:853`, `:901`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:58`, `:142`, `:161`, `:176`.

Leitores produtivos de `data_evento`, agrupados:

- Núcleo: `app/chamados/juizes.py:266`, `:363`, `:623`, `:659`, `:932`, `:1199`, `:1525`, `:1965`, `:2031`; `app/chamados/services/materializacao.py:279`, `:344`, `:473`, `:517`, `:533`, `:621`, `:1256`; `app/chamados/veredito_lavrado.py:229`; `app/chamados/services/fio.py:87`, `:777`, `:839`, `:904`, `:1014`.
- Emissão/declaração/validação: `app/chamados/services/disputa_emissao.py:392`, `:408`, `:433`, `:821`, `:963`, `:1293`, `:1389`, `:1707`; `app/chamados/services/declaracao_texto.py:237`, `:330`, `:337`, `:550`, `:625`; `app/chamados/services/validacao.py:268`, `:425`, `:476`, `:489`, `:645`.
- Reconciliação: `app/chamados/reconciliador.py:370`, `:944`, `:1040`, `:1072`, `:1142`, `:1293`, `:1543`, `:1553`, `:1628`, `:1641`.
- Outros serviços/telas: `app/chamados/canal.py:65`; `app/chamados/services/aviso_pendencia.py:20`; `cobranca.py:83`; `competencia_trancada.py:107`, `:252`; `explicador.py:70`, `:126`; `fabrica_da_celula.py:118`, `:137`; `historico.py:108`; `limbo_folgas.py:202`; `passivo_validado.py:113`, `:519`; `pergunta_no_app.py:92`, `:109`; `relavra_via.py:101`, `:152`; `vigia_fio.py:816`; `app/chamados/templatetags/chamados_extras.py:31`; `app/api/views.py:1184`, `:2664`; `app/api/views_mensageria.py:568`; `app/colaboradores/services/calendario.py:473`; `app/folha/previa.py:65`; `app/ponto/services/dia_do_colab.py:65`; `app/ponto/services/veredito_celula.py:176`.
- Comandos: `app/chamados/management/commands/backfill_decisoes_pergunta.py:58`, `:139`, `:144`; `backfill_veredito_lavrado.py:111`; `dedup_perguntas_fato.py:54`, `:69`; `disparar_perguntas_competencia.py:213`; `encerrar_perguntas_com_fato.py:74`; `migrar_data_evento_dia_do_turno.py:79`, `:126`, `:186`, `:306`, `:315`; `reabrir_orfas_enterradas.py:68`, `:86`, `:117`; `auditar_invariantes_chamados.py:63`, `:148`, `:282`, `:325`, `:892`, `:914`, `:957`, `:1662`, `:1775`, `:1801`; `app/ponto/management/commands/mapa_divergencia.py:272`; `app/ponto/management/commands/vigia_plantio_overnight.py:86`.

Leitores de `via_resolucao`, além dos próprios escritores:

- `app/chamados/juizes.py:648`, `:659`, `:814`, `:1147`, `:1173`; `app/chamados/models.py:2474`, `:2498`, `:2937`, `:2939`, `:3061`, `:3066`; `app/chamados/reconciliador.py:1133`, `:1144`; `app/chamados/services/materializacao.py:438`, `:623`, `:665`, `:1080`; `passivo_validado.py:141`, `:515`; `relavra_via.py:116`, `:153`, `:179`, `:230`; `fio.py:958`; `fabrica_da_celula.py:119`; `explicador.py:95`; `competencia_trancada.py:251`; `app/ponto/services/cartorio.py:1599`; `app/ponto/services/flip_auto.py:129`.
- Comandos: `app/chamados/management/commands/auditar_invariantes_chamados.py:202`, `:433`, `:542`, `:955`, `:1707`; `backfill_decisoes_pergunta.py:96`, `:127`, `:178`; `dedup_perguntas_fato.py:74`; `encerrar_perguntas_com_fato.py:33`, `:42`; `migrar_data_evento_dia_do_turno.py:52`; `reabrir_orfas_enterradas.py:67`, `:129`; `relavrar_via_i6.py:58`, `:88`.

Semântica temporal: `abrir` declara dia do turno, mas conversa grava dia civil do nascimento no mesmo campo; os leitores frequentemente não consultam `conversa`. Referências: `app/chamados/models.py:2523`, `:2534`, `:2540`; leitura da janela em `app/chamados/veredito_lavrado.py:229`. A janela viva é inclusiva nas duas pontas, `app/chamados/juizes.py:589`; a expiração reproduz essa inclusão em `app/chamados/veredito_lavrado.py:116`. `via_pelo_fato` usa o dia civil inteiro para provar ausência de batidas, deliberadamente, e depois consulta o turno com a mesma data: `app/chamados/juizes.py:1451`, `:1462`, `:1463`, `:1481`; `batidas_apuraveis` inclui início e fim, `app/ponto/turnos.py:2868`, `:2870`.

## 07/11 08:53 · sessao 20696c111c9b · - · - · origem=Ronald (tmux)

Achados confirmados na cópia, sem execução de banco ou testes. [nao verificado]

1. **O backfill não cumpre “data_evento NULL ... saem ... ANTES do índice”.** `fora_do_indice()` apenas lista MARCO sem dia e BATIDA sem origem; `aplicar()` recebe somente colisões e admin sem ato, sem reparar nem retratar esses dois baldes. A guarda da migration exclui expressamente as chaves NULL e permite subir os índices com essas linhas restantes. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:114`, `:149`, `:263`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:32`, `:35`)

2. **A proteção de competência exportada pode ignorar participantes de colisão BATIDA.** A chave BATIDA não inclui dia; o teste constrói colisão com dois dias diferentes. `competencia_de()` calcula a competência de cada pergunta, mas `handle()` classifica o grupo inteiro apenas pela competência de `pks[0]`. Se a primeira estiver aberta e outra exportada, `paradas_apply` não protege esta última e `aplicar()` pode retratá-la. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:83`, `:137`, `:235`, `:243`, `:263`; `app/chamados/tests/test_e1_v5_portas_backfill.py:141`)

3. **O comando faz a verificação da exportação antes das travas e não a repete no apply.** `handle()` consulta `competencia_entregue`; `aplicar()` abre seus próprios atomics por grupo e por pergunta sem consultar novamente a competência. Uma alteração concorrente entre essas etapas não é coberta pelo código mostrado; ocorrência real em produção [nao verificado]. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:243`, `:259`, `:158`, `:174`)

4. **F — “a primeira fica” diverge deliberadamente do código e do selo.** `canonica_do_grupo()` ordena por PK, prefere a primeira com `resposta_colab.strip()` não vazia e somente na ausência de resposta conserva o menor PK. Portanto, uma pergunta posterior respondida retrata a primeira não respondida. Entre respondidas, vence o menor PK; espaços em branco não contam como resposta. O backfill usa essa autoridade no DRY e no apply; o teste exige justamente a substituição da primeira pela segunda respondida. (`app/chamados/services/fio.py:943`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:165`, `:233`; `app/chamados/tests/test_e1_v5_portas_backfill.py:119`)

**D — mordida dos quatro arquivos novos**

| Selo | O que verifica | Mutante de produção que escapa |
|---|---|---|
| `test_e1_v5_familia.py` | Família obrigatória e cobertura; campos dos constraints do **modelo**; listas congeladas da migration. (`:40`, `:44`, `:78`, `:91`) | Remover `instancia_marco` dos `fields` da **AddConstraint da migration**, preservando o modelo e as listas: esse selo não inspeciona os campos da operação migratória. Também não compara os conectivos e exclusões da condição: `_motivos_da_condicao()` extrai somente `motivo__in`. (`:62`, `:84`, `:91`; migration `:53`) |
| `test_e1_v5_indice.py` | Corrida crua MARCO/BATIDA, episódios distintos, motivos distintos, conversa/duplicata, porta sobre legado sem origem. (`:62`, `:69`, `:84`, `:90`, `:96`, `:110`) | Remover `instancia_marco` da chave real do índice passa por todos esses casos: nenhuma fixture cria duas instâncias diferentes no mesmo dia/motivo. Também não há caso para exclusão por `motivo_retratado`. (`:64`, `:71`, `:96`, `:106`) |
| `test_e1_v5_ordem_trava.py` | Ordem das tabelas vistas nos quatro caminhos isolados; dois mutantes simulados. (`:88`, `:95`, `:103`, `:112`, `:121`, `:135`) | Desligar o signal do cartório, ou retirar a fabricação no ciclo, não torna vermelho o teste de `analisar`: ele exige apenas a trava do Chamado. A fixture não informa célula ou contexto de dia; o signal só chama o cartório quando encontra dia. (`:77`, `:88`; `app/chamados/signals.py:176`) |
| `test_e1_v5_portas_backfill.py` | Recusa de colisão nas duas portas; ato de fechamento; colisões MARCO/BATIDA, idempotência, exportada simulada, ordem. (`:56`, `:70`, `:81`, `:119`, `:137`, `:150`, `:161`, `:173`) | Remover `ato_por` das novas gravações passa: os testes conferem somente `ato`/`ato_em`. Também passam ausência de tratamento para NULL e verificação de exportação somente da primeira competência, pois faltam esses casos. (`:87`, `:156`, `:141`, `:164`) | [nao verificado]

A guarda `_guarda()` poderia virar `noop` sem ser exercitada pelos novos testes: eles retiram/devolvem os índices diretamente por `schema_editor`, sem executar a migration 0053 sobre acervo sujo. (`app/chamados/tests/acervo_sem_indice.py:18`; `app/chamados/tests/test_e1_v5_portas_backfill.py:95`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:29`)

O coletor de travas deduplica **por tabela**, sem identificar linha e sem reiniciar por transação. Assim, um segundo Colaborador travado depois de Pergunta, ou uma segunda Disputa depois de Chamado, pode ficar invisível se essa tabela já apareceu. Também captura somente a primeira tabela do `FROM`, não todas as tabelas potencialmente alcançadas por um SELECT com joins. (`app/chamados/tests/test_e1_v5_ordem_trava.py:29`, `:38`, `:43`, `:45`)

**G — migration 0053**

- A mesma classe `Migration` contém `RunPython(_guarda)` e as duas `AddConstraint`, sem `atomic=False`. Pelo padrão do Django/PostgreSQL isso corresponde a uma transação de migration; confirmação do executor instalado e da execução real [nao verificado]. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:42`)
- A guarda faz somente consultas agregadas, sem INSERT/UPDATE/DELETE. Portanto, ela própria não produz eventos de FK pendentes que expliquem `pending trigger events`; o ambiente real e eventuais triggers externos [nao verificado]. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:29`)
- Guarda e índices usam a **mesma função `_no_fato()`**, com as mesmas listas congeladas. A guarda acrescenta `data_evento IS NOT NULL` e `batida_origem IS NOT NULL`; isso corresponde às chaves que podem colidir num índice UNIQUE com NULLs distintos. Não há divergência local de `conversa`, motivo ou vias entre guarda e condição. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:25`, `:32`, `:35`, `:54`, `:59`)
- O helper de teste executa `SET CONSTRAINTS ALL IMMEDIATE` antes do DDL, mas a migration não executa DML anterior que exija essa drenagem. A necessidade concreta no banco de produção [nao verificado]. (`app/chamados/tests/acervo_sem_indice.py:18`; migration `:29`)

**A — censo de `DecisaoPergunta.ato/ato_em/ato_por`**

Escritor físico único: `registrar()` constrói as instâncias e faz `bulk_create`. (`app/chamados/models.py:3517`)

Produtores dos valores:

- Fechamento da disputa: SILENCIO, carimbo da pergunta e usuário. (`app/chamados/models.py:1878`)
- Retratação de motivo: RETRATACAO, instante do ato e ator. (`app/chamados/models.py:1998`)
- Resposta humana: RESPOSTA, `_agora` compartilhado com `respondida_em`, usuário. (`app/chamados/services/disputa_emissao.py:1369`)
- Reabertura/recusa: ato recebido, instante único e `por`. (`app/chamados/services/disputa_emissao.py:1478`)
- Resposta automática e resposta carimbada pela trilha: RESPOSTA e instante recebido, sem autor informado. (`app/chamados/services/disputa_emissao.py:1519`, `:1540`)
- Validação e devolução: VALIDACAO com `validada_em`; DEVOLUCAO com novo `timezone.now()`; autor recebido. (`app/chamados/services/validacao.py:139`, `:205`)
- Absorção: ABSORCAO com o mesmo instante do carimbo e usuário. (`app/chamados/services/materializacao.py:558`)
- Fechamento do fio: SILENCIO com `materializada_em` e autor. (`app/ponto/services/veredito_celula.py:182`)
- Backfill: SILENCIO com `materializada_em` e `materializada_por_id`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:181`)
- Projeção legada: RESPOSTA/VALIDACAO com seus carimbos originais; validação conserva `validada_por_id`, resposta não informa autor. (`app/chamados/models.py:3505`)

Leitores:

- Ordenação por `ato_em`, depois PK. (`app/chamados/models.py:3463`)
- Correspondência de legado por `(pergunta, ato, ato_em)`. (`app/chamados/models.py:3501`)
- Histórico da tela lê somente RESPOSTA e atos com foto. (`app/chamados/models.py:3082`)
- Fio faz prefetch de `ato_por`; template mostra ato, instante e autor. (`app/chamados/services/fio.py:231`; `app/templates/chamados/partials/modal_fio.html:208`)
- Backfill verifica existência de qualquer SILENCIO/VALIDACAO, sem correspondência de instante. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:125`, `:179`)

**Possível ESMERIL em `ato`:** SILENCIO é apresentado como “Silencio encerrado pela porta”, e o fechamento da disputa recusa respostas pendentes antes desse ato. Já `fechar_o_fio_do_dia()` percorre `q_vivas()`, cujo universo inclui resposta não vazia, e grava SILENCIO mesmo assim. O enum passa a cobrir tanto ausência de resposta quanto encerramento administrativo de pergunta respondida. (`app/chamados/models.py:3439`, `:1848`, `:2918`; `app/ponto/services/veredito_celula.py:174`)

## 07/11 08:53 · sessao 184f8267eb13 · - · - · origem=Ronald (tmux)

Achados para C/D/E, por leitura estática; execução concorrente e ocorrência em produção [nao verificado].

1. **A ordem global continua violada no fechamento de disputa.** `DisputaSupervisao.fechar` abre atomic em `app/chamados/models.py:1866`; nas exceções, grava Pergunta primeiro (`:1875` → `fechar_por_admin`, `:2675` → `carimbar_identidade`, `:2797`); depois grava Disputa (`:1890`). O post_save pede `ch.reconciliar()` (`app/chamados/signals.py:41-42`), que chama `_recarregar_estado_se_mudou` (`app/chamados/models.py:627-628`, `:649`) e só então trava Colaborador (`:686`). Mesmo sem perguntas vivas, a sequência Disputa UPDATE → Colaborador permanece quando o pai está vivo (`app/chamados/signals.py:33`).

2. **Aprovação/materialização de justificativa começa pelo Chamado.** `aprovar_justificativa_dp` abre atomic e chama `travar_origem_antes` (`app/ponto/services/justificativa.py:125-126`), cuja trava é Chamado FOR UPDATE (`:114`); depois `encerrar_chamado_de_origem` (`:202`) trava Chamado novamente (`:38`) e chama `retratar` (`:42`) → `_retratar` (`app/chamados/reconciliador.py:522-523`) → `reconciliar` → Colaborador (`app/chamados/models.py:649`, `:686`). `materializar_saida_dp` tem a mesma entrada (`app/ponto/services/justificativa.py:327-329`) e chama o fechamento da origem (`:393`).

3. **Validação de folga grava Pergunta antes de chegar ao Colaborador.** Ramo de folga já declarada: atomic (`app/chamados/services/validacao.py:280`) → `_carimbar_validacao` (`:281`) → save UPDATE Pergunta (`:137`) → `fechar_o_fio_do_dia` (`:289`) → `c.reconciliar` (`app/ponto/services/veredito_celula.py:190`) → trava Colaborador (`app/chamados/models.py:686`). Ramo de nova folga também grava a validação primeiro (`app/chamados/services/validacao.py:307-309`) antes de declarar o dia.

4. **A própria função tocada `fechar_o_fio_do_dia` não estabelece a ordem prometida.** Percorre e grava Perguntas (`app/ponto/services/veredito_celula.py:174-177`) antes de reconciliar Chamados (`:189-190`), que adquire Colaborador (`app/chamados/models.py:686`). Seus chamadores usam atomic envolvendo a função: `veredito_celula` (`app/ponto/services/veredito_celula.py:211`, `:332`) e declaração de folga (`app/ponto/services/declarar_dia.py:188`, `:204`). Uma trava anterior eventualmente adquirida por efeitos de cada ramo não é garantida por essa função; cenário explícito da validação já acima (`app/chamados/services/validacao.py:280-289`).

5. **Comando de reata mantém Chamado → Colaborador.** `reatar_beco_folga_ao_limbo` abre atomic e trava pai Chamado (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99-100`); chama reata (`:105`) → `reatar_perguntas_ao_limbo` → `reapontar_resolvedora` (`app/chamados/services/limbo_folgas.py:243`) → trava Colaborador (`app/chamados/services/fio.py:853`, caso sem nova data).

6. **O ciclo literal analisar → cartório → fábrica ainda admite Chamado antes de Disputa.** O novo `_gravar_estado` trava Colaborador e depois grava Chamado (`app/chamados/models.py:285`, `:290`). O post_save com dia conhecido chama cartório (`app/chamados/signals.py:177-180`), cujo final chama fábrica (`app/ponto/services/cartorio.py:997`). A fábrica pode chamar `perguntar` (`app/chamados/services/fabrica_da_celula.py:176`) → `criar_ou_atualizar_disputa` (`:139`) → `_abrir_ou_agregar_disputa` (`app/chamados/services/disputa_emissao.py:642`) → Disputa FOR UPDATE (`:511`). Também pode reler Pergunta e Disputa juntas (`app/chamados/services/fabrica_da_celula.py:166` → `app/chamados/services/relavra_via.py:221`). Portanto, colocar Colaborador primeiro resolve aquela inversão de duas tabelas no trecho, mas não implementa a ordem literal Colaborador → Disputa → Chamado → Pergunta no ciclo inteiro (`app/chamados/models.py:279-290`; caminhos citados).

7. **O selo novo não percorre o ciclo do aval.** A fixture de Chamado omite célula e data (`app/chamados/tests/test_e1_v5_ordem_trava.py:77-79`); o teste de analisar só executa essa fixture (`:88-93`). O signal só chama cartório com dia conhecido (`app/chamados/signals.py:177-180`), e `dia_do_fato(fortes=True)` devolve None sem célula/data (`app/chamados/juizes.py:93-124`). Assim, um mutante que retire a trava prévia somente quando o Chamado possui célula/dia não é exercitado pelo teste analisar; os ramos reais com cartório/fábrica também ficam fora dessa fixture (`app/chamados/tests/test_e1_v5_ordem_trava.py:77-93`).

8. **O detector de trava confunde tabela já vista com linha já travada.** `travas` registra somente a primeira ocorrência por nome de tabela (`app/chamados/tests/test_e1_v5_ordem_trava.py:38`, `:45-47`). Mutante: travar Colaborador A, depois Chamado, depois Colaborador B; a segunda trava de colaborador é descartada e o detector não denuncia a inversão por outra linha (`:45-47`, `:57-65`). O parser de FOR UPDATE também só lê o primeiro `FROM`, sem interpretar tabelas de JOIN/`OF` (`:43-44`); a produção tem precisamente uma trava conjunta de Pergunta+Disputa (`app/chamados/services/relavra_via.py:221-224`).

Os testes novos de ordem exercitam apenas analisar, nascer reapontando duplicata, reapontar e rechavear (`app/chamados/tests/test_e1_v5_ordem_trava.py:88-119`); os caminhos de fechar disputa, validar folga e aprovar justificativa acima não são chamadas desses quatro casos (`mesmo arquivo:88-119`).

## 07/11 08:52 · sessao 3374abd0dab0 · V2-I2 · sonda · origem=Code

**A volta 2 fecha a diferença em 27/09: +420 minutos. Ela ainda não identifica uma mudança de campo que causou o zero gravado, o autor dessa mudança ou o dono conclusivo L-099.** [nao verificado]

1. **Dia e campo explicativo**

| Dia / célula | Previsto hoje | DP gravado | Diferença |
|---|---:|---:|---:|
| 27/09/2026 / 33835 | 420 | 0 | **+420** |
| Total informado | 10920 | 10500 | **+420** |

A tabela reproduz a única divergência emitida pela sonda fornecida. [nao verificado]

- **`trabalha=True`** permite calcular a jornada; a função retorna zero quando `eh_dia_trabalho` não retorna exatamente `True`. Não foi fornecido o valor histórico de `trabalha` usado na lavratura do DP. ([utils.py:2287](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/utils.py:2287); [nao verificado])
- **DNA:** os marcos visíveis explicam os 420 atuais: 15–23 são 480 minutos, descontados 60 de pausa. **A troca 19–20 → 20–21 acrescenta zero minutos**, pela mesma aritmética. ([utils.py:2293](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/utils.py:2293), [utils.py:2326](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/utils.py:2326))
- **Geradora e vigência:** a seleção atual procura a geradora nomeada pela célula e rejeita o dia fora de suas datas. O resultado atual de 420 indica que essa seleção passou; porém, o raspado não fornece o PK, as datas nem seus estados históricos. ([alimentacao.py:269](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/alimentacao.py:269); [nao verificado])

Portanto, **o DNA explica o valor atual; nenhum dos quatro campos está demonstrado como causa histórica de 0 → 420**. [nao verificado]

Há uma diferença decisiva entre os leitores: a grade usada no fechamento prefere **`ata.minutos_previstos`**, inclusive quando vale **zero**, e só chama `minutos_previstos_do_dia` se esse campo for `None`. O mapa destinado ao DP inclui somente dias classificados como `trabalho` ou `ausencia`. Assim, também precisam ser investigados **ATA e classificação do dia**, além dos quatro campos propostos. ([leitor_celula.py:380](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/services/leitor_celula.py:380), [dia_pago.py:387](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/services/dia_pago.py:387))

2. **Escritor, momento e disparador**

O escritor compatível com os marcadores apresentados é **`regenerar_celulas_vinculo`**: reatribui `trabalha`, `escala_geradora` e `dna`, guarda `dna_anterior`, atualiza `regenerada_em` e incrementa o contador. Sua condição exige diferença em **qualquer um** dos três campos; não prova que todos mudaram. ([celula.py:263](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:263), [celula.py:271](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:271))

O marcador informado é **23/10/2026 13:04:42.382279 UTC**, contador **2**. A atribuição desse ato concreto ao escritor e ao seu chamador permanece [nao verificado].

A auditoria pertinente é **`acao='regenerar_celulas_vinculo'`, `modelo='EscalaColaborador'`, `objeto_id=PK da geradora`**; registra autor opcional, motivo e janela. Os logs fornecidos são apenas `veredito_celula` de **33829–33834**, sem a regeneração de **33835**. **Quem disparou: [nao verificado].** ([celula.py:334](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:334); [nao verificado])

O marcador também não certifica toda escrita posterior: existem caminhos que alteram campos sem atualizá-lo. ([celula.py:243](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:243), [celula.py:865](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:865))

3. **Ordem temporal**

| Marco informado | UTC |
|---|---|
| Célula `regenerada_em` | 23/10 13:04:42.382279 |
| FM `atualizado_em` | 29/10 10:26:49.276449 |
| DP `lavrado_em` | 29/10 10:26:49.281559 |
| Célula `julgada_em` / `insumos_em` atuais | 02/11 07:25:34.015313 |

Valores e ordenação derivados do raspado. [nao verificado]

**O DP é posterior à regeneração registrada e ao carimbo do FM, mas anterior ao julgamento atual da célula.** Portanto, a cronologia fornecida não sustenta “DP anterior à regeneração”. [nao verificado]

O campo temporal do DP nesta cópia chama-se **`lavrado_em`**, com `auto_now=True`; não `atualizado_em`. ([models.py:380](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/models.py:380))

4. **Dono L-099 e prova faltante**

**Dono conclusivo: [nao verificado].** A L-099 exige coerência com o cadastro existente; regeneração, isoladamente, não demonstra cadastro errado. ([LEIS.md:122](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/docs/LEIS.md:122))

O juiz aplica esta prioridade: flags de cadastro ausente/indefinido → **CADASTRO**; batida fora de turno ou quantidade ímpar → **BATIDA**; fase deslocada ou Cadastro × Realidade → **CADASTRO**; restante → **ESTRUTURA**. `furo_apurado` e `veredito='furo'` não são argumentos desse juiz. **Zero batidas é par**; com as demais flags falsas, retorna **ESTRUTURA**. ([e6_oraculo.py:53](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/management/commands/e6_oraculo.py:53), [e6_oraculo.py:98](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/management/commands/e6_oraculo.py:98))

Falta exatamente:

- **Autoria:** PK da geradora e auditoria completa da regeneração de 23/10, com `usuario`, `ator`, descrição e valores anteriores/posteriores. ([celula.py:334](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/portas/celula.py:334))
- **Causa do zero:** ATA/grade consumida em 29/10, sobretudo `minutos_previstos` e `tipo_dia`, mais os estados históricos de trabalha/geradora/vigência. A ATA atual está truncada e seu julgamento é posterior ao DP. ([leitor_celula.py:380](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/escala/services/leitor_celula.py:380); [nao verificado])
- **Dono:** `horas_do_dia` do oráculo e os seis indicadores efetivos: `folga_nao_definida`, `sem_vinculo_no_dia`, `sem_vinculo_em_dia_de_trabalho`, `batida_fora_de_turno`, `fase_deslocada` e `em_cadastro_x_realidade`. Os indicadores da célula não substituem essa alimentação. ([e6_oraculo.py:172](/home/ronald/codex_ws/ffbda473e3a653dedb413f163cfc2ae2d7e0443f/app/ponto/management/commands/e6_oraculo.py:172))

## 07/11 08:52 · sessao 9beb35ca1c56 · - · - · origem=Ronald (tmux)

1. **Bloqueia — mistura de dia UTC e local permanece.** `esta_stale` usa `timezone.localdate(agora)` para comparar a expiração, mas `p.julgado_em.date()` para decidir se a lavra antecedeu a virada; a varredura duplica essa combinação (`app/chamados/veredito_lavrado.py:198`, `app/chamados/veredito_lavrado.py:201`, `app/chamados/services/raiox.py:42`). Contraexemplo reproduzido apenas em memória, extraindo a função por AST: expiração `2026-11-21`, julgamento `2026-11-21 01:30 UTC` (=20/11 22:30 SP), agora `21/11 09:00 SP`, sem marcos → `False`, embora o julgamento local anteceda a expiração (`app/chamados/veredito_lavrado.py:198`). O teste novo exercita apenas `agora` em UTC, com julgamento às 20h locais, portanto não cobre esse caso (`app/chamados/tests/test_e2_tarefa_do_dia.py:623`, `app/chamados/tests/test_e2_tarefa_do_dia.py:637`).

2. **Bloqueia — lavra do futuro continua aceita no passado.** `esta_stale` não exige `julgado_em <= agora`; sem expiração e sem marco posterior ao julgamento, devolve `False` mesmo com julgamento em 2036 e agora em 2026 (`app/chamados/veredito_lavrado.py:196`, `app/chamados/veredito_lavrado.py:203`). A carga aceita esse veredito como vigente (`app/chamados/services/tarefa_do_dia.py:297`). A própria fixture “fresco” grava `D+3650`, escondendo esse defeito temporal (`app/chamados/tests/test_e2_tarefa_do_dia.py:129`). Contraexemplo reproduzido sem banco por extração AST da função (`app/chamados/veredito_lavrado.py:187`).

3. **Fila — os relógios do juiz e da expiração não têm o mesmo contrato.** `expira_em` usa a data local recebida; `lavrar` fornece `timezone.localdate(p.julgado_em)` (`app/chamados/veredito_lavrado.py:115`, `app/chamados/veredito_lavrado.py:245`). Entretanto, o juiz efetivamente chamado por `lavrar` usa `_dentro_da_janela_viva`, que continua lendo `datetime.date.today()` (`app/chamados/veredito_lavrado.py:235`, `app/chamados/juizes.py:563`, `app/chamados/juizes.py:661`). Caso demonstrativo: SO em UTC, 21/11 01:30 UTC =20/11 22:30 SP: juiz usa janela de 21/11, expiração usa janela de 20/11 (`app/chamados/juizes.py:564`, `app/chamados/veredito_lavrado.py:117`). Fuso efetivo do SO em produção: [nao verificado]. O selo examina somente funções diretas nomeadas e não alcança esse helper (`app/chamados/tests/test_e2_tarefa_do_dia.py:743`).

4. **Fila — chamadores não migrados para `ate`.** Há dois: `_mdl_s2([colaborador.pk])` no fio e a chamada no teste de concordância (`app/chamados/services/fio.py:248`, `app/chamados/services/fio.py:249`, `app/chamados/tests/test_selo_concordancia_varredura_stale.py:58`). Não causam `TypeError`, porque `ate` continua opcional (`app/chamados/veredito_lavrado.py:125`). No fio, o resultado chega a `_sem_acao`, mas essa função ignora `marcos` e simplesmente lê o veredito, portanto a ausência de `ate` não muda essa decisão hoje; deixa cinco queries sem uso efetivo nessa decisão (`app/chamados/services/fio.py:284`, `app/chamados/services/raiox.py:152`, `app/chamados/services/raiox.py:174`, `app/chamados/services/raiox.py:183`). No teste, a varredura usa `ate=agora` e o comparador lê o banco inteiro com outro relógio, deixando de comparar precisamente o mesmo recorte temporal (`app/chamados/services/raiox.py:35`, `app/chamados/tests/test_selo_concordancia_varredura_stale.py:56`).

5. **Fila — migração de assinaturas realizada nos chamadores executáveis encontrados.** Produção: `lavrar` e backfill fornecem `hoje`; tarefa e auditor fornecem `marcos, agora` a `esta_stale`; tarefa, auditor e varredura fornecem `ate` (`app/chamados/veredito_lavrado.py:245`, `app/chamados/management/commands/backfill_veredito_lavrado.py:111`, `app/chamados/services/tarefa_do_dia.py:296`, `app/chamados/services/tarefa_do_dia.py:298`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1161`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`, `app/chamados/services/raiox.py:36`). Chamadas antigas nos testes novos são intencionais, sob `assertRaises(TypeError)` (`app/chamados/tests/test_e2_tarefa_do_dia.py:648`). Demais ocorrências textuais antigas identificadas são comentários/docstrings, não execução (`app/chamados/services/fio.py:814`, `app/core/portas.py:319`).

6. **Fila — mapa dos escritores e leitores dos marcos.** Todos os marcos abaixo são instantes de registro/alteração/trilha, não o dia do turno ou necessariamente o instante da ocorrência da batida; `marcos_do_lote` os agrega por colaborador sem limitar datas de células ao horizonte (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:165`, `app/chamados/veredito_lavrado.py:176`).

   - `Batida.criado_em`: `auto_now_add`, criado pela porta de batida (`app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`); leitor `marcos_do_lote` (`app/chamados/veredito_lavrado.py:151`).
   - `Batida.retratada_em`: porta `retratar_batida` usa `timezone.now()`; desfazer retratação limpa por `anotar` (`app/ponto/registro_batida.py:274`, `app/ponto/services/retratar_humano.py:207`); leitor (`app/chamados/veredito_lavrado.py:154`).
   - `CelulaDia.gerada_em`: `auto_now_add` (`app/escala/models.py:1702`).
   - `CelulaDia.regenerada_em`: regeneração e desfazer regeneração pela porta (`app/ponto/portas/celula.py:274`, `app/ponto/portas/celula.py:792`).
   - `CelulaDia.editada_em`: edição humana pela porta, com `agora=timezone.now()` (`app/ponto/portas/celula.py:598`, `app/ponto/portas/celula.py:607`).
   - `CelulaDia.veredito_em`: porta, `agora or timezone.now()` (`app/ponto/portas/celula.py:1032`).
   - `CelulaDia.julgada_em`: porta `carimbar`, com `agora` obrigatório recebido (`app/ponto/portas/celula.py:1047`, `app/ponto/portas/celula.py:1060`).
   - Os cinco marcos de célula são lidos conjuntamente (`app/chamados/veredito_lavrado.py:167`).
   - `PerguntaDisputa.materializada_em`: atribuição central `_escrever_carimbo`, com `quando` recebido ou lido na porta; descarimbo também limpa o campo (`app/chamados/models.py:2658`, `app/chamados/models.py:2683`, `app/chamados/models.py:2802`); leitor (`app/chamados/veredito_lavrado.py:176`).
   - `PerguntaDisputa.respondida_em`: resposta pelo relógio do ato; resposta automática/trilha usam instante recebido; carimbar pode preencher e desfazer/reabrir pode limpar/restaurar (`app/chamados/services/disputa_emissao.py:1340`, `app/chamados/services/disputa_emissao.py:1346`, `app/chamados/services/disputa_emissao.py:1516`, `app/chamados/services/disputa_emissao.py:1537`, `app/chamados/models.py:2691`, `app/chamados/models.py:2722`); leitor (`app/chamados/veredito_lavrado.py:180`).

   Com `ate`, cada campo usa limite inclusivo `<= ate`; sem `ate`, lê todos os valores atuais. Para invalidar a lavra, o marco precisa ser estritamente `> julgado_em` (`app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:140`, `app/chamados/veredito_lavrado.py:205`). O limite não reconstrói versões históricas sobrescritas: lê os campos atuais filtrados (`app/chamados/veredito_lavrado.py:167`).

7. **Fila — `janela_expira_em` tem escritores declarados e expiração inclusiva.** `lavrar` calcula e atribui a data e salva o trio; o backfill calcula separadamente, atribui o trio e persiste pela porta de lote; desfazer restaura a fotografia em memória (`app/chamados/veredito_lavrado.py:243`, `app/chamados/veredito_lavrado.py:247`, `app/chamados/management/commands/backfill_veredito_lavrado.py:111`, `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `app/chamados/management/commands/backfill_veredito_lavrado.py:143`, `app/chamados/veredito_lavrado.py:261`). Leitores principais: `esta_stale`, varredura e diagnóstico/recorte do backfill (`app/chamados/veredito_lavrado.py:198`, `app/chamados/services/raiox.py:42`, `app/chamados/management/commands/backfill_veredito_lavrado.py:121`). Expira no próprio dia (`<= hoje`), enquanto a janela viva do juiz inclui as duas pontas (`app/chamados/veredito_lavrado.py:198`, `app/chamados/juizes.py:589`). `expira_em` recebe uma **data local**; `esta_stale` recebe um **instante** e converte para data local; o backfill lê `hoje` e `agora` em chamadas separadas, potencialmente atravessando a meia-noite (`app/chamados/veredito_lavrado.py:109`, `app/chamados/veredito_lavrado.py:192`, `app/chamados/management/commands/backfill_veredito_lavrado.py:94`).

8. **Fila — pendência E1 não impede execução por assinatura.** `lavrar` continua lendo o relógio e está explicitamente excluído do selo como borda; `models.py` chama a assinatura preservada (`app/chamados/veredito_lavrado.py:227`, `app/chamados/veredito_lavrado.py:244`, `app/chamados/tests/test_e2_tarefa_do_dia.py:722`, `app/chamados/models.py:2709`). O fio continua chamando `marcos_do_lote` sem `ate`, aceito pelo valor padrão; o efeito atual é o descrito no achado 4 (`app/chamados/services/fio.py:249`, `app/chamados/veredito_lavrado.py:125`). Nenhum teste Django foi executado nesta auditoria; comportamento de banco/produção permanece [nao verificado].

## 07/11 08:52 · sessao b52aa7feb371 · - · - · origem=Ronald (tmux)

1. **B3 realizado — fila, sem bloqueio demonstrado neste recorte.** A carga chama o leitor único com cadastro recarregado, células e vínculos completos; `_tarefa_pura` recebe o resultado e retorna sem ações para `fora_vinculo` e isento. O juiz de dia sem trabalho continua delegado pela ponte do suprajuiz. **Caso:** demissão D−1, admissão D+1, isento e demissão D com/sem fato estão explicitamente confrontados por Caso13. `app/chamados/services/tarefa_do_dia.py:276`, `:315`, `:171`, `:174`, `:184`; `app/ponto/supra_juiz.py:65`; `app/chamados/tests/test_e2_tarefa_do_dia.py:555`, `:562`, `:569`, `:576`, `:584`.

2. **Todas as formas de `_celulas_da_ata` receberam o campo — fila.** Com lâmpadas, escreve `acesa_lavrada=l_.get('acesa')`; sem lâmpadas, cada célula agregada recebe `None`. Não existe ramo `sem_ancora` ou retorno antecipado correspondente nesta função; `sem_ancora` é transportado no dia. **Caso:** ata agregada mantém ignorância; ata com `acesa=None` produz `missing=False`, mas conserva `acesa_lavrada=None`. `app/escala/services/leitor_celula.py:114`, `:119`, `:137`, `:160`, `:174`, `:421`; `app/escala/tests/test_grade_alimentada.py:57`.

3. **Censo da lâmpada — fila.** O produtor `lampadas_do_dia` escreve True/None/False no ramo de DNA sem coluna, e valores booleanos nos demais ramos; a dispensa força True sem batida e o teto força None para marco pendente. `ata_do_dia` inclui a lista na ata. O novo campo `acesa_lavrada` é somente projeção em memória, lida pela tarefa e tratada como ignorância quando None. **Caso:** `acesa=True,luz=None,dispensada=...` significa dispensa, não batida ocorrida; a tarefa apenas cala a cobrança. `app/ponto/services/cartorio.py:638`, `:642`, `:644`, `:677`, `:689`, `:702`, `:752`, `:774`, `:881`; `app/escala/services/leitor_celula.py:137`; `app/chamados/services/tarefa_do_dia.py:81`, `:95`, `:211`, `:214`.

4. **Censo de `fora_vinculo` — fila.** O leitor escreve esse tipo a partir de admissão, demissão e fato lavrado; a tarefa, declaração, materialização e fechamento o consomem. **Caso:** admissão é inclusiva; antes dela um fato permite conservar o dia; demissão D sem fato é fora, D com fato permanece; depois da demissão o fato não vence a condição. Portanto “fato vence cadastro dos dois lados” não deve ser interpretado como permissão geral para fatos posteriores à demissão. `app/escala/services/leitor_celula.py:385`, `:389`, `:393`; `app/chamados/services/tarefa_do_dia.py:171`; `app/chamados/services/declaracao_texto.py:112`; `app/chamados/services/materializacao.py:239`; `app/ponto/services/fechamento.py:1036`.

5. **Censo de isenção — fila.** A fonte é `Colaborador.isento_ponto`; o juiz lê somente a flag. A tela Passe Livre grava pela porta de cadastro; a migration histórica também escreve a flag. O leitor publica `isento_ponto`, aplica o overlay de apresentação e a tarefa utiliza o valor publicado. **Caso:** o overlay pode mudar `missing` para False sem mudar `acesa_lavrada`; isso não cria cobrança porque a tarefa retorna antes pelo gate de isento. `app/colaboradores/models.py:294`; `app/core/constants.py:13`; `app/colaboradores/views.py:175`; `app/colaboradores/services/colaborador.py:55`; `app/colaboradores/migrations/0043_isento_ponto.py:13`; `app/escala/services/leitor_celula.py:360`, `:441`, `:445`; `app/escala/utils.py:858`; `app/chamados/services/tarefa_do_dia.py:174`.

6. **Igualdade alimentada: nenhum contraexemplo válido encontrado; selo incompleto — fila.** Com o mesmo cadastro, células completas e vínculos completos do mesmo estado, os caminhos convergem para o mesmo cálculo; o mapa é recortado à janela e a carga efetivamente fornece a lista inteira de vínculos. **Caso:** o selo cobre somente um vínculo, células existentes e demissão; não demonstra múltiplos vínculos, ausência de vínculo/célula ou admissão. Um contraexemplo baseado em dois vínculos com mesma `data_inicio` seria inválido: existe unicidade `(colaborador,data_inicio)`. `app/escala/services/leitor_celula.py:361`, `:364`, `:372`, `:406`, `:411`; `app/ponto/turnos.py:2988`; `app/escala/tests/test_grade_alimentada.py:26`, `:33`, `:44`; `app/escala/models.py:1004`.

7. **Caso13 e grade não passam por ausência total de exercício — fila.** Caso13 possui contra-casos que exigem cobrança, além dos casos que exigem silêncio; o selo da grade exige igualdade, ausência de consultas às duas tabelas e triestado explícito. Entretanto Caso13 verifica comportamento, sem impedir uma futura cópia da regra que produza os mesmos resultados. **Caso:** remover os gates atuais quebra os casos silenciosos; simplesmente calar tudo quebra `test_vinculo_vigente_segue_cobrado` e demissão com fato. `app/chamados/tests/test_e2_tarefa_do_dia.py:555`, `:576`, `:591`; `app/escala/tests/test_grade_alimentada.py:51`, `:52`, `:60`.

## 07/11 08:51 · sessao a5fd7da30510 · - · - · origem=Ronald (tmux)

- **Dono conclusivo: [nao verificado].** A L-099 cobra coerência determinística e idempotente com o cadastro existente; não permite atribuir a divergência ao cadastro apenas porque houve regeneração. (`app/docs/LEIS.md:122`)

- **Juiz exato, nesta ordem:** `folga_nao_definida` ou qualquer flag de ausência de vínculo → **CADASTRO**; `batida_fora_de_turno` → **BATIDA**; quantidade ímpar de `horas_do_dia` → **BATIDA**; `fase_deslocada` → **CADASTRO**; presença em Cadastro × Realidade → **CADASTRO**; resto → **ESTRUTURA**. (`app/ponto/management/commands/e6_oraculo.py:98`)

- **`furo_apurado=True`, `veredito='furo'`, `tem_entrada=False` e `tem_saida=False` não são argumentos desse juiz.** Assim, eles não fecham a classificação BATIDA. (`app/ponto/management/commands/e6_oraculo.py:53`)

- **Se forem realmente zero batidas no universo do E6**, zero é par e o ramo de paridade não resulta BATIDA; com todas as demais flags falsas, resulta ESTRUTURA. O código declara expressamente esse caso. (`app/ponto/management/commands/e6_oraculo.py:72`; `app/ponto/management/commands/e6_oraculo.py:106`; `app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:143`)

- **O previsto atual de 420, se produzido pela função indicada sobre os insumos reais, implica que a seleção atual encontrou vínculo e tipo de escala e que `eh_dia_trabalho` retornou exatamente `True`.** A função retornaria zero antes de chegar à geometria caso essas guardas falhassem. Isso não demonstra ausência de defeito de folga. (`app/escala/utils.py:2281`; `app/escala/utils.py:2287`; `app/escala/models.py:1241`)

**Falta exatamente, para reproduzir a decisão do juiz em 27/09:**

1. `horas_do_dia` efetivamente agrupadas pelo oráculo, derivadas de `batidas_apuraveis`, incluindo a janela ampliada; os indicadores da célula não substituem esse vetor. (`app/ponto/management/commands/e6_oraculo.py:141`)
2. `batida_fora_de_turno`, calculado com os turnos reais: exige realizado aberto e órfã real no universo apurável. (`app/ponto/management/commands/e6_oraculo.py:186`)
3. Resultado de `cadastro_do_dia_carregado(270, 2026-09-27)`: `folga_nao_definida`, `sem_vinculo_no_dia` e `sem_vinculo_em_dia_de_trabalho`. (`app/ponto/management/commands/e6_oraculo.py:172`; `app/ponto/supra_juiz.py:166`)
4. Pertinência de 27/09 à lista `dias_cadastro_x_realidade` do resultado do motor. (`app/ponto/management/commands/e6_oraculo.py:154`)
5. `fase_deslocada` pela lavra as-of usada no julgamento, ou o resultado falso dessa consulta. (`app/ponto/management/commands/e6_oraculo.py:164`)

Para auditar o cadastro bruto desses resultados: PK da geradora e suas datas de vigência; definição de folga do template e `FolgaDia` do mês. A seleção usa a geradora e rejeita datas fora de sua vigência; o defeito 6x1 depende de `folga_dia_semana` e existência de folga no mês. (`app/escala/alimentacao.py:269`; `app/escala/models.py:1241`)

## 07/11 08:51 · sessao 6689704095a9 · - · - · origem=Ronald (tmux)

- O escritor que corresponde à assinatura `dna_anterior` preenchido + `regenerada_em` + contador é `regenerar_celulas_vinculo`: ele reatribui **trabalha, escala_geradora e dna juntos**, guarda DNA anterior e atualiza carimbos. Isso não prova que os três valores tenham mudado: a condição aceita diferença em qualquer um deles. (`app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:271`)
- A regeneração por dia delega ao mesmo escritor; a descrição da auditoria distingue o rótulo “pela correcao do DIA”. (`app/ponto/portas/celula.py:734`)
- A auditoria pertinente usa **`acao='regenerar_celulas_vinculo'`, `modelo='EscalaColaborador'`, `objeto_id=ec.pk`**, com usuário opcional, motivo, janela e número de alterações; sua gravação é best-effort. (`app/ponto/portas/celula.py:334`, `app/ponto/portas/celula.py:342`, `app/ponto/portas/celula.py:350`)
- Os logs raspados contêm apenas `veredito_celula` de **33829 a 33834**; não contêm a regeneração de **33835**, nem a trilha do seu vínculo. Assim, **autor, disparador e ato concreto em 23/10 permanecem [nao verificado]**.
- Para obter a autoria, falta **ID do vínculo** e a linha completa de auditoria dessa ação na janela da regeneração: timestamp, `usuario`, `ator`, descrição e `valor_antes/valor_depois`. A vinculação ao usuário/processo é feita por `evento_kw` → `evento`, com fallback de request ou `sistema:<comando>`. (`app/core/observ.py:108`, `app/core/observ.py:117`, `app/core/observ.py:134`, `app/core/observ.py:146`)
- O marcador não certifica sozinho a última escrita de todos os campos: o gerador pode reescrever hoje/futuro sem atualizá-lo; o ramo fora da vigência grava `trabalha=False`/DNA sem carimbos; enriquecimento de DNA nulo também não os atualiza. (`app/ponto/portas/celula.py:237`, `app/ponto/portas/celula.py:859`, `app/ponto/portas/celula.py:865`)
- A edição humana normal deixaria `origem='editada'`, `editada_em` e `editada_por`; a reversão normal limparia `dna_anterior` e avançaria contador/carimbo. (`app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:786`)
- **Vigência não é campo da célula:** o signal observa `EscalaColaborador.data_inicio/data_fim` e chama regeneração quando mudam. Falta a trilha do vínculo para atribuir uma alteração histórica dessa vigência. (`app/escala/signals.py:25`, `app/escala/signals.py:56`, `app/escala/signals.py:71`)
- `dna_anterior` guarda somente a versão imediatamente anterior, sem valores anteriores de `trabalha`/geradora nem histórico completo. A mudança visível de pausa **19–20 → 20–21** não revela qual desses outros campos mudou. (`app/escala/models.py:1714`, `app/escala/models.py:1726`)

## 07/11 08:51 · sessao 0ffdc017cf7f · - · - · origem=Ronald (tmux)

1. **B4 realizado para o mutante declarado — fila.** O detector inclui `ponto_fechamentomensal`, a prova negativa o usa e o mutante injeta uma consulta real a `FechamentoMensal.objects.exists()`; a prova positiva exige encontrá-la. Não é comparação vazia: a fixture cria colaborador e vínculo antes da carga. Caso: `Caso10CompetenciaExportada`. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:77`, `:85`, `:507`, `:518`, `:533`, `:539`, `:544`. Execução Django [nao verificado].

2. **O selo de relógio admite leitura fora da borda — bloqueia a certificação ampla.** A detecção exige chamada sem argumentos **e sem keywords**, além de reconhecer apenas cinco nomes literais. Os contraexemplos `timezone.localdate(value=None)` e `clock = timezone.now; return clock()` retornaram `[]` quando executei somente o helper AST extraído, em memória. Ambos escapam ao selo; os mutantes existentes só exercitam grafias simples. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:724`, `:735`, `:737`, `:755`, `:760`. O helper também ignora funções que deixem de ser `FunctionDef` de primeiro nível: `:743`.

3. **Caso13 morde a regressão B3 comportamental — fila.** Os testes exigem razões específicas e ausência de ações para demissão anterior, admissão posterior e isenção; há contracaso com fato na demissão exigindo `perguntar`, além de vínculo vigente. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:555`, `:562`, `:569`, `:576`, `:584`, `:591`. A igualdade lote/casca compartilha o mesmo carregador e não demonstra, sozinha, uso do leitor único: `app/chamados/services/tarefa_do_dia.py:351`, `app/chamados/tests/test_e2_tarefa_do_dia.py:599`.

4. **O novo selo temporal cobre dois casos reais, mas não todo o contrato — fila.** Ele compara fato das 22h com perguntas às 21h/23h e usa instante UTC cuja data local ainda é D; essas expectativas distinguem retirada do limite temporal e retorno a `agora.date()`. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:623`, `:627`, `:630`, `:637`, `:641`. Não exercita nesse selo os demais marcos nem igualdade exata com `ate`: o fato alterado é apenas `CelulaDia.veredito_em`, em `:630`.

5. **Lavra futura continua aceita, inclusive pelas fixtures — bloqueia vigência histórica estrita.** A fixture geral grava `julgado_em=D+3650`; `esta_stale` não verifica `julgado_em > agora`, podendo devolver `False` para uma lavra que ainda não existia. Caso demonstrável pelo corpo: veredito `nao`, lavra futura, janela `None`, marcos `{}` ⇒ `False`; a carga a aceita em `_lavra`. Referências: `app/chamados/tests/test_e2_tarefa_do_dia.py:127`, `app/chamados/veredito_lavrado.py:196`, `:203`, `:207`, `app/chamados/services/tarefa_do_dia.py:297`.

6. **`test_grade_alimentada` não certifica a equivalência em todo caso — fila.** A fixture contém um único vínculo e apenas a forma de ata com `lampadas`; o teste cobre filtragem de célula externa, demissão e triestado, mas não múltiplos vínculos, colaborador sem vínculo, admissão ou ata agregada. Referências: `app/escala/tests/test_grade_alimentada.py:20`, `:26`, `:28`, `:33`, `:44`, `:57`. O resultado efetivo para esses casos adicionais cabe à auditoria do leitor [nao verificado neste subtrabalho].

7. **Não classificar a convivência dos cinco respondedores como juiz clandestino — fila E3 declarada.** O registro autoriza `_C19`, declara a pureza e mantém explicitamente os cinco respondedores até E3; a tabela repete o contrato. Referências: `app/core/juizes.py:150`, `:154`, `:285`; `app/docs/JUIZES.md:73`, `:80`. Para dia sem trabalho, a nova tarefa chama o adaptador que delega ao juiz registrado, sem copiar o predicado: `app/chamados/services/tarefa_do_dia.py:184`, `app/ponto/supra_juiz.py:65`, `:69`, `:71`; autoridade em `app/docs/JUIZES.md:59`.

8. **0514(3), universo da sombra inteiro, não está demonstrado — bloqueia sua certificação.** A API percorre `[ini,fim]` e a carga recebe colaboradores explicitamente; isso não prova que o chamador da sombra seleciona toda célula do horizonte. Referências: `app/escala/services/leitor_celula.py:375`, `:377`, `:444`, `app/chamados/services/tarefa_do_dia.py:340`. Não encontrei script/artefato da sombra na árvore pesquisada; seleção efetiva e execução do universo [nao verificado].

## 07/11 08:49 · sessao f13e743f1403 · V2-I2 · sonda · origem=Code

**27/09 é o candidato visível aos +420 minutos, mas a sonda não fecha a atribuição por dia nem o dono L-099.** Ela informa FM=10500 e soma dos DP motor=10500; os 10920 aparecem no enunciado, sem resultado novo por dia: todos os `DP_atual` estão `None`. [nao verificado]

**1) Dias e mudanças observáveis**

Na tabela, “anterior” significa exclusivamente `dna_anterior`, não o DNA usado para calcular o DP gravado. O modelo guarda apenas uma versão anterior, não toda a sequência de regenerações. (`app/escala/models.py:1714`)

| Dias | DNA anterior → atual, no trecho fornecido | DP gravado |
|---|---|---|
| **27/09** | 15–23, pausa **19–20 → 20–21** | **0**. [nao verificado] |
| 21, 22, 25, 26, 28 e 29/09; 02, 04, 05, 06, 09, 11, 12, 13, 16, 18, 19 e 20/10 | Mesma mudança de pausa; entrada e saída permanecem 15–23 | 420 em cada dia. [nao verificado] |
| 23 e 30/09; 07 e 14/10 | `marcos=null`, tipo **313** → 15–23, pausa 20–21 | 420 em cada dia. [nao verificado] |
| 24/09; 01, 08 e 15/10 | 15–23, pausa 19–20 → `marcos=null`, tipo **397** | 0, exceto **01/10=None**. [nao verificado] |
| 03, 10 e 17/10 | `marcos=null`, tipo **397** → 15–23, pausa 20–21 | 420 em cada dia. [nao verificado] |

**A mudança de pausa, isoladamente, acrescenta zero minutos:** tanto 19–20 como 20–21 descontam 60 de uma jornada de 480, resultando em 420. Essa é a aritmética implementada pelo previsto. (`app/escala/utils.py:2293`; `app/escala/utils.py:2326`)

Em **27/09**, portanto, o contraste relevante é **DP gravado=0 versus DNA atual com geometria de 420**; não é aumento da duração entre os dois DNAs. [nao verificado] Para afirmar que o previsto atual desse dia é 420, faltam `trabalha=True` e vínculo válido: a função verifica essas condições antes de calcular os marcos, e `eh_dia_trabalho` lê `cel.trabalha`. (`app/escala/utils.py:2281`; `app/escala/models.py:1094`)

Os horários de regeneração apresentados são:

| Células | Última regeneração registrada, UTC | Contador |
|---|---|---|
| **27/09** | **23/10/2026 13:04:42.382279** | 2. [nao verificado] |
| Demais dias, exceto os três abaixo | 23/10/2026, entre 13:04:42.372643 e 13:04:42.414014 | 2. [nao verificado] |
| 03/10 | 23/10/2026 16:49:42.903745 | 4. [nao verificado] |
| 10/10 | 23/10/2026 16:49:42.910592 | 4. [nao verificado] |
| 17/10 | 23/10/2026 16:49:42.917070 | 4. [nao verificado] |

Todas aparecem com `origem='gerada'` e `editada_em=None`; a sonda não identifica a pessoa responsável. [nao verificado] Isso é compatível com regeneração: esse escritor preserva esses campos e registra separadamente DNA anterior, horário e contador. (`app/ponto/portas/celula.py:263`; `app/ponto/portas/celula.py:271`)

**2) Escritores e histórico**

| Operação | Escritor efetivo e consequência |
|---|---|
| `gerar_celulas` | O comando reexporta a porta; `gerar_celulas_periodo` cria trabalha/geradora/DNA. (`app/ponto/management/commands/gerar_celulas.py:13`; `app/ponto/portas/celula.py:848`) |
| **Regeneração registrada nas células** | `regenerar_celulas_vinculo` copia DNA para `dna_anterior`, grava o novo, atualiza horário e incrementa contador. É o escritor correspondente aos marcadores apresentados. (`app/ponto/portas/celula.py:271`) |
| Regeneração de um dia | Delega ao mesmo escritor; os marcadores não distinguem esse chamador da regeneração por vínculo. (`app/ponto/portas/celula.py:734`) |
| Edição humana | `editar_dia` grava `origem='editada'`, `editada_em` e `editada_por`. Essa assinatura não aparece na sonda. (`app/ponto/portas/celula.py:599`) [nao verificado] |
| Outros caminhos | O gerador pode atualizar hoje/futuro sem atualizar os marcadores de regeneração; o ramo fora da vigência também altera trabalha/DNA sem esses marcadores. Logo, eles não certificam toda escrita posterior. (`app/ponto/portas/celula.py:865`; `app/ponto/portas/celula.py:237`) |
| Reversão | Restaura DNA anterior, mas **limpa** `dna_anterior` e avança contador/horário. (`app/ponto/portas/celula.py:783`) |

**Quem disparou cada alteração permanece desconhecido.** A regeneração recebe autor opcional e tenta registrá-lo na auditoria; a célula não tem campo `regenerada_por`. (`app/ponto/portas/celula.py:334`; `app/escala/models.py:1718`) A auditoria desses atos não foi fornecida. [nao verificado]

**Não foi possível conferir o `git log` do HEAD:** esta cópia não contém `.git`; presença, datas e ancestralidade dos commits no hash informado ficam [nao verificado]. A evidência documental local permite apenas estas distinções: [nao verificado]

- A **R4 de 27/09** corrigiu a leitura da **pausa do realizado** para célula com marcos nulos; preservou o fallback de `marcos_do_dia`. (`app/escala/models.py:1138`; `app/escala/models.py:1151`)
- O previsto atual ainda conserva esse fallback quando não encontra pausas; o código registra explicitamente a colisão R4 × F1. (`app/escala/utils.py:2307`; `app/escala/utils.py:2317`; `app/escala/utils.py:2335`)
- Há registro de alimentação `celulas/folgas` em **26/09**, incluindo correção de DNA alimentado que ficava invisível. (`app/escala/utils.py:2257`)
- A cura documentada especificamente como **PREVISTO** é de **02/10**, para descontar todas as pausas, com identificadores `fecb9a67` e `07d89f21`; sua inclusão no histórico do HEAD não pôde ser conferida. (`app/docs/TICKETS.md:246`) [nao verificado]

**3) Dono L-099**

**Dono conclusivo dos +420: [nao verificado].** A L-099 exige coerência com o cadastro existente; regeneração, sozinha, não prova cadastro errado nem defeito estrutural. (`app/docs/LEIS.md:122`)

O juiz existente usa folga indefinida, ausência de vínculo, batida fora de turno, paridade, fase deslocada e presença em Cadastro × Realidade; esses insumos não acompanham a sonda. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:113`) [nao verificado]

Para concluir faltam:

- **Previsto novo por dia**, com identificação da execução que produziu 10920, esclarecendo também `DP_prev=None` em 01/10. [nao verificado]
- **`trabalha`, geradora, vigências e DNAs completos**, para aplicar as guardas do previsto. (`app/escala/utils.py:2281`)
- **Trilha dos atos**, para identificar autor, motivo e versões intermediárias. (`app/escala/models.py:1714`; `app/ponto/portas/celula.py:334`)
- **Insumos do juiz de dono e histórico Git**, para fechar respectivamente L-099 e a relação com alguma cura commitada. (`app/ponto/management/commands/e6_oraculo.py:113`) [nao verificado]

## 07/11 08:47 · sessao 5a0fbffa6c33 · - · - · origem=Ronald (tmux)

| Operação | Escrita e prova |
|---|---|
| `gerar_celulas` | O command reexporta as funções da porta em `app/ponto/management/commands/gerar_celulas.py:10-13` e chama `gerar_celulas_periodo` em `:309-311`. |
| Nascimento da célula | `gerar_celulas_periodo` grava `origem='gerada'`, `trabalha`, geradora e DNA em `app/ponto/portas/celula.py:848-852`; `gerada_em` é `auto_now_add`, em `app/escala/models.py:1702`. A regeneração também pode criar célula ausente em `app/ponto/portas/celula.py:258-260`. |
| Reescrita registrada da sonda | `regenerar_celulas_vinculo` restringe a célula existente a `origem='gerada'` e exige alteração de trabalha/geradora/DNA em `app/ponto/portas/celula.py:263`; guarda o DNA anterior, troca o DNA, registra horário e incrementa contador em `:271-277`. Não altera `origem`, `gerada_em` ou `editada_em` nesse ramo, conforme os campos salvos em `:276-277`. |
| Regeneração por dia | `regenerar_celulas_dia` usa exatamente o escritor acima, com horizonte desabilitado, em `app/ponto/portas/celula.py:734-737`. Portanto os marcadores da célula não distinguem esse chamador do ato por vínculo. |
| Edição humana | `editar_dia` exige autor e motivo em `app/ponto/portas/celula.py:559-562`; grava `origem='editada'`, `editada_em`, `editada_por`, trabalha/geradora/DNA em `:599-609`, e trilha antes/depois em `:610-618`. A regeneração comum não alcança essa origem, pela condição em `:263`. |
| Reversão | `reverter_regeneracao_dia` restaura DNA anterior, **limpa** `dna_anterior`, incrementa contador e muda horário em `app/ponto/portas/celula.py:783-795`. A trilha recebe ambos os DNAs e autor em `:797-803`. |
| Outra escrita sem esses marcadores | No gerador, passado só recebe DNA se estiver nulo (`app/ponto/portas/celula.py:856-861`); hoje/futuro pode mudar trabalha/geradora/DNA **sem** guardar anterior ou atualizar regeneração (`:865-871`). O ramo fora de vigência da própria regeneração também muda trabalha/DNA sem esses marcadores (`:237-245`). |

`dna_anterior` é apenas a versão imediatamente anterior, e o histórico completo pertence ao `LogAuditoria`, conforme `app/escala/models.py:1714-1717`. Assim, `regeneracoes=2/4` não permite reconstruir as duas/quatro versões intermediárias.

A trilha da regeneração recebe `autor` opcional e registra ação `regenerar_celulas_vinculo`, vínculo, intervalo, motivo e quantidade em `app/ponto/portas/celula.py:331-349`. A célula não contém um campo `regenerada_por`: contém horário/contador/DNA anterior em `app/escala/models.py:1718-1729`, enquanto o campo de pessoa disponível é `editada_por` em `:1732-1735`. Quem executou os atos apresentados na sonda permanece `[nao verificado]`.

Há vários disparadores possíveis: alteração de vigência (`app/escala/signals.py:71-73`), alteração de âncora (`:117-119`) e edição de template (`app/escala/services/cadastro_tipo.py:307-320`, `:348-350`). Os dois signals e a propagação de template mostrados chamam a regeneração sem fornecer `autor`, conforme esses mesmos trechos; portanto até a trilha pode ter usuário nulo. A sonda não identifica qual desses disparadores produziu cada ato `[nao verificado]`.

_2691 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
