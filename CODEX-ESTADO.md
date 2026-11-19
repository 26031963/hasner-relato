# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 08:11** · publicado em **19/11/2026 08:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 08:11 · sessao d110a5090543 · - · - · origem=Ronald (tmux)

Investigação dos leitores e seus chamadores enviada ao agente principal, com referências à base e ressalvas sobre as guardas e a carga em lote.

## 19/11 08:11 · sessao de53552e4dba · - · - · origem=Ronald (tmux)

Evidências da volta 3 e das guardas foram enviadas ao agente principal, incluindo a limitação do K6 às metades de intervalo e o filtro SQL anterior à releitura de `validada_em` ([materializacao.py:804](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:804), [materializacao.py:2104](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2104)).

## 19/11 08:11 · sessao 96fb0bcc2955 · - · - · origem=Ronald (tmux)

Enviado ao agente principal o levantamento da testemunha, da conservação do passivo, dos dois juízes e do corte temporal, com referências à base e casos discriminantes. Também sinalizei que `pergunta_respondida` recebe tanto `LeituraDoAto` quanto objetos temporais sintéticos, e que o corte da projeção atual não prova, por si só, o resultado de um replay histórico.

## 19/11 08:09 · sessao af39d0380fdf · E4C-F5 · verificador · origem=Code

**ETAPA A — AVAL-DEVOLVIDO: não**, para a revisão e correção do brief. As divergências abaixo precisam constar do C3. A etapa não foi executada nem o C3 foi criado, pois esta revisão foi autorizada somente para leitura. A identidade da cópia com o hash informado não pôde ser comprovada por Git: a cópia não contém `.git` [não verificado].

**ETAPA B — AVAL-DEVOLVIDO: sim**, neste momento. O literal exige antes C3 fechado, matriz completa, zero perguntas de lei e aprovação do censo pelo wrapper; esses resultados não estão no dado fornecido [não verificado]. Além disso, a cobertura das três voltas e o alcance das portas de escrita precisam ser demonstrados antes da construção.

## 1. Premissas na cópia

**a) Testemunha: confere.** `DecisaoPergunta.leitura` retorna `LeituraDoAto`, com os três campos `validada_em`, `respondida_em` e `resposta_colab` ([models.py:19](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:19), [models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295), [models.py:4326](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4326)).

**b) As 15 entradas listadas no §2 existem nas posições citadas.** Todas contêm os campos indicados; a última é um `refresh_from_db` que nomeia os campos, e as condições que os usam aparecem nas linhas seguintes ([validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262), [validacao.py:264](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:264)).

- Juízes: [juizes.py:1389](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1389) e [juizes.py:1552](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1552).
- Serviços: [acoes_disputa.py:349](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:349), [declaracao_texto.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:228), [disputa_emissao.py:2083](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:2083) e [fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188).
- Materialização: [materializacao.py:199](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:199), [materializacao.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:201) e [materializacao.py:2123](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2123).
- Validação: [validacao.py:640](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:640), [validacao.py:685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:685), [validacao.py:898](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:898), [validacao.py:1064](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:1064) e [validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262).
- Template: [chamados_tags.py:166](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/templatetags/chamados_tags.py:166).

**c) `leituras`: a premissa literal da assinatura não confere.** A função pública `tarefa_do_dia` aceita `carga`, não `leituras` ([tarefa_do_dia.py:825](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:825)). O kwarg `leituras` existe na função interna `_tarefa_pura`; a carga monta um mapa por pergunta e o repassa ([tarefa_do_dia.py:196](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:196), [tarefa_do_dia.py:425](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:425), [tarefa_do_dia.py:677](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:677)). Isso não demonstra, por si só, o teto de consultas de cada sítio a migrar [não verificado].

**d) Juízes e passivo: existem, com ressalva.** Os dois juízes existem e hoje leem a projeção ([juizes.py:1382](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1382), [juizes.py:1544](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1544)). `DecisaoPergunta.leitura` incorpora o passivo por `linhas_da_projecao_sem_linha` ([models.py:4238](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4238), [models.py:4308](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4308)). A conservação não é identidade literal para qualquer projeção: texto sem `respondida_em` não entra como resposta e `None` é normalizado para texto vazio; texto sem carimbo é classificado como estado ilegal ([models.py:4251](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4251), [models.py:4260](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4260), [juizes.py:1537](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1537)). O corte temporal do primeiro juiz também precisa ser preservado ([juizes.py:1391](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1391)).

**e) Teste: arquivo existe, referência à expectativa não confere literalmente.** As linhas 330 e 334 criam o chamado e aplicam um *mock* de relógio; não são a expectativa do dono. O caso SILÊNCIO espera `silencio_decidido` e compara a tarefa antes e depois, sem um `assert` explícito de dono admin nessas linhas ([test_f1nova_b1_juiz_respondida.py:328](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:328), [test_f1nova_b1_juiz_respondida.py:338](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:338), [test_f1nova_b1_juiz_respondida.py:343](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:343)).

## 2. Colisão com aval e vínculos

Pelos textos raspados, o novo literal conserva as proibições centrais do item 2 da ESPINHA: escrita nova, alteração de motor, folha, exportação, DiaPago, escala ou vínculo, e pouso antes das condições indicadas. A alteração da testemunha corrige uma premissa do texto anterior; sua equivalência com todos os cortes anteriores é **[não verificado]**. [nao verificado]

A proibição de tocar vínculo é compatível, em texto, com as regras existentes de `vinculos.py`: primeiro apontamento prevalece e a porta não apaga vínculo com `None` ([vinculos.py:11](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vinculos.py:11), [vinculos.py:24](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vinculos.py:24)). Efeitos indiretos de todas as portas sobre vínculos são **[não verificado]**.

**MANHA-1911-0810: colisão [não verificado].** Seu conteúdo não consta do dado raspado apresentado nem foi localizado na cópia examinada.

## 3. Condições que não fecham ou exigem prova

- **19 linhas versus 15 sítios:** o §2 fornecido lista 15 posições, mas duas são linhas distintas da mesma função `par_do_intervalo` ([materializacao.py:199](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:199), [materializacao.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:201)). A correspondência das 19 linhas do censo com esses 15 sítios é **[não verificado]**. Além disso, o RED-5 do histórico é mantido pelo literal, embora o histórico esteja fora da lista de 15 posições; a matriz precisa explicitar seu tratamento ([historico.py:96](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/historico.py:96)).
- **Voltas 1/2/3:** a partição completa dos 15 sítios, sem sobra, é **[não verificado]**. `par_do_intervalo`, por exemplo, é compartilhada entre leitor de tela, validação e guarda de plantio; sua classificação não decorre apenas do nome da função ([materializacao.py:181](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:181)).
- **Conta:** 40 + 213 = **253**, não 353. O literal raspado estabelece 253; a existência, composição, eventual sobreposição e abrangência real dessas classes em produção são **[não verificado]**.
- **`models.py`:** a testemunha e o passivo já existem nesse arquivo, mas que algum dos 15 sítios *precise* alterá-lo é **[não verificado]** ([models.py:4238](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4238), [models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295)). O wrapper da cópia contém restrição para escrita em `models.py` ([codex_pergunta.sh:346](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/bin/codex_pergunta.sh:346)).

## 4. Exportada, dinheiro e efeitos de escrita

**Há alcance sobre escrita existente, expressamente relevante para a volta 3.** A condição em `materializacao.py:2123` determina se chama `_materializar_uma`; essa porta pode criar batida ou ausência conforme o tipo e chama a porta de registro de batida ([materializacao.py:2123](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2123), [materializacao.py:2069](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2069), [materializacao.py:1487](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1487)). A guarda de devolução de validação também pode limpar campos e registrar ato ([validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262), [validacao.py:269](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:269)).

A materialização contém uma guarda para competência entregue antes da escrita ([materializacao.py:1866](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1866)). Que **todas** as portas da volta 3 preservem os casos entregues ou exportados, que não haja efeito sobre célula ou dinheiro fora do declarado e que nenhuma classe adicional mude são **[não verificado]**. A medição exigida pelo literal continua necessária; o dado raspado não traz seus resultados.

**perguntas de lei: 0 identificadas nesta revisão.** As pendências acima são de premissa, escopo e evidência; não há, no material disponível, base suficiente para afirmar que exigem uma nova decisão de lei [não verificado].

## 19/11 08:08 · sessao 7e4d5d4d8b08 · - · - · origem=Ronald (tmux)

As 15 entradas listadas no §2 existem nas linhas indicadas e contêm os campos citados: [juizes.py:1389](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1389), [juizes.py:1552](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1552), [acoes_disputa.py:349](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:349), [declaracao_texto.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:228), [disputa_emissao.py:2083](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:2083), [fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188), [materializacao.py:199](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:199), [materializacao.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:201), [materializacao.py:2123](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2123), [validacao.py:640](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:640), [validacao.py:685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:685), [validacao.py:898](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:898), [validacao.py:1064](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:1064), [chamados_tags.py:166](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/templatetags/chamados_tags.py:166) e [validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262). As entradas de `par_do_intervalo` são duas linhas da mesma função; em `validacao.py:262`, a operação é um `refresh_from_db`, seguido de guardas nas linhas 264–269. [materializacao.py:175](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:175), [validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262)

A porta de materialização pode criar batida: [materializacao.py:1487](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1487), [registro_batida.py:176](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/registro_batida.py:176). O `post_save` pode acionar julgamento e escrita da ata da célula, sujeito aos guardas da função: [signals.py:85](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/signals.py:85), [signals.py:131](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/signals.py:131), [cartorio.py:2012](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/services/cartorio.py:2012), [cartorio.py:1987](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/services/cartorio.py:1987).

## 19/11 08:08 · sessao da3a0e5d0a29 · - · - · origem=Ronald (tmux)

Análise enviada ao agente principal, com referências às assinaturas, à carga em lote, às regras de vínculos e à restrição do wrapper. A colisão com MANHA-1911-0810 permanece **[não verificado]**, pois seu conteúdo não foi localizado.

## 19/11 08:08 · sessao 29e5d408b193 · - · - · origem=Ronald (tmux)

Verificação enviada ao agente principal, com referências às linhas da testemunha, dos juízes, da conservação do passivo, do teste e da leitura em lote. [nao verificado]

## 19/11 08:06 · sessao 62c6e0723961 · DST-L2-JZ1 · verificador · origem=Code

**1. Premissas — NÃO CONFERE integralmente.**

- **(a)** Na cópia c4, `_ato_da_retratacao` já chama o escritor extraído. Esse escritor grava `status`, `descricao` **e `atualizado_em`**. O comentário registra que o antigo código inline do Fix A salvava sem `atualizado_em`, mas o Fix A executado pela cópia hoje não faz isso. [reconciliador.py:563](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:563), [justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319).
- **(b) CONFERE.** Aprovar e rejeitar entram em `atomic`, travam a origem antes e fazem `select_for_update` da justificativa pendente. [justificativa.py:140](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:140), [justificativa.py:364](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:364).
- **(c) CONFERE.** Nos dois sítios, o lote recebe os chamados cujo fecho retornou sucesso; a ocorrência é chamada para os que tiveram rejeição. [cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030), [cartorio.py:1570](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1570).
- **(d) CONFERE com limite.** Um `save(update_fields=['status', 'descricao'])` não atualizaria o `auto_now` de `atualizado_em`; o modelo também tem `criado_em` e `aprovada_em`, mas esse save não os altera. O predicado lê os três: portanto a auto-rejeição, por esse save isolado, não acrescentaria um novo carimbo visível a ele. Isso não garante `False` se algum carimbo já estiver na janela ou houver outro fato novo. [models.py:164](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:164), [models.py:179](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:179), [cartorio.py:393](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:393).
- **(e) CONFERE**, com uma precisão: o contador filtra status decidido e a **data já armazenada** em `atualizado_em`. Sem atualizar o campo, a rejeição ainda pode entrar no contador se esse carimbo anterior já for de hoje; não se pode afirmar que ela nunca será contada. [views.py:1738](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/views.py:1738).

