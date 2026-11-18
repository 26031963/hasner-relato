# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 09:06** · publicado em **18/11/2026 09:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 09:06 · sessao 2dc2a33c16b9 · - · - · origem=Ronald (tmux)

## 1 Premissas e sitios

A identificação independente da cópia como `dd489c627` continua **[nao verificado]**: a árvore foi fornecida sem `.git`. As referências a `brief` e `matriz` abaixo são as linhas numeradas na remedição anexada; a correspondência delas com arquivos externos originais é **[nao verificado]**.

A revisão abaixo não cobre todas as premissas e todos os degraus pedidos. Por isso, não seria correto apresentar uma contagem fechada de CONFERE, MOVEU e CAIU.

| Premissa ou sítio dos anexos | Julgamento na cópia |
|---|---|
| D01 conservado e SQL-R lidos da sequência de atos — brief:28–29; matriz:22–23 | **CAIU.** `pergunta_respondida` e sua face SQL leem instante e texto da projeção. [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575) |
| Grade filtra apenas `respondida_em` — brief:84; matriz D01:33 | **CAIU.** A grade usa `q_respondida_com_texto()` antes de `fio_vivo`. O RED antigo que exige prender uma pergunta com resposta apenas no ato não decorre dessa implementação. [reconciliar_grade.py:460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:460), [juizes.py:1552](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1552) |
| I21a testa só instante — brief:37; matriz D22:71 | **CAIU.** A contraprova chama `pergunta_respondida`. [auditar_invariantes_chamados.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:176) |
| Piso I2 testa só instante — brief:41; matriz D26:75 | **CAIU.** O piso chama o mesmo juiz. [auditar_invariantes_chamados.py:592](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:592) |
| I21 usa R cru — brief:46; matriz D32:81 | **CAIU.** I21 usa a face SQL de respondida; V e C continuam em filtros projetados. [auditar_invariantes_chamados.py:1576](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1576) |
| C2 exige R nulo — brief:47; matriz D33:82 | **CAIU.** C2 usa a negação SQL do juiz de resposta e mantém V/C projetados. [auditar_invariantes_chamados.py:1605](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1605) |
| F1 testa texto isolado — brief:48; matriz D34:83 | **CAIU.** O ramo chama `pergunta_respondida`. [auditar_invariantes_chamados.py:1657](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1657) |
| Dedup rotula pela presença de texto — brief:55; matriz D14:59 | **CAIU.** `com_resp` usa `pergunta_respondida`. [dedup_perguntas_fato.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/dedup_perguntas_fato.py:76) |
| Reabrir órfãs, sua porta e colisão com RELAVRA-11 — brief:42–44,114–118; matriz D07–D10b:47–51 | **CAIU.** A lápide registra a saída do comando e de `reabrir_fantasma`; os degraus e a colisão textual pressupostos deixam de ser sítios vivos da cópia. [crons.py:835](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:835), [models.py:3429](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3429) |
| Limbo é reparo da projeção com guarda nominal — brief:54,138–140; matriz D11–D13 e P11:54–56,118–119 | **CONFERE.** O filtro é texto com instante nulo e o comando recusa quando o conjunto difere de `MEDIDAS`. [curar_perguntas_em_limbo.py:5](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:5), [curar_perguntas_em_limbo.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:46) |
| I18 usa V e autor da projeção — matriz D29:78 | **MOVEU** para [auditar_invariantes_chamados.py:1077](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1077): o filtro usa `validada_em` e exclui por `validada_por__username`. |
| F2/F3 pré-filtram V projetada — matriz D35–D36:84–85 | **MOVEU** para [auditar_invariantes_chamados.py:1770](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1770) e [auditar_invariantes_chamados.py:1784](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1784). |
| `universo_i6` devolve pares `(pk, materializada_em)` — matriz P8:111 | **CAIU.** Os pares são dados internos da consulta; o retorno é conjunto de violações e dicionário de pisos. [auditar_invariantes_chamados.py:471](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:471), [auditar_invariantes_chamados.py:485](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:485) |
| Existe selo parcial de vitalidade, sem cobertura geral de R — matriz:14–19 | **CONFERE.** O selo declara apenas C/V e seu walker examina formas específicas de comparação, `__isnull` e condição. [test_contract_leitores_vitalidade.py:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:14), [test_contract_leitores_vitalidade.py:313](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:313) |
| Três crons e auditoria sem persistência — brief:141–146; matriz P10:116–117 | **CAIU parcialmente.** As três agendas conferem, mas o auditor com `--html` grava arquivo. [crons.py:392](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:392), [crons.py:462](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:462), [crons.py:542](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:542), [auditar_invariantes_chamados.py:1859](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1859) |

**CONFERE a, MOVEU b, CAIU c: [nao verificado]** para o conjunto completo. Faltam o julgamento individual dos demais sítios do brief, D01–D36/D10b e contratos P1–P12, inclusive provas e destinos propostos.

## 2 O que mudou

Li o conteúdo do `diff_F9.patch` fornecido no pedido. A aplicação exata do patch entre os dois hashes e a identidade dos objetos Git permanecem **[nao verificado]**, pois não há `.git` na cópia.

- **Grade:** o patch troca o filtro por instante pela face SQL de L-130. Isso já resolve, no código atual, o caso da remedição anterior em que instante sem texto prendia a órfã como respondida. [reconciliar_grade.py:460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:460), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575)
- **Backfill:** o patch acrescenta `respondida_em` ao `only()` usado na escolha da canônica; a leitura de resposta deixa de depender apenas do texto carregado nessa consulta. [backfill_decisoes_pergunta.py:504](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_decisoes_pergunta.py:504)
- **Juiz e modelos:** nasce `q_nao_respondida`, usada, por exemplo, na ressuscitação; a propriedade `respondida` delega ao juiz. O patch também introduz as marcas `da_ressuscitacao` e `resposta_mantem_validacao`. [juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583), [models.py:1533](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1533), [models.py:3674](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3674), [models.py:4071](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4071)
- **Escritores e leitores novos identificados dessas marcas:** a resposta humana declara `resposta_mantem_validacao=False`; respostas automática e pela trilha declaram `True`; a fotografia da projeção também declara `True`. A tarefa lê `es_da_ressuscitacao`. [disputa_emissao.py:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527), [disputa_emissao.py:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681), [disputa_emissao.py:1703](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1703), [models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143), [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427)
- **Materialização:** o patch passa a consultar o juiz de resposta e reúne a guarda de anuência dos três tipos de resposta. A seleção distingue respondida com texto de instante sem texto, que entra para recusa. [materializacao.py:871](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:871), [materializacao.py:1269](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1269), [materializacao.py:2090](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2090)
- **Fantasmas:** o patch acrescenta tratamento e contador de recusa por competência entregue; o trecho de exibição de C continua existente. [reconciliar_fantasmas.py:213](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_fantasmas.py:213), [reconciliar_fantasmas.py:274](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_fantasmas.py:274)
- **Selo e retirada da reabertura:** o patch acrescenta uma entrada à allowlist e retira a entrada do comando extinto. A lápide no modelo confirma a retirada da porta. [test_contract_leitores_vitalidade.py:16](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:16), [test_contract_leitores_vitalidade.py:177](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:177), [models.py:3429](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3429)

Há uma **inconsistência a resolver**: a remedição anexada atribui à base `fbc5c76ce` leituras antigas em I21a, I2, C2, F1 e dedup, enquanto a cópia tem juízes nesses sítios e o patch fornecido não traz hunks desses dois arquivos. Não há evidência suficiente aqui para atribuir essas mudanças a um commit do intervalo nem para declarar qual retrato histórico está errado. [auditar_invariantes_chamados.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:176), [auditar_invariantes_chamados.py:592](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:592), [dedup_perguntas_fato.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/dedup_perguntas_fato.py:76); histórico **[nao verificado]**.

## 3 Nao verificado de ALCANCE

Foram fechados estes vínculos pontuais:

- `universo_i6` é chamado por `relavrar_via_i6`; o consumidor desempacota o retorno real. [relavrar_via_i6.py:47](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/relavrar_via_i6.py:47), [relavrar_via_i6.py:49](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/relavrar_via_i6.py:49)
- Dedup importa `aplicar` e `CONTADORES_DO_APPLY` do backfill. [dedup_perguntas_fato.py:105](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/dedup_perguntas_fato.py:105)
- A worklist importa e usa `_parse_pendentes` e `MODULOS_ALVO`. [worklist.py:37](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/worklist.py:37)
- O selo atual exclui `models.py` e `juizes.py` do percurso e não constitui, sozinho, censo integral dos campos RVC. [test_contract_leitores_vitalidade.py:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:14), [test_contract_leitores_vitalidade.py:323](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:323)

Continuam abertos **[nao verificado]**: o inventário integral de escritores e leitores RVC/atos, inclusive ambas as cascas; chamadas dinâmicas; todas as interseções de arquivos entre fatias; e a revisão de cada ponto de alcance assinalado no brief e na matriz. Falta um censo completo da árvore e a reconciliação de cada ocorrência com os anexos.

**alcance nao verificado restante: [nao verificado]** — não foi possível contar com segurança os pontos individuais ainda abertos.

## 4 Perguntas de lei

A busca nos documentos de lei encontrou respostas já registradas para parte das perguntas antigas:

- **DECIDIDO PELA LEI L-130:** respondida exige instante e texto não vazio; os dois meios-pares são ilegais. [LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151)
- **DECIDIDO PELA LEI CORTES-F2-F9-1611:** a face SQL de validação é face do juiz registrado, condicionada ao selo de equivalência; o corte também mantém guardas de reparo da projeção na projeção. [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509)
- **DECIDIDO PELA LEI LU-08:** a conferência vale para o par conferido. [CORTES.md:487](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:487)
- **DECIDIDO PELA LEI L-111:** a regra registrada trata da retirada de função que perdeu o último chamador de produção. [LEIS.md:137](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:137)

A revisão foi parcial: **zero perguntas novas foi identificado nos pontos acima, mas a contagem integral de perguntas de lei da F9 continua [nao verificado]**. Em particular, a existência de texto sem instante na ressuscitação está protegida por uma exceção no código; sua decisão de lei e sua pertinência específica à F9 não foram fechadas nesta revisão. [models.py:1538](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1538)

perguntas de lei: 0

## 18/11 09:06 · sessao 96825741f0ad · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**A remedição abaixo é parcial:** não consegui julgar todas as premissas do brief e da matriz nem fechar uma contagem defensável de CONFERE/MOVEU/CAIU. As referências `B` e `M` são as linhas dos anexos fornecidos, usando a numeração da remedição; a correspondência com arquivos externos não foi verificada.

| Premissa e sítio antigo | Julgamento no código da cópia |
|---|---|
| B:20–28; M:22–28, U1: procedência escolhe autor e instante de V, com fallback para C. | **CONFERE** — [procedencia_batida.py:71](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:71) e [procedencia_batida.py:74](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:74). |
| B:21; M:33–35, U2: filtro de via e V e leitura da recusa técnica. | **CONFERE** — [flip_auto.py:129](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:129). |
| B:22; M:41–42, U3: gate lê texto projetado por `strip`. | **CAIU** — os dois usos passam pelo juiz `pergunta_respondida`: [cartorio.py:1641](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1641), [cartorio.py:1649](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1649), [cartorio.py:1666](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1666). |
| B:23; M:43, U4: gate lê via duplicata da projeção. | **MOVEU** para [cartorio.py:1651](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1651). |
| B:24; M:48–50, U5: SILÊNCIO recebe carimbo C e texto projetado. | **CONFERE** — [veredito_celula.py:196](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:196). |
| B:25; M:56, U6: calendário usa `exclude(resposta_colab='')`. | **CAIU** — usa a face SQL do juiz: [calendario.py:499](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:499), [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505). |
| B:26; M:62–63, U7: ficha lê texto diretamente e a retenção usa I **ou** T. | **CAIU** — a ficha e a retenção perguntam ao juiz: [ficha.py:169](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:169), [ficha.py:172](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:172), [models.py:1933](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1933). |
| B:27; M:68, U8: prévia decide pelo texto isolado. | **CAIU** — [previa.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/previa.py:142). |
| B:28; M:73, U9: dia serializa o texto projetado por truthiness. | **CONFERE** — [dia_do_colab.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_do_colab.py:70). |
| B:61–64; M:75–84, F1 fornece a antiga face conservada de leitura dos atos. | **CAIU como contrato operativo da F8** — o juiz atual devolve booleano para instante e texto; há faces SQL positiva e negativa: [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575), [juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583). |

CONFERE, MOVEU e CAIU para **todas** as premissas: **[nao verificado]**. Os dez julgamentos acima não são uma contagem da matriz completa.

## 2 O que mudou

O patch fornecido compara `fbc5c76ce` com `dd489c627`; a cópia não permite confirmar o diff independentemente por Git. Os seguintes efeitos do patch foram conferidos nos sítios da árvore atual. [nao verificado]

- A B2 substituiu as leituras isoladas de R em cartório, calendário, ficha e prévia pelo juiz ou sua face SQL. A retenção chamada pela ficha também passou de I **ou** T para I **e** T. Isso retira essas mudanças do trabalho ainda pendente da F8, embora o resultado em dados reais permaneça **[nao verificado]**. [cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505), [ficha.py:172](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:172), [previa.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/previa.py:142), [models.py:1933](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1933).
- Novos leitores do juiz no intervalo incluem a lista `respondidas` e a resposta na API, as duas portas web e o vigia; a face negativa também passou a alimentar a reabertura. [api/views.py:2578](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2578), [api/views.py:2622](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2622), [chamados/views.py:1281](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1281), [chamados/views.py:1418](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1418), [vigia_fio.py:636](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/vigia_fio.py:636).
- A materialização ganhou uma guarda comum de anuência nos três ramos; quando o juiz diz que não há resposta, ela chama a porta de recusa técnica. Assim, U2 continua dependente do motivo projetado, inclusive de um caminho novo que pode escrevê-lo. [materializacao.py:1269](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1269), [materializacao.py:1289](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1289), [materializacao.py:1670](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1670).
- A F2 acrescentou as marcas `da_ressuscitacao` e `resposta_mantem_validacao`; a carga da tarefa exclui da sequência os atos identificados pela primeira. A resposta sintética da projeção declara a segunda como verdadeira. [models.py:4067](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4067), [models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143), [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427).
- O teste B2 antes indisponível agora está na cópia e contém casos para meios pares e para a face SQL negativa; sua execução e a cobertura completa dos degraus F8 permanecem **[nao verificado]**. [test_f1nova_b2_leitores.py:61](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_f1nova_b2_leitores.py:61), [test_f1nova_b2_leitores.py:134](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_f1nova_b2_leitores.py:134).

O inventário completo de escritores e leitores novos do intervalo **não foi concluído**. [nao verificado]

## 3 Nao verificado de ALCANCE

- **U7b foi fechado quanto ao corpo:** a ficha chama `respostas_sem_veredito`, que delega a `sem_veredito_entre`; este agora usa o juiz. Outros consumidores encontrados incluem o reconciliador e o suprajuiz. [ficha.py:169](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:169), [models.py:1914](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1914), [models.py:1933](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1933), [reconciliador.py:1208](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1208), [supra_juiz.py:573](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:573).
- **Cadeia de U2:** além do comando automático, há consumo de `avaliar_fila` pela tela. [flip_automatico.py:32](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/flip_automatico.py:32), [views.py:3459](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/views.py:3459).
- **Templates globais:** há leitura direta de texto em templates de chamados; isso não prova que recebam o objeto pelos consumidores específicos de ficha, prévia ou calendário. [modal_fio.html:219](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:219), [_lote_preview.html:24](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_lote_preview.html:24).
- **Continua aberto:** censo exaustivo de escritores e leitores dos campos, cruzamento completo com F3/F5/F6/F9 e consumidores nas duas cascas. Falta percorrer esses grafos e confrontá-los com os briefs respectivos; não se pode concluir ausência a partir dos sítios acima. **[nao verificado]**

alcance nao verificado restante: **[nao verificado]** — não fiz a contagem exaustiva dos pontos abertos.

## 4 Perguntas de lei

- V legada com resposta sintética posterior continua validada? **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 8: sim.** O mesmo corte pede medir perguntas e batidas contra a árvore pousada; essa medida permanece **[nao verificado]**. [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509).
- Instante sem texto ou texto sem instante conta como resposta? **DECIDIDO PELA LEI L-130: não**; o juiz exige instante e texto não vazio. [LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151), [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544).
- A dúvida sobre substituir o texto corrente pela foto do último ato foi respondida no anexo `matriz.md`, seção **RESPOSTAS-1711-1005 item 4**, pela opção A: leitores e SILÊNCIO seguem o texto corrente; a foto fica como histórico. Não localizei esse corte nos três arquivos de leis pesquisados; a linha de um registro na árvore permanece **[nao verificado]**. O SILÊNCIO atual copia o texto projetado em [veredito_celula.py:197](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:197).

Não identifiquei pergunta de lei restante entre essas três, mas **não concluí a revisão de todas as premissas**; o número abaixo não certifica que a fatia inteira foi esgotada. [nao verificado]

perguntas de lei: 0

## 18/11 09:06 · sessao 1219370d65e6 · ESCALA-NAO-DESCREVE · censo · origem=Code

**O censo ainda não está fechado:** a cópia permite fechar os pontos sobre reexports e o escopo dos escritores do vínculo, mas não comprova a identidade do hash nem a exaustividade de acessos dinâmicos e de todos os leitores dos nomes em dicionários. [não verificado]

**P1 — leitor dinâmico: aberto quanto à exaustividade.**

- `serie(chave, escopo='frota', escopo_id=None)` aceita qualquer chave como argumento, inclusive `esmeril_espelho`, e devolve objetos `MetricaSnapshot` inteiros. Na busca por chamadas e imports de `serie` em `app/` e `bin/`, não encontrei chamador; chamadas montadas dinamicamente permanecem [não verificado]. [app/inteligencia/metricas.py:62](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/metricas.py:62)
- Os leitores parametrizados encontrados com conjuntos **restritos** não recebem essa chave nas chamadas mostradas: `_snap` recebe `fabricante_estado` ou `pacotes_caidos`; a iteração da saúde usa `preenchimento_espelho` e `preenchimento_marcos`; e o vigia de cadastro futuro itera suas quatro constantes `celula_futura_*`. [app/api/views_mensageria.py:324](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:324), [app/inteligencia/management/commands/lavrar_fabrica.py:21](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/management/commands/lavrar_fabrica.py:21), [app/api/views_mensageria.py:597](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:597), [app/ponto/services/vigia_cadastro_futuro.py:6](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/vigia_cadastro_futuro.py:6), [app/ponto/services/vigia_cadastro_futuro.py:111](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/vigia_cadastro_futuro.py:111)
- O `admin.py` de inteligência está vazio de registros, e a busca por registro explícito não encontrou `MetricaSnapshot`; registro indireto permanece [não verificado]. Não identifiquei export genérico do modelo na busca feita; exaustividade permanece [não verificado]. A sombra lê **literalmente** `esmeril_espelho` e serializa o detalhe inteiro. [app/inteligencia/admin.py:2](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/admin.py:2), [bin/sombra.sh:138](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra.sh:138)

**P2 — reexports: fecha para a busca estática.** Os dois `__init__.py` de serviços existem e estão vazios; a busca nos `__init__.py` de `app/` não encontrou referências aos módulos, e a busca em `app/` e `bin/` não encontrou os imports relativos especificados. Reexport construído dinamicamente permanece [não verificado]. [app/ponto/services/__init__.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/__init__.py), [app/escala/services/__init__.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/__init__.py)

**P3 — escritores globais: fecha para o escopo declarado da obra.** Para censar *novos caminhos de escrita introduzidos pela obra*, basta seguir a chamada da porta e seus efeitos: a lista lê o vínculo vigente e copia seus dados para as linhas; a realocação encaminha ao juiz e ao executor existentes, que fecham ou alteram vigências. O censo global continua necessário se a pergunta for concorrência, interferência de outros caminhos ou exclusividade de escrita desses campos; essas propriedades globais permanecem [não verificado]. [app/escala/services/cadastro_realidade.py:748](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:748), [app/escala/regua_defesa.py:291](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/regua_defesa.py:291), [app/colaboradores/services/vinculo.py:211](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/vinculo.py:211)

**P4 — amostra confere; identidade do HEAD não.** Os trechos existem e correspondem ao descrito: o esmeril só insere colaborador e calcula destino quando `sig` é verdadeiro; a lista lê a lavra, monta linhas, agrupa em `postos` e retorna os campos citados. Isso comprova o conteúdo da amostra, não que a cópia seja o archive do hash informado: identidade do HEAD **[não verificado]**. [app/ponto/services/esmeril_espelho.py:206](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:206), [app/escala/services/cadastro_realidade.py:872](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:872)

**N1 — colisões em dicionários.** Há homônimos com significados diferentes; os acessos encontrados usam dicionários diferentes do detalhe superior da lavra:

- `orfas`: listas de horas no `dia` ou na `ata`, lidas, por exemplo, pelo esmeril, cartório, propositor, juiz e grade; há também `out['orfas']` como contador do reconciliador. [app/ponto/services/esmeril_espelho.py:125](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:125), [app/ponto/services/cartorio.py:736](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:736), [app/ponto/propositor.py:39](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/propositor.py:39), [app/ponto/juiz_batida.py:161](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/juiz_batida.py:161), [app/escala/utils.py:799](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:799), [app/chamados/reconciliador.py:1420](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1420)
- `proposta`: o resultado de `propor_escala` contém uma proposta individual, consumida por `_destino`, escala automática, PDF e comando; o chat lê outra proposta em `dados`, e o comando de ritmo lê proposta em uma linha de vínculo. [app/ponto/services/esmeril_espelho.py:234](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:234), [app/escala/services/escala_auto.py:117](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/escala_auto.py:117), [app/escala/proposta_pdf.py:75](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/proposta_pdf.py:75), [app/escala/management/commands/propor_escala_limbo.py:39](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/management/commands/propor_escala_limbo.py:39), [app/core/views_ia.py:124](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/views_ia.py:124), [app/escala/management/commands/ritmo_discordante.py:17](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/management/commands/ritmo_discordante.py:17)
- A busca textual por `orfas_desde` em código, templates e shell não encontrou ocorrência; leitores com nome construído dinamicamente e a identificação exaustiva de **todo** acesso a `orfas` e `proposta` em qualquer dict permanecem [não verificado]. Acrescentar as três chaves ao detalhe muda seu MD5; a reposição da sombra também usa o tamanho do detalhe como `valor`. [app/ponto/services/esmeril_espelho.py:260](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:260), [bin/sombra.sh:155](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra.sh:155)

**N2 — parâmetro `medida`:** os dois chamadores locais passam três argumentos posicionais; `escala_certa` passa os mesmos três posicionais e `so_escala=True` por palavra-chave. Acrescentar `medida` depois de `so_escala` preserva essas chamadas identificadas; chamadas dinâmicas permanecem [não verificado]. [app/ponto/services/esmeril_espelho.py:79](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:79), [app/ponto/services/esmeril_espelho.py:195](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:195), [app/ponto/services/esmeril_espelho.py:220](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:220), [app/escala/services/escala_certa.py:42](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/escala_certa.py:42)

**N3 — novos campos da lista.** Nenhum dos nomes propostos aparece no dict atual de `_vinculos` ou no retorno de `lista`; `linha = dict(v, ...)` propagaria também o novo `ciclo`. Nos consumidores e testes examinados, encontrei acesso por campos e iteração de `postos`, não comparação ou serialização integral da linha ou do retorno. A ausência desses usos em **todos** os leitores e testes permanece [não verificado]. [app/escala/services/cadastro_realidade.py:754](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:754), [app/escala/services/cadastro_realidade.py:887](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:887), [app/escala/services/cadastro_realidade.py:956](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:956), [app/escala/tests/test_cadastro_x_realidade.py:133](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/tests/test_cadastro_x_realidade.py:133), [app/escala/tests/test_e4_parte4_cinco_copias.py:99](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/tests/test_e4_parte4_cinco_copias.py:99)

**N4 — rota, view e selos.** O nome e o caminho propostos não aparecem nas rotas atuais de escala, e a busca pelo nome da view não encontrou definição existente. [app/escala/urls.py:6](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/urls.py:6)

Há varreduras relevantes: o selo de sessão percorre rotas e tenta POST quando GET retorna 405; `portas_do_codigo` descobre views POST por inspeção e alimenta o mapa de portas, sem exigir cadastro manual no trecho examinado; o selo do diagrama compara os artefatos gerados. A prova de casca também percorre os URLconfs para verificar imports. Outros selos ou mapas de permissão que possam exigir cadastro permanecem [não verificado]. [app/core/tests/test_rota_pede_sessao.py:235](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/tests/test_rota_pede_sessao.py:235), [app/core/portas_da_tela.py:103](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/portas_da_tela.py:103), [app/core/management/commands/gerar_diagrama.py:17](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/management/commands/gerar_diagrama.py:17), [bin/prova_de_casca.py:72](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/prova_de_casca.py:72)

censo fechado: nao -- faltam exaustividade dos acessos dinâmicos e dos leitores de nomes em dicts, verificação integral dos consumidores da lista e dos selos de rota, e comprovação independente da identidade do hash [não verificado].

## 18/11 09:05 · sessao 6a83051f4826 · ESPURIA-TELA · leitura · origem=Code

