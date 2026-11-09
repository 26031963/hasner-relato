# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 08:50** · publicado em **09/11/2026 08:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 08:50 · sessao 6499c93501d2 · SELOS-CANON-I1 · verificador · origem=Code

**O item 1 está curado quanto à abertura indevida da vaga.** O espelho está no predicado comum; o Agent chega nele, e o wrapper usa o mesmo predicado, embora recuse previamente os IDs com espaço. Há resíduos fora do diff e um limite na prova do PAREI, detalhados abaixo. (`bin/disjuntor.py:1686`, `:1850`, `:2248`, `:2267`.)

1. **Cura nas duas vias e caminhos alternativos**

   - O Agent chama `_censo_nega`, que consulta `_pertence_fila1`. Agora, célula `O9` contra obra `O9 (C)` ou `O9 NOTURNO-X` produz `ValueError`, convertido em negativa NAO SEI. A saída antecipada como FORA deixou de ocorrer nesses pares. (`bin/disjuntor.py:1850`, `:1704`, `:1686`, `:1691`, `:1730`.)
   - O wrapper chama `--censo` antes de prender a vaga; `main_censo` consulta `_pertence_fila1` e passa a mesma lista de células para `_censo_nega`. Os exemplos com espaço já são recusados antes dessa consulta; no wrapper real, a validação é ainda anterior, no shell. (`bin/codex_pergunta.sh:188`, `:234`, `:240`; `bin/disjuntor.py:2248`, `:2266–2275`.)
   - Nos caminhos de autorização de censo examinados, a decisão passa pelo predicado comum. A igualdade existente nele compara **obra com token**, dentro do próprio juiz. (`bin/disjuntor.py:1685`, `:1704`, `:2267`.)
   - **Existem igualdades fora dele:** o hook compara a célula crua com o alvo do marcador para localizar o item em curso e remove esse item da lista por igualdade. São decisões de marcador/ordem, distintas da autorização do censo. (`bin/hook_stop_fila1.py:223`, `:273`.)
   - **Existe uma derivação própria de fila no alarme**, descrita no item 4. Ela alimenta o disparo do alarme, sem autorizar vaga de construtor. (`bin/alarme_sessao_ociosa.py:89–116`, `:148–154`.)

2. **Quem define, quem lê e significado dos retornos**

   | Função | Definição e leitores encontrados |
   |---|---|
   | `_pertence_fila1` | Definida em `bin/disjuntor.py:1669`; leitores produtivos em `:1704` e `:2267`; chamadas diretas do selo em `bin/tests/test_censo_antes_da_vaga.sh:605` e `:611`. |
   | `_token_da_celula` | Definida em `bin/disjuntor.py:1651`; lida nas listas `exatas` e `nomeiam`, em `:1685–1686`. |
   | `_casa_id_inteiro` | Definida em `bin/hook_stop_fila1.py:110`; lida pelo predicado em `bin/disjuntor.py:1686` e pelo julgamento do PAREI em `bin/hook_stop_fila1.py:1301` e `:1313`. |

   As versões mutantes são escritas pelo mecanismo do selo em cópias de `bin/`; há mutações desses critérios em `bin/tests/test_censo_antes_da_vaga.sh:750–763`, aplicadas por `:669–678`.

   Os dois leitores produtivos de `_pertence_fila1` concordam: `False` permite seguir como FORA; `True` exige o censo; exceção gera negativa NAO SEI. (`bin/disjuntor.py:1704–1731`, `:2267–2281`.) `_token_da_celula` retorna string ou `None`; o espelho não transforma `None` em correspondência, porque `_casa_id_inteiro` rejeita ID vazio. (`bin/disjuntor.py:1660–1666`; `bin/hook_stop_fila1.py:115`.)

   A nomeação ignora caixa e usa fronteira ASCII que exclui letra, dígito e hífen; underscore é separador. A extração do token usa a mesma classe de fronteira. A igualdade exata permanece sensível à caixa, razão de `o9 × O9` resultar em ambiguidade. (`bin/hook_stop_fila1.py:117`; `bin/disjuntor.py:1664`, `:1685–1691`.)

   Alarme e handoff chegam a esses critérios indiretamente pelo hook. Para a prova de PAREI censo, ambos respeitam o contrato próprio do reconhecedor: `None` significa “não é linha de censo”; `''`, prova válida; texto, motivo de reprovação. (`bin/alarme_sessao_ociosa.py:127–140`; `bin/handoff_sessao.sh:129–134`; `bin/hook_stop_fila1.py:773–785`.) O handoff também consome os julgamentos booleanos do hook como booleanos. (`bin/handoff_sessao.sh:120–123`.)

3. **Matriz do predicado literal**

   Cada linha considera `_pertence_fila1(obra, [celula])`. A coluna anterior corresponde à retirada do espelho, exatamente a mutação declarada no selo. (`bin/disjuntor.py:1685–1692`; `bin/tests/test_censo_antes_da_vaga.sh:763`.)

   | Obra | Célula | Atual | Sem espelho | Sítio da saída atual |
   |---|---|---|---|---|
   | `O1` | `O142` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `O142` | `O1` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `O9` | `O9` | `True` — DENTRO | `True` | `bin/disjuntor.py:1689–1690` |
   | `O9 (C)` | `O9` | `ValueError` — NAO SEI | `False` | `bin/disjuntor.py:1691` |
   | `O9 NOTURNO-X` | `O9` | `ValueError` — NAO SEI | `False` | `bin/disjuntor.py:1691` |
   | `OBRA-Q (A)` | `OBRA-Q` | `ValueError` — NAO SEI | `False` | `bin/disjuntor.py:1691` |
   | `09-TURNO-ABERTO-EXPOSTA` | `O9` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `O67b` | `O9` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `o9` | `O9` | `ValueError` — NAO SEI | `ValueError` | `bin/disjuntor.py:1691` |

   **Há três conversões de FORA para NAO SEI**, e elas passam a negar na porta do Agent. O código as declara ambiguidades pretendidas; demonstrar que alguma corresponde a uma obra **de fato fora da fila** em produção é **[nao verificado]** com o dado fornecido. Também não há base para garantir ausência de toda negativa legítima afetada. (`bin/disjuntor.py:1674–1680`, `:1730–1731`.)

   A matriz não equivale à extração do prompt: `OBRA-Q (A)` vira `OBRA-Q` no Agent; `09-TURNO-ABERTO-EXPOSTA`, `O67b` e `o9` não satisfazem o reconhecedor de obra desse caminho. Obra ilegível é negada antes da pertença. (`bin/disjuntor.py:157`, `:602–627`, `:1847–1849`.)

4. **Classes de reprovação**

   - **Derivação paralela presente:** o alarme conta sua própria fila, excluindo `☑`, `FEITO` e `FECHAD`; o hook usa marcadores e `_item_parado`, incluindo exclusões como CONGELADA, FILA 2 e PAREI. Uma linha apenas CONGELADA/FILA 2 pode ser contada pelo alarme e excluída pelo hook. (`bin/alarme_sessao_ociosa.py:89–116`; `bin/hook_stop_fila1.py:95–97`, `:171–180`, `:231–252`.)
   - **Escritores fora da porta presentes na cópia:** `gerar_cortes.py` e `deploy_agendado.sh` regravam RELATO diretamente; o escritor canônico declara esse resíduo. A cura examinada usa `no_relato`/`no_topo`. (`bin/gerar_cortes.py:117–136`; `bin/deploy_agendado.sh:31–44`; `app/core/esteira_vigia.py:839–840`; `bin/disjuntor.py:294`, `:324`.)
   - **Segundo juiz de negócio concorrendo com autoridade registrada:** não demonstrado nesta cura; conclusão abrangente sobre todos os registros: **[nao verificado]**. O caminho examinado reutiliza o hook, o predicado, o coletor e o juiz de brief. (`bin/disjuntor.py:1637`, `:1704`, `:1710`, `:1715`.)
   - **Relógio fora da borda:** não aparece na decisão de pertença/token; a medida recebe `agora` e filtra futuro e janela. (`bin/disjuntor.py:1651–1692`, `:2066`, `:2094`, `:2127–2131`.)
   - **Ordem de trava:** o censo antecede a vaga; as portas de RELATO compartilham trava, liberada ao retornar. Não identifiquei inversão nesse percurso. (`bin/codex_pergunta.sh:234–240`; `app/core/esteira_vigia.py:843`, `:846–862`, `:892`.)
   - **Campo com dois significados:** `_censo_nega` usa `None` para “porta passa”, abrangendo FORA e aprovação; isso está documentado, e `main_censo` distingue FORA pelo predicado. Não encontrei leitor tomando esse `None` por DENTRO. (`bin/disjuntor.py:1696`, `:1704–1705`, `:1724`, `:2267–2275`.)

5. **Mordida dos casos novos e seu limite**

   - **Laço Agent:** prepara célula `O9` sem `O9 (C)` e exige negativa JSON, motivo de censo, PAREI com a obra inteira e negativa correspondente na trilha. Saída vazia reprova. Sem espelho, o predicado retorna FORA antes da validação de forma, retirando a negativa cobrada. (`bin/tests/test_censo_antes_da_vaga.sh:580`, `:520–527`, `:588–591`; `bin/disjuntor.py:1687–1688`, `:1704–1706`.)
   - **ESPELHO direto:** exige `ValueError` nos três pares e exatamente `False` nos dois controles de prefixo. Portanto, “sempre retornar FORA” e “sempre levantar” reprovam. Aceita qualquer `ValueError`, sem validar a mensagem. (`bin/tests/test_censo_antes_da_vaga.sh:603–619`.)
   - **Mutante:** remove precisamente o espelho e executa `casos_pertenca`; há exigência de âncora única, controle sem mutação e propagação de falha pelo retorno dos casos. (`bin/tests/test_censo_antes_da_vaga.sh:763`, `:675–686`, `:662–665`, `:647`.)
   - **Wrapper:** os dois casos são controles expressamente declarados, inclusive no nome; passam com ou sem a cura porque a forma é recusada antes da pertença. (`bin/tests/test_censo_antes_da_vaga.sh:585–593`; `bin/disjuntor.py:2248–2250`.)
   - **Limite material:** o laço Agent prova o PAREI **na mensagem**. Para esses IDs com espaço, `_parei_censo_no_relato` retorna antes de gravar; o laço não cobra persistência no topo. Logo, a mordida demonstra negativa/trilha, mas não demonstra PAREI gravado desses exemplos. (`bin/tests/test_censo_antes_da_vaga.sh:520–527`, `:590–591`; `bin/disjuntor.py:309–311`.)

A execução integral do selo e seu resultado verde permanecem **[nao verificado]**; o script cria e altera arquivos de teste. (`bin/tests/test_censo_antes_da_vaga.sh:27–35`, `:669–678`.) O conteúdo integral da lei citada como `CORTES.md:428` também permanece **[nao verificado]**: a cópia termina em `app/docs/CORTES.md:425`, e o anexo fornecido traz somente o início daquela linha.

## 09/11 08:48 · sessao 400ac2cb4b5d · - · - · origem=Ronald (tmux)

Não encontrei classe nova dessas no caminho da cura do censo, com os limites abaixo.

- **Juiz/derivação única:** `_fila1_viva` delega o universo ao hook; `_censo_nega` usa `_pertence_fila1`, `_briefs_do_censo` e `censo_do_brief`. Agent chama essa porta; wrapper chama `--censo`. A classificação adicional em `main_censo` repete o mesmo predicado e reutiliza a mesma lista `fila`. (`bin/disjuntor.py:1633`, `bin/disjuntor.py:1704`, `bin/disjuntor.py:1710`, `bin/disjuntor.py:1715`, `bin/disjuntor.py:1850`, `bin/disjuntor.py:2266`, `bin/disjuntor.py:2275`; `bin/codex_pergunta.sh:234`.)
- **Escritor/ordem:** a negativa passa por `ev.no_relato`; PAREI passa por `ev.no_topo` e é relido pelo reconhecedor do Stop. Ambas as portas usam o mesmo flock; leitura, classificação, idempotência e gravação do topo ficam sob a trava. O wrapper executa censo antes de tentar vaga; a trava do RELATO já foi fechada quando chega à vaga. (`bin/disjuntor.py:293`, `bin/disjuntor.py:323`; `app/core/esteira_vigia.py:842`, `app/core/esteira_vigia.py:859`, `app/core/esteira_vigia.py:877`, `app/core/esteira_vigia.py:893`; `bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`.)
- **Relógio:** pertença/token não consultam relógio; `_censo_24h` recebe `agora`, exclui futuro e aplica a janela ao casar a abertura. `trilha` consulta relógio para carimbar o registro. (`bin/disjuntor.py:1651`, `bin/disjuntor.py:1669`, `bin/disjuntor.py:2066`, `bin/disjuntor.py:2094`, `bin/disjuntor.py:2127`, `bin/disjuntor.py:198`.)
- **Campo/retorno:** `None` de `_censo_nega` significa “porta passa”, tanto FORA quanto APROVADO; `main_censo` distingue FORA pelo bool do mesmo `_pertence_fila1`, portanto não interpreta `None` como DENTRO. (`bin/disjuntor.py:1696`, `bin/disjuntor.py:1704`, `bin/disjuntor.py:1723`, `bin/disjuntor.py:2267`, `bin/disjuntor.py:2271`.)
- **Resíduo explícito fora dessa cura:** o escritor declara escritores externos que não pegam sua trava: edição manual, `gerar_cortes.py` e `deploy_agendado.sh`. O alcance desses caminhos não foi auditado por este agente. (`app/core/esteira_vigia.py:838`.) A lista usada pelo censo contém somente andáveis; a situação de obra PARADA continua dependente desse recorte. (`bin/disjuntor.py:1637`.)

Autoridade registrada para esta pergunta específica em outro lugar e efeitos atuais em produção: [nao verificado].

## 09/11 08:47 · sessao fb09f4e39450 · - · - · origem=Ronald (tmux)

Os três novos elementos têm mordida por leitura do código; execução integral do selo permanece [nao verificado].

- **Laço Agent:** a fixture remove `O9 (C)` e mantém `O9` sozinho antes dos casos (`bin/tests/test_censo_antes_da_vaga.sh:576`, `:580`). Os prompts preservam as identidades `O9 (C)` e `O9 NOTURNO-X`, pela cauda e pela identidade de id nu (`bin/disjuntor.py:164`, `:616`, `:624`). Os casos exigem JSON `deny`, `DISJUNTOR CENSO` e PAREI da obra completa (`bin/tests/test_censo_antes_da_vaga.sh:520`), além de última negativa correspondente e ausência de FORA (`:524`); saída vazia reprova (`:522`). O laço executa essas verificações nas duas obras (`:588`–`:593`).

- **Por que mata a ausência do espelho:** sem o segundo sentido, essas obras não aparecem dentro da célula `O9`, logo `nomeiam` e `exatas` ficam vazios e a função retorna `False` (`bin/disjuntor.py:1685`–`:1688`). `_censo_nega` então retorna `None` antes da validação da forma (`:1704`–`:1706`), deixando de fornecer a negativa exigida pelo caso Agent (`bin/tests/test_censo_antes_da_vaga.sh:590`). Com o espelho, há nomeação sem correspondência exata, e a função levanta `ValueError`, convertido em negativa NAO SEI (`bin/disjuntor.py:1686`, `:1691`, `:1730`).

- **Wrapper:** são controles sem poder de distinguir a cura, expressamente declarados assim no comentário e no nome do caso (`bin/tests/test_censo_antes_da_vaga.sh:585`, `:593`). `main_censo` recusa espaço antes da leitura da fila e da pertença (`bin/disjuntor.py:2248`, `:2266`). Não há controle ocultamente apresentado como mordida nova nesses dois casos (`bin/tests/test_censo_antes_da_vaga.sh:585`–`:586`).

- **ESPELHO direto:** chama o predicado efetivamente importado do diretório `$B`, exige `ValueError` nos três pares e reprova qualquer retorno, inclusive `False` (`bin/tests/test_censo_antes_da_vaga.sh:598`–`:608`). Os controles `O1/O142` e inverso exigem exatamente `False`; um predicado que sempre levante também reprova (`:609`–`:619`). A chamada direta cobre `OBRA-Q (A)`, que o Agent normalizaria para `OBRA-Q` (`bin/disjuntor.py:624`–`:627`; justificativa no selo `:596`).