**2. Colisão com o aval vigente — PROCEDE.** O literal fornecido manda subir o teto medido e escrever a causa ao lado, mas também proíbe editar o selo JZ1 além do fixture já cortado. Como o teto e sua justificativa estão no arquivo do selo, cumprir ambas as ordens literalmente exige esclarecer a exceção. [não verificado: os anexos transcritos não têm linhas numeradas], [test_selo_chokepoint_escrita.py:196](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_selo_chokepoint_escrita.py:196).

A nova lei fornecida resolve a pergunta anterior de `_APROVANDO` para os dois caminhos; a exclusão já ocorre antes do portão na cópia. A ocorrência continua prevista apenas para quem rejeitou, e o reconciliador já evita repetir a consulta de retenção. [não verificado: anexo sem linhas numeradas], [justificativa.py:299](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:299), [reconciliador.py:538](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:538).

O `atomic` externo proposto **muda a fronteira transacional** para chamadores que hoje chegam sem transação: o `atomic` da transição termina antes da rejeição e ocorrência; com o envoltório, os três efeitos passam a compartilhar o resultado da transação. O efeito concreto sobre tratamento de erros, locks e callbacks de cada chamador está [não verificado]. [fecho_runtime.py:161](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/fecho_runtime.py:161), [cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030).

**3. Condições do literal — PROCEDE a ressalva.** O selo conta comandos SQL capturados durante cenários completos, inclusive `SAVEPOINT`/`RELEASE`, compara o total com um **teto**, e verifica separadamente a quantidade exata de registros `on_commit`. Não mede apenas consultas de dados nem garante, por si, que o teto iguale o valor medido. [test_selo_chokepoint_escrita.py:287](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_selo_chokepoint_escrita.py:287), [test_selo_chokepoint_escrita.py:428](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_selo_chokepoint_escrita.py:428).

Um `atomic` aninhado **pode** acrescentar um par mensurável; o valor e as chaves afetadas dependem do posicionamento e do caminho efetivamente percorrido. Em particular, envolver a construção de `_porm` pode abrir o bloco mesmo sem fecho; o selo tem cenários em que chamados ficam vivos. Delta real, SQL e eventual delta sem fecho: **[não verificado]**. [cartorio.py:1564](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1564), [test_selo_chokepoint_escrita.py:445](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_selo_chokepoint_escrita.py:445).

O `False` pedido para R8 é **alcançável em princípio** com a opção (b), se nenhum outro carimbo ou fato cair na janela; não é consequência universal de omitir `atualizado_em`. O R8 atual apenas imprime o resultado e afirma a estabilidade após o segundo julgamento; não afirma `False`. Sua execução alterada está [não verificado]. [cartorio.py:393](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:393), [test_dst_l2_jz1_c3_lote_portao.py:129](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:129).

**4. Exportada, dinheiro ou `!` fora do declarado — NÃO COMPROVADO.** Os sítios examinados escrevem justificativa e ocorrência; o fechamento financeiro lê justificativas aprovadas, de modo que uma mudança no conjunto aprovado pode ter efeito indireto. Escrita direta nova em exportada, motor, folha, espelho ou `FechamentoMensal`, e impacto efetivo nesses destinos: **[não verificado]**. A equivalência dos sítios na main indicada também está **[não verificado]**. [justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319), [fechamento.py:660](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/fechamento.py:660).

**AVAL-DEVOLVIDO: sim**  
**perguntas de lei: 1** — esclarecer a autorização para editar os tetos e suas justificativas diante da proibição literal de editar o selo. [não verificado: anexo sem linhas numeradas]

## 19/11 03:29 · sessao f04606e63662 · ESCALA-NAO-DESCREVE · sonda · origem=Code

A escrita em `/app/logs/errors.log` fica no `tmpfs` do container e morre com ele: a árvore é montada em `/app` como somente leitura, `/app/logs` recebe um `tmpfs` próprio e o container usa `--rm`. Portanto, essa escrita não chega ao arquivo do host pelo caminho mostrado e não impede a sonda pelo critério corrigido. [bin/sombra.sh:182](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/bin/sombra.sh:182), [bin/sombra.sh:189](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/bin/sombra.sh:189), [app/config/settings/saas.py:136](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/saas.py:136).

A sonda abaixo usa `proposta` da função real como universo, consulta o ciclo atual pela função da tela e mede turnos pela porta solicitada. Uma proposta cujo tipo não aparece no mapa de ciclos é contada como erro, sem presumir que seja não cíclica. [app/ponto/services/esmeril_espelho.py:215](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:215), [app/escala/services/cadastro_realidade.py:819](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:819), [app/ponto/turnos.py:1850](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1850).

```python
import collections
import datetime
import os
import sys

HOJE = datetime.date(2026, 11, 19)

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import TipoEscala
from escala.services.cadastro_realidade import ciclos_dos_destinos
from ponto.services.esmeril_espelho import anomalias_da_frota
from ponto.turnos import turnos_do_colab


INI = HOJE - datetime.timedelta(days=60)
FIM = HOJE - datetime.timedelta(days=1)


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def main():
    universo = None
    erros = 0
    contador = 0
    ciclicas = 0
    nao_ciclicas = 0
    por_confianca = collections.Counter()
    por_empresa = collections.Counter()
    linhas_casos = []
    erros_casos = []
    etapa = "GUARDA_SOMBRA"

    try:
        with transaction.atomic():
            guarda_sombra()

            etapa = "UNIVERSO"
            proposta = anomalias_da_frota(HOJE)["proposta"]
            universo = len(proposta)

            etapa = "CICLOS"
            ciclos = ciclos_dos_destinos(proposta)

            etapa = "CASOS"
            for cid, p in sorted(
                proposta.items(), key=lambda item: int(item[0])
            ):
                try:
                    # O savepoint permite continuar após erro de banco no caso.
                    with transaction.atomic():
                        colab = Colaborador.objects.get(pk=int(cid))
                        empresa = colab.empresa_id
                        tipo_id = p["tipo_escala_id"]
                        codigo = p["codigo"]
                        confianca = p["confianca"]

                        if tipo_id not in ciclos:
                            raise RuntimeError("ciclo_ausente")

                        ciclica = ciclos[tipo_id] in TipoEscala.CICLICOS
                        n_turnos = n_cruzam = n_abertos = 0

                        if not ciclica:
                            turnos = list(turnos_do_colab(colab, INI, FIM))
                            n_turnos = len(turnos)
                            for turno in turnos:
                                if turno.saida is None:
                                    n_abertos += 1
                                if turno.entrada is not None and turno.saida is not None:
                                    entrada = timezone.localtime(
                                        turno.entrada.timestamp
                                    )
                                    saida = timezone.localtime(
                                        turno.saida.timestamp
                                    )
                                    if entrada.date() != saida.date():
                                        n_cruzam += 1

                    por_confianca[str(confianca)] += 1
                    por_empresa[str(empresa)] += 1
                    if ciclica:
                        ciclicas += 1
                    else:
                        nao_ciclicas += 1
                        if n_cruzam > 0:
                            contador += 1
                            linhas_casos.append(
                                "col%d empresa=%s codigo=%s confianca=%s "
                                "n_turnos=%d n_cruzam=%d n_abertos=%d"
                                % (
                                    int(cid), empresa, codigo, confianca,
                                    n_turnos, n_cruzam, n_abertos,
                                )
                            )
                except Exception:
                    erros += 1
                    erros_casos.append("ERRO_CASO col%d" % int(cid))

        print("TOTAL=%d" % universo)
        print("CICLICAS=%d NAO_CICLICAS=%d" % (ciclicas, nao_ciclicas))
        for confianca, total in sorted(por_confianca.items()):
            print("CONFIANCA=%s TOTAL=%d" % (confianca, total))
        for empresa, total in sorted(por_empresa.items()):
            print("EMPRESA=%s TOTAL=%d" % (empresa, total))
        for linha in linhas_casos:
            print(linha)
        for linha in erros_casos:
            print(linha)
        print("CONTADOR=%d UNIVERSO=%d ERROS=%d" % (contador, universo, erros))
        print("FIM_SONDA_NOTURNO")
        return 0 if erros == 0 else 1

    except Exception:
        erros += 1
        print("ERRO_FATAL etapa=%s" % etapa)
        print(
            "CONTADOR=%d UNIVERSO=%s ERROS=%d"
            % (contador, universo if universo is not None else "NAO_VERIFICADO", erros)
        )
        print("FIM_SONDA_NOTURNO")
        return 1


sys.exit(main())
```

As contagens por ciclo, confiança e empresa incluem os casos processados com sucesso; um erro de caso aparece separado e pode tornar essas contagens parciais. Os resultados e a execução da sonda são [não verificado].

perguntas de lei: 0

## 19/11 03:28 · sessao d379f5ba58d0 · ESCALA-NAO-DESCREVE · sonda · origem=Code

Não posso entregar a sonda que chama `anomalias_da_frota`: há um caminho que pode escrever em arquivo de log, e a instrução foi não usar a função se ela escrever qualquer coisa.

A função chama `assinaturas_do_colab`, que pode chamar `get_motor_cct`; se a leitura do parâmetro falhar, `get_motor_cct` registra uma exceção. Na configuração herdada pela sombra, o logger raiz envia erros ao handler de arquivo `/app/logs/errors.log`. A transação `READ ONLY` do molde protege o banco, mas não impede essa escrita em arquivo. [app/ponto/services/esmeril_espelho.py:235](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:235), [app/ponto/services/esmeril_espelho.py:170](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:170), [app/core/regua_cct.py:442](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/regua_cct.py:442), [app/config/settings/sombra.py:27](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/sombra.py:27), [app/config/settings/saas.py:136](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/saas.py:136), [app/config/settings/saas.py:183](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/saas.py:183).

Também há escrita de warning no caso de empate entre aplicações de CCT vigentes; se esse caso ocorre no dado da sombra é [não verificado]. [app/core/regua_cct.py:265](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/regua_cct.py:265), [app/core/regua_cct.py:366](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/regua_cct.py:366).

A janela solicitada confere: o detector recebe `hoje - 60 dias` até `hoje - 1 dia`. A porta `turnos_do_colab` retorna turnos por `data_turno` na janela, mas seu código consulta `Batida` diretamente com `retratada_em__isnull=True`; ela não chama `batidas_apuraveis` nessa carga. [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241), [app/ponto/turnos.py:1850](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1850), [app/ponto/turnos.py:1867](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1867).

Universo, contagens e ocorrência efetiva dos caminhos de log: [não verificado].

perguntas de lei: 0

## 19/11 03:17 · sessao c55e102a4eb1 · DST-L2-JZ1 · verificador · origem=Code

**Veredito: BLOQUEIA.** Há uma corrida que pode sobrescrever uma decisão do DP, uma janela em que o fecho fica sem seus efeitos associados e uma mudança de significado de “fato novo” que o R8 não cobre. A exclusão de `_APROVANDO` também altera o universo literal de D3 e precisa de decisão de lei. [app/ponto/services/justificativa.py:304](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:304), [app/ponto/services/cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030), [app/chamados/models.py:1389](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/models.py:1389).

