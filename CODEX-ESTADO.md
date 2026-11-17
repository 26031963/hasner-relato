# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 01:36** · publicado em **17/11/2026 01:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 01:36 · sessao d9fc54d941c8 · PREPARA-F2-F4-F7 · sonda · origem=Code

**F7 permanece com `lei 0` própria e `RED que morde 2`, como classificação estática.** A property continua divergindo do juiz; os prefills continuam lendo o texto cru. A execução desses REDs permanece **[nao verificado]**. (`app/chamados/models.py:3665`; `app/chamados/juizes.py:1552`; `app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:154`.)

Todos os valores abaixo foram recebidos no dado raspado, sem reconferência independente: **[nao verificado]**. As referências ao código sustentam sua interpretação.

**SCRIPT 1 — projeção sem linha**

A função procura correspondência por **pergunta, ato e instante**. O recorte do comando inclui horizonte, perguntas sem dia e conversas; a projeção integral amplia essa seleção. (`app/chamados/models.py:4087`; `app/chamados/models.py:4094`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:59`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `backfill_recorte_linhas_resposta` | 2992 [nao verificado] | **Confirma e dimensiona** RESPOSTAS projetadas sem linha correspondente no recorte. |
| `backfill_recorte_linhas_validacao` | 2248 [nao verificado] | **Confirma e dimensiona** VALIDAÇÕES projetadas sem correspondência no mesmo recorte. |
| `backfill_recorte_perguntas` | 3023 [nao verificado] | **Dimensiona** perguntas distintas com alguma dessas faltas. |
| `projecao_integral_linhas_resposta` | 16941 [nao verificado] | **Dimensiona** a falta de linhas RESPOSTA no universo integral. |
| `projecao_integral_linhas_validacao` | 15115 [nao verificado] | **Dimensiona** a falta de linhas VALIDAÇÃO no universo integral. |
| `projecao_integral_perguntas` | 16972 [nao verificado] | **Dimensiona** perguntas distintas afetadas no universo integral. |

Esses números preenchem a lacuna quantitativa do final da §4 e dimensionam o R1b da §5. Não estabelecem equivalência com o antigo fold nem demonstram revogação de validação humana; a função apenas propõe conservar a projeção existente. Recorte e integral não se somam. (`app/chamados/models.py:4082`; `app/chamados/models.py:4098`; `app/chamados/models.py:4102`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`.)

**SCRIPT 2 — divergências físicas e estados**

