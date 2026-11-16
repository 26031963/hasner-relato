# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 21:40** · publicado em **15/11/2026 21:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 21:40 · sessao cdf8362d726a · HE-REVOGADA-NA-PORTA · verificador · origem=Code

**0 procedente identificado no diff funcional.** A cura consulta a autoridade existente e mantém a escrita na porta; há observações sobre registro, semântica dos carimbos e duas afirmações incorretas da entrega sobre o cron (`app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:369`).

A identidade dos commits, o merge-tree limpo e os resultados de pista não foram conferidos nesta cópia sem histórico Git: **[nao verificado]**. [nao verificado]

1. **Efeito do merge do main**

   Considerando a lista de arquivos fornecida, não identifiquei ligação nova com as portas de HE. O template alterado trata prévia de selfie e resposta da batida, enviando para `/api/ponto/bater/`; as decisões de HE usam outras rotas (`app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:288`, `app/templates/ponto/bater_ponto_v2.html:360`; `app/ponto/urls.py:19`, `app/ponto/urls.py:21`, `app/ponto/urls.py:24`).

   Portanto, existe mudança de comportamento na tela de batida, mas nenhuma interferência identificada em `decidir_he`, nos lotes ou em `vigentes()`. A certificação integral do delta histórico do merge fica **[nao verificado]**; os caminhos locais de HE estão em `app/ponto/views.py:532`, `app/ponto/views.py:617`, `app/ponto/views.py:712` e `app/ponto/views.py:754`.

2. **Checklist sobre o diff inteiro**

   | Classe | Resultado da leitura |
   |---|---|
   | Segundo juiz | `_vigente` apenas consulta `DecisaoHE.objects.vigentes().filter(pk=...).exists()`; o predicado continua exclusivamente no QuerySet (`app/ponto/portas/he.py:148`; `app/ponto/models.py:1267`). |
   | Segundo escritor fora da porta | Os atos continuam convergindo para `_gravar`; a revogação continua em `_marcar_revogada`, ambos dentro de `he.py` (`app/ponto/portas/he.py:66`, `app/ponto/portas/he.py:84`, `app/ponto/portas/he.py:403`). |
   | Relógio fora da borda | O helper novo não consulta relógio. O carimbo de revogação continua no escritor existente (`app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:401`). |
   | Derivação paralela | Não há regra nova de vigência nem medição nova de minutos: a porta consulta o QuerySet e recebe os minutos do chamador (`app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:187`, `app/ponto/views.py:533`). |
   | RED sem mordida | Os casos verificam reativação, origem, minutos, mesma quantidade de linhas, trilha e idempotência; também preservam a barreira contra recusa vigente do admin (`app/ponto/tests/test_he_revogada_na_porta.py:57`, `app/ponto/tests/test_he_revogada_na_porta.py:60`, `app/ponto/tests/test_he_revogada_na_porta.py:64`, `app/ponto/tests/test_he_revogada_na_porta.py:78`, `app/ponto/tests/test_he_revogada_na_porta.py:106`). |
   | Ordem de trava | Continua sendo transação → lock da linha crua → consulta de vigência → escrita. No lote, a chamada interna torna a consultar a mesma decisão; o helper não adquire outra trava (`app/ponto/portas/he.py:158`, `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:576`, `app/ponto/portas/he.py:583`, `app/ponto/portas/he.py:595`). |
   | Pouso antes do pré-requisito | A regra exige suite inteira verde antes do opus final. O anexo informa suite pendente; sua conclusão posterior e o opus final são **[nao verificado]** (`CLAUDE.md:377`). | [nao verificado]

   O RED novo **não prova dinheiro persistido**: ele substitui `recalcular_por_evento` por mock e verifica sua chamada. Também não exercita prévia/confirmar com a linha revogada (`app/ponto/tests/test_he_revogada_na_porta.py:29`, `app/ponto/tests/test_he_revogada_na_porta.py:55`, `app/ponto/tests/test_he_revogada_na_porta.py:66`, `app/ponto/tests/test_he_revogada_na_porta.py:94`).

   O serviço, entretanto, mantém um único `_ato` para prévia e confirmação; a prévia desfaz a transação, e a confirmação rejeita divergência do DIFF (`app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:322`, `app/ponto/services/autorizacao_he_periodo.py:344`, `app/ponto/services/autorizacao_he_periodo.py:346`).

3. **Quem escreve e quem lê, contra JUIZES.md**

   **Não há juiz de vigência de `DecisaoHE` registrado em `core/juizes.py`.** A autoridade executável existe em `DecisaoHEQuerySet.vigentes()`, mas não aparece nas declarações do registro; portanto, chamá-la de “juiz registrado” seria incorreto (`app/ponto/models.py:1265`; `app/core/juizes.py:186`, `app/core/juizes.py:731`, `app/core/juizes.py:1048`, `app/core/juizes.py:1196`, `app/core/juizes.py:1314`).

   Isso é uma lacuna do registro, não duplicação introduzida por `_vigente`. O próprio lookup aponta `core/juizes.py` como registro vivo e determina corrigir divergências (`app/docs/JUIZES.md:10`, `app/docs/JUIZES.md:14`; `app/ponto/portas/he.py:148`).

   | Campo/valor | Quem escreve | Quem lê |
   |---|---|---|
   | `pk`, `colaborador`, `data` | `_gravar` cria a linha; a atualização conserva sua identidade (`app/ponto/portas/he.py:66`, `app/ponto/portas/he.py:84`). | Locks das quatro portas e consulta por PK (`app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:346`, `app/ponto/portas/he.py:473`, `app/ponto/portas/he.py:583`). |
   | `estado` | `_gravar`, na criação e troca (`app/ponto/portas/he.py:67`, `app/ponto/portas/he.py:75`). | Guardas da porta, motor, gestão e retrato (`app/ponto/portas/he.py:197`, `app/ponto/portas/he.py:488`, `app/ponto/portas/he.py:590`; `app/ponto/motor_calculo_v2.py:1417`; `app/ponto/services/gestao_he.py:86`; `app/ponto/services/he_pendente_lavrado.py:145`). |
   | `origem` | `_gravar`; admin e sistema chegam com origem explícita (`app/ponto/portas/he.py:76`, `app/ponto/portas/he.py:208`, `app/ponto/portas/he.py:371`). | Guarda de revogação, mensagem do lote e trilha (`app/ponto/portas/he.py:478`, `app/ponto/portas/he.py:594`, `app/ponto/portas/he.py:99`). |
   | `minutos_na_decisao` | `_gravar`, usando os minutos recebidos (`app/ponto/portas/he.py:67`, `app/ponto/portas/he.py:75`). | Retornos idempotentes, revogação, trilha e soma da gestão. É foto da decisão, não pendência viva (`app/ponto/portas/he.py:203`, `app/ponto/portas/he.py:497`, `app/ponto/portas/he.py:398`; `app/ponto/services/gestao_he.py:108`; `app/ponto/models.py:1278`). |
   | `motivo`, `decidida_por` | `_gravar`; a exclusão do usuário também pode tornar `decidida_por` nulo por `SET_NULL` (`app/ponto/portas/he.py:68`, `app/ponto/portas/he.py:76`; `app/ponto/models.py:1311`). | `motivo` é lido na trilha; nenhum leitor operacional de `decidida_por` foi identificado. Acessos dinâmicos: **[nao verificado]** (`app/ponto/portas/he.py:61`, `app/ponto/portas/he.py:398`). |
   | `revogada_em` | `_marcar_revogada` preenche; `_gravar` limpa (`app/ponto/portas/he.py:401`, `app/ponto/portas/he.py:83`). | Juiz de vigência e trilha; as quatro políticas passam pelo helper (`app/ponto/models.py:1267`; `app/ponto/portas/he.py:63`, `app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:347`, `app/ponto/portas/he.py:481`, `app/ponto/portas/he.py:589`). |
   | `revogada_motivo`, `minutos_na_revogacao` | Marcador preenche; gravador limpa (`app/ponto/portas/he.py:402`, `app/ponto/portas/he.py:83`). | Trilha; não participam do predicado de vigência (`app/ponto/portas/he.py:412`, `app/ponto/portas/he.py:413`; `app/ponto/models.py:1267`). |
   | `vigente` | Não é campo: é pertencimento ao QuerySet, determinado pela ausência de marca de revogação (`app/ponto/models.py:1267`, `app/ponto/models.py:1348`). | Helper, motor, gestão e comandos (`app/ponto/portas/he.py:148`; `app/ponto/motor_calculo_v2.py:1417`; `app/ponto/services/gestao_he.py:87`; `app/ponto/management/commands/lavrar_he_pendente.py:48`; `app/ponto/management/commands/recusar_ponta_pequena.py:120`). |
   | `decidido_em` / `decidida_em`, `atualizada_em` | O nome real é `decidida_em`, automático **na criação**; `atualizada_em` é renovado nas alterações (`app/ponto/models.py:1316`, `app/ponto/models.py:1317`; `app/ponto/portas/he.py:84`, `app/ponto/portas/he.py:403`). | Nenhum leitor operacional desses carimbos de `DecisaoHE` foi identificado; acesso dinâmico **[nao verificado]**. |
   | `criada`, `trocada`, `no_op` e contadores | Produzidos pela porta e agregados pelos lotes (`app/ponto/portas/he.py:69`, `app/ponto/portas/he.py:87`, `app/ponto/portas/he.py:203`, `app/ponto/portas/he.py:231`, `app/ponto/portas/he.py:270`, `app/ponto/portas/he.py:600`). | Mensagens das views e serviço de período (`app/ponto/views.py:539`, `app/ponto/views.py:618`, `app/ponto/views.py:758`; `app/ponto/services/autorizacao_he_periodo.py:299`, `app/ponto/services/autorizacao_he_periodo.py:347`). |

   A reativação chamada de “decisão nova” **preserva `decidida_em` original**: o update não renova esse campo. Ele registra a criação da linha; o carimbo da alteração fica em `atualizada_em`. Não foi demonstrado leitor que confunda esses instantes (`app/ponto/models.py:1316`, `app/ponto/models.py:1317`; `app/ponto/portas/he.py:84`).

4. **Pouso em competência ABERTA move dinheiro sozinho?** [nao verificado]

   **Não pelo caminho desta obra.** A escrita financeira exige uma chamada que alcance `decidir_he`; depois de gravar a decisão, a porta agenda o recálculo no commit (`app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:230`; `app/ponto/services/fechamento.py:1824`).

   O caminho é: decisão individual ou lote administrativo → `decidir_he` → `recalcular_por_evento` → competência do fato → recálculo do colaborador naquela competência inteira → persistência de `FechamentoMensal` e relavratura de `DiaPago` (`app/ponto/portas/he.py:265`, `app/ponto/portas/he.py:595`; `app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1201`; `app/ponto/services/dia_pago.py:359`).

   Mesmo um **“Não”** agora efetivo pode disparar essa relavratura inteira; diferença financeira total igual a zero não está provada pelos testes mockados nem pelos dados fornecidos (`app/ponto/portas/he.py:230`; `app/ponto/tests/test_he_revogada_na_porta.py:55`). **[nao verificado quanto aos valores persistidos]**

   Os crons de HE não chegam a esse recálculo: a re-recusa apenas grava e retorna; a revogação apenas marca a linha. Outros gatilhos existentes, como batida, podem recalcular posteriormente, mas não são disparados pelo pouso desta alteração (`app/ponto/portas/he.py:369`, `app/ponto/portas/he.py:372`, `app/ponto/portas/he.py:500`; `app/ponto/registro_batida.py:179`).

   Um apply financeiro posterior continua sujeito aos três requisitos documentados — reversão anterior, integridade da exportada e prova de frota posterior. Cumprimento nesta entrega: **[nao verificado]** (`CLAUDE.md:406`).

Lista final de achados:

- **Observação — narrativa do cron incorreta:** a re-recusa já aceitava linha revogada no diff anterior fornecido; existe inclusive teste específico desse comportamento. Ela não chama recálculo financeiro (`app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:222`, `app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:237`; `app/ponto/portas/he.py:369`).
- **Observação — registro incompleto:** `vigentes()` é a autoridade executável consultada, mas não está registrada em `core/juizes.py`; `_vigente` não cria outra regra (`app/ponto/models.py:1267`; `app/ponto/portas/he.py:148`; `app/core/juizes.py:186`; `app/docs/JUIZES.md:12`).
- **Observação — carimbo preservado:** reativar a linha não renova `decidida_em`; não foi demonstrado efeito incorreto em leitor (`app/ponto/models.py:1316`; `app/ponto/portas/he.py:84`). **[nao verificado quanto a leitores dinâmicos]**
- **Observação — alcance dos selos:** o selo O273 exclui `he.py`; os novos testes cobrem comportamento, mas não os valores financeiros persistidos (`app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:473`; `app/ponto/tests/test_he_revogada_na_porta.py:55`).
- **Observação — conclusão operacional pendente:** suite inteira verde, opus final e condições reais de produção não foram certificados nesta leitura **[nao verificado]**; a exigência está em `CLAUDE.md:377`. [nao verificado]

