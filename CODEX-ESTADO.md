# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 14:49** · publicado em **19/11/2026 14:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 14:49 · sessao a66bbb7c88ef · CADEIA-20-PASSO-1 · leitura · origem=Code

1. **Quem cria as colunas e acomoda a batida**

O caminho começa em `TipoEscala.save()`: sem par completo de intervalo, inclusive em `horarios_por_dia`, ele define `intervalo_modo='duracao'`. `_intervalo_livre()` verifica esse modo — não verifica diretamente a ausência de horários. ([models.py:790](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/models.py:790), [models.py:243](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/models.py:243))

Dentro de `montar_grade_prevista_periodo_por_turno`, a sequência é:

- `_intervalo_livre()` ativa `_tem_dur`; o builder insere **dois slots** `('I', '·S')` e `('I', '·E')` na régua. ([utils.py:1392](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1392), [utils.py:1411](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1411))
- Sem `_hii/_hfi`, recolhe as batidas `is_intervalo=True`, ainda não consumidas, entre os instantes inicial e final do turno; com seus horários, fabrica marcos internos alternando `S/E`. **Esse ramo não exige um par completo de batidas.** ([utils.py:1662](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1662), [utils.py:1720](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1720))
- Chama `_match_marcos(..., turno=None, derivar=False)`, que usa `_alinhar`; depois carimba as células internas com `papel='I'`, `intervalo=True` e ocupa os slots disponíveis. Isso explica uma única `S 01:30` ocupar o primeiro `I`. ([utils.py:1724](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1724), [utils.py:646](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:646), [utils.py:1749](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1749))

Existe ainda um segundo caminho: uma flag **não consumida** pode ocupar um slot `I` vazio na conservação final; essa passagem procura a linha pela **data civil da batida**. ([utils.py:1843](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1843))

**Commit de introdução: [não verificado].** A cópia não contém histórico Git utilizável; `git log` e `git blame` retornaram “not a git repository”. O comentário “28/06” documenta uma atribuição da regra, mas não comprova seu commit. ([models.py:776](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/models.py:776))

2. **Diurno 08–17 com `S 12:00 is_intervalo=True`**

**Vai para a coluna `I`, pelo mesmo caminho**, sob as mesmas condições de cadastro e dia de trabalho. `12:00` está entre as pontas, entra em `_fdt`, gera o marco interno e recebe `papel='I'`; não há teste de “diurno versus noturno” nessa decisão. ([utils.py:1662](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1662), [utils.py:1720](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1720), [utils.py:1752](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1752))

A meia-noite muda os **instantes absolutos**: `encadear_marcos` acrescenta um dia quando o próximo horário não supera o anterior. Assim, `01:30 de d+1` fica dentro de `[22:00 de d, 06:00 de d+1]`; o predicado de inclusão continua igual. ([utils.py:344](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:344), [utils.py:1666](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1666))

3. **Quando uma flag vira órfã**

Há três caminhos relevantes:

- O matcher deixa a batida sem marco e a marca como consumida; a conservação final então a ignora, mesmo havendo `I` vazio. ([utils.py:678](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:678), [utils.py:1845](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1845))
- Uma célula interna casada excede a quantidade de slots `I`. ([utils.py:1753](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1753))
- Uma flag chega não consumida à conservação final, mas sua linha civil não tem slot `I` livre. ([utils.py:1850](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1850))

**Caso mínimo mantendo as duas pontas, sem depender de empates: três batidas** — `E d 22:00`, `S d+1 06:00`, `S d+1 06:30 is_intervalo=True`. A última sobrevive ao dedupe de dez minutos, fica fora do turno previsto — portanto não fabrica marco interno —, mas dentro do envelope do matcher, que vai até `07:30` com `tol_min=90`. Como as pontas já estão ocupadas exatamente, `06:30` sobra e fica consumida. ([utils.py:1355](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1355), [utils.py:1666](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1666), [utils.py:638](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:638), [utils.py:678](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:678))

**Se a sobra precisa estar dentro de 22–06**, um caso por saturação é: `E22`, `S00:30(flag)`, `E01:00(flag)`, `S01:30(flag)`, `S06`. As três internas geram três marcos; somente duas cabem nos slots `I`, então `01:30` vira órfã. São cinco batidas para esse mecanismo, preservando as pontas. ([utils.py:1411](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1411), [utils.py:1721](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1721), [utils.py:1758](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1758))

Nos dois exemplos, a sobra produzida dentro do processamento do turno é guardada no **dia da entrada `d`**, conservando o instante em `d+1`. ([utils.py:1727](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1727), [utils.py:1784](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1784))

4. **Quem consome essa decisão**

- **Cartório:** chama a grade e transforma o dia em ata. A ressalva decisiva é que, quando existem marcos reconhecidos no DNA, `lampadas_do_dia` percorre **as chaves do DNA**; o `I` sintético não vira automaticamente lâmpada. A ata copia a lista de órfãs resultante. ([cartorio.py:1287](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/cartorio.py:1287), [cartorio.py:660](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/cartorio.py:660), [cartorio.py:675](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/cartorio.py:675), [cartorio.py:764](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/cartorio.py:764))
- **Espelho:** normalmente lê a célula lavrada, reconstruindo as células pelas lâmpadas da ata. Também possui caminho pelo builder; o adaptador apresenta suas células e órfãs. ([espelho.py:533](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/espelho.py:533), [leitor_celula.py:114](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/services/leitor_celula.py:114), [espelho.py:503](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/espelho.py:503), [utils.py:1131](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1131))
- **Motor:** recebe, quando há células alimentadas, o mapa de papéis derivado da ata e o passa ao pareador. O papel `Xi` exige duas luzes de pausa; não decorre simplesmente de uma célula bruta com `papel='I'`. ([motor_calculo_v2.py:494](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/motor_calculo_v2.py:494), [turnos.py:1567](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/turnos.py:1567), [turnos.py:1827](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/turnos.py:1827))
- **`detectar_ausencias`:** consulta a entrada pela autoridade de turnos, o veto pela célula e, para fechar chamado existente, as lâmpadas dos marcos cobrados. São leituras indiretas; esses caminhos não consultam o `papel='I'` bruto da grade. ([detectar_ausencias.py:336](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/management/commands/detectar_ausencias.py:336), [detectar_ausencias.py:631](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/management/commands/detectar_ausencias.py:631), [detectar_ausencias.py:404](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/management/commands/detectar_ausencias.py:404))

## 19/11 14:49 · sessao f9c895e56fad · - · - · origem=Ronald (tmux)

ok

## 19/11 14:48 · sessao 989197c67577 · - · - · origem=Ronald (tmux)

Para Q3, **“5 batidas é o mínimo” só vale para excesso de slots internos, mantendo as duas pontas e evitando colisões de instantes**. Há casos menores. (app/escala/utils.py:1664; app/escala/utils.py:1735; app/escala/utils.py:1753)

- **Excesso interno, caso claro:** `E22(d), S01(flag,d+1), E02(flag,d+1), S03(flag,d+1), S06(d+1)`. Os três flags fabricam três marcos internos; os dois primeiros ocupam `I`, `03:00` excede os dois slots e vira órfã do dia `d`, com instante em `d+1`. Pontas reais impedem a reconciliação posterior. (app/escala/utils.py:1720; app/escala/utils.py:1752; app/escala/utils.py:1758; app/escala/utils.py:1784; app/escala/utils.py:2039)

- **Menor, preservando pontas, mas fora do intervalo estrito do turno:** `E22(d), S06:00(d+1), S06:30(flag,d+1)`. O dedupe mantém a última S porque são 30 minutos de diferença; `06:30` não fabrica marco porque está fora de `[22,06]`, mas entra no envelope até `07:30`; o matcher a declara órfã e consumida. A conservação final pula batidas consumidas, mesmo com slots `I` livres. Portanto ela fica órfã em `d`. (app/escala/utils.py:1355; app/escala/utils.py:1666; app/escala/utils.py:641; app/escala/utils.py:678; app/escala/utils.py:680; app/escala/utils.py:1845; app/escala/utils.py:1784)

