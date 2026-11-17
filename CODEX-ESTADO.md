# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 16:12** · publicado em **17/11/2026 16:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 16:12 · sessao a324e3d67aa4 · PORTA-EXPORT · censo · origem=Code

Os anexos estão no texto da solicitação; **não localizei arquivos correspondentes na cópia**. As referências abaixo contam as linhas de cada bloco fornecido, a partir da primeira linha após `== anexo: ... ==`; a correspondência com linhas de arquivos originais é [não verificado].

1. **Estados na tela e GET — (e) não perguntado.** A pergunta tratou dos textos de FALHOU e DIVERGENTE: “**textos: FALHOU =** … **DIVERGENTE acrescenta** ‘Avise o suporte Hasner.’ e conserva a lista” (v2.q:7–8). A resposta disse “**Textos e lista | Fechado no código**” (v2.r, seção 7; linha [não verificado]). A exigência de mostrar os três estados no GET e o smoke de PRONTA não aparecem nessa pergunta (v2.q:5–9; opus_final_H.txt:3).

2. **Instrumento na mesma raia do produto — (e) não perguntado.** A pergunta sobre classes históricas incluía “**pouso antes do pre-requisito**” (v2.q:22–24), mas não a separação de raias para o pouso dos instrumentos (opus_final_H.txt:4; v2.q:22–24). A resposta examinou “**Ordem de trava no caminho normal**” e declarou inversão “**[nao verificado]**” (v2.r, seção 6; linha [não verificado]); não encontrei resposta sobre a raia apontada pelo opus [não verificado]. [nao verificado]

3. **Tentativa não criada diante de outra aberta — (c) fora do alcance da revisão de H.** A pergunta de v4 abrangia “**Ha caminho em que assumir levanta e nenhum registro mostra a falha?**” (v4.q:12–13). O verificador respondeu que “**cron/manual não passa tentativa, e `falhar_ao_assumir` retorna `False` imediatamente**” (v4.r, seção 4; linha [não verificado]). A triagem registra que a solução que introduziu `_cunhar` foi feita em **H**, “**sem 5a volta**” (v4.triagem:1,5); o comportamento específico apontado pelo opus pertence a essa solução posterior (opus_final_H.txt:5; v4.triagem:5).

4. **Leitura que passou a gravar — (a) tema presente, não visto.** A primeira pergunta pediu, para cada campo lido ou gravado pelo diff, “**QUEM ESCREVE e QUEM LE** … **no repositorio inteiro**” (v1.q:11–13). A resposta identificou `estado_da_porta` como leitor do carimbo: “**Classificação da porta**” (v1.r:10), mas não apontou que essa leitura passaria a gravar por meio do vigia [não verificado]. O chamador rotulado “SO LEITURA” é o elemento específico do achado do opus (opus_final_H.txt:6). [nao verificado]

5. **Chave da trava sem dia e virada da meia-noite — (a) tema presente, não visto.** A pergunta incluiu colisões da chave (v1.q:17–19) e depois perguntou expressamente por “**data_ref do pedido x do carimbo na virada da meia-noite**” (v2.q:12–13). A resposta examinou colisões da chave por ano, schema e faixa de ID (v1.r, seção 4; linha [não verificado]) e, sobre meia-noite, disse que “**a leitura ainda pode atravessar a meia-noite entre as duas consultas**” (v2.r, seção 6; linha [não verificado]). Não apontou a ausência do dia em `k1` com a consequência descrita pelo opus [não verificado].

6. **DIVERGENTE não remede no clique — (e) não perguntado.** A pergunta de v2 especificou o texto e a lista de DIVERGENTE (v2.q:7–8); não perguntou se clicar nesse estado disparava nova medição [não verificado]. A resposta disse “**Textos e lista | Fechado no código**” (v2.r, seção 7; linha [não verificado]). O comportamento de remedir no clique é o objeto do achado do opus (opus_final_H.txt:6).

7. **Vivacidade por `pg_locks` como segundo predicado — (a) tema presente, não visto.** A pergunta juntou o teste de vida por `pg_locks` (v2.q:16–18) à classe “**2o juiz que duplica juiz registrado**” (v2.q:22–24). A resposta descreveu “**`vigiar → medicao_viva`, comparando-o com `pg_locks.pid`**” (v2.r:14) e afirmou “**Clone de juiz registrado: não demonstrado**” (v2.r, seção 6; linha [não verificado]). Não identificou a relação com `job_morreu` apontada pelo opus (opus_final_H.txt:6; v2.r, seção 6; linha [não verificado]). [nao verificado]

8. **`lavrar_verde_para_teste` sem chamador de produção — (a) tema presente, não visto como o achado formulado.** A pergunta pediu “**Algum escritor de carimbo … fora de concluir (inclusive lavrar, testes de outros apps, comandos)?**” (v3.q:15–17). A resposta viu o helper: “**o helper corrigido usa `assumir → concluir`, inclusive para o teste de relatórios**”, e disse não ter encontrado outro chamador produtivo de escrita do carimbo (v3.r:16). Ela não afirmou que o próprio helper tinha zero chamadores de produção [não verificado]; essa é a formulação do opus (opus_final_H.txt:6). [nao verificado]

9. **Descarte da leitura concorrente — (b) visto e descartado pela triagem.** A pergunta pediu caminho de PRONTA sem o par exigido e o inverso (v3.q:18–19). A resposta viu a corrida: “**um cron pode substituir o pedido concluído por outra tentativa EM_CURSO**” entre consultas, mas a descreveu como “**uma leitura válida do par anterior**” (v3.r:24). A triagem descartou pelo motivo: “**devolve o par anterior VALIDO, nunca um par que nao existiu**” (v3.triagem:7). A avaliação posterior do opus consta em opus_final_H.txt:6.

10. **Cron em produção — (c) dado de produção fora do alcance.** A pergunta já delimitava: “**cron diario (NAO esta neste diff)**” (v1.q:8). A resposta declarou “**Banco, logs, execução integrada e cron implantado: [nao verificado]**” (v1.r:3). Os fatos de duração, ambiente, `nice` e rc diário aparecem no dado do opus, sem banco ou logs adicionais fornecidos (opus_final_H.txt:6; [não verificado]).

## 17/11 15:52 · sessao fa3c0b2a4cb7 · E4C-F2 · verificador · origem=Code

**Veredito: BLOQUEIA.** A janela ainda permite que código P1 ressuscite uma pergunta sem gravar o ato compensador que os leitores P2 precisam. Há também divergência, neste hash, entre a validação pela sequência e consumidores que seguem julgando pela projeção. A ocorrência desses casos em produção é **[não verificado]**. ([models.py:1632](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:1632), [models.py:4299](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4299), [juizes.py:1382](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/juizes.py:1382))

A cópia está sem metadados Git: não pude confirmar o hash nem consultar diretamente P1/main. As comparações com essas versões abaixo usam o diff fornecido; onde ele não mostra o comportamento, marco **[não verificado]**. Não consultei banco nem logs de produção. [não verificado]

1. **BLOQUEIA — P1 ainda pode escrever depois da 0056.** O diff fornecido mostra que o P1 ressuscitava zerando a projeção sem a REABERTURA compensadora; o P2 acrescenta essa linha após fotografar a projeção. Se a ressuscitação P1 ocorrer depois da 0056, a migração não cria a linha faltante e a leitura P2 pode continuar vendo uma RESPOSTA ou VALIDACAO antiga como vigente, embora a projeção tenha sido zerada. Os quatro escritores de RESPOSTA declararem corretamente seu booleano não cobre essa ausência de ato. Portanto, **matar apenas processos anteriores ao P1 não basta**: também é necessário impedir ou drenar a escrita P1 que atravesse a troca, ou demonstrar outra garantia equivalente. ([models.py:1650](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:1650), [models.py:1692](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:1692), [models.py:4305](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4305))

   Há outra corrida de versão: o diff mostra o P2 movendo o relógio da resposta humana para depois da trava. Uma resposta P1 já em voo pode ter instante anterior à REABERTURA P2 que venceu a trava, mas ser gravada depois dela; a leitura P2 ordena pelo instante e pode concluir que a resposta foi apagada, enquanto a projeção ficou respondida. O campo `False` dessa resposta continua correto; o defeito é a ordem temporal. ([disputa_emissao.py:1517](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/disputa_emissao.py:1517), [models.py:4301](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4301))

2. **BLOQUEIA — validação e tarefa podem discordar.** Para `VALIDACAO → RESPOSTA(resposta_mantem_validacao=True)`, a leitura Python conserva a validação e a face SQL usa o mesmo booleano para esse juízo. Porém, se a pergunta chega a `_pela_decisao`, sem passivo e com texto não vazio, a tarefa olha o último ato RESPOSTA e retorna `aguarda_validacao`, com ação do admin. Para uma instância de marco, esse caminho é chamado mesmo sem a guarda `viva_c6` usada na exposição adicional ao admin. O resultado da tarefa inteira depende dos demais filtros e da célula; incidência em produção **[não verificado]**. ([models.py:4272](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4272), [models.py:3545](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:3545), [tarefa_do_dia.py:154](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/tarefa_do_dia.py:154), [tarefa_do_dia.py:319](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/tarefa_do_dia.py:319))

   A tela não tem resultado determinado somente por essa sequência: o card e o modal ainda leem `validada_em` da projeção; se ela estiver preenchida, mostram validada. Na mesma sequência com motivo preenchido, o diff não mostra mudança funcional em `_pela_decisao`, mas a igualdade do resultado completo com a main, inclusive projeção e tela, é **[não verificado]**. ([\_pergunta_card.html:23](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/templates/chamados/partials/_pergunta_card.html:23), [modal_fio.html:217](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/templates/chamados/partials/modal_fio.html:217))

3. **BLOQUEIA — duas fontes para vitalidade.** `q_vivas()` passou a excluir pela validação da sequência, mas o juiz Python C6 ainda começa pela projeção `p.validada_em`. Quando sequência e projeção divergem — inclusive no cenário do achado 1 — a consulta SQL e o juiz Python podem dar respostas diferentes para a mesma pergunta. Isso contraria o requisito de uma autoridade por pergunta da L-002; não é necessário supor dados reais para demonstrar a divergência dos critérios. ([models.py:3605](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:3605), [juizes.py:1382](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/juizes.py:1382), [juizes.py:1403](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/juizes.py:1403), [LEIS.md:60](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/docs/LEIS.md:60))

4. **FILA — relógio ainda fora da trava.** `reabrir_para_correcao` obtém o instante antes de adquirir a trava e o usa na REABERTURA ou RECUSA; a leitura nova ordena os atos por esse instante. Isso preserva a classe de risco corrigida na resposta humana. Chamadores examinados de materialização e recusa já obtêm trava externa, portanto **não afirmo uma corrida efetivamente alcançável por esses caminhos**; alcance por todos os chamadores e impacto em produção **[não verificado]**. ([disputa_emissao.py:1629](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/disputa_emissao.py:1629), [materializacao.py:1160](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/materializacao.py:1160), [validacao.py:920](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/validacao.py:920))

5. **NAO PROCEDE — a REABERTURA marcada não entra na tarefa carregada pelo P1/P2; FILA — aparece na trilha.** A carga filtra a marca antes de montar a sequência entregue à tarefa. Já `respostas_anteriores` inclui toda REABERTURA como ato com foto, e o modal mostra texto, data e motivo, inclusive quando o texto é vazio. Isso confirma o item 7 da triagem; o anexo o descreve como comportamento pretendido. Outro leitor P1 que decida diretamente pela linha marcada, além da trilha e da carga examinadas, **[não verificado]**; materialização, validação, vigia e reconciliador não foram demonstrados integralmente para todas as entradas. ([tarefa_do_dia.py:421](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/tarefa_do_dia.py:421), [models.py:3784](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:3784), [models.py:4155](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4155), [modal_fio.html:209](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/templates/chamados/partials/modal_fio.html:209))

6. **FILA — 0056: a equivalência histórica afirmada não está demonstrada.** A migração marca `True` para RESPOSTA cujo motivo é diferente de `''`, sem corte de data ou PK. Espaços contam como preenchido; ela não redefine para `False` uma linha já `True` com motivo vazio. Não há, no dado fornecido, censo de respostas humanas históricas com motivo, de motivos compostos só por espaços, nem dessas combinações de booleano e motivo: existência e impacto **[não verificado]**. ([0056:12](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12))

   A premissa de que **P1/main já aplicava esse predicado à sequência** não está comprovada: o diff fornecido introduz `DecisaoPergunta.leitura` no P2, enquanto os leitores antigos mostrados liam a projeção. Logo não certifico equivalência da 0056 com um leitor P1/main de `ato_motivo`. Pelo percurso normal das migrações, as linhas anteriores à 0051 eram REABERTURA e o campo foi renomeado antes de se acrescentar o tipo RESPOSTA; uma RESPOSTA anterior ao campo por outro caminho é **[não verificado]**. ([models.py:4285](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4285), [0051:40](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/migrations/0051_decisao_pergunta.py:40))

   O `RunPython` pede o modelo histórico; as migrações examinadas não serializam o manager imutável vivo, de modo que o `update` histórico não chama, por esse caminho, o `_DecisaoQuerySet.update` que levanta exceção. Trigger existente no banco de produção **[não verificado]**. O `SET CONSTRAINTS` força a checagem imediata das constraints diferidas; a justificativa da 0051 era um `ALTER` posterior, ausente ao fim da 0056. Sua necessidade aqui e sua suficiência diante de triggers reais são **[não verificado]**. ([0056:12](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12), [models.py:4112](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4112), [0051:101](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/migrations/0051_decisao_pergunta.py:101), [0056:23](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:23))

7. **NAO PROCEDE, com limites — censo dos dois campos.** Os quatro montadores de RESPOSTA encontrados declaram `resposta_mantem_validacao`: humana `False`; automática, carimbo pela trilha e foto da projeção `True`. A foto também alimenta backfills indiretamente. A REABERTURA comum não declara explicitamente `da_ressuscitacao`: recebe `False` pelo default; a ressuscitação declara `True`. Portanto, a afirmação “cada REABERTURA declara o campo” **não procede literalmente**, embora os valores desses caminhos sejam os esperados. ([disputa_emissao.py:1532](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/disputa_emissao.py:1532), [disputa_emissao.py:1686](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/disputa_emissao.py:1686), [disputa_emissao.py:1708](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/disputa_emissao.py:1708), [models.py:4251](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4251), [disputa_emissao.py:1639](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/disputa_emissao.py:1639), [models.py:4178](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4178))

   Os leitores de negócio diretos do primeiro booleano são `_resposta_desfaz_validacao` e `q_validada_pelo_ato`; ambos tratam `False` como resposta que desfaz. A marca de ressuscitação é lida por `es_da_ressuscitacao`, usado pela carga da tarefa. Não encontrei leitor de RESPOSTA que ainda use `ato_motivo` para decidir esse booleano; o uso encontrado em materialização identifica uma DEVOLUCAO BUG-PAR. A criação normal converge em `registrar`; a 0056 é atualização excepcional fora dessa porta. Reflexão ou SQL arbitrário fora do censo **[não verificado]**. ([models.py:4272](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4272), [models.py:3547](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:3547), [models.py:4212](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4212), [materializacao.py:275](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/materializacao.py:275), [models.py:4324](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4324))

8. **NAO PROCEDE para os selos examinados; FILA para seus limites.** O teste da 0056 insere uma RESPOSTA com motivo e default `False`, chama `preencher` e exige `True`: a remoção básica do update o faria falhar. Ele não executa a operação inteira da migração nem o `SET CONSTRAINTS`, e desfaz o savepoint antes da checagem final de FK. Executei, sem escrita e sem banco, o teste de censo por sítio do E4B C12: **passou**; isso verifica o alinhamento coberto por esse teste, não todos os selos da raia. O selo das respostas documenta que montagem indireta com ato variável fica fora de sua análise AST. ([test_f2_p2_backfill_0056.py:23](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/tests/test_f2_p2_backfill_0056.py:23), [test_e4b_c12_selo.py:21](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/tests/test_e4b_c12_selo.py:21), [test_f2_resposta_mantem_validacao.py:6](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/tests/test_f2_resposta_mantem_validacao.py:6))