**0 procedente funcional identificado; isso não certifica os pré-requisitos do pouso** (`app/ponto/portas/he.py:148`, `app/ponto/portas/he.py:196`, `app/ponto/portas/he.py:589`; `CLAUDE.md:377`).

## 15/11 21:40 · sessao 749cb25387fe · - · - · origem=Ronald (tmux)

**PROCEDENTE.** A recusa deixa a linha HTMX mostrando um posto não gravado: atribuição ocorre antes da porta (`app/colaboradores/views.py:327`), a guarda recusa antes do save (`app/colaboradores/services/colaborador.py:148`), e o `except` usa o objeto alterado em `posto_atual` (`app/colaboradores/views.py:342`), renderizado no nome do posto (`app/templates/colaboradores/partials/vinculo_row.html:20`).

O formulário substitui a linha por `outerHTML` (`app/templates/colaboradores/partials/painel_vinculo.html:114`). O listener posterior recarrega somente o painel (`app/templates/colaboradores/vincular.html:165`), cuja atualização altera `#vinc-painel`, sem corrigir a linha (`app/templates/colaboradores/vincular.html:143`).

A nova recusa da guarda expõe o problema; c5 não cobre essa tela (`app/colaboradores/tests/test_lote3_c5_recusa_e_trava.py:8`). Reprodução em navegador: [nao verificado].

## 15/11 21:38 · sessao d92c19e18381 · RETOMA-HE-LOTE3 · verificador · origem=Code

**1. Premissas — PROCEDE: a justificativa factual precisa ser corrigida.** (Anexos `pista_estado.txt`, `he_suite_veredito.txt` e `lote3_entrega.md`, seção c5.)

- **Confere:** as obras e versões estão documentadas: HE `019e5157a`; LOTE 3 c5 `874fb76d1`. O LOTE 3 declarou antecipadamente o `!` de VÍNCULO. (Anexos `he_revogada_entrega.md`, “Cópia e sha”; `lote3_entrega.md`, abertura e seção c5.)
- **Confere:** QUEIMA proibia job novo enquanto a pista não andasse; seu encerramento foi explícito após a redefinição às 19:33. (Anexos `queima_ate_a_redefinicao.txt`, item 6; `pos_redefinicao_1935.txt`, item 2.)
- **Não confere como fato comprovado:** “os pedidos delas foram cancelados 13:13, rc 130”. O extrato mostra cancelamentos dos hashes `6e2fc913`, `042adf7f` e `1ec22451`, sem identificar HE/LOTE 3 nem informar a hora do cancelamento. A HE tem, adicionalmente, suite inteira verde informada em `019e5157a`. (Anexos `pista_estado.txt`, `he_suite_veredito.txt`; fato medido fornecido no enunciado.)
- **Não verificável daqui:** a autorização **original** específica para pousar HE em competência 11 sem LU2-04. Essa conduta está registrada no RELATO, mas o texto original anexado, PRODUCAO-MAXIMA, não contém esse corte específico. (`app/docs/RELATO.md:171`; anexo `producao_maxima.txt`, itens 2 e 5.)

**2. Colisão com avais vigentes — NÃO PROCEDE quanto à fila, aos tetos e à sequência de verificação demonstrados.** (Anexos `retoma_he_revogada_e_lote3.txt`, `relato_topo.txt` e `triagem_da_trava_1717.txt`, item A.)

**Confere:** RETOMA preserva R3 c17/F1/BUG-ENC na frente e não abre construtor; isso é compatível com o topo fornecido, inclusive a primeira vaga da c17 após COMPLEMENTO-1. O teto de dois construtores continua aplicável a qualquer eventual volta de construção. (Anexo `relato_topo.txt`, linhas iniciadas por “RESPONDIDA por COMPLEMENTO-1” e “DECIDIDO PELA LEI”; `CLAUDE.md:288`.)

**Confere:** pre-verificador após mudança da árvore, suite inteira antes do opus e preferência da espinha correspondem à TRIAGEM A. A restrição adicional de só abrir construção com achado procedente não dispensa a parada por classe procedente repetida em três voltas. (Anexos `triagem_da_trava_1717.txt`, A(1)/A(4); `pos_redefinicao_1935.txt`, 4.1; `CLAUDE.md:375`.) [nao verificado]

**Não verificável daqui:** cotejo integral de CONSTRUIR-R3-C17-E-F1-NOVA e COMPLEMENTO-1: seus textos completos não foram fornecidos; o topo permite conferir apenas os efeitos ali registrados. (Anexo `relato_topo.txt`.)

**3. Fechamento literal — PROCEDE: o aval assume RED evidenciado que não foi demonstrado para o LOTE 3 c5.** (Anexos `retoma_he_revogada_e_lote3.txt`, linha LEI-AKITA; `lote3_entrega.md`, seção c5; `CLAUDE.md:28`.)

**Não confere:** “RED=os já evidenciados nas cópias” não fecha para c5. A entrega descreve RED **esperado** em `c4a30b872`, e o dado fornecido não apresenta execução vermelha desse RED. Suite verde da cura, quando existir, não substitui essa evidência. (Anexo `lote3_entrega.md`, seção c5, “Testes e mordida”; `CLAUDE.md:28` e `CLAUDE.md:193`.)

**Confere nos demais exemplos:**

- “Teto 3 da pista” tem respaldo no registro PISTA-VOLTOU; é distinto do teto de construtores. (`app/docs/RELATO.md:30`.)
- “Suite inteira pre-opus na pista” foi autorizada pela promoção da PISTA-2. (Anexo `producao_maxima.txt`, item 3.) [nao verificado]
- “Pre-verificador no hash final” após merge é executável e necessário. O verde antigo da HE não certifica automaticamente `5937d715b`; o aval já manda repetir a sequência na árvore final. (Anexo `triagem_da_trava_1717.txt`, A(1); `CLAUDE.md:320`; fato medido fornecido no enunciado.)
- “Construção nova só com procedente” é uma condição de entrada possível; não demonstra que alguma volta já esteja autorizada. (Anexo `retoma_he_revogada_e_lote3.txt`, “PROIBIDO”.)

**4. Exportada, dinheiro e `!` — PROCEDE quanto ao alcance da HE ainda não fechado; toque efetivo em produção é não verificável daqui.** (`wt-he-revogada-095943/app/ponto/portas/he.py:195,230`; `app/ponto/services/fechamento.py:539,560,1809`.)

**Confere:** dinheiro em competência aberta e mudança da porta de vínculo estão declarados; o LOTE 3 mantém o pouso condicionado ao `!`. Portanto, esses dois efeitos, por si, não são extrapolação oculta. (Anexos `retoma_he_revogada_e_lote3.txt`, itens 1/2; `he_revogada_entrega.md`, “Pergunta de pouso antecipada”; `lote3_entrega.md`, abertura.) [nao verificado]

**Não fecha:** a proibição “tocar exportada” não está garantida pelo caminho da HE. A porta passa a efetivar decisões antes ignoradas e chama o recalculador pela data recebida; esse recalculador alcança exportadas, com guarda específica para 09/2026. A própria entrega deixa LU2-04 aberta sobre esse efeito. (`wt-he-revogada-095943/app/ponto/portas/he.py:195,206,230`; `app/ponto/services/fechamento.py:36,539,560,1809`; anexo `he_revogada_entrega.md`, “Pergunta de pouso antecipada”.)

**Não verificável daqui:** existência de afetados em exportada, valores efetivamente movidos e cumprimento de reversão prévia, hash de exportadas intactas e prova de frota. Os anexos fornecidos não fecham essas provas; elas continuam exigidas para o caminho de dinheiro em aberta. (Anexos fornecidos; `CLAUDE.md:406`.)

**VEREDITO: 1 PROCEDE; 2 NÃO PROCEDE no alcance verificável; 3 PROCEDE; 4 PROCEDE.** Defeitos: premissa de cancelamento sem sustentação, RED c5 presumido e alcance de exportadas da HE sem fechamento demonstrado. (Fontes citadas nos respectivos itens.)

## 15/11 21:38 · sessao 52e91cfce4a5 · - · - · origem=Ronald (tmux)

**Conclusão: nenhum PROCEDENTE novo no caminho de dinheiro; há erro no relato do anexo sobre o cron.** A re-recusa automática não chama recálculo, e esse ramo já aceitava linha revogada no diff anterior fornecido; a troca por `_vigente` preserva o comportamento enquanto `vigentes()` for `revogada_em IS NULL` (`app/ponto/portas/he.py:346`, `app/ponto/portas/he.py:369`, anexo he_obra.diff [nao verificado quanto ao histórico Git]).

- **Pousar não move dinheiro sozinho.** O módulo declara funções; a escrita decorrente da obra exige chamar `decidir_he` ou o lote que o chama, e o recálculo só é agendado depois de `_gravar` (`app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:229`). O boot de ponto apenas importa signals (`app/ponto/apps.py:7`). Estado real de deploy/cron/banco: [nao verificado].
- **Clique de um dia:** POST → porta (`app/ponto/views.py:521`, `app/ponto/views.py:532`); transação e guardas de estado, permissão, motivo e minutos → lock → juiz → escrita → recálculo (`app/ponto/portas/he.py:158`, `app/ponto/portas/he.py:175`, `app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:206`, `app/ponto/portas/he.py:230`).
- **Recusa administrativa em lote:** view → `recusar_em_lote` → `decidir_he` por dia, portanto chega ao mesmo recálculo (`app/ponto/views.py:617`, `app/ponto/portas/he.py:265`).
- **Autorização por período:** views chamam prévia/confirmar (`app/ponto/views.py:712`, `app/ponto/views.py:754`); ambos usam `_ato`, que mede antes, chama `autorizar_em_lote`, mede depois (`app/ponto/services/autorizacao_he_periodo.py:271`, `app/ponto/services/autorizacao_he_periodo.py:277`, `app/ponto/services/autorizacao_he_periodo.py:280`). Prévia força rollback; confirmação compara o diff dentro da transação e aborta se divergir; callbacks `on_commit` só sobrevivem à confirmação aceita (`app/ponto/services/autorizacao_he_periodo.py:321`, `app/ponto/services/autorizacao_he_periodo.py:343`, `app/ponto/services/autorizacao_he_periodo.py:346`, `app/ponto/services/fechamento.py:1824`).
- **Dinheiro efetivamente alcançado:** callback calcula competência do fato e relavra o colaborador naquela competência inteira (`app/ponto/services/fechamento.py:1809`). `FechamentoMensal` é obtido/criado e, se aberto, salvo; outros estados recebem update dos campos calculados (`app/ponto/services/fechamento.py:898`, `app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1160`). A mesma execução chama lavra dos dias; `DiaPago` sofre delete e bulk_create por colaborador/competência/versão (`app/ponto/services/fechamento.py:1200`, `app/ponto/services/fechamento.py:1277`, `app/ponto/services/dia_pago.py:359`).
- **Exportada:** o código carimba a escrita, sem bloqueio geral por exportação (`app/ponto/services/fechamento.py:539`). A guarda específica exclui do universo os colaboradores das empresas exportadas na competência guardada (`app/ponto/services/fechamento.py:560`); a entrada é setembro/2026 (`app/ponto/services/fechamento.py:36`). Confirmação de autorização em exportada também escreve pauta DP (`app/ponto/services/autorizacao_he_periodo.py:299`). Isso descreve o código; decidir precedência normativa de L-092/L-113 fora do que a cópia documenta seria [nao verificado].
- **Cron da ponta pequena:** seleciona decisões vigentes, exige foto do dia, célula julgada e marcos vencidos, depois chama porta (`app/ponto/management/commands/recusar_ponta_pequena.py:105`, `app/ponto/management/commands/recusar_ponta_pequena.py:120`, `app/ponto/management/commands/recusar_ponta_pequena.py:145`, `app/ponto/management/commands/recusar_ponta_pequena.py:150`, `app/ponto/management/commands/recusar_ponta_pequena.py:170`). A porta só grava decisão e retorna, sem `recalcular_por_evento` (`app/ponto/portas/he.py:369`). O registro do cron confirma explicitamente essa separação (`app/config/crons.py:1582`). Portanto o trecho do anexo “clique (ou cron acima) ... agora chama recalcular_por_evento” está incorreto diante da cópia (`app/ponto/portas/he.py:230`, `app/ponto/portas/he.py:369`).
- **Cron lavrador:** pede revogação pela porta e lavra snapshot; a revogação só marca a linha, sem recálculo monetário (`app/ponto/management/commands/lavrar_he_pendente.py:59`, `app/ponto/management/commands/lavrar_he_pendente.py:71`, `app/ponto/portas/he.py:500`).
- **Outros gatilhos preexistentes:** uma batida chama recálculo por evento, e alteração de célula também pode fazê-lo, sem clique HE (`app/ponto/registro_batida.py:162`, `app/ponto/registro_batida.py:179`, `app/ponto/portas/celula.py:486`). Não decorrem da nova consulta `_vigente` (`app/ponto/portas/he.py:148`).
- **Locks/relógio:** a porta mantém lock da mesma linha antes de perguntar vigência; o lote já abre atomic e chama a porta, que torna a travar a mesma decisão (`app/ponto/portas/he.py:195`, `app/ponto/portas/he.py:576`, `app/ponto/portas/he.py:583`, `app/ponto/portas/he.py:595`). O helper novo só faz `exists`, sem relógio (`app/ponto/portas/he.py:148`); o relógio da revogação continua no escritor `_marcar_revogada` (`app/ponto/portas/he.py:401`).
- **Limite do RED:** os casos novos mockam o recálculo e provam chamada/idempotência, estado e trilha; não provam os valores de dinheiro persistidos (`app/ponto/tests/test_he_revogada_na_porta.py:55`, `app/ponto/tests/test_he_revogada_na_porta.py:66`, `app/ponto/tests/test_he_revogada_na_porta.py:73`, `app/ponto/tests/test_he_revogada_na_porta.py:97`). Execução e resultado real da pista: [nao verificado].

