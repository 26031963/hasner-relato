# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 00:03** · publicado em **01/11/2026 00:04** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 00:03 · sessao 68821586ba49 · V3-P5-B · verificador · origem=Code

**O caso central foi corrigido: desconto da pausa prorrogada de 0 → 60 minutos, HE50 noturna de 3 → 2 horas e noturnas de 9 → 8 horas.** Há ressalvas sobre comparação estrita de `float`, isolamento do teste e alcance na alimentação produtiva. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:60; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:86; app/ponto/motor_calculo_v2.py:191; app/ponto/calculador/regras.py:823; app/ponto/calculador/regras.py:840)

**1. Diff e escopo**

O diff anexo do motor tem **1 hunk**, iniciado em `@@ -137,13 +137,63 @@`. Todas as linhas alteradas pertencem ao corpo do `def`, que começa na linha 121 e termina na 196. **Não procede alteração de linha externa ao corpo.** O hunk inclui contexto externo — linhas vazias e o comentário seguinte — sem modificá-lo. (diff.txt:5; [app/ponto/motor_calculo_v2.py:121](/home/ronald/codex_ws/03c8d106ed4e606c62803a85716b1e1a6954632a/app/ponto/motor_calculo_v2.py:121); app/ponto/motor_calculo_v2.py:196; app/ponto/motor_calculo_v2.py:199)

A identidade do histórico e a ordem efetiva dos dois commits não puderam ser conferidas: a cópia disponível não contém `.git`. [não verificado] [nao verificado]

**2. Cálculo novo e propriedades temporais**

O corpo faz, nesta ordem:

- Localiza entrada e saída; localiza e recorta cada pausa ao período. (app/ponto/motor_calculo_v2.py:162; app/ponto/motor_calculo_v2.py:164)
- Ordena as pausas, descarta recortes vazios/invertidos e une peças sobrepostas ou adjacentes. (app/ponto/motor_calculo_v2.py:166; app/ponto/motor_calculo_v2.py:169)
- Percorre cada peça unida, cortando na próxima hora cheia **do instante novamente normalizado no fuso local**. Se esse candidato não avançar, usa um minuto de avanço. (app/ponto/motor_calculo_v2.py:179; app/ponto/motor_calculo_v2.py:186; app/ponto/motor_calculo_v2.py:188)
- Calcula `N(entrada,b) − N(entrada,a)`. Se positivo acima de `1e-9`, soma a duração inteira do trecho, uma vez, independentemente da multiplicidade contada por `N`. (app/ponto/motor_calculo_v2.py:191)
- Retorna o menor entre essa soma e `N(entrada,saida)`. (app/ponto/motor_calculo_v2.py:196)

**Laço parado ou `b <= a`: não procede**, enquanto as operações de datetime terminarem sem exceção. Dentro do laço, `_f > a`; o candidato é estritamente maior que `a`, inclusive pelo avanço subsidiário; portanto `min(_f,proxima) > a`. Cada iteração avança. (app/ponto/motor_calculo_v2.py:179; app/ponto/motor_calculo_v2.py:188; app/ponto/motor_calculo_v2.py:190; app/ponto/motor_calculo_v2.py:194)

Aware de outro fuso é convertido para o fuso da casa; naive é localizado nesse fuso. Horas ambíguas/inexistentes são entregues a `pytz.localize`, sem política explícita de rejeição no código. O cursor relê o instante normalizado antes do próximo corte. O fuso configurado nesta cópia é São Paulo. (app/ponto/motor_calculo_v2.py:73; app/ponto/motor_calculo_v2.py:186; app/config/settings/base.py:93)

**Não encontrei corte atravessando uma borda 22/05 no interior**, nos casos com segundos, na mudança histórica de offset e na entrada/saída do verão examinados. Os testes delimitam essas janelas explicitamente, incluindo 04:59:30–05:01, cujo desconto é 0,5 minuto. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:415; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:420; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:422; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:480)

**Resultado negativo ou superior a `N(entrada,saida)`: não procede.** A soma recebe apenas durações positivas, `N` é não negativo, e o retorno aplica diretamente o teto em `N`. Acima de 26 horas esse teto é zero. (app/ponto/motor_calculo_v2.py:91; app/ponto/motor_calculo_v2.py:94; app/ponto/motor_calculo_v2.py:115; app/ponto/motor_calculo_v2.py:193; app/ponto/motor_calculo_v2.py:196)

**Superior à união: matematicamente não; em comparação estrita de `float`, procede um excesso de arredondamento.** No próprio caso 22:00:30–07:00:15, pausa 05:29:45–06:30:10:

```text
duração da união = 60.416666666666664 min
resultado novo  = 60.41666666666667  min
diferença       = 7.105427357601002e-15 min
```

Isso decorre da soma separada dos trechos e fica abaixo da tolerância `1e-9` empregada nas propriedades. Portanto **não procede prometer desigualdade estrita contra a duração da união em aritmética binária**. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:142; app/ponto/motor_calculo_v2.py:193; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:203)

Também não há garantia de retorno para todos os extremos representáveis de datetime: a chamada final a `N` executa operações sem guarda de overflow. Por exemplo, entrada/saída UTC em 31/12/9999, 15:00–16:00, sem pausas, passa a alcançar esse cálculo e pode lançar `OverflowError`; o corpo antigo simplesmente devolvia zero sem pausas. Isso é exceção, não laço infinito. (app/ponto/motor_calculo_v2.py:196; app/ponto/motor_calculo_v2.py:100; app/ponto/motor_calculo_v2.py:104; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:89)

**3. Teste, assinatura, oráculo e mutantes** [nao verificado]

**Não procede import inexistente no HEAD.** `_aw` está definido no próprio arquivo; os símbolos do motor importados pelo teste existem. Django é dependência declarada. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:41; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:44; app/ponto/motor_calculo_v2.py:28; app/ponto/motor_calculo_v2.py:73; app/ponto/motor_calculo_v2.py:82; app/ponto/motor_calculo_v2.py:121; app/requirements.txt:13)

**Não procede incompatibilidade com `do_dia`.** Os argumentos usados pertencem à assinatura; `he_pontas` contém exatamente `(entrada, saída, extra_50_min, extra_100_min)`, os quatro valores desempacotados neste HEAD. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:60; [app/ponto/calculador/regras.py:572](/home/ronald/codex_ws/03c8d106ed4e606c62803a85716b1e1a6954632a/app/ponto/calculador/regras.py:572); app/ponto/calculador/regras.py:818)

As expectativas centrais são justificadas pelo cenário declarado: 480−360 = 120 minutos extras; a pausa não é trabalho; com prorrogação e fator 1, os 480 minutos trabalhados são noturnos. **Não procede dizer que todas as expectativas vêm exclusivamente da CLT:** a escolha da cauda trabalhada como HE, o gatilho pelo toque em 00–05 e a proteção de 26 horas são políticas da casa/código, reconhecidas no teste. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:14; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151; app/ponto/motor_calculo_v2.py:93; app/ponto/motor_calculo_v2.py:110; app/ponto/motor_calculo_v2.py:413)

`_regra` não chama `minutos_noturnos`; constrói janela e união por minuto. Entretanto reutiliza `localizar`, trunca minutos e declara que não cobre o relógio de verão daquela grade de 2026. Os casos de segundos/históricos possuem expectativas próprias por interseção de janelas. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:160; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:164; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:167; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:410; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:473)

**Não procede patch ineficaz por alias.** Os helpers importam a função durante a chamada. No caminho integrado, `he_noturna` resolve o global do módulo do motor; o método emprestado por `_ContextoNoturno` também resolve esse global. Ambos chegam ao mutante. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:105; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:194; app/ponto/calculador/regras.py:63; app/ponto/calculador/regras.py:816; app/ponto/motor_calculo_v2.py:442; app/ponto/motor_calculo_v2.py:1915)

Os cinco mutantes têm testemunhas: peça isolada, diferença crua, limite por peça longa, cursor com offset fixo e piso incondicional com segundos. **Não procede RED sem mordida nesses caminhos.** (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:356; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:359; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:365; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:454; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:531)

**Relógio solto não procede:** datas e durações são explícitas; não há leitura de “agora” no corpo alterado ou no teste. (app/ponto/motor_calculo_v2.py:162; app/ponto/motor_calculo_v2.py:186; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:187)

Há uma ressalva de isolamento: **`feriados=set()` não torna o conjunto autoritativo**. A ausência da data aciona fallback ORM; uma exceção é capturada e convertida em `False`. Assim, o “dia comum” não está inteiramente fechado pela fixture. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66; app/ponto/motor_calculo_v2.py:953; app/ponto/motor_calculo_v2.py:39; app/core/feriados.py:95; app/core/feriados.py:29)

Os 34 testes passaram em exercício em memória com substitutos para a infraestrutura Django ausente; isso **não certifica execução Django real nem suíte completa**. [não verificado] [nao verificado]

**4. Antes/depois e chamadores**

Comparação com o corpo antigo preservado no mutante, para datetimes locais aware: (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:86)

| Forma de entrada | Desconto antigo → novo |
|---|---:|
| 22–07, pausa 05:30–06:30, prorrogação ligada | **0 → 60 min** |
| Mesmo caso, prorrogação desligada | **0 → 0 min** |
| Pausa 01–02 repetida duas vezes | **120 → 60 min** |
| Pausa cobrindo todo o recorte 21h–23h do dia seguinte, 26h, prorrogação ligada | **1560 → 1500 min** |
| Recorte acima de 26h, pausa noturna de uma hora | **60 → 0 min** |
| Sem pausas, datas ordinárias | **0 → 0 min** |

As mudanças decorrem respectivamente da janela do recorte, união, leitura por presença e teto do todo. (app/ponto/motor_calculo_v2.py:164; app/ponto/motor_calculo_v2.py:191; app/ponto/motor_calculo_v2.py:196; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:282)

Pontas naive com pausa não vazia também mudam o comportamento: o antigo comparava pausas já aware com pontas ainda naive, podendo lançar `TypeError`; o novo localiza as pontas. **Não procede que a mudança seja apenas numérica para todas as formas de entrada.** (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:91; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:92; app/ponto/motor_calculo_v2.py:162)

Há **dois chamadores diretos produtivos**:

| Chamador | O que recebe antes → depois no caso central |
|---|---|
| `MotorBase._noturno_dos_intervalos` | Desconto **0 → 60 min**; seu leitor `calcular_noturnas` passa de **540 → 480 min relógio**, multiplicados pelo fator cadastrado para minutos legais. (app/ponto/motor_calculo_v2.py:1915; app/ponto/motor_calculo_v2.py:971; app/ponto/motor_calculo_v2.py:980) |
| `he_noturna` | Na cauda 04–07, desconto **0 → 60 min**; com HE50=120 e HE100=0, devolve **(180,0) → (120,0) min**. (app/ponto/motor_calculo_v2.py:440; app/ponto/motor_calculo_v2.py:442; app/ponto/motor_calculo_v2.py:450) |

Os outros cinco pontos diretos estão no teste: `_f`, teste com segundos, `_celula` e os dois `_caso`. Seus resultados seguem as formas acima; os históricos sem prorrogação permanecem **354, 270 e 450 minutos**, e os casos ordinários de borda permanecem **0,5 e 1 minuto**. O caso naive inexistente passa de erro de comparação para **239,5 minutos**. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:108; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:142; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:197; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:409; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:472)

**Não procede estender o resultado do RED indistintamente ao calculador produtivo diário:** `alimentacao.py` passa `intervalos=()`, com pares já segmentados; ali o desconto continua zero. O adaptador V3 por par, por sua vez, fornece as pausas julgadas. (app/ponto/calculador/alimentacao.py:121; app/ponto/calculador/alimentacao.py:512; app/ponto/calculador/alimentacao.py:514; app/ponto/calculador/periodo.py:507)

O comentário do motor ainda atribui o caso a HE100; **isso não procede para o teste deste HEAD**, que verifica HE50 e mantém HE100 em zero. (app/ponto/motor_calculo_v2.py:145; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:70)

**5. Escritores, leitores e classes reprovadas**

| Valor | Quem escreve/produz → quem lê, até a gravação |
|---|---|
| Fuso | `settings.TIME_ZONE` → `TZ_LOCAL` → localização e cursor. (app/config/settings/base.py:93; app/ponto/motor_calculo_v2.py:28; app/ponto/motor_calculo_v2.py:186) |
| Pontas e pausas | `registrar_batida` grava timestamps; autoridades produzem períodos/intervalos; o motor recupera timestamps exatos e carimba as pontas e pausas usadas. (app/ponto/registro_batida.py:157; app/ponto/turnos.py:543; app/ponto/motor_calculo_v2.py:609; app/ponto/motor_calculo_v2.py:616; app/ponto/motor_calculo_v2.py:721; app/ponto/motor_calculo_v2.py:1828) |
| Prorrogação | Cadastro/régua → `get_motor_cct` → atributo de `MotorBase` → wrapper e split da HE. (app/core/models.py:72; app/core/regua_cct.py:153; app/core/regua_cct.py:466; app/ponto/motor_calculo_v2.py:762; app/ponto/motor_calculo_v2.py:1916; app/ponto/motor_calculo_v2.py:1005) |
| Desconto devolvido | `calcular_noturnas`, `noturnas_do_dia` e `he_noturna` subtraem o escalar. (app/ponto/motor_calculo_v2.py:973; app/ponto/calculador/regras.py:86; app/ponto/motor_calculo_v2.py:442) |
| Noturnas do período | `_escrever_noturnas` grava minutos relógio/legais; o adaptador V3 também os preenche por par. (app/ponto/motor_calculo_v2.py:991; app/ponto/motor_calculo_v2.py:992; app/ponto/calculador/periodo.py:751) |
| HE noturna do período | `_aplicar_he_noturna` grava os minutos de HE50/HE100 noturna. (app/ponto/motor_calculo_v2.py:1002; app/ponto/motor_calculo_v2.py:1008; app/ponto/motor_calculo_v2.py:1010) |
| Gravação diária | `lavrar` soma os três valores ou recebe rubricas do calculador e persiste `DiaPago.horas_noturnas`, `horas_extras_50_noturna`, `horas_extras_100_noturna`. (app/ponto/services/dia_pago.py:179; app/ponto/services/dia_pago.py:322; app/ponto/services/dia_pago.py:358; app/ponto/models.py:327; app/ponto/models.py:329; app/ponto/models.py:332) |
| Gravação mensal | `ResultadoMes` soma os minutos em horas; fechamento arredonda e grava os três campos correspondentes de `FechamentoMensal`. (app/ponto/motor_calculo_v2.py:321; app/ponto/motor_calculo_v2.py:340; app/ponto/motor_calculo_v2.py:344; app/ponto/services/fechamento.py:893; app/ponto/services/fechamento.py:896; app/ponto/services/fechamento.py:899; app/ponto/services/fechamento.py:1128) |
| Leitura posterior | Apresentação diária lê `DiaPago`; exportador lê o adicional noturno do fechamento. (app/ponto/services/dia_pago.py:922; app/folha/export.py:436) |

**Segundo juiz e derivação paralela: não procedem no corpo alterado.** Ele consulta o mesmo `minutos_noturnos`, não duplica as constantes 22/05 nem decide novamente o cadastro da prorrogação; a autoridade do cálculo por par permanece registrada no motor. (app/ponto/motor_calculo_v2.py:191; app/ponto/motor_calculo_v2.py:196; app/core/juizes.py:1160)

**Segundo escritor fora da porta: não procede neste diff.** O corpo devolve um escalar; os consumidores continuam usando os escritores existentes. **Relógio fora da borda e RED sem mordida também não procedem**, pelas verificações descritas acima. (app/ponto/motor_calculo_v2.py:196; app/ponto/motor_calculo_v2.py:992; app/ponto/services/dia_pago.py:358; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347)

Ordem operacional de travas, suíte completa verde, impacto nas competências e pouso após os pré-requisitos: **[não verificado]**. O material fornecido não traz prova operacional deste HEAD; portanto este laudo não certifica esses pré-requisitos. [não verificado] [nao verificado]

## 01/11 00:02 · sessao 5a22aa01608c · cobparc · verificador · origem=Code

**REPROVAR a volta 2.** A exclusão de `dispensada` corrige os dois contadores alterados, mas ainda há caminhos que contam dispensa como batida, preservam julgamento antigo ou impedem a pergunta sobre o marco descoberto. (`app/ponto/services/cartorio.py:1204`; `app/ponto/status_dia.py:143`; `app/escala/services/leitor_celula.py:83`; `app/chamados/services/declaracao_texto.py:95`)

Revisão estática da cópia e dos anexos. Identidade dos objetos Git, execução dos testes, ocorrência e quantidade em produção: **[nao verificado]**. A cópia não disponibilizou repositório Git; o censo abaixo usou `rg` equivalente, sem banco ou logs. **[nao verificado]** [nao verificado]

**1. Quem escreve e quem lê**

| Valor | Escritores e leitores encontrados | Efeito da volta 2 |
|---|---|---|
| Célula `dispensada` | A grade escreve a dispensa parcial; cartório propaga para lâmpada; leitor reconstitui na célula. Existe também dispensa integral da ata. (`app/escala/utils.py:1207`; `app/ponto/services/cartorio.py:446`; `app/escala/services/leitor_celula.py:119`; `app/ponto/services/cartorio.py:649`) | O delta muda os leitores `n_bat` e `fatos_do_status`; o marcador continua sem hora/delta. (`app/ponto/services/cartorio.py:1205`; `app/ponto/status_dia.py:145`; `app/escala/utils.py:1207`) |
| `n_bat` do cartório | Escrito pela soma de células casadas e órfãs; entregue como `batidas_no_dia` ao supra. (`app/ponto/services/cartorio.py:1204`; `app/ponto/services/cartorio.py:1250`) | A dispensa deixa de simular batida; isso muda quais ramos do supra são alcançados. (`app/ponto/supra_juiz.py:313`; `app/ponto/supra_juiz.py:437`) |
| `tem_batida` do status | `fatos_do_status` escreve o retorno; os chamadores produtivos são completar status, nunca-bateu e lavra comum. A porta entrega os fatos a `status_da_lavra`. (`app/ponto/status_dia.py:148`; `app/ponto/services/cartorio.py:765`; `app/ponto/services/cartorio.py:1078`; `app/ponto/services/cartorio.py:1619`; `app/ponto/portas/celula.py:698`) | Agora exclui dispensa, mantendo órfãs e realizado positivo como evidência. O leitor do status usa o resultado em suas precedências. (`app/ponto/status_dia.py:143`; `app/ponto/status_dia.py:96`) |
| Retorno de `_cob_parcial_da_impressao` | Produzido pelo helper; consumido somente pelos dois cálculos de impressão do cartório, nos ramos nunca-bateu e normal. (`app/ponto/services/cartorio.py:334`; `app/ponto/services/cartorio.py:1029`; `app/ponto/services/cartorio.py:1180`) | Acrescenta os índices dispensados ao insumo `cob_parcial`; o hash incorpora a string. (`app/ponto/services/cartorio.py:338`; `app/ponto/services/cartorio.py:406`) |
| `cel.impressao` | Escrita pela porta `carimbar`, chamada pelos dois ramos do cartório. Lida na idempotência da porta, nas duas decisões de pulo e no diagnóstico de veredito velho. (`app/ponto/portas/celula.py:801`; `app/ponto/services/cartorio.py:1096`; `app/ponto/services/cartorio.py:1635`; `app/ponto/portas/celula.py:797`; `app/ponto/services/cartorio.py:1031`; `app/ponto/services/cartorio.py:1189`; `app/ponto/management/commands/celula_veredito_velho.py:110`) | Os leitores continuam comparando igualdade; muda o valor recebido quando há dispensa. (`app/ponto/services/cartorio.py:335`; `app/ponto/services/cartorio.py:1189`) |
| `_parcial_so_lista` | Derivado uma vez por iteração, lendo cobertura parcial, julgamento anterior e `houve_fato_novo`. Lido pelos dois portões e pelos respectivos contadores. (`app/ponto/services/cartorio.py:1535`; `app/ponto/services/cartorio.py:1550`; `app/ponto/services/cartorio.py:1553`; `app/ponto/services/cartorio.py:1596`; `app/ponto/services/cartorio.py:1599`) | Fecha a abertura sem fato novo nos dois portões. Não governa a ressuscitação anterior nem o espelhamento do vivo. (`app/ponto/services/cartorio.py:1453`; `app/ponto/services/cartorio.py:1611`) |
| `out['parcial_na_lista_sem_push']` | Criado/incrementado nos dois portões; cada incremento lê o acumulado. O teste lê o resultado; o resumo do command não o imprime. (`app/ponto/services/cartorio.py:1551`; `app/ponto/services/cartorio.py:1597`; `app/ponto/tests/test_cobparc_marco_no_periodo.py:246`; `app/ponto/management/commands/processar_cartorio.py:66`) | `_contou_lista` evita duplicação entre portões **na mesma iteração**, não entre passadas. (`app/ponto/services/cartorio.py:1537`; `app/ponto/services/cartorio.py:828`) |

