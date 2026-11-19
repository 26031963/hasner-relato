# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 13:59** · publicado em **19/11/2026 14:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 13:59 · sessao 57d2e9a9fdd8 · ESPELHO-HE-ROTULO · leitura · origem=Code

A lista é exibida diretamente no espelho e no cartão PDF com texto fixo sobre decisão; esses dois pontos **não consultam o estado vigente para escolher esse texto**. Ela também é copiada para o retrato como `pontas` e transformada em `he_pendente` no portão de exportação, onde há leitores que consultam o estado vigente. [app/templates/ponto/espelho.html:189; app/relatorios/pdf_espelho.py:932; app/ponto/services/he_pendente_lavrado.py:148; app/folha/porta_export.py:184; app/folha/porta_export.py:681]

A consulta de `estado_por_dia` usa `DecisaoHE.objects.vigentes()`. Quando indico “sim”, a consulta ocorre antes da decisão ou apresentação descrita; isso não significa que ela filtre a lista original. [app/ponto/services/gestao_he.py:63; app/ponto/services/gestao_he.py:86]

| Leitor | Classe | O que faz e consulta de decisão |
|---|---|---|
| [espelho.html:189](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/templates/ponto/espelho.html:189) | **EXIBE** | Para cada ponta, mostra minutos e o rótulo fixo “sem decisão”; o tooltip usa hora, sentido e marco. **Não consulta** a decisão vigente para esse rótulo. [app/templates/ponto/espelho.html:189] |
| [pdf_espelho.py:932](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/relatorios/pdf_espelho.py:932) | **EXIBE** | Acrescenta às observações minutos e, conforme o sentido, o marco ou a descrição de dia sem jornada prevista; a frase diz “não conta” ou “não conta sem decisão”. **Não consulta** a decisão vigente para escolher essa frase. [app/relatorios/pdf_espelho.py:932; app/relatorios/pdf_espelho.py:937] |
| [he_pendente_lavrado.py:68](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/he_pendente_lavrado.py:68) | **CONTA** | Soma os minutos de todas as pontas do dia. A função **não consulta** decisão; seus chamadores usam o total para o retrato e o portão. [app/ponto/services/he_pendente_lavrado.py:55; app/ponto/services/he_pendente_lavrado.py:137; app/folha/porta_export.py:194] |
| [he_pendente_lavrado.py:132](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/he_pendente_lavrado.py:132) | **DECIDE / CONTA** | `apurar` seleciona dias com lista não vazia, conta dias e pendências e copia os campos para `pontas` no retrato. **Não consulta diretamente**: recebe um mapa opcional de decisões, cujo padrão é vazio; o comando de lavratura carrega `vigentes()` e fornece esse mapa. [app/ponto/services/he_pendente_lavrado.py:89; app/ponto/services/he_pendente_lavrado.py:132; app/ponto/services/he_pendente_lavrado.py:148; app/ponto/management/commands/lavrar_he_pendente.py:48; app/ponto/management/commands/lavrar_he_pendente.py:62] |
| [porta_export.py:184](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/folha/porta_export.py:184) | **DECIDE / CONTA** | Filtra os dias pela competência e transforma cada ponta em uma entrada de `he_pendente`, incluindo minutos da ponta e do dia. **Não consulta antes da coleta**; depois, `medir` consulta `estado_por_dia` para selecionar dias acima do limite e sem decisão, que podem contar como falhas e travar o TXT se a trava estiver ligada. [app/folha/porta_export.py:181; app/folha/porta_export.py:191; app/folha/porta_export.py:225; app/folha/porta_export.py:681] |
| [porta_export.py:858](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/folha/porta_export.py:858) | **EXIBE / CONTA** | Na mensagem de recusa do TXT, usa `he_pendente` para mostrar colaboradores, dias e minutos dos dias que travam. **Sim**, recebe os dias selecionados com o estado consultado na medição e guardado no resultado; não faz nova consulta ao montar a mensagem. [app/folha/porta_export.py:681; app/folha/porta_export.py:745; app/folha/porta_export.py:858] |
| [gestao_he.py:129](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/gestao_he.py:129) | **CONTA / EXIBE** | Lê o alias `pontas`, soma minutos antes e depois e escolhe o sentido dominante para a tira; `_padrao_em_palavras` conta sentidos para a frase de padrão. **Sim**, no fluxo de `enriquecer`, `estado_por_dia` é consultado antes dessas derivações. [app/ponto/services/gestao_he.py:127; app/ponto/services/gestao_he.py:137; app/ponto/services/gestao_he.py:205; app/ponto/services/gestao_he.py:250; app/ponto/services/gestao_he.py:303] |
| [gestao_he.html:105](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/templates/ponto/gestao_he.html:105) e [gestao_he.html:307](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/templates/ponto/gestao_he.html:307) | **EXIBE** | Mostra a tira e a tabela de apoio com sentido, hora, marco, minutos e estado de cada dia. **Sim**, a view entrega o retrato por `enriquecer`, que sobrepõe o estado vivo antes da renderização. [app/templates/ponto/gestao_he.html:105; app/templates/ponto/gestao_he.html:307; app/ponto/views.py:945; app/ponto/services/gestao_he.py:250; app/ponto/services/gestao_he.py:278] |
| [views.py:841](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/views.py:841) | **EXIBE / CONTA** | O PDF da Gestão de HE mostra dias, quantidade sem decisão e minutos fora derivados do retrato. **Sim**, chama `enriquecer`, que consulta `estado_por_dia`, antes de montar a tabela. [app/ponto/views.py:804; app/ponto/views.py:807; app/ponto/views.py:841; app/ponto/services/gestao_he.py:250] |
| [gestao_he.py:401](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/gestao_he.py:401) | **DECIDE** | `itens_sem_decisao` seleciona, do retrato enriquecido, dias e minutos para os atos, conforme ausência de decisão e o critério de limite. **Sim**, indiretamente por `enriquecer`. [app/ponto/services/gestao_he.py:401; app/ponto/services/gestao_he.py:250] |
| [gestao_he.py:466](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/gestao_he.py:466) | **CONTA** | Conta, no retrato, os dias sem decisão para o contador da Central. **Sim**, consulta `estado_por_dia` antes de contar. [app/ponto/services/gestao_he.py:466; app/ponto/services/gestao_he.py:475] |
| [recusar_ponta_pequena.py:120](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/management/commands/recusar_ponta_pequena.py:120) | **DECIDE / CONTA** | Lê dias e minutos derivados do retrato, exclui dias já decididos e encaminha candidatos à porta de recusa de ponta pequena. **Sim**, consulta `DecisaoHE.objects.vigentes()` antes de percorrer os dias. [app/ponto/management/commands/recusar_ponta_pequena.py:120; app/ponto/management/commands/recusar_ponta_pequena.py:133; app/ponto/management/commands/recusar_ponta_pequena.py:156; app/ponto/management/commands/recusar_ponta_pequena.py:170] |

A distinção sobre o espelho e seu PDF é limitada ao **rótulo**: o resultado do motor que alimenta a montagem pode, ele próprio, consultar decisões vigentes; isso não é uma consulta de estado usada por esses dois leitores para escolher a frase exibida. [app/ponto/services/espelho.py:1241; app/ponto/motor_calculo_v2.py:1417; app/templates/ponto/espelho.html:189; app/relatorios/pdf_espelho.py:932]

**Escritores da lista no montador:**

- [espelho.py:735](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/espelho.py:735): inicializa `_he_pendencias` vazia. [app/ponto/services/espelho.py:735]
- [espelho.py:741](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/espelho.py:741): atribui o retorno de `_marcar_pontas`. [app/ponto/services/espelho.py:741]
- [espelho.py:754](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/espelho.py:754): concatena as pontas traduzidas do relato do motor. [app/ponto/services/espelho.py:754]
- [espelho.py:760](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/espelho.py:760): concatena entradas de dias sem jornada prevista. [app/ponto/services/espelho.py:760]
- [espelho.py:790](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/espelho.py:790): coloca a lista na chave `he_fora_da_janela` do dia. [app/ponto/services/espelho.py:790]

## 19/11 13:59 · sessao 73ff15d4b022 · - · - · origem=Ronald (tmux)

A montagem e os escritores identificados foram comunicados ao agente principal, com as distinções entre a consulta de decisões antes da lavratura e a consulta antes da decisão de travar o export. [app/ponto/services/espelho.py:735](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/espelho.py:735), [app/ponto/management/commands/lavrar_he_pendente.py:48](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/management/commands/lavrar_he_pendente.py:48), [app/folha/porta_export.py:680](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/folha/porta_export.py:680)

## 19/11 13:59 · sessao 7dea0595464f · DST-L2-JZ1-C7 · verificador · origem=Code

**Resultado: BLOQUEIA.** O diff implementa a decisão de retenção depois da trava, mas o refresh também muda o comportamento fora de `_APROVANDO` quando o ponteiro no banco difere da instância. Isso contraria a leitura literal de B2(iii), que diz “fora de `_APROVANDO` nada muda”; o próprio teste novo documenta e exige essa mudança. Uma autorização para essa diferença está [não verificado]. ([teste C7:17](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:17), [teste C7:177](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:177))

A cópia disponível contém o código e o teste do diff, mas não contém `.git`; portanto, não pude verificar o HEAD nem reconstruir o diff com Git. Os anexos foram fornecidos no pedido sem numeração própria de linhas; onde dependo deles, identifico o anexo e marco a localização em arquivo como [não verificado]. [nao verificado]

1. **B1 — PROCEDE-MENOR.** A implementação entra no `atomic`, chama `trava_na_ordem`, faz refresh somente de `justificativa_relacionada`, consulta o juiz existente e, havendo retenção, chama `registrar_retencao` e retorna `(False, 'retencao')` antes da cerca. A trava é condicional à existência de `colaborador_id`. ([reconciliador.py:536](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:536), [reconciliador.py:544](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:544))

   A leitura anterior ao `atomic` **saiu**; não há duas consultas. O literal diz que ela “pode ficar”, e não que deve ficar; sua remoção, isoladamente, não contradiz essa frase. O teste anterior também cobra uma única consulta da regra na unidade. A consequência de remover a saída cedo é que um chamado já retido passa a entrar na transação e a tomar a ordem global antes de retornar; isso exige a análise do item 5. ([reconciliador.py:552](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:552), [teste c3:436](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:436); literal B1 no anexo [não verificado])

   O diff fornecido altera somente o reconciliador e acrescenta o teste; não altera código de criador [não verificado quanto à completude do diff fornecido]. [nao verificado]

2. **B2 — PROCEDE-MENOR.** Os testes chamam `retratar_com_motivo` real; nos casos de aprovação, marcam o contexto com `no_ato_de_aprovar`, sem passar pela porta completa de aprovação do DP. A costura chama a trava real e injeta uma vez em torno dela; nos testes de uma conexão, as alterações são feitas **depois** da trava real, na mesma transação. Isso força a ordem de execução, mas não demonstra que outro criador consiga commitar depois da trava. ([teste C7:71](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:71), [teste C7:104](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:104))

   - **(i) Morde por análise:** J3 com origem nasce depois da trava real; exige retenção, chamado aberto, J3 pendente e uma trilha. No código velho, a consulta anterior já teria passado e D3 alcançaria J3 por origem. Para o velho passar, seria necessário que a criação ocorresse antes da consulta antiga ou que a decisão fosse novamente tomada após a costura. ([teste C7:116](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:116), [justificativa.py:279](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:279))
   - **(ii) Morde por análise:** atualiza o ponteiro no banco e mantém a instância antiga; exige retenção e preservação de um título alterado na memória. O código velho precisaria receber a instância já atualizada ou reler o ponteiro para passar. O `update` direto é costura de teste, não a porta produtiva de escrita. ([teste C7:135](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:135), [vinculos.py:81](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/services/vinculos.py:81))
   - **(iii) É controle, não RED:** pelo vínculo de origem, exige resolução e rejeição por D3. O código velho também pode satisfazê-lo; ele demonstra a regra no cenário montado, não ausência de qualquer mudança fora de `_APROVANDO`. O teste adicional iii-b confirma a mudança descrita no item 3. ([teste C7:160](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:160), [teste C7:177](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:177))
   - **(iv) Morde por análise:** duas instâncias velhas passam pela unidade, com o ponteiro alterado na primeira chamada; exige duas recusas e uma linha. O código velho precisaria enxergar J3 nas duas leituras antigas para atender essas asserções. A deduplicação existente compara o último detalhe gravado. ([teste C7:198](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:198), [reconciliador.py:490](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:490))
   - **Duas conexões: morde por análise.** A thread R para **antes** da trava real; a outra conexão cria J3 e a libera por `Event`. No código velho, a leitura cedo já ocorreu antes da pausa; para passar, J3 teria de existir antes daquela leitura ou haver nova decisão depois. No código novo não existe leitura cedo, apesar da descrição do teste. O teste chama a unidade real, mas cria a justificativa diretamente com o cartório silenciado, não pela API, tela ou cron completos. ([teste C7:230](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:230), [teste C7:41](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:41))

   RED executado antes da alteração, resultado verde e logs dos três mutantes: **[não verificado]**. Comentários que afirmam morte dos mutantes não são a evidência de execução. ([teste C7:24](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:24))

3. **Fora de `_APROVANDO` — BLOQUEIA.** No cenário de iii-b, J3 é pendente, não tem `chamado_origem=ch`, e o banco passa a apontar para ela enquanto `ch` em memória ainda tem ponteiro vazio. Antes, D3 usava o ponteiro velho e não alcançava J3: o chamado resolvia e J3 permanecia pendente. Depois, o refresh põe o ponteiro gravado na instância; D3 alcança e rejeita J3, enquanto o chamado resolve. Isso é uma mudança observável, embora a regra D3 continue sendo usada. ([teste C7:177](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:177), [justificativa.py:279](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:279), [justificativa.py:337](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:337))

   O texto de B2(iii) combina “nada muda” com “a rejeição pelo D3 segue sendo a regra”. O diff satisfaz a segunda frase no caso testado, mas não a primeira em seu sentido literal. É necessário devolver essa divergência para decisão; autorização para ela [não verificado].

4. **Escritores, leitores e significado — PROCEDE-MENOR.**

   - **Ponteiro `justificativa_relacionada`:** a porta `apontar_justificativa` grava sob a regra “primeiro vale”; a tela de justificativa e o cron mostrado a chamam. D3 e a retenção leem o ponteiro da instância; outros leitores incluem a transição humana e a premissa regularizada. ([vinculos.py:81](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/services/vinculos.py:81), [ponto/views.py:1648](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/views.py:1648), [processar_alertas_turno.py:426](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/management/commands/processar_alertas_turno.py:426), [justificativa.py:280](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:280), [models.py:142](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/models.py:142), [models.py:1325](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/models.py:1325))
   - **`Justificativa.chamado_origem`:** a tela o preenche em justificativa existente ou criada; o cron o fornece na criação automática. O vínculo é lido por D3 e também pela rotina que trata o chamado de origem após aprovação. ([ponto/views.py:1634](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/views.py:1634), [processar_alertas_turno.py:392](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/management/commands/processar_alertas_turno.py:392), [justificativa.py:33](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:33), [justificativa.py:279](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:279))
   - **Significados:** não são FKs inversas garantidas. Uma representa a justificativa apontada pelo chamado; a outra, o chamado que originou a justificativa. D3 deliberadamente usa a união e aceita inclusive ponteiro para justificativa com outra origem. Assim, são duas relações usadas como evidência de vínculo para D3, não dois campos necessariamente equivalentes. ([models.py:75](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/models.py:75), [ponto/models.py:173](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/models.py:173), [justificativa.py:267](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:267), [justificativa.py:298](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:298))
   - **Status e trilha:** o ato pede a transição do chamado para `resolvido` e chama o escritor de rejeição das justificativas; esse escritor grava `status='rejeitada'`. A retenção lê e grava a trilha por `registrar_retencao`, que consulta a última linha e chama `evento`; o teste também lê `valor_depois`. A deduplicação da justificativa alheia compara o ID pendente, embora a linha também armazene `em_aprovacao`: uma mudança apenas de `em_aprovacao` não gera outra linha por essa comparação. ([reconciliador.py:572](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:572), [justificativa.py:347](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:347), [reconciliador.py:465](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:465), [teste C7:49](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:49))

   **Depois do retorno:** geofence lê `pk` e `colaborador_id`, mas também devolve `perdeu`, que contém a própria instância `ch`; portanto, o ponteiro atualizado pode ser observado por quem consumir esse retorno. O comando de fantasmas passa `c` ao resolver e à nota e lê identificadores; os dois laços do reconciliador usam o resultado e, em erro, o `pk`. Não identifiquei nesses trechos leitura direta posterior do ponteiro; efeitos em todos os consumidores indiretos de `perdeu` e do resolver estão [não verificado]. ([geofence.py:437](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/geofence.py:437), [geofence.py:443](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/geofence.py:443), [reconciliar_fantasmas.py:281](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/management/commands/reconciliar_fantasmas.py:281), [reconciliador.py:1054](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:1054), [reconciliador.py:1515](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:1515))