## 15/11 21:38 · sessao 749cb25387fe · - · - · origem=Ronald (tmux)

- **PROCEDENTE, residual declarado:** `lancar_ausencia` pode gravar a Ausencia e depois recusar a mudança de situação: lookup elegível em `app/ponto/views.py:2441`, INSERT com atomic próprio em `app/ponto/services/ausencia.py:478`, retorno em `:512`, escrita posterior de situação em `app/ponto/views.py:2472`; o `except` apenas devolve erro em `:2495`. O teste cobre desligado antes do POST, sem simular essa janela (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:192`).

- **Observação — universo:** férias continua fora da lista, pois somente ativo/afastado entra (`app/ponto/views.py:2404`). Frequência e efeito em produção: [nao verificado].

- **Observação — mordida c5:** os testes verificam recusa com mensagem e banco preservado nas três telas (`app/colaboradores/tests/test_lote3_c5_recusa_e_trava.py:65`, `:83`, `:103`); os cinco testes de porta verificam FOR UPDATE antes de SELECT da guarda e UPDATE, savepoint aberto e registro na porta global (`:130`, `:147`, `:152`, `:158`, `:164`, `:171`). São testes de sequência SQL com uma conexão; concorrência real e ordem com vários colaboradores não são exercitadas (`:125`, `:147`). Resultado RED e suite: [nao verificado].

- **Observação — limites v2 curados:** selfie agora exige `foto` em `update_fields` e relê o nome persistido (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:267`); porta de reativação testa origem não desligada e destino diferente de ativo (`:122`). O modelo continua incluindo os campos de conclusão do onboarding (`app/colaboradores/models.py:420`); o signal continua ignorando `update_fields` (`app/chamados/signals.py:271`).

- **Observação — auditoria:** o tratamento de `request.user=None` está corrigido (`app/core/auditoria.py:24`). O teste exige linha, usuário NULL e situação desligado (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:228`), mas não exige `LogAuditoria.ator=sistema:<comando>`; esse valor depende de `sys.argv` (`app/core/observ.py:22`, `:117`, `:123`). Valor real em produção: [nao verificado].

- **Observação — selo de diagrama:** novos leitores/imports de juiz ficam fora dos artefatos por desenho do gerador (`app/core/management/commands/gerar_diagrama.py:861`, `:936`). Portanto o import novo, sozinho, não demonstra falha nesse selo; execução do check: [nao verificado].

- **Observação — descarte financeiro:** ausência de mudança direta de fórmula não prova resultado monetário invariável: o diff muda a admissão de Ausencia (`app/ponto/views.py:2441`) e bloqueia transições de situação (`app/colaboradores/services/colaborador.py:63`), enquanto exportação lê Ausencia (`app/folha/export.py:83`) e desligado (`:1206`). Efeito financeiro real ou DIFF_FOLHA=0: [nao verificado].

## 15/11 21:38 · sessao 8d6abc92fbaa · - · - · origem=Ronald (tmux)

- **Observação — juiz único preservado na cura.** O registro vivo aponta `colaboradores/services/desligamento.py::esta_desligado` em `app/core/juizes.py:268-271`; o predicado é exclusivamente `situacao == 'desligado'` em `app/colaboradores/services/desligamento.py:23-32`. A guarda nova o consulta para destino e linha gravada em `app/colaboradores/services/colaborador.py:62-66`; a reativação também o consulta em `app/colaboradores/services/colaborador.py:181-185`.

- **Observação — comparação crua remanescente, preexistente.** A tela reativar ainda responde à pergunta com `colaborador.situacao != 'desligado'` em `app/colaboradores/views.py:1430`; `situacao_exibida` também compara o estado diretamente em `app/colaboradores/services/situacao_exibida.py:56-58`. Pelo diff fornecido, essas comparações não nasceram nesta obra [nao verificado quanto ao histórico Git, ausente nesta cópia]. O contrato explícito do juiz pede “nunca situacao cru” em `app/colaboradores/services/desligamento.py:26-30`.

- **Observação — JUIZES.md incompleto, sem autoridade concorrente.** O documento declara ser lookup e manda consultar o registro vivo em `app/docs/JUIZES.md:7-15`; a seção de ausência/férias não relaciona desligamento em `app/docs/JUIZES.md:55-58`. O vivo registra a autoridade em `app/core/juizes.py:271`.

- **Observação — escritores únicos registrados.** Colaborador pertence à porta `colaboradores/services/colaborador.py` e HistoricoVinculo a `colaboradores/services/historico_vinculo.py` em `app/core/portas.py:450` e `app/core/portas.py:461`. As duas criações efetivas do histórico estão nesse módulo, em `app/colaboradores/services/historico_vinculo.py:13-17` e `app/colaboradores/services/historico_vinculo.py:25-27`; desligar/reativar/transferir chamam a porta em `app/colaboradores/services/desligamento.py:113-117`, `app/colaboradores/services/desligamento.py:139-146`, `app/colaboradores/services/desligamento.py:252-259` e `app/colaboradores/views.py:1404-1411`.

- **Observação — relógio fora da borda preexistente.** O juiz de desligamento lê `date.today()` duas vezes, em `app/colaboradores/services/desligamento.py:40` e `app/colaboradores/services/desligamento.py:60`; o executor lê `_tz.now()` em `app/colaboradores/services/desligamento.py:155`; reativar lê `_tz.localdate()` em `app/colaboradores/services/desligamento.py:255`. O ADR determina receber hoje/agora e ler uma vez na borda em `app/docs/PLANO-PADROES.md:99-103`. A transferência lê data na própria view em `app/colaboradores/views.py:1407`. Pelo diff fornecido, a obra não introduz essas leituras [nao verificado quanto ao histórico Git].

- **Observação — semântica de `situacao`.** Os quatro valores são ativo/afastado/ferias/desligado em `app/colaboradores/models.py:295-300`; o campo é `app/colaboradores/models.py:323`. Desligado é estado efetivo, independente de data futura e afastamento, conforme `app/colaboradores/services/desligamento.py:26-32`. A escrita afastado e a leitura ativo continuam autorizadas pelo corte registrado em `app/core/juizes.py:455-473`; a nova guarda não cria derivador de afastamento em `app/colaboradores/services/colaborador.py:62-66`.

- **Observação — o campo existente é `data_demissao`.** Está declarado em `app/colaboradores/models.py:325`; `data_desligamento` não foi encontrado no censo da cópia [nao verificado fora da cópia]. `data_demissao` guarda intenção futura no agendamento em `app/colaboradores/services/desligamento.py:107-111`, data efetiva em `app/colaboradores/services/desligamento.py:129-133`, e é limpa na reativação em `app/colaboradores/services/desligamento.py:242-247`. O cron reconhece intenção vencida por data preenchida/vencida e situação diferente de desligado em `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37-40`; a folha também usa a data para rescisão/recorte em `app/folha/export.py:60`, `app/folha/export.py:1068` e `app/folha/export.py:1087-1088`.

- **Observação — `HistoricoVinculo.motivo='desligamento'` tem dois eventos.** Agendamento e efetivação usam esse mesmo motivo em `app/colaboradores/services/desligamento.py:115` e `app/colaboradores/services/desligamento.py:143`; distinguem-se pelo `obs` prefixado AGENDADO em `app/colaboradores/services/desligamento.py:116`. Portanto duas linhas brutas não provam cascata repetida: podem ser esses dois eventos legítimos, conforme os dois caminhos citados. O anexo corretamente pede exclusão das linhas AGENDADO; nenhum resultado dessa sonda foi fornecido [nao verificado].

- **Observação — `reativacao` fora de choices, preexistente.** O executor escreve esse motivo em `app/colaboradores/services/desligamento.py:256`, mas `MOTIVO_CHOICES` só enumera admissao/transferencia/desligamento/mudanca/correcao em `app/colaboradores/models.py:554-560`; a criação direta não chama validação de choices em `app/colaboradores/services/historico_vinculo.py:25-27`. A UI lê `get_motivo_display` em `app/templates/colaboradores/editar.html:187`. A troca de porta da obra não introduziu esse valor, segundo o diff fornecido [nao verificado quanto ao histórico Git].

- **Observação — leitores do histórico identificados.** A view entrega seis linhas e contagem em `app/colaboradores/views.py:306-307`; o painel lê data/obs e demais atributos em `app/templates/colaboradores/partials/painel_vinculo.html:58-65`; a edição mostra data/motivo/posto/obs em `app/templates/colaboradores/editar.html:179-187`; inteligência carrega a linha do tempo em `app/inteligencia/resumo.py:51-52`.

**0 procedente novo de semântica/juiz/escritor demonstrado por este recorte; observações e dívidas preexistentes acima, com evidência estática nos locais citados.**

## 15/11 21:38 · sessao 0dfa17437ac4 · - · - · origem=Ronald (tmux)

Juiz e registro:

- A autoridade executável é `DecisaoHEQuerySet.vigentes`, cujo único predicado é `revogada_em__isnull=True`; `_vigente` consulta essa autoridade por PK, sem repetir o predicado (`app/ponto/models.py:1265-1267`; `app/ponto/portas/he.py:148`).
- **Não há juiz de vigência de DecisaoHE registrado em `core/juizes.py`**: as declarações de `JUIZES` estão em `app/core/juizes.py:186-349`, `:731-744`, `:1048`, `:1196-1212` e `:1314-1315`, e nenhuma contém essa autoridade. `JUIZES.md` identifica esse arquivo como registro vivo e manda corrigir divergências (`app/docs/JUIZES.md:10-14`). Lacuna preexistente, porque o diff fornecido não altera esses arquivos; não constitui segundo juiz criado pela obra [nao verificado quanto ao histórico sem Git].

Censo de campos:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `colaborador`, `data` | `_gravar` na criação (`app/ponto/portas/he.py:66-68`) | Portas por colaborador/dia (`:195`, `:346`, `:473`, `:583-584`); motor (`app/ponto/motor_calculo_v2.py:1417-1418`); gestão (`app/ponto/services/gestao_he.py:86-89`); comandos (`app/ponto/management/commands/lavrar_he_pendente.py:48-49`; `app/ponto/management/commands/recusar_ponta_pequena.py:120-121`). |
| `estado` | `_gravar` (`app/ponto/portas/he.py:66-68`, `:75`, `:84`) | Guardas/idempotência (`:197`, `:358`, `:488`, `:590`), motor (`app/ponto/motor_calculo_v2.py:1417`), gestão (`app/ponto/services/gestao_he.py:86`, `:107-110`), retrato (`app/ponto/services/he_pendente_lavrado.py:145-149`). |
| `origem` | `_gravar`, inclusive troca (`app/ponto/portas/he.py:68`, `:76`, `:84`) | Guarda de revogação (`:478`), erro do lote (`:594`), trilhas (`:62`, `:99`, `:399`, `:411`). O campo representa o autor do estado atual, não o primeiro (`app/ponto/models.py:1294-1296`). |
| `minutos_na_decisao` | `_gravar` (`app/ponto/portas/he.py:67`, `:75`, `:84`) | Retornos idempotentes (`:203`, `:358`), revogação (`:480`, `:487`, `:493`, `:496-497`), gestão por soma (`app/ponto/services/gestao_he.py:108`), trilhas (`app/ponto/portas/he.py:61`, `:398`, `:411`). É foto, não minutos vivos (`app/ponto/models.py:1278-1281`). |
| `motivo`, `decidida_por` | `_gravar` (`app/ponto/portas/he.py:68`, `:76`, `:84`) | `motivo` na trilha (`:61`, `:98`, `:398`); preservação referida no ato de período (`app/ponto/services/autorizacao_he_periodo.py:198-199`). Não encontrei leitor operacional de `decidida_por` no censo de produção [nao verificado quanto a acesso dinâmico externo]. |
| `revogada_em` | `_marcar_revogada` preenche (`app/ponto/portas/he.py:401-403`); `_gravar` limpa (`:83-85`) | Juiz (`app/ponto/models.py:1267`) e trilha (`app/ponto/portas/he.py:63`, `:412`). As quatro políticas consultam `_vigente` (`:196`, `:347`, `:481`, `:589`). |
| `revogada_motivo`, `minutos_na_revogacao` | Marcador preenche (`app/ponto/portas/he.py:402-403`); gravador limpa (`:83-85`) | Trilha (`:412-413`); texto inventariado para raspagem (`bin/sombra_regras_pessoais.py:118-125`). Não são predicados de vigência (`app/ponto/models.py:1267`). |
| `vigente` | Não é campo de DecisaoHE; é pertencimento ao QuerySet (`app/ponto/models.py:1265-1267`, `:1348`). | Motor, gestão e comandos nos endereços acima; helper `_vigente` (`app/ponto/portas/he.py:148`). |
| `decidido_em` | Esse nome não existe em DecisaoHE: o campo real é **`decidida_em`**, automático na criação (`app/ponto/models.py:1316`). | Nenhum leitor de produção de `DecisaoHE.decidida_em` encontrado por busca; acesso dinâmico [nao verificado]. |
| `decidida_em`, `atualizada_em` | ORM: `auto_now_add` e `auto_now` (`app/ponto/models.py:1316-1317`); só `atualizada_em` entra nos updates (`app/ponto/portas/he.py:84-86`, `:403`). | Nenhum leitor operacional encontrado para esses carimbos [nao verificado quanto a acesso dinâmico]. |

Observações relevantes:

- A “decisão nova” que revive a linha mantém `decidida_em` original: o campo é `auto_now_add`, o update não o inclui; o instante da alteração pertence a `atualizada_em` e à trilha (`app/ponto/models.py:1316-1317`; `app/ponto/portas/he.py:84-86`, `:94-99`). Isso já ocorria em trocas de estado e não aparece como regressão demonstrada do diff [nao verificado quanto ao histórico sem Git].
- **O anexo erra sobre mudança no cron:** no diff base, `recusar_ponta_pequena` já aceitava linha revogada porque barrava somente `_ja.revogada_em is None`; trocar por `_vigente` preserva exatamente esse resultado sob o juiz atual (`app/ponto/models.py:1267`; `app/ponto/portas/he.py:346-347`; base no diff fornecido [nao verificado quanto ao histórico]).
- **Recusa do sistema não chama recálculo**, inclusive quando revive linha: grava e retorna (`app/ponto/portas/he.py:369-372`). O recálculo está em `decidir_he` (`:229-230`); o teste anterior explicitamente proíbe recálculo da recusa automática (`app/ponto/tests/test_o214_ponta_pequena.py:425-426`).
- O selo que exige `vigentes()` **exclui integralmente he.py**, portanto não detectaria uma regressão de suas guardas (`app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:460-478`). Os testes novos precisam ser a proteção dessas guardas (`app/ponto/tests/test_he_revogada_na_porta.py:49`, `:68`, `:83`, `:92`, `:108`, `:120`).
- Escritores funcionais encontrados continuam `_gravar` e `_marcar_revogada`, ambos na porta (`app/ponto/portas/he.py:66-86`, `:401-403`); nenhum escritor novo no diff fornecido [nao verificado quanto a acessos dinâmicos externos].

**0 procedente funcional introduzido identificado nesta fatia da análise** (`app/ponto/portas/he.py:148`, `:196-208`, `:347`, `:481`, `:589-595`).

## 15/11 21:38 · sessao 263abdcd5488 · - · - · origem=Ronald (tmux)

Auditoria estática do c5:

- A trava passa pela porta registrada: `_travar` chama `PerguntaDisputa.trava_na_ordem`; esta ordena os colaboradores por pk e trava Colaborador antes de Disputa, Chamado e Pergunta. As cinco portas mantêm trava, leitura e escrita dentro do mesmo `atomic`. `app/colaboradores/services/colaborador.py:78`, `app/colaboradores/services/colaborador.py:120`, `app/colaboradores/services/colaborador.py:134`, `app/colaboradores/services/colaborador.py:147`, `app/colaboradores/services/colaborador.py:163`, `app/colaboradores/services/colaborador.py:182`, `app/chamados/models.py:2730`, `app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`.
- Editar, transferir e reativar traduzem `DesligadoSemReadmissao` depois da saída do bloco transacional; os casos específicos de recusa deixam mensagem. Execução efetiva dessas telas/testes: [nao verificado]. `app/colaboradores/views.py:1350`, `app/colaboradores/views.py:1412`, `app/colaboradores/views.py:1436`.
- O decorator `gate_disputa` não adquire trava nem abre transação; apenas consulta a disputa e chama a view. `app/colaboradores/mixins.py:300`.
- **PROCEDENTE — residual de ausência parcialmente gravada:** a view chama `criar_ausencia`, cujo INSERT termina em `atomic` próprio, e somente depois tenta alterar `situacao`; se a guarda recusar, o `except` da view traduz o erro, mas não desfaz a ausência já gravada. `app/ponto/views.py:2452`, `app/ponto/services/ausencia.py:478`, `app/ponto/views.py:2472`, `app/ponto/views.py:2495`.
- **Observação — desligamento simultâneo ainda pode repetir a cascata:** a validação de “já desligado” ocorre antes da trava; destino `desligado` faz a guarda retornar imediatamente. Dois executores com veredictos anteriores podem serializar o UPDATE, mas ambos seguem para histórico e chamado. É preexistente ao c5; ocorrência em produção [nao verificado]. `app/colaboradores/views.py:1450`, `app/colaboradores/views.py:1453`, `app/colaboradores/services/desligamento.py:38`, `app/colaboradores/services/colaborador.py:63`, `app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:195`. O cron também julga antes de entrar no executor: `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:43`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`.
- **Observação — objeto desligado antigo pode desfazer uma reativação:** editar/transferir/vincular podem carregar `situacao='desligado'`; após reativação concorrente, `salvar_inteiro` aceita o destino desligado sem conferir o estado atual e grava o objeto inteiro. Isso recoloca `desligado` fora do ato `executar_desligamento`, embora pela porta de persistência. É preexistente ao c5; ocorrência em produção [nao verificado]. `app/colaboradores/views.py:1243`, `app/colaboradores/views.py:1346`, `app/colaboradores/views.py:1381`, `app/colaboradores/views.py:1399`, `app/colaboradores/views.py:319`, `app/colaboradores/views.py:329`, `app/colaboradores/services/colaborador.py:63`, `app/colaboradores/services/colaborador.py:149`.
- **Observação condicional sobre ordem:** `Colaborador.save` pode adquirir trava de outro Colaborador quando `matricula_hasner` está vazia. A porta c5 já terá travado o alvo, sem reunir ambos por pk no topo. A existência de linhas persistidas sem matrícula e uma corrida que execute esse ramo são [nao verificado]; portanto não sustenta afirmar ausência absoluta de inversão em qualquer dado. `app/colaboradores/models.py:427`, `app/colaboradores/models.py:436`, `app/colaboradores/services/colaborador.py:148`, `app/colaboradores/services/colaborador.py:149`.

## 15/11 21:38 · sessao 068acc45ca88 · - · - · origem=Ronald (tmux)

- **Observação — merge:** a cópia mostra comportamento novo de apresentação na tela de batida: prévia da selfie alternativa e mensagens conforme `status` da resposta (`app/templates/ponto/bater_ponto_v2.html:168`, `:269`, `:288`, `:365`). Essa tela é renderizada por `bater_ponto`, outra função do mesmo `views.py` que contém chamadores de HE (`app/ponto/views.py:359`, `:499`). O envio é a `/api/ponto/bater/`, enquanto as ações HE têm rotas próprias (`app/templates/ponto/bater_ponto_v2.html:360`; `app/ponto/urls.py:19`, `:21`, `:24`, `:25`). Não identifiquei ligação nova com as portas de HE nesses trechos. Confirmação integral do delta histórico do main e dos SHAs: **[nao verificado]**, pois a cópia não contém `.git`.

- **Observação — o relato do cron está errado:** no diff fornecido, o trecho antigo de `recusar_ponta_pequena` já barrava somente `_ja.revogada_em is None`; uma linha revogada já passava para `_gravar`. A substituição por `_vigente(_ja)` preserva esse comportamento sob o juiz atual. A cópia inclusive contém teste anterior específico de re-recusar a mesma linha depois da revogação, preservando PK e limpando marcas (`app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:222`, `:235`, `:237`, `:240`, `:242`). A porta atual chama `_gravar` e retorna, sem `recalcular_por_evento` (`app/ponto/portas/he.py:369`, `:372`). Assim, a afirmação “esta obra passa a re-recusar via cron e esse ato chama recálculo” não é sustentada pelo código.

- **Observação — RED tem mordida funcional:** a linha revogada nasce pelas duas portas reais (`app/ponto/tests/test_he_revogada_na_porta.py:43`, `:44`); os casos conferem vigência, mesma quantidade de linhas, origem, minutos, trilha e idempotência (`:57`, `:60`, `:63`, `:64`, `:65`, `:78`, `:80`). Recusa em lote e autorização em lote têm verificações próprias (`:83`, `:92`); a recusa vigente do admin continua protegida (`:106`).

- **Observação — alcance do RED:** os testes novos mockam `recalcular_por_evento`; provam sua chamada, não os números gravados em `FechamentoMensal`/`DiaPago` (`app/ponto/tests/test_he_revogada_na_porta.py:29`, `:55`, `:66`, `:73`, `:97`). Não há teste novo de prévia/confirmar passando pela view/serviço: as importações e atos exercitados são diretamente as portas (`:52`, `:85`, `:94`). Resultados da pista relatados no anexo: **[nao verificado]**.

- **Observação — selo existente:** o selo O273 exclui explicitamente `ponto/portas/he.py` da busca de leitores crus; portanto ele não protege sozinho a nova delegação `_vigente` dentro da porta (`app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:460`, `:473`). Isso é limitação declarada, não prova de defeito. Os testes comportamentais novos cobrem os ramos principais (`app/ponto/tests/test_he_revogada_na_porta.py:50`, `:68`, `:83`, `:92`, `:106`, `:118`).

**0 procedente neste recorte**; a incorreção da narrativa sobre cron é observação documental, sustentada pelo teste de reativação já existente e pelo corpo atual da porta (`app/ponto/tests/test_o273_recusa_do_sistema_envelhece.py:222`; `app/ponto/portas/he.py:369`).

## 15/11 21:09 · sessao 6ac596dbfc1f · BUG-ENC-CELULAS-FORMA · verificador · origem=Code

1) **A carga de `CelulaDia` existe nesse caminho; o vigia não explica esses quatro cenários.** O caminho é:

`registrar_batida` → `Batida.objects.create` (`app/ponto/registro_batida.py:158`) → receiver `_cartorio_post_save_batida` (`app/ponto/signals.py:135`) → `_julgar_dias` → `julgar_celula` (`app/ponto/signals.py:93`) → `julgar_colab` (`app/ponto/services/cartorio.py:2040`) → `_julgar_colab_corpo` (`app/ponto/services/cartorio.py:996`) → `_turno_encerrado` (`app/ponto/services/cartorio.py:1397`) → `dias_encerrados` (`app/ponto/services/cartorio.py:1154`) → `turnos_do_colab`, sem `celulas` (`app/ponto/turnos.py:2162`).

Isso ativa `_cel_per` com a janela com pad e executa uma consulta de `CelulaDia`. (`app/ponto/turnos.py:1867`; `app/ponto/turnos.py:1889`; `app/ponto/turnos.py:1891`; `app/escala/alimentacao.py:75`)

`_ontem_em_janela` pertence ao caminho de varredura do vigia e do comando de competência. (`app/ponto/services/vigia_de_hora.py:123`; `app/chamados/management/commands/disparar_perguntas_competencia.py:186`) A atribuição **exclusiva do delta líquido +1** nos quatro cenários, sem a captura SQL mencionada pelo comentário, permanece **[não verificado]**.

2) **A unidade garantida pelo memo é uma carga por execução de `_julgar_colab_corpo` que consulta encerramento.** `_enc` nasce dentro desse corpo; chamadas seguintes nessa execução reutilizam o mapa. (`app/ponto/services/cartorio.py:1149`; `app/ponto/services/cartorio.py:1152`)

Isso não garante uma carga por chamada completa de `julgar_colab`: fechar por marcos enfileira a célula para rejulgamento, e o wrapper executa novamente o corpo, recriando `_enc`. A consulta de encerramento ocorre inclusive antes do pulo por impressão. (`app/ponto/services/cartorio.py:1543`; `app/ponto/services/cartorio.py:1021`; `app/ponto/services/cartorio.py:1397`; `app/ponto/services/cartorio.py:1413`)

Portanto, “constante por passada, independente do número de chamados” confere; usar o memo como explicação suficiente de “+1 no ato inteiro” precisa correção. (`app/ponto/services/cartorio.py:1152`; `app/ponto/services/cartorio.py:1021`)

