# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 20:18** · publicado em **17/11/2026 20:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 20:18 · sessao 623b984d2c9f · PORTA-DO-EXPORT-SEM-BECO · verificador · origem=Code

**PROCEDE: NÃO.** O selo ainda permite que o ESTADO publique um número diferente do devolvido pelo leitor único: ele compara a saída do ESTADO com a frase produzida pelo mesmo medidor, e só testa a frase da porta do export com o valor sintético `2`. [bin/tests/test_recusa_estado.sh:81](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:81), [bin/tests/test_recusa_estado.sh:93](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:93)

1. **BLOQUEIA 1 continua aberto.** Um mutante concreto é inserir, em `linha_porta_export`, depois do retorno para `sem_arquivo` e antes do retorno que formata a frase: [bin/recusa_estado.py:71](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:71)

   ```python
   if d['recusas'] != 2:
       d = dict(d, recusas=d['recusas'] + 1)
   ```

   Para qualquer contagem diferente de `2`, a frase passa a mostrar uma unidade a mais que `porta_export_falhou_hoje` devolveu. A AST nova examina os retornos de `ler` e `ler_porta_export` e proíbe certos nomes, mas não verifica essa alteração em `linha_porta_export`; o caso sintético com `2` permanece igual e a comparação do ESTADO usa a mesma função alterada dos dois lados. [bin/recusa_estado.py:68](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:68), [bin/tests/test_recusa_estado.sh:37](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:37), [bin/tests/test_recusa_estado.sh:83](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:83), [bin/tests/test_recusa_estado.sh:90](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:90)

   A passagem é uma **conclusão estática**, condicionada às demais etapas do selo passarem e às duas leituras da raiz permanecerem coerentes; não executei o mutante nem o selo, pois o próprio selo cria e remove arquivos temporários. Resultado executado: **[não verificado]**. [bin/tests/test_recusa_estado.sh:59](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:59)

2. **BLOQUEIA 2: há registro de vermelho, mas não prova completa das mutações.** O anexo fornecido registra, para M1–M5, nome, arquivo, `diff stat`, mensagem e `rc=1`; isso informa o motivo *reportado* pelo selo para cada execução. Porém, o `diff stat` não mostra as linhas alteradas: não comprova que cada mutação corresponde ao nome atribuído, nem permite avaliar se houve outra alteração com o mesmo tamanho. A autenticidade e a reprodução do log são **[não verificado]**. [anexo `instr_v5_mutantes.log`, fornecido na pergunta: não verificado]

3. **Quem escreve e quem lê os valores:**

   | Valor ou sinal | Quem escreve ou produz | Quem lê |
   |---|---|---|
   | Linhas sintéticas do teste | O próprio selo cria o `ponto.log` temporário e escreve as portas e datas de teste. [selo:70](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:70) | `ler` e `ler_porta_export`; o selo verifica ausência, contagens, esperado e frase. [selo:68](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:68), [selo:81](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:81) |
   | Recusas do Resolver dia | Os envelopes chamam `registrar`, que emite a linha no logger; a configuração SaaS encaminha `ponto` ao arquivo. [respostas.py:54](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/respostas.py:54), [respostas.py:78](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/respostas.py:78), [recusa_critica.py:70](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:70), [saas.py:160](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/config/settings/saas.py:160), [saas.py:181](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/config/settings/saas.py:181) | `contar` lê o log e produz `recusas` e `por_usuario`; `resolver_dia_recusado` acrescenta o rótulo; o medidor e o ESTADO consomem o resultado. [recusa_critica.py:95](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:95), [recusa_critica.py:108](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:108), [recusa_critica.py:126](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:126), [gerar_estado.py:265](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:265) |
   | Falhas ou divergências da porta do export | `_recusa` chama o mesmo `registrar` com a porta do export. [porta_export.py:1063](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/folha/porta_export.py:1063) | `porta_export_falhou_hoje` conta as linhas e define `esperado = 0`; o medidor formata contagem, esperado e vermelho, e o ESTADO usa a frase. [recusa_critica.py:133](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:133), [recusa_estado.py:60](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:60), [gerar_estado.py:274](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:274) |
   | `sem_arquivo`, data e frases do ESTADO | Os dois métodos do medidor produzem `sem_arquivo` quando falta o arquivo; o gerador fornece uma data e escreve o ESTADO. [recusa_estado.py:37](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:37), [recusa_estado.py:60](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:60), [gerar_estado.py:270](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:270), [gerar_estado.py:399](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:399) | O selo lê os retornos sintéticos e compara as duas frases do medidor com linhas do ESTADO gerado. [selo:83](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:83), [selo:100](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:100) |

4. **FILAs 4–7:** permanecem como riscos já apontados: duas leituras em momentos distintos; erro do gerador ocultado; igualdade dos fusos sustentada no código por comentário; e AST que procura substrings sem impedir reatribuição de `_hoje_recusa`. [selo:90](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:90), [selo:93](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:93), [gerar_estado.py:267](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:267), [selo:30](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:30)

   A mudança não demonstra, por si, que qualquer uma dessas quatro filas tenha virado um **novo** bloqueio. A falha de integridade demonstrada acima mantém aberto o BLOQUEIA 1; a checagem de `: 2**` reduz o caso da antiga FILA 3 apenas para esse exemplo sintético. [selo:83](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:83)

## 17/11 20:18 · sessao 7e1d803ea754 · E4C-F1-B2 · verificador · origem=Code

A tabela ainda precisa de correção: há **7 linhas procedentes entre os 26 sítios pedidos**, além de 6 itens comuns ou seções procedentes. O descarte de B4 resolve a colisão física, mas deixa duas contradições; os descartes de B9a e B9b não demonstram compatibilidade com o aval posterior.

Os caminhos de código abaixo são relativos a `app/chamados/`, salvo indicação diferente. Os avais são os anexos transcritos na solicitação, identificados por item.

| ID | Revisão |
|---|---|
| Cabeçalho | **PROCEDE** — O hash integral da cópia não pôde ser confirmado por Git: a cópia não contém repositório Git [não verificado]. Além disso, o cabeçalho declara zero indecisos enquanto deixa P21 no topo; o aval já manda usar a mesma leitura nos comandos que escrevem por item (anexo 18:45, item 2; `management/commands/reabrir_orfas_enterradas.py:85`, `:157`). |
| F0 | **CONFERE** — A guarda retorna fora da cerca; há quatro chamadas abridoras no código de produção (`services/fio.py:1396-1398`; `reconciliador.py:553`, `:1701`; `services/becos.py:66`; `services/disputa_emissao.py:1909`). |
| F1 | **CONFERE** — Cerca aninhada executa no contexto externo, sem abrir savepoint próprio (`services/fio.py:1433-1434`). |
| F2 | **CONFERE** — O abridor testa a anotação de veto dentro do atomic e devolve a recusa após o rollback (`services/fio.py:1439-1446`). |
| F3 | **CONFERE** — A guarda antecede o save de pergunta existente e o update em lote (`models.py:3574-3577`, `:3400-3407`). |
| F4 | **PROCEDE** — `evento` protege sua própria gravação, mas a consulta da última retenção ocorre antes dele e fora desse tratamento; falha nessa leitura pode subir, contrariando a garantia proposta de que falha da trilha não derruba o ato (`reconciliador.py:489-493`; `app/core/observ.py:94-105`, `:133-136`; anexo 18:45, item 6). |
| F5 | **CONFERE** — O juiz existente retorna `None` para dia indecidível e usa memo; o resumo e a lista são exigências do aval, ainda não uma implementação demonstrada (`services/fio.py:1358-1368`; anexo 17:20, R3). |
| F6 | **CONFERE** — A extração pode preservar o universo se mantiver exatamente dia do turno, dia **gravado**, exclusão de conversa e o `continue` para conjunto vazio; o juiz e a agregação permanecem separados (`services/fio.py:1399-1422`). |
| F7 | **CONFERE** — A regra da validação existe no caminho comum; o retorno antecipado da folga fica como achado (`services/validacao.py:711-712`, `:740-742`; anexo 18:45, item 2(i)). |
| F8 | **PROCEDE** — A lista descrita como deduplicada por `(pk, motivo)` não especifica como preservar **duas entidades de trilha** para o mesmo pk exigidas por B2a/B2b; as chamadas são sequenciais e `ch_novo` aparece depois (`reconciliador.py:977-1001`, `:1664-1666`). O coletor proposto ainda não existe; `_ESPERAS_163` é somente exemplo de contexto (`services/materializacao.py:1726-1734`). |
| F9 | **CONFERE** — O ponto proposto fica depois dos dois retornos sem escrita e antes da lavra e do save (`models.py:3241-3242`, `:3264-3273`). |
| INDECISOS | **PROCEDE** — A9d está decidido, mas P21 não deve permanecer em “manter ou trocar”: a regra posterior inclui comando de sistema e exige a mesma leitura de dias (`management/commands/reabrir_orfas_enterradas.py:85`, `:94-99`, `:157`; `services/fio.py:1403-1413`; anexo 18:45, item 2(ii)). |
| ACHADOS | **CONFERE** — As seis portas ficam como lista sem obra e sem cerca por decisão expressa; a folga passa antes da regra comum (`services/validacao.py:711-712`, `:740-742`; anexo 19:50, item 3). |
| RED-NOVO-1 | **CONFERE** — O gate chama o reaponte e hoje levanta `ValueError` quando ele recusa; o caso adicional precisa distinguir recusa por veto (`models.py:435-439`, `:689`). |
| RED-NOVO-2 | **CONFERE** — O controle deve chegar a `carimbar` e sair sem escrita para testar a posição do julgamento (`services/materializacao.py:2161-2167`; `models.py:3241-3242`, `:3264-3266`). |
| RED-NOVO-3 | **PROCEDE** — O controle especificado como **não ENTREGUE** não demonstra que `julga_antes_de_carimbar` contaria indevidamente uma saída sem escrita: para morder esse mutante, o controle precisa ser vetável e sair por C6 ou fora do fato (`models.py:3241-3242`, `:3264-3266`; `services/fio.py:1418-1419`). |
| DECISÕES | **PROCEDE** — A decisão B4 repete o descarte apesar das contradições de alvo e de dia; P21 também não deve ficar pendente da escolha manter/trocar (`services/fio.py:1403-1411`, `:969-976`; `management/commands/reabrir_orfas_enterradas.py:85`, `:157`; anexo 18:45, item 2(ii)). |
| A1 | **CONFERE** — O save de justificativa e as escritas de pergunta ficam no ato cercado; o resolver aninhado não abre outra unidade (`reconciliador.py:543-580`; `services/fio.py:1433-1446`). |
| A2 | **CONFERE** — Resolver e fecho ficam sob o abridor do beco; o veto aninhado pertence ao abridor externo (`services/becos.py:42-72`; `services/fio.py:1433-1446`). |
| A3 | **CONFERE** — Direto, resolver abre cerca; aninhado, executa na existente (`services/disputa_emissao.py:1902-1914`; `services/fio.py:1433-1434`). |
| A4 | **CONFERE** — A transição alcança o gate de reata e a relavra; fora de cerca o tratamento deve ser por item (`models.py:435-439`, `:642-656`, `:689`, `:307-312`; anexo 18:45, itens 2-3). |
| A5 | **CONFERE** — O laço da relavra chama a escrita por pergunta; dentro da cerca prevalece o veto do abridor (`models.py:307-312`; `services/fio.py:1439-1446`; anexo 18:45, item 3). |
| A6 | **CONFERE** — Encerrar passa pelo gate e pela gravação do estado; os pontos de pergunta são os identificados na tabela (`models.py:1686-1693`, `:435-439`, `:307-312`). |
| A7 | **CONFERE** — O caminho da tranca passa pelo gate e pela relavra, sem abridor próprio (`models.py:1711`, `:307-312`; `services/competencia_trancada.py:207-209`; anexo 18:45, item 3). |
| A8 | **CONFERE** — Filtrar depois do universo e antes de `alvo` também exclui a vetada das decisões, células e disputas derivadas (`models.py:1601-1609`, `:1621`, `:1638-1649`). |
| A9a | **CONFERE** — O fecho por desligamento continua sem cerca; a cadeia S144 posterior é ato de sistema por item (`models.py:2009-2012`; `signals.py:164-167`; anexo 18:45, itens 1-4). |
| A9c | **CONFERE** — Os `continue` do S144 e as duas saídas de `carimbar` antecedem F9 (`services/materializacao.py:2154-2167`; `models.py:3241-3242`, `:3264-3273`). |
| A9d | **CONFERE** — O aval posterior decide disputa fechada e pergunta vetada sem gravação; F9 permite retorno sem exceção (`services/competencia_trancada.py:222-229`; `signals.py:164-167`; `models.py:3264-3273`; anexo 19:50, item 2). |
| B1a | **CONFERE** — `False` sai pelo `continue`, fora de `n`; a trilha sem chamado deve ter a pergunta como alvo (`reconciliador.py:628-645`; anexo 17:50, item 1). |
| B1b | **CONFERE** — O fecho posterior pode disparar S144; seu julgamento fica em F9 (`reconciliador.py:652-660`; `signals.py:164-167`; `models.py:3264-3273`). |
| B2a | **PROCEDE** — A saída sem fecho confere, mas a especificação de duas trilhas para um pk não fecha com o coletor F8 descrito apenas como lista deduplicada (`reconciliador.py:400-415`, `:1664-1666`, `:977-1001`). |
| B2b | **PROCEDE** — `ch_novo` é o chamado da costura e a recusa retorna `False`, mas falta preservar esse segundo alvo sem duplicar a contagem já feita em B2a (`reconciliador.py:977-1001`, `:1664-1666`). |
| B2c | **CONFERE** — Costura e resolver estão no mesmo abridor HIST; a recusa devolve `False` (`reconciliador.py:1690-1705`; `services/fio.py:1433-1446`). |
| B2d | **PROCEDE** — `(False, mot)` é o retorno de `reapontar_resolvedora`; o chamador de B2d o consome e devolve apenas `False`, não o par afirmado na tabela (`reconciliador.py:1712-1716`). |
| B3a | **CONFERE** — `encerrar_fantasma` delega ao carimbo; `False` pula o evento (`models.py:3421-3429`; `reconciliador.py:1191-1193`). |
| B3b | **CONFERE** — O fecho alcança S144 e, nele, F9 depois das saídas sem escrita (`reconciliador.py:1216-1218`; `services/materializacao.py:2154-2167`; `models.py:3264-3273`). |
| B4 | **PROCEDE** — O risco de índice ao datar a principal com perdedora viva está documentado, mas a célula manda trilha ora ao chamado destino, ora sempre à pergunta. Também afirma que a porta lê `data_evento` da instância: ela lê o **gravado** e o dia do turno (`services/fio.py:939-947`, `:969-976`, `:1403-1411`; `reconciliador.py:489-493`). |
| B5 | **CONFERE** — `lavrar` grava por item; o backfill citado usa o escritor em lote, não essa chamada (`veredito_lavrado.py:240-250`, `:254-275`; `management/commands/backfill_veredito_lavrado.py:143-146`). |
| B6 | **CONFERE** — O `bulk_update` usa a lista recebida e não passa pelo `save` de cada pergunta (`veredito_lavrado.py:240-251`; `models.py:3565-3577`). |
| B7 | **CONFERE** — O ponto antes do save fica depois dos dois `continue` do laço (`services/relavra_via.py:117-127`). |
| B8 | **PROCEDE** — O pré-veto existente usa dia gravado e `novo_dia` e devolve recusa antes da porta; a tabela não demonstra a mesma leitura F6 nem o contrato por item que conta e deixa os demais seguirem, inclusive sob cerca (`services/fio.py:1105-1123`, `:1403-1419`; `models.py:3072-3076`; anexo 18:45, item 2(ii)). |
| B9a | **PROCEDE** — O código confirma o aval anterior por grupo, mas o aval posterior inclui backfill e determina julgamento por item; o descarte não demonstra que a regra antiga prevalece nessa colisão (`management/commands/backfill_decisoes_pergunta.py:244-262`; anexo 18:45, item 2(ii)). |
| B9b | **PROCEDE** — Grupo na ida e reversão tudo ou nada existem, mas sua prevalência sobre o comando posterior por item não foi resolvida pelo descarte (`management/commands/migrar_data_evento_dia_do_turno.py:380-393`, `:476-495`; anexo 18:45, item 2(ii)). |