- **Colisão interna, mantendo pontas:** `E22, S01(flag), E01(sem flag), S06`, com as duas batidas de `01:00` no mesmo instante e nessa ordem na entrada. Tipos diferentes sobrevivem ao dedupe; existe só um marco sintético `01:00`. No empate, o DP atribui o marco à segunda batida e deixa o flag órfão. São quatro batidas. (app/escala/utils.py:1344; app/escala/utils.py:1355; app/escala/utils.py:229; app/escala/utils.py:237; app/escala/utils.py:1664; app/escala/utils.py:1721; app/escala/utils.py:678)

- **Sem exigir pontas reais**, apenas `S01(flag), E01(sem flag)` no mesmo instante e nessa ordem já deixa o flag órfão no matcher; o cluster remove sua atribuição distante à ponta, e `consumed` impede resgate posterior. Não recomendo chamar esse caso de mínimo universal sem delimitar as premissas. (app/escala/utils.py:646; app/escala/utils.py:659; app/escala/utils.py:663; app/escala/utils.py:665; app/escala/utils.py:680; app/escala/utils.py:1845)

Simulei `_alinhar` e `_match_marcos` extraídos por AST em memória, com stubs de construção das células e da guarda usando os valores atuais `90`/`15`; confirmei os resultados locais acima. Execução integral do builder/Django: **[não verificado]**. (app/escala/regua_defesa.py:395; app/escala/orfa_classe.py:3)

## 19/11 14:46 · sessao ae4ce0df198f · - · - · origem=Ronald (tmux)

Para a pergunta 4, os leitores não consomem todos diretamente o dicionário `papel='I'`:

- **Cartório:** monta a grade em `app/ponto/services/cartorio.py:1287`, conta células preenchidas mais órfãs em `app/ponto/services/cartorio.py:1537` e gera a ata em `app/ponto/services/cartorio.py:1549`. A ata copia `orfas` em `app/ponto/services/cartorio.py:764`.
- **Ressalva decisiva:** quando há DNA com marcos reconhecidos, as lâmpadas vêm somente das chaves declaradas do DNA (`app/ponto/services/cartorio.py:639`, `app/ponto/services/cartorio.py:660`, `app/ponto/services/cartorio.py:675`). Assim, uma coluna I sintética não se torna automaticamente lâmpada de intervalo; somente o ramo alternativo percorre todas as células (`app/ponto/services/cartorio.py:735`).
- **Espelho:** normalmente lê a célula lavrada, por `grade_da_celula` (`app/ponto/services/espelho.py:533`); o leitor reconstrói as células a partir das lâmpadas (`app/escala/services/leitor_celula.py:114`, `app/escala/services/leitor_celula.py:129`). O builder é caminho de degradação (`app/ponto/services/espelho.py:503`, `app/ponto/services/espelho.py:574`) e chega à montagem da grade em `app/escala/utils.py:2384`. A apresentação recebe realizado e órfãs em `app/ponto/services/espelho.py:703`, preservados pelo adaptador em `app/escala/utils.py:1128`, `app/escala/utils.py:1131`.
- **Motor:** recebe o papel derivado da ata quando há células alimentadas (`app/ponto/motor_calculo_v2.py:494`, `app/ponto/motor_calculo_v2.py:500`) e passa esse mapa ao pareador (`app/ponto/motor_calculo_v2.py:503`). Não lê diretamente o `papel='I'` da grade; o mapa distingue `X` e `Xi`, e `Xi` exige pelo menos duas luzes de pausa (`app/ponto/turnos.py:1567`, `app/ponto/turnos.py:1827`).
- **detectar_ausencias:** consulta a entrada pela autoridade de turnos (`app/ponto/management/commands/detectar_ausencias.py:336`), cujo pareador recebe o papel da ata (`app/ponto/turnos.py:1997`). Antes da emissão, consulta o veto da célula (`app/ponto/management/commands/detectar_ausencias.py:631`), que lê `trabalha` e `veredito` (`app/ponto/marcos.py:155`). Para fechar chamado, consulta as lâmpadas dos marcos cobrados (`app/ponto/management/commands/detectar_ausencias.py:404`, `app/ponto/services/lastro.py:193`, `app/ponto/services/lastro.py:175`).

Não há, no dado fornecido, comprovação de impacto financeiro nem de comportamento adicional em produção. [não verificado]

## 19/11 14:46 · sessao 6f8405a34ae5 · - · - · origem=Ronald (tmux)

O commit inicial permanece **[não verificado]**: `git log`, `git blame` e `git status` falham com `fatal: not a git repository`; esta cópia não contém `.git`. Não há como fornecer o `log -S`/blame solicitado apenas com a árvore disponível. **[não verificado]** [nao verificado]

A cadeia atual que explica o comportamento é:

- `TipoEscala.save()` deriva `intervalo_modo='duracao'` na ausência de par completo no escalar e em `horarios_por_dia`: `app/escala/models.py:790–794`. `_intervalo_livre()` retorna exatamente esse modo: `app/escala/models.py:243–250`.
- O builder marca `_tem_dur=True` ao encontrar `_intervalo_livre()` e cria dois slots `('I', '·S')`, `('I', '·E')`: `app/escala/utils.py:1392–1411`.
- Sem `_hii` e `_hfi`, recolhe batidas não consumidas com `is_intervalo=True` dentro dos instantes absolutos do turno: `app/escala/utils.py:1662–1666`; fabrica marcos internos alternando `S/E`: `app/escala/utils.py:1720–1722`.
- Após o matcher, transforma as células internas fora da régua em `papel='I'`, `intervalo=True` e ocupa os slots; excedentes tornam-se órfãs: `app/escala/utils.py:1749–1759`.
- Conservação final também tenta inserir flag não consumida em slot I vazio, mas usa a **data civil da batida**; sem slot livre, torna-a órfã: `app/escala/utils.py:1847–1861`.

Há um teste que documenta explicitamente o contrato de absorção: “sem hii/hfi” + flag deve aparecer na grade e não virar órfã, com `E19/S02(flag)/E03(flag)/S07`: `app/escala/tests/test_split_adeilton.py:81–106`.

Comentários atribuem a regra de derivar duração a “28/06”, mas isso não comprova introdução em commit: `app/escala/models.py:776–780`. O documento cita `5f915ff4`, de 15/10, para um refinamento posterior do ramo **sem flag** — exclusão de sobras próximas das pontas — portanto não responde à origem dos slots e da absorção por flag: `app/docs/TICKETS.md:136`.

## 19/11 14:43 · sessao 85d51754a8da · REPLAY-F5-SOL · verificador · origem=Code

Referências aos anexos usam a numeração do texto fornecido, começando na primeira linha de cada anexo.

1. **NÃO BLOQUEIA — fechado o bloqueio específico do `strip()`.** Os dois logs mostram a mesma mutação aplicada: comparação após `strip()` nos dois lados. O velho passa três testes, `rc=0`; o novo falha entre quatro testes, `rc=1`. (`runs_v4.txt:30`, `runs_v4.txt:62`, `runs_v4.txt:76`, `runs_v4.txt:87`) [nao verificado]

   A falha nova é a assertiva de negócio: recebeu `(0, 1)` quando esperava `(1, 1)`, depois de trocar a projeção para `'ato '`. A fixture chegou à comparação; não foi erro de montagem. O segundo cenário, `' ato'`, existe, mas esse log não chega até ele porque a primeira assertiva interrompe o teste. (`runs_v4.txt:39`, `runs_v4.txt:43`, `app/chamados/tests/test_e4c_f5_contador_texto.py:45`, `app/chamados/tests/test_e4c_f5_contador_texto.py:47`)