Quanto às demais leis e à ordem global de travas, **não certifico ausência de outras violações**: a inspeção acima encontrou as divergências concretas descritas, mas não provou todos os caminhos tocados pelo diff. [não verificado]

## 17/11 15:51 · sessao 1bf0a76b13d6 · - · - · origem=Ronald (tmux)

- **BLOQUEIA — relógio antes da trava.** `reabrir_para_correcao` coleta o instante antes de travar o colaborador e usa esse instante no ato; depois limpa a projeção. Se uma resposta entrar enquanto a reabertura espera a trava, a sequência pode ordenar a reabertura antes da resposta, embora a projeção tenha sido limpa depois. A leitura da sequência então conserva a resposta. [disputa_emissao.py:1629](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/disputa_emissao.py:1629), [disputa_emissao.py:1639](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/services/disputa_emissao.py:1639), [models.py:4305](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:4305).

- **BLOQUEIA — fontes diferentes para vida.** O filtro SQL exclui a pergunta validada pela sequência, enquanto o C6 lê a validação da projeção. Com validação vigente na sequência e `validada_em=None` na projeção, o SQL pode excluir uma pergunta que o C6 considera viva, contrariando a contenção declarada. Ocorrência em produção: **[nao verificado]**. [models.py:3576](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:3576), [models.py:3608](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/models.py:3608), [juizes.py:1382](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/juizes.py:1382), [juizes.py:1403](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/juizes.py:1403).

- **NAO PROCEDE — desalinhamento específico do selo E4B C12.** Os nove sítios nominais permanecem nas linhas declaradas; executei o módulo sem banco e sem escrita de bytecode: 11 testes passaram. O censo completo é exercido em [test_e4b_c12_selo.py:21](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/tests/test_e4b_c12_selo.py:21); as linhas nominais estão em [e4b_c12_ast.py:17](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/tests/e4b_c12_ast.py:17).

- **FILA — limite do teste da 0056.** Ele morde a ausência do preenchimento básico: insere resposta com motivo e valor padrão, chama `preencher` e exige `True`. Não executa as operações completas da migração nem testa `SET CONSTRAINTS`; reverte o savepoint antes da checagem da FK. [test_f2_p2_backfill_0056.py:23](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/tests/test_f2_p2_backfill_0056.py:23), [test_f2_p2_backfill_0056.py:35](/home/ronald/codex_ws/73dd280735b2d14a4b6b1be0587b690daf165aa4/app/chamados/tests/test_f2_p2_backfill_0056.py:35). A necessidade e suficiência do `SET CONSTRAINTS` na instalação real e a existência de triggers no banco: **[nao verificado]**.

## 17/11 15:51 · sessao 01c6432fa93c · - · - · origem=Ronald (tmux)

Censo e ressalvas enviados ao agente principal, com referências aos escritores, leitores, migração e chamadas que adquirem trava antes de reabrir.

## 17/11 15:51 · sessao 7b584747ec0a · - · - · origem=Ronald (tmux)

Achados e referências enviados ao agente principal. A cópia não contém metadados Git, então a comparação com P1 e main ficou limitada ao diff fornecido e ao código presente. [não verificado]

## 17/11 15:44 · sessao 663bc85bf2f6 · E4C-F1-B2 · leitura · origem=Code

```python
# SONDA1: texto para bin/sonda_leitura.sh; nao executada. [nao verificado]
# A propria sonda so le; o invocador tambem altera credencial/GRANTs e
# arquivos de apoio e trilha: bin/sonda_leitura.sh:28, :115, :145, :245.
# Resultado em producao e cobertura: [nao verificado].
#
# Referencias das funcoes de dominio chamadas diretamente:
# pergunta_viva: app/chamados/juizes.py:866
# _dia_de_turno_da_pergunta: app/chamados/juizes.py:338
# familia_do_motivo: app/chamados/catalogo/motivos.py:53
# _fonte_materializada: app/chamados/management/commands/
#     migrar_data_evento_dia_do_turno.py:65
# _fonte_viva: mesmo arquivo:94
# turnos_do_colab: app/ponto/turnos.py:1850
# dia_do_turno_da_batida: app/ponto/turnos.py:2939
# competencia_do_dia_entregue: app/chamados/services/vigia_fio.py:785
#
# _fonte_viva retorna None tanto para caso coerente quanto para ausencia de
# turno identificavel: mesmo arquivo do migrador:94-125. Aqui None e
# INDETERMINADA; nao e convertido em dia igual nem em zero.
# Conversa nao tem dia de turno pela porta: app/chamados/juizes.py:338-342.
# Outras classes sem fonte definida nos ramos pedidos ficam INDETERMINADAS.
# As contagens usam uma unica fotografia REPEATABLE READ; igualdade de
# contagens nao prova ausencia de alteracoes concorrentes. [nao verificado]

import datetime as dt
from collections import Counter, defaultdict

from django.db import connection, transaction
from django_tenants.utils import schema_context

from chamados.catalogo.motivos import FAMILIA_BATIDA, familia_do_motivo
from chamados.juizes import _dia_de_turno_da_pergunta, pergunta_viva
from chamados.management.commands.migrar_data_evento_dia_do_turno import (
    _Fora,
    _fonte_materializada,
    _fonte_viva,
)
from chamados.models import PerguntaDisputa
from chamados.services.vigia_fio import competencia_do_dia_entregue
from ponto.models import Batida
from ponto.turnos import dia_do_turno_da_batida, turnos_do_colab


def contagens():
    # Todas as tabelas comuns e particionadas dos dois schemas acessiveis pela
    # porta. ONLY conta cada relacao fisica uma vez, inclusive particoes.
    with connection.cursor() as cursor:
        cursor.execute("""
            SELECT n.nspname, c.relname
              FROM pg_catalog.pg_class c
              JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
             WHERE n.nspname IN ('public', 'juliani')
               AND c.relkind IN ('r', 'p')
             ORDER BY n.nspname, c.relname
        """)
        tabelas = cursor.fetchall()
        resultado = {}
        for schema, tabela in tabelas:
            identificador = (
                connection.ops.quote_name(schema)
                + "."
                + connection.ops.quote_name(tabela)
            )
            cursor.execute("SELECT count(*) FROM ONLY " + identificador)
            resultado[(schema, tabela)] = cursor.fetchone()[0]
        return resultado


def resumo_empresas(contador):
    return " ".join(
        "empresa%s=%s" % (empresa, contador.get(empresa, 0))
        for empresa in (2, 3, 4)
    ) + " outras=%s" % sum(
        n for empresa, n in contador.items() if empresa not in (2, 3, 4)
    )


def fonte_da_pergunta(p, colab, dia):
    # Episodio BATIDA e a propria batida de origem:
    # app/chamados/models.py:2541-2546.
    if familia_do_motivo(p.motivo) == FAMILIA_BATIDA:
        if not p.batida_origem_id:
            return None
        return Batida.objects.filter(
            pk=p.batida_origem_id, retratada_em__isnull=True
        ).first()

    # O migrador usa estas fontes para perguntas de hora:
    # migrar_data_evento_dia_do_turno.py:65-79, :94-126, :147-157.
    if p.motivo not in ("saida_sem_entrada", "orfao_14h"):
        return None
    if dia is None:
        return None

    um = dt.timedelta(days=1)
    turnos = turnos_do_colab(colab, dia - 2 * um, dia + um)
    if p.materializada_em:
        plantadas = {
            p.pk: list(Batida.objects.filter(
                pergunta_origem_id=p.pk, retratada_em__isnull=True
            ))
        }
        fonte, _turno = _fonte_materializada(p, turnos, plantadas)
        return fonte

    resultado = _fonte_viva(p, colab, turnos)
    return resultado[0] if resultado is not None else None


def executar():
    falhas = []
    vivas = 0
    alvos = []
    indeterminadas = Counter()
    dias_indeterminados = Counter()
    memo_competencias = {}

    with schema_context("juliani"):
        with transaction.atomic():
            try:
                # Antes de qualquer consulta desta transacao.
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                    )
                    cursor.execute("SELECT current_user, current_schema()")
                    role, schema = cursor.fetchone()
                    print("ROLE=%s SCHEMA=%s" % (role, schema))
                    if role != "leitor" or schema != "juliani":
                        raise RuntimeError("role_ou_schema_incorreto")

                antes = contagens()
                for (schema, tabela), quantidade in antes.items():
                    print("ANTES %s.%s=%s" % (schema, tabela, quantidade))

                perguntas = PerguntaDisputa.objects.select_related(
                    "disputa__colaborador__empresa"
                ).order_by("pk")

                for p in perguntas.iterator(chunk_size=200):
                    cid = p.disputa.colaborador_id
                    empresa_id = p.disputa.colaborador.empresa_id
                    etapa = "juiz_viva"
                    try:
                        # Savepoint recupera erros SQL sem invalidar a
                        # transacao principal e permite listar outras falhas.
                        with transaction.atomic():
                            if not pergunta_viva(p):
                                continue
                            vivas += 1
                            colab = p.disputa.colaborador
                            empresa = colab.empresa
                            etapa = "dia_gravado"
                            dia_gravado = _dia_de_turno_da_pergunta(p)
                            etapa = "fonte"
                            try:
                                fonte = fonte_da_pergunta(p, colab, dia_gravado)
                            except _Fora:
                                fonte = None
                            if fonte is None:
                                indeterminadas[empresa_id] += 1
                                continue

                            etapa = "dia_turno"
                            dia_turno = dia_do_turno_da_batida(colab, fonte)
                            if dia_turno is None:
                                indeterminadas[empresa_id] += 1
                                continue
                            if dia_gravado is None:
                                dias_indeterminados[empresa_id] += 1
                                continue

                            etapa = "competencia"
                            entregue_gravado = competencia_do_dia_entregue(
                                dia_gravado, empresa, memo=memo_competencias
                            )
                            entregue_turno = competencia_do_dia_entregue(
                                dia_turno, empresa, memo=memo_competencias
                            )
                            if entregue_gravado is None or entregue_turno is None:
                                dias_indeterminados[empresa_id] += 1
                            elif entregue_gravado is False and entregue_turno is True:
                                alvos.append(
                                    (cid, p.pk, empresa_id, dia_gravado, dia_turno)
                                )
                    except Exception as exc:
                        # Nao imprime mensagem da excecao: pode conter dados
                        # pessoais. A etapa e o tipo permitem localizar a falha.
                        falhas.append((cid, p.pk, etapa, type(exc).__name__))

                por_empresa = Counter(item[2] for item in alvos)
                pessoas = {item[0] for item in alvos}
                print("UNIVERSO_VIVO=%s" % vivas)
                print(
                    "ALVO=%s PESSOAS_DISTINTAS=%s %s"
                    % (len(alvos), len(pessoas), resumo_empresas(por_empresa))
                )
                for cid, pk, empresa_id, gravado, turno in alvos[:3]:
                    print(
                        "EXEMPLO col%s pergunta=%s empresa=%s "
                        "dia_gravado=%s dia_turno=%s"
                        % (cid, pk, empresa_id, gravado, turno)
                    )
                print(
                    "FONTE_INDETERMINADA=%s %s"
                    % (sum(indeterminadas.values()), resumo_empresas(indeterminadas))
                )
                print(
                    "DIA_INDETERMINADO=%s %s"
                    % (
                        sum(dias_indeterminados.values()),
                        resumo_empresas(dias_indeterminados),
                    )
                )
                print("FALHAS=%s" % len(falhas))
                for cid, pk, etapa, tipo in falhas:
                    print(
                        "FALHA col%s pergunta=%s etapa=%s tipo=%s"
                        % (cid, pk, etapa, tipo)
                    )
                if falhas:
                    print("COBERTURA INCOMPLETA")

                depois = contagens()
                for (schema, tabela), quantidade in depois.items():
                    print("DEPOIS %s.%s=%s" % (schema, tabela, quantidade))
                print("CONTAGENS_IGUAIS=%s" % (antes == depois))
            finally:
                transaction.set_rollback(True)


try:
    executar()
except Exception as exc:
    # Falha geral impede concluir o censo; nao expor texto da excecao.
    print("FALHA_GERAL tipo=%s" % type(exc).__name__)
    print("COBERTURA INCOMPLETA")
    print("CONTAGENS_IGUAIS=INDETERMINADO")
finally:
    print("FIM_SONDA1")
```

## 17/11 15:44 · sessao bb3764e09ea6 · E4C-F1-B2 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 2: PROCEDE.** A decisão de cercar o satélite é compatível com a ideia de ato próprio, mas o texto não define a unidade do rollback nem o dono da trilha desse ato. Essas escolhas mudam o comportamento de `reconciliar_chamados_colab` e precisam voltar ao Ronald. ([app/chamados/signals.py:365](app/chamados/signals.py#L365); [app/chamados/reconciliador.py:1457](app/chamados/reconciliador.py#L1457))