**Quem escreve e quem lê:** `recusa_motivo` é escrito, entre outros pontos, pelo materializador e pela recusa humana, e lido pelo próprio materializador e pela validação (`services/materializacao.py:1676-1690`; `services/validacao.py:262-266`, `:916-922`). Os carimbos `materializada_em`, `materializada_por` e `via_resolucao` são atribuídos pela porta de carimbo e lidos pelo C6 (`models.py:3218-3222`, `:3260-3273`; `juizes.py:1389-1405`). `evento` cria `LogAuditoria`; `registrar_retencao` lê a última linha para deduplicar (`app/core/observ.py:94-105`, `:133-136`; `reconciliador.py:489-493`). `_CERCA` é escrito e restaurado pelo abridor e lido pela guarda (`services/fio.py:1381`, `:1396`, `:1433-1436`, `:1449`). O coletor F8 é proposto, portanto seus escritores e leitores ainda não existem na cópia; `_ESPERAS_163` apenas demonstra a técnica de `ContextVar` (`services/materializacao.py:1726-1734`).

A extração F6 deve preservar a consulta ao **dia gravado**, inclusive quando B4 já atribuiu outro dia em memória; tratar essa atribuição como leitura existente mudaria o predicado. A ativação exclusiva do coletor por sistema e sua ausência em portas humanas são requisitos do desenho, ainda não comportamento implementado (`services/fio.py:917-928`, `:1403-1413`; `services/materializacao.py:1726-1734`).

**PROCEDE: 13** — 7 dos sítios e 6 itens comuns/seções. **LINHAS RESPONDIDAS: 26 de 26.**

## 17/11 20:17 · sessao 015814b2b862 · - · - · origem=Ronald (tmux)

- **B2d — PROCEDE:** `reapontar_resolvedora` devolve `(False, motivo)`, mas `reatar_pergunta_orfa` trata o par e devolve apenas `False` ao seu chamador ([reconciliador.py:1712](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:1712)).

- **B2a/B2b e F8 — PROCEDE:** a tabela descreve um coletor de `(pk, motivo)` deduplicado por pk, mas exige duas trilhas com alvos diferentes para a mesma pergunta. As chamadas são sequenciais, e o chamado novo só aparece no segundo caminho; o desenho precisa preservar os dois alvos sem duplicar a contagem ([reconciliador.py:1664](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:1664), [reconciliador.py:998](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:998)).

- **P21/INDECISOS — PROCEDE:** o aval transcrito exige a mesma leitura de dias para comando de sistema e nenhum segundo leitor; a tabela deixa indeciso se mantém o leitor próprio de P21 ([reabrir_orfas_enterradas.py:85](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:85), [fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403)).

- **A9d/F9:** as duas saídas sem escrita precedem a lavra e o save, como descrito; S144 chama `carimbar` depois dos dois `continue` ([models.py:3241](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3241), [models.py:3264](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3264), [materializacao.py:2154](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/materializacao.py:2154)).

## 17/11 20:17 · sessao 3ae0314e0af1 · E4C-F1-B2 · verificador · origem=Code

| id | Revisão |
|---|---|
| P1 | **PROCEDE** — O ramo de dia diferente mantém `ReusoSemRechave`, que desfaz o ato, enquanto o aval vigente determina recusa por item e continuação dos demais. O ramo sem `upd` não escreve. `models.py:3060`, `models.py:3072`, `models.py:3117`. |
| P3 | **CONFERE** — O julgamento precede a decisão e o carimbo; a retirada do motivo e a tentativa de fecho continuam. `models.py:2145`, `models.py:2147`, `models.py:2163`. |
| P4 | **CONFERE** — Os alvos já excluem as vias de saída sem escrita da porta; o segundo laço decide o fecho. `encerrar_perguntas_com_fato.py:31`, `encerrar_perguntas_com_fato.py:65`, `models.py:3415`. |
| P5 | **CONFERE** — A seleção já restringe à via da competência; a recusa por item precede o descarimbo. `competencia_trancada.py:276`, `models.py:3459`. |
| P6 | **CONFERE** — A validação comum tem regra própria; o retorno antecipado da folga permanece no achado X4. `validacao.py:711`, `validacao.py:740`. |
| P7 | **CONFERE** — A devolução está após a segunda verificação de competência da validação humana. `validacao.py:777`, `validacao.py:781`. |
| P11 | **CONFERE** — Há escrita com `_pend` mesmo se `ts` for `None`; sem `_pend`, essa saída não conta por P11. `materializacao.py:1347`, `materializacao.py:1353`. |
| P12 | **CONFERE** — O ponto é o carimbo de identidade efetivo do ramo; a chamada anterior de recusa tem julgamento próprio em P13. `materializacao.py:1450`, `materializacao.py:1461`. |
| P13 | **CONFERE** — Julgar apenas com `_carimba or _muda_motivo` evita contar a chamada que não escreve nem anota. `materializacao.py:1676`, `materializacao.py:1680`, `materializacao.py:1691`. |
| P14 | **CONFERE** — O `save` ocorre somente no ramo `_muda_motivo`; a chamada isolada não conta. `materializacao.py:1688`, `materializacao.py:1690`. |
| P15 | **PROCEDE** — A saída C6/fora do fato não conta **por P15**, mas a tabela afirma que não conta no ato: a reprovação acrescenta erro e o `finally` pode tentar outra escrita em P13; se essa escrita for vetada, ela conta. `models.py:3241`, `models.py:3264`, `materializacao.py:1968`, `materializacao.py:1976`, `materializacao.py:1990`. |
| P17 | **CONFERE** — A linha fora do fato permanece na captura para acordar célula, mas é excluída da decisão e do update; não conta como veto da escrita de pergunta. `disputa_emissao.py:1625`, `disputa_emissao.py:1645`, `disputa_emissao.py:1648`, `models.py:3405`. |
| P18 | **CONFERE** — Resposta automática é ato de sistema; texto presente sai sem escrita. A validação comum barra, mas a folga retorna antes da guarda. `disputa_emissao.py:1663`, `disputa_emissao.py:1677`, `validacao.py:711`, `validacao.py:740`. |
| P19 | **CONFERE** — O ponto proposto vem após o retorno sem escrita e antes da atribuição e do save. `disputa_emissao.py:1692`, `disputa_emissao.py:1696`, `disputa_emissao.py:1698`. |
| P20 | **CONFERE** — O laço relê, passa pelos `continue` e só então tenta descarimbar; as vias de identidade ficam fora da seleção. `relavra_via.py:181`, `relavra_via.py:236`, `relavra_via.py:242`, `relavra_via.py:246`. |
| P21 | **PROCEDE** — O comando usa dia gravado em leitor próprio; a porta usa turno e dia gravado. A tabela corretamente mantém a decisão sobre o leitor existente no topo. `reabrir_orfas_enterradas.py:86`, `reabrir_orfas_enterradas.py:94`, `fio.py:1403`, `fio.py:1409`. |
| X1 | **CONFERE** — Os `continue` e as duas saídas sem escrita de `carimbar` precedem F9; no caminho da tranca, a disputa fecha e a pergunta vetada conta. `materializacao.py:2161`, `materializacao.py:2167`, `models.py:3241`, `models.py:3264`, `competencia_trancada.py:229`. |
| X2 | **CONFERE** — O K6 existente não substitui a leitura comum; o ponto proposto fica após `campos` vazio e antes do save. `validacao.py:338`, `validacao.py:346`, `validacao.py:351`, `fio.py:1409`. |
| X3 | **CONFERE** — O julgamento após o impedimento e antes do primeiro `registrar` evita decisão sem o update correspondente. `validacao.py:432`, `validacao.py:439`, `validacao.py:440`. |
| X5 | **CONFERE** — O laço é de sistema e pode seguir por item; o laço posterior de chamados permanece. `veredito_celula.py:189`, `veredito_celula.py:202`. |
| X6 | **CONFERE** — A saída idempotente precede o carimbo. O descarte B9a tem fundamento no aval por grupo registrado no código. `fio.py:1483`, `fio.py:1492`, `backfill_decisoes_pergunta.py:245`. |
| X7 | **CONFERE** — O reaponte já trata `False` como recusa e segue; as perdedoras passam pela porta própria. `fio.py:2328`, `fio.py:2329`, `fio.py:2334`. |
| X8 | **CONFERE** — Fora de cerca, deixar a vetada sem reata e permitir o fechamento decorre da classe (ii); não há colisão identificada nos avais dados. Outras recusas conservam o `ValueError`. `models.py:410`, `models.py:435`, `models.py:438`. |
| X9 | **CONFERE** — O laço registra o resultado de cada reaponte e já pula a pergunta apontada ao pai. `limbo_folgas.py:239`, `limbo_folgas.py:242`, `limbo_folgas.py:245`. |
| X10 | **CONFERE** — A vetada permanece **sem reata**; se o furo não fecha e outra reatou, o rollback desfaz as reatas e o gate é tentado novamente. A trilha proposta precisa ser drenada fora do savepoint. `limbo_folgas.py:278`, `limbo_folgas.py:283`, `limbo_folgas.py:285`, `limbo_folgas.py:289`. |
| X12 | **CONFERE** — A saída de recusa do reaponte serve ao julgamento por item; o descarte B4 tem fundamento na colisão e no índice, preservando os demais itens do laço. `reatar_beco_folga_ao_limbo.py:135`, `fio.py:939`, `fio.py:944`. |
| X13 | **CONFERE** — O fecho aciona S144 e herda o ponto F9 de X1, após as saídas sem escrita. `materializacao.py:2030`, `signals.py:164`, `models.py:3264`. |
| X14 | **CONFERE** — A emissão chama o fecho, cuja cadeia S144 herda X1. `disputa_emissao.py:1298`, `signals.py:164`, `materializacao.py:2167`. |
| X15 | **CONFERE** — A ordem proposta libera a cerca antes do callback; o descarte B9b está apoiado no grupo da ida e na reversão integral já declarados no código. `fio.py:1438`, `fio.py:1449`, `migrar_data_evento_dia_do_turno.py:382`, `migrar_data_evento_dia_do_turno.py:482`. |

