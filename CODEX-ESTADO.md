# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 00:25** · publicado em **01/11/2026 00:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 00:25 · sessao ab9069c1e335 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**O comentário foi corrigido; o delta desta volta não muda valores calculados.** As duas alterações são “HE100” → “HE50”, coerentes com o teste central. (app/ponto/motor_calculo_v2.py:145; app/ponto/motor_calculo_v2.py:146; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:70)

**1. Situação dos achados da b1**

| Achado | Estado em 5394309c |
|---|---|
| **P1 — excesso microscópico de `float` sobre a união** | **Superado pelo descarte da triagem; sem cura numérica.** A acumulação continua em `float`, e as propriedades continuam usando tolerância `1e-9`. (app/ponto/motor_calculo_v2.py:193; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:203; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:205) |
| **P2 — overflow no extremo de datetime** | **Superado pelo descarte da triagem; possibilidade técnica permanece.** O retorno consulta `minutos_noturnos` mesmo sem pausas; essa função continua avançando datas sem guarda de overflow. (app/ponto/motor_calculo_v2.py:196; app/ponto/motor_calculo_v2.py:104; app/ponto/motor_calculo_v2.py:117) |
| **P3 — `feriados=set()` permite fallback ORM** | **Superado como impeditivo pela triagem; mecanismo permanece.** Data ausente chama `is_feriado`, que consulta o juiz e converte exceções em `False`. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66; app/ponto/motor_calculo_v2.py:953; app/ponto/motor_calculo_v2.py:955; app/ponto/motor_calculo_v2.py:39; app/ponto/motor_calculo_v2.py:43) |
| **P4 — comentário atribuindo o caso à HE100** | **Resolvido.** Agora diz HE50, como as expectativas HE50=2h, HE100=0 e HE50 noturna=2h. (app/ponto/motor_calculo_v2.py:145; app/ponto/motor_calculo_v2.py:146; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:70; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:72) |

As demais conclusões e ressalvas da b1 permanecem:

- **Caso central corrigido anteriormente, preservado:** desconto esperado de 60 minutos, HE50 noturna de 2h e noturnas de 8h. Isso está nas asserções; execução nesta árvore é **[não verificado]**. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:112; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:72; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:82)
- **Laço parado, resultado negativo ou acima do todo seguem afastados pela estrutura**, ressalvadas exceções: o cursor avança, somente durações positivas entram na soma e o retorno aplica o teto do todo. (app/ponto/motor_calculo_v2.py:179; app/ponto/motor_calculo_v2.py:188; app/ponto/motor_calculo_v2.py:193; app/ponto/motor_calculo_v2.py:196)
- **Normalização e limites temporais permanecem:** aware converte de fuso; naive usa `localize` sem política explícita para ambiguidade/inexistência; o cursor relê o instante normalizado. Há casos específicos de mudança de offset, verão e segundos. (app/ponto/motor_calculo_v2.py:73; app/ponto/motor_calculo_v2.py:186; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:415; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:420; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:480)
- **Import, helper, assinatura e alcance do patch seguem sem defeito identificado:** `_aw` é local, Django é declarado, `he_pontas` é desempacotado em quatro campos e o wrapper resolve a função substituída pelos mutantes. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:44; app/requirements.txt:13; app/ponto/calculador/regras.py:818; app/ponto/motor_calculo_v2.py:1915)
- **Limites das expectativas e do oráculo permanecem:** a grade reutiliza `localizar`, trabalha com minutos inteiros e reconhece políticas da casa; os casos históricos e com segundos usam interseções próprias. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:157; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:160; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:167; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:410; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:473)
- **Alcance produtivo continua delimitado:** a alimentação diária passa `intervalos=()`, enquanto o adaptador por par fornece as pausas. (app/ponto/calculador/alimentacao.py:514; app/ponto/calculador/periodo.py:507)
- **Nova suíte após a correção, suíte completa, impacto produtivo, ordem de travas e pouso:** **[não verificado]**. O GREEN anterior mencionado na triagem não demonstra execução após este delta. **[não verificado]** [nao verificado]

**2. Linhas alteradas fora do corpo**

**Nenhuma no `diff2.txt` fornecido.** As únicas linhas alteradas são os comentários **145 e 146**, dentro da função delimitada por **121–196**. A comparação sintática em memória, revertendo apenas essas duas substituições, resultou em AST idêntica. (diff2.txt:5; app/ponto/motor_calculo_v2.py:121; app/ponto/motor_calculo_v2.py:145; app/ponto/motor_calculo_v2.py:146; app/ponto/motor_calculo_v2.py:196)

A completude histórica do anexo perante os commits não pôde ser conferida: a cópia não contém `.git`. **[não verificado]** [nao verificado]

**3. Escritores, leitores e mudança de valores**

O corpo devolve **um único escalar: desconto noturno em minutos**, produzido pela acumulação em `total` e limitado pelo noturno do recorte. Não devolve diretamente HE50, HE100 ou minutos legais. (app/ponto/motor_calculo_v2.py:176; app/ponto/motor_calculo_v2.py:193; app/ponto/motor_calculo_v2.py:196)

| Caminho | Quem lê e quem escreve o resultado derivado |
|---|---|
| **HE noturna** | `he_noturna` subtrai o escalar e reparte o saldo entre HE50/HE100. `_aplicar_he_noturna` grava os campos do período; `regras.do_dia` grava as rubricas em horas. (app/ponto/motor_calculo_v2.py:442; app/ponto/motor_calculo_v2.py:448; app/ponto/motor_calculo_v2.py:450; app/ponto/motor_calculo_v2.py:1008; app/ponto/motor_calculo_v2.py:1010; app/ponto/calculador/regras.py:828; app/ponto/calculador/regras.py:830) |
| **Noturnas pelo motor** | `_noturno_dos_intervalos` encaminha o escalar; `calcular_noturnas` subtrai e aplica o fator; `_escrever_noturnas` grava relógio/legais no período. (app/ponto/motor_calculo_v2.py:1915; app/ponto/motor_calculo_v2.py:973; app/ponto/motor_calculo_v2.py:974; app/ponto/motor_calculo_v2.py:992; app/ponto/motor_calculo_v2.py:993) |
| **Noturnas pelo calculador** | `_ContextoNoturno` empresta o mesmo wrapper; `noturnas_do_dia` subtrai o desconto. `do_dia` escreve horas/detalhes; o adaptador por par escreve relógio/legais no período. (app/ponto/calculador/regras.py:63; app/ponto/calculador/regras.py:86; app/ponto/calculador/regras.py:840; app/ponto/calculador/regras.py:841; app/ponto/calculador/periodo.py:507; app/ponto/calculador/periodo.py:752; app/ponto/calculador/periodo.py:753) |
| **Persistência e apresentação posteriores** | `lavrar` agrega e persiste `DiaPago`; `ResultadoMes` agrega para o fechamento mensal. Apresentação diária e exportação leem esses resultados. (app/ponto/services/dia_pago.py:179; app/ponto/services/dia_pago.py:322; app/ponto/services/dia_pago.py:358; app/ponto/motor_calculo_v2.py:321; app/ponto/motor_calculo_v2.py:340; app/ponto/motor_calculo_v2.py:344; app/ponto/services/fechamento.py:1128; app/ponto/services/dia_pago.py:922; app/folha/export.py:436) |

Os outros cinco locais de chamada direta encontrados estão no teste P5: `_f`, caso com segundos, `_celula` e os dois `_caso`. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:108; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:142; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:197; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:409; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:472)

**Nenhum desses leitores muda de valor pelo delta2**, que altera somente comentários. As mudanças numéricas descritas na b1 pertencem à cura anterior, preservada nesta cópia. (diff2.txt:9; diff2.txt:11; app/ponto/motor_calculo_v2.py:145; app/ponto/motor_calculo_v2.py:196)

**4. Classes que já reprovaram obra**

**Não é possível certificar todo o diff `e0bdbc8e..5394309c`: esse diff não foi fornecido e não há histórico Git na cópia. [não verificado]** No corpo P5, consumidores e testes examinados:

| Classe | Resultado delimitado ao que foi lido |
|---|---|
| **Segundo juiz** | **Não encontrado.** O corpo consulta a mesma `minutos_noturnos`; a autoridade do valor do par permanece registrada. (app/ponto/motor_calculo_v2.py:191; app/ponto/motor_calculo_v2.py:196; app/core/juizes.py:1160) |
| **Segundo escritor introduzido pela cura** | **Não encontrado.** O corpo retorna o escalar; os campos derivados passam pelos escritores existentes. (app/ponto/motor_calculo_v2.py:196; app/ponto/motor_calculo_v2.py:992; app/ponto/motor_calculo_v2.py:1008; app/ponto/calculador/periodo.py:752) |
| **Relógio fora da borda** | **Não encontrado.** O cursor deriva das entradas e os testes fixam datas, sem leitura de “agora” nesses trechos. (app/ponto/motor_calculo_v2.py:162; app/ponto/motor_calculo_v2.py:186; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:187) |
| **Derivação paralela de fonte única** | **Não encontrada no corpo produtivo.** A presença noturna é consultada em `minutos_noturnos`; a regra independente está no teste como oráculo. (app/ponto/motor_calculo_v2.py:191; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151) |
| **RED sem mordida** | **Não encontrado por inspeção.** O patch alcança o símbolo chamado, e há testemunhas para os cinco mutantes. Execução nesta árvore: **[não verificado]**. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:356; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:359; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:365; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:454; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:531) |

## 01/11 00:24 · sessao 19d5e5096e49 · - · - · origem=Ronald (tmux)