5. **Classes de reprovação — BLOQUEIA pela mudança do item 3; ordem e selos pendentes de evidência.**

   - **Juiz duplicado, segundo escritor, derivação paralela e relógio fora da borda:** nas linhas produtivas adicionadas, o diff chama o juiz e o escritor existentes, sem nova derivação ou leitura de relógio. Isso é uma inspeção do diff fornecido, não um censo de toda a aplicação. ([reconciliador.py:557](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:557))
   - **RED e selo:** há casos que mordem por análise no item 2, mas vermelho anterior, verde e três mutantes mortos em disco estão [não verificado]. O controle iii não é, por si, um RED. ([teste C7:160](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:160))
   - **Ordem de trava:** agora até uma retenção que antes sairia cedo toma, quando há colaborador, a ordem `Colaborador → Disputa → Chamado`, podendo esperar ali; não está demonstrado que a espera seja inofensiva em todos os caminhos. A aprovação do DP chama `travar_origem_antes` antes de travar a justificativa, portanto esse caminho já chega com a ordem global tomada para o universo selecionado. Os caminhos diretos de geofence e fantasmas chamam a unidade sem uma trava anterior visível nos trechos examinados; os caminhos públicos via `retratar`, inclusive comandos e sinais, precisam ser avaliados no contexto transacional de cada chamador. Ausência de inversão ou deadlock em **todos** eles: [não verificado]. ([reconciliador.py:544](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:544), [models.py:2816](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/models.py:2816), [justificativa.py:126](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:126), [justificativa.py:138](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:138), [geofence.py:443](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/geofence.py:443), [reconciliar_fantasmas.py:281](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/management/commands/reconciliar_fantasmas.py:281), [reconciliador.py:594](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:594))
   - **Pré-requisito e pouso:** o brief anexado afirma etapa A fechada e liberação de ordem; os arquivos e evidências dessa etapa, a posição na fila, suite, opus e pouso/deploy estão [não verificado]. [nao verificado]
   - **Campo com dois significados:** as FKs têm significados distintos, mas D3 explicitamente as reúne para esta pergunta. O problema demonstrado pelo diff é a mudança de qual valor do ponteiro D3 observa, inclusive fora da aprovação. ([justificativa.py:267](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/ponto/services/justificativa.py:267))

6. **Proibidos — PROCEDE-MENOR.** No diff fornecido, não há alteração dos arquivos proibidos, da ordem interna de `trava_na_ordem`, de criador, nem introdução de juiz ou fallback. O refresh novo é de um campo; o refresh da instância inteira na recusa da cerca já aparece como contexto, sem adição no diff. A realização de suite antes do verificador, uso das vagas, deploy e demais condições operacionais estão [não verificado]. ([reconciliador.py:557](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:557), [reconciliador.py:563](/home/ronald/codex_ws/8ccab728f2a9b9d9130541ea853a6f66d9028fe3/app/chamados/reconciliador.py:563); diff e literal nos anexos [não verificado quanto à completude])

## 19/11 13:59 · sessao 8ab032f9d007 · ESPELHO-HE-ROTULO · sonda · origem=Code

# app/ponto/services/espelho.py:1210 — assinatura; retorno em app/ponto/services/espelho.py:1310
# app/ponto/services/gestao_he.py:63 — assinatura; mapa de decisões vigentes em app/ponto/services/gestao_he.py:86
# app/ponto/services/espelho.py:790 — lista; campo em minutos somado conforme app/ponto/services/he_pendente_lavrado.py:68

from collections import defaultdict
from datetime import date, datetime
from zoneinfo import ZoneInfo
import re
import sys

from django.db import connection

from colaboradores.models import Colaborador
from ponto.services.espelho import espelho_do_colab
from ponto.services.gestao_he import estado_por_dia
from ponto.services.he_pendente_lavrado import data_do_dia, minutos_fora_do_dia


INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)

# Instante fixo no fim do periodo solicitado; nao representa a hora da execucao.
AGORA = datetime(2026, 11, 20, 23, 59, 59, tzinfo=ZoneInfo("America/Sao_Paulo"))

SEM_DECISAO = "sem_decisao"
CLASSES_INICIAIS = (SEM_DECISAO, "autorizado", "nao")


def somente_leitura(execute, sql, params, many, context):
    texto = sql.strip()
    if texto.endswith(";"):
        texto = texto[:-1].rstrip()
    if ";" in texto or not re.match(r"^(SELECT|SET|SHOW)\b", texto, re.IGNORECASE):
        raise RuntimeError("SQL bloqueado pela guarda de leitura")
    return execute(sql, params, many, context)


def relatar_erro(pk, exc):
    # A mensagem da excecao pode conter dados pessoais; publica-se a classe.
    nome = type(exc).__name__
    if not re.fullmatch(r"[A-Za-z_][A-Za-z_0-9]*", nome):
        nome = "Exception"
    print(f"ERRO colab_pk={pk} excecao={nome} mensagem=[redigida]")


def executar():
    if getattr(connection, "schema_name", None) != "juliani":
        print("ERROS=1 schema_invalido=1 NAO_CONCLUIDO")
        return 1

    classes = defaultdict(lambda: {"dias": 0, "colabs": set(), "minutos": 0, "amostra": []})
    for estado in CLASSES_INICIAIS:
        classes[estado]

    universo_dias = 0
    universo_colabs = set()
    universo_minutos = 0
    erros = 0

    try:
        ids = list(
            Colaborador.objects.filter(empresa_id__in=(2, 3, 4))
            .order_by("pk")
            .values_list("pk", flat=True)
        )
    except Exception as exc:
        relatar_erro(0, exc)
        print("ERROS=1 NAO_CONCLUIDO")
        return 1

    for pk in ids:
        try:
            colab = Colaborador.objects.get(pk=pk)
            espelho = espelho_do_colab(
                colab, FIM, INI, FIM, ano=2026, agora=AGORA
            )
            decisoes = estado_por_dia([pk], INI, FIM)
            linhas = []
            datas_vistas = set()

            for dia in espelho["dias"]:
                data = data_do_dia(dia)
                if data is None:
                    raise ValueError("Data de dia ilegivel")
                if not INI <= data <= FIM:
                    continue
                if data in datas_vistas:
                    raise ValueError("Dia duplicado no espelho")
                datas_vistas.add(data)
                if not dia["he_fora_da_janela"]:
                    continue

                chave = (pk, data.isoformat())
                bruto = decisoes[chave] if chave in decisoes else SEM_DECISAO
                # Um estado fora deste formato nao e impresso para evitar dados pessoais.
                if not isinstance(bruto, str) or not re.fullmatch(r"[a-z_]{1,32}", bruto):
                    raise ValueError("Estado bruto nao imprimivel")
                minutos = minutos_fora_do_dia(dia)
                linhas.append((data, minutos, bruto))

            # Consolida apenas depois que todo o colaborador foi processado.
            for data, minutos, bruto in linhas:
                grupo = classes[bruto]
                grupo["dias"] += 1
                grupo["colabs"].add(pk)
                grupo["minutos"] += minutos
                if len(grupo["amostra"]) < 10:
                    grupo["amostra"].append((pk, data.isoformat(), minutos, bruto))
                universo_dias += 1
                universo_colabs.add(pk)
                universo_minutos += minutos
        except Exception as exc:
            erros += 1
            relatar_erro(pk, exc)

    ordem = list(CLASSES_INICIAIS) + sorted(set(classes) - set(CLASSES_INICIAIS))
    contador_dias = 0
    contador_colabs = set()
    contador_minutos = 0

    for estado in ordem:
        grupo = classes[estado]
        contador_dias += grupo["dias"]
        contador_colabs.update(grupo["colabs"])
        contador_minutos += grupo["minutos"]
        print(
            f"CLASSE={estado} dia_colab={grupo['dias']} "
            f"colabs_distintos={len(grupo['colabs'])} minutos={grupo['minutos']}"
        )
        for pk, data, minutos, bruto in grupo["amostra"]:
            print(
                f"AMOSTRA classe={estado} colab_pk={pk} data={data} "
                f"minutos={minutos} estado_bruto={bruto}"
            )

    print(
        f"UNIVERSO dia_colab={universo_dias} "
        f"colabs_distintos={len(universo_colabs)} minutos={universo_minutos}"
    )
    print(
        f"CONTADOR dia_colab={contador_dias} "
        f"colabs_distintos={len(contador_colabs)} minutos={contador_minutos}"
    )
    print(f"ERROS={erros}")
    if erros:
        print("NAO_CONCLUIDO")
        return 1
    if (
        contador_dias != universo_dias
        or contador_colabs != universo_colabs
        or contador_minutos != universo_minutos
    ):
        print("NAO_CONCLUIDO divergencia=1")
        return 1
    return 0


try:
    with connection.execute_wrapper(somente_leitura):
        codigo = executar()
except Exception as exc:
    relatar_erro(0, exc)
    print("ERROS=1 NAO_CONCLUIDO")
    codigo = 1

if codigo:
    sys.exit(codigo)

## 19/11 13:59 · sessao 4cef408e07d9 · - · - · origem=Ronald (tmux)

Leitores de apresentação identificados:

- [espelho.html:189](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/templates/ponto/espelho.html:189) — **EXIBE** cada ponta com minutos e o rótulo fixo “sem decisão”; o tooltip usa hora, sentido e marco. A view entrega os dias de `espelho_do_colab` ao template sem consultar o estado da decisão para esse rótulo ([views.py:160](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/views.py:160), [views.py:196](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/views.py:196)).
- [pdf_espelho.py:932](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/relatorios/pdf_espelho.py:932) — **EXIBE** nas observações os minutos e, conforme o sentido, a frase de dia sem jornada ou o sentido e marco. O PDF obtém os dias de `espelho_do_colab` ([pdf_espelho.py:596](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/relatorios/pdf_espelho.py:596)); não consulta `DecisaoHE` ou `estado_por_dia` para formular essa frase [não verificado].

Há uma consulta indireta relevante: o motor consulta `DecisaoHE.objects.vigentes()` ([motor_calculo_v2.py:1397](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/motor_calculo_v2.py:1397)) e, para dia sem previsto, deixa de publicar a ponta quando o dia está autorizado ([motor_calculo_v2.py:1451](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/motor_calculo_v2.py:1451)); o montador traduz esse relato para a lista ([espelho.py:760](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/espelho.py:760)). Isso não é uma consulta do estado pelo template ou pelo texto do PDF para escolher o rótulo ([espelho.html:189](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/templates/ponto/espelho.html:189), [pdf_espelho.py:932](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/relatorios/pdf_espelho.py:932)).

## 19/11 13:59 · sessao 291761594f20 · - · - · origem=Ronald (tmux)

Rastreamento Python enviado ao agente principal, incluindo leitores diretos, aliases no retrato e escritores da lista.

## 19/11 13:54 · sessao a3cce7227f37 · CADEIA-20-PASSO-1 · verificador · origem=Code

**Veredito: BLOQUEIA.** A cópia trata os dois defeitos específicos de associação apontados na volta 2, mas ainda não prova correspondência por célula, equivalência da costura ao comando nem teto de consultas na primeira lavra. A alteração da reconciliação também alcança dados lidos por caminhos de pagamento; seu efeito concreto em produção é **[não verificado]**. ([cartorio.py:987](app/ponto/services/cartorio.py:987); [cartorio.py:1023](app/ponto/services/cartorio.py:1023); [test_cadeia20_costura.py:31](app/escala/tests/test_cadeia20_costura.py:31); [test_cadeia20_lavra.py:222](app/escala/tests/test_cadeia20_lavra.py:222); [leitor_celula.py:420](app/escala/services/leitor_celula.py:420))

A árvore disponível não é um repositório Git; a identidade do `HEAD` e os diffs não puderam ser conferidos independentemente. A comparação abaixo usa os diffs fornecidos e o código presente. Execução dos testes, resultados dos mutantes e dados de produção são **[não verificado]**. ([test_cadeia20_mutantes.py:1](app/escala/tests/test_cadeia20_mutantes.py:1); [não verificado])

**1. Bloqueios da volta 2**

- **Associação e identidade — NÃO BLOQUEIA pelos dois defeitos específicos da volta 2; garantia completa [não verificado].** A grade agora forma `orfas` a partir dos pares ordenados `(hora, instante)`; a lavra associa ocorrências repetidas pela posição dentro da mesma hora, desde que as contagens coincidam. Na reconciliação, remove hora e instante pela mesma posição e recusa instante duplicado. Os REDs descrevem os dois cenários, mas sua execução é **[não verificado]**. ([utils.py:1867](app/escala/utils.py:1867); [utils.py:2043](app/escala/utils.py:2043); [utils.py:2084](app/escala/utils.py:2084); [cartorio.py:976](app/ponto/services/cartorio.py:976); [test_cadeia20_costura.py:66](app/escala/tests/test_cadeia20_costura.py:66))

  Sobra uma limitação de identidade: candidatos de pausas são deduplicados entre si por `pk`, mas podem coexistir com cópias em turnos; o juiz examina os carimbos de todos os encontrados e toma o primeiro turno encontrado. O código não demonstra que cópias da mesma batida, com papéis diferentes entre segmentos, sempre produzam a resposta correta. ([orfa_classe.py:25](app/escala/orfa_classe.py:25); [orfa_classe.py:40](app/escala/orfa_classe.py:40); [orfa_classe.py:68](app/escala/orfa_classe.py:68))

- **Universo agregado versus por célula — BLOQUEIA.** `contador_igual_universo` continua comparando apenas a soma global. Uma entrada excedente em uma célula pode compensar uma faltante em outra; a igualdade tampouco verifica hora ou identidade. O teste remove uma entrada, mas não testa compensação entre células. ([cartorio.py:1002](app/ponto/services/cartorio.py:1002); [cartorio.py:1023](app/ponto/services/cartorio.py:1023); [test_cadeia20_lavra.py:135](app/escala/tests/test_cadeia20_lavra.py:135))

- **Costura versus comando e mutantes — BLOQUEIA como prova completa.** A costura força uma célula com `apply_=True`; o comando percorre as células passadas do colaborador e também tem modos sem `--apply` e sem `--forcar`. O patch do builder agora alcança também a referência importada pelo teste, e o catálogo inclui os mutantes dos dois defeitos da volta 2; isso não estabelece execução bem-sucedida nem equivalência a todos os modos do comando. ([test_cadeia20_costura.py:27](app/escala/tests/test_cadeia20_costura.py:27); [processar_cartorio.py:25](app/ponto/management/commands/processar_cartorio.py:25); [processar_cartorio.py:61](app/ponto/management/commands/processar_cartorio.py:61); [test_cadeia20_mutantes.py:51](app/escala/tests/test_cadeia20_mutantes.py:51); [test_cadeia20_mutantes.py:95](app/escala/tests/test_cadeia20_mutantes.py:95))

- **Fonte única e selo com mordida — BLOQUEIA como prova completa.** Os pares vêm dos índices do alinhador e passam a ser mantidos juntos nas operações citadas; os dois novos REDs e mutantes visam precisamente a associação e a remoção. Permanecem **[não verificado]** sua execução, a cobertura de fronteiras e a prova por célula. ([utils.py:1723](app/escala/utils.py:1723); [utils.py:2084](app/escala/utils.py:2084); [test_cadeia20_mutantes.py:51](app/escala/tests/test_cadeia20_mutantes.py:51))

**2. O que muda em `turnos.py`**

**NÃO BLOQUEIA como afirmação de que o corpo de `parear_turnos` foi alterado:** o diff fornecido não altera sua definição nem seu corpo; a assinatura presente começa em [turnos.py:1192](app/ponto/turnos.py:1192). Comportamento idêntico em todas as execuções é **[não verificado]**; o fato verificável é que o diff não edita essa função. ([turnos.py:1192](app/ponto/turnos.py:1192); [não verificado])

O diff acrescenta `avulsas=None` às assinaturas de `turnos_do_colab` e `_turnos_do_colab_puro`, e a casca passa o argumento ao corpo. Dentro de cada segmento, o resultado de `parear_turnos` passa a ser materializado em `list`; se o coletor foi fornecido, recebe `(seg_ini, seg_fim, batidas)` com cópias de `seg_bs` carimbadas como miolo e ausentes, por identidade de objeto, de todos os turnos pareados daquele segmento. Depois permanece o filtro por `data_turno` e a ordenação do retorno. Com o default `None`, o coletor não é preenchido e os chamadores continuam recebendo a lista filtrada e ordenada de turnos; igualdade comportamental universal é **[não verificado]**. ([turnos.py:1850](app/ponto/turnos.py:1850); [turnos.py:1920](app/ponto/turnos.py:1920); [turnos.py:1984](app/ponto/turnos.py:1984); [turnos.py:1997](app/ponto/turnos.py:1997); [turnos.py:3056](app/ponto/turnos.py:3056))

O diff inteiro fornecido não contém alteração em `motor_calculo_v2.py`; a conferência independente por Git é **[não verificado]**. Não decido aqui se a edição de `turnos.py` viola o aval. ([motor_calculo_v2.py:503](app/ponto/motor_calculo_v2.py:503); [não verificado])

**3. Escritores, leitores e significados**

- **`razoes_orfas` e `falha` — NÃO BLOQUEIA quanto aos escritores diretos encontrados.** `razao_da_sobra` constrói cada entrada e sua `falha`; `_lavrar_razoes_sobras` pode substituir razão e falha por `BATIDA_AMBIGUA` e atribui a lista à ata, inclusive `[]` para ausência de sobras com insumos. É chamada nos dois ramos antes da porta `lavrar_veredito`. `medir_razoes_sobras` lê `razao` e `falha`; os demais leitores diretos encontrados são testes. Leitores dinâmicos ou externos à cópia são **[não verificado]**. ([orfa_classe.py:80](app/escala/orfa_classe.py:80); [cartorio.py:959](app/ponto/services/cartorio.py:959); [cartorio.py:995](app/ponto/services/cartorio.py:995); [cartorio.py:1015](app/ponto/services/cartorio.py:1015); [cartorio.py:1397](app/ponto/services/cartorio.py:1397); [cartorio.py:1978](app/ponto/services/cartorio.py:1978))

- **Impressão da lavra — BLOQUEIA se a obra exigir atualização automática das atas antigas.** `impressao_insumos` monta o hash com batidas, cobertura, chamados, DNA e opções declaradas; não inclui a nova lista nem a marca CXR. Impressão igual pula a célula sem `forcar`; o comando oferece `--forcar`. A relavra efetiva da competência é **[não verificado]**. ([cartorio.py:490](app/ponto/services/cartorio.py:490); [cartorio.py:499](app/ponto/services/cartorio.py:499); [cartorio.py:601](app/ponto/services/cartorio.py:601); [cartorio.py:1515](app/ponto/services/cartorio.py:1515); [processar_cartorio.py:29](app/ponto/management/commands/processar_cartorio.py:29))

- **Coletor `avulsas` — NÃO BLOQUEIA quanto ao significado declarado, com a ressalva das cópias.** O escritor é `_turnos_do_colab_puro`; a grade cria a lista e a coloca em `insumos_razao_sobras`; a lavra achata todos os segmentos, descartando os limites ao formar `pausas`. Uma batida pode ter cópias rasas em segmentos distintos; a deduplicação posterior usa `pk` quando existe. ([turnos.py:1984](app/ponto/turnos.py:1984); [turnos.py:2000](app/ponto/turnos.py:2000); [utils.py:1936](app/escala/utils.py:1936); [utils.py:2117](app/escala/utils.py:2117); [cartorio.py:975](app/ponto/services/cartorio.py:975); [orfa_classe.py:25](app/escala/orfa_classe.py:25))