Comparando com o diff fornecido, a história anterior dos quatro comentários foi preservada após o novo prefixo. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`; `app/ponto/tests/test_selo_chokepoint_escrita.py:210`; `app/ponto/tests/test_selo_chokepoint_escrita.py:228`; `app/ponto/tests/test_selo_chokepoint_escrita.py:235`)

3) **Sim, as três identidades conferem:**

| Registro na catraca | Ocorrência na função correspondente |
|---|---|
| `app/core/tests/test_medida_turno_do_par.py:336` | `_dia_das_batidas_puro` — `app/ponto/turnos.py:2967` |
| `app/core/tests/test_medida_turno_do_par.py:337` | `dia_do_turno_da_batida` — `app/ponto/turnos.py:2945` |
| `app/core/tests/test_medida_turno_do_par.py:338` | `par_completo` — `app/ponto/turnos.py:2212` |

A catraca verifica identidade/critério e linhas separadamente. (`app/core/tests/test_medida_turno_do_par.py:543`; `app/core/tests/test_medida_turno_do_par.py:549`)

**VEREDITO:**

- **1 procedente — precisão da justificativa C8:** delimitar “1 query” por execução do corpo; o memo não sustenta sozinho “+1 por ato completo”. Os valores dos tetos não estão demonstrados incorretos. (`app/ponto/services/cartorio.py:1149`; `app/ponto/services/cartorio.py:1021`)

## 15/11 21:08 · sessao 2c95008215f6 · - · - · origem=Ronald (tmux)

`_ontem_em_janela` não integra o caminho dos quatro cenários: suas chamadas de produção estão em `rodar` e no comando `disparar_perguntas_competencia` (`app/ponto/services/vigia_de_hora.py:123`; `app/chamados/management/commands/disparar_perguntas_competencia.py:186`).

Caminho dos testes: [nao verificado]

- Captura de `registrar_batida` nos helpers dos cenários (`app/ponto/tests/test_selo_chokepoint_escrita.py:429`; `app/ponto/tests/test_selo_chokepoint_escrita.py:465`).
- `Batida.objects.create` → receiver `post_save` → `_julgar_dias` → `julgar_celula`, para d−1 e d (`app/ponto/registro_batida.py:158`; `app/ponto/signals.py:131`; `app/ponto/signals.py:135`; `app/ponto/signals.py:93`; `app/ponto/signals.py:112`).
- `julgar_celula` → `julgar_colab` → `_julgar_colab_corpo` → `_turno_encerrado` → `dias_encerrados` (`app/ponto/services/cartorio.py:2040`; `app/ponto/services/cartorio.py:996`; `app/ponto/services/cartorio.py:1397`; `app/ponto/services/cartorio.py:1154`).
- `dias_encerrados` chama `turnos_do_colab` sem `celulas`; este carrega `celulas_do_periodo` na janela com pad, mediante uma consulta (`app/ponto/turnos.py:2162`; `app/ponto/turnos.py:1867`; `app/ponto/turnos.py:1889`; `app/ponto/turnos.py:1891`; `app/escala/alimentacao.py:73`).

Ressalva material para a pergunta 2: `_enc` é local **a cada execução de `_julgar_colab_corpo`**, não compartilhado por todo `julgar_colab` (`app/ponto/services/cartorio.py:1149`). O wrapper pode executar novamente o corpo após reconciliação ou emissão (`app/ponto/services/cartorio.py:1021`; `app/ponto/services/cartorio.py:1086`). No ramo normal de dia passado, a consulta de encerramento precede inclusive o pulo por impressão (`app/ponto/services/cartorio.py:1397`; `app/ponto/services/cartorio.py:1409`). Fechar por marcos também enfileira a célula para essa segunda execução (`app/ponto/services/cartorio.py:1539`; `app/ponto/services/cartorio.py:1543`).

Portanto, “uma query por execução do corpo que consulta encerramento, independente do número de itens nessa execução” é sustentado; “uma query por chamada completa de `julgar_colab`” não é sustentado pelo memo descrito (`app/ponto/services/cartorio.py:1149`; `app/ponto/services/cartorio.py:1152`; `app/ponto/services/cartorio.py:1021`). A atribuição experimental exclusiva do delta +1, sem SQL ou log disponibilizado, permanece [não verificado].

## 15/11 21:08 · sessao 6c5d93b3ec55 · - · - · origem=Ronald (tmux)

Sim: as três linhas continuam nas funções registradas, com o mesmo critério B1:

| Registro | Função atual | Ocorrência |
|---|---|---|
| `app/core/tests/test_medida_turno_do_par.py:336` | `_dia_das_batidas_puro`, definida em `app/ponto/turnos.py:2957` | `out.setdefault(b.pk, data_local(b.timestamp))` — `app/ponto/turnos.py:2967` |
| `app/core/tests/test_medida_turno_do_par.py:337` | `dia_do_turno_da_batida`, definida em `app/ponto/turnos.py:2939` | `d = data_local(batida.timestamp)` — `app/ponto/turnos.py:2945` |
| `app/core/tests/test_medida_turno_do_par.py:338` | `par_completo`, definida em `app/ponto/turnos.py:2202` | `d = data_local(max(a.timestamp, b.timestamp))` — `app/ponto/turnos.py:2212` |

O detector registra a linha da ocorrência AST, e não a linha do `def`; a identidade é construída como `arquivo::função`. `app/core/medida_turno_do_par.py:220`, `app/core/medida_turno_do_par.py:226`, `app/core/medida_turno_do_par.py:401`.

A catraca compara identidade/critério e linhas separadamente; o censo executado apenas em memória retornou exatamente esses três registros. `app/core/tests/test_medida_turno_do_par.py:543`, `app/core/tests/test_medida_turno_do_par.py:549`, `app/core/medida_turno_do_par.py:448`.

## 15/11 20:31 · sessao 7690ac4ac86d · E4C-F1-NOVA · verificador · origem=Code

1) **NAO PROCEDE — as premissas de código conferem.**

- `_recusa_par_ilegal` compara o par atual com o carregado e permite o legado inalterado: `if not self._state.adding and _par == getattr(self, '_par_carregado', None): return` (`app/chamados/models.py:3562-3564`). O teste também permite `passivo.save()` e alteração de outro campo (`app/chamados/tests/test_e3_10_cura_no_escritor.py:190-196`).
- Os três predicados existem: retenção usa `I OU T` (`app/chamados/models.py:1905-1909`); fila usa `q_respondida_com_texto()` (`app/chamados/juizes.py:1725`), cuja expressão é `I E T` (`app/chamados/juizes.py:1559`); property usa somente `respondida_em is not None` (`app/chamados/models.py:3641`).
- Ressuscitação e simulação limpam somente instante: `respondida_em=None` e `_q.respondida_em = None` (`app/chamados/models.py:1613`, `app/chamados/models.py:1555`). O lote realmente dispensa conferir texto quando recebe instante nulo (`app/chamados/models.py:3349-3352`).
- O contrato atual é unilateral e absolve explicitamente só instante: `self.assertNotIn(GATILHO, e3, 'acusaria carimbo-sem-texto, que e legitimo')` (`app/chamados/tests/test_contract_par_resposta.py:113`).

Universo, contagens e composição 141 = 111 + 30 provêm exclusivamente do dado raspado; confirmação independente em produção, pks efetivos e conteúdo da c9: **[nao verificado]**.

2) **NAO PROCEDE — não identifiquei colisão demonstrada.**

A L-130 já estabelece unidade pergunta, juiz único, recusa dos dois pares e preservação das materializadas: “validadas/materializadas ficam” (`app/docs/LEIS.md:151`). Portanto, excluir as 30 materializadas da futura zeragem respeita esse literal; sua quantidade vem somente do anexo **[nao verificado]**.

A exceção de competência confere: “volta para a fila, com a resposta que tinha” (`app/chamados/models.py:3417-3418`). Fantasma apenas executa `self.descarimbar()` (`app/chamados/models.py:3395-3397`); essa porta salva somente via, instante e autor da materialização (`app/chamados/models.py:3291-3295`).

Registrar a frase em CORTES antes do pouso atende à trava que a exige (`CLAUDE.md:74-76`). Quanto ao item A, relógio e seleção de respondidas ocupam trechos distintos de `placar_aceite`: `agora = _tz.now()` versus filtro por `respondida_em` (`app/chamados/services/verbos.py:286`, `app/chamados/services/verbos.py:333-345`). Sobreposição concreta dos futuros patches: **[nao verificado]**.

3) **NAO PROCEDE — não encontrei condição literalmente inexequível.**

Já existe `REABERTURA`, incluída em `ATOS_COM_FOTO` (`app/chamados/models.py:4001`, `app/chamados/models.py:4012`). O escritor existente prepara zeragem dos dois campos e registra texto/instante anteriores dentro do mesmo atomic, antes do lote (`app/chamados/services/disputa_emissao.py:1609`, `app/chamados/services/disputa_emissao.py:1621-1648`).

A simulação pode acompanhar essa zeragem preservando seu universo atual, que já exige texto NULL ou vazio (`app/chamados/models.py:1530-1533`). As provas futuras de zero mudança em B1 e zero efeito entregue em B2 ainda são **[nao verificado]**; condição que falhar não autoriza pouso: “aval condicional que nao fecha na condicao = PAROU” (`CLAUDE.md:37-38`).

4) **NAO PROCEDE — nenhum alcance adicional que exija `!` ficou demonstrado.**

Há efeito indireto possível sobre encerramento: o CSF consulta `sem_veredito_entre` e retorna `not perguntas_que_seguram_csf(ch)` (`app/ponto/supra_juiz.py:573-588`). Isso justifica medir os efeitos de escritores e crons; o resultado efetivo é **[nao verificado]**.

O gravado exportado e `ponto/motor_calculo_v2.py` continuam exigindo autorização própria (`CLAUDE.md:265-268`). Se surgir dinheiro em aberta, a tabela não substitui reversão anterior, exportadas intactas com hash e prova de frota posterior (`CLAUDE.md:406-410`). Movimento financeiro adicional causado por B: **[nao verificado]**.

Correspondência desta cópia com HEAD/main `e84a167f4`: **[nao verificado]**.

VEREDITO: TUDO NAO PROCEDE

## 15/11 20:31 · sessao 53a8218927c2 · E3-RESTO-R3-C17 · verificador · origem=Code

AVAL-DEVOLVIDO — ITEM A, por inexatidão de premissa na pergunta 1.

1) **PROCEDE — a descrição do substituto c14 não confere.** A premissa “dict de 3 chaves” vale para `app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`: `Mock(return_value=dict(cego=0, calados=0, dias_sem_leitura_sistema=n))`. Entretanto, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:315-317` faz `d, _txt = vigia_censo(self.MISTO)` e `censo=lambda emps, hoje: d`. Esse helper executa o censo real e devolve seu resultado integral: `app/chamados/tests/test_e3_r3_c6_sem_banco.py:253-254`, `d = funcao(..., 'censo')(...)` / `return d, ...`. As nove chaves vêm de `app/folha/vigia_retido_calado.py:61-62` e `:89`. A descrição precisa distinguir os dois substitutos.

   As premissas centrais conferem: universo iniciado em retratos, `qs = RetratoFolha.objects.all()` (`app/folha/vigia_retido_calado.py:46`); C5 recebendo o parâmetro original, `colabs_nunca_bateu(empresas, hoje)` (`:60`); `None` abrangendo todas as empresas, `Empresa.objects.all() if empresas is None` (`app/escala/services/cadastro_realidade.py:163`); alarme estrutural atual, `ALARMA_SEMPRE = ('cego',)` (`app/folha/management/commands/vigiar_retido_calado.py:16`). Os relógios excluídos também existem: `gerado_em = timezone.now()` (`app/relatorios/pdf_base.py:133`) e `agora = _tz.now()` / `hoje = _tz.localdate()` (`app/chamados/services/verbos.py:286-287`).

2) **NAO PROCEDE.** O papel proposto permanece compatível com “CRON DE VARREDURA E VIGIA, NUNCA JUIZ” (`app/docs/LEIS.md:94`, L-060) e “so alarma, NUNCA juiz” (`CLAUDE.md:172`). O balde vigente está registrado como “sem acao disponivel” (`app/docs/CORTES.md:437`, LU-05), correspondendo ao filtro `dono_da_linha(l, hoje=hoje) == 'sem_acao_disponivel'` (`app/folha/vigia_retido_calado.py:66`). Os textos anteriores completos de 4.2/4.3 e ORDEM-VIVA-1511-NOITE: **[nao verificado]**.

3) **NAO PROCEDE.** Os comportamentos novos são construção autorizada, não requisitos já satisfeitos: hoje `dataset_furos(..., agora=None)` permite omissão (`app/relatorios/furos.py:103`) e transporta `agora` ao D4 (`:123-124`). Também existe o encaixe para eliminar o relógio interno do limbo: a borda captura `_agora` e deriva `hoje` (`app/relatorios/views.py:1519-1520`), mas ainda chama `escalas_em_limbo(empresa)` (`:1554`). A realização futura da medição de produção e sua publicação: **[nao verificado]**.

4) **NAO PROCEDE.** O caminho examinado consulta e conta, terminando em `return out` (`app/folha/vigia_retido_calado.py:44-90`); o comando declara “NAO TEM `--apply`” (`app/folha/management/commands/vigiar_retido_calado.py:9`) e alarma por `raise SystemExit(2)` (`:41-42`). Isso não demonstra escrita no gravado exportado nem intervenção no motor, ações que exigem autorização específica (`CLAUDE.md:142`, `:265-268`). Pares efetivamente ausentes e efeitos concretos em produção: **[nao verificado]**.

VEREDITO: PROCEDE em 1

## 15/11 20:30 · sessao b0949139420b · - · - · origem=Ronald (tmux)

Não identifiquei procedente concreto nas premissas dos escritores ou na executabilidade de B1.