**Achados**

1. **BLOQUEIA — decisão concorrente pode ser sobrescrita.** Trecho: `filter(..., status='pendente')`, seguido de `list(_qs...)` e depois `j.save(update_fields=['status', ...])`, sem trava da justificativa nem condição de status no update. Entre a leitura e o save automático, o DP pode aprovar a mesma linha; o save automático pode então gravar `rejeitada` sobre a aprovação. A porta do DP usa `atomic`, trava os chamados na ordem e faz `select_for_update()` da justificativa; o escritor novo não compartilha essa proteção. A execução efetiva dessa corrida em banco está [não verificado]. [app/ponto/services/justificativa.py:304](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:304), [app/ponto/services/justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319), [app/ponto/services/justificativa.py:140](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:140), [app/ponto/services/justificativa.py:364](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:364).

2. **BLOQUEIA — fecho, rejeição e ocorrência não formam necessariamente uma transação.** Trecho: `_fech = [... transicionar_com_pauta(...)]` e, só depois, `rejeitar_justificativas_dos_chamados(...)` e `_ocorrencia_da_rejeicao_no_fecho(...)`; o mesmo ocorre em `_porm`. O `atomic` da porta de transição termina ao retornar, antes dos dois novos efeitos. Quando não houver transação externa, uma falha posterior pode deixar chamado fechado com justificativa pendente, ou justificativa rejeitada sem ocorrência D5. O reconciliador, em contraste, envolve seu ato na transação de S128. Quais entradas reais fornecem uma transação externa em todos os casos estão [não verificado]. [app/ponto/services/cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030), [app/ponto/services/cartorio.py:1570](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1570), [app/chamados/services/fecho_runtime.py:161](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/fecho_runtime.py:161), [app/chamados/reconciliador.py:543](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:543).

3. **BLOQUEIA — a rejeição automática passa a ser fato novo para outro chamado do mesmo dia.** Trecho: o novo save inclui `atualizado_em`, e `houve_fato_novo` aceita qualquer justificativa do colaborador e dia com esse carimbo em `(desde, ate]`. Se um irmão foi encerrado via admin antes dessa rejeição e os demais fatos não satisfaziam o predicado, sua `recusa_da_reabertura` deixa de retornar `admin_sem_fato_novo`; outras guardas ainda podem recusar a reabertura. O teste R8 verifica o chamado encerrado pelo sistema e só imprime `fato`, sem afirmar seu valor nem criar o irmão administrativo. Portanto não prova a ausência desse efeito. [app/ponto/services/justificativa.py:322](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:322), [app/ponto/services/cartorio.py:393](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:393), [app/ponto/services/cartorio.py:405](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:405), [app/chamados/models.py:1389](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/models.py:1389), [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:146](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:146).

4. **BLOQUEIA — pergunta de lei: `_APROVANDO` foi excluída de D3 mesmo com portão aberto.** Trecho: `_qs.exclude(pk=_APROVANDO.get())` ocorre **antes** do `if portao_de_retencao`, de modo que vale também no reconciliador, que chama com `False`. Se o ponteiro é a própria J em aprovação, o predicado de retenção não responde `justificativa_alheia`; ainda assim a J pendente apontada é excluída. Isso contraria o universo literal de D3 fornecido no enunciado; não é consequência da condição R7, que fala de retenção respondendo reter. O teste C4 demonstra a razão prática pretendida — evitar rejeição transitória e ocorrência falsa —, mas a autorização dessa exceção ao universo de D3 está **[não verificado]**. Pergunta: a exclusão deve valer nos dois caminhos, inclusive no reconciliador? [app/ponto/services/justificativa.py:303](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:303), [app/chamados/reconciliador.py:452](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:452), [app/chamados/reconciliador.py:574](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:574), [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:343](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:343).

5. **BLOQUEIA — R9 não sela o leitor até o ESTADO.** Trecho: o teste verifica a chave no carimbo e a saída do comando, mas não executa `fio_estado.linha` nem o gerador do ESTADO. Apagar ou quebrar o leitor final pode deixar esse teste verde. O leitor existe no código: compara genericamente as chaves do carimbo e mostra apenas as fora do esperado; o gerador o chama. Os logs RED/verde, mutantes por sítio e medições antes/depois exigidos no dado fornecido estão **[não verificado]**; a árvore examinada não é um repositório Git, portanto o hash e o diff também não foram confirmados independentemente. [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:273](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:273), [bin/fio_estado.py:121](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/fio_estado.py:121), [bin/fio_estado.py:191](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/fio_estado.py:191), [bin/gerar_estado.py:260](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/gerar_estado.py:260).

**Campos e efeitos comprovados**

| Campo ou dado | Quem escreve e quem lê, nos caminhos examinados |
|---|---|
| `Justificativa.status` | A função nova grava `rejeitada`; aprovação e rejeição do DP também escrevem status. O próprio lote e D4 filtram `pendente`; API e painel leem o estado. Há outros escritores de rejeição existentes: sua conformidade com a porta aplicável está [não verificado]. [justificativa.py:304](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:304), [justificativa.py:205](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:205), [justificativa.py:374](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:374), [api/views.py:2105](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:2105), [ponto/views.py:1739](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/views.py:1739). |
| `descricao` | O colaborador a fornece pela API; a rejeição automática acrescenta motivo ao mesmo texto; a API devolve o campo ao colaborador. Assim, descrição de entrada e anotação do sistema compartilham o campo. [api/views.py:1994](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:1994), [justificativa.py:323](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:323), [api/views.py:2107](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:2107). |
| `atualizado_em` | `auto_now`, incluído explicitamente no save novo; o painel usa a **data** para decisões “hoje” e `houve_fato_novo` usa o **instante** como mudança de insumo. A configuração usa `USE_TZ=True` e fuso `America/Sao_Paulo`. [ponto/models.py:179](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:179), [justificativa.py:324](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:324), [ponto/views.py:1741](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/views.py:1741), [cartorio.py:405](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:405), [base.py:93](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/config/settings/base.py:93). |
| Ponteiro e `chamado_origem` | `apontar_justificativa` escreve o ponteiro; a API pode escrever a origem ao criar ou completar a justificativa. O lote lê a união das duas relações; D4 também lê ambas. Outros escritores e leitores completos estão [não verificado]. [vinculos.py:81](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/vinculos.py:81), [api/views.py:1989](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:1989), [justificativa.py:294](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:294), [justificativa.py:342](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:342). |
| `criado_em` e `resolvido_em` | O primeiro nasce com `auto_now_add`; a transição terminal carimba o segundo e a reabertura o limpa. D4 compara **instantes**, com `criado_em > resolvido_em` estrito: igualdade não conta. Sua unidade é **pares justificativa × chamado**, deduplicados entre ponteiro e origem, não necessariamente justificativas distintas. [ponto/models.py:179](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:179), [chamados/models.py:243](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/models.py:243), [justificativa.py:330](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:330). |
| Ocorrência D5 | O cartório chama `anexar_ocorrencia` após rejeição, passando conteúdo, tipo e IDs; a porta grava `FollowupChamado` e deriva chave por chamado, tipo, referência e autor. O teste lê ocorrências por conteúdo. Outros consumidores do follow-up estão [não verificado]. [cartorio.py:47](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:47), [nota.py:62](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/nota.py:62), [nota.py:88](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/nota.py:88), [test_dst_l2_jz1_c3_lote_portao.py:43](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:43). |
| Chave nova do vigia | O vigia chama D4, grava o valor no `censo` e zero em `esperado`; `fio_estado` compara os dois e só exibe a chave quando divergem. O cálculo é de frota, inclusive quando o comando imprime em recorte de empresa; o carimbo é escrito apenas para frota. [vigiar_fio_morto.py:103](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/management/commands/vigiar_fio_morto.py:103), [vigiar_fio_morto.py:190](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/management/commands/vigiar_fio_morto.py:190), [vigiar_fio_morto.py:243](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/management/commands/vigiar_fio_morto.py:243), [bin/fio_estado.py:124](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/fio_estado.py:124). |

**Dinheiro e restante do checklist.** A alteração nova grava status, descrição, carimbo e ocorrência; não encontrei, nesses trechos, escrita direta em motor, folha, espelho, `DiaPago` ou export. Isso não prova ausência de efeito indireto: fechamento lê justificativas **aprovadas**, e a corrida do achado 1 pode alterar esse conjunto. Impacto efetivo em dinheiro ou export está [não verificado]. [justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319), [fechamento.py:660](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/fechamento.py:660).

O `post_save` sai por `em_julgamento` dentro do cartório; fora dele chama `julgar_celula`. Os testes declaram os dois cenários, mas sua execução e a morte dos mutantes estão [não verificado]. Não demonstrei segundo juiz registrado, relógio novo fora da borda ou pouso realizado; esses pontos permanecem [não verificado]. [ponto/signals.py:85](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/signals.py:85), [ponto/signals.py:161](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/signals.py:161), [cartorio.py:992](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:992).

BLOQUEIA: 5 | FILA: 0 | perguntas de lei: 1

## 19/11 03:12 · sessao da34ab3038c3 · ESCALA-NAO-DESCREVE · sonda · origem=Code

A medição pedida **não pode ser concluída com a janela exata a partir da lavra descrita pelo código**: a geração aceita um `hoje` fornecido pelo chamador, mas a proposta guardada não registra esse valor nem os limites da janela. `lavrado_em` registra o relógio ao fim da geração; usá-lo como `hoje` exigiria uma hipótese. [app/ponto/services/esmeril_espelho.py:215](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:215), [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241), [app/ponto/services/esmeril_espelho.py:246](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:246), [app/ponto/services/esmeril_espelho.py:253](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:253).

**A. Código**

1. **Gravação e leitura.** `lavrar` chama `gravar_lavra(d)` e também escreve um espelho JSON; o banco é a autoridade. O caminho padrão do arquivo é `logs/esmeril_espelho.json`. [app/ponto/services/esmeril_espelho.py:28](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:28), [app/ponto/services/esmeril_espelho.py:321](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:321).

   Trechos literais da gravação:

   ```python
   CHAVE_LAVRA = 'esmeril_espelho'
   ```

   ```python
   MetricaSnapshot.objects.update_or_create(
       chave=CHAVE_LAVRA, escopo='frota', escopo_id=None, data_ref=hoje,
       ...
       'valor': None, 'detalhe': d or {}})
   ```

   O campo é `MetricaSnapshot.detalhe`, um `JSONField`; `ler_lavra()` busca o snapshot mais recente por `data_ref` e, na sombra, verifica o marcador antes de devolvê-lo. [app/inteligencia/models.py:81](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/inteligencia/models.py:81), [app/inteligencia/models.py:95](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/inteligencia/models.py:95), [app/ponto/services/esmeril_espelho.py:291](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:291), [app/ponto/services/esmeril_espelho.py:359](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:359).

   Trecho literal do mapa, com chave igual ao PK do colaborador convertido em string:

   ```python
   proposta[str(c.pk)] = {'tipo_escala_id': p['tipo_escala_id'], 'codigo': p['codigo'],
                          'confianca': r['confianca']}
   ```

   O mapa é incluído no retrato sob a chave `proposta`; só recebe resultados com proposta, para colaboradores com assinatura. [app/ponto/services/esmeril_espelho.py:239](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:239), [app/ponto/services/esmeril_espelho.py:253](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:253).

