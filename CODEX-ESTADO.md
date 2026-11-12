# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 12:19** · publicado em **12/11/2026 12:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 12:19 · sessao 856fc304b1e9 · AUDITORIA-DE-TRAVAS-CORTE · verificador · origem=Code

REPROVADO

O delta cura o truncamento e reforça as transições dos casos 5 e 8. Persistem as divergências com o aval literal e a compatibilidade descoberta da cota antiga; além disso, um ramo do caso 6 ainda aceita aviso ausente. (`bin/disjuntor.py:218`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1185`; `bin/tests/test_disjuntor.sh:1222`, `bin/tests/test_disjuntor.sh:1250`, `bin/tests/test_disjuntor.sh:1306`)

Referências aos anexos contam desde a primeira linha de conteúdo. Execução local do selo e confirmação do intervalo pelo histórico Git: [nao verificado]. As mordidas abaixo são as do log fornecido.

**1. Destino de cada achado procedente da volta 2**

| Achado | O delta cura? | O selo morde? |
|---|---|---|
| Aviso truncado em nomes longos | **Sim.** DISJUNTOR/SOBE recebem `inteiro=True`; somente a linha livre final do SOBE fica limitada a 200 caracteres. (`bin/disjuntor.py:153`, `bin/disjuntor.py:218`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:1081`) | **Sim:** `B3-aviso-cortado`, `B3-sobe-cortado` e `B3-sobe-linha-sem-limite` caem no caso 6. (`item2v3_mutantes.log:30`, `item2v3_mutantes.log:45`, `item2v3_mutantes.log:60`) |
| Testemunha positiva da medida `lib['auto']` | Já estava curado; permanece exigindo trilha positiva, igualdade com o medidor e apresentação correspondente. (`bin/tests/test_disjuntor.sh:1284`, `bin/tests/test_disjuntor.sh:1292`) | Mutante continua declarado; sua execução nesta volta: [nao verificado]. (`bin/tests/test_disjuntor.sh:2036`) |
| Contrato FALHA × SOBE | Já estava conciliado: contrato pede SOBE e código produz SOBE. (`contrato_item2.txt:4`; `bin/disjuntor.py:1060`) | Caso 2 exige exatamente um SOBE; mutante específico FALHA/SOBE não consta entre os onze do item. (`bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Cota e medida com nomes distintos | Mantido: `auto_usada` governa a cota; `lib['auto']` conta eventos em 24 h. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:2029`, `bin/disjuntor.py:2033`, `bin/disjuntor.py:2058`) | Há verificações de ambas; a compatibilidade com a chave antiga continua sem caso/mutante específico. (`bin/tests/test_disjuntor.sh:1179`, `bin/tests/test_disjuntor.sh:1292`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Cota herdada em `r['auto']` ignorada | **Não.** O predicado continua lendo somente `auto_usada`; a migração existente trata o limiar, sem converter aquela chave. Existência desse estado em produção: [nao verificado]. (`bin/disjuntor.py:815`, `bin/disjuntor.py:832`, `bin/disjuntor.py:1185`) | Não há mordida demonstrada para esse caminho. (`bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Precedência das portas antigas | Já estava curado; mantém ordenação estável antes de AUTO. (`bin/disjuntor.py:1050`) | Caso 8 foi reforçado; mutante de precedência continua declarado, mas sua execução nesta volta: [nao verificado]. (`bin/tests/test_disjuntor.sh:1306`, `bin/tests/test_disjuntor.sh:2037`) |
| Disparo não condicionado literalmente a DESENHO | **Não.** Continua disparando pelo número de reprovações/voltas e retornando PARADA antes de examinar liberações. A classe só entra na porta posterior. (`aval_auditoria_de_travas_corte_1211.txt:4`; `bin/disjuntor.py:1031`, `bin/disjuntor.py:1040`, `bin/disjuntor.py:1074` [correção: validação da classe em `bin/disjuntor.py:774`]) | Não há mutante declarado que cobre essa divergência entre aval e contrato. (`contrato_item2.txt:2`; `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Segunda ocorrência pode sair pela porta antiga sem autoria Ronald | **Não.** ERRO-BOBO exige motivo, sem conferir autor; a cota restringe apenas AUTO. Isso preserva o contrato, mas mantém a divergência com o aval literal. (`contrato_item2.txt:2`; `aval_auditoria_de_travas_corte_1211.txt:4`; `bin/disjuntor.py:768`, `bin/disjuntor.py:1057`) | O caso 5 exige que essa porta continue liberando com cota gasta. (`bin/tests/test_disjuntor.sh:1227`, `bin/tests/test_disjuntor.sh:1230`) |
| Caso 5 podia passar sem provar liberação | **Sim.** Agora exige estado previamente disparado, LIBERA do tipo correto acrescentada e preservação da cota. (`bin/tests/test_disjuntor.sh:1220`, `bin/tests/test_disjuntor.sh:1228`, `bin/tests/test_disjuntor.sh:1232`) | **Sim:** `B3-c5-solta-sem-libera` passa a derrubar também o caso 5. Antes já derrubava o caso 6; não era sobrevivente da suíte inteira. (`item2v3_mutantes.log:67`, `item2v3_mutantes.log:73`) |
| Caso 8 podia passar sem segundo disparo/liberação final | **Sim.** Exige segundo disparo, evento AUTO e cota unitária finais. (`bin/tests/test_disjuntor.sh:1300`, `bin/tests/test_disjuntor.sh:1306`) | **Sim:** `B3-c8-sem-2o-disparo` passa de rc=0 para rc=8. (`item2v3_mutantes.log:81`, `item2v3_mutantes.log:89`) |
| Relógio fora da borda | **Não; permanece herdado.** Há `time.time()` dentro de `decide_b3` e `datetime.now()` dentro de `trilha`. (`bin/disjuntor.py:999`, `bin/disjuntor.py:217`) | Os cinco mutantes novos não atacam isso. (`bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2049`) |

**2. Quem escreve e quem lê; detalhe maior que 300**

| Campo/valor | Quem escreve/produz | Quem lê |
|---|---|---|
| Parâmetro `inteiro` | Padrão `False` na assinatura; `_anuncia` fornece o resultado de `ev in B3_AVISO_INTEIRO`. O espião encaminha `**kw`. (`bin/disjuntor.py:210`, `bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:1077`) | `trilha` escolhe corte em 300 ou ausência desse corte. O parâmetro não é persistido na linha. (`bin/disjuntor.py:217`, `bin/disjuntor.py:218`) |
| `B3_AVISO_INTEIRO` | Constante `('DISJUNTOR', 'SOBE')`. (`bin/disjuntor.py:153`) | `_anuncia`; os mutantes retiram o encaminhamento ou SOBE da tupla. (`bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2042`) |
| `B3_SOBE_LINHA` | Constante `200`. (`bin/disjuntor.py:154`) | Montagem do SOBE e assert do sufixo limitado; mutante remove o corte. (`bin/disjuntor.py:1062`; `bin/tests/test_disjuntor.sh:1278`, `bin/tests/test_disjuntor.sh:2043`) |
| Detalhe DISJUNTOR B3 | `decide_b3` monta obra, motivo e recado; `_anuncia → trilha` grava após persistência, normalizando espaços. (`bin/disjuntor.py:1036`, `bin/disjuntor.py:1038`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:217`) | `ler()` conta pelo prefixo; selo procura o aviso completo e observa evento/trava. (`bin/disjuntor.py:2059`; `bin/tests/test_disjuntor.sh:1234`, `bin/tests/test_disjuntor.sh:1265`, `bin/tests/test_disjuntor.sh:1086`) |
| Detalhe SOBE B3 | `decide_b3` monta obra, SHA, cota, instrução e linha limitada ao final; passa pela mesma porta de anúncio. (`bin/disjuntor.py:1060`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:1081`) | `conta()` e listas `sobe/sobe2` do selo. `ler()` não contabiliza SOBE. (`bin/tests/test_disjuntor.sh:1165`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1276`; `bin/disjuntor.py:2035`) |

**Os leitores identificados em `bin/` continuam compatíveis com o detalhe ampliado:**

- `ler()` percorre linhas completas, filtra pelo token do evento e conta DISJUNTOR usando `p[3]`; o sufixo longo não muda essa posição. (`bin/disjuntor.py:2039`, `bin/disjuntor.py:2041`, `bin/disjuntor.py:2060`)
- `_censo_24h()` também percorre linhas completas; seu regex ancorado descarta DISJUNTOR/SOBE. (`bin/disjuntor.py:2105`, `bin/disjuntor.py:2147`, `bin/disjuntor.py:2149`)
- ESTADO e CLI usam `ler/linha`. A referência do hook ao caminho apenas o exclui como prova de espera. (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`; `bin/disjuntor.py:2403`; `bin/hook_stop_fila1.py:666`)
- Os selos usam leitura integral por linha, `grep` ou `tail`, sem teto de 300. Isso inclui o selo de censo fora do diff. (`bin/tests/test_disjuntor.sh:96`, `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1236`; `bin/tests/test_censo_antes_da_vaga.sh:158`, `bin/tests/test_censo_antes_da_vaga.sh:525`)

**3. Segunda AUTO, DESENHO, rearme e reinício**

- **Segunda AUTO sozinha, com `auto_usada` preservada: não libera.** Registra SHA recusado em `auto_sobe`, deduplica SOBE e continua sem consumir a linha. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:1063`, `bin/disjuntor.py:1069`)
- **Com estado antigo apenas em `auto`: pode aceitar outra AUTO.** Ausência de `auto_usada` é lida como zero, conforme o próprio contrato. Ocorrência em produção: [nao verificado]. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:1065`; `contrato_item2.txt:4`)
- **`classe=DESENHO` não libera pela porta nova.** Um motivo dizendo “erro de desenho” sob rótulo FORMA pode passar: o reconhecedor valida o rótulo e motivo alfanumérico. Classificação real em produção: [nao verificado]. (`bin/disjuntor.py:148`, `bin/disjuntor.py:774`)
- **Rearme e reinício com JSON preservado não zeram a cota.** Rearme altera outros campos; o carregamento seguinte recupera o objeto persistido. Arquivo ausente inicia `{}`. (`bin/disjuntor.py:1148`, `bin/disjuntor.py:1150`, `bin/disjuntor.py:377`, `bin/disjuntor.py:380`)
- **Uma linha antiga ERRO-BOBO pode liberar com cota gasta**, sem comprovar autoria Ronald; isso não é uma segunda AUTO aceita. (`bin/disjuntor.py:768`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1057`)

**4. Espião do caso 5 de `b3_v4`**

**Sim, preserva a prova anterior:** tenta adquirir a trava por outro descritor, registra se estava ocupada e exige presença de DISJUNTOR/LIBERA/REARMA antes do `all(...)`. `**kw` é encaminhado intacto à função original. Não prova SOBE nem verifica diretamente o valor de `inteiro`. (`bin/tests/test_disjuntor.sh:1067`, `bin/tests/test_disjuntor.sh:1071`, `bin/tests/test_disjuntor.sh:1077`, `bin/tests/test_disjuntor.sh:1086`, `bin/tests/test_disjuntor.sh:1087`)

**5. Assert vazio e mordida dos mutantes**

**Casos 5 e 8:** os vazios apontados foram curados pelas testemunhas de transição descritas acima. (`bin/tests/test_disjuntor.sh:1222`, `bin/tests/test_disjuntor.sh:1228`, `bin/tests/test_disjuntor.sh:1306`)

**Caso 6: ainda há ramo vazio.** O retorno de `dispara('OBRA-F')` é ignorado; `ultima()` pode retornar `''`. Nesse caso, “`AUTO ja usada` ausente” passa, e `mensagem_b3` ainda ensina AUTOLIBERA sem exigir registro da obra. Portanto esse ramo aceita ausência de disparo/aviso F. As provas novas de textos longos exigem eventos positivos e não passam integralmente vazias. (`bin/tests/test_disjuntor.sh:1240`, `bin/tests/test_disjuntor.sh:1245`, `bin/tests/test_disjuntor.sh:1250`, `bin/tests/test_disjuntor.sh:1265`, `bin/tests/test_disjuntor.sh:1278`; `bin/disjuntor.py:1202`)

| Mutante novo | Mordida no log final |
|---|---|
| `B3-aviso-cortado` | Caso 6, rc=6. (`item2v3_mutantes.log:30`, `item2v3_mutantes.log:31`) |
| `B3-sobe-cortado` | Caso 6, rc=6. (`item2v3_mutantes.log:45`, `item2v3_mutantes.log:46`) |
| `B3-sobe-linha-sem-limite` | Caso 6, rc=6. (`item2v3_mutantes.log:60`, `item2v3_mutantes.log:61`) |
| `B3-c5-solta-sem-libera` | Casos 5/6, rc=5. (`item2v3_mutantes.log:73`, `item2v3_mutantes.log:74`) |
| `B3-c8-sem-2o-disparo` | Caso 8, rc=8. (`item2v3_mutantes.log:88`, `item2v3_mutantes.log:89`) |

**Nenhum dos cinco novos sobrevive no log; o controle passa.** A execução nesta volta dos seis antigos e de `b3_v4`: [nao verificado]. (`item2v3_mutantes.log:15`, `item2v3_mutantes.log:16`; `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2039`)

**Contrato:** continuam oito casos numerados, agora com mais cenários internos; há onze mutantes do item. “8 casos” permanece correto; “6 mutantes” virou divergência documental relevante para rastrear cobertura, sem alterar por si só a regra operacional. (`contrato_item2.txt:5`; `bin/tests/test_disjuntor.sh:1180`, `bin/tests/test_disjuntor.sh:1307`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`)

**6. Classes de reprovação**

| Classe | Resultado |
|---|---|
| Segundo escritor fora da porta | Não identificado para a cota operacional: acréscimo específico em `decide_b3`, persistência por `_com_estado`. (`bin/disjuntor.py:1065`, `bin/disjuntor.py:1089`, `bin/disjuntor.py:390`) |
| Relógio fora da borda | **Presente, herdado:** fallback em `decide_b3`; wrapper chega sem instante explícito; `trilha` captura seu próprio horário. (`bin/disjuntor.py:999`, `bin/disjuntor.py:1172`, `bin/disjuntor.py:217`; `bin/codex_pergunta.sh:378`) |
| Selo sem mordida | Nenhum sobrevivente entre os cinco novos demonstrados; permanece a checagem vazia de aviso F. Mordida atual dos seis antigos: [nao verificado]. (`item2v3_mutantes.log:30`, `item2v3_mutantes.log:45`, `item2v3_mutantes.log:60`, `item2v3_mutantes.log:73`, `item2v3_mutantes.log:88`; `bin/tests/test_disjuntor.sh:1250`) |
| Campo com dois significados | Não nos campos atuais: cota e medida permanecem distintas. A chave antiga continua sem conversão. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:2033`, `bin/disjuntor.py:2058`, `bin/disjuntor.py:815`) |
| Ordem de trava | Não identifiquei inversão introduzida: gravação e anúncio permanecem sob a trava do estado; wrapper/triagem tomam resposta antes de estado. (`bin/disjuntor.py:370`, `bin/disjuntor.py:391`, `bin/disjuntor.py:402`, `bin/disjuntor.py:405`, `bin/disjuntor.py:908`, `bin/disjuntor.py:940`; `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`) |

## 12/11 12:17 · sessao 9aa9d65cb110 · - · - · origem=Ronald (tmux)

Os achados persistentes da volta 2 **não foram curados por este delta**: o disparo continua condicionado apenas ao número de reprovações/voltas, e retorna PARADA nessa chamada; a classe só é examinada posteriormente na porta AUTOLIBERA. (`bin/disjuntor.py:1031`, `bin/disjuntor.py:1040`, `bin/disjuntor.py:774`)

A exclusividade “segunda ocorrência só Ronald” também permanece falsa pela porta antiga: `ERRO-BOBO OBRA-A: motivo` exige apenas motivo alfanumérico, sem autoria; essas candidatas precedem AUTO e a cota só restringe `tipo == 'auto'`. Isso é preservação expressa da porta antiga pelo contrato, mas continua divergindo da leitura literal do aval já apontada na volta 2. (`bin/disjuntor.py:768`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1057`; `contrato_item2.txt:2`; `aval_auditoria_de_travas_corte_1211.txt:4`)

Para a pergunta 3:

- **Estado atual com `auto_usada`: segunda AUTOLIBERA sozinha não libera.** Acrescenta SHA a `auto_sobe` uma vez, anuncia SOBE e continua sem consumir SHA; o único acréscimo a `auto_usada` fica depois dessa guarda. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:1063`, `bin/disjuntor.py:1065`, `bin/disjuntor.py:1069`)
- **DESENHO pela porta nova:** rótulo `classe=DESENHO` é recusado porque as classes aceitas são FORMA/ERRO-BOBO; o texto “erro de desenho” sob rótulo FORMA pode passar, pois o reconhecedor valida rótulo e presença de letra/dígito, não a natureza do defeito. Classificação real em produção: `[nao verificado]`. (`bin/disjuntor.py:148`, `bin/disjuntor.py:774`)
- **Rearme não zera cota:** altera apenas `rearmado`, `reprovados`, `codex`, `disparado`. A decisão subsequente continua lendo `auto_usada`. (`bin/disjuntor.py:1148`, `bin/disjuntor.py:1150`, `bin/disjuntor.py:1185`)
- **Reinício com JSON preservado não zera cota:** a transição carrega JSON existente e o grava inteiro; carregamento de consulta faz o mesmo. Arquivo ausente inicia `{}`; isto é perda/ausência de estado, não efeito necessário do reinício. (`bin/disjuntor.py:377`, `bin/disjuntor.py:380`, `bin/disjuntor.py:390`, `bin/disjuntor.py:411`, `bin/disjuntor.py:414`)
- **Compatibilidade herdada continua descoberta:** estado com `auto=['sha-antigo']` sem `auto_usada` é lido como cota livre, podendo aceitar nova AUTO; o predicado lê só `auto_usada`, a migração existente só trata limiar/reprovações e não converte `auto`. O contrato explicitamente chama ausência de `auto_usada` de zero; existência de tal estado em produção: `[nao verificado]`. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:815`, `bin/disjuntor.py:832`, `bin/disjuntor.py:1065`; `contrato_item2.txt:4`)

Classes da pergunta 6:

| Classe | Evidência |
|---|---|
| Segundo escritor da cota fora da porta | Não identificado no código operacional pesquisado: acréscimo específico só em `decide_b3`; persistência genérica por `_com_estado`. Os escritores adicionais em `bin/tests` são fixtures. (`bin/disjuntor.py:1065`, `bin/disjuntor.py:1089`, `bin/disjuntor.py:390`; `bin/tests/test_disjuntor.sh:1153`) |
| Relógio fora da borda | Persiste, herdado: `decide_b3` chama `time.time()` se não recebe instante; wrapper chama `b3_parada → estado_b3` sem instante. `trilha` também lê `datetime.now()`. Stop e PreToolUse fornecem instante capturado na borda. (`bin/disjuntor.py:999`, `bin/disjuntor.py:1172`, `bin/disjuntor.py:217`; `bin/codex_pergunta.sh:378`; `bin/hook_stop_fila1.py:1198`, `bin/hook_stop_fila1.py:1204`; `bin/disjuntor.py:1951`, `bin/disjuntor.py:1834`) |
| Campo com dois significados | Nos nomes atuais não: cota cumulativa `auto_usada` versus medida de LIBERA(auto) em 24h `lib['auto']`; compatibilidade da chave antiga segue sem conversão. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:2029`, `bin/disjuntor.py:2033`, `bin/disjuntor.py:2056`) |
| Ordem de trava | Não identifiquei inversão introduzida: estado é travado antes de ler/mutar/gravar/anunciar; anúncio ocorre antes de fechar FD. Wrapper e triagem tomam resposta antes de estado. (`bin/disjuntor.py:370`, `bin/disjuntor.py:385`, `bin/disjuntor.py:391`, `bin/disjuntor.py:402`, `bin/disjuntor.py:405`; `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`; `bin/disjuntor.py:908`, `bin/disjuntor.py:940`) |

Cura efetiva do aviso truncado: `_anuncia` passa `inteiro=True` para DISJUNTOR/SOBE; `trilha` deixa de aplicar corte de 300 nesses chamados; SOBE preserva instrução antes da linha livre, e somente essa linha fica limitada a 200. (`bin/disjuntor.py:153`, `bin/disjuntor.py:154`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:218`, `bin/disjuntor.py:1060`, `bin/disjuntor.py:1062`)

## 12/11 12:17 · sessao 5f42930059da · - · - · origem=Ronald (tmux)

- **Espião `b3_v4` caso 5 preservado.** `**kw` segue intacto para `orig`; a tentativa de `flock(LOCK_EX|LOCK_NB)` em descritor separado continua detectando trava ocupada. O assert exige presença dos três eventos antes de `all(...)`, impedindo `all([])`. Prova DISJUNTOR/LIBERA/REARMA sob trava; SOBE não está no filtro. (`bin/tests/test_disjuntor.sh:1067`, `bin/tests/test_disjuntor.sh:1071`, `bin/tests/test_disjuntor.sh:1077`, `bin/tests/test_disjuntor.sh:1086`, `bin/tests/test_disjuntor.sh:1087`)

- **Caso 5 corrigido:** controles exigem estado previamente disparado, incremento LIBERA do tipo certo e cota apropriada; terceiro controle exige preservação exata da cota. Lista obrigatoriamente contém três resultados. (`bin/tests/test_disjuntor.sh:1220`, `bin/tests/test_disjuntor.sh:1222`, `bin/tests/test_disjuntor.sh:1228`, `bin/tests/test_disjuntor.sh:1232`)

- **Caso 8 corrigido:** segundo disparo deve retornar verdadeiro, AUTO final deve produzir evento e cota unitária. (`bin/tests/test_disjuntor.sh:1300`, `bin/tests/test_disjuntor.sh:1303`, `bin/tests/test_disjuntor.sh:1306`)

- **Caso 6 tem ramo residual vazio:** ignora retorno de `dispara('OBRA-F')`; `ultima` pode devolver `''`, e o assert aceita ausência de aviso porque só exige ausência da marca de cota gasta, além da instrução em `mensagem_b3`, que funciona mesmo sem registro da obra. Assim pode passar sem disparo/aviso F. As novas provas longas, entretanto, exigem disparos e SOBE positivos; não passam integralmente vazias. (`bin/tests/test_disjuntor.sh:1240`, `bin/tests/test_disjuntor.sh:1245`, `bin/tests/test_disjuntor.sh:1250`, `bin/disjuntor.py:1202`, `bin/disjuntor.py:1205`, `bin/tests/test_disjuntor.sh:1265`, `bin/tests/test_disjuntor.sh:1278`)

- **Inventário:** seguem oito casos numerados; há onze mutantes item 2, seis antigos e cinco novos. Logo “8 casos” permanece correto; “6 mutantes” está desatualizado, divergência documental sem alteração das regras operacionais. (`bin/tests/test_disjuntor.sh:1180`, `bin/tests/test_disjuntor.sh:1307`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2039`, `bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2049`; `contrato_item2.txt:5`)

- **Mordidas:** o log anexado mostra os cinco novos mutantes derrubados — três no caso 6, um no 5 e um no 8 — e controle rc=0; execução atual dos seis antigos e de `b3_v4`: `[nao verificado]`. O harness exige âncora única, controle sem mutação e falha se o caso aceitar a mutação. (`bin/tests/test_disjuntor.sh:1715`, `bin/tests/test_disjuntor.sh:1719`, `bin/tests/test_disjuntor.sh:1723`, `bin/tests/test_disjuntor.sh:1741`; localizações exatas no log anexado: `[nao verificado]`)

## 12/11 12:17 · sessao ad20865149a1 · - · - · origem=Ronald (tmux)

Inventário e compatibilidade dos leitores, por inspeção somente leitura:

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `inteiro` | Padrão `False` na assinatura; `_anuncia` passa `ev in B3_AVISO_INTEIRO`. O espião do selo recebe e encaminha `**kw`. (`bin/disjuntor.py:210`, `bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:1067`, `bin/tests/test_disjuntor.sh:1077`) | `trilha` escolhe o limite `None` ou `300`; nenhum outro consumidor semântico encontrado. (`bin/disjuntor.py:218`) |
| `B3_AVISO_INTEIRO` | Tupla constante `('DISJUNTOR', 'SOBE')`. (`bin/disjuntor.py:153`) | `_anuncia` determina a opção de escrita; mutantes removem o encaminhamento ou o SOBE. (`bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2042`) |
| `B3_SOBE_LINHA` | Constante `200`. (`bin/disjuntor.py:154`) | Construção de SOBE corta somente o texto livre final; selo exige linha maior que o limite e sufixo cortado. (`bin/disjuntor.py:1062`; `bin/tests/test_disjuntor.sh:1278`, `bin/tests/test_disjuntor.sh:1279`, `bin/tests/test_disjuntor.sh:2043`) |
| Detalhe DISJUNTOR B3 | `decide_b3` produz prefixo, motivo e `_recado_auto`; `_anuncia` entrega a `trilha`, que normaliza espaços e grava uma linha. (`bin/disjuntor.py:1036`, `bin/disjuntor.py:1038`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:217`) | `ler`, auxiliares de teste `_tem`/`ultima`, espião B3 v4. (`bin/disjuntor.py:2059`; `bin/tests/test_disjuntor.sh:96`, `bin/tests/test_disjuntor.sh:1234`, `bin/tests/test_disjuntor.sh:1086`) |
| Detalhe SOBE | `decide_b3` produz obra, SHA, cota e instrução, seguidos de `ln[:200]`; mesma porta de anúncio/escrita. (`bin/disjuntor.py:1060`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:1081`) | Selo `conta` e listas `sobe`/`sobe2`; `ler` não enumera SOBE, `_censo_24h` o descarta. (`bin/tests/test_disjuntor.sh:1165`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1276`; `bin/disjuntor.py:2035`, `bin/disjuntor.py:2105`, `bin/disjuntor.py:2148`) |

**Todos os leitores diretos identificados em `bin/` suportam detalhe maior que 300:** a escrita continua em uma única linha; nenhuma leitura identificada limita bytes/caracteres a 300. (`bin/disjuntor.py:217`, `bin/disjuntor.py:2039`, `bin/disjuntor.py:2147`; `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1236`)

