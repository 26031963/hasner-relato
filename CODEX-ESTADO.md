# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 17:23** · publicado em **12/11/2026 17:27** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 17:23 · sessao b18576745da1 · TRANCA-VIVAS-08 · sonda · origem=Code

1. **Transcrição integral do dado fornecido.** A execução e o hash da sonda não foram conferidos independentemente nesta cópia. [nao verificado]

```text
sonda_leitura: sonda_v2.py [8c23d857275b021a] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T17:13:38.126957-03:00
PF#7 emp2 04/2026 universo=0 vivas_juiz=0 contador_medir=0
PF#8 emp2 05/2026 universo=24 vivas_juiz=0 contador_medir=0
PF#9 emp2 06/2026 universo=37 vivas_juiz=0 contador_medir=0
PF#10 emp2 07/2026 universo=152 vivas_juiz=0 contador_medir=0
PF#3 emp2 08/2026 universo=12898 vivas_juiz=4 contador_medir=0
  VIVA #20798 col367 disputa#2502 data_evento=2026-08-01 dia_turno=2026-08-01 conversa=False C20=False no_medir=False respondida=False validada=False materializada=True motivo=saida_sem_entrada
  VIVA #47879 col444 disputa#3309 data_evento=2026-08-03 dia_turno=2026-08-03 conversa=False C20=False no_medir=False respondida=True validada=False materializada=False motivo=orfao_14h
  VIVA #47881 col444 disputa#3309 data_evento=2026-08-04 dia_turno=2026-08-04 conversa=False C20=False no_medir=False respondida=True validada=False materializada=False motivo=intervalo_volta
  VIVA #47884 col444 disputa#3309 data_evento=2026-08-05 dia_turno=2026-08-05 conversa=False C20=False no_medir=False respondida=True validada=False materializada=False motivo=intervalo_volta
PF#11 emp3 04/2026 universo=0 vivas_juiz=0 contador_medir=0
PF#12 emp3 05/2026 universo=7 vivas_juiz=0 contador_medir=0
PF#13 emp3 06/2026 universo=28 vivas_juiz=0 contador_medir=0
PF#4 emp3 07/2026 universo=1165 vivas_juiz=0 contador_medir=0
PF#5 emp3 08/2026 universo=2048 vivas_juiz=0 contador_medir=0
PF#14 emp4 04/2026 universo=2 vivas_juiz=0 contador_medir=0
PF#15 emp4 05/2026 universo=63 vivas_juiz=0 contador_medir=0
PF#16 emp4 06/2026 universo=616 vivas_juiz=0 contador_medir=0
PF#2 emp4 07/2026 universo=493 vivas_juiz=0 contador_medir=0
PF#6 emp4 08/2026 universo=399 vivas_juiz=0 contador_medir=0
TOTAL {'universo': 17932, 'vivas': 4, 'medir': 0, 'classe:trancado_False': 4}
total_erros=0
== anexo: retira_acervo_tranca_2.txt ==
aval Ronald: RETIRA ACERVO-TRANCA-2 (publicado 16:3x, contraria CLAUDE.md 7b DINHEIRO item 3: nenhuma trava por exportada/trancada/paga; correcao provada entra em qualquer competencia). Nao executar os itens 1, 2, 3 e 5; nenhuma lei nova. Fica SO o item 4, que e lei existente (TRANCA-DE-VERDADE): as 4 perguntas vivas na comp 08 emp2 trancada -> listar pk e o degrau de chamados/juizes.py::pergunta_viva que as deixou vivas; curar na origem se for defeito; contador vivas_apesar_da_tranca na frota, esperado 0. Segue a fila; PAREI so em lei ou !
```

2. **Por PF, conforme a saída fornecida.** [nao verificado]

| PF | Empresa | Competência | Universo | Vivas juiz | Contador medir | Evidência |
|---|---|---|---:|---:|---:|---|
| 7 | 2 | 04/2026 | 0 | 0 | 0 | [nao verificado] |
| 8 | 2 | 05/2026 | 24 | 0 | 0 | [nao verificado] |
| 9 | 2 | 06/2026 | 37 | 0 | 0 | [nao verificado] |
| 10 | 2 | 07/2026 | 152 | 0 | 0 | [nao verificado] |
| 3 | 2 | 08/2026 | 12898 | 4 | 0 | [nao verificado] |
| 11 | 3 | 04/2026 | 0 | 0 | 0 | [nao verificado] |
| 12 | 3 | 05/2026 | 7 | 0 | 0 | [nao verificado] |
| 13 | 3 | 06/2026 | 28 | 0 | 0 | [nao verificado] |
| 4 | 3 | 07/2026 | 1165 | 0 | 0 | [nao verificado] |
| 5 | 3 | 08/2026 | 2048 | 0 | 0 | [nao verificado] |
| 14 | 4 | 04/2026 | 2 | 0 | 0 | [nao verificado] |
| 15 | 4 | 05/2026 | 63 | 0 | 0 | [nao verificado] |
| 16 | 4 | 06/2026 | 616 | 0 | 0 | [nao verificado] |
| 2 | 4 | 07/2026 | 493 | 0 | 0 | [nao verificado] |
| 6 | 4 | 08/2026 | 399 | 0 | 0 | [nao verificado] |

**Há defeito de UNIVERSO em `vivas_apesar_da_tranca`, quando usado para contar todas as vivas da competência.** A lista nasce vazia e só recebe perguntas no ramo em que pai ou destino pertence a `resgatados`; no ramo comum, apenas perguntas mortas são registradas, e as vivas ficam sem inclusão. Isso contraria o comentário “contador tem que ver o mesmo que quem valida” e a LEI-AKITA 8. (`app/chamados/services/competencia_trancada.py:81`, `app/chamados/services/competencia_trancada.py:119–134`, `CLAUDE.md:35–36`)

Esse recorte é mais estreito que a exceção humana do juiz: `resgatados` exige chamado vivo elegível e `resgatado_em > pf.fechado_em`; o juiz também reconhece “Cobrar este dia” e “Resolver dia”. (`app/chamados/services/competencia_trancada.py:89–106`, `app/chamados/juizes.py:1083–1089`)

Já `r['perguntas']` vazio tem semântica intencional: representa perguntas que seriam carimbadas pela operação, retirada após a mudança para tranca por leitura. Isso não justifica omitir vivas de `vivas_apesar_da_tranca`. (`app/chamados/services/competencia_trancada.py:126–134`)

O significado operacional de `no_medir=False` dentro da sonda, inclusive se representa ausência do filtro SQL ou ausência da lista final, é **[nao verificado]**.

3. **Degrau e motivo de sobrevivência de cada VIVA.**

`pergunta_viva()` delega a `_pergunta_viva_calc()`; a tranca é testada antes da resposta do colaborador. Portanto resposta não ultrapassa uma tranca que tenha retornado `True`. (`app/chamados/juizes.py:888–894`, `app/chamados/juizes.py:1391–1407`)

| VIVA | O que os campos publicados permitem identificar | [nao verificado]
|---|---|
| **#20798** | Com `respondida=False` e `materializada=True`, são compatíveis o retorno por **célula acusando** ou por **materialização sem testemunha suficiente**. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1473–1475`) |
| **#47879** | São compatíveis o ramo de **resposta com texto sem concordância comprovada com a luz**, **célula acusando** ou **ausência de materialização**. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1407–1419`, `app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1466–1467`) |
| **#47881** | Mesmas possibilidades: resposta nas condições do juiz, célula acusando ou ausência de materialização. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1407–1419`, `app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1466–1467`) |
| **#47884** | Mesmas possibilidades: resposta nas condições do juiz, célula acusando ou ausência de materialização. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1407–1419`, `app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1466–1467`) |

`respondida=True` sozinho não prova passagem pelo ramo da resposta: esse ramo também exige texto não vazio e ausência de decisão administrativa do dia. Esses campos não foram publicados. (`app/chamados/juizes.py:1405–1419`; valores individuais: [nao verificado])

Existe ainda o retorno por falta dentro da janela viva. Com `hoje=2026-11-12`, a janela calculada pelo código é 20/09–20/11, excluindo os quatro dias de agosto; o argumento efetivamente usado pela sonda é **[nao verificado]**. (`app/chamados/juizes.py:592–618`, `app/chamados/juizes.py:1454–1462`, `app/ponto/janelas.py:10–33`, `app/ponto/janelas.py:36–63`)

**Por que C20 não as calou:** nas quatro linhas aparecem `conversa=False`, dia preenchido e `C20=False`; logo, o dado não sustenta “conversa” nem “dia None” como explicação individual. [nao verificado]

O código transforma conversa em dia de turno `None`; para as demais perguntas usa `data_evento`. O juiz começa em `False`, exige dia/colaborador/empresa, procura o PF da competência calculada e, encontrando-o, retorna `not tocou`. (`app/chamados/juizes.py:334–338`, `app/chamados/juizes.py:991–992`, `app/chamados/juizes.py:1023–1027`, `app/chamados/juizes.py:1062–1079`)

Assim, **`C20=False` não identifica sozinho “sem PF” ou “toque posterior”**. O PF efetivamente encontrado, seu `fechado_em` e a trilha humana de cada pergunta são **[nao verificado]**.

Se o juiz encontrou PF vivo para aquele colaborador/dia, seu resultado `False` corresponde a toque reconhecido. Os atos admitidos são `cobrar_dia_manual`, `veredito_celula` e `desarquivar_chamado`, com usuário identificado diferente do sistema e instante estritamente posterior à tranca. **Qual ato ocorreu para #20798, #47879, #47881 e #47884: [nao verificado].** (`app/chamados/juizes.py:1078–1089`, `app/chamados/juizes.py:1252–1254`, `app/chamados/juizes.py:1268`, `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1315`)

4. **Escritores, leitores e significado de dia.**

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `conversa` | `abrir()` passa `False`; `abrir_conversa()` passa `True`; nascimento instancia e salva. (`app/chamados/models.py:2790`, `app/chamados/models.py:2802`, `app/chamados/models.py:3015–3018`) | O juiz usa o discriminador para retirar o dia civil da leitura de turno e da tranca. (`app/chamados/juizes.py:334–338`, `app/chamados/juizes.py:991–992`) |
| `data_evento` | A atribuição central é `gravar_dia_do_fato`; nascimento, costura de linha sem dia e rechaveamento passam por ela. (`app/chamados/services/fio.py:791–802`, `app/chamados/models.py:3017`, `app/chamados/services/fio.py:880`, `app/chamados/services/fio.py:1038`) | Fato recebe **dia do turno**; conversa recebe **dia civil local do pedido**, conservado pelo derivador. (`app/chamados/models.py:2784–2803`, `app/chamados/services/fio.py:1631–1636`) |
| `vivas_apesar_da_tranca` | `medir()` inicializa e acrescenta somente no ramo dos resgatados. (`app/chamados/services/competencia_trancada.py:81`, `app/chamados/services/competencia_trancada.py:119–124`) | `medir_frota()` lê seu tamanho, expõe a contagem por competência e a soma em `ficam_com_o_admin`. (`app/chamados/services/competencia_trancada.py:313–320`) |

**Os leitores não aplicam todos a mesma distinção de dia.** O juiz distingue conversa de turno; `medir()` seleciona pela faixa de `data_evento` cru, sem discriminar conversa, e `reabrir()` também usa esse campo cru. Portanto esses filtros podem incluir o dia civil de uma conversa no universo da competência. (`app/chamados/juizes.py:334–338`, `app/chamados/services/competencia_trancada.py:107–109`, `app/chamados/services/competencia_trancada.py:252–253`)

Outro leitor cru é a lavra, que passa `p.data_evento` a `expira_em`; a documentação dessa função ainda associa conversa a dia `None`, enquanto `abrir_conversa()` grava `localdate()`. (`app/chamados/veredito_lavrado.py:131`, `app/chamados/veredito_lavrado.py:273`, `app/chamados/models.py:2802`)

Na exposição do contador, o comando imprime `ficam_com_o_admin` como `decidir_pelo_dp`, mas seu total principal soma somente chamados, perguntas e disputas; a API entrega `por_competencia`, onde está a contagem própria das vivas. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:20–23`, `app/api/views_mensageria.py:1573–1574`)

A saída informa **`total_erros=0`**. [nao verificado]

## 12/11 17:21 · sessao d3492b1ae160 · - · - · origem=Ronald (tmux)

- **Defeito de universo do contador, quando apresentado como todas as perguntas vivas apesar da tranca.** `medir()` inicializa `vivas_apesar_da_tranca=[]` e só acrescenta perguntas quando o pai ou destino pertence a `resgatados`; o ramo geral acrescenta somente perguntas mortas a `perguntas_lidas_trancadas`, deixando vivas sem inclusão no contador. O próprio comentário exige contador igual à fila; LEI-AKITA 8 exige “contador == universo”. (`app/chamados/services/competencia_trancada.py:77`, `:119`, `:123`, `:124`, `:130`, `:133`; `CLAUDE.md:35`)
- A diferença de universo é mais estreita que “houve toque humano”: `resgatados` contém somente chamados com status VIVO, competência do chamado igual à medida, módulo sujeito à tranca e `resgatado_em > pf.fechado_em`. A exceção do juiz abrange também Cobrar dia e Resolver dia. (`app/chamados/services/competencia_trancada.py:89`, `:93`, `:97`, `:101`, `:105`; `app/chamados/juizes.py:1071`, `:1083`)
- **Não confundir o significado operacional de `perguntas=[]` com o defeito de `vivas_apesar_da_tranca`.** A primeira lista é vazia por construção porque as perguntas saem da fila por leitura e não são mais carimbadas; isso é semântica explicitamente documentada. A segunda pretende enxergar vivas que permanecem na fila, mas só tem o append restrito aos resgatados. (`app/chamados/services/competencia_trancada.py:126`, `:128`, `:120`, `:124`)
- A causa específica de cada `no_medir=False`, além do que literalmente diz a saída fornecida, é **[não verificado]**: não localizei fonte `sonda_v2.py` nesta exploração, e o código da sonda não foi fornecido.

**Escritor e leitores do contador:**

| Papel | Evidência |
|---|---|
| Inicialização e único append da lista | `app/chamados/services/competencia_trancada.py:81`, `:124` |
| `medir_frota()` lê a lista, acrescenta seu tamanho a `ficam_com_o_admin` e expõe `vivas_apesar_da_tranca` por competência | `app/chamados/services/competencia_trancada.py:313`, `:316`, `:317` |
| A soma de frota não inclui chave própria para total `vivas_apesar_da_tranca`; a informação fica dentro de `por_competencia` | `app/chamados/services/competencia_trancada.py:311`, `:318`, `:320` |
| Command lê `medir_frota`, imprime `ficam_com_o_admin` como `decidir_pelo_dp`; total principal soma só chamados + perguntas + disputas | `app/chamados/management/commands/chamados_em_competencia_trancada.py:13`, `:20`, `:23` |
| API lê `medir_frota` e devolve `ficam_com_o_admin` e `por_competencia` | `app/api/views_mensageria.py:1562`, `:1573`, `:1574` |
| Cliente HTTP e ferramenta propagam resposta da API | `mensageria/nucleo/core_client.py:239`, `:241`; `mensageria/nucleo/ferramentas.py:2035`, `:2036` |
| Placar lê somente total principal do command, sem campo próprio de vivas | `bin/placar_code.sh:287`, `:289` |
| Command de encerramento enumera várias listas, mas omite a lista `vivas_apesar_da_tranca` dessa impressão | `app/chamados/management/commands/encerrar_por_competencia_trancada.py:36`, `:38`, `:39` |

O `medir()` usa a janela de fechamento e filtro direto em `data_evento`; para chamados usa `competencia_do_chamado`. O juiz da tranca usa dia do turno e competência calculada por `janela_atual`; assim os leitores não usam todos a mesma expressão de dia, embora uma igualdade entre os dois dias seja mostrada nas quatro linhas da saída fornecida. (`app/chamados/services/competencia_trancada.py:87`, `:93`, `:108`; `app/chamados/juizes.py:984`, `:992`, `:1062`)

## 12/11 17:21 · sessao 2b245d6a8a87 · - · - · origem=Ronald (tmux)

Evidências dos campos:

- `conversa`: `abrir()` passa `False` em `app/chamados/models.py:2790`; `abrir_conversa()` passa `True` e `localdate()` em `app/chamados/models.py:2802`; `_nascer()` instancia e salva em `app/chamados/models.py:3015` e `app/chamados/models.py:3018`. O default é `False` em `app/chamados/models.py:2436`.
- Chamadores de conversa: reabertura administrativa em `app/chamados/services/acoes_disputa.py:263`; emissão sem dia de turno em `app/chamados/services/disputa_emissao.py:431` e `app/chamados/services/disputa_emissao.py:439`; `ancora_ausente` em `app/chamados/services/disputa_emissao.py:1371`.
- `data_evento`: atribuição centralizada em `app/chamados/services/fio.py:801`; nascimento chama e salva em `app/chamados/models.py:3017`; costura de linha sem dia chama em `app/chamados/services/fio.py:880` e salva em `app/chamados/services/fio.py:928`; rechaveamento chama em `app/chamados/services/fio.py:1038` e salva em `app/chamados/services/fio.py:1044`. A costura que data recusa conversa em `app/chamados/services/fio.py:876`; o rechaveamento também em `app/chamados/services/fio.py:1019`.
- Semântica do escritor: fatos recebem dia do turno; conversa recebe dia civil. As duas portas provam essa distinção em `app/chamados/models.py:2790` e `app/chamados/models.py:2802`; o derivador conserva o pedido/gravado da conversa em `app/chamados/services/fio.py:1631`.
- Leitor juiz: `_dia_de_turno_da_pergunta()` transforma conversa em `None` e lê `data_evento` nos demais casos em `app/chamados/juizes.py:334`; a tranca usa esse helper em `app/chamados/juizes.py:991`. `resolvedora_valida()` também reconhece conversa e retorna sem conferir turno em `app/chamados/juizes.py:2260`.
- Leitor `medir()`: filtra `data_evento` cru entre `ini/fim`, sem excluir `conversa`, em `app/chamados/services/competencia_trancada.py:107`. Portanto seu universo pode incluir dia civil de conversa; o universo do juiz distingue conversa. `reabrir()` também filtra o campo cru em `app/chamados/services/competencia_trancada.py:252`.
- Outro leitor cru: `lavrar()` calcula `expira_em(p.data_evento, ...)` em `app/chamados/veredito_lavrado.py:273`; a docstring de `expira_em` ainda associa conversa a dia `None` em `app/chamados/veredito_lavrado.py:131`, enquanto a porta grava `localdate()` em `app/chamados/models.py:2802`. Não afirmar que todos os leitores adotam a mesma interpretação.
- Contador `vivas_apesar_da_tranca`: lista inicializada em `app/chamados/services/competencia_trancada.py:81`; único `append` de produção encontrado no ramo dos chamados resgatados, condicionado ao juiz, em `app/chamados/services/competencia_trancada.py:119` e `app/chamados/services/competencia_trancada.py:124`. No ramo comum, pergunta viva não recebe append; só mortas entram em `perguntas_lidas_trancadas`, em `app/chamados/services/competencia_trancada.py:133`.
- `medir_frota()` lê `len` da lista tanto para `ficam_com_o_admin` quanto para a linha própria do contador em `app/chamados/services/competencia_trancada.py:316`; agrega apenas `ficam_com_o_admin` e os outros três totais em `app/chamados/services/competencia_trancada.py:318`.
- Comando da frota imprime `ficam_com_o_admin` como `decidir_pelo_dp` em `app/chamados/management/commands/chamados_em_competencia_trancada.py:20`; o total final soma somente chamados/perguntas/disputas, omitindo vivas/ficam_com_o_admin, em `app/chamados/management/commands/chamados_em_competencia_trancada.py:23`.

## 12/11 17:21 · sessao c74d56b50639 · - · - · origem=Ronald (tmux)

`pergunta_viva()` delega à lei em `_pergunta_viva_calc()`; com carga usa diretamente o cálculo, sem carga pode reutilizar memo por instância. (`app/chamados/juizes.py:862`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:890`, `app/chamados/juizes.py:894`)

A saída fornecida **não identifica o degrau exato nem o toque humano específico de nenhuma das quatro VIVAs**. [não verificado]

| Pergunta | Degraus compatíveis com os campos publicados, sem horizonte temporal diferente | [nao verificado]
|---|---|
| #20798: respondida=False, materializada=True | Célula acusa → `True` em `app/chamados/juizes.py:1441`; falta na janela viva → `True` em `app/chamados/juizes.py:1462`; carimbo sem materialização de fato → retorno em `app/chamados/juizes.py:1473`. Qual ocorreu: [não verificado]. |
| #47879, #47881, #47884: respondida=True, materializada=False | Resposta **com texto**, sem decisão admin e sem concordância comprovada com a luz → retorno em `app/chamados/juizes.py:1419`; célula acusa → `app/chamados/juizes.py:1441`; falta na janela → `app/chamados/juizes.py:1462`; ausência de materialização → `app/chamados/juizes.py:1467`. Qual ocorreu: [não verificado]. |

O carimbo `respondida=True` sozinho não prova que o ramo da resposta foi tomado: ele exige também texto não vazio e ausência da decisão admin descrita no código. (`app/chamados/juizes.py:1405`, `app/chamados/juizes.py:1407`, `app/chamados/juizes.py:1415`)

O ramo da falta depende da janela anterior/corrente, calculada com `hoje` recebido ou `date.today()`; não é lícito assumir que toda VIVA chegou nesse ramo. (`app/chamados/juizes.py:584`, `app/chamados/juizes.py:592`, `app/chamados/juizes.py:612`, `app/chamados/juizes.py:618`, `app/chamados/juizes.py:1455`)

`C20=False` explica apenas que a tranca não encerrou a pergunta no teste de `app/chamados/juizes.py:1395`; a causa individual não está publicada. [não verificado]

O leitor de dia retorna `None` para conversa e `data_evento` para as demais; `dia_da_pergunta_trancado()` passa esse dia ao juiz em lote. (`app/chamados/juizes.py:334`, `app/chamados/juizes.py:338`, `app/chamados/juizes.py:991`)

As quatro linhas dizem `conversa=False` e exibem dia não nulo, portanto não sustentam “era conversa” ou “dia None” como causa. [não verificado]

O juiz começa em `False`; só considera pares com dia, colaborador e empresa. Calcula a competência pela janela e procura PF vivo; apenas PF encontrado vai ao teste de toque, cujo resultado final é `not tocou`. (`app/chamados/juizes.py:1023`, `app/chamados/juizes.py:1025`, `app/chamados/juizes.py:1062`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1069`, `app/chamados/juizes.py:1078`)

PF vivo significa empresa/mês/ano correspondentes e `reaberto_em IS NULL`. (`app/ponto/services/fechamento.py:1464`, `app/ponto/services/fechamento.py:1470`)

Assim, `C20=False` sozinho não separa ausência de empresa/PF resolvido de toque posterior. O PF correspondente efetivamente lido e seu `fechado_em` não foram publicados. [não verificado] [nao verificado]

Os atos admissíveis são `cobrar_dia_manual`, `veredito_celula` e `desarquivar_chamado`; a trilha deve ter usuário diferente do sistema e instante estritamente posterior à tranca. (`app/chamados/juizes.py:1083`, `app/chamados/juizes.py:1087`, `app/chamados/juizes.py:1252`, `app/chamados/juizes.py:1268`, `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1315`)

Qual desses atos ocorreu para cada VIVA: **[não verificado]**.

## 12/11 17:16 · sessao 4d8764033314 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · verificador · origem=Code

**REPROVO a pré-verificação:** há ampliação de escrita em **HOJE** pelo cron e D1 permite apagar julgamento sem garantir a trilha obrigatória. Também permanece a classe “relógio fora da borda”. Evidências: [cron](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/ponto/management/commands/gerar_celulas.py:321), [limpeza D1](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/ponto/portas/celula.py:1148), [trilha best-effort](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/core/observ.py:105), [relógio](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/ponto/services/cadastro_futuro.py:22).

**1. Portas e alcance no Salvar**

O suplemento comum seleciona somente células **geradas, existentes, posteriores a hoje, dentro da janela corrente**, respeitando `desde`/`datas`; chama a mesma porta com horizonte desligado. (`app/ponto/services/cadastro_futuro.py:22`, `:24`, `:28`, `:41`)

| Porta | Função/caminho | Alcança futura existente? |
|---|---|---|
| Estrutura: município | `atualizar_posto` / `definir_municipio_posto` → `_evento_do_municipio` → `reavaliar_pela_troca_de_municipio` | Sim. (`app/colaboradores/services/estrutura.py:59`, `:65`, `:88`; `app/colaboradores/services/vinculo.py:947`) |
| Criar/alterar vínculo | `executar_vinculo` → suplemento explícito | Sim. (`app/colaboradores/services/vinculo.py:330`) |
| Alterar vigência por `save` | `_ec_vigencia_regenera_celulas` | Sim. (`app/escala/signals.py:80`) |
| Fechar/encerrar vínculos | `fechar_vigencia`; `encerrar_vinculo` também provoca o signal | Sim. (`app/colaboradores/services/vinculo.py:649`, `:829`; `app/escala/signals.py:81`) |
| Reabrir vigência impossível | `reabrir_vigencia_impossivel` | Sim, no mesmo atomic. (`app/colaboradores/services/vinculo.py:716`, `:737`) |
| Restaurar snapshot | `reverter_para_snapshot` | Sim. (`app/colaboradores/services/vinculo.py:1016`, `:1020`) |
| Trabalhar em feriado | `marcar_trabalha_em_feriado` | Sim. (`app/colaboradores/services/vinculo.py:897`) |
| Âncora | `_ec_ancora_regenera_celulas` | Sim. (`app/escala/signals.py:132`) |
| Tipo pela tela | `salvar_tipo` → `_propagar_futuras_template` | Sim, incluindo vínculos encerrados. (`app/escala/services/cadastro_tipo.py:326`, `:373`) |
| Tipo pelo admin | `save_model` → `salvar_tipo_no_admin` | Sim; acrescenta somente propagação futura. (`app/escala/admin.py:11`; `app/escala/services/cadastro_tipo.py:380`) |
| Folga manual/foto | `lancar_folga`, `alterar_folga`, lote/reparentamento → `relavrar_celulas_da_foto` | Sim. (`app/escala/folgas.py:298`; `app/ponto/tests/test_cadastro_futuro_c3_folgas.py:51`, `:57`, `:71`, `:79`, `:86`) |
| Calendário de folga | Salvar calendário/trocar calendário → `sincronizar_foto_futura_do_calendario` | Sim. (`app/escala/folgas.py:598`, `:638`; `app/colaboradores/services/vinculo.py:459`) |
| Configuração de feriado | Formulário/importação/exclusão → signals → `regenerar_futuras_por_feriado` | Sim, nas datas afetadas, incluindo a data antiga quando movido. (`app/core/views_config.py:70`, `:83`, `:144`, `:201`; `app/core/signals.py:19`, `:33`, `:39`) |
| Escala automática | `aplicar_no_vinculo` / `desfazer` → regeneração explícita | Sim; aqui cria ausentes e estende horizonte por ato declarado. (`app/escala/services/escala_auto_executor.py:107`, `:147`) |
| Correção retroativa | `_aplicar` → trecho histórico + suplemento | Sim. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:231`, `:243`) |

