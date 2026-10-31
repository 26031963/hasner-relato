# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 09:27** · publicado em **31/10/2026 09:30** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 09:27 · sessao f29f24b0d110 · DISJUNTOR-B3-TOPO · verificador · origem=Code

**REPROVADO.** Há caminhos que liberam o Stop antes da cobrança B3 e uma colisão entre IDs admitidos pelo próprio contador (`bin/hook_stop_fila1.py:1224`, `bin/hook_stop_fila1.py:1231`, `bin/hook_stop_fila1.py:1298`; `bin/disjuntor.py:483`).

**(1) Onde o desenho falha**

- **ESPERANDO verdadeiro contorna a cobrança.** A ordem relevante é PAREI → ESPERANDO com prova → teto → B2 → B3/recados. Portanto, pergunta ausente ou na terceira linha não é cobrada quando uma saída anterior libera. `_libera()` encerra o processo (`bin/hook_stop_fila1.py:1224`, `:1229`, `:1282`, `:1293`, `:1298`, `:133`).
- **O selo não exercita essa espera verdadeira.** Os quatro casos novos usam `pid=1`; esse PID está sempre entre os ancestrais e é recusado como prova. Eles chegam à cobrança B3 pelo caminho “ESPERANDO sem prova” (`bin/tests/test_disjuntor.sh:335`, `:337`, `:339`, `:341`; `bin/hook_stop_fila1.py:477`, `:529`).
- **IDs hifenados semelhantes estão protegidos, mas IDs com cauda não.** A regex rejeita `PRE-OBRA-X` e `OBRA-X-OUTRA` para `OBRA-X`. Contudo, aceita `DESENHO O9 (C)` ou `DESENHO O9 NOTURNO-FOO` como pergunta de `O9`, embora `_obra_de()` produza três identidades distintas. Confirmei esses resultados em memória (`bin/hook_stop_fila1.py:414`–`:415`; `bin/disjuntor.py:81`–`:85`, `:483`–`:484`).
- **“Duas primeiras úteis” tem uma restrição adicional:** depois da primeira linha capturada, uma linha vazia encerra o bloco. `ESPERANDO`, branco, pergunta resulta em somente uma linha lida; pelo contrato literal de duas linhas úteis, a segunda pergunta deveria contar (`bin/hook_stop_fila1.py:430`–`:436`).
- **A validação é apenas lexical:** exige a substring maiúscula `DESENHO` e o ID na mesma linha. Aceita `REDESENHO OBRA-X` e `OBRA-X NAO TEM PERGUNTA DE DESENHO`, sem verificar QUAL PADRÃO/POR QUÊ (`bin/hook_stop_fila1.py:415`, `:1048`–`:1050`).

**(2) Quem escreve e quem lê**

| Dado | Escritor | Leitor |
|---|---|---|
| Pergunta no topo | O construtor, conforme instrução emitida pelo PreToolUse; no selo, `_topo()` grava a fixture (`bin/disjuntor.py:919`–`:921`; `bin/tests/test_disjuntor.sh:72`). | B3 lê `_bloco_do_topo()` e aplica `_pergunta_b3_escrita()` (`bin/hook_stop_fila1.py:1045`–`:1047`). |
| Primeira linha ESPERANDO/PAREI | Declaração manual; o teto atual apenas registra/libera, tendo abandonado a escrita automática de PAREI (`bin/hook_stop_fila1.py:1283`–`:1291`). | Ambos usam `_linha_do_topo()`, derivada do mesmo bloco (`bin/hook_stop_fila1.py:407`, `:652`, `:1102`). |
| Contador B3 | `varrer_b3()` acumula agentes reprovados por obra, deduplica e dispara no limiar; `_com_estado()` persiste em `logs/sessao/disjuntor_estado.json` (`bin/disjuntor.py:54`, `:533`–`:550`, `:242`–`:245`). | Stop e PreToolUse chamam a varredura; `b3_disparados()` consulta o estado (`bin/hook_stop_fila1.py:1046`; `bin/disjuntor.py:918`, `:566`). |
| Rearme B3 | CLI chama `rearma_b3()`, preserva agentes antigos em `rearmado` e zera contagem/disparo (`bin/disjuntor.py:1138`, `:560`–`:563`). | A varredura ignora esses agentes antigos (`bin/disjuntor.py:535`). |

Escrever a pergunta **cala o recado**, sem rearmar o B3: o quarto verificador continua negado pelo PreToolUse, que decide pela chave da obra sem consultar o topo (`bin/hook_stop_fila1.py:1047`; `bin/disjuntor.py:915`–`:922`).

`_rteto()` restaura outro contador, `logs/hook_stop_fila1.json`; ele não restaura o estado B3 (`bin/tests/test_disjuntor.sh:333`–`:334`; `bin/hook_stop_fila1.py:35`; `bin/disjuntor.py:54`).

**(3) O selo morde cada ramo novo? Não.**

| Comportamento | Cobertura existente |
|---|---|
| Aceitar segunda linha | Caso `:335`–`:336`; mutante `:715`. |
| Rejeitar terceira linha | Caso `:337`–`:338`; mutante `:718`. |
| Rejeitar `OBRA-X-OUTRA` | Caso `:339`–`:340`; mutante `:716`. |
| Exigir `DESENHO` | Caso `:341`–`:342`; mutante `:717`. |

Todas as referências dessa tabela são de `bin/tests/test_disjuntor.sh`.

Há mutantes que preservam os resultados desses casos:

- Retirar **somente o lookbehind** da regex aceita `PRE-OBRA-X`; o selo testa apenas a fronteira direita (`bin/hook_stop_fila1.py:414`; `bin/tests/test_disjuntor.sh:339`).
- Separar os testes em `any(DESENHO)` e `any(id)` aceita `['DESENHO OBRA-Y', 'OBRA-X']`; falta esse caso cruzado (`bin/hook_stop_fila1.py:415`; `bin/tests/test_disjuntor.sh:335`–`:342`).
- Limitar B3 à última linha lida perde a pergunta válida na primeira linha; todos os positivos novos a colocam na segunda (`bin/hook_stop_fila1.py:1045`; `bin/tests/test_disjuntor.sh:335`–`:342`).
- Reverter `return out or None` para `return None` perde a leitura parcial após `OSError`. Esse ramo novo não tem cenário nem mutante declarado; também afeta ESPERANDO/PAREI pelo leitor compartilhado (`bin/hook_stop_fila1.py:439`, `:407`; `bin/tests/test_disjuntor.sh:72`, `:715`–`:718`).

Além disso, o positivo exige somente ausência do recado: uma exceção convertida em `FALHA + None` pode satisfazê-lo. Falta conferir a decisão esperada e ausência de falha (`bin/tests/test_disjuntor.sh:336`; `bin/hook_stop_fila1.py:1057`–`:1062`).

Validei funções isoladas em memória. Não executei o selo, pois ele cria, altera e apaga arquivos; sobrevivência no selo completo: **[nao verificado]** (`bin/tests/test_disjuntor.sh:37`–`:49`, `:637`–`:646`).

## 31/10 09:25 · sessao 47865606b8e9 · - · - · origem=Ronald (tmux)

O selo cobre quatro comportamentos: pergunta na 2ª linha, rejeição na 3ª, rejeição de `OBRA-X-OUTRA` e rejeição de prosa sem `DESENHO`; os respectivos mutantes estão declarados. `bin/tests/test_disjuntor.sh:335`, `:337`, `:339`, `:341`, `:715`, `:716`, `:717`, `:718`.

Há lacunas concretas:

- **Fronteira esquerda:** remover somente `(?<![\w-])` mantém os resultados dos quatro casos atuais, mas aceita `DESENHO PREFIXO-OBRA-X`; o teste existente exercita somente a fronteira direita com `OBRA-X-OUTRA`. `bin/hook_stop_fila1.py:414`; `bin/tests/test_disjuntor.sh:339`.
- **DESENHO e ID na mesma linha:** trocar a expressão por dois `any` independentes mantém os casos atuais, mas aceita `['DESENHO OBRA-Y', 'OBRA-X']`. `bin/hook_stop_fila1.py:415`; `bin/tests/test_disjuntor.sh:335`.
- **Pergunta na primeira linha:** limitar somente o B3 à última linha do bloco passa pelos quatro exemplos atuais, todos com `ESPERANDO` na primeira, mas perde pergunta na primeira com texto neutro na segunda. `bin/hook_stop_fila1.py:1045`; `bin/tests/test_disjuntor.sh:335`, `:337`, `:339`, `:341`.
- **OSError após leitura parcial:** voltar `return out or None` para `return None` desfaz um ramo novo sem cenário correspondente; `_topo` sempre escreve arquivo regular completo. A mudança também afeta PAREI e ESPERANDO porque usam `_linha_do_topo`. `bin/hook_stop_fila1.py:439`, `:407`, `:652`, `:1102`; `bin/tests/test_disjuntor.sh:72`.
- **Exceção confundida com sucesso:** o positivo apenas procura ausência de B3; uma exceção exclusiva do reconhecimento da pergunta vira `FALHA + None` e satisfaz essa asserção. O helper de autorização positiva existe, mas esse caso não o usa. `bin/tests/test_disjuntor.sh:336`, `:91`; `bin/hook_stop_fila1.py:1057`.

Verifiquei os dois primeiros mutantes e o erro parcial executando funções extraídas por AST apenas em memória. Sobrevivência no selo completo: **[nao verificado]**. Executar o selo criaria e apagaria arquivos temporários, contrariando a restrição de leitura. `bin/tests/test_disjuntor.sh:37`, `:39`, `:49`, `:637`, `:646`.

## 31/10 09:25 · sessao 2142652f636b · - · - · origem=Ronald (tmux)

Mapa de produtores e consumidores:

| Dado | Escreve | Lê |
|---|---|---|
| Pergunta de DESENHO no topo | O construtor é o produtor exigido pela mensagem do PreToolUse; não é escrita automaticamente por esse ramo (`bin/disjuntor.py:919-922`). No selo, `_topo` escreve o RELATO fictício (`bin/tests/test_disjuntor.sh:72`). | `_bloco_do_topo` abre RELATO e captura até duas linhas (`bin/hook_stop_fila1.py:428-437`); B3 consulta esse bloco e chama `_pergunta_b3_escrita` (`bin/hook_stop_fila1.py:1045-1047`). |
| Primeira linha: ESPERANDO/PAREI | A declaração do topo é humana; o hook deixou de produzir `PAREI: hook-teto`, conforme comentário do ramo atual (`bin/hook_stop_fila1.py:1283-1291`). | `_linha_do_topo` retorna a primeira linha do mesmo bloco (`bin/hook_stop_fila1.py:407-408`); ESPERANDO lê em `:652-655`; PAREI lê em `:1102-1105`. |
| Contador B3 | `varrer_b3` coleta agentes reprovados (`bin/disjuntor.py:517-525`), acumula por obra e deduplica por `aid` (`:533-538`), marca disparado no limiar (`:540-548`). `_com_estado` grava o JSON atomicamente (`:242-245`), cujo caminho é `logs/sessao/disjuntor_estado.json` (`:54`). | Stop chama `varrer_b3` (`bin/hook_stop_fila1.py:1046`); PreToolUse também varre antes de decidir (`bin/disjuntor.py:915-918`); `b3_disparados` consulta o estado persistido (`:566-567`). |
| Rearme | CLI `--rearma-b3` (`bin/disjuntor.py:1138-1141`) chama `rearma_b3`, arquiva os IDs já contados em `rearmado` e zera lista/disparo (`:553-563`). | A próxima varredura ignora IDs rearmados (`bin/disjuntor.py:535-536`). |

A pergunta escrita cala o recado do Stop, mas não rearma o contador nem permite o quarto verificador: o Stop consulta o topo (`bin/hook_stop_fila1.py:1045-1050`), enquanto PreToolUse nega pela presença da chave da obra na varredura, sem consultar RELATO (`bin/disjuntor.py:915-922`).

Identidade da obra:

- No topo, as duas fronteiras `(?<![\w-])` e `(?![\w-])` impedem que `OBRA-X-OUTRA`, `OUTRA-OBRA-X` ou `_OBRA-X` satisfaçam `OBRA-X` (`bin/hook_stop_fila1.py:414-415`).
- O contador usa a obra extraída como chave exata; a gramática reconhece IDs hifenados inteiros (`bin/disjuntor.py:81-85`, `:470-485`) e PreToolUse usa pertinência exata da chave (`:918`).
- Há um resíduo fora dessa gramática: `obra OBRA-X_OUTRA` é extraída como `OBRA-X`, porque a regex externa não exige fronteira à direita e o `fullmatch` valida apenas o grupo já capturado (`bin/disjuntor.py:84-85`, `:475`). Reprodução feita em memória, sem importar o módulo inteiro ou escrever arquivos; `_` não integra a gramática declarada de IDs (`:81`). 

Os escritores automáticos de alarmes preservam o topo: `no_relato` insere na seção de alarmes ou antes da primeira seção `##`, e depois grava o arquivo (`app/core/esteira_vigia.py:863-884`). Outros leitores possuem janelas diferentes: handoff percorre até a primeira seção (`bin/handoff_sessao.sh:94-108`); alarme de ociosidade procura `PAREI:` nas primeiras 40 linhas físicas (`bin/alarme_sessao_ociosa.py:119-127`).

## 31/10 09:25 · sessao 9083f468370d · - · - · origem=Ronald (tmux)

1. **A cobrança B3 pode ser pulada no caminho real do Stop.** `main()` libera PAREI em `bin/hook_stop_fila1.py:1224` e ESPERANDO com prova em `bin/hook_stop_fila1.py:1229`; só acrescenta B3 ao recado em `bin/hook_stop_fila1.py:1298`. Portanto pergunta ausente ou na terceira linha não produz cobrança quando ESPERANDO tem prova; teto e B2 também podem liberar antes da varredura (`bin/hook_stop_fila1.py:1282`, `bin/hook_stop_fila1.py:1293`).

2. **O selo não cobre ESPERANDO com prova.** Seus quatro casos usam `pid=1` (`bin/tests/test_disjuntor.sh:335`, `bin/tests/test_disjuntor.sh:337`, `bin/tests/test_disjuntor.sh:339`, `bin/tests/test_disjuntor.sh:341`); PID 1 pertence sempre ao conjunto de ancestrais (`bin/hook_stop_fila1.py:477`) e é recusado (`bin/hook_stop_fila1.py:529`). Logo o cenário central “espera real + pergunta fora das duas linhas” permanece descoberto nesses testes (`bin/hook_stop_fila1.py:1230`, `bin/tests/test_disjuntor.sh:335`).

3. **“Duas primeiras úteis” significa duas linhas contíguas após começar.** Branco ou cabeçalho `# RELATO` depois da primeira linha encerra a leitura (`bin/hook_stop_fila1.py:431`). Assim ESPERANDO, branco, pergunta não aceita a pergunta embora ela seja a segunda não vazia; confirmei com execução apenas em memória das funções extraídas por AST (`bin/hook_stop_fila1.py:430`, `bin/hook_stop_fila1.py:435`). Se o contrato exige contiguidade, essa restrição precisa constar dele [nao verificado].

4. **Prefixo e sufixo semelhantes estão corretamente recusados no helper atual.** As duas fronteiras `(?<![\w-])` e `(?![\w-])` recusam tanto `PRE-OBRA-X` quanto `OBRA-X-OUTRA` para obra `OBRA-X` (`bin/hook_stop_fila1.py:414`). O caso novo do selo testa somente `OBRA-X-OUTRA`, portanto cobre a fronteira à direita; não cobre isoladamente a fronteira à esquerda (`bin/tests/test_disjuntor.sh:339`).

5. **Ainda não se verifica que há pergunta de desenho.** A aceitação exige apenas substring maiúscula `DESENHO` e ID inteiro na mesma linha; aceita, por exemplo, `OBRA-X NAO TEM PERGUNTA DE DESENHO` e `REDESENHO OBRA-X`, confirmado em memória (`bin/hook_stop_fila1.py:415`). A mensagem exige QUAL PADRÃO e POR QUE, mas o predicado não verifica esses elementos (`bin/hook_stop_fila1.py:1048`, `bin/hook_stop_fila1.py:415`).

6. **A leitura parcial preserva somente `OSError`.** Depois de ler uma linha, `OSError` devolve o trecho já obtido (`bin/hook_stop_fila1.py:438`); exceção diferente escapa ao helper e é convertida em ausência de recado pelo invólucro (`bin/hook_stop_fila1.py:1057`, `bin/hook_stop_fila1.py:1062`). O leitor abre UTF-8 sem política de substituição (`bin/hook_stop_fila1.py:428`).