- `ler()` percorre linhas completas, separa tokens e usa data/hora/evento; para DISJUNTOR conta `p[3]`, portanto o sufixo ampliado não muda a leitura. SOBE não participa da enumeração. (`bin/disjuntor.py:2035`, `bin/disjuntor.py:2040`, `bin/disjuntor.py:2044`, `bin/disjuntor.py:2059`)
- `_censo_24h()` percorre linhas completas; o regex ancorado aceita apenas `CENSO ...` e `NEGOU DISJUNTOR CENSO:`, portanto ignora os dois eventos ampliados. (`bin/disjuntor.py:2105`, `bin/disjuntor.py:2146`, `bin/disjuntor.py:2148`)
- `gerar_estado.py` e CLI `--linha` consomem a derivação `ler/linha`, sem leitor alternativo de detalhe. (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`; `bin/disjuntor.py:2402`, `bin/disjuntor.py:2403`)
- Os leitores do selo disjuntor usam `grep`, `tail`, busca por substring ou leitura integral por linha; o espião observa evento/trava e encaminha o detalhe. (`bin/tests/test_disjuntor.sh:96`, `bin/tests/test_disjuntor.sh:97`, `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1236`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1276`, `bin/tests/test_disjuntor.sh:1285`, `bin/tests/test_disjuntor.sh:1705`)
- O outro selo que lê diretamente a trilha é `test_censo_antes_da_vaga.sh`: `grep`/`tail` procuram eventos CENSO; o filtro seleciona os eventos antes de `tail`. Não depende do comprimento do detalhe B3. (`bin/tests/test_censo_antes_da_vaga.sh:114`, `bin/tests/test_censo_antes_da_vaga.sh:158`, `bin/tests/test_censo_antes_da_vaga.sh:249`, `bin/tests/test_censo_antes_da_vaga.sh:275`, `bin/tests/test_censo_antes_da_vaga.sh:314`, `bin/tests/test_censo_antes_da_vaga.sh:525`, `bin/tests/test_censo_antes_da_vaga.sh:543`)
- A referência do hook a `disjuntor.log` apenas o inclui entre arquivos que não provam espera; não interpreta seu detalhe. (`bin/hook_stop_fila1.py:664`, `bin/hook_stop_fila1.py:666`)

**Espião do caso 5 B3 v4:** continua provando presença de DISJUNTOR/LIBERA/REARMA e que a trava estava ocupada em todas essas chamadas. A exigência de conjunto exato impede o `all([])`. `**kw` apenas preserva a assinatura nova e chega à função original; o teste não verifica diretamente o valor de `inteiro`, obrigação coberta pelos novos casos de texto longo. (`bin/tests/test_disjuntor.sh:1067`, `bin/tests/test_disjuntor.sh:1071`, `bin/tests/test_disjuntor.sh:1077`, `bin/tests/test_disjuntor.sh:1086`, `bin/tests/test_disjuntor.sh:1087`, `bin/tests/test_disjuntor.sh:1252`, `bin/tests/test_disjuntor.sh:1278`)

## 12/11 12:11 · sessao ccf3a0772d66 · SAIDA-NOTURNA-NO-DIA-CERTO · censo · origem=Code

Critério: censo estático dos caminhos operacionais, templates e comandos. Testes e cópias históricas não entram nas contagens; mecanismos genéricos e ferramentas da sombra aparecem separados. A identificação do hash e os estados de produção de #51467/#56641 são os fornecidos no texto; confirmação independente: `[nao verificado]`. [nao verificado]

**1. Escritores de `PerguntaDisputa.pergunta`**

São **oito expressões de interpolação de data: quatro `.format` e quatro `.replace`, em sete locais lógicos**. Os dois ramos de `texto_pergunta_com_data` contam como duas expressões, uma função (`app/chamados/catalogo/query.py:64,66`).

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/chamados/catalogo/query.py:64` | Substitui `{data}` pelo ISO convertido para `dd/mm/aaaa`. | Data de `contexto_disputa[motivo]['data']`, lida em `:58,59`; não deriva o dia civil do marco. Chamadores: `app/chamados/services/disputa_emissao.py:345,354`. |
| `app/chamados/catalogo/query.py:66` | Na exceção, substitui `{data}` pela string original. | Mesma origem; nesse ramo não valida nem converte a data. |
| `app/chamados/services/disputa_emissao.py:879` | Formata pergunta para cada marco faltante. | Usa `dia` da tupla de `_marcos_faltantes`, exigindo igualdade com `data_evento` em `:873,874`. A tupla usa **data da célula**, inclusive quando a célula é a da véspera: `app/chamados/services/declaracao_texto.py:109,139`. |
| `app/chamados/services/disputa_emissao.py:1151` | Formata pergunta de intervalo. | Usa `_m['data']` de `chamado.contexto_json.marcos_faltantes`, convertido em `:1143,1148`. Um produtor comprovado grava ali o **dia civil do marco encadeado**, potencialmente D+1: `app/ponto/management/commands/detectar_intervalo_ausente.py:77,78,82`. |
| `app/chamados/services/disputa_emissao.py:1208` | Formata pergunta do grupo com batida de origem. | Usa `data_evento` resolvido em `:1205`: **dia do turno da batida**, pelo resolvedor em `app/escala/servico_jornada.py:495,509`. |
| `app/chamados/services/disputa_emissao.py:1246` | Formata pergunta sem batida de origem. | Usa `_ddc(ch.contexto_json)`, em `:1232`. A hierarquia aceita `data_turno`, `data_batida`, data de `entrada_timestamp`, `pendentes` e `horario_previsto`: `app/chamados/data_evento.py:16,27,31,39`. Não tem uma única origem temporal. |
| `app/chamados/services/declaracao_texto.py:374` | Substitui `{data}` ao criar pergunta da declaração. | Usa `dia` escolhido pelo casamento da hora em `:341,345`; origem na data da célula de `_marcos_faltantes`, em `:109,139`. |
| `app/chamados/services/declaracao_texto.py:664` | Substitui `{data}` ao aplicar proposta textual. | Usa `p['dia']`; proposta deriva de `_marcos_faltantes` e `_casar_hora` em `:615,625,628,635`. |
| `app/chamados/services/acoes_disputa.py:265` | Formata confirmação administrativa com `% label_motivo`. | **Sem data no texto.** Cria conversa em `:262,268`; a conversa recebe dia civil da emissão em `app/chamados/models.py:2802`. |
| `app/chamados/services/disputa_emissao.py:1374` | Copia texto de catálogo da âncora ou literal de fallback. | **Sem interpolação de data.** Cria conversa `ancora_ausente`, tipo `data_hora`, em `:1369,1377`. |
| `app/chamados/services/disputa_emissao.py:440,447` | Encaminha `texto` para `abrir_conversa` ou `abrir`. | O texto já foi produzido em `:345,354`; a data do nascimento passa por outra derivação em `:371,424,451`. Não recompõe o texto depois disso. |
| `app/chamados/services/disputa_emissao.py:840` | `_criar_pergunta_se_couber` encaminha pergunta e data recebidas. | Não reformata. |
| `app/chamados/models.py:3015,3017,3018` | `_nascer` instancia com `**campos`, atribui a data e insere. | Persistência comum do texto recebido. No reuso retorna a pergunta existente em `:3009`, sem reescrever seu texto. |
| `app/chamados/catalogo/perguntas.py:49`; `app/chamados/models.py:3566` | Definem “Confirme o horário da saída no dia {data}”. | São **templates**, não formatadores. A referência raspada `models.py:3564-3565` não identifica o formatador nesta cópia. |
| `bin/sombra_regras_pessoais.py:85`; `bin/sombra_mascara_pessoal.py:54,55,57,62,64` | Na cópia sombra, lê, mascara e grava `pergunta` por `bulk_update`. | Escritor adicional fora da emissão do domínio; não projeta nenhuma data. Transformação definida em `bin/sombra_regras_pessoais.py:29,33`. |

**2. Leitores de `PerguntaDisputa.pergunta`**

Há **20 ocorrências explícitas de leitura do campo: 14 Python e seis templates**, discriminadas abaixo. Excluir `__str__` e o comando smoke produz 18; esse recorte deixa leitores de fora (`app/chamados/models.py:3547`; `app/chamados/management/commands/smoke_chamados.py:108`). Além dessas 20, existem leitura genérica, leitores da sombra e propagação indireta.

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/api/views.py:1155` | API de chamados copia `p.pergunta`. | Texto cru; data enviada separadamente em `:1153,1154`. |
| `app/api/views.py:2551` | API de perguntas do app monta `_item`. | Texto cru para pendentes/histórico. |
| `app/api/views_mensageria.py:564` | API da fila de validação copia pergunta. | Texto cru; `data_evento` separado em `:568`. |
| `app/ponto/services/dia_do_colab.py:69` | Consulta do dia entrega pergunta. | Texto cru; seleção por `data_evento=data` em `:65`. |
| `app/chamados/views_cobrar.py:291` | Monta amostra de validação em lote. | Texto cru. |
| `app/chamados/canal.py:81` | Monta pergunta muda com resposta no fio. | Texto cru; correlação por colaborador/data em `:71,79`. |
| `app/chamados/services/explicador.py:71` | Monta dict explicativo. | Conserva texto cru; estado, ação e dia ficam em campos separados. |
| `app/chamados/services/disputa_emissao.py:1549` | Copia pergunta para ocorrência de resposta. | Envolve texto cru com tag/resposta; grava em `:1555,1556`. |
| `app/chamados/services/validacao.py:599` | Copia pergunta para ocorrência de recusa. | Envolve texto cru com autor, resposta anterior e motivo. |
| `app/chamados/templatetags/chamados_extras.py:11` | Detecta pergunta “cega” com regex de data. | Inspeciona o texto gravado; não o recompõe. |
| `app/chamados/management/commands/auditar_disputas.py:74` | Procura data no texto. | Regex sobre o campo cru. |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:33` | Procura ocorrência correspondente. | Usa o texto cru como substring `Pergunta: ...`. |
| `app/chamados/management/commands/smoke_chamados.py:108` | Verifica `{data}` residual. | Leitura operacional do campo persistido. |
| `app/chamados/models.py:3547` | `__str__`. | Prefixa motivo e corta texto cru em 60 caracteres. |
| `app/templates/chamados/partials/_card_atendimento.html:93` | Cartão da resposta. | Exibe texto cru. |
| `app/templates/chamados/partials/_card_atendimento.html:175` | Cartão do histórico. | Exibe texto cru. |
| `app/templates/chamados/partials/_pergunta_card.html:28` | Cartão de supervisão/Central. | Exibe texto cru. |
| `app/templates/chamados/partials/modal_fio.html:160` | Modal do fio. | Exibe texto cru; data do fato separada em `:167`. |
| `app/templates/chamados/partials/_lote_preview.html:24` | Prévia de lote. | Texto cru com `truncatechars:60`. |
| `app/templates/colaboradores/partials/painel_colaborador.html:197` | Painel do colaborador, bloco “no app”. | Exibe texto cru. |
| `app/chamados/services/foto_e4b.py:251,272` | **Leitor genérico:** fotografa todos os campos concretos. | Inclui `pergunta` por `value_from_object`; não renderiza nem recompõe. |
| `bin/sombra_mascara_pessoal.py:54`; `bin/sombra_regras_pessoais.py:85` | **Sombra:** lê para mascarar. | Não é apresentação da pergunta. |
| `bin/sombra_selo_pessoal.py:74,77`; `bin/sombra_regras_pessoais.py:85` | **Sombra:** SQL confere se o texto foi mascarado. | Leitura adicional por regra dinâmica de campos. |
| `app/templates/chamados/partials/modal_fio.html:439` | **Indireto:** exibe dict do explicador. | Texto copiado por `app/chamados/services/explicador.py:71`, truncado. |
| `app/chamados/views_cobrar.py:180,186` | **Indireto:** repassa dict do canal. | Origem em `app/chamados/canal.py:81`. |
| `mensageria/nucleo/ferramentas.py:119` | **Indireto:** copia pergunta recebida da API. | Não recompõe. |
| `app/relatorios/pdf_chamados.py:145,147` | **PDF indireto:** imprime `fup.conteudo`. | Pode imprimir a pergunta copiada pelas ocorrências de resposta/recusa; limita conteúdo a 1.000 caracteres. Fontes dessas cópias: `app/chamados/services/disputa_emissao.py:1549,1556`; `app/chamados/services/validacao.py:598,600`. |
| `app/chamados/catalogo/query.py:45,52,64,66` | Existe `texto_pergunta_com_data`. | É interpolador do **catálogo no emissor**, não projeção comum dos leitores. Os acessos acima leem diretamente o campo ou sua cópia. |
| Renderizador específico do cliente nativo/PWA | Execução efetiva e eventual recomposição no cliente. | `[nao verificado]`; os pontos comprovados nesta cópia são APIs e templates acima. |

**3. `VIA_FATO_OCUPADO`: escritores, leitores e colisão**

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/chamados/models.py:2488` | Declara `'fato_ocupado'`. | Definição da constante. |
| `app/chamados/services/materializacao.py:1095,1097,1099,1105` | Único chamador operacional encontrado que solicita gravação dessa via. | Predicado exato: `p.materializada_em is None and codigo == 'COLISAO'`. **Não consulta o marco cobrado nem sua lâmpada.** |
| `app/chamados/catalogo/recusa.py:66,79,80,81,82,83` | Classifica `COLISAO` pela substring `colisao tipo-oposto`. | Classificação textual, sem comparação de marcos. |
| `app/chamados/models.py:3179,3186,3187,3189,3191` | `carimbar_identidade` persiste a solicitação. | Grava via, instante, autor e, quando fornecido, motivo da recusa. Não pergunta ao C6 nem limpa resposta/validação. |
| `app/ponto/registro_batida.py:94,138,139,145,150` | Produz a recusa original de colisão. | Existe batida **oposta, não retratada**, na janela inclusiva `timestamp ± g_op`, e a candidata não casa com marco próprio. Sem autoridade humana, recusa. Não recebe a pergunta nem consulta sua lâmpada. |
| `app/ponto/registro_batida.py:21,28,32` | Define a janela de colisão. | 300 segundos com escala/marcos no dia civil local da candidata; senão 60 segundos. |
| `app/ponto/registro_batida.py:35,42,49` | `_casa_marco_proprio` excepciona a colisão por geometria. | Procura algum marco previsto do mesmo tipo, no dia local ou véspera, a até cinco minutos. Portanto a origem não olha **somente** vizinhança, mas tampouco identifica o marco da `PerguntaDisputa`. |
| `app/chamados/models.py:2511` | Inclui a via na identidade. | Consumidores na tabela 4. |
| `app/chamados/models.py:2535` | Define descrição de apresentação. | “Fato já ocupado (batida existente no slot)”. |
| `app/chamados/models.py:3738`; `app/chamados/juizes.py:679,689,691` | Inclui em `VIAS_SEM_TESTEMUNHA`; juiz reconhece a família e a identidade carimbada. | Família distinta de `VIAS_DE_IDENTIDADE`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:47,48,460,746` | Família sem testemunha participa do piso I6 e de sua apresentação. | Leitura indireta da via por pertencimento ao conjunto. |
| `app/templates/chamados/partials/_card_atendimento.html:186` | Trata literalmente `fato_ocupado` ou `paridade_recusada`. | Exibe “já havia batida registrada”. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40,42` | Permite apenas vias vazia/legado/nula. | `fato_ocupado` fica fora desse recorte por filtro genérico. |
| `app/chamados/migrations/0040_a27_recusa_motivo.py:21`; `0041_b2_via_legado_sem_lastro.py:16`; `0042_hx_via_ausencia_dia.py:16`; `0046_via_celula_absolveu.py:16`; `0048_via_competencia_trancada.py:15`; `0051_decisao_pergunta.py:111` | Preservam o valor nos `choices` históricos. | Declaração de esquema; não são novos escritores de registros com essa via. |

**4. Leitores de `VIAS_DE_IDENTIDADE` e efeito de retirar somente `fato_ocupado`**

São **quatro acessos diretos à tupla e cinco consumidores operacionais das funções de acesso**, além da definição (`app/chamados/models.py:3090,3096,3119,3179,3348`; `app/chamados/juizes.py:689,1427`; `app/chamados/services/relavra_via.py:181`; `app/chamados/management/commands/auditar_invariantes_chamados.py:195`).

| Sítio arquivo:linha | O que faz | Observação: retirada somente da tupla |
|---|---|---|
| `app/chamados/models.py:2511` | Fonte: admin, duplicata, fato ocupado, paridade recusada, motivo retratado. | Passaria de cinco para quatro valores. |
| `app/chamados/models.py:3090` | `vias_de_identidade()` devolve a tupla. | Deixaria de devolver fato ocupado. |
| `app/chamados/models.py:3096` | `vias_que_encerram_por_identidade()` acrescenta competência trancada. | Deixaria de devolver fato ocupado; conservaria tranca. |
| `app/chamados/models.py:3119,3140` | `carimbar` rejeita vias da família de identidade. | Fato ocupado passaria a ser aceito por essa porta e submetido ao C6. |
| `app/chamados/models.py:3179,3180`; `app/chamados/services/materializacao.py:1105` | A porta atual exige identidade. | **O chamador atual de colisão passaria a levantar `ValueError` antes de gravar.** |
| `app/chamados/models.py:3348,3351,3362` | `q_vivas` exclui identidade materializada, condicionado aos demais predicados. | Deixaria de excluir fato ocupado por identidade; resposta, validação e célula continuariam condicionando o resultado. |
| `app/chamados/juizes.py:679,689,691,693,698,705` | Juiz do lastro absolve identidades carimbadas. | Deixaria de absolver sempre por identidade. Contudo a via continuaria em `VIAS_SEM_TESTEMUNHA`, por `app/chamados/models.py:3738`, e poderia absolver por ausência de dia, janela, classificação do dia ou exceção. Não passaria automaticamente a exigir a lâmpada específica. |
| `app/chamados/juizes.py:1427,1428` | C6 encerra identidade materializada. | Seguiria aos degraus de lâmpada, célula, lei e lastro em `:1435,1452,1454,1472`. **Validação continuaria matando antes**, em `:1391,1392`; resposta pode retornar antes em `:1407,1419`. |
| `app/chamados/services/relavra_via.py:181,236,242,246` | Filtro de carimbo desfazível exclui identidades. | Fato ocupado se tornaria elegível; só cairia após releitura e C6 vivo. Validação ainda pode impedir isso. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:195,202,206` | Contraprova F1T classifica o degrau da morte. | Deixaria de rotular “via de identidade”; se ainda morta, cairia em outra classificação; se viva, entraria no alarme. |

**5. `fato_do_marco_no_espelho`: predicado, universo, chamadas e D+1**

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/chamados/juizes.py:922,941,943` | Mapeia `saida_sem_entrada→E/hi`, `orfao_14h→S/hf`, `intervalo_saida→S/hii`, `intervalo_volta→E/hfi`. | Motivo sem mapeamento retorna `None`. |
| `app/chamados/juizes.py:944,948,965,966` | Lê `p.celula`, `cel.dna.marcos` e `cel.ata.lampadas`. | Não calcula timestamp, não lê resposta nem usa `data_evento` para conferir a identidade da célula. |
| `app/chamados/juizes.py:956,960` | Para intervalo, reúne as posições correspondentes de todas as pausas do DNA. | Cobra AND das pausas; não escolhe uma única pausa pela resposta. |
| `app/chamados/juizes.py:962,963,968,969,971` | Procura lâmpadas por `(hora,tipo)` e retorna `all(bool(acesa))`. | Sem hora ou faltando lâmpada: `None`. Lâmpada presente com `acesa=None`: **False**, pela conversão `bool`. |
| `app/chamados/models.py:1502,1504` | Ressuscitação não devolve pergunta de marco aceso. | Usa cópia com célula do julgamento em curso. |
| `app/chamados/services/materializacao.py:906,907,919,930` | Guarda de paridade consulta a lâmpada antes de carimbar. | `is not True` segue à recusa com motivo. Essa guarda não cobre a colisão de `:1099,1105`. |
| `app/chamados/juizes.py:1334`; `:1419` | `_resposta_concorda_com_a_luz` exige marco aceso. | C6 usa esse resultado no ramo de resposta. |
| `app/chamados/juizes.py:1435` | C6 encerra pergunta materializada com marco aceso. | Executa depois dos degraus anteriores. |
| `app/chamados/juizes.py:1452` | C6 também consulta o marco no ramo da célula. | Segundo sítio de chamada dentro do C6. |
| `app/chamados/reconciliador.py:1083,1085,1087,1097` | `veredito_orfa` consulta a lâmpada. | Exclui resposta com texto; acrescenta condições de vitalidade/teto. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:67,68,78,83` | Classifica luz True/False/None e seleciona luz False com C6 vivo. | Universo de fantasmas materializadas, não validadas. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32,40,43` | Seleciona marco aceso para recarimbar via. | Universo materializado, não validado e restrito por via. |
| `app/ponto/services/veredito_celula.py:47,56,60,66,67,68` | `_abs_marcos` encadeia entrada, pausas e saída; soma dias enquanto `dt <= prev`. | Entrada 19h e saída 7h resulta em saída **D+1**. |
| `app/ponto/services/veredito_celula.py:251,252,275,280,282,291` | Usa cadeia absoluta e preserva o instante do candidato faltante. | O candidato de saída pode ficar em D+1. |
| `app/ponto/services/cartorio.py:620,648,656` | Ata mantém lâmpadas das chaves do DNA por tipo/hora. | O juiz pode reconhecer `hf=07:00` na célula do turno D, embora o marco absoluto esteja em D+1. |
| `app/chamados/juizes.py:941,948,965,971`; `app/ponto/services/veredito_celula.py:66,68` | **Responde para a saída noturna pela lâmpada da célula do turno.** | Não recalcula nem certifica o dia civil da saída. Célula/lâmpada efetivamente gravadas de #51467/#56641: `[nao verificado]`. |

São **oito sítios de chamada, em sete funções e seis arquivos**, nas oito linhas de consumidores acima (`app/chamados/models.py:1504`; `app/chamados/services/materializacao.py:907`; `app/chamados/juizes.py:1334,1435,1452`; `app/chamados/reconciliador.py:1085`; comandos `reabrir_orfas_enterradas.py:78` e `encerrar_perguntas_com_fato.py:43`).