Os demais leitores diretos de `dispensada` são a impressão e `_parcial_sem_resto`; não encontrei leitor direto em template. (`app/ponto/services/cartorio.py:335`; `app/ponto/supra_juiz.py:312`) O calendário possui uma chave homônima `tem_batida`, construída de seus próprios fatos, sem consumir esse retorno do status. (`app/colaboradores/services/calendario.py:810`)

Para **`marcos_faltantes`**, a cadeia alterada é: ata atual → `lastro.marcos_apagados` → tradução/nomeação → parâmetro do emissor → contexto do chamado. O portão sem batida agora usa essa cadeia também para dia parcial. (`app/ponto/services/cartorio.py:1557`; `app/ponto/services/cartorio.py:1674`; `app/ponto/management/commands/detectar_ausencias.py:704`)

O nascimento grava pelos defaults de `ChamadoColaborador.abrir`; chamado existente usa `_espelhar_marcos`, que compara e escreve pela porta de contexto. Lista vazia/`None` ainda aciona o fallback da entrada prevista. (`app/ponto/management/commands/detectar_ausencias.py:688`; `app/ponto/management/commands/detectar_ausencias.py:704`; `app/ponto/management/commands/detectar_ausencias.py:556`; `app/chamados/services/contexto.py:96`; `app/ponto/management/commands/detectar_ausencias.py:475`)

Leitores fora do delta:

- Marco por motivo/tipo e materialização: `app/chamados/juizes.py:506`; `app/chamados/services/materializacao.py:259`.
- Lastro/fechamento e texto do supra: `app/ponto/services/lastro.py:75`; `app/ponto/supra_juiz.py:129`.
- Badges da grade e cobrança órfã por regeneração: `app/escala/utils.py:1039`; `app/escala/services/regeneracao.py:107`; `app/escala/services/regeneracao.py:156`.
- Geração/correção de contexto e seleção de chamados sem marco: `app/ponto/management/commands/gerar_celulas.py:251`; `app/ponto/management/commands/reconciliar_grade.py:448`; `app/ponto/management/commands/curar_residuo_bug115.py:73`; `app/chamados/management/commands/nomear_marco_por_lampada.py:50`.
- Emissão de perguntas de intervalo e guarda de abertura: `app/chamados/services/disputa_emissao.py:947`; `app/chamados/models.py:882`.

Esses leitores não são alterados pelo delta fornecido, mas recebem a lista reduzida aos marcos apagados. Há outros produtores pela mesma porta de contexto, além do contexto de nascimento montado pelo reconciliador. (`app/ponto/management/commands/reconciliar_grade.py:473`; `app/chamados/management/commands/nomear_marco_por_lampada.py:109`; `app/chamados/reconciliador.py:677`)

**2. Alcance da impressão**

Mantidos os demais insumos, o helper muda o hash somente quando há **base verdadeira e algum índice dispensado**. Sem dispensa retorna exatamente a base anterior; motivo/slug da dispensa não entra no hash. Portanto, essa adição não provoca rejulgamento dos dias sem marco dispensado. (`app/ponto/services/cartorio.py:334`; `app/ponto/services/cartorio.py:338`; `app/ponto/services/cartorio.py:406`)

**Achado A — P1: a cura não alcança todos os dias cuja regra mudou.** Contraexemplo: marcos 08/12/13/17, batidas 07:58E/12:02S, parcial 12:30–12:45. Nenhum marco é dispensado; o período permanece `fora`. A classificação nova acusa `FURO_PARCIAL`, mas uma célula anteriormente `concorde`, com os mesmos insumos e `forcar=False`, conserva o hash e é pulada. A declaração de versões atual não contém uma versão adicional para esta obra. (`app/escala/utils.py:1204`; `app/ponto/supra_juiz.py:390`; `app/ponto/services/cartorio.py:334`; `app/ponto/services/cartorio.py:1189`; `app/ponto/services/cartorio.py:111`)

O teste de impressão cobre 13–17 → 14–17, com dispensa presente; não cobre essa migração de regra sem dispensa. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:354`)

**3. Portões, chamado vivo e contador**

Nos dois portões, parcial previamente julgada sem fato novo deixa de chamar `_abrir_chamado`; primeiro julgamento ou fato novo permite a tentativa, sujeita aos demais vetos. `houve_fato_novo` lê batidas, ausência, justificativa, edição/regeneração e vencimento de marco. (`app/ponto/services/cartorio.py:1535`; `app/ponto/services/cartorio.py:1553`; `app/ponto/services/cartorio.py:1599`; `app/ponto/services/cartorio.py:283`)

No ramo vivo, `_espelhar_marcos` escreve contexto e nota, sem chamada de push nessa operação. A ressuscitação devolve perguntas à pendência e pode reabrir disputa, antes do bloqueio novo. Não confirmei abertura de `batida_ausente` sem fato novo por esses caminhos; o reemissor c2 também para ao encontrar ausência. (`app/ponto/management/commands/detectar_ausencias.py:553`; `app/chamados/models.py:1488`; `app/chamados/models.py:1516`; `app/chamados/reconciliador.py:825`)

Persistem estes problemas:

- **Achado B — P1: fato novo pode abrir chamado sem gerar a pergunta do marco descoberto.** Contraexemplo: 08/12/13/17, batidas 07:58/12:02, parcial 14–17 e fato novo. O cartório cobra E13, mas `_marcos_faltantes` ignora `tipo_dia='ausencia'`; a fábrica recebe faltantes vazios. O agrupador de chamados nomeados passa pelo mesmo leitor. (`app/ponto/services/cartorio.py:1600`; `app/chamados/services/declaracao_texto.py:95`; `app/chamados/services/fabrica_da_celula.py:49`; `app/chamados/services/disputa_emissao.py:974`)

- **Achado C — P2: chamado vivo sem batida mantém marcos dispensados no contexto.** Contraexemplo: chamado vivo cobra 08/12/13/17; parcial 13–17 dispensa os dois últimos, sem batidas no dia. Com fio vivo, o portão sem batida não acusa; sem fato/batida, também não há `FURO_PARCIAL`. Como 08/12 continuam apagados, o chamado permanece, mas o espelhamento está subordinado a `FURO_PARCIAL`: 13/17 ficam na lista antiga. Essa lista continua sendo fonte da materialização. (`app/ponto/supra_juiz.py:313`; `app/ponto/supra_juiz.py:379`; `app/ponto/services/lastro.py:174`; `app/ponto/services/cartorio.py:1583`; `app/ponto/services/cartorio.py:1611`; `app/chamados/services/materializacao.py:235`)

- **Achado D — P2: o push ainda pode nomear a entrada dispensada.** Contraexemplo: 12x36 07–19, parcial 07–13, nenhuma batida, primeiro julgamento. A lista enviada ao chamado pode conter somente S19, mas `dt_inicio` continua sendo 07h. Para ciclo operacional, o push usa esse instante em “Previsto 07:00” e no atraso. A lista foi corrigida; o aviso ainda aponta o marco coberto. (`app/ponto/services/cartorio.py:1546`; `app/ponto/services/cartorio.py:1557`; `app/ponto/services/cartorio.py:1562`; `app/ponto/management/commands/detectar_ausencias.py:585`; `app/ponto/management/commands/detectar_ausencias.py:776`)

- **Achado E — P2: o contador pode contar um dia duas vezes no mesmo ato.** Contraexemplo: parcial 14–17 já existente, batidas 07:58/12:02 anteriores ao julgamento e chamado legado vivo cobrando somente S17, sem pergunta viva. A cura dispensa S17, mantém E13 apagada e fecha o chamado antigo pela ata atual; o fechamento enfileira rejulgamento. A primeira passada conta lista-sem-push; a segunda usa o mesmo `out`, reinicia `_contou_lista` e conta novamente. O carimbo intermediário faz a segunda consulta usar `(agora, agora]`, sem fato novo. Resultado do fluxo: **2 para um dia**. (`app/chamados/models.py:1236`; `app/chamados/models.py:400`; `app/ponto/services/cartorio.py:1319`; `app/ponto/services/cartorio.py:1596`; `app/ponto/services/cartorio.py:832`; `app/ponto/services/cartorio.py:1537`; `app/ponto/portas/celula.py:800`; `app/ponto/services/cartorio.py:278`)

**4. Censo de “não missing = batida” e variantes**

**Achado F — P1: `n_ok_lavrado` ainda conta dispensa como batida/casamento e cala o furo.** Contraexemplo: 08/12/13/17, adesão anterior, nenhuma batida, parcial 13–17. Ata com quatro lâmpadas e duas missing produz `n_ok=2`; o supra prefere esse número e não entra no portão que exige zero. Os commands `supra_juiz` e `emitir_furo_retroativo` usam essa grade, divergindo do cartório corrigido. (`app/escala/services/leitor_celula.py:83`; `app/escala/services/leitor_celula.py:361`; `app/ponto/supra_juiz.py:229`; `app/ponto/supra_juiz.py:313`; `app/ponto/management/commands/supra_juiz.py:206`; `app/ponto/management/commands/emitir_furo_retroativo.py:99`)

| Sítio | Hora/delta? | Resultado |
|---|---|---|
| `app/escala/services/leitor_celula.py:83`, `:89`, `:335` | Não | Contam dispensa em `n_ok`/`n_casadas`; a mesma conta também alimenta `_teve_fato` na fronteira admissão/demissão. |
| `app/inteligencia/calculadores.py:153` | Não | Conta não-missing, mas filtra dia `trabalho` em `:141`; parcial rotulada ausência não entra. |
| `app/ponto/propositor.py:40` | Hora depois, em `:52` | Seleção ampla; tipo `trabalho` em `:36` e hora impedem usar a dispensa. |
| `app/ponto/propositor.py:213` | Delta não nulo | Exclui a dispensa com `delta=None`. |
| `app/ponto/management/commands/propor_reorganizacao.py:102` | Hora em `:103` | Exclui a dispensa. |
| `app/ponto/supra_juiz.py:89` | Hora em `:90` | Recontagem exclui dispensa; `n_ok` lavrado contorna essa proteção em `:229`. |
| `app/ponto/turnos.py:2675` | Hora; delta em `:2678` | Exclui a dispensa. |
| `app/ponto/services/cartorio.py:478`, `:503` | Hora | Excluem a dispensa das horas reais/usadas. |
| `app/ponto/services/cartorio.py:532` | Não | Estado de lâmpada: permite acesa sem luz e propaga dispensa em `:563`, `:565`; não é contagem de batida. |
| `app/ponto/services/cartorio.py:1205` | Não | Agora exclui explicitamente dispensa. |
| `app/ponto/status_dia.py:144` | Não | Agora exclui isenção e dispensa em `:145`. |
| `app/escala/utils.py:812` | Hora | Preserva hora real; crédito de isenção segue em `:820`. |
| `app/escala/utils.py:1958` | Não nessa guarda | Decide substituição de ponta missing; não conta batidas. |
| `app/escala/utils.py:1990` | Hora em `:1991` | Dispensa sem hora não satisfaz. |
| `app/chamados/detector_anomalias.py:212` | Hora/tipo em `:213` | Recebe casamento novo de `_match_marcos` em `:210`, antes do overlay de ausência. |
| `app/chamados/services/declaracao_texto.py:107` | Não | Seleciona faltantes; o filtro de tipo em `:95` causa o achado B. |
| `app/ponto/janela_he.py:294`; `app/ponto/services/veredito_celula.py:251` | Não nessas guardas | Selecionam missing para pendência/simulação; não contam batidas. |
| `app/ponto/services/bordas_realizado.py:51` | Confia em `n_ok` | Outro leitor da conta; seu chamador restringe a `trabalho` em `:74`. |

**5. Descartes da triagem**

**Procedem de fato três descartes:**

- **`materializacao._coberto_por_ausencia` — P2.** Existir qualquer ausência não-falta continua bastando para impedir a pergunta. Contraexemplo: parcial 14–17 não explica E13, mas essa guarda bloquearia E13 mesmo após corrigir o filtro do achado B. “Conservador quanto a push” não resolve a ausência de pergunta respondível. (`app/chamados/services/materializacao.py:55`; `app/chamados/services/disputa_emissao.py:763`; `app/chamados/services/pergunta_no_app.py:74`)

- **`classificar_falta` — P2.** Contraexemplo: batidas 07:58E/12:02S e parcial 13–17. A grade dispensa os marcos restantes, mas a contagem E/S contra todos os marcos ainda acusa entrada faltante. Há leitor que transforma esse retorno diretamente em pergunta por `entradas_pendentes`, sem consultar as lâmpadas: portanto, “os marcos sempre vêm das lâmpadas” não vale para todo o repo. (`app/escala/servico_jornada.py:469`; `app/escala/utils.py:1207`; `app/chamados/services/disputa_emissao.py:267`; `app/chamados/services/disputa_emissao.py:389`)

- **Âncora noturna com pausa — P2.** Contraexemplo: E19/S23/E00/S07, parcial 23–07. A menor entrada é E00, deixando a régua E00/S07/E19/S23. O helper encadeia essa ordem e dá S07=420; o período cobre 1380–1860. A dispensa nova deixa S07 missing, embora esteja coberta. A origem da ordem é preexistente, mas a cobrança indevida é herdada pelo escritor novo. (`app/escala/utils.py:1335`; `app/escala/utils.py:1668`; `app/ponto/turnos.py:2661`; `app/ponto/turnos.py:2634`; `app/escala/utils.py:1209`)

Os demais descartes ficam assim:

| Descarte | Conclusão |
|---|---|
| `dia=None` | Não procede nos três chamadores produtivos: todos passam `dia`. (`app/ponto/services/cartorio.py:1042`; `:1217`; `:1613`) |
| `_parcial_sem_resto` com sem-vínculo | Não confirmei contraexemplo no builder: sem vínculo não monta turno e entrega previsto zero. (`app/escala/utils.py:1271`; `:1559`; `:1739`) |
| Duas parciais | Há desempate explícito por `(data, pk)`, sem composição de períodos. (`app/ponto/turnos.py:2768`) |
| Justificativa integral | Continua sendo dispensa integral conforme a decisão fornecida; o código dispensa lâmpadas pela marca `justificado`. (`app/ponto/services/cartorio.py:641`; `:649`) |
| Marco DNA fora da régua | A limitação continua no alinhamento por coluna; não confirmei novo contraexemplo além do declarado. (`app/escala/utils.py:1668`; `:1675`) |
| Contador ausente do resumo | A ausência no resumo é real; a lista administrativa tem fonte no veredito/protestos. Isso não elimina a duplicação do achado E. (`app/ponto/management/commands/processar_cartorio.py:66`; `app/ponto/services/furos_sem_cobranca.py:3`) |
| Concorrência sem releitura travada | Não há prova fornecida de corrida concreta desta obra; execução concorrente **[nao verificado]**. A coordenação do corpo permanece sem uma transação abrangente. (`app/ponto/services/cartorio.py:800`) |

**6. Checklist**

| Item | Resultado |
|---|---|
| Segundo juiz | A dispensa nova usa o juiz de período registrado, sem outra comparação própria de bordas. Persistem interpretações divergentes em `classificar_falta`, no filtro de tipo e na cobertura ampla. (`app/core/juizes.py:244`; `app/escala/utils.py:1210`; `app/escala/servico_jornada.py:469`; `app/chamados/services/declaracao_texto.py:95`; `app/chamados/services/materializacao.py:66`) |
| Segundo escritor fora da porta | Não confirmado no delta: lavra/carimbo passam pela porta da célula; espelhamento passa pela porta de contexto. (`app/ponto/services/cartorio.py:1615`; `:1635`; `app/ponto/management/commands/detectar_ausencias.py:558`; `app/chamados/services/contexto.py:100`) |
| Relógio fora da borda | Os auxiliares novos usam insumos recebidos; encerramento recebe `agora`. Há leitura interna preexistente de `_tz.now()` em `classificar_falta`. (`app/ponto/turnos.py:2634`; `app/ponto/services/cartorio.py:941`; `app/escala/servico_jornada.py:377`) |
| Derivação paralela de fonte única | Ainda existe contagem aritmética de casamento sem distinguir dispensa, além da contagem crua de E/S. (`app/escala/services/leitor_celula.py:83`; `app/escala/servico_jornada.py:469`) |
| Selo/RED sem mordida | Os testes novos verificam chamado, não a pergunta respondível; o teste de impressão cobre somente caso com dispensa. Não cobrem `n_ok` lavrado nem a duplicação entre passadas. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:237`; `:252`; `:354`; `app/ponto/services/cartorio.py:832`) RED/GREEN executado: **[nao verificado]**. |
| Ordem de trava | Nenhuma inversão nova demonstrada; garantia concorrente **[nao verificado]**. A sequência de lavra/E2/reconciliação continua explícita, mas permite a segunda passagem que duplica o contador. (`app/ponto/services/cartorio.py:811`; `:828`) |
| `JUIZES.md` | **P3: desatualizado.** A tabela diz que ata preenchida diferente de trabalho cala; a função agora excetua ausência parcial. Contraexemplo documental: `trabalha=True`, ata `ausencia`, parcial verdadeira — a função cobra, a descrição manda calar. (`app/docs/JUIZES.md:55`; `app/chamados/juizes.py:411`) |

## 01/11 00:01 · sessao c90b455d9af5 · - · - · origem=Ronald (tmux)

**P2 — contador duplica o mesmo dia na segunda passada.** `_contou_lista` é reiniciado por julgamento (`app/ponto/services/cartorio.py:1537`), enquanto B5.3b repete o corpo com o mesmo `out` (`app/ponto/services/cartorio.py:828-832`).

Contraexemplo: parcial 14–17 já existente, batidas 07:58E/12:02S, julgamento anterior, nenhum fato novo e chamado legado vivo cobrando somente S17, sem pergunta viva. A nova regra dispensa S17; E13 continua apagada. O chamado fecha porque sua premissa lê a ata em curso e encontra S17 acesa (`app/chamados/models.py:1227-1237`); sem pergunta viva, a outra guarda permite (`app/chamados/models.py:400-402`). O fechamento enfileira a célula para rejulgar (`app/ponto/services/cartorio.py:1319-1326`).

A primeira passada incrementa o contador (`app/ponto/services/cartorio.py:1596-1598`) e carimba `julgada_em=agora` (`app/ponto/services/cartorio.py:1635`, `app/ponto/portas/celula.py:800`). Na segunda, o marcador local reinicia e a janela `(agora, agora]` não contém fato novo (`app/ponto/services/cartorio.py:278-298`): incrementa novamente. **Resultado estático: 2 para um dia; execução e ocorrência em produção [nao verificado].**

## 01/11 00:00 · sessao c90b455d9af5 · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

- **P1/P2 — chamado do parcial sem pergunta respondível.** `_marcos_faltantes` descarta todo dia diferente de `trabalho`, inclusive `ausencia` parcial (`app/chamados/services/declaracao_texto.py:90-96`). A fábrica lê esse resultado e devolve nenhum motivo (`app/chamados/services/fabrica_da_celula.py:48-64`). Contraexemplo: escala 08/12/13/17, batidas 07:58E/12:02S, parcial 14–17, fato novo: E13 permanece apagada e o cartório pode abrir chamado, mas a fábrica lê `[]`. O agrupador de chamados nomeados passa pelo mesmo leitor (`app/chamados/services/disputa_emissao.py:969-979`).

- **P2 — descarte `materializacao._coberto_por_ausencia` é defeito funcional real.** Ele responde cobertura integral pela existência de qualquer ausência não-falta (`app/chamados/services/materializacao.py:55-68`) e bloqueia a criação da pergunta (`app/chamados/services/disputa_emissao.py:763-770`). Mesmo corrigindo o filtro anterior, parcial 14–17 bloquearia a pergunta de E13. A retenção do app declara explicitamente essa causa (`app/chamados/services/pergunta_no_app.py:71-75`). Ser conservador quanto a push não torna correta a supressão da pergunta de marco descoberto.

- **P2 — descarte da âncora noturna reproduz cobrança indevida.** A união escolhe a menor entrada entre todas, incluindo retorno de intervalo (`app/escala/utils.py:1335-1343`), e alinha as células nessa ordem (`app/escala/utils.py:1664-1668`). Turno E19/S23/E00/S07 resulta em ordem E00/S07/E19/S23. O novo helper encadeia essa ordem, atribuindo S07 a 420 minutos (`app/ponto/turnos.py:2651-2661`), embora parcial 23–07 cubra 1380–1860 (`app/ponto/turnos.py:2634-2640`). A dispensa nova deixa S07 missing (`app/escala/utils.py:1204-1210`). A régua preexistia; seu novo consumidor herda o erro.

- **P2 — `classificar_falta` continua divergindo da dispensa.** Conta E/S das batidas contra todos os marcos, sem ler `dispensada` (`app/escala/servico_jornada.py:447-480`). Contraexemplo: batidas E07:58/S12:02 e parcial13–17: grade dispensa os dois marcos restantes, mas a contagem acusa entrada faltante (`app/escala/servico_jornada.py:469-472`). Há caminho que transforma esse retorno diretamente em pergunta: `entradas_pendentes` → `_cf` → `gerar_perguntas_disputa`, sem consultar lâmpadas ou cobertura (`app/chamados/services/disputa_emissao.py:267-274`, `app/chamados/services/disputa_emissao.py:389`). A ocorrência desse contexto em produção é **[nao verificado]**.

