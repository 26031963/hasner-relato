# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 20:14** · publicado em **18/11/2026 20:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 20:14 · sessao 2830c5318874 · E4C-F9 · verificador · origem=Code

**Resultado: BLOQUEIA.** Os sete pontos M1–M7 foram alterados nos destinos indicados, mas há três achados procedentes: M7 pode exibir uma resposta diferente daquela usada para classificar e validar; o selo AST não cobre todas as formas de leitura crua; e faltam os comentários exigidos nos dois comandos FICA que o próprio brief proíbe editar. ([validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [test_e4c_f9_comandos_leem_o_ato.py:123](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:123), [backfill_decisoes_pergunta.py:56](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [curar_perguntas_em_limbo.py:46](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/curar_perguntas_em_limbo.py:46); brief fornecido [não verificado como arquivo])

Usei a cópia indicada, cujo `HEAD` corresponde ao hash informado; os resultados de execução abaixo são os **vereditos fornecidos**, não execuções independentes. [não verificado]

1. **M1–M7 e FICA**

| Item | Resultado |
|---|---|
| **M1 — OK** | O rótulo consulta `leitura(_p).validada_em`. ([auditar_invariantes_chamados.py:206](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:206)) |
| **M2 — OK** | I21 usa `q_nao_validada_pelo_ato()`. ([auditar_invariantes_chamados.py:1586](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1586)) |
| **M3 — OK** | C2 usa a mesma face SQL. ([auditar_invariantes_chamados.py:1617](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1617)) |
| **M4 — OK** | F2 usa a mesma face SQL. ([auditar_invariantes_chamados.py:1781](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1781)) |
| **M5 — OK** | F3 usa a mesma face SQL. ([auditar_invariantes_chamados.py:1794](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1794)) |
| **M6 — OK quanto à troca solicitada** | O filtro usa a mesma face SQL e conserva o filtro de materialização. O efeito da escrita está no item 2. ([encerrar_perguntas_com_fato.py:31](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:31)) |
| **M7 — BLOQUEIA pela composição do comando** | A impressão usa o texto do ato, como pedido, mas a classificação continua usando a coluna; veja item 5. ([validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [validar_classe_a.py:80](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:80)) |

**FICA — BLOQUEIA por instruções incompatíveis.** Não identifiquei mudança de predicado nos FICA do diff: I18 e I20 permanecem na projeção, com comentários; os dois comandos FICA externos aos três comandos alterados também permaneceram sem alteração. Porém, neles faltam os comentários de uma linha pedidos pelo brief, que ao mesmo tempo proíbe editá-los. ([auditar_invariantes_chamados.py:1081](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1081), [auditar_invariantes_chamados.py:1113](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1113), [backfill_decisoes_pergunta.py:56](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [curar_perguntas_em_limbo.py:46](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/curar_perguntas_em_limbo.py:46); brief fornecido [não verificado como arquivo])

2. **M6 e `--apply` — OK quanto à classe do efeito; impacto em produção não verificado**

Entra no universo a pergunta **materializada** cuja coluna `validada_em` está preenchida, mas cuja validação vigente foi desfeita por ato posterior — por exemplo, uma validação seguida de devolução. Sai a materializada com coluna nula e validação vigente na sequência. A face também considera o passivo da projeção sem linha correspondente; portanto, coluna preenchida sem ato não entra automaticamente. As demais condições do comando continuam aplicáveis. ([models.py:3539](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3539), [models.py:3551](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3551), [encerrar_perguntas_com_fato.py:31](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:31))

Com `--apply`, entre as elegíveis, o comando exige via sem história e marco aceso; chama a porta que troca `via_resolucao`, registra evento e pode fechar a disputa pela porta com suas guardas. A troca de via demonstrada não planta batida nem altera diretamente resposta ou validação. ([encerrar_perguntas_com_fato.py:41](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:41), [encerrar_perguntas_com_fato.py:66](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:66), [encerrar_perguntas_com_fato.py:81](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:81), [models.py:3489](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3489))

Quantidade de perguntas que entram ou saem em produção e efeito final sobre dinheiro, folha, retenção ou competência entregue: **[não verificado]**. O teste novo cobre o universo em *dry-run*, não o resultado de `--apply`. ([test_e4c_f9_comandos_leem_o_ato.py:96](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:96)) [nao verificado]

3. **Selo e mutante — BLOQUEIA**

O selo tem allowlist por arquivo, função, campo e ancestral `add('I18'/'I20')`; ele detecta atributos e palavras-chave ORM, mas não detecta, por exemplo, nomes de campo passados como strings a `values()` ou `values_list()`. Também permite ocorrências adicionais do campo dentro do mesmo `add` permitido. Assim, não garante a proibição geral de leituras cruas pedida no brief. ([test_e4c_f9_comandos_leem_o_ato.py:123](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:123), [test_e4c_f9_comandos_leem_o_ato.py:145](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:145))

**O mutante literal de M2 morreria:** `validada_em__isnull=True` é uma keyword capturada pelo selo e rejeitada fora de I18; o teste funcional de M2 também espera incluir a pergunta divergente que esse filtro excluiria. ([test_e4c_f9_comandos_leem_o_ato.py:51](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:51), [test_e4c_f9_comandos_leem_o_ato.py:70](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:70), [test_e4c_f9_comandos_leem_o_ato.py:133](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:133)) [nao verificado]

O anexo relata RED `rc=1`, GREEN `rc=0` e falha para cada mutante M1–M7; sem os logs completos, a causa individual de cada falha é **[não verificado]**. Para M2–M5, o teste extrai e executa isoladamente o predicado de validação; M7 simula o classificador. ([test_e4c_f9_comandos_leem_o_ato.py:23](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:23), [test_e4c_f9_comandos_leem_o_ato.py:49](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:49), [test_e4c_f9_comandos_leem_o_ato.py:112](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:112); vereditos fornecidos [não verificado como arquivo]) [nao verificado]

4. **Classes de reprovação**

- **Segundo juiz — OK:** as trocas chamam faces existentes; o juiz de resposta registrado continua sendo `pergunta_respondida`. ([models.py:3539](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3539), [core/juizes.py:306](/home/ronald/wt-codex-e4c-f9-191111/app/core/juizes.py:306))
- **Segundo escritor — OK no diff:** M6 continua chamando a porta existente e a sequência tem `registrar` como porta de escrita. ([encerrar_perguntas_com_fato.py:69](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:69), [models.py:4329](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4329))
- **Relógio — OK:** o teste novo usa datas fixas e congela `AGORA`; não contém chamada a `timezone.now()`. ([test_e4c_f9_comandos_leem_o_ato.py:17](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:17), [test_e4c_f9_comandos_leem_o_ato.py:59](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:59), [cortes_f2_f9_apoio.py:30](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/cortes_f2_f9_apoio.py:30)) [nao verificado]
- **Derivação paralela — BLOQUEIA em M7**, pela divergência concreta entre impressão e classificação descrita no item 5; não foi criada uma função nova de juiz. ([validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [juizes.py:2034](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:2034))
- **Selo/RED — BLOQUEIA pelo alcance do selo:** o mutante literal morde, mas a cobertura estrutural não é completa, conforme item 3. ([test_e4c_f9_comandos_leem_o_ato.py:127](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:127))
- **Ordem de trava — OK quanto ao diff:** não há alteração da ordem de travas nas portas existentes; segurança concorrente de M6 sob `--apply` **[não verificado]**. ([encerrar_perguntas_com_fato.py:66](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:66))
- **Semântica — FILA:** os nomes representam os mesmos conceitos de validação e resposta vigentes, mas não necessariamente os mesmos valores quando projeção e atos divergem. DEVOLUÇÃO limpa validação e conserva resposta; REABERTURA limpa ambas. A leitura ainda incorpora o passivo projetado sem linha. ([models.py:4272](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4272), [models.py:4295](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4295), [models.py:4317](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4317))

5. **Quem escreve, quem lê e discordância — BLOQUEIA**

A validação humana escreve `validada_em` e registra VALIDACAO; devolução limpa a coluna e registra DEVOLUCAO. A resposta humana escreve `resposta_colab`, pode limpar validação e registra RESPOSTA; a reabertura limpa o par e registra o ato. Há outros escritores da projeção, inclusive `carimbar`; `registrar` grava as linhas da sequência. ([validacao.py:168](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/validacao.py:168), [validacao.py:273](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/validacao.py:273), [disputa_emissao.py:1481](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/disputa_emissao.py:1481), [disputa_emissao.py:1617](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/disputa_emissao.py:1617), [models.py:3299](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3299), [models.py:4329](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4329))

Os M passam a ler linhas por `leitura` ou pela sua face SQL: ambas consideram VALIDACAO, atos que a limpam e o passivo projetado; `leitura` também processa RESPOSTA e os atos que a limpam. O juiz registrado de respostas sem veredito já lê `leitura` e aplica `pergunta_respondida`. ([models.py:3542](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3542), [models.py:4308](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4308), [models.py:2003](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:2003), [core/juizes.py:308](/home/ronald/wt-codex-e4c-f9-191111/app/core/juizes.py:308))

**Discordância concreta:** em M7, `respostas_sem_veredito()` seleciona pela resposta vigente do ato, a impressão mostra essa resposta, mas `classificar_resposta_validacao(q)` lê `q.resposta_colab` da projeção. Para uma pergunta de escolha com texto do ato divergente do texto projetado, a classe pode corresponder a uma resposta enquanto a linha impressa mostra outra; com `--apply`, a classe alimenta a chamada de validação. O teste mascara o classificador, portanto não cobre essa composição. ([validar_classe_a.py:54](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:54), [validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [validar_classe_a.py:97](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:97), [juizes.py:2034](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:2034), [test_e4c_f9_comandos_leem_o_ato.py:112](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:112)) [nao verificado]

**FILA:** M2 e M3 também combinam a validação pelo ato com faces SQL de resposta que leem a projeção; em divergência de resposta, isso pode produzir universo diferente de um leitor que passe `LeituraDoAto` ao juiz. A migração dessas faces não foi autorizada nesta fatia. ([auditar_invariantes_chamados.py:1586](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1586), [auditar_invariantes_chamados.py:1617](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1617), [juizes.py:1575](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:1575), [models.py:2003](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:2003))

6. **LEI-AKITA — FILA:** origem, testemunhas, teste e zero juiz novo cabem na forma exigida; `quem-mais-le=<censo>` fica incompleto sem entregar o censo efetivo e incluir a discordância acima. O padrão existe no glossário e no plano como CQRS/projeção reconstruível; isso não demonstra, por si só, que esta obra implementou um mecanismo novo de reconstrução. ([CLAUDE.md:62](/home/ronald/wt-codex-e4c-f9-191111/CLAUDE.md:62), [GLOSSARIO.md:24](/home/ronald/wt-codex-e4c-f9-191111/app/docs/GLOSSARIO.md:24), [PLANO-PADROES.md:33](/home/ronald/wt-codex-e4c-f9-191111/app/docs/PLANO-PADROES.md:33)) [nao verificado]

**Fecho: BLOQUEIA procedentes = 3; FILA = 2.** Os bloqueios são M7, alcance do selo e comentários FICA incompatíveis com o escopo de arquivos; as filas são a composição das faces SQL de resposta e o censo completo da linha LEI-AKITA. ([validar_classe_a.py:63](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:63), [test_e4c_f9_comandos_leem_o_ato.py:123](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:123), [backfill_decisoes_pergunta.py:56](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [juizes.py:1575](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/juizes.py:1575), [CLAUDE.md:62](/home/ronald/wt-codex-e4c-f9-191111/CLAUDE.md:62))

## 18/11 20:13 · sessao 4cc4e412b975 · E4C-F6F7 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: PROCEDE.** O defeito localizado é a identificação do corte das 10:10: a cópia registra seu item 1 como devolvido por questão de casador sem órfã→marco, e o texto integral desse corte não está disponível para confirmar a regra atribuída a ele. A condição de parada específica das linhas 163–167 **não** foi acionada: elas conferem com a triagem. ([BACKLOG.md:2331](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/docs/BACKLOG.md:2331); [tarefa_do_dia.py:163](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:163))

**1. RESPOSTA — PROCEDE.**

1. **Premissas.** A triagem descreve corretamente o código. O trecho literal é:

   ```python
   k = len(decisoes)
   while k and decisoes[k - 1].ato == D.RESPOSTA and decisoes[k - 1].resposta_mantem_validacao:
       k -= 1
   if 0 < k < len(decisoes) and decisoes[k - 1].ato == D.VALIDACAO:
       ult = decisoes[k - 1]
   ```

   O laço percorre a cauda, mas só troca `ult` quando o ato imediatamente anterior é `VALIDACAO`. ([tarefa_do_dia.py:163](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:163))

   `DecisaoPergunta.leitura(p)` existe e retorna `LeituraDoAto` com `validada_em`; ela incorpora também linhas passivas da projeção. ([models.py:19](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:19); [models.py:4295](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4295))

   A atribuição da transparência ao **item 1 de RESPOSTAS-DE-LEI-1811-1010** fica **[não verificado]**: o registro encontrado identifica o item 1 devolvido pelo tema do casador, sem apresentar a regra atribuída pelo aval. ([BACKLOG.md:2331](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/docs/BACKLOG.md:2331))

   Hoje, `V → SILENCIO` termina em `silencio_decidido, []`. `ABSORCAO` e `RETRATACAO` existem como atos; a primeira tem ramos conforme o estado da ausência, enquanto a segunda cai no retorno `ato_desconhecido:retratacao, None`. ([models.py:4152](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4152); [tarefa_do_dia.py:185](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:185))

2. **Aval vigente.** O anexo determina pouso depois da simulação, compatível com a trava de véspera para folha, exportação, retenção e DiaPago. A compatibilidade integral com o item F9 da c12 e com o texto do REPROVAS é **[não verificado]**; o anexo exige refazer o DIFF-RETIDO. ([CORTES.md:537](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/docs/CORTES.md:537); anexo `resposta_lei_f6f7_3_1811_2015.txt`, itens 1 e 5, fornecido no pedido, sem numeração de linhas)

3. **Literal.** O aval manda usar o último ato que não é `RESPOSTA(mantem)` **se** a leitura estiver validada e houver essa cauda. Para `RECUSA`, `DEVOLUCAO` ou `REABERTURA` como último ato anterior, os ramos atuais existem, mas esses atos limpam a validação na leitura; a possibilidade de a leitura ainda dizer validada, inclusive por linha passiva, é dependente da sequência completa **[não verificado]**. Se não houver cauda `R(mantem)`, a instrução de pular a cauda não se aplica; o aval não enuncia outra troca de ato para esse caso. ([models.py:4277](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4277); [models.py:4308](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4308); [tarefa_do_dia.py:172](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:172); anexo, item 1, sem numeração de linhas)

4. **Alcance.** A mudança proposta pode alterar a porta publicada e seus leitores: `fora_da_folha`, retrato e cadastro a consomem. O veredito de exportação é obtido de `classificar_export` e os dias retidos de `_dias_que_retem`; portanto, mudança na porta **não prova**, por si, mudança no conjunto retido. Retenção efetiva, dinheiro e efeito em produção: **[não verificado]**. ([fora_da_folha.py:71](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/folha/fora_da_folha.py:71); [fora_da_folha.py:272](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/folha/fora_da_folha.py:272); [retrato_folha.py:1213](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/folha/retrato_folha.py:1213); [cadastro_realidade.py:472](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/escala/services/cadastro_realidade.py:472))

**2. RED — NAO PROCEDE.**

1. Os comportamentos atuais relevantes estão nos ramos de resposta, validação e silêncio; para resposta com texto, `V → S → R(mantem)` hoje chega a `aguarda_validacao`. ([tarefa_do_dia.py:163](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:163))
2. Colisão comprovada com a janela de pouso: não identificada; execução e pouso continuam sujeitos ao corte de véspera. ([CORTES.md:537](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/docs/CORTES.md:537))
3. `R que DESFAZ` é distinguível: `resposta_mantem_validacao=False`, e a leitura limpa a validação. Os resultados vermelho, verde e mutante morto são **[não verificado]**. ([models.py:4195](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4195); [models.py:4280](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4280))
4. Efeito dos testes sobre exportação ou dinheiro: **[não verificado]**. [nao verificado]

**3. SELO — NAO PROCEDE.**

1. A leitura do ato existe; o selo proposto ainda não tem resultado apresentado no dado fornecido. ([models.py:4295](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4295); anexo, item 3, sem numeração de linhas)
2. Colisão com aval vigente: **[não verificado]**.
3. A capacidade da catraca AST proposta de detectar todas as formas indiretas de decidir validação é **[não verificado]**; a triagem também registra mutação indireta como não medida. (Anexo `f6f7.triagem`, parágrafo “Postmortem F6F7-4”, sem numeração de linhas)
4. Alteração de exportação, retenção ou dinheiro pelo selo: **[não verificado]**.

**4. MEDIR — NAO PROCEDE.**

1. A medição pedida é distinta da cura: o anexo pede duas contagens e declara que a segunda não será curada aqui. (Anexo, item 4, sem numeração de linhas)
2. Colisão comprovada com o corte de véspera: não identificada no pedido de medição; condições de execução da sonda em produção são **[não verificado]**. ([CORTES.md:537](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/docs/CORTES.md:537))
3. As duas contagens não constam do dado raspado: **[não verificado]**.
4. O efeito material e o conjunto retido dessas perguntas são **[não verificado]**; a tarefa e o classificador de exportação têm papéis separados no leitor citado. ([fora_da_folha.py:272](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/folha/fora_da_folha.py:272))

**5. DIFF-RETIDO — NAO PROCEDE.**

1. O anexo manda refazer o diff `main × ponta` na ponta nova; a triagem diz que ele não foi medido nela. (Anexo, item 5; `f6f7.triagem`, parágrafo “Postmortem F6F7-1”, ambos sem numeração de linhas)
2. A concordância literal com REPROVAS item 1(b), inclusive a dispensa de repetir o verificador extra, é **[não verificado]**: o texto integral desse aval não está no dado fornecido.
3. O resultado do diff e o delta ainda são **[não verificado]**.
4. Se o retido muda, quanto muda e se há efeito em dinheiro: **[não verificado]**. ([fora_da_folha.py:272](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/folha/fora_da_folha.py:272))

A identidade Git da cópia solicitada também é **[não verificado]**: o diretório fornecido não contém metadados Git acessíveis; as referências acima são aos arquivos presentes nessa cópia.

## 18/11 20:11 · sessao 69bb51b09ccf · RAIA-ESPURIAS · leitura · origem=Code

A régua escolhida para (a) é **10 minutos por ponta**, inclusive, usando `TOLERANCIA_CONFORMIDADE_MIN`; a constante é usada para pontualidade por marcação, e sua aplicação à comparação com o DNA vizinho é uma **escolha operacional desta medida**, não uma regra de fase já comprovada. [app/ponto/motor_calculo_v2.py:52](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/motor_calculo_v2.py:52) [app/ponto/motor_calculo_v2.py:60](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/motor_calculo_v2.py:60)

Para (e), uso **60 segundos, inclusive**, pois a porta de registro procura batida viva do mesmo tipo nessa janela; `JANELA_CLUSTER_MIN=15` agrupa corridas de órfãs e responde a outra pergunta. [app/ponto/registro_batida.py:118](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/registro_batida.py:118) [app/ponto/registro_batida.py:130](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/registro_batida.py:130) [app/escala/orfa_classe.py:28](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:28) [app/escala/orfa_classe.py:41](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:41)

**As contagens do anexo e a execução do script abaixo estão [não verificado].** O script faz a conferência contra 283 dias e 645 órfãs, mas imprime os valores calculados mesmo quando diferem. [não verificado]

Para tornar a proposta da volta 7 executável, o script explicita estas interpretações: [não verificado]

- Um par observável é `E→S` **consecutivo na sequência completa de batidas**, com ambas as pontas sem `*`; para (a)/(b), a entrada deve estar em `[+0]`. Para (c), um par pode começar em `[-1]` e terminar em `[+0]`.
- A compatibilidade compara cada par com um dos períodos `hi→hii`, `hfi→hf`, ou com `hi→hf` quando não há intervalo declarado. Isso trata cada par como unidade observável; não comprova uma jornada integral.
- A correspondência usa minuto e tipo de `cas`. Se o tipo for `-`, só admite a batida quando houver uma única candidata naquele minuto; zero ou várias candidatas vão a (f). Qualquer outra ambiguidade de correspondência também vai a (f).
- (c) exige exatamente uma órfã na linha. Uma linha com várias órfãs e alguma ponta sem par recebe o motivo de sequência incompleta em (f), salvo quando uma única classe entre (a), (b), (d) e (e) se aplicar. Sobreposições sempre vão a (f).
- `destino=sim` e `destino=nao medido` são lidos como rótulos do anexo; “desconhecido” não significa ausência de destino. Dono é o campo informado na linha, não uma atribuição causal derivada da classe.

```python
#!/usr/bin/env python3
"""Censo somente em memoria do formato s2_v4_compacto.txt.

Uso: python3 censo.py /caminho/s2_v4_compacto.txt
Nao importa Django, nao acessa banco e nao escreve arquivos.
"""
import re
import sys
from collections import Counter
from datetime import date

DNA_MIN = 10       # ponto/motor_calculo_v2.py:60; aplicacao aqui: escolha da medida
ECO_SEG = 60       # ponto/registro_batida.py:130-139; limite inclusivo

BAT_RE = re.compile(
    r"\[([+-]\d+)\]([ES])(\d{2}):(\d{2}):(\d{2})"
    r"(?:\([^)]*\))?(\*)?"
)
CAS_RE = re.compile(
    r"(?<!\S)(\d{2}:\d{2}):([^/\s]+)/([ES-])"
    r"(?:\([^)]*\))?"
)
DNA_RE = re.compile(
    r"dna([+-]1)=([TF])(?:\s+"
    r"(\d{2}:\d{2}|None)-(\d{2}:\d{2}|None)-"
    r"(\d{2}:\d{2}|None)-(\d{2}:\d{2}|None))?"
)
HEAD_RE = re.compile(
    r"^(col\d+)\s+(\d{4}-\d{2}-\d{2})\s+"
    r"(folga|ausencia)\s+(ESTRUTURA|CADASTRO|BATIDA)\s+"
    r"(sim|nao medido)\s+p=(\d+)\s+c=(\d+)$"
)
CLASSES = "abcdef"


def erro(linha, mensagem):
    raise ValueError("linha %s: %s" % (linha, mensagem))


def minuto(texto):
    h, m = map(int, texto.split(":"))
    if not (0 <= h <= 23 and 0 <= m <= 59):
        raise ValueError("horario invalido: " + texto)
    return h * 60 + m


def segundos(h, m, s):
    h, m, s = int(h), int(m), int(s)
    if not (0 <= h <= 23 and 0 <= m <= 59 and 0 <= s <= 59):
        raise ValueError("horario de batida invalido")
    return h * 3600 + m * 60 + s


def parse_linha(texto, numero):
    partes = [p.strip() for p in texto.split("|")]
    if len(partes) != 5:
        erro(numero, "esperados cinco campos separados por |")
    cab = HEAD_RE.fullmatch(partes[0])
    if not cab:
        erro(numero, "cabecalho da linha invalido")
    col, dia, tipo_dia, dono, destino, perguntas, chamados = cab.groups()
    date.fromisoformat(dia)

    orfas = partes[1].split(",") if partes[1] else []
    if not orfas or any(not re.fullmatch(r"\d{2}:\d{2}", x) for x in orfas):
        erro(numero, "lista de orfas invalida ou vazia")
    for x in orfas:
        minuto(x)

    if not partes[2].startswith("cas="):
        erro(numero, "campo cas ausente")
    cas_texto = partes[2][4:].strip()
    cas = list(CAS_RE.finditer(cas_texto))
    resto = CAS_RE.sub("", cas_texto).strip()
    if resto:
        erro(numero, "trecho cas nao interpretado: " + resto)
    cas_por_hora = {}
    for m in cas:
        hora, resultado, tipo = m.groups()
        cas_por_hora.setdefault(hora, []).append((resultado, tipo))
    if Counter(orfas) != Counter(m.group(1) for m in cas):
        erro(numero, "minutos de orfas e cas nao coincidem")

    if not partes[3].startswith("bat="):
        erro(numero, "campo bat ausente")
    bat_texto = partes[3][4:].strip()
    achadas = list(BAT_RE.finditer(bat_texto))
    resto = BAT_RE.sub("", bat_texto).strip()
    if resto:
        erro(numero, "trecho bat nao interpretado: " + resto)
    bat = []
    for m in achadas:
        desloc, tipo, h, mi, s, estrela = m.groups()
        desloc = int(desloc)
        if desloc not in (-1, 0, 1):
            erro(numero, "deslocamento de batida fora de -1..+1")
        seg_dia = segundos(h, mi, s)
        bat.append({
            "dia": desloc,
            "tipo": tipo,
            "seg": desloc * 86400 + seg_dia,
            "hora": "%s:%s" % (h, mi),
            "estrela": bool(estrela),
        })
    bat.sort(key=lambda b: b["seg"])

    dnas = {}
    matches = list(DNA_RE.finditer(partes[4]))
    resto = DNA_RE.sub("", partes[4]).strip()
    if resto:
        erro(numero, "trecho dna nao interpretado: " + resto)
    for m in matches:
        lado, trabalha, *horarios = m.groups()
        if lado in dnas:
            erro(numero, "DNA repetido: " + lado)
        if trabalha == "T":
            if not all(horarios):
                erro(numero, "DNA T sem quatro marcos")
            dnas[lado] = horarios
        else:
            if any(horarios):
                erro(numero, "DNA F com marcos")
            dnas[lado] = None
    if set(dnas) != {"-1", "+1"}:
        erro(numero, "faltam os dois DNAs vizinhos")

    # Cada ocorrencia de cas pertence a uma ocorrencia de orfa de mesmo minuto.
    # Minutos repetidos nao permitem atribuir as ocorrencias com seguranca.
    motivos = []
    if any(n > 1 for n in Counter(orfas).values()):
        motivos.append("orfas repetidas no mesmo minuto")
    correspondencias = []
    usados = set()
    for hora in orfas:
        itens = cas_por_hora[hora]
        if len(itens) != 1:
            correspondencias.append(None)
            continue
        _, tipo = itens[0]
        candidatos = [
            i for i, b in enumerate(bat)
            if b["dia"] == 0 and b["hora"] == hora
            and (tipo == "-" or b["tipo"] == tipo)
        ]
        if len(candidatos) != 1:
            motivos.append(
                "correspondencia %s %s: %d candidatas"
                % (hora, tipo, len(candidatos))
            )
            correspondencias.append(None)
        else:
            i = candidatos[0]
            if i in usados:
                motivos.append("batida associada a mais de uma orfa: " + hora)
            usados.add(i)
            correspondencias.append(i)

    return {
        "linha": numero, "col": col, "data": dia, "tipo_dia": tipo_dia,
        "dono": dono, "destino": destino, "perguntas": int(perguntas),
        "chamados": int(chamados), "orfas": orfas, "bat": bat,
        "correspondencias": correspondencias, "dna": dnas,
        "nao_mensuravel": any(m.group(2) == "NAO_CLASSIFICADA" for m in cas),
        "ambiguidades": motivos,
    }


def pares_observaveis(bat):
    # A adjacencia e medida na lista inteira, inclusive batidas com estrela.
    pares = []
    for i in range(len(bat) - 1):
        a, b = bat[i], bat[i + 1]
        if (a["tipo"] == "E" and b["tipo"] == "S"
                and not a["estrela"] and not b["estrela"]
                and b["seg"] > a["seg"]):
            pares.append((i, i + 1))
    return pares


def periodos_dna(h):
    if h is None:
        return []
    hi, hii, hfi, hf = h
    if hi == "None" or hf == "None":
        return []
    if hii == "None" and hfi == "None":
        return [(minuto(hi), minuto(hf))]
    if hii == "None" or hfi == "None":
        return []
    return [(minuto(hi), minuto(hii)), (minuto(hfi), minuto(hf))]


def distancia_relogio(seg, marco_min):
    # Distancia circular: a data da ponta de DNA nao vem no anexo.
    previsto = marco_min * 60
    d = abs((seg % 86400) - previsto)
    return min(d, 86400 - d)


def compativel(linha, par, tolerancia_min):
    entrada, saida = (linha["bat"][i] for i in par)
    for horarios in linha["dna"].values():
        for ini, fim in periodos_dna(horarios):
            if (distancia_relogio(entrada["seg"], ini) <= tolerancia_min * 60
                    and distancia_relogio(saida["seg"], fim) <= tolerancia_min * 60):
                return True
    return False


def classificar(linha, tolerancia_min, eco_seg):
    bat = linha["bat"]
    pares = pares_observaveis(bat)
    pares_dia = [p for p in pares if bat[p[0]]["dia"] == 0]
    indices_pareados = {i for par in pares for i in par}
    corr = [i for i in linha["correspondencias"] if i is not None]

    a = (linha["tipo_dia"] == "folga"
         and any(compativel(linha, par, tolerancia_min) for par in pares_dia))
    b = linha["tipo_dia"] == "folga" and bool(pares_dia) and not a
    c = (len(linha["orfas"]) == 1 and len(corr) == 1
         and not bat[corr[0]]["estrela"] and corr[0] not in indices_pareados)
    d = any(bat[i]["estrela"] for i in corr)
    e = any(
        j != i and outra["tipo"] == bat[i]["tipo"]
        and abs(outra["seg"] - bat[i]["seg"]) <= eco_seg
        for i in corr for j, outra in enumerate(bat)
    )
    ativos = [nome for nome, valor in zip(CLASSES, (a, b, c, d, e, False))
              if valor]

    motivos = list(linha["ambiguidades"])
    if len(ativos) > 1:
        motivos.insert(0, "sobreposicao " + "+".join(ativos))
    if motivos:
        return "f", "; ".join(dict.fromkeys(motivos)), a, e
    if len(ativos) == 1:
        return ativos[0], "", a, e

    soltas = [i for i in corr if not bat[i]["estrela"]
              and i not in indices_pareados]
    if len(linha["orfas"]) > 1 and soltas:
        return "f", "sequencia incompleta com varias orfas", a, e
    return "f", "nenhum criterio aplicavel", a, e


def chave_col(col):
    return int(col[3:])


def tabela(linhas, classificacoes, titulo):
    print("\n" + titulo)
    print("classe | dias | pessoas | orfas | ESTRUTURA | CADASTRO | BATIDA"
          " | destino sim | destino desconhecido | casamento nao mensuravel")
    for classe in CLASSES:
        grupo = [r for r, resultado in zip(linhas, classificacoes)
                 if resultado[0] == classe]
        donos = Counter(r["dono"] for r in grupo)
        destinos = Counter(r["destino"] for r in grupo)
        print("%s | %d | %d | %d | %d | %d | %d | %d | %d | %d" % (
            classe, len(grupo), len({r["col"] for r in grupo}),
            sum(len(r["orfas"]) for r in grupo),
            donos["ESTRUTURA"], donos["CADASTRO"], donos["BATIDA"],
            destinos["sim"], destinos["nao medido"],
            sum(r["nao_mensuravel"] for r in grupo),
        ))


def main():
    if len(sys.argv) != 2:
        raise SystemExit("uso: python3 censo.py caminho_do_anexo")
    with open(sys.argv[1], encoding="utf-8") as arquivo:
        cabecalho = arquivo.readline()
        if not cabecalho.startswith("# col data tipo_dia_grade "):
            raise ValueError("linha 1: cabecalho inesperado")
        linhas = []
        for numero, texto in enumerate(arquivo, 2):
            texto = texto.strip()
            if texto:
                linhas.append(parse_linha(texto, numero))

    chaves = [(r["col"], r["data"]) for r in linhas]
    if len(chaves) != len(set(chaves)):
        raise ValueError("colaborador e data repetidos no anexo")
    if not linhas:
        raise ValueError("anexo sem linhas de dados")

    resultados = [classificar(r, DNA_MIN, ECO_SEG) for r in linhas]
    print("UNIDADE: dia-colaborador; pessoas distintas por grupo")
    print("DONO e DESTINO: campos informados no anexo")
    print("CASAMENTO NAO MENSURAVEL: dias com ao menos um NAO_CLASSIFICADA")
    print("DNA: %s min por ponta, inclusive; escolha de uso da constante"
          % DNA_MIN)
    print("ECO: %s s, inclusive; batidas vivas do mesmo tipo" % ECO_SEG)
    tabela(linhas, resultados, "TOTAL POR CLASSE")
    for tipo in ("folga", "ausencia"):
        indices = [i for i, r in enumerate(linhas) if r["tipo_dia"] == tipo]
        tabela([linhas[i] for i in indices],
               [resultados[i] for i in indices],
               "TIPO_DIA_GRADE=" + tipo)

    print("\nLISTAS POR CLASSE: col data [tipo_dia_grade]")
    for classe in CLASSES:
        grupo = sorted(
            (r for r, resultado in zip(linhas, resultados)
             if resultado[0] == classe),
            key=lambda r: (chave_col(r["col"]), r["data"])
        )
        print("\n" + classe)
        for r in grupo:
            print("%s %s %s" % (r["col"], r["data"], r["tipo_dia"]))

    print("\nMOTIVOS DE CADA LINHA EM (f)")
    for r, resultado in sorted(
        zip(linhas, resultados),
        key=lambda x: (chave_col(x[0]["col"]), x[0]["data"])
    ):
        if resultado[0] == "f":
            print("%s %s linha=%d: %s"
                  % (r["col"], r["data"], r["linha"], resultado[1]))

    print("\nSENSIBILIDADE: elegiveis = predicado verdadeiro antes"
          " de ambiguidades/sobreposicoes; classe = classificacao final")
    for tol in (DNA_MIN / 2, DNA_MIN, DNA_MIN * 2):
        rs = [classificar(r, tol, ECO_SEG) for r in linhas]
        print("DNA %g min: (a) elegiveis=%d classe=%d"
              % (tol, sum(x[2] for x in rs), sum(x[0] == "a" for x in rs)))
    for janela in (ECO_SEG / 2, ECO_SEG, ECO_SEG * 2):
        rs = [classificar(r, DNA_MIN, janela) for r in linhas]
        print("ECO %g s: (e) elegiveis=%d classe=%d"
              % (janela, sum(x[3] for x in rs), sum(x[0] == "e" for x in rs)))

    total_dias = len(linhas)
    total_orfas = sum(len(r["orfas"]) for r in linhas)
    soma_dias = sum(sum(x[0] == c for x in resultados) for c in CLASSES)
    soma_orfas = sum(
        len(r["orfas"]) for r, resultado in zip(linhas, resultados)
        if resultado[0] in CLASSES
    )
    confere = soma_dias == total_dias and soma_orfas == total_orfas
    esperado = total_dias == 283 and total_orfas == 645
    print("\nSOMA_CONFERE dias=%d orfas=%d particao=%s"
          " referencia_283_645=%s"
          % (soma_dias, soma_orfas, "sim" if confere else "NAO",
             "sim" if esperado else "NAO"))
    print("ENTRADA dias=%d orfas=%d" % (total_dias, total_orfas))


if __name__ == "__main__":
    try:
        main()
    except (OSError, ValueError) as exc:
        raise SystemExit("ERRO: %s" % exc)
```

## 18/11 20:09 · sessao c5c08839c7fc · RAIA-ESPURIAS · censo · origem=Code

Não consegui fechar o censo linha a linha nem conferir as somas de **283 dias e 645 órfãs**. Esses totais são informados no pedido; minha contagem independente está **[não verificado]**. O texto está na mensagem, mas não encontrei `s2_v4_compacto.txt` como arquivo na cópia, e não consegui processar o texto integral de modo reproduzível neste ambiente. **[não verificado]**

Os critérios abaixo são uma proposta operacional para a leitura; não são classificação concluída. O formato informado para `bat`, `cas` e `dna` consta do cabeçalho reproduzido de `s2_v4_compacto.txt`; sua numeração e conteúdo no arquivo original estão **[não verificado]**.

- **Turno completo observável:** sequência cronológica `E→S` em `bat`, com ambas as pontas sem `*` e duração positiva; para turno iniciado no dia, admitir a saída em `[+1]`. Isso comprova apenas o padrão de batidas no recorte, não a efetiva prestação de trabalho. **[não verificado]**
- **(a) Fase invertida:** turno completo observável iniciado em `[+0]`, `tipo_dia_grade=folga` e `dna-1=T` ou `dna+1=T`, com horários do turno compatíveis com o DNA vizinho. A tolerância de compatibilidade ainda precisa ser fixada para que o critério seja reproduzível; `T` sozinho não comprova inversão de fase. **[não verificado]**
- **(b) Avulso:** turno completo observável em folga sem evidência suficiente para (a). O anexo, por si só, não demonstra troca ou extra sem registro; esse nome causal permanece **[não verificado]**.
- **(c) Batida solta:** uma órfã correspondente a batida sem `*` em `[+0]`, sem par `E→S` completo que a inclua no recorte. Uma sequência incompleta com várias batidas deve ir para (f), salvo se outro critério se aplicar de forma exclusiva. **[não verificado]**
- **(d) Plantada por declaração:** ao menos uma órfã que corresponda, por minuto e tipo, a batida `[+0]` marcada `*`; correspondência ambígua vai para (f). Uma `*` em dia vizinho não basta para classificar a órfã do dia como plantada. **[não verificado]**
- **(e) Eco/toque repetido:** órfã correspondente a batida `[+0]` que tenha outra batida viva do mesmo tipo a menos de **2 minutos**, incluindo a fronteira entre dias. Os 2 minutos são uma **escolha da medida**, sem constante da casa verificada. **[não verificado]**
- **(f) Outra, nomeada:** qualquer sobreposição entre critérios, ausência de critério, correspondência ambígua ou sequência incompleta. O motivo deve acompanhar cada linha. **[não verificado]**

Em qualquer classe, uma linha com `NAO_CLASSIFICADA` em `cas` deve entrar também no subtotal **casamento não mensurável**, sem ser excluída da classe. `destino=nao medido` deve ser contado como **destino desconhecido**, não como ausência de destino. **[não verificado]**

A tabela por classe, as listas completas e a linha `SOMA_CONFERE` permanecem **[não verificado]**; não seria correto preencher esses resultados sem a contagem. Para concluir com conferência reproduzível, é necessário disponibilizar o anexo como arquivo legível nesta sessão; a leitura pode continuar sem criar, editar ou apagar arquivos.

## 18/11 20:09 · sessao b9482d524e1f · CADEIA-DO-CONTRATO-20 · censo · origem=Code

**Censo: 34 necessários, 0 superados e 7 não decididos.** Para os 32 arquivos que o anexo `os9.txt` deixa fora da lista, aplico a premissa dada: a mudança da cópia ainda não está no HEAD. Para dois dos nove, o código da cópia identifica a alteração e o diff do main mostra mudança em outro ponto. Para os sete restantes, o trecho exato da mudança da cópia ou sua necessidade após a integração fica [não verificado].

**Convenções:** N = necessário; ND = não decidido; B = referência ao mesmo arquivo na tabela B2b, conforme `r.txt`; F5 = relação observável com DST/turno, sem afirmar cruzamento com os logs F5, que não foram anexados. A interseção por arquivo com a espinha é zero, conforme `os41.txt` e a medição informada. O diff foi fornecido no corpo da pergunta sem numeração própria; suas referências abaixo usam arquivo e cabeçalho `@@` do trecho.

1. [calendario.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:499) — **ND**: cópia lê fatos e recusas; main altera relógio e oferta em `main_nos_41.diff`, `@@ -114,26 +114,55 @@` e `@@ -458,6 +487,7 @@`; diff exato da cópia [não verificado]. B não; F5: arquivo trata turno [calendario.py:214].
2. [test_calendario_ausencia_pela_autoridade.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:52) — **N**: teste de recusa de tipo sem faixa; B não; F5 [não verificado].
3. [test_medida_turno_do_par.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/core/tests/test_medida_turno_do_par.py:247) — **ND**: conflito nos registros de linhas; a resposta final depende da medição na árvore integrada [não verificado]. B não; F5: mede turno.
4. [leitor_celula.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/escala/services/leitor_celula.py:433) — **N**: transporta campos da ausência lavrada; B não; F5 [não verificado].
5. [semana_do_posto.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/escala/services/semana_do_posto.py:178) — **N**: leitura de faixa e propagação de recusa; B não; F5 [não verificado].
6. [utils.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/escala/utils.py:1205) — **N**: LASTRO lê o rótulo pela precedência; o main altera proteção de marcos em `main_nos_41.diff`, `@@ -499,6 +499,15 @@`, sem a mesma alteração de rótulo. B não; F5: arquivo trata turno [utils.py:1292].
7. [situacao.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ferias/situacao.py:89) — **ND**: cópia trata `TipoSemFaixa`; main expõe saldo negativo em `main_nos_41.diff`, `@@ -293,8 +294,12 @@`; diff exato da cópia [não verificado]. B não; F5 [não verificado].
8. [export.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/folha/export.py:1175) — **ND**: toca diretamente exportação e a função `motivos_retencao_celula`; main altera essa função em `main_nos_41.diff`, `@@ -1192,16 +1192,49 @@`; trecho alterado pela cópia [não verificado]. B sim: `competencia_entregue`, X2/X3 de `r.txt`; F5: arquivo trata turno [export.py:1179].
9. [semanal.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/pautas/semanal.py:63) — **N**: rótulo do contador de tipos sem faixa; B não; F5 [não verificado].
10. [ausencias.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/catalogo/ausencias.py:1) — **N**: catálogo de ausências; alteração específica [não verificado]. B não; F5 [não verificado].
11. [processar_cartorio.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/management/commands/processar_cartorio.py:72) — **N**: trata `TipoSemFaixa`; B não; F5 [não verificado].
12. [precedencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/precedencia.py:96) — **N**: recusa tipo sem faixa; B não; F5 [não verificado].
13. [ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/ausencia.py:1306) — **ND**: conflito de conteúdo; cópia consulta `faixa_de`, main altera portas de férias e projeção em `main_nos_41.diff`, `@@ -63,6 +64,137 @@`; a região exata conflitada [não verificado]. B não; F5: arquivo trata parcial que atravessa meia-noite [ausencia.py:882].
14. [cartorio.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/cartorio.py:773) — **N**: LASTRO transporta campos de cobertura; main troca o predicado de resposta em `main_nos_41.diff`, `@@ -1638,7 +1638,8 @@`. B sim: entrada da ressuscitação, A8 de `r.txt`; F5 [não verificado].
15. [dia_decidido.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_decidido.py:340) — **ND**: cópia recebe fatos da precedência; main acrescenta mapa de apresentação em `main_nos_41.diff`, `@@ -70,6 +70,20 @@`; diff exato da cópia [não verificado]. B não; F5: arquivo trata turno [dia_decidido.py:281].
16. [dia_do_colab.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_do_colab.py:12) — **N**: obtém fatos e recusa por dia; B não; F5 [não verificado].
17. [dia_pago.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_pago.py:389) — **N**: toca diretamente dia pago; LASTRO preserva predicado legado. B não; F5: arquivo trata turno [dia_pago.py:847].
18. [espelho.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:649) — **N**: trata `TipoSemFaixa`; B não; F5 [não verificado].
19. [fechamento.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/fechamento.py:1081) — **N**: trata `TipoSemFaixa` no fechamento; B não; F5 [não verificado].
20. [vigia_ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/vigia_ausencia.py:157) — **N**: detecta tipos ativos sem faixa; B não; F5 [não verificado].
21. [status_dia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/status_dia.py:248) — **N**: recebe recusas por colaborador e data; B não; F5 [não verificado].
22. [lastro_c3_fontes.json](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/lastro_c3_fontes.json:1) — **N**: fixture de fontes; B não; F5 [não verificado].
23. [lastro_c6_recortes.json](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/lastro_c6_recortes.json:1) — **N**: fixture de recortes; B não; F5 [não verificado].
24. [lastro_c9_recortes.json](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/lastro_c9_recortes.json:1) — **N**: fixture de recortes; B não; F5 [não verificado].
25. [test_a2_precedencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_a2_precedencia.py:294) — **ND**: cópia testa faixa e precedência; main adapta fixtures de férias em `main_nos_41.diff`, `@@ -81,7 +81,15 @@`; diff exato da cópia [não verificado]. B não; F5 [não verificado].
26. [test_a6_vigia_familia_ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_a6_vigia_familia_ausencia.py:128) — **N**: testa contador de tipos sem faixa; B não; F5 [não verificado].
27. [test_cobparc_marco_no_periodo.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_cobparc_marco_no_periodo.py:60) — **N**: testa rótulo do dia coberto; B não; F5 [não verificado].
28. [test_e3_isento_na_impressao.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_e3_isento_na_impressao.py:104) — **N**: verifica regra sem aumento de versão; B não; F5 [não verificado].
29. [test_e3_status_lavrado.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_e3_status_lavrado.py:123) — **N**: testa classificação por faixa e tipo de dia; B não; F5 [não verificado].
30. [test_lastro_c10_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c10_sem_banco.py:20) — **N**: variantes de teste c10; B não; F5 [não verificado].
31. [test_lastro_c11_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c11_sem_banco.py:22) — **N**: testes c11; B não; F5 [não verificado].
32. [test_lastro_c12_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c12_sem_banco.py:22) — **N**: variantes e mutantes c12; B não; F5 [não verificado].
33. [test_lastro_c5_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c5_sem_banco.py:33) — **N**: suporte aos testes c5; B não; F5 [não verificado].
34. [test_lastro_c7_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c7_sem_banco.py:19) — **N**: suporte aos testes c7; B não; F5 [não verificado].
35. [test_lastro_faixa_sem_default.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_faixa_sem_default.py:11) — **N**: testes de faixa explícita; B não; F5 [não verificado].
36. [test_o219_versao_da_regra.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_o219_versao_da_regra.py:79) — **N**: registra preservação de versão e fotos anteriores; B não; F5 [não verificado].
37. [test_porta_ausencia_ferias.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_porta_ausencia_ferias.py:24) — **N**: testa a porta ausência/férias; B não; F5 [não verificado].
38. [test_smoke_clique_tipo_ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_smoke_clique_tipo_ausencia.py:90) — **N**: smoke da interação com tipo de ausência; B não; F5 [não verificado].
39. [views_tipos_ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/views_tipos_ausencia.py:28) — **N**: views do cadastro de tipo de ausência; B não; F5 [não verificado].
40. [pdf_espelho.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/relatorios/pdf_espelho.py:471) — **N**: LASTRO lê espelho antes de consumir órfãs para receber recusa por dia; B não; F5 [não verificado].
41. [test_rotulo_ausencia_pdf.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/relatorios/tests/test_rotulo_ausencia_pdf.py:64) — **N**: testa recusa e rótulo no PDF; B não; F5 [não verificado].

Os dois conflitos de conteúdo constam de `mergetree.txt`:

- **`test_medida_turno_do_par.py` — texto**, nos registros de linhas: a cópia documenta deslocamentos LASTRO preservando identidades [test_medida_turno_do_par.py:247]; o main atualiza contagens e referências para a sua árvore em `main_nos_41.diff`, `@@ -1,7 +1,7 @@` e `@@ -244,7 +244,7 @@`. Os números finais após integração exigem medição [não verificado].
- **`ausencia.py` — natureza [não verificado]**: a cópia valida faixa [ausencia.py:1306]; o main acrescenta guarda de férias derivadas e invalidação de projeções em `main_nos_41.diff`, `@@ -63,6 +64,137 @@`. `mergetree.txt` informa o arquivo, mas não fornece os marcadores nem os trechos exatos do conflito; não há base para classificá-lo como apenas texto ou como conflito de regra.

**Fecho:** necessários **34**; superados **0**; não decididos **7**. Toque direto em `folha/export.py` ou `ponto/services/dia_pago.py`: **2**, listados nos itens 8 e 17; alcance da alteração da cópia na função de retenção `motivos_retencao_celula` **[não verificado]**. Conflitos **2**: um de texto e um de natureza **[não verificado]**. Cruzam espinha por arquivo **0**, conforme medição informada; cruzam B2b por referência de arquivo **2**: `folha/export.py` e `ponto/services/cartorio.py`, conforme `r.txt`. Perguntas de lei: **[não verificado]**.

## 18/11 20:07 · sessao d8b2bf037792 · RAIA-ESPURIAS · leitura · origem=Code

A sonda abaixo é **texto não executado**; seus totais e seu funcionamento em sombra permanecem **[não verificado]**. O recorte preserva o de `s2_v3.py`, inclusive `tipo_dia in ("folga", "ausencia")`, e compara os totais observados com 283 dias, 117 pessoas e 645 órfãs informados no pedido. [não verificado: anexo `s2_v3.py` e dado raspado]

Para destino, a contagem de perguntas usa `PerguntaDisputa.colaborador` e `data_evento`; a de chamados usa `ChamadoColaborador.colaborador` e `celula__data`. São contagens de linhas, inclusive encerradas: a sonda não inventa um filtro de estado. [chamados/models.py:44](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:44), [chamados/models.py:56](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:56), [chamados/models.py:2599](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2599), [chamados/models.py:2610](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2610), [chamados/models.py:2620](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2620)

A lista fica **“nao medido”**: `lista()` captura falhas internas de componentes, inclusive C11 e C12, e também agrega assinaturas sem data de dia; sua saída, isoladamente, não permite afirmar que uma ausência de linha prova ausência daquele dia. Por isso, `sem_destino` fica desconhecido quando pergunta e chamado são zero; a sonda informa separadamente os dias com destino comprovado por pergunta ou chamado. [cadastro_realidade.py:848](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/cadastro_realidade.py:848), [cadastro_realidade.py:922](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/cadastro_realidade.py:922)

**Texto da sonda** — para uso posterior exclusivamente com `bin/sonda_leitura.sh --sombra <arquivo.py>`; não foi criado arquivo nesta sessão. [bin/sonda_leitura.sh:4](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:4)

```python
# ITEM 2: fatos por (colaborador, dia); nao classifica.
# Executar somente: bin/sonda_leitura.sh --sombra <arquivo.py>
#
# Universo identico ao s2_v3 fornecido: CelulaDia com ata.n_orfas > 0,
# empresas 2/3/4, e grade.tipo_dia em ("folga", "ausencia").
#
# Pergunta -> dia: PerguntaDisputa.data_evento e colaborador:
# app/chamados/models.py:2610, :2620.
# Chamado -> dia: ChamadoColaborador.celula.data e colaborador:
# app/chamados/models.py:44, :56.
# Lista: nao medida. lista() captura falhas de componentes e inclui
# assinaturas sem data: app/escala/services/cadastro_realidade.py:848-861,
# :922-928. Assim, zero pergunta/chamado nao prova ausencia de destino.
#
# Casamento: transcricao da funcao da volta 5 fornecida.
# Resultado classe=None indica casamento valido, nao classificacao da orfa.

import json
import re
from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta

from django.db import connection
from django.utils import timezone

from chamados.models import ChamadoColaborador, PerguntaDisputa
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.models import Batida
from ponto.services.cxr_na_lista import cxr_lavrado

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
ERROS = Counter()
parados = set()
universo_completo = True


def emitir(obj):
    print(json.dumps(obj, ensure_ascii=False, sort_keys=True, default=str))


def campos(modelo):
    emitir({
        "registro": "campos",
        "modelo": modelo._meta.label,
        "campos": [
            f.name for f in modelo._meta.get_fields()
            if getattr(f, "concrete", False)
        ],
    })


def permissao_negada(exc):
    vistos = set()
    atual = exc
    while atual is not None and id(atual) not in vistos:
        vistos.add(id(atual))
        codigo = getattr(atual, "sqlstate", None) or getattr(atual, "pgcode", None)
        if codigo in ("42501", "25006"):
            return True
        if isinstance(atual, PermissionError):
            return True
        atual = getattr(atual, "__cause__", None) or getattr(
            atual, "__context__", None
        )
    return False


def erro(etapa, exc, cid=None):
    global universo_completo
    ERROS[(etapa, type(exc).__name__)] += 1
    universo_completo = False
    obj = {
        "registro": "erro",
        "etapa": etapa,
        "tipo": type(exc).__name__,
        "col": "col{}".format(cid) if cid is not None else None,
    }
    if permissao_negada(exc):
        obj["permissao_negada"] = True
        obj["chamada"] = etapa
        obj["acao"] = "parar este colab" if cid is not None else "parar etapa global"
        if cid is not None:
            parados.add(cid)
    emitir(obj)


def hhmm(valor):
    if not isinstance(valor, str) or not re.fullmatch(r"\d{2}:\d{2}", valor):
        return None
    h, m = map(int, valor.split(":"))
    if h > 23 or m > 59:
        return None
    return time(h, m)


def local_minuto(ts):
    return timezone.localtime(ts).replace(second=0, microsecond=0)


def instante(dia, hora):
    return timezone.make_aware(
        datetime.combine(dia, hora), timezone.get_current_timezone()
    )


def ocorrencias_da_ata(ata):
    # Mesmo comportamento do anexo s1_v4.py; divergencia nao e ocultada.
    if not isinstance(ata, dict):
        raise ValueError("ata nao e objeto")
    lista = ata.get("orfas") or []
    if not isinstance(lista, list):
        raise ValueError("ata.orfas nao e lista")
    lista = [str(x) if x else "?" for x in lista]
    n = ata.get("n_orfas")
    n = len(lista) if n is None else int(n)
    if n < 0:
        raise ValueError("n_orfas negativo")
    return lista if lista else ["?"] * n


def virada(cel, dia_grade):
    pares = {}
    for marco in dia_grade.get("marcos") or []:
        if isinstance(marco, dict) and marco.get("tipo") in ("E", "S"):
            h = hhmm(marco.get("hora"))
            if h is not None:
                pares.setdefault(marco["tipo"], []).append(h)
    if pares.get("E") and pares.get("S"):
        return pares["S"][-1] <= pares["E"][0]
    dna = cel.dna if isinstance(cel.dna, dict) else {}
    marcos = dna.get("marcos") or {}
    if isinstance(marcos, dict):
        hi, hf = hhmm(marcos.get("hi")), hhmm(marcos.get("hf"))
        if hi is not None and hf is not None:
            return hf <= hi
    return None


def casar_orfa(cel, valor, d, por_minuto):
    """Extracao do bloco de casamento de s1_v4.py, fornecida na volta 5."""
    hora = hhmm(valor)
    if valor == "?":
        return "sem_hora", None, None, None
    if hora is None:
        return "NAO_CLASSIFICADA", "hora da ata invalida", None, None

    alvo = instante(cel.data, hora)
    hoje = por_minuto.get(alvo, [])
    amanha = por_minuto.get(alvo + timedelta(days=1), [])
    atravessa = virada(cel, d)

    if len(hoje) > 1:
        return (
            "nao_mensuravel_b",
            "multiplas batidas no minuto da celula",
            None,
            None,
        )
    if hoje and amanha:
        if atravessa is True:
            return (
                "nao_mensuravel_a",
                "virada e batidas nos dois dias",
                None,
                None,
            )
        if atravessa is None:
            return (
                "NAO_CLASSIFICADA",
                "virada desconhecida com batidas nos dois dias",
                None,
                None,
            )
    if not hoje:
        return (
            "NAO_CLASSIFICADA",
            "sem batida viva no minuto da data da celula",
            None,
            None,
        )

    b = hoje[0]
    instante_b = local_minuto(b.timestamp)
    if b.tipo not in ("E", "S"):
        return (
            "NAO_CLASSIFICADA",
            "tipo da batida desconhecido",
            b,
            instante_b,
        )
    return None, None, b, instante_b


def retrato_batida(b):
    return {
        "pk": b.pk,
        "timestamp_local": timezone.localtime(b.timestamp).isoformat(),
        "tipo": b.tipo,
        "origem": b.origem,
        "canal": b.canal,
        "pergunta_origem_id": b.pergunta_origem_id,
    }


def vizinho(cel):
    if cel is None:
        return {"celula_existe": False, "trabalha": None, "marcos_dna": None}
    dna = cel.dna if isinstance(cel.dna, dict) else {}
    return {
        "celula_existe": True,
        "trabalha": cel.trabalha,
        "marcos_dna": dna.get("marcos"),
    }


def executar():
    global universo_completo

    # Recusa outros bancos antes de consultar dados.
    if connection.settings_dict.get("NAME") != "sombra":
        raise RuntimeError("banco nao e sombra")

    for modelo in (
        Colaborador, CelulaDia, Batida, PerguntaDisputa, ChamadoColaborador
    ):
        campos(modelo)

    agora = timezone.now()
    hoje = timezone.localtime(agora).date()
    emitir({
        "registro": "inicio",
        "inicio": INI.isoformat(),
        "fim": FIM.isoformat(),
        "empresas": list(EMPRESAS),
        "agora_local": timezone.localtime(agora).isoformat(),
        "referencia": {"dias": 283, "pessoas": 117, "orfas": 645},
        "lista": "nao medido",
        "motivo_lista": (
            "lista captura falhas internas e agrega assinaturas sem data; "
            "ausencia do dia nao demonstrada"
        ),
    })

    etapa = "carregar_celulas_universo"
    try:
        cels = list(
            CelulaDia.objects.filter(
                colaborador__empresa_id__in=EMPRESAS,
                data__range=(INI, FIM),
                ata__n_orfas__gt=0,
            )
            .select_related("colaborador")
            .order_by("colaborador_id", "data", "pk")
        )
    except Exception as exc:
        erro(etapa, exc)
        emitir({"registro": "universo", "estado": "nao medido"})
        return

    por_colab = defaultdict(list)
    for cel in cels:
        por_colab[cel.colaborador_id].append(cel)

    universo = {}
    for cid, itens in sorted(por_colab.items()):
        etapa = "grade_da_celula(colab, INI, FIM)"
        try:
            grade = grade_da_celula(itens[0].colaborador, INI, FIM)
            dias_grade = {d["data"]: d for d in grade["dias"]}
            selecionados = []
            for cel in itens:
                d = dias_grade[cel.data]
                if d["tipo_dia"] in ("folga", "ausencia"):
                    selecionados.append((cel, d))
            if selecionados:
                # A unidade pedida e (colab, dia): duplicata nao pode ser
                # silenciosamente fundida ou contada como outro dia.
                datas = [cel.data for cel, _ in selecionados]
                if len(datas) != len(set(datas)):
                    raise ValueError("mais de uma celula por colab e dia")
                universo[cid] = selecionados
        except Exception as exc:
            erro(etapa, exc, cid)

    # O total do s2_v3 usa n_orfas; a lista da ata e contada separadamente.
    n_dias = sum(len(itens) for itens in universo.values())
    n_orfas_ata = 0
    n_orfas_lista = 0
    contagens_invalidas = 0
    for cid, itens in sorted(universo.items()):
        for cel, _ in itens:
            try:
                ata = cel.ata or {}
                n = ata.get("n_orfas")
                if not isinstance(n, int) or isinstance(n, bool) or n <= 0:
                    contagens_invalidas += 1
                else:
                    n_orfas_ata += n
                n_orfas_lista += len(ocorrencias_da_ata(ata))
            except Exception as exc:
                erro("contar_orfas_ata", exc, cid)
                contagens_invalidas += 1
                if cid in parados:
                    break

    emitir({
        "registro": "universo",
        "dias": n_dias,
        "pessoas": len(universo),
        "orfas_n_ata": n_orfas_ata,
        "orfas_lista_ata": n_orfas_lista,
        "contagens_invalidas": contagens_invalidas,
        "completo": universo_completo,
        "confere_referencia": (
            universo_completo and contagens_invalidas == 0
            and (n_dias, len(universo), n_orfas_ata) == (283, 117, 645)
        ),
    })

    etapa = "cxr_lavrado()"
    try:
        ler_cxr = cxr_lavrado()
    except Exception as exc:
        erro(etapa, exc)
        ler_cxr = None

    # Mede cobertura antes de chamar dono; nunca substitui None por conjunto vazio.
    cxr_por_colab = {}
    for cid in sorted(universo):
        if cid in parados:
            continue
        if ler_cxr is None:
            cxr_por_colab[cid] = "erro"
            continue
        try:
            cxr_por_colab[cid] = ler_cxr(cid, INI)
        except Exception as exc:
            erro("ler_cxr(colab, INI)", exc, cid)
            cxr_por_colab[cid] = "erro"

    emitir({
        "registro": "cobertura_lavra_pessoas",
        "cobertas": sum(isinstance(x, set) for x in cxr_por_colab.values()),
        "cxr_none": sum(x is None for x in cxr_por_colab.values()),
        "desconhecidas": (
            len(universo) - sum(
                isinstance(x, set) or x is None for x in cxr_por_colab.values()
            )
        ),
        "decisao": "sem_motor=True; nao chamar o motor",
    })

    dono_valores = Counter()
    cxr_none = 0
    cxr_desconhecido = 0
    dono_none = 0
    dono_desconhecido = 0
    com_destino = 0
    sem_destino = 0
    destino_desconhecido = 0
    linhas = 0
    casamentos = Counter()

    for cid, itens in sorted(universo.items()):
        if cid in parados:
            continue
        col = itens[0][0].colaborador
        etapa = "carregar_celulas_vizinhas"
        try:
            vizinhas = {
                c.data: c for c in CelulaDia.objects.filter(
                    colaborador_id=cid,
                    data__range=(INI - timedelta(days=1), FIM + timedelta(days=1)),
                )
            }

            etapa = "consultar_Batida_viva"
            lo = instante(INI - timedelta(days=1), time.min)
            hi = instante(FIM + timedelta(days=2), time.min)
            batidas = list(
                Batida.objects.filter(
                    colaborador_id=cid,
                    retratada_em__isnull=True,
                    timestamp__gte=lo,
                    timestamp__lt=hi,
                ).order_by("timestamp", "pk")
            )
            por_minuto = defaultdict(list)
            por_data = defaultdict(list)
            for b in batidas:
                local = timezone.localtime(b.timestamp)
                por_minuto[local_minuto(b.timestamp)].append(b)
                por_data[local.date()].append(b)

            etapa = "contar_PerguntaDisputa_por_data_evento"
            perguntas = Counter(
                PerguntaDisputa.objects.filter(
                    colaborador_id=cid, data_evento__range=(INI, FIM)
                ).values_list("data_evento", flat=True)
            )

            etapa = "contar_ChamadoColaborador_por_celula__data"
            chamados = Counter(
                ChamadoColaborador.objects.filter(
                    colaborador_id=cid, celula__data__range=(INI, FIM)
                ).values_list("celula__data", flat=True)
            )

            cxr = cxr_por_colab.get(cid, "erro")
            ins = None
            if cxr != "erro":
                etapa = "InsumosDoDono(... sem_motor=True, cxr=cxr)"
                ins = InsumosDoDono(
                    col, INI, FIM, hoje, agora=agora,
                    sem_motor=True, cxr=cxr
                )

            for cel, d in itens:
                dia = cel.data
                etapa = "ocorrencias_da_ata"
                orfas = ocorrencias_da_ata(cel.ata or {})
                resultados = []
                for valor in orfas:
                    etapa = "casar_orfa"
                    classe, motivo, b, ts = casar_orfa(
                        cel, valor, d, por_minuto
                    )
                    casamentos[classe or "casada_tipo_valido"] += 1
                    resultados.append({
                        "orfa": valor,
                        "resultado": classe,
                        "motivo": motivo,
                        "batida_pk": b.pk if b is not None else None,
                        "instante_local": ts.isoformat() if ts else None,
                    })

                dono = "nao medido"
                if ins is not None:
                    etapa = "InsumosDoDono.dono(dia)"
                    dono = ins.dono(dia)
                    if dono is None:
                        dono_none += 1
                    else:
                        dono_valores[str(dono)] += 1
                else:
                    dono_desconhecido += 1

                if cxr == "erro":
                    cxr_estado = "nao medido"
                    cxr_desconhecido += 1
                else:
                    cxr_estado = cxr is None
                    if cxr_estado:
                        cxr_none += 1

                np = perguntas[dia]
                nc = chamados[dia]
                if np or nc:
                    destino = "sim"
                    com_destino += 1
                else:
                    # Lista nao medida: nao e possivel provar "sem destino".
                    destino = "nao medido"
                    destino_desconhecido += 1

                conjunto_batidas = []
                for deslocamento in (-1, 0, 1):
                    data_local = dia + timedelta(days=deslocamento)
                    conjunto_batidas.extend(
                        retrato_batida(b) for b in por_data[data_local]
                    )

                etapa = "emitir_linha"
                emitir({
                    "registro": "dia",
                    "col": "col{}".format(cid),
                    "data": dia.isoformat(),
                    "tipo_dia_grade": d["tipo_dia"],
                    "n_orfas_ata": (cel.ata or {}).get("n_orfas"),
                    "orfas_ata": orfas,
                    "casamentos": resultados,
                    "batidas_vivas_d_menos_1_a_d_mais_1": conjunto_batidas,
                    "d_menos_1": vizinho(vizinhas.get(dia - timedelta(days=1))),
                    "d_mais_1": vizinho(vizinhas.get(dia + timedelta(days=1))),
                    "cxr_none": cxr_estado,
                    "dono": dono,
                    "dono_none": (
                        "nao medido" if ins is None else dono is None
                    ),
                    "perguntas": np,
                    "chamados": nc,
                    "lista": "nao medido",
                    "destino": destino,
                })
                linhas += 1
        except Exception as exc:
            erro(etapa, exc, cid)
            # Toda falha interrompe este colab; em particular, permissao
            # negada imprime a chamada acima e nao tenta outra porta.

    dias_sem_linha = n_dias - linhas
    emitir({
        "registro": "contadores",
        "dias_universo": n_dias,
        "pessoas_universo": len(universo),
        "orfas_n_ata": n_orfas_ata,
        "orfas_lista_ata": n_orfas_lista,
        "linhas_emitidas": linhas,
        "dias_sem_linha": dias_sem_linha,
        "dono_por_valor_dias_emitidos": dict(dono_valores),
        "cxr_none_dias_emitidos": cxr_none,
        "cxr_desconhecido_dias_emitidos": cxr_desconhecido,
        "dono_none_dias_emitidos": dono_none,
        "dono_desconhecido_dias_emitidos": dono_desconhecido,
        "com_destino_comprovado_pergunta_ou_chamado": com_destino,
        "sem_destino": "nao medido",
        "destino_desconhecido_dias_emitidos": destino_desconhecido,
        "casamentos": dict(casamentos),
        "universo_completo": universo_completo,
    })


try:
    executar()
except Exce

[truncado em 20000 de 20504 caracteres]

## 18/11 20:06 · sessao 764881c8eb4e · CADEIA-DO-CONTRATO-20 · censo · origem=Code

**O censo não permite decidir quais dos 41 arquivos ainda são necessários.** A árvore fornecida não tem metadados Git, e o anexo do main lista caminhos, sem o conteúdo das mudanças. Assim, os sítios abaixo são observáveis na cópia, mas, salvo onde o próprio código os identifica, **o trecho exato alterado contra a base e sua necessidade contra o HEAD ficam [não verificado]**.

Convenções para cada linha: **ND** = necessidade contra o HEAD não decidida; **E** = cruzamento por mesmo arquivo com as listas E4C-F9/F6F7; **B** = referência ao arquivo na `tabela_unica`; **F5** = [não verificado], pois não foi fornecida lista de arquivos da obra F5. Nas 41 linhas, E = não, pela interseção dos nomes fornecidos; isso não prova ausência de dependência entre arquivos. Para B, distingo abaixo as referências encontradas.

1. [calendario.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:499) — lê fatos e recusas por período; trecho exato do diff [não verificado]; ND; main lista `app/colaboradores/services/calendario.py`; B não.
2. [test_calendario_ausencia_pela_autoridade.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:52) — teste de recusa de tipo sem faixa; diff exato [não verificado]; ND; B não.
3. [test_medida_turno_do_par.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/core/tests/test_medida_turno_do_par.py:247) — registra ajuste de referências de linha LASTRO; ND; main lista `app/core/tests/test_medida_turno_do_par.py`; B não.
4. [leitor_celula.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/escala/services/leitor_celula.py:433) — transporta campos de ausência lavrados, identificado como LASTRO c5; ND; B não.
5. [semana_do_posto.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/escala/services/semana_do_posto.py:178) — leitura de faixa e propagação de `TipoSemFaixa`; diff exato [não verificado]; ND; B não.
6. [utils.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/escala/utils.py:1205) — LASTRO c8 lê o rótulo da faixa pela precedência; ND; main lista `app/escala/utils.py`; B não.
7. [situacao.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ferias/situacao.py:89) — converte `TipoSemFaixa` em aviso; diff exato [não verificado]; ND; main lista `app/ferias/situacao.py`; B não.
8. [export.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/folha/export.py:1175) — arquivo do export e da retenção; trecho alterado exato [não verificado]; ND; main lista `app/folha/export.py`; B referencia `folha.export.competencia_entregue` nas linhas X2/X3 fornecidas.
9. [semanal.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/pautas/semanal.py:63) — inclui rótulo do contador `tipos_ativos_sem_faixa`; ND; B não.
10. [ausencias.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/catalogo/ausencias.py:1) — catálogo de ausências; função e alteração exata [não verificado]; ND; B não.
11. [processar_cartorio.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/management/commands/processar_cartorio.py:72) — trata `TipoSemFaixa` no processamento; diff exato [não verificado]; ND; B não.
12. [precedencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/precedencia.py:96) — define recusa `TipoSemFaixa` e tratamento por período; diff exato [não verificado]; ND; B não.
13. [ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/ausencia.py:1306) — consulta `faixa_de` e trata tipo sem faixa; diff exato [não verificado]; ND; main lista `app/ponto/services/ausencia.py`; B não.
14. [cartorio.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/cartorio.py:773) — transporta campos de cobertura da grade, identificado como LASTRO c5; ND; main lista `app/ponto/services/cartorio.py`; B referencia este arquivo como entrada da ressuscitação em A8.
15. [dia_decidido.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_decidido.py:340) — recebe fatos da precedência; trecho alterado exato [não verificado]; ND; main lista `app/ponto/services/dia_decidido.py`; B não.
16. [dia_do_colab.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_do_colab.py:12) — obtém fatos e recusa por dia; diff exato [não verificado]; ND; B não.
17. [dia_pago.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_pago.py:389) — mantém predicado legado identificado como LASTRO c6; ND; B não.
18. [espelho.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:649) — trata `TipoSemFaixa`; diff exato [não verificado]; ND; B não.
19. [fechamento.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/fechamento.py:1081) — trata `TipoSemFaixa` no fechamento; diff exato [não verificado]; ND; B não.
20. [vigia_ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/vigia_ausencia.py:157) — detecta tipos ativos sem faixa; diff exato [não verificado]; ND; B não.
21. [status_dia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/status_dia.py:248) — recebe recusas por colaborador e data; diff exato [não verificado]; ND; B não.
22. [lastro_c3_fontes.json](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/lastro_c3_fontes.json:1) — fixture de fontes c3; conteúdo comparativo contra base [não verificado]; ND; B não.
23. [lastro_c6_recortes.json](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/lastro_c6_recortes.json:1) — fixture de recortes c6; conteúdo comparativo [não verificado]; ND; B não.
24. [lastro_c9_recortes.json](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/lastro_c9_recortes.json:1) — fixture de recortes c9; conteúdo comparativo [não verificado]; ND; B não.
25. [test_a2_precedencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_a2_precedencia.py:294) — testes de faixa e precedência; diff exato [não verificado]; ND; main lista `app/ponto/tests/test_a2_precedencia.py`; B não.
26. [test_a6_vigia_familia_ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_a6_vigia_familia_ausencia.py:128) — contempla contador de tipos ativos sem faixa; diff exato [não verificado]; ND; B não.
27. [test_cobparc_marco_no_periodo.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_cobparc_marco_no_periodo.py:60) — alimenta a precedência para rótulo do dia coberto, com referência a LASTRO c8; ND; B não.
28. [test_e3_isento_na_impressao.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_e3_isento_na_impressao.py:104) — verifica regra sem aumento de versão, identificado como LASTRO c7; ND; B não.
29. [test_e3_status_lavrado.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_e3_status_lavrado.py:123) — testa classificação por faixa e tipo de dia; diff exato [não verificado]; ND; B não.
30. [test_lastro_c10_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c10_sem_banco.py:20) — instalação de variantes de teste c10; diff exato [não verificado]; ND; B não.
31. [test_lastro_c11_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c11_sem_banco.py:22) — testes c11 com instalação c10; diff exato [não verificado]; ND; B não.
32. [test_lastro_c12_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c12_sem_banco.py:22) — variantes e mutantes c12; diff exato [não verificado]; ND; B não.
33. [test_lastro_c5_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c5_sem_banco.py:33) — suporte a testes c5 por fonte/revisão; diff exato [não verificado]; ND; B não.
34. [test_lastro_c7_sem_banco.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_c7_sem_banco.py:19) — suporte aos testes c7; diff exato [não verificado]; ND; B não.
35. [test_lastro_faixa_sem_default.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_lastro_faixa_sem_default.py:11) — testes de faixa explícita; diff exato [não verificado]; ND; B não.
36. [test_o219_versao_da_regra.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_o219_versao_da_regra.py:79) — registra que c7 não aumenta versão e preserva fotos anteriores; ND; B não.
37. [test_porta_ausencia_ferias.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_porta_ausencia_ferias.py:24) — testes da porta ausência/férias; trecho alterado [não verificado]; ND; B não.
38. [test_smoke_clique_tipo_ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/tests/test_smoke_clique_tipo_ausencia.py:90) — smoke da interação com tipo de ausência; trecho alterado [não verificado]; ND; B não.
39. [views_tipos_ausencia.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/views_tipos_ausencia.py:28) — views do cadastro de tipo de ausência; trecho alterado [não verificado]; ND; B não.
40. [pdf_espelho.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/relatorios/pdf_espelho.py:471) — LASTRO c10 lê o espelho antes de consumir órfãs, para receber recusa por dia; ND; B não.
41. [test_rotulo_ausencia_pdf.py](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/relatorios/tests/test_rotulo_ausencia_pdf.py:64) — testes de recusa de tipo sem faixa e de rótulo no PDF; diff exato [não verificado]; ND; B não.

Quanto às **colunas de estado**, há acesso no arquivo inteiro, sem prova de que esses acessos integrem o diff LASTRO: `ChamadoColaborador.status_local` em [calendario.py:437](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:437), [ausencia.py:710](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/ausencia.py:710) e [cartorio.py:484](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/cartorio.py:484). O calendário também consulta `PerguntaDisputa.resposta_colab` em [calendario.py:473](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:473); o cartório lê `resposta_colab` e `via_resolucao` em [cartorio.py:1647](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/cartorio.py:1647). O leitor [dia_do_colab.py:65](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_do_colab.py:65) consulta perguntas e delega a avaliação de vivacidade; alteração de coluna de estado nesse sítio **[não verificado]**. Toque em coluna de estado de `DisputaSupervisao`: **[não verificado]**.

**Fecho:** necessários comprovados = **0**; superados comprovados = **0**; não decididos = **41**. Tocam diretamente os arquivos `folha/export.py` ou `ponto/services/dia_pago.py` = **2**; alcance das alterações sobre funções de retenção = **[não verificado]**. Cruzam por arquivo com E4C-F9/F6F7 = **0**, lista vazia; F5 = **[não verificado]**. Referências B2b por arquivo = **2**: `folha/export.py` e `ponto/services/cartorio.py`, conforme a tabela fornecida; isso não demonstra conflito entre os trechos alterados. Perguntas de lei = **[não verificado]**: não há base para declarar zero com o diff e a necessidade dos 41 ainda não decididos.

## 18/11 20:06 · sessao 318ec104c5da · RAIA-ESPURIAS · leitura · origem=Code

A sonda abaixo usa os sete dias informados e identifica cada par pelas horas da transcrição; se houver duplicidade, ausência ou outra batida entre as duas pontas, registra erro em vez de escolher. As horas transcritas identificam candidatos por minuto; os instantes e as distâncias impressos virão das batidas, sem arredondamento. [não verificado]

A comparação apresentada mede cada ponta contra seu respectivo marco do DNA, com limite inclusivo de 90 minutos. É uma medição explícita do envelope pedido, não uma reprodução integral da decisão de `parear_turnos`, que também considera a distância aos marcos de borda. [app/docs/LEIS.md:80](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/LEIS.md:80), [app/ponto/turnos.py:1246](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1246)

Para o classificador, a ata guarda as órfãs e lâmpadas, mas estas não são necessariamente as células originais da grade. Por isso a chamada com dados da ata é identificada como **projeção**, e a sonda registra a falta da grade original como erro de etapa. Também chama a função real sobre o par isolado como teste separado, sem confundir esse teste com classificação observada. [app/ponto/services/cartorio.py:599](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/cartorio.py:599), [app/ponto/services/cartorio.py:730](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/cartorio.py:730), [app/escala/orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53)

```python
# ITEM 5 -- texto de sonda; executar somente por:
# bin/sonda_leitura.sh --sombra <arquivo.py>
#
# Sete pares e horas de identificacao: ra.md transcrito na pergunta;
# col444 e col964, ambas as pontas: app/docs/RELATO.md:8.
# As horas sao apenas chaves de selecao, nao substituem timestamps.
#
# L-032: app/docs/LEIS.md:80.
# classificar_orfas e _runs_de_rajada: app/escala/orfa_classe.py:40,53.
# Outros leitores da constante JANELA_CLUSTER_MIN:
#   app/escala/utils.py:502-508, _protegida_da_guarda;
#   app/ponto/turnos.py:1581,1712,1729, leitor de papeis.
# cluster_espurio usa 15 literal, nao importa a constante:
#   app/ponto/services/cluster_espurio.py:15-36.
# Universo vivo: app/ponto/turnos.py:2868-2894.

import datetime
from decimal import Decimal

ERROS = 0

# (colaborador PK, data, hora local da S, hora local da E)
CASOS = (
    (252, "2026-11-09", "12:38", "12:52"),
    (252, "2026-11-10", "12:39", "12:52"),
    (252, "2026-11-11", "12:39", "12:52"),
    (252, "2026-11-13", "12:35", "12:49"),
    (252, "2026-11-17", "12:50", "13:05"),
    (444, "2026-11-16", "13:55", "14:01"),
    (964, "2026-10-28", "11:35", "11:46"),
)

RAIO_US = 90 * 60 * 1_000_000
MINUTO_US = 60 * 1_000_000


def erro(etapa, tipo):
    global ERROS
    ERROS += 1
    # Nao imprimir mensagem ou repr: podem conter dados pessoais.
    print("ERRO etapa=%s tipo=%s" % (etapa, tipo))


def us(td):
    return ((td.days * 86400 + td.seconds) * 1_000_000
            + td.microseconds)


def minutos_exatos(micros):
    # Valor racional exato, inclusive quando nao termina em decimal.
    return "%s/%s" % (micros, MINUTO_US)


def principal():
    from django.utils import timezone
    from colaboradores.models import Colaborador, Empresa
    from escala.models import CelulaDia
    from escala import orfa_classe
    from ponto.models import Batida
    from ponto.services.cluster_espurio import _leitura_da_celula
    from ponto.turnos import batidas_apuraveis

    def local(dt):
        return timezone.localtime(dt)

    def limite(dia):
        return timezone.make_aware(
            datetime.datetime.combine(dia, datetime.time.min),
            timezone.get_current_timezone(),
        )

    def marco_no_dia(dia, valor):
        if not isinstance(valor, str):
            raise ValueError("marco ausente")
        hora = datetime.time.fromisoformat(valor)
        if hora.tzinfo is not None:
            raise ValueError("marco com fuso")
        return timezone.make_aware(
            datetime.datetime.combine(dia, hora),
            timezone.get_current_timezone(),
        )

    # Campos antes de qualquer consulta aos modelos.
    for modelo in (Colaborador, Empresa, CelulaDia, Batida):
        print("CAMPOS %s: %s" % (
            modelo._meta.label,
            ", ".join(f.name for f in modelo._meta.concrete_fields),
        ))

    print("L032 raio_min=90 fonte=app/docs/LEIS.md:80")
    print("JANELA_CLUSTER_MIN=%s fonte=app/escala/orfa_classe.py:28"
          % orfa_classe.JANELA_CLUSTER_MIN)
    if orfa_classe.JANELA_CLUSTER_MIN != 15:
        erro("constante_cluster", "ValorDiferenteDe15")

    for cid, data_texto, hora_s, hora_e in CASOS:
        dia = datetime.date.fromisoformat(data_texto)
        etapa = "col%s_%s" % (cid, data_texto)
        print("\nCASO col%s dia=%s" % (cid, data_texto))

        try:
            colab = Colaborador.objects.get(pk=cid)
        except Exception as exc:
            erro(etapa + "_colaborador", type(exc).__name__)
            continue

        try:
            cel = CelulaDia.objects.get(colaborador_id=cid, data=dia)
            dna = cel.dna
            if not isinstance(dna, dict):
                raise ValueError("dna")
            marcos = dna.get("marcos")
            if not isinstance(marcos, dict):
                raise ValueError("marcos")
            hii = marco_no_dia(dia, marcos.get("hii"))
            hfi = marco_no_dia(dia, marcos.get("hfi"))
            # Nao inventar a data do marco em pausa que cruza a meia-noite.
            if hfi <= hii:
                raise ValueError("data dos marcos ambigua")
            print("CADASTRO col%s dia=%s hii=%s hfi=%s"
                  % (cid, dia, local(hii).isoformat(),
                     local(hfi).isoformat()))
        except Exception as exc:
            erro(etapa + "_cadastro", type(exc).__name__)
            cel = None
            hii = hfi = None

        try:
            ini, fim = limite(dia), limite(dia + datetime.timedelta(days=1))
            bats = list(
                batidas_apuraveis(colab, ini=ini)
                .filter(timestamp__lt=fim)
                .order_by("timestamp", "pk")
            )
            for b in bats:
                print("BATIDA_VIVA col%s dia=%s instante=%s tipo=%s"
                      % (cid, dia, local(b.timestamp).isoformat(), b.tipo))

            def coincide(b, hhmm, tipo):
                return (local(b.timestamp).strftime("%H:%M") == hhmm
                        and (b.tipo or "").upper() == tipo)

            ss = [b for b in bats if coincide(b, hora_s, "S")]
            es = [b for b in bats if coincide(b, hora_e, "E")]
            if len(ss) != 1 or len(es) != 1:
                raise ValueError("pontas ausentes ou ambiguas")
            s, e = ss[0], es[0]
            if s.timestamp >= e.timestamp:
                raise ValueError("ordem")
            if any(s.timestamp < b.timestamp < e.timestamp for b in bats):
                raise ValueError("batida intermediaria")
            if sum(b.timestamp == s.timestamp for b in bats) != 1:
                raise ValueError("empate na saida")
            if sum(b.timestamp == e.timestamp for b in bats) != 1:
                raise ValueError("empate na entrada")

            print("PAR_CANDIDATO col%s dia=%s S=%s E=%s duracao_min=%s"
                  % (cid, dia, local(s.timestamp).isoformat(),
                     local(e.timestamp).isoformat(),
                     minutos_exatos(us(e.timestamp - s.timestamp))))
            print("PAR_VINCULO_AO_INTERVALO=[nao verificado] "
                  "criterio=horas_transcritas_e_adjacencia")

            if hii is None or hfi is None:
                erro(etapa + "_envelope", "CadastroIndisponivel")
            else:
                ds = abs(us(s.timestamp - hii))
                de = abs(us(e.timestamp - hfi))
                dentro = ds <= RAIO_US and de <= RAIO_US
                print(
                    "ENVELOPE col%s dia=%s dentro=%s "
                    "dist_S_hii_min=%s dist_E_hfi_min=%s "
                    "excesso_S_min=%s excesso_E_min=%s"
                    % (
                        cid, dia, dentro,
                        minutos_exatos(ds), minutos_exatos(de),
                        minutos_exatos(max(0, ds - RAIO_US)),
                        minutos_exatos(max(0, de - RAIO_US)),
                    )
                )

            # Pergunta hipotetica ao codigo real: as DUAS horas do par
            # como orfas em dia de trabalho com marco vazio. A funcao
            # opera em HH:MM; isto nao prova que a ata classifique o par.
            horas_par = [hora_s, hora_e]
            ordenadas = sorted(horas_par, key=orfa_classe._minutos)
            runs = orfa_classe._runs_de_rajada(ordenadas)
            hipotese = {
                "tipo_dia": "trabalho",
                "orfas": horas_par,
                "celulas": [{"missing": True}],
            }
            print("PAR_ISOLADO runs_reais=%s classe_hipotetica=%s"
                  % (runs, orfa_classe.classificar_orfas(hipotese)))
        except Exception as exc:
            erro(etapa + "_batidas_par", type(exc).__name__)

        if cel is None:
            erro(etapa + "_leitores", "CelulaIndisponivel")
            continue

        try:
            leitura = _leitura_da_celula(cel)
            print("LEITOR_ATA_CLUSTER col%s dia=%s resultado=%s"
                  % (cid, dia, leitura))
        except Exception as exc:
            erro(etapa + "_leitor_ata", type(exc).__name__)

        try:
            ata = cel.ata
            if not isinstance(ata, dict):
                raise ValueError("ata")
            orfas = ata.get("orfas")
            lampadas = ata.get("lampadas")
            tipo_dia = ata.get("tipo_dia")
            if (not isinstance(orfas, list)
                    or not isinstance(lampadas, list)
                    or not isinstance(tipo_dia, str)):
                raise ValueError("insumos")
            if not all(isinstance(h, str) for h in orfas):
                raise ValueError("orfas")
            if not all(isinstance(lamp, dict) for lamp in lampadas):
                raise ValueError("lampadas")

            # Projecao, nao reconstrucao comprovada da grade: a ata pode
            # conter lampadas que nao correspondem as celulas da regua.
            projecao = {
                "tipo_dia": tipo_dia,
                "orfas": orfas,
                "celulas": [
                    {"missing": lamp.get("acesa") is False}
                    for lamp in lampadas
                ],
            }
            print("ATA col%s dia=%s tipo_dia=%s orfas=%s "
                  "lampadas_apagadas=%s"
                  % (cid, dia, tipo_dia, orfas,
                     sum(lamp.get("acesa") is False for lamp in lampadas)))
            print("CLASSIFICAR_ORFAS_PROJECAO_ATA=%s"
                  % orfa_classe.classificar_orfas(projecao))
            erro(etapa + "_classificacao_grade",
                 "GradeOriginalNaoVerificada")
        except Exception as exc:
            erro(etapa + "_classificacao_ata", type(exc).__name__)


try:
    principal()
except Exception as exc:
    erro("principal", type(exc).__name__)
finally:
    print("ERROS=%s" % ERROS)
    print("FIM_ITEM_5")
```

As distâncias em minutos saem como frações exatas de microssegundos por `60000000`; o excesso é zero para uma ponta dentro do limite e positivo para uma ponta fora dele. [não verificado: sonda não executada] A chamada de `classificar_orfas` sobre a projeção não prova que esse leitor tenha chamado o par de rajada na grade real; essa conclusão depende da grade e do vínculo das órfãs às batidas. [app/escala/orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53), [app/docs/RELATO.md:181](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/RELATO.md:181)

## 18/11 20:04 · sessao d3b0d5a45e98 · RAIA-ESPURIAS · leitura · origem=Code

A autorização resolve o bloqueio da volta 4: o trecho de casamento pode ser extraído e reutilizado. **Ainda não tenho uma sonda completa verificável para o item 2**: não concluí a ligação dos dias às perguntas, aos chamados e às linhas da lista do admin, nem a classificação de “turno real” sem presumir que toda batida sem `pergunta_origem` seja real. O próprio modelo admite batida plantada antiga com esse campo vazio. [Batida](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/models.py:34)

O bloco abaixo é a extração do casamento fornecido em `s1_v4.py`, preservando a ordem dos testes e seus resultados. Depende das mesmas funções `hhmm`, `instante`, `virada` e `local_minuto` do anexo. O anexo não tem caminho com linha na cópia verificado. [não verificado] [nao verificado]

```python
def casar_orfa(cel, valor, d, por_minuto):
    """Extracao do bloco de casamento de s1_v4.py.

    Retorna classe, motivo, batida, instante. Classe None significa
    casamento com tipo valido; a classificacao posterior fica ao chamador.
    """
    hora = hhmm(valor)
    if valor == "?":
        return "sem_hora", None, None, None
    if hora is None:
        return "NAO_CLASSIFICADA", "hora da ata invalida", None, None

    alvo = instante(cel.data, hora)
    hoje = por_minuto.get(alvo, [])
    amanha = por_minuto.get(alvo + timedelta(days=1), [])
    atravessa = virada(cel, d)

    if len(hoje) > 1:
        return (
            "nao_mensuravel_b",
            "multiplas batidas no minuto da celula",
            None,
            None,
        )
    if hoje and amanha:
        if atravessa is True:
            return (
                "nao_mensuravel_a",
                "virada e batidas nos dois dias",
                None,
                None,
            )
        if atravessa is None:
            return (
                "NAO_CLASSIFICADA",
                "virada desconhecida com batidas nos dois dias",
                None,
                None,
            )
    if not hoje:
        return (
            "NAO_CLASSIFICADA",
            "sem batida viva no minuto da data da celula",
            None,
            None,
        )

    b = hoje[0]
    instante_b = local_minuto(b.timestamp)
    if b.tipo not in ("E", "S"):
        return (
            "NAO_CLASSIFICADA",
            "tipo da batida desconhecido",
            b,
            instante_b,
        )
    return None, None, b, instante_b
```

A montagem do dono está confirmada na cópia; `cxr=None` e `dono(dia) is None` precisam de contadores distintos, pois o dono ainda pode responder sem a lavra. [cadastro_realidade.py:1003](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/cadastro_realidade.py:1003), [e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182)

Não executei sonda nem produzi os totais, a quebra por dono, os destinos ou a comparação com 645; tudo isso permanece **[não verificado]**. Não criei nem alterei arquivos. [não verificado] [nao verificado]

## 18/11 19:53 · sessao cc0cfb9ec34d · RAIA-ESPURIAS · leitura · origem=Code

Não consigo fornecer uma sonda completa que cumpra literalmente o requisito de reutilizar **a função de casamento do `s1_v4.py`**: no anexo fornecido, o casamento está dentro do laço principal, não em uma função chamável. As funções `hhmm`, `local_minuto`, `instante` e `virada` são auxiliares; nenhuma delas executa o casamento inteiro. [não verificado: o anexo fornecido não tem arquivo com linhas disponíveis na cópia]

Também há uma diferença relevante no tratamento da lavra: `dono(data)` pode responder mesmo quando `cxr=None`; retorna `None` quando a resposta dependeria da lista não lavrada. Portanto, **falta de cobertura da lavra e dono sem resposta devem ser contados separadamente**. [e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182), [cxr_na_lista.py:165](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/cxr_na_lista.py:165)

A montagem e a chamada verificadas são `InsumosDoDono(c, ini, fim, hoje, agora=agora, sem_motor=True, cxr=ler_cxr(c.pk, ini))` e `ins.dono(dia)`. [cadastro_realidade.py:1010](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/cadastro_realidade.py:1010), [e6_oraculo.py:128](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:128)

Para concluir o texto sem criar uma segunda implementação do casamento, preciso da função extraída do `s1_v4.py` ou de autorização para extrair **o bloco existente, sem alterar sua lógica**, para uma função reutilizada pela sonda. Até lá, a classificação, os destinos e a soma contra 645 permanecem **[não verificado]**.

## 18/11 19:51 · sessao 31bc149b290c · RAIA-ESPURIAS · leitura · origem=Code

A regra nova permite preparar o casamento por minuto exato. Ainda assim, o dado fornecido não contém as atas e batidas necessárias para calcular o quadro atual; os números abaixo serão produzidos somente quando a sonda for executada na sombra. A diferença para 1.267, por si só, não prova sua causa. [não verificado]

A sonda usa a ata como denominador e a grade como porta de classificação. Isso importa porque a grade pode devolver `fora_vinculo` sem transportar as órfãs de uma célula existente; nesse caso, a sonda as conserva em `NAO_CLASSIFICADA`. A lista de horas da ata também pode estar ausente, caso em que a grade representa a contagem por `?`. [app/escala/services/leitor_celula.py:180; app/escala/services/leitor_celula.py:379; app/escala/services/leitor_celula.py:389; app/escala/services/leitor_celula.py:420]

**Limite da sonda:** a virada é reconhecida quando os marcos da própria grade ou o DNA da célula apresentam entrada e saída. Se houver batidas no mesmo minuto nos dois dias e a virada não puder ser decidida por esses dados, a ocorrência fica em `NAO_CLASSIFICADA`. Para o turno real, a sonda chama `turnos_do_colab` e informa quantos turnos retornaram, mas as definições solicitadas para F e T1–T6 não dependem de um turno real completo. [app/escala/services/leitor_celula.py:413; app/escala/models.py:1672; app/ponto/turnos.py:1850; não verificado]

Salve o texto somente em ambiente autorizado e execute **apenas** `bin/sonda_leitura.sh --sombra <arquivo.py>`. Não executei a sonda nem criei arquivo: o envelope também cria trilha e pode iniciar a sombra, incompatíveis com a instrução desta chamada de não criar nem alterar arquivos. [bin/sonda_leitura.sh:103; bin/sonda_leitura.sh:252]

```python
# ITEM 1 — AUDITORIA-ESPURIAS — medida, nao juiz.
# Universo: CelulaDia de empresas 2, 3 e 4, 2026-10-21 a 2026-11-20.
# Fonte das ocorrencias: CelulaDia.ata; porta da classificacao: grade_da_celula.
# Casamento: Batida viva do mesmo colab no minuto exato da data da celula.
# Exclusoes anteriores as classes: sem_hora; nao_mensuravel (a) e (b).
# F: tipo_dia == folga; le grade_da_celula.
# T6: trabalho e grade demonstra ausencia de marcos; le grade_da_celula.
# T1: trabalho e algum marco missing a <=90 min; le grade_da_celula e Batida.
# T2: trabalho, ha marco missing e todos estao a >90 min; le grade_da_celula e Batida.
# T5: outra Batida viva do mesmo tipo a <=10 min; le Batida.
# T4: entre as restantes, pares de orfas E seguidas da S seguinte, em ordem;
#     le grade_da_celula e Batida.
# T3: restante com instante e tipo decidiveis; le grade_da_celula e Batida.
# T5 auxiliar: outra Batida viva de qualquer tipo a <=5 min; le Batida.
# Diagnostico adicional: turnos_do_colab; nao decide as classes acima.
# Precedencia: F, T6, T1, T2, T5, T4, T3.
# Dias e pessoas sao conjuntos por classe e podem aparecer em varias classes.

from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
import re

from django.db import DatabaseError
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from ponto.models import Batida
from ponto.turnos import turnos_do_colab

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
CLASSES = ("F", "T6", "T1", "T2", "T5", "T4", "T3",
           "nao_mensuravel_a", "nao_mensuravel_b",
           "sem_hora", "NAO_CLASSIFICADA")

erros = 0
totais = Counter()
dias = defaultdict(set)
pessoas = defaultdict(set)
verdes = Counter()
motivos = Counter()
t5_aux = Counter()
total_ata = 0
turnos_encontrados = 0
universo_completo = True


def campos(modelo):
    print("CAMPOS", modelo._meta.label,
          " ".join(f.name for f in modelo._meta.get_fields()
                   if getattr(f, "concrete", False)))


def registrar(classe, cid, dia, verde, motivo=None): [nao verificado]
    totais[classe] += 1
    dias[classe].add((cid, dia))
    pessoas[classe].add(cid)
    if verde is True: [nao verificado]
        verdes[classe] += 1
    elif verde is None: [nao verificado]
        verdes[classe + "_verde_indeterminado"] += 1
    if motivo:
        motivos[(classe, motivo)] += 1


def falha(etapa, exc, cid=None):
    global erros
    erros += 1
    # Nao imprimir mensagem da excecao: pode conter nome ou outro dado pessoal.
    print("ERRO", "etapa=" + etapa, "tipo=" + type(exc).__name__,
          "col=" + ("col" + str(cid) if cid is not None else "universo"))


def perm_negada(exc):
    atual = exc
    while atual is not None:
        if getattr(atual, "pgcode", None) == "42501":
            return True
        if getattr(atual, "sqlstate", None) == "42501":
            return True
        atual = getattr(atual, "__cause__", None)
    return False


def hhmm(valor):
    if not isinstance(valor, str) or not re.fullmatch(r"\d{2}:\d{2}", valor):
        return None
    h, m = map(int, valor.split(":"))
    if h > 23 or m > 59:
        return None
    return time(h, m)


def local_minuto(ts):
    return timezone.localtime(ts).replace(second=0, microsecond=0)


def instante(dia, hora):
    return timezone.make_aware(datetime.combine(dia, hora),
                               timezone.get_current_timezone())


def ocorrencias_da_ata(ata):
    # O cartorio normalmente grava ambos; divergencia e conservada e avisada.
    if not isinstance(ata, dict):
        raise ValueError("ata nao e objeto")
    lista = ata.get("orfas") or []
    if not isinstance(lista, list):
        raise ValueError("ata.orfas nao e lista")
    lista = [str(x) if x else "?" for x in lista]
    n = ata.get("n_orfas")
    n = len(lista) if n is None else int(n)
    if n < 0:
        raise ValueError("n_orfas negativo")
    if lista and len(lista) != n:
        print("ATA_CONTAGEM_DIVERGENTE",
              "lista=" + str(len(lista)), "n_orfas=" + str(n))
    # A lista lavrada e a fonte quando existe, como em _orfas_da_ata.
    return lista if lista else ["?"] * n


def virada(cel, dia_grade):
    # True/False so quando ha os dois extremos; None significa nao sei.
    pares = {}
    for marco in dia_grade.get("marcos") or []:
        if isinstance(marco, dict) and marco.get("tipo") in ("E", "S"):
            h = hhmm(marco.get("hora"))
            if h is not None:
                pares.setdefault(marco["tipo"], []).append(h)
    if pares.get("E") and pares.get("S"):
        return pares["S"][-1] <= pares["E"][0]
    dna = cel.dna if isinstance(cel.dna, dict) else {}
    marcos = dna.get("marcos") or {}
    if isinstance(marcos, dict):
        hi, hf = hhmm(marcos.get("hi")), hhmm(marcos.get("hf"))
        if hi is not None and hf is not None:
            return hf <= hi
    return None


def verde_do_dia(d):
    if d.get("tipo_dia") != "trabalho":
        return False
    celulas = d.get("celulas")
    if not isinstance(celulas, list):
        return None
    if not celulas:
        return None
    if any(not isinstance(c, dict) or "missing" not in c for c in celulas):
        return None
    return not any(c["missing"] for c in celulas)


def distancias_missing(d, data_celula, ts_orfa):
    # A ordem dos marcos permite datar marcos apos a virada.
    celulas = d.get("celulas")
    if not isinstance(celulas, list):
        return None, "celulas da grade ausentes"
    if any(not isinstance(c, dict) or "missing" not in c for c in celulas):
        return None, "estado missing desconhecido"
    if not any(c["missing"] for c in celulas):
        return [], None
    resultado = []
    dia_marco = data_celula
    anterior = None
    for c in celulas:
        h = hhmm(c.get("hora_prevista"))
        if h is None:
            if c["missing"]:
                return None, "marco missing sem hora prevista"
            # Sem hora de um marco intermediario, a virada posterior e incerta.
            return None, "marco sem hora impede datar os missing"
        if anterior is not None and h < anterior:
            dia_marco += timedelta(days=1)
        anterior = h
        if c["missing"]:
            resultado.append(abs((instante(dia_marco, h) - ts_orfa)
                                 .total_seconds()) / 60)
    return resultado, None


def tem_outra(batida, batidas, minutos, mesmo_tipo):
    for outra in batidas:
        if outra.pk == batida.pk:
            continue
        if mesmo_tipo and outra.tipo != batida.tipo:
            continue
        if abs((outra.timestamp - batida.timestamp).total_seconds()) <= minutos * 60:
            return True
    return False


for modelo in (Colaborador, CelulaDia, Batida):
    campos(modelo)

print("UNIVERSO", INI.isoformat(), FIM.isoformat(), "empresas=2,3,4")
print("REFERENCIA_17_11=1267")
print("DIA_VERDE_REFERENCIA=83_dias/50_pessoas/105_orfas [nao verificado]")

try:
    ids = list(
        CelulaDia.objects.filter(
            data__range=(INI, FIM),
            colaborador__empresa_id__in=EMPRESAS,
        ).values_list("colaborador_id", flat=True).distinct().order_by("colaborador_id")
    )
except Exception as exc:
    falha("listar_colaboradores_da_celula", exc)
    ids = []
    universo_completo = False

for cid in ids:
    etiqueta = "col" + str(cid)
    etapa = "carregar_celulas"
    cels = None
    ocorrencias = []
    try:
        cels = list(CelulaDia.objects.filter(
            colaborador_id=cid, data__range=(INI, FIM)
        ).order_by("data", "pk"))
        for cel in cels:
            for valor in ocorrencias_da_ata(cel.ata or {}):
                ocorrencias.append((cel, valor))
        total_ata += len(ocorrencias)
        if not ocorrencias:
            continue

        etapa = "carregar_colaborador"
        colab = Colaborador.objects.get(pk=cid)

        etapa = "grade_da_celula(colab, INI, FIM)"
        grade = grade_da_celula(colab, INI, FIM)
        por_data = {d["data"]: d for d in grade["dias"]}

        etapa = "turnos_do_colab(colab, INI, FIM)"
        turnos = turnos_do_colab(colab, INI, FIM)
        turnos_encontrados += len(turnos)

        etapa = "consultar_Batida_viva"
        # Margem cobre T5 nas bordas e casamento no dia seguinte.
        batidas = list(Batida.objects.filter(
            colaborador_id=cid,
            retratada_em__isnull=True,
            timestamp__date__gte=INI - timedelta(days=1),
            timestamp__date__lte=FIM + timedelta(days=1),
        ).order_by("timestamp", "pk"))
        por_minuto = defaultdict(list)
        for b in batidas:
            por_minuto[local_minuto(b.timestamp)].append(b)

        # Primeiro decide exclusoes e classes anteriores a T4.
        registros = []
        for cel, valor in ocorrencias:
            d = por_data.get(cel.data)
            r = {"cel": cel, "valor": valor, "classe": None,
                 "motivo": None, "batida": None, "verde": None} [nao verificado]
            registros.append(r)
            if d is None:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "dia ausente da grade"
                continue
            r["verde"] = verde_do_dia(d) [nao verificado]
            hora = hhmm(valor)
            if valor == "?":
                r["classe"] = "sem_hora"
                continue
            if hora is None:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "hora da ata invalida"
                continue

            alvo = instante(cel.data, hora)
            hoje = por_minuto.get(alvo, [])
            amanha = por_minuto.get(alvo + timedelta(days=1), [])
            atravessa = virada(cel, d)

            # (b) tem prioridade quando o proprio minuto da celula e multiplo.
            if len(hoje) > 1:
                r["classe"], r["motivo"] = "nao_mensuravel_b", "multiplas batidas no minuto da celula"
                continue
            if hoje and amanha:
                if atravessa is True:
                    r["classe"], r["motivo"] = "nao_mensuravel_a", "virada e batidas nos dois dias"
                    continue
                if atravessa is None:
                    r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "virada desconhecida com batidas nos dois dias"
                    continue
            if not hoje:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "sem batida viva no minuto da data da celula"
                continue

            b = hoje[0]
            r["batida"] = b
            r["instante"] = local_minuto(b.timestamp)
            if b.tipo not in ("E", "S"):
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "tipo da batida desconhecido"
                continue

            if tem_outra(b, batidas, 5, False):
                t5_aux["5min_qualquer_tipo"] += 1
            if tem_outra(b, batidas, 10, True):
                t5_aux["10min_mesmo_tipo"] += 1

            tipo = d.get("tipo_dia")
            if tipo == "folga":
                r["classe"] = "F"
                continue
            if tipo != "trabalho":
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "tipo_dia=" + str(tipo)
                continue

            celdia = d.get("celulas")
            marcos = d.get("marcos")
            if not isinstance(celdia, list) or not isinstance(marcos, list):
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "marcos ou celulas ausentes"
                continue
            if not marcos:
                if not celdia:
                    r["classe"] = "T6"
                else:
                    r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "ata agregada nao identifica marcos"
                continue

            dist, motivo = distancias_missing(d, cel.data, alvo)
            if dist is None:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", motivo
            elif dist:
                r["classe"] = "T1" if min(dist) <= 90 else "T2"
            elif tem_outra(b, batidas, 10, True):
                r["classe"] = "T5"

        # T4: pareia E com a S seguinte entre ocorrencias ainda elegiveis.
        # Se uma ocorrencia nao decidida pode interferir, nao forcar T4/T3.
        por_dia = defaultdict(list)
        for r in registros:
            por_dia[r["cel"].data].append(r)
        for data_celula, itens in por_dia.items():
            pendentes = [r for r in itens if r["classe"] is None]
            incerto = any(
                r["classe"] in ("NAO_CLASSIFICADA", "nao_mensuravel_a",
                                 "nao_mensuravel_b", "sem_hora")
                for r in itens
            )
            if incerto:
                for r in pendentes:
                    r["classe"] = "NAO_CLASSIFICADA"
                    r["motivo"] = "outra orfa incerta pode interferir no pareamento T4"
                continue
            pendentes.sort(key=lambda r: (r["instante"], r["batida"].pk))
            entrada = None
            for r in pendentes:
                if r["batida"].tipo == "E":
                    if entrada is not None:
                        entrada["classe"] = "T3"
                    entrada = r
                elif entrada is not None:
                    entrada["classe"] = r["classe"] = "T4"
                    entrada = None
                else:
                    r["classe"] = "T3"
            if entrada is not None:
                entrada["classe"] = "T3"

        for r in registros:
            registrar(r["classe"], cid, r["cel"].data,
                      r["verde"], r["motivo"]) [nao verificado]

    except Exception as exc:
        falha(etapa, exc, cid)
        if perm_negada(exc):
            print("PERMISSAO_DE_ESCRITA_NEGADA",
                  "chamada=" + etapa, "col=" + etiqueta,
                  "acao=parar_este_colab")
        # Se a ata ja foi contada, preserva todas as suas ocorrencias.
        if ocorrencias:
            for cel, valor in ocorrencias:
                registrar("NAO_CLASSIFICADA", cid, cel.data, None,
                          "erro na etapa " + etapa)
        else:
            universo_completo = False

print("TURNOS_REAIS_RETORNADOS", turnos_encontrados)
print("T5_AUX_5MIN_QUALQUER_TIPO", t5_aux["5min_qualquer_tipo"])
print("T5_AUX_10MIN_MESMO_TIPO", t5_aux["10min_mesmo_tipo"])
print("T5_AUX_ESCOPO=orfas_casadas_com_tipo_valido_antes_da_precedencia")

for classe in CLASSES:
    print("CLASSE", classe,
          "dias=" + str(len(dias[classe])),
          "pessoas=" + str(len(pessoas[classe])),
          "orfas=" + str(totais[classe]),
          "dia_verde_orfas=" + str(verdes[classe]),
          "verde_indeterminado=" + str(verdes[classe + "_verde_indeterminado"]))

for (classe, motivo), n in sorted(motivos.items()):
    print("MOTIVO", classe, "orfas=" + str(n), motivo)

soma = sum(totais.values())
print("TOTAL_ORFAS_ATA", total_ata,
      "universo_completo=" + str(universo_completo))
print("SOMA_CLASSES_E_EXCLUSOES", soma)
print("SOMA_CONFERE", soma == total_ata and universo_completo)
if universo_completo:
    print("DIFERENCA_PARA_1267", total_ata - 1267)
else:
    print("DIFERENCA_PARA_1267 [nao verificado]: universo incompleto")
print("ERROS=" + str(erros))
print("FIM_ITEM1")
```

As interfaces verificadas na cópia são `grade_da_celula` em [leitor_celula.py:350](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:350), `turnos_do_colab` em [turnos.py:1850](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1850) e a lavratura das horas em [cartorio.py:730](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:730). A execução, os totais e o comportamento das chamadas sob o papel leitor permanecem **[não verificado]**.

## 18/11 19:50 · sessao 988b5681a1a8 · RAIA-ESPURIAS · leitura · origem=Code

**Sonda parcial, não executada:** consegui conferir a porta da grade e preparar o levantamento de cobertura da lavra, mas não concluí a implementação e a revisão das seis classes. Portanto, os totais por classe, destinos, origens observadas, `SOMA_CONFERE` e `NAO_CLASSIFICADO` permanecem **[nao verificado]**.

Escolhi `grade_da_celula` para definir o universo: ela devolve `tipo_dia` a partir da ata e da célula e transporta as órfãs lavradas; para ata que só tem contagem, devolve `?`. [app/escala/services/leitor_celula.py:350] [app/escala/services/leitor_celula.py:180]

A cobertura precisa ser medida antes de atribuir donos: `cxr_lavrado` retorna `None` quando a lavra não cobre o colaborador na competência. [app/ponto/services/cxr_na_lista.py:165] A decisão técnica é **não chamar o motor**, independentemente da cobertura; os não cobertos devem ser contados como `dono_sem_resposta`, conforme o complemento fornecido. [nao verificado]

Não executei o envelope: além de não haver banco disponível, o próprio script prevê criação ou alteração de recursos, incluindo GRANTs e arquivos, o que conflita com a regra desta chamada de não criar, editar ou apagar arquivos. [bin/sonda_leitura.sh:1] [bin/sonda_leitura.sh:146] [nao verificado]

```python
# SONDA PARCIAL — ITEM 2 — NAO EXECUTADA
#
# Esta parte mede apenas o universo pela grade e a cobertura da lavra.
# Nao classifica dias, nao consulta destinos e nao chama InsumosDoDono.
#
# Porta: escala/services/leitor_celula.py:350 grade_da_celula
# Lavra: ponto/services/cxr_na_lista.py:165 cxr_lavrado
#
# As classes abaixo sao as definicoes solicitadas, AINDA NAO IMPLEMENTADAS:
# a: turno E..S completo na folga e trabalho declarado em dia vizinho;
#    le turnos_do_colab e grade_da_celula.
# b: turno E..S completo, sem a; le turnos_do_colab e grade_da_celula.
# d: orfa casada com pergunta_origem ou origem de declaracao; le Batida.
# e: orfa casada a ate 10 min de outra batida do mesmo tipo; le Batida.
# c: batida solta; le Batida e turnos_do_colab.
# f: restante com motivo; le os mesmos insumos anteriores.
# Precedencia: a > b > d > e > c > f.
#
# NAO USAR como resultado do item 2: faltam casamento, classificacao,
# destinos, dono, nao_mensuravel, reconciliacao e validacao em sombra.

from collections import Counter, defaultdict
from datetime import date

from django.apps import apps

from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from ponto.services.cxr_na_lista import cxr_lavrado

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
ERROS = Counter()


def erro(etapa, exc):
    ERROS[(etapa, type(exc).__name__)] += 1
    print("ERRO etapa={} tipo={}".format(etapa, type(exc).__name__))


def executar():
    for modelo in sorted(apps.get_models(), key=lambda m: m._meta.label_lower):
        print(
            "CAMPOS {} {}".format(
                modelo._meta.label,
                [campo.name for campo in modelo._meta.get_fields()],
            )
        )

    print(
        "UNIVERSO fonte=grade_da_celula "
        "empresas=2,3,4 inicio={} fim={}".format(INI, FIM)
    )
    print("PRECEDENCIA a>b>d>e>c>f IMPLEMENTADA=nao")
    print(
        "COMPARACAO_17_11 referencia=1267 "
        "total_geral_atual=[nao verificado] motivo=[nao verificado]"
    )

    try:
        celulas = list(
            CelulaDia.objects.filter(
                colaborador__empresa_id__in=EMPRESAS,
                data__range=(INI, FIM),
                ata__n_orfas__gt=0,
            )
            .select_related("colaborador")
            .order_by("colaborador_id", "data", "pk")
        )
    except Exception as exc:
        erro("carregar_celulas", exc)
        print("COBERTURA_LAVRA cobertos=[nao verificado] "
              "nao_cobertos=[nao verificado]")
        return

    por_colab = defaultdict(list)
    for cel in celulas:
        por_colab[cel.colaborador_id].append(cel)

    # Seleciona pela saida da grade, sem filtrar previamente por ata.tipo_dia.
    universo = defaultdict(list)
    grade_falhou = set()
    for cid, cels in sorted(por_colab.items()):
        try:
            grade = grade_da_celula(cels[0].colaborador, INI, FIM)
            dias = {dia["data"]: dia for dia in grade["dias"]}
            for cel in cels:
                dia = dias[cel.data]
                if dia["tipo_dia"] in ("folga", "ausencia"):
                    universo[cid].append((cel, dia))
        except Exception as exc:
            erro("grade_da_celula", exc)
            grade_falhou.add(cid)

    cobertos = 0
    nao_cobertos = 0
    cobertura_desconhecida = 0
    try:
        ler_cxr = cxr_lavrado()
    except Exception as exc:
        erro("cxr_lavrado", exc)
        ler_cxr = None

    for cid in sorted(universo):
        if ler_cxr is None:
            cobertura_desconhecida += 1
            continue
        try:
            cxr = ler_cxr(cid, INI)
            if cxr is None:
                nao_cobertos += 1
            else:
                cobertos += 1
        except Exception as exc:
            erro("cobertura_colab", exc)
            cobertura_desconhecida += 1

    print(
        "COBERTURA_LAVRA cobertos={} nao_cobertos={} desconhecidos={} "
        "colabs_com_grade_falha={}".format(
            cobertos, nao_cobertos, cobertura_desconhecida, len(grade_falhou)
        )
    )

    dias = sum(len(itens) for itens in universo.values())
    orfas = 0
    contagens_invalidas = 0
    for itens in universo.values():
        for cel, dia in itens:
            n = (cel.ata or {}).get("n_orfas")
            if isinstance(n, int) and not isinstance(n, bool) and n > 0:
                orfas += n
            else:
                contagens_invalidas += 1
            print(
                "DIA col{} data={} tipo_dia={} n_orfas={} "
                "horas_na_grade={}".format(
                    cel.colaborador_id,
                    cel.data.isoformat(),
                    dia["tipo_dia"],
                    n,
                    len(dia["orfas"]),
                )
            )

    print(
        "UNIVERSO_PARCIAL dias={} pessoas={} orfas={} "
        "contagens_invalidas={} grade_falhou={}".format(
            dias, len(universo), orfas, contagens_invalidas, len(grade_falhou)
        )
    )
    print("SOMA_CONFERE=[nao verificado]")
    print("NAO_CLASSIFICADO=[nao verificado]")
    print("ITEM2=INCOMPLETO")


try:
    executar()
except Exception as exc:
    erro("execucao", exc)
finally:
    print(
        "ERROS_DETALHE",
        {
            "{}:{}".format(etapa, tipo): n
            for (etapa, tipo), n in sorted(ERROS.items())
        },
    )
    print("ERROS={}".format(sum(ERROS.values())))
    print("FIM_ITEM2")
```

A parte ainda necessária é implementar o casamento por minuto com data e as exclusões do COMPLEMENTO-3, calcular as classes com a precedência pedida e incorporar dono e destinos. A lista administrativa captura algumas exceções internamente, de modo que uma ausência nela não basta, por si só, para provar ausência de destino. [app/escala/services/cadastro_realidade.py:790]

## 18/11 19:46 · sessao a784106a2534 · RAIA-ESPURIAS · leitura · origem=Code

```python
# Executar no shell Django do tenant, na sombra.
# Referencias da copia:
# app/ponto/models.py:22, :40, :45, :55, :67, :79
# app/colaboradores/models.py:321
# app/ponto/registro_batida.py:94
# app/escala/utils.py:631, :673
# app/ponto/services/cartorio.py:730, :745
#
# Pares: batidas adjacentes de cada colaborador na ordem (timestamp, pk).
# Em empate de timestamp, o menor pk vem primeiro. Em A-B-C, A-B e B-C
# contam separadamente quando cada par atende aos criterios; A-C nao conta.

from collections import Counter
from datetime import date, datetime, time, timedelta, timezone as dt_timezone

from django.utils import timezone
from ponto.models import Batida
from ponto.registro_batida import colisoes_tipo_oposto

ERROS = 0
INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
FAIXAS = ("0-10s", "11-60s", "61s-10min")
DIAS_COL67 = (
    date(2026, 11, 10),
    date(2026, 11, 11),
    date(2026, 11, 14),
    date(2026, 11, 15),
)


def erro(etapa, exc):
    global ERROS
    ERROS += 1
    print(f"ERRO etapa={etapa} tipo={type(exc).__name__}")


def instante(valor):
    if valor is None:
        return None
    if timezone.is_naive(valor):
        raise ValueError("datetime sem timezone")
    local = timezone.localtime(valor)
    # Milissegundos truncados; nao altera os instantes usados nos calculos.
    return local.strftime("%Y-%m-%d %H:%M:%S.") + (
        f"{local.microsecond // 1000:03d} " + local.strftime("%z")
    )


def faixa(segundos):
    if 0 <= segundos <= 10:
        return FAIXAS[0]
    if 10 < segundos <= 60:
        return FAIXAS[1]
    if 60 < segundos <= 600:
        return FAIXAS[2]
    return None


def limites_amplos(inicio, fim):
    # Margem em UTC; o teste definitivo e a data local de cada timestamp. [nao verificado]
    inicio_utc = datetime.combine(
        inicio - timedelta(days=2), time.min, tzinfo=dt_timezone.utc
    )
    fim_utc = datetime.combine(
        fim + timedelta(days=3), time.min, tzinfo=dt_timezone.utc
    )
    return inicio_utc, fim_utc


def dados_batida(b):
    return {
        "pk": b.pk,
        "timestamp_local": instante(b.timestamp),
        "tipo": b.tipo,
        "origem": b.origem,
        "canal": b.canal,
        "criado_em_local": instante(b.criado_em),
        "pergunta_origem_id": b.pergunta_origem_id,
        "retratada_em_local": instante(b.retratada_em),
        "retratada": b.retratada_em is not None,
    }


def executar():
    pares = []
    universo_disponivel = False

    print(
        "CRITERIO pares_adjacentes=(timestamp,pk) "
        "empate=menor_pk_primeiro encadeamento=AB_e_BC_se_elegiveis"
    )

    try:
        inicio_utc, fim_utc = limites_amplos(INI, FIM)
        candidatos = (
            Batida.objects.filter(
                retratada_em__isnull=True,
                colaborador__empresa_id__in=EMPRESAS,
                timestamp__gte=inicio_utc,
                timestamp__lt=fim_utc,
            )
            .select_related("colaborador")
            .order_by("colaborador_id", "timestamp", "pk")
        )
        batidas = []
        for b in candidatos:
            dia = timezone.localtime(b.timestamp).date()
            if INI <= dia <= FIM:
                batidas.append(b)

        anterior = None
        for b in batidas:
            if anterior is not None and anterior.colaborador_id == b.colaborador_id:
                segundos = (b.timestamp - anterior.timestamp).total_seconds()
                grupo = faixa(segundos)
                if grupo is not None and anterior.tipo != b.tipo:
                    pares.append((anterior, b, segundos, grupo))
            anterior = b
        universo_disponivel = True
        print(f"UNIVERSO batidas={len(batidas)} pares={len(pares)}")
    except Exception as exc:
        erro("universo_e_pares", exc)
        pares = []

    if universo_disponivel:
        try:
            for grupo in FAIXAS:
                itens = [p for p in pares if p[3] == grupo]
                print(
                    f"ITEM1 faixa={grupo} pares={len(itens)} "
                    f"pessoas={len({a.colaborador_id for a, _, _, _ in itens})}"
                )
                origens = Counter(
                    (
                        a.origem,
                        a.pergunta_origem_id is None,
                        b.origem,
                        b.pergunta_origem_id is None,
                    )
                    for a, b, _, _ in itens
                )
                for chave, quantidade in sorted(
                    origens.items(), key=lambda item: repr(item[0])
                ):
                    origem1, pergunta1_nula, origem2, pergunta2_nula = chave
                    print(
                        f"ITEM1_ORIGENS faixa={grupo} "
                        f"origem1={origem1!r} pergunta1_nula={pergunta1_nula} "
                        f"origem2={origem2!r} pergunta2_nula={pergunta2_nula} "
                        f"pares={quantidade}"
                    )
        except Exception as exc:
            erro("item1", exc)

        colidem_por_faixa = Counter()
        janelas_por_faixa = {grupo: Counter() for grupo in FAIXAS}
        avaliados_por_faixa = Counter()
        falhas_por_faixa = Counter()

        for a, b, segundos, grupo in pares:
            try:
                janela, ids = colisoes_tipo_oposto(
                    b.colaborador, b.timestamp, b.tipo
                )
                janela_s = janela.total_seconds()
                colide = a.pk in ids
                avaliados_por_faixa[grupo] += 1
                janelas_por_faixa[grupo][janela_s] += 1
                if colide:
                    colidem_por_faixa[grupo] += 1
                    print(
                        f"COLIDE_GRAVADO col{a.colaborador_id} "
                        f"faixa={grupo} segundos={segundos} "
                        f"janela_segundos={janela_s} "
                        f"b1={dados_batida(a)!r} b2={dados_batida(b)!r}"
                    )
            except Exception as exc:
                falhas_por_faixa[grupo] += 1
                erro("item2_par", exc)

        try:
            for grupo in FAIXAS:
                print(
                    f"ITEM2 faixa={grupo} avaliados={avaliados_por_faixa[grupo]} "
                    f"colidem={colidem_por_faixa[grupo]} "
                    f"falhas={falhas_por_faixa[grupo]}"
                )
                for janela_s, quantidade in sorted(janelas_por_faixa[grupo].items()):
                    print(
                        f"ITEM2_JANELA faixa={grupo} "
                        f"segundos={janela_s} pares={quantidade}"
                    )
        except Exception as exc:
            erro("item2_resumo", exc)

        try:
            for a, b, segundos, grupo in pares:
                print(
                    f"ITEM3 nao_mensuravel col{a.colaborador_id} "
                    f"b1_pk={a.pk} b2_pk={b.pk} "
                    f"faixa={grupo} segundos={segundos}"
                )
            print(f"ITEM3 total_nao_mensuravel={len(pares)}")
        except Exception as exc:
            erro("item3", exc)
    else:
        print("ITENS_1_2_3 indisponiveis=universo_nao_calculado")

    # O caso col67 e independente do filtro de empresas e inclui retratadas.
    for dia in DIAS_COL67:
        try:
            inicio_utc, fim_utc = limites_amplos(dia, dia)
            candidatos = (
                Batida.objects.filter(
                    colaborador_id=67,
                    timestamp__gte=inicio_utc,
                    timestamp__lt=fim_utc,
                )
                .select_related("colaborador")
                .order_by("timestamp", "pk")
            )
            do_dia = [
                b for b in candidatos
                if timezone.localtime(b.timestamp).date() == dia
            ]
            print(f"ITEM4 col67 dia={dia.isoformat()} batidas={len(do_dia)}")
            for b in do_dia:
                try:
                    registro = dados_batida(b)
                    try:
                        janela, ids = colisoes_tipo_oposto(
                            b.colaborador, b.timestamp, b.tipo
                        )
                        resultado = {
                            "janela_segundos": janela.total_seconds(),
                            "batidas_pks": ids,
                        }
                    except Exception as exc:
                        erro("item4_colisao", exc)
                        resultado = "indisponivel"
                    print(
                        f"ITEM4_BATIDA col67 dia={dia.isoformat()} "
                        f"batida={registro!r} colisao={resultado!r}"
                    )
                except Exception as exc:
                    erro("item4_batida", exc)
        except Exception as exc:
            erro("item4_dia", exc)


try:
    executar()
except Exception as exc:
    erro("execucao", exc)
finally:
    print(f"ERROS={ERROS}")
    print("FIM_S5II_V3")
```

- O caminho da empresa é `Batida.colaborador.empresa_id`: [app/ponto/models.py:22](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/models.py:22), [app/colaboradores/models.py:321](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/colaboradores/models.py:321). A função de colisão consulta batidas e devolve janela e IDs; seu corpo não grava: [app/ponto/registro_batida.py:94](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:94).

- A ata recebe as órfãs como texto; a formação examinada fornece `HH:MM`, sem instante completo. Por isso o item 3 não faz casamento: [app/ponto/services/cartorio.py:730](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:730), [app/escala/utils.py:631](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:631), [app/escala/utils.py:673](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:673).

- **Caminhos de escrita identificados para interpretar a saída:** `app`, com canal derivado `apk` ou `pwa`, passa por `registrar_batida` nas duas APIs e na triagem web: [app/api/views.py:654](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:654), [app/api/views_core.py:645](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views_core.py:645), [app/ponto/services/triagem_batida.py:700](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/triagem_batida.py:700). `manual` passa pela porta nos caminhos de [veredito](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/veredito_celula.py:323), [justificativa](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/justificativa.py:188), [ajuste retroativo](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/justificativa.py:361) e [aprovação em lote](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/views.py:1818). Também passam pela porta `excecao_autorizada`, `reg_externa`, `disputa_s84_retro` e `disputa_ancora`: [app/chamados/services/acoes_chamado.py:44](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/acoes_chamado.py:44), [app/chamados/services/regularizacao_ext.py:78](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/regularizacao_ext.py:78), [app/chamados/services/materializacao.py:1488](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:1488), [app/chamados/services/materializacao.py:1605](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:1605). A porta pode gravar colisão com autoridade humana: [app/ponto/registro_batida.py:162](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:162).

- `pergunta_origem` pode ser vinculada posteriormente, inclusive a batida reutilizada, por `anotar`: [app/chamados/services/materializacao.py:1537](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:1537). O campo `canal=NULL` admite mais de uma explicação na árvore: [app/ponto/models.py:46](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/models.py:46). Há também escrita em lote de dados sintéticos na sombra, sem passar por `registrar_batida`: [app/ponto/registro_batida.py:351](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:351), [app/ponto/management/commands/gerar_batidas_smoke_motor.py:107](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/gerar_batidas_smoke_motor.py:107).

- **Não confirmado:** [não verificado] quais valores aparecem nos pares reais, seus totais, e se existem valores adicionais ou históricos cujo caminho de escrita precise ser identificado. [não verificado] O caminho efetivamente percorrido por cada batida, ou se ela é anterior à regra, não pode ser atribuído apenas aos valores atuais. [não verificado] A sonda não foi executada na sombra nesta sessão.

## 18/11 19:43 · sessao 4a795076c73e · HOOK-B3-PAREI-LEGITIMO · leitura · origem=Code

A trilha fornecida contém **21 linhas `BLOCK` entre 18:54:01 e 18:55:28**, cada uma seguida de `TETO-NAO-CONTA`, além do par `BLOCK`/`ALLOW` de 18:51:07. O anexo descreve nove bloqueios seguidos, mas esse número não corresponde à quantidade de linhas `BLOCK` no recorte apresentado; a relação entre as duas contagens é **[não verificado]**. [Trilha fornecida, linhas 5–48; COMPLEMENTO-1 fornecido, item 1]

A cópia não tem metadados Git disponíveis: `git rev-parse HEAD` falhou. Portanto, a igualdade entre o HEAD da cópia e o hash informado é **[não verificado]**. A correspondência dos eventos com o código abaixo é direta quanto aos textos dos ramos, mas depende de o código executado ser o desta cópia; isso é especialmente relevante para 18:51, quando a trilha registra outro prefixo de HEAD. [Trilha fornecida, linhas 5–8; bin/hook_stop_fila1.py:1404; bin/hook_stop_fila1.py:1448]

### Bloqueio → ramo → motivo

| Instante | Ramo e motivo na trilha | Desfecho que a sequência mostra |
|---|---|---|
| 18:51:07 | `BLOCK fila 1 com item aberto`: registro em [bin/hook_stop_fila1.py:1404]. | Em seguida, `ALLOW ALARME teto...`: ramo `n > TETO and not _esperando`, em [bin/hook_stop_fila1.py:1439] e [bin/hook_stop_fila1.py:1448]; `_libera` registra e sai em [bin/hook_stop_fila1.py:145]. A versão executada com prefixo `c8b6d1ec25` é **[não verificado]**. |
| 18:54:01 | `BLOCK fila 1 com item aberto`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1427]. |
| 18:54:14 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:34 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:38 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:40 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA` às 18:54:41, [bin/hook_stop_fila1.py:1429]. |
| 18:54:46 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:53 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:55 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:57 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:54:59 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:02 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:05 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:10 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:12 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:15 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:17 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:19 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:21 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:24 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:26 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |
| 18:55:28 | Mesmo `BLOCK`, [bin/hook_stop_fila1.py:1404]. | Seguido de `TETO-NAO-CONTA`, [bin/hook_stop_fila1.py:1429]. |

O `BLOCK` da trilha é escrito **antes** do teto e do B2: sozinho, ele ainda não prova a decisão final. O par das 18:51 ilustra isso: houve `BLOCK` seguido de `ALLOW`. As linhas vizinhas de 18:46:31 e 18:47:52 mostram a mesma sequência com o prefixo antigo. [bin/hook_stop_fila1.py:1404; bin/hook_stop_fila1.py:1439; bin/hook_stop_fila1.py:1450; trilha fornecida, linhas 1–6]

Para os pares de 18:54–18:55, o código mostra que o ramo de trabalho em curso impede tanto a aplicação do teto quanto a consulta ao B2; depois disso, o caminho normal emite `decision: block`. Uma exceção posterior, porém, chama o caminho de falha aberta, e a trilha fornecida não registra a saída JSON do cliente; assim, o resultado final de cada chamada, considerado isoladamente, é **[não verificado]**. [bin/hook_stop_fila1.py:1427; bin/hook_stop_fila1.py:1439; bin/hook_stop_fila1.py:1450; bin/hook_stop_fila1.py:1459; bin/hook_stop_fila1.py:1460]

Antes do `BLOCK`, o fluxo passou pelas portas de B4, orçamento, PAREI, ESPERANDO comprovado e fila vazia sem sair por elas. A trilha não mostra o motivo particular da recusa de cada uma: por exemplo, não distingue ESPERANDO ausente de outras condições sem prova, nem informa o estado interno de B4 ou orçamento. [bin/hook_stop_fila1.py:1361; bin/hook_stop_fila1.py:1372; bin/hook_stop_fila1.py:1381; bin/hook_stop_fila1.py:1386; bin/hook_stop_fila1.py:1394]

### Veredito das hipóteses

- **(a) `[não verificável pela trilha]` quanto ao texto real; procede quanto ao código.** Se a primeira linha útil lida fosse `! PAREI: ...`, ela não começaria por `PAREI:` e `_parei_do_em_curso` retornaria falso. A trilha não guarda o topo do RELATO, então ela não prova que essa era a linha nem que esse foi o teste que falhou. Essa explicação é possível, mas sua probabilidade também é **[não verificado]**. [bin/hook_stop_fila1.py:413; bin/hook_stop_fila1.py:1260; bin/hook_stop_fila1.py:43]

- **(b) Procede como regra do código; sua ocorrência concreta é `[não verificável pela trilha]`.** Para o PAREI genérico liberar, o item em curso precisa ser identificado por ID inteiro na linha e não pode haver outro item andável; há ainda filtros como `!`, exclusão de CLEAR/COTA e SONDA. A trilha não registra o texto do PAREI nem a lista de andáveis, portanto não identifica qual condição falhou. [bin/hook_stop_fila1.py:1243; bin/hook_stop_fila1.py:1263; bin/hook_stop_fila1.py:1275; bin/hook_stop_fila1.py:1280; bin/hook_stop_fila1.py:1286; bin/hook_stop_fila1.py:1310; bin/hook_stop_fila1.py:270]

- **(c) Procede para teto e B2, condicionada à correspondência com a versão executada; “não sobra válvula” não procede como afirmação geral.** `TETO-NAO-CONTA` mostra que o detector retornou verdadeiro naquele ramo, mas não identifica qual processo foi detectado nem prova, por si só, sua natureza real. Com `_esperando` verdadeiro, o teto não libera e B2 é pulado. Ainda existem saídas anteriores, como PAREI ou ESPERANDO comprovado, e a falha aberta posterior; elas não explicam uma liberação na sequência normal observada dos pares de 18:54–18:55. [bin/hook_stop_fila1.py:1427; bin/hook_stop_fila1.py:1439; bin/hook_stop_fila1.py:1450; bin/hook_stop_fila1.py:1381; bin/hook_stop_fila1.py:1386; bin/hook_stop_fila1.py:1460]

### Quem mais lê e regras de pista

O alcance de uma cura concreta é **[não verificado]**, pois nenhuma mudança foi definida. Se tocar a leitura do topo, o B3 também depende dela: `disjuntor._topo_consistente` chama `_bloco_do_topo`, e `decide_b3` passa as linhas a `linhas_libera_b3`, que reconhece `DESENHO` e `ERRO-BOBO` com requisitos próprios. O handoff também chama os dois juízes de PAREI e lê a linha do topo; `_linha_do_topo` serve ainda ao ESPERANDO. [bin/disjuntor.py:764; bin/disjuntor.py:788; bin/disjuntor.py:728; bin/disjuntor.py:1029; bin/handoff_sessao.sh:120; bin/handoff_sessao.sh:129; bin/hook_stop_fila1.py:413]

A regra **HOOK-PAREI-COM-ANDAVEL** impede a liberação pelo PAREI genérico enquanto houver outro item andável; **HOOK-TETO-NAO-CONTA-ESPERA** impede contar a espera como avanço do teto e impede aplicá-lo enquanto o detector indica trabalho em curso. Elas estão explicitadas no código; não encontrei esses nomes literais em `CORTES.md` ou `CLAUDE.md` da cópia. A consolidação de parada e autonomia em `CLAUDE.md` aponta para a forma viva no hook e no disjuntor. [bin/hook_stop_fila1.py:1232; bin/hook_stop_fila1.py:1418; CLAUDE.md:251; resultado da busca nos dois arquivos: **[não verificado por arquivo:linha]**]

**Regra de pista que a cura muda: N = [não verificado]**; candidatas a avaliar conforme a cura forem definidas: `HOOK-PAREI-COM-ANDAVEL` e `HOOK-TETO-NAO-CONTA-ESPERA`. Alterar a interpretação do prefixo, por si só, não demonstra mudança em qualquer uma das duas regras. [bin/hook_stop_fila1.py:1232; bin/hook_stop_fila1.py:1263; bin/hook_stop_fila1.py:1418]

## 18/11 19:43 · sessao 4e4d144f130c · RAIA-ESPURIAS · leitura · origem=Code

**Sonda parcial, não executada:** as decisões da sessão permitem medir identidade, dono e destino, mas não encontrei na cópia uma função que produza as seis classes com a precedência pedida. A porta `classificar_orfas` devolve `folga_desmentida` para todas as órfãs de um dia sem trabalho; ela não distingue as classes (a)–(f). [app/escala/orfa_classe.py:53]

A sonda abaixo aplica o casamento autorizado, chama o dono para todos os colaboradores e mede os destinos. Ela conserva todos os dias em `NAO_CLASSIFICADO`, sem chamar casos ambíguos de descartados. A definição operacional necessária para distinguir as seis classes usando funções reais permanece **[nao verificado]**.

Para chamados, usa `dia_do_fato`, a porta da casa, com `fortes=True`; para a lista do admin, a presença é agregada por colaborador. A própria `lista` captura internamente algumas exceções; portanto, mesmo que a chamada retorne, a completude de seus resultados permanece **[nao verificado]**. [app/chamados/juizes.py:88] [app/escala/services/cadastro_realidade.py:773] [app/escala/services/cadastro_realidade.py:869]

A montagem do dono pode chamar `espelho_do_colab`; os efeitos transitivos completos dessa cadeia e da lista não foram verificados. Ambas são chamadas em transações revertidas por exceção. Rollback de banco não garante reversão de eventual efeito externo **[nao verificado]**. [app/ponto/management/commands/e6_oraculo.py:162] [app/ponto/management/commands/e6_oraculo.py:182]

```python
# SONDA PARCIAL — AUDITORIA-ESPURIAS ITEM 2 — NAO EXECUTADA
# Executar somente pelo envelope autorizado da sessao.
#
# Fontes:
# app/escala/orfa_classe.py:53 — porta de folga.
# app/ponto/turnos.py:2868 — batidas_apuraveis.
# app/ponto/management/commands/e6_oraculo.py:113,182 — dono.
# app/chamados/juizes.py:88 — dia_do_fato.
# app/escala/services/cadastro_realidade.py:773 — lista por colaborador.
#
# Casamento autorizado: uma hora da ata no dia D procura batidas
# apuraveis no dia civil D ou D+1. E a inversao da consulta do bloco B:
# cada batida olha a ata de seu dia civil e da vespera.
# 1 candidata = identificada; 0 ou >1 = ambigua.
# A ata continua sendo a fonte da contagem, inclusive sem lista coerente.
#
# Esta sonda NAO implementa um juiz novo para as classes (a)-(f).
# Precedencia solicitada, ainda nao aplicada: a > b > d > e > c > f. [nao verificado]

from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
from time import monotonic
import re

from django.apps import apps
from django.db import transaction
from django.utils import timezone

from chamados.juizes import dia_do_fato
from chamados.models import ChamadoColaborador, PerguntaDisputa
from escala.models import CelulaDia
from escala.orfa_classe import FOLGA_DESMENTIDA, classificar_orfas
from escala.services.cadastro_realidade import lista
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.turnos import batidas_apuraveis

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
UM_DIA = timedelta(days=1)
ERROS = Counter()


class ReverterSonda(Exception):
    pass


def erro(etapa, exc):
    chave = (etapa, type(exc).__name__)
    ERROS[chave] += 1
    print("ERRO etapa={} tipo={}".format(*chave))


def limite(dia):
    return timezone.make_aware(
        datetime.combine(dia, time.min),
        timezone.get_current_timezone(),
    )


def local(ts):
    if timezone.is_naive(ts):
        raise ValueError("datetime sem timezone")
    return timezone.localtime(ts)


def instante(ts):
    return local(ts).isoformat()


def executar():
    # Inclui modelos que as funcoes da casa podem ler indiretamente.
    for modelo in sorted(apps.get_models(), key=lambda m: m._meta.label_lower):
        print("CAMPOS {} {}".format(
            modelo._meta.label,
            [f.name for f in modelo._meta.get_fields()],
        ))

    # Relogio de leitura para os leitores que o exigem; nao limita o universo.
    agora = timezone.now()
    hoje = local(agora).date()
    print("RELOGIO_LEITURA", instante(agora))
    print("UNIVERSO empresas=2,3,4 inicio={} fim={} corte_hoje=nenhum".format(
        INI.isoformat(), FIM.isoformat()))
    print("PRECEDENCIA a>b>d>e>c>f NAO_APLICADA")
    print("COMPARACAO_17_11 valor=1267 unidade=sobras_todas_as_classes "
          "comparavel_com_folga=nao")
    print("LISTA_ADMIN_AGREGACAO=colaborador")

    celulas = list(
        CelulaDia.objects.filter(
            colaborador__empresa_id__in=EMPRESAS,
            data__range=(INI, FIM),
            ata__n_orfas__gt=0,
            ata__tipo_dia__in=("folga", "ausencia"),
        )
        .select_related("colaborador", "colaborador__empresa")
        .order_by("colaborador_id", "data", "pk")
    )

    por_col = defaultdict(list)
    for cel in celulas:
        por_col[cel.colaborador_id].append(cel)

    # Os indicadores abaixo sao triestado: True, False ou None (desconhecido).
    donos = {}
    perguntas = None
    chamados = None
    listados = None

    try:
        perguntas = set(
            PerguntaDisputa.objects.filter(
                colaborador_id__in=por_col,
                data_evento__range=(INI, FIM),
            ).values_list("colaborador_id", "data_evento")
        )
    except Exception as exc:
        erro("perguntas", exc)

    try:
        chamados = set()
        # Nao se restringe por criado_em: o dia do fato nao e o da criacao.
        qs = ChamadoColaborador.objects.filter(
            colaborador_id__in=por_col
        ).select_related("celula").order_by("colaborador_id", "pk")
        for chamado in qs.iterator():
            try:
                dia = dia_do_fato(chamado, fortes=True)
                if dia:
                    if isinstance(dia, datetime):
                        dia = local(dia).date()
                    elif isinstance(dia, str):
                        dia = date.fromisoformat(dia[:10])
                    if INI <= dia <= FIM:
                        chamados.add((chamado.colaborador_id, dia))
            except Exception as exc:
                erro("dia_do_fato", exc)
                # Uma falha impede afirmar ausencia de chamado.
                chamados = None
                break
    except Exception as exc:
        erro("chamados", exc)
        chamados = None

    try:
        resultado_lista = None
        try:
            with transaction.atomic():
                resultado_lista = lista(
                    hoje=hoje, empresas=EMPRESAS, agora=agora
                )
                raise ReverterSonda()
        except ReverterSonda:
            pass
        listados = set()
        if resultado_lista:
            for _posto, linhas in resultado_lista.get("postos", []):
                for linha in linhas:
                    listados.add(linha["colab_id"])
        print("LISTA_ADMIN estado=retornou "
              "falhas_internas_capturadas_pela_funcao=[nao_verificado]")
    except Exception as exc:
        erro("lista_admin", exc)
        listados = None

    # Monta uma vez por colaborador e chama dono para TODOS os seus dias.
    for cid, cels in sorted(por_col.items()):
        inicio_tempo = monotonic()
        try:
            try:
                with transaction.atomic():
                    ins = InsumosDoDono(
                        cels[0].colaborador, INI, FIM, hoje, agora=agora
                    )
                    for cel in cels:
                        try:
                            valor = ins.dono(cel.data)
                            donos[(cid, cel.data)] = (
                                valor if valor in (
                                    "ESTRUTURA", "CADASTRO", "BATIDA"
                                ) else "nao_sei"
                            )
                        except Exception as exc:
                            erro("dono", exc)
                            donos[(cid, cel.data)] = "nao_sei"
                    raise ReverterSonda()
            except ReverterSonda:
                pass
        except Exception as exc:
            erro("insumos_dono", exc)
            for cel in cels:
                donos[(cid, cel.data)] = "nao_sei"
        finally:
            print("TEMPO_DONO col{} segundos={:.3f}".format(
                cid, monotonic() - inicio_tempo))

    total_orfas = 0
    identificadas = 0
    ambiguas = 0
    dono_total = Counter()
    destino_total = Counter()
    pessoas = set()
    dias_contados = 0

    for cid, cels in sorted(por_col.items()):
        colab = cels[0].colaborador
        indice = None
        try:
            indice = defaultdict(list)
            bs = batidas_apuraveis(
                colab, limite(INI), limite(FIM + timedelta(days=2))
            ).order_by("timestamp", "pk")
            for batida in bs.iterator():
                dt = local(batida.timestamp)
                indice[(dt.date(), dt.strftime("%H:%M"))].append(batida)
        except Exception as exc:
            erro("batidas_apuraveis", exc)
            indice = None

        for cel in cels:
            try:
                ata = cel.ata
                n = ata["n_orfas"]
                if isinstance(n, bool) or not isinstance(n, int) or n <= 0:
                    raise ValueError("n_orfas invalido")

                dias_contados += 1
                total_orfas += n
                pessoas.add(cid)
                chave = (cid, cel.data)
                dono = donos.get(chave, "nao_sei")
                dono_total[dono] += 1

                p = None if perguntas is None else chave in perguntas
                c = None if chamados is None else chave in chamados
                l = None if listados is None else cid in listados
                if True in (p, c, l):
                    destino = "com_destino"
                elif None in (p, c, l):
                    destino = "desconhecido"
                else:
                    # A lista pode ocultar falha interna: nao afirmar ausencia.
                    destino = "sem_destino_aparente_lista_nao_verificada"
                destino_total[destino] += 1

                horas = ata.get("orfas")
                coerente = (
                    isinstance(horas, list)
                    and len(horas) == n
                    and all(
                        isinstance(h, str)
                        and re.fullmatch(r"\d{2}:\d{2}", h)
                        for h in horas
                    )
                )

                if coerente:
                    porta = classificar_orfas({
                        "tipo_dia": ata.get("tipo_dia"),
                        "orfas": horas,
                    })
                    if any(v != FOLGA_DESMENTIDA for v in porta.values()):
                        raise ValueError("porta_folga_nao_confirmada")

                    # Repeticao da mesma hora na ata tambem e ambigua:
                    # nao atribui uma mesma batida a duas ocorrencias.
                    frequencia = Counter(horas)
                    for hora in horas:
                        candidatas = []
                        if indice is not None:
                            candidatas = (
                                indice.get((cel.data, hora), [])
                                + indice.get((cel.data + UM_DIA, hora), [])
                            )
                        if (
                            indice is not None
                            and frequencia[hora] == 1
                            and len(candidatas) == 1
                        ):
                            identificadas += 1
                        else:
                            ambiguas += 1
                else:
                    # Cada ocorrencia contada na ata permanece no universo.
                    ambiguas += n

                print(
                    "DIA col{} data={} orfas={} dono={} pergunta={} "
                    "chamado={} lista_colab={} destino={} classe=nao_classificado".format(
                        cid, cel.data.isoformat(), n, dono, p, c, l, destino
                    )
                )
            except Exception as exc:
                erro("celula", exc)

    print("UNIVERSO dias={} pessoas={} orfas={}".format(
        dias_contados, len(pessoas), total_orfas))
    print("IDENTIDADE identificadas={} ambiguas={}".format(
        identificadas, ambiguas))
    print("DONO_NAO_CLASSIFICADO", dict(sorted(dono_total.items())))
    print("DESTINO_NAO_CLASSIFICADO", dict(sorted(destino_total.items())))
    print("SOMA_IDENTIDADE_CONFERE={}".format(
        identificadas + ambiguas == total_orfas))
    print("SOMA_CLASSES_CONFERE=[nao_verificado]")
    print("NAO_CLASSIFICADO={}".format(dias_contados))
    print("ITEM2=INCOMPLETO")


try:
    executar()
except Exception as exc:
    erro("execucao", exc)
finally:
    print("ERROS_DETALHE", {
        "{}:{}".format(etapa, tipo): n
        for (etapa, tipo), n in sorted(ERROS.items())
    })
    print("ERROS={}".format(sum(ERROS.values())))
    print("FIM_ITEM2")
```

A regra acima trata horas repetidas na própria ata como ambíguas para evitar atribuir uma única batida a duas ocorrências; essa restrição adicional não consta expressamente da decisão da sessão **[nao verificado]**. Os valores de produção, a soma das seis classes e `NAO_CLASSIFICADO` só poderão ser conhecidos após a execução e a definição verificável das classes **[nao verificado]**.

## 18/11 19:43 · sessao 7cdc59c5f93f · RAIA-ESPURIAS · leitura · origem=Code

O ponto principal é que **tela e PDF podem classificar a mesma batida por fontes diferentes**: na competência, a tela normalmente lê a ata; o PDF chama novamente o construtor da grade e usa seu resultado para marcar as batidas impressas ([espelho.py:506](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/espelho.py:506), [pdf_espelho.py:193](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:193), [utils.py:2334](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:2334)). **Caso real atual: [não verificado].**

A leitura abaixo é da cópia identificada no pedido; não executei aplicação nem sondas, e não tenho acesso ao banco ou aos logs de produção. O anexo informa 1.267 sobras na medição de 17/11, mas não fornece os casos necessários para verificar os pares de leitores hoje: **número atual, diferença e causa: [não verificado]**. [nao verificado]

| Leitor | Fonte e alcance observado | Unidade |
|---|---|---|
| Espelho, marca `orfas_grade` | Normalmente `grade_da_celula` transporta `ata['orfas']`; ata antiga sem horas pode virar uma lista de `?`. Antes do início da apuração a tela usa o construtor; também cai nele se `remapear_na_regua` levantar `AtaAgregadaSemMarcos`. Sem grade ou marcos, o adaptador retorna lista vazia. ([leitor_celula.py:180](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:180), [leitor_celula.py:420](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:420), [espelho.py:519](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/espelho.py:519), [espelho.py:528](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/espelho.py:528), [utils.py:1095](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1095), [espelho.html:240](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/ponto/espelho.html:240)). | Hora órfã na linha de dia-colab. |
| PDF/cartão, marca de espúria | `grade_espelho_janela` chama `montar_grade_prevista_periodo`; o PDF compara as horas dessa grade com as batidas das linhas trazidas pelo espelho e imprime as marcadas separadamente. A comparação é por `HH:MM`, não por identificador da batida. ([pdf_espelho.py:193](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:193), [utils.py:2348](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:2348), [pdf_espelho.py:602](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:602), [pdf_espelho.py:606](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:606), [pdf_espelho.py:874](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:874)). | Batida marcada, agrupada por dia-colab. |
| `furos_diarios.apurar_colab` | Conta `len(orfas)` da grade lavrada, mas só devolve dias elegíveis: trabalho ou ausência parcial que cobra resto, até ontem, com marco previsto; também exclui isentos e escala sob a régua indicada. O ranking soma o campo persistido de `FuroDiario`, portanto é uma projeção e pode depender de quando foi alimentada. ([furos_diarios.py:12](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:12), [furos_diarios.py:25](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:25), [furos_diarios.py:123](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:123)). | Órfãs por dia-colab; ranking agrega por colab ou outro eixo. |
| Vigia de cluster | Lê diretamente a ata; exige pelo menos duas horas órfãs a até 15 minutos e uma lâmpada apagada, e consulta o juiz de dia sem trabalho. Ata sem as listas necessárias é **não medida**. ([cluster_espurio.py:15](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cluster_espurio.py:15)). | Dia-colab com assinatura; acompanha lista de horas órfãs. |
| Vigia pareador × célula | Compara `CelulaDia.ata` com `turnos_do_colab`: procura borda de turno fechado que também consta como órfã, com lâmpada de ponta apagada. Exclui turnos não posicionáveis ou com mais de um turno no dia e informa `cego`. ([vigia_pareador_celula.py:42](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:42), [vigia_pareador_celula.py:93](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:93), [vigia_pareador_celula.py:113](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:113)). | Borda de turno; comando também conta colaboradores distintos ([comando:60](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/vigia_pareador_celula.py:60)). |
| `sobras_sem_razao` do aval | **Implementação e universo da função real da medida de 17/11: [não verificado].** Não consegui identificá-la na leitura realizada; não equiparo os vigias acima a esse instrumento. | [não verificado]. |
| Lista Cadastro × Realidade | A assinatura C9 mostra uma classe específica de turno aberto e saída órfã separada pelo teto, a partir da lavra de `teto_na_lista`, que lê o carimbo do pareador; a lista também combina outras assinaturas e filtra vínculos/empresas visíveis. Não é demonstrada como lista de todas as sobras da ata. ([cadastro_realidade.py:99](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:99), [cadastro_realidade.py:869](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:869)). | Linha por colab, com assinatura C9 de dia e ocorrências ([cadastro_realidade.py:890](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:890)). |
| Placar estrutural R2 | Guarda texto e número de medida; declara `cadastro_realidade.lista` como fonte para remedição. A linha não é uma contagem ao vivo de todas as órfãs. ([placar_estrutural.py:32](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/core/placar_estrutural.py:32), [placar_estrutural.py:135](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/core/placar_estrutural.py:135)). | Colaboradores na medida R2; não é medida geral de órfãs. |
| API `api_espelho_v2` | Usa o espelho para dados e totais, e monta batidas por dia com a geometria de `turnos_do_colab`; na estrutura examinada, cada batida tem tipo, hora, timestamp e origem, sem marca de órfã. O app recebe batidas e status, mas uma classificação de sobra nessa resposta não foi encontrada. ([views.py:1350](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:1350), [views.py:1430](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:1430), [views.py:1461](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:1461), [views.py:1535](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views.py:1535)). | Batida e dia; classificação explícita de órfã: [não verificado]. |

**Pares e sondas.** As sondas abaixo são propostas, **não executadas**. Usam datas fixas de 21/10 a 18/11/2026, que cabem na janela de competência 11 informada no aval; a janela efetiva por empresa deve ser conferida antes de interpretar ausência de caso. O envelope aceita `--sombra`, mas o próprio script documenta que mantém artefatos de infraestrutura e trilha: não o executei nesta tarefa de somente leitura de arquivos ([sonda_leitura.sh:4](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:4), [sonda_leitura.sh:29](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:29)).

**Tela × PDF.** Além da diferença ata versus construtor, o PDF só marca uma órfã quando encontra uma batida da linha com a mesma hora; a tela pode apresentar a hora da grade mesmo sem essa correspondência ([pdf_espelho.py:602](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:602), [utils.py:1121](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1121)). **Caso real: [não verificado].**

```python
import datetime as dt
from django.utils import timezone
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from ponto.models import Batida
from ponto.services.espelho import espelho_do_colab
from relatorios.pdf_espelho import _coletar_dados_espelho

INI, FIM = dt.date(2026, 10, 21), dt.date(2026, 11, 18)
AGORA = timezone.make_aware(dt.datetime(2026, 11, 19, 12))
for modelo in (Colaborador, CelulaDia, Batida):
    print("CAMPOS", modelo.__name__, [f.name for f in modelo._meta.fields])
print("INSTANTE", timezone.localtime(AGORA).isoformat())
erros, achou = 0, False
ids = CelulaDia.objects.filter(data__range=(INI, FIM)).values_list(
    "colaborador_id", flat=True
).distinct().order_by("colaborador_id")
for c in Colaborador.objects.filter(pk__in=ids).order_by("pk").iterator():
    try:
        tela = espelho_do_colab(c, FIM, INI, FIM, agora=AGORA)
        papel = _coletar_dados_espelho(c, INI, FIM, agora=AGORA)
        td = {d["data"]: d for d in tela["dias"] if INI <= d["data"] <= FIM}
        for d in papel["dias"]:
            dia = d["data"]
            if dia not in td:
                continue
            a = set(td[dia].get("orfas_grade") or [])
            b = {timezone.localtime(x.timestamp).strftime("%H:%M")
                 for x in d.get("batidas", []) if getattr(x, "orfa", False)}
            if a != b:
                print("DIVERGE col%s %s tela_n=%s pdf_horas_n=%s" %
                      (c.pk, dia.isoformat(), len(a), len(b)))
                achou = True
                break
        if achou:
            break
    except Exception as exc:
        erros += 1
        print("ERRO col%s %s" % (c.pk, type(exc).__name__))
print("CASO_ENCONTRADO=%s" % int(achou))
print("ERROS=%s" % erros)
```

**Ata × apuração de furos.** A mesma ata pode conter órfãs em folga ou outro dia excluído da apuração; nesse caso a ausência de linha em `apurar_colab` significa *fora do universo do contador*, não zero órfãs ([furos_diarios.py:32](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:32), [furos_diarios.py:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:49)). **Caso real: [não verificado].**

```python
import datetime as dt
from django.utils import timezone
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from escala.services.furos_diarios import apurar_colab

INI, FIM = dt.date(2026, 10, 21), dt.date(2026, 11, 18)
AGORA = timezone.make_aware(dt.datetime(2026, 11, 19, 12))
for modelo in (Colaborador, CelulaDia):
    print("CAMPOS", modelo.__name__, [f.name for f in modelo._meta.fields])
print("INSTANTE", timezone.localtime(AGORA).isoformat())
erros, achou = 0, False
ids = CelulaDia.objects.filter(data__range=(INI, FIM)).values_list(
    "colaborador_id", flat=True
).distinct().order_by("colaborador_id")
for c in Colaborador.objects.filter(pk__in=ids).order_by("pk").iterator():
    try:
        grade = grade_da_celula(c, INI, FIM)
        apurados = {x[0]: x[3] for x in apurar_colab(c, INI, FIM, hoje=AGORA.date())}
        for d in grade["dias"]:
            n = len(d.get("orfas") or [])
            if n and (d["data"] not in apurados or apurados[d["data"]] != n):
                print("DIVERGE col%s %s ata_n=%s apurado=%s tipo=%s" %
                      (c.pk, d["data"].isoformat(), n,
                       apurados.get(d["data"], "fora_do_universo"), d.get("tipo_dia")))
                achou = True
                break
        if achou:
            break
    except Exception as exc:
        erros += 1
        print("ERRO col%s %s" % (c.pk, type(exc).__name__))
print("CASO_ENCONTRADO=%s" % int(achou))
print("ERROS=%s" % erros)
```

**Ata × vigia de cluster.** Uma ou mais órfãs podem existir sem preencher a assinatura do vigia; uma ata sem listas também não pode ser tratada como ausência de cluster ([cluster_espurio.py:15](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cluster_espurio.py:15)). **Caso real: [não verificado].**

```python
import datetime as dt
from django.utils import timezone
from escala.models import CelulaDia
from ponto.services.cluster_espurio import _leitura_da_celula

INI, FIM = dt.date(2026, 10, 21), dt.date(2026, 11, 18)
AGORA = timezone.make_aware(dt.datetime(2026, 11, 19, 12))
print("CAMPOS CelulaDia", [f.name for f in CelulaDia._meta.fields])
print("INSTANTE", timezone.localtime(AGORA).isoformat())
erros, achou = 0, False
for cel in CelulaDia.objects.filter(data__range=(INI, FIM)).order_by(
        "colaborador_id", "data").iterator():
    try:
        n = len((cel.ata or {}).get("orfas") or [])
        if not n:
            continue
        resultado = _leitura_da_celula(cel)
        if not resultado:
            estado = "nao_medido" if resultado is None else "sem_assinatura"
            print("DIVERGE col%s %s ata_n=%s vigia=%s" %
                  (cel.colaborador_id, cel.data.isoformat(), n, estado))
            achou = True
            break
    except Exception as exc:
        erros += 1
        print("ERRO col%s %s" % (cel.colaborador_id, type(exc).__name__))
print("CASO_ENCONTRADO=%s" % int(achou))
print("ERROS=%s" % erros)
```

**Pareador × ata.** O próprio `censo` procura a divergência entre borda de par fechado e órfã lavrada, com filtros específicos; `cego=1` impede interpretar ausência de casos como confirmação de igualdade ([vigia_pareador_celula.py:42](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:42), [vigia_pareador_celula.py:97](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:97)). **Caso real: [não verificado].**

```python
import datetime as dt
from django.utils import timezone
from colaboradores.models import Colaborador, Empresa
from escala.models import CelulaDia
from ponto.models import Batida
from ponto.services.vigia_pareador_celula import censo

AGORA = timezone.make_aware(dt.datetime(2026, 11, 19, 12))
for modelo in (Colaborador, Empresa, CelulaDia, Batida):
    print("CAMPOS", modelo.__name__, [f.name for f in modelo._meta.fields])
print("INSTANTE", timezone.localtime(AGORA).isoformat())
erros = 0
try:
    r = censo(11, 2026)
    print("CEGO=%s" % r["cego"])
    if r["casos"]:
        cid, dia, borda, _hora = r["casos"][0]
        print("DIVERGE col%s %s borda=%s" % (cid, dia.isoformat(), borda))
    else:
        print("CASO_ENCONTRADO=0")
except Exception as exc:
    erros += 1
    print("ERRO", type(exc).__name__)
print("ERROS=%s" % erros)
```

Quanto aos juízes, o registro vivo declara a pergunta **“a batida é espúria (cluster)?” em `SEM_JUIZ`**, com decisões em sítios diferentes no detector e no cluster-guard; portanto há derivação fora de juiz único registrado para essa pergunta ([juizes.py:1224](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/core/juizes.py:1224)). Isso não demonstra, por si só, que toda divergência entre PDF e tela seja uma violação desse mesmo juiz: a classificação e os caminhos específicos precisam ser confrontados com o caso ([pdf_espelho.py:606](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:606), [espelho.py:519](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/espelho.py:519)).

O item **não está fechado**: faltam a identificação da função real de `sobras_sem_razao`, a conferência completa dos leitores “e afins”, sondas para os pares envolvendo lista/placar/API e a execução das sondas na sombra para apresentar casos reais. Tudo isso permanece **[não verificado]**.

## 18/11 19:42 · sessao 210aba8b0ce7 · RAIA-ESPURIAS · leitura · origem=Code

**A sonda completa está bloqueada pela identificação de D1.** O anexo informado dá o total de sete dias e três pessoas, mas não fornece as datas nem a função e o predicado da medida de 17/11. [não verificado] A cópia contém `escala.orfa_classe.classificar_orfas` e o leitor de cluster `dias_com_cluster`, mas não confirmei que qualquer um deles, isoladamente, reproduza a classe D1 daquela medida. [app/escala/orfa_classe.py:53; app/ponto/services/cluster_espurio.py:40]

A L-032 registra **90 minutos do marco de intervalo**; o pareador também compara a distância ao marco de borda. Esses são dois fatos distintos: o raio numérico consta da lei, enquanto a comparação adicional consta do código. [app/docs/LEIS.md:80; app/ponto/turnos.py:1246] O resultado por par, inclusive a distância fora do envelope, permanece [não verificado].

Abaixo vai uma **sonda parcial**, que imprime a função e o predicado reais de cluster, os dias que essa porta encontra e as batidas apuráveis ao redor deles. Ela marca esses dias como candidatos, não como os sete D1, e termina com erro explícito pela parte que falta. Não foi executada. [não verificado]

```python
# SONDA PARCIAL -- AUDITORIA-ESPURIAS item 5.
# Executar somente pelo envelope bin/sonda_leitura.sh --sombra <arquivo.py>.
# bin/sonda_leitura.sh:4-5, 54-59
#
# NAO identifica D1: a funcao e o predicado da medida de 17/11
# nao foram encontrados no material fornecido. [nao verificado]
#
# Porta de cluster: ponto/services/cluster_espurio.py:15,40
# Universo apuravel: ponto/turnos.py:2868
# Janela da competencia por empresa: ponto/janelas.py:54
# L-032: docs/LEIS.md:80, envelope de 90 min do marco de intervalo.
# Classe de orfas: escala/orfa_classe.py:28,40,53.

import datetime
import inspect

from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from escala.models import CelulaDia
from escala import orfa_classe
from ponto.models import Batida
from ponto.janelas import janela_fechamento
from ponto.services.cluster_espurio import (
    _leitura_da_celula,
    dias_com_cluster,
)
from ponto.turnos import batidas_apuraveis

ENVELOPE_L032_MIN = 90  # app/docs/LEIS.md:80
COLS = (252, 964, 444)
ERROS = 0


def erro(etapa, exc):
    global ERROS
    ERROS += 1
    # Apenas o tipo: a mensagem da excecao pode conter dados pessoais.
    print("ERRO etapa=%s tipo=%s" % (etapa, type(exc).__name__))


def instante(dt):
    return timezone.localtime(dt).isoformat()


def limite_local(dia):
    return timezone.make_aware(
        datetime.datetime.combine(dia, datetime.time.min),
        timezone.get_current_timezone(),
    )


def main():
    global ERROS

    # Nomes de campos antes das leituras dos modelos.
    for modelo in (Colaborador, Empresa, CelulaDia, Batida):
        print(
            "CAMPOS %s: %s"
            % (
                modelo._meta.label,
                ", ".join(f.name for f in modelo._meta.concrete_fields),
            )
        )

    print("L032 envelope_min=%s fonte=app/docs/LEIS.md:80" % ENVELOPE_L032_MIN)
    print(
        "JANELA_CLUSTER_MIN=%s fonte=app/escala/orfa_classe.py:28"
        % orfa_classe.JANELA_CLUSTER_MIN
    )
    for funcao in (
        orfa_classe._runs_de_rajada,
        orfa_classe.classificar_orfas,
        _leitura_da_celula,
        dias_com_cluster,
    ):
        print("FUNCAO %s.%s" % (funcao.__module__, funcao.__name__))
        print(inspect.getsource(funcao))

    print(
        "LIMITE: os dias retornados por dias_com_cluster sao candidatos "
        "de cluster; equivalencia com os 7 D1 [nao verificado]"
    )

    for cid in COLS:
        try:
            colab = Colaborador.objects.select_related("empresa").get(pk=cid)
            inicio, fim = janela_fechamento(11, 2026, colab.empresa)
            print("col%s competencia=11/2026 inicio=%s fim=%s" % (cid, inicio, fim))

            achados = dias_com_cluster(
                colab, None, fim, (fim - inicio).days + 1
            )
            print("col%s candidatos_cluster=%s" % (cid, len(achados)))

            for dia, orfas, missing in sorted(achados):
                try:
                    cel = CelulaDia.objects.get(colaborador_id=cid, data=dia)
                    dna = cel.dna if isinstance(cel.dna, dict) else {}
                    print(
                        "CANDIDATO col%s dia=%s orfas_hora_da_ata=%s "
                        "missing=%s trabalha=%s"
                        % (cid, dia, orfas, missing, cel.trabalha)
                    )
                    # Campos seguros e pertinentes do DNA; nao imprimir objetos
                    # ou repr de colaborador/batida, que podem conter nome.
                    print(
                        "DNA col%s dia=%s versao=%s marcos=%s "
                        "minutos_jornada=%s intervalo_livre=%s"
                        % (
                            cid, dia, dna.get("dna_versao"), dna.get("marcos"),
                            dna.get("minutos_jornada"), dna.get("intervalo_livre"),
                        )
                    )

                    # Contexto civil D-1 ate D+1. Nao afirma que todas essas
                    # batidas pertencem ao turno do dia.
                    ini = limite_local(dia - datetime.timedelta(days=1))
                    fim_exclusivo = limite_local(dia + datetime.timedelta(days=2))
                    bats = batidas_apuraveis(colab, ini=ini).filter(
                        timestamp__lt=fim_exclusivo
                    )
                    for batida in bats:
                        print(
                            "BATIDA_CONTEXTO col%s dia=%s instante=%s tipo=%s"
                            % (cid, dia, instante(batida.timestamp), batida.tipo)
                        )
                    print(
                        "PAR_S_E e ENVELOPE: [nao verificado]; "
                        "predicado D1 e atribuicao ao turno nao comprovados"
                    )
                except Exception as exc:
                    erro("dia_col%s_%s" % (cid, dia), exc)
        except Exception as exc:
            erro("col%s" % cid, exc)

    # Contabiliza a lacuna conhecida mesmo se todas as consultas funcionarem.
    ERROS += 1
    print(
        "ERRO etapa=identificacao_D1 tipo=FonteDaMedidaNaoVerificada "
        "esperado=7_dias"
    )


try:
    main()
except Exception as exc:
    erro("principal", exc)
finally:
    print("ERROS=%s" % ERROS)
    print("FIM_ITEM_5")
```

Por leitura de código, estes são os pontos relacionados que consegui verificar; **não é possível afirmar quais deles classificam os sete casos como intervalo sem as datas e os resultados da sonda D1**. [não verificado]

- `classificar_orfas` chama de `eco_cluster` uma corrida de ao menos duas órfãs em dia de trabalho com marco faltante; a corrida usa distância de até 15 minutos entre órfãs consecutivas. [app/escala/orfa_classe.py:26; app/escala/orfa_classe.py:40; app/escala/orfa_classe.py:53]
- `_leitura_da_celula`, usado por `dias_com_cluster`, chama de assinatura de cluster ao menos duas órfãs próximas e uma lâmpada apagada na ata. `pendencias` consome esses achados. [app/ponto/services/cluster_espurio.py:15; app/ponto/services/cluster_espurio.py:40; app/ponto/services/cluster_espurio.py:165]
- `papel_por_minuto_da_ata` usa `JANELA_CLUSTER_MIN` para reconhecer como eco de cluster uma batida próxima de luz acesa; esse predicado não é o mesmo do leitor de duas órfãs. [app/ponto/turnos.py:1565; app/ponto/turnos.py:1711]
- `_protegida_da_guarda` usa a mesma constante na exceção da guarda de rajada; o casamento de marcos contém a guarda chamada `cluster-guard`. [app/escala/utils.py:502; app/escala/utils.py:643]

Para fechar o item sem inventar a seleção, falta a função ou o texto da sonda da **medida de 17/11** que produziu os sete dias D1. [não verificado]

## 18/11 19:42 · sessao ef75106255e3 · RAIA-ESPURIAS · leitura · origem=Code

**Item 6 permanece aberto.** Não encontrei na cópia nem no anexo fornecido a porta da medida de 17/11, o intervalo exato da competência usado por ela ou os 73 pares identificados. Assim, não posso afirmar que uma consulta de pares reproduziria os **73 turnos / 34 pessoas**, nem conferir os **9 turnos reais colados antes**. [não verificado]

**Predicado de trabalho, ainda não verificado contra a medida:** turno formado pela função real da casa, com as duas pontas contendo `origem='disputa_s84_retro'` **ou** `pergunta_origem_id` preenchido; em cada ponta, a combinação e os campos devem ser impressos. A inclusão por `pergunta_origem` é necessária para não presumir que todo plantio conservou aquela origem; o campo também admite `NULL` para plantios antigos. [app/ponto/turnos.py:1850; app/ponto/models.py:33]

**Critério proposto de fase, não aplicado:** comparar, nos 14 dias anteriores a cada turno, os marcos das células de trabalho com os turnos completos formados a partir das batidas não plantadas; chamar “invertida” somente quando uma fase for inequivocamente diurna e a outra noturna. Os limites horários e o tratamento de fase mista não estão definidos no material fornecido. [não verificado] [nao verificado]

A sonda abaixo entrega a parte independente do universo: as células de **col366 entre 14 e 22/11/2026**, com DNA atual e anterior e carimbos de regeneração. Ela **não fecha o item** nem atribui causa à célula invertida. O DNA anterior conserva apenas a versão imediatamente anterior, de modo que não demonstra o histórico completo. [app/escala/models.py:1714]

A cópia registra que a sombra consultada em 18/11 às 18:47 era anterior à cura das 08:19; se essa ainda for a base usada pelo envelope, seus dados não demonstram o alcance da cura. A atualidade da sombra no momento da execução é [não verificado]. [app/docs/RELATO.md:28]

`_curar` grava arquivo e executa alteração de vínculo; não é chamada nesta sonda. [app/ponto/management/commands/censo_fase_12x36.py:339]

```python
# AUDITORIA-ESPURIAS, item 6 -- SONDA PARCIAL, NAO EXECUTADA.
#
# Predicado candidato do universo, NAO VERIFICADO contra a medida de 17/11:
# turno da funcao real com ambas as pontas satisfazendo, individualmente,
# origem == "disputa_s84_retro" OU pergunta_origem_id != None.
# Falta identificar janela, fonte das sobras e porta da medida.
#
# Criterio de fase proposto, NAO APLICADO: escala diurna x real noturno, [nao verificado]
# ou escala noturna x real diurno; limites horarios e fase mista indefinidos.
#
# Esta parte consulta exclusivamente CelulaDia de col366, 14 a 22/11/2026.
# Nao chama regeneracao, cura, construtor nem funcao que grave.
# Referencias: escala/models.py:1635, :1672, :1680, :1714;
# ponto/management/commands/gerar_celulas.py:73.

import datetime
import json

from django.utils import timezone
from escala.models import CelulaDia

erros = 1  # Item incompleto: universo e criterio de fase nao verificados.


def instante(valor):
    if valor is None:
        return None
    return timezone.localtime(valor).isoformat()


print("ITEM_6 SONDA_PARCIAL")
print("UNIVERSO=NAO_VERIFICADO; FASE=NAO_VERIFICADA")
print("ESPERADO_NO_ANEXO turnos=73 pessoas=34 reais_colados=9")
print("CAMPOS CelulaDia:", ", ".join(f.name for f in CelulaDia._meta.fields))

try:
    inicio = datetime.date(2026, 11, 14)
    fim = datetime.date(2026, 11, 22)
    celulas = list(
        CelulaDia.objects.filter(
            colaborador_id=366,
            data__gte=inicio,
            data__lte=fim,
        ).order_by("data", "pk")
    )
    por_dia = {}
    for cel in celulas:
        por_dia.setdefault(cel.data, []).append(cel)

    dia = inicio
    while dia <= fim:
        encontrados = por_dia.get(dia, [])
        if not encontrados:
            print("col366 data=%s CELULA_AUSENTE" % dia.isoformat())
        for cel in encontrados:
            try:
                dna = cel.dna
                marcos = (dna or {}).get("marcos")
                registro = {
                    "colaborador": "col366",
                    "celula_pk": cel.pk,
                    "data": cel.data.isoformat(),
                    "trabalha": cel.trabalha,
                    "origem": cel.origem,
                    "escala_geradora_id": cel.escala_geradora_id,
                    "gerada_em": instante(cel.gerada_em),
                    "editada_em": instante(cel.editada_em),
                    "regenerada_em": instante(cel.regenerada_em),
                    "regeneracoes": cel.regeneracoes,
                    "marcos_presentes": CelulaDia.marcos_presentes(marcos),
                    "dna": dna,
                    "dna_anterior": cel.dna_anterior,
                }
                print(json.dumps(registro, ensure_ascii=False, default=str))
            except Exception as exc:
                erros += 1
                print(
                    "ERRO col366 data=%s celula_pk=%s tipo=%s"
                    % (dia.isoformat(), cel.pk, type(exc).__name__)
                )
        dia += datetime.timedelta(days=1)
except Exception as exc:
    erros += 1
    print("ERRO leitura_col366 tipo=%s" % type(exc).__name__)

print("ALCANCE_CURA_COL366=NAO_VERIFICADO")
print("CAUSA_CELULA_INVERTIDA_COL366=NAO_VERIFICADA")
print("FASES_E_CONTAGENS_ITEM_6=NAO_MEDIDAS")
print("ERROS=%d" % erros)
print("FIM_ITEM_6")
```

A sonda não foi executada. Para completá-la é necessário recuperar a função e o recorte da medida de 17/11, além do critério de fase; até lá, listas e vereditos individuais permanecem [não verificado].

## 18/11 19:42 · sessao 3593b933b4bd · QUEIMA-CODEX-B · leitura · origem=Code

A sonda fornecida mostra que, nos 28 dias, a grade e a ata apontam os mesmos marcos faltantes; ela **não mostra** o veredito gravado, o julgamento atual nem os horários necessários para distinguir veredito velho de julgamento atual concorde ([RELATO.md:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:49); dados por dia no anexo `sonda28.20261118_172212.out`, numeração de linhas [nao verificado]). O item 5 do aval fornecido registra a opção 2 e pede essa medida adicional; a pergunta posterior mantém as duas opções abertas para este levantamento ([PROMPTS.md:2571](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/PROMPTS.md:2571), [RELATO.md:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:49)).

A árvore fornecida não tem metadados Git acessíveis no diretório informado; sua identidade com o hash pedido está [nao verificado].

**Opção 1 — (c) sai da raia e a divergência é atribuída a outro leitor, após censo.** A mudança expressa é de destino e investigação: retirar a obra (c) da raia e identificar o leitor responsável; a opção não especifica uma alteração de código ([RELATO.md:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:49), [BACKLOG.md:2352](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2352)). A divergência registrada de EFEITOS-REAIS #2 é entre **ata e veredito**: a ata acusa o marco e o emissor obedece ao veredito concorde ([RELATO.md:121](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:121), [RELATO.md:123](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:123)). O leitor exato usado pelo `guarda29` permanece [nao verificado].

Os leitores conferidos não fazem todos a mesma pergunta:

- [leitor_celula.py:350](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:350) expõe a grade a partir da ata; [utils.py:1089](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1089) expõe `missing`; [declaracao_texto.py:110](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/declaracao_texto.py:110) usa marcos faltantes para declaração, com filtros próprios.
- [supra_juiz.py:317](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:317) conta `missing` para julgar o dia; [fabrica_da_celula.py:120](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:120) lê o **veredito gravado** para decidir se cala; [furos.py:103](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/furos.py:103) usa status lavrado e ata para finalidades distintas. Assim, concordância no marco `missing` não demonstra concordância no julgamento ou no dever de perguntar ([supra_juiz.py:387](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:387), [fabrica_da_celula.py:120](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:120)).

**Opção 2 — (c) fica e a sonda mede o leitor usado pelo guarda29.** O aval fornecido determina comparar, por dia, veredito gravado e `julgada_em`, retorno atual de `classificar_dia` e datas dos últimos fatos; se o julgamento atual acusar e o gravado for anterior ao último fato, o ramo é “veredito velho”; se o atual também for concorde, pede o ramo que cala e pré-desenho próprio ([PROMPTS.md:2571](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/PROMPTS.md:2571); literal do anexo `reprovas_1811_1740.txt`, numeração de linhas [nao verificado]).

Os sítios para essa medida e eventual desenho são:

- [models.py:1745](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/models.py:1745) guarda `julgada_em`; [celula.py:1181](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/portas/celula.py:1181) mostra que o carimbo é idempotente e não necessariamente avança numa repetição sem mudança. Logo, seu significado é o do carimbo dessa porta, não uma prova isolada de que não houve execução posterior ([celula.py:1188](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/portas/celula.py:1188)).
- [cartorio.py:1398](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1398) contém a guarda de julgamento em dia; [cartorio.py:1416](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1416) prepara fatos da grade; [cartorio.py:1464](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1464) chama o juiz; [cartorio.py:1851](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1851) lavra o resultado e a ata.
- [supra_juiz.py:217](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:217) devolve códigos; o ramo de furo parcial depende de outros fatos além de `missing` ([supra_juiz.py:387](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:387), [supra_juiz.py:429](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:429)). Também chamam o juiz [cartorio.py:1269](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1269), [supra_juiz.py:223](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/supra_juiz.py:223) e [emitir_furo_retroativo.py:101](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/emitir_furo_retroativo.py:101). A equivalência de todos os insumos desses chamadores está [nao verificado].
- A fábrica lê `veredito`, `ata` e `julgada_em` para perguntas diferentes ([fabrica_da_celula.py:120](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:120)); a re-lavra das perguntas também usa marcos temporais da célula ([veredito_lavrado.py:186](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/veredito_lavrado.py:186)). Esses consumidores não tratam todos os campos como equivalentes. O censo completo dos leitores e escritores dos campos tocados está [nao verificado].

**Lei existente.** Nenhuma linha encontrada em `LEIS.md` ou `CORTES.md` decide expressamente se (c) sai ou fica, ou identifica o leitor usado pelo `guarda29`. Busquei `RAIA-INTERVALO`, `REPROVAS-1811-1740`, `EFEITOS-REAIS`, `guarda29`, `sonda28`, `projeção reconstruível`, `rejulgar`, `re-julgar`, `julgada_em` e `fato muda`. Há regras que respondem **parcialmente ao desenho condicional**:

- L-061: “O CONSUMIDOR LE A LAMPADA, nunca re-julga: chamado, pergunta, tela e folha leem a celula/ata” ([LEIS.md:95](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:95)).
- L-124: “fato novo rejulga normal em qualquer competencia” ([LEIS.md:145](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:145), [CORTES.md:461](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:461)). Isso não demonstra que houve fato posterior nos 28 dias [nao verificado].

| Opção | Sítios | Leitores | Lei existente |
|---|---|---|---|
| 1: (c) sai; censo nomeia leitor | [RELATO.md:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:49), [BACKLOG.md:2352](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2352) | Grade, juiz, fábrica e relatório usam os fatos para decisões distintas; leitor do guarda29 e censo completo [nao verificado] | Nenhuma linha encontrada decide a saída; L-061 trata consumidores ([LEIS.md:95](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:95)). |
| 2: (c) fica; medir julgamento | [cartorio.py:1398](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1398), [supra_juiz.py:217](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:217), [celula.py:1181](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/portas/celula.py:1181) | Cartório e comandos chamam o juiz; fábrica e re-lavra consomem campos gravados. Equivalência completa [nao verificado] | Nenhuma linha encontrada decide a permanência; L-124 responde ao caso de fato novo ([LEIS.md:145](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:145)). |

## 18/11 19:42 · sessao 98dbc781cb85 · RAIA-ESPURIAS · leitura · origem=Code

**Não consigo entregar uma sonda que garanta a partição pedida com `NAO_CLASSIFICADA=0` sem inventar a identidade ou o instante de algumas órfãs.** A decisão da sessão resolve qual porta usar; ela não acrescenta à ata os dados necessários às classes T1–T5. [app/escala/services/leitor_celula.py:180; app/ponto/services/cartorio.py:730]

- A ata grava as órfãs como strings de hora, sem identificador da batida, tipo E/S ou data do instante. A porta devolve `?` para cada órfã de uma ata que tenha apenas contagem. Assim, essas órfãs podem ser contadas, mas não há hora para medir distância nem tipo para decidir T5 ou T4. [app/ponto/services/cartorio.py:742; app/escala/services/leitor_celula.py:180]
- Consultar `batidas_apuraveis` fornece batidas com timestamp e tipo, mas essa função não estabelece qual delas corresponde a cada string da ata. Uma correspondência por minuto pode ter mais de uma candidata; o raio-x fornecido mostra, por exemplo, duas batidas de tipos opostos no mesmo minuto para `col67` em 14/11. Escolher uma exigiria uma regra de associação ainda não demonstrada. [app/ponto/turnos.py:2868; app/ponto/models.py:22; não verificado]
- A data da célula é a do dia da grade; atribuí-la automaticamente à hora da órfã também não resolve o instante em turnos que atravessam a meia-noite. O próprio dado fornecido mostra uma órfã apresentada no dia 10 com batidas após a virada para o dia 11. [app/escala/services/leitor_celula.py:375; não verificado]
- A função real `classificar_orfas` permite identificar `folga_desmentida` pela grade, mas classifica qualquer `tipo_dia` diferente de `trabalho` nessa classe, inclusive `indefinido`; não é equivalente, em todos os casos, ao juiz `dia_sem_trabalho`. Além disso, seu retorno é um dicionário por hora, portanto não preserva como entradas distintas duas órfãs com a mesma hora. [app/escala/orfa_classe.py:53; app/chamados/juizes.py:445]
- O rótulo T4 exige um par E→S de órfãs, mas o pedido não determina como escolher pares quando há várias candidatas; tampouco identifiquei uma função real que associe as strings da ata a batidas e devolva exatamente essa classificação. [app/ponto/services/cartorio.py:745; não verificado]

Há também uma diferença de definição que impede chamar o resultado de reprodução da medida de 17/11: o anexo daquela medida descreve T5 como **outra batida até 5 minutos**, enquanto a instrução atual determina **mesmo tipo até 10 minutos**. A instrução atual pode governar uma nova medida, mas sua diferença para 1.267 não pode ser atribuída a uma causa específica sem execução e sem o script perdido. [não verificado]

Para tornar a sonda completa e auditável, falta uma decisão sobre **como associar cada ocorrência de hora da ata a uma batida datada e tipada, inclusive candidatos múltiplos e virada de dia**, e como parear as ocorrências para T4. As órfãs sem hora ainda precisariam permanecer numa linha própria, sem afirmar uma das classes que dependem de hora ou tipo. Até essas decisões, `NAO_CLASSIFICADA` pode ser maior que zero; imprimir zero por construção ocultaria a falta de evidência. [app/escala/services/leitor_celula.py:180; não verificado]

Não executei sonda nem alterei arquivos. Não há resultado atual, `ERROS=0` ou causa comprovada para eventual diferença em relação a 1.267. [não verificado] [nao verificado]

_7139 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