**6. Ramo de recusa “PENDENTE com motivo”**

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/chamados/services/materializacao.py:1099,1107,1108,1109` | **Existe:** fora do carimbo de colisão, se o motivo mudou, salva somente `recusa_motivo`. | Trunca motivo a 240 caracteres em `:1096`. Não limpa resposta, validação, materialização ou via. |
| `app/chamados/services/materializacao.py:1110,1111,1113` | Anexa ocorrência `recusa_materializacao` ao destino. | Só quando houve carimbo ou mudança de motivo e existe destino; inclui código/texto/ação traduzidos. |
| `app/chamados/models.py:3042,3044,3046,3049` | Calcula o rótulo de estado. | O ramo anterior **não garante o rótulo `PENDENTE`**: com validação pode continuar `VALIDADA`; com resposta, `RESPONDIDA`. |
| `app/chamados/services/validacao.py:188,190,192,199,202,205` | Lê motivo e devolve validação recusada. | Operação separada: para resposta de hora, sem materialização e com motivo, limpa validação, preserva resposta e registra `DEVOLUCAO`. Chamada em `:464`. |
| `app/chamados/services/materializacao.py:842,919,1219,1303`; `app/chamados/services/passivo_validado.py:545` | Chamadores de `_carimbar_recusa`. | Cobrem ausência de resposta, paridade sem luz, divergência de data, erro final da unidade e recusa de geometria do lote. |
| `app/chamados/services/materializacao.py:1095,1098`; `app/chamados/services/validacao.py:147,188,192,208,225,228,582,604`; `app/chamados/services/passivo_validado.py:66,83` | Leem o motivo para comparação, devolução, foto e trilha. | Leitores operacionais do campo persistido. |
| `app/chamados/services/explicador.py:90`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:186` | Apresentam o motivo ou o usam como explicação. | No modal, sua exibição está dentro do ramo `pendencia_admin`, em `:222`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:536,1075,1112` | Usa motivo vazio como condição dos universos 

[truncado em 20000 de 44505 caracteres]

## 12/11 12:06 · sessao c16ee12ee21d · - · - · origem=Ronald (tmux)

Censo P8 concluído em leitura; inventários completos de leitores por campo enviados nas quatro mensagens anteriores. A tabela abaixo consolida escritores e significado.

| Campo / sítio arquivo:linha | O que escreve | Observação |
|---|---|---|
| `data_evento` — `app/chamados/services/fio.py:801` | Única atribuição explícita: `linha.data_evento = dia_do_turno`. | Nascimento chama em `app/chamados/models.py:3017`, persistindo em `:3018`; costura chama em `app/chamados/services/fio.py:880`, com reversão em memória em `:898` e `:913`; rechaveamento chama em `:1038`, persistindo em `:1044`. |
| `data_evento` — `app/chamados/models.py:2790`, `:2802`; `app/chamados/services/fio.py:1631` | Porta do fato recebe dia do turno; porta da conversa fornece `localdate()`; discriminador `conversa` preserva o pedido. | **Não possui significado universal:** fato = dia do turno; conversa = dia civil de emissão, comprovado em `app/chamados/models.py:2802` e `app/chamados/services/fio.py:1632`. |
| `via_resolucao` — `app/chamados/models.py:3102` | Atribuição junto com instante e autor do carimbo. | Persistida por `carimbar` em `:3153`, por `carimbar_identidade` em `:3191` e por `descarimbar` em `:3213`. |
| `via_resolucao` — `app/chamados/models.py:3267`, `:3276`; `app/chamados/services/relavra_via.py:125` | Lote limpa `''`; encerramento por fato troca para `fato_materializado`; relavra substitui pela via derivada atual. | Últimas duas persistem em `app/chamados/models.py:3277` e `app/chamados/services/relavra_via.py:127`. Default vazio em `app/chamados/models.py:2563`. |
| `materializada_em` — `app/chamados/models.py:3100` | Instante do ato de carimbar; também `None` no descarimbo. | Mesmo conjunto de portas do carimbo acima; lote limpa em `app/chamados/models.py:3267`. |
| `materializada_em` — `app/chamados/models.py:3065`; `app/chamados/services/materializacao.py:1105` | Carimbo também nasce por decisão administrativa e por colisão recusada. | **Não comprova sozinho batida criada:** administrador chama identidade e colisão chama `fato_ocupado`. O help text “batida criada + chamado fechado” em `app/chamados/models.py:3396` é mais estreito que esses usos. |
| `respondida_em` — `app/chamados/services/disputa_emissao.py:1468`, `:1653`, `:1674` | Resposta corrente usa `now()`; resposta automática usa instante recebido; reconstrução pela trilha usa instante recebido. | Persistência em `:1509` **[nao verificado: confirmar linha exata do save normal]**, `:1655` e `:1676`. |
| `respondida_em` — `app/chamados/models.py:3133` | Carimbo pode preencher instante quando há texto e `resposta` pede responder/validar. | Não exige passagem pela atribuição normal de resposta: condição em `app/chamados/models.py:3131`. |
| `respondida_em` — `app/chamados/services/disputa_emissao.py:1593`, `:1632`; `app/chamados/models.py:1567` | Reabertura limpa; ressuscitação limpa. | Reabertura também limpa resposta; ressuscitação opera universo sem texto, filtrado em `app/chamados/models.py:1487`. |
| `validada_em` — `app/chamados/services/validacao.py:134`, `:199` | Validação usa `timezone.now()`; devolução limpa `None`. | Persistência em `:137` e `:202`; atos `VALIDACAO` e `DEVOLUCAO` em `:139` e `:205`. |
| `validada_em` — `app/chamados/models.py:3136`; `app/chamados/services/disputa_emissao.py:1480` | Carimbo pode preencher validação vazia quando `resposta='validar'`; resposta nova desfaz validação anterior. | Condições em `app/chamados/models.py:3135` e `app/chamados/services/disputa_emissao.py:1471`. |
| `validada_em` — `app/chamados/services/disputa_emissao.py:1593`, `:1632`; `app/chamados/models.py:1567` | Reabertura e ressuscitação limpam. | Não confundir com julgamento da célula: estado projetado usa validação e materialização separadamente em `app/chamados/models.py:3042`. |
| `resposta_colab` — `app/chamados/services/disputa_emissao.py:1467`, `:1652` | Resposta normal e resposta automática. | Automática não sobrescreve texto existente: guarda em `:1647`. |
| `resposta_colab` — `app/chamados/services/disputa_emissao.py:1593`, `:1632` | Reabertura limpa `''`. | Salva foto anterior em `DecisaoPergunta` em `:1615`, lendo texto e carimbos em `:1619`. |
| `resposta_colab` — `app/chamados/models.py:2448` | Default `''` no nascimento. | É texto polimórfico: hora, escape e identificadores de posto/escala; leitores diferenciados em `app/chamados/models.py:3527`, `:3535` e `app/chamados/services/materializacao.py:359`. |
| Memória, sem novo escritor persistente — `app/chamados/models.py:1508`, `:1509`, `:3164`, `:3165` | Limpa carimbos na cópia julgada; restaura instância após recusa/erro de `carimbar`. | Não contar como persistências independentes: cópia em `:1495`; função de restauração em `:3160`. |
| Fora do fluxo de produção: `bin/sombra_regras_pessoais.py:85` → `bin/sombra_mascara_pessoal.py:54`, `:57`, `:62`, `:64` | Lê e transforma `pergunta` e `resposta_colab`, persistindo por `bulk_update` na cópia sombra. | Leitor adicional SQL da máscara: `bin/sombra_selo_pessoal.py:74`, `:77`. |
| Todos os sete campos — `app/chamados/services/foto_e4b.py:251`, `:272` | Leitor genérico fotografa todos os campos concretos de `PerguntaDisputa`. | Deve entrar no censo mesmo sem referência literal ao nome de cada campo; inclusive `pergunta`. |

Complementos para consolidar a tabela P8:

- Leitores dinâmicos de instantes: `app/chamados/services/historico.py:98`, `:105`; leitor de reversão de materialização/via/resposta/validação: `app/chamados/services/passivo_validado.py:64`, `:66`, `:83`; agregador dos três instantes: `app/chamados/services/painel.py:66`.
- O instante efetivo de uma saída noturna não é a simples soma da data gravada com a hora: combinação inicial em `app/chamados/services/materializacao.py:419`, ajuste overnight em `:426`. A classificação de conflito já usa `_slot_esperado` em `app/chamados/services/validacao.py:872`.
- API expõe `data_evento` diretamente como ISO e rótulo em `app/api/views.py:1153`, `:1154`; template apresenta essa data sem recompor o dia civil do marco em `app/templates/chamados/partials/_card_atendimento.html:90`.
- Migração histórica com leitura de identidade: `app/chamados/migrations/0053_pergunta_unica_por_familia.py:32`, `:33`, `:40`; schema original de resposta/carimbos: `app/chamados/migrations/0014_perguntadisputa.py:24`, `:25`, `:26`.
- Auxiliar externo que lê respondida/validada: `bin/censo_folha.py:28`, `:29`; consumidor indireto do payload de pergunta/data: `mensageria/nucleo/ferramentas.py:119`, `:127`.
- Excluir homônimo de outro modelo: `app/chamados/services/regularizacao_ext.py:199`, `:220` refere-se a `RegularizacaoExterna`, não `PerguntaDisputa`.

## 12/11 12:03 · sessao d57e2b6a2b71 · - · - · origem=Ronald (tmux)

Contagem por expressão, sem testes/snapshots: **8 interpolações de data = 4 `.format` + 4 `.replace`; 7 locais lógicos**, porque `query.py:64,66` são os dois ramos da mesma função. Há ainda formatação administrativa sem data e cópia de catálogo para âncora (`app/chamados/services/acoes_disputa.py:265`; `app/chamados/services/disputa_emissao.py:1374`).

| Sítio arquivo:linha | O que faz | Observação / fonte da data |
|---|---|---|
| `app/chamados/catalogo/query.py:64` | `replace('{data}', ISO convertido para dd/mm/aaaa)` | Usa **contexto da disputa por motivo**, `contexto_disputa[motivo]['data']`, lido em `:58-59`; não consulta marco absoluto. Chamado no emissor em `disputa_emissao.py:345,354`. |
| `app/chamados/catalogo/query.py:66` | Ramo de exceção: `replace('{data}', data_str)` | Usa a mesma string crua do contexto; não valida turno/dia civil nesse ramo (`query.py:58-66`). |
| `app/chamados/services/disputa_emissao.py:879` | Formata texto de cada marco faltante | `dia` vem da tupla retornada por `_marcos_faltantes` (`:859-861,873`) ou `faltantes` recebido; exige `dia == data_evento` (`:874`). A tupla é ancorada na **data da célula**, inclusive a véspera, em `declaracao_texto.py:109,139`. |
| `app/chamados/services/disputa_emissao.py:1151` | Formata pergunta de intervalo a partir do chamado | `_de = date.fromisoformat(_m['data'])`, onde `_m` vem de `contexto_json.marcos_faltantes` (`:1143-1148`). Um produtor demonstrável põe ali o **dia civil do marco encadeado**: `app/ponto/management/commands/detectar_intervalo_ausente.py:77-84`. O emissor passa esse valor a `dia_do_turno_da_celula` e à criação (`disputa_emissao.py:1152-1155`); origem efetiva de cada registro de produção `[nao verificado]`. |
| `app/chamados/services/disputa_emissao.py:1208` | Formata pergunta do grupo com batida de origem | Usa `data_evento = resolver_data_evento(...batida_origem...)` (`:1205`). O resolvedor retorna **dia do turno da batida**, sem somar o dia civil da saída (`app/escala/servico_jornada.py:495-509`). |
| `app/chamados/services/disputa_emissao.py:1246` | Formata pergunta sem batida de origem | Usa `data_evento = _ddc(ch.contexto_json)` (`:1232`; import `:1042`). `_ddc` tenta `data_turno`, `data_batida`, `entrada_timestamp`, `pendentes`, `horario_previsto`: portanto aceita fontes com nomes e bases diferentes (`app/chamados/data_evento.py:16-39`). |
| `app/chamados/services/declaracao_texto.py:374` | Formata pergunta criada na declaração do colaborador | `dia` vem do marco faltante casado por `_casar_hora` (`:341-345`); `_marcos_faltantes` devolve data da célula em `:109,139`. Persiste pelo `abrir` em `:376-377`. |
| `app/chamados/services/declaracao_texto.py:664` | Formata pergunta criada ao aplicar declaração textual | Usa `p['dia']`, produzido por `propostas_do_chamado`: grade → `_marcos_faltantes` (`:615`) → `_casar_hora` (`:625-628`) → dict (`:635`); persiste em `:666-667`. |
| `app/chamados/services/acoes_disputa.py:265` | Formata confirmação de reabertura administrativa com `% label_motivo` | **Sem data no texto**; nasce como conversa (`:262-268`), cujo `data_evento` é `localdate()` (`app/chamados/models.py:2795-2803`). |
| `app/chamados/services/disputa_emissao.py:1374` | Copia pergunta de catálogo da âncora ou literal de fallback | **Sem data conhecida no texto**; cria conversa `ancora_ausente`, tipo `data_hora` (`:1369-1377`). |
| `app/chamados/services/disputa_emissao.py:440,447` | Entrega `texto` ao nascimento, por `abrir_conversa` ou `abrir` | Texto vem de `texto_pergunta_com_data` (`:345,354`); data passada ao nascimento pode ser derivada depois por outro caminho (`:371-424,451`). Logo texto e `data_evento` não são recalculados juntos nesse ponto. |
| `app/chamados/services/disputa_emissao.py:840` | Repasse de `pergunta` por `_criar_pergunta_se_couber` | Não reformata; encaminha texto e data recebidos ao `abrir` (`:823-845`). |
| `app/chamados/models.py:3015` e `:3018` | Instancia com `**campos` e salva inserção | Persistência comum dos textos recebidos; `abrir` e `abrir_conversa` chegam via `_sob_contrato/_nascer` (`:2790-2803,2820-2828`). O reuso retorna a existente sem reescrever texto (`:2998-3009`). |
| `app/chamados/catalogo/perguntas.py:49` e `app/chamados/models.py:3566` | Literais do template “Confirme o horário da saída no dia {data}” | São **fontes do template**, não formatação de `data_evento`. A referência raspada `models.py:3564-3565` não corresponde a um formatador nesta cópia (`models.py:3564-3566`). |

**Leitores diretos do campo persistido: 20 ocorrências = 14 Python + 6 templates.** Se excluir `__str__` e o comando smoke, restam 18; isso é um recorte possível, não a contagem de todos os leitores. Evidências nas vinte linhas abaixo.

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/api/views.py:1155` | API de chamados fornece `p.pergunta` | Texto cru; data é fornecida separadamente em `:1153-1154`. |
| `app/api/views.py:2551` | API das perguntas do app, `_item` | Texto cru para pendentes/histórico (`:2519-2559`). |
| `app/api/views_mensageria.py:564` | API da fila de validação | Texto cru; `data_evento` separado em `:568`. |
| `app/ponto/services/dia_do_colab.py:69` | Consulta do dia entrega pergunta | Texto cru; seleciona pelo `data_evento=data` (`:64-71`). |
| `app/chamados/views_cobrar.py:291` | Amostra da validação em lote | Texto cru no dict de saída. |
| `app/chamados/canal.py:81` | Perguntas mudas com resposta no fio | Texto cru; correlaciona fio por colaborador e `data_evento` (`:71-84`). |
| `app/chamados/services/explicador.py:71` | Monta dict explicativo | Mantém texto cru; calcula estado/ação em outros campos (`:68-79`). |
| `app/chamados/services/disputa_emissao.py:1549` | Copia pergunta para nota de resposta | Envolve o texto cru com tag e resposta; salva ocorrência no fio (`:1555-1556`). |
| `app/chamados/services/validacao.py:599` | Copia pergunta para nota de recusa | Envolve texto cru com autor, resposta anterior e motivo (`:598-600`). |
| `app/chamados/templatetags/chamados_extras.py:11` | Detecta “pergunta cega” | Lê texto cru com regex de data; não o recompõe. |
| `app/chamados/management/commands/auditar_disputas.py:74` | Auditor procura data no texto | Regex no campo cru (`:69-83`). |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:33` | Procura nota correspondente | Usa texto cru como substring `Pergunta: ...`; não recompõe. |
| `app/chamados/management/commands/smoke_chamados.py:108` | Verifica placeholder residual | Lê pergunta persistida e acusa `{data}`; o `p_cat.pergunta` de `:97` é catálogo, não campo persistido (`:94-108`). |
| `app/chamados/models.py:3547` | `__str__` da pergunta | Prefixa motivo e corta texto cru em 60 caracteres. |
| `app/templates/chamados/partials/_card_atendimento.html:93` | Cartão de resposta do colaborador | Exibe `p.pergunta` cru; sem projeção de marco. |
| `app/templates/chamados/partials/_card_atendimento.html:175` | Cartão de pergunta no histórico | Exibe `p.pergunta` cru. |
| `app/templates/chamados/partials/_pergunta_card.html:28` | Cartão de supervisão/Central | Exibe `perg.pergunta` cru. |
| `app/templates/chamados/partials/modal_fio.html:160` | Pergunta no modal do fio | Exibe texto cru; data do fato é mostrada em bloco separado (`:167`). |
| `app/templates/chamados/partials/_lote_preview.html:24` | Prévia de lote | Campo cru com `truncatechars:60`; data em coluna separada. |
| `app/templates/colaboradores/partials/painel_colaborador.html:197` | Painel do colaborador, bloco “no app” | Exibe `p.pergunta` cru. |
| `app/templates/chamados/partials/modal_fio.html:439` | **Propagação indireta:** dict do explicador | Exibe texto copiado com truncamento, além do dia separado; origem direta em `explicador.py:71`. Não entra nas 20 leituras diretas. |
| `app/chamados/views_cobrar.py:186` | **Propagação indireta:** dict do canal | Repassa texto cru já copiado por `canal.py:81`; relação demonstrada em `views_cobrar.py:180-187`. |
| `mensageria/nucleo/ferramentas.py:119` | **Propagação indireta:** item recebido da API | Copia `i.get('pergunta')` para avaliação; não recompõe. |
| `app/relatorios/pdf_chamados.py:145` e `:147` | **PDF indireto:** imprime `fup.conteudo` | Pode carregar as cópias da pergunta feitas em `disputa_emissao.py:1549-1556` e `validacao.py:598-600`; corta conteúdo em 1.000 caracteres. Não lê `PerguntaDisputa.pergunta` diretamente. |
| `app/chamados/catalogo/query.py:45` | Interpolador existente de catálogo | Não é projeção universal dos leitores: recebe motivo/contexto e lê **catálogo** (`:52-66`); chamados encontrados no emissor `disputa_emissao.py:345,354` e auditor `auditar_disputas.py:191`. Os leitores diretos acima acessam o campo. |

Não apareceu escritor de `p.pergunta = ...` nem `update(pergunta=...)` fora dos testes na varredura da cópia; a prova positiva disponível é a cadeia de inserção por `**campos` em `app/chamados/models.py:3015-3018`; ausência absoluta por mecanismos dinâmicos `[nao verificado]`. PWA com renderizador próprio de `PerguntaDisputa.pergunta` não foi demonstrado; os pontos de app comprovados são APIs e templates citados, e a execução efetiva do cliente em produção é `[nao verificado]`.

## 12/11 12:03 · sessao 84c8983eaadc · - · - · origem=Ronald (tmux)

Censo somente leitura. Não encontrei `AGENTS.md` na árvore nem nos ancestrais pesquisados; regra adicional de arquivo: `[nao verificado]`.

**3. VIA_FATO_OCUPADO**

| sítio arquivo:linha | o que faz | observação |
|---|---|---|
| `app/chamados/models.py:2488` | Declara `VIA_FATO_OCUPADO='fato_ocupado'`. | Definição, não escrita de registro. |
| `app/chamados/services/materializacao.py:1095`, `:1097`, `:1099`, `:1105` | **Único chamador ativo encontrado que grava essa via**: relê carimbo/via/recusa, traduz texto, decide `_carimba = materializada_em is None and codigo == 'COLISAO'`, chama `carimbar_identidade`. | Não consulta motivo/marco/lâmpada da pergunta. |
| `app/chamados/catalogo/recusa.py:66`, `:77` | Tradutor classifica `COLISAO` quando o texto contém `colisao tipo-oposto`, sem distinguir maiúsculas. | `any(m in baixo...)` em `:81-83`; classificação textual. |
| `app/chamados/models.py:3179`, `:3186`, `:3187`, `:3189`, `:3191`; `:3098` | Porta valida a família, grava `materializada_em`, `materializada_por`, `via_resolucao`, e `recusa_motivo` quando fornecido. | Não pergunta ao C6 (`:3170-3177`). Não limpa resposta nem validação. |
| `app/ponto/registro_batida.py:138`, `:139`, `:145`, `:150` | Origem da mensagem: existe batida oposta não retratada na janela inclusiva `timestamp±g_op`, e a candidata **não** casa com marco previsto do próprio tipo; sem autoridade humana retorna recusa. | **Não é somente vizinhança**: há exceção por geometria prevista. Entretanto não consulta **o marco cobrado pela PerguntaDisputa**, nem a sua lâmpada. A assinatura recebe colaborador, timestamp, tipo, origem (`:94`). |
| `app/ponto/registro_batida.py:21` | `g_op=300s` quando há escala e marcos no dia civil local da timestamp; senão `60s`. | Código `:28-32`; o texto do catálogo fala genericamente “menos de 5 minutos” (`recusa.py:67`). |
| `app/ponto/registro_batida.py:35` | `_casa_marco_proprio`: verifica dia local e véspera; considera algum marco previsto do mesmo tipo a distância `<=5min`. | `:42-49`; usa `escala_vigente` e `marcos_turno`, sem PerguntaDisputa. |
| `app/chamados/models.py:2511` | Inclui a via em `VIAS_DE_IDENTIDADE`. | Leitores derivados na tabela 4. |
| `app/chamados/models.py:2535` | Choice/descrição da via. | Disponível a `get_via_resolucao_display`. |
| `app/chamados/models.py:3738` | Inclui em `VIAS_SEM_TESTEMUNHA`. | Família separada da identidade; retirar de uma não retira da outra. |
| `app/chamados/juizes.py:679`, `:689` | Juiz do lastro reconhece a família sem testemunha e absolve sempre as identidades carimbadas. | Se sair só da identidade, continua no ramo sem testemunha, mas passa por janela/classificação de falta (`:690-705`). |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:47`, `:48`, `:460`, `:746` | Família sem testemunha alimenta piso I6 e sua apresentação. | Fato ocupado permanece nesse piso se apenas a identidade mudar. |
| `app/templates/chamados/partials/_card_atendimento.html:186` | Caso literal `fato_ocupado` ou `paridade_recusada`: “ja havia batida registrada.” | Leitor de apresentação explícito. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `:42` | Só sobrescreve vias `('', 'legado_sem_lastro', None)`. | Fato ocupado é excluído pelo filtro genérico, embora citado apenas no comentário `:36`. |

Migrações `app/chamados/migrations/0040_a27_recusa_motivo.py:21`, `0041_b2_via_legado_sem_lastro.py:16`, `0042_hx_via_ausencia_dia.py:16`, `0046_via_celula_absolveu.py:16`, `0048_via_competencia_trancada.py:15`, `0051_decisao_pergunta.py:111` preservam o valor em **choices**, não constituem novos escritores desse valor em registros.

**4. VIAS_DE_IDENTIDADE — todos os consumidores ativos diretos e pelas duas funções da fonte**