2. **BLOQUEIA — permanece uma lacuna na prova de comparação literal.** O caso novo exige que espaço inicial e final sejam significativos para o contador vivo, cuja comparação continua direta. Ele não prova sensibilidade a todas as outras diferenças textuais. (`app/chamados/tests/test_e4c_f5_contador_texto.py:40`, `app/chamados/services/vigia_texto_resposta.py:16`)

   **Mutante plausível:** aplicar `casefold()` aos dois operandos. Por leitura, sobreviveria aos casos desse módulo: preserva as diferenças de espaço e mantém `'ato'` diferente de `'projecao'`; nenhum cenário confronta `'ato'` com `'ATO'`. É plausível como normalização indevida, pois os campos aceitam texto sem restrição de caixa. Sobrevivência executada desse mutante: **[não verificado]**. (`app/chamados/tests/test_e4c_f5_contador_texto.py:23`, `app/chamados/tests/test_e4c_f5_contador_texto.py:31`, `app/chamados/tests/test_e4c_f5_contador_texto.py:45`, `app/chamados/models.py:2634`, `app/chamados/models.py:4170`)

   Já “ignorar `None` versus `''`” não oferece contraexemplo plausível nesse caminho atual: o contador já normaliza a projeção com `or ''`, e `leitura()` produz texto vazio para ausência, normalizando também o texto dos atos. (`app/chamados/services/vigia_texto_resposta.py:16`, `app/chamados/models.py:4263`, `app/chamados/models.py:4310`, `app/chamados/models.py:4316`, `app/chamados/models.py:4325`)

3. **NÃO BLOQUEIA — a troca corrige a causa tipada identificável.** O consumidor faz `ch.id in _resp` e `ch.id in _viva`; inteiros provocam `TypeError`, conjuntos permitem essas operações e correspondem ao contrato real de retorno. (`app/chamados/services/fio.py:441`, `app/chamados/services/fio.py:454`, `app/chamados/services/raiox.py:149`, `app/chamados/services/raiox.py:158`, `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:146`)

   Isso permite avançar pelo trecho antes interrompido, mas **não certifica o fio inteiro**: permanecem quatro patches, `com_explicacao` fica no padrão falso e o teste não exige desenlace válido nem ausência de exceção capturada. Os logs desta volta executam somente o módulo do contador; execução corrigida do teste de custo: **[não verificado]**. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:143`, `app/chamados/services/fio.py:195`, `app/chamados/services/fio.py:340`, `app/chamados/services/fio.py:459`, `runs_v4.txt:7`, `runs_v4.txt:16`)

   A métrica permanece “mesmo número de consultas com uma e cinco perguntas”, acompanhada da quantidade mínima de chamadas a `leitura()`. O número absoluto pode mudar: agora pode alcançar `_orf(ch)`, cujo caminho contém consulta `.exists()`, antes interrompido pelo pertencimento em inteiro. Totais efetivos antes/depois: **[não verificado]**. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88`, `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:147`, `app/chamados/services/fio.py:456`, `app/chamados/services/disputa_emissao.py:776`, `app/chamados/services/disputa_emissao.py:810`)

4. **BLOQUEIA — apenas pela cobertura residual de RED descrita no item 2.** O novo RED tem mordida demonstrada contra `strip()`, mas deixa descoberta a normalização por caixa; não cabe concluir fidelidade literal geral. (`runs_v4.txt:39`, `app/chamados/tests/test_e4c_f5_contador_texto.py:40`)

   No delta fornecido, não aparece segundo juiz, segundo escritor fora da porta, relógio fora da borda, derivação paralela, nova ordem de trava ou campo com dois significados: as alterações se limitam ao novo teste e ao retorno da fixture de créditos. Essa conclusão fica restrita ao delta. (`diff_v4.patch:1`, `diff_v4.patch:22`) [nao verificado]

## 19/11 14:35 · sessao 2d4833eae486 · E4C-F5 · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 1, 3a, 3b e 3c**, pelos motivos abaixo. “PROCEDE” significa procedência da objeção ao item. As evidências centrais são o corte posterior sobre texto, a prova de contagem já existente e a separação entre produto e instrumento. (`app/docs/RELATO.md:32`; `app/chamados/tests/test_e4c_f5_contador_texto.py:26-38`; `CLAUDE.md:327-328`.)

Os anexos raspados são a referência fornecida para esta revisão; sua correspondência com arquivos integrais locais, a identidade Git da cópia e os resultados dos logs citados ficam **[não verificado]**. As conclusões sobre código são por inspeção; execução de REDs e mutantes **[não verificado]**.

**1 — PROCEDE: “0815 INTEIRO como está” desconsidera uma decisão posterior registrada.**

1. **Premissas:** conferem as três combinações: app passa a pergunta projetada ao juiz, que lê R/T do argumento; a property passa a leitura do ato; o fio combina instante do ato com texto projetado. (`app/chamados/services/pergunta_no_app.py:83-86`; `app/chamados/juizes.py:1552`; `app/chamados/models.py:3791`; `app/chamados/services/fio.py:189-192`.)
2. **Colisão:** a cópia registra **RESPOSTAS-1911-1000, item 1(B)**: “texto segue da projecao; F5 migra so R/V”. Isso altera a migração de texto prevista no 0815/brief fornecidos; reafirmar aquele desenho integralmente deixa duas instruções incompatíveis. (`app/docs/RELATO.md:20,32`; `app/docs/PROMPTS.md:2615`.)
3. **Literal:** concluir as três voltas em um pouso atende à exigência do 0815 fornecido; o defeito é preservar “como está” um desenho cuja regra de texto foi posteriormente modificada. A própria ordem sofreu transferência documentada da tag para a volta 2. (`app/docs/RELATO.md:32,36-37`.)
4. **Competência/dinheiro/`!`:** a volta 3 alcança escrita e recálculo financeiro por portas existentes; isso já está declarado no 0815 fornecido. Não há prova de movimento proibido efetivo **[não verificado]**. A guarda encontrada não demonstra proteção universal: K6 seleciona metades de intervalo e pula os demais tipos. (`app/chamados/services/validacao.py:743,753-755`; `app/chamados/services/materializacao.py:804-820`; `CLAUDE.md:265-268`.)

**2 — NÃO PROCEDE: a certificação precisa acompanhar a árvore final.**

1. **Premissas:** `Ran 17040 OK`, o conteúdo do merge e a árvore certificada por `suite_m` são **[não verificado]**.
2. **Colisão:** tratar a suíte anterior como sinal é compatível com “A arvore que a suite certificou e a que POUSA”; a certificação não acompanha alterações posteriores de Python. (`CLAUDE.md:320-324`.) [nao verificado]
3. **Literal:** pedir suíte e opus sobre as três voltas não reduz o PRONTO fornecido. A execução e a ordem efetivamente observadas permanecem **[não verificado]**. [nao verificado]
4. **Competência/dinheiro/`!`:** a exigência trata de certificação; não autoriza qualquer exceção às fronteiras de produção. (`CLAUDE.md:265-268,320-328`.)

**3a — PROCEDE: autoridade e prazo não fecham com as instruções preservadas.**

1. **Premissas:** as três fontes conferem, inclusive a mistura explícita do fio. (`app/chamados/services/pergunta_no_app.py:86`; `app/chamados/juizes.py:1552`; `app/chamados/models.py:3791`; `app/chamados/services/fio.py:192`.)
2. **Colisão:** “mesma autoridade” não determina qual leitura prevalece diante do corte **RESPOSTAS-1911-1000 item 1(B)**. Interpretá-la como R/T integralmente do ato contraria “F5 migra so R/V” e o teste que exige texto projetado no fio. (`app/docs/RELATO.md:32`; `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:58-65`.)
3. **Literal:** mudar somente os dois juízes da volta 2 não unifica os três sítios: o fio chama `respondida_sem_texto` com uma leitura híbrida; a property já entrega R/T da leitura do ato. Portanto, “ao fim da volta 2”, junto da manutenção integral daquela divisão de voltas, exige alterações adicionais fora dos dois juízes. O encaixe dessas alterações não está definido no literal fornecido **[não verificado]**. (`app/chamados/services/fio.py:187-192`; `app/chamados/models.py:3791`; `app/chamados/juizes.py:1389,1552`.)
4. **Competência/dinheiro/`!`:** o juiz temporal alimenta vitalidade, e o juiz de resposta também recebe objetos já projetados. Seus efeitos não se limitam aos três exemplos. Movimento real de entregue/exportada **[não verificado]**; não há autorização para tocar seu gravado. (`app/chamados/juizes.py:1404,1423`; `CLAUDE.md:265-268`.)

**3b — PROCEDE: a carga confere; igualdade de contagem não evidencia o RED pedido.**