A property testa somente I; o juiz exige I e texto após trim. Portanto, a divergência contada corresponde a instante preenchido com texto vazio/branco. (`app/chamados/models.py:3665`; `app/chamados/juizes.py:1546`; `app/chamados/juizes.py:1552`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `perguntas_lidas` | 50447 [nao verificado] | **Dimensiona** o universo percorrido pelas comparações. |
| `property_respondida_diverge_juiz` | 9626 [nao verificado] | **Confirma e dimensiona o primeiro RED** da §5 na sombra recebida. |
| `U2_fisico_ultima_devolucao_com_V` | 0 [nao verificado] | **Derruba a ocorrência atual dessa forma física no universo sondado**; não demonstra equivalência entre projeção e fold. |
| `U1_forma_materializada_sem_identidade` | 27486 [nao verificado] | **Dimensiona candidatos físicos U1**; materialização sem via de identidade ainda exige julgamento de lastro. |
| `U1_forma_juiz_lastro_false` | 4431 [nao verificado] | **Confirma e dimensiona discordância entre carimbo e juiz de lastro** dentro desses candidatos; não acrescenta automaticamente RED próprio F7. |
| `U1_falhas_juiz_lastro` | 0 [nao verificado] | **Confirma completude das chamadas externas desse juiz na sonda**; não significa ausência de diagnósticos internos. |

U2 usa a ordenação temporal da sequência. U1 exclui as vias que encerram por identidade e consulta o juiz de testemunha externa. (`app/chamados/models.py:4056`; `app/chamados/models.py:3202`; `app/chamados/juizes.py:666`.)

Os números seguintes contam retornos do **adaptador**, inclusive perguntas que podem ficar fora dos gates visuais. Não são contagens de botões apresentados nem de cliques executados. (`app/chamados/templatetags/chamados_tags.py:132`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/_pergunta_card.html:42`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `estado_card_aplicada` | 5869 [nao verificado] | **Dimensiona** o ramo por carimbo; não certifica lastro. |
| `estado_card_aplicavel` | 430 [nao verificado] | **Dimensiona** o retorno “aplicável agora”; não prova validação ou plantio. |
| `estado_card_bloqueada_competencia` | 3 [nao verificado] | **Dimensiona** o impedimento F6 cujo gate conserva o toque de Validar. |
| `estado_card_bloqueada_competencia_entregue` | 65 [nao verificado] | **Confirma e dimensiona** o estado próprio de entregue, retirado de Validar pelos templates. |
| `estado_card_bloqueada_data` | 11 [nao verificado] | **Dimensiona** o impedimento de data em disputa. |
| `estado_card_bloqueada_paridade` | 122 [nao verificado] | **Dimensiona** o impedimento de paridade. |
| `estado_card_espera_declarado_x_gravado` | 9 [nao verificado] | **Confirma e dimensiona** a espera tratada na §4/§5. |
| `estado_card_fora_de_escopo` | 43205 [nao verificado] | **Dimensiona** retornos sem estado de aplicação; não significa perguntas sem resposta ou sem problema. |
| `estado_card_invalida` | 301 [nao verificado] | **Dimensiona** casos em que o derivador não produziu instante de plantio. |
| `estado_card_ja_satisfeita` | 75 [nao verificado] | **Dimensiona** batida aprovada já existente na janela. |
| `estado_card_recusada_fato_ocupado` | 28 [nao verificado] | **Dimensiona** recusas terminais por fato ocupado. |
| `estado_card_recusada_paridade` | 329 [nao verificado] | **Dimensiona** recusas terminais por paridade. |

As autoridades desses estados estão em `app/chamados/services/materializacao.py:840`, `app/chamados/services/materializacao.py:862`, `app/chamados/services/materializacao.py:886`, `app/chamados/services/materializacao.py:891`, `app/chamados/services/materializacao.py:895`, `app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:903` e `app/chamados/services/materializacao.py:933`. Os gates distinguem entregue, espera e competência apenas fechada. (`app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:49`; `app/templates/chamados/partials/modal_fio.html:274`.)

`aplicada` decorre do carimbo antes da consulta ao derivador; por isso seus 5869 não refutam os 4431 resultados U1 sem lastro. A interseção individual dessas contagens é **[nao verificado]**. (`app/chamados/services/materializacao.py:871`.)

O **segundo RED**, texto retirado reaparecendo em prefill, continua sustentado pelo código, mas não recebeu contador específico: a sonda não cruza texto sem I, reabertura, vitalidade e tipo de entrada. Sua ocorrência individual na sombra é **[nao verificado]**. (`app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:154`.)

**SCRIPT 3 — BUG-PAR e U163**

O contador parte dos IDs fechados de U163 e separa carimbadas das atualmente sem carimbo; pertencimento não deriva do estado ou da recusa. (`app/chamados/services/universo_bug_par.py:12`; `app/chamados/services/universo_bug_par.py:33`; `app/chamados/services/materializacao.py:321`; `app/chamados/services/materializacao.py:326`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `BUGPAR_universo` | 60 [nao verificado] | **Dimensiona** U163 atualmente sem carimbo; não comprova identidade com os IDs originais de U60. |
| `BUGPAR_n_aplicaveis` | 40 [nao verificado] | **Dimensiona** as atualmente aplicáveis pela testemunha. |
| `BUGPAR_resolvidas` | 0 [nao verificado] | **Dimensiona** ausência de carimbadas que deixaram de satisfazer o predicado BUG-PAR-103. |
| `BUGPAR_pendentes_aval` | 79 [nao verificado] | **Dimensiona** pendências administrativas, incluindo estados que impedem validar. |
| `BUGPAR_congelados_no_recorte` | 163 [nao verificado] | **Dimensiona** pertencimento presente no recorte; esse contador não deve zerar. |
| `BUGPAR_legitimas` | 10 [nao verificado] | **Dimensiona** retornos `bloqueada_paridade`. |
| `BUGPAR_outras` | 1 [nao verificado] | **Dimensiona** outro estado; qual estado e pergunta: **[nao verificado]**. |
| `BUGPAR_espera_declarado` | 9 [nao verificado] | **Confirma e dimensiona** a espera declarado × gravado da §4/§5. |
| `BUGPAR_bug_par_103` | 19 [nao verificado] | **Dimensiona** carimbadas absolvidas devolvíveis em competência não entregue. |
| `BUGPAR_bug_par_103_pauta_dp` | 84 [nao verificado] | **Dimensiona** carimbadas absolvidas em competência entregue, separadas para Pauta DP. |
| `BUGPAR_bug_par_103_sem_dia` | 0 [nao verificado] | **Dimensiona** ausência de dia faltante nesse subconjunto carimbado. |
| `U163_existentes` | 163 [nao verificado] | **Confirma e dimensiona** presença dos IDs da fonte fechada. |
| `U163_competencia_entregue` | 85 [nao verificado] | **Dimensiona** toda U163 em entregue pelo dia do fato; não se soma aos 84 de Pauta DP. |
| `U163_sem_dia_do_fato` | 0 [nao verificado] | **Dimensiona** ausência de dia faltante em toda U163 existente; não responde pelo universo fora163. |

As partições são as do contador real: `40+10+1+9=60`, `60+19=79` e `79+0+84+0=163`, aplicadas aos valores recebidos **[nao verificado]**. (`app/chamados/services/materializacao.py:330`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:351`; `app/chamados/services/materializacao.py:356`.)

**Correção da §4:** já há redação registrada para `CORTE-U163-NO-K6`: retirar a exclusão de U163 da guarda L1/L5 e manter F6 intocada. Cai a lacuna “literal não verificado”; permanece a implementação pendente na F4, pois o código ainda pula U163. Os 85 dimensionam esse complemento; os 40 aplicáveis são resultado da guarda atual, não previsão após a mudança. (`app/docs/BACKLOG.md:823`; `app/chamados/services/materializacao.py:795`; `app/chamados/services/materializacao.py:806`.)

**SCRIPT 4 — ressuscitação**

A seleção real exige materialização, texto `NULL` ou vazio, exclusões por via, guarda do marco e C6 sobre a pergunta como ficaria após a limpeza. Portanto, não é censo de toda resposta vazia ou de todos os pares ilegais. (`app/chamados/models.py:1530`; `app/chamados/models.py:1533`; `app/chamados/models.py:1550`; `app/chamados/models.py:1553`.)

| Rótulo | Valor recebido | O que responde em §4/§5 |
|---|---:|---|
| `ressuscitacao_atual_chamados` | 730 [nao verificado] | **Dimensiona** chamados com seleção não vazia pela porta atual. |
| `ressuscitacao_atual_perguntas` | 2025 [nao verificado] | **Dimensiona** perguntas distintas que seriam selecionadas. |
| `ressuscitacao_atual_colaboradores` | 119 [nao verificado] | **Dimensiona** pessoas distintas alcançadas pela seleção. |

Isso preenche o alcance da dependência herdada na §4, sem decidir “deveria voltar ao colaborador?”. Essa pergunta continua expressamente não decidida. Não demonstra ressuscitação executada. (`app/chamados/models.py:1501`; `app/docs/LEIS.md:151`; `app/docs/CORTES.md:488`.)

**ERROR e números ausentes**

**Scripts 2/3, causa em uma linha:** marco cobrado e ATA discordam no tipo; o código registra `Exception` como diagnóstico e continua retornando o tipo do marco cobrado. (`app/chamados/services/materializacao.py:568`; `app/chamados/services/materializacao.py:571`; `app/chamados/services/materializacao.py:576`.)

`NoneType: None` decorre de `exc_info=True` nessa chamada sem exceção ativa. (`app/core/observ.py:79`; `app/chamados/services/materializacao.py:573`.)

No dado recebido, os scripts 1–4 imprimiram todos os números previstos e terminou `rc_sombra=0`: nenhum número solicitado ficou ausente por esses diagnósticos **[nao verificado]**. Não há contador dedicado de divergências de marco; `U1_falhas_juiz_lastro=0` não mede isso. (`app/core/observ.py:75`; `app/core/observ.py:77`.)

O SCRIPT 5 foi retirado deliberadamente desta volta; não apresentou falha nem número de frota **[nao verificado]**.

**Contagem final:** nenhum número acrescenta pergunta própria de lei F7 ou novo RED que morde. Mantêm-se `lei 0` própria e `RED que morde 2`; o primeiro ganhou dimensão de sombra, e o segundo continua sem dimensão de frota. A pergunta herdada de ressuscitação permanece aberta; U163 passa de lacuna documental a complemento F4 já decidido. (`app/docs/LEIS.md:151`; `app/docs/BACKLOG.md:823`; `app/chamados/models.py:3665`; `app/chamados/juizes.py:1552`; `app/templates/chamados/partials/_card_atendimento.html:148`.)

## 17/11 01:35 · sessao ab507d5ea233 · - · - · origem=Ronald (tmux)

SCRIPT 3 interpreta assim os valores do dado fornecido; confirmação independente dos valores em banco: **[nao verificado]**.

| Rótulo | Valor | Resposta às seções 4/5 |
|---|---:|---|
| `BUGPAR_universo` | 60 | Dimensiona as perguntas atuais de U163 sem carimbo de materialização. Não comprova que os IDs sejam exatamente os do conjunto original U60. (`app/chamados/services/materializacao.py:321`, `:326`, `:338`, `:351`.) |
| `BUGPAR_n_aplicaveis` | 40 | Dimensiona as que a testemunha real devolve como `aplicavel`; não comprova execução da validação nem teste RED. (`app/chamados/services/materializacao.py:339`, `:340`, `:352`.) |
| `BUGPAR_resolvidas` | 0 | Nenhuma carimbada deixou de satisfazer o predicado `paridade_absolvida_bug_par_103` neste recorte. (`app/chamados/services/materializacao.py:326`, `:327`, `:328`.) |
| `BUGPAR_pendentes_aval` | 79 | Dimensiona pendências administrativas: `universo + len(bug_par_103)` = 60+19, incluindo estados que ainda impedem validar. (`app/chamados/services/materializacao.py:314`, `:356`.) |
| `BUGPAR_congelados_no_recorte` | 163 | Dimensiona pertencimento fechado no recorte; não é um contador destinado a zerar. (`app/chamados/services/materializacao.py:315`, `:356`; `app/chamados/services/universo_bug_par.py:30`.) |
| `BUGPAR_legitimas` | 10 | Dimensiona respostas cuja testemunha retorna `bloqueada_paridade`; não significa dez leis ou dez REDs. (`app/chamados/services/materializacao.py:342`, `:343`.) |
| `BUGPAR_outras` | 1 | Dimensiona um estado diferente de aplicável/paridade/espera; o estado individual não consta do dado. (`app/chamados/services/materializacao.py:344`, `:346`, `:347`; identidade/estado: **[nao verificado]**.) |
| `BUGPAR_espera_declarado` | 9 | Confirma ocorrência da espera prevista pelo corte declarado versus gravado; dimensiona o caso RED da seção 5, sem demonstrar o HTML ou clique. (`app/chamados/services/materializacao.py:305`, `:344`, `:736`, `:741`.) |
| `BUGPAR_bug_par_103` | 19 | Dimensiona carimbadas absolvidas devolvíveis, com competência não entregue. (`app/chamados/services/materializacao.py:326`, `:330`, `:335`.) |
| `BUGPAR_bug_par_103_pauta_dp` | 84 | Dimensiona carimbadas absolvidas em competência entregue, separadas para Pauta DP. (`app/chamados/services/materializacao.py:333`, `:334`; `app/chamados/services/validacao.py:338`, `:339`.) |
| `BUGPAR_bug_par_103_sem_dia` | 0 | Nenhuma carimbada absolvida sem data do fato nesse subconjunto. (`app/chamados/services/materializacao.py:330`, `:331`, `:332`.) |
| `U163_existentes` | 163 | Dimensiona presença em banco dos IDs da fonte fechada; pertencimento não depende de recusa ou lâmpada. (`app/chamados/services/universo_bug_par.py:12`, `:20`, `:30`, `:33`.) |
| `U163_competencia_entregue` | 85 | Dimensiona toda a interseção U163 × competência entregue pelo dia do fato, maior que o subconjunto Pauta DP de 84; não soma aos 84. (`app/chamados/services/materializacao.py:776`, `:784`, `:788`, `:789`.) |
| `U163_sem_dia_do_fato` | 0 | Nenhuma pergunta existente de U163 sem dia do fato; não extrapola às perguntas fora163. (`app/chamados/services/materializacao.py:782`, `:784`.) |

As partições fecham: 40+10+1+9=60; 60+19=79; 79+0+84+0=163. São relações do contador real. (`app/chamados/services/materializacao.py:351`, `:356`.) Como as carimbadas são 19+84 e `resolvidas=0`, a diferença 85−84 situa uma entregue entre as 60 sem carimbo; sua identidade é **[nao verificado]**.

**Correção relevante da seção 4:** a cópia contém a redação registrada do `CORTE-U163-NO-K6`: retirar a exclusão das 163 de `k6_consulta_a_pergunta`, aplicar L1/L5 e manter F6 intocada; é complemento F4 com sonda antes de construir. A antiga lacuna “literal [nao verificado]” pode ser baixada. (`app/docs/BACKLOG.md:823`.) O código ainda exclui U163, portanto os 85 dimensionam esse complemento pendente, sem constituir nova pergunta de lei F7. (`app/chamados/services/materializacao.py:792`, `:795`, `:806`.)

**ERROR dos scripts 2/3, em uma linha:** divergência entre o tipo do marco cobrado e o tipo da ATA; o código registra um objeto `Exception` e segue retornando o tipo do marco cobrado, sem `raise` nesse ramo. (`app/chamados/services/materializacao.py:527`, `:568`, `:571`, `:576`.) O `NoneType: None` vem de `exc_info=True` sem exceção ativa nessa chamada. (`app/core/observ.py:79`; `app/chamados/services/materializacao.py:573`.)

Não há evidência de script abortado: os números solicitados dos scripts 1–4 e `rc_sombra=0` estão no dado; confirmação independente da execução: **[nao verificado]**. Script 5 foi explicitamente excluído, não falhou **[nao verificado]**. Falta número próprio para divergências de marco e exceções engolidas pelo adaptador: `U1_falhas_juiz_lastro=0` não mede esses eventos. (`app/core/observ.py:75`, `:77`; `app/chamados/templatetags/chamados_tags.py:135`, `:137`.)

## 17/11 01:34 · sessao 19c1683c44fa · - · - · origem=Ronald (tmux)

Somente leitura; conclusões das sondas 1 e 4:

| NUMERO recebido | Interpretação e efeito em §4/§5 | Código atual |
|---|---|---|
| `backfill_recorte_linhas_resposta = 2992` | Linhas RESPOSTA propostas no recorte do comando; confirma existência e dimensiona projeção sem correspondência na sequência. Dimensiona §4 e R1b de §5; não fecha o antigo oráculo de fold. Valor recebido: [nao verificado] | `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `app/chamados/models.py:4098` |
| `backfill_recorte_linhas_validacao = 2248` | Mesmo recorte, linhas VALIDACAO propostas. Dimensiona a conservação necessária; não significa 2248 perguntas adicionais. Valor recebido: [nao verificado] | `app/chamados/models.py:4102` |
| `backfill_recorte_perguntas = 3023` | Perguntas distintas com ao menos uma das duas faltas. O recorte inclui horizonte L-124, perguntas sem dia e conversas. Valor recebido: [nao verificado] | `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:59` |
| `projecao_integral_linhas_resposta = 16941` | Linhas RESPOSTA propostas sobre todas as perguntas percorridas pela sonda; dimensiona estoque integral, além do recorte. Valor recebido: [nao verificado] | `app/chamados/models.py:4091`; `app/chamados/models.py:4098` |
| `projecao_integral_linhas_validacao = 15115` | Linhas VALIDACAO propostas no universo integral. Valor recebido: [nao verificado] | `app/chamados/models.py:4102` |
| `projecao_integral_perguntas = 16972` | Perguntas distintas com alguma falta no universo integral. Recorte e integral não se somam, pois a função de domínio é a mesma e o comando restringe a seleção. Valor recebido: [nao verificado] | `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `app/chamados/models.py:4089` |
| `ressuscitacao_atual_chamados = 730` | Chamados com lista não vazia da porta hoje; dimensiona a dependência herdada de ressuscitação em §4. Valor recebido: [nao verificado] | `app/chamados/models.py:1501`; `app/chamados/models.py:1507` |
| `ressuscitacao_atual_perguntas = 2025` | Perguntas distintas que a porta selecionaria; não demonstra que foram ressuscitadas. Valor recebido: [nao verificado] | `app/chamados/models.py:1502`; `app/chamados/models.py:1530`; `app/chamados/models.py:1557` |
| `ressuscitacao_atual_colaboradores = 119` | Pessoas distintas abrangidas pelas 2025 perguntas; dimensiona alcance da pendência herdada. Não fornece lista individual. Valor/lista: [nao verificado] | `app/chamados/models.py:1507` |

A falta de linha é definida por correspondência de **pergunta, ato e instante**, e não por inexistência de qualquer RESPOSTA/VALIDACAO. A função conserva o texto atualmente projetado; não recupera resposta antiga já sobrescrita. (`app/chamados/models.py:4082`; `app/chamados/models.py:4087`; `app/chamados/models.py:4094`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:13`.)

O universo da ressuscitação exige materialização presente e texto `NULL` ou `''`, exclui vias fora do fato, pula marco já aceso e consulta C6 sobre cópia em memória após a limpeza. Portanto, os 2025 não são censo de toda resposta vazia, nem de todos os pares ilegais da L-130. (`app/chamados/models.py:1530`; `app/chamados/models.py:1533`; `app/chamados/models.py:1535`; `app/chamados/models.py:1550`; `app/chamados/models.py:1553`; `app/chamados/models.py:1557`.)

Os números da sonda 4 **dimensionam**, sem resolver “deveria voltar ao colaborador?”. Essa lei continua expressamente não decidida; a implementação conserva a fila atual, limpa o par inteiro e registra previamente as linhas projetadas faltantes, sem usar REABERTURA para mudar o dono. (`app/docs/CORTES.md:488`; `app/docs/LEIS.md:151`; `app/chamados/models.py:1613`; `app/chamados/models.py:1622`; `app/chamados/models.py:1629`.)

Essas duas sondas, por seus predicados, não acrescentam pergunta própria de lei F7 nem novo caso de RED: R1b permanece dimensão de conservação, sem prova do antigo fold; ressuscitação continua dependência herdada. Contagem final depende da conferência das demais sondas e do código atual dos dois REDs. (`app/chamados/models.py:4087`; `app/docs/CORTES.md:488`.)

## 17/11 01:27 · sessao a1991c3094f8 · PREPARA-F2-F4-F7 · sonda · origem=Code

**Os números não mudam a entrega: lei 0 nova; RED que morde 3.** Eles dimensionam o estoque e derrubam a inferência “uma U163 entregue na fila ⇒ uma que hoje plantaria”. Não demonstram execução dos três RED. (`app/docs/RELATO.md:17`; resultados da sonda [nao verificado])

Na cópia atual, lista e contador já constam respondidos por complemento; permanece devolvida a condição completa do RED. Essa atualização vem do registro do corte, não dos números. (`app/docs/RELATO.md:5`, `app/docs/RELATO.md:8`)

Os valores abaixo são os fornecidos no **DADO RASPADO**, sem verificação independente: **[nao verificado]**. As referências sustentam sua interpretação pelo código. Uso estas abreviações, com equivalência literal `sigla:n = arquivo:linha`:

| Sigla | Arquivo |
|---|---|
| M | `app/chamados/services/materializacao.py` |
| MD | `app/chamados/models.py` |
| J | `app/chamados/juizes.py` |
| V | `app/chamados/services/validacao.py` |
| BF | `app/chamados/management/commands/backfill_decisoes_pergunta.py` |
| BP | `app/chamados/management/commands/bug_par_103.py` |
| U | `app/chamados/services/universo_bug_par.py` |

**SCRIPT 1 — lacunas e pré-backfill, seção 4.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `integral_linhas_resposta` | 16941 | **Dimensiona** lacunas RESPOSTA por correspondência de pergunta/ato/instante; não mede divergência de texto. (`MD:4094`, `MD:4098`) |
| `integral_linhas_resposta_em_validadas_nao_materializadas` | 96 | **Confirma** lacuna RESPOSTA também nesse recorte operacional. (`MD:4098`, `M:2063`) |
| `integral_linhas_validacao` | 15115 | **Dimensiona** lacunas VALIDACAO por correspondência de pergunta/ato/instante. (`MD:4102`) |
| `integral_linhas_validacao_em_validadas_nao_materializadas` | 96 | **Confirma** lacuna VALIDACAO nesse recorte; não somar às 96 anteriores como perguntas distintas. (`MD:4098`, `MD:4102`) |
| `integral_perguntas_examinadas` | 50447 | **Dimensiona** o denominador informado; seleção integral exata da sonda [nao verificado]. |
| `recorte_oficial_linhas_resposta` | 2992 | **Dimensiona** o backfill oficial, cujo recorte inclui horizonte, sem dia e conversa; difere do integral. (`BF:55`, `BF:59`, `BF:67`) |
| `recorte_oficial_linhas_validacao` | 2248 | **Dimensiona** a mesma pendência oficial para VALIDACAO. (`BF:59`, `BF:67`, `MD:4102`) |
| `recorte_oficial_nao_entregue_resposta` | 2992 | **Dimensiona:** igual ao total oficial RESPOSTA; não demonstra lacuna entregue nesse recorte. Critério exato dessa classificação [nao verificado]. |
| `recorte_oficial_nao_entregue_validacao` | 2248 | **Dimensiona:** igual ao total oficial VALIDACAO; mesma limitação. Critério exato [nao verificado]. |

**O pré-backfill não está satisfeito como “zero lacunas”.** Tampouco cabe usar o integral como quantidade autorizada do backfill oficial: o comando tem recorte próprio. (`MD:4081`, `BF:55`; valores [nao verificado])

**SCRIPT 2 — contador, seção 4 e OPUS-C4-B da seção 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `contador_aplicaveis` | 40 | **Dimensiona** perguntas cuja testemunha diz `aplicavel`; não prova validação ou plantio. (`M:340`) |
| `contador_bug_par_103` | 19 | **Dimensiona** carimbadas elegíveis ainda devolvíveis, com competência não entregue. (`M:326`, `M:330`, `M:336`) |
| `contador_bug_par_103_pauta_dp` | 84 | **Dimensiona** carimbadas elegíveis separadas por competência entregue. (`M:333`) |
| `contador_bug_par_103_sem_dia` | 0 | **Dimensiona** ausência de exemplar nesse balde; não derruba o RED estrutural sem dia. (`M:331`, `M:795`) |
| `contador_congelados_no_recorte` | 163 | **Confirma** presença do universo congelado no recorte; pertencimento vem do arquivo fechado. (`M:321`, `M:356`, `U:30`) |
| `contador_delta_conservacao` | 0 | **Confirma** conservação aritmética: `79 + 0 + 84 + 0 = 163`. (`M:316`) |
| `contador_espera_declarado` | 9 | **Dimensiona** a espera declarado×gravado, que impede tratar todo pendente como plantável. (`M:344`, `M:741`) |
| `contador_legitimas` | 10 | **Dimensiona** testemunhas `bloqueada_paridade`. (`M:342`) |
| `contador_n_aplicaveis` | 40 | **Confirma** o mesmo tamanho de `contador_aplicaveis`; não são outras 40. (`M:352`, `M:353`) |
| `contador_outras` | 1 | **Dimensiona** outro estado da testemunha; estado individual [nao verificado]. (`M:347`) |
| `contador_pendentes_aval` | 79 | **Confirma** `60 + 19`; mede espera do admin, não promessa de plantio. (`M:314`, `M:356`) |
| `contador_resolvidas` | 0 | **Dimensiona** o estado atual; não demonstra a transição exigida pelo OPUS-C4-B. (`M:327`) |
| `contador_universo` | 60 | **Confirma** a partição corrente `40 + 10 + 9 + 1`; não é o total congelado U163. (`M:351`) |

O OPUS-C4-B **continua sem prova da transição pela autoridade real**: a fotografia acima não executa `pendentes 1→0 / resolvidas 0→1`. O teste existente verifica essa transição, mas sua fixture instala `competencia_entregue=False`. (`app/chamados/tests/test_bug_par_volta.py:463`, `app/chamados/tests/test_bug_par_volta.py:468`, `app/chamados/tests/test_bug_par_projetado.py:51`)

**SCRIPT 2 — lista textual, seção 4 e RED da lista na seção 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `pauta_comando_linhas` | 1178 | **Dimensiona** a saída textual; não são 1178 U163 nem Pautas criadas. Distinção entre itens e linhas físicas da extração [nao verificado]. (`BP:85`, `BP:89`) |
| `pauta_comando_u163_distintas` | 84 | **Dimensiona** presença U163 na saída; igualdade numérica com o balde 84 não comprova identidade individual. (`BP:53`, `BP:62`) |
| `pauta_comando_u60_distintas` | 0 | **Confirma** a omissão atual de U60: primeiro percurso U103; segundo exclui U163. Sustenta a mordida da lista. (`BP:53`, `BP:72`) |

**SCRIPT 2 — U163 e montagem do RED, seções 4 e 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `u163_presentes` | 163 | **Confirma** presença informada do universo fechado `60 + 103`. (`U:20`, `U:30`) |
| `u163_sem_dia_do_fato` | 0 | **Dimensiona:** nenhum exemplar observado para L5; não elimina o defeito estrutural do pulo anterior à consulta do dia. (`M:795`, `M:806`, `M:809`) |
| `u163_competencia_entregue` | 85 | **Dimensiona** competência entregue pelo dia do fato; não significa 85 validáveis. (`M:776`, `M:786`) |
| `u163_entregue_estado_recusada_paridade` | 84 | **Dimensiona** recusas já carimbadas, devolvidas pela testemunha antes do derivador; não são candidatas atuais a plantio. (`M:862`, `M:868`) |
| `u163_entregue_estado_bloqueada_competencia_entregue` | 1 | **Confirma** bloqueio já existente na composição pergunta/irmã; não demonstra a falha procurada pelo novo RED. (`M:885`, `V:224`, `V:225`) |
| `u163_entregue_fm_do_fato_aberto` | 78 | **Dimensiona** FM aberto; sozinho não libera K6 nem as demais guardas. (`M:818`, `V:652`) |
| `u163_entregue_fm_do_fato_aprovado` | 7 | **Dimensiona** o restante da partição `78 + 7 = 85`; aprovação FM não substitui a autoridade K6. (`M:786`, `M:830`) |
| `u163_entregue_na_fila_real` | 1 | **Confirma** uma pergunta no filtro R/V/C do laço; pertencer à fila não equivale a poder plantar. (`M:2063`, `M:2084`) |
| `u163_entregue_fila_testemunha_aplicavel` | 0 | **Derruba** “a única entregue na fila hoje plantaria”; nenhuma aplicável é mostrada nesse recorte. (`M:885`, `M:2063`) |
| `u163_aplicavel_com_bloqueio_real` | 0 | **Dimensiona** a classe nominada; não fornece exemplar do candidato RED. Predicado completo da sonda [nao verificado]. (`V:225`) |
| `u163_aplicavel_sem_bloqueio_real` | 0 | **Dimensiona** a classe nominada; não fecha prova de plantio liberado. Predicado completo [nao verificado]. (`V:225`) |
| `u163_aplicavel_exige_confirmacao` | 0 | **Dimensiona** a classe nominada; não demonstra passagem pela confirmação. Predicado completo [nao verificado]. (`V:615`) |
| `u163_aplicavel_sem_implausivel` | 0 | **Dimensiona** a classe nominada; não demonstra passagem pelas demais guardas. Predicado completo [nao verificado]. (`V:615`) |

Os quatro últimos zeros **não significam “zero aplicáveis em toda U163”**: o contador informou 40. A relação exata entre esses filtros exige o corpo integral da sonda, ausente do texto fornecido. (`M:340`, `M:352`; filtros [nao verificado])

**SCRIPT 3 — L-130, reparo e absorção, seções 4 e 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `respondida_sem_texto` | 9626 | **Dimensiona** estoque com instante e texto vazio após trim. Não prova nova escrita ilegal nem que todo esse estoque deva voltar a pendente. (`J:1534`, `app/docs/LEIS.md:151`) |
| `texto_sem_carimbo` | 0 | **Derruba** hipótese de estoque observado do meio par inverso; não elimina sua guarda nem reabre a lei do reparo. (`J:1541`, `app/docs/CORTES.md:488`) |
| `absorcao_pares_retornados_pela_funcao_real` | 0 | **Dimensiona** ausência de pares retornados na execução fornecida; não prova inexistência histórica de absorção. (`M:1105`, `M:1109`) |
| `absorcao_perguntas_retornadas_pela_funcao_real` | 0 | **Dimensiona** ausência de perguntas retornadas pelo predicado atual, que exclui Batida viva. (`M:1116`, `M:1118`) |
| `absorcao_linhas_no_acervo` | 56 | **Confirma** acervo histórico informado de ABSORCAO; não equivale ao conjunto devolvível hoje. (`MD:4048`, `M:1105`) |
| `absorcao_pares_no_acervo` | 52 | **Dimensiona** pares históricos distintos informados; chave exata dessa agregação [nao verificado]. Não contradiz retorno atual zero. (`MD:4059`, `M:1109`) |

A diferença entre acervo e retorno atual **não revoga o corte**: M9b/M10 permanecem no carimbo nesta F4. (`app/docs/CORTES.md:488`)

**SCRIPT 4 — J1, seções 4 e 5.**

| Rótulo | Valor | O que responde |
|---|---:|---|
| `j1_hora_examinadas` | 7240 | **Dimensiona** o recorte `hora` comparado; seleção exata [nao verificado]. (`J:376`, `M:599`) |
| `j1_hora_divergentes` | 0 | **Confirma** paridade observada nesse recorte; derruba divergência J1 observada nele. O juiz chama o mesmo derivador. (`J:376`) |
| `j1_hora_indeterminadas` | 0 | **Confirma** ausência de indeterminação reportada nessa comparação; não certifica plantio ou fechamento. (`J:378`, `M:2084`) |

Esses números não cobrem o parse próprio de `data_hora`, nem demonstram execução das duas metades ou C6. (`J:363`, `M:214`, `M:222`, `M:2086`)

**ERROR — causa e número faltante.**

| Script | Causa em uma linha | O que ficou sem número |
|---|---|---|
| 2 | `marcos_faltantes` e ATA discordam sobre E/S; o código registra o diagnóstico e retorna `t1 or t2`. (`M:568`, `M:571`, `M:576`) | Nenhuma perda demonstrada; os contadores finais foram fornecidos. Completude contra o roteiro integral [nao verificado]. |
| 4 | Mesmo diagnóstico durante a derivação; isso difere de divergência juiz×materializador. (`M:568`, `J:376`) | Os três números J1 foram fornecidos; outros números previstos [nao verificado]. |

`NoneType: None` é compatível com `exc_info=True` fora de um `except`; não é, por si, traceback de aborto. O dado termina com `rc_sombra=0` [nao verificado]. (`app/core/observ.py:79`, `M:571`)

**Contagem mantida: lei 0 nova / RED que morde 3.** Os zeros não retiram os casos estruturais U163 entregue e U163 sem dia; a omissão U60 sustenta o caso da lista. O candidato produtivo que atravessaria todas as guardas continua sem demonstração, e o OPUS-C4-B permanece lacuna de prova. (`app/docs/RELATO.md:17`, `app/docs/RELATO.md:8`, `M:795`, `BP:72`, `app/chamados/tests/test_bug_par_projetado.py:51`; execução dos RED [nao verificado])

## 17/11 01:25 · sessao 233baf0da3f5 · - · - · origem=Ronald (tmux)

Referências atuais para SCRIPT 2, todas somente lidas:

| Número recebido | Interpretação e efeito |
|---|---|
| `contador_aplicaveis=40` | Comprimento da lista de testemunhas `aplicavel`; dimensiona o recorte, sem provar ato de validação ou plantio. `app/chamados/services/materializacao.py:340` |
| `contador_n_aplicaveis=40` | Mesmo número anterior, exposto como escalar; não somar outra vez. `app/chamados/services/materializacao.py:352` |
| `contador_legitimas=10` | Dez testemunhas `bloqueada_paridade`; dimensiona impedimentos atuais. `app/chamados/services/materializacao.py:342` |
| `contador_espera_declarado=9` | Nove testemunhas `espera_declarado_x_gravado`; dimensiona guarda que antecede validação. `app/chamados/services/materializacao.py:344`, `app/chamados/services/validacao.py:611` |
| `contador_outras=1` | Uma testemunha em outro estado; estado individual não mostrado pelo dado. `app/chamados/services/materializacao.py:347`; identidade/estado exato [nao verificado] |
| `contador_universo=60` | Soma `40+10+9+1`; universo corrente sem carimbo no recorte, não total U163. `app/chamados/services/materializacao.py:351` |
| `contador_bug_par_103=19` | Carimbadas U103 ainda elegíveis e cuja competência do fato não foi entregue. `app/chamados/services/materializacao.py:326`, `app/chamados/services/materializacao.py:336` |
| `contador_bug_par_103_pauta_dp=84` | Carimbadas elegíveis de competência entregue, separadas para Pauta DP. `app/chamados/services/materializacao.py:333` |
| `contador_bug_par_103_sem_dia=0` | Nenhuma carimbada elegível sem dia do fato nesse balde; não elimina RED estrutural sem dia. `app/chamados/services/materializacao.py:331`, `app/chamados/services/materializacao.py:795` |
| `contador_pendentes_aval=79` | `60+19`; aguardam admin. Não é 163, nem promessa de que todas plantariam hoje. `app/chamados/services/materializacao.py:356` |
| `contador_resolvidas=0` | Nenhuma carimbada saiu da condição de caso no recorte. Não demonstra transição do OPUS-C4-B. `app/chamados/services/materializacao.py:327` |
| `contador_congelados_no_recorte=163` | Pertencimento presente no recorte; U60 e U103 vêm de arquivo fechado, disjuntos, união U163. `app/chamados/services/materializacao.py:356`, `app/chamados/services/universo_bug_par.py:20`, `app/chamados/services/universo_bug_par.py:30` |
| `contador_delta_conservacao=0` | Confirma conservação aritmética: `79+0+84+0=163`. Fórmula documental: `app/chamados/services/materializacao.py:316` |
| `pauta_comando_linhas=1178` | Volume recebido da saída textual; código da sonda não disponível para conferir se conta itens ou linhas físicas [nao verificado]. Comando publica `pauta` em `app/chamados/management/commands/bug_par_103.py:85` |
| `pauta_comando_u163_distintas=84` | Dimensiona presença U163 na lista; corresponde numericamente às 84 U103 entregues. Identidade individual dessa correspondência [nao verificado]. Seleção U103: `app/chamados/management/commands/bug_par_103.py:53`, inclusão: `app/chamados/management/commands/bug_par_103.py:62` |
| `pauta_comando_u60_distintas=0` | Confirma omissão atual de U60 nessa saída: primeiro percurso U103, segundo exclui U163. `app/chamados/management/commands/bug_par_103.py:53`, `app/chamados/management/commands/bug_par_103.py:72` |
| `u163_presentes=163` | Todos os membros presentes segundo dado; cardinalidade fechada do arquivo confere com 60+103. `app/chamados/services/universo_bug_par.py:20`, `app/chamados/services/universo_bug_par.py:30` |
| `u163_sem_dia_do_fato=0` | Sem exemplar atual para RED L5; não derruba o pulo estrutural antes de consultar dia. `app/chamados/services/materializacao.py:795`, `app/chamados/services/materializacao.py:806` |
| `u163_competencia_entregue=85` | Dimensiona autoridade K6 pelo fato; não implica 85 validáveis. Autoridade real: `app/chamados/services/materializacao.py:776`, `app/chamados/services/materializacao.py:786` |
| `u163_entregue_estado_recusada_paridade=84` | Testemunha retorna recusa pelo carimbo antes do derivador; não são 84 candidatas a plantio. `app/chamados/services/materializacao.py:862`, `app/chamados/services/materializacao.py:868` |
| `u163_entregue_estado_bloqueada_competencia_entregue=1` | Um caso já bloqueia hoje pela composição pergunta/irmã; não demonstra falha da extensão U163. `app/chamados/services/materializacao.py:885`, `app/chamados/services/validacao.py:224` |
| `u163_entregue_fm_do_fato_aberto=78` | Dimensiona FM; FM aberto sozinho não libera K6 nem demais guardas. `app/chamados/services/materializacao.py:818`, `app/chamados/services/validacao.py:652` |
| `u163_entregue_fm_do_fato_aprovado=7` | Completa `78+7=85`; não substitui entregue pela autoridade K6. `app/chamados/services/materializacao.py:786`, `app/chamados/services/materializacao.py:830` |
| `u163_entregue_na_fila_real=1` | Uma satisfaz filtro R/V/C do laço. Não equivale a testemunha aplicável. `app/chamados/services/materializacao.py:2063` |
| `u163_entregue_fila_testemunha_aplicavel=0` | Derruba a inferência “uma na fila ⇒ uma hoje plantaria”; não elimina RED estrutural. `app/chamados/services/materializacao.py:2063`, `app/chamados/services/materializacao.py:885` |
| `u163_aplicavel_com_bloqueio_real=0` | Nenhum candidato aplicável com bloqueio real mostrado; não prova ausência futura. Critério exato da sonda [nao verificado]; composição atual em `app/chamados/services/validacao.py:225` |
| `u163_aplicavel_sem_bloqueio_real=0` | Nenhum candidato aplicável liberado pela autoridade real mostrado; OPUS-C4-B continua sem prova do ato real. Critério da sonda [nao verificado]; teste existente valida transição em `app/chamados/tests/test_bug_par_volta.py:464` |
| `u163_aplicavel_exige_confirmacao=0` | Nenhum candidato aplicável nessa classe mostrado; não é prova de validação. Guarda: `app/chamados/services/validacao.py:615` |
| `u163_aplicavel_sem_implausivel=0` | Nenhum candidato aplicável sem implausibilidade mostrado; não fecha montagem do RED. Guarda: `app/chamados/services/validacao.py:615` |

O caso bloqueado entre U163 deve envolver pergunta fora163 na composição pelo código: `_k6_que_barra` ignora U163, e `entrega_que_barra_a_validacao` compõe pergunta/irmã. Sem ITEM, identidade e encadeamento individual continuam [nao verificado]. `app/chamados/services/materializacao.py:795`, `app/chamados/services/materializacao.py:805`, `app/chamados/services/validacao.py:224`.

OPUS-C4-B permanece lacuna de prova: teste existente verifica `pendentes 1→0`, `resolvidas 0→1`, congelado e autoridade conservados, mas fixture instala `competencia_entregue=False`. `app/chamados/tests/test_bug_par_volta.py:463`, `app/chamados/tests/test_bug_par_volta.py:468`, `app/chamados/tests/test_bug_par_volta.py:471`, `app/chamados/tests/test_bug_par_projetado.py:51`.

Contagem anterior está documentada: **lei 0 nova; RED que morde 3**, com três pendências U163 no topo. Estes números dimensionam as pendências e afastam candidato produtivo presumido; não resolvem abrangência da lista, destino de balde ou condição normativa do RED. `app/docs/RELATO.md:17`, `app/docs/RELATO.md:19`, `app/docs/RELATO.md:6`, `app/docs/RELATO.md:8`.

## 17/11 01:25 · sessao d8e751783447 · - · - · origem=Ronald (tmux)

- **Script 1:** lacunas são por correspondência `(pergunta, ato, instante)`, não ausência de qualquer ato nem divergência de texto; 96 respostas + 96 validações não significam 192 perguntas distintas. (`app/chamados/models.py:4094`, `app/chamados/models.py:4098`, `app/chamados/models.py:4102`)
- **Recorte oficial:** horizonte L-124, perguntas sem dia e conversas; difere do integral. O comando mede R/V e não as aplica. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`)
- **Script 3:** 9.626 `respondida_sem_texto` dimensionam o estoque do meio par ilegal; não provam escrita nova irregular. Zero `texto_sem_carimbo` elimina estoque observado desse segundo meio par, preservando o controle. (`app/chamados/juizes.py:1534`, `app/chamados/juizes.py:1541`)
- **Absorção:** função real seleciona carimbo, via, escape no grupo e ausência de Batida viva; acervo ABSORCAO guarda vínculos históricos. Portanto 0 pares/0 perguntas retornados coexistem com 56 linhas/52 pares históricos, sem revogar o corte que mantém o leitor por carimbo. (`app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1109`, `app/chamados/services/materializacao.py:1116`, `app/chamados/models.py:4048`, `app/docs/CORTES.md:487`)
- **Script 4:** 7.240 examinadas, 0 divergentes e 0 indeterminadas confirmam paridade observada no recorte `hora`; não comprovam paridade `data_hora`, composição das duas metades, escrita ou C6. Juiz de hora chama o mesmo derivador; `data_hora` tem parse próprio. (`app/chamados/juizes.py:363`, `app/chamados/juizes.py:376`, `app/chamados/services/materializacao.py:599`)
- **ERROR dos scripts 2/4:** conflito `marcos_faltantes`×ATA é registrado, mas prevalece `t1` e a função retorna normalmente; não é divergência entre juiz e materializador. `NoneType: None` decorre do logger com `exc_info=True` chamado fora de `except`. (`app/chamados/services/materializacao.py:568`, `app/chamados/services/materializacao.py:571`, `app/chamados/services/materializacao.py:576`, `app/core/observ.py:79`)
- O dado fornecido termina com os números J1 e `rc_sombra=0`; não demonstra script abortado nem número previsto perdido. Como o texto dos scripts da sonda não acompanha o anexo truncado, completude dos rótulos previstos: **[nao verificado]**.

## 17/11 01:22 · sessao 1739fa0bd37f · REVISAO-AVAL-O10D1711 · verificador · origem=Code

1. **Premissas: conferem, com uma referência errada.** PREPARA fechado, perguntas de lei zero, d1=d2 permitido e cruzamento dependente da lista do construtor estão registrados (`app/docs/RELATO.md:147`; `app/docs/RELATO.md:1811`). O código confirma atomic por chamada, ordenação apenas do par e retorno `'recusada'` capturado, justificando rollback externo e ordenação da composição (`app/ponto/portas/celula.py:690`; `app/ponto/portas/celula.py:711`; `app/ponto/portas/celula.py:732`). Opus resolve as vedações ao Codex (`app/docs/LEIS.md:147`). **A O10-D está em BACKLOG:2198; :2199 é RETRATO-POR-EVENTO** (`app/docs/BACKLOG.md:2198`; `app/docs/BACKLOG.md:2199`).

2. **Cortes: preservar estas condições vigentes.** Suite de não-pouso lateral vai à PISTA; VM fica para espinha, pouso e push, com exceção por indisponibilidade registrada. O item 7 não dispensa esse roteamento (`app/docs/CORTES.md:490`; `app/docs/RELATO.md:102`). A F2 pode abrir após B2 sem esperar CLASSE-D (`app/docs/RELATO.md:4`). O brief registrado da CLASSE-D trata C5 por dia/dono; LU2-07 e LU2-08 conservam questões abertas, que esta obra não resolve (`app/docs/RELATO.md:421`; `app/docs/RELATO.md:354`; `app/docs/RELATO.md:352`; `app/docs/RELATO.md:422`). Leitura integral dos briefs físicos e cruzamento deles com a futura lista O10-D: **[nao verificado]**.

3. **Literal: as condições futuras ainda não estão demonstradas.** Lista, diff da B2 contra sua base, interseções, vaga e fila atual: **[nao verificado]**. O wrapper oferece titular e `flock`, sem interface de fila ou prioridade; ceder à espinha depende do despacho (`bin/trava_teste.sh:28`; `bin/trava_teste.sh:35`; `app/docs/RELATO.md:6`). d1=d2 exige caso efetivo: hoje as duas chamadas repetem coordenadas e escrevem trabalha/folga usando DNA do vínculo próprio; aceitar datas iguais sozinho não prova troca de turnos (`app/ponto/portas/celula.py:711`; `app/ponto/portas/celula.py:622`; `app/ponto/portas/celula.py:629`). Permanecem obrigatórios quatro controles antes da primeira escrita, recusa falante, ID próprio e escala-base intocada (`app/docs/BACKLOG.md:2198`).

4. **Dinheiro: o caminho alcança recálculo; alcance real fora do declarado não está provado.** A edição agenda fechamento no commit (`app/ponto/portas/celula.py:665`; `app/ponto/portas/celula.py:496`; `app/ponto/services/fechamento.py:1824`). A guarda barra lavrada, mas o TOCTOU permanece registrado (`app/ponto/portas/celula.py:606`; `app/docs/BACKLOG.md:2197`). Competência paga também conserva exigência de `!`, mesmo sem exportação (`CLAUDE.md:404`). Rollback SQL ainda não prova reversão integral de efeitos externos: há caminho de rejulgamento síncrono até notificação imediata (`app/ponto/portas/celula.py:472`; `app/ponto/services/cartorio.py:1791`; `app/ponto/management/commands/detectar_ausencias.py:779`). Valores alterados, exportada atingida ou notificação efetivamente enviada: **[nao verificado]**.

**VEREDITO: PROCEDE (item 2: corrigir BACKLOG:2199 para BACKLOG:2198; a linha citada pertence a outra obra — app/docs/BACKLOG.md:2199).**

## 17/11 01:21 · sessao aa74e1dc86c0 · PREPARA-F3-F5-F6-F8-F9 · leitura · origem=Code

## 1 DEPENDE e premissas

**A F1 pressuposta pela F3 caiu como contrato disponível.** A BASE fornece `pergunta_respondida(p)`, que exige instante **e** texto da projeção; destina à B2 os demais leitores. Isso não fornece a antiga `leitura(p) → LeituraDosAtos`. (`app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1551`, `app/chamados/juizes.py:1552`)

Identificação recebida da cópia: `fbc5c76ced33bf25943f3a5bc78ebff257d68c56`. Confirmação independente do SHA do HEAD, ancestralidade e diffs históricos: **[nao verificado]**. Banco, logs, execução dos testes e números atuais de frota: **[nao verificado]**.

As referências `brief.md` e `matriz.md` abaixo numeram o texto fornecido, começando pelo título como linha 1; correspondência com os arquivos externos originais: **[nao verificado]**. **CAIU** significa que a premissa não pode ser reutilizada para orientar esta BASE; não desmente sua cópia histórica.

**Cada ocorrência literal de `[DEPENDE-F1]`:**

| Origem | Premissa | Julgamento na BASE |
|---|---|---|
| `brief.md:57` | Guarda sem resposta passa a `leitura().resposta` | **CAIU.** Juiz disponível exige o par projetado; guarda atual testa somente instante. (`app/chamados/juizes.py:1552`, `app/chamados/services/validacao.py:594`) |
| `brief.md:58` | Validação corrente vem de `leitura().instante_validacao` | **CAIU.** A porta continua lendo V; a interface pressuposta não é o juiz pousado. (`app/chamados/services/validacao.py:597`, `app/chamados/juizes.py:1544`) |
| `brief.md:59` | Recusar consulta validação/resposta da antiga leitura | **CAIU.** Guardas atuais consultam V/R projetados. (`app/chamados/services/validacao.py:810`, `app/chamados/services/validacao.py:813`) |
| `brief.md:66` | Ações recebem aquela leitura | **CAIU.** Reabrir, regularizar e corrigir continuam consultando campos. (`app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/acoes_disputa.py:132`, `app/chamados/services/acoes_disputa.py:347`) |
| `brief.md:67` | Classe, aceite de folga e idade recebem resposta/instante do fold | **CAIU.** Continuam lendo T/R. (`app/chamados/services/validacao.py:380`, `app/chamados/services/validacao.py:1094`, `app/chamados/services/validacao.py:1167`) |
| `brief.md:85` | DP RESPOSTA com projeção vazia basta para validar | **CAIU como oráculo da F1 pousada.** O juiz atual exige R/T projetados. (`app/chamados/juizes.py:1552`) |
| `brief.md:87` | Ato presente torna projeção vazia equivalente à cheia | **CAIU como contrato disponível.** Classe e ações não recebem o fold antigo. (`app/chamados/services/validacao.py:1094`, `app/chamados/services/acoes_disputa.py:355`) |
| `brief.md:129` | Esperar a F1 do contrato `leitura` resolve a dependência | **CAIU.** O contrato pousado é outro; B2 permanece explicitamente encarregada dos leitores. (`app/chamados/juizes.py:1550`, `app/chamados/juizes.py:1551`) |
| `matriz.md:9` | `_leitura_conservada`, `_leitura_da_pergunta`, `_q_resposta_do_ato` são as autoridades atuais | **CAIU.** Projeção temporal e juiz de respondida são os contratos atuais. (`app/chamados/juizes.py:1382`, `app/chamados/juizes.py:1544`) |
| `matriz.md:16` | D1 recebe a antiga leitura | **CAIU.** (`app/chamados/services/validacao.py:594`, `app/chamados/juizes.py:1552`) |
| `matriz.md:21` | A F1 determina a conservação usada pela fixture | **CAIU a dependência da F1.** O helper de correspondência existe no model, independentemente daquela API. (`app/chamados/models.py:4081`, `app/chamados/models.py:4094`) |
| `matriz.md:24` | D2 recebe validação corrente do fold | **CAIU.** (`app/chamados/services/validacao.py:597`, `app/chamados/juizes.py:1544`) |
| `matriz.md:29` | D3 recebe validação corrente do fold | **CAIU.** (`app/chamados/services/validacao.py:810`, `app/chamados/juizes.py:1544`) |
| `matriz.md:33` | D4 recebe resposta corrente do fold | **CAIU.** (`app/chamados/services/validacao.py:813`, `app/chamados/juizes.py:1552`) |
| `matriz.md:44` | D7 pode migrar para aquele juiz de validação | **CAIU o suporte pressuposto.** Os dois leitores continuam projetivos. (`app/chamados/services/validacao.py:975`, `app/chamados/services/validacao.py:996`) |
| `matriz.md:49` | D8 recebe texto do fold | **CAIU.** (`app/chamados/services/validacao.py:1094`) |
| `matriz.md:53` | D9 recebe texto do fold | **CAIU.** (`app/chamados/services/validacao.py:380`) |
| `matriz.md:57` | D10 recebe instante do fold | **CAIU.** (`app/chamados/services/validacao.py:1167`) |
| `matriz.md:67` | Leituras internas da devolução recebem `motivo_recusa`/V do fold | **CAIU.** Há refresh e leitura dos campos persistidos. (`app/chamados/services/validacao.py:262`, `app/chamados/services/validacao.py:266`, `app/chamados/services/validacao.py:269`) |
| `matriz.md:101` | D16 recebe instante do fold | **CAIU.** (`app/chamados/services/acoes_disputa.py:97`) |
| `matriz.md:103` | D17 recebe instante do fold | **CAIU.** (`app/chamados/services/acoes_disputa.py:132`) |
| `matriz.md:106` | D18 recebe V/T do fold | **CAIU.** (`app/chamados/services/acoes_disputa.py:347`, `app/chamados/services/acoes_disputa.py:355`) |
| `matriz.md:128` | Contrato fechado D1–D4/D7/D18 dispõe de `atos_pergunta.leitura` | **CAIU.** O contrato atual não fornece esse redutor. (`app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1551`) |
| `matriz.md:129` | Contrato fechado D8–D10/D16–D18 dispõe daquela leitura | **CAIU.** (`app/chamados/juizes.py:1544`, `app/chamados/services/validacao.py:1094`) |
| `matriz.md:141` | Os onze degraus estão liberados pela antiga F1 | **CAIU o suporte comum.** A enumeração histórica de onze degraus não demonstra disponibilidade da interface. (`app/chamados/juizes.py:1550`, `app/chamados/juizes.py:1551`) |

**Demais premissas dos anexos:**

| Origem | Premissa | Julgamento |
|---|---|---|
| `brief.md:10–15`; `matriz.md:3–5` | Offsets e ausência de mudanças nas cópias antigas podem ser transportados | **CAIU para esta BASE.** Validar está em `:574`, recusar em `:770`; ações ganharam linhas. Os diffs históricos permanecem **[nao verificado]**. (`app/chamados/services/validacao.py:574`, `app/chamados/services/validacao.py:770`, `app/chamados/services/acoes_disputa.py:92`) |
| `brief.md:15`; `matriz.md:16–113` | Os leitores centrais ainda leem projeção | **CONFERE.** (`app/chamados/services/validacao.py:594`, `app/chamados/services/verbos.py:264`, `app/chamados/services/painel.py:67`, `app/chamados/services/acoes_disputa.py:347`, `app/chamados/veredito_lavrado.py:207`) |
| `brief.md:16–18`; `matriz.md:37` | Recusa fora do fato já está curada | **CONFERE.** Retorna antes da escrita; casca traduz como informação. (`app/chamados/services/validacao.py:817`, `app/chamados/services/acoes_disputa.py:87`) |
| `brief.md:19–21`; `matriz.md:61,133` | A20 fica nos campos próprios | **CONFERE.** Isso não comprova implementação integral da LU-08, que limita a conferência ao par conferido: **[nao verificado]**. (`app/chamados/services/validacao.py:905`, `app/chamados/services/validacao.py:907`, `app/docs/CORTES.md:466`) |
| `brief.md:61` | F1 fornece materialização conservada por parâmetro | **CAIU.** A projeção temporal lê M atual; não recebe esse parâmetro. A permanência de M no escopo conservado continua expressa nos anexos. (`app/chamados/juizes.py:1382`, `app/chamados/juizes.py:1390`; `matriz.md:133`) |
| `brief.md:22,66`; `matriz.md:104,107,133` | POST e fechamento da disputa não são projeções da resposta | **CONFERE.** (`app/chamados/services/acoes_disputa.py:83`, `app/chamados/services/acoes_disputa.py:125`, `app/chamados/services/acoes_disputa.py:257`, `app/chamados/services/acoes_disputa.py:349`) |
| `brief.md:32–39` | A enumeração antiga esgota escritores | **CAIU.** Há lote, nascimento com `**campos`, ressuscitação e devolução BUG-PAR. (`app/chamados/models.py:1629`, `app/chamados/models.py:3124`, `app/chamados/models.py:3393`, `app/chamados/services/validacao.py:349`) |
| `brief.md:38` | `registrar` é a porta produtiva de escrita DP | **CONFERE no censo estático.** (`app/chamados/models.py:4110`, `app/chamados/models.py:4120`) |
| `brief.md:40–45`; `matriz.md:123–125` | `carimbar('validar')` pode escrever V sem VALIDACAO | **CONFERE.** O corpo atribui e salva sem registrar esse ato; absorção registra ABSORCAO. Frota: **[nao verificado]**. (`app/chamados/models.py:3245`, `app/chamados/models.py:3262`, `app/chamados/services/materializacao.py:1043`, `app/chamados/services/materializacao.py:1045`) |
| `brief.md:46` | Nenhum leitor F3 é sem janela | **CAIU como afirmação geral.** Marcador agrega o acervo; `marcos_do_lote(ate=None)` também. Isso não transforma esses leitores em contadores D04. (`app/chamados/services/painel.py:66`, `app/chamados/veredito_lavrado.py:163`) |
| `brief.md:98–101,129` | BUG-PAR ainda precisa pousar | **CAIU.** Seus corpos estão presentes, e há registro documental do pouso c4+c4c. (`app/chamados/services/validacao.py:308`, `app/chamados/services/validacao.py:526`, `app/docs/BACKLOG.md:2084`) |
| `brief.md:98–101` | BUG-PAR acrescenta espera, irmã, par, devolução e mensagem real | **CONFERE.** (`app/chamados/services/validacao.py:354`, `app/chamados/services/validacao.py:554`, `app/chamados/services/validacao.py:611`, `app/chamados/services/validacao.py:638`, `app/chamados/services/acoes_disputa.py:63`) |
| `brief.md:102` | DEVOLUCAO entra automaticamente em D04/D06 | **CAIU como descrição atual.** D04 pretendido conta **VALIDACAO**, não soma DEVOLUCAO como validação; os marcadores atuais nem consultam DP. (`app/docs/BACKLOG.md:2091`, `app/chamados/services/verbos.py:263`, `app/chamados/services/painel.py:66`, `app/chamados/veredito_lavrado.py:202`) |
| `brief.md:103–105` | Ausência de conflito das cópias F1/F2/E3/LOTE7/UI-CAMERA/RELAVRA serve como garantia atual | **CAIU como garantia reutilizável.** Diffs dessas raias: **[nao verificado]**; B2 fornecida cruza três arquivos centrais, conforme parte 6. (`app/chamados/services/validacao.py:594`, `app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/verbos.py:335`) |
| `matriz.md:101` | `total_pend` conta apenas corrigíveis | **CAIU.** Conta todas as perguntas da disputa; corrigíveis são buscadas depois. (`app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/acoes_disputa.py:99`) |
| `matriz.md:54,121` | Existem dois chamadores externos de `_e_aceite_de_folga` nos offsets antigos | **CAIU.** O sítio atual importa em `:247` e chama em `:259`. (`app/chamados/services/vigia_fio.py:247`, `app/chamados/services/vigia_fio.py:259`) |
| `matriz.md:19–21,79–81` | As fixtures indicadas não escrevem DP explicitamente | **CONFERE.** Usam `objects.create`; o `save` não chama o conservador. D12 também fornece texto, não somente instante. `_nascer`, outra porta, registra conservação explicitamente. (`app/chamados/tests/test_service_validar_puro.py:47`, `app/chamados/tests/test_placar_aceite.py:35`, `app/chamados/models.py:3550`, `app/chamados/models.py:3129`) |
| `matriz.md:139–141` | Os quinze NENHUM continuam sendo o censo de provas | **CAIU.** Foram encontradas provas adicionais, discriminadas na parte 3. (`app/chamados/tests/test_validar_lote.py:131`, `app/chamados/tests/test_fila_validar_lote.py:42`, `app/chamados/tests/test_guard_s116_detalhe_local.py:52`, `app/escala/tests/test_signal_ancora.py:218`) |

A contagem seguinte considera **as 25 ocorrências literais da marca**, incluindo repetições contratuais e resumo; as demais premissas foram julgadas separadamente nas linhas acima. (`brief.md:57,58,59,66,67,85,87,129`; `matriz.md:9,16,21,24,29,33,44,49,53,57,67,101,103,106,128,129,141`)

DEPENDE CAIU 25 de 25

## 2 Censo escritores e leitores

Censo estático por grep/AST, incluindo atributos, aliases, keywords e strings ORM; templates foram varridos textualmente. Completude para nomes inteiramente construídos em execução: **[nao verificado]**. **NOVO** significa ausente da enumeração do brief, sem afirmar nascimento recente.

Nesta seção, caminhos abreviados são relativos a `app/`. R=`respondida_em`; T=`resposta_colab`; V=`validada_em`; VP=`validada_por[/_id]`; C=`materializada_em`; M=`recusa_motivo`; DC=`data_conferida_*`.

**Escritores físicos e simulações:**

| Campo | Escritores encontrados |
|---|---|
| R/T | Resposta manual `chamados/services/disputa_emissao.py:1481,1482,1483`; automática `:1674,1675,1677`; reparo da trilha, somente R, `:1696,1699`; reabertura via lote `:1609,1648`. |
| R/T — **NOVO** | Ressuscitação `chamados/models.py:1629,1630`; nascimento por `**campos`, `:3124,3127`. |
| V/VP | Validação `chamados/services/validacao.py:168,169,171`; devolução `:273,274,276`; **NOVO BUG-PAR** `:349,350,351`; nova resposta limpa V/VP em `chamados/services/disputa_emissao.py:1494,1495`; reabertura em lote `:1609,1648`; ressuscitação `chamados/models.py:1629`; carimbo `:3245,3246`; nascimento `:3124`. |
| C | `_escrever_carimbo`, `chamados/models.py:3209`; persistências individuais `:3262,3299,3322`; **NOVO** lote direto `:3391,3393`; nascimento `:3124`. |
| M — **NOVO no censo** | Escape humano `chamados/services/disputa_emissao.py:1511,1513`; motivo técnico `chamados/services/materializacao.py:1332,1333`; recusa terminal `:1665,1666`; identidade `chamados/models.py:3298,3299`; recusa administrativa `chamados/services/validacao.py:827,833`; nascimento `chamados/models.py:3124`. |
| DC | Conferência `chamados/services/validacao.py:907,908,909`. |
| Memória, sem persistência por essas atribuições | Simulação da ressuscitação `chamados/models.py:1554,1555,1556`; rollback do carimbo `:3273,3274,3275`. |
| Disputa: fechamento/resolução | Fechar `chamados/models.py:2013,2015`; reabrir admin `:2230,2232`; reabrir sistema `:2254,2256`; complemento de resolução `:2346`. |
| DP | Porta produtiva `chamados/models.py:4110`, com `bulk_create` interno em `:4120`; alteração de linha existente é recusada em `:4070`. |

`regularizacao_ext.py:199` escreve C de **outro modelo**, `RegularizacaoExterna`; não é escritora de C da pergunta. (`chamados/services/regularizacao_ext.py:199`, `chamados/models.py:4152`)

**Produtores e leitores de DP:**

| Papel | Sítios |
|---|---|
| Produtores já nomeados | `chamados/models.py:1613,2005,2137,3129`; `chamados/services/disputa_emissao.py:1525,1631,1678,1699`; `chamados/services/validacao.py:173,279`; `chamados/services/materializacao.py:1045`. |
| Produtores **NOVOS** | `chamados/services/validacao.py:354`; `chamados/management/commands/backfill_fio_morto_e4b.py:141`; `chamados/management/commands/backfill_decisoes_pergunta.py:324`; `ponto/services/veredito_celula.py:195`. |
| Leitores | Histórico `chamados/models.py:3675`; correspondência `:4094`; **NOVOS** rótulo BUG-PAR `chamados/services/materializacao.py:275`, prefetch `chamados/services/fio.py:263`, tarefa `chamados/services/tarefa_do_dia.py:158,159,178,421,430`, foto `chamados/services/foto_e4b.py:280`, medidor `chamados/management/commands/backfill_decisoes_pergunta.py:99,315`, histórico HTML `templates/chamados/partials/modal_fio.html:205,210,212`. |

As chamadas ao conservador podem registrar assinaturas anteriores; não representam necessariamente um gesto humano novo. A correspondência é pergunta + espécie de ato + instante. (`chamados/models.py:4087`, `chamados/models.py:4094`, `chamados/models.py:4098`, `chamados/models.py:4102`)

**Leitores nos cinco arquivos centrais:**

| Arquivo/função | Leituras atuais |
|---|---|
| `validacao.py` — guardas | R `chamados/services/validacao.py:594,813`; V `:597,810`; C `:681,717`. |
| **NOVO** fotografia anterior | V/VP/M e fechamento da disputa em `chamados/services/validacao.py:181,182`; consumidores em `:268,294,627`. |
| Devolução recusada | Refresh dos campos `chamados/services/validacao.py:262`; C `:264`; M `:266`; V `:269`; T/M na DP `:281,282`; fechamento `:288,291,293`; fotos/trilha `:300,301,302`. |
| **NOVO** devolução BUG-PAR | C/VP/V/M `chamados/services/validacao.py:341,342,343,344`; T `:356`; valores posteriores `:363,364`. |
| **NOVO** par | V da irmã determina se haverá nova VALIDACAO em `chamados/services/validacao.py:553,554`; count de duas C em `:556`. |
| Aceite de folga | T `chamados/services/validacao.py:380`; chamador interno `:623`. |
| **NOVO no brief** lote | V em `chamados/services/validacao.py:975,996`. |
| Classe | T `chamados/services/validacao.py:1094,1096,1098,1103,1104,1105,1106,1109`. |
| Fila | R para idade `chamados/services/validacao.py:1167`. |
| Recusa: foto/motivo | T/R/M `chamados/services/validacao.py:823,824,825,843,847,848`. |
| Conferência | DC `chamados/services/validacao.py:905,907,908,909`. |
| `acoes_disputa.py` | R `chamados/services/acoes_disputa.py:97,132`; V `:347`; T `:355`; fechamento da disputa `:257,349`. POST M/resolução em `:83,125` são homônimos. |
| `verbos.py` | V/VP `chamados/services/verbos.py:264,265,266,268,270`; R `:335,336,342,345`. |
| `painel.py` | R/V/C no agregado `chamados/services/painel.py:67`; VP `:262`; V nas janelas `:270,271,272,273`; **NOVO** transporte das janelas pelo modo resumo `:530,535`. |
| `veredito_lavrado.py` | C `chamados/veredito_lavrado.py:203,204`; R `:207,208`. |

**Chamadores das funções alcançadas:**

| Função | Chamadores produtivos |
|---|---|
| `validar_pergunta` | `chamados/services/acoes_disputa.py:19,381`; **NOVOS** `api/foto_recebida.py:51`, `chamados/services/declaracao_texto.py:696`, `chamados/management/commands/validar_classe_a.py:100`. |
| `recusar_pergunta` | `chamados/services/acoes_disputa.py:82`. |
| `_carimbar_validacao` | `chamados/services/validacao.py:432,464,554,655`. |
| `devolver_validacao_recusada` | `chamados/services/validacao.py:693`. |
| **NOVO** `devolver_paridade_bug_par_103` | `chamados/management/commands/bug_par_103.py:106`. |
| `_e_aceite_de_folga` | `chamados/services/validacao.py:623`; **NOVO** `chamados/services/vigia_fio.py:259`. |
| **NOVO** `validar_lote_classe_a` | `chamados/views_cobrar.py:261,381`; `core/views_ia.py:169`. |
| `classe_do_lote` | `chamados/services/validacao.py:978,1165`; **NOVO** `api/views_mensageria.py:545`. |
| **NOVO** `fila_validar_classificada` | `chamados/views_cobrar.py:288,349,361`. |
| **NOVO** `conferir_data_do_fato` | `chamados/views.py:1312`. |
| `placar_equipe` | `chamados/views.py:1486`. |
| `placar_aceite` | `chamados/services/painel.py:298`; `chamados/views.py:1488`; `folha/views_retrato.py:515`. |
| `marcador_do_placar` / `etag_do_placar` | `chamados/services/painel.py:91`; `chamados/views.py:820`. |
| `montar_contexto_painel` | `chamados/views.py:727,852`; `core/management/commands/lavrar_badge_navbar.py:39`; `chamados/management/commands/backfill_fio_morto_e4b.py:72`. |
| `marcos_do_lote` | `chamados/services/raiox.py:38`; alias no fio `chamados/services/fio.py:281,282`; `chamados/services/tarefa_do_dia.py:434`; `chamados/management/commands/auditar_invariantes_chamados.py:1163`. |
| `acao_validar_pergunta` | `chamados/views.py:501`; `chamados/services/fio.py:655`; `chamados/views_cobrar.py:208`; `chamados/services/validacao.py:989`. |
| `acao_recusar_pergunta` | `chamados/views.py:518`; `chamados/services/fio.py:684`. |
| `acao_reabrir_disputa` | `chamados/services/fio.py:690`. |
| `acao_regularizar_disputa` | `chamados/views.py:475`. |
| `acao_corrigir_resposta` | `chamados/views.py:536`; `chamados/services/fio.py:685`. |

**Censo global adicional dos campos — NOVO na enumeração do brief.** Inclui leitura, filtro, fotografia e transporte; uma ocorrência não significa, por si, julgamento de estado.

| Arquivo | Campos e sítios |
|---|---|
| `api/views.py` | T `:1163,2561`; R `:1165,2563,2577,2620,2637`. |
| `api/views_mensageria.py` | T `:567`; R `:569,570`. |
| `chamados/canal.py` | T `:65`. |
| `chamados/juizes.py` | C `:292,683,711,858,1390,1502`; R `:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753`; T `:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052`; V `:856,1389`. |
| `chamados/models.py` | R `:1614,1924,2949,3155,3158,3237,3241,3243,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100`; T 

[truncado em 20000 de 64284 caracteres]

## 17/11 01:21 · sessao 5534e82872ea · PREPARA-F3-F5-F6-F8-F9 · leitura · origem=Code

## 1 DEPENDE e premissas

**A premissa central caiu:** a F1 disponível declara `pergunta_respondida(p)`, baseada em instante **e** texto da projeção. Ela não fornece o contrato `LeituraDosAtos` pressuposto pelos anexos; o próprio código destina a migração dos leitores B/C/D à B2. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`; `app/chamados/juizes.py:1552`)

BASE identificada pelo despacho: `fbc5c76ced33bf25943f3a5bc78ebff257d68c56`. Confirmação independente do HEAD, ancestralidade e diffs históricos: **[nao verificado]**. Resultados de banco, sondas e testes nesta entrega: **[nao verificado]**.

As referências `brief:N` e `matriz:N` contam as linhas dos anexos fornecidos, incluindo vazias, começando no título. A contagem abaixo considera **cada ocorrência literal de `[DEPENDE-...]`**; premissas sem marca são julgadas separadamente. Correspondência com os arquivos originais externos: **[nao verificado]**.

| Marca no anexo | Premissa julgada | Veredito e prova na BASE |
|---|---|---|
| brief:15 — U1 | F1 fornece texto/instante pelo ato | **CAIU.** A autoridade pousada julga o par projetado; serializer ainda lê os campos. (`app/chamados/juizes.py:1552`; `app/api/views_mensageria.py:567`; `app/api/views_mensageria.py:569`) |
| brief:16 — U2 | Mesmo contrato para chamados/v2 | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views.py:1163`; `app/api/views.py:1165`) |
| brief:17 — U3 | Mesmo contrato para disputa/perguntas | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views.py:2561`; `app/api/views.py:2563`) |
| brief:19 — U5 | Filtro passa a usar o antigo redutor | **CAIU.** Juiz vigente exige o par; filtro atual exige somente instante. (`app/chamados/juizes.py:1552`; `app/api/views.py:2577`) |
| brief:20 — U6 | `ja_respondida` usa o antigo redutor | **CAIU.** (`app/chamados/juizes.py:1552`; `app/api/views.py:2620`) |
| brief:22 — U8 | Amostra usa texto do ato fornecido pela F1 | **CAIU.** (`app/chamados/juizes.py:1544`; `app/chamados/views_cobrar.py:291`) |
| brief:23 — U9 | Resposta web usa o antigo redutor | **CAIU.** (`app/chamados/juizes.py:1552`; `app/chamados/views.py:1280`) |
| brief:25 — U11 | Face SQL conserva P5 pelo ato | **CAIU.** A face vigente exige instante e texto não vazio. (`app/chamados/juizes.py:1575`; `app/chamados/juizes.py:1580`) |
| brief:26 — U12 | Tag recebe leitura do ato da F1 | **CAIU.** Tag ainda testa texto projetado e pede classificação. (`app/chamados/templatetags/chamados_tags.py:166`; `app/chamados/templatetags/chamados_tags.py:171`) |
| brief:66 | `L = atos_pergunta.leitura` é autoridade disponível | **CAIU.** Contrato disponível: `pergunta_respondida`. (`app/chamados/juizes.py:1544`) |
| brief:132 | D01 é leitor mecânico sem registro de juiz | **CAIU.** O juiz de respondida está registrado. (`app/core/juizes.py:170`; `app/core/juizes.py:306`) |
| matriz:8 | F1 vigente é a interface de `be2b8d733` | **CAIU como contrato da BASE.** (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`) |
| matriz:9 — `[DEPENDE-R3]` | Ainda é necessário aguardar o pouso da R3 | **CAIU.** Há registro de R3 c17 no ar; ancestralidade do hash antigo: **[nao verificado]**. (`app/docs/BACKLOG.md:770`; `app/docs/RELATO.md:243`) |
| matriz:9 — `[DEPENDE-F2]` | Continua dependendo dos leitores de estado/display da F2 | **CONFERE como dependência de interface.** Estado/display continuam projetivos; presença histórica de `8c6577245`: **[nao verificado]**. (`app/chamados/models.py:3151`; `app/chamados/models.py:3688`) |
| matriz:32 — U1a | Texto por `L.resposta` | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views_mensageria.py:567`) |
| matriz:33 — U1b, primeira marca | Instante por `L.instante_resposta` | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views_mensageria.py:569`) |
| matriz:33 — U1b, segunda marca | P5 entra na fila F1 e derruba o lote | **CAIU hoje.** A fila exige instante não nulo. (`app/chamados/juizes.py:1751`; `app/chamados/juizes.py:1580`) |
| matriz:34 — U1c | P5 chega à subtração pela fila F1 | **CAIU hoje.** A subtração continua insegura para chamada direta com `None`. (`app/chamados/juizes.py:1751`; `app/api/views_mensageria.py:570`) |
| matriz:35 — U2, `[DEPENDE-F1]` | Texto/instante pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views.py:1163`; `app/api/views.py:1165`) |
| matriz:35 — U2, `[DEPENDE-F2]` | `resposta_display` permanece interface da F2 | **CONFERE.** A API consome essa property, ainda projetiva. (`app/api/views.py:1164`; `app/chamados/models.py:3688`) |
| matriz:36 — U3 | Texto/instante pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1544`; `app/api/views.py:2561`; `app/api/views.py:2563`) |
| matriz:37 — U4 | Migração de T depende daquela F1 | **CAIU o vínculo com o contrato antigo; CONFERE T como autoridade existente.** (`app/chamados/juizes.py:1544`; `app/chamados/services/tarefa_do_dia.py:142`; `app/docs/CORTES.md:488`) |
| matriz:38 — U5, primeira marca | Respondidas por `L` | **CAIU.** (`app/chamados/juizes.py:1552`; `app/api/views.py:2577`) |
| matriz:38 — U5, segunda marca | P5 pode contar como respondida conservada | **CAIU.** Texto sem instante é par ilegal e não é respondida. (`app/chamados/juizes.py:1541`; `app/chamados/juizes.py:1552`; `app/docs/LEIS.md:151`) |
| matriz:39 — U6 | `ja_respondida` por `L.instante_resposta/L.resposta` | **CAIU.** A pergunta “respondida?” tem juiz único atual. (`app/chamados/juizes.py:1544`; `app/api/views.py:2620`) |
| matriz:40 — U7a, primeira marca | Seleção depende daquela interface F1 | **CAIU.** O juiz atual está disponível; o filtro segue só por instante. (`app/chamados/juizes.py:1552`; `app/chamados/views_cobrar.py:169`) |
| matriz:40 — U7a, segunda marca | P5 é resposta elegível pela F1 | **CAIU.** (`app/chamados/juizes.py:1541`; `app/chamados/juizes.py:1552`) |
| matriz:41 — U7b, primeira marca | T depende daquela interface F1 | **CAIU o vínculo antigo; CONFERE T existente.** (`app/chamados/services/tarefa_do_dia.py:142`; `app/chamados/juizes.py:1544`) |
| matriz:41 — U7b, segunda marca | Alteração do contador é consequência daquela F1 | **CAIU essa causa.** O universo cru, incluindo mortas, continua demonstrado no próprio filtro. (`app/chamados/views_cobrar.py:175`; `app/chamados/views_cobrar.py:176`) |
| matriz:42 — U8 | Texto pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1544`; `app/chamados/views_cobrar.py:291`) |
| matriz:43 — U9a | Bool pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1552`; `app/chamados/views.py:1280`) |
| matriz:45 — U10, primeira marca | Corrigíveis por `L` da F1 antiga | **CAIU.** Corrigíveis ainda exige instante não nulo. (`app/chamados/juizes.py:1744`) |
| matriz:45 — U10, segunda marca | P5 entra em corrigíveis e troca o escritor | **CAIU hoje.** O filtro atual exclui P5. (`app/chamados/juizes.py:1744`; `app/chamados/views.py:1420`) |
| matriz:46 — U11 | Vazio por `L.resposta` | **CAIU.** (`app/chamados/juizes.py:1544`; `app/chamados/canal.py:65`) |
| matriz:47 — U12 | Tag pelo antigo redutor | **CAIU.** (`app/chamados/juizes.py:1544`; `app/chamados/templatetags/chamados_tags.py:166`) |
| matriz:57 | Impactos P5 só podem ser medidos depois daquela F1 | **CAIU.** Os predicados atuais e as filas reais permitem medir P5 agora. (`app/chamados/juizes.py:1537`; `app/chamados/juizes.py:1742`; `app/chamados/juizes.py:1749`) |
| matriz:64 | Contrato de saída inteiro usa leitura dos atos F1 | **CAIU como interface disponível.** (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`) |
| matriz:68 | RED P5/`None` decorre do universo F1 | **CAIU essa entrada pela fila atual.** (`app/chamados/juizes.py:1580`; `app/chamados/juizes.py:1751`) |
| matriz:87 | P5 é resposta conservada válida para lista/correção | **CAIU quanto a “respondida”.** (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1552`) |
| matriz:98 | 500 no lote atual por P5 | **CAIU.** Serializer inseguro permanece, mas a fila exclui a entrada nula. (`app/api/views_mensageria.py:569`; `app/chamados/juizes.py:1751`) |