| sítio arquivo:linha | o que faz | observação / efeito se fato ocupado sair apenas da tupla |
|---|---|---|
| `app/chamados/models.py:2511` | Fonte: admin, duplicata, fato ocupado, paridade recusada, motivo retratado. | Passa de cinco para quatro valores. |
| `app/chamados/models.py:3090` | `vias_de_identidade()` devolve a própria tupla. | Deixa de devolver fato ocupado. |
| `app/chamados/models.py:3096` | `vias_que_encerram_por_identidade()` acrescenta competência trancada. | Deixa de devolver fato ocupado; conserva tranca. |
| `app/chamados/models.py:3119` | `carimbar` recusa famílias que pertencem à outra porta. | Fato ocupado passa a poder entrar nesta porta e será submetido ao C6 (`:3140`). |
| `app/chamados/models.py:3179` | `carimbar_identidade` exige pertencimento. | **Chamador atual de `materializacao.py:1105` passa a levantar `ValueError`** (`:3180`) antes de gravar. |
| `app/chamados/models.py:3348`, `:3351`, `:3362` | `q_vivas` faz pré-filtro SQL: identidade materializada exclui perguntas sem resposta viva. | Fato ocupado deixa de excluir por identidade; validação, resposta e veredito da célula ainda condicionam o resultado (`:3349-3363`). |
| `app/chamados/juizes.py:689`, `:691` | `_pergunta_materializada_de_fato`: identidades carimbadas absolvem sem reclassificar o dia. | Fato ocupado deixa de absolver sempre. Continua em `VIAS_SEM_TESTEMUNHA` (`models.py:3738`): pode absolver por ausência de dia, fora da janela, falta não acusada ou exceção (`juizes.py:690-705`); não passa automaticamente a exigir a lâmpada específica. |
| `app/chamados/juizes.py:1427`, `:1428` | C6 mata identidade materializada no degrau posterior à resposta. | Fato ocupado segue para lâmpada (`:1434-1436`), veredito da célula (`:1438-1453`), lei do dia (`:1454-1462`) e lastro (`:1472-1479`). **Validada ainda morre antes de tudo** (`:1391-1392`); resposta não vazia também pode retornar antes (`:1407-1419`). |
| `app/chamados/services/relavra_via.py:181` | `q_carimbo_desfazivel` exclui identidades materializadas. | Fato ocupado torna-se elegível; só cai se a linha relida passar no filtro e o C6 a considerar viva (`:236-246`). Validada permanece morta no C6. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:195`, `:206` | `_contraprova_f1t` rotula o degrau pelo qual morreu. | Não classifica mais como “via de identidade”; se ainda morta e não validada, rotula “carimbo/celula”. Se viva, vira alarme (`:202-210`). |

São **quatro acessos diretos à tupla** (`models.py:3090`, `:3096`; `relavra_via.py:181`; auditor `:195`) e **cinco consumidores operacionais das funções de acesso** (`models.py:3119`, `:3179`, `:3348`; `juizes.py:689`, `:1427`). A própria definição está em `models.py:2511`.

**5. fato_do_marco_no_espelho**

| sítio arquivo:linha | o que faz | observação |
|---|---|---|
| `app/chamados/juizes.py:922` | Universo de motivos: `saida_sem_entrada→(E,hi)`, `orfao_14h→(S,hf)`, `intervalo_saida→(S,hii)`, `intervalo_volta→(E,hfi)`. | Outro motivo resulta `None` (`:941-943`). |
| `app/chamados/juizes.py:944`, `:948` | Usa exclusivamente `p.celula`, `cel.dna.marcos` e `cel.ata.lampadas`. | Não consulta banco diretamente, resposta, `data_evento`, timestamp, `veredito`, `_abs_marcos` ou batidas. FK pode ser carregada pelo ORM ao acessar `p.celula`. |
| `app/chamados/juizes.py:956`, `:960` | Para intervalo, seleciona todas as chaves existentes da mesma posição dos pares de pausa. | Pergunta de intervalo cobra AND das pausas, sem identificar uma única instância de pausa. |
| `app/chamados/juizes.py:962`, `:965`, `:968`, `:969`, `:971` | Faz lookup das lâmpadas por `(hora,tipo)`; `None` sem horas ou se faltar alguma lâmpada; retorna `all(bool(acesa))`. | `acesa=None` numa lâmpada presente vira **False**, porque usa `bool`, não `None`. Não valida identidade/data da célula contra a pergunta. |
| `app/chamados/models.py:1504` | `_universo_da_ressuscitacao` não ressuscita marco já aceso. | Usa cópia com a célula do julgamento em curso (`:1502`). |
| `app/chamados/services/materializacao.py:906`, `:907` | Guarda de **paridade** só carimba a recusa quando o marco está aceso. | `is not True` vai à recusa pendente; essa guarda não cobre COLISAO (`:919-930`, `:1099-1106`). |
| `app/chamados/juizes.py:1334` | `_resposta_concorda_com_a_luz` exige marco aceso antes de comparar resposta/luz e coerência do dia. | C6 o chama em `:1419`. |
| `app/chamados/juizes.py:1435` | C6: com materialização e marco aceso, mata antes do veredito do dia. | Chamado só depois dos degraus anteriores. |
| `app/chamados/juizes.py:1452` | C6: célula que não acusa com marcos pendentes ainda pode absolver se o marco da pergunta acendeu. | Segundo sítio de chamada dentro de `_pergunta_viva_calc`. |
| `app/chamados/reconciliador.py:1085` | `veredito_orfa` exige marco aceso; não trata resposta com texto (`:1083`) e depois exige C6 vivo e teto encerrado (`:1087-1097`). | Consumidor de órfãs. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:78` | Mede True/False/None das fantasmas e só considera luz False com C6 vivo (`:83`). | Recorte específico `VIA_FANTASMA`, materializada, não validada (`:67-68`). |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:43` | Seleciona perguntas materializadas não validadas e sem história para recarimbar marco aceso. | Via permitida em `:40-42`. |
| `app/ponto/services/veredito_celula.py:47`, `:56`, `:66`, `:67` | `_abs_marcos` encadeia entrada, pausas e saída, acrescentando dias enquanto o instante não for posterior ao anterior. | Entrada 19h/saída 7h produz S em D+1 pela regra `:67-68`. |
| `app/ponto/services/veredito_celula.py:251`, `:252`, `:275`, `:280` | Veredito usa cadeia absoluta, indexa por `(tipo,HH:MM)` e cria candidatos no instante absoluto faltante. | Instante D+1 preservado no candidato (`:282`, `:291`). |
| `app/ponto/services/cartorio.py:620`, `:648`, `:656` | Ata representa lâmpadas com chaves do DNA e casamento por tipo/hora. | Isso permite ao juiz acima ler `hf='07:00'` na célula do turno D, mesmo sendo saída D+1. |

Contagem das chamadas ativas do juiz: **8 sítios em 7 funções, 6 arquivos**: `models.py:1504`; `materializacao.py:907`; `juizes.py:1334`, `:1435`, `:1452`; `reconciliador.py:1085`; comandos `reabrir_orfas_enterradas.py:78`, `encerrar_perguntas_com_fato.py:43`.

Conclusão demonstrável para noturna: o juiz consegue responder pela **lâmpada de saída `hf` da célula do turno**, inclusive quando a cadeia absoluta a coloca em D+1; ele não recalcula nem confere o dia civil do marco (`juizes.py:941-971`; `veredito_celula.py:66-71`). A lâmpada e a célula corretas nos casos de produção 51467/56641: `[nao verificado]`.

## 12/11 12:01 · sessao 9a4a3a589e82 · MAPA-DO-FIO-MORTO · censo · origem=Code

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **d — SEM_ACAO** | 241 (`recorte.out:50`) | **Juiz:** `censo_alcance`: sem vivas ligadas à célula, colaborador na CxR e dia fora dela; rota `sem_porta`, papel `nenhuma porta` (`app/chamados/services/vigia_fio.py:610`, `app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:699`). **Escritor:** `d` não é campo persistido; a retenção lê veredito/protestos, escritos por `lavrar_veredito` (`app/folha/export.py:1241`, `app/ponto/portas/celula.py:974`). Ausência de viva não identifica nascimento, morte ou escritor histórico [nao verificado]. | **E3:** “vigia_fio leem SO tarefa_do_dia”; **E4B:** “backfill do fio morto (matriz_e4 C3, classes a-e) pela porta canonica” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1894`). Cobrem leitura/backfill; `tarefa_nada` não produz ato reparador no comando (`app/chamados/management/commands/backfill_fio_morto_e4b.py:117`). | **Decisão específica para retenção com tarefa vazia: não consta dos dois anexos da pergunta 2** (`avais_e3_leitores.txt:5`, `avais_e3_leitores.txt:18`; `censo.r`, seção 3 [nao verificado quanto à numeração física]). Que os **241** tenham tarefa vazia também não foi medido: `SEM_ACAO` é classificação da sonda, sem consulta a C19 (`sonda_mapa_fio.py:39`). |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **porta_fila — decidir — SEM_ACAO** | 15 (`recorte.out:36`) | **Rota:** C17 aceita chamado da célula na fila antes de exigir pergunta; censo copia rota e verbos (`app/chamados/juizes.py:1647`, `app/chamados/juizes.py:1657`, `app/chamados/services/vigia_fio.py:642`). **Papel:** `particionar_verbos → creditos_perguntas → verbo_do_chamado`; ausência de crédito pode resultar em `decidir` (`app/chamados/services/verbos.py:34`, `app/chamados/services/raiox.py:135`, `app/chamados/catalogo/natureza.py:61`). **Escritores:** status por `_gravar_estado`; mortes/carimbos pelas portas discriminadas na resposta 4 (`app/chamados/models.py:201`, `app/chamados/models.py:3153`). Autoria histórica individual [nao verificado]. | **E3:** “5 respondedores morrem com certidao” e leitores passam a C19 (`app/docs/BACKLOG.md:1873`). C19 já publica somente portas de ações exigíveis; fila residual não cria tarefa (`app/chamados/services/tarefa_do_dia.py:538`). **E4C/E4D:** “LEITORES DIVERGE + selo 2” / “parar projecao” (`app/docs/BACKLOG.md:1885`). | Os **15 ficam fora do comando E4B**, cujo recorte exige `sem_porta` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:79`). A migração E3 não demonstra a ação C19 nem a cura histórica de cada dia; isso não aparece no recorte [nao verificado]. Cobertura adicional específica por outro PRONTO [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **porta_fila — cobrar — SEM_ACAO** | 6 (`recorte.out:30`) | **Rota:** chamado da célula em `qs_na_fila`; **papel:** crédito de pergunta sem resposta resulta em `cobrar` (`app/chamados/juizes.py:1647`, `app/chamados/services/raiox.py:140`, `app/chamados/catalogo/natureza.py:50`, `app/chamados/catalogo/natureza.py:60`). Crédito usa `q_vivas`/lavra; app acrescenta C6 e disputa aberta: são predicados diferentes (`app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135`, `app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`). **Escritores:** nascimento `_nascer`, resposta/reabertura e estado do chamado (`app/chamados/models.py:3015`, `app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:228`). | **E3:** “fio, tela admin, API/app, badge/PWA […] leem SO tarefa_do_dia” (`app/docs/BACKLOG.md:1873`). **RAIZ:** “elo raiz<-filhos […] DisputaSupervisao nao reage” cobre a relação estrutural, sem identificar estes seis (`app/docs/BACKLOG.md:1904`). | **E4B exclui estes seis `porta_fila`** (`app/chamados/management/commands/backfill_fio_morto_e4b.py:79`). `app=0` não revela qual guarda excluiu cada pergunta; atribuir os seis a disputa fechada, identidade ou outro motivo é [nao verificado]. PRONTO específico que decida cada um desses estados [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **a — registrado — SEM_ACAO** | 2 (`recorte.out:47`) | **Juiz:** `censo_alcance` encontra chamado vivo fora da fila e publica o status como detalhe (`app/chamados/services/vigia_fio.py:664`, `app/chamados/services/vigia_fio.py:669`). `registrado` pertence a VIVOS, mas não a COBRANCA (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`). **Escritores:** nascimento `abrir/criar`, decidido pelo motor; mudanças por `_gravar_estado` (`app/chamados/models.py:934`, `app/chamados/models.py:972`, `app/chamados/models.py:1061`, `app/chamados/models.py:1096`, `app/chamados/models.py:228`). | **E4B:** “classes a-e”; **E3:** “vigia_fio leem SO tarefa_do_dia” (`app/docs/BACKLOG.md:1894`, `app/docs/BACKLOG.md:1873`). **E1:** “censo de toda porta do fato” cobre a estrutura das entradas, sem provar defeito nestes registros (`app/docs/BACKLOG.md:1859`). | O PRONTO não transforma `registrado` em ação exigível; o catálogo declara registro sem cobrança (`app/chamados/catalogo/motor.py:25`). A ação C19 e o ato E4B aplicável aos dois não foram fornecidos [nao verificado]. Eventual lacuna além desse escopo [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **e — pergunta_viva_sob_chamado_terminal — SEM_ACAO** | 2 (`recorte.out:52`) | **Juiz:** após as causas anteriores, viva com âncora terminal recebe esse detalhe (`app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:687`). Âncora é destino, senão chamado da disputa (`app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:117`). **Escritores:** estado terminal por `_gravar_estado`; vínculo por `_nascer/reapontar_resolvedora` (`app/chamados/models.py:228`, `app/chamados/models.py:3015`, `app/chamados/services/fio.py:880`). Sequência histórica concreta [nao verificado]. | **E4B:** “classes a-e”; cura de credor sem resposta existe em `reabrir_credor_de_pergunta_viva` (`app/docs/BACKLOG.md:1894`, `app/chamados/services/disputa_emissao.py:1805`). **E3:** leitura única; **RAIZ:** “elo raiz<-filhos” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1904`). | A cura do credor possui vetos de cascata, exportação, limbo e reabertura; não promete tornar a pergunta pendente no app (`app/chamados/services/disputa_emissao.py:1835`, `app/chamados/services/disputa_emissao.py:1839`, `app/chamados/services/disputa_emissao.py:1848`). Motivo de `app=0` e cobertura individual desses vetos [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **e — sem_pergunta_viva — SEM_ACAO** | 1 (`recorte.out:53`) | **Juiz:** sem vivas e sem enquadramento nas causas anteriores recebe esse detalhe (`app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:685`). **Escritor compatível com a via raspada `fato_ocupado`:** recusa COLISAO do materializador → `carimbar_identidade` (`app/chamados/services/materializacao.py:1097`, `app/chamados/services/materializacao.py:1105`, `app/chamados/models.py:3191`). A via não prova autoria histórica [nao verificado]. | **E4B:** “classes a-e”; **E3:** leitura única; **E4C/E4D:** “LEITORES DIVERGE + selo 2” / “parar projecao” (`app/docs/BACKLOG.md:1894`, `app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1885`). | A via de identidade pode encerrar a pergunta sem resolver a exigibilidade do dia; C6 e C19 fazem leituras distintas (`app/chamados/juizes.py:1428`, `app/chamados/services/tarefa_do_dia.py:230`). Ação necessária, C19 concreto e cura desse dia não constam da medida [nao verificado]. Cobertura contratual adicional específica [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **e — COLAB_RESPONDE** | 48 (`recorte.out:51`) | **Juiz da causa:** viva sob âncora terminal (`app/chamados/services/vigia_fio.py:687`). **Juiz da ação da sonda:** existe pendência do app no mesmo `data_evento`; não significa reparação completa do dia (`sonda_mapa_fio.py:27`, `sonda_mapa_fio.py:41`). **Escritores:** estado terminal `_gravar_estado`; nascimento/costura da pergunta `_nascer/reapontar_resolvedora` (`app/chamados/models.py:228`, `app/chamados/models.py:3015`, `app/chamados/services/fio.py:880`). | **E4B:** “backfill do fio morto […] classes a-e”; reabre credor de pergunta viva sem resposta sob âncora fora de VIVOS (`app/docs/BACKLOG.md:1894`, `app/chamados/services/disputa_emissao.py:1821`, `app/chamados/services/disputa_emissao.py:1827`). **E3:** leitores únicos; **RAIZ:** “elo raiz<-filhos” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1904`). | **Cobertura parcial:** a cascata pode impedir a reabertura; o dado não contém os insumos dos vetos (`app/chamados/services/disputa_emissao.py:1835`, `app/chamados/services/disputa_emissao.py:1839`, `app/chamados/services/disputa_emissao.py:1848`). Não há prova de 48 curas nem de que a pergunta contada em `app` seja a mesma que determinou a causa [nao verificado]. Lacuna contratual adicional [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **c — ADMIN_VALIDA** | 15 (`recorte.out:48`) | **Juiz:** viva respondida sob âncora terminal; validação ausente decorre de C6 (`app/chamados/services/vigia_fio.py:671`, `app/chamados/services/vigia_fio.py:675`, `app/chamados/juizes.py:1391`). **Escritores:** resposta por `responder_pergunta_disputa_core`, automática, trilha ou complemento de `carimbar`; status por `_gravar_estado` (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3133`, `app/chamados/models.py:228`). | **E4B:** “classes a-e”; **E3:** “tela admin […] leem SO tarefa_do_dia”; **RAIZ:** reação raiz/filhos (`app/docs/BACKLOG.md:1894`, `app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1904`). **E4C/E4D:** leitores/escritores da projeção (`app/docs/BACKLOG.md:1885`). | **A cura de reabrir credor exclui respondidas**, portanto não alcança diretamente a pergunta que caracteriza `c` (`app/chamados/services/disputa_emissao.py:1821`). Outros atos E4B dependem da resposta C19, ausente na medida (`app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`). PRONTO específico garantindo a passagem desses 15 a uma lista de validação [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **d — COLAB_RESPONDE** | 2 (`recorte.out:49`) | **Causa:** ausência de vivas por **FK célula** e dia fora da CxR (`app/chamados/services/vigia_fio.py:610`, `app/chamados/services/vigia_fio.py:677`). **Ação:** pendência por **data_evento**, em outro universo; app não exclui conversas (`sonda_mapa_fio.py:27`, `sonda_mapa_fio.py:30`, `app/chamados/services/pergunta_no_app.py:49`). **Escritores dos vínculos:** `_nascer` associa célula do fato; `rechavear_data_evento` muda dia/célula (`app/chamados/models.py:2897`, `app/chamados/models.py:3015`, `app/chamados/services/fio.py:1038`). | **E3:** API/app e vigia leem C19; **E1:** “censo de toda porta do fato”; **E4B:** “classes a-e” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1859`, `app/docs/BACKLOG.md:1894`). | A diferença de universos não prova erro de identidade, célula ou C6. Qual pergunta gera `app=1`, sua natureza e seu vínculo com a célula são [nao verificado]. Cobertura individual ou lacuna adicional não podem ser atribuídas pelos agregados [nao verificado]. |

**2. NAO.** O brief E3 fornece o procedimento V/T/X e exige decisões no censo; não escreve o que C19 deve mandar — nem que deva mandar nada — para o estado específico “folha retém e tarefa não manda nada”. O `censo.r` fornecido distingue nada/desconhecido/sem porta e conserva o universo, sem decidir esse estado (`avais_e3_leitores.txt:5`, `avais_e3_leitores.txt:18`; `censo.r`, seção 3, itens 4–5 [nao verificado quanto à numeração física]).

Os **241 são `SEM_ACAO` da sonda**, não uma medição de `tarefa_do_dia == nada`; essa classificação consulta validação, app, Resolver dia e rota (`recorte.out:50`, `sonda_mapa_fio.py:39`).

| 3. Tela candidata | View/template e função REAL que monta a lista | c — 15 | e — 48 |
|---|---|---|---|
| **Drawer administrativo — Mensagens** | `painel_colaborador` chama `projecao_do_app`; lista real: `perguntas_do_app → consulta_do_app → projecao_do_app`; template percorre pendências (`app/colaboradores/views.py:940`, `app/chamados/services/pergunta_no_app.py:45`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/services/pergunta_no_app.py:78`, `app/templates/colaboradores/partials/painel_colaborador.html:193`). | **Exclui a pergunta respondida** de `pendentes`; ela vai ao histórico. Outra pendência do mesmo dia pode aparecer (`app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/pergunta_no_app.py:74`; ocorrência individual [nao verificado]). | **Inclui as pendências que produziram `app>0`**, sem filtro de status da âncora/silêncio (`sonda_mapa_fio.py:27`, `app/chamados/services/pergunta_no_app.py:71`). Mostra dia do turno para fatos; conversa recebe dia `None`. Identidade entre pendência do app e pergunta sob terminal [nao verificado] (`app/chamados/services/pergunta_no_app.py:106`). |
| **Modal administrativo do fio** | `modal_fio_colaborador → montar_chamados_fio(so_janela=False)`; template percorre cartões e perguntas (`app/chamados/views.py:889`, `app/chamados/services/fio.py:163`, `app/templates/chamados/partials/modal_fio.html:68`, `app/templates/chamados/partials/modal_fio.html:157`). | **O predicado admite:** todos os chamados do colaborador; perguntas de disputa aberta e vivas de disputa fechada. Terminal/silêncio não excluem (`app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:235`). Porém a enumeração exige disputa com chamado; esse FK não consta do recorte: inclusão individual [nao verificado] (`app/chamados/services/fio.py:184`). | Mesmo predicado: **admite pergunta sob terminal**, com a mesma ressalva do FK da disputa (`app/chamados/services/fio.py:184`, `app/chamados/services/fio.py:240`). Inclusão das 48 perguntas concretas [nao verificado]. |
| **Página administrativa Fio** | `fio_colaborador → montar_chamados_fio`; template renderiza cartões/followups (`app/chamados/views.py:677`, `app/chamados/services/fio.py:163`, `app/templates/chamados/fio_colaborador.html:153`, `app/templates/chamados/fio_colaborador.html:237`). | Inclui o **chamado terminal**, pois o carregamento filtra somente colaborador; esse template não possui a lista de perguntas do modal. Ver chamado não comprova ver a pergunta (`app/chamados/services/fio.py:177`, `app/templates/chamados/fio_colaborador.html:161`). | Mesmo comportamento (`app/chamados/services/fio.py:177`, `app/templates/chamados/fio_colaborador.html:161`). |
| **Central/painel de gestão** | View `painel_gestao → montar_contexto_painel`; templates `painel_gestao.html` e `_lista_chamados.html` (`app/chamados/views.py:727`, `app/chamados/views.py:740`, `app/chamados/views.py:767`, `app/chamados/services/painel.py:198`). | **Exclui a âncora terminal do padrão:** exige COBRANCA. Filtro explícito pelo status admite o chamado; “histórico” cobre resolvido/fechado, sem provar qual status ocorreu aqui (`app/chamados/services/painel.py:302`, `app/chamados/services/painel.py:306`, `app/chamados/services/painel.py:316`; status individual [nao verificado]). | Mesmo predicado: **fora da lista padrão**, acessível pelo filtro explícito correspondente (`app/chamados/services/painel.py:306`, `app/chamados/services/painel.py:316`). |
| **Pendências de disputa** | `pendencias_disputa → relatorios_disputa.coletar`; template de pendências (`app/colaboradores/views.py:2998`, `app/colaboradores/views.py:3001`, `app/chamados/relatorios_disputa.py:26`, `app/templates/colaboradores/partials/pendencias_disputa.html:6`). | **Condicional:** `coletar` exige disputa aberta; o recorte não fornece seu fechamento. Categoria depende de todas as vivas da disputa (`app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:40`). Inclusão individual [nao verificado]. | A disputa da **pendência do app** entra, pois o app exige disputa aberta. A tela mostra pessoa/disputa, não cada dia/pergunta (`app/chamados/services/pergunta_no_app.py:71`, `app/chamados/relatorios_disputa.py:40`, `app/templates/colaboradores/partials/pendencias_disputa.html:6`). Correspondência com a pergunta terminal [nao verificado]. |
| **Calendário do perfil** | `calendario_ponto → contexto_calendario`; grade `_calendario_grade.html` (`app/colaboradores/views.py:1949`, `app/colaboradores/services/calendario.py:159`, `app/templates/colaboradores/partials/_calendario_grade.html:67`). | **Inclui o dia na janela selecionada**, enumerada inclusive; não lista a pergunta. Ticket da âncora terminal fica fora da carga VIVOS; eventual pai vivo é outro vínculo (`app/colaboradores/services/calendario.py:437`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:603`, `app/colaboradores/services/calendario.py:721`). | Mesmo predicado: **dia visível na janela**, sem prova de exposição da pergunta terminal (`app/colaboradores/services/calendario.py:603`, `app/colaboradores/services/calendario.py:721`). |
| **Cadastro × Realidade** | View `cadastro_realidade → cadastro_realidade.lista`; ramo candidato C11 é `beco_em_lote` (`app/escala/views.py:476`, `app/escala/views.py:487`, `app/escala/services/cadastro_realidade.py:787`, `app/escala/services/cadastro_realidade.py:442`). | **Inclusão [nao verificado]:** C11 exige ativo, veredito `indefinida`, protesto específico, sem porta, viva sob terminal e folga não definida; o recorte não contém todos esses insumos (`app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:515`, `app/escala/services/cadastro_realidade.py:518`). | Mesma insuficiência: `sem_porta` com terminal **não basta** para inclusão (`app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:518`). |

| 4. Campo/estado | Quem ESCREVE | Quem LÊ | Mesmo significado e bordas |
|---|---|---|---|
| **`rota`** | Saída transitória de C17; censo copia ou atribui balde/`sem_porta`. C19 também produz sua própria rota, a partir das ações (`app/chamados/juizes.py:1657`, `app/chamados/services/vigia_fio.py:646`, `app/chamados/services/vigia_fio.py:699`, `app/chamados/services/tarefa_do_dia.py:553`). | Sonda; backfill seleciona `sem_porta`; folha lê C17 (`sonda_mapa_fio.py:32`, `sonda_mapa_fio.py:47`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:79`, `app/folha/fora_da_folha.py:67`). | **Não equivalentes:** C17 admite presença na fila; C19 exige porta de ação julgada. Não são campos persistidos da pergunta (`app/chamados/juizes.py:1647`, `app/chamados/services/tarefa_do_dia.py:541`, `app/chamados/services/tarefa_do_dia.py:546`). |
| **`causa`/`detalhe`** | `censo_alcance`, por precedência a→b→c→d→e; grava apenas a estrutura de retorno (`app/chamados/services/vigia_fio.py:667`, `app/chamados/services/vigia_fio.py:699`). | Sonda agrega somente `sem

[truncado em 20000 de 31536 caracteres]

## 12/11 12:00 · sessao 25cc736dbb89 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**Veredito geral: bloqueia declarar a c16 integralmente fechada.** A guarda do resumo foi corrigida e existe agora teste integrado com o recalculador real; porém a garantia literal “nunca turno aberto” sem célula continua sem preservação, e outro leitor da marca ainda elimina a data ao filtrar anomalias. (`app/relatorios/pdf_espelho.py:477`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`; `app/ponto/tests/test_lastro_c11_sem_banco.py:418`; `app/relatorios/pdf_espelho.py:675`)

**1. Bloqueios da preverif c14**

| Bloqueio | Veredito | O contraexemplo ainda reproduz? |
|---|---|---|
| Órfã 10/11 07:02 suprime entrada legítima 11/11 07:02 | **Fecha nesse ponto.** | **Não** no resumo nem na marca. A seleção restringe o HH:MM à linha recebida e guarda o timestamp completo; guarda e marca consultam esse mesmo conjunto. O teste exige uma abertura em 11/11, preservando a órfã de 10/11. (`app/relatorios/pdf_espelho.py:475`; `:477`; `:178`; `:516`; `:568`; `app/ponto/tests/test_lastro_c14_sem_banco.py:118`) |
| Prova integrada de (d), com recalculador substituído | **Fecha por inspeção; execução ORM [nao verificado].** | O mutante `return {}` já não sobreviveria: `_lido()` chama o recalculador real e exige o colaborador no retorno. O simulador também é chamado realmente. A prova antiga continua parcial, agora declaradamente. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`; `:85`; `:89`; `app/ponto/tests/test_lastro_c14_sem_banco.py:393`) |
| Selo sem célula enfraquece “nunca turno aberto” | **Fecha a comparação com a tela; bloqueia afirmar preservação da garantia literal.** | O comportamento sem ata permanece: marca ausente e um turno aberto no harness. O novo selo compara contadores, mas deixou de exigir zero; a documentação restringiu “nunca” ao caso **com ata**. O resultado da fixture ORM de 16:19 permanece **[nao verificado]**. (`app/ponto/tests/test_lastro_c11_sem_banco.py:416`; `:418`; `app/relatorios/tests/test_cartao_marca_espuria.py:8`; `:135`; `:155`) |

**Há uma sobrevivência da classe “perder a data”, fora da guarda corrigida:** o cabeçalho reúne HH:MM de todas as órfãs e remove qualquer anomalia contendo esses horários. Com órfã em **10/11 07:02**, também elimina **“Entrada sem saída em 11/11 07:02.”** O formato dessa mensagem vem do adaptador produtivo. Isso bloqueia considerar a classe completamente fechada no escopo solicitado; ocorrência efetiva em produção: **[nao verificado]**. (`app/relatorios/pdf_espelho.py:675`; `:678`; `app/ponto/adaptador_motor.py:40`)

**2. Teste c16 — fecha a costura; tem limites explícitos** [nao verificado]

As três funções são importadas diretamente e chamadas sem substituição por `Mock`: `julgar_celula`, `recalcular_fechamento_mes` e `instantaneo`. O congelamento substitui o relógio, não essas funções. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:31`; `:34`; `:35`; `:46`; `:79`; `:83`; `:89`)

O oráculo é **misto, com âncora independente suficiente para esse caso**:

- **Pela regra:** cadastro 08–17 com intervalo 12–13, quatro batidas na segunda e ausência aprovada na terça; esperados literais de **960 previstos, dois dias, 480 abonados e 480 realizados**. Não são capturados da saída para formar o esperado. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:56`; `:66`; `:73`; `:107`)
- **Por invariância:** depois da relavra, compara seis campos e o retrato do retido com o estado anterior. A contraprova retira os metadados e exige perda de 480 no previsto e abono zero. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:116`; `:123`; `:130`; `:139`)
- **Limite:** a ata antiga continua sintetizada da nova; isso não executa o cartório histórico. `ausencia_label` é cobrado por presença, sem assert de conteúdo. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:95`; `:101`)

**Mutantes produtivos que fariam falhar, por inspeção:**

| Mutante | Mordida |
|---|---|
| Recalculador retorna `{}` | Falha no `assertIn`. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:85`) |
| Cartório perde os metadados da ausência | Falha na presença/tipo/id da ata nova. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:95`; `:97`; `:98`) |
| Cálculo ignora `ausencia_tipo` | Falha na contraprova que exige diferença de 480. A regra produtiva consulta esse campo. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:139`; `app/ponto/services/dia_pago.py:392`) |
| Simulador consulta fechamento gravado para os retidos | Nesta fixture, sem fechamento gravado, desaparece a prontidão exigida. (`app/folha/management/commands/simular_folha.py:76`; `:85`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:125`; `:128`) |

O comentário “MESMO objeto” excede o demonstrado pelo teste: `_lido()` e `_foto()` fazem chamadas independentes. A implementação do simulador transporta os objetos da sua própria chamada ao recalculador. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`; `:89`; `:121`; `app/folha/management/commands/simular_folha.py:130`; `:152`)

**3. Selo cartão — fecha a comparação sem célula**

Compara **a mesma autoridade funcional**, consultada duas vezes: chama `espelho_do_colab` diretamente e chama o coletor PDF, que também consulta esse serviço, com o mesmo período e instante nesse cenário. Não compara a mesma instância de retorno. (`app/relatorios/tests/test_cartao_marca_espuria.py:75`; `:129`; `:132`; `app/relatorios/pdf_espelho.py:474`)

O contador esperado é lido diretamente do resumo da tela. Para a marca, o teste **deriva um booleano** por pertinência do HH:MM em `orfas_grade` daquele dia; portanto não lê um `batida_tela.orfa` pronto, mas também não reconstrói grade ou pareamento. (`app/relatorios/tests/test_cartao_marca_espuria.py:133`; `:140`; `:142`; `:144`)

**Fila:** isso prova a comparação de contador e marca sem célula, não a disposição visual completa. O positivo com ata verifica horário presente e ausência de “turno aberto”, mas não exige o asterisco nem a separação das marcações comuns. (`app/relatorios/tests/test_cartao_marca_espuria.py:109`; `:110`; `:112`; `:155`)

**4. Checklist das classes**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando autoridade | **Fecha quanto ao delta de classificação:** não reaparece grade própria no PDF; ele adapta `orfas_grade` recebido da tela. **Fila herdada:** PDF e tela continuam contando abertos com guardas diferentes. (`app/relatorios/pdf_espelho.py:474`; `:479`; `:516`; `app/ponto/services/espelho.py:436`) |
| Segundo escritor fora da porta | **Fecha no delta produtivo:** escreve somente atributo de apresentação `batida.orfa` e estruturas do retorno. A ata continua passando pela porta; os `update(ata=...)` novos são fixtures do teste. (`app/relatorios/pdf_espelho.py:568`; `app/ponto/portas/celula.py:1033`; `:1040`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:103`; `:137`) |
| Relógio fora da borda | **Fila herdada:** `agora` chega à tela, mas não a `folha_manda`, que consulta turno vivo e teto sem recebê-lo; ambos podem ler o relógio corrente. (`app/relatorios/pdf_espelho.py:474`; `:587`; `app/relatorios/cartao_pela_celula.py:398`; `:412`; `app/ponto/turnos.py:2290`; `app/relatorios/cartao_pela_celula.py:40`) |
| Derivação paralela de fonte única | **Bloqueia fechar a classe:** o filtro global de anomalias continua decidindo por coincidência textual de HH:MM. A contagem própria de abertos também permanece. (`app/relatorios/pdf_espelho.py:675`; `:679`; `:516`) |
| Selo/RED sem mordida | **Fecha para as operações novas examinadas:** há casos adversários de data, contador e marca, além da contraprova c16. Execução ORM e RED histórico: **[nao verificado]**. (`app/ponto/tests/test_lastro_c14_sem_banco.py:118`; `:129`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:130`) |
| Campo com dois significados | **Fecha a perda de data na guarda; fila nos leitores restantes:** o desenho ainda reduz marcas a HH:MM para suprimir alertas da linha. `orfa=False` continua podendo significar falta de informação disponível, pois ata antiga admite `?` e ausência de célula devolve vazio. (`app/relatorios/pdf_espelho.py:178`; `:919`; `:923`; `app/escala/services/leitor_celula.py:190`; `:390`; `:394`) |