- **P3 — JUIZES.md desatualizado.** A tabela diz que `ata.tipo_dia != trabalho` cala cobrança (`app/docs/JUIZES.md:55-57`), mas a função agora excetua ausência parcial (`app/chamados/juizes.py:411-412`). O próprio documento exige atualização no mesmo commit (`app/docs/JUIZES.md:14`).

Esclarecimentos importantes:

- **Não confirmei bypass da lista por c2.** Embora a geração possa chamar `reatar_pergunta_orfa` (`app/chamados/services/disputa_emissao.py:404-409`), c2 retorna antes de emitir quando existe ausência, incluindo parcial (`app/chamados/reconciliador.py:825-827`). A abertura fica depois, em `app/chamados/reconciliador.py:866`.
- **Retiro a hipótese de novo chamado-pai pela fábrica no parcial comum:** o filtro `tipo_dia != trabalho` impede que ela obtenha motivos (`app/chamados/services/declaracao_texto.py:95-96`, `app/chamados/services/fabrica_da_celula.py:111`).
- **`dia=None` não procede nos chamadores produtivos de `dispensar_por_ausencia`:** os três passam o dia (`app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1613`).
- **Duas parciais:** há desempate explícito por `(data, pk)`, com sobrescrita, não composição de períodos (`app/ponto/turnos.py:2768-2785`).
- **Sem vínculo:** builder consulta a autoridade de vínculo (`app/escala/utils.py:1271-1278`); sem vínculo não monta turno e produz previsto zero (`app/escala/utils.py:1559-1561`, `app/escala/utils.py:1739-1761`). Não confirmei contraexemplo real para `_parcial_sem_resto` apagar acusação de cadastro.
- **Escrita pela porta:** lavra e carimbo continuam nas portas existentes (`app/ponto/services/cartorio.py:1615`, `app/ponto/portas/celula.py:787-807`); nenhuma segunda escrita persistente nova confirmada.
- **Relógio:** novos auxiliares de período usam dados e instante recebido; o julgamento carrega encerramento com `agora` (`app/ponto/services/cartorio.py:938-942`). A chamada preexistente de relógio interno permanece em `classificar_falta` (`app/escala/servico_jornada.py:375-378`).
- **RED/GREEN e ordem de trava:** execução, mutação RED, suite e concorrência reais **[nao verificado]**. Os testes da obra verificam apenas existência de chamado `batida_ausente`, não pergunta no app (`app/ponto/tests/test_cobparc_marco_no_periodo.py:237-252`), deixando escapar o primeiro achado.

## 31/10 23:59 · sessao 49461eeffde4 · - · - · origem=Ronald (tmux)

Achado confirmado de alcance da cura, **P1/P2**: dias parciais afetados pela obra podem continuar com o julgamento antigo quando nenhum marco foi dispensado. `_cob_parcial_da_impressao` devolve exatamente `periodo_parcial` quando não existe `dispensada` (`app/ponto/services/cartorio.py:334-338`); o hash codifica essa string em `:406`, e a comparação pula a célula já carimbada em `:1189-1195`. O full fornecido muda `'fora' -> pass` para acusação, sem alterar `REGRA_VERSOES` (`:111`). Contraexemplo: marcos 08/12/13/17, batidas 07:58/12:02, ausência parcial 12:30–12:45. Nenhum marco missing cai no período (`app/escala/utils.py:1204-1211`), período agregado continua `fora`, hash mantém os mesmos campos (`cartorio.py:350-355`, `:406`), mas nova classificação deveria produzir `FURO_PARCIAL` (`app/ponto/supra_juiz.py:390-399`). Se já estava `concorde` com esse hash e `forcar=False`, permanece pulada. Existência desse estado em produção: [nao verificado]. O teste novo só cobre mudança 13–17 para 14–17, com índices dispensados presentes nas duas situações; não cobre migração de regra sem dispensada (`app/ponto/tests/test_cobparc_marco_no_periodo.py:354-361`).

**Impressão: escritores/leitores.**

- Retorno do helper: produzido em `cartorio.py:334-338`; dois consumidores, ambos chamados `impressao_insumos`, no ramo nunca-bateu `:1029-1030` e ramo normal `:1180-1185`. Não há outro consumidor direto encontrado. Hash produzido em `:350-407`.
- Impressão da célula: único escritor de produção encontrado é a porta `app/ponto/portas/celula.py:801`, salvando em `:805-807`; recebe do cartório `:1096` e `:1635`. Leitores operacionais: idempotência da própria porta `celula.py:797`, `_em_dia_nb` em `cartorio.py:1031` e `_em_dia` em `:1189`. Leitor diagnóstico/snapshot: `app/ponto/management/commands/celula_veredito_velho.py:110`. Modelo declara coluna em `app/escala/models.py:1714`. Esses leitores não mudam no delta fornecido.
- A mudança do helper só acrescenta sufixo quando **base truthy e há dispensada**; índices ordenados pela posição na lista; motivo/slug não entra no hash (`cartorio.py:334-338`). Sem dispensada não há mudança causada por esse helper; ausência de base também preserva retorno (`:336-337`). Isso exclui rejulgamento em massa por essa adição, mas causa o problema de alcance acima.

**`marcos_faltantes`: caminho alterado e leitores diretos fora do diff.**

- Cartório deriva os marcos da ata atual, não recalcula a cobertura: `_marcos_do_furo_parcial` lê `lastro.marcos_apagados` e nomeia por tradutor único (`cartorio.py:1654-1685`; `app/ponto/services/lastro.py:118-135`). No portão sem batida, novo argumento em `cartorio.py:1557-1563`; no parcial, argumento em `:1600-1602`; chamado vivo espelha em `:1609-1611`.
- Nascimento grava a lista recebida em `app/ponto/management/commands/detectar_ausencias.py:700-706`; se argumento vazio/None, recua para entrada prevista em `:475-497`. Descrição lê em `:670-679`. Chamado existente vivo atualiza via `_espelhar_marcos` em `:738-739`; este compara e grava através da porta `chamados.services.contexto.renovar_contexto` em `:554-559`; porta efetivamente grava em `app/chamados/services/contexto.py:96-102`.
- Leitores diretos sem mudança no delta: `app/chamados/juizes.py:506` (marco por motivo/tipo); `app/ponto/services/lastro.py:75` (todos os marcos cobrados, lastro/fechamento); `app/ponto/supra_juiz.py:129` (texto de retratação); `app/escala/utils.py:1039` (badges de chamados por célula); `app/escala/services/regeneracao.py:107` e `:156` (cobrança órfã/relato); `app/ponto/management/commands/gerar_celulas.py:251` (marcar órfã); `app/ponto/management/commands/reconciliar_grade.py:448` (reconstruir contexto); `app/ponto/management/commands/curar_residuo_bug115.py:73` e `app/chamados/management/commands/nomear_marco_por_lampada.py:50` (selecionar sem marco).
- Leitores indiretos relevantes: materialização consulta `entrada_de_marco` em `app/chamados/services/materializacao.py:259` e usa tipo em `:260-261`; emissão de perguntas de intervalo lê lista em `app/chamados/services/disputa_emissao.py:947-958`. A criação em `app/chamados/models.py:882` entrega os marcos dos defaults para a guarda de abertura.
- Outros produtores diretos: `app/chamados/reconciliador.py:677-686` monta contexto de nascimento a partir da ata; `app/ponto/management/commands/reconciliar_grade.py:473` renova pela porta e `:518` prepara defaults; `app/chamados/management/commands/nomear_marco_por_lampada.py:109` renova pela porta. Estes não mudam no delta.

**Candidato adicional a verificar no consolidado:** chamado VIVO, dia parcial **sem batida**, contexto antigo cobrando 08/12/13/17, nova ausência 13–17. Correção `n_bat=0` tira o dia de `FURO_PARCIAL`; com cobrança viva, também não entra no furo sem cobrança (`supra_juiz.py:313-316`). Como 08/12 continuam apagados, `marcos_acesos=False` (`lastro.py:174-180`) e chamado permanece vivo. O único espelhamento do contexto no cartório continua subordinado a `FURO_PARCIAL` (`cartorio.py:1583-1611`), deixando 13/17 dispensados no contexto. Precisaria consolidar com E2/perguntas antes de fixar gravidade e efeito sobre novas perguntas; o contexto stale, nesse caminho, é consequência estática da fonte citada.

Não há `.git` na cópia: comparação histórica direta dos objetos/commit [nao verificado]; comparação antes/depois acima utiliza o full fornecido pelo usuário. [nao verificado]

## 31/10 23:59 · sessao 3e4869b912cd · - · - · origem=Ronald (tmux)

Achado adicional procedente: **P1 — o leitor da ata continua contando marco dispensado como batida/casamento**, e isso suprime o furo sem batida nos dois comandos que usam `grade_da_celula`. A conta é `n_celulas - n_missing`, sem ler hora, delta ou `dispensada`; viaja como `n_ok`, e o supra prefere esse valor à contagem com hora. O portão do furo exige `n_ok == 0`. (`app/escala/services/leitor_celula.py:83`, `app/escala/services/leitor_celula.py:361`, `app/ponto/supra_juiz.py:229`, `app/ponto/supra_juiz.py:313`)

Contraexemplo: dia 08/12/13/17, adesão anterior, nenhuma batida no dia, parcial 13–17. A ata tem quatro lâmpadas, duas apagadas e duas dispensadas: `n_ok_lavrado=2`. O cartório corrigido conta `n_bat=0` e acusa o resto; os comandos `supra_juiz` e `emitir_furo_retroativo` recebem `n_ok=2`, não entram no portão sem batida e não têm fato para os ramos seguintes. O segundo comando somente emite quando encontra `FURO_SEM_COBRANCA`. (`app/escala/utils.py:1207`, `app/ponto/services/cartorio.py:1204`, `app/escala/services/leitor_celula.py:83`, `app/ponto/management/commands/supra_juiz.py:206`, `app/ponto/management/commands/emitir_furo_retroativo.py:99`, `app/ponto/management/commands/emitir_furo_retroativo.py:109`, `app/ponto/supra_juiz.py:376`)

Outro consumidor da mesma conta trata a dispensa como fato para ultrapassar a fronteira de admissão/demissão: `_teve_fato` usa `_n_ok > 0`, sem hora/delta. Contraexemplo: ata com dispensa e nenhuma batida antes da admissão deixa de virar `fora_vinculo`. (`app/escala/services/leitor_celula.py:335`, `app/escala/services/leitor_celula.py:339`)

Censo de produção das ocorrências de `not …get('missing')` e variantes relevantes:

| Sítio | Lê hora/delta? | Resultado |
|---|---|---|
| `app/inteligencia/calculadores.py:153` | Não | Ainda conta não missing como batido; só processa `tipo_dia='trabalho'`, portanto a parcial rotulada ausência não entra (`:141`). |
| `app/ponto/propositor.py:40` | A seleção não; valida hora depois (`:52`) | Parcial excluída pelo tipo (`:36`); não gera proposta com a dispensa sem hora. |
| `app/ponto/propositor.py:213` | Delta não nulo | Dispensa possui `delta=None`; excluída. |
| `app/ponto/management/commands/propor_reorganizacao.py:102` | Hora (`:103`) | Dispensa sem hora excluída. |
| `app/ponto/supra_juiz.py:89` | Hora (`:90`) | Dispensa sem hora excluída na recontagem; **`n_ok` lavrado contorna essa proteção**, conforme achado acima. |
| `app/ponto/turnos.py:2675` | Hora | Dispensa excluída. |
| `app/ponto/services/cartorio.py:478` | Hora | Dispensa excluída das horas reais. |
| `app/ponto/services/cartorio.py:503` | Hora | Dispensa excluída das horas usadas. |
| `app/ponto/services/cartorio.py:532` | Não | Estado de lâmpada; acende a dispensada sem inventar luz (`:563`) e propaga marcador (`:565`). |
| `app/ponto/services/cartorio.py:1205` | Não | Agora exclui explicitamente `dispensada`. |
| `app/ponto/status_dia.py:144` | Não | Agora exclui `isencao` e `dispensada` (`:145`). |
| `app/escala/utils.py:812` | Hora | Guarda para preservar hora; ausência de hora vai ao crédito de isenção (`:820`). |
| `app/escala/utils.py:1958` | Não nessa guarda | Apenas decide se ponta está missing; dispensada impede substituir a ponta. |
| `app/escala/utils.py:1990` | Hora pertence ao turno e é posterior (`:1991`) | Dispensa sem hora não satisfaz. |
| `app/chamados/detector_anomalias.py:212` | Hora alvo e tipo real (`:213`) | Usa células recém produzidas por `_match_marcos` (`:210`), sem overlay de dispensa. |
| `app/chamados/services/declaracao_texto.py:107` | Não | Seleciona faltantes, não conta batidas; além disso ignora ausência parcial por `tipo_dia != trabalho` (`:95`). |
| `app/ponto/janela_he.py:294` | Não nessa guarda | Seleciona somente missing para pendência de HE, não conta batidas. |
| `app/ponto/services/veredito_celula.py:251` | Não nessa guarda | Seleciona missing para simulação, não conta batidas. |

Variantes aritméticas: `n_ok_lavrado` e `n_casadas_lavrado` continuam contando dispensa; `_teve_batida_na_ata` também acredita em `n_ok`, porém seu chamador filtra `tipo_dia='trabalho'`, excluindo a ausência parcial. (`app/escala/services/leitor_celula.py:83`, `app/escala/services/leitor_celula.py:89`, `app/ponto/services/bordas_realizado.py:51`, `app/ponto/services/bordas_realizado.py:74`)

Mapa direto de `dispensada`:

- Escritor da célula parcial: grade em `app/escala/utils.py:1207`.
- Propagadores: célula → lâmpada em `app/ponto/services/cartorio.py:446`; lâmpada → célula em `app/escala/services/leitor_celula.py:119`.
- Outro escritor é a dispensa integral da ata, que marca `acesa=True`, `luz=None`, `dispensada=motivo`; não é escritor concorrente do overlay parcial da grade. (`app/ponto/services/cartorio.py:649`)
- Leitores diretos restantes: impressão em `app/ponto/services/cartorio.py:335`; `n_bat` em `app/ponto/services/cartorio.py:1205`; status em `app/ponto/status_dia.py:145`; supressão do furo quando não resta marco em `app/ponto/supra_juiz.py:312`.

`tem_batida` é escrito pelo retorno de `fatos_do_status` e lido por `status_da_lavra` através da porta. Os três chamadores de produção de `fatos_do_status` estão no cartório: completar status, nunca bateu e lavra comum. (`app/ponto/status_dia.py:148`, `app/ponto/portas/celula.py:698`, `app/ponto/services/cartorio.py:765`, `app/ponto/services/cartorio.py:1078`, `app/ponto/services/cartorio.py:1619`) O calendário também possui uma chave homônima, mas a escreve de entradas/saídas próprias; não lê esse retorno. (`app/colaboradores/services/calendario.py:810`)

Os testes novos do cartório não exercitam `grade_da_celula → n_ok_lavrado → classificar_dia`: os casos de ausência sem batida chamam `julgar_colab`; o RED puro de status passa um dicionário direto. Assim não mordem o achado do leitor. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:295`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:308`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:324`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:236`)

Não executei testes nem consultei banco/logs; ocorrência e volume em produção [nao verificado]. `git grep` não pôde ser executado porque a cópia não contém `.git`; usei `rg` sobre os arquivos presentes [nao verificado]. [nao verificado]

## 31/10 23:59 · sessao 9245d15d5a00 · - · - · origem=Ronald (tmux)

Não encontrei contraexemplo temporal novo no corpo autorizado; as salvaguardas sustentam as propriedades solicitadas para `datetime` válidos, dentro dos limites representáveis. (`app/ponto/motor_calculo_v2.py:162`, `app/ponto/motor_calculo_v2.py:188`, `app/ponto/motor_calculo_v2.py:196`)

- **Laço parado / `b <= a`: NÃO procede.** Se a próxima hora não avança, soma um minuto; como `_f > a`, o mínimo entre `_f` e a próxima ponta continua maior que `a`. (`app/ponto/motor_calculo_v2.py:179`, `app/ponto/motor_calculo_v2.py:186`, `app/ponto/motor_calculo_v2.py:188`, `app/ponto/motor_calculo_v2.py:190`, `app/ponto/motor_calculo_v2.py:194`)
- **Retorno negativo / acima da união / acima de `N(entrada,saida)`: NÃO procede.** A união elimina sobreposição; cada trecho contribui no máximo sua duração positiva; o retorno tem teto em `N`, que soma exclusivamente durações positivas ou retorna zero. (`app/ponto/motor_calculo_v2.py:164`, `app/ponto/motor_calculo_v2.py:167`, `app/ponto/motor_calculo_v2.py:169`, `app/ponto/motor_calculo_v2.py:193`, `app/ponto/motor_calculo_v2.py:196`, `app/ponto/motor_calculo_v2.py:91`, `app/ponto/motor_calculo_v2.py:115`)
- **Borda noturna interior com segundos: não encontrei.** O corte usa a próxima hora de parede do instante normalizado; o passo de um minuto é condicionado à ausência de avanço. (`app/ponto/motor_calculo_v2.py:186`, `app/ponto/motor_calculo_v2.py:188`)
- Aware de qualquer fuso entra por `astimezone(TZ_LOCAL)`; naive entra por `TZ_LOCAL.localize`; o cursor normaliza novamente antes de escolher a hora seguinte. (`app/ponto/motor_calculo_v2.py:77`, `app/ponto/motor_calculo_v2.py:79`, `app/ponto/motor_calculo_v2.py:186`)
- Não há tratamento explícito de overflow dos limites de `datetime`; isso pode produzir exceção e não autoriza afirmar que a função retorna para absolutamente qualquer valor representável. (`app/ponto/motor_calculo_v2.py:186`, `app/ponto/motor_calculo_v2.py:104`, `app/ponto/motor_calculo_v2.py:117`)

Executei em memória 7.280 combinações ao redor das transições históricas de São Paulo disponíveis no `pytz==2024.1`, com segundos e ambos os valores de prorrogação, sem divergências frente à interseção/unificação explícita das janelas; não gravei artefato dessa execução. [não verificado] [nao verificado]

Valores obtidos nos casos já declarados pelos testes históricos, todos sem prorrogação: 1914 `N=414`, desconto `354`; entrada do verão de novembro/2018 `N=360`, desconto `270`; saída do verão de fevereiro/2018 `N=480`, desconto `450` minutos. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:415`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:419`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:423`, `app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:193`)

## 31/10 23:58 · sessao 4c4eacfa057d · - · - · origem=Ronald (tmux)

Censo dos chamadores produtivos: existem **dois usos diretos** do nome `minutos_noturnos_dos_intervalos`: `he_noturna` e o wrapper `MotorBase._noturno_dos_intervalos`; não existe terceira chamada direta produtiva encontrada pelo nome. `app/ponto/motor_calculo_v2.py:442`, `app/ponto/motor_calculo_v2.py:1915`.

| Chamador/leitor | Antes → depois, caso 22:00–07:00, pausa 05:30–06:30, pr=True |
|---|---|
| `MotorBase._noturno_dos_intervalos` | Desconto **0 → 60 min**; só repassa o parâmetro cadastrado. `app/ponto/motor_calculo_v2.py:1915` |
| `MotorBase.calcular_noturnas` via wrapper | Relógio **540 → 480 min**; legais são esse resultado multiplicado pelo fator cadastrado **1 ou 60/52,5**. `app/ponto/motor_calculo_v2.py:971`, `app/ponto/motor_calculo_v2.py:973`, `app/ponto/motor_calculo_v2.py:980` |
| `regras.noturnas_do_dia` via método emprestado | Relógio **540 → 480 min**, mesmo fator; método emprestado é o próprio `MotorBase._noturno_dos_intervalos`. `app/ponto/calculador/regras.py:63`, `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:88` |
| `he_noturna`, HE50=120, HE100=0 | A cauda de 120 minutos trabalhados começa às **04:00**; N(04–07)=180; desconto **0 → 60**; retorna **(180,0) → (120,0) min**. `app/ponto/motor_calculo_v2.py:392`, `app/ponto/motor_calculo_v2.py:400`, `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:442`, `app/ponto/motor_calculo_v2.py:450` |

Outras formas mudam: pausa repetida 01:00–02:00 no mesmo recorte passa de **120 → 60 min**; pausa igual ao recorte de 26h, 21:00–23:00 do dia seguinte, com pr=True, passa de **1560 → 1500 min**; recorte acima de 26h com pausa noturna de 60 min passa de **60 → 0 min**; ausência de intervalos permanece **0 → 0**. O corpo novo unifica pausas e limita pelo todo; o antigo está preservado como `_por_peca`. `app/ponto/motor_calculo_v2.py:164`, `app/ponto/motor_calculo_v2.py:169`, `app/ponto/motor_calculo_v2.py:193`, `app/ponto/motor_calculo_v2.py:196`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:88`.