- **Instantes da grade — BLOQUEIA quanto à prova de todas as fronteiras.** `locs` contém datetimes localizados; o coletor do alinhador fornece índices para recuperar instantes, e a conservação de batidas não consumidas usa a data civil local. O alinhamento organiza sobras pela data do turno; a reconciliação de saída cruzada procura a lista do dia seguinte. Portanto a chave de data da coleção pode representar o dia ao qual a grade atribuiu a sobra por caminhos diferentes; `hora` é `HH:MM` local e o outro elemento é o instante completo. `data_turno` e `fuso` entram na assinatura de `razao_da_sobra`, mas seu corpo não os usa para filtrar candidatos. Correção de todos os casos de meia-noite é **[não verificado]**. ([utils.py:1343](app/escala/utils.py:1343); [utils.py:1723](app/escala/utils.py:1723); [utils.py:1782](app/escala/utils.py:1782); [utils.py:1841](app/escala/utils.py:1841); [utils.py:2001](app/escala/utils.py:2001); [orfa_classe.py:30](app/escala/orfa_classe.py:30))

- **Medida — BLOQUEIA se seu booleano for usado como prova de correspondência.** O retorno conta sobras, entradas com e sem razão, falhas, células e órfãs sem chave, e uma igualdade global. `falhas` conta entradas sem razão que tenham `falha`; não prova que toda entrada sem razão tenha uma falha nomeada. Os leitores diretos encontrados são testes; outros são **[não verificado]**. ([cartorio.py:1002](app/ponto/services/cartorio.py:1002); [test_cadeia20_costura.py:52](app/escala/tests/test_cadeia20_costura.py:52))

**4. Classes estruturais**

- **Segundo juiz — NÃO BLOQUEIA por duplicação demonstrada; registro da nova pergunta [não verificado].** O registro vivo aponta `turnos_do_colab` para pertencimento ao turno, `_match_marcos` para ocupação de marco e `dia_sem_trabalho` para aquela pergunta. A nova função consulta os resultados e classifica a razão; não estabeleci que sua pergunta inteira duplique uma registrada. ([juizes.py:215](app/core/juizes.py:215); [juizes.py:302](app/core/juizes.py:302); [orfa_classe.py:30](app/escala/orfa_classe.py:30))

- **Segundo escritor — NÃO BLOQUEIA nos escritores diretos encontrados:** a montagem e a substituição de `falha` ocorrem antes da atribuição pela função de lavra, nos dois caminhos que chegam à porta do veredito. Escritores dinâmicos ou externos são **[não verificado]**. ([cartorio.py:959](app/ponto/services/cartorio.py:959); [cartorio.py:995](app/ponto/services/cartorio.py:995); [cartorio.py:1978](app/ponto/services/cartorio.py:1978))

- **Relógio fora da borda e `timezone.now()` solto — NÃO BLOQUEIA por chamada nova demonstrada; trânsito completo [não verificado].** O comando obtém `hoje` e `agora` na borda; os testes ORM relevantes congelam o relógio. A lavra consulta o fuso corrente, enquanto o juiz recebe `fuso` sem usá-lo no corpo. ([processar_cartorio.py:38](app/ponto/management/commands/processar_cartorio.py:38); [test_cadeia20_costura.py:20](app/escala/tests/test_cadeia20_costura.py:20); [test_cadeia20_lavra.py:200](app/escala/tests/test_cadeia20_lavra.py:200); [cartorio.py:994](app/ponto/services/cartorio.py:994); [orfa_classe.py:30](app/escala/orfa_classe.py:30))

- **Derivação paralela e campo com dois significados — BLOQUEIA quanto à prova de semântica nas fronteiras.** A origem dos instantes é o alinhamento da grade, mas a coleção por data reúne atribuição por dia de turno e conservação por dia civil. A classificação ainda usa `HH:MM` para rajada e instante completo para encontrar a batida; não são a mesma unidade de identidade. ([utils.py:1723](app/escala/utils.py:1723); [utils.py:1841](app/escala/utils.py:1841); [orfa_classe.py:50](app/escala/orfa_classe.py:50); [orfa_classe.py:57](app/escala/orfa_classe.py:57))

- **Selo de consultas — BLOQUEIA como prova do custo da primeira lavra.** Ele mede a diferença entre os braços na **segunda** chamada de cada um, reutilizando os mesmos objetos `colab` e `cels` que cada braço já percorreu. Assim mede consultas adicionais dessa segunda passada; uma consulta por dia dependente de cache de objeto aquecido na primeira pode escapar. O limite `cxr.call_count <= 1` também é contado após reset na segunda passada. Isso não demonstra que a implementação atual faça consulta por dia: esse fato é **[não verificado]**. ([test_cadeia20_lavra.py:206](app/escala/tests/test_cadeia20_lavra.py:206); [test_cadeia20_lavra.py:222](app/escala/tests/test_cadeia20_lavra.py:222); [test_cadeia20_lavra.py:239](app/escala/tests/test_cadeia20_lavra.py:239))

- **Ordem de trava — NÃO BLOQUEIA por inversão demonstrada; prova completa [não verificado].** O trecho novo de CXR consulta snapshot e monta mapas; as transações existentes do cartório e o `select_for_update` encontrado na porta da célula estão em outros pontos. A ordem de todas as travas transitivas não foi estabelecida. ([cxr_na_lista.py:152](app/ponto/services/cxr_na_lista.py:152); [cxr_na_lista.py:165](app/ponto/services/cxr_na_lista.py:165); [cartorio.py:1109](app/ponto/services/cartorio.py:1109); [celula.py:630](app/ponto/portas/celula.py:630))

**5. Leitores que pagam**

**BLOQUEIA quanto à alegação de isolamento dos dados existentes.** O diff muda a reconciliação que remove uma órfã e preenche uma célula da grade; esses dados podem entrar na ata lavrada. A folha projeta `ata['orfas']` pela grade da célula e usa órfãs em causas; o caminho de `DiaPago` lê campos da grade no fechamento. O motor lê a ata para construir períodos e chama diretamente `parear_turnos`; também chama `_match_marcos`, cuja assinatura ganhou coletor opcional. Não encontrei leitura direta de `razoes_orfas` nesses caminhos. Diferença concreta de valor pago ou de resultado em produção é **[não verificado]**. ([utils.py:2043](app/escala/utils.py:2043); [utils.py:2084](app/escala/utils.py:2084); [leitor_celula.py:180](app/escala/services/leitor_celula.py:180); [leitor_celula.py:420](app/escala/services/leitor_celula.py:420); [causas.py:43](app/folha/causas.py:43); [fechamento.py:1060](app/ponto/services/fechamento.py:1060); [dia_pago.py:364](app/ponto/services/dia_pago.py:364); [motor_calculo_v2.py:503](app/ponto/motor_calculo_v2.py:503); [motor_calculo_v2.py:616](app/ponto/motor_calculo_v2.py:616); [motor_calculo_v2.py:3057](app/ponto/motor_calculo_v2.py:3057))

## 19/11 13:54 · sessao 2c978e7b59fc · FERIADO-POR-POSTO · verificador · origem=Code

**AVAL-DEVOLVIDO — revisão do item A.** A construção não foi iniciada. A cópia não contém metadados Git; por isso, o hash do HEAD informado não pôde ser confirmado. Os resultados de produção abaixo são os dos anexos fornecidos, sem acesso independente ao banco ou aos logs. **[não verificado]**

**A1 — NÃO CONFERE integralmente.** O comando atual recusa FECHA sem município, identifica chamados vivos cujo vínculo no dia pertence ao posto e cujo feriado o alcança, e encerra no dry-run antes de escrever. No `--apply`, salva a marca, registra evento com antes/depois e depois tenta retratar cada chamado por `transicionar_com_pauta(..., ato=ch.reconciliar)`; a marca e cada chamado usam transações separadas. [marcar_feriado_do_posto.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/marcar_feriado_do_posto.py:86), [comando:103](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/marcar_feriado_do_posto.py:103), [comando:124](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/marcar_feriado_do_posto.py:124), [comando:128](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/marcar_feriado_do_posto.py:128).

`estrutura.marcar_opera_em_feriado` apenas atribui e salva; a busca por chamadas Python encontrou o comando como único chamador atual. A porta de vínculo recebe `(ec, desejado, *, autor=None, motivo='')`, salva a chave e regenera células, mas **não registra diretamente uma trilha da alteração da chave**. A trilha existente é a da regeneração, condicionada a células alteradas. [estrutura.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/estrutura.py:83), [comando:131](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/marcar_feriado_do_posto.py:131), [vinculo.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/vinculo.py:869), [celula.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/portas/celula.py:369).

A sonda anexada compõe o universo e os grupos em código inline; `q_vinculo_cobre_a_data` e a lista de ciclos existem no código, mas a existência de uma função reutilizável equivalente ao predicado completo da sonda é **[não verificado]**. A exceção administrativa também não integra o predicado inline anexado. [alimentacao.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/alimentacao.py:219), [models.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/models.py:32). `cargo_e_administrativo` normaliza o texto e consulta uma lista de quatro cargos; o sincronizador atual passa vínculos divergentes pela porta de vínculo. [sincronizar_feriado_admin.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/sincronizar_feriado_admin.py:4), [sincronizador:42](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/sincronizar_feriado_admin.py:42).

**A2 — NÃO SEI.** Reaplicar a marca pela nova porta é comportamento proposto, ainda inexistente; a porta atual do vínculo retorna imediatamente quando a chave já é igual ao valor desejado. [vinculo.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/vinculo.py:890). Que todos os 11 J sejam elegíveis após excluir cargos administrativos é **[não verificado]**.

**A3 — CONFERE, com limite.** Os três sítios copiam a chave e o posto do objeto de vínculo usado como fonte. No terceiro, a fonte é `antigo`; isso não prova que o posto de `antigo` seja igual ao do vínculo ajustado que está sendo desfeito. [vinculo.py:967](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/vinculo.py:967), [vinculo.py:1084](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/vinculo.py:1084), [vinculo.py:1095](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/vinculo.py:1095).

**A4 — NÃO CONFERE no literal “nasce com 11” como conclusão provada.** O anexo informa 11 J, mas o predicado J não exclui cargos administrativos e só mede OPERA × chave False; A4 também inclui o sentido FECHA × chave True. O brief informa zero no sentido inverso, mas não demonstra a classificação administrativa dos J. **[não verificado]** O contador existente é um molde, não o mesmo universo: filtra colaborador ativo, chave True e somente 5x2/6x1; a sonda usa também semanal e não inclui aquele filtro de situação. [models.py:919](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/models.py:919), [models.py:32](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/models.py:32).

**A5 — NÃO CONFERE integralmente.** `sugestoes()` chama `presenca_em_feriado()` e calcula as sugestões a cada chamada; retorna apenas postos ainda sem marca e com evidência suficiente. Assim, “lida, nunca recalculada” não descreve a função atual como leitura de um resultado persistido, e ela não fornece sugestão para posto já marcado. [feriado_do_posto.py:74](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/feriado_do_posto.py:74), [feriado_do_posto.py:136](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/feriado_do_posto.py:136). A permissão de `form_posto` é `editar_postos`. [views.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/views.py:732). A busca textual nos HTML não encontrou os nomes das duas marcas; leitura ou escrita indireta por outro nome é **[não verificado]**. A disponibilidade de sugestões para os postos 596 e 3 vem apenas do anexo. **[não verificado]**

**R1 — NÃO CONFERE integralmente.** A porta atual é idempotente para o mesmo valor: não salva nem regenera novamente. Porém, não garante uma linha de trilha por vínculo: registra trilha de regeneração somente quando há alteração de célula, e futuras são regeneradas em chamadas individuais, podendo produzir nenhuma ou várias linhas. O comportamento completo da nova propagação ainda é **[não verificado]**. [vinculo.py:890](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/vinculo.py:890), [cadastro_futuro.py:43](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/cadastro_futuro.py:43), [celula.py:369](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/portas/celula.py:369).

**R2 — NÃO SEI.** A exclusão administrativa está declarada para a porta proposta, mas não é aplicada automaticamente pela própria `marcar_trabalha_em_feriado`; o chamador terá de aplicá-la. [vinculo.py:869](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/vinculo.py:869), [sincronizar_feriado_admin.py:19](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/sincronizar_feriado_admin.py:19).

**R3 — NÃO SEI.** A retratação descrita existe hoje no comando, para os chamados selecionados e sujeitos ao gate da transição; sua passagem pela porta proposta ainda não existe. [marcar_feriado_do_posto.py:109](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/marcar_feriado_do_posto.py:109), [comando:139](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/marcar_feriado_do_posto.py:139).

**R4 — CONFERE quanto ao universo declarado.** A lista de ciclos da sonda inclui 6x1, 5x2 e semanal, sem 12x36; a exclusão pela futura porta permanece **[não verificado]**. [models.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/models.py:32).

**R5 — NÃO SEI no literal completo.** A regeneração usa a guarda de lavra e pula o dia protegido, salvo a abertura explícita `apesar_da_lavra`, que a porta de vínculo não passa. Isso sustenta a proteção da célula nesse caminho. Já a integridade de todo o `FechamentoMensal` exportado não decorre apenas dessa guarda: dias tocados agendam recálculo por competência, e o escritor de fechamento não barra competência exportada. A ocorrência concreta de recálculo de um fechamento exportado neste cenário é **[não verificado]**. [celula.py:218](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/portas/celula.py:218), [celula.py:284](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/portas/celula.py:284), [celula.py:483](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/portas/celula.py:483), [fechamento.py:485](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/fechamento.py:485).

**R6 — CONFERE quanto à guarda atual.** A regeneração só reescreve célula existente quando sua origem é `gerada`; a busca de futuras também seleciona apenas `gerada`. Isso é compatível com a regra de preservação da editada descrita no anexo; a vigência independente do aval FILA-PARALELA-1150 é **[não verificado]**. [celula.py:291](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/portas/celula.py:291), [cadastro_futuro.py:21](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/cadastro_futuro.py:21).

**R7 — NÃO CONFERE na contagem e NÃO SEI no dinheiro.** Nas linhas J do anexo há **11**, não 10, dias-colaborador com batidas: sete em 02/11 e quatro em 15/11. A apurabilidade e os valores monetários independentes são **[não verificado]**. O motor consulta `feriado_em_dobra` e `eh_feriado(dia)` para a dobra, sem consultar ali se o dia é previsto; isso, isoladamente, não prova que rubricas e total permanecem iguais. [motor_calculo_v2.py](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/motor_calculo_v2.py:914). Além disso, o recálculo por evento é agendado em `on_commit`: um ensaio que termine em `atomic + raise` não executa esse callback por si só; a medição precisa contemplar esse fato. [fechamento.py:1798](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/fechamento.py:1798).

**R8 — NÃO SEI.** O contador, a propagação e o mutante propostos ainda não existem; não há resultado RED/GREEN a verificar. **[não verificado]**

**SELO — NÃO SEI.** O selo AST e o mutante são propostos, sem resultado apresentado. Hoje há um chamador textual da função de escrita em `estrutura`, mas isso não prova um selo AST sobre todas as formas possíveis de escrever o campo. [estrutura.py:83](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/estrutura.py:83), [comando:131](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/management/commands/marcar_feriado_do_posto.py:131).

**PROIBIDO — NÃO SEI quanto a colisão integral.** O desenho mantém expressamente fora do escopo a caixa de vínculo, DIA-CONVOCADO, o motor e `SO_EM_DIA_DE_JORNADA`; a cópia registra DIA-CONVOCADO como complemento futuro. Não localizei os textos independentes dos avais CANCELA-UI e FILA-PARALELA pelo nome, portanto sua vigência integral é **[não verificado]**. [BACKLOG.md](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/BACKLOG.md:2280). `SO_EM_DIA_DE_JORNADA` contém somente `falta`, e a guarda consulta a jornada do vínculo. [precedencia.py:498](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/precedencia.py:498), [precedencia.py:536](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/precedencia.py:536).

O fato para o `!`: a propagação proposta, disparada pelo ato do admin, grava `EscalaColaborador.trabalha_em_feriado`; a seção 7b item 2 nomeia **dado de ESCALA ou VINCULO, inclusive retroativo**, entre os itens que exigem `!`. Se esse ato se enquadra na exigência ou se o aval apresentado já a satisfaz, **não decido**. [vinculo.py:892](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/vinculo.py:892), [CLAUDE.md:265](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:265).

Quanto a dinheiro, a regeneração pode agendar `recalcular_por_evento`; este chama `recalcular_fechamento_mes`, que grava `FechamentoMensal` e chama a lavratura de `DiaPago` na versão motor e, a partir de 10/2026, também na versão oráculo. Os valores efetivamente alterados e qualquer alcance fora das condições declaradas são **[não verificado]**. [celula.py:483](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/portas/celula.py:483), [fechamento.py:1798](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/fechamento.py:1798), [fechamento.py:1137](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/fechamento.py:1137), [fechamento.py:1191](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/fechamento.py:1191), [fechamento.py:1265](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/fechamento.py:1265).

**VAGA — NÃO SEI.** Vaga, fila viva, diff do passo 1, arquivo `os41.txt`, estado da pista e trava da VM não foram demonstrados pelo código ou pelos anexos fornecidos. **[não verificado]**

## 19/11 13:53 · sessao 532e7d4efe15 · ESCALA-NAO-DESCREVE · leitura · origem=Code

**O cadastro participa das duas decisões:** ajuda a atribuir batidas a um turno e pode mudar qual batida fecha esse turno. A decisão também usa fatos das batidas e regras fixadas no código; não é uma simples leitura da escala prevista. O retorno marca `aberto=True` exatamente quando há entrada e não há saída escolhida. [app/ponto/turnos.py:312](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:312)