**5. Escritores e leitores dos campos do delta e de suas fontes**

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `orfas` da grade | Pareamento com marcos; publicação por dia e conservação das não consumidas. (`app/escala/utils.py:1725`; `:1782`; `:1835`; `:1858`) | Cartório e adaptador de apresentação. (`app/ponto/services/cartorio.py:736`; `app/escala/utils.py:1113`) |
| `ata.orfas` | Cartório monta; porta atribui e persiste. (`app/ponto/services/cartorio.py:736`; `app/ponto/portas/celula.py:1033`; `:1040`) | Leitor conserva horas ou devolve `?`, publicando `orfas`. (`app/escala/services/leitor_celula.py:187`; `:190`; `:420`) |
| `dias[].orfas_grade` | Adaptador → espelho; sem grade/marcos pode devolver vazio. (`app/escala/utils.py:1087`; `:1113`; `app/ponto/services/espelho.py:699`; `:854`) | HTML, mapa PDF e esperado do selo. (`app/templates/ponto/espelho.html:240`; `app/relatorios/pdf_espelho.py:475`; `app/relatorios/tests/test_cartao_marca_espuria.py:143`) |
| `dias`, `data`, `batidas` | Espelho publica linhas e batidas agrupadas pelo juiz. Reclamadas recebem dia do turno; restantes recebem dia civil. (`app/ponto/services/espelho.py:579`; `:663`; `:775`; `:776`; `app/ponto/turnos.py:2953`; `:2955`) | PDF indexa classificação, seleciona timestamps e recorta linhas. (`app/relatorios/pdf_espelho.py:475`; `:478`; `:562`) |
| `batida.timestamp` | Registro da batida grava o instante recebido. (`app/ponto/registro_batida.py:157`) | Adaptador de períodos, ponte de dia e PDF. (`app/ponto/adaptador_motor.py:37`; `app/ponto/services/espelho.py:458`; `app/relatorios/pdf_espelho.py:478`; `:568`) |
| `resultado.periodos`, `periodos_ft`; `periodo.entrada`, `turno_aberto` | Motor acrescenta períodos e marca aberto quando falta saída; entrada vem do timestamp transportado pelo adaptador. (`app/ponto/motor_calculo_v2.py:2492`; `:2494`; `:2751`; `:2753`; `app/ponto/adaptador_motor.py:37`) | Guarda PDF e conta da tela. (`app/relatorios/pdf_espelho.py:178`; `:515`; `:516`; `app/ponto/services/espelho.py:424`; `:436`) |
| `_orfas_por_data`, `_orfas_instantes` | PDF transforma o contrato recebido em mapa por linha e conjunto de instantes. (`app/relatorios/pdf_espelho.py:475`; `:477`) | Construção do conjunto, guarda e marca. (`app/relatorios/pdf_espelho.py:479`; `:178`; `:516`; `:568`) |
| `resumo.turnos_abertos` | PDF inicia e incrementa; tela conta separadamente. Na competência, folha pode sobrescrever com o fechamento. (`app/relatorios/pdf_espelho.py:253`; `:517`; `app/ponto/services/espelho.py:437`; `app/relatorios/cartao_pela_celula.py:112`; `:346`) | Retorno PDF, HTML e selo comparativo. (`app/relatorios/pdf_espelho.py:603`; `app/templates/ponto/espelho.html:90`; `app/relatorios/tests/test_cartao_marca_espuria.py:133`) |
| `_abertos_ts`, `dias[].turnos`, `datas_turno_aberto` | PDF guarda entradas contadas; cards vêm da ponte instante→pk→dia da tela; PDF publica datas das linhas correspondentes. (`app/relatorios/pdf_espelho.py:514`; `:518`; `:576`; `:578`; `app/ponto/services/espelho.py:461`; `:777`) | `folha_manda` e desenho dos alertas. (`app/relatorios/cartao_pela_celula.py:396`; `:418`; `app/relatorios/pdf_espelho.py:921`) |
| `batida.orfa` | PDF atribui em memória; calendário tem outro escritor transitório. (`app/relatorios/pdf_espelho.py:568`; `app/colaboradores/services/calendario.py:627`) | Filtro global de anomalias, separação, asterisco, alertas e legenda. (`app/relatorios/pdf_espelho.py:675`; `:835`; `:844`; `:919`; `:983`) |

**Ressalva de origem:** `orfas_grade` nem sempre vem da ata: antes de `apur_ini` e no tratamento de `AtaAgregadaSemMarcos`, a tela usa o builder. O PDF recebe esses caminhos pelo mesmo contrato da tela. (`app/ponto/services/espelho.py:493`; `:523`; `:536`; `:542`; `:565`)

## 12/11 11:57 · sessao b126c59da370 · - · - · origem=Ronald (tmux)

Censo do delta e dos produtores/leitores:

| Campo | Escritor/origem | Leitores |
|---|---|---|
| `ata.orfas` | `ata_do_dia` copia `dia.orfas` como strings; porta atribui ata e persiste com evento. (`app/ponto/services/cartorio.py:727`, `:736`; `app/ponto/portas/celula.py:1006`, `:1033`, `:1040`, `:899`) | `_orfas_da_ata` conserva horas; ata antiga vira `?`; `grade_da_celula` publica. (`app/escala/services/leitor_celula.py:180`, `:187`, `:190`, `:420`) |
| `orfas_grade` | Adaptador devolve `gd.orfas`, espelho publica; sem grade/marcos devolve vazio. (`app/escala/utils.py:1087`, `:1113`; `app/ponto/services/espelho.py:699`, `:702`, `:854`) | HTML e PDF. (`app/templates/ponto/espelho.html:151`, `:240`; `app/relatorios/pdf_espelho.py:475`) |
| `dias[].data` | Espelho percorre datas da janela e publica a linha. (`app/ponto/services/espelho.py:663`, `:775`) | Indexação de órfãs e recorte PDF; datas dos turnos abertos. (`app/relatorios/pdf_espelho.py:475`, `:479`, `:562`, `:577`) |
| `dias[].batidas` | Juiz atribui data por `pk`; espelho agrupa e publica. (`app/ponto/turnos.py:2945`, `:2953`, `:2955`; `app/ponto/services/espelho.py:579`, `:665`, `:776`) | PDF recupera timestamps classificados e atribui apresentação. (`app/relatorios/pdf_espelho.py:478`, `:567`) |
| `batida.timestamp` | Campo persistido da batida; adaptador transporta como entrada de período. (`app/ponto/models.py:23`; `app/ponto/adaptador_motor.py:37`, `:43`) | Ponte de dia, seleção de órfãs, marca e desenho. (`app/ponto/services/espelho.py:458`; `app/relatorios/pdf_espelho.py:478`, `:568`, `:840`) |
| `periodo.entrada`, `periodo.turno_aberto` | Motor cria entrada localizada e marca aberto quando saída é `None`; pareamento vem da autoridade. (`app/ponto/motor_calculo_v2.py:509`, `:530`, `:2751`, `:2753`, `:2878`, `:2880`, `:3216`, `:3218`) | Guarda do resumo PDF e conta da tela. (`app/relatorios/pdf_espelho.py:178`, `:516`; `app/ponto/services/espelho.py:426`, `:436`) |
| `_orfas_por_data`, `_orfas_instantes` | PDF adapta a classificação lida da tela, restringindo HH:MM à própria linha e armazenando timestamp completo. (`app/relatorios/pdf_espelho.py:475`, `:477`) | Mesma coleção alimenta resumo e `batida.orfa`. (`app/relatorios/pdf_espelho.py:178`, `:516`, `:568`) |
| `batida.orfa` | PDF atribui atributo transitório, sem `save`. Outro escritor transitório existe no calendário; não encontrei chamada desse serviço pelo PDF. (`app/relatorios/pdf_espelho.py:568`; `app/colaboradores/services/calendario.py:622`, `:627`) | Filtro de anomalias, separação/asterisco, alertas da linha e legenda. (`app/relatorios/pdf_espelho.py:675`, `:679`, `:835`, `:844`, `:919`, `:923`, `:983`) |
| `resumo.turnos_abertos` | PDF inicia em zero e conta com guarda; tela conta sem guarda; na competência a autoridade folha pode sobrescrever pelo campo do fechamento. (`app/relatorios/pdf_espelho.py:253`, `:516`, `:517`, `:587`; `app/ponto/services/espelho.py:436`, `:437`; `app/relatorios/cartao_pela_celula.py:112`, `:346`) | Resumo HTML e retorno PDF; datas abertas alimentam `folha_manda`. (`app/templates/ponto/espelho.html:90`, `:92`; `app/relatorios/pdf_espelho.py:603`; `app/relatorios/cartao_pela_celula.py:396`, `:418`) |
| `_abertos_ts`, `datas_turno_aberto`, `dias[].turnos` | PDF guarda entradas contadas; data vem dos cards publicados pela tela, que usa ponte instante→pk→juiz. (`app/relatorios/pdf_espelho.py:518`, `:576`, `:578`; `app/ponto/services/espelho.py:453`, `:461`, `:463`, `:666`, `:777`) | Compositor de abertos e alertas do desenho. (`app/relatorios/pdf_espelho.py:587`, `:597`, `:921`; `app/relatorios/cartao_pela_celula.py:418`) |

**Atenção ao alcance da origem:** `orfas_grade` nem sempre vem de ata: o trecho anterior a `apur_ini` usa builder, assim como o ramo `AtaAgregadaSemMarcos`. O PDF recebe ambos pelo contrato da tela. (`app/ponto/services/espelho.py:493`, `:523`, `:533`, `:536`, `:542`)

**Fecho específico da data:** órfã 10/11 07:02 já não contamina entrada 11/11 07:02 no resumo nem na marca, porque ambos leem timestamps completos recuperados dentro da linha correta. (`app/relatorios/pdf_espelho.py:475`, `:479`, `:178`, `:516`, `:568`)

**Fila herdada comprovável por leitura:** o desenho ainda apaga a data de marcas para filtrar anomalias globalmente. Uma órfã 10/11 07:02 elimina do cabeçalho qualquer anomalia de outro dia contendo “07:02”; ocorrência concreta de produção [nao verificado]. (`app/relatorios/pdf_espelho.py:675`, `:679`; formato das anomalias em `app/ponto/adaptador_motor.py:34`, `:41`)

**Fila semântica:** a classificação original só distingue strings HH:MM; atas antigas podem conter `?`, sem célula retorna vazio, e o adaptador sem marcos também descarta órfãs. Portanto `batida.orfa=False` continua significando ausência de marca disponível, sem provar casamento legítimo. (`app/escala/services/leitor_celula.py:190`, `:390`, `:394`; `app/escala/utils.py:1087`; `app/relatorios/pdf_espelho.py:568`)

**Fila herdada de relógio:** `agora` chega à tela, mas não a `folha_manda`; esta chama turno vivo e teto sem repassar o instante. (`app/relatorios/pdf_espelho.py:474`, `:587`; `app/relatorios/cartao_pela_celula.py:398`, `:412`; `app/ponto/turnos.py:2290`, `:2291`; `app/relatorios/cartao_pela_celula.py:40`)

## 12/11 11:56 · sessao d257aac0c7f0 · - · - · origem=Ronald (tmux)

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `respondida_em` | `responder_pergunta_disputa_core`: instante `now`, texto junto, salva e registra ato (`app/chamados/services/disputa_emissao.py:1462`, `:1467`, `:1468`, `:1508`, `:1511`). `responder_automaticamente`: instante recebido (`:1652`, `:1653`, `:1655`). `carimbar_resposta_pela_trilha`: instante da nota recebido (`:1661`, `:1665`, `:1674`, `:1676`). `carimbar`: completa nulo, apenas com texto, pelo instante do carimbo (`app/chamados/models.py:3125`, `:3131`, `:3133`). `_nascer` conserva `**campos` recebidos (`:3015`, `:3018`, `:3020`). Limpeza: `reabrir_para_correcao` (`app/chamados/services/disputa_emissao.py:1593`, `:1630`, `:1632`), `ressuscitar_perguntas` (`app/chamados/models.py:1567`). | DateTimeField: instante, não dia (`app/chamados/models.py:2449`). C6 lê projeção temporal, ignora `> agora`, inclui igualdade (`app/chamados/juizes.py:1375`, `:1380`, `:1389`, `:1407`). App usa cru `None` + disputa aberta (`app/chamados/services/pergunta_no_app.py:70`, `:71`). Crédito separa cru nulo/não nulo (`app/chamados/services/raiox.py:135`, `:139`, `:140`). Fio usa `all(p.respondida_em…)` (`app/chamados/services/fio.py:340`). Portanto instante tem tipo comum, mas presença de carimbo recebe significados diferentes de pendência nesses leitores. |
| `validada_em` | `_carimbar_validacao`: `timezone.now`, salva e registra ato (`app/chamados/services/validacao.py:127`, `:134`, `:137`, `:139`). `carimbar(...resposta='validar')`: completa nulo com instante comum se texto (`app/chamados/models.py:3131`, `:3135`, `:3136`, `:3153`). `_nascer` conserva recebido (`:3015`, `:3020`). Limpam: nova resposta (`app/chamados/services/disputa_emissao.py:1471`, `:1480`, `:1508`), devolução (`app/chamados/services/validacao.py:199`, `:202`), reabertura e ressuscitação acima. | DateTimeField (`app/chamados/models.py:2450`). C6 ignora validação posterior a `agora`; presente no instante mata a pergunta antes do marco (`app/chamados/juizes.py:1378`, `:1391`). C19 usa decisão `VALIDACAO` e lâmpada: validada sem efeito pede materializar (`app/chamados/services/tarefa_do_dia.py:141`, `:145`, `:146`); não equivale a morte/encerramento. C19 inclui atos `ato_em <= agora` (`:389`, `:391`) e passivo `<= agora` (`:398`, `:399`). |
| `silenciado_ate`, autor, motivo | Escritores físicos `arquivo.silenciar` (`app/chamados/services/arquivo.py:141`, `:166`, `:171`) e `arquivo.levantar` (`:177`, `:202`, `:210`). Chamadores: arquivar/desarquivar/lote (`app/chamados/views_arquivar.py:37`, `:77`, `:140`); ausência (`app/chamados/services/silencio_ausencia.py:73`, `:87`); isentos (`app/chamados/services/silencio_isentos.py:43`); comandos competência/prescritos/sem dia (`app/chamados/management/commands/arquivar_competencia_encerrada.py:288`; `arquivar_prescritos.py:94`; `arquivar_sem_dia_irrecuperavel.py:94`). | Instante de vencimento, ativo exclusivamente `> agora`; igualdade já não silencia (`app/chamados/models.py:508`, `:512`; `app/chamados/services/painel.py:188`, `:190`). Partição usa o mesmo `>` para ARQUIVADO, mas campo preenchido e vencido vira HISTORICO (`app/chamados/services/verbos.py:48`, `:51`), enquanto `qs_na_fila` volta a incluí-lo: classificações diferentes após vencimento. Silêncio não altera `status_local` (`app/chamados/services/arquivo.py:154`). |
| ato `SILENCIO` | `DisputaSupervisao.fechar` nas portas admin/desligamento chama `fechar_por_admin` e registra ato com instante do carimbo (`app/chamados/models.py:1919`, `:1922`, `:1925`, `:1926`). | C19 interpreta como decisão que encerra ações (`app/chamados/services/tarefa_do_dia.py:150`, `:151`). É ato de pergunta, distinto do véu temporal do chamado, que não muda estado (`app/chamados/services/arquivo.py:154`; `app/chamados/models.py:3842`, `:3844`). |
| `status_local` do chamado | Juiz motor, transição humana (`app/chamados/models.py:123`, `:124`); escrita persistente `_gravar_estado` (`:201`, `:228`, `:296`). Nascimento `abrir`: motor decide e `get_or_create` (`:934`, `:936`, `:939`, `:972`); `criar`: motor decide e INSERT (`:1061`, `:1065`, `:1096`). | Censo lê status das âncoras (`app/chamados/services/vigia_fio.py:618`) e usa TERMINAIS (`:675`, `:687`). Fila exige COBRANCA (`app/chamados/services/painel.py:189`). Catálogo distingue VIVOS, COBRANCA, ENCERRADOS e TERMINAIS (`app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`). C6/lastro pode ler ENCERRADOS, não todo TERMINAIS (`app/chamados/models.py:330`, `:332`). Status terminal não prova pergunta morta; app não filtra status do chamado (`app/chamados/services/pergunta_no_app.py:70`, `:71`). Status concreto dentro de TERMINAIS dos 48: [nao verificado]. |
| `via_resolucao` | `_escrever_carimbo` atribui em memória (`app/chamados/models.py:3098`, `:3102`); persistem `carimbar` (`:3153`), identidade (`:3191`), descarimbo (`:3213`), lote (`:3266`, `:3267`). Exceções físicas: `encerrar_por_fato_no_espelho` (`:3276`, `:3277`), `relavra_via.relavrar` (`app/chamados/services/relavra_via.py:125`, `:127`). | C6 lê via associada à materialização; carimbo posterior ao horizonte é retirado, identidade fora do fato permanece (`app/chamados/juizes.py:1376`, `:1382`, `:1384`); vias que vencem resposta (`:1402`) e demais identidades (`:1428`) têm degraus próprios. C19 lê decisões/efeitos/lavra, não resume tudo à via (`app/chamados/services/tarefa_do_dia.py:116`, `:133`, `:141`, `:150`). Via é vocabulário de encerramento (`app/chamados/models.py:2546`); a via isolada não prova qual escritor histórico agiu [nao verificado]. |
| Dia ao qual R/V/via se referem | Instantes acima registram o ato; o dia é outro campo (`app/chamados/models.py:2449`, `:2450`; `app/chamados/juizes.py:334`). | `_dia_de_turno_da_pergunta` usa `data_evento` como dia de turno para fato; conversa grava dia civil e devolve `None` ao juiz de turno (`app/chamados/juizes.py:334`, `:335`, `:338`). C6 janela inclui as duas pontas (`:618`). C19 carrega fatos `conversa=False`, intervalo de dias inclusivo `gte/lte` (`app/chamados/services/tarefa_do_dia.py:373`, `:375`, `:376`). |

| Via raspada | Juiz/escritor atual comprovado |
|---|---|
| `celula_absolveu` | `perguntas_absolvidas_pela_celula` → `retratar_perguntas_absolvidas_por_celula` → `carimbar` (`app/chamados/reconciliador.py:585`, `:596`; `app/chamados/models.py:3140`, `:3153`). |
| `fantasma` | `veredito_orfa` exige luz `is True`, viva e dia encerrado (`app/chamados/reconciliador.py:1083`, `:1085`, `:1087`, `:1089`, `:1091`); consumidor chama `encerrar_fantasma` (`:1148`) → `carimbar` (`app/chamados/models.py:3280`, `:3288`). |
| `fato_ocupado` | Recusa COLISAO, ainda não materializada → `carimbar_identidade` (`app/chamados/services/materializacao.py:1097`, `:1099`, `:1105`). |
| `declaracao` / `ausencia_dia` | Absorção escolhe por `declarou_hora` e carimba (`app/chamados/services/materializacao.py:548`, `:555`, `:567`); materialização principal escolhe via e carimba com validação/lavra (`:1283`, `:1284`). |
| `chamado_encerrado` | `resolver_perguntas_por_chamado_resolvido`: exige C6 morta, escolhe `via_pelo_fato` ou fallback, chama carimbar (`app/chamados/services/disputa_emissao.py:2048`, `:2058`, `:2059`, `:2060`); seleção final da via (`app/chamados/juizes.py:1757`). |
| Autoria histórica individual dessas linhas | As portas atuais acima não provam autor, instante nem caminho que produziu cada registro raspado [nao verificado]. |

## 12/11 11:56 · sessao 6014d0662f17 · - · - · origem=Ronald (tmux)

**Q2 — fecha por inspeção; execução ORM [nao verificado].** O módulo importa diretamente `instantaneo`, `julgar_celula` e `recalcular_fechamento_mes`, chama os três e não instala duplos: `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:31`, `:34`, `:35`, `:79`, `:83`, `:89`.

- **Regra independente:** cadastro 08–17, intervalo 12–13 e jornada 480 (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:56`); quatro batidas reais na segunda (`:66`); ausência folga aprovada na terça (`:73`). Os esperados literais são 960 previstos, 2 dias, 480 abonados, 480 realizados (`:107`). A regra produtiva preserva ausência como jornada, inclui o previsto e abona a parte não realizada (`app/ponto/services/dia_pago.py:392`, `:398`, `:400`, `:402`).

- **Integração real:** o recalculador lê `grade_do_fechamento`, chama `por_dia_da_grade` e preenche previstos/abono/realizado (`app/ponto/services/fechamento.py:1014`, `:1047`, `:1048`, `:1062`, `:1064`); o leitor transporta os três metadados da ata (`app/escala/services/leitor_celula.py:436`). O simulador chama novamente o recalculador real e entrega seus objetos aos três consumidores (`app/folha/management/commands/simular_folha.py:129`, `:133`, `:134`, `:152`).

- **Morde, por raciocínio do código:** `return {}` antecipado reprova `assertIn` (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:85`); perda do transporte na ata reprova presença/tipo/id (`:95`, `:97`, `:98`); recalculador cego a `ausencia_tipo` reprova contraprova que exige 480→0 de abono e diferença 480 no previsto (`:130`, `:139`). Trocar a autoridade de `_retidos` pelo gravado, vazio nesta fixture, deixa `apto_folha=None` e reprova o acesso/asserção de percentagem (`app/folha/management/commands/simular_folha.py:76`, `:85`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:125`, `:128`). Execução desses mutantes [nao verificado].

- **Limites:** a ata antiga continua sintetizada da nova, removendo metadados e mudando `tipo_dia`; não é reprodução executada do cartório histórico (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:101`). Os asserts posteriores também comparam saídas antes/depois; não são todos valores independentes (`:116`, `:119`, `:123`). `ausencia_label` é cobrado por presença, sem conferir seu conteúdo (`:95`, `:98`).