Classificação dos achados da b1, considerando a triagem fornecida:

| Achado | Estado nesta volta | Conferência atual |
|---|---|---|
| P1: arredondamento acima da união | **Superado pelo descarte; sem alteração numérica.** A soma continua em `float`; a propriedade permite `1e-9`. | `app/ponto/motor_calculo_v2.py:193`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:201`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:203`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:205` |
| P2: overflow no limite de datetime | **Superado pelo descarte; possibilidade técnica permanece.** O retorno chama `minutos_noturnos` mesmo sem pausas; essa função incrementa dia sem guarda. | `app/ponto/motor_calculo_v2.py:196`; `app/ponto/motor_calculo_v2.py:104`; `app/ponto/motor_calculo_v2.py:117` |
| P3: `feriados=set()` não fecha a fixture | **Superado pelo descarte; fallback permanece.** Data ausente consulta `is_feriado`; esta chama o juiz e converte exceção em `False`. A triagem relata GREEN anterior, mas não fornece prova de nova execução nesta árvore. | `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66`; `app/ponto/motor_calculo_v2.py:953`; `app/ponto/motor_calculo_v2.py:955`; `app/ponto/motor_calculo_v2.py:39`; `app/ponto/motor_calculo_v2.py:43`; nova execução: **[não verificado]** |
| P4: comentário HE100 em caso HE50 | **Resolvido.** As duas linhas agora dizem HE50, coerente com HE50=2h e HE100=0 do teste. | `app/ponto/motor_calculo_v2.py:145`; `app/ponto/motor_calculo_v2.py:146`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:70`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:71`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:72` |

Demais ressalvas da b1:

- **Alcance da alimentação produtiva segue delimitado:** o caminho diário entrega `intervalos=()`; o adaptador por par entrega as pausas. O delta apenas de comentário não altera isso. (`app/ponto/calculador/alimentacao.py:512`; `app/ponto/calculador/alimentacao.py:514`; `app/ponto/calculador/periodo.py:507`)
- **Origem das expectativas segue explícita:** o teste declara a regra e reconhece o gatilho por toque em 00–05 e o teto de 26h como escolhas da casa; a aproximação da HE está documentada na própria função. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:14`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:157`; `app/ponto/motor_calculo_v2.py:413`)
- **Limites do oráculo permanecem:** reutiliza `localizar`, opera minuto inteiro e declara grade sem verão em 2026; segundos e história têm expectativas próprias de interseção. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:160`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:164`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:167`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:410`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:473`)
- **Import/helper/assinatura/alias permanecem sem defeito identificado:** `_aw` é local, Django é declarado, `do_dia` aceita os argumentos e desempacota quatro campos; os imports dos helpers ocorrem durante a chamada e o contexto empresta o método do motor. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:44`; `app/requirements.txt:13`; `app/ponto/calculador/regras.py:572`; `app/ponto/calculador/regras.py:818`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:105`; `app/ponto/calculador/regras.py:63`; `app/ponto/motor_calculo_v2.py:1915`)
- **Laço parado segue afastado pela estrutura:** `_f > a`, o próximo candidato avança ou recebe avanço subsidiário, e `a=b`; exceções continuam fora dessa garantia. (`app/ponto/motor_calculo_v2.py:179`; `app/ponto/motor_calculo_v2.py:188`; `app/ponto/motor_calculo_v2.py:190`; `app/ponto/motor_calculo_v2.py:194`)
- **RED sem mordida não encontrado no teste lido:** patch alcança a função e exige falha do caso; há testemunhas para os cinco mutantes. Isso é evidência estrutural, sem alegar execução nesta árvore. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:348`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:356`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:359`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:365`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:454`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:531`)
- **Relógio externo não encontrado no corpo nem teste lidos:** o corpo deriva cortes das entradas e o teste usa datas fixadas. (`app/ponto/motor_calculo_v2.py:162`; `app/ponto/motor_calculo_v2.py:186`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:187`)
- **Suíte inteira da árvore 5394309c, produção, ordem de travas e pouso seguem [não verificado].** A triagem apenas relata GREEN de 34 testes da situação anterior; não há execução feita por este explorer. **[não verificado]**

## 01/11 00:23 · sessao c5483f4142a7 · - · - · origem=Ronald (tmux)

Não encontrei essas classes no corpo atual e no teste P5 lidos; **o diff total `e0bdbc8e..5394309c` não pôde ser conferido**, pois a cópia não contém `.git` nem foi fornecido esse diff completo. [não verificado]

| Classe | Resultado e evidência |
|---|---|
| Segundo juiz | **Não encontrado no corpo lido**: a janela continua sendo consultada em `minutos_noturnos`; o corpo não repete os limites 22/05 ou o gatilho da prorrogação. (`app/ponto/motor_calculo_v2.py:191`; `app/ponto/motor_calculo_v2.py:196`) A autoridade do valor do par permanece registrada em `app/core/juizes.py:1160`. |
| Segundo escritor | **Não encontrado no corpo lido**: produz um escalar por `return`; os consumidores usam as atribuições existentes dos campos noturnos e HE noturna. (`app/ponto/motor_calculo_v2.py:196`; `app/ponto/motor_calculo_v2.py:992`; `app/ponto/motor_calculo_v2.py:993`; `app/ponto/motor_calculo_v2.py:1008`; `app/ponto/motor_calculo_v2.py:1010`) |
| Relógio fora da borda | **Não encontrado no corpo/teste lidos**: o cursor deriva do instante recebido, normalizado no fuso; os testes fixam datas. Não há consulta a “agora” nesses trechos. (`app/ponto/motor_calculo_v2.py:162`; `app/ponto/motor_calculo_v2.py:186`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:187`) |
| Derivação paralela de fonte única | **Não encontrada no corpo produtivo lido**: mede presença pela mesma `minutos_noturnos`, unindo intervalos antes da soma. (`app/ponto/motor_calculo_v2.py:164`; `app/ponto/motor_calculo_v2.py:169`; `app/ponto/motor_calculo_v2.py:191`) A regra independente está no teste, como oráculo, e não em outro leitor produtivo. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:177`) |
| RED sem mordida | **Não encontrado por inspeção dos caminhos**: os testes substituem o símbolo realmente chamado e exigem falha dos casos ou divergência da grade para cinco mutantes. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:356`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:359`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:365`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:454`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:531`) Execução nesta árvore: [não verificado]. |

O delta fornecido nesta volta troca somente “HE 100” por “HE 50” em duas linhas de comentário, agora coerentes com o teste central. (`app/ponto/motor_calculo_v2.py:145`; `app/ponto/motor_calculo_v2.py:146`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:70`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:72`)

## 01/11 00:23 · sessao 5e6f299b7c4a · - · - · origem=Ronald (tmux)

O corpo devolve **um único valor**, o desconto noturno em minutos (`float`), produzido por `total = 0.0`, acumulação e teto final; não devolve HE50/HE100 nem minutos legais. (`app/ponto/motor_calculo_v2.py:121`, `:176`, `:193`, `:196`)

| Leitor direto produtivo | Uso do escalar e leitores seguintes |
|---|---|
| `he_noturna` | Subtrai o desconto do noturno da cauda extra; reparte o saldo entre HE50/HE100 e devolve o par. (`app/ponto/motor_calculo_v2.py:442`, `:448`, `:450`, `:451`) |
| `MotorBase._noturno_dos_intervalos` | Encaminha o escalar com a prorrogação cadastrada. (`app/ponto/motor_calculo_v2.py:1915`, `:1916`) |

O wrapper tem dois consumidores: `calcular_noturnas` subtrai o desconto, aplica o fator e retorna relógio/legais; `_escrever_noturnas` grava esses resultados no período. (`app/ponto/motor_calculo_v2.py:973`, `:974`, `:975`, `:991`, `:992`, `:993`)

O outro consumidor usa o mesmo método emprestado por `_ContextoNoturno`; `noturnas_do_dia` subtrai o desconto e retorna relógio/legais. `do_dia` converte legais para horas e registra detalhes; `noturno_no_par` devolve `NoturnoV3`, lido pelo adaptador que preenche os dois campos do período. (`app/ponto/calculador/regras.py:63`, `:86`, `:89`, `:836`, `:840`, `:841`, `:842`; `app/ponto/calculador/periodo.py:507`, `:510`, `:751`, `:752`, `:753`)

O split de HE é lido por `_aplicar_he_noturna`, que preenche os campos do período, e por `do_dia`, que soma os splits e escreve as rubricas diárias em horas. (`app/ponto/motor_calculo_v2.py:1002`, `:1008`, `:1010`; `app/ponto/calculador/regras.py:823`, `:825`, `:826`, `:828`, `:830`)

Na busca textual em `app/**/*.py`, os outros cinco locais de chamada direta são todos no teste P5: helper `_f`, caso com segundos, `_celula` e os dois helpers `_caso`; seus resultados alimentam asserções de valor/propriedades. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:108`, `:142`, `:143`, `:197`, `:199`, `:201`, `:203`, `:205`, `:409`, `:411`, `:472`, `:475`)

Os escritores/leitores posteriores relevantes encontrados são:

- `lavrar` soma os campos dos períodos em horas; pode receber rubricas do calculador conforme a proveniência, constrói `DiaPago` e persiste por `bulk_create`. (`app/ponto/services/dia_pago.py:179`, `:181`, `:184`, `:224`, `:322`, `:327`, `:328`, `:343`, `:358`)
- `ResultadoMes` soma minutos legais e HE noturna em horas; fechamento arredonda e grava os três campos mensais. (`app/ponto/motor_calculo_v2.py:321`, `:340`, `:344`; `app/ponto/services/fechamento.py:893`, `:896`, `:899`, `:1128`, `:1131`, `:1134`, `:1137`)
- Apresentação diária lê `DiaPago.horas_noturnas`; exportação lê `FechamentoMensal.horas_noturnas` como adicional noturno. (`app/ponto/services/dia_pago.py:922`; `app/folha/export.py:436`)