Não executei o selo, pois ele escreve o RELATO de teste e restaura/remove o contador (`bin/tests/test_disjuntor.sh:334`, `bin/tests/test_disjuntor.sh:335`).

## 31/10 09:05 · sessao b14d140bc3b4 · E3-STATUS · verificador · origem=Code

**O delta ainda deixa uma falha central: no julgamento normal, `folga_humana` fica sempre `False`, inclusive quando a porta acaba lavrando `indefinida`.** O adaptador recebe `_vd`, que nesse ramo só pode ser `None` ou `cobrado`; a porta deriva o veredito efetivo depois, pelos códigos. Portanto, a exceção nova de feriado/folga humana não funciona nesse caminho. (`app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1245`, `app/ponto/services/cartorio.py:1558`; `app/ponto/status_dia.py:122`; `app/ponto/portas/celula.py:658`, `app/ponto/portas/celula.py:698`.)

Os anexos fornecidos não contêm linhas de células de produção: quantidade de dias afetados e ocorrência dos cenários abaixo em produção **[não verificado]**.

**1. Matriz de saída e divergências**

A tupla solicitada precisa também de **`previsto_min`**. Exemplo alcançável pelo código: ausência parcial sobre folga ou sobre trabalho, sem batida, não isento, `nunca_bateu`, `folga_humana=False`: com previsto zero sai `folga`; com previsto positivo sai `irregular`. A grade permite ausência parcial sobre folga, e o ramo sem adesão força `nunca_bateu` fora da cobertura integral. (`app/escala/utils.py:1170`, `app/escala/utils.py:1190`, `app/escala/utils.py:1743`, `app/escala/utils.py:1751`; `app/ponto/services/cartorio.py:1032`; `app/ponto/status_dia.py:85`, `app/ponto/status_dia.py:93`, `app/ponto/status_dia.py:99`.)

Na tabela, **a primeira linha aplicável vence**. `B` = tem batida; `C` = cobertura parcial; `I` = isento; `H` = folga humana; `N` = previsto diferente de zero. `A` = `{furo, discordante, cobrado, nunca_bateu, fato_em_ausencia}`. Essa partição cobre todas as entradas da função, incluindo todas as alcançáveis. (`app/ponto/status_dia.py:42`, `app/ponto/status_dia.py:83`.)

| B | Tipo da grade | Condição adicional | Status | Prova |
|---|---|---|---|---|
| 0 | `falta` | qualquer | `falta` | `app/ponto/status_dia.py:83` |
| 0 | `ausencia` | não `(C e não I e V ∈ A∪{indefinida})` | `ausencia` | `app/ponto/status_dia.py:85` |
| qualquer | qualquer | `V=indefinida`, H=1 | B=0: `folga`; B=1: `folga_trabalhada` | `app/ponto/status_dia.py:88` |
| qualquer | qualquer | `V=indefinida`, H=0 | `escala_suspeita` | `app/ponto/status_dia.py:91` |
| 0 | qualquer | tipo=`folga` ou N=0 | `folga` | `app/ponto/status_dia.py:93` |
| 0 | qualquer | I=1 | `ok`, rótulo “isento de ponto” | `app/ponto/status_dia.py:95` |
| 0 | qualquer | V=`coberta` | `ausencia` | `app/ponto/status_dia.py:97` |
| 0 | qualquer | V∈A | `irregular` | `app/ponto/status_dia.py:99` |
| 0 | qualquer | restante | `sem_certidao` | `app/ponto/status_dia.py:101` |
| 1 | qualquer | tipo=`folga` ou V=`fato_sem_previsao` | `folga_trabalhada` | `app/ponto/status_dia.py:102` |
| 1 | qualquer | V∈A | `irregular` | `app/ponto/status_dia.py:104` |
| 1 | qualquer | V∈{`concorde`,`coberta`,`cobranca_indevida`} | `ok`, rótulo de isento se I=1 | `app/ponto/status_dia.py:106` |
| 1 | qualquer | restante | `sem_certidao` | `app/ponto/status_dia.py:108` |

Os filtros de alcance são importantes:

- **Normal:** H=0 sempre, pelo defeito de `_vd` descrito acima. O veredito efetivo já está disponível pela autoridade da porta em `_tl.vereditos`, mas esse resultado não alimenta `fatos_do_status`. (`app/ponto/services/cartorio.py:1249`, `app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1560`.)
- **D5:** usa `cel.veredito`; pode obter H=1 para `indefinida`, somente com `--apply`, status nulo e carimbo considerado atualizado. Status já preenchido é ignorado. (`app/ponto/services/cartorio.py:726`, `app/ponto/services/cartorio.py:729`, `app/ponto/services/cartorio.py:733`.)
- **Nunca bateu:** deriva o veredito, mas força `nunca_bateu` quando não há cobertura integral. (`app/ponto/services/cartorio.py:997`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1041`.)
- **Isento:** o juiz retorna sem códigos; `isento + indefinida` não nasce desse julgamento fresco. Uma célula histórica que já tenha `indefinida` pode seguir outro caminho no D5. (`app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:735`.)
- **Admin:** seus vereditos passam por outra regra: `trabalhou→ok`, `faltou→falta`, `justificado→ausencia`; a porta preserva a decisão humana. (`app/ponto/status_dia.py:39`; `app/ponto/portas/celula.py:694`, `app/ponto/portas/celula.py:738`.)

`indefinida` também inclui `FASE_NAO_DECLARADA_COM_PREVISTO` e `PARCIAL_SEM_PERIODO`, além de `PREVISTO_COM_DIA_INDEFINIDO`; pode existir com batida. (`app/escala/models.py:1583`; `app/ponto/supra_juiz.py:350`.)

As divergências demonstráveis são:

| Situação | Lavra | A2/calendário/ADR |
|---|---|---|
| Normal, `indefinida`, A2 reconheceria feriado/folga humana | H permanece falso; em geral `escala_suspeita` | Calendário suprime trabalho pelo feriado/folga humana; contradiz a costura prometida pelo ADR. (`app/ponto/status_dia.py:88`; `app/colaboradores/services/calendario.py:112`; `app/docs/ADR/0002-celula-e-o-dia.md:55`.) |
| Parcial, não isento, sem batida, veredito acusador | `irregular`, `escala_suspeita` ou até `folga` se previsto zero | Calendário retorna ausência/falta imediatamente pela faixa A2, sem receber `cobertura_parcial`. O supra sustenta cobrar o restante, mas a igualdade com o calendário não existe. (`app/ponto/status_dia.py:85`, `app/ponto/status_dia.py:93`; `app/colaboradores/services/calendario.py:109`; `app/ponto/supra_juiz.py:279`.) |
| Ausência aprovada `folga_compensatoria`/`feriado_folga`, sem batida | Grade chama `ausencia`; lavra retorna `ausencia` antes de consultar H | A2 classifica como faixa `folga`; calendário retorna `folga`. Com batida, a lavra pode ainda produzir `irregular` onde o calendário produz `folga_trabalhada`. (`app/escala/utils.py:1173`, `app/escala/utils.py:1180`; `app/ponto/precedencia.py:62`; `app/ponto/status_dia.py:85`, `app/ponto/status_dia.py:104`; `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:146`.) |
| Feriado supressor, grade ainda `trabalho`, veredito diferente de `indefinida` | A nova consulta é dispensada; lavra pode produzir `irregular`/`ok` | Calendário considera feriado para qualquer veredito. A exceção `trabalha_em_feriado` deve ser respeitada: nesse caso A2 pode manter trabalho. (`app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:99`, `app/ponto/status_dia.py:106`; `app/colaboradores/services/calendario.py:112`; `app/ponto/precedencia.py:330`.) |
| Folga só de escala, `indefinida` | `escala_suspeita` | Calendário preserva `folga`/`folga_trabalhada` contra o mapa de veredito quando reconhece que não é trabalho. Essa diferença já vinha da regra anterior ao delta. (`app/ponto/status_dia.py:91`; `app/colaboradores/services/calendario.py:134`, `app/colaboradores/services/calendario.py:146`, `app/colaboradores/services/calendario.py:168`.) |
| Trabalho previsto, não isento, sem batida, `nunca_bateu` | `irregular` | Calendário mapeia para `falta`, quando nenhuma precedência anterior o absorve. Também é diferença anterior ao delta. (`app/ponto/status_dia.py:42`, `app/ponto/status_dia.py:99`; `app/colaboradores/services/calendario.py:56`, `app/colaboradores/services/calendario.py:168`.) |
| Trabalho previsto, não isento, sem batida, `concorde` | `sem_certidao` | Calendário bloqueia a sobreposição por `ok` e conserva sua classificação anterior, conforme cobertura e demais fatos. (`app/ponto/status_dia.py:101`; `app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:161`.) |

A comparação com o calendário exige mais dados que a tupla: faixa A2, efeito da ausência, trabalho reconhecido pela célula, futuro, turno aberto e cobertura também participam. Portanto, não há uma coluna única de “status do calendário” determinada pelos seis valores pedidos. (`app/colaboradores/services/calendario.py:109`, `app/colaboradores/services/calendario.py:119`, `app/colaboradores/services/calendario.py:144`, `app/colaboradores/services/calendario.py:799`.)

**2. Escritores e leitores**

| Valor | Quem produz/escreve | Quem lê no caminho auditado |
|---|---|---|
| `folga_humana` da lavra | Adaptador inicia falso e deriva pela A2 somente quando recebe `indefinida`; é valor em memória. (`app/ponto/status_dia.py:121`, `app/ponto/status_dia.py:126`, `app/ponto/status_dia.py:136`.) | `status_da_lavra`, somente no ramo `indefinida`. (`app/ponto/status_dia.py:89`.) |
| `folga_humana` da tela | Outra expressão, inline. (`app/colaboradores/services/calendario.py:801`.) | `_classificar`, condicionada à faixa `folga`. (`app/colaboradores/services/calendario.py:116`.) |
| `cobertura_parcial` | Grade, pela resposta de `cobre_parte_do_dia`. (`app/escala/utils.py:1138`, `app/escala/utils.py:1150`, `app/escala/utils.py:1190`.) | Supra, cartório, impressão e adaptador/status. (`app/ponto/supra_juiz.py:243`, `app/ponto/supra_juiz.py:279`; `app/ponto/services/cartorio.py:991`, `app/ponto/services/cartorio.py:1144`, `app/ponto/services/cartorio.py:1153`; `app/ponto/status_dia.py:86`, `app/ponto/status_dia.py:136`.) |
| `tipo_dia` da grade | Base `trabalho/folga`; overlays de FolgaDia e ausência aprovada escrevem `folga/falta/ausencia`. (`app/escala/utils.py:1743`, `app/escala/utils.py:1749`, `app/escala/utils.py:1118`, `app/escala/utils.py:1174`, `app/escala/utils.py:1180`.) | Supra, cartório e adaptador/status. (`app/ponto/supra_juiz.py:197`; `app/ponto/services/cartorio.py:997`; `app/ponto/status_dia.py:133`, `app/ponto/status_dia.py:83`.) |
| `veredito=indefinida` | Supra produz códigos; `derivar_veredito` os traduz; porta resolve o veredito efetivo e o grava. (`app/ponto/supra_juiz.py:305`, `app/ponto/supra_juiz.py:312`, `app/ponto/supra_juiz.py:357`; `app/escala/models.py:1583`; `app/ponto/portas/celula.py:658`, `app/ponto/portas/celula.py:770`.) | Adaptador decide consultar A2; regra decide o status; calendário tem seu próprio mapa. (`app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:88`; `app/colaboradores/services/calendario.py:56`.) |
| A2 `faixa` e `tipo_do_dia` | `fatos_do_periodo` monta candidatos, ordena a precedência e escreve o vencedor; sem jurisdição escreve `None`. `fatos_do_dia` apenas delega. (`app/ponto/precedencia.py:209`, `app/ponto/precedencia.py:332`, `app/ponto/precedencia.py:344`, `app/ponto/precedencia.py:349`, `app/ponto/precedencia.py:356`.) | Helper novo, calendário, texto de supressão e projeção da ata. (`app/ponto/precedencia.py:188`, `app/ponto/precedencia.py:192`, `app/ponto/precedencia.py:378`; `app/colaboradores/services/calendario.py:793`; `app/ponto/services/cartorio.py:1672`.) |
| A2 `fatos.folga_lancada` | A2 consulta FolgaDia, filtra calendário de vínculo alheio e escreve o booleano. (`app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:313`, `app/ponto/precedencia.py:365`.) | Helper, calendário e regra de repouso para férias. (`app/ponto/precedencia.py:191`; `app/colaboradores/services/calendario.py:801`; `app/ferias/situacao.py:97`.) |
| `status_dia/status_rotulo` | `_aplicar_status`, dentro da porta; persistência pelo mesmo mecanismo da lavra. (`app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:773`.) | Juiz D4 lê a coluna após tratar encerramento. Calendário continua na regra antiga até E4. (`app/ponto/status_dia.py:158`, `app/ponto/status_dia.py:163`; `app/docs/ADR/0002-celula-e-o-dia.md:73`.) |

Outros consumidores de **`faixa`** incluem reavaliação de feriado, semana do posto, veto de emissão, previsão e diagnóstico. (`app/chamados/services/feriado_reavaliar.py:36`; `app/escala/services/semana_do_posto.py:177`; `app/ponto/management/commands/detectar_ausencias.py:616`; `app/ponto/services/abriu_nao_bateu.py:123`; `app/ponto/services/dia_do_colab.py:130`.)

Outros consumidores de **`tipo_do_dia`** incluem folha sem furo, vigias e palavra do dia. O cartório também persiste uma projeção A2 em `ata['precedencia']`, pela porta. (`app/folha/services/sem_furo.py:26`; `app/ponto/services/faltas_de_hoje.py:61`; `app/ponto/services/deslogados.py:166`; `app/ponto/services/dia_decidido.py:370`; `app/ponto/services/cartorio.py:1671`; `app/ponto/portas/celula.py:819`.)

**3. O helper responde à mesma pergunta do inline?**

**Para o mesmo dict A2, equivale ao efeito combinado das duas condições do calendário**, isto é:

`faixa=feriado OU (faixa=folga E (folga_lancada OU subtipo de folga))`.

Não equivale ao booleano inline isolado: feriado sem FolgaDia dá helper verdadeiro e booleano inline falso, mas o calendário trata feriado separadamente. (`app/ponto/precedencia.py:187`; `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:801`.)

**A alimentação, porém, não é igual.** O caminho novo chama `fatos_do_dia` sem parâmetros e consulta FolgaDia de todos os vínculos da pessoa. Quando o vínculo ativo cobre a janela, o calendário injeta apenas suas datas de folga, transformadas em `True`. (`app/ponto/status_dia.py:126`; `app/ponto/precedencia.py:209`, `app/ponto/precedencia.py:234`; `app/colaboradores/services/calendario.py:593`, `app/colaboradores/services/calendario.py:604`.)

Há dois contraexemplos por construção; ocorrência em produção **[não verificado]**:

| Mesmo colaborador/data | A2 sem parâmetros | A2 alimentada pelo calendário |
|---|---|---|
| Folga **manual** no vínculo antigo B; ativo A cobre a janela; célula de trabalho por A; sem ausência/feriado | Encontra a folga de B e a preserva: H verdadeiro | Só recebeu datas de A; pode responder trabalho/H falso. (`app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:313`, `app/ponto/precedencia.py:337`; `app/colaboradores/services/calendario.py:607`; `app/escala/alimentacao.py:66`.) |
| Folga **de calendário** no ativo A; célula gerada pelo vínculo B | Descarta a folga de A quando o vínculo selecionado é B | Recebe `True`, que perdeu origem e vínculo; a verificação de propriedade não consegue descartá-la. Pode responder folga/H verdadeiro. (`app/ponto/precedencia.py:305`, `app/ponto/precedencia.py:313`; `app/colaboradores/services/calendario.py:607`.) |

Mesmo com insumos iguais, o status final pode diferir: calendário preserva `aberto` antes de `folga_trabalhada`; o escritor não recebe turno aberto. O D4 trata o relógio separadamente, conforme o ADR. (`app/colaboradores/services/calendario.py:144`; `app/ponto/status_dia.py:90`, `app/ponto/status_dia.py:158`; `app/docs/ADR/0002-celula-e-o-dia.md:21`.)

**4. Checklist**

| Item | Resultado |
|---|---|
| Segundo juiz | O helper projeta fatos já julgados pela A2; não escolhe vínculo nem reordena precedência. Contudo, duplica o predicado que permanece inline no calendário. A2 e status D4 estão registrados; a coexistência dos leitores está declarada até E4. (`app/ponto/precedencia.py:187`; `app/colaboradores/services/calendario.py:801`; `app/core/juizes.py:160`, `app/core/juizes.py:173`; `app/docs/ADR/0002-celula-e-o-dia.md:73`.) |
| Segundo escritor fora da porta | Não aparece no delta: os três sítios passam fatos; status continua escrito na porta. (`app/ponto/services/cartorio.py:733`, `app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:1558`; `app/ponto/portas/celula.py:705`.) |
| Relógio fora da borda | O delta não acrescenta leitura. D5 captura um instante e deriva `hoje`. Já `julgar_celula` lê `localdate()` independentemente de `agora`, além de outros `now()`; é anterior ao delta fornecido. (`app/ponto/management/commands/lavrar_status_competencia.py:43`; `app/ponto/services/cartorio.py:1687`, `app/ponto/services/cartorio.py:1708`, `app/ponto/services/cartorio.py:1711`.) |
| Derivação paralela | Existe no predicado de folga e nas alimentações diferentes descritas acima. Além disso, a costura consulta o override `_vd`, enquanto a porta resolve o veredito efetivo. (`app/ponto/precedencia.py:191`; `app/colaboradores/services/calendario.py:607`, `app/colaboradores/services/calendario.py:801`; `app/ponto/services/cartorio.py:1560`; `app/ponto/portas/celula.py:658`.) |
| Selo/RED sem mordida | O selo de escritor tem casos contra atribuição/save, update e bulk_update indevidos. O selo de costura verifica apenas a presença de `fatos_do_status=`: aceita qualquer `f`. O teste novo chama diretamente o adaptador com `indefinida` e A2 mockada; não atravessa `_vd=None → porta deriva indefinida`. (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:259`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:166`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:284`; `app/ponto/tests/test_e3_status_lavrado.py:133`.) |
| Ordem de trava | Há atomicidade de save+evento, mas isso não prova serialização da leitura dos insumos até a escrita. Nenhuma aquisição explícita de trava de linha aparece nesses caminhos; inversão/contenda efetiva **[não verificado]**. (`app/ponto/portas/celula.py:639`; `app/ponto/management/commands/lavrar_status_competencia.py:49`; `app/ponto/services/cartorio.py:776`.) |
| Impressão e reaplicação | O novo fato consultado não entra no hash; os pulos por impressão antecedem o adaptador. D5 ignora status já preenchido, portanto não é mecanismo de correção dos status anteriores à r3. A promessa de proteção pelo mesmo hash não cobre diretamente a nova leitura; o ADR já admite a lacuna de isenção. (`app/ponto/services/cartorio.py:333`, `app/ponto/services/cartorio.py:385`, `app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:726`; `app/docs/ADR/0002-celula-e-o-dia.md:59`.) |