- **Comentário impreciso:** “MESMO objeto” não é identidade demonstrada: `_lido()` e `_foto()` fazem chamadas independentes, embora a implementação atual do simulador transporte seus próprios objetos do recalculador (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`, `:89`, `:121`; `app/folha/management/commands/simular_folha.py:130`). A execução do ensaio com banco e o seu resultado permanecem [nao verificado].

## 12/11 11:56 · sessao a845178fa0c6 · - · - · origem=Ronald (tmux)

| Tela candidata | View/template e função real da lista | Classe c — 15 | Classe e — 48 |
|---|---|---|---|
| Drawer lateral, aba Mensagens | `app/colaboradores/views.py:915`, chamada `:940`, render `:990`; template `app/templates/colaboradores/partials/painel_colaborador.html:193`; lista real: `perguntas_do_app` → `consulta_do_app` → `projecao_do_app`, `app/chamados/services/pergunta_no_app.py:45`, `:61`, `:78`. | A pergunta respondida fica fora de `pendentes`: exige `respondida_em is None`; vai a `historico`, que esse bloco não renderiza. Outras perguntas do dia podem entrar; col516 tem `app=1` no dado. (`app/chamados/services/pergunta_no_app.py:71`, `:74`; `app/templates/colaboradores/partials/painel_colaborador.html:193`) | **Inclui as perguntas pendentes medidas por `app>0`**: usa a mesma consulta da sonda; não filtra status da âncora ou silêncio. Mostra pergunta, dia e marco. (`app/chamados/services/pergunta_no_app.py:49`, `:58`, `:71`, `:106`; `app/templates/colaboradores/partials/painel_colaborador.html:195`) |
| Modal do fio | `app/chamados/views.py:869`, montagem `:889`, render `:927`; template `app/templates/chamados/partials/modal_fio.html:68`, perguntas `:157`; lista real `montar_chamados_fio`, `app/chamados/services/fio.py:163`. | Chamados entram sem filtro de status/silêncio; pergunta viva entra tanto em disputa aberta quanto fechada. **Ressalva:** enumeração de disputas exige `chamado__isnull=False`, ausente no dado; inclusão individual permanece [nao verificado]. (`app/chamados/services/fio.py:177`, `:184`, `:221`, `:235`) | Mesmo predicado e mesma ressalva de FK. Terminal não exclui cartão/pergunta; não equivale a aparecer na fila principal. (`app/chamados/services/fio.py:177`, `:186`, `:225`, `:240`) |
| Página Fio administrativo | `app/chamados/views.py:659`, montagem `:677`, render `:697`; `app/templates/chamados/fio_colaborador.html:153`; lista real `montar_chamados_fio`, `app/chamados/services/fio.py:163`. | Inclui o chamado terminal pelo filtro apenas de colaborador; template renderiza cartões e followups, sem loop de `perguntas_disputa`. Ver o chamado não comprova ver a pergunta. (`app/chamados/services/fio.py:177`; `app/templates/chamados/fio_colaborador.html:161`, `:237`) | Mesmo comportamento. (`app/chamados/services/fio.py:177`; `app/templates/chamados/fio_colaborador.html:161`, `:237`) |
| Central/painel de gestão | `app/chamados/views.py:721`, chamada `:727`, templates `:740`, `:767`; lista real `montar_contexto_painel`, `app/chamados/services/painel.py:198`. | Âncora terminal **excluída do padrão** por `status_local__in=COBRANCA`. Drill explícito pelo status a inclui; `historico` inclui apenas resolvido/fechado, não cancelado/superado. Status concreto de cada âncora: [nao verificado]. (`app/chamados/services/painel.py:302`, `:306`, `:316`; `app/chamados/catalogo/motor.py:29`) | Mesmo predicado. (`app/chamados/services/painel.py:302`, `:306`, `:316`) |
| Pendências de disputa | `app/colaboradores/views.py:2998`, chamada `:3001`, template `:3011`; lista real `coletar`, `app/chamados/relatorios_disputa.py:26`. | **Condicional:** só disputa aberta entra. Dado não fornece `fechada_em`; inclusão dos 15 [nao verificado]. Se aberta, a categoria depende de TODAS as vivas da disputa: qualquer sem resposta → `AGUARDA_COLAB`; caso contrário → `AGUARDA_SUPERVISAO`. (`app/chamados/relatorios_disputa.py:18`, `:21`, `:40`) | **Disputa incluída**, pois pendência do app exige disputa aberta; classificação `AGUARDA_COLAB` pela pergunta sem resposta. A tela mostra pessoa e idade da disputa, não dias/perguntas individuais. (`app/chamados/services/pergunta_no_app.py:71`; `app/chamados/relatorios_disputa.py:21`, `:41`; `app/templates/colaboradores/partials/pendencias_disputa.html:6`) |
| Calendário do perfil | `app/colaboradores/views.py:1938`, chamada `:1949`, template `:1965`; lista real `contexto_calendario`, `app/colaboradores/services/calendario.py:159`; grade `app/templates/colaboradores/partials/_calendario_grade.html:67`. | **Dia incluído na janela escolhida**, sem filtro por classe/status de chamado: enumera datas inclusive. Âncora terminal não vira badge de ticket: tickets vêm de `VIVOS`; exceção agrupada exige pai vivo. Resolver dia ausente conforme dado e gate do template. (`app/colaboradores/services/calendario.py:437`, `:447`, `:603`, `:609`, `:721`; `app/templates/colaboradores/partials/_calendario_grade.html:125`) | Mesmo predicado: dia visível, pergunta não listada por essa grade. (`app/colaboradores/services/calendario.py:603`, `:721`; `app/templates/colaboradores/partials/_calendario_grade.html:67`) |
| Cadastro × Realidade | `app/escala/views.py:476`, chamada `:487`, template `:492`; lista real `lista`, `app/escala/services/cadastro_realidade.py:787`; ramo C11 `beco_em_lote`, `:442`. | Inclusão **[nao verificado]**: C11 exige ativo, `veredito='indefinida'`, protesto específico, sem porta, viva sob terminal e `folga_nao_definida`; o recorte não informa todos esses campos. (`app/escala/services/cadastro_realidade.py:477`, `:488`, `:515`, `:518`) | Mesma insuficiência. Sem porta + terminal não bastam para provar presença na lista. (`app/escala/services/cadastro_realidade.py:477`, `:518`) |

## 12/11 11:55 · sessao d4ac8dbd5e32 · - · - · origem=Ronald (tmux)

| Obra | PRONTO literal curto na cópia | Cobertura comprovável e limite |
|---|---|---|
| E3-LEITORES | “fio, tela admin, API/app, badge/PWA, o_que_segura, vigia_fio leem SO tarefa_do_dia; 5 respondedores morrem com certidao” (`app/docs/BACKLOG.md:1873`) | Cobre migração dos leitores de todas as classes; não decide por si a ação do estado d-241. O PRONTO do anexo exige “V curada (ou 0)”, mas o dado não traz classificação V/T/X desses 241. (`avais_e3_leitores.txt:5`, `avais_e3_leitores.txt:9`; linhas contadas no anexo colado, arquivo físico [nao verificado]) |
| E4B | “backfill do fio morto (matriz_e4 C3, classes a-e) pela porta canonica” (`app/docs/BACKLOG.md:1894`) | O universo executável é só `sem_porta`; os 21 `porta_fila` ficam fora desse comando. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`) |
| E4B, cura e | PRONTO acima. (`app/docs/BACKLOG.md:1894`) | `reabrir_credor_de_pergunta_viva` cobre pergunta sem resposta, viva, com âncora fora de VIVOS; veta limbo/exportadas/recusa da reabertura. Portanto cobre a classe e-48 em escopo, sem prova de cura dos 48; c-15 respondida é excluída dessa cura. (`app/chamados/services/disputa_emissao.py:1805`, `:1821`, `:1827`, `:1835`, `:1839`, `:1848`, `:1851`) |
| E4B, demais atos | PRONTO acima. (`app/docs/BACKLOG.md:1894`) | Consulta C19, registra passivo sem linha, pergunta e materializa conforme ação; `tarefa_nada` produz rótulo, sem ato; `rejulgar` só retorna aviso. Não prova cura de d-241, a-2 ou e-sem-viva-1 pelos agregados. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `:98`, `:101`, `:117`, `:138`, `:142`, `:144`, `:148`) |
| E4-ESCRITORES/E4C/E4D | “16 portas de estado da pergunta encolhem para efeitos + decisoes de E1”; “E4c LEITORES DIVERGE + selo 2 -> E4d parar projecao” (`app/docs/BACKLOG.md:1885`) | Cobre troca de representação/leitores/escritores; o trecho não promete ação humana específica nem transição individual dessas nove classes. (`app/docs/BACKLOG.md:1885`) |
| E1-ENTRADAS | “ampliada ao censo de toda porta do fato com as 3 curas de classe no mesmo commit” (`app/docs/BACKLOG.md:1859`) | Cobre portas do fato em âmbito estrutural; atribuir qualquer desses estados concretos a uma das três curas, sem dados adicionais, permanece [nao verificado]. |
| RAIZ | “elo raiz<-filhos: PerguntaDisputa com escritores fora do agregado, DisputaSupervisao nao reage”; sequência F0–F7. (`app/docs/BACKLOG.md:1904`) | Cobre a reação do agregado e o problema da dona fechada em âmbito estrutural; o PRONTO fornecido não identifica qual cura individual alcança c-15/e-48 ou os seis `cobrar` sem app. (`app/docs/BACKLOG.md:1904`) |
| PORTA-QUE-NAO-AGE | Substituída por MAPA-DO-FIO-MORTO no anexo. (`anexo_backlog.txt:11`, arquivo físico [nao verificado]) | Não usar a antiga promessa de 311→326 como cobertura ativa; o aval MAPA é medida, nenhum código. (`aval_mapa_do_fio_morto_1211.txt:2`, `:4`, arquivo físico [nao verificado]) |

**Pergunta 2: NÃO.** Nos anexos `avais_e3_leitores.txt` e `censo.r` não há decisão específica sobre o que C19 deve mandar — ou que deva mandar nada — quando o dia retém a folha e C19 não envia ação. A fase V/T/X é procedimento para decidir; “conservar o universo” e distinguir nada/desconhecido/sem porta não escolhem a ação desse estado. (`avais_e3_leitores.txt:4`, `:5`, `:10`, `:18`; `censo.r`, seção 3, decisões 4–5; arquivos físicos [nao verificado])

Atenção: a cópia possui relato histórico que discutiu esse desacordo e reclassificou universos anteriores; não equivale a decisão escrita no brief/censo anexados nem classifica os atuais 241. (`app/docs/RELATO.md:363`, `:365`, `:370`)

Diferença essencial para d-2: `censo_alcance` lê vivas por **FK célula**; a sonda lê fatos por **data_evento** e o app possui outro universo. Portanto `d + COLAB_RESPONDE` não autoriza concluir incoerência de C6 nem identificar o escritor concreto. (`app/chamados/services/vigia_fio.py:610`, `:614`, `:640`, `:677`; `sonda_mapa_fio.py`, laço de `ps_dia`/`pend_app`, arquivo físico [nao verificado])

## 12/11 11:49 · sessao 6d7e592f4fec · AUDITORIA-DE-TRAVAS-CORTE · verificador · origem=Code

REPROVADO

As curas de medição e precedência estão presentes e os seis mutantes declarados caem no log fornecido. Restam divergências com o aval literal, um aviso truncado para nomes maiores e a possibilidade de gastar novamente a cota herdada em `auto`. Ocorrência desses caminhos em produção: `[nao verificado]`. (`bin/disjuntor.py:1024`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:210`; `item2v2_mutantes.log:21`, `item2v2_mutantes.log:76`)

As referências aos anexos contam linhas desde o primeiro conteúdo de cada anexo. Comparação pelo histórico Git do intervalo informado e execução integral local do selo: `[nao verificado]`. O script cria, altera e apaga arquivos; minha avaliação da execução usa o log anexado. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:51`, `bin/tests/test_disjuntor.sh:1676`)

**Delta dos achados da volta 1:**

| Achado | A volta 2 cura? | O selo morde? |
|---|---|---|
| Aviso da segunda AUTO | **Parcialmente.** `auto_esgotada` governa tanto a mensagem quanto o aviso do disparo. Porém `trilha()` corta o detalhe em 300 caracteres: para `AUDITORIA-DE-TRAVAS-CORTE`, com uma reprovação final, o aviso tem 317 e termina em `so linha`, perdendo `/rearma do Ronald`. (`bin/disjuntor.py:1031`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:1183`, `bin/disjuntor.py:1194`, `bin/disjuntor.py:210`) | **Sim, para a retirada do aviso**, com `OBRA-A`; não cobre o truncamento acima. O mutante falha no caso 6, rc=6. (`bin/tests/test_disjuntor.sh:1233`, `bin/tests/test_disjuntor.sh:1239`, `bin/tests/test_disjuntor.sh:1988`; `item2v2_mutantes.log:51`, `item2v2_mutantes.log:55`) |
| Testemunha positiva de `lib['auto']` | **Sim.** O caso 7 exige `N >= 1`, igualdade entre trilha e medidor e presença de `auto=N` na apresentação. (`bin/tests/test_disjuntor.sh:1241`, `bin/tests/test_disjuntor.sh:1249`) | **Sim.** Retirar `auto` do medidor produz trilha=3, medidor=0 e rc=7. (`bin/tests/test_disjuntor.sh:1991`; `item2v2_mutantes.log:63`, `item2v2_mutantes.log:66`) |
| Contrato FALHA × SOBE | **O contrato fornecido está corrigido:** agora pede SOBE, como o código. Não é uma troca de evento no código, que já usava SOBE na volta 1. (`contrato_item2.txt:4`; `bin/disjuntor.py:1053`; `preverif_item2.r`, resposta 1 — localização exata `[nao verificado]`) | O caso 2 exige exatamente um SOBE; trocar o evento por FALHA violaria esse assert. Não há mutante específico dessa troca entre os seis declarados. (`bin/tests/test_disjuntor.sh:1181`, `bin/tests/test_disjuntor.sh:1190`, `bin/tests/test_disjuntor.sh:1986`) |
| Nomes da cota e da medida | **Sim, na representação atual:** `auto_usada` é a lista cumulativa por obra; `lib['auto']` conta eventos na janela do painel. **Há ressalva de compatibilidade:** a guarda ignora um antigo `r['auto']`. (`bin/disjuntor.py:150`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:2025`) | Os casos 1/2 verificam `auto_usada` e o 7 verifica a medida positiva. Não há caso nem mutante declarado para preservar cota anteriormente gravada em `auto`. (`bin/tests/test_disjuntor.sh:1174`, `bin/tests/test_disjuntor.sh:1184`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1986`) |
| Precedência DESENHO/ERRO-BOBO × AUTO | **Sim.** A ordenação estável põe ambas as portas antigas antes de AUTO, preservando sua ordem relativa; a cota só é gasta se nenhuma candidata antiga elegível liberar. (`bin/disjuntor.py:1043`, `bin/disjuntor.py:1046`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:1065`) | **Sim para a composição com ERRO-BOBO:** sem ordenação, gasta AUTO e o caso 8 cai, rc=8. DESENHO participa da mesma regra, mas não tem composição própria no caso 8. (`bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1992`; `item2v2_mutantes.log:75`, `item2v2_mutantes.log:77`) |

**1. O diff realiza literalmente o item 2?**

**Não integralmente.** O disparo continua sendo por quantidade de reprovações finais ou voltas Codex procedentes, sem consultar FORMA/ERRO-BOBO/DESENHO; a chamada que dispara retorna PARADA antes de examinar liberações. A classe é examinada na linha posterior `AUTOLIBERA`. Isso implementa o mecanismo de porta separada do contrato, mas não um disparo condicionado literalmente a DESENHO. (`aval_auditoria_de_travas_corte_1211.txt:4`; `contrato_item2.txt:2`; `bin/disjuntor.py:656`, `bin/disjuntor.py:1024`, `bin/disjuntor.py:1033`, `bin/disjuntor.py:767`)

A segunda ocorrência também não fica exclusivamente dependente do Ronald: a cota restringe apenas `tipo == 'auto'`; uma nova linha antiga `ERRO-BOBO` libera sem cota nem comprovação de autoria. O próprio guardião descreve essa porta como disponível ao Code. Isso preserva o contrato, mas diverge da leitura literal “a 2a da mesma obra sobe a mim”. (`aval_auditoria_de_travas_corte_1211.txt:4`; `contrato_item2.txt:2`; `bin/disjuntor.py:239`, `bin/disjuntor.py:761`, `bin/disjuntor.py:1050`)

A porta nova exige classe aceita e motivo com letra ou dígito; registra a liberação com tipo `auto` e a linha original, incluindo a classe. Esse trecho atende ao rastro exigido. (`bin/disjuntor.py:148`, `bin/disjuntor.py:767`, `bin/disjuntor.py:1062`)

**2. Quem escreve e quem lê?**

| Campo/valor | Escritores | Leitores |
|---|---|---|
| Antigo `est['b3'][obra]['auto']` | Nenhum escritor específico identificado na implementação atual; o JSON genérico preserva chaves existentes. (`bin/disjuntor.py:378`, `bin/disjuntor.py:383`, `bin/disjuntor.py:1057`) | Nenhum leitor semântico atual da cota: a guarda lê exclusivamente `auto_usada`. Os carregadores genéricos leem o objeto inteiro. (`bin/disjuntor.py:371`, `bin/disjuntor.py:405`, `bin/disjuntor.py:1177`) |
| `auto_usada` | `decide_b3` acrescenta o SHA quando AUTO libera; `_com_estado` persiste. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:383`) | `auto_esgotada`, chamado pela recusa, pelo aviso do disparo e por `mensagem_b3`; testes 1, 2 e 8. (`bin/disjuntor.py:1050`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1194`; `bin/tests/test_disjuntor.sh:1174`, `bin/tests/test_disjuntor.sh:1184`, `bin/tests/test_disjuntor.sh:1256`) |
| `auto_sobe` | `decide_b3` acrescenta SHA recusado, persistido pela mesma porta. (`bin/disjuntor.py:1052`, `bin/disjuntor.py:1081`) | A própria deduplicação de SOBE. (`bin/disjuntor.py:1051`) |
| `lib['auto']` → `b3_libera['auto']` | `ler()` inicializa e incrementa a partir de eventos LIBERA reconhecidos após o corte temporal. (`bin/disjuntor.py:2021`, `bin/disjuntor.py:2025`, `bin/disjuntor.py:2048`, `bin/disjuntor.py:2093`) | `linha()`; `gerar_estado.py` via `ler/linha`; CLI `--linha`; testes antigo e positivo novo. (`bin/disjuntor.py:2310`, `bin/disjuntor.py:2313`, `bin/disjuntor.py:2395`; `bin/gerar_estado.py:280`; `bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:1246`) |
| Evento `SOBE` | `decide_b3` prepara; `_anuncia` chama `trilha()` somente após persistência bem-sucedida. (`bin/disjuntor.py:1053`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1073`) | Leitor específico identificado: `conta()` no selo. O medidor do ESTADO não enumera SOBE; o leitor de censo o descarta pelo reconhecedor. (`bin/tests/test_disjuntor.sh:1160`, `bin/tests/test_disjuntor.sh:1181`; `bin/disjuntor.py:2027`, `bin/disjuntor.py:2097`, `bin/disjuntor.py:2140`) |
| SHA consumido | `_sha_b3` calcula; `decide_b3` acrescenta a `consumidas`. `_fotografa` grava os SHA anteriores em `topo_no_disparo`. (`bin/disjuntor.py:681`, `bin/disjuntor.py:1061`, `bin/disjuntor.py:836`, `bin/disjuntor.py:845`) | Guarda contra reutilização/anterioridade; AUTO aceita também o armazena em `auto_usada`, e AUTO recusada em `auto_sobe`. (`bin/disjuntor.py:1046`, `bin/disjuntor.py:1052`, `bin/disjuntor.py:1057`) |
| `B3_AUTO_COTA` | Constante definida como 1. (`bin/disjuntor.py:149`) | Predicado da cota, detalhe de SOBE e instrução da mensagem. (`bin/disjuntor.py:1177`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:1187`) |
| Classe/tipo `auto` | Code escreve a linha conforme contrato; o reconhecedor produz a candidata `('auto', linha)`. A fixture escreve pelo auxiliar `rel()`. (`contrato_item2.txt:2`; `bin/disjuntor.py:768`; `bin/tests/test_disjuntor.sh:1151`) | Reconhecedor valida classe/motivo; decisão aplica precedência/cota; LIBERA leva o tipo ao medidor. (`bin/disjuntor.py:767`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:2048`) |

Os leitores decisórios fora do trecho alterado são:

- **Stop:** `hook_stop_fila1 → julga_b3 → decide_b3`; para cada parada, chama `mensagem_b3`, que agora relê a cota persistida. (`bin/hook_stop_fila1.py:1204`, `bin/hook_stop_fila1.py:1206`; `bin/disjuntor.py:1102`, `bin/disjuntor.py:1194`)
- **PreToolUse:** `_nega_b3 → estado_b3 → julga_b3 → decide_b3`; PARADA recebe `mensagem_b3`. (`bin/disjuntor.py:1826`, `bin/disjuntor.py:1828`, `bin/disjuntor.py:1111`, `bin/disjuntor.py:1102`)
- **Wrapper:** `codex_pergunta.sh → --b3-parada → b3_parada → estado_b3`; retorno não zero recusa a volta. (`bin/codex_pergunta.sh:378`; `bin/disjuntor.py:2407`, `bin/disjuntor.py:1164`)
- **ESTADO:** `gerar_estado.py` recebe a medida por `ler/linha`, incorpora `dj_txt` e grava a página. Não usa a cota como contador do painel. (`bin/gerar_estado.py:281`, `bin/gerar_estado.py:378`, `bin/gerar_estado.py:390`; `bin/disjuntor.py:2025`)
- **Carregadores genéricos:** os demais consumidores de `ler_estado` selecionam B1, B4 ou listas históricas; a referência do hook aos arquivos do disjuntor serve para rejeitá-los como prova de espera. (`bin/disjuntor.py:600`, `bin/disjuntor.py:1907`, `bin/disjuntor.py:2238`; `bin/hook_stop_fila1.py:664`)

**3. Segunda AUTO, DESENHO e preservação da cota.**

**Estado atual, segunda AUTO sozinha:** não libera; acrescenta o SHA recusado a `auto_sobe`, anuncia uma vez e não o acrescenta a `consumidas`. Duas chamadas e a preservação após rearme são exercitadas pelo caso 2. (`bin/disjuntor.py:1050`, `bin/disjuntor.py:1055`, `bin/disjuntor.py:1061`; `bin/tests/test_disjuntor.sh:1176`, `bin/tests/test_disjuntor.sh:1190`; `item2v2_mutantes.log:3`)

**Segunda ocorrência pela porta antiga:** com cota gasta, obra disparada e linhas novas posteriores à fotografia:

```text
AUTOLIBERA OBRA-A: classe=FORMA segundo erro
ERRO-BOBO OBRA-A: segundo erro escrito pelo Code
```

A linha antiga libera primeiro. Na volta 2, diferentemente do contraexemplo anterior, essa composição nem chega a produzir SOBE para a AUTO, pois encerra o loop na antiga. Não há autenticação do autor no reconhecedor. (`bin/disjuntor.py:761`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1065`; `bin/tests/test_disjuntor.sh:1218`)

**DESENHO pela porta nova:** `classe=DESENHO` é rejeitada. A validação é do rótulo, portanto `classe=FORMA erro de desenho` pode ser aceita; a natureza real do defeito não é julgada pelo reconhecedor. Classificação correta em produção: `[nao verificado]`. (`bin/disjuntor.py:148`, `bin/disjuntor.py:767`; `bin/tests/test_disjuntor.sh:1195`)

**Rearme/reinício com estado atual:** rearme não apaga `auto_usada`; a próxima decisão relê o JSON. Se o arquivo desaparecer, a inicialização parte de `{}`; isso é perda de estado, não um efeito necessário do reinício. (`bin/disjuntor.py:1140`, `bin/disjuntor.py:1142`, `bin/disjuntor.py:370`, `bin/disjuntor.py:372`)

**Contraexemplo de compatibilidade:** obra disparada, demais condições de liberação satisfeitas, estado contendo `auto=['sha-da-primeira']` e sem `auto_usada`; uma AUTO nova libera e passa a criar `auto_usada=[sha-da-segunda]`. Não há conversão da chave antiga na migração existente. O contrato novo explicitamente define ausência de `auto_usada` como zero, portanto esse comportamento obedece ao contrato, mas não preserva “1x por obra” entre versões. Existência desse estado em produção: `[nao verificado]`. (`bin/disjuntor.py:808`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:1177`; `contrato_item2.txt:4`)

**4. As linhas DESENHO/ERRO-BOBO antigas mudaram?**

Seus validadores continuam com as mesmas exigências mostradas no diff, e a ordenação nova preserva sua ordem relativa. A cota AUTO não impede essas linhas de liberar. (`bin/disjuntor.py:750`, `bin/disjuntor.py:761`, `bin/disjuntor.py:1043`, `bin/disjuntor.py:1050`)

Na composição introduzida na volta 1, há mudança corretiva: AUTO antes de ERRO-BOBO deixa de gastar cota e consumir o SHA da AUTO; a antiga vence e recebe o evento LIBERA. Isso é demonstrado pelo caso 8. O mesmo critério de ordenação inclui DESENHO. (`bin/disjuntor.py:1043`, `bin/disjuntor.py:1061`, `bin/disjuntor.py:1062`; `bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1261`)

**5. O selo morde cada mutante? Há assert vazio?**

**Sim, os seis declarados mordem no log fornecido; o controle sem mutação passa.** (`item2v2_mutantes.log:10`, `item2v2_mutantes.log:11`)

| Mutante | Mordida identificada |
|---|---|
| `auto-libera-a-2a` | Caso 2: segunda libera e cota chega a 2; falhas `[2]`, rc=2. (`bin/tests/test_disjuntor.sh:1190`; `item2v2_mutantes.log:14`, `item2v2_mutantes.log:21`) |
| `auto-libera-desenho` | Caso 3: DESENHO reconhecida e liberada; falhas `[3]`, rc=3. (`bin/tests/test_disjuntor.sh:1199`; `item2v2_mutantes.log:26`, `item2v2_mutantes.log:32`) |
| `sem-classe-libera` | Casos 3/4: reconhece DESENHO e libera os quatro inválidos; falhas `[3, 4]`, rc=3. (`bin/tests/test_disjuntor.sh:1199`, `bin/tests/test_disjuntor.sh:1210`; `item2v2_mutantes.log:37`, `item2v2_mutantes.log:43`) |
| `sem-aviso-da-2a` | Caso 6: mensagem e aviso perdem a marca; falhas `[6]`, rc=6. (`bin/tests/test_disjuntor.sh:1239`; `item2v2_mutantes.log:51`, `item2v2_mutantes.log:54`) |
| `medidor-sem-auto` | Caso 7: trilha=3, medidor=0; falhas `[7]`, rc=7. (`bin/tests/test_disjuntor.sh:1249`; `item2v2_mutantes.log:63`, `item2v2_mutantes.log:65`) |
| `auto-sem-precedencia` | Caso 8: ERRO-BOBO=0, AUTO=1, cota gasta e AUTO posterior recusada; falhas `[8]`, rc=8. (`bin/tests/test_disjuntor.sh:1261`; `item2v2_mutantes.log:75`, `item2v2_mutantes.log:76`) |

Não há `all([])` nos controles atuais: `rs` recebe quatro resultados e `ctrl`, três. O positivo do medidor também exige pelo menos um evento. (`bin/tests/test_disjuntor.sh:1205`, `bin/tests/test_disjuntor.sh:1208`, `bin/tests/test_disjuntor.sh:1214`, `bin/tests/test_disjuntor.sh:1219`, `bin/tests/test_disjuntor.sh:1249`)

Persistem fragilidades: o caso 5 ignora o retorno de `dispara()` e só exige ausência no mapa de paradas; o caso 8 também ignora o segundo disparo e não exige LIBERA(auto)/cota após a tentativa final. São verificações que podem passar sem provar aquela transição se a preparação falhar; o log fornecido não demonstra esse esvaziamento. (`bin/tests/test_disjuntor.sh:1217`, `bin/tests/test_disjuntor.sh:1219`, `bin/tests/test_disjuntor.sh:1257`, `bin/tests/test_disjuntor.sh:1261`)

O harness exige âncora única e inclui controle da cópia sem mutação. Isso sustenta os mutantes declarados, sem cobrir automaticamente truncamento, migração da chave antiga ou composição AUTO+DESENHO. (`bin/tests/test_disjuntor.sh:1670`, `bin/tests/test_disjuntor.sh:1674`, `bin/tests/test_disjuntor.sh:1696`, `bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1986`)

**6. Classes que já reprovaram obra.**

| Classe | Resultado |
|---|---|
| Segundo escritor fora da porta | **Não identificado para a cota atual.** A alteração específica de `auto_usada` está em `decide_b3`, persistida por `_com_estado`. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:1081`) |
| Relógio fora da borda | **Presente, herdado:** `decide_b3` chama `time.time()` quando `agora` falta; `b3_parada` não fornece esse instante. `trilha()` também lê `datetime.now()`. A nova cota é independente de relógio. (`bin/disjuntor.py:992`, `bin/disjuntor.py:1164`, `bin/disjuntor.py:210`, `bin/disjuntor.py:1177`) |
| Selo sem mordida | **Não para os seis declarados.** Há lacunas concretas de cobertura, descritas acima, e o nome curto do caso 6 deixa passar o truncamento. (`item2v2_mutantes.log:21`, `item2v2_mutantes.log:32`, `item2v2_mutantes.log:43`, `item2v2_mutantes.log:54`, `item2v2_mutantes.log:65`, `item2v2_mutantes.log:76`; `bin/tests/test_disjuntor.sh:1236`; `bin/disjuntor.py:210`) |
| Campo com dois significados | **Curado nos nomes atuais:** `auto_usada` e `lib['auto']` são células distintas para cota e medida. A compatibilidade com o antigo `auto` permanece descoberta. (`bin/disjuntor.py:150`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:2025`) |
| Ordem de trava | **Não identifiquei inversão acrescentada.** Decisão, persistência e anúncio permanecem sob a trava do estado; wrapper e triagem adquirem resposta antes de estado. A nova leitura da mensagem não adquire outra trava. (`bin/disjuntor.py:363`, `bin/disjuntor.py:395`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:901`, `bin/disjuntor.py:933`, `bin/disjuntor.py:404`; `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`) |

“Sobe ao Ronald” tem evidência de evento SOBE e de mensagens locais; entrega efetiva ao Ronald permanece `[nao verificado]`. O ESTADO não apresenta SOBE como evento próprio. (`bin/disjuntor.py:1053`, `bin/disjuntor.py:1183`, `bin/disjuntor.py:2027`)

## 12/11 11:48 · sessao 3d6205c84e19 · PORTA-QUE-NAO-AGE · sonda · origem=Code

Os valores abaixo vêm exclusivamente do JSON fornecido. Nas tabelas, `[nao verificado]` marca a ausência de comprovação desses valores por arquivo:linha da cópia; as referências ao código explicam os campos.

1. `medicao_completa = true`; `dias_erro = 0`; `dias_medidos = 15`, de `dias_fixos = 15`. Não há dia com erro, etapa/função de falha ou classe de erro reportados. [nao verificado]

2. Tabela por dia.