- **Mutante:** remove exatamente o segundo sentido e roda `casos_pertenca` (`bin/tests/test_censo_antes_da_vaga.sh:763`). A infraestrutura exige âncora única (`:675`–`:678`), exige retorno não zero dos casos (`:681`–`:686`) e roda previamente uma cópia sem mutação nos mesmos grupos (`:662`–`:665`). `casos_pertenca` acumula `ok=1` e retorna esse valor, de modo que suas falhas chegam ao julgador do mutante (`:590`, `:619`, `:647`).

Confirmei apenas em memória, com `python3 -B`, o resultado do predicado vivo e dessa substituição: os três pares espelho dão `ValueError` no vivo e `False` no mutante; os dois controles de prefixo dão `False` em ambos. Esses resultados são explicados por `bin/disjuntor.py:1685`–`:1692` e pela substituição de `bin/tests/test_censo_antes_da_vaga.sh:763`; a suite completa e o seu resultado final são [nao verificado].

Limite: a asserção direta aceita qualquer `ValueError`, sem conferir sua mensagem (`bin/tests/test_censo_antes_da_vaga.sh:606`). Na implementação lida, para essas entradas não vazias, o `ValueError` explícito é o de pertença ambígua (`bin/disjuntor.py:1681`, `:1691`); não encontrei vacuidade concreta nesse trecho.

## 09/11 08:47 · sessao 1b91c105ad23 · - · - · origem=Ronald (tmux)

Rastreio das três funções, incluindo arquivos fora do diff:

- **`_pertence_fila1`**: definição única em `bin/disjuntor.py:1669`. Leitores produtivos encontrados: `_censo_nega`, em `bin/disjuntor.py:1704`, e `main_censo`, em `bin/disjuntor.py:2267`. Ambos interpretam `False` como FORA/passagem e exceção como NAO SEI/negativa; `True` leva ao censo (`bin/disjuntor.py:1704-1731`, `bin/disjuntor.py:2267-2281`).
- **`_token_da_celula`**: definição única em `bin/disjuntor.py:1651`; leitores produtivos encontrados somente nas listas `exatas` e `nomeiam` de `_pertence_fila1`, em `bin/disjuntor.py:1685-1686`. Retorna string ou `None`; a nomeação não aceita id vazio, portanto o `None` não casa pelo espelho (`bin/disjuntor.py:1660-1666`, `bin/hook_stop_fila1.py:115-117`).
- **`_casa_id_inteiro`**: definição única em `bin/hook_stop_fila1.py:110`. Leitores diretos encontrados: `_pertence_fila1`, em `bin/disjuntor.py:1686`, e `_parei_do_em_curso`, em `bin/hook_stop_fila1.py:1301` e `bin/hook_stop_fila1.py:1313`. Todos consomem bool; a implementação usa caixa normalizada e fronteira `[A-Z0-9-]`, inclusive permitindo underscore como separador (`bin/hook_stop_fila1.py:115-117`). A fronteira de extração da célula usa a mesma classe ASCII (`bin/disjuntor.py:1664`).

**Fora do diff:** o handoff não lê diretamente essas três funções: pede item em curso ao hook, e usa seus bools como bools, sem a antiga confusão bool/lista (`bin/handoff_sessao.sh:64`, `bin/handoff_sessao.sh:71`, `bin/handoff_sessao.sh:120-123`). Para PAREI censo, pede o reconhecedor compartilhado e distingue `None`, `''` e motivo (`bin/handoff_sessao.sh:129-134`). O alarme também não lê diretamente as três funções; pede `prova_parei_censo` e só considera provado o retorno `''` (`bin/alarme_sessao_ociosa.py:127-140`).

**Classe presente fora do diff: derivação paralela da fila.** O alarme possui `_fila` próprio: procura `## OBRAS`, conta bullets/linhas e exclui `☑`, `FEITO` ou `FECHAD` em qualquer posição (`bin/alarme_sessao_ociosa.py:89-116`). O juiz compartilhado do hook delimita por marcador e usa `_item_parado`, que também exclui CONGELADA, FILA 2, PAREI e outras marcas (`bin/hook_stop_fila1.py:95-97`, `bin/hook_stop_fila1.py:171-180`, `bin/hook_stop_fila1.py:231-252`). Assim, são respostas potencialmente divergentes à contagem da fila; o alarme usa sua resposta para decidir o disparo (`bin/alarme_sessao_ociosa.py:40-45`, `bin/alarme_sessao_ociosa.py:148-150`). Isso é herdado e não constitui outro caminho de autorização da vaga do censo (`bin/alarme_sessao_ociosa.py:144-164`).

**Igualdades fora do predicado existem, mas respondem outra pergunta:** o hook compara célula crua com o alvo declarado no marcador para localizar o item em curso (`bin/hook_stop_fila1.py:209-227`) e remove esse item por igualdade da lista de andáveis (`bin/hook_stop_fila1.py:270-273`). Não convém afirmar genericamente “não há igualdade obra × célula”; há essa igualdade, sem decidir a pertença usada pelo censo (`bin/disjuntor.py:1704`, `bin/disjuntor.py:2267`).

Leitores do selo encontrados: chamadas diretas a `_pertence_fila1` em `bin/tests/test_censo_antes_da_vaga.sh:605` e `bin/tests/test_censo_antes_da_vaga.sh:611`; mutações textuais das listas em `bin/tests/test_censo_antes_da_vaga.sh:750` e `bin/tests/test_censo_antes_da_vaga.sh:763`. Chamadas dinâmicas não identificáveis por referências nominais permanecem [nao verificado].

## 09/11 08:41 · sessao 880e3d56c067 · FIO-E0-V4-E3 · construtor · origem=Code

| Hunk | Alteração / escrita e leitura |
|---|---|
| [core/juizes.py:759](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/core/juizes.py:759) | Baixa pontual do dashboard; registro lido pelo selo em `core/tests/test_contract_juiz_tela.py:34`. |
| [chamados/models.py:1795](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/models.py:1795) | Método removido, lápide no sítio. Guarda contra retorno em `chamados/tests/test_e3_pendentes_app.py:285`. |
| [tarefa_do_dia.py:363](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/services/tarefa_do_dia.py:363) | Carga escreve `_verbos`, `_fila`, `_fila_cel`; tarefa lê em `:411` e `:422`. Fontes chamadas: `particionar_verbos(qs_vivos())['mapa']` e `qs_na_fila()`, também lidas por C17 em `chamados/juizes.py:1447` e `:1450`. |
| [tarefa_do_dia.py:418](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/services/tarefa_do_dia.py:418) | Escreve campos aditivos `rota/fila/verbos/fio`; leitores novos: DIFF `:120` e testes `test_e3_tarefa_portas.py:96`. | [nao verificado]
| [verbos.py:30](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/services/verbos.py:30) | Recebe `agora` da tarefa em `tarefa_do_dia.py:367`; conserva valor padrão dos leitores existentes, incluindo `painel.py:230` e `juizes.py:1447`. |
| [test_e3_pendentes_app.py:99](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_e3_pendentes_app.py:99) | M1–M5 com fixtures ORM, relógio congelado e views completas; M6/M7 mantidos em `:306` e `:321`. |
| [test_esmeril2_left_join.py:19](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_esmeril2_left_join.py:19) | Migra para `consulta_do_app`: preserva ausência de fantasma e exige duas perguntas → duas pendências. |
| [test_fio_le_o_juiz.py:263](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_fio_le_o_juiz.py:263) | Migra leitura para `consulta_do_app`; preserva desaparecimento da pergunta absolvida. |
| [test_dashboard_badge.py:37](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/core/tests/test_dashboard_badge.py:37) | Migra comparação da tela para `consulta_do_app`. Seus leitores de produção continuam em `api/views.py:325`, `api/views_core.py:314`, `core/views.py:253` e `api/views.py:2562`. |
| `test_badge_disputa_pendente.py` — excluído | Sustentava unidade por disputa; substituição por pergunta em `test_e3_pendentes_app.py:176`. |
| [test_e3_tarefa_portas.py:1](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_e3_tarefa_portas.py:1) | Quatro respostas escritas antes do aditivo; fixtures escrevem fatos, testes leem a tarefa. | [nao verificado]
| [diff_tarefa_portas.py:23](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/management/commands/diff_tarefa_portas.py:23) | Barreira SQL; comparação lê `_porta` e tarefa em `:118`. Escreve somente saída agregada em `:132`. |
| [test_e3_diff_portas.py:49](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/test_e3_diff_portas.py:49) | Selo injeta escrita indireta e exige aborto antes do driver; identidade antes da raspagem em `:72`. |
| [e3_mutantes.py:15](/home/ronald/wt-codex-fio-e0-v4-e3-073210/app/chamados/tests/e3_mutantes.py:15) | Catálogo de mutações; `aplicar_mutante`, em `:53`, remenda funções somente em memória e restaura após cada teste. | [nao verificado]

**DECISAO-ABERTA:** a interpretação “sem ações ⇒ sem porta” está explícita em `test_e3_tarefa_portas.py:10`. Medição na sombra por L-099/SONDA-SEM-GRANT e efeito na folha: **[nao verificado]**. C17 conserva os três chamadores em `fora_da_folha.py:67`, `vigia_fio.py:635` e `cadastro_realidade.py:488`.

Comandos preparados, **não executados**; RED, mortes dos mutantes e resultados da suíte: **[nao verificado]**. [nao verificado]

```bash
t() {
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    --settings=config.settings.ci "$@"
}
SELOS="chamados.tests.test_e3_pendentes_app chamados.tests.test_e3_tarefa_portas chamados.tests.test_e3_diff_portas"

t $SELOS core.tests.test_contract_juiz_tela \
  chamados.tests.test_esmeril2_left_join \
  chamados.tests.test_fio_le_o_juiz core.tests.test_dashboard_badge

# RED: retirar o aditivo.
E3_MUTANTE=sem_aditivo t chamados.tests.test_e3_tarefa_portas

# Um comando por mutante catalogado; esperado RED.
for m in $(PYTHONDONTWRITEBYTECODE=1 python3 -c \
  'from chamados.tests.e3_mutantes import MUTANTES; print(*MUTANTES, "legado_publico", "registro_dashboard", "quarto_c17_alias")')
do
  E3_MUTANTE="$m" t $SELOS
done

# DIFF: NÃO RODADO; universo a fornecer pela sombra.
PYTHONDONTWRITEBYTECODE=1 python3 manage.py diff_tarefa_portas \
  --colaborador ID --inicio AAAA-MM-DD --fim AAAA-MM-DD
```

Saída observada da compilação e auditoria estática dos arquivos acima:

```text
py_compile: OK nos 12 .py presentes; saída interceptada em memória.
Arquivos de bytecode escritos: 0.
Mutantes: 18 alvos textuais únicos, sintaxe compilável; sem execução.
C17: 3 chamadores diretos por AST.
Legado removido: 0 chamadas de produção por AST.
git diff --check: sem saída.
```

## 09/11 08:34 · sessao b4de1741aa55 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

**PROCEDENTE: a cura do par reconhecido está estruturada, mas identidade, leitores da espera e rollback do O330 ainda impedem afirmar o contrato universal.** `app/chamados/services/materializacao.py:84`, `app/chamados/services/explicador.py:97`, `app/chamados/services/validacao.py:190`.

1. **ATO ÚNICO — NAO PROCEDE para o par identificado; a universalidade não está atendida.**

   O par reconhecido entra no mesmo `atomic`, põe ambas em voo, desliga a guarda individual e julga paridade/lastro depois das duas chamadas. Carimbos e reconciliação vêm depois. Toda batida nova desse ramo passa pela porta; reuso pode dispensar `registrar_batida`. `app/chamados/services/materializacao.py:1315`, `app/chamados/services/materializacao.py:1335`, `app/chamados/services/materializacao.py:1340`, `app/chamados/services/materializacao.py:1347`, `app/chamados/services/materializacao.py:919`, `app/chamados/services/materializacao.py:987`.

   **Ainda existe aplicação individual de intervalo:** `sem_par`, `par_ambiguo` e `marcos_sem_par` caem no avulso. A irmã já aplicada é explicitamente cobrada como controle desse comportamento. Não equivale a garantir ato conjunto para todo motivo de intervalo. `app/chamados/services/materializacao.py:77`, `app/chamados/services/materializacao.py:1400`, `app/chamados/services/materializacao.py:1447`, `app/chamados/tests/test_bug_par_intervalo.py:444`.

   Chamadores encontrados por AST:

   | Função chamada | Chamadores; apelido |
   |---|---|
   | `_materializar_uma` | `validar_pergunta`, import sem alias: `app/chamados/services/validacao.py:466`, chamadas `:469`, `:474`; varredura, nome local: `app/chamados/services/materializacao.py:1565`; `_plantar_o_dia`, import sem alias: `app/chamados/services/passivo_validado.py:39`, chamada `:529`. |
   | `_materializar_resposta_hora` | `_materializar_par` e `_materializar_avulsa`, nomes locais: `app/chamados/services/materializacao.py:1336`, `app/chamados/services/materializacao.py:1447`; reexport sem chamada: `app/chamados/models.py:4077`. |
   | `registrar_batida`, neste materializador | Import sem alias; ramo hora e ramo data/hora: `app/chamados/services/materializacao.py:986`, `app/chamados/services/materializacao.py:987`, `app/chamados/services/materializacao.py:1068`. |

   **FILA — outros atos de preenchimento de intervalo também admitem resultado parcial:** `veredito_celula` captura falha por escrita e só desfaz quando nenhuma ação deu certo; regularização externa continua após uma criação recusada. São decisões do dia/regularização externa, com unidades próprias, não o aceite destas duas `PerguntaDisputa`. Cura: auditar atomicidade dessas unidades separadamente; não incluí-las numa promessa universal já cumprida. `app/ponto/services/veredito_celula.py:320`, `app/ponto/services/veredito_celula.py:327`, `app/ponto/services/veredito_celula.py:342`, `app/chamados/services/regularizacao_ext.py:172`, `app/chamados/services/regularizacao_ext.py:176`, `app/chamados/services/regularizacao_ext.py:197`.