O **custo incremental** da nova costura é:

- **Cartório normal:** zero consultas A2 adicionais, justamente porque `_vd` nunca ativa o ramo novo. Isso não significa custo total zero: `julgar_celula` já consulta A2 posteriormente para carimbar precedência. (`app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1560`, `app/ponto/services/cartorio.py:1670`, `app/ponto/services/cartorio.py:1719`.)
- **D5 dry:** nenhuma consulta nova dessa costura. **D5 apply:** uma chamada A2 por célula elegível com `indefinida`, sem reaproveitamento em lote. (`app/ponto/services/cartorio.py:733`; `app/ponto/status_dia.py:122`.)
- **Por chamada A2:** cinco consultas estruturais básicas — ausências, FolgaDia, feriados, células e vínculos — mais foto de folgas se houver vínculos. Podem somar consulta de geradora ausente, nome do feriado e acessos lazy de localização. Número observado e duração **[não verificado]**. (`app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:245`, `app/ponto/precedencia.py:271`, `app/ponto/precedencia.py:275`, `app/ponto/precedencia.py:289`; `app/escala/alimentacao.py:123`; `app/core/feriados.py:100`, `app/core/feriados.py:113`.)

O contrato do ADR exige lote por competência, igualdade da tela com a folha e precedência preservada; os desvios acima impedem considerar essas propriedades demonstradas pela r3. (`app/docs/ADR/0002-celula-e-o-dia.md:24`, `app/docs/ADR/0002-celula-e-o-dia.md:29`, `app/docs/ADR/0002-celula-e-o-dia.md:51`.)

## 31/10 09:04 · sessao a00cb56b707a · - · - · origem=Ronald (tmux)

Censo adicional dos leitores A2, delimitado a acessos de produção aos campos solicitados:

| Campo | Leitores e uso |
|---|---|
| `faixa` | Classificação do calendário: `app/colaboradores/services/calendario.py:107`, montagem `:793`; novo helper: `app/ponto/precedencia.py:188`; texto de supressão `:378`; carimbo de ata: `app/ponto/services/cartorio.py:1673`; chamados espúrios: `app/chamados/services/feriado_reavaliar.py:36`; buracos da semana: `app/escala/services/semana_do_posto.py:177`; veto de feriado: `app/ponto/management/commands/detectar_ausencias.py:616`; filtro de previsão: `app/ponto/services/abriu_nao_bateu.py:123`; diagnóstico: `app/ponto/services/dia_do_colab.py:130`. |
| `tipo_do_dia` | Calendário, inclusive efeito desconta: `app/colaboradores/services/calendario.py:794`, `:798`, `:802`; helper: `app/ponto/precedencia.py:192`; texto de supressão `:378`; ata: `app/ponto/services/cartorio.py:1672`; folha sem furo: `app/folha/services/sem_furo.py:26`; vigias: `app/ponto/services/faltas_de_hoje.py:61`, `app/ponto/services/deslogados.py:166`; palavra do dia: `app/ponto/services/dia_decidido.py:370`; diagnóstico: `app/ponto/services/dia_do_colab.py:86`. |
| `fatos.folga_lancada` | Calendário: `app/colaboradores/services/calendario.py:801`; helper: `app/ponto/precedencia.py:191`; repouso para aviso de férias CLT §3: `app/ferias/situacao.py:97`. O último consulta os fatos subjacentes mesmo com férias vencendo, conforme a própria regra explica em `app/ferias/situacao.py:82`. |

Os produtores continuam os já indicados: sem jurisdição, A2 escreve `None` em `app/ponto/precedencia.py:344` e `:346`; com vencedor escreve `tipo_do_dia`, `faixa` e booleano `folga_lancada` em `:356`, `:357`, `:365`. O cartório persiste uma projeção `tipo_do_dia/faixa/...` na ata pela porta em `app/ponto/services/cartorio.py:1671`; a porta escreve `ata['precedencia']` em `app/ponto/portas/celula.py:819`.

O espelho transporta o dict A2 para o tradutor da palavra, sem recalcular esses campos: carrega o período em `app/ponto/services/espelho.py:659`, transporta em `:881` e entrega a `dia_decidido.do_dia` em `:351`; o acesso efetivo a `tipo_do_dia` está em `app/ponto/services/dia_decidido.py:370`.

Há dois valores de memória chamados `folga_humana`: o adaptador novo escreve em `app/ponto/status_dia.py:126` e devolve em `:136`, consumido pela regra em `:89`; o calendário escreve sua própria expressão inline em `app/colaboradores/services/calendario.py:801`, consumida em `:116`. Isso identifica as duas derivações sem confundi-las com uma coluna persistente.

## 31/10 09:03 · sessao a00cb56b707a · - · - · origem=Ronald (tmux)

Evidências consolidadas, todas da cópia local:

- **Escritor persistente:** o delta acrescenta argumentos aos três chamadores; a aplicação e escrita de `status_dia/status_rotulo` continuam na porta, em `app/ponto/portas/celula.py:686`, `:698`, `:701`, `:705`. A persistência do save e evento é atômica em `app/ponto/portas/celula.py:634`, `:639`. A delegação do modelo permanece em `app/escala/models.py:1787`.
- **Falha central da costura:** no ramo normal, `_vd` recebe somente `None` ou `'cobrado'`; o adaptador recebe esse valor, enquanto a porta pode derivar `'indefinida'` pelos códigos. Logo o ramo normal não consulta a nova A2, mesmo quando o status finalmente é julgado com veredito `'indefinida'`. `app/ponto/services/cartorio.py:1220`, `:1245`, `:1558`, `:1560`; `app/ponto/portas/celula.py:658`, `:745`; `app/ponto/status_dia.py:122`.
- **D5 efetivamente consulta A2:** passa `cel.veredito`; somente em apply, status nulo e carimbo considerado em dia. Não existe cache/lote para essa nova consulta. `app/ponto/services/cartorio.py:726`, `:729`, `:733`, `:735`.
- **Ramo nunca bateu:** deriva pelos códigos, mas fora da cobertura integral força `'nunca_bateu'`; a própria grade define cobertura integral como ausência/falta sem parcial. `app/ponto/services/cartorio.py:997`, `:1031`, `:1032`, `:1041`, `:1047`. Caso efetivo de nova consulta A2 nesse ramo: [não verificado].

Custo incremental da A2 por chamada efetiva do D5:

| Consulta | Prova |
|---|---|
| Ausências | `app/ponto/precedencia.py:227` |
| FolgaDia do dia | `app/ponto/precedencia.py:236` |
| Feriados | `app/ponto/precedencia.py:245`; `app/core/feriados.py:82` |
| Células | `app/ponto/precedencia.py:271`; `app/escala/alimentacao.py:43` |
| Vínculos candidatos | `app/ponto/precedencia.py:275`; `app/escala/alimentacao.py:116` |
| Foto de folgas, se há vínculos | `app/ponto/precedencia.py:289`; `app/escala/alimentacao.py:66` |
| Extra se geradora falta entre candidatos | `app/escala/alimentacao.py:123` |
| Extra para nome, se feriado | `app/ponto/precedencia.py:364`; `app/core/feriados.py:100` |
| Possíveis acessos lazy de posto/município | `app/ponto/precedencia.py:243`; `app/core/feriados.py:113` |

São **cinco consultas estruturais básicas, seis com vínculos, mais extras condicionais**; o número observado e duração são [não verificado]. O normal paga **zero consultas incrementais da nova costura**, por causa de `_vd`; `julgar_celula` já consulta A2 separadamente após o julgamento, para carimbar precedência. `app/ponto/services/cartorio.py:1220`, `:1560`, `:1670`, `:1717`.

Mapa de valores:

| Valor | Escritor/origem | Leitores principais |
|---|---|---|
| `folga_humana` | Local do adaptador, derivado do helper A2: `app/ponto/status_dia.py:121`, `:126`, `:136`; helper `app/ponto/precedencia.py:179` | `app/ponto/status_dia.py:89` |
| `cobertura_parcial` | Grade, via juiz `cobre_parte_do_dia`: `app/escala/utils.py:1137`, `:1150`, `:1190` | `app/ponto/supra_juiz.py:243`, `:279`; `app/ponto/services/cartorio.py:991`, `:1144`; `app/ponto/status_dia.py:136`, `:86` |
| `tipo_dia` | Base trabalho/folga: `app/escala/utils.py:1743`, `:1749`; overlay folga `:1118`; falta/ausência `:1174`, `:1180` | `app/ponto/supra_juiz.py:279`; adaptador `app/ponto/status_dia.py:133`; regra `:83` |
| `veredito='indefinida'` | Mapa de três códigos: `app/escala/models.py:1583`, `:1592`; códigos no supra, inclusive parcial sem período: `app/ponto/supra_juiz.py:305`, `:312`, `:324`, `:357` | Porta deriva/grava: `app/ponto/portas/celula.py:658`, `:770`; adaptador `app/ponto/status_dia.py:122`; status `:88` |
| A2 `faixa`, `tipo_do_dia`, `fatos.folga_lancada` | `app/ponto/precedencia.py:344`, `:346`, `:350`, `:356`, `:357`, `:365`; FolgaDia de outro calendário é filtrada em `:313` | Novo helper `app/ponto/precedencia.py:188`, `:191`; ata `app/ponto/services/cartorio.py:1672`, `:1673` |

Checklist adicional:

- **Registro:** A2 e status estão registrados em `app/core/juizes.py:160`, `:173`; a autoridade nova continua sendo `status_do_dia_lavrado`, não `status_da_lavra`, conforme declaração em `app/docs/ADR/0002-celula-e-o-dia.md:63`. O helper é projeção dos fatos já julgados, mas conserva derivação paralela à expressão inline do calendário até E4, como o próprio docstring admite. `app/ponto/precedencia.py:184`.
- **Selo tem mordida para escritor:** planta atribuição/save, update e bulk_update indevidos de status. `app/ponto/tests/test_selo_veredito_so_pela_porta.py:259`.
- **Selo da costura não morde o defeito:** verifica apenas presença da keyword `fatos_do_status`, aceita um valor arbitrário `f`, não verifica `colab/data/veredito` nem identidade com veredito efetivo. `app/ponto/tests/test_selo_veredito_so_pela_porta.py:156`, `:166`, `:284`.
- **Testes r3 não atravessam a costura normal:** chamam diretamente o adaptador com `'indefinida'` e A2 mockada; não detectam `_vd=None` chegando ao adaptador enquanto a porta deriva `'indefinida'`. `app/ponto/tests/test_e3_status_lavrado.py:133`, `:135`, `:137`.
- **Relógio D5:** uma leitura na borda, com `hoje` derivado dela. `app/ponto/management/commands/lavrar_status_competencia.py:43`. `_completar_status` não passa `agora`, mas o caminho que só muda status preserva `veredito_em`. `app/ponto/services/cartorio.py:733`; `app/ponto/portas/celula.py:753`, `:757`.
- **Relógio legado da unidade:** `julgar_celula` lê `localdate()` independentemente do `agora` injetado e pode voltar a ler `now()`. Isso antecede o delta dado. `app/ponto/services/cartorio.py:1687`, `:1708`, `:1711`.
- **Travas:** não há aquisição explícita de trava de linha no D5, em `julgar_colab` ou na lavra; há atomic no save+evento. Ordem/contenda efetiva de locks: [não verificado]. Trechos: `app/ponto/management/commands/lavrar_status_competencia.py:49`, `:55`; `app/ponto/services/cartorio.py:739`, `:776`; `app/ponto/portas/celula.py:639`.
- **Impressão não inclui o novo fato:** lista de insumos codificados está em `app/ponto/services/cartorio.py:333`, `:339`, `:344`, `:352`, `:362`, `:377`, `:385`; `folga_humana` não participa. Pulo por impressão vem antes do adaptador, e D5 ignora status já preenchido. `app/ponto/services/cartorio.py:1157`, `:1161`, `:726`. A promessa do ADR de proteção pelo mesmo hash merece contestação; ele já admite explicitamente a lacuna de isenção. `app/docs/ADR/0002-celula-e-o-dia.md:59`.

## 31/10 09:03 · sessao 020e958af6b5 · F1C4-3 · verificador · origem=Code

O delta corrige o caso `trabalha=None` sem tipo na ata, mas mantém uma decisão própria no supra e não garante equivalência com `dia_sem_trabalho`. Há um contraexemplo no código: ata explícita com `tipo_dia='indefinido'`. (`app/escala/services/leitor_celula.py:344-345`; `app/chamados/juizes.py:408`; `app/ponto/supra_juiz.py:48,443-447`)

**(1) Escritores, leitores e efeito do delta**

- **`SEM_JURISDICAO`:** uma definição em produção, no supra, e um leitor executável: a condição que emite `COBRANCA_EM_DIA_SEM_TRABALHO`. Acrescentar `'indefinido'` impede esse código quando o dia tem esse valor; os outros requisitos continuam sendo previsto zero, chamado vivo e cobertura `None`. (`app/ponto/supra_juiz.py:48,443-447`)
- **`tipo_dia='indefinido'`:** o escritor literal é `grade_da_celula`. Primeiro prevalece um `ata.tipo_dia` verdadeiro; sem ele, o leitor traduz `True → trabalho`, `False → folga`, `None → indefinido` e entrega o resultado no dicionário do dia. O delta não muda esse escritor. (`app/escala/services/leitor_celula.py:344-345,359`)
- **Tipo lavrado:** `ata_do_dia` copia o `tipo_dia` recebido; portanto, escrever o rótulo no resultado do leitor e lavrá-lo na ata são operações distintas. (`app/ponto/services/cartorio.py:558,566`)

Os consumidores do classificador ficam assim:

| Consumidor | Mudança de resposta |
|---|---|
| Command `supra_juiz` | Deixa de receber aquela acusação para `indefinido` e, por ela, deixa de chegar à retratação. A escrita continua dependendo das guardas de conversa, cobrança órfã e `--executar`. (`app/ponto/management/commands/supra_juiz.py:206,218,225,239,243-249`) |
| Cartório, ramo normal | Se receber esse tipo, perde essa causa de reconciliação; outra causa ainda pode reconciliar. (`app/ponto/services/cartorio.py:1169-1179,1268-1276`) |
| Cartório, ramo “nunca bateu” | Sem efeito nesse ramo: passa `chamados_vivos=0`, incompatível com a condição alterada. (`app/ponto/services/cartorio.py:996-1000`; `app/ponto/supra_juiz.py:444`) |
| `emitir_furo_retroativo` | A lista pode perder o código, mas a seleção continua exigindo `FURO_SEM_COBRANCA`. (`app/ponto/management/commands/emitir_furo_retroativo.py:99-110`) |

Os leitores genéricos de `tipo_dia` não passam a consultar a tupla: declaração continua pulando dias diferentes de trabalho; worklist continua devolvendo ausência de gap; relatório de furos continua classificando esse dia como folga; inteligência e furos diários continuam excluindo-o; as seleções de valores da folha continuam restritas aos tipos que já aceitavam. (`app/chamados/services/declaracao_texto.py:90-96`; `app/ponto/worklist.py:18-20`; `app/relatorios/furos.py:151-153`; `app/inteligencia/calculadores.py:46-47,141-142`; `app/escala/services/furos_diarios.py:39-40`; `app/folha/export.py:1071-1080`)

**Não retratar não equivale a implementar “limbo cobra”:** o ramo de furo do supra continua exigindo trabalho ou ausência parcial; `'indefinido'` não entra nele por esta mudança. (`app/ponto/supra_juiz.py:281-285`)

**(2) Predicado paralelo e comparação**

Sim, permanece um predicado próprio no supra: `tipo_dia != 'trabalho' and tipo_dia not in SEM_JURISDICAO`. Ele já existia; o delta amplia sua exceção, sem delegar a `dia_sem_trabalho`. (`app/ponto/supra_juiz.py:443`; diff fornecido)

Com `trabalha=None`, dentro da vida do colaborador e satisfeitos os demais requisitos da acusação:

| Ata | `dia_sem_trabalho` | Supra após delta |
|---|---:|---|
| Ausente ou sem tipo verdadeiro | `False` | Não acusa |
| `tipo_dia='trabalho'` | `False` | Não acusa |
| `tipo_dia='folga'`, `'ausencia'` ou `'falta'` | `True` | Acusa |
| **`tipo_dia='indefinido'` explícito** | **`True`** | **Não acusa** |

A tabela decorre da prioridade da ata no leitor, do predicado do juiz e da exclusão no supra. **Ocorrência de ata explícita `'indefinido'` em produção: [não verificado].** (`app/escala/services/leitor_celula.py:344-345`; `app/chamados/juizes.py:408`; `app/ponto/supra_juiz.py:48,443-447`)

O problema é que o mesmo rótulo chega ao supra tanto por ausência de decisão quanto por valor explícito da ata; a condição não distingue essas origens. Isso impede afirmar que o delta preserva integralmente as duas cláusulas do aval. (`app/escala/services/leitor_celula.py:344-345,359`; `app/ponto/supra_juiz.py:443`)

Há uma precisão sobre “juiz registrado”: o registro e o lookup apontam `EscalaColaborador.eh_dia_trabalho` para dia de trabalho; `dia_sem_trabalho` é o helper existente usado pelos consumidores de chamados. Não aparece nominalmente como autoridade registrada nesses dois arquivos. (`app/core/juizes.py:158`; `app/docs/JUIZES.md:66-73`; `app/chamados/juizes.py:443`; `app/chamados/models.py:1227-1233`)

**(3) Mordida do RED e mudança do teste antigo** [nao verificado]

**O RED novo morde a reversão da tupla.** Sua entrada satisfaz todos os requisitos restantes: `indefinido`, previsto zero, chamado vivo e cobertura `None`. Retirar `'indefinido'` da tupla faz reaparecer o código proibido pelo `assertNotIn`; o controle de folga continua acusando. O ensaio da função real, com reversão somente em memória, confirmou esse resultado. (`app/ponto/tests/test_f1c4_3_supra_indefinido_nao_retrata.py:17-31`; `app/ponto/supra_juiz.py:443-447`)

Seu alcance, porém, é a classificação de uma string: não passa pela célula, pelo leitor ou pelo executor, nem separa `indefinido` sintético de `indefinido` lavrado. Logo, morde o delta, mas não prova o contrato inteiro. (`app/ponto/tests/test_f1c4_3_supra_indefinido_nao_retrata.py:17-28`)

A mudança antiga inverte uma **caracterização**, como o próprio comentário atual registra. A lei substantiva já diz `None=indeterminado (limbo cobra)`; a classe também afirma que “não sei” não autoriza “sei que não”. Portanto, a expectativa antiga não constitui fundamento para revogar essa lei. (`app/ponto/tests/test_supra_juiz.py:197-200,238-245`; `app/escala/models.py:1656`; `app/docs/LEIS.md:73`; `CLAUDE.md:45-51`)

Execução histórica dos commits RED/GREEN e resultado da suíte completa: **[não verificado]**. [nao verificado]

**(4) Classes de reprovação**

- **Derivação paralela de fonte única: aplica-se.** O supra mantém sua própria resposta e há divergência construível com o juiz existente. A exigência de leitor sem regra própria está expressa. (`app/ponto/supra_juiz.py:443`; `app/chamados/juizes.py:408`; `CLAUDE.md:17-22`)
- **Segundo juiz:** aplica-se como duplicação da decisão; dizer que o delta *cria* um segundo juiz registrado seria impreciso, pois a condição já existia e o registro nominal é outro. (`app/ponto/supra_juiz.py:443`; `app/core/juizes.py:158`; `CLAUDE.md:281-283`)
- **RED sem mordida: não para a reversão examinada.** Há mordida, com a limitação de cobertura descrita acima. (`app/ponto/tests/test_f1c4_3_supra_indefinido_nao_retrata.py:17-31`)
- **Segundo escritor fora da porta e relógio fora da borda:** o delta não acrescenta escrita nem leitura de relógio ao código produtivo; muda a tupla que controla uma acusação, cuja retratação continua encaminhada à porta existente. (`app/ponto/supra_juiz.py:48,443`; `app/ponto/management/commands/supra_juiz.py:222,248-249`)
- **Ordem de trava e pouso antes do pré-requisito:** violação efetiva **[não verificado]**; o material fornecido não comprova a sequência de validação e pouso. A exigência de RED evidenciado consta da regra. (`CLAUDE.md:28-29`) [nao verificado]

## 31/10 09:03 · sessao 63820f55298b · - · - · origem=Ronald (tmux)

Q1 pode ser respondida integralmente por uma tabela de decisão ordenada, mas a tupla pedida omite `previsto_min`, que também decide a saída. A grade produz trabalho com `min_por_data.get(d, 0)`, enquanto a lavra testa `not previsto_min`; portanto não é correto dizer que os seis campos bastam (`app/escala/utils.py:1749-1753`; `app/ponto/status_dia.py:93-94`).

Use B=tem_batida, P=cobertura_parcial, I=isento, H=folga_humana, N=bool(previsto_min), A={furo,discordante,cobrado,nunca_bateu,fato_em_ausencia}. Primeira linha aplicável vence; `*` significa qualquer valor (`app/ponto/status_dia.py:42-44,83-108`):

| B | tipo | condição adicional | saída |
|---|---|---|---|
| 0 | falta | * | falta |
| 0 | ausencia | não(P e não I e v∈A∪{indefinida}) | ausencia |
| * | * | v=indefinida,H=1 | B?folga_trabalhada:folga |
| * | * | v=indefinida,H=0 | escala_suspeita |
| 0 | * | tipo=folga ou N=0 | folga |
| 0 | * | I=1 | ok, rótulo isento |
| 0 | * | v=coberta | ausencia |
| 0 | * | v∈A | irregular |
| 0 | * | resto | sem_certidao |
| 1 | * | tipo=folga ou v=fato_sem_previsao | folga_trabalhada |
| 1 | * | v∈A | irregular |
| 1 | * | v∈{concorde,coberta,cobranca_indevida} | ok, rótulo isento se I |
| 1 | * | resto | sem_certidao |

Isto cobre o domínio da função, inclusive combinações artificiais; alcance de cada célula em produção não é fornecido pelo dado. A produção tem filtros úteis: P nasce apenas no ramo grade `tipo_dia=ausencia`; juiz devolve imediatamente lista vazia para I=1; nunca-bateu força v=nunca_bateu quando não existe cobertura integral; vereditos de admin passam por outra regra na porta (`app/escala/utils.py:1173-1190`; `app/ponto/supra_juiz.py:195-196`; `app/ponto/services/cartorio.py:997,1026-1041`; `app/ponto/portas/celula.py:694-698`).

**Defeito crítico do delta / costura real:** ramo normal inicializa `_vd=None` e só pode trocar para `'cobrado'`; passa este `_vd` como argumento `veredito` de `fatos_do_status`. Essa função só consulta A2 quando o argumento é `'indefinida'`, logo no ramo normal H é sempre False (`app/ponto/services/cartorio.py:1220-1245,1558-1560`; `app/ponto/status_dia.py:121-126`). Depois a porta deriva o veredito efetivo dos códigos e o passa à regra do status (`app/ponto/portas/celula.py:658,698,745-752`). Assim `indefinida` derivada normalmente vem com H falso mesmo se A2 dissesse feriado/folga humana. O veredito efetivo já é calculado em `_tl.vereditos` antes, pela autoridade certa, mas não alimenta fatos_do_status (`app/ponto/services/cartorio.py:1249-1252`). O D5 usa cel.veredito e pode preencher H verdadeiro para o mesmo veredito, causando resultados diferentes entre evento e lote (`app/ponto/services/cartorio.py:735-736`).

`indefinida` não é apenas PREVISTO_COM_DIA_INDEFINIDO: mapa inclui FASE_NAO_DECLARADA_COM_PREVISTO e PARCIAL_SEM_PERIODO (`app/escala/models.py:1579-1599`). O primeiro nasce no ramo sem fato com dia de cobrança e previsto; parcial integra dia_de_cobranca (`app/ponto/supra_juiz.py:279-326`). PARCIAL_SEM_PERIODO nasce com fato em cobertura parcial aprovada sem período; `indefinida` pode portanto ter batida (`app/ponto/supra_juiz.py:348-358`). Sem vínculo válido também acusa indefinida sem previsto, com ou sem fato, desde que os portões pertinentes passem (`app/ponto/supra_juiz.py:367-370,405-407`).

**Divergências adicionais demonstráveis pelo código:**

1. Ausência aprovada de subtipo `folga_compensatoria`/`feriado_folga`: grade chama de `ausencia`, pois todo slug diferente de falta segue o mesmo ramo; A2 classifica ambos como faixa folga (`app/escala/utils.py:1170-1190`; `app/ponto/precedencia.py:60-63`). Sem batida lavra devolve ausencia antes de olhar H/v; calendário força trabalho=False e devolve folga (`app/ponto/status_dia.py:85-90`; `app/colaboradores/services/calendario.py:116-117,134-135`). Com batida calendário dá folga_trabalhada ou aberto, lavra pode dar irregular/ok porque tipo grade é ausencia (`app/colaboradores/services/calendario.py:144-147`; `app/ponto/status_dia.py:102-108`). Aprovação é realmente o filtro da grade para folha, independentemente do efeito ABONA/SUPRIME (`app/ponto/turnos.py:2482-2491,2734-2738`).

2. Feriado que suprime trabalho só é consultado pela nova costura para v=indefinida. Para v diferente, grade pode continuar trabalho, H false e lavra irregular/ok; calendário respeita faixa feriado para qualquer veredito, protegendo folga/folga_trabalhada da sobreposição pelo mapa (`app/ponto/status_dia.py:122-126,93-108`; `app/colaboradores/services/calendario.py:112-115,168-169`). A2 tem exceção trabalha_em_feriado e previsto=True; este caso não é feriado supressor (`app/ponto/precedencia.py:328-336`). Não afirmar todo feriado como desvio.

3. Parcial não isento sem batida e veredito acusador: lavra deliberadamente irregular/escala_suspeita, enquanto calendário retorna ausencia imediatamente para faixa atestado/afastamento/ferias, sem ler P ou veredito (`app/ponto/status_dia.py:85-91,99-100`; `app/colaboradores/services/calendario.py:109-110`). É mudança desejada pela cobrança parcial do juiz, mas quebra alegação de igualdade com calendário/precedência do ADR (`app/ponto/supra_juiz.py:274-283`; `app/docs/ADR/0002-celula-e-o-dia.md:51-57`).

4. Nunca-bateu trabalho previsto sem batida: lavra irregular, calendário mapa diz falta; exceção isento, cobertura e folga podem absorver antes (`app/ponto/status_dia.py:93-101`; `app/colaboradores/services/calendario.py:54-58,109-143,168-173`). Este desvio é pré-delta, mas relevante à matriz e à afirmação ADR, e testes declaram expressamente irregular pela lei (`app/ponto/tests/test_e3_status_lavrado.py:54-60`).

5. `indefinida` em folga só de escala: lavra escala_suspeita, calendário preserva folga/folga_trabalhada quando trabalho=False; mapa não sobrescreve essas palavras (`app/ponto/status_dia.py:88-94`; `app/colaboradores/services/calendario.py:134-147,168-169`). Isso também é escolha declarada na regra, e não prova de equivalência com calendário (`app/ponto/status_dia.py:72-78`).

6. `concorde` sem batida em trabalho previsto e não isento: lavra sem_certidao; calendário não aplica ok sem batida e permanece falta/ausencia conforme cobertura (`app/ponto/status_dia.py:95-101`; `app/colaboradores/services/calendario.py:131-133,161-162`). Pode ocorrer antes da adesão ou por dia justificado/cobertura pendente, dependendo dos gates do juiz; não inventar ocorrência produção (`app/ponto/supra_juiz.py:263,301-303,338-339`; `app/escala/models.py:1595-1596`).

Os novos testes de H testam a função pura com H fornecido e mockam fatos_do_dia diretamente; não cobrem a passagem `_vd=None` → porta deriva indefinida (`app/ponto/tests/test_e3_status_lavrado.py:100-140`). Portanto “morde a costura” no comentário não prova o ramo real (`app/ponto/tests/test_e3_status_lavrado.py:118-120`).

## 31/10 09:02 · sessao 82ba77670bad · - · - · origem=Ronald (tmux)

Conclusões Q3, todas só pelo código; ocorrência nos dados de produção desses cenários: **[não verificado]**.

1. **Com o mesmo dict A2, o helper equivale à combinação das duas condições de precedência do calendário**, não ao booleano `folga_humana` inline isolado. O helper retorna `faixa == 'feriado' OR (faixa == 'folga' AND (folga_lancada OR subtipo_de_folga))`; o calendário trata `feriado` separadamente e só usa seu booleano inline quando `faixa == 'folga'`. Assim, feriado puro dá helper `True` e booleano inline `False`, mas ambos suprimem trabalho. Referências: `app/ponto/precedencia.py:187-192`; `app/colaboradores/services/calendario.py:112-117`; `app/colaboradores/services/calendario.py:801-802`.

2. **A alimentação A2 difere, permitindo respostas diferentes para a mesma pessoa/data.** `fatos_do_status` chama `fatos_do_dia(colab,data)` sem parâmetros adicionais; este chama `fatos_do_periodo` sem overrides. Esse caminho busca FolgaDia de todos os vínculos da pessoa, preserva o objeto e pode inspecionar origem/vínculo. O calendário, quando o ativo cobre a janela, fornece `{data: True}` contendo somente as folgas daquele ativo. Referências: `app/ponto/status_dia.py:122-126`; `app/ponto/precedencia.py:209`; `app/ponto/precedencia.py:234-238`; `app/colaboradores/services/calendario.py:593-608`; `app/escala/alimentacao.py:66-68`.

3. **Contraexemplo A: FolgaDia manual do vínculo antigo B, ativo A cobre a janela, célula de trabalho gerada por A, sem ausência/feriado.** A consulta sem overrides encontra a folga de B; como manual não tem `calendario_origem_id`, ela continua válida, A2 responde folga e helper `True`. A alimentação do calendário só traz datas de A, omite a folga manual de B e pode responder trabalho/`folga_humana=False`. Referências: `app/ponto/precedencia.py:234-238`; `app/ponto/precedencia.py:313-315`; `app/ponto/precedencia.py:337-350`; `app/colaboradores/services/calendario.py:593-608`; `app/escala/alimentacao.py:66-68`. A própria bateria já codifica que folga manual do vínculo antigo é da pessoa, mas testa o caminho `fatos_do_dia`, não a alimentação do calendário: `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:41-52`.