Nos trechos históricos de cadastro examinados, o novo suplemento não amplia o alcance: vínculo mantém início→hoje; município mantém somente hoje quando coberto; template mantém janela anterior→hoje; folgas mantêm o ramo histórico; correção mantém piso/topo anteriores. (`app/colaboradores/services/vinculo.py:319`, `:323`, `:941`; `app/escala/services/cadastro_tipo.py:337`, `:339`, `:351`; `app/escala/folgas.py:290`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`, `:228`)

**Há, porém, ampliação proibida na obra:** o cron antigo enumerava somente `ativa=True`; o novo `_regenerar_hoje_existentes` regenera células de **HOJE de vínculo inativo** respondido pela juíza. O teste exige precisamente essa reescrita. (`app/ponto/portas/celula.py:829`, versão anterior no diff fornecido; `app/ponto/management/commands/gerar_celulas.py:316`, `:321`, `:323`; `app/ponto/tests/test_cadastro_futuro_c4.py:165`)

**2. D1: limpeza, trilha e fato humano**

A limpeza ocorre após reescrever DNA no ramo normal, no ramo sem vínculo e na reversão. Anula os 13 campos de julgamento e usa `salvar_e_emitir`, preservando a emissão do evento da célula. (`app/ponto/portas/celula.py:262`, `:307`, `:832`, `:1103`, `:1146`; `:948`)

**Defeito: “com trilha” não está garantido.** A função ignora o retorno de `evento`; esse registrador engole falhas, desfaz apenas a tentativa de trilha e retorna `None`. Assim, a limpeza pode confirmar sem o registro antes/depois. A própria `editar_dia` já contém o tratamento contrário: exige trilha e levanta erro se ela faltar. (`app/ponto/portas/celula.py:1149`; `app/core/observ.py:105`, `:124`, `:139`; `app/ponto/portas/celula.py:643`)

O único detector de decisão humana nessa limpeza é `veredito_via == 'admin'`: conserva o julgamento, registra “conservado” e retorna normalmente **depois de o DNA ter sido escrito**. Não inspeciona o conteúdo da ata nem interrompe o Salvar. Portanto, o comentário que chama isso de “PAREI” descreve conservação/contagem, não uma parada operacional. (`app/ponto/portas/celula.py:301`, `:1128`, `:1136`, `:1140`)

Ausência/folga/atestado externos permanecem intactos: a limpeza altera somente atributos da célula, incluindo sua FK `ausencia`. O teste preserva o objeto externo. Existência de ata contendo fato humano em produção: **[não verificado]**. (`app/ponto/portas/celula.py:1103`, `:1146`; `app/ponto/tests/test_cadastro_futuro_c5.py:165`)

| Escritor que não passa pela limpeza | Constatação | Escopo |
|---|---|---|
| `editar_dia` | Pode escrever DNA futuro e marcar origem editada; não chama a limpeza. (`app/ponto/portas/celula.py:617`, `:633`) | Fora de “Salvar regenerando cadastro”; é edição humana. |
| `julgar_colab` em lote | Os laços recebem células sem a guarda futura do avulso e podem lavrar/carimbar. (`app/ponto/services/cartorio.py:1212`, `:1353`, `:1851`; guarda avulsa `:2025`) | Fora da regeneração de cadastro; continua caminho de julgamento futuro. |
| `veredito_celula` | Pode criar ausência e lavrar julgamento admin sem guarda de data futura. (`app/ponto/services/veredito_celula.py:211`, `:332`, `:351`) | Fora de “Salvar regenerando”; continua capaz de produzir julgamento futuro. |

Esses caminhos não são omissões da chamada D1 no **Salvar regenerando**; são limites da promessa global “futura com ata/carimbo = 0”, que o vigia também observa. (`app/ponto/services/vigia_cadastro_futuro.py:29`, `:45`)

**3. D2 e `substitui_dono`**

Nos eventos de atributo, o suplemento pergunta `vinculos_dos_dias` e permite somente dono igual ao vínculo ou resposta `None`; não calcula outro dono localmente. Portanto, `None` permite seguir para a porta, que verifica cobertura. (`app/ponto/services/cadastro_futuro.py:34`, `:37`; `app/ponto/portas/celula.py:224`)

As chamadas com `substitui_dono=True` encontradas são somente atos de linha do tempo: executar vínculo, fechar/reabrir vigência, restaurar snapshot, correção retroativa e signal de vigência. (`app/colaboradores/services/vinculo.py:335`, `:651`, `:739`, `:1020`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:245`; `app/escala/signals.py:83`)

**Município sem o antigo filtro não é defeito D2.** A chamada de hoje continua condicionada à cobertura hoje; a chamada futura passa pelo suplemento com `substitui_dono=False` e seu filtro de dono. (`app/colaboradores/services/vinculo.py:941`, `:947`; `app/ponto/services/cadastro_futuro.py:34`)

**4. D3, D5, D6, D7 e feriado**

| Item | Verificação |
|---|---|
| D3 | Realizado: futura sem sucessor recebe geradora nula, `montar_dna(None, ...)`, DNA anterior e carimbos de regeneração; a célula permanece. (`app/ponto/portas/celula.py:246`, `:255`, `:256`, `:257`) |
| D5 | Realizado: reabertura e suplemento compartilham o atomic; suplemento só seleciona futuras existentes. (`app/colaboradores/services/vinculo.py:716`, `:737`; `app/ponto/services/cadastro_futuro.py:24`, `:28`) |
| D6 | Realizado: aplicar/desfazer declaram `estender_horizonte=True`; início/volta são futuros. (`app/escala/services/escala_auto_executor.py:100`, `:108`, `:143`, `:148`) |
| D7, corpo do gerador | Realizado: inativo somente em `d > hoje`, escolhido pela juíza; cria ausente e preserva futura existente. (`app/ponto/portas/celula.py:899`, `:904`, `:921`) |
| D7, cron completo | **Não fica restrito a `d > hoje`**: a casca adicional reescreve hoje de inativo, conforme achado 1. (`app/ponto/management/commands/gerar_celulas.py:323`; `app/ponto/tests/test_cadastro_futuro_c4.py:165`) |
| Feriado | Realizado: `DateField.to_python` canoniza na fronteira; salvar entrega datas nova/antiga, excluir entrega a excluída; serviço recebe datas e consulta a juíza. (`app/core/signals.py:19`, `:25`, `:33`, `:39`; `app/ponto/services/cadastro_futuro.py:57`, `:67`) |

**5. Vigia e linha de CADASTRO**

O vigia lê `vinculo_do_dia` com a geradora como alimentação; sem geradora, usa o carregador da mesma autoridade. A candidata (a) não foi implementada: o ramo da geradora continua sem exigir `ativa`. (`app/ponto/services/vigia_cadastro_futuro.py:41`, `:58`; `app/escala/alimentacao.py:269`, `:282`, `:284`)

Vínculo inativo sem fim respondido pela juíza entra em `na_lista_do_admin_c6`; a porta recusa regenerá-lo. A lista administrativa C6 usa o predicado canônico de vínculo sem vigência. Isso classifica CADASTRO, sem cura automática. (`app/ponto/services/vigia_cadastro_futuro.py:69`; `app/ponto/portas/celula.py:148`; `app/escala/services/cadastro_realidade.py:320`, `:829`)

Os quatro indicadores têm esperado zero; o cron mede antes da geração, lavra depois, e a API lê snapshot. Ausência de medição retorna `None`, não zero. O **N de produção exigido antes de construir**, comprovado pelo dado raspado fornecido: **[não verificado]**. (`app/ponto/services/vigia_cadastro_futuro.py:86`, `:99`, `:119`; `app/ponto/management/commands/gerar_celulas.py:346`, `:353`; `app/api/views_mensageria.py:647`)

**6. Quem escreve e quem lê os campos**

| Campo | Escritores persistentes | Leitores identificados |
|---|---|---|
| `CelulaDia.ata` | Lavra, carimbo de precedência e limpeza D1, todos na porta. (`app/ponto/portas/celula.py:1055`, `:1082`, `:1187`, `:1146`) | Grade/folha, cartório, chamados, relatórios e vigia. (`app/escala/services/leitor_celula.py:379`; `app/folha/previa.py:83`; `app/ponto/services/cartorio.py:451`; `app/chamados/juizes.py:450`; `app/relatorios/cartao_pela_celula.py:287`; `app/ponto/services/vigia_cadastro_futuro.py:45`) |
| `dna` | Conteúdo produzido por `montar_dna`; persistido por geração, regeneração, edição humana e reversão. (`app/ponto/management/commands/gerar_celulas.py:73`; `app/ponto/portas/celula.py:256`, `:301`, `:633`, `:819`, `:904`, `:915`) | Marcos/pausas da escala, grade, cartório, julgamento admin, chamados e vigia. (`app/escala/models.py:1124`, `:1189`, `:1235`; `app/escala/utils.py:46`; `app/ponto/services/cartorio.py:1851`; `app/ponto/services/veredito_celula.py:31`; `app/chamados/juizes.py:948`; `app/ponto/services/vigia_cadastro_futuro.py:60`, `:79`) |
| `regenerada_em` | Regeneração e reversão. (`app/ponto/portas/celula.py:258`, `:303`, `:826`) | Cobrança órfã/carimbos de reescrita, fase, lavra e cartório. (`app/escala/services/regeneracao.py:84`, `:101`, `:119`; `app/escala/services/fase_12x36.py:189`; `app/chamados/veredito_lavrado.py:193`; `app/ponto/services/cartorio.py:393`) |
| `regeneracoes` | Incrementos na regeneração e reversão. (`app/ponto/portas/celula.py:259`, `:304`, `:827`) | Os próprios incrementos leem o valor anterior; outro consumidor de produção: **[não verificado]**. |
| `dna_anterior` | Regeneração guarda; reversão limpa. (`app/ponto/portas/celula.py:257`, `:302`, `:820`) | Reversão e comando operacional. (`app/ponto/portas/celula.py:810`, `:816`; `app/ponto/management/commands/aplicar_col900.py:173`) |
| `EscalaColaborador.data_fim` | Porta de vínculo: reativar, fechar, reabrir, encerrar, corrigir/criar trecho, snapshot e desfazer automático. (`app/colaboradores/services/vinculo.py:271`, `:629`, `:717`, `:831`, `:958`, `:968`, `:1018`, `:1088`) | Cobertura, vínculo do dia, candidatura, signals e cadastro. (`app/escala/alimentacao.py:150`, `:189`, `:205`, `:228`; `app/escala/signals.py:25`, `:57`; `app/escala/services/cadastro_realidade.py:306`) |
| `EscalaColaborador.ativa` | Porta de vínculo, incluindo criação, absorção e restauração. (`app/colaboradores/services/vinculo.py:270`, `:286`, `:629`, `:717`, `:786`, `:830`, `:959`, `:1018`, `:1075`, `:1088`, `:1094`) | Candidatura/cobertura, vínculo do dia, D7 e cadastro. (`app/escala/alimentacao.py:150`, `:189`, `:205`, `:228`; `app/ponto/portas/celula.py:899`; `app/escala/services/cadastro_realidade.py:256`, `:306`) |

A tabela identifica escritores e leitores concretos; **censo global exaustivo de todos os consumidores de `ata`/`dna` e, especialmente, `data_fim`/`ativa`: [não verificado]**.

O JUIZES confirma autoridades distintas para vínculo do dia e cobertura, além de grade/precedência/ausência/feriado. Não autoriza substituir uma pergunta pela outra. (`app/docs/JUIZES.md:103`, `:105`, `:107`, `:108`, `:121`, `:123`)

**7. Classes de reprovação**

| Classe | Presença/ausência demonstrada |
|---|---|
| Segundo juiz | Ausente nos dois serviços novos: usam autoridades existentes de vínculo, cobertura e produtor de DNA. (`app/ponto/services/cadastro_futuro.py:35`, `:41`, `:67`; `app/ponto/services/vigia_cadastro_futuro.py:26`, `:58`, `:79`) |
| Segundo escritor fora da porta | Ausente nesses serviços: suplemento chama a porta; vigia não salva células. (`app/ponto/services/cadastro_futuro.py:41`; `app/ponto/services/vigia_cadastro_futuro.py:43`, `:83`) |
| Relógio fora da borda | **Presente:** defaults leem `localdate()` no interior; há chamadores reais sem `hoje`. É exatamente a armadilha descrita no plano. (`app/ponto/services/cadastro_futuro.py:22`, `:53`; `app/core/signals.py:33`; `app/escala/services/cadastro_tipo.py:386`; `app/docs/PLANO-PADROES.md:92`, `:94`) |
| Derivação paralela da fonte única | Ausente na decisão de vínculo dos serviços novos; comparação de cadastro reutiliza `montar_dna`. (`app/ponto/services/cadastro_futuro.py:36`; `app/ponto/services/vigia_cadastro_futuro.py:58`, `:79`) |
| Selo/RED sem mordida | Existem mutantes executáveis D1/D3 e D2/D5/D6/D7/feriado; **não cobrem a exigência de abortar quando falta trilha D1**. O teste de admin exige conservação com selo 1. (`app/ponto/tests/test_cadastro_futuro_c5.py:27`, `:151`, `:199`; `app/ponto/tests/test_cadastro_futuro_c5b.py:30`; `app/ponto/portas/celula.py:1149`) |
| Ordem de trava | Snapshot executa vínculo→célula; há teste dessa sequência. Concorrência real PostgreSQL: **[não verificado]**. (`app/colaboradores/services/vinculo.py:1005`, `:1011`; `app/ponto/tests/test_cadastro_futuro_c3.py:228`) |
| Pouso antes do pré-requisito | Pouso, ordem efetiva E3/E4B e certificação completa: **[não verificado]** pelo material autorizado. | [nao verificado]
| Campo com dois significados | `ativa` e vigência têm usos distintos explícitos; a exceção da geradora inativa sem fim segue o aval (b), com C6. Não demonstrado novo conflito semântico nos carimbos de regeneração. (`app/escala/alimentacao.py:189`, `:205`, `:282`; `app/ponto/services/vigia_cadastro_futuro.py:69`; `app/escala/models.py:1714`) |

**8. Leis dos sítios**

- **L-007: violação demonstrada na garantia de trilha D1.** O ato crítico pode confirmar sem antes/depois persistidos. (`CLAUDE.md:32`; `app/ponto/portas/celula.py:1149`; `app/core/observ.py:139`)
- **Proibição expressa do aval de ampliar `d <= hoje`: violada pela casca do cron**, conforme o caso de vínculo inativo descrito no item 1. (`app/ponto/management/commands/gerar_celulas.py:323`; `app/ponto/tests/test_cadastro_futuro_c4.py:165`)
- **L-020:** denúncia da reescrita futura está presente. (`app/ponto/portas/celula.py:257`, `:258`, `:259`, `:302`)
- **L-002:** vigia e suplemento consultam as autoridades existentes. (`app/ponto/services/vigia_cadastro_futuro.py:58`, `:79`; `app/ponto/services/cadastro_futuro.py:36`)
- **L-052:** guardas de lavra e célula humana permanecem na porta; abertura continua exigindo o ato `apesar_da_lavra`. (`app/ponto/portas/celula.py:213`, `:248`, `:279`, `:292`)
- **L-090:** janela vem do juiz de competência; mês civil da foto de folga serve à fase mensal, explicitamente. (`app/ponto/management/commands/gerar_celulas.py:290`; `app/escala/folgas.py:629`)
- Violação nova concreta de **L-006, L-009, L-023, L-050, L-071, L-086 ou L-091** nesta leitura: **[não verificado]**.

## 12/11 17:14 · sessao 18d0805c03a7 · - · - · origem=Ronald (tmux)

5. Vigia realizado conforme (b), sem cura:

- `medir` recebe `hoje`, seleciona `data > hoje`; selo D1 conta todas futuras, demais baldes só geradas (`app/ponto/services/vigia_cadastro_futuro.py:24`, `:31`, `:45`, `:49`).
- Com geradora, alimenta `vinculo_do_dia`; sem geradora, usa `vinculos_dos_dias` (`app/ponto/services/vigia_cadastro_futuro.py:41`, `:58`). O carregador delega à mesma juíza (`app/escala/alimentacao.py:358`).
- A juíza conserva geradora inativa sem fim se as datas não a excluem; a candidata (a) não foi implementada (`app/escala/alimentacao.py:269`, `:282`, `:284`).
- Inativa sem fim vira `na_lista_do_admin_c6`, fora do contador estrutural esperado zero (`app/ponto/services/vigia_cadastro_futuro.py:69`). A porta recusa regenerá-la (`app/ponto/portas/celula.py:148`). A lista real C6 consulta o predicado canônico, formata por vínculo e agrega ao cadastro (`app/escala/services/cadastro_realidade.py:320`, `:331`, `:829`).
- Os quatro indicadores levam esperado zero; cron mede antes de gerar e lavra depois; API lê o snapshot, ausência é `None`, nunca zero (`app/ponto/services/vigia_cadastro_futuro.py:86`, `:99`, `:119`; `app/ponto/management/commands/gerar_celulas.py:346`, `:353`; `app/api/views_mensageria.py:647`).
- N de produção exigido antes de construir não consta do dado fornecido [não verificado]. Há relato local afirmando N=4, 143 células/51 futuras, mas não considero esse relato substituto da sonda raspada autorizada (`app/docs/RELATO.md:15`).

7. Classes:

- **Relógio fora da borda: PRESENTE, prova direta.** A lei manda ler uma vez na borda e condena explicitamente `hoje=None` lendo por dentro (`app/docs/GLOSSARIO.md:19`; `app/docs/PLANO-PADROES.md:92`, `:94`). Ambos novos serviços fazem isso (`app/ponto/services/cadastro_futuro.py:22`, `:53`). Caminhos efetivos sem injeção: sinais de feriado (`app/core/signals.py:33`, `:39`); admin de template → propagador com `hoje=None` (`app/escala/services/cadastro_tipo.py:386`, `:368`, `:375`); fechar/reabrir vigência (`app/colaboradores/services/vinculo.py:649`, `:737`). O vigia e o command estão corretos neste ponto (`app/ponto/services/vigia_cadastro_futuro.py:24`; `app/ponto/management/commands/gerar_celulas.py:339`, `:346`).
- **Segundo juiz/derivação paralela: ausentes nos dois novos serviços inspecionados.** Eles chamam autoridades existentes de vínculo e produtor de DNA (`app/ponto/services/cadastro_futuro.py:35`, `:41`, `:67`; `app/ponto/services/vigia_cadastro_futuro.py:26`, `:58`, `:76`, `:79`). Executei o predicado real `_so_alimenta` sobre ambos; nenhum tomador por fora encontrado; definição do censo em `app/escala/tests/test_contract_juiz_escala.py:97`, `:124`.
- **Selo sem mordida: D1/D3 têm mordida demonstrada nos ensaios.** Fontes dos mutantes e aplicação em memória: `app/ponto/tests/test_cadastro_futuro_c5.py:27`, `:74`; `app/ponto/tests/test_cadastro_futuro_c5b.py:30`, `:65`. Resultado observado na sessão, sem arquivo gerado: seis módulos, **113 testes OK**; **24/24 mutantes c5+c5b rc=1**, dos quais 21 por AssertionError e três por erro semântico do corpo mutado. D1/D3: todos os 14 por asserção. Persistência dessa saída em arquivo [não verificado].
- Os três erros semânticos foram: D2 que remove teste de `None` → `None.pk` em `app/ponto/services/cadastro_futuro.py:38`; feriado salvo/excluído sem canonização → comparação `date < str` em `:57`. Não foram importação ou fixture.
- **Ordem de travas:** snapshot declara e executa vínculo→célula (`app/colaboradores/services/vinculo.py:1005`, `:1012`); suplemento trava células antes de regenerar (`app/ponto/services/cadastro_futuro.py:28`). Prova global de concorrência PostgreSQL [não verificado]; os testes assumem ORM em memória (`app/ponto/tests/test_cadastro_futuro_c3.py:1`).
- **Pouso antes do pré-requisito:** não se comprova pouso no dado fornecido [não verificado]. Relato local diz E4B já pousada antes de abrir construção (`app/docs/RELATO.md:97`); regra exige suíte inteira antes do opus (`CLAUDE.md:377`). Execução offline não certifica suíte inteira.
- **Campo com dois significados:** a distinção `ativa`/vigência está explicitada em duas autoridades diferentes por pergunta, não autoriza trocar uma pela outra (`app/escala/alimentacao.py:189`, `:205`, `:228`; `app/docs/JUIZES.md:121`). O caso inativo sem fim conserva dono e ganha C6, conforme aval (b) (`app/ponto/services/vigia_cadastro_futuro.py:69`).

8. Leis dos sítios:

- A violação nova demonstrada é **Clock Injection** acima (`app/docs/PLANO-PADROES.md:92`, `:94`; `app/ponto/services/cadastro_futuro.py:22`, `:53`).
- Evento no Salvar e cron vigia seguem o desenho de `CLAUDE.md:171`; testemunha lê a autoridade em `app/ponto/services/vigia_cadastro_futuro.py:58`; preservação do cadastro C6 segue L-099 em `CLAUDE.md:179`.
- L-020 denuncia a reescrita futura com DNA anterior/instante/contador (`app/ponto/portas/celula.py:256`, `:258`, `:259`, `:302`). Guardas de editada/exportada continuam no escritor (`app/ponto/portas/celula.py:248`, `:250`, `:285`, `:289`).
- L-090: janela das futuras vem do `_janela_default` e autoridade de janela; mês civil da foto tem propósito explícito de fase mensal (`app/ponto/services/cadastro_futuro.py:23`; `app/ponto/management/commands/gerar_celulas.py:291`; `app/escala/folgas.py:629`).
- Nenhuma violação concreta nova de L-086/L-091/L-071 provada nesta leitura [não verificado]; não ampliaria conclusões sobre cálculo/front com esses ensaios sem banco.

## 12/11 17:14 · sessao 2f1c44e0a6a0 · - · - · origem=Ronald (tmux)