**Autoridade registrada.** Em `JUIZES['turno/marcos']`, `turnos_do_colab` responde “a que turno (`data_turno`) pertence a batida?”. Em `JUIZES['tela']`, responde “quais são os turnos do colaborador (pares de batida)?”. A linha 1014 é uma pendência do leitor `escala/views.py::propostas_limbo_pdf`, para “padrão batido / proposta”, que **deveria ler** essa autoridade; não é uma segunda declaração de juiz. [app/core/juizes.py:215](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/core/juizes.py:215) [app/core/juizes.py:727](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/core/juizes.py:727) [app/core/juizes.py:739](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/core/juizes.py:739) [app/core/juizes.py:1013](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/core/juizes.py:1013)

**Caminho de alimentação.** `turnos_do_colab` busca batidas apuráveis e células de `ini − 2 dias` a `fim + 1 dia`; carrega vínculos candidatos e inclui as geradoras nomeadas pelas células. Ao final, só devolve turnos cujo `data_turno` pertence ao segmento solicitado. [app/ponto/turnos.py:1834](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1834) [app/ponto/turnos.py:1867](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1867) [app/ponto/turnos.py:1889](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1889) [app/ponto/turnos.py:1997](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1997)

- A candidatura lê `EscalaColaborador.data_inicio`, `data_fim` e `ativa`. Para cada dia, `vinculo_do_dia` privilegia a `CelulaDia.escala_geradora` nomeada, desde que encontrada e dentro da vigência; sem geradora, escolhe entre vínculos que cobrem o dia, por ativo e início mais recente. Dias consecutivos com o mesmo dono formam um segmento; dia sem dono também é pareado. [app/escala/alimentacao.py:148](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/alimentacao.py:148) [app/escala/alimentacao.py:269](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/alimentacao.py:269) [app/ponto/turnos.py:1961](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1961)
- Do `tipo_escala` do dono, o pareador recebe `hora_inicio`, `hora_fim` e o par de intervalo; recebe também o segundo par quando os dois pares estão completos. Recebe ainda o mapa do `CelulaDia.dna.marcos` por data: `hi`, `hf`, `hii`, `hfi`. Se o template declara o segundo intervalo completo, esse mapa de quatro posições **não é passado naquele segmento**, para não perder o segundo intervalo do template. [app/ponto/turnos.py:1937](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1937) [app/ponto/turnos.py:1989](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1989) [app/ponto/turnos.py:1504](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1504) [app/escala/utils.py:84](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/utils.py:84)

**(a) A que turno pertence a batida.** O pareador ordena as batidas por instante. Para decidir se uma batida é interna, procura os marcos de intervalo e borda do DNA na data local da batida; se não encontra candidatos úteis, usa os marcos do template do segmento. A distância é medida ao horário nas datas local, anterior e seguinte. Uma batida até **90 minutos** de um marco de intervalo é absorvida se ele estiver ao menos tão próximo quanto a borda; uma volta deslocada pode ser absorvida até **3 horas** depois da saída de intervalo, respeitada a distância da borda. [app/ponto/turnos.py:1217](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1217) [app/ponto/turnos.py:1229](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1229) [app/ponto/turnos.py:326](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:326) [app/ponto/turnos.py:1356](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1356) [app/ponto/turnos.py:1402](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1402)

A **ata da célula também intervém**: `papel_por_minuto_da_ata` consulta `periodos_do_dia`, que lê `CelulaDia.ata.lampadas` e `dna.marcos`. Quando a ata descreve o turno e passa as guardas, seu papel `E`/`S` prevalece sobre o tipo gravado; `X`/`Xi` torna a batida interna. Duas luzes de pausa reconhecidas recebem `Xi`, mesmo se o intervalo ultrapassar o teto de três horas usado para inferência. Ata desalinhada, turno incompleto, minutos ambíguos ou as duas pontas a mais de 180 minutos dos respectivos marcos são exemplos em que aquele dia não fornece esse papel. [app/ponto/turnos.py:1565](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1565) [app/ponto/juiz_batida.py:149](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/juiz_batida.py:149) [app/ponto/turnos.py:1631](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1631) [app/ponto/turnos.py:1682](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1682) [app/ponto/turnos.py:1695](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1695) [app/ponto/turnos.py:1827](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1827)

A âncora da ata é o **dia da célula e a sequência de seus marcos congelados**: `periodos_do_dia` reconstrói os instantes esperados a partir de `celula.data`, `hi`, `hf` e pausas; a luz `HH:MM` recebe a data do marco, ajustada em um dia se a diferença passar de 12 horas. Isso permite que uma saída civil em D+1 pertença ao turno da célula D. [app/ponto/juiz_batida.py:86](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/juiz_batida.py:86) [app/ponto/juiz_batida.py:166](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/juiz_batida.py:166)

Para `data_turno`, a regra geral parte da data local da entrada, ou da saída órfã. A exceção consulta `hi`/`hf` da véspera: se cruzam a meia-noite, uma entrada antes de `hf` e a mais de 90 minutos de `hi` pode ir para ontem; se há saída, ela deve caber no envelope ancorado no `hi` da véspera, com duração prevista mais quatro horas. A saída órfã usa o mesmo envelope. A consulta à véspera se cala em certas condições quando já existe turno anterior; com mapa diário presente usa os marcos da véspera, e sem mapa usa as duas bordas do template. [app/ponto/turnos.py:216](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:216) [app/ponto/turnos.py:285](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:285) [app/ponto/turnos.py:1253](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1253) [app/ponto/turnos.py:1295](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1295)

**(b) Onde fecha.** Uma saída elegível fecha a entrada corrente, mas o teto lê `hi`/`hf` do DNA da **data local da entrada**: duração prevista, calculada módulo 24 horas, mais quatro horas. Sem as duas bordas ou com bordas iguais, o laço usa o envelope de 26 horas. Se **ambas** as batidas estão a mais de 180 minutos de seus respectivos marcos, o laço volta ao envelope de 26 horas; acima do teto aplicável, deixa a entrada aberta e a saída órfã. Pausa reconhecida antes desse ramo não fecha o turno. [app/ponto/turnos.py:66](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:66) [app/ponto/turnos.py:1421](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1421) [app/ponto/turnos.py:1440](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1440) [app/ponto/motor_calculo_v2.py:1931](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/motor_calculo_v2.py:1931)

Depois do laço, dois passos ainda podem fechar uma aberta: com a saída órfã seguinte antes da próxima entrada, ou com a última `S` absorvida como pausa quando existe batida posterior que prova que o dia seguiu. Ambos usam o teto do dia previsto + 4 horas, ou **14 horas** sem bordas utilizáveis. Há ainda uma exceção posicional que pode tomar uma batida inicialmente aberta como entrada: exige pausa fechada anterior, duas bordas no DNA e candidata mais próxima do `hf` do turno do que do `hi` concorrente, entre outras guardas. [app/ponto/turnos.py:1029](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1029) [app/ponto/turnos.py:1083](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1083) [app/ponto/turnos.py:1165](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1165) [app/ponto/turnos.py:991](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:991)

**O que não vem do cadastro nesta decisão.** Não é obrigatório haver escala ou marco para formar turnos: o segmento sem dono é pareado sem marcos, e, sem papel da ata, o laço recorre ao `tipo` gravado. A sequência e os instantes reais, `is_intervalo`, a existência de batida posterior, o reconhecimento de eco de sincronização e a recusa de par de até 60 segundos vêm das batidas ou de constantes do código. Assim, a escala não fixa por si só o instante efetivo de fechamento. [app/ponto/turnos.py:1956](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1956) [app/ponto/turnos.py:1377](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1377) [app/ponto/turnos.py:344](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:344) [app/ponto/turnos.py:117](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:117) [app/ponto/management/commands/detectar_par_relampago.py:49](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/management/commands/detectar_par_relampago.py:49)

No caminho examinado, não há consulta a `eh_dia_trabalho`, à fase `data_ancora_colaborador` ou a folgas/feriados para decidir o pareamento; isso não elimina o efeito indireto da célula já materializada, de sua geradora, DNA e ata. A afirmação é limitada à cadeia descrita acima. [app/ponto/turnos.py:1923](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1923) [app/ponto/turnos.py:1192](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1192) [app/ponto/juiz_batida.py:149](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/juiz_batida.py:149)

## 19/11 13:53 · sessao 4787df3b5471 · ESCALA-NAO-DESCREVE · sonda · origem=Code

```python
"""C8 sonda34f: terceira fonte = turnos fechados do juiz; somente leitura."""
import datetime
import re
from collections import Counter

from django.db import connection
from django.utils import timezone

import escala.detector_proposta as DP
from colaboradores.models import Colaborador
from escala.alimentacao import q_vinculo_cobre_a_data, vinculo_do_dia
from escala.models import EscalaColaborador, TipoEscala
from escala.servico_jornada import turno_cruza_meia_noite
from ponto.calculador.nucleo import minutos_do_oraculo
from ponto.services.esmeril_espelho import anomalias_da_frota
from ponto.turnos import batidas_apuraveis, turnos_do_colab

ALVOS = [
    85, 110, 111, 121, 134, 219, 253, 272, 282, 290,
    340, 344, 416, 418, 467, 496, 502, 511, 612, 841,
    854, 877, 888, 898, 900, 926, 932, 935, 993, 915,
    146, 220, 489, 934,
]

ERROS = 0
CONTADOS = {"34": 0, "233": 0}
UNIVERSO = None
ORIGINAL = DP.medir_padrao


def erro(etapa, exc, cid=None):
    global ERROS
    ERROS += 1
    tb = exc.__traceback__
    while tb is not None and tb.tb_next is not None:
        tb = tb.tb_next
    print(
        "ERRO etapa=%s colab_pk=%s tipo=%s linha=%s"
        % (
            etapa,
            cid if isinstance(cid, int) else "?",
            re.sub(r"[^A-Za-z0-9_]", "", type(exc).__name__),
            tb.tb_lineno if tb else "?",
        )
    )


def somente_select(execute, sql, params, many, context):
    if not sql.lstrip().upper().startswith("SELECT "):
        raise RuntimeError("SQL_NAO_SELECT_BLOQUEADO")
    return execute(sql, params, many, context)


def agregar(por_dia):
    """Mesmo agregador da 34e: moda de E/S e marcos; mediana da dispersao."""
    dias = sorted(por_dia)
    if not dias:
        return {
            "dias": 0,
            "marcos_moda": None,
            "entrada": None,
            "saida": None,
            "duracao_h": None,
            "dispersao_entrada_min": None,
            "dows": {},
            "padrao_dias": "sem dados",
        }

    marcos = Counter(len(por_dia[d]) for d in dias)
    ent = Counter(por_dia[d][0].strftime("%H:%M") for d in dias)
    sai = Counter(por_dia[d][-1].strftime("%H:%M") for d in dias)
    ent_modal = ent.most_common(1)[0][0]
    em = DP._min_de(ent_modal)
    disp = sorted(
        DP._dist(DP._min_de(por_dia[d][0].strftime("%H:%M")), em)
        for d in dias
    )
    dur = [
        (por_dia[d][-1] - por_dia[d][0]).total_seconds() / 3600.0
        for d in dias
        if len(por_dia[d]) > 1
    ]
    gaps = Counter((dias[i + 1] - dias[i]).days for i in range(len(dias) - 1))
    if (
        gaps
        and gaps.most_common(1)[0][0] == 2
        and gaps.most_common(1)[0][1] >= len(dias) * 0.6
    ):
        padrao = "alternado"
    elif len(dias) >= 5:
        padrao = "semanal"
    else:
        padrao = "indefinido"
    return {
        "dias": len(dias),
        "marcos_moda": marcos.most_common(1)[0][0],
        "marcos_freq": marcos.most_common(1)[0][1],
        "entrada": ent_modal,
        "saida": sai.most_common(1)[0][0],
        "duracao_h": round(sum(dur) / len(dur), 1) if dur else None,
        "dispersao_entrada_min": disp[len(disp) // 2] if disp else None,
        "dows": dict(sorted(Counter(d.weekday() for d in dias).items())),
        "padrao_dias": padrao,
    }


def medir_pelo_nucleo(colab, ini, fim):
    de = timezone.make_aware(
        datetime.datetime.combine(ini, datetime.time.min)
    ) - datetime.timedelta(hours=12)
    ate = timezone.make_aware(
        datetime.datetime.combine(fim, datetime.time.max)
    ) + datetime.timedelta(hours=14)
    ts = [timezone.localtime(b.timestamp) for b in batidas_apuraveis(colab, de, ate)]
    v = minutos_do_oraculo(ts)
    chaves = sorted(k for k in v.horas if ini.isoformat() <= k <= fim.isoformat())
    dias_k = [k for k in chaves if k not in v.impares]
    dias = [datetime.date.fromisoformat(k) for k in dias_k]
    if not dias:
        return {
            "dias": 0,
            "marcos_moda": None,
            "entrada": None,
            "saida": None,
            "duracao_h": None,
            "dispersao_entrada_min": None,
            "dows": {},
            "padrao_dias": "sem dados",
        }
    h = {k: v.horas[k] for k in dias_k}
    marcos = Counter(len(h[k]) for k in dias_k)
    ent = Counter(h[k][0] for k in dias_k)
    sai = Counter(h[k][-1] for k in dias_k)
    ent_modal = ent.most_common(1)[0][0]
    em = DP._min_de(ent_modal)
    disp = sorted(DP._dist(DP._min_de(h[k][0]), em) for k in dias_k)
    dur = [v.envelope[k] / 60.0 for k in dias_k if len(h[k]) > 1]
    gaps = Counter((dias[i + 1] - dias[i]).days for i in range(len(dias) - 1))
    if (
        gaps
        and gaps.most_common(1)[0][0] == 2
        and gaps.most_common(1)[0][1] >= len(dias) * 0.6
    ):
        padrao = "alternado"
    elif len(dias) >= 5:
        padrao = "semanal"
    else:
        padrao = "indefinido"
    return {
        "dias": len(dias),
        "marcos_moda": marcos.most_common(1)[0][0],
        "marcos_freq": marcos.most_common(1)[0][1],
        "entrada": ent_modal,
        "saida": sai.most_common(1)[0][0],
        "duracao_h": round(sum(dur) / len(dur), 1) if dur else None,
        "dispersao_entrada_min": disp[len(disp) // 2] if disp else None,
        "dows": dict(sorted(Counter(d.weekday() for d in dias).items())),
        "padrao_dias": padrao,
    }


def medir_pelo_juiz(colab, ini, fim):
    # Uma observacao por data_turno: une turnos fechados da mesma data,
    # deduplicando batidas pelo pk e preservando a ordem temporal.
    por_dia = {}
    for turno in turnos_do_colab(colab, ini, fim):
        if turno.aberto is not False:
            continue
        if turno.data_turno is None or not ini <= turno.data_turno <= fim:
            continue
        if turno.entrada is None or turno.saida is None:
            continue
        dia = por_dia.setdefault(turno.data_turno, {})
        for batida in turno.batidas:
            if batida.pk is None:
                raise ValueError("BATIDA_SEM_PK")
            dia[batida.pk] = timezone.localtime(batida.timestamp)
    por_dia = {
        d: sorted(batidas.values())
        for d, batidas in por_dia.items()
        if batidas
    }
    return agregar(por_dia)


def proposta_com(medidor, colab, ini, fim):
    DP.medir_padrao = medidor
    try:
        return DP.propor_escala(colab, ini, fim)
    finally:
        DP.medir_padrao = ORIGINAL


def tipo_da_proposta(resultado):
    proposta = resultado["proposta"]
    if proposta is None:
        return None
    return TipoEscala.objects.get(pk=proposta["tipo_escala_id"])


def vinc(cid, hoje):
    candidatos = list(
        EscalaColaborador.objects.filter(
            q_vinculo_cobre_a_data(hoje), colaborador_id=cid
        )
        .select_related("tipo_escala")
        .order_by("pk")
    )
    ec = vinculo_do_dia(cid, hoje, candidatos)
    if ec is None:
        raise ValueError("SEM_VINCULO")
    return ec


def marcos(te, dia):
    hi, hf, ii, iff = te.marcos_do_dia(dia)
    f = lambda t: t.strftime("%H:%M") if t else "-"
    return "%s-%s/%s-%s" % (f(hi), f(hf), f(ii), f(iff))


def cruza(te, dia):
    hi, hf, _, _ = te.marcos_do_dia(dia)
    if hi is None or hf is None:
        raise ValueError("MARCOS_AUSENTES")
    return int(turno_cruza_meia_noite(hi, hf))


def igual(a, b, dia):
    return (
        a is not None
        and b is not None
        and (a.pk == b.pk or marcos(a, dia) == marcos(b, dia))
    )


def descrever(te, dia):
    if te is None:
        return "pk=NONE marcos=NONE cruza=-"
    return "pk=%d marcos=%s cruza=%d" % (te.pk, marcos(te, dia), cruza(te, dia))


def medida(m):
    return "%s/%s marcos=%s dias=%s" % (
        m["entrada"], m["saida"], m["marcos_moda"], m["dias"]
    )


def analisar(cid, hoje, ini, fim, velhas):
    ec = vinc(cid, hoje)
    colab = Colaborador.objects.get(pk=cid)
    cad = ec.tipo_escala
    velha = TipoEscala.objects.get(pk=velhas[cid]["tipo_escala_id"])
    civil = ORIGINAL(colab, ini, fim)
    rn = proposta_com(medir_pelo_nucleo, colab, ini, fim)
    rj = proposta_com(medir_pelo_juiz, colab, ini, fim)
    nova = tipo_da_proposta(rn)
    juiz = tipo_da_proposta(rj)

    if igual(juiz, cad, hoje):
        classe = "juiz==cadastro"
    elif igual(juiz, nova, hoje):
        classe = "juiz==nova"
    elif igual(juiz, velha, hoje):
        classe = "juiz==velha"
    else:
        classe = "nenhuma"

    return ec, cad, velha, nova, juiz, civil, rn["padrao"], rj["padrao"], classe


def imprimir_linha(grupo, cid, dados, hoje):
    ec, cad, velha, nova, juiz, civil, nucleo, med_juiz, classe = dados
    print(
        "%s vinculo_pk=%d colab_pk=%d cadastro{%s} velha{%s} nova{%s} "
        "juiz{%s} civil{%s} nucleo{%s} juiz_medida{%s} classe=%s"
        % (
            grupo, ec.pk, cid,
            descrever(cad, hoje), descrever(velha, hoje),
            descrever(nova, hoje), descrever(juiz, hoje),
            medida(civil), medida(nucleo), medida(med_juiz), classe,
        )
    )


def executar():
    global UNIVERSO
    if getattr(connection, "schema_name", None) != "juliani":
        raise RuntimeError("SCHEMA_NAO_E_JULIANI")
    hoje = timezone.localtime(timezone.now()).date()
    ini = hoje - datetime.timedelta(days=60)
    fim = hoje - datetime.timedelta(days=1)
    print("HOJE=%s INI=%s FIM=%s" % (hoje, ini, fim))

    DP.medir_padrao = ORIGINAL
    det = anomalias_da_frota(hoje)
    velhas = {int(k): p for k, p in det["proposta"].items()}
    UNIVERSO = len(velhas)
    print("UNIVERSO_PROPOSTAS_VELHAS=%d" % UNIVERSO)
    if UNIVERSO != 233:
        erro("universo", ValueError("UNIVERSO_DIFERENTE_DE_233"))
    if len(ALVOS) != 34 or len(set(ALVOS)) != 34:
        erro("alvos", ValueError("ALVOS_DIFERENTES_DE_34"))

    # Cada matriz cruza dois indicadores independentes de igualdade ao cadastro.
    pares = (("velha", "nova"), ("velha", "juiz"), ("nova", "juiz"))
    matrizes = {par: Counter() for par in pares}
    classes = Counter()

    for grupo, ids in (("34", ALVOS), ("233", sorted(velhas))):
        print("== GRUPO=%s" % grupo)
        for cid in ids:
            try:
                dados = analisar(cid, hoje, ini, fim, velhas)
                imprimir_linha(grupo, cid, dados, hoje)
                if grupo == "233":
                    _, cad, velha, nova, juiz, _, _, _, classe = dados
                    tipos = {"velha": velha, "nova": nova, "juiz": juiz}
                    classes[classe] += 1
                    for a, b in pares:
                        if tipos[a] is None or tipos[b] is None:
                            matrizes[(a, b)]["sem_proposta"] += 1
                        else:
                            chave = (int(igual(tipos[a], cad, hoje)),
                                     int(igual(tipos[b], cad, hoje)))
                            matrizes[(a, b)][chave] += 1
                CONTADOS[grupo] += 1
            except Exception as exc:
                DP.medir_padrao = ORIGINAL
                erro("grupo_" + grupo, exc, cid)

    print("CLASSES_233 %s" % " ".join(
        "%s=%d" % (k, classes[k])
        for k in ("juiz==cadastro", "juiz==nova", "juiz==velha", "nenhuma")
    ))
    for a, b in pares:
        c = matrizes[(a, b)]
        print(
            "2X2_233 %s_x_cadastro %s_x_cadastro "
            "sim_sim=%d sim_nao=%d nao_sim=%d nao_nao=%d sem_proposta=%d"
            % (a, b, c[(1, 1)], c[(1, 0)], c[(0, 1)], c[(0, 0)],
               c["sem_proposta"])
        )

    if CONTADOS["34"] != 34:
        erro("contados_34", ValueError("CONTAGEM_DIFERENTE_DE_34"))
    if CONTADOS["233"] != 233:
        erro("contados_233", ValueError("CONTAGEM_DIFERENTE_DE_233"))


try:
    with connection.execute_wrapper(somente_select):
        executar()
except Exception as exc:
    erro("execucao", exc)
finally:
    DP.medir_padrao = ORIGINAL
    print(
        "CONTADOS_34=%d CONTADOS_34==34=%s "
        "CONTADOS_233=%d CONTADOS_233==233=%s UNIVERSO=%s ERROS=%d"
        % (
            CONTADOS["34"], CONTADOS["34"] == 34,
            CONTADOS["233"], CONTADOS["233"] == 233,
            UNIVERSO, ERROS,
        )
    )

if ERROS:
    raise SystemExit(1)
```