4. **Contraexemplo B: ativo A cobre a janela, célula geradora B, FolgaDia de calendário vinculada a A, sem ausência/feriado.** Sem overrides, A2 descarta essa FolgaDia quando o vínculo selecionado é B; no calendário a linha virou `True`, e `getattr(True,'calendario_origem_id',None)` impede a verificação de propriedade do calendário. A2 do dia pode responder trabalho/helper `False`, enquanto A2 alimentada pela tela responde folga/helper efetivo `True`. Referências: `app/ponto/precedencia.py:305-315`; `app/colaboradores/services/calendario.py:607-608`; `app/ponto/precedencia.py:337-350`.

5. **O contraexemplo B também admite dívida de cadastro:** se B estiver fora da própria vigência, o juiz de vínculo devolve `None`, mesmo existindo ativo A; A2 continua lendo `trabalha=True` da célula para `previsto`, mas descarta o objeto FolgaDia de A porque seu vínculo não é `None`. O booleano passado pelo calendário continua escapando desse descarte. Referências: `app/escala/alimentacao.py:237-251`; `app/ponto/precedencia.py:305-322`; `app/colaboradores/services/calendario.py:607-608`. A correspondência exata desse cenário com veredito `indefinida` depende do restante do julgamento; o supra acusa célula de trabalho sem vínculo mesmo sem previsto no ramo com realizado: `app/ponto/supra_juiz.py:367-370`.

6. **Mesmo com helper equivalente, não há equivalência geral entre status final e calendário.** O calendário conserva `aberto` antes de `folga_trabalhada`, enquanto a regra nova, em `indefinida + folga_humana + batida`, devolve diretamente `folga_trabalhada`; o leitor D4 pode corrigir a temporalidade antes do encerramento, mas o escritor não recebe `turno_aberto`. Referências: `app/colaboradores/services/calendario.py:144-147`; `app/colaboradores/services/calendario.py:168-169`; `app/ponto/status_dia.py:88-91`; `app/docs/ADR/0002-celula-e-o-dia.md:63-64`.

7. **Há divergência explícita na ausência parcial sem batida:** o calendário retorna ausência/falta imediatamente para qualquer faixa de ausência, sem receber `cobertura_parcial`; a lavra deixa o veredito acusador decidir quando parcial e não isento. Isso não foi tornado igual pelo helper. Referências: `app/colaboradores/services/calendario.py:109-110`; `app/colaboradores/services/calendario.py:799-820`; `app/ponto/status_dia.py:85-87`; `app/ponto/status_dia.py:99-100`.

8. **A afirmação documental “mesma pergunta… escrita UMA vez” é mais forte que a implementação:** o predicado permanece no calendário e o helper foi acrescentado; ADR permite leitores congelados até E4, mas isso não elimina a duplicação nem iguala seus insumos. Referências: `app/ponto/precedencia.py:184-185`; `app/colaboradores/services/calendario.py:801-802`; `app/docs/ADR/0002-celula-e-o-dia.md:73-74`.

## 31/10 09:02 · sessao fe1c61ccd0f1 · - · - · origem=Ronald (tmux)

O RED novo morde a reversão: a fixture tem `tipo_dia='indefinido'`, previsto/realizado zero, chamado vivo, cobertura `None`; removendo apenas `'indefinido'` de `SEM_JURISDICAO`, o ramo volta a emitir `COBRANCA_EM_DIA_SEM_TRABALHO`, contrariando `assertNotIn`. O controle `'folga'` continua emitindo nos dois casos. Confirmei executando a função real e alterando a tupla somente em memória, com Python `-B`; não executei a suíte Django. Fontes: `app/ponto/tests/test_f1c4_3_supra_indefinido_nao_retrata.py:17`, `:28`, `:31`; `app/ponto/supra_juiz.py:48`, `:443`.

A alteração antiga muda a expectativa, mas o próprio arquivo identifica a expectativa anterior como “caracterização de 31/08”. A lei da classe já dizia “‘não sei’ nunca vira ‘sei que não’”, e o modelo declara `None=indeterminado (limbo cobra)`. Portanto, há fundamento para corrigir a caracterização; não encontrei lei que mandasse retratar `'indefinido'`. Fontes: `app/ponto/tests/test_supra_juiz.py:197`, `:238`; `app/escala/models.py:1656`; `app/docs/LEIS.md:73`; `CLAUDE.md:45`.

Limite importante do RED: ele testa diretamente a string, sem passar pela leitura da célula nem pelo executor. Não distingue `'indefinido'` produzido pelo fallback de `'indefinido'` explicitamente presente na ata. Fontes: `app/ponto/tests/test_f1c4_3_supra_indefinido_nao_retrata.py:17`, `:20`.

Isso permite um contraexemplo estático: com `trabalha=None` e ata explícita `tipo_dia='indefinido'`, `dia_sem_trabalho` responde `True`, pois a ata tem tipo diferente de trabalho; o supra exclui esse tipo do ramo. A presença desse caso em produção é [não verificado]. Fontes: `app/chamados/juizes.py:407`, `:408`; `app/ponto/supra_juiz.py:48`, `:443`.

Evidência de execução histórica dos commits RED/GREEN, suíte inteira e ordem efetivamente cumprida não consta dos anexos fornecidos: [não verificado]. As exigências documentadas são RED evidenciado, suíte inteira verde antes do opus final e opus antes do pouso. Fontes: `CLAUDE.md:28`, `:595`, `:600`. [nao verificado]

## 31/10 09:01 · sessao 8fa74d6ff054 · - · - · origem=Ronald (tmux)

Censo de produção, fora de `*/tests/`:

- `SEM_JURISDICAO` tem uma definição, `app/ponto/supra_juiz.py:48`, e um leitor executável, o predicado de `classificar_dia` em `app/ponto/supra_juiz.py:443`. O delta só impede que `tipo_dia='indefinido'` produza `COBRANCA_EM_DIA_SEM_TRABALHO`; ainda exige previsto zero, chamado vivo e ausência de cobertura (`app/ponto/supra_juiz.py:443-447`).
- O escritor literal de `tipo_dia='indefinido'` é `grade_da_celula`: prevalece `ata.get('tipo_dia')`; faltando valor verdadeiro, `trabalha=True → trabalho`, `False → folga`, `None → indefinido`. Escreve o resultado no dicionário do dia (`app/escala/services/leitor_celula.py:344-345`, `app/escala/services/leitor_celula.py:359`). A presença de uma ata sem `tipo_dia` não muda esse fallback (`app/escala/services/leitor_celula.py:331`, `app/escala/services/leitor_celula.py:344`).
- O próprio supra lê `dia['tipo_dia']` em `app/ponto/supra_juiz.py:199`. Seus demais ramos por natureza do dia não mudam: o furo exige trabalho ou ausência parcial (`app/ponto/supra_juiz.py:281-285`), e outros ramos também exigem trabalho (`app/ponto/supra_juiz.py:258`, `app/ponto/supra_juiz.py:420`, `app/ponto/supra_juiz.py:448`, `app/ponto/supra_juiz.py:463`). Portanto impedir retratação não equivale a começar a emitir cobrança para o `indefinido`.

Consequências para os consumidores do classificador:

| Consumidor | Efeito do delta e evidência |
|---|---|
| Command `supra_juiz` | Deixa de receber aquele código no caso indefinido; consequentemente não entra por ele no ramo retratável. Chamada em `app/ponto/management/commands/supra_juiz.py:206`; seleção em `:218`; retratação pela porta `chamados.reconciliador.retratar` em `:222`, `:248`. Execução ainda depende das guardas de conversa (`:225`), cobrança órfã (`:239`) e execução/trava (`:243`). |
| Cartório, ramo normal | Chama o classificador (`app/ponto/services/cartorio.py:1169`) e reconcilia se recebeu um dos dois códigos (`app/ponto/services/cartorio.py:1268-1276`). A retirada deste código retira essa causa de entrada; outro código continua podendo acionar a reconciliação (`:1268`). |
| Cartório, ramo nunca bateu | Não muda por este delta: passa `chamados_vivos=0`, tornando inalcançável o ramo alterado (`app/ponto/services/cartorio.py:996-1000`; exigência em `app/ponto/supra_juiz.py:444`). |
| `emitir_furo_retroativo` | Recebe a lista modificada, mas só age por `FURO_SEM_COBRANCA`; essa seleção não muda (`app/ponto/management/commands/emitir_furo_retroativo.py:99-110`). |

Leitores de `tipo_dia` que continuam com a resposta anterior, porque não consultam `SEM_JURISDICAO`:

- `declaracao_texto` ignora `indefinido` como qualquer dia diferente de trabalho e preserva `ESTADO_LIDO`; o comentário chama explicitamente folga/indefinido de dia conhecido sem marco (`app/chamados/services/declaracao_texto.py:86`, `app/chamados/services/declaracao_texto.py:90-96`).
- `worklist` devolve `gap=None` e motivo “dia de indefinido pela celula” (`app/ponto/worklist.py:18-20`).
- O relatório de furos rotula o dia como `'folga'`; as duas consultas auxiliares o excluem (`app/relatorios/furos.py:151-153`, `app/relatorios/furos.py:218`, `app/relatorios/furos.py:237`).
- Métricas de inteligência excluem o dia (`app/inteligencia/calculadores.py:46-47`, `app/inteligencia/calculadores.py:141-142`); a apuração de furos diários também o exclui (`app/escala/services/furos_diarios.py:39-40`).
- Exportação de valores inclui apenas trabalho/ausência; `indefinido` segue fora (`app/folha/export.py:1071`, `app/folha/export.py:1075`, `app/folha/export.py:1080`). A causa monetária agrupa previstos somente quando trabalho (`app/folha/causas.py:41`).
- `disputa_emissao` exige trabalho (`app/chamados/services/disputa_emissao.py:68`); `materializacao` só distingue ausência de célula/fora do vínculo antes de repassar marcos, portanto o delta não a altera (`app/chamados/services/materializacao.py:223`).

Ponto decisivo para a comparação dos juízes: uma ata **explícita** `{'tipo_dia': 'indefinido'}` passa intacta pelo leitor, pois tem precedência sobre `trabalha` (`app/escala/services/leitor_celula.py:344`). O supra agora a exclui do ramo (`app/ponto/supra_juiz.py:48`, `app/ponto/supra_juiz.py:443`), enquanto `chamados.juizes.dia_sem_trabalho` devolve `True` por qualquer `tipo_dia` presente e diferente de trabalho (`app/chamados/juizes.py:408`). Sem `tipo_dia` na ata e com `trabalha=None`, ambos deixam de afirmar dia sem trabalho (`app/chamados/juizes.py:408`, `app/escala/services/leitor_celula.py:345`, `app/ponto/supra_juiz.py:443`). Ocorrência real de ata explícita `indefinido` em produção: [não verificado].

## 31/10 09:01 · sessao aae15ce37be6 · - · - · origem=Ronald (tmux)

- O supra mantém um predicado local de “sem trabalho”: `tipo_dia != 'trabalho' and tipo_dia not in SEM_JURISDICAO`, condicionado ainda a previsto zero, chamado vivo e ausência de cobertura. Ele não consulta `chamados.juizes.dia_sem_trabalho`; o delta acrescenta uma exceção à regra local já existente. (`app/ponto/supra_juiz.py:48`, `app/ponto/supra_juiz.py:443`)

- O juiz real responde **True** somente quando `cel.trabalha is False` ou quando a ata contém `tipo_dia` verdadeiro e diferente de `trabalho`. Portanto `None` sozinho não significa folga. (`app/chamados/juizes.py:398`, `app/chamados/juizes.py:407`, `app/chamados/juizes.py:408`)

- O leitor dá prioridade ao `ata.tipo_dia`; no fallback traduz True→trabalho, False→folga, None→indefinido. Logo preserva a procedência no comportamento, mas não a transporta como distinção explícita entre “indefinido da ata” e “indefinido inferido”. (`app/escala/services/leitor_celula.py:344`, `app/escala/services/leitor_celula.py:345`, `app/escala/services/leitor_celula.py:359`)

Com `trabalha=None` e demais gates do supra satisfeitos:

| Ata | `dia_sem_trabalho` | Supra após delta |
|---|---:|---|
| Sem `tipo_dia` ou valor vazio | False | Não acusa: leitor produz indefinido |
| `trabalho` | False | Não acusa |
| `folga`, `ausencia`, `falta` | True | Acusa |
| `indefinido` explicitamente presente | True | Não acusa |

Evidências para todas as células: `app/chamados/juizes.py:408`, `app/escala/services/leitor_celula.py:344`, `app/ponto/supra_juiz.py:48`, `app/ponto/supra_juiz.py:443`. A existência de ata com `tipo_dia='indefinido'` em produção é **[não verificado]**.

- Outra divergência permitida pelas funções: `trabalha=False` com ata `tipo_dia='trabalho'` produz True no juiz, mas o leitor entrega trabalho e o supra não acusa. A existência desse par em produção é **[não verificado]**. (`app/chamados/juizes.py:408`, `app/escala/services/leitor_celula.py:344`, `app/ponto/supra_juiz.py:443`)

- A regra de origem diz expressamente `None=indeterminado (limbo cobra)`; a grade só interrompe como folga quando `work is False`, tratando None como `sem_ancora`. (`app/escala/models.py:1656`, `app/escala/utils.py:725`, `app/escala/utils.py:726`, `app/escala/utils.py:729`)

- **Cuidado com “juiz registrado”:** o registro declara a pergunta “o dia é de trabalho para o vínculo?” em `EscalaColaborador.eh_dia_trabalho`, e o lookup aponta o mesmo endereço; `dia_sem_trabalho` é o helper existente utilizado por `_celula_furada`. Não encontrei o nome `dia_sem_trabalho` no registro ou no lookup; classificá-lo como autoridade nominalmente registrada seria impreciso. (`app/core/juizes.py:158`, `app/docs/JUIZES.md:66`, `app/chamados/juizes.py:443`)

- A regra arquitetural é um juiz por pergunta; o lookup diz que a lei está no contrato 1 e o registro vivo em `core/juizes.py`. A L-061 manda consumir a ata. Assim, o delta corrige o caso None sem ata, mas deixa uma segunda expressão da decisão no supra; dizer que o delta **cria** esse segundo juiz exagera, pois a condição local já existia no diff fornecido. (`CLAUDE.md:281`, `app/docs/JUIZES.md:10`, `app/docs/LEIS.md:95`, `app/ponto/supra_juiz.py:443`)

## 31/10 08:54 · sessao 1ebd570c5147 · DISJUNTOR-B3-TOPO · verificador · origem=Code

**O contrato promete reconhecer uma pergunta; o código reconhece apenas a presença do ID em até duas linhas.** Essa diferença permite falsos positivos (`bin/hook_stop_fila1.py:1038–1043`).

**(a) Leitores e limites do desenho**

- `_linha_do_topo()` atende ESPERANDO, orçamento/CLEAR, recuperação do CLEAR, B4 e PAREI (`bin/hook_stop_fila1.py:645`, `:898`, `:1015`, `:1034`, `:1095`).
- Em leitura normal, esses consumidores continuam recebendo a primeira linha. Porém agora dependem da leitura adicional: um `OSError` depois da primeira linha descarta o resultado já obtido e retorna `None`. Portanto “a MESMA […] letra por letra” não garante equivalência nas falhas (`bin/hook_stop_fila1.py:405–432`).
- Há leitores independentes: o handoff percorre até a primeira seção `##`, seleciona PAREI/lei/! e mostra até quatro entradas; o alarme procura `PAREI:` nas primeiras 40 linhas. Seus parsers não passam pelo helper modificado; o handoff também consulta os vereditos do hook, ficando indiretamente sujeito à mudança acima (`bin/handoff_sessao.sh:92–123`; `bin/alarme_sessao_ociosa.py:119–127`).
- O Stop chama efetivamente `'recados'`, depois das saídas por PAREI, ESPERANDO provado, fila vazia, teto e B2. Logo a decisão B3 desse ramo **não acontece em todo Stop**, inclusive quando ESPERANDO já libera com prova (`bin/hook_stop_fila1.py:1217–1224`, `:1236–1237`, `:1275–1291`).
- `OSError` no helper vira `None`, convertido pelo B3 em topo vazio: cobra a pergunta sem registrar essa leitura como `FALHA`. Outras exceções dentro de `_disjuntor_contido` tentam registrar `FALHA` e retornam `None`, mantendo o caminho normal; quem libera diretamente por exceção é o `except` externo do `main` (`bin/hook_stop_fila1.py:431–432`, `:1038–1055`, `:1291–1297`).
- O interruptor está respeitado antes desses ramos; `DJ.desligado` verifica `DISJUNTOR_DESLIGADO` por `lexists` (`bin/hook_stop_fila1.py:1029–1030`; `bin/disjuntor.py:120–122`).