Achado concreto: a casca do cron amplia o universo de escrita de HOJE para vínculo inativo. O novo `_regenerar_hoje_existentes` seleciona toda célula gerada de HOJE e regenera pelo vínculo respondido pelo juiz, sem exigir `ativa=True` (`app/ponto/management/commands/gerar_celulas.py:316`, `:321`, `:323`). O teste fixa o vínculo inativo (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:282`) e exige que a célula de HOJE seja reescrita por ele (`app/ponto/tests/test_cadastro_futuro_c4.py:165`, `:168`, `:171`). Executei esse teste isolado com `python3 -B`, sem banco: 1 teste, OK, rc 0; a execução não tem arquivo de evidência persistido [não verificado]. O diff fornecido mostra que o loop antigo enumerava somente `ativa=True`; portanto esse caso era excluído antes [não verificado: referência histórica disponível apenas no diff do prompt].

Censo das portas:

| Porta | Caminho que alcança futura existente | Alcance anterior preservado |
|---|---|---|
| Estrutura: município | `atualizar_posto` / `definir_municipio_posto` → evento → `reavaliar_pela_troca_de_municipio` (`app/colaboradores/services/estrutura.py:59`, `:65`, `:88`; `app/colaboradores/services/vinculo.py:947`) | Chamada direta permanece somente HOJE e quando cobre HOJE (`app/colaboradores/services/vinculo.py:941`) |
| Executar vínculo | Suplemento explícito após cadastro (`app/colaboradores/services/vinculo.py:333`) | Trecho `data_inicio..hoje`, horizonte desligado (`:319`, `:323`) |
| Encerrar vínculo | `save` em `encerrar_vinculo` → signal de vigência (`app/colaboradores/services/vinculo.py:829`; `app/escala/signals.py:81`) | Signal limita ramo histórico a HOJE (`app/escala/signals.py:72`) |
| Fechar vigência/desligamento | `fechar_vigencia` → suplemento; desligamento delega a ele (`app/colaboradores/services/vinculo.py:649`, `:849`) | Suplemento começa após menor fim antigo/novo e somente futuro (`:648`; `app/ponto/services/cadastro_futuro.py:24`) |
| Reabrir vigência impossível | Update e suplemento no mesmo atomic (`app/colaboradores/services/vinculo.py:716`, `:737`) | Somente futuras existentes pelo suplemento (`app/ponto/services/cadastro_futuro.py:24`, `:28`) |
| Snapshot | Restauração → suplemento (`app/colaboradores/services/vinculo.py:1016`, `:1020`) | Suplemento somente futuro (`app/ponto/services/cadastro_futuro.py:24`) |
| Flag trabalha em feriado | Chamada histórica separada do suplemento (`app/colaboradores/services/vinculo.py:891`, `:897`) | Janela anterior até HOJE (`:886`, `:888`, `:892`) |
| Âncora | Signal → suplemento (`app/escala/signals.py:132`) | Janela atual até HOJE (`:120`, `:125`) |
| Tipo pela tela | `salvar_tipo` → propagação histórica + propagação futura (`app/escala/services/cadastro_tipo.py:321`, `:326`) | Ativos, janela anterior até HOJE (`:337`, `:339`, `:351`) |
| Tipo pelo admin | `salvar_tipo_no_admin` → somente propagação futura (`app/escala/services/cadastro_tipo.py:380`, `:386`) | Nenhuma chamada histórica nova nesse caminho (`:382`) |
| Folga manual/foto | Portas convergem em `relavrar_celulas_da_foto`, que chama suplemento (`app/escala/folgas.py:215`, `:298`, `:337`) | Ramo histórico conserva desde até HOJE; rejulgamento mantém intervalo vazio específico (`:284`, `:290`, `:294`) |
| Calendário de folga | Editor → sincronização futura → suplemento (`app/escala/folgas.py:598`, `:638`); troca de calendário também chama sincronização (`app/colaboradores/services/vinculo.py:459`) | Fotos novas recortadas para amanhã em diante, preservando manuais (`app/escala/folgas.py:184`, `:187`, `:632`) |
| Feriado criar/editar/importar/excluir | ORM dispara signals → serviço futuro (`app/core/views_config.py:70`, `:83`, `:144`, `:201`; `app/core/signals.py:33`, `:39`) | Serviço aceita exclusivamente `hoje < data` (`app/ponto/services/cadastro_futuro.py:57`) |
| Escala auto aplicar/desfazer | Regeneração explícita com `estender_horizonte=True` (`app/escala/services/escala_auto_executor.py:107`, `:147`) | Início estritamente futuro / volta a partir de amanhã (`:100`, `:143`) |
| Corrigir escala retroativa | Suplemento após trecho histórico (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:243`) | Mesmo piso e topo limitado a HOJE (`:225`, `:228`, `:231`) |