`F(ids)` representa o objeto completo: `rota = porta_fila`, `fio = chamado`, `fila = [ids]` e `verbos = {cada id: decidir}`. Esses valores são os reportados no JSON. [nao verificado] O formato da porta está em `app/chamados/juizes.py:1657`; os significados de fila, verbos e fio, em `app/chamados/juizes.py:1621`.

A coluna resolver transcreve `resolver_dia_oferecido_nas_datas`; dono transcreve `dono_fora_do_fio_chamado`. O mapeamento exato desse último campo raspado à função do código é [nao verificado]. `motivos_a_perguntar` devolve os motivos que ainda pedem pergunta (`app/chamados/services/fabrica_da_celula.py:92`).

| col | data | porta_alcancavel_do_dia | resolver | n perguntas | n vivas | motivos_a_perguntar | dono | Evidência |
|---|---|---|---|---:|---:|---|---|---|
| col129 | 2026-10-25 | F(35101) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col129 | 2026-11-01 | F(36592, 37904) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col129 | 2026-11-02 | F(37905) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col129 | 2026-11-08 | F(37925) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col258 | 2026-10-30 | F(35974) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |
| col258 | 2026-11-06 | F(37406) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |
| col258 | 2026-11-09 | F(38110) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |
| col427 | 2026-09-21 | F(28969) | False | 0 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-21 | F(33286) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-23 | F(34254) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-25 | F(34850) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col451 | 2026-10-27 | F(35331) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col502 | 2026-10-28 | F(35432) | False | 4 | 0 | vazio `[]` | False | [nao verificado] |
| col899 | 2026-10-24 | F(34431) | False | 2 | 0 | vazio `[]` | False | [nao verificado] |
| col999 | 2026-10-28 | F(35602, 35697) | False | 1 | 0 | vazio `[]` | False | [nao verificado] |

3. Tabela por pergunta.

Respondida/validada indica exclusivamente o preenchimento do respectivo timestamp, campos definidos em `app/chamados/models.py:2449` e `app/chamados/models.py:2450`. `validada_por` está em `app/chamados/models.py:3378`; `via_resolucao`, em `app/chamados/models.py:2562`. Vitalidade é calculada em `app/chamados/juizes.py:1389`; o marco admite `True`, `False` e `None` (`app/chamados/juizes.py:930`).

Datas e horas abaixo são locais, todas de 2026, com offset `-03:00`; os microssegundos foram preservados. [nao verificado] “Quem carimbou” usa somente `auditoria_com_payload_da_via_atual`, conforme solicitado; autoria por outros campos ou decisões permanece [nao verificado]. Os campos de usuário e criação da auditoria estão em `app/core/models.py:295` e `app/core/models.py:307`.

| pk | col / dia | motivo | via_resolucao | Respondida? / local | Validada? / local | validada_por_id | pergunta_viva | fato_do_marco_no_espelho | Quem carimbou: usuario_id / criado_em_local / auditoria pk | Evidência |
|---:|---|---|---|---|---|---:|---|---|---|---|
| 50169 | col129 / 10-25 | saida_sem_entrada | fantasma | sim / 10-27 07:53:49.378656 | sim / 10-28 09:45:05.311132 | 657 | False | False | [nao verificado] | [nao verificado] |
| 50170 | col129 / 10-25 | orfao_14h | fantasma | não / null | não / null | null | False | True | [nao verificado] | [nao verificado] |
| 52730 | col129 / 11-01 | saida_sem_entrada | declaracao | sim / 11-02 18:07:34.143947 | sim / 11-09 07:44:34.128888 | 657 | False | False | [nao verificado] | [nao verificado] |
| 53028 | col129 / 11-01 | orfao_14h | fantasma | não / null | não / null | null | False | True | 666 / 11-02 09:42:03.090529 / 845648 | [nao verificado] |
| 53288 | col129 / 11-02 | saida_sem_entrada | declaracao | sim / 11-03 06:31:00.239353 | sim / 11-09 07:44:49.944781 | 657 | False | False | [nao verificado] | [nao verificado] |
| 53333 | col129 / 11-02 | orfao_14h | declaracao | sim / 11-09 06:41:16.655654 | sim / 11-09 07:44:39.581541 | 657 | False | True | [nao verificado] | [nao verificado] |
| 55524 | col129 / 11-08 | saida_sem_entrada | declaracao | sim / 11-09 06:31:03.566947 | sim / 11-09 07:46:49.602920 | 657 | False | False | [nao verificado] | [nao verificado] |
| 55594 | col129 / 11-08 | orfao_14h | declaracao | sim / 11-09 06:31:38.641578 | sim / 11-09 07:51:24.055621 | 657 | False | True | [nao verificado] | [nao verificado] |
| 52131 | col258 / 10-30 | orfao_14h | fantasma | não / null | não / null | null | False | True | 666 / 11-10 12:21:02.666408 / 927797 | [nao verificado] |
| 54827 | col258 / 11-06 | orfao_14h | fantasma | não / null | não / null | null | False | True | 666 / 11-10 12:16:02.672755 / 927673 | [nao verificado] |
| 56040 | col258 / 11-09 | orfao_14h | ausencia_dia | não / null | não / null | null | False | True | [nao verificado] | [nao verificado] |
| 46703 | col451 / 10-21 | saida_sem_entrada | fantasma | sim / 10-24 19:12:21.246122 | sim / 10-26 14:54:46.529352 | 657 | False | False | [nao verificado] | [nao verificado] |
| 46704 | col451 / 10-21 | orfao_14h | declaracao | sim / 10-24 19:12:31.807198 | sim / 10-29 11:33:16.500910 | 657 | False | True | [nao verificado] | [nao verificado] |
| 49422 | col451 / 10-23 | saida_sem_entrada | fantasma | sim / 10-24 19:12:52.450098 | sim / 10-26 14:54:46.995846 | 657 | False | False | [nao verificado] | [nao verificado] |
| 49423 | col451 / 10-23 | orfao_14h | declaracao | sim / 10-24 19:13:02.454158 | sim / 10-29 11:33:43.367442 | 657 | False | True | [nao verificado] | [nao verificado] |
| 50179 | col451 / 10-25 | saida_sem_entrada | fantasma | sim / 10-28 18:56:55.079529 | sim / 10-29 11:33:57.614654 | 657 | False | False | [nao verificado] | [nao verificado] |
| 50180 | col451 / 10-25 | orfao_14h | declaracao | sim / 10-28 18:57:04.067558 | sim / 10-29 11:34:01.448378 | 657 | False | True | [nao verificado] | [nao verificado] |
| 50883 | col451 / 10-27 | saida_sem_entrada | fantasma | sim / 10-28 18:57:23.247879 | sim / 10-29 11:34:13.719129 | 657 | False | False | [nao verificado] | [nao verificado] |
| 51025 | col451 / 10-27 | orfao_14h | declaracao | sim / 10-28 18:57:32.171962 | sim / 10-29 11:34:17.021036 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51042 | col502 / 10-28 | saida_sem_entrada | declaracao | sim / 10-30 14:02:10.543140 | sim / 10-30 15:04:02.578131 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51108 | col502 / 10-28 | intervalo_saida | declaracao | sim / 10-30 13:56:58.855588 | sim / 10-30 15:04:09.520077 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51132 | col502 / 10-28 | intervalo_volta | declaracao | sim / 10-30 13:57:12.532781 | sim / 10-30 15:04:15.720394 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51185 | col502 / 10-28 | orfao_14h | declaracao | sim / 10-30 07:16:04.323504 | sim / 10-30 12:19:16.432883 | 657 | False | False | [nao verificado] | [nao verificado] |
| 49654 | col899 / 10-24 | orfao_14h | fato_ocupado | sim / 11-11 07:40:55.133099 | sim / 11-11 07:41:09.186539 | 657 | False | False | [nao verificado] | [nao verificado] |
| 49655 | col899 / 10-24 | saida_sem_entrada | declaracao | sim / 11-01 12:19:46.972682 | sim / 11-11 07:40:58.843348 | 657 | False | True | [nao verificado] | [nao verificado] |
| 51467 | col999 / 10-28 | orfao_14h | fato_ocupado | sim / 11-12 07:08:34.900551 | sim / 11-12 07:08:40.334664 | 657 | False | False | [nao verificado] | [nao verificado] |

4. Classes com o filtro solicitado: `pergunta_viva = False` e `fato_do_marco_no_espelho ∈ {False, None}`. Nesse recorte, os marcos são todos `False`; não há `None`. [nao verificado]

| Classe / via_resolucao | n perguntas | n dias-colab | Dias | pks | Evidência |
|---|---:|---:|---|---|---|
| fantasma | 5 | 5 | col129: 2026-10-25; col451: 2026-10-21, 2026-10-23, 2026-10-25, 2026-10-27 | 50169, 46703, 49422, 50179, 50883 | [nao verificado] |
| declaracao | 4 | 4 | col129: 2026-11-01, 2026-11-02, 2026-11-08; col502: 2026-10-28 | 52730, 53288, 55524, 51185 | [nao verificado] |
| fato_ocupado | 2 | 2 | col899: 2026-10-24; col999: 2026-10-28 | 49654, 51467 | [nao verificado] |
| Sem pergunta, com motivos_a_perguntar não vazio | 0 | 0 | nenhum | nenhum | [nao verificado] |

Esses agrupamentos registram a via atual e o estado observado. A causa C6 por degrau permanece `[nao verificado]`, conforme o próprio JSON.

5. Para col427 em 2026-09-21, o D4 reporta `status = folga`, `fonte = lavrado`; a janela atual informada é 2026-10-21 a 2026-11-20. [nao verificado]

| Operando da regra anexada | Valor / resultado | Evidência |
|---|---|---|
| `dia.get('pode_ultimo_recurso')` | True | [nao verificado] |
| `dia['data'] < hoje` | `2026-09-21 < 2026-11-12` → True | [nao verificado] |
| `dia.get('status') in STATUS_QUE_PEDEM_DECISAO` | `folga` no conjunto informado → False | [nao verificado] |
| Conjunção / retorno real | `True and True and False` → False | [nao verificado] |

**O operando que torna o resultado falso é a pertinência do status:** `folga` não pertence a `('falta', 'irregular', 'aberto', 'sem_certidao')`. O conjunto está em `app/colaboradores/services/calendario.py:94`; a conjunção, em `app/colaboradores/services/calendario.py:103`.

6. **As classes filtradas do item 4 cobrem 11 dos 15 dias**, com soma `5 + 4 + 2 = 11`, sem sobreposição entre esses grupos. Os quatro dias restantes são: [nao verificado]

| Dia-colab fora das classes filtradas | Por quê | Evidência |
|---|---|---|
| col258 / 2026-10-30 | Única pergunta, pk 52131, tem marco True | [nao verificado] |
| col258 / 2026-11-06 | Única pergunta, pk 54827, tem marco True | [nao verificado] |
| col258 / 2026-11-09 | Única pergunta, pk 56040, tem marco True | [nao verificado] |
| col427 / 2026-09-21 | Não tem pergunta; motivos_a_perguntar também está vazio, portanto não entra na classe própria solicitada | [nao verificado] |

Considerando **todas as seis classes originais do JSON**, inclusive as de marco aceso, a soma é `5 + 4 + 4 + 8 + 1 + 2 = 24` participações, cobrindo **14 dias-colab distintos** por haver sobreposições. Nesse conjunto original, o único dia fora de qualquer classe é **col427 / 2026-09-21**, sem perguntas. [nao verificado]

## 12/11 11:46 · sessao ca639e2b122d · - · - · origem=Ronald (tmux)

Âncoras no código, somente leitura:

| Campo/regra | Arquivo:linha |
|---|---|
| `pergunta_viva` | `app/chamados/juizes.py:862`; cálculo delegado em `app/chamados/juizes.py:1389` |
| `validada_em` preenchida encerra a pergunta nesse cálculo | `app/chamados/juizes.py:1391` |
| `fato_do_marco_no_espelho`: `True`, `False` ou `None`; `None` significa impossibilidade de afirmar | `app/chamados/juizes.py:930` |
| Mapa motivo → marco | `app/chamados/juizes.py:922`: `saida_sem_entrada`→`hi`; `orfao_14h`→`hf`; `intervalo_saida`→`hii`; `intervalo_volta`→`hfi` |
| Leitura das lâmpadas e resultado do marco | `app/chamados/juizes.py:965` |
| `motivos_a_perguntar` | `app/chamados/services/fabrica_da_celula.py:92`; retorno exclui os motivos já representados no domínio do fato em `app/chamados/services/fabrica_da_celula.py:154` |
| `porta_alcancavel_do_dia` | `app/chamados/juizes.py:1612`; retorno `rota`, `fila`, `verbos`, `fio` em `app/chamados/juizes.py:1657` |
| Significado de `fila`, `verbos` e `fio` | `app/chamados/juizes.py:1621` |
| `respondida_em` / `validada_em` | `app/chamados/models.py:2449` / `app/chamados/models.py:2450` |
| `validada_por` | `app/chamados/models.py:3378` |
| `via_resolucao` | `app/chamados/models.py:2562` |
| Auditoria: `usuario`, payload antes/depois, `criado_em` | `app/core/models.py:295`, `app/core/models.py:300`, `app/core/models.py:301`, `app/core/models.py:307` |
| `dono_fora_do_fio` | `app/chamados/services/vigia_fio.py:274`; ramos em `app/chamados/services/vigia_fio.py:306`; retorno sem dono em `app/chamados/services/vigia_fio.py:315` |

O mapeamento exato do campo raspado `dono_fora_do_fio_chamado` à função encontrada está [nao verificado].

## 12/11 11:46 · sessao e902d776f71d · FIO-E0-V4-E3-LEITORES · verificador · origem=Code

BLOQUEIA — a migração está parcial: permanecem respondedores próprios, há divergência entre sombra e vigia, e silêncio vencido continua com dois significados. (`app/chamados/services/raiox.py:139`, `app/chamados/management/commands/diff_e3_sombra.py:54`, `app/chamados/services/vigia_fio.py:650`, `app/chamados/services/verbos.py:51`)

FILA — identidade: HEAD `513ff80771b84636b5b485452a74058b2a507ebb` e comparação Git com `087fe758a`: [nao verificado]; a cópia disponibilizada não contém `.git`. Execução ORM, suite e mortes dos mutantes: [nao verificado]. As reproduções abaixo usaram somente funções extraídas por AST, em memória, com entradas sintéticas. [nao verificado]

- FILA — **item 1, sonda/soma:** existe registro histórico de **232**, reconciliado como 118+114; não é resultado certificado desta volta. Sonda atual e inclusão dos mesmos 232 sítios/dias: [nao verificado]. RED de cura não se aplica à medição. (`app/docs/RELATO.md:249`)

- FILA — **item 2, V/T/X:** há classificações históricas diferentes: V=3/T=228/X=1 e, posteriormente, V=15/T=0/X=217 distribuído em subclasses. Classificação válida para este HEAD: [nao verificado]; não escolher uma delas como resultado atual. Sem RED de cura aplicável. (`app/docs/RELATO.md:235`, `app/docs/RELATO.md:237`, `app/docs/RELATO.md:238`, `app/docs/RELATO.md:239`, `app/docs/RELATO.md:240`, `app/docs/RELATO.md:241`)

- FILA — **item 3, V curada ou zero:** a árvore contém exposição administrativa pela vida C6 e pela decisão E1; esses corpos não são alterados pelo diff fornecido. Cura dos V reais, quantidade residual e RED correspondente nesta volta: [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:243`, `app/chamados/services/tarefa_do_dia.py:314`)

- FILA — **item 4, chamadores C17:** **nada de defeito local demonstrado na substituição**: folha, vigia, beco e pauta consultam a porta da tarefa. Há casos que separam C17 de C19; retornar ao C17 contraria as expectativas desses testes. Mutantes executados por leitor: [nao verificado]. (`app/folha/fora_da_folha.py:72`, `app/chamados/services/vigia_fio.py:638`, `app/escala/services/cadastro_realidade.py:497`, `app/ponto/services/pautas_dos_discordantes.py:81`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:124`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:154`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:170`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:166`)

- BLOQUEIA — **item 5, cinco respondedores:** não morreram: crédito, relatório, cobrança e template continuam respondendo por critérios próprios; o fio ainda consome crédito/desenlace. Contraexemplo: timestamp preenchido com resposta vazia produz “aguarda supervisão”, enquanto C19 exige RESPONDER do colaborador. Certidão e RED/mutante por cada sítio restante: [nao verificado]. (`app/chamados/services/raiox.py:139`, `app/chamados/relatorios_disputa.py:21`, `app/chamados/services/cobranca.py:229`, `app/templates/chamados/partials/_card_atendimento.html:94`, `app/chamados/services/fio.py:405`, `app/chamados/services/tarefa_do_dia.py:137`)

- BLOQUEIA — **item 6, catraca/paridade:** a catraca verifica somente chamadas C17; não certifica zero respondedores nem zero derivações próprias. Contraexemplo: `creditos_perguntas` permanece decidindo por timestamp sem infringir essa contagem. A injeção sintética testa retorno ao C17, não esses respondedores. Paridade executada: [nao verificado]. (`app/chamados/tests/test_e3_pendentes_app.py:33`, `app/chamados/tests/test_e3_pendentes_app.py:145`, `app/chamados/tests/test_e3_pendentes_app.py:150`, `app/chamados/services/raiox.py:139`)

- BLOQUEIA — **item 7, DIFF da sombra:** a projeção do instrumento ficou diferente da produção. Para `verbos={âncora: 'validar'}`, `fio='colab'`, a sombra informa `exige_admin=True`; o vigia informa False. É exatamente a combinação coberta pelo caso de resposta vazia; o teste da sombra conserva o cálculo anterior. Queda real por classe: [nao verificado]. (`app/chamados/management/commands/diff_e3_sombra.py:54`, `app/chamados/services/vigia_fio.py:650`, `app/chamados/tests/test_o261e_f5_alcance.py:177`, `app/chamados/tests/test_e3_diff_sombra.py:143`)

- FILA — **item 8, suite inteira:** resultado desta árvore [nao verificado]; não há certificação de execução no material fornecido. Sem sítio de cura/RED próprio: é etapa de execução. [nao verificado] [nao verificado]

- FILA — **item 9, opus final:** aprovação desta árvore [nao verificado]. Sem sítio de cura/RED próprio. [nao verificado]

- FILA — **item 10, pouso:** pouso deste HEAD [nao verificado]. Sem sítio de cura/RED próprio. [nao verificado] [nao verificado]

- FILA — **item 11, cache por disputa:** **nada no caso aberto multicartão demonstrado pelo RED**: o mapa agora é por disputa e alimenta o cache correspondente. Reverter para perguntas do cartão contraria os asserts de resumo e universo; morte executada [nao verificado]. Limite restante: disputa **fechada** com uma viva e uma validada entrega somente a viva ao explicador, porque as mortas são excluídas antes do mapa; portanto “disputa inteira” não vale literalmente nesse ramo. (`app/chamados/services/fio.py:257`, `app/chamados/services/fio.py:330`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:95`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:105`, `app/chamados/services/fio.py:241`, `app/chamados/services/fio.py:255`)

- FILA — **item 12, aviso sem anuência:** **nada de defeito local demonstrado na cura**: web e API usam `aguarda_o_colaborador`, recortando perguntas sem resposta/texto e consultando C15/C6. Reverter ao timestamp cru contraria o caso morta por célula e o caso viva sem texto. Execução/mutante morto [nao verificado]. (`app/chamados/services/fio.py:163`, `app/chamados/services/fio.py:358`, `app/api/views.py:1175`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:125`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:143`, `app/chamados/tests/test_e3_leitores_fio_disputa.py:156`)

- FILA — **bloqueante #4, relógio:** **nada na cura direta de `alimentar_portas`**: C6 agora recebe `agora`; C19 já o passa em sua carga. Retirar o argumento contraria o RED e a verificação de repasse. `fio_vivo` continua sem parâmetro temporal, mas não é chamado diretamente pelo corpo C19 conferido. Execução/mutante morto [nao verificado]. (`app/chamados/juizes.py:1575`, `app/chamados/services/tarefa_do_dia.py:493`, `app/chamados/services/tarefa_do_dia.py:533`, `app/chamados/tests/test_e3_bloqueantes.py:110`, `app/chamados/tests/test_e3_bloqueantes.py:135`, `app/chamados/juizes.py:1494`)

- FILA — **bloqueante #8, furo sem canal:** **nada de derivação crua restante no trecho curado**: mede a porta publicada pela tarefa. Repor `VIVOS`/timestamp nulo contraria os casos chamado sem ação, âncora silenciada e registrada; existe controle positivo com porta. Execução/mutante morto [nao verificado]. (`app/chamados/services/furo_sem_canal.py:90`, `app/chamados/services/furo_sem_canal.py:100`, `app/chamados/tests/test_e3_bloqueantes.py:148`, `app/chamados/tests/test_e3_bloqueantes.py:155`, `app/chamados/tests/test_e3_bloqueantes.py:164`, `app/chamados/tests/test_e3_bloqueantes.py:171`)

- BLOQUEIA — **bloqueante #9, silêncio:** o filtro comum foi colocado no modelo, não em C19, e a lista real do admin continua divergindo. Exemplo: chamado aberto com `silenciado_ate=agora−1min` volta ao queryset e à lista do colaborador, mas vira `HISTORICO` e desaparece do painel padrão. O RED morde a reversão para `IS NULL`, porém só verifica os dois querysets; não morde essa divergência de apresentação. (`app/chamados/models.py:524`, `app/chamados/services/painel.py:190`, `app/chamados/services/janela_colab.py:78`, `app/chamados/services/verbos.py:51`, `app/chamados/services/painel.py:324`, `app/chamados/tests/test_e3_b4_silencio_vencido.py:35`)

- BLOQUEIA — **#9, porta aparentemente alcançável:** com pergunta exigível ancorada no chamado vencido da célula, a tarefa pode publicar `rota='porta_fila'`, `fila=()`, `verbos={âncora: 'historico'}`. `portas` conserva o chamado; somente `fila` o exclui. O consumidor recebe uma porta cujo chamado foi ocultado na lista administrativa. (`app/chamados/juizes.py:1533`, `app/chamados/services/tarefa_do_dia.py:548`, `app/chamados/services/tarefa_do_dia.py:550`, `app/chamados/services/tarefa_do_dia.py:551`, `app/chamados/services/tarefa_do_dia.py:555`, `app/chamados/services/painel.py:331`)

- BLOQUEIA — **segundo juiz de fechamento, fora do diff e alcançado pelo item 11:** o explicador calcula `pode_fechar` somente como `not fio_vivo`, omitindo a guarda C7 de resposta sem veredito. Exemplo: resposta com texto, sem validação/materialização, célula `ok` decidida pelo admin, dia não trancado: C6 pode dar morta, explicador diz “Tudo resolvido”, mas C7 recusa fechar. (`app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:140`, `app/chamados/juizes.py:1404`, `app/chamados/juizes.py:1452`, `app/chamados/models.py:1855`, `app/chamados/models.py:1866`, `app/chamados/models.py:1899`)

- BLOQUEIA — **mutante sem alvo:** `diff_fachada_vazia` procura o antigo corpo de `candidata`; agora existe um alias. O harness lança “Mutante sem alvo” antes de executar teste. Esse RC não comprova mordida do RED. (`app/chamados/tests/e3_mutantes.py:112`, `app/chamados/management/commands/diff_e3_sombra.py:43`, `app/chamados/tests/e3_mutantes.py:162`)

- BLOQUEIA — **mutante temporal equivalente:** `alimentacao_sem_agora` substitui somente a primeira ocorrência de `agora=self.agora)`: atinge C6 em `_ler_vida`, deixando `alimentar_fila` intacta. C6 recupera o mesmo instante de `carga.agora`; portanto o mutante não reverte a alimentação pretendida. (`app/chamados/tests/e3_mutantes.py:101`, `app/chamados/tests/e3_mutantes.py:164`, `app/chamados/services/tarefa_do_dia.py:493`, `app/chamados/services/tarefa_do_dia.py:533`, `app/chamados/juizes.py:883`)

- BLOQUEIA — **leitor esquecido: API/app/badges/PWA:** a consulta ainda manda qualquer pergunta com timestamp para histórico. Exemplo: resposta vazia com timestamp deixa de contar como pendência no app/dashboard, enquanto C19 exige RESPONDER. Os dois kernels consomem essa projeção antiga; a alteração da correção do cartão não migra esses consumidores. (`app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/pergunta_no_app.py:91`, `app/chamados/services/tarefa_do_dia.py:137`, `app/api/views.py:315`, `app/api/views_core.py:303`, `app/api/views.py:993`, `app/api/views.py:2533`, `app/core/views.py:252`)

- BLOQUEIA — **leitor esquecido: fio/admin/ficha:** crédito por timestamp alimenta verbo e desenlace; ficha traduz esse verbo em dono. No mesmo caso vazio, oferecem validação/admin enquanto a tarefa exige resposta do colaborador. (`app/chamados/services/raiox.py:139`, `app/chamados/services/verbos.py:34`, `app/chamados/catalogo/natureza.py:48`, `app/chamados/services/fio.py:421`, `app/chamados/services/desenlace.py:72`, `app/colaboradores/services/ficha.py:159`, `app/chamados/services/tarefa_do_dia.py:137`)

- BLOQUEIA — **leitor esquecido: cobrança:** `_calados` ainda usa pré-filtro `q_vivas`, timestamp nulo e disputa aberta. Pergunta com resposta vazia e timestamp deixa de ser cobrada por esse leitor, embora C19 ainda exija RESPONDER; pergunta morta por tranca também não é eliminada por uma consulta efetiva ao C6 nesse sítio. (`app/chamados/services/cobranca.py:229`, `app/chamados/services/tarefa_do_dia.py:137`, `app/chamados/juizes.py:1395`)

- BLOQUEIA — **leitor esquecido: calendário:** exclui resposta vazia antes de consultar C6 e deriva `pode_ultimo_recurso` separadamente. Exemplo: pergunta viva com resposta vazia em competência passada não bloqueia essa oferta, enquanto C19 ainda publica RESPONDER. (`app/colaboradores/services/calendario.py:474`, `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:738`, `app/chamados/services/tarefa_do_dia.py:137`)

- FILA — **leitor esquecido: copiloto/navbar:** copiloto continua expondo C6 e `pergunta_no_app.do_dia`, sem a tarefa; navbar continua contando chamados da fila. Exemplo: chamado na fila sem pergunta pode contar no navbar, enquanto C19 tem apenas ação do sistema e nenhuma porta humana. A navbar usa unidade diferente de perguntas; não equiparar numericamente os badges. (`app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:108`, `app/core/context_processors.py:185`, `app/chamados/tests/test_e3_leitores_c17_chamadores.py:101`)

- BLOQUEIA — **campo `respondida_em`: significados incompatíveis entre leitores.** É instante de resposta humana, automática ou recuperada da trilha; também pode ser preenchido por `carimbar` e limpo na reabertura. C6 recorta pelo instante; C19 lê a decisão; crédito/app/relatório ainda interpretam presença como resposta suficiente. Contraexemplo comum: timestamp com texto vazio. (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3145`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/juizes.py:1380`, `app/chamados/services/tarefa_do_dia.py:135`, `app/chamados/services/raiox.py:139`, `app/chamados/services/pergunta_no_app.py:71`, `app/chamados/relatorios_disputa.py:21`)

- BLOQUEIA — **campo `silenciado_ate`: escritores e fronteira estão identificados, leitores discordam.** `arquivo.silenciar` grava; `arquivo.levantar` limpa. O modelo define silêncio vigente por `ate > agora`: igualdade já venceu. Partição/painel conservam significado histórico permanente para prazo preenchido vencido, produzindo o contraexemplo #9. (`app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202`, `app/chamados/models.py:512`, `app/chamados/models.py:524`, `app/chamados/services/verbos.py:51`, `app/chamados/services/painel.py:331`)

- FILA — **campo `data_evento`: dois significados discriminados.** Fato usa dia do turno; conversa usa dia civil. Nascimento, costura e rechaveamento passam pela atribuição em `gravar_dia_do_fato`; C19 exclui conversas e filtra datas inclusivamente. Mistura material adicional demonstrada: **nada**; legado vivo com data NULL [nao verificado], portanto a condição de triagem #2 continua sem medida. (`app/chamados/models.py:2796`, `app/chamados/models.py:2808`, `app/chamados/models.py:3029`, `app/chamados/services/fio.py:819`, `app/chamados/services/fio.py:898`, `app/chamados/services/fio.py:1056`, `app/chamados/services/tarefa_do_dia.py:375`)

- FILA — **campo `status_local`: nada de segundo escritor persistente demonstrado nos caminhos conferidos.** Gravação de transição em `_gravar_estado`; nascimento pela abertura. As outras atribuições indicadas simulam/restauram memória. Leitores usam universos distintos: VIVOS para crédito, COBRANCA para fila, fila para portas; chamado vivo não equivale a tarefa exigível. (`app/chamados/models.py:228`, `app/chamados/models.py:1108`, `app/chamados/models.py:356`, `app/chamados/models.py:371`, `app/chamados/services/raiox.py:139`, `app/chamados/services/painel.py:189`, `app/chamados/juizes.py:1533`)

- BLOQUEIA — **valores `pode_fechar` e `porta`:** não são campos persistentes nesses trechos. C7 calcula fechamento; o explicador escreve uma resposta paralela incompleta. C19 escreve rota/fila/verbos/fio em memória; `porta_da_tarefa` somente projeta, e folha/vigia/beco/pauta leem essa saída. Os contraexemplos são a guarda C7 omitida e a porta com fila vazia descritas acima. (`app/chamados/models.py:1866`, `app/chamados/services/explicador.py:163`, `app/chamados/services/tarefa_do_dia.py:555`, `app/chamados/services/tarefa_do_dia.py:565`, `app/folha/fora_da_folha.py:75`, `app/chamados/services/vigia_fio.py:638`, `app/escala/services/cadastro_realidade.py:497`, `app/ponto/services/pautas_dos_discordantes.py:81`)

- FILA — **relógios: não existe uniformidade completa.** Nas bordas, vigia e lavrador do retrato capturam um instante; C19/C6 usam dia civil local e incluem fatos com instante `<= agora`. Porém `censo_alcance(agora=..., hoje=None)` lê o dia da máquina; `chamados_visiveis_colab(agora=...)` também calcula sua janela sem derivá-la desse instante. Exemplo: consultar outro mês com apenas `agora` mistura janela da máquina com silêncio/tarefa do instante recebido. (`app/chamados/management/commands/vigiar_fio_morto.py:166`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/chamados/services/tarefa_do_dia.py:393`, `app/chamados/veredito_lavrado.py:104`, `app/chamados/services/vigia_fio.py:567`, `app/chamados/services/janela_colab.py:42`, `app/chamados/services/janela_colab.py:58`)