A janela e os 34 alvos seguem o anexo `sonda34e.py`; linhas do anexo [não verificado].
O universo vem de `anomalias_da_frota()['proposta']`, como na 34e; [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/esmeril_espelho.py:241).
A fonte nova agrupa batidas de turnos fechados por `data_turno`; empate de moda segue `Counter.most_common`, como o agregador em [app/escala/detector_proposta.py:70](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/detector_proposta.py:70).
A proposta usa tolerância de 30 minutos; a classe compara PK ou marcos exatamente, como na 34e; [app/escala/detector_proposta.py:34](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/detector_proposta.py:34), linhas do anexo [não verificado].
A chamada usada é `turnos_do_colab(colab, ini, fim)`, com `celulas=None` por padrão: [app/ponto/turnos.py:1850](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/turnos.py:1850).

## 19/11 13:52 · sessao dfea873bfe81 · - · - · origem=Ronald (tmux)

- **Regra `!`:** [CLAUDE.md:265](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:265) inclui dado de escala ou vínculo, inclusive retroativo, e o gravado de competência exportada. A chave proposta para propagação é campo de `EscalaColaborador` ([models.py:871](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/models.py:871), [models.py:896](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/models.py:896)); se o ato futuro do admin se enquadra na exigência de `!`, **não decido**.

- **Contador:** o molde existente usa vínculo que cobre a data, colaborador ativo, chave verdadeira e ciclos 5x2/6x1 ([models.py:919](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/models.py:919)). O predicado da sonda anexada usa `CICLOS_FERIADO_VIRA_FOLGA` e não explicita filtro de colaborador ativo [anexo fornecido; não verificado em arquivo].

- **Sugestões:** `sugestoes()` chama `presenca_em_feriado()` a cada execução e exclui postos já marcados ([feriado_do_posto.py:74](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/feriado_do_posto.py:74), [feriado_do_posto.py:136](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/feriado_do_posto.py:136)). A proposta de sugestão “lida, nunca recalculada” requer esclarecer a fonte do resultado armazenado [não verificado].

- **Tela:** `form_posto` exige `editar_postos` ([views.py:732](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/views.py:732)); seu dicionário de dados não inclui a marca ([views.py:851](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/views.py:851)).

- **Motor:** a dobra consulta `feriado_em_dobra` e `eh_feriado(dia)` ([motor_calculo_v2.py:914](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/motor_calculo_v2.py:914)); o padrão deriva do ciclo ([motor_calculo_v2.py:3693](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/motor_calculo_v2.py:3693)). Um ramo escreve minutos HE 100 quando a função retorna verdadeiro ([motor_calculo_v2.py:2809](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/motor_calculo_v2.py:2809)); o efeito nos totais dos J é [não verificado].

- **Precedência:** `SO_EM_DIA_DE_JORNADA` contém somente `falta` ([precedencia.py:498](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/precedencia.py:498)); o veto consulta `eh_dia_trabalho` ([precedencia.py:536](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/precedencia.py:536)).

- **DIA-CONVOCADO:** consta como complemento futuro, com ato individual por `editar_dia` ([BACKLOG.md:2280](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/BACKLOG.md:2280)); consta registrado sem construção ([RELATO.md:11](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/RELATO.md:11)). Os literais `CANCELA-UI-TRABALHA-EM-FERIADO-1130` e `FILA-PARALELA-1150` não foram localizados na busca da cópia; vigência fora do anexo [não verificado].

## 19/11 13:52 · sessao c318157a9add · - · - · origem=Ronald (tmux)

A porta de vínculo é idempotente para o mesmo valor, mas não garante uma linha de trilha por vínculo alterado. A regeneração preserva células editadas e barra dias lavrados; ela também pode acionar escritores de fechamento e de `DiaPago`. Os detalhes e as referências foram enviados ao agente principal. ([vinculo.py:869](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/vinculo.py:869), [celula.py:284](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/portas/celula.py:284), [fechamento.py:1199](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/fechamento.py:1199))

## 19/11 13:52 · sessao ecce8e4875e4 · - · - · origem=Ronald (tmux)

Verificação das premissas enviada ao agente principal, com referências de arquivo e linha.

## 19/11 13:34 · sessao 38e1a6886686 · CADEIA-20 · leitura · origem=Code

**Conclusão:** há caminhos alterados que podem mudar valores de fechamento e `DiaPago`; o efeito líquido em produção não foi medido. O recalculador também pode alcançar exportadas, sujeito à guarda específica da competência. [app/ponto/services/fechamento.py:540] [app/ponto/services/fechamento.py:1047] [não verificado]

Na tabela, **sim** significa que o diff muda um valor ou condição no caminho financeiro, não que houve diferença medida em produção. **Ambas** indica alcance de execução em competência aberta e exportada; não comprova escrita ocorrida. [não verificado]

| Arquivo | Função mudada | Muda valor que chega a dinheiro? | Caminho e competência alcançada |
|---|---|---|---|
| `colaboradores/services/calendario.py` | `contexto_calendario` [app/colaboradores/services/calendario.py:159] | Não: compõe status e palavra para a tela. [app/colaboradores/services/calendario.py:604] [app/colaboradores/services/calendario.py:710] | **Nenhuma**: não achei caminho desse retorno até fechamento ou TXT. [não verificado] |
| `escala/services/leitor_celula.py` | `grade_da_celula` [app/escala/services/leitor_celula.py:350] | Sim: transporta `ausencia_tipo` da ata, consumido nos mapas de minutos. [app/escala/services/leitor_celula.py:435] [app/ponto/services/dia_pago.py:390] | **Ambas**: `grade_do_fechamento` lê célula/ata gravada; os mapas são recalculados para fechamento ou retrato da exportada. [app/folha/export.py:329] [app/ponto/services/dia_pago.py:466] |
| `escala/utils.py` | `_marcar_ausencias` [app/escala/utils.py:1156] | Sim: muda rótulo e pode recusar falta de faixa; a cobertura é levada à ata. [app/escala/utils.py:1204] [app/escala/utils.py:1233] [app/ponto/services/cartorio.py:770] | **Ambas, condicional à relavratura da ata**: cartório → célula/ata → grade → mapas → fechamento/`DiaPago`; ata antiga continua fotografia até fato novo. [app/ponto/services/cartorio.py:142] [app/folha/export.py:329] [app/ponto/services/fechamento.py:1047] |
| `folha/export.py` | `prontidao` [app/folha/export.py:1025] | Não: altera percentual de prontidão, não os valores gravados de folha. [app/folha/export.py:1095] [app/folha/export.py:1110] | **Nenhuma para dinheiro**: com snapshot lê fechamento; sem snapshot deriva a grade para o indicador. [app/folha/export.py:1045] [app/folha/export.py:1095] |
| `ponto/catalogo/ausencias.py` | `_do_banco`, `invalidar_cache`, `vigente`, `efeito_vigente` [app/ponto/catalogo/ausencias.py:240] [app/ponto/catalogo/ausencias.py:262] [app/ponto/catalogo/ausencias.py:269] [app/ponto/catalogo/ausencias.py:298] | Sim: efeito de tipo inativo pode passar a ser lido. [app/ponto/catalogo/ausencias.py:280] [app/ponto/catalogo/ausencias.py:298] | **Ambas**: `Ausencia.efeito` → datas justificadas/suprimidas → motor → fechamento/`DiaPago`; recalcula, não altera por si o gravado. [app/ponto/models.py:841] [app/ponto/services/fechamento.py:694] [app/ponto/services/fechamento.py:1205] |
| `ponto/services/ausencia.py` | `gravar_tipo_ausencia` [app/ponto/services/ausencia.py:1302] | Não sei: barra criação/ativação sem faixa e alteração de código, interferindo em cadastro futuro. [app/ponto/services/ausencia.py:1306] | **Ambas, indiretamente**: cadastro → efeito da ausência → recálculo; diferença financeira efetiva depende dos dados e atos. [app/ponto/services/ausencia.py:1313] [app/ponto/services/fechamento.py:694] [não verificado] |
| `ponto/services/cartorio.py` | `ata_do_dia`, `_julgar_colab_corpo`, `contadores` [app/ponto/services/cartorio.py:731] [app/ponto/services/cartorio.py:1135] [app/ponto/services/cartorio.py:1964] | Sim: transporta cobertura na ata e muda o teste de cobertura integral. [app/ponto/services/cartorio.py:770] [app/ponto/services/cartorio.py:1251] | **Ambas, se houver relavratura**: ata → leitor de célula → mapas → fechamento/`DiaPago`. Relavrar não regenera célula. [app/escala/services/leitor_celula.py:435] [app/ponto/services/dia_pago.py:390] [app/ponto/services/cartorio.py:958] |
| `ponto/services/dia_decidido.py` | `veredito_do_dia` [app/ponto/services/dia_decidido.py:323] | Não: muda o veredito de apresentação para `folga_da_escala`. [app/ponto/services/dia_decidido.py:371] | **Nenhuma**: caminho financeiro a partir desse retorno [não verificado]. |
| `ponto/services/dia_do_colab.py` | `dia_do_colab` [app/ponto/services/dia_do_colab.py:6] | Não: devolve recusa no diagnóstico do dia. [app/ponto/services/dia_do_colab.py:15] [app/ponto/services/dia_do_colab.py:108] | **Nenhuma**: caminho financeiro a partir desse retorno [não verificado]. |
| `ponto/services/dia_pago.py` | `por_dia_da_grade` [app/ponto/services/dia_pago.py:364] | Sim: o predicado passa a incluir `ausencia_tipo`, produzindo mapas de abono, previsto e realizado. [app/ponto/services/dia_pago.py:390] | **Ambas**: fechamento recalcula os mapas; `retratar_exportada` lê fechamento e células/atas gravados, deriva os mapas e escreve os três minutos em `DiaPago`. [app/ponto/services/fechamento.py:1047] [app/ponto/services/dia_pago.py:466] |
| `ponto/services/espelho.py` | `aplicar_palavra_do_dia`, `montar_dias` [app/ponto/services/espelho.py:297] [app/ponto/services/espelho.py:368] | Não: transporta e apresenta recusa. [app/ponto/services/espelho.py:337] [app/ponto/services/espelho.py:645] | **Nenhuma**: caminho financeiro desses campos privados [não verificado]. |
| `ponto/services/fechamento.py` | `recalcular_fechamento_mes` [app/ponto/services/fechamento.py:449] | Sim: altera seleção de dias previstos e propagação de `TipoSemFaixa`. [app/ponto/services/fechamento.py:1047] [app/ponto/services/fechamento.py:1089] | **Ambas, salvo guarda específica**: recalcula e pode gravar `FechamentoMensal` e lavrar `DiaPago`, inclusive em exportada não barrada. [app/ponto/services/fechamento.py:540] [app/ponto/services/fechamento.py:561] [app/ponto/services/fechamento.py:1165] [app/ponto/services/fechamento.py:1205] |
| `ponto/status_dia.py` | `status_dos_dias_lavrados` [app/ponto/status_dia.py:221] | Não sei: retorna `sem_certidao` para recusa; efeito financeiro indireto não foi estabelecido. [app/ponto/status_dia.py:318] [não verificado] | **Nenhuma comprovada**: caminho até dinheiro [não verificado]. |
| `relatorios/pdf_espelho.py` | `_coletar_dados_espelho` [app/relatorios/pdf_espelho.py:187] | Não: passa a obter órfãs do espelho para o documento. [app/relatorios/pdf_espelho.py:471] | **Nenhuma para dinheiro**: saída é coleta do PDF; caminho de escrita financeira [não verificado]. |

1. **Leitores e escritas.** `retratar_exportada` lê o `FechamentoMensal` gravado, mas deriva os três mapas das células/atas e pode escrever `DiaPago`; portanto, não é simplesmente uma cópia dos três totais mensais. [app/ponto/services/dia_pago.py:466] [app/ponto/services/dia_pago.py:469] [app/ponto/services/dia_pago.py:486] `prontidao` lê o snapshot gravado quando disponível. [app/folha/export.py:1045] `montar_linhas_export` lê normalmente fechamentos gravados, mas também consulta ausências para faltas; `competencia_entregue` lê o registro vigente de `ExportacaoDominio`. [app/folha/export.py:717] [app/folha/export.py:740] [app/folha/export.py:588] **Sim**, o caminho alterado do recalculador escreve `FechamentoMensal`; a guarda não é geral para toda exportada. [app/ponto/services/fechamento.py:561] [app/ponto/services/fechamento.py:1165] A relavratura de ata **não regenera** célula; outro caminho de regeneração causado pelos trechos do diff [não verificado]. [app/ponto/services/cartorio.py:958]

2. **POUSO.** Para apply em aberta, a regra exige, antes, reversão em `logs/` com snapshot por colaborador; depois, hash antes/depois provando exportada intacta e prova de frota no mesmo marco. O anexo não enumera os campos do snapshot: para este caminho, é necessário cobrir os campos efetivamente escritos no fechamento e as linhas de `DiaPago` atingidas. [regra_dinheiro.txt, item 1, anexo fornecido] [app/ponto/services/fechamento.py:1165] [app/ponto/services/fechamento.py:1196] Para exportada, a regra exige hash do **`FechamentoMensal`**, DIFF de frota antes e a porta específica; o TXT tem hash em **`ExportacaoDominio.hash_sha256`**, do conteúdo. [regra_dinheiro.txt, itens 2–3, anexo fornecido] [app/folha/models.py:83] [app/folha/models.py:86] A função real do cálculo é `recalcular_fechamento_mes`, com universo de `universo_da_competencia`; prova deve abranger esse universo inteiro, sem filtro de colaboradores. [app/ponto/services/fechamento.py:383] [app/ponto/services/fechamento.py:449] [app/ponto/services/fechamento.py:555] A função ou métrica específica de prova para este pouso [não verificado]. [nao verificado]

3. **Dependência do passo 2:** não achei referência de produção a `razoes_orfas` ou a razão de sobra na busca textual da cópia. Dependência sem esses nomes ou entregue por outro mecanismo [não verificado].

4. **[não verificado]** Hash Git da cópia: o diretório não contém metadados Git; a identificação base/LASTRO e o diff são os fornecidos no enunciado. **[não verificado]** Dados de produção, diferenças por colaborador, hashes antes/depois, efeito líquido e execução de qualquer apply. **[não verificado]** Lista exaustiva de todos os leitores indiretos e eventual dependência não textual do passo 2.