**Os três sítios são consumidores posteriores**, segundo as cadeias de chamadas encontradas na cópia. Isso não significa que toda consulta à precedência seja posterior: `fatos_do_dia` também é chamado dentro da cadeia de `julgar_celula`, inclusive durante a lavra e na emissão de chamado ([cartorio.py:2041](app/ponto/services/cartorio.py#L2041), [cartorio.py:1858](app/ponto/services/cartorio.py#L1858), [cartorio.py:1792](app/ponto/services/cartorio.py#L1792)).

O HEAD informado, `dd489c627`, está **[não verificado]**: a cópia aberta não contém metadados Git que permitam conferi-lo.

### S1 — `veredito_do_dia`

**Consumidor.** A linha 376 normaliza `fatos`; a leitura de `tipo_do_dia` ocorre efetivamente na linha 384. A função traduz fatos e decisões recebidos em uma palavra visual ([dia_decidido.py:337](app/ponto/services/dia_decidido.py#L337), [dia_decidido.py:376](app/ponto/services/dia_decidido.py#L376), [dia_decidido.py:384](app/ponto/services/dia_decidido.py#L384)).

O único chamador direto de produção encontrado para `veredito_do_dia` é `do_dia`, em [dia_decidido.py:421](app/ponto/services/dia_decidido.py#L421). Os **dois** chamadores diretos de produção de `do_dia` são:

- `aplicar_palavra_do_dia`, em [espelho.py:340](app/ponto/services/espelho.py#L340). Uma cadeia de tela é [ponto/views.py:161](app/ponto/views.py#L161) → [espelho.py:1291](app/ponto/services/espelho.py#L1291) → `aplicar_palavra_do_dia` → `do_dia` → `veredito_do_dia`. Há também a entrada de PDF em [pdf_espelho.py:638](app/relatorios/pdf_espelho.py#L638).
- `contexto_calendario`, em [calendario.py:748](app/colaboradores/services/calendario.py#L748). Uma cadeia de tela é [colaboradores/views.py:1990](app/colaboradores/views.py#L1990) → `contexto_calendario` → `do_dia` → `veredito_do_dia`.

O calendário também é usado pela porta de tarefa: [tarefa_do_dia.py:619](app/chamados/services/tarefa_do_dia.py#L619) → [calendario.py:157](app/colaboradores/services/calendario.py#L157) → [calendario.py:748](app/colaboradores/services/calendario.py#L748). Esse caminho é condicionado à ausência de rota e consulta uma tarefa que já recebeu a lavra; portanto, a descrição precisa é **consumidor posterior**, não exclusivamente tela/PDF ([tarefa_do_dia.py:588](app/chamados/services/tarefa_do_dia.py#L588), [tarefa_do_dia.py:611](app/chamados/services/tarefa_do_dia.py#L611)).

### S2 — `status_dos_dias_lavrados`

**Consumidor.** Ele lê a célula e devolve o status lavrado quando disponível; nos ramos sem lavra e sem batida, consulta a precedência ou usa fatos já fornecidos ([status_dia.py:272](app/ponto/status_dia.py#L272), [status_dia.py:286](app/ponto/status_dia.py#L286), [status_dia.py:296](app/ponto/status_dia.py#L296)). A linha 250 é o import; as chamadas são as linhas 298 e 304 ([status_dia.py:250](app/ponto/status_dia.py#L250), [status_dia.py:298](app/ponto/status_dia.py#L298), [status_dia.py:304](app/ponto/status_dia.py#L304)).

Todos os chamadores diretos de produção encontrados são:

| Chamador | Cadeia até uma entrada consumidora |
|---|---|
| `status_do_dia_lavrado` em [status_dia.py:218](app/ponto/status_dia.py#L218) | É o invólucro de um par; não encontrei chamada de produção dele na cópia. |
| `contexto_calendario` em [calendario.py:639](app/colaboradores/services/calendario.py#L639) | [colaboradores/views.py:1990](app/colaboradores/views.py#L1990) → calendário; também há a cadeia posterior pela tarefa descrita em S1. |
| `montar_semanas` em [espelho.py:170](app/ponto/services/espelho.py#L170) | [ponto/views.py:187](app/ponto/views.py#L187) → `montar_semanas`. |
| `_d4_em_lote` em [painel_op.py:336](app/core/services/painel_op.py#L336) | [core/views.py:607](app/core/views.py#L607) → [painel_op.py:236](app/core/services/painel_op.py#L236) → `_d4_em_lote`. |
| `api_espelho_v2` em [api/views.py:1490](app/api/views.py#L1490) | A própria view é a entrada em [api/views.py:1327](app/api/views.py#L1327). |
| `dataset_furos` em [furos.py:127](app/relatorios/furos.py#L127) | [relatorios/views.py:1509](app/relatorios/views.py#L1509) → `dataset_furos`; outra entrada está em [relatorios/views.py:1543](app/relatorios/views.py#L1543). |

**Distinção necessária:** a função vizinha `fatos_do_status` não é esse leitor. Ela consulta `fatos_do_dia` condicionalmente e é chamada pelo cartório para fornecer fatos à lavra ([status_dia.py:166](app/ponto/status_dia.py#L166), [status_dia.py:177](app/ponto/status_dia.py#L177), [cartorio.py:1854](app/ponto/services/cartorio.py#L1854)).

### S3 — `dias_com_furo`

**Consumidor.** Ele primeiro identifica lâmpadas apagadas na ata e depois consulta a precedência para selecionar os dias de trabalho ([sem_furo.py:20](app/folha/services/sem_furo.py#L20), [sem_furo.py:25](app/folha/services/sem_furo.py#L25)).

O único chamador direto de produção encontrado é `colabs_sem_furo`, em [sem_furo.py:51](app/folha/services/sem_furo.py#L51). A cadeia completa até a entrada é [bin/placar_code.sh:385](bin/placar_code.sh#L385) → [command `colabs_sem_furo`:13](app/folha/management/commands/colabs_sem_furo.py#L13) → `colabs_sem_furo` → `dias_com_furo`; o comando imprime o contador ([colabs_sem_furo.py:20](app/folha/management/commands/colabs_sem_furo.py#L20)).

### Chamadores de produção da precedência

A lista abaixo contém as **chamadas diretas** encontradas, incluindo a chamada interna de `fatos_do_dia` para `fatos_do_periodo`; imports e referências apenas em comentários não são contados.

| Função chamada | Chamadores diretos de produção |
|---|---|
| `fatos_do_dia` | `fatos_do_status` ([status_dia.py:181](app/ponto/status_dia.py#L181)); `_fato_do_dia` ([semana_do_posto.py:177](app/escala/services/semana_do_posto.py#L177)); `_dia_de_descanso` ([situacao.py:94](app/ferias/situacao.py#L94)); `espurios` ([feriado_reavaliar.py:34](app/chamados/services/feriado_reavaliar.py#L34)); `apurar` ([deslogados.py:166](app/ponto/services/deslogados.py#L166)); `com_nomes` ([abriu_nao_bateu.py:123](app/ponto/services/abriu_nao_bateu.py#L123)); `faltas_de_hoje_sem_cobranca` ([faltas_de_hoje.py:61](app/ponto/services/faltas_de_hoje.py#L61)); `dia_do_colab` ([dia_do_colab.py:15](app/ponto/services/dia_do_colab.py#L15)); `aplicar_precedencia_na_celula` ([cartorio.py:2000](app/ponto/services/cartorio.py#L2000)); `Command._abrir_chamado` ([detectar_ausencias.py:617](app/ponto/management/commands/detectar_ausencias.py#L617)). |
| `fatos_do_periodo` | `fatos_do_dia` ([precedencia.py:256](app/ponto/precedencia.py#L256)); `status_dos_dias_lavrados`, em dois ramos ([status_dia.py:298](app/ponto/status_dia.py#L298), [status_dia.py:304](app/ponto/status_dia.py#L304)); `montar_dias`, pelo alias `_fatos_periodo` ([espelho.py:648](app/ponto/services/espelho.py#L648)); `contexto_calendario` ([calendario.py:539](app/colaboradores/services/calendario.py#L539)); `dias_com_furo` ([sem_furo.py:25](app/folha/services/sem_furo.py#L25)). |

**Sim, há chamadas dentro da cadeia de `julgar_celula`:**

1. **Durante a lavra:** [julgar_celula:2041](app/ponto/services/cartorio.py#L2041) → `julgar_colab` → [_julgar_colab_corpo:996](app/ponto/services/cartorio.py#L996) → [`fatos_do_status`:1858](app/ponto/services/cartorio.py#L1858) → [`fatos_do_dia`:181](app/ponto/status_dia.py#L181) → [`fatos_do_periodo`:256](app/ponto/precedencia.py#L256). A consulta em `fatos_do_status` ocorre quando o veredito é `indefinida` ([status_dia.py:177](app/ponto/status_dia.py#L177)).
2. **No funil de emissão:** `julgar_celula` → `julgar_colab` → `_julgar_colab_corpo` → [`_abrir_chamado`:1792](app/ponto/services/cartorio.py#L1792), também chamado em [cartorio.py:1830](app/ponto/services/cartorio.py#L1830) → [`fatos_do_dia`:617](app/ponto/management/commands/detectar_ausencias.py#L617) → `fatos_do_periodo`.
3. **Depois do julgamento, ainda dentro de `julgar_celula`:** [cartorio.py:2049](app/ponto/services/cartorio.py#L2049) → [`aplicar_precedencia_na_celula`:2000](app/ponto/services/cartorio.py#L2000) → `fatos_do_dia` → `fatos_do_periodo`. O próprio código situa essa gravação depois de `julgar_colab` ([cartorio.py:2044](app/ponto/services/cartorio.py#L2044)).

O registro vivo identifica `fatos_do_dia` como autoridade de precedência e de feriado que suprime ([juizes.py:196](app/core/juizes.py#L196), [juizes.py:312](app/core/juizes.py#L312)).

## 18/11 09:04 · sessao a89b9ddeaa16 · - · - · origem=Ronald (tmux)

S1 é consumidor de apresentação: `veredito_do_dia` lê `tipo_do_dia` em [dia_decidido.py:384](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:384) e produz um rótulo para o dia; a linha 376 apenas normaliza `fatos` ([dia_decidido.py:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:376)).

O único chamador direto de produção de `veredito_do_dia` é `do_dia` ([dia_decidido.py:421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:421)). Os dois chamadores diretos de produção de `do_dia` são `aplicar_palavra_do_dia` ([espelho.py:340](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:340)) e `contexto_calendario` ([calendario.py:748](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:748)).

Cadeias até entradas consumidoras:

- Tela: [ponto/views.py:161](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/views.py:161) → `espelho_do_colab` → [espelho.py:1291](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:1291) → `aplicar_palavra_do_dia` → [espelho.py:340](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:340) → `do_dia` → [dia_decidido.py:421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:421) → S1.
- App: [api/views.py:1361](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:1361) → `espelho_do_colab` → mesma cadeia ([espelho.py:1291](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:1291)).
- Calendário: [colaboradores/views.py:1990](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/views.py:1990) → `contexto_calendario` → [calendario.py:748](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:748) → `do_dia` → [dia_decidido.py:421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:421) → S1.
- PDF: [pdf_espelho.py:598](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/pdf_espelho.py:598) chama o espelho; [pdf_espelho.py:638](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/pdf_espelho.py:638) chama diretamente `aplicar_palavra_do_dia`.

O calendário também é consultado pela tarefa depois de `_tarefa_pura` e da leitura da lavra ([tarefa_do_dia.py:588](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:588), [tarefa_do_dia.py:619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:619), [calendario.py:157](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:157)).

## 18/11 09:04 · sessao 12ac5374c6e9 · - · - · origem=Ronald (tmux)

S3 é **consumidor**: `dias_com_furo` lê lâmpadas apagadas da ata em [sem_furo.py:20](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/services/sem_furo.py:20), consulta a precedência em [sem_furo.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/services/sem_furo.py:25) e seleciona dias de trabalho em [sem_furo.py:26](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/services/sem_furo.py:26). O registro vivo o classifica como TELA em [juizes.py:821](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/juizes.py:821).

A cadeia de produção encontrada é:

`bin/placar_code.sh:385` → `folha/management/commands/colabs_sem_furo.py:13` → `folha/services/sem_furo.py:51` → `dias_com_furo`.

Esses são os únicos chamadores de produção encontrados para `colabs_sem_furo` e `dias_com_furo`, respectivamente. O comando imprime o contador em [colabs_sem_furo.py:20](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/management/commands/colabs_sem_furo.py:20).

## 18/11 09:04 · sessao 4e6f31bf2190 · [nome]-NAO-DESFAZ-O-PAPEL · censo · origem=Code

**Resultado:** os emissores identificados da grade estão em `escala/utils.py`; há uma sobra no builder por turno que não está expressamente incluída nas três situações de P3 descritas no contrato. A procedência anexada a uma subclasse de `str` pode sobreviver às cópias rasas e ordenações da grade, mas é perdida na construção da ata por `str(h)`. [app/escala/utils.py:1845; app/ponto/services/cartorio.py:745]

## 1. Emissores

| Sítio e função | Produtor | Item emitido |
|---|---|---|
| [app/escala/utils.py:631] e [app/escala/utils.py:673] — `_match_marcos` | P1 | Duas alternativas de alinhamento; ambas criam novas strings com `strftime`. As chamadas definitivas alimentam os builders em [app/escala/utils.py:787] e [app/escala/utils.py:1712]. |
| [app/escala/utils.py:810] — `montar_grade_prevista` | P2 | Sobra não consumida, arquivada pela data civil; cria string com `strftime`. |
| [app/escala/utils.py:1733] — `montar_grade_prevista_periodo_por_turno` | P3 | Ponta de override sem coluna; insere o objeto `hora` da célula. |
| [app/escala/utils.py:1744] — mesma função | P3 | Marco interno excedente aos slots I; insere o objeto `hora` da célula. |
| [app/escala/utils.py:1843] — mesma função | P3 | Sobra de intervalo sem slot I livre; cria string com `strftime`. |
| [app/escala/utils.py:1845] — mesma função | **Outro, pela definição fornecida** | Sobra não consumida que não entra no ramo de intervalo; cria string com `strftime` e arquiva pela data civil. Ela se parece com P2, mas P2 foi definido para o builder de um vínculo. |

O script fornecido **viu todos esses sítios**. Seus resultados em `ponto/turnos.py` não são emissão na lista `orfas` de um dia da grade: ali a lista local contém instantes e alimenta um mapa de papéis com valor `'X'`. [app/ponto/turnos.py:1732; app/ponto/turnos.py:1743; app/ponto/turnos.py:1787; app/ponto/turnos.py:1804]

Emissor adicional em outro módulo de produção que monte a grade/dia: **[não verificado]**. A busca textual e o AST fornecido não demonstram exaustividade para aliases ou construção indireta. [app/escala/utils.py:799; app/escala/utils.py:1822]

## 2. Mutação e transporte

- `_match_marcos` cria cada item com `strftime`; o builder de um vínculo entrega a própria lista ao dia. No builder por turno, `orfas_por_data[d] = orfas` mantém a lista e `list(...)` cria outra lista, preservando as referências aos itens. [app/escala/utils.py:631; app/escala/utils.py:673; app/escala/utils.py:799; app/escala/utils.py:1769; app/escala/utils.py:1822]
- Os `append` de sobras criam novas strings; os dois `append` de células inserem o objeto já guardado em `_cc['hora']`. `sorted` cria outra lista, preservando os objetos dos itens. [app/escala/utils.py:810; app/escala/utils.py:814; app/escala/utils.py:1733; app/escala/utils.py:1744; app/escala/utils.py:1843; app/escala/utils.py:1845; app/escala/utils.py:1849]
- O ramo intermitente inicializa `orfas_por_data[d]` com `[]`; isso é inicialização, não recriação de item já emitido. [app/escala/utils.py:1578]
- Folga lançada, falta aprovada, ausência integral e justificativa substituem a lista por `[]`: os itens deixam de integrar o dia. Ausência parcial pode alterar células sem limpar a lista. A isenção altera células e flags, mas mantém as órfãs. [app/escala/utils.py:1160; app/escala/utils.py:1224; app/escala/utils.py:1255; app/escala/utils.py:1262; app/escala/utils.py:1269; app/escala/utils.py:823; app/escala/utils.py:904]
- Antes da reconciliação, a ata temporária do juiz recebe uma cópia rasa da lista. A reconciliação procura uma nova string `strftime` por igualdade e remove o primeiro item igual; os demais itens sobrevivem. A célula preenchida é criada novamente, sem transportar o objeto removido. [app/escala/utils.py:1897; app/escala/utils.py:1906; app/escala/utils.py:2016; app/escala/utils.py:2050]
- No legacy, o builder de cada segmento fornece o dia; a montagem final reutiliza a lista `sd.get('orfas', [])`, sem recriar seus itens. O dispatcher escolhe entre esse caminho e o builder por turno. [app/escala/utils.py:1027; app/escala/utils.py:1032; app/escala/utils.py:1054; app/escala/utils.py:2095]
- `clipar_grade_janela` substitui a lista por `[]` em dias futuros ou anteriores ao piso; esse clip é chamado pelo wrapper **depois** do retorno do builder. `montar_realizado_grade` também entrega uma cópia rasa da lista. [app/escala/utils.py:962; app/escala/utils.py:973; app/escala/utils.py:977; app/escala/utils.py:1121; app/escala/utils.py:2348]

## 3. Chamadas auxiliares de `_match_marcos`

- **Dry-run S155:** `_orf2` vira `_orfset`, usado para selecionar candidatas internas; a lista retornada pelo dry-run não é colocada diretamente num dia. A decisão pode influenciar os marcos usados na chamada definitiva. [app/escala/utils.py:1660; app/escala/utils.py:1662; app/escala/utils.py:1684; app/escala/utils.py:1712]
- **`marco_ocupado_pela_batida`:** recebe `_orfas` e usa apenas as células para devolver rótulo e delta; não encaminha a lista. [app/escala/utils.py:427; app/escala/utils.py:429]
- **Detector de anomalias:** recebe `_orfas` e procura a batida nas células para classificar o resultado; não encaminha a lista. [app/chamados/detector_anomalias.py:212; app/chamados/detector_anomalias.py:214]
- **Motor v2:** recebe `_orfas_z5` e usa as células para relacionar marcos a períodos e aplicar tolerância; não encaminha a lista. [app/ponto/motor_calculo_v2.py:3062; app/ponto/motor_calculo_v2.py:3063]

## 4. Leitores e efeito de uma subclasse de `str`

Leitores identificados que podem receber o `dia` ou suas órfãs em memória:

- **Dentro da montagem e apresentação:** cópia para o juiz e reconciliação em [app/escala/utils.py:1903] e [app/escala/utils.py:1979]; extração para realizado em [app/escala/utils.py:1121]; calendário em [app/colaboradores/services/calendario.py:409]; PDF em [app/relatorios/pdf_espelho.py:477] e [app/relatorios/pdf_espelho.py:552].
- **Classificação e julgamento:** classificador em [app/escala/orfa_classe.py:53]; furos em [app/escala/services/furos_diarios.py:22]; causas da folha em [app/folha/causas.py:43] e [app/folha/causas.py:100]; propositor em [app/ponto/propositor.py:34]; bordas em [app/ponto/services/bordas_realizado.py:49]; status em [app/ponto/status_dia.py:188]; supra-juiz em [app/ponto/supra_juiz.py:392] e [app/ponto/supra_juiz.py:527]; horas reais para ausência parcial em [app/ponto/turnos.py:2658]; simulação em [app/ponto/services/simular_dia.py:11]; score do comando em [app/ponto/management/commands/detectar_par_relampago.py:136].
- **Cartório:** lâmpadas, contagem e ata em [app/ponto/services/cartorio.py:628], [app/ponto/services/cartorio.py:736] e [app/ponto/services/cartorio.py:745]; contagem para julgamento em [app/ponto/services/cartorio.py:1424].

Há leitores adicionais da **ata já construída**, que não devem ser confundidos com leitores do objeto original da grade: por exemplo [app/escala/services/leitor_celula.py:187], [app/chamados/juizes.py:514] e [app/ponto/services/cluster_espurio.py:29].

Para uma subclasse comum de `str` que mantenha igualdade, hash e conversão textual normais:

- `isinstance(h, str)` no cartório aceita a subclasse; o propositor também usa `isinstance`, por meio de `_tem_hora`. Uma busca limitada ao nome `orfa` não encontra essas duas verificações dos itens. [app/ponto/services/cartorio.py:628; app/ponto/propositor.py:24; app/ponto/propositor.py:52]
- `str(h)` na ata produz texto sem o atributo extra; portanto a procedência não chega pela lista de órfãs à ata entregue à lavratura. `horas_reais_do_dia` também converte os itens para `str`, mas só para seu cálculo de minutos. [app/ponto/services/cartorio.py:745; app/ponto/services/cartorio.py:1852; app/ponto/turnos.py:2679]
- A **impressão do cartório** não serializa o `dia` inteiro: compõe partes dos insumos e calcula SHA-256; do dia, o caminho mostrado extrai o estado de cobertura parcial. A mera adição do atributo à órfã não entra diretamente nessa impressão. [app/ponto/services/cartorio.py:455; app/ponto/services/cartorio.py:471; app/ponto/services/cartorio.py:480; app/ponto/services/cartorio.py:556; app/ponto/services/cartorio.py:1398]
- A reconciliação compara uma lista derivada de órfãs com uma lista contendo a hora e usa igualdade para `remove`; o classificador usa a hora como chave de dicionário, de modo que itens com a mesma hora não recebem chaves distintas por produtor. [app/escala/utils.py:2016; app/escala/utils.py:2043; app/escala/utils.py:2050; app/escala/orfa_classe.py:74]

Comparação por `type(x) is str`, serialização integral por outro caminho, cache ou comparação integral de dia/lista em **todos** os leitores transitivos: **[não verificado]**. Consequentemente, não é demonstrável aqui que nenhum outro leitor mude de comportamento; subclasses que sobrescrevam igualdade, hash, conversão ou outras operações também exigiriam análise própria. [app/ponto/propositor.py:24; app/ponto/services/cartorio.py:745]

## 5. Distância a marco vazio

Não identifiquei uma função de produção que receba a órfã e o dia e responda à pergunta completa “existe marco **missing** a até N minutos”; a inexistência em toda a produção é **[não verificado]**. `classificar_orfas` verifica se há *algum* missing e classifica por agrupamento de órfãs, sem medir a distância de cada uma ao marco vazio. [app/escala/orfa_classe.py:35; app/escala/orfa_classe.py:68; app/escala/orfa_classe.py:74]

As primitivas existentes são:

- `distancia_do_marco`: distância circular pela hora do dia, em minutos, preservando fração; aceita `time`, `datetime` ou número de minutos, não diretamente a string `HH:MM`. [app/escala/regua_defesa.py:395; app/escala/regua_defesa.py:412; app/escala/regua_defesa.py:422]
- `perto_do_marco`: compara essa distância com limite inclusivo, cujo padrão é `RAIO_DO_MARCO_MIN = 90`. O chamador ainda precisa escolher os marcos correspondentes a células missing e converter a hora da órfã. [app/escala/regua_defesa.py:392; app/escala/regua_defesa.py:442]
- `turnos._dist_marco`: menor distância de um `datetime` a uma lista de horários, considerando dia anterior, mesmo dia e dia seguinte; não filtra células missing. [app/ponto/turnos.py:326]
- `marco_ocupado_pela_batida`: responde qual marco o pareamento ocupou e seu delta, uma pergunta diferente de proximidade a marco vazio. [app/escala/utils.py:387; app/escala/utils.py:427]

## 6. Fechamento

**censo fechado: nao -- falta verificar exaustivamente emissores e leitores indiretos por aliases, serialização/cache e comparações integrais dos dias ou listas, além dos efeitos de uma subclasse concreta de str.** [não verificado]

## 18/11 09:03 · sessao ee80ede8516e · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

Os números de `brief` e `matriz` abaixo são os dos textos anexados, contados desde seus títulos; a correspondência com arquivos originais separados é [nao verificado]. A unidade de contagem é a mesma da remedida anexada: **45 premissas**, inclusive as que reúnem ocorrências repetidas. “CAIU” pode significar que caiu o **suporte ou resultado pressuposto**, embora o sítio continue existindo.

| Nº | Premissa e sítio no brief/matriz | Juízo na cópia dd489c627 |
|---:|---|---|
| 1 | F1 fornece `LeituraDosAtos`; brief:76–78, matriz:14–18 | **CAIU**: a F1 disponível expõe o juiz booleano sobre instante e texto da projeção. `app/chamados/juizes.py:1544` |
| 2 | Redutor F1 limpa resposta e validação por ato; matriz:19–20 | **CAIU como interface disponível**; o juiz presente lê o par projetado. `app/chamados/juizes.py:1552` |
| 3 | Conservação sem DP é `_leitura_conservada`; brief:78,83, matriz:21–22 | **CAIU nesse formato**: a conservação encontrada compara ato e instante da projeção. `app/chamados/models.py:4132` |
| 4 | Faces SQL de resposta consultam atos; brief:93–94, matriz:23–25 | **CAIU**: as faces consultam campos projetados. `app/chamados/juizes.py:1565`, `app/chamados/juizes.py:1575` |
| 5 | Espera fica metade projeção, metade ato; brief:20, matriz:35 | **CAIU**: seus componentes são projetivos. `app/chamados/services/fio.py:187`, `app/chamados/juizes.py:1531` |
| 6 | F2 já fornece `estado` por ato; matriz:26–28 | **CAIU**: `estado` usa V/C e a property de respondida, agora delegada ao juiz projetivo. `app/chamados/models.py:3152`, `app/chamados/models.py:3674` |
| 7 | POST usa a antiga leitura F1; brief:21, matriz:36 | **CAIU a interface pressuposta**; o POST agora chama o juiz. `app/chamados/services/fio.py:708` |
| 8 | Canônica usa texto do redutor e aguarda R3; brief:22, matriz:37 | **CAIU como dependência atual**: usa o juiz do par. `app/chamados/services/fio.py:1283` |
| 9 | Mérito perdedor migra pelo redutor F1; brief:22, matriz:38 | **CAIU a interface**; usa o juiz do par. `app/chamados/services/fio.py:1296` |
| 10 | Lavra sem turno recebe texto da leitura; brief:23, matriz:39 | **CAIU o suporte pressuposto**; a guarda passou ao juiz e conserva C. `app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:368` |
| 11 | Universo absolvível dispõe da face V por ato da F2 antiga; brief:24, matriz:40 | **CAIU como suporte pousado**; o SQL ainda filtra V projetada. `app/chamados/reconciliador.py:674` |
| 12 | Órfãs abertas consumirão o redutor F1; brief:25, matriz:41 | **CAIU o suporte**; o filtro é C/V, não R/C como diz a matriz. `app/chamados/reconciliador.py:1093` |
| 13 | Veredito de órfã recebe texto da leitura F1; brief:26, matriz:42 | **CAIU a interface**; agora chama o juiz do par. `app/chamados/reconciliador.py:1127` |
| 14 | Fotografias da trilha usam `LeituraDosAtos`; brief:27, matriz:43 | **CAIU o contrato pressuposto**; as fotos ainda usam campos da pergunta. `app/chamados/reconciliador.py:1189`, `app/chamados/reconciliador.py:1199` |
| 15 | C em SQL permanece projetada; brief:28,94–95, matriz:44,90 | **CONFERE**: reatáveis filtram C e o enum não contém ato MATERIALIZACAO. `app/chamados/reconciliador.py:1549`, `app/chamados/models.py:4037` |
| 16 | Explicador recebe resposta/estado pelo ato; brief:29, matriz:45 | **CAIU como implementação presente**; lê texto e `estado` da pergunta. `app/chamados/services/explicador.py:102`, `app/chamados/models.py:3152` |
| 17 | Validação dispõe do instante do redutor/face V antiga; brief:30,87, matriz:46 | **CAIU o suporte pressuposto**; a guarda ainda lê V projetada. `app/chamados/services/declaracao_texto.py:227` |
| 18 | Declaração compara última RESPOSTA do redutor; brief:31, matriz:47 | **CAIU o contrato**; compara texto projetado e chama o juiz. `app/chamados/services/declaracao_texto.py:418` |
| 19 | Aplicação da declaração dispõe do redutor; brief:32, matriz:48 | **CAIU a interface**; a guarda passou ao juiz. `app/chamados/services/declaracao_texto.py:688` |
| 20 | Causa `c` do vigia lê R cru; brief:33, matriz:49 | **CAIU**: agora chama o juiz. `app/chamados/services/vigia_fio.py:636` |
| 21 | Vigia de resposta vazia mede a projeção; brief:34,52–54, matriz:50,89 | **CONFERE**: conta o par projetado; escolha de migrar ou aposentar ainda é [nao verificado]. `app/chamados/services/vigia_fio.py:738`, `app/chamados/services/vigia_fio.py:747` |
| 22 | Resposta vazia na partição pelo antigo RED-2; brief:85,105–108, matriz:51 | **CAIU como contrato antigo de redutor**. O código atual coloca o meio par pendente; o corte específico posterior diz fora do app. A relação entre os dois exige a pergunta 1 da seção 4. `app/chamados/services/pergunta_no_app.py:82`, `app/docs/CORTES.md:509` |
| 23 | Beco dispõe da leitura por ato F1; brief:37, matriz:52 | **CAIU a interface**; o beco usa o juiz. `app/chamados/services/becos.py:31` |
| 24 | F1 fornece accessor do histórico por atos; brief:38, matriz:53,85 | **CAIU**: histórico emite pelos campos atuais. `app/chamados/services/historico.py:99` |
| 25 | Vigia carimbo consulta V pela face F2 antiga; brief:39,87, matriz:54 | **CAIU como suporte presente**; filtra V projetada. `app/chamados/services/vigia_carimbo.py:98` |
| 26 | `_pela_decisao` já lê decisões; brief:46,78, matriz:55 | **CONFERE** para exigibilidade, com seus ramos e limites próprios. `app/chamados/services/tarefa_do_dia.py:142`, `app/chamados/services/tarefa_do_dia.py:158` |
| 27 | Ordenação DP é `(ato_em, pk)`; brief:76, matriz:73 | **CONFERE**. `app/chamados/models.py:4082` |
| 28 | C não tem ato próprio e fica conservada; brief:89,94–95, matriz:76 | **CONFERE** para o enum e os leitores C examinados. `app/chamados/models.py:4037`, `app/chamados/reconciliador.py:1549` |
| 29 | Arquivos F5 não persistem diretamente R/T/V/C no censo antigo; brief:64–72 | **CONFERE quanto aos sítios examinados**: declarador e lavra chamam portas. Cobertura de nomes dinâmicos é [nao verificado]. `app/chamados/services/declaracao_texto.py:688`, `app/chamados/reconciliador.py:366` |
| 30 | Inventário antigo esgota escritores e atos cobrem todas as escritas; brief:64–72 | **CAIU**: há nascimento, carimbo com resposta e portas de conservação a considerar. `app/chamados/models.py:2938`, `app/chamados/models.py:3222`, `app/chamados/models.py:4132` |
| 31 | Furo sem canal e competência trancada não têm migração própria R/T/V/C identificada; brief:45–46, matriz:57–58 | **CONFERE nos corpos examinados**. `app/chamados/services/furo_sem_canal.py:94`, `app/chamados/services/competencia_trancada.py:276` |
| 32 | Tarefa, cobrança e cascas herdam a partição; brief:41–46, matriz:55–56,94–95 | **MOVEU**: tarefa em `app/chamados/services/tarefa_do_dia.py:536`; cobrança em `app/chamados/services/cobranca.py:48`; cascas em `app/core/views.py:253` e `app/api/views.py:998`. |
| 33 | F/VIA/X ficam fora da migração descrita; brief:47–50, matriz:60 | **CONFERE como separação dos campos**, não como prova de que todo consumidor externo está coberto. `app/chamados/services/explicador.py:108`, `app/chamados/services/becos.py:27` |
| 34 | Guarda conservada é `recusa_respondida_sem_texto`, unilateral; brief:50, matriz:60 | **CAIU**: a guarda é simétrica, `recusa_par_ilegal`. `app/chamados/services/fio.py:1701` |
| 35 | R3 histórica ainda precisa pousar; brief:126,154–155, matriz:6–9 | **CAIU segundo o registro local**; ancestralidade Git independente é [nao verificado]. `app/docs/RELATO.md:235` |
| 36 | F1 antiga é a entrega ainda aguardada; brief:127,154–155, matriz:6–7 | **CAIU**: o contrato real é outro. `app/chamados/juizes.py:1544` |
| 37 | F2 antiga já oferece V/estado por ato; brief:128, matriz:108 | **CAIU como descrição do código presente**; P1 adiciona marcas, não o redutor antigo. `app/chamados/models.py:4065`, `app/chamados/reconciliador.py:674` |
| 38 | BUG-PAR ainda está fora do main; brief:129, matriz:7 | **CAIU segundo o registro local**; ancestralidade Git é [nao verificado]. `app/docs/RELATO.md:32` |
| 39 | BUG-PAR cruza fio e explicador; brief:129–132 | **CONFERE como alcance funcional presente**: testemunha e explicação continuam nos dois. `app/chamados/services/fio.py:377`, `app/chamados/services/explicador.py:124` |
| 40 | Censo antigo e offsets bastam; brief:12–15,58–62, matriz:8–10 | **CAIU**: há o consumidor `projecao_do_app` e leitores alterados pelo juiz. `app/chamados/services/pergunta_no_app.py:101`, `app/chamados/services/raiox.py:147` |
| 41 | F5 tem zero questões de lei depois do corte; brief:136–138, matriz:98–102,110–111 | **CAIU como conclusão completa desta remedida**: o corte responde à pergunta antiga, mas o ponto silêncio/absorção não determina sozinho a partição proposta. Ver seção 4. `app/docs/CORTES.md:509`, `app/chamados/services/pergunta_no_app.py:82` |
| 42 | Cobertura da mudança de badge pode ser presumida sem conferir contrato público; brief:150–152 | **CAIU como dispensa de conferência**: D05 conserva fases públicas e torna exigibilidade aditiva. O texto literal do aval antigo é [nao verificado]. `app/docs/BACKLOG.md:2137` |
| 43 | Ordem histórica é a fila atual completa; brief:132,154–155, matriz:92–93 | **CAIU**: B2a tem alcance limitado e B2b permanece aberta no código. `app/chamados/services/fio.py:1400` |
| 44 | Aproximação histórica é medição atual pela função real; brief:139–146, matriz:77–78 | **CAIU como medida atual**: o juiz real é outro; números atuais e comparação pelo fold antigo são [nao verificado]. `app/chamados/juizes.py:1544` |
| 45 | Rito operacional de suíte/deploy e cascas; brief:156–157 | **CONFERE como regra escrita**, não como execução nesta leitura. `CLAUDE.md:90`, `CLAUDE.md:95` | [nao verificado]

**CONFERE 10, MOVEU 1, CAIU 34.**

O raio-X do brief #16, que a numeração da matriz distribui de outro modo, já chama o juiz para classificar resposta e ainda usa C na seleção; não é uma migração para o redutor antigo. `app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:147`, `app/chamados/services/raiox.py:182`

## 2 O que mudou

O delta abaixo foi lido no **anexo `diff_F5.patch` fornecido**, de `fbc5c76ce` para `dd489c627`; a cópia foi informada sem `.git`, portanto a ancestralidade e uma segunda comparação Git independente são [nao verificado]. O `diffstat_F5.txt` registra 25 arquivos alterados nesse recorte. [nao verificado: arquivo físico do anexo]

- **Juiz e leitores de resposta.** Nasceram `q_nao_respondida` e a delegação da property `respondida`; POST, canônica, mérito, lavra sem turno, veredito de órfã, beco, declarador, partição e causa `c` passaram a perguntar ao juiz do par. Isso elimina os REDs antigos que dependiam apenas de leitura crua nesses degraus, mas não instala leitura da sequência de atos. `app/chamados/juizes.py:1583`, `app/chamados/models.py:3674`, `app/chamados/services/fio.py:708`, `app/chamados/services/fio.py:1283`, `app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:1127`, `app/chamados/services/becos.py:31`, `app/chamados/services/declaracao_texto.py:688`, `app/chamados/services/pergunta_no_app.py:82`, `app/chamados/services/vigia_fio.py:636`
- **App e duas cascas.** A partição passou a incluir como pendente o instante com texto vazio, e o teste foi invertido para esse resultado; a API também passou a usar o juiz na lista de respondidas e no envio. A mudança alcança a consulta consumida pelo core e pela API e a contagem/badge de `projecao_do_app`. A divergência com o corte específico fica na seção 4. `app/chamados/tests/test_o261e_f5_alcance.py:184`, `app/api/views.py:2578`, `app/api/views.py:2622`, `app/core/views.py:253`, `app/api/views.py:998`, `app/chamados/services/pergunta_no_app.py:101`
- **Novos leitores e guardas fora dos 14 arquivos nominais.** Validação e recusa consultam o juiz; materialização usa uma guarda de anuência nos três tipos e seleciona as duas faces SQL nomeadas. `estado` e consumidores da property também passam pelo juiz. `app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:901`, `app/chamados/services/materializacao.py:1269`, `app/chamados/services/materializacao.py:1342`, `app/chamados/services/materializacao.py:1577`, `app/chamados/services/materializacao.py:1661`, `app/chamados/services/materializacao.py:2091`, `app/chamados/models.py:3152`
- **Escritores e marcas novas.** A porta `devolver_instante_sem_texto_l130` fotografa o anterior e zera o par pela porta de lote. P1 adiciona `da_ressuscitacao` e `resposta_mantem_validacao`: resposta humana declara `False`, automática e pela trilha declaram `True`, assim como a fotografia da projeção. A carga da tarefa lê a primeira marca antes de escolher o último ato; o código declara o escritor `True` para P2 e não mostra leitor da segunda neste pouso. `app/chamados/services/validacao.py:408`, `app/chamados/models.py:4065`, `app/chamados/services/disputa_emissao.py:1527`, `app/chamados/services/disputa_emissao.py:1681`, `app/chamados/services/disputa_emissao.py:1703`, `app/chamados/models.py:4143`, `app/chamados/services/tarefa_do_dia.py:421`
- **Cerca e alcance.** As portas `save` e lote consultam o veto dentro de ato cercado; retratação, costura HIST, beco e resolver abrem esse escopo. A própria documentação limita B2a a esses quatro abridores e deixa B2b aberta; não se pode estender essa proteção aos demais sítios por inferência. `app/chamados/models.py:3398`, `app/chamados/models.py:3563`, `app/chamados/services/fio.py:1389`, `app/chamados/reconciliador.py:553`, `app/chamados/reconciliador.py:1701`, `app/chamados/services/becos.py:65`, `app/chamados/services/disputa_emissao.py:1912`
- **Demais efeitos do patch.** O caminho `reabrir_fantasma` foi retirado; a catraca passou a declarar `PAROU_EM = 0` e os testes tocados ajustaram fixtures e expectativas para o juiz. O patch também contém baixa do pendente de feriado intermitente em `core/juizes`, sem relação demonstrada com a decisão F5. Execução dos testes nesta leitura: [nao verificado]. `app/chamados/models.py:3429`, `app/chamados/tests/test_e3_catraca_respondedores.py:54`, `app/chamados/tests/test_o261e_f5_alcance.py:184`, `app/core/juizes.py:492`

## 3 Nao verificado de ALCANCE

1. **Silêncio/absorção — escritores e leitores fechados; resultado proposto no app não fechado.** O item é a linha **SILENCIO / ABSORCAO do brief:90**, repetida no contraexemplo do **brief:118**. `DisputaSupervisao.fechar` escreve SILENCIO ao encerrar pela porta; a materialização escreve ABSORCAO com referências ao escape e à ausência. A carga lê decisões ordenadas e `_pela_decisao` lê os dois atos: silêncio retorna lista vazia; absorção depende do status da ausência. A partição atual, por sua vez, lê juiz de resposta e fechamento da disputa, sem ler diretamente esses atos. Portanto, essas referências **fecham quem escreve e quem lê**, mas não provam a transposição “silêncio = histórico; absorção rejeitada = pendente” para toda entrada da partição: essa escolha está formulada na pergunta 2 da seção 4. `app/chamados/models.py:2007`, `app/chamados/models.py:2013`, `app/chamados/services/materializacao.py:1046`, `app/chamados/services/materializacao.py:1048`, `app/chamados/services/tarefa_do_dia.py:421`, `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/services/pergunta_no_app.py:82`
2. **Cascas e consumidor omitido — fechado para os sítios identificados.** Core e API chamam `consulta_do_app`; aviso e cobrança também; tarefa usa a partição e `projecao_do_app` calcula contagem e badge. A existência de outros consumidores por importação dinâmica permanece [nao verificado]. `app/core/views.py:253`, `app/api/views.py:998`, `app/api/views.py:2538`, `app/chamados/services/aviso_pendencia.py:9`, `app/chamados/services/cobranca.py:48`, `app/chamados/services/tarefa_do_dia.py:535`, `app/chamados/services/pergunta_no_app.py:101`
3. **Arquivos cruzados com F3/F6/F8/F9 — aberto.** Seus briefs não constam dos dados fornecidos; faltam as listas e os diffs dessas fatias para julgar interseções. Os sítios comuns de F5 efetivamente existentes incluem partição, tarefa e autoridade compartilhada. `app/chamados/services/pergunta_no_app.py:76`, `app/chamados/services/tarefa_do_dia.py:142`, `app/chamados/juizes.py:1544`
4. **Inventário exaustivo de escritores e leitores — aberto.** A remedida anexada lista consumidores estáticos e templates, mas cobertura de nomes construídos dinamicamente e de toda chamada transitiva é [nao verificado]. Exemplos de portas e leitores efetivamente encontrados são `registrar`, `carimbar`, tarefa e histórico. `app/chamados/models.py:4152`, `app/chamados/models.py:3222`, `app/chamados/services/tarefa_do_dia.py:421`, `app/chamados/services/historico.py:69`

**alcance nao verificado restante: 2** — itens 3 e 4. O comportamento normativo do item 1 está convertido em pergunta de lei na seção 4.

## 4 Perguntas de lei

A busca por respostas escritas foi feita em `app/docs/LEIS.md`, `app/docs/CORTES.md` e `app/docs/DOSSIES.md`. Foram encontradas estas decisões pertinentes:

- **DECIDIDO PELA LEI L-130:** respondida exige instante e texto; o juiz é único também para o app. `app/docs/LEIS.md:151`
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 5:** o corte específico da F5 diz resposta vazia fora do app e ação do admin; registra, sem decidir, a questão geral da devolução ao colaborador. `app/docs/CORTES.md:509`
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 3:** na F4, M9b e M10 ficam no carimbo e o predicado de absorção pertence à E4d. Isso não escreve, por si, a tabela de partição pedida pelo brief F5. `app/docs/CORTES.md:509`
- **DECIDIDO PELA LEI LU-16:** silêncio não fecha; só fato na célula. A linha encontrada trata do chamado e não demonstra, sozinha, a classificação de toda pergunta com ato SILENCIO na partição do app. `app/docs/CORTES.md:495`

As perguntas abaixo permanecem quando se confrontam essas respostas com os casos específicos:

1. Para a F5, o corte específico posterior que mantém **instante com texto vazio fora do app e ação do admin** prevalece nesse caso, ou a regra geral L-130 aplicada pela B2 coloca o meio par pendente no app? O código e o teste atuais implementam a segunda resposta; o corte específico escreve a primeira. `app/docs/CORTES.md:509`, `app/docs/LEIS.md:151`, `app/chamados/services/pergunta_no_app.py:82`, `app/chamados/tests/test_o261e_f5_alcance.py:184`
2. **Brief:90 e brief:118:** ao definir a partição F5, SILENCIO deve significar histórico e ABSORCAO rejeitada deve significar pendente, como propõe o brief, inclusive quando o par projetado e o estado da disputa apontarem outra classificação? A tarefa já lê os atos, mas a partição atual usa outros campos; o corte anexado de silêncio para a tarefa não declara essa equivalência para o app. `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/services/tarefa_do_dia.py:178`, `app/chamados/services/pergunta_no_app.py:82`
3. A resposta vazia deve voltar ao colaborador na ressuscitação? O próprio corte registra essa questão como não decidida. `app/docs/CORTES.md:509`, `app/docs/LEIS.md:151`
4. Como preservar e tratar, na ressuscitação, texto sem instante e sem validação, quando não há linha correspondente que o guarde? O universo atual o exclui e identifica a regra da trilha como ainda a decidir. `app/chamados/models.py:1541`, `app/chamados/models.py:4132`

perguntas de lei: 4

## 18/11 09:03 · sessao e769aa64755a · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Remedida parcial:** confirmei os sítios abaixo, mas não concluí o julgamento de todas as premissas do brief e dos 49 degraus da matriz. As contagens ao fim desta seção valem somente para as 12 linhas da tabela, não para o conjunto pedido. [nao verificado]

`M` = [materializacao.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py), `D` = [disputa_emissao.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py), `J` = [juizes.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py). As linhas de `brief` e `matriz` abaixo são as do texto fornecido; sua correspondência com arquivos externos à cópia é [nao verificado].

| Premissa e sítio no anexo | Veredito na cópia |
|---|---|
| Brief:106–107; matriz:22–28: F1 fornece `leitura()`/D01 com decisões e instante | **CAIU.** O juiz pousado `pergunta_respondida` responde pelo instante e texto da projeção. [J:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544) |
| Matriz:91, J1: o juiz envolve a pergunta com texto de D01 | **CAIU.** `_slot_esperado` passa a pergunta ao derivador. [J:345](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:345), [J:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:376) |
| Brief:115–118; matriz:110: três chamadas bastam para a paridade | **CAIU.** Há também duas chamadas na composição do par. [M:214](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:214), [M:222](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:222), [J:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:376) |
| Matriz:40, M7: testemunha lê a resposta e chama o derivador | **MOVEU** para [M:852](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:852); a condição de resposta agora consulta o juiz em [M:871](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:871). |
| Matriz:43, M9b: texto da irmã entra no ato ABSORCAO | **MOVEU** para [M:1050](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1050). |
| Matriz:44, M10: absorvidas são agrupadas pelo carimbo e texto | **MOVEU** para [M:1088](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1088), [M:1112](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1112). |
| Matriz:64, E1: resposta humana grava projeção e ato | **CONFERE** a escrita da projeção em [D:1481](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1481) e do ato em [D:1525](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1525). |
| Matriz:66, E3: nova resposta limpa validação anterior | **CONFERE** em [D:1484](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1484). |
| Matriz:68, E5: resposta chama `reabrir_por_resposta` | **CONFERE** em [D:1499](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1499). |
| Matriz:74, E11: automática preserva texto já existente | **MOVEU** para [D:1664](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1664). |
| Matriz:77, E14: guarda do reparo exige texto sem instante | **MOVEU** para [D:1694](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1694). |
| Matriz:43, M9b: ninguém lê ABSORCAO | **CAIU.** A tarefa lê a decisão de absorção. [tarefa_do_dia.py:178](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:178) |

CONFERE 3, MOVEU 5, CAIU 4 — **somente nos 12 julgamentos acima; total solicitado [nao verificado]**.

## 2 O que mudou

Li o **diff real fornecido em `diff_F4.patch`**. Como a cópia não tem `.git`, a comparação independente dos hashes e a verificação de que o patch é completo continuam **[nao verificado]**. O diffstat fornecido situa o patch entre `520d7b557` e `dd489c627`; ele não é, por si só, o diff desde `77dfa94fa`. [nao verificado]

- **Juiz e leitores de resposta.** A B2 acrescentou a face SQL negativa `q_nao_respondida`; recobrança, credor, validação, recusa e leitores do fio passaram a consultar as faces do juiz ou `pergunta_respondida`. Isso muda o tratamento de meio par: instante sozinho não equivale a resposta completa. [J:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583), [D:1737](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1737), [D:1846](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1846), [validacao.py:681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:681)
- **Materialização.** A guarda comum de anuência passou a ser chamada nos ramos hora, data-hora e escolha. A fila inclui a face “instante sem texto” para que essa guarda produza recusa com dono, além da face respondida. O texto usado pelo derivador continua vindo da projeção. [M:1269](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1269), [M:1342](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1342), [M:1577](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1577), [M:1661](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1661), [M:2092](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2092), [M:678](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:678)
- **Atos F2.** O patch acrescenta `da_ressuscitacao` e `resposta_mantem_validacao`. A resposta humana declara `False`; automática, reparo pela trilha e foto da projeção declaram `True`. A carga da tarefa lê o predicado da ressuscitação. [models.py:4067](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4067), [D:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527), [D:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681), [D:1703](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1703), [models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143), [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427)
- **Cerca de escrita.** O patch acrescenta a vigilância na porta de `save` e no lote e envolve o resolver em ato cercado. O próprio código limita o julgamento aos atos cercados e registra a B2b como aberta para os demais sítios. [models.py:3394](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3394), [models.py:3559](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3559), [D:1901](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1901), [fio.py:1389](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1389)
- **Outros trechos do patch.** O backfill passou a carregar `respondida_em` para a escolha canônica; os testes fornecidos no patch ajustam expectativas e fixtures à B2. A alteração de `core/juizes.py` registra a baixa da pendência de feriado intermitente; seu efeito sobre a F4 [nao verificado]. [fio.py:1276](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1276), [test_estado_aplicacao.py:35](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_estado_aplicacao.py:35), [core/juizes.py:489](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/juizes.py:489)

O censo completo de **todos** os escritores e leitores novos dos campos e funções da fatia nesta árvore permanece **[nao verificado]**; os sítios acima são os confirmados nesta leitura.

## 3 Nao verificado de ALCANCE

1. **Chamadores do derivador:** foram encontrados cinco sítios: as duas metades do par, a testemunha, o materializador e o juiz. A igualdade dos dados efetivamente passados em todos os cenários continua **[nao verificado]**. [M:214](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:214), [M:222](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:222), [M:898](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:898), [M:1348](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1348), [J:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:376)
2. **Cascas:** a casca administrativa recebe `estado_aplicacao` pelo fio e o modal o lê; o cartão do colaborador também lê a resposta projetada. O censo completo dos leitores nas duas cascas e de seus caminhos cruzados continua **[nao verificado]**. [fio.py:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:376), [modal_fio.html:230](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:230), [_card_atendimento.html:106](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_card_atendimento.html:106)
3. **Censo transversal:** falta reconferir integralmente escritores, leitores, chamadas indiretas e arquivos cruzados da lista da remedida anterior contra esta árvore. O predicado de resposta e as portas de escrita já mostram por que o censo antigo, isoladamente, não fecha esse alcance. [J:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [models.py:3559](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3559)

**alcance nao verificado restante: 3 grupos identificados; total de sítios [nao verificado].**

## 4 Perguntas de lei

- **DECIDIDO PELA LEI CORTES-F2-F9-1611, itens 3 e 4:** absorção M9b/M10 permanece no carimbo nesta F4; guardas de reparo permanecem na projeção pela allowlist. [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509)
- **DECIDIDO PELA LEI L-130:** respondida exige instante e texto após trim. Esse predicado existe na árvore; ele não comprova a existência do D01 antigo. [LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151), [J:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544)
- **RESPONDIDO no dado fornecido:** as duas condições U163 indicadas no pedido não são perguntas de lei. A4 também exige ausência de qualquer batida do colaborador no instante exato, além da ausência de batida do mesmo tipo na janela. O registro da resposta ao instante exato aparece em [RELATO.md:304](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/RELATO.md:304); a resposta posterior sobre a janela é dado fornecido **[nao verificado]** independentemente.

A busca nos três arquivos de lei foi feita para os temas acima, mas a revisão incompleta de todas as premissas impede afirmar que não exista outra pergunta de lei na fatia. **A contagem seguinte é somente das perguntas remanescentes identificadas nesta leitura parcial; o total da fatia permanece [nao verificado].**

perguntas de lei: 0

## 18/11 09:02 · sessao 52bc9e82cad0 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Resultado parcial:** a B2 alterou várias guardas de resposta para o juiz L-130, mas esse juiz lê o par projetado e não fornece a antiga API `atos_pergunta.leitura`; as guardas de validação, os placares e os marcadores citados ainda têm leituras projetivas. (`app/chamados/juizes.py:1544`, `app/chamados/services/validacao.py:682`, `app/chamados/services/validacao.py:685`, `app/chamados/services/verbos.py:264`, `app/chamados/services/painel.py:67`, `app/chamados/veredito_lavrado.py:203`)

**Limite da contagem:** julguei abaixo os 21 degraus D1–D21 da matriz e 12 premissas adicionais do brief. Isso **não é uma contagem de todas as afirmações e ocorrências** dos dois anexos: a decomposição integral, inclusive cada prova e cada ocorrência repetida de `[DEPENDE-F1]`, permanece **[nao verificado]**. As linhas de `brief.md` e `matriz.md` são as do texto fornecido, numeradas a partir do título; sua correspondência com arquivos externos é **[nao verificado]**.

**Critério:** CONFERE = a premissa comportamental permanece; MOVEU = o sítio ou mecanismo mudou, com a equivalência indicada; CAIU = a premissa ou o contrato proposto não pode ser reutilizado como descrito. A mudança de número de linha, por si só, não significa mudança de comportamento.

| Unidade e origem | Veredito e sítio em dd489c627 |
|---|---|
| D1 — `matriz.md:16–21`, `brief.md:57` | **CAIU** a leitura do ato proposta. A guarda agora usa `pergunta_respondida`, sobre instante **e** texto projetados. (`app/chamados/services/validacao.py:681`, `app/chamados/juizes.py:1552`) |
| D2 — `matriz.md:24–26`, `brief.md:58` | **MOVEU** a guarda de V para `validacao.py:685`; continua projetiva. A disponibilidade do fold antigo **CAIU**. (`app/chamados/services/validacao.py:685`, `app/chamados/juizes.py:1544`) |
| D3 — `matriz.md:29–31`, `brief.md:59` | **MOVEU** para `validacao.py:898`; continua lendo V projetada. A leitura do fold **CAIU**. (`app/chamados/services/validacao.py:898`) |
| D4 — `matriz.md:33–35`, `brief.md:59` | **CAIU** a leitura de ato proposta: a guarda usa agora o juiz do par projetado. (`app/chamados/services/validacao.py:901`, `app/chamados/juizes.py:1552`) |
| D5 — `matriz.md:37–38`, `brief.md:16–18,60` | **CONFERE** a saída fora do fato antes da escrita; o sítio **MOVEU**. (`app/chamados/services/validacao.py:906`, `app/chamados/services/validacao.py:922`) |
| D6 — `matriz.md:40–42`, `brief.md:61` | **CONFERE** a leitura conservada de materialização; a afirmação de que a F1 disponível entrega C pelo parâmetro do fold **CAIU**. (`app/chamados/services/validacao.py:769`, `app/chamados/juizes.py:1390`) |
| D7 — `matriz.md:44–47` | **MOVEU**: o lote ainda lê V nos dois pontos, fora da tabela original do brief. (`app/chamados/services/validacao.py:1064`, `app/chamados/services/validacao.py:1085`) |
| D8 — `matriz.md:49–51`, `brief.md:67` | **MOVEU**: classificação ainda lê texto projetado; o fold proposto não está demonstrado. (`app/chamados/services/validacao.py:1183`) |
| D9 — `matriz.md:53–55`, `brief.md:67` | **MOVEU**: o helper continua comparando o texto projetado. (`app/chamados/services/validacao.py:465`, `app/chamados/services/validacao.py:467`) |
| D10 — `matriz.md:57–59`, `brief.md:67` | **MOVEU**: a idade continua usando `respondida_em` projetada. (`app/chamados/services/validacao.py:1256`) |
| D11 — `matriz.md:69–73`, `brief.md:62,75–78` | **MOVEU** o sítio; o placar ainda conta V corrente e exige staff, não linhas DP. (`app/chamados/services/verbos.py:264`, `app/chamados/services/verbos.py:265`) |
| D12 — `matriz.md:75–81`, `brief.md:63,93` | **CAIU** a descrição atual como filtro somente por instante: B2 acrescentou a face SQL de instante e texto. A contagem por DP proposta ainda não está no leitor. (`app/chamados/services/verbos.py:333`, `app/chamados/services/verbos.py:337`, `app/chamados/services/verbos.py:344`) |
| D13 — `matriz.md:83–88`, `brief.md:64,79–84` | **CONFERE** que o marcador agrega projeções e que D06 ainda é trabalho proposto; continua a restrição de duas queries no teste. (`app/chamados/services/painel.py:50`, `app/chamados/services/painel.py:67`, `app/chamados/tests/test_selo_cadeia_placar.py:72`) |
| D14 — `matriz.md:90` | **CONFERE** que a ETag consome o marcador; a função começa em outra linha. (`app/chamados/services/painel.py:79`, `app/chamados/services/painel.py:91`) |
| D15 — `matriz.md:92–96`, `brief.md:62,89–92` | **CONFERE** a divergência de predicado: painel não aplica o filtro staff do placar. (`app/chamados/services/painel.py:261`, `app/chamados/services/painel.py:270`, `app/chamados/services/verbos.py:265`) |
| D16 — `matriz.md:101–102`, `brief.md:66` | **CAIU** a descrição do count: usa a negação SQL do juiz e conta perguntas da disputa; corrigíveis são consultadas separadamente. (`app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/acoes_disputa.py:99`) |
| D17 — `matriz.md:103–105`, `brief.md:66` | **CAIU** a leitura exclusiva de instante: a guarda usa o juiz do par projetado. `resolucao` no POST e fechamento da disputa continuam distintos dos campos da pergunta. (`app/chamados/services/acoes_disputa.py:125`, `app/chamados/services/acoes_disputa.py:134`, `app/chamados/services/acoes_disputa.py:259`) |
| D18 — `matriz.md:106–108`, `brief.md:66` | **MOVEU**: correção ainda lê V e texto projetados; fechamento é da disputa. (`app/chamados/services/acoes_disputa.py:349`, `app/chamados/services/acoes_disputa.py:351`, `app/chamados/services/acoes_disputa.py:357`) |
| D19 — `matriz.md:110–113`, `brief.md:65` | **CONFERE** o agregado temporal de C/R com `ate`; a proposta de marcador DP ainda não está demonstrada no corpo. (`app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:163`, `app/chamados/veredito_lavrado.py:203`, `app/chamados/veredito_lavrado.py:207`) |
| D20 — `matriz.md:61–62`, `brief.md:19–21,68` | **MOVEU**: conferência continua usando os campos próprios. Implementação integral de LU-08: **[nao verificado]**. (`app/chamados/services/validacao.py:994`, `app/chamados/services/validacao.py:996`, `app/docs/CORTES.md:466`) |
| D21 — `matriz.md:64–67`, `brief.md:70` | **CONFERE** que validação e devolução escrevem projeção e ato; a devolução também lê C, motivo e V. (`app/chamados/services/validacao.py:168`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:262`, `app/chamados/services/validacao.py:279`) |

Premissas adicionais, contadas uma vez cada:

| Origem | Veredito |
|---|---|
| `brief.md:10–15`; `matriz.md:3–5` — transportar offsets antigos | **CAIU** para a cópia: validar agora começa em `validacao.py:661`. (`app/chamados/services/validacao.py:661`) |
| `brief.md:22` — `recusa_motivo` da ação é POST | **CONFERE**. (`app/chamados/services/acoes_disputa.py:83`) |
| `brief.md:32–39` — lista de escritores como censo completo | **CAIU** como inventário completo: há, por exemplo, a devolução L-130 e a porta em lote. (`app/chamados/services/validacao.py:440`, `app/chamados/models.py:3371`) |
| `brief.md:38` — `registrar` como porta de gravação DP | **CONFERE** na porta identificada; completude para escritas dinâmicas: **[nao verificado]**. (`app/chamados/models.py:4152`) |
| `brief.md:40–45`; `matriz.md:123–125` — carimbo pode gravar V sem VALIDACAO | **CONFERE** no corpo de `carimbar`; a quantidade atual do furo é **[nao verificado]**. (`app/chamados/models.py:3222`, `app/chamados/models.py:3253`) |
| `brief.md:46` — nenhum leitor F3 é sem janela | **CAIU** como afirmação geral: `marcos_do_lote` aceita `ate=None` e o marcador agrega o acervo. (`app/chamados/veredito_lavrado.py:151`, `app/chamados/services/painel.py:66`) |
| `brief.md:47–50` — consumidores dos marcos e marcador | **MOVEU** em parte: raio X, fio e tarefa continuam chamando os marcos; a ETag chama o marcador. (`app/chamados/services/raiox.py:38`, `app/chamados/services/fio.py:282`, `app/chamados/services/tarefa_do_dia.py:440`, `app/chamados/services/painel.py:91`) |
| `brief.md:85–88,129`; `matriz.md:9,128–129,141` — antiga F1 libera o fold e os REDs com projeção vazia | **CAIU** como contrato disponível: L-130 exige instante e texto projetados. (`app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1552`, `app/docs/LEIS.md:151`) |
| `brief.md:98–101,129` — BUG-PAR ainda precisa pousar | **CAIU** como estado atual: os corpos e a tradução da mensagem já estão na árvore. (`app/chamados/services/validacao.py:308`, `app/chamados/services/validacao.py:640`, `app/chamados/services/acoes_disputa.py:63`) |
| `brief.md:102` — DEVOLUCAO entra automaticamente em D04/D06 | **CAIU** como comportamento atual: os leitores ainda não contam DP; o D04 descrito conta VALIDACAO, não DEVOLUCAO. (`app/chamados/services/verbos.py:264`, `app/chamados/services/painel.py:67`, `app/docs/BACKLOG.md:2091`) |
| `brief.md:103–105` — ausência de conflito histórico garante ausência de cruzamento atual | **CAIU** como garantia atual: o patch fornecido altera três arquivos centrais da fatia. (`app/chamados/services/validacao.py:682`, `app/chamados/services/acoes_disputa.py:98`, `app/chamados/services/verbos.py:333`) |
| `matriz.md:139–141` — quinze degraus sem prova permanecem censo completo | **CAIU**: há, por exemplo, provas de lote, classes, S116 e corte temporal. Execução delas: **[nao verificado]**. (`app/chamados/tests/test_validar_lote.py:131`, `app/chamados/tests/test_fila_validar_lote.py:42`, `app/chamados/tests/test_guard_s116_detalhe_local.py:52`, `app/escala/tests/test_signal_ancora.py:218`) |

Nas **33 unidades acima**, com classificação principal única por linha: **CONFERE 8, MOVEU 11, CAIU 14**. A contagem integral de todas as premissas dos anexos permanece **[nao verificado]**.

## 2 O que mudou

Li o `diff_F3.patch` fornecido: ele descreve o delta de `fbc5c76ce` para `dd489c627`; a identidade do HEAD e a ancestralidade por Git são **[nao verificado]**, pois a cópia não tem `.git`. O diffstat e os commits são os anexados pelo usuário; não há confirmação independente por Git **[nao verificado]**.

- **Guardas centrais:** o patch troca a verificação somente de instante pelo juiz L-130 em validar, recusar, reabrir e regularizar. Isso muda o tratamento de instante sem texto; não implementa leitura do livro de atos. (`app/chamados/services/validacao.py:682`, `app/chamados/services/validacao.py:902`, `app/chamados/services/acoes_disputa.py:98`, `app/chamados/services/acoes_disputa.py:134`, `app/chamados/juizes.py:1552`)
- **Aceite:** o patch acrescenta `q_respondida_com_texto`; o instante projetado continua definindo janela e comparação com cobrança. O universo deixou de incluir instante sem texto, mas ainda não é consulta de DP RESPOSTA. (`app/chamados/services/verbos.py:333`, `app/chamados/services/verbos.py:337`, `app/chamados/services/verbos.py:344`)
- **Novos escritor e leitores relevantes:** `devolver_instante_sem_texto_l130` conserva a projeção em linhas DP e zera o par pela porta em lote; o predicado de impedimento lê V e C. A cerca passou a ser consultada pela escrita individual e em lote, com alcance limitado aos atos cercados. (`app/chamados/services/validacao.py:381`, `app/chamados/services/validacao.py:438`, `app/chamados/services/validacao.py:440`, `app/chamados/models.py:3397`, `app/chamados/models.py:3562`, `app/chamados/services/fio.py:1394`)
- **Novos campos DP:** escritores de RESPOSTA declaram se ela mantém a validação; a carga da tarefa lê a marca de ressuscitação. O comentário do modelo descreve esta etapa como expand e a marca de ressuscitação como inerte até o próximo passo. (`app/chamados/models.py:4065`, `app/chamados/models.py:4071`, `app/chamados/models.py:4077`, `app/chamados/services/disputa_emissao.py:1527`, `app/chamados/services/disputa_emissao.py:1681`, `app/chamados/services/disputa_emissao.py:1703`, `app/chamados/services/tarefa_do_dia.py:427`)
- **Leitores cruzados novos no delta:** API, fio, materialização, auditoria e propriedade `respondida` passaram a consultar o juiz ou sua face SQL. A materialização acrescentou uma guarda comum de anuência aos três ramos. (`app/api/views.py:2578`, `app/api/views.py:2622`, `app/chamados/services/fio.py:707`, `app/chamados/services/materializacao.py:1269`, `app/chamados/services/materializacao.py:1342`, `app/chamados/services/materializacao.py:1577`, `app/chamados/services/materializacao.py:1661`, `app/chamados/management/commands/auditar_invariantes_chamados.py:176`, `app/chamados/models.py:3674`)
- **Placar e marcos:** o patch não altera `painel.py` nem `veredito_lavrado.py`; seus marcadores continuam com os contratos encontrados na árvore. O teste de duas queries e o retorno temporal dos marcos restringem uma implementação D06: o consumidor compara o marco com `julgado_em`. (`app/chamados/services/painel.py:50`, `app/chamados/tests/test_selo_cadeia_placar.py:72`, `app/chamados/veredito_lavrado.py:152`, `app/chamados/services/raiox.py:46`)

Os números de frota do brief são dados históricos fornecidos; sua atualização e a execução dos testes são **[nao verificado]**. [nao verificado]

## 3 Nao verificado de ALCANCE

- **Duas cascas:** a rota do kernel alcança a validação pela rota de foto e pelo serviço comum; a casca de fio também chama a ação de validar. (`app/config/urls_core.py:6`, `app/api/urls_core.py:8`, `app/api/views_core.py:844`, `app/api/foto_recebida.py:51`, `app/chamados/services/fio.py:656`)
- **Leitores externos:** `classe_do_lote` é consumida pela mensageria; `_e_aceite_de_folga` pelo vigia; `placar_aceite` pelo painel e retrato de folha; o marcador alimenta a ETag e os marcos alimentam raio X, fio e tarefa. (`app/api/views_mensageria.py:545`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/services/painel.py:298`, `app/folha/views_retrato.py:515`, `app/chamados/services/painel.py:91`, `app/chamados/services/raiox.py:38`, `app/chamados/services/fio.py:282`, `app/chamados/services/tarefa_do_dia.py:440`)
- **Testemunha persistida:** o comando obtém o contexto do painel e grava o payload quando aplicado; a chamada ao contexto não é, por si, a escrita. (`app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/management/commands/lavrar_badge_navbar.py:91`)
- **Abertos:** **[nao verificado]** completude de todos os escritores e leitores, inclusive nomes construídos dinamicamente; falta um censo integral reproduzível da árvore. **[nao verificado]** cruzamento efetivo com F5/F6/F8/F9 e futuras edições das outras fatias; faltam seus briefs e diffs. **[nao verificado]** a contagem integral de todas as premissas e ocorrências dos anexos; falta auditá-las uma a uma contra esta árvore. Os sítios confirmados acima não demonstram esses três fechamentos. (`app/chamados/models.py:4152`, `app/chamados/services/validacao.py:682`)

**alcance nao verificado restante: 3**

## 4 Perguntas de lei

Pesquisei `app/docs/LEIS.md`, `app/docs/CORTES.md` e `app/docs/DOSSIES.md` pelos identificadores e termos de validação, devolução, aceite, marcador e ressuscitação; uma resposta fora desses termos é **[nao verificado]**.

- **DECIDIDO PELA LEI L-130:** “respondida” exige instante e texto não vazio; isso responde à pergunta sobre o juiz de resposta atual. Não fornece, por si, o antigo fold de validação corrente. (`app/docs/LEIS.md:151`, `app/chamados/juizes.py:1544`)
- **DECIDIDO PELA LEI SEXTA-NOITE-1311/D04**, conforme o registro encontrado: duas validações com devolução no meio contam duas. O brief já ressalva a possibilidade de contestação da aplicação ao mesmo request; identificar request pelo dado disponível é **[nao verificado]**. (`app/docs/BACKLOG.md:2091`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:279`)
- **Predicado staff:** o brief e a matriz identificam EQUIPE-STAFF-0708 e o placar o aplica; o literal dessa decisão em LEIS/CORTES/DOSSIES, pela busca realizada, é **[nao verificado]**. (`app/chamados/services/verbos.py:265`, `app/chamados/services/painel.py:261`)
- **Autor nulo no aceite:** o contrato fornecido pede resposta após a última cobrança, sem predicado de autor; o leitor atual compara colaborador e instante. A identidade de quem respondeu quando `ato_por` é nulo é **[nao verificado]**. (`matriz.md:75–81`, `matriz.md:131`, `app/chamados/services/verbos.py:328`, `app/chamados/services/verbos.py:344`)
- **Pergunta aberta:** para `marcos_do_lote(ate=...)`, qual resultado deve prevalecer quando uma DP inserida depois da lavra traz `ato_em` anterior ao corte: preservar a fotografia histórica pelo instante do ato ou invalidar a lavra pela inserção retroativa? Os anexos exigem perceber backfill e respeitar `ate`, enquanto o contrato atual retorna um marco temporal; não encontrei uma resposta expressa que resolva essa combinação. (`brief.md:82–83`, `matriz.md:110–113`, `matriz.md:132`, `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:163`, `app/chamados/services/raiox.py:46`)

perguntas de lei: 1

## 18/11 09:02 · sessao 5a5db08b4a70 · E4C-F2-P2 · censo · origem=Code

**O censo não está fechado.** Há leitores fora do TSV e um escritor de `via_resolucao` que executa sem adquirir a trava do colaborador. Não concluí a classificação de todos os leitores, a auditoria de todas as formas dinâmicas nem a prova de alcance simultâneo desse escritor; esses pontos permanecem abertos. [app/chamados/services/passivo_validado.py:64; app/chamados/management/commands/encerrar_perguntas_com_fato.py:65; app/chamados/models.py:3408]

### 1. Escritores e leitores identificados

A lista abaixo é **parcial**: não certifica “todos” os escritores ou leitores pedidos.

- **`validada_em` e `validada_por`:** `PerguntaDisputa.carimbar` pode gravar ambos; `responder_pergunta_disputa_core` pode zerá-los; `_carimbar_validacao`, `devolver_validacao_recusada` e `devolver_paridade_bug_par_103` os gravam; `descarimbar_em_lote` recebe ambos para zerar de `ressuscitar_perguntas` e `reabrir_para_correcao`. `_nascer` também aceita `**campos` na inserção, portanto seus chamadores precisam ser auditados para identificar quais valores efetivamente fornecem. [app/chamados/models.py:3253; app/chamados/services/disputa_emissao.py:1494; app/chamados/services/validacao.py:168; app/chamados/services/validacao.py:273; app/chamados/services/validacao.py:349; app/chamados/models.py:1637; app/chamados/services/disputa_emissao.py:1610; app/chamados/models.py:3132]

- **`respondida_em` e `resposta_colab`:** a resposta humana e a automática gravam o par; `carimbar_resposta_pela_trilha` grava o instante; `carimbar` pode gravar o instante; os três chamadores de `descarimbar_em_lote` podem limpar o par conforme os argumentos passados. A inserção por `_nascer(**campos)` também precisa ser rastreada. [app/chamados/services/disputa_emissao.py:1481; app/chamados/services/disputa_emissao.py:1675; app/chamados/services/disputa_emissao.py:1698; app/chamados/models.py:3250; app/chamados/models.py:1637; app/chamados/services/disputa_emissao.py:1649; app/chamados/services/validacao.py:440; app/chamados/models.py:3132]

- **`materializada_em`, `materializada_por` e `via_resolucao`:** `_escrever_carimbo` atribui os três em memória, e `carimbar`, `carimbar_identidade` e `descarimbar` são portas que os salvam; `descarimbar_em_lote` faz `UPDATE` dos três. Para `via_resolucao`, há ainda `encerrar_por_fato_no_espelho` e `relavrar`. A atribuição de `materializada_em` em `regularizacao_ext` é de `reg`, não está demonstrada como escrita de `PerguntaDisputa`. [app/chamados/models.py:3216; app/chamados/models.py:3271; app/chamados/models.py:3305; app/chamados/models.py:3327; app/chamados/models.py:3404; app/chamados/models.py:3415; app/chamados/services/relavra_via.py:125; app/chamados/services/regularizacao_ext.py:199]

- **`resposta_mantem_validacao`:** a projeção sem linha fornece `True`, a resposta humana fornece `False`, e a automática e a carimbada pela trilha fornecem `True`; `registrar` materializa os dicionários por `bulk_create`. O campo tem default `False`. [app/chamados/models.py:4141; app/chamados/services/disputa_emissao.py:1525; app/chamados/services/disputa_emissao.py:1679; app/chamados/services/disputa_emissao.py:1701; app/chamados/models.py:4162; app/chamados/models.py:4077]

- **`da_ressuscitacao`:** o campo tem default `False` e é lido por `es_da_ressuscitacao`; não encontrei um fornecimento explícito de `True` nas ocorrências examinadas. A migração existente adiciona o campo; isso não prova o conteúdo de produção. [app/chamados/models.py:4071; app/chamados/models.py:4103; app/chamados/migrations/0054_decisao_da_ressuscitacao.py:17]

- **`ato_motivo`:** é fornecido, entre outros, pelas portas de fechamento e remoção de motivo, projeção sem linha, reabertura, resposta automática e pela trilha, devoluções, backfill e veredito da célula; `registrar` grava os dicionários recebidos. Os leitores identificados incluem `rotulo_bug_par_103` e o template do fio. [app/chamados/models.py:2015; app/chamados/models.py:2148; app/chamados/models.py:4142; app/chamados/services/disputa_emissao.py:1635; app/chamados/services/disputa_emissao.py:1680; app/chamados/services/disputa_emissao.py:1702; app/chamados/services/validacao.py:282; app/chamados/services/validacao.py:357; app/chamados/management/commands/backfill_decisoes_pergunta.py:327; app/ponto/services/veredito_celula.py:197; app/chamados/models.py:4162; app/chamados/services/materializacao.py:275; app/templates/chamados/partials/modal_fio.html:212]

As atribuições em `_universo_da_ressuscitacao` modificam uma **cópia em memória** para julgamento; `_desfazer_em_memoria` restaura a instância após recusa. Não devem ser contadas como gravações independentes no banco. [app/chamados/models.py:1548; app/chamados/models.py:1560; app/chamados/models.py:3278]

A classificação completa dos leitores por campo, arquivo e função permanece [nao verificado]. O TSV contém leitores adicionais em juízes, serviços, comandos e API, mas não conclui sozinho os leitores dinâmicos e de templates. [app/chamados/juizes.py:1389; app/chamados/services/painel.py:67; app/api/views.py:1163; app/templates/chamados/partials/modal_fio.html:219]

### 2. Chamadores identificados

Os pontos abaixo são chamadas diretas identificadas; a auditoria completa de referências, apelidos e reexportações permanece aberta.

- `ressuscitar_perguntas`: `reabrir`, `desarquivar_chamado`, `_julgar_colab_corpo`. [app/chamados/models.py:1498; app/chamados/views_arquivar.py:86; app/ponto/services/cartorio.py:1674]
- `_universo_da_ressuscitacao`: `perguntas_a_ressuscitar`, `perguntas_puladas_na_ressuscitacao`, `ressuscitar_perguntas`. [app/chamados/models.py:1507; app/chamados/models.py:1512; app/chamados/models.py:1599]
- `descarimbar_em_lote`: `ressuscitar_perguntas`, `reabrir_para_correcao`, `devolver_instante_sem_texto_l130`. [app/chamados/models.py:1637; app/chamados/services/disputa_emissao.py:1649; app/chamados/services/validacao.py:440]
- `vigiar_escrita_de_perguntas`: `descarimbar_em_lote` e `PerguntaDisputa.save`, com imports locais. [app/chamados/models.py:3398; app/chamados/models.py:3399; app/chamados/models.py:3565; app/chamados/models.py:3566]
- `executar_sob_cerca`: `retratar_com_motivo`, `reatar_pergunta_orfa`, `fechar_beco`, `resolver_perguntas_por_chamado_resolvido`. [app/chamados/reconciliador.py:553; app/chamados/reconciliador.py:1701; app/chamados/services/becos.py:66; app/chamados/services/disputa_emissao.py:1912]
- `es_da_ressuscitacao`: `CargaDaTarefa.__init__`. [app/chamados/services/tarefa_do_dia.py:427]
- `linhas_da_projecao_sem_linha`: `_escoar`, `_executar_dia`, `ressuscitar_perguntas`, `_nascer`, `responder_pergunta_disputa_core`, `responder_automaticamente`, `CargaDaTarefa.__init__`, `_carimbar_validacao`, `devolver_validacao_recusada`, `devolver_paridade_bug_par_103` e `devolver_instante_sem_texto_l130`. [app/chamados/management/commands/backfill_decisoes_pergunta.py:67; app/chamados/management/commands/backfill_fio_morto_e4b.py:141; app/chamados/models.py:1619; app/chamados/models.py:3137; app/chamados/services/disputa_emissao.py:1480; app/chamados/services/disputa_emissao.py:1674; app/chamados/services/tarefa_do_dia.py:436; app/chamados/services/validacao.py:167; app/chamados/services/validacao.py:272; app/chamados/services/validacao.py:345; app/chamados/services/validacao.py:436]

Para `leitura`, `q_validada_pelo_ato`, `q_nao_validada_pelo_ato` e `_resposta_desfaz_validacao`, a identificação do alvo e de seus chamadores na árvore inteira permanece [nao verificado]. O modelo registra que, nesta cópia, o novo campo ainda não é lido; não é suficiente para concluir que uma função genérica chamada `leitura` inexista em toda a árvore. [app/chamados/models.py:4067; app/chamados/models.py:4077]

### 3. Achados que escapam ao AST

- `_reversao_de` lê dinamicamente os nomes em `CAMPOS_REVERSAO`, inclusive `materializada_em`, `materializada_por_id`, `via_resolucao`, `respondida_em`, `validada_em` e `validada_por_id`. O `getattr` recebe a variável `c`, enquanto o script fornecido só reconhece o segundo argumento quando é constante. [app/chamados/services/passivo_validado.py:64; app/chamados/services/passivo_validado.py:81]
- Os templates leem `ato_motivo`, `resposta_colab`, `respondida_em`, `validada_em` e `via_resolucao`; o script fornecido percorre somente arquivos `.py`. [app/templates/chamados/partials/modal_fio.html:212; app/templates/chamados/partials/modal_fio.html:219; app/templates/chamados/partials/modal_fio.html:228; app/templates/chamados/partials/_card_atendimento.html:106; app/templates/chamados/partials/_card_atendimento.html:192; app/templates/chamados/partials/_pergunta_card.html:20]
- A entrada HTML da resposta usa o nome `resposta`, que a view traduz para o argumento do escritor; não é um formulário que submete literalmente `resposta_colab`. [app/templates/chamados/partials/_card_atendimento.html:113; app/chamados/views.py:1265; app/chamados/views.py:1284]

Não concluo zero para SQL cru, nomes concatenados, `values`/`annotate` dinâmicos, serializers, forms, signals e referências indiretas: a busca e a inspeção necessárias para sustentar esse zero permanecem abertas. A busca por forms/serializers inicialmente apontou caminhos que não existem nesta cópia, portanto não sustenta conclusão sobre todos eles. [nao verificado]

### 4. Concorrência sem a trava do colaborador

`encerrar_por_fato_no_espelho` grava `via_resolucao` por `save`; seu chamador de produção encontrado, o comando `encerrar_perguntas_com_fato --apply`, seleciona perguntas materializadas e não validadas, monta `alvo` e depois grava cada uma dentro de `transaction.atomic()`, sem chamar `trava_na_ordem` nesse caminho. O `save` mostrado invoca o vigia e abre um `atomic`, mas não adquire por si a trava do colaborador. [app/chamados/models.py:3408; app/chamados/models.py:3415; app/chamados/management/commands/encerrar_perguntas_com_fato.py:31; app/chamados/management/commands/encerrar_perguntas_com_fato.py:41; app/chamados/management/commands/encerrar_perguntas_com_fato.py:65; app/chamados/models.py:3555]

Há **sobreposição potencial dos filtros estruturais**, mas não demonstrei que uma mesma pergunta satisfaça simultaneamente os dois julgamentos: o comando exige marco aceso, enquanto `_universo_da_ressuscitacao` pula marco aceso. Tampouco demonstrei, pela árvore aberta, a sequência concreta de mudança de marco necessária entre as leituras. Portanto a resposta exigida sobre alcance concorrente da mesma pergunta permanece [nao verificado]. [app/chamados/management/commands/encerrar_perguntas_com_fato.py:40; app/chamados/models.py:1530; app/chamados/models.py:1555]

Na cópia atual, `ressuscitar_perguntas` calcula `ids` antes de adquirir a trava e usa esses IDs depois; a releitura do universo sob a trava foi descrita pelo usuário como mudança pretendida, não como comportamento já presente. [app/chamados/models.py:1599; app/chamados/models.py:1606; app/chamados/models.py:1618]

censo fechado: nao -- leitores e escritores completos, referências indiretas e alcance concorrente da mesma pergunta ainda abertos.

## 18/11 09:01 · sessao faffd1a5cd87 · DIA-SEM-TRABALHO-PORTA-L1 · leitura · origem=Code

A porta atual de **Resolver dia** não representa as quatro decisões pedidas: seu vocabulário é `trabalhou`, `faltou` e `justificado`, e o ramo `trabalhou` recusa a grade cujo `tipo_dia` não seja `trabalho`. A decisão de HE é outra decisão, sobre minutos fora da janela. ([app/ponto/services/veredito_celula.py:18](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:18), [app/ponto/services/veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243), [app/ponto/models.py:1270](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/models.py:1270))

A cópia aberta não tem metadados Git; a identidade do HEAD `dd489c627` está **[não verificado]**. As referências abaixo são aos arquivos efetivamente abertos nessa cópia.

### Matriz de transições

Os estados da primeira coluna são os pedidos para a decisão do admin; quando uma operação existente tem alcance diferente, a tabela descreve esse limite. O brief define pessoa × dia do turno com batida apurável, três saídas e retenção da trilha após mudança do fato. (brief:L3, brief:L29, brief:L30)

| Estado ou transição | Juiz: quem decide | Operação que roda ou pode correr | Persistido | Retorno ou recusa | Prova |
|---|---|---|---|---|---|
| **Ainda sem decisão** | Um juiz da decisão integral desta porta não foi identificado **[não verificado]**. `decisao_do_dia` identifica apenas vereditos humanos de Resolver dia; ausência de `DecisaoHE` vigente responde apenas à HE. | O calendário oferece Resolver dia somente com último recurso, dia passado e status elegível. A view chama `veredito_celula`. | A ata pode conter `tipo_dia`, contagem e horas das órfãs; isso registra fatos, não uma das quatro decisões pedidas. | `decisao_do_dia` devolve `{}` se não acha veredito humano; a oferta depende também de data anterior ao início da competência e ausência de pergunta viva. | [veredito_celula.py:85](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:85), [calendario.py:94](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:94), [calendario.py:117](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:117), [cartorio.py:730](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:730), [views.py:3466](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/views.py:3466) |
| **Sem decisão → trabalho reconhecido** | O admin escolhe `trabalhou` na porta existente; não foi demonstrado juiz que reconheça esse resultado para o caso central **[não verificado]**. | `veredito_celula(..., veredito='trabalhou')`. A autorização de HE é operação separada, `decidir_he`. | Para grade `tipo_dia != trabalho`, o retorno ocorre antes da lavra humana. `decidir_he`, se chamado e aceito, grava `DecisaoHE` e tenta trilha. | Resolver dia retorna `ok=False`, “a escala nao preve trabalho nesse dia”. HE exige permissão, motivo para autorizar e minutos positivos; não confirma, por si, a decisão sobre a batida. | [veredito_celula.py:228](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:228), [veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243), [he.py:159](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:159), [he.py:89](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:89) |
| **Sem decisão → escala a corrigir; correção aplicada** | O admin é autor da edição; `vinculo_do_dia_carregado` fornece o vínculo necessário. Não foi identificada uma decisão persistida “escala a corrigir” nesta porta **[não verificado]**. | A operação existente de correção é `editar_dia(..., trabalha=True)`. | Grava em `CelulaDia` `trabalha`, `dna`, vínculo gerador, origem editada, autor e instante, com trilha; depois aciona o tratamento de mudança do dia. | Retorna `editada`, `igual`, `barrada` ou `sem_vinculo`; exige autor e motivo e tem guarda de competência lavrada. O resultado posterior específico da porta DST é **[não verificado]**. | [celula.py:572](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:572), [celula.py:622](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:622), [celula.py:632](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:632) |
| **Sem decisão → batida invalidada** | O admin teria de decidir “não vale” segundo o brief; autoridade existente para registrar esse significado integral na porta é **[não verificado]**. | `retratar_batida` é uma operação existente sobre uma batida; não foi demonstrado que seja a opção integral pedida para a porta. | Marca `aprovada=False` e `retratada_em`, sem apagar a batida, e aciona recálculo por evento. | Devolve a batida; se já retratada, retorna sem nova ação. Ela sai da apuração. A preservação e leitura da decisão DST após isso são **[não verificado]**. | brief:L29–L30; [registro_batida.py:287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/registro_batida.py:287), [calendario.py:652](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:652) |
| **Troca entre decisões; revogação** | Para os quatro estados DST, regra e operação de troca ou revogação **[não verificado]**. Para HE, `vigentes()` julga se a linha vale; a porta do admin pode trocar `autorizado` e `nao`. | `decidir_he` troca a decisão HE; `revogar_recusa_do_sistema` revoga somente a recusa do sistema quando sua premissa cai. | A troca atualiza a mesma linha HE e tenta trilha antes/depois; revogação marca a linha, sem apagá-la. | A revogação automática recusa tocar decisão do admin; linha revogada sai das vigentes. Isso não demonstra revogação de uma decisão DST. | [models.py:1265](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/models.py:1265), [he.py:44](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:44), [he.py:375](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:375), [he.py:473](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:473) |
| **Segunda execução igual** | Cada operação existente aplica sua própria regra; idempotência da decisão DST completa **[não verificado]**. | `decidir_he`, `editar_dia`, `retratar_batida` e `lavrar_veredito` têm retornos de repetição; o despachante `veredito_celula` também registra sua própria trilha. | HE igual e vigente não cria trilha; edição igual e retratação repetida não escrevem. A lavra igual retorna sem save, mas o despachante pode registrar outra linha para o ato: não se pode concluir “uma única linha” para a porta inteira. | HE retorna `no_op=True`; edição retorna `igual`; lavra retorna `[]`; retratação retorna a batida. | [he.py:195](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/he.py:195), [celula.py:632](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:632), [celula.py:1075](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:1075), [veredito_celula.py:351](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:351), [registro_batida.py:289](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/registro_batida.py:289) |
| **HE decidida e vigente + órfã na ata em dia sem turno** | `vigentes()` responde pela validade da decisão HE; `dia_sem_trabalho` responde à supressão de cobrança, não à validade da órfã. Nenhuma equivalência entre HE decidida e decisão DST foi demonstrada **[não verificado]**. | O mapa de gestão lê a HE vigente; cartório lavra órfãs; supra-juiz as inclui no fato; calendário as marca; PDF as separa. `_celula_furada` verifica dia sem trabalho antes do ramo que lê órfãs. | `DecisaoHE` conserva estado e foto de minutos; `ata` conserva `orfas` e `n_orfas`. | Gestão devolve estado HE por pessoa/dia; o PDF imprime órfãs em cinza com `*`. A cobrança por `_celula_furada` pode retornar pela véspera antes de chegar às órfãs; Resolver dia recusa `trabalhou` se a grade não for trabalho. A indicação final específica DST na tela é **[não verificado]**. | [gestao_he.py:63](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/gestao_he.py:63), [cartorio.py:730](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:730), [supra_juiz.py:392](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:392), [calendario.py:655](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:655), [pdf_espelho.py:873](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/pdf_espelho.py:873), [juizes.py:497](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:497), [veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243) |

A leitura visual atual também não resolve a decisão: `veredito_do_dia` traduz insumos já decididos e dá precedência à cobertura, rejeição, folga e feriado; `do_dia` pode apresentar uma pendência de ausência. Isso não demonstra a indicação específica de batida sem decisão. ([app/ponto/services/dia_decidido.py:337](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:337), [app/ponto/services/dia_decidido.py:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:376), [app/ponto/services/dia_decidido.py:434](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:434))

O brief formula L1 como escolha entre porta do admin, pergunta ao colaborador ou lista. Hoje, `perguntar_entradas_pendentes` percorre entradas de uma disputa e só pergunta se `classificar_falta` produz motivo; esta retorna sem motivo quando a célula tem `trabalha=False`. Portanto, o código aberto não sustenta que essa função pergunte **genericamente** toda entrada órfã em folga. (brief:L33–L35; [disputa_emissao.py:257](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:257), [servico_jornada.py:308](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/servico_jornada.py:308))

### Juiz e lei faltantes

**Nome descritivo para a pergunta faltante:** `decisao_admin_dia_sem_trabalho`. **Predicado:** “qual decisão vigente o admin tomou sobre a batida apurável: nenhuma, trabalho reconhecido, escala a corrigir ou batida invalidada?” **Universo:** pessoa × dia do turno cuja célula diz sem trabalho e que tem batida apurável. Esse nome é uma identificação da pergunta, não um juiz existente demonstrado na cópia; a unidade e os resultados vêm do brief. (brief:L3, brief:L29–L30)

O juiz existente mais próximo para a **decisão humana do dia** é `decisao_do_dia`: lê os vereditos humanos da célula e autoria da trilha, mas seu universo é o Resolver dia e seu vocabulário não contém os quatro resultados; além disso, a operação que o alimenta recusa `trabalhou` no caso central. `dia_sem_trabalho`, por sua vez, julga se a cobrança cala, com exceção para ausência parcial; não responde qual decisão o admin tomou sobre a batida. Falta cortar a lei que relaciona esses estados à porta e às transições, inclusive L1 e L2 indicadas no brief. ([app/ponto/services/veredito_celula.py:85](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:85), [app/ponto/services/veredito_celula.py:18](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:18), [app/ponto/services/veredito_celula.py:243](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:243), [app/chamados/juizes.py:445](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:445), brief:L33–L35)

PERGUNTA DE DESENHO DST-L1: juiz decisao_admin_dia_sem_trabalho com predicado qual decisão vigente do admin sobre a batida apurável: nenhuma, trabalho reconhecido, escala a corrigir ou batida invalidada; universo pessoa × dia do turno cuja célula diz sem trabalho e há batida apurável; alternativa existente decisao_do_dia nao responde porque lê o vocabulário de Resolver dia, cuja operação recusa trabalhou no caso central, e não os quatro resultados desta decisão.

## 18/11 09:01 · sessao d526f93f08a1 · ORDEM-1811-ITEM8 · sonda · origem=Code

Os dados das tabelas vêm da sonda fornecida no enunciado; sua leitura de produção não foi verificada independentemente. **[não verificado]** Os marcos faltantes são inferências pela ordem e pelo tipo das batidas, não a lista de `marcos_faltantes` gravada em cada chamado. A convenção é `hi` = entrada, `hii` = saída para intervalo, `hfi` = volta e `hf` = saída final. [app/ponto/tests/test_intervalo_ausente.py:17](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_intervalo_ausente.py:17)

Nas tabelas, `E` = entrada e `S` = saída; todas as batidas listadas são de app e os horários são locais. Datas são de 2026. Os donos são os retornos informados pela sonda. **[não verificado]**

### col868

Marcos esperados: `hi` 19:00, `hii` 01:00, `hfi` 02:00, `hf` 07:00. **[não verificado]**

| Dia | Marco faltante inferido | Batidas | Dono |
|---|---|---|---|
| 23/10 | hii | 23/10 18:58 E; 24/10 01:46 E, 02:45 E, 06:53 S | CADASTRO |
| 25/10 | hii | 25/10 18:58 E; 26/10 01:45 E, 02:37 E, 06:53 S | CADASTRO |
| 27/10 | hii | 27/10 18:59 E; 28/10 01:47 E, 02:49 E, 06:51 S | CADASTRO |
| 29/10 | hii | 29/10 18:45 E; 30/10 01:50 E, 03:25 E, 07:00 S | CADASTRO |
| 31/10 | hii | 31/10 18:54 E; 01/11 02:11 E, 03:25 E, 06:55 S | CADASTRO |
| 02/11 | hii | 02/11 18:58 E; 03/11 01:46 E, 02:49 E, 06:51 S | CADASTRO |
| 04/11 | hii | 04/11 19:14 E; 05/11 01:55 E, 02:53 E, 06:59 S | CADASTRO |
| 06/11 | hii | 06/11 18:45 E; 07/11 02:00 E, 03:05 E, 06:53 S | CADASTRO |
| 08/11 | hii, hfi, hf | 08/11 18:57 E | BATIDA |
| 14/11 | hii, hfi | 14/11 18:42 E; 15/11 07:03 S | CADASTRO |
| 16/11 | Indeterminado: há quatro batidas e o agregado informa um furo e uma órfã; a sonda não mostra o casamento por marco. **[não verificado]** | 16/11 19:16 E; 17/11 03:39 S, 04:35 E, 06:49 S | CADASTRO |

Nos oito primeiros dias, não há `S` na sequência do intervalo; há duas entradas intermediárias. Qual delas foi associada a `hfi` e qual ficou órfã não é mostrado. **[não verificado]**

### col934

Marcos esperados: `hi` 23:00, `hii` 05:00, `hfi` 06:00, `hf` 11:00. **[não verificado]**

| Dia | Marcos faltantes inferidos | Batidas | Dono |
|---|---|---|---|
| 06/11 | hi, hii, hfi, hf | Nenhuma | CADASTRO |
| 07/11 | hii, hfi | 07/11 23:00 E; 08/11 10:45 S | CADASTRO |
| 09/11 | hii, hfi | 09/11 22:57 E; 10/11 11:09 S | CADASTRO |
| 11/11 | hii, hfi | 11/11 23:01 E; 12/11 11:00 S | CADASTRO |
| 13/11 | hii, hfi | 13/11 22:57 E; 14/11 11:00 S | CADASTRO |
| 15/11 | hii, hfi | 15/11 22:58 E; 16/11 11:04 S | CADASTRO |
| 17/11 | hii, hfi, hf | 17/11 23:27 E | BATIDA |

### Chamados

Todos têm tipo **Ausência / Batida não realizada** e campo `motivo = None`; a sonda mostra que existe a chave `marcos_faltantes` no contexto, mas não seu conteúdo. Os dias e donos abaixo são os informados pela sonda. **[não verificado]**

| Colab | ID | Dia | Motivo | Dono |
|---|---:|---|---|---|
| col868 | 39641 | 23/10 | None | CADASTRO |
| col868 | 39642 | 25/10 | None | CADASTRO |
| col868 | 39643 | 27/10 | None | CADASTRO |
| col868 | 39644 | 29/10 | None | CADASTRO |
| col868 | 39645 | 31/10 | None | CADASTRO |
| col868 | 39646 | 02/11 | None | CADASTRO |
| col868 | 39647 | 04/11 | None | CADASTRO |
| col868 | 39648 | 06/11 | None | CADASTRO |
| col868 | 39649 | 08/11 | None | BATIDA |
| col868 | 39650 | 14/11 | None | CADASTRO |
| col868 | 39651 | 16/11 | None | CADASTRO |
| col934 | 39652 | 07/11 | None | CADASTRO |
| col934 | 39653 | 09/11 | None | CADASTRO |
| col934 | 39654 | 11/11 | None | CADASTRO |
| col934 | 39655 | 13/11 | None | CADASTRO |
| col934 | 39656 | 15/11 | None | CADASTRO |
| col934 | 39664 | 17/11 | None | BATIDA |

Não há chamado listado para o furo de col934 em 06/11; a sonda não informa a razão. **[não verificado]** Os chamados 39641–39650 aparecem criados às 08:19, com precisão de minuto, portanto não se pode estabelecer se foram criados estritamente depois de 08:19:00. **[não verificado]**

A sonda não mostra o casamento efetivo das batidas nem o conteúdo dos marcos cobrados. Também não resolve um dono que dependa da lista `cxr` quando ela não está disponível com `sem_motor=True`: nesse ramo, o código retorna `None`; nenhum dos donos exibidos nas tabelas veio como `None` ou `ESTRUTURA`. [app/ponto/management/commands/e6_oraculo.py:189](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/e6_oraculo.py:189) **[não verificado]**

## 18/11 09:01 · sessao 04f507b131aa · ESCALA-NAO-DESCREVE · censo · origem=Code

**O censo não está fechado.** A inspeção encontrou consumidores relevantes e uma segunda escrita da chave na sombra, mas não permite afirmar uma lista exaustiva de acessos dinâmicos ou de todos os escritores dos campos de `EscalaColaborador`. O diretório fornecido também não é um repositório Git identificável por `git rev-parse`; a identidade do HEAD e do hash informado fica **[não verificado]**.

Os caminhos abaixo são relativos a `app/`, salvo `bin/`. Os dados de produção atuais, inclusive o conteúdo efetivo da lavra e a contagem de órfãs, permanecem **[não verificado]**.

### 1. Escritores e leitores encontrados, por objeto

**(a) Snapshot e arquivo da lavra do esmeril.**

- `anomalias_da_frota` monta `lavrado_em`, `universo`, `colabs_com_anomalia_recorrente`, `por_assinatura`, `por_colab` e `destino`; só coloca uma pessoa em `por_colab` e chama o proponente quando há `sig`. [ponto/services/esmeril_espelho.py:206](app/ponto/services/esmeril_espelho.py#L206)
- `lavrar` chama o montador e `gravar_lavra`; depois escreve o mesmo objeto em arquivo temporário e o substitui no caminho configurado. `gravar_lavra` faz `update_or_create` do snapshot e grava `detalhe=d`. [ponto/services/esmeril_espelho.py:242](app/ponto/services/esmeril_espelho.py#L242), [ponto/services/esmeril_espelho.py:296](app/ponto/services/esmeril_espelho.py#L296), [ponto/services/esmeril_espelho.py:317](app/ponto/services/esmeril_espelho.py#L317)
- **Outra escrita da mesma chave existe na sombra:** `bin/sombra.sh` copia o `detalhe` de produção e faz `update_or_create` diretamente na chave, além de gravar o marcador de MD5. Isso é um caminho de reposição da sombra, não uma chamada a `gravar_lavra`. [bin/sombra.sh:138](bin/sombra.sh#L138), [bin/sombra.sh:151](bin/sombra.sh#L151)
- `ler_lavra` busca o snapshot mais recente, verifica o MD5 quando em sombra e devolve `detalhe`; o código informa que deixou de usar o JSON como fonte. A busca pelo caminho literal e pela configuração identificou o escritor do arquivo, mas nenhum leitor atual do JSON com esse nome; leitores por caminho construído dinamicamente ficam **[não verificado]**. [ponto/services/esmeril_espelho.py:334](app/ponto/services/esmeril_espelho.py#L334)
- Leitores identificados do snapshot por `ler_lavra`: lista Cadastro x Realidade; Pauta do esmeril; lista resumida para a revisão semanal; bloco “sem marco”; propostas automáticas; API de anomalias; comando do esmeril. [escala/services/cadastro_realidade.py:872](app/escala/services/cadastro_realidade.py#L872), [ponto/services/pautas_do_esmeril.py:52](app/ponto/services/pautas_do_esmeril.py#L52), [ponto/services/esmeril_espelho.py:356](app/ponto/services/esmeril_espelho.py#L356), [chamados/services/validacao.py:1218](app/chamados/services/validacao.py#L1218), [escala/services/escala_auto.py:163](app/escala/services/escala_auto.py#L163), [api/views_mensageria.py:1786](app/api/views_mensageria.py#L1786), [ponto/management/commands/esmeril_espelho.py:15](app/ponto/management/commands/esmeril_espelho.py#L15)
- `bin/sombra.sh` também lê diretamente o snapshot e serializa o detalhe inteiro; o placar encontrado chama o comando e extrai seu contador textual. A função genérica `inteligencia.metricas.serie(chave, ...)` pode buscar snapshots pela chave recebida; uso dela com esta chave por chamador dinâmico fica **[não verificado]**. [bin/sombra.sh:141](bin/sombra.sh#L141), [bin/placar_code.sh:389](bin/placar_code.sh#L389), [inteligencia/metricas.py:62](app/inteligencia/metricas.py#L62)

**Colisões de nome:** `ata.orfas` é a lista da célula usada no cálculo de A5; não é o novo mapa superior proposto. O `destino` da C7 vem da lavra do termômetro, não do mapa `destino` do esmeril. A `proposta` lida por `_destino` é o resultado do proponente, antes de virar frase. [ponto/services/esmeril_espelho.py:115](app/ponto/services/esmeril_espelho.py#L115), [escala/services/cadastro_realidade.py:314](app/escala/services/cadastro_realidade.py#L314), [ponto/services/esmeril_espelho.py:234](app/ponto/services/esmeril_espelho.py#L234)

**(b) `assinaturas_do_colab` e `janela`.** A função retorna um dicionário de assinaturas; há retorno antecipado sem células e outro para `so_escala=True`, antes do laço que percorre órfãs. `janela` retorna início da competência aberta mais antiga alcançável e ontem. Chamadores identificados: `anomalias_do_colab` e `anomalias_da_frota` chamam ambas; `escala_certa` chama assinaturas com `so_escala=True`. [ponto/services/esmeril_espelho.py:38](app/ponto/services/esmeril_espelho.py#L38), [ponto/services/esmeril_espelho.py:79](app/ponto/services/esmeril_espelho.py#L79), [ponto/services/esmeril_espelho.py:192](app/ponto/services/esmeril_espelho.py#L192), [ponto/services/esmeril_espelho.py:206](app/ponto/services/esmeril_espelho.py#L206), [escala/services/escala_certa.py:27](app/escala/services/escala_certa.py#L27)

**(c) `lista`, `pdf_bytes`, `dias_na_lista`, `_vinculos`, `acao`.** Os consumidores encontrados e o contrato de `postos` estão no item 4. `_vinculos` consulta o vínculo vigente e fornece os dados usados por `lista`; `acao` transforma código e vínculo em rótulo e URL. A2/A8 hoje apontam ao vínculo; A11 e C11 apontam ao Plano de Folgas. [escala/services/cadastro_realidade.py:748](app/escala/services/cadastro_realidade.py#L748), [escala/services/cadastro_realidade.py:656](app/escala/services/cadastro_realidade.py#L656), [escala/services/cadastro_realidade.py:708](app/escala/services/cadastro_realidade.py#L708)

**(d) Porta do vínculo.** Os chamadores e escritas identificados estão no item 5. A relação global de todos os escritores desses mesmos campos no projeto fica **[não verificado]**; o código da própria porta chama `fechar_vigencia` e `absorver_vigencias_posteriores`, além de criar ou atualizar o vínculo. [colaboradores/services/vinculo.py:211](app/colaboradores/services/vinculo.py#L211), [colaboradores/services/vinculo.py:253](app/colaboradores/services/vinculo.py#L253), [colaboradores/services/vinculo.py:259](app/colaboradores/services/vinculo.py#L259)

**(e) View e template.** A tela exige `ver_escala`, chama `lista` com empresas visíveis e passa o resultado ao template ou PDF. As rotas C7 usam POST e `_is_gestor`; essa função aceita `is_staff` ou grupo `dp_rh`, `gestor` ou `admin_ti`. A realocação C7 lê destino da lavra do termômetro pelo vínculo, não do corpo do POST. [escala/views.py:440](app/escala/views.py#L440), [escala/views.py:462](app/escala/views.py#L462), [escala/views.py:475](app/escala/views.py#L475), [chamados/views.py:24](app/chamados/views.py#L24), [chamados/views.py:35](app/chamados/views.py#L35), [escala/urls.py:14](app/escala/urls.py#L14)

O template estende `base.html` e inclui `core/_btn_pdf.html`; a referência de produção encontrada que o renderiza é a view citada. Inclusões ou extensões por nome montado dinamicamente ficam **[não verificado]**. [templates/escala/cadastro_x_realidade.html:1](app/templates/escala/cadastro_x_realidade.html#L1), [templates/escala/cadastro_x_realidade.html:11](app/templates/escala/cadastro_x_realidade.html#L11), [escala/views.py:492](app/escala/views.py#L492)

### 2. Efeito das duas chaves novas no topo

**Sim, há mudança observável na sombra.** `_md5_da_lavra` serializa o detalhe inteiro com chaves ordenadas: acrescentar `orfas` e `proposta` altera o hash. Se detalhe e marcador forem copiados juntos, ambos usam o mesmo cálculo; se apenas um mudar, a guarda levanta `SemLavra`. Além disso, a reposição em `bin/sombra.sh` grava `valor=len(detalhe)`, que cresce em dois para um detalhe antes sem essas chaves. [ponto/services/esmeril_espelho.py:260](app/ponto/services/esmeril_espelho.py#L260), [ponto/services/esmeril_espelho.py:266](app/ponto/services/esmeril_espelho.py#L266), [bin/sombra.sh:155](bin/sombra.sh#L155)

A API de anomalias seleciona explicitamente quatro chaves e não inclui os novos mapas; o comando do placar também lê campos existentes, e o shell extrai somente a linha de contagem. Esses leitores identificados não passam a expor as chaves novas por adicioná-las ao detalhe. [api/views_mensageria.py:1794](app/api/views_mensageria.py#L1794), [ponto/management/commands/esmeril_espelho.py:21](app/ponto/management/commands/esmeril_espelho.py#L21), [bin/placar_code.sh:389](bin/placar_code.sh#L389)

Que **nenhum outro** leitor genérico do snapshot itere ou serialize o detalhe inteiro fica **[não verificado]**. [inteligencia/metricas.py:62](app/inteligencia/metricas.py#L62)

### 3. Pressuposto sobre `por_colab` e `destino`

**Sim.** O montador só insere em `por_colab` quando `sig` é verdadeiro e usa `len(por_colab)` como número de pessoas com anomalia recorrente; `gravar_lavra` também usa esse comprimento como numerador. A Pauta percorre as assinaturas de cada entrada, e o bloco “sem marco” trata a presença da pessoa como indicação de assinaturas. [ponto/services/esmeril_espelho.py:220](app/ponto/services/esmeril_espelho.py#L220), [ponto/services/esmeril_espelho.py:323](app/ponto/services/esmeril_espelho.py#L323), [ponto/services/pautas_do_esmeril.py:27](app/ponto/services/pautas_do_esmeril.py#L27), [chamados/services/validacao.py:1229](app/chamados/services/validacao.py#L1229)

Manter `por_colab` e `destino` byte a byte equivalentes enquanto os mapas novos ficam em chaves separadas preserva **essas leituras identificadas**; isso não preserva o MD5 do detalhe inteiro, como acima. Preservação de todos os consumidores possíveis fica **[não verificado]**. [escala/services/cadastro_realidade.py:872](app/escala/services/cadastro_realidade.py#L872), [ponto/services/esmeril_espelho.py:260](app/ponto/services/esmeril_espelho.py#L260)

### 4. Retorno de `cadastro_realidade.lista`

O retorno atual é `None` se não houver `por_colab` nem cadastro; caso contrário, contém `lavrado_em`, `universo`, `colabs_com_anomalia_recorrente`, `colabs_do_cadastro`, `colabs_listados`, `por_assinatura`, `previsto_cego` e `postos`. `postos` é lista de pares `(nome_do_posto, lista_de_linhas)`, com linhas ordenadas por nome dentro de cada posto. [escala/services/cadastro_realidade.py:872](app/escala/services/cadastro_realidade.py#L872), [escala/services/cadastro_realidade.py:930](app/escala/services/cadastro_realidade.py#L930), [escala/services/cadastro_realidade.py:956](app/escala/services/cadastro_realidade.py#L956)

Cada linha começa com `colab_id`, `nome`, `matricula`, `empresa_id`, `busca`, `posto`, `posto_id`, `template`, `template_id`; recebe `destino` e `assinaturas`. Assinaturas carregam campos comuns como código, descrição, dono, ocorrências, ação e URL, podendo incluir campos específicos da classe. [escala/services/cadastro_realidade.py:754](app/escala/services/cadastro_realidade.py#L754), [escala/services/cadastro_realidade.py:887](app/escala/services/cadastro_realidade.py#L887), [escala/services/cadastro_realidade.py:912](app/escala/services/cadastro_realidade.py#L912)

Leitores identificados:

- Template: contadores, `previsto_cego`, `por_assinatura`, pares de `postos`, nome, matrícula, template, destino e campos das assinaturas, inclusive os dados C7 para formulários. [templates/escala/cadastro_x_realidade.html:8](app/templates/escala/cadastro_x_realidade.html#L8), [templates/escala/cadastro_x_realidade.html:17](app/templates/escala/cadastro_x_realidade.html#L17), [templates/escala/cadastro_x_realidade.html:21](app/templates/escala/cadastro_x_realidade.html#L21)
- PDF: contadores, `lavrado_em`, `postos`, nome, matrícula, template, assinaturas, ação e destino; percorre a ordem que recebe. [escala/services/cadastro_realidade.py:1073](app/escala/services/cadastro_realidade.py#L1073), [escala/services/cadastro_realidade.py:1083](app/escala/services/cadastro_realidade.py#L1083)
- `dias_na_lista`: pares de `postos`, `colab_id`, assinaturas com `data`, `codigo` e `dono_do_dia`. O vigia chama `lista` e `dias_na_lista` e também extrai diretamente os IDs das linhas. [escala/services/cadastro_realidade.py:1030](app/escala/services/cadastro_realidade.py#L1030), [chamados/services/vigia_fio.py:190](app/chamados/services/vigia_fio.py#L190)
- Comandos de fechamento de avisos C7 e chamados C8: pares de `postos`, `colab_id`, `assinaturas`, `codigo` e `detalhe`. [escala/management/commands/fechar_avisos_regua_na_lista.py:27](app/escala/management/commands/fechar_avisos_regua_na_lista.py#L27), [ponto/management/commands/fechar_chamados_vinculo_na_lista.py:31](app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py#L31)

Assim, remover ou mudar a forma de `postos` sem adaptar esses leitores altera ou quebra seus percursos; acrescentar uma lista separada e conservar a forma antiga evita essa incompatibilidade específica. [escala/services/cadastro_realidade.py:1044](app/escala/services/cadastro_realidade.py#L1044), [chamados/services/vigia_fio.py:196](app/chamados/services/vigia_fio.py#L196)

### 5. Chamadas, datas, autorização e trilha do vínculo

- **C7 da lista:** `realocar_regua_da_lista` obtém o destino do termômetro e chama `realocar_vinculo_para_destino`; o núcleo usa `timezone.localdate()` como início. A borda exige login, POST e `_is_gestor`. [escala/views.py:440](app/escala/views.py#L440), [escala/views.py:462](app/escala/views.py#L462), [escala/regua_defesa.py:310](app/escala/regua_defesa.py#L310)
- **Chamado antigo:** `realocar_para_destino` usa `contexto_json['destino']`, chama o mesmo núcleo com início hoje e, no sucesso, anexa ocorrência com origem, destino, data e usuário. A view exige gestor e aplica a checagem contextual indicada no código. [chamados/views.py:418](app/chamados/views.py#L418), [chamados/views.py:449](app/chamados/views.py#L449), [escala/regua_defesa.py:326](app/escala/regua_defesa.py#L326)
- **Tela de vínculo:** passa `request.POST` ao juiz, portanto a data é a fornecida no formulário; exige POST e `editar_escalas`. [colaboradores/views.py:391](app/colaboradores/views.py#L391), [colaboradores/views.py:428](app/colaboradores/views.py#L428)
- **Chat:** o preview chama o juiz; na confirmação, a data vem da proposta de sessão e a borda exige `editar_escalas`. O executor registra ainda um log específico de troca pelo chat. [colaboradores/services/troca_pelo_chat.py:50](app/colaboradores/services/troca_pelo_chat.py#L50), [core/views_ia.py:945](app/core/views_ia.py#L945), [core/views_ia.py:997](app/core/views_ia.py#L997)
- **Comando de cura de fase:** usa `x['virada']` como início e `x['proposta_ancora']` como âncora; seleciona o primeiro superusuário ativo para assinar a trilha. [ponto/management/commands/censo_fase_12x36.py:363](app/ponto/management/commands/censo_fase_12x36.py#L363), [ponto/management/commands/censo_fase_12x36.py:423](app/ponto/management/commands/censo_fase_12x36.py#L423)

A porta não faz, por si, uma checagem geral de staff ou de `editar_escalas`: o juiz lê data e template e aplica seus vetos de domínio; as permissões citadas estão nas bordas. O núcleo de realocação recusa destino cíclico e passa o perfil e o request do chamador. [colaboradores/services/vinculo.py:52](app/colaboradores/services/vinculo.py#L52), [escala/regua_defesa.py:303](app/escala/regua_defesa.py#L303)

Na execução, vínculos anteriores são fechados em D−1 e posteriores podem ser absorvidos; o existente da mesma data recebe tipo, posto, `ativa=True` e `data_fim=None`, ou um novo é criado com posto, tipo, início e ativo. A trilha usa o usuário do request: fechamento registra antes/depois de fim e ativo com `quem` e motivo; absorção registra antes/depois e histórico; o vínculo criado ou editado recebe log por PK e foto dos campos, além de `registrar_mudanca` com texto anterior/posterior e motivo `correcao` ou `mudanca`. [colaboradores/services/vinculo.py:211](app/colaboradores/services/vinculo.py#L211), [colaboradores/services/vinculo.py:259](app/colaboradores/services/vinculo.py#L259), [colaboradores/services/vinculo.py:638](app/colaboradores/services/vinculo.py#L638), [colaboradores/services/vinculo.py:798](app/colaboradores/services/vinculo.py#L798), [colaboradores/services/vinculo.py:349](app/colaboradores/services/vinculo.py#L349)

### 6. Identificação de 6x1 no Plano de Folgas

O campo é `ec.tipo_escala.tipo_ciclo`, comparado com `TipoEscala.CICLO_6x1`, cujo valor é `'6x1'`. O grid do Plano chama `ec.defeito_folga_nao_definida(...)`; esse método faz a comparação e verifica folga semanal ou `FolgaDia` no mês. A view não faz ela mesma uma comparação direta com `CICLO_6x1` no trecho do grid. [escala/models.py:24](app/escala/models.py#L24), [escala/models.py:117](app/escala/models.py#L117), [escala/models.py:1241](app/escala/models.py#L1241), [escala/views_plano_folgas.py:83](app/escala/views_plano_folgas.py#L83)

### 7. Furos encontrados além do anexo AST

- O script exclui chamadas simples pelo nome quando não há import reconhecido. Por isso, as chamadas locais de `janela`, `assinaturas_do_colab`, `anomalias_da_frota`, `gravar_lavra` e `ler_lavra` dentro do próprio módulo precisam ser seguidas manualmente. [ponto/services/esmeril_espelho.py:192](app/ponto/services/esmeril_espelho.py#L192), [ponto/services/esmeril_espelho.py:303](app/ponto/services/esmeril_espelho.py#L303), [ponto/services/esmeril_espelho.py:356](app/ponto/services/esmeril_espelho.py#L356)
- O alias local `_lp` para `linhas_para_a_pauta` não estava no conjunto de nomes-alvo; ele leva à leitura da lavra na revisão semanal. [escala/services/fase_12x36.py:240](app/escala/services/fase_12x36.py#L240), [ponto/services/esmeril_espelho.py:356](app/ponto/services/esmeril_espelho.py#L356)
- Template Django, shell, serialização integral e `json.load` da cópia da sombra estão fora da varredura Python do anexo. [templates/escala/cadastro_x_realidade.html:21](app/templates/escala/cadastro_x_realidade.html#L21), [bin/sombra.sh:141](bin/sombra.sh#L141), [bin/sombra.sh:155](bin/sombra.sh#L155)
- A API usa import de módulo com alias `_es` e constrói resposta com compreensão e `**`; ela seleciona explicitamente as quatro chaves indicadas acima. [api/views_mensageria.py:1791](app/api/views_mensageria.py#L1791)
- Um reexport por `__init__`, import relativo ou outro alias que alcance estes objetos além dos exemplos acima permanece **[não verificado]**. O mesmo vale para leitores genéricos por chave recebida em tempo de execução. [inteligencia/metricas.py:62](app/inteligencia/metricas.py#L62)

A descrição em `config/crons.py` sobre `horas_folga_trabalhada` é texto de diagnóstico; o código atual de `ler_lavra` registra que o consumidor de dinheiro deixou de ler essa lavra, e `escala_certa` mede assinaturas diretamente. A execução efetiva em produção permanece **[não verificado]**. [config/crons.py:1064](app/config/crons.py#L1064), [ponto/services/esmeril_espelho.py:348](app/ponto/services/esmeril_espelho.py#L348), [escala/services/escala_certa.py:27](app/escala/services/escala_certa.py#L27)

censo fechado: nao -- faltam exaustividade de acessos dinamicos/reexports e de escritores globais dos campos do vinculo; identidade do HEAD e estado de producao nao verificados.

## 18/11 09:01 · sessao d57fe0d8679b · DIA-SEM-TRABALHO-PORTA · censo · origem=Code

HASH LIDO = dd489c627bb0a8d5f8782945c7e240e6621c98ed

O hash acima é o informado no pedido; a cópia não contém metadados Git para confirmá-lo com `git rev-parse`. [não verificado]

O censo encontrou a diferença central: `_protesta` considera `cel.trabalha` e os marcos acesos, mas não consulta `tipo_dia` da ata; o juiz `dia_sem_trabalho` também considera o tipo da ata e a exceção de ausência parcial. [cartorio.py:1458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1458), [juizes.py:445](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:445)

### 1. Script e resultado da leitura

O script abaixo faz uma varredura sintática de todos os `app/**/*.py`, inclusive testes, sem regex. Resolve imports com `as`, inclusive imports locais e relativos, e imprime também acessos às chaves em objetos que **podem não ser atas**; essa classificação exige leitura do contexto. Ele registra literais de dicionário como escritas. Chamadas indiretas, chaves construídas dinamicamente e alterações por métodos como `update` com dicionário dinâmico não são provadas por esta análise. **Esta versão consolidada do script não foi executada**; executei uma versão preliminar, cujo resultado parcial está resumido abaixo. [não verificado] [nao verificado]

```python
import ast
import importlib.util
from pathlib import Path

ROOT = Path("app")
KEYS = {"tipo_dia", "cobertura_parcial"}
TARGETS = {
    "chamados.juizes.dia_sem_trabalho",
    "chamados.juizes.ausencia_parcial_cobra_o_resto",
    "ponto.supra_juiz._dia_sem_trabalho_pelo_juiz",
}


def module_name(path):
    parts = path.relative_to(ROOT).with_suffix("").parts
    if parts[-1] == "__init__":
        parts = parts[:-1]
    return ".".join(parts)


def absolute_import(module, level, current, is_package):
    if not level:
        return module or ""
    package = current if is_package else current.rpartition(".")[0]
    try:
        return importlib.util.resolve_name(
            "." * level + (module or ""), package
        )
    except (ImportError, ValueError):
        return "." * level + (module or "")


def bindings(body, current, is_package):
    """Imports e definições diretamente no corpo de um escopo."""
    found = {}
    for node in body:
        if isinstance(node, ast.Import):
            for alias in node.names:
                local = alias.asname or alias.name.split(".")[0]
                found[local] = alias.name if alias.asname else local
        elif isinstance(node, ast.ImportFrom):
            base = absolute_import(
                node.module, node.level, current, is_package
            )
            for alias in node.names:
                if alias.name != "*":
                    found[alias.asname or alias.name] = (
                        f"{base}.{alias.name}" if base else alias.name
                    )
        elif isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef,
                               ast.ClassDef)):
            found[node.name] = f"{current}.{node.name}"
    return found


class Census(ast.NodeVisitor):
    def __init__(self, path, tree):
        self.path = path
        self.current = module_name(path)
        self.is_package = path.name == "__init__.py"
        self.scopes = [
            bindings(tree.body, self.current, self.is_package)
        ]
        self.names = []
        self.rows = []

    def resolve(self, node):
        if isinstance(node, ast.Name):
            for scope in reversed(self.scopes):
                if node.id in scope:
                    return scope[node.id]
            return node.id
        if isinstance(node, ast.Attribute):
            base = self.resolve(node.value)
            return f"{base}.{node.attr}" if base else node.attr
        return ""

    def add(self, node, what, action):
        self.rows.append((
            str(self.path), node.lineno,
            ".".join(self.names) or "<módulo>", what, action
        ))

    def scoped(self, node):
        self.names.append(node.name)
        self.scopes.append(
            bindings(node.body, self.current, self.is_package)
        )
        # Visita também decoradores e valores padrão.
        self.generic_visit(node)
        self.scopes.pop()
        self.names.pop()

    visit_FunctionDef = scoped
    visit_AsyncFunctionDef = scoped
    visit_ClassDef = scoped

    def visit_Call(self, node):
        target = self.resolve(node.func)
        if target in TARGETS:
            self.add(node, target, "chamada")
        if (
            isinstance(node.func, ast.Attribute)
            and node.func.attr == "get"
            and node.args
            and isinstance(node.args[0], ast.Constant)
            and node.args[0].value in KEYS
        ):
            self.add(node, node.args[0].value, "leitura .get")
        self.generic_visit(node)

    def visit_Subscript(self, node):
        if isinstance(node.slice, ast.Constant) and node.slice.value in KEYS:
            action = (
                "escrita" if isinstance(node.ctx, ast.Store)
                else "remoção" if isinstance(node.ctx, ast.Del)
                else "leitura"
            )
            self.add(node, node.slice.value, action)
        self.generic_visit(node)

    def visit_Dict(self, node):
        for key in node.keys:
            if isinstance(key, ast.Constant) and key.value in KEYS:
                self.add(key, key.value, "escrita em literal")
        self.generic_visit(node)


rows = []
errors = []
for path in sorted(ROOT.rglob("*.py")):
    try:
        tree = ast.parse(path.read_bytes(), filename=str(path))
    except (SyntaxError, UnicodeDecodeError) as exc:
        errors.append((path, exc))
        continue
    visitor = Census(path, tree)
    visitor.visit(tree)
    rows.extend(visitor.rows)

print("arquivo:linha\tfunção/classe\titem\toperação")
for path, line, function, item, action in sorted(rows):
    print(f"{path}:{line}\t{function}\t{item}\t{action}")
for path, error in errors:
    print(f"ERRO DE PARSE: {path}: {error}")
print(f"TOTAL: {len(rows)}; ERROS: {len(errors)}")
```

A versão preliminar executada encontrou 240 ocorrências sintáticas, incluindo testes e acessos a grades, mas não detectou a chamada local de `ausencia_parcial_cobra_o_resto` em [juizes.py:458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:458), nem literais de dicionário. Portanto, **não é a tabela completa pedida**. [não verificado]

Principais ocorrências de produção confirmadas na leitura:

| Item | Sítios e função/operação |
|---|---|
| Juiz `dia_sem_trabalho` | [juizes.py:498](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:498) `_celula_furada`; [models.py:1331](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1331) `premissa_regularizada`; [fabrica_da_celula.py:112](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:112) e [201](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:201); [materializacao.py:431](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:431); [detectar_intervalo_ausente.py:51](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/detectar_intervalo_ausente.py:51); [reconciliar_grade.py:282](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:282); [cluster_espurio.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cluster_espurio.py:25); adaptador [supra_juiz.py:84](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:84). |
| Adaptador `_dia_sem_trabalho_pelo_juiz` | Leituras em [cartorio.py:1255](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1255), [supra_juiz.py:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:509) e [tarefa_do_dia.py:233](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:233), [505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:505), [517](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:517). |
| `tipo_dia` na ata | Escrita por cópia em [cartorio.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:738); leitura pelo juiz em [juizes.py:442](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:442) e [460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:460). Outros acessos da varredura são também a dias da grade e precisam de classificação individual. [não verificado] |
| `cobertura_parcial` | Escrita na grade em [utils.py:1230](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1230), copiada para a ata em [cartorio.py:766](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:766), transportada pelo leitor em [leitor_celula.py:433](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/leitor_celula.py:433); leitura pelo predicado em [juizes.py:442](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:442). Demais acessos: [não verificado]. |
| `ausencia_parcial_cobra_o_resto` | Chamadas em [juizes.py:458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:458), [declaracao_texto.py:120](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/declaracao_texto.py:120), [tarefa_do_dia.py:242](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:242), [furos_diarios.py:49](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/furos_diarios.py:49), [calculadores.py:50](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/calculadores.py:50) e [149](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/calculadores.py:149), [furos.py:199](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/furos.py:199). |

### 2. Escrita do `tipo_dia` da ata deste julgamento

`_ata_agora` é composta por `ata_do_dia`, `dispensar_por_ausencia` e `_ata_ate_o_teto` em [cartorio.py:1436](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1436). A chave é copiada de `dia.get('tipo_dia')` por `ata_do_dia` em [cartorio.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:738); as etapas de dispensa e teto alteram lâmpadas e contagens, preservando essa chave em [cartorio.py:800](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:800) e [cartorio.py:907](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:907).

A origem do valor é a grade montada em [cartorio.py:1175](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1175):

| Valor | Escrita na grade |
|---|---|
| `folga` | Construtores em [utils.py:760](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:760) e [utils.py:1801](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1801); folga lançada válida em [utils.py:1151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1151). |
| `trabalho` | Construtores em [utils.py:764](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:764) e [utils.py:1807](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1807). |
| `falta` | Cobertura classificada como falta, respeitada a condição sobre folga, em [utils.py:1210](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1210). |
| `ausencia` | Cobertura sobreposta pela grade em [utils.py:1220](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1220). |
| Outros valores na ata deste caminho | [não verificado]. `ata_do_dia` aceita `None` se receber um `dia` arbitrário sem a chave, mas isso não demonstra que a grade deste julgamento produza tal entrada: [cartorio.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:738). |

A sobreposição de folga ou cobertura na grade pode produzir tipo diferente de `trabalho` enquanto `cel.trabalha` é `True` ou `None`, pois o cartório passa separadamente o atributo da célula e a ata construída da grade: [utils.py:1147](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1147), [utils.py:1210](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1210), [cartorio.py:1464](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1464). Há teste explícito de `True` com ausência parcial em [test_emis_ausencia_parcial_cobra.py:78](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_emis_ausencia_parcial_cobra.py:78). A ocorrência em produção é [não verificado].

### 3. Consumidores de `_protesta` e efeitos

| Consumidor | Uso |
|---|---|
| `_protestam` | Filtra `vivos` em [cartorio.py:1460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1460); seu tamanho alimenta `chamados_vivos` em [cartorio.py:1464](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1464). |
| Lâmpada `_vd` | Se não há códigos e há vivos, um chamado com fio que protesta produz `cobrado` em [cartorio.py:1479](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1479) e [1502](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1502). |
| `_nao_protestam` / `_adiar` | Separa os que não protestam e pode adiar o lote em [cartorio.py:1533](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1533); processamento posterior em [cartorio.py:999](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:999). |
| `_porm` | Tenta transicionar os não adiados para `resolvido`; os efetivamente transicionados saem de `vivos` e provocam rejulgamento em [cartorio.py:1539](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1539). |

Em `classificar_dia`, `chamados_vivos` participa dos códigos `COBRANCA_SEM_FURO`, `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_EM_DIA_COBERTO`: [supra_juiz.py:480](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:480), [507](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:507), [543](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:543).

`cobrado` integra `VEREDITOS_ACUSAM` em [models.py:1767](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/models.py:1767). Entre os leitores identificados estão folha/export em [export.py:1273](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/export.py:1273); status em [status_dia.py:68](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/status_dia.py:68); relatório/tela em [dia_da_frota.py:51](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/dia_da_frota.py:51); vigias em [vigia_pausa.py:155](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/services/vigia_pausa.py:155), [vigia_ausencia_veredito.py:41](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/vigia_ausencia_veredito.py:41) e [vigia_fio.py:286](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/vigia_fio.py:286). A lista completa de leitores diretos e indiretos é [não verificado].

No fechamento, `transicionar_com_pauta` executa o ato e encaminha recusas à pauta em [fecho_runtime.py:146](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fecho_runtime.py:146). `reconciliar` aplica as guardas e grava o estado quando permitido em [models.py:642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:642); ao resolver, o receiver chama a resolução das perguntas em [signals.py:91](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:91). A relação exaustiva das reações a jusante é [não verificado].

### 4. Predicados próprios

- `_protesta`: `cel.trabalha is not False` **e** `marcos_acesos(x, ata_deste_julgamento) is not True`; não consulta `dia_sem_trabalho`: [cartorio.py:1458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1458).
- No ramo de nunca bateu, `_coberto_inteiro_nb` testa `tipo_dia in ('ausencia', 'falta')` e ausência de `cobertura_parcial`; serve à decisão de `sem_adesao`, não equivale ao juiz geral: [cartorio.py:1237](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1237), [1257](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1257).
- `marcos_acesos` examina o lastro dos marcos cobrados, com resultado indeterminado quando não consegue decidir; é uma decisão por marco: [lastro.py:153](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/lastro.py:153).

A inexistência de outros predicados próprios nos três arquivos é [não verificado].

### 5. Origem da ata entregue ao juiz

O juiz lê `cel.ata or {}` e `cel.trabalha`; para ausência parcial, só `trabalha is False` torna o dia sem trabalho; nos demais casos também basta `tipo_dia` preenchido e diferente de `trabalho`: [juizes.py:454](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:454).

| Caminho | Origem identificada |
|---|---|
| `_celula_furada` e `premissa_regularizada` | Usam `celula_do_dia_agora`; a porta pode projetar a ata do julgamento em curso sobre a célula e, fora dele, conservar a lavrada: [juizes.py:407](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:407), [491](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:491), [models.py:1327](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1327). |
| Materialização, comando de intervalo e reconciliação de grade | Consultam células carregadas do banco em [materializacao.py:421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:421), [detectar_intervalo_ausente.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/detectar_intervalo_ausente.py:46), [reconciliar_grade.py:189](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:189). |
| Fábrica e cluster | Recebem uma célula como argumento em [fabrica_da_celula.py:92](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:92), [178](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:178), [cluster_espurio.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cluster_espurio.py:25); a origem em **cada** chamador desses serviços é [não ve

[truncado em 20000 de 21190 caracteres]

## 18/11 09:00 · sessao fabad78fa82a · ESPURIA-TELA · censo · origem=Code

```python
#!/usr/bin/env python3
# Salve este código como censo.py fora do repositório.
# Na raiz da cópia do repositório, execute: python3 /caminho/para/censo.py
# Para salvar a saída: python3 /caminho/para/censo.py > censo.tsv

import ast
import collections
import pathlib
import re
import sys

ROOT = pathlib.Path.cwd()
APP = ROOT / "app"
TEMPLATES = APP / "templates"
STATIC_JS = APP / "static" / "js"

NAMES = {
    "dia_decidido",
    "veredito_do_dia",
    "do_dia",
    "decididos_do_periodo",
    "palavra_dia",
    "cor_dia",
    "veredito_dia",
    "precedencia",
    "fatos_do_periodo",
}
NAME_RE = re.compile(
    r"(?<![A-Za-z0-9_])(" + "|".join(sorted(map(re.escape, NAMES))) +
    r")(?![A-Za-z0-9_])"
)

if not APP.is_dir():
    sys.exit("Erro: execute na raiz do repositório, com o código em app/.")


def rel(path):
    return path.relative_to(ROOT).as_posix()


def read(path):
    try:
        return path.read_text(encoding="utf-8")
    except (OSError, UnicodeError) as exc:
        print("ERRO_LEITURA\t{}\t{}".format(rel(path), exc), file=sys.stderr)
        return None


def is_test(path):
    relative = path.relative_to(APP)
    return "tests" in relative.parts[:-1] or path.name.startswith("test_")


def dotted(node):
    if isinstance(node, ast.Name):
        return node.id
    if isinstance(node, ast.Attribute):
        prefix = dotted(node.value)
        return prefix + "." + node.attr if prefix else None
    return None


def module_name(path):
    parts = list(path.relative_to(APP).with_suffix("").parts)
    if parts and parts[-1] == "__init__":
        parts.pop()
    return ".".join(parts)


def resolve_import(path, node):
    module = node.module or ""
    if not node.level:
        return module
    package = module_name(path).split(".")
    if path.name != "__init__.py":
        package = package[:-1]
    up = node.level - 1
    if up:
        package = package[:-up] if up <= len(package) else []
    return ".".join(package + ([module] if module else []))


class PythonCensus(ast.NodeVisitor):
    def __init__(self, path):
        self.path = path
        self.rows = set()
        self.parents = []
        self.scopes = [dict()]
        self.functions = []

    def emit(self, node, name, kind):
        if name in NAMES:
            self.rows.add((
                rel(self.path),
                getattr(node, "lineno", 0),
                ".".join(self.functions) or "<modulo>",
                name,
                kind,
            ))

    def lookup(self, name):
        for scope in reversed(self.scopes):
            if name in scope:
                return scope[name]
        return None

    def canonical(self, node):
        if isinstance(node, ast.Name):
            return self.lookup(node.id) or (node.id if node.id in NAMES else None)
        if isinstance(node, ast.Attribute):
            if node.attr in NAMES:
                return node.attr
            return None
        return None

    def visit(self, node):
        if node is None:
            return None
        self.parents.append(node)
        try:
            return super().visit(node)
        finally:
            self.parents.pop()

    def visit_Import(self, node):
        for alias in node.names:
            parts = alias.name.split(".")
            for part in parts:
                self.emit(node, part, "import")
            bound = alias.asname or parts[0]
            target = parts[-1] if alias.asname else parts[0]
            self.scopes[-1][bound] = target if target in NAMES else None

    def visit_ImportFrom(self, node):
        module = resolve_import(self.path, node)
        for part in module.split("."):
            self.emit(node, part, "import")
        for alias in node.names:
            self.emit(node, alias.name, "import")
            bound = alias.asname or alias.name
            self.scopes[-1][bound] = alias.name if alias.name in NAMES else None

    def visit_FunctionDef(self, node):
        self._function(node)

    def visit_AsyncFunctionDef(self, node):
        self._function(node)

    def _function(self, node):
        self.emit(node, node.name, "definicao")
        self.scopes[-1][node.name] = node.name if node.name in NAMES else None
        for decorator in node.decorator_list:
            self.visit(decorator)
        for default in node.args.defaults + node.args.kw_defaults:
            self.visit(default)
        if node.returns:
            self.visit(node.returns)
        self.functions.append(node.name)
        self.scopes.append({})
        for arg in (
            node.args.posonlyargs + node.args.args + node.args.kwonlyargs +
            ([node.args.vararg] if node.args.vararg else []) +
            ([node.args.kwarg] if node.args.kwarg else [])
        ):
            self.scopes[-1][arg.arg] = None
            if arg.annotation:
                self.visit(arg.annotation)
        for statement in node.body:
            self.visit(statement)
        self.scopes.pop()
        self.functions.pop()

    def visit_ClassDef(self, node):
        self.emit(node, node.name, "definicao")
        for decorator in node.decorator_list:
            self.visit(decorator)
        for base in node.bases:
            self.visit(base)
        for keyword in node.keywords:
            self.visit(keyword.value)
        self.scopes.append({})
        for statement in node.body:
            self.visit(statement)
        self.scopes.pop()

    def visit_Name(self, node):
        name = self.canonical(node)
        if name:
            parent = self.parents[-2] if len(self.parents) > 1 else None
            kind = (
                "chamada" if isinstance(parent, ast.Call) and parent.func is node
                else "definicao" if isinstance(node.ctx, (ast.Store, ast.Del))
                else "atributo"
            )
            self.emit(node, name, kind)

    def visit_Attribute(self, node):
        name = self.canonical(node)
        if name:
            parent = self.parents[-2] if len(self.parents) > 1 else None
            kind = (
                "chamada" if isinstance(parent, ast.Call) and parent.func is node
                else "definicao" if isinstance(node.ctx, (ast.Store, ast.Del))
                else "atributo"
            )
            self.emit(node, name, kind)
        self.visit(node.value)

    def visit_Call(self, node):
        if (
            isinstance(node.func, ast.Name)
            and node.func.id == "getattr"
            and len(node.args) >= 2
            and isinstance(node.args[1], ast.Constant)
            and isinstance(node.args[1].value, str)
        ):
            value = node.args[1].value
            if value in NAMES:
                parent = self.parents[-2] if len(self.parents) > 1 else None
                kind = (
                    "chamada" if isinstance(parent, ast.Call) and parent.func is node
                    else "string"
                )
                self.emit(node.args[1], value, kind)
        self.generic_visit(node)

    def visit_Dict(self, node):
        for key, value in zip(node.keys, node.values):
            if isinstance(key, ast.Constant) and isinstance(key.value, str):
                if key.value in NAMES:
                    self.emit(key, key.value, "string")
            self.visit(key)
            self.visit(value)

    def visit_Subscript(self, node):
        if isinstance(node.slice, ast.Constant) and isinstance(node.slice.value, str):
            if node.slice.value in NAMES:
                self.emit(node.slice, node.slice.value, "string")
        self.generic_visit(node)


python_rows = {"producao": set(), "testes": set()} [nao verificado]
python_sources = {}
python_trees = {}
for path in sorted(APP.rglob("*.py")):
    source = read(path)
    if source is None:
        continue
    python_sources[path] = source
    try:
        tree = ast.parse(source, filename=rel(path))
    except SyntaxError as exc:
        print(
            "ERRO_AST\t{}\t{}\t{}".format(rel(path), exc.lineno, exc.msg),
            file=sys.stderr,
        )
        continue
    python_trees[path] = tree
    census = PythonCensus(path)
    census.visit(tree)
    python_rows["testes" if is_test(path) else "producao"].update(census.rows) [nao verificado]

template_sources = {}
for path in sorted(TEMPLATES.rglob("*.html")):
    source = read(path)
    if source is not None:
        template_sources[path.relative_to(TEMPLATES).as_posix()] = source

text_rows = []
for name, source in template_sources.items():
    for number, line in enumerate(source.splitlines(), 1):
        for match in NAME_RE.finditer(line):
            text_rows.append((rel(TEMPLATES / name), number, match.group(1)))

for path in sorted(STATIC_JS.rglob("*.js")):
    source = read(path)
    if source is None:
        continue
    for number, line in enumerate(source.splitlines(), 1):
        for match in NAME_RE.finditer(line):
            text_rows.append((rel(path), number, match.group(1)))

# As arestas apontam do template que fornece a casca para quem o estende,
# e do template que inclui para o template incluído.
edges = collections.defaultdict(set)
tag_re = re.compile(r"{%\s*(extends|include)\s+(.+?)\s*%}", re.DOTALL)
literal_re = re.compile(r"""^\s*(['"])(.*?)\1""", re.DOTALL)
default_re = re.compile(r"""\bbase_tpl\s*\|\s*default\s*:\s*(['"])(.*?)\1""")

for name, source in template_sources.items():
    for match in tag_re.finditer(source):
        kind, expression = match.groups()
        literal = literal_re.match(expression)
        if literal:
            target = literal.group(2)
            if kind == "extends":
                edges[target].add(name)
            else:
                edges[name].add(target)
        elif kind == "extends" and default_re.search(expression):
            edges["base.html"].add(name)
            edges["base_app.html"].add(name)

shells = collections.defaultdict(set)
roots = {"base.html": "base", "base_app.html": "app"}
for root, shell in roots.items():
    if root not in template_sources:
        print("ROOT_AUSENTE\t{}".format(root), file=sys.stderr)
    else:
        shells[root].add(shell)


def propagate_edges():
    changed = False
    for origin, targets in edges.items():
        for target in targets:
            before = len(shells[target])
            shells[target].update(shells[origin])
            changed |= len(shells[target]) != before
    return changed


# Agrupa referências literais por função ou classe de view.
# Para render(), o template é o segundo argumento; render_to_string()
# usa o primeiro. Também aceita os respectivos argumentos nomeados.
view_templates = collections.defaultdict(set)


def literal_html(node):
    if isinstance(node, ast.Constant) and isinstance(node.value, str):
        return node.value if node.value.endswith(".html") else None
    return None


class ViewTemplates(ast.NodeVisitor):
    def __init__(self, path):
        self.path = path
        self.scope = []

    def key(self):
        return (rel(self.path), ".".join(self.scope) or "<modulo>")

    def visit_FunctionDef(self, node):
        self.scope.append(node.name)
        self.generic_visit(node)
        self.scope.pop()

    visit_AsyncFunctionDef = visit_FunctionDef

    def visit_ClassDef(self, node):
        self.scope.append(node.name)
        self.generic_visit(node)
        self.scope.pop()

    def visit_Call(self, node):
        func = dotted(node.func) or ""
        leaf = func.rsplit(".", 1)[-1]
        index = 1 if leaf == "render" else 0
        if leaf in {"render", "render_to_string"}:
            candidates = []
            if len(node.args) > index:
                candidates.append(node.args[index])
            candidates.extend(
                kw.value for kw in node.keywords
                if kw.arg in {"template_name", "template"}
            )
            for candidate in candidates:
                name = literal_html(candidate)
                if name:
                    self.view_templates_add(name)
        self.generic_visit(node)

    def visit_Assign(self, node):
        if any(
            isinstance(target, ast.Name) and target.id == "template_name"
            or isinstance(target, ast.Attribute) and target.attr == "template_name"
            for target in node.targets
        ):
            name = literal_html(node.value)
            if name:
                self.view_templates_add(name)
        self.generic_visit(node)

    def visit_AnnAssign(self, node):
        target = node.target
        if (
            isinstance(target, ast.Name) and target.id == "template_name"
            or isinstance(target, ast.Attribute) and target.attr == "template_name"
        ):
            name = literal_html(node.value)
            if name:
                self.view_templates_add(name)
        self.generic_visit(node)

    def view_templates_add(self, name):
        view_templates[self.key()].add(name)
        # Métodos e atributos da mesma classe compartilham a casca da view.
        if len(self.scope) >= 2:
            view_templates[(rel(self.path), self.scope[0])].add(name)


for path, tree in python_trees.items():
    if not is_test(path):
        ViewTemplates(path).visit(tree)

# Compartilhar cascas somente dentro da mesma view; depois propagar includes
# e herança novamente até atingir o ponto fixo.
while True:
    changed = propagate_edges()
    for names in view_templates.values():
        combined = set()
        for name in names:
            combined.update(shells[name])
        for name in names:
            before = len(shells[name])
            shells[name].update(combined)
            changed |= len(shells[name]) != before
    if not changed:
        break


def shell_name(name):
    value = shells[name]
    if value == {"base", "app"}:
        return "ambas"
    if value == {"base"}:
        return "base"
    if value == {"app"}:
        return "app"
    return "nenhuma"


print("PYTHON_PRODUCAO")
print("arquivo\tlinha\tfuncao_envolvente\tnome\ttipo")
for row in sorted(python_rows["producao"]):
    print(*row, sep="\t")

print("PYTHON_TESTES")
print("arquivo\tlinha\tfuncao_envolvente\tnome\ttipo")
for row in sorted(python_rows["testes"]): [nao verificado]
    print(*row, sep="\t")

print("HTML_JS")
print("arquivo\tlinha\tnome")
for row in sorted(text_rows):
    print(*row, sep="\t")

matched_templates = sorted({
    path[len("app/templates/"):]
    for path, _, _ in text_rows
    if path.startswith("app/templates/")
})
print("CASCAS_TEMPLATES")
print("arquivo\tcasca")
for name in matched_templates:
    print("app/templates/{}\t{}".format(name, shell_name(name)))

print("CONTAGEM_CASCA")
print("casca\ttemplates")
shell_counts = collections.Counter(shell_name(name) for name in matched_templates)
for shell in ("base", "app", "ambas", "nenhuma"):
    print("{}\t{}".format(shell, shell_counts[shell]))

print("CONTAGEM_TIPO")
print("grupo\ttipo\tocorrencias")
for group in ("producao", "testes"): [nao verificado]
    counts = collections.Counter(row[4] for row in python_rows[group])
    for kind in ("definicao", "import", "chamada", "atributo", "string"):
        print("{}\t{}\t{}".format(group, kind, counts[kind]))

print("CONTAGEM_HTML_JS")
print("tipo\tocorrencias")
print("html\t{}".format(sum(p.startswith("app/templates/") for p, _, _ in text_rows)))
print("js\t{}".format(sum(p.startswith("app/static/js/") for p, _, _ in text_rows)))
```

## 18/11 08:59 · sessao ad8a49d19ed1 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Limite da conferência:** a cópia fornecida não contém `.git`; `git rev-parse HEAD` falhou. Trato os arquivos lidos como a cópia identificada no pedido, mas a identidade independente com `dd489c627` é **[nao verificado]**. As referências `brief` e `matriz` abaixo são as linhas dos textos raspados, contadas conforme a remedição fornecida; sua correspondência com arquivos originais é **[nao verificado]**.

Julgo as **45 premissas da remedição**. `MOVEU` identifica o sítio que passou a usar outro juiz ou mudou de linha; quando a premissa era a existência do antigo redutor por atos, essa parte **caiu** mesmo no sítio movido. [nao verificado]

| # | Premissa e origem no brief/matriz | Julgamento na cópia |
|---|---|---|
| 1 | Contrato F1 `LeituraDosAtos` — brief:76–78; matriz:14,16–18 | **CAIU:** o juiz disponível é booleano sobre o par projetado (`app/chamados/juizes.py:1544`, `:1552`). |
| 2 | Redutor F1 limpa estados por ato — brief:84–88; matriz:19–20 | **CAIU** como interface demonstrada; a leitura temporal examinada usa projeção (`app/chamados/juizes.py:1389`, `:1400`). |
| 3 | Conservação por `_leitura_conservada` — brief:78,83; matriz:21–22 | **CAIU** nesse formato; existe correspondência entre projeção e linhas de decisão (`app/chamados/models.py:4121`). |
| 4 | Faces SQL R consultam atos — brief:93–94; matriz:23–25 | **CAIU:** as faces consultam os campos projetados (`app/chamados/juizes.py:1565`, `:1575`). |
| 5 | Espera ficou metade projeção, metade ato — brief:20; matriz:26–28,35 | **CAIU:** os dois termos atuais examinam a projeção (`app/chamados/services/fio.py:187`; `app/chamados/juizes.py:1531`). |
| 6 | F2 já fornece `estado` por ato — matriz:29 | **CAIU:** o estado usa campos projetados e o juiz de respondida (`app/chamados/models.py:3159`, `:3675`). |
| 7 | POST usa a leitura antiga — brief:21; matriz:36 | **MOVEU:** guarda usa `pergunta_respondida`; o redutor antigo não foi demonstrado (`app/chamados/services/fio.py:707`). |
| 8 | Canônica usa texto do redutor — brief:22; matriz:37 | **MOVEU:** seleção usa o juiz do par projetado (`app/chamados/services/fio.py:1281`). |
| 9 | Mérito perdedor usa redutor — brief:22; matriz:38 | **MOVEU:** usa o juiz do par (`app/chamados/services/fio.py:1293`). |
| 10 | Lavra sem turno lê texto do redutor — brief:23; matriz:39 | **MOVEU:** guarda de resposta usa o juiz; C continua campo projetado (`app/chamados/reconciliador.py:365`, `:368`). |
| 11 | Universo absolvível dispõe de SQL V por ato — brief:24; matriz:40 | **CAIU:** filtro usa V e C projetados (`app/chamados/reconciliador.py:673`). |
| 12 | Órfãs abertas consomem o redutor — brief:25; matriz:41 | **CAIU:** filtro é C/V, não R/C como descrevia a matriz (`app/chamados/reconciliador.py:1093`). |
| 13 | Veredito de órfã lê texto do ato — brief:26; matriz:42 | **MOVEU:** guarda usa o juiz projetivo (`app/chamados/reconciliador.py:1126`). |
| 14 | Fotos de órfãs usam `LeituraDosAtos` — brief:27; matriz:43 | **CAIU:** a trilha fotografa C e via da pergunta (`app/chamados/reconciliador.py:1189`, `:1199`). |
| 15 | C em SQL permanece — brief:28,94–95; matriz:44,90 | **CONFERE:** órfãs reatáveis filtram C (`app/chamados/reconciliador.py:1549`). |
| 16 | Explicador herda estado e resposta por ato — brief:29; matriz:45 | **CAIU:** usa `estado` e texto projetado (`app/chamados/services/explicador.py:95`, `:102`; `app/chamados/models.py:3159`). |
| 17 | Guarda de validação dispõe do instante do redutor — brief:30,87; matriz:46 | **CAIU:** testa V projetada (`app/chamados/services/declaracao_texto.py:228`). |
| 18 | Declaração compara última resposta do redutor — brief:31; matriz:47 | **MOVEU:** a guarda consulta o juiz e compara texto projetado (`app/chamados/services/declaracao_texto.py:418`). |
| 19 | Aplicador dispõe da resposta do redutor — brief:32; matriz:48 | **MOVEU:** usa o juiz antes de chamar a porta (`app/chamados/services/declaracao_texto.py:688`). |
| 20 | Causa `c` lê R cru — brief:33; matriz:49 | **MOVEU:** agora usa `pergunta_respondida` (`app/chamados/services/vigia_fio.py:636`). |
| 21 | Vigia de resposta vazia mede integridade da projeção — brief:52–54; matriz:50,89 | **CONFERE:** conta o par projetado; escolha técnica final da F5 continua **[nao verificado]** (`app/chamados/services/vigia_fio.py:739`, `:747`). |
| 22 | Resposta vazia permanece fora das pendentes — brief:85,105–108; matriz:51,69–71,82 | **CAIU na implementação para uma pergunta viva de disputa aberta:** a negação do juiz inclui o meio par nas pendentes (`app/chamados/services/pergunta_no_app.py:79`; `app/chamados/juizes.py:1544`). O corte diz fora do app; ver §4 (`app/docs/CORTES.md:509`). |
| 23 | Beco dispõe do redutor por ato — brief:37; matriz:52 | **MOVEU:** usa a negação do juiz projetivo (`app/chamados/services/becos.py:29`). |
| 24 | F1 fornece accessor ao histórico por atos — brief:38; matriz:53,85 | **CAIU:** histórico ainda emite por campos atuais (`app/chamados/services/historico.py:98`, `:109`). |
| 25 | Vigia carimbo dispõe da face V da F2 — brief:39,87; matriz:54 | **CAIU como suporte demonstrado:** o universo continua em V projetada (`app/chamados/services/vigia_carimbo.py:98`). |
| 26 | `_pela_decisao` já lê decisões — brief:46,78; matriz:55 | **CONFERE** (`app/chamados/services/tarefa_do_dia.py:142`, `:158`). |
| 27 | Ordenação DP `(ato_em, pk)` — brief:76; matriz:73 | **CONFERE** (`app/chamados/models.py:4082`). |
| 28 | C não tem ato de materialização e permanece conservado — brief:89,94–95; matriz:76 | **CONFERE** quanto à enumeração e à projeção atual (`app/chamados/models.py:4037`; `app/chamados/juizes.py:1390`). |
| 29 | Arquivos F5 não persistem diretamente R/T/V/C — brief:64–72 | **CONFERE no recorte de escrita direta examinado**; declarador e reconciliador chamam portas existentes. Exaustividade de nomes dinâmicos: **[nao verificado]** (`app/chamados/services/declaracao_texto.py:688`; `app/chamados/reconciliador.py:404`). |
| 30 | Inventário antigo esgota escritores e atos cobrem toda escrita — brief:64–72 | **CAIU:** `carimbar` pode preencher R/V e salvar; conservação também produz linhas (`app/chamados/models.py:3249`, `:3271`, `:4121`). |
| 31 | Furo sem canal e competência trancada não têm migração R/V/C própria identificada — brief:45–46; matriz:57–58 | **CONFERE nos corpos examinados** (`app/chamados/services/furo_sem_canal.py:94`; `app/chamados/services/competencia_trancada.py:276`). |
| 32 | Tarefa, cobrança e cascas herdam a partição — brief:41–46; matriz:55–56,94–95 | **MOVEU:** carga da tarefa está em `:535`; consumidores seguem delegando (`app/chamados/services/tarefa_do_dia.py:535`; `app/chamados/services/cobranca.py:48`; `app/core/views.py:253`; `app/api/views.py:998`, `:2538`). |
| 33 | F/VIA/X são classes distintas de R/V/C — brief:47–50; matriz:60 | **CONFERE como distinção de campos**; a validade normativa de toda a allowlist atual é **[nao verificado]** (`app/chamados/services/explicador.py:108`, `:113`, `:118`). |
| 34 | Guarda se chama `recusa_respondida_sem_texto` e é unilateral — brief:50; matriz:60 | **CAIU:** é `recusa_par_ilegal`, simétrica (`app/chamados/services/fio.py:1699`). |
| 35 | R3 ainda não pousou — brief:126,154–155; matriz:6–9 | **CAIU segundo o registro local**; ancestralidade Git independente **[nao verificado]** (`app/docs/RELATO.md:577`). |
| 36 | F1 antiga é a entrega ainda aguardada — brief:127,154–155; matriz:6–7 | **CAIU:** B1 registrada e juiz real diferente (`app/docs/CORTES.md:513`; `app/chamados/juizes.py:1544`). |
| 37 | F2 antiga já fornece suporte V/estado pressuposto — brief:128; matriz:108 | **CAIU:** há expand de campos, mas V e estado continuam com as leituras citadas acima (`app/chamados/models.py:4065`; `app/chamados/services/vigia_carimbo.py:98`). |
| 38 | BUG-PAR ainda está fora — brief:129; matriz:7 | **CAIU segundo o registro local**; ancestralidade independente **[nao verificado]** (`app/docs/RELATO.md:32`). |
| 39 | BUG-PAR cruza explicador/fio — brief:129–132 | **CONFERE no código atual:** testemunha e explicação existem (`app/chamados/services/fio.py:376`; `app/chamados/services/explicador.py:124`). |
| 40 | Censo e offsets antigos bastam — brief:12–15,58–62; matriz:8–10 | **CAIU:** partição e consumidor de badge têm sítios e comportamento que exigem recenso (`app/chamados/services/pergunta_no_app.py:79`, `:101`). |
| 41 | Pergunta específica original da resposta vazia foi cortada — brief:136–138; matriz:98–102,110–111 | **CONFERE como resposta escrita:** corte manda fora do app e dono admin; implementação atual merece reconciliação, §4 (`app/docs/CORTES.md:509`; `app/chamados/services/pergunta_no_app.py:82`). |
| 42 | Cobertura da mudança de badge pode ser presumida — brief:150–152 | **CAIU como dispensa de conferência:** há contrato de fases públicas e corte específico (`app/docs/BACKLOG.md:2091`; `app/docs/CORTES.md:509`). Cobertura literal do aval antigo **[nao verificado]**. |
| 43 | Ordem histórica ainda é a fila completa — brief:132,154–155; matriz:92–93 | **CAIU:** registro diz B2a no ar e B2b aberta; situação Git de RELAVRA-11 **[nao verificado]** (`app/docs/RELATO.md:5`). |
| 44 | Sombra aproximada antiga é medida atual pela função real — brief:139–146; matriz:77–78,91 | **CAIU:** o juiz disponível é outro e valores atuais são **[nao verificado]** (`app/chamados/juizes.py:1544`). |
| 45 | Há rito escrito de suíte e deploy — brief:156–157 | **CONFERE como regra escrita**, sem afirmar execução nesta leitura (`CLAUDE.md:90`, `:95`). | [nao verificado]

CONFERE 11, MOVEU 10, CAIU 24

## 2 O que mudou

**O diff real pedido permanece [nao verificado].** A cópia não é repositório Git, então não foi possível executar `git show` ou `git diff` entre `fbc5c76ce` e `dd489c627`, nem confirmar os commits do anexo independentemente. O `diffstat_F5.txt` fornecido é evidência do stat declarado, não substitui a leitura do diff real.

O confronto entre a remedição raspada e o código da cópia mostra estas mudanças de sítio e efeito; a atribuição de cada mudança a um commit específico é **[nao verificado]**: [nao verificado]

- **Leitores de resposta:** POST, canônica, mérito, lavra sem turno, veredito de órfã, declaração, beco, partição e causa `c` usam agora o juiz `pergunta_respondida`. Ele exige instante e texto na **projeção**; isso não implementa o antigo fold de atos (`app/chamados/juizes.py:1544`; `app/chamados/services/fio.py:707`, `:1281`, `:1293`; `app/chamados/reconciliador.py:365`, `:1126`; `app/chamados/services/declaracao_texto.py:418`, `:688`; `app/chamados/services/becos.py:29`; `app/chamados/services/pergunta_no_app.py:79`; `app/chamados/services/vigia_fio.py:636`).
- **V e C:** os universos de órfãs e absolvição ainda filtram V/C projetados; o histórico ainda usa os carimbos atuais. A migração geral a atos não está demonstrada (`app/chamados/reconciliador.py:673`, `:1093`, `:1549`; `app/chamados/services/historico.py:98`; `app/chamados/services/vigia_carimbo.py:98`).
- **Expand F2:** `da_ressuscitacao` e `resposta_mantem_validacao` foram acrescentados ao modelo. A resposta humana declara `False`; respostas automática e pela trilha declaram `True`; a conservação também fornece `True` (`app/chamados/models.py:4065`, `:4071`, `:4077`, `:4143`; `app/chamados/services/disputa_emissao.py:1527`, `:1681`, `:1703`). A carga da tarefa lê `es_da_ressuscitacao` e exclui o ato marcado antes de escolher o último; a própria documentação do modelo diz que a marca `True` ainda não é gravada no P1 e que não há leitor de `resposta_mantem_validacao` nesse estágio (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/models.py:4067`, `:4103`).
- **Cerca B2a:** o registro local informa pouso parcial e deixa 39 sítios para B2b; portanto, não se deve tratar a B2 inteira como concluída (`app/docs/RELATO.md:5`). O conteúdo exato do diff de cerca e seus demais escritores/leitores novos é **[nao verificado]**.
- **Duas cascas e badge:** a consulta é consumida por `core/views`, pelos dois sítios de `api/views`, por cobrança e aviso; `projecao_do_app` também a transforma em contagem e badge (`app/core/views.py:253`; `app/api/views.py:998`, `:2538`, `:315`; `app/chamados/services/cobranca.py:48`; `app/chamados/services/aviso_pendencia.py:9`; `app/chamados/services/pergunta_no_app.py:101`, `:109`). A API ainda usa o juiz para filtrar respondidas (`app/api/views.py:2578`, `:2621`).

A remedição antiga descreve outros escritores e leitores, mas uma lista **exaustiva de novos sítios no intervalo** depende do diff real indisponível: **[nao verificado]**. Números atuais de produção e execução de testes também são **[nao verificado]**. [nao verificado]

## 3 Nao verificado de ALCANCE

1. **Silêncio/absorção — ponto localizado:** o `[nao verificado]` está na matriz de transição do **brief:90**, repetido no contraexemplo do **brief:118**: “silêncio = histórico; absorção rejeitada = pendente” na partição. O fechamento registra `SILENCIO`; a materialização registra `ABSORCAO`; `_pela_decisao` lê último ato e status da ausência (`app/chamados/models.py:2011`; `app/chamados/services/materializacao.py:1048`; `app/chamados/services/tarefa_do_dia.py:158`, `:176`). A consulta primeiro restringe a perguntas vivas e a partição lê juiz de resposta e fechamento, sem ler esses atos (`app/chamados/services/pergunta_no_app.py:45`, `:61`, `:76`). **O alcance de escrita e leitura examinado fecha; a transposição normativa não fecha** e está formulada como pergunta na §4.

2. **Cascas e consumidor omitido:** fechados os sítios identificados de consulta, contagem e badge nas duas cascas, cobrança, aviso e carga da tarefa (`app/core/views.py:253`; `app/api/views.py:315`, `:998`, `:2538`; `app/chamados/services/pergunta_no_app.py:101`; `app/chamados/services/tarefa_do_dia.py:535`; `app/chamados/services/cobranca.py:48`; `app/chamados/services/aviso_pendencia.py:9`). Exaustividade de consumidores construídos dinamicamente: **[nao verificado]**; falta censo completo no ambiente com os artefatos e referências necessários.

3. **Arquivos cruzados e novos escritores/leitores no intervalo:** **[nao verificado]**. O stat fornecido identifica arquivos alterados, e os sítios atuais citados na §2 identificam parte do alcance; falta o diff real e sua conferência, inclusive para F3/F6/F8/F9.

4. **Vigia de projeção:** quem lê o par e sua finalidade estão identificados (`app/chamados/services/vigia_fio.py:739`, `:747`); se a F5 o preservará até E4d ou o substituirá continua **[nao verificado]**. Falta a decisão técnica da implementação.

**alcance nao verificado restante: 3** — itens 2, 3 e 4; o item 1 passa à pergunta de lei, sem contar sua decisão como alcance de código fechado.

## 4 Perguntas de lei

A busca em `LEIS.md`, `CORTES.md` e `DOSSIES.md` encontrou respostas escritas para os seguintes pontos:

- **DECIDIDO PELA LEI L-130:** respondida exige instante e texto não vazio, com juiz único para as finalidades listadas (`app/docs/LEIS.md:151`).
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 5:** resposta vazia fica fora do app na F5; a ação é do admin. O mesmo corte registra, sem decidir, se ela deveria voltar ao colaborador (`app/docs/CORTES.md:509`).
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 3:** o predicado de absorção por ato pertence à E4d (`app/docs/CORTES.md:509`).
- **DECIDIDO PELA LEI LU-16:** silêncio não fecha; só fato na célula (`app/docs/CORTES.md:495`). O corte posterior fornecido no brief resolve o dono admin do silêncio na tarefa; o registro local também o identifica (`app/docs/RELATO.md:202`). Essas respostas não especificam por si a classificação condicional na partição do app (`app/chamados/services/pergunta_no_app.py:76`).

Perguntas ainda abertas:

1. **Silêncio/absorção:** se uma pergunta cujo último ato é `SILENCIO` ou `ABSORCAO` chega viva à partição, a F5 deve aplicar o juiz atual de resposta e fechamento, ou os resultados propostos no brief — silêncio no histórico e absorção rejeitada pendente? O código da partição não lê esses atos, e o corte reserva o predicado de absorção por ato à E4d (`app/chamados/services/pergunta_no_app.py:76`; `app/docs/CORTES.md:509`).

2. **Resposta vazia viva e disputa aberta:** como conciliar a exclusão expressa do app no corte F5 com a L-130 e a partição atual, cuja negação do juiz inclui o par com instante e texto vazio nas pendentes? O corte decide o caso original, mas essa combinação posterior de regras e implementação não está reconciliada nas fontes examinadas (`app/docs/CORTES.md:509`; `app/docs/LEIS.md:151`; `app/chamados/services/pergunta_no_app.py:79`; `app/chamados/juizes.py:1544`).

3. **Devolução ao colaborador:** resposta vazia deve voltar ao colaborador? O corte registra expressamente a pergunta como não decidida junto de `RESSUSCITAR-DEVOLVE-AO-COLABORADOR` (`app/docs/CORTES.md:509`; `app/docs/LEIS.md:151`).

perguntas de lei: 3

## 18/11 08:59 · sessao 99111fae192f · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Limite da identificação:** a cópia não contém metadados Git; portanto, a identidade do HEAD `dd489c627`, a ancestralidade e o diff real entre hashes estão **[nao verificado]**. Os caminhos abaixo são os arquivos presentes na cópia. Os números `brief.md` e `matriz.md` referem-se ao texto fornecido, contado a partir do título; sua correspondência com arquivos externos está **[nao verificado]**.

A contagem ao fim desta seção cobre os **21 degraus D1–D21 da matriz**, cada um contado uma vez. **MOVEU** significa que o sítio ou comportamento observado mudou; **CAIU** significa que a premissa descrita para o sítio deixou de valer. O contrato antigo `atos_pergunta.leitura` é julgado separadamente, pois é uma dependência comum, não um degrau adicional.

| Degrau / premissa e sítio antigo | Julgamento na cópia |
|---|---|
| D1, validar testa somente instante de resposta — `matriz.md:16–21`, `brief.md:57` | **CAIU:** consulta `pergunta_respondida`, que exige instante e texto: `app/chamados/services/validacao.py:681`, `app/chamados/juizes.py:1544`. |
| D2, guarda de validação corrente lê V — `matriz.md:24`, `brief.md:58` | **MOVEU:** continua lendo `validada_em`: `app/chamados/services/validacao.py:685`. |
| D3, recusa de pergunta validada lê V — `matriz.md:29`, `brief.md:59` | **MOVEU:** `app/chamados/services/validacao.py:898`. |
| D4, recusa testa somente instante de resposta — `matriz.md:33`, `brief.md:59` | **CAIU:** usa o juiz de resposta: `app/chamados/services/validacao.py:901`. |
| D5, recusa fora do fato retorna antes da escrita — `matriz.md:37`, `brief.md:16–18` | **MOVEU:** `app/chamados/services/validacao.py:906`, escrita posterior em `app/chamados/services/validacao.py:916`. |
| D6, materialização conserva leitura de C — `matriz.md:39–41`, `brief.md:61` | **MOVEU:** `app/chamados/services/validacao.py:769`. |
| D7, lote ainda lê V nos dois pontos — `matriz.md:44–46` | **MOVEU:** `app/chamados/services/validacao.py:1064`, `app/chamados/services/validacao.py:1085`. |
| D8, classe lê texto projetado — `matriz.md:49` | **MOVEU:** `app/chamados/services/validacao.py:1183`. |
| D9, aceite de folga compara texto projetado — `matriz.md:53–54` | **MOVEU:** `app/chamados/services/validacao.py:465`. |
| D10, idade da fila lê instante projetado — `matriz.md:57` | **MOVEU:** `app/chamados/services/validacao.py:1256`. |
| D11, placar conta V corrente com staff — `matriz.md:71`, `brief.md:62` | **CONFERE:** `app/chamados/services/verbos.py:263`. |
| D12, aceite usa instante de resposta após cobrança — `matriz.md:77–81`, `brief.md:63` | **MOVEU:** conserva a comparação temporal, mas agora exige também o juiz SQL de resposta: `app/chamados/services/verbos.py:333`, `app/chamados/services/verbos.py:344`. |
| D13, marcador agrega R/V/C projetados — `matriz.md:84–87`, `brief.md:64` | **CONFERE:** `app/chamados/services/painel.py:64`. |
| D14, ETag consome esse marcador — `matriz.md:89` | **CONFERE:** `app/chamados/services/painel.py:91`. |
| D15, painel conta V sem exigir staff — `matriz.md:92–96`, `brief.md:62` | **CONFERE:** `app/chamados/services/painel.py:261`, `app/chamados/services/painel.py:270`. |
| D16, reabrir conta R nulo — `matriz.md:101` | **CAIU:** conta a negação SQL do juiz; o universo continua sendo todas as perguntas da disputa, antes de buscar corrigíveis: `app/chamados/services/acoes_disputa.py:97`. |
| D17, regularizar testa R nulo — `matriz.md:103` | **CAIU:** consulta o juiz nas perguntas vivas: `app/chamados/services/acoes_disputa.py:132`. |
| D18, corrigir consulta V e texto — `matriz.md:106–107` | **MOVEU:** `app/chamados/services/acoes_disputa.py:349`, `app/chamados/services/acoes_disputa.py:357`. |
| D19, marcos agregam C/R e respeitam `ate` — `matriz.md:111–113`, `brief.md:65` | **CONFERE:** `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:202`. |
| D20, A20 usa campos próprios — `matriz.md:61`, `brief.md:19–21` | **MOVEU:** `app/chamados/services/validacao.py:994`. A implementação integral de LU-08 está **[nao verificado]**; o corte diz que a conferência vale para o par conferido: `app/docs/CORTES.md:487`. |
| D21, escrita de validação/devolução e leituras internas — `matriz.md:65–67` | **MOVEU:** validação registra ato em `app/chamados/services/validacao.py:173`; devolução ainda lê C, motivo e V em `app/chamados/services/validacao.py:262` e registra ato em `app/chamados/services/validacao.py:279`. |

A F1 antiga pressuposta em `brief.md:57–59,66–67,85–87,129` e `matriz.md:9,128–129` **CAIU como contrato disponível**: o juiz encontrado responde se o par projetado tem instante e texto; a cópia não tem `app/chamados/services/atos_pergunta.py` **[nao verificado quanto ao histórico]**. Ele não demonstra o antigo redutor de validação corrente: `app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1551`.

O teste antigo “ato RESPOSTA presente com projeção vazia deve validar” não decorre desse juiz; em contrapartida, a premissa anterior de que as portas aceitavam instante sem texto também caiu: `app/chamados/juizes.py:1552`, `app/chamados/services/validacao.py:681`.

**CONFERE 5, MOVEU 12, CAIU 4.**

## 2 O que mudou

O anexo `diffstat_F3.txt` informa alterações em 17 arquivos entre `fbc5c76ce` e `dd489c627`; **o diff real e a atribuição dessas alterações a commits não puderam ser lidos**, porque a cópia não é um repositório Git **[nao verificado]**. A comparação abaixo usa a remedição fornecida como descrição da base antiga e os corpos presentes como estado observado.

- **Resposta nas portas e ações:** a remedição descrevia guardas por instante; agora validar, recusar, reabrir e regularizar consultam o juiz de resposta, inclusive sua face SQL: `app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:901`, `app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/acoes_disputa.py:133`. Isso altera o tratamento de instante sem texto; não entrega a leitura histórica de atos pretendida pela F3: `app/chamados/juizes.py:1544`.
- **Aceite:** além da comparação com a última cobrança, há agora `q_respondida_com_texto`; colaboradores continuam deduplicados: `app/chamados/services/verbos.py:333`, `app/chamados/services/verbos.py:339`.
- **Novo escritor encontrado na cópia:** a porta de estoque L-130 conserva linhas faltantes e zera o par de resposta pelo lote: `app/chamados/services/validacao.py:408`, `app/chamados/services/validacao.py:436`. A recusa de pares ilegais é consultada no nascimento, no lote e no `save`: `app/chamados/models.py:2956`, `app/chamados/models.py:3381`, `app/chamados/models.py:3599`.
- **Novos campos e leitor de atos:** o modelo declara `da_ressuscitacao` e `resposta_mantem_validacao`; a tarefa consulta o primeiro, e os escritores de resposta declaram o segundo: `app/chamados/models.py:4065`, `app/chamados/services/tarefa_do_dia.py:421`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1679`. Isso é relevante para qualquer futura leitura da sequência; não demonstra que a F3 já a consome.
- **Leitores de placar e marcos:** permanecem projetivos nos sítios observados: `app/chamados/services/verbos.py:263`, `app/chamados/services/painel.py:64`, `app/chamados/veredito_lavrado.py:202`. A ausência de mudança histórica nesses arquivos está **[nao verificado]** sem o diff.
- **Furo do carimbo:** a remedição antiga relata que `carimbar(..., resposta='validar')` escreve V sem registrar `VALIDACAO`; o método atual e a chamada de absorção estão em `app/chamados/models.py:3222` e `app/chamados/services/materializacao.py:1048`. A quantidade atual de casos está **[nao verificado]**.

Os números de frota do brief são resultados históricos fornecidos, não medições atuais. Banco, logs e execução de testes: **[nao verificado]**. [nao verificado]

## 3 Nao verificado de ALCANCE

- **Duas cascas de foto — fechado estaticamente:** a rota do kernel inclui `api.urls_core`, que aponta a foto para `views_core`; as duas views chamam o serviço comum, que chama `validar_pergunta`: `app/config/urls_core.py:6`, `app/api/urls_core.py:8`, `app/api/views_core.py:844`, `app/api/views.py:860`, `app/api/foto_recebida.py:51`. Alcance efetivamente percorrido em produção: **[nao verificado]**.
- **Classe e folga fora da fatia — fechado nos chamadores identificados:** mensageria chama `classe_do_lote`; `vigia_fio` importa e chama o mesmo helper de aceite de folga usado pela validação: `app/api/views_mensageria.py:545`, `app/chamados/services/vigia_fio.py:247`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/services/validacao.py:711`.
- **Marcos e placar — fechado nos consumidores citados:** raio-X, fio, tarefa e auditoria consomem `marcos_do_lote`; painel, views e retrato consomem os placares: `app/chamados/services/raiox.py:38`, `app/chamados/services/fio.py:282`, `app/chamados/services/tarefa_do_dia.py:440`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`, `app/chamados/services/painel.py:298`, `app/chamados/views.py:1488`, `app/folha/views_retrato.py:515`. O retorno de marcos é temporal e é comparado a `julgado_em`: `app/chamados/veredito_lavrado.py:152`, `app/chamados/veredito_lavrado.py:232`.
- **Escritores e leitores globais — aberto:** a remedição fornece um censo anterior, mas não foi possível reconferir integralmente cada campo, alias, escrita ORM e template contra o delta real. Faltam histórico Git e um novo censo completo da cópia; nomes construídos dinamicamente também permanecem **[nao verificado]**. Exemplos novos observados estão em `app/chamados/services/validacao.py:408` e `app/chamados/services/tarefa_do_dia.py:421`.
- **Arquivos cruzados com B2/F2/F4/F7/F5/F6/F8/F9 — aberto:** a tabela histórica da remedição não prova os diffs ou escopos finais dessas obras. Faltam seus diffs e briefs correspondentes **[nao verificado]**.
- **Retroatividade de D06 — aberto como verificação de alcance e contrato:** `marcos_do_lote` exige `datetime` e corte `ate`, enquanto a conservação pode inserir uma linha com instante antigo: `app/chamados/veredito_lavrado.py:151`, `app/chamados/models.py:4122`. Falta demonstrar como uma alteração retroativa alcançará cada consumidor preservando esses contratos **[nao verificado]**.

**alcance nao verificado restante: 3** — censo global, cruzamento final das obras e propagação de ato retroativo. A ocorrência real das rotas em produção também permanece **[nao verificado]**, mas não impede o fechamento estático das duas cascas.

## 4 Perguntas de lei

A busca por resposta em `LEIS.md`, `CORTES.md` e `DOSSIES.md` encontrou:

- **DECIDIDO PELA LEI L-130:** resposta corrente exige instante e texto; o corte registra a mesma definição: `app/docs/LEIS.md:151`, `app/docs/CORTES.md:507`.
- **DECIDIDO PELA LEI LU-08:** conferência vale para o par conferido: `app/docs/CORTES.md:487`.
- **DECIDIDO PELO CORTE D04:** duas validações com devolução no meio contam como dois atos; o registro localizado está no backlog, não nas três fontes de lei pesquisadas: `app/docs/BACKLOG.md:2137`. A exceção por mesmo request não aparece nesse registro **[nao verificado]**.
- O requisito de staff é identificado no código por `EQUIPE-STAFF-0708`, mas o literal desse corte nas três fontes pesquisadas está **[nao verificado]**: `app/chamados/services/verbos.py:265`.
- O brief determina contar resposta posterior à cobrança sem filtro de autor (`brief.md:63`); `ato_por` admite nulo: `app/chamados/models.py:4054`. Essa definição fornecida não exige presumir a identidade do autor nulo.

Perguntas ainda abertas, sem propor resposta:

1. **Qual é o predicado autorizado para validação corrente a partir da sequência de atos?** O contrato antigo não está disponível na cópia, e as guardas ainda usam V projetada: `app/chamados/juizes.py:1544`, `app/chamados/services/validacao.py:685`, `app/chamados/services/validacao.py:898`.
2. **No aceite por atos, uma resposta histórica posterior à cobrança conta mesmo se a resposta corrente não satisfaz L-130?** O brief pede ato posterior à cobrança (`brief.md:63`), enquanto a implementação atual exige o juiz de resposta corrente: `app/chamados/services/verbos.py:333`, `app/docs/LEIS.md:151`. A conciliação dessas duas respostas candidatas está **[nao verificado]**.

perguntas de lei: 2

## 18/11 08:59 · sessao c4e7a94d2442 · - · - · origem=Ronald (tmux)

Resultados enviados ao agente principal, com referências aos predicados e às origens das atas.

## 18/11 08:58 · sessao c7370679eb16 · - · - · origem=Ronald (tmux)

Rastreamento da pergunta 3 enviado ao agente principal, com referências aos quatro consumidores de `_protesta`, aos três ramos de `classificar_dia`, aos leitores de `cobrado` identificados e à cadeia de fechamento, pauta e perguntas. A exaustividade dos leitores indiretos permanece não verificada.

_6826 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
