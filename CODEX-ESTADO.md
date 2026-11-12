# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 02:58** · publicado em **12/11/2026 03:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 02:58 · sessao 8dc5a164915d · - · - · origem=Ronald (tmux)

Travas 1, 2, 3 e 9 — evidências para consolidação:

| Trava | Medição sustentada | Limitação / proposta |
|---|---|---|
| Preverif Codex | Anexo `triagem.log`: 15 linhas, 12 procedentes e 3 descartadas. Cruzamento com `chamadas_fim.tsv`: 80,58 min de consultas, sendo 67,07 min procedentes e 13,52 min descartadas. | Procedente conta uma **volta**, independentemente do número de achados; não prova sozinho defeito curado (`bin/disjuntor.py:870`, `bin/disjuntor.py:900`, `bin/disjuntor.py:911`). Proposta **MUDA**: conservar revisão e distinguir chamada, achado, cura e descarte; custo e economia completos não medidos. |
| Opus final | Pelo menos dois exemplos documentados de defeito confirmado após preverif: PAR-NAO-ADJACENTE, cura `d361dfee`; SELOS-CANON-I1, cura `ff47644a` na cópia, correspondente à volta 5 `f56dc9628` no gitlog fornecido (`app/docs/RELATO.md:548`, `app/docs/RELATO.md:550`, `app/docs/RELATO.md:539`, `app/docs/RELATO.md:543`). | Total de leituras e minutos não medidos: faltam registros de início/fim dos agentes finais. A definição usa `model: inherit`, portanto o nome do papel não prova modelo opus em toda chamada (`.claude/agents/verificador.md:5`). Proposta **FICA**: há falhas documentadas do preverif (`app/docs/RELATO.md:544`, `app/docs/RELATO.md:549`). |
| B3 | Contagem manual do anexo: 50 disparos, 48 liberações — 22 desenho e 26 erro-bobo —, 6 rearmes e 7 migrações. Dois disparos explicitam terceira volta Codex: E4A-2, 10/11 02:22:34 e 11:49:24. | B3 recebe reprovação ou terceira volta procedente; não diagnostica defeito (`bin/disjuntor.py:834`, `bin/disjuntor.py:955`, `bin/disjuntor.py:1007`). **ERRO-BOBO não significa sem defeito**. Proposta **MUDA**: separar confirmação de defeito, forma e pergunta de desenho; economia causal não medida. |
| Timeout construtor | Três linhas `construtor rc=124`: E3 09/11 07:47:50, 907 s; PAUTA 10/11 00:30:33, 904 s; BO-DISPUTA 10/11 17:12:03, 905 s. Soma 2.716 s = 45,27 min (`chamadas_fim.tsv`, linhas identificadas pelos horários). | Default 900 é configurável (`bin/codex_pergunta.sh:83`); execução construtor usa `timeout -k 30` (`bin/codex_pergunta.sh:401`). E3 teve entrega parcial preservada e commitada; BO-DISPUTA também teve arquivos tocados (`app/docs/RELATO.md:519`, `app/docs/RELATO.md:296`). Proposta **MUDA**: teto por trabalho medido; ganho de um novo teto não medido. |

Contagem manual dos 50 disparos B3 por data, para conferir: 31/10=6; 01/11=6; 02/11=5; 04/11=4; 05/11=7; 06/11=1; 07/11=3; 08/11=3; 09/11=2; 10/11=4; 11/11=7; 12/11=2 (`disjuntor_14d.log`, linhas `DISJUNTOR B3` nessas datas). O disparo DATA-EVENTO-CRU de 11/11 17:31:19 diz reprovação final, mas sua liberação às 17:31:55 relata terceira volta Codex; classificação consistente da origem fica [nao verificado].

Defeito real do preverif com cura explícita: F1C4-3-EMISSORES, triagem v12 procedente, volta 13 `e548d8ca`, com correção do teste nunca_bateu e documentação; isso demonstra pelo menos uma volta procedente com cura, sem transformar as 12 procedentes em 12 defeitos (`app/docs/RELATO.md:1446`). E4A-2 preverif9 confirmou criação indevida de perguntas por escritores em veredito não acusador; a errata exigiu reverter a mudança do juiz e ajustar a prova (`app/docs/RELATO.md:328`, `app/docs/RELATO.md:327`, `app/docs/RELATO.md:322`; commits `ea76f9f16` e `858bc0527` no anexo gitlog).

Há pelo menos cinco episódios documentados de reprovação por forma/prova/documento: F1C4 v11, GRADE-FOLGA-JUIZ, TOAST e duas voltas MODO-COMUM (`app/docs/RELATO.md:1426`, `app/docs/RELATO.md:1428`, `app/docs/RELATO.md:1278`, `app/docs/RELATO.md:1271`, `app/docs/RELATO.md:1276`). Cinco suites inteiras posteriores associadas somam **88,11 min**, sem comprovar que remover a trava evitaria essas execuções:

- `f1c4emis/v13/suite.log`: 1.173,218 s.
- `grade_juiz/vb/suite_60258b80.log`: 1.036,669 s.
- `toast/suite_36768b29.log`: 1.165,072 s.
- `mc/suite_2d78b6ec.log`: 961,538 s.
- `mc/suite_a25e2149.log`: 950,240 s.

Fonte dos cinco números: linhas com esses caminhos no anexo `suites_14d.tsv`. São custo associado, não incremento causal medido.

Escritores, leitores e cegueiras:

- **Chamadas**: escritor wrapper, linhas `ini` e `fim` ligadas por obra/hash/pid (`bin/codex_pergunta.sh:253`, `bin/codex_pergunta.sh:259`, `bin/codex_pergunta.sh:292`); leitor `codex_limite_estado._linhas_chamadas` (`bin/codex_limite_estado.py:49`), usado pelo gerador (`bin/gerar_codex_estado.py:41`). A duração começa antes de cópia, slots e trava da resposta, portanto inclui preparação/espera (`bin/codex_pergunta.sh:252`, `bin/codex_pergunta.sh:295`, `bin/codex_pergunta.sh:360`, `bin/codex_pergunta.sh:376`). Recusas anteriores ao `ini` ficam fora do anexo de encerradas (`bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:258`).
- **Triagem**: escritor único `--triagem`, após conferir sidecar, papel e SHA (`bin/disjuntor.py:869`, `bin/disjuntor.py:892`, `bin/disjuntor.py:910`); leitor `_triagens`, consumido por `decide_b3` (`bin/disjuntor.py:921`, `bin/disjuntor.py:981`). Não registra classe, commit da cura nem custo de cada achado (`bin/disjuntor.py:911`).
- **Disjuntor**: escritor `trilha`; detalhe limitado a 300 caracteres e falha de escrita absorvida (`bin/disjuntor.py:193`, `bin/disjuntor.py:199`, `bin/disjuntor.py:200`). B3 anuncia após gravar estado (`bin/disjuntor.py:1043`); leitor do ESTADO agrega eventos e tipos de liberação (`bin/disjuntor.py:1971`, `bin/disjuntor.py:1979`, `bin/disjuntor.py:1999`). Liberação por texto válido não certifica cura (`bin/disjuntor.py:1025`, `bin/disjuntor.py:1031`).
- **Limite**: escritor wrapper (`bin/codex_pergunta.sh:434`, `bin/codex_pergunta.sh:439`); leitor conta linhas válidas (`bin/codex_limite_estado.py:27`, `bin/codex_limite_estado.py:41`). Oito recusas no anexo, 609 s = 10,15 min nas chamadas correspondentes; são limite do serviço, não timeout do construtor (`limite.log:1-8`; `chamadas_fim.tsv`, 05/11 15:05:08–15:05:46).
- **Gitlog anexo**: produtor da exportação e consumidor específico não identificados na cópia [nao verificado]. O código contém leitores de histórico como `tickets_placar.sh`, mas isso não identifica quem produziu este anexo (`bin/tickets_placar.sh:54`). Assunto truncado não prova conteúdo do patch [nao verificado].

A regra de preverif, descarte anexado, suite antes do final e retorno ao preverif após reprovação está em `CLAUDE.md:372-381`; a própria regra exige postmortem por achado e distingue falha do Codex, triagem, ausência de pergunta e falta de alcance (`CLAUDE.md:382-388`). [nao verificado]

## 12/11 02:57 · sessao 27d0677041ae · - · - · origem=Ronald (tmux)

**8 — Catraca de número de linha**

- O selo é `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit`: igualdade literal entre `LINHAS_REGISTRADAS` e listas `i['linhas']`; deslocamento sem alteração semântica reprova. [app/core/tests/test_medida_turno_do_par.py:539]  
- Há **pelo menos 4 suites cujo único vermelho documentado foi essa catraca**: E2 (`93a48702`, texto igual nas 149 entradas), PAR-NAO-ADJACENTE (`795cafdf`, texto idêntico), SELOS-CANON-I1 (`f0103bef`, texto idêntico) e UI-PAINEL (`2ca185e0e`, linhas movidas pelo encurtamento de `api_me`). [app/docs/RELATO.md:560; app/docs/RELATO.md:553; app/docs/RELATO.md:547; app/docs/RELATO.md:46]
- As quatro falhas somam **88,637 min** e as quatro repetições verdes **89,889 min**; **8 suites observadas / 178,526 min**, das quais **4 repetições / 89,889 min** são custo incremental documentado. Fontes anexas: `suites_14d.tsv`, E2 `e2_rebase/suite_v2,v3`; PAR `suite_v3,v4`; SELOS `i1_suite_v4,v5`; UI `suite_v3,v4`. Os números foram calculados das durações respectivas `1153.712,1151.186,1382.738,1579.099,1478.109,1118.817,1303.664,1544.245` s.
- Outros disparos misturados com defeitos reais não permitem cobrar a suite inteira exclusivamente da catraca. [app/docs/RELATO.md:223; app/docs/RELATO.md:275; app/docs/RELATO.md:316]
- **MUDA**, proposta: identidade estrutural do sítio + conteúdo normalizado no lugar da posição absoluta. Perderia aviso de mera mudança de posição; preservaria controle de sítios novos/alterados. Ganho potencial medido no recorte: evitar as quatro repetições acima; resultado da mudança é [nao verificado].
- Quem escreve o registro é a alteração do teste em cada commit; quem lê é o teste de igualdade. O medidor produz as ocorrências por AST, e o ESTADO lê o mesmo medidor. [app/core/tests/test_medida_turno_do_par.py:17; app/core/tests/test_medida_turno_do_par.py:539; bin/turno_do_par_estado.py:28; bin/gerar_estado.py:299]
- Cegueira declarada: AST lexical, sem prova semântica, fluxo, tipos ou chamadas dinâmicas; posições não dizem se houve defeito. [app/core/medida_turno_do_par.py:18; app/core/medida_turno_do_par.py:32]
- Não confundir os dois disparos de E4a1 `CLASSIFICAR` com a subtrava de linhas: são registro de candidatos informativos. [app/docs/RELATO.md:381; app/docs/RELATO.md:386; app/core/tests/test_medida_turno_do_par.py:545]

**10 — `padrao=`**

- Classifica negócio por **caminho**: `app/` exceto `/tests/` e `app/docs/`; para `n/a (instrumento)`, não examina se o diff é comentário. [bin/tests/test_lei_akita.sh:122; bin/tests/test_lei_akita.sh:316]
- Há **2 commits só de comentário/anotação** documentados como barrados: `11f82cfec` e `6c6689036`; foram reescritos para `d75e5f2e1` e `ea1f0cab8`, somente campo `padrao=`, com árvores idênticas. **19 pares** tiveram diff vazio, incluindo **17 descendentes** refeitos; são custo formal, não cura de comportamento. [app/docs/BACKLOG.md:1906; app/docs/RELATO.md:319]
- Exemplo adicional de custo formal: SELOS-CANON-I1, **3 commits** reword com árvore igual, porque tocavam `esteira_vigia.py` e o caminho os classificava negócio. [app/docs/RELATO.md:483] [nao verificado]
- Exemplo misto: PUSH-PRESO-14 traz **4 commits**, sendo **3 R1-PUBLICA** de medida e **1 merge PAUTA-08**; não classificar todos como falsos positivos. [app/docs/RELATO.md:854; app/docs/RELATO.md:851]
- Disparos completos e minutos/suites atribuíveis **não medidos**: faltam logs dos portões com instante/commit/rc e duração; `chamadas_fim` não mede reword ou espera de push. [nao verificado] [nao verificado]
- Escritor da saída `PADRAO_FALHA`, `PADRAO_JULGADOS` é o próprio selo; leitores são pre-push e laço de selos da régua. [bin/tests/test_lei_akita.sh:755; bin/pre-push.sh:38; bin/regua.sh:138]
- **MUDA**, proposta: discriminar comentário/anotação por conteúdo, preservando exigência nos commits de comportamento. Sair perde verificação de vocabulário/declaração; ficar conserva os custos documentados de reword. A semântica de “realiza o padrão” continua dependendo de leitura, porque o código verifica termo e caminhos. [bin/tests/test_lei_akita.sh:307; bin/tests/test_lei_akita.sh:324; app/docs/LEIS.md:144]

**11 — Suite, trava e vigias** [nao verificado]

- `suite.sh` recusa inteira sem motivo `pre-opus <OBRA>`/`pouso <OBRA>` antes de montagem/docker; recortes também passam pela trava. [bin/suite.sh:154; bin/suite.sh:173; bin/suite.sh:221]
- A trava usa flock, espera padrão **3600 s**, rc **75** quando não consegue; escreve somente ocupante atual `.quem`, sem histórico de duração/fila. [bin/trava_teste.sh:21; bin/trava_teste.sh:35]
- O histórico `logs/trava_teste.hist` ainda é pendência, portanto custo de espera **não medido**. [app/docs/BACKLOG.md:2391]
- Unidade autorizada da TRAVA-SELO foi **somente `suite.sh`**; régua e pre-push continuam entradas próprias. [app/docs/BACKLOG.md:2385; bin/regua.sh:165; bin/pre-push.sh:142]
- **Vigia:** documentação registra **152 suites de 19/09–26/09**, depois pausado, e **0 suites/dia / 0 min hoje**; isso é registro documental fora da janela, não validação independente dos 14 dias. Fonte host citada não veio nos anexos. [app/docs/BACKLOG.md:2390; app/docs/RELATO.md:147]
- Vigia escreve logs por rodada, estado/falhas e histórico; lê ele mesmo para confirmação e transição; `placar_code.sh` lê estado/histórico e `esteira_vigia.py` lê falhas. [bin/vigia_arvore.sh:18; bin/vigia_arvore.sh:31; bin/vigia_arvore.sh:76; bin/vigia_arvore.sh:148; bin/vigia_arvore.sh:156; bin/placar_code.sh:526; bin/placar_code.sh:534; app/core/esteira_vigia.py:1196]
- **Régua relógio:** uma inteira por chamada, pela trava. **Régua calendário:** três inteiras por chamada, **fora da trava**, com banco próprio. [bin/regua_relogio.sh:51; bin/regua_calendario.sh:41]
- Documento registra **0 chamadores atuais** e último artefato em 19/10; sem logs dos 14 dias, custo do período **não medido**. [app/docs/BACKLOG.md:2390]
- TRAVA-SELO custou documentalmente **11 voltas**, embora a linha resuma “3 preverif Codex, 1 opus” e outras linhas enumerem preverif v6–v9 e reprova/aprova. Não promover esse resumo contraditório a total auditado. [app/docs/RELATO.md:141; app/docs/RELATO.md:154; app/docs/RELATO.md:157; app/docs/RELATO.md:173]
- **FICA** flock; **MUDA** observabilidade (pedido/concessão/fim por run) e tratamento da régua calendário. Sair perde serialização; ficar custa fila sem medição e guarda apenas do chamador obediente. [bin/trava_teste.sh:35; bin/suite.sh:8; bin/regua_calendario.sh:43]