2. **TRAVAS — NAO PROCEDE a inversão entre os dois validadores descritos.**

   `_travar_ato_materializacao` trava primeiro Colaborador; depois entrega o universo à porta que adquire Disputas → Chamados → Perguntas, cada tabela por PK. Dois validadores do mesmo colaborador serializam antes de alcançar qualquer metade. `app/chamados/services/materializacao.py:95`, `app/chamados/services/materializacao.py:104`, `app/chamados/models.py:2674`, `app/chamados/models.py:2683`, `app/chamados/models.py:2696`, `app/chamados/models.py:2700`.

   Seus quatro sítios de chamada estão dentro de transação: validação, envelope comum, competência O330 e dia O330; todos usam import sem alias ou nome local. `app/chamados/services/validacao.py:428`, `app/chamados/services/validacao.py:434`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/materializacao.py:1379`, `app/chamados/services/passivo_validado.py:371`, `app/chamados/services/passivo_validado.py:432`, `app/chamados/services/passivo_validado.py:519`, `app/chamados/services/passivo_validado.py:520`.

   **FILA — prova concorrente:** R3 executa as validações sequencialmente; não exercita duas conexões disputando locks. Deadlock executado ou ausência dele: **[nao verificado]**. `app/chamados/tests/test_bug_par_intervalo.py:174`.

3. **ESTADO — PROCEDENTE: a espera ganhou significado que vários leitores não conhecem.**

   A primeira metade fica `VALIDADA`, sem `materializada_em`, morta para C6 e fora de `respostas_sem_veredito`. A porta nova preserva essa validação e retorna `aguardando_par`. `app/chamados/models.py:3041`, `app/chamados/juizes.py:1153`, `app/chamados/models.py:1851`, `app/chamados/services/validacao.py:192`, `app/chamados/services/validacao.py:482`.

   | Leitor | Consequência |
   |---|---|
   | Explicador/modal | Diagnostica `validada_sem_gravar`, manda corrigir escala ou TI/DP e anuncia “o sistema não conseguiu gravar”, mesmo com espera legítima. **PROCEDENTE.** `app/chamados/services/explicador.py:97`, `app/templates/chamados/partials/modal_fio.html:449`. |
   | Declaração/admin/app | Considera validação sem registro uma falha e manda refazer o horário; a projeção da API reutiliza essa família de leitores. **PROCEDENTE.** `app/chamados/services/declaracao_texto.py:221`, `app/chamados/services/declaracao_texto.py:406`, `app/chamados/services/declaracao_texto.py:677`, `app/api/views.py:1119`. |
   | Vigia do carimbo/O330 | Inclui a espera em `sem_plantio_nenhum` e `resposta_enviada_sem_gravacao`, “esperado 0”; o reparador seleciona esse universo. **PROCEDENTE.** `app/chamados/services/vigia_carimbo.py:97`, `app/chamados/services/vigia_carimbo.py:116`, `app/chamados/services/vigia_carimbo.py:140`, `app/chamados/services/passivo_validado.py:324`. |
   | Cron desse vigia | O padrão não alarma esse passivo: usa apenas `--alarme`; o contador entra com `--alarme-passivo`. **NAO PROCEDE** alarme automático padrão por essa causa. `app/config/crons.py:1751`, `app/chamados/management/commands/vigiar_carimbo_sem_registro.py:73`. |
   | Placar | A metade sai do crédito de perguntas vivas; conta como validação humana, sem exigir aplicação. **NAO PROCEDE** chamar essa contagem de materialização. `app/chamados/services/raiox.py:72`, `app/chamados/services/verbos.py:34`, `app/chamados/services/painel.py:268`. |
   | Auditor I18 | Não a acusa como sem dono, pois exige `recusa_motivo=''`. **NAO PROCEDE** essa falsa orfandade específica. `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`. |

   **PROCEDENTE — fechamento contingente à irmã:** a espera não bloqueia C7 por si. Se a irmã sair do conjunto vivo/sem-veredito, `pode_fechar()` pode autorizar fechamento com a primeira ainda sem aplicação; há fechadores automáticos que consomem esse resultado. Cura: representar a pendência do ato no juiz de fechamento e testar retirada da irmã entre aceites. `app/chamados/models.py:1851`, `app/chamados/models.py:1861`, `app/chamados/services/becos.py:56`.

   `vigia_fio` incorpora classes do vigia do carimbo; `o_que_segura` delega à porta alcançável. Reabertura ou impacto quantitativo dessas esperas em produção: **[nao verificado]**. `app/chamados/services/vigia_fio.py:463`, `app/chamados/services/vigia_fio.py:485`, `app/folha/fora_da_folha.py:65`, `app/folha/fora_da_folha.py:221`.

   **Cura comum:** os leitores acima devem consumir um estado canônico de espera antes de diagnosticar falha, preservando separadamente a dívida real de gravação. `app/chamados/services/materializacao.py:496`.

4. **IDENTIDADE — PROCEDENTE: contar uma saída e uma volta não prova a mesma pausa.**

   O identificador consulta disputa/data/motivo e compara o mapa global, que hoje fornece sempre `S/hii` e `E/hfi`. Não consulta os slots reais do dia. Assim, uma saída remanescente da primeira pausa e uma volta da segunda passam pela identificação. As duas pausas compartilham os mesmos motivos. Cura mínima: abster quando houver múltiplas pausas reais sem identidade suficiente; solução completa: vincular os slots pela autoridade existente de marcos. `app/chamados/services/materializacao.py:70`, `app/chamados/services/materializacao.py:84`, `app/chamados/juizes.py:885`, `app/ponto/marcos.py:35`.

   `MARCO_DA_PERGUNTA`, `PARES_PAUSA_DNA`, `instancia_do_motivo` e a leitura das lâmpadas respondem perguntas diferentes; não são outro agrupador equivalente de perguntas. Não precisam chamar o agrupador novo. A divergência é ele afirmar identidade sem consumir a distinção real entre pausas. `app/chamados/juizes.py:882`, `app/escala/models.py:1652`, `app/chamados/services/declaracao_texto.py:42`, `app/chamados/juizes.py:917`.

   **PROCEDENTE — prova insuficiente de preservação de HEAD:** R7 conserva escala de uma pausa e cria quatro perguntas; prova ambiguidade por quantidade, não duas pausas no DNA nem o casal cruzado remanescente. Para quatro elegíveis há fallback avulso; preservação universal com duas pausas reais: **[nao verificado]**. `app/chamados/tests/test_bug_par_intervalo.py:45`, `app/chamados/tests/test_bug_par_intervalo.py:255`, `app/chamados/services/materializacao.py:77`, `app/chamados/services/materializacao.py:1400`.

5. **TEXTO — parcialmente atendido.**

   **NAO PROCEDE** espera por substring: existe chave exata, reconhecida antes das regras textuais, mantendo retorno triplo. Os consumidores desempacotam esse contrato sem enumeração fechada de códigos. `app/chamados/catalogo/recusa.py:25`, `app/chamados/catalogo/recusa.py:95`, `app/chamados/services/acoes_disputa.py:34`, `app/chamados/services/fio.py:703`, `app/chamados/services/validacao.py:548`.

   **NAO PROCEDE** “colada” por vizinha distante no novo ramo: ele exige mesmo colaborador/tipo, não retratada, dentro de ±60 segundos inclusivos. **PROCEDENTE**, porém, a duplicação dessa regra fora da porta. Cura: extrair o predicado de leitura em `registro_batida.py` e reutilizá-lo nos dois consumidores. `app/chamados/services/materializacao.py:1328`, `app/chamados/services/materializacao.py:1333`, `app/ponto/registro_batida.py:106`.

   O prefixo de recusa conjunta conserva a causa para classificação; a casca de validação repassa a mensagem de espera sem anunciar ponto gravado. **NAO PROCEDE** quebra desses contratos. Permanecem as divergências de leitores do item 3. `app/chamados/services/materializacao.py:1362`, `app/chamados/catalogo/recusa.py:99`, `app/chamados/services/acoes_disputa.py:50`.

   O modal ganhou caixas de espera nos ramos validada/não validada. É renderizado pela casca modal e pelo retorno parcial compartilhado pelos POSTs das duas cascas; o GET do fio completo usa outro template. O cartão de detalhe continua apenas com “validada”. **PROCEDENTE** apresentação incompleta; cura: compartilhar a apresentação do estado. `app/templates/chamados/partials/modal_fio.html:229`, `app/templates/chamados/partials/modal_fio.html:243`, `app/chamados/views.py:927`, `app/chamados/services/fio.py:494`, `app/chamados/views.py:674`, `app/chamados/views.py:697`, `app/templates/chamados/partials/_pergunta_card.html:20`.

6. **ROLLBACK/E1 — atendido no caso normal; PROCEDENTE a exceção O330.**

   A falha da segunda escrita desfaz as novas batidas/carimbos antes da recusa; depois há releitura e devolução das validações, acrescentando decisões. Não há edição/apagamento de decisões anteriores nesse fluxo. **NAO PROCEDE** quebra append-only demonstrada pelo diff. `app/chamados/services/materializacao.py:1315`, `app/chamados/services/materializacao.py:1356`, `app/chamados/services/materializacao.py:1361`, `app/chamados/services/validacao.py:202`, `app/chamados/services/validacao.py:208`, `app/chamados/models.py:3943`.

   **PROCEDENTE — carimbo prévio sem lastro:** O330 admite esse estado; rollback restaura o carimbo anterior, e a devolução retorna sem limpar validação quando `materializada_em` permanece preenchido. Portanto não garante “zero materializada_em e ambas devolvidas” nesse universo. Cura: tratar explicitamente o descarimbo sem lastro pela porta dona antes da devolução, preservando E1. R4 não cobre essa combinação. `app/chamados/services/passivo_validado.py:531`, `app/chamados/services/materializacao.py:73`, `app/chamados/services/validacao.py:190`, `app/chamados/models.py:3203`, `app/chamados/tests/test_bug_par_intervalo.py:182`, `app/chamados/tests/test_bug_par_intervalo.py:390`.

7. **CLASSES/PROVAS — não tomar GREEN ou RED agregado como demonstração completa.**

   **NAO PROCEDE** segundo escritor novo de batida/carimbo fora das portas nesse ramo: criação, vínculo e carimbo usam as portas existentes. **PROCEDENTE** derivação paralela de proximidade e identidade insuficiente, conforme itens 4–5. `app/chamados/services/materializacao.py:987`, `app/chamados/services/materializacao.py:1008`, `app/chamados/services/materializacao.py:1256`.

   **FILA — relógio:** o helper extraído lê `now()` por metade, e a aprovação lê outro instante. A expressão já existia no corpo anterior fornecido; regressão temporal concreta: **[nao verificado]**. Cura sugerida: receber o instante da borda do ato. `app/chamados/services/materializacao.py:1223`, `app/chamados/services/materializacao.py:1348`, `app/chamados/services/materializacao.py:993`.

   Os mutantes substituem função real no módulo e executam teste-alvo; morte efetiva por asserção de M1–M18: **[nao verificado]**. M1/M10 miram rollback/devolução; M2/M3 espera/guarda individual; M4–M6 julgamento final; M7/M13 ambiguidade/contagem; M8 proximidade; M9 mapa; M11–M18 reconciliação, contagem, voo, travas e universos. `app/chamados/tests/mutantes_bug_par_intervalo.py:18`, `app/chamados/tests/mutantes_bug_par_intervalo.py:43`, `app/chamados/tests/mutantes_bug_par_intervalo.py:53`, `app/chamados/tests/mutantes_bug_par_intervalo.py:127`.

   **PROCEDENTE:** M9 só altera artificialmente o mapa; não morde identidade real de duas pausas. **FILA:** M15/M18 conferem argumentos de trava, não concorrência SQL. R9 contém controles de comportamento legado que podem falhar na base apenas pelo import novo; isso não demonstra necessidade funcional da cura. `app/chamados/tests/test_bug_par_intervalo.py:463`, `app/chamados/tests/test_bug_par_intervalo.py:332`, `app/chamados/tests/test_bug_par_intervalo.py:361`, `app/chamados/tests/test_bug_par_intervalo.py:419`, `app/chamados/tests/test_bug_par_intervalo.py:446`.

8. **ESCRITORES/LEITORES E SEMÂNTICA — inventário central, complementando os sítios acima.**

   | Campo/fonte | Quem escreve | Quem lê |
   |---|---|---|
   | `validada_em/por` | Validação/devolução: `app/chamados/services/validacao.py:134`, `:202`; nova resposta: `app/chamados/services/disputa_emissao.py:1477`; carimbo/ressuscitação: `app/chamados/models.py:3135`, `:1565`. | C6/C7/estado: `app/chamados/juizes.py:1153`, `app/chamados/models.py:1851`, `:3041`; declaração, explicador, vigias e placares: item 3; projeção E1: `app/chamados/models.py:3924`; reconciliação: `app/chamados/reconciliador.py:629`, `:1049`. |
   | `materializada_em/por`, `via_resolucao` | Porta interna e descarimbo em lote: `app/chamados/models.py:3099`, `:3266`; entradas `carimbar`, identidade, descarimbar: `:3103`, `:3168`, `:3203`. | Identificador/seletor: `app/chamados/services/materializacao.py:73`, `:1560`; fato/C6: `app/chamados/juizes.py:660`, `:1164`; relavra: `app/chamados/services/relavra_via.py:181`, `:244`; placar/veredito: `app/chamados/services/painel.py:67`, `app/chamados/veredito_lavrado.py:188`. |
   | `recusa_motivo`, `pendencia_admin` | Ramo hora/recusa/limpeza conjunta: `app/chamados/services/materializacao.py:910`, `:1163`, `:1352`; recusa humana: `app/chamados/services/validacao.py:604`; escape: `app/chamados/services/disputa_emissao.py:1494`; identidade: `app/chamados/models.py:3188`. | Devolução/catálogo: `app/chamados/services/validacao.py:193`, `app/chamados/catalogo/recusa.py:95`; explicador: `app/chamados/services/explicador.py:85`; auditorias: `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`; flip: `app/ponto/services/flip_auto.py:132`; apresentação residual: `app/templates/chamados/partials/_card_atendimento.html:186`. |
   | `resposta_colab`, `respondida_em` | Resposta: `app/chamados/services/disputa_emissao.py:1464`; auto-resposta por carimbo: `app/chamados/models.py:3130`. | Prontidão/derivação: `app/chamados/services/materializacao.py:1384`, `:410`; C6/C7: `app/chamados/juizes.py:1169`, `app/chamados/models.py:1852`; E1: `app/chamados/models.py:3931`. |
   | Identidade, motivo, dia, destino, conversa | Abertura factual/conversa: `app/chamados/models.py:2784`, `:2809`, `:3016`. | Identificador/travas: `app/chamados/services/materializacao.py:67`, `:99`; derivação: `:393`; fechamento/alcance: `app/chamados/models.py:1850`, `app/chamados/juizes.py:948`. |
   | `contexto_json.marcos_faltantes` | Nascimento/reconstrução/renovação: `app/chamados/reconciliador.py:707`, `app/ponto/services/cartorio.py:1742`, `app/ponto/management/commands/reconciliar_grade.py:473`; porta de renovação: `app/chamados/services/contexto.py:102`. | Casamento/plantio: `app/chamados/juizes.py:534`, `app/chamados/services/materializacao.py:327`, `:463`; emissão: `app/chamados/services/disputa_emissao.py:1140`; lastro/grade: `app/ponto/services/lastro.py:75`, `app/escala/utils.py:1066`; regeneração: `app/escala/services/regeneracao.py:149`. |
   | Batida: `timestamp`, tipo, origem, aprovação, vínculo | Criação: `app/ponto/registro_batida.py:157`; anotação: `:254`; vínculo: `app/chamados/services/materializacao.py:1008`. | Proximidade/paridade: `app/ponto/registro_batida.py:107`, `app/chamados/services/materializacao.py:148`; lastro: `app/chamados/juizes.py:267`; competência: `app/ponto/janelas.py:84`. |

   **PROCEDENTE** a divergência semântica de `validada_em`: aceite pendente no escritor novo, terminalidade/falha em leitores antigos. **FILA** a fronteira de competência: O330 agrupa pelo dia do turno, enquanto a porta verifica o dia civil local do timestamp; são cálculos distintos já presentes, sem contraprova produtiva neste dado. A derivação overnight continua centralizada e aware; não encontrei nova conversão de fuso nesse diff. `app/chamados/services/materializacao.py:1402`, `app/chamados/juizes.py:1153`, `app/chamados/services/passivo_validado.py:113`, `app/ponto/janelas.py:84`, `app/chamados/services/materializacao.py:468`, `app/chamados/services/materializacao.py:475`.

   Exaustividade de **todos** os leitores/escritores de cada atributo incidental transitivamente alcançado pelo diff, além deste inventário: **[nao verificado]**. Execução independente de RED/GREEN, população completa e resultados dos mutantes: **[nao verificado]**.

## 09/11 08:31 · sessao 0c3a374ff426 · - · - · origem=Ronald (tmux)

Achados:

- **PROCEDENTE — explicador contradiz a espera.** A primeira metade torna-se `VALIDADA` (`app/chamados/models.py:3041`); sem batida, `explicar_pergunta` a classifica `validada_sem_gravar`, manda corrigir cadastro/escala ou TI/DP, sem verificar a espera (`app/chamados/services/explicador.py:97`, `app/chamados/services/explicador.py:103`). Com batidas no entorno, atribui intervalo deslocado ao cadastro (`app/chamados/services/explicador.py:47`, `app/chamados/services/explicador.py:58`). Esse explicador é incluído no fio administrativo (`app/chamados/services/fio.py:302`, `app/chamados/services/fio.py:312`) e o modal mostra “o sistema não conseguiu gravar”, com dono cadastro/TI (`app/templates/chamados/partials/modal_fio.html:449`). **Cura:** explicador consumir o estado canônico `aguardando_par`, antes de diagnosticar falha.

- **PROCEDENTE — declaração/app trata a espera como falha.** `_resposta_validada` lê somente `validada_em` (`app/chamados/services/declaracao_texto.py:221`). As portas de declaração devolvem `validada_sem_registro` e mandam refazer o horário pela supervisão (`app/chamados/services/declaracao_texto.py:406`, `app/chamados/services/declaracao_texto.py:677`); a consulta usada pela apresentação replica essa guarda (`app/chamados/services/declaracao_texto.py:272`). Os cards e o formulário continuam anunciando falha/refação (`app/templates/chamados/partials/_card_atendimento.html:215`, `app/templates/chamados/partials/_card_atendimento.html:290`, `app/templates/chamados/partials/modal_fio.html:146`). **Cura:** distinguir explicitamente espera legítima da validação sem gravação defeituosa.

- **PROCEDENTE — proteção de fechamento é contingente à irmã.** A metade validada fica morta para C6 (`app/chamados/juizes.py:1153`), sai de `q_vivas` (`app/chamados/models.py:3359`) e de `respostas_sem_veredito` (`app/chamados/models.py:1851`). `pode_fechar` não exige aplicação das metades validadas (`app/chamados/models.py:1861`). Portanto, se a irmã for retirada/absolvida/trancada e não restar outra pergunta viva/sem-veredito, a disputa pode fechar com aquela metade ainda pendente; o fecho automático usa esse juiz (`app/chamados/services/disputa_emissao.py:1960`, `app/chamados/services/becos.py:56`). No caso normal da fixture, a irmã continua viva e impede fechar (`app/chamados/tests/test_bug_par_intervalo.py:115`). **Cura:** representar a pendência do ato no juiz de fechamento e testar retirada da irmã entre os dois aceites.

- **PROCEDENTE — espera infla dívida de gravação.** `vigia_carimbo.classificar` seleciona todas as horas validadas, inclusive espera (`app/chamados/services/vigia_carimbo.py:97`), e classifica a metade sem batida como `sem_plantio_nenhum` (`app/chamados/services/vigia_carimbo.py:116`); o censo a soma em `resposta_enviada_sem_gravacao`, rotulado “esperado 0” (`app/chamados/services/vigia_carimbo.py:129`, `app/chamados/services/vigia_carimbo.py:140`). O reparador seleciona esse universo (`app/chamados/services/passivo_validado.py:324`). **Cura:** classe explícita de espera, preservando separadamente o contador de falhas reais.

- **NÃO PROCEDE — alarme de passivo no cron padrão.** O command só acrescenta esse contador ao alarme com `--alarme-passivo` (`app/chamados/management/commands/vigiar_carimbo_sem_registro.py:73`); o cron usa apenas `--alarme` (`app/config/crons.py:1751`). A combinação dos dois argumentos pode alarmar a espera legítima (`app/chamados/management/commands/vigiar_carimbo_sem_registro.py:77`).

- **NÃO PROCEDE — materializada no estado comum.** Sem `materializada_em`, o estado é `VALIDADA`, não `RESOLVIDA` (`app/chamados/models.py:3043`); `pergunta_sem_acao_possivel` devolve False (`app/chamados/services/raiox.py:173`). A espera fica fora do crédito de perguntas vivas, pois este lê `q_vivas` (`app/chamados/services/raiox.py:72`). Badge e verbos seguem esse crédito (`app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`); a irmã determina a ação remanescente.

- **NÃO PROCEDE — devolução automática da espera pela porta nova.** A devolução exclui exatamente `ESPERA_PAR_INTERVALO` (`app/chamados/services/validacao.py:192`); a validação retorna antes da varredura/fecho (`app/chamados/services/validacao.py:482`, `app/chamados/services/validacao.py:489`).

- **FILA — impacto efetivo em `vigia_fio`/`o_que_segura`.** `vigia_fio.fio_do_dia` delega ao C6 (`app/chamados/services/vigia_fio.py:165`); seus dias mortos e decisões pendentes recebem classes de `vigia_carimbo` (`app/chamados/services/vigia_fio.py:463`, `app/chamados/services/vigia_fio.py:485`). `o_que_segura` lê `porta_alcancavel_do_dia` e relata o papel/fila dessa porta (`app/folha/fora_da_folha.py:65`, `app/folha/fora_da_folha.py:221`). A ocorrência numérica em produção e reabertura por esses leitores permanecem **[nao verificado]**.

Inventário de escritores dos três campos de PerguntaDisputa, por código executável:

| Campo | Escritores |
|---|---|
| `validada_em` | `_carimbar_validacao` (`app/chamados/services/validacao.py:134`); devolução (`app/chamados/services/validacao.py:202`); resposta nova (`app/chamados/services/disputa_emissao.py:1477`); `carimbar` (`app/chamados/models.py:3135`); ressuscitação em lote (`app/chamados/models.py:1565`); restauração apenas em memória (`app/chamados/models.py:3164`). |
| `materializada_em` | `_escrever_carimbo` (`app/chamados/models.py:3099`), chamado pelas portas `carimbar`/identidade; descarimbo em lote (`app/chamados/models.py:3266`). `regularizacao_ext.py:198` escreve campo homônimo de outro modelo. |
| `recusa_motivo` | `_materializar_resposta_hora` (`app/chamados/services/materializacao.py:910`); `_carimbar_recusa` (`app/chamados/services/materializacao.py:1163`); limpeza do par (`app/chamados/services/materializacao.py:1352`); recusa humana (`app/chamados/services/validacao.py:604`); resposta escape (`app/chamados/services/disputa_emissao.py:1494`); `carimbar_identidade` (`app/chamados/models.py:3188`), também usado pela deduplicação (`app/chamados/services/fio.py:1351`). |

Outros leitores relevantes fora do diff:

- `validada_em`: inscrições/âncoras (`app/chamados/models.py:310`, `app/chamados/models.py:344`); C7 (`app/chamados/models.py:1851`); projeção E1 (`app/chamados/models.py:3924`); irmãs sem consumo (`app/chamados/juizes.py:1769`); reconciliação de órfãs (`app/chamados/reconciliador.py:629`, `app/chamados/reconciliador.py:1049`); resposta/correção (`app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/acoes_disputa.py:345`); contagem de validações da equipe (`app/chamados/services/verbos.py:240`, `app/chamados/services/painel.py:268`); procedência de batida (`app/ponto/services/procedencia_batida.py:74`); auditoria (`app/chamados/management/commands/auditar_invariantes_chamados.py:1075`).
- `materializada_em`: juiz do fato (`app/chamados/juizes.py:660`, `app/chamados/juizes.py:687`); C6 (`app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1227`); reutilização (`app/chamados/juizes.py:1245`); leitor da ação possível (`app/chamados/services/raiox.py:173`); marco do placar (`app/chamados/services/painel.py:67`); marco do veredito (`app/chamados/veredito_lavrado.py:188`); abertura de perguntas (`app/chamados/services/disputa_emissao.py:228`, `app/chamados/services/disputa_emissao.py:1067`, `app/chamados/services/disputa_emissao.py:1323`); reconciliação (`app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:1500`); desfazedor de carimbo (`app/chamados/services/relavra_via.py:181`, `app/chamados/services/relavra_via.py:244`).
- `recusa_motivo`: devolução (`app/chamados/services/validacao.py:193`); explicador (`app/chamados/services/explicador.py:90`); fila de flip (`app/ponto/services/flip_auto.py:132`); auditorias de ausência de dono (`app/chamados/management/commands/auditar_invariantes_chamados.py:1075`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`); apresentação crua residual no card (`app/templates/chamados/partials/_card_atendimento.html:186`).