2. **Dia usado pelo detector.** Ele consulta `batidas_apuraveis` e agrupa cada batida pela data civil local do seu próprio instante:

   ```python
   for b in batidas_apuraveis(colab, de, ate):
       lt = _tz.localtime(b.timestamp)
       por_dia[lt.date()].append(lt)
   ```

   Assim, uma batida após meia-noite cai no dia civil seguinte, não necessariamente no dia do turno; o próprio módulo declara a possibilidade de inflar `dias` e deslocar a entrada modal. [app/escala/detector_proposta.py:18](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/detector_proposta.py:18), [app/escala/detector_proposta.py:49](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/detector_proposta.py:49).

3. **Cíclica e não cíclica.** A lavra não guarda o ciclo. A tela consulta o `tipo_ciclo` **atual** do `TipoEscala` pelo ID da proposta. Sua constante é literalmente:

   ```python
   CICLICOS = ('12x36', '24x48')
   ```

   O predicado retorna `'ciclico'` para esses dois valores e `'aplicar'` para os demais que passem pelas outras condições; um template apagado não entra no mapa. A classificação histórica no momento da lavra é [não verificado]. [app/escala/models.py:386](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/models.py:386), [app/escala/services/cadastro_realidade.py:819](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:819), [app/escala/services/cadastro_realidade.py:830](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:830).

4. **Confiança e botão.** O campo é `confianca`. Para proposta existente, o detector atribui `alta` ou `media`; `insuficiente` acompanha `proposta: None`. [app/escala/detector_proposta.py:118](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/detector_proposta.py:118).

   Trechos literais:

   ```python
   if n >= MIN_DIAS_ALTA and coerente and estavel:
       conf, pq = 'alta', ...
   ...
       conf, pq = 'media', '; '.join(_faltas)
   ```

   O predicado do botão não consulta `confianca`; a view a lê para compor o motivo passado à porta de gravação:

   ```python
   if botao_da_proposta(v, proposta, sig, ciclos_dos_destinos({'p': proposta})) != 'aplicar':
   ```

   ```python
   motivo='proposta do esmeril (lavra de %s): %s, confianca %s' % (
       lavra.get('lavrado_em') or '?', proposta.get('codigo') or '?', proposta.get('confianca') or '?'),
   ```

   [app/escala/services/cadastro_realidade.py:830](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:830), [app/escala/views.py:493](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:493).

**B. Sonda parcial, não executada**

A sonda abaixo lê o universo da lavra pela função real, classifica pelo ciclo atual e conta confiança e empresa. Para cada proposta não cíclica, registra erro: sem os limites comprovados não chama o pareador para produzir uma contagem que poderia pertencer a outra janela. Portanto, **não entrega a lista de expostos nem um resultado noturno utilizável quando houver propostas não cíclicas**. O dado raspado fornecido não informa o conteúdo da lavra nem os limites efetivos: [não verificado]. A ausência de lavra no banco resulta em `UNIVERSO=0` com motivo, conforme `ler_lavra()`; falha da guarda ou do marcador resulta em erro. [app/ponto/services/esmeril_espelho.py:359](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:359).

```python
import collections
import os
import sys

from django.conf import settings
from django.db import connection, transaction

from colaboradores.models import Colaborador
from escala.models import TipoEscala
from escala.services.cadastro_realidade import ciclos_dos_destinos
from ponto.services.esmeril_espelho import ler_lavra


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def main():
    universo = 0
    expostos = 0
    erros = 0
    etapa = "GUARDA_OU_LAVRA"
    classes = collections.Counter()
    confiancas = collections.Counter()
    empresas = collections.Counter()
    linhas = []

    try:
        with transaction.atomic():
            guarda_sombra()
            lavra = ler_lavra()

            if lavra is None:
                linhas.append("MOTIVO=SEM_LAVRA_VALIDA_NO_BANCO")
            else:
                etapa = "MAPA_PROPOSTA"
                if "proposta" not in lavra:
                    linhas.append("MOTIVO=CAMPO_PROPOSTA_AUSENTE_NA_LAVRA")
                else:
                    propostas = lavra["proposta"]
                    if not isinstance(propostas, dict):
                        raise ValueError("mapa_proposta")
                    universo = len(propostas)

                    etapa = "CICLOS_ATUAIS"
                    # Uma proposta malformada não deve impedir os demais casos.
                    validas = {
                        chave: p
                        for chave, p in propostas.items()
                        if isinstance(p, dict)
                    }
                    ciclos = ciclos_dos_destinos(validas)

                    for chave, proposta in propostas.items():
                        cid = None
                        try:
                            # Savepoint permite seguir após erro de consulta por caso.
                            with transaction.atomic():
                                if not isinstance(chave, str):
                                    raise ValueError("chave")
                                cid = int(chave)
                                if cid <= 0 or str(cid) != chave:
                                    raise ValueError("chave")
                                if not isinstance(proposta, dict):
                                    raise ValueError("proposta")

                                tipo_id = proposta.get("tipo_escala_id")
                                if tipo_id not in ciclos:
                                    raise ValueError("tipo_ausente")
                                ciclo = ciclos[tipo_id]
                                if not isinstance(ciclo, str):
                                    raise ValueError("ciclo")

                                colab = Colaborador.objects.only(
                                    "id", "empresa_id"
                                ).get(pk=cid)
                                empresa = colab.empresa_id
                                if empresa is None:
                                    raise ValueError("empresa_ausente")

                                conf = proposta.get("confianca")
                                if conf not in ("alta", "media", "baixa",
                                               "insuficiente"):
                                    raise ValueError("confianca")

                                classe = (
                                    "ciclica"
                                    if ciclo in TipoEscala.CICLICOS
                                    else "nao_ciclica"
                                )

                            classes[classe] += 1
                            confiancas[conf] += 1
                            empresas[empresa] += 1

                            if classe == "nao_ciclica":
                                erros += 1
                                linhas.append(
                                    "ERRO col%d empresa=%s "
                                    "motivo=JANELA_DO_DETECTOR_NAO_COMPROVADA"
                                    % (cid, empresa)
                                )
                        except Exception:
                            erros += 1
                            if cid is not None and cid > 0:
                                linhas.append(
                                    "ERRO col%d motivo=CASO_NAO_MEDIDO" % cid
                                )
                            else:
                                linhas.append("ERRO motivo=CHAVE_INVALIDA")

                    linhas.append(
                        "CLASSIFICADAS ciclicas=%d nao_ciclicas=%d"
                        % (classes["ciclica"], classes["nao_ciclica"])
                    )
                    for conf, total in sorted(confiancas.items()):
                        linhas.append(
                            "CONFIANCA %s=%d" % (conf, total)
                        )
                    for empresa, total in sorted(
                        empresas.items(), key=lambda item: str(item[0])
                    ):
                        linhas.append(
                            "EMPRESA %s propostas=%d" % (empresa, total)
                        )
                    linhas.append(
                        "EXPOSTOS=NAO_MEDIDO "
                        "motivo=JANELA_DO_DETECTOR_NAO_COMPROVADA"
                    )
    except Exception:
        erros += 1
        linhas.append("ERRO etapa=" + etapa)

    for linha in linhas:
        print(linha)
    print("CONTADOR=%d UNIVERSO=%d ERROS=%d" % (expostos, universo, erros))
    print("FIM_SONDA_NOTURNO")
    return 0 if erros == 0 else 1


sys.exit(main())
```

Para obter o número solicitado é preciso comprovar, por evidência externa à proposta guardada, qual foi o `hoje` passado à geração daquela lavra; a identificação desse valor é [não verificado]. [app/ponto/services/esmeril_espelho.py:215](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:215), [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241).

perguntas de lei: 1

## 19/11 02:25 · sessao 71601a41dd9b · ESCALA-NAO-DESCREVE · verificador · origem=Code

**Não considero a árvore pronta para o opus final.** Há dois bloqueios identificáveis pela leitura: a proposta passa a ser guardada apesar da L-042, e a nova ação permite aplicar uma proposta calculada por dia civil mesmo no contraexemplo noturno que o próprio detector declara problemático. [app/docs/LEIS.md:89] [app/ponto/services/esmeril_espelho.py:241] [app/escala/detector_proposta.py:18] [app/escala/views.py:502]

1. **Quem escreve e quem lê.**

   - `ata.orfas` é lido de cada `CelulaDia`; `assinaturas_do_colab` soma seus comprimentos em `medida['orfas']`. `anomalias_da_frota` escreve os mapas `orfas` e `orfas_desde`, este último com o início da janela usada na medição. `lavrar` entrega o resultado a `gravar_lavra`, que grava o snapshot; `ler_lavra` lê esse snapshot. [app/ponto/services/esmeril_espelho.py:99] [app/ponto/services/esmeril_espelho.py:130] [app/ponto/services/esmeril_espelho.py:233] [app/ponto/services/esmeril_espelho.py:321] [app/ponto/services/esmeril_espelho.py:342] [app/ponto/services/esmeril_espelho.py:359]

   - `lista` lê esses mapas e escreve `carga`, `carga_desde` e `carga_texto` nas linhas; `resumo_da_carga` lê as linhas e produz a frase e os totais. Tela e PDF leem `linhas` e `carga_resumo`; os leitores anteriores ainda podem ler `postos`. A carga é contagem de órfãs, e a data é o início da janela, não a data da primeira órfã. [app/escala/services/cadastro_realidade.py:784] [app/escala/services/cadastro_realidade.py:969] [app/escala/services/cadastro_realidade.py:981] [app/escala/services/cadastro_realidade.py:1053] [app/templates/escala/cadastro_x_realidade.html:17] [app/escala/services/cadastro_realidade.py:1183] [app/escala/services/cadastro_realidade.py:1149]

   - `propor_escala` produz a proposta e a confiança; `anomalias_da_frota` grava tanto a frase `destino` quanto `proposta` estruturada. `lista` e `aplicar_proposta_da_lista` leem a proposta da lavra. O ciclo é relido de `TipoEscala` por `ciclos_dos_destinos`; `_vinculos` lê o vínculo de hoje usando as autoridades de cobertura e vínculo do dia registradas em `JUIZES.md`. [app/escala/detector_proposta.py:118] [app/ponto/services/esmeril_espelho.py:241] [app/escala/services/cadastro_realidade.py:753] [app/escala/services/cadastro_realidade.py:819] [app/escala/views.py:493] [app/docs/JUIZES.md:142]

   - `botao_da_proposta` lê vínculo, assinatura, proposta e ciclos; a tela lê seu resultado, e a view o consulta novamente antes do POST. A view passa destino, `hoje` e `motivo` a `realocar_vinculo_para_destino`, que chama `julgar_vinculo` e `executar_vinculo`; `confirm_txt` chega à observação do histórico pela porta de vínculo. [app/escala/services/cadastro_realidade.py:830] [app/templates/escala/cadastro_x_realidade.html:32] [app/escala/views.py:496] [app/escala/regua_defesa.py:315] [app/colaboradores/services/vinculo.py:146] [app/colaboradores/services/vinculo.py:355]