**PROCEDE: 3, LINHAS RESPONDIDAS: 29 de 29.**

## 17/11 20:17 · sessao a06993d1296e · CURA-DE-FASE · censo · origem=Code

O censo ficou **incompleto**: confirmei a cadeia da virada e alguns outros sítios, mas não rastreei a origem de todas as datas nem validei todos os candidatos da varredura. Portanto, os totais pedidos não estão verificados.

| Chamador | Origem da data e veredito |
|---|---|
| [censo_fase_12x36.py:413](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:413) → `escala_vigente` | **TEXTO POSSÍVEL.** `linhas` recebe `virada` por `str(virada)` em [censo_fase_12x36.py:224](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:224); o filtro forma `_auto` em [censo_fase_12x36.py:257](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:257), entregue a `_curar` em [censo_fase_12x36.py:268](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:268). |
| [servico_jornada.py:66](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/servico_jornada.py:66) → `vinculo_do_dia` | **TEXTO POSSÍVEL.** Repassa o parâmetro `data` recebido em [servico_jornada.py:15](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/servico_jornada.py:15), inclusive pela cadeia do censo acima. |
| [alimentacao.py:285](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:285) → `vinculo_cobre_a_data` | **TEXTO POSSÍVEL.** Repassa o parâmetro de [alimentacao.py:228](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:228), alcançado pela cadeia anterior; a comparação com `data_inicio` está em [alimentacao.py:214](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:214). |
| [aplicar_col900.py:176](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:176) → `vinculo_do_dia` | **DATA GARANTIDA.** `dias` é construído com `date.fromisoformat` em [aplicar_col900.py:159](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:159) e percorrido em [aplicar_col900.py:172](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:172). |
| [aplicar_col900.py:229](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:229) → `vinculo_do_dia` | **DATA GARANTIDA.** O valor de `it['dia']` passa por `date.fromisoformat` em [aplicar_col900.py:226](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:226). |
| [diff_col900.py:69](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/diff_col900.py:69) → `vinculo_do_dia` | **DATA GARANTIDA.** `it['dia']` passa por `date.fromisoformat` em [diff_col900.py:66](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/diff_col900.py:66). |
| [alimentacao.py:196](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/calculador/alimentacao.py:196) → `vinculo_do_dia` | **DUVIDOSO.** `_dia` vem dos valores de `resultado.dia_da_jornada` em [alimentacao.py:193](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/calculador/alimentacao.py:193). Falta rastrear quem preenche esse mapa, inclusive no caminho da lavra. [não verificado] |
| [fechamento.py:191](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/fechamento.py:191) → `vinculo_do_dia` | **DUVIDOSO.** `_d` vem da chave do mapa `celulas` em [fechamento.py:189](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/fechamento.py:189). Falta rastrear a construção do mapa em todos os chamadores. [não verificado] |

**TEXTO POSSÍVEL: [não verificado]** — há três sítios confirmados na tabela, contando separadamente o repasse entre funções.

**DUVIDOSO: [não verificado]** — há pelo menos os dois sítios apresentados; faltam os demais candidatos.

A varredura abaixo retornou **119 candidatos fora de testes e 54 em testes**. Ela reconhece imports locais, aliases e chamadas por atributo, mas é uma busca de candidatos: não resolve escopos, sombreamento ou proveniência entre funções. Assim, esses números **não são a contagem final de chamadores nem a contagem validada de testes**. [não verificado] [nao verificado]

```python
import ast
import pathlib
import collections

names = {
    "escala_vigente",
    "vinculo_do_dia",
    "vinculo_cobre_a_data",
    "q_vinculo_cobre_a_data",
}
counts = collections.Counter()

for p in sorted(pathlib.Path("app").rglob("*.py")):
    try:
        t = ast.parse(p.read_bytes())
    except (SyntaxError, ValueError):
        continue

    aliases = {n: n for n in names}
    for x in ast.walk(t):
        if (
            isinstance(x, ast.ImportFrom)
            and (x.module or "").endswith(("servico_jornada", "alimentacao"))
        ):
            for a in x.names:
                if a.name in names:
                    aliases[a.asname or a.name] = a.name

    parent = {
        child: node
        for node in ast.walk(t)
        for child in ast.iter_child_nodes(node)
    }

    for x in ast.walk(t):
        if not isinstance(x, ast.Call):
            continue

        f = x.func
        target = None
        if isinstance(f, ast.Name) and f.id in aliases:
            target = aliases[f.id]
        elif isinstance(f, ast.Attribute) and f.attr in names:
            target = f.attr
        if not target:
            continue

        q = parent.get(x)
        while q and not isinstance(q, (ast.FunctionDef, ast.AsyncFunctionDef)):
            q = parent.get(q)

        test = (
            any(part in ("tests", "test") for part in p.parts)
            or p.name.startswith("test_")
        )
        counts["test" if test else "main"] += 1

        if not test:
            function = q.name if q else "<module>"
            print(f"{p}:{x.lineno} {function} {target} {ast.unparse(x)}")

print("COUNTS", dict(counts))
```

## 17/11 20:17 · sessao 9ce9c6b0f615 · - · - · origem=Ronald (tmux)

Pontos enviados ao agente principal:

- F6/B4: a porta lê `data_evento` gravado, não o valor novo em memória; a proposta de B4 precisa preservar essa leitura literal ([fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403), [fio.py:916](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:916)).
- F4: a gravação por `evento` é protegida, mas a consulta anterior de deduplicação não está dentro desse tratamento ([reconciliador.py:489](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:489), [observ.py:105](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/core/observ.py:105)).
- F9: as duas saídas sem escrita precedem a lavra e o save ([models.py:3241](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3241), [models.py:3264](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3264)).
- O coletor é proposto; `_ESPERAS_163` é uma referência de forma, já existente ([materializacao.py:1728](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/materializacao.py:1728)).

## 17/11 20:17 · sessao 8f35ea13b72c · - · - · origem=Ronald (tmux)

- **B4 — PROCEDE:** a célula atribui a trilha ao chamado de destino, quando houver, e também manda usar sempre `PerguntaDisputa`; as duas instruções divergem. O código atual registra a retenção no chamado ([reconciliador.py:489](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:489)). A célula também diz que a guarda lê `data_evento` da instância, mas ela lê o dia do turno da instância e o dia gravado ([fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403)). O risco de colisão ao datar a principal com perdedora viva está documentado em [fio.py:939](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:939).

- **B9a — PROCEDE:** o código confirma o aval anterior por grupo ([backfill_decisoes_pergunta.py:245](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/backfill_decisoes_pergunta.py:245)), mas o aval transcrito das 18:45 inclui backfill e determina julgamento por item, com os demais seguindo. A prevalência do aval anterior, afirmada no descarte, não está demonstrada pelo dado fornecido [não verificado].

- **B9b — PROCEDE:** o código confirma a ida por grupo e a reversão tudo ou nada ([migrar_data_evento_dia_do_turno.py:380](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:380), [migrar_data_evento_dia_do_turno.py:482](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:482)). A compatibilidade dessas unidades com o comando posterior de julgamento por item não está resolvida no dado fornecido [não verificado].

## 17/11 20:17 · sessao 48db4edd951f · PORTA-DO-EXPORT-SEM-BECO · construtor · origem=Code

A implementação e os testes Ra–Rg estão escritos na cópia, mas a entrega **ainda não está certificada**: não executei os testes nem os mutantes, pois eles usam banco; seus resultados e a conformidade completa com o aval permanecem **[não verificado]**. A análise sintática dos módulos de produção e do novo arquivo de testes passou, assim como `git diff --check` **[verificado localmente]**. [nao verificado]

Arquivos alterados:

- [porta_export.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1302): predicado de tentativa morta sem escrita, leitura do estado, espera opcional e textos. `conferir` conserva a lista técnica no estado divergente ([porta_export.py:811](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:811)).
- [selo_leitores_no_mesmo_numero.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:145): espera e encerramento pelo CAS para `--tentativa`.
- [views.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/views.py:315), [urls.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/urls.py:18) e [_estado_da_porta.html](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/templates/folha/_estado_da_porta.html:1): rota, inclusão no preview e bloco com recarga em conferência.
- [test_porta_estado_na_tela.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_estado_na_tela.py:33): casos Ra–Rg. A capacidade de cada caso matar seu mutante permanece **[não verificado]**.
- [test_porta_sem_beco.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_sem_beco.py:107) e [test_porta_sem_beco_item2.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_sem_beco_item2.py:226): inversões autorizadas para leitura sem escrita e persistência pelo vigia.
- [test_selo_leitores_no_mesmo_numero.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:135): mock aceita argumentos nomeados.
- [test_porta_do_export.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_do_export.py:53): atualização das três asserções de texto autorizadas.

As três trocas foram `NAO FOI MEDIDA HOJE` → `ainda nao foram conferidos hoje`, preservando as mensagens de falha:

- TXT sem medição: [linha 53](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_do_export.py:53).
- Carimbo de outra competência: [linha 529](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_do_export.py:529).
- Carimbo de ontem: [linha 536](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_do_export.py:536).

A busca pelos três textos antigos nos testes encontrou outras ocorrências em comentários, não em asserções ([test_integracao.py:158](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_integracao.py:158), [test_sm1_monster_export.py:122](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_sm1_monster_export.py:122), [test_cauda_trilha_do_cartao.py:107](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/relatorios/tests/test_cauda_trilha_do_cartao.py:107)). [nao verificado]

Para execução posterior em ambiente de testes autorizado, o comando dos RED é: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test folha.tests.test_porta_estado_na_tela
```

Os comandos abaixo aplicam os mutantes **em memória** e executam os casos correspondentes; não foram executados **[não verificado]**.

Mutante de leitura que chama `vigiar`, esperado falhar em Rb e Rc:

```bash
PYTHONDONTWRITEBYTECODE=1 python - <<'PY'
import os
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings.saas')
import django
django.setup()
from folha import porta_export as pe
from django.core.management import execute_from_command_line
original = pe.estado_da_porta
def mutante(empresa, mes, ano):
    pe.vigiar(empresa, mes, ano, pe._linha_do_pedido(empresa, mes, ano))
    return original(empresa, mes, ano)
pe.estado_da_porta = mutante
execute_from_command_line(['manage.py', 'test',
    'folha.tests.test_porta_estado_na_tela.EstadoNaTelaTest.test_Rb_curso_morto_nao_grava',
    'folha.tests.test_porta_estado_na_tela.EstadoNaTelaTest.test_Rc_pedido_vencido_nao_grava'])
PY
```

Mutante de trava sem espera, esperado falhar em Rd:

```bash
PYTHONDONTWRITEBYTECODE=1 python - <<'PY'
import os
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings.saas')
import django
django.setup()
from folha import porta_export as pe
from django.core.management import execute_from_command_line
original = pe.trava_da_medicao
def mutante(empresa, mes, ano, espera_ate=None):
    return original(empresa, mes, ano)
pe.trava_da_medicao = mutante
execute_from_command_line(['manage.py', 'test',
    'folha.tests.test_porta_estado_na_tela.EsperaDaTentativaTest.test_Rd_espera_ate_liberar_e_mede'])