- **Comparação com o par carregado confere:** `app/chamados/models.py:3563-3564`: `if not self._state.adding and _par == getattr(self, '_par_carregado', None): return`. O `save` chama a guarda em `:3529`; `from_db` guarda a foto em `:3546`. O teste mantém explicitamente o legado intocado: `app/chamados/tests/test_e3_10_cura_no_escritor.py:192-193`: `passivo.save()` e `passivo.save(update_fields=['pergunta'])`.
- **Guarda atual é unilateral, como descrito:** nascimento chama `recusa_respondida_sem_texto` em `app/chamados/models.py:2921-2923`; automática em `app/chamados/services/disputa_emissao.py:1671`. A porta verifica apenas `respondida_sem_texto` em `app/chamados/services/fio.py:1601-1605`. O lote só consulta texto quando o novo instante é não nulo: `app/chamados/models.py:3349-3352`: `if outros['respondida_em'] is not None:`. Portanto a passagem `respondida_em=None` sem conferir texto existe.
- **Ressuscitação e simulação limpam só instante:** `app/chamados/models.py:1613`: `descarimbar_em_lote(... respondida_em=None)`; simulação `:1555`: `_q.respondida_em = None`. O universo atual já restringe texto a NULL/`''` em `:1533`, permitindo preservar o comportamento de seleção ao acrescentar zeragem do texto.
- **Exceções de reabertura conferem:** `app/chamados/models.py:3417-3421`: “com a resposta que tinha” e `self.descarimbar()`; fantasma faz apenas `self.descarimbar()` em `:3395-3397`; reabertura por resposta retorna `self.descarimbar(salvar=False)` em `:3412-3414`. `descarimbar` grava somente `via_resolucao`, `materializada_em`, `materializada_por` em `:3291-3295`.
- **Ato e escritor existentes comportam o pedido:** `app/chamados/models.py:4001` já define `REABERTURA`; `:4012` inclui esse ato entre os que admitem foto, com constraint em `:4040-4043`. `app/chamados/services/disputa_emissao.py:1609` zera ambos os campos; dentro do atomic de `:1621`, registra texto e instante anteriores em `:1631-1637`, antes do lote em `:1648`. Não precisa criar tipo de ato para cumprir esse pedido.
- **Contrato estático atual é unilateral:** `app/chamados/tests/test_contract_par_resposta.py:81`: `if GATILHO in escritos and EXIGIDO not in escritos`; `:113` absolve explicitamente escrita só de instante. A mudança solicitada deve adaptar esse contrato para as duas direções, sem confundir escrituras legítimas que completam o par com transições ilegais; por exemplo, `carimbar_resposta_pela_trilha` exige texto já existente em `app/chamados/services/disputa_emissao.py:1691` antes de escrever só instante em `:1695-1697`.

## 15/11 20:30 · sessao d94200c4f926 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- Premissas principais conferem: [app/relatorios/pdf_base.py:133] `gerado_em = timezone.now()`; [app/chamados/services/verbos.py:286] `agora = _tz.now()` e [:287] `hoje = _tz.localdate()`. As exclusões nomeadas do aval correspondem a relógios existentes.
- [app/relatorios/furos.py:103] tem `dataset_furos(... agora=None)`; [:123-124] transporta `agora` ao D4 e [:160-164] ao calendário. Exigir instante válido é mudança futura expressamente autorizada; não é premissa de comportamento já existente.
- [app/relatorios/furos.py:59] lê `timezone.localdate()` em `escalas_em_limbo`; [app/relatorios/views.py:1519-1520] já captura `_agora` e deriva `hoje`, mas [:1554] chama `escalas_em_limbo(empresa)` sem dia. Item 6 corresponde ao código.
- O selo perde multiplicidade: [app/chamados/tests/test_e3_r3_c15_sem_banco.py:162-163] retorna `sorted({n.func.attr ...})`, conjunto de nomes. [:182-184] reconhece somente atribuições cujo chamado seja atributo `now`. Item 8 cobre defeitos efetivos.
- Casos c16 citados existem: [app/chamados/tests/test_e3_r3_c16_sem_banco.py:78-113] cobre meia-noite, competência explícita velha, relavra parcial, falta parcial e vazio. [app/chamados/tests/e3_r3_c16_mutantes.py:26-29] contém alvos literais do laço atual; renovar alvo após refatorar é coerente.
- Substitutos sem banco citados conferem: c4:149-157, c5:96-103, c6:246-253, c14:341-350, c15:77-85 e c16:63-74 não fornecem consulta de `empresas_visiveis`. Negativos c5:243-245 são falhas de assinatura antes do corpo.

Única inexatidão literal encontrada no anexo (a): a descrição conjunta “substituto do command c11 ... e c14 ... (dict de 3 chaves)” só vale para c11. [app/chamados/tests/test_e3_r3_c11_sem_banco.py:275] contém `Mock(return_value=dict(cego=0, calados=0, dias_sem_leitura_sistema=n))`. Em c14, [:315] `d, _txt = vigia_censo(self.MISTO)` e [:317] `censo=lambda emps, hoje: d` devolvem o dict integral. A origem está em [app/chamados/tests/test_e3_r3_c14_sem_banco.py:23] (helper importado do c6) e [app/chamados/tests/test_e3_r3_c6_sem_banco.py:253-254] (executa `censo` real). As nove chaves estão em [app/folha/vigia_retido_calado.py:61-62] e [:89]. Essa inexatidão não impede construir, mas contraria a descrição literal do anexo se a pergunta 1 exigir conferir também essa premissa auxiliar.

Não identifiquei outra condição técnica inexequível na ordem da construção nos trechos examinados; execução efetiva futura [nao verificado].

## 15/11 20:29 · sessao cabd9c751e18 · - · - · origem=Ronald (tmux)

Q2: **NAO PROCEDE**, quanto às leis e exceções examinadas.

- A L-130 já fixa `I ∧ T`, unidade pergunta, recusa das duas metades ilegais e preservação das validadas/materializadas: `app/docs/LEIS.md:151`, trecho: “validadas/materializadas ficam”.
- A exceção de competência confere: `app/chamados/models.py:3417-3421`, trecho: “volta para a fila, com a resposta que tinha”; executa `self.descarimbar()`.
- A fantasma só desfaz o carimbo: `app/chamados/models.py:3392-3397`, trecho: “So desfaz a via `fantasma` […] Gemea de `reabrir_por_competencia`”.
- Registrar a frase antes de pousar satisfaz a trava: `CLAUDE.md:74-76`, trecho: “exige a frase `corte Ronald: juiz <nome> nasce` no CORTES.md”.

Q4: **NAO PROCEDE** como acusação demonstrada pelos caminhos examinados; efeito financeiro real da mudança permanece **[nao verificado]**.

- O gate do CSF recebe a retenção que B2 pretende trocar: `app/ponto/supra_juiz.py:573-588`, trechos: `DisputaSupervisao.sem_veredito_entre(...)` e `return not perguntas_que_seguram_csf(ch)`. Portanto a tabela de efeitos secundários exigida pelo aval é necessária.
- `!` continua obrigatório para gravado exportado e motor: `CLAUDE.md:265-268`, trecho: “tocar o GRAVADO de competencia EXPORTADA […] `ponto/motor_calculo_v2.py`”.
- Caso apareça dinheiro em aberta, publicar a tabela não dispensa as condições gerais: `CLAUDE.md:406-410`, trecho: “PRE-APROVADO com os TRES juntos”: reversão antes, exportadas intactas com hash e prova de frota depois.

A L-126 proíbe **construtor Codex** de tocar `core/juizes.py`: `app/docs/LEIS.md:147`, trecho: “PROIBIDO ao construtor […] `core/juizes.py`”. O item B não identifica o construtor, portanto isso não demonstra colisão do aval; atribuição efetiva permanece **[nao verificado]**.

## 15/11 20:29 · sessao 3c54cafb2d59 · - · - · origem=Ronald (tmux)

2) **NAO PROCEDE** quanto a colisão identificável nas leis lidas. O alarme permanece vigia: `CLAUDE.md:172`, “contador ‘esperado 0’, so alarma, NUNCA juiz”; `app/docs/LEIS.md:94` (L-060), “CRON DE VARREDURA E VIGIA, NUNCA JUIZ”. A medição pela função real e a publicação exigidas pelo item são compatíveis com `CLAUDE.md:35-38`, “Medir chamando a funcao REAL” e “ESCOPO DO AVAL E’ LITERAL”. Os textos completos de 4.2/4.3 e ORDEM-VIVA-1511 não foram localizados na árvore: **[nao verificado]**.

4) **NAO PROCEDE** quanto a exigência adicional de `!` demonstrada pelo escopo A. `CLAUDE.md:265-268` exige `!` para “tocar o GRAVADO de competencia EXPORTADA”, escala/vínculo, regra fora do pedido, atalhos e motor. Nos caminhos examinados:

- `app/folha/vigia_retido_calado.py:44-90`: consultas, contagens e `return out`.
- `app/folha/management/commands/vigiar_retido_calado.py:9`: “NAO TEM `--apply`”; `:37-42`: seleção dos alarmes e `raise SystemExit(2)`.
- `app/escala/services/cadastro_realidade.py:150`: “Nenhum push: a lista nao emite”; `:163-173`: consultas e `return out`.
- `app/relatorios/furos.py:52-100`: consultas e montagem da lista de limbo; `:123-124`: `status_dos_dias_lavrados(..., agora=agora)`; `:160-167`: calendário e retorno dos cards.

Efeito concreto em produção, pares ausentes e motivos prováveis: **[nao verificado]**.

## 15/11 20:25 · sessao 540be23be5a7 · BUG-ENC-CELULAS-FORMA · verificador · origem=Code

O diff remove os dois repasses incompatíveis. Não encontrei chamador de produção residual que entregue chave composta ao tripwire. Permanecem dois problemas anteriores ao diff: alimentação potencialmente sem pad no fechamento e diferença entre os carregadores singular e plural. (`app/ponto/turnos.py:2162`; `app/ponto/services/vigia_de_hora.py:61`; `app/ponto/services/fechamento.py:605`; `app/ponto/turnos.py:2105`; `app/ponto/turnos.py:2154`.)

Execução dos testes, RED anterior, GREEN, cumprimento dos tetos e alcance em produção: **[nao verificado]**. [nao verificado]

**1. Quem produz, quem lê e todos os chamadores de produção**

Os contratos são diferentes:

| Função | Forma de `celulas` e leitores |
|---|---|
| `turnos_do_colab` | `{data: CelulaDia}` da janela com pad; alimenta candidatos por geradora, papel da ata e segmentação. Converte internamente para composta ao perguntar a `vinculo_do_dia`. (`app/ponto/turnos.py:1854`; `app/ponto/turnos.py:1893`; `app/ponto/turnos.py:1944`; `app/ponto/turnos.py:1961`; `app/ponto/turnos.py:1965`.) |
| `dia_encerrado` | Repassa o argumento a `dia_encerrado_motivo`. (`app/ponto/turnos.py:2083`.) |
| `dia_encerrado_motivo` | Repassa `{data: CelulaDia}` somente ao pareador; não fornece essa alimentação a `_escala_vigente` nem ao julgamento do fim previsto. (`app/ponto/turnos.py:2104`; `app/ponto/turnos.py:2105`.) |
| `dias_encerrados` | `{(colaborador_id,data): CelulaDia}`; alimenta vínculo e DNA do encerramento. Depois do diff, não alimenta o pareador com esse mapa. (`app/ponto/turnos.py:2138`; `app/ponto/turnos.py:2153`; `app/ponto/turnos.py:2162`; `app/ponto/turnos.py:2178`; `app/ponto/turnos.py:2179`.) |

O produtor canônico `celulas_do_periodo` constrói chaves compostas; o pareador, quando recebe `None`, carrega o pad e projeta para data. O leitor final do DNA procura `(self.colaborador_id,data)`. (`app/escala/alimentacao.py:73`; `app/ponto/turnos.py:1889`; `app/ponto/turnos.py:1891`; `app/escala/models.py:1120`.)

**Chamadores que fornecem `celulas`, incluindo repasses internos:**

| Chamador | Destino | Forma e alcance |
|---|---|---|
| `app/escala/utils.py:1581` | `turnos_do_colab` | Data, projetada da carga feita com `janela_com_pad`. (`app/escala/utils.py:1307`; `app/escala/utils.py:1308`.) |
| `app/escala/utils.py:1898` | `turnos_do_colab` | Data; conserva células do pad e substitui a ata dos dias montados por cópias em memória. (`app/escala/utils.py:1881`; `app/escala/utils.py:1884`; `app/escala/utils.py:1886`; `app/escala/utils.py:1897`.) |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` | `turnos_do_colab` | Data; carga `[ini−2,fim+2]`, que contém o pad atual `[ini−2,fim+1]`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:87`; `app/ponto/turnos.py:1847`.) |
| `app/ponto/turnos.py:2104` | `turnos_do_colab` | Repassa o parâmetro de `dia_encerrado_motivo`. |
| `app/ponto/services/fechamento.py:377` | `dia_encerrado` | Data; recorte do mapa recebido, com risco de pad incompleto explicado abaixo. (`app/ponto/services/fechamento.py:370`; `app/ponto/services/fechamento.py:375`.) |
| `app/ponto/turnos.py:2083` | `dia_encerrado_motivo` | Repassa o parâmetro de `dia_encerrado`. |
| `app/ponto/services/cartorio.py:1154` | `dias_encerrados` | Composta `(c.pk,x.data)`, sem `turnos`; agora não alcança o tripwire. (`app/ponto/turnos.py:2162`.) |