**12 — SUITE-SO-NO-FIM** [nao verificado]

- O piloto foi registrado às **06:28** e **substituído “sem efeito” às 06:29** por TRAVA-MENOS-FILA-COM-SELO. Portanto não existe ensaio causal demonstrado do piloto. [app/docs/PROMPTS.md:2086; app/docs/PROMPTS.md:2087]
- Anexo: E3 em 11/11 contém **7 inteiras**, **4 FAILED / 3 OK**, **161,441 min**; E4B-ALCANCE contém **6 inteiras**, **4 FAILED / 2 OK**, **128,479 min**. Total **13 inteiras / 289,919 min**, custo observado das obras, **não custo causal do piloto**. Fontes `suites_14d.tsv`: E3 `suite_c8b,int,int2,int3,int4,pre_opus,pre_opus2`; E4B `suite1,2,3,4,c9,c10`.
- Falhas “só-na-final”, suites evitadas e espera economizada **não medidos**: faltam fase da volta e resultados dos recortes anteriores associados ao mesmo hash; o agregado não os fornece. [nao verificado]
- Há defeito real E3 identificado no opus mesmo após suite verde: marcador sem horizonte; cura `2e0810cd0` no gitlog anexado, pouso `072556ec8`. Não atribuir essa descoberta ao piloto. [app/docs/RELATO.md:138; `gitlog_14d.txt`, linhas de `2e0810cd0` e `072556ec8`]
- **MUDA**, proposta: manter a conduta final já implementada e medir fase/hash/testes novos revelados só na inteira antes de avaliar o tripwire. Sair amplia repetição de suites; ficar pode adiar descoberta de interação global, atualmente não quantificada. [bin/suite.sh:80; bin/suite.sh:133; app/docs/PROMPTS.md:2086]

## 12/11 02:57 · sessao b13ca50a7ce7 · - · - · origem=Ronald (tmux)

Consolidação das travas 4–7, somente por leitura.

| Trava | a) disparos | b) defeito real | c) sem defeito | d) custo | proposta e fonte |
|---|---|---|---|---|---|
| 4. Stop | **1.006 registros `BLOCK fila`**, 2 `BLOCK RELATO`, 3 `BLOCK ORCAMENTO`; 237 liberações por teto; 568 registros de trabalho em curso. Não são decisões finais distintas: `BLOCK fila` antecede a eventual liberação por teto. `hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:1404,1427–1448`. | **Não medido**: agregado não vincula bloqueio a achado/obra/cura. Falta log cru com decisão e resultado da triagem. `hook_stop_por_dia.tsv:1–78`. | **Não medido**: os 237 alarmes evidenciam o teto, sem provar ausência de defeito; os 568 eventos em curso também não provam falso positivo. `bin/hook_stop_fila1.py:1420–1448`; anexo:1–78. | Suites/minutos atribuíveis: **não medido**. Faltam identidade da chamada/suite e intervalo de espera por bloqueio. `bin/hook_stop_fila1.py:52–54`. | **MUDA**, proposta: leitura da lista autorizada e da ordem vigente; manter perde-se custo não medido de cobrança indevida; sair perde a contenção de encerramento com fila aberta. Fundamento: código conta toda tabela, não lista fechada. `bin/hook_stop_fila1.py:178–180,238–252,270–287`; `app/docs/CORTES.md:446`; `app/docs/BACKLOG.md:1758`. |
| 5. PreToolUse | Hook é **`bin/disjuntor.py pretool`**, registrado em `.claude/settings.json:62–69`. Número de invocações/negações do hook: **não medido**; anexo não traz censo de chamadas do hook, e contém eventos `via=wrapper` distintos. `bin/disjuntor.py:1802–1809,1832–1834`. | **Não medido**: falta vínculo negativa→achado confirmado→commit. `bin/disjuntor.py:1901–1907`. | **Não medido**: falta triagem das negativas. `bin/disjuntor.py:1901–1907`. | **Não medido**: faltam durações do hook e espera decorrente de cada negativa. `bin/disjuntor.py:193–199`. | **FICA**, com medição da própria porta e conferência da instalação efetiva; sair perde B1/B3/B4/CENSO, ficar custa verificação por ferramenta e negativas cujo custo não foi medido. `bin/disjuntor.py:1820–1872`; instalação da cópia não prova instalação efetiva no host [nao verificado]. |
| 6. ESPERANDO | **1.839 liberações com prova + 590 registros sem prova = 2.429 avaliações declaradas**, sem garantia de chamadas/esperas únicas. `hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:1386–1390`. | **Não medido**: prova de vida não é prova de defeito; falta resultado posterior por espera. `bin/hook_stop_fila1.py:789–838`. | **Não medido**: 590 “sem prova” não são automaticamente falsos positivos; podem representar processo morto, campo inválido, primeira observação ou arquivo parado. `bin/hook_stop_fila1.py:816–835,721–722`. | Suites/minutos evitados ou impostos: **não medido**. Faltam identificação/duração de cada espera e contrafactual sem trava. `bin/hook_stop_fila1.py:52–54`. | **FICA**, proposta: mantém liberação quando há produtor provado; retirar perde essa saída e pode retomar cobranças de fila durante espera; ficar mantém custo de prova/recusas não medido. `bin/hook_stop_fila1.py:1383–1390`. |
| 7. Uma vaga Codex por repo | **Não medido**: recusa por vaga ocupada ocorre antes da instalação do trap e do `ini`, ficando fora de `chamadas_fim.tsv`. `bin/codex_pergunta.sh:239–240,252–269,291–293`. | **Não medido**: falta trilha das recusas e confirmação de colisão evitada. `bin/codex_pergunta.sh:183,240`. | **Não medido**: falta tentativa recusada, motivo e tarefa que poderia prosseguir. `bin/codex_pergunta.sh:183,240`. | **Não medido**: `flock -n` recusa imediatamente; tempo perdido na retomada não é registrado. `bin/codex_pergunta.sh:240`. | **FICA**, proposta: registrar tentativa/recusa antes da admissão; sair perde exclusão entre construtores Codex no mesmo repositório; ficar custa serialização/retomada não medidas. A vaga Codex não implementa por si só “uma vaga, nada em paralelo” entre todos os tipos de construtor. `bin/codex_pergunta.sh:184–185,235–240`; `app/docs/BACKLOG.md:1758`. |

**Achado específico dos “937 andáveis”.** A causa está comprovada: `_item_parado()` exclui fechado e formas da regex da coluna de estado; a regex conhece congelado, fila 2, aguardando, PAREI, não construir e outras formas, mas não a lista fechada CONGELA/POS-20. `_itens_andaveis()` varre todas as linhas do bloco OBRAS; `_andaveis_fora_do_em_curso()` apenas remove o ID do marcador; o recado imprime o tamanho dessa lista e manda pegar outro item. A descrição em prosa de ORDEM-POS-E4A2 e a lista CONGELA não participam desse cálculo. `bin/hook_stop_fila1.py:95–97,166–180,238–252,270–287,1391–1393`; `app/docs/BACKLOG.md:1758`; `app/docs/CORTES.md:446`.

**Frequência desse recado: não medida.** O agregado só preserva `BLOCK fila 1 com item aberto`, sem o conteúdo de `item` nem `reason`: há 1.006 registros dessa classe, limite superior para a subclasse, mas não seu total. O valor 937 e o horário específico dependem do relato do prompt [nao verificado]; falta o log cru ou transcript daquela saída. Mesmo o log cru do hook corta `item` em 60 caracteres, limitando a recuperação do número. `hook_stop_por_dia.tsv:1–78`; `bin/hook_stop_fila1.py:52–54,285–287,1404`.

**Escritores, leitores e cegueiras.**

- **`logs/hook_stop_fila1.log`:** escritor `_trilha()` do Stop; registra instante, decisão, motivo truncado, item truncado e HEAD. Leitor operacional: B2, chamado pelo Stop. Cego a utilidade, falso positivo, duração e commit de cura; `BLOCK` pode preceder `ALLOW` da mesma execução. `bin/hook_stop_fila1.py:43–56,1202,1404,1448`; `bin/disjuntor.py:1263–1284`.
- **`logs/hook_stop_espera.json`:** Stop lê observação anterior, escreve mtime por caminho/inode e substitui atomicamente; primeira observação não prova avanço. É memória de prova, não histórico de espera nem resultado. `bin/hook_stop_fila1.py:681–709,721–723`.
- **Linha `ESPERANDO`:** wrapper emite no stderr para construtor; o Stop lê a linha do painel e avalia PID/arquivo. Autor de cada transcrição manual para o painel e persistência concreta no host [nao verificado]. `bin/codex_pergunta.sh:381–385`; `bin/hook_stop_fila1.py:798–836`.
- **`logs/sessao/disjuntor.log`:** escritor `trilha()` do disjuntor, com detalhe truncado em 300 caracteres; negativas PreToolUse são registradas antes da resposta e da tentativa de anotação no RELATO. Leitor `ler()` alimenta o ESTADO; também há leitura para casar CENSO. Não mede todas as invocações permitidas nem duração das negativas. `bin/disjuntor.py:193–206,1802–1809,1901–1907,1971–2008,2090`.
- **`logs/codex/chamadas.log`:** wrapper escreve `ini` e `fim`; leitores incluem `codex_limite_estado.py` e `gerar_codex_estado.py`. Cego às recusas pré-admissão, inclusive vaga ocupada: elas ocorrem antes de `INI=1`. `bin/codex_pergunta.sh:239–240,253–260,269,291–293`; `bin/codex_limite_estado.py:22,50`; `bin/gerar_codex_estado.py:41,153`.
- **`logs/construtor.vaga`:** wrapper abre append e toma lock exclusivo sem espera; o caminho deriva do git-common-dir compartilhado pelas worktrees. É objeto de exclusão, sem histórico de ocupação/recusas no próprio arquivo. `bin/codex_pergunta.sh:69–78,235–240`.

**Falha de instalação documentada:** a cópia relata que o PreToolUse da raiz não disparou em sessão iniciada em `app/`, permitindo que B3 só fosse observado pelo Stop após pousos; são exemplos documentais, não contagem completa de falhas dos 14 dias. `app/docs/RELATO.md:7539,7676`.

## 12/11 01:02 · sessao 85364df2b7f9 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVO a árvore c7 por dois bloqueios:** a grade pode contornar uma recusa de A2; e o status da folga parcial pode virar `ok` no app apesar do veredito `furo`. São contraexemplos de código; ocorrência desses estados em produção: **[não verificado]**. (`app/escala/utils.py:1208`; `app/ponto/precedencia.py:409`; `app/ponto/status_dia.py:157`; `app/ponto/status_dia.py:80`.)

1. **Escritores, leitores e efeito da condição nova**

   | Chave | Quem produz → quem coloca na ata |
   |---|---|
   | `tipo_dia` | Grade normal, overlay de folga e overlay de ausência/falta → `ata_do_dia`. (`app/escala/utils.py:1810`; `app/escala/utils.py:1816`; `app/escala/utils.py:1151`; `app/escala/utils.py:1220`; `app/escala/utils.py:1229`; `app/ponto/services/cartorio.py:729`.) |
   | `ausencia_tipo` | Overlay de ausência não falta → transporte para ata. (`app/escala/utils.py:1230`; `app/ponto/services/cartorio.py:766`.) |
   | `ausencia_id` | Overlay de falta e demais ausências → transporte para ata. (`app/escala/utils.py:1222`; `app/escala/utils.py:1232`; `app/ponto/services/cartorio.py:766`.) |
   | `cobertura_parcial` | `cobre_parte_do_dia` responde; grade carimba → ata transporta. (`app/ponto/turnos.py:2600`; `app/escala/utils.py:1190`; `app/escala/utils.py:1239`; `app/ponto/services/cartorio.py:757`.) |

   A persistência continua pela porta existente; o leitor da célula transporta as chaves da ata. Não encontrei segundo escritor nesse percurso. (`app/ponto/services/cartorio.py:1288`; `app/ponto/services/cartorio.py:1849`; `app/ponto/portas/celula.py:974`; `app/escala/services/leitor_celula.py:436`.)

   **Quem lê `dia_sem_trabalho`:** `_celula_furada`, premissa do chamado, fábrica, materialização, cluster espúrio, detector de intervalo e reconciliador; pelo adaptador do supra, também classificação, cartório e tarefa do dia. (`app/chamados/juizes.py:497`; `app/chamados/models.py:1284`; `app/chamados/services/fabrica_da_celula.py:112`; `app/chamados/services/fabrica_da_celula.py:201`; `app/chamados/services/materializacao.py:174`; `app/ponto/services/cluster_espurio.py:25`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; `app/ponto/management/commands/reconciliar_grade.py:282`; `app/ponto/supra_juiz.py:71`; `app/ponto/supra_juiz.py:495`; `app/ponto/services/cartorio.py:1251`; `app/chamados/services/tarefa_do_dia.py:207`; `app/chamados/services/tarefa_do_dia.py:462`; `app/chamados/services/tarefa_do_dia.py:474`.)

   **A condição nova preserva a cobrança da folga humana parcial produzida pela grade:** ela recebe `ausencia_tipo/id` e `cobertura_parcial`, portanto entra na exceção parcial; o marco fora do período continua faltando. Não encontrei nessa cadeia produtor de `falta+parcial` nem de `folga+parcial` sem ambas as chaves — combinações fabricadas diretamente pelo teste antigo. (`app/escala/utils.py:1219`; `app/escala/utils.py:1230`; `app/escala/utils.py:1239`; `app/escala/utils.py:1253`; `app/chamados/juizes.py:455`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:32`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:51`.)

   **Dia real cujo veredito ou dinheiro mudou contra `29e201d0c`: [não verificado].** O comparador disponível congela `f218…`, e P1 altera explicitamente o cadastro sintético para permitir parcial. Os mapas atuais preservam previsto, realizado limitado ao previsto e abono restante; isso não certifica dinheiro real nem todas as rubricas contra aquela base. (`app/ponto/tests/test_lastro_c5_sem_banco.py:27`; `app/ponto/tests/test_lastro_c5_sem_banco.py:598`; `app/ponto/services/dia_pago.py:392`.)

   **BLOQUEIA — leitor do status não migrado.** Contraexemplo: a configuração de P1, folga parcial de 120 minutos, 08h–10h, previsto 480 e marco das 17h ainda faltando. O supra mantém a acusação, mas o status lê prioritariamente `tipo_dia='folga'`:

   | Estado do dia | Veredito | Status c7 | Mesmo registro com `tipo_dia='ausencia'` |
   |---|---|---|---|
   | Sem batidas | `furo` | `folga` | `irregular` |
   | Realizado 180 e órfã às 16h | `furo` | `folga_trabalhada` | `irregular` |

   A construção está em `app/ponto/tests/test_lastro_c5_sem_banco.py:598`; os resultados decorrem de `app/ponto/supra_juiz.py:366`, `app/ponto/supra_juiz.py:396`, `app/ponto/status_dia.py:148`, `app/ponto/status_dia.py:157` e `app/ponto/status_dia.py:159`.

   Isso alcança a lavratura e o app: **`folga_trabalhada → ok`, enquanto `irregular → alerta`**. Contraria o próprio contrato de mostrar a acusação do restante da parcial. (`app/ponto/services/cartorio.py:1853`; `app/ponto/portas/celula.py:963`; [app/ponto/status_dia.py:80](/home/ronald/codex_ws/09abf29115766f6f068b1ab0f09b0e6fd8f08824/app/ponto/status_dia.py:80); `app/ponto/status_dia.py:135`.)