**Premissas sem marca:**

| Origem | Julgamento |
|---|---|
| brief:3; matriz:5–7 — bases e offsets antigos orientam a leitura atual | **CAIU como referência de localização.** U12 mudou para `:166`, e o modal chama o filtro em `:274`. Igualdade histórica dos blobs: **[nao verificado]**. (`app/chamados/templatetags/chamados_tags.py:166`; `app/templates/chamados/partials/modal_fio.html:274`) |
| matriz:12–19 — assinatura, leitura conservada, P5, ordenação e classificação pelo ato | **CAIU esse conjunto contratual.** Fila ordena por `respondida_em`; classificação lê texto projetado. (`app/chamados/juizes.py:1753`; `app/chamados/juizes.py:2030`; `app/chamados/juizes.py:1552`) |
| brief:18; matriz:37,65–66 — `aguarda_admin` já pode ser descrito por uma tabela própria de estados | **CAIU.** O corte exige ler o dono declarado pela tarefa, sem tabela própria no leitor. (`app/docs/CORTES.md:488`) |
| brief:20,65,138 — F6 inteira é leitura, sem persistência | **CAIU.** U6/U9 afetam auditoria/followup; U7 chama validação; U10 escolhe recobrar ou reabrir. (`app/chamados/services/disputa_emissao.py:1546`; `app/chamados/services/disputa_emissao.py:1569`; `app/chamados/views_cobrar.py:208`; `app/chamados/views.py:1425`; `app/chamados/views.py:1436`) |
| brief:44–47 — zero atribuições físicas dos campos nos sete arquivos | **CONFERE para os campos censados; não elimina persistência delegada.** Os sítios são leituras e chamadas de portas. (`app/api/views.py:2620`; `app/api/views.py:2627`; `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:208`; `app/chamados/views.py:1280`; `app/chamados/views.py:1283`) |
| brief:29–31; matriz:48–49 — S1/S2 SOME | **CONFERE.** Delegam; o complemento também os exclui da seleção RED. (`app/chamados/relatorios_disputa.py:24`; `app/chamados/relatorios_disputa.py:26`; `app/api/views.py:1179`; `app/docs/CORTES.md:491`) |
| brief:33–35; matriz:44 — U9b inalcançável | **CONFERE por leitura:** a primeira guarda elimina a mesma pergunta morta exigida depois. (`app/chamados/views.py:1242`; `app/chamados/views.py:1243`; `app/chamados/views.py:1257`) |
| brief:34,79,99 — morta na web retorna HTTP 404 | **CAIU.** Web chama `resposta_acao` com erro/redirecionamento; API retorna 404 explícito. (`app/chamados/views.py:1244`; `app/chamados/views.py:1246`; `app/api/views.py:2619`) |
| brief:37–38 — `fechada_em` permanece D02 | **CONFERE a separação dos leitores indicados.** (`app/chamados/views_cobrar.py:161`; `app/chamados/views_cobrar.py:164`; `app/chamados/relatorios_disputa.py:46`) |
| brief:51; matriz:75–76 — contrato público conserva chaves/fases | **CONFERE a regra escrita.** Não autoriza trocar fases por exigibilidade. (`app/docs/BACKLOG.md:2091`; `app/api/views.py:2552`; `app/api/views.py:2553`) |
| brief:53–54 — partição do app ainda lê instante/disputa aberta | **CONFERE.** Propriedade da futura F5 pelo brief dela: **[nao verificado]**. (`app/chamados/services/pergunta_no_app.py:76`; `app/chamados/services/pergunta_no_app.py:81`) |
| brief:71,95–96 — U12 mostra texto “X” ou “sem texto” | **CAIU.** A tag devolve `impl/fr`; vazio devolve ambos nulos. (`app/chamados/templatetags/chamados_tags.py:167`; `app/chamados/templatetags/chamados_tags.py:175`; `app/chamados/templatetags/chamados_tags.py:176`) |
| brief:74; matriz:84–86 — estados de validação sem lavra podem ser agrupados com efeito pendente | **CAIU esse agrupamento.** T distingue sem lavra, materializar, rejulgar e encerrada; o corte manda sem dono=False. (`app/chamados/services/tarefa_do_dia.py:169`; `app/chamados/services/tarefa_do_dia.py:172`; `app/chamados/services/tarefa_do_dia.py:174`; `app/docs/CORTES.md:488`) |
| brief:77,127–128; matriz:69 — passivo conserva apresentação | **CONFERE a conservação; não confirma o número histórico.** T retorna desconhecimento, e os leitores atuais preservam campos. (`app/chamados/services/tarefa_do_dia.py:155`; `app/api/views.py:2561`; `app/api/views.py:2563`; `app/docs/BACKLOG.md:2091`) |
| brief:83–98 — divergência ato/projeção basta como oráculo R1/R3/R4/R5 | **CAIU como RED automaticamente autorizado.** L-130 julga o par atual; o redutor pressuposto não é o contrato disponível. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`) |
| brief:107–114 — interseções dos hashes antigos, inclusive RELAVRA-11 | **[nao verificado] por diff histórico.** Os sítios atuais compartilhados continuam localizáveis. (`app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:291`; `app/chamados/templatetags/chamados_tags.py:166`) |
| brief:111 — BUG-PAR ainda futura e acrescenta apenas filtro vizinho | **CAIU.** Registro declara pouso c4+c4c; há também adaptador de aplicação e fallback. (`app/docs/BACKLOG.md:2084`; `app/chamados/templatetags/chamados_tags.py:132`; `app/chamados/templatetags/chamados_tags.py:139`) |
| brief:116–118 — rotas F6 passam pela UI | **CONFERE no Caddyfile.** O matcher core não contém essas rotas; o fallback aponta para UI. Recarga efetiva das cascas: **[nao verificado]**. (`Caddyfile:8`; `Caddyfile:9`; `Caddyfile:10`) |
| brief:121–124 — validação sem efeito conserva `aguarda_admin` | **CONFERE a regra D05**, inclusive quando T atribui a ação ao sistema. (`app/docs/BACKLOG.md:2091`; `app/chamados/services/tarefa_do_dia.py:172`; `app/chamados/services/tarefa_do_dia.py:174`) |
| brief:129–130 — todas as antigas respostas vazias ficam invariavelmente fora do app | **CAIU como regra universal atual.** L-130 distingue cura do estoque aberto e estoque entregue; os números atuais são **[nao verificado]**. (`app/docs/LEIS.md:151`) |
| brief:143–144 — aguardar F1 antiga e rebase futuro sobre BUG-PAR | **CAIU nessa forma.** F1 B1 e BUG-PAR têm registros de pouso; B2 continua sendo a reconferência necessária dos leitores. (`app/docs/CORTES.md:488`; `app/docs/BACKLOG.md:2084`; `app/chamados/juizes.py:1551`) |
| matriz:107 — ainda pedir conferência do APK ao Fernando | **CAIU.** O complemento registra que o campo é opcional, que `dias_esperando` não é lido e manda baixar a pendência. Fonte Android independente: **[nao verificado]**. (`app/docs/CORTES.md:491`) |

**Contagem das marcas:** 11 no brief e 29 na matriz; duas dependências da interface F2 permanecem. As demais 38 caíram na forma pressuposta pelos anexos. As ocorrências e seus fundamentos estão individualizados acima. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`; `app/chamados/models.py:3151`; `app/chamados/models.py:3688`)