Outra mudança de contrato: pontas naive com pausa não vazia podiam produzir `TypeError`, porque o corpo antigo localizava as pausas e as comparava com entrada/saída ainda naive. O novo localiza também entrada/saída. Não procede dizer que só o número de minutos muda para qualquer forma de entrada. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:92`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:94`, `app/ponto/motor_calculo_v2.py:162`.

O caminho produtivo do calculador diário **passa `intervalos=()`**, usando pares já quebrados nos segmentos trabalhados; portanto, não procede afirmar que esse chamador produtivo recebe a mesma forma de entrada do RED com par inteiro + pausa. Seu desconto direto continua 0. As pontas de HE continuam sendo as pontas do período inteiro, mas sem intervalos nessa chamada; a cura não resolve essa alimentação, fora do escopo. `app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:96`, `app/ponto/calculador/alimentacao.py:121`, `app/ponto/calculador/alimentacao.py:145`, `app/ponto/calculador/alimentacao.py:512`, `app/ponto/calculador/alimentacao.py:514`.

O adaptador V3 por par, por sua vez, fornece os intervalos julgados ao mesmo `noturnas_do_dia` e grava de volta os dois resultados no período. `app/ponto/calculador/periodo.py:507`, `app/ponto/calculador/periodo.py:751`, `app/ponto/calculador/periodo.py:752`, `app/ponto/calculador/periodo.py:753`.

Rastreio de origem e gravação:

- **Pontas/pausas:** `Batida.timestamp` é o campo persistido; `parear_turnos` recebe as batidas, e o motor adapta seus turnos para períodos/intervalos. Havendo ata suficiente, `_periodos_pelo_marco` lê `periodos_do_dia`, recupera o instante exato da batida e preenche `intervalos[ent]` com todas as pausas julgadas. `app/ponto/models.py:23`, `app/ponto/turnos.py:1190`, `app/ponto/motor_calculo_v2.py:530`, `app/ponto/motor_calculo_v2.py:602`, `app/ponto/motor_calculo_v2.py:616`, `app/ponto/motor_calculo_v2.py:721`.
- **Segmento usado:** `_carimbar_segmento` escreve `efetivo_entrada`, `efetivo_saida`, `intervalos_do_motor`; o split lê esse último campo. `app/ponto/motor_calculo_v2.py:1828`, `app/ponto/motor_calculo_v2.py:1830`, `app/ponto/motor_calculo_v2.py:1006`.
- **Prorrogação:** cadastro `Sindicato.prorrogacao_noturna_pos5h`; régua CLT por ciclo ou CCT; `get_motor_cct` fornece o valor ao motor, `MotorBase` o armazena, wrapper e `he_noturna` o leem. O diff não cria outro decisor da prorrogação. `app/core/models.py:72`, `app/core/regua_cct.py:153`, `app/core/regua_cct.py:454`, `app/core/regua_cct.py:466`, `app/ponto/motor_calculo_v2.py:758`, `app/ponto/motor_calculo_v2.py:762`, `app/ponto/motor_calculo_v2.py:1916`, `app/ponto/motor_calculo_v2.py:1005`.
- **Minutos noturnos do período:** `_escrever_noturnas` chama `calcular_noturnas` e escreve `PeriodoCalculo.minutos_noturnos_relogio/legais`; os três ramos passam por ele. `app/ponto/motor_calculo_v2.py:991`, `app/ponto/motor_calculo_v2.py:992`, `app/ponto/motor_calculo_v2.py:993`, `app/ponto/motor_calculo_v2.py:2780`, `app/ponto/motor_calculo_v2.py:2884`, `app/ponto/motor_calculo_v2.py:3235`.
- **HE noturna:** `_aplicar_he_noturna` lê entradas, extras e pausas do período e escreve `minutos_extra_50_noturna/100_noturna`; a soma mensal divide por 60. `app/ponto/motor_calculo_v2.py:1002`, `app/ponto/motor_calculo_v2.py:1008`, `app/ponto/motor_calculo_v2.py:1010`, `app/ponto/motor_calculo_v2.py:340`, `app/ponto/motor_calculo_v2.py:344`.
- **Fechamento mensal:** soma `minutos_noturnos_legais` de períodos normais e folgas; fechamento atribui as três rubricas arredondadas e salva/atualiza os campos persistidos. `app/ponto/motor_calculo_v2.py:321`, `app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:896`, `app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1112`, `app/ponto/services/fechamento.py:1131`, `app/ponto/services/fechamento.py:1134`, `app/ponto/services/fechamento.py:1137`; campos `app/ponto/models.py:413`, `app/ponto/models.py:416`, `app/ponto/models.py:419`.
- **DiaPago motor:** `lavrar` soma os três campos por dia e persiste pela porta existente, `bulk_create`; folga inclui noturnas. `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:181`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:224`, `app/ponto/services/dia_pago.py:343`, `app/ponto/services/dia_pago.py:358`; campos `app/ponto/models.py:327`, `app/ponto/models.py:329`, `app/ponto/models.py:332`.
- **DiaPago oráculo:** `do_dia` devolve as rubricas das chamadas compartilhadas; fechamento entrega o resultado a **essa mesma** `lavrar`, com `versao='oraculo'`; `lavrar` substitui os campos declarados antes de persistir. `app/ponto/calculador/regras.py:823`, `app/ponto/calculador/regras.py:828`, `app/ponto/calculador/regras.py:830`, `app/ponto/calculador/regras.py:840`, `app/ponto/services/fechamento.py:1239`, `app/ponto/services/fechamento.py:1245`, `app/ponto/services/fechamento.py:1256`, `app/ponto/services/fechamento.py:1262`, `app/ponto/services/dia_pago.py:322`, `app/ponto/services/dia_pago.py:328`.
- **Leitores finais demonstrados:** apresentação diária lê `DiaPago.horas_noturnas`; exportador lê `FechamentoMensal.horas_noturnas` no evento `adicional_noturno`. `app/ponto/services/dia_pago.py:922`, `app/folha/export.py:436`.

Classes reprovadas: **segundo escritor fora da porta não procede para este diff**, que retorna um escalar e usa as portas existentes; **segundo juiz/derivação paralela não procede no corpo**, que consulta o mesmo `minutos_noturnos` e mantém o cadastro como fonte da prorrogação, sem replicar constantes 22/05; **relógio fora da borda não procede no corpo**, que lê apenas argumentos e configuração de fuso. A autoridade de horas por par continua registrada no motor. `app/ponto/motor_calculo_v2.py:162`, `app/ponto/motor_calculo_v2.py:186`, `app/ponto/motor_calculo_v2.py:191`, `app/ponto/motor_calculo_v2.py:196`, `app/ponto/services/dia_pago.py:358`, `app/core/juizes.py:1160`.

Ordem de trava e pouso antes do pré-requisito: o corpo não executa escrita nem trava; ocorrência operacional nesta obra **[não verificado]**. Impacto real em pessoas/competências ou campos já gravados **[não verificado]**: o dado fornecido não contém comparação de produção, e os relatos históricos do repositório não equivalem a uma sonda deste HEAD. [nao verificado]

## 31/10 23:58 · sessao da27367c06d2 · - · - · origem=Ronald (tmux)

Questão 3:

- **Não procede import inexistente no HEAD.** `_aw` é definido no próprio teste, e `TZ_LOCAL`, `localizar`, `minutos_noturnos`, `minutos_noturnos_dos_intervalos` e `he_noturna` existem no motor. Django é dependência declarada, embora não esteja instalado neste ambiente. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:39`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:44`, `app/ponto/motor_calculo_v2.py:28`, `app/ponto/motor_calculo_v2.py:73`, `app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:121`, `app/ponto/motor_calculo_v2.py:409`, `app/requirements.txt:13`; disponibilidade local de Django [não verificado por arquivo:linha].
- **Não procede incompatibilidade de argumentos.** Todos os keywords de `_r()` pertencem à assinatura; `ponta=(entrada,saida,120.0,0.0)` tem exatamente os quatro valores desempacotados. `pontualidade=None` declara ausência dessa entrada, sem afetar as rubricas verificadas. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:60`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61`, `app/ponto/calculador/regras.py:572`, `app/ponto/calculador/regras.py:623`, `app/ponto/calculador/regras.py:818`.
- As expectativas centrais são aritmética explicitada: 480−360=120 minutos extras, pausa excluída, janela prorrogada e fator 1,0. Porém **não procede dizer que toda expectativa decorre apenas da CLT**: escolher os últimos minutos trabalhados como HE, o gatilho pelo toque em 00–05 e devolver zero acima de 26h são políticas do código/casa, reconhecidas no teste. `_regra` constrói janela e união sem chamar `minutos_noturnos`, mas reutiliza `localizar`; não é oráculo independente da localização temporal. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:14`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:160`, `app/ponto/motor_calculo_v2.py:93`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:413`.
- **Não procede patch ineficaz por alias importado.** `_celula`, `_f`, os casos históricos e os de segundos importam a função dentro da chamada. O caminho integrado importa `he_noturna`, cujo corpo resolve o nome global patchado no módulo do motor. O outro caminho empresta `_noturno_dos_intervalos`, cujo corpo também resolve esse global. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:105`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:194`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:353`, `app/ponto/calculador/regras.py:63`, `app/ponto/calculador/regras.py:86`, `app/ponto/calculador/regras.py:816`, `app/ponto/motor_calculo_v2.py:442`, `app/ponto/motor_calculo_v2.py:1915`.
- Não há leitura de “agora” no teste ou nos caminhos exercitados; as datas e durações são explícitas. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:47`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:187`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:395`.
- Há ressalva: `feriados=set()` **não fecha a fonte de feriados**. A ausência da data no conjunto chama fallback ORM; `is_feriado` captura qualquer exceção e devolve `False`. Assim o caso “dia comum” pode passar porque a consulta foi recusada pelo framework, não porque o conjunto vazio seja autoritativo. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66`, `app/ponto/motor_calculo_v2.py:951`, `app/ponto/motor_calculo_v2.py:39`, `app/core/feriados.py:95`, `app/core/feriados.py:82`, `app/core/feriados.py:28`. Comportamento concreto da recusa do Django instalado [não verificado].

Executei os **34 testes em memória**, com código real do motor/regras/teste, substituindo somente a infraestrutura ausente: `SimpleTestCase` por `unittest.TestCase`, settings pelo `TIME_ZONE` configurado, módulo de modelos pela função real `piso_intervalo_art71` extraída por AST, e o fallback de feriado por exceção de recusa de DB. Todos passaram, inclusive os MORDE; `_r()` produziu HEAD `(HE50,HE100,HE50not,noturnas)=(2,0,2,8)` e mutante antigo `(2,0,3,9)`. São resultados de execução em memória [não verificado por arquivo:linha]; **suite Django real verde [não verificado]**. Configuração/função usadas: `app/config/settings/base.py:93`, `app/escala/models.py:6`.

## 31/10 23:58 · sessao c2a56091f44e · GRADE-FOLGA-JUIZ · verificador · origem=Code

**Ainda bloqueia o pouso:** `medir` corrigiu o vínculo usado para selecionar as folgas, mas continua contando conflitos em dias de outro vínculo ou sem vínculo. O rótulo promete um recorte que o laço da contagem não aplica. (`app/escala/services/fase_conflitante.py:99`, `app/escala/services/fase_conflitante.py:105`, `app/escala/services/fase_conflitante.py:123`)

As referências abaixo são da cópia indicada. Identidade Git do hash, execução da suíte Django, RED histórico, remedição de frota, snapshot atual e hashes de produção: **[não verificado]**. [nao verificado]

**1. Quem escreve, quem lê e de onde vêm os vínculos**

O carregador busca todas as `FolgaDia` da **pessoa**, ordenadas por PK. O juiz percorre as candidatas até encontrar uma válida; o predicado aceita manual independentemente do vínculo e aceita calendário somente quando seu `escala_colaborador_id` corresponde ao vínculo recebido. (`app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:212`, `app/ponto/precedencia.py:235`)

| Sítio | Vínculo entregue ao juiz e sua origem |
|---|---|
| `veto_de_lancamento` | `escala_vigente(colab,d)`, somente quando há candidatas. (`app/ponto/precedencia.py:501`, `app/ponto/precedencia.py:505`) |
| `declarar_dia._folga_do_dia` | `_ec_do_dia(colaborador,data)`, que delega a `escala_vigente`. (`app/ponto/services/declarar_dia.py:56`, `app/ponto/services/declarar_dia.py:74`) |
| `rejulgar_ausencia_em_folga.Command.handle` | `escala_vigente(cid,d)`, para cada dia com candidatas dos colaboradores com ausência aprovada. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:63`) |
| `fase_conflitante.medir` | `_vdd = vinculo_do_dia(...)`, alimentado com células do intervalo das candidatas e vínculos carregados com `mais=` das geradoras. Só aceita sementes quando `_vdd.pk == ec.pk`. (`app/escala/services/fase_conflitante.py:94`, `app/escala/services/fase_conflitante.py:99`) |

Nos três primeiros, `escala_vigente` carrega a célula com sua geradora; havendo geradora, entrega-a à juíza; sem ela, carrega os candidatos do período. Portanto, **`escala_vigente` não significa simplesmente “o ativo”**. (`app/escala/servico_jornada.py:46`, `app/escala/servico_jornada.py:55`)

A regra final é: geradora existente e dentro das datas vence; geradora fora das datas devolve `None`, sem escolher outro vínculo; sem geradora, escolhe entre os que cobrem o dia, priorizando ativo e início mais recente. (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:247`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:253`)

O rastreio dos contratos de produção tocados pelo diff fica assim:

| Função/saída | Quem produz/escreve | Quem lê, inclusive fora do diff |
|---|---|---|
| `folgas_lancadas_do_periodo` | Constrói `{data: [FolgaDia,…]}`; não grava tabela. (`app/ponto/precedencia.py:205`) | Os quatro sítios acima, `fatos_do_periodo` e overlay da grade. (`app/ponto/precedencia.py:281`, `app/escala/utils.py:1119`) |
| `folga_lancada_vale_no_dia` | Predicado puro. (`app/ponto/precedencia.py:235`) | `folga_lancada_do_dia`. (`app/ponto/precedencia.py:217`) |
| `folga_lancada_do_dia` | Seleciona uma candidata; não persiste. (`app/ponto/precedencia.py:216`) | Quatro sítios, precedência e grade. (`app/ponto/precedencia.py:357`, `app/escala/utils.py:1124`) |
| `fatos_do_periodo` / `fatos_do_dia` | Produzem `faixa`, `tipo_do_dia`, `era_dia_previsto`, `suprimidos` e `fatos`, incluindo `folga_lancada`. (`app/ponto/precedencia.py:256`, `app/ponto/precedencia.py:397`) | Calendário, espelho, cartório, status, reavaliação de chamados, férias e folha. (`app/colaboradores/services/calendario.py:617`, `app/ponto/services/espelho.py:659`, `app/ponto/services/cartorio.py:1694`, `app/ponto/status_dia.py:139`, `app/chamados/services/feriado_reavaliar.py:33`, `app/ferias/situacao.py:94`, `app/folha/services/sem_furo.py:25`) |
| `veto_de_lancamento` | Devolve autorização ou levanta veto; não grava a folga. (`app/ponto/precedencia.py:505`) | Portas de folga, ausência, férias e declaração. (`app/escala/folgas.py:258`, `app/escala/folgas.py:283`, `app/ponto/services/ausencia.py:380`, `app/ferias/models.py:428`, `app/ferias/views.py:253`, `app/ponto/services/declarar_dia.py:120`) |
| `_folga_do_dia` / `o_dia_ja_e_folga` | Adaptam a resposta do juiz. (`app/ponto/services/declarar_dia.py:74`, `app/ponto/services/declarar_dia.py:95`) | `_recusa_do_dia` e validação das respostas do admin. (`app/ponto/services/declarar_dia.py:112`, `app/chamados/services/validacao.py:261`, `app/chamados/services/validacao.py:316`) |
| `_recusa_do_dia` | Produz mensagem; a leitura da origem escolhe a frase depois da seleção pelo juiz. (`app/ponto/services/declarar_dia.py:112`) | Preview `checar_declaracao`, usado pelo ato `declarar_dia`. (`app/ponto/services/declarar_dia.py:147`, `app/ponto/services/declarar_dia.py:160`) |
| `_aplicar_folgas_foto` | Altera somente `tipo_dia`, `celulas` e `orfas` do dicionário da grade. (`app/escala/utils.py:1124`) | Builders individual e por turno; depois cartório, espelho, PDF e simulação. (`app/escala/utils.py:770`, `app/escala/utils.py:1794`, `app/ponto/services/cartorio.py:946`, `app/ponto/services/espelho.py:496`, `app/relatorios/pdf_espelho.py:193`, `app/ponto/services/simular_dia.py:7`) |
| `folga_humana_ou_feriado` | Lê os fatos da precedência e produz booleano. (`app/ponto/precedencia.py:186`) | Calendário e fatos do status lavrado. (`app/colaboradores/services/calendario.py:820`, `app/ponto/status_dia.py:139`) |
| `trabalho_do_dia` | Normaliza a previsão para o calendário usando faixa/folga humana. (`app/colaboradores/services/calendario.py:102`) | `_classificar` e publicação de `eh_trabalho`. (`app/colaboradores/services/calendario.py:126`, `app/colaboradores/services/calendario.py:840`) |
| `_classificar` / `contexto_calendario` | Produzem `status`, `status_rotulo`, `eh_trabalho` e resumo em memória. (`app/colaboradores/services/calendario.py:839`, `app/colaboradores/services/calendario.py:903`, `app/colaboradores/services/calendario.py:954`) | View, portas de status/contagem, drawer, template e oferta de “Resolver dia”. (`app/colaboradores/views.py:1946`, `app/colaboradores/services/calendario.py:223`, `app/colaboradores/services/calendario.py:234`, `app/core/juizes.py:848`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/colaboradores/services/calendario.py:202`) |
| `medir`, `_foto_diz`, `_ancora_diz` | Produzem total, vínculos e amostra; as duas auxiliares alimentam a comparação. (`app/escala/services/fase_conflitante.py:38`, `app/escala/services/fase_conflitante.py:57`, `app/escala/services/fase_conflitante.py:107`) | `lavrar_fase_conflitante`. (`app/escala/management/commands/lavrar_fase_conflitante.py:31`) |
| `REGRA_VERSOES` | Declaração passa a incluir v3/2026-10. (`app/ponto/services/cartorio.py:111`) | `versao_para`; o resultado entra na impressão dos insumos e decide o pulo por impressão. (`app/ponto/services/cartorio.py:209`, `app/ponto/services/cartorio.py:367`, `app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:1173`) |
| `JUIZES` e documentação derivada | Registro recebe a autoridade; lookup recebe seu endereço. (`app/core/juizes.py:192`, `app/docs/JUIZES.md:84`) | Gerador dos mapas/diagrama e selo de correspondência. (`app/core/management/commands/gerar_diagrama.py:751`, `app/core/tests/test_selo_diagrama_do_codigo.py:46`) |

Outros leitores da precedência alcançados pela mudança são os serviços de dia do colaborador, deslogados, ausência de batida hoje, faltas de hoje e semana do posto. (`app/ponto/services/dia_do_colab.py:14`, `app/ponto/services/deslogados.py:166`, `app/ponto/services/abriu_nao_bateu.py:123`, `app/ponto/services/faltas_de_hoje.py:61`, `app/escala/services/semana_do_posto.py:177`)

Os campos persistidos envolvidos têm os seguintes escritores e leitores relevantes:

| Campos | Quem escreve | Quem lê |
|---|---|---|
| `FolgaDia.data`, `escala_colaborador`, `calendario_origem`, `tipo` | Materialização do calendário; lançamento/alteração manual; reparentamento. (`app/escala/folgas.py:165`, `app/escala/folgas.py:254`, `app/escala/folgas.py:274`, `app/escala/folgas.py:435`) | Carregador da pessoa e predicado; permanecem leitores por vínculo no gerador/cadastro. `tipo` é rótulo informativo, não critério do juiz novo. (`app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:237`, `app/escala/models.py:1257`, `app/escala/views_plano_folgas.py:60`, `app/escala/models.py:1562`) |
| `EscalaColaborador.colaborador`, `tipo_escala`, `data_inicio`, `data_fim`, `ativa`, `data_ancora_colaborador`, `trabalha_em_feriado` | Portas de vínculo criam, atualizam, encerram e corrigem trechos. (`app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:278`, `app/colaboradores/services/vinculo.py:797`, `app/colaboradores/services/vinculo.py:832`) | Carregadores/juíza, precedência, grade e métrica. (`app/escala/alimentacao.py:116`, `app/escala/alimentacao.py:182`, `app/ponto/precedencia.py:372`, `app/escala/services/fase_conflitante.py:79`, `app/escala/services/fase_conflitante.py:46`) |
| `CelulaDia.trabalha`, `escala_geradora`, `dna` | Geração, regeneração, lote e reversão pela porta da célula. (`app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:530`, `app/ponto/portas/celula.py:592`) | Juíza do vínculo, precedência, calendário e builders. (`app/escala/alimentacao.py:237`, `app/ponto/precedencia.py:348`, `app/colaboradores/services/calendario.py:785`, `app/escala/utils.py:1260`) |
| `Ausencia.colaborador`, `tipo`, `data`, `data_fim`, `status` | Porta de ausência cria, edita e muda estado. (`app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:417`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:808`) | Precedência e comando de rejulgamento; expansão dos dias delegada a `dias_da_ausencia`. (`app/ponto/precedencia.py:273`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:33`) |
| `Empresa.ativa`, `dia_inicio_competencia` | Formulário e portas de estrutura. (`app/colaboradores/views.py:496`, `app/colaboradores/views.py:521`, `app/colaboradores/services/estrutura.py:12`) | Comando seleciona empresas ativas; juiz das janelas lê o corte. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:54`, `app/ponto/janelas.py:42`) |
| `CelulaDia.ata`, veredito/status; impressão/carimbos | Cartório monta a ata; `lavrar_veredito` e `carimbar` persistem pela porta. (`app/ponto/services/cartorio.py:574`, `app/ponto/portas/celula.py:709`, `app/ponto/portas/celula.py:787`) | Folha, apresentação e reconciliação dos chamados. (`app/escala/models.py:1742`, `app/chamados/reconciliador.py:1304`) |
| `MetricaSnapshot` — numerador, denominador, detalhe, datas | Command de lavratura; `calculado_em` é automático. (`app/escala/management/commands/lavrar_fase_conflitante.py:35`, `app/inteligencia/models.py:96`) | `ultimo_lavrado` e API de saúde. (`app/escala/services/fase_conflitante.py:137`, `app/api/views_mensageria.py:643`) |