- FILA — **ordem de trava: nada novo demonstrado nos caminhos conferidos.** Silêncio chama a porta de ordem; rechaveamento trava antes da escrita; o núcleo implementa C→D→CH→P. Ausência global de inversão por execução/reflexão [nao verificado]. (`app/chamados/services/arquivo.py:170`, `app/chamados/services/arquivo.py:209`, `app/chamados/services/fio.py:1025`, `app/chamados/models.py:2679`, `app/chamados/models.py:2688`, `app/chamados/models.py:2701`, `app/chamados/models.py:2705`)

- FILA — **limite escritor/cascata:** permanece a condição de `post_save` que trata qualquer silêncio preenchido como motivo para não reprocessar isento, inclusive vencido. Exemplo: entrada em estado vivo com prazo vencido retorna antes do processamento. É lado escritor a registrar no censo, não prova de cura pelo novo filtro. (`app/chamados/signals.py:285`, `app/chamados/signals.py:299`)

- FILA — **classe A/B:** no diff fornecido, **classe B**: nenhum hunk em static/JS, service worker ou template base. Conferência independente pelo Git [nao verificado].

- FILA — **fora do escopo:** **nada demonstrado** como alteração de produto alheia aos avais; propagação de `agora` alcança os chamadores dos serviços migrados. Portão de arquivos contra o diff separado da E4B c10, vaga/teto e marcos desta volta no RELATO: [nao verificado]. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78`, `app/folha/retrato_folha.py:1158`, `app/ponto/management/commands/pautas_discordantes.py:68`)

## 12/11 11:44 · sessao f335ea201ffa · PORTA-QUE-NAO-AGE · censo · origem=Code

perguntas de lei: **8 — D1–D8 abaixo, sem escolher entre as leituras existentes.**

O código permite que uma pergunta deixe de estar viva **antes da conferência do marco cobrado**: por validação, tranca, identidade ou absolvição do dia. A porta `carimbar` consulta o C6 depois de aplicar os carimbos em memória; com `resposta='validar'`, a própria validação recém-aplicada pode determinar a morte (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1395`, `app/chamados/juizes.py:1428`, `app/chamados/models.py:3131`, `app/chamados/models.py:3140`).

O dado raspado informa os 15 dias, mas não traz os campos das perguntas, autores, instantes, atas ou status D4. Portanto, **a classe de morte de cada um dos 15 e o veto concreto do col427 em 21/09 permanecem [nao verificado]**. Os testes citados abaixo foram lidos; execução e resultado atual: [nao verificado]. [nao verificado]

**Convenções da matriz**

| Sigla | Campos |
|---|---|
| C | `materializada_em`, `materializada_por`, `via_resolucao` (`app/chamados/models.py:3129`). |
| R | `resposta_colab`, `respondida_em` (`app/chamados/services/disputa_emissao.py:1467`). |
| V | `validada_em`, `validada_por` (`app/chamados/services/validacao.py:134`). |
| L | `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/models.py:3150`). |

**1. Escritores encontrados por AST e grep**

A varredura usou em memória o detector AST existente, ampliando os campos pesquisados. O detector acompanha atribuições, `setattr`, construtores, dicionários propagados, `update` e `bulk_update`; declara limitações para `__dict__`, nomes vindos de outro módulo e efeitos de `SET_NULL` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:99`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`). Universo informado pela execução: 767 arquivos Python de produção em `app`, sem testes/migrations/assets/logs, zero falhas de parse; artefato persistido dessa execução: [nao verificado].

| Escritor físico | Campos e persistência |
|---|---|
| `_escrever_carimbo` | Atribui C; quem persiste são `carimbar`, `carimbar_identidade` e `descarimbar`, por `save(update_fields=campos)` (`app/chamados/models.py:3098`, `app/chamados/models.py:3153`, `app/chamados/models.py:3191`, `app/chamados/models.py:3213`). |
| `carimbar` | Além de C, completa `respondida_em` e V nulos quando há texto e o parâmetro pede responder/validar; pode persistir L no mesmo `save` (`app/chamados/models.py:3131`, `app/chamados/models.py:3150`). |
| `descarimbar_em_lote` | `PerguntaDisputa.objects…exclude(…FORA_DO_FATO).update(...)`: limpa C e os campos recebidos em `**outros` (`app/chamados/models.py:3253`, `app/chamados/models.py:3265`). |
| `encerrar_por_fato_no_espelho` | Atribui e salva apenas `via_resolucao='fato_materializado'` (`app/chamados/models.py:3276`). |
| `relavra_via.relavrar` | Atribui a nova via, relavra e salva `via_resolucao` + L (`app/chamados/services/relavra_via.py:125`). |
| `_carimbar_validacao` | Atribui V, salva os dois campos e registra `DecisaoPergunta.VALIDACAO` (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:139`). |
| `devolver_validacao_recusada` | Limpa V, salva e registra `DEVOLUCAO` (`app/chamados/services/validacao.py:199`, `app/chamados/services/validacao.py:205`). |
| `responder_pergunta_disputa_core` | Atribui R; limpa V anterior; incorpora eventual descarimbo por resposta; salva em transação (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1485`, `app/chamados/services/disputa_emissao.py:1508`). |
| `responder_automaticamente` | Atribui/salva R e registra `RESPOSTA` (`app/chamados/services/disputa_emissao.py:1652`). |
| `carimbar_resposta_pela_trilha` | Atribui/salva `respondida_em` e registra `RESPOSTA` com o instante recebido (`app/chamados/services/disputa_emissao.py:1674`). |
| `reabrir_para_correcao` | Monta limpeza de R/V; usa `qs.update(**campos)` ou `descarimbar_em_lote`, conforme `desfazer_absorcao` (`app/chamados/services/disputa_emissao.py:1593`, `app/chamados/services/disputa_emissao.py:1629`). |
| `ressuscitar_perguntas` | Passa `respondida_em=None`, `validada_em=None`, `validada_por=None` para `descarimbar_em_lote` (`app/chamados/models.py:1567`). |
| Nascimento `_nascer` | Recebe `**campos`, constrói a pergunta e faz `save(force_insert=True)`; assinaturas R/V recebidas são conservadas e viram atos da sequência (`app/chamados/models.py:3015`, `app/chamados/models.py:3018`). |
| Efeito declarativo da FK | `validada_por` admite `SET_NULL` ao apagar o usuário; não é atribuição explícita de um serviço de pergunta (`app/chamados/models.py:3378`). |

As atribuições em `_universo_da_ressuscitacao` e `_desfazer_em_memoria` servem à simulação/restauração em memória, sem persistência própria (`app/chamados/models.py:1507`, `app/chamados/models.py:3160`).

Não apareceu outro `bulk_update` dos quatro campos no universo varrido; garantia fora dos limites declarados do detector: [nao verificado]. `flip_auto` é leitura por `filter(via_resolucao=...)`, não escritor (`app/ponto/services/flip_auto.py:128`).

Os apelidos/reexports relevantes não constituem escritores adicionais:

- `_reabrir`: `app/chamados/models.py:2220` e `app/chamados/services/acoes_disputa.py:102`; `_reabrir2`: `app/chamados/views.py:1435`.
- Reexports dos corpos de materialização e emissão por `chamados.models`: `app/chamados/models.py:4088`, `app/chamados/models.py:4093`.
- `PerguntaDisputa` como `_PD`, `_PD_ordem` e `_PD_inf`: por exemplo, `app/chamados/services/disputa_emissao.py:1501`, `app/chamados/services/validacao.py:428`, `app/chamados/services/painel.py:260`.
- Juízes importados com apelido no materializador: `declarou_hora as _declarou`, `via_pelo_fato as _via_fato`, `fato_do_marco_no_espelho as _lampada_do_marco` (`app/chamados/services/materializacao.py:1267`, `app/chamados/services/materializacao.py:1270`, `app/chamados/services/materializacao.py:906`).

**2. Matriz das operações que encerram ou carimbam encerramento**

`carimbar` não equivale a “exigiu marco aceso”: aplica C e, opcionalmente, resposta/validação; consulta C6; se viva, restaura e retorna `False`; se morta, salva e retorna `True` (`app/chamados/models.py:3130`, `app/chamados/models.py:3140`, `app/chamados/models.py:3153`). `carimbar_identidade` grava sem consultar C6; o julgamento fica no chamador (`app/chamados/models.py:3169`).

| Via / entrada | Juiz → operação | Persistido → retorno | Prova que exercita |
|---|---|---|---|
| **Validação**, resposta recebida, ainda não validada | Guardas de resposta/implausibilidade → `_carimbar_validacao`; depois materialização (`app/chamados/services/validacao.py:393`, `app/chamados/services/validacao.py:404`, `app/chamados/services/validacao.py:435`). | V + ato `VALIDACAO`; recusa de hora declarada pode desfazer V. Retorna `validada`, `ja_no_espelho` ou `nao_gravou` (`app/chamados/services/validacao.py:461`, `app/chamados/services/validacao.py:499`, `app/chamados/services/validacao.py:515`). | C6 mata validada sem exigir prova do marco: `app/chamados/tests/test_pergunta_viva.py:17`. Fecho sem batida no slot: `app/chamados/tests/test_fechamento_unificado.py:74`. |
| **`admin_decidiu`**, dia decidido | `q_vivas` do dia → `fechar_por_admin` → identidade (`app/ponto/services/veredito_celula.py:180`, `app/ponto/services/veredito_celula.py:190`). | C + ato `SILENCIO`; helper retorna booleano (`app/ponto/services/veredito_celula.py:195`, `app/chamados/models.py:3065`). | `app/chamados/tests/test_e1_v5_portas_backfill.py:83`. |
| **Silêncio / desligamento**, disputa com perguntas vivas | `respostas_sem_veredito` barra resposta sem julgamento; portas `admin_encerra/desligamento` percorrem vivas e chamam `fechar_por_admin` (`app/chamados/models.py:1887`, `app/chamados/models.py:1919`). | C nas perguntas; ato `SILENCIO`; fecho/autor/resolução/motivo na disputa. `True`; repetição idêntica, `False` (`app/chamados/models.py:1925`, `app/chamados/models.py:1933`, `app/chamados/models.py:1880`). | `app/chamados/tests/test_e4_expand_orm.py:63`. |
| **Folga aceita → `admin_decidiu`** | `o_dia_ja_e_folga` ou `checar_declaracao` → validação e decisão de folga (`app/chamados/services/validacao.py:280`, `app/chamados/services/validacao.py:305`). | V + C; retorna `folga_ja_declarada` ou `folga_declarada` (`app/chamados/services/validacao.py:288`, `app/chamados/services/validacao.py:302`, `app/chamados/services/validacao.py:320`, `app/chamados/services/validacao.py:367`). | Teste específico localizado, corpo não aberto: [nao verificado]. |
| **`motivo_retratado`**, motivo removido | Guarda de motivo/disputa → `remover_motivo` → identidade (`app/chamados/models.py:2034`, `app/chamados/models.py:2062`). | Ato `RETRATACAO` + C, preservando carimbo anterior quando existente; retorna `removido` (`app/chamados/models.py:2057`, `app/chamados/models.py:2068`). | Preserva resposta e tira de viva: `app/chamados/tests/test_e1_entradas.py:142`. |
| **`duplicata`**, linha perdedora do fato | `plano_de_retratacao` → `retratar_duplicata` → identidade (`app/chamados/services/fio.py:1341`, `app/chamados/services/fio.py:1346`). | C, conservando instante/autor anterior; `recusa_motivo` aponta canônica. `True`; já duplicata, `False` (`app/chamados/services/fio.py:1351`, `app/chamados/services/fio.py:1360`). | Apply/idempotência: `app/chamados/tests/test_e1_v5_portas_backfill.py:207`; veto de mérito: `app/chamados/tests/test_e1_v9.py:210`. |
| **`fato_ocupado`**, colisão de tipo oposto | `traduzir_recusa`; código `COLISAO` e sem materialização → identidade, sem teste local da lâmpada (`app/chamados/services/materializacao.py:1097`). | C + motivo, eventualmente nota no destino; retorno implícito `None` (`app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1113`). | `app/chamados/tests/test_a27_recusa_com_dono.py:47`. |
| **`paridade_recusada`**, vizinha do mesmo tipo | **Exige lâmpada `is True`**; depois identidade (`app/chamados/services/materializacao.py:906`, `app/chamados/services/materializacao.py:930`). | C; retorna `None`. Apagada/indeterminada registra recusa, sem C (`app/chamados/services/materializacao.py:919`, `app/chamados/services/materializacao.py:931`). | Três valores da lâmpada: `app/chamados/tests/test_o261_paridade_respeita_a_lampada.py:174`, `:183`, `:192`. |
| **`celula_absolveu`**, pergunta absolvida pelo dia | `perguntas_absolvidas_pela_celula` → `carimbar`/C6 (`app/chamados/reconciliador.py:585`, `app/chamados/reconciliador.py:596`). | C; incrementa quantidade e pode fechar disputa pelo C7; retorna quantidade (`app/chamados/reconciliador.py:599`, `app/chamados/reconciliador.py:612`, `app/chamados/reconciliador.py:618`). | Consumidor: `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:147`; teto bloqueia carimbo: `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:62`. |
| **`sem_turno_aberto`**, motivo admitido, sem texto/materialização | Núcleo sem turno aberto; C6 antes e dentro de `carimbar` (`app/chamados/reconciliador.py:363`, `app/chamados/reconciliador.py:398`, `app/chamados/reconciliador.py:406`). | C; eventual fecho C7; retorna `True` quando lavrou (`app/chamados/reconciliador.py:412`, `app/chamados/reconciliador.py:415`). | **Pergunta previamente validada, marco apagado, via persistida e disputa fechada:** `app/chamados/tests/test_o261_lavra_le_a_lampada.py:148`. |
| **`fantasma`**, órfã atual | `veredito_orfa` exige marco aceso, C6 viva e teto encerrado → consumidor → `encerrar_fantasma`/`carimbar` (`app/chamados/reconciliador.py:1083`, `app/chamados/reconciliador.py:1108`, `app/chamados/models.py:3288`). | C; booleano do helper; consumidor devolve contagens (`app/chamados/models.py:3285`, `app/chamados/reconciliador.py:1113`). | Recusa viva respondida e aceita aprovada: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665`, `:676`. |
| **`juiz_aprovou`**, pendente após fecho da disputa | Lastro aprova; C6 também precisa aceitar o carimbo (`app/chamados/services/materializacao.py:1433`). | C; devolve lista dos PKs carimbados (`app/chamados/services/materializacao.py:1437`). | Resposta por validar não sai: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:687`. |
| **`declaracao`**, aplicação da resposta | Materializa/reutiliza efeito; para hora/data_hora exige lastro; escolhe via por `_declarou`; consulta C6 com V aplicada (`app/chamados/services/materializacao.py:1238`, `app/chamados/services/materializacao.py:1267`, `app/chamados/services/materializacao.py:1283`). | C + complementos R/V + L; C6 recusando provoca rollback; unidade marca sucesso (`app/chamados/services/materializacao.py:1283`, `app/chamados/services/materializacao.py:1288`). | Plantio real: `app/chamados/tests/test_fechamento_unificado.py:90`. Absorção também pode dar essa via sem plantio próprio: `app/chamados/tests/test_hx_via_ausencia_dia.py:104`. |
| **`ausencia_dia`**, escape/irmãs absorvidas | `declarou_hora` distingue declaração de escape; `carimbar(...resposta='validar')` consulta C6 (`app/chamados/services/materializacao.py:548`, `app/chamados/services/materializacao.py:555`, `app/chamados/services/materializacao.py:567`). | C, complementos R/V conforme texto; ato `ABSORCAO` se gravou (`app/chamados/services/materializacao.py:569`). Retorno integral do serviço: [nao verificado]. | Irmã muda continua sem resposta e recebe a via: `app/chamados/tests/test_hx_via_ausencia_dia.py:78`; escape: `:90`. |
| **`completude_turno`** | `via_pelo_fato`: TM com saída e motivo fora de intervalo → S128 ou materializador (`app/chamados/juizes.py:1754`, `app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/materializacao.py:1270`). | Pelo S128: C, complementos R/V e L; retorna contagens de perguntas/disputas (`app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:2094`). | S128 com/sem batida no slot: `app/chamados/tests/test_fechamento_unificado.py:60`, `:74`. |
| **`adesao`** | Agrupado, sem batida apurável tocando o dia civil e pai não LIMBO-FOLGAS → `via_pelo_fato` → carimbo (`app/chamados/juizes.py:1713`, `app/chamados/juizes.py:1733`, `app/chamados/juizes.py:1740`). | Mesmos campos/retorno do S128 acima (`app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:2094`). | Teste exercitando especificamente essa seleção: [nao verificado]. |
| **`cadastro_folga`** | Mesmo ramo sem batida; pai `sem_previsao_escala/folga_nao_definida` → carimbo (`app/chamados/juizes.py:1738`). | Mesmos campos/retorno do S128 (`app/chamados/services/disputa_emissao.py:1986`, `app/chamados/services/disputa_emissao.py:2094`). | Teste exercitando especificamente essa seleção: [nao verificado]. |
| **`chamado_encerrado`** | Resto de `via_pelo_fato`; também ramo sem credor, após C6 já dar morta (`app/chamados/juizes.py:1757`, `app/chamados/services/disputa_emissao.py:2048`, `app/chamados/services/disputa_emissao.py:2058`). | C; no ramo sem credor completa apenas resposta, não valida; trilha `fecho_sem_credor`; retorno global em contagens (`app/chamados/services/disputa_emissao.py:2060`, `app/chamados/services/disputa_emissao.py:2063`, `app/chamados/services/disputa_emissao.py:2094`). | Sem texto fecha; com resposta viva recusa: `app/chamados/tests/test_o261_resposta_vazia.py:78`, `:103`. |
| **`fato_materializado`** | Comando exige lâmpada `is True` → `encerrar_por_fato_no_espelho` (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:41`). | Só via; `True`, ou `False` se já nessa via/fora do fato (`app/chamados/models.py:3274`). | Teste específico do comando: [nao verificado]. |
| **`competencia_trancada`** | Escritor antigo retirado; hoje C6 lê a tranca. A porta de identidade não aceita novo carimbo dessa via (`app/chamados/models.py:3067`, `app/chamados/models.py:3119`). | Nenhum campo novo; acervo antigo é descarimbado ao reabrir (`app/chamados/models.py:3318`). | Leitura sem escrita/destrancar: `app/chamados/tests/test_c7_red_minimos.py:361`, `:369`. |
| **`legado_sem_lastro` / via vazia** | Vocabulário/acervo lido; escritor atual atribuindo esses valores como encerramento não localizado: [nao verificado]. | `legado_sem_lastro` e `chamado_encerrado` pertencem a `VIAS_SEM_LASTRO`; resposta posterior pode descarimbar (`app/chamados/models.py:3766`, `app/chamados/models.py:3314`). | Prova específica de morte por essa via isolada: [nao verificado]. |
| **Re-lavra de via derivada** | `declarou_hora` ou `via_pelo_fato`; apenas vias anteriores `declaracao/completude_turno` (`app/chamados/services/relavra_via.py:77`, `app/chamados/services/relavra_via.py:95`, `app/chamados/services/relavra_via.py:118`). | Via + L; retorna `(n_via,n_ver)` (`app/chamados/services/relavra_via.py:125`, `app/chamados/services/relavra_via.py:139`). | Teste funcional específico: [nao verificado]. |

`respondida_em` sozinho **não é uma via de morte**: resposta com texto, sem validação, normalmente mantém a pergunta viva; as exceções anteriores na ordem do C6 continuam prevalecendo (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1402`, `app/chamados/juizes.py:1407`). As portas automática e de recuperação pela trilha gravam resposta; a validação posterior pertence a outro escritor (`app/chamados/services/disputa_emissao.py:1638`, `app/chamados/services/disputa_emissao.py:1661`).

**3. Saídas de viva que acontecem só por leitura**

Estas saídas não precisam de uma nova escrita nos quatro campos pesquisados (`app/chamados/juizes.py:1389`).

| Estado de entrada → juiz | Operação / persistência / retorno | Prova |
|---|---|---|
| V presente → primeiro degrau C6 | Não consulta marco; nenhuma escrita; `False` (`app/chamados/juizes.py:1391`). | `app/chamados/tests/test_pergunta_viva.py:17`. |
| Dia trancado, sem toque humano posterior → `dia_da_pergunta_trancado` | Nenhuma escrita; C6 `False` (`app/chamados/juizes.py:974`, `app/chamados/juizes.py:1395`). | `app/chamados/tests/test_c7_red_minimos.py:361`, `:391`. |
| `duplicata/motivo_retratado/tranca` com materialização | Vence até resposta existente; nenhuma escrita; `False` (`app/chamados/models.py:2525`, `app/chamados/juizes.py:1402`). | Duplicata respondida sai da fila: `app/chamados/tests/test_leitores_ouvem_a_lei.py:28`. |
| Demais identidades com materialização, alcançado o degrau | Nenhuma consulta necessária ao marco; `False` (`app/chamados/juizes.py:1427`). | `app/chamados/tests/test_pergunta_viva.py:31`. |
| Célula não acusadora, sem marcos pendentes | C6 absolve pelo dia; não exige positivamente a luz do marco nesse primeiro termo; `False` (`app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1452`). | `app/chamados/tests/test_pergunta_viva.py:39`. |
| Materializada, via sem testemunha | Lastro aceita identidade, ausência de dia, fora da janela viva, ou ausência de falta; exceção da classificação também termina em `True`; C6 pode devolver `False` (`app/chamados/juizes.py:679`, `app/chamados/juizes.py:691`, `app/chamados/juizes.py:702`, `app/chamados/juizes.py:1473`). | Cobertura exata de todas essas condições: [nao verificado]. |
| `escolha` materializada | Lastro usa apenas `materializada_em`; não exige batida/lâmpada (`app/chamados/juizes.py:706`). | Teste específico desse ramo: [nao verificado]. |
| Hora/data_hora, slot não derivável | Lastro pode aceitar destino encerrado ou retratação com trilha; na janela viva tenta vetar pela falta (`app/chamados/juizes.py:765`, `app/chamados/juizes.py:786`, `app/chamados/juizes.py:797`). | Teste específico de cada fallback: [nao verificado]. |
| Hora/data_hora ou texto, últimos degraus | Lastro aceita batida viva no slot, retratação com trilha ou saída do turno; esses termos não exigem a ata do marco acesa (`app/chamados/juizes.py:752`, `app/chamados/juizes.py:812`, `app/chamados/juizes.py:817`). | Batida no slot: `app/chamados/tests/test_fechamento_unificado.py:48`; ocorrência com ata apagada: [nao verificado]. |
| Exceção ao co

[truncado em 20000 de 30259 caracteres]

_4229 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