D2: eventos de atributo consultam `vinculos_dos_dias` e só permitem dono igual ao vínculo ou juiz calado (`app/ponto/services/cadastro_futuro.py:34`). `substitui_dono=True` aparece somente em executar vínculo, fechar/reabrir vigência, snapshot, correção retroativa e signal de vigência (`app/colaboradores/services/vinculo.py:335`, `:651`, `:739`, `:1020`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:245`; `app/escala/signals.py:83`).

Município sem o antigo filtro sobre o universo futuro não é defeito D2: apenas a chamada de HOJE exige cobertura HOJE; o suplemento futuro mantém `substitui_dono=False` e aplica o filtro do juiz (`app/colaboradores/services/vinculo.py:941`, `:947`; `app/ponto/services/cadastro_futuro.py:34`).

D3: a futura encerrada sem sucessor recebe geradora nula, DNA produzido por `montar_dna(None, ...)` e denúncia de regeneração (`app/ponto/portas/celula.py:246`, `:255`, `:256`, `:257`). O produtor recusa `trabalha=True` sem vínculo (`app/ponto/management/commands/gerar_celulas.py:80`).

D5: realizado dentro do mesmo atomic do update (`app/colaboradores/services/vinculo.py:716`, `:737`).

D6: ambas chamadas declaram horizonte explicitamente e mantêm o efeito estritamente futuro (`app/escala/services/escala_auto_executor.py:100`, `:108`, `:143`, `:148`).

D7: o corpo `gerar_celulas_periodo` só admite inativo em `d > hoje`, mediante juiz, cria ausente e preserva futura existente (`app/ponto/portas/celula.py:899`, `:904`, `:921`). Isso não sana o achado separado da casca do cron para HOJE (`app/ponto/management/commands/gerar_celulas.py:323`).

Feriado: a fronteira converte pela autoridade `DateField.to_python`, preserva a data anterior e entrega datas ao serviço; salvar e excluir passam por ela (`app/core/signals.py:19`, `:25`, `:33`, `:39`). O serviço não converte representação e consulta `vinculo_do_dia` (`app/ponto/services/cadastro_futuro.py:57`, `:67`).

Não classificar `marcar_opera_em_feriado` como defeito de regeneração do DNA existente: ele grava o posto (`app/colaboradores/services/estrutura.py:82`), e a leitura para DNA documentada é o default de vínculo NOVO (`app/escala/models.py:905`, `:910`, `:911`; `app/colaboradores/services/vinculo.py:290`). `atualizar_empresa` também não dispara regeneração (`app/colaboradores/services/estrutura.py:18`), mas mudança de `dia_inicio_competencia` altera a autoridade da janela, não campo do produtor de DNA (`app/ponto/janelas.py:36`; `app/ponto/management/commands/gerar_celulas.py:104`); defeito nesta obra [não verificado].

## 12/11 17:14 · sessao 19a0673d17e0 · - · - · origem=Ronald (tmux)

Achados D1:

- A limpeza fica em `app/ponto/portas/celula.py:1115`; é chamada depois da reescrita normal (`:301-310`), no ramo futura sem vínculo (`:253-263`) e ao reverter regeneração (`:819-836`). Limpa os 13 campos de `CAMPOS_DO_JULGAMENTO` (`:1103-1104`), conserva antes/depois em evento (`:1146-1152`) e emite outbox pelo `salvar_e_emitir` (`:948-956`).
- **Defeito comprovado: trilha não é obrigatória na execução.** `desfazer_julgamento_da_futura` ignora o retorno de `evento` (`app/ponto/portas/celula.py:1149`); esse escritor engole falha com savepoint e retorna `None`, deixando o negócio confirmar (`app/core/observ.py:94-105`, `:124-139`). Diferentemente, `editar_dia` exige retorno da trilha e levanta erro (`app/ponto/portas/celula.py:643-654`).
- **PAREI literal não implementado.** O único detector humano é `cel.veredito_via == 'admin'`; ele conserva tudo, registra evento e retorna normalmente (`app/ponto/portas/celula.py:1140-1145`), depois do DNA já ter sido escrito (`:301-310`). Não há inspeção do conteúdo da ata (`:1136-1147`). A ocorrência de ata com fato humano em produção continua [não verificado].
- O fato externo Ausencia/folga/atestado não é apagado por essa função: ela só anula atributos da própria CelulaDia, inclusive a FK ausência (`app/ponto/portas/celula.py:1103-1104`, `:1146-1148`); o teste demonstra preservar o objeto externo (`app/ponto/tests/test_cadastro_futuro_c5.py:165-172`).
- Executei ensaio `python3 -B` puramente em memória com a fixture `C5Test.setUp` (`app/ponto/tests/test_cadastro_futuro_c5.py:93`), `julgar` (`:114`) e `regenerar` (`:127`): retorno da trilha forçado a `None` → regeneração retorna 1 e ata fica `None`; via admin → regeneração retorna 1, troca geradora para 2 e conserva ata admin. Isso reproduz os dois comportamentos acima, sem provar ocorrência em produção. O teste existente exige conservação e selo 1, em vez de bloqueio (`:199-208`).

Escritores futuros que não passam pela limpeza:

| Escritor | Evidência | Enquadramento |
|---|---|---|
| `editar_dia` | Escreve DNA e origem editada, sem limite futuro nem limpeza (`app/ponto/portas/celula.py:617-642`); sua cauda não julga futuras (`:459-465`). | Fora do escopo literal “Salvar regenerando cadastro”; pode conservar ata antiga ao mudar DNA humano. |
| `julgar_colab` | Aceita lista recebida (`app/ponto/services/cartorio.py:940`); laços não filtram futuras (`:1212`, `:1353`), lavra e carimba (`:1851-1874`). | Não regenera DNA e não é Salvar cadastro; a guarda futura existe no avulso `julgar_celula` (`:2025-2026`), não no lote. |
| `veredito_celula` | Serviço não verifica data futura antes de executar (`app/ponto/services/veredito_celula.py:211-227`); pode criar ausência (`:332-339`) e lavra admin (`:351-352`). | Fora de “Salvar regenerando”; é caminho que pode criar julgamento futuro. |

“Quem escreve / quem lê”:

| Campo | Escritores persistentes encontrados | Leitores identificados |
|---|---|---|
| `CelulaDia.ata` | `lavrar_veredito` (`app/ponto/portas/celula.py:1055`, `:1082`), `carimbar_precedencia` (`:1187-1193`), limpeza D1 (`:1146-1148`). Cartório usa a porta (`app/ponto/services/cartorio.py:1853`); admin usa a porta (`app/ponto/services/veredito_celula.py:352`). | Grade/folha `app/escala/services/leitor_celula.py:379`; `app/folha/previa.py:83`; `app/folha/validacao_pdf.py:47`; relatórios `app/relatorios/cartao_pela_celula.py:287`, `app/relatorios/dia_da_frota.py:61`; chamados `app/chamados/juizes.py:450`, `:493`, `:966`, `:1344`, `app/chamados/services/materializacao.py:172`, `app/chamados/services/disputa_emissao.py:940`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/reconciliador.py:701`, `:879`; pautas `app/pautas/services.py:432`; feriado do posto `app/colaboradores/services/feriado_do_posto.py:97`; batida `app/ponto/juiz_batida.py:157`; lastro `app/ponto/services/lastro.py:89`; worklist `app/ponto/worklist.py:22`; cluster `app/ponto/services/cluster_espurio.py:27`; cartório `app/ponto/services/cartorio.py:451`, `:1440`; vigias `app/ponto/services/vigia_entrada.py:80`, `:123`, `app/ponto/services/vigia_cadastro_futuro.py:21`, `:45`, `app/ponto/services/vigia_pareador_celula.py:71`; diagnósticos `app/ponto/services/diagnostico_escala.py:156`, `app/ponto/services/esmeril_espelho.py:125`; comandos `app/ponto/management/commands/vigia_b5_ausencia.py:79`, `app/ponto/management/commands/backfill_b5_insumos.py:73`, `app/ponto/management/commands/tripwire_celulas.py:258`, `:341`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:61`, `app/ponto/management/commands/detectar_intervalo_ausente.py:47`, `app/ponto/management/commands/curar_residuo_bug115.py:45`, `:145`, `app/ponto/management/commands/celula_veredito_velho.py:108`; `app/folha/services/sem_furo.py:22`. |
| `CelulaDia.dna` | Produtor do conteúdo: `app/ponto/management/commands/gerar_celulas.py:73`. Gravação: regeneração `app/ponto/portas/celula.py:256`, `:273`, `:288-289`, `:301`; edição `:633-642`; reversão `:819`; criação/backfill do gerador `:904-917`. | Marcos/pausas/extra `app/escala/models.py:1124`, `:1189`, `:1235`; utilitários `app/escala/utils.py:46`, `:97`, `:130`, `:166`; cartório `app/ponto/services/cartorio.py:1251`, `:1257`, `:1398`, `:1437`, `:1851`, `:1872`; admin `app/ponto/services/veredito_celula.py:31`; vigia novo `app/ponto/services/vigia_cadastro_futuro.py:60`, `:79`; juiz de batida `app/ponto/juiz_batida.py:158`; chamados `app/chamados/juizes.py:948`, `:1338`; pautas `app/pautas/services.py:424`; dia `app/ponto/services/dia_do_colab.py:46`; diagnósticos `app/ponto/services/diagnostico_escala.py:153`, `app/ponto/services/esmeril_espelho.py:116`; vigia entrada `app/ponto/services/vigia_entrada.py:126`; faltas hoje `app/ponto/services/faltas_de_hoje.py:43`; comandos `app/ponto/management/commands/tripwire_celulas.py:30`, `:99`, `:259`, `app/ponto/management/commands/detectar_intervalo_ausente.py:53`, `app/ponto/management/commands/celulas_contra_o_template.py` [não verificado: caminho correto é escala/management/commands/celulas_contra_o_template.py:46-49], `app/ponto/management/commands/censo_col900.py:119`, `app/ponto/management/commands/censo_fase_12x36.py:392`, `app/ponto/management/commands/aplicar_col900.py:76`, `:172`. |
| `regenerada_em` | Regeneração `app/ponto/portas/celula.py:258`, `:303`; reversão `:826`. | `app/escala/services/regeneracao.py:84`, `:101-105`, `:119-122`; `app/escala/services/fase_12x36.py:189`; `app/chamados/veredito_lavrado.py:193`; `app/ponto/services/cartorio.py:393`; `app/ponto/services/dia_do_colab.py:94`. |
| `regeneracoes` | Regeneração incrementa `app/ponto/portas/celula.py:259`, `:304`; reversão incrementa `:827`. | Próprios incrementos leem o valor anterior (`:259`, `:304`, `:827`); outro leitor de produção encontrado [não verificado]. |
| `dna_anterior` | Regeneração guarda anterior `app/ponto/portas/celula.py:257`, `:302`; reversão limpa `:820`. | Reversão `app/ponto/portas/celula.py:810`, `:816`; comando operacional `app/ponto/management/commands/aplicar_col900.py:173`. |
| `EscalaColaborador.data_fim` | Todas as atribuições EC localizadas ficam na porta de vínculo: reativação `app/colaboradores/services/vinculo.py:271`; fechar `:629`; reabrir `:717`, `:731`; encerrar `:831-832`; criar encerrada/corrigir `:958`, `:968`; snapshot `:1018`; desfazer auto `:1088`. | Juízes `app/escala/alimentacao.py:189`, `:205`, `:228`; candidatura em lote `:150`; signals fotografam/comparecem `app/escala/signals.py:25-28`, `:57-60`, `:67`; portas usam os valores anteriores e os novos (`app/colaboradores/services/vinculo.py:698`, `:731`, `:958`); cadastro realidade `app/escala/services/cadastro_realidade.py:306-315`. Censo global de todos os leitores de data_fim não fechado [não verificado]. |
| `EscalaColaborador.ativa` | Porta de vínculo: reativação `app/colaboradores/services/vinculo.py:270`; criação `:286`, `:978`, `:1075`, `:1094`; fechar `:629`; reabrir `:717`, `:732`; absorver `:786`; encerrar `:830-832`; criar encerrada `:959`; snapshot `:1018`; desfazer auto `:1088`. | Vigência declarada/cobertura e vínculo do dia `app/escala/alimentacao.py:189`, `:205`, `:228`; candidatura `:150`; gerador D7 `app/ponto/portas/celula.py:899`; leitores administrativos/cadastro `app/escala/services/cadastro_realidade.py:256`, `:306-315`. Censo global de todos os leitores de ativa não fechado [não verificado]. |

As atribuições de ata fora da porta em `app/escala/utils.py:1887` são sobre cópia em memória nunca salva (`:1886`); a equivalente da porta também declara cópia de leitura (`app/ponto/portas/celula.py:981-996`). Não são segundo escritor persistente por essa evidência.

Autoridades do JUIZES: marcos congelados na EC (`app/docs/JUIZES.md:22-23`); dia de trabalho/precedência/ausência/feriado (`:103-109`); folga manual da pessoa versus calendário do vínculo (`:113-119`); vínculo do dia e cobertura (`:121-125`). A denúncia da regeneração não muda significado: `regenerada_em` exclui edição humana (`app/escala/models.py:1718-1721`), `regeneracoes` conta reescritas (`:1723-1724`) e `dna_anterior` guarda somente a última versão (`:1714-1729`).

## 12/11 17:03 · sessao 5849d664db84 · TIPO-NA-FONTE · censo · origem=Code

A cópia permite confirmar **12 chaves de topo na ata** e **9 no DNA**, pelos escritores identificados; não sustenta uma enumeração de 72 chaves de topo (`app/ponto/services/cartorio.py:738`, `app/ponto/services/cartorio.py:767`, `app/ponto/portas/celula.py:1084`, `app/ponto/management/commands/gerar_celulas.py:95`, `app/ponto/management/commands/gerar_celulas.py:122`).

**Identidade desta cópia com o HEAD do main: [nao verificado].** O registro local menciona o push `38605f141`, mas isso não autentica a árvore recebida (`app/docs/RELATO.md:32`).

O script foi executado em memória, sem instalação. Resultados dessa execução sem artefato persistido: **[nao verificado]** para conferência externa. Abaixo estão o inventário verificável no código e o texto integral executado.

Para as contagens, a unidade é **operação AST com chave literal de topo**. Incluí `management`, construtores e cópias em memória; separei testes e excluí migrations. Cópias integrais, chaves dinâmicas e subchaves aparecem separadamente no instrumento. **Não apresento esse perímetro como prova de cobertura irrestrita de Python dinâmico. [nao verificado]** [nao verificado]

| Medida | Resultado no perímetro | Diferença aritmética do aval | Evidência |
|---|---:|---:|---|
| Chaves de topo da ata | 12 | −60 | `app/ponto/services/cartorio.py:738`, `app/ponto/services/cartorio.py:767`, `app/ponto/portas/celula.py:1084` |
| Acessos literais de topo da ata | 95 em 27 arquivos | −79 acessos; +6 arquivos | Inventário abaixo; escritores em `app/ponto/services/cartorio.py:738` e leitores em `app/escala/services/leitor_celula.py:68` |
| Chaves de topo do DNA | 9 | Aval sem número | `app/ponto/management/commands/gerar_celulas.py:95`, `app/ponto/management/commands/gerar_celulas.py:122` |
| Acessos literais de topo do DNA | 41 em 19 arquivos | Aval sem número | Inventário abaixo, incluindo restauração em `app/ponto/portas/celula.py:788` |
| `strftime('%H:%M')` em `app/` | 200 chamadas | +39 | Relação integral em C |
| Mesmo formato fora de `app/` | +1 chamada | Total 201; +40 | `bin/disjuntor.py:2266` |

A equivalência entre esses recortes e os greps do aval é **[nao verificado]**. Portanto, **95 não substitui silenciosamente a catraca 174**. Há operações adicionais de cópia e chave variável na lavra, além de consulta ORM ao JSON (`app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:998`, `app/ponto/portas/celula.py:999`, `app/chamados/models.py:3356`).

Os tipos abaixo são **tipos produzidos pelos caminhos de código**. O conteúdo efetivamente existente em produção é **[nao verificado]**: os campos são JSON sem esquema declarado, e a porta aceita `dict(ata)` (`app/escala/models.py:1697`, `app/escala/models.py:1778`, `app/ponto/portas/celula.py:995`).

**B — Ata.** Nesta tabela, R/W contam operações literais de topo; os locais completos dos leitores vêm logo depois.

| Chave | R/W | Tipo produzido | Escritores `arquivo:linha:função` |
|---|---:|---|---|
| `tipo_dia` | 5/1 | string ou null | `app/ponto/services/cartorio.py:738:ata_do_dia` |
| `lampadas` | 40/3 | lista de dict | `app/ponto/services/cartorio.py:738:ata_do_dia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto`; `app/escala/utils.py:1892:montar_grade_prevista_periodo_por_turno` |
| `minutos_previstos` | 1/1 | int | `app/ponto/services/cartorio.py:739:ata_do_dia` |
| `minutos_realizados` | 3/1 | int | `app/ponto/services/cartorio.py:740:ata_do_dia` |
| `sem_ancora` | 1/1 | bool | `app/ponto/services/cartorio.py:741:ata_do_dia` |
| `n_orfas` | 4/1 | int | `app/ponto/services/cartorio.py:742:ata_do_dia` |
| `orfas` | 10/2 | lista de strings HH:MM, sem data | `app/ponto/services/cartorio.py:745:ata_do_dia`; `app/escala/utils.py:1895:montar_grade_prevista_periodo_por_turno`; formatação em `app/escala/utils.py:1832` |
| `n_missing` | 3/3 | int | `app/ponto/services/cartorio.py:746:ata_do_dia`; `app/ponto/services/cartorio.py:812:dispensar_por_ausencia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto` |
| `n_celulas` | 4/1 | int | `app/ponto/services/cartorio.py:762:ata_do_dia` |
| `cobertura_parcial` | 3/1 | bool; True quando presente | `app/ponto/services/cartorio.py:767:ata_do_dia` |
| `periodo_parcial` | 3/1 | string: `sem_periodo`, `dentro`, `fora`, `nao_sei` | `app/ponto/services/cartorio.py:771:ata_do_dia`; vocabulário em `app/ponto/turnos.py:2618` |
| `precedencia` | 1/1 | dict | `app/ponto/portas/celula.py:1084:carimbar_precedencia`; conteúdo em `app/ponto/services/cartorio.py:2000:aplicar_precedencia_na_celula` |

As duas escritas de `escala/utils.py` são em cópia em memória; a cópia do objeto ocorre antes delas (`app/escala/utils.py:1886`). A atribuição da ata na lavra ocorre na porta (`app/ponto/portas/celula.py:1006`, `app/ponto/portas/celula.py:1033`).

Leitores, preservando multiplicidades na mesma linha:

```text
tipo_dia:
 app/chamados/juizes.py:438:ausencia_parcial_cobra_o_resto
 app/chamados/juizes.py:456:dia_sem_trabalho [2 operações]
 app/ponto/management/commands/tripwire_celulas.py:342:medir_ata_fantasma
 app/escala/services/leitor_celula.py:397:grade_da_celula

minutos_previstos:
 app/escala/services/leitor_celula.py:399:grade_da_celula

minutos_realizados:
 app/folha/validacao_pdf.py:48:referencia_do_espelho
 app/escala/services/leitor_celula.py:386:grade_da_celula
 app/escala/services/leitor_celula.py:423:grade_da_celula

sem_ancora:
 app/escala/services/leitor_celula.py:421:grade_da_celula

n_orfas:
 app/chamados/juizes.py:510:_celula_furada
 app/ponto/services/cartorio.py:839:insumos_da_ata
 app/ponto/services/vigia_pareador_celula.py:74:censo
 app/escala/services/leitor_celula.py:190:_orfas_da_ata

orfas:
 app/ponto/juiz_batida.py:161:periodos_do_dia
 app/relatorios/cartao_pela_celula.py:288:orfas_e_isencao_da_ata
 app/chamados/juizes.py:510:_celula_furada
 app/ponto/services/cluster_espurio.py:29:_leitura_da_celula
 app/ponto/services/cluster_espurio.py:31:_leitura_da_celula
 app/ponto/services/vigia_pareador_celula.py:74:censo
 app/ponto/services/esmeril_espelho.py:125:assinaturas_do_colab
 app/ponto/management/commands/tripwire_celulas.py:264:medir_sobra_com_tipo_trocado
 app/escala/services/leitor_celula.py:187:_orfas_da_ata
 app/chamados/services/materializacao.py:181:_plantio_preenche_furo_na_data

n_missing:
 app/escala/services/leitor_celula.py:105:n_ok_lavrado
 app/escala/services/leitor_celula.py:175:_celulas_da_ata
 app/escala/services/leitor_celula.py:385:grade_da_celula

n_celulas:
 app/ponto/management/commands/tripwire_celulas.py:344:medir_ata_fantasma
 app/escala/services/leitor_celula.py:91:n_ok_lavrado
 app/escala/services/leitor_celula.py:105:n_ok_lavrado
 app/escala/services/leitor_celula.py:385:grade_da_celula

cobertura_parcial:
 app/chamados/juizes.py:438:ausencia_parcial_cobra_o_resto
 app/ponto/services/cartorio.py:424:parcial_so_lista
 app/escala/services/leitor_celula.py:433:grade_da_celula

periodo_parcial:
 app/ponto/services/cartorio.py:426:parcial_so_lista
 app/escala/services/leitor_celula.py:435:grade_da_celula
 app/escala/services/leitor_celula.py:436:grade_da_celula

precedencia:
 app/ponto/portas/celula.py:1081:carimbar_precedencia

lampadas:
 app/folha/previa.py:84:montar_previa
 app/ponto/juiz_batida.py:160:periodos_do_dia
 app/ponto/worklist.py:22:linha_da_ata
 app/relatorios/cartao_pela_celula.py:289:orfas_e_isencao_da_ata
 app/relatorios/dia_da_frota.py:21:_faltando
 app/chamados/reconciliador.py:701:_ctx_c2
 app/chamados/juizes.py:505:_celula_furada
 app/chamados/juizes.py:508:_celula_furada
 app/chamados/juizes.py:966:fato_do_marco_no_espelho
 app/chamados/juizes.py:1344:_resposta_concorda_com_a_luz
 app/pautas/services.py:432:contexto_do_dia_texto
 app/folha/services/sem_furo.py:22:dias_com_furo
 app/colaboradores/services/feriado_do_posto.py:97:presenca_em_feriado
 app/ponto/services/lastro.py:89:_lampada
 app/ponto/services/lastro.py:113:marcos_inexistentes
 app/ponto/services/lastro.py:128:marcos_apagados
 app/ponto/services/lastro.py:149:marcos_pendentes
 app/ponto/services/vigia_entrada.py:80:por_dia_da_semana
 app/ponto/services/vigia_entrada.py:123:do_dia
 app/ponto/services/cartorio.py:451:_marco_venceu_apagado
 app/ponto/services/cartorio.py:805:dispensar_por_ausencia
 app/ponto/services/cartorio.py:834:insumos_da_ata
 app/ponto/services/cartorio.py:913:_ata_ate_o_teto
 app/ponto/services/dia_do_colab.py:30:dia_do_colab
 app/ponto/services/cluster_espurio.py:29:_leitura_da_celula
 app/ponto/services/cluster_espurio.py:32:_leitura_da_celula
 app/ponto/services/vigia_pareador_celula.py:78:censo
 app/ponto/services/esmeril_espelho.py:129:assinaturas_do_colab
 app/ponto/services/diagnostico_escala.py:156:diagnostico_escala
 app/ponto/management/commands/tripwire_celulas.py:264:medir_sobra_com_tipo_trocado
 app/ponto/management/commands/tripwire_celulas.py:342:medir_ata_fantasma
 app/ponto/management/commands/curar_residuo_bug115.py:45:classificar
 app/ponto/management/commands/curar_residuo_bug115.py:145:_lavrar
 app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:61:classificar
 app/ponto/management/commands/detectar_intervalo_ausente.py:64:furos_intervalo
 app/escala/services/leitor_celula.py:68:_n_dispensadas
 app/escala/services/leitor_celula.py:82:_justificado_lavrado
 app/escala/services/leitor_celula.py:103:n_ok_lavrado
 app/escala/services/leitor_celula.py:115:_celulas_da_ata
 app/chamados/services/materializacao.py:173:_plantio_preenche_furo_na_data
```

As subchaves declaradas de `lampadas[]` são `tipo`, `hora`, `acesa`, `luz`, `tipo_real`, `isencao`, `dispensada`, `pendente`; `hora` é previsão ou rótulo posicional, enquanto `luz` guarda HH:MM realizado ou null (`app/ponto/services/cartorio.py:675`, `app/ponto/services/cartorio.py:702`, `app/ponto/services/cartorio.py:720`, `app/ponto/services/cartorio.py:591`, `app/ponto/services/cartorio.py:596`, `app/ponto/services/cartorio.py:915`, `app/ponto/juiz_batida.py:193`).

`precedencia` recebe `tipo_do_dia`, `faixa`, `rotulo`, `suprimidos`, `era_dia_previsto`, `texto`; os valores vêm do juiz de precedência, e `suprimidos` é lista de strings (`app/ponto/services/cartorio.py:2000`, `app/ponto/precedencia.py:391`).

**Comparação de hora sem data: sim para `orfas` e para `lampadas.hora/luz`.** Há ordenação/distância de órfãs, pertinência de batida por HH:MM, igualdade de marco previsto e comparação da resposta com a luz (`app/ponto/services/cluster_espurio.py:34`, `app/ponto/services/cluster_espurio.py:35`, `app/ponto/services/vigia_pareador_celula.py:110`, `app/ponto/services/vigia_pareador_celula.py:111`, `app/ponto/services/lastro.py:90`, `app/ponto/juiz_batida.py:230`, `app/chamados/juizes.py:1349`, `app/chamados/juizes.py:1358`, `app/chamados/juizes.py:1362`).

Há ainda leitor ORM de `lampadas.pendente`, projeção da ata inteira e preservação dinâmica de `precedencia`; não os converti em falsos acessos literais adicionais (`app/chamados/models.py:3356`, `app/chamados/reconciliador.py:879`, `app/ponto/portas/celula.py:104`, `app/ponto/portas/celula.py:997`).

**B — DNA.** A restauração está incluída: `alvo` é atribuído ao DNA antes das duas leituras por chave (`app/ponto/portas/celula.py:786`, `app/ponto/portas/celula.py:788`, `app/ponto/portas/celula.py:789`).

| Chave | R/W | Tipo produzido | Escritor | Leitor |
|---|---:|---|---|---|
| `dna_versao` | 1/1 | int, 2 | `app/ponto/management/commands/gerar_celulas.py:96:montar_dna`; constante em `app/ponto/management/commands/gerar_celulas.py:16` | `app/ponto/services/cartorio.py:484:impressao_insumos` |
| `vinculo_id` | 1/1 | ID numérico | `app/ponto/management/commands/gerar_celulas.py:97:montar_dna` | `app/ponto/portas/celula.py:789:reverter_regeneracao_dia` |
| `tipo_escala_id` | 0/1 | ID numérico | `app/ponto/management/commands/gerar_celulas.py:98:montar_dna` | Sem leitura literal individual no perímetro; blob comparado em `app/ponto/portas/celula.py:263` |
| `tipo_ciclo` | 1/1 | string | `app/ponto/management/commands/gerar_celulas.py:99:montar_dna`; campo em `app/escala/models.py:117` | `app/ponto/services/vigia_entrada.py:129:do_dia` |
| `marcos` | 27/1 | dict ou null | `app/ponto/management/commands/gerar_celulas.py:100:montar_dna` | Relação abaixo |
| `minutos_jornada` | 0/1 | int | `app/ponto/management/commands/gerar_celulas.py:101:montar_dna`; campo em `app/escala/models.py:49` | Sem leitura literal individual no perímetro; blob comparado em `app/ponto/portas/celula.py:263` |
| `intervalo_livre` | 0/1 | bool | `app/ponto/management/commands/gerar_celulas.py:102:montar_dna` | Sem leitura literal individual no perímetro; blob comparado em `app/ponto/portas/celula.py:263` |
| `lampadas` | 1/1 | lista de dict ou null | `app/ponto/management/commands/gerar_celulas.py:103:montar_dna` | `app/escala/utils.py:153:lampadas_de_dna` |
| `extra_declarada` | 1/1 | dict `{antes:int,depois:int}` | `app/ponto/management/commands/gerar_celulas.py:122:montar_dna` | `app/escala/models.py:1235:EscalaColaborador.extra_declarada_do_dia` |

Leitores de `marcos`:

```text
app/chamados/juizes.py:948:fato_do_marco_no_espelho
app/chamados/juizes.py:1338:_resposta_concorda_com_a_luz
app/chamados/services/adesao.py:36:universo_adesao
app/escala/models.py:1124:EscalaColaborador.marcos_do_dia
app/escala/models.py:1189:EscalaColaborador.pausas_do_dia
app/escala/utils.py:53:pausas_dna_periodo
app/escala/utils.py:104:marcos_dna_periodo
app/escala/utils.py:137:pausas_de_celulas
app/pautas/services.py:424:contexto_do_dia_texto
app/ponto/juiz_batida.py:159:periodos_do_dia [2 operações]
app/ponto/management/commands/aplicar_col900.py:172:Command.handle
app/ponto/management/commands/censo_col900.py:118:Command.handle
app/ponto/management/commands/censo_col900.py:119:Command.handle
app/ponto/management/commands/detectar_intervalo_ausente.py:53:furos_intervalo
app/ponto/management/commands/tripwire_celulas.py:99:medir_pausa_faltando
app/ponto/management/commands/tripwire_celulas.py:259:medir_sobra_com_tipo_trocado
app/ponto/portas/celula.py:788:reverter_regeneracao_dia
app/ponto/services/cartorio.py:484:impressao_insumos
app/ponto/services/cartorio.py:615:lampadas_do_dia
app/ponto/services/cartorio.py:840:insumos_da_ata
app/ponto/services/dia_do_colab.py:46:dia_do_colab
app/ponto/services/diagnostico_escala.py:153:diagnostico_escala
app/ponto/services/esmeril_espelho.py:116:assinaturas_do_colab
app/ponto/services/faltas_de_hoje.py:43:faltas_de_hoje_sem_cobranca
app/ponto/services/veredito_celula.py:31:_marcos_do_dia
app/ponto/services/vigia_entrada.py:126:do_dia
```

As subchaves de `marcos` são `hi`, `hf`, `hii`, `hfi`, com HH:MM ou null, e os opcionais `hii2`, `hfi2`; a declaração canônica dos pares está no modelo (`app/ponto/management/commands/gerar_celulas.py:83`, `app/ponto/management/commands/gerar_celulas.py:93`, `app/escala/models.py:1652`).

`dna.lampadas[]` recebe `chave`, `tipo`, `hora`, `instante`. **Esse `instante` é ISO sem fuso**, construído por `datetime.combine(...).isoformat(...)`; não o classifico como instante com fuso (`app/ponto/management/commands/gerar_celulas.py:68`, `app/ponto/management/commands/gerar_celulas.py:69`). O consumidor lê `hora` e `instante` para montar um dict indexado por HH:MM (`app/escala/utils.py:154`, `app/escala/utils.py:155`).

`extra_declarada.antes/depois` são inteiros e ambos são lidos pelo acessor do vínculo (`app/ponto/management/commands/gerar_celulas.py:119`, `app/ponto/management/commands/gerar_celulas.py:122`, `app/escala/models.py:1238`).

**Comparação de hora sem data: sim para `marcos` e `lampadas`.** Inclui pertinência, chave de dict, distância em minutos e igualdade ATA/DNA (`app/ponto/services/cartorio.py:640`, `app/ponto/services/cartorio.py:648`, `app/ponto/services/cartorio.py:666`, `app/ponto/services/cartorio.py:846`, `app/ponto/juiz_batida.py:230`, `app/ponto/services/vigia_entrada.py:127`, `app/escala/utils.py:155`). As comparações do blob inteiro também leem as demais chaves (`app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:594`, `app/ponto/portas/celula.py:872`).

**C — Classificação integral de `strftime('%H:%M')`.** A relação abaixo contém as 200 chamadas de `app/`; números repetidos preservam chamadas distintas na mesma linha. Os caminhos desta relação são relativos a `app/`.

Legenda dos fatos em P/C: **B** batida; **M** marco realizado transportado da batida; **O** órfã; **R** resposta/plantio pretendido; **V** marco previsto; **T** relógio da tentativa/recepção; **A** período declarado de ausência; **E** evento da esteira; **X** tentativa de materialização que entrou no erro. A distinção entre tentativa e batida efetiva aparece no tratamento offline e nos contextos de tentativa (`app/api/views.py:508`, `app/api/views.py:627`, `app/ponto/services/triagem_batida.py:741`); o erro de materialização está em `app/ponto/services/veredito_celula.py:329`.

A classificação primária usa P antes de C quando ambos se demonstram: **P62, C53, R83, U2**. As seis sobreposições P+C são `app/escala/utils.py:291`, `app/escala/utils.py:730`, `app/escala/utils.py:749`, `app/ponto/motor_calculo_v2.py:1671`, `app/ponto/motor_calculo_v2.py:1672`, `app/ponto/management/commands/detectar_ausencias.py:484`. Assim, há **59 chamadas com uso comparativo**, contando as sobreposições.

```text
arquivo | PERSISTIDO [linha:fato] | COMPARADO [linha:fato] | RENDER [linhas] | U
api/views.py | 627:T,645:T | — | 355,535,681,800,1457,2035,2959,2960 | —
api/views_core.py | 618:T,636:T | — | 343,521,672,794,1280,1281 | —
chamados/detector_anomalias.py | 234:V,235:B | 213:B | — | 222
chamados/juizes.py | — | 1814:V | — | —
chamados/models.py | — | — | 4070,4072,4076,4078 | —
chamados/reconciliador.py | — | 210:B | — | —
chamados/services/acoes_chamado.py | — | — | 383 | —
chamados/services/explicador.py | — | — | 57 | —
chamados/services/materializacao.py | — | 182:R | 465 | —
chamados/services/painel.py | — | — | 395 | —
chamados/services/tarefa_do_dia.py | — | 78:V | — | —
chamados/views_cobrar.py | — | — | 42,57 | —
colaboradores/services/calendario.py | — | 625:B | — | —
colaboradores/services/ficha.py | — | — | 28 | —
colaboradores/views.py | — | — | 2312,2313 | —
core/esteira_vigia.py | 1304:E,1560:E,1569:E,1572:E,1580:E,1590:E | — | — | —
core/templatetags/hasner_filters.py | — | — | 102,102,147 | —
core/views.py | — | — | 825 | —
escala/models.py | — | — | 415,415,444,444,452,452,509,509,517,517,546,546,664,664,665,665 | —
escala/servico_jornada.py | — | 455:V,456:B | 201,201 | —
escala/utils.py | 291:V,326:M,621:O,662:O,712:V,730:V,749:V,799:O,1549:M,1828:M,1832:O,1834:O | 378:V,429:B,782:V,1675:O,1707:V,1719:V,1724:V,1742:V,1754:V,1755:V,2005:B/O,2028:B/O | — | —
pautas/services.py | — | — | 431 | —
ponto/calculador/alimentacao.py | — | 119:B | — | —
ponto/calculador/nucleo.py | 142:B,146:B,238:B | — | — | —
ponto/management/commands/censo_col900.py | 138:B | — | — | —
ponto/management/commands/desvio_o68b.py | — | — | 125,126 | —
ponto/management/commands/detectar_ausencias.py | 484:V,848:B | — | 410,418,429,436,452,782 | —
ponto/management/commands/detectar_intervalo_ausente.py | — | 67:V,74:V,79:V,82:V,84:V | — | 81
ponto/management/commands/detectar_par_relampago.py | 97:B,98:B | — | 393,394 | —
ponto/management/commands/diff_calculador.py | — | — | 460 | —
ponto/management/commands/diff_reclassificar_partido.py | — | — | 97,98 | —
ponto/management/commands/dna_x_batida_real.py | 122:V,123:V | — | — | —
ponto/management/commands/emitir_furo_retroativo.py | — | — | 152 | —
ponto/management/commands/emitir_turno_aberto_retroativo.py | 167:V | — | — | —
ponto/management/commands/gerar_celulas.py | 33:V | — | — | —
ponto/management/commands/processar_alertas_avancados.py | 136:V | — | — | —
ponto/management/commands/reconciliar_fantasmas.py | 121:B,123:B,134:B,134:V | — | — | —
ponto/management/commands/tripwire_celulas.py | — | 192:V,195:O,280:V | — | —
ponto/management/commands/tripwire_tipo_batida.py | — | — | 64 | —
ponto/marcos.py | — | 11:V | — | —
ponto/motor_cal

[truncado em 20000 de 54867 caracteres]

## 12/11 16:53 · sessao 55d549d73138 · - · - · origem=Ronald (tmux)

Censo por AST: **200 chamadas** em **185 linhas de 59 arquivos** de `app/`, excluindo diretórios `tests`, `__pycache__`, `.venv`, `venv` e arquivos `test_*.py`; zero erros de parse. São 162 chamadas fora de `management` e 38 dentro. A diferença contra 161 é **+39 chamadas**; o comando/recorte do grep do aval não foi fornecido, portanto equivalência entre as medições é [não verificado]. Incluindo o repositório inteiro com os mesmos filtros há mais uma chamada de render em `bin/disjuntor.py:2266`.

A classificação primária usa **P > C > R** quando há mais de um destino demonstrado: **P62 + C53 + R83 + U2 = 200**. `U` identifica valor descartado, fora da taxonomia solicitada. Sobreposição explícita P+C: seis chamadas — `app/escala/utils.py:291`, `app/escala/utils.py:730`, `app/escala/utils.py:749`, `app/ponto/motor_calculo_v2.py:1671`, `app/ponto/motor_calculo_v2.py:1672`, `app/ponto/management/commands/detectar_ausencias.py:484`. Contando essas sobreposições, são 59 chamadas com uso comparativo. Os totais são a soma da tabela abaixo; classificação de todos os destinos possíveis em execução é [não verificado].

Na tabela, linhas repetidas significam duas chamadas na mesma linha; cada número compõe `arquivo:linha`.

```text
arquivo                                                       PERSISTIDO                 COMPARADO                       RENDER                              SEM USO
app/api/views.py                                              627,645                                                     355,535,681,800,1457,2035,2959,2960
app/api/views_core.py                                         618,636                                                     343,521,672,794,1280,1281
app/chamados/detector_anomalias.py                             234,235                    213                                                                 222
app/chamados/juizes.py                                                                   1814
app/chamados/models.py                                                                                                   4070,4072,4076,4078
app/chamados/reconciliador.py                                                            210
app/chamados/services/acoes_chamado.py                                                                                    383
app/chamados/services/explicador.py                                                                                       57
app/chamados/services/materializacao.py                                                   182                            465
app/chamados/services/painel.py                                                                                          395
app/chamados/services/tarefa_do_dia.py                                                     78
app/chamados/views_cobrar.py                                                                                             42,57
app/colaboradores/services/calendario.py                                                   625
app/colaboradores/services/ficha.py                                                                                      28
app/colaboradores/views.py                                                                                              2312,2313
app/core/esteira_vigia.py                                     1304,1560,1569,1572,1580,1590
app/core/templatetags/hasner_filters.py                                                                                  102,102,147
app/core/views.py                                                                                                       825
app/escala/models.py                                                                                                    415,415,444,444,452,452,509,509,
                                                                                                                       517,517,546,546,664,664,665,665
app/escala/servico_jornada.py                                                             455,456                        201,201
app/escala/utils.py                                          291,326,621,662,712,730,749,  378,429,782,1675,1707,1719,
                                                            799,1549,1828,1832,1834     1724,1742,1754,1755,2005,2028
app/pautas/services.py                                                                                                  431
app/ponto/calculador/alimentacao.py                                                       119
app/ponto/calculador/nucleo.py                               142,146,238
app/ponto/management/commands/censo_col900.py                 138
app/ponto/management/commands/desvio_o68b.py                                                                              125,126
app/ponto/management/commands/detectar_ausencias.py           484,848                                                     410,418,429,436,452,782
app/ponto/management/commands/detectar_intervalo_ausente.py                                67,74,79,82,84                                                      81
app/ponto/management/commands/detectar_par_relampago.py       97,98                                                       393,394
app/ponto/management/commands/diff_calculador.py                                                                          460
app/ponto/management/commands/diff_reclassificar_partido.py                                                               97,98
app/ponto/management/commands/dna_x_batida_real.py            122,123
app/ponto/management/commands/emitir_furo_retroativo.py                                                                    152
app/ponto/management/commands/emitir_turno_aberto_retroativo.py 167
app/ponto/management/commands/gerar_celulas.py               33
app/ponto/management/commands/processar_alertas_avancados.py 136
app/ponto/management/commands/reconciliar_fantasmas.py        121,123,134,134
app/ponto/management/commands/tripwire_celulas.py                                         192,195,280
app/ponto/management/commands/tripwire_tipo_batida.py                                                                     64
app/ponto/marcos.py                                                                     11
app/ponto/motor_calculo_v2.py                                1671,1672                   1488,3069                      2221,2222,2229,2230
app/ponto/propositor.py                                                                 340,342,342,343,345
app/ponto/services/ausencia.py                               910,911,934,934
app/ponto/services/cartorio.py                                                          878,1927
app/ponto/services/dia_do_colab.py                                                                                       57
app/ponto/services/diagnostico_escala.py                                                 26
app/ponto/services/entrada_sincronizada.py                    28
app/ponto/services/espelho.py                                                                                           210
app/ponto/services/faltas_de_hoje.py                                                     45
app/ponto/services/flip_auto.py                                                         219,221,221,222,232            319
app/ponto/services/lastro.py                                                                                            313
app/ponto/services/passivo_s84.py                            103
app/ponto/services/teto_na_lista.py                          82,83
app/ponto/services/triagem_batida.py                         472,473,479,480,492,540,
                                                            586,611,741                                               187
app/ponto/services/veredito_celula.py                        320,329                     252
app/ponto/services/vigia_pareador_celula.py                                              110
app/ponto/utils_turno_orfao.py                                                                                          40
app/relatorios/pdf_espelho.py                                                           178,609,716,960,963             732,733,881,885,907,907,955
app/relatorios/services.py                                                                                             133
```

Os dois valores sem uso são:

- `app/chamados/detector_anomalias.py:222`: `horario_real` no retorno `informativa`. Os dois chamadores tratam `grave` e `leve`, sem consumir esse retorno: `app/api/views.py:701`, `app/api/views.py:702`, `app/api/views.py:705`, `app/api/views_core.py:692`, `app/api/views_core.py:693`, `app/api/views_core.py:696`.
- `app/ponto/management/commands/detectar_intervalo_ausente.py:81`: a hora compõe `marcos_faltantes`, mas o chamador usa apenas as datas e a quantidade de furos; a emissão está encerrada por `continue`: `app/ponto/management/commands/detectar_intervalo_ausente.py:120`, `app/ponto/management/commands/detectar_intervalo_ausente.py:121`, `app/ponto/management/commands/detectar_intervalo_ausente.py:156`, `app/ponto/management/commands/detectar_intervalo_ausente.py:157`.

Persistidos: fato e destino demonstrado:

| Origem | Fato/previsão | Destino/prova |
|---|---|---|
| `app/escala/utils.py:326`, `:1549`, `:1828` | Batida/marco realizado | A hora realizada da célula torna-se `lampadas[].luz`: `app/ponto/services/cartorio.py:711`, `:712`, `:736`, `:738`. |
| `app/escala/utils.py:621`, `:662`, `:799`, `:1832`, `:1834` | Órfã | A lista da grade vira `ata['orfas']` sem instante: `app/ponto/services/cartorio.py:743`, `:745`. |
| `app/escala/utils.py:291`, `:712`, `:730`, `:749` | Marco previsto | Produzem a régua `tipo/hora`; a régua alimenta a hora prevista das lâmpadas: `app/escala/utils.py:734`, `:753`, `:1401`; `app/ponto/services/cartorio.py:638`, `:648`, `:711`. |
| `app/chamados/detector_anomalias.py:234`, `:235` | Previsto e batida, respectivamente | `ctx_desvio` vira `contexto_json` de chamado: `app/api/views.py:714`, `:717`, `:726`; `app/api/views_core.py:708`, `:720`. |
| `app/api/views.py:627`, `:645`; `app/api/views_core.py:618`, `:636` | Hora da tentativa/recepção, pelo relógio do servidor | Compõem contexto de disputa: `app/api/views.py:497`, `:498`, `:625`, `:642`, `:765`. Não são necessariamente o instante efetivo offline da batida: `app/api/views.py:508`. |
| `app/ponto/services/triagem_batida.py:472`, `:479`, `:741` | Tentativa/batida web | Chamado JSON, sessão, disputa: `app/ponto/services/triagem_batida.py:470`, `:486`, `:735`. |
| `app/ponto/services/triagem_batida.py:473`, `:480`, `:492` | Previsto | Chamado/sessão e texto da justificativa da batida: `app/ponto/services/triagem_batida.py:470`, `:486`, `:690`, `:697`, `:702`. |
| `app/ponto/services/triagem_batida.py:540`, `:586`, `:611` | Batida | Texto humano salvo em ocorrência e descrição de justificativa: `app/ponto/services/triagem_batida.py:603`, `:609`, `:613`. |
| `app/ponto/services/entrada_sincronizada.py:28` | Batida sincronizada | Ocorrência, justificativa e resolução: `app/ponto/services/entrada_sincronizada.py:35`, `:42`, `:43`, `:60`. |
| `app/ponto/motor_calculo_v2.py:1671`, `:1672` | Batida real e previsto | Lista do motor levada a `MetricaSnapshot.detalhe`: `app/ponto/motor_calculo_v2.py:2719`; `app/ponto/services/cxr_na_lista.py:79`, `:86`, `:130`, `:145`, `:148`. |
| `app/ponto/services/teto_na_lista.py:82`, `:83` | Marcos previstos `hi/hf` | Snapshot: `app/ponto/services/teto_na_lista.py:113`, `:116`. |
| `app/ponto/services/ausencia.py:910`, `:911`, `:934` duas chamadas | Período declarado de ausência | Auditoria antes/depois: `app/ponto/services/ausencia.py:932`, `:933`, `:934`; `app/core/auditoria.py:22`. Realização do período é [não verificado]. |
| `app/ponto/services/veredito_celula.py:320`, `:329` | Batida corrigida; tentativa de materializar marco | Ações/erros salvos em auditoria: `app/ponto/services/veredito_celula.py:356`, `:358`. Sucesso da materialização do caso `:329` é [não verificado]. |
| `app/ponto/management/commands/detectar_par_relampago.py:97`, `:98` | Batidas do par | `contexto_json['par']`: `app/ponto/management/commands/detectar_par_relampago.py:383`. |
| `app/ponto/management/commands/detectar_ausencias.py:484` | Marco previsto | `contexto_json['marcos_faltantes']`: `app/ponto/management/commands/detectar_ausencias.py:706`, `:710`, `:712`. |
| `app/ponto/management/commands/detectar_ausencias.py:848` | Batida | Ocorrência textual: `app/ponto/management/commands/detectar_ausencias.py:852`. |
| `app/ponto/management/commands/gerar_celulas.py:33` | Marco previsto | Helper alimenta `dna.marcos` e `dna.lampadas[].hora`: `app/ponto/management/commands/gerar_celulas.py:68`, `:83`, `:93`, `:100`, `:103`. |
| `app/ponto/management/commands/processar_alertas_avancados.py:136`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:167` | Saída prevista | Descrição do chamado: `app/ponto/management/commands/processar_alertas_avancados.py:121`, `:133`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`, `:159`. |
| `app/ponto/management/commands/reconciliar_fantasmas.py:121`, `:123`, `:134` duas chamadas | Entrada/saída reais; saída real e saída prevista na linha 134 | Diagnóstico `detalhe` salvo em ocorrência/auditoria: `app/ponto/management/commands/reconciliar_fantasmas.py:205`, `:206`, `:278`, `:280`, `:286`, `:299`. |
| `app/ponto/calculador/nucleo.py:142`, `:146`, `:238` | Batidas | Arquivo CSV diagnóstico: `app/ponto/management/commands/e6_oraculo.py:330`, `:404`, `:444`, `:445`. |
| `app/ponto/management/commands/censo_col900.py:138` | Primeira batida | Arquivo JSON opcional: `app/ponto/management/commands/censo_col900.py:178`, `:179`. |
| `app/ponto/management/commands/dna_x_batida_real.py:122`, `:123` | Marcos previstos | Arquivo JSON opcional: `app/ponto/management/commands/dna_x_batida_real.py:155`, `:156`, `:157`. |
| `app/ponto/services/passivo_s84.py:103` | Batida | Arquivo de reversão: `app/ponto/management/commands/passivo_disputa_retro.py:139`, `:141`, `:146`, `:148`. |
| `app/core/esteira_vigia.py:1304`, `:1560`, `:1569`, `:1572`, `:1580`, `:1590` | Eventos da esteira; não batida/órfã/resposta/marco | Arquivos de relato/fim: `app/core/esteira_vigia.py:1329`, `:1559`, `:1570`, `:1578`, `:1589`; o argumento de `destravar` chega a `f.write`: `app/core/esteira_vigia.py:1027`, `:1028`. |

Comparados: fato e operação demonstrada:

| Origem | Fato/previsão e uso |
|---|---|
| `app/escala/utils.py:291`, `:730`, `:749`, `:782`, `:1707`, `:1719`, `:1724`, `:1742`, `:1754`, `:1755` | Marcos previstos: chaves de dict, pertinência e busca de coluna. `app/escala/utils.py:292`, `:730`, `:749`, `:785`, `:1708`, `:1719`, `:1724`, `:1745`, `:1754`, `:1755`. |
| `app/escala/utils.py:378` | Marco previsto encadeado: chave de dict para casar régua e instante. `app/escala/utils.py:378`, `:381`, `:382`. |
| `app/escala/utils.py:429` | Batida: igualdade com hora realizada da célula. `app/escala/utils.py:431`. |
| `app/escala/utils.py:1675` | Batida/órfã: pertinência na lista de órfãs convertida em conjunto. `app/escala/utils.py:1650`, `:1651`, `:1675`. |
| `app/escala/utils.py:2005`, `:2028` | Batidas/órfãs: pertinência, igualdade de listas e ordem textual da hora realizada. `app/escala/utils.py:2006`, `:2032`, `:2034`, `:2039`. |
| `app/ponto/motor_calculo_v2.py:1488`; `app/ponto/calculador/alimentacao.py:119` | Entrada real: chave `(dia, HH:MM)` e pertinência no conjunto de pares bloqueados. `app/ponto/calculador/alimentacao.py:74`, `:119`. |
| `app/ponto/motor_calculo_v2.py:1671`, `:1672` | Real/previsto: deduplicação por igualdade do item; previsto também casa com marco da célula. `app/ponto/services/cxr_na_lista.py:86`, `:107`; `app/ponto/janela_he.py:296` e continuação do corpo. |
| `app/ponto/motor_calculo_v2.py:3069` | Entrada real: igualdade com hora realizada do alinhamento. `app/ponto/motor_calculo_v2.py:3069`. |
| `app/ponto/propositor.py:340`, `:342` duas chamadas, `:343`, `:345`; `app/escala/servico_jornada.py:455`, `:456`; `app/ponto/services/flip_auto.py:219`, `:221` duas chamadas, `:222`, `:232` | Previstos e batidas convertidos à mesma representação de hora do dia para atribuição posicional. `app/ponto/propositor.py:351`; `app/escala/servico_jornada.py:455`; `app/ponto/services/flip_auto.py:233`, `:238`. |
| `app/ponto/marcos.py:11` | Previsto: helper utilizado em igualdades de hora. `app/ponto/marcos.py:31`, `:33`, `:37`, `:39`, `:47`, `:49`, `:63`, `:65`, `:108`. |
| `app/chamados/reconciliador.py:210` | Batida: chave de dict usada para recuperar os integrantes do par por HH:MM. `app/chamados/reconciliador.py:209`, `:213`. |
| `app/chamados/detector_anomalias.py:213` | Batida: igualdade com hora realizada do alinhamento. `app/chamados/detector_anomalias.py:214`. |
| `app/chamados/juizes.py:1814` | Previsto: retorna HH:MM para comparação com resposta; a resposta é reduzida a minutos do dia. `app/chamados/juizes.py:1852`, `:1858`, `:1863`, `:1868`, `:1878`. |
| `app/chamados/services/materializacao.py:182` | Instante pretendido pela resposta/plantio: atribuição contra slots e horas reais/órfãs. `app/chamados/services/materializacao.py:179`, `:180`, `:181`, `:182`. |
| `app/chamados/services/tarefa_do_dia.py:78` | Previsto: igualdade com a hora prevista da lâmpada. `app/chamados/services/tarefa_do_dia.py:78`. |
| `app/ponto/services/cartorio.py:878`, `:1927` | Previstos: conjunto de marcos vencidos e dict de instantes por `(tipo, HH:MM)`. `app/ponto/services/cartorio.py:447`, `:450`, `:1927`, `:1930`, `:1931`, `:1936`. |
| `app/ponto/services/veredito_celula.py:252` | Previsto: chave de dict para materialização do marco faltante. `app/ponto/services/veredito_celula.py:275`, `:276`. |
| `app/ponto/services/faltas_de_hoje.py:45` | Previsto: volta a `datetime` para comparação com agora. `app/ponto/services/faltas_de_hoje.py:48`, `:49`. |
| `app/ponto/services/diagnostico_escala.py:26` | Batida: helper serve render e chaves de dict; também distância entre primeira entrada e `dna.marcos.hi`. `app/ponto/services/diagnostico_escala.py:160`, `:163`, `:165`, `:167`, `:172`. |
| `app/colaboradores/services/calendario.py:625` | Batida: pertinência em órfãs e lookup por hora. `app/colaboradores/services/calendario.py:626`, `:631`. |
| `app/ponto/services/vigia_pareador_celula.py:110` | Batida: pertinência em órfãs civis. `app/ponto/services/vigia_pareador_celula.py:111`. |
| `app/relatorios/pdf_espelho.py:178`, `:609`, `:716`, `:960`, `:963` | Batidas/órfãs: pertinência por HH:MM, inclusive filtragem textual de anomalias. `app/relatorios/pdf_espelho.py:178`, `:609`, `:720`, `:964`. |
| `app/ponto/management/commands/tripwire_celulas.py:192`, `:195`, `:280` | Previsto, órfã e previsto, respectivamente: igualdade HH:MM antes de teste por instante. `app/ponto/management/commands/tripwire_celulas.py:192`, `:195`, `:280`, `:285`. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:67`, `:74`, `:79`, `:82`, `:84` | Previstos: lookup de lâmpadas e de instante encadeado por `(tipo, HH:MM)`. `app/ponto/management/commands/detectar_intervalo_ausente.py:67`, `:74`, `:79`, `:82`, `:84`. |
| `app/ponto/management/commands/detectar_ausencias.py:484` | Previsto: traduzido/comparado por `marco_para_motivo` e persistido nos marcos faltantes. `app/ponto/management/commands/detectar_ausencias.py:482`, `:484`, `:712`; `app/ponto/marcos.py:31`. |

A lei fornecida exclui `dna.hi/hf` previstos; o produtor desta cópia efetivamente os escreve como hora do dia por `_hhmm`: `app/ponto/management/commands/gerar_celulas.py:33`, `:83`. O principal escritor de fato em HH:MM está antes da lavra: `_celula_do_marco` grava a batida em `hora`, e o cartório a transporta para `lampadas[].luz`; órfãs são transportadas como strings: `app/escala/utils.py:326`; `app/ponto/services/cartorio.py:712`, `:745`.

A classificação P inclui texto humano salvo, CSV/JSON diagnóstico e arquivos da est

[truncado em 20000 de 20154 caracteres]

## 12/11 16:52 · sessao c79798754d32 · C20-O168 · verificador · origem=Code

**A guarda cobre os quatro sítios alterados e a cascata E2, mas não todos os fechamentos automáticos alcançados pelo evento. Há também divergência entre “pai vivo preservado” e o universo efetivamente contado.** (`app/ponto/services/cartorio.py:965`, `app/chamados/reconciliador.py:651`, `app/ponto/services/lastro.py:401`, `app/chamados/services/disputa_emissao.py:2091`.)

Execução da suíte, execução de mutantes e ocorrência dos achados em produção: **[nao verificado]**. [nao verificado]

**1. Caminhos que fecham e cobertura da guarda**

| Caminho | Passa por `pode_executar_csf`? | Prova |
|---|---|---|
| Cartório: adiados pela lâmpada | **Sim** | `app/ponto/services/cartorio.py:1054` → guarda em `app/ponto/services/cartorio.py:965`. |
| Cartório: RECONCILIA-MARCO | **Sim** | `app/ponto/services/cartorio.py:1589`. |
| Cartório: MARCO-INEXISTENTE | **Sim** | `app/ponto/services/cartorio.py:1602`. |
| Cartório: B5.3b | **Sim** | `app/ponto/services/cartorio.py:1615`, `app/ponto/services/cartorio.py:1623`. |
| E2 → `disputa.fechar` → S88 | **Sim, antes de fechar a disputa** | Lote consulta o juiz em `app/chamados/reconciliador.py:649`; preservação precede `.fechar()` em `app/chamados/reconciliador.py:621`, `app/chamados/reconciliador.py:629`. |
| Cartório: FIO-DUP e FIO-DUP-C8 | **Não** | Fechos em `app/ponto/services/cartorio.py:1701`, `app/ponto/services/cartorio.py:1716`; isenção própria por texto em `app/ponto/services/cartorio.py:1687`. |
| `reconciliar_chamados_colab` → premissa morta → `_retratar` | **Não** | `app/chamados/reconciliador.py:1475`, `app/chamados/reconciliador.py:1498`, `app/chamados/reconciliador.py:524`. |
| `reconciliar_chamados_colab` → órfãs → `lavrar_sem_turno_aberto` → disputa → S88 | **Não** | `app/chamados/reconciliador.py:1467`, `app/chamados/reconciliador.py:1653`, `app/chamados/reconciliador.py:413`. |
| Consumidor `EventoCelula` → E2 | **Sim** | `app/chamados/reconciliador.py:1399` → proteção central em `app/chamados/reconciliador.py:621`. |
| Consumidor → chamados absolvidos pela célula → `_retratar` | **Não** | `app/chamados/reconciliador.py:1402`, `app/chamados/reconciliador.py:1038`, `app/chamados/reconciliador.py:524`. |
| Consumidor → premissa morta → `_retratar` | **Não** | `app/chamados/reconciliador.py:1406`, `app/chamados/reconciliador.py:1498`. |
| Consumidor → órfãs pela lâmpada → disputa → S88 | **Não** | `app/chamados/reconciliador.py:1409`, `app/chamados/reconciliador.py:1205`. |
| Consumidor → lastro por marco → `reconciliar` | **Não** | `app/chamados/reconciliador.py:1417`, `app/ponto/services/lastro.py:395`, `app/ponto/services/lastro.py:401`. |
| S143/S128 → resolver perguntas → disputa → S88 | **Não** | S143 chama resolver em `app/chamados/signals.py:117`; candidatos incluem disputas abertas do colaborador em `app/chamados/services/disputa_emissao.py:2026`; fechamento em `app/chamados/services/disputa_emissao.py:2092`. |
| Fábrica → geração → reatar órfã | **Não nos fechos alcançados** | Geração chama reata em `app/chamados/services/disputa_emissao.py:459`; reata chega aos fechos em `app/chamados/reconciliador.py:1653`, `app/chamados/reconciliador.py:1682`. |
| Emissor do cartório → repasse de órfãos → auto-fecho da disputa | **Não** | Emissor chama repasse em `app/ponto/management/commands/detectar_ausencias.py:792`; auto-fecho em `app/chamados/services/disputa_emissao.py:1278`, `app/chamados/services/disputa_emissao.py:1284`. |

Há ainda fechamento por agrupamento LIMBO e por premissa de cluster, ambos sem CSF. O enquadramento desses dois domínios na lei específica de **cobrança sem furo** é **[nao verificado]**. (`app/chamados/services/limbo_folgas.py:284`, `app/ponto/services/cartorio.py:1085`, `app/ponto/services/cluster_espurio.py:190`.)

**S88 e o gate não completam a proteção ausente nos chamadores:** S88 chama a porta; `reconciliar` consulta motor, premissa e pergunta viva, sem CSF. (`app/chamados/signals.py:43`, `app/chamados/models.py:672`, `app/chamados/models.py:683`, `app/chamados/models.py:686`.)

As guardas existentes são diferentes: `_retratar` retém resposta **sem consumo**, enquanto CSF retém qualquer `respondida_em`, inclusive resposta já validada/materializada. C7 pergunta por fio vivo e resposta sem veredito. (`app/chamados/reconciliador.py:448`, `app/chamados/juizes.py:2018`, `app/ponto/supra_juiz.py:545`, `app/chamados/models.py:1843`, `app/chamados/models.py:1854`.)

O FIO-DUP está explicitamente declarado como lacuna em fila no próprio helper; não o apresento como cura prometida pelo delta. (`app/ponto/services/cartorio.py:948`.)

**2. Constância e mordida do teste** [nao verificado]

- **O desenho de consultas em lote existe nos dois helpers.** `_pais_com_conversa` carrega as duas famílias de perguntas; `_fecham_cobrancas_sem_furo` carrega origem/perguntas, recebendo as resolvedoras já carregadas pelo caminho real do cartório. (`app/chamados/reconciliador.py:649`, `app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:128`, `app/ponto/services/cartorio.py:1369`.)
- **A comparação explícita 1 × 3 mede somente `_pais_com_conversa`.** Não mede o E2 completo nem os quatro sítios diretos. O E2 conserva, inclusive, `refresh_from_db` dentro do laço por disputa. (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:281`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:286`, `app/chamados/reconciliador.py:620`.)
- **Um mutante que acrescente uma query por disputa dentro do helper é alcançado pela igualdade:** acrescentaria +1 versus +3 às capturas. Isso é conclusão estática; execução do mutante **[nao verificado]**. Mutação fora desse trecho capturado não é demonstrada por esse teste. (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:275`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:282`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:286`.)
- Os tetos **222/282** estão registrados, mas o selo geral cobra `medido <= teto`; não compara o delta da guarda entre os cenários. CPU e volume de perguntas também não são constantes: o juiz percorre as coleções. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `app/ponto/tests/test_selo_chokepoint_escrita.py:210`, `app/ponto/tests/test_selo_chokepoint_escrita.py:293`, `app/ponto/supra_juiz.py:543`.)