A espera não perde o registro de validação E1: `_carimbar_validacao` grava projeção e decisão na mesma transação, usando o mesmo instante (`app/chamados/services/validacao.py:134`, `app/chamados/services/validacao.py:136`, `app/chamados/services/validacao.py:139`). A contagem da equipe aumenta pelo aceite mesmo sem aplicação, coerente com contar validações (`app/chamados/services/verbos.py:240`); chamá-la de aplicação seria **[nao verificado]**.

## 09/11 08:31 · sessao 1c264a102439 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura:

- **PROCEDENTE — identidade não prova “mesma pausa”.** `identificar_par_intervalo` agrupa apenas disputa/data/motivo e consulta o mapa global; não lê DNA do dia, hora do marco, destino nem `instancia_marco`. Com o mapa atual, a comparação canônica sempre recebe `S/hii + E/hfi`. Uma saída sobrevivente da primeira pausa e uma volta sobrevivente da segunda passam como par. `app/chamados/services/materializacao.py:70`; `app/chamados/services/materializacao.py:84`; `app/chamados/juizes.py:885`. As duas pausas efetivamente compartilham motivos no emissor: `app/ponto/marcos.py:35`. Cura: identificar os slots reais pela autoridade de marcos, ou abster quando o DNA tiver múltiplas pausas sem identidade suficiente; não inventar índice.

- **PROCEDENTE — a contraprova R7 não cobre duas pausas reais.** O teste cria quatro perguntas, mas conserva a escala de uma pausa e testa a ambiguidade por contagem. Não cobre duas perguntas remanescentes de pausas distintas nem duas pausas no DNA. `app/chamados/tests/test_bug_par_intervalo.py:45`; `app/chamados/tests/test_bug_par_intervalo.py:255`. A preservação universal de HEAD com duas pausas é **[nao verificado]**; para quatro perguntas elegíveis o código cai no avulso. `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:1400`.

- **PROCEDENTE — duplicação do predicado de proximidade.** O par repete consulta e tolerância ±60s inclusivos da porta. `app/chamados/services/materializacao.py:1328`; `app/ponto/registro_batida.py:106`. Cura: extrair consulta/predicado de leitura no dono `registro_batida.py`, consumido pela porta e pelo par. A mensagem “colada” tem testemunha concreta nesse novo caminho, filtrada por mesmo colaborador/tipo e não retratada. `app/chamados/services/materializacao.py:1329`; `app/chamados/services/materializacao.py:1333`. O catálogo isoladamente apenas reconhece substring, sem medir batida. `app/chamados/catalogo/recusa.py:39`; `app/chamados/catalogo/recusa.py:99`.

- **PROCEDENTE — leitor do app interpreta espera como defeito.** `declarar_hora_colab` e `aplicar_declaracao` consideram qualquer `validada_em` uma `validada_sem_registro` quando o marco falta e mandam a supervisão refazer o horário, sem distinguir a nova espera legítima. `app/chamados/services/declaracao_texto.py:210`; `app/chamados/services/declaracao_texto.py:372`; `app/chamados/services/declaracao_texto.py:406`; `app/chamados/services/declaracao_texto.py:662`; `app/chamados/services/declaracao_texto.py:677`. Cura: consultar estado de aplicação/espera antes desse retorno e devolver “falta a outra metade”, preservando a declaração.

- **NAO PROCEDE — espera depende de substring.** Há chave exata e retorno triplo compatível com `traduzir_recusa`; reconhecimento ocorre antes das regras por substring. `app/chamados/catalogo/recusa.py:25`; `app/chamados/catalogo/recusa.py:95`. Recusa conjunta preserva os marcadores de classificação porque apenas prefixa a causa. `app/chamados/services/materializacao.py:1362`; `app/chamados/catalogo/recusa.py:99`. Chamadores desempacotam três valores sem enumeração fechada: `app/chamados/services/acoes_disputa.py:34`; `app/chamados/services/acoes_disputa.py:172`; `app/chamados/services/fio.py:703`; `app/chamados/services/passivo_validado.py:144`; `app/chamados/services/validacao.py:548`.

- **NAO PROCEDE — casca de validação anuncia ponto gravado para espera.** Estado diferente de `validada` e `ok=True` usa a mensagem do serviço. `app/chamados/services/acoes_disputa.py:50`; `app/chamados/services/acoes_disputa.py:52`; `app/chamados/services/validacao.py:485`. `aplicar_declaracao` também repassa estado/mensagem do serviço na primeira aplicação. `app/chamados/services/declaracao_texto.py:690`.

- **PROCEDENTE — apresentação não alcança todas as telas.** O modal acrescentou caixas de espera nos ramos validada e não validada. `app/templates/chamados/partials/modal_fio.html:229`; `app/templates/chamados/partials/modal_fio.html:243`. O cartão da página de detalhe só mostra “validada” e não lê `estado_aplicacao`. `app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/detalhe_local.html:310`. Cura: compartilhar apresentação do estado de aplicação. O modal é renderizado diretamente pela casca modal e nos retornos parciais usados pelas duas cascas; a página completa do fio usa outro template, sem incluir o modal. `app/chamados/views.py:927`; `app/chamados/services/fio.py:494`; `app/chamados/views.py:674`; `app/chamados/views.py:697`. Há outro render do modal no retorno de recobrança. `app/chamados/views.py:1399`.

- **FILA — relógio fora da borda.** `_carimbar_aplicacao`, extração agora compartilhada por duas metades, lê relógio por chamada; aprovação da batida lê outro instante. Não existe instante único recebido pelo ato. `app/chamados/services/materializacao.py:1223`; `app/chamados/services/materializacao.py:1348`; `app/chamados/services/materializacao.py:993`. É extração de comportamento anterior no diff fornecido, não prova de regressão temporal. Cura sugerida: instante recebido da borda e repassado às duas escritas/carimbos. Efeito em produção **[nao verificado]**.

Mutantes e RED:

- **NAO PROCEDE quanto a mutação apenas de fonte fictícia:** o executor recompila a função real no dicionário global do módulo e chama o teste selecionado; verifica âncora única. `app/chamados/tests/mutantes_bug_par_intervalo.py:76`; `app/chamados/tests/mutantes_bug_par_intervalo.py:127`. Execução/morte efetiva M1–M18: **[nao verificado]**.
- **FILA — R3 não prova ausência de deadlock:** faz duas validações sequenciais. `app/chamados/tests/test_bug_par_intervalo.py:174`. M15 e M18 verificam os IDs passados a `trava_na_ordem`, sem duas conexões concorrentes nem observação de locks SQL. `app/chamados/tests/test_bug_par_intervalo.py:332`; `app/chamados/tests/test_bug_par_intervalo.py:361`. Cura: teste concorrente de duas transações reais.
- M1/M10 apontam ao rollback/devolução; M3 à guarda individual; M4 à paridade final; M5 à ordem temporal; M6 ao lastro; M8 à proximidade; M11 à reconciliação; M12 à contagem; M14 ao voo; M16 ao universo; M17 ao filtro de carimbo; M7/M13 à ambiguidade/contagem; M9 à tabela global. Correspondências estão declaradas no executor. `app/chamados/tests/mutantes_bug_par_intervalo.py:18`; `app/chamados/tests/mutantes_bug_par_intervalo.py:30`; `app/chamados/tests/mutantes_bug_par_intervalo.py:43`; `app/chamados/tests/mutantes_bug_par_intervalo.py:53`; `app/chamados/tests/mutantes_bug_par_intervalo.py:60`.
- **PROCEDENTE — M9 não morde identidade real de pausa:** só altera `MARCO_DA_PERGUNTA` artificialmente; não testa DNA com duas pausas/casal cruzado. `app/chamados/tests/test_bug_par_intervalo.py:463`.
- **FILA — RED por símbolo ausente não demonstra cura funcional:** controles R9 de ausência/irmã já aplicada exigem a função nova por import, embora seus resultados preservem o caminho avulso; a base pode errar no import antes de testar o comportamento. `app/chamados/tests/test_bug_par_intervalo.py:419`; `app/chamados/tests/test_bug_par_intervalo.py:431`; `app/chamados/tests/test_bug_par_intervalo.py:446`. R7 também exige a chave nova de contagem depois de conferir a recusa legada. `app/chamados/tests/test_bug_par_intervalo.py:261`; `app/chamados/tests/test_bug_par_intervalo.py:270`. R1/R2/R3, por sua vez, exigem explicitamente espera e aplicação das duas; não são meros controles de HEAD. `app/chamados/tests/test_bug_par_intervalo.py:151`; `app/chamados/tests/test_bug_par_intervalo.py:164`; `app/chamados/tests/test_bug_par_intervalo.py:174`.

Fontes paralelas existentes, com perguntas diferentes: mapa motivo→marco `app/chamados/juizes.py:882`; classificação marco→motivo `app/ponto/marcos.py:14`; posição→motivo/instância `app/chamados/services/declaracao_texto.py:42`; pares canônicos de DNA `app/escala/models.py:1652`; leitura de todas as luzes das pausas `app/chamados/juizes.py:917`; defesa que se abstém com duas pausas `app/chamados/juizes.py:1124`. Não são outro agrupador de perguntas equivalente a `identificar_par_intervalo`; exigirem chamar esse agrupador seria inverter dependências. A divergência concreta é o agrupador novo afirmar identidade sem usar os slots que essas autoridades distinguem. `app/chamados/services/materializacao.py:84`.