2. **Catraca: bate.** Comparei em memória o literal `LINHAS_REGISTRADAS` com os itens de `censo('app')`, usando exatamente a chave e as linhas cobradas pelo teste: 9 identidades de pareadores e 85 de derivações, sem divergências. O medidor registra a linha AST da chamada reconhecida, não a linha da definição. Em particular, `entradas_fora_do_inicio/B1` é `[524]` e `assinaturas_do_colab/B1` é `[179, 186]`. Isso verifica a catraca estática, não a semântica da jornada. [app/core/tests/test_medida_turno_do_par.py:558] [app/core/medida_turno_do_par.py:197] [app/escala/regua_defesa.py:524] [app/ponto/services/esmeril_espelho.py:179] [app/ponto/services/esmeril_espelho.py:186] [nao verificado]

3. **Delta da catraca contra cada pai:** nos dois diffs fornecidos, somente `app/core/tests/test_medida_turno_do_par.py` muda; nenhum `.py` de produção muda **nesse delta**. A cópia não contém `.git`, portanto a comparação dos pais foi feita pelos anexos fornecidos. [app/core/tests/test_medida_turno_do_par.py:247] [não verificado]

4. **Classes de reprovação.**

   - **BLOQUEIA 1 — proposta guardada:** a L-042 diz que a proposta é recalculada e não guardada; a obra passa a gravar `proposta` estruturada na lavra e a ação posterior a lê. O teste novo exige essa gravação, portanto não é apenas comentário ou intenção. [app/docs/LEIS.md:89] [app/ponto/services/esmeril_espelho.py:243] [app/escala/views.py:493] [app/escala/tests/test_escala_nao_descreve.py:100] [nao verificado]

   - A decisão de destino cíclico aparece tanto no predicado do botão quanto na porta de realocação. É duplicação de guarda observável; não identifiquei em `JUIZES.md` um juiz registrado para essa pergunta que permita classificá-la, por si só, como segundo juiz registrado. [app/escala/services/cadastro_realidade.py:849] [app/escala/regua_defesa.py:313] [app/docs/JUIZES.md:142]

   - A escrita do vínculo encontrada passa por `julgar_vinculo` e `executar_vinculo`; a nova view lê `hoje` e o passa adiante, embora a função de realocação conserve leitura própria para seus outros chamadores. Segundo escritor fora da porta, relógio novo fora da borda, derivação paralela de fonte única ou selo sem mordida: **presença não demonstrada nesta verificação**. [app/escala/views.py:496] [app/escala/regua_defesa.py:315] [app/escala/regua_defesa.py:319] [não verificado]

5. **Contraexemplo de integração — BLOQUEIA 2.** Considere seis turnos noturnos consecutivos de 21h a 5h. O detector já existente agrupa por data civil: as saídas de madrugada e entradas da noite podem formar dias com primeira batida às 5h e última às 21h. Se houver template não cíclico compatível com esses horários, ele pode ser proposto com confiança média; a nova ação aceita proposta não cíclica sem exigir confiança alta e pode gravar esse template a partir de hoje. O próprio detector documenta que o agrupamento civil desloca a entrada modal no noturno e afirma que nada é escrito a partir dele; a ação introduzida pela obra altera essa consequência. É um contraexemplo deduzido do código, não uma ocorrência de produção observada. [app/escala/detector_proposta.py:18] [app/escala/detector_proposta.py:49] [app/escala/detector_proposta.py:95] [app/escala/detector_proposta.py:139] [app/escala/services/cadastro_realidade.py:830] [app/escala/views.py:502]

Execução dos testes Django e ocorrência dos casos em produção: [não verificado]. [nao verificado]

BLOQUEIA: 2  
perguntas de lei: 0

## 19/11 02:20 · sessao 2836601fc984 · CHECK-DE-TELA · sonda · origem=Code

A sonda abaixo é **parcial e não foi executada**: a referência, os tempos, as consultas e a correspondência da cópia ao hash são [não verificado]. A cópia não contém metadados Git verificáveis. [não verificado]

Há dois limites que a sonda declara como erro:

- Não foi comprovada uma rota de tela para `tarefa_do_dia`; a função encontrada recebe colaborador, dia do turno e instante. A sonda não a apresenta como uma tela medida. [app/chamados/services/tarefa_do_dia.py:738]
- No placar sem filtro, se houver retrato lavrado, não foi comprovado o tamanho do universo que produziu aquele retrato. Nesse caso, a carga **b** é declarada não medida; a sonda não troca esse universo por uma contagem atual. [app/chamados/views.py:840]

A porta informada também merece uma correção: o código de `--rodar` usa a rede Docker `saas-hasner_default`, com proxies apontados para a porta local 9; ele não declara `--network none`. Antes de executar a sonda, `--rodar` chama `lavra_de_prod`; o script informa que esse acesso a produção é de leitura. [bin/sombra.sh:60] [bin/sombra.sh:183] [bin/sombra.sh:486]

Rastreio das medições propostas:

1. **Espelho em tela:** `/ponto/espelho/` → `espelho_ponto` → `espelho_do_colab`. **PDF:** `/relatorios/espelho/avulso/` → `espelho_pdf_avulso` → `gerar_pdf_espelho`; o GET abre o formulário e a geração é chamada pelo ramo POST, por isso a sonda mede diretamente o serviço, sem POST. [app/config/urls.py:29] [app/ponto/urls.py:10] [app/ponto/views.py:96] [app/ponto/views.py:160] [app/relatorios/urls.py:28] [app/relatorios/views.py:1050] [app/relatorios/views.py:1126] [app/relatorios/views.py:1145] [app/relatorios/services.py:49]
2. **Tarefa do dia:** rota e view [não verificado]; serviço `tarefa_do_dia`. [app/chamados/services/tarefa_do_dia.py:738]
3. **Central:** `/chamados/` → `painel_gestao` → `montar_contexto_painel` e `pendentes_na_central`. [app/config/urls.py:28] [app/chamados/urls.py:9] [app/chamados/views.py:721] [app/chamados/views.py:726] [app/chamados/views.py:759]
4. **Placar:** `/chamados/placar/` → `placar_json` → `etag_do_placar` e, conforme o ramo, `PlacarNavbar` ou `montar_contexto_painel`. [app/chamados/urls.py:24] [app/chamados/views.py:771] [app/chamados/views.py:812] [app/chamados/views.py:840]
5. **App:** `/api/me/` → `api_me` → leitores incluindo `turno_aberto_de` e `projecao_do_app`. Cada resposta trata de um colaborador. [app/config/urls.py:37] [app/api/urls.py:64] [app/api/views.py:284] [app/api/views.py:306] [app/api/views.py:315] [app/api/views.py:336]

A seleção inclui todos os colaboradores cadastrados, inclusive os sem batidas, e usa a janela corrente de cada empresa; em empate, a mediana inferior e o primeiro máximo por PK. Se o colaborador escolhido não tiver usuário ativo, a carga falha explicitamente. Para o app, ambas as chamadas têm universo de um colaborador: a segunda usa o mesmo máximo de batidas escolhido para o espelho, sem afirmar que seja o usuário mais custoso do app. [app/ponto/models.py:22] [app/ponto/janelas.py:60] [app/colaboradores/models.py:351] [app/api/views.py:336]

O `RequestFactory` recebe usuário, tenant, sessão e mensagens; as views identificadas leem `request.user`, e o processador de contexto também o lê. Outros atributos de middleware necessários em todos os caminhos são [não verificado]. [app/ponto/views.py:113] [app/chamados/views.py:723] [app/api/views.py:286] [app/core/context_processors.py:7]