## 19/11 13:12 · sessao b851691f9de6 · PISTA-ESCOLHE-SOZINHA · verificador · origem=Code

**REVISAO: PROCEDE — devolver I1, I2, I3, I4, SELO e PRONTO. PROIBIDO: NAO PROCEDE como item a devolver.** Os principais bloqueios são a prova da árvore efetivamente testada, o protocolo de espera e retorno da pista e a fonte da decisão do verificador. [CLAUDE.md:119](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:119), [CLAUDE.md:320](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:320), [app/docs/RELATO.md:1088](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/RELATO.md:1088).

Os três anexos foram fornecidos no texto, sem arquivos nem numeração de linhas verificáveis; identifico suas afirmações como **[não verificado: anexo raspado sem arquivo:linha]**. A cópia tampouco contém `.git`, portanto o HEAD informado não pôde ser confirmado por Git. **[não verificado]**

- **I1 — PROCEDE.** As cinco linhas estão presentes no contrato fornecido, mas presença não significa que todas sejam mensuráveis. **[não verificado: anexo raspado sem arquivo:linha]** A linha de falha prevê `--dir` sujo, sem definir como detectar alterações rastreadas, arquivos não rastreados e mudanças durante a espera; o `suite.sh` atual monta o diretório e não prova sua igualdade ao commit. [bin/suite.sh:283](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/suite.sh:283), [bin/suite.sh:316](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/suite.sh:316). A medida do alarme também não tem observação histórica definida para a disponibilidade da pista durante a espera; `--quem` registra o chamador da trava local, não o estado da pista remota. [bin/trava_teste.sh:21](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/trava_teste.sh:21), [CLAUDE.md:131](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:131). A reversão `SUITE_PISTA=0` está nomeada, mas sua existência e efeito no código atual são **[não verificado]**.

- **I2 — PROCEDE.** Hoje `_run` executa Docker pela trava local; `volta_intermediaria.sh` chama `suite.sh --volta`; régua e pre-push têm chamadas próprias pela trava, com o pre-push atualmente na linha 142, não 130. [bin/suite.sh:316](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/suite.sh:316), [bin/volta_intermediaria.sh:31](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/volta_intermediaria.sh:31), [bin/regua.sh:165](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/regua.sh:165), [bin/pre-push.sh:142](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/pre-push.sh:142).

  A regra proposta não define operacionalmente “pista livre”, nem resolve a corrida entre observar o estado e pedir o job; `--quem` tampouco prova que a VM não tem suite fora da trava. [CLAUDE.md:131](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:131). O anexo lista `pede`, `espera`, `estado`, `log` e `--only`, e relata a exigência de SHA completo e `refs/pista/<sha>`; ele **não mostra a saída de `espera <id>`**, seu rc, nem que entregue o log Django no formato exigido. **[não verificado: anexo raspado sem arquivo:linha]** Logo, não está demonstrado que `OK/FAILED`, `Ran N` e rc do processo chegam ao chamador conforme L-074. [CLAUDE.md:119](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:119).

  Também falta provar que a árvore do SHA é a cópia certificada e, no pouso, a árvore que pousa. O helper `--igual` existente cobre apenas `.py` e `.html` sob `app/`, e não é chamado por `suite.sh` para essa prova; `staticfiles` é ignorado pelo Git e montado de fora da cópia. [bin/arvore_do_push.sh:85](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/arvore_do_push.sh:85), [.gitignore:9](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/.gitignore:9), [bin/arvore_do_push.sh:50](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/arvore_do_push.sh:50), [CLAUDE.md:320](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:320). Mesma árvore e equivalência dos recursos da pista remota: **[não verificado]**.

- **I3 — PROCEDE.** O registro proposto não define fonte para “pista livre” ao longo de toda a espera, nem como associar uma volta sem `--motivo` a uma obra; a chamada atual da volta não passa esse argumento. [bin/volta_intermediaria.sh:31](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/volta_intermediaria.sh:31). Também faltam critérios de início e fim da fila, cancelamento, gravação concorrente e tratamento de eventos ausentes para que as 24 horas e o alarme sejam reproduzíveis. **[não verificado: anexo raspado sem arquivo:linha]**

- **I4 — PROCEDE.** O aval diz cobrar que a suite inteira espere o verificador com zero bloqueios, mas não define uma prova legível pelo instrumento que vincule resultado, obra e **mesmo hash**, nem como distinguir zero procedentes de rc zero ou de ausência de resultado. **[não verificado: anexo raspado sem arquivo:linha]** O RELATO registra que a identidade da chamada não prova “0 procedente” e que o item A ficou como conduta; uma guarda automática exige fechar essa lacuna e sua abrangência. [app/docs/RELATO.md:1088](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/RELATO.md:1088). O significado e a prova de “cancelado por volta superada” também não estão definidos no dado. **[não verificado: anexo raspado sem arquivo:linha]**

- **SELO — PROCEDE.** Um `ssh` falso pode exercitar os cinco cenários propostos, mas sua simples previsão não demonstra que eles mordem as decisões e os resultados reais: faltam asserções para igualdade da árvore, rc 71 versus vermelho da suite, saída `Ran`/veredito/rc, indisponibilidade durante a espera, corrida de ocupação e retorno à VM com motivo. **[não verificado: anexo raspado sem arquivo:linha]** Além disso, a alteração deve continuar satisfazendo os selos existentes: porta e trava, motivo, fonte única de labels e montagem. [bin/tests/test_suite_sh.sh:56](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/tests/test_suite_sh.sh:56), [bin/tests/test_suite_motivo.sh:3](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/tests/test_suite_motivo.sh:3), [bin/tests/test_labels_fonte_unica.sh:20](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/tests/test_labels_fonte_unica.sh:20), [bin/tests/test_montagem_vem_do_arvore_do_push.sh:55](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/tests/test_montagem_vem_do_arvore_do_push.sh:55).

- **PROIBIDO — NAO PROCEDE.** As restrições declaradas preservam régua, pre-push, trava, labels e lado remoto; o instrumento tem pouso próprio. **[não verificado: anexo raspado sem arquivo:linha]** Isso é compatível com a separação vigente do pouso de instrumento e produto. [CLAUDE.md:325](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:325). Não há demonstração de que o banco da pista remota seja distinto do banco da VM, portanto a observância de um run por banco ainda depende dessa prova. [CLAUDE.md:131](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:131); **[não verificado]**. Toque em exportada, dinheiro ou `!` fora do declarado: **[não verificado]**.

- **PRONTO — PROCEDE.** A próxima suite sair automaticamente na pista não comprova, por si, as condições de identidade da árvore, retorno do veredito, contador e guarda do verificador apontadas acima. [CLAUDE.md:119](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:119), [CLAUDE.md:320](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:320). O critério também precisa dizer como tratar pista ocupada ou indisponível, pois o próprio aval admite execução na VM nesses casos. **[não verificado: anexo raspado sem arquivo:linha]**

## 19/11 13:11 · sessao 5716e421dcbf · E4C-F1-B2b · verificador · origem=Code

**AVAL-DEVOLVIDO:** procedem as objeções em B1, B2, B3, B4 e PRONTO. A ordem posterior à F5 está expressamente excepcionada pelo aval das 13:15; a situação real da F5, da C7 e de produção não foi verificada. Os anexos foram fornecidos no enunciado sem numeração de linhas; por isso identifico neles seção e item, com **[linha não verificada]**, sem inventar `arquivo:linha`.

**ORDEM — NÃO PROCEDE.** O aval 1315 excepciona nominalmente o item 1 do aval 1300: F0 passa à frente da C7, após a F5 estar no ar; C7 espera e rebasa. Isso resolve a divergência com a ordem anterior e com o texto histórico do brief que ainda pede F5 e C7 antes da F0. A C7 cruza o arquivo da trilha, cuja âncora atual está em [reconciliador.py:465](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/reconciliador.py:465); os estados “F5 no ar” e “C7 não pousou” são [não verificado]. (Anexos `aval_1315.txt`, ORDEM; `aval_1300.txt`, item 1; `brief_1911.md`, seções 4–5: [linha não verificada].)

**B1 — PROCEDE (devolver).** As âncoras existem nos caminhos pedidos: [fio.py:1381](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1381), [fio.py:1389](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1389), [fio.py:1431](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1431), [fio.py:1488](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1488), [juizes.py:338](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/juizes.py:338), [materializacao.py:1818](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/materializacao.py:1818), [models.py:3299](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/models.py:3299), [models.py:3364](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/models.py:3364), [models.py:3389](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/models.py:3389) e [reconciliador.py:465](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/reconciliador.py:465).

Mas a tabela F6 exige **extrair a leitura e chamá-la pela guarda**; isso colide com “sem ligar em nenhum sítio” e com a proibição literal de chamadas fora de testes em B3. A leitura atual está em [fio.py:1409](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1409); tabela volta 8, FATOS COMUNS F6 e aval 1315, B1/B3: [linha não verificada]. A função de dias pode morar em `fio.py`, sem juiz novo, pois o juiz de dia e o veto já existem em [juizes.py:338](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/juizes.py:338) e [fio.py:1358](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1358).

O teto de três arquivos **comporta os destinos indicados**: dias, coletor e sentinela em `fio.py`; extensão da trilha em `reconciliador.py`; `models.py` é a terceira localização autorizada. Mas acomodação de arquivos não resolve o conflito de ligação; a tabela coloca os pontos F9/F9b dentro dos métodos de `models.py`, enquanto o aval também proíbe ligar instrumentos nesta fatia. (Tabela volta 8, F8–F9b e aval 1315, B1/B4: [linha não verificada]; [models.py:3339](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/models.py:3339), [models.py:3376](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/models.py:3376).)

As linhas de início das âncoras do brief batem com a cópia, mas **nem todas as referências internas batem literalmente**: `values_list` está em 1410, os discriminadores em 1414–1417 e o `continue` em 1418–1419, contra os números resumidos no brief. A identidade Git da cópia como o hash informado é [não verificado]: esta árvore não dispõe de `.git`. ([fio.py:1409](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1409); `brief_1911.md`, seção 1: [linha não verificada].)

**B2 — PROCEDE (devolver).** M2 precisa distinguir **efeito final** de alteração durante a operação: um coletor utilizável tem de ligar estado no `ContextVar` e depois restaurá-lo; o molde existente faz `set` e `reset` em [materializacao.py:1741](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/materializacao.py:1741). Além disso, `registrar_retencao` é uma porta de escrita de auditoria, não uma função que possa ser chamada para registrar uma retenção e simultaneamente nunca alterar `LogAuditoria`: [reconciliador.py:495](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/reconciliador.py:495).

M3 também não fecha como teste de cura de X6 enquanto X6 permanecer desligada: hoje ela ignora o retorno de `carimbar_identidade` e, no caminho não idempotente sem exceção, devolve `True`; já duplicata marcada devolve `False`. Matar o mutante de X6 que engole `RETIDA` exige mudar o tratamento em X6, atribuído à fatia posterior pela tabela. ([fio.py:1493](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1493), [fio.py:1502](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1502); tabela volta 8, X6; `BRIEF-FATIAS.md`, F3: [linha não verificada].)

M1 é um teste de ligação indevida em sítio real; sua execução e capacidade de matar o mutante são [não verificado]. M4 descreve a comparação desejada, mas a extensão da porta de trilha tem o efeito de escrita apontado em M2. (Anexo `brief_1911.md`, seção 3; tabela volta 8, F4: [linha não verificada].) [nao verificado]

**B3 — PROCEDE (devolver).** A guarda é de fato no-op sem cerca: lê `_CERCA` e retorna antes dos imports e da consulta em [fio.py:1402](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1402). Porém, o selo de **zero chamadas aos quatro instrumentos fora de testes** conflita com a chamada da função extraída pela própria guarda, exigida por F6. Também não demonstra que a trilha estendida esteja sem uso: `registrar_retencao` já é chamada em produção, por exemplo em [reconciliador.py:562](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/reconciliador.py:562). Mesmo um AST restrito ao diff pode não contar chamadas antigas que passam a alcançar o corpo modificado. É necessário precisar o que o selo conta e qual comportamento deve permanecer igual. (Aval 1315, B3; tabela volta 8, F6: [linha não verificada].)

**B4 — PROCEDE (devolver).** O teto e a sequência de prova não demonstram, por si, incompatibilidade com o aval 1300; porém B4 exige os quatro mutantes mortos e o selo verde enquanto B2/B3 têm as contradições acima. Portanto, a prova completa não está executável no literal sem esclarecer esses critérios. Resultado de RED, mutantes, suite, verificador, opus, deploy e `cmp`: [não verificado]. (Aval 1315, B2–B4: [linha não verificada]; [fio.py:1502](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1502), [reconciliador.py:495](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/reconciliador.py:495).)

**PROIBIDO — NÃO PROCEDE.** Não identifiquei, no escopo declarado de instrumentos desligados, autorização para alterar gravado de competência exportada, dinheiro, escala/vínculo ou motor; o aval também proíbe juiz novo e fallback. A extração dos dias pode reutilizar o juiz e o veto existentes. Isso depende de resolver as ligações e verificar o diff quando existir; efeitos reais em produção são [não verificado]. (Aval 1315, PROIBIDO: [linha não verificada]; [CLAUDE.md:265](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/CLAUDE.md:265), [fio.py:1358](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1358).)

**PRONTO — PROCEDE (devolver).** “F0 no ar com selo verde” e “0 chamada fora de tests” dependem da definição corrigida de B1–B3; ainda não há evidência fornecida de construção ou colocação no ar. A exigência de aval próprio para F1 está expressa no anexo e permanece condicionada ao término da F0. (Aval 1315, PRONTO: [linha não verificada]; conflitos demonstrados em [fio.py:1409](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/services/fio.py:1409) e [reconciliador.py:562](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/reconciliador.py:562).)

REVISAO: PROCEDE (itens B1, B2, B3, B4, PRONTO)

## 19/11 12:59 · sessao 8d9301ad0fee · CADEIA-20 · verificador · origem=Code

1. **PREMISSAS: NÃO PROCEDE.**

   - **(a)** A nota da célula diz literalmente “PARCIAL até a O168, a O169 e o LASTRO”; ela não menciona os passos 1 e 2 ([contratos_estruturais.py:262](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/core/contratos_estruturais.py:262), [linha 270](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/core/contratos_estruturais.py:270)). O registro de chegada diz “O168, LASTRO; O169 já pousada” ([CORTES.md:469](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/CORTES.md:469)). A exigência de `CSF_RETRATADO` medido em zero consta do anexo `condicao.md`, mas a medição em produção é **[não verificado]**.
   - **(b)** Há contradição direta: o anexo `brief.md`, em “Números”, afirma “intersecção por ARQUIVO ... com o passo 1 ... **0**” e, no item anterior, inclui `escala/utils.py` e `cartorio.py` no auto-merge. Os dois também constam da lista `os41.txt` fornecida e da definição do passo 1 na pergunta. Portanto, pela definição pedida, há **pelo menos dois arquivos em comum**, não zero. Os anexos foram fornecidos no texto, sem arquivos nem linhas verificáveis nesta cópia: **[não verificado: arquivo:linha dos anexos]**.
   - **(c)** A ausência do passo 2 na nota da célula não prova que ele não seja pré-requisito do passo 3. A ordem registrada diz “**um passo só abre com o anterior NO AR**” e enumera passo 2 antes do 3 ([BACKLOG.md:1960](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/BACKLOG.md:1960)). A inexistência de dependência de código é **[não verificado]**.

2. **COLISÃO COM A ORDEM REGISTRADA: PROCEDE.** O registro de 18/11 identifica a cadeia ([PROMPTS.md:2577](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/PROMPTS.md:2577)); o SANDUÍCHE enumera “CADEIA p2 ... CADEIA p3” ([PROMPTS.md:2587](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/PROMPTS.md:2587)). O BACKLOG explicita a obrigatoriedade do anterior no ar e ainda registra “construtor DEPOIS do acessor” para LASTRO ([BACKLOG.md:1960](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/BACKLOG.md:1960), [linha 1965](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/BACKLOG.md:1965)).

   O item 5 fornecido diz que o passo 3 “passa NA FRENTE do passo 2”, de modo que altera expressamente essa ordem. Se isso também revoga a trava específica “DEPOIS do acessor”, e o estado vigente dos avais além do material disponível: **[não verificado]**. O texto integral dos avais apontados pelos caminhos em `PROMPTS.md` não está disponível nesta cópia: **[não verificado]**.

3. **CONDIÇÃO QUE NÃO FECHA NO LITERAL: PROCEDE.** A execução paralela “ao lado do passo 1” é justificada no item 5 por “0 arquivo em comum”, premissa contradita pelos dois arquivos acima. A condição da B2b diz “se cruzar arquivo, a espinha pousa primeiro”; a interseção efetiva com a F0 é **[não verificado]**. Cópia nova do HEAD, quatro conflitos e seus motivos, resultado do verificador, suite posterior, opus e pouso próprio são condições expressas no item 5 fornecido; seu cumprimento é futuro ou não demonstrado: **[não verificado]**. O diretório recebido não contém `.git`, portanto o hash do HEAD e o `merge-tree` alegados não puderam ser conferidos: **[não verificado]**. [nao verificado]