## 09/11 08:31 · sessao f854c2b1485a · - · - · origem=Ronald (tmux)

**ATO/TRAVAS — NAO PROCEDE para par identificado; universalidade exige ressalvas.**

- Chamadores AST de `_materializar_uma`: `validar_pergunta`, import sem alias, `app/chamados/services/validacao.py:466`, chamadas `:469`/`:474`; varredura, nome local, `app/chamados/services/materializacao.py:1565`; `_plantar_o_dia`, import sem alias `app/chamados/services/passivo_validado.py:39`, chamada `:529`.
- Chamadores AST de `_materializar_resposta_hora`: somente `_materializar_par`, nome local, `app/chamados/services/materializacao.py:1336`, e `_materializar_avulsa`, nome local, `:1447`. Existe reexport, sem chamada, em `app/chamados/models.py:4077`.
- Par identificado: ambas entram em voo antes da primeira escrita; mesmo savepoint; ramo individual ignora guarda intermediária; guarda final, lastro, carimbos e reconciliação vêm depois das duas chamadas. `app/chamados/services/materializacao.py:1315`, `:1317`, `:1336`, `:935`, `:1338`, `:1345`, `:1348`, `:1355`.
- Toda **nova criação** desse ramo usa `registrar_batida`, import sem alias; porém reuso consulta batida e pula a chamada à porta, portanto “ambas sempre chamam registrar” literalmente não é verdadeiro. `app/chamados/services/materializacao.py:919`, `:925`, `:931`, `:986`, `:987`; `app/chamados/juizes.py:235`.
- Continua havendo aplicação avulsa de pergunta de intervalo quando identidade retorna `sem_par`, `par_ambiguo` ou `marcos_sem_par`; pode criar só uma metade. Isso preserva a rota individual para esse universo. `app/chamados/services/materializacao.py:77`, `:79`, `:85`, `:1400`, `:1447`. O teste de irmã já aplicada exige esse comportamento. `app/chamados/tests/test_bug_par_intervalo.py:444`.

**TRAVAS — NAO PROCEDE para inversão entre esses dois validadores.**

- Chamadores AST de `_travar_ato_materializacao`, todos sem alias: `validacao.py:434` (import `:433`), `materializacao.py:1379` (local), `passivo_validado.py:432` (import `:431`) e `:520` (import `:513`). Transações anteriores: `validacao.py:428`, `materializacao.py:1378`, `passivo_validado.py:371`/`:519`.
- O helper pega Colaborador antes de enumerar o universo; depois trava Disputas, Chamados e Perguntas. A porta ordena cada grupo por PK. `app/chamados/services/materializacao.py:95`, `:97`, `:104`; `app/chamados/models.py:2674`, `:2683`, `:2696`, `:2700`.
- Mesmo colaborador serializa os dois validadores antes da primeira Pergunta; validar saída/volta em ordem inversa não cria a inversão de travas descrita. `app/chamados/services/validacao.py:434`, `:439`; `app/chamados/services/materializacao.py:95`. R3 é sequencial; concorrência executada **[nao verificado]**. `app/chamados/tests/test_bug_par_intervalo.py:174`.

**ROLLBACK — NAO PROCEDE no caso normal; PROCEDENTE na promessa universal para O330.**

- Falha da segunda escrita sai do savepoint antes da recusa; releitura recupera estado anterior; recusa usa `encerrar_colisao=False`; devolução limpa validação e acrescenta decisão. Novas batidas/carimbos do par são revertidos pela estrutura transacional. `app/chamados/services/materializacao.py:1315`, `:1356`, `:1361`, `:1364`; `app/chamados/services/validacao.py:198`, `:202`, `:205`, `:208`.
- **Contraexemplo O330:** o reparador inclui perguntas com `materializada_em` anterior sem lastro; rollback restaura esse carimbo; `devolver_validacao_recusada` retorna antes de limpar validação se `materializada_em` não é nulo. Portanto não garante “zero materializada_em e devolução das duas” nesse universo. Cura sugerida: tratamento explícito pelo dono de descarimbo para carimbo sem lastro, antes da devolução, preservando E1; ou delimitar formalmente o contrato como preservação do estado anterior. `app/chamados/services/passivo_validado.py:531`; `app/chamados/services/materializacao.py:73`, `:74`, `:1361`, `:1365`; `app/chamados/services/validacao.py:188`, `:190`; porta de descarimbo `app/chamados/models.py:3203`.
- E1 do fluxo novo acrescenta `VALIDACAO`/`DEVOLUCAO`; não atualiza/apaga decisão anterior. A projeção legada é capturada antes da troca. `app/chamados/services/validacao.py:133`, `:139`, `:201`, `:208`. Modelo recusa edição/apagamento e porta usa `bulk_create`. `app/chamados/models.py:3903`, `:3908`, `:3943`.
- R4 verifica falha da segunda escrita no validador normal; não cobre esse rollback de O330 com carimbo prévio. `app/chamados/tests/test_bug_par_intervalo.py:182`, `:194`; fixtures O330 com carimbos falsos só verificam sucesso/exclusão de universo. `app/chamados/tests/test_bug_par_intervalo.py:382`, `:390`, `:403`.

**OUTROS CAMINHOS — FILA, por serem atos diferentes de aplicar respostas desse par.**

- `veredito_celula` pode preencher marcos de intervalo pela porta e depois fechar perguntas do dia; simula o conjunto antes, mas captura erro por escrita e só dá rollback quando nenhuma ação teve sucesso. Falha posterior pode conservar a primeira batida. É decisão do dia, sem chamar `_materializar_resposta_hora`/identificador de perguntas. Cura da unidade desse outro ato fica fora da correção estreita, mas impede afirmar atomicidade universal de todo preenchimento de intervalo. `app/ponto/services/veredito_celula.py:290`, `:305`, `:320`, `:322`, `:327`, `:342`, `:346`, `:179`, `:189`.
- Regularização externa também tem ramo próprio de par de intervalo; `_cria_batida` transforma recusa em retorno `None`, permitindo prosseguir com a irmã e materializar o lote. Não aplica `PerguntaDisputa`. `app/chamados/services/regularizacao_ext.py:77`, `:83`, `:88`, `:121`, `:167`, `:172`, `:176`, `:197`.

## 09/11 08:26 · sessao 2170a5030b34 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 08:26 · sessao c216c7c7d88f · - · - · origem=Ronald (tmux)

Fixtures e riscos consolidados:

- Dois motivos válidos no mesmo dia: `saida_sem_entrada` e `orfao_14h`; derivam instâncias `inicio`/`fim`, evitando colisão da identidade. `chamados/services/declaracao_texto.py:44`; `chamados/models.py:3425`.
- Crie `Empresa`, `User`, `Colaborador(nome='col001', usuario=user, deve_trocar_senha=False, plataforma_principal='android')`, `CelulaDia` com DNA 08–17, disputa e perguntas por `.objects.create()`. Reafirme após montar as linhas `veredito='furo'`, `veredito_via='cartorio'`, ata com ambas as lâmpadas apagadas. C6 responde viva pelo veredito acusador. `chamados/juizes.py:1354`; exemplo existente em `chamados/tests/test_fio_le_o_juiz.py:50`.
- Para M2/M3/morta, use `QuerySet.update()` nos carimbos `fechada_em`, `respondida_em`/`resposta_colab` ou `validada_em`. Evita efeitos dos signals de fechamento e continua sendo ORM real. `chamados/signals.py:18`; `chamados/signals.py:237`.
- M5: para cada colaborador, repita `orfao_14h` em datas distintas; a unicidade inclui dia e motivo. `chamados/models.py:2437`.
- Ambos os kernels: `APIRequestFactory().get()`, `force_authenticate()`, chamada à função importada de cada módulo; padrão existente em `api/tests/test_me_posto_geo.py:103`.
- Dashboard: `Client.force_login()` e `reverse('core:dashboard')`, já empregado em `core/tests/test_dashboard_badge.py:21`. Plataforma preenchida evita escrita de classificação durante GET. `core/views.py:143`.
- Registro a baixar: exclusivamente a expressão `_p(...)` de duas linhas em `core/juizes.py:759`.
- Delete `test_badge_disputa_pendente.py`: sustenta explicitamente contagem por disputa. `chamados/tests/test_badge_disputa_pendente.py:8`.
- Migre `test_esmeril2_left_join.py`: preserve defesa contra pai sem filho e altere duas perguntas para contagem **2** pela consulta única. `chamados/tests/test_esmeril2_left_join.py:14`.
- Migre referências do fio e dashboard para `len(consulta_do_app(... )['pendentes'])`; preservam comparação com vitalidade e tela. `chamados/tests/test_fio_le_o_juiz.py:261`; `core/tests/test_dashboard_badge.py:37`.
- Preserve M6: alimentação pronta, sem mock de ORM/C6. `chamados/tests/test_e3_pendentes_app.py:374`. Mutantes da matriz precisam recompilar funções reais em memória para alcançar os novos testes ORM; hoje só alteram `SOURCES` consumido pelos executores fake. `chamados/tests/test_e3_pendentes_app.py:414`.

Não escrevi arquivos nem executei testes. [nao verificado] [nao verificado]

## 09/11 08:25 · sessao afb90b29fac5 · - · - · origem=Ronald (tmux)

- Fontes que C17 já lê: `chamados/juizes.py::alimentar_portas` fornece `verbo` por `particionar_verbos(qs_vivos())` (`:1290-1293`), `fila_ids`/`fila_cel` por `qs_na_fila()` (`:1296-1301`). C17 compõe essas fontes em `:1376-1392`. Usar diretamente as autoridades dentro da carga da tarefa evita copiar C17.
- Atenção ao relógio: `particionar_verbos` lê `timezone.now()` em `chamados/services/verbos.py:35`. Acrescentar parâmetro opcional `agora` permite à tarefa preservar seu relógio explícito; `qs_na_fila(agora=)` já existe em `painel.py:158-190`.
- A carga da tarefa já coleta âncoras de perguntas e sua pertença à fila, mas não chamados da célula: `tarefa_do_dia.py:358-361`. O retorno atual sai por `_tarefa_pura` em `:399-402`. Ali cabe enriquecer aditivamente com `rota`, `fila`, `verbos`, `fio`, preservando campos anteriores.
- Fixture ORM de referência: `test_e2_tarefa_do_dia.py:69-138`: Empresa, Praça, Posto, Colaborador, TipoEscala, EscalaColaborador, CelulaDia com DNA e lâmpadas. Reafirmar a ata depois de criar pergunta porque os sinais relavram (`:104-108`). Aplicar `freeze_time` na classe nova e nomes `colNNN`.
- Caso A/B real já demonstrado em `test_o261e_f5b_juiz_porta.py:124-141`: A aberto da célula, B aberto sem célula, pergunta destinada a B. Painel responde `{A: 'decidir', B: 'cobrar'}`; C17 responde `fila=(A,)`.
- Dia isento: fonte é `Colaborador.isento_ponto`, lida por `core/constants.py:13-18`; tarefa extingue ações em `tarefa_do_dia.py:179-187`.
- DECISAO-ABERTA: os anexos determinam que prevalece a tarefa, mas não fixam os valores literais dos campos aditivos nos casos sem pergunta/sem ações. A interpretação precisa ficar explícita nos quatro testes escritos antes da implementação. Execução/comportamento futuro: [nao verificado]. [nao verificado]

## 09/11 08:24 · sessao 2e5a8dc4c11c · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 08:24 · sessao ce974f8b148b · - · - · origem=Ronald (tmux)

6. Os selos têm mordida parcial, sem prova integral do contrato:

- **Leitura sem escrita no preparo capturado:** executam o envelope real nos dois modos, exigem consulta positiva, cluster, `sql_escrita=0`, arquivo intacto e host no argv. A detecção externa é regex sobre `sql.log`. (`bin/tests/test_sonda_sem_grant.sh:82`, `bin/tests/test_sonda_sem_grant.sh:85`, `bin/tests/test_sonda_sem_grant.sh:91`, `bin/tests/test_sonda_sem_grant.sh:93`, `bin/tests/test_sonda_sem_grant.sh:96`)
- **Recusa nomeando falta e porta:** cobrem tabela sem grant, role ausente, catálogo vazio/falho, login falho, HOST antigo e arquivo ausente; exigem rc 5 e ausência da execução simulada. Não há casos separados para arquivo sem senha, permissões erradas, login com usuário/read-only errado, falta exclusiva de sequência/default/USAGE ou tentativa de SQL proibido pelo classificador. (`bin/tests/test_sonda_sem_grant.sh:100`, `bin/tests/test_sonda_sem_grant.sh:109`, `bin/tests/test_sonda_sem_grant.sh:123`; caminhos omitidos em `bin/sonda_leitura.sh:170`, `bin/sonda_leitura.sh:173`, `bin/sonda_leitura.sh:204`)
- **Porta existe e escreve:** cobram algumas frases de DCL/DDL, senha sem HOST, modo 600 e trilha, nos dois destinos. Não procuram outro escritor na árvore; copiam apenas cinco scripts e substituem `sombra_db.sh` por stub. Também não exigem cluster correto para o provisionador. (`bin/tests/test_sonda_sem_grant.sh:67`, `bin/tests/test_sonda_sem_grant.sh:70`, `bin/tests/test_sonda_sem_grant.sh:129`, `bin/tests/test_sonda_sem_grant.sh:133`, `bin/tests/test_sonda_sem_grant.sh:141`)
- **Propagação rc 5 no wrapper:** o novo caso exige rc 5, Codex não chamado, mensagem/porta visíveis e sentinelas brutas escondidas; usa envelope falso. Não cobre integração com envelope real nem payload terminando em 5. (`bin/tests/test_codex_sonda.sh:40`, `bin/tests/test_codex_sonda.sh:55`, `bin/tests/test_codex_sonda.sh:85`)
- **Refazer chama porta antes de carimbar:** verifica somente presença textual e posição das duas linhas; não executa `refazer` nem força falha do provisionador. (`bin/tests/test_sonda_sem_grant.sh:150`)

**O docker falso não cobre o payload.** Captura stdin/argv somente de `docker exec` contendo `psql`; `docker run` apenas imprime `SONDA_RODOU` e devolve zero. Não executa Django, driver ou `/sonda/sonda.py`; `sombra_db.sh` também não participa. (`bin/tests/test_sonda_sem_grant.sh:41`, `bin/tests/test_sonda_sem_grant.sh:46`, `bin/tests/test_sonda_sem_grant.sh:59`, `bin/tests/test_sonda_sem_grant.sh:70`; execução real em `bin/sonda_leitura.sh:241`)

Mutantes plausíveis sobreviventes, por inspeção; execução dos mutantes **[nao verificado]**:

1. Fazer `_escritas_no_texto` sempre devolver zero: nenhum caso injeta comando proibido; o preparo atual continua SELECT. (`bin/sonda_leitura.sh:147`; casos em `bin/tests/test_sonda_sem_grant.sh:85`, `bin/tests/test_sonda_sem_grant.sh:99`)
2. Acrescentar `SELECT ... INTO ...` ou SELECT de função com efeito: o detector externo não contempla `INTO` nem identifica efeitos de funções. (`bin/tests/test_sonda_sem_grant.sh:32`)
3. Remover o `falhou` após provisionamento, mantendo a chamada antes do carimbo: o teste de forma continua satisfeito. (`bin/sombra.sh:306`, `bin/sombra.sh:307`; `bin/tests/test_sonda_sem_grant.sh:150`)
4. Acrescentar escritor em outro arquivo ou no bootstrap verdadeiro: não há censo/assertiva global de exclusividade. (`bin/tests/test_sonda_sem_grant.sh:67`, `bin/tests/test_sonda_sem_grant.sh:70`)
5. Fazer o provisionador usar sempre `saas_db`: suas asserções procuram frases e `destino=` na trilha, sem conferir container/banco dos grants em cada modo. (`bin/tests/test_sonda_sem_grant.sh:129`, `bin/tests/test_sonda_sem_grant.sh:133`, `bin/tests/test_sonda_sem_grant.sh:141`)