```python
# Sonda para exec(open(script).read()), em tenant_command shell --schema=juliani.
# Nao imprime respostas, nomes, CPF, SQL nem mensagens de excecao.

import contextlib
import io
import logging
import re
from time import perf_counter

from django.conf import settings
from django.contrib.auth import get_user_model
from django.contrib.messages.storage.fallback import FallbackStorage
from django.contrib.sessions.middleware import SessionMiddleware
from django.db import connection, transaction
from django.db.models import Count, Q
from django.test import RequestFactory
from django.test.utils import CaptureQueriesContext
from django.utils import timezone
from rest_framework.test import force_authenticate

from colaboradores.models import Colaborador
from ponto.janelas import janela_atual
from ponto.views import espelho_ponto
from relatorios.services import gerar_pdf_espelho
from chamados.views import painel_gestao, placar_json
from chamados.services.painel import montar_contexto_painel, qs_vivos
from core.models import PlacarNavbar
from api.views import api_me


class DesfazerSonda(Exception):
    pass


class NaoMedida(Exception):
    pass


erros = 0
fabrica = RequestFactory()
nivel_logging_anterior = logging.root.manager.disable
logging.disable(logging.CRITICAL)


def guardar_sombra():
    cfg = settings.DATABASES["default"]
    if cfg.get("NAME") != "sombra" or cfg.get("HOST") != "sombra_db":
        raise NaoMedida("configuracao fora da sombra")
    if getattr(connection, "schema_name", None) != "juliani":
        raise NaoMedida("schema diferente de juliani")
    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        banco, schema = cursor.fetchone()
    if banco != "sombra" or schema != "juliani":
        raise NaoMedida("banco ou schema inesperado")


def calcular_selecao():
    """Uma contagem por colaborador na janela corrente da sua empresa."""
    guardar_sombra()
    hoje = timezone.localdate()
    linhas = []
    empresas = list(
        Colaborador.objects.order_by()
        .values_list("empresa_id", flat=True)
        .distinct()
    )
    for empresa_id in empresas:
        inicio, fim = janela_atual(hoje, empresa_id)
        qs = (
            Colaborador.objects.filter(empresa_id=empresa_id)
            .select_related("empresa", "usuario")
            .annotate(
                n_batidas=Count(
                    "batidas",
                    filter=Q(
                        batidas__timestamp__date__gte=inicio,
                        batidas__timestamp__date__lte=fim,
                    ),
                )
            )
        )
        linhas.extend(qs)
    if not linhas:
        raise NaoMedida("nenhum colaborador cadastrado")

    linhas.sort(key=lambda c: (c.n_batidas, c.pk))
    mediana = linhas[(len(linhas) - 1) // 2]
    maior_contagem = linhas[-1].n_batidas
    maior = next(c for c in linhas if c.n_batidas == maior_contagem)
    return mediana, maior, len(linhas)


selecao_cache = None


def selecao():
    global selecao_cache
    if selecao_cache is None:
        selecao_cache = calcular_selecao()
        mediana, maior, total = selecao_cache
        print(
            "SELECAO universo_colaboradores=%d mediana=col%d batidas=%d "
            "maximo=col%d batidas=%d"
            % (total, mediana.pk, mediana.n_batidas, maior.pk, maior.n_batidas)
        )
    return selecao_cache


def usuario_colab(colab):
    if not colab.usuario_id or not colab.usuario.is_active:
        raise NaoMedida("colaborador escolhido sem usuario ativo")
    return colab.usuario


def usuario_gestor():
    # Staff satisfaz _is_gestor e tambem a porta do PDF avulso.
    user = get_user_model().objects.filter(
        is_active=True, is_staff=True
    ).order_by("pk").first()
    if user is None:
        raise NaoMedida("nenhum usuario staff ativo")
    return user


def pedido(caminho, user, parametros=None, drf=False):
    req = fabrica.get(caminho, data=parametros or {})
    req.user = user
    req.tenant = connection.tenant
    SessionMiddleware(lambda request: None).process_request(req)
    req._messages = FallbackStorage(req)
    if drf:
        force_authenticate(req, user=user)
    return req


def executar_revertendo(funcao, medir=False):
    """
    A funcao, incluindo renderizacao e avaliacao pedida pelo chamador, roda
    dentro de atomic. O rollback e provocado pela excecao propria.
    """
    resultado = None
    consultas = escritas = 0
    ms = 0.0
    try:
        with transaction.atomic():
            if medir:
                with CaptureQueriesContext(connection) as capturadas:
                    inicio = perf_counter()
                    resultado = funcao()
                    if hasattr(resultado, "render") and not getattr(
                        resultado, "is_rendered", True
                    ):
                        resultado.render()
                    ms = (perf_counter() - inicio) * 1000
                consultas = len(capturadas)
                escritas = sum(
                    bool(re.match(
                        r"^\s*(?:INSERT|UPDATE|DELETE)\b",
                        consulta["sql"],
                        flags=re.IGNORECASE,
                    ))
                    for consulta in capturadas
                )
            else:
                resultado = funcao()
            raise DesfazerSonda()
    except DesfazerSonda:
        return resultado, consultas, escritas, ms


def verificar_resposta(resposta):
    status = getattr(resposta, "status_code", None)
    if status is not None and status != 200:
        raise NaoMedida("view nao retornou HTTP 200")
    return resposta


def medir(numero, nome, carga, universo, funcao):
    # Views e servicos podem imprimir dados; descartar sua saida.
    with contextlib.redirect_stdout(io.StringIO()), \
         contextlib.redirect_stderr(io.StringIO()):
        _, consultas, escritas, ms = executar_revertendo(funcao, medir=True)
    print(
        "TELA %d %s carga=%s universo=%d consultas=%d escritas=%d ms=%.3f"
        % (numero, nome, carga, universo, consultas, escritas, ms)
    )


def falha(numero, nome, carga, exc):
    # Nao imprimir str(exc): pode conter nome, CPF ou outros dados.
    global erros
    erros = 1
    print(
        "TELA %d NAO MEDIDA %s carga=%s motivo=%s"
        % (numero, nome, carga, type(exc).__name__)
    )


def tentar(numero, nome, carga, funcao):
    try:
        funcao()
    except Exception as exc:
        falha(numero, nome, carga, exc)


def janela_colab(colab):
    return janela_atual(timezone.localdate(), colab.empresa_id)


def medir_espelho(carga, colab):
    user = usuario_colab(colab)
    _, fim = janela_colab(colab)
    req = pedido(
        "/ponto/espelho/",
        user,
        {"mes": fim.month, "ano": fim.year},
    )
    print("AMOSTRA tela=1 carga=%s col%d u%d" % (carga, colab.pk, user.pk))
    medir(
        1, "espelho", carga, 1,
        lambda: verificar_resposta(espelho_ponto(req)),
    )


def medir_pdf(carga, colab):
    user = usuario_gestor()
    _, fim = janela_colab(colab)
    req = pedido("/relatorios/espelho/avulso/", user)
    print("AMOSTRA tela=1_pdf carga=%s col%d u%d" % (carga, colab.pk, user.pk))

    def gerar():
        resultado = gerar_pdf_espelho(
            colaborador=colab,
            mes=fim.month,
            ano=fim.year,
            request=req,
            user=user,
            chamado=None,
        )
        if not resultado.get("pdf_bytes"):
            raise NaoMedida("servico nao devolveu PDF")
        return None

    medir(1, "espelho_pdf_servico", carga, 1, gerar)


def total_central(colab_id):
    # O total e calculado pela mesma montagem da Central, com rollback proprio.
    def apurar():
        ctx = montar_contexto_painel(
            f_status="",
            f_urgencia="",
            f_q="",
            f_colab=str(colab_id) if colab_id is not None else "",
            f_empresa="",
            f_praca="",
            pagina=1,
            f_verbo="",
            f_modo="resumo",
        )
        return int(ctx["total"])

    with contextlib.redirect_stdout(io.StringIO()), \
         contextlib.redirect_stderr(io.StringIO()):
        total, _, _, _ = executar_revertendo(apurar)
    return total


def medir_central(carga, colab_id):
    user = usuario_gestor()
    parametros = {} if colab_id is None else {"colab_id": str(colab_id)}
    universo = total_central(colab_id)
    req = pedido("/chamados/", user, parametros)
    print("AMOSTRA tela=3 carga=%s u%d" % (carga, user.pk))
    medir(
        3, "central", carga, universo,
        lambda: verificar_resposta(painel_gestao(req)),
    )


def medir_placar(carga, colab_id):
    user = usuario_gestor()
    parametros = {} if colab_id is None else {"colab_id": str(colab_id)}

    if colab_id is None:
        retrato = PlacarNavbar.objects.filter(pk=1).first()
        if retrato is not None and retrato.payload_placar:
            raise NaoMedida("universo do retrato lavrado nao comprovado")
        universo = qs_vivos().count()
    else:
        universo = qs_vivos(f_colab=str(colab_id)).count()

    req = pedido("/chamados/placar/", user, parametros)
    print("AMOSTRA tela=4 carga=%s u%d" % (carga, user.pk))
    medir(
        4, "placar", carga, universo,
        lambda: verificar_resposta(placar_json(req)),
    )


def medir_api(carga, colab):
    user = usuario_colab(colab)
    req = pedido("/api/me/", user, drf=True)
    print("AMOSTRA tela=5 carga=%s col%d u%d" % (carga, colab.pk, user.pk))
    medir(
        5, "api_me", carga, 1,
        lambda: verificar_resposta(api_me(req)),
    )


try:
    try:
        mediana, maior, _ = selecao()
        for carga, colab in (("a", mediana), ("b", maior)):
            tentar(1, "espelho", carga, lambda c=carga, x=colab: medir_espelho(c, x))
            tentar(1, "espelho_pdf_servico", carga, lambda c=carga, x=colab: medir_pdf(c, x))
    except Exception as exc:
        falha(1, "espelho_e_pdf", "a_b", exc)

    try:
        guardar_sombra()
        raise NaoMedida("rota e view da tarefa do dia nao comprovadas")
    except Exception as exc:
        falha(2, "tarefa_do_dia", "a_b", exc)

    try:
        mediana, _, _ = selecao()
        tentar(3, "central", "a", lambda: medir_central("a", mediana.pk))
        tentar(3, "central", "b", lambda: medir_central("b", None))
    except Exception as exc:
        falha(3, "central", "a_b", exc)

    try:
        mediana, _, _ = selecao()
        tentar(4, "placar", "a", lambda: medir_placar("a", mediana.pk))
        tentar(4, "placar", "b", lambda: medir_placar("b", None))
    except Exception as exc:
        falha(4, "placar", "a_b", exc)

    try:
        mediana, maior, _ = selecao()
        tentar(5, "api_me", "a", lambda: medir_api("a", mediana))
        tentar(5, "api_me", "b", lambda: medir_api("b", maior))
    except Exception as exc:
        falha(5, "api_me", "a_b", exc)
finally:
    logging.disable(nivel_logging_anterior)
    print("ERROS=%d" % erros)
    print("FIM_CHECK_TELA")
```

Na Central, `universo` é o total de chamados da lista padrão antes da paginação; a página padrão agrupa colaboradores e pagina esses grupos. No placar recomputado, `universo` é a contagem atual de chamados vivos do filtro. A Central captura internamente falhas do atalho de HE, portanto HTTP 200 não prova que esse atalho foi montado. [app/chamados/services/painel.py:333] [app/chamados/services/painel.py:449] [app/chamados/services/painel.py:498] [app/chamados/services/painel.py:135] [app/chamados/views.py:759]

A contagem de escritas registra comandos SQL iniciados por `INSERT`, `UPDATE` ou `DELETE`; escritas indiretas que não apareçam nesses comandos são [não verificado]. O script não foi testado no envelope de execução: compatibilidade em execução é [não verificado]. A gravação do resultado de referência também permanece [não verificado].

perguntas de lei: 0

## 19/11 02:20 · sessao 6ea3da4bc833 · ESCALA-NAO-DESCREVE · verificador · origem=Code

1. **BLOQUEIA — a interação semântica com o comboio continua [não verificado].** A prova fornecida de composição textual resolve a pendência de sobreposição de arquivos da c7: ela informa que os dois arquivos comuns não têm linhas do merge fora da união dos pais e que os arquivos exclusivos são idênticos aos respectivos lados. A prova está em `composicao.txt` fornecido na pergunta; numeração de linhas do anexo **[não verificado]**. Ela não mostra, porém, quais símbolos o comboio alterou nem seus contratos anteriores.

   Os pontos de contato identificáveis que precisam ser comparados com o diff do comboio são:

   | Símbolo usado pela ESCALA | Uso e definição na cópia | Mudança pelo comboio; composição |
   |---|---|---|
   | `_is_gestor` | A nova ação o usa em [views.py:484](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:484); definição em [chamados/views.py:35](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/chamados/views.py:35). | Assinatura, retorno e significado anteriores **[não verificado]**; composição **[não verificado]**. |
   | `turnos_do_colab`, `batidas_apuraveis` | O esmeril os importa em [esmeril_espelho.py:92](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:92); definições em [turnos.py:1850](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1850) e [turnos.py:2872](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:2872). | Alteração desses símbolos e composição **[não verificado]**. |
   | `dias_com_ausencia_inteira_do_periodo` | Uso em [esmeril_espelho.py:169](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:169); definição em [turnos.py:2707](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:2707). | Alteração e composição **[não verificado]**. |
   | `ChamadoColaborador` | Importado pelo esmeril em [esmeril_espelho.py:89](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:89); classe em [models.py:33](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/chamados/models.py:33). | Alterações pertinentes de campos ou significado e composição **[não verificado]**. |
   | `minutos_previstos_do_dia`, `marco_ocupado_pela_batida` | Usados pela régua em [regua_defesa.py:355](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/regua_defesa.py:355) e [regua_defesa.py:492](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/regua_defesa.py:492); definições em [utils.py:2241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/utils.py:2241) e [utils.py:387](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/utils.py:387). | Alteração e composição **[não verificado]**. |

   Esses são **candidatos**, não uma lista comprovada de símbolos efetivamente mudados pelo comboio; a exaustividade também é **[não verificado]**. A lista e o `stat` fornecidos identificam arquivos alterados, mas não o delta de seus símbolos **[não verificado]**. É necessário o diff completo base → comboio ou os objetos Git para concluir a comparação; a cópia consultada não é um repositório Git **[não verificado]**.

2. **NÃO BLOQUEIA — o F6 vem da base**, pela combinação da prova fornecida e do diff completo da ESCALA: `cadastro_realidade.py` consta entre os arquivos exclusivos da ESCALA, o merge é informado como idêntico a esse lado, e o diff base → ESCALA não altera `um_dono_por_dia`. A função F6 está em [cadastro_realidade.py:1076](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:1076), e sua chamada precede a leitura da lavra em [cadastro_realidade.py:957](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:957). A conferência direta dos objetos Git é **[não verificado]**.

3. **NÃO BLOQUEIA — L-040 e L-130 não se contradizem no texto examinado.** A primeira determina a lista Cadastro × Realidade ordenada por carga e o mesmo conteúdo no PDF em [LEIS.md:88](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/docs/LEIS.md:88); a segunda define quando uma pergunta está respondida e o tratamento da resposta ao reabrir em [LEIS.md:151](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/docs/LEIS.md:151). Que L-130 seja a única lei alterada pelo comboio é **[não verificado]**, pois seu diff não foi fornecido.

BLOQUEIA: 1  
perguntas de lei: 0

## 19/11 02:19 · sessao 7d81c532abdb · - · - · origem=Ronald (tmux)