**Demais chamadas de `turnos_do_colab`: todas omitem `celulas`, portanto recebem o default `None`.** (`app/ponto/turnos.py:1850`.)

| Chamadas |
|---|
| `app/ponto/turnos.py:780`; `app/ponto/turnos.py:2162`; `app/ponto/turnos.py:2193`; `app/ponto/turnos.py:2925`; `app/ponto/turnos.py:3116` |
| `app/ponto/turno_leitura.py:89`; `app/ponto/turno_leitura.py:95` |
| `app/ponto/nucleo.py:47` |
| `app/ponto/views.py:380`; `app/ponto/views.py:1588`; `app/ponto/views.py:3217` |
| `app/relatorios/pdf_espelho.py:213`; `app/relatorios/pdf_espelho.py:451` |
| `app/escala/regua_defesa.py:358` |
| `app/escala/servico_jornada.py:249`; `app/escala/servico_jornada.py:403` |
| `app/api/views.py:1441` |
| `app/chamados/models.py:1359` |
| `app/chamados/juizes.py:401`; `app/chamados/juizes.py:2168` |
| `app/chamados/detector_anomalias.py:157` |
| `app/colaboradores/services/drawer.py:45` |
| `app/ponto/services/espelho.py:614` |
| `app/ponto/services/bordas_realizado.py:66` |
| `app/ponto/services/teto_na_lista.py:68` |
| `app/ponto/services/vigia_pareador_celula.py:97` |
| `app/ponto/services/esmeril_espelho.py:98` |
| `app/ponto/services/triagem_batida.py:553` |
| `app/ponto/services/regularizacao.py:61` |
| `app/ponto/services/passivo_s84.py:82` |
| `app/ponto/management/commands/detectar_par_relampago.py:101` |
| `app/ponto/management/commands/dna_x_batida_real.py:69` |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:46` |
| `app/ponto/management/commands/detectar_ausencias.py:337` |
| `app/ponto/management/commands/vigia_plantio_overnight.py:38` |
| `app/ponto/management/commands/verificar_nucleo.py:29` |
| `app/ponto/management/commands/e6_oraculo.py:159` |
| `app/ponto/management/commands/censo_fase_12x36.py:180` |
| `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124` |
| `app/escala/services/escala_auto.py:87` |
| `app/escala/services/impacto_troca.py:18` |
| `app/chamados/services/materializacao.py:124`; `app/chamados/services/materializacao.py:221` |
| `app/chamados/services/disputa_emissao.py:722` |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:145` |

**Demais chamadas das outras três funções: todas omitem `celulas`.**

| Destino | Chamadas |
|---|---|
| `dia_encerrado` | `app/ponto/services/vigia_de_hora.py:61`; `app/ponto/services/flip_auto.py:285`; `app/ponto/management/commands/detectar_ausencias.py:474` |
| `dia_encerrado_motivo` | `app/chamados/reconciliador.py:1091` |
| `dias_encerrados` | `app/ponto/turnos.py:2191`; `app/ponto/management/commands/supra_juiz.py:121`; `app/ponto/management/commands/emitir_furo_retroativo.py:63` |

Os métodos homônimos chamados em `app/ponto/status_dia.py:274` e `app/core/services/painel_op.py:334` são de `CargaDaJanela`; chamam diretamente o corpo puro, sem passar pelas quatro funções públicas. (`app/ponto/turnos.py:3057`.)

**Tupla residual em produção:** nenhum fornecedor acima entrega composta ao pareador depois do diff; a composta do cartório permanece no encerramento. Chamadores externos à cópia ou construídos dinamicamente: **[nao verificado]**. (`app/escala/utils.py:1581`; `app/escala/utils.py:1898`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:86`; `app/ponto/services/fechamento.py:375`; `app/ponto/turnos.py:2162`.)

**Data sem pad: sim, há caminho no fechamento.** O envelope começa no início do corte 2 e termina no fim do corte 28; as células são carregadas nesse envelope sem ampliação. `_furos_encerrados` calcula o pad de cada dia, mas apenas copia o que encontra e sempre passa um dict, inclusive vazio. (`app/ponto/services/fechamento.py:584`; `app/ponto/services/fechamento.py:605`; `app/ponto/services/fechamento.py:606`; `app/ponto/services/fechamento.py:370`; `app/ponto/services/fechamento.py:377`.)

Consequentemente, cortes **2 e 3** podem ultrapassar o envelope no início, e corte **28**, no fim. O dict não-`None` suprime a carga própria; o tripwire não verifica completude. É problema anterior ao diff; clientes nesses cortes e células efetivamente omitidas em produção são **[nao verificado]**. (`app/ponto/janelas.py:15`; `app/ponto/janelas.py:20`; `app/ponto/turnos.py:1847`; `app/ponto/turnos.py:1865`; `app/ponto/turnos.py:1889`.)

**2. Testes existentes que quebrariam por chave composta** [nao verificado]

Não encontrei teste existente passando composta diretamente ao pareador ou aos dois singulares. As compostas encontradas no teste existente são destinadas a `dias_encerrados`, cujo contrato continua válido. (`app/ponto/tests/test_c1_dias_encerrados.py:64`; `app/ponto/tests/test_c1_dias_encerrados.py:66`; `app/ponto/tests/test_c1_dias_encerrados.py:77`; `app/ponto/turnos.py:2162`.)

O novo teste que passa composta ao pareador **espera `TypeError`**; o novo teste do cartório passa composta corretamente à casca plural. (`app/ponto/tests/test_bug_enc_celulas_forma.py:87`; `app/ponto/tests/test_bug_enc_celulas_forma.py:88`; `app/ponto/tests/test_bug_enc_celulas_forma.py:33`.)

Resultado efetivo da suíte: **[nao verificado]**. [nao verificado]

**3. Custo novo e selos de queries**

**Sim: há uma nova consulta direta de células por execução alcançada do memo `_enc` do cartório.** Antes a alimentação não-`None` suprimia essa carga; agora a chamada sem `celulas` aciona `celulas_do_periodo` do pad, que executa um SELECT. Não é uma consulta por dia: o mapa de encerramento fica memoizado por corpo/janela. (`app/ponto/services/cartorio.py:1152`; `app/ponto/services/cartorio.py:1154`; `app/ponto/turnos.py:2162`; `app/ponto/turnos.py:1889`; `app/ponto/turnos.py:1891`; `app/escala/alimentacao.py:73`.)

O cron chama o julgamento por colaborador; segunda passada e releitura podem abrir outro corpo e outro memo. O delta líquido total pode também mudar pela consulta complementar de geradoras. Volume, tempo e delta líquido de produção: **[nao verificado]**. (`app/ponto/management/commands/processar_cartorio.py:59`; `app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:1021`; `app/ponto/services/cartorio.py:1086`; `app/escala/alimentacao.py:155`; `app/escala/alimentacao.py:157`.)

**Existe cobertura indireta com teto:** o selo de chokepoint registra batida histórica com células e furos vivos, captura SQL, exige o fechamento dos chamados e cobra o teto. O outro ramo mede chamados que permanecem vivos. A cobrança usa `assertLessEqual`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:281`; `app/ponto/tests/test_selo_chokepoint_escrita.py:428`; `app/ponto/tests/test_selo_chokepoint_escrita.py:437`; `app/ponto/tests/test_selo_chokepoint_escrita.py:443`; `app/ponto/tests/test_selo_chokepoint_escrita.py:464`; `app/ponto/tests/test_selo_chokepoint_escrita.py:478`; `app/ponto/tests/test_selo_chokepoint_escrita.py:293`.)

O caminho é signal → `julgar_celula` → `julgar_colab` → `_turno_encerrado` para dia passado. (`app/ponto/signals.py:135`; `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:2040`; `app/ponto/services/cartorio.py:1397`.)

Os testes novos verificam respostas e paridade, sem contagem de queries; o selo do D4 mede o painel com lote. Teto específico do cron e aprovação dinâmica desses tetos nesta obra: **[nao verificado]**. (`app/ponto/tests/test_bug_enc_celulas_forma.py:43`; `app/ponto/tests/test_e4_carga_paridade.py:148`; `app/core/tests/test_e4_7iii_lote_d4.py:43`; `app/core/tests/test_e4_7iii_lote_d4.py:60`.)

**4. Lista, queryset e outros tipos**

O tripwire verifica elementos iterados que sejam tuplas; **não valida que o argumento seja um mapa nem que suas chaves sejam datas**. Depois, o código exige `.values()` e `.items()`. (`app/ponto/turnos.py:1865`; `app/ponto/turnos.py:1893`; `app/ponto/turnos.py:1631`; `app/ponto/turnos.py:1961`.)

Por leitura do código: lista não vazia contendo tupla dispara o `TypeError`; lista sem tuplas, inclusive vazia, chega a `.values()` e falha; queryset pode ser avaliado pela condição booleana e depois falhar na exigência de `.items()`. Execução desses casos: **[nao verificado]**. (`app/ponto/turnos.py:1865`; `app/ponto/turnos.py:1893`; `app/ponto/turnos.py:1631`.)

Não encontrei fornecedor de produção usando lista/queryset nos quatro parâmetros; os fornecedores explícitos estão na tabela da pergunta 1. Há teste usando **`MappingProxyType({})`**, que é mapa e não `dict`, como alimentação válida. (`app/escala/utils.py:1581`; `app/escala/utils.py:1898`; `app/ponto/services/fechamento.py:377`; `app/ponto/services/cartorio.py:1154`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:54`; `app/ponto/tests/test_o142_turno_invariante_na_janela.py:169`.)

**5. Classes que já reprovaram obra**

| Classe | Parecer |
|---|---|
| Segundo juiz | Não identificado no diff: continuam as autoridades registradas e os mesmos corpos puros. (`app/core/juizes.py:212`; `app/core/juizes.py:239`; `app/ponto/turnos.py:1920`; `app/ponto/turnos.py:2161`; `app/ponto/turnos.py:3048`; `app/ponto/turnos.py:3057`.) |
| Segundo escritor | Não identificado nas alterações: validação, carga e delegação, sem novo escritor de estado. (`app/ponto/turnos.py:1865`; `app/ponto/turnos.py:2153`; `app/ponto/services/vigia_de_hora.py:61`.) |
| Relógio fora da borda | Não há nova leitura: o vigia fornece o instante deslocado; os defaults existentes só leem o relógio quando falta `agora`. (`app/ponto/services/vigia_de_hora.py:61`; `app/ponto/turnos.py:2102`; `app/ponto/turnos.py:2148`.) |
| Derivação paralela | A recarga não cria regra concorrente: vínculo e pareamento continuam nas mesmas autoridades. (`app/ponto/turnos.py:1965`; `app/ponto/turnos.py:1997`; `app/ponto/turnos.py:2178`.) |
| Selo/RED sem mordida | Há expectativas concretas para cartório/vigia e mutante que remove `mais=` do lote. Execução RED/GREEN/mutante: **[nao verificado]**. (`app/ponto/tests/test_bug_enc_celulas_forma.py:49`; `app/ponto/tests/test_bug_enc_celulas_forma.py:50`; `app/ponto/tests/test_bug_enc_celulas_forma.py:72`; `app/ponto/tests/test_bug_enc_celulas_forma.py:75`; `app/ponto/tests/test_e4_carga_paridade.py:159`; `app/ponto/tests/test_e4_carga_paridade.py:165`.) |
| **ESMERIL-SEMANTICA** | **Permanece:** o mesmo nome representa dois formatos e alimenta leitores diferentes. O diff documenta e interrompe a passagem incompatível; não uniformiza o contrato. (`app/ponto/turnos.py:2076`; `app/ponto/turnos.py:2104`; `app/ponto/turnos.py:2138`; `app/ponto/turnos.py:2155`; `app/ponto/turnos.py:2178`.) |

**6. Paridade entre lote, casca, cartório e vigia**

**Lote versus casca plural:** para mesma janela, snapshot e alimentação completa, não encontrei divergência estática: ambos usam `_dias_encerrados_puro` e obtêm turnos pelo mesmo `_turnos_do_colab_puro`, com pad. Os testes disponíveis cruzam X/Y/Z, duas janelas e três instantes; paridade universal executada é **[nao verificado]**. (`app/ponto/turnos.py:2161`; `app/ponto/turnos.py:1920`; `app/ponto/turnos.py:2989`; `app/ponto/turnos.py:3048`; `app/ponto/turnos.py:3057`; `app/ponto/tests/test_e4_carga_paridade.py:47`; `app/ponto/tests/test_e4_carga_paridade.py:48`; `app/ponto/tests/test_e4_carga_paridade.py:145`.)

**Plural/cartório versus singular/vigia: não há garantia universal.** Há contraexemplo além de X/Y, inferido do código:

- Célula de `c1` aponta para vínculo vigente de `c2`. Isso já existe como cenário de teste: grava-se a geradora alheia e exige-se que `escala_vigente(c1,D)` a devolva. (`app/chamados/tests/test_e3_c8c_orm.py:68`; `app/chamados/tests/test_e3_c8c_orm.py:74`; `app/chamados/tests/test_e3_c8c_orm.py:78`.)
- O singular entrega diretamente essa geradora à juíza; o plural e o lote carregam somente vínculos de `c1`. Assim, no plural a geradora falta, a juíza devolve `None` e o encerramento resulta em `sem_previsto`. (`app/escala/servico_jornada.py:61`; `app/escala/servico_jornada.py:63`; `app/ponto/turnos.py:2154`; `app/ponto/turnos.py:3002`; `app/escala/alimentacao.py:279`; `app/escala/alimentacao.py:281`; `app/ponto/turnos.py:2127`; `app/ponto/turnos.py:2129`.)
- Com template noturno 19–07, nenhuma batida, nenhuma célula de `c2` nesse dia e consulta às 08h de D+1, o singular pode encerrar pelo template, enquanto plural/lote permanecem em `sem_previsto`. (`app/escala/models.py:1122`; `app/escala/models.py:1133`; `app/ponto/turnos.py:2122`; `app/ponto/turnos.py:2130`.)
- No vigia às 09h, teto de 30 minutos desloca a pergunta para 08h; vencidas as guardas, ele pode soltar a célula, embora plural/lote não afirmem encerramento. (`app/ponto/services/cartorio.py:862`; `app/ponto/services/vigia_de_hora.py:47`; `app/ponto/services/vigia_de_hora.py:53`; `app/ponto/services/vigia_de_hora.py:55`; `app/ponto/services/vigia_de_hora.py:61`.)

A assimetria é anterior às três mudanças; execução dessa contraprova e ocorrência em produção são **[nao verificado]**. O teste de candidatos, inclusive, exige que vínculo alheio não entre no balde do colaborador solicitado. (`app/escala/tests/test_o131_um_carregador_de_candidatos.py:150`; `app/escala/tests/test_o131_um_carregador_de_candidatos.py:155`.)

**7. Leis dos sítios**

Não identifiquei violação direta **introduzida pelo diff** das leis de domínio listadas:

| Lei | Conferência |
|---|---|
| L-025 | O pareador continua excluindo retratadas. (`app/docs/LEIS.md:76`; `app/ponto/turnos.py:1869`.) |
| L-032, L-085, L-087 | Continuam ata, seleção por geradora e pareador; não nasce regra própria de intervalo, tipo gravado ou dia civil. (`app/docs/LEIS.md:80`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:119`; `app/ponto/turnos.py:1944`; `app/ponto/turnos.py:1965`; `app/ponto/turnos.py:1997`.) |
| L-102, L-103, L-115 | As alterações tratam entrada/carga, sem redefinir palavra, zero ou saída posicional. Regressão comportamental completa: **[nao verificado]**. (`app/docs/LEIS.md:128`; `app/docs/LEIS.md:129`; `app/docs/LEIS.md:153`; `app/ponto/turnos.py:1865`; `app/ponto/turnos.py:2161`.) |
| L-073 | Permanece o julgamento por aberta viva e fim previsto, com instante fornecido pelo vigia. (`app/docs/LEIS.md:100`; `app/ponto/turnos.py:2122`; `app/ponto/turnos.py:2127`; `app/ponto/services/vigia_de_hora.py:61`.) |