PY
```

Mapa dos campos no caminho alterado:

| Campo | Escreve | Lê |
|---|---|---|
| `detalhe['estado']` | `pedir`, `_cunhar` e `_mudar_estado` ([porta_export.py:1092](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1092), [porta_export.py:1130](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1130), [porta_export.py:1035](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1035)) | `tentativa_morta` e `estado_da_porta` ([porta_export.py:1302](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1302), [porta_export.py:1345](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1345)) |
| `pedido_em` | `pedir` ([porta_export.py:1103](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1103)) | `tentativa_morta` e comando ([porta_export.py:1313](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1313), [comando:153](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153)) |
| `concluido_em` | `_mudar_estado`, pelo nome do estado ([porta_export.py:1063](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1063)) | `_campos_do_texto` ([porta_export.py:1392](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1392)) |
| `n` | `resultado_de` fornece; `concluir` grava ([porta_export.py:1224](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1224), [porta_export.py:1265](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1265)) | `_campos_do_texto` ([porta_export.py:1393](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1393)) |
| `backend_pid` | `_cunhar` e `assumir` ([porta_export.py:1147](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1147), [porta_export.py:1213](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1213)) | `tentativa_morta` e `medicao_viva` ([porta_export.py:1307](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1307), [porta_export.py:1003](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1003)) |
| Carimbo | `lavrar`, chamado por `concluir` ([porta_export.py:768](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:768), [porta_export.py:1262](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1262)) | `estado_da_porta` e `conferir` ([porta_export.py:1362](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1362), [porta_export.py:825](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:825)) |

LEI-AKITA: origem=folha/porta_export.py; testemunha=estado_da_porta; RED=Ra–Rg **[escritos, execução não verificada]**; quem-mais-le=censo do item 0 **[conferência completa não verificada]**; juizes novos=0 **[não verificado]**; padrao=job assincrono com estado visivel + escritor unico **[não verificado]**.

## 17/11 20:17 · sessao 742aae255a9a · ESCALA-NAO-DESCREVE · censo · origem=Code

**O censo não está fechado para liberar o construtor:** há quatro sítios adicionais que dependem do formato agrupado de `lista()` e precisam ser preservados ou adaptados; há também um segundo escritor da mesma chave na rotina de sombra. O número de sítios adicionais que **efetivamente mudarão de comportamento** depende da implementação e está **[não verificado]**. Os quatro são impactos estáticos identificados, não quatro mudanças de comportamento comprovadas. ([cadastro_realidade.py:1044](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1044), [vigia_fio.py:194](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:194), [fechar_avisos_regua_na_lista.py:28](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/management/commands/fechar_avisos_regua_na_lista.py:28), [fechar_chamados_vinculo_na_lista.py:36](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:36), [sombra.sh:157](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/bin/sombra.sh:157))

A leitura foi feita na cópia fornecida, sem criar, editar ou apagar arquivos. Rodei `python3` com `ast` sobre os arquivos Python de `app/`, procurando chamadas e imports, inclusive aliases, imports locais e relativos; complementei com buscas textuais no repositório. A associação estática de nomes não prova chamadas dinâmicas nem resolução completa de escopo. O hash efetivo da cópia é **[não verificado]**: o diretório não apresenta metadados Git acessíveis. [nao verificado]

**A — Escrita e armazenamento**

- `anomalias_da_frota()` constrói o retrato; `lavrar()` chama `gravar_lavra(d)`. O comando `esmeril_espelho --apply` chama `lavrar()`, e o cron o agenda. ([esmeril_espelho.py:206](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:206), [esmeril_espelho.py:296](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:296), [comando esmeril_espelho.py:16](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/esmeril_espelho.py:16), [crons.py:640](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:640))
- A autoridade é `inteligencia.MetricaSnapshot.detalhe`, JSON identificado por `chave='esmeril_espelho'`, `escopo='frota'`, `escopo_id=None` e `data_ref=hoje`; `ler_lavra()` escolhe a `data_ref` mais recente. `lavrar()` também escreve o espelho JSON no caminho configurado, por padrão `logs/esmeril_espelho.json`. ([esmeril_espelho.py:242](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:242), [esmeril_espelho.py:303](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:303), [esmeril_espelho.py:317](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:317), [esmeril_espelho.py:334](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:334), [models.py:81](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/inteligencia/models.py:81))
- **Outro escritor da mesma chave:** `bin/sombra.sh` copia o `detalhe` de produção e faz `update_or_create` diretamente na sombra, além de gravar outra chave de marcador. Portanto, `gravar_lavra` é o escritor do fluxo normal encontrado, mas não o único código que escreve essa chave em todos os ambientes. ([sombra.sh:136](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/bin/sombra.sh:136))

**B — Leitores**

| Leitor | O que lê |
|---|---|
| Cadastro x Realidade | `lista()` lê `por_colab` e `destino`; o template exibe a frase, e `pdf_bytes()` recebe a saída de `lista()` e a imprime. ([cadastro_realidade.py:872](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:872), [template:29](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/templates/escala/cadastro_x_realidade.html:29), [cadastro_realidade.py:1083](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1083)) |
| Pautas do esmeril | `sincronizar()` lê a lavra; `alvo_da_lavra()` usa `por_colab`. A Pauta semanal também chama `linhas_para_a_pauta()`, que usa `por_assinatura`. ([pautas_do_esmeril.py:27](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/pautas_do_esmeril.py:27), [pautas_do_esmeril.py:52](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/pautas_do_esmeril.py:52), [esmeril_espelho.py:356](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:356), [fase_12x36.py:241](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/fase_12x36.py:241)) |
| Copiloto/mensageria | Sem termo, lê a lavra e devolve somente os campos de resumo e `por_assinatura`; com termo, calcula `anomalias_do_colab()` na hora. Essa API não devolve `destino` nem `por_colab` nesse caminho. ([views_mensageria.py:1786](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/api/views_mensageria.py:1786)) |
| Placar | Chama o comando de leitura e extrai o contador, sem ler diretamente os dois mapas. ([placar_code.sh:389](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/bin/placar_code.sh:389), [comando esmeril_espelho.py:17](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/esmeril_espelho.py:17)) |
| Validação de chamados | `sem_marco_por_colab()` lê `por_colab` para rotular o cadastro na fila. ([validacao.py:1129](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/validacao.py:1129), [views_cobrar.py:367](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/views_cobrar.py:367)) |
| Escala automática | `dry_da_frota()` lê `por_colab` e passa as assinaturas às propostas do vínculo. ([escala_auto.py:163](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/escala_auto.py:163)) |
| Sombra | Lê diretamente o snapshot para copiá-lo; a guarda de sombra compara o hash do detalhe com o marcador. ([sombra.sh:138](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/bin/sombra.sh:138), [esmeril_espelho.py:266](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:266)) |

Sobre `horas_folga_trabalhada`: a referência em `config/crons.py` aparece em **:1061**, não em :1072, e descreve uma definição histórica. Na cópia atual, `escala_certa` chama `assinaturas_do_colab(..., so_escala=True)` para a competência do dia, sem ler a lavra; o cálculo usa essa decisão para distribuir as horas. Assim, alterar a função compartilhada de assinaturas exige preservar esse caminho, embora acrescentar campos apenas ao retrato não torne a folha leitora deles. ([crons.py:1058](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:1058), [escala_certa.py:19](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/escala_certa.py:19), [regras.py:664](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/calculador/regras.py:664))

**C — Chamadores**

- `realocar_vinculo_para_destino`: a ação C7 da lista e o adaptador dos chamados antigos `realocar_para_destino`; há chamadas de teste. A porta determina hoje, recusa destinos cíclicos e chama `executar_vinculo(request, ...)`. ([views.py:463](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:463), [regua_defesa.py:291](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:291), [regua_defesa.py:326](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:326), [test_termometro_camada3.py:100](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/tests/test_termometro_camada3.py:100))
- `propor_escala`: esmeril, `proposta_pdf.coletar`, `escala_auto` e comando `propor_escala_limbo`, além de testes. No esmeril atual, só é chamado quando a pessoa tem alguma assinatura; a confiança está fora do dicionário `proposta` retornado. ([esmeril_espelho.py:213](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:213), [proposta_pdf.py:45](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/proposta_pdf.py:45), [escala_auto.py:119](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/escala_auto.py:119), [propor_escala_limbo.py:37](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/management/commands/propor_escala_limbo.py:37), [detector_proposta.py:153](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/detector_proposta.py:153))

**D — 6x1 e template vigente**

O campo é `ec.tipo_escala.tipo_ciclo`, comparado a `TipoEscala.CICLO_6x1`. O Plano de Folgas chama `ec.defeito_folga_nao_definida()` para seu indicador; esse método contém a comparação. O teste de 6x1 deve olhar o ciclo, e não usar o resultado inteiro do método como sinônimo de 6x1: ele também verifica folga fixa e `FolgaDia`. ([views_plano_folgas.py:83](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views_plano_folgas.py:83), [models.py:1241](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:1241), [models.py:30](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:30))

Para o vínculo vigente hoje, `_vinculos()` filtra com `q_vinculo_cobre_a_data(hoje)`, resolve com `vinculo_do_dia()` e publica `ec.tipo_escala_id` como `template_id`; será necessário também disponibilizar o ciclo para a decisão da linha. O Plano de Folgas atualmente filtra seus vínculos por `ativa=True`, diferença que merece teste no clique. ([cadastro_realidade.py:748](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:748), [views_plano_folgas.py:55](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views_plano_folgas.py:55))

**E — Dependências da lista, PDF, template e rotas**

A view chama `lista()` uma vez, envia o resultado ao HTML ou a `pdf_bytes()`. Hoje o contrato devolve `postos`, com pessoas ordenadas por nome dentro de cada posto; HTML e PDF iteram esse agrupamento. A C7 usa POST, `_is_gestor` e destino de outra lavra, a do termômetro. ([views.py:440](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:440), [views.py:475](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:475), [cadastro_realidade.py:930](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:930), [template:21](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/templates/escala/cadastro_x_realidade.html:21), [cadastro_realidade.py:1083](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1083), [urls.py:14](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/urls.py:14))

Os quatro sítios adicionais identificados que leem `postos` são:

1. `dias_na_lista()`, usado pelo vigia do fio para reconhecer dias e donos. ([cadastro_realidade.py:1030](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1030), [vigia_fio.py:195](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:195))
2. O próprio vigia do fio, que também percorre `postos` diretamente para obter pessoas. ([vigia_fio.py:196](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:196))
3. O comando que identifica avisos C7 para eventual fechamento. ([fechar_avisos_regua_na_lista.py:28](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/management/commands/fechar_avisos_regua_na_lista.py:28))
4. O comando equivalente para chamados C8. ([fechar_chamados_vinculo_na_lista.py:36](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:36))

Remover `postos` sem adaptação faria esses leitores perderem as linhas ou falharem; manter compatibilidade pode evitar mudança funcional, mas sua compatibilidade com a exigência literal de “uma lista só” depende da solução concreta **[não verificado]**. ([cadastro_realidade.py:1044](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1044), [vigia_fio.py:196](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:196))

Há outra premissa a fechar: a janela do esmeril é calculada **por pessoa**, considerando empresa, competências entregues e existência de células. Um único “desde DD/MM” para o total não está necessariamente definido pelo código atual; a data comum aplicável ao dado de produção é **[não verificado]**. ([esmeril_espelho.py:38](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:38), [esmeril_espelho.py:219](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:219))

**F — Arquivos estimados para a obra**

- [esmeril_espelho.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:79): contagem no laço existente, publicação dos mapas e preservação dos demais chamadores de assinaturas.
- [cadastro_realidade.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:717): dados e ações da lista, adaptação de `dias_na_lista` e PDF; o PDF fica neste mesmo arquivo.
- [views.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:440), [urls.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/urls.py:14) e [cadastro_x_realidade.html](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/templates/escala/cadastro_x_realidade.html:21): rota, autorização e apresentação.
- Potencialmente os três arquivos externos que percorrem `postos`: [vigia_fio.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:194), [fechar_avisos_regua_na_lista.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/management/commands/fechar_avisos_regua_na_lista.py:27) e [fechar_chamados_vinculo_na_lista.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:31); a necessidade de editá-los depende do contrato escolhido **[não verificado]**.
- Testes existentes, incluindo [test_cadastro_x_realidade.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/tests/test_cadastro_x_realidade.py:42), [test_termometro_camada3.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/tests/test_termometro_camada3.py:86) e [test_o261_vigia_fio.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/tests/test_o261_vigia_fio.py:651), mais casos necessários para R1–R7 **[estimativa; não verificado até a implementação]**.

O cruzamento de arquivos com as raias B2, F2 P2, c4 e PORTA v6, seu estado atual e eventuais colisões são **[não verificado]**: o dado fornecido identifica as raias, mas não fornece suas listas atuais de arquivos. A medição contra 1267, as quantidades fora da lista e o estado da lavra de produção também são **[não verificado]**.

## 17/11 20:16 · sessao 96041dfececd · CURA-DE-FASE · verificador · origem=Code

**1 — Premissas: CONFERE, com ressalva sobre a escrita.** O censo monta `virada` e `proposta_ancora` como texto; `_curar` passa `x['virada']` a `escala_vigente`, que encaminha a data a `vinculo_do_dia`; no caminho sem geradora, a comparação pode chegar a `date <= str`. Há também comparação com data no caminho com geradora. ([censo_fase_12x36.py:224](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:224), [censo_fase_12x36.py:413](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:413), [servico_jornada.py:63](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/servico_jornada.py:63), [alimentacao.py:214](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:214), [alimentacao.py:282](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:282))

As comparações de `NO_OP` usam texto; manter texto no `post` **fecha o contrato dessa porta**: `julgar_vinculo` converte tanto `data_inicio` quanto `data_ancora` com `datetime.date.fromisoformat`. A execução de vínculo ocorre depois da chamada que falhou, mas a reversão em arquivo é escrita **antes** dela. O anexo relata zero vínculos mudados nessa corrida; a confirmação independente em produção é **[não verificado]**. ([censo_fase_12x36.py:397](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:397), [censo_fase_12x36.py:414](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:414), [vinculo.py:52](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:52))

**2 — Colisão com aval vigente: não demonstrada [não verificado].** O corte registrado reserva a rodada de produção ao Ronald, restrita a `--mes 11 --ano 2026`; o teste em banco de teste não é essa rodada. Há teto documentado de dois construtores, e o anexo concede exceção nominal a esta cura; a ocupação atual e o cruzamento real de arquivos com raias em voo são **[não verificado]**. A abrangência do item 2 ainda depende do censo: não é possível garantir agora que todos os sítios eventualmente encontrados caibam no arquivo nomeado ou fora de zonas invioláveis. ([CORTES.md:537](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:537), [CLAUDE.md:141](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:141), [CLAUDE.md:291](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:291))

**3 — R1: CONFERE como teste construível; resultado [não verificado].** `datetime.date.fromisoformat`, já usada na porta, converte a data ISO sem alterar `x` nem o `post`. Há fixtures de teste com empresa, posto, usuário, vínculo e batidas sintéticas; uma linha no formato do censo pode ser montada sem copiar dado de produção. O teste precisa demonstrar a escrita e a saída `desde`, já que a porta pode recusar e `_curar` captura a exceção. Nenhum RED foi executado nesta revisão. ([vinculo.py:54](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:54), [test_fase_12x36.py:34](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/tests/test_fase_12x36.py:34), [censo_fase_12x36.py:421](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:421))

**3 — R2: PROCEDE como ressalva à premissa de alcance automático do `NO_OP`.** Repetir literalmente a mesma lista em `_curar` pode testar o ramo pedido, mas só se, após a primeira escrita, a juíza devolver para a virada um vínculo com **ambos** início e âncora iguais aos textos da linha. A célula e sua geradora influenciam essa resposta; converter a data, por si só, não prova a condição. Numa nova execução do comando, a lista é recalculada e filtrada antes de `_curar`: a linha pode deixar de ser automática e nem chegar ao ramo. A reversão JSON é fotografia de vínculos e células, não a fonte da lista `auto`. Portanto, R2 deve fixar e verificar o estado que faz a mesma linha chegar ao `NO_OP`; seu resultado concreto é **[não verificado]**. ([censo_fase_12x36.py:254](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:254), [censo_fase_12x36.py:380](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:380), [censo_fase_12x36.py:413](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:413), [alimentacao.py:269](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:269))

**3 — R3: [não verificado].** A classe e o RED por sítio estão definidos no anexo, mas o censo AST pedido ainda não foi feito; não há base para afirmar quantidade, arquivos atingidos ou ausência de colisão. A chamada de origem está confirmada em [censo_fase_12x36.py:413](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:413).

**4 — Exportada, dinheiro e `!`: efeito fora do declarado não demonstrado [não verificado].** A porta de vínculo pode regenerar células e acionar reconciliação; a porta de célula declara guarda para exportada e pode acionar recálculo. Logo, não cabe tratar uma futura cura em produção como sem possível efeito sobre dinheiro; o alcance real dos dados e de outros sítios do item 2 é **[não verificado]**. O corte reserva a execução produtiva ao Ronald. ([vinculo.py:315](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:315), [celula.py:112](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/portas/celula.py:112), [celula.py:460](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/portas/celula.py:460), [CORTES.md:537](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:537))

**MEDIR: [não verificado].** Esta cópia não disponibiliza o histórico Git nem o log de produção; não foi possível identificar o commit de origem nem corridas posteriores por `git log -S` e `logs/censo_fase_12x36.log`. O anexo fornece o resultado da corrida das 20:11, mas não o histórico pedido. O hash da cópia também não pôde ser confirmado por Git **[não verificado]**.

**PROCEDE: 1 — R2**, limitado à premissa de que converter a data garante, por si só, que a segunda passagem alcance `NO_OP`. A existência de um teste com a mesma linha e estado adequado continua possível; seu resultado permanece **[não verificado]**. [nao verificado]

## 17/11 20:16 · sessao 4be1158694af · ESCALA-NAO-DESCREVE · verificador · origem=Code

**AVAL-DEVOLVIDO — PROCEDE nos itens 0, 2 e 4 e no RED R4.** Os achados são: leitor de dinheiro atribuído incorretamente à lavra; início único da janela sem definição; ausência de `!` literal para o toque em vínculo; e R4 exigir sempre um vínculo novo quando a porta também pode atualizar um existente. ([esmeril_espelho.py:351](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:351), [esmeril_espelho.py:38](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:38), [CLAUDE.md:265](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:265), [vinculo.py:100](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:100))

A cópia foi lida nos caminhos abaixo; o hash HEAD informado não pôde ser conferido porque a cópia aberta não contém metadados Git. [nao verificado]

### Item 0 — PROCEDE

- **Premissa do leitor: PROCEDE.** `config/crons.py:1072` não é leitor da lavra: inicia texto descritivo de outra ferramenta; a referência a `horas_folga_trabalhada` está em comentário anterior. A própria implementação de `ler_lavra` declara que dinheiro deixou de ler a lavra; portanto o censo não deve registrar o leitor alegado sem prova por AST. ([crons.py:1060](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:1060), [crons.py:1072](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:1072), [esmeril_espelho.py:351](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:351))
- **Escritor e chamadas: CONFERE.** `gravar_lavra` grava o snapshot; há chamadas à porta de realocação pela C7 e pelo caminho de chamado antigo, além de testes. O censo pedido por AST ainda deve distinguir produção e testes. ([esmeril_espelho.py:317](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:317), [views.py:463](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:463), [regua_defesa.py:329](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:329))
- **Colisão e condição de abertura: CONFERE como regra; resultado pendente.** Cruzar arquivos e parar em arquivo comum é compatível com a regra de raias; o cruzamento real com as raias citadas não foi fornecido. A regra de um Codex construtor por vez é afirmada no anexo fornecido, mas sua fonte integral e o estado atual das vagas são [nao verificado]. ([CLAUDE.md:309](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:309))

### Item 1 — CONFERE

- **Premissas e armazenamento: CONFERE.** O laço percorre `ata.orfas` por célula, enquanto A5 conta faixas que atingem limiar; somar `len(orfas)` é uma medida distinta. O retrato inteiro é gravado em `MetricaSnapshot.detalhe`, um `JSONField`; os mapas cabem no mesmo documento e na mesma chave de snapshot, sem migration. ([esmeril_espelho.py:115](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:115), [esmeril_espelho.py:139](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:139), [esmeril_espelho.py:324](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:324), [models.py:95](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/inteligencia/models.py:95))
- **Medida e corte: CONFERE.** O cartório filtra batidas apuráveis antes da grade, de modo que a diferença para 1267 precisa ser medida e explicada, não presumida. A agregação serve à ordem pedida por END-L3; o resultado real, a cobertura das atas e a diferença para 1267 são [nao verificado]. ([cartorio.py:1170](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/cartorio.py:1170), [CORTES.md:535](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:535))
- **Dinheiro/exportada: CONFERE quanto ao desenho declarado de projeção.** A lavra é uma projeção gravada em snapshot; a confirmação de que os leitores permanecerão compatíveis depende do censo e da implementação. ([esmeril_espelho.py:317](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:317))

### Item 2 — PROCEDE

- **Premissa e corte: CONFERE.** Hoje a lista une IDs das assinaturas e do cadastro, filtra empresas visíveis e agrupa por posto; o PDF percorre esses grupos. A ordem global exigirá alterar ambos os consumidores, preservando o universo explicitado no aval. O corte diz “para todos”; a interpretação de limitar isso aos que já entram na lista foi explicitada no aval em revisão. ([cadastro_realidade.py:872](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:872), [cadastro_realidade.py:930](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:930), [cadastro_realidade.py:1083](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1083), [CORTES.md:535](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:535))
- **Literal da janela: PROCEDE.** Não há um único início da janela de frota armazenado hoje: `janela(c)` depende da empresa, entrega e existência de células da pessoa; o retorno da frota não publica esses inícios, e `data_ini` do snapshot é o dia da gravação. Assim, “desde DD/MM” no topo não tem valor único definido quando as janelas diferem. É necessário definir o significado e a apresentação antes de prometer esse rótulo. ([esmeril_espelho.py:38](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:38), [esmeril_espelho.py:219](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:219), [esmeril_espelho.py:229](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:229), [esmeril_espelho.py:326](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:326))
- **Ausência de medida: [nao verificado].** O aval distingue ausência de zero, mas não especifica posição na ordenação nem texto da linha para uma pessoa da lista sem medida; converter ausência em “0 batidas” contrariaria essa distinção. ([esmeril_espelho.py:95](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:95))

### Item 3 — CONFERE

- **Premissas: CONFERE, com correção de estrutura.** O proponente devolve `tipo_escala_id`, `codigo` e `ciclo` dentro de `proposta`; `confianca` fica no objeto externo. O esmeril o chama apenas quando há assinatura A e atualmente transforma o resultado em frase. Preservar frase e acrescentar mapa cabe no JSON existente. ([detector_proposta.py:153](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/detector_proposta.py:153), [esmeril_espelho.py:221](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:221), [esmeril_espelho.py:234](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:234))
- **Corte e alcance: CONFERE.** END-L1 pede botão para proposta; o aval mantém expressamente o universo atual do proponente. Pessoas presentes apenas por assinatura C podem, portanto, ficar sem proposta; quantas são em produção é [nao verificado]. ([CORTES.md:533](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:533), [cadastro_realidade.py:876](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:876))

### Item 4 — PROCEDE

- **Campos e porta: CONFERE.** “Diferente do template vigente” pode comparar `proposta.tipo_escala_id` com `template_id` do vínculo vigente hoje, já lido por `_vinculos`. Para 6x1, o campo é `ec.tipo_escala.tipo_ciclo == TipoEscala.CICLO_6x1`; o Plano chama a verificação de defeito que usa esse campo. C11 e A11 apontam ao Plano; A2/A8 hoje apontam ao vínculo. ([cadastro_realidade.py:748](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:748), [models.py:1241](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:1241), [views_plano_folgas.py:83](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views_plano_folgas.py:83), [cadastro_realidade.py:708](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:708))
- **Após o clique: CONFERE como desenho possível.** Não é preciso atualizar a lavra: a lista já lê o vínculo vigente no request; comparar seu `template_id` atual com o destino ainda lavrado pode retirar o botão após realocação bem-sucedida. A lavra segue diária, com cron previsto às 08:30. A guarda deve também ser conferida no POST para pedido direto ou repetido. ([cadastro_realidade.py:748](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:748), [crons.py:640](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:640))
- **Permissão e ciclo: CONFERE.** A C7 exige POST e `_is_gestor`, recupera destino da lavra e chama a porta; a recusa de cíclico está em **310–311**, não em 308. ([views.py:440](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:440), [views.py:463](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:463), [regua_defesa.py:310](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:310))
- **`!`: PROCEDE.** O aval declara o clique e o smoke para uma pessoa, mas não contém `!` literal para dado de escala/vínculo. A seção 7b exige `!` para esse dado, inclusive quando não retroativo, e manda interpretar o escopo literalmente. A porta fecha ou absorve vigências e regenera células; não é apenas leitura. Portanto, o toque de produção do item 4 carece de `!` literal verificável. ([CLAUDE.md:265](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:265), [vinculo.py:211](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:211), [vinculo.py:248](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:248), [vinculo.py:315](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:315))
- **Exportada, dinheiro e pouso: [nao verificado] no caso concreto.** A data de hoje limita o início pedido, mas os dados fornecidos não provam o estado da competência nem o efeito monetário concreto. Se houver apply de dinheiro em competência não exportada, a regra exige reversão prévia, prova de exportada intacta e prova de frota; gravado exportado exige `!` e sua porta própria. O pouso após a simulação é compatível com VESPERA; a régua geral de pouso continua aplicável. ([CLAUDE.md:406](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:406), [CLAUDE.md:411](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:411), [CORTES.md:536](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:536), [CLAUDE.md:320](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:320))

### RED R1–R7

- **R1 — CONFERE.** Três entradas distribuídas em duas células somam três; pessoa medida sem entradas soma zero. A5 não serve como substituto: conta faixas recorrentes, não o total das entradas. A classificação de pessoa sem células como medida ou ausente precisa ser definida pela implementação sem confundir os estados. ([esmeril_espelho.py:95](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:95), [esmeril_espelho.py:125](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:125), [esmeril_espelho.py:139](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:139))
- **R2 — CONFERE**, sujeito à definição da ausência de medida e ao achado da janela do item 2. O mutante por nome deve usar cargas diferentes para poder ser morto. ([cadastro_realidade.py:962](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:962))
- **R3 — CONFERE.** A estrutura e a frase podem coexistir no mesmo JSON; o teste deve ler efetivamente a frase pelo contrato antigo. ([esmeril_espelho.py:229](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:229))
- **R4 — PROCEDE.** A porta procura vínculo com `data_inicio=hoje`: se existir, pode atualizá-lo; só cria outro se não existir. O RED deve fixar a pré-condição de inexistência para exigir “vínculo novo”, ou testar separadamente criação e atualização. A retirada do botão pode usar o vínculo relido, como descrito no item 4. ([vinculo.py:100](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:100), [vinculo.py:259](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:259))
- **R5 — CONFERE.** A porta recusa 12x36 e 24x48 e orienta o wizard; o teste deve provar tanto a ausência do botão quanto a recusa do POST direto. ([regua_defesa.py:310](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:310), [models.py:386](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:386))
- **R6 — CONFERE.** O discriminante 6x1 existe no tipo do vínculo; o controle 12x36 impede generalizar a substituição de A2/A8 a todos. ([models.py:1246](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:1246), [cadastro_realidade.py:717](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:717))
- **R7 — CONFERE.** A guarda da C7 fornece o padrão verificável de POST e `_is_gestor`; o RED deve conferir ausência de escrita. ([views.py:440](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:440))

**PROCEDE: 4 — item 0, item 2, item 4 e R4.**

## 17/11 20:11 · sessao 0cbdddbdbae2 · PORTA-DO-EXPORT-SEM-BECO · construtor · origem=Code

Parei com a entrega **incompleta**: três asserções existentes exigem `NAO FOI MEDIDA HOJE`, mas o aval exige que `conferir` devolva a nova frase literal para `SEM_MEDICAO` ([test_porta_do_export.py:53](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/folha/tests/test_porta_do_export.py:53), [test_porta_do_export.py:529](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/folha/tests/test_porta_do_export.py:529), [test_porta_do_export.py:536](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/folha/tests/test_porta_do_export.py:536), [porta_export.py:932](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/folha/porta_export.py:932)). Pela inspeção, essas três asserções falharão; sua instrução manda parar e listar quando outro teste existente quebrar. A execução delas permanece **[não verificado]**. [nao verificado]

As alterações parciais ficaram na cópia:

- [porta_export.py](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/folha/porta_export.py): predicado de tentativa morta, leitura sem persistência, espera opcional, textos e montagem do bloco.
- [selo_leitores_no_mesmo_numero.py](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py): espera e encerramento do pedido no ramo com tentativa.
- [views.py](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/folha/views.py), [urls.py](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/folha/urls.py) e [_estado_da_porta.html](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/templates/folha/_estado_da_porta.html): rota e bloco exibido no preview.
- [test_porta_sem_beco.py](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/folha/tests/test_porta_sem_beco.py) e [test_porta_sem_beco_item2.py](/home/ronald/wt-codex-porta-do-export-sem-beco-200717/app/folha/tests/test_porta_sem_beco_item2.py): inversão das duas asserções autorizadas.

A análise sintática dos quatro módulos de produção alterados passou **[verificado localmente]**. Não executei testes com banco, conforme a restrição. **Ra–Rg não foram escritos nem executados; os mutantes não foram preparados nem executados; a cura e a suíte permanecem [não verificado].** Também falta concluir e conferir o mapa solicitado de escritores e leitores dos campos. [nao verificado]

LEI-AKITA: origem=folha/porta_export.py; testemunha=estado_da_porta; RED=Ra-Rg **[não entregues]**; quem-mais-le=censo do item 0 **[conferência da implementação incompleta]**; juizes novos=0 **[não verificado]**; padrao=job assincrono com estado visivel + escritor unico **[não verificado]**.

## 17/11 20:06 · sessao 867aa006560c · PORTA-DO-EXPORT-SEM-BECO · verificador · origem=Code

1. **CONFERE — os dois mutantes descritos são barrados**, considerando a saída normal do gerador. A AST passa a contar `AnnAssign`; uma atribuição adicional `recusa_txt: str = ''` aumenta a contagem, e substituir a atribuição original por ela também falha porque o selo exige `=` como primeira atribuição. O selo também executa `gerar_estado.py` e exige os dois prefixos no arquivo gerado, detectando a troca isolada de `recusa_txt` por `''` no argumento do modelo. ([test_recusa_estado.sh:22](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:22), [test_recusa_estado.sh:67](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:67), [gerar_estado.py:384](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:384))

2. **PROCEDE — ainda há bypass da verificação de publicação.** Exemplo de mutante: trocar o argumento `recusa_txt` por um bloco de código Markdown contendo linhas literais iniciadas pelos dois prefixos exigidos. A AST das atribuições continuaria igual e os dois `grep` encontrariam as linhas, embora os valores calculados não aparecessem no corpo informativo do ESTADO. O selo verifica apenas prefixos no arquivo inteiro, não a seção, o fechamento da frase ou os valores. É um contraexemplo por inspeção, não executado. ([gerar_estado.py:342](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:342), [gerar_estado.py:379](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:379), [test_recusa_estado.sh:68](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:68)) Além disso, o selo examina o arquivo **antes** da raspagem e cópia que produzem o ESTADO publicado; a presença final em produção é **[não verificado]**. ([relato.sh:38](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/relato.sh:38), [relato.sh:114](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/relato.sh:114), [raspar_publico.py:28](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/raspar_publico.py:28))

3. **PROCEDE — há efeitos fora do temporário e possibilidade de vermelho alheio ao contrato.** O destino explícito do ESTADO e o log sintético ficam em `$T`, mas as importações Python podem criar `__pycache__` na cópia, pois o selo não desativa bytecode; `git status` também pode atualizar o índice do worktree. A ocorrência efetiva depende do ambiente **[não verificado]**. ([test_recusa_estado.sh:36](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:36), [test_recusa_estado.sh:40](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:40), [gerar_estado.py:183](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:183))

   O gerador lê a raiz real, chama Git e tmux e usa o relógio; falhas desses subprocessos são convertidas em texto, portanto Git ausente ou worktree sem repositório não implicam, por si só, vermelho. Porém, por exemplo, se `esteira.pausada` existir e não puder ser aberto, a exceção de leitura pode derrubar o gerador e o selo fica vermelho por motivo alheio às frases. A chamada examinada de `fabrica_estado.ler` não executa o Docker: este aparece em `_lavrar`, chamado somente pelo caminho `--lavrar`. Dependência de rede no caminho executado completo: **[não verificado]**. ([gerar_estado.py:23](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:23), [gerar_estado.py:178](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:178), [gerar_estado.py:270](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:270), [fabrica_estado.py:24](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/fabrica_estado.py:24), [fabrica_estado.py:61](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/fabrica_estado.py:61))

4. **CONFERE — fluxo principal de escritores e leitores:**

   | Arquivo ou valor | Quem escreve / produz | Quem lê / usa |
   |---|---|---|
   | `gerar_estado.py` e `recusa_estado.py` da cópia | Escritor anterior **[não verificado]** | O selo lê o primeiro pela AST e pelo `grep`, e importa o segundo. ([test_recusa_estado.sh:18](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:18), [test_recusa_estado.sh:43](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:43)) |
   | `$T/app/core/recusa_critica.py` | O selo copia o arquivo da raiz. | `recusa_estado._leitor` carrega a cópia quando recebe `$T`. ([test_recusa_estado.sh:38](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:38), [recusa_estado.py:30](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/recusa_estado.py:30)) |
   | `$T/app/logs/ponto.log` | O Python do selo cria e acrescenta linhas sintéticas. | `ler` e `ler_porta_export` chamam os contadores comuns, que leem o arquivo e até três rotacionados e filtram data e porta. ([test_recusa_estado.sh:49](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:49), [recusa_estado.py:37](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/recusa_estado.py:37), [recusa_critica.py:95](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/app/core/recusa_critica.py:95)) |
   | `ponto.log` da raiz usada pelo gerador | `registrar` emite para o logger; o handler configurado grava `/app/logs/ponto.log`. Os envelopes e a porta do export chamam `registrar`. O arquivo efetivo em produção é **[não verificado]**. | O gerador passa `raiz` e a mesma data aos dois leitores; `recusa_estado` procura `raiz/app/logs`. ([recusa_critica.py:70](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/app/core/recusa_critica.py:70), [saas.py:160](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/app/config/settings/saas.py:160), [respostas.py:55](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/app/core/respostas.py:55), [porta_export.py:1071](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/app/folha/porta_export.py:1071), [gerar_estado.py:270](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:270), [recusa_estado.py:23](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/recusa_estado.py:23)) |
   | Contagens, `sem_arquivo`, `esperado` e frases | Os leitores comuns produzem as contagens; `recusa_estado` produz a representação de arquivo ausente e compõe as frases. | O selo compara a saída sintética com `True 1 2 0 1 True`; o gerador insere as frases em `recusa_txt`. ([recusa_critica.py:108](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/app/core/recusa_critica.py:108), [recusa_estado.py:41](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/recusa_estado.py:41), [recusa_estado.py:71](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/recusa_estado.py:71), [test_recusa_estado.sh:60](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:60), [gerar_estado.py:271](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:271)) |
   | `$T/ESTADO.md` | `gerar_estado.py` escreve no destino passado pelo selo. | Os dois `grep` do selo leem o arquivo; na publicação, `raspar_publico.py` lê outro ESTADO temporário e escreve no destino público. ([gerar_estado.py:399](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:399), [test_recusa_estado.sh:67](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:67), [raspar_publico.py:28](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/raspar_publico.py:28)) |

   O gerador também consulta outros medidores, arquivos de `logs/` e worktrees; o inventário integral de seus escritores e efeitos indiretos é **[não verificado]**. ([gerar_estado.py:198](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/gerar_estado.py:198), [raias_estado.py:51](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/raias_estado.py:51))

**PROCEDE: 2** — perguntas 2 e 3. Não executei o selo nem os mutantes, pois eles criam arquivos; as conclusões são por leitura. ([test_recusa_estado.sh:36](/home/ronald/codex_ws/494ffb93e96eaf04fc115a31f4233fac744b1709/bin/tests/test_recusa_estado.sh:36))

## 17/11 20:05 · sessao 774a688b709b · PORTA-DO-EXPORT-SEM-BECO · censo · origem=Code

**Sítios fora da lista de partida que mudam:** o script `bin/txt_antes_badge_linha.py` deixa de provocar a persistência de falha ao chamar `conferir`; e o comando de medição passa a esperar ou encerrar o pedido no ramo `--tentativa`. Os testes que exigem persistência ou recusa pela leitura também mudam. As ocorrências estão detalhadas em D e E. [bin/txt_antes_badge_linha.py:27,60; app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140–158; app/folha/tests/test_porta_sem_beco.py:107–115; app/folha/tests/test_porta_sem_beco_item2.py:226–232]

**Identidade da cópia:** o diretório recebido não tem metadados Git; a correspondência exata com o HEAD `ba4cb6c3c` está **[não verificado]**. O censo abaixo é da cópia presente, por inspeção de AST dos Python em `app/`, `bin/` e `scripts/`, complementada por busca textual e leitura dos fluxos. Chamadas ou acesso construídos dinamicamente que não apareçam nessas formas estão **[não verificado]**. Não há observação de banco, logs ou estado de produção.

Para encurtar as tabelas, `P` significa [app/folha/porta_export.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:768); `C`, [selo_leitores_no_mesmo_numero.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:70); `T`, [test_porta_sem_beco.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/tests/test_porta_sem_beco.py:1); `T2`, [test_porta_sem_beco_item2.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/tests/test_porta_sem_beco_item2.py:1); `T4`, [test_porta_sem_beco_item4.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/tests/test_porta_sem_beco_item4.py:1); `TV1`, [test_porta_sem_beco_v1.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/tests/test_porta_sem_beco_v1.py:1); e `TE`, [test_porta_do_export.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/tests/test_porta_do_export.py:1). Em cada célula, os números após o apelido são linhas desse arquivo.

### A — Escrita do pedido e do carimbo

| Função ou caminho | Escrita identificada |
|---|---|
| `abrir_a_porta → pedir` | `pedir` cria a linha diária com `PEDIDO`; se já terminal, chama `_mudar_estado` para reiniciá-la. P:1085,1095–1101,1108–1111,1382–1386. |
| `assumir → _cunhar` | Sem tentativa recebida, `_cunhar` cria ou atualiza a linha diária em `EM_CURSO`; se não assumir, cria ou atualiza linha própria com sufixo UUID e `NAO_ASSUMIU`. P:1123,1145–1155,1163–1180,1199–1204. |
| Comando → `_cunhar` | Sem `--tentativa` e sem trava, o comando passa `_cunhar` a `_fechar`, com `sem_trava=True`, para registrar a linha própria. C:145–149; P:1163–1175. |
| Comando → `assumir → _mudar_estado` | Com tentativa recebida, adota a linha por CAS de `PEDIDO` para `EM_CURSO`. C:163–166; P:1208–1214. |
| Comando → `concluir → lavrar` e `_mudar_estado` | `lavrar` faz `update_or_create` do **carimbo**; `concluir` muda o estado da tentativa no mesmo bloco transacional. C:183–188; P:768–780,1237–1258. |
| Comando → `encerrar → _falhar → _mudar_estado` | Encerra a tentativa em `FALHOU`, condicionada à identidade e estado da linha. C:183–190; P:1028–1058,1077–1082,1265–1269. |
| Comando → `falhar_ao_assumir → _cunhar` ou `encerrar` | A recuperação pode cunhar a tentativa própria e encerrá-la, ou registrar a recusa de assumir. C:164–169; P:1272–1292. |
| `disparar → encerrar` | Se o processo não nasce, encerra o pedido em `FALHOU`. P:1384–1386,1390,1417–1421. |
| `assumir`, `vigiar_abertos` ou `estado_da_porta` → `vigiar → _falhar → _mudar_estado` | Hoje, leitura de tentativa morta ou pedido vencido pode persistir `FALHOU`. P:1200–1202,1295–1315,1326–1329,1349. |
| `lavrar_verde_para_teste → assumir → concluir` | Helper de teste usa os caminhos de pedido e carimbo. P:875–891. | [nao verificado]
| Escrita direta em testes | `TE._carimbo` cria/atualiza as duas chaves; T2 atualiza uma linha por PK e cria um pedido diretamente; T2 também chama `lavrar` e `_mudar_estado` diretamente. TE:33–47; T2:42–52,102,305,314–317. | [nao verificado]

A lista inicial omite, portanto, o escritor físico do carimbo `lavrar`, o atualizador físico `_mudar_estado` e os caminhos `falhar_ao_assumir` e `disparar`, além das escritas diretas dos testes. Não encontrei exclusão dessas linhas nas formas estáticas inspecionadas; exclusão dinâmica está **[não verificado]**. P:779–780,1028–1058,1272–1292,1417–1421; TE:33–47. [nao verificado]

### B — Leitura das duas chaves

| Leitor | Leitura e caminho |
|---|---|
| `_linha_do_pedido` | `filter` da chave diária; chamado por `pedir`, `assumir` e `estado_da_porta`. P:1012–1015,1104,1200,1349. |
| `_linha_do_dia_travada` | `filter` da chave diária com `select_for_update`; chamado por `_cunhar`. P:1115–1120,1145. |
| `assumir` | Busca pedido por chave e UUID para adotá-lo. P:1208–1212. |
| `falhar_ao_assumir` | Busca pedido por chave e UUID antes da recuperação. P:1280–1289. |
| `vigiar_abertos` | Busca pelo prefixo `_PEDIDO + ':'` e estados abertos. P:1324–1329. |
| `estado_da_porta` | Lê o pedido por `_linha_do_pedido` e consulta diretamente o carimbo diário. P:1347–1352. |
| `conferir` | Recebe o resultado de `estado_da_porta` e lê o detalhe do carimbo divergente. P:820–826. |
| `abrir_a_porta` | Lê indiretamente pelo estado e por `conferir`. P:1382–1387. |
| `lavrar_verde_para_teste` | Faz `get` do carimbo depois de concluir. P:875–891. |
| Testes: consultas ao modelo fora de P | Carimbo: T:139,183–184; TV1:65; T2:90,132,266,341. Pedido: T2:258,262,567; T4:24. Há leituras por PK de linhas já obtidas em T2:351–352,430,440. | [nao verificado]
| Testes: gravação via ORM que também consulta | `update_or_create` da fixture das duas chaves. TE:42–47. | [nao verificado]

### C — Chamadores e leitura da tabela de textos

`_TEXTO_DO_ESTADO` é tabela, não função: o leitor identificado é `conferir`, em P:824; a definição está em P:931–938.

| Alvo | Chamadores de produção, comando e script |
|---|---|
| `estado_da_porta` | `conferir` P:820; `abrir_a_porta` P:1382. |
| `conferir` | `abrir_a_porta` P:1387; script `pe.conferir`, com import `from folha import porta_export as pe`, [bin/txt_antes_badge_linha.py:27,60](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/bin/txt_antes_badge_linha.py:27). |
| `vigiar` | `assumir` P:1202; `vigiar_abertos` P:1329; `estado_da_porta` P:1349. |
| `vigiar_abertos` | `C.handle`: import local `from folha import porta_export as _pe` C:71 e chamada C:76. |
| `trava_da_medicao` | `C.handle`: mesmo import local `_pe` C:71 e chamada C:146. |
| `medicao_viva` | `vigiar` P:1304. |
| `abrir_a_porta` | Import local `abrir_a_porta as _abrir_porta` e chamada em [app/folha/views.py:130–131](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/views.py:130). |

| Alvo | Chamadas em testes | [nao verificado]
|---|---|
| `estado_da_porta` | Alias local `pe`: T:85,102,112,122,125,136,148,159,195,204,218,219,278,284; TV1:35,39,67; T4:116,124; T2:53,93,100,103,111,134,171,172,180,181,188,190,230,231,306,318,323,368,408,410,465,482,521,526,544,563,597,620. T:289 guarda a função real e T:316 a substitui por mock. |
| `conferir` | Import local direto em TE:51–52,60–65,79–83,112–116,519–544; alias `pe` em T:87,96,149,160,167,168,176,177,206; TV1:41; T2:57,112,209,221,308. |
| `vigiar` | `pe.vigiar` em TV1:50 e T2:355; referência e mock em T2:603,612. |
| `vigiar_abertos` | `pe.vigiar_abertos` em T4:125; mocks em T2:611 e [app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:134](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:134). |
| `trava_da_medicao` | `pe.trava_da_medicao` em T:199,253; T2:47; T4:61,101. Mocks em T4:33 e [app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:135](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:135). |
| `abrir_a_porta` | `pe.abrir_a_porta` em T:308. |
| `medicao_viva` | Mock em TV1:49; chamada de produção em P:1304. |

### D — Efeito das duas alterações propostas

Aqui “leitura pura” pressupõe o comportamento descrito no dado: detectar a mesma tentativa morta ou pedido vencido e responder `FALHA` sem persistir. O retorno completo inclui o objeto `pedido`; seu `detalhe['estado']` continua aberto nessa leitura. P:1295–1315,1349–1369.

| Chamador | Estado passa a só ler | Espera opcional com default sem espera |
|---|---|---|
| `conferir`, P:820 | **MUDA** em pedido vencido ou sessão morta: mantém o código/texto de falha sob a premissa acima, mas deixa de persistir `FALHOU` e registrar a recusa. P:820–824,1077–1082,1295–1315. |
| `abrir_a_porta`, P:1382 | **NÃO MUDA** quanto à persistência pretendida se receber o `vigiar` anterior à leitura, como especificado no dado; continua podendo pedir e disparar após falha. Interleavings de concorrência da implementação futura: **[não verificado]**. P:1382–1387; dado raspado, item 1. |
| Chamadas de `estado_da_porta` em T, TV1, T2 e T4, listadas em C | **MUDA** o efeito observável quando a entrada está morta/vencida: não grava nem emite recusa; muda também o objeto `pedido` retornado. Falham especificamente a exigência de estado persistido em T:107–115 e a de uma recusa em T2:226–232. Nos casos que só verificam o código e recebem o mesmo código, a asserção indicada **NÃO MUDA** por essa razão; resultado da suíte inteira **[não verificado]**. | [nao verificado]
| `C.handle → trava_da_medicao`, C:140–158 | **MUDA** para `--tentativa` se o comando optar pela espera: hoje, sem a trava, imprime compartilhamento e continua; o dado pede aguardar e, no limite, encerrar o próprio pedido. Para cron/manual sem tentativa, **NÃO MUDA** pelo default sem espera. C:140–158; dado raspado, item 3. |
| Chamadas diretas de trava em T:199,253; T2:47; T4:61,101 | **NÃO MUDA** pela adição do parâmetro com default sem espera: não passam opção de espera. P:973–986; linhas de chamada citadas. |
| Mock de trava no teste do comando | **[não verificado]** quanto à implementação futura: o mock aceita `*a`, mas não palavras-chave; passar a opção nomeada nesse caminho pode quebrá-lo. [app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:135](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:135). |

### E — Fora da lista de partida

| Sítio | Mudança demonstrável pela proposta |
|---|---|
| Script `bin/txt_antes_badge_linha.py:27,60` | **MUDA:** chama `conferir`; em tentativa morta/vencida deixa de causar a gravação e a linha de recusa que hoje vêm de `estado_da_porta → vigiar`. O valor textual sob a premissa de mesma classificação continua o de falha. P:820,1077–1082,1295–1315,1349. |
| `C.handle`, C:140–158 | **MUDA:** o ramo do job com `--tentativa` hoje sai ao perder a trava; o dado determina espera e fechamento no limite. |
| T:107–115 | **MUDA:** exige explicitamente `pedido.detalhe['estado'] == FALHOU` depois da leitura. |
| T2:226–232 | **MUDA:** exige uma linha de recusa após duas leituras; leitura pura não a produz por si. P:1077–1082,1349. |

A lista de partida está incompleta como censo de funções: `lavrar` é quem efetivamente escreve o carimbo; `_mudar_estado` atualiza o detalhe do pedido; `falhar_ao_assumir` e `disparar` alcançam escritores; e há leitores ORM e fixtures fora da lista. As funções e caminhos estão discriminados em A e B, com as respectivas linhas. P:779–780,1028–1058,1284–1292,1417–1421; TE:33–47.

## 17/11 20:05 · sessao 35d8fddad05c · PORTA-DO-EXPORT-SEM-BECO · verificador · origem=Code

**AVAL-DEVOLVIDO — itens 0, 1, 3, 5 e RED.** Os demais itens conferem dentro dos limites abaixo. O diretório não contém metadados Git; portanto, a identidade da cópia como HEAD `ba4cb6c3c` está **[não verificado]**.

Os textos integrais dos avais 1655 e 1005 não estão disponíveis na cópia. A comparação com eles é limitada ao registro em [RELATO.md:39](app/docs/RELATO.md#L39) e [RELATO.md:46](app/docs/RELATO.md#L46); colisões com cláusulas não reproduzidas ficam **[não verificado]**. O corte VESPERA está em [CORTES.json:2015](app/docs/CORTES.json#L2015).

1. **Item 0 — PROCEDE.**
   - **Premissas:** os pontos de partida incluem os leitores e escritores centrais; o censo AST pedido ainda é trabalho futuro **[não verificado]**. O carimbo também tem escritor `lavrar` em [porta_export.py:768](app/folha/porta_export.py#L768).
   - **Cortes:** o registro de 1655 encaminha os itens 4–6 à obra, mas o texto integral não está disponível **[não verificado]** ([RELATO.md:46](app/docs/RELATO.md#L46)).
   - **Literal:** há sítio fora da lista inicial com mudança de comportamento: [txt_antes_badge_linha.py:60](bin/txt_antes_badge_linha.py#L60) chama `conferir`, que hoje chega a `vigiar` ([porta_export.py:820](app/folha/porta_export.py#L820), [porta_export.py:1349](app/folha/porta_export.py#L1349)). Pela própria condição do item 0, esse efeito deve constar no topo e não ser decidido sozinho. O teste de [test_porta_sem_beco_item2.py:226](app/folha/tests/test_porta_sem_beco_item2.py#L226) também depende da escrita pela leitura.
   - **Alcance:** o censo é de leitura; a mudança encontrada altera a persistência indireta, não demonstra escrita em exportada ou dinheiro ([porta_export.py:1295](app/folha/porta_export.py#L1295)). Outros alcances **[não verificado]**.

2. **Item 1 — PROCEDE.**
   - **Premissas:** `estado_da_porta` chama `vigiar` hoje; `abrir_a_porta` o faz **indiretamente**, ao chamar `estado_da_porta`, e não por chamada direta ([porta_export.py:1349](app/folha/porta_export.py#L1349), [porta_export.py:1382](app/folha/porta_export.py#L1382)).
   - **Cortes:** não identifiquei colisão nos trechos disponíveis; comparação integral com 1655 e 1005 **[não verificado]** ([RELATO.md:46](app/docs/RELATO.md#L46)).
   - **Literal:** o corpo indicado também considera morto um `PEDIDO` com `pedido_em` ausente ou inválido, não apenas vencido ([porta_export.py:1310](app/folha/porta_export.py#L1310)). Além disso, ele consulta banco e relógio; para ser *puro* literalmente, esses fatos precisam ser fornecidos como entradas, sem perder esse caso ([porta_export.py:996](app/folha/porta_export.py#L996), [porta_export.py:1314](app/folha/porta_export.py#L1314)).
   - **Alcance:** altera a vigília e a recusa persistida da porta, declaradas no item; o POST usa essa porta antes do TXT ([porta_export.py:1303](app/folha/porta_export.py#L1303), [views.py:128](app/folha/views.py#L128)). Escrita adicional em exportada, dinheiro ou `!` **[não verificado]**.

3. **Item 2 — CONFERE.**
   - **Premissas:** `medicao_viva` consulta a sessão do banco e tem o chamador produtivo indicado; `job_morreu` consulta PID do processo ([porta_export.py:996](app/folha/porta_export.py#L996), [porta_export.py:1304](app/folha/porta_export.py#L1304), [models.py:140](app/relatorios/models.py#L140)).
   - **Cortes:** manter a função não mostra colisão nos trechos disponíveis; avais integrais **[não verificado]** ([RELATO.md:39](app/docs/RELATO.md#L39)).
   - **Literal:** as perguntas das funções são distintas, como afirma o item ([porta_export.py:997](app/folha/porta_export.py#L997), [models.py:151](app/relatorios/models.py#L151)).
   - **Alcance:** o item mantém o comportamento; efeito novo sobre exportada, dinheiro ou `!` não está declarado nem demonstrado ([porta_export.py:996](app/folha/porta_export.py#L996)).

4. **Item 3 — PROCEDE.**
   - **Premissas:** com `--tentativa`, a trava ocupada leva à mensagem “compartilhada” e à saída sem fechar o pedido; sem tentativa, grava-se linha própria ([selo_leitores_no_mesmo_numero.py:140](app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py#L140)). A trava tem um chamador produtivo, compartilhado pelas modalidades, além de chamadores de teste ([selo_leitores_no_mesmo_numero.py:146](app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py#L146), [test_porta_sem_beco_item4.py:61](app/folha/tests/test_porta_sem_beco_item4.py#L61)).
   - **Cortes:** preservar cron sem absorção é compatível com o trecho registrado do item 4 de 1655 ([RELATO.md:39](app/docs/RELATO.md#L39), [porta_export.py:1163](app/folha/porta_export.py#L1163)); restante do texto integral **[não verificado]**.
   - **Literal:** `PEDIDO` impede outra tentativa de assumir pela linha do dia, mas isso **não prova** que quem já segura a trava a soltará em seguida; há inclusive possibilidade de colisão da chave CRC entre pares distintos ([porta_export.py:952](app/folha/porta_export.py#L952), [porta_export.py:1163](app/folha/porta_export.py#L1163)). Na borda do prazo, um vigia pode fechar o pedido antes da adoção; `encerrar` só vence se o CAS ainda encontrar a mesma tentativa aberta ([porta_export.py:1211](app/folha/porta_export.py#L1211), [porta_export.py:1265](app/folha/porta_export.py#L1265), [porta_export.py:1314](app/folha/porta_export.py#L1314)). O tratamento da corrida precisa ficar explícito.
   - **Alcance:** muda quando o job mede e encerra seu pedido, na porta declarada; não demonstra escrita em exportada ou dinheiro fora desse alcance ([selo_leitores_no_mesmo_numero.py:183](app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py#L183)). Outros efeitos **[não verificado]**.

5. **Item 4 — CONFERE.**
   - **Premissas:** o preview existe, exige `ver_folha` e retorna vazio sem os parâmetros; a tela inicial tem empresa vazia ([views.py:315](app/folha/views.py#L315), [exportar.html:26](app/templates/folha/exportar.html#L26)).
   - **Cortes:** a retirada das duas cascas responde ao problema registrado em [RELATO.md:39](app/docs/RELATO.md#L39); demais cláusulas integrais de 1655 e 1005 **[não verificado]**.
   - **Literal:** `exportar.html` estende `base.html`, que já carrega HTMX; existe precedente de polling restrito ao fragmento ([exportar.html:1](app/templates/folha/exportar.html#L1), [base.html:19](app/templates/base.html#L19), [_lote_progresso.html:4](app/templates/holerite/_lote_progresso.html#L4)). O funcionamento da rota nova ainda **[não verificado]**.
   - **Alcance:** acrescenta leitura na tela de exportação; o preview atual já consulta a classificação do export ([views.py:332](app/folha/views.py#L332)). Escrita em exportada, dinheiro ou `!` fora do declarado **[não verificado]**.

6. **Item 5 — PROCEDE.**
   - **Premissas:** `n` é gravado na tentativa e `concluido_em` é formado na transição; o POST divergente hoje contém a lista técnica ([porta_export.py:1232](app/folha/porta_export.py#L1232), [porta_export.py:1255](app/folha/porta_export.py#L1255), [porta_export.py:1053](app/folha/porta_export.py#L1053), [porta_export.py:825](app/folha/porta_export.py#L825)).
   - **Cortes:** o aval dado substitui o item 5 de 1655 e mantém a lista técnica no POST; a comparação com o texto integral anterior fica **[não verificado]** ([RELATO.md:39](app/docs/RELATO.md#L39), [porta_export.py:870](app/folha/porta_export.py#L870)).
   - **Literal:** “N divergências entre espelho e folha” não descreve fielmente o `n` exigido: ele inclui, entre outros, dias de HE sem decisão quando travam e cartão × TXT da competência inteira ([porta_export.py:680](app/folha/porta_export.py#L680), [porta_export.py:753](app/folha/porta_export.py#L753)). O texto precisa de decisão; não cabe trocá-lo silenciosamente. Para `PRONTA`, `conferir` precisa continuar retornando vazio para liberar o POST ([porta_export.py:820](app/folha/porta_export.py#L820)).
   - **Alcance:** a mudança textual não demonstra alteração de exportada ou dinheiro; a recusa existente permanece no caminho do POST ([views.py:130](app/folha/views.py#L130)). Efeito fora do declarado **[não verificado]**.

7. **RED / PROIBIDO / POUSO — PROCEDE.** [nao verificado]
   - **Premissas:** Rf corresponde ao comportamento atual: cron sem tentativa e sem trava grava `NAO_ASSUMIU` em linha própria ([selo_leitores_no_mesmo_numero.py:145](app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py#L145), [porta_export.py:1169](app/folha/porta_export.py#L1169)). Os RED e mutantes propostos ainda não foram executados **[não verificado]**.
   - **Cortes:** a janela de pouso coincide com VESPERA; o fim efetivo da simulação **[não verificado]** ([CORTES.json:2015](app/docs/CORTES.json#L2015)). A exceção nominal de vaga aparece no despacho, mas seu texto integral **[não verificado]** ([RELATO.md:46](app/docs/RELATO.md#L46)).
   - **Literal:** Rd promete adoção e medição se a trava soltar antes do limite, mas a liberação anterior ao limite não garante que a adoção ocorrerá antes de um vigia vencer o CAS. O caso precisa fixar essa sincronização ou decidir o resultado da corrida ([porta_export.py:1211](app/folha/porta_export.py#L1211), [porta_export.py:1314](app/folha/porta_export.py#L1314)). Re também depende de o encerramento vencer o CAS ([porta_export.py:1077](app/folha/porta_export.py#L1077)).
   - **Alcance:** o pouso toca export, como declarado; as proibições vedam escrita em GET e afrouxamento da porta. Ausência de efeitos em exportada, dinheiro ou `!` na implementação futura **[não verificado]** ([views.py:128](app/folha/views.py#L128)).

**PROCEDE: 5** — item 0: sítio adicional que perde escrita indireta; item 1: predicado incompleto e pureza literal; item 3: garantia de liberação e corrida no prazo; item 5: significado de `N`; RED: corrida de Rd/Re.

## 17/11 20:04 · sessao e31732ea5497 · - · - · origem=Ronald (tmux)

Censo e análise dos chamadores enviados ao agente principal, incluindo o script `bin/txt_antes_badge_linha.py` e os testes que dependem da persistência feita por `estado_da_porta`.

## 17/11 20:04 · sessao 9d6e3a864103 · - · - · origem=Ronald (tmux)

- O corpo de `vigiar` considera morto também um `PEDIDO` com `pedido_em` ausente ou inválido; essa condição precisa ser preservada na extração ([porta_export.py:1308](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1308)). O corpo atual consulta banco e relógio, portanto não é literalmente puro sem receber esses fatos como entradas ([porta_export.py:1303](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1303)).
- `abrir_a_porta` já aciona `vigiar` indiretamente por `estado_da_porta`; a chamada direta proposta muda a ordem e o ponto da persistência ([porta_export.py:1349](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1349), [porta_export.py:1382](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1382)). Um teste vigente espera emissão de recusa ao ler o estado e precisará ser ajustado ([test_porta_sem_beco_item2.py:226](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/tests/test_porta_sem_beco_item2.py:226)).
- O comando deixa o pedido órfão quando recebe `--tentativa` e não pega a trava; sem tentativa, grava a linha própria ([selo_leitores_no_mesmo_numero.py:145](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:145), [porta_export.py:1163](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1163)). A existência de `PEDIDO` impede outra tentativa sem UUID de assumir, mas não garante que a sessão que já segura a trava vá soltá-la logo; há inclusive colisão possível entre chaves CRC ([porta_export.py:952](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:952)).
- `vigiar_abertos` roda antes da medição e outro vigia pode vencer a corrida no limite, encerrando o pedido antes da adoção ([selo_leitores_no_mesmo_numero.py:74](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:74), [porta_export.py:1211](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1211)). `encerrar` só grava a recusa se vencer o CAS ([porta_export.py:1077](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1077)).
- O censo AST integral solicitado no item 0 permanece [não verificado].

## 17/11 20:04 · sessao d4a30cc27631 · - · - · origem=Ronald (tmux)

- **VESPERA:** o corte vigente proíbe pouso de alterações em folha ou export de 18/11 às 18h até o fim da simulação de 19/11; a janela proposta é compatível. O fim efetivo da simulação está **[não verificado]**. [CORTES.json:2015](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.json:2015)

- **Aval 1655, item 4:** o código documenta e implementa que cron ou manual sem `--tentativa` deixa uma linha própria `NAO_ASSUMIU` quando outra tentativa está aberta ou a trava está ocupada, preservando a linha da outra tentativa. Isso sustenta a compatibilidade da regra de não absorção e do controle Rf. [porta_export.py:1123](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1123), [porta_export.py:1163](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1163), [selo_leitores_no_mesmo_numero.py:145](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:145)

- **Limite da comparação com avais:** o relato registra a exceção nominal de vaga e identifica a pergunta sobre o pedido que perdeu a trava, mas os textos integrais citados dos avais 1655 e 1005 não foram localizados na cópia; a comparação com todas as suas cláusulas permanece **[não verificado]**. [RELATO.md:39](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/RELATO.md:39), [RELATO.md:46](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/RELATO.md:46)

- **Justificativa temporal do item 3:** `_cunhar` impede que outra execução assuma a linha aberta, mas isso não demonstra que quem já detém a trava a soltará em seguida. A trava cobre a medição e também admite colisão de CRC32 entre chaves distintas. A soltura antes do prazo está **[não verificado]**; o limite proposto prevê a falha quando ela não ocorrer. [porta_export.py:952](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:952), [selo_leitores_no_mesmo_numero.py:183](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:183)

## 17/11 20:04 · sessao 0758b69961af · - · - · origem=Ronald (tmux)

Censo de escritores e leitores enviado ao agente principal, incluindo as escritas diretas das fixtures e os caminhos do comando que passam escritores como argumentos. Referências principais: [porta_export.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1028), [selo_leitores_no_mesmo_numero.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:145), [test_porta_do_export.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/tests/test_porta_do_export.py:33) e [test_porta_sem_beco_item2.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/tests/test_porta_sem_beco_item2.py:42).

## 17/11 20:04 · sessao f2d2234f4bd7 · - · - · origem=Ronald (tmux)

Item 5 tem uma condição que não fecha: o `N` gravado é o total de `falhas`, que inclui categorias além de divergências entre espelho e folha, inclusive HE sem decisão quando configurada para travar e cartão × TXT da competência inteira ([porta_export.py:680](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:680), [porta_export.py:723](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:723), [porta_export.py:1217](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1217)). O texto literal precisa de decisão.

As demais premissas da tela conferem: a permissão `ver_folha` é usada no preview; a tela estende `base.html`, que carrega HTMX; e o registro da tentativa recebe `n` e `concluido_em` ([views.py:314](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/views.py:314), [exportar.html:1](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/templates/folha/exportar.html:1), [base.html:19](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/templates/base.html:19), [porta_export.py:1053](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1053), [porta_export.py:1255](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/folha/porta_export.py:1255)).

_6707 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