DEPENDE CAIU 38 de 40

## 2 Censo escritores e leitores

Convenção: caminhos do inventário abaixo relativos a `app/`; **R**=`respondida_em`, **T**=`resposta_colab`, **V/VP**=`validada_em/validada_por`, **M/MP**=`materializada_em/materializada_por`. **NOVO** significa ausente da enumeração do brief, sem afirmar nascimento recente. Acessos inteiramente construídos dinamicamente: **[nao verificado]**.

**Escritores físicos e portas:**

| Campos | Escritor/persistência na BASE |
|---|---|
| R/T e campos iniciais — **NOVO** | Nascimento por `**campos`, `save` e conservação da projeção anterior. (`chamados/models.py:3124`; `chamados/models.py:3127`; `chamados/models.py:3129`) |
| R/T | Resposta humana: atribui, salva e registra RESPOSTA. (`chamados/services/disputa_emissao.py:1481`; `:1482`; `:1522`; `:1525`) |
| R/T — **NOVO** | Automática: atribui, salva e registra RESPOSTA. (`chamados/services/disputa_emissao.py:1674`; `:1675`; `:1677`; `:1678`) |
| R — **NOVO** | Reparo pela trilha: instante testemunhado e RESPOSTA. (`chamados/services/disputa_emissao.py:1696`; `:1698`; `:1699`) |
| V/VP | Nova resposta limpa validação anterior. (`chamados/services/disputa_emissao.py:1494`; `:1495`; `:1496`) |
| V/VP | `_carimbar_validacao`: escreve e registra VALIDACAO. (`chamados/services/validacao.py:168`; `:169`; `:171`; `:173`) |
| V/VP | Devolução recusada: limpa e registra DEVOLUCAO. (`chamados/services/validacao.py:273`; `:274`; `:276`; `:279`) |
| V/VP — **NOVO BUG-PAR** | Devolução de paridade: limpa e registra DEVOLUCAO. (`chamados/services/validacao.py:349`; `:350`; `:351`; `:354`) |
| R/V/VP — **NOVO** | `carimbar(resposta=)` pode preencher e salvar sem registrar RESPOSTA/VALIDACAO nesse corpo. (`chamados/models.py:3242`; `:3245`; `:3246`; `:3262`) |
| R/T/V/VP | Reabertura prepara zeragem e usa porta de lote. (`chamados/services/disputa_emissao.py:1609`; `:1648`) |
| R/T/V/VP — **NOVO** | Ressuscitação conserva linhas e zera o par inteiro pela porta de lote. (`chamados/models.py:1613`; `:1629`; `:1630`) |
| M/MP | `_escrever_carimbo` atribui; `carimbar`, identidade, descarimbo e lote persistem. (`chamados/models.py:3209`; `:3210`; `:3262`; `:3300`; `:3322`; `:3393`) |
| Lote — **NOVO** | `descarimbar_em_lote` também persiste campos recebidos em `outros`. (`chamados/models.py:3391`; `:3393`) |
| Só memória — **NOVO** | Simulação da ressuscitação e restauração após falha. (`chamados/models.py:1554`; `:1555`; `:1556`; `:3273`; `:3274`; `:3275`) |
| Sequência — **NOVO no censo F6** | Porta persistente `DecisaoPergunta.registrar`, com `bulk_create` interno. (`chamados/models.py:4110`; `:4120`) |