O corte que revogava o nascimento de `dia_encerrado` foi corrigido; o registro vivo mantém essa autoridade. Portanto, a existência desse juiz não reprova o diff. (`app/docs/CORTES.md:302`; `app/core/juizes.py:239`.)

Entretanto, **não considero demonstrado o fechamento integral da classe**: o pad incompleto permanece em leitor alcançado, e a contraprova singular/plural limita a coerência universal exigida pela L-099. RED evidenciado, GREEN e suíte continuam **[nao verificado]**, requisitos que o código dos testes sozinho não comprova. (`app/ponto/services/fechamento.py:605`; `app/ponto/services/fechamento.py:377`; `app/ponto/turnos.py:2105`; `app/ponto/turnos.py:2154`; `app/docs/LEIS.md:122`; `CLAUDE.md:28`; `CLAUDE.md:193`.)

## 15/11 20:23 · sessao 7a047bcec0f8 · - · - · origem=Ronald (tmux)

**5. Classes de reprovação**

- **Segundo juiz:** o diff conserva o juiz registrado `dia_encerrado` e o pareador registrado `turnos_do_colab`; casca e lote chamam os mesmos corpos `_dias_encerrados_puro` e `_turnos_do_colab_puro`. Não aparece regra concorrente nas três alterações fornecidas. (`app/core/juizes.py:212`, `app/core/juizes.py:239`, `app/ponto/turnos.py:1920`, `app/ponto/turnos.py:2161`, `app/ponto/turnos.py:3048`, `app/ponto/turnos.py:3057`.)
- **Segundo escritor:** os trechos alterados fazem validação, carga e delegação; o escritor do veredito continua sendo a porta `ponto.portas.celula.lavrar_veredito`, acessada pelo método do modelo. (`app/ponto/turnos.py:1865`, `app/ponto/turnos.py:2153`, `app/ponto/services/vigia_de_hora.py:61`, `app/escala/models.py:1819`.)
- **Relógio fora da borda:** não há nova leitura do relógio nas alterações; o vigia entrega `agora - 2*teto`; os defaults existentes das cascas leem `timezone.now()` somente quando `agora is None`. (`app/ponto/services/vigia_de_hora.py:61`, `app/ponto/turnos.py:2102`, `app/ponto/turnos.py:2148`.)
- **Derivação paralela:** o diff passa a carregar novamente células para o pareamento, mas continua perguntando às mesmas autoridades de vínculo e geometria; duplicação de leitura não constitui segundo cálculo da regra. (`app/ponto/turnos.py:1891`, `app/ponto/turnos.py:1965`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2178`.)
- **Selo sem mordida:** os testes novos têm expectativas independentes para X/Y e para o vigia; o teste de paridade também exige que retirar `mais=` produza divergência. A existência dessas asserções é verificável; RED anterior, GREEN e execução dos mutantes nesta obra são **[nao verificado]**. (`app/ponto/tests/test_bug_enc_celulas_forma.py:46`, `app/ponto/tests/test_bug_enc_celulas_forma.py:71`, `app/ponto/tests/test_e4_carga_paridade.py:127`, `app/ponto/tests/test_e4_carga_paridade.py:150`.)
- **ESMERIL-SEMANTICA: permanece.** Singular `celulas` é mapa `{data}` destinado somente ao pareador; plural é mapa `{(pk,data)}` destinado ao vínculo e ao DNA do encerramento. O singular nem repassa esse argumento à seleção de escala/fim previsto. O diff documenta a assimetria, mas não unifica o contrato. (`app/ponto/turnos.py:2076`, `app/ponto/turnos.py:2104`, `app/ponto/turnos.py:2138`, `app/ponto/turnos.py:2155`, `app/ponto/turnos.py:2178`.)

**6. Paridade e contraexemplo além de X/Y**

**CargaDaJanela versus casca plural:** para mesmos colaboradores, janela, snapshot, células completas e turnos da mesma janela, o encadeamento agora é equivalente: ambas entregam vínculos/células ao mesmo julgamento; ambas obtêm os turnos pelo mesmo corpo e pad. Não encontrei divergência estática nessas condições. A prova executável disponível cobre X/Y/Z, duas janelas e três instantes; paridade universal executada é **[nao verificado]**. (`app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1920`, `app/ponto/turnos.py:2161`, `app/ponto/turnos.py:2989`, `app/ponto/turnos.py:3048`, `app/ponto/turnos.py:3057`, `app/ponto/tests/test_e4_carga_paridade.py:47`, `app/ponto/tests/test_e4_carga_paridade.py:145`.)

**Cartório/plural versus vigia/singular: não há garantia universal. Contraexemplo inferido do código, não executado:**

1. Célula de `c1` em D aponta para vínculo vigente de `c2`, noturno 19–07; nenhuma batida; hoje D+1, agora 09:00. O modelo declara FK simples para a geradora, com unicidade apenas `(colaborador,data)`; não declara igualdade entre os donos. A classe `CelulaDia` não contém `save`/`clean` próprios. (`app/escala/models.py:1635`, `app/escala/models.py:1692`, `app/escala/models.py:1830`.)
2. Isso já é uma fixture aceita por teste existente: ele grava geradora de outro colaborador e exige que `escala_vigente(c1,D)` devolva o vínculo de `c2`; a base confirma que `ec2` pertence a `c2`. (`app/chamados/tests/test_e3_c8c_orm.py:68`, `app/chamados/tests/test_e3_c8c_orm.py:74`, `app/chamados/tests/test_e3_c8c_orm.py:78`, `app/chamados/tests/test_e2_tarefa_do_dia.py:80`, `app/chamados/tests/test_e2_tarefa_do_dia.py:88`.)
3. Singular obtém a geradora diretamente e passa `[_ger]` à juíza; a juíza valida pk/vigência e não valida o dono. Sem célula de `c2`, os marcos caem no template 19–07. Sem turno aberto, às 08:00 o singular responde encerrado. (`app/escala/servico_jornada.py:49`, `app/escala/servico_jornada.py:61`, `app/escala/alimentacao.py:279`, `app/escala/models.py:1122`, `app/escala/models.py:1133`, `app/ponto/turnos.py:2104`, `app/ponto/turnos.py:2122`, `app/ponto/turnos.py:2127`, `app/ponto/turnos.py:2130`.)
4. Plural e lote carregam somente vínculos de `c1`; a geradora `c2` não está na lista, logo a juíza retorna `None` e o motivo é `sem_previsto`, dia não encerrado. (`app/ponto/turnos.py:2154`, `app/ponto/turnos.py:3002`, `app/ponto/turnos.py:3057`, `app/escala/alimentacao.py:279`, `app/ponto/turnos.py:2127`.)
5. No vigia, fim07 passa pelas guardas de ontem; teto30min desloca a pergunta das09 para08, e o singular encerra, fazendo `_ontem_em_janela` retornar `False`. A leitura equivalente do fechamento plural/lote ainda seria não encerrado. (`app/ponto/services/cartorio.py:862`, `app/ponto/services/vigia_de_hora.py:47`, `app/ponto/services/vigia_de_hora.py:53`, `app/ponto/services/vigia_de_hora.py:55`, `app/ponto/services/vigia_de_hora.py:61`.)

Essa diferença decorre dos carregadores que o diff fornecido não altera; existência desse cadastro em produção e execução da contraprova são **[nao verificado]**. O teste antigo de candidatos, inclusive, exige que `mais=` de outro colaborador não entre no balde do solicitado, em contraste com o carregador singular direto. (`app/escala/tests/test_o131_um_carregador_de_candidatos.py:142`, `app/escala/alimentacao.py:155`, `app/escala/servico_jornada.py:61`.)

**7. Leis**

- **L-025:** o pareador continua excluindo batidas retratadas; a mudança não remove o filtro. (`app/docs/LEIS.md:76`, `app/ponto/turnos.py:1868`, `app/ponto/turnos.py:3029`.)
- **L-032, L-085, L-087:** a correção conserva o pareador e sua alimentação por ata/DNA, a seleção por geradora e a data do turno; não introduz ramo novo para intervalo, tipo gravado ou dia civil. (`app/docs/LEIS.md:80`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:119`, `app/ponto/turnos.py:1944`, `app/ponto/turnos.py:1965`, `app/ponto/turnos.py:1997`.)
- **L-102/L-103/L-115:** os trechos alterados não mudam os contratos de palavra/zero/saída posicional descritos nessas leis; a revisão estática não identifica violação direta delas. Regressão comportamental completa é **[nao verificado]**. (`app/docs/LEIS.md:128`, `app/docs/LEIS.md:129`, `app/docs/LEIS.md:153`, `app/ponto/turnos.py:1865`, `app/ponto/turnos.py:2161`.)
- **Teto temporal/L-073:** permanece a conjunção “aberta viva” + “fim previsto”, consultando as autoridades existentes, e o vigia continua perguntando por instante. (`app/docs/LEIS.md:100`, `app/ponto/turnos.py:2122`, `app/ponto/turnos.py:2127`, `app/ponto/services/vigia_de_hora.py:61`.)
- **Cortes de juízes:** o registro vivo mantém geometria em `turnos_do_colab` e encerramento em `dia_encerrado`; os cortes de `turno_aberto_ha`, cobertura e recusa do admin tratam outras perguntas e não exigem nova autoridade para este diff. (`app/core/juizes.py:212`, `app/core/juizes.py:239`, `app/docs/CORTES.md:303`, `app/docs/CORTES.md:336`, `app/docs/CORTES.md:371`, `app/docs/CORTES.md:381`, `app/docs/CORTES.md:389`.)
- **LEI-AKITA5/RED e anti-vacuidade:** há testes com expectativas concretas e mutante explícito, mas o requisito histórico “RED evidenciado → GREEN → suite” não pode ser certificado pelo material lido; **[nao verificado]**. (`CLAUDE.md:28`, `CLAUDE.md:193`, `CLAUDE.md:194`, `app/ponto/tests/test_bug_enc_celulas_forma.py:49`, `app/ponto/tests/test_e4_carga_paridade.py:150`.)

_5791 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