No calendário, o dicionário consumido pelo classificador contém também ausência descontável, futuro, isenção, suspeita de régua, batidas, turno aberto, desvio, estado da célula, veredito/via, cobertura e retenção da folha; todos são montados em `contexto_calendario`, e os leitores são `_classificar` e `trabalho_do_dia`. (`app/colaboradores/services/calendario.py:818`, `app/colaboradores/services/calendario.py:122`, `app/colaboradores/services/calendario.py:839`)

Na grade legacy, o `lambda _d: esc` não é um desvio nos dias publicados: o chamador seleciona os vínculos pela juíza, segmenta por vínculo e só retém os dias do segmento. No builder por turno, o overlay recebe diretamente o mapa diário. (`app/escala/utils.py:770`, `app/escala/utils.py:972`, `app/escala/utils.py:976`, `app/escala/utils.py:995`, `app/escala/utils.py:999`, `app/escala/utils.py:1794`)

Esse rastreio identifica os produtores e consumidores acima; exclusividade de todos os escritores e eventuais chamadas dinâmicas de todo o repositório **[não verificado]**.

**2. O que `medir` responde depois do delta**

Há duas etapas diferentes: **seleção das sementes** e **contagem dos dias por paridade**. Só a primeira consulta o vínculo do dia. (`app/escala/services/fase_conflitante.py:91`, `app/escala/services/fase_conflitante.py:99`, `app/escala/services/fase_conflitante.py:105`)

| Situação | Seleção da foto | Contagem posterior |
|---|---|---|
| Vínculos sobrepostos | A geradora válida decide; calendário do novo em dia gerado pelo velho não entra na foto do novo. (`app/escala/alimentacao.py:247`, `app/escala/services/fase_conflitante.py:100`) | Havendo outra semente válida do novo, pode contar também o dia do velho. (`app/escala/services/fase_conflitante.py:105`) |
| Dia sem célula | Usa o vínculo que cobre a data, com prioridade ativo/início recente. (`app/escala/alimentacao.py:253`) | Pode contar o dia sem célula pela fase extrapolada. (`app/escala/services/fase_conflitante.py:63`, `app/escala/services/fase_conflitante.py:107`) |
| Geradora fora da vigência | Juíza devolve `None`; não entra semente. (`app/escala/alimentacao.py:250`, `app/escala/services/fase_conflitante.py:100`) | O dia ainda pode ser contado a partir de outra semente. (`app/escala/services/fase_conflitante.py:105`) |
| Dono da célula inativo | Pode ser o vínculo do dia se suas datas o permitem; porém o laço externo só visita ativos. (`app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:252`, `app/escala/services/fase_conflitante.py:79`) | Não ganha linha própria; seu dia pode aparecer no conflito atribuído ao ativo. (`app/escala/services/fase_conflitante.py:100`, `app/escala/services/fase_conflitante.py:113`) |
| Manual presa ao velho | O juiz aceita; entra na foto do ativo se ele for o vínculo daquele dia. (`app/ponto/precedencia.py:237`, `app/escala/services/fase_conflitante.py:100`) | A fase dessa semente é extrapolada à janela inteira. (`app/escala/services/fase_conflitante.py:63`, `app/escala/services/fase_conflitante.py:105`) |
| Dia sem vínculo | Não entra semente, mesmo havendo manual aceita pelo predicado. (`app/ponto/precedencia.py:237`, `app/escala/services/fase_conflitante.py:100`) | Pode ser contado depois; o laço não consulta vínculo/vigência. (`app/escala/services/fase_conflitante.py:105`) |

**Contraexemplo:** no desenho do próprio teste parcial — janela 10–20/10, velho gerando 10–14, novo gerando 15–20, calendário do novo nos pares e âncora 20/07 — as sementes aceitas são 16/18/20, mas a conta inclui **11 dias**, começando em 10/10. A extrapolação de paridade e a ausência de filtro no laço explicam os cinco dias alheios. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:195`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:213`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:228`, `app/escala/services/fase_conflitante.py:63`, `app/escala/services/fase_conflitante.py:105`)

O teste parcial passa porque exige apenas um vínculo e seu PK; não exige total nem exclusão dos dias do velho. O teste com todos os dias gerados pelo velho exige efetivamente `0/0`. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:222`, `app/ponto/tests/test_grade_folga_juiz_sitios.py:228`)

O rótulo e `o_que_e` descrevem “folga lançada do dia” aceita com vínculo do dia, mas a conta compara **fase extrapolada de sementes filtradas** com âncora, em toda a janela. Extrapolar fase já era intenção histórica; apresentar isso como resposta diária do juiz e como recorte aos dias do vínculo não corresponde ao laço. (`app/escala/services/fase_conflitante.py:57`, `app/escala/services/fase_conflitante.py:105`, `app/escala/services/fase_conflitante.py:123`, `app/escala/services/fase_conflitante.py:129`, `app/escala/tests/test_fase_conflitante.py:57`)

O snapshot e sua exposição seguem este caminho:

- O command grava `numerador=total`, `denominador=vinculos` e resultado integral em `detalhe`; sem `--apply`, só imprime. (`app/escala/management/commands/lavrar_fase_conflitante.py:31`, `app/escala/management/commands/lavrar_fase_conflitante.py:35`)
- `ultimo_lavrado` devolve o **detalhe armazenado**, acrescentando `medido_em`; não atualiza rótulo de snapshot antigo. Sem snapshot, devolve `total=None` e `sem_medida=True`. (`app/escala/services/fase_conflitante.py:137`, `app/escala/services/fase_conflitante.py:139`)
- A API expõe `contadores.fase_conflitante`. Entretanto, `saude_da_operacao` percorre somente `metricas` e devolve a lista construída delas: esse caminho de contexto da IA descarta `contadores`. (`app/api/views_mensageria.py:643`, `mensageria/nucleo/ferramentas.py:259`, `mensageria/nucleo/ferramentas.py:265`, `mensageria/nucleo/ferramentas.py:294`)

**3. Catraca e mordidas**

| Forma | Resultado |
|---|---|
| `from … import FolgaDia as FD` | **Pega.** Recolhe `ImportFrom` e `asname`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:74`) |
| Import local | **Pega** o mesmo `ImportFrom` dentro de função. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:74`) |
| `import escala.models as m; m.FolgaDia…` | **Escapa.** A raiz `m` não é reconhecida como modelo. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:75`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:92`) |
| `getattr(fd, nome_em_variavel)` | **Escapa.** Exige string constante. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:55`) |
| `hasattr(fd, 'calendario_origem_id')` | **Escapa.** Essa chamada não é reconhecida. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:53`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:61`) |
| `operator.attrgetter(...)` | **Escapa.** Não corresponde às três formas examinadas. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:61`) |
| `values` / `values_list` com literal | **Pega** origem; pega filtro quando há pessoa e data. Variável ou `*campos` não é resolvido. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:43`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:95`) |
| Função em `__init__.py` | **Pega.** O nome do arquivo não é excluído; código em nível de módulo continua fora. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:106`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:124`) |
| Método `__init__` | **Pega** como `Classe.__init__`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:108`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:113`) |
| Import relativo de `FolgaDia` | **Pega.** Não exige módulo absoluto específico. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:75`) |
| Função aninhada dentro das três funções `JUIZ` | **Escapa.** O `continue` exclui a função e todos os descendentes. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:109`) |
| Método de classe em `precedencia.py` com nome igual ao juiz | **Pega.** A dispensa exige prefixo vazio. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:109`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:113`) |

Os **sete `MORDE` da catraca mordem os exemplos específicos que anunciam**: filtro pessoa/data; alias/manager; comparação da origem; cópia literal do predicado; cópia literal do carregador; leitura sem comparação; segundo juiz adicional em `precedencia.py`. O teste do alias cobre `from … import … as …`, não alias de módulo. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:183`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:188`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:194`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:198`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:209`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:217`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:223`)

“Só encolhe” **não está garantido historicamente**: o teste exige igualdade entre ach

[truncado em 20000 de 31031 caracteres]

## 31/10 23:55 · sessao 2055a3481e64 · - · - · origem=Ronald (tmux)

Nos quatro sítios, a escolha de vínculo deixou de ser própria: veto usa `escala_vigente(colab,d)` (`app/ponto/precedencia.py:501`); declaração usa `_ec_do_dia` (`app/ponto/services/declarar_dia.py:74`); rejulgamento usa `escala_vigente(cid,d)` (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`). A casca carrega célula e geradora, chama `vinculo_do_dia` com a geradora ou, sem ela, com os candidatos do período (`app/escala/servico_jornada.py:46`, `app/escala/servico_jornada.py:57`, `app/escala/servico_jornada.py:58`). O quarto sítio, métrica, ficou com o outro agente. [não verificado]

O legado da grade **não introduz um segundo feed incorreto nos dias publicados**: ele seleciona pela juíza, agrupa segmentos do mesmo vínculo, chama o builder individual com esse vínculo e só retém dias do segmento; o aquecimento adicional é descartado (`app/escala/utils.py:972`, `app/escala/utils.py:976`, `app/escala/utils.py:995`, `app/escala/utils.py:999`). O `lambda _d: esc` está no builder individual (`app/escala/utils.py:770`); a busca de produção encontrou a chamada desse builder somente no legado (`app/escala/utils.py:995`). O builder por turno alimenta `vinculo_do_dia` com todos os vínculos e células, publica o mapa diário e o passa ao overlay (`app/escala/utils.py:1248`, `app/escala/utils.py:1260`, `app/escala/utils.py:1263`, `app/escala/utils.py:1268`, `app/escala/utils.py:1794`).

O overlay escreve **somente os dicionários da grade**: `tipo_dia='folga'`, `celulas=[None]…`, `orfas=[]` (`app/escala/utils.py:1124`). O cartório consome a grade e leva esses campos para `ata_do_dia` (`app/ponto/services/cartorio.py:946`, `app/ponto/services/cartorio.py:579`). Também chegam aos leitores de espelho, PDF e simulação (`app/ponto/services/espelho.py:496`, `app/relatorios/pdf_espelho.py:193`, `app/ponto/services/simular_dia.py:7`).

O calendário agora deriva `status` e `eh_trabalho` dos mesmos fatos: o helper só troca a previsão por `False` quando vence feriado ou folga humana (`app/colaboradores/services/calendario.py:102`); o dicionário é montado, classificado e publicado nas linhas respectivas (`app/colaboradores/services/calendario.py:818`, `app/colaboradores/services/calendario.py:839`, `app/colaboradores/services/calendario.py:903`). `eh_trabalho` tem consumidor funcional no oferecimento de resolver o dia (`app/colaboradores/services/calendario.py:202`); `status` alimenta resumo e desenho da tela (`app/colaboradores/services/calendario.py:954`, `app/templates/colaboradores/partials/_calendario_grade.html:69`).

Donos de escrita fora do diff, sem pretensão de listar cada chamador do repositório:

| Dado | Escritor/porta e leitores relevantes |
|---|---|
| `FolgaDia.data/calendario_origem/escala_colaborador/tipo` | Calendário materializa preservando manuais; lançamento cria manual; alteração remove/troca tipo; reparentamento troca vínculo (`app/escala/folgas.py:165`, `app/escala/folgas.py:254`, `app/escala/folgas.py:274`, `app/escala/folgas.py:435`). Leitor novo é o carregador único (`app/ponto/precedencia.py:206`); gerador e cadastro ainda leem foto por vínculo (`app/escala/models.py:1257`, `app/escala/models.py:1281`, `app/escala/views_plano_folgas.py:60`). |
| `EscalaColaborador.tipo_escala/ativa/data_inicio/data_fim/data_ancora_colaborador` | Portas de vínculo criam/atualizam, encerram e corrigem trechos (`app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:278`, `app/colaboradores/services/vinculo.py:797`, `app/colaboradores/services/vinculo.py:832`, `app/colaboradores/services/vinculo.py:842`). Juíza e carregadores leem vigência/atividade (`app/escala/alimentacao.py:116`, `app/escala/alimentacao.py:182`, `app/escala/alimentacao.py:237`). |
| `CelulaDia.trabalha/escala_geradora/dna` | Mesmo módulo de porta gera/regenera, gera lote e reverte (`app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:592`, `app/ponto/portas/celula.py:530`). Precedência lê célula/vínculo (`app/ponto/precedencia.py:348`); grades carregam pela alimentação (`app/escala/utils.py:1260`). |
| `Ausencia.tipo/data/data_fim/status` | Porta monta e salva, edita e muda estado (`app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:417`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:808`). Precedência e comando leem (`app/ponto/precedencia.py:273`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`). |
| `Empresa.ativa/dia_inicio_competencia` | Formulário alimenta portas de estrutura, que criam/atualizam (`app/colaboradores/views.py:496`, `app/colaboradores/views.py:521`, `app/colaboradores/services/estrutura.py:12`). Corte efetivo é lido pelo juiz das janelas (`app/ponto/janelas.py:42`, `app/ponto/janelas.py:60`). |
| `REGRA_VERSOES`, impressão, ata | Declaração v3 (`app/ponto/services/cartorio.py:111`); `versao_para` escolhe maior horizonte aplicável (`app/ponto/services/cartorio.py:209`); impressão codifica versão (`app/ponto/services/cartorio.py:367`); lavra/carimbo persistem pela porta (`app/ponto/portas/celula.py:767`, `app/ponto/portas/celula.py:800`). |
| Aceite/recusa de folga | `_folga_do_dia` alimenta consulta e mensagem (`app/ponto/services/declarar_dia.py:95`, `app/ponto/services/declarar_dia.py:112`); validação de respostas usa o booleano para idempotência (`app/chamados/services/validacao.py:261`, `app/chamados/services/validacao.py:316`). Veto também é consumido por lançamento de folgas, criação de ausência e férias (`app/escala/folgas.py:258`, `app/ponto/services/ausencia.py:380`, `app/ferias/models.py:428`). |

**Exportada:** existe escrita possível na célula/ata da competência 09 com `--forcar`: o command alcança todas as células passadas e repassa a flag (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:65`); a flag vence o pulo por impressão (`app/ponto/services/cartorio.py:1173`). A alteração pode produzir `EventoCelula` e efeitos em chamados/perguntas/disputas (`app/ponto/portas/celula.py:622`, `app/ponto/portas/celula.py:634`, `app/chamados/reconciliador.py:1304`). Esse fluxo do cartório não chama lavratura de `DiaPago` ou fechamento; o consumidor mostrado retrata chamados/perguntas/disputas e marca eventos (`app/chamados/reconciliador.py:1304`, `app/chamados/reconciliador.py:1315`). O teste compara fechamento inteiro e todas as exportações antes/depois, incluindo conteúdo TXT; isso é prova programada de fixture, não hash de produção (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:106`). Hash real do TXT: [não verificado].

Classes reprovadoras:

- **Segundo juiz/derivação paralela:** os sítios alterados delegam ao juiz registrado, cuja criação tem corte literal (`app/core/juizes.py:192`, `app/docs/CORTES.md:416`). Subsistem as filas: gerador usa foto do próprio vínculo (`app/escala/models.py:1281`); C5 monta universo próprio sem distinguir origem de calendário (`app/chamados/management/commands/auditar_invariantes_chamados.py:1532`).
- **Segundo escritor:** as mudanças da grade/calendário são em memória; persistência indireta continua na porta da célula (`app/escala/utils.py:1124`, `app/colaboradores/services/calendario.py:898`, `app/ponto/portas/celula.py:634`).
- **Relógio/ordem de trava:** não há relógio ou lock novo nos trechos de julgamento/overlay alterados (`app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:212`, `app/ponto/precedencia.py:222`, `app/escala/utils.py:1114`). O comando lê relógio na borda (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:52`).
- **Pouso antes do pré-requisito:** a própria declaração v3 exige ensaio de frota com `--apply` antes do pouso (`app/ponto/services/cartorio.py:174`). Cumprimento/remedição com feed novo: [não verificado].
- **Motor/escala/vínculo:** o diff integral fornecido não contém alteração em `motor_calculo_v2.py`, modelo/porta de escala ou porta de vínculo. [não verificado — evidência negativa é o anexo do usuário, sem arquivo local numerado].

## 31/10 23:54 · sessao 526f3e56522d · - · - · origem=Ronald (tmux)

`medir` foi **parcialmente curado**: o feed das sementes usa o juiz correto; o universo dos dias contados e o novo rótulo continuam divergindo. (`app/escala/services/fase_conflitante.py:91`, `:99`, `:105`, `:123`)

- Universo externo: somente vínculos `ativa=True`; o dono inativo de uma célula pode ser escolhido por `vinculo_do_dia`, mas nunca ganha sua própria linha na métrica. (`app/escala/services/fase_conflitante.py:79`; `app/escala/alimentacao.py:247`)
- As sementes são FolgaDia da **pessoa**, recortadas pela vigência do vínculo ativo iterado; somente entram se o vínculo do dia for esse ativo e o juiz aceitar a candidata. Carrega inclusive geradoras fora da query de vigência mediante `mais=`. (`app/escala/services/fase_conflitante.py:91`, `:95`, `:97`, `:100`; `app/ponto/precedencia.py:206`; `app/escala/alimentacao.py:123`)
- Sobreposição: geradora válida vence; sem célula/geradora, vence vínculo que cobre o dia, priorizando ativo e início mais recente. (`app/escala/alimentacao.py:237`, `:247`, `:250`, `:253`)
- Geradora fora da vigência: juiz devolve `None`, sem fallback; a candidata não entra nas sementes. (`app/escala/alimentacao.py:250`; `app/escala/services/fase_conflitante.py:100`)
- Dono inativo cuja vigência cobre o dia: juiz devolve esse dono; candidata desse dia não entra nas sementes do ativo. (`app/escala/alimentacao.py:247`, `:250`, `:252`; `app/escala/services/fase_conflitante.py:100`)
- Manual presa ao velho: predicado aceita independentemente do vínculo da linha; entra nas sementes do ativo se ele for vínculo daquele dia. Se o dia for do inativo ou não houver vínculo, `medir` descarta a semente apesar de o juiz da folga aceitar a manual. (`app/ponto/precedencia.py:237`; `app/escala/services/fase_conflitante.py:100`)
- Sem vínculo: não entra semente; **isso não impede o dia de ser contado depois**. (`app/escala/services/fase_conflitante.py:100`, `:105`)

**Contraexemplo executado em memória, com AST real e ORM substituído, sem banco/arquivo:** no próprio desenho do teste parcial — janela 10–20/10, velho gerando 10–14, novo gerando 15–20, calendário do novo nos pares, âncora 20/07 — a métrica aceita sementes somente 16/18/20 e retorna **11 dias**, primeiro 10/10, último 20/10. Conta cinco dias do velho. Variantes com os primeiros cinco dias sem vínculo ou com geradora fora da vigência também retornam 11. Causa: `_foto_diz` extrapola paridade da semente mais próxima e o laço contado não pergunta vínculo do dia nem vigência. (`app/escala/services/fase_conflitante.py:61`, `:63`, `:105`, `:107`, `:117`)

O teste parcial novo passa porque verifica apenas `vinculos == 1` e o pk da amostra; não verifica total nem quais dias foram contados. O caso todos os dias gerados pelo velho retorna zero e é efetivamente coberto. (`app/ponto/tests/test_grade_folga_juiz_sitios.py:222`, `:225`, `:228`, `:231`)

**Rótulo e `o_que_e`:** dizem “folga lançada do dia … nos dias deste vínculo” e “FOTO (`folga_lancada_do_dia` com vínculo do dia)”; a comparação real é **fase extrapolada** de sementes filtradas versus âncora, em toda a janela. Também compara dias sem lançamento, meses sem foto e dias fora da vigência. A extrapolação já era intencional no contador anterior; o defeito é apresentar essa fase como resposta diária do juiz e prometer recorte que o laço não aplica. (`app/escala/services/fase_conflitante.py:57`, `:63`, `:91`, `:105`, `:123`, `:129`; `app/escala/tests/test_fase_conflitante.py:57`, `:67`)

A fila GERADOR está corretamente explicitada: o gerador ainda lê foto por vínculo e usa o teste mensal; a métrica mudou para sementes da pessoa aceitas pelo juiz, sem teste mensal. (`app/escala/services/fase_conflitante.py:22`; `app/escala/models.py:1252`, `:1257`, `:1260`; `app/docs/JUIZES.md:87`)

**Snapshot e leitores:**

- `lavrar_fase_conflitante` chama `medir`; com `--apply` grava `MetricaSnapshot`, numerador = total, denominador = vínculos, `detalhe` = resultado integral; sem flag só imprime. Cron declarado às 07:14 com `--apply`. (`app/escala/management/commands/lavrar_fase_conflitante.py:30`, `:32`, `:35`; `app/config/crons.py:479`)
- `ultimo_lavrado` escolhe o último snapshot por data/calculado_em e devolve **detalhe gravado**, acrescido de `medido_em`; sem snapshot devolve `sem_medida`, total `None`. Não recompõe rótulo de snapshot antigo. (`app/escala/services/fase_conflitante.py:137`, `:140`, `:141`)
- API `/api/mensageria/saude/` expõe-o em `contadores.fase_conflitante`. (`app/api/urls.py:16`; `app/api/views_mensageria.py:632`, `:643`, `:647`)
- A ponte `core_client.saude` recebe esse endpoint, mas `saude_da_operacao` percorre somente `dados['metricas']` e retorna apenas medidas; **descarta `contadores`**, portanto o caminho de contexto do chat não mostra este contador. (`mensageria/nucleo/core_client.py:108`, `:115`; `mensageria/nucleo/ferramentas.py:259`, `:265`, `:294`, `:380`)
- Remedição de produção/sombra, inclusive substituir o antigo 181/4, e conteúdo real do snapshot atual: **[não verificado]**.

Sugestão de classificação: bloqueio do item (b) do opus **parcialmente curado**; o feed e a explicitação GERADOR foram curados, mas há contraexemplo de universo e rótulo. Remedição exigida pelo laudo permanece **[não verificado]**. (`app/escala/services/fase_conflitante.py:99`, `:105`, `:123`; `opus_final.txt`, anexo dado pelo usuário, item (b) **[não verificado: arquivo não localizado na cópia]**)

## 31/10 23:54 · sessao b468b72a8c67 · - · - · origem=Ronald (tmux)

A catraca cura o bloqueio literal do opus: agora reconhece o `getattr` usado pelo predicado, testa a cópia literal dele e do carregador, inclui `precedencia.py` na varredura e poupa apenas as três funções nomeadas no nível superior. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:53`, `:109`, `:198`, `:209`; `app/ponto/precedencia.py:237`)