Portanto: **lote nos novos sítios; prova comparativa de constância somente no helper E2**, sem certificação do custo completo do evento. (`app/ponto/services/cartorio.py:962`, `app/chamados/reconciliador.py:649`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:282`.)

**3. Contador `csf_com_conversa_preservado`**

| Aspecto | Escritor/leitor e prova |
|---|---|
| Unidade implementada | `pk` de chamado distinto **por ato**, pelo conjunto em `app/ponto/services/cartorio.py:978`. |
| Incrementador | `_contar_csf_preservado`, em `app/ponto/services/cartorio.py:981`. |
| Chamador direto | Guarda negativa em `app/ponto/services/cartorio.py:966`. |
| Chamador pela cascata | Callback fornecido em `app/ponto/services/cartorio.py:1050`, invocado em `app/chamados/reconciliador.py:626`. |
| Releitura | Compartilha o conjunto e soma somente incrementos novos em `app/ponto/services/cartorio.py:1131`, `app/ponto/services/cartorio.py:1134`. |
| Duração | Conjunto removido no `finally`; o número permanece acumulado no `out` do lote. (`app/ponto/services/cartorio.py:1150`, `app/ponto/services/cartorio.py:977`.) |
| Inicialização | Surge no primeiro incremento; não consta no retorno inicial de `contadores()`. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:2005`.) |
| Leitores | Consolidação da releitura e testes de resultado. (`app/ponto/services/cartorio.py:1133`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:244`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:298`.) |
| Exposição | `julgar_celula` devolve `out`; os resumos dos dois comandos não imprimem esse contador. (`app/ponto/services/cartorio.py:2105`, `app/ponto/management/commands/processar_cartorio.py:66`, `app/ponto/management/commands/lavrar_status_competencia.py:58`.) |