**Nenhum leitor muda de valor pelo delta2 fornecido:** a alteração é somente “HE 100” → “HE 50” nos comentários; o retorno executável continua em `:196`. (`app/ponto/motor_calculo_v2.py:145`, `:146`, `:196`; completude histórica do anexo: [não verificado])

As mudanças `0→60`, HE50 noturna `3→2h` e noturnas `9→8h` pertencem à volta1; o mutante preserva o cálculo anterior por peça, e o teste declara o caso e as expectativas atuais. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:57`, `:60`, `:62`, `:72`, `:82`, `:86`, `:94`, `:112`)

O alcance produtivo permanece delimitado: a alimentação diária passa `intervalos=()`, enquanto o adaptador por par fornece intervalos julgados; não se deve atribuir à alimentação diária o mesmo desconto não vazio do RED. (`app/ponto/calculador/alimentacao.py:512`, `:513`, `:514`; `app/ponto/calculador/periodo.py:501`, `:507`)

## 01/11 00:16 · sessao 37fb4120dc78 · CONTENCAO-QUE-MORDE · leitura · origem=Code

A v2 nomeia vários pontos de encaixe corretamente, mas eles ainda executam o contrato anterior: B3 conta reprovações, B1 depende de tokens, e o Stop não cobra gerações de contadores. Os gates de modelo, suite antes do opus, seis Agents vivos e contrato no push não aparecem nas respectivas portas. (`bin/disjuntor.py:449`, `bin/disjuntor.py:912`, `bin/disjuntor.py:1451`, `bin/hook_stop_fila1.py:1184`, `bin/pre-push.sh:200`)

A identificação desta cópia com o HASH informado não pôde ser confirmada por Git: ela não contém `.git`. **[não verificado]** Nos casos abaixo, separo contraexemplo dedutível do código de ocorrência em produção; a sonda fornecida não contém decisões dos hooks suficientes para demonstrar doze ocorrências reais. **[não verificado]**

Para evitar repetir proprietários em cada item, este é o mapa compartilhado de escrita e leitura:

| Estado/campos | Quem escreve | Quem lê |
|---|---|---|
| `disjuntor_estado.json` | Escrita física por `_com_estado`, sob flock e substituição de temporário. Ausente começa como `{}`; ilegível levanta. (`bin/disjuntor.py:237`, `bin/disjuntor.py:262`, `bin/disjuntor.py:274`) | `_com_estado` e `ler_estado`; decisões B1/B3/B4 e painel. (`bin/disjuntor.py:260`, `bin/disjuntor.py:291`, `bin/disjuntor.py:1626`) |
| `b3[obra]` | `decide_b3`: `reprovados`, `codex`, `disparado`, fotografia/instante do disparo, `liberado`, `consumidas`; rearme altera `rearmado` e limpa listas correntes. Não existe `voltas` nesses fluxos. (`bin/disjuntor.py:713`, `bin/disjuntor.py:891`, `bin/disjuntor.py:936`, `bin/disjuntor.py:1018`) | A mesma decisão, chamada pelas portas B3; painel lê liberações pela trilha. (`bin/disjuntor.py:980`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:1574`) |
| `b3_sem_obra` | `decide_b3` acrescenta IDs dos verificadores finais sem obra encontrados posteriormente nos transcripts. (`bin/disjuntor.py:892`) | Painel conta o comprimento acumulado, sem janela temporal. (`bin/disjuntor.py:1624`) |
| `b1[id].{checado,disparado,motivo}` | `b1_do_agente`, via `_com_estado`. (`bin/disjuntor.py:481`, `bin/disjuntor.py:486`) | Próximo PreToolUse do agente e Stop, neste último apenas para disparos já gravados. (`bin/disjuntor.py:476`, `bin/hook_stop_fila1.py:1054`) |
| `b4.negou_desde`, `b4_para.desde` | PreToolUse/B4 Stop, via `_com_estado`. (`bin/disjuntor.py:1465`, `bin/disjuntor.py:1215`) | Transições B4; não há sequência persistida de Stops com cota desconhecida. (`bin/disjuntor.py:1206`, `bin/disjuntor.py:1461`) |
| `estado_contadores.json` | **Nenhum escritor atual no gerador contratado**: ele termina gravando o Markdown em `dst`. (`bin/gerar_estado.py:376`) | **Nenhum leitor atual no Stop contratado**: o fluxo não consulta esse sidecar. (`bin/hook_stop_fila1.py:1184`, `bin/hook_stop_fila1.py:1300`) |
| `triagem.log`: hora, obra, volta, veredito | CLI `--triagem` → `_triagem_sob_trava`, sob a trava do estado; atualiza também `b3_triagem_linhas`. (`bin/disjuntor.py:804`, `bin/disjuntor.py:812`, `bin/disjuntor.py:815`) | `_triagens`, consumido por decisão, triagem e rearme; `_procedentes` seleciona as voltas procedentes. (`bin/disjuntor.py:823`, `bin/disjuntor.py:857`, `bin/disjuntor.py:1007`) |
| `<resposta>.id`: obra, papel, hash, pid, fim, sha | Wrapper remove o anterior e grava outro somente com rc 0 e resposta desta chamada. Reutilizar o caminho substitui o histórico anterior. (`bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:198`) | `_le_id` durante triagem; valida campos e SHA da resposta. Não há hoje semeadura geral desses arquivos. (`bin/disjuntor.py:753`, `bin/disjuntor.py:795`, `bin/disjuntor.py:800`) |
| `cota.json` | Statusline grava percentual/reset, hora, contexto e session_id; sem os dois números válidos não grava. (`bin/statusline_cota.py:45`, `bin/statusline_cota.py:51`) | `ler_cota` decide por percentual/reset; B4 e painel o chamam. Hora/session_id não participam da validade. (`bin/disjuntor.py:1069`, `bin/disjuntor.py:1111`, `bin/disjuntor.py:1619`) |
| Transcripts/meta de subagent | O leitor atribui a escrita ao harness; implementação desse escritor fora da cópia. (`bin/disjuntor.py:336`, `bin/disjuntor.py:369`) **[não verificado]** | B1 lê meta, tokens, cópias citadas e mtimes; B3 lê tipo, prompt e Handback; painel lê cache e modelo. (`bin/disjuntor.py:415`, `bin/disjuntor.py:449`, `bin/disjuntor.py:546`, `bin/disjuntor.py:742`, `bin/disjuntor.py:1512`, `bin/disjuntor.py:1631`) |

Nas fichas, **C/E/F/M/D** significam lacunas em quem chama / quem espera / falha-limite / medida / quem desliga.