**(b) Escritores, leitores e restauração**

| Arquivo | Escritores e leitores identificados |
|---|---|
| `logs/hook_stop_fila1.json` | O próprio hook lê o JSON e grava `head/blocks/item`; `_zera_teto` também grava, zerando o contador (`bin/hook_stop_fila1.py:35`, `:1058–1064`, `:1248–1271`). |
| Contador nos testes | `test_disjuntor` copia/restaura; orçamento semeia/confere; teto de espera semeia/altera; teste ESPERANDO remove antes de rodar (`bin/tests/test_disjuntor.sh:333–339`; `bin/tests/test_orcamento_contexto.sh:194–198`; `bin/tests/test_hook_teto_nao_conta_espera.sh:54`, `:79–82`; `bin/tests/test_hook_esperando_com_prova.sh:65`). |
| `RELATO.md`: escrita | `_topo` escreve a fixture; negativas do disjuntor chamam `no_relato`, que lê e substitui sob trava. Também escrevem `gerar_cortes` e `deploy_agendado` (`bin/tests/test_disjuntor.sh:72`; `bin/disjuntor.py:201–203`; `app/core/esteira_vigia.py:844–884`; `bin/gerar_cortes.py:117–136`; `bin/deploy_agendado.sh:37–44`). |
| `RELATO.md`: leitura | Além dos leitores do topo citados, há o checador integral, publicação/raspagem e checagem da versão no índice (`bin/relato_afirma_com_prova.py:88`; `bin/relato.sh:56`, `:104`; `bin/relato_prova_no_commit.sh:10`). |

O ramo B3 apenas monta a cobrança; não escreve a pergunta. Quem efetivamente a escreveu em produção: `[nao verificado]` (`bin/hook_stop_fila1.py:1037–1049`).

**A restauração funciona no caminho verde, mas não é garantida em todos os caminhos.** O backup precede os dois Stops e a restauração precede a continuação; porém `return 18/19` pula a restauração. Backup vazio, inexistente ou com falha de cópia resulta em remoção, e o sucesso da cópia de restauração não é conferido (`bin/tests/test_disjuntor.sh:333–340`).

A proteção é necessária: o teto libera quando `n > 5`, antes da varredura B3 que alimenta a verificação final de `b3_sem_obra`. Os casos posteriores recriam a árvore temporária, limitando contaminação entre casos (`bin/hook_stop_fila1.py:37`, `:1275–1291`; `bin/tests/test_disjuntor.sh:368–371`, `:48–49`).

**(c) Os mutantes mordem?**

Por análise estática, **ambos atingem a costura pretendida**:

- **Só linha 1:** deixa de encontrar OBRA-X na segunda linha, emite o recado e provoca `return 18` (`bin/tests/test_disjuntor.sh:334–335`, `:710`; `bin/hook_stop_fila1.py:1040–1043`).
- **Bloco inteiro:** passa a alcançar a pergunta na terceira linha, cala o recado e provoca `return 19` (`bin/tests/test_disjuntor.sh:336–337`, `:711`; `bin/hook_stop_fila1.py:424–430`).

A primeira asserção, isoladamente, **passa por ausência de sinal**: não exige resposta válida nem ausência de `FALHA`; `_passa` possui essa proteção, mas não é usado ali. A segunda exige presença do recado, portanto silêncio generalizado não faz o caso inteiro passar (`bin/tests/test_disjuntor.sh:91–93`, `:335–337`).

O harness confere âncora única e possui controle sem mutação, mas aceita **qualquer retorno não zero** como morte do mutante; não exige especificamente 18/19. Assim, uma falha alheia também pode aparentar morte (`bin/tests/test_disjuntor.sh:635–646`).

Verde no commit, rc=18 na base e mortes efetivamente executadas: `[nao verificado]`. [nao verificado]

**(d) Prefixo e prosa antiga**

**Sim.** Com OBRA-X disparada, estes conteúdos na segunda linha silenciam a cobrança por dedução direta do teste de substring: [nao verificado]

- Pergunta de outra obra: `> **PERGUNTA DE DESENHO B3 OBRA-X-OUTRA:** ...`
- Prosa antiga: `> ontem revisamos OBRA-X`

Não há validação de ID inteiro, formato de pergunta, atualidade ou ESPERANDO na primeira linha (`bin/hook_stop_fila1.py:1038–1043`).

O limite apenas exclui a terceira linha e encerra em linha vazia após começar; não distingue história de pergunta. Os novos fixtures cobrem posição segunda/terceira, sem cobrir prefixo ou prosa (`bin/hook_stop_fila1.py:424–430`; `bin/tests/test_disjuntor.sh:334–337`).

Esse falso positivo **cala o recado do Stop**, mas não rearma B3: a negativa ao quarto verificador continua independente do topo (`bin/disjuntor.py:915–922`). Ocorrência concreta desses exemplos em produção: `[nao verificado]`.

## 31/10 08:54 · sessao 243547c7a1e6 · - · - · origem=Ronald (tmux)

(c) Os dois mutantes atingem a costura pretendida por análise estática: “só linha 1” torna visível o recado quando a pergunta está na segunda linha, causando `return 18`; “99 linhas” inclui a pergunta da terceira linha e cala o recado, causando `return 19` (`bin/tests/test_disjuntor.sh:334–337`, `bin/tests/test_disjuntor.sh:710–711`, `bin/hook_stop_fila1.py:1038–1043`).

A primeira asserção verifica somente ausência do recado; não verifica `FALHA`, nem usa `_passa`, que faz essa verificação. A segunda exige presença do recado, portanto ausência generalizada de sinal derruba o caso. O harness aceita qualquer falha do caso como morte do mutante, sem exigir especificamente rc 18/19; há controle de cópia sem mutação (`bin/tests/test_disjuntor.sh:74–76`, `bin/tests/test_disjuntor.sh:91–93`, `bin/tests/test_disjuntor.sh:335–337`, `bin/tests/test_disjuntor.sh:635–646`). Execução efetiva dos mutantes e resultado verde: `[nao verificado]`.

A fixture `pid=1` não deve liberar pela espera: o PID 1 integra explicitamente os ancestrais rejeitados, e o Stop libera somente com prova (`bin/hook_stop_fila1.py:470`, `bin/hook_stop_fila1.py:519–523`, `bin/hook_stop_fila1.py:1222–1224`).

(d) Sim: o B3 procura uma substring nas duas linhas concatenadas, sem validar pergunta, fronteira do ID ou `ESPERANDO` na primeira linha. Com OBRA-X disparada, tanto `PERGUNTA ... OBRA-X-OUTRA` quanto `ontem revisamos OBRA-X` na segunda linha calam o recado. São contraexemplos derivados do código; ocorrência em produção: `[nao verificado]` (`bin/hook_stop_fila1.py:419–430`, `bin/hook_stop_fila1.py:1038–1043`).

Uma linha vazia após o painel encerra a leitura; a terceira linha fica excluída pelo limite. Os novos fixtures cobrem pergunta na segunda/terceira linha, sem distinguir prosa ou ID prefixado (`bin/hook_stop_fila1.py:424–430`, `bin/tests/test_disjuntor.sh:334–337`). Calar o recado não rearma a obra: o quarto verificador continua negado independentemente do RELATO (`bin/disjuntor.py:915–922`).

## 31/10 08:53 · sessao 1d3211662a7d · - · - · origem=Ronald (tmux)

(b) Auditoria estática, sem execução dos testes: [nao verificado]

- **Contador:** `ESTADO` aponta para `logs/hook_stop_fila1.json`; o próprio Stop lê o JSON, calcula `blocks` por HEAD e o regrava. `_zera_teto` é outro escritor do mesmo arquivo, chamado pela transição de orçamento. Referências: `bin/hook_stop_fila1.py:35`, `bin/hook_stop_fila1.py:1248`, `bin/hook_stop_fila1.py:1268`, `bin/hook_stop_fila1.py:1270`, `bin/hook_stop_fila1.py:1058`, `bin/hook_stop_fila1.py:1213`.

- **Consumidores de teste do contador:** `test_disjuntor.sh` lê para backup e escreve/remove para restauração; `test_orcamento_contexto.sh` semeia e confere valores; `test_hook_teto_nao_conta_espera.sh` semeia, lê e altera o contador; `test_hook_esperando_com_prova.sh` remove-o antes de cada execução. Referências: `bin/tests/test_disjuntor.sh:333`, `bin/tests/test_disjuntor.sh:339`, `bin/tests/test_orcamento_contexto.sh:194`, `bin/tests/test_orcamento_contexto.sh:198`, `bin/tests/test_hook_teto_nao_conta_espera.sh:54`, `bin/tests/test_hook_teto_nao_conta_espera.sh:79`, `bin/tests/test_hook_teto_nao_conta_espera.sh:82`, `bin/tests/test_hook_esperando_com_prova.sh:65`.

- **Restauração:** no caminho verde, salva antes dos dois Stops e restaura depois deles, antes dos casos posteriores. Isso evita consumir duas unidades adicionais do teto: o teto libera antes da chamada que varre B3. Referências: `bin/tests/test_disjuntor.sh:333`, `bin/tests/test_disjuntor.sh:335`, `bin/tests/test_disjuntor.sh:337`, `bin/tests/test_disjuntor.sh:339`, `bin/hook_stop_fila1.py:1275`, `bin/hook_stop_fila1.py:1291`.

- **Limites da restauração:** não é incondicional: `return 18/19` ocorre antes dela; backup ausente, vazio ou falha de `cp` tornam-se indistinguíveis e levam à remoção do contador; o sucesso do `cp` de restauração não é checado. Os casos seguintes recriam a árvore temporária, e o trap remove a temporária ao final. Referências: `bin/tests/test_disjuntor.sh:333`, `bin/tests/test_disjuntor.sh:335`, `bin/tests/test_disjuntor.sh:337`, `bin/tests/test_disjuntor.sh:339`, `bin/tests/test_disjuntor.sh:48`, `bin/tests/test_disjuntor.sh:39`.

- **RELATO, escritores concretos:** o teste escreve o topo por `_topo`; o disjuntor registra negativas por `relato_negacao → no_relato`; esse escritor lê e substitui o arquivo sob trava, inserindo na seção de alarmes. Há outros escritores fora dessa porta: `gerar_cortes.py` e `deploy_agendado.sh`. Referências: `bin/tests/test_disjuntor.sh:72`, `bin/disjuntor.py:201`, `bin/disjuntor.py:203`, `app/core/esteira_vigia.py:844`, `app/core/esteira_vigia.py:860`, `app/core/esteira_vigia.py:884`, `bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136`, `bin/deploy_agendado.sh:37`, `bin/deploy_agendado.sh:44`. A documentação identifica o topo como escrito à mão; autoria/execução humana efetiva em produção: `[nao verificado]` (`app/core/esteira_vigia.py:829`).

- **RELATO, leitores concretos:** o hook lê pelo helper do topo e pelo checador de afirmações; handoff percorre até a primeira seção `##`; alarme de ociosidade lê 40 linhas; publicação passa o arquivo pelo checador e raspador; o pre-commit lê a versão do índice. Referências: `bin/hook_stop_fila1.py:421`, `bin/hook_stop_fila1.py:1167`, `bin/relato_afirma_com_prova.py:88`, `bin/handoff_sessao.sh:92`, `bin/handoff_sessao.sh:102`, `bin/alarme_sessao_ociosa.py:124`, `bin/relato.sh:56`, `bin/relato.sh:104`, `bin/relato_prova_no_commit.sh:10`.

- **Contrato de falha:** exceção em `_disjuntor_contido` produz tentativa de trilha `FALHA` e retorna `None`; isso mantém o caminho normal do hook, não significa liberar o Stop automaticamente. O `except` externo do `main` é que libera por falha aberta. Referências: `bin/hook_stop_fila1.py:1050`, `bin/hook_stop_fila1.py:1055`, `bin/hook_stop_fila1.py:1291`, `bin/hook_stop_fila1.py:1296`.

## 31/10 08:53 · sessao e2950ef2ed7b · - · - · origem=Ronald (tmux)

(a) Leitores e diferenças:

- `_linha_do_topo()` passa a executar `_bloco_do_topo()` inteiro antes de devolver a primeira linha; para texto legível, conserva o valor anterior, mas uma falha `OSError` ao tentar avançar após a primeira linha agora descarta essa linha e devolve `None`. Portanto o comentário “MESMA […] letra por letra” não vale para todas as falhas de leitura (`bin/hook_stop_fila1.py:405`, `bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:421`, `bin/hook_stop_fila1.py:431`).
- Consumidores da primeira linha: ESPERANDO (`bin/hook_stop_fila1.py:645`), orçamento/CLEAR (`bin/hook_stop_fila1.py:898`, `bin/hook_stop_fila1.py:1015`), B4 (`bin/hook_stop_fila1.py:1034`) e PAREI (`bin/hook_stop_fila1.py:1095`). Todos passam a depender dessa leitura adicional (`bin/hook_stop_fila1.py:407`).
- `UnicodeDecodeError` não é capturado pelo helper, cujo `except` cobre somente `OSError`; ESPERANDO o converte em “leitura falhou”, o disjuntor o contém como `FALHA`, e uma exceção que escape até `main` libera o hook (`bin/hook_stop_fila1.py:431`, `bin/hook_stop_fila1.py:671`, `bin/hook_stop_fila1.py:1050`, `bin/hook_stop_fila1.py:1296`).
- Leitores independentes em `bin/`: handoff lê até a primeira seção `## `, seleciona PAREI/lei/! e imprime até quatro; alarme busca qualquer substring `PAREI:` nas primeiras 40 linhas. Seus parsers não usam o helper alterado; o handoff também consulta os vereditos do hook (`bin/handoff_sessao.sh:92`, `bin/handoff_sessao.sh:102`, `bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:110`, `bin/handoff_sessao.sh:121`, `bin/alarme_sessao_ociosa.py:123`, `bin/alarme_sessao_ociosa.py:127`).
- Outro leitor integral: Stop chama o checador de afirmações; este lê todo o arquivo, sem usar o helper (`bin/hook_stop_fila1.py:1167`, `bin/relato_afirma_com_prova.py:88`).

Limites do contrato:

- `RELATO` ilegível por `OSError` produz `None` no helper, mas B3 transforma isso em topo vazio e pode emitir a cobrança; esse caminho não entra na trilha `FALHA` (`bin/hook_stop_fila1.py:431`, `bin/hook_stop_fila1.py:1038`, `bin/hook_stop_fila1.py:1040`).
- Stop real solicita `'recados'` em `main`, depois das liberações por PAREI, ESPERANDO provado, fila vazia, teto e B2. Logo B3 não decide em todos os Stops; ESPERANDO válido já libera antes dessa verificação (`bin/hook_stop_fila1.py:1217`, `bin/hook_stop_fila1.py:1223`, `bin/hook_stop_fila1.py:1236`, `bin/hook_stop_fila1.py:1275`, `bin/hook_stop_fila1.py:1286`, `bin/hook_stop_fila1.py:1291`).

## 31/10 08:47 · sessao ac2386711bba · E3-STATUS · verificador · origem=Code

O delta ainda deixa uma regressão demonstrável: **ausência parcial de isento que nunca bateu passa a receber `ok`**, embora a regra preserve o rótulo de ausência acima da isenção. Também permanecem perdas de precedência entre grade, lavra e calendário. (`app/ponto/services/cartorio.py:1040`; `app/ponto/status_dia.py:80`; `app/escala/utils.py:799`.)

**1. Combinações alcançáveis e incoerências**

As cinco coordenadas pedidas não bastam para determinar a resposta: falta `previsto_min`. A mesma tupla `(ausencia, False, True, True, nunca_bateu)` recebe `ok` com previsto positivo e `folga` com previsto zero. Além disso, comparar com o calendário exige conhecer a faixa A2 e o subtipo da ausência, informações que `fatos_do_status` não transporta. (`app/ponto/status_dia.py:80`; `app/ponto/status_dia.py:82`; `app/ponto/status_dia.py:84`; `app/ponto/status_dia.py:112`; `app/colaboradores/services/calendario.py:107`.)