4. **TOQUE FORA DO DECLARADO: NÃO PROCEDE como conclusão provada; [não verificado].** O brief fornecido declara “`export.py` só na prontidão sem snapshot” e “DIFF de frota (comp 11) obrigatório no pouso”; o BACKLOG também declara que os 41 incluem `folha/export.py` e `dia_pago.py`, com DIFF de frota ([BACKLOG.md:1960](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/BACKLOG.md:1960)). O código desta cópia usa `grade_do_fechamento` na prontidão ([export.py:1092](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/folha/export.py:1092)); sem o diff da cópia LASTRO, não se comprova que esse seja o único trecho alterado nem que `motivos_retencao_celula` esteja intocado: **[não verificado]**.

   Nos 41, os seguintes **arquivos da cópia disponível têm referências identificáveis** a dinheiro, folha, exportada ou fechamento; isto é um inventário de arquivos, não prova de quais linhas a mudança LASTRO altera:

   - Produção: [calendario.py:299](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/services/calendario.py:299) “DINHEIRO DO DIA”; [leitor_celula.py:2](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/services/leitor_celula.py:2) `grade_do_fechamento`; [utils.py:37](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/escala/utils.py:37) “PAREAMENTO -- dinheiro”; [export.py:4](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/folha/export.py:4) `FechamentoMensal`; [ausencias.py:9](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/catalogo/ausencias.py:9) “efeito ... na folha”; [ausencia.py:146](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/ausencia.py:146) `FechamentoMensal`; [cartorio.py:235](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/cartorio.py:235) “FechamentoMensal da 09 (exportada)”; [dia_decidido.py:12](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/dia_decidido.py:12) “para a folha”; [dia_do_colab.py:52](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/dia_do_colab.py:52) “definição que a folha usa”; [dia_pago.py:405](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/dia_pago.py:405) `retratar_exportada`; [espelho.py:93](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/espelho.py:93) `FechamentoMensal`; [fechamento.py:448](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/services/fechamento.py:448) `recalcular_fechamento_mes`; [status_dia.py:12](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/status_dia.py:12) `DiaPago`; [pdf_espelho.py:835](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/relatorios/pdf_espelho.py:835) `DiaPago.horas_trabalhadas`.
   - Testes: [test_calendario_ausencia_pela_autoridade.py:4](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:4) “falta para a folha”; [test_medida_turno_do_par.py:186](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/core/tests/test_medida_turno_do_par.py:186) `folha/export.py`; [test_a2_precedencia.py:541](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/tests/test_a2_precedencia.py:541) “folha”; [test_e3_status_lavrado.py:291](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/tests/test_e3_status_lavrado.py:291) `FechamentoMensal` e `ExportacaoDominio`; [test_o219_versao_da_regra.py:181](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/tests/test_o219_versao_da_regra.py:181) `LeitorPuroSemRelogioESemExportada`.

   Os três JSONs e seis testes `test_lastro_*` da lista fornecida não estão nesta cópia; seus sítios são **[não verificado]**. Impacto efetivo em exportada, dinheiro ou necessidade de `!`: **[não verificado]**. [nao verificado]

**REVISAO: PROCEDE (devolver)** — a premissa de zero arquivos comuns é contradita por `escala/utils.py` e `ponto/services/cartorio.py`; há também ordem anterior explícita que segura o passo 3 ([BACKLOG.md:1960](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/BACKLOG.md:1960), [linha 1965](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/BACKLOG.md:1965)); a lista e o auto-merge dos anexos fornecidos têm referência de arquivo:linha **[não verificado]**.

## 19/11 12:59 · sessao c7df8e725a1a · CADEIA-20 · sonda · origem=Code

A leitura literal é o **contador de retratações efetivamente aceitas pelo `--executar`**, não um contador restrito a chamados com conversa: o comando primeiro preserva os barrados por `pode_executar_csf`, depois chama `retratar` e incrementa `CSF_RETRATADO` somente quando a porta retorna verdadeiro. O dry-run conta outro indicador, `CSF_RETRATAVEL_DRY`, e não testa se a porta de retratação aceitaria o ato. A O168 exige o contador medido em zero antes da retirada do `--executar`. [app/ponto/management/commands/supra_juiz.py:260](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/management/commands/supra_juiz.py:260), [app/ponto/management/commands/supra_juiz.py:279](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/management/commands/supra_juiz.py:279), [app/docs/BACKLOG.md:1544](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/docs/BACKLOG.md:1544).

A frase também pode motivar duas perguntas distintas:

- **Histórico:** a implementação atual grava ocorrência com texto que identifica o supra em `FollowupChamado.conteudo`; `tipo='retratado'` é passado à porta da nota. Isso permite procurar essa assinatura, mas não demonstra, por si só, que havia resposta sem veredito **no instante da retratação**, nem que a assinatura cobre todas as versões históricas. Portanto, o histórico de “retratados pelo supra com conversa naquele instante” fica **[não verificado]**. [app/ponto/supra_juiz.py:122](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/supra_juiz.py:122), [app/chamados/reconciliador.py:583](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/reconciliador.py:583), [app/chamados/models.py:1805](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/chamados/models.py:1805).
- **Hoje:** executar o comando real na sombra e reverter a transação mede o que ele fez *nesse ensaio*, inclusive o contador literal e as recusas da guarda. A guarda pergunta por respostas sem veredito nas perguntas do pai e nas que resolvem o chamado; “conversa” aqui tem esse significado específico. O cron de `--executar` usa a competência corrente por empresa. [app/ponto/supra_juiz.py:561](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/supra_juiz.py:561), [app/config/crons.py:599](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/config/crons.py:599), [app/ponto/management/commands/supra_juiz.py:85](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/management/commands/supra_juiz.py:85).

**Nenhum resultado de produção foi fornecido:** os valores permanecem **[não verificado]**. A sonda abaixo não foi executada. Ela declara o universo efetivo do comando, que exclui colaboradores sem batida na janela ampliada, e conta separadamente as ocorrências históricas com a assinatura conhecida. A igualdade de contadores é verificada; qualquer falha invalida a medida. [app/ponto/management/commands/supra_juiz.py:102](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/management/commands/supra_juiz.py:102), [app/ponto/management/commands/supra_juiz.py:110](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/management/commands/supra_juiz.py:110), [app/ponto/management/commands/supra_juiz.py:205](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/app/ponto/management/commands/supra_juiz.py:205).

A sonda deve ser entregue à sessão para `bin/sombra.sh --rodar`; não criei arquivo. O próprio invólucro usa o tenant `juliani` e os settings da sombra. A prova de rollback abaixo verifica uma escrita sentinela e as linhas observadas, enquanto a transação reverte o conjunto das escritas no banco. [bin/sombra.sh:182](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/sombra.sh:182), [bin/sombra.sh:483](/home/ronald/codex_ws/688696dbb32b7859fb38a141c54d5a6f456056b3/bin/sombra.sh:483).

```python
import contextlib
import io
import logging
import os
import re
import sys
import traceback
from collections import Counter
from unittest.mock import patch

from django.conf import settings
from django.core.management import call_command
from django.db import connection, transaction
from django.utils import timezone

from chamados.catalogo.modulos import data_do_chamado
from chamados.models import ChamadoColaborador, FollowupChamado
from ponto.janelas import janela_atual
from ponto.management.commands.e6_oraculo import InsumosDoDono
import chamados.reconciliador as reconciliador
import ponto.supra_juiz as supra


EMPRESAS = (2, 3, 4)
ASSINATURA = "Cobranca retratada pelo supra-juiz (ultima instancia)."
DONOS = {"ESTRUTURA", "CADASTRO", "BATIDA"}
ERROS = 0


class DesfazerEnsaio(Exception):
    pass


def erro():
    """Traceback sem valores de exceção, argumentos ou linhas de código."""
    global ERROS
    ERROS += 1
    _, exc, tb = sys.exc_info()
    print("Traceback (mais recente por ultimo):")
    for frame in traceback.extract_tb(tb):
        print("  arquivo=%s linha=%s funcao=%s" % (
            os.path.basename(frame.filename), frame.lineno, frame.name
        ))
    print("  excecao=%s" % type(exc).__name__)


def exigir(condicao):
    if not condicao:
        raise RuntimeError("verificacao falhou")


def data_chamado(ch):
    valor = data_do_chamado(ch.modulo_origem, ch.contexto_json)
    if valor is None:
        return None
    if hasattr(valor, "date") and not hasattr(valor, "day"):
        valor = valor.date()
    if isinstance(valor, str):
        from datetime import date
        return date.fromisoformat(valor[:10])
    return valor


def dono_do_caso(ch, dia, cache):
    if dia is None:
        return "[nao verificado]"
    try:
        # O carregador chama o juiz real e, quando necessário, o motor.
        # A janela da competência é obtida pela autoridade existente.
        chave = (ch.colaborador_id, dia.year, dia.month)
        if chave not in cache:
            # Usa a janela do próprio cron quando cobre o dia; para outras
            # datas históricas, não inventa uma competência.
            ini, fim = janela_atual(timezone.localdate(), ch.colaborador.empresa_id)
            if not ini <= dia <= fim:
                cache[chave] = None
            else:
                agora = timezone.now()
                cache[chave] = InsumosDoDono(
                    ch.colaborador, ini, fim, timezone.localdate(), agora=agora
                )
        insumos = cache[chave]
        if insumos is None:
            return "[nao verificado]"
        dono = insumos.dono(dia)
        return dono if dono in DONOS else "[nao verificado]"
    except Exception:
        erro()
        return "[nao verificado]"


def linha_caso(rotulo, empresa, ch, cache):
    try:
        dia = data_chamado(ch)
        dono = dono_do_caso(ch, dia, cache)
        print(
            "%s empresa=%s chamado=%s colab=%s data=%s dono=%s"
            % (
                rotulo, empresa, ch.pk, ch.colaborador_id,
                dia.isoformat() if dia else "[nao verificado]", dono,
            )
        )
    except Exception:
        erro()
        print(
            "%s empresa=%s chamado=%s colab=%s data=[nao verificado] "
            "dono=[nao verificado]"
            % (rotulo, empresa, ch.pk, ch.colaborador_id)
        )


def placar_do_comando(texto):
    linhas = texto.splitlines()
    avaliados = None
    placar = {}
    no_placar = False
    for linha in linhas:
        m = re.fullmatch(r"dias-colab avaliados: (\d+)", linha.strip())
        if m:
            exigir(avaliados is None)
            avaliados = int(m.group(1))
        if linha.strip() == "--- PLACAR POR CONTRADICAO ---":
            no_placar = True
            continue
        if no_placar and linha.startswith("---"):
            no_placar = False
        if no_placar:
            m = re.fullmatch(r"\s*([A-Z0-9_]+)\s+(\d+)\s*", linha)
            exigir(m is not None)
            exigir(m.group(1) not in placar)
            placar[m.group(1)] = int(m.group(2))
    exigir(avaliados is not None)
    exigir("--- PLACAR POR CONTRADICAO ---" in texto)
    return avaliados, placar


def validar_ambiente():
    exigir(os.environ.get("DJANGO_SETTINGS_MODULE") == "config.settings.sombra")
    exigir(getattr(settings, "HASNER_SOMBRA", False))
    exigir(connection.settings_dict["NAME"] == "sombra")
    exigir(connection.settings_dict["HOST"] == "sombra_db")
    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        banco, schema = cursor.fetchone()
    exigir(banco == "sombra" and schema == "juliani")
    exigir(not connection.in_atomic_block)
    exigir(connection.get_autocommit())


def historico_assinatura(empresa):
    # Universo: ocorrências de sistema com a assinatura conhecida, em toda
    # a história disponível da empresa. Conta ocorrências e chamados distintos.
    # Não afirma existência de conversa no momento histórico.
    qs = FollowupChamado.objects.filter(
        chamado__colaborador__empresa_id=empresa,
        autor_tipo="sistema",
        conteudo__contains=ASSINATURA,
    ).select_related("chamado__colaborador").order_by("pk")

    universo = qs.count()
    percorridos = 0
    chamados = {}
    cache = {}
    for nota in qs.iterator():
        percorridos += 1
        chamados[nota.chamado_id] = nota.chamado

    exigir(percorridos == universo)
    print(
        "HISTORICO empresa=%s universo_ocorrencias=%s contador=%s "
        "chamados_distintos=%s conversa_no_instante=[nao verificado] "
        "cobertura_assinaturas_antigas=[nao verificado]"
        % (empresa, universo, percorridos, len(chamados))
    )
    for pk in sorted(chamados):
        linha_caso("HISTORICO_ASSINATURA", empresa, chamados[pk], cache)


def ensaio(empresa):
    visitas = []
    retratados = []
    recusados = []
    capturado = io.StringIO()
    real_guarda = supra.pode_executar_csf
    real_retratar = reconciliador.retratar
    instantaneos = {}
    sentinela = "sonda_cadeia20_rollback"

    def guarda_observada(ch):
        resultado = real_guarda(ch)
        visitas.append((ch.pk, bool(resultado)))
        return resultado

    def retratar_observado(ch, *args, **kwargs):
        # Guarda status e IDs de followups antes da escrita para conferência
        # depois do rollback. A função real permanece responsável pelo ato.
        if ch.pk not in instantaneos:
            instantaneos[ch.pk] = (
                ChamadoColaborador.objects.values_list(
                    "status_local", flat=True
                ).get(pk=ch.pk),
                tuple(FollowupChamado.objects.filter(
                    chamado_id=ch.pk
                ).order_by("pk").values_list("pk", flat=True)),
            )
        resultado = real_retratar(ch, *args, **kwargs)
        (retratados if resultado else recusados).append(ch.pk)
        return resultado

    try:
        with transaction.atomic():
            # PostgreSQL reverte também a criação desta tabela temporária.
            with connection.cursor() as cursor:
                cursor.execute("SELECT to_regclass('pg_temp.%s')" % sentinela)
                exigir(cursor.fetchone()[0] is None)
                cursor.execute(
                    "CREATE TEMP TABLE %s (valor integer) ON COMMIT DROP" [nao verificado]
                    % sentinela
                )
                cursor.execute("INSERT INTO %s VALUES (1)" % sentinela)
                cursor.execute("SELECT count(*) FROM %s" % sentinela)
                exigir(cursor.fetchone()[0] == 1)

            # A saída nominal do comando fica exclusivamente em memória.
            # As funções observadas continuam executando suas implementações.
            with patch.object(supra, "pode_executar_csf", guarda_observada), \
                 patch.object(reconciliador, "retratar", retratar_observado), \
                 contextlib.redirect_stdout(capturado), \
                 contextlib.redirect_stderr(capturado):
                call_command(
                    "supra_juiz", empresa=empresa, executar=True,
                    top=0, stdout=capturado, stderr=capturado,
                )

            avaliados, placar = placar_do_comando(capturado.getvalue())
            preservados = [pk for pk, permitido in visitas if not permitido]
            permitidos = [pk for pk, permitido in visitas if permitido]

            exigir(len(preservados) == placar.get(
                "CSF_COM_CONVERSA_PRESERVADO", 0
            ))
            exigir(len(retratados) == placar.get("CSF_RETRATADO", 0))
            exigir(len(recusados) == placar.get(
                "CSF_RETRATACAO_RECUSADA", 0
            ))
            exigir(len(visitas) == len(preservados) + len(permitidos))

            # Todos os permitidos devem terminar em retratação, recusa,
            # órfã de regeneração ou trava T6.1.
            exigir(len(permitidos) == (
                len(retratados) + len(recusados)
                + placar.get("COBRANCA_ORFA_REGENERACAO", 0)
                + placar.get("CSF_RETRATAVEL_TRAVADO_T6_1", 0)
            ))

            print(
                "ENSAIO empresa=%s dias_colab_avaliados=%s "
                "universo_guardas=%s contador_guardas=%s "
                "CSF_RETRATADO=%s CSF_COM_CONVERSA_PRESERVADO=%s "
                "CSF_RETRATACAO_RECUSADA=%s "
                "COBRANCA_ORFA_REGENERACAO=%s "
                "CSF_RETRATAVEL_TRAVADO_T6_1=%s "
                "CSF_SEM_LAVRA=%s CSF_LAVRA_SEM_CODIGO=%s"
                % (
                    empresa, avaliados, len(visitas),
                    len(preservados) + len(permitidos),
                    len(retratados), len(preservados), len(recusados),
                    placar.get("COBRANCA_ORFA_REGENERACAO", 0),
                    placar.get("CSF_RETRATAVEL_TRAVADO_T6_1", 0),
                    placar.get("CSF_SEM_LAVRA", 0),
                    placar.get("CSF_LAVRA_SEM_CODIGO", 0),
                )
            )

            # A classificação do dono também ocorre dentro do atomic.
            cache = {}
            categorias = (
                ("CSF_RETRATADO", retratados),
                ("CSF_COM_CONVERSA_PRESERVADO", preservados),
                ("CSF_RETRATACAO_RECUSADA", recusados),
            )
            for rotulo, pks in categorias:
                for pk in pks:
                    ch = ChamadoColaborador.objects.select_related(
                        "colaborador"
                    ).get(pk=pk)
                    linha_caso(rotulo, empresa, ch, cache)

            raise DesfazerEnsaio()

    except DesfazerEnsaio:
        pass

    # Verifica a reversão da sentinela e das linhas dos chamados que
    # chegaram à porta real de retratação.
    with connection.cursor() as cursor:
        cursor.execute("SELECT to_regclass('pg_temp.%s')" % sentinela)
        exigir(cursor.fetchone()[0] is None)

    conferidos = 0
    for pk, (status, followups) in instantaneos.items():
        exigir(
            ChamadoColaborador.objects.values_list(
                "status_local", flat=True
            ).get(pk=pk) == status
        )
        exigir(
            tuple(FollowupChamado.objects.filter(
                chamado_id=pk
            ).order_by("pk").values_list("pk", flat=True)) == followups
        )
        conferidos += 1
    exigir(conferidos == len(instantaneos))
    print(
        "ROLLBACK empresa=%s sentinela_revertida=1 "
        "universo_chamados=%s contador_conferidos=%s"
        % (empresa, len(instantaneos), conferidos)
    )


def main():
    global ERROS
    try:
        validar_ambiente()
    except Exception:
        erro()
        print("MEDIDA_INVALIDA ERROS=%s" % ERROS)
        return

    # Evita que mensagens de logging do comando exponham dados pessoais.
    nivel_anterior = logging.root.manager.disable
    logging.disable(logging.CRITICAL)
    try:
        for empresa in EMPRESAS:
            try:
                historico_assinatura(empresa)
            except Exception:
                erro()
                print("HISTORICO_INVALIDO empresa=%s" % empresa)

            try:
                ini, fim = janela_atual(timezone.localdate(), empresa)
                print("JANELA empresa=%s ini=%s fim=%s" % (empresa, ini, fim))
                ensaio(empresa)
            except Exception:
                erro()
                print("ENSAIO_INVALIDO empresa=%s" % empresa)
    finally:
        logging.disable(nivel_anterior)

    print("ERROS=%s" % ERROS)
    print("MEDIDA=%s" % ("VALIDA" if ERROS == 0 else "INVALIDA"))


main()
```