**AVAL-DEVOLVIDO — item 3: PROCEDE.** A porta viva e a vigilância do lote são identificáveis, mas “cada sítio passa pela porta” deixa ambíguo onde se abre a cerca e qual ato é desfeito, especialmente na tranca e em chamadas aninhadas. A correção dos falsos positivos é possível em princípio, mas o selo proposto não está disponível no dado para confirmar a implementação ou sua mordida. ([app/chamados/services/fio.py:1389](app/chamados/services/fio.py#L1389); [app/chamados/services/fio.py:1425](app/chamados/services/fio.py#L1425); [app/chamados/services/disputa_emissao.py:1896](app/chamados/services/disputa_emissao.py#L1896); selo `selo_diff.txt` [não verificado])

A cópia fornecida não contém metadados Git acessíveis: os caminhos e linhas abaixo foram conferidos na cópia, mas a identidade do seu `HEAD` com o hash informado está **[não verificado]**.

## Item 2 — satélite `on_commit`

**O que a cerca faz hoje.** A vigia anota as empresas vetadas e levanta `EscritaEmCompetenciaEntregue`. Na abertura externa, `executar_sob_cerca` executa o ato em `atomic`, desfaz esse escopo se houver veto — inclusive se a exceção tiver sido engolida — e retorna `(None, vetadas)`. Em cerca aninhada, ela executa no estado externo; a exceção ou a anotação fica para a abertura externa resolver. Exceção sem veto anotado é relançada. ([app/chamados/services/fio.py:1420](app/chamados/services/fio.py#L1420); [app/chamados/services/fio.py:1433](app/chamados/services/fio.py#L1433))

Logo, a premissa “veto não sobe para dentro do ato já commitado” é coerente para o veto tratado pela abertura **externa** do callback: ele é chamado por `on_commit` e a cerca anterior já restaura seu contexto ao terminar. Isso não equivale a dizer que qualquer erro do callback será convertido em veto; hoje o callback envolve sua execução em `engolir`. ([app/chamados/signals.py:365](app/chamados/signals.py#L365); [app/chamados/signals.py:371](app/chamados/signals.py#L371); [app/chamados/services/fio.py:1443](app/chamados/services/fio.py#L1443); [app/chamados/services/fio.py:1449](app/chamados/services/fio.py#L1449))

**A unidade precisa ser decidida.** Hoje o satélite executa, em sequência, absolvições pela célula, reconciliação de órfãs e retratação de premissas mortas. As absolvições têm `atomic` por carimbo e capturam falhas por pergunta; as órfãs também capturam por pergunta; as premissas capturam por chamado. ([app/chamados/reconciliador.py:1477](app/chamados/reconciliador.py#L1477); [app/chamados/reconciliador.py:636](app/chamados/reconciliador.py#L636); [app/chamados/reconciliador.py:645](app/chamados/reconciliador.py#L645); [app/chamados/reconciliador.py:1569](app/chamados/reconciliador.py#L1569); [app/chamados/reconciliador.py:1501](app/chamados/reconciliador.py#L1501))

Se o callback inteiro entrar em **uma** abertura externa, um veto anotado em qualquer etapa desfaz também as etapas anteriores bem-sucedidas daquele callback, apesar das capturas internas; sem essa abertura, as transações individuais podem concluir separadamente. O aval não explicita se quer rollback do satélite inteiro ou de cada ato interno. ([app/chamados/services/fio.py:1439](app/chamados/services/fio.py#L1439); [app/chamados/reconciliador.py:636](app/chamados/reconciliador.py#L636); [app/chamados/reconciliador.py:1477](app/chamados/reconciliador.py#L1477))

**Trilha.** O escritor canônico existente é `registrar_retencao(ch, ret)`: para `RECUSA_COMPETENCIA_ENTREGUE`, ele deduplica por chamado e empresas e delega a `core.observ.evento`. A cerca não escreve a trilha por si; o abridor deve fazê-lo depois do rollback, como já faz a porta do resolver. Para o satélite, o aval não especifica qual chamado identifica a recusa: o filho que disparou o callback ou outro chamado alcançado pela varredura. A escolha é relevante porque a deduplicação e o objeto da trilha usam `ch.pk`. A gravação da auditoria também é best-effort; garantia de linha persistida em caso de falha da própria auditoria é **[não verificado]**. ([app/chamados/reconciliador.py:459](app/chamados/reconciliador.py#L459); [app/chamados/reconciliador.py:477](app/chamados/reconciliador.py#L477); [app/chamados/reconciliador.py:489](app/chamados/reconciliador.py#L489); [app/chamados/services/disputa_emissao.py:1909](app/chamados/services/disputa_emissao.py#L1909); [app/core/observ.py:124](app/core/observ.py#L124))

Não identifiquei contradição direta entre a decisão de um ato próprio e L-099 ou a regra de escritor único; a lacuna é o contrato concreto de atomicidade e de atribuição da recusa. A L-099 exige dono da divergência e separa estrutura de cadastro e batida; ela não escolhe, por si, a unidade transacional deste callback. ([app/docs/LEIS.md:122](app/docs/LEIS.md#L122); [CLAUDE.md:178](CLAUDE.md#L178))

## Item 3 — os 12 sítios e o selo

A leitura literal de L-051 protege o passivo histórico com a via `competencia_trancada` e proíbe perguntas novas com essa via; o código da tranca declara também que a pergunta do dia trancado não deve ser escrita, enquanto chamado e disputa têm portas próprias. Portanto, cercar uma escrita de pergunta não exige criar outro escritor de estado, mas cercar **o ato inteiro** pode impedir escritas de chamado ou disputa que hoje pertencem às suas portas. ([app/docs/LEIS.md:92](app/docs/LEIS.md#L92); [app/chamados/services/competencia_trancada.py:8](app/chamados/services/competencia_trancada.py#L8); [app/chamados/models.py:1946](app/chamados/models.py#L1946))

| Sítios do anexo | Confronto e decisão ainda necessária |
|---|---|
| `reconciliar`, `transicionar`, `encerrar` | Convergem no escritor canônico `_gravar_estado`, que pode relavrar perguntas. A vigia pode proteger a escrita sem substituir esse escritor; falta definir o abridor e o rollback do ato avulso e de seus chamadores. ([app/chamados/models.py:128](app/chamados/models.py#L128); [app/chamados/models.py:450](app/chamados/models.py#L450); [app/chamados/models.py:655](app/chamados/models.py#L655); [app/chamados/models.py:1693](app/chamados/models.py#L1693); [app/chamados/models.py:305](app/chamados/models.py#L305)) |
| `reabrir` | O descarimbo em lote já chama a vigia quando existe cerca; a abertura avulsa e sua unidade permanecem a especificar. O escritor do descarimbo deve continuar sendo a porta existente. ([app/chamados/models.py:1639](app/chamados/models.py#L1639); [app/chamados/models.py:3401](app/chamados/models.py#L3401); [app/chamados/services/fio.py:1396](app/chamados/services/fio.py#L1396)) |
| `Disputa.fechar` | É a porta única da disputa e pode carimbar perguntas nas vias nomeadas. A cerca pode envolvê-la sem outro escritor, mas o veto do ato inteiro também desfaz o fecho da disputa. ([app/chamados/models.py:1946](app/chamados/models.py#L1946); [app/chamados/models.py:2009](app/chamados/models.py#L2009); [app/chamados/models.py:2023](app/chamados/models.py#L2023); [app/chamados/services/fio.py:1439](app/chamados/services/fio.py#L1439)) |
| `reapontar` e `lavrar` | A costura usa o escritor canônico e salva a pergunta; `lavrar` persiste pelo `save` quando solicitado. Já passam pela vigia **se** houver cerca externa; falta definir a fronteira das chamadas avulsas, sem duplicar a costura ou a lavra. ([app/chamados/services/fio.py:974](app/chamados/services/fio.py#L974); [app/chamados/veredito_lavrado.py:270](app/chamados/veredito_lavrado.py#L270); [app/chamados/models.py:3572](app/chamados/models.py#L3572)) |
| Encerramento por tranca | É o ponto de maior ambiguidade: a via da tranca deve encerrar o chamado e permitir o fecho próprio da disputa sem carimbar a pergunta trancada. O resolver dessa via retorna antes de escrever perguntas, mas `_gravar_estado` tem relavra de perguntas ancoradas; uma cerca abrangente pode vetar e desfazer também o encerramento do chamado. Não há base para excluir o sítio por nome nem para criar escrita alternativa. ([app/chamados/services/competencia_trancada.py:8](app/chamados/services/competencia_trancada.py#L8); [app/chamados/services/disputa_emissao.py:1896](app/chamados/services/disputa_emissao.py#L1896); [app/chamados/services/disputa_emissao.py:1931](app/chamados/services/disputa_emissao.py#L1931); [app/chamados/models.py:305](app/chamados/models.py#L305)) |
| Absolvidas pela célula e órfãs pela lâmpada | Ambas usam as portas existentes de carimbo/fecho; cercá-las não exige outro escritor. É preciso escolher se o veto desfaz cada carimbo ou uma varredura maior, pois as etapas e capturas atuais diferem. ([app/chamados/reconciliador.py:636](app/chamados/reconciliador.py#L636); [app/chamados/reconciliador.py:645](app/chamados/reconciliador.py#L645); [app/chamados/reconciliador.py:1191](app/chamados/reconciliador.py#L1191); [app/chamados/reconciliador.py:1215](app/chamados/reconciliador.py#L1215)) |
| Reatar órfã | O ramo HIST já abre cerca e registra recusa; os ramos `c1` e derivável seguem outros caminhos. Uma abertura comum deve preservar o escritor de costura e definir como a recusa dos demais ramos é atribuída, inclusive quando aninhada no satélite. ([app/chamados/reconciliador.py:1664](app/chamados/reconciliador.py#L1664); [app/chamados/reconciliador.py:1690](app/chamados/reconciliador.py#L1690); [app/chamados/reconciliador.py:1701](app/chamados/reconciliador.py#L1701); [app/chamados/reconciliador.py:1712](app/chamados/reconciliador.py#L1712)) |
| Lavra em lote | **Sim:** `gravar_lavras_em_lote` já concentra o `bulk_update` dos três campos; pode chamar a vigia sobre a lista imediatamente antes do bulk, mantendo a persistência nessa mesma porta. A vigia só veta dentro de uma cerca, portanto também é preciso definir quem abre o ato e o tamanho do rollback do lote; os dois chamadores mostrados são o lote cheio e a sobra do backfill. ([app/chamados/veredito_lavrado.py:237](app/chamados/veredito_lavrado.py#L237); [app/chamados/veredito_lavrado.py:248](app/chamados/veredito_lavrado.py#L248); [app/chamados/services/fio.py:1396](app/chamados/services/fio.py#L1396); [app/chamados/management/commands/backfill_veredito_lavrado.py:141](app/chamados/management/commands/backfill_veredito_lavrado.py#L141)) |

**Falsos positivos do grafo.** Os dois nomes associados ao resolver, segundo o anexo fornecido, incluem a via trancada; o código comprova que essa via retorna antes das consultas e escritas de pergunta. Uma cura sem exclusão nominal é o selo representar o ramo e o retorno antecipado, com um caso que aceite esse caminho sem escrita e um mutante que acrescente escrita alcançável antes do retorno e faça o selo falhar. Isso é uma proposta de teste estrutural, não uma certificação existente: o conteúdo de `selo_diff.txt` e a execução desse caso estão **[não verificado]**. Também não basta corrigir esses dois falsos positivos para provar completude: o próprio anexo relata callbacks perdidos pelo grafo; sua implementação exata no selo está **[não verificado]**. ([app/chamados/services/disputa_emissao.py:1896](app/chamados/services/disputa_emissao.py#L1896); [app/chamados/services/disputa_emissao.py:1931](app/chamados/services/disputa_emissao.py#L1931); [app/chamados/services/becos.py:55](app/chamados/services/becos.py#L55); [app/chamados/reconciliador.py:1690](app/chamados/reconciliador.py#L1690))

A ocorrência desses vetos ou conflitos em produção e os resultados de testes ou logs são **[não verificado]**. [nao verificado]

## 17/11 15:35 · sessao 812ed054067f · E4C-F1-B2 · verificador · origem=Code

**AVAL-DEVOLVIDO: itens 2 e 3, por premissa sobre a implementação viva neste hash.** As referências indicadas para a cerca correspondem a outros conteúdos; o `save` atual não contém a vigilância descrita no anexo. (`app/chamados/services/fio.py:1419`; `app/chamados/services/fio.py:1428`; `app/chamados/models.py:3550`)

**P1 / item 1 — NAO PROCEDE.**

**(i) Lei/código:** manter a leitura da cerca exclusivamente em `_dia_de_turno_da_pergunta` é coerente com juiz único. Essa porta devolve `data_evento`, ou `None` para conversa; ela não consulta batida nem corrige o legado. Medir o passivo separadamente preserva esse contrato. (`CLAUDE.md:20`; `app/chamados/juizes.py:338`; `app/chamados/juizes.py:342`)

**(ii) Premissas:** o dono citado existe: o comando seleciona/classifica, e `fio.rechavear_data_evento` escreve. Grupos cujo dia velho, novo ou de membro do fato toque competência entregue ficam inteiros **FORA**, publicados com DIFF e aguardando `!`; portanto esse passivo tem destino declarado. (`app/chamados/services/fio.py:1026`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:299`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:328`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:345`)

**“ENTREGUE” vem de K6**, `folha.export.competencia_entregue`: exportação vigente, sem invalidação. A composição `vigia_fio.competencia_do_dia_entregue` primeiro obtém a competência pela janela da empresa e depois chama K6; não equivale a testar mês civil, tranca ou holerite publicado. (`app/docs/CORTES.md:342`; `app/chamados/services/vigia_fio.py:799`; `app/folha/export.py:556`; `app/folha/export.py:588`)

A batida que alimenta `dia_do_turno_da_batida` depende da classe:

| Classe | Fonte existente |
|---|---|
| Episódio BATIDA | Precisamente `p.batida_origem_id`. (`app/chamados/services/fio.py:1534`) |
| Legada viva `saida_sem_entrada` | Saída do único turno só de saída identificado pelo dia gravado. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:98`) |
| Legada viva `orfao_14h` | Entrada do único turno aberto cuja saída prevista cai no dia gravado. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108`) |
| Materializada, no universo do migrador | Única batida plantada viva ligada por `pergunta_origem`; há exclusão própria para `orfao_14h` materializada. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:168`) |

O migrador entrega essa fonte ao juiz na linha 160. Usar universalmente `batida_origem` perderia justamente os dois motivos noturnos, que são MARCO. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160`; `app/chamados/catalogo/motivos.py:34`; `app/chamados/models.py:2541`)

**(iii) Dono/ambiguidade:** há caminhos existentes para a fonte e para o FORA. Na sonda, “viva” deve vir de `pergunta_viva`, não do rótulo simplificado do migrador baseado em `materializada_em`; fontes indeterminadas devem aparecer separadamente, sem virar zero implícito. (`app/chamados/juizes.py:866`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:173`; `CLAUDE.md:35`)

Contagem, pessoas e empresas desse cruzamento em produção: **[nao verificado]**.

**P2 / item 2 — PROCEDE, por premissa factual da base.**

**(i) Lei:** tratar o satélite como ato próprio é compatível com escritor único e trilha. (`CLAUDE.md:32`)

**(ii) Premissa errada:** `_reconciliar` existe e está registrado em `on_commit`, mas chama diretamente `reconciliar_chamados_colab`, sob `engolir`. No trecho atribuído a `executar_sob_cerca` há constantes de identidade e `q_do_horizonte`; não há ali a implementação descrita por pv3.r. Assim, não é possível comprovar neste hash o contrato da **“mesma executar_sob_cerca”**. (`app/chamados/signals.py:365`; `app/chamados/signals.py:370`; `app/chamados/signals.py:371`; `app/chamados/services/fio.py:1419`; `app/chamados/services/fio.py:1428`)

**(iii) Atomicidade/trilha:** `engolir` já impede propagação de `Exception`, mas apenas registra contador e `logging.error`. Isso não demonstra rollback integral do satélite: o reconciliador chama várias etapas, e absolvidas tem transação por pergunta e captura interna de exceções. (`app/core/observ.py:73`; `app/core/observ.py:85`; `app/chamados/reconciliador.py:1433`; `app/chamados/reconciliador.py:593`; `app/chamados/reconciliador.py:602`)

Para cumprir “vetado, não escreve, deixa trilha”, a implementação precisa desfazer a unidade satélite e registrar o veto **depois desse rollback**, pelo escritor canônico `core.observ.evento`. A trilha específica desse veto: **[nao verificado]**. A porta canônica cria `LogAuditoria`; `registrar_retencao` não aceita genericamente esse motivo — seu ramo residual registraria falsamente `justificativa_alheia`. (`app/core/observ.py:94`; `app/core/observ.py:133`; `app/chamados/reconciliador.py:467`; `app/chamados/reconciliador.py:474`)

**Motivo da devolução:** identificar a base que contém a cerca citada e reapresentar seu contrato de rollback e tradução do veto; o callback vivo, sozinho, não comprova o comportamento pedido. (`app/chamados/signals.py:365`; `app/chamados/reconciliador.py:1433`)

**P3 / item 3 — PROCEDE pela expressão “guarda VIVO”; cercar os sítios não apresenta conflito necessário com L-051.**

Os 12 sítios têm correspondência nominal nesta cópia:

| Grupo | Sítios vivos |
|---|---|
| 7 avulsos | `transicionar` (`app/chamados/models.py:128`); `reconciliar` (`app/chamados/models.py:642`); `reabrir` (`app/chamados/models.py:1409`); `encerrar` (`app/chamados/models.py:1669`); `DisputaSupervisao.fechar` (`app/chamados/models.py:1936`); `reapontar_resolvedora` (`app/chamados/services/fio.py:851`); `lavrar` (`app/chamados/veredito_lavrado.py:254`). |
| Tranca | `encerrar_por_competencia_trancada`. (`app/chamados/models.py:1686`) |
| 2 lâmpada | `retratar_perguntas_absolvidas_por_celula`; `encerrar_orfas_pela_lampada`. (`app/chamados/reconciliador.py:550`; `app/chamados/reconciliador.py:1113`) |
| Reatar | `reatar_pergunta_orfa`. (`app/chamados/reconciliador.py:1534`) |
| Lote | `gravar_lavras_em_lote`. (`app/chamados/veredito_lavrado.py:240`) |

**(i) Lei/escritores:** L-051 mantém o passivo intocado e proíbe nova pergunta com a via da tranca. O código atual faz a pergunta sair por leitura, sem carimbo; a porta encerra o **Chamado** por `_gravar_estado`, e `Disputa.fechar` exclui a tranca dos ramos que carimbam perguntas. Cercar escritas efetivas não exige alterar esses contratos. (`app/docs/LEIS.md:92`; `app/docs/CORTES.md:287`; `app/chamados/services/competencia_trancada.py:133`; `app/chamados/models.py:1703`; `app/chamados/models.py:1946`; `app/chamados/models.py:1999`)

A cerca pode envolver os atos preservando os escritores existentes: `_gravar_estado` para estado do Chamado, portas do carimbo, costura e porta da lavra. (`app/chamados/models.py:296`; `app/chamados/models.py:3187`; `app/chamados/services/fio.py:972`; `app/chamados/veredito_lavrado.py:275`)

**Lavra em lote: sim**, pode receber vigilância dentro de `gravar_lavras_em_lote`, antes do `bulk_update`, conservando os três campos declarados e o escritor atual. Lote cheio e sobra já passam por essa função. (`app/chamados/veredito_lavrado.py:237`; `app/chamados/veredito_lavrado.py:248`; `app/chamados/veredito_lavrado.py:250`; `app/chamados/management/commands/backfill_veredito_lavrado.py:143`; `app/chamados/management/commands/backfill_veredito_lavrado.py:146`)

**(ii)/(iii) Motivo da devolução:** a premissa “porta VIVA” não corresponde ao `save` desta cópia, que chama recusa do par, identidade de nascimento e `super().save`. É preciso corrigir a referência da base antes de certificar a cobertura dos 12 sítios. Correção dos dois falsos positivos e completude do selo anexado: **[nao verificado]**. (`app/chamados/models.py:3550`; `app/chamados/models.py:3558`; `app/chamados/models.py:3561`)

**P4 / item 4 — NAO PROCEDE.**

**(i)/(ii) Coerência:** `RELATO` é exatamente `app/docs/RELATO.md`. O coletor atual reúne `logs/<OBRA>/brief*.md` com os briefs citados na abertura; ele coleta documentos a julgar, enquanto o item 4 define onde buscar evidência das decisões. Excluir briefs da busca não os retira da validação de CENSO. (`bin/disjuntor.py:125`; `bin/disjuntor.py:1599`; `bin/disjuntor.py:1607`; `bin/disjuntor.py:1717`)

**(iii) Complemento:** `<OBRA>` resolve como o **ID completo `<MAE>-<COMPL>`**, sem separar mãe e complemento. `OBRA_CENSO_ID` aceita esse token, e `_briefs_da_obra` o usa integralmente como nome da pasta; não há fallback para a pasta da mãe. (`bin/disjuntor.py:162`; `bin/disjuntor.py:1601`; `bin/disjuntor.py:1604`)

O casamento literal pedido é uma exigência a implementar: o juiz atual verifica a forma do bloco, referências e perguntas cortadas; não executa essa busca documental. Conformidade de uma implementação futura dessa busca: **[nao verificado]**. (`bin/disjuntor.py:1543`; `bin/disjuntor.py:1565`; `bin/disjuntor.py:1574`)

## 17/11 15:33 · sessao da74144497fc · - · - · origem=Ronald (tmux)

**P1 — NAO PROCEDE.** Não identifiquei contradição que exija devolver o item ao Ronald; a seleção da fonte pode seguir os caminhos já existentes, sem criar outro juiz. (`app/chamados/juizes.py:338`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:150`; `app/chamados/services/fio.py:1026`)

- **Porta do dia:** `_dia_de_turno_da_pergunta` devolve `data_evento`, exceto conversa, que recebe `None`. Não consulta batida nem corrige o legado. Preservar essa porta e medir o passivo separadamente é executável. (`app/chamados/juizes.py:338-342`)
- **ENTREGUE:** vem de `folha.export.competencia_entregue`, juiz K6 autorizado por corte. `vigia_fio.competencia_do_dia_entregue` primeiro consulta a janela da empresa e usa mês/ano do fim da janela; depois chama K6. Este exige exportação vigente sem invalidação, não basta status de fechamento nem holerite publicado. (`app/docs/CORTES.md:342`; `app/chamados/services/vigia_fio.py:799-807`; `app/folha/export.py:556`; `app/folha/export.py:588-594`)
- **Qual batida:** para episódio BATIDA, `_dia_do_fato` consulta precisamente `p.batida_origem_id`. Para o legado noturno dos dois motivos MARCO, essa FK não é a fonte: o migrador usa a **saída** do único turno só de saída (`saida_sem_entrada`) ou a **entrada** do único turno aberto cuja saída prevista cai no dia gravado (`orfao_14h`). Materializada usa a única batida plantada viva ligada por `pergunta_origem`. Essas fontes alimentam `dia_do_turno_da_batida` na linha 160. (`app/chamados/services/fio.py:1534-1541`; `app/chamados/catalogo/motivos.py:34-35`; `app/chamados/models.py:2541-2545`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65-79`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:98-126`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160`)
- **Dono do passivo:** o comando identifica/classifica; `rechavear_data_evento` é a porta de escrita. Grupos que toquem competência entregue ficam inteiros FORA, publicados com DIFF e aguardando `!`; os grupos limpos seguem. Portanto o caso “gravado aberto, turno entregue” já tem destino declarado, embora não seja aplicado automaticamente. (`app/chamados/services/fio.py:1026-1040`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:328-346`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:393-400`)
- **Cuidados executáveis:** “viva” deve ser perguntado a `pergunta_viva`; o rótulo do migrador, baseado somente em `materializada_em`, não substitui esse juiz. Fonte ausente/ambígua deve sair declarada FORA, sem virar zero implícito. (`app/chamados/juizes.py:866`; `app/chamados/juizes.py:893`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:121-124`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:173-175`)

**Contagem, pessoas e empresas do cruzamento solicitado: [nao verificado].** O dado fornecido não contém resultado dessa sonda. **[nao verificado]**

## 17/11 15:33 · sessao 61097389b01c · E4C-F2 · verificador · origem=Code

1. **NAO PROCEDE — persistência do achado 1: curado no código.** `resposta_mantem_validacao` casa `resposta` e agora está em `ISENTOS` (`bin/sombra_regras_pessoais.py:163`, `bin/sombra_regras_pessoais.py:236`). As migrations criam somente esse booleano e `da_ressuscitacao`; o segundo não casa `PADRAO_PESSOAL`. Portanto, nenhuma **outra coluna criada pelas 0054/0055** fica descoberta por esse critério (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:15`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:16`, `bin/sombra_regras_pessoais.py:161`).

2. **NAO PROCEDE — regressão nos leitores das regras; há mudança esperada na saída.** Mantido o mesmo esquema/modelo contendo a coluna:
   - O inventário ganha uma cobertura e perde essa ocorrência em `achadas_fora`; sai com `rc=0` se não houver outra descoberta (`bin/inventario_pessoal_dos_modelos.py:52`, `:78`, `:85`, `:92`).
   - `sombra_selo_pessoal` ganha uma em `inventariadas` e uma em `isentas`; `colunas_pessoais` permanece igual. A falha dessa coluna desaparece; outras falhas ainda podem impedir o selo (`bin/sombra_selo_pessoal.py:25`, `:34`, `:93`, `:94`).
   - A máscara pessoal percorre as regras de transformação, sem consultar `ISENTOS`; a nova isenção não muda sua transformação (`bin/sombra_mascara_pessoal.py:17`, `:44`). Execução real dos selos nesta rodada: **[nao verificado]**.

3. **NAO PROCEDE — reabrir o item 3.** A limitação da montagem indireta está documentada e corresponde ao detector (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:7`, `:32`, `:51`). A via genérica apenas materializa os dicionários recebidos; o backfill delega ao montador de projeção coberto (`app/chamados/models.py:4149`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:25`).

4. **FILA — corrigir uma afirmação factual do item 5, sem reabrir seu risco.** “Posto/Colaborador” está impreciso: ambos os chamadores passam **Colaborador**, na admissão e na edição (`app/colaboradores/views.py:1115`, `app/colaboradores/services/colaborador.py:209`, `app/colaboradores/views.py:1280`). A comparação de fotos realmente captura todos os campos concretos de `DecisaoPergunta`, mas antes/depois usam o mesmo modelo na mesma execução (`app/chamados/services/foto_e4b.py:251`, `:280`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:303`, `:319`). Os exemplos conferidos de `values()` também comparam dentro do mesmo teste (`app/chamados/tests/test_e4b_c4_orm.py:315`, `:334`). A medida específica de `describe`, sem localização no anexo: **[nao verificado]**.

5. **FILA documental — confronto com a lei anexada na volta 1: [nao verificado].** O texto daquela lei não consta dos anexos desta mensagem. A isenção corresponde a um campo booleano, conforme modelo e migration (`app/chamados/models.py:4064`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:20`); isso sustenta a cura técnica, mas não certifica conformidade com o anexo ausente.

## 17/11 15:33 · sessao 1af8a116569a · - · - · origem=Ronald (tmux)

**P3/item 3 — PROCEDE pela premissa “guarda VIVO”; não encontrei conflito necessário com L-051 ou com os escritores existentes.** O `save` vivo somente abre `atomic`, chama `_recusa_par_ilegal`, gera a chave no nascimento e delega ao `super`; não chama `vigiar_escrita_de_perguntas` (`app/chamados/models.py:3550-3561`). No trecho indicado de `fio.py`, existe `retratar_duplicata`, seguido das constantes da migração e de `q_do_horizonte`; não existe ali `executar_sob_cerca` (`app/chamados/services/fio.py:1392-1428`). Busca textual em `app/chamados` e `bin` também não localizou os dois identificadores; existência da implementação anexada em outra árvore: **[nao verificado]**.

Os 12 sítios descritos têm correspondência nesta cópia:

| Grupo | Sítios vivos |
|---|---|
| 7 avulsos | `reconciliar` (`app/chamados/models.py:642`); `transicionar` (`app/chamados/models.py:128`); `encerrar` (`app/chamados/models.py:1669`); `reabrir` (`app/chamados/models.py:1409`); `DisputaSupervisao.fechar` (`app/chamados/models.py:1936`); `reapontar_resolvedora` (`app/chamados/services/fio.py:851`); `lavrar` (`app/chamados/veredito_lavrado.py:254`). |
| Tranca | `encerrar_por_competencia_trancada` (`app/chamados/models.py:1686`). |
| 2 lâmpada | `retratar_perguntas_absolvidas_por_celula` (`app/chamados/reconciliador.py:550`); `encerrar_orfas_pela_lampada` (`app/chamados/reconciliador.py:1113`). |
| Reatar | `reatar_pergunta_orfa` (`app/chamados/reconciliador.py:1534`). |
| Lote | `gravar_lavras_em_lote` (`app/chamados/veredito_lavrado.py:240`). |

**L-051:** acrescentar julgamento antes das escritas efetivas não exige criar carimbo de tranca. A lei mantém o passivo intocado e proíbe via nova (`app/docs/LEIS.md:92`; `app/docs/CORTES.md:287`). O código vivo já declara a morte da pergunta por leitura, sem carimbo (`app/chamados/services/competencia_trancada.py:133-135`); a porta da tranca grava o estado do **Chamado** pelo escritor `_gravar_estado` (`app/chamados/models.py:1699-1703`). A porta `Disputa.fechar` só carimba perguntas nas exceções desligamento/admin, excluindo a tranca (`app/chamados/models.py:1946`; `app/chamados/models.py:1999-2017`). Ambas as portas de carimbo rejeitam escrever essa via (`app/chamados/models.py:3228-3229`; `app/chamados/models.py:3288-3289`).

**Escritor único:** a cerca pode envolver os atos e a vigia anteceder a persistência sem atribuir estado por fora: estado do Chamado permanece em `_gravar_estado` (`app/chamados/models.py:228-296`), carimbo permanece nas portas documentadas (`app/chamados/models.py:3183-3194`), costura permanece em `reapontar_resolvedora`, que lavra em memória e salva conjuntamente (`app/chamados/services/fio.py:972-975`). Isso é compatível com um escritor por estado (`CLAUDE.md:32-34`).

**Lavra em lote: sim, pode ganhar vigia no próprio lote, antes do `bulk_update`, conservando esse escritor.** A lista é materializada dentro da função e os únicos campos persistidos são `CAMPOS_DA_LAVRA` (`app/chamados/veredito_lavrado.py:237-250`). O backfill já chama essa porta para lote cheio e sobra (`app/chamados/management/commands/backfill_veredito_lavrado.py:138-146`). Implementação concreta da vigia e de seu rollback neste hash: **[nao verificado]**, pois os identificadores prometidos não estão presentes no código citado acima.

**Motivo de devolução:** corrigir a referência à implementação “viva” e reapresentar a matriz contra esta cópia; não usar as linhas do anexo como prova de cercas já existentes (`app/chamados/models.py:3550-3561`; `app/chamados/services/fio.py:1392-1428`).

## 17/11 15:33 · sessao 01584a8f7648 · - · - · origem=Ronald (tmux)

**P2/item 2 — PROCEDE por premissa factual incompatível com esta cópia e ausência de contrato verificável da cerca.** O princípio de ato próprio é compatível com escritor único; a suposta implementação compartilhada não está disponível aqui. (`app/docs/LEIS.md:65`; `app/chamados/services/fio.py:1413`; `app/chamados/services/fio.py:1420`)

- O callback existe: `_reconciliar` chama `reconciliar_chamados_colab` dentro de `engolir` e é registrado com `transaction.on_commit`. Ele não chama uma cerca. (`app/chamados/signals.py:365`; `app/chamados/signals.py:370`; `app/chamados/signals.py:371`)
- **A premissa “mesma executar_sob_cerca (~1420–1450)” não corresponde ao vivo:** nesse trecho há `CHAVE_DA_FAMILIA`, `_NULO_FORA_DO_INDICE` e `q_do_horizonte`; a função nomeada não foi encontrada na busca da cópia. A ausência global fica **[nao verificado]** por não ter linha positiva, mas o trecho citado está objetivamente errado. (`app/chamados/services/fio.py:1419`; `app/chamados/services/fio.py:1425`; `app/chamados/services/fio.py:1428`)
- Também o `save` vivo da pergunta possui somente `atomic`, recusa do par de resposta, identidade de nascimento e `super().save`; não há vigilância de competência nesse corpo. (`app/chamados/models.py:3550`; `app/chamados/models.py:3557`; `app/chamados/models.py:3561`)
- “Não levantar” tem apoio no callback atual: `engolir` captura `Exception`. Mas essa captura apenas incrementa contador e escreve `logging.error`; não cria linha em `LogAuditoria` nem demonstra rollback de todas as escritas já feitas pelo satélite. (`app/core/observ.py:73`; `app/core/observ.py:79`; `app/core/observ.py:85`; `app/core/observ.py:90`)
- O satélite chama absolvidas, órfãs e premissa morta em sequência, sem `atomic` envolvendo a unidade inteira; absolvidas possui transação própria por pergunta e captura suas próprias exceções. Logo, acrescentar apenas um `except` externo não prova “vetado, não escreve”. (`app/chamados/reconciliador.py:1433`; `app/chamados/reconciliador.py:1442`; `app/chamados/reconciliador.py:593`; `app/chamados/reconciliador.py:602`)
- **Onde a trilha deveria persistir:** pela porta canônica `core.observ.evento`, que cria `LogAuditoria` em transação própria. Ela precisaria ser chamada depois do rollback da unidade vetada; a ação/payload específicos de veto não estão definidos no trecho vivo examinado. (`app/core/observ.py:94`; `app/core/observ.py:96`; `app/core/observ.py:133`; `app/core/observ.py:134`; definição específica: **[nao verificado]**)
- **Não reutilizar literalmente `registrar_retencao` para competência:** ela só distingue `irma_orfa` do restante; qualquer outro motivo é gravado como `retratacao_retida_justificativa_alheia`, com descrição de justificativa pendente. Isso produziria trilha falsa. (`app/chamados/reconciliador.py:467`; `app/chamados/reconciliador.py:468`; `app/chamados/reconciliador.py:473`; `app/chamados/reconciliador.py:483`)

**Motivo concreto da devolução:** identificar a base que contém a cerca mencionada e explicitar a tradução do veto para trilha após rollback. Sem esse código, afirmar que a mesma porta já permite o comportamento pedido é **[nao verificado]**; o callback e a porta de auditoria existentes não bastam para garantir atomicidade do satélite. (`app/chamados/signals.py:365`; `app/chamados/reconciliador.py:1433`; `app/core/observ.py:134`)

## 17/11 15:32 · sessao c5c10b54ae88 · - · - · origem=Ronald (tmux)

- **FILA — correção factual na triagem, sem reabrir o item 5:** “Posto/Colaborador” está errado. Os dois chamadores são de **Colaborador**: admissão em `app/colaboradores/views.py:1113`–1118, com construtor confirmado em `app/colaboradores/services/colaborador.py:209`–212; edição em `app/colaboradores/views.py:1259`–1285. Nenhum desses dois recebe `DecisaoPergunta`.

- **NAO PROCEDE — item 3:** a montagem indireta está expressamente documentada em `app/chamados/tests/test_f2_resposta_mantem_validacao.py:7`; o detector só reconhece atributo `RESPOSTA` ou literal `'resposta'` em `:32`–34, aplicado em `:51`. Os quatro montadores e valores constam de `:21`–25 e das implementações `app/chamados/services/disputa_emissao.py:1525`, `:1679`, `:1701` e `app/chamados/models.py:4128`. A escrita genérica `cls(**ln)` usa os dicionários recebidos em `app/chamados/models.py:4149`; a via de backfill monta RESPOSTA pela função coberta em `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`. O `ato` variável de reabertura está limitado a `ATOS_COM_FOTO` em `app/chamados/services/disputa_emissao.py:1620` e usado em `:1633`.

- **NAO PROCEDE — comparação foto_e4b:** antes/depois ocorrem na mesma execução e transação, em `app/chamados/management/commands/backfill_fio_morto_e4b.py:280`, `:303`, `:319`–321. A foto realmente inclui todos os campos concretos (`app/chamados/services/foto_e4b.py:251`) de `DecisaoPergunta` (`:280`), mas os dois lados usam esse mesmo modelo.

- **NAO PROCEDE — admin:** `app/core/admin.py:50` enumera campos genericamente; `:74` o aplica apenas a `self.model`. A busca por `DecisaoPergunta` em `*admin*.py` só encontrou um arquivo de teste, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:85`; nenhum registro nominal em produção. Registro dinâmico sob outro nome: **[nao verificado]**.

- **Limite da medida “describe/values()”:** não há arquivo:linha de `describe` identificado no anexo, e a busca de `describe(` em Python/shell não encontrou ocorrência: **[nao verificado]** para essa parte específica. Exemplos `values()` efetivamente comparados na mesma execução estão em `app/chamados/tests/test_e4b_c4_orm.py:315`, `:334` e `:390`, `:434`; isso confirma esses exemplos, não uma lista original ausente.

## 17/11 15:29 · sessao 0365105362ad · E4C-F2 · verificador · origem=Code

**BLOQUEIA o P1 pelo inventário pessoal:** a coluna `resposta_mantem_validacao` aciona um selo existente e não foi inventariada (`app/chamados/models.py:4064`; `bin/sombra_regras_pessoais.py:163`; `bin/inventario_pessoal_dos_modelos.py:78`). Os demais achados estão discriminados abaixo.

1. **BLOQUEIA — coluna nova derruba o selo de inventário.**

   O padrão casa qualquer nome contendo `resposta`; a cobertura de `DecisaoPergunta` contém `ato_texto`/`ato_motivo`, e a isenção existente cobre `pergunta_id`, sem a coluna nova (`bin/sombra_regras_pessoais.py:142`; `:163`; `:233`). `chamados` pertence ao universo inspecionado (`app/config/settings/saas.py:38`).

   O inventário percorre `concrete_fields`, acusa campos descobertos e sai com erro; o selo de host exige seu resultado OK (`bin/inventario_pessoal_dos_modelos.py:73`; `:78`; `:87`; `bin/tests/test_inventario_pessoal_no_commit.sh:31`). O selo da sombra também recusará a coluna após ela aparecer no catálogo (`bin/sombra_selo_pessoal.py:30`; `:34`; `:94`; `bin/sombra.sh:275`).

   **Cura necessária:** declarar a coluna em `ISENTOS`, com o motivo de ser booleano de estado; essa é a correção indicada pelo próprio inventário (`bin/inventario_pessoal_dos_modelos.py:91`). Execução integral desses selos: **[não verificado]**.

2. **NAO PROCEDE — escritor de RESPOSTA omitido ou valor declarado divergente do padrão de motivo.**

   Os quatro montadores encontrados declaram o campo:

   | Montador | Valor | Correspondência com `ato_motivo` |
   |---|---:|---|
   | Porta humana | `False` | Motivo omitido, cujo default é vazio (`app/chamados/services/disputa_emissao.py:1525`; `:1527`; `app/chamados/models.py:4043`). |
   | Resposta automática | `True` | Motivo constante preenchido (`app/chamados/services/disputa_emissao.py:1679`; `:1680`; `:1681`). |
   | Carimbo pela trilha | `True` | Prefixo preenchido, mesmo quando o argumento `motivo` for vazio (`app/chamados/services/disputa_emissao.py:1701`; `:1702`; `:1703`). |
   | Foto da projeção sem linha | `True` | Motivo constante preenchido (`app/chamados/models.py:4106`; `:4128`; `:4129`; `:4130`). |

   Todos desembocam em `registrar`, cujo `bulk_create` materializa os dicionários (`app/chamados/models.py:4139`; `:4149`). A montagem com `ato` variável está limitada a REABERTURA/RECUSA pela guarda de `ATOS_COM_FOTO`; não constitui quinto escritor de RESPOSTA (`app/chamados/services/disputa_emissao.py:1620`; `:1633`; `app/chamados/models.py:4036`).

   Nas migrações anteriores, a transformação do acervo atribui REABERTURA; o `RunPython` correspondente atualiza chaves de `PerguntaDisputa`, sem criar RESPOSTA (`app/chamados/migrations/0051_decisao_pergunta.py:22`; `:49`; `:61`; `:101`). `backfill_decisoes_pergunta` usa o montador para medir RESPOSTA, enquanto sua escrita própria identificada é SILENCIO (`app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `:324`).

3. **FILA — o tripwire cobre montadores explícitos, sem fechar fluxo indireto.**

   Ele reconhece literalmente `'resposta'` ou `.RESPOSTA`, percorre apenas `app/` e exclui migrações; não acompanha `ato` variável, expansão `**kwargs`, SQL ou alterações posteriores de dicionários (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:20`; `:32`; `:48`; `:51`; `:65`).

   Estas vias existentes ficam fora da inspeção **do local chamador**, embora deleguem aos montadores cobertos:

   - Ressuscitação e nascimento com assinaturas: `app/chamados/models.py:1613`; `:3129`.
   - Preservação anterior às respostas humana/automática: `app/chamados/services/disputa_emissao.py:1480`; `:1674`.
   - Validação e devoluções: `app/chamados/services/validacao.py:167`; `:272`; `:345`.
   - Backfill efetivamente escritor: `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.
   - Command pela porta da trilha: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.
   - Entradas humanas: `app/chamados/views.py:1283`; `app/api/views.py:2627`; `app/chamados/services/acoes_disputa.py:373`; `app/chamados/services/declaracao_texto.py:419`; `:693`.
   - Entrada automática: `app/api/foto_recebida.py:48`.

   Portanto, essas vias não são escritores adicionais omitindo o campo; são pontos cegos de acompanhamento do selo, cujo reconhecimento termina na montagem explícita (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:51`; `app/chamados/models.py:4149`).

4. **NAO PROCEDE para leitor de negócio antecipado; FILA para a afirmação absoluta “ninguém lê”.**

   `da_ressuscitacao` nasce `False`; seu leitor de negócio é `es_da_ressuscitacao`, chamado pela carga. Não há escritor de produção atribuindo `True` nos montadores encontrados; o teste fabrica essa situação (`app/chamados/models.py:4058`; `:4096`; `app/chamados/services/tarefa_do_dia.py:427`; `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:79`).

   `resposta_mantem_validacao` é escrito pelos quatro montadores acima. Não encontrei julgamento de negócio usando seu valor; `_pela_decisao` continua decidindo RESPOSTA pelo ato e texto (`app/chamados/services/tarefa_do_dia.py:158`; `:159`; `:160`; `:162`). **Existem leitores genéricos**, além dos checks do banco, de modo que “nenhum leitor” precisa significar “nenhum leitor de negócio” (`app/chamados/models.py:4082`; `:4085`; `app/chamados/services/foto_e4b.py:251`).

5. **FILA — leitores genéricos mudam a representação das linhas.**

   | Caminho | Efeito |
   |---|---|
   | Foto E4B | Ambas as colunas entram em `campos`, pois enumera todos os campos concretos e inclui `DecisaoPergunta` (`app/chamados/services/foto_e4b.py:251`; `:280`). |
   | Reversão E4B | O arquivo passa a carregar essas colunas nas fotos (`app/chamados/management/commands/backfill_fio_morto_e4b.py:284`; `:287`). |
   | Comparação E4B | Compara dicionários inteiros; foto base versus foto P1 diferirá mesmo com ambos os valores `False` (`app/chamados/services/foto_e4b.py:118`; `:121`). |
   | Inventário de modelos/catalogação | A coluna de resposta muda o resultado para reprovação, conforme achado 1 (`bin/inventario_pessoal_dos_modelos.py:73`; `bin/sombra_selo_pessoal.py:30`). |
   | `describe model` | Passa a listar os dois campos e tipos (`app/core/management/commands/describe.py:55`; `:65`). |
   | `values()` no teste existente | A foto ganha as colunas, mas compara dentro da mesma execução (`app/chamados/tests/test_e1_entradas.py:279`; `:295`). |

   No command E4B real, antes/depois são produzidos no mesmo processo e transação; não foi identificado caminho que compare uma foto antiga recuperada contra uma nova após deploy (`app/chamados/management/commands/backfill_fio_morto_e4b.py:280`; `:303`; `:319`). **Golden, hash persistido ou comparação efetivamente atravessando deploy: [não verificado].**

   O helper de admin enumera `_meta.fields`, mas registro de `DecisaoPergunta` nele: **[não verificado]** (`app/core/admin.py:50`). Uso desse modelo em `model_to_dict`, serializers ou exportação adicional: **[não verificado]**. O helper de reexibição também enumera campos, mas uso com `DecisaoPergunta`: **[não verificado]** (`app/core/reexibir.py:30`).

6. **NAO PROCEDE — leitor de último ato escapando ao filtro da carga.**

   Os dois sítios encontrados que decidem pelo último ato recebem o mapa filtrado:

   | Sítio | Decisão | Caminho |
   |---|---|---|
   | `_pela_decisao` | Última REABERTURA/RECUSA devolve ao colaborador (`app/chamados/services/tarefa_do_dia.py:158`; `:163`). | Recebe `_dec` da carga (`:427`; `:430`; `:588`; `:589`). |
   | `_tarefa_pura` | Última RETRATAÇÃO exclui a pergunta (`app/chamados/services/tarefa_do_dia.py:259`; `:260`). | Mesmo mapa filtrado (`:588`; `:589`). |

   A chamada pública sem carga cria essa mesma carga; portanto não abre desvio pelo caminho avulso (`app/chamados/services/tarefa_do_dia.py:745`; `:746`).

   Os outros usos decisórios de REABERTURA/`ATOS_COM_FOTO` são o predicado da marca, a guarda do escritor e as constraints, sem escolher último ato por outra sequência (`app/chamados/models.py:4096`; `app/chamados/services/disputa_emissao.py:1620`; `app/chamados/models.py:4077`; `:4082`).

7. **FILA/P2 — o histórico lê REABERTURA por fora da carga.**

   `respostas_anteriores` percorre `self.decisoes.all()`; qualquer `ATOS_COM_FOTO` entra no histórico e limpa `vigente` (`app/chamados/models.py:3675`; `:3680`; `:3682`). Usa prefetch próprio, sem filtro da marca, ou query própria quando não há cache (`app/chamados/services/fio.py:263`; `app/chamados/models.py:3675`). O wrapper delega a esse leitor (`app/chamados/services/disputa_emissao.py:2126`).

   Uma REABERTURA marcada do P2 aparecerá como “Reaberta para correcao”, inclusive com motivo, porque o template usa o choice geral (`app/chamados/models.py:4031`; `app/templates/chamados/partials/modal_fio.html:205`; `:212`). **Mudança de saída demonstrável; incorreção semântica do histórico: [não verificado].** Ressuscitar efetivamente limpa a resposta, e o histórico declara mostrar respostas que deixaram de ser vigentes (`app/chamados/models.py:3668`; `:1629`).

   O outro leitor desse prefetch, `rotulo_bug_par_103`, reage apenas a DEVOLUCAO com prefixo próprio; a REABERTURA marcada não satisfaz essa condição (`app/chamados/services/materializacao.py:275`).

8. **NAO PROCEDE para falha das constraints no P1; FILA para valor transitório e pré-requisito do P2.**

   As colunas nascem com `default=False` e `db_default=False`. Como cada check aceita incondicionalmente o respectivo campo `False`, nenhuma linha existente será recusada **por esses dois checks** (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:19`; `:24`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:20`; `:25`).

   INSERT antigo omitindo as colunas recebe `False`; UPDATE antigo que as preserve também não viola esses checks. Reescritas normais de decisões já são recusadas pelo modelo/queryset (`app/chamados/models.py:3996`; `:4002`; `:4098`).

   **Há valor semanticamente incompleto:** resposta automática/trilha/projeção gravada por processo anterior ao P1 recebe `resposta_mantem_validacao=False`, embora pertença ao grupo que o P1 marca `True`. Isso é aceito nesta expansão e exige o backfill posterior, já previsto com a condição de não sobreviver processo anterior ao P1 (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:3`; `:4`; `:5`). Drenagem real desses processos: **[não verificado]**.

9. **FILA/P2 — preservar validação ainda não orienta a tarefa deste hash.**

   Para sequência `VALIDACAO → RESPOSTA(texto, resposta_mantem_validacao=True)`, este código escolhe a RESPOSTA e retorna `aguarda_validacao`; o filtro só remove REABERTURA marcada (`app/chamados/services/tarefa_do_dia.py:158`; `:162`; `:427`).

   Esse comportamento já existe com os escritores automáticos/sintéticos do próprio P1; não é regressão introduzida exclusivamente pelo futuro P2 (`app/chamados/services/disputa_emissao.py:1681`; `:1703`; `app/chamados/models.py:4130`). A conservação desse resultado entre leitores P1/P2 precisa ser conferida no diff P2; seu resultado final é **[não verificado]**.

   Também não confere como implementação a frase “a regra de hoje deriva de `ato_motivo`”: os valores dos quatro escritores coincidem com esse padrão, mas a tarefa não consulta motivo; a projeção temporal lê os campos da pergunta (`app/chamados/models.py:4062`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/juizes.py:1389`). O leitor decisório de motivo localizado trata DEVOLUCAO BUG-PAR (`app/chamados/services/materializacao.py:275`).

10. **NAO PROCEDE para testes decorativos; FILA para limites da prova.** [nao verificado]

   - **Default:** o teste omite ambas as colunas no INSERT, exige `(False, False)` e confere o catálogo. Retirar `db_default` da migração atinge diretamente essa prova (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:25`; `:32`; `:35`).
   - **Tripwire:** exige os quatro locais/valores e contém fonte sintética com omissão, valor não constante e formas distintas de montagem. Morde sintaxe e constantes, sem provar o comportamento completo dos escritores (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:76`; `:80`; `:82`).
   - **Filtro:** recria carga, exige uma instância da pergunta e compara a razão antes/depois; a REABERTURA comum deve continuar devolvendo ao colaborador (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:73`; `:75`; `:88`; `:95`). Sem o filtro, o ramo alcançado retorna `devolvida_ao_colaborador` e reprova a asserção (`app/chamados/services/tarefa_do_dia.py:163`). **O teste compara a razão, não a tarefa inteira** (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:76`; `:89`).

   A análise em memória confirmou o censo AST e a diferença do ramo com/sem filtragem. Execução integral Django, fixture ORM e mutante integral sem filtro: **[não verificado]**.

11. **NAO PROCEDE — duplicação de juiz, novo escritor, relógio ou inversão de trava no trecho alterado.**

   O predicado novo identifica origem da linha; não responde às perguntas de C19 ou C21, cujas autoridades permanecem registradas (`app/chamados/models.py:4096`; `app/core/juizes.py:159`; `:170`; `:304`; `:306`). A escrita continua em `registrar`; a carga usa `agora` recebido, exigido pela API (`app/chamados/models.py:4149`; `app/chamados/services/tarefa_do_dia.py:429`; `:743`). Os acréscimos não introduzem aquisição de trava antes/depois da escrita existente (`app/chamados/services/disputa_emissao.py:1520`; `:1525`; `:1677`).

   Na semântica, `da_ressuscitacao` significa origem do compensador, não dono da tarefa; `resposta_mantem_validacao=True` significa que aquela RESPOSTA não revoga validação, não que existe validação vigente (`app/chamados/models.py:4056`; `:4059`). `ato`, texto, motivo e instante conservam seus papéis nos montadores; a foto sintética usa o instante original da resposta (`app/chamados/models.py:4020`; `:4128`; `app/chamados/services/disputa_emissao.py:1525`; `:1680`). A ressalva semântica transitória do estoque está no achado 8.

12. **NAO PROCEDE — contrariedade demonstrada às leis anexadas neste P1.**

   O filtro preserva a interpretação anterior da tarefa diante do compensador, sem criar outra autoridade de dono/porta; isso é compatível com L-129 e o contrato do C19 (`app/docs/LEIS.md:149`; `app/docs/CORTES.md:430`; `app/chamados/services/tarefa_do_dia.py:427`). A limpeza integral do par exigida pela L-130 continua pela porta existente (`app/docs/LEIS.md:151`; `app/chamados/models.py:1629`). As novas migrações expandem o esquema sem antecipar backfill/leitores, conforme a conservação descrita no corte F2 (`app/docs/CORTES.md:488`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:1`).

   Cumprimento operacional dos pré-requisitos de pouso, execução de `migrate`, defaults reais e estado dos processos: **[não verificado]**. [nao verificado]

## 17/11 15:29 · sessao 5e3d30c858e3 · FIDELIDADE-BATIDA-X-ESPELHO · sonda · origem=Code

Os números abaixo são transcritos da saída fornecida; a confirmação deles em arquivo local está **[não verificado]**. Nas tabelas, **C/P = contagem de batidas / pessoas distintas**.

| Superfície | Classe | Total C/P | Empresa 2 C/P | Empresa 3 C/P | Empresa 4 C/P | Até 3 exemplos: pessoa + data | Referência |
|---|---|---:|---:|---:|---:|---|---|
| ESPELHO_ATE_HOJE | **a — não aparece em nenhum dia** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| ESPELHO_ATE_HOJE | **b — mais de um dia** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| ESPELHO_ATE_HOJE | **c — dia errado** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| ESPELHO_ATE_HOJE | **d — impressa não apurável sem procedência** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| ESPELHO_ATE_HOJE | **e — impressa como órfã** | NA | NA | NA | NA | — | [não verificado] |
| PDF_REAL | **a — não aparece em nenhum dia** | **135/75** | 100/56 | 31/17 | 4/2 | col44 + 2026-10-21; col59 + 2026-10-21; col59 + 2026-10-21¹ | [não verificado] |
| PDF_REAL | **b — mais de um dia** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| PDF_REAL | **c — dia errado** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| PDF_REAL | **d — impressa não apurável sem procedência** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| PDF_REAL | **e — impressa como órfã** | **1.136/204** | 896/164 | 238/39 | 2/1 | col43 + 2026-11-02; col43 + 2026-11-02; col44 + 2026-11-09² | [não verificado] |

¹ São três batidas distintas: PKs **132891, 132730, 132746**, respectivamente. [não verificado]  
² São três batidas distintas: PKs **143768, 143890, 150459**, respectivamente. [não verificado]

A classe **e** usa o atributo `orfa` atribuído pelo coletor do PDF; por isso, NA no espelho não significa zero. [`app/relatorios/pdf_espelho.py:609`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:609)

A borda pedida — saída cross-meia-noite do último dia de cada recorte — ficou assim na saída fornecida. [não verificado]

| Superfície | borda_a C/P | borda_elegiveis C/P | borda_a / borda_elegiveis | Empresa 2 | Empresa 3 | Empresa 4 | Referência |
|---|---:|---:|---|---|---|---|---|
| ESPELHO_ATE_HOJE | 0/0 | 0/0 | **0/0 — sem elegíveis** | 0/0 | 0/0 | 0/0 | [não verificado] |
| PDF_REAL | 0/0 | 85/85 | **0/85** | 0/64 | 0/17 | 0/4 | [não verificado] |

Assim, **nenhuma das 135 batidas de (a) no PDF foi classificada como essa borda**. [não verificado]

As janelas declaradas são iguais para as empresas **2, 3 e 4**, na competência **11/2026**. [não verificado]

| Recorte | Início | Fim usado | Fim da competência | Referência |
|---|---|---|---|---|
| ESPELHO_ATE_HOJE | 2026-10-21 | 2026-11-17 | 2026-11-20 | [não verificado] |
| PDF_REAL | 2026-10-21 | 2026-11-16 | 2026-11-20 | [não verificado] |

O instante declarado foi **2026-11-17T15:18:26.061216-03:00**. [não verificado] O teto do PDF em ontem está implementado no coletor. [`app/relatorios/pdf_espelho.py:287`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:287)

A cobertura e os controles declarados foram:

| Indicador | Resultado da saída | Referência |
|---|---|---|
| Frota / medidos | **545/545**; empresa 2: **424**, empresa 3: **101**, empresa 4: **20** | [não verificado] |
| Falhas | **falhas=0**; `FALHAS_POR_EMPRESA {}` | [não verificado] |
| COBERTURA | **COMPLETA** | [não verificado] |
| Tabelas antes/depois | **103/103** | [não verificado] |
| CONTAGENS_IGUAIS | **True** | [não verificado] |
| ROLLBACK_FINAL | **OK** | [não verificado] |

Para as classes positivas, os limites são:

- **(a), PDF_REAL:** a saída acusa 135 PKs esperados ausentes de todos os dias retornados por essa superfície. Não permite afirmar apagamento do banco, causa da ausência, ausência em outros documentos/períodos ou enquadramento dos 135 em `sem_turno`; isso permanece **[não verificado]**. Pelo limite declarado no item1.r, a medição observa estruturas consumidas pelo desenho, sem inspecionar bytes renderizados. **[não verificado]**
- **(e), PDF_REAL:** os 1.136 casos representam o atributo `orfa` observado pela sonda. **[não verificado]** Não certificam, por identidade de PK, que cada batida seja efetivamente órfã: o coletor atribui esse atributo comparando **HH:MM**, enquanto a identidade do mapa de batidas é o **PK**. [`app/relatorios/pdf_espelho.py:609`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:609); [`app/ponto/turnos.py:2916`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/ponto/turnos.py:2916). A presença efetiva do asterisco nos bytes de cada PDF permanece **[não verificado]**; o desenho prevê a impressão das batidas sinalizadas com `*HH:MM`. [`app/relatorios/pdf_espelho.py:885`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:885)

Os zeros também respeitam o alcance dos predicados: **c=0** não certifica dia de turno para batidas sem turno, diagnóstico que a saída separa; **d=0** verifica o campo de procedência, sem certificar sua legibilidade integral no documento. **[não verificado]** O PDF corta as observações agregadas em 120 caracteres. [`app/relatorios/pdf_espelho.py:974`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:974)

Por fim, `falhas=0` não elimina a limitação de exceções internas silenciosas declarada no item1.r: a leitura de `DiaPago` pode capturar uma exceção e seguir com um dicionário vazio. [`app/ponto/services/espelho.py:409`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/ponto/services/espelho.py:409) `CONTAGENS_IGUAIS=True` informa igualdade das contagens, sem demonstrar igualdade integral do conteúdo. **[não verificado]**

## 17/11 15:28 · sessao 6a420b646c9b · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — novo campo quebra inventário pessoal.** `resposta_mantem_validacao` nasce em `app/chamados/models.py:4064`; o padrão considera qualquer nome contendo `resposta` pessoal (`bin/sombra_regras_pessoais.py:163`), mas sua cobertura para DecisaoPergunta declara somente `ato_texto`/`ato_motivo` (`bin/sombra_regras_pessoais.py:142`) e a isenção existente é apenas a FK (`bin/sombra_regras_pessoais.py:233`). O inventário percorre todos os campos concretos, acusa coluna descoberta e sai 1 (`bin/inventario_pessoal_dos_modelos.py:73`, `bin/inventario_pessoal_dos_modelos.py:78`, `bin/inventario_pessoal_dos_modelos.py:87`). O selo host transforma a falta de OK em reprovação (`bin/tests/test_inventario_pessoal_no_commit.sh:31`). O selo da sombra também encontra a coluna pelo catálogo e recusa (`bin/sombra_selo_pessoal.py:30`, `bin/sombra_selo_pessoal.py:34`, `bin/sombra_selo_pessoal.py:94`), interrompendo o refazer (`bin/sombra.sh:275`). `chamados` está no universo tenant (`app/config/settings/saas.py:38`). Confirmei por AST/regex em memória que o nome casa e não consta em ISENTOS; execução do selo host/DB [não verificado].

2. **FILA — a frase “nenhum leitor lê resposta_mantem_validacao” precisa delimitar leitores de negócio.** A fotografia E4B enumera todos os campos concretos (`app/chamados/services/foto_e4b.py:251`) e inclui DecisaoPergunta (`app/chamados/services/foto_e4b.py:280`): ambas as colunas entram na saída e no arquivo de reversão (`app/chamados/management/commands/backfill_fio_morto_e4b.py:284`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:287`). A comparação de fotos considera igualdade integral dos dicionários (`app/chamados/services/foto_e4b.py:116`). Uma foto anterior ao deploy comparada a outra posterior produziria diferenças de esquema; caminho que efetivamente recupere foto antiga para essa comparação [não verificado]. No command real, antes/depois são fotografados pelo mesmo processo, dentro do atomic (`app/chamados/management/commands/backfill_fio_morto_e4b.py:280`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:303`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:319`); a releitura do arquivo é imediata e compara contra o conteúdo recém-gerado (`app/chamados/management/commands/backfill_fio_morto_e4b.py:56`). Hash ou golden persistido cruzando deploy nesse caminho [não verificado].

3. **NAO PROCEDE — outros genéricos, com escopo.** `describe model DecisaoPergunta` passa a listar os dois nomes/tipos (`app/core/management/commands/describe.py:43`, `app/core/management/commands/describe.py:55`, `app/core/management/commands/describe.py:65`). A foto de teste que chama `values()` sem projeção também ganha ambos os campos, mas compara antes/depois na mesma execução (`app/chamados/tests/test_e1_entradas.py:279`, `app/chamados/tests/test_e1_entradas.py:295`). O genérico de formulário `core/reexibir.py` enumera campos (`app/core/reexibir.py:30`), porém os chamadores encontrados são de Colaborador/Posto (`app/colaboradores/views.py:1114`, `app/colaboradores/views.py:1279`); uso com DecisaoPergunta [não verificado]. O mixin admin é genérico (`app/core/admin.py:50`), mas registro de DecisaoPergunta no admin [não verificado] — `app/chamados/admin.py` está vazio. Uso de `model_to_dict`, `ModelSerializer` ou `serializers.serialize` com DecisaoPergunta [não verificado]; não encontrei esses mecanismos na busca pelos `.py` da cópia.

4. **NAO PROCEDE para selo vazio; execução integral [não verificado].** Executei sem Django/DB, via AST em memória e `python3 -B`, os dois métodos do tripwire substituindo somente a base por `unittest.TestCase`: passaram o censo real de quatro escritores e a fonte sintética (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:76`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:82`). O selo exclui migrations/tests e só reconhece ato sintaticamente literal/atributo RESPOSTA (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:32`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:51`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:65`): indiretos e SQL continuam fora.

5. **NAO PROCEDE quanto ao ramo do RED do filtro; fixture ORM [não verificado].** A fixture declara RESPOSTA antes da REABERTURA (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:67`, `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:79`), recria carga a cada leitura e exige uma instância da pergunta (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:73`). O ramo real retorna `aguarda_validacao` para RESPOSTA textual, e `devolvida_ao_colaborador` para última REABERTURA (`app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:163`). Extraí e executei esse corpo por AST com insumos mínimos: antes = `aguarda_validacao`; marcada sem filtro = `devolvida_ao_colaborador`; marcada filtrada = `aguarda_validacao`. Assim, o mutante alcançado pelo ramo reprova as asserções (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:88`). A execução da fixture inteira e do mutante sem filtro no ORM [não verificado]. O teste compara apenas a razão, não a tarefa inteira (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:76`, `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:89`).

6. **NAO PROCEDE para RED de default decorativo; execução DB [não verificado].** O teste insere omitindo ambas as colunas e exige retorno `(False, False)` (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:25`, `app/chamados/tests/test_f2_db_default_das_colunas_novas.py:32`), além de conferir os defaults no catálogo (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:35`). Remover somente o default Python não faria esse teste vermelho; remover `db_default` da migração retiraria justamente o mecanismo que o INSERT omitido exige (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:18`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:19`). Sem DB, resultado efetivo [não verificado].

## 17/11 15:27 · sessao 6b961a0c2650 · - · - · origem=Ronald (tmux)

**NAO PROCEDE — escritor RESPOSTA omitido ou com valor divergente do padrão de motivo.** Os quatro montadores efetivos declaram:

| Montador | Campo | Motivo |
|---|---:|---|
| `app/chamados/services/disputa_emissao.py:1525` | False, linha 1527 | Omitido; default `''` em `app/chamados/models.py:4043` |
| `app/chamados/services/disputa_emissao.py:1679` | True, linha 1681 | Texto constante, linha 1680 |
| `app/chamados/services/disputa_emissao.py:1701` | True, linha 1703 | Prefixo constante `"pela trilha: "`, linha 1702 |
| `app/chamados/models.py:4128` | True, linha 4130 | Constante definida na linha 4106, usada na linha 4129 |

Todos desembocam no único `bulk_create`, em `app/chamados/models.py:4149`; as atualizações em lote são recusadas em `app/chamados/models.py:3996`, `app/chamados/models.py:4002`, e o `save` de linha existente em `app/chamados/models.py:4098`.

**FILA — premissa documental sobre “regra de hoje deriva de ato_motivo” não confere como implementação.** O único leitor decisório de `ato_motivo` localizado usa **DEVOLUCAO**, não RESPOSTA, em `app/chamados/services/materializacao.py:275`; a outra leitura é exibição em `app/templates/chamados/partials/modal_fio.html:212`. A projeção temporal lê os campos da própria pergunta, sem decisões ou motivo, em `app/chamados/juizes.py:1389`. Logo, os quatro valores novos reproduzem o **padrão de preenchimento do motivo** dos escritores; não localizei juiz atual que derive esse booleano de motivo. A promessa contrária está em `app/chamados/models.py:4062` e `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:3`.

**NAO PROCEDE — montagem indireta com `ato` variável produzindo RESPOSTA.** A montagem variável existente em `app/chamados/services/disputa_emissao.py:1633` aceita apenas `ATOS_COM_FOTO`, mediante guarda na linha 1620; o conjunto é `(REABERTURA, RECUSA)` em `app/chamados/models.py:4036`.

**FILA — alcance real do tripwire.** O scanner só reconhece o literal `'resposta'` ou atributo `.RESPOSTA` (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:32`, linha 51), ignora migrações e só percorre `app/` (linhas 20, 64–66). Não acompanha fluxo por variáveis, `**kwargs`, mutação posterior de dicionário nem o conteúdo recebido em `registrar` (`app/chamados/models.py:4139`). Os chamadores indiretos existentes, portanto, ficam fora da análise do próprio local de chamada, embora usem o montador coberto:

- Ressuscitação e nascimento com assinaturas: `app/chamados/models.py:1613`, `app/chamados/models.py:3129`.
- Preservação antes de resposta humana/automática: `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1674`.
- Validação e devoluções: `app/chamados/services/validacao.py:167`, `app/chamados/services/validacao.py:272`, `app/chamados/services/validacao.py:345`.
- Backfill efetivamente escritor: `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.
- Entrada de command pela porta da trilha: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.
- Entradas humanas: `app/chamados/views.py:1283`, `app/api/views.py:2627`, `app/chamados/services/acoes_disputa.py:373`, `app/chamados/services/declaracao_texto.py:419`, `app/chamados/services/declaracao_texto.py:693`.
- Entrada automática: `app/api/foto_recebida.py:48`.

O `backfill_decisoes_pergunta` **mede** RESPOSTA com o montador em `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; a escrita própria localizada é SILENCIO, linhas 324–327. Não acrescentar um quinto montador RESPOSTA por causa dessa medição. A migração anterior cria `RespostaAnterior` em `app/chamados/migrations/0050_resposta_anterior.py:17`; a 0051 transforma o acervo em REABERTURA (`app/chamados/migrations/0051_decisao_pergunta.py:49`, linha 61), e seu `RunPython` apenas preenche chaves de PerguntaDisputa (linhas 22–27 e 101).

**NAO PROCEDE — rejeição de linhas anteriores pelas duas constraints novas.** Ambas as colunas nascem False (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:19`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:20`), satisfazendo incondicionalmente o segundo braço dos respectivos checks (linhas 24 e 25). INSERT velho omitindo as colunas recebe False pelo `db_default` desses mesmos locais; UPDATE velho que preserve essas colunas também não viola tais checks.

**FILA — valor semântico transitório e pré-requisito do P2.** Processo anterior ao P1 ainda escreve False para RESPOSTA automática/trilha/projeção, mesmo com motivo preenchido, porque omite o campo; isso é aceito pelo check da 0055 e previsto pelo adiamento do backfill (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:3`). O P2 precisa garantir que nenhum desses processos sobreviva ao backfill, exigência já escrita nas linhas 4–5; drenagem efetiva é **[não verificado]**.

**Validação:** executei somente o extrato puro das funções AST do tripwire, em memória e com `python3 -B`; ele retornou exatamente os quatro locais e valores da tabela. Testes Django, defaults reais do catálogo, execução de migrate e inspeção da base `fbc5c76ce` são **[não verificado]**; a cópia não contém `.git`, e não usei banco nem logs.

## 17/11 15:27 · sessao e7821b5c428f · - · - · origem=Ronald (tmux)

Censo de leitores concluído, somente por código.

1. **NÃO PROCEDE — o filtro cobre os dois sítios que decidem pelo último ato de `DecisaoPergunta`.** A carga faz query própria ordenada por pergunta/instante/pk e exclui `es_da_ressuscitacao(d)` antes de alimentar `_dec` (`app/chamados/services/tarefa_do_dia.py:421`, `:427`, `:430`). Esse mapa chega a `_tarefa_pura` em `:588`, `:589`; os dois julgamentos do último ato são:
   - `_pela_decisao`: `ult = decisoes[-1]`, e REABERTURA/RECUSA viram `devolvida_ao_colaborador` (`app/chamados/services/tarefa_do_dia.py:158`, `:163`, `:164`). Chamado em `:273` e `:334`.
   - Exclusão de pergunta cujo último ato é RETRATAÇÃO (`app/chamados/services/tarefa_do_dia.py:259`, `:260`).
   
   A entrada pública sem carga também cria essa carga (`app/chamados/services/tarefa_do_dia.py:745`, `:746`). Portanto ambos recebem a sequência filtrada, inclusive na janela P2 (`app/chamados/services/tarefa_do_dia.py:427`, `:588`).

2. **FILA/P2 — há leitor de REABERTURA fora da carga, mas não decide fila nem último ato: histórico da resposta.** `PerguntaDisputa.respostas_anteriores` percorre `self.decisoes.all()` e, para qualquer `ATOS_COM_FOTO`, acrescenta a decisão ao histórico e limpa `vigente`; `ATOS_COM_FOTO` continua incluindo REABERTURA (`app/chamados/models.py:3675`, `:3680`, `:3681`, `:3682`, `:4036`). Esse caminho usa `Prefetch('decisoes')` próprio, sem filtro de marca (`app/chamados/services/fio.py:263`, `:264`), ou query própria quando não há cache (`app/chamados/models.py:3675`). O wrapper delega ao mesmo leitor (`app/chamados/services/disputa_emissao.py:2122`, `:2126`). O modal publica o histórico (`app/templates/chamados/partials/modal_fio.html:205`, `:208`, `:212`).
   
   Quando P2 gravar uma REABERTURA marcada, ela **entrará nesse histórico**, e o rótulo será “Reaberta para correcao”, pois o template usa `get_ato_display` e o choice permanece esse (`app/templates/chamados/partials/modal_fio.html:212`; `app/chamados/models.py:4031`). Isso é mudança de saída verificável pelo código. Ser semanticamente errado é **[não verificado]**: o próprio contrato do leitor diz que expõe respostas que deixaram de ser vigentes (`app/chamados/models.py:3668`), e ressuscitar zera a resposta (`app/chamados/models.py:1629`, `:1630`). Não confundir esse histórico com desvio do dono da fila.

3. **NÃO PROCEDE — demais usos de REABERTURA/ATOS_COM_FOTO não são leitores de sequência que decidam fila.**
   - `es_da_ressuscitacao`: testa ato REABERTURA e a coluna (`app/chamados/models.py:4096`).
   - A porta `reabrir_para_correcao` valida o parâmetro `ato` contra `ATOS_COM_FOTO`, antes de escrever; não lê uma linha P2 (`app/chamados/services/disputa_emissao.py:1620`, `:1621`, `:1633`).
   - Constraints de fotos e marca restringem atos admissíveis; não julgam sequência (`app/chamados/models.py:4077`, `:4082`).
   - `linhas_da_projecao_sem_linha` consulta apenas RESPOSTA/VALIDAÇÃO por correspondência de ato e instante; a REABERTURA marcada não entra nessa consulta (`app/chamados/models.py:4122`, `:4123`, `:4126`, `:4131`).
   - `rotulo_bug_par_103` percorre o mesmo prefetch, mas só reage a DEVOLUÇÃO com prefixo de motivo; REABERTURA marcada não satisfaz (`app/chamados/services/materializacao.py:275`, `:276`).
   - Backfill consulta existência de SILÊNCIO/VALIDAÇÃO, não último ato nem REABERTURA (`app/chamados/management/commands/backfill_decisoes_pergunta.py:315`).

4. **FILA/P2, sem regressão nova demonstrada — último ato RESPOSTA ignora `resposta_mantem_validacao`.** Toda RESPOSTA com texto é `aguarda_validacao`, independentemente do novo booleano (`app/chamados/services/tarefa_do_dia.py:159`, `:160`, `:162`). A carga só remove REABERTURA marcada (`app/chamados/services/tarefa_do_dia.py:427`, `:428`). Logo uma sequência VALIDACAO seguida de RESPOSTA com `resposta_mantem_validacao=True` continua caindo em `aguarda_validacao` neste hash (`app/chamados/services/tarefa_do_dia.py:158`, `:162`). Isso já existe para os escritores automáticos e sintéticos, que neste P1 gravam True (`app/chamados/services/disputa_emissao.py:1681`, `:1703`; `app/chamados/models.py:4130`); atribuir o erro especificamente à futura janela P2 exige o contrato final dos leitores P2, **[não verificado]**. Não achei leitor de sequência derivando esse significado de `ato_motivo` neste hash; a tarefa sequer consulta motivo em `_pela_decisao` (`app/chamados/services/tarefa_do_dia.py:142`, `:187`).

5. **NÃO PROCEDE — novo predicado não duplica os juízes registrados C19/C21.** C19 responde ações/dono/porta e é registrado em `tarefa_do_dia`; C21 responde par instante+texto e é registrado em `pergunta_respondida` (`app/core/juizes.py:159`, `:170`, `:304`, `:306`). O novo predicado só identifica a marca da linha (`app/chamados/models.py:4096`), e o filtro atua dentro da carga do próprio C19 (`app/chamados/services/tarefa_do_dia.py:427`). Não há novo relógio no trecho alterado: compara `ato_em` ao `agora` recebido (`app/chamados/services/tarefa_do_dia.py:429`); a API exige `agora` e valida igualdade com a carga (`app/chamados/services/tarefa_do_dia.py:743`, `:747`).

6. **Leitor genérico confirmado, para consolidar com o outro explorador:** a foto E4B incorpora todos os campos concretos (`app/chamados/services/foto_e4b.py:251`) e fotografa explicitamente `DecisaoPergunta` (`:280`); ambas as colunas passam a compor `campos` por esse caminho. A comparação entre fotos considera desigualdade de dicionários inteiros (`app/chamados/services/foto_e4b.py:118`, `:121`), portanto foto antes com código base versus foto depois com P1 muda mesmo com False nos dois campos. Execução concreta atravessando deploy/golden existente é **[não verificado]**.

Não usei banco nem executei testes com banco; resultados de execução e existência real de processos antigos são **[não verificado]**. [nao verificado]

## 17/11 15:18 · sessao b5989c47cce4 · E4C-F2 · censo · origem=Code

Nos anexos colados, conto a primeira linha de cada anexo como 1, incluindo linhas vazias. As referências à cópia usam seus próprios números de linha.

**1 — Escritor antigo `responder_automaticamente`: (b), com ressalva sobre “descartou”.** O Codex viu exatamente o defeito: “RESPOSTA automática ou pela trilha [...] inserida pelo código antigo após o backfill, omitindo a coluna. Ela recebe `False`, e a leitura nova desfaz a validação”. A triagem respondeu “PROCEDENTE, FORA DA CONDICAO 1, sobe ao coordenador” e “Nao aplicado (escopo do aval literal: condicao 1)”. Portanto, houve exclusão da cura por escopo, não rejeição técnica do achado. (`v5.r:25`; `triagem_v5.txt:6–10`.) [nao verificado]

O escritor antigo está confirmado: grava RESPOSTA com motivo sem declarar a coluna nova. ([disputa_emissao.py:1678](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/services/disputa_emissao.py:1678).)

**1 — Escritor antigo `carimbar_resposta_pela_trilha`: (b), na mesma acepção.** O trecho “RESPOSTA automática ou pela trilha” identifica também este escritor; a triagem aceita o defeito e deixa a correção fora do escopo autorizado. O código antigo confirma a RESPOSTA com motivo e sem a coluna. (`v5.r:25`; `triagem_v5.txt:6–10`; [disputa_emissao.py:1699](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/services/disputa_emissao.py:1699).)

Há uma ressalva factual ao laudo: a cópia descreve `curar_perguntas_em_limbo` como “cura de acervo que roda UMA VEZ”, enquanto a linha 1482 nomeia outro comando. Seu agendamento como cron e sua execução durante a janela são **[nao verificado]**. ([crons.py:1476](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/config/crons.py:1476); `app/config/crons.py:1477`; `app/config/crons.py:1482`.)

**1 — Escritor antigo da foto da projeção: (a).** O tema estava presente: “automática, carimbo pela trilha e foto da projeção escrevem `True`”. A fábrica e seus backfills também estavam no censo anterior. Entretanto, o contraexemplo temporal do v5 nomeia apenas automática e trilha. Assim, “TERCEIRO nao citado pelo Codex” deve ser restringido a **não identificado como terceiro escritor antigo afetado pela janela**; ele foi citado como montador. (`v5.r:22,25`; `v2.r:25,29`; `opus_final_c8f5.txt:4–5`.)

A cópia confirma a fábrica antiga produzindo RESPOSTA com `ato_motivo`, sem a coluna nova. ([models.py:4100](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/models.py:4100); `app/chamados/models.py:4101`.)

Como complemento comum aos três escritores, a correção sugerida pela triagem era executar novamente o backfill “DEPOIS do HUP”; porém o deploy documenta que comandos por `docker exec`, incluindo cron em curso, não recebem esse sinal. Esse prolongamento da janela não foi explicitado no contraexemplo do v5. (`triagem_v5.txt:9`; `bin/deploy.sh:329–330`; `v5.r:25`.)

**2 — Janela inversa merge→migrate: (a) apenas quanto ao mecanismo concreto do cron.** A incompatibilidade geral **já foi vista**: “Código novo executado contra banco sem 0055 é incompatível, pois já exige a coluna”. Portanto, seria incorreto classificá-la integralmente como despercebida. O trecho não identifica o cron que nasce após o merge e antes da migration; é essa especificação que o opus acrescenta. (`v2.r:90`; `opus_final_c8f5.txt:13`.)

Descarte dessa janela por alguma triagem fornecida: **[nao verificado]**. A expressão “classe da casa, nao bloqueia” pertence ao próprio opus, não a uma triagem do Codex. (`opus_final_c8f5.txt:13`.)

**3 — Condição documental aberta: (e), provisória.** O suporte literal da pendência é “SEM ato de devolucao” e “lei NAO decidida”; o laudo registra ainda zero ocorrências dos dois cortes em `CORTES.md/json`. ([LEIS.md:151](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/docs/LEIS.md:151); `opus_final_c8f5.txt:14–15`.)

Não identifiquei nas respostas fornecidas uma análise dessa pendência documental; que ela tenha sido perguntada é **[nao verificado]**. **Sem as `.q`, é impossível distinguir com segurança (a) de (e).** A triagem mencionar o corte como justificativa funcional não comprova uma pergunta sobre sua documentação. (`triagem_v1.txt:18–20`; conteúdo das perguntas: **[nao verificado]**.)

**4 — Decisão por prefixo de `ato_motivo` em materialização: (a).** O Codex descreveu o comportamento, mas não o denunciou como contradição: “O leitor decisório de motivo [...] restringe-se a **DEVOLUCAO BUG-PAR**” e “permanece o reconhecimento textual de DEVOLUCAO BUG-PAR, fora dessa cura”. (`v2.r:41,91`.)

A cópia confirma literalmente `.startswith(MOTIVO_BUG_PAR_103)`. A triagem “8-11: conferencia, sem defeito apontado” não documenta descarte de uma acusação desse defeito. ([materializacao.py:275](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/services/materializacao.py:275); `triagem_v1.txt:22`.)

- Resposta automática antiga → **(b), excluída da cura por escopo**. (`triagem_v5.txt:6–10`)
- Carimbo antigo pela trilha → **(b), excluído da cura por escopo**. (`triagem_v5.txt:6–10`)
- Foto antiga da projeção → **(a)**. (`v5.r:22,25`)
- Janela inversa → **(a) parcial: mecanismo do cron; incompatibilidade já vista**. (`v2.r:90`; `opus_final_c8f5.txt:13`)
- Pendência documental → **(e) provisória; (a)/(e) indistinguíveis sem `.q`**. [nao verificado]
- Prefixo de motivo na materialização → **(a)**. (`v2.r:41,91`)

## 17/11 15:17 · sessao 46f631bd2ab0 · FIDELIDADE-BATIDA-X-ESPELHO · leitura · origem=Code

**A) Não localizei um contador de frota que verifique o contrato completo por identidade de batida.** A inexistência absoluta fora dos caminhos inspecionados permanece [não verificado]. Os instrumentos encontrados verificam fixtures, minutos ou resumos, como detalhado abaixo. `app/relatorios/tests/test_cartao_nao_perde_batida.py:57`; `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:78`; `app/folha/porta_export.py:496`.

A cadeia real preserva objetos `Batida` em `dia['batidas']`: o espelho agrupa por `pk → dia`, o coletor do PDF consome esses dias e acrescenta `b.orfa`, e o desenho imprime tanto as normais quanto as órfãs. A identidade correta é **PK**, pois timestamps podem colidir. `app/ponto/services/espelho.py:450`; `app/ponto/services/espelho.py:574`; `app/ponto/services/espelho.py:764`; `app/relatorios/pdf_espelho.py:596`; `app/relatorios/pdf_espelho.py:604`; `app/relatorios/pdf_espelho.py:876`; `app/ponto/turnos.py:2916`.

| Instrumento | Universo, exclusões e esperado | Quem chama; placar; o que não cobre |
|---|---|---|
| `CartaoNaoPerdeBatidaTest` | Uma pessoa, três turnos noturnos e uma espúria; somente os PKs plantados na fixture. Espera faltantes vazios e nenhuma repetição. `app/relatorios/tests/test_cartao_nao_perde_batida.py:24`; `:39`; `:57`; `:67`. | Os testes chamam `_coletar_dados_espelho`. Não consultam `batidas_apuraveis`, não varrem frota nem comparam o dia de cada PK com o juiz. `app/relatorios/tests/test_cartao_nao_perde_batida.py:58`; `:61`; `:68`; `:71`. Vínculo nominal desse teste com item do placar: [não verificado]. |
| `pdf_x_espelho_divergentes` | Empresas em geral, mas somente pessoas classificadas como `entra` no TXT; classificação que falha e pessoa não encontrada são puladas. Compara **minutos e aberto por dia**, esperando zero divergentes. `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:56`; `:78`; `:97`; `:114`. | Comando manual, declarado como não cron; chama coletor do PDF e `espelho_do_colab`. Está em **E4-pdf-x-espelho**. Não compara PKs, completude, duplicação ou procedência. `app/config/crons.py:1139`; `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:73`; `:92`; `app/core/espelho_verdade.py:124`. |
| `selo_leitores_no_mesmo_numero` → `folha.porta_export.medir` | Empresas ativas; a comparação tela/PDF considera `classificar_export(... status='entra')`. Compara cinco escalares do resumo, tolerância `0.01`; o selo espera zero divergências. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`; `app/folha/porta_export.py:469`; `:496`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:178`. | O comando chama `medir` **e lavra**. É selo de porta/manual, relacionado ao **E4** e fonte do **R4 estrutural**. Uma perda compartilhada pelos leitores pode conservar os escalares: o predicado não contém identidade de batida. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88`; `app/config/crons.py:887`; `app/core/espelho_verdade.py:120`; `app/core/placar_estrutural.py:414`; `app/folha/porta_export.py:497`. |
| `e6_oraculo` | Empresas ativas, pessoas com `FechamentoMensal`; opcionalmente só TXT. Exclui isenção/ausência, falta de lavratura, zero dos dois lados e diferença explicada pela janela. Compara minutos, aceitando diferença até dez minutos. `app/ponto/management/commands/e6_oraculo.py:303`; `:339`; `:365`. | Lê apuráveis, mas usa pareamento independente; não certifica cada PK impresso contra `turnos_do_colab`. Relaciona-se a **E6-sistema-bate** e **E6-batida-impar**. `app/ponto/management/commands/e6_oraculo.py:8`; `app/core/espelho_verdade.py:180`; `:188`. |
| `esmeril_espelho` | Pessoas em operação, excluindo isentos; janela da competência mais antiga não entregue até ontem. Conta assinaturas recorrentes; A5 usa órfãs da ata com repetição mínima. `app/ponto/services/esmeril_espelho.py:38`; `:125`; `:139`; `:206`. | Há cron declarado com `--apply`. Seus insumos são células, turnos e ata; não compara `dia['batidas']` com o universo apurável. `app/config/crons.py:640`; `app/ponto/services/esmeril_espelho.py:95`. Item E1–E6/R específico dessa fidelidade: [não verificado]. |

Portanto, **E4/R4 são os mais próximos na frota; o teste `CartaoNaoPerdeBatidaTest` é o mais próximo por PK**. O R4 enumera comparações entre leitores e valores, sem declarar o par “apuráveis × marcações impressas”. `app/core/placar_estrutural.py:211`; `app/relatorios/tests/test_cartao_nao_perde_batida.py:61`.

**B) Script proposto, sem execução no banco. [não verificado]**

Há dois recortes necessários: `ESPELHO_ATE_HOJE` mede o espelho solicitado até hoje; `PDF_REAL` respeita o teto em ontem do coletor. A classe **e** pertence a `PDF_REAL`, pois esse coletor atribui `orfa`. Assim, batidas de hoje não são acusadas como perdas do PDF por causa do teto previsto. `app/ponto/services/espelho.py:1194`; `app/relatorios/pdf_espelho.py:285`; `:598`; `:604`.

A janela vem de `janela_competencia(empresa, 11, 2026)`; “ativa” significa `situacao='ativo'`. O universo apurável inclui pendentes de DP e exclui retratadas; o script não exclui isentos nem exige entrada no TXT. `app/folha/export.py:47`; `app/colaboradores/models.py:296`; `:323`; `app/ponto/turnos.py:2871`.

| Classe | Identificação usada |
|---|---|
| a | `b.pk` esperado ausente de **todos os dias retornados** da pessoa. Universo por `batidas_apuraveis` e dia por `dia_das_batidas`, incluindo as não reclamadas por turno. `app/ponto/turnos.py:2868`; `:2898`; `:2957`. |
| b | Mesmo `b.pk` em mais de um `dia['data']`. Há diagnóstico adicional para repetição no mesmo dia. `app/ponto/services/espelho.py:764`; `app/ponto/turnos.py:2916`. |
| c | `b.pk` em `t.batidas`, comparando `dia['data']` com `t.data_turno`. Batida sem turno é publicada separadamente, sem inventar dia de turno. `app/ponto/turnos.py:43`; `:2961`. |
| d | `b.pk` fora de `batidas_apuraveis(colab)` **sem recorte temporal**, e `b.procedencia` vazio. `app/ponto/turnos.py:2879`; `app/ponto/services/espelho.py:755`; `app/relatorios/pdf_espelho.py:951`. |
| e | `b.pk` cujo `b.orfa` foi atribuído pelo coletor real. **A atribuição atual usa HH:MM**, embora a contagem abaixo preserve PK. `app/relatorios/pdf_espelho.py:604`. |
| borda | Subconjunto de a: `t.saida.pk`, com `t.cross_meianoite` e `t.data_turno == fim` daquele recorte. Publica também o denominador elegível. `app/ponto/turnos.py:43`; `app/relatorios/pdf_espelho.py:271`. |

O script mede as estruturas consumidas pelo desenho; não inspeciona bytes renderizados. Em particular, d verifica o campo de procedência, enquanto o PDF limita o texto agregado de observações a 120 caracteres. As células sintéticas de isenção são impressas por outro campo e não entram como `Batida`. `app/relatorios/pdf_espelho.py:887`; `:951`; `:974`.

O código abaixo propõe transação PostgreSQL somente leitura, savepoint por pessoa, contagens antes/depois no mesmo snapshot e rollback deliberado. Captura exceções propagadas, erros SQL e avisos registrados; exceções internas sem log, como a leitura degradada de `DiaPago`, continuam sendo uma limitação observável do código da casa. **Validação em execução: [não verificado].** `app/ponto/services/espelho.py:409`.

```python
# Proposta para colar como um bloco no manage.py shell.
# Execucao e resultados no tenant: [nao verificado].
# Nenhuma funcao geradora de PDF, lavra ou comando --apply e chamada.

import logging
from collections import Counter, defaultdict
from datetime import date

_incidentes = set()
_handle_original = logging.Logger.handle


def _log_sem_arquivo(self, registro):
    # Somente memoria; nao encaminha mensagens/PII aos handlers.
    if registro.levelno >= logging.WARNING:
        exc = registro.exc_info
        classe = exc[0].__name__ if exc and exc[0] else (
            "Log" + registro.levelname
        )
        _incidentes.add(classe)


class _RollbackFinal(Exception):
    pass


class _LeituraDegradada(Exception):
    pass


def executar_fidelidade():
    from django.db import connection, transaction
    from django.utils import timezone
    from django_tenants.utils import schema_context
    from colaboradores.models import Colaborador, Empresa
    from folha.export import janela_competencia
    from ponto.turnos import (
        batidas_apuraveis, turnos_do_colab, dia_das_batidas,
    )
    from ponto.services.espelho import espelho_do_colab
    from relatorios.pdf_espelho import _coletar_dados_espelho
    from relatorios.cartao_pela_celula import ate_ontem

    empresas_ids = (2, 3, 4)
    agora = timezone.now()
    hoje = timezone.localtime(agora).date()
    falhas = set()
    falhas_empresa = Counter()
    medidos = Counter()
    totais = defaultdict(lambda: {
        "contagem": 0, "pessoas": set(),
        "empresa": Counter(), "pessoas_empresa": defaultdict(set),
        "exemplos": [],
    })
    classes = (
        "a", "b", "c", "d", "e", "borda_a",
        "universo_apuravel", "impressas",
        "sem_turno", "juiz_multidia",
        "repetida_mesmo_dia", "borda_elegiveis",
    )

    def vigiar_sql(execute, sql, params, many, context):
        try:
            return execute(sql, params, many, context)
        except Exception as erro:
            _incidentes.add(type(erro).__name__)
            raise

    def contar_tabelas(rotulo):
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT table_name
                  FROM information_schema.tables
                 WHERE table_schema = %s
                   AND table_type = 'BASE TABLE'
                 ORDER BY table_name
            """, ["juliani"])
            tabelas = [r[0] for r in cursor.fetchall()]
            if not tabelas:
                raise RuntimeError("SchemaSemTabelas")
            q = connection.ops.quote_name
            contagens = {}
            for tabela in tabelas:
                cursor.execute(
                    "SELECT COUNT(*) FROM %s.%s"
                    % (q("juliani"), q(tabela))
                )
                contagens[tabela] = cursor.fetchone()[0]
        print("TABELAS_%s quantidade=%d" % (rotulo, len(contagens)))
        for tabela, quantidade in contagens.items():
            print("%s juliani.%s=%d" % (rotulo, tabela, quantidade))
        return contagens

    def medir_pessoa(colab, ini, fim, dias, apuraveis, pdf=False):
        # Universo de apresentacao: autoridade real, sem copiar seu
        # tratamento de orfas/espurias/borda.
        # app/ponto/turnos.py:2898,2957
        turnos = turnos_do_colab(colab, ini, fim)
        mapa = dia_das_batidas(colab, ini, fim, turnos=turnos)

        esperadas = {
            pk: b for pk, b in apuraveis.items()
            if b.timestamp <= agora
            and ini <= mapa.get(pk, date.min) <= fim
        }

        juiz = defaultdict(set)
        bordas = set()
        for turno in turnos:
            for b in turno.batidas:
                juiz[b.pk].add(turno.data_turno)
            if (
                turno.data_turno == fim
                and turno.cross_meianoite
                and turno.saida is not None
                and turno.saida.pk in esperadas
            ):
                bordas.add(turno.saida.pk)

        # Nao recorta antes de indexar: batida presente em outro dia
        # retornado deve entrar em c, em vez de virar falsa ausencia.
        impressas = defaultdict(list)
        objetos = {}
        for dia in dias:
            for b in dia["batidas"]:
                if b.pk is None:
                    raise ValueError("MarcacaoSemPK")
                impressas[b.pk].append(dia["data"])
                objetos.setdefault(b.pk, b)

        alvo = set(esperadas) | {
            pk for pk, datas in impressas.items()
            if any(ini <= d <= fim for d in datas)
        }
        achados = {classe: {} for classe in classes}

        def incluir(classe, pk, dia, b):
            achados[classe][pk] = (
                dia, timezone.localtime(b.timestamp).isoformat()
            )

        faltantes = set(esperadas) - set(impressas)
        for pk, b in esperadas.items():
            incluir("universo_apuravel", pk, mapa[pk], b)
            if pk in faltantes:
                incluir("a", pk, mapa[pk], b)

        for pk in bordas:
            incluir("borda_elegiveis", pk, fim, esperadas[pk])
            if pk in faltantes:
                incluir("borda_a", pk, fim, esperadas[pk])

        for pk in sorted(alvo & set(impressas)):
            b = objetos[pk]
            datas = impressas[pk]
            dias_distintos = set(datas)
            primeiro_dia = min(datas)
            incluir("impressas", pk, primeiro_dia, b)

            if len(dias_distintos) > 1:
                incluir("b", pk, primeiro_dia, b)
            if any(n > 1 for n in Counter(datas).values()):
                incluir("repetida_mesmo_dia", pk, primeiro_dia, b)

            dias_juiz = juiz.get(pk, set())
            if not dias_juiz:
                incluir("sem_turno", pk, primeiro_dia, b)
            else:
                if len(dias_juiz) > 1:
                    incluir("juiz_multidia", pk, primeiro_dia, b)
                errados = [
                    d for d in datas
                    if any(d != esperado for esperado in dias_juiz)
                ]
                if errados:
                    incluir("c", pk, min(errados), b)

            if pk not in apuraveis and not getattr(b, "procedencia", None):
                incluir("d", pk, primeiro_dia, b)

            if pdf:
                # Usa o atributo posto pelo coletor; nao refaz HH:MM.
                # app/relatorios/pdf_espelho.py:604-609
                if not hasattr(b, "orfa"):
                    raise AttributeError("ColetorSemFlagOrfa")
                if b.orfa:
                    incluir("e", pk, primeiro_dia, b)

        return achados

    def acumular(superficie, colab, achados):
        for classe, casos in achados.items():
            if superficie == "ESPELHO_ATE_HOJE" and classe == "e":
                continue
            total = totais[(superficie, classe)]
            total["contagem"] += len(casos)
            if casos:
                total["pessoas"].add(colab.pk)
                total["empresa"][colab.empresa_id] += len(casos)
                total["pessoas_empresa"][colab.empresa_id].add(colab.pk)
            for pk, (dia, instante) in sorted(casos.items()):
                if len(total["exemplos"]) >= 3:
                    break
                total["exemplos"].append(
                    "col%s %s batida=%s instante=%s"
                    % (colab.pk, dia.isoformat(), pk, instante)
                )

    if connection.in_atomic_block:
        raise RuntimeError("ExigeShellForaDeTransacao")

    with schema_context("juliani"):
        try:
            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL "
                        "REPEATABLE READ, READ ONLY"
                    )
                    cursor.execute("SELECT current_schema()")
                    if cursor.fetchone()[0] != "juliani":
                        raise RuntimeError("SchemaIncorreto")

                antes = contar_tabelas("ANTES")
                try:
                    empresas = Empresa.objects.in_bulk(empresas_ids)
                    if set(empresas) != set(empresas_ids):
                        raise LookupError("EmpresaSolicitadaAusente")

                    janelas = {}
                    for eid in empresas_ids:
                        ini, fim_competencia = janela_competencia(
                            empresas[eid], 11, 2026
                        )
                        fim = min(fim_competencia, hoje)
                        if fim < ini:
                            raise ValueError("CompetenciaAindaNaoIniciada")
                        fim_pdf = max(ini, ate_ontem(fim, hoje=hoje))
                        janelas[eid] = (ini, fim, fim_pdf)
                        print(
                            "JANELA emp%s competencia=11/2026 "
                            "inicio=%s fim_competencia=%s "
                            "espelho_ate=%s pdf_ate=%s"
                            % (eid, ini, fim_competencia, fim, fim_pdf)
                        )

                    frota = list(
                        Colaborador.objects.filter(
                            empresa_id__in=empresas_ids,
                            situacao="ativo",
                        ).select_related("empresa").order_by("pk")
                    )
                    frota_empresa = Counter(c.empresa_id for c in frota)
                    print("INSTANTE", timezone.localtime(agora).isoformat())
                    print("FROTA", len(frota), dict(frota_empresa))
                    if not frota:
                        raise ValueError("UniversoVazio")

                    for colab in frota:
                        _incidentes.clear()
                        try:
                            # Savepoint: erro SQL de uma pessoa nao
                            # inutiliza a transacao das demais.
                            with transaction.atomic():
                                with connection.execute_wrapper(vigiar_sql):
                                    ini, fim, fim_pdf = janelas[colab.empresa_id]

                                    # Vida inteira para d nao confundir
                                    # fora da janela com nao apuravel.
                                    # app/ponto/turnos.py:2879
                                    apuraveis = {
                                        b.pk: b
                                        for b in batidas_apuraveis(colab)
                                    }

                                    espelho = espelho_do_colab(
                                        colab, hoje, ini, fim,
                                        ano=2026, agora=agora,
                                    )
                                    resultado_espelho = medir_pessoa(
                                        colab, ini, fim,
                                        espelho["dias"], apuraveis,
                                    )

                                    # O coletor chama o montador real e
                                    # atribui o flag exato usado no papel.
                                    # app/relatorios/pdf_espelho.py:596,604
                                    pdf = _coletar_dados_espelho(
                                        colab, ini, fim, agora=agora
                                    )
                                    resultado_pdf = medir_pessoa(
                                        colab, ini, fim_pdf,
                                        pdf["dias"], apuraveis, pdf=True,
                                    )

                                    if _incidentes:
                                        raise _LeituraDegradada()

                            # So publica a pessoa depois das duas leituras.
                            acumular(
                                "ESPELHO_ATE_HOJE", colab, resultado_espelho
                            )
                            acumular("PDF_REAL", colab, resultado_pdf)
                            medidos[colab.empresa_id] += 1

                        except Exception as erro:
                            falhas.add(colab.pk)
                            falhas_empresa[colab.empresa_id] += 1
                            print(
                                "ERRO col%s emp%s classe=%s internas=%s"
                                % (
                                    colab.pk, colab.empresa_id,
                                    type(erro).__name__,
                                    ",".join(sorted(_incidentes)) or "-",
                                )
                            )
                            

[truncado em 20000 de 22503 caracteres]

_6632 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