1. **Premissas:** `contar()` seleciona todas as perguntas por padrão, aplica prefetch e percorre o queryset sem iterador/lote. O vigia chama essa função dentro de `tranca_em_lote`. **51.563 é [não verificado]**, embora conste do comentário. (`app/chamados/services/vigia_texto_resposta.py:3,8-18`; `app/chamados/management/commands/vigiar_fio_morto.py:186-195`.)
2. **Colisão:** preservar os resultados enquanto se altera a carga é compatível com o contrato existente de comparação entre ato e texto corrente. (`app/chamados/services/vigia_texto_resposta.py:1-4,16-18`.)
3. **Literal:** “RED de contagem igual antes e depois”, isoladamente, não demonstra a falha de carregamento: a implementação atual já satisfaz testes de contagens, recortes e múltiplos atos. O teste de custo existente mede consultas, não carregamento integral em memória. (`app/chamados/tests/test_e4c_f5_contador_texto.py:26-38,67-80`.)
4. **Competência/dinheiro/`!`:** o corpo dessa função consulta, compara e conta; não contém escrita desses domínios nem chamada ao motor. (`app/chamados/services/vigia_texto_resposta.py:8-18`.)

**3c — PROCEDE: a alternativa “leitor no ESTADO” colide com o pouso único.** [nao verificado]

1. **Premissas:** o universo é gravado em chave própria, fora de `censo/esperado`; o ESTADO compara esses dois mapas e não considera essa chave. O teste lê o universo. Assim, zero nesse universo não gera, por esse campo, alerta próprio de cegueira. (`app/chamados/management/commands/vigiar_fio_morto.py:275-287`; `bin/fio_estado.py:121-126,191-197`; `app/chamados/tests/test_e4c_f5_contador_texto.py:65`.)
2. **Colisão:** a frase vigente é **“INSTRUMENTO (…) nao pousa com PRODUTO: pouso proprio, depois”**. O próprio leitor do ESTADO declara esse enquadramento. Prender sua alteração à mesma obra e ao único pouso da F5 conflita com essa regra. (`CLAUDE.md:327-328`; `bin/fio_estado.py:4-7`.)
3. **Literal:** a alternativa de retirada não apresenta essa colisão de pouso. Contudo, a L-111 citada trata literalmente de função sem chamador de produção; sua aplicação a uma chave de dados não é literal. (`app/docs/LEIS.md:137`; `app/chamados/management/commands/vigiar_fio_morto.py:287`.)
4. **Competência/dinheiro/`!`:** retirar a chave não equivale a apagar um arquivo usado por produção. Não há exigência automática de `!` por essa retirada; apagar o arquivo inteiro seria outra ação. (`CLAUDE.md:265-268`; `app/chamados/management/commands/vigiar_fio_morto.py:287`.)

**3d — NÃO PROCEDE: o reforço pedido corresponde à fragilidade encontrada.**

1. **Premissas:** o selo **já usa AST**, mas reconhece atributos e chamadas pelo identificador literal `p`. Nesta cópia, seu trecho relevante está em **153–178**, e não integralmente em 146–176. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:146-178`.)
2. **Colisão:** eliminar a dependência do nome da variável preserva a finalidade do selo. Seu limite vigente é R/V; T está expressamente excluído pela lei B. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:154-167`.)
3. **Literal:** a condição de apelido é pertinente: as comparações com `'p'` deixam esse reconhecimento escapar. A sobrevivência de mutante executado é **[não verificado]**. Além disso, `respondida_sem_texto(p)` não tem argumentos posteriores para a inspeção feita pelo selo. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:165-178`.)
4. **Competência/dinheiro/`!`:** o sítio declarado é código de teste, sem escrita de produção nesse trecho. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:153-178`.)

**3e — NÃO PROCEDE: a triagem não determina mudança de regra.**

1. **Premissas:** a comparação do contador é literal; há teste para espaços. Os dois `.lower()` citados atuam em leituras para classificação. A afirmação global de que nenhum escritor indireto transforma caixa permanece **[não verificado]**. (`app/chamados/services/vigia_texto_resposta.py:16`; `app/chamados/tests/test_e4c_f5_contador_texto.py:40-48`; `app/chamados/juizes.py:2041-2047`; `app/chamados/services/validacao.py:1181-1193`.)
2. **Colisão:** triagem motivada não contraria o contrato literal existente. (`app/chamados/services/vigia_texto_resposta.py:16`.)
3. **Literal:** o teste desse módulo não contém o confronto `'Ato'`/`'ato'`; resultado do mutante correspondente **[não verificado]**. (`app/chamados/tests/test_e4c_f5_contador_texto.py:20-80`.)
4. **Competência/dinheiro/`!`:** a exigência fornecida é documental; qualquer alteração futura de regra permanece **[não verificado]** e sujeita ao escopo literal. (`CLAUDE.md:265-268`.)

**4 — NÃO PROCEDE: a exigência de prova é compatível com o portão.**

1. **Premissas:** fechamento do PRONTO, medidas e smoke da árvore inteira são **[não verificado]**.
2. **Colisão:** a linha exigida reproduz o PRONTO do 0815 fornecido; não demonstra redução dele. A regra local também exige passagem de etapa com a porta anterior provada no RELATO. (`CLAUDE.md:282-285`.)
3. **Literal:** “item sem prova = não pede suíte” é uma condição verificável; não equivale a afirmar que a condição já esteja cumprida **[não verificado]**. [nao verificado]
4. **Competência/dinheiro/`!`:** publicar o registro não autoriza movimentos adicionais; permanecem as restrições de competência, dados e motor. (`CLAUDE.md:265-268,285-287`.)

**5 — NÃO PROCEDE como objeção demonstrada; a remissão ao item d tem alcance limitado.**

1. **Premissas:** vagas atuais, estado de FERIADO/C7/B2b e texto integral do aval 1335 são **[não verificado]**.
2. **Colisão:** prioridade de fila 1 encontra respaldo local. O PRE-APROVADO **d** exige agente esperando job próprio, job destacado e necessidade da vaga por obra de fila 1. (`CLAUDE.md:288-294`.)
3. **Literal:** esse d não constitui autorização geral para cancelar qualquer suíte ou opus ativo. A ocorrência concreta de uma ação incompatível é **[não verificado]**. (`CLAUDE.md:289-294`.) [nao verificado]
4. **Competência/dinheiro/`!`:** prioridade e liberação de vaga não suspendem as ações reservadas ao `!`. (`CLAUDE.md:265-268,294-295`.)

**6 — NÃO PROCEDE: preservar trabalho na cópia não é pousar parcialmente.**

1. **Premissas:** os commits efetivamente existentes e a linha de encerramento do dia são **[não verificado]**. [nao verificado]
2. **Colisão:** o requisito fornecido de commit por volta e ausência de pouso parcial mantém a separação entre construir na cópia e integrar/publicar. (`CLAUDE.md:309-313,325-328`.) [nao verificado]
3. **Literal:** o encerramento do dia não prova PRONTO nem substitui certificação da árvore final; a suíte certifica a árvore que pousa. (`CLAUDE.md:320-324`.) [nao verificado]
4. **Competência/dinheiro/`!`:** commit na cópia não autoriza alterar gravado exportado, dados de escala/vínculo, motor ou apagar arquivo de produção. (`CLAUDE.md:265-268`.) [nao verificado]

## 19/11 14:34 · sessao aae86a382a75 · - · - · origem=Ronald (tmux)

**Competência/dinheiro/`!`:**