**Defeito de universo:** `_pais_com_conversa` anuncia pai VIVO, mas usa `not in ENCERRADOS`. Isso admite `cancelado` e `superado`, que não são vivos e já seriam recusados por RECONCILIAR. Esses pais podem segurar a disputa e alcançar o contador. Ocorrência real: **[nao verificado]**. (`app/chamados/reconciliador.py:638`, `app/chamados/reconciliador.py:651`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:111`, `app/chamados/reconciliador.py:626`.)

Também há divergência no contrato do callback: a documentação promete uma chamada por pai, mas a implementação chama por disputa preservada. O contador consegue deduplicar essas chamadas; o callback, isoladamente, não oferece essa garantia. (`app/chamados/reconciliador.py:559`, `app/chamados/reconciliador.py:618`, `app/chamados/reconciliador.py:626`, `app/chamados/models.py:1750`, `app/ponto/services/cartorio.py:979`.)

**4. Quem escreve e quem lê**

**É o mesmo juiz**, importado pelos dois helpers e pelo executor supra. C7 permanece outra pergunta, registrada: “a disputa pode fechar?”. (`app/ponto/services/cartorio.py:958`, `app/chamados/reconciliador.py:645`, `app/ponto/management/commands/supra_juiz.py:257`, `app/core/juizes.py:279`.)

| Campo/função | Quem escreve ou define | Quem lê na decisão auditada |
|---|---|---|
| `respondida_em` | Resposta humana, automática e recuperação pela trilha: `app/chamados/services/disputa_emissao.py:1468`, `:1653`, `:1674`; carimbo: `app/chamados/models.py:3133`; reversões: `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1567`. | CSF: `app/ponto/supra_juiz.py:545`, `:548`; C7: `app/chamados/models.py:1845`; retenção: `app/chamados/juizes.py:2020`. |
| `resposta_colab` | `app/chamados/services/disputa_emissao.py:1467`, `:1652`; limpeza em `:1593`. | Isenção FIO-DUP: `app/ponto/services/cartorio.py:1691`; **CSF não lê texto**, lê carimbo em `app/ponto/supra_juiz.py:545`. |
| `disputa_origem` / `Disputa.chamado` | Relação declarada em `app/chamados/models.py:1750`; nascimento em `app/chamados/services/disputa_emissao.py:587`, `:1123`, `:1358`. | CSF: `app/ponto/supra_juiz.py:543`; lote E2: `app/chamados/reconciliador.py:649`; S88: `app/chamados/signals.py:33`. |
| `Disputa.perguntas` / `Pergunta.disputa` | Relação em `app/chamados/models.py:2417`; nascimento em `app/chamados/models.py:3015`. | CSF: `app/ponto/supra_juiz.py:544`; C7: `app/chamados/models.py:1854`. |
| `perguntas_que_resolvem` / `chamado_destino` | Relação em `app/chamados/models.py:3386`; nascimento/reuso em `:3015`, `:2983`; reaponte em `app/chamados/services/fio.py:926`. | CSF: `app/ponto/supra_juiz.py:547`; cartório: `app/ponto/services/cartorio.py:1439`, `:1695`; lote E2: `app/chamados/reconciliador.py:649`. |
| `status_local` | Núcleo `_gravar_estado`: `app/chamados/models.py:228`; nascimento: `app/chamados/models.py:1096`. | Motor: `app/chamados/models.py:674`; filtro novo: `app/chamados/reconciliador.py:651`; S88: `app/chamados/signals.py:34`. |
| `VIVOS` / `ENCERRADOS` | Fonte: `app/chamados/catalogo/motor.py:24`, `:30`. | Cartório usa VIVOS em `app/ponto/services/cartorio.py:1434`; helper E2 usa ENCERRADOS em `app/chamados/reconciliador.py:651`. |
| `fechada_em` | Fecha em `app/chamados/models.py:1933`; reabre em `:2150`, `:2174`. | E2: `app/chamados/reconciliador.py:620`, `:621`; S88: `app/chamados/signals.py:29`. |
| `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição conjunta em `app/chamados/models.py:3098`; persistência do carimbo em `:3153`; reversão em lote em `:3265`. | Seleção E2 em `app/chamados/reconciliador.py:663`; C7 em `app/chamados/models.py:1844`; retenção em `app/chamados/juizes.py:2019`. |
| `pode_executar_csf` | Definição pura em `app/ponto/supra_juiz.py:539`. | Supra em `app/ponto/management/commands/supra_juiz.py:261`; cartório em `app/ponto/services/cartorio.py:965`; E2 em `app/chamados/reconciliador.py:651`. |
| `_pais_com_conversa` | Produz mapa por disputa em `app/chamados/reconciliador.py:650`. | E2 em `app/chamados/reconciliador.py:617`, `:621`, `:626`; teste de custo em `app/ponto/tests/test_o168_guarda_csf_no_evento.py:282`. |
| `_fecham_cobrancas_sem_furo` | Produz lista apenas dos fechamentos confirmados em `app/ponto/services/cartorio.py:968`. | Quatro sítios: `app/ponto/services/cartorio.py:1054`, `:1589`, `:1602`, `:1623`. |
| E2: retorno e `ao_preservar` | Retorno continua número de perguntas carimbadas em `app/chamados/reconciliador.py:607`, `:634`; callback em `:626`. | Cartório em `app/ponto/services/cartorio.py:1050`; consumidor em `app/chamados/reconciliador.py:1399`; reconciliador em `:1466`. |
| `_csf_preservados_vistos` / `CHAVES_DO_ATO` | Criação/adição em `app/ponto/services/cartorio.py:978`; compartilhamento em `:1131`; declaração em `:2001`. | Deduplicação em `app/ponto/services/cartorio.py:979`; drenagem em `:1150`; selo em `app/ponto/tests/test_out_do_lote.py:195`, `:203`. |
| Filas/contadores existentes | Fechos alimentam `reconciliados`/`rejulgar` em `app/ponto/services/cartorio.py:1056`; corpo escreve `cods` em `:1527`. | Segunda passada em `app/ponto/services/cartorio.py:1060`; releitura/consolidação em `:1136`; devolução ao lote em `:1166`. |
| Tetos, registro de linhas e manifesto E4B | Literais alterados em `app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`; `app/core/tests/test_medida_turno_do_par.py:322`; `app/chamados/tests/e4b_c12_sitios.json:261`. | Cobrança do teto em `app/ponto/tests/test_selo_chokepoint_escrita.py:292`; igualdade das linhas em `app/core/tests/test_medida_turno_do_par.py:547`; manifesto em `app/chamados/tests/e4b_c12_verificar.py:17`, `:85`. |

O quadro identifica as portas e consumidores discriminados nesta auditoria; **censo global exaustivo de todos os leitores legados desses campos: [nao verificado]**.

**5. Classes que já reprovaram obra**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando autoridade | **Ausente nos helpers novos:** ambos chamam CSF; C7 continua registrado separadamente. (`app/ponto/services/cartorio.py:965`, `app/chamados/reconciliador.py:651`, `app/core/juizes.py:279`.) |
| Segundo escritor fora da porta | **Ausente no delta lido:** mantém `transicionar_com_pauta`, `carimbar` e `fechar`. (`app/ponto/services/cartorio.py:968`, `app/chamados/reconciliador.py:604`, `:629`.) |
| Relógio fora da borda | **Nenhuma leitura nova nos helpers.** A cadeia preexistente tem relógios em `carimbar`, `fechar` e `julgar_celula`; conformidade global com a borda **[nao verificado]**. (`app/chamados/models.py:3125`, `:1933`, `app/ponto/services/cartorio.py:2067`, `:2091`.) |
| Derivação paralela de fonte única | **Ausente nos helpers; permanece isenção não equivalente no FIO-DUP:** texto nas resolvedoras versus carimbo também na origem. (`app/ponto/services/cartorio.py:1691`, `app/ponto/supra_juiz.py:543`, `:547`.) |
| Selo/RED sem mordida | **Há pares controle/protegido de estado final** para os quatro sítios e cascata; o teste do evento seguinte declara provar somente estabilidade. Execução dos REDs/mutantes **[nao verificado]**. (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:114`, `:122`, `:131`, `:136`, `:143`, `:148`, `:157`, `:162`, `:227`, `:236`, `:259`.) |
| Ordem de trava | **Nenhuma inversão nova identificada:** E2 mantém trava do colaborador antes do carimbo; fechamento chama a porta de ordem. Implementação ordena Colaborador→Disputa→Chamado→Pergunta. (`app/chamados/reconciliador.py:601`, `app/chamados/models.py:1911`, `:2667`, `:2676`, `:2689`, `:2693`.) |
| Pouso antes do pré-requisito | **[nao verificado]**; suíte inteira precede opus, e Codex não aprova sozinho o pouso. (`CLAUDE.md:377`, `CLAUDE.md:380`.) | [nao verificado]
| ESMERIL-SEMÂNTICA | **Presente:** “pai vivo preservado” admite terminais absolutos e pode alimentar o contador. (`app/chamados/reconciliador.py:638`, `:651`, `:626`, `app/chamados/catalogo/motor.py:31`.) |

**6. Leis dos sítios**

- **O168 não está universalmente atendida nos caminhos solicitados:** persistem fechamentos sem consulta à guarda, especialmente lastro no consumidor e cascata S143/S128. (`app/ponto/services/lastro.py:401`, `app/chamados/services/disputa_emissao.py:2092`.)
- **Contador == universo:** há divergência demonstrável entre o rótulo “vivo preservado” e a seleção de pais terminais; é incompatível com o contrato anunciado e com a exigência de o rótulo dizer o que a conta faz. (`CLAUDE.md:35`, `app/ponto/services/cartorio.py:974`, `app/chamados/reconciliador.py:651`.)
- **L-061 e C7:** os helpers novos conservam leitura da célula e consulta aos juízes existentes; não identifiquei novo julgamento paralelo da lâmpada nesses helpers. (`app/docs/LEIS.md:95`, `app/chamados/reconciliador.py:604`, `:628`, `:661`.)
- **RELÓGIO-INJETADO:** o delta não acrescenta relógio; conformidade da cadeia inteira **[nao verificado]**, pelos sítios preexistentes discriminados acima. (`app/docs/CORTES.md:370`, `app/chamados/models.py:3125`, `app/ponto/services/cartorio.py:2067`.)
- **L-073, L-124, L-115 e L-113:** conformidade geral **[nao verificado]**; esta leitura não certifica teto temporal, horizonte, pareamento e cálculo/folha por ausência de alteração nesses mecanismos. (`app/docs/LEIS.md:100`, `:145`, `:151`, `:153`.)
- **E2-EMISSORES-COMPLEMENTO, LEI-TRANCA-E2-JUIZ e DOIS-AGENTES:** nenhuma violação adicional demonstrada nesta leitura; atendimento integral **[nao verificado]**. (`app/docs/CORTES.md:360`, `:427`, `:70`, `:259`.)

## 12/11 16:50 · sessao 151f9586a29f · - · - · origem=Ronald (tmux)

Censo ATA no perímetro literal conferido: **12 chaves de topo, 95 operações R/W, 27 arquivos**; excluindo `management`, **86 operações em 23 arquivos**. Unidade: operação AST com chave literal, incluindo escrita em construtor e em cópia apenas em memória; excluídos cópia integral `*`, chaves dinâmicas, aninhadas e lookup ORM. [não verificado: estatística reproduzida em Python em memória, sem arquivo de resultado]

| Chave | R | W | Tipo produzido | Escritor |
|---|---:|---:|---|---|
| `tipo_dia` | 5 | 1 | string ou `None` | `app/ponto/services/cartorio.py:738:ata_do_dia` |
| `lampadas` | 40 | 3 | lista de dict | `app/ponto/services/cartorio.py:738:ata_do_dia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto`; `app/escala/utils.py:1892:montar_grade_prevista_periodo_por_turno` |
| `minutos_previstos` | 1 | 1 | int | `app/ponto/services/cartorio.py:739:ata_do_dia` |
| `minutos_realizados` | 3 | 1 | int | `app/ponto/services/cartorio.py:740:ata_do_dia` |
| `sem_ancora` | 1 | 1 | bool | `app/ponto/services/cartorio.py:741:ata_do_dia` |
| `n_orfas` | 4 | 1 | int | `app/ponto/services/cartorio.py:742:ata_do_dia` |
| `orfas` | 10 | 2 | lista de strings HH:MM | `app/ponto/services/cartorio.py:745:ata_do_dia`; `app/escala/utils.py:1895:montar_grade_prevista_periodo_por_turno`; formatação em `app/escala/utils.py:1832` |
| `n_missing` | 3 | 3 | int | `app/ponto/services/cartorio.py:746:ata_do_dia`; `app/ponto/services/cartorio.py:812:dispensar_por_ausencia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto` |
| `n_celulas` | 4 | 1 | int | `app/ponto/services/cartorio.py:762:ata_do_dia` |
| `cobertura_parcial` | 3 | 1 | bool, `True` quando presente | `app/ponto/services/cartorio.py:767:ata_do_dia` |
| `periodo_parcial` | 3 | 1 | enum string: `sem_periodo`, `dentro`, `fora`, `nao_sei` | `app/ponto/services/cartorio.py:771:ata_do_dia`; origem `app/ponto/turnos.py:2618` |
| `precedencia` | 1 | 1 | dict | `app/ponto/portas/celula.py:1084:carimbar_precedencia`; conteúdo em `app/ponto/services/cartorio.py:2000` |

Os dois escritores de `app/escala/utils.py` são cópia em memória: a cópia do objeto está em `app/escala/utils.py:1886`; os demais caminhos de persistência passam pela porta que atribui `cel.ata` em `app/ponto/portas/celula.py:1006` e `app/ponto/portas/celula.py:1033`.

`lampadas` contém `tipo`, `hora`, `acesa`, `luz`, `tipo_real`, mais opcionais `isencao`, `dispensada`, `pendente`: construção em `app/ponto/services/cartorio.py:702`, `app/ponto/services/cartorio.py:720`; opcionais em `app/ponto/services/cartorio.py:591`, `app/ponto/services/cartorio.py:596`, `app/ponto/services/cartorio.py:915`. `luz` guarda HH:MM ou `None`; `hora` é marco previsto ou rótulo posicional `·I#`. `app/ponto/services/cartorio.py:703`; `app/ponto/juiz_batida.py:193`

Comparações de hora sem data comprovadas:

- `orfas`: ordenação e distância de minutos em `app/ponto/services/cluster_espurio.py:34` e `app/ponto/services/cluster_espurio.py:35`; pertinência de HH:MM em `app/ponto/services/vigia_pareador_celula.py:110` e `app/ponto/services/vigia_pareador_celula.py:111`.
- `lampadas.hora`: igualdade com hora pedida em `app/ponto/services/lastro.py:90`; igualdade entre marcos ATA/DNA em `app/ponto/juiz_batida.py:230`; comparação com DNA em `app/ponto/services/cartorio.py:848`.
- `lampadas.luz`: resposta `data_hora` perde a data em `app/chamados/juizes.py:1349`; depois compara minutos circulares com a luz em `app/chamados/juizes.py:1358` e `app/chamados/juizes.py:1362`.
- A reconstrução atual da data da luz está em `app/ponto/juiz_batida.py:97` e `app/ponto/juiz_batida.py:215`.

Fora do perímetro literal informado, há leitor ORM de `lampadas.pendente` em `app/chamados/models.py:3356`, projeção `values('ata')` em `app/chamados/reconciliador.py:879` e cópia dinâmica da chave `precedencia` por laço em `app/ponto/portas/celula.py:997`, cuja declaração está em `app/ponto/portas/celula.py:104`.

A diferença aritmética em relação às 72 do aval é **−60 chaves de topo**; a razão da divergência é [não verificado]. A lista completa de leitores por chave e declarações de parâmetros/retornos foi enviada nas mensagens anteriores. [não verificado: registro das mensagens]

Os tipos acima são os produzidos pelo código; o conteúdo efetivamente presente no banco é [não verificado]. A porta aceita `dict(ata)` em `app/ponto/portas/celula.py:995`, e o campo é JSON sem esquema declarado em `app/escala/models.py:1778`.

## 12/11 16:49 · sessao e2e181b5434c · - · - · origem=Ronald (tmux)

Complemento da exploração, só leitura:

1. **Fábrica do consumidor e do cartório alcança a cascata c1 já descoberta.** Consumidor chama fábrica em `app/chamados/reconciliador.py:1421`; fábrica percorre `perguntar_por_eventos` → `perguntar_pela_celula` → `perguntar` em `app/chamados/services/fabrica_da_celula.py:301`, `:219`, `:172-174`. A geração de perguntas chama `reatar_pergunta_orfa` em `app/chamados/services/disputa_emissao.py:459-464`; este pode chamar `lavrar_sem_turno_aberto` em `app/chamados/reconciliador.py:1653`, que fecha disputa sem consultar CSF em `:412-414`. O cartório chama a mesma fábrica em `app/ponto/services/cartorio.py:1076-1088`.

2. **Há cascata adicional S143/S128 → disputa.fechar → S88 sem CSF.** O ramo histórico de reatar chama `resolver_perguntas_por_chamado_resolvido` em `app/chamados/reconciliador.py:1682`; `_retratar` também o chama em `:537`; qualquer save terminal que escreva status acorda S143 em `app/chamados/signals.py:91-117`. O resolver forma candidatos incluindo todas as disputas abertas do colaborador em `app/chamados/services/disputa_emissao.py:2026-2028`; fecha candidatos pelo C7, com veto de pergunta em voo, em `:2089-2092`. Não chama CSF nesse fecho. O filtro de pai/destino vivo em `:2032-2037` limita o **carimbo sem credor**, não o laço final de fechamento em `:2074-2092`.

3. **Emissor chamado pelo cartório alcança outro auto-fecho.** `julgar_celula` injeta `detectar_ausencias.Command` em `app/ponto/services/cartorio.py:2066`, `:2091-2092`; corpo chama `_abrir_chamado` em `:1841`, `:1879`. Esse emissor chama `criar_disputa_a_partir_de_chamados_orfaos` em `app/ponto/management/commands/detectar_ausencias.py:792-803`. A porta de repasse fecha a disputa automaticamente quando C7 permite e não houve abstenção por ata em `app/chamados/services/disputa_emissao.py:1270-1287`, sem CSF.

4. **Agrupamento LIMBO também pode fechar pelo ramo do emissor**, com regra própria: emissor chama `agrupar_furo` em `app/ponto/management/commands/detectar_ausencias.py:759-763`; agrupador reconcilia em `app/chamados/services/limbo_folgas.py:284-287`, depois da costura das vivas e do carimbo de agrupamento (`:275-283`), sem CSF. É agrupamento sob pai vivo, portanto não afirmo que seja fecho “sem furo” abrangido pela lei O168: **[nao verificado]**.

5. **Outro ramo explícito do cartório é cluster:** chama `cluster_espurio.reconciliar_colab` em `app/ponto/services/cartorio.py:1081-1085`; esse reconcilia chamados que a premissa matou em `app/ponto/services/cluster_espurio.py:187-191`, sem CSF. O módulo é `cluster_batida_espuria` (`:11`), diferente da cobrança sem furo tratada no novo helper; aplicabilidade exata da lei O168 a esse domínio: **[nao verificado]**.

6. **Não encontrei fecho direto no dedup de nascimento de pergunta lido.** O reuso atualiza célula/destino e retorna em `app/chamados/models.py:2922-3009`; nascimento grava pergunta e retorna em `:3014-3032`. O núcleo de criação/agregação de disputa reutiliza aberta ou cria nova, sem fechar, em `app/chamados/services/disputa_emissao.py:530-603`. O fecho relevante está nos ramos acima, e não nessas operações de dedup.

São caminhos estruturais com chamada real de fechamento, sujeitos às demais guardas. Ocorrência concreta, resultado de execução e existência de dados que atravessem todas as condições: **[nao verificado]**.

## 12/11 16:49 · sessao b4c3d76f5cee · - · - · origem=Ronald (tmux)

DNA: 9 chaves de topo escritas pelo produtor; `DNA_CHAVES` declara apenas 8 e omite `extra_declarada`. Referências: `app/ponto/management/commands/gerar_celulas.py:17`, `:95`, `:122`.

| Chave | Escritor em `gerar_celulas.py:montar_dna` | Tipo produzido | Leitor por chave |
|---|---|---|---|
| `dna_versao` | :96 | int, atualmente 2 (:16) | `app/ponto/services/cartorio.py:484:impressao_insumos` |
| `vinculo_id` | :97 | ID do vínculo | Nenhum acesso por chave identificado; blob inteiro é comparado em `app/ponto/portas/celula.py:263`. |
| `tipo_escala_id` | :98 | ID do tipo | Nenhum acesso por chave identificado; comparação do blob em `app/ponto/portas/celula.py:263`. |
| `tipo_ciclo` | :99 | string (`app/escala/models.py:117`) | `app/ponto/services/vigia_entrada.py:129:do_dia` |
| `marcos` | :100 | dict ou null (:77,:83) | 26 acessos AST, lista abaixo. |
| `minutos_jornada` | :101 | int (`app/escala/models.py:49`) | Nenhum acesso por chave identificado; comparação do blob em `app/ponto/portas/celula.py:263`. |
| `intervalo_livre` | :102 | bool | Nenhum acesso por chave identificado; comparação do blob em `app/ponto/portas/celula.py:263`. |
| `lampadas` | :103 | list[dict] ou null (:78,:94) | `app/escala/utils.py:153:lampadas_de_dna` |
| `extra_declarada` | :122 | dict `{antes:int,depois:int}` (:119,:120) | `app/escala/models.py:1235:extra_declarada_do_dia` |

Esses são os tipos produzidos pelo código; conteúdo efetivamente armazenado em produção: [não verificado].

Leitores de `marcos`, contando chamadas AST e preservando duas chamadas na linha 159:

```text
app/chamados/juizes.py:948:fato_do_marco_no_espelho
app/chamados/juizes.py:1338:_resposta_concorda_com_a_luz
app/chamados/services/adesao.py:36:universo_adesao
app/escala/models.py:1124:marcos_do_dia
app/escala/models.py:1189:pausas_do_dia
app/escala/utils.py:53:pausas_dna_periodo
app/escala/utils.py:104:marcos_dna_periodo
app/escala/utils.py:137:pausas_de_celulas
app/pautas/services.py:424:contexto_do_dia_texto
app/ponto/juiz_batida.py:159:periodos_do_dia [2 chamadas]
app/ponto/management/commands/aplicar_col900.py:172:handle
app/ponto/management/commands/censo_col900.py:118:handle
app/ponto/management/commands/censo_col900.py:119:handle
app/ponto/management/commands/detectar_intervalo_ausente.py:53:furos_intervalo
app/ponto/management/commands/tripwire_celulas.py:99:medir_pausa_faltando
app/ponto/management/commands/tripwire_celulas.py:259:medir_sobra_com_tipo_trocado
app/ponto/services/cartorio.py:484:impressao_insumos
app/ponto/services/cartorio.py:615:lampadas_do_dia
app/ponto/services/cartorio.py:840:insumos_da_ata
app/ponto/services/dia_do_colab.py:46:dia_do_colab
app/ponto/services/diagnostico_escala.py:153:diagnostico_escala
app/ponto/services/esmeril_espelho.py:116:assinaturas_do_colab
app/ponto/services/faltas_de_hoje.py:43:faltas_de_hoje_sem_cobranca
app/ponto/services/veredito_celula.py:31:_marcos_do_dia
app/ponto/services/vigia_entrada.py:126:do_dia
```

As subchaves `marcos.hi/hf/hii/hfi` contêm HH:MM ou null; `hii2/hfi2` são opcionais. Escritas: `app/ponto/management/commands/gerar_celulas.py:83`, `:93`; vocabulário: `app/escala/models.py:1652`.

`lampadas[]` contém `{chave,tipo,hora,instante}`: `hora` é HH:MM; `instante` é ISO **sem fuso**, obtido por `datetime.combine(...).isoformat(...)`. Referências: `app/ponto/management/commands/gerar_celulas.py:68`, `:69`.

Comparação de hora sem data existe para `marcos`:

```text
app/ponto/services/cartorio.py:640:lampadas_do_dia [pertinência]
app/ponto/services/cartorio.py:648:lampadas_do_dia [chave de dict]
app/ponto/services/cartorio.py:650:lampadas_do_dia [consulta de dict]
app/ponto/services/cartorio.py:657:lampadas_do_dia [consulta de dict]
app/ponto/services/cartorio.py:666:lampadas_do_dia [distância em minutos]
app/ponto/services/cartorio.py:846:insumos_da_ata._um [igualdade; hi/hf passados em :855]
app/chamados/juizes.py:965:fato_do_marco_no_espelho [chave de dict]
app/chamados/juizes.py:968:fato_do_marco_no_espelho [consulta de dict]
app/chamados/juizes.py:1345:_resposta_concorda_com_a_luz [igualdade]
app/ponto/juiz_batida.py:230:periodos_do_dia [igualdade dos minutos]
app/ponto/services/vigia_entrada.py:127:do_dia [comparação hi-hf]
app/ponto/services/dia_do_colab.py:47:dia_do_colab [pertinência]
app/ponto/management/commands/censo_col900.py:120:handle [igualdade do dict]
```

Também existe para `lampadas`: `app/escala/utils.py:155:lampadas_de_dna` usa `lamp['hora']` como chave de dict.

Comparações do blob inteiro incluem `app/ponto/portas/celula.py:263`, `:594`, `:872` e `app/ponto/management/commands/tripwire_celulas.py:30`; portanto ausência de leitor individual não significa ausência de uso.

Mapa para o instrumento:

```text
PRODUTOR:
app/ponto/management/commands/gerar_celulas.py:73:montar_dna
  retorna dna em :123

PARÂMETROS DECLARADOS DNA:
app/escala/utils.py:149:lampadas_de_dna                 posição0 dna
app/ponto/services/cartorio.py:471:impressao_insumos    posição3 dna
app/ponto/services/cartorio.py:599:lampadas_do_dia      posição2 dna
app/ponto/services/cartorio.py:730:ata_do_dia           posição2 dna
app/ponto/services/cartorio.py:816:insumos_da_ata       posição1 dna

DESEMPACOTAMENTO ORM:
app/escala/utils.py:52:pausas_dna_periodo
app/escala/utils.py:103:marcos_dna_periodo
app/escala/utils.py:136:pausas_de_celulas
app/escala/utils.py:164:lampadas_dna_periodo
app/chamados/services/adesao.py:33:universo_adesao

APELIDO DE RETORNO:
app/ponto/management/commands/censo_col900.py:115:handle
  cand = montar_dna(...); leitura cand.get('marcos') em :118
```

`alvo` em `app/ponto/portas/celula.py:783` vem de `dna_anterior`; a restauração escreve o blob inteiro em `:786`, lê `marcos` em `:788` e `vinculo_id` em `:789`. Esses leitores não foram contados como acessos ao DNA atual.

Contagem AST limitada aos acessos explícitos às nove chaves de topo, em `app/**/*.py`, sem testes nem migrations: 30 leitores em 17 arquivos + 9 escritores em um arquivo adicional = 39 acessos em 18 arquivos; subtotal management = 15; restante = 24. Os locais acima e o literal escritor em `app/ponto/management/commands/gerar_celulas.py:95` sustentam a contagem. Não inclui blob inteiro, DNA anterior ou chaves aninhadas.

## 12/11 16:48 · sessao 345dbb2b82ed · TRANCA-VIVAS-08 · sonda · origem=Code

**O dado recebido não lista nenhuma VIVA para autopsiar; as quatro citadas no aval não vêm acompanhadas de PKs ou campos individuais. A causa delas permanece [não verificado].** A exceção para toque humano posterior à tranca é lei expressa na cópia, mas isso não prova que tenha ocorrido nesses quatro casos. [app/docs/CORTES.md:83](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/docs/CORTES.md:83)

**1. Saída da sonda**

Não reexecutei a sonda nesta sessão. O mecanismo `--sonda` executa o envelope antes de chamar este leitor e entrega a saída raspada no prompt. [bin/codex_pergunta.sh:15](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/bin/codex_pergunta.sh:15), [bin/codex_pergunta.sh:325](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/bin/codex_pergunta.sh:325)

Segue a transcrição integral da saída fornecida; execução e valores não foram verificados independentemente. [não verificado]

```text
sonda_leitura: sonda_vivas08.py [796d8c68faf664cb] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T16:38:40.927867-03:00
FROTA PF vivos=15 vivas_apesar_da_tranca total=0
  emp2 04/2026 PF#7 fechado_em=2026-11-12 18:05:50.735140+00:00 vivas=0 []
  emp2 05/2026 PF#8 fechado_em=2026-11-12 18:06:50.433372+00:00 vivas=0 []
  emp2 06/2026 PF#9 fechado_em=2026-11-12 18:07:49.785772+00:00 vivas=0 []
  emp2 07/2026 PF#10 fechado_em=2026-11-12 18:09:18.110150+00:00 vivas=0 []
  emp2 08/2026 PF#3 fechado_em=2026-09-16 12:11:43.807307+00:00 vivas=0 []
  emp3 04/2026 PF#11 fechado_em=2026-11-12 18:10:56.216540+00:00 vivas=0 []
  emp3 05/2026 PF#12 fechado_em=2026-11-12 18:11:08.343652+00:00 vivas=0 []
  emp3 06/2026 PF#13 fechado_em=2026-11-12 18:11:20.561701+00:00 vivas=0 []
  emp3 07/2026 PF#4 fechado_em=2026-09-16 12:11:43.836459+00:00 vivas=0 []
  emp3 08/2026 PF#5 fechado_em=2026-09-16 12:11:43.853839+00:00 vivas=0 []
  emp4 04/2026 PF#14 fechado_em=2026-11-12 18:11:33.013144+00:00 vivas=0 []
  emp4 05/2026 PF#15 fechado_em=2026-11-12 18:11:35.049693+00:00 vivas=0 []
  emp4 06/2026 PF#16 fechado_em=2026-11-12 18:11:59.631123+00:00 vivas=0 []
  emp4 07/2026 PF#2 fechado_em=2026-07-31 18:45:51.703839+00:00 vivas=0 []
  emp4 08/2026 PF#6 fechado_em=2026-09-16 12:11:43.867520+00:00 vivas=0 []
total_erros=0
```

O texto reporta `total_erros=0`, portanto não apresenta a condição `total_erros>0` indicada no pedido. A cobertura efetiva da sonda, sem seu código-fonte, permanece [não verificado].

**2. Degraus, competência e exceção do admin**

Não existe linha `VIVA` no recorte para atribuir um retorno concreto de `_pergunta_viva_calc`. Atribuir resposta, conversa, veredito ou toque às quatro históricas seria [não verificado].

A ordem comprovada no código é:

| Etapa | Resultado e condição |
|---|---|
| Validação | `validada_em` preenchido devolve `False`. [juizes.py:1391](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1391) |
| Tranca | `dia_da_pergunta_trancado(p)` verdadeiro devolve `False`, **antes da resposta**. [juizes.py:1395](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1395) |
| Respondida | Havendo carimbo e texto, sem decisão explícita do admin na célula, devolve `True` quando a concordância com a luz não é comprovadamente verdadeira. [juizes.py:1405](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1405), [juizes.py:1419](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1419) |
| Célula acusando | Superadas as guardas anteriores, `veredito` em `VEREDITOS_ACUSAM` devolve `True`. [juizes.py:1440](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1440) |
| Falta pela lei | Dia dentro da janela viva e `classificar_falta(...)[1]` não nulo devolvem `True`. [juizes.py:1454](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1454) |
| Sem materialização | Superados os anteriores, `materializada_em is None` devolve `True`. [juizes.py:1466](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1466) |
| Testemunha externa | Último retorno é a negação de `_pergunta_materializada_de_fato`; portanto devolve `True` quando essa autoridade devolve falso. [juizes.py:1473](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1473) |

A tranca pode não calar porque:

- **É conversa:** `_dia_de_turno_da_pergunta` devolve `None`, mesmo havendo `data_evento` civil. [juizes.py:334](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:334)
- **Faltam dia, colaborador ou empresa:** o par não entra na avaliação e conserva `False`. [juizes.py:1023](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1023)
- **Não há PF vivo para a competência calculada:** somente PF encontrado segue para a decisão do toque. [juizes.py:1062](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1062)
- **Há toque humano posterior reconhecido:** o resultado da tranca é `not tocou`. [juizes.py:1074](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1074)

“Humano” nesse predicado significa `LogAuditoria.usuario_id` não nulo e diferente do usuário de sistema, com `criado_em` **estritamente maior** que `PF.fechado_em`. As ações reconhecidas são cobrar aquele dia, resolver a célula daquele dia ou desarquivar chamado ligado à célula daquele colaborador/dia. O predicado não exige consulta a `is_staff` nesses filtros. [juizes.py:1252](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1252), [juizes.py:1261](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1261), [juizes.py:1287](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1287), [juizes.py:1300](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1300)

Isso é **exceção legal ao silêncio daquele dia**, registrada no C20; não reabre o PF. A reabertura do período ocorre em outra porta, escrevendo `reaberto_em`. [core/juizes.py:156](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/core/juizes.py:156), [fechamento.py:1432](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1432)

A comparação com `medir()` revela duas questões distintas:

| Questão | Constatação |
|---|---|
| Pergunta de fato | O juiz lê `data_evento` como dia do turno. Tanto sua `janela_atual` quanto a `janela_fechamento` de `medir()` usam o corte cadastrado da empresa; não são dois cálculos independentes de competência. [juizes.py:338](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:338), [ponto/janelas.py:54](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/janelas.py:54) |
| Conversa | `medir()` seleciona por `data_evento` bruto, sem excluir `conversa`; a tranca devolve dia `None` para conversa. **Há diferença comprovada de universo e significado nesse caminho.** [competencia_trancada.py:107](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:107), [juizes.py:338](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:338) |
| Contador | `medir()` acrescenta VIVAs a `vivas_apesar_da_tranca` somente no ramo de destino/pai resgatado. Fora dele, acrescenta apenas as mortas a `perguntas_lidas_trancadas`; uma viva não recebe entrada nesse contador. Logo seu zero não certifica sozinho todas as perguntas selecionadas. [competencia_trancada.py:119](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:119), [competencia_trancada.py:130](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:130) |

Portanto: **toque posterior reconhecido é lei; interpretar o dia civil de conversa como competência de fato seria ESMERIL-SEMÂNTICA entre esses leitores.** Qual dessas situações explica as quatro perguntas — e se a sonda usa esse contador de `medir()` — permanece [não verificado].

**3. Quem escreve e quem lê os campos**

O mapa abaixo identifica os campos diretos do C6 e da tranca; os leitores citados são os pertinentes à decisão investigada. [juizes.py:1389](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1389), [juizes.py:1004](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1004)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `data_evento` | Atribuição em `fio.gravar_dia_do_fato`; nascimento salva em `_nascer`. [fio.py:801](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/fio.py:801), [models.py:3017](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3017) | Discriminador do turno e filtro de `medir`. [juizes.py:338](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:338), [competencia_trancada.py:108](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:108) |
| `conversa` | `abrir` passa falso; `abrir_conversa` passa verdadeiro e dia civil; `_nascer` persiste. [models.py:2790](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:2790), [models.py:2802](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:2802), [models.py:3015](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3015) | `_dia_de_turno_da_pergunta`. [juizes.py:338](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:338) |
| `validada_em` | Validação, devolução e `carimbar(resposta='validar')`. [validacao.py:134](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/validacao.py:134), [validacao.py:199](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/validacao.py:199), [models.py:3136](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3136) | Projeção temporal e primeira guarda. [juizes.py:1375](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1375), [juizes.py:1391](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1391) |
| `resposta_colab`, `respondida_em` | Resposta humana, automática e recuperação pela trilha; `carimbar` também preenche o timestamp. [disputa_emissao.py:1467](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/disputa_emissao.py:1467), [disputa_emissao.py:1652](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/disputa_emissao.py:1652), [disputa_emissao.py:1674](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/disputa_emissao.py:1674), [models.py:3133](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3133) | Projeção e ramo da respondida. [juizes.py:1375](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1375), [juizes.py:1407](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1407) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, portas que salvam, limpeza em lote e alteração isolada da via por fato no espelho. [models.py:3098](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3098), [models.py:3153](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3153), [models.py:3265](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3265), [models.py:3276](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3276) | Guardas de via, marco e materialização. [juizes.py:1402](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1402), [juizes.py:1428](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1428), [juizes.py:1466](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1466) |
| `motivo`, `tipo_resposta`, `celula`, `disputa` | Nascimento da pergunta; reuso pode reapontar a célula. [models.py:3015](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3015), [models.py:2967](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:2967) | Marco, comparação da resposta e colaborador da tranca. [juizes.py:941](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:941), [juizes.py:1332](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1332), [juizes.py:991](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:991) |
| Célula `veredito`, `veredito_via` | Porta `lavrar_veredito`. [celula.py:1035](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1035) | Decisão do admin e acusação do dia. [juizes.py:1405](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1405), [juizes.py:1438](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1438) |
| Célula `dna.marcos`, `ata.lampadas` | Portas de geração/edição do DNA e lavra da ata; cartório marca pendência temporal. [celula.py:260](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:260), [celula.py:604](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:604), [celula.py:1033](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1033), [cartorio.py:915](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:915) | Lê `hora`, `tipo`, `acesa`, `luz` e `pendente`. [juizes.py:948](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:948), [juizes.py:1344](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1344), [lastro.py:148](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/lastro.py:148) |
| Empresa `dia_inicio_competencia` | CRUD repassa às portas de estrutura, que persistem os campos. [colaboradores/views.py:520](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/colaboradores/views.py:520), [estrutura.py:12](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/colaboradores/services/estrutura.py:12) | `corte_da_empresa`, usado pelas duas janelas. [janelas.py:50](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/janelas.py:50) |
| PF `empresa_id`, `mes`, `ano`, `reaberto_em` | `trancar_periodo` cria/retranca; `reabrir_periodo` grava reabertura. [fechamento.py:1408](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1408), [fechamento.py:1417](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1417), [fechamento.py:1439](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1439) | `periodo_trancado`: exige `reaberto_em IS NULL`; `ativo` não decide. [fechamento.py:1464](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1464) |
| PF `fechado_em` | Automático na criação; atualizado na retranca. [ponto/models.py:926](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/models.py:926), [fechamento.py:1415](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1415) | Limite do toque e comparação com resgate. [juizes.py:1070](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1070), [competencia_trancada.py:105](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:105) |
| Chamado `resgatado_em` | `arquivo.levantar` conserva o primeiro carimbo. [arquivo.py:191](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/arquivo.py:191), [arquivo.py:205](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/arquivo.py:205) | `medir` monta `resgatados`; esse campo pode representar mão humana **ou do colaborador**, portanto não prova sozinho toque de admin. [competencia_trancada.py:105](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:105), [arquivo.py:180](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/arquivo.py:180) |
| Log `usuario`, `acao`, `modelo`, `objeto_id`, `descricao`, `criado_em` | Escritor comum `core.observ.evento`; timestamp automático do modelo. [observ.py:134](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/core/observ.py:134), [core/models.py:307](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/core/models.py:307) | Filtros de humano, ação, alvo e instante em `_toque_humano_nos_dias`. [juizes.py:1252](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1252) |

Os produtores concretos das três ações reconhecidas são `cobrar_dia_manual`, `veredito_celula` e a porta de desarquivar que chama `arquivo.levantar`. [cobrar_dia.py:39](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/cobrar_dia.py:39), [veredito_celula.py:356](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/veredito_celula.py:356), [views_arquivar.py:77](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/views_arquivar.py:77)

O último degrau também consulta testemunhas externas de batida/turno; o mapa acima não é um censo transitivo completo dessas dependências. [juizes.py:662](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:662), [juizes.py:752](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:752)

## 12/11 16:48 · sessao c2950311afaf · LASTRO-FAIXA-SEM-DEFAULT · leitura · origem=Code

A troca precisa começar antes da lavra: hoje a grade reduz a órfã a `HH:MM`; o cartório recebe essa string e a grava. Alterar apenas a serialização em `ata_do_dia` não recupera data, segundos nem offset. (`app/escala/utils.py:621`, `app/escala/utils.py:662`, `app/ponto/services/cartorio.py:745`)

Usei o hash informado como identificação da cópia; sua igualdade com o HEAD de `main` é [não verificado]. Os anexos foram lidos na mensagem; sua correspondência com arquivos locais é [não verificado].

1. **Fonte e escritores atuais**

A fonte da lavra é `ata_do_dia`, em [cartorio.py:730](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:730). Ela grava `n_orfas=len(...)` e `orfas=[str(h)...]`; `CelulaDia.ata` é um `JSONField`. (`app/ponto/services/cartorio.py:742`, `app/ponto/services/cartorio.py:745`, `app/escala/models.py:1778`)

O conteúdo produzido hoje é `list[str]` de **HH:MM, sem data, segundos ou fuso**. Antes desse descarte, a grade tem datetimes provenientes de `timezone.localtime(b.timestamp)`; a configuração da cópia declara `America/Sao_Paulo` e `USE_TZ=True`. O fuso efetivamente ativo numa execução de produção é [não verificado]. (`app/escala/utils.py:621`, `app/escala/utils.py:662`, `app/escala/utils.py:683`, `app/escala/utils.py:1323`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

| Escritor ou transportador da ata | Operação atual |
|---|---|
| Cartório — construção | `ata_do_dia` constrói o dict e escreve `orfas` e `n_orfas`. (`app/ponto/services/cartorio.py:738`, `app/ponto/services/cartorio.py:742`, `app/ponto/services/cartorio.py:745`) |
| Porta — substituição persistente | `lavrar_veredito` atribui a ata inteira nos dois ramos; conserva da ata anterior somente chaves de outra operação, atualmente `precedencia`. (`app/ponto/portas/celula.py:1006`, `app/ponto/portas/celula.py:1033`, `app/ponto/portas/celula.py:997`, `app/ponto/portas/celula.py:104`) |
| Porta — precedência | `carimbar_precedencia` copia a ata, muda `precedencia`, atribui a ata inteira e salva; transporta as órfãs sem interpretá-las. (`app/ponto/portas/celula.py:1083`, `app/ponto/portas/celula.py:1084`, `app/ponto/portas/celula.py:1085`, `app/ponto/portas/celula.py:1087`) |
| Grade — ata sintética | Constrói outra ata, **em memória**, para consultar o juiz do turno; inclui `orfas` e também precisa migrar para ISO. A cópia é explicitamente não salva. (`app/escala/utils.py:1886`, `app/escala/utils.py:1887`, `app/escala/utils.py:1895`) |
| Porta — projeção | `celula_como_o_julgamento_deixa` copia a ata inteira para uma célula de leitura, sem persistência. (`app/ponto/portas/celula.py:933`, `app/ponto/portas/celula.py:945`) |
| Cartório — julgamento corrente | Constrói `SimpleNamespace(ata=...)` e publica a ata transitória do julgamento. (`app/ponto/services/cartorio.py:1436`, `app/ponto/services/cartorio.py:1440`) |
| Cartório — alterações internas | `dispensar_por_ausencia` e `_ata_ate_o_teto` alteram lâmpadas/`n_missing`; não substituem a lista de órfãs nesses atos. (`app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:812`, `app/ponto/services/cartorio.py:913`, `app/ponto/services/cartorio.py:917`) |

O modelo encaminha a lavra à porta. No caminho normal, o cartório chama com `save=False`; `carimbar` efetiva a gravação junto dos demais campos, chegando a `cel.save(update_fields=campos)` e à emissão do evento. (`app/escala/models.py:1819`, `app/escala/models.py:1827`, `app/ponto/services/cartorio.py:1853`, `app/ponto/services/cartorio.py:1873`, `app/ponto/portas/celula.py:1072`, `app/ponto/portas/celula.py:905`, `app/ponto/portas/celula.py:907`)

O censo estático não encontrou outro escritor produtivo de `.ata`, `update(ata=...)` ou `bulk_update` de ata; ausência de escrita por metaprogramação arbitrária é [não verificado]. Os snapshots encontrados leem a ata e escrevem arquivos externos; não são escritores da ata no banco. (`bin/snapshot_o118_col221.py:18`, `bin/snapshot_o118_col221.py:30`, `app/ponto/management/commands/celula_veredito_velho.py:108`, `app/ponto/management/commands/celula_veredito_velho.py:112`)

Para executar a matriz, proponho mudar o transporte **da própria lista `orfas`** para datetimes aware desde `_match_marcos`; preservar o instante nas promoções de célula para órfã; e serializar `t.isoformat()` somente na construção da ata persistível. A ata sintética deve usar a mesma forma ISO. Não há necessidade de outra lista. Os pontos atuais de perda estão em `app/escala/utils.py:621`, `app/escala/utils.py:662`, `app/escala/utils.py:799`, `app/escala/utils.py:1722`, `app/escala/utils.py:1733`, `app/escala/utils.py:1832`, `app/escala/utils.py:1834`.

Há um consumidor **dentro da própria lavra**: `lampadas_do_dia` transforma órfãs em minutos por fatias de string e procura proximidade de até 30 minutos. Ele também precisa receber instantes e compará-los com marcos absolutos da autoridade existente; passar ISO para `_min` quebraria esse caminho. A invariante já documentada é “acesa e órfã são disjuntas”. (`app/ponto/services/cartorio.py:623`, `app/ponto/services/cartorio.py:628`, `app/ponto/services/cartorio.py:666`, `app/ponto/services/cartorio.py:702`)

2. **Leitor único e desconhecimento**

Já existe algo próximo: `_orfas_da_ata(ata)`, em [leitor_celula.py:180](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/services/leitor_celula.py:180). Hoje ele repassa strings ou fabrica marcadores `'?'` pela contagem; `grade_da_celula` o chama para publicar `dia['orfas']`. (`app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`, `app/escala/services/leitor_celula.py:420`)

Proponho substituí-lo pela assinatura pública abaixo, no mesmo módulo; é o encaixe técnico da porta solicitada sobre o leitor existente. (`app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:420`)

```python
def orfas_da_ata(celula: CelulaDia) -> list[datetime.datetime] | None:
    ...
```

O contrato proposto distingue três situações, sem usar a data da linha para completar o dado. A necessidade da distinção já aparece no leitor atual, que recebe atas com apenas contagem. (`app/escala/services/leitor_celula.py:183`, `app/escala/services/leitor_celula.py:190`)

| Retorno proposto | Significado |
|---|---|
| `[]` | Lista explicitamente vazia e sem contagem positiva contraditória: zero órfãs conhecido. Proposta sobre os campos atuais. (`app/ponto/services/cartorio.py:742`, `app/ponto/services/cartorio.py:745`) |
| `[datetime aware, ...]` | Todos os elementos são instantes ISO completos com offset, preservando segundos e ocorrências. Proposta de substituição da leitura atual. (`app/escala/services/leitor_celula.py:187`) |
| `None` | Sem célula/ata/lista; somente HH:MM; somente contagem positiva; elemento inválido ou sem offset; lista incompleta/contraditória. Nenhum elemento parcial é publicado como lista completa. Proposta para substituir a degradação por `'?'`. (`app/escala/services/leitor_celula.py:190`) |

`None` precisa atravessar grade, tela e PDF sem `or []`, `or set()` ou `bool(...)` que o transforme em ausência conhecida; esses padrões existem hoje nos consumidores. (`app/escala/utils.py:1110`, `app/relatorios/pdf_espelho.py:607`, `app/ponto/supra_juiz.py:379`)

Para consumidores que perguntam **quantidade**, proponho transportar também o escalar já lavrado `n_orfas`, hoje ausente da projeção de `grade_da_celula`. Uma ata antiga pode saber “há duas” sem saber quais instantes; isso não autoriza inventar identidades nem exige uma lista paralela. (`app/ponto/services/cartorio.py:742`, `app/escala/services/leitor_celula.py:418`, `app/escala/services/leitor_celula.py:420`, `app/escala/services/furos_diarios.py:22`)

3. **Migração dos leitores atuais**

O censo por AST foi executado em memória sobre os Python da cópia, complementado por busca dos templates; não foi criado artefato de censo. Reprodutibilidade por arquivo salvo e cobertura de acessos dinâmicos são [não verificado].

Os cinco caminhos pedidos exigem as seguintes mudanças nos sítios atuais:

| Caminho | Hoje | Comparação por instante e render final propostos |
|---|---|---|
| Tela/espelho | Lê `grade_da_celula`, adapta a lista e publica `orfas_grade`. Há caminhos que recorrem ao builder para geometria antiga. (`app/ponto/services/espelho.py:519`, `app/ponto/services/espelho.py:489`, `app/ponto/services/espelho.py:689`, `app/ponto/services/espelho.py:844`) | `orfas_grade` transporta a resposta do leitor único, inclusive `None`. Geometria reconstruída não prova órfã lavrada. O HH:MM fica nos templates, que hoje imprimem `o` diretamente. (`app/templates/ponto/espelho.html:151`, `app/templates/ponto/espelho.html:240`) |
| Calendário | Guarda órfãs em set por `(dia,mês)`; compara `b.timestamp.strftime('%H:%M')` e escreve `b.orfa=True`. (`app/colaboradores/services/calendario.py:380`, `app/colaboradores/services/calendario.py:381`, `app/colaboradores/services/calendario.py:622`, `app/colaboradores/services/calendario.py:625`, `app/colaboradores/services/calendario.py:627`) | Localizar a resposta por data completa; reconhecer a batida por timestamp aware. Desconhecimento não vira `False`. O render final já usa `timestamp|date:"H:i"`. (`app/templates/colaboradores/partials/_calendario_grade.html:121`) |
| PDF — entrada espúria | Remonta a grade e une HH:MM de toda a janela; `_entrada_espuria` testa a entrada por HH:MM. (`app/relatorios/pdf_espelho.py:475`, `app/relatorios/pdf_espelho.py:477`, `app/relatorios/pdf_espelho.py:178`, `app/relatorios/pdf_espelho.py:513`) | Consumir a publicação da tela oriunda da porta; testar `periodo.entrada` contra instantes. Entrada de classificação desconhecida continua desconhecida. Proposta nos mesmos sítios. |
| PDF — marcas e alertas | Remonta a grade novamente, associa por `data_fim - índice`, marca `b.orfa` por HH:MM e suprime alertas por HH:MM. (`app/relatorios/pdf_espelho.py:543`, `app/relatorios/pdf_espelho.py:551`, `app/relatorios/pdf_espelho.py:552`, `app/relatorios/pdf_espelho.py:609`, `app/relatorios/pdf_espelho.py:960`, `app/relatorios/pdf_espelho.py:964`) | Usar a resposta já publicada, comparar `b.timestamp`/`pp.entrada` e preservar o estado desconhecido. A data localiza a célula; não completa o instante. HH:MM final permanece na impressão das marcas. (`app/relatorios/pdf_espelho.py:881`, `app/relatorios/pdf_espelho.py:885`) |
| Dry-run de pontas | Obtém `_orf2`, transforma em set e testa `strftime('%H:%M') in _orfset`. (`app/escala/utils.py:1650`, `app/escala/utils.py:1651`, `app/escala/utils.py:1675`) | `_match_marcos` devolve instantes; o teste passa a `locs[k][0] in _orfset`. Manter a cópia de `consumed` e os raios existentes. (`app/escala/utils.py:1649`, `app/escala/utils.py:1672`, `app/escala/utils.py:1676`) |
| Reconciliação | Reconhece/remove órfã por `_hhmm_t`; a exceção de entrada também intersecta horas do turno e ordena strings. (`app/escala/utils.py:2005`, `app/escala/utils.py:2006`, `app/escala/utils.py:2028`, `app/escala/utils.py:2032`, `app/escala/utils.py:2034`, `app/escala/utils.py:2039`) | Reconhecer e remover **uma ocorrência do instante exato**; conjunto de timestamps do turno; comparação com o instante preservado da célula. Conservar as guardas de negócio, inclusive a exclusão noturna atual dessa exceção. (`app/escala/utils.py:2027`) |
| Grade — formação/conservação | `_match_marcos`, sobras e promoções produzem HH:MM; a ordenação é textual. (`app/escala/utils.py:621`, `app/escala/utils.py:662`, `app/escala/utils.py:799`, `app/escala/utils.py:803`, `app/escala/utils.py:1722`, `app/escala/utils.py:1733`, `app/escala/utils.py:1838`) | Conservar datetimes aware na única lista; ordenar cronologicamente. `_celula_do_marco` precisa conservar o instante em memória para promoções posteriores, pois hoje guarda somente a hora formatada. (`app/escala/utils.py:326`) |

Na reconciliação noturna, a seleção atual da lista de saída usa `cross_meianoite` do juiz e `data_turno + 1`. Isso pode continuar localizando a célula correspondente; não pode servir para fabricar a data de uma órfã HH:MM. (`app/escala/utils.py:1958`, `app/escala/utils.py:1971`, `app/escala/utils.py:1973`)

Há também duas perdas de apresentação: `montar_realizado_grade` devolve `[], []` quando faltam marcos, descartando órfãs; os templates colocam as órfãs dentro do ramo que exige `realizado_grade`. A lista/ignorância precisa sobreviver ao adaptador e renderizar independentemente de haver células realizadas. (`app/escala/utils.py:1084`, `app/escala/utils.py:1110`, `app/templates/ponto/espelho.html:151`, `app/templates/ponto/espelho.html:231`, `app/templates/ponto/espelho.html:240`)

O PDF ainda suprime anomalias procurando **substring HH:MM**. A fonte dessas mensagens já descartou o instante no adaptador do motor; portanto acrescentar `DD/MM` ao filtro não atende à matriz. Proponho preservar no produtor um fato de anomalia com tipo e instante, transportar esse fato e formatar a mensagem no render; mensagens antigas sem identidade não podem ser casadas por texto. (`app/relatorios/pdf_espelho.py:716`, `app/relatorios/pdf_espelho.py:719`, `app/relatorios/pdf_espelho.py:720`, `app/ponto/adaptador_motor.py:33`, `app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:40`, `app/ponto/adaptador_motor.py:42`)

O restante do censo de **leitores diretos da ata**, além de `_orfas_da_ata`, é este:

| Leitor direto | Uso atual e migração necessária |
|---|---|
| `periodos_do_dia` | Copia strings para seu resultado. Passar pelo leitor único e conservar `None`. (`app/ponto/juiz_batida.py:161`, `app/ponto/juiz_batida.py:164`) |
| `_celula_furada` | Testa lista ou `n_orfas`. Retirar o acesso direto à lista; conservar a informação de existência quando a contagem lavrada a prova. (`app/chamados/juizes.py:510`) |
| Materialização | Filtra órfãs HH:MM, mistura com luzes e alimenta o propositor; o plantio também vira HH:MM. Migrar a atribuição para coordenadas temporais que preservem o instante; dado incompleto não absolve por uma lista parcial. (`app/chamados/services/materializacao.py:177`, `app/chamados/services/materializacao.py:181`, `app/chamados/services/materializacao.py:182`) |
| Cluster espúrio | Lê a lista e calcula rajada por minutos extraídos da string. Usar o leitor único e diferenças entre instantes; `None` conserva “não medido”, já previsto nessa API. (`app/ponto/services/cluster_espurio.py:17`, `app/ponto/services/cluster_espurio.py:29`, `app/ponto/services/cluster_espurio.py:31`, `app/ponto/services/cluster_espurio.py:34`, `app/ponto/services/cluster_espurio.py:35`) |
| Esmeril A5 | Agrupa recorrência em faixas de 30 minutos, com pelo menos três ocorrências. Ler instantes pela porta e classificá-los em faixas temporais aware da data real; agregar a categoria da faixa entre dias, preservando a regra de recorrência. HH:MM fica no rótulo final. (`app/ponto/services/esmeril_espelho.py:19`, `app/ponto/services/esmeril_espelho.py:125`, `app/ponto/services/esmeril_espelho.py:128`, `app/ponto/services/esmeril_espelho.py:139`) |
| Vigia pareador/célula | Carrega órfãs diretamente e identifica borda por HH:MM. Carregar pela porta, comparar `b.timestamp` e registrar legado como não medido. (`app/ponto/services/vigia_pareador_celula.py:74`, `app/ponto/services/vigia_pareador_celula.py:109`, `app/ponto/services/vigia_pareador_celula.py:111`) |
| Tripwire de células | Peneira por HH:MM circular; até `confirmada_por_instante` identifica a órfã pela hora antes de conferir distância. Passar o instante exato e compará-lo aos marcos absolutos. (`app/ponto/management/commands/tripwire_celulas.py:165`, `app/ponto/management/commands/tripwire_celulas.py:177`, `app/ponto/management/commands/tripwire_celulas.py:195`, `app/ponto/management/commands/tripwire_celulas.py:264`) |
| Cartão pela célula | `orfas_e_isencao_da_ata` devolve sets de strings. Migrar para a porta ou retirar se confirmado sem chamador produtivo; foi localizada chamada no teste. Completude de chamadas dinâmicas é [não verificado]. (`app/relatorios/cartao_pela_celula.py:277`, `app/relatorios/cartao_pela_celula.py:288`, `app/relatorios/tests/test_cartao_pela_celula.py:166`) |
| Snapshot O118 | Conta células pela lista não vazia; também deve abandonar a leitura direta. Essa ferramenta escreve snapshot quando executada. (`bin/snapshot_o118_col221.py:32`, `bin/snapshot_o118_col221.py:30`) |

Os demais leitores da **projeção `dia['orfas']`** também precisam acompanhar o tipo novo:

| Leitor | Ajuste necessário |
|---|---|
| Classificador de órfãs | Chaves do resultado por instante; rajadas por diferença temporal, substituindo o parser HH:MM. (`app/escala/orfa_classe.py:31`, `app/escala/orfa_classe.py:44`, `app/escala/orfa_classe.py:58`, `app/escala/orfa_classe.py:74`) |
| Propositor | Substituir validação e cluster por HH:MM; preservar o estado já existente de “não avaliável”. A DP atual também recebe strings de horas e precisa de adaptação temporal. (`app/ponto/propositor.py:39`, `app/ponto/propositor.py:44`, `app/ponto/propositor.py:52`, `app/ponto/propositor.py:55`, `app/ponto/propositor.py:112`, `app/ponto/propositor.py:123`) |
| Comando do propositor | Não filtrar os instantes por `_h_ok` de tamanho cinco nem reconvertê-los a HH:MM para chamar a DP. (`app/ponto/management/commands/propor_reorganizacao.py:93`, `app/ponto/management/commands/propor_reorganizacao.py:104`, `app/ponto/management/commands/propor_reorganizacao.py:107`) |
| Ausência parcial | `horas_reais_do_dia` assume explicitamente a data da linha para a órfã. Calcular deslocamento a partir do instante real e da origem temporal declarada; legado permanece desconhecido. (`app/ponto/turnos.py:2649`, `app/ponto/turnos.py:2667`, `app/ponto/turnos.py:2643`) |
| Suprajuiz | Usa existência, quantidade e descrição; formatar somente a descrição. `None` não deve ativar reconstrução pelas batidas. (`app/ponto/supra_juiz.py:379`, `app/ponto/supra_juiz.py:514`, `app/ponto/supra_juiz.py:520`, `app/ponto/supra_juiz.py:521`) |
| Causas da folha | Usa presença de órfãs; distinguir presença provada por contagem de identidade desconhecida. (`app/folha/causas.py:43`, `app/folha/causas.py:100`) |
| Fatos do status | Conta órfãs para `tem_batida`; conservar contagem conhecida, sem transformar desconhecimento em zero. (`app/ponto/status_dia.py:188`, `app/ponto/status_dia.py:190`) |
| Furos diários | Conta para o agregado; transportar a contagem lavrada quando faltar identidade. (`app/escala/services/furos_diarios.py:22`, `app/escala/services/furos_diarios.py:54`) |
| Bordas do realizado | Testa existência; preservar a distinção entre presença conhecida e não medido. (`app/ponto/services/bordas_realizado.py:49`) |
| Simulação | Órfã impede “dia perfeito”; identidade desconhecida não pode provar perfeição. (`app/ponto/services/simular_dia.py:12`) |
| Detector de par relâmpago | Usa quantidade no score; não precisa renderizar hora para contar. (`app/ponto/management/commands/detectar_par_relampago.py:140`) |
| Cartório | Conta as órfãs da grade para `n_bat`, além do consumidor geométrico já listado. (`app/ponto/services/cartorio.py:1426`, `app/ponto/services/cartorio.py:628`) |

As limpezas da grade devem operar sobre a única lista: clip, folga, ausência e justificativa. O transporte legacy também precisa preservar o tipo e a ignorância. (`app/escala/utils.py:962`, `app/escala/utils.py:966`, `app/escala/utils.py:1153`, `app/escala/utils.py:1218`, `app/escala/utils.py:1253`, `app/escala/utils.py:1258`, `app/escala/utils.py:1043`)

Os homônimos de `orfas` referentes a perguntas e autoridades não pertencem a este fluxo de batidas. (`app/chamados/reconciliador.py:1319`, `app/chamados/reconciliador.py:1377`, `app/core/management/commands/gerar_diagrama.py:783`, `app/core/management/commands/gerar_diagrama.py:854`)

4. **O que aproveitar e o que rejeitar de c13–c19**

Servem como **especificação de fronteiras**: preservar o instante antes da 

[truncado em 20000 de 27277 caracteres]

## 12/11 16:47 · sessao e0b666b05179 · - · - · origem=Ronald (tmux)

Matriz das classes, restrita ao código lido:

| Classe | Resultado e prova |
|---|---|
| Segundo juiz novo | **Ausente nos helpers novos**: ambos importam e chamam `pode_executar_csf`, cujo critério é `respondida_em` nas perguntas da origem ou destinadas ao chamado (`app/ponto/services/cartorio.py:958`, `:965`; `app/chamados/reconciliador.py:645`, `:651`; `app/ponto/supra_juiz.py:539`). C7 continua sendo `DisputaSupervisao.pode_fechar` (`app/core/juizes.py:279`; `app/chamados/reconciliador.py:628`). |
| Segundo escritor fora da porta | **Ausente no delta lido**: helper usa `transicionar_com_pauta(... ato=x.reconciliar)`; E2 conserva `carimbar` e `fechar` (`app/ponto/services/cartorio.py:968`; `app/chamados/reconciliador.py:604`, `:629`). A escrita efetiva permanece nas portas (`app/chamados/models.py:228`, `:1933`, `:3153`). |
| Relógio fora da borda | **Nenhuma leitura nova nos helpers/contador** (`app/ponto/services/cartorio.py:956`; `app/chamados/reconciliador.py:644`). Não certificar a cadeia inteira: `carimbar(quando=None)` lê `now()`, `fechar` também; `julgar_celula` lê `localdate()` e `now()` (`app/chamados/models.py:3125`, `:1933`; `app/ponto/services/cartorio.py:2067`, `:2088`). São trechos preexistentes, não introduzidos pelo anexo. A lei limita a borda a view/comando/cron/consumidor OUTBOX (`app/docs/CORTES.md:370`). |
| Derivação paralela da fonte | **Ausente nos helpers novos**, que consultam o juiz; **presente como caminho paralelo preexistente no FIO-DUP**: sua isenção lê texto apenas nas resolvedoras, enquanto CSF lê `respondida_em` também na origem (`app/ponto/services/cartorio.py:1687`, `:1695`, `:1698`, `:1715`; `app/ponto/supra_juiz.py:543`, `:547`). |
| Selo/RED sem mordida | Há pares controle/protegido de estado final para quatro sítios e cascata (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:114`, `:122`, `:131`, `:136`, `:143`, `:148`, `:157`, `:162`, `:227`, `:236`). **O caso do evento seguinte não prova guarda**: começa depois do carimbo, e sua própria documentação o identifica como estabilidade (`:259`). Execução de RED/mutantes [nao verificado]. |
| Ordem de trava | **Nenhuma inversão nova identificada**: helper só lê; E2 mantém atomic + trava do colaborador antes de carimbar; fechar usa `trava_na_ordem` (`app/chamados/reconciliador.py:601`, `:602`, `:604`; `app/chamados/models.py:1911`). Ordem implementada: Colaborador `:2667`, Disputa `:2676`, Chamado `:2689`, Pergunta `:2693`. |
| Pouso antes do pré-requisito | **[nao verificado]**. A regra exige suíte inteira verde antes do opus final e proíbe aprovação isolada pelo Codex (`CLAUDE.md:377`, `:380`). | [nao verificado]
| Campo com dois significados | **Presente: “pai vivo preservado” inclui terminal absoluto.** `_pais_com_conversa` promete pai VIVO, mas usa `status_local not in ENCERRADOS` (`app/chamados/reconciliador.py:638`, `:651`). `cancelado`/`superado` pertencem a TERMINAIS, não a VIVOS nem a ENCERRADOS (`app/chamados/catalogo/motor.py:24`, `:30`, `:31`). Assim, o helper admite esses pais com conversa, impede o fechamento da disputa e chama o contador (`app/chamados/reconciliador.py:621`, `:626`); o cartório contabiliza por esse callback (`app/ponto/services/cartorio.py:1050`, `:981`). Contudo, RECONCILIAR já não poderia fechar esses pais (`app/chamados/catalogo/motor.py:111`, `:161`; `app/chamados/models.py:674`). Ocorrência real desses estados [nao verificado]. |

Leis dos sítios:

- **L-061:** não identifiquei rejulgamento novo da célula nos helpers; E2 mantém leitura da célula julgada e passa pelo juiz/carimbo existente (`app/docs/LEIS.md:95`; `app/chamados/reconciliador.py:604`, `:661`).
- **C7 e juiz único:** conservados nos caminhos novos (`app/docs/JUIZES.md:62`; `app/core/juizes.py:279`; `app/chamados/reconciliador.py:628`).
- **RELOGIO-INJETADO:** nenhum relógio acrescentado; cadeia existente conserva leituras mencionadas acima, portanto não cabe certificar conformidade global (`app/docs/CORTES.md:370`; `app/chamados/models.py:3125`; `app/ponto/services/cartorio.py:2067`).
- **L-073, L-124, L-115 e L-113:** o delta não acrescenta cálculo de teto, horizonte, pareamento ou folha; conformidade geral dessas leis [nao verificado] (`app/docs/LEIS.md:100`, `:145`, `:151`, `:153`).
- **E2-EMISSORES-COMPLEMENTO:** trata dos emissores e precedência no nascimento, não da guarda nova de fechamento; nenhum emissor é alterado no diff fornecido (`app/docs/CORTES.md:360`).
- **DOIS-AGENTES:** os cortes citados referem-se às raias O261/O256 após O206, não estabelecem requisito técnico novo para O168 (`app/docs/CORTES.md:70`, `:259`).

O achado novo comprovado é a divergência **VIVO versus não ENCERRADO**, que afeta tanto a retenção da disputa quanto o significado do contador; quantidade afetada em produção [nao verificado]. (`app/chamados/reconciliador.py:621`, `:638`, `:651`; `app/ponto/services/cartorio.py:974`, `:1050`.)

## 12/11 16:46 · sessao 361cf60ab2b9 · - · - · origem=Ronald (tmux)

2. **Constância SQL da guarda: desenho em lote nos dois helpers; prova 1×3 só em `_pais_com_conversa`.**

- `_pais_com_conversa` carrega em lote `chamado`, disputas/perguntas de origem e perguntas resolvedoras, depois consulta o mesmo juiz: `app/chamados/reconciliador.py:649-651`.
- `_fecham_cobrancas_sem_furo` também faz prefetch da lista inteira para origem/perguntas antes do laço: `app/ponto/services/cartorio.py:959-965`. As resolvedoras já vêm prefetched pelo carregador real: `app/ponto/services/cartorio.py:127-128`; o corpo usa esse carregador em `:1369`, filtra `vivos` em `:1434` e chama o helper nos quatro sítios `:1054`, `:1589`, `:1602`, `:1623`.
- Portanto, as dependências SQL da guarda estão em lote em todos esses sítios, por invocação do helper; o juiz apenas percorre coleções e lê `respondida_em`: `app/ponto/supra_juiz.py:539-550`. Isso não torna CPU, volume de linhas ou evento inteiro constantes: há laços em `:543-548`, e o E2 contém `refresh_from_db` por disputa em `app/chamados/reconciliador.py:618-620`.
- O teste captura **somente** `_pais_com_conversa(lista)` e compara a contagem com 1 e 3 disputas: `app/ponto/tests/test_o168_guarda_csf_no_evento.py:275-286`. Um mutante que acrescente uma query por disputa **dentro desse helper** produz acréscimos +1/+3 e reprova pela igualdade de `:286` — conclusão estática; execução do mutante [nao verificado]. Mutação fora do helper não é coberta por essa captura: `:281-282`.
- Não há comparação equivalente de custo da guarda para os quatro sítios diretos nesse teste: ele importa e mede apenas `_pais_com_conversa` em `:273/:282`. Os tetos gerais apenas exigem `medido <= teto`, sem comparar o delta da guarda: `app/ponto/tests/test_selo_chokepoint_escrita.py:287-298`, `:435-443`. A medição efetiva nesta cópia [nao verificado].

3. **Contador: chamado distinto por ato, acumulado numericamente no lote.**

| Elemento | Prova |
|---|---|
| Incrementador | `_contar_csf_preservado` cria/consulta o conjunto de pk, adiciona o novo pk e incrementa o número: `app/ponto/services/cartorio.py:978-981`. |
| Chamador direto | Guarda negativa chama incrementador antes do `continue`: `app/ponto/services/cartorio.py:965-967`. |
| Chamador E2 | Callback fornecido pelo cartório: `app/ponto/services/cartorio.py:1050`; chamado quando a disputa é preservada: `app/chamados/reconciliador.py:621-627`. |
| Consolidação da releitura | Compartilha o mesmo conjunto e soma somente os incrementos locais: `app/ponto/services/cartorio.py:1131-1135`. |
| Vida do conjunto | Removido no `finally` do ato: `app/ponto/services/cartorio.py:1150`; declarado em `CHAVES_DO_ATO`: `:2001-2002`. |
| Unidade efetiva | O conjunto deduplica por `pk` no ato; como sai no `finally`, não deduplica globalmente entre atos diferentes: `app/ponto/services/cartorio.py:978-981`, `:1150`. |
| Leitor nominal de produção | A consolidação da releitura em `app/ponto/services/cartorio.py:1133-1135`. |
| Exposição genérica | `julgar_celula` devolve `out`: `app/ponto/services/cartorio.py:2105`. |
| Leitores de teste | `app/ponto/tests/test_o168_guarda_csf_no_evento.py:126`, `:140`, `:152`, `:166`, `:234`, `:244`, `:257`, `:298`. |
| Saída operacional | O resumo de `processar_cartorio` não imprime esse campo: `app/ponto/management/commands/processar_cartorio.py:66-70`; o de `lavrar_status_competencia` também não: `app/ponto/management/commands/lavrar_status_competencia.py:58-65`. |
| Inicialização | `contadores()` não declara `csf_com_conversa_preservado`; o campo surge no primeiro incremento: `app/ponto/services/cartorio.py:2005-2021`, `:981`. |

O selo unitário verifica `(7,7,8,7) → 2`: `app/ponto/tests/test_o168_guarda_csf_no_evento.py:293-298`; a cascata exige exatamente 1 e a repetição do ato novamente 1: `:244`, `:257`. O contador registra a retenção pela guarda no ponto de avaliação; certificação de que o chamado permanece vivo após todos os demais caminhos [nao verificado].

## 12/11 16:46 · sessao e2e181b5434c · - · - · origem=Ronald (tmux)

**Cobertura estática da guarda: parcial.** O juiz é `app/ponto/supra_juiz.py:539-550`; barra qualquer `respondida_em` na disputa de origem ou nas resolvedoras, inclusive resposta já validada/materializada.

| Caminho | Guarda CSF? | Prova |
|---|---|---|
| Cartório: adiados | Sim | `app/ponto/services/cartorio.py:1050-1054` → helper `:958-968`. |
| Cartório: RECONCILIA-MARCO | Sim | `app/ponto/services/cartorio.py:1583-1589`. |
| Cartório: MARCO-INEXISTENTE | Sim | `app/ponto/services/cartorio.py:1600-1602`. |
| Cartório: B5.3b | Sim | `app/ponto/services/cartorio.py:1615-1623`. |
| E2 → disputa.fechar → S88 | Sim, antes de fechar a disputa | `app/chamados/reconciliador.py:617-629`; lote chama juiz em `:645-651`. |
| FIO-DUP / FIO-DUP-C8 | Não | Fechos diretos pela porta em `app/ponto/services/cartorio.py:1701` e `:1716`; isenção própria lê apenas texto das resolvedoras, `:1687-1699`. |
| `reconciliar_chamados_colab` → premissa morta → `_retratar` | Não | Encadeamento em `app/chamados/reconciliador.py:1475`, `:1492-1498`, `:519-524`. |
| `reconciliar_chamados_colab` → órfãs → lavrar sem turno → disputa.fechar → S88 | Não | `app/chamados/reconciliador.py:1467`, `:1558-1560`, `:1653`, `:412-414`. |
| Consumidor → E2 | Sim | `app/chamados/reconciliador.py:1399`; proteção central em `:617-651`. |
| Consumidor → absolvidos pela célula → `_retratar` | Não | `app/chamados/reconciliador.py:1402`, `:1034-1043`, `:519-524`. |
| Consumidor → premissa morta → `_retratar` | Não | `app/chamados/reconciliador.py:1406`, `:1492-1498`. |
| Consumidor → órfãs pela lâmpada → disputa.fechar → S88 | Não | `app/chamados/reconciliador.py:1409`, `:1192-1206`. |
| Consumidor → lastro → reconciliar | Não | `app/chamados/reconciliador.py:1416-1417`; `app/ponto/services/lastro.py:388-403`. |

As chamadas de produção do E2 lidas são cartório, consumidor e `reconciliar_chamados_colab`: `app/ponto/services/cartorio.py:1049-1050`; `app/chamados/reconciliador.py:1399`, `:1466`. O receiver da batida chama este último em `app/ponto/signals.py:206-215`; o filho morto também, após commit, em `app/chamados/signals.py:352-371`.

**S88 não tem guarda CSF própria:** recebe qualquer save de disputa fechada, busca pai vivo e pede reconciliação em `app/chamados/signals.py:19-43`. A disputa escreve seu fechamento em `app/chamados/models.py:1933-1937`, disparando esse receiver. Além do E2, os dois sítios automáticos do reconciliador acima chegam à mesma porta em `app/chamados/reconciliador.py:413`, `:1205`.

**As outras guardas não são equivalentes.**

- `_retratar` retém somente respostas sem consumo ou justificativa alheia pendente: `app/chamados/reconciliador.py:438-455`, `:519-524`. O discriminador de respostas exige `materializada_em IS NULL`, `validada_em IS NULL`, `respondida_em IS NOT NULL`: `app/chamados/juizes.py:2018-2020`.
- C7 exige ausência de resposta sem veredito e de fio vivo: `app/chamados/models.py:1843-1854`. A conversa já consumida não é, por definição desse código, a mesma pergunta que C7 responde.
- `reconciliar` passa pelo motor, premissa e pergunta viva, sem chamar CSF: `app/chamados/models.py:642-655`, `:672-694`. A premissa regularizada autoriza antes do dedup em `:739-740`; ausência de vivas autoriza em `:419-421`.
- `transicionar_com_pauta` executa o ato e escreve pautas em atomic; não acrescenta guarda CSF: `app/chamados/services/fecho_runtime.py:156-169`.

Isso prova **caminhos estruturais de fechamento sem consulta ao juiz CSF**; não prova ocorrência concreta em produção nem que uma fixture específica atravessará todas as outras condições: **[nao verificado]**. Nenhum teste foi executado e nenhum banco/log foi consultado: **[nao verificado]**. [nao verificado]

## 12/11 16:45 · sessao bab0c9032b66 · - · - · origem=Ronald (tmux)

Mapa de escrita/leitura dos campos do C6, verificado apenas no código:

| Campos | Quem escreve | Quem lê na decisão |
|---|---|---|
| `validada_em` | `_carimbar_validacao`: `app/chamados/services/validacao.py:134`, salva em `:137`; devolução limpa em `:199`; `PerguntaDisputa.carimbar(resposta='validar')` também atribui em `app/chamados/models.py:3136` | Projeção temporal: `app/chamados/juizes.py:1375`; morte imediata: `:1391` |
| `resposta_colab`, `respondida_em` | Resposta humana: `app/chamados/services/disputa_emissao.py:1467`; resposta automática: `:1652`; recuperação de carimbo pela trilha: `:1674`; `carimbar` também preenche `respondida_em`: `app/chamados/models.py:3133` | Projeção: `app/chamados/juizes.py:1375`; ramo respondida: `:1407`; comparação com luz: `:1346` |
| `materializada_em`, `via_resolucao` | Atribuição conjunta `_escrever_carimbo`: `app/chamados/models.py:3098`; portas persistem em `:3153` e `:3191`; limpeza em lote: `:3265`; mudança apenas da via por fato no espelho: `:3276` | Projeção: `app/chamados/juizes.py:1376`; precedência de via: `:1402`, `:1428`; marco materializado: `:1434`; sem carimbo: `:1466`; testemunha externa: `:1473` |
| `motivo`, `tipo_resposta`, `celula` | Emissor passa motivo/tipo: `app/chamados/services/disputa_emissao.py:444`; `_nascer` monta e salva a pergunta, incluindo célula e demais campos: `app/chamados/models.py:3015` | Motivo e célula para identificar marco: `app/chamados/juizes.py:941`; tipo para comparar resposta/luz: `:1332`; testemunha por tipo: `:706` |
| `celula.veredito`, `celula.veredito_via` | Porta `lavrar_veredito`: `app/ponto/portas/celula.py:974`, atribui em `:1035` e persiste em `:1040` | Decisão explícita do admin: `app/chamados/juizes.py:1405`; acusação do dia: `:1438`; concordância do dia: `:1365` |
| `celula.dna.marcos` | Nascimento: `app/ponto/portas/celula.py:260`; regeneração: `:272`; edição: `:604`; reversão: `:786`; geração de período: `:857`, `:865`, `:875` | Identificação das lâmpadas: `app/chamados/juizes.py:948`; comparação resposta/luz: `:1338` |
| `celula.ata.lampadas` (`hora`, `tipo`, `acesa`, `luz`, `pendente`) | Cartório prepara e entrega a ata: `app/ponto/services/cartorio.py:1851`; `_ata_ate_o_teto` marca `pendente=True`: `:915`; porta persiste ata: `app/ponto/portas/celula.py:1033` | Marco aceso: `app/chamados/juizes.py:965`; luz para comparar resposta: `:1344`; `marcos_pendentes`: `app/ponto/services/lastro.py:148`, chamado pelo C6 em `app/chamados/juizes.py:1452` |

Não afirmar escritor único literal de resposta/validação: há caminhos explícitos adicionais nas referências acima. Para carimbo, `_escrever_carimbo` concentra a atribuição conjunta, mas existem limpeza em lote e mudanças isoladas de via explicitadas acima. `app/chamados/models.py:3098`, `app/chamados/models.py:3265`, `app/chamados/models.py:3276`.

O último degrau delega a `_pergunta_materializada_de_fato`, que lê também testemunhas externas de batida/turno; sua investigação completa excede os campos diretos mapeados aqui. `app/chamados/juizes.py:662`, `app/chamados/juizes.py:708`, `app/chamados/juizes.py:1473`.

_4361 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