Executei em memória os dez testes do arquivo, usando `unittest.TestCase` no lugar da base Django e extraindo somente as três funções do juiz por AST; todos passaram, inclusive a comparação da varredura real com a allowlist. Isso não constitui execução da suíte Django. [não verificado] [nao verificado]

| Caso solicitado | Resultado das sondas e fundamento |
|---|---|
| `from escala.models import FolgaDia as FD` | Morde; recolhe `ImportFrom` e `asname`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:74`, `:188`) |
| Import local dentro de função | Morde; coleta imports com `ast.walk(arvore)` antes de examinar funções. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:74`) |
| `import escala.models as m; m.FolgaDia...` | Escapa; a raiz é `m`, que não entra em `apelidos`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:75`, `:92`) |
| `getattr(fd, nome_em_variavel)` | Escapa; exige `ast.Constant` no segundo argumento. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:55`) |
| `hasattr(fd, 'calendario_origem_id')` | Escapa; o único nome de chamada reconhecido para leitura indireta é `getattr`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:55`, `:61`) |
| `operator.attrgetter('calendario_origem_id')(fd)` | Escapa; não é atributo do campo, subscrito ou `getattr` literal. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:61`) |
| `values`/`values_list` com string literal | Morde origem; também morde filtro se houver filtro por pessoa e coluna `data`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:43`, `:95`) |
| `values(*campos)` com nomes em variável | Escapa; recolhe somente argumentos posicionais que sejam strings constantes. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:44`) |
| Função em `__init__.py` | Morde; nenhum tratamento especial exclui esse arquivo. Código no nível de módulo continua fora. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:106`, `:124`) |
| Método `__init__` | Morde como `Classe.__init__`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:108`, `:112`) |
| Import relativo `from .models import FolgaDia as FD` | Morde; não testa módulo nem nível de relatividade do `ImportFrom`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:75`) |
| Função aninhada dentro de uma das três funções `JUIZ` | Escapa; o `continue` pula a função inteira e suas descendentes. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:109`) |
| Método de classe em `precedencia.py` com nome igual ao juiz | Morde; a exceção exige `pref == ''`, e métodos recebem prefixo da classe. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:109`, `:113`) |

Os sete testes `MORDE` da catraca acusam efetivamente os exemplos anunciados: filtro pessoa/data, alias e manager relacionado, comparação da origem, cópia literal do predicado, cópia literal do carregador, leitura sem comparação e segundo juiz adicional em `precedencia.py`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:183`, `:188`, `:194`, `:198`, `:209`, `:217`, `:223`)

Há duas limitações adicionais:

- A cópia literal apenas de `folga_lancada_do_dia`, chamando o predicado registrado, escapa: seu corpo não contém filtro nem leitura da origem. (`app/ponto/precedencia.py:216`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:85`)
- “Só encolhe” compara achados e allowlist do estado atual; não impede adicionar código e entrada juntos. Como o resultado é um conjunto por função/tipo, uma segunda derivação do mesmo tipo dentro de função já permitida também não cria chave nova. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:88`, `:104`, `:169`)

Para os `MORDE` dos demais arquivos, a inspeção estática mostra as seguintes distinções; execução contra banco e RED dos mutantes permanecem [não verificado]:

| Teste | Mutante distinguido pelas asserções | [nao verificado]
|---|---|
| Calendário, folga manual do velho | Restaurar foto exclusiva do ativo; também restaurar `eh_trabalho` cru da célula. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:56`) |
| Calendário, calendário do ativo em célula do velho | Restaurar `{data: True}` do ativo. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:102`) |
| Grade, calendário alheio | Overlay indiscriminado no builder por turno; o próprio arquivo declara que o legacy já passava esse lado. (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:21`, `:70`) |
| Grade, candidata alheia de menor PK | Guardar somente a primeira candidata; a asserção da precedência distingue esse defeito mesmo se a grade continuar acertando. (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`) |
| Grade, manual fora da janela | Carregar apenas vínculos que cruzam a janela. (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:91`) |
| Grade, 09/08/simular | Restaurar portão que devolve a ata antiga abaixo do horizonte; ambas as flags são cobertas. (`app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:74`, `:79`, `:84`, `:98`, `:103`) |
| Cartório, 09 com `--forcar` | Impedir rejulgamento abaixo do horizonte. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:101`) |
| Cartório, 09 sem `--forcar` | Rejulgar automaticamente a 09. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:121`) |
| Versão viva | Tupla incorreta ou leitura que não devolve 09=1, 10=3 e 08=None. (`app/ponto/tests/test_o219_versao_da_regra.py:76`, `:78`) |

O teste de exportação compara linha integral, conteúdo do TXT e conjunto integral de exportações antes/depois da fixture. O campo `hash_sha256` da fixture é `'a' * 64`, portanto esse teste demonstra preservação do valor armazenado, não valida que esse valor seja o SHA-256 do conteúdo. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:68`, `:79`, `:106`)

Minha classificação: o bloqueio **(a) do opus está curado no escopo exato reclamado**; as outras formas acima continuam lacunas da catraca. C5 e gerador já são filas expressamente declaradas, sem migração realizada por este selo. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:17`, `:153`)

## 31/10 23:52 · sessao c9c21cb2b2af · CONTENCAO-QUE-MORDE · leitura · origem=Code

**O contrato pressupõe mecanismos que a cópia ainda não oferece:** não há fiscalização geral de contadores por duas gerações; B1 exige consumo de tokens; verificador final sem obra reconhecida não é negado por esse motivo. (`bin/hook_stop_fila1.py:1034`, `bin/disjuntor.py:452`, `bin/disjuntor.py:1457`)

**(1) Composição, “esperado”, geração e leitores/escritores**

`gerar_estado.py` reúne resultados de medidores e escreve **Markdown**, sem esquema unificado `{contador, valor, esperado, geracao}`. `sem_agente` e interrompidas recebem “esperado 0” na prosa; fio morto tem mapa `esperado` estruturado no carimbo; leitores do dia têm `esperado_no_fim`. Portanto, não é tudo prosa, mas também não existe campo estruturado uniforme por contador. (`bin/gerar_estado.py:222`, `bin/gerar_estado.py:240`, `bin/fio_estado.py:123`, `app/core/medida_leitor_do_dia.py:409`)

Existe indicação de regeneração: o gerador obtém data/hora até segundos e imprime **“Medido em”**. Não existe número monotônico nem protocolo de gerações consecutivas; usar esse texto como identidade exigiria definir repetição, ausência, leitura inválida e regenerações no mesmo segundo. (`bin/gerar_estado.py:294`, `bin/gerar_estado.py:300`, `bin/gerar_estado.py:376`)

| Artefato/medida | Quem escreve ou produz | Quem lê |
|---|---|---|
| ESTADO | `relato.sh` chama o gerador para um temporário; `gerar` escreve `dst`; publicação vai para outro repositório por padrão | Destino padrão `/home/ronald/hasner-relato`; o Stop atual consulta B4/B2/recados, sem ler esse ESTADO. (`bin/relato.sh:23`, `bin/relato.sh:38`, `bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1034`) |
| Raias | Declarações por `raia_agente.sh`, combinadas com Git pelo medidor | `raias_estado` → gerador. (`bin/raia_agente.sh:49`, `bin/raias_estado.py:91`, `bin/gerar_estado.py:215`) |
| Interrompidas | Registro na célula de estado do BACKLOG | `interrompidas_estado` → gerador. (`bin/interrompidas_estado.py:89`, `bin/interrompidas_estado.py:105`, `bin/gerar_estado.py:236`) |
| Fio morto | `vigiar_fio_morto` grava carimbo com censo e esperado | `fio_estado` compara os dois → gerador. (`app/chamados/management/commands/vigiar_fio_morto.py:114`, `bin/fio_estado.py:125`, `bin/gerar_estado.py:260`) |
| Leitores do dia | Censo AST calculado na árvore | `leitor_dia_estado` → gerador. (`app/core/medida_leitor_do_dia.py:350`, `bin/leitor_dia_estado.py:35`, `bin/gerar_estado.py:290`) |
| Disjuntores | `dispara` escreve trilha; `_com_estado` grava estado JSON sob trava | `ler_estado`, decisões dos disjuntores e painel do gerador. (`bin/disjuntor.py:139`, `bin/disjuntor.py:237`, `bin/disjuntor.py:291`, `bin/gerar_estado.py:280`) |
| Cota | `statusline_cota.py` | `disjuntor.ler_cota`. (`bin/statusline_cota.py:51`, `bin/disjuntor.py:1064`) |

**(2) Contadores com esperado declarado e valores disponíveis**

Não há nesta cópia um ESTADO publicado nem os carimbos de execução necessários para afirmar valores atuais de produção. **Valores atuais desses artefatos: [não verificado].** O `logs/` é excluído do versionamento. (`.gitignore:53`) [nao verificado]

| Contador publicado pelo caminho do ESTADO | Esperado declarado | Valor atual verificável | [nao verificado]
|---|---|---|
| `sem_agente` | 0. (`bin/gerar_estado.py:222`) | [não verificado] |
| `interrompidas_mais_de_24h` | 0. (`bin/gerar_estado.py:240`) | [não verificado] |
| `fio_morto`, `fio_morto_cobrando`, `fio_morto_com_alvo`, `dias_futuros_no_universo` | Todos 0. (`app/chamados/management/commands/vigiar_fio_morto.py:87`) | [não verificado] |
| `respondida_sem_resposta`, `materializada_viva`, `fio_morto_alcance` | Todos 0. (`app/chamados/management/commands/vigiar_fio_morto.py:95`) | [não verificado] |
| Disparos B1, B2, B3, B4, B4-CODEX e ANEXO-ENXUTO em 24 h | 0, declarado no texto do painel. (`bin/disjuntor.py:1678`, `bin/disjuntor.py:1691`) | [não verificado] |
| Negações sem linha no RELATO | 0. (`bin/disjuntor.py:1683`) | [não verificado] |
| Verificador final sem obra reconhecida | 0. (`bin/disjuntor.py:1692`) | [não verificado] |
| Leitores com derivação própria do dia | **0 no fim do programa**, não uma declaração de que deveriam estar zerados agora. (`app/core/medida_leitor_do_dia.py:409`) | Reconto somente de leitura da cópia: **5 = 5 status + 0 valor**, correspondentes às cinco funções declaradas; cálculo pelo censo. (`app/core/medida_leitor_do_dia.py:319`, `app/core/medida_leitor_do_dia.py:390`, `app/core/medida_leitor_do_dia.py:394`) |

Há números **históricos gravados**, que não provam o “hoje” do contrato: raias `sem_agente=7` em 11/10; interrompidas acima de 24 h `=0` em 18/10; fio antigo `=4` e alcance `=388` na construção F5 de 30/10; disparos em 24 h `=0` às 08:3x de 31/10. (`app/docs/BACKLOG.md:1366`, `app/docs/BACKLOG.md:1483`, `app/docs/RELATO.md:249`, `app/docs/RELATO.md:166`)

Nem toda medida do ESTADO tem esperado zero: recusas explicitamente **não** têm; os placares estruturais possuem `numero`/`meta`, inclusive textuais. Logo, “nenhuma allowlist” ainda exige definir como descobrir quais medidas têm expectativa comparável. (`bin/recusa_estado.py:11`, `bin/gerar_estado.py:97`)

**(3) O que B1 chama de agente ocioso**

B1 lê o JSONL do subagente e seu `.meta.json`, derivados do caminho do transcript principal. Isenta tipos de somente leitura, incluindo `verificador` e `Explore`. (`bin/disjuntor.py:335`, `bin/disjuntor.py:353`, `bin/disjuntor.py:418`, `bin/disjuntor.py:91`)

A condição atual exige:

- Vida registrada de pelo menos **15 minutos** e tokens positivos na janela `agora − 15 min`. Soma `input_tokens`, `output_tokens` e `cache_creation_input_tokens`, deduplicando mensagens; não soma cache lido. (`bin/disjuntor.py:428`, `bin/disjuntor.py:443`, `bin/disjuntor.py:449`, `bin/disjuntor.py:450`)
- Diretório existente cujo caminho absoluto terminado em `wt-*` apareceu no transcript. Isso identifica uma cópia citada, sem comprovar propriedade. (`bin/disjuntor.py:93`, `bin/disjuntor.py:425`)
- Nenhum arquivo regular com **mtime recente** nas cópias reconhecidas, excluídos `.git` e caches. “Novo” inclui arquivo antigo editado ou tocado, sem verificar autoria ou conteúdo. (`bin/disjuntor.py:115`, `bin/disjuntor.py:387`, `bin/disjuntor.py:405`, `bin/disjuntor.py:454`)

**B1 não examina jobs/processos próprios em fundo. Sem tokens recentes, não dispara.** Também não possui timer autônomo: a medida ocorre no PreToolUse do agente, no máximo uma vez por minuto; DIFF/apply suspende B1 e `SubagentHandback` é permitido. O Stop apenas anuncia agentes cujo B1 já disparou. (`bin/disjuntor.py:413`, `bin/disjuntor.py:452`, `bin/disjuntor.py:478`, `bin/disjuntor.py:1434`, `bin/hook_stop_fila1.py:1053`)

**(4) “Verificador sem obra lida”**

A medida reconhece **rótulo de obra no prompt**, não comprova leitura efetiva de arquivos da obra. `_veredito_e_obra` examina a primeira mensagem humana do transcript; a varredura coleta finais sem obra reconhecida e acumula IDs únicos em `b3_sem_obra`. O painel publica o comprimento dessa lista com esperado 0. (`bin/disjuntor.py:504`, `bin/disjuntor.py:546`, `bin/disjuntor.py:736`, `bin/disjuntor.py:892`, `bin/disjuntor.py:1624`, `bin/disjuntor.py:1692`)

Esse contador é **acumulado**, sem janela de 24 h nesse cálculo. (`bin/disjuntor.py:892`, `bin/disjuntor.py:1626`)

No PreToolUse atual, final sem obra reconhecida faz `nb3f = None`: pula a decisão B3 e segue para B4. Pode ser negado por cota/escada ou outra guarda aplicável, mas **não pela ausência de obra em si**. Exceção no ramo B3 é caso separado: gera negativa por “não sei”. (`bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1496`)

**(5) `statusline_cota.py` e ausência de `cota.json`**

Grava somente quando chegam **os dois** campos semanais como números finitos, excluindo booleanos: `rate_limits.seven_day.used_percentage` e `rate_limits.seven_day.resets_at`. Ao gravar, não exige reset futuro nem valida faixa percentual. (`bin/statusline_cota.py:26`, `bin/statusline_cota.py:40`, `bin/statusline_cota.py:45`, `bin/statusline_cota.py:47`)

Também lê `context_window.total_input_tokens`, `context_window.used_percentage` e `session_id`. Grava esses dados como `contexto_tokens`, `contexto_pct`, `session_id`, junto de `seven_day_pct`, `seven_day_resets_at` e `hora` gerada, usando temporário e substituição atômica. Sem medida válida preserva o arquivo anterior; falha de escrita é silenciada. (`bin/statusline_cota.py:43`, `bin/statusline_cota.py:49`, `bin/statusline_cota.py:51`, `bin/statusline_cota.py:54`, `bin/statusline_cota.py:55`)

`ler_cota` decide apenas por percentual e reset: arquivo ausente/ilegível, medida inválida ou reset vencido resultam em `None`. Hora e sessão não determinam validade. (`bin/disjuntor.py:1069`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1075`)

Com `None`, B4 não bloqueia por desconhecimento da cota. Sem escada humana, o Stop remove `b4_para` e segue seu fluxo normal; **não conta Stops consecutivos de cota desconhecida**. O painel mostra “não sei”. (`bin/disjuntor.py:1126`, `bin/disjuntor.py:1204`, `bin/disjuntor.py:1208`, `bin/disjuntor.py:1690`)

**(6) Veredito por linha do contrato e limites omitidos**

| Linha | Confronto com a cópia |
|---|---|
| **1 — QUEM CHAMA** | Correta como proposta de integração, incompleta como descrição do existente: Stop não lê contadores gerais do ESTADO; há expectativas heterogêneas e falta implementar a história de gerações. `_com_estado` já é a porta de gravação do estado próprio. (`bin/hook_stop_fila1.py:1034`, `bin/gerar_estado.py:222`, `bin/fio_estado.py:123`, `bin/disjuntor.py:237`) |
| **2 — QUEM ESPERA** | Os novos bloqueios não existem: sem tokens B1 permite; final sem obra pula B3; cota desconhecida permite pelo B4. “Escada N” reconhece somente a palavra inicial, ignora N e não grava medida manual com hora; a condição dura até outra mensagem humana. (`bin/disjuntor.py:452`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:1127`, `bin/disjuntor.py:87`, `bin/disjuntor.py:1079`, `bin/disjuntor.py:1114`) |
| **3 — FALHA/LIMITE** | Desligar hoje não exige linha no RELATO: basta existência da chave, inclusive link. “Nunca verde calado” não é garantia transversal: leitura falha do BACKLOG vira texto vazio no medidor de interrompidas; falhas gerais do Stop liberam. “≥4 fora hoje” **[não verificado]**. (`bin/disjuntor.py:144`, `bin/hook_stop_fila1.py:1039`, `bin/interrompidas_estado.py:91`, `bin/hook_stop_fila1.py:1305`) |
| **4 — MEDIDA** | Os testes existentes não demonstram os quatro novos comportamentos: o caso sem obra verifica contagem posterior; B1 usa consumo recente; o outro arquivo testa portas PAREI. Mordida dos quatro mutantes novos e estado das raias antigas: **[não verificado]**. (`bin/tests/test_disjuntor.sh:227`, `bin/tests/test_disjuntor.sh:427`, `bin/tests/test_hook_parei_so_lei.sh:40`) |
| **5 — QUEM DESLIGA** | Há guarda textual contra ferramentas que nomeiem a chave, inclusive quando desligado; isso não autentica a identidade de Ronald nem exige registro no RELATO. A política humana é mais ampla que a proteção implementada. (`bin/disjuntor.py:152`, `bin/disjuntor.py:160`, `bin/disjuntor.py:1486`) |

O limite central omitido é **atribuição e observabilidade**: mtime não prova trabalho do agente; ausência de ferramenta não provoca medição B1; “obra lida” significa identificação textual; e ausência de `cota.json` não prova por que o payload faltou. O RELATO registra a ausência, mas o escritor silencia erros e não conserva o payload recebido: a **causa real continua [não verificado]**. (`bin/disjuntor.py:405`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:504`, `app/docs/RELATO.md:5597`, `bin/statusline_cota.py:35`, `bin/statusline_cota.py:55`)