- Espelho: `/ponto/espelho/` chama `espelho_ponto`, que lê `request.user.colaborador`, calcula a janela por empresa e chama `espelho_do_colab`. [app/config/urls.py:29](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/config/urls.py:29) [app/ponto/urls.py:10](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/urls.py:10) [app/ponto/views.py:96](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/views.py:96) [app/ponto/views.py:113](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/views.py:113) [app/ponto/views.py:156](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/views.py:156)

- PDF avulso: o GET de `/relatorios/espelho/avulso/` mostra o formulário; a geração chama o serviço `gerar_pdf_espelho` no ramo POST. O serviço pode ser medido diretamente, mas pode recusar uma disputa aberta. [app/relatorios/urls.py:28](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/urls.py:28) [app/relatorios/views.py:1050](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/views.py:1050) [app/relatorios/views.py:1126](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/views.py:1126) [app/relatorios/views.py:1145](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/views.py:1145) [app/relatorios/services.py:49](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/services.py:49) [app/relatorios/pdf_espelho.py:1084](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/pdf_espelho.py:1084)

- O PDF auditável grava `RegistroPDFAuditavel`, e seu conteúdo inclui nome e CPF: a medição precisa reverter a transação e não imprimir nem salvar os bytes. [app/relatorios/pdf_base.py:151](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/pdf_base.py:151) [app/relatorios/pdf_espelho.py:738](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/relatorios/pdf_espelho.py:738)

- A competência corrente varia com o corte da empresa; `janela_atual` fornece seus limites. A contagem pode usar `Batida.colaborador` e `Batida.timestamp`, incluindo colaboradores com zero batidas no universo da mediana. [app/ponto/janelas.py:36](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/janelas.py:36) [app/ponto/janelas.py:60](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/janelas.py:60) [app/ponto/models.py:22](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/models.py:22)

## 19/11 02:19 · sessao 17766839b9d7 · E4C-F3-LAVRA-FOTO · verificador · origem=Code

**Parecer: BLOQUEIA.** O teste tem mordida nas formas exemplificadas, mas não garante a propriedade geral prometida; há também uma contradição temporal concreta entre o selo e a premissa usada para confiar na lavra. ([test_f3_lavra_foto_sem_decisao.py:3](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:3), [veredito_lavrado.py:213](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:213))

1. **BLOQUEIA — selo com escape.** O AST detecta os exemplos de import direto, apelido de import, módulo, `get_model`, manager relacionado e função aninhada; os testes de mutação correspondentes estão em [test_f3_lavra_foto_sem_decisao.py:165](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:165) e [test_f3_lavra_foto_sem_decisao.py:200](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:200). Mas ele registra apelidos de import sem propagar uma atribuição posterior, e a captura SQL só observa os caminhos executados pela fixture. ([test_f3_lavra_foto_sem_decisao.py:75](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:75), [test_f3_lavra_foto_sem_decisao.py:312](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:312))

   Mutante proposto, sem alterar arquivos: no módulo, `from chamados.models import DecisaoPergunta as _DP` seguido de `_X = _DP`; dentro de `marcos_do_lote`, `if ate is not None and ate.year == 2001: list(_X.objects.all())`. O corpo usa `_X`, nome que o detector não adiciona ao conjunto de apelidos; o ramo não executa com os dois valores da fixture, `ATE` em 9998 e `None`. Assim, a leitura escaparia ao AST e à captura SQL. ([test_f3_lavra_foto_sem_decisao.py:78](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:78), [test_f3_lavra_foto_sem_decisao.py:119](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:119), [test_f3_lavra_foto_sem_decisao.py:272](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:272)) Resultado efetivo da execução do mutante: **[não verificado]**.

2. **NÃO BLOQUEIA — localização e anti-vacuidade.** O detector procura a função de módulo pelo nome e exige exatamente uma ocorrência; o teste também verifica que um nome ausente levanta erro. Não há allowlist no detector: ele forma os nomes relacionados a partir do modelo e acrescenta nomes fixos proibidos. ([test_f3_lavra_foto_sem_decisao.py:51](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:51), [test_f3_lavra_foto_sem_decisao.py:98](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:98), [test_f3_lavra_foto_sem_decisao.py:158](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:158))

3. **NÃO BLOQUEIA — escopo do diff e mecânica do teste.** O diff fornecido contém somente o novo teste; alteração de `.py` de produção pelo diff: não. A cópia não tem metadados Git disponíveis para confirmar independentemente o hash ou comparar a base: **[não verificado]**. O teste usa datas explícitas para `ATE`, `MARCO` e `ATO`; porém, o `setUpTestData` cria colaborador e disputa cujos modelos têm `auto_now_add` em `criado_em` e `aberta_em`. Esses campos não são os marcos consultados pela função inspecionada. ([test_f3_lavra_foto_sem_decisao.py:271](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:271), [colaboradores/models.py:380](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/colaboradores/models.py:380), [chamados/models.py:1893](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/models.py:1893), [veredito_lavrado.py:177](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:177)) Os `subTest` envolvem assertivas e não há `try` que as engula no teste novo. ([test_f3_lavra_foto_sem_decisao.py:308](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:308), [test_f3_lavra_foto_sem_decisao.py:316](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:316))

4. **BLOQUEIA — premissa temporal da lavra.** `marcos_do_lote` usa o máximo de `respondida_em`, cortado por `ate`; `esta_stale` só considera esse marco quando ele é posterior a `julgado_em`. ([veredito_lavrado.py:201](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:201), [veredito_lavrado.py:230](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:230)) Já a cura pela trilha pode escrever **agora** um `respondida_em` antigo, correspondente à nota, e registrar uma decisão com esse mesmo instante antigo. ([disputa_emissao.py:1692](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1692), [curar_perguntas_em_limbo.py:68](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/management/commands/curar_perguntas_em_limbo.py:68))

   Consequência demonstrável pelo código: se o instante da nota for anterior à lavra, a escrita posterior não gera, por esse campo, um marco maior que `julgado_em`; excluir a decisão também impede que sua inserção mude os marcos. Isso contradiz a premissa de `esta_stale` de detectar insumo alterado depois da lavra e afeta o consumidor da tarefa, que confia na lavra quando ela não está stale. ([veredito_lavrado.py:213](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:213), [tarefa_do_dia.py:433](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/tarefa_do_dia.py:433)) Ocorrência e impacto em produção: **[não verificado]**.

   Aqui `respondida_em` representa o instante atribuído ao ato, mas é também usado como indicador de quando um insumo mudou para fins de invalidação; esses instantes podem divergir numa cura retroativa. A regra desejada para corrigir essa divergência é uma **pergunta de lei**, pois o brief exclui alterar o leitor e o carimbo. ([disputa_emissao.py:1693](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1693), [veredito_lavrado.py:213](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:213); escopo do brief fornecido: **[não verificado em arquivo local]**)

5. **NÃO BLOQUEIA — censo dos caminhos principais e derivação paralela.** `marcos_do_lote` deriva os marcos de batidas, células e perguntas; não grava esses insumos. `esta_stale`, `fio`, `raiox`, `tarefa_do_dia` e o auditor consomem os marcos. ([veredito_lavrado.py:151](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/veredito_lavrado.py:151), [fio.py:282](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/fio.py:282), [raiox.py:38](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/raiox.py:38), [tarefa_do_dia.py:435](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/tarefa_do_dia.py:435), [auditar_invariantes_chamados.py:1174](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/management/commands/auditar_invariantes_chamados.py:1174))

   A resposta humana grava `respondida_em` e registra a decisão com o mesmo instante; a resposta automática e a cura pela trilha também escrevem ambos. `DecisaoPergunta.registrar` grava a sequência; `DecisaoPergunta.leitura` lê as decisões e incorpora o passivo da projeção. ([disputa_emissao.py:1527](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1527), [disputa_emissao.py:1681](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1681), [disputa_emissao.py:1705](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/disputa_emissao.py:1705), [models.py:4295](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/models.py:4295), [models.py:4329](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/models.py:4329)) O novo teste não introduz derivação paralela em produção; a duplicação de lógica de stale já existente em `raiox` está declarada no próprio código. ([raiox.py:20](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/raiox.py:20), [raiox.py:39](/home/ronald/codex_ws/88f67e29fc33bfd59b430eb77e1ed7b86af89f23/app/chamados/services/raiox.py:39))

Execução da suíte, logs de RED e resultados em banco: **[não verificado]**. [nao verificado]

BLOQUEIA: 2  
perguntas de lei: 1

## 19/11 02:18 · sessao 3ac06bef2621 · - · - · origem=Ronald (tmux)

Achados enviados ao agente principal, com rotas, serviços, permissões, filtros e limites de verificação dos universos. [app/chamados/views.py:719] [app/chamados/views.py:770] [app/chamados/services/painel.py:198]

## 19/11 02:18 · sessao bfe32ac9cfed · - · - · origem=Ronald (tmux)

- **Tarefa do dia:** `tarefa_do_dia` é um serviço que recebe colaborador, dia do turno e instante; não comprovei uma rota de tela correspondente. [app/chamados/services/tarefa_do_dia.py:738] A equivalência com “Meu Atendimento” é [não verificado]; essa rota chama `montar_chamados_fio` e pode registrar auditoria no GET. [app/chamados/urls.py:16] [app/chamados/views.py:566] [app/chamados/views.py:583]
- **Consulta do app:** `/api/me/` chama `api_me`, exige autenticação e usa `request.user.colaborador`. [app/config/urls.py:37] [app/api/urls.py:64] [app/api/views.py:282] Entre os serviços chamados estão `turno_aberto_de`, `proximo_tipo_de` e `projecao_do_app`. [app/api/views.py:306] [app/api/views.py:308] [app/api/views.py:315] A resposta trata de um colaborador e contém seu nome, por isso não deve ser impressa pela sonda. [app/api/views.py:336]

## 19/11 02:18 · sessao e37c8a1bfe88 · ESPELHO-VERDADE · sonda · origem=Code

A lista nominal por PK e os totais **não estão no dado raspado**; são **[não verificado]**. O anexo contém o molde e uma referência a outra sonda, mas não contém a saída desta apuração. O script abaixo é entregue como texto e não foi executado. [não verificado]

**A) Caminho até o dono**