7. Classes presentes ou parcialmente resolvidas:

- **Derivação paralela:** a pergunta de catálogo foi centralizada corretamente; ambos chamam as mesmas funções. Mas “pronto” operacional continua desigual: o provisionador anuncia pronto após catálogo; o leitor exige ainda arquivo válido e login TCP com `leitor|on`. (`bin/sonda_leitor_comum.sh:33`, `bin/sonda_leitor_comum.sh:69`; `bin/sonda_provisionar.sh:90`; `bin/sonda_leitura.sh:169`, `bin/sonda_leitura.sh:198`)
- **Selo sem mordida:** o comentário remete o mutante a log externo; o próprio selo não o constrói/executa. O novo caso do wrapper demonstra as assertivas contra resposta simulada, sem provar confinamento do payload. Resultados/log citado **[nao verificado]**. (`bin/tests/test_sonda_sem_grant.sh:21`; `bin/tests/test_codex_sonda.sh:40`, `bin/tests/test_codex_sonda.sh:85`)
- **`sql_escrita` não mede todo SQL escrito:** incrementa somente texto bloqueado pelo classificador, antes de enviar. Login usa caminho próprio, payload usa Django e bootstrap é outra chamada. Zero significa nenhuma tentativa classificada nesses pontos, não ausência integral de SQL de escrita. (`bin/sonda_leitura.sh:111`, `bin/sonda_leitura.sh:157`, `bin/sonda_leitura.sh:162`, `bin/sonda_leitura.sh:200`, `bin/sonda_leitura.sh:241`)
- **rc 5 mistura causas:** credencial indisponível, catálogo/readiness, login e tentativa interna de SQL de escrita produzem o mesmo código; `recusa=` conserva texto normalizado, sem classe estruturada. (`bin/sonda_leitura.sh:125`, `bin/sonda_leitura.sh:127`, `bin/sonda_leitura.sh:160`, `bin/sonda_leitura.sh:169`, `bin/sonda_leitura.sh:181`, `bin/sonda_leitura.sh:202`)
- **Recusa silenciosa ao bloquear SQL:** `_sql` chama `_recusa`, que executa `exit 5`; sua invocação na conferência redireciona stderr inteiro a `/dev/null`. Esse `exit` encerra o shell antes do `|| _recusa` externo, escondendo mensagem/porta justamente na tentativa de escrita; execução concreta **[nao verificado]**. (`bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:131`, `bin/sonda_leitura.sh:158`, `bin/sonda_leitura.sh:179`)
- **Ausência de trava global para credencial:** a porta usa o mesmo `$ENVFILE.novo` e reescreve o segredo antes do banco sem flock próprio; duas invocações independentes prod/sombra podem competir. A trava de `sombra.sh` protege aquele fluxo, não a invocação administrativa direta. Corrida concreta **[nao verificado]**. (`bin/sonda_provisionar.sh:61`, `bin/sonda_provisionar.sh:64`, `bin/sonda_provisionar.sh:69`; `bin/sombra_trava.sh:21`, `bin/sombra_trava.sh:28`)

Os selos entram no loop da régua; sua existência não prova que executaram ou passaram. O pre-push documenta que esse loop não roda ali. Resultados **[nao verificado]**. (`bin/regua.sh:138`; `bin/pre-push.sh:13`)

## 09/11 08:23 · sessao 7707844abd3f · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 08:20 · sessao 7e7cab7dc194 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

A cura está escrita: o par identificado aplica as duas metades no mesmo `atomic`, com paridade e lastro julgados após as duas escritas. **[RED nao executado]**; GREEN e mortalidade dos mutantes permanecem **[nao verificado]**. Fonte: [materializacao.py:1302](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/services/materializacao.py:1302).

Por hunk:

| Arquivo:linha | Alteração |
|---|---|
| `chamados/services/materializacao.py:55` | Predicado único `identificar_par_intervalo`: disputa/data, unicidade das metades e mesmo par canônico. Ambiguidade devolve `par_ambiguo`; consulta `chamados/juizes.py:882` e `escala/models.py:1652`. |
| `chamados/services/materializacao.py:91` | Reúne disputas, pais, destinos, clones e perguntas antes da escrita; adquire travas pela porta existente de `chamados/models.py:2637`. |
| `chamados/services/materializacao.py:844` | `par_em_ato` adia a guarda individual e a reconciliação. Escrita continua por `registrar_batida`, em `:987`. |
| `chamados/services/materializacao.py:1220` | Extrai o carimbo compartilhado; `:1264` extrai a conferência de data. Ambos servem ao par e ao caminho avulso. |
| `chamados/services/materializacao.py:1302` | Mantém ambas em voo; grava, julga conjunto, exige lastro, carimba e reconcilia. Falha desfaz o savepoint e devolve as duas validações pelo dono. |
| `chamados/services/materializacao.py:1372` | Despacha par/avulsa, espera e contadores. Respeita o universo selecionado; `:1415` conserva o caminho avulso. |
| `chamados/services/validacao.py:434` | Trava antes de validar; preserva o primeiro aceite e retorna `aguardando_par`. A devolução distingue espera de recusa em `:193`. |
| `chamados/services/materializacao.py:1526` | Varredura processa cada unidade uma vez e conta ambas as perguntas aplicadas. |
| `chamados/services/passivo_validado.py:432` | Pré-trava toda a competência; `:529` entrega ao ato apenas o universo selecionado pelo O330, incluindo carimbos sem lastro. |
| `chamados/catalogo/recusa.py:25` | Espera com código próprio e tradução por igualdade. `COLADA` exige testemunha de mesmo tipo dentro de ±60s; paridade distante perde esse texto. |
| `templates/chamados/partials/modal_fio.html:229` | Exibe a espera nos ramos validado e não validado; segundo ponto em `:243`. |
| `chamados/tests/test_contract_leitores_vitalidade.py:124` | Atualiza o censo para o corpo avulso extraído e a elegibilidade do par. |

Os escritores e leitores dos campos envolvidos ficam assim:

| Campo/função | Quem escreve | Quem lê |
|---|---|---|
| `resposta_colab/respondida_em` | Porta existente, `chamados/services/disputa_emissao.py:1422` | Derivação `materializacao.py:384`; prontidão `:1383`. |
| `validada_em/por` e decisões E1 | Dono existente, `validacao.py:127` e `:163`; sequência em `chamados/models.py:3943` | Dispatcher `materializacao.py:1383`; C6 `chamados/juizes.py:1153`; consulta `chamados/models.py:3359`. |
| `recusa_motivo` | `materializacao.py:1144`; limpeza após aplicação em `:1352` | Devolução/espera `validacao.py:193`; catálogo `recusa.py:92`; apresentação `materializacao.py:479`. |
| Batida: instante, tipo e origem | Porta existente `ponto/registro_batida.py:94` | Guarda `materializacao.py:139`; lastro existente `chamados/juizes.py:645`. |
| `pergunta_origem` | `anotar`, chamado em `materializacao.py:1008`; porta em `ponto/registro_batida.py:235` | Lastro `chamados/juizes.py:703`. |
| Materialização, via e lavra | `materializacao.py:1220` → `chamados/models.py:3103` → `chamados/veredito_lavrado.py:234` | Identidade `materializacao.py:55`; estado `:479`; seleção `:1557`; estado do modelo `chamados/models.py:3035`. |
| `pendencia_admin` | Ramo existente `materializacao.py:910` | Template `templates/chamados/partials/modal_fio.html:221`. |
| Estado de aplicação | `materializacao.py:479`, montado por `chamados/services/fio.py:336` | Template `modal_fio.html:229`; casca da validação `chamados/services/acoes_disputa.py:13`. |
| `processadas/aplicadas/pares` | Dispatcher `materializacao.py:1372` | Varredura `:1568`; O330 `passivo_validado.py:570`; consolidação `:385`; saída JSON existente `chamados/management/commands/materializar_passivo_validado.py:135`. |
| Fechamento | Cauda existente `materializacao.py:1490` → `chamados/models.py:1863` | Autoridade `pode_fechar`, `chamados/models.py:1856`. |

O primeiro aceite permanece validado e aparece com “Falta a outra metade do intervalo”. Para C6, essa primeira pergunta continua morta pelo degrau de validação; a outra metade segura o fechamento nominal. O RED cobra isso, inclusive executando callbacks após o primeiro aceite. Fontes: `validacao.py:482`, `chamados/juizes.py:1153`, `chamados/models.py:1856`, [test_bug_par_intervalo.py:104](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/tests/test_bug_par_intervalo.py:104) e `:164`. Resultado funcional: **[nao verificado]**.

O RED cobre R1–R9, rollback da segunda escrita, lastro, reconciliação, proteção em voo, travas e limites do O330. R7 usa 20/09 porque o índice parcial impede duplicar os motivos na data nominal. Fontes: `chamados/tests/test_bug_par_intervalo.py:35`, `:151`, `:182`, `:255`, `:342`, `:390`, `:401`, `:418`; índice em `chamados/models.py:3482`.

Comando reservado à sessão para RED na base e GREEN na cura: `chamados/tests/test_bug_par_intervalo.py:1`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_intervalo --noinput
```

Cada mutante altera o fonte real **em memória** e executa seu teste-alvo. Os comandos individuais abaixo correspondem às definições de [mutantes_bug_par_intervalo.py:17](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/tests/mutantes_bug_par_intervalo.py:17). Não foram executados; falha de ambiente não demonstra mutante morto. Fonte: `mutantes_bug_par_intervalo.py:1`. [nao verificado]

```bash
mutar() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo "$1"
}
mutar M1  # remove atomic do par
mutar M2  # remove espera
mutar M3  # restaura guarda individual
mutar M4  # remove paridade final
mutar M5  # remove ordem temporal
mutar M6  # remove lastro
mutar M7  # escolhe metades ambiguas
mutar M8  # remove testemunha de colada
mutar M9  # remove par canonico
mutar M10 # remove devolucao das validacoes
mutar M11 # reconcilia entre metades
mutar M12 # conta apenas uma aplicacao
mutar M13 # zera contador
mutar M14 # remove protecao em voo
mutar M15 # remove trava das perguntas
mutar M16 # aplica fora do universo O330
mutar M17 # repara carimbo pela porta normal
mutar M18 # ignora disputas adicionais
```

DECISAO-ABERTA: nenhuma escolha adicional de identidade foi introduzida; ambiguidades seguem pelo caminho avulso, conforme `materializacao.py:55` e `:1400`. Permanecem **[nao verificado]**: execução RED/GREEN, mortalidade dos mutantes, concorrência real, renderização Django, censo populacional e commit introdutor. [nao verificado]

Log final: executei o comando abaixo, que usa `py_compile` sem persistir bytecode; sete arquivos Python compilaram e os 18 mutantes tiveram âncora única, sintaxe e teste-alvo conferidos. Implementação da checagem: `chamados/tests/mutantes_bug_par_intervalo.py:87`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo --sintaxe
```

```text
py_compile: 7 arquivos, rc 0, sem persistencia
M1–M18: ancora unica, sintaxe e teste-alvo conferidos [nao verificado]
RED/GREEN: NAO EXECUTADOS
Mutantes funcionais: NAO EXECUTADOS
```

## 09/11 08:12 · sessao 79c1c84e8d87 · - · - · origem=Ronald (tmux)

Encontrei dois problemas concretos:

- **Ordem de travas no O330:** a transação percorre várias disputas, mas trava cada disputa apenas ao chegar à unidade. Uma segunda disputa fechada pode ser travada depois das perguntas da primeira. Precisa reunir todas as disputas, destinos, clones e perguntas no topo da transação. `chamados/services/passivo_validado.py:371`; `chamados/services/passivo_validado.py:429`; `chamados/services/materializacao.py:90`; `chamados/services/materializacao.py:1372`.
- **Aplicação fora do universo do reparador:** a identidade admite irmã carimbada sem lastro; essa irmã pode ter sido excluída do lote por `plantio_fora_do_slot`. A unidade a aplica mesmo assim, e o contador só contabiliza perguntas do grupo original. Precisa exigir autorização das duas metades pelo universo do lote, recusando o conjunto se faltar uma. `chamados/services/materializacao.py:73`; `chamados/services/materializacao.py:1329`; `chamados/services/passivo_validado.py:330`; `chamados/services/passivo_validado.py:523`; `chamados/services/vigia_carimbo.py:113`.

O segundo problema também amplia o universo da porta normal: uma volta já carimbada sem lastro volta a ser aplicada ao validar a saída, embora a seleção normal exclua perguntas materializadas. O teste R9 atual cobre apenas motivo retratado. `chamados/services/materializacao.py:73`; `chamados/services/materializacao.py:1542`; `chamados/tests/test_bug_par_intervalo.py:310`.

O R8 atual usa perguntas sem carimbo; falta cobrir o formato real do passivo com carimbo falso e a irmã fora do universo. `chamados/tests/test_bug_par_intervalo.py:297`; `chamados/tests/test_o330_plantio_nao_abre_turno.py:101`.

Revisão por leitura; testes não executados. [nao verificado] [nao verificado]

## 09/11 08:04 · sessao 47aa24b5876b · DEPLOY-EXCLUSAO-ENVELOPE · verificador · origem=Code

**Não daria VERDE irrestrito nesta volta.** P2 ainda permite alarme sem compartilhada quando a biblioteca não carrega; o marcador de P1 pode sobreviver ao envelope e provocar falsa recusa; o RELATO continua publicando parâmetros anteriores. (`bin/cron_run.sh:54`, `app/core/esteira_vigia.py:917`, `bin/trava_deploy_exclusao.sh:61`, `app/docs/RELATO.md:25`)

Resultados executados do protocolo, RED/GREEN, m1–m5 e ocorrência dos cenários em produção: **[nao verificado]**. O selo cria, altera e remove arquivos, portanto sua execução não cabe nesta revisão somente leitura. (`bin/tests/test_deploy_exclusao_cron.sh:60`, `bin/tests/test_deploy_exclusao_cron.sh:78`, `bin/tests/test_deploy_exclusao_cron.sh:170`)

1. **P1–P6: cura efetiva e limites**