O homônimo `RegularizacaoExterna` não pertence à pergunta. (`chamados/services/regularizacao_ext.py:199`; `chamados/models.py:4152`)

**Leitores, seletores, fotos e transportes dos campos.** Os escritores acima também aparecem neste inventário. Fora dos sete módulos nomeados pelo brief, os sítios são **NOVO no censo**.

```text
api/views.py T:1163,2561; R:1165,2563,2577,2620,2637
api/views_mensageria.py T:567; R:569,570
chamados/canal.py T:65
chamados/templatetags/chamados_tags.py T:139,166

NOVO chamados/juizes.py
 M:292,683,711,858,1390,1502
 R:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753
 T:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052
 V:856,1389

NOVO chamados/models.py
 M:1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485
 MP:3210,3236,3238,3293,3296,3320,3365,3393
 R:1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100
 T:1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105
 V:310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104
 VP:1614,1629,3237,3246,3247,3275,4105

NOVO chamados/reconciliador.py
 M:367,630,1050,1145,1155,1505; MP:1146,1156
 T:365,1083; V:631,1050

NOVO chamados/services/acoes_disputa.py R:97,132; T:355; V:347
NOVO chamados/services/becos.py R:30
NOVO chamados/services/declaracao_texto.py R:417,419,687; T:417; V:227

NOVO chamados/services/disputa_emissao.py
 M:228,776,1084,1104,1340,1637,1930,2062,2073,2090
 R:1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091
 T:1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079
 V:1484,1485,1494,1496,1537,1538,1609,1636,2062
 VP:1484,1495,1496,1537,1538,1609,1636

NOVO chamados/services/explicador.py T:55,102,103
NOVO chamados/services/fabrica_da_celula.py M:171
NOVO chamados/services/fio.py R:187,706,1613; T:377,1279,1291,1613
NOVO chamados/services/foto_e4b.py M/R/V:180
NOVO chamados/services/historico.py R:99; V:101; M:103; T:109

NOVO chamados/services/materializacao.py
 M:148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127
 MP:1154; R:1151,2064
 T:546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619
 V:199,201,1152,2065,2082; VP:1152

NOVO chamados/services/painel.py M/R/V:67; VP:262; V:270,271,272,273
NOVO chamados/services/passivo_validado.py M:64,534,535,536; MP:64,535; R/V/VP:66
NOVO chamados/services/pergunta_no_app.py R:81
NOVO chamados/services/raiox.py M:135,182; R/T:138,147
NOVO chamados/services/relavra_via.py M:181,244,252

NOVO chamados/services/validacao.py
 M:262,264,341,363,556,681,701,717; MP:342,363
 R:594,813,824,848,1167
 T:175,263,281,356,380,823,843,848,1094,1096,1098,1

[truncado em 20000 de 59525 caracteres]

## 17/11 01:21 · sessao 725e070a4ac0 · - · - · origem=Ronald (tmux)

- **Premissas confirmadas:** `trocar_plantao` abre `atomic` próprio, ordena cartas por `pk` e apenas as duas células da chamada; captura `_TrocaRecusada` e devolve `'recusada'`. Portanto, ordenar as quatro antes das chamadas e provocar rollback externo são exigências coerentes, ainda por implementar (`app/ponto/portas/celula.py:690`, `app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:711`, `app/ponto/portas/celula.py:732`). A lei respondida, d1=d2 e o contrato constam no RELATO (`app/docs/RELATO.md:1811`).
- **QUEM:** opus construtor resolve a vedação ao Codex sobre `celula.py` e item que pede `!`; a regra também exige separação entre construção e certificação (`app/docs/LEIS.md:147`).
- **Referência inexata:** a obra está em `BACKLOG.md:2198`; `:2199` é RETRATO-POR-EVENTO (`app/docs/BACKLOG.md:2198`, `app/docs/BACKLOG.md:2199`).
- **d1=d2 exige implementação efetiva:** duas chamadas atuais sobre o mesmo dia repetem as duas coordenadas, escrevendo os papéis False/True e depois True/False; o DNA vem do vínculo próprio e de seus marcos. Isso não demonstra uma permuta de turnos no mesmo dia. É lacuna da implementação atual, não contradição nova do aval que permite mudar a porta (`app/ponto/portas/celula.py:711`, `app/ponto/portas/celula.py:622`, `app/ponto/portas/celula.py:629`, `app/ponto/management/commands/gerar_celulas.py:85`, `app/ponto/management/commands/gerar_celulas.py:90`).
- **Dinheiro é alcançado:** a edição chama o rabo comum, que agenda recálculo do fechamento no commit (`app/ponto/portas/celula.py:665`, `app/ponto/portas/celula.py:483`, `app/ponto/portas/celula.py:496`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1824`). Valores ou impacto real: **[nao verificado]**.
- **Exportada:** a porta barra a competência lavrada, salvo `apesar_da_lavra`; a enumeração das exportações não usa trava e o TOCTOU já está registrado na fila. O novo DIFF precisa demonstrar o alcance, e não pode presumir que a guarda tornou a composição concorrente segura (`app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:621`, `app/ponto/management/commands/gerar_celulas.py:170`, `app/docs/BACKLOG.md:2197`). Competência exportada efetivamente atingida: **[nao verificado]**.
- **Limite material do rollback:** a edição rejulga sincronamente; o cartório pode abrir chamado e o emissor envia push imediatamente. Por dedução estática desse caminho, rollback SQL não recolhe uma notificação já enviada. É condição que o construtor precisa enfrentar para prometer rollback integral; envio real nesse cenário: **[nao verificado]** (`app/ponto/portas/celula.py:472`, `app/ponto/services/cartorio.py:1791`, `app/ponto/management/commands/detectar_ausencias.py:779`, `app/chamados/push_utils.py:40`).
- **Não interpretar como autorização para reduzir o contrato:** a obra existente exige quatro validações antes da primeira escrita, rollback integral, recusa falante, ID próprio ligando células/pedido e escala-base intocada; o item 2 do novo aval lista só três obrigações do PREPARA, mas a FONTE mantém o contrato maior (`app/docs/BACKLOG.md:2198`). Ausência dessas obrigações na construção futura: **[nao verificado]**.

## 17/11 01:21 · sessao 26402f43f3aa · REVISAO-AVAL-C3PREP · verificador · origem=Code

Referências aos anexos contam as linhas a partir de “aval Ronald”.

1. **Premissas:** a cópia registra PREPARA F2/F4/F7, entregas e sondas da volta 2 (`app/docs/RELATO.md:16–23`), além do PID 2218451 (`app/docs/RELATO.md:3`). O teto do papel sonda é 1179 s (`bin/codex_pergunta.sh:95`). Resultado independente de `revisao_c2prep`, HEAD atual do main, quantidade de tentativas e rc de cada uma: **[nao verificado]**.

2. **Colisão com o vigente:** não identificada no escopo de leitura declarado (`complemento3_prepara_1711.txt:1–6`). A precedência antiga da CLASSE-D (`app/docs/BACKLOG.md:1990`; `app/docs/RELATO.md:421`) foi expressamente dispensada para abrir F2 pelo COMPLEMENTO-1 (`app/docs/RELATO.md:4`). LU2-07 e a parte aberta da LU2-08 continuam pendentes; o PREPARA não as resolve (`app/docs/RELATO.md:352–354`, `:422`). Os cortes das fatias permanecem aplicáveis (`app/docs/CORTES.md:488`). Brief integral e cruzamentos atuais das oito remedidas: **[nao verificado]**.

3. **Literal:** o C3 substitui expressamente a espera da volta 2 e preserva o restante do C2; não há contradição nessa antecipação (`complemento3_prepara_1711.txt:2`; `complemento2_prepara_f3_f5_f6_f8_f9_1711.txt:4`). O ramo “duas vezes ou mais” depende da medição solicitada: não pode ser tratado como ocorrido nem como ausente **[nao verificado]**. Na interpretação dos rc, 124 é timeout do Codex; expiração da trava da resposta retorna 3 (`bin/codex_pergunta.sh:392`, `:425–428`, `:447`). O envelope SQL roda antes desse timeout, portanto esse rc não prova que o SQL excedeu 1179 s (`bin/codex_pergunta.sh:329`, `:425`).

4. **Exportada, dinheiro ou `!`:** nenhuma escrita nesses domínios é autorizada pelos anexos; o escopo permanece registro e leitura, sem construtor, suite, trava ou mudança do push (`complemento3_prepara_1711.txt:1`, `:6`; `complemento2_prepara_f3_f5_f6_f8_f9_1711.txt:3`, `:6`). A guarda de exportada continua expressa no corte da F2 (`app/docs/CORTES.md:488`). Execução efetiva das sondas e seus efeitos: **[nao verificado]**.

VEREDITO: CONFERE (`complemento3_prepara_1711.txt:2–6`; `app/docs/RELATO.md:4`; `bin/codex_pergunta.sh:95`).

## 17/11 01:21 · sessao a191aa5b5315 · - · - · origem=Ronald (tmux)

- **Premissas documentadas:** O10-D exige quatro pré-validações antes da primeira escrita, rollback integral, recusa falante, ID próprio e escala-base intocada; construtor opus e pouso no `!` de escala já constam em `app/docs/BACKLOG.md:2198`. PREPARA fechada, lei zero e cruzamento dependente da lista do construtor constam em `app/docs/RELATO.md:147`; LOTE7 registrado como pousado em `app/docs/RELATO.md:164`. A referência do aval a `BACKLOG:2199` erra uma linha: ali está RETRATO-POR-EVENTO (`app/docs/BACKLOG.md:2199`).

- **Pista/trava, item 7:** o corte vigente manda suite inteira/pre-opus lateral à PISTA; pista ocupada significa esperar, e VM é fallback somente com pista indisponível e causa registrada (`app/docs/CORTES.md:490`). O complemento distingue suite de não-pouso lateral na PISTA e VM para espinha/pouso/push (`app/docs/RELATO.md:102`). Portanto “pedir a trava na brecha” não autoriza escolher VM enquanto a PISTA funciona; essa leitura colidiria com esses cortes (`app/docs/CORTES.md:490`; `app/docs/RELATO.md:102`). O item pode coexistir se conservar esse roteamento; intenção literal de substituir o roteamento: [nao verificado].

- **Fila não é garantia mecânica:** `--quem` só informa titular; aquisição usa `flock`, sem interface de fila ou prioridade (`bin/trava_teste.sh:28`; `bin/trava_teste.sh:35`). O RELATO já registra que prioridade depende do despacho (`app/docs/RELATO.md:6`). Estado atual “espinha sem suite na fila” e cessão efetiva: [nao verificado].

- **CLASSE-D/LU2:** CLASSE-D R1 trata C5 por dia/dono, muda tela/PDF e admite pouso separado sem aguardar LU2-07/08 (`app/docs/RELATO.md:421`). LU2-08 conserva corte pendente para FATO_EM_AUSENCIA e migração do insumo em `InsumosDoDono.dono()` (`app/docs/RELATO.md:352`; `app/docs/RELATO.md:422`). LU2-07 conserva questão emp1 e preservação L-124 da competência 10 paga (`app/docs/RELATO.md:354`). O aval não deve ser interpretado como autorização para decidir esses pontos; cruzamento efetivo com futura lista de O10-D: [nao verificado]. Os briefs físicos referidos em `logs/` não estão disponíveis nesta cópia: [nao verificado].

- **Ordem atualizada:** não bloquear F2 simplesmente pela ordem antiga CLASSE-D → F2 de `app/docs/BACKLOG.md:1990`: o complemento já permite F2 após B2 sem aguardar CLASSE-D (`app/docs/RELATO.md:4`).

- **Verificação obrigatória permanece:** todo construtor deve passar pelo pre-verificador, triagem com descartes anexos ao opus, suite inteira verde e depois opus final (`CLAUDE.md:372`; `CLAUDE.md:376`; `CLAUDE.md:378`). O aval cita os papéis, mas execução desses portões ainda é [nao verificado]. [nao verificado]

## 17/11 01:20 · sessao dc4aa7b78cbb · - · - · origem=Ronald (tmux)

- **Teto/condição literal:** “teto do papel sonda” corresponde ao teto de **1179 s**, eventualmente rebaixado por `CODEX_TIMEOUT` (`bin/codex_pergunta.sh:95-98`). O timeout envolve o Codex desse papel (`bin/codex_pergunta.sh:425-428`); `124` é tratado como timeout (`bin/codex_pergunta.sh:447-451`). Portanto, **não há colisão literal** no item 3. Quantidade de tentativas e respectivos rc: **[nao verificado]**.
- **Precisão operacional:** a execução do envelope antecede o timeout do Codex (`bin/codex_pergunta.sh:323-329`). Assim, timeout do papel não prova demora do SQL; o aval fala do papel, portanto isso não fundamenta PROCEDE (`bin/codex_pergunta.sh:95`; `bin/codex_pergunta.sh:425`).
- **Paralelismo:** leituras independentes em paralelo estão previstas até o teto medido (`CLAUDE.md:357-365`), e o wrapper controla slots (`bin/codex_pergunta.sh:364-381`). Soltar as cinco leituras não exige aumentar esse teto.
- **Abertura F2/CLASSE-D:** o registro vigente já responde que F2 abre pelas quatro condições contra HEAD com B2 pousada, **sem esperar CLASSE-D** (`app/docs/RELATO.md:4`). A frota aparece como medida posterior de volta 2, com lei zero já registrada (`app/docs/RELATO.md:16-19`).
- **B2/suite/trava/push:** o PREPARA registrado já é sem cópia, suite ou trava (`app/docs/RELATO.md:23`). B2 consta aberta separadamente (`app/docs/RELATO.md:30`); regras próprias da VM/pista seguem em `app/docs/CORTES.md:490`. Não encontrei colisão nesses pontos; estado real dos processos/logs: **[nao verificado]**.

## 17/11 01:20 · sessao 281d67d961ba · - · - · origem=Ronald (tmux)

- **Sem colisão com CLASSE-D na abertura da F2:** a ordem antiga exige CLASSE-D antes da F2 (`app/docs/BACKLOG.md:1990`; `app/docs/RELATO.md:421`), mas o registro posterior responde expressamente: F2 abre pelas quatro condições contra HEAD com B2 pousada, **sem esperar CLASSE-D** (`app/docs/RELATO.md:4`).
- **CLASSE-D continua distinta:** R1 propõe C5 por dia e dono por `InsumosDoDono`, sem esperar código LU2-07/08 (`app/docs/RELATO.md:421`). A cópia ainda publica C5 por colaborador, sem data (`app/escala/services/cadastro_realidade.py:176`), e exclui C5 de `DONO_PELO_JUIZ` (`app/escala/services/cadastro_realidade.py:968`).
- **LU2-07/08 não foram resolvidas por esse PREPARA:** LU2-07 mantém pergunta sobre operação emp1 e retenção por assinatura datada (`app/docs/RELATO.md:354`); cron limita empresas a 2/3/4 (`app/config/crons.py:17`). LU2-08 distingue parcial sem período decidido e fato em ausência aberto (`app/docs/RELATO.md:352`); brief mantém insumo em `InsumosDoDono.dono()` e espera corte para fatos em ausência (`app/docs/RELATO.md:422`).
- **Cuidado com referência histórica:** o corte COBERTURA-PARCIAL-PERIODO citado como `CORTES.md:370` no RELATO está nesta cópia em `app/docs/CORTES.md:386`: parcial sem período vai a CADASTRO e não a FATO_EM_AUSENCIA.
- **Cruzamentos atuais das oito remedidas e brief integral CLASSE-D:** [nao verificado]. Os arquivos `logs/...` citados não estão na cópia; os registros disponíveis são os resumos acima (`app/docs/RELATO.md:421`; `app/docs/RELATO.md:422`).
- **PREPARA permanece leitura sem construção**, compatível com despacho anterior, sem cópia/suíte/trava (`app/docs/RELATO.md:23`); as matrizes já eram preparação separada da construção (`app/docs/BACKLOG.md:2063`).

## 17/11 01:19 · sessao 349b1eb9edec · PREPARA-F3-F5-F6-F8-F9 · leitura · origem=Code

## 1 DEPENDE e premissas

**A premissa central da F8 caiu:** a F1 atual oferece `pergunta_respondida(p)`, que exige instante **e** texto após trim; o próprio contrato deixa os leitores B/C/D para a B2. A antiga interface de leitura dos atos não pode orientar esta preparação. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1550`; `app/chamados/juizes.py:1552`)

**A L1 antiga já foi decidida:** V legada permanece válida diante de R sintética posterior. O corte exige medir perguntas e batidas contra a árvore pousada; os números atuais continuam **[nao verificado]**. (`app/docs/CORTES.md:488`, item 8)

BASE identificada pelo pedido: `fbc5c76ced33bf25943f3a5bc78ebff257d68c56`. Confirmação independente do SHA/HEAD, ancestralidade e diferenças entre os commits históricos: **[nao verificado]**. Resultados de testes e sondas em execução: **[nao verificado]**.

`B` e `M` abaixo significam os anexos `brief.md` e `matriz.md`, numerados desde o título, incluindo linhas vazias. Correspondência dessa numeração com arquivos externos originais: **[nao verificado]**. As referências `app/...` são da BASE.

**Cada marca literal dos anexos:**

| ID | Origem | Premissa | Julgamento na BASE |
|---|---|---|---|
| B1 | B:18 | Contrato F1 da coluna U1–U9 fornece leitura de R/V por atos | **CAIU.** O contrato atual retorna booleano sobre a projeção. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1552`) |
| B2 | B:31 | As oito trocas e as exceções dependem daquela interface | **CAIU como contrato de execução.** Os leitores continuam crus; sua migração B/C/D é atribuída à B2. (`app/chamados/juizes.py:1551`; `app/ponto/services/cartorio.py:1641`; `app/colaboradores/services/calendario.py:504`) |
| B3 | B:56 | O contrato não fornece a recusa técnica de U2 | **CONFERE o limite.** O juiz atual não retorna motivos; U2 lê o motivo técnico projetado. (`app/chamados/juizes.py:1552`; `app/ponto/services/flip_auto.py:132`; `app/chamados/services/materializacao.py:1665`) |
| B4 | B:61 | Estão disponíveis as faces privada, conservada e SQL descritas em B:62–64 | **CAIU.** As faces concretas atuais são `pergunta_respondida` e seu `Q` projetivo. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1575`) |
| B5 | B:68 | A comparação de frota pode usar a antiga leitura conservada como juiz atual | **CAIU.** A projeção temporal e o juiz atual leem campos; a comparação histórica não mede esta BASE. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1552`) |
| M1 | M:8 | F1 vigente é a interface de `be2b8d733` | **CAIU.** O contrato declara F1 NOVA B1/L-130. (`app/chamados/juizes.py:1545`; `app/chamados/juizes.py:1550`) |
| M2 | M:24, primeira marca | U1 lê `autor_validacao` da leitura F1 | **CAIU.** A função atual não fornece autor. U1 continua lendo V/C diretamente. (`app/chamados/juizes.py:1552`; `app/ponto/services/procedencia_batida.py:72`) |
| M3 | M:24, segunda marca | A fixture V sem DP só passa por conservação do fold | **CAIU a exclusividade.** V projetada é lida diretamente, inclusive pelo C6. (`app/ponto/services/procedencia_batida.py:74`; `app/chamados/juizes.py:1405`) |
| M4 | M:25 | Não há autor C fornecido pelo contrato F1 | **CONFERE o limite.** O contrato atual é booleano; o fallback de autor continua projetivo. (`app/chamados/juizes.py:1552`; `app/ponto/services/procedencia_batida.py:72`) |
| M5 | M:26 | U1 recebe `instante_validacao` da leitura F1 | **CAIU.** U1 lê o campo atual; o juiz L-130 não devolve instante. (`app/ponto/services/procedencia_batida.py:74`; `app/chamados/juizes.py:1552`) |
| M6 | M:27 | U1 recebe `instante_materializacao` daquela leitura | **CAIU.** O fallback atual é o campo C. (`app/ponto/services/procedencia_batida.py:74`) |
| M7 | M:34 | U2 recebe `instante_validacao` do contrato antigo | **CAIU.** A seleção atual é SQL sobre V projetada. (`app/ponto/services/flip_auto.py:130`; `app/chamados/juizes.py:1552`) |
| M8 | M:35 | Existe `leitura.motivo_recusa` como alternativa executável atual | **CAIU a interface pressuposta.** Permanece válida a distinção entre recusa técnica e RECUSA administrativa. (`app/chamados/juizes.py:1552`; `app/chamados/services/materializacao.py:1665`; `app/chamados/services/validacao.py:836`) |
| M9 | M:42 | U3y sob F1 só preserva R pela face conservada antiga | **CAIU a explicação.** O teste usa instante e texto; o leitor atual continua projetivo. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:351`; `app/ponto/services/cartorio.py:1641`) |
| M10 | M:50 | O comportamento de SILENCIO pode ser explicado pelos redutores antigos citados | **CAIU o suporte dessa interface.** A tarefa atual lê SILENCIO; sua fotografia continua projetiva. (`app/chamados/services/tarefa_do_dia.py:176`; `app/ponto/services/veredito_celula.py:197`) |
| M11 | M:56 | A face SQL pública exclui texto só-branco | **CONFERE**, agora exigindo também instante. (`app/chamados/juizes.py:1575`; `app/chamados/juizes.py:1580`) |
| M12 | M:75 | A seção seguinte descreve a F1 disponível | **CAIU.** `carga_pergunta_viva` oferece `carga_da_vida`; o juiz de resposta é o booleano L-130. (`app/chamados/services/carga_pergunta_viva.py:69`; `app/chamados/juizes.py:1544`) |
| M13 | M:89 | P1 pode ser cumprido por `LeiturasDaVida`/face conservada antiga | **CAIU a API.** O juiz único e sua face SQL atuais estão nomeados. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1575`) |
| M14 | M:106 | L1 continua herdada e sem decisão | **CAIU.** O item 8 posterior responde SIM. (`app/docs/CORTES.md:488`) |
| R1 | M:10 | Calendário tem a forma funcional R3 | **CONFERE a forma**, sem certificar ancestralidade dos hashes. (`app/colaboradores/services/calendario.py:188`; `app/colaboradores/services/calendario.py:506`) |
| R2 | M:56 | Filtro R está no calendário na forma R3 | **CONFERE.** (`app/colaboradores/services/calendario.py:504`) |
| R3 | M:57, primeira marca | C6 recebe `agora`; falha da oferta fica desconhecida | **CONFERE.** (`app/colaboradores/services/calendario.py:506`; `app/colaboradores/services/calendario.py:509`) |
| R4 | M:57, segunda marca | A tarefa carrega a oferta plural do calendário | **CONFERE**, com chamada atual deslocada. (`app/chamados/services/tarefa_do_dia.py:607`; `app/chamados/services/tarefa_do_dia.py:613`) |
| R5 | M:57, terceira marca | Pin do calendário é `[273]` | **CONFERE.** (`app/core/tests/test_medida_turno_do_par.py:275`) |
| R6 | M:103 | F8 deve considerar a forma R3 do calendário | **CONFERE a dependência funcional.** A ordem histórica entre commits permanece **[nao verificado]**. (`app/colaboradores/services/calendario.py:188`; `app/colaboradores/services/calendario.py:504`) |