2. **`_coberto_inteiro_nb`: derivação paralela, sem identidade literal com outro juiz**

   Os juízes registrados já respondem cobertura para folha/cobrança; a parcialidade vem da grade. `_coberto_inteiro_nb` compõe novamente presença de cobertura e integralidade usando rótulo/chaves e o carimbo, decidindo impressão e substituição por `nunca_bateu`. **Fila**, pela dependência semântica de `tipo_dia`; não demonstrei erro produzido por essa composição em um dia válido. (`app/core/juizes.py:190`; `app/core/juizes.py:191`; `app/ponto/services/cartorio.py:1221`; `app/ponto/services/cartorio.py:1231`; `app/ponto/services/cartorio.py:1239`; `app/ponto/services/cartorio.py:1255`; `app/ponto/services/cartorio.py:1285`.)

   Não é correto chamá-lo de cópia exata de `ausencia_explica_o_dia_inteiro`: **falta aprovada integral** dá verdadeiro no predicado NB e falso naquele juiz, que exclui falta explicitamente. Também não equivale a `dia_sem_trabalho`: folga sem ausência pode ser dia sem trabalho sem representar cobertura. São perguntas distintas; essa diferença, sozinha, não prova segundo juiz indevido. (`app/ponto/turnos.py:2677`; `app/ponto/services/cartorio.py:1239`; `app/chamados/juizes.py:459`.)

3. **Atalho de `_marcar_ausencias`**

   **Não achei divergência do `tipo_dia` traduzido entre as faixas conhecidas `!= folga`.** São `ferias`, `afastamento` e `atestado`, todas superiores a feriado/folga; outra ausência superior continua traduzindo para `ausencia`. Li mapa, ordem e tradução. (`app/ponto/precedencia.py:46`; `app/ponto/precedencia.py:52`; `app/escala/utils.py:1228`.)

   **BLOQUEIA — contorna a recusa de A2.** Contraexemplo concreto: duas ausências aprovadas no mesmo dia, mesma data, `fantasma_xyz` pk1 e `atestado` pk2. Cobertura escolhe atestado pelo maior pk; c7 valida apenas esse tipo e escreve `ausencia`. A2 percorre ambas e levanta `TipoSemFaixa`. A equivalência dos rótulos conhecidos não preserva a recusa. (`app/ponto/turnos.py:2753`; `app/ponto/turnos.py:2781`; [app/escala/utils.py:1208](/home/ronald/codex_ws/09abf29115766f6f068b1ab0f09b0e6fd8f08824/app/escala/utils.py:1208); `app/escala/utils.py:1209`; `app/ponto/precedencia.py:298`; `app/ponto/precedencia.py:409`; `app/ponto/precedencia.py:105`.)

   O vigia pode denunciar o cadastro desconhecido, mas seu contador não transforma o resultado da grade em recusa daquele dia. Existência desse par em produção: **[não verificado]**. (`app/ponto/services/vigia_ausencia.py:163`; `app/ponto/services/vigia_ausencia.py:168`.)

4. **Férias: `None`, avisos e silêncio**

   O único consumidor produtivo de `_dia_de_descanso` é `_vespera`, que efetivamente trata `None` como falso e omite a data. Porém as duas entradas produtivas passam a lista: o resultado completo conserva o aviso vermelho `tipo_sem_faixa`. Nesse percurso há **recusa nomeada da conclusão**, não fundamento para concluir “não é repouso”. (`app/ferias/situacao.py:96`; `app/ferias/situacao.py:115`; `app/ferias/situacao.py:251`; `app/ferias/situacao.py:325`.)

   Os avisos são retornados/indexados, publicados pela API e preservados na remarcação, que remove somente `colisao`. Não encontrei leitor produtivo usando `avisos=None`. (`app/ferias/situacao.py:297`; `app/ferias/situacao.py:304`; `app/ferias/situacao.py:360`; `app/api/views_mensageria.py:844`; `app/api/views_mensageria.py:846`; `app/ferias/views.py:496`; `app/ferias/views.py:522`.)

   **Fila — interface permite fallback silencioso:** `_vespera(colab, inicio)` sem lista, com `TipoSemFaixa` nos dois dias, devolve `[]` e perde os avisos. A assinatura permite isso; uso produtivo atual desse caminho: **[não verificado]**. (`app/ferias/situacao.py:72`; `app/ferias/situacao.py:97`; `app/ferias/situacao.py:107`.)

   **Fila — `except` que engole permanece:** `RuntimeError` na leitura vira `False` sem aviso, comportamento exigido pelo teste novo. A view também zera avisos diante de qualquer exceção. A captura genérica de `_dia_de_descanso` já existia no delta recebido. (`app/ferias/situacao.py:100`; `app/ponto/tests/test_lastro_c7_sem_banco.py:124`; `app/ferias/views.py:499`.)

5. **Versões e horizonte**

   **Ata comp11 efetivamente lavrada sob v5 em algum ambiente: [não verificado].** Os 21 casos do harness são sintetizados e partem de regra antiga **4**. (`app/ponto/tests/test_lastro_c7_sem_banco.py:149`; `app/ponto/tests/test_lastro_c7_sem_banco.py:157`.)

   **Fila condicional:** se houver impressão gravada com regra5, voltar à regra4 muda o hash e provoca rejulgamento sem outro fato novo. A versão entra na impressão; os dois ramos só pulam quando ela coincide. Portanto “sem bump conserva FOTO” exige distinguir atas v4 de eventuais atas v5. (`app/ponto/services/cartorio.py:490`; `app/ponto/services/cartorio.py:1254`; `app/ponto/services/cartorio.py:1256`; `app/ponto/services/cartorio.py:1397`; `app/ponto/services/cartorio.py:1405`.)

   `versao_para` é lido por `_regra_do_dia`, que alimenta esses dois hashes. `HORIZONTE_DO_FATO` continua literal em **21/09/2026**, no modelo e na migration; não é recalculado automaticamente pela tupla. (`app/ponto/services/cartorio.py:1185`; `app/chamados/models.py:2403`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:28`.)

   Leem esse horizonte: constraints do modelo, guarda/constraints da migration, recorte e grupos do fio, backfill e deduplicação. Ele delimita quais perguntas entram nessas operações e garantias de unicidade. (`app/chamados/models.py:3486`; `app/chamados/models.py:3492`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:33`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:57`; `app/chamados/services/fio.py:1412`; `app/chamados/services/fio.py:1947`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:277`; `app/chamados/management/commands/dedup_perguntas_fato.py:78`.)

   Remover v5 restaura a igualdade exigida pelo teste entre o literal e o início da competência do maior horizonte declarado; não altera fisicamente o índice por si só. (`app/ponto/services/cartorio.py:132`; `app/chamados/tests/test_e1_v6_horizonte.py:36`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:57`.)

6. **O que os testes mordem** [nao verificado]

   **Mordem implementações atuais extraídas por AST**, incluindo corpos originais das noturnas e B5u/S2S3. Não são somente texto congelado. Entretanto ORM, módulos externos, entrada da grade e persistência são substituídos; não certificam a integração cadastro → batidas → banco. (`app/ponto/tests/test_lastro_c5_sem_banco.py:39`; `app/ponto/tests/test_lastro_c5_sem_banco.py:111`; `app/ponto/tests/test_lastro_c5_sem_banco.py:214`; `app/ponto/tests/test_lastro_c5_sem_banco.py:576`; `app/ponto/tests/test_lastro_c5_sem_banco.py:583`; `app/ponto/tests/test_lastro_c7_sem_banco.py:79`; `app/ponto/tests/test_lastro_c7_sem_banco.py:178`.)

   **Sim, o JSON pode divergir e o GREEN continuar. Fila — procedência sem mordida:** o carregador confere a string de revisão e devolve `ast`; não verifica `sha256_fonte`. Contraexemplo: alterar somente esse SHA não muda nenhuma asserção desse carregador. A divergência normal entre uma base antiga e a árvore atual é esperada; a lacuna é certificar que o congelado corresponde à revisão afirmada. (`app/ponto/tests/lastro_c3_fontes.json:5`; `app/ponto/tests/test_lastro_c5_sem_banco.py:33`.)

   Os recortes c6 só substituem funções na variante RED; a instalação ocorre pelo `__main__`, não pelo GREEN normal de unittest. E P1 verifica juiz/supra da parcial, mas não seu status — por isso deixa passar o primeiro bloqueio. (`app/ponto/tests/test_lastro_c7_sem_banco.py:31`; `app/ponto/tests/test_lastro_c7_sem_banco.py:204`; `app/ponto/tests/test_lastro_c5_sem_banco.py:613`; `app/ponto/tests/test_lastro_c5_sem_banco.py:623`.)

7. **Consolidação pelas classes de reprovação**

   - **Derivação paralela de fonte única — bloqueia:** item 3, grade conclui onde A2 recusaria. (`app/escala/utils.py:1209`; `app/ponto/precedencia.py:409`.)
   - **Campo com dois significados/leitor não migrado — bloqueia:** item 1, `folga` da faixa passa a ser lida como dia inteiro sem jornada no status. (`app/ponto/status_dia.py:42`; `app/ponto/status_dia.py:148`.)
   - **Segundo juiz — não demonstrei identidade indevida no NB:** a comparação com o juiz de “explica inteiro” difere legitimamente na falta; fica a derivação em fila descrita no item 2. (`app/ponto/services/cartorio.py:1239`; `app/ponto/turnos.py:2677`.)
   - **Selo sem mordida — fila:** identidade do congelado; adicionalmente, P1 não cobre o status parcial. (`app/ponto/tests/test_lastro_c5_sem_banco.py:33`; `app/ponto/tests/test_lastro_c5_sem_banco.py:623`.)
   - **`except` que engole — fila:** retorno silencioso para erros diferentes de `TipoSemFaixa`. (`app/ferias/situacao.py:100`; `app/ferias/views.py:499`.)
   - **Segundo escritor fora da porta — sem achado no percurso examinado:** montagem da ata e lavratura continuam separadas pela porta existente. (`app/ponto/services/cartorio.py:721`; `app/ponto/portas/celula.py:974`.)
   - **Relógio fora da borda — sem achado no delta examinado:** `versao_para` usa a competência da data recebida; as mudanças de cobertura e captura examinadas não introduzem leitura de relógio. (`app/ponto/services/cartorio.py:249`; `app/escala/utils.py:1208`; `app/ferias/situacao.py:96`.)

## 12/11 01:00 · sessao 9b3ed64a1791 · - · - · origem=Ronald (tmux)

**Confirmado: bloqueia — leitor do status não migrou para a folga parcial.** O status ignora `cobertura_parcial` quando `tipo_dia='folga'`, contrariando seu próprio contrato de mostrar a acusação do resto e a invariante de concordância com o veredito. (`app/ponto/status_dia.py:18`, `app/ponto/status_dia.py:135`, `app/ponto/status_dia.py:141`, `app/ponto/status_dia.py:148`, `app/ponto/status_dia.py:157`.)

Reprodução em memória, usando corpos atuais e a configuração sintética de P1: folga aprovada de 120 minutos, 08h–10h; jornada prevista de 480 minutos; marcos 08h/17h; saída ainda `missing`. (`app/ponto/tests/test_lastro_c5_sem_banco.py:598`, `app/ponto/tests/test_lastro_c5_sem_banco.py:600`, `app/ponto/tests/test_lastro_c5_sem_banco.py:606`.)

| Batidas | Supra/veredito | Status atual | Mesmo registro rotulado `ausencia` |
|---|---|---|---|
| Nenhuma | `FURO_SEM_COBRANCA` → `furo` | `folga` | `irregular` |
| 180 minutos; órfã às 16h | `FURO_PARCIAL` → `furo` | `folga_trabalhada` | `irregular` |

Os resultados decorrem dos ramos de parcial do supra, do mapa de vereditos e dos ramos prioritários de folga no status. (`app/ponto/supra_juiz.py:366`, `app/ponto/supra_juiz.py:396`; `app/escala/models.py:1612`; `app/ponto/status_dia.py:148`, `app/ponto/status_dia.py:157`, `app/ponto/status_dia.py:159`.)

O efeito alcança a lavratura: o cartório transporta os fatos do status; a porta chama `status_da_lavra` e grava o resultado junto do veredito. (`app/ponto/services/cartorio.py:1853`; `app/ponto/portas/celula.py:951`, `app/ponto/portas/celula.py:963`, `app/ponto/portas/celula.py:1017`, `app/ponto/portas/celula.py:1038`.) No app, a divergência vira **`ok` em vez de `alerta`**; o rótulo também afirma “sem jornada prevista” apesar dos 480 previstos. (`app/ponto/status_dia.py:36`, `app/ponto/status_dia.py:42`, `app/ponto/status_dia.py:80`, `app/ponto/status_dia.py:81`.)

Classificação: **leitor não migrado / campo com dois significados**, não segundo escritor. (`app/ponto/status_dia.py:117`, `app/ponto/status_dia.py:148`; `app/ponto/portas/celula.py:976`.) P1 verifica grade, ata, juiz e supra, mas não o status parcial; seu selo permanece verde diante desse defeito. (`app/ponto/tests/test_lastro_c5_sem_banco.py:588`, `app/ponto/tests/test_lastro_c5_sem_banco.py:613`, `app/ponto/tests/test_lastro_c5_sem_banco.py:623`.)

Existência dessa configuração de cadastro em produção: **[não verificado]**. A fixture altera explicitamente a capacidade parcial do tipo. (`app/ponto/tests/test_lastro_c5_sem_banco.py:598`.)

## 12/11 00:58 · sessao f7cfc522e417 · - · - · origem=Ronald (tmux)

3. **Não encontrei divergência de `tipo_dia` para faixas conhecidas `!= folga`.** Todas são `ferias`, `afastamento` ou `atestado`, acima de feriado/folga; outra ausência superior também traduz `ausencia`. Li mapa, ordem e tradução. (`app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:52`, `app/escala/utils.py:1228`)

**BLOQUEIA — bypass da recusa / derivação paralela.** Contraexemplo reproduzido em memória: duas ausências aprovadas no mesmo dia, `fantasma_xyz` pk1 e `atestado` pk2. Cobertura escolhe atestado pela recência; grade c7 valida somente atestado e escreve `ausencia`; A2 percorre ambas e levanta `TipoSemFaixa`. A otimização preserva o rótulo dos conhecidos, mas elimina uma recusa que A2 produziria. (`app/ponto/turnos.py:2753`, `app/ponto/turnos.py:2781`, `app/escala/utils.py:1208`, `app/escala/utils.py:1209`, `app/ponto/precedencia.py:298`, `app/ponto/precedencia.py:409`, `app/ponto/precedencia.py:105`)

A reprodução usou o harness e seus doubles, sem ORM; existência desse cenário em produção é [não verificado]. (`app/ponto/tests/test_lastro_c5_sem_banco.py:183`, `app/ponto/tests/test_lastro_c5_sem_banco.py:243`)

4. **O consumidor booleano realmente colapsa `None` com falso:** `_vespera` não inclui aquela data nos repousos. Porém ambos os leitores produtivos passam uma lista e recebem o aviso vermelho `tipo_sem_faixa`; o resultado completo preserva a recusa nomeada. Não encontrei consumidor produtivo que use `avisos=None`. (`app/ferias/situacao.py:96`, `app/ferias/situacao.py:115`, `app/ferias/situacao.py:166`, `app/ferias/situacao.py:251`, `app/ferias/situacao.py:320`, `app/ferias/situacao.py:325`)

Os avisos saem no retorno/indexação, são publicados pela API e preservados pela remarcação, que remove somente `colisao`. Portanto não há base para interpretar a ausência de aviso `vespera` como autorização; a API inclusive manda responder com observações, nunca “pode/não pode”. (`app/ferias/situacao.py:297`, `app/ferias/situacao.py:304`, `app/ferias/situacao.py:360`, `app/api/views_mensageria.py:844`, `app/api/views_mensageria.py:846`, `app/api/views_mensageria.py:847`, `app/ferias/views.py:496`, `app/ferias/views.py:522`)

**FILA — interface permite silêncio.** `_vespera(colab, inicio)` sem lista, diante de `TipoSemFaixa` nos dois dias, retorna `[]` e perde todos os avisos. Esse caminho é permitido pelas assinaturas, mas não identifiquei leitor produtivo atual que o utilize. (`app/ferias/situacao.py:72`, `app/ferias/situacao.py:97`, `app/ferias/situacao.py:107`, `app/ferias/situacao.py:115`)

**FILA — `except` que engole permanece:** qualquer outra exceção em `fatos_do_dia` vira `False` sem aviso; por exemplo, um `RuntimeError` de leitura. O teste novo exige esse comportamento. A remarcação também transforma qualquer exceção na obtenção dos avisos em lista vazia. Ocorrência real e introdução dessas capturas pela obra: [não verificado]. (`app/ferias/situacao.py:100`, `app/ponto/tests/test_lastro_c7_sem_banco.py:124`, `app/ferias/views.py:499`)

## 12/11 00:58 · sessao 6721847f8e52 · - · - · origem=Ronald (tmux)

5. **v5 gravada em ambiente real: [não verificado].** O código atual declara v4 (`app/ponto/services/cartorio.py:132`). Se existir célula comp11 gravada sob v5, voltar a v4 altera o hash mesmo sem fato novo: a versão integra a impressão (`app/ponto/services/cartorio.py:490`), os dois ramos calculam a versão atual (`app/ponto/services/cartorio.py:1254`, `app/ponto/services/cartorio.py:1397`) e só pulam quando o hash coincide (`app/ponto/services/cartorio.py:1256`, `app/ponto/services/cartorio.py:1405`). Contraexemplo executado em memória com o harness: regra antiga4 → pulada; antiga5 → carimbada, ata `ausencia`→`folga`; construção desse cenário em `app/ponto/tests/test_lastro_c5_sem_banco.py:539`, `app/ponto/tests/test_lastro_c5_sem_banco.py:579`. **Fila condicional**, sem prova de alcance real. [não verificado quanto à existência em ambiente]

`versao_para` tem um chamador de produção, `_regra_do_dia`, que alimenta os dois hashes (`app/ponto/services/cartorio.py:1185`, `app/ponto/services/cartorio.py:1254`, `app/ponto/services/cartorio.py:1397`). `HORIZONTE_DO_FATO` permanece literal em 21/09/2026, tanto no modelo quanto na migration (`app/chamados/models.py:2403`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:28`). Seus leitores são:

- Índices únicos do modelo (`app/chamados/models.py:3483`, `app/chamados/models.py:3492`).
- Guarda e índices da migration (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:31`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:39`, `app/chamados/migrations/0053_pergunta_unica_por_familia.py:57`).
- Recorte do fio, grupos e contadores (`app/chamados/services/fio.py:1412`, `app/chamados/services/fio.py:1436`, `app/chamados/services/fio.py:1947`).
- Backfill e deduplicação (`app/chamados/management/commands/backfill_decisoes_pergunta.py:104`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:277`, `app/chamados/management/commands/dedup_perguntas_fato.py:78`).
- Conferência do corte empresarial, cuja divergência recusa o apply (`app/chamados/management/commands/backfill_decisoes_pergunta.py:145`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:242`).

O bump não move automaticamente esse literal nem o índice; quebra o selo que exige literal = início da competência do maior horizonte declarado (`app/chamados/tests/test_e1_v6_horizonte.py:36`). Remover v5 restaura essa igualdade (`app/ponto/services/cartorio.py:132`, `app/chamados/models.py:2403`).

6. **Os GREEN executam código atual, mas não a integração real.** Sem revisão, `fonte` lê a árvore (`app/ponto/tests/test_lastro_c5_sem_banco.py:39`); `carregar` compila funções AST selecionadas (`app/ponto/tests/test_lastro_c5_sem_banco.py:111`). C7 roda corpos originais das noturnas e B5u/S2S3 (`app/ponto/tests/test_lastro_c7_sem_banco.py:79`, `app/ponto/tests/test_lastro_c7_sem_banco.py:178`). ORM, módulos, grade de entrada e persistência são substituídos (`app/ponto/tests/test_lastro_c5_sem_banco.py:214`, `app/ponto/tests/test_lastro_c5_sem_banco.py:576`, `app/ponto/tests/test_lastro_c5_sem_banco.py:583`).

**Fila — procedência do congelado sem mordida:** o JSON declara revisão e SHA, mas o carregador confere apenas a string da revisão e devolve `ast`; não valida SHA (`app/ponto/tests/lastro_c3_fontes.json:2`, `app/ponto/tests/lastro_c3_fontes.json:5`, `app/ponto/tests/test_lastro_c5_sem_banco.py:33`). Contraexemplo: adulterar só `sha256_fonte` não altera nenhum comportamento ou assert desse carregador (`app/ponto/tests/test_lastro_c5_sem_banco.py:35`). O congelado é c3/f218, não evidência da base29 (`app/ponto/tests/test_lastro_c5_sem_banco.py:27`, `app/ponto/tests/lastro_c3_fontes.json:2`). Os recortes c6 só substituem funções na variante RED, instalada pelo `__main__`; não participam do GREEN normal (`app/ponto/tests/test_lastro_c7_sem_banco.py:31`, `app/ponto/tests/test_lastro_c7_sem_banco.py:204`).

As 21 atas são sintetizadas e exercitam o ramo sem adesão, com batidas substituídas por vazio; não provam as 21 atas reais nem cadastro+batidas (`app/ponto/tests/test_lastro_c7_sem_banco.py:149`, `app/ponto/tests/test_lastro_c5_sem_banco.py:563`). Executei c5+c7: 30 testes passaram, sem bytecode/escrita; resultado da execução desta sessão, sem arquivo de registro. [não verificado por registro em arquivo]

## 12/11 00:58 · sessao 9b3ed64a1791 · - · - · origem=Ronald (tmux)

Itens 1/2 — conclusões:

- **Folga humana parcial segue cobrando o resto.** A grade obtém a ausência aprovada pelo juiz `cobertura_ausencia_periodo`, calcula parcial pelo juiz `cobre_parte_do_dia`, escreve `tipo_dia`, `ausencia_tipo/id` e `cobertura_parcial`, e conserva o marco fora do período como `missing`; a ata transporta esses campos, portanto a condição nova entra na exceção parcial. (`app/escala/utils.py:1188`, `app/escala/utils.py:1228`, `app/escala/utils.py:1239`, `app/escala/utils.py:1253`; `app/ponto/services/cartorio.py:757`, `app/ponto/services/cartorio.py:766`; `app/chamados/juizes.py:455`.)
- **Não encontrei produtor real nesta cadeia para os casos que c7 passou a excetuar:** `falta+parcial` e `folga+parcial` sem ambas as chaves. O ramo `falta` não escreve carimbo parcial; a folga humana escreve as duas chaves antes do carimbo. Os testes antigos fabricam diretamente aquelas combinações. (`app/escala/utils.py:1219`, `app/escala/utils.py:1230`, `app/escala/utils.py:1239`; `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:32`, `app/chamados/tests/test_emis_ausencia_parcial_cobra.py:51`.)
- **Mudança comprovada de um dia real contra `29e201d0c`: [não verificado].** O congelado disponível declara revisão `f218d86a719dbdfab1979856aba769c9198cb43a`, não a base pedida. (`app/ponto/tests/lastro_c3_fontes.json:2`.) O teste parcial é sintético e altera o cadastro da fixture para permitir horas; não prova a configuração real desses tipos. (`app/ponto/tests/test_lastro_c5_sem_banco.py:593`, `app/ponto/tests/test_lastro_c5_sem_banco.py:598`.)

**Escritores:**

| Chave | Origem → ata → persistência |
|---|---|
| `tipo_dia` | Grade normal trabalho/folga (`app/escala/utils.py:1810`, `app/escala/utils.py:1816`), overlay folga lançada (`app/escala/utils.py:1151`), overlay ausência/falta (`app/escala/utils.py:1220`, `app/escala/utils.py:1229`) → `ata_do_dia` (`app/ponto/services/cartorio.py:729`). |
| `ausencia_tipo` | Overlay ausência não falta (`app/escala/utils.py:1230`) → transporte para ata (`app/ponto/services/cartorio.py:766`). |
| `ausencia_id` | Falta (`app/escala/utils.py:1222`) e demais ausências (`app/escala/utils.py:1232`) → transporte para ata (`app/ponto/services/cartorio.py:766`). |
| `cobertura_parcial` | Juiz `cobre_parte_do_dia` (`app/ponto/turnos.py:2585`, `app/ponto/turnos.py:2600`) alimenta grade (`app/escala/utils.py:1190`, `app/escala/utils.py:1239`) → ata (`app/ponto/services/cartorio.py:757`). |

A lavratura chama a porta existente nos dois ramos; a atribuição de `cel.ata` continua na porta. (`app/ponto/services/cartorio.py:1288`, `app/ponto/services/cartorio.py:1849`; `app/ponto/portas/celula.py:974`, `app/ponto/portas/celula.py:1006`, `app/ponto/portas/celula.py:1033`.) O leitor reconstrói os campos da ata sem consultar novamente a cobertura. (`app/escala/services/leitor_celula.py:436`, `app/escala/services/leitor_celula.py:439`.)

**Leitores de `dia_sem_trabalho`:** `_celula_furada`, premissa do chamado, fábrica, materialização, cluster espúrio, detector de intervalo e reconciliador; por adaptador, supra juiz, cartório e tarefa do dia. (`app/chamados/juizes.py:497`; `app/chamados/models.py:1284`; `app/chamados/services/fabrica_da_celula.py:112`, `app/chamados/services/fabrica_da_celula.py:201`; `app/chamados/services/materializacao.py:174`; `app/ponto/services/cluster_espurio.py:25`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; `app/ponto/management/commands/reconciliar_grade.py:280`; `app/ponto/supra_juiz.py:71`, `app/ponto/supra_juiz.py:495`; `app/ponto/services/cartorio.py:1251`; `app/chamados/services/tarefa_do_dia.py:207`, `app/chamados/services/tarefa_do_dia.py:462`, `app/chamados/services/tarefa_do_dia.py:474`.)

**Item 2 — derivação paralela, com ressalva de pergunta.** `_coberto_inteiro_nb` reconstrói “há cobertura integral” de rótulo/chaves e carimbo, embora a cobertura já tenha sido obtida por `_ac_nb` e sua parcialidade pela grade; seu resultado decide hash e substituição do veredito por `nunca_bateu`. (`app/ponto/services/cartorio.py:1221`, `app/ponto/services/cartorio.py:1231`, `app/ponto/services/cartorio.py:1239`, `app/ponto/services/cartorio.py:1255`, `app/ponto/services/cartorio.py:1285`.) As autoridades de cobertura existentes estão registradas. (`app/core/juizes.py:190`, `app/core/juizes.py:191`, `app/core/juizes.py:253`.)

Não é correto afirmar igualdade literal com `ausencia_explica_o_dia_inteiro`: esse juiz **exclui falta**, enquanto `_coberto_inteiro_nb` inclui falta. (`app/ponto/turnos.py:2677`; `app/ponto/services/cartorio.py:1239`.) Tampouco é igual a `dia_sem_trabalho`: folga sem ausência pode ser dia sem trabalho e não cobertura. (`app/chamados/juizes.py:459`; `app/ponto/services/cartorio.py:1239`.)

**Gravidade sugerida: fila**, pela inferência de cobertura a partir de `tipo_dia`; um contraexemplo real produzido pela árvore que altere resultado por essa duplicação permanece **[não verificado]**. O caso concreto de divergência entre predicados é uma falta aprovada integral: `_coberto_inteiro_nb=True`, `ausencia_explica_o_dia_inteiro=False`, distinção explicitamente codificada; tratá-los como a mesma pergunta produziria reprovação falsa. (`app/ponto/services/cartorio.py:1239`; `app/ponto/turnos.py:2677`.)

## 12/11 00:31 · sessao 28ee7fe73b79 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

c7 construída na cópia, sem bump e sem commit, conforme a proibição inicial. As 16 regressões receberam tratamento; o `test_05` permanece intacto, pendente do corte sobre o PDF. [Relato por vermelho](docs/RELATO.md:9).

Validação local:

- **61 testes sem banco GREEN**, mais a CATRACA de linhas GREEN. [Execuções](docs/lastro_faixa_c7_execucoes.txt:6102).
- **RED da c6:** 27 falhas de asserção, zero erros. [Evidência](docs/lastro_faixa_c7_execucoes.txt:6172).
- **22 mutantes mortos**, com comandos individuais registrados. [Resumo](docs/RELATO.md:63).

Propagação de `TipoSemFaixa`, proposta restritiva para o PDF e quem escreve/lê cada campo estão no [relato](docs/RELATO.md:35).

A suite Django dos 17, o teto SQL e o opus final seguem **[não verificado]**. O Python local não tem Django. [Registro](docs/lastro_faixa_c7_execucoes.txt:6879).

