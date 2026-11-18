# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 12:47** · publicado em **18/11/2026 12:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 12:47 · sessao e36752fddc88 · EVAL-NOTURNO-SEM-VEREDITO · verificador · origem=Code

**AVAL-DEVOLVIDO — item 3:** procede devolver as perguntas **1 e 3** para completar o censo e fechar os casos do desenho. Não identifiquei colisão com o aval ou com a lei escrita, nem toque declarado em exportada ou dinheiro, nos limites da cópia e dos anexos. O próprio aval mantém a obra parada até o rearme do B3. [anexo aval.txt, item 3: “A obra segue PARADA ate eu rearmar o B3”]

**1. Premissas do código e censo — PROCEDE devolver parcialmente.**

- As linhas citadas existem e explicam a reprovação: o código escolhe `max(datas)` **antes** de verificar se a data é futura. Assim, um futuro sozinho, ou coexistindo com um relatório anterior válido, produz `medido=False`; a linha publicada diz “sem medida”, sem `ALERTA`. [bin/eval_estado.py:51](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:51) [bin/eval_estado.py:56](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:56) [bin/eval_estado.py:63](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:63)
- Pasta vazia e pasta sem data válida também dão `sem medida`, sem `ALERTA`; pasta ausente cai no tratamento de `OSError` e tem a mesma saída geral. [bin/eval_estado.py:38](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:38) [bin/eval_estado.py:51](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:51) [bin/eval_estado.py:63](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:63)
- `quem-mais-le=bin/gerar_estado.py` identifica o consumidor de publicação: ele importa `ler` e `linha`, e inclui o resultado no ESTADO. O censo textual também encontra o selo, que executa a CLI do medidor, lê seu JSON e suas linhas, e verifica a integração por `grep`; `bin/relato.sh` chama o gerador, indiretamente. Devolver a linha de censo com esses papéis distinguidos. [bin/gerar_estado.py:316](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:316) [bin/gerar_estado.py:402](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:402) [bin/tests/test_eval_estado.sh:19](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:19) [bin/tests/test_eval_estado.sh:30](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:30) [bin/relato.sh:38](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/relato.sh:38)
- A identidade Git da cópia com o hash informado é **[não verificado]**: a cópia disponibilizada não tem metadados Git acessíveis.

**2. Colisão com aval ou lei escrita — NÃO PROCEDE**, quanto ao desenho declarado. O item 4 original pede a idade pelo nome, limite `<= 1`, publicação no ESTADO e caso de três dias; o complemento especifica como tratar futuro e ausência. Não encontrei, no censo textual desses nomes e da fonte, outra implementação que responda à mesma idade: o medidor existente é `eval_estado.ler`, e o gerador consome seu resultado. Isso é conclusão limitada ao censo textual, não prova contra chamadas dinâmicas desconhecidas. [anexo aval_origem.txt, item 4: “esperado <= 1, fonte = data do ultimo eval/runs/relatorio_*.md”] [anexo aval.txt, item 3: “relatorio com data FUTURA nao conta”] [bin/eval_estado.py:32](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:32) [bin/gerar_estado.py:316](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:316)

A implementação deve continuar usando essa autoridade, com contrato de instrumento e pouso próprio previstos nas regras; a dispensa de padrão para instrumento depende de não tocar código de negócio. [CLAUDE.md:394](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/CLAUDE.md:394) [app/docs/LEIS.md:131](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/app/docs/LEIS.md:131) [app/docs/LEIS.md:144](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/app/docs/LEIS.md:144)

**3. Condições que não fecham no literal — PROCEDE devolver os pontos abaixo.**

- **Hoje e futuro sozinho.** Na publicação atual, a borda captura `date.today()` ou recebe data explícita e a passa ao medidor; o escritor usa `time.strftime` no nome. Ambos usam a data local do processo, mas a configuração efetiva de fuso em produção é **[não verificado]**. O literal manda excluir o futuro e dar-lhe linha própria, e manda `ALERTA sem medida` para pasta vazia, ausente ou “sem nome valido”; não diz expressamente se um futuro sozinho, cujo nome e data são válidos mas inelegíveis, também recebe `ALERTA sem medida`. Devolver essa saída combinada para ser fixada. [bin/gerar_estado.py:417](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:417) [eval/noturno.py:113](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/eval/noturno.py:113) [anexo aval.txt, item 3: “data <= hoje” e “pasta vazia, ausente ou sem nome valido = ALERTA ‘sem medida’”]
- **Limite.** “Mais de 1 dia” coincide com o código: `ESPERADO_MAX = 1` e `dias > ESPERADO_MAX`. Dois dias já alertam; o selo atual verifica esse limite. Não há condição a devolver nesse ponto. [bin/eval_estado.py:29](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:29) [bin/eval_estado.py:59](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:59) [bin/tests/test_eval_estado.sh:51](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:51)
- **Nome inválido.** `relatorio_2026-13-45.md` casa com a expressão, falha na construção da data e entra em `invalidos`; `relatorio_x.md` não casa e é ignorado. Ambos isoladamente deixam a pasta sem data válida, mas não percorrem o mesmo ramo. Devolver a definição de quais nomes inválidos devem ser reconhecidos e testados. [bin/eval_estado.py:28](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:28) [bin/eval_estado.py:43](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:43)
- **Cobertura.** Os quatro exemplos nomeados não especificam, por si, futuro coexistindo com relatório válido, nem pasta ausente ou as duas classes de nome acima. “Mutante por ramo” exige mordida nos ramos que forem implementados, mas não descreve esses arranjos de entrada. Devolver a inclusão explícita do caso misto: idade e eventual alerta do válido **mais** linha do futuro; acrescentar casos para ausência e para as classes de nome que o desenho vier a exigir. [anexo aval.txt, item 3: “Selo com caso que MORDE em cada um dos quatro (...) e mutante por ramo”] [bin/eval_estado.py:43](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:43)
- **RED pelo caso.** É possível nesta cópia: o medidor existe e o selo já o chama com data e raiz injetadas. Um teste das saídas novas pode falhar pelo comportamento atual, sem depender da ausência do arquivo. O selo atual, porém, só exige `sem medida` nos casos sem relatório e só muta o limite; não prova os novos alertas nem o ramo futuro. [bin/tests/test_eval_estado.sh:19](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:19) [bin/tests/test_eval_estado.sh:66](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:66) [bin/tests/test_eval_estado.sh:79](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/tests/test_eval_estado.sh:79)

**4. Exportada, dinheiro ou `!` fora do declarado — NÃO PROCEDE**, para o desenho e os arquivos do contador examinados: o medidor lê nomes em `eval/runs`, calcula a idade e compõe texto; o gerador o publica no ESTADO. Não há, nesse fluxo examinado, escrita de competência ou operação de dinheiro. Uma implementação futura fora desses arquivos ou seus efeitos indiretos são **[não verificado]**. A liberação do B3 continua sendo de Ronald, conforme o anexo. [bin/eval_estado.py:26](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:26) [bin/eval_estado.py:32](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/eval_estado.py:32) [bin/gerar_estado.py:316](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/bin/gerar_estado.py:316) [CLAUDE.md:265](/home/ronald/codex_ws/13f5c2913616985ee1c4a7f13593e116b1ea4644/CLAUDE.md:265) [anexo aval.txt, item 3: “A obra segue PARADA ate eu rearmar o B3”]

## 18/11 12:34 · sessao a709fac15f77 · COMBOIO-DAS-SUITES-1811 · verificador · origem=Code

**Resultado:** as premissas medidas sustentam a devolução dos itens **1 e 2**. A colisão com os avais vigentes e o alcance sobre exportada, dinheiro e `!` não podem ser fechados integralmente com os diffs disponíveis. O anexo prevê devolução por item e continuidade do outro item quando apenas um proceder: “PROCEDE = AVAL-DEVOLVIDO por item; o outro item segue” (*aval.txt*, revisão).

A cópia do HEAD fornecida não contém metadados Git; por isso, **o diff de cada raia contra main está [não verificado]**. As listas de arquivos abaixo são as que constam literalmente de *medidas.md*, não uma substituição por um diff Git.

### 1. Premissas — **PROCEDE: devolver itens 1 e 2**

- **Suite da P2:** o aval diz “a suite da P2 que esta rodando”; a medida diz “NAO esta rodando: terminou FAILED 2” e que sua base não continha `d54160999`. A afirmação do aval não confere (*aval.txt*, item 1; *medidas.md*, parágrafo da suite da P2).
- **F4 e F6F7 sobre P2:** `main+P2+F4` foi medido limpo, mas acrescentar F6F7 deu conflito em `app/chamados/services/tarefa_do_dia.py`; F6F7 também conflita sobre `main+P2`, identificando o conflito com a P2 (*medidas.md*, parágrafo “merge-tree”). O próprio aval prevê excluir a raia em conflito (*aval.txt*, item 1). Assim, a árvore com as três raias não está disponível sem resolver o conflito.
- **Cirineu c4c:** a identificação confere: a raia medida é `raia/cirineu-pouso-115210`, `4ec3e15d0 = main + 843b57e43 c4c` (*medidas.md*, primeira lista).
- **ESPURIA c9:** a condição de entrada após RED e GREEN está declarada no aval; a medida registra RED existente e GREEN não rodado. Portanto, a condição **ainda não está satisfeita** (*aval.txt*, item 2; *medidas.md*, parágrafo “ESPURIA c9”).
- **Deslocamento de `escala/utils.py`:** o arquivo consta da própria Cirineu, e a medida diz que nenhuma de P2, F4, F6F7 ou ESCALA toca esse arquivo ou a catraca. A atribuição do deslocamento à soma das árvores não está demonstrada; **[não verificado]** que a catraca seria o único vermelho ou qual seria sua causa (*aval.txt*, item 2; *medidas.md*, parágrafo “Comboio B”).

### 2. Colisão com aval vigente — **NÃO PROCEDE, com alcance parcial [não verificado]**

O texto do comboio preserva a ordem P2 → F4 → F6F7 e a janela de véspera (*aval.txt*, itens 1 e 3; *ordem_viva_1811_manha_0855.txt*, itens 0 e 3). A decisão de Cirineu mantém a espinha à frente e determina pouso antes das 18h se fechar verde, ou permanência em raia até o fim da simulação; o comboio B conserva o prazo, mas sua posição efetiva de pouso deve respeitar também essa ordem (*cirineu_12h_decidida_1811_1110.txt*; *aval.txt*, item 2). **A execução efetiva dessa ordem está [não verificado].** [nao verificado]

Quanto aos arquivos contra `main`, o pedido de listar o diff completo de cada raia **não pôde ser cumprido**:

| Raia | Arquivos fornecidos no dado raspado |
|---|---|
| P2 | Diff completo **[não verificado]**. |
| F4 | Diff completo **[não verificado]**. |
| F6F7 | `app/chamados/services/tarefa_do_dia.py` aparece como local do conflito; diff completo **[não verificado]**. |
| Cirineu | `core/tests/test_medida_turno_do_par.py`, `escala/tests/test_cirineu_nao_desfaz_o_papel.py`, `escala/utils.py`. |
| ESCALA | `escala/regua_defesa.py`, `escala/services/cadastro_realidade.py`, `escala/urls.py`, `escala/views.py`, `ponto/services/esmeril_espelho.py`, `templates/escala/cadastro_x_realidade.html` e “teste novo”, sem caminho informado. |
| ESPURIA c9 | Diff completo **[não verificado]**. |

Fonte das listas: *medidas.md*, parágrafos “merge-tree” e “Comboio B”. Portanto, **quais raias tocam folha, export, retenção ou DiaPago está [não verificado]**; não há base para declarar uma colisão concreta com a janela nem para descartá-la em todas as raias. A janela é expressa em *ordem_viva_1811_manha_0855.txt*, item 0.

### 3. Condições literais — **PROCEDE: devolver itens 1 e 2**

**a. Árvore do B.** A medida do B foi feita sobre `main+P2+F4`, chegando a `8a60c3c31` com Cirineu e ESCALA; c9 ainda não tinha GREEN (*medidas.md*, parágrafo “Comboio B” e “ESPURIA c9”). Se F6F7 pousar antes de B, a árvore resultante incluirá F6F7 e **não será automaticamente a árvore medida para B**. Se só P2+F4 pousarem antes, a base medida corresponde à espinha descrita, mas a igualdade da árvore final, inclusive com as raias efetivamente incluídas, exige comparação: **[não verificado]**. A regra exige que a árvore certificada seja a que pousa e `cmp` antes do commit ([CLAUDE.md:320](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:320)). “Mesma medida de conflito” não certifica essa igualdade (*aval.txt*, item 2).

**b. F6F7 fora do comboio A.** Para pousar depois de P2 e F4, sua suite precisa certificar a árvore que resultará desse pouso sobre a ponta então vigente, com o conflito resolvido; uma suite da raia isolada, sem P2, não demonstra isso ([CLAUDE.md:320](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:320)). A suite isolada medida usava base sem `d54160999` e sem P2 e terminou FAILED (*medidas.md*, parágrafo “Suite do F6F7 sozinho”). A resolução e a árvore resultante estão **[não verificado]**.

**c. Cancelamento do job.** O job começou às 12:30:09 e foi cancelado às 12:31:56, antes do aval das 12:35 (*medidas.md*, parágrafo do job; *aval.txt*, cabeçalho). O aval posterior proíbe interromper suite em curso, mas o dado não sustenta aplicar essa proibição retroativamente (*aval.txt*, “PROIBIDO”). A pré-aprovação permite cancelar job em fila ou rodando **de volta superada da mesma obra**, com linha no RELATO ([CLAUDE.md:288](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:288)). Que fosse volta superada da mesma obra e que a linha exigida exista está **[não verificado]**; não se pode concluir que o cancelamento estava coberto.

**d. Opus por obra.** O aval exige “cada um com o seu opus final sobre o proprio diff” (*aval.txt*, item 1), e a regra exige suite inteira verde antes do opus final por obra ([CLAUDE.md:372](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:372)). O literal consultado **não fixa** se o comando de diff é contra `main` original ou contra a ponta anterior: **[não verificado]**. Para identificar o que cada obra acrescenta, o diff contra a ponta anterior isola sua contribuição; a revisão também precisa considerar os sítios e leitores/escritores exigidos pela regra, inclusive fora do diff ([CLAUDE.md:379](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:379)). Isso é uma interpretação operacional, não uma base de comparação prescrita literalmente.

### 4. Exportada, dinheiro ou `!` fora do declarado — **[não verificado]; sem base para PROCEDE ou NÃO PROCEDE**

A regra exige `!` para, entre outros casos, tocar o gravado de competência exportada ou dado de escala/vínculo ([CLAUDE.md:265](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/CLAUDE.md:265)). Há autorização expressa para o botão de vínculo pela porta existente com trilha (*ordem_viva_1811_manha_0855.txt*, última linha), e Cirineu não deve regerar célula da competência 11 antes do corte posterior à simulação (*cirineu_12h_decidida_1811_1110.txt*). Sem os diffs completos e sem dado de produção além do raspado, **o alcance real sobre exportada, dinheiro ou outro `!` permanece [não verificado]**.

**Fecho:** 1 **PROCEDE — itens 1 e 2 devolvidos**; 2 **NÃO PROCEDE no texto examinado, com alcance dos diffs [não verificado]**; 3 **PROCEDE — itens 1 e 2 devolvidos**, pelas condições de árvore que precisam fechar; 4 **[não verificado] — não classificável**.

## 18/11 12:16 · sessao d2c13875bd5b · DSTL1-PORTA-HE · sonda · origem=Code

No dado fornecido, não aparece erro nem traceback. `CONCLUSAO_LIBERADA` está presente e `rc_sombra=0`. [não verificado]

Cópia das três linhas solicitadas: [não verificado]

```text
FUNIL_HOJE_CAP0611 {'B_trabalha': 277, 'B_sem_trabalho': 139, 'C_pago>0': 78, 'C_ESTRUTURA_DINHEIRO': 54, 'C_ESTR_2mais': 36, 'C_decisao_he': 4, 'C_porta': 3}
HOJE_ESTR_2mais_MARCADORES {'regen_pos': 4, 'regeneracoes': 12, 'dna_anterior': 12, 'julgada_pos': 9, 'diapago_lavrado_pos': 33, 'sem_marcador': 3, 'colab_desligado': 4} de 36
HOJE_ESTR_POR_DIA {'2026-11-02': 11, '2026-11-05': 1, '2026-10-30': 1, '2026-11-03': 1, '2026-10-22': 4, '2026-10-29': 1, '2026-11-01': 2, '2026-10-21': 2, '2026-10-23': 1, '2026-10-31': 1, '2026-10-24': 2, '2026-10-25': 3, '2026-10-28': 1, '2026-10-27': 1, '2026-11-06': 1, '2026-11-04': 1, '2026-10-26': 2}
```