Admin não integra o domínio de `status_da_lavra`: `_status_alvo` desvia tanto a decisão direta quanto a célula anteriormente decidida pelo admin para `status_do_admin`. As três chamadas do cartório passam fatos, mas esse desvio permanece. (`app/ponto/portas/celula.py:694`; `app/ponto/services/cartorio.py:733`; `app/ponto/services/cartorio.py:1041`; `app/ponto/services/cartorio.py:1556`.)

Na tabela, a tupla segue a ordem solicitada; `B` e `I` significam os dois valores booleanos quando indicados.

| Combinação e condição adicional | Status lavrado | Alcance e avaliação |
|---|---|---|
| `(ausencia, False, True, True, nunca_bateu)`, previsto positivo | `ok` | **Regressão do delta.** A grade conserva ausência parcial; o juiz isento retorna vazio, mas o ramo sem adesão força `nunca_bateu`. A nova exceção pula `ausencia` e alcança o isento. A regra escrita preserva ausência/folga no rótulo. (`app/escala/utils.py:1180`; `app/escala/utils.py:1190`; `app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:1031`; `app/ponto/services/cartorio.py:1040`; `app/ponto/status_dia.py:80`; `app/escala/utils.py:799`.) |
| `(ausencia, False, True, I, nunca_bateu)`, previsto zero | `folga` | O ramo sem adesão também força essa palavra na cobertura parcial sem previsto. A lavra perde a ausência porque cai no teste de zero. Há **conflito textual a explicitar**: A2 mantém ausência acima de folga, enquanto L-089 restringe seu efeito ao dia previsto de trabalho; a própria auditoria das leis registra essa divergência de classificação. Não há palavra normativa única demonstrada apenas pela tupla. (`app/escala/utils.py:1743`; `app/escala/utils.py:1170`; `app/ponto/services/cartorio.py:996`; `app/ponto/services/cartorio.py:1040`; `app/ponto/status_dia.py:82`; `app/ponto/precedencia.py:46`; `app/docs/LEIS.md:121`; `app/docs/LEIS.md:172`.) |
| `(folga, B, False, False, indefinida)`, **com feriado ou folga humana prevalecente** | `escala_suspeita` | **Regressão de precedência do delta.** Célula `trabalha=True` com geradora inválida produz grade folga/zero e acusação de cadastro. Feriado ou folga manual ainda podem vencer pela A2. A lavra agora ultrapassa essa natureza, contrariando a exceção expressa do ADR para `folga`/`folga_trabalhada`. (`app/ponto/supra_juiz.py:168`; `app/ponto/supra_juiz.py:171`; `app/escala/utils.py:1543`; `app/escala/utils.py:1743`; `app/ponto/supra_juiz.py:367`; `app/ponto/supra_juiz.py:405`; `app/ponto/precedencia.py:296`; `app/ponto/precedencia.py:319`; `app/ponto/status_dia.py:77`; `app/docs/ADR/0002-celula-e-o-dia.md:51`.) |
| `(ausencia, False, False, I, concorde)`, subtipo `folga_compensatoria` ou `feriado_folga` | `ausencia` | **Defeito anterior ao delta.** A2 classifica esses subtipos como folga; `_marcar_ausencias` os achata para ausência. A lavra não recebe o subtipo necessário para conservar essa distinção. (`app/ponto/precedencia.py:60`; `app/escala/utils.py:1179`; `app/ponto/status_dia.py:80`; `app/ponto/status_dia.py:112`.) |
| `(ausencia, True, False, False, fato_em_ausencia)`, mesmos subtipos de folga | `irregular` | **Defeito anterior ao delta.** O realizado pode reaparecer depois de o overlay limpar células/órfãs; o juiz acusa fato em ausência e a lavra segue essa acusação, embora A2 diga que era folga. (`app/escala/utils.py:1195`; `app/escala/utils.py:1985`; `app/escala/utils.py:1998`; `app/ponto/supra_juiz.py:361`; `app/escala/models.py:1587`; `app/ponto/status_dia.py:93`; `app/ponto/precedencia.py:60`.) |
| `(ausencia, True, False, True, concorde)`, mesmos subtipos de folga | `ok` | A isenção faz o juiz retornar vazio, derivando `concorde`; a lavra dá `ok`, também perdendo a natureza de folga humana. (`app/ponto/supra_juiz.py:195`; `app/escala/models.py:1595`; `app/ponto/status_dia.py:95`; `app/ponto/precedencia.py:60`.) |

Há ainda uma **lacuna alcançável**, sem gabarito de palavra suficientemente demonstrado: `(trabalho, False, False, False, concorde)`, previsto positivo, após justificativa aprovada. A grade mantém trabalho, marca `justificado` e limpa o furo; o juiz remove `FURO_*`; a lavra retorna `sem_certidao` apesar do julgamento existente. O calendário pode retornar `falta`, que não se sustenta sem ausência descontante pela L-039. A palavra correta dessa justificativa específica é `[nao verificado]`. (`app/escala/utils.py:1198`; `app/escala/utils.py:1203`; `app/ponto/supra_juiz.py:338`; `app/escala/models.py:1595`; `app/ponto/status_dia.py:90`; `app/colaboradores/services/calendario.py:133`; `app/colaboradores/services/calendario.py:161`; `app/docs/LEIS.md:87`.)

**Não é correto reprovar toda ocorrência de `folga + indefinida`.** Sem feriado/folga humana, o calendário pode consultar a célula `trabalha=True` e também mostrar `escala_suspeita`: a “folga” da grade veio da ausência de vínculo válido, não necessariamente da natureza A2 do dia. (`app/ponto/supra_juiz.py:168`; `app/escala/models.py:1071`; `app/colaboradores/services/calendario.py:770`; `app/colaboradores/services/calendario.py:158`.)

Existe também o caminho de rollback: `GRADE_POR_TURNO=False` remapeia a grade descartando `minutos_previstos` e `cobertura_parcial`; a lavra normaliza o previsto ausente para zero. Isso permite, por exemplo, trabalho sem batida virar `folga`. O default da cópia é verdadeiro; valor efetivo de produção e ocorrência desse caminho são `[nao verificado]`. (`app/escala/utils.py:1016`; `app/escala/utils.py:2026`; `app/escala/utils.py:2029`; `app/ponto/status_dia.py:112`; `app/ponto/status_dia.py:82`; `app/config/settings/base.py:165`.)

Exaustividade sobre **estados históricos entregues pelo D5** é `[nao verificado]`: ele reutiliza o veredito armazenado se a impressão coincide, e a própria ADR reconhece que isenção não está nessa impressão. Portanto “mesmo hash” não demonstra que o veredito corresponde a todos os fatos atuais. (`app/ponto/services/cartorio.py:999`; `app/ponto/services/cartorio.py:1155`; `app/ponto/services/cartorio.py:733`; `app/docs/ADR/0002-celula-e-o-dia.md:58`.)

**2. Diferenças em relação ao calendário**

| Situação | Lavra × calendário | Qual regra sustenta |
|---|---|---|
| Trabalho sem batida, `nunca_bateu`, não isento, previsto positivo | `irregular` × `falta` | **Lavra:** falta exige decisão humana. (`app/ponto/status_dia.py:88`; `app/colaboradores/services/calendario.py:56`; `app/docs/LEIS.md:87`.) |
| Mesmo caso, isento | `ok`, rótulo próprio × calendário pode sobrepor `falta` | **Lavra:** ADR determina `ok` para isento; o mapa antigo do calendário ainda sobrepõe `nunca_bateu`. (`app/ponto/status_dia.py:84`; `app/colaboradores/services/calendario.py:122`; `app/colaboradores/services/calendario.py:168`; `app/docs/ADR/0002-celula-e-o-dia.md:42`.) |
| Ausência parcial, sem batida, não isento, previsto positivo, `furo` | `irregular` × `ausencia` | **Lavra:** BO-INSS mantém cobrança do restante do dia. O calendário retorna antes de ler o veredito. (`app/ponto/supra_juiz.py:274`; `app/ponto/supra_juiz.py:279`; `app/ponto/status_dia.py:80`; `app/colaboradores/services/calendario.py:109`.) |
| Ausência parcial de isento sem adesão | `ok` × `ausencia` | **Calendário:** ausência conserva o rótulo acima da isenção; é a regressão descrita acima. (`app/ponto/status_dia.py:80`; `app/colaboradores/services/calendario.py:109`; `app/escala/utils.py:799`.) |
| `indefinida` em feriado/folga humana | `escala_suspeita` × `folga`/`folga_trabalhada` | **Calendário:** exceção de precedência expressa no ADR. L-099 exige coerência, mas não escreve uma prioridade geral de `indefinida` sobre a natureza do dia. (`app/ponto/status_dia.py:77`; `app/colaboradores/services/calendario.py:112`; `app/colaboradores/services/calendario.py:116`; `app/colaboradores/services/calendario.py:168`; `app/docs/ADR/0002-celula-e-o-dia.md:51`; `app/docs/LEIS.md:122`.) |
| Folga lançada pelos subtipos de ausência | `ausencia`, `irregular` ou `ok` × `folga`/`folga_trabalhada` | **Calendário:** A2 reconhece explicitamente esses subtipos como folga. (`app/ponto/precedencia.py:60`; `app/ponto/status_dia.py:80`; `app/ponto/status_dia.py:93`; `app/ponto/status_dia.py:95`; `app/colaboradores/services/calendario.py:116`.) |
| Trabalho coberto, encerrado, `fato_em_ausencia`, batidas regulares | `irregular` × pode ser `ok` ou `sem_certidao` | **Lavra:** o juiz acusa o trabalho em cobertura. O calendário não possui esse veredito no mapa e depende das pontas/desvio. A ressalva são os subtipos de folga da linha anterior. (`app/ponto/supra_juiz.py:361`; `app/ponto/status_dia.py:42`; `app/ponto/status_dia.py:93`; `app/colaboradores/services/calendario.py:54`; `app/colaboradores/services/calendario.py:150`; `app/colaboradores/services/calendario.py:153`.) |
| `fato_sem_previsao` com batida | `folga_trabalhada` × pode permanecer `ok`, `irregular`, `sem_certidao` ou `aberto` | Não há mapa desse veredito no calendário. Se a natureza efetiva for folga, ADR/A2 sustentam `folga_trabalhada`; para outras causas de previsto zero, gabarito universal de status é `[nao verificado]`. (`app/ponto/status_dia.py:91`; `app/colaboradores/services/calendario.py:54`; `app/colaboradores/services/calendario.py:144`; `app/colaboradores/services/calendario.py:148`; `app/docs/ADR/0002-celula-e-o-dia.md:53`.) |
| Admin: `trabalhou`, `faltou`, `justificado` | Lavra sempre `ok`, `falta`, `ausencia`; calendário pode preservar natureza/relógio, suprimir `ok` sem batida ou mapear `justificado→folga` | **Lavra:** decisão humana prevalece expressamente. Esses casos passam por `status_do_admin`, não por `status_da_lavra`. (`app/ponto/status_dia.py:39`; `app/ponto/portas/celula.py:694`; `app/colaboradores/services/calendario.py:58`; `app/colaboradores/services/calendario.py:161`; `app/colaboradores/services/calendario.py:168`; `app/docs/ADR/0002-celula-e-o-dia.md:31`.) |
| Trabalho sem batida, `concorde`, previsto positivo, após justificativa | `sem_certidao` × pode ser `falta` | **Nenhum dos dois demonstrado integralmente:** calendário contraria L-039; palavra correta da lavra é `[nao verificado]`. (`app/ponto/status_dia.py:90`; `app/colaboradores/services/calendario.py:133`; `app/docs/LEIS.md:87`.) |

A coluna lavrada e o estado temporal são comparações diferentes: D4 deve devolver `previsto`/`aberto` antes do encerramento. O calendário, por sua vez, deixa de considerar o turno em curso como “futuro” quando já existe veredito; isso pode expor acusação antes do encerramento. Nesse ponto, **D4 sustenta o leitor novo**, não a coluna isolada nem essa exceção do calendário. (`app/docs/ADR/0002-celula-e-o-dia.md:21`; `app/ponto/status_dia.py:136`; `app/ponto/status_dia.py:138`; `app/colaboradores/services/calendario.py:807`.)

**3. Quem escreve e quem lê**

O inventário abaixo distingue a chave da grade, sua materialização na ata e campos homônimos de dinheiro; estes últimos são entidades distintas segundo o ADR. (`app/ponto/services/cartorio.py:567`; `app/ponto/services/cartorio.py:568`; `app/docs/ADR/0002-celula-e-o-dia.md:8`.)

| Dado | Escritores de produção | Leitores diretos |
|---|---|---|
| `cobertura_parcial` | Grade: `app/escala/utils.py:1190`, consultando o juiz em `app/escala/utils.py:1150`. Reconstrução pela célula: `app/escala/services/leitor_celula.py:394`. Transporte dos fatos: `app/ponto/status_dia.py:115`. | `app/ponto/supra_juiz.py:243`, `app/ponto/supra_juiz.py:279`, `app/ponto/supra_juiz.py:350`; `app/ponto/services/cartorio.py:990`, `app/ponto/services/cartorio.py:998`, `app/ponto/services/cartorio.py:1142`, `app/ponto/services/cartorio.py:1151`; `app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:115`; `app/escala/services/leitor_celula.py:395`. |
| `tipo_dia` | Grade: `app/escala/utils.py:1743`, `app/escala/utils.py:1749`; overlays: `app/escala/utils.py:1118`, `app/escala/utils.py:1174`, `app/escala/utils.py:1180`. Ata: `app/ponto/services/cartorio.py:567`, atribuída/persistida pela porta em `app/ponto/portas/celula.py:768`, `app/ponto/portas/celula.py:640`. Reconstrução: `app/escala/services/leitor_celula.py:340`, `app/escala/services/leitor_celula.py:342`, `app/escala/services/leitor_celula.py:359`. Rollback: `app/escala/utils.py:727`, `app/escala/utils.py:752`, `app/escala/utils.py:987`, `app/escala/utils.py:1011`, `app/escala/utils.py:1016`. | Núcleo: `app/ponto/status_dia.py:112`, `app/ponto/supra_juiz.py:197`, `app/ponto/services/cartorio.py:567`, `app/ponto/services/cartorio.py:996`, `app/escala/utils.py:850`, `app/escala/utils.py:1170`, `app/escala/utils.py:1198`, `app/escala/services/leitor_celula.py:344`, `app/escala/services/leitor_celula.py:384`. Demais consumidores abaixo. |
| `minutos_previstos` da grade | `app/escala/utils.py:1743`, `app/escala/utils.py:1751`; exceção intermitente: `app/escala/utils.py:2010`. Ata: `app/ponto/services/cartorio.py:568`. Reconstrução: `app/escala/services/leitor_celula.py:338`, `app/escala/services/leitor_celula.py:363`, com fallback ao juiz em `app/escala/services/leitor_celula.py:353`. | `app/ponto/status_dia.py:112`; `app/ponto/supra_juiz.py:198`; `app/ponto/services/cartorio.py:568`; `app/escala/services/leitor_celula.py:346`; `app/folha/causas.py:58`, `app/folha/causas.py:84`; `app/folha/export.py:1072`, `app/folha/export.py:1074`, `app/folha/export.py:1079`; `app/ponto/services/fechamento.py:1016`; `app/ponto/services/dia_pago.py:391`; `app/ponto/services/bordas_realizado.py:76`. |
| Veredito `indefinida` | Palavra derivada pelo mapa em `app/escala/models.py:1583`, aplicado em `app/escala/models.py:1597`. Fontes dos códigos: `app/ponto/supra_juiz.py:305`, `app/ponto/supra_juiz.py:312`, `app/ponto/supra_juiz.py:324`, `app/ponto/supra_juiz.py:357`, `app/ponto/supra_juiz.py:370`, `app/ponto/supra_juiz.py:383`, `app/ponto/supra_juiz.py:397`, `app/ponto/supra_juiz.py:407`. Escrita/persistência: `app/ponto/portas/celula.py:770`, `app/ponto/portas/celula.py:775`; o modelo delega em `app/escala/models.py:1794`. | Leitores literais: `app/ponto/status_dia.py:77`; `app/colaboradores/services/calendario.py:56`; `app/relatorios/furos.py:26`; `app/relatorios/dia_da_frota.py:12`. Também integra `VEREDITOS_ACUSAM`, cujos consumidores seguem abaixo. (`app/escala/models.py:1735`.) |

Outros leitores de **`tipo_dia`**, agrupados por consumidor:

- Folha/dinheiro: `app/folha/causas.py:40`, `app/folha/causas.py:41`, `app/folha/causas.py:100`; `app/folha/export.py:1071`, `app/folha/export.py:1075`, `app/folha/export.py:1080`; `app/ponto/services/fechamento.py:1011`, `app/ponto/services/fechamento.py:1012`, `app/ponto/services/fechamento.py:1015`; `app/ponto/services/dia_pago.py:386`, `app/ponto/services/dia_pago.py:393`, `app/ponto/services/dia_pago.py:398`; `app/ponto/services/bordas_realizado.py:74`.
- Ponto/escala: `app/ponto/propositor.py:36`, `app/ponto/propositor.py:210`; `app/ponto/worklist.py:19`; `app/ponto/services/veredito_celula.py:221`; `app/ponto/services/espelho.py:690`; `app/ponto/services/simular_dia.py:12`; `app/escala/orfa_classe.py:65`; `app/escala/services/furos_diarios.py:39`.
- Chamados/relatórios: `app/chamados/services/materializacao.py:223`; `app/chamados/services/declaracao_texto.py:90`; `app/chamados/services/disputa_emissao.py:68`; `app/chamados/juizes.py:401`; `app/relatorios/furos.py:147`, `app/relatorios/furos.py:151`, `app/relatorios/furos.py:218`, `app/relatorios/furos.py:237`; `app/inteligencia/calculadores.py:46`, `app/inteligencia/calculadores.py:141`.

O calendário usa **`tipo_do_dia` da precedência**, não a chave `tipo_dia` da grade; confundi-las esconderia justamente as diferenças acima. (`app/colaboradores/services/calendario.py:794`; `app/ponto/status_dia.py:112`.)

Os homônimos persistidos **`minutos_previstos` em DiaPago/FechamentoMensal** têm escritores em `app/ponto/services/dia_pago.py:287`, `app/ponto/services/dia_pago.py:474`, `app/ponto/services/dia_pago.py:480`, `app/ponto/services/dia_pago.py:493`, `app/ponto/services/fechamento.py:1051`, `app/ponto/services/fechamento.py:1062`; leitores em `app/folha/export.py:1019`, `app/folha/export.py:1024`, `app/folha/porta_export.py:135`, `app/folha/porta_export.py:347`, `app/ponto/services/dia_pago.py:491`, `app/ponto/services/fechamento.py:1149`, `app/relatorios/cartao_pela_celula.py:117`, `app/relatorios/cartao_pela_celula.py:120`, `app/relatorios/cartao_pela_celula.py:172`. O histórico do modelo consulta/acumula o juiz em `app/ponto/models.py:879`, `app/ponto/models.py:883`.

Leitores de **`indefinida` via `VEREDITOS_ACUSAM`**: `app/folha/export.py:1215`; `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1074`; `app/chamados/models.py:2623`, `app/chamados/models.py:2624`; `app/chamados/services/disputa_emissao.py:705`; `app/chamados/services/limbo_folgas.py:194`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/vigia_fio.py:314`, `app/chamados/services/vigia_fio.py:362`; `app/chamados/services/silencio_ausencia.py:41`; `app/chamados/services/fabrica_da_celula.py:86`; `app/chamados/reconciliador.py:598`, `app/chamados/reconciliador.py:982`; `app/folha/services/vigia_pausa.py:155`, `app/folha/services/vigia_pausa.py:157`; `app/ponto/services/vigia_ausencia_veredito.py:41`, `app/ponto/services/vigia_ausencia_veredito.py:42`; `app/relatorios/dia_da_frota.py:55`.

**Efeito do delta nos leitores:** muda o status calculado pela porta e, depois de persistido, a resposta de `status_do_dia_lavrado`; não muda a palavra do veredito nem migra calendário, folha ou chamados para outra leitura. O calendário permanece congelado até E4. (`app/ponto/portas/celula.py:698`; `app/ponto/status_dia.py:142`; `app/docs/ADR/0002-celula-e-o-dia.md:71`.)

**A correção não alcança automaticamente status existentes:** D5 retorna para qualquer status não nulo; o julgamento normal também pode pular pela impressão. Quantos registros continuam com a resposta anterior é `[nao verificado]`. (`app/ponto/services/cartorio.py:726`; `app/ponto/services/cartorio.py:1155`; `app/ponto/services/cartorio.py:1159`.)

**4. Checklist**

- **Segundo juiz:** `status_da_lavra` e `status_do_dia_lavrado` são as duas metades declaradas da autoridade, com registro e corte. O calendário continua sendo derivação antiga expressamente mantida até E4; essa coexistência não constitui criação clandestina pelo delta. (`app/core/juizes.py:169`; `app/docs/JUIZES.md:74`; `app/docs/CORTES.md:414`; `app/core/medida_leitor_do_dia.py:269`; `app/core/medida_leitor_do_dia.py:284`; `app/docs/ADR/0002-celula-e-o-dia.md:71`.)
- **Segundo escritor fora da porta:** a atribuição de status encontrada está na porta; persistência passa pelo save central, inclusive após `save=False` via `carimbar`. Há dois escritores **em memória** de `cobertura_parcial`, ambos consultando a autoridade existente; isso não é outro escritor da coluna de status. (`app/ponto/portas/celula.py:705`; `app/ponto/portas/celula.py:640`; `app/ponto/portas/celula.py:807`; `app/escala/utils.py:1150`; `app/escala/utils.py:1190`; `app/escala/services/leitor_celula.py:394`.)
- **Relógio fora da borda:** o delta corrige o comando para uma leitura e deriva `hoje` dela. Permanecem leituras preexistentes no leitor 

[truncado em 20000 de 22791 caracteres]

## 31/10 08:44 · sessao dd7a74435bc0 · - · - · origem=Ronald (tmux)

Leitura complementar concluída, sem escrita de arquivo. Os três testes novos executados por AST em memória passam no código atual; no mutante que restaura o comportamento anterior, mantendo a assinatura nova, há **2 falhas e 1 erro**: ausência parcial volta a `ausencia` em vez de `irregular`, folga indefinida volta a `folga`, e desaparece a chave `cobertura_parcial`. Isso comprova mordida contra reversão, sem comprovar RED histórico nem integração Django completa. Referências: `app/ponto/tests/test_e3_status_lavrado.py:78`, `:88`, `:101`; comportamento revertido: `app/ponto/status_dia.py:75`, `:115`.

Diferenças confirmadas para a consolidação:

- **Parcial sem batida acusada**: lavra `irregular`, calendário retorna `ausencia` antes de consultar veredito. A cobrança parcial tem regra explícita BO-INSS; portanto sustenta a lavra. Caminho: `app/escala/utils.py:1184` → `app/ponto/supra_juiz.py:274` e `:279` → `app/ponto/status_dia.py:80` e `:88`; calendário: `app/colaboradores/services/calendario.py:109`.
- **`fato_em_ausencia` com batidas completas regulares em cobertura parcial**: lavra `irregular`, calendário pode mostrar `ok`, pois `_VER_STATUS` não contém esse veredito. Pode mostrar também `sem_certidao`, `aberto` ou `folga_trabalhada` conforme os outros fatos. O ramo do juiz diz expressamente que acusa e retém: `app/ponto/supra_juiz.py:348`, `:361`; mapa: `app/escala/models.py:1587`; lavra: `app/ponto/status_dia.py:42`, `:93`; calendário: `app/colaboradores/services/calendario.py:54`, `:144`, `:150`, `:153`, `:158`. Para dia encerrado de trabalho coberto, a regra sustenta a acusação da lavra; ADR exige coerência tela/folha: `app/docs/ADR/0002-celula-e-o-dia.md:29`.
- **`nunca_bateu` em trabalho sem batida**: lavra `irregular`, calendário `falta`; para isento, lavra `ok` e calendário ainda pode sobrepor `falta`. Sustentação da lavra: falta exige decisão, `app/docs/LEIS.md:87`; isento `ok`, `app/docs/ADR/0002-celula-e-o-dia.md:42`, e validação do rótulo em `app/docs/CORTES.md:411`. Código: `app/ponto/status_dia.py:84`, `:88`; calendário `app/colaboradores/services/calendario.py:56`, `:122`, `:168`.
- **Admin**: a porta sempre aplica `trabalhou→ok`, `faltou→falta`, `justificado→ausencia`; calendário pode contrariar qualquer um pelos retornos de ausência/feriado, status protegidos, supressão de `ok` sem batida, mapa `justificado→folga` ou overlay final de cobertura. A regra sustenta a lavra: decisão humana prevalece. Referências: `app/ponto/portas/celula.py:694`; `app/ponto/status_dia.py:39`; `app/colaboradores/services/calendario.py:58`, `:109`, `:112`, `:161`, `:168`, `:172`; `app/docs/ADR/0002-celula-e-o-dia.md:31`, `:44`.
- **Relógio é comparação separada**: a coluna não pretende mostrar `previsto/aberto`; o leitor D4 cobre a coluna antes do encerramento. O calendário também consulta encerramento, mas desativa seu `futuro` de turno em curso quando já há veredito. Referências: `app/ponto/status_dia.py:136`, `:138`; `app/colaboradores/services/calendario.py:697`, `:807`; regra D4: `app/docs/ADR/0002-celula-e-o-dia.md:21`.
- **Não concluir divergência automaticamente de `tipo_dia='folga'` + `indefinida`**: o caminho sem vínculo exige célula `trabalha=True`, que o calendário também consulta; pode resultar `escala_suspeita` nos dois. Referências: `app/ponto/supra_juiz.py:168`, `:405`; `app/colaboradores/services/calendario.py:760`, `:770`, `:158`.

Checklist:

- As duas funções novas são metades declaradas da autoridade: `app/core/medida_leitor_do_dia.py:269`; juiz registrado em `app/core/juizes.py:169` e `app/docs/JUIZES.md:74`. O calendário permanece derivador antigo declarado, congelado até E4: `app/core/medida_leitor_do_dia.py:284`; `app/docs/ADR/0002-celula-e-o-dia.md:70`.
- O comando corrigiu para uma leitura na borda e passa ambos os valores: `app/ponto/management/commands/lavrar_status_competencia.py:43`, `:55`. A função pura de lavra não lê relógio: `app/ponto/status_dia.py:55`.
- Escrita de status acontece na porta: `app/ponto/portas/celula.py:705`, `:752`, `:773`; selo inclui os campos e exige fatos: `app/ponto/tests/test_selo_veredito_so_pela_porta.py:16`, `:156`.
- Ordem de pousos escrita: `app/docs/CORTES.md:413`; E2 antes de E3: `app/docs/BACKLOG.md:773`. Execução efetiva dessa ordem e pouso: `[nao verificado]`.
- E2 não está demonstrada apenas por selos verdes: lacuna de isenção reconhecida no ADR e `trabalha` ainda fora da impressão na catraca. Referências: `app/docs/ADR/0002-celula-e-o-dia.md:58`; `app/escala/tests/test_d3_guarda_da_linha_celula.py:68`.
- O comentário do teste associa sem batida a `PARCIAL_SEM_PERIODO`, mas esse código real exige realizado ou órfãs, que tornam `tem_batida=True`. O caso sem batida com `indefinida` pode vir dos outros códigos de cadastro. Referências: `app/ponto/tests/test_e3_status_lavrado.py:82`; `app/ponto/supra_juiz.py:280`, `:348`; `app/ponto/status_dia.py:109`.

## 31/10 08:44 · sessao 8cc0e7d5deae · - · - · origem=Ronald (tmux)

Achados de alcance confirmados:

1. **Regressão nova: ausência parcial do isento sem adesão.** Tupla `(ausencia, False, True, True, nunca_bateu)`: o cartório força `nunca_bateu` apesar do juiz retornar vazio para isento (`app/ponto/services/cartorio.py:963`, `app/ponto/services/cartorio.py:996`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1040`; `app/ponto/supra_juiz.py:195`). A lavra nova salta `ausencia` e retorna `ok` com previsto positivo; retorna `folga` com previsto zero (`app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:82`, `app/ponto/status_dia.py:84`). O calendário retorna `ausencia` antes de consultar isenção (`app/colaboradores/services/calendario.py:109`). A regra escrita mantém a natureza/rotulagem de ausência apesar da isenção (`app/escala/utils.py:795`, `app/escala/utils.py:799`).

2. **`indefinida` sobre feriado ou folga humana.** Alcance: célula `trabalha=True`, geradora inválida, cadastro retorna `sem_vinculo_em_dia_de_trabalho=True`, grade entrega folga/zero, juiz produz `indefinida` (`app/ponto/supra_juiz.py:168`, `app/ponto/supra_juiz.py:171`, `app/escala/utils.py:1543`, `app/escala/utils.py:1743`, `app/ponto/supra_juiz.py:367`, `app/ponto/supra_juiz.py:405`, `app/escala/models.py:1583`). Havendo feriado ou folga manual, A2 ainda reconhece essa precedência (`app/ponto/precedencia.py:291`, `app/ponto/precedencia.py:296`, `app/ponto/precedencia.py:314`, `app/ponto/precedencia.py:319`, `app/ponto/precedencia.py:321`). A lavra dá `escala_suspeita`; calendário dá `folga`/`folga_trabalhada`, protegidas contra o veredito (`app/ponto/status_dia.py:77`; `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:146`, `app/colaboradores/services/calendario.py:168`). ADR sustenta a precedência do calendário (`app/docs/ADR/0002-celula-e-o-dia.md:51`). **Sem feriado/folga humana, não inferir diferença apenas de `tipo_dia='folga'`: o calendário lê a célula soberana `trabalha=True` e pode também resultar `escala_suspeita`** (`app/escala/models.py:1071`, `app/colaboradores/services/calendario.py:770`).

3. **Folga lançada como ausência perde sua natureza na grade.** `folga_compensatoria`/`feriado_folga` pertencem à faixa folga de A2, mas `_marcar_ausencias` escreve `tipo_dia='ausencia'` para todo não-falta (`app/ponto/precedencia.py:60`, `app/escala/utils.py:1179`). Sem batida, lavra `ausencia` versus calendário `folga`; com realizado e veredito `fato_em_ausencia`, lavra `irregular` versus calendário `folga_trabalhada` (`app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:93`; `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:134`, `app/colaboradores/services/calendario.py:146`). O segundo caso é alcançável porque o realizado é calculado depois de o overlay limpar células/órfãs (`app/escala/utils.py:1195`, `app/escala/utils.py:1985`, `app/escala/utils.py:1998`; `app/ponto/supra_juiz.py:361`). ADR/A2 sustentam o calendário (`app/docs/ADR/0002-celula-e-o-dia.md:54`; `app/ponto/precedencia.py:60`).

4. **Justificativa aprovada pode produzir trabalho sem batida e `concorde`, lavrado `sem_certidao`.** A grade limpa missing/órfãs e marca `justificado`, mantendo trabalho; o juiz remove o código de furo; vazio deriva `concorde`; a lavra cai no retorno residual (`app/escala/utils.py:1198`, `app/escala/utils.py:1203`; `app/ponto/supra_juiz.py:338`; `app/escala/models.py:1595`; `app/ponto/status_dia.py:90`). Calendário pode mostrar `falta`, pois suprime o `ok` sem batida (`app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:161`). A palavra correta dessa justificativa específica não está explicitamente fixada nos trechos normativos que li `[nao verificado]`; `falta` sem ausência descontante contraria L-039 (`app/docs/LEIS.md:87`).

Cuidados para a matriz:

- As cinco coordenadas solicitadas não determinam a resposta: é necessário incluir `previsto_min`; ausência parcial sem batida e `nunca_bateu` retorna `folga` se zero, `irregular` se positivo e não isento, `ok` se positivo e isento (`app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:82`, `app/ponto/status_dia.py:84`, `app/ponto/status_dia.py:88`).
- Admin não chega a `status_da_lavra`: `_status_alvo` desvia para `status_do_admin` (`app/ponto/portas/celula.py:694`).
- `PARCIAL_SEM_PERIODO` exige realizado ou órfã; o teste novo com `tem_batida=False` e essa justificativa no comentário não demonstra esse caminho (`app/ponto/supra_juiz.py:348`, `app/ponto/supra_juiz.py:351`; `app/ponto/tests/test_e3_status_lavrado.py:84`).
- O rollback legacy descarta previsto e cobertura parcial no remapeamento; também é caminho do código, embora a flag de produção seja `[nao verificado]` (`app/escala/utils.py:1016`, `app/escala/utils.py:2026`; `app/config/settings/base.py:165`).
- Frequências, colaboradores atingidos e ocorrência efetiva em produção: `[nao verificado]`.

_928 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