## 31/10 23:51 · sessao a24b0a52d011 · - · - · origem=Ronald (tmux)

`gerar_estado.py` compõe **Markdown**, com medidores separados; não há objeto unificado `{contador, medido, esperado, geracao}`. As metas `sem_agente=0` e `interrompidas_mais_de_24h=0` são texto interpolado; fio morto e leitores do dia possuem expectativas estruturadas em suas próprias fontes (`bin/gerar_estado.py:222`, `bin/gerar_estado.py:240`, `bin/fio_estado.py:123`, `app/core/medida_leitor_do_dia.py:409`).

Há **carimbo textual de regeneração**: executa `date '+%d/%m/%Y %H:%M:%S'` e publica `Medido em …`; não há número monotônico ou identidade robusta de geração. Assim, dizer que não existe nenhuma indicação de regeneração seria incorreto; dizer que já existe protocolo de “duas gerações consecutivas” também seria incorreto (`bin/gerar_estado.py:294`, `bin/gerar_estado.py:300`, `bin/gerar_estado.py:376`).

Escritores/leitores:

- `bin/relato.sh` chama o gerador para `$TMP/ESTADO.md`; o gerador grava `dst`; o relato raspa/publica em `$DEST/ESTADO.md`, cujo destino padrão é outro repositório, `/home/ronald/hasner-relato` (`bin/relato.sh:23`, `bin/relato.sh:38`, `bin/gerar_estado.py:376`, `bin/relato.sh:103`).
- A programação declarada do timer é 5 minutos, sem garantia de execução efetiva nesta cópia (`bin/hasner-relato.timer:6`; execução efetiva [não verificado]).
- Raias: `raia_agente.sh` escreve `logs/raias/<nome>.agente`; `raias_estado.ler()` combina declarações e Git; o gerador consome o resultado (`bin/raia_agente.sh:49`, `bin/raia_agente.sh:54`, `bin/raias_estado.py:91`, `bin/raias_estado.py:101`, `bin/gerar_estado.py:215`).
- Interrompidas: o medidor lê a célula de estado do BACKLOG e o juiz do topo; o gerador publica a composição (`bin/interrompidas_estado.py:89`, `bin/interrompidas_estado.py:93`, `bin/interrompidas_estado.py:105`, `bin/gerar_estado.py:236`).
- Fio: `vigiar_fio_morto` grava atomicamente o carimbo com `esperado`; `fio_estado` lê/compara; o gerador chama esse leitor (`app/chamados/management/commands/vigiar_fio_morto.py:114`, `app/chamados/management/commands/vigiar_fio_morto.py:131`, `app/chamados/management/commands/vigiar_fio_morto.py:211`, `bin/fio_estado.py:104`, `bin/fio_estado.py:125`, `bin/gerar_estado.py:260`).
- Leitores do dia: censo AST no ato, retornando `numero`, `itens`, `esperado_no_fim`; o adaptador compõe Markdown, consumido pelo gerador (`app/core/medida_leitor_do_dia.py:390`, `app/core/medida_leitor_do_dia.py:393`, `app/core/medida_leitor_do_dia.py:409`, `bin/leitor_dia_estado.py:35`, `bin/leitor_dia_estado.py:46`, `bin/gerar_estado.py:290`).
- Recusas: envelope de resposta escreve a trilha `ponto.log`; medidor usa o mesmo leitor da LINHA HAIKU; não é esperado zero (`bin/recusa_estado.py:5`, `bin/recusa_estado.py:11`).
- Codex limite: escritor declarado é `codex_pergunta.sh`, leitor/compositor é `codex_limite_estado`, consumido pelo gerador (`bin/gerar_estado.py:267`, `bin/codex_limite_estado.py:247`).
- Orçamento: o gerador documenta Stop como escritor da trilha/estado e `marco_de_clear.py` como escritor do marco; consome `orcamento_contexto` (`bin/gerar_estado.py:271`).
- Disjuntores: o gerador consome `disjuntor.ler/linha`; a própria seção declara `disjuntor.dispara` escritor da trilha (`bin/gerar_estado.py:277`).

Censo do **esperado declarado** dentro das seções operacionais do ESTADO:

| Contador | Esperado | Valor corrente comprovável |
|---|---|---|
| `sem_agente` | 0 (`bin/gerar_estado.py:222`) | [não verificado] |
| `interrompidas_mais_de_24h` | 0 (`bin/gerar_estado.py:240`) | [não verificado] |
| `fio_morto`, `fio_morto_cobrando`, `fio_morto_com_alvo`, `dias_futuros_no_universo` | 0, dados do escritor (`app/chamados/management/commands/vigiar_fio_morto.py:87`) | [não verificado] |
| `respondida_sem_resposta`, `materializada_viva`, `fio_morto_alcance` | 0, dados do escritor (`app/chamados/management/commands/vigiar_fio_morto.py:95`) | [não verificado] |
| Disparos B1/B2/B3/B4/B4-CODEX/ANEXO-ENXUTO em 24h | 0, rótulo do compositor (`bin/disjuntor.py:1678`, `bin/disjuntor.py:1691`) | [não verificado] |
| Negações sem linha no RELATO | 0, rótulo do compositor (`bin/disjuntor.py:1683`) | [não verificado] |
| Verificador final sem obra lida | 0, rótulo do compositor (`bin/disjuntor.py:1692`) | [não verificado] |
| Leitores com derivação própria do dia | 0 **no fim do programa** (`app/core/medida_leitor_do_dia.py:409`) | Declarações atuais: **5 entradas**, o que não substitui o censo medido (`app/core/medida_leitor_do_dia.py:319`) |

`recusa_estado` explicitamente rejeita esperado zero; orçamento e limite Codex compõem medidas informativas sem campo `esperado`; os placares estruturais usam campos `meta` e `numero`, incluindo strings extensas, que não são automaticamente um esquema numérico comparável (`bin/recusa_estado.py:11`, `bin/orcamento_contexto.py:382`, `bin/codex_limite_estado.py:246`, `bin/gerar_estado.py:97`, `app/core/placar_estrutural.py:21`).

Os arquivos de execução não vieram na cópia inspecionada; `logs/` é ignorado. Não há dado suficiente para confirmar “>=4 fora hoje” (`.gitignore:53`; contagem atual [não verificado]).

Números **históricos gravados em documentos**, que não devem ser promovidos a atuais:

- Raias em **11/10 17:3x**: `sem_agente=7`, `velhas=6`, união 30 (`app/docs/BACKLOG.md:1366`).
- Interrompidas em **18/10 00:4x**: universo 2, abertas 1, maiores de 24h 0 (`app/docs/BACKLOG.md:1483`).
- Fio em **30/10 21:27**, ainda durante a construção F5: `fio_morto_alcance=388`, contador antigo `fio_morto=4` (`app/docs/RELATO.md:249`).
- Leitores em **31/10 00:0x**: 7; a fonte atual já declara somente 5 entradas (`app/docs/RELATO.md:227`, `app/core/medida_leitor_do_dia.py:319`).
- Disparos em **31/10 08:3x**: documento registra 0 e cota ainda sem arquivo; isso não comprova o estado posterior (`app/docs/RELATO.md:166`).

Limites adicionais encontrados: `interrompidas_estado.ler()` trata falha de leitura do BACKLOG como texto vazio, resultando em lista vazia/zeros; portanto “ESTADO ilegível nunca verde calado” não descreve uma garantia transversal dos medidores atuais (`bin/interrompidas_estado.py:88`, `bin/interrompidas_estado.py:92`, `bin/interrompidas_estado.py:136`). Fio tem tratamento melhor: ausência é não medido, erro de forma é ilegível, universo vazio é cego e carimbo velho não dá número na manchete (`bin/fio_estado.py:106`, `bin/fio_estado.py:115`, `bin/fio_estado.py:87`, `bin/fio_estado.py:140`).

## 31/10 23:51 · sessao d7d684624d82 · - · - · origem=Ronald (tmux)

1. **Stop atual não fiscaliza contadores gerais nem “duas gerações”.** A integração chama B4, B2 e recados de B3/B1; recados vêm do estado próprio do disjuntor e transcripts, sem parser de `ESTADO`, `esperado`, `FORA` ou geração nessa integração. (`bin/hook_stop_fila1.py:1034`, `bin/hook_stop_fila1.py:1043`, `bin/hook_stop_fila1.py:1048`, `bin/hook_stop_fila1.py:1053`, `bin/hook_stop_fila1.py:1206`, `bin/hook_stop_fila1.py:1294`, `bin/hook_stop_fila1.py:1299`)

2. **`statusline_cota.py` só grava quando os dois números semanais chegam válidos:** `rate_limits.seven_day.used_percentage` e `.resets_at`, ambos numéricos finitos, excluindo booleanos; não exige que reset esteja no futuro nem valida faixa percentual ao gravar. (`bin/statusline_cota.py:26`, `bin/statusline_cota.py:40`, `bin/statusline_cota.py:45`, `bin/statusline_cota.py:47`)

   Lê também `context_window.total_input_tokens`, `.used_percentage` e `session_id`; grava `seven_day_pct`, `seven_day_resets_at`, `hora` gerada, `contexto_tokens`, `contexto_pct`, `session_id`; usa temporário e `os.replace`. (`bin/statusline_cota.py:43`, `bin/statusline_cota.py:49`, `bin/statusline_cota.py:51`, `bin/statusline_cota.py:54`)

   Sem medida válida, preserva o arquivo anterior; erros na gravação são engolidos e a saída visual ainda pode apresentar a porcentagem recebida. (`bin/statusline_cota.py:47`, `bin/statusline_cota.py:55`, `bin/statusline_cota.py:61`)

3. **Arquivo de cota ausente, ilegível, não numérico, não finito ou vencido → `None`.** `ler_cota` usa apenas `seven_day_pct` e `seven_day_resets_at`; hora/sessão não participam da decisão. (`bin/disjuntor.py:1064`, `bin/disjuntor.py:1069`, `bin/disjuntor.py:1070`, `bin/disjuntor.py:1072`, `bin/disjuntor.py:1075`)

   `None` não gera contenção por cota: B4 permite novo Agent se não houver escada humana; no Stop, sem escada, remove `b4_para` e retorna `None`. Não há contagem de Stops com cota desconhecida nesse caminho. (`bin/disjuntor.py:1108`, `bin/disjuntor.py:1114`, `bin/disjuntor.py:1126`, `bin/disjuntor.py:1204`, `bin/disjuntor.py:1208`)

   O teste existente exige liberação com medida NaN e com reset vencido. (`bin/tests/test_disjuntor.sh:453`, `bin/tests/test_disjuntor.sh:454`, `bin/tests/test_disjuntor.sh:473`)

4. **Estado próprio:** `_com_estado` lê/decide/grava JSON sob flock e substituição atômica; ausente começa `{}`, ilegível levanta, falha de gravação registra FALHA e conserva a decisão já tomada; `ler_estado` lê sem essa mutação. (`bin/disjuntor.py:237`, `bin/disjuntor.py:249`, `bin/disjuntor.py:262`, `bin/disjuntor.py:268`, `bin/disjuntor.py:271`, `bin/disjuntor.py:281`, `bin/disjuntor.py:291`)

5. **Desligamento não exige linha no RELATO.** Basta `lexists` da chave, inclusive link; Stop retorna imediatamente e PreToolUse pula `pretool`, mantendo `guarda_chave`. O teste desliga inclusive com topo neutro. (`bin/disjuntor.py:144`, `bin/disjuntor.py:1486`, `bin/hook_stop_fila1.py:1039`, `bin/tests/test_disjuntor.sh:591`)

   A guarda é textual, negando ferramentas que nomeiem a chave e alterações reconhecidas de settings; isso implementa uma proteção de caminho, não uma autenticação de identidade humana. (`bin/disjuntor.py:152`, `bin/disjuntor.py:160`, `bin/disjuntor.py:164`, `bin/tests/test_disjuntor.sh:601`, `bin/tests/test_disjuntor.sh:605`)

6. **“Escada N” não é hoje fonte numérica manual com hora:** o parser reconhece somente a palavra inicial `escada`; ignora o N, não grava cota, e considera a última mensagem humana, encerrando a condição na mensagem humana seguinte. (`bin/disjuntor.py:87`, `bin/disjuntor.py:1080`, `bin/disjuntor.py:1102`, `bin/disjuntor.py:1114`)

7. **Medida/testes do contrato ainda não demonstrados:** `test_hook_parei_so_lei.sh` cobre decisão técnica, pergunta de lei, `!` e item de só registro; os quatro mutantes da nova contenção não aparecem nesses casos. (`bin/tests/test_hook_parei_so_lei.sh:40`, `bin/tests/test_hook_parei_so_lei.sh:48`, `bin/tests/test_hook_parei_so_lei.sh:59`, `bin/tests/test_hook_parei_so_lei.sh:65`)

   O teste existente de verificador sem obra comprova contagem posterior no painel, e seu mutante tira o contador; não é teste de recusa preventiva por ausência de obra. (`bin/tests/test_disjuntor.sh:427`, `bin/tests/test_disjuntor.sh:430`, `bin/tests/test_disjuntor.sh:1585`)

8. **Payload sem `rate_limits`: causa não demonstrada.** O RELATO registra ausência de `cota.json` e afirma que o payload nunca trouxe a seção; o script não salva payload recebido nem diagnostica a origem da ausência. A causa real do payload ausente permanece **[não verificado]**. (`app/docs/RELATO.md:5597`, `bin/statusline_cota.py:35`, `bin/statusline_cota.py:45`, `bin/statusline_cota.py:51`)

9. **Configuração gravada:** Stop, PreToolUse e statusline estão configurados para caminhos absolutos `/home/ronald/saas-hasner/...`; sua instalação/execução real nesta sessão não é demonstrada só por essa cópia. (`.claude/settings.json:9`, `.claude/settings.json:68`, `.claude/settings.json:76`; execução real **[não verificado]**)

## 31/10 23:50 · sessao 3a37361d563a · - · - · origem=Ronald (tmux)

B1:

- Fonte: `<transcript principal sem .jsonl>/subagents/agent-<id>.jsonl` e o `.meta.json` correspondente; exige `agent_id` e transcript existente. (`bin/disjuntor.py:335`, `bin/disjuntor.py:353`, `bin/disjuntor.py:464`)
- Isenta `verificador`, `Explore`, `Plan`, `claude-code-guide`, `statusline-setup`; meta ilegível significa “não sei” e não dispara. (`bin/disjuntor.py:90`, `bin/disjuntor.py:415`)
- Copia própria significa caminho absoluto terminado em `wt-*` citado em qualquer texto do transcript e que seja diretório existente; não há prova de propriedade desse caminho. (`bin/disjuntor.py:92`, `bin/disjuntor.py:425`)
- Janela: `agora - 15*60`; exige primeiro timestamp válido anterior/igual ao início da janela e tokens positivos em mensagens `assistant` recentes, deduplicadas por `message.id`/`uuid`; soma `input_tokens`, `output_tokens`, `cache_creation_input_tokens`, sem `cache_read_input_tokens`. (`bin/disjuntor.py:428`, `bin/disjuntor.py:441`, `bin/disjuntor.py:449`)
- **Sem tokens nos últimos 15 minutos não dispara**, contrariando contrato 2 “gastando tokens ou não”. (`bin/disjuntor.py:452`)
- “Arquivo novo” significa qualquer arquivo regular sem seguir symlinks com `mtime >= janela`, em qualquer cópia reconhecida; edição/touch de arquivo velho basta, sem autoria nem conteúdo/commit, excluindo `.git` e caches listados. (`bin/disjuntor.py:115`, `bin/disjuntor.py:387`, `bin/disjuntor.py:405`, `bin/disjuntor.py:454`)
- **Não mede job/processo próprio em fundo**: `medir_b1` lê apenas meta/transcript/cópias/mtime; a outra função `agentes_sem_entrega` reconhece lançamento e término do próprio subagente a partir de strings do transcript principal, não jobs internos dele. (`bin/disjuntor.py:366`, `bin/disjuntor.py:413`)
- B1 é reavaliado no PreToolUse no máximo uma vez/minuto; depois de disparado fica latched. Não há timer autônomo: somente entrada de ferramenta daquele agente provoca medida; `SubagentHandback` passa e `logs/sombra.lock` tomada/ilegível suspende B1. (`bin/disjuntor.py:473`, `bin/disjuntor.py:307`, `bin/disjuntor.py:1434`)
- Stop **não mede B1**: apenas cruza B1 já disparados com agentes sem notificação de término e pede `TaskStop`. (`bin/hook_stop_fila1.py:1053`)

Verificador sem obra:

- Reconhece “final” por palavras leitura/verificação/revisão/review com final, com exclusões de desenho; reconhece obra por âncoras `obra(s)` ou `leitura ... da`, token ID e validação de caixa/cauda. Isso mede rótulo no prompt, não evidência de arquivos efetivamente lidos. (`bin/disjuntor.py:94`, `bin/disjuntor.py:110`, `bin/disjuntor.py:500`, `bin/disjuntor.py:504`, `bin/disjuntor.py:518`)
- `_veredito_e_obra` examina primeira mensagem `user` do transcript; final sem obra reconhecida retorna `obra=''`, mesmo sem handback/veredito. (`bin/disjuntor.py:546`, `bin/disjuntor.py:551`, `bin/disjuntor.py:557`)
- `_reprovacoes_finais` varre apenas agentes cujo meta diz `agentType=verificador`; coleta IDs finais sem obra; `decide_b3` acumula IDs únicos em `b3_sem_obra` sob `_com_estado`. (`bin/disjuntor.py:736`, `bin/disjuntor.py:878`, `bin/disjuntor.py:892`, `bin/disjuntor.py:959`)
- `_b3_sem_obra` lê o comprimento da lista; ilegível é `None`; `ler()` expõe e `linha()` imprime “verificador final sem obra lida (esperado 0)”. **Não é janela de 24h**, embora esteja ao lado de medidas de 24h. (`bin/disjuntor.py:1620`, `bin/disjuntor.py:1624`, `bin/disjuntor.py:1691`)
- **Hoje PreToolUse não nega simplesmente por faltar obra**: `_nega_b3(ob,...) if ob else None`; segue B4, que pode negar por cota/escada, senão libera. Exceção no ramo B3 é caso distinto e nega por “não sei”, inclusive classificação `'*'` sem obra. (`bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1410`, `bin/disjuntor.py:1496`)
- Teste atual só comprova que final sem obra já executado entra no contador após Stop; não testa a negativa contratada na criação. (`bin/tests/test_disjuntor.sh:427`)
- Testes B1 usam explicitamente 510 tokens recentes; controles cobrem arquivo recente, somente leitura, vida curta e DIFF, sem controle de agente quieto com job próprio. (`bin/tests/test_disjuntor.sh:227`, `bin/tests/test_disjuntor.sh:243`, `bin/tests/test_disjuntor.sh:252`)
- `test_hook_parei_so_lei.sh` contém casos de decisão técnica, pergunta de lei, espera de `!` e só registro; não é evidência dos quatro mutantes novos do contrato. (`bin/tests/test_hook_parei_so_lei.sh:40`, `bin/tests/test_hook_parei_so_lei.sh:48`, `bin/tests/test_hook_parei_so_lei.sh:59`, `bin/tests/test_hook_parei_so_lei.sh:65`)

Limites adicionais:

- O desligamento existente testa só `lexists(DISJUNTOR_DESLIGADO)`; Stop e main PreToolUse pulam disjuntores sem exigir linha no RELATO. A guarda de chave ainda funciona desligado. (`bin/disjuntor.py:144`, `bin/disjuntor.py:1486`, `bin/hook_stop_fila1.py:1039`)
- Falha aberta geral permanece fora do ramo B3; B1 “não sei” nunca dispara, incompatível com supor proteção universal que nunca fica verde silencioso. (`bin/disjuntor.py:414`, `bin/disjuntor.py:1496`)
- `escada <N>` hoje só aciona escada por prefixo da última mensagem humana e dura até próxima humana; não grava percentual/hora em `cota.json`. (`bin/disjuntor.py:1079`, `bin/disjuntor.py:1108`)

## 31/10 23:48 · sessao 35d3d4711061 · DISJUNTOR-TETO-DA-OBRA · leitura · origem=Code

**A premissa central precisa de correção: o B3 não conta todas as voltas de construtor/verificador por obra. Ele mede reprovações finais de agentes e chamadas Codex triadas PROCEDENTE, com limiares separados.** (`bin/disjuntor.py:736`, `bin/disjuntor.py:857`, `bin/disjuntor.py:910`)