## 19/11 12:55 · sessao 0972cf0af452 · CADEIA-20-PASSO-1 · construtor · origem=Code

Implementei a sincronização dos instantes com as órfãs, a ligação por ocorrência no cartório, os dois testes RED, os mutantes m13 e m14 e o ajuste da medição de consultas. A execução dos testes e dos mutantes está **[não verificada]**; foi verificada apenas a sintaxe dos cinco arquivos alterados. [nao verificado]

A causa exata do `-14 != -28` também permanece **[não verificada]**. A medição anterior reutilizava o colaborador e executava primeiro o caminho sem lavra; o código possui caches de instância e de processo que podem sobreviver ao rollback. A nova medição usa colaborador novo e aquece os dois caminhos antes das capturas, mantendo a igualdade dos acréscimos e o limite de uma chamada a CXR. A relação específica desses caches com os `-14` e `-28` não foi demonstrada. ([test_cadeia20_lavra.py:205](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:205), [colaboradores/models.py:455](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/colaboradores/models.py:455), [ausencias.py:257](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/catalogo/ausencias.py:257))

**Inventário**

- A grade ordena os pares `(hora, instante)` juntos e, na reconciliação, remove ambos pelo índice do instante identificado. Se houver mais de um par com o mesmo instante exato, a borda não é reconciliada: o transporte não distingue qual ocorrência pertence a ela. Esse caso fica sem adivinhação e sua ocorrência em produção é **[não verificada]**. ([utils.py:1867](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1867), [utils.py:2043](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2043), [utils.py:2084](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2084))
- No cartório, a k-ésima ocorrência recebe o k-ésimo instante da mesma hora; contagens divergentes geram `BATIDA_AMBIGUA`. ([cartorio.py:975](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:975))
- O RED de duas batidas reais no mesmo minuto está em [test_cadeia20_costura.py:66](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_costura.py:66); o RED da reconciliação está em [test_cadeia20_costura.py:81](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_costura.py:81).
- No catálogo, `m13_ambigua_por_hora` deve ser morto por `test_RED_duas_sobras_mesmo_minuto`, e `m14_instante_nao_removido` por `test_RED_reconciliacao_retira_instante_da_sobra`. Resultado de ambos: **[não verificado]**. ([test_cadeia20_mutantes.py:52](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_mutantes.py:52))

**Censo das alterações de órfãs na grade**

- Inicialização e montagem: listas vazias no intermitente e na folga; lista do turno e instantes nascidos do mesmo coletor, com acréscimos pareados nas duas pontas e nos excedentes internos. ([utils.py:1589](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1589), [utils.py:1724](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1724), [utils.py:1746](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1746), [utils.py:1758](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1758), [utils.py:1834](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1834))
- Conservação acrescenta hora e instante nos dois ramos; a ordenação posterior opera sobre os pares. ([utils.py:1859](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1859), [utils.py:1863](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1863), [utils.py:1867](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1867))
- Folga, falta, ausência integral e justificativa limpam a lista inteira; a grade limpa os instantes correspondentes após os overlays. ([utils.py:1174](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1174), [utils.py:1239](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1239), [utils.py:1274](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1274), [utils.py:1279](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1279), [utils.py:1884](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1884))
- A reconciliação remove a ocorrência identificada; o recorte posterior da grade também limpa os instantes dos dias cujas órfãs apaga. ([utils.py:2084](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2084), [utils.py:979](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:979), [utils.py:983](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:983))
- Ficaram fora os demais itens do laudo, por estarem fora do escopo solicitado. A identificação de ocorrências com instante exatamente igual permanece limitada como descrito acima. ([utils.py:2046](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2046))

Comandos para a sessão executar, **não executados aqui**:

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_costura.CosturaTest.test_RED_duas_sobras_mesmo_minuto
PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_costura.CosturaTest.test_RED_reconciliacao_retira_instante_da_sobra
PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_lavra.TetoConsultaLavraTest.test_mesmo_acrescimo_em_n_e_2n_dias
CADEIA20_MUTANTE=m13_ambigua_por_hora PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
CADEIA20_MUTANTE=m14_instante_nao_removido PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
```

## 19/11 12:51 · sessao 93aecad35d67 · CADEIA-20-PASSO-1 · verificador · origem=Code

**BLOQUEIA.** A cópia transporta instantes, mas a lavra volta a ligá-los às sobras por `HH:MM`: se há dois instantes na mesma hora e no mesmo dia da grade, atribui `BATIDA_AMBIGUA` a cada ocorrência sem usar o instante para distingui-las. Além disso, a grade pode retirar uma sobra depois de registrar seu instante, sem retirar esse instante da lista transportada. Isso deixa a ligação pedida sem garantia de correspondência um a um. ([app/ponto/services/cartorio.py:975](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:975); [app/escala/utils.py:1855](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1855); [app/escala/utils.py:2067](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2067))

A árvore disponível não é um repositório Git: a confirmação independente do `HEAD` e dos dois diffs por Git ficou **[não verificado]**. A análise abaixo usa os diffs fornecidos e os arquivos presentes na cópia. Não executei testes que criariam banco ou arquivos; resultados reais dos mutantes ficam **[não verificado]**. ([app/escala/tests/test_cadeia20_mutantes.py:1](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:1))

### 1. Itens procedentes da triagem

| Item | Veredito | Evidência e falta |
|---|---|---|
| **1a** | **NÃO BLOQUEIA quanto à medida; alcance da relavra [não verificado]** | A medida lê `cel.ata`; a impressão igual ainda pula células sem `forcar`, e o comando oferece `--forcar`. O dado não prova que a relavra da competência 11 ocorreu. ([cartorio.py:994](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:994); [cartorio.py:1511](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1511); [processar_cartorio.py:29](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:29)) |
| **2** | **NÃO BLOQUEIA** | A marca é calculada ao lavrar, entra em cada resposta e o teste muda a fonte e verifica a ata antiga e uma nova lavra. A costura exige ata idêntica em duas lavras forçadas; execução **[não verificado]**. ([cartorio.py:980](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:980); [test_cadeia20_lavra.py:160](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:160); [test_cadeia20_costura.py:54](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:54)) |
| **3** | **NÃO BLOQUEIA, com limite da igualdade no item 4** | O contador transitório saiu; a função única soma sobre as atas recebidas e separa legado sem chave. ([cartorio.py:994](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:994); [cartorio.py:2072](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:2072)) |
| **4** | **BLOQUEIA** | O transporte por instante existe, mas a etapa `hora → lista de instantes` e a reconciliação posterior descritas acima impedem afirmar que cada sobra recebe seu próprio instante. Falta preservar e reconciliar a associação por ocorrência até a ata. ([utils.py:1719](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1719); [utils.py:2067](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2067); [cartorio.py:976](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:976)) |
| **5** | **NÃO BLOQUEIA** | `cxr_lavrado` declara os dias da lavra e devolve `None` quando a cobertura não é conhecida; a marca preserva esse `None`. ([cxr_na_lista.py:165](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cxr_na_lista.py:165); [cartorio.py:980](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:980)) |
| **6** | **NÃO BLOQUEIA quanto à carga por colaborador** | `_cxr` inicializa na primeira consulta e memoriza; sem sobra a lavra retorna antes da consulta. Há teste de consultas para 2 e 4 dias; execução **[não verificado]**. O comando ainda busca a última lavra uma vez quando há fila. ([cartorio.py:964](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:964); [cartorio.py:1052](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1052); [test_cadeia20_lavra.py:188](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:188); [processar_cartorio.py:59](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:59)) |
| **7** | **BLOQUEIA como prova completa** | Há costura e catálogo de 12 mutantes, mas a costura força uma célula e não reproduz todas as condições do comando; tampouco prova o caso de duas ocorrências da mesma hora descrito no item 4. Resultado de execução dos mutantes **[não verificado]**. ([test_cadeia20_costura.py:31](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:31); [test_cadeia20_mutantes.py:25](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:25)) |
| **8** | **NÃO BLOQUEIA** | O diff fornecido remove o executor que alterava `sys.modules`; o novo catálogo é um `TestCase` Django. Sua execução **[não verificado]**. ([test_cadeia20_mutantes.py:51](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:51); [não verificado] quanto à conferência Git independente) |

### 2. Escritores, leitores e efeitos

- **`ata['razoes_orfas']` e `falha` — NÃO BLOQUEIA quanto à porta de escrita.** O juiz monta `hora`, `razao`, `falha` e `escala_nao_descreve`; `_lavrar_razoes_sobras` pode substituir a resposta por `BATIDA_AMBIGUA` e escreve a lista na ata. Ele é chamado nos ramos `nunca_bateu` e principal antes de `lavrar_veredito`. A medida lê `razao` e `falha`; os demais leitores diretos encontrados são testes. ([orfa_classe.py:78](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:78); [cartorio.py:987](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:987); [cartorio.py:1389](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1389); [cartorio.py:1970](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1970); [cartorio.py:1007](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1007))

- **`insumos_razao_sobras` — BLOQUEIA pela associação de instantes.** A grade escreve `turnos`, `avulsas` e `instantes`; o cartório lê os três. O dispatcher legado pode devolver grade sem a chave, caso em que a lavra retorna sem escrever razões. `grade_espelho_janela` devolve o dicionário inteiro, portanto seus chamadores recebem também a chave adicional quando o builder por turno é usado; serialização integral por algum deles permanece **[não verificado]**. ([utils.py:1925](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1925); [utils.py:2099](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2099); [utils.py:2113](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2113); [utils.py:2366](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2366); [cartorio.py:967](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:967))

- **Coletor de `_match_marcos` — NÃO BLOQUEIA quanto ao significado dos índices.** Nos dois ramos, `marcos` mapeia índice do marco para índice em `locs`; `orfas` contém índices em `locs`, não em `B` nem na lista final de órfãs. A grade lê ambos para transportar instantes. ([utils.py:623](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:623); [utils.py:642](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:642); [utils.py:676](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:676); [utils.py:1719](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1719))

- **Retorno da medida e contador removido — NÃO BLOQUEIA quanto a leitores diretos encontrados.** `medir_razoes_sobras` é consumida nos testes encontrados; não encontrei leitor de produção direto de `sobras_sem_razao` na cópia. O comando imprime outros contadores, sem esse campo. A ausência de leitores dinâmicos ou externos à cópia é **[não verificado]**. ([cartorio.py:994](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:994); [test_cadeia20_costura.py:52](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:52); [processar_cartorio.py:68](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:68))

- **Impressão, ata e consumidores indiretos — NÃO BLOQUEIA a impressão; censo completo [não verificado].** A impressão de insumos não inclui a nova marca ou a ata; por isso a chave nova não força relavra, e células com impressão igual podem ser puladas. A ata é `JSONField`, e uma ata relavrada passa a ter conteúdo adicional; comparação ou serialização da ata inteira pode, portanto, observar conteúdo diferente, mas os efeitos em todos os leitores de tela, comando, ESTADO e vigia não foram integralmente estabelecidos. ([cartorio.py:490](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:490); [cartorio.py:601](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:601); [cartorio.py:1511](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1511); [models.py:1778](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/models.py:1778))

### 3. Instante, identidade e meia-noite

**BLOQUEIA a garantia de ligação correta.** `_identidade_batida` usa `pk`, ou `id` quando falta `pk`; candidatos com o mesmo `pk` não viram ambíguos apenas por serem cópias em turno e pausa. Porém as avulsas são deduplicadas entre si, não removidas dos candidatos de turno; a classificação consulta os carimbos de todos os encontrados e escolhe o primeiro turno encontrado. Os testes cobrem cópias com o mesmo `pk` e duas batidas com `pk` distintos, mas não demonstram todos os papéis possíveis das cópias. ([orfa_classe.py:25](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:25); [orfa_classe.py:42](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:42); [orfa_classe.py:63](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:63); [test_cadeia20_lavra.py:111](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:111))

O `locs` guarda `localtime(b.timestamp)`, preservando o valor temporal e microssegundos na conversão; o pareador recebe cópias rasas das batidas carregadas, com o timestamp do objeto copiado. Portanto a comparação pretende igualdade de `datetime`, não identidade do mesmo objeto `datetime`; igualdade efetiva para todos os registros de produção é **[não verificado]**. ([utils.py:1339](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1339); [turnos.py:1984](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/turnos.py:1984); [orfa_classe.py:58](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:58))

Na fronteira, a grade pode atribuir pelo dia do turno no alinhamento e pelo dia civil na conservação S133; a reconciliação da saída cruzada procura a lista do dia seguinte. `data_turno` é recebido pelo juiz de razão, mas não filtra seus candidatos: o instante é o discriminador ali. Correção de todos os casos de fronteira é **[não verificado]**, além do defeito de associação por hora já demonstrado no código. ([utils.py:1778](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1778); [utils.py:1839](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1839); [utils.py:1997](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1997); [orfa_classe.py:30](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:30))

### 4. Universo da medida

**NÃO BLOQUEIA para o tratamento de ata antiga; BLOQUEIA se `contador_igual_universo` for tomado como prova de correspondência por célula.** `sobras_total` soma `ata['orfas']` de todas as células recebidas. Ata com órfãs e sem a chave incrementa células e órfãs sem chave e não entra em `com_razao` nem `sem_razao`. ([cartorio.py:998](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:998))

A igualdade, porém, compara **totais agregados**: uma razão excedente numa célula e uma faltante em outra podem compensar-se. Ela verifica igualdade de cardinalidades globais, não identidade, ordem ou correspondência por célula. O teste cobre uma entrada removida, não essa compensação. ([cartorio.py:1007](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1007); [cartorio.py:1015](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1015); [test_cadeia20_lavra.py:145](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:145))

### 5. Costura e mutantes

**BLOQUEIA como equivalência completa ao comando.** A costura chama `julgar_colab` com uma célula, `apply_=True`, `forcar=True` e lavra explícita. O comando passa todas as células passadas do colaborador, usa as opções de `--apply` e `--forcar`, e também passa `prefix` e `write`. Assim, a costura exercita a porta real, mas não o mesmo universo nem todos os modos do comando. ([test_cadeia20_costura.py:27](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:27); [processar_cartorio.py:38](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:38); [processar_cartorio.py:61](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/management/commands/processar_cartorio.py:61))

**Mutantes: resultado [não verificado].** O executor exige uma ocorrência textual, executa a fonte mutada, aplica patches e exige falha com regex; isso é uma condição do teste, não prova de que os 12 passaram sem executá-los. O patch do builder altera o atributo do módulo usado pelo dispatcher do cartório, mas a referência importada diretamente em `test_cadeia20_costura` continua original: a premissa da grade usa essa referência original, enquanto a chamada do cartório passa pelo módulo alterado. Não concluo que todos os chamadores possíveis estejam cobertos. ([test_cadeia20_mutantes.py:73](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:73); [test_cadeia20_mutantes.py:84](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:84); [test_cadeia20_costura.py:10](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:10); [cartorio.py:1272](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:1272))

**NÃO BLOQUEIA por duplicação:** `MutantesTest` herda os dois testes públicos de `CosturaTest`, de modo que a descoberta normal os executa também na subclasse; isso repete trabalho, mas não demonstra por si só resultado incorreto. Os testes ORM citados congelam o relógio; não encontrei `timezone.now()` direto nos arquivos novos pesquisados, mas todas as chamadas transitivas sem relógio solto ficam **[não verificado]**. ([test_cadeia20_mutantes.py:51](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:51); [test_cadeia20_costura.py:20](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_costura.py:20); [test_cadeia20_lavra.py:200](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_lavra.py:200))

### 6. Classes estruturais

- **Segundo juiz — NÃO BLOQUEIA por duplicação demonstrada; registro [não verificado].** O juiz de razão consulta o juiz de dia sem trabalho e o papel dos turnos; a grade continua usando `_match_marcos` para o alinhamento. Não estabeleci pela leitura realizada que a nova pergunta duplique um juiz registrado, nem confirmei seu registro vivo como autoridade. ([cartorio.py:971](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:971); [orfa_classe.py:38](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:38); [utils.py:595](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:595))
- **Segundo escritor — NÃO BLOQUEIA quanto aos escritores encontrados:** a lista da ata é escrita na função de lavra, inclusive o vazio; a alteração de `falha` ocorre nessa mesma função antes da atribuição. ([cartorio.py:959](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:959); [cartorio.py:987](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:987))
- **Relógio — NÃO BLOQUEIA por leitura nova de instante demonstrada:** o juiz recebe fuso e o cartório obtém o fuso corrente; a grade recebe `agora`. Uma prova de todas as chamadas transitivas fica **[não verificado]**. ([orfa_classe.py:30](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/orfa_classe.py:30); [cartorio.py:983](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:983); [utils.py:1307](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1307))
- **Fonte única e semântica — BLOQUEIA na costura das ocorrências:** os instantes são derivados de `locs` da grade, mas sua lista deixa de acompanhar a remoção de órfãs e é reagrupada por `HH:MM`. Os significados de `hora` local, instante e data da lista precisam permanecer associados até a gravação. ([utils.py:1339](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:1339); [utils.py:2067](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/utils.py:2067); [cartorio.py:976](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/ponto/services/cartorio.py:976))
- **Selo com mordida — BLOQUEIA como prova suficiente:** o catálogo pretende matar mutações nas duas chamadas, no instante, identidade, rótulos e medida; sua execução e cobertura do defeito de associação apontado acima não estão demonstradas. ([test_cadeia20_mutantes.py:25](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:25); [test_cadeia20_mutantes.py:68](/home/ronald/codex_ws/e884742c378e1ef929456616e40efc363a2f84e1/app/escala/tests/test_cadeia20_mutantes.py:68))

### 7. Órfãos após remoções

**NÃO BLOQUEIA para os símbolos pesquisados; censo completo [não verificado].** Não encontrei chamador de código da constante antiga `RAZOES_SEM_EXPLICACAO`, de `classificar_orfas` ou do campo removido `sobras_sem_razao`; há referências históricas em documentação. `JANELA_CLUSTER

[truncado em 20000 de 20521 caracteres]

_7367 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