## 12/11 00:19 · sessao 9443b128960e · - · - · origem=Ronald (tmux)

Os 3 FAIL de impressão são causados por duplos que declaram `tipo_dia='ausencia'` sem fornecer as chaves que c6 passou a ler (`ponto/services/cartorio.py:1237`; `ponto/tests/test_e3_nb_impressao_memoria.py:365`, `:401`; `chamados/tests/test_e3_pos_suite_sem_banco.py:139`).

- Reproduzi os 3 FAIL com `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria chamados.tests.test_e3_pos_suite_sem_banco -q`: 31 testes, 3 FAIL. [nao verificado]
- Em diagnóstico exclusivamente em memória, fornecer `ausencia_tipo`/`ausencia_id` aos duplos com cobertura fez os mesmos 31 testes passarem, preservando todas as asserções. A grade real fornece essas chaves (`escala/utils.py:1228`).
- Não há frase do aval autorizando mudar o hash B5u. Inverter essas asserções seria indevido. A decisão da sessão “Cobrança e dinheiro passaram a ler a CHAVE de cobertura” sustenta atualizar as entradas dos testes; isso precisa ser registrado como adaptação do contrato, sem mudar o comportamento caracterizado (`ponto/services/cartorio.py:1227`).

`dia_sem_trabalho` é regressão de código: a exceção parcial passou a alcançar qualquer rótulo (`chamados/juizes.py:455`). Cura sugerida: falta continua dia sem trabalho; folga só entra na exceção parcial quando há chave de cobertura. Isso preserva o teste antigo com carimbo espúrio (`chamados/tests/test_emis_ausencia_parcial_cobra.py:50`) e o P1 com folga humana parcial real (`ponto/tests/test_lastro_c5_sem_banco.py:609`). Mantenha `ausencia_parcial_cobra_o_resto` lendo apenas o carimbo, pois o contrato da ata antiga exige isso (`ponto/tests/test_lastro_c5_sem_banco.py:621`).

CATRACA: o censo em memória encontrou as mesmas identidades e quatro deslocamentos no registro (`core/tests/test_medida_turno_do_par.py:247`):

| Sítio | Antes → c6 atual |
|---|---|
| `escala/utils.py::montar_grade_prevista_periodo_por_turno` | `[1480,1500,1510,1523,1534,1820]` → `[1484,1504,1514,1527,1538,1824]` |
| `ponto/services/cartorio.py::_julgar_colab_corpo` | `[1191,1344]` → `[1198,1351]` |
| `ponto/services/espelho.py::autoridade_do_periodo` | `[1029]` → `[1032]` |
| `ponto/services/fechamento.py::_so_a_data` | `[1569]` → `[1568]` |

Recalcule esses números depois das curas; a igualdade é cobrada em `core/tests/test_medida_turno_do_par.py:539`. Nenhum arquivo foi escrito por este explorer.

## 12/11 00:18 · sessao ef9964a52222 · - · - · origem=Ronald (tmux)

Mapa na c6, por leitura estática; execução com banco não realizada [não verificado].

- Origem: `faixa_de` levanta `TipoSemFaixa` em `ponto/precedencia.py:105`; `fatos_do_periodo` chama para cada ausência em `ponto/precedencia.py:409`.
- Grade: `_marcar_ausencias` consulta o período sem captura em `escala/utils.py:1205`; builder chama o overlay em `escala/utils.py:1852`; contrato dos renders chama o builder em `escala/utils.py:2341`.
- PDF: `_coletar_dados_espelho` chama a grade sem captura em `relatorios/pdf_espelho.py:475`; PDF informacional propaga em `relatorios/pdf_espelho.py:1046`; PDF mensal propaga em `relatorios/pdf_espelho.py:1093`. O traceback fornecido confirma a interrupção desse coletor.
- Tela: o primeiro caminho pelo builder tem captura geral em `ponto/services/espelho.py:569`, mas a consulta independente de A2 em `ponto/services/espelho.py:649` recaptura e levanta especificamente em `ponto/services/espelho.py:651`. Propaga por `espelho_do_colab` em `ponto/services/espelho.py:1265`, tela web em `ponto/views.py:158` e API em `api/views.py:1354`.
- Cartório: julgamento consulta o builder sem captura em `ponto/services/cartorio.py:1168`, chamado em `ponto/services/cartorio.py:990`. Outra leitura direta, `aplicar_precedencia_na_celula`, consulta A2 em `ponto/services/cartorio.py:1992`, mas seu chamador captura e registra em `ponto/services/cartorio.py:2042`.
- Folha: **não há propagação direta por essa grade**. `grade_do_fechamento` lê a ata por `grade_da_celula` em `folha/export.py:335` e `folha/export.py:338`; prontidão consome essa grade em `folha/export.py:1095`. Falha anterior na lavratura pode impedir atualização, mas impacto efetivo não medido [não verificado].
- Semana do posto: `_fato_do_dia` recaptura e levanta em `escala/services/semana_do_posto.py:178`; sua leitura ocorre em `escala/services/semana_do_posto.py:151`.

Forma **já existente** que conserva o nome sem derrubar seu leitor: vigia captura `TipoSemFaixa`, acumula os códigos e devolve o contador `tipos_ativos_sem_faixa` em `ponto/services/vigia_ausencia.py:163` e `ponto/services/vigia_ausencia.py:168`. Não encontrei representação de dia recusado nas grades/PDF; os únicos tratamentos específicos de produção aparecem nos locais acima [não verificado quanto a mecanismos indiretos].

Proposta restritiva: manter `faixa_de` recusando, manter `test_05` intacto e registrar a decisão pendente sobre representação da recusa no PDF. Usar apenas o contador como justificativa para continuar julgando o dia inventaria uma resposta que ele não fornece (`ponto/services/vigia_ausencia.py:168`).

Férias é regressão: contrato explícito exige leitura sem exceção (`ferias/situacao.py:1`, `ferias/situacao.py:22`, `ferias/situacao.py:38`); c6 introduziu `raise` em `ferias/situacao.py:96`, que viola o selo AST em `ferias/test_a3_situacao_de_ferias.py:739`. A estrutura existente `_aviso` permite preservar a recusa nomeada sem derrubar o leitor (`ferias/situacao.py:64`): capturar o erro, anexar aviso vermelho com seu texto nos caminhos de situação/agendamento, sem concluir descanso para o dia não julgado (`ferias/situacao.py:245`, `ferias/situacao.py:322`). Isso requer transportar avisos ao helper; não exige campo de modelo. Nenhum arquivo foi alterado.

## 11/11 23:53 · sessao 9bd2d7f7b5cd · FIO-E0-V4-E4B · verificador · origem=Code

A pré-verificação tem pendências: o selo aceita formas concretas de escrita fora da porta; há caminho estático para Pauta por limbo sem competência exportada; e a explicação dos tetos não fecha a aritmética do cenário com três chamados. (app/chamados/tests/e4b_c12_ast.py:247; app/chamados/services/disputa_emissao.py:1841; app/chamados/services/fecho_runtime.py:119; app/ponto/tests/test_selo_chokepoint_escrita.py:210)

Não achei no material fornecido a tabela comparativa da corrente da manhã e do cartório emp. 2/3/4 exigida pelo aval; resultado desse DIFF, ORM, concorrência e identidade Git independente permanecem **[não verificado]**. A própria entrega distingue o snapshot textual da identidade Git e declara ORM/PROD não verificados. (app/docs/FIO-E0-V4-E4B-c12.txt:3; app/docs/FIO-E0-V4-E4B-c12.txt:4)

**1. Censo**

Não achei outro escritor corrente de **fechamento/resolução** fora da porta e dos nove sítios internos nominais. O corpo nominal inclui o gravador e os gates; o selo procura chamadas adicionais fora desse corpo. Isso não prova ausência de reflexão ou SQL arbitrário. (app/chamados/tests/e4b_c12_ast.py:18; app/chamados/tests/e4b_c12_ast.py:220; app/chamados/tests/e4b_c12_ast.py:249; completude dinâmica **[não verificado]**)

Para a pergunta literal sobre **qualquer escrita de status**, existem chamadores não terminais fora da porta e da tabela S/E. `analisar` e `reabrir` chegam ao gravador, mas não constam de `GATES`; seus eventos levam a `em_analise`, portanto não são fechadores/resolvedores do aval. (app/chamados/models.py:807; app/chamados/models.py:1443; app/chamados/tests/e4b_c12_ast.py:14; app/chamados/catalogo/motor.py:112)

| Operação fora da porta | Chamadores encontrados |
|---|---|
| `analisar` | app/chamados/views.py:1513; app/ponto/services/regularizacao.py:137; app/chamados/services/disputa_emissao.py:1085, :1159, :1178, :1223, :1252, :1379; app/chamados/management/commands/geofence_registrados_para_analise.py:32 |
| `reabrir` | app/chamados/models.py:1020; app/chamados/views_arquivar.py:70; app/ponto/management/commands/reconciliar_grade.py:530; app/ponto/management/commands/detectar_ausencias.py:302; app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49; app/chamados/services/disputa_emissao.py:1851, :2011; app/chamados/services/competencia_trancada.py:266; app/chamados/services/acoes_disputa.py:270; app/chamados/services/adesao.py:93; app/chamados/management/commands/reabrir_orfas_enterradas.py:172 |

As migrations escrevem fora da porta: 0038 altera aberto/registrado; 0039 faz o backfill de estado/via. Elas estão nominalizadas no selo separado. A aplicação anterior em PROD e a afirmação de tabela vazia em instalação nova aparecem como comentário, sem prova correspondente no dado raspado. (app/chamados/migrations/0038_a3_registrado_backfill.py:10; app/chamados/migrations/0038_a3_registrado_backfill.py:15; app/chamados/migrations/0039_a3_via_backfill.py:24; app/chamados/migrations/0039_a3_via_backfill.py:33; app/chamados/tests/test_e4b_c12_selo.py:93; app/chamados/tests/test_e4b_c12_selo.py:97)

O selo principal só percorre `app/` e exclui migrations e testes. Não achei outro fechador terminal literal nas buscas de views/API, cascatas, comandos e scripts externos; censo irrestrito de tasks dinâmicas, aliases interprocedurais e SQL: **[não verificado]**. (app/chamados/tests/e4b_c12_ast.py:11; app/chamados/tests/e4b_c12_ast.py:34)

**2. Equivalência dos sítios**

O ramo `ato=` executa o callable sem lhe acrescentar argumentos e devolve seu resultado exato. Assim, os `user` externos não transformam `reconciliar()` em ato humano nem `encerrar()` deixa de usar sua via original. (app/chamados/services/fecho_runtime.py:165; app/chamados/services/fecho_runtime.py:169; app/chamados/models.py:655; app/chamados/models.py:1620)

A comparação reversível em memória recuperou o AST dos arquivos anteriores, descontados os imports, contra o snapshot textual usado pelo runner; equivalência contra o commit Git real **[não verificado]**. O runner carrega essa base textual. (app/chamados/tests/e4b_c12_verificar.py:17; app/chamados/tests/e4b_c12_verificar.py:18)

Na tabela, “booleano” significa que a seleção, condição ou retorno continua usando o booleano original; “descartado” significa que o chamador não consome o retorno. A preservação desse retorno está no ramo da porta. (app/chamados/services/fecho_runtime.py:165; app/chamados/services/fecho_runtime.py:169)

| Sítio | Callable/operação e uso mantidos | Código |
|---|---|---|
| S01 | `reconciliar()`, booleano | app/chamados/management/commands/auditar_chamados_orfaos.py:68 |
| S02–S04 | `transicionar(destino, usuário)`, retorno descartado; sem `ato=` | app/chamados/management/commands/smoke_chamados.py:135, :148, :164 |
| S05 | `encerrar()`, booleano | app/chamados/reconciliador.py:497 |
| S06 | `reconciliar()`, booleano | app/chamados/reconciliador.py:524 |
| S07–S08 | `reconciliar()`, booleano | app/chamados/services/adesao.py:116, :151 |
| S09 | `encerrar_por_competencia_trancada()`, booleano | app/chamados/services/competencia_trancada.py:185 |
| S10 | `reconciliar()`, booleano | app/chamados/services/feriado_reavaliar.py:48 |
| S11 | `reconciliar()`, booleano | app/chamados/services/limbo_folgas.py:102 |
| S12–S13 | `reconciliar()`, booleano; conserva rollback entre tentativas | app/chamados/services/limbo_folgas.py:284, :286, :287 |
| S14 | `reconciliar()`, descartado | app/chamados/services/materializacao.py:583 |
| S15 | `reconciliar()`, booleano devolvido | app/chamados/services/materializacao.py:772 |
| S16 | `encerrar()`, descartado | app/chamados/services/regularizacao_ext.py:206 |
| S17 | `reconciliar()`, booleano | app/chamados/services/silencio_ausencia.py:102 |
| S18 | `reconciliar()`, booleano | app/chamados/signals.py:43 |
| S19 | `reconciliar()`, booleano | app/colaboradores/management/commands/marcar_feriado_do_posto.py:141 |
| S20 | `encerrar()`, booleano combinado com status | app/colaboradores/services/desligamento.py:156 |
| S21 | `reconciliar()`, booleano | app/colaboradores/services/pos_vinculo.py:43 |
| S22 | `encerrar()`, descartado | app/colaboradores/services/vinculo.py:388 |
| S23 | `cancelar(motivo=...)`, booleano; motivo e `ch=ch` conservados | app/escala/management/commands/fechar_avisos_regua_na_lista.py:48 |
| S24 | `reconciliar()`, booleano | app/ponto/management/commands/curar_residuo_bug115.py:152 |
| S25 | `reconciliar()`, descartado | app/ponto/management/commands/detectar_ausencias.py:159 |
| S26–S28 | `reconciliar()`, booleano | app/ponto/management/commands/detectar_ausencias.py:825, :835, :846 |
| S29 | `reconciliar()`, booleano; dry continua em `reconciliaria()` | app/ponto/management/commands/detectar_intervalo_ausente.py:139 |
| S30 | `fechar(sistema, resolucao=...)`, `None` descartado; defaults capturam `ch/dia/te` | app/ponto/management/commands/fechar_chamados_meio_periodo.py:110 |
| S31 | `cancelar(motivo=...)`, booleano; motivo e `ch=ch` conservados | app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58 |
| S32 | `reconciliar()`, booleano | app/ponto/services/ausencia.py:695 |
| S33–S38 | `reconciliar()`, booleano nas seleções/condições | app/ponto/services/cartorio.py:987, :1512, :1525, :1546, :1624, :1639 |
| S39 | `reconciliar()`, booleano | app/ponto/services/cluster_espurio.py:125 |
| S40 | `reconciliar()`, atribuído a `_fechou` | app/ponto/services/cobertura.py:123 |
| S41 | `reconciliar()`, booleano | app/ponto/services/cobertura.py:318 |
| S42 | `reconciliar()`, booleano; serviço continua devolvendo chamado/`None` | app/ponto/services/entrada_sincronizada.py:32 |
| S43 | `reconciliar()`, booleano | app/ponto/services/lastro.py:401 |
| S44 | `reconciliar()`, booleano | app/ponto/services/triagem_batida.py:598 |
| S45 | `reconciliar()`, booleano | app/ponto/services/veredito_celula.py:203 |
| S46 | `transicionar('em_analise', user)`, descartado; sem `ato=` | app/chamados/services/acoes_chamado.py:238 |