**Demais premissas sobre F1/BUG-PAR/outras fatias, contadas separadamente das marcas:**

| ID | Origem | Premissa | Julgamento na BASE |
|---|---|---|---|
| A1 | B:3; M:6–10 | As bases antigas e a F1 citada descrevem o contrato de partida atual | **CAIU como base operativa.** O código declara F1 NOVA B1. Identidade histórica dos blobs: **[nao verificado]**. (`app/chamados/juizes.py:1550`) |
| A2 | B:9–13,30; M:56–57 | Calendário possui `agora`, `_oferta_conhecida` e C6 temporal | **CONFERE.** (`app/colaboradores/services/calendario.py:188`; `app/colaboradores/services/calendario.py:490`; `app/colaboradores/services/calendario.py:506`; `app/colaboradores/services/calendario.py:624`) |
| A3 | B:33; M:57 | O chamador plural passou do vigia para a tarefa | **CONFERE o chamador atual.** (`app/chamados/services/tarefa_do_dia.py:613`) |
| A4 | B:20–28,39; M:22–73 | Os oito arquivos ainda contêm os leitores projetivos apontados | **CONFERE.** (`app/ponto/services/procedencia_batida.py:72`; `app/ponto/services/flip_auto.py:130`; `app/ponto/services/cartorio.py:1641`; `app/ponto/services/veredito_celula.py:197`; `app/colaboradores/services/calendario.py:504`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/ponto/services/dia_do_colab.py:70`) |
| A5 | B:55–56; M:35 | `recusa_motivo` possui somente os dois escritores enumerados | **CAIU.** Há escape do colaborador, pendência técnica e porta genérica, além dos dois sentidos descritos. (`app/chamados/services/disputa_emissao.py:1511`; `app/chamados/services/materializacao.py:1332`; `app/chamados/models.py:3298`) |
| A6 | B:24; M:49 | C de U5 é carimbo do mesmo ato de fechamento | **CONFERE.** (`app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:196`) |
| A7 | B:59; M:63 | A troca isolada de U7a basta para unificar a fonte da ficha | **CAIU.** A ficha também delega à retenção, ainda I OU T. (`app/colaboradores/services/ficha.py:169`; `app/chamados/models.py:1924`; `app/chamados/models.py:1925`) |
| A8 | M:76–84 | `LeituraDosAtos`, `_leitura_conservada` e `LeiturasDaVida` são o contrato disponível | **CAIU.** O contrato concreto é juiz booleano, projeção temporal e carga de vitalidade. (`app/chamados/juizes.py:1382`; `app/chamados/juizes.py:1544`; `app/chamados/services/carga_pergunta_viva.py:69`) |
| A9 | B:63,123; M:82–84 | A face SQL atual materializa frota em Python ou usa a Subquery conservada histórica | **CAIU.** A face atual é um `Q` sobre instante e regex. (`app/chamados/juizes.py:1580`) |
| A10 | B:64,92; M:79–81 | A F8 atual necessariamente precisa de `decisoes=`/prefetch para chamar o juiz | **CAIU.** O juiz lê apenas dois campos do objeto. (`app/chamados/juizes.py:1552`) |
| A11 | B:92 | Não existe selo de queries do calendário | **CAIU como ausência geral.** Existe limite do perfil fixo; constante em N carimbadas continua **[nao verificado]**. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:230`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:237`) |
| A12 | B:49 | `classificar_pares` chama a função `fila_flip` | **CAIU.** É `append` numa lista local; a função `fila_flip` é quem chama `classificar_pares`. (`app/ponto/services/flip_auto.py:112`; `app/ponto/services/flip_auto.py:146`) |
| A13 | B:101,104; M:36 | A BUG-PAR antiga, ainda externa, basta para descrever a dependência atual | **CAIU.** A porta vigente já contém K6 antes da devolução. Comparação com o hash antigo: **[nao verificado]**. (`app/chamados/services/validacao.py:338`; `app/chamados/services/validacao.py:339`) |
| A14 | B:104; M:36 | BUG-PAR pode retirar perguntas do universo de U2 | **CONFERE**, quando passa pelo recorte atual: descarimba, limpa V e registra DEVOLUCAO; sem dia/entregue não devolve. (`app/chamados/services/validacao.py:339`; `app/chamados/services/validacao.py:346`; `app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:354`) |
| A15 | B:49,108; M:36 | A cadeia relevante de flip se esgota no cron | **CAIU como censo completo.** Também existe consumidor humano de `avaliar_fila`. (`app/ponto/views.py:3457`; `app/ponto/views.py:3459`) |
| A16 | B:21,23,31,115; M:92–95 | Via, motivo técnico e fallback C permanecem projetivos | **CONFERE nos corpos e nas exceções descritas.** (`app/ponto/services/flip_auto.py:129`; `app/ponto/services/flip_auto.py:132`; `app/ponto/services/cartorio.py:1650`; `app/ponto/services/procedencia_batida.py:72`; `app/ponto/services/procedencia_batida.py:74`) |
| A17 | B:48; M:23,96–97 | O teste de U1 mede uma query, mas sua fixture não carimba R/V | **CONFERE.** (`app/ponto/tests/test_procedencia_batida.py:123`; `app/ponto/tests/test_procedencia_batida.py:126`) |
| A18 | M:99 | U2 já satisfaz relógio na borda da rodada | **CAIU como comportamento presente.** A rodada lê um instante; U2 lê outro. (`app/ponto/services/flip_auto.py:126`; `app/ponto/services/flip_auto.py:263`; `app/ponto/services/flip_auto.py:268`) |
| A19 | B:102; M:102 | Os três pins identificados permanecem relevantes | **CONFERE.** (`app/core/tests/test_medida_turno_do_par.py:275`; `app/core/tests/test_medida_turno_do_par.py:319`; `app/core/tests/test_medida_turno_do_par.py:330`) |
| A20 | B:110,119,121; M:106–109,118–119 | Ainda falta responder L1 e escolher entre fold puro/conservado/backfill para liberar F8 | **CAIU.** L1 foi respondida; o juiz atual não exige linhas DP para reconhecer o par projetado. (`app/docs/CORTES.md:488`; `app/chamados/juizes.py:1552`) |

As afirmações históricas de igualdade de blobs, neutralidade de frota, quantidade de mudanças de procedência e fila de produção não são medidas desta BASE: **[nao verificado]**. A resposta sintética não revogar V é regra escrita; sua quantidade é objeto da sonda. (`app/docs/CORTES.md:488`)

Contagem: 25 marcas literais mais 20 premissas adicionais; cada linha acima conta uma vez. Os contadores textuais de M:116 não são novas dependências. Correspondência desses contadores com os arquivos externos: **[nao verificado]**.

DEPENDE CAIU 28 de 45

## 2 Censo escritores e leitores

Convenções: caminhos relativos à raiz da BASE; `T=resposta_colab`, `I=respondida_em`, `V=validada_em`, `VP=validada_por/_id`, `C=materializada_em`, `CP=materializada_por/_id`, `X=via_resolucao`, `M=recusa_motivo`. **NOVO** significa não enumerado como esse sítio no brief F8. Cobertura de nomes inteiramente construídos em execução: **[nao verificado]**.

**Escritores físicos, portas e simulações:**

| Campos | Escritores |
|---|---|
| T/I | **NOVO:** resposta humana em `app/chamados/services/disputa_emissao.py:1481,1482,1522,1525`; automática em `:1674,1675,1677,1678`; trilha preenche I em `:1696,1698,1699`. |
| T/I/V/VP | **NOVO:** ressuscitação em `app/chamados/models.py:1629,1630`; reabertura em `app/chamados/services/disputa_emissao.py:1609,1648`; nascimento genérico em `app/chamados/models.py:3124,3127,3129`. |
| I/V/VP | **NOVO:** `carimbar(resposta=)` em `app/chamados/models.py:3242,3245,3246,3262`; esse corpo não registra RESPOSTA/VALIDACAO. |
| V/VP | `app/chamados/services/validacao.py:168,169,171,173`; devolução em `:273,274,276,279`; **NOVO BUG-PAR** em `:349,350,351,354`; nova resposta limpa em `app/chamados/services/disputa_emissao.py:1494,1495`. |
| C/CP/X | `_escrever_carimbo` em `app/chamados/models.py:3209,3210,3211`; persistidores em `:3262,3300,3322`; **NOVO** lote em `:3391,3393`. |
| Somente X | **NOVO:** encerramento específico em `app/chamados/models.py:3402,3403`; relavra em `app/chamados/services/relavra_via.py:125,127`. |
| M | Escape **NOVO** em `app/chamados/services/disputa_emissao.py:1511,1522`; pendência técnica **NOVA** em `app/chamados/services/materializacao.py:1332,1333`; `_carimbar_recusa` em `:1665,1666`; colisão delega em `:1662,1663` a `app/chamados/models.py:3298,3300`; administrativa em `app/chamados/services/validacao.py:827,833,836`; nascimento por `**campos` em `app/chamados/models.py:3124`. |
| Apenas memória | Simulação em `app/chamados/models.py:1554,1555,1556`; rollback em `:3273,3274,3275`. |
| DP | Escritor físico `DecisaoPergunta.registrar`, com lote interno. (`app/chamados/models.py:4110`; `app/chamados/models.py:4120`) |

`regularizacao_ext.py` escreve outro modelo; não pertence aos escritores da pergunta. (`app/chamados/services/regularizacao_ext.py:199`; `app/chamados/models.py:4152`)

**Leitores, filtros e transportes globais.** Os escritores acima também leem campos. Cada grupo `:n,n` abaixo indica linhas do arquivo nomeado; strings de campos/cargas são inventariadas, sem equipará-las automaticamente a julgamento.

| Arquivo | Campos e linhas |
|---|---|
| **NOVO** `app/api/views.py` | T `:1163,2561`; I `:1165,2563,2577,2620,2637`. |
| **NOVO** `app/api/views_mensageria.py` | T `:567`; I `:569,570`. |
| **NOVO** `app/chamados/canal.py` | T `:65`. |
| `app/chamados/juizes.py` | C `:292,683,711,858,1390,1502`; X `:683,695,857,1390`; I `:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753`; T `:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052`; V `:856,1389`. **NOVOS** os sítios não presentes na enumeração antiga das faces. |
| `app/chamados/models.py` | C `:1531,1923,2006,3153,3156,3166,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485`; CP `:3236,3238,3293,3296,3320,3365,3393`; I `:1614,1924,2949,3155,3158,3237,3241,3243,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100`; T `:1533,1614,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105`; V `:310,344,1614,1923,3151,3237,3244,3247,3331,3496,4091,4102,4103,4104`; VP `:1614,1629,3237,3247,4105`; X `:1535,2001,2010,2011,2136,2141,2829,2860,3165,3236,3238,3296,3311,3320,3331,3365,3392,3393,3400,3403,3411,3423,3440,3447,3483,3485,3652,3658`; M `:3299`. **NOVO** além dos métodos explicitamente citados pelo brief. |
| **NOVO** `app/chamados/reconciliador.py` | C `:367,630,1050,1145,1155,1505`; CP `:1146,1156`; T `:365,1083`; V `:631,1050`; X `:1144,1155`. |
| **NOVO** `app/chamados/services/acoes_disputa.py` | M `:83`; I `:97,132`; V `:347`; T `:355`. |
| **NOVO** `app/chamados/services/becos.py` | I `:30`. |
| **NOVO** `app/chamados/services/competencia_trancada.py` | X `:276,282`. |
| **NOVO** `app/chamados/services/declaracao_texto.py` | V `:227`; I `:417,419,687`; T `:417`. |
| **NOVO no inventário F8** `app/chamados/services/disputa_emissao.py` | C `:228,776,1084,1104,1340,1637,1930,2062,2073,2090`; I `:1483,1609,1635,1677,1692,1698,1703,1735,1843,1861,2074,2091`; T `:1483,1609,1635,1663,1677,1692,1700,1861,2079`; V `:1484,1485,1496,1537,1538,1609,1636,2062`; VP `:1484,1496,1537,1538,1609,1636`; X `:337,409,428,446,1637,2072,2073,2087,2090`; M `:1513`. |
| **NOVO** `app/chamados/services/explicador.py` | T `:55,102,103`; M `:113`; X `:118`. |
| **NOVO** `app/chamados/services/fabrica_da_celula.py` | X `:149`; C `:171`. |
| **NOVO** `app/chamados/services/fio.py` | I `:187,706,1613`; T `:377,1279,1291,1613`; M `:564,1407`; X `:1147,1214,1290,1397,1480,1921,1957,1979,2024`. |
| **NOVO** `app/chamados/services/foto_e4b.py` | C/I/V `:180`. |
| **NOVO** `app/chamados/services/historico.py` | I `:99`; V `:101`; C `:103`; T `:109`. |
| `app/chamados/services/materializacao.py` | C `:148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127`; CP `:1154`; I `:1151,2064`; T `:546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619`; V `:199,201,1152,2065,2082`; VP `:1152`; X `:261,292,862,868,1108,1150,1652`; M `:1333,1652,1655,1663,1666`. **NOVO** além do materializador técnico citado. |
| **NOVO** `app/chamados/services/painel.py` | C/I `:67`; V `:67,270,271,272,273`; VP `:262`. |
| **NOVO** `app/chamados/services/passivo_validado.py` | C `:64,534,535,536`; CP `:64,535`; M/I/V/VP `:66`; X `:64,140,141,142,530,535`. |
| **NOVO** `app/chamados/services/pergunta_no_app.py` | I `:81`. |
| **NOVO** `app/chamados/services/raiox.py` | C `:135,182`; I/T `:138,147`. |
| **NOVO** `app/chamados/services/relavra_via.py` | C `:181,244,252`; X `:118,121,123,127,134,136,155,181,245,252`. |
| **NOVO** `app/chamados/services/tarefa_do_dia.py` | X `:256,257`; sequência em `:421`; projeção sem linha em `:430`. |
| `app/chamados/services/validacao.py` | C `:262,264,341,363,556,681,701,717`; CP `:342,363`; I `:594,813,824,848,1167`; T `:175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109`; V `:171,173,181,262,269,276,300,302,343,351,364,489,553,597,701,810,975,996`; VP `:171,181,262,276,300,302,343,351,364,489`; X `:341,363`; M `:181,262,266,282,299,301,302,344,364,825,833,847`. **NOVO** além da recusa administrativa antiga. |
| **NOVO** `app/chamados/services/verbos.py` | I `:335,336,342,345`; V `:264,270`; VP `:264,265,266,268`. |
| **NOVO** `app/chamados/services/vigia_carimbo.py` | V `:98`. |
| **NOVO** `app/chamados/services/vigia_fio.py` | I `:636,746`; T `:749`. |
| **NOVO** `app/chamados/templatetags/chamados_tags.py` | T `:139,166`. |
| **NOVO** `app/chamados/veredito_lavrado.py` | C `:203,204`; I `:207,208`. |
| **NOVO** `app/chamados/views.py` | C `:1257`; I `:1280,1416`. |
| **NOVO** `app/chamados/views_cobrar.py` | I `:169,176`; T `:291`. |
| `app/colaboradores/services/calendario.py` | T `:504`. |
| `app/colaboradores/services/ficha.py` | T `:171`; retenção `

[truncado em 20000 de 60460 caracteres]

## 17/11 01:19 · sessao d5206513f7f1 · PREPARA-F3-F5-F6-F8-F9 · leitura · origem=Code

## 1 DEPENDE e premissas

**A premissa central CAIU:** a F1 pousada fornece `pergunta_respondida(p)`, sobre instante **e** texto da projeção. O contrato antigo `LeituraDosAtos/leitura/_leitura_conservada` não descreve essa F1. O próprio código reserva os leitores B/C/D à B2. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1550`.)

BASE identificada no pedido: `fbc5c76ced33bf25943f3a5bc78ebff257d68c56`; confirmação independente do HEAD e da ancestralidade Git: **[nao verificado]**. Execução de testes, sondas e números atuais de produção: **[nao verificado]**.

As referências `brief.md` e `matriz.md` contam as linhas do texto anexado, incluindo vazias, desde o título; correspondência com os arquivos originais: **[nao verificado]**. A contagem abaixo usa **uma premissa por linha numerada**; ocorrências repetidas das marcas estão reunidas na mesma premissa.

| Nº | Origem | Premissa julgada | Veredito na BASE |
|---|---|---|---|
| 1 | brief:76–78; matriz:14,16–18,67,107 — `[DEPENDE-F1]` estrutural | F1 oferece `LeituraDosAtos` como contrato dos leitores | **CAIU.** O contrato pousado é o juiz booleano sobre o par projetado. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1552`.) |
| 2 | matriz:19–20; brief:84–88 | O redutor F1 limpa resposta/validação conforme os atos descritos | **CAIU como interface disponível.** A projeção temporal lê os campos atuais e corta instantes futuros. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1391`; `app/chamados/juizes.py:1400`.) |
| 3 | brief:78,83; matriz:21–22,75,86 | Sem DP, F1 conserva por `_leitura_conservada`, inclusive P5 | **CAIU nesse formato.** A conservação disponível mede correspondência ato+instante; não constitui aquele redutor. (`app/chamados/models.py:4081`; `app/chamados/models.py:4094`; `app/chamados/models.py:4098`.) |
| 4 | brief:93–94; matriz:23–25 | As faces SQL R da F1 consultam atos | **CAIU.** Consultam instante/texto da projeção. (`app/chamados/juizes.py:1565`; `app/chamados/juizes.py:1572`; `app/chamados/juizes.py:1575`; `app/chamados/juizes.py:1580`.) |
| 5 | brief:20; matriz:26–28,35 — `[DEPENDE-F1]` #1 | `espera_o_texto` ficou metade projeção/metade ato após F1 | **CAIU.** Ambos os termos continuam projetivos. (`app/chamados/services/fio.py:187`; `app/chamados/juizes.py:1534`.) |
| 6 | matriz:29 — `[DEPENDE-F2]` | F2 já oferece `estado` pelo ato | **CAIU como suporte pousado.** `estado` continua lendo V/C/R. (`app/chamados/models.py:3151`; `app/chamados/models.py:3153`; `app/chamados/models.py:3155`.) |
| 7 | matriz:36 — `[DEPENDE-F1]` #2 | POST pode trocar R cru pela `leitura` herdada | **CAIU a interface pressuposta.** O degrau atual continua buscando instante nulo. (`app/chamados/services/fio.py:706`; `app/chamados/juizes.py:1544`.) |
| 8 | matriz:37 — `[DEPENDE-F1]`, `[DEPENDE-R3]` #3 | Canônica dispõe do texto do redutor; R3 antiga é a base obrigatória | **CAIU como dependência atual.** Canônica lê texto projetado; R3 c17 tem pouso registrado. (`app/chamados/services/fio.py:1279`; `app/docs/RELATO.md:235`.) |
| 9 | matriz:38 — `[DEPENDE-F1]` #4 | Mérito perdedor migra pelo contrato F1 descrito | **CAIU a interface.** Continua lendo `resposta_colab`. (`app/chamados/services/fio.py:1291`; `app/chamados/juizes.py:1544`.) |
| 10 | matriz:39 — `[DEPENDE-F1]` #5 | Lavrar sem turno recebe texto da `leitura` | **CAIU como suporte presente.** Guarda atual lê texto e C projetados. (`app/chamados/reconciliador.py:365`; `app/chamados/reconciliador.py:367`.) |
| 11 | matriz:40 — `[DEPENDE-F2]` #6 | Universo absolvível dispõe da face SQL V por ato | **CAIU como suporte pousado.** O filtro continua `validada_em__isnull=True`. (`app/chamados/reconciliador.py:631`; `app/chamados/models.py:3496`.) |
| 12 | matriz:41 — `[DEPENDE-F1]` #7 | Órfãs abertas podem consumir o redutor F1 antigo | **CAIU.** O universo atual filtra C/V projetados. A descrição “filtro R/C” dessa linha também caiu: não há R nesse filtro. (`app/chamados/reconciliador.py:1049`; `app/chamados/reconciliador.py:1050`.) |
| 13 | matriz:42 — `[DEPENDE-F1]` #8 | Veredito de órfã recebe texto do ato pela F1 | **CAIU a dependência satisfeita.** A guarda lê texto projetado. (`app/chamados/reconciliador.py:1083`.) |
| 14 | matriz:43 — `[DEPENDE-F1]` #9 | Fotografias da trilha podem citar campos de `LeituraDosAtos` | **CAIU o contrato pressuposto.** As fotografias atuais usam campos da pergunta. (`app/chamados/reconciliador.py:1145`; `app/chamados/reconciliador.py:1155`; `app/chamados/juizes.py:1544`.) |
| 15 | matriz:44 #10; saída:90 | C em SQL permanece na projeção | **CONFERE.** Órfãs reatáveis filtram C; a enumeração DP não tem MATERIALIZACAO. (`app/chamados/reconciliador.py:1505`; `app/chamados/models.py:4024`.) |
| 16 | matriz:45 — `[DEPENDE-F1]`, `[DEPENDE-F2]` #11 | Explicador herda resposta/estado pelo ato | **CAIU como implementação presente.** Lê `p.estado` e texto projetado. (`app/chamados/services/explicador.py:95`; `app/chamados/services/explicador.py:102`; `app/chamados/services/explicador.py:103`.) |
| 17 | brief:87,110; matriz:46,84 — `[DEPENDE-F1]`, `[DEPENDE-F2]` #12 | Validação pode ser lida pelo instante do redutor/face V disponível | **CAIU o suporte pressuposto.** A guarda atual testa V projetada. (`app/chamados/services/declaracao_texto.py:227`; `app/chamados/models.py:3496`.) |
| 18 | matriz:47 — `[DEPENDE-F1]` #13 | Já declarada/re-resposta compara a última RESPOSTA da leitura F1 | **CAIU como contrato presente.** Compara instante/texto projetados. (`app/chamados/services/declaracao_texto.py:417`; `app/chamados/services/declaracao_texto.py:419`.) |
| 19 | matriz:48 — `[DEPENDE-F1]` #14 | Aplicar declaração dispõe da resposta do redutor | **CAIU a interface.** O degrau atual decide pelo instante cru. (`app/chamados/services/declaracao_texto.py:687`.) |
| 20 | brief:33; matriz:49 #15 | Causa “c” do alcance ainda lê R cru | **CONFERE.** Não se confunde com o contador de integridade do par. (`app/chamados/services/vigia_fio.py:636`; `app/chamados/services/vigia_fio.py:737`.) |
| 21 | brief:52–54; matriz:50,89 #16 | `respondida_sem_resposta` mede o par gravado pela porta; classificação é técnica | **CONFERE a natureza documentada.** A função declara que não julga a pergunta e lê os campos escritos pela porta. Escolha técnica final da F5: **[nao verificado]**. (`app/chamados/services/vigia_fio.py:738`; `app/chamados/services/vigia_fio.py:740`; `app/chamados/services/vigia_fio.py:746`.) |
| 22 | brief:85,105–108; matriz:51,69–71,82 — `[DEPENDE-F1]` #17 | Resposta vazia deve voltar às pendentes do app | **CAIU pelo corte posterior.** Permanece fora do app; ação pertence ao admin. (`app/docs/CORTES.md:488`; `app/chamados/services/pergunta_no_app.py:81`.) |
| 23 | matriz:52 — `[DEPENDE-F1]` #18 | Beco dispõe da leitura por ato herdada | **CAIU a interface.** Visibilidade continua por instante nulo. (`app/chamados/services/becos.py:30`; `app/chamados/juizes.py:1544`.) |
| 24 | matriz:53,85 — `[DEPENDE-F1]` #19 | F1 fornece o accessor necessário ao histórico por atos | **CAIU esse suporte.** Histórico ainda gera eventos por campos; sequência DP existe separadamente. (`app/chamados/services/historico.py:98`; `app/chamados/services/historico.py:105`; `app/chamados/models.py:4056`.) |
| 25 | matriz:54; brief:87,110 — `[DEPENDE-F2]` #20 | Vigia carimbo já pode consultar V pela face F2 | **CAIU como suporte pousado.** Universo continua por V projetada. (`app/chamados/services/vigia_carimbo.py:98`; `app/chamados/models.py:3496`.) |
| 26 | brief:46,78; matriz:55 i1 | `_pela_decisao` já lê decisões | **CONFERE.** Lê o último ato para exigibilidade; isso não prova o antigo redutor F1. (`app/chamados/services/tarefa_do_dia.py:142`; `app/chamados/services/tarefa_do_dia.py:158`.) |
| 27 | brief:76; matriz:73 | Ordenação DP é `(ato_em, pk)` | **CONFERE.** (`app/chamados/models.py:4056`.) |
| 28 | brief:89,94–95; matriz:76 | C não tem ato; conserva o campo atual | **CONFERE.** Não há recuperação de instante apagado nessa projeção. (`app/chamados/models.py:4024`; `app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1400`.) |
| 29 | brief:64–72 | Os arquivos F5 não são persistidores diretos de R/T/V/C | **CONFERE no censo direto.** Os declaradores chamam portas existentes; lavras usam carimbo existente. (`app/chamados/services/declaracao_texto.py:419`; `app/chamados/services/declaracao_texto.py:693`; `app/chamados/reconciliador.py:406`.) |
| 30 | brief:64–72 | O inventário antigo esgota os escritores; atos correspondentes já cobrem toda escrita | **CAIU.** Faltavam nascimento, ressuscitação e devolução BUG-PAR; `carimbar(resposta=)` pode preencher R/V sem registrar os respectivos atos nesse corpo. (`app/chamados/models.py:3124`; `app/chamados/models.py:1629`; `app/chamados/services/validacao.py:349`; `app/chamados/models.py:3242`; `app/chamados/models.py:3262`.) |
| 31 | brief:45–46; matriz:57–58 | Furo sem canal/competência trancada não têm migração R/T/V/C própria identificada | **CONFERE para os corpos examinados.** Furo lê tarefa; competência conserva via. (`app/chamados/services/furo_sem_canal.py:94`; `app/chamados/services/competencia_trancada.py:276`; `app/chamados/services/competencia_trancada.py:282`.) |
| 32 | brief:41–46; matriz:55–56,94–95 | Tarefa, cobrança, consulta e consumidores externos herdam a partição | **CONFERE.** (`app/chamados/services/tarefa_do_dia.py:530`; `app/chamados/services/cobranca.py:48`; `app/core/views.py:253`; `app/api/views.py:998`; `app/api/views.py:2538`.) |
| 33 | brief:47–50; matriz:60 | F/VIA/X permanecem fora da migração R/T/V/C descrita | **CONFERE como separação dos campos.** Há leituras próprias de via, pendência, recusa e fechamento. (`app/chamados/services/explicador.py:108`; `app/chamados/services/explicador.py:113`; `app/chamados/services/explicador.py:118`; `app/chamados/services/becos.py:27`.) |
| 34 | brief:50; matriz:60 | A guarda conservada ainda é `recusa_respondida_sem_texto`, unilateral, para E4d | **CAIU essa descrição.** Agora é `recusa_par_ilegal`, simétrica, sobre o par que será escrito. (`app/chamados/services/fio.py:1603`; `app/chamados/services/fio.py:1613`; `app/chamados/services/fio.py:1618`.) |
| 35 | brief:126,154–155; matriz:6–9,92 | R3 histórica ainda é pré-requisito não pousado | **CAIU segundo o registro local.** R3 c17 tem pouso registrado. Ancestralidade independente: **[nao verificado]**. (`app/docs/RELATO.md:235`.) |
| 36 | brief:127,154–155; matriz:6–7,92 | F1 antiga ainda é a entrega a aguardar | **CAIU.** B1 real tem pouso registrado e contrato diferente. (`app/docs/RELATO.md:174`; `app/chamados/juizes.py:1544`.) |
| 37 | brief:128; matriz:108 — resumo `[DEPENDE-F2]` | F2 antiga fornece o suporte V/estado pressuposto | **CAIU como descrição da BASE.** Permanecem campos projetados; a B2 aparece aberta. (`app/chamados/models.py:3151`; `app/chamados/models.py:3496`; `app/docs/RELATO.md:30`.) |
| 38 | brief:129; matriz:7 | BUG-PAR ainda está fora do main | **CAIU segundo registro local.** Pousou c4+c4c. (`app/docs/RELATO.md:32`.) |
| 39 | brief:129–132 | BUG-PAR cruza explicador/fio | **CONFERE**, com alcance maior: testemunha, indisponibilidade e espera declarado×gravado. (`app/chamados/services/fio.py:376`; `app/chamados/services/fio.py:384`; `app/chamados/services/explicador.py:124`; `app/chamados/services/explicador.py:132`.) |
| 40 | brief:12–15,58–62; matriz:8–10 | O censo antigo e seus offsets bastam para esta BASE | **CAIU como inventário vigente.** Há novos consumidores e mudanças nos corpos. Os diffs históricos exatos: **[nao verificado]**. (`app/chamados/services/pergunta_no_app.py:100`; `app/chamados/services/raiox.py:147`; `app/chamados/services/explicador.py:124`.) |
| 41 | brief:136–138; matriz:98–102,110–111 | A pergunta própria F5 está resolvida, ficando zero pergunta própria de lei | **CONFERE após o corte.** A pergunta geral de devolver ao colaborador permanece expressamente não decidida. (`app/docs/CORTES.md:488`.) |
| 42 | brief:150–152 | A mudança de badge/fase pode ser presumida coberta sem conferir conservação pública | **CAIU como dispensa dessa conferência.** D05 conserva fases públicas e torna exigibilidade aditiva; o corte F5 conserva resposta vazia fora do app. (`app/docs/BACKLOG.md:2091`; `app/docs/CORTES.md:488`.) |
| 43 | brief:132,154–155; matriz:92–93 | A ordem histórica representa todos os pousos ainda pendentes | **CAIU como fila atual.** R3/B1/BUG-PAR têm pouso registrado; B2 está aberta. Situação do hash RELAVRA-11 citado: **[nao verificado]**. (`app/docs/RELATO.md:235`; `app/docs/RELATO.md:174`; `app/docs/RELATO.md:32`; `app/docs/RELATO.md:30`.) |
| 44 | brief:139–146; matriz:77–78,91 | A aproximação histórica serve como medida atual pela função real | **CAIU como medição atual.** A autoridade real disponível de correspondência é outra; valores atuais e antiga comparação por fold: **[nao verificado]**. (`app/chamados/models.py:4081`; `app/chamados/juizes.py:1544`.) |
| 45 | brief:156–157 | Publicação de código exige rito de suíte/deploy e cascas juntas | **CONFERE como regra operacional escrita**, sem execução nesta leitura. (`CLAUDE.md:90`; `CLAUDE.md:91`; `CLAUDE.md:95`.) | [nao verificado]

Os hashes/diffs históricos de RELAVRA-11, LOTE7 e UI-CAMERA não foram demonstrados pela cópia. Suas afirmações históricas de interseção/ausência de interseção permanecem **[nao verificado]**; não foram convertidas em confirmação de Git.

DEPENDE CAIU 32 de 45

## 2 Censo escritores e leitores

R=`respondida_em`; T=`resposta_colab`; V=`validada_em`; C=`materializada_em`. O levantamento combinou AST de atributos, atribuições, keywords ORM, strings de campos e chamadas/aliases, com grep de templates. Cobertura de nomes inteiramente construídos em execução: **[nao verificado]**. A distinção entre projeção e sequência está documentada no modelo. (`app/chamados/models.py:4007`; `app/chamados/models.py:4011`.)

**NOVO** significa ausente da enumeração do brief, inclusive novo sítio dentro de arquivo já citado.

**Escritores e conservação**

| Campo/objeto | Escritores encontrados |
|---|---|
| R/T — humana | `responder_pergunta_disputa_core`: atribuição/persistência e RESPOSTA. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1483`; `app/chamados/services/disputa_emissao.py:1525`.) |
| R/T — automática | `responder_automaticamente`: conserva anterior, escreve par e RESPOSTA. (`app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1678`.) |
| R — trilha | `carimbar_resposta_pela_trilha`. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1699`.) |
| V/autor | Validação; nova resposta; devolução; **NOVO** devolução BUG-PAR. (`app/chamados/services/validacao.py:168`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/validacao.py:273`; `app/chamados/services/validacao.py:349`.) |
| R/T/V por lote | Reabertura fotografa e passa zeragem à porta; **NOVO** ressuscitação conserva anterior e zera o par inteiro. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1648`; `app/chamados/models.py:1613`; `app/chamados/models.py:1629`; `app/chamados/models.py:1630`.) |
| **NOVO** R/V por carimbo | `carimbar(resposta=)` pode preencher R/V; o corpo salva sem registrar RESPOSTA/VALIDACAO correspondentes. (`app/chamados/models.py:3240`; `app/chamados/models.py:3242`; `app/chamados/models.py:3245`; `app/chamados/models.py:3262`.) |
| C/autor/via | `_escrever_carimbo` atribui em memória; portas e lote persistem. (`app/chamados/models.py:3209`; `app/chamados/models.py:3210`; `app/chamados/models.py:3211`; `app/chamados/models.py:3262`; `app/chamados/models.py:3299`; `app/chamados/models.py:3322`; `app/chamados/models.py:3393`.) |
| **NOVO** nascimento | `_nascer` recebe `**campos`, salva e conserva projeção sem linha. (`app/chamados/models.py:3124`; `app/chamados/models.py:3127`; `app/chamados/models.py:3129`.) |
| Apenas memória | Simulação da ressuscitação e reversão do carimbo. (`app/chamados/models.py:1554`; `app/chamados/models.py:1555`; `app/chamados/models.py:1556`; `app/chamados/models.py:3273`; `app/chamados/models.py:3274`.) |
| Homônimo excluído | `regularizacao_ext.py:199` escreve outro modelo; não C da pergunta. (`app/chamados/services/regularizacao_ext.py:199`; `app/chamados/models.py:4152`.) |
| DP — persistidor | `DecisaoPergunta.registrar`, com `bulk_create` interno. (`app/chamados/models.py:4110`; `app/chamados/models.py:4120`.) |

**Produtores e leitores da sequência**

| Papel | Sítios |
|---|---|
| Produtores DP no model | Conservação/nascimento, SILENCIO e RETRATACAO. (`app/chamados/models.py:1613`; `app/chamados/models.py:2005`; `app/chamados/models.py:2137`; `app/chamados/models.py:3129`.) |
| Produtores DP nos serviços | Resposta/reabertura/automática/trilha; validação/devoluções; absorção. (`app/chamados/services/disputa_emissao.py:1525`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1678`; `app/chamados/services/disputa_emissao.py:1699`; `app/chamados/services/validacao.py:173`; `app/chamados/services/validacao.py:279`; **NOVO** `app/chamados/services/validacao.py:354`; `app/chamados/services/materializacao.py:1045`.) |
| Outros produtores DP | Veredito de célula e backfills. (`app/ponto/services/veredito_celula.py:195`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.) |
| Leitores DP | Respostas anteriores/correspondência; prefetch; tarefa; foto; medidor. (`app/chamados/models.py:3675`; `app/chamados/models.py:4094`; `app/chamados/services/fio.py:263`; `app/chamados/services/tarefa_do_dia.py:159`; `app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:430`; `app/chamados/services/foto_e4b.py:280`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:315`.) |
| **NOVO** leitor BUG-PAR | Rótulo consulta decisões. (`app/chamados/services/materializacao.py:275`.) |

**Censo global dos campos em produção**

Cada número nas colunas abaixo significa `arquivo da primeira coluna:linha`. Inclui leitura, escrita, filtros e transporte; os escritores estão classificados acima. Os sítios exteriores aos 14 arquivos do brief ampliam o censo; não implicam alteração automática pela F5.

| Arquivo | R | T | V | C |
|---|---|---|---|---|
| `app/chamados/services/fio.py` | 187,706,1613 | **NOVO** 377; 1279,1291,1613 | — | — |
| `app/chamados/reconciliador.py` | — | 365,1083 | 631,1050 | 367,630,1050,1145,1155,1505 |
| `app/chamados/services/explicador.py` | — | 55,102,103 | — | — |
| `app/chamados/services/declaracao_texto.py` | 417,419,687 | 417 | 227 | — |
| `app/chamados/services/vigia_fio.py` | 636,746 | 749 | — | — |
| `app/chamados/services/raiox.py` | 138,147 | 138,147 | — | 135,182 |
| `app/chamados/services/pergunta_no_app.py` | 81 | — | — | — |
| `app/chamados/services/becos.py` | 30 | — | — | — |
| `app/chamados/services/historico.py` | 99 | 109 | 101 | 103 |
| `app/chamados/services/vigia_carimbo.py` | — | — | 98 | — |
| `app/chamados/models.py` | 1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100 | 1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105 | 310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104 | 1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485 |
| `app/chamados/juizes.py` | 

[truncado em 20000 de 65829 caracteres]

## 17/11 01:19 · sessao dd1609596b5a · PREPARA-F3-F5-F6-F8-F9 · leitura · origem=Code

## 1 DEPENDE e premissas

BASE: cópia identificada no pedido por `fbc5c76ced33bf25943f3a5bc78ebff257d68c56`. Confirmação independente do HEAD/main: **[nao verificado]**. Banco, logs, execução de testes e medidas atuais de produção: **[nao verificado]**.

**A premissa central caiu:** a F1 atual fornece `pergunta_respondida`, sobre instante **e** texto da projeção. O D01 conservado pressuposto pelos anexos não é o contrato desses corpos. A face SQL-R existe, mas também consulta a projeção. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1580`)