Contagem das 19 linhas `AMOSTRA` por `razao_hoje`: [não verificado]

| `razao_hoje` | Contagem |
|---|---:|
| `pago_sem_decisao_ou_pago` | 11 |
| `ESTRUTURA_DINHEIRO` | 8 |

Dicionário `marc` de cada linha, agrupado por `razao_hoje`: [não verificado]

| `razao_hoje` | Amostra | `marc` |
|---|---|---|
| `pago_sem_decisao_ou_pago` | `col43` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col207` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col369` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col384` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col789` | `{'regeneracoes': 5, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col822` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col962` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col967` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col991` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col1003` | `{'diapago_lavrado_pos': 1}` |
| `pago_sem_decisao_ou_pago` | `col749` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col97` | `{'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col242` | `{'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col278` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col617` | `{'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col820` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col864` | `{'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col944` | `{'diapago_lavrado_pos': 1}` |
| `ESTRUTURA_DINHEIRO` | `col900` | `{'regen_pos': 1, 'regeneracoes': 1, 'dna_anterior': 1, 'diapago_lavrado_pos': 1}` |

## 18/11 12:15 · sessao 365c53fd8db0 · DSTL1-PORTA-HE · sonda · origem=Code

A saída fornecida não contém erro nem traceback e chegou a `CONCLUSAO_LIBERADA`, seguida de `rc_sombra=0`. [não verificado]

As tabelas abaixo transcrevem ou contam o dado fornecido no enunciado, que não tem arquivo e linha associados. [não verificado]

**I4**

| Contagem | Valor |
|---|---:|
| Linhas com `causaC=True` | 65 |
| Total de linhas | 65 |

| `classe_e6` entre as `causaC` | Quantidade |
|---|---:|
| `bate_ate_10min` | 52 |
| `DIVERGE` | 7 |
| `sem_trabalho_ambos` | 6 |

**DONOS_DOS_DIVERGENTES**

| Dono | Quantidade |
|---|---:|
| `ESTRUTURA` | 2 |
| `BATIDA` | 5 |

**I2 e I2_RELATO**

| Colaborador | Dia | `janela_he_ativa` | `desde` | `piso` | `saida_ativa` | `dia_ge_desde` | `he_fora_espelho` | `motor_cxr_do_dia` | `motor_sem_previsto_fora` | `I2_RELATO` |
|---|---|---|---|---:|---|---|---|---|---|---|
| col741 | 2026-11-15 | True | 2026-08-21 | 0 | True | True | `[]` | `[]` | `[]` | `[] -> []` |
| col899 | 2026-11-01 | True | 2026-08-21 | 0 | True | True | `[]` | `[]` | `[]` | `[] -> []` |
| col944 | 2026-11-02 | True | 2026-08-21 | 0 | True | True | `[]` | `[]` | `[]` | `[]` | `[] -> []` |

**I4SOMA**

| Colaborador | `soma_min_real` | `fech_min_real` |
|---|---:|---:|
| col55 | 8352.0 | 8352 |
| col61 | 5758.0 | 5758 |
| col114 | 9221.0 | 9221 |
| col123 | 9356.0 | 9356 |
| col130 | 8915.0 | 8915 |
| col137 | 5572.0 | 5572 |
| col198 | 8639.0 | 8639 |
| col203 | 8819.0 | 8819 |
| col206 | 8485.0 | 8485 |
| col227 | 9113.0 | 9113 |
| col237 | 9360.0 | 9360 |
| col242 | 6150.0 | 6150 |
| col250 | 5931.0 | 5931 |
| col270 | 2083.0 | 2083 |
| col278 | 8731.0 | 8731 |
| col281 | 8639.0 | 8639 |
| col282 | 8528.0 | 8528 |
| col328 | 6871.0 | 6871 |
| col331 | 6894.0 | 6894 |
| col334 | 7325.0 | 7325 |
| col364 | 8630.0 | 8630 |
| col373 | 1880.0 | 1880 |
| col398 | 9097.0 | 9097 |
| col399 | 9299.0 | 9299 |
| col424 | 9356.0 | 9356 |
| col444 | 8061.0 | 8061 |
| col451 | 5040.0 | 5040 |
| col476 | 9455.0 | 9455 |
| col489 | 8698.0 | 8698 |
| col515 | 9229.0 | 9229 |
| col617 | 6300.0 | 6300 |
| col627 | 8313.0 | 8313 |
| col634 | 8114.0 | 8114 |
| col696 | 0.0 | 0 |
| col741 | 711.0 | 711 |
| col835 | 9217.0 | 9217 |
| col843 | 9234.0 | 9234 |
| col845 | 8580.0 | 8580 |
| col849 | 9232.0 | 9232 |
| col864 | 10056.0 | 10056 |
| col899 | 7081.0 | 7081 |
| col913 | 8889.0 | 8889 |
| col932 | 7763.0 | 7763 |
| col935 | 8580.0 | 8580 |
| col936 | 7597.0 | 7597 |
| col944 | 7730.0 | 7730 |
| col950 | 4427.0 | 4427 |
| col985 | 9452.0 | 9452 |
| col996 | 9271.0 | 9271 |
| col998 | 8074.0 | 8074 |
| col999 | 7692.0 | 7692 |
| col1001 | 6748.0 | 6748 |
| col1012 | 4710.0 | 4710 |
| col1045 | 0.0 | 0 |

Linhas `I4SOMA` com valores numericamente diferentes: **0 de 54**. [não verificado]

## 18/11 12:15 · sessao 24f25bb8b191 · DSTL1-PORTA-HE · leitura · origem=Code

**Resultado principal:** o código distingue o `DiaPago.minutos_realizados` dos minutos da ata e dos minutos calculados pelo pareador. `pago_do_dia` e `rubricas_do_dia` **não leem** esse campo do `DiaPago`; portanto, não é possível inferir o pagamento ou uma pendência de HE apenas a partir de ele ser zero. ([dia_pago.py:859](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:859), [espelho.py:775](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:775))

A lista abaixo registra os leitores e caminhos identificados na inspeção. **A exaustividade de todos os leitores indiretos ou dinâmicos de produção permanece [não verificado].** Não houve consulta a banco nem a logs de produção.

### A. Leitores e origem dos valores

**Produção do campo.** `DiaPago.minutos_realizados` é `FloatField(default=0.0)`, sem `null=True` na definição. `por_dia_da_grade` forma o mapa apenas com dias `trabalho` e `ausencia`, usando `min(realizado ou 0, previsto ou 0)`; assim, `folga` e `feriado` não entram nesse mapa. `fechamento` o passa à lavratura. Isso descreve a construção do mapa, não prova o conteúdo de uma linha concreta. ([models.py:351](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:351), [dia_pago.py:386](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:386), [fechamento.py:1060](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:1060), [fechamento.py:1200](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:1200))

**Leituras efetivas de `DiaPago.minutos_realizados` identificadas:**

| Leitor | Uso de 0 ou nulo em folga/feriado |
|---|---|
| `soma_da_competencia` | Faz `Sum` dinâmico de todos os `CAMPOS`, inclusive minutos realizados e linha de ajuste. Zero contribui zero; agregado nulo vira zero. Sem nenhuma linha, retorna `None`. Não filtra tipo de dia. ([dia_pago.py:34](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:34), [dia_pago.py:550](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:550)) |
| `soma_do_periodo` | Faz `values`/`annotate(Sum(...))` de `CAMPOS_DIA`, agrupando por colaborador. Zero contribui zero e agregado nulo vira zero; a função também devolve conjuntos para distinguir ausência de lavratura e de apuração. Não filtra tipo de dia. ([dia_pago.py:752](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:752), [dia_pago.py:784](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:784)) |
| `folha.porta_export.medir`, função interna `_medir_dia_pago` | Compara cada campo da soma da competência com o homônimo do `FechamentoMensal`; pode registrar divergência e classificá-la conforme exportação e comparação com a grade. Compara o **total**, não decide isoladamente o estado de uma folga. ([porta_export.py:378](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:378), [porta_export.py:451](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:451)) |
| `foto_do_colaborador` / `mudancas` | Fotografa todos os campos concretos das linhas `DiaPago` e compara os dicionários completos. Uma mudança no campo pode aparecer como linha alterada, sem regra especial para folga ou feriado. ([foto_e4b.py:209](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:209), [foto_e4b.py:247](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:247), [foto_e4b.py:289](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:289), [foto_e4b.py:116](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:116)) |

**Quem recebe `DiaPago`, mas não usa esse campo para o valor exibido:**

- `linhas_do_periodo` devolve objetos `DiaPago` por data. `pago_do_dia` testa rubricas de dinheiro e devolve `None` se a linha falta ou se todas essas rubricas são zero; caso contrário, soma horas trabalhadas, folga trabalhada e a parcela de folga-feriado no detalhe. `rubricas_do_dia` lê outras rubricas e, sem linha, devolve zeros com `lavrada=False`. Nenhuma das duas funções consulta `DiaPago.minutos_realizados`. ([dia_pago.py:833](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:833), [dia_pago.py:859](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:859), [dia_pago.py:890](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:890), [dia_pago.py:901](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:901))
- No **espelho**, `montar_dias` entrega `pago_h` e `rubricas` por essas funções. O template omite rubricas zeradas e, sem lavratura, mostra “sem apuração ainda”. O **PDF** lê `pago_h` e rubricas; rubricas zero ficam vazias. O drill do relatório transforma `pago_h=None` em zero interno e exibe vazio se zero. Esses comportamentos dependem das rubricas, não do minuto gravado em `DiaPago`. ([espelho.py:805](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:805), [espelho.html:268](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/templates/ponto/espelho.html:268), [pdf_espelho.py:901](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/pdf_espelho.py:901), [services.py:142](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/services.py:142))
- No **app**, `horas_do_dia` converte `pago_do_dia=None` em `0.0` quando existe linha; sem linha, usa o período do motor e marca a fonte como não lavrada. A testemunha `espelho_x_dia_pago` da folha também usa `pago_do_dia`, convertendo seu `None` em zero para comparar com os períodos do motor. ([espelho.py:120](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:120), [api/views.py:1531](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/api/views.py:1531), [porta_export.py:101](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:101))
- O relatório e o cartão chamam as agregações que incluem o minuto, mas os usos identificados para os valores apresentados selecionam outras chaves de rubricas. O efeito de **todos** os demais consumidores indiretos dessas agregações sobre o minuto está [não verificado]. ([relatorios/views.py:481](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/views.py:481), [cartao_pela_celula.py:141](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/cartao_pela_celula.py:141))

**Homônimos que não são o campo de `DiaPago`:**

- `dia['minutos_realizados']` do espelho vem de `_real_dia.minutos`, calculado por `realizado_do_dia`; pode ser `None`. O E6 usa esse valor primeiro e só recorre a `pago_h` quando ele é `None`. ([espelho.py:675](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:675), [espelho.py:783](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:783), [e6_oraculo.py:263](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:263))
- `CelulaDia.ata['minutos_realizados']`: `referencia_do_espelho` no PDF de conferência converte zero ou `None` em zero e só incrementa dias e minutos quando o valor é verdadeiro. ([validacao_pdf.py:29](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/validacao_pdf.py:29), [validacao_pdf.py:44](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/validacao_pdf.py:44))
- O `dia` da **grade** em `status_dia.fatos_do_status` converte zero ou `None` em zero para `tem_batida`; ainda pode haver `tem_batida=True` por células ou órfãs. `supra_juiz.classificar_dia` também lê o valor da grade com `or 0`. `folha.causas.classificar_causa` soma o realizado dos dias úteis e pode emitir `realizado_zero` conforme as outras guardas. ([status_dia.py:166](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/status_dia.py:166), [status_dia.py:185](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/status_dia.py:185), [supra_juiz.py:217](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/supra_juiz.py:217), [causas.py:28](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/causas.py:28), [causas.py:84](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/causas.py:84))
- `folha.export` lê também **`FechamentoMensal.minutos_realizados`**: zero é usado como zero; `None` faz fallback para `trab`. No fallback pela grade, o cálculo do realizado seleciona dias de trabalho. Isso não é leitura do campo diário `DiaPago`. ([export.py:1076](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/export.py:1076), [export.py:1095](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/export.py:1095))
- `core/juizes.py` registra a autoridade `realizado_do_dia`; esse registro não lê o valor de um `DiaPago`. Leitura do campo por `veredito_celula` ou pelo motor: **[não verificado]**. ([juizes.py:203](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/core/juizes.py:203))

### B. Juiz e rótulo `ESTRUTURA_DINHEIRO`

O predicado exato que **produz `ESTRUTURA_DINHEIRO` está [não verificado]**: a busca pelo literal em `app/` encontrou referências documentais, mas não um produtor executável identificado. A descrição documental não substitui o predicado do código. ([RELATO.md:2674](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:2674))

O que está verificado é o juiz E6, que retorna `CADASTRO`, `BATIDA` ou `ESTRUTURA`. Para retornar **`ESTRUTURA`**, todas estas condições devem valer: os três indicadores cadastrais são falsos; `batida_fora_de_turno` é falso; a quantidade de `horas_do_dia` é par, inclusive zero; `fase_deslocada` é falso; e `em_cadastro_x_realidade` é falso. A função, por si, não exige que haja divergência numérica. ([e6_oraculo.py:53](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:53), [e6_oraculo.py:98](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:98))

Os insumos montados por `InsumosDoDono` são:

- **Horas/paridade:** timestamps de `batidas_apuraveis` na janela ampliada, processados por `minutos_do_oraculo`. ([e6_oraculo.py:149](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:149))
- **Cadastro:** callback cadastral fornecido ou `cadastro_do_dia_carregado`, que fornece os indicadores passados ao juiz. ([e6_oraculo.py:182](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:182), [supra_juiz.py:173](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/supra_juiz.py:173))
- **Batida fora de turno:** `realizado_do_dia(...).aberto`, mais a existência de entrada solta que permaneça apurável; os turnos vêm de `turnos_do_colab`. ([e6_oraculo.py:156](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:156), [e6_oraculo.py:198](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:198))
- **Fase:** dias informados pelo chamador ou lidos por `dias_de_fase_as_of`. **Cadastro x realidade:** conjunto fornecido ou `resultado.dias_cadastro_x_realidade` do espelho/motor. Com `sem_motor=True` e sem esse conjunto, uma resposta que dependeria dele pode ser `None`. ([e6_oraculo.py:128](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:128), [e6_oraculo.py:162](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:162), [e6_oraculo.py:174](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:174), [e6_oraculo.py:189](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:189))

**Nenhum desses insumos é `DiaPago.minutos_realizados`.** Na comparação que antecede a chamada ao dono, o E6 lê primeiro o minuto do **espelho/pareador**; se for `None`, usa `pago_h`, que vem das rubricas descritas acima. Alterar somente o minuto de `DiaPago` não altera diretamente essa entrada nem o juiz. Se o minuto **do espelho** for zero, ele é comparado como zero; se for `None`, há fallback para `pago_h`, e, sem ambos, o dia sai da comparação. ([e6_oraculo.py:242](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:242), [e6_oraculo.py:332](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:332), [espelho.py:783](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:783), [dia_pago.py:890](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:890))

### C. Condições para não aparecer pendência de HE

Ter duas ou mais batidas apuráveis **não basta** para publicar uma pendência: a lista do dia é a união de três vias, e só fica vazia quando nenhuma delas produz item. As condições abaixo são gerais; não atribuem causa a nenhum dia concreto. ([espelho.py:720](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:720))

1. **Cadastro e vigência.** A lista começa vazia e as três vias só são executadas se `janela_he_ativa` for verdadeira, `janela_he_desde` existir e a data for igual ou posterior a ela. Saída desligada impede as pontas de saída, mas não impede por si uma ponta de entrada ou o item de dia inteiro. ([espelho.py:384](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:384), [espelho.py:720](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:720), [janela_he.py:194](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:194))

2. **Vínculo e grade.** Sem vínculo **do dia**, as extras declaradas permanecem zero; o código continua e ainda tenta produzir pendência. Se não houver montador de realizado, `_real_grade` fica vazia, eliminando as duas vias que precisam de células-alvo, mas isso não elimina necessariamente a via de dia inteiro. ([espelho.py:688](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:688), [espelho.py:720](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:720), [janela_he.py:272](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:272))

3. **Ponta diretamente atribuída.** `marcar_pontas_fora` considera a primeira célula de marco tipo `E` e a última tipo `S`; ignora célula `missing`, demais marcos, saída com a opção desligada e desvio no sentido errado ou zero. Da sobra no sentido correto desconta a extra declarada; se a sobra for zero ou não exceder o piso, não produz aquela ponta. Igualdade com o piso fica dentro da janela. ([janela_he.py:51](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:51), [janela_he.py:73](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:73), [janela_he.py:194](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:194))

4. **Recuperação de ponta pelo relato.** `missing` sozinho **não elimina a pendência**: `pontas_do_relato` pode recuperá-la. Essa via não acrescenta item se faltar relato do mesmo dia com causa de janela e sentido reconhecido, se a saída estiver desligada, se o sentido já tiver sido marcado, se não existir célula-alvo `missing`, se o marco não coincidir ou se os minutos arredondados não forem positivos. A ausência nessa via tampouco elimina as outras duas. ([janela_he.py:267](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:267), [janela_he.py:293](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:293), [janela_he.py:311](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/janela_he.py:311))

5. **Dia inteiro sem previsto.** A terceira via traduz itens do motor com a mesma data e minutos verdadeiros. O motor deixa de produzir esse item, entre outras guardas, se faltar entrada ou minutos trabalhados, se não for folga nem feriado, se a empresa não estiver na janela, se o previsto não for exatamente zero, se houver autorização, ou, para folga com colaborador conhecido, se `escala_certa_no_dia` for falsa. Essas guardas são específicas dessa via. ([espelho.py:743](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:743), [motor_calculo_v2.py:1440](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1440), [motor_calculo_v2.py:1465](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1465))

6. **Apuração e competência.** `apurar` percorre apenas colaboradores da empresa com `situacao='ativo'`; erro ao obter o espelho pula o colaborador e incrementa erros. Para os dias recebidos, rejeita data ilegível ou fora da janela da competência e pula lista `he_fora_da_janela` vazia. Uma decisão existente muda o estado e a contagem de “sem decisão”, mas não remove automaticamente o dia que já tem pontas da fotografia. ([he_pendente_lavrado.py:89](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:89), [he_pendente_lavrado.py:114](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:114), [he_pendente_lavrado.py:132](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:132))

7. **Publicação e visualização.** A Gestão lê uma fotografia que pode não existir; a leitura devolve também quando foi calculada. O cron mantém a competência atual e a anterior enquanto esta não estiver entregue. Na Gestão, busca, praça, posto, estado e dono podem ocultar uma linha ou dia já fotografado; isso é filtro de exibição, não prova de que a pendência não foi lavrada. ([he_pendente_lavrado.py:193](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:193), [he_pendente_lavrado.py:209](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:209), [gestao_he.py:236](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:236), [gestao_he.py:317](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:317))

## 18/11 12:13 · sessao 807b8f662b4b · - · - · origem=Ronald (tmux)

A busca em `app/` encontrou o literal `ESTRUTURA_DINHEIRO` em documentação, mas não em um produtor executável. O predicado exato do rótulo composto está **[não verificado]**. A documentação o descreve como dias sem pago, decisão de HE, porta ou verbo de administrador, mas isso não substitui o predicado da sonda que os classificou ([app/docs/RELATO.md:2674](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:2674)).

O juiz E6 responde outra pergunta e retorna apenas `CADASTRO`, `BATIDA` ou `ESTRUTURA`. Ele retorna `ESTRUTURA` quando os três indicadores de cadastro são falsos, `batida_fora_de_turno` é falso, a quantidade de `horas_do_dia` é par, `fase_deslocada` é falso e o dia não está em cadastro versus realidade; zero batidas também é par ([e6_oraculo.py:98](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:98)). Com `sem_motor=True`, sem conjunto CXR fornecido, o resultado que dependeria dessa lista fica `None` ([e6_oraculo.py:189](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:189)).

Os insumos de `InsumosDoDono` são:

- `horas`: timestamps das batidas apuráveis na janela ampliada, agrupados pelo oráculo; batidas retratadas são excluídas ([e6_oraculo.py:149](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:149), [nucleo.py:230](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/calculador/nucleo.py:230), [turnos.py:2868](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:2868)).
- `batida_fora_de_turno`: o estado `aberto` de `realizado_do_dia` e a existência de entrada solta apurável, usando turnos carregados para a janela ampliada ([e6_oraculo.py:156](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:156), [e6_oraculo.py:198](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:198)).
- Indicadores cadastrais: vínculo do dia e defeito de folga; na ausência de vínculo, célula e data de demissão alimentam o indicador específico de dia de trabalho ([supra_juiz.py:179](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/supra_juiz.py:179), [supra_juiz.py:206](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/supra_juiz.py:206)).
- `fase_deslocada`: dias da lavra mais recente que cobre cada data, em `itens` e `itens_reabertos` ([e6_oraculo.py:174](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:174), [fase_12x36.py:117](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/services/fase_12x36.py:117)).
- Cadastro versus realidade: conjunto fornecido ao carregador ou extraído de `resultado.dias_cadastro_x_realidade` do motor no espelho ([e6_oraculo.py:142](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:142), [e6_oraculo.py:162](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:162), [motor_calculo_v2.py:2719](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:2719)).

Nenhum desses insumos lê diretamente `DiaPago.minutos_realizados`. A comparação feita pelo comando E6 antes de perguntar o dono lê o homônimo do espelho, proveniente de `_real_dia.minutos` do pareador; se ele for `None`, usa `pago_h`, quando disponível ([espelho.py:777](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:777), [e6_oraculo.py:263](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/e6_oraculo.py:263)). `pago_do_dia` usa rubricas monetárias e detalhe, não esse campo de minutos ([dia_pago.py:859](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:859), [dia_pago.py:890](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:890)). Assim, alterar apenas `DiaPago.minutos_realizados` para `0` ou `None` não muda diretamente esses insumos nem esse fallback; o efeito sobre o rótulo composto da sonda permanece **[não verificado]**.

## 18/11 12:13 · sessao dc1dc864d1a9 · - · - · origem=Ronald (tmux)

Investigação da pergunta A enviada ao agente principal, com as referências de código e a distinção entre `DiaPago.minutos_realizados`, o valor da ata e o valor do espelho.

## 18/11 12:13 · sessao 0720f43be677 · - · - · origem=Ronald (tmux)

Investigação da pergunta C concluída e enviada ao agente principal, com as condições de cada uma das três vias de publicação e os filtros posteriores da Gestão de HE.

## 18/11 12:12 · sessao a39022e4c520 · F5-DSTL1 · sonda · origem=Code

**Não foi possível conferir a implementação da sonda:** o texto fornecido contém sua saída, mas `sonda_matriz.py` não foi localizado na cópia. Assim, a origem exata de cada campo, o hash informado da sonda e a existência de regra reimplementada nela ficam **[não verificado]**. Abaixo, distingo os valores observados no dado raspado das regras das funções reais disponíveis na cópia.

A distinção central é que a tarefa julga o **dia do colaborador**, enquanto a âncora e seu verbo descrevem um **chamado**. A tarefa reúne ações das instâncias e possíveis portas do dia para escolher um fio e uma rota; o verbo isolado de uma âncora não determina necessariamente essa escolha. [tarefa_do_dia.py:633](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:633)

- **`razao`**: o dado traz valores como `fora_vinculo`, `instancia_aceso`, `motivo_fora_do_mapa` e `pos_fora_das_instancias(trabalho)`; a fórmula que produz esses rótulos na sonda é **[não verificado]**. A tarefa real possui uma razão do dia e razões por instância: por exemplo, retorna `fora_vinculo` no nível do dia, e usa `aceso`, `dia_sem_trabalho` ou razões de ausência nas instâncias. Esses rótulos explicam o julgamento da tarefa; por si só, não estabelecem que a pergunta específica tem uma ação associada. [tarefa_do_dia.py:208](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:208), [tarefa_do_dia.py:282](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:282)

- **`dia_fio`**: no formato da tarefa real, `fio` é quem age a seguir no dia. Há `colab` quando uma instância associa `responder/colaborador` a uma pergunta pendente no app; ações administrativas alcançáveis e ações de sistema também entram na escolha, assim como certos verbos residuais. A precedência é `colab`, `admin`, `chamado`, `sistema`; sem esses donos, resulta `nao_sei` ou `nada`. `cobrar`, `historico`, `arquivado` e verbo nulo não dão dono como residual nessa função. Que `dia_fio` tenha sido copiado diretamente desse retorno é **[não verificado]**. [tarefa_do_dia.py:657](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:657)

- **`dia_rota`**: a tarefa publica `porta_app` para `colab`; para outros donos com portas vivas, escolhe `porta_fila` ou `porta_pergunta` conforme a ligação dos chamados ao dia; admin sem chamado vivo pode receber `porta_acao`. `nada` e `nao_sei` não têm rota. Se o calendário oferecer `resolver_dia` quando a rota é nula, a tarefa pode acrescentar essa ação e publicar `admin/porta_acao`. A extração pela sonda é **[não verificado]**. [tarefa_do_dia.py:678](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:678), [tarefa_do_dia.py:609](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:609)

- **`dia_acao`**: na tarefa, o nome publicado é a primeira ação administrativa alcançável quando o fio final é `admin`; se esse fio vier apenas de verbo residual, o nome vem da mesma escada de verbos. Para outro fio, o campo é `None`. Logo, `None` com `colab/porta_app` não significa que não há `responder` nas ações das instâncias: significa que esse campo resumido não nomeia uma ação administrativa. A extração pela sonda é **[não verificado]**. [tarefa_do_dia.py:704](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:704)

- **`ancora_na_fila`**: a âncora real é o `chamado_destino` da pergunta ou, na falta dele, o chamado da disputa. A fila real parte dos chamados vivos, filtra os estados de cobrança e exclui os silenciados até depois de `agora`. O predicado exato usado para preencher o booleano da sonda é **[não verificado]**. [fio.py:97](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/fio.py:97), [painel.py:158](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/painel.py:158)

- **`ancora_verbo`**: a partição real calcula um verbo por chamado a partir de sua origem e dos créditos de perguntas; quando há `silenciado_ate`, pode classificá-lo como `arquivado` ou `historico`. Esse verbo descreve o chamado, não necessariamente a ação da pergunta ou o dono escolhido para todo o dia. O modo pelo qual a sonda o obteve, inclusive quando a âncora está fora da fila, é **[não verificado]**. [verbos.py:30](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/verbos.py:30)

- **`fio_do_verbo`**: a função real `fio_pelos_verbos` usa a escada `cobrar → colab`, `validar → admin`, `decidir → admin`, nessa ordem; se nenhum verbo da escada aparecer, retorna `chamado`. A opção `preservar_porta` altera o tratamento de `decidir`. Porém, a saída raspada mostra `None` para `historico` e `arquivado`, portanto **não se pode afirmar que esse campo seja simplesmente o retorno direto da função para o verbo isolado**; o filtro ou a lógica usados pela sonda são **[não verificado]**. [juizes.py:1685](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/juizes.py:1685), [juizes.py:1690](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/juizes.py:1690)

O próprio dado ilustra por que as perspectivas não devem ser confundidas: na PK **33121**, o dia aparece como `admin/porta_acao/resolver_dia`, enquanto a âncora aparece como `cobrar` e `fio_do_verbo=colab`; na PK **58325**, o dia aparece como `colab/porta_app`, enquanto a âncora aparece como `validar/admin`. Isso é uma observação das linhas fornecidas; a causa factual específica de cada divergência é **[não verificado]**.

Também nas linhas fornecidas, `acoes_da_pergunta` está vazio, mas `dia_acoes` pode conter ações de outras instâncias do mesmo dia — inclusive `responder`, `decidir_ausencia` ou `resolver_dia`. A tarefa real atribui ações por instância e pode escolher uma pergunta por posição; portanto, uma ação no dia não demonstra, sozinha, que ela esteja associada à PK da linha. [tarefa_do_dia.py:319](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:319)

**Conclusão sobre reimplementação:** há evidência de que `fio_do_verbo` **não equivale simplesmente** à chamada direta de `fio_pelos_verbos` sobre qualquer verbo observado, mas isso não prova reimplementação: pode haver filtro anterior ou posterior. Sem o código da sonda, não é possível apontar com segurança qual campo reimplementa uma regra, nem confirmar que os demais leem as funções reais **[não verificado]**.

## 18/11 12:11 · sessao cdd0105b9bb0 · DSTL1-PORTA-HE · verificador · origem=Code

**Resultado: o item 1 não fecha no literal para todo o universo.** A ausência de decisão vigente é uma leitura existente, e a autorização de HE também existe, mas há um terceiro estado, `nao`, e a porta exige minutos positivos fornecidos pelo chamador. Além disso, executar uma decisão de HE pode acionar recálculo e alterar o tratamento de dinheiro; isso é diferente de apenas registrar o aval, que o texto fornecido descreve como a ação imediata. [app/ponto/models.py:1265](app/ponto/models.py:1265), [app/ponto/models.py:1283](app/ponto/models.py:1283), [app/ponto/portas/he.py:159](app/ponto/portas/he.py:159), [app/ponto/portas/he.py:186](app/ponto/portas/he.py:186), [app/ponto/portas/he.py:229](app/ponto/portas/he.py:229); aval fornecido, item 1 [sem arquivo:linha verificado].

A identidade da cópia com o hash informado é **[não verificado]**: `git rev-parse HEAD` não retornou um hash. As referências abaixo são das linhas da cópia disponível.

### 1. Premissas e unidade — **NÃO PROCEDE integralmente**

- `vigentes()` é exatamente o filtro `revogada_em__isnull=True`; a linha revogada permanece acessível pelo manager sem filtro. Portanto, “nenhuma” significa ausência de **decisão vigente**, não necessariamente ausência histórica de linha. [app/ponto/models.py:1251](app/ponto/models.py:1251), [app/ponto/models.py:1265](app/ponto/models.py:1265).
- A assinatura é `decidir_he(colaborador, data, estado, *, minutos, autor, motivo='', request=None)`. Aceita somente `autorizado` ou `nao`, exige `autorizar_he`, data do tipo `date`, motivo de pelo menos 10 caracteres para autorizar e `int(minutos) > 0`. Não recalcula nem confirma os minutos: recebe-os de quem chama. A função não contém guarda específica de folga ou de situação ativa do colaborador. [app/ponto/portas/he.py:37](app/ponto/portas/he.py:37), [app/ponto/portas/he.py:124](app/ponto/portas/he.py:124), [app/ponto/portas/he.py:159](app/ponto/portas/he.py:159), [app/ponto/portas/he.py:175](app/ponto/portas/he.py:175).
- “Autorizada” existe como `DecisaoHE.estado == 'autorizado'`; a alternativa é `estado == 'nao'`. `_gravar` cria ou atualiza estado, foto dos minutos, motivo, autor e origem, reativando eventual linha revogada; a trilha é **best effort**, pois falha de auditoria não desfaz a decisão. Mesma decisão vigente retorna `no_op`. [app/ponto/models.py:1283](app/ponto/models.py:1283), [app/ponto/portas/he.py:61](app/ponto/portas/he.py:61), [app/ponto/portas/he.py:89](app/ponto/portas/he.py:89), [app/ponto/portas/he.py:195](app/ponto/portas/he.py:195).
- A view recebe POST autenticado e chama a porta com o usuário; recusa em lote e autorização em lote também chamam `decidir_he`. A permissão é conferida na própria porta. Existe ainda recusa de ponta pequena pelo sistema, que usa o mesmo `_gravar`, com origem `sistema`: logo, `decidir_he` não é o único **chamador** que produz decisões de HE, embora `_gravar` centralize a escrita do estado. [app/ponto/views.py:498](app/ponto/views.py:498), [app/ponto/portas/he.py:260](app/ponto/portas/he.py:260), [app/ponto/portas/he.py:595](app/ponto/portas/he.py:595), [app/ponto/portas/he.py:369](app/ponto/portas/he.py:369).
- Há uma única linha por `(colaborador, data)`, com `data` fornecida pelo chamador; o modelo e a porta não impõem que seja `data_turno`. No caminho L-097 examinado, o motor consulta pela `data_jornada`, obtida do mapa do juiz de turno, com fallback para data civil e herança do período anterior em certas condições. Isso **favorece o encaixe**, mas não prova que toda data entregue pela tela/porta coincida com a unidade do aval em todas as bordas. [app/ponto/models.py:1300](app/ponto/models.py:1300), [app/ponto/models.py:1354](app/ponto/models.py:1354), [app/ponto/views.py:526](app/ponto/views.py:526), [app/ponto/motor_calculo_v2.py:2399](app/ponto/motor_calculo_v2.py:2399), [app/ponto/motor_calculo_v2.py:2461](app/ponto/motor_calculo_v2.py:2461). Casamento universal: **[não verificado]**.

### 2. Colisão com o corte e L-097 — **NÃO PROCEDE afirmar colisão textual**

O corte DST-L1-1800 diz que entrada real em dia sem turno vai à **porta de decisão do admin**, mas não nomeia a porta de HE nem determina outra porta concreta. O registro da revisão anterior identifica justamente como lacuna qual objeto observável seria essa porta. Assim, o texto disponível não prova que o corte exigia outra porta; tampouco prova que a porta de HE já cumpria integralmente o alcance do corte. [app/docs/CORTES.md:532](app/docs/CORTES.md:532), [app/docs/RELATO.md:2680](app/docs/RELATO.md:2680).

A L-097 consulta o mapa de decisões vigentes e, no ramo aplicável de dia sem previsto, só sai do bloqueio por decisão quando o estado é `autorizado`. O próprio ramo tem condições e exceções, inclusive empresa na janela, previsto zero e folga com escala certa ou feriado; não se deve generalizar o bloqueio para toda célula sem trabalho. A escolha de HE é compatível com esse mecanismo **nos casos que ele alcança**, mas sua cobertura integral da porta DST-L1 não está demonstrada. [app/ponto/motor_calculo_v2.py:1397](app/ponto/motor_calculo_v2.py:1397), [app/ponto/motor_calculo_v2.py:1422](app/ponto/motor_calculo_v2.py:1422), [app/ponto/motor_calculo_v2.py:1440](app/ponto/motor_calculo_v2.py:1440), [app/ponto/motor_calculo_v2.py:1455](app/ponto/motor_calculo_v2.py:1455).

### 3. Cobertura literal — **NÃO PROCEDE**

**A e B não formam uma partição exaustiva.** Uma decisão vigente `nao` não é “nenhuma”, porque existe decisão vigente, nem é B, porque não é `autorizado`. O modelo explicita que essa recusa é decisão, e a trava do export também trata `nao` como decisão. Se a recusa estiver revogada, aí sim a consulta por vigentes a exclui. [app/ponto/models.py:1283](app/ponto/models.py:1283), [app/folha/porta_export.py:218](app/folha/porta_export.py:218), [app/ponto/models.py:1265](app/ponto/models.py:1265).

Nos **65 dias da medida fornecida**, o anexo relata 55 servidos, sete com minutos de HE zero e três sem pendência publicada; relata também que quatro dos 55 são de desligados. Esses números são o dado raspado fornecido, sem os arquivos brutos disponíveis para conferência nesta revisão **[não verificado em arquivo:linha]**. O código confirma os limites que importam: [nao verificado]

- Com `minutos=0`, a porta recusa tanto autorização como recusa; portanto, esses dias podem continuar em A, mas A não registra uma decisão humana sobre o trabalho. Um turno sem fim fechado pode contribuir zero ao realizado do pareador. [app/ponto/portas/he.py:186](app/ponto/portas/he.py:186), [app/ponto/turnos.py:609](app/ponto/turnos.py:609), [app/ponto/turnos.py:733](app/ponto/turnos.py:733).
- Para os três sem pendência publicada, a porta não exige consulta à pendência: exige um número positivo fornecido pelo chamador. Com o zero publicado indicado na medida, ela recusa; que exista outro número positivo legítimo e um caminho de tela que o forneça para esses casos é **[não verificado]**. A causa da não publicação também é **[não verificado]**. [app/ponto/portas/he.py:164](app/ponto/portas/he.py:164), [app/ponto/portas/he.py:186](app/ponto/portas/he.py:186).
- O apurador da lista percorre somente colaboradores ativos e somente dias com `he_fora_da_janela`; a porta individual não contém essa filtragem. Assim, aceitar uma chamada não demonstra que o administrador consiga alcançar aquele dia pela lista existente. [app/ponto/services/he_pendente_lavrado.py:92](app/ponto/services/he_pendente_lavrado.py:92), [app/ponto/services/he_pendente_lavrado.py:132](app/ponto/services/he_pendente_lavrado.py:132), [app/ponto/portas/he.py:175](app/ponto/portas/he.py:175).

`editar_dia` altera a célula e registra trilha; `retratar` atua sobre a batida e a retira da apuração. **A ausência de DecisaoHE, isoladamente, não distingue esses atos de “ninguém decidiu”**: para distingui-los, o leitor precisa consultar a célula, a batida ou suas trilhas. Que uma tela proposta fará essa composição é **[não verificado]**. A edição ou retratação também pode mudar se o caso ainda pertence ao universo, portanto não se pode presumir que permaneça nele após o ato. [app/ponto/portas/celula.py:572](app/ponto/portas/celula.py:572), [app/ponto/services/retratar_humano.py:150](app/ponto/services/retratar_humano.py:150), [app/ponto/services/gestao_he.py:86](app/ponto/services/gestao_he.py:86).

### 4. Exportada, dinheiro, motor e DiaPago — **PROCEDE como alerta de efeito**

O **registro documental** pedido pelo aval não executa a porta nem altera esses dados; nenhuma execução foi solicitada ou realizada nesta revisão. O uso posterior de `decidir_he`, porém, grava a decisão e chama `recalcular_por_evento` para a data, salvo `no_op`; o recálculo é agendado após o commit. O código descreve recálculo inclusive em competência exportada, sujeito à guarda específica. O efeito concreto em uma competência ou pessoa é **[não verificado]**. [app/ponto/portas/he.py:195](app/ponto/portas/he.py:195), [app/ponto/portas/he.py:210](app/ponto/portas/he.py:210), [app/ponto/portas/he.py:229](app/ponto/portas/he.py:229), [app/ponto/services/fechamento.py:1798](app/ponto/services/fechamento.py:1798).

O motor lê a autorização e pode deixar de bloquear minutos no ramo L-097; a porta de export consulta o estado para dias pendentes acima do limite, e a configuração da empresa determina se essa contagem integra as falhas que travam o export. Portanto, não é correto declarar o **uso** da porta sem alcance sobre dinheiro, motor ou trava. A existência de um `!` concreto em dado exportado e seu valor são **[não verificado]**. [app/ponto/motor_calculo_v2.py:1455](app/ponto/motor_calculo_v2.py:1455), [app/folha/porta_export.py:680](app/folha/porta_export.py:680), [app/folha/porta_export.py:687](app/folha/porta_export.py:687).

### Campos: escritores e leitores identificados

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `revogada_em` | `_marcar_revogada` marca; `_gravar` limpa ao reviver a linha. [app/ponto/portas/he.py:401](app/ponto/portas/he.py:401), [app/ponto/portas/he.py:83](app/ponto/portas/he.py:83) | `vigentes()` julga pelo nulo; `_vigente`, Gestão HE, motor e lavrador usam esse juiz. A própria porta também lê o valor para trilha. [app/ponto/models.py:1265](app/ponto/models.py:1265), [app/ponto/portas/he.py:130](app/ponto/portas/he.py:130), [app/ponto/services/gestao_he.py:86](app/ponto/services/gestao_he.py:86), [app/ponto/motor_calculo_v2.py:1416](app/ponto/motor_calculo_v2.py:1416), [app/ponto/management/commands/lavrar_he_pendente.py:48](app/ponto/management/commands/lavrar_he_pendente.py:48), [app/ponto/portas/he.py:61](app/ponto/portas/he.py:61) |
| `estado` | `_gravar`, chamado pela decisão do admin e pela recusa do sistema. [app/ponto/portas/he.py:66](app/ponto/portas/he.py:66), [app/ponto/portas/he.py:75](app/ponto/portas/he.py:75), [app/ponto/portas/he.py:206](app/ponto/portas/he.py:206), [app/ponto/portas/he.py:369](app/ponto/portas/he.py:369) | Porta para idempotência e revogação; Gestão HE e apurador de pendências; motor; export indiretamente pelo mapa da Gestão HE. [app/ponto/portas/he.py:197](app/ponto/portas/he.py:197), [app/ponto/portas/he.py:488](app/ponto/portas/he.py:488), [app/ponto/services/gestao_he.py:86](app/ponto/services/gestao_he.py:86), [app/ponto/services/he_pendente_lavrado.py:145](app/ponto/services/he_pendente_lavrado.py:145), [app/ponto/motor_calculo_v2.py:1416](app/ponto/motor_calculo_v2.py:1416), [app/folha/porta_export.py:682](app/folha/porta_export.py:682) |
| `data` | `_gravar` na criação, a partir do argumento; a atualização mantém a chave existente. [app/ponto/portas/he.py:65](app/ponto/portas/he.py:65), [app/ponto/portas/he.py:75](app/ponto/portas/he.py:75) | Porta localiza a linha; Gestão HE e lavrador formam mapas por pessoa e data; motor forma mapa por data. [app/ponto/portas/he.py:195](app/ponto/portas/he.py:195), [app/ponto/services/gestao_he.py:86](app/ponto/services/gestao_he.py:86), [app/ponto/management/commands/lavrar_he_pendente.py:48](app/ponto/management/commands/lavrar_he_pendente.py:48), [app/ponto/motor_calculo_v2.py:1416](app/ponto/motor_calculo_v2.py:1416) |
| `minutos_na_decisao` | `_gravar`, com minutos fornecidos pelo chamador; é foto, não pendência viva. [app/ponto/portas/he.py:164](app/ponto/portas/he.py:164), [app/ponto/portas/he.py:66](app/ponto/portas/he.py:66), [app/ponto/portas/he.py:75](app/ponto/portas/he.py:75) | Porta para retorno, trilha e revogação; Gestão HE soma a foto por estado. O mapa do motor examinado contém `data` e `estado`, não essa foto. [app/ponto/portas/he.py:61](app/ponto/portas/he.py:61), [app/ponto/portas/he.py:197](app/ponto/portas/he.py:197), [app/ponto/portas/he.py:497](app/ponto/portas/he.py:497), [app/ponto/services/gestao_he.py:92](app/ponto/services/gestao_he.py:92), [app/ponto/motor_calculo_v2.py:1416](app/ponto/motor_calculo_v2.py:1416) |

A tabela identifica os caminhos relevantes encontrados na cópia; a exaustividade de todos os leitores indiretos e administrativos é **[não verificado]**.

## 18/11 12:10 · sessao 43d86bf18c68 · F5-DSTL1 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 2: PROCEDE nas perguntas 1 e 3.**

1. Para uma pergunta sem `pergunta_id` em nenhuma instância da tarefa, qual valor deve entrar no par conservado: um marcador explícito de “sem associação”, o resultado inteiro da tarefa do dia, ou outro valor? E qual é exatamente o universo de perguntas a comparar?

2. Para a lista anterior ao código, qual predicado define cada letra, sobretudo “RESPOSTA vazia”: último ato `RESPOSTA` com `ato_texto` vazio, projeção `resposta_colab` vazia, ou ambos? Como tratar sobreposição entre letras e efeitos indiretos sobre outras perguntas do mesmo dia?

A cópia não contém metadados Git acessíveis: `git rev-parse HEAD` falhou. Portanto, o hash informado como HEAD é **[não verificado]**. As referências abaixo são da cópia disponível. Os anexos raspados foram fornecidos no pedido sem arquivo e numeração de linhas verificáveis; seus números são identificados como **[não verificado]**.

1. **Premissas — PROCEDE.** A pendência é calculável por pergunta **dentro de um universo definido**: `consulta_do_app` primeiro seleciona perguntas vivas e depois `particao_do_app` as divide; uma viva está pendente se o juiz disser que não foi respondida e a disputa estiver aberta. Para uma pergunta fora da seleção de vivas, é necessário definir se o valor booleano significa “não pertence à lista de pendentes” ou se ela fica fora do universo da comparação. ([pergunta_no_app.py:45](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:45), [pergunta_no_app.py:64](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:64), [pergunta_no_app.py:76](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:76))

   A outra metade **não está definida por pergunta para todas elas**. `tarefa()` devolve um dicionário por colaborador e dia, com instâncias; a associação exige encontrar uma instância cujo `pergunta_id` seja o pk. `fora_vinculo` retorna sem instâncias. Motivo fora do mapa ou posição fora da grade pode passar por `expor_admin`, mas só é acrescentado quando há exposição efetiva; uma ação de colaborador, por si, não é acrescentada por essa via. Chamar `_pela_decisao` diretamente produz um resultado hipotético para a pergunta, mas não prova que a tarefa publicou essa razão e essas ações para ela. ([tarefa_do_dia.py:208](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:208), [tarefa_do_dia.py:267](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:267), [tarefa_do_dia.py:332](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:332), [tarefa_do_dia.py:342](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:342))

   O anexo relata, por exemplo, 94 casos `fora_vinculo`, 92 com motivo fora do mapa e outros com posição fora das instâncias; isso demonstra a relevância da pergunta, mas as contagens e os casos raspados são **[não verificado]**. O código aceita `agora` explícito, corta decisões por esse instante e exige que a carga tenha o mesmo relógio. Isso não garante repetição apenas com **HOJE** fixo: é preciso fixar o instante completo, os dados e a janela da carga; a estabilidade de todas as dependências é **[não verificado]**. ([tarefa_do_dia.py:417](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:417), [tarefa_do_dia.py:571](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:571), [tarefa_do_dia.py:739](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:739))

2. **Colisão com aval ou lei — NÃO PROCEDE**, com limite de verificação. A L-130 define respondida como instante **e** texto não vazio; o juiz e a partição atuais aplicam essa regra. Assim, retirar o RED universal “toda pendente tem RESPONDER” não revoga, por si, a regra de pendência da L-130. ([LEIS.md:151](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/LEIS.md:151), [juizes.py:1544](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/juizes.py:1544), [pergunta_no_app.py:76](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:76))

   O texto antigo em `CORTES.md` dizia que resposta vazia ficaria fora do app e que o painel diria admin; o `RELATO` registra resposta posterior alcançando também essa frase, e um teste do HEAD já espera pendência no app. A compatibilidade da **retirada do RED universal** com essa obrigação específica depende de não interpretá-la como revogação da lei de resposta vazia. ([CORTES.md:509](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/CORTES.md:509), [RELATO.md:37](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:37), [test_o261e_f5_alcance.py:192](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/tests/test_o261e_f5_alcance.py:192))

   `F5-L1-VALE-L130-1811` aparece identificado como vigente no índice do relato, mas seu literal completo e `F5-DSTL1-RESPOSTAS-1811` não foram localizados na cópia consultada: confronto literal integral **[não verificado]**. A própria L-130 registra `RESSUSCITAR-DEVOLVE-AO-COLABORADOR` como lei não decidida; se a letra (c) pretender decidi-la, a colisão exige resposta do dono **[não verificado quanto ao alcance pretendido]**. ([RELATO.md:9001](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:9001), [LEIS.md:151](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/LEIS.md:151))

3. **Literal, lista e mutante — PROCEDE.** Há insumos existentes para listar candidatos antes da F5, mas as letras ainda não definem sem ambiguidade a **lista de exceções do par**:

   - **(a)** Candidatos: perguntas do universo escolhido cujo último ato elegível seja `SILENCIO`; para saber quais efetivamente saem de pendentes, intersectar com os pendentes atuais. A carga ordena por `ato_em, pk`, exclui atos marcados de ressuscitação e corta em `agora`. O relato registra 11 mudanças previstas para histórico; a lista nominal não está no dado fornecido. ([tarefa_do_dia.py:417](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:417), [RELATO.md:33](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:33))
   - **(b)** `_pela_decisao` identifica último ato `RESPOSTA` com `ato_texto` vazio; o juiz L-130 examina, porém, a **projeção** `respondida_em` e `resposta_colab`. Não são o mesmo predicado: o passivo sem linha é detectado separadamente e tem precedência na tarefa. O texto do aval não fixa qual dos dois define a letra nem se a lista inclui casos já pendentes. ([tarefa_do_dia.py:154](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:154), [juizes.py:1544](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/juizes.py:1544), [models.py:4122](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:4122))
   - **(c)** Candidatos: último ato elegível `ABSORCAO` e status da ausência ligada `rejeitada`; `_pela_decisao` devolve `absorcao_rejeitada` e RESPONDER. O relato registra zero mudanças previstas naquele levantamento, não uma prova de zero para outro instante ou universo. ([tarefa_do_dia.py:178](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:178), [RELATO.md:33](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:33))

   Se as três letras forem definidas exclusivamente por **um mesmo último ato**, são mutuamente exclusivas, pois os atos são valores distintos. Se (b) usar a projeção de texto vazio, pode sobrepor-se a (a) ou (c); a ausência ou presença de casos concretos sobrepostos é **[não verificado]**. ([models.py:4037](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:4037), [tarefa_do_dia.py:158](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:158))

   Há **caminho para efeito indireto**: a carga lê a partição em `_pendentes_app`; `_portas_da_tarefa` usa o conjunto para escolher dono e rota do dia; se não houver rota, a oferta de `resolver_dia` pode acrescentar ação à primeira instância. Logo, mudar a pendência de uma pergunta pode alterar a ação publicada em outra instância do mesmo dia, sob as condições desses ramos. A ocorrência e os pks afetados na sombra são **[não verificado]**. ([tarefa_do_dia.py:535](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:535), [tarefa_do_dia.py:657](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:657), [tarefa_do_dia.py:690](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:690))

   O **mutante é escrevível conceitualmente**: alterar em `particao_do_app` a classificação de uma pergunta viva de uma razão escolhida fora da lista, mantendo a referência anterior e os dados fixos. Um comparador que inclua esse pk deve detectar a mudança do booleano; para testar o efeito indireto, é preciso um caso que percorra o ramo de `resolver_dia`. A existência de um caso adequado e o mutante realmente morder na sombra são **[não verificado]**. Também é preciso definir “razão não listada” para perguntas sem associação à tarefa. ([pergunta_no_app.py:76](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:76), [tarefa_do_dia.py:690](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:690))

4. **Exportada, dinheiro, motor, fechamento, DiaPago ou `!` — NÃO PROCEDE** para o item 2 como medida de leitura. A tarefa consulta competência entregue e usa o resultado para limitar exposições administrativas; isso é leitura de exportada, não evidência de escrita nela. O item proposto compara saídas, mas o comportamento de uma futura implementação da F5 nesses domínios é **[não verificado]**. A L-130 reserva os 141 casos de competência entregue para `!` próprio; a conservação não autoriza curá-los. ([tarefa_do_dia.py:473](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:473), [tarefa_do_dia.py:267](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:267), [LEIS.md:151](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/LEIS.md:151))

**Quem escreve e quem lê.** Os endereços abaixo identificam as portas e leitores relevantes para esta medida; um inventário exaustivo de todos os escritores e leitores do repositório é **[não verificado]**.

| Valor | Quem escreve ou produz | Quem lê |
|---|---|---|
| Pendentes do app | É resultado calculado em memória por `particao_do_app`; seus insumos incluem resposta e fechamento da disputa. A resposta humana passa por `responder_pergunta_disputa_core`; o fechamento atribui `fechada_em`. ([pergunta_no_app.py:76](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:76), [disputa_emissao.py:1481](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/disputa_emissao.py:1481), [models.py:2021](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2021)) | A projeção do app, a carga da tarefa e o contador do painel. ([pergunta_no_app.py:101](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:101), [tarefa_do_dia.py:535](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:535), [views.py:252](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/core/views.py:252)) |
| Razão e ações da tarefa | São calculadas em memória por `_tarefa_pura` e `_pela_decisao`; `_portas_da_tarefa` também pode acrescentar `resolver_dia`. ([tarefa_do_dia.py:142](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:190](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:190), [tarefa_do_dia.py:690](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:690)) | A própria `_portas_da_tarefa` lê as ações para compor fio e rota; o contrato público devolve a tarefa ao chamador. ([tarefa_do_dia.py:633](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:633), [tarefa_do_dia.py:739](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:739)) |
| Último ato `DecisaoPergunta` | A sequência persistida tem porta única `registrar`; por exemplo, o fechamento registra `SILENCIO`, e a porta de resposta registra `RESPOSTA`. ([models.py:4019](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:4019), [models.py:4152](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:4152), [models.py:2011](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2011), [disputa_emissao.py:1525](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/disputa_emissao.py:1525)) | A carga seleciona e ordena as decisões elegíveis; `_pela_decisao` lê a última dessa sequência filtrada. ([tarefa_do_dia.py:417](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:417), [tarefa_do_dia.py:158](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:158)) |

## 18/11 12:10 · sessao ab56235153ca · F5-DSTL1 · leitura · origem=Code

**A ação para a pergunta é a resposta do colaborador no app.** A consulta do app lista a pergunta pendente e a API oferece a resposta pelo ID dela, mesmo que a tarefa do dia não a tenha associado a `responder/colaborador`. Isso, porém, não determina a porta publicada para o dia: ela pode apontar para outra ação, para um chamado, para nenhuma porta conhecida ou para desconhecimento. [app/api/views.py:2524](app/api/views.py:2524), [app/api/views.py:2583](app/api/views.py:2583), [app/chamados/services/tarefa_do_dia.py:657](app/chamados/services/tarefa_do_dia.py:657), [app/chamados/services/tarefa_do_dia.py:675](app/chamados/services/tarefa_do_dia.py:675)

O hash do HEAD **[não verificado]**: a cópia disponibilizada não contém `.git`; as referências abaixo são aos arquivos dessa cópia. [não verificado]

1. **Onde cada resposta é construída**

   - **Por pergunta e instância:** `_pela_decisao` retorna uma razão e ações com campos `acao`, `de` e `porta`. Sem decisão, ou após recusa/reabertura, atribui `responder` ao colaborador; resposta com texto leva a `validar/admin`; há ainda ações de sistema, de ausência, lista vazia e casos indeterminados. [app/chamados/services/tarefa_do_dia.py:135](app/chamados/services/tarefa_do_dia.py:135), [app/chamados/services/tarefa_do_dia.py:142](app/chamados/services/tarefa_do_dia.py:142)

   - **Montagem da tarefa:** `_tarefa_pura` cria `instancias` com `pergunta_id`, `razao` e `acoes`. A associação depende da posição e do motivo; `fora_vinculo` retorna cedo, lâmpada acesa e dia sem trabalho seguem ramos próprios, e ausência pode ocupar a instância. Para pergunta fora do mapa ou das instâncias, `expor_admin` só acrescenta ação administrativa ou indeterminação, não uma ação de resposta do colaborador. [app/chamados/services/tarefa_do_dia.py:208](app/chamados/services/tarefa_do_dia.py:208), [app/chamados/services/tarefa_do_dia.py:254](app/chamados/services/tarefa_do_dia.py:254), [app/chamados/services/tarefa_do_dia.py:267](app/chamados/services/tarefa_do_dia.py:267), [app/chamados/services/tarefa_do_dia.py:288](app/chamados/services/tarefa_do_dia.py:288), [app/chamados/services/tarefa_do_dia.py:309](app/chamados/services/tarefa_do_dia.py:309), [app/chamados/services/tarefa_do_dia.py:342](app/chamados/services/tarefa_do_dia.py:342)

   - **Porta do dia:** `_portas_da_tarefa` reúne ações, âncoras e chamados da fila, publica `fio`, `rota`, `fila`, `verbos` e `acao`, e exige tanto pergunta pendente no app quanto ação `responder/colaborador` associada à instância para declarar a porta do colaborador. A projeção pública é `porta_da_tarefa`. [app/chamados/services/tarefa_do_dia.py:633](app/chamados/services/tarefa_do_dia.py:633), [app/chamados/services/tarefa_do_dia.py:657](app/chamados/services/tarefa_do_dia.py:657), [app/chamados/services/tarefa_do_dia.py:714](app/chamados/services/tarefa_do_dia.py:714), [app/chamados/services/tarefa_do_dia.py:718](app/chamados/services/tarefa_do_dia.py:718)

   - **Âncora e verbo:** `chamado_que_lista` escolhe `chamado_destino_id` e, na falta, o chamado da disputa. `alimentar_fila` obtém a pertença à fila e o mapa de verbos do painel; ausência na partição deixa verbo `None`. A escada `FIO_DO_VERBO` associa `cobrar` a `colab` e `validar`/`decidir` a `admin`; `fio_pelos_verbos` a lê. `arquivado` e `historico` estão fora da lista padrão. O verbo concreto da âncora desta pergunta **[não verificado]**. [app/chamados/services/fio.py:97](app/chamados/services/fio.py:97), [app/chamados/juizes.py:1594](app/chamados/juizes.py:1594), [app/chamados/juizes.py:1681](app/chamados/juizes.py:1681), [app/chamados/services/verbos.py:12](app/chamados/services/verbos.py:12)

   - **App:** `particao_do_app` considera pendente a pergunta viva sem resposta, em disputa aberta. `api_disputa_perguntas` serve essas perguntas e suas opções; `api_disputa_responder` aceita POST do colaborador vinculado, verifica a vitalidade e passa a resposta ao núcleo, que ainda pode recusá-la. Portanto, a oferta é **do colaborador**, sujeita às validações da porta. [app/chamados/services/pergunta_no_app.py:45](app/chamados/services/pergunta_no_app.py:45), [app/chamados/services/pergunta_no_app.py:76](app/chamados/services/pergunta_no_app.py:76), [app/api/views.py:2524](app/api/views.py:2524), [app/api/views.py:2610](app/api/views.py:2610)

2. **Quem responde pelo dia sem instância da pergunta**

   Para afirmar a ação disponível **do dia** e seu dono, a autoridade é `tarefa_do_dia`, lida por **`porta_da_tarefa`**; os leitores de folha, pauta, vigia e beco foram direcionados a essa projeção. Um `fio='admin'` com rota é a resposta publicada para o dia, não prova de que a pergunta pendente tenha ação administrativa própria. [app/core/juizes.py:132](app/core/juizes.py:132), [app/core/juizes.py:153](app/core/juizes.py:153), [app/chamados/services/tarefa_do_dia.py:675](app/chamados/services/tarefa_do_dia.py:675)

   A mesma `porta_da_tarefa` responde à **ausência conhecida de porta do dia** com `None`; `_fio_da_porta` a lê como `nada`. Se há desconhecimento, a projeção pode ter `rota=None` e `nao_sei=True`, que não é uma negativa conhecida. Isso não autoriza concluir que ninguém possa responder à pergunta no app. [app/chamados/services/tarefa_do_dia.py:718](app/chamados/services/tarefa_do_dia.py:718), [app/folha/fora_da_folha.py:86](app/folha/fora_da_folha.py:86)

   `resolver_dia` entra como **ação administrativa do dia**, se a autoridade do calendário a oferece e a tarefa de base está sem rota; a tarefa então publica `admin`/`porta_acao`/`resolver_dia`, inclusive sem marco anterior. A oferta é consultada por `resolver_dia_oferecido_nas_datas`; falha da consulta produz desconhecimento. Isso não associa, por si só, `responder` à pergunta. [app/chamados/services/tarefa_do_dia.py:609](app/chamados/services/tarefa_do_dia.py:609), [app/chamados/services/tarefa_do_dia.py:690](app/chamados/services/tarefa_do_dia.py:690), [app/colaboradores/services/calendario.py:125](app/colaboradores/services/calendario.py:125)

3. **Vigia ou contador específico**

   Nas leituras e na busca realizadas, **não encontrei um estado próprio que cruze “pergunta pendente no app” com “tarefa não associa a ela ação `responder/colaborador`”**; exaustividade de todos os leitores **[não verificado]**. A tarefa faz esse cruzamento para decidir se publica a porta do colaborador, mas não publica ali um alerta específico para a divergência. [app/chamados/services/tarefa_do_dia.py:530](app/chamados/services/tarefa_do_dia.py:530), [app/chamados/services/tarefa_do_dia.py:657](app/chamados/services/tarefa_do_dia.py:657)

   Há sinais mais amplos: o badge conta pendentes no app sem testar a associação à tarefa; `censo_alcance` conta portas e dias sem porta, com causas por dia, mas não nomeia esse cruzamento como classe própria. `dias_sem_fio` também opera por dia discordante e fio, separando desconhecimento. [app/chamados/services/pergunta_no_app.py:101](app/chamados/services/pergunta_no_app.py:101), [app/chamados/services/vigia_fio.py:589](app/chamados/services/vigia_fio.py:589), [app/chamados/services/vigia_fio.py:621](app/chamados/services/vigia_fio.py:621), [app/ponto/services/pautas_dos_discordantes.py:46](app/ponto/services/pautas_dos_discordantes.py:46)

   `pergunta_sem_acao_possivel` tem outro significado: trata pergunta já materializada de fato na mesa; não é esse detector. [app/chamados/services/raiox.py:160](app/chamados/services/raiox.py:160)

## 18/11 11:59 · sessao 7dd20f0f2148 · S6-ORACULO-FERIADO · sonda · origem=Code

[Não verificado]

```text
=== SOMBRA (sombra_db, leitor) 18/11 11:58:07 ===
sonda_leitura: sonda.py [b0295e9b96b03c82] no cpuset 4-7, lendo sombra como leitor (LEITURA)

VERSOES_NA_COMP ['motor', 'oraculo']
ORACULO_COM_HE100FER>0_total_linhas 95
ORACULO_COM_HE100FER>0_de_colab_com_escala_intermitente 11
POR_CICLO_NO_DIA x motor {(('intermitente',), 'motor_he100fer=0'): 7, (('6x1',), 'motor_he100fer>0'): 79, (('12x36', '6x1'), 'motor_he100fer>0'): 2, (('personalizado',), 'motor_he100fer>0'): 4, (('6x1', 'intermitente'), 'motor_he100fer>0'): 2, (('12x36',), 'motor_he100fer=0'): 1}
INTERMITENTE_NO_DIA 9 datas ['2026-11-02', '2026-11-15']
NAO_INTERMITENTES_com_he100fer>0_no_oraculo 84
--- col85 2026-11-02
  ORACULO linha: trab=11.9 he100fer=11.9 he100=11.9 he50=0.0 folga=0.0
  MOTOR  linha: trab=11.9 he100fer=0.0 he100=0.0 he50=0.0 folga=0.0
  LEITOR espelho/calendario/PDF/app (linhas_do_periodo->pago_do_dia, rubricas_do_dia): pago=11.897632222222223 extras_100=0.0 extras_50=0.0 trab=11.9 lavrada=True
  LEITOR extrato/gestao_he (soma_do_periodo): he100fer=0.0 he100=0.0 he50=0.0 trab=11.9 sem_lavr=False sem_apur=False
  LEITOR gestao_he he_lavrada_do_colab: (0.0, '')
  LEITOR cartao-PDF/porta_export (soma_da_competencia, competencia inteira): he100fer=0.0 he100=0.0 trab=78.2
  CONTRAFATO leitor com versao=oraculo: pago=11.897632222222223 extras_100=11.9
--- col146 2026-11-02
  ORACULO linha: trab=13.5 he100fer=13.5 he100=13.5 he50=0.0 folga=0.0
  MOTOR  linha: trab=13.5 he100fer=0.0 he100=0.0 he50=0.0 folga=0.0
  LEITOR espelho/calendario/PDF/app (linhas_do_periodo->pago_do_dia, rubricas_do_dia): pago=13.50255722222222 extras_100=0.0 extras_50=0.0 trab=13.5 lavrada=True
  LEITOR extrato/gestao_he (soma_do_periodo): he100fer=0.0 he100=0.0 he50=0.0 trab=13.5 sem_lavr=False sem_apur=False
  LEITOR gestao_he he_lavrada_do_colab: (0.0, '')
  LEITOR cartao-PDF/porta_export (soma_da_competencia, competencia inteira): he100fer=0.0 he100=0.0 trab=53.58
  CONTRAFATO leitor com versao=oraculo: pago=13.50255722222222 extras_100=13.5
--- col346 2026-11-02
  ORACULO linha: trab=11.65 he100fer=11.65 he100=11.65 he50=0.0 folga=0.0
  MOTOR  linha: trab=11.65 he100fer=11.65 he100=11.65 he50=0.0 folga=0.0
  LEITOR espelho/calendario/PDF/app (linhas_do_periodo->pago_do_dia, rubricas_do_dia): pago=11.646084444444446 extras_100=11.65 extras_50=0.0 trab=11.65 lavrada=True
  LEITOR extrato/gestao_he (soma_do_periodo): he100fer=11.65 he100=11.65 he50=0.0 trab=11.65 sem_lavr=False sem_apur=False
  LEITOR gestao_he he_lavrada_do_colab: (11.65, '')
  LEITOR cartao-PDF/porta_export (soma_da_competencia, competencia inteira): he100fer=23.17 he100=23.17 trab=81.37
  CONTRAFATO leitor com versao=oraculo: pago=11.646084444444446 extras_100=11.65
--- col346 2026-11-15
  ORACULO linha: trab=11.52 he100fer=11.52 he100=11.52 he50=0.0 folga=0.0
  MOTOR  linha: trab=11.52 he100fer=11.52 he100=11.52 he50=0.0 folga=0.0
  LEITOR espelho/calendario/PDF/app (linhas_do_periodo->pago_do_dia, rubricas_do_dia): pago=11.521407777777776 extras_100=11.52 extras_50=0.0 trab=11.52 lavrada=True
  LEITOR extrato/gestao_he (soma_do_periodo): he100fer=11.52 he100=11.52 he50=0.0 trab=11.52 sem_lavr=False sem_apur=False
  LEITOR gestao_he he_lavrada_do_colab: (11.52, '')
  LEITOR cartao-PDF/porta_export (soma_da_competencia, competencia inteira): he100fer=23.17 he100=23.17 trab=81.37
  CONTRAFATO leitor com versao=oraculo: pago=11.521407777777776 extras_100=11.52
--- col365 2026-11-02
  ORACULO linha: trab=12.0 he100fer=12.0 he100=12.0 he50=0.0 folga=0.0
  MOTOR  linha: trab=12.0 he100fer=0.0 he100=0.0 he50=0.0 folga=0.0
  LEITOR espelho/calendario/PDF/app (linhas_do_periodo->pago_do_dia, rubricas_do_dia): pago=12.0 extras_100=0.0 extras_50=0.0 trab=12.0 lavrada=True
  LEITOR extrato/gestao_he (soma_do_periodo): he100fer=0.0 he100=0.0 he50=0.0 trab=12.0 sem_lavr=False sem_apur=False
  LEITOR gestao_he he_lavrada_do_colab: (0.0, '')
  LEITOR cartao-PDF/porta_export (soma_da_competencia, competencia inteira): he100fer=0.0 he100=0.0 trab=80.16
  CONTRAFATO leitor com versao=oraculo: pago=12.0 extras_100=12.0
--- col365 2026-11-15
  ORACULO linha: trab=11.04 he100fer=11.04 he100=11.04 he50=0.0 folga=0.0
  MOTOR  linha: trab=11.04 he100fer=0.0 he100=0.0 he50=0.0 folga=0.0
  LEITOR espelho/calendario/PDF/app (linhas_do_periodo->pago_do_dia, rubricas_do_dia): pago=11.036849166666666 extras_100=0.0 extras_50=0.0 trab=11.04 lavrada=True
  LEITOR extrato/gestao_he (soma_do_periodo): he100fer=0.0 he100=0.0 he50=0.0 trab=11.04 sem_lavr=False sem_apur=False
  LEITOR gestao_he he_lavrada_do_colab: (0.0, '')
  LEITOR cartao-PDF/porta_export (soma_da_competencia, competencia inteira): he100fer=0.0 he100=0.0 trab=80.16
  CONTRAFATO leitor com versao=oraculo: pago=11.036849166666666 extras_100=11.04
--- col830 2026-11-02
  ORACULO linha: trab=12.07 he100fer=12.07 he100=12.07 he50=0.0 folga=0.0
  MOTOR  linha: trab=12.07 he100fer=0.0 he100=0.0 he50=0.0 folga=0.0
  LEITOR espelho/calendario/PDF/app (linhas_do_periodo->pago_do_dia, rubricas_do_dia): pago=12.066969811944444 extras_100=0.0 extras_50=0.0 trab=12.07 lavrada=True
  LEITOR extrato/gestao_he (soma_do_periodo): he100fer=0.0 he100=0.0 he50=0.0 trab=12.07 sem_lavr=False sem_apur=False
  LEITOR gestao_he he_lavrada_do_colab: (0.0, '')
  LEITOR cartao-PDF/porta_export (soma_da_competencia, competencia inteira): he100fer=0.0 he100=0.0 trab=105.07
  CONTRAFATO leitor com versao=oraculo: pago=12.066969811944444 extras_100=12.07
--- col830 2026-11-15
  ORACULO linha: trab=11.89 he100fer=11.89 he100=11.89 he50=0.0 folga=0.0
  MOTOR  linha: trab=11.89 he100fer=0.0 he100=0.0 he50=0.0 folga=0.0
  LEITOR espelho/calendario/PDF/app (linhas_do_periodo->pago_do_dia, rubricas_do_dia): pago=11.885994444444444 extras_100=0.0 extras_50=0.0 trab=11.89 lavrada=True
  LEITOR extrato/gestao_he (soma_do_periodo): he100fer=0.0 he100=0.0 he50=0.0 trab=11.89 sem_lavr=False sem_apur=False
  LEITOR gestao_he he_lavrada_do_colab: (0.0, '')
  LEITOR cartao-PDF/porta_export (soma_da_competencia, competencia inteira): he100fer=0.0 he100=0.0 trab=105.07
  CONTRAFATO leitor com versao=oraculo: pago=11.885994444444444 extras_100=11.89
--- col975 2026-11-02
  ORACULO linha: trab=3.96 he100fer=3.96 he100=3.96 he50=0.0 folga=0.0
  MOTOR  linha: trab=3.96 he100fer=0.0 he100=0.0 he50=0.0 folga=0.0
  LEITOR espelho/calendario/PDF/app (linhas_do_periodo->pago_do_dia, rubricas_do_dia): pago=3.9582752777777777 extras_100=0.0 extras_50=0.0 trab=3.96 lavrada=True
  LEITOR extrato/gestao_he (soma_do_periodo): he100fer=0.0 he100=0.0 he50=0.0 trab=3.96 sem_lavr=False sem_apur=False
  LEITOR gestao_he he_lavrada_do_colab: (0.0, '')
  LEITOR cartao-PDF/porta_export (soma_da_competencia, competencia inteira): he100fer=0.0 he100=0.0 trab=32.87
  CONTRAFATO leitor com versao=oraculo: pago=3.9582752777777777 extras_100=3.96
rc_sombra=0
```

## 18/11 11:58 · sessao 5f4761714ba9 · S6-ORACULO-FERIADO · leitura · origem=Code

A lavra `versao='oraculo'` **pode chegar a fotos, arquivos de reversão, diffs e contagens que não filtram versão**. Nos caminhos de tela, PDF, API e relatório rastreados abaixo, as chamadas usam o padrão `motor`; o TXT Dominio lê `FechamentoMensal`. A existência ou o conteúdo de linhas em produção é **[não verificado]**. [app/ponto/models.py:322](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:322) · [app/chamados/services/foto_e4b.py:289](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:289) · [app/ponto/services/dia_pago.py:833](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:833) · [app/folha/export.py:719](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/export.py:719)

O campo `versao` documenta `motor` e `oraculo`, com default `motor`, mas é um `CharField` sem `choices`: esses são os dois valores documentados, não uma enumeração fechada pelo modelo. A constraint inclui `versao`, portanto **não escolhe uma única versão** para uma consulta sem filtro; o modelo não declara manager com filtro de versão. [app/ponto/models.py:303](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:303) · [app/ponto/models.py:322](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:322) · [app/ponto/models.py:382](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/models.py:382)

Para abreviar a tabela: **feriado** significa `horas_extras_100_feriado`; **CAMPOS_DIA** são todas as rubricas e os três campos de minutos, incluindo feriado; **CAMPOS** acrescenta reflexo DSR e saldo de banco. “Oráculo possível” significa que o código aceita ou inclui essa versão, **condicionado à existência das linhas**, não que elas existam em produção. [app/ponto/services/dia_pago.py:33](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:33)

### Consultas ORM e seus destinos

| Leitura ORM e função | Versão e campos efetivamente usados | Destino; caminho do oráculo |
|---|---|---|
| [dia_pago.py:466](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:466), `retratar_exportada` | `versao` argumento, default `motor` ([linha 405](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:405)); `data` e minutos abonados, previstos e realizados. Não usa feriado no cálculo. | Atualiza ou cria retrato de grade ([linha 481](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:481), [linha 538](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:538)). **Oráculo possível** pelo `--versao` passado pelo comando ([lavrar_dias_pagos.py:52](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/lavrar_dias_pagos.py:52), [linha 123](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/lavrar_dias_pagos.py:123)). |
| [dia_pago.py:504](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:504), `retratar_exportada` | Mesmo argumento; linha de ajuste, reflexo DSR e saldo de banco. | Retrato/escrita do ajuste ([linha 518](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:518)). **Oráculo possível** pela mesma opção do comando. |
| [dia_pago.py:559](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:559), `soma_da_competencia` | `versao` argumento, default `motor`; `Sum` de **CAMPOS**, inclusive feriado ([linha 562](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:562)). | PDF/cartão usa rubricas selecionadas ([cartao_pela_celula.py:143](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/cartao_pela_celula.py:143), [linha 173](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/cartao_pela_celula.py:173)); porta compara com fechamento/badge ([porta_export.py:267](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:267), [linha 388](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:388)). Esses chamadores usam `motor`; chamada explícita com `oraculo` é possível, mas destino produtivo assim **[não verificado]**. |
| [dia_pago.py:588](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:588), `lavratura_sem_furo` | Argumento default `motor`; `exists` de `tipo` e `furo_apurado IS NULL`. | Vigia da porta ([porta_export.py:430](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:430)). Chamador rastreado usa `motor`; chamada explícita com outra versão é possível. |
| [dia_pago.py:645](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:645), `apurar_furo_da_exportada` | Argumento default `motor`; lê `pk`, `data`, `furo_apurado`, `lavrado_em` ([linha 649](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:649)). | Medida/reversão e atualização de furo ([linha 651](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:651), [linha 671](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:671)). **Oráculo possível**: comando escolhe versões observadas com nulos ([apurar_furo_da_exportada.py:99](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/apurar_furo_da_exportada.py:99)). |
| [dia_pago.py:698](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:698), `dias_com_abono` | Argumento default `motor`; conta dias com `minutos_abonados > 0`. | Contagem no resumo do cartão/PDF ([cartao_pela_celula.py:190](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/cartao_pela_celula.py:190)). Chamador usa `motor`; outra versão pode ser passada à função. |
| [dia_pago.py:784](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:784), `soma_do_periodo` | Argumento default `motor` ([linha 752](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:752)); agrega cada campo de **CAMPOS_DIA**, incluindo feriado. | Extrato exibido seleciona trabalhadas, noturnas, HE50, HE100 e atraso ([views.py:487](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/views.py:487), [linha 520](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/views.py:520), [template:70](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/templates/relatorios/extrato_parcial.html:70)); gestão e autorização usam HE50 + HE100 ([gestao_he.py:256](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:256), [autorizacao_he_periodo.py:174](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/autorizacao_he_periodo.py:174), [dia_pago.py:749](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:749)); comando usa em diff ([diff_extrato_parcial.py:103](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/management/commands/diff_extrato_parcial.py:103)). Chamadas citadas usam `motor`; destino produtivo com argumento `oraculo` **[não verificado]**. |
| [dia_pago.py:810](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:810), `soma_do_periodo` | Mesmo argumento; `colaborador_id`, `mes`, `ano`, distintos; não lê feriado. | Classifica ausência de lavratura/apuração para os mesmos destinos ([linha 814](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:814)). Chamadas citadas usam `motor`; argumento `oraculo` é possível. |
| [dia_pago.py:855](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:855), `linhas_do_periodo` | Argumento default `motor`; carrega linhas de dia completas. Consumidores efetivos estão na tabela seguinte. | Espelho, calendário e API chamam sem versão; portanto usam `motor` ([espelho.py:410](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:410), [calendario.py:319](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/services/calendario.py:319), [api/views.py:1449](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/api/views.py:1449)). Destino produtivo com argumento `oraculo` **[não verificado]**. |
| [porta_export.py:75](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:75), `dias_fora_do_dia_pago` | Argumento default `motor`, ou mapa fornecido pelo chamador; lê data e campos de `pago_do_dia`, inclusive feriado como teste de presença. | Diff espelho × dia pago ([linha 107](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:107)). Chamador rastreado fornece mapa `motor` ([linha 588](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:588)); a interface também aceita `oraculo` ou outro mapa. |
| [porta_export.py:284](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:284), `regua_das_linhas` | Literal `motor`; agrupa `versao_motor`, conta PK e obtém máximo de `lavrado_em` ([linha 285](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:285)). | Diagnóstico da régua da linha × badge ([linha 443](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:443)); **oráculo não passa pelo filtro**. |
| [porta_export.py:588](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:588), `medir` | Literal `motor`; mapa por `data`, consumido por `pago_do_dia`. | Diff da porta ([linha 597](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:597)), persistido na métrica ([linha 779](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:779)); **oráculo não passa**. |
| [porta_export.py:650](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:650), `medir` | Literal `motor`; `colaborador_id`. | Compõe universo da medida ([linha 658](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/porta_export.py:658)); **oráculo não passa**. |
| [foto_e4b.py:289](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:289), `foto_do_colaborador` | **Não filtra versão**; lê todos os campos concretos, inclusive feriado ([linha 247](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:247)). | Foto, diff e relato de campos numéricos ([linha 187](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/foto_e4b.py:187)); comando imprime diff JSON ([backfill_fio_morto_e4b.py:404](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/management/commands/backfill_fio_morto_e4b.py:404)). **Sim, oráculo pode chegar** se houver linha. |
| [apurar_furo_da_exportada.py:59](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/apurar_furo_da_exportada.py:59), `_universo` | **Não filtra versão**; lê `versao`, colaborador e `furo_apurado` ([linha 61](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/apurar_furo_da_exportada.py:61)). | Contagem por versão impressa ([linha 101](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/apurar_furo_da_exportada.py:101)). **Oráculo pode chegar**. |
| [lavrar_dias_pagos.py:140](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/lavrar_dias_pagos.py:140), `handle` | Filtra argumento `--versao`, default `motor`; `count`, sem rubricas. | Contagem impressa ([linha 141](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/lavrar_dias_pagos.py:141)). **Oráculo possível** com `--versao oraculo` ([linha 52](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/management/commands/lavrar_dias_pagos.py:52)). |

### Destinos que consomem as linhas do período

| Consumidor | Campos e destino | Oráculo |
|---|---|---|
| [espelho.py:410](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:410), serviço do espelho | `pago_do_dia` verifica rubricas incluindo feriado e calcula trabalhadas + folga trabalhada + `detalhe.folga_feriado_min`; não soma diretamente o campo feriado ([dia_pago.py:861](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:861), [linha 890](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:890)). `rubricas_do_dia` usa HE50, HE100, atraso, antecipada, noturnas, intra e trabalhadas ([linha 914](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:914)); exibição no [espelho.html:276](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/templates/ponto/espelho.html:276) e [PDF:902](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/pdf_espelho.py:902). | Chamada usa default `motor`; não chega por esse caminho. |
| [calendario.py:319](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/services/calendario.py:319) | Usa `rubricas_do_dia` ([linha 689](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/services/calendario.py:689)) e entrega a linha e o indicador de lavra ao calendário ([linha 778](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/colaboradores/services/calendario.py:778)). | Default `motor`; não chega por esse caminho. |
| [api/views.py:1449](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/api/views.py:1449), `api_espelho_v2` | Obtém horas do dia e exporta `horas_trabalhadas` e `dia_pago_lavrada` no payload ([linha 1531](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/api/views.py:1531), [linha 1546](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/api/views.py:1546)). | Default `motor`; não chega por esse caminho. |

A soma consulta feriado separadamente, mas o extrato e o cartão selecionam `horas_extras_100` como total, sem acrescentar o subset de feriado outra vez. [app/relatorios/views.py:525](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/views.py:525) · [app/relatorios/cartao_pela_celula.py:177](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/relatorios/cartao_pela_celula.py:177)

No TXT Dominio, `eventos_do_fechamento` lê o feriado de **`FechamentoMensal`**, separa-o da HE100 normal e o transforma em evento; a geração consulta fechamentos e monta as linhas. Um caminho em que o valor de uma linha `DiaPago` `oraculo` alimente esse TXT é **[não verificado]**. [app/folha/export.py:420](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/export.py:420) · [app/folha/export.py:719](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/export.py:719) · [app/folha/export.py:730](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/export.py:730)

### Scripts avulsos fora de `app/`

Estes são leitores no código da cópia; sua execução em produção é **[não verificada]**.

| Leitura e função | Versão, campos e destino | Oráculo |
|---|---|---|
| [snapshot_badge_linha_09_10.py:50](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/snapshot_badge_linha_09_10.py:50), nível de módulo | Sem filtro; identificação, versão, origem e **CAMPOS**, inclusive feriado; grava JSON de reversão ([linha 71](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/snapshot_badge_linha_09_10.py:71)). A consulta separada em [linha 78](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/snapshot_badge_linha_09_10.py:78) conta todas as versões. | **Pode chegar** ao JSON e à contagem. |
| [r5_idempotencia_frota.py:65](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/r5_idempotencia_frota.py:65), função de foto | Sem filtro; chave com colaborador, data e versão; hash de trabalhadas, realizados e `getattr(horas_extras)` ([linha 67](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/r5_idempotencia_frota.py:67)). | **Pode chegar** ao diff/hash; feriado não é lido nominalmente. |
| [relavrar_o118_col221.py:40](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/relavrar_o118_col221.py:40), [o119:39](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/relavrar_o119_col221.py:39), [o120:69](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/relavrar_o120_col221_09.py:69), nível de módulo | Sem filtro; campos do modelo por introspecção, inclusive feriado; JSON de reversão ([o118:31](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/relavrar_o118_col221.py:31), [linha 45](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/relavrar_o118_col221.py:45), [o120:71](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/relavrar_o120_col221_09.py:71)). | **Pode chegar** à foto. |
| [relavrar_o118_col221.py:60](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/relavrar_o118_col221.py:60), [o119:59](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/relavrar_o119_col221.py:59), nível de módulo | Literal `motor`; `data` e `minutos_previstos`, para contagem e soma impressas ([o118:62](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/relavrar_o118_col221.py:62)). | Não passa pelo filtro. |
| [snapshot_o263_comp10.py:61](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/snapshot_o263_comp10.py:61), [snapshot_relavratura_10_2026.py:34](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/snapshot_relavratura_10_2026.py:34), nível de módulo | Literal `motor`; campos do modelo por introspecção, inclusive feriado, para foto de reversão ([o263:42](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/snapshot_o263_comp10.py:42), [relavratura:26](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/snapshot_relavratura_10_2026.py:26)). | Não passa pelo filtro. |
| [remedir_congelados_placar_e.py:21](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/remedir_congelados_placar_e.py:21) e [linha 61](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/remedir_congelados_placar_e.py:61), nível de módulo | Literal `motor`; data, atraso e saída antecipada; medida e impressão ([linha 50](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/remedir_congelados_placar_e.py:50), [linha 64](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/bin/remedir_congelados_placar_e.py:64)). | Não passa pelo filtro. |

### Escrita

| Escritor | O que grava |
|---|---|
| [fechamento.py:1265](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:1265), `recalcular_fechamento_mes` | Para competência a partir de 10/2026, calcula rubricas e chama a lavra com literal `versao='oraculo'` ([linha 1271](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:1271), [linha 1277](

[truncado em 20000 de 24018 caracteres]

## 18/11 11:57 · sessao f1d73f865ba4 · S6-ORACULO-FERIADO · sonda · origem=Code

Os valores abaixo reproduzem o dado raspado fornecido. A marca **[não verificado]** indica que não há arquivo e linha identificáveis para conferi-los. O número de `he100fer` do oráculo aparece em `trab` nos leitores de todos os casos; nas rubricas de extras dos leitores, aparece para col346 e col610. [não verificado]

Em todas as linhas, `he50=0.0` e `folga=0.0` no oráculo e no motor. Nos leitores de espelho, `extras_50=0.0` e `lavrada=True`; nos de extrato, `he50=0.0`, `sem_lavr=False` e `sem_apur=False`. [não verificado]

| Colaborador e data | Oráculo: `trab / he100fer / he100` | Motor: `trab / he100fer / he100` |
|---|---|---|
| col85 2026-11-02 [não verificado] | 11.9 / 11.9 / 11.9 | 11.9 / 0.0 / 0.0 |
| col146 2026-11-02 [não verificado] | 13.5 / 13.5 / 13.5 | 13.5 / 0.0 / 0.0 |
| col346 2026-11-02 [não verificado] | 11.65 / 11.65 / 11.65 | 11.65 / 11.65 / 11.65 |
| col346 2026-11-15 [não verificado] | 11.52 / 11.52 / 11.52 | 11.52 / 11.52 / 11.52 |
| col365 2026-11-02 [não verificado] | 12.0 / 12.0 / 12.0 | 12.0 / 0.0 / 0.0 |
| col365 2026-11-15 [não verificado] | 11.04 / 11.04 / 11.04 | 11.04 / 0.0 / 0.0 |
| col610 2026-11-02 [não verificado] | 11.8 / 11.8 / 11.8 | 11.8 / 11.8 / 11.8 |
| col610 2026-11-15 [não verificado] | 12.0 / 12.0 / 12.0 | 12.0 / 12.0 / 12.0 |
| col830 2026-11-02 [não verificado] | 12.07 / 12.07 / 12.07 | 12.07 / 0.0 / 0.0 |
| col830 2026-11-15 [não verificado] | 11.89 / 11.89 / 11.89 | 11.89 / 0.0 / 0.0 |
| col975 2026-11-02 [não verificado] | 3.96 / 3.96 / 3.96 | 3.96 / 0.0 / 0.0 |

| Colaborador e data | Espelho/calendário/PDF/app: `pago / extras_100 / trab` | Extrato/gestão HE: `he100fer / he100 / trab` | Gestão HE: `he_lavrada_do_colab` |
|---|---|---|---|
| col85 2026-11-02 [não verificado] | 11.897632222222223 / 0.0 / 11.9 | 0.0 / 0.0 / 11.9 | (0.0, '') |
| col146 2026-11-02 [não verificado] | 13.50255722222222 / 0.0 / 13.5 | 0.0 / 0.0 / 13.5 | (0.0, '') |
| col346 2026-11-02 [não verificado] | 11.646084444444446 / 11.65 / 11.65 | 11.65 / 11.65 / 11.65 | (11.65, '') |
| col346 2026-11-15 [não verificado] | 11.521407777777776 / 11.52 / 11.52 | 11.52 / 11.52 / 11.52 | (11.52, '') |
| col365 2026-11-02 [não verificado] | 12.0 / 0.0 / 12.0 | 0.0 / 0.0 / 12.0 | (0.0, '') |
| col365 2026-11-15 [não verificado] | 11.036849166666666 / 0.0 / 11.04 | 0.0 / 0.0 / 11.04 | (0.0, '') |
| col610 2026-11-02 [não verificado] | 11.797882500000002 / 11.8 / 11.8 | 11.8 / 11.8 / 11.8 | (11.8, '') |
| col610 2026-11-15 [não verificado] | 12.0 / 12.0 / 12.0 | 12.0 / 12.0 / 12.0 | (12.0, '') |
| col830 2026-11-02 [não verificado] | 12.066969811944444 / 0.0 / 12.07 | 0.0 / 0.0 / 12.07 | (0.0, '') |
| col830 2026-11-15 [não verificado] | 11.885994444444444 / 0.0 / 11.89 | 0.0 / 0.0 / 11.89 | (0.0, '') |
| col975 2026-11-02 [não verificado] | 3.9582752777777777 / 0.0 / 3.96 | 0.0 / 0.0 / 3.96 | (0.0, '') |

O cartão-PDF/porta_export mostra a **soma da competência inteira**, repetida abaixo para cada data. O contrafato é a saída registrada com `versao=oraculo`. [não verificado]

| Colaborador e data | Cartão-PDF/porta_export: `he100fer / he100 / trab` | Contrafato: `pago / extras_100` |
|---|---|---|
| col85 2026-11-02 [não verificado] | 0.0 / 0.0 / 78.2 | 11.897632222222223 / 11.9 |
| col146 2026-11-02 [não verificado] | 0.0 / 0.0 / 53.58 | 13.50255722222222 / 13.5 |
| col346 2026-11-02 [não verificado] | 23.17 / 23.17 / 81.37 | 11.646084444444446 / 11.65 |
| col346 2026-11-15 [não verificado] | 23.17 / 23.17 / 81.37 | 11.521407777777776 / 11.52 |
| col365 2026-11-02 [não verificado] | 0.0 / 0.0 / 80.16 | 12.0 / 12.0 |
| col365 2026-11-15 [não verificado] | 0.0 / 0.0 / 80.16 | 11.036849166666666 / 11.04 |
| col610 2026-11-02 [não verificado] | 23.8 / 23.8 / 106.99 | 11.797882500000002 / 11.8 |
| col610 2026-11-15 [não verificado] | 23.8 / 23.8 / 106.99 | 12.0 / 12.0 |
| col830 2026-11-02 [não verificado] | 0.0 / 0.0 / 105.07 | 12.066969811944444 / 12.07 |
| col830 2026-11-15 [não verificado] | 0.0 / 0.0 / 105.07 | 11.885994444444444 / 11.89 |
| col975 2026-11-02 [não verificado] | 0.0 / 0.0 / 32.87 | 3.9582752777777777 / 3.96 |

A sonda registra `VERSOES_NA_COMP ['motor', 'oraculo']`, 95 linhas do oráculo com `he100fer>0`, das quais 11 são de colaboradores com escala intermitente e 84 de não intermitentes; `rc_sombra=0`. [não verificado]

## 18/11 11:55 · sessao 3c38a202ef76 · - · - · origem=Ronald (tmux)

Investigação consolidada e enviada ao agente principal, incluindo a leitura direta em `app/chamados/services/foto_e4b.py:289`, os consumidores indiretos de API, calendário e relatórios, e os scripts de fotografia e medição em `bin/`.

## 18/11 11:55 · sessao fd0b8557198b · - · - · origem=Ronald (tmux)

Rastreamento de `app/ponto` enviado ao agente principal, com consultas, filtros de versão, campos, destinos e caminhos que aceitam `oraculo`.

## 18/11 11:55 · sessao f504ec3d3f63 · F5-DSTL1 · verificador · origem=Code

**AVAL-DEVOLVIDO — nova 1(d), perguntas 1 e 3.** A exceção de `passivo_sem_linha` resolve a contradição universal apontada na revisão anterior, mas o RED ainda não está demonstrado e a igualdade dos PKs de passivo pendente pode colidir com 1(a). A ocorrência dessa interseção em produção é **[não verificado]**. ([tarefa_do_dia.py:154](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:154), [RELATO.md:33](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:33))

A cópia não contém `.git`; sua identidade com o hash informado e a condição de HEAD são **[não verificado]**. As referências abaixo apontam para os arquivos presentes. Os resultados de produção são os informados no material e no relato, sem medição independente. ([RELATO.md:25](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:25))

### 1. Premissas — **PROCEDE**

- **Passivo sem ação:** confere com precisão: `_pela_decisao` retorna `('passivo_sem_linha', None)` antes de ler o último ato. Quando esse resultado é exposto numa instância da tarefa, a lista fica `[]` e `nao_sei` é verdadeiro. O passivo significa falta de linha de **resposta ou validação correspondente ao mesmo instante**, não ausência de qualquer decisão na sequência. ([tarefa_do_dia.py:142](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:332](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:332), [models.py:4122](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:4122))
- **Pendente no app:** `consulta_do_app(colab)` obtém as perguntas vivas e chama `particao_do_app`; esta põe em `pendentes` a pergunta não respondida pelo juiz L-130 **e** cuja disputa está aberta. A carga da tarefa usa a mesma partição e C6, restritos às perguntas de fato da janela. ([pergunta_no_app.py:45](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:45), [pergunta_no_app.py:64](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:64), [tarefa_do_dia.py:530](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:530))
- **Ação:** é um dicionário em `instancias[*].acoes`, com `acao='responder'` e `de='colaborador'`; `porta` é outro campo. O próprio cálculo de rota exige tanto essa ação quanto o PK em pendentes do app. A tarefa é por colaborador e dia, com ações por instância de marco; não é uma lista geral de ações por pergunta. ([tarefa_do_dia.py:42](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:42), [tarefa_do_dia.py:135](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:135), [tarefa_do_dia.py:657](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:657))
- **Conjunto por PK:** é calculável pela interseção dos PKs de `consulta_do_app(...)[ 'pendentes' ]` com os PKs devolvidos por `DecisaoPergunta.linhas_da_projecao_sem_linha`, aplicando o mesmo relógio da carga ao `ato_em`. Para reproduzir especificamente `_passivo` da tarefa, é preciso também respeitar seu universo de perguntas `conversa=False` e da janela. Isso não prova que toda pergunta pendente tenha uma instância associável na saída da tarefa. ([models.py:4132](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:4132), [tarefa_do_dia.py:405](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:405), [tarefa_do_dia.py:433](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:433))

**Motivo da devolução:** o cálculo existe, mas o RED está enunciado para *toda pergunta* pendente, enquanto a estrutura de ação consultada é por instância e a carga exclui conversas. Falta definir como o RED trata as perguntas sem instância correspondente. ([pergunta_no_app.py:52](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:52), [tarefa_do_dia.py:405](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:405))

### 2. Colisão com aval ou lei — **PROCEDE**, pela colisão condicional com 1(a)

A exceção de passivo, por si, não identifica colisão direta com L-130: esta define quando há resposta; a nova 1(d) conserva a rota e excetua passivos do RED. O relato registra que o corte F5-L1-VALE-L130-1811 revogou também a frase de painel/admin do corte antigo; o literal completo desse corte posterior na cópia é **[não verificado]**. ([LEIS.md:151](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/LEIS.md:151), [CORTES.md:509](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/CORTES.md:509), [RELATO.md:37](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:37))

Há, porém, **colisão literal se uma pergunta estiver simultaneamente no passivo pendente e entre as que 1(a) manda para histórico**: 1(a) a retira de pendentes, enquanto 1(d) exige exatamente os mesmos PKs no conjunto passivo pendente antes e depois. O código não impede essa combinação: o passivo é verificado antes do último ato e pode coexistir com uma linha `SILENCIO`. A existência de PK nessa interseção na produção é **[não verificado]**. ([tarefa_do_dia.py:154](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:154), [models.py:4135](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:4135), [RELATO.md:33](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:33))

Quanto a 1(c) e `RESSUSCITAR-DEVOLVE-AO-COLABORADOR`, a ressalva de 1(c) permanece necessária: o relato registra respostas posteriores por cortes de dono, mas o alcance literal desses cortes sobre toda absorção rejeitada é **[não verificado]**. A conservação de 1(d) não resolve essa questão. ([LEIS.md:151](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/LEIS.md:151), [RELATO.md:304](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:304), [RELATO.md:365](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:365))

### 3. Condições literais e RED — **PROCEDE**

O RED integrador pode comparar, por PK e no mesmo relógio, `consulta_do_app`/`particao_do_app`, `linhas_da_projecao_sem_linha` e `tarefa_do_dia(...).instancias`, procurando na instância da pergunta a ação `responder` de `colaborador`. Testar somente `_pela_decisao` não demonstra a relação com a lista do app nem cobre perguntas que a tarefa não associa a uma instância. A implementação real da F5 ainda não existe no material descrito. ([pergunta_no_app.py:64](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:64), [tarefa_do_dia.py:332](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:332), [RELATO.md:34](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:34))

**O RED é satisfeito hoje? [não verificado].** O código permite resultados sem `responder` fora do passivo: `SILENCIO` retorna lista vazia; resposta com texto aguarda validação do admin; absorção pendente pede decisão do admin. A partição atual não consulta o último ato. Isso demonstra a possibilidade lógica, mas não prova que cada exemplo esteja simultaneamente pendente no app em produção. Disputa sem pergunta não é, por si, uma *pergunta pendente* abrangida pelo RED. ([tarefa_do_dia.py:158](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:158), [tarefa_do_dia.py:176](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:176), [pergunta_no_app.py:76](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:76))

O dado informado registra 11 `SILENCIO` que sairiam de pendente pela proposta e 29 `REABERTURA` sem ação por passivo. Essas 29 eram descritas na previsão como hoje em histórico; portanto, esse número **não é a contagem solicitada de passivos pendentes hoje**. A interseção dos 11 `SILENCIO` com passivo, os PKs e a contagem atual de violações fora da exceção são **[não verificado]**. ([RELATO.md:23](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:23), [RELATO.md:33](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:33), [RELATO.md:34](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:34))

Para fechar o literal, é necessário medir a interseção **passivo ∩ pendente ∩ SILÊNCIO que 1(a) moveria**, por PK, e decidir a precedência se não for vazia; também é necessário definir o tratamento das perguntas pendentes sem instância de tarefa. O resultado dessas medidas é **[não verificado]**. ([tarefa_do_dia.py:154](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:154), [tarefa_do_dia.py:405](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:405))

### 4. Alcance — **NÃO PROCEDE**

A nova 1(d) declara conservação, medição e registro; não determina escrita em exportada, dinheiro, motor, fechamento ou `DiaPago`, nem autoriza `!` novo. A tarefa existente lê o estado de exportada para uma extensão de exposição administrativa; isso é leitura existente, não evidência de escrita. O efeito de uma implementação futura sobre esses domínios é **[não verificado]**. ([tarefa_do_dia.py:267](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:267), [RELATO.md:34](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/RELATO.md:34))

### Quem escreve e quem lê

- **`passivo_sem_linha`:** é valor calculado. A autoridade `linhas_da_projecao_sem_linha` lê `respondida_em`, `resposta_colab`, `validada_em` e linhas `DecisaoPergunta`; a carga escreve o conjunto transitório `_passivo`, lido por `_pela_decisao`. Exemplos de escritores dos insumos são a porta de resposta, a validação e `DecisaoPergunta.registrar`. O inventário exaustivo de escritores é **[não verificado]**. ([models.py:4122](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:4122), [models.py:4151](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:4151), [tarefa_do_dia.py:433](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:433), [disputa_emissao.py:1476](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/disputa_emissao.py:1476), [validacao.py:161](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/validacao.py:161))
- **Pendente no app:** é lista calculada por `particao_do_app`, que lê o juiz `pergunta_respondida` e `disputa.fechada_em`; `consulta_do_app`, a projeção do app e a carga da tarefa leem a partição. A resposta escreve o par de resposta; o fechamento da disputa escreve `fechada_em`; a reabertura limpa o par. O inventário exaustivo de escritores é **[não verificado]**. ([pergunta_no_app.py:76](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:76), [pergunta_no_app.py:101](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/pergunta_no_app.py:101), [tarefa_do_dia.py:535](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:535), [models.py:2021](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/models.py:2021), [disputa_emissao.py:1610](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/disputa_emissao.py:1610))
- **`acoes` / `RESPONDER`:** `_acao` monta o dicionário transitório; `_pela_decisao` produz `responder` em seus ramos aplicáveis e `_tarefa_pura` publica a lista na instância. `_portas_da_tarefa` lê ação e pendência para publicar a rota do colaborador. A autoridade documentada para a tarefa é `tarefa_do_dia`; a função é pura, sem escritor persistente de tarefa. Outros leitores completos da saída são **[não verificado]**. ([tarefa_do_dia.py:135](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:135), [tarefa_do_dia.py:156](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:156), [tarefa_do_dia.py:332](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:332), [tarefa_do_dia.py:657](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/chamados/services/tarefa_do_dia.py:657), [JUIZES.md:81](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/docs/JUIZES.md:81))

## 18/11 11:55 · sessao 568f4c3dc290 · - · - · origem=Ronald (tmux)

Investigação de `app/folha` enviada ao agente principal, com as quatro consultas diretas, seus filtros e destinos, e a distinção entre a leitura de `DiaPago` e a geração do TXT a partir de `FechamentoMensal`.

## 18/11 11:40 · sessao 2f15d4625b3b · DST-L1-B-MEDIDA · sonda · origem=Code

**A premissa de que o pareador devolve zero para todos esses dias não confere.** Nas 65 linhas `DIA` com `n_bat >= 2`, ele devolve minutos positivos em **58** e zero em **7**. Nos 58 positivos, a ata traz o mesmo número e `DiaPago.minutos_realizados` aparece como `0,0`; nos outros sete, pareador e ata são zero, mas o DiaPago é `0,0` em três e `None` em quatro. Os valores e a contagem abaixo vêm exclusivamente das linhas `DIA` fornecidas; elas não têm arquivo:linha, portanto são marcadas **[não verificado em arquivo]**.

A cópia consultada está no diretório informado, mas o diretório não contém metadados Git para confirmar o hash por `git rev-parse`. **[não verificado]**

## 1. Os três números, por dia

Nas primeiras tabelas, **A = primeiro elemento de `realizado_do_dia`**, **B = `ata_min_real`** e **C = `diapago_min_real`**. Todas as datas são de 2026. Os valores são a transcrição da saída fornecida **[não verificado em arquivo]**.

| Colaborador | Data | A e B, minutos | C |
|---|---:|---:|---:|
| col55 | 11/11 | 68 | 0,0 |
| col61 | 08/11 | 718 | 0,0 |
| col114 | 02/11 | 439 | 0,0 |
| col114 | 15/11 | 499 | 0,0 |
| col123 | 05/11 | 722 | 0,0 |
| col137 | 16/11 | 685 | 0,0 |
| col198 | 08/11 | 721 | 0,0 |
| col203 | 02/11 | 425 | 0,0 |
| col203 | 15/11 | 427 | 0,0 |
| col206 | 09/11 | 501 | 0,0 |
| col206 | 16/11 | 515 | 0,0 |
| col237 | 07/11 | 725 | 0,0 |
| col242 | 02/11 | 423 | 0,0 |
| col270 | 22/10 | 420 | 0,0 |
| col270 | 29/10 | 420 | 0,0 |
| col278 | 02/11 | 423 | 0,0 |
| col278 | 15/11 | 440 | 0,0 |
| col281 | 08/11 | 723 | 0,0 |
| col282 | 01/11 | 724 | 0,0 |
| col328 | 22/10 | 60 | 0,0 |
| col331 | 17/11 | 684 | 0,0 |
| col334 | 16/11 | 209 | 0,0 |
| col364 | 08/11 | 725 | 0,0 |
| col373 | 21/10 | 361 | 0,0 |
| col373 | 23/10 | 362 | 0,0 |
| col398 | 07/11 | 720 | 0,0 |
| col399 | 31/10 | 722 | 0,0 |
| col424 | 24/10 | 715 | 0,0 |
| col444 | 25/10 | 579 | 0,0 |
| col476 | 02/11 | 447 | 0,0 |
| col489 | 09/11 | 419 | 0,0 |
| col617 | 02/11 | 420 | 0,0 |
| col627 | 21/10 | 665 | 0,0 |
| col634 | 27/10 | 689 | 0,0 |
| col696 | 22/10 | 556 | 0,0 |
| col696 | 24/10 | 564 | 0,0 |
| col741 | 15/11 | 718 | 0,0 |
| col835 | 15/11 | 720 | 0,0 |
| col843 | 07/11 | 662 | 0,0 |
| col845 | 06/11 | 567 | 0,0 |
| col849 | 04/11 | 251 | 0,0 |
| col864 | 02/11 | 409 | 0,0 |
| col899 | 01/11 | 122 | 0,0 |
| col913 | 25/10 | 661 | 0,0 |
| col932 | 26/10 | 386 | 0,0 |
| col935 | 13/11 | 660 | 0,0 |
| col936 | 15/11 | 314 | 0,0 |
| col944 | 02/11 | 275 | 0,0 |
| col950 | 15/11 | 629 | 0,0 |
| col985 | 02/11 | 241 | 0,0 |
| col996 | 02/11 | 781 | 0,0 |
| col998 | 26/10 | 366 | 0,0 |
| col1001 | 02/11 | 442 | 0,0 |
| col1001 | 15/11 | 442 | 0,0 |
| col1012 | 12/11 | 721 | 0,0 |
| col1012 | 16/11 | 430 | 0,0 |
| col1045 | 14/11 | 659 | 0,0 |
| col1045 | 16/11 | 658 | 0,0 |

| Colaborador | Data | A | B | C |
|---|---:|---:|---:|---:|
| col130 | 30/10 | 0 | 0 | `None` |
| col227 | 03/11 | 0 | 0 | `None` |
| col250 | 07/11 | 0 | 0 | `None` |
| col451 | 22/10 | 0 | 0 | 0,0 |
| col451 | 15/11 | 0 | 0 | 0,0 |
| col515 | 28/10 | 0 | 0 | `None` |
| col999 | 25/10 | 0 | 0 | 0,0 |

**Onde os números chegam a zero:**

- **A, nos sete casos:** `realizado_dos_turnos` seleciona somente turnos cujo `data_turno` é a data consultada. O total começa em zero; um turno sem fim fechado é ignorado e o total é devolvido. Para esses sete, a saída mostra turno do próprio dia aberto e sem `fim_fechado`; as batidas anteriores pertencem a turno com `data_turno` anterior. Em col451 de 22/10, esse turno anterior também não tem entrada. Não há evidência, nesses sete, de que `bruto <= 0` tenha sido a causa. [turnos.py:609](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:609), [turnos.py:719](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:719), [turnos.py:733](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:733); dados da sonda **[não verificado em arquivo]**.
- **B:** o montador passa para a grade o resultado do pareador, convertendo `sem_turno` em zero; `ata_do_dia` copia esse número para a ata com `int(... or 0)`. Nos sete da tabela, a saída mostra `sem_turno=False` e A=0, de modo que B acompanha esse zero. [utils.py:2045](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/utils.py:2045), [cartorio.py:730](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/cartorio.py:730); dados da sonda **[não verificado em arquivo]**.
- **C:** `por_dia_da_grade` exclui `tipo_dia='folga'` do mapa de realizado; quando outra rubrica cria a linha do dia, `lavrar` a inicia com os campos em `0,0`, sem receber contribuição desse mapa. Portanto esse zero é do **relógio de grade do DiaPago**, e não prova que o pareador foi zero nem que todas as rubricas do DiaPago foram zero. O caminho de retrato também atribui `real.get(d) or 0` às linhas que retrata. Qual caminho gravou cada linha da sonda **[não verificado]**. [dia_pago.py:167](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:167), [dia_pago.py:285](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:285), [dia_pago.py:386](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:386), [dia_pago.py:464](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:464).

O agrupamento observável soma **58 + 7 = 65**: 58 com A/B positivos e C zero; sete com A/B zero por turno do dia sem fim fechado, dos quais três têm C zero e quatro `None`. `None` não deve ser contado como zero. **[não verificado em arquivo: linhas da sonda]**

O fato de `trabalha` e `tipo_dia` não serem sinônimos também aparece em **col741, 15/11**: `trabalha=True`, `tipo_dia='folga'`, A/B=718 e C=0,0. O filtro de C usa `tipo_dia`. [dia_pago.py:386](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:386); linha da sonda **[não verificado em arquivo]**.

## 2. Escritores e leitores

- **A — retorno do pareador:** `realizado_do_dia` delega a `realizado_dos_turnos`; o montador da grade o usa para formar o dia e o serviço de espelho o consulta para preencher `minutos_realizados` e `realizado_h`. A tela de espelho usa `realizado_h` também para mostrar parcial de dia aberto. [turnos.py:774](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:774), [utils.py:2045](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/utils.py:2045), [espelho.py:675](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:675), [espelho.html:187](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/templates/ponto/espelho.html:187).
- **B — ata da célula:** o cartório monta `ata.minutos_realizados` a partir do dia da grade e usa a porta da célula na lavratura; `leitor_celula` lê a ata mesmo quando o tipo é folga. O PDF de validação soma diretamente os minutos de **todas** as atas encontradas, sem filtro de folga: portanto há, sim, leitor de produção de B nesses dias, como referência de conferência. [cartorio.py:730](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/cartorio.py:730), [cartorio.py:1437](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/cartorio.py:1437), [leitor_celula.py:397](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/escala/services/leitor_celula.py:397), [validacao_pdf.py:29](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/validacao_pdf.py:29).
- **C — DiaPago:** o fechamento obtém os mapas da grade e os passa a `lavrar`; o retrato de competência exportada é outro caminho que atualiza esses campos. Para o cálculo de percentual da folha, o fallback da grade seleciona trabalho e ausência, excluindo folga; o espelho usa também outras rubricas do DiaPago para o pago do dia. [fechamento.py:1060](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:1060), [fechamento.py:1200](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:1200), [dia_pago.py:464](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:464), [export.py:1095](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/folha/export.py:1095), [espelho.py:801](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:801).

## 3. A porta de HE

**A porta existente aceita um número positivo fornecido pelo chamador, mas isso sozinho não prova que ela resolveria cada dia pela tela nem qual valor seria pago.** Ela exige estado `AUTORIZADO` ou `NAO`, permissão `autorizar_he`, data válida e minutos convertíveis para inteiro e maiores que zero; para autorizar, exige motivo de pelo menos 10 caracteres. Não há nessa função guarda específica que recuse `tipo_dia='folga'` ou `trabalha=False`. Uma decisão vigente igual retorna `no_op`; caso contrário `_gravar` cria ou atualiza a decisão e tenta registrar trilha, e a porta agenda recálculo por evento. [he.py:37](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/portas/he.py:37), [he.py:44](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/portas/he.py:44), [he.py:159](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/portas/he.py:159), [he.py:195](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/portas/he.py:195).

A view recebe `minutos` do formulário; a porta **não recalcula nem confirma** que sejam os minutos do pareador. A tela de gestão obtém a pendência somando `he_fora_da_janela`; o espelho forma essa lista com pontas e com os dias sem previsto publicados pelo motor, quando a janela de HE está ativa. `estado_por_dia` lê as decisões vigentes, não calcula minutos. A saída fornecida não inclui a lista `he_fora_da_janela`, a configuração da janela, a escala certa ou os minutos do motor para cada caso. [views.py:512](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/views.py:512), [he_pendente_lavrado.py:68](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/he_pendente_lavrado.py:68), [espelho.py:720](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/espelho.py:720), [gestao_he.py:63](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/gestao_he.py:63); dados ausentes **[não verificado]**.

No motor, para o caso abrangido de folga ou feriado, previsto zero, empresa dentro da janela e escala certa quando aplicável, a ausência de autorização faz o período calculado ser bloqueado e publicado como pendência; uma decisão vigente `autorizado` faz o motor sair antes desse bloqueio. Isso **não escreve os minutos da decisão no pagamento**: o motor recalcula os períodos, e o fechamento classifica folga com escala certa em `horas_folga_trabalhada`, feriado aplicável em `horas_extras_100`, e folga sem escala certa em trabalhadas. O recálculo é agendado após commit e pode falhar; o resultado efetivo em cada dia não está na sonda. [motor_calculo_v2.py:1397](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1397), [motor_calculo_v2.py:1440](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1440), [motor_calculo_v2.py:1471](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/motor_calculo_v2.py:1471), [fechamento.py:933](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:933), [fechamento.py:1798](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/fechamento.py:1798).

**Classificação dos 65 dias com o minuto A da tabela como argumento direto da porta:**

- **58 dias positivos**, exatamente as 58 linhas da primeira tabela: passam **a guarda numérica** de `minutos > 0`, condicionados às demais exigências da porta. Se a tela existente oferece esses minutos, se o motor os identifica como pendência e se a autorização produziria pagamento naquele valor: **[não verificado]**. [he.py:175](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/portas/he.py:175).
- **Sete dias zero**, exatamente as sete linhas da segunda tabela: **não passam** com A=0, pois a porta recusa minutos `<= 0`. O motivo do zero do pareador é o turno do dia sem fim fechado, após a seleção por `data_turno`, conforme descrito acima. [he.py:186](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/portas/he.py:186), [turnos.py:719](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/turnos.py:719).

Assim, **58 passam a condição numérica e sete não passam; a utilidade efetiva da porta por dia permanece não verificada** com os dados fornecidos. A decisão autorizada pode liberar o bloqueio do motor nas condições descritas, sem escritor novo, mas não transforma automaticamente `DiaPago.minutos_realizados` de folga no número A: esse campo continua vindo do mapa da grade que exclui folga. [he.py:229](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/portas/he.py:229), [dia_pago.py:386](/home/ronald/codex_ws/d541609996e4a8a54198178991ad0c1a17db1f01/app/ponto/services/dia_pago.py:386).

_6946 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