E01–E09 descartam o retorno da porta; não achei consumidor desses sítios confundindo contagem de Pautas com booleano ou objeto. (app/chamados/models.py:455; app/chamados/services/acoes_chamado.py:71, :226, :283, :110, :152, :322; app/chamados/services/fio.py:722; app/colaboradores/views.py:2069)

**A equivalência de ordem não é absoluta em S30.** Antes, a porta interna de `fechar()` entregava a Pauta e depois vinha a nota; agora a reentrada interna reutiliza o coletor, a nota é escrita e o wrapper externo entrega a Pauta depois. Estado continua antes da nota, mas Pauta e nota trocam de ordem, e a nota passa a integrar o atomic externo da porta. (app/chamados/models.py:455; app/chamados/models.py:458; app/chamados/services/fecho_runtime.py:173; app/chamados/services/fecho_runtime.py:168; app/ponto/management/commands/fechar_chamados_meio_periodo.py:110)

Não achei outra troca entre escritas antigas dos sítios; equivalência operacional com ORM e concorrência: **[não verificado]**. (app/docs/FIO-E0-V4-E4B-c12.txt:10)

**3. Porta, reentrada e atomicidade**

| Caso | Comportamento do código |
|---|---|
| Reentrada imediata do mesmo chamado | Executa o gate usando o coletor atual; não abre outro atomic nem entrega outro lote. (app/chamados/services/fecho_runtime.py:158; app/chamados/services/fecho_runtime.py:173) |
| Outro chamado aninhado | Abre seu atomic e coletor; restaura o anterior antes de entregar seu lote. (app/chamados/services/fecho_runtime.py:161; app/chamados/services/fecho_runtime.py:167) |
| A→B→A | A comparação é somente com o chamado do coletor corrente; o segundo A, enquanto B está corrente, abre outro lote. Não existe mapa global de chamados ativos. (app/chamados/services/fecho_runtime.py:156; app/chamados/services/fecho_runtime.py:159) |
| Exceção que sai de `ato()` | Restaura o contexto e pula `_escrever_pautas`; a exceção sai pelo atomic da porta. (app/chamados/services/fecho_runtime.py:164; app/chamados/services/fecho_runtime.py:168) |
| Falha na entrega da Pauta | Propaga fora do S143 e sai pelo mesmo atomic do ato. (app/chamados/services/fecho_runtime.py:161; app/chamados/services/fecho_runtime.py:168) |
| Chamador captura essa falha | A captura externa ocorre depois da saída do atomic da porta; o receiver S88 é exemplo de chamador que captura. (app/chamados/signals.py:42; app/chamados/signals.py:47) |
| Chamador já em atomic | A porta pede atomic com savepoint padrão; os gates automáticos internos continuam com `savepoint=False`. (app/chamados/services/fecho_runtime.py:161; app/chamados/models.py:651; app/chamados/models.py:1613) |
| Autocommit | O atomic externo da porta delimita ato e entrega; não se pode chamar seu custo de SAVEPOINT/RELEASE em todos os contextos. (app/chamados/services/fecho_runtime.py:161; comportamento efetivo do backend **[não verificado]**) |

Há caminhos de **transição sem Pauta**:

- O S143 captura falhas do resolver. O coletor só incorpora as recusas depois de o contexto e o savepoint saírem sem erro; portanto uma falha pode preservar o fechamento e descartar a coleta. Isso é consequência explícita do best-effort existente. (app/chamados/signals.py:116; app/chamados/signals.py:118; app/chamados/services/fecho_runtime.py:35; app/chamados/services/fecho_runtime.py:40)
- Pergunta sem dia de turno não ancora Pauta; via `competencia_trancada` também sai antes do processamento normal do resolver. (app/chamados/services/fecho_runtime.py:120; app/chamados/services/fecho_runtime.py:122; app/chamados/services/disputa_emissao.py:1901)
- Sem recusa coletada, não há entrega. (app/chamados/services/fecho_runtime.py:118; app/chamados/services/fecho_runtime.py:128)

A porta também **não exige uma transição bem-sucedida para escrever**: entrega o lote independentemente de `resultado`, inclusive `False`. O teste de memória exige justamente esse contrato. Nos gates produtivos inventariados, não achei um caso comprovado de Pauta sem nenhuma transição no ato; essa garantia não é imposta pela porta. (app/chamados/services/fecho_runtime.py:165; app/chamados/services/fecho_runtime.py:168; app/chamados/tests/test_e4b_c12_memoria.py:27; app/chamados/tests/test_e4b_c12_memoria.py:31)

O retorno antecipado de `_escrever_pautas` só retira o atomic vazio quando nenhuma recusa tem âncora. O atomic externo continua cobrindo o ato; não achei mudança de rollback ou reentrada causada por esse retorno. (app/chamados/services/fecho_runtime.py:128; app/chamados/services/fecho_runtime.py:130; app/chamados/services/fecho_runtime.py:161)

**4. Quem escreve, quem lê e significado**

O quadro abaixo identifica escritores e famílias de leitores encontradas; não certifico censo irrestrito de todos os leitores dinâmicos/SQL. **[não verificado]**

| Campo/valor | Escritores | Leitores identificados |
|---|---|---|
| `status_local` | Atribuição e persistência em app/chamados/models.py:228, :296; nascimento em :1096; simulação restaurada em :356, :371; migrations citadas no item 1 | Gates/tripwire em app/chamados/models.py:165, :219, :221; S143 em app/chamados/signals.py:97, :105; destino em app/chamados/juizes.py:760; fio em app/chamados/services/fio.py:389, :432; fila em app/chamados/services/na_fila.py:17; painel em app/chamados/services/painel.py:213; cartório em app/ponto/services/cartorio.py:1358, :1616; cascatas em app/colaboradores/services/desligamento.py:153, :156 e app/colaboradores/services/vinculo.py:386 |
| `status_local`, outros consumidores | Mesmo escritor acima | app/chamados/reconciliador.py:247, :840; app/ponto/management/commands/detectar_ausencias.py:153, :194, :259; app/api/views.py:320, :1044, :1972; app/api/views_core.py:309; app/chamados/views.py:976, :1477; app/colaboradores/services/calendario.py:437; app/relatorios/views.py:745, :920; app/relatorios/pdf_chamados.py:116; app/chamados/management/commands/auditar_invariantes_chamados.py:573, :1068 |
| `status_local`, apresentação | Mesmo escritor acima | app/templates/chamados/detalhe_local.html:68; app/templates/chamados/detalhe.html:36; app/templates/chamados/fio_colaborador.html:167; app/templates/chamados/partials/_lista_chamados.html:33; app/templates/chamados/partials/_card_atendimento.html:13; app/templates/chamados/partials/modal_fio.html:98; app/templates/chamados/partials/painel_chamado_turno.html:18; app/templates/relatorios/historico_chamados.html:121 |
| `via_encerramento` | app/chamados/models.py:245, :249; backfill em app/chamados/migrations/0039_a3_via_backfill.py:24 | app/chamados/models.py:715, :1008, :1343, :1376; app/chamados/views_arquivar.py:64, :165; app/ponto/services/furos_sem_cobranca.py:69; app/chamados/services/disputa_emissao.py:1874, :1901; app/chamados/services/competencia_trancada.py:194, :260, :271; app/chamados/services/adesao.py:91; app/chamados/management/commands/auditar_invariantes_chamados.py:1070; app/chamados/management/commands/reabrir_orfas_enterradas.py:135 |
| `resolvido_em` | app/chamados/models.py:244, :248 | app/chamados/models.py:243, :715, :1343, :1347, :1376; app/chamados/services/disputa_emissao.py:1861; app/chamados/services/painel.py:65, :245, :249; app/chamados/services/verbos.py:230, :237; app/chamados/services/fila.py:109; app/chamados/views_arquivar.py:166; app/chamados/management/commands/auditar_invariantes_chamados.py:574; app/chamados/management/commands/reabrir_orfas_enterradas.py:134; app/templates/relatorios/historico_chamados.html:127; app/templates/chamados/detalhe_local.html:88; app/templates/chamados/detalhe.html:56 |
| `Pauta.ancora_id/texto` | E4B monta em app/chamados/services/fecho_runtime.py:58, :120; INSERT delegado em app/pautas/services.py:180; resposta copia âncora em :199 | Dedup em app/chamados/services/fecho_runtime.py:136; lista/âncora em app/pautas/services.py:322, :336; API em app/pautas/views.py:40, :47; apresentação em app/templates/core/_copiloto.html:405, :570 e app/templates/colaboradores/detalhe.html:119; suporte em app/pautas/management/commands/suporte_hasner.py:109 |
| `_RECUSAS` | Default, `set`, `reset` e atualização do mapa em app/chamados/services/fecho_runtime.py:13, :20, :35, :39, :40, :163, :167, :179, :183 | Somente o runtime: app/chamados/services/fecho_runtime.py:18, :30, :156 |
| `PerguntaDisputa.via_resolucao/materializada_em` | Atribuição conjunta em app/chamados/models.py:3098; persistem `carimbar` em :3155, identidade em :3191, descarimbo em :3213, lote em :3265; troca isolada de via em :3276 | C6 em app/chamados/juizes.py:662, :674, :689, :1342, :1368, :1374, :1405; estado/SQL em app/chamados/models.py:3044, :3056, :3351; materializador em app/chamados/services/materializacao.py:435, :439, :626, :1090, :1373; validação em app/chamados/services/validacao.py:188, :461, :472 |
| Carimbo da pergunta, outros consumidores | Mesmas portas acima | app/chamados/services/disputa_emissao.py:1908, :2040; app/chamados/reconciliador.py:353, :367, :1144, :1495; app/chamados/veredito_lavrado.py:203; app/chamados/services/fio.py:1101, :1168, :1244, :1351, :1434, :1854; app/chamados/services/passivo_validado.py:21, :64, :140; app/chamados/services/relavra_via.py:42, :58, :66, :181, :244; app/chamados/services/fabrica_da_celula.py:149, :171; app/chamados/services/tarefa_do_dia.py:227; app/ponto/services/cartorio.py:1623 |
| Carimbo da pergunta, tela/auditoria/reparo | Mesmas portas acima | app/chamados/services/painel.py:67; app/chamados/services/raiox.py:135; app/chamados/services/historico.py:103; app/chamados/views.py:1257; app/templates/chamados/partials/_card_atendimento.html:182; app/chamados/management/commands/auditar_invariantes_chamados.py:433, :470, :955, :1710; app/chamados/management/commands/auditar_disputas.py:71, :150; app/chamados/management/commands/reabrir_orfas_enterradas.py:67, :129, :142; app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70, :147; app/chamados/management/commands/relavrar_via_i6.py:32, :58; app/chamados/management/commands/backfill_decisoes_pergunta.py:101, :314, :325, :330 |

Os significados relevantes não são todos intercambiáveis:

- `ENCERRADOS` é resolvido/fechado; `TERMINAIS` acrescenta cancelado/superado; `NAO_FECHADOS` inclui resolvido. A porta coleta por `ENCERRADOS`, não por todo terminal. (app/chamados/catalogo/motor.py:30; app/chamados/catalogo/motor.py:31; app/chamados/catalogo/motor.py:32; app/chamados/services/fecho_runtime.py:158)
- `resolvido_em` preserva o instante anterior em resolvido→fechado, mas `via_encerramento` é sobrescrita. Portanto o par não necessariamente descreve o mesmo ato terminal; isso já está no núcleo existente. (app/chamados/models.py:242; app/chamados/models.py:245)
- A competência da Pauta usa o mês do **fim da janela**, com corte empresarial; não usa simplesmente o mês civil da pergunta. As datas inicial e final representam o intervalo corte…corte−1. (app/chamados/services/fecho_runtime.py:124; app/chamados/services/fecho_runtime.py:125; app/ponto/janelas.py:10; app/ponto/janelas.py:24)
- Conversa usa dia civil em `data_evento`, e o juiz devolve `None` para seu dia de turno. O runtime respeita isso; `_vivas_entre` ainda consulta competência pelo `data_evento` cru. Essa divergência é preexistente e impede afirmar uniformidade total. (app/chamados/juizes.py:332; app/chamados/services/fecho_runtime.py:120; app/chamados/models.py:364)
- `materializada_em` não prova plantio de batida: a porta de identidade grava `admin_decidiu` sem plantio, embora o help text ainda diga “batida criada + chamado fechado”. O C6 lê via e carimbo juntos. (app/chamados/models.py:3396; app/chamados/models.py:3170; app/chamados/juizes.py:1368)
- `_RECUSAS` contém identidades e motivos, sem grandeza temporal. O runtime não acrescenta relógio próprio; os relógios alcançados continuam nos escritores de estado/carimbo/contador. O fuso configurado é São Paulo, com `USE_TZ=True`. (app/chamados/services/fecho_runtime.py:19; app/chamados/models.py:244; app/chamados/models.py:3186; app/pautas/services.py:362; app/pautas/services.py:366; app/confi

[truncado em 20000 de 31158 caracteres]

## 11/11 23:49 · sessao 78d66b49c516 · - · - · origem=Ronald (tmux)

**Item 4 — escritores/leitores e semântica**

- `status_local`: escritor persistente canônico `_gravar_estado`, atribuição em `app/chamados/models.py:228`, `save` em `:296`; INSERT de nascimento em `:1096`; simulação com restauração em `:356`/`:371`. Os gates usam o motor existente, e seus significados continuam separados: VIVOS, COBRANCA, ENCERRADOS, TERMINAIS e NAO_FECHADOS em `app/chamados/catalogo/motor.py:24-32`; autoridades registradas em `app/core/juizes.py:273-279`.
- Leitores de `status_local`, por famílias: gate/tripwire `app/chamados/models.py:165`, `:219`, `:221`, `:364`; receiver `app/chamados/signals.py:97`, `:105`; C6/destino `app/chamados/juizes.py:760`; fio/fila `app/chamados/services/fio.py:389`, `:432`, `app/chamados/services/na_fila.py:17`; painel `app/chamados/services/painel.py:213-224`; cartório `app/ponto/services/cartorio.py:1358`, `:1616`; encerramentos/cascatas `app/colaboradores/services/desligamento.py:153`, `:156`, `app/colaboradores/services/vinculo.py:386`; emissores/reconciliadores `app/ponto/management/commands/detectar_ausencias.py:153`, `:194`, `:259`, `app/chamados/reconciliador.py:247`, `:840`; API/telas `app/api/views.py:320`, `:1044`, `:1972`, `app/api/views_core.py:309`, `app/chamados/views.py:976`, `:1477`, `app/colaboradores/services/calendario.py:437`; relatórios `app/relatorios/views.py:745`, `:920`, `app/relatorios/pdf_chamados.py:116`; auditorias `app/chamados/management/commands/auditar_invariantes_chamados.py:573`, `:1068`; templates `app/templates/chamados/detalhe_local.html:68`, `app/templates/chamados/detalhe.html:36`, `app/templates/chamados/fio_colaborador.html:167`, `app/templates/chamados/partials/_lista_chamados.html:33`, `app/templates/chamados/partials/_card_atendimento.html:13`, `app/templates/chamados/partials/modal_fio.html:98`, `app/templates/chamados/partials/painel_chamado_turno.html:18`, `app/templates/relatorios/historico_chamados.html:121`. Catálogo acima resume famílias; exaustividade de leitores, sobretudo SQL/dinâmicos, **[não verificado]**.
- `via_encerramento`: escrito em `app/chamados/models.py:245`, limpo em `:249`; significado “quem/como deixou de ser vivo”, fonte em `app/chamados/catalogo/motor.py:86-96`. Leitores: `app/chamados/models.py:1008`, `:1343`; `app/chamados/views_arquivar.py:64`, `:165`; `app/ponto/services/furos_sem_cobranca.py:69-70`; `app/chamados/services/disputa_emissao.py:1874`, `:1901`; `app/chamados/services/competencia_trancada.py:260`; `app/chamados/services/adesao.py:91`; auditoria em `app/chamados/management/commands/auditar_invariantes_chamados.py:1070`; relatório de reparo em `app/chamados/management/commands/reabrir_orfas_enterradas.py:135`.
- `resolvido_em`: instante da primeira saída de vivo; `resolvido→fechado` preserva carimbo, em `app/chamados/models.py:242-244`; reabertura limpa em `:248`. Leitores: proteção contra reabertura `app/chamados/models.py:1343-1347`; dedup de nota `app/chamados/services/disputa_emissao.py:1861`; janela civil/deslizante inclusiva `>=` em `app/chamados/services/painel.py:235-252`; `app/chamados/services/verbos.py:230`, `:237`; `app/chamados/services/fila.py:109`; ordenação `app/chamados/views_arquivar.py:166`; auditoria `app/chamados/management/commands/auditar_invariantes_chamados.py:574`; templates `app/templates/relatorios/historico_chamados.html:127`, `app/templates/chamados/detalhe_local.html:88`, `app/templates/chamados/detalhe.html:56`.
- `Pauta.ancora_id/texto`: E4B monta texto em `app/chamados/services/fecho_runtime.py:58-83`, competência em `:120-126`; único INSERT delegado em `app/pautas/services.py:180-185`; resposta copia âncora em `:199`. Semântica empresa+competência documentada em `app/pautas/models.py:34`, `:84-89`. Leitores: dedup `app/chamados/services/fecho_runtime.py:136-137`; lista/chip `app/pautas/services.py:322`, `:336`; API `app/pautas/views.py:40-41`, `:47`; UI `app/templates/core/_copiloto.html:405`, `:570-573`; ficha `app/templates/colaboradores/detalhe.html:119`; suporte `app/pautas/management/commands/suporte_hasner.py:109-113`. Competência usa mês do **fim** da janela, com intervalo inclusivo corte..corte−1 em `app/ponto/janelas.py:10-32`, `:60-63`.
- `_RECUSAS`: somente em `app/chamados/services/fecho_runtime.py`: default `:13`, leituras `:18`, `:30`, `:156`, atribuições/restaurações `:35`, `:39`, `:163`, `:167`, `:179`, `:183`; significado `{chamado:pk, perguntas:{pk:(pergunta,motivo)}}` em `:19-20`, `:34`, `:162`; não contém instante/unidade temporal.
- `PerguntaDisputa.materializada_em/via_resolucao`: atribuição central conjunta em `app/chamados/models.py:3098-3102`; persistência pelas portas `carimbar`, `carimbar_identidade`, `descarimbar` e lote em `:3155`, `:3191`, `:3213`, `:3265-3267`; exceção nominal troca apenas via no fato já materializado em `:3276-3277`. C7 chama `fechar_por_admin` em `:1922`, usando o próprio instante no registro em `:1926`; S143 chama `carimbar` em `app/chamados/services/disputa_emissao.py:2060`.
- Leitores principais desses campos: C6 `app/chamados/juizes.py:662`, `:674`, `:689`, `:1342`, `:1368`, `:1374`, `:1405`; estado/SQL de vivas `app/chamados/models.py:3044`, `:3056`, `:3351-3353`; materializador `app/chamados/services/materializacao.py:435`, `:439`, `:626-633`, `:1090`, `:1373`; validação `app/chamados/services/validacao.py:188-190`, `:461`, `:472`; S143 `app/chamados/services/disputa_emissao.py:1908`, `:2040`; reconciliador `app/chamados/reconciliador.py:353`, `:367`, `:1144`, `:1495`; veredito lavrado `app/chamados/veredito_lavrado.py:203-204`; fio `app/chamados/services/fio.py:1101`, `:1168`, `:1244`, `:1351`, `:1434`, `:1854`; passivo `app/chamados/services/passivo_validado.py:21`, `:64`, `:140-142`; relavra `app/chamados/services/relavra_via.py:42`, `:58`, `:66`, `:181`, `:244-252`; fábrica `app/chamados/services/fabrica_da_celula.py:149`, `:171`; tarefa `app/chamados/services/tarefa_do_dia.py:227-228`; cartório `app/ponto/services/cartorio.py:1623`; painel/raiox/histórico `app/chamados/services/painel.py:67`, `app/chamados/services/raiox.py:135`, `app/chamados/services/historico.py:103`; tela `app/chamados/views.py:1257`, `app/templates/chamados/partials/_card_atendimento.html:182`, `:186`; auditorias `app/chamados/management/commands/auditar_invariantes_chamados.py:433`, `:470`, `:955`, `:1710`, `app/chamados/management/commands/auditar_disputas.py:71`, `:150`; reparadores `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `:129`, `:142`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70-72`, `:147`, `app/chamados/management/commands/relavrar_via_i6.py:32`, `:58-68`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:101`, `:314`, `:325`, `:330`.

**Achados para itens 4/5**

1. **Pauta sem exportada é permitida pelo código.** Limbo sozinho produz `recusa:credor_carrega_limbo:...`; o sufixo exportada é opcional em `app/chamados/services/disputa_emissao.py:1835-1846`; S143 coleta todo esse prefixo em `:1948-1952`; `_escrever_pautas` exige dia válido, sem exigir exportada no motivo, em `app/chamados/services/fecho_runtime.py:119-126`, `:139-141`. A origem humana `resolvido→fechado`, agora coletada em `:173-184`, permite alcançar isso sem guarda de origem viva em `app/chamados/models.py:165`. A carga inclui irmã limbo mesmo morta, porque `app/chamados/services/disputa_emissao.py:1798-1801` usa `_ancoradas`, cujo filtro não exige viva em `app/chamados/models.py:344-347`; a pergunta alvo pode continuar viva pela célula furada em `app/chamados/juizes.py:1378-1381`. Ocorrência PROD e execução ORM **[não verificado]**.
2. **Dois usos de data preexistentes:** gate lê `data_evento` cru em `app/chamados/models.py:364`; runtime usa juiz que devolve `None` para conversa em `app/chamados/services/fecho_runtime.py:120`, `app/chamados/juizes.py:332-336`. Assim, dia civil de conversa pode ser tratado como exportado na guarda e não ancorar Pauta. Não é mudança nova no diff fornecido; ocorrência **[não verificado]**.
3. **Ambiguidade documental preexistente:** `materializada_em` diz “batida criada + chamado fechado” em `app/chamados/models.py:3394-3396`, mas `carimbar_identidade` registra decisões sem plantio em `:3170-3191`; C6 lê via+carimbo como identidade em `app/chamados/juizes.py:1360-1369`. Não achei novo significado introduzido pelo diff nesses campos.
4. **Relógio novo no runtime: não achei.** O alcance novo passa pelos relógios preexistentes do núcleo/carimbo em `app/chamados/models.py:244`, `:3125`, `:3186` e contador em `app/pautas/services.py:362`, `:366`; timezone configurado `America/Sao_Paulo`/`USE_TZ=True` em `app/config/settings/base.py:93`, `:95`.
5. **Juiz duplicado no runtime: não achei:** composição chama juiz do dia e janela em `app/chamados/services/fecho_runtime.py:113`, `:116`, `:120-125`, sem regra própria de vitalidade; autoridades registradas em `app/core/juizes.py:273-292`.
6. Ordem canônica existente C→D→Ch→P em `app/chamados/models.py:2630-2694`; nova entrega acrescenta contador DP→âncoras ordenadas após ato em `app/chamados/services/fecho_runtime.py:130-141`, `app/pautas/services.py:344-352`. Ciclo concorrente concreto causado pela ampliação **[não verificado]**; não certificar ordem global de lotes multicollaboradores sem medição/inspeção do atomic externo.

## 11/11 23:48 · sessao 1d0a0301f85d · - · - · origem=Ronald (tmux)

**Item 2.** Contra o snapshot textual, reverter os 46 enxertos recupera o AST anterior dos 30 arquivos, descontados os imports; o runner também usa esse snapshot, carregado em `app/chamados/tests/e4b_c12_verificar.py:17-18`. Identidade Git da base e comparação independente de commits: [não verificado]; a cópia não contém repositório Git [não verificado].

O ramo `ato=` devolve exatamente o resultado do callable, inclusive `False` e `None`; `user` e `novo_status` externos não são repassados ao método original, que conserva seus argumentos (`app/chamados/services/fecho_runtime.py:157-169`). `reconciliar`, `encerrar`, `encerrar_por_competencia_trancada` e `cancelar` devolvem booleanos; `fechar` não possui retorno explícito (`app/chamados/models.py:642`, `app/chamados/models.py:656`, `app/chamados/models.py:1606`, `app/chamados/models.py:1621`, `app/chamados/models.py:1641`, `app/chamados/models.py:774`, `app/chamados/models.py:793`, `app/chamados/models.py:452`).

- S01–S13: mesmos métodos/argumentos e mesmos usos booleanos; S02–S04 descartam retorno; S12/S13 preservam as duas tentativas e rollback intermediário (`app/chamados/management/commands/auditar_chamados_orfaos.py:68`; `app/chamados/management/commands/smoke_chamados.py:135`, `:148`, `:164`; `app/chamados/reconciliador.py:497`, `:524`; `app/chamados/services/adesao.py:116`, `:151`; `app/chamados/services/competencia_trancada.py:185`; `app/chamados/services/feriado_reavaliar.py:48`; `app/chamados/services/limbo_folgas.py:102`, `:284`, `:287`).
- S14–S25: S14/S16/S22/S25 descartam resultado; S15 devolve o booleano; demais usam condição booleana; S23 mantém motivo completo e captura `ch=ch` (`app/chamados/services/materializacao.py:583`, `:772`; `app/chamados/services/regularizacao_ext.py:206`; `app/chamados/services/silencio_ausencia.py:102`; `app/chamados/signals.py:43`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:141`; `app/colaboradores/services/desligamento.py:156`; `app/colaboradores/services/pos_vinculo.py:43`; `app/colaboradores/services/vinculo.py:388`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:48`; `app/ponto/management/commands/curar_residuo_bug115.py:152`; `app/ponto/management/commands/detectar_ausencias.py:159`).
- S26–S38: usos booleanos preservados, inclusive seleção das comprehensions e ramo dry; S30 conserva argumentos e descarta `None`; S31 conserva motivo e captura `ch=ch` (`app/ponto/management/commands/detectar_ausencias.py:825`, `:835`, `:846`; `app/ponto/management/commands/detectar_intervalo_ausente.py:139`; `app/ponto/management/commands/fechar_chamados_meio_periodo.py:110`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`; `app/ponto/services/ausencia.py:695`; `app/ponto/services/cartorio.py:987`, `:1512`, `:1525`, `:1546`, `:1624`, `:1639`).
- S39–S46: usos booleanos preservados; S40 mantém `_fechou`; S46 descarta retorno e conserva a transição não terminal (`app/ponto/services/cluster_espurio.py:125`; `app/ponto/services/cobertura.py:123`, `:318`; `app/ponto/services/entrada_sincronizada.py:32`; `app/ponto/services/lastro.py:401`; `app/ponto/services/triagem_batida.py:598`; `app/ponto/services/veredito_celula.py:203`; `app/chamados/services/acoes_chamado.py:238`).
- E01–E09 descartam retorno da porta, portanto não há conversão indevida de contagem em booleano nesses chamadores (`app/chamados/models.py:455`; `app/chamados/services/acoes_chamado.py:71`, `:226`, `:283`, `:110`, `:152`, `:322`; `app/chamados/services/fio.py:722`; `app/colaboradores/views.py:2069`).

**Ressalva S30:** a ordem entre a Pauta existente de E01 e a nota muda. Antes, `fechar` escrevia Pauta pela porta interna e depois nota; com wrapper externo, a reentrada interna retorna sem escrever Pauta, a nota nasce e só depois o wrapper entrega Pauta (`app/chamados/models.py:455-458`; `app/ponto/management/commands/fechar_chamados_meio_periodo.py:110`; `app/chamados/services/fecho_runtime.py:173-176`, `:168`). Não achei troca na ordem entre as escritas antigas de estado e nota; equivalência operacional ORM [não verificado].

**Item 7.** A primeira chamada pula a disputa porque a irmã morta tem resposta sem veredito; essa lista exige `materializada_em is None` e texto/resposta (`app/colaboradores/services/desligamento.py:140-143`; `app/chamados/models.py:1843-1847`). S20 encerra o chamado; S143 poupa a viva e carimba a irmã morta com `resposta='validar'`, removendo o impedimento da disputa (`app/colaboradores/services/desligamento.py:156`; `app/chamados/services/disputa_emissao.py:1966-1987`).

Na segunda chamada, a disputa entra em `fechar(..., motivo=desligamento)`; C7 chama `fechar_por_admin` nas vivas e grava `admin_decidiu`/`materializada_em` por `carimbar_identidade` (`app/colaboradores/services/desligamento.py:143`; `app/chamados/models.py:1919-1927`; `app/chamados/models.py:3065`; `app/chamados/models.py:3186-3191`). Esse enterro ocorre **antes** do save da disputa e fora de S143/porta do Chamado; o save da disputa pode depois acionar S88, mas a pergunta já foi carimbada (`app/chamados/models.py:1937`; `app/chamados/signals.py:19-43`).

O teste alterado ainda exige viva sem `materializada_em` antes da segunda chamada, verifica Pauta idêntica depois e passa a exigir a via C7 esperada; não achei ocultação de enterro na primeira chamada (`app/chamados/tests/test_e4b_c12_orm.py:168-179`). Resultado ORM e demonstração pareada contra c11 real: [não verificado]. O teste não afirma explicitamente `materializada_em is not None` após a segunda chamada (`app/chamados/tests/test_e4b_c12_orm.py:174-179`).

**Item 8.** O mecanismo de +2 por entrada **aninhada e não reentrante** é coerente com o `atomic()` padrão; isso inclui entrada cujo gate devolve `False` (`app/chamados/services/fecho_runtime.py:158-169`). Sem recusas, o retorno anterior ao atomic remove o savepoint vazio de `_escrever_pautas` (`app/chamados/services/fecho_runtime.py:118-130`).