As referências `brief:NN` e `matriz:NN` abaixo numeram os anexos do enunciado, contando o título como linha 1; correspondência com os arquivos externos originais: **[nao verificado]**. A contagem considera **uma premissa por linha da tabela**, agrupando repetições explicitamente indicadas; CAIU parcial entra em CAIU.

Abreviações usadas nas referências seguintes:

| Sigla | Arquivo da BASE |
|---|---|
| J | `app/chamados/juizes.py` |
| MD | `app/chamados/models.py` |
| A | `app/chamados/management/commands/auditar_invariantes_chamados.py` |
| B | `app/chamados/management/commands/backfill_decisoes_pergunta.py` |
| L | `app/chamados/management/commands/curar_perguntas_em_limbo.py` |
| G | `app/ponto/management/commands/reconciliar_grade.py` |
| S | `app/chamados/tests/test_contract_leitores_vitalidade.py` |
| cmd/ | `app/chamados/management/commands/` |
| pcmd/ | `app/ponto/management/commands/` |
| T/ | `app/chamados/tests/` |

Assim, `A:177` significa `app/chamados/management/commands/auditar_invariantes_chamados.py:177`.

| # | Origem: marca ou premissa | Julgamento na BASE |
|---|---|---|
| 1 | `[DEPENDE-F1]` D01/`atos_pergunta.leitura` conservado — brief:28; matriz:22; P4:100–102; resumo:139–141 | **CAIU.** O leitor atual recebe os campos projetados; o juiz de respondida testa esse par. (`J:1389`; `J:1423`; `J:1552`) |
| 2 | `[DEPENDE-F1]` SQL-R lê atos; offsets da F1 antiga — brief:29; matriz:12,23 | **CAIU quanto à fonte.** Os nomes SQL existem, mas leem R/T projetados. (`J:1565`; `J:1572`; `J:1575`; `J:1580`) |
| 3 | `[DEPENDE-F1]` walker `leituras_rvc` reutilizável da F1 — brief:71–73,77,110; matriz:109 | **CAIU como recurso disponível.** O walker vigente é `ofensores`, com alcance menor. O walker histórico: **[nao verificado]**. (`S:14`; `S:310`; `S:328`) |
| 4 | `[DEPENDE-F2]` `_q_validada_pelo_ato` disponível, inclusive questão público/privado — brief:30,135–137; matriz:13,23,126–128 | **CAIU como API da BASE.** `q_vivas` ainda filtra `validada_em`; o corte autoriza a face do juiz, mas não demonstra aquela implementação presente. (`MD:3496`; `app/docs/CORTES.md:488`) |
| 5 | `[DEPENDE-F2]` selo F2 por função já disponível para união — brief:74–75 | **CAIU como pré-requisito demonstrado.** O contrato presente exclui models/juízes e só cobre C/V. O selo histórico citado: **[nao verificado]**. (`S:14`; `S:320`) |
| 6 | `[DEPENDE-F1]` D01/SQL-R daria `ORFA_FIO_PRESO` para RESPOSTA somente no ato — brief:84; matriz:33 | **CAIU o oráculo antigo.** O filtro atual é R projetado; o juiz atual também não transforma a existência de RESPOSTA em par respondido. (`G:460`; `G:461`; `J:1552`) |
| 7 | `[DEPENDE-F1]` D06/D08: `fio_vivo` e `pergunta_viva` já leem atos conservados — matriz:44,48 | **CAIU a fonte pressuposta.** `fio_vivo` chama C6; C6 começa pela projeção temporal. (`J:1519`; `J:1404`; `J:1389`) |
| 8 | `[DEPENDE-F1]` D11/D14/D15: nota, rótulo dedup e impressão classe A podem consumir D01 — matriz:54,59,60 | **CAIU como dependência satisfeita.** Os três leem T diretamente. (`L:32`; `cmd/dedup_perguntas_fato.py:75`; `cmd/validar_classe_a.py:80`; `J:1552`) |
| 9 | `[DEPENDE-F1]` D22 contraprova I21a recebe D01-R — matriz:71 | **CAIU.** A contraprova ainda testa apenas R. (`A:177`) |
| 10 | `[DEPENDE-F1]` D23 rótulo F1t recebe D01-V — matriz:72 | **CAIU.** O rótulo usa V projetada depois de consultar C6. (`A:202`; `A:205`) |
| 11 | `[DEPENDE-F1]` D26 I2 recebe D01-R — matriz:75 | **CAIU.** O piso ainda usa somente instante. (`A:592`) |
| 12 | `[DEPENDE-F1/F2]` D32 I21 precisa migrar R cru e V para as interfaces antigas — matriz:81 | **CAIU parcialmente.** R já usa a face SQL atual; V/C continuam projetados. (`A:1574`; `A:1575`; `A:1576`; `A:1577`) |
| 13 | `[DEPENDE-F1/F2]` D33 C2 dispõe de SQL-R/SQL-V por atos — matriz:82 | **CAIU.** C2 ainda exige R/V/C nulos da projeção. (`A:1604`; `A:1605`) |
| 14 | `[DEPENDE-F1]` D34 F1 dispõe de D01 para texto/vida — matriz:83 | **CAIU a interface.** C6 e texto continuam sendo leituras distintas, sendo T lida diretamente. (`A:1655`; `A:1657`; `J:1389`) |
| 15 | `[DEPENDE-F1/F2]` D35/D36 usam C6 por ato e migrarão V pela face existente — matriz:84–85 | **CAIU.** Os pré-filtros V são projetivos e C6 também começa pela projeção. (`A:1768`; `A:1780`; `J:1404`) |
| 16 | `[DEPENDE-F2]` D05/D07 dispõem da seleção V por ato — matriz:43,47 | **CAIU.** Encerrar e reabrir órfãs filtram V projetada. (`cmd/encerrar_perguntas_com_fato.py:32`; `cmd/reabrir_orfas_enterradas.py:68`) |
| 17 | `[DEPENDE-F2]` D29/I18 dispõe de V e autor vigentes pelo ato — matriz:78,95–97,128 | **CAIU como implementação presente.** A consulta usa V e `validada_por__username` projetados. (`A:1075`; `A:1076`) |
| 18 | `[DEPENDE-F1/F2]` conservação dos legados pelos folds antigos; divergências V — brief:86–97; matriz:87–90 | **CAIU a explicação pelo fold disponível.** A conservação atual dos valores vem dos campos; lacunas da sequência são medidas por correspondência de instante. Números atuais: **[nao verificado]**. (`J:1389`; `MD:4094`; `MD:4098`; `MD:4102`) |
| 19 | Nenhum dos 12 comandos foi tocado pela F1 — brief:13–16; matriz:10 | **CAIU como descrição da BASE atual.** I21 contém a adoção explícita da face SQL da F1 nova. O diff histórico dos hashes: **[nao verificado]**. (`A:1572`; `A:1574`) |
| 20 | Retrato BUG-PAR limitado a crons/teste, sem dependência relevante para esses leitores — brief:123 | **CAIU como retrato atual suficiente.** A BASE registra c4+c4c e contém composição K6, testemunha e devolução próprias. Interseção histórica exata do hash antigo: **[nao verificado]**. (`app/docs/BACKLOG.md:2084`; `app/chamados/services/materializacao.py:792`; `app/chamados/services/validacao.py:349`) |
| 21 | RELAVRA-11 cruza backfill/reabrir; seus sítios precisam ser reconferidos — brief:114–118,125–127,160; matriz:120–121 | **CONFERE a dependência dos sítios.** A BASE ainda mostra `marcos_da_competencia` e `competencia_entregue` nesses corpos. Pouso do hash citado: **[nao verificado]**. (`cmd/reabrir_orfas_enterradas.py:95`; `B:453`; `B:471`; `B:498`) |
| 22 | C fica projetado até E4d; nenhum ato de conclusão atual — brief:31–32,131–133; matriz:124 | **CONFERE.** O ato de conclusão está registrado para E4d; escrita dupla morre no Contract. (`app/docs/BACKLOG.md:1981`; `MD:4024`) |
| 23 | Backfill, reversões e I20 examinam integridade da própria projeção — brief:33,48–50; matriz:106–107 | **CONFERE.** Há correspondência pergunta/ato/instante, fotos antes/depois e I20 projetivo. (`MD:4094`; `cmd/reabrir_orfas_enterradas.py:129`; `A:1105`) |
| 24 | Construtor pode escolher livremente limbo projetivo ou “ato sem projeção” — brief:138–140; matriz:118–119,128 | **CAIU como escolha técnica livre.** O comando tem corte próprio e recusa mudança do conjunto nominal. (`L:3`; `L:13`; `L:17`; `L:46`; `L:48`) |
| 25 | Correção da matriz: existe selo parcial; R e cargas gerais ficam fora — matriz:14–19 | **CONFERE.** O selo cobre C/V e formas específicas de comparação. (`S:14`; `S:328`; `S:330`; `S:332`) |
| 26 | Existem os três importadores produtivos enumerados — brief:147–148; matriz:111–112 | **CONFERE.** Relavra importa I6; dedup importa o apply; worklist importa parser/catálogo. (`cmd/relavrar_via_i6.py:46`; `cmd/dedup_perguntas_fato.py:104`; `app/ponto/worklist.py:37`) |
| 27 | P8: `universo_i6` devolve `(pk, materializada_em)` — matriz:111 | **CAIU.** Retorna `(set de violações, dict de pisos)`; aqueles pares são consulta interna. (`A:457`; `A:470`; `A:484`; `cmd/relavrar_via_i6.py:49`) |
| 28 | Selo global só fecha após todos os leitores, não apenas os 12 — brief:78,126–127; matriz:108–110 | **CONFERE a condição.** Contract só apaga depois de nenhum leitor antigo permanecer. A lista efetiva das demais fatias: **[nao verificado]**. (`app/docs/PLANO-PADROES.md:92`; `app/docs/PLANO-PADROES.md:94`; `app/docs/BACKLOG.md:2036`) |
| 29 | Os 12 não escrevem diretamente atributos RVC; as operações seguem portas — brief:21–22; matriz:113–115 | **CONFERE no censo estático.** Fotos não são atribuições de campo; reabertura, correção e registro seguem portas. (`cmd/reabrir_orfas_enterradas.py:129`; `cmd/reabrir_orfas_enterradas.py:157`; `L:67`; `L:69`; `B:324`) |
| 30 | Três crons e “nada persistido” pelas auditorias — brief:141–146; matriz:116–117 | **CAIU parcialmente.** Agendas/flags conferem, mas auditor com `--html` grava arquivo. Grade está sem apply; alarme desliga apply dos fantasmas. (`app/config/crons.py:392`; `app/config/crons.py:462`; `app/config/crons.py:542`; `A:1855`; `pcmd/reconciliar_fantasmas.py:153`) |