- O item 1 inclui portas que podem mover dinheiro por efeito: `validar_pergunta` carimba e chama recalculo por evento (`app/chamados/services/validacao.py:743`, `:753`); o evento chama `recalcular_fechamento_mes` (`app/ponto/services/fechamento.py:1809`) e agenda por `on_commit` (`:1824`). Isso já está declarado nas medidas e restrições do 0815 fornecido; movimento real de produção **[não verificado]**.
- A presença de K6 não prova cerca universal de entregue/exportada: a composição usa `_k6_que_barra` (`app/chamados/services/validacao.py:225`), que consulta só metades de intervalo (`app/chamados/services/materializacao.py:804`, `:808`) e pula as outras (`:819`). Com `somente_entregue=True`, o restante retorna sem barreira (`:841`). Casos concretos que mudariam **[não verificado]**.
- A correção escreve resposta antes de chamar validação (`app/chamados/services/acoes_disputa.py:375`, `:383`); a recusa grava motivo e reabre a resposta (`app/chamados/services/validacao.py:922`, `:925`); a devolução limpa V/VP e registra ato (`:273`, `:276`, `:279`). Não presumir que a guarda da validação cobre cada chamada direta.
- `!` exige tocar gravado exportado, dado de escala/vínculo, apagar/voltar arquivo usado em produção, regra fora do pedido, atalho ou `motor_calculo_v2.py` (`CLAUDE.md:265`). O aval 1435 não determina esses atos fora do escopo; execução futura **[não verificado]**.
- Retirar contador/campo não equivale a apagar arquivo de produção: a exigência literal é apagar/voltar **arquivo** (`CLAUDE.md:266`).

**3c: colisão documental confirmada.**

O leitor do ESTADO é instrumento declarado no próprio arquivo: `bin/fio_estado.py:4` a `:7` diz que o produto pousou antes e que esse leitor pousa “em commit proprio, depois do produto”. A regra vigente exige instrumento separado de produto (`CLAUDE.md:327`, `app/docs/LEIS.md:131`). Portanto, a alternativa “ganha leitor no ESTADO”, presa à F5 inteira em um pouso, tem colisão literal; não é automaticamente item de `!` (`CLAUDE.md:265`).

**Aval adicional citado pelo laudo:**

Há registro local de RESPOSTAS-1911-1000 item 1(B): texto segue da projeção, F5 migra somente R/V e nasce contador de divergência (`app/docs/RELATO.md:32`, `app/docs/PROMPTS.md:2615`). O RELATO registra explicitamente instante do ato e texto da projeção (`app/docs/RELATO.md:20`). “Uma fonte” não pode ser interpretada automaticamente como migrar T para ato; o literal integral desse aval **[não verificado]**.

**5: vagas.**

PRE-APROVADO(d) exige job destacado **e** obra de fila 1 precisando da vaga (`CLAUDE.md:292`); ordem geral é fila 1, negócio, instrumento (`:291`). O 1335 citado e o estado atual de B2b/FERIADO/C7 **[não verificado]**.

## 19/11 14:34 · sessao 9e8384c60c96 · - · - · origem=Ronald (tmux)

Itens auditados, sem execução de testes nem acesso a banco/log: [nao verificado]

**1 — PROCEDE quanto à compatibilidade literal, com premissa técnica confirmada.**

- As três combinações existem: app passa a pergunta crua ao juiz (`app/chamados/services/pergunta_no_app.py:83-86`), cujo corpo lê R/T do argumento (`app/chamados/juizes.py:1552`); property passa `DecisaoPergunta.leitura(self)` (`app/chamados/models.py:3791`); fio combina R da leitura com T da projeção (`app/chamados/services/fio.py:189-192`).
- Há aval posterior ao 0815 registrado na cópia: **RESPOSTAS-1911-1000 item 1(B)** determina “texto segue da projecao; F5 migra so R/V” (`app/docs/RELATO.md:32`); a linha explicativa registra devolução do texto à projeção e manutenção do instante no ato (`app/docs/RELATO.md:20`). Assim, reafirmar 0815 “INTEIRO como está”, sem tratar esse corte posterior, deixa conflito documental relevante (`app/docs/RELATO.md:20`, `app/docs/RELATO.md:32`).
- A cópia documenta tag retirada da volta 1 e transferida à volta 2, embora a divisão original a tenha colocado na volta 1 (`app/docs/RELATO.md:36-37`). A tag ainda lê texto cru em `app/chamados/templatetags/chamados_tags.py:166`.
- Hash real, conteúdo dos commits e pouso: [não verificado]. A árvore não contém `.git` na raiz; não há confirmação Git independente [não verificado]. [nao verificado]
- Alcance financeiro/exportado deste item depende da continuação ainda não realizada [não verificado].

**3a — PROCEDE por conflito de autoridade não resolvido no literal.**