A explicação “2 entradas por chamado” não bate com +8 para três chamados: duas vezes três entradas, a +2 cada, seriam +12; +8 corresponde a quatro entradas (`app/ponto/tests/test_selo_chokepoint_escrita.py:210`). O código permite uma entrada por chamado no loop e outra por disputa/pai fechado no E2: E2 roda antes do loop, fecha disputas distintas e dispara S88; depois o loop ainda pode entrar para um chamado já fechado pelo signal (`app/ponto/services/cartorio.py:983-987`; `app/chamados/reconciliador.py:609-615`; `app/chamados/signals.py:34-43`). Logo, “N chamados + um pai de disputa” explicaria +4/+8; contagem efetiva dessa execução [não verificado].

“Nunca por item” só é defensável para o custo do savepoint por entrada. A porta é chamada dentro de loops por chamado; com recusas, `_escrever_pautas` percorre perguntas, acessa suas relações e janela, depois executa lock/dedup/escrita por competência (`app/ponto/services/cartorio.py:987`; `app/chamados/services/fecho_runtime.py:119-141`). Totais observados 217/277 e alegação de diff SQL exclusivamente SAVEPOINT/RELEASE: [não verificado]; constam como declarações nos comentários e no plano, sem a medição no dado raspado (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`; `PLANO_PISCADA.md:142`).

## 11/11 23:47 · sessao 7ca0371690e3 · - · - · origem=Ronald (tmux)

Itens 1 e 6 — somente leitura.

**Censo**

Não achei escritor corrente de fechamento/resolução de `ChamadoColaborador` fora da porta e do corpo nominal; os nove sítios internos estão em `app/chamados/tests/e4b_c12_ast.py:18`, e o teste cobra ausência de outros escritores em `app/chamados/tests/test_e4b_c12_selo.py:22`. Completude contra reflexão/SQL arbitrário: [não verificado].

A leitura literal de “todo escritor de status” encontra **20 chamadores não terminais fora da porta**, omitidos pelo conjunto `GATES`; `analisar` e `reabrir` escrevem em `app/chamados/models.py:807` e `app/chamados/models.py:1443`, mas seus nomes não constam de `app/chamados/tests/e4b_c12_ast.py:14`. O motor limita esses eventos ao destino `em_analise`, portanto não são fechadores/resolvedores do aval (`app/chamados/catalogo/motor.py:112`).

- `analisar`: `app/chamados/views.py:1513`; `app/ponto/services/regularizacao.py:137`; `app/chamados/services/disputa_emissao.py:1085`, `:1159`, `:1178`, `:1223`, `:1252`, `:1379`; `app/chamados/management/commands/geofence_registrados_para_analise.py:32`.
- `reabrir`: `app/chamados/models.py:1020`; `app/chamados/views_arquivar.py:70`; `app/ponto/management/commands/reconciliar_grade.py:530`; `app/ponto/management/commands/detectar_ausencias.py:302`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`; `app/chamados/services/disputa_emissao.py:1851`, `:2011`; `app/chamados/services/competencia_trancada.py:266`; `app/chamados/services/acoes_disputa.py:270`; `app/chamados/services/adesao.py:93`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:172`.

As migrations escrevem fora da porta: 0038 em `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; 0039 em `app/chamados/migrations/0039_a3_via_backfill.py:24`, `:33`. São nominalizadas em `app/chamados/tests/test_e4b_c12_selo.py:97`. Aplicação prévia/vazio de tenant novo só aparece como alegação de comentário e não está comprovada pelo dado raspado (`app/chamados/tests/test_e4b_c12_selo.py:93`).

Não achei escrita terminal literal em scripts externos de `bin`/`mensageria` nem SQL literal de `status_local`; completude dessa ausência é [não verificado]. O selo restringe a varredura a `app`, pois `ROOT` nasce em `app/chamados/tests/e4b_c12_ast.py:11`, e ainda exclui migrations, diretórios de teste e arquivos com prefixos `test_`/`tests_` (`app/chamados/tests/e4b_c12_ast.py:34`).

**Mordida e buracos**

Os testes existentes cobrem botão direto, update, aliases, getattr literal, loop, save e bulk_update literal (`app/chamados/tests/test_e4b_c12_selo.py:48`, `:64`, `:77`). Executei o runner somente em memória, com `python3 -B`/`PYTHONDONTWRITEBYTECODE=1`: **17 testes, OK**, sem banco; registro persistente desse resultado [não verificado].

Experimentos somente em memória mostraram:

| Forma | Resultado do AST | Explicação no código |
|---|---|---|
| `import chamados.models as m; ...encerrar()` | Morde | Resolve imports/atributos (`app/chamados/tests/e4b_c12_ast.py:109`, `:190`) |
| `C.custom.filter(...).update(status_local=...)` | Morde | Update aceita tipo desconhecido (`app/chamados/tests/e4b_c12_ast.py:253`) |
| `bulk_update(..., ['status_local'])` | Morde | Campo literal reconhecido (`app/chamados/tests/e4b_c12_ast.py:259`) |
| `getattr(c, 'encer' + 'rar')()` | Passa | Só resolve string `Constant` (`app/chamados/tests/e4b_c12_ast.py:114`) |
| Método herdado: `class Proxy(C): ... self.encerrar()` | Passa | Não resolve herança; compara tipo exatamente com TARGET (`app/chamados/tests/e4b_c12_ast.py:127`, `:249`) |
| `bulk_update(..., ['status_' + 'local'])` | Passa | Soma faz união dos fragmentos, não concatenação (`app/chamados/tests/e4b_c12_ast.py:161`) |
| `update(**{'status_local':'fechado'})` | Passa | Ignora keyword com `arg=None` (`app/chamados/tests/e4b_c12_ast.py:241`) |
| `save(False, False, None, ['status_local'])` | Passa | Só examina keyword `update_fields` e atribuição anterior (`app/chamados/tests/e4b_c12_ast.py:264`) |
| `transicionar_com_pauta(..., ato=c.encerrar())` | Passa | Autoriza qualquer AST dentro de `ato`; a chamada seria avaliada antes da entrada da porta (`app/chamados/tests/e4b_c12_ast.py:246`) |

O selo de migration pega update literal e atribuição, conforme seus dois adversariais (`app/chamados/tests/test_e4b_c12_selo.py:133`). Entretanto, **não pega bulk_update com fields posicional ou keyword `fields`, getattr literal, `save(update_fields=...)`, nem `**kwargs`**, pois a regra só reconhece chamadas de atributo com keyword `status_local`/`defaults`, ou atribuição explícita (`app/chamados/tests/test_e4b_c12_selo.py:112`).

Há um buraco adicional mais forte: inserir **uma nova escrita terminal literal dentro de `0039.backfill`** mantém o selo verde, porque os achados são um conjunto apenas de `(arquivo, função)`; a nova operação não muda esse conjunto (`app/chamados/tests/test_e4b_c12_selo.py:115`, `:119`, `:131`). Reproduzi em memória acrescentando `C.objects.update(status_local='fechado')` dentro dessa função; registro persistente [não verificado].

A descoberta de migrations ainda depende de nomes `0*.py`, excluindo uma futura `1000_*.py` (`app/chamados/tests/test_e4b_c12_selo.py:126`).

## 11/11 23:44 · sessao d83266f85771 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

C6 aplicada: cobertura separada do rótulo, mapas legados preservados, efeito de tipos inativos mantido e versão 5 com horizonte `2026-11`. [Relato, censo e escritores/leitores](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c6.r:1). [nao verificado]

22 testes GREEN; RED na base `f218d86a7` e na c5; 15 mutantes mortos, incluindo os nove anteriores. [Comandos e resultados](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c6_execucoes.r:4).

Sem commit, pela proibição inicial. [Registro](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c6.r:66). Certificação da suíte completa e Opus final: [nao verificado]. [nao verificado]

## 11/11 23:23 · sessao 2ecf1a0154a7 · FIO-E0-V4-E4B · construtor · origem=Code

C12 aplicada. A [entrega com tabela, leitores/escritores e comandos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c12.txt:6) registra os 46 sítios migrados. [nao verificado]

Validação local:

- [15 testes novos verdes](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:1185) e [106 de regressão verdes](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:730). [nao verificado]
- [55 mutantes: exatamente um subTest vermelho por sítio](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:470).
- [Base: 46 falhas esperadas](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:1162).

ORM D3/D5 preparado, sem execução: **[não verificado]**. Sem commit, conforme a [proibição da obra](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c12.txt:4). Certificação permanece com Code/suite/opus. [nao verificado]

## 11/11 23:01 · sessao e5ede8a32f5b · - · - · origem=Ronald (tmux)

Exploração somente leitura concluída; ORM não executado [não verificado].

- Reutilizar `RecusaS143RuntimeTest._cenario`: cria exportação, fechamento e pergunta viva com assertions reais (`chamados/tests/test_e4b_c6_orm.py:45`).
- D3 deve testar `resolvido → fechado`: matriz permite (`chamados/catalogo/motor.py:102`), guarda humana de viva só roda para origem `VIVOS` (`chamados/models.py:165`), e save terminal dispara S143 (`chamados/signals.py:96`). Resultado ORM: [não verificado].
- Vínculo: tornar categoria `escala_aceite` e chamar `_executar_aceite` com template do vínculo existente; fechamento precede criação (`colaboradores/services/vinculo.py:384`, `colaboradores/services/vinculo.py:393`).
- Desligamento: `_cenario(limbo=True)` mantém disputa mista aberta pela resposta sem veredito (`chamados/models.py:1843`); cascata pula essa disputa e alcança `encerrar` (`colaboradores/services/desligamento.py:139`, `colaboradores/services/desligamento.py:155`).
- Regularização: preparar disputas fechadas sem carimbar a pergunta, verificar C6 viva, então materializar reg confirmada. Guard só barra disputas abertas (`chamados/services/regularizacao_ext.py:58`), mas coleta S143 usa `_ancoradas()` também de disputas fechadas (`chamados/services/disputa_emissao.py:1940`). C6 não consulta `disputa.fechada_em` (`chamados/juizes.py:1330`). Existência desse acervo em produção: [não verificado].
- Cron: chamar `Command()._fechar_chamado(ch, entrada)` com chamado não-furo da fixture; gate permite premissa não-furo (`chamados/models.py:728`), sítio preserva booleano (`ponto/management/commands/detectar_ausencias.py:842`).
- Signal: fechar outra disputa sem perguntas cujo pai seja `ch`, conservando a disputa da viva; receiver reconcilia pai (`chamados/signals.py:28`).
- API: chamado `batida_ausente` no dia aberto, marco E08 aceso na célula e viva em outro dia exportado. Lookup é real (`chamados/juizes.py:2105`), juiz lê célula gravada (`ponto/services/lastro.py:185`).
- Cartório: cobrar marco antigo E07 num dia cuja grade contém E08/S17; manter viva em outro dia exportado. Ramo `marcos_inexistentes → reconciliar` exercita `_julgar_colab_corpo` sem adiar fechamento (`ponto/services/cartorio.py:1523`); gate consulta mesmo juiz (`chamados/models.py:1298`).

Para mutantes ORM, seguir enxerto em memória já existente, substituindo somente função/sítio e preservando globais (`chamados/tests/e4b_c11_verificar.py:78`, `chamados/tests/e4b_c11_verificar.py:90`). Execução e comandos novos dependem dos nomes finais [não verificado].

## 11/11 22:57 · sessao 65a24b6b5e82 · FIO-E0-V4-E4B · sonda · origem=Code

JSON integral fornecido, sem reexecução independente: não localizei `sonda_mig0039.py` na cópia. [não verificado]

```json
{"aplicada_em": "2026-08-23 04:09:15.485039+00:00", "chamados_superado": 107, "mig0039_aplicada": 1, "migrations_chamados_posteriores": 14} [nao verificado]
```

**(a)** O acionamento registrado é `RunPython(backfill, desfazer)`: `backfill` na aplicação efetiva da 0039; `desfazer` na reversão dela, com dependência da 0038. ([0039_a3_via_backfill.py:38](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0039_a3_via_backfill.py:38), [0039_a3_via_backfill.py:39](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0039_a3_via_backfill.py:39))

“Somente podem rodar por migrate/rollback” seria excessivo: testes chamam ambas diretamente. ([test_esmeril2_migracoes_reversas.py:36](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/tests/test_esmeril2_migracoes_reversas.py:36), [test_esmeril2_migracoes_reversas.py:39](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/tests/test_esmeril2_migracoes_reversas.py:39))

**(b)** No fluxo de tenant novo implementado, **zero**: a tabela nasce por `CreateModel`, sem inserções; as migrations de dados anteriores apenas atualizam registros existentes. ([0001_initial.py:16](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0001_initial.py:16), [0023_rebaixar_urgencia_resolvidos.py:4](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:4), [0024_rebaixar_urgencia_resolvidos_regressao.py:5](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:5), [0038_a3_registrado_backfill.py:9](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0038_a3_registrado_backfill.py:9))

O provisionamento cria o tenant com schema automático antes do bootstrap administrativo. ([models.py:21](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/tenants/models.py:21), [services.py:11](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/tenants/services.py:11), [services.py:13](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/tenants/services.py:13)) Contagem medida num tenant novo de produção: [não verificado].

**(c)** Não encontrei importação ou chamada dessas funções em código de produção fora de migrations na busca estática da cópia. Ausência em execução real: [não verificado]. O import direto encontrado está em teste; as referências produtivas encontradas à 0039 são comentários. ([test_esmeril2_migracoes_reversas.py:11](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/tests/test_esmeril2_migracoes_reversas.py:11), [cluster_espurio.py:134](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cluster_espurio.py:134), [cluster_espurio.py:136](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cluster_espurio.py:136))

Atribuir os 107 `superado` à execução da 0039, apenas com esse JSON: [não verificado].

## 11/11 22:57 · sessao 7e1f793c05ad · - · - · origem=Ronald (tmux)

No fluxo de tenant novo implementado, a 0039 encontra **0 `ChamadoColaborador`**: a tabela é criada em `app/chamados/migrations/0001_initial.py:16`, sem inserção de registros; as migrações de dados anteriores apenas alteram objetos existentes (`app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:4`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:5`, `app/chamados/migrations/0038_a3_registrado_backfill.py:9`).

O provisionamento cria `Cliente` antes do bootstrap (`app/tenants/services.py:11`, `app/tenants/services.py:13`), com criação automática do schema habilitada (`app/tenants/models.py:14`, `app/tenants/models.py:21`). O bootstrap posterior cria Empresa, Setores e usuário administrador (`app/tenants/bootstrap.py:27`, `app/tenants/bootstrap.py:32`, `app/tenants/bootstrap.py:34`). O teste de scaffolding também contém a asserção `ChamadoColaborador.objects.count() == 0` (`app/tenants/tests.py:13`).

Isso descreve o fluxo da cópia; não executei testes nem medi um tenant novo em produção. O dado fornecido não contém essa contagem de tenant novo. [não verificado] [nao verificado]

_4077 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