DEPENDE CAIU 23 de 30

## 2 Censo escritores e leitores

R=`respondida_em`; T=`resposta_colab`; V=`validada_em`; VP=`validada_por[/_id]`; C=`materializada_em`; CP=`materializada_por[/_id]`. Os campos pertencem à pergunta nos seguintes sítios; o homônimo de regularização externa é outro modelo. (`MD:2552`; `MD:2553`; `MD:2554`; `MD:3514`; `MD:3530`; `MD:3534`; `MD:4152`)

O levantamento combina AST de atributos, keywords e strings ORM com busca textual de templates/importadores. **NOVO** significa ausente do inventário correspondente do brief, sem afirmar criação recente. Resolução de nomes inteiramente dinâmicos: **[nao verificado]**.

**Escritores e portas, além dos comandos:**

| Campos | Escritor, persistência e sequência |
|---|---|
| R/T — **NOVO** | Resposta humana: atribui e persiste o par; registra RESPOSTA. (`app/chamados/services/disputa_emissao.py:1481`; `:1482`; `:1483`; `:1525`) |
| R/T — **NOVO** | Automática: conserva anterior, escreve par e RESPOSTA. (`app/chamados/services/disputa_emissao.py:1673`; `:1674`; `:1675`; `:1677`; `:1678`) |
| R | Reparo pela trilha: carimbo e RESPOSTA retroativa. (`app/chamados/services/disputa_emissao.py:1696`; `:1698`; `:1699`) |
| V/VP — **NOVO** | Nova resposta invalida a validação anterior. (`app/chamados/services/disputa_emissao.py:1494`; `:1495`; `:1496`) |
| V/VP — **NOVO** | `_carimbar_validacao`: campos e VALIDACAO. (`app/chamados/services/validacao.py:168`; `:169`; `:171`; `:173`) |
| V/VP — **NOVO** | Devolução recusada e devolução BUG-PAR: zeragem e DEVOLUCAO. (`app/chamados/services/validacao.py:273`; `:276`; `:279`; `:349`; `:351`; `:354`) |
| C/CP — **NOVO** | `_escrever_carimbo`; persistem carimbo comum, identidade e descarimbo. (`MD:3209`; `MD:3210`; `MD:3262`; `MD:3300`; `MD:3322`) |
| R/V/VP — **NOVO** | `carimbar(resposta=)` também preenche esses campos; o corpo salva sem registrar RESPOSTA/VALIDACAO. (`MD:3242`; `MD:3245`; `MD:3246`; `MD:3262`) |
| C/CP e campos adicionais — **NOVO** | `descarimbar_em_lote`; ressuscitação e reabertura passam a zeragem por ele. (`MD:3391`; `MD:3393`; `MD:1629`; `MD:1630`; `app/chamados/services/disputa_emissao.py:1609`; `:1648`) |
| R/T por entrada — **NOVO** | `_nascer` recebe campos, instancia/salva e conserva projeção sem linha. (`MD:2949`; `MD:2950`; `MD:3124`; `MD:3127`; `MD:3129`) |
| Apenas memória — **NOVO** | Simulação de ressuscitação e restauração após rollback. (`MD:1554`; `MD:1555`; `MD:1556`; `MD:3273`; `MD:3274`; `MD:3275`) |
| DecisaoPergunta | Escritor físico único `registrar`, com escrita em lote interna. (`MD:4110`; `MD:4120`) |

**Todos os sítios RVC encontrados nos 12 comandos:**

| Arquivo | Leituras, seleção ou transporte |
|---|---|
| `cmd/auditar_invariantes_chamados.py` | R `:177,592,1105,1605`; T `:1106,1657`; V `:205,1075,1576,1604,1768,1780`; VP `:1076`; C `:470,538,588,905,955,1075,1156,1577,1604,1769`. |
| `cmd/backfill_decisoes_pergunta.py` | R `:56,60`; T `:61,326,504`; V `:56,60,102`; VP `:60`; C `:101,325`; CP `:326`. |
| `cmd/reabrir_orfas_enterradas.py` | C `:67,88,91,129,142,160`; CP `:130,143,160`; V `:68`. |
| `cmd/curar_perguntas_em_limbo.py` | R `:46,69`; T `:32,46`. |
| `cmd/auditar_disputas.py` | C `:71,150`. |
| `cmd/encerrar_perguntas_com_fato.py` | C/V `:32`. |
| `cmd/dedup_perguntas_fato.py` | T `:75`. |
| `cmd/migrar_data_evento_dia_do_turno.py` | C `:147`. |
| `cmd/validar_classe_a.py` | T `:80`. |
| `cmd/relavrar_via_i6.py` | Sem acesso RVC direto; herda I6 por importação `:46` e chamada `:49`. |
| `pcmd/reconciliar_fantasmas.py` | C `:213`. |
| `pcmd/reconciliar_grade.py` | R `:461`. |

**NOVO no inventário repo-wide do brief: leitores, seletores e transportadores fora dos 12.** Esta enumeração inclui os corpos escritores quando também leem/fotografam os campos; não transforma toda ocorrência em juiz.

```text
app/api/views.py:1163,2561 [T]; :1165,2563,2577,2620,2637 [R]
app/api/views_mensageria.py:567 [T]; :569,570 [R]
app/chamados/canal.py:65 [T]

app/chamados/juizes.py:292,683,711,858,1390,1502 [C]
 :352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753 [R]
 :364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052 [T]
 :856,1389 [V]

app/chamados/models.py:
 :1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,
  3331,3365,3393,3411,3423,3440,3483,3485 [C]
 :3210,3236,3238,3293,3296,3320,3365,3393 [CP]
 :1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,
  3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100 [R]
 :1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,
  3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105 [T]
 :310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,
  3496,4091,4102,4103,4104 [V]
 :1614,1629,3237,3246,3247,3275,4105 [VP]

app/chamados/reconciliador.py:367,630,1050,1145,1155,1505 [C]
 :1146,1156 [CP]; :365,1083 [T]; :631,1050 [V]
app/chamados/services/acoes_disputa.py:97,132 [R]; :355 [T]; :347 [V]
app/chamados/services/becos.py:30 [R]
app/chamados/services/declaracao_texto.py:417,419,687 [R]; :417 [T]; :227 [V]
app/chamados/services/disputa_emissao.py:
 :228,776,1084,1104,1340,1637,1930,2062,2073,2090 [C]
 :1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091 [R]
 :1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079 [T]
 :1484,1485,1494,1496,1537,1538,1609,1636,2062 [V]
 :1484,1495,1496,1537,1538,1609,1636 [VP]
app/chamados/services/explicador.py:55,102,103 [T]
app/chamados/services/fabrica_da_celula.py:171 [C]
app/chamados/services/fio.py:187,706,1613 [R]; :377,1279,1291,1613 [T]
app/chamados/services/foto_e4b.py:180 [C,R,V]
app/chamados/services/historico.py:99 [R]; :101 [V]; :103 [C]; :109 [T]
app/chamados/services/materializacao.py:
 :148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127 [C]
 :1154 [CP]; :1151,2064 [R]
 :546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,
  1534,1558,1567,1619 [T]
 :199,201,1152,2065,2082 [V]; :1152 [VP]
app/chamados/services/painel.py:67 [C,R,V]; :270,271,272,273 [V]; :262 [VP]
app/chamados/services/passivo_validado.py:64,534,535,536 [C]; :64,535 [CP]; :66 [R,V,VP]
app/chamados/services/pergunta_no_app.py:81 [R]
app/chamados/services/raiox.py:135,182 [C]; :138,147 [R,T]
app/chamados/services/relavra_via.py:181,244,252 [C]
app/chamados/services/validacao.py:
 :262,264,341,363,556,681,701,717 [C]; :342,363 [CP]
 :594,813,824,848,1167 [R]
 :175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109 [T]
 :168,171,173,181,262,269,273,276,300,302,343,349,351,364,489,553,597,
  701,810,975,996 [V]
 :169,171,181,262,274,276,300,302,343,350,351,364,489 [VP]
app/chamados/services/verbos.py:335,336,342,345 [R]; :264,270 [V]; :264,265,266,268 [VP]
app/chamados/services/vigia_carimbo.py:98 [V]
app/chamados/services/vigia_fio.py:636,746 [R]; :749 [T]
app/chamados/templatetags/chamados_tags.py:139,166 [T]
app/chamados/veredito_lavrado.py:203,204 [C]; :207,208 [R]
app/chamados/views.py:1257 [C]; :1280,1416 [R]
app/chamados/views_cobrar.py:169,176 [R]; :291 [T]
app/chamados/management/commands/bug_par_103.py:66 [C]; :67 [CP]; :68 [V,VP]

app/colaboradores/services/calendario.py:504 [T]
app/colaboradores/services/ficha.py:171 [T]
app/folha/previa.py:142 [T]
app/ponto/services/cartorio.py:1641 [T]
app/ponto/services/dia_do_colab.py:70 [T]
app/ponto/services/flip_auto.py:130 [V]
app/ponto/services/procedencia_batida.py:74 [C,V]; :71,72 [CP,VP]
app/ponto/services/veredito_celula.py:196 [C]; :197 [T]

bin/censo_folha.py:28,29 [R]; :29 [V]
bin/sombra_regras_pessoais.py:85 [T]

app/templates/chamados/partials/_card_atendimento.html:
 :106,143,391 [R,T/display]; :114,141,148,154,182,183 [T]
app/templates/chamados/partials/_lote_preview.html:24 [T]
app/templates/chamados/partials/_pergunta_card.html:20,23,42 [V]; :24,29,32 [R]
app/templates/chamados/partials/modal_fio.html:185,217,220 [R]; :219 [T]; :228 [V]
```

**Sequência DP e conservação também entram no censo:**

| Objeto | Escritores/leitores |
|---|---|
| Produtores de atos | Resposta/reabertura/automática/trilha: `app/chamados/services/disputa_emissao.py:1525,1631,1678,1699`; validação/devoluções: `app/chamados/services/validacao.py:173,279,354`; absorção: `app/chamados/services/materializacao.py:1045`; silêncio/retratação: `MD:2005,2137`; silêncio do espelho: `app/ponto/services/veredito_celula.py:195`; backfill: `B:324`. |
| Leitores do helper de correspondência | `MD:1613,3129`; `app/chamados/services/disputa_emissao.py:1480,1673`; `app/chamados/services/validacao.py:167,272,345`; **NOVO** `app/chamados/services/tarefa_do_dia.py:430`; `B:67`; **NOVO** `cmd/backfill_fio_morto_e4b.py:141`. |
| Outros leitores DP — **NOVO** | Histórico `MD:3675`; rótulo BUG-PAR `app/chamados/services/materializacao.py:275`; tarefa `app/chamados/services/tarefa_do_dia.py:159,178,421`; prefetch `app/chamados/services/fio.py:263`; fotografia `app/chamados/services/foto_e4b.py:280`; backfill `B:99,315`. |

**Funções da fatia e consumidores encontrados:**

| Função | Produção | Testes/contratos consumidores | [nao verificado]
|---|---|---|
| `nota_da_resposta` | `L:56` | Mock em `T/test_curar_perguntas_em_limbo.py:65`. |
| `linhas_a_criar` | `B:455` | `T/test_e1_v6_horizonte.py:125`; registros em `T/test_e3_catraca_respondedores.py:68,81`; `S:43`. |
| `admin_decidiu_sem_ato` | `B:487,488` | `T/test_e1_v13d_exportada_fora.py:64,73,88,156,157`; `T/test_e1_v6_horizonte.py:130,302,303,312,335`; `S:282`. |
| `universo_i6` | `A:496,737`; `cmd/relavrar_via_i6.py:49` | `T/test_selo_lapide_legado.py:89`; `T/test_selo_i6_ouve_a_lei.py:166,178,188,199`; registro em `app/core/tests/test_medida_turno_do_par.py:367`. |
| `_i6_pares` | `A:461` | Substituição/restauração em `T/test_selo_lapide_legado.py:85,87,91`; `S:89`. |
| `_contraprova_i21a` | `A:1587` | Chamador direto de teste: **NENHUM localizado**; chamadas dinâmicas **[nao verificado]**. | [nao verificado]
| `_contraprova_f1t` | `A:1723` | Registro `S:277`; chamador direto de teste: **NENHUM localizado**; chamadas dinâmicas **[nao verificado]**. | [nao verificado]
| `_relavrar_orfa` | `G:228` | `app/escala/tests/test_cobranca_orfa_regeneracao.py:301,314,320,328,345`; **NOVO** `T/test_porta_chamado_e2.py:65,72`. |
| `classificar` do migrador | `cmd/migrar_data_evento_dia_do_turno.py:212` | `T/test_e1_v15f.py:351,357,362,366,372,376,381`; registros `S:45`; `app/core/tests/test_medida_turno_do_par.py:169,265`. |
| `aplicar`/`CONTADORES_DO_APPLY` | `cmd/dedup_perguntas_fato.py:104` | Família backfill/dedup; exemplos `T/test_e1_v9.py:212,235,250,261`. |
| `_parse_pendentes`/`MODULOS_ALVO` | `app/ponto/worklist.py:37` | Contratos do consumidor: alcance além das referências inspecionadas **[nao verificado]**. |
| Walker atual `ofensores` | Sem consumidor de domíni

[truncado em 20000 de 54349 caracteres]

## 17/11 01:16 · sessao 01a855b0f46d · PREPARA-F2-F4-F7 · sonda · origem=Code

**O anexo recebido está truncado: termina em `pergunta=53057 a`, dentro do SCRIPT 1. Não aparecem totalizadores, resultados de RED nem mensagens de ERRO. A entrega `logs/ESPINHA-BRIEFS/PREPARA-1611/f7.r` também não foi localizada nesta cópia; a associação exata às perguntas das seções 4 e 5 fica [nao verificado].**

| Rótulo recebido | Valor visível | O que confirma, derruba ou dimensiona |
|---|---|---|
| `ITEM backfill_recorte … ato=resposta` | IDs individuais; exemplo: col283, pergunta **33692**. [nao verificado] | Confirma presença de itens **listados pela sonda**; não fornece o total do recorte nem prova execução do backfill. O significado exato do filtro da sonda permanece [nao verificado]. |
| `ITEM backfill_recorte … ato=validacao` | IDs individuais; exemplo: col950, pergunta **55894**. [nao verificado] | Confirma presença dessa categoria na listagem; não dimensiona seu universo completo nem demonstra mordida de RED. [nao verificado] |
| Mesma pergunta nos dois atos | Exemplo: col503, pergunta **33719**, aparece como resposta e validação. [nao verificado] | Não demonstra duplicação indevida: o código trata resposta e validação separadamente e verifica correspondência por **pergunta + ato + instante**. `app/chamados/models.py:4094`, `:4098`, `:4102`. |
| `pergunta=NNNNN` e `colNNN` | Identificadores, não quantidades. [nao verificado] | Cada ocorrência identifica um item; não corresponde a “lei N” nem a “RED que morde n”. A ligação individual às seções anteriores permanece [nao verificado]. |

No código, `linhas_a_criar()` consulta a função que encontra resposta/validação da projeção sem linha correspondente na sequência; isso mede **linhas candidatas**, não comprova escrita. `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `:67`, `:82`; `app/chamados/models.py:4081`.

**Scripts com ERRO:** nenhum erro está visível no trecho recebido; causa [nao verificado]. Ficaram sem número os totais completos do recorte, os demais resultados das SONDAS e qualquer execução de RED; truncamento não comprova falha do script. [nao verificado]

**Contagens da entrega:** nenhum resultado recebido sustenta alterar “lei N” ou “RED que morde n”; seus valores específicos permanecem [nao verificado]. Existe um registro de **F7 “lei 0”** em `app/docs/RELATO.md:403`, mas sua identidade com a entrega PREPARA solicitada é [nao verificado]. Para fechar a avaliação pedida, faltam o `f7.r` e a continuação integral da saída. [nao verificado]

## 17/11 01:16 · sessao 359cd9de0f44 · - · - · origem=Ronald (tmux)

- A cópia registra F7 com **lei 0**, 22 linhas em quatro templates, dependências F1+F2+BUG-PAR e smoke no pouso (`app/docs/RELATO.md:403`).
- A grade aponta para `logs/ESPINHA-BRIEFS/F7/matriz.md` e `brief.md`, com referência de commit marcada `[nao verificado]` (`app/docs/GRADE-MESTRA.md:1808`).
- Não localizei `logs/ESPINHA-BRIEFS/PREPARA-1611/f7.r`, a entrega PREPARA anterior, suas seções 4/5 nem a contagem “RED que morde n”; portanto, o conteúdo e os totais específicos dessa entrega permanecem [nao verificado].
- O registro “lei 0” acima pertence ao brief F7 citado pelo RELATO; sua identidade com a entrega PREPARA pedida pelo usuário permanece [nao verificado].

## 17/11 01:12 · sessao ec551772e0be · REVISAO-AVAL-RD1711 · verificador · origem=Code

1. **Premissas: conferem, exceto a garantia de criação discutida no item 3.** K6 hoje exclui U163; a validação exige resposta, ausência de validação anterior, passagem pela espera declarado × gravado e pelo implausível. (`app/chamados/services/materializacao.py:795`; `app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:597`; `app/chamados/services/validacao.py:609`; `app/chamados/services/validacao.py:615`)

   Entregue com `FechamentoMensal` aberto é compatível: K6 consulta exportação vigente; F6 barra fechamento aprovado/exportado. B1 também confere: o juiz preserva L-123-MEDIDA-EM-PY, e o hook já chama esse juiz. A causa concreta de `4633d2c17` permanece **[nao verificado]**. (`app/folha/export.py:564`; `app/ponto/janelas.py:86`; `bin/tests/test_lei_akita.sh:348`; `bin/commit_msg.sh:29`)

2. **Colisões: nenhuma nova identificada nos registros conferidos.** F4 sobre B2 já é o encaixe declarado; a inclusão de U163 em L1/L5 sucede expressamente o tratamento anterior, preservando F6. Continua obrigatória a condição J1: F4 logo atrás de F1/F2 ou contador esperado zero no intervalo. (`app/docs/RELATO.md:26`; `app/docs/RELATO.md:28`; `app/docs/BACKLOG.md:823`; `app/docs/CORTES.md:488`)

   BACKLOG e resumo do brief CLASSE-D registram R1 antes de F2; o registro posterior dispensa F2 de esperar CLASSE-D. LU2-07/LU2-08 conservam seus ramos abertos, sem decisão nova por este aval. Conferência do brief integral: **[nao verificado]**; seu resumo consta no RELATO. (`app/docs/BACKLOG.md:1990`; `app/docs/RELATO.md:421`; `app/docs/RELATO.md:4`; `app/docs/RELATO.md:352`; `app/docs/RELATO.md:354`)

   B preserva a permissão cuja supressão motivou a devolução e mantém a exceção nominal fechada, com catraca 13. (`app/docs/RELATO.md:7`; `bin/tests/test_lei_akita.sh:348`; `bin/tests/test_lei_akita.sh:411`; `bin/tests/test_lei_akita.sh:413`)

3. **Literal: ainda não fecha a contagem `+1` de A4.** A busca de batida existente não filtra aprovação; o escritor reutiliza qualquer encontrada e só cria quando não encontra nenhuma. Portanto, “sem batida aprovada” ainda permite reuso sem criação. (`app/chamados/juizes.py:247`; `app/chamados/services/materializacao.py:1347`; `app/chamados/services/materializacao.py:1353`)

   Contraexemplo de código: sem irmã, batida não aprovada do mesmo tipo exatamente em `ts`. Ela não produz `ja_satisfeita` e não entra na vizinhança anterior/posterior, cujos limites são estritos; ainda assim, o escritor a reutiliza. Nenhuma guarda precisa ser afrouxada. Ocorrência em produção: **[nao verificado]**. (`app/chamados/services/materializacao.py:899`; `app/chamados/services/materializacao.py:113`; `app/chamados/services/materializacao.py:116`; `app/chamados/services/materializacao.py:1347`)

   Para fechar esse ponto, A4 precisa exigir ausência de qualquer batida retornada pela busca de reuso. (`app/chamados/juizes.py:239`; `app/chamados/services/materializacao.py:1353`)

4. **Exportada, dinheiro e `!`: não identifiquei ampliação além do bloqueio declarado.** A guarda está antes do carimbo e do recálculo na validação e antes das escritas do materializador. Alteração do `FechamentoMensal` exportado continua sob `!`; impacto monetário efetivo: **[nao verificado]**. (`app/chamados/services/validacao.py:652`; `app/chamados/services/validacao.py:655`; `app/chamados/services/validacao.py:666`; `app/chamados/services/materializacao.py:1829`; `CLAUDE.md:411`)

**VEREDITO: PROCEDE (item 3/A4: “sem batida aprovada” permite reuso sem criação e não garante `+1`; app/chamados/juizes.py:247; app/chamados/services/materializacao.py:1353).**

_6298 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