- O comando percorre empresas ativas e colaboradores com `FechamentoMensal` na competência pedida; `--so-txt` é um filtro opcional, que a sonda abaixo não aplica. A janela é calculada por empresa. [app/ponto/management/commands/e6_oraculo.py:303](app/ponto/management/commands/e6_oraculo.py#L303)
- Para cada colaborador, obtém o espelho e constrói `InsumosDoDono`. Percorre os dias da janela e exclui os sem linha no espelho, isentos ou com ausência, sem minutos lavrados, com zero em ambos e aqueles cuja diferença é a janela declarada. Os minutos do espelho vêm primeiro de `minutos_realizados`, depois de `pago_h`; se ambos faltam, não há comparação. [app/ponto/management/commands/e6_oraculo.py:314](app/ponto/management/commands/e6_oraculo.py#L314), [app/ponto/management/commands/e6_oraculo.py:263](app/ponto/management/commands/e6_oraculo.py#L263)
- Entre os dias comparados, só os com diferença absoluta **maior que 10 minutos** chegam a `_ins.dono(dia)`. Assim, “universo avaliado por dono” abaixo significa dias divergentes que chegaram ao juiz, não todos os dias comparados. [app/ponto/management/commands/e6_oraculo.py:362](app/ponto/management/commands/e6_oraculo.py#L362)
- `horas_do_dia` vem de `minutos_do_oraculo` sobre timestamps locais de `batidas_apuraveis`, numa janela de 12 horas antes do início até 14 horas depois do dia seguinte ao fim; `dono()` passa as horas atribuídas à data. `em_cadastro_x_realidade` é a pertinência da data ao conjunto extraído de `dias_cadastro_x_realidade` do resultado do espelho. [app/ponto/management/commands/e6_oraculo.py:149](app/ponto/management/commands/e6_oraculo.py#L149), [app/ponto/management/commands/e6_oraculo.py:162](app/ponto/management/commands/e6_oraculo.py#L162), [app/ponto/management/commands/e6_oraculo.py:182](app/ponto/management/commands/e6_oraculo.py#L182)
- As três flags de cadastro vêm de `cadastro_do_dia_carregado`: ele consulta o vínculo do dia e, quando não existe, também a célula e a data de demissão; com vínculo, consulta `defeito_folga_nao_definida`. [app/ponto/supra_juiz.py:179](app/ponto/supra_juiz.py#L179), [app/ponto/supra_juiz.py:194](app/ponto/supra_juiz.py#L194)
- `batida_fora_de_turno` usa `realizado_do_dia` e exige turno aberto com entrada solta presente entre as batidas apuráveis; os turnos são carregados para a janela ampliada em um dia de cada lado. `fase_deslocada` vem da pertinência da data aos dias retornados por `dias_de_fase_as_of`. [app/ponto/management/commands/e6_oraculo.py:156](app/ponto/management/commands/e6_oraculo.py#L156), [app/ponto/management/commands/e6_oraculo.py:174](app/ponto/management/commands/e6_oraculo.py#L174), [app/ponto/management/commands/e6_oraculo.py:198](app/ponto/management/commands/e6_oraculo.py#L198)
- Há **uma única cláusula executável que devolve ESTRUTURA**: o lado falso do retorno condicional em [app/ponto/management/commands/e6_oraculo.py:110](app/ponto/management/commands/e6_oraculo.py#L110). Para alcançá-la, nenhum dos retornos anteriores de cadastro, batida ou fase pode ter ocorrido, e `em_cadastro_x_realidade` deve ser falso. [app/ponto/management/commands/e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py#L98)

**B) Sonda**

O juiz devolve só o dono. A sonda registra, com `sys.settrace`, a linha de retorno efetivamente executada pela função real; para `ESTRUTURA`, exige a linha 110 verificada nesta cópia e falha se o código mudou. Não reconstrói as condições do juiz. [app/ponto/management/commands/e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py#L98)

Para cada empresa, usa a janela que contém a data do ato como corrente e exige que ela não esteja trancada. Define “última fechada” como a competência anterior mais recente com tranca viva; a autoridade do código define fechada como trancada e usa `reaberto_em` para identificar a tranca viva. Se faltar uma dessas condições, a sonda falha e não apresenta contagens parciais. [app/ponto/janelas.py:60](app/ponto/janelas.py#L60), [app/ponto/services/fechamento.py:1464](app/ponto/services/fechamento.py#L1464)

Executar no mesmo envelope Django de leitura do anexo, com a árvore montada em leitura e o processo limitado ao cpuset de teste; a sonda também verifica sua afinidade. A fonte de recursos declara por padrão CPUs 4–7 para teste. [bin/sombra.sh:182](bin/sombra.sh#L182), [bin/recursos.sh:37](bin/recursos.sh#L37)

```python
# Sonda como texto: executar no envelope Django da sombra, tenant juliani.
# Executar Python com -B e processo no cpuset de teste. [nao verificado]
# Não imprime nome nem CPF. Nenhum arquivo é criado por este código.

import sys
sys.dont_write_bytecode = True

import collections
import datetime
import os

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa, Colaborador
from ponto.models import FechamentoMensal, PeriodoFechado
from ponto import janelas
from ponto.services.espelho import espelho_do_colab
from ponto.services.fechamento import competencia_trancada
from ponto.management.commands import e6_oraculo as e6


DONOS = ("ESTRUTURA", "CADASTRO", "BATIDA")
ARQUIVO_JUIZ = "app/ponto/management/commands/e6_oraculo.py"
LINHA_ESTRUTURA = 110


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")

    # TESTE_CPUSET pode ser fornecido pelo envelope a partir de recursos.sh.
    especificacao = os.environ.get("TESTE_CPUSET", "4-7")
    permitidas = set()
    for parte in especificacao.split(","):
        limites = parte.strip().split("-")
        if len(limites) == 1:
            permitidas.add(int(limites[0]))
        elif len(limites) == 2:
            inicio, fim = map(int, limites)
            if inicio > fim:
                raise RuntimeError("cpuset_invalido")
            permitidas.update(range(inicio, fim + 1))
        else:
            raise RuntimeError("cpuset_invalido")
    afinidade = os.sched_getaffinity(0)
    if not afinidade or not afinidade.issubset(permitidas):
        raise RuntimeError("cpuset")

    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")

    with connection.cursor() as cursor:
        cursor.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY"
        )
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def dono_e_linha(insumos, dia):
    # Observa o retorno da função real chamada por InsumosDoDono.dono().
    if sys.gettrace() is not None:
        raise RuntimeError("trace_preexistente")

    retornos = []

    def observar(frame, evento, argumento):
        if frame.f_code is e6.dono_da_divergencia.__code__:
            if evento == "return":
                retornos.append((frame.f_lineno, argumento))
            return observar
        return observar

    try:
        sys.settrace(observar)
        dono = insumos.dono(dia)
    finally:
        sys.settrace(None)

    # Com o espelho já fornecido, dono() deve chamar o juiz uma vez.
    if len(retornos) != 1 or retornos[0][1] != dono:
        raise RuntimeError("retorno_do_juiz")
    if dono not in DONOS:
        raise RuntimeError("dono_desconhecido")

    linha = retornos[0][0]
    if dono == "ESTRUTURA" and linha != LINHA_ESTRUTURA:
        raise RuntimeError("clausula_mudou")
    return dono, linha


def main():
    etapa = "GUARDA"
    try:
        linhas = []
        contagem_donos = collections.Counter()
        por_clausula = collections.Counter()
        por_empresa = collections.Counter()
        universo = 0

        with transaction.atomic():
            guarda_sombra()
            agora = timezone.now()
            hoje = timezone.localtime(agora).date()

            etapa = "COMPETENCIAS"
            empresas = list(Empresa.objects.filter(ativa=True).order_by("pk"))

            for empresa in empresas:
                ini_atual, fim_atual = janelas.janela_atual(hoje, empresa)
                atual = (fim_atual.year, fim_atual.month)

                if competencia_trancada(empresa.pk, atual[1], atual[0]):
                    raise RuntimeError("corrente_trancada")

                # A ordem ano/mês identifica a competência mais recente,
                # anterior à corrente, cuja tranca não foi reaberta.
                trancas = PeriodoFechado.objects.filter(
                    empresa=empresa, reaberto_em__isnull=True
                ).order_by("-ano", "-mes")
                fechada = next(
                    (
                        (p.ano, p.mes)
                        for p in trancas
                        if (p.ano, p.mes) < atual
                    ),
                    None,
                )
                if fechada is None:
                    raise RuntimeError("sem_fechada_anterior")

                for tipo, (ano, mes) in (
                    ("corrente", atual),
                    ("ultima_fechada", fechada),
                ):
                    etapa = "JANELA"
                    ini, fim = janelas.janela_fechamento(mes, ano, empresa)
                    competencia = "%04d-%02d" % (ano, mes)
                    linhas.append(
                        "ESCOPO empresa=%s tipo=%s competencia=%s inicio=%s fim=%s"
                        % (empresa.pk, tipo, competencia, ini, fim)
                    )

                    ids = FechamentoMensal.objects.filter(
                        mes=mes, ano=ano, colaborador__empresa=empresa
                    ).values_list("colaborador_id", flat=True)

                    colaboradores = Colaborador.objects.filter(
                        pk__in=ids
                    ).order_by("pk")

                    for colab in colaboradores:
                        etapa = "ESPELHO"
                        espelho = espelho_do_colab(
                            colab, hoje, ini, fim, agora=agora
                        )
                        dias_espelho = {
                            str(item.get("data"))[:10]: item
                            for item in (espelho.get("dias") or [])
                        }

                        etapa = "INSUMOS"
                        insumos = e6.InsumosDoDono(
                            colab, ini, fim, hoje,
                            espelho=espelho, agora=agora
                        )
                        minutos = insumos.oraculo.minutos

                        dia = ini
                        while dia <= fim:
                            chave = str(dia)
                            registro = dias_espelho.get(chave)

                            if registro is not None:
                                if not (
                                    registro.get("isencao")
                                    or registro.get("ausencia") is not None
                                ):
                                    m_esp, _ = e6.minutos_do_espelho(registro)
                                    if m_esp is not None:
                                        m_e6 = minutos.get(chave, 0.0)
                                        if not (m_esp == 0 and m_e6 == 0):
                                            diferenca = m_e6 - m_esp
                                            if not e6.diferenca_e_a_janela_declarada(
                                                diferenca, registro
                                            ):
                                                if abs(diferenca) > 10:
                                                    etapa = "DONO"
                                                    dono, linha = dono_e_linha(
                                                        insumos, dia
                                                    )
                                                    universo += 1
                                                    contagem_donos[dono] += 1

                                                    if dono == "ESTRUTURA":
                                                        clausula = "%s:%d" % (
                                                            ARQUIVO_JUIZ, linha
                                                        )
                                                        por_clausula[clausula] += 1
                                                        por_empresa[empresa.pk] += 1
                                                        linhas.append(
                                                            "ESTRUTURA %s colab_pk=%s "
                                                            "empresa=%s dia=%s clausula=%s"
                                                            % (
                                                                competencia,
                                                                colab.pk,
                                                                empresa.pk,
                                                                chave,
                                                                clausula,
                                                            )
                                                        )
                            dia += datetime.timedelta(days=1)

            etapa = "CONFERENCIA"
            contador = sum(contagem_donos[dono] for dono in DONOS)
            if contador != universo:
                raise RuntimeError("soma_donos")

            for clausula, quantidade in sorted(por_clausula.items()):
                linhas.append(
                    "RESUMO_CLAUSULA clausula=%s total=%d"
                    % (clausula, quantidade)
                )
            for empresa in empresas:
                linhas.append(
                    "RESUMO_EMPRESA empresa=%s estrutura=%d"
                    % (empresa.pk, por_empresa[empresa.pk])
                )
            for dono in DONOS:
                linhas.append(
                    "RESUMO_DONO dono=%s total=%d"
                    % (dono, contagem_donos[dono])
                )
            linhas.append(
                "CONFERENCIA contador=%d universo_avaliado=%d "
                "contador==universo=%s"
                % (contador, universo, "s" if contador == universo else "n")
            )

        for linha in linhas:
            print(linha)
        print("ERROS=0")
        print("FIM_SONDA_EV_V1")
        return 0

    except Exception:
        print("ERRO etapa=%s" % etapa)
        print("ERROS=1")
        print("FIM_SONDA_EV_V1")
        return 1


sys.exit(main())
```

A execução, a disponibilidade das duas competências para cada empresa e a saída sem efeitos indiretos das funções chamadas permanecem **[não verificado]**. A transação `READ ONLY` faz a execução falhar caso uma dessas funções tente escrever no banco; os demais efeitos indiretos não foram comprovados. [não verificado]

perguntas de lei: 0

_7275 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