| Item | Caminho corrigido | O que permanece aberto |
|---|---|---|
| **P1 — parcial** | Envelope toma compartilhada e exporta marcador; deploy com migration chega à aquisição exclusiva, reconhece marcador e retorna 2 sem executar o `flock`; deploy traduz isso em recusa rc 1, nomeando envelope e trava. Remove a espera de 600 s contra o ancestral nesse caminho. (`bin/trava_deploy_exclusao.sh:57`, `bin/trava_deploy_exclusao.sh:58`, `bin/trava_deploy_exclusao.sh:61`, `bin/deploy.sh:328`, `bin/deploy.sh:330`, `bin/deploy.sh:95`) | Marcador não prova posse atual: não valida PID nem compartilhada. Há caminho assíncrono concreto descrito abaixo. A recusa também ocorre somente depois dos portões anteriores, não imediatamente na entrada do script. (`bin/trava_deploy_exclusao.sh:61`, `bin/deploy.sh:308`, `bin/deploy.sh:325`) |
| **P2 — parcial** | Timeout com biblioteca carregada → fecha fd 4 → RC75/placar → readquire compartilhada sem teto → chama alarme → grava FLAG se a chamada funcionar. Assim o alarme espera a exclusiva sair. (`bin/trava_deploy_exclusao.sh:53`, `bin/trava_deploy_exclusao.sh:64`, `bin/cron_run.sh:28`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`) | Biblioteca ausente → função inexistente → `! declare -F` permite o `docker exec` sem compartilhada. Falha na readquisição ou no Docker também não tem retry nesse epílogo; a FLAG não nasce. (`bin/cron_run.sh:17`, `bin/cron_run.sh:54`, `bin/cron_run.sh:59`) |
| **P3 — corrigido no caminho que executa comando** | `ini` e `T0` agora nascem depois da aquisição; espera fica em campo separado. Elimina a inclusão da fila em `fim−ini` daquele caminho. (`bin/cron_run.sh:17`, `bin/cron_run.sh:19`, `bin/cron_run.sh:20`, `bin/cron_run.sh:31`) | SEM VEZ também recebe `ini`, embora nenhum comando comece; `dur` inclui preparação da marca. Os consumidores continuam contando tentativas recusadas, conforme detalhado adiante. (`bin/cron_run.sh:23`, `bin/cron_run.sh:26`, `bin/cron_run.sh:31`, `app/core/management/commands/pipeline_placar.py:74`) |
| **P4 — corrigido para os filhos escolhidos** | Docker de top/HUP, árvore de disponibilidade e Docker da prova recebem fd 4 fechado. Fecha a herança pelo neto da prova contemplado pelo selo. (`bin/deploy.sh:391`, `bin/deploy.sh:395`, `bin/deploy.sh:417`, `bin/deploy.sh:436`) | Não equivale a fechar em todo filho; pipelines auxiliares e `sleep` continuam herdando. Migrate/restart ficam deliberadamente abertos. Morte do deploy durante essas operações não é provada pelo caso f. (`bin/deploy.sh:351`, `bin/deploy.sh:378`, `bin/deploy.sh:401`, `bin/deploy.sh:405`, `bin/tests/test_deploy_exclusao_cron.sh:304`) |
| **P5 — asserção acrescentada; mordida não certificada** | Caso g introduz provas lentas e exige três provas terminadas antes do comando, em normal e restart. (`bin/tests/test_deploy_exclusao_cron.sh:104`, `bin/tests/test_deploy_exclusao_cron.sh:320`, `bin/tests/test_deploy_exclusao_cron.sh:333`) | Não comprova que o cron chegou durante a prova; definições/resultados dos mutantes não foram fornecidos. Mordida observada: **[nao verificado]**. (`bin/tests/test_deploy_exclusao_cron.sh:324`, `bin/tests/test_deploy_exclusao_cron.sh:329`) |
| **P6 — parcial documental** | O contrato v3 anexado descreve 300/600, retomada sem teto, marcador e `espera_deploy`. (`contrato_v3.txt:3`, `contrato_v3.txt:5`, anexo) | RELATO ainda contém cron600/deploy não bloqueante/`espera_lock`/dois contadores; acima há somente resumo de v2. Envio “uma vez” e publicação física do v3: **[nao verificado]**. (`app/docs/RELATO.md:21`, `app/docs/RELATO.md:25`) |

2. **Aninhamento: existe caminho legítimo para marcador obsoleto**

O caminho encontrado é condicional, mas está no código:

- `vigia_arvore` é RAW envelopado; quando passa a vermelho, chama `esteira_vigia.py curar`. (`app/config/crons.py:296`, `app/config/crons.py:1863`, `bin/vigia_arvore.sh:172`, `bin/vigia_arvore.sh:176`)
- **`curar` não é opção reconhecida por `main()`**: cai em `passada()`. O auto-revert possui outra opção, `auto-revert`, e também pode ser acionado pela passada em `cura_sem_efeito`. (`app/core/esteira_vigia.py:1578`, `app/core/esteira_vigia.py:1606`, `app/core/esteira_vigia.py:1549`, `app/core/esteira_vigia.py:1552`)
- A passada pode escolher um relance e chamar `relancar → _lancar`. Se `systemd-run` falhar, `_lancar` usa `Popen(nohup bash esteira.sh, start_new_session=True)`, sem substituir ambiente nem preservar fd 4 explicitamente. O processo destacado pode conservar o marcador depois que o envelope termina. (`app/core/esteira_vigia.py:542`, `app/core/esteira_vigia.py:562`, `app/core/esteira_vigia.py:1516`, `app/core/esteira_vigia.py:936`, `app/core/esteira_vigia.py:913`, `app/core/esteira_vigia.py:917`)
- Existe esteira legítima que chega a deploy: o código cria fatias pelo molde de autocura; seu `esteira.sh` chama `cadeia.sh`, que chama deploy. Se chegar depois da saída do envelope e houver migration pendente, o marcador antigo provoca falsa recusa de aninhamento. (`app/core/esteira_vigia.py:1426`, `app/core/esteira_vigia.py:1429`, `bin/molde_autocura/esteira.sh:23`, `bin/molde_autocura/cadeia.sh:86`, `bin/deploy.sh:325`, `bin/trava_deploy_exclusao.sh:61`)

Falha efetiva do `systemd-run`, duração desse descendente e migration pendente nessa ocasião: **[nao verificado]**.

Nos demais caminhos pedidos:

| Caminho | Resultado da leitura |
|---|---|
| Humano | Deploy não limpa nem valida a procedência do marcador; se recebido, basta estar preenchido para recusar a aquisição exclusiva. Shell interativo filho efetivamente lançado por esses fluxos: **[nao verificado]**. (`bin/trava_deploy_exclusao.sh:61`) |
| `deploy_agendado` | A linha gravada em cron.d não transporta o ambiente do agendador; `rodar` executa esteiras sincronamente. Uma chamada direta de `rodar` tampouco limpa marcador herdado. (`bin/deploy_agendado.sh:55`, `bin/deploy_agendado.sh:56`, `bin/deploy_agendado.sh:73`) |
| Reload 03:30 | É envelopado, mas recusa migration pendente e, no HUP puro, não pede exclusiva. (`app/config/crons.py:326`, `bin/deploy.sh:214`, `bin/deploy.sh:325`) |
| Cadeia/esteira | O caminho destacado acima alcança a cadeia de autocura. A cadeia chama deploy sem limpar o marcador. (`app/core/esteira_vigia.py:917`, `bin/molde_autocura/esteira.sh:23`, `bin/molde_autocura/cadeia.sh:86`) |
| `pos_push` | O script atual atualiza e commita TICKETS; não contém porta direta de deploy. (`bin/pos_push.sh:19`, `bin/pos_push.sh:29`, `bin/pos_push.sh:39`) |

O único escritor funcional localizado é o `export` da biblioteca; o único leitor decisório é o teste de variável não vazia. Deploy também a imprime. `solta_trava_deploy` apenas destrava/fecha fd, sem limpar marcador. (`bin/trava_deploy_exclusao.sh:58`, `bin/trava_deploy_exclusao.sh:61`, `bin/trava_deploy_exclusao.sh:65`, `bin/deploy.sh:331`)

Ainda em P1/F6: os deploys de `auto_revert` usam `capture_output=True` e descartam o retorno. Portanto a recusa emitida pelo deploy não está demonstrada como visível no log desse chamador, apesar do comentário novo. (`app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1269`, `app/core/esteira_vigia.py:1275`, `bin/deploy.sh:321`)

3. **Epílogo: espera infinita possível; ciclo de deadlock não demonstrado**

**Sim, o cron pode ficar indefinidamente no epílogo.** A retomada chama `flock -s` sem `-w`; migrate e prova de rota não têm timeout. A exclusiva só é solta no caminho normal posterior às provas. (`bin/trava_deploy_exclusao.sh:50`, `bin/trava_deploy_exclusao.sh:64`, `bin/deploy.sh:351`, `bin/deploy.sh:433`, `bin/deploy.sh:456`)

Isso demonstra retenção ilimitada, não um ciclo por si só. No trecho sob exclusiva, a consulta de cron do restart **recusa imediatamente**, sem esperar o envelope; os demais passos esperam workers, disponibilidade e rotas. Não encontrei nesse percurso exclusiva → espera cron → espera compartilhada. Bloqueios externos de banco/runtime: **[nao verificado]**. (`bin/deploy.sh:113`, `bin/deploy.sh:115`, `bin/deploy.sh:377`, `bin/deploy.sh:400`, `bin/deploy.sh:417`, `bin/deploy.sh:433`)

O percurso do alarme é `cron_quebrado.handle → avisar_quebra → escrever`: cria Pauta, grava trilha em LogAuditoria e atualiza ContadorPauta. Recuperação atualiza Pauta, trilha e contador. Não encontrei chamada a deploy ou outro `cron_run` nesse percurso de escrita. (`app/core/management/commands/cron_quebrado.py:22`, `app/core/management/commands/cron_quebrado.py:37`, `app/core/services/crons_quebrados.py:71`, `app/pautas/services.py:180`, `app/pautas/services.py:186`, `app/pautas/services.py:188`, `app/pautas/services.py:272`, `app/core/observ.py:133`)

Há também **tempo invisível**: placar, `fim`, `dur` e `espera_deploy` são gravados antes da retomada ilimitada. O envelope pode continuar vivo por tempo arbitrário depois do `fim` registrado. (`bin/cron_run.sh:18`, `bin/cron_run.sh:30`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`, `bin/cron_run.sh:59`)

4. **`ini`, leitores e histórico**

A alteração restaura o sentido aproximado anterior para comandos executados: início depois da fila, ainda antes da preparação da marca. Não restaura literalmente “início do comando” para SEM VEZ, pois nesse ramo não existe comando iniciado. (`bin/cron_run.sh:19`, `bin/cron_run.sh:23`, `bin/cron_run.sh:25`, `bin/cron_run.sh:26`)

| Leitor | Consequência |
|---|---|
| `corrida_no_horario` | Compara o minuto de `ini` com tolerância `<3`. Um disparo legítimo que espere 3–5 minutos pela trava pode sair da medição de duração. (`app/config/crons.py:2304`, `app/config/crons.py:2329`, `app/config/crons.py:2332`, `bin/trava_deploy_exclusao.sh:37`) |
| `crons_duracao` | Filtra data/horário de `ini`, mas aceita `dur` sem filtrar exit. Tentativa SEM VEZ pode entrar como amostra curta. (`bin/crons_duracao.py:69`, `bin/crons_duracao.py:71`, `bin/crons_duracao.py:73`, `bin/crons_duracao.py:75`) |
| `pipeline_placar` | Agrupa pelo dia de `ini`; toda linha incrementa `rodou`, inclusive comando recusado; 75 incrementa erros. Espera atravessando meia-noite pode mudar o dia de atribuição. (`app/core/management/commands/pipeline_placar.py:70`, `app/core/management/commands/pipeline_placar.py:74`, `app/core/management/commands/pipeline_placar.py:78`) |
| `crons_quebrados` | Usa `ini` na janela de 26 horas e na seleção da corrida mais recente; 75 continua erro. (`app/core/services/crons_quebrados.py:36`, `app/core/services/crons_quebrados.py:39`, `app/core/services/crons_quebrados.py:42`, `app/core/services/crons_quebrados.py:46`) |

O escritor apenas apenda; não regrava histórico. Esses leitores não normalizam versões usando `espera_deploy`. Portanto, **se existirem linhas da v1**, continuam interpretadas literalmente com o sentido antigo de chegada, enquanto novas linhas usam o instante posterior à aquisição. Existência e quantidade dessas linhas em produção: **[nao verificado]**. (`bin/cron_run.sh:31`, `bin/crons_duracao.py:69`, `app/core/management/commands/pipeline_placar.py:70`, `app/core/services/crons_quebrados.py:36`)

Na busca por `placar.jsonl`, não localizei outro leitor operacional direto além desses; as referências de `vigiar_fio_morto` e `sombra.sh` são explicativas. (`app/chamados/management/commands/vigiar_fio_morto.py:19`, `bin/sombra.sh:122`)

5. **fd 4: cobertura útil, sem garantia universal**

Os fechamentos cobrem as chamadas Docker apontadas por P4 e toda a árvore iniciada por `timeout` na disponibilidade. Contudo, `awk`, `tr`, `sed`, `tail`, substituições de comando e o `sleep 2` não receberam fechamento próprio. Logo “todo filho nasce sem fd 4” seria falso; não localizei lançamento explicitamente destacado desses auxiliares sob a exclusiva. Sobrevivência efetiva após morte do deploy: **[nao verificado]**. (`bin/deploy.sh:391`, `bin/deploy.sh:401`, `bin/deploy.sh:405`, `bin/deploy.sh:417`, `bin/deploy.sh:436`)

**Migrate e Docker restart conservarem fd é coerente com a intenção declarada:** enquanto esses clientes conservarem o descritor, podem conservar a exclusão após a saída do shell. Isso não prova a promessa mais forte de exclusão enquanto qualquer trabalho interno ao container sobreviver; o selo observa o fd do Docker falso no host. Essa garantia dentro do container: **[nao verificado]**. (`bin/deploy.sh:94`, `bin/deploy.sh:351`, `bin/deploy.sh:378`, `bin/tests/test_deploy_exclusao_cron.sh:92`)

O caso f verifica diretamente fd fechado nos filhos classificados e aberto no migrate. Porém executa deploy com sucesso, chegando ao unlock explícito; não mata o deploy durante migrate/restart. Assim não certifica F1 nem a retenção depois de abandono anormal. (`bin/tests/test_deploy_exclusao_cron.sh:304`, `bin/tests/test_deploy_exclusao_cron.sh:307`, `bin/tests/test_deploy_exclusao_cron.sh:310`, `bin/deploy.sh:456`)

6. **Selo, m1–m5 e casos vazios**

**m1, m2, m3, m4 e m5: [nao verificado] individualmente.** Não foram fornecidas suas definições nem resultados; o selo aceita fontes alternativas, mas isso não identifica qual transformação corresponde a cada nome. (`bin/tests/test_deploy_exclusao_cron.sh:43`, `bin/tests/test_deploy_exclusao_cron.sh:44`, `bin/tests/test_deploy_exclusao_cron.sh:45`)

Há asserções dirigidas às propriedades: b2 cobra alarme depois da última prova e FLAG; e cobra recusa aninhada rápida e identificada; f cobra descritores; g cobra comando depois das três provas. Capacidade observada de cada mutante violá-las: **[nao verificado]**. (`bin/tests/test_deploy_exclusao_cron.sh:217`, `bin/tests/test_deploy_exclusao_cron.sh:293`, `bin/tests/test_deploy_exclusao_cron.sh:310`, `bin/tests/test_deploy_exclusao_cron.sh:333`)

Quanto ao **RED de g**, o delta fornecido não move a soltura: a v1 já a fazia depois das provas. Portanto não há soltura prematura real demonstrada na v1; o novo caso protege essa propriedade contra regressão introduzida. RED executado contra a base sem exclusão ou contra mutante de soltura antecipada: **[nao verificado]**. (`diff_v2_delta.diff`, anexo; `bin/deploy.sh:451`, `bin/deploy.sh:456`)

Há lacunas concretas:

- **d3 pode passar sem comprovar seu cenário:** ignora retorno da espera por `CMD-INI` e do `wait`; basta qualquer falha antecipada do deploy e zero restart. Não exige diagnóstico de recusa por cron nem sucesso do cron. (`bin/tests/test_deploy_exclusao_cron.sh:278`, `bin/tests/test_deploy_exclusao_cron.sh:280`, `bin/tests/test_deploy_exclusao_cron.sh:282`)
- **g exige provas e comando presentes**, mas não sobreposição. Os retornos de espera são ignorados; se o cron só for escalonado depois das provas, passa sem disputar aquela janela. (`bin/tests/test_deploy_exclusao_cron.sh:324`, `bin/tests/test_deploy_exclusao_cron.sh:327`, `bin/tests/test_deploy_exclusao_cron.sh:329`, `bin/tests/test_deploy_exclusao_cron.sh:333`)
- **b1 aceita folga negativa arbitrária:** cobra somente `(fim−ini)−dur <=1`, sem limite inferior nem confronto com timestamp do comando. (`bin/tests/test_deploy_exclusao_cron.sh:144`, `bin/tests/test_deploy_exclusao_cron.sh:197`)
- **d4 verifica desigualdade numérica**, não orçamento completo de deploy. (`bin/tests/test_deploy_exclusao_cron.sh:344`)

7. **Classes anteriores de reprovação**