- A premissa das três combinações confere nos mesmos endereços acima (`app/chamados/services/pergunta_no_app.py:86`, `app/chamados/models.py:3791`, `app/chamados/services/fio.py:192`).
- “Mesma autoridade” não define literalmente qual par deve prevalecer diante do corte B: texto da projeção é exigência registrada do **RESPOSTAS-1911-1000 item 1(B)** (`app/docs/RELATO.md:20`, `app/docs/RELATO.md:32`), enquanto a testemunha `leitura` devolve R/T da sequência com passivo (`app/chamados/models.py:4295-4326`).
- Migrar o fio para R/T do ato contraria também a expectativa atual explícita do teste “Lei B”, que exige continuar esperando quando ato tem texto e projeção tem texto vazio (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:58-65`).
- A condição “fim da volta 2” não exige necessariamente antecipar portas da volta 3: os três sítios citados são partição, property e espera (`app/chamados/services/pergunta_no_app.py:80-90`, `app/chamados/models.py:3785-3791`, `app/chamados/services/fio.py:178-192`). Contudo, a ordem exata dos reparos ainda não foi implementada [não verificado].
- L-130 exige um juiz para todas as finalidades, mas não resolve sozinho a contradição específica entre ato e texto corrente (`app/docs/LEIS.md:151`).

**3d — NÃO PROCEDE como objeção à exigência do item; a premissa da fragilidade confere.**

- O selo já usa AST; seu intervalo real nesta cópia é **153–178**, enquanto 146–150 pertence ao teste de custo do fio (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:146-160`).
- Ele restringe reconhecimento ao identificador literal `p`, tanto em atributos quanto no primeiro argumento de chamadas (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:165-167`). Logo, um apelido escapa a essas condições por inspeção estática dessas linhas (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:165-178`).
- `respondida_sem_texto(p)` não contém argumento posterior a inspecionar e o selo não percorre o corpo do predicado chamado (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:168-178`); o predicado efetivamente lê R/T crus do argumento (`app/chamados/juizes.py:1531-1534`).
- O caso existente com R projetada ausente mantém T projetado preenchido; não é o caso R=None/T='' mencionado no laudo (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:42-49`).
- O item pede reforço do selo e mutante; resultado de mutante executado [não verificado]. Não há colisão documental demonstrada por esse reforço [não verificado].
- Atenção ao limite já estabelecido: selo atual varre apenas R/V e exclui T expressamente por lei B (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:156`).

**3e — NÃO PROCEDE como objeção à triagem pedida; a comparação literal confere.**

- O contador compara texto por `!=`, sem normalização (`app/chamados/services/vigia_texto_resposta.py:16`).
- Há teste para diferenças apenas de espaços, mas nenhum caso `'Ato'`/`'ato'` nesse módulo de testes (`app/chamados/tests/test_e4c_f5_contador_texto.py:20-80`).
- Os dois `.lower()` citados pelo laudo são leituras para classificação, usando variável local `resp`, sem gravar os campos nesses trechos (`app/chamados/juizes.py:2041-2047`, `app/chamados/services/validacao.py:1181-1193`).
- Os escritores citados zeram ou copiam texto: `app/chamados/models.py:1575`, `app/chamados/models.py:1702-1703`, `app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/disputa_emissao.py:1617`, `app/chamados/services/disputa_emissao.py:1682-1686`, `app/chamados/services/validacao.py:440`. Exclusividade global de todos os escritores e ausência absoluta de transformação por caminho indireto [não verificado].
- “Triagem com motivo” não obriga mudar comparação nem criar regra de caixa; implementação futura e impacto fora do declarado [não verificado].

## 19/11 14:33 · sessao cb05fce58f6e · - · - · origem=Ronald (tmux)

**3b — PROCEDE, por condição de RED que não fecha literalmente.**

1. A carga integral confere: `contar()` seleciona todas as perguntas por padrão, aplica `prefetch_related('decisoes')` e percorre o queryset diretamente, sem iterador/lote (`app/chamados/services/vigia_texto_resposta.py:8-18`). A chamada fica dentro de `tranca_em_lote` (`app/chamados/management/commands/vigiar_fio_morto.py:186-195`), e há elo configurado na corrente cron (`app/config/crons.py:1737`). As **51.563 linhas são [não verificado]**: aparecem somente como declaração no comentário (`app/chamados/services/vigia_texto_resposta.py:3`).
2. Mudar a forma da carga preservando a comparação não colide, por si, com a lei registrada: ela estabelece texto da projeção, ato como testemunha (`app/chamados/services/vigia_texto_resposta.py:1-4`; `app/docs/RELATO.md:20`).
3. “RED de contagem igual antes e depois” mistura conservação com falha anterior: o código atual já retorna `(divergentes, universo)` e os testes existentes já exigem esses totais para conjuntos com/sem divergência, recorte e múltiplos atos (`app/chamados/services/vigia_texto_resposta.py:16-18`; `app/chamados/tests/test_e4c_f5_contador_texto.py:26-38`). A igualdade numérica, isolada, não demonstra o defeito de carga nem uma falha anterior; o teste de custo existente cobra quantidade de consultas, não pico de memória/execução em lotes (`app/chamados/tests/test_e4c_f5_contador_texto.py:67-80`). Não há RED novo executado nesta revisão [não verificado].
4. O corpo de `contar` só consulta e conta; não contém escrita em domínio/dinheiro/escala/vínculo nem acesso ao motor (`app/chamados/services/vigia_texto_resposta.py:8-18`). Ausência de efeitos globais de toda execução em produção [não verificado].

**3c — PROCEDE, por colisão da alternativa “leitor no ESTADO” com o pouso único.** [nao verificado]

1. A chave é escrita fora de `censo/esperado` (`app/chamados/management/commands/vigiar_fio_morto.py:269-287`). Busca textual integral encontrou somente esse escritor e o assert de teste (`app/chamados/tests/test_e4c_f5_contador_texto.py:65`); o leitor ESTADO compara apenas as chaves de `esperado` contra `censo` e não lê o universo textual (`bin/fio_estado.py:121-126`, `:143-162`). Portanto, zero divergências com universo textual zero não produz alerta por esse universo; isso é independente das outras condições de validade do carimbo (`bin/fio_estado.py:74-95`, `:125-141`, `:191-197`). **A referência `bin/fio_estado.py:66-67` do laudo não comprova o alegado: contém `TETO_H`, não o contador.**
2. A frase vigente é: **“INSTRUMENTO (`bin/`, selo de host, portao do push) nao pousa com PRODUTO: pouso proprio, depois”** (`CLAUDE.md:327-328`; também `app/docs/LEIS.md:131`). O próprio leitor declara que pousa como instrumento em commit próprio depois do produto (`bin/fio_estado.py:4-7`). Assim, exigir esse leitor dentro da MESMA obra, antes do opus final e sob UM pouso, não fecha sem exceção declarada; o 1435 diz “Nenhuma lei nova” e preserva o 0815 integralmente (anexo raspado; inexistência de arquivo local do aval [não verificado]).
3. A alternativa “sai no mesmo commit” tem fundamento apenas analógico na L-111: o literal da lei trata **função** sem chamador de produção, não qualquer chave informativa (`app/docs/LEIS.md:137`; `CLAUDE.md:191`). A chave aqui é um dado gravado e o total continua sendo impresso na saída (`app/chamados/management/commands/vigiar_fio_morto.py:228-229`, `:287`). Não afirmar que L-111 manda literalmente apagar este campo.
4. Apagar essa chave não equivale, no código apresentado, a apagar arquivo usado por produção; modificar o leitor host é instrumento (`app/chamados/management/commands/vigiar_fio_morto.py:287`; `bin/fio_estado.py:6-7`). O `!` exige apagar/voltar ao HEAD **arquivo** de produção, mexer no gravado exportado, dado de escala/vínculo ou motor (`CLAUDE.md:265-268`). Efeito futuro não declarado [não verificado].

**Evidências extras importantes para sua consolidação:**

- O aval **RESPOSTAS-1911-1000** não veio como anexo literal, mas a cópia registra seu conteúdo: **“F5 migra so R/V”** (`app/docs/PROMPTS.md:2615`), **“TEXTO da projecao … instante segue do ato”** (`app/docs/RELATO.md:20`). A unificação do texto no ato foi adiada para a E4D, quando morrer a projeção (`app/docs/BACKLOG.md:2021`). Isso importa especialmente para 3a: a escolha de autoridade não é livre nem resolvida somente por exigir “mesma autoridade”.
- O contador de divergência já tem leitor genérico pelo `esperado` (`app/chamados/management/commands/vigiar_fio_morto.py:108`, `:277`; `bin/fio_estado.py:125-126`, `:191-194`); a lacuna é especificamente seu **universo**, não ausência completa de leitor do contador.
- A linha de fila local exige contador com leitor ESTADO e `>0 = PERGUNTA no topo` e o atribui ao RESPOSTAS-1911-1000 item 1 (`app/docs/BACKLOG.md:2026`); não demonstra que universo zero já seja tratado.

## 19/11 14:08 · sessao 01b1b0d2b971 · CADEIA-20-PASSO-1 · construtor · origem=Code

Implementei as alterações de P1 a P5 na cópia. A execução dos testes e a morte dos mutantes permanecem **[não verificado]**; não rodei a suíte nem acessei banco. A análise sintática dos sete arquivos alterados passou com `ast.parse`. [escala/tests/test_cadeia20_mutantes.py:1] [nao verificado]

**Inventário**

- P1 e P5: a medida conta células desalinhadas por quantidade ou ordem de horas e entradas sem razão e sem falha nomeada. O booleano também exige zero células desalinhadas. [cartorio.py:1001](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1001)
- P2: removi `data_turno` e `fuso` da assinatura. Antes e depois, os chamadores encontrados são [cartorio.py:991](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:991), [test_cadeia20_coletor.py:15](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_coletor.py:15) e [test_razao_da_sobra.py:29](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:29); a assinatura está em [orfa_classe.py:30](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:30).
- P3: o teste captura a primeira lavra de cada braço, com objetos recarregados e rollback; compara `SELECT` por causa das gravações dependentes da diferença de estado. [test_cadeia20_lavra.py:207](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:207) [nao verificado]
- P4: acrescentei os casos de sobra noturna e saída cruzada, com a resposta esperada nos docstrings. Sua passagem e a necessidade eventual de cura adicional estão **[não verificado]**. [test_cadeia20_costura.py:110](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_costura.py:110)
- REDs de P1 e P5: [test_cadeia20_lavra.py:149](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:149). Mutantes novos m15 a m19: [test_cadeia20_mutantes.py:61](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_mutantes.py:61).

Para a sessão executar os REDs e o teto, com a configuração Django e banco de teste próprios da sessão **[não verificado]**: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_lavra.LavraDasSobrasTest.test_RED_medida_compensacao_e_ordem_por_celula
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_lavra.LavraDasSobrasTest.test_RED_medida_sem_razao_sem_falha
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_lavra.TetoConsultaLavraTest.test_mesmo_acrescimo_em_n_e_2n_dias
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_costura.CosturaTest.test_RED_sobra_depois_da_meia_noite_no_dia_do_turno
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_costura.CosturaTest.test_RED_saida_cruzada_remove_mesma_ocorrencia
```

Comandos individuais dos mutantes novos **[não verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m15_instante_saida_cruzada_nao_removido python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m16_ignora_desalinhamento python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m17_ignora_falta_de_falha python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m18_ignora_hora python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m19_ignora_desalinhamento_no_booleano python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
```

## 19/11 14:08 · sessao 35d98b4515e0 · E4C-F5 · censo · origem=Code

O censo abaixo classifica os **cinco achados numerados**. O item “Casefold” não entra na contagem porque não está numerado; ele foi explicitamente tratado em `verif4/r.txt`, item 2 [linha do anexo não verificada].

Os anexos foram fornecidos no texto da pergunta, sem numeração de linhas, e não foram localizados como arquivos na cópia. Por isso, identifico cada trecho pelo caminho e pelo item do anexo; a **linha física de cada citação dos anexos está [não verificada]**. A cópia examinada também não contém o contador nem o teste novo mencionados no opus, de modo que sua correspondência com a versão final permanece [não verificada]. [nao verificado]

Para distinguir **(a)** de **(d)**, uso **(a)** quando uma resposta viu parte do tema, mas não identificou o achado completo; uso **(d)** quando a pergunta cobria a lacuna específica e nenhuma resposta ou triagem fornecida a identificou.

1. **BLOQUEIA — classe (a), falha do Codex.** A pergunta de verificação incluía pré-requisito e pouso; a resposta deixou esses pontos sem verificação, embora a revisão anterior tivesse registrado as exigências das três voltas e das provas. O dado fornecido não mostra uma triagem que tenha descartado esse achado específico [não verificado]. [nao verificado]

   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif2/q.txt`, item 6: “pouso antes do pre-requisito”; “Alguma presente no diff ou nos escritores/leitores dos campos que ele toca, mesmo fora do diff?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif2/r.txt`, item 6: “Pré-requisitos e pouso real também são **[não verificado]**.”
   - Evidência de que o Codex já havia visto as condições, `logs/ESPINHA-BRIEFS/F5/c3/etapaA2/r.txt`, §7: “três voltas verdes na ordem 1/2/3”, “medidas (a), (b) e (c) no relato” e “smoke do app — badge e pendentes — nas duas cascas”.

   A ausência das medidas e do smoke no RELATO é uma conclusão do opus fornecido; sua conferência independente no RELATO está [não verificado].

2. **Três combinações de fonte — classe (a), falha do Codex.** O verificador viu a diferença entre a API e o leitor alterado, mas não identificou na resposta a combinação de três fontes descrita no achado. A medição do impacto em produção está [não verificado].

   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/q.txt`, item 2: “QUEM ESCREVE e QUEM LE cada campo/valor que o diff le ou grava”; “todos supoem o MESMO significado?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/r.txt`, item 1: “a API ainda expõe `respondida_em` da projeção no JSON; o botão de correção, porém, consulta o leitor alterado.”
   - Limite literal da resposta, no item 2 do mesmo arquivo: “A enumeração exaustiva de **todos** os escritores e leitores fora desse escopo, e a uniformidade de seus fusos e significados, permanecem **[não verificado]**.”

   A triagem de `verif1` tratou de outra divisão de fontes, na tag, e a considerou procedente: “tag combina fontes”; “a tag SAI da volta 1 e entra na volta 2” (`logs/ESPINHA-BRIEFS/F5/v1/verif1/triagem.txt`, item 1). Ela não é um descarte do achado das três combinações [não verificado].

3. **Carga integral do contador em memória — classe (e), não perguntado.** As perguntas de custo citadas exigiam verificar consultas e N+1; não pediam o volume mantido em memória nem uso de `iterator`. Tempo e memória em produção estão [não verificado].

   - Pergunta literal mais próxima, `logs/ESPINHA-BRIEFS/F5/v1/verif3/q.txt`, item 5: “Custo: cada chamador real dos leitores tocados traz `decisoes` prefetchado? Consulta por linha (N+1) em algum caminho de tela/lote?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/r.txt`, item 5: “Cobrança, fio, histórico e contador carregam `decisoes` em lote” e “custo total e outros N+1 das telas ou da frota são **[não verificado]**.”
   - Quanto à ausência de pergunta sobre memória em todas as perguntas fornecidas: [não verificado] por linha física dos anexos.

4. **Universo sem leitor e zero sobre universo vazio — classe (e), não perguntado.** Foi pedida a publicação do universo e quem lia o contador de divergência; os trechos citados não pediam quem lia a chave de universo nem o comportamento do ESTADO para universo vazio.

   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/q3.txt`, item 1: “`reg['divergencia_texto_resposta_universo'] >= 1`.”
   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/q.txt`, item 3: “Quem le o contador (ESTADO) e o que acontece com valor > 0?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/r.txt`, item 2: “o vigia grava o resultado em `censo` e zero em `esperado`; o leitor do ESTADO compara as duas entradas genericamente.”
   - O achado específico sobre não haver leitor do universo e sobre universo vazio não aparece nesses trechos de resposta; sua ausência nas demais respostas fornecidas está [não verificado] por linha física dos anexos.

5. **Selo limitado à variável `p` e caso ausente — classe (d), modo comum.** A pergunta pedia mutantes plausíveis de cada sítio e a resposta apontou outro mutante, do contador. O dado de perguntas, respostas e triagens fornecido não identifica a limitação do selo a `p` nem o caso específico descrito pelo opus [não verificado] por linha física dos anexos.

   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/q.txt`, item 4: “Ha mutante plausivel de cada sitio tocado que nenhum teste morde?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/r.txt`, item 4: “**Mutante plausível sem mordida demonstrada:** comparar os textos após `strip()` no contador”.
   - Pergunta anterior sobre a forma do selo, `logs/ESPINHA-BRIEFS/F5/v1/verif1/q.txt`, item 5: “Apelido (`getattr(p, campo)` com campo variavel, `p.__dict__`, `values()`) escapa?”
   - Resposta literal correspondente, `logs/ESPINHA-BRIEFS/F5/v1/verif1/r.txt`, item 5: “Ele detecta atributos diretos do argumento e certas strings literais em chamadas; portanto `getattr(p, campo)` com nome variável, `p.__dict__` e `values()` com campo variável escapam dessa inspeção.”

| Achado | Classe |
|---|---|
| 1. Escopo do aval e pouso | (a) Codex | [nao verificado]
| 2. Três combinações de fonte | (a) Codex |
| 3. Memória do contador | (e) Não perguntado |
| 4. Leitura do universo vazio | (e) Não perguntado |
| 5. Selo e caso ausente | (d) Modo comum |

**Contagem:** (a) 2; (b) 0; (c) 0; (d) 1; (e) 2. Essa contagem é sobre os cinco achados numerados do anexo `opus_final.r`; numeração física do anexo [não verificada].

## 19/11 14:07 · sessao f279c43dcb2d · CADEIA-20-PASSO-1 · sonda · origem=Code

# app/ponto/management/commands/processar_cartorio.py:33
# app/ponto/services/cartorio.py:959
# app/ponto/services/cartorio.py:2002
# app/ponto/services/cartorio.py::medir_razoes_sobras [nao verificado]
# app/ponto/services/cxr_na_lista.py:152
# app/ponto/services/espelho.py:51
# app/ponto/services/espelho.py:1210
# app/chamados/reconciliador.py:465
# app/core/observ.py:94
# app/core/models.py:285
# app/chamados/models.py:33
# app/escala/models.py:1635

import contextlib
import copy
import hashlib
import inspect
import io
import json
from datetime import date, datetime
from zoneinfo import ZoneInfo

from django.db import transaction
from django.utils import timezone
from freezegun import freeze_time

from colaboradores.models import Colaborador
from core.models import LogAuditoria
from escala.models import CelulaDia
from ponto.management.commands.detectar_ausencias import Command as Emissor
from ponto.services import cartorio
from ponto.services.cxr_na_lista import ultimo_lavrado
from ponto.services.espelho import espelho_do_colab
import chamados.models as modelos_chamados


INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
HOJE = FIM
FUSO = ZoneInfo("America/Sao_Paulo")
AGORA = datetime(2026, 11, 20, 23, 59, tzinfo=FUSO)
CAMPOS = (
    "minutos_trabalhados",
    "minutos_atraso",
    "minutos_saida_antecipada",
    "minutos_intrajornada_indenizada",
    "minutos_extra",
    "minutos_noturnos_legais",
)
ACOES_RETENCAO = (
    "retratacao_retida_irma_orfa",
    "retratacao_retida_competencia_entregue",
    "retratacao_retida_justificativa_alheia",
)


class DesfazerSonda(Exception):
    pass


def json_canonico(valor):
    return json.dumps(
        valor, sort_keys=True, ensure_ascii=True, separators=(",", ":"), default=str
    )


def fotografia(queryset):
    return {
        linha["pk"]: json_canonico(linha)
        for linha in queryset.order_by().values("pk", *[
            campo.attname for campo in queryset.model._meta.concrete_fields
            if not campo.primary_key
        ])
    }


def variacao(antes, depois):
    return {
        "antes": len(antes),
        "depois": len(depois),
        "criados": len(depois.keys() - antes.keys()),
        "alterados": sum(
            antes[pk] != depois[pk] for pk in antes.keys() & depois.keys()
        ),
        "excluidos": len(antes.keys() - depois.keys()),
    }


def somar_medida(total, medida):
    for chave, valor in medida.items():
        if chave == "contador_igual_universo":
            total[chave] = total.get(chave, True) and valor
        elif isinstance(valor, dict):
            destino = total.setdefault(chave, {})
            for subchave, numero in valor.items():
                destino[subchave] = destino.get(subchave, 0) + numero
        else:
            total[chave] = total.get(chave, 0) + valor


def main():
    linhas = []
    erros = []
    desfeito = False
    concluiu_transacao = False
    primeira_celula = None
    estado_original = None

    modelo_chamado = getattr(modelos_chamados, "Chamado", None)
    if modelo_chamado is None:
        modelo_chamado = modelos_chamados.ChamadoColaborador

    with freeze_time(AGORA), timezone.override(FUSO):
        try:
            with transaction.atomic():
                universo = CelulaDia.objects.filter(
                    colaborador__empresa_id__in=(2, 3, 4),
                    data__gte=INI,
                    data__lte=FIM,
                )
                ids = list(
                    universo.order_by()
                    .values_list("colaborador_id", flat=True)
                    .distinct()
                    .order_by("colaborador_id")
                )
                quantidade_celulas = universo.count()
                primeira_celula = universo.order_by("pk").values_list(
                    "pk", flat=True
                ).first()
                if primeira_celula is not None:
                    estado_original = fotografia(
                        CelulaDia.objects.filter(pk=primeira_celula)
                    )

                chamados_antes = fotografia(modelo_chamado.objects.all())
                retencoes = LogAuditoria.objects.filter(
                    acao__in=ACOES_RETENCAO, modelo="ChamadoColaborador"
                )
                retencoes_antes = fotografia(retencoes)

                lavra = ultimo_lavrado() if ids else None
                aceita_lavra = (
                    "lavra" in inspect.signature(cartorio.julgar_colab).parameters
                )
                medir = getattr(cartorio, "medir_razoes_sobras", None)
                medida_total = {}
                totais = {campo: 0 for campo in CAMPOS}
                totais.update(periodos=0, celulas=0, orfas=0)
                atas_primeira = {}
                ids_concluidos = []

                def relavrar(colab, celulas):
                    kwargs = dict(
                        hoje=HOJE,
                        agora=AGORA,
                        apply_=True,
                        emissor=Emissor(),
                        out=cartorio.contadores(),
                        prefix="",
                        write=lambda mensagem: None,
                        forcar=True,
                    )
                    if aceita_lavra:
                        kwargs["lavra"] = lavra
                    cartorio.julgar_colab(colab, celulas, **kwargs)

                for cid in ids:
                    try:
                        with transaction.atomic(), contextlib.redirect_stdout(io.StringIO()):
                            colab = Colaborador.objects.get(pk=cid)
                            celulas_passadas = list(
                                CelulaDia.objects.filter(
                                    colaborador=colab,
                                    data__gte=INI,
                                    data__lte=FIM,
                                    data__lt=HOJE,
                                ).order_by("data", "pk")
                            )
                            if celulas_passadas:
                                relavrar(colab, celulas_passadas)

                            celulas = list(
                                CelulaDia.objects.filter(
                                    colaborador=colab, data__gte=INI, data__lte=FIM
                                ).order_by("data", "pk")
                            )
                            espelho = espelho_do_colab(
                                colab, HOJE, INI, FIM, agora=AGORA
                            )
                            resultado = espelho["resultado"]
                            periodos = list(resultado.periodos or []) + list(
                                resultado.periodos_ft or []
                            )
                            periodos = [
                                p for p in periodos
                                if INI <= timezone.localtime(p.entrada).date() <= FIM
                            ]
                            dinheiro = {
                                campo: sum(getattr(p, campo) or 0 for p in periodos)
                                for campo in CAMPOS
                            }
                            dinheiro["periodos"] = len(periodos)
                            orfas = []
                            atas = {}
                            for celula in celulas:
                                ata = celula.ata or {}
                                lista = ata.get("orfas") or []
                                if not isinstance(lista, list):
                                    raise TypeError("orfas")
                                orfas.append([celula.pk, lista, len(lista)])
                                atas[celula.pk] = copy.deepcopy(celula.ata)

                            medida = medir(celulas) if medir is not None else None

                        dinheiro["orfas_sha1"] = hashlib.sha1(
                            json_canonico(orfas).encode("utf-8")
                        ).hexdigest()
                        linhas.append("DF {} {}".format(cid, json_canonico(dinheiro)))
                        for campo in CAMPOS:
                            totais[campo] += dinheiro[campo]
                        totais["periodos"] += dinheiro["periodos"]
                        totais["celulas"] += len(orfas)
                        totais["orfas"] += sum(item[2] for item in orfas)
                        atas_primeira.update(atas)
                        ids_concluidos.append(cid)
                        if medida is not None:
                            somar_medida(medida_total, medida)
                    except Exception as exc:
                        erros.append((cid, type(exc).__name__))

                mudaram = 0
                for cid in ids_concluidos:
                    try:
                        with transaction.atomic(), contextlib.redirect_stdout(io.StringIO()):
                            colab = Colaborador.objects.get(pk=cid)
                            celulas_passadas = list(
                                CelulaDia.objects.filter(
                                    colaborador=colab,
                                    data__gte=INI,
                                    data__lte=FIM,
                                    data__lt=HOJE,
                                ).order_by("data", "pk")
                            )
                            if celulas_passadas:
                                relavrar(colab, celulas_passadas)
                            atuais = list(
                                CelulaDia.objects.filter(
                                    colaborador=colab, data__gte=INI, data__lte=FIM
                                ).values_list("pk", "ata")
                            )
                            mudancas_colab = sum(
                                pk not in atas_primeira or atas_primeira[pk] != ata
                                for pk, ata in atuais
                            )
                            pks_atuais = {pk for pk, _ in atuais}
                            pks_primeira = {
                                pk for pk, _ in CelulaDia.objects.filter(
                                    colaborador=colab, data__gte=INI, data__lte=FIM
                                ).values_list("pk", "ata")
                                if pk in atas_primeira
                            }
                            mudancas_colab += len(pks_primeira - pks_atuais)
                        mudaram += mudancas_colab
                    except Exception as exc:
                        erros.append((cid, type(exc).__name__))

                chamados_depois = fotografia(modelo_chamado.objects.all())
                retencoes_depois = fotografia(retencoes)

                linhas.append("TOTAL " + json_canonico(totais))
                if medir is None:
                    linhas.append("MEDIDA ausente")
                else:
                    if not ids_concluidos:
                        medida_total = medir([])
                    linhas.append("MEDIDA " + json_canonico(medida_total))
                linhas.append(
                    "IDEMP celulas_que_mudaram_na_2a={}".format(mudaram)
                )
                linhas.append(
                    "RETENCAO antes={} depois={}".format(
                        json_canonico({
                            "chamados": len(chamados_antes),
                            "retencoes": len(retencoes_antes),
                        }),
                        json_canonico({
                            "chamados": variacao(chamados_antes, chamados_depois),
                            "retencoes": variacao(retencoes_antes, retencoes_depois),
                        }),
                    )
                )
                linhas.append(
                    "UNIVERSO colabs={} celulas={}".format(
                        len(ids), quantidade_celulas
                    )
                )
                concluiu_transacao = True
                raise DesfazerSonda()
        except DesfazerSonda:
            pass
        except Exception as exc:
            erros.append((0, type(exc).__name__))

        if primeira_celula is not None and estado_original is not None:
            try:
                desfeito = (
                    fotografia(CelulaDia.objects.filter(pk=primeira_celula))
                    == estado_original
                )
            except Exception as exc:
                erros.append((0, type(exc).__name__))

    for linha in linhas:
        print(linha)
    for cid, classe in erros:
        print("ERRO {} {}".format(cid, classe))
    print("ERROS={}".format(len(erros)))
    print("DESFEITO={}".format(desfeito))
    if erros or not concluiu_transacao or not desfeito:
        print("NAO_CONCLUIDO")


main()

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

_7380 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