1. **Contagens, fontes, escritores e leitores — contrato 1**

   | Medida atual | Fonte e escritor | Quem conta/lê |
   |---|---|---|
   | Reprovações finais por obra | Transcripts `subagents/agent-*.jsonl`, metadado `agentType=verificador`, prompt reconhecido como final e `SubagentHandback` iniciado por `REPROVADO`. O código atribui os transcripts ao harness; a implementação desse escritor é **[não verificado]**. | `_reprovacoes_finais` coleta por `agent_id`; `decide_b3` acumula em `b3[obra].reprovados`. (`bin/disjuntor.py:335`, `bin/disjuntor.py:546`, `bin/disjuntor.py:736`, `bin/disjuntor.py:896`) |
   | Chamadas Codex verificador PROCEDENTES por obra | O wrapper escreve resposta e `<resposta>.id`; `--triagem` escreve `logs/codex/triagem.log`. | `_triagens` lê o último veredito de cada chamada; `decide_b3` recalcula `b3[obra].codex`. Uma chamada com vários achados vale uma volta. (`bin/codex_pergunta.sh:198`, `bin/disjuntor.py:773`, `bin/disjuntor.py:813`, `bin/disjuntor.py:850`, `bin/disjuntor.py:904`) |
   | Voltas de construtor | Não existe essa medida no B3; a consulta na abertura de `Agent` alcança somente `verificador` com prompt final. | Construtores ficam fora dessa consulta. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`) |

   O escritor do JSON é `_com_estado`, usado pela decisão, triagem e rearme. Os consumidores da decisão são Stop, PreToolUse para **Agent e Bash/Codex**, e também o wrapper por `--b3-parada`; portanto o contrato 1 omite um chamador existente. (`bin/disjuntor.py:237`, `bin/disjuntor.py:818`, `bin/disjuntor.py:959`, `bin/disjuntor.py:1032`, `bin/hook_stop_fila1.py:1048`, `bin/disjuntor.py:1443`, `bin/disjuntor.py:1457`, `bin/codex_pergunta.sh:165`)

   **Nem tudo zera no rearme:** `reprovados` e `codex` ficam vazios e `disparado=False`, mas os identificadores são preservados em `rearmado`, incluindo todas as chamadas já triadas da obra, qualquer que seja o veredito. A liberação por linha preserva identificadores em `liberado` e hashes em `consumidas`. Sobrevive histórico parcial, não um contador completo de voltas. (`bin/disjuntor.py:1017`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:936`)

   Os limiares atuais são **uma reprovação final OU três chamadas Codex PROCEDENTES**; o número retornado é `max(n, nc)`, não a soma. (`bin/disjuntor.py:76`, `bin/disjuntor.py:77`, `bin/disjuntor.py:912`, `bin/disjuntor.py:924`)

2. **Segundo ERRO-BOBO — contrato 3**

   `linhas_libera_b3` não consulta histórico: aceita `ERRO-BOBO <obra>: <motivo>` com ao menos uma letra ou dígito no motivo. Não existe regra especial para o segundo erro da obra. (`bin/disjuntor.py:626`, `bin/disjuntor.py:650`)

   `decide_b3` verifica se a linha está fora da fotografia do disparo, ainda não foi consumida e tem `mtime` suficiente. **Outro ERRO-BOBO, com hash diferente e atendendo essas condições, libera novamente**; não vira DESENHO obrigatório. Repetir a mesma linha consumida não libera. (`bin/disjuntor.py:930`, `bin/disjuntor.py:932`, `bin/disjuntor.py:934`, `bin/disjuntor.py:938`)

   O teste atual cobre consumo da mesma linha e liberação por ERRO-BOBO; não estabelece uma proibição para o segundo ERRO-BOBO distinto. (`bin/tests/test_disjuntor.sh:382`, `bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:1468`)

3. **O que a triagem prova — contrato 3**

   **O sidecar é escrito pelo wrapper**, após sucesso e resposta nova: contém `obra`, `papel`, `hash` do commit consultado, `pid`, `fim` e `sha` da resposta. `--triagem` exige papel `verificador` e confere se o SHA corresponde à resposta. (`bin/codex_pergunta.sh:198`, `bin/disjuntor.py:753`, `bin/disjuntor.py:795`)

   **A linha escrita por `--triagem`** tem quatro campos separados por TAB: `data/hora`, `obra`, `volta`, `veredito`. A identidade efetiva da volta é `hash12@pid@fim@sha12`; os vereditos aceitos são `procedente` e `descartado`. Repetir o mesmo veredito é idempotente; mudar o veredito acrescenta linha e prevalece o último. (`bin/disjuntor.py:79`, `bin/disjuntor.py:802`, `bin/disjuntor.py:808`, `bin/disjuntor.py:813`, `bin/disjuntor.py:850`)

   Isso permite identificar, **por obra e chamada**, uma classificação PROCEDENTE. Não individualiza cada achado nem registra vínculo entre achado, diff seguinte e cura; o SHA confere a resposta, não a correção. (`bin/disjuntor.py:773`, `bin/disjuntor.py:800`, `bin/disjuntor.py:813`)

   Consequentemente, **um PROCEDENTE isolado sem cura não impede hoje o próximo final** pelo B3. O bloqueio Codex exige três PROCEDENTES; mesmo depois disso, uma linha DESENHO válida pode liberar sem comprovação de diff. O teste atual exercita essa liberação apenas escrevendo DESENHO. (`bin/disjuntor.py:912`, `bin/disjuntor.py:930`, `bin/disjuntor.py:938`, `bin/tests/test_disjuntor.sh:1153`, `bin/tests/test_disjuntor.sh:1162`)

4. **ESTADO e medida — contratos 2 e 4**

   O gerador importa `disjuntor.ler/linha` e compõe a seção por essa interface. Hoje `ler` agrega disparos, negativas e liberações em 24 horas; do JSON B3 consulta `b3_sem_obra`. Não publica voltas por obra. (`bin/gerar_estado.py:277`, `bin/gerar_estado.py:280`, `bin/disjuntor.py:1548`, `bin/disjuntor.py:1554`, `bin/disjuntor.py:1620`, `bin/disjuntor.py:1624`, `bin/disjuntor.py:1691`)

   **Lugar natural proposto:** acrescentar a leitura e a apresentação em `disjuntor.ler/linha`, mantendo `gerar_estado` como consumidor. É uma inferência da divisão atual; o contador persistente ainda precisa de definição. (`bin/gerar_estado.py:277`, `bin/gerar_estado.py:281`, `bin/disjuntor.py:1615`)

   Para listar **todas as obras abertas**, falta definir a fonte desse universo: registros B3 nascem de reprovações ou PROCEDENTES, portanto não representam obras abertas sem esses eventos. (`bin/disjuntor.py:897`, `bin/disjuntor.py:902`)

   Os três cenários novos e seus mutantes não constam da suíte atual; a lista existente cobre os mecanismos anteriores de B3, triagem, rearme e concorrência. “Cada um morde” e a contagem real **GRADE-FOLGA-JUIZ = 9** são **[não verificado]**; o anexo fornece somente a expectativa. (`bin/tests/test_disjuntor.sh:1468`, `bin/tests/test_disjuntor.sh:1507`, `bin/tests/test_disjuntor.sh:1587`, `bin/tests/test_disjuntor.sh:1784`)

   Não executei a suíte: ela cria temporários, árvores e arquivos mutantes, incompatíveis com a regra de somente leitura. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:1475`)

5. **Correções e limites do contrato, linha por linha**

   | Linha | Avaliação diante da cópia |
   |---|---|
   | **1** | **Premissa incompleta:** as fontes já lidas não cobrem todas as voltas, e há também o chamador do wrapper. Guardar o teto dentro de `decide_b3` não faz construtores passarem automaticamente por essa decisão. (`bin/disjuntor.py:736`, `bin/disjuntor.py:857`, `bin/disjuntor.py:1454`, `bin/codex_pergunta.sh:165`) |
   | **2** | **Comportamento novo:** não há teto de seis voltas, emissão de `PADRAO-DE-CIMA` ou comandos `reabre com N voltas`/`redesenha` nessa interface. A CLI atual oferece triagem, consulta e rearme. (`bin/disjuntor.py:76`, `bin/disjuntor.py:912`, `bin/disjuntor.py:1699`) |
   | **3** | **Exige mudanças:** segundo ERRO-BOBO ainda libera; não há prova de cura; “B3 do dia” é impreciso, pois decisão e rearme não têm janela diária. Estado ilegível de fato levanta em `_com_estado`, e o juiz transforma a exceção em NAO_SEI, recusado na porta B3. (`bin/disjuntor.py:930`, `bin/disjuntor.py:1003`, `bin/disjuntor.py:265`, `bin/disjuntor.py:981`, `bin/disjuntor.py:1046`) |
   | **4** | **Critério futuro, sem comprovação atual:** faltam os três cenários novos e o painel por obra; execução e número nove são **[não verificado]**. (`bin/tests/test_disjuntor.sh:1468`, `bin/disjuntor.py:1691`) |
   | **5** | **Autoridade parcialmente instrumentalizada:** há guarda textual contra o Code citar a chave ou executar `--rearma-b3`; a função de rearme não autentica Ronald, e `USER` serve ao registro. Reabertura por quantidade e redesenho ainda não existem na CLI. (`bin/disjuntor.py:160`, `bin/disjuntor.py:1003`, `bin/disjuntor.py:1026`, `bin/disjuntor.py:1699`) |

   **ID não reconhecido:** um Agent final sem obra reconhecida pula a consulta B3; posteriormente pode aparecer apenas em `b3_sem_obra`. O contrato precisa declarar o tratamento dessa abertura. (`bin/disjuntor.py:1455`, `bin/disjuntor.py:1457`, `bin/disjuntor.py:746`, `bin/disjuntor.py:893`)

   **Concorrência:** a transição B3 é serializada, mas não há reserva de uma volta por obra entre autorização e execução. O wrapper trava por caminho da resposta; por inferência, chamadas da mesma obra com respostas diferentes podem passar antes de chegarem novas triagens. (`bin/disjuntor.py:253`, `bin/disjuntor.py:286`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:174`)

   **Desligamento incompleto:** Stop e PreToolUse consultam `DISJUNTOR_DESLIGADO`; a porta B3 do wrapper não consulta. Assim, pela leitura do fluxo, a chave não libera essa porta para obra PARADA/NAO_SEI. Também não há exigência de linha no RELATO para reconhecer o desligamento: basta a existência da chave. (`bin/hook_stop_fila1.py:1039`, `bin/disjuntor.py:1487`, `bin/codex_pergunta.sh:165`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:144`)

   **Retroatividade:** o histórico disponível é parcial, e a porta do wrapper decide sem transcript, usando reprovações de agentes já persistidas. Portanto “obras abertas entram com as voltas que têm” precisa definir alcance, fontes e tratamento de voltas ausentes ou ainda em voo. (`bin/disjuntor.py:736`, `bin/disjuntor.py:898`, `bin/disjuntor.py:1039`)

## 31/10 23:47 · sessao b0d1525dba7e · - · - · origem=Ronald (tmux)

- **Chamadores reais:** Stop chama `julga_b3` em `bin/hook_stop_fila1.py:1048`, acrescentado ao recado em `bin/hook_stop_fila1.py:1300`; PreToolUse chama `estado_b3` via `_nega_b3` (`bin/disjuntor.py:1402`), tanto para `Bash codex_pergunta` (`bin/disjuntor.py:1440`) quanto para `Agent verificador final` (`bin/disjuntor.py:1454`). Existe ainda a porta do wrapper: `bin/codex_pergunta.sh:165` → `b3_parada` → `estado_b3` (`bin/disjuntor.py:1042`) → `julga_b3` → `decide_b3` (`bin/disjuntor.py:980`). Portanto linha 1 omite um chamador existente e restringe incorretamente PreToolUse a Agent. (`bin/disjuntor_settings.json:7` usa matcher vazio.)

- **Não há contagem total de construtor/verificador em B3:** os transcripts produzem exclusivamente reprovações finais por `agent_id` (`bin/disjuntor.py:736`); são exigidos metadado `agentType=verificador`, prompt reconhecido como final e entrega `SubagentHandback` cuja primeira linha começa `REPROVADO` (`bin/disjuntor.py:742`, `bin/disjuntor.py:555`, `bin/disjuntor.py:561`). O harness é identificado no código como escritor dos transcripts (`bin/disjuntor.py:336`); sua implementação não está verificada. A outra fonte são voltas Codex verificador triadas PROCEDENTE, uma por chamada (`bin/disjuntor.py:773`, `bin/disjuntor.py:858`). O retorno usa **max(n, nc)**, não soma (`bin/disjuntor.py:924`).

- **Outras medidas não equivalem a voltas por obra:** o Stop guarda bloqueios por HEAD, teto 5 (`bin/hook_stop_fila1.py:35`, `bin/hook_stop_fila1.py:37`, `bin/hook_stop_fila1.py:1250`, `bin/hook_stop_fila1.py:1277`); o wrapper tem default `CODEX_TETO=6`, mas para execuções simultâneas globais, configurável por env (`bin/codex_pergunta.sh:47`, `bin/codex_pergunta.sh:137`). O painel conta agentes Claude por modelo/tipo em 24h, sem obra (`bin/disjuntor.py:1631`, `bin/disjuntor.py:1646`), e chamadas Codex por papel, incluindo `ini` de chamadas recusadas (`bin/codex_limite_estado.py:71`, `bin/codex_pergunta.sh:88`, `bin/codex_pergunta.sh:165`).

- **ESTADO:** `bin/gerar_estado.py:280` importa `ler/linha` de `disjuntor`; `bin/gerar_estado.py:281` compõe a linha, inserida na seção Disjuntores em `bin/gerar_estado.py:336` e `bin/gerar_estado.py:365`. O leitor atual deriva disparos, negativas e liberações da trilha em janela 24h (`bin/disjuntor.py:1548`, `bin/disjuntor.py:1554`, `bin/disjuntor.py:1568`); do JSON B3 lê apenas `b3_sem_obra` (`bin/disjuntor.py:1624`). Não publica lista ou contagem por obra (`bin/disjuntor.py:1615`, `bin/disjuntor.py:1691`). Lugar natural proposto: ampliar esse `ler/linha`, preservando `gerar_estado` como consumidor; é inferência arquitetural sustentada por `bin/gerar_estado.py:277`.

- **Universo de obras abertas:** o gerador já usa BACKLOG/bloco OBRAS para obras interrompidas (`bin/gerar_estado.py:236`); leitor correspondente abre `app/docs/BACKLOG.md` e recorta `OBRAS:INICIO/FIM` (`bin/interrompidas_estado.py:89`, `bin/interrompidas_estado.py:94`). Esse leitor só enumera interrompidas (`bin/interrompidas_estado.py:106`); o contrato ainda precisa definir a fonte e o critério de todas as abertas. O B3 cria registros apenas a partir de reprovações ou PROCEDENTES (`bin/disjuntor.py:897`, `bin/disjuntor.py:902`).

- **ID ausente:** Agent final sem ID reconhecido **pula a consulta B3**, pois `_nega_b3` só roda se `ob` (`bin/disjuntor.py:1455` e `bin/disjuntor.py:1457`). Depois pode aparecer apenas no agregado `b3_sem_obra` (`bin/disjuntor.py:746`, `bin/disjuntor.py:893`). Reconhecedor exige formato específico e primeira ocorrência válida (`bin/disjuntor.py:97`, `bin/disjuntor.py:104`, `bin/disjuntor.py:514`, `bin/disjuntor.py:521`).

- **Chave desligada não cobre a porta do wrapper:** Stop verifica `desligado` (`bin/hook_stop_fila1.py:1039`), PreToolUse também (`bin/disjuntor.py:1487`), mas wrapper consulta incondicionalmente para papel verificador (`bin/codex_pergunta.sh:164`), e `b3_parada` consulta o juiz sem checar a chave (`bin/disjuntor.py:1042`). Inferência direta: uma obra parada/NAO_SEI continua recusada por essa porta mesmo com a chave existente. O contrato precisa declarar ou corrigir essa divergência.

- **Concorrência:** transição B3 é serializada por flock e gravação atômica (`bin/disjuntor.py:249`, `bin/disjuntor.py:253`, `bin/disjuntor.py:268`, `bin/disjuntor.py:274`); anúncios também ficam sob a trava (`bin/disjuntor.py:284`). O wrapper trava **por caminho da resposta** (`bin/codex_pergunta.sh:162`), verifica B3 (`bin/codex_pergunta.sh:165`) e depois executa Codex (`bin/codex_pergunta.sh:174`). Não há reserva por obra nessa transação; duas respostas diferentes da mesma obra podem ser admitidas antes dos resultados serem triados — inferência desses trechos e da gravação posterior à resposta em `bin/disjuntor.py:802` e `bin/disjuntor.py:813`.

- **“B3 do dia” e autoridade Ronald:** as decisões/rearme não possuem reset por data (`bin/disjuntor.py:896`, `bin/disjuntor.py:1003`); 24h é só a janela do painel (`bin/disjuntor.py:1548`). “Só Ronald” é guarda textual de ferramentas (`bin/disjuntor.py:160`, `bin/disjuntor.py:162`), sem autenticação em `rearma_b3`; `USER` é usado somente no registro (`bin/disjuntor.py:1026`).

## 31/10 23:46 · sessao 13474eb913ba · - · - · origem=Ronald (tmux)

Leitura integral concluída; o teste tem 1.791 linhas e sua execução cria temporários, árvores e cópias mutantes, portanto não o executei sob a regra de somente leitura. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:50`, `bin/tests/test_disjuntor.sh:1475`)

- **Linha 1 do contrato — trilha insuficiente para contar toda volta:** os testes distinguem a leitura curta que não conta, a reprovação final que para a obra e a chamada Codex que só conta depois de triada; não equivalem a um contador geral de construtor/verificador. (`bin/tests/test_disjuntor.sh:341`, `bin/tests/test_disjuntor.sh:344`, `bin/tests/test_disjuntor.sh:1144`, `bin/tests/test_disjuntor.sh:1150`)
- **Linha 2 — teto 6 e sétima negada:** não aparecem entre os casos declarados; há limites diferentes: primeira reprovação final e terceira volta Codex PROCEDENTE. (`bin/tests/test_disjuntor.sh:785`, `bin/tests/test_disjuntor.sh:1153`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1468`)
- **Linha 3 — rearme:** os testes exigem que o rearme exclua também triagens PROCEDENTES ainda não varridas, inclusive numa obra sem decisão anterior. Isso comprova reset do B3, não permanência de um teto acumulado. (`bin/tests/test_disjuntor.sh:1181`, `bin/tests/test_disjuntor.sh:1186`, `bin/tests/test_disjuntor.sh:1287`)
- **Linha 3 — segundo ERRO-BOBO:** existe teste de liberação por ERRO-BOBO e de consumo único da mesma linha; não há cenário de segundo ERRO-BOBO diferente na mesma obra. A forma aceita até uma letra ou um dígito como motivo. (`bin/tests/test_disjuntor.sh:382`, `bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:706`, `bin/tests/test_disjuntor.sh:740`, `bin/tests/test_disjuntor.sh:1468`)
- **Linha 3 — PROCEDENTE sem cura:** o teste atual libera uma obra com três PROCEDENTES apenas escrevendo DESENHO; não produz nem verifica diff de cura nesse caminho. (`bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1162`, `bin/tests/test_disjuntor.sh:1164`, `bin/tests/test_disjuntor.sh:1165`)
- **Linha 4 — três mutantes novos:** não constam no inventário atual; os mutantes existentes cobrem limiar Codex 3→2, contagem de DESCARTADO, reset e consumo da mesma linha, entre outros. (`bin/tests/test_disjuntor.sh:1558`, `bin/tests/test_disjuntor.sh:1560`, `bin/tests/test_disjuntor.sh:1597`, `bin/tests/test_disjuntor.sh:1632`, `bin/tests/test_disjuntor.sh:1784`) “Cada um morde” e “GRADE-FOLGA-JUIZ = 9” permanecem **[não verificado]**.
- **Linha 5 — chave e autorização:** há testes para a chave desligar PreToolUse e Stop, e para negar ao Code comandos com a chave ou `--rearma-b3`. Não há testes de “reabre com N voltas”, “redesenha” ou reversão de commit neste instrumento. (`bin/tests/test_disjuntor.sh:585`, `bin/tests/test_disjuntor.sh:593`, `bin/tests/test_disjuntor.sh:596`, `bin/tests/test_disjuntor.sh:603`, `bin/tests/test_disjuntor.sh:1468`)

Limites úteis para a consolidação: há teste de reprovação final sem obra reconhecida aparecer em `b3_sem_obra`; há testes de transição sob uma trava, anúncio sob a trava, triagem bloqueada por trava ocupada e consulta após obtenção de slot. Isso não constitui teste de reserva atômica de seis admissões concorrentes. (`bin/tests/test_disjuntor.sh:427`, `bin/tests/test_disjuntor.sh:820`, `bin/tests/test_disjuntor.sh:1064`, `bin/tests/test_disjuntor.sh:1347`, `bin/tests/test_disjuntor.sh:1419`, `bin/tests/test_disjuntor.sh:1468`)

_1306 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