| Classe | Situação nesta cópia |
|---|---|
| Segunda fonte do caminho | Uma definição funcional localizada. Entretanto, deriva da biblioteca carregada: deploy usa sua própria raiz e envelope usa a viva absoluta. Cópias distintas continuam podendo disputar arquivos distintos enquanto chamam os mesmos containers; não há guarda de raiz viva nesses trechos. (`bin/trava_deploy_exclusao.sh:32`, `bin/deploy.sh:53`, `bin/deploy.sh:326`, `bin/cron_run.sh:17`, `bin/deploy.sh:351`) |
| Derivação paralela | Aquisição, tetos e fd estão centralizados. Já a inferência “marcador preenchido = ancestral ainda segura compartilhada” não é validada e falha no caminho destacado acima. (`bin/trava_deploy_exclusao.sh:44`, `bin/trava_deploy_exclusao.sh:61`, `app/core/esteira_vigia.py:917`) |
| Selo sem mordida | Novas asserções existem; d3 permanece permissivo e g não assegura sobreposição. Mordida de m1–m5: **[nao verificado]**. (`bin/tests/test_deploy_exclusao_cron.sh:282`, `bin/tests/test_deploy_exclusao_cron.sh:333`) |
| Ordem de trava | Cadeia toma árvore, régua e depois exclusiva. A autoespera compartilhada → exclusiva recebe recusa; não demonstrei ciclo inverso sob exclusiva. (`bin/molde_autocura/cadeia.sh:10`, `bin/molde_autocura/cadeia.sh:13`, `bin/molde_autocura/cadeia.sh:86`, `bin/trava_deploy_exclusao.sh:61`) |
| Relógio | Espera e duração usam relógio de parede. Mesmo sem contenção, cruzar um segundo entre os dois `date` pode produzir espera 1; d2 exige exatamente zero. Saltos efetivos de relógio: **[nao verificado]**. (`bin/trava_deploy_exclusao.sh:49`, `bin/trava_deploy_exclusao.sh:51`, `bin/cron_run.sh:20`, `bin/cron_run.sh:30`, `bin/tests/test_deploy_exclusao_cron.sh:268`) |
| ESMERIL — `espera_deploy` | Mede aquisição inicial, exclui retomada do alarme e não identifica causa de 75. Um comando pode esperar, obter vez e devolver 75; falha imediata pode registrar zero. (`bin/cron_run.sh:18`, `bin/cron_run.sh:25`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`, `bin/trava_deploy_exclusao.sh:46`) |
| ESMERIL — `ini`/`dur` | Sucesso recupera início depois da fila; recusa recebe início sem comando; duração inclui preparação e exclui epílogo. O leitor `rodou` continua somando tentativa sem execução. (`bin/cron_run.sh:19`, `bin/cron_run.sh:23`, `bin/cron_run.sh:26`, `bin/cron_run.sh:54`, `app/core/management/commands/pipeline_placar.py:74`) |

## 09/11 08:01 · sessao ac563cc2105e · - · - · origem=Ronald (tmux)

**Há caminho legítimo no código para marcador herdado sobreviver ao envelope, condicionado à falha de `systemd-run`; ocorrência em produção [nao verificado].** O teste usa somente variável não vazia, sem validar PID, arquivo ou posse da compartilhada. (`bin/trava_deploy_exclusao.sh:61`)

Caminho completo:

1. O cron agenda `vigia_arvore.sh`, envelopado pelo gerador de crontab. (`app/config/crons.py:296`, `app/config/crons.py:1863`)
2. O envelope exporta o marcador e chama o comando sincronamente. (`bin/trava_deploy_exclusao.sh:58`, `bin/cron_run.sh:25`)
3. Na transição para vermelho, o script chama **`esteira_vigia.py curar`**. (`bin/vigia_arvore.sh:172`, `bin/vigia_arvore.sh:176`)
4. **`curar` não é opção tratada em `main()`**: a opção de auto-revert se chama `auto-revert`; `curar` cai na chamada geral de `passada()`. (`app/core/esteira_vigia.py:1577`, `app/core/esteira_vigia.py:1578`, `app/core/esteira_vigia.py:1606`)
5. A passada pode selecionar um relance e chamar `relancar()`, que chama `_lancar()`. (`app/core/esteira_vigia.py:542`, `app/core/esteira_vigia.py:562`, `app/core/esteira_vigia.py:1513`, `app/core/esteira_vigia.py:1516`, `app/core/esteira_vigia.py:936`)
6. `_lancar()` tenta `systemd-run`; **se ele falhar**, usa `Popen(['nohup','bash','esteira.sh'], start_new_session=True)`, sem fornecer ambiente substituto nem `pass_fds`. O descendente herda o marcador e pode continuar depois do retorno do Python e da saída do envelope; sua efetiva duração nessa produção [nao verificado]. (`app/core/esteira_vigia.py:913`, `app/core/esteira_vigia.py:915`, `app/core/esteira_vigia.py:917`, `bin/cron_run.sh:59`)
7. Há fatia legítima capaz de chegar a deploy: o próprio código cria a partir de `bin/molde_autocura` e enfileira; seu `esteira.sh` chama `cadeia.sh`, que chama deploy. (`app/core/esteira_vigia.py:1414`, `app/core/esteira_vigia.py:1417`, `app/core/esteira_vigia.py:1424`, `app/core/esteira_vigia.py:1426`, `app/core/esteira_vigia.py:1428`, `bin/molde_autocura/esteira.sh:23`, `bin/molde_autocura/cadeia.sh:86`)
8. Se esse deploy precisar de migration, o marcador antigo o faz recusar como aninhado, mesmo depois do envelope ancestral terminar. Pendência efetiva de migration nesse cenário [nao verificado]. (`bin/deploy.sh:236`, `bin/deploy.sh:325`, `bin/trava_deploy_exclusao.sh:61`, `bin/deploy.sh:329`)

**Limites e distinções:**

- O molde de fatia moderno entrega ao integrador e não chama cadeia/deploy nesse fluxo; o caminho completo acima usa o molde de autocura ainda criado pelo código. (`bin/molde_fatia/esteira.sh:24`, `bin/molde_fatia/esteira.sh:26`, `app/core/esteira_vigia.py:1426`)
- O caminho síncrono para auto-revert existe na passada, no ramo `cura_sem_efeito`; não é correto dizer que a opção literal `curar` chama diretamente `auto_revert()`. (`app/core/esteira_vigia.py:1549`, `app/core/esteira_vigia.py:1552`, `app/core/esteira_vigia.py:1578`, `app/core/esteira_vigia.py:1606`)
- `auto_revert()` chama ambos os deploys com `subprocess.run`; enquanto essa chamada síncrona provém do envelope, o ancestral continua vivo. Os retornos são descartados; `capture_output=True` também captura a recusa sem publicá-la nesse trecho, portanto o comentário “a recusa fala no log dele” não está demonstrado pelo chamador. (`app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1268`, `app/core/esteira_vigia.py:1269`, `app/core/esteira_vigia.py:1275`, `bin/deploy.sh:321`)
- `deploy_agendado` agenda uma nova execução direta em `/etc/cron.d` e roda as esteiras sincronamente. Agendar dentro de um envelope não grava esse marcador na linha gerada; invocar diretamente `rodar` em ambiente já contaminado tampouco o limpa. (`bin/deploy_agendado.sh:55`, `bin/deploy_agendado.sh:56`, `bin/deploy_agendado.sh:73`)
- Reload 03:30 é envelopado, mas recusa migration pendente e evita aquisição exclusiva quando faz somente HUP. (`app/config/crons.py:326`, `app/config/crons.py:1863`, `bin/deploy.sh:214`, `bin/deploy.sh:325`)
- `pos_push.sh` atual escreve/commita TICKETS e termina; não é porta direta de deploy. (`bin/pos_push.sh:18`, `bin/pos_push.sh:19`, `bin/pos_push.sh:20`, `bin/pos_push.sh:29`, `bin/pos_push.sh:39`)
- Shell interativo destacado com marcador antigo é possível em tese, mas não identifiquei lançamento desse tipo nos caminhos lidos: **[nao verificado]**.
- O observador de árvore é assíncrono, fecha fds 6–9 e conserva 4 se herdado, mas apenas espera o pai e grava evento; não chama deploy. (`bin/trava.sh:67`, `bin/trava.sh:68`, `bin/trava.sh:69`)
- O fabricante também lança esteira destacada, porém caminho de cron até ele não foi demonstrado: **[nao verificado]**. (`bin/fabricante.sh:187`)

O único escritor funcional localizado do marcador é o export da biblioteca; o único leitor decisório é seu teste de não vazio; deploy apenas o imprime. Não existe limpeza no `solta_trava_deploy()`, que somente destrava/fecha o fd. (`bin/trava_deploy_exclusao.sh:58`, `bin/trava_deploy_exclusao.sh:61`, `bin/trava_deploy_exclusao.sh:65`, `bin/deploy.sh:331`, `bin/deploy.sh:332`)

## 09/11 08:01 · sessao 76b19fbb493b · - · - · origem=Ronald (tmux)

P2 está curado no caminho normal de timeout com biblioteca carregada: aquisição compartilhada falha, fd4 fecha, comando não roda, RC75 entra no placar e o epílogo readquire compartilhada sem teto antes do `docker exec cron_quebrado`; FLAG nasce somente com retorno zero. (`bin/trava_deploy_exclusao.sh:50`, `bin/trava_deploy_exclusao.sh:53`, `bin/trava_deploy_exclusao.sh:64`, `bin/cron_run.sh:17`, `bin/cron_run.sh:28`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`)

**P2 não está fechado para biblioteca ausente/ilegível:** esse ramo também é oficialmente SEM VEZ/75; porém a condição `! declare -F trava_alarme_compartilhada` libera o `docker exec` sem readquirir qualquer compartilhada. Ocorrência em produção: [nao verificado]. (`bin/cron_run.sh:10`, `bin/cron_run.sh:16`, `bin/cron_run.sh:17`, `bin/cron_run.sh:28`, `bin/cron_run.sh:54`)

**Pode esperar para sempre:** a readquisição é `flock -s` sem `-w`; o deploy pode conservar exclusiva dentro do migrate ou de uma prova sem timeout. O cron fica vivo no epílogo, sem FIM e sem MARCA no ramo SEM VEZ. Isso é retenção ilimitada demonstrável, não prova de ciclo de deadlock. (`bin/trava_deploy_exclusao.sh:50`, `bin/trava_deploy_exclusao.sh:64`, `bin/deploy.sh:351`, `bin/deploy.sh:433`, `bin/deploy.sh:456`, `bin/cron_run.sh:21`, `bin/cron_run.sh:40`, `bin/cron_run.sh:54`, `bin/cron_run.sh:59`)

No trecho com exclusiva, não encontrei deploy esperando um cron/envelope: o segundo `recusa_restart_com_cron` consulta marcas e falha imediatamente; troca de workers e provas esperam workers/HTTP, não carimbos da corrente. Não há ciclo concreto deploy exclusiva → espera cron → espera compartilhada nesses caminhos lidos. Bloqueios externos de banco/runtime: [nao verificado]. (`bin/deploy.sh:113`, `bin/deploy.sh:115`, `bin/deploy.sh:377`, `bin/deploy.sh:400`, `bin/deploy.sh:417`, `bin/deploy.sh:433`)

O escritor do alarme segue `cron_quebrado.handle → avisar_quebra → pautas.services.escrever`: consulta User/Empresa/Pauta, cria Pauta, escreve LogAuditoria e atualiza ContadorPauta. Recuperação atualiza Pauta, trilha e contador. Não encontrei chamada a deploy/cron_run nesse percurso nem receptor de sinais para Pauta/ContadorPauta/LogAuditoria; os modelos lidos não sobrescrevem `save`. Registro dinâmico externo: [nao verificado]. (`app/core/management/commands/cron_quebrado.py:22`, `app/core/management/commands/cron_quebrado.py:35`, `app/core/management/commands/cron_quebrado.py:37`, `app/core/services/crons_quebrados.py:63`, `app/core/services/crons_quebrados.py:66`, `app/core/services/crons_quebrados.py:71`, `app/core/services/crons_quebrados.py:80`, `app/pautas/services.py:180`, `app/pautas/services.py:186`, `app/pautas/services.py:188`, `app/pautas/services.py:272`, `app/pautas/services.py:344`, `app/pautas/services.py:369`, `app/core/observ.py:133`, `app/pautas/models.py:67`, `app/pautas/models.py:143`)

P3 restaura `ini` após aquisição no caminho que executa CMD; conserva assinatura legível pelos leitores existentes. Literalmente, `ini` antecede mkdir/MARCA e `dur` inclui essa preparação. No SEM VEZ não existe início de comando: `ini` representa o instante após aquisição recusada; `dur` mede o curto ramo de recusa. Logo “ini = início do COMANDO” ainda não descreve todas as linhas. (`bin/cron_run.sh:17`, `bin/cron_run.sh:19`, `bin/cron_run.sh:20`, `bin/cron_run.sh:23`, `bin/cron_run.sh:25`, `bin/cron_run.sh:26`, `bin/cron_run.sh:31`)

| Leitor real | Efeito |
|---|---|
| `crons_quebrados` | Usa `ini` para janela de 26h e corrida mais recente; 75 continua erro. Mudança de ordenação possível quando há linhas concorrentes e semânticas históricas distintas. (`app/core/services/crons_quebrados.py:36`, `app/core/services/crons_quebrados.py:39`, `app/core/services/crons_quebrados.py:42`, `app/core/services/crons_quebrados.py:46`) |
| `pipeline_placar` | Usa data de `ini`; cada linha soma `rodou`, inclusive SEM VEZ; 75 soma erros; `dur` entra no máximo. Espera cruzando meia-noite desloca a tentativa ao dia do fim da aquisição. (`app/core/management/commands/pipeline_placar.py:70`, `app/core/management/commands/pipeline_placar.py:74`, `app/core/management/commands/pipeline_placar.py:78`, `app/core/management/commands/pipeline_placar.py:80`) |
| `crons_duracao` | Usa data e filtro de horário de `ini`; inclui qualquer `dur` inteiro, sem filtrar exit. SEM VEZ pode contribuir amostra curta se estiver no horário. (`bin/crons_duracao.py:69`, `bin/crons_duracao.py:71`, `bin/crons_duracao.py:73`, `bin/crons_duracao.py:75`) |
| `corrida_no_horario` | Tolerância padrão de 3 minutos, comparação `<3`; comando atrasado pela trava em 3–5 minutos pode sair da medição mesmo sendo disparo agendado legítimo. (`app/config/crons.py:2304`, `app/config/crons.py:2329`, `app/config/crons.py:2332`, `bin/trava_deploy_exclusao.sh:37`) |

Não achei outro leitor operacional direto de JSONL pelo grep de `placar.jsonl`; `vigiar_fio_morto` apenas comenta o arquivo, e `sombra.sh` referencia `pipeline_placar`. Não há consumo operacional de `espera_deploy` nesses leitores. (`app/chamados/management/commands/vigiar_fio_morto.py:19`, `bin/sombra.sh:122`, `bin/crons_duracao.py:69`, `app/core/management/commands/pipeline_placar.py:70`, `app/core/services/crons_quebrados.py:36`)

Não há migração/regravação histórica no escritor: ele só apenda JSON. Linhas antigas continuam interpretadas pelo seu `ini` literal; leitores não usam versão nem `espera_deploy` para normalizar registros v1/v2. Existência de registros v1 em produção: [nao verificado]. (`bin/cron_run.sh:31`, `app/core/services/crons_quebrados.py:36`, `app/core/management/commands/pipeline_placar.py:70`, `bin/crons_duracao.py:69`)

**Métrica invisível do P2:** `ESPERA` é capturada e placar gravado antes da readquisição ilimitada. Portanto o tempo de epílogo não aparece em `espera_deploy`, `dur` ou `fim`, embora o envelope continue vivo. (`bin/cron_run.sh:18`, `bin/cron_run.sh:30`, `bin/cron_run.sh:31`, `bin/cron_run.sh:54`, `bin/cron_run.sh:59`)

**F4 não se resolve por `espera_deploy>0`:** sucesso de aquisição também pode esperar e CMD retornar 75; falha imediata de aquisição pode registrar espera zero; ajuste de relógio pode alterar o valor. É duração de aquisição, não causa. (`bin/trava_deploy_exclusao.sh:46`, `bin/trava_deploy_exclusao.sh:49`, `bin/trava_deploy_exclusao.sh:50`, `bin/trava_deploy_exclusao.sh:51`, `bin/cron_run.sh:25`, `bin/cron_run.sh:31`)

P6 continua divergente no RELATO real: linha 25 ainda publica cron600/deploy não bloqueante/espera_lock/dois contadores/desliga só corte; linha21 fala contrato v2. Não há atualização para v3 nesse trecho. Envio único ao Codex não pode ser comprovado pelo código. (`app/docs/RELATO.md:21`, `app/docs/RELATO.md:25`; envio: [nao verificado])

_3078 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