1. **Teto de seis voltas por obra**

   **(a) Sítio:** `decide_b3` é o ponto comum correto, mas hoje mede reprovações finais e PROCEDENTES Codex, não toda abertura de Agent ou chamada de verificador. Construtor não consulta B3. O Stop continua sendo chamador da decisão, embora a v2 o enumere apenas nos itens 6/12. (`bin/disjuntor.py:878`, `bin/disjuntor.py:912`, `bin/disjuntor.py:1454`, `bin/hook_stop_fila1.py:1048`)

   **(b) Estado:** usa B3, transcripts e triagem do mapa; `b3[obra].voltas` e sua semeadura ainda não têm escritor/leitor implementados. O `.id` existente só nasce ao término bem-sucedido, depois da autorização. (`bin/disjuntor.py:891`, `bin/disjuntor.py:936`, `bin/codex_pergunta.sh:198`)

   **(c) Contraexemplo estático:** sete construtores da mesma obra podem passar quando B4 permite: a porta não incrementa nem consulta teto de voltas. Sete chamadas Codex sem PROCEDENTES suficientes também não atingem o B3 atual. Ocorrência real, inclusive a contagem 9 de GRADE-FOLGA-JUIZ: **[não verificado]**. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:912`)

   **(d) Lacunas:** **C:** consulta da decisão versus registro de abertura; **E:** se N reabre para total absoluto ou concede N adicionais; **F:** identidade/idempotência, soma das fontes, chamadas falhadas e reserva de voo; **M:** a semente não pode presumir `.id` imutável; **D:** as CLIs reabre/redesenha não existem no despacho atual. Locks por resposta e sidecar posterior não reservam uma volta entre respostas distintas. (`bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:198`, `bin/disjuntor.py:1699`)

2. **Segundo ERRO-BOBO vira DESENHO**

   **(a) Sítio:** `linhas_libera_b3` reconhece a linha; `decide_b3` a consome. O contrato acerta os sítios, mas o reconhecedor não consulta histórico por tipo. (`bin/disjuntor.py:626`, `bin/disjuntor.py:650`, `bin/disjuntor.py:930`)

   **(b) Estado:** `consumidas` guarda SHA da linha; `liberado` recebe reprovações/triagens anteriores. O tipo aparece no anúncio da trilha, sem acompanhar cada entrada de `consumidas`. (`bin/disjuntor.py:936`, `bin/disjuntor.py:939`, `bin/disjuntor.py:940`)

   **(c) Contraexemplo estático:** ERRO-BOBO A libera; nova reprovação dispara; ERRO-BOBO B, diferente, posterior e fora da fotografia, libera novamente. Não há teste de “segundo por obra” nessa transição. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:930`, `bin/disjuntor.py:932`, `bin/disjuntor.py:938`)

   **(d) Lacunas:** **C:** o reconhecedor sozinho não possui o histórico; **E:** esclarecer DESENHO automático versus liberação exclusiva de Ronald; **F:** migração dos SHAs antigos sem tipo; **M:** painel de liberações em 24 h não equivale ao histórico vitalício da obra; **D:** hoje uma linha válida do Code libera, enquanto a guarda reserva apenas `--rearma-b3`. (`bin/disjuntor.py:626`, `bin/disjuntor.py:939`, `bin/disjuntor.py:1552`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:162`)

3. **Opus final com PROCEDENTE sem commit de cura** [nao verificado]

   **(a) Sítio:** `_procedentes` identifica os PROCEDENTES corretamente, mas não julga cura, cópia ou HEAD. A porta final só pergunta ao B3 atual. (`bin/disjuntor.py:857`, `bin/disjuntor.py:1457`)

   **(b) Estado:** `.id` contém hash completo; a triagem transforma a identidade em `hash12@pid@fim@sha12`. Seu log não preserva caminho da cópia nem vínculo achado→cura. (`bin/codex_pergunta.sh:199`, `bin/disjuntor.py:802`, `bin/disjuntor.py:813`)

   **(c) Contraexemplo estático atual:** um ou dois PROCEDENTES, sem cura, ainda permitem outro final: o limiar Codex é três. O teste atual exige passagem com dois. (`bin/disjuntor.py:77`, `bin/disjuntor.py:912`, `bin/tests/test_disjuntor.sh:1153`) **Contraexemplo do critério contratado:** HEAD diferente pode ser commit alheio ao achado; desigualdade de hash não demonstra cura. Ocorrência real: **[não verificado]**.

   **(d) Lacunas:** **C:** qual cópia é autoridade; **E:** quais PROCEDENTES permanecem pendentes; **F:** posterioridade/ancestralidade e diferença entre mudança e cura; **M:** prova com commit diferente que não cura; **D:** alcance de rearme/DESENHO sobre esse impedimento. Hoje o rearme retira todas as triagens anteriores da contagem corrente. (`bin/disjuntor.py:802`, `bin/disjuntor.py:1018`)

4. **Verificador final sem obra reconhecida**

   **(a) Sítio:** `_obra_de` é o reconhecedor correto; a porta atual usa ausência de obra para pular a negativa. O wrapper já exige `CODEX_OBRA`, mas somente não vazio. (`bin/disjuntor.py:504`, `bin/disjuntor.py:1457`, `bin/codex_pergunta.sh:45`)

   **(b) Estado:** `b3_sem_obra` é preenchido depois, lendo transcripts de verificadores finais; o painel publica total acumulado, sem timestamps para 24 h. (`bin/disjuntor.py:736`, `bin/disjuntor.py:892`, `bin/disjuntor.py:1624`)

   **(c) Contraexemplo estático:** “LEITURA FINAL antes do pouso, sem o nome dela” passa se B4 permitir. Esse prompt existe no teste, que verifica apenas a contagem posterior. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:1457`, `bin/tests/test_disjuntor.sh:427`)

   **(d) Lacunas:** **C:** universo continua `subagent_type=verificador` mais final ou passa a incluir outros tipos/modelos; **E:** reconhecimento do rótulo não prova leitura da obra; **F:** primeiro ID reconhecido pode ser de outra obra; **M:** abertura negada versus contagem posterior e janela 24 h; **D:** aplicação da chave a essa nova negativa. O reconhecedor atual toma a primeira ocorrência válida. (`bin/disjuntor.py:94`, `bin/disjuntor.py:504`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1487`)

5. **B1 com job próprio em fundo e nenhuma escrita por 15 min**

   **(a) Sítio:** `medir_b1` é correto. “PreToolUse Agent/Bash” descreve estreitamente o chamador: o matcher é geral, e B1 age nas ferramentas do agente com `agent_id`, exceto Handback. Stop apenas consulta disparos persistidos. (`.claude/settings.json:64`, `bin/disjuntor.py:1435`, `bin/hook_stop_fila1.py:1054`)

   **(b) Estado:** B1 lê meta/transcript/cópias citadas/mtimes; escreve os campos B1 do mapa. Não interpreta lançamento e conclusão de Bash em fundo. (`bin/disjuntor.py:415`, `bin/disjuntor.py:425`, `bin/disjuntor.py:428`, `bin/disjuntor.py:481`)

   **(c) Contraexemplo estático:** tokens zero causam não-disparo antes de verificar arquivos, mesmo havendo job em fundo. (`bin/disjuntor.py:452`) A sonda das 17 esperas não prova esse caso: não fornece mtimes nem decisões do hook. B1 exclui `cache_read`, mas os retornos informados também têm tokens das categorias que ele soma; portanto “esperou três minutos” não demonstra ausência de tokens para B1. Ocorrência real de liberação/negação: **[não verificado]**. (`bin/disjuntor.py:449`)

   **(d) Lacunas:** **C:** sem próxima ferramenta não há nova medição; **E:** negar ferramenta não encerra o job; **F:** ownership por caminho citado, isenções por tipo e fontes ilegíveis; **M:** job sem tokens, término do job e arquivos escritos por terceiro; **D:** chave hoje basta existir, sem RELATO obrigatório. Meta/cópia ilegível hoje não dispara B1. (`bin/disjuntor.py:1435`, `bin/disjuntor.py:1438`, `bin/disjuntor.py:418`, `bin/disjuntor.py:425`, `bin/disjuntor.py:457`, `bin/disjuntor.py:146`)

6. **Contador fora em duas gerações consecutivas**

   **(a) Sítio:** nenhum mecanismo atual. O contrato nomeia corretamente o futuro produtor `gerar_estado` e leitor Stop, mas o produtor atual só grava Markdown. (`bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1184`)

   **(b) Estado:** sidecar e histórico de gerações não têm proprietário implementado. Os esperados atuais são heterogêneos: prosa de raias/interrompidas, mapa do carimbo do fio e esperado ao fim do programa dos leitores. (`bin/gerar_estado.py:222`, `bin/gerar_estado.py:240`, `bin/fio_estado.py:123`, `bin/leitor_dia_estado.py:46`)

   **(c) Contraexemplo estático:** PAREI ou ESPERANDO com prova podem liberar o Stop sem qualquer consulta a contador fora. Os valores dos “≥4 hoje” não vieram discriminados na sonda. Ocorrência real dessa liberação: **[não verificado]**. (`bin/hook_stop_fila1.py:1226`, `bin/hook_stop_fila1.py:1231`)

   **(d) Lacunas:** **C:** quando uma publicação está concluída e o que ocorre se o gerador falha; **E:** reconhecimento de cada FORA e validade da justificativa; **F:** geração repetida, saltos, sidecar velho, reinício, contador desaparecido e “sem esperado”; **M:** duas gerações distintas, atualização parcial e comparação estruturada; **D:** precedência da chave e da linha de desligamento. Hoje o gerador escreve o destino diretamente e a chave só testa existência. (`bin/gerar_estado.py:376`, `bin/disjuntor.py:146`)

7. **Raias velhas, tar e preservação de commits não pousados** [nao verificado]

   **(a) Sítio:** `raias_estado::ler` mede; `gerar_estado` publica. **Nenhum sítio de arquivamento/remoção** nesses fluxos. O contrato nomeia a publicação, mas não o executor da limpeza. (`bin/raias_estado.py:89`, `bin/raias_estado.py:125`, `bin/gerar_estado.py:215`)

   **(b) Estado:** `raia_agente.sh` escreve declarações em `logs/raias/*.agente`; `raias_estado` lê esses campos e Git; o gerador lê seu resultado. (`bin/raia_agente.sh:54`, `bin/raia_agente.sh:66`, `bin/raias_estado.py:62`, `bin/gerar_estado.py:216`)

   **(c) Contraexemplos do dado:** `tmp-ui`, com dois patches pendentes e sem worktree, fica fora do universo Git enumerado; entradas B sob `.claude/worktrees` são excluídas; `wt-comboio`, `ahead=1/plus=0`, pode ser contado velho apesar de patch já pousado. Esses registros são da sonda, sem reprodução no HEAD: **[não verificado]**. O código exclui `.claude/worktrees` e mede `rev-list`, não equivalência por `cherry`. (`bin/raias_estado.py:85`, `bin/raias_estado.py:96`, `bin/raias_estado.py:101`, `bin/raias_estado.py:123`)

   **(d) Lacunas:** **C:** executor e gatilho; **E:** destino da lista ao Ronald; **F:** definição de velha, detached, A* suja, C em uso e sucesso verificável do tar; **M:** universo completo versus contador atual; **D:** chave/reversão do instrumento não restaura worktree já removida. A medição atual define velha por último commit à frente há mais de seis horas. (`bin/raias_estado.py:39`, `bin/raias_estado.py:103`, `bin/raias_estado.py:123`) Política de restauração: **[não verificado]**.

8. **Modelo explícito e papel do Agent**

   **(a) Sítio:** nenhum gate atual; a porta Agent lê tipo/prompt e consulta B3/B4. `model` e `description` não são critérios de autorização ali. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1460`)

   **(b) Estado:** o painel lê modelo do primeiro `api-request` e tipo da meta, depois do lançamento; conta agentes por modelo/tipo. Não calcula custo por modelo em 24 h. (`bin/disjuntor.py:1631`, `bin/disjuntor.py:1643`, `bin/disjuntor.py:1647`)

   **(c) Contraexemplo estático:** Agent sem `model`, ou opus descrito como contagem, passa quando B1/B3/B4 permitem. Os fixtures de verificador já omitem `model`. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:1451`, `bin/tests/test_disjuntor.sh:322`)

   **(d) Lacunas:** **C:** campo autoritativo e sintaxe dos papéis; **E:** classificação de espera de suite; **F:** aliases, modelos desconhecidos e descrições com múltiplos papéis; **M:** custo precisa de tokens/preço e janela, além da contagem existente; **D:** alcance da chave sobre a nova porta. O custo aceito “mal rotulado custa uma volta” exige decisão conjunta com item 1: negar antes de registrar custa zero; registrar sem identidade pode cobrar repetição. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1631`) Regra dessa cobrança: **[não verificado]**.

9. **Opus final somente com suite inteira verde da árvore** [nao verificado]

   **(a) Sítio:** nenhum gate na abertura do Agent. A regra textual existe; o ramo não lê certificado de suite. (`CLAUDE.md:557`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`)

   **(b) Estado:** há prova adjacente: `regua.sh` escreve `.regua_stamp` com árvore/status/hora; pre-push lê. O contrato não estabelece se esse é o certificado do item 9, nem seu vínculo com a cópia da obra. (`bin/regua.sh:196`, `bin/pre-push.sh:53`, `bin/pre-push.sh:66`) Vínculo contratado: **[não verificado]**.

   **(c) Contraexemplo estático:** final de obra livre pode abrir sem suite registrada. Além disso, o atalho da própria régua compara somente impressão de Python/HTML do app e status: mudança só em `bin/` pode retornar “JA VERDE” sem executar os selos novos. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:1457`, `bin/regua.sh:33`, `bin/regua.sh:86`, `bin/regua.sh:93`, `bin/regua.sh:138`)

   **(d) Lacunas:** **C:** produtor autorizado do certificado; **E:** definição de inteira; **F:** hash de commit versus árvore, sujeira e alteração durante execução; **M:** prova parcial nunca valer como inteira — a régua já distingue `--only`; **D:** exceção/reabertura e chave. Sem produtor acordado, o gate negaria todo final sem certificado. (`bin/arvore_hash.sh:4`, `bin/regua.sh:181`, `bin/disjuntor.py:1487`) Comportamento do novo gate: **[não verificado]**.

10. **Instrumento commitado exige contrato no push** [nao verificado]

    **(a) Sítio:** nenhum gate atual; `bin/pre-push.sh` é o encaixe correto. O instalador gera wrapper em `.git/hooks/pre-push`; instalação efetiva no host: **[não verificado]**. (`bin/pre-push.sh:33`, `bin/pre-push.sh:200`, `bin/hooks.sh:26`, `bin/hooks.sh:51`)

    **(b) Estado:** falta escritor/leitor do vínculo instrumento→obra→contrato. O pre-push já resolve `PUSH_SHA`, que pode diferir do HEAD; conferir contrato apenas no disco certificaria outra árvore. (`bin/pre-push.sh:33`, `bin/pre-push.sh:37`, `bin/pre-push.sh:39`)

    **(c) Contraexemplo estático:** instrumento sem contrato pode atingir “push liberado” se os demais checks passam. `SKIP_TESTS=1` libera antes de todos os gates. Push concreto dessa classe: **[não verificado]**. (`bin/pre-push.sh:5`, `bin/pre-push.sh:200`)

    **(d) Lacunas:** **C:** classificação de instrumento/hook/cron/selo e alcance do push; **E:** relação entre múltiplas obras e contratos substituídos; **F:** existência versus contrato efetivamente commitado — `logs/` é ignorado; **M:** selo obrigatório em cada push, fora do arsenal pulável; **D:** relação de Ronald/chave/reversão com `SKIP_TESTS`. (`.gitignore:53`, `bin/pre-push.sh:14`, `bin/pre-push.sh:66`, `bin/pre-push.sh:159`, `bin/pre-push.sh:5`)

11. **Sétimo agente vivo negado**

    **(a) Sítio:** nenhum teto Agent atual. `agentes_sem_entrega` reconhece agentes assíncronos lançados sem notificação final; a porta Agent não usa esse leitor. (`bin/disjuntor.py:366`, `bin/disjuntor.py:376`, `bin/disjuntor.py:1451`)

    **(b) Estado:** o helper lê o transcript principal e deduplica IDs; não reserva slots nem persiste registro de todos os vivos. O teto de slots do wrapper é de processos Codex, com `CODEX_TETO` configurável, outra medida. (`bin/disjuntor.py:376`, `bin/disjuntor.py:379`, `bin/codex_pergunta.sh:47`, `bin/codex_pergunta.sh:143`)

    **(c) Contraexemplo estático:** sete Agents podem passar se os outros disjuntores permitirem; não há consulta à quantidade de vivos nessa porta. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1475`)

    **(d) Lacunas:** **C:** sessão, host ou árvore de agentes; **E:** fila/recusa e liberação da vaga; **F:** síncronos, filhos, falha de lançamento e notificação perdida; **M:** disputa simultânea pela sexta vag

[truncado em 20000 de 24510 caracteres]

## 01/11 00:14 · sessao 0398ef9dc32b · - · - · origem=Ronald (tmux)

Leitura concluída só no código. Os casos abaixo são contraexemplos deduzidos do HEAD; ocorrência em produção, salvo o que consta expressamente no dado raspado, é **[não verificado]**.

**Chamadores e fronteira comum**

- O settings aponta PreToolUse sem filtro para `disjuntor.py pretool` e Stop para `hook_stop_fila1.py`; o contrato acerta esses nomes. `.claude/settings.json:3`, `.claude/settings.json:9`, `.claude/settings.json:62`, `.claude/settings.json:68`.
- PreToolUse só consulta B3 no Bash para wrapper identificado como `papel=verificador` e obra presente; no Agent, só para `subagent_type=verificador`, prompt final e obra reconhecida. O Agent comum/construtor não consulta nem registra volta. `bin/disjuntor.py:1440`, `bin/disjuntor.py:1443`, `bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`.
- A porta do wrapper existe depois de adquirir slot e trava da resposta; chama `--b3-parada` para papel verificador. `bin/codex_pergunta.sh:137`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:164`.
- A chave hoje basta por existência, inclusive symlink; PreToolUse a consulta, a porta `b3_parada` não consulta. Nenhum desses dois pontos exige linha no RELATO. `bin/disjuntor.py:144`, `bin/disjuntor.py:1486`, `bin/disjuntor.py:1036`, `bin/disjuntor.py:1042`.

**Item 1 — teto seis/voltas**

(a) O local de encaixe é `decide_b3`, mas o mecanismo atual conta reprovações finais opus e voltas Codex triadas PROCEDENTE; não conta aberturas Agent nem toda chamada Codex. O contrato nomeia o local certo como destino, não como capacidade já existente. `bin/disjuntor.py:878`, `bin/disjuntor.py:896`, `bin/disjuntor.py:902`, `bin/disjuntor.py:910`.

(b) Escritor atual: `_com_estado` grava o JSON inteiro sob flock; `decide_b3` modifica `b3`, `reprovados`, `codex`, `disparado`, `liberado`, `consumidas`, `topo_no_disparo`, `disparado_em`, `b3_sem_obra`; `rearma_b3` modifica `rearmado` e zera listas correntes. Não há `b3[obra].voltas`. `bin/disjuntor.py:237`, `bin/disjuntor.py:271`, `bin/disjuntor.py:891`, `bin/disjuntor.py:936`, `bin/disjuntor.py:939`, `bin/disjuntor.py:1018`.

(c) Sete Agents construtores com obra reconhecida e cota disponível passam: esse ramo não consulta B3 nem incrementa volta. O mesmo vale para sete chamadas Codex sem triagem PROCEDENTE suficiente. Ocorrência real com sete disparos: **[não verificado]**. O teste atual exige que três chamadas não triadas não parem. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1460`, `bin/tests/test_disjuntor.sh:1144`.

(d) Faltam identidade idempotente da abertura, unidade exata de volta entre fontes, reserva entre autorização e execução, significado de N adicional/absoluto na reabertura e alcance da semeadura. A falta de reserva é demonstrável: locks do wrapper são por resposta e sidecar só nasce ao fim rc0; respostas distintas podem autorizar antes que qualquer sidecar apareça. `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:198`. Há CLI atual só de triagem/consulta/rearme, sem reabre/redesenha. `bin/disjuntor.py:1699`.

**Item 2 — segundo ERRO-BOBO**

(a) O sítio certo é `linhas_libera_b3` + consumo em `decide_b3`, mas hoje qualquer ERRO-BOBO novo com motivo alfanumérico pode liberar. `bin/disjuntor.py:626`, `bin/disjuntor.py:650`, `bin/disjuntor.py:930`.

(b) `consumidas` contém apenas hashes das linhas, sem tipo; a linha aceita movimenta todas as reprovações e triagens anteriores para `liberado`. `bin/disjuntor.py:932`, `bin/disjuntor.py:936`, `bin/disjuntor.py:939`.

(c) Primeiro ERRO-BOBO A libera; nova reprovação dispara; segundo ERRO-BOBO B, diferente de A, fora da fotografia e posterior ao disparo, também libera. O código não conta liberações por tipo. Produção: **[não verificado]**. `bin/disjuntor.py:930`, `bin/disjuntor.py:932`, `bin/disjuntor.py:934`, `bin/disjuntor.py:940`.

(d) A migração do histórico só com sha para histórico com tipo não está definida; a trilha possui anúncios com tipo que o painel já conta em janela 24h, mas isso difere de histórico vitalício da obra. `bin/disjuntor.py:939`, `bin/disjuntor.py:1552`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:1574`. Também permanece ambíguo se DESENHO válido libera automaticamente ou exige Ronald: o contrato mantém ambas as formulações **[não verificado]**.

**Item 3 — PROCEDENTE sem cura**

(a) `_procedentes` é fonte certa para identificar procedentes, mas não certifica cura; apenas seleciona IDs das voltas cujo último veredito é procedente. Não consulta git/HEAD/cópia. `bin/disjuntor.py:857`.

(b) Wrapper escreve `.id` com obra/papel/hash/pid/fim/sha da resposta; triagem lê, valida sha da resposta, transforma em identificador `hash12@pid@fim@sha12` e grava `hora,obra,volta,veredito` no log. O caminho da cópia não é preservado nesse log. `bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:199`, `bin/disjuntor.py:795`, `bin/disjuntor.py:800`, `bin/disjuntor.py:802`, `bin/disjuntor.py:812`.

(c) Um ou dois PROCEDENTES sem cura ainda permitem opus final: limiar atual Codex é três, independentemente de HEAD posterior. O teste explicitamente pede passagem com dois. Produção: **[não verificado]**. `bin/disjuntor.py:77`, `bin/disjuntor.py:912`, `bin/tests/test_disjuntor.sh:1153`.

(d) O contrato precisa distinguir “HEAD diferente” de “commit de cura posterior”: commit alheio ao achado muda HEAD sem curar; comparação de desigualdade não prova ancestralidade, autoria nem correção. Isso é limite lógico da medida proposta, ocorrência real **[não verificado]**. O identificador atual ainda trunca hash a 12 e perde caminho; resolver identidade/cópia antecede esse item. `bin/disjuntor.py:802`, `bin/disjuntor.py:813`.

**Item 4 — verificador final sem obra**

(a) `_obra_de` é o reconhecedor certo, mas hoje ausência pula a negativa. `bin/disjuntor.py:504`, `bin/disjuntor.py:1455`, `bin/disjuntor.py:1457`.

(b) Após o agente existir, `_reprovacoes_finais` lê meta `agentType=verificador`, prompt e primeira linha do SubagentHandback; `decide_b3` acumula IDs sem obra em `b3_sem_obra`. O painel retorna comprimento acumulado, sem timestamps/janela 24h. `bin/disjuntor.py:546`, `bin/disjuntor.py:561`, `bin/disjuntor.py:736`, `bin/disjuntor.py:892`, `bin/disjuntor.py:1624`.

(c) Agent verificador com “LEITURA FINAL antes do pouso, sem o nome dela” passa a porta quando B4 não nega; depois pode entrar no contador. O fixture atual contém justamente esse prompt e só verifica contagem posterior. Produção: **[não verificado]**. `bin/disjuntor.py:1457`, `bin/tests/test_disjuntor.sh:427`.

(d) Escopo “verificador final” precisa permanecer ligado ao tipo/final ou declarar novo universo; modelo opus não é critério atual. O contrato aceita rótulo como medida, não prova leitura efetiva; prompt com ID formalmente válido mas de obra errada satisfaz o parser. `bin/disjuntor.py:1454`, `bin/disjuntor.py:504`.

**Item 8 — modelo/papel**

(a) **Nenhum mecanismo atual de negativa por model/description/papel.** O ramo Agent lê tipo/prompt e B4; a contagem do modelo é posterior, por meta + primeiro `api-request`. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1631`.

(b) Transcript/meta são escritos pelo harness segundo o contrato local do leitor; o escritor executável do harness não está nesta cópia **[não verificado]**. O leitor atual usa `params.model` do primeiro api-request e `agentType` da meta. `bin/disjuntor.py:1632`, `bin/disjuntor.py:1641`.

(c) Agent sem `model`, ou opus descrito como contagem, passa se B1/B3/B4 não negarem; fixtures atuais de Agent verificador nem enviam `model`. Produção: **[não verificado]**. `bin/tests/test_disjuntor.sh:322`, `bin/disjuntor.py:1451`.

(d) Lacunas: sintaxe/caixa/acentos dos papéis; campo description versus prompt; vocabulário aliases do modelo; ordem com item1; modelos desconhecidos; definição de “espera de suite”. “Agente mal rotulado custa uma volta” exige contar antes de negar; negar antes custa zero; sem identidade da tentativa, reapresentação pode contar repetidamente. `bin/disjuntor.py:1451`; detalhes de política **[não verificado]**.

**Item 9 — suite inteira verde por árvore/hash** [nao verificado]

(a) **Nenhum gate atual na abertura do opus.** Existe regra textual suite antes do opus, sem leitura de selo no ramo Agent. `CLAUDE.md:557`, `bin/disjuntor.py:1451`.

(b) Writer/reader/schema/local do certificado pretendido não estão nomeados no contrato nem nesse ramo **[não verificado]**; ramo atual só lê B3 e B4. `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`.

(c) Opus final de obra livre passa sem qualquer suite registrada. Produção: **[não verificado]**. `bin/disjuntor.py:1457`, `bin/disjuntor.py:1475`.

(d) Falta definir “inteira”, origem autêntica do verde, suites necessárias, validade temporal, hash commit versus árvore/estado sujo e relação com cópia citada. Identidade do item3/4 e escritor do selo antecedem gate; gate sem produtor causaria negação permanente **[não verificado]**. A regra textual pede suite inteira da árvore, não fornece formato de prova. `CLAUDE.md:557`. [nao verificado]

**Item 11 — sétimo agente vivo**

(a) **Nenhum teto de seis na porta Agent.** Existe leitor de agentes de fundo sem entrega, mas a porta não o usa. `bin/disjuntor.py:366`, `bin/disjuntor.py:1451`.

(b) `agentes_sem_entrega` lê transcript principal, reconhece strings de lançamento assíncrono e notificações com task-id/status; não é registro de todos agentes vivos nem usa processos. `bin/disjuntor.py:367`, `bin/disjuntor.py:376`.

(c) Sete Agents novos passam enquanto B4/B3/B1 permitem; não há contagem de vivos nesse caminho. Produção: **[não verificado]**. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1475`.

(d) Falta universo (sessão/host; fundo/síncrono; agentes filhos), tratamento de transcript ausente/notification perdida, slot/reserva/liberação e CLI/chave de desligar. O helper só vê o resultado do lançamento depois que ele foi registrado, logo não resolve corrida entre autorizações simultâneas. `bin/disjuntor.py:369`, `bin/disjuntor.py:376`.

**Matriz de escritores/leitores pedidos**

- `disjuntor_estado.json`: escrita física única `_com_estado`, JSON temporário + replace sob flock; leitura `_com_estado`, `ler_estado`; consumidores B1/B3/B4 e painel. Estado ausente é `{}`, diferente de ilegível que levanta — isso conflita com “estado/sidecar ilegível=NAO_SEI” apenas se o contrato pretender incluir ausência de estado já conhecido. `bin/disjuntor.py:237`, `bin/disjuntor.py:262`, `bin/disjuntor.py:291`.
- `triagem.log`: escritor `--triagem` → `_triagem_sob_trava`; leitor `_triagens` sob trava do estado, consumido por decide/rearme. Perda/truncamento detectados por contador de linhas e presença das voltas conhecidas. `bin/disjuntor.py:804`, `bin/disjuntor.py:812`, `bin/disjuntor.py:823`, `bin/disjuntor.py:1007`.
- `.id`: wrapper remove anterior antes da chamada e grava novo só rc0 com resposta desta chamada; disjuntor lê para triagem. Portanto não é histórico completo e “durável entre sessões” não significa imutável: reutilizar resposta remove o anterior. `bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:198`, `bin/disjuntor.py:753`.
- Transcripts subagent: escritor harness não disponível **[não verificado]**; leitores locais `_meta`, B1, `_veredito_e_obra`, `_reprovacoes_finais`, cache/modelos do painel. Linhas JSON inválidas são puladas, meta ilegível no leitor de reprovações vira `{}` e agente pode deixar de ser classificado como verificador, sem NAO_SEI nesse percurso. `bin/disjuntor.py:342`, `bin/disjuntor.py:353`, `bin/disjuntor.py:742`, `bin/disjuntor.py:1513`, `bin/disjuntor.py:1631`.

**Ordem que o código exige**

- Antes dos gates 1/3/9/11: identidade da obra/cópia, esquema durável, escritores, idempotência e reserva; os pontos atuais são consulta prévia e sidecar posterior. `bin/disjuntor.py:1451`, `bin/codex_pergunta.sh:198`.
- Item8 e item1 precisam pousar juntos ou declarar qual negativa consome volta; o custo declarado de rótulo errado não resulta automaticamente da ordem proposta **[não verificado]**.
- O tratamento comum NAO_SEI precisa preceder extensão dos ramos: exceções hoje falham fechado só no ramo B3 final/verificador; Agent construtor falha aberto. `bin/disjuntor.py:1410`, `bin/disjuntor.py:1496`.
- Desligamento deve alcançar wrapper antes de depender dele para novos gates; hoje chave libera PreToolUse, mas wrapper continua recusando obra parada. `bin/disjuntor.py:1487`, `bin/codex_pergunta.sh:165`.
- O selo de testes atual cobre B3 anterior (três PROCEDENTES, consumo por sha, contagem posterior sem obra), não prova os novos contratos; suite não foi executada nesta leitura. `bin/tests/test_disjuntor.sh:1125`, `bin/tests/test_disjuntor.sh:427`, `bin/tests/test_disjuntor.sh:1468`.

## 01/11 00:14 · sessao dc80f0305892 · - · - · origem=Ronald (tmux)

Leitura sem executar testes ou geradores. As constatações abaixo são da cópia; a ocorrência em produção de entradas hipotéticas permanece [não verificado]. [nao verificado]

**Item 5 — B1, job próprio em fundo, sem escrita por 15 min**

- **Sítio atual e nome:** `medir_b1` é o medidor (`bin/disjuntor.py:413`), chamado por `b1_do_agente` (`bin/disjuntor.py:480`), somente quando o próprio agente passa no PreToolUse (`bin/disjuntor.py:1435`). O contrato acerta o B1, mas “PreToolUse Agent/Bash” é estreito: o matcher cobre todas as ferramentas (`.claude/settings.json:64`) e o B1 roda para `agent_id`, exceto `SubagentHandback` (`bin/disjuntor.py:1435`). Stop não mede B1: lê IDs já disparados e ainda sem entrega (`bin/hook_stop_fila1.py:1053`).
- **Escritor/leitor:** o harness é indicado como escritor dos transcripts e notificações (`bin/disjuntor.py:336`, `bin/disjuntor.py:369`); sua implementação não está nesta cópia [não verificado]. `medir_b1` lê meta/transcript, caminhos citados e mtimes (`bin/disjuntor.py:415`, `bin/disjuntor.py:421`, `bin/disjuntor.py:425`, `bin/disjuntor.py:454`). `b1_do_agente` escreve `b1[id].{checado,disparado,motivo}` via `_com_estado` (`bin/disjuntor.py:481`, `bin/disjuntor.py:486`); `b1_disparados` lê para Stop (`bin/disjuntor.py:489`).
- **Contraexemplo demonstrável:** tokens zero fazem não-disparo antes de qualquer leitura de arquivos (`bin/disjuntor.py:452`), mesmo que exista Bash em fundo: o medidor não interpreta jobs (`bin/disjuntor.py:428`). Meta/transcript/cópia ilegíveis também não disparam (`bin/disjuntor.py:417`, `bin/disjuntor.py:424`, `bin/disjuntor.py:457`), contrariando o NAO_SEI genérico do v2.
- **Limite do caso real:** a sonda informa 17 esperas, gaps máximos de 184,5 s e tokens nas mensagens; não informa mtimes nem as chamadas de hook. Não prova uma liberação ou negação concreta do B1 [não verificado]. B1 soma input/output/cache_creation, **não** cache_read (`bin/disjuntor.py:449`); inferir disparo só dos ~2,49 Mtok/h de cache_read seria incorreto (`bin/disjuntor.py:449`).
- **Lacunas nos cinco campos:** quem chama precisa incluir todas as ferramentas e reconhecer que não há relógio autônomo (`bin/disjuntor.py:1435`); quem espera precisa distinguir impedir próxima ferramenta de matar job (`bin/disjuntor.py:1438`); falha/limite precisa resolver isenções `SO_LEITURA`, ausência de cópia e ownership por caminho citado (`bin/disjuntor.py:91`, `bin/disjuntor.py:425`); medida precisa incluir jobs sem tokens e erro de leitura (`bin/disjuntor.py:452`); desligar hoje só testa existência da chave, sem exigir RELATO (`bin/disjuntor.py:146`).

**Item 6 — contadores fora em duas gerações**

- **Sítio atual:** nenhum mecanismo de sidecar/gerações no gerador ou Stop; `gerar_estado` escreve apenas seu `dst` Markdown (`bin/gerar_estado.py:376`), e Stop extrai apenas `transcript_path` do payload (`bin/hook_stop_fila1.py:1187`). O contrato nomeia corretamente o futuro produtor/leitor, mas não descreve comportamento já existente (`bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1034`).
- **Esperado atual heterogêneo:** raias e interrompidas o expressam em prosa (`bin/gerar_estado.py:222`, `bin/gerar_estado.py:240`); fio compara valores contra mapa vindo do carimbo (`bin/fio_estado.py:123`); leitores do dia têm `esperado_no_fim` (`app/core/medida_leitor_do_dia.py:409`, `bin/leitor_dia_estado.py:46`); placar usa `meta`, inclusive textual (`bin/gerar_estado.py:97`, `bin/gerar_estado.py:104`).
- **Escritor/leitor:** atualmente não há escritor/leitor de `estado_contadores.json` nos sítios contratados (`bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1034`). `_com_estado` é o ponto existente adequado para a história compartilhada: lê/muta/grava sob flock e substituição atômica (`bin/disjuntor.py:237`, `bin/disjuntor.py:253`, `bin/disjuntor.py:268`, `bin/disjuntor.py:274`).
- **Contraexemplo demonstrável:** PAREI legítimo encerra antes de qualquer cobrança de contadores (`bin/hook_stop_fila1.py:1226`); ESPERANDO com prova também encerra (`bin/hook_stop_fila1.py:1231`). Contadores fora duas vezes não alteram esses ramos porque não são lidos (`bin/hook_stop_fila1.py:1184`). Valores reais dos “≥4 fora hoje” não foram fornecidos [não verificado].
- **Lacunas:** chamador precisa definir publicação concluída e falha antes de publicar; esperado/valor precisam definir comparação e “sem esperado”; duas gerações precisa definir reinício, sidecar velho, saltos, geração repetida e mudança do conjunto; medida precisa demonstrar histórico e publicação parcial; desligar não possui exigência atual de RELATO (`bin/disjuntor.py:146`). Esses casos não têm tratamento do novo sidecar no código atual (`bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1034`).

**Item 12 — cota desconhecida por mais de um Stop**

- **Sítio atual e nome:** `ler_cota` existe (`bin/disjuntor.py:1064`), B4 Stop existe (`bin/disjuntor.py:1202`), mas contar Stops de desconhecido e cobrar FORA não existe nessa transição: cota desconhecida não produz `escada`, e `b4_stop` apaga `b4_para` quando não há escada (`bin/disjuntor.py:1111`, `bin/disjuntor.py:1208`).
- **Escritor:** `statusline_cota.py` está configurado (`.claude/settings.json:76`); valida os dois números, grava `{seven_day_pct,seven_day_resets_at,hora,contexto_tokens,contexto_pct,session_id}` e troca atomicamente (`bin/statusline_cota.py:45`, `bin/statusline_cota.py:51`, `bin/statusline_cota.py:54`). Sem ambos, não escreve (`bin/statusline_cota.py:47`); erro é silenciado (`bin/statusline_cota.py:55`). Não há escritor manual de “escada N” aqui: só reconhece prefixo `escada`, sem extrair N (`bin/disjuntor.py:87`, `bin/disjuntor.py:1115`).
- **Leitor:** `ler_cota` lê percentual/reset e ignora hora/session_id (`bin/disjuntor.py:1069`); `escada` e `b4_decide` o usam (`bin/disjuntor.py:1111`, `bin/disjuntor.py:1126`); painel também (`bin/disjuntor.py:1619`). Transição B4 escreve `b4_para.desde` via `_com_estado` (`bin/disjuntor.py:1215`, `bin/disjuntor.py:1222`).
- **Contraexemplo:** cota ausente/ilegível retorna None (`bin/disjuntor.py:1075`), e Agent passa B4 sem escada (`bin/disjuntor.py:1127`). A sonda relata cota ausente, mas não traz uma chamada e sua decisão: a ocorrência concreta de liberação é [não verificado]. Causa do payload ausente e execução efetiva da statusline também são [não verificado].
- **Lacunas:** definir N/unidade/intervalo, hora/validade e precedência entre fonte manual e statusline; definir se o Stop seguinte à linha FORA reinicia sequência; definir persistência entre sessões; separar desconhecido de escada real; resolver Stop excepcional que hoje falha aberto (`bin/hook_stop_fila1.py:1305`). Quem desliga hoje é existência de chave (`bin/hook_stop_fila1.py:1039`); RELATO obrigatório é novo (`bin/disjuntor.py:146`).

**Dependências e medidas que afetam a consolidação**

- Antes de item 6 cobrar: contratos estruturados dos medidores → sidecar e geração concluída → histórico sob trava → decisão Stop. As fontes atuais não compartilham esquema (`bin/gerar_estado.py:222`, `bin/fio_estado.py:123`, `app/core/medida_leitor_do_dia.py:409`).
- Cobranças de 6/12 têm de preceder **todos** os atalhos que liberam; há B4, orçamento, PAREI, ESPERANDO, teto e B2 (`bin/hook_stop_fila1.py:1207`, `bin/hook_stop_fila1.py:1218`, `bin/hook_stop_fila1.py:1226`, `bin/hook_stop_fila1.py:1231`, `bin/hook_stop_fila1.py:1293`, `bin/hook_stop_fila1.py:1295`). Inserir só nos “recados” finais deixa os atalhos passarem (`bin/hook_stop_fila1.py:1300`).
- Exceções do disjuntor no Stop retornam None (`bin/hook_stop_fila1.py:1059`); exceção geral libera (`bin/hook_stop_fila1.py:1305`). Isso precisa ser conciliado com “nunca verde calado” antes da cobrança (`bin/hook_stop_fila1.py:1305`).
- `gerar_estado` é chamado depois de `relato_nomes.sh`, cuja falha encerra o publicador (`bin/relato.sh:32`, `bin/relato.sh:38`). Sidecar pode ficar velho se essa dependência falhar; ocorrência real [não verificado].
- “Custo por modelo 24 h” não é a medida atual: `_agentes_24h` conta agentes por modelo/tipo, usando mtime do meta (`bin/disjuntor.py:1631`, `bin/disjuntor.py:1637`, `bin/disjuntor.py:1647`); cache é soma de cache_read por **dia civil**, não custo nem janela móvel (`bin/disjuntor.py:1512`, `bin/disjuntor.py:1536`, `bin/disjuntor.py:1543`).
- As suítes atuais têm casos B1/B4 e escada-palavra (`bin/tests/test_disjuntor.sh:1468`); `escada_palavra` explicitamente encerra escada com a mensagem humana seguinte (`bin/tests/test_disjuntor.sh:500`). Resultado de execução atual dos 12 mutantes prometidos é [não verificado].

## 01/11 00:14 · sessao 63ce410ff0b6 · - · - · origem=Ronald (tmux)

**Item 7 — raias velhas**

- **Sítio:** `bin/raias_estado.py::ler`; contrato aponta o ESTADO corretamente como publicador, mas não nomeia chamador/escritor de limpeza. O gerador importa o medidor e publica suas linhas; o medidor somente consulta Git/declarações e retorna dados, sem arquivar ou remover worktrees. `bin/gerar_estado.py:215`, `bin/gerar_estado.py:222`, `bin/raias_estado.py:89`, `bin/raias_estado.py:125`.
- **Escritor/leitor:** `bin/raia_agente.sh` grava `logs/raias/<raia>.agente` com `RAIA`, `OBRA`, `QUEM`, `DESDE`, `FALTA`; atualiza `FALTA`, registra histórico e remove a declaração no fechamento. `bin/raia_agente.sh:54`, `bin/raia_agente.sh:58`, `bin/raia_agente.sh:66`, `bin/raia_agente.sh:76`, `bin/raia_agente.sh:77`. O único leitor declarado é `raias_estado`, que lê os pares `chave=valor`; `gerar_estado` lê o resultado. `bin/raia_agente.sh:10`, `bin/raias_estado.py:62`, `bin/gerar_estado.py:216`.
- **Universo incorreto para o dado raspado:** o medidor exclui `.claude/worktrees`, só enumera worktrees e declarações, portanto não alcança branches sem worktree; conta `rev-list main..HEAD`, sem avaliar equivalência de patches por `git cherry`. `bin/raias_estado.py:85`, `bin/raias_estado.py:93`, `bin/raias_estado.py:96`, `bin/raias_estado.py:101`. No dado fornecido, `tmp-ui` tem dois patches não pousados sem worktree; existem entradas B sob `.claude/worktrees`; `wt-comboio` tem `ahead=1`, `plus=0`, idade 451,8 h. As duas primeiras classes escapam e a terceira é contada velha pelo critério atual, embora não tenha patch pendente; ocorrência fornecida, sem arquivo disponível no HEAD [não verificado]. A regra que marca velha usa apenas idade do último commit à frente, sem descontar patch já pousado. `bin/raias_estado.py:103`, `bin/raias_estado.py:123`.
- **Outro caso estático:** falha de Git retorna `''`, que vira contagem zero; nenhuma declaração e nenhum resultado Git permitem publicar “nenhuma raia viva”. `bin/raias_estado.py:42`, `bin/raias_estado.py:48`, `bin/raias_estado.py:101`, `bin/gerar_estado.py:229`. Ocorrência concreta dessa falha [não verificado].
- **Lacunas:** falta definir quem chama/espera a limpeza; universo inclui branch sem worktree, detached e `.claude/worktrees`? “velha” continua sendo último commit >6h, nascimento, inatividade ou classe A/B/C da sonda? O mecanismo atual usa >6h do último commit à frente e “agente vivo” significa somente declaração não baixada. `bin/raias_estado.py:20`, `bin/raias_estado.py:39`, `bin/raias_estado.py:103`, `bin/raias_estado.py:123`. Tar sem verificação de sucesso/restauração, preservação de sujeira/A*, exclusão de C, destino da lista B e comprovação de “motivo” continuam sem sítio implementado [não verificado]. A chave do disjuntor não é consultada por esse medidor. `bin/raias_estado.py:89`, `bin/raias_estado.py:135`.
- **Medida existente não cobre limpeza:** o teste de raias verifica publicação, diferença com/sem declaração e unicidade de acesso a `logs/raias`; seu caso real sintético é uma única worktree um commit à frente. Não prova tar, remoção segura, branches sem worktree ou equivalência de patches. `bin/tests/test_raia_declarada_no_estado.sh:5`, `bin/tests/test_raia_declarada_no_estado.sh:38`, `bin/tests/test_raia_declarada_no_estado.sh:53`, `bin/tests/test_raia_declarada_no_estado.sh:68`.

**Item 10 — contrato exigido no push** [nao verificado]

- **Sítio:** nenhum gate de contrato implementado em `bin/pre-push.sh`; o arquivo faz gates específicos, resolve `PUSH_SHA`, testa a árvore e termina liberando. O nome do ponto de encaixe está correto, a regra é nova. `bin/pre-push.sh:10`, `bin/pre-push.sh:19`, `bin/pre-push.sh:33`, `bin/pre-push.sh:38`, `bin/pre-push.sh:198`, `bin/pre-push.sh:200`.
- **Chamador efetivo:** `hooks.sh install` escreve `.git/hooks/pre-push` como wrapper delegando para `$(git rev-parse --show-toplevel)/bin/pre-push.sh`; `hooks.sh check` verifica delegação. Instalação real no host desta sonda [não verificado]. `bin/hooks.sh:26`, `bin/hooks.sh:51`, `bin/hooks.sh:56`.
- **Leitores/escritores adjacentes:** a régua escreve `.regua_stamp` com árvore/status/hora; o pre-push lê esse arquivo e pode pular o arsenal. `bin/regua.sh:196`, `bin/regua.sh:198`, `bin/regua.sh:199`, `bin/pre-push.sh:53`, `bin/pre-push.sh:66`, `bin/pre-push.sh:70`. Nenhum escritor/leitor de contrato exigido está estabelecido por esses sítios [não verificado].
- **Contraexemplo estático:** um push só de instrumento pode chegar a `OK — push liberado` sem leitura de contrato; além disso, `SKIP_TESTS=1` retorna sucesso antes de todos os gates. Um push concreto dessa classe na produção [não verificado]. `bin/pre-push.sh:5`, `bin/pre-push.sh:200`.
- **Lacunas nos cinco campos:** falta vínculo determinístico arquivo/commit→obra→contrato, universo de mudanças de “hook/cron/selo de host”, regra para vários instrumentos/obras no mesmo push, qual contrato substitui qual e se deve estar no mesmo commit, no alcance do push ou apenas na árvore. O arquivo já distingue `PUSH_SHA` de HEAD; contrato conferido só no disco pode certificar outra árvore. `bin/pre-push.sh:33`, `bin/pre-push.sh:37`, `bin/pre-push.sh:39`. O destino pedido fica sob `logs/`, ignorado pelo Git; `.claude/` também é ignorada. ` .gitignore:53`, `.gitignore:66`. Quem desliga especificamente o gate e como interage com `SKIP_TESTS` não foi definido; a linha atual libera por variável de ambiente. `bin/pre-push.sh:5`.

**Medida, documentação e dependências**

- Os testes de host são chamados pela régua, antes da suite, no laço `bin/tests/test_*.sh`; o próprio pre-push registra que esse laço não roda ali e pode haver régua “JÁ VERDE”. Logo o gate 10 precisa estar na parte incondicional do push se pretende julgar cada push; colocar apenas um selo no laço da régua não cumpre essa função. `bin/regua.sh:138`, `bin/regua.sh:140`, `bin/pre-push.sh:14`, `bin/pre-push.sh:15`, `bin/pre-push.sh:66`.
- O teste atual do disjuntor já tem mutantes com controle da cópia íntegra e falha se a costura removida não for detectada. Isso fornece estrutura de medida, sem provar os 12 RED novos nem resultado vermelho/verde desta obra. `bin/tests/test_disjuntor.sh:1474`, `bin/tests/test_disjuntor.sh:1481`, `bin/tests/test_disjuntor.sh:1489`. Execução dos 12 novos [não verificado].
- `test_hook_parei_so_lei.sh` cobre PAREI técnico/lei, `!` e item só registrado; não há nesses casos a nova regra de contadores/FORA. `bin/tests/test_hook_parei_so_lei.sh:40`, `bin/tests/test_hook_parei_so_lei.sh:48`, `bin/tests/test_hook_parei_so_lei.sh:68`, `bin/tests/test_hook_parei_so_lei.sh:78`.
- A seção 7b já declara separação do pouso de instrumento e produto, raias velhas esperado zero e contrato de cinco linhas mandado uma vez ao Codex; não equivale ao novo enforcement do push. `CLAUDE.md:512`, `CLAUDE.md:514`, `CLAUDE.md:574`. A revisão de linhas de mecanismo no mesmo commit é coerente com a documentação no marco, cuja regra está em `CLAUDE.md:517`. [nao verificado]
- A ordem do item 7 depende primeiro de resolver o universo/identidade de “não pousado” e só depois de medir zero/justificar ou remover: o produtor atual não alcança o universo da sonda e confunde ahead com patches pendentes. `bin/raias_estado.py:85`, `bin/raias_estado.py:96`, `bin/raias_estado.py:101`. A ordem do item 10 depende de tornar inequívoco contrato commitado/obra e o alcance `PUSH_SHA` antes de endurecer a recusa; `logs/` é ignorado hoje. `.gitignore:53`, `bin/pre-push.sh:33`. O contrato prevê gate10 apenas no segundo pouso; a proteção dos instrumentos do primeiro pouso pelo gate10 ainda inexistente [não verificado].

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

_1314 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
