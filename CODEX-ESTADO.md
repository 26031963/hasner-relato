# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 06:45** · publicado em **31/10/2026 06:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 06:45 · sessao 9fffb8394a9e · FIO-ESTADO-LE-O-ALCANCE · verificador · origem=Code

**Há achados no tratamento de falhas e na força do selo. A conjunção de tipos do alcance está correta; o escritor produz o esquema esperado.** (`bin/fio_estado.py:71`, `bin/fio_estado.py:106`, `bin/tests/test_fio_estado.sh:135`, `app/chamados/management/commands/vigiar_fio_morto.py:216`)

**1. Contrato 3, 3’, 4 e 4’.**

Cada condição do 3’:

| Condição | Implementação literal |
|---|---|
| `alcance` é dict | Sim. Ausência, `None`, lista, string e booleano são rejeitados. (`bin/fio_estado.py:79`, `bin/fio_estado.py:80`) |
| `alcance_universo` int > 0 | Sim. Usa `type(v) is int`: rejeita bool, float, string, ausência e `None`; zero e negativos também invalidam. (`bin/fio_estado.py:71`, `bin/fio_estado.py:84`, `bin/fio_estado.py:87`) |
| `alcance_cego is False` | Sim. `True` dá CEGO; ausência, `None` e inteiro `0` não passam como `False`. (`bin/fio_estado.py:82`, `bin/fio_estado.py:89`) |
| `esperado` contém `fio_morto_alcance` | Sim, exige presença. **O valor dessa chave pode ser `None`, bool ou outro tipo**: o 3’ exige presença, e o código também. `esperado` inteiro ausente/`None` vira `{}` e falha. (`bin/fio_estado.py:91`, `bin/fio_estado.py:119`, `bin/fio_estado.py:172`) |
| `censo.fio_morto_alcance` int | Sim. Ausência, `None`, bool, float e string são rejeitados. `censo` ausente/`None` vira `{}`. (`bin/fio_estado.py:71`, `bin/fio_estado.py:93`, `bin/fio_estado.py:118`) |
| `medido_em` legível | Sim para a validação comum: ausência, `None`, vazio e valor não interpretável impedem a manchete numérica. (`bin/fio_estado.py:123`, `bin/fio_estado.py:125`, `bin/fio_estado.py:134`) |
| Velho quando idade > `TETO_H`, sem arredondamento prévio | A comparação usa horas cruas; arredonda depois. Há uma ressalva de fuso descrita na resposta 3. (`bin/fio_estado.py:127`, `bin/fio_estado.py:129`, `bin/fio_estado.py:130`) |

Os desvios encontrados:

- **Erro de leitura não vira ILEGIVEL.** JSON quebrado, como `{`, ou `PermissionError` devolvem `medido=False, ilegivel=False`; `linha()` publica **NUNCA MEDIDO**, inclusive dizendo “não há” arquivo existente. Isso contraria “exceção de leitura = ILEGIVEL”. (`bin/fio_estado.py:106`, `bin/fio_estado.py:107`, `bin/fio_estado.py:164`)
- **“Nunca levanta / gerador não cai” não é garantido:** há `RecursionError` não capturado, detalhado na resposta 3. (`bin/fio_estado.py:105`, `bin/fio_estado.py:106`, `bin/fio_estado.py:111`, `bin/gerar_estado.py:261`)
- **Ausente permanece descrito como amarelo**, com manchete NUNCA MEDIDO; o diff não estabelece o vermelho literal pedido. (`bin/fio_estado.py:26`, `bin/fio_estado.py:165`, `bin/gerar_estado.py:258`)
- **4/4’: a troca do contador tem mordida**, pois os casos cruzados exigem 4 e 0 na primeira linha; a garantia do selo contra falhas de data/esquema é incompleta, conforme resposta 5. (`bin/tests/test_fio_estado.sh:140`, `bin/tests/test_fio_estado.sh:141`, `bin/tests/test_fio_estado.sh:145`, `bin/tests/test_fio_estado.sh:146`, `bin/tests/test_fio_estado.sh:173`)

**2. Quem escreve e quem lê.**

O command grava o JSON por `_carimbar`; `censo_alcance` produz o bloco em memória. O gerador consome apenas `ler → linha`. (`app/chamados/management/commands/vigiar_fio_morto.py:114`, `app/chamados/management/commands/vigiar_fio_morto.py:206`, `bin/gerar_estado.py:260`)

| Chave | Escritor: nome, tipo e forma | Leitor |
|---|---|---|
| `alcance` | Dict integral retornado pelo serviço. (`app/chamados/services/vigia_fio.py:539`, `app/chamados/management/commands/vigiar_fio_morto.py:218`) | Valida o bloco e extrai campos. (`bin/fio_estado.py:79`, `bin/fio_estado.py:138`) |
| `alcance.alcance_universo` | `len(dias)`, int. (`app/chamados/services/vigia_fio.py:553`) | Valida e imprime. (`bin/fio_estado.py:85`, `bin/fio_estado.py:173`) |
| `alcance.alcance_cego` | `not dias`, bool. (`app/chamados/services/vigia_fio.py:554`) | Testa identidade booleana. (`bin/fio_estado.py:82`, `bin/fio_estado.py:89`) |
| `alcance.alcance_de_hoje_pra_frente` | Int, começa zero e incrementa. (`app/chamados/services/vigia_fio.py:540`, `app/chamados/services/vigia_fio.py:605`) | Extrai e imprime. (`bin/fio_estado.py:150`, `bin/fio_estado.py:175`) |
| `censo.fio_morto_alcance` | Int do bloco, copiado para `censo`. (`app/chamados/services/vigia_fio.py:650`, `app/chamados/management/commands/vigiar_fio_morto.py:217`) | Valida e usa na manchete. (`bin/fio_estado.py:93`, `bin/fio_estado.py:174`) |
| `esperado.fio_morto_alcance` | Int `0`, copiado de `ESPERADO_DO_CARIMBO`. (`app/chamados/management/commands/vigiar_fio_morto.py:95`, `app/chamados/management/commands/vigiar_fio_morto.py:211`) | Exige chave e compara valor recebido. (`bin/fio_estado.py:91`, `bin/fio_estado.py:172`, `bin/fio_estado.py:176`) |
| `alarma_em_0` | Lista de strings; atualmente sem alcance, por desenho declarado. (`app/chamados/management/commands/vigiar_fio_morto.py:82`, `app/chamados/management/commands/vigiar_fio_morto.py:92`, `app/chamados/management/commands/vigiar_fio_morto.py:213`) | Consulta pertença para o aviso. (`bin/fio_estado.py:143`, `bin/fio_estado.py:178`) |
| `medido_em` | String ISO com offset, derivada do mesmo `agora` usado na rodada. (`app/chamados/management/commands/vigiar_fio_morto.py:162`, `app/chamados/management/commands/vigiar_fio_morto.py:207`) | Interpreta, calcula idade e imprime. (`bin/fio_estado.py:125`, `bin/fio_estado.py:128`, `bin/fio_estado.py:194`) |

**Recorte indevidamente carimbado: sem achado.** `--empresa` transforma `empresas` em lista; somente `empresas is None` permite gravar. O outro ramo declara que não carimbou. (`app/chamados/management/commands/vigiar_fio_morto.py:152`, `app/chamados/management/commands/vigiar_fio_morto.py:156`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/vigiar_fio_morto.py:223`)

**Sempre inválido por incompatibilidade: sem achado.** Os nomes e tipos coincidem; universo vazio produz legitimamente zero/cego e deve ser rejeitado. A frota do serviço já é a de `empresas_visiveis()`. (`app/chamados/services/vigia_fio.py:513`, `app/chamados/services/vigia_fio.py:553`, `app/chamados/services/vigia_fio.py:555`)

O destino pode ser sobrescrito por `VIGIA_FIO_CARIMBO`; o selo confere somente o default literal. A configuração ativa de produção está `[nao verificado]`. (`app/chamados/management/commands/vigiar_fio_morto.py:110`, `bin/tests/test_fio_estado.sh:119`, `bin/tests/test_fio_estado.sh:122`)

**3. Exceções e número na primeira linha.**

**Sim: JSON válido muito aninhado derruba `ler` e a seção do gerador.** Reproduzi em memória com `'[' * 10000 + '0' + ']' * 10000`: o parser levanta `RecursionError`; nenhum dos dois `except` a captura. Com os outros leitores substituídos em memória, `gerar()` também caiu na chamada do fio. (`bin/fio_estado.py:105`, `bin/fio_estado.py:106`, `bin/fio_estado.py:111`, `bin/gerar_estado.py:261`)

**`linha(ler(JSON))` levantar por formas comuns: sem achado.** Formas que provocam `AttributeError`, `TypeError` ou `KeyError` durante a leitura são convertidas em ILEGIVEL antes da composição. (`bin/fio_estado.py:111`, `bin/fio_estado.py:168`)

**Há manchete numérica com carimbo efetivamente velho se o offset diferir do host.** O leitor remove o offset sem converter o instante. Reprodução: `agora=2026-10-31 12:00` local −03 e `medido_em="2026-10-30T11:00:00+00:00"` significam **28 horas**, mas o leitor calcula **25**, aceita e publica `**fio morto: 0**`. A configuração base usa São Paulo. (`bin/fio_estado.py:127`, `bin/fio_estado.py:128`, `bin/fio_estado.py:129`, `bin/fio_estado.py:173`, `app/config/settings/base.py:93`)

Além disso, **a primeira linha inválida contém números diagnósticos**: universo `0`, contagem inválida `'4'` ou limite `26 h`. Portanto “nenhum número na PRIMEIRA linha”, tomado literalmente, não é cumprido; embora esses exemplos não publiquem uma contagem no título `**fio morto: N**`. (`bin/fio_estado.py:88`, `bin/fio_estado.py:94`, `bin/fio_estado.py:137`, `bin/fio_estado.py:181`)

**4. Regra própria do leitor.**

**Sem achado de novo limiar de negócio, filtro temporal do universo ou derivação paralela do alcance.** A manchete usa o contador recebido e compara com o esperado recebido; “hoje pra frente” é apenas impresso, já separado pelo serviço. As exigências de universo e cegueira são as expressamente pedidas pelo 3’. (`bin/fio_estado.py:85`, `bin/fio_estado.py:89`, `bin/fio_estado.py:172`, `bin/fio_estado.py:174`, `app/chamados/services/vigia_fio.py:604`)

A idade continua sendo derivação local; seu descarte do offset tem o problema descrito acima. (`bin/fio_estado.py:127`, `bin/fio_estado.py:128`)

**5. Selo: ausência de sinal e mutantes sobreviventes.**

- **Traceback pode passar como “sem número”.** `manchete()` mistura stderr e perde o status do leitor; `num()` retorna vazio para silêncio, traceback ou formato inesperado. Casos 12, 13, 15 e 16 exigem somente vazio. (`bin/tests/test_fio_estado.sh:19`, `bin/tests/test_fio_estado.sh:135`, `bin/tests/test_fio_estado.sh:136`, `bin/tests/test_fio_estado.sh:163`, `bin/tests/test_fio_estado.sh:166`, `bin/tests/test_fio_estado.sh:173`, `bin/tests/test_fio_estado.sh:183`)
- **Mutante de crash sobrevivente:** retirar `ValueError` do tratamento da data faz `ontem-de-manha` levantar exceção; o caso 15 continua satisfeito pelo `num()` vazio. (`bin/fio_estado.py:125`, `bin/fio_estado.py:131`, `bin/fio_estado.py:111`, `bin/tests/test_fio_estado.sh:172`, `bin/tests/test_fio_estado.sh:173`)
- **Mutante de segundo limiar sobrevivente:** trocar a leitura de `esp` por `esp = 0`. Todas as fixtures fixam o esperado **do alcance** em zero; o caso do esperado 7 varia somente o contador antigo. A comparação em memória preservou as saídas das fixtures reconstruídas. (`bin/fio_estado.py:172`, `bin/tests/test_fio_estado.sh:48`, `bin/tests/test_fio_estado.sh:49`, `bin/tests/test_fio_estado.sh:105`)
- **Mutantes de tipo sobreviventes:** `_inteiro` com `isinstance(v, int)` aceitaria bool; relaxar a identidade de `alcance_cego` poderia aceitar zero/ausência/`None`. Essas entradas não aparecem nas fixtures. (`bin/fio_estado.py:71`, `bin/fio_estado.py:89`, `bin/tests/test_fio_estado.sh:46`, `bin/tests/test_fio_estado.sh:53`, `bin/tests/test_fio_estado.sh:54`)
- **Caso 19 prova texto, não composição ou tipos:** seus três `grep`s podem continuar verdes com um retorno incompatível de `censo_alcance`. (`bin/tests/test_fio_estado.sh:199`, `bin/tests/test_fio_estado.sh:200`, `bin/tests/test_fio_estado.sh:201`)
- **Mordida efetiva:** os casos 8/9 detectam a volta geral ao contador antigo; o caso 18 exige rc zero, ausência de traceback e ILEGIVEL para as duas formas testadas. (`bin/tests/test_fio_estado.sh:141`, `bin/tests/test_fio_estado.sh:146`, `bin/tests/test_fio_estado.sh:192`, `bin/tests/test_fio_estado.sh:193`, `bin/tests/test_fio_estado.sh:194`)

Execução integral do selo e confirmação dos logs GREEN/RED/M1–M7: `[nao verificado]`. O shell cria e apaga arquivos, contrariando a restrição desta auditoria; as reproduções foram feitas em memória. (`bin/tests/test_fio_estado.sh:32`, `bin/tests/test_fio_estado.sh:71`, `bin/tests/test_fio_estado.sh:98`)

## 31/10 06:43 · sessao abb14f791384 · - · - · origem=Ronald (tmux)

- **Exceção não contida:** JSON válido com 10.000 arrays aninhados levanta `RecursionError` em `json.load`; os `except` não a capturam. A chamada direta do gerador também não tem proteção. Reproduzido em memória, sem arquivos. `bin/fio_estado.py:105`, `bin/fio_estado.py:106`, `bin/fio_estado.py:111`, `bin/gerar_estado.py:261`.

- **Erro de leitura rotulado incorretamente:** JSON sintaticamente inválido, como `{`, produz `NUNCA MEDIDO`, em vez de `ILEGIVEL`: o tratamento devolve `medido=False, ilegivel=False` e `linha` toma primeiro o ramo de ausência. `bin/fio_estado.py:106`, `bin/fio_estado.py:107`, `bin/fio_estado.py:164`.

- **Manchete numérica com medição inválida:** sem achado quanto ao contador `**fio morto: N**`; a conjunção rejeita ausência, `None`, booleanos nas contagens e data inválida/velha. Entretanto, a primeira linha contém números diagnósticos — universo `0`, censo inválido `'4'`, idade limite `26 h` — se “sem número” for interpretado literalmente como nenhuma ocorrência numérica. `bin/fio_estado.py:71`, `bin/fio_estado.py:80`, `bin/fio_estado.py:88`, `bin/fio_estado.py:94`, `bin/fio_estado.py:134`, `bin/fio_estado.py:137`, `bin/fio_estado.py:181`.

- **`esperado.fio_morto_alcance=None`:** aceita e publica o contador com `esperado None`; isso cumpre literalmente o 3', que exige presença da chave, sem especificar seu tipo/valor. `bin/fio_estado.py:91`, `bin/fio_estado.py:172`, `bin/fio_estado.py:173`.

- **`linha(ler(JSON))` com formas comuns:** sem achado de exceção adicional; erros estruturais relevantes são contidos antes da composição. Valores vazios no topo ou em `censo`/`esperado` são normalizados para `{}`. `bin/fio_estado.py:105`, `bin/fio_estado.py:111`, `bin/fio_estado.py:118`, `bin/fio_estado.py:119`, `bin/fio_estado.py:168`.

## 31/10 06:43 · sessao 8ae4b7d542a0 · - · - · origem=Ronald (tmux)

Achados do selo, com leitura e execução apenas em memória:

- **Ausência de sinal aceita como sucesso:** `manchete()` mistura stderr, pega só a primeira linha e perde o status do leitor; `num()` devolve vazio para traceback, silêncio ou qualquer formato inesperado. Só há `set -u`, sem `pipefail`. Os casos universo zero, esperado sem chave, data inválida/vazia e velho/fronteira exigem apenas esse vazio. Assim, nesses casos, uma exceção passa pela exigência “sem número”. (`bin/tests/test_fio_estado.sh:19`, `:135`, `:136`, `:163`, `:166`, `:173`, `:175`, `:179`, `:183`.)

- **Mutante plausível que passa:** trocar `esp = x['esperado'].get('fio_morto_alcance')` por `esp = 0`. Todas as fixtures fixam `esperado.fio_morto_alcance=0`; o caso que varia o esperado para 7 varia somente `fio_morto` antigo. Logo o selo deixa entrar exatamente um limiar próprio na manchete. Reconstruí 18 fixtures em memória e obtive saídas idênticas entre curado e mutante; com alcance medido=7/esperado=7, o mutante acusa indevidamente `FORA DO ESPERADO`. (`bin/fio_estado.py:172`, `:176`; `bin/tests/test_fio_estado.sh:44`, `:48`, `:49`, `:105`, `:106`, `:111`.)

- **Mutante de crash que passa:** retirar `ValueError` do tratamento de data. `medido_em='ontem-de-manha'` passa a levantar `ValueError`, não capturado pelo tratamento externo; seu traceback produz `num()` vazio e satisfaz o caso 15. Verificado em memória. (`bin/fio_estado.py:125`, `:131`, `:111`; `bin/tests/test_fio_estado.sh:135`, `:136`, `:172`, `:173`.)

- **Mutantes de tipo não cobertos:** trocar `type(v) is int` por `isinstance(v, int)` aceitaria bool; relaxar `alcance_cego is False` aceitaria ausência/None/zero conforme a substituição. As fixtures não produzem bool nas contagens nem ausência/None/0 em `alcance_cego`. (`bin/fio_estado.py:71`, `:89`; `bin/tests/test_fio_estado.sh:46`, `:47`, `:53`, `:54`.)

- **Caso 19 é textual:** os três `grep`s verificam fragmentos do command, sem executar a composição nem conferir o retorno de `censo_alcance` e seus tipos. Podem permanecer verdes se o command conservar aqueles textos e a função retornar esquema incompatível. (`bin/tests/test_fio_estado.sh:199`, `:200`, `:201`.)

- **Parte com mordida efetiva:** antigo=0/alcance=4 exige numeral 4 e divergência na primeira linha; antigo=9/alcance=0 exige numeral 0. Um retorno geral à manchete antiga é detectado. O caso 18 também exige rc=0, ausência de traceback e `ILEGIVEL` para as duas formas que realmente testa. (`bin/tests/test_fio_estado.sh:140`, `:141`, `:142`, `:145`, `:146`, `:189`, `:191`, `:192`, `:193`, `:194`.)

Não executei o shell completo porque ele cria e apaga arquivos; a comparação de mutantes foi feita com módulos compilados em memória e `open` substituído por `StringIO`. (`bin/tests/test_fio_estado.sh:32`, `:33`, `:71`, `:98`; execução em memória: `[nao verificado]` como artefato persistente.)

## 31/10 06:43 · sessao 751699ed982a · - · - · origem=Ronald (tmux)

Pergunta 2: **sem achado de incompatibilidade escritor/leitor**; as formas escritas correspondem às lidas. O command monta o registro e é quem grava JSON; `censo_alcance` apenas produz o dict em memória. (`app/chamados/management/commands/vigiar_fio_morto.py:114`, `app/chamados/management/commands/vigiar_fio_morto.py:206`, `app/chamados/services/vigia_fio.py:539`)

| Chave | Quem produz/escreve, nome e tipo | Quem lê |
|---|---|---|
| `alcance` | `censo_alcance` cria dict, retornado e inserido integralmente por `'alcance': alc`. (`app/chamados/services/vigia_fio.py:539`, `app/chamados/services/vigia_fio.py:655`, `app/chamados/management/commands/vigiar_fio_morto.py:218`) | `_alcance` exige dict; `_ler_carimbo` extrai seus campos. (`bin/fio_estado.py:79`, `bin/fio_estado.py:138`) |
| `alcance.alcance_universo` | `len(dias)`, int, inclusive zero. (`app/chamados/services/vigia_fio.py:553`) | Teste de int verdadeiro, `>0`, extração e impressão. (`bin/fio_estado.py:85`, `bin/fio_estado.py:87`, `bin/fio_estado.py:148`, `bin/fio_estado.py:173`) |
| `alcance.alcance_cego` | `not dias`, bool, presente mesmo no retorno antecipado vazio. (`app/chamados/services/vigia_fio.py:554`, `app/chamados/services/vigia_fio.py:555`) | Identidade `is True`/`is not False`. (`bin/fio_estado.py:82`, `bin/fio_estado.py:89`) |
| `alcance.alcance_de_hoje_pra_frente` | int inicial zero, incrementado por dia que entra nessa rota. (`app/chamados/services/vigia_fio.py:540`, `app/chamados/services/vigia_fio.py:604`) | Extraído e impresso sem recalcular. (`bin/fio_estado.py:150`, `bin/fio_estado.py:175`) |
| `censo.fio_morto_alcance` | int inicial zero, incrementado; command copia a mesma chave de `alc` para `censo`. (`app/chamados/services/vigia_fio.py:540`, `app/chamados/services/vigia_fio.py:650`, `app/chamados/management/commands/vigiar_fio_morto.py:216`) | `_alcance` exige int verdadeiro; manchete usa esse valor. (`bin/fio_estado.py:93`, `bin/fio_estado.py:147`, `bin/fio_estado.py:174`) |
| `esperado.fio_morto_alcance` | int literal `0` em `ESPERADO_DO_CARIMBO`, copiado ao registro. (`app/chamados/management/commands/vigiar_fio_morto.py:95`, `app/chamados/management/commands/vigiar_fio_morto.py:211`) | Exige presença; compara valor do próprio carimbo. (`bin/fio_estado.py:91`, `bin/fio_estado.py:172`, `bin/fio_estado.py:176`) |
| `alarma_em_0` | Lista JSON de strings a partir da tupla `ALARMA_EM_0`; atualmente exclui alcance por desenho explícito. (`app/chamados/management/commands/vigiar_fio_morto.py:82`, `app/chamados/management/commands/vigiar_fio_morto.py:92`, `app/chamados/management/commands/vigiar_fio_morto.py:213`) | Converte para lista e consulta pertença para o aviso. (`bin/fio_estado.py:143`, `bin/fio_estado.py:178`) |
| `medido_em` | String ISO com offset, `timezone.localtime(agora).isoformat()`; `agora` é capturado uma vez e alimenta também `hoje` dos censos. (`app/chamados/management/commands/vigiar_fio_morto.py:162`, `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:207`) | `fromisoformat`, idade, teto e impressão. (`bin/fio_estado.py:123`, `bin/fio_estado.py:125`, `bin/fio_estado.py:129`, `bin/fio_estado.py:194`) |

**Recorte: sem achado de caminho normal que carimbe `--empresa` como frota.** `empresas` começa `None`; qualquer argumento truthy vira lista, mesmo lista vazia para ids inexistentes, e o carimbo só ocorre sob `empresas is None`. O outro ramo declara “NAO carimbado”. (`app/chamados/management/commands/vigiar_fio_morto.py:152`, `app/chamados/management/commands/vigiar_fio_morto.py:153`, `app/chamados/management/commands/vigiar_fio_morto.py:156`, `app/chamados/management/commands/vigiar_fio_morto.py:205`, `app/chamados/management/commands/vigiar_fio_morto.py:223`)

A frota do alcance já significa o universo de `empresas_visiveis()` e competências da fonte: empresas ativas, sem CNPJ de diagnóstico; o serviço documenta isso e aplica eventual interseção. Não é filtro acrescentado pelo leitor. (`app/chamados/services/vigia_fio.py:513`, `app/chamados/services/vigia_fio.py:535`, `app/chamados/services/vigia_fio.py:536`, `app/chamados/services/vigia_fio.py:547`, `app/colaboradores/queries.py:78`, `app/colaboradores/queries.py:85`)

**Sempre inválido: sem achado de incompatibilidade de esquema.** Há retorno legítimo vazio com universo zero e cegueira verdadeira, que o contrato manda rejeitar; isso depende do dado, não torna toda rodada inválida. (`app/chamados/services/vigia_fio.py:553`, `app/chamados/services/vigia_fio.py:555`, `bin/fio_estado.py:82`)

O destino real pode ser alterado por `settings.VIGIA_FIO_CARIMBO`; o selo do host verifica apenas o caminho default literal. Isso limita a prova de concordância do caso 6, mas não demonstra divergência de produção. (`app/chamados/management/commands/vigiar_fio_morto.py:110`, `bin/tests/test_fio_estado.sh:119`, `bin/tests/test_fio_estado.sh:122`) Configuração ativa de produção: `[nao verificado]`.

Há testes Django existentes que conferem a fotografia do censo e o esperado, recusam carimbo em recorte e verificam alcance real não zero; não foram executados nesta auditoria de leitura. (`app/chamados/tests/test_o261_vigia_fio.py:780`, `app/chamados/tests/test_o261_vigia_fio.py:787`, `app/chamados/tests/test_o261_vigia_fio.py:810`, `app/chamados/tests/test_o261e_f5_alcance.py:444`) Execução: `[nao verificado]`.

Pergunta 4: **sem achado de novo limiar de negócio, filtro de data do universo ou derivação paralela do alcance.** O leitor compara com `esperado` e imprime o contador e o “hoje pra frente” recebidos; as condições de universo e cegueira são as exigidas pelo 3'. (`bin/fio_estado.py:85`, `bin/fio_estado.py:89`, `bin/fio_estado.py:172`, `bin/fio_estado.py:174`, `bin/fio_estado.py:176`) O recorte temporal de “hoje pra frente” já foi feito no serviço. (`app/chamados/services/vigia_fio.py:604`)

## 31/10 04:46 · sessao 512dd43d114e · O261-E-F1-CLASSE-4 · verificador · origem=Code

**Reprovaria o delta4 pelo rollback que apaga efeitos intencionais de `reabrir()`.** A projeção do julgamento também não garante que a lavra ocorrerá depois das escritas que ela autoriza. (`app/chamados/views_arquivar.py:81`; `app/chamados/models.py:1362`; `app/ponto/services/cartorio.py:1376`; `app/ponto/services/cartorio.py:1520`)

Execução dos testes, RED contra a base e incidência em produção: **[nao verificado]**. [nao verificado]

**1. Contraexemplos encontrados**

- **PROCEDENTE — rollback apaga o redirecionamento da recusa.** Chamado encerrado pela tranca, premissa ainda viva, irmão vivo do mesmo turno: `reabrir()` migra resolvedoras para o irmão, escreve nota e trilha de redirecionamento, depois retorna `False`. O novo `set_rollback(True)` desfaz tudo isso junto com o levantamento do véu. Mesmo sem pergunta migrável, a nota de continuidade e a trilha são escritas incondicionalmente nesse ramo e agora desaparecem. Portanto, “recusou” não equivale a “não escreveu nada”. (`app/chamados/models.py:1359`; `app/chamados/models.py:1362`; `app/chamados/models.py:1365`; `app/chamados/models.py:1368`; `app/chamados/models.py:1374`; `app/chamados/views_arquivar.py:81`)

- **PROCEDENTE — ressuscitação pode persistir sem a lavra prevista.** Contraexemplo: célula gravada `concorde`, julgamento atual acusador, pergunta carimbada sem resposta. A projeção permite ressuscitá-la; o `UPDATE` dos carimbos acontece antes da lavra. Depois disso, uma exceção no callback `write`, chamado em `[FIO-RESSUSCITA]`, impede chegar à lavra. `julgar_colab()` tem `finally`, mas não uma transação envolvendo essas duas escritas. O mapa é limpo; a pergunta pode ficar desmarcada sobre a célula antiga, que continua absolvendo. (`app/chamados/models.py:1434`; `app/chamados/models.py:1442`; `app/chamados/models.py:1485`; `app/ponto/services/cartorio.py:746`; `app/ponto/services/cartorio.py:1402`; `app/ponto/services/cartorio.py:1520`; `app/ponto/services/cartorio.py:854`)

  Há também uma chamada de produção entre ressuscitar e lavrar: `_espelhar_marcos()` grava contexto e nota; falha propagada nesse caminho igualmente impede a lavra posterior. Ocorrência dessa falha em produção: **[nao verificado]**. (`app/ponto/services/cartorio.py:1516`; `app/ponto/management/commands/detectar_ausencias.py:558`; `app/ponto/management/commands/detectar_ausencias.py:563`; `app/chamados/services/nota.py:107`)

- **PROCEDENTE — o toque de desarquivar continua invisível para chamado sem `celula_id`.** Contraexemplo: chamado cujo dia vem do contexto, pergunta em competência trancada, mas chamado sem célula ligada. A trilha de desarquivar é escrita, porém o leitor procura exclusivamente chamados ligados às células daquele dia; esse chamado não entra em `chs`. Antecipar a trilha não resolve esse caso. O teste novo liga explicitamente a célula ao chamado. (`app/chamados/juizes.py:102`; `app/chamados/juizes.py:967`; `app/chamados/juizes.py:968`; `app/chamados/juizes.py:972`; `app/chamados/services/arquivo.py:209`; `app/chamados/tests/test_competencia_trancada.py:188`)

- **PROCEDENTE — segundo resgate após retranca pode não produzir novo toque.** Primeiro resgate: véu limpo e primeiro `resgatado_em` preservado. Retrancar avança `fechado_em`; o resgate antigo deixa de proteger o chamado, que pode ser encerrado novamente sem receber novo véu. No segundo clique, `levantar()` retorna sem escrita nem trilha porque o véu está limpo e já existe `resgatado_em`. Na ausência de outro toque posterior à nova tranca, C6 continua calando a pergunta. É lacuna residual do desenho B. (`app/chamados/services/arquivo.py:193`; `app/chamados/services/arquivo.py:196`; `app/chamados/services/arquivo.py:204`; `app/ponto/services/fechamento.py:1383`; `app/chamados/services/competencia_trancada.py:104`; `app/chamados/models.py:1551`; `app/chamados/juizes.py:965`; `app/chamados/juizes.py:1030`)

**2. Quem escreve e quem lê**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `_tl.vereditos[(colab, dia)]` | Cartório calcula pela função da porta e publica a dupla; limpa no `finally`. (`app/ponto/services/cartorio.py:1214`; `app/ponto/services/cartorio.py:857`) | `veredito_deste_julgamento()` → `celula_deste_julgamento()`. (`app/ponto/services/cartorio.py:64`; `app/chamados/juizes.py:394`) |
| `_tl.atas[(colab, dia)]` | Cartório publica `_ata_agora.ata`; limpa no `finally`. (`app/ponto/services/cartorio.py:1148`; `app/ponto/services/cartorio.py:856`) | `ata_deste_julgamento()` → `celula_deste_julgamento()`. (`app/ponto/services/cartorio.py:55`; `app/chamados/juizes.py:393`) |
| `veredito/veredito_via` da cópia | `celula_deste_julgamento()` atribui a dupla projetada. No registro persistido, a lavra atribui esses campos. (`app/chamados/juizes.py:402`; `app/ponto/portas/celula.py:710`) | C6 lê veredito e via; o coletor da premissa também recebe a cópia, mas sua leitura de veredito limita-se à via `admin`. (`app/chamados/juizes.py:1038`; `app/chamados/juizes.py:1074`; `app/chamados/models.py:1230`) |
| `ata` da cópia | Recebe diretamente o dicionário publicado em `_tl.atas`. Na persistência, a lavra escreve ata e `carimbar_precedencia()` acrescenta precedência pela mesma porta. (`app/chamados/juizes.py:400`; `app/ponto/portas/celula.py:708`; `app/ponto/portas/celula.py:759`) | Guarda do marco, C6 via marcos pendentes, coletor da premissa e `_celula_furada()`. (`app/chamados/juizes.py:886`; `app/chamados/juizes.py:1088`; `app/chamados/models.py:1236`; `app/chamados/juizes.py:443`) |
| `Pergunta._state.fields_cache` | O queryset usa `select_related`; a cópia recebe `_state` e dicionário próprios antes de atribuir `_q.celula`. (`app/chamados/models.py:1424`; `app/chamados/models.py:1430`; `app/chamados/models.py:1431`; `app/chamados/models.py:1434`) | Acesso às relações na avaliação; `_chave_memo_pv()` também consulta explicitamente o cache de `chamado_destino`. (`app/chamados/juizes.py:777`; `app/chamados/juizes.py:778`; `app/chamados/juizes.py:779`) |
| Log `cobrar_dia_manual` | `cobrar_dia()` → `registrar_log()` → `evento()` → `LogAuditoria.objects.create()`. (`app/chamados/services/cobrar_dia.py:37`; `app/core/auditoria.py:22`; `app/core/observ.py:134`) | `_admin_tocou_o_dia_depois()`: usuário humano, posterior à tranca, colaborador e substring `dia=ISO`. (`app/chamados/juizes.py:965`; `app/chamados/juizes.py:969`) |
| Log `desarquivar_chamado` | View → `arquivo.levantar()` → `_trilha()` → o mesmo escritor `evento()`. (`app/chamados/views_arquivar.py:73`; `app/chamados/services/arquivo.py:209`; `app/chamados/services/arquivo.py:137`) | O mesmo leitor de toque, restringindo os objetos aos chamados ligados às células do dia. (`app/chamados/juizes.py:968`; `app/chamados/juizes.py:972`) |

`celula_do_dia_agora()` passa efetivamente a devolver também o veredito projetado. Nos dois leitores anteriores encontrados, isso não muda a decisão normal: `_celula_furada()` usa ata/trabalho; `premissa_regularizada()` só distingue a via `admin`, preservada pela projeção. (`app/chamados/juizes.py:378`; `app/chamados/juizes.py:443`; `app/chamados/models.py:1230`; `app/ponto/portas/celula.py:656`)

Na ressuscitação, a mudança funcional é real e coerente com o objetivo A: tanto o guarda do marco quanto C6 recebem a célula da pergunta com ata/veredito em curso. A ressalva é a falta de garantia de lavra posterior demonstrada acima. (`app/chamados/models.py:1434`; `app/chamados/models.py:1436`; `app/chamados/models.py:1442`; `app/ponto/services/cartorio.py:1520`)

**3. Igualdade entre previsão e lavra**

No caminho que chega normalmente à lavra, com os mesmos insumos, **a dupla final coincide**: publicação e lavra recebem `cel`, `via='cartorio'`, `_vd` e `cods`; a lavra chama a mesma função de derivação. Admin é preservado nos dois caminhos. (`app/ponto/services/cartorio.py:1214`; `app/ponto/services/cartorio.py:1520`; `app/ponto/portas/celula.py:698`; `app/ponto/portas/celula.py:656`; `app/ponto/portas/celula.py:692`)

Isso não significa que sempre haja gravação:

- **`apply_=False`:** publica os mapas, mas não entra na lavra. Um callback `write` pode consultá-los durante os relatórios; consumidor de produção que faça essa consulta: **[nao verificado]**. (`app/ponto/services/cartorio.py:1214`; `app/ponto/services/cartorio.py:1463`; `app/ponto/services/cartorio.py:1501`; `app/ponto/services/cartorio.py:1517`)
- **Sem vivos:** não impede lavrar; a condição final exige apenas `apply_`. (`app/ponto/services/cartorio.py:1517`)
- **Exceção:** pode ocorrer depois da leitura da previsão e do `UPDATE` da pergunta, antes da lavra, conforme o contraexemplo acima. (`app/ponto/services/cartorio.py:1376`; `app/ponto/services/cartorio.py:1402`; `app/ponto/services/cartorio.py:1520`)
- **Admin ou repetição idempotente:** a lavra pode preservar a dupla e escrever apenas ata, ou retornar sem escrever. (`app/ponto/portas/celula.py:692`; `app/ponto/portas/celula.py:697`; `app/ponto/portas/celula.py:705`)
- **`calado`:** somente altera/restaura `_tl.ativo`; não limpa os mapas nem impede diretamente uma chamada explícita ao julgamento. Os signals consultam esse estado para evitar reentrada. (`app/ponto/services/cartorio.py:72`; `app/ponto/services/cartorio.py:77`; `app/chamados/signals.py:174`)

A ata projetada também não é literalmente toda a ata final: a lavra preserva chaves de outra operação, como `precedencia`, enquanto o mapa recebe a ata calculada antes desse merge. Efeito adverso dessa diferença nos leitores desta obra: **[nao verificado]**. (`app/ponto/services/cartorio.py:1148`; `app/ponto/portas/celula.py:94`; `app/ponto/portas/celula.py:684`; `app/ponto/portas/celula.py:686`)

**4. Cópia rasa, vazamento e `save()`**

A troca de FK na pergunta está protegida pela separação explícita de `_state` e `fields_cache`. A simulação retorna IDs; a escrita real acontece por outro queryset, sem salvar `_q` ou sua célula projetada nesses caminhos. (`app/chamados/models.py:1430`; `app/chamados/models.py:1431`; `app/chamados/models.py:1434`; `app/chamados/models.py:1443`; `app/chamados/models.py:1483`; `app/chamados/models.py:1485`)

**DUVIDA — isolamento de objetos mutáveis não é garantido.** A cópia é rasa e a ata projetada é o próprio dicionário do mapa: uma operação como `out.ata['lampadas'].append(...)` alteraria a ata compartilhada; mutação aninhada no DNA compartilhado poderia atingir o original. Não encontrei essas escritas nos consumidores atuais, que percorrem os valores para leitura. (`app/chamados/juizes.py:398`; `app/chamados/juizes.py:400`; `app/chamados/juizes.py:869`; `app/chamados/juizes.py:886`; `app/ponto/services/lastro.py:148`)

A cópia não recebe uma proteção contra persistência: continua sendo uma cópia do model. Um caminho atual que execute `save()` nela: **[nao verificado]**; os consumidores encontrados são o carregador da premissa e a simulação da ressuscitação. (`app/chamados/juizes.py:378`; `app/chamados/models.py:1434`)

**5. Ordem do admin, rollback e cobrança sem escala**

Levantar antes não altera diretamente as guardas de reabertura por `arquivado/resgatado/silenciado`: as guardas consultam veto, decisão administrativa, premissa, agrupamento, irmão e transição, sem ler esses campos do véu. A alteração pretendida atua depois, na ressuscitação que consulta a trilha. (`app/chamados/models.py:1285`; `app/chamados/models.py:1290`; `app/chamados/models.py:1296`; `app/chamados/models.py:1299`; `app/chamados/models.py:1302`; `app/chamados/models.py:1306`; `app/chamados/models.py:1389`)

O rollback abrange as escritas de banco feitas dentro daquele bloco, inclusive levantamento, logs e os efeitos do ramo de redirecionamento. Por isso resolve a compensação do véu, mas introduz a regressão do item 1. Não se deve confundir isso com desfazer atributos Python ou efeitos externos; garantia desses últimos: **[nao verificado]**. (`app/chamados/views_arquivar.py:65`; `app/chamados/views_arquivar.py:81`; `app/chamados/services/arquivo.py:201`; `app/chamados/models.py:1362`)

Em `cobrar_dia`, **sim, fica uma trilha mesmo quando não existe célula**: o log antecede o julgamento, que retorna `None` nesse caso. Não demonstrei violação de contador/selo: o teste existente exige uma linha por toque, explicitamente “não por efeito”. Esse toque poderá ser reconhecido depois se a célula passar a existir, pois o leitor não exige sucesso da cobrança original. (`app/chamados/services/cobrar_dia.py:37`; `app/chamados/services/cobrar_dia.py:40`; `app/ponto/services/cartorio.py:1658`; `app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py:275`; `app/chamados/juizes.py:967`; `app/chamados/juizes.py:969`)

**DUVIDA — trilha obrigatória sobre escritor best-effort.** Ambos os fluxos prosseguem sem confirmar a criação do log; `evento()` pode falhar isoladamente e retornar `None`. Contraexemplo condicional: falha ao inserir a trilha, negócio prossegue, C6 continua enxergando “sem toque”. Ocorrência em produção: **[nao verificado]**. (`app/core/observ.py:105`; `app/core/observ.py:133`; `app/core/observ.py:139`; `app/chamados/services/cobrar_dia.py:39`; `app/chamados/services/arquivo.py:209`; `app/chamados/services/arquivo.py:210`; `app/chamados/juizes.py:1030`)

**DUVIDA — memo anterior ao toque.** Consultar o toque, gravar nova trilha e consultar novamente no mesmo dono de `_tranca_memo` conserva o resultado anterior. A nova ordem não invalida esse cache. Rota HTTP atual que necessariamente execute essa sequência: **[nao verificado]**. (`app/chamados/juizes.py:943`; `app/chamados/juizes.py:947`; `app/chamados/juizes.py:959`; `app/chamados/juizes.py:973`; `app/chamados/services/cobrar_dia.py:37`)

**6. Checklist das classes anteriores**

| Classe | Resultado desta leitura |
|---|---|
| Segundo juiz registrado | Não demonstrado: a ressuscitação consulta o C6 registrado; a previsão reutiliza a derivação chamada pela lavra. (`app/core/juizes.py:238`; `app/chamados/models.py:1442`; `app/ponto/portas/celula.py:698`) |
| Segundo escritor fora da porta | Não demonstrado na persistência nova: a projeção escreve memória; pergunta é atualizada pela porta existente; logs convergem para `evento()`. (`app/chamados/juizes.py:400`; `app/chamados/models.py:1485`; `app/core/observ.py:134`) |
| Relógio fora da borda | Nova leitura de relógio nos trechos acrescentados: **[nao verificado]**. O helper recebe insumos e deriva a dupla sem relógio; o julgamento já recebe `hoje/agora`. (`app/ponto/portas/celula.py:647`; `app/ponto/services/cartorio.py:749`) |
| Derivação paralela | Não demonstrada para o veredito de cartório: previsão e lavra chamam a mesma função. (`app/ponto/services/cartorio.py:1214`; `app/ponto/portas/celula.py:698`) |
| Selo/RED sem mordida | **DUVIDA:** o teste de isolamento não cria pergunta nem exercita a troca de FK/`fields_cache`; verifica atributos da célula. Os testes novos também não cobrem retranca, rollback de redirecionamento ou exceção após ressuscitar. RED executado: **[nao verificado]**. (`app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:185`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:197`; `app/chamados/tests/test_competencia_trancada.py:177`) |
| Ordem de trava | **DUVIDA:** a nova transação mantém a escrita do chamado desde `levantar`; `abrir()` possui trava da pessoa. Um novo ciclo concreto de deadlock causado por este delta: **[nao verificado]**. (`app/chamados/views_arquivar.py:65`; `app/chamados/services/arquivo.py:208`; `app/chamados/models.py:864`) |
| Pouso antes do pré-requisito | **PROCEDENTE:** a pergunta pode ser ressuscitada pela previsão antes de a lavra estar garantida; exceção posterior deixa a escrita sem seu fundamento persistido. (`app/ponto/services/cartorio.py:1376`; `app/ponto/services/cartorio.py:1402`; `app/ponto/services/cartorio.py:1520`) |

## 31/10 04:44 · sessao 9f8804bc43d8 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — regressão: rollback apaga o redirecionamento que `reabrir()` executa intencionalmente ao recusar.** Com irmão vivo do mesmo turno, `reabrir()` migra resolvedoras, escreve nota indicando onde continuar e registra `reabertura_redirecionada`, então retorna `False` (`app/chamados/models.py:1359`, `app/chamados/models.py:1362`, `app/chamados/models.py:1365`, `app/chamados/models.py:1368`, `app/chamados/models.py:1374`). O novo `atomic` + `set_rollback(True)` desfaz esses efeitos também, embora o comentário diga “nada escrito, como antes” (`app/chamados/views_arquivar.py:65`, `app/chamados/views_arquivar.py:72`, `app/chamados/views_arquivar.py:81`). A migração não marca rollback em seu caminho normal: salva o novo destino e retorna sucesso (`app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:847`, `app/chamados/services/fio.py:849`). **Contraexemplo mínimo:** fechado pela tranca com irmão vivo; mesmo sem perguntas migráveis, a nota e a trilha de redirecionamento sempre eram escritas, e agora desaparecem (`app/chamados/models.py:1365`, `app/chamados/models.py:1368`). Persistência anterior demonstrada pelo próprio diff fornecido, que chamava `reabrir()` antes de qualquer `atomic` externo [nao verificado quanto a execução].

2. **PROCEDENTE — lacuna residual: segundo resgate depois de retrancar não gera o novo toque necessário.** `levantar()` guarda o primeiro `resgatado_em`; véu limpo com esse carimbo vira no-op sem trilha (`app/chamados/services/arquivo.py:193`, `app/chamados/services/arquivo.py:196`, `app/chamados/services/arquivo.py:204`). Retrancar atualiza `fechado_em` (`app/ponto/services/fechamento.py:1383`); a regra só protege resgate posterior à nova tranca (`app/chamados/services/competencia_trancada.py:104`) e pode encerrar novamente, sem escrever véu (`app/chamados/models.py:1534`, `app/chamados/models.py:1551`). No segundo clique, não nasce log posterior à nova tranca; C6 continua calando a pergunta apesar da reabertura do chamado (`app/chamados/views_arquivar.py:73`, `app/chamados/views_arquivar.py:80`, `app/chamados/juizes.py:965`, `app/chamados/juizes.py:972`, `app/chamados/juizes.py:1030`). Isso vale também para pergunta pendente, sem novo carimbo de materialização (`app/chamados/juizes.py:1025`).

3. **PROCEDENTE — lacuna residual: toque de desarquivar ignora chamado sem `celula_id`.** O leitor enumera os chamados exclusivamente por célula ligada, embora o juiz do dia reconheça datas pelo contexto (`app/chamados/juizes.py:967`, `app/chamados/juizes.py:968`, `app/chamados/juizes.py:972`, `app/chamados/juizes.py:89`, `app/chamados/juizes.py:102`). **Contraexemplo:** chamado da tranca datado pelo contexto, `celula_id=None`, pergunta em dia trancado; `levantar` registra o resgate, mas essa linha não pertence aos `chs` consultados pelo C6 (`app/chamados/services/arquivo.py:209`, `app/chamados/juizes.py:968`). O novo teste liga explicitamente a célula ao chamado, cobrindo somente o caso favorável (`app/chamados/tests/test_competencia_trancada.py:188`).

4. **DUVIDA — dependência obrigatória construída sobre trilha best-effort.** `cobrar_dia` prossegue sem verificar se o registro foi criado (`app/chamados/services/cobrar_dia.py:37`, `app/chamados/services/cobrar_dia.py:39`); `registrar_log` engole exceções e `evento` usa savepoint próprio, devolvendo `None` se falhar (`app/core/auditoria.py:17`, `app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`). `levantar` igualmente retorna `True` sem confirmar a trilha (`app/chamados/services/arquivo.py:208`, `app/chamados/services/arquivo.py:209`, `app/chamados/services/arquivo.py:210`). **Contraexemplo condicional:** falha isolada na criação de `LogAuditoria`; o negócio continua, mas C6 não reconhece toque e não ressuscita (`app/chamados/juizes.py:969`, `app/chamados/juizes.py:1030`). Ocorrência de tal falha em produção [nao verificado].

5. **DUVIDA — memo não invalidado após toque.** O resultado é cacheado na request, ou no colaborador fora dela, e retorna antes de consultar novos logs (`app/chamados/juizes.py:943`, `app/chamados/juizes.py:947`, `app/chamados/juizes.py:957`, `app/chamados/juizes.py:959`, `app/chamados/juizes.py:973`). Nenhum dos escritores alterados invalida esse cache (`app/chamados/services/cobrar_dia.py:37`, `app/chamados/services/arquivo.py:209`). Sequência que consulte C6, grave toque e consulte novamente na mesma request preserva `False`; caminho HTTP atual que necessariamente faça essa sequência [nao verificado].

6. **Resposta à precondição:** as guardas de `recusa_da_reabertura()` não consultam `resgatado_em` nem `silenciado_*`; consultam veto, resolução administrativa, premissa, agrupamento, irmão e transição (`app/chamados/models.py:1285`, `app/chamados/models.py:1290`, `app/chamados/models.py:1296`, `app/chamados/models.py:1299`, `app/chamados/models.py:1302`, `app/chamados/models.py:1306`). Agrupamento e irmão também usam status, sem véu (`app/chamados/models.py:783`, `app/chamados/models.py:791`, `app/chamados/juizes.py:1683`). O principal efeito pretendido da nova ordem é disponibilizar o log antes da ressuscitação (`app/chamados/models.py:1389`, `app/chamados/models.py:1442`).

7. **Resposta ao dia sem escala:** sim, a linha fica antes do retorno `None` por célula inexistente (`app/chamados/services/cobrar_dia.py:37`, `app/chamados/services/cobrar_dia.py:40`, `app/ponto/services/cartorio.py:1658`). Não demonstrei violação de contador: o selo existente exige uma linha por toque, explicitamente “não por efeito” (`app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py:275`). O novo registro pode futuramente liberar C6 se a célula aparecer; esse leitor usa existência da linha por colaborador/data, sem exigir sucesso da cobrança original (`app/chamados/juizes.py:967`, `app/chamados/juizes.py:969`). Aprovação ou proibição desse efeito pelo contrato [nao verificado].

8. **Escritores/leitores dos logs:** `cobrar_dia` → `registrar_log` → `core.observ.evento` → `LogAuditoria.objects.create`; `desarquivar_chamado` → `arquivo.levantar` → `_trilha` → o mesmo `evento` (`app/chamados/services/cobrar_dia.py:37`, `app/core/auditoria.py:22`, `app/core/observ.py:134`, `app/chamados/views_arquivar.py:73`, `app/chamados/services/arquivo.py:209`, `app/chamados/services/arquivo.py:137`). O leitor funcional é `_admin_tocou_o_dia_depois`: exige usuário humano, posterior à tranca, ação/modelo/objeto certos e, para cobrar, substring `dia=ISO` (`app/chamados/juizes.py:965`, `app/chamados/juizes.py:969`, `app/chamados/juizes.py:972`). Não há segundo escritor de banco introduzido nesses trechos (`app/core/observ.py:134`).

## 31/10 04:42 · sessao bf8e6edd6249 · - · - · origem=Ronald (tmux)

Cartório — conclusões com leitura estática:

1. **PROCEDENTE: a previsão pode fundamentar escrita sem que a lavra aconteça.** `ressuscitar_perguntas()` lê a projeção e grava `materializada_em=None` etc. (`app/chamados/models.py:1434`, `:1442`, `:1485`). Depois, antes da lavra (`app/ponto/services/cartorio.py:1520`), há chamadas que podem lançar: `_w` em `:1402`, `_espelhar_marcos` em `:1516`. Esta última grava contexto (`app/ponto/management/commands/detectar_ausencias.py:558`) e nota (`:563`; INSERT em `app/chamados/services/nota.py:103`/`:107`) sem capturar a exceção. `julgar_colab` possui `try/finally`, sem atomic englobando o corpo (`app/ponto/services/cartorio.py:747`, `:853`, `:854`); `julgar_celula` também não abre atomic (`:1670`). Chamadores diretos de produção igualmente não o fazem: `cobrar_dia` (`app/chamados/services/cobrar_dia.py:37`) e command (`app/ponto/management/commands/processar_cartorio.py:59`). Contraexemplo: célula gravada concorde, agora com furo parcial; pergunta antes carimbada é ressuscitada pela previsão acusadora; falha de INSERT da nota ao espelhar marcos impede a lavra, deixando a pergunta desmarcada sobre a célula antiga concorde. Ocorrência em produção: [nao verificado].

2. **Igualdade normal, com ressalva de persistência.** A previsão e a lavra usam exatamente `cel`, `via='cartorio'`, `_vd`, `cods` (`app/ponto/services/cartorio.py:1214`, `:1520`), e a lavra chama a mesma derivação (`app/ponto/portas/celula.py:698`). Não há reatribuição de `_vd`/`cods` entre ambas. Admin permanece com a dupla original na previsão (`app/ponto/portas/celula.py:656`) e na lavra (`:692`). A lavra pode apenas atualizar ata ou retornar sem alteração; “mesma dupla final” não significa “sempre grava” (`:693`, `:705`). `carimbar` persiste cel+evento em atomic próprio, sem abranger a ressuscitação anterior (`app/ponto/portas/celula.py:746`, `:639`).

3. **Caminhos sem lavra.** `apply_=False` ainda publica previsão (`app/ponto/services/cartorio.py:1214`), mas não entra em `:1517`; callback `write` pode consultá-la nos relatórios de emissão/furo parcial (`:1463`, `:1501`; callback definido em `:746`). Não achei consumidor de produção que consulte nesse callback: [nao verificado]. Sem vivos não impede lavra: a condição de `:1517` exige apenas `apply_`. `calado` somente troca/restaura `_tl.ativo`, sem limpar mapas (`:72`–`:77`); não constitui sozinho caminho sem lavra. O ramo `nunca_bateu` lavra separadamente, sem publicar mapas e sem ressuscitação (`:1012`, `:1034`).

4. **Mapa escritores/leitores.** `_tl.atas`: criação/escrita `app/ponto/services/cartorio.py:1146`/`:1148`; leitura pelo accessor `:55`. `_tl.vereditos`: criação/escrita `:1212`/`:1214`; leitura pelo accessor `:64`. Ambos chegam à projeção em `app/chamados/juizes.py:393`/`:394` e são limpos no finally em `app/ponto/services/cartorio.py:856`/`:857`. São mapas acumulados do julgamento inteiro, inclusive passadas anteriores, até o finally (`:776`, `:841`, `:856`).

5. **Leitores anteriores não ganham mudança funcional de veredito no fluxo normal.** `celula_do_dia_agora` alimenta `_celula_furada` (`app/chamados/juizes.py:437`) e `premissa_regularizada` (`app/chamados/models.py:1228`). O primeiro lê ata/trabalha, sem consultar veredito (`app/chamados/juizes.py:443`, `:444`, `:453`). O segundo lê somente `veredito_via=='admin'`, preservado pelo helper, antes de trabalhar com ata/marcos (`app/chamados/models.py:1230`, `:1232`, `:1236`, `:1245`). A mudança funcional pretendida concentra-se na ressuscitação: marco sobre ata projetada (`app/chamados/models.py:1436`; `app/chamados/juizes.py:865`, `:886`) e C6 sobre veredito projetado (`app/chamados/models.py:1442`; `app/chamados/juizes.py:1074`).

6. **Ata projetada não é literalmente a ata final.** A lavra preserva `precedencia` da ata anterior, enquanto `_tl.atas` recebe a ata bruta (`app/ponto/portas/celula.py:94`, `:684`, `:686`; `app/ponto/services/cartorio.py:1148`). Não encontrei leitor de `precedencia` nos consumidores examinados; efeito adverso desse detalhe nesta obra: [nao verificado].

7. **Juiz duplicado: não demonstrado no caminho cartório.** C6 continua registrado em `app/core/juizes.py:238`, é chamado pela ressuscitação (`app/chamados/models.py:1442`); a previsão reutiliza a função da lavra (`app/ponto/portas/celula.py:698`). Há pequena divergência contratual do helper para entrada arbitrária: não valida `via`, enquanto a lavra valida (`app/ponto/portas/celula.py:655`, `:662`, `:675`). Contraexemplo: helper aceita `via='invalida', veredito='trabalhou'`; lavra lança. Todos os consumidores atuais do helper passam cartório (`app/ponto/services/cartorio.py:1214`, `app/ponto/portas/celula.py:698`), portanto não o classificaria como falha atual de produção.

## 31/10 04:42 · sessao 31b18887450b · - · - · origem=Ronald (tmux)

Não identifiquei vazamento ou `save()` da cópia nos caminhos atuais: `celula_deste_julgamento` alimenta o carregador da premissa e a cópia de pergunta; a ressuscitação devolve IDs e escreve por outro queryset. (`app/chamados/juizes.py:378`, `app/chamados/models.py:1434`, `app/chamados/models.py:1442`, `app/chamados/models.py:1483`)

- **Cópia de Pergunta:** `_state` e `fields_cache` são explicitamente separados; a troca de `celula` ocorre depois dessa separação. `_pv_memo` é removido antes da avaliação e reescrito somente em `_q`. (`app/chamados/models.py:1427`, `app/chamados/models.py:1430`, `app/chamados/models.py:1439`, `app/chamados/juizes.py:823`)
- **Aliases mutáveis — DUVIDA:** a cópia rasa conserva referências dos campos mutáveis, e `out.ata` recebe diretamente o dict da ata em curso. Uma mutação aninhada poderia alterar os objetos compartilhados; os consumidores examinados apenas leem esses valores. (`app/chamados/juizes.py:398`, `app/chamados/juizes.py:400`, `app/chamados/juizes.py:869`, `app/chamados/juizes.py:886`, `app/ponto/services/lastro.py:148`)
- **Selo insuficiente — DUVIDA:** o teste cujo comentário promete isolamento da FK não cria uma pergunta nem exercita `fields_cache`; compara somente `veredito` e `ata` de células. Não comprova o isolamento implementado na pergunta. (`app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:185`, `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:188`, `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:197`, `app/chamados/models.py:1431`)
- **Juiz duplicado:** a porta consulta o C6 registrado, sem nova derivação de “pergunta viva” nesse trecho. (`app/core/juizes.py:238`, `app/chamados/models.py:1442`)
- **Leitores:** `fato_do_marco_no_espelho` passa a receber a ata em curso; C6 recebe também o veredito previsto. A mudança alcança o guarda da lâmpada e os ramos de veredito acusatório/provisório. (`app/chamados/models.py:1434`, `app/chamados/models.py:1436`, `app/chamados/models.py:1442`, `app/chamados/juizes.py:1074`, `app/chamados/juizes.py:1088`)
- **FK nula — DUVIDA:** perguntas podem ter `celula=None`; nesse caso não recebem a previsão do julgamento e o guarda do marco responde `None`. C6 segue pela classificação do dia ou pelo padrão vivo. Não estabeleci regressão introduzida pelo delta4 nesse caso. (`app/chamados/models.py:2191`, `app/chamados/juizes.py:390`, `app/chamados/juizes.py:865`, `app/chamados/juizes.py:1090`, `app/chamados/juizes.py:1100`)
- **Memo de toque — DUVIDA:** `_tranca_memo` vive no request ou colaborador e retorna o resultado anterior sem reconsultar trilhas. Consultar C6 antes do toque e novamente depois, no mesmo dono do memo, conserva o resultado antigo; a ocorrência desse encadeamento numa rota normal permanece [nao verificado]. (`app/chamados/juizes.py:943`, `app/chamados/juizes.py:947`, `app/chamados/juizes.py:957`, `app/chamados/juizes.py:973`)

## 31/10 04:21 · sessao 47b7718fd31d · O261-E-F1-CLASSE-4 · verificador · origem=Code

Há dois riscos de ordem no novo filtro: ele pode consultar a célula **antes da lavra atual** e a tranca **antes da trilha humana que autoriza o resgate**. Ambos podem deixar a pergunta carimbada após a tentativa de ressuscitação. Ocorrência e quantidade em produção: [nao verificado]. (`app/ponto/services/cartorio.py:1359`, `app/ponto/services/cartorio.py:1503`, `app/chamados/views_arquivar.py:68`, `app/chamados/views_arquivar.py:75`.)

A análise abaixo é estática. Execução dos testes, resultados de mutantes e correspondência Git de `7b76efa7..HEAD`: [nao verificado].

**1. Quem escreve, quem lê e todos os chamadores de produção**

Na tabela, **M** = `materializada_em`; **V** = `validada_em`; **R** = `respondida_em`; **Via** = `via_resolucao`. São os campos declarados em `app/chamados/models.py:2202`, `app/chamados/models.py:2203`, `app/chamados/models.py:2288`, `app/chamados/models.py:2648`.

| Escritor/porta | Campos e operação |
|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | Limpa M/V/R/Via e `validada_por`; é a escrita persistente usada pelo diff. `app/chamados/models.py:1477`. |
| `_universo_da_ressuscitacao` | Limpa M/V/R/Via **somente na cópia** consultada pelo C6. `app/chamados/models.py:1430`, `app/chamados/models.py:1432`. |
| `PerguntaDisputa.fechar_por_admin` | Grava M/Via. `app/chamados/models.py:2488`. |
| `fechar_por_competencia_trancada` | Grava M/Via. `app/chamados/models.py:2502`. |
| `encerrar_por_fato_no_espelho` | Troca Via. `app/chamados/models.py:2515`. |
| `encerrar_fantasma` / `reabrir_fantasma` | Gravam/limpam M/Via. `app/chamados/models.py:2527`, `app/chamados/models.py:2542`. |
| `reabrir_por_competencia` | Limpa M/Via. `app/chamados/models.py:2572`. |
| `reabrir_por_resposta` | Prepara a limpeza de M/Via em memória; o núcleo da resposta salva. `app/chamados/models.py:2562`, `app/chamados/services/disputa_emissao.py:1265`. |
| `responder_pergunta_disputa_core` | Grava R, desfaz V anterior e salva as limpezas acima. `app/chamados/services/disputa_emissao.py:1248`, `app/chamados/services/disputa_emissao.py:1260`, `app/chamados/services/disputa_emissao.py:1266`. |
| `reabrir_para_correcao` | Limpa R/V; opcionalmente M/Via. `app/chamados/services/disputa_emissao.py:1358`, `app/chamados/services/disputa_emissao.py:1360`. |
| `responder_automaticamente` / `carimbar_resposta_pela_trilha` | Gravam R. `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1409`. |
| `resolver_perguntas_por_chamado_resolvido` | Grava M/Via; conforme texto e ramo, também R/V. `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/disputa_emissao.py:1570`, `app/chamados/services/disputa_emissao.py:1644`. |
| Materializador de ausência | Grava M/Via e completa R/V quando há texto. `app/chamados/services/materializacao.py:509`, `app/chamados/services/materializacao.py:511`, `app/chamados/services/materializacao.py:526`. |
| Materialização de hora, recusa por colisão e pendentes aprovadas | Gravam M/Via. `app/chamados/services/materializacao.py:858`, `app/chamados/services/materializacao.py:1042`, `app/chamados/services/materializacao.py:1339`. |
| `_materializar_uma` | Grava M/Via e completa R/V quando há texto. `app/chamados/services/materializacao.py:1149`, `app/chamados/services/materializacao.py:1177`, `app/chamados/services/materializacao.py:1190`. |
| `_carimbar_validacao` / `devolver_validacao_recusada` | Gravam/desfazem V. `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`. |
| Reconciliador: `lavrar_sem_turno_aberto` e E2 | Gravam M/Via. `app/chamados/reconciliador.py:402`, `app/chamados/reconciliador.py:621`. |
| `fio.retratar_duplicata` | Grava Via e, se necessário, M. `app/chamados/services/fio.py:892`. |
| `relavrar` / `desfazer_carimbo_sem_lastro` | Trocam Via / limpam M/Via. `app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:231`. |

O nascimento passa por `PerguntaDisputa.abrir`, que aceita `**campos` e os entrega ao `create`; portanto também pode inicializar esses valores. (`app/chamados/models.py:2364`, `app/chamados/models.py:2442`.)

Os leitores diretos, além das próprias guardas dos escritores acima, distribuem-se assim:

| Leitor | Campos usados |
|---|---|
| `estado`, `respondida`, `q_vivas` | M/V/R na tradução de estado; R na propriedade `respondida`; os quatro no filtro SQL. `app/chamados/models.py:2465`, `app/chamados/models.py:2675`, `app/chamados/models.py:2603`. |
| Disputa: pendentes e respostas sem veredito | R; M/V/R. `app/chamados/models.py:1712`, `app/chamados/models.py:1758`. |
| C6 e seu memo | Os quatro; V mata primeiro, R participa da resposta, M/Via das vias e do lastro. `app/chamados/juizes.py:768`, `app/chamados/juizes.py:1006`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:1080`. |
| Juiz de materialização/testemunha | M/Via; trilha de retratação também exige M. `app/chamados/juizes.py:603`, `app/chamados/juizes.py:629`, `app/chamados/juizes.py:275`. |
| Filas corrigíveis, respondidas a validar e irmãs sem consumo | R; M/V/R nas irmãs. `app/chamados/juizes.py:1120`, `app/chamados/juizes.py:1127`, `app/chamados/juizes.py:1476`. |
| Lavra de cache e transição do chamado | M/R nos marcos do lote; V na seleção de perguntas afetadas pela transição. `app/chamados/veredito_lavrado.py:163`, `app/chamados/models.py:293`, `app/chamados/models.py:327`. |
| Cartório | M no universo; Via no ramo “todas duplicatas”. `app/chamados/models.py:1421`, `app/ponto/services/cartorio.py:1336`. |
| Emissão de disputa | M na derivação de destino, órfãos, âncora e exclusão da fábrica; R na recobrança. `app/chamados/services/disputa_emissao.py:211`, `app/chamados/services/disputa_emissao.py:695`, `app/chamados/services/disputa_emissao.py:874`, `app/chamados/services/disputa_emissao.py:894`, `app/chamados/services/disputa_emissao.py:1117`, `app/chamados/services/disputa_emissao.py:1445`. |
| Materialização | M/Via no estado aplicado e absorção; M/V/R nas filas/devoluções. `app/chamados/services/materializacao.py:417`, `app/chamados/services/materializacao.py:577`, `app/chamados/services/materializacao.py:620`, `app/chamados/services/materializacao.py:1287`. |
| Validação | M/V/R em elegibilidade, recusa, lote e fila. `app/chamados/services/validacao.py:363`, `app/chamados/services/validacao.py:422`, `app/chamados/services/validacao.py:534`, `app/chamados/services/validacao.py:685`, `app/chamados/services/validacao.py:877`. |
| Fábrica, relavra e reabertura de competência | M/Via nos filtros. `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:121`, `app/chamados/services/relavra_via.py:153`, `app/chamados/services/relavra_via.py:179`, `app/chamados/services/competencia_trancada.py:244`. |
| Reconciliador | M/V nos universos de E2 e órfãs; M no reatamento. `app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:1004`, `app/chamados/reconciliador.py:1411`. |
| Fio, ações e declaração | R no POST/resposta/correção; V na resposta validada e correção administrativa. `app/chamados/services/fio.py:336`, `app/chamados/services/fio.py:656`, `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:130`, `app/chamados/services/acoes_disputa.py:341`, `app/chamados/services/declaracao_texto.py:208`, `app/chamados/services/declaracao_texto.py:363`, `app/chamados/services/declaracao_texto.py:632`. |
| Painel, verbos, histórico e raio-X | M/V/R nos estados/filtros; Via no explicador. `app/chamados/services/painel.py:67`, `app/chamados/services/painel.py:268`, `app/chamados/services/verbos.py:237`, `app/chamados/services/verbos.py:308`, `app/chamados/services/historico.py:99`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:173`, `app/chamados/services/explicador.py:95`. |
| Aviso, cobrança, becos e relatório | R. `app/chamados/services/aviso_pendencia.py:13`, `app/chamados/services/cobranca.py:229`, `app/chamados/services/becos.py:30`, `app/chamados/relatorios_disputa.py:20`. |
| Vigias e serviços de ponto | V no canal/carimbo; R no vigia de resposta e supra-juiz; M/V na procedência; Via/V no flip. `app/chamados/services/furo_sem_canal.py:81`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/services/vigia_fio.py:417`, `app/ponto/supra_juiz.py:485`, `app/ponto/services/procedencia_batida.py:74`, `app/ponto/services/flip_auto.py:129`. |
| Views HTML e REST | M/R na resposta HTML; R nas listas e respostas da API; R na validação inline/mensageria. `app/chamados/views.py:1257`, `app/chamados/views.py:1280`, `app/chamados/views.py:1416`, `app/api/views.py:1009`, `app/api/views.py:1175`, `app/api/views.py:1187`, `app/api/views.py:2634`, `app/api/views.py:2688`, `app/api/views.py:2705`, `app/chamados/views_cobrar.py:169`, `app/api/views_mensageria.py:569`. |
| Templates | R/V no modal e pergunta; R/Via no cartão de atendimento. `app/templates/chamados/partials/modal_fio.html:184`, `app/templates/chamados/partials/modal_fio.html:223`, `app/templates/chamados/partials/_pergunta_card.html:20`, `app/templates/chamados/partials/_pergunta_card.html:24`, `app/templates/chamados/partials/_card_atendimento.html:94`, `app/templates/chamados/partials/_card_atendimento.html:186`. |

Também há leitores administrativos: auditorias de disputa/invariantes, cura de limbo, encerramento por fato, dedup, relavra, recuperação de órfãs, reconciliação de grade/fantasmas e passivo validado. Eles leem os carimbos ou a Via para selecionar, diagnosticar ou fotografar reversão. (`app/chamados/management/commands/auditar_disputas.py:71`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1568`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`, `app/chamados/management/commands/dedup_perguntas_fato.py:74`, `app/chamados/management/commands/relavrar_via_i6.py:58`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`, `app/ponto/management/commands/reconciliar_grade.py:453`, `app/ponto/management/commands/reconciliar_fantasmas.py:212`, `app/chamados/services/passivo_validado.py:64`.) O censo externo também lê R/V. (`bin/censo_folha.py:28`.)

O grafo completo de chamadas diretas das portas do diff é:

| Valor/porta | Produtor | Todos os chamadores diretos de produção encontrados |
|---|---|---|
| `_universo_da_ressuscitacao` → `(ids, marco, c6)` | Classificação e retorno em `app/chamados/models.py:1425`, `app/chamados/models.py:1435`. | `perguntas_a_ressuscitar`, `perguntas_puladas_na_ressuscitacao`, `ressuscitar_perguntas`: `app/chamados/models.py:1398`, `app/chamados/models.py:1403`, `app/chamados/models.py:1468`. |
| `perguntas_a_ressuscitar` → IDs | `app/chamados/models.py:1398`. | Somente `reabrir_orfas_enterradas`: `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`. A seleção vale para dry **e** apply: `app/chamados/management/commands/reabrir_orfas_enterradas.py:121`. |
| `perguntas_puladas_na_ressuscitacao` → dicionário | `app/chamados/models.py:1404`. | Somente cartório, quando `_n == 0`: `app/ponto/services/cartorio.py:1360`, `app/ponto/services/cartorio.py:1370`. |
| `ressuscitar_perguntas` → quantidade atualizada | `app/chamados/models.py:1477`, `app/chamados/models.py:1507`. | `reabrir` e cartório: `app/chamados/models.py:1389`, `app/ponto/services/cartorio.py:1359`. |
| `fio_marco_aceso` / `fio_c6_morta` | Leitura/incremento no próprio cartório: `app/ponto/services/cartorio.py:1372`, `app/ponto/services/cartorio.py:1376`. | Retorno genérico em `out`: `app/ponto/services/cartorio.py:1668`. Nenhum consumidor específico identificado; o resumo batch não os imprime: `app/ponto/management/commands/processar_cartorio.py:66`. |
| `[FIO-MARCO-ACESO]` / `[FIO-C6-MORTA]` | `app/ponto/services/cartorio.py:1373`, `app/ponto/services/cartorio.py:1377`. | Callback `write`; sem callback, descarte: `app/ponto/services/cartorio.py:737`. Batch passa `stdout.write`: `app/ponto/management/commands/processar_cartorio.py:65`. Parsers externos: [nao verificado]. |
| `[FIO-DUP-C8]` | Somente após fechar outros chamados: `app/ponto/services/cartorio.py:1353`, `app/ponto/services/cartorio.py:1356`. | Mesma cadeia `write`/stdout. `app/ponto/services/cartorio.py:737`, `app/ponto/management/commands/processar_cartorio.py:65`. |

A trilha persistente recebe `pulados_marco_aceso` e `pulados_c6_morta` **somente quando alguma pergunta voltou**; com zero, a porta retorna antes da trilha. (`app/chamados/models.py:1474`, `app/chamados/models.py:1482`, `app/chamados/models.py:1499`, `app/chamados/models.py:1501`.)

**2. Cópia consultada pelo C6 versus registro atualizado**

Para os quatro campos perguntados, a transformação coincide: ambos deixam M/V/R nulos e Via vazia. A diferença é `validada_por`: o `update` limpa, a cópia conserva. O C6 e sua chave de memo não leem esse autor; portanto essa diferença não altera o resultado atual do juiz. (`app/chamados/models.py:1432`, `app/chamados/models.py:1477`, `app/chamados/juizes.py:767`, `app/chamados/juizes.py:1005`.)

Nenhum dos dois limpa `materializada_por`, nem os campos de veredito lavrado da pergunta. Esses valores tampouco entram na decisão do C6 sobre essa cópia sem materialização. (`app/chamados/models.py:1432`, `app/chamados/models.py:1477`, `app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1080`.)

Sobre a cópia, o C6:

- Dá **morta** se o dia está trancado sem toque humano admitido. (`app/chamados/juizes.py:1010`, `app/chamados/juizes.py:907`.)
- Dá **viva** se a célula acusa. (`app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1056`.)
- Dá **morta** se a célula tem veredito não acusatório definitivo; o provisório sem fato do marco pode prosseguir vivo. (`app/chamados/juizes.py:1067`, `app/chamados/juizes.py:1068`.)
- Nos caminhos restantes, acaba dando viva porque `materializada_em` foi zerada. (`app/chamados/juizes.py:1070`, `app/chamados/juizes.py:1080`.)

O guarda anterior continua indispensável: o degrau do marco aceso exige M presente, mas a cópia já tem M nula. Por isso a porta pergunta pelo marco **antes** de limpar a cópia. (`app/chamados/models.py:1427`, `app/chamados/models.py:1432`, `app/chamados/juizes.py:1050`.)

Quanto aos caches:

- `_pv_memo` é removido da cópia; o C6 calcula e cria outro memo. Não há reaproveitamento desse resultado anterior. (`app/chamados/models.py:1431`, `app/chamados/juizes.py:792`, `app/chamados/juizes.py:803`.)
- `celula` e `disputa` vêm carregadas pela consulta. O C6 lê diretamente essa célula. A chave consulta `_state.fields_cache` para o destino, mas a cópia sem M não chega ao ramo de testemunha que usa o estado do destino. (`app/chamados/models.py:1424`, `app/chamados/juizes.py:758`, `app/chamados/juizes.py:1017`, `app/chamados/juizes.py:1080`.)
- Existe outro cache: `_tranca_memo`, na request ou no colaborador. Ele conserva a resposta de tranca e de toque humano durante sua vida; limpar `_pv_memo` não o limpa. (`app/chamados/juizes.py:899`, `app/chamados/juizes.py:919`, `app/chamados/juizes.py:937`.)
- A implementação instalada de `copy.copy(Model)` e o compartilhamento exato de `_state` não estão demonstrados pelo código da aplicação: [nao verificado]. O caminho mostrado não altera as FKs na cópia. (`app/chamados/models.py:1430`.)

Assim, **com os mesmos fatos externos, a cópia e o registro relido após o `update` recebem a mesma resposta do C6**. Isso não vale para reutilizar uma instância antiga: a porta faz `QuerySet.update`, sem atualizar os objetos já carregados pelo chamador. (`app/chamados/models.py:1475`, `app/chamados/models.py:1477`.)

A discrepância relevante encontrada é temporal: no cartório, a célula carregada pode representar a passada anterior, embora a rodada já tenha calculado outra ata. Isso é tratado no item 3. (`app/ponto/services/cartorio.py:1134`, `app/ponto/services/cartorio.py:1359`, `app/ponto/services/cartorio.py:1503`.)

**3. Comportamento por chamador e casos de retorno incorreto**

Nos caminhos que chegam a `reabrir`, a mudança comum é: uma candidata materializada e sem texto, antes aceita após o guarda do marco, agora fica carimbada se o C6 sobre sua versão limpa disser morta. A reabertura do chamado ainda pode retornar `True` mesmo com zero perguntas ressuscitadas. (`app/chamados/models.py:1420`, `app/chamados/models.py:1434`, `app/chamados/models.py:1389`, `app/chamados/models.py:1390`.)

Todos os chamadores de produção de `reabrir` encontrados são:

| Chamador | Caso atingido |
|---|---|
| Renascimento por premissa viva | Candidatas mudas do chamado reaproveitado passam pelo novo filtro; é chamado pelo dedup de `abrir` e pela resposta tardia. `app/chamados/models.py:978`, `app/chamados/models.py:881`, `app/chamados/services/resposta_tardia.py:45`. |
| Desarquivar humano | Perguntas do chamado encerrado pela competência passam pelo C6 antes da trilha do resgate. `app/chamados/views_arquivar.py:68`, `app/chamados/views_arquivar.py:75`. |
| Adesão | Candidatas mudas do pai reaberto recebem o filtro comum. `app/chamados/services/adesao.py:92`, `app/chamados/models.py:1434`. |
| Reabrir competência | A porta específica já limpa primeiro as perguntas com Via da tranca; outras candidatas do chamado passam pelo filtro comum. `app/chamados/services/competencia_trancada.py:247`, `app/chamados/services/competencia_trancada.py:258`. |
| Reabrir disputa administrativamente | Abre uma nova pergunta de confirmação; candidatas antigas do chamado passam pelo filtro comum. `app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/acoes_disputa.py:266`. |
| Resposta aguardando validação | Reabre o pai; perguntas com texto continuam fora do universo, mas suas irmãs mudas podem mudar. `app/chamados/services/disputa_emissao.py:1604`, `app/chamados/services/disputa_emissao.py:1606`, `app/chamados/models.py:1423`. |
| Recuperar órfãs enterradas | A pergunta-alvo é aberta antes do pai; o novo filtro afeta as demais candidatas do pai. `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`. |
| Recuperar chamado de ausência aberta | Candidatas mudas passam pelo filtro comum, se a reabertura for aceita. `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`, `app/chamados/models.py:1389`. |
| Reconciliar grade | Mesmo efeito nas candidatas mudas do chamado reaberto. `app/ponto/management/commands/reconciliar_grade.py:522`, `app/chamados/models.py:1434`. |
| Detector, ramo de limbo | Mesmo efeito nas candidatas mudas do chamado reaberto. `app/ponto/management/commands/detectar_ausencias.py:301`, `app/chamados/models.py:1434`. |

O outro chamador direto, **cartório**, deixa de vetar a ressuscitação por `_protesta(x)` no dia julgado e passa a julgar a pergunta pela célula dela. Isso permite o caso legítimo D+1 em folga cobrando D furado, coberto pelo novo teste. (`app/ponto/services/cartorio.py:1156`, `app/ponto/services/cartorio.py:1359`, `app/chamados/models.py:1434`, `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:63`.)

As entradas diretas de `julgar_colab` são batch, defesa de escala, comando de célula velha e wrapper singular. O corpo também roda na segunda passada e na releitura. (`app/ponto/management/commands/processar_cartorio.py:65`, `app/escala/regua_defesa.py:692`, `app/ponto/management/commands/celula_veredito_velho.py:144`, `app/ponto/services/cartorio.py:1654`, `app/ponto/services/cartorio.py:744`, `app/ponto/services/cartorio.py:767`, `app/ponto/services/cartorio.py:832`.)

Os chamadores diretos de `julgar_celula` são os sinais de chamado e ponto, reconciliador, cobrar dia, pós-vínculo, recuperação de ausência em folga, cura BUG115, vigia de hora, furos sem cobrança e porta de célula. (`app/chamados/signals.py:180`, `app/ponto/signals.py:93`, `app/chamados/reconciliador.py:675`, `app/chamados/services/cobrar_dia.py:29`, `app/col

[truncado em 20000 de 33283 caracteres]

## 31/10 04:16 · sessao 9e4a2c2c0086 · - · - · origem=Ronald (tmux)

Inventário enviado ao pai em mensagens, com leitores, escritores e chamadores diretos por `arquivo:linha`.

Achados principais:

- O diff não cria escritor persistente dos quatro campos: altera a seleção do `update` existente; a atribuição nova ocorre na cópia em memória. (`app/chamados/models.py:1430`, `app/chamados/models.py:1477`)
- `_tem_resposta_do_colab` significa “existe texto após `strip`”, sem exigir `respondida_em`; esse critério coincide com as proteções do reconciliador, materializador, seleção da canônica e prévia da folha. (`app/ponto/services/cartorio.py:1327`, `app/chamados/reconciliador.py:365`, `app/chamados/services/materializacao.py:769`, `app/chamados/services/fio.py:874`, `app/folha/previa.py:142`)
- Esse critério difere de `PerguntaDisputa.respondida` — somente timestamp —, do degrau de resposta do C6 — timestamp e texto —, e de `respostas_sem_veredito` — timestamp ou texto, sujeito aos demais filtros. (`app/chamados/models.py:2675`, `app/chamados/juizes.py:1020`, `app/chamados/models.py:1758`)
- Há discrepância prévia de normalização: texto formado apenas por espaços não isenta o chamado no cartório, mas tampouco pertence ao universo da ressuscitação, que aceita somente `NULL` ou `''` exato. Se esse universo ficar vazio, o novo diagnóstico não escreve nenhum dos dois marcadores; ocorrência real [nao verificado]. (`app/ponto/services/cartorio.py:1334`, `app/chamados/models.py:1423`, `app/chamados/models.py:1474`, `app/ponto/services/cartorio.py:1371`)
- A declaração documental de escritor único de `validada_em` não descreve todas as atribuições existentes: materialização e resolução automática também carimbam o campo, enquanto resposta/correção/ressuscitação o limpam. Essa multiplicidade antecede a alteração analisada. (`app/docs/JUIZES.md:38`, `app/chamados/services/materializacao.py:1195`, `app/chamados/services/disputa_emissao.py:1575`, `app/chamados/services/disputa_emissao.py:1260`, `app/chamados/models.py:1477`)
- `via_resolucao` já está explicitamente fora da declaração de escritor único. (`app/core/portas.py:352`)

## 31/10 04:16 · sessao af9e322f8a5d · - · - · origem=Ronald (tmux)

Inventário e achados por leitura estática; testes e manifestações em produção não executados [nao verificado]. [nao verificado]

1. Chamadores diretos dos métodos novos/alterados:

| Valor/método | Escritor/produtor | Leitores/chamadores de produção |
|---|---|---|
| `_universo_da_ressuscitacao()` → `(ids, marco, c6)` | Monta listas e retorna em `app/chamados/models.py:1420` e `:1435` | `perguntas_a_ressuscitar`, `models.py:1398`; `perguntas_puladas_na_ressuscitacao`, `:1403`; `ressuscitar_perguntas`, `:1468`. |
| `perguntas_a_ressuscitar()` → IDs | `app/chamados/models.py:1398` | Único chamador direto encontrado: comando `app/chamados/management/commands/reabrir_orfas_enterradas.py:106`, para vetar irmãs que ressuscitariam fora do escopo; não é apenas dry, a seleção precede tanto dry quanto apply (`:104`, `:121`). |
| `perguntas_puladas_na_ressuscitacao()` → dict | `app/chamados/models.py:1403` e `:1404` | Único chamador direto encontrado: `app/ponto/services/cartorio.py:1370`, somente quando ressuscitação retornou zero (`:1360`). |
| `ressuscitar_perguntas()` → contagem | `app/chamados/models.py:1477`, `:1507` | Dois: `ChamadoColaborador.reabrir`, `models.py:1389`; `_julgar_colab_corpo`, `cartorio.py:1359`. |
| `fio_marco_aceso` | Incremento/read-modify-write em `app/ponto/services/cartorio.py:1372` | Nenhum consumidor específico de produção encontrado; `julgar_celula` devolve `out` em `:1668`; o resumo batch não imprime este contador (`app/ponto/management/commands/processar_cartorio.py:66`). |
| `fio_c6_morta` | Incremento/read-modify-write em `app/ponto/services/cartorio.py:1376` | Mesma ausência de consumidor específico e mesmo retorno genérico; `cartorio.py:1668`, `processar_cartorio.py:66`. |
| `[FIO-MARCO-ACESO]` | `app/ponto/services/cartorio.py:1373` | Vai para callback `_w`, definido como `write` ou descarte em `:737`; batch passa `stdout.write` em `processar_cartorio.py:65`; parsers externos [nao verificado]. |
| `[FIO-C6-MORTA]` | `app/ponto/services/cartorio.py:1377` | Mesma cadeia `_w`/`write`; `cartorio.py:737`, `processar_cartorio.py:65`; parsers externos [nao verificado]. |
| `[FIO-DUP-C8]` | `app/ponto/services/cartorio.py:1356` | Mesma cadeia; emitido somente se lista `_out_dup` não vazia (`:1353`); parsers externos [nao verificado]. |

Os dois contadores não existem no dict inicial de `contadores()`; são chaves esparsas abertas pelos incrementos (`app/ponto/services/cartorio.py:1571`, `:1372`, `:1376`).

2. Entradas de produção que chegam a `reabrir()`:

- `_renascer_por_premissa_viva` em `app/chamados/models.py:978`, chamado pelo dedup de `abrir` em `:881` e por resposta tardia em `app/chamados/services/resposta_tardia.py:45`.
- Desarquivar por humano: `app/chamados/views_arquivar.py:68`.
- Adesão: `app/chamados/services/adesao.py:92`.
- Reabertura de competência: `app/chamados/services/competencia_trancada.py:258`.
- Reabertura administrativa de disputa: `app/chamados/services/acoes_disputa.py:266`.
- Resposta aguardando validação: `app/chamados/services/disputa_emissao.py:1606`.
- Correção de órfãs: `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`.
- Correção de ausência aberta: `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`.
- Reconciliação de grade: `app/ponto/management/commands/reconciliar_grade.py:522`.
- Detector, caso de limbo: `app/ponto/management/commands/detectar_ausencias.py:301`.

Para todos esses, o novo filtro recusa perguntas cuja versão sem carimbos o C6 dá por morta, incluindo dia trancado (`app/chamados/models.py:1432`, `:1434`; `app/chamados/juizes.py:1010`) ou célula definitiva que não acusa (`juizes.py:1053`, `:1068`). As guardas que recusam a própria reabertura continuam antes da ressuscitação (`models.py:1338`, `:1375`, `:1389`).

Entradas diretas de `julgar_colab`: batch `app/ponto/management/commands/processar_cartorio.py:65`; defesa de escala `app/escala/regua_defesa.py:692`; comando de célula velha `app/ponto/management/commands/celula_veredito_velho.py:144`; wrapper singular `app/ponto/services/cartorio.py:1654`. O corpo é chamado pela primeira passada, segunda passada e releitura em `cartorio.py:744`, `:767`, `:832`.

Chamadores diretos de `julgar_celula`: `app/chamados/signals.py:180`, `app/chamados/reconciliador.py:675`, `app/ponto/signals.py:93`, `app/chamados/services/cobrar_dia.py:29`, `app/colaboradores/services/pos_vinculo.py:72`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:71`, `app/ponto/management/commands/curar_residuo_bug115.py:143`, `app/ponto/services/vigia_de_hora.py:130`, `app/ponto/services/furos_sem_cobranca.py:128`, `app/ponto/portas/celula.py:372`.

3. Achado forte: decisão antes da lavra pode perder pergunta que deveria voltar.

O cartório calcula e expõe a ata desta rodada em `app/ponto/services/cartorio.py:1134` e `:1138`, mas chama ressuscitação em `:1359`, antes de persistir veredito e ata em `:1503` e `:1518`. O novo universo carrega `p.celula` do banco (`app/chamados/models.py:1424`); o C6 lê essa FK diretamente em `app/chamados/juizes.py:1017`, `:1053` e `:1068`; o juiz do marco também usa diretamente `p.celula.ata` (`juizes.py:845`, `:866`). O mecanismo `celula_do_dia_agora` que vê a ata em andamento é usado por `_celula_furada` (`juizes.py:417`), não por esses degraus do C6.

Caso dedutível: célula anteriormente `concorde`, agora furada; chamado permanece vivo; pergunta materializada/sem texto, marco antigo apagado ou desconhecido. A cópia é recusada pelo C6 a partir do `concorde` antigo; `_n=0` não enfileira `rejulgar`, pois isso só ocorre no ramo `_n` (`cartorio.py:1378`, `:1384`). O chamado vivo cala emissão (`:1421`). A impressão já foi calculada com fio morto (`:1095`, `:1104`) e será gravada (`:1518`), podendo fazer a rodada seguinte sem novo insumo pular (`:1113`). Portanto pode ficar preso até novo insumo ou `forcar`, e não somente uma rodada; ocorrência real no dado fornecido [nao verificado].

O inverso também é permitido pela ordem: célula antiga acusa, ata atual absolve; a cópia pode ser ressuscitada usando estado antigo, antes da lavra atual (`cartorio.py:1359`, `:1503`; `juizes.py:1056`). Outros blocos podem impedir alguns desses cenários, conforme guardas/reconciliação (`cartorio.py:1219`, `:1259`); não afirmar incidência real [nao verificado].

4. Achado forte: desarquivar em competência trancada consulta o pré-requisito antes de escrevê-lo.

`desarquivar_chamado` chama `ch.reabrir(humano=True)` em `app/chamados/views_arquivar.py:68`; só depois registra o toque humano `desarquivar_chamado`, em `:75`. O novo C6 consulta tranca (`app/chamados/juizes.py:1010`), cuja exceção depende de trilha humana posterior à tranca (`:904`, `:907`, `:945`, `:952`), inclusive a ação `TOQUE_DESARQUIVAR` (`:916`).

Sem toque anterior, o novo universo pula a pergunta antes de o próprio clique escrever a trilha. `arquivo.levantar` salva e registra a trilha em `app/chamados/services/arquivo.py:208` e `:209`; esse save não muda status e o signal não rejulga (`app/chamados/signals.py:169`). A view não ressuscita novamente (`views_arquivar.py:75`, `:80`). Se a pergunta conservou `via_resolucao=competencia_trancada` e `materializada_em`, mesmo a nova leitura após a trilha continua morta pelo degrau explícito dessa via (`juizes.py:1015`). O método pode devolver sucesso de reabertura com pergunta ainda fora da fila (`models.py:1389`, `:1390`). Manifestação em produção [nao verificado].

5. Efeito no comando que lê `perguntas_a_ressuscitar`.

Irmã materializada sem texto cujo C6 hipotético é morto deixa de aparecer na lista `irmas`; o comando deixa de recusar por `ressuscitaria_irmas` exclusivamente por essa irmã (`app/chamados/management/commands/reabrir_orfas_enterradas.py:106`, `:107`). No apply, abre primeiro a pergunta-alvo por `reabrir_fantasma`, depois disputa e pai (`:146`, `:154`, `:161`); a própria pergunta-alvo já saiu do universo materializado quando o pai ressuscita (`app/chamados/models.py:1421`). A promessa de segurança de irmãs depende de a seleção e o apply lerem o mesmo estado; não existe trava de linha nessas leituras (`reabrir_orfas_enterradas.py:104`, `:145`); concorrência real [nao verificado].

6. Checklist.

- Juiz duplicado de vitalidade: não é introduzido; o registro vivo aponta C6 para `pergunta_viva` (`app/core/juizes.py:112`, `:238`) e a porta chama essa função (`app/chamados/models.py:1434`). `_protesta` continua respondendo outra pergunta, sobre o chamado no dia julgado (`app/ponto/services/cartorio.py:1156`).
- Escritor fora de porta: o delta adiciona mutações somente à cópia, enquanto o update persistente continua no escritor existente (`app/chamados/models.py:1430`, `:1477`). A porta já é declarada no censo (`app/core/portas.py:246`; `app/core/censo_escritas.py:27`).
- Relógio fora da borda: o novo caminho para C6 pode chegar a `_dentro_da_janela_viva` em `app/chamados/juizes.py:1070`, que lê `datetime.date.today()` dentro do juiz em `:518`, sem receber `agora` do cartório. Não é leitura adicionada à função, mas passa a ser alcançada na ressuscitação.
- Derivação paralela: mesma regra, porém duas execuções; `ressuscitar_perguntas` chama universo (`app/chamados/models.py:1468`), e o cartório, quando zero, repete universo através de `perguntas_puladas` (`cartorio.py:1370`; `models.py:1403`). Não recebe os motivos do resultado que acabou de produzir zero; mudança concorrente pode alterar diagnóstico [nao verificado].
- Ordem de trava: o delta não introduz `atomic`/`select_for_update`; seleção e update permanecem separados (`app/chamados/models.py:1468`, `:1477`). O caminho de estado faz save de chamado antes de relavrar perguntas (`models.py:274`, `:279`, `:292`), preservando a ordem documentada chamado→pergunta (`:263`). Ausência de deadlock em produção [nao verificado].
- Pouso antes do pré-requisito: os dois casos acima, C6 antes da lavra corrente e C6 antes da trilha do desarquivar, são pontos concretos (`app/ponto/services/cartorio.py:1359`, `:1503`; `app/chamados/views_arquivar.py:68`, `:75`).
- Selo/RED: execução efetiva e resultado de mutações [nao verificado]; o agente de testes deve consolidar a análise estática. [nao verificado]

## 31/10 04:15 · sessao 99f570337b55 · - · - · origem=Ronald (tmux)

Auditoria estática; não executei testes nem mutações. Verde/vermelho abaixo é inferência pelo código, não resultado medido [nao verificado]. [nao verificado]

| Teste novo/alterado | O que efetivamente protege | Mutação de produção que sobrevive ao teste individual | [nao verificado]
|---|---|---|
| `test_MORDE_dia_em_competencia_trancada...destrancar...` | Executa E2 por `_caso`; exige ausência de carimbo durante tranca e C6 vivo após remover a tranca. Retirar somente o guarda de tranca do E2 faria carimbar e falharia. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:47`, `:67`, `:84`, `:96`, `:103`; `app/chamados/reconciliador.py:611`, `:617`, `:621`; `app/chamados/juizes.py:1010`. | Substituir a delegação `if pergunta_viva(p): continue` por uma cópia do teto: `if marcos_pendentes(p.celula) and fato_do_marco_no_espelho(p) is not True: continue`, mantendo o guarda de tranca. O caso continua pulando pela condição provisória e todos os quatro testes deste arquivo continuam com os resultados esperados por leitura: aceso/provisório carimba; apagado/provisório pula; trancado/apagado/provisório pula; definitivo carimba. Portanto não prova “SO ao C6”. `app/chamados/reconciliador.py:611`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:70`, `:75`, `:93`, `:106`; teto original `app/chamados/juizes.py:1067`. |
| `test_MORDE_fio_dup_c8_nao_fecha_o_velho_com_resposta_do_colab` | Protege o velho respondido contra fechamento/reaponte neste cenário, em duas ordens de nascimento. A fixture coloca simultaneamente texto e timestamp. `app/ponto/tests/test_fechar3_troca_escala_gate.py:330`, `:334`, `:339`, `:343`. | Trocar o predicado para `any(q.respondida_em is not None for q in perguntas)`: a fixture não distingue timestamp de texto. Restaurar os dois `any(...strip()...)` inline também passa: o diff apenas os extrai para um helper. Desativar todo o fallback FIO-DUP-C8 também preserva as três asserções negativas deste teste; o controle existente de reaponte, porém, cobraria positividade. `app/ponto/services/cartorio.py:1327`, `:1341`, `:1351`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:339`, `:343`, `:345`, controle `:313`, `:325`. |
| `test_MORDE_vespera_turno_aberto_na_folga_religa...` | Executa cartório de D+1, mantendo pergunta da célula D acusatória; observa materializada limpa e C6 vivo. Morde restaurar o bloqueio `_protesta` do dia julgado, pela diferença D/D+1 descrita e construída. `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:63`, `:65`, `:75`, `:81`, `:86`, `:90`. | Retirar o filtro C6 da porta e incluir toda pergunta não pulada pelo marco: a célula desta fixture já acusa, portanto o teste continua ressuscitando. `app/chamados/models.py:1427`, `:1434`; fixture `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:65`. |
| `test_MORDE_reabrir_nao_apaga_o_celula_absolveu...` | Executa `reabrir`, verifica manutenção de `via_resolucao`, materialização e lista `c6_morta`; a célula é lavrada com batidas e validada como definitiva. Morde aceitar toda pergunta apenas porque o marco é desconhecido: `manual_admin` não tem marco próprio. `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:94`, `:106`, `:126`, `:132`, `:136`, `:139`, `:142`; `app/chamados/juizes.py:823`, `:842`. | Remover a chamada de ressuscitação de `reabrir`: a pergunta já nasce materializada com a via esperada; a consulta posterior `perguntas_puladas...` ainda pode classificá-la como morta. O controle positivo seguinte serve para cobrar a ligação. `app/chamados/models.py:1389`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:60`, `:101`, `:139`, `:144`. |
| `test_controle_reabrir_com_celula_que_acusa_religa` | Cobra positividade: reabertura precisa limpar materialização e devolver pergunta viva na célula acusatória. `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:144`, `:149`, `:150`. | Retirar a consulta ao C6 e aceitar toda candidata sem marco aceso preserva este controle, mas seria capturado pelo teste negativo anterior. `app/chamados/models.py:1427`, `:1434`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:132`, `:144`. |

Duas mutações sobrevivem **aos três testes novos de ressuscitação conjuntamente**: [nao verificado]

- Trocar `pergunta_viva(_q)` por `_q.celula.veredito in CelulaDia.VEREDITOS_ACUSAM`: as fixtures cobrem somente `furo/concorde/furo`, sem distinguir o juiz completo de uma derivação paralela por veredito. Isso perde os degraus tranca, resposta, identidade e teto do C6, mas preserva as asserções presentes. `app/chamados/models.py:1434`; `app/chamados/juizes.py:1006`, `:1010`, `:1020`, `:1044`, `:1068`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:65`, `:126`, `:129`, `:90`, `:142`, `:149`.
- Omitir `via_resolucao=''` do `update`: positivos só cobram materialização limpa/C6 vivo, e suas vias `completude_turno`/`celula_absolveu` não integram o conjunto que encerra por identidade. O negativo exige justamente conservar a via. Também não há fixture de `respondida_em` ou `validada_por` preenchidos para morder a omissão dessas limpezas. `app/chamados/models.py:1477`; `app/chamados/juizes.py:1039`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:56`, `:60`, `:81`, `:101`, `:139`, `:149`.

A nova classificação observável não tem mordida nesses testes: remover os contadores/linhas `fio_marco_aceso`, `fio_c6_morta`, `[FIO-MARCO-ACESO]`, `[FIO-C6-MORTA]` ou a chave de auditoria `pulados_c6_morta` preserva suas asserções; apenas a lista pública `c6_morta` é verificada no negativo. `app/ponto/services/cartorio.py:1370`, `:1372`, `:1376`; `app/chamados/models.py:1501`; `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:90`, `:139`, `:142`, `:149`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:343`.

O cabeçalho E2 cita um log de mutação, mas esse comentário não prova a execução dos testes deste delta; o RED histórico citado permanece [nao verificado]. `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:5`.

## 31/10 03:49 · sessao 80bb87b1c8db · O261-E-F5C · verificador · origem=Code

1. **Pertença à fila: não localizei leitor residual usando `verbo` como conjunto da fila.** O produtor separa `fila_ids` de `verbo`; o juiz consulta `fila_ids`, e o vigia já lê `alim['fila_ids']`. A folha apenas encaminha a alimentação ao juiz. (`app/chamados/juizes.py:1161`, `app/chamados/juizes.py:1231`, `app/chamados/services/vigia_fio.py:567`, `app/folha/fora_da_folha.py:59`)

   O teste de alimentação usa o produtor real; os dicionários manuais encontrados simulam **a saída da porta**, não `alimentar_portas()`. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`, `app/folha/tests/test_haiku_fora_da_folha.py:101`, `app/folha/tests/test_haiku_fora_da_folha.py:129`, `app/chamados/tests/test_o261e_f5_alcance.py:307`)

2. **`.get(anc)` mistura ausência de chave com `None` legítimo.** Auditoria recebe pilha `registro`, cujo verbo é `None`, e a partição grava esse valor; nesse caso, o antigo `[]` também devolveria `None`. A mudança efetiva é aceitar silenciosamente uma âncora presente em `fila_ids`, mas ausente do mapa, antes um `KeyError`. (`app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:52`, `app/chamados/juizes.py:1231`, `app/chamados/juizes.py:1232`)

   O produtor normal inclui a fila no universo vivo e grava cada linha no mapa; ocorrência dessa ausência em produção: **[nao verificado]**. (`app/chamados/services/painel.py:186`, `app/chamados/services/verbos.py:44`)

   O tratamento pelos leitores é:

   - **Juiz:** `{anc: None}` continua sendo porta; se não houver `cobrar` ou `validar`, resulta em fio `chamado`. Para chamados da célula, `None` também passa pelo filtro de visibilidade. (`app/chamados/juizes.py:1183`, `app/chamados/juizes.py:1233`, `app/chamados/juizes.py:1236`)
   - **Folha:** lê `fio`, sem interpretar `verbos`; nesse caso chega a `chamado`, grupo C e `DESTRAVA['chamado']`. (`app/folha/fora_da_folha.py:70`, `app/folha/fora_da_folha.py:80`, `app/folha/fora_da_folha.py:199`)
   - **Vigia:** conta a porta, registra papel textual `"None"` e esse valor sozinho não incrementa `alcance_porta_exige_o_admin`. (`app/chamados/services/vigia_fio.py:599`)
   - **Testes:** aceitam explicitamente `None` na escada e numa porta mockada; esses casos não exercitam uma chave ausente na nova linha `.get(anc)`. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:247`, `app/folha/tests/test_haiku_fora_da_folha.py:129`)

3. **`DESTRAVA['admin']` cobre os ramos atuais de `validar`.** São dois: módulo em `MODULOS_DO_ADMIN_VALIDAR` — atualmente `gps_fora_geofence` — ou resposta creditada. Auditoria sai antes como `None`; silêncio substitui o verbo não nulo por `arquivado`/`historico`. A frase nova contempla resposta e chamado que o admin valida. (`app/folha/fora_da_folha.py:41`, `app/chamados/catalogo/natureza.py:29`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:59`, `app/chamados/services/verbos.py:48`)

   Ressalva: nesse crédito, “respondida” significa `respondida_em is not None`, sem conferir texto; a frase promete acompanhar o que o painel põe em Validar, portanto não contradiz esse comportamento. (`app/chamados/services/raiox.py:137`, `app/folha/fora_da_folha.py:41`)

4. **O teste novo da ficha tem mordida parcial.** [nao verificado]

   - **Tupla literal:** isoladamente, não detecta voltar a `('arquivado', 'historico')`: essa é exatamente a constante atual, e os dois resultados cobrados permanecem iguais. O AST adjacente detectaria essa escrita literal porque proíbe `'historico'`; porém, exigir a presença do nome da constante no módulo não prova que ele governa `dono`. (`app/chamados/services/verbos.py:18`, `app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:114`, `app/colaboradores/tests/test_ficha_do_colab.py:128`)
   - **Truncamento `[:1]`:** detecta, pois exige dono `arquivado` para os dois verbos produzidos pelas duas datas. (`app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:113`, `app/chamados/services/verbos.py:51`)
   - **Mudança da fonte:** não testa se o consumidor acompanha uma constante alterada. Adicionar/remover membro pode derrubar a igualdade final mesmo com a ficha obedecendo à fonte, pois os estímulos continuam produzindo apenas os dois verbos atuais; reordenar passa pela comparação de conjuntos. (`app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:114`, `app/chamados/services/verbos.py:51`)
   - **Relógio solto:** sim, transitivamente. O teste não congela localmente; chama a ficha, que lê `timezone.now()`, e a partição lê outro. `self.hoje` também vem de `localdate()`. O runner só congela se houver `CONGELAR_EM`. (`app/colaboradores/tests/test_ficha_do_colab.py:18`, `app/colaboradores/tests/test_ficha_do_colab.py:30`, `app/colaboradores/tests/test_ficha_do_colab.py:98`, `app/colaboradores/services/ficha.py:117`, `app/chamados/services/verbos.py:35`, `app/core/runner_quarentena.py:94`)

5. **Checklist, restrito ao delta:**

   | Item | Resultado |
   |---|---|
   | 2º juiz | A folha delega ao juiz da porta; não reproduz a tradução de verbos. (`app/folha/fora_da_folha.py:59`, `app/folha/fora_da_folha.py:70`) |
   | 2º escritor fora da porta | A troca para `.get` apenas altera leitura e montagem do retorno. (`app/chamados/juizes.py:1232`, `app/chamados/juizes.py:1237`) |
   | Relógio fora da borda | Permanece alcançado pelo teste novo, sem freeze local. (`app/colaboradores/tests/test_ficha_do_colab.py:111`, `app/colaboradores/services/ficha.py:117`, `app/chamados/services/verbos.py:35`) |
   | Derivação paralela de fonte única | A ficha atual usa a constante; o teste comportamental novo não prova essa dependência. (`app/colaboradores/services/ficha.py:160`, `app/colaboradores/tests/test_ficha_do_colab.py:114`) |
   | Selo sem mordida | Mordida parcial na ficha; os testes de `None` citados não distinguem valor legítimo de chave ausente. (`app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:247`, `app/folha/tests/test_haiku_fora_da_folha.py:129`) |

Execução dos testes e efeitos em produção: **[nao verificado]**. [nao verificado]

## 31/10 03:48 · sessao 2ffa2e9478c8 · - · - · origem=Ronald (tmux)

Item 2 — `.get(anc)` tem dois `None` diferentes:

- **Chave presente, valor `None`:** módulos de auditoria recebem pilha `registro`, cujo verbo é `None`; a partição grava esse valor no mapa. O silêncio não o substitui porque exige `v is not None`. [natureza.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/catalogo/natureza.py:44), [natureza.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/catalogo/natureza.py:63), [verbos.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/verbos.py:47). O `[]` anterior já devolvia esse `None`. [juizes.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/juizes.py:1232)
- **Chave ausente:** a alteração passa a esconder alimentação inconsistente, devolvendo o mesmo `None`; anteriormente haveria `KeyError`. O produtor normal deriva a fila de `qs_vivos` e grava todas as linhas na partição; ocorrência de ausência entre consultas ou em produção é [não verificado]. [painel.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/painel.py:186), [verbos.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/verbos.py:52), [juizes.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/juizes.py:1232)

Os consumidores não rejeitam `None`:

- O juiz conserva `{anc: None}` como porta existente; sem `cobrar` ou `validar`, produz fio `chamado`. `None` também passa pelo filtro de visibilidade, pois não pertence a `FORA_DA_LISTA_PADRAO`. [juizes.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/juizes.py:1183), [juizes.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/juizes.py:1233)
- A folha lê somente `porta['fio']`: recebe `chamado`, classifica como C e usa `DESTRAVA['chamado']`. [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:68), [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:80), [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:199)
- O vigia contabiliza a porta, não exige admin por esse valor e imprime papel literal `"None"`; se houver também `validar`/`decidir`, a interseção ainda exige admin. [vigia_fio.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/vigia_fio.py:599)
- O teste F5B cobre apenas a escada isolada com `{1: None}`; o teste da folha injeta porta já pronta com `None` e exige `[555]`. Esses casos não exercitam a nova ausência de chave em `.get(anc)`. [test_o261e_f5b_juiz_porta.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/tests/test_o261e_f5b_juiz_porta.py:243), [test_haiku_fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/tests/test_haiku_fora_da_folha.py:129)

Item 3 — **O texto novo cobre os dois ramos atuais que resultam em `validar`:** módulo em `MODULOS_DO_ADMIN_VALIDAR` (`gps_fora_geofence`) ou presença no conjunto de respostas creditadas. Auditoria vence antes; pergunta pendente e demais casos resultam em outros verbos. O silêncio substitui qualquer verbo não nulo por `arquivado`/`historico`, retirando-o de `validar`. [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:41), [natureza.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/catalogo/natureza.py:29), [natureza.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/catalogo/natureza.py:44), [verbos.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/verbos.py:48)

A ressalva é que “resposta creditada” significa `respondida_em is not None` no crédito, sem verificar texto nesse ponto; a nova frase limita-se ao que o painel põe em Validar. [raiox.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/chamados/services/raiox.py:133), [fora_da_folha.py](/home/ronald/codex_ws/cc62d1683f6c9fe19793707279321e2bdc7fda15/app/folha/fora_da_folha.py:41)

## 31/10 03:48 · sessao 388eeedeb856 · - · - · origem=Ronald (tmux)

- **O novo teste não morde a tupla literal atual.** Hoje `FORA_DA_LISTA_PADRAO` é exatamente `('arquivado', 'historico')`; trocar o uso da constante na ficha por essa tupla preserva as duas respostas cobradas pelo teste (`app/chamados/services/verbos.py:13`, `app/chamados/services/verbos.py:18`, `app/colaboradores/services/ficha.py:160`, `app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:114`).

- **O AST adjacente morde essa escrita literal**, porque proíbe qualquer constante `'historico'`; contudo, exigir que o nome `FORA_DA_LISTA_PADRAO` apareça em algum ponto do módulo não comprova que governa `dono` (`app/colaboradores/tests/test_ficha_do_colab.py:125`, `app/colaboradores/tests/test_ficha_do_colab.py:128`, `app/colaboradores/tests/test_ficha_do_colab.py:129`).

- **O novo teste morde truncar para `FORA_DA_LISTA_PADRAO[:1]`**, pois exige `dono='arquivado'` nos dois casos produzidos pelo silêncio vencido/vigente; o segundo verbo ficaria sem esse dono (`app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:113`, `app/chamados/services/verbos.py:51`, `app/colaboradores/services/ficha.py:160`).

- **Mudança de conteúdo da constante não é exercitada por mutação.** Os estímulos continuam produzindo somente `HISTORICO` e `ARQUIVADO`; adicionar/remover um membro da constante faz a igualdade final falhar mesmo com a ficha obedecendo à constante. Reordená-la passa, pois a comparação usa conjuntos (`app/chamados/services/verbos.py:51`, `app/colaboradores/tests/test_ficha_do_colab.py:109`, `app/colaboradores/tests/test_ficha_do_colab.py:114`).

- **Há relógio solto transitivo, sem congelamento local:** o teste chama a ficha, que captura `timezone.now()`, e esta chama a partição, que captura outro `timezone.now()`; `self.hoje` vem de `timezone.localdate()` (`app/colaboradores/tests/test_ficha_do_colab.py:30`, `app/colaboradores/tests/test_ficha_do_colab.py:98`, `app/colaboradores/tests/test_ficha_do_colab.py:111`, `app/colaboradores/services/ficha.py:117`, `app/colaboradores/services/ficha.py:155`, `app/chamados/services/verbos.py:35`). O congelamento global depende de `CONGELAR_EM`, e esse arquivo já está na lista de relógios soltos (`app/core/runner_quarentena.py:94`, `app/core/runner_quarentena.py:99`, `app/core/tests/testes_sem_relogio.txt:233`).

## 31/10 03:48 · sessao 052b0de4e58d · - · - · origem=Ronald (tmux)

Item 1: **nenhum leitor residual localizado usando `verbo` como conjunto da fila**. O produtor separa `fila_ids`/`fila_cel`, vindos de `qs_na_fila`, do mapa `verbo`, vindo de `qs_vivos` (`app/chamados/juizes.py:1153`, `app/chamados/juizes.py:1161`).

- O juiz decide pertença por `fila_cel` e `fila_ids`; usa `verbo` apenas para consultar valores e visibilidade (`app/chamados/juizes.py:1227`, `app/chamados/juizes.py:1228`, `app/chamados/juizes.py:1231`, `app/chamados/juizes.py:1232`, `app/chamados/juizes.py:1236`).
- O vigia **já lê `alim['fila_ids']`**; o teste de pertença no véu consulta esse conjunto (`app/chamados/services/vigia_fio.py:567`, `app/chamados/services/vigia_fio.py:620`).
- A folha passa a alimentação ao juiz, sem interpretar `verbo` (`app/folha/fora_da_folha.py:54`, `app/folha/fora_da_folha.py:59`).
- O teste de alimentação usa o produtor real (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`).
- Os dicts manuais localizados são **saídas da porta**, devolvidas por mock do juiz; não simulam `alimentar_portas` (`app/folha/tests/test_haiku_fora_da_folha.py:101`, `app/folha/tests/test_haiku_fora_da_folha.py:106`, `app/folha/tests/test_haiku_fora_da_folha.py:129`, `app/folha/tests/test_haiku_fora_da_folha.py:143`).

## 31/10 03:40 · sessao 6c4eeff007e0 · O261-E-F5C · verificador · origem=Code

A cura separa corretamente **verbo** de **pertencimento à fila**, mas ainda há falhas de custo, garantias textuais excessivas e cobertura incompleta dos testes. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1158`, `app/chamados/services/vigia_fio.py:549`, `app/colaboradores/tests/test_ficha_do_colab.py:110`)

1. **Quem escreve e quem lê o dict**

   | Chave | Escritor | Todos os leitores diretos encontrados |
   |---|---|---|
   | `verbo` | `particionar_verbos(qs_vivos())`, `app/chamados/juizes.py:1158` | Fila, âncora e seleção dos números visíveis: `app/chamados/juizes.py:1224`, `:1228`, `:1232`. |
   | `fila_ids` | Inicialização e inclusão: `app/chamados/juizes.py:1148`, `:1151`; retorno: `:1158` | Porta da pergunta: `app/chamados/juizes.py:1227`; vigia: `app/chamados/services/vigia_fio.py:566`, usado em `:619`. |
   | `fila_cel` | `app/chamados/juizes.py:1149`, `:1153` | Porta da fila: `app/chamados/juizes.py:1223`. |
   | `vivas` | `app/chamados/juizes.py:1154`, `:1157` | Porta: `app/chamados/juizes.py:1225`; vigia: `app/chamados/services/vigia_fio.py:571`, `:576`, `:595`, `:616`. |
   | `colab` | `app/chamados/juizes.py:1159` | Identificação do colaborador para consultar o app: `app/chamados/juizes.py:1227`. |
   | `no_app` | Vazio em `app/chamados/juizes.py:1159`; preenchimento lazy em `:1167` | `_no_app`: `app/chamados/juizes.py:1163`, `:1168`; chamadas pela porta em `:1227` e pelo vigia em `app/chamados/services/vigia_fio.py:580`. |

   A folha e as pautas transportam a alimentação ao juiz; o teste de equivalência também usa alimentação real. (`app/folha/fora_da_folha.py:53`, `:58`, `:138`, `:190`; `app/ponto/services/pautas_dos_discordantes.py:74`, `:76`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`)

   Não encontrei leitor atual desse dict usando `verbo` como conjunto da fila: os dois pontos relevantes agora usam `fila_ids`. Também não encontrei teste construindo manualmente a **alimentação**; os dicts manuais e mocks encontrados representam a **saída da porta**. (`app/chamados/juizes.py:1227`; `app/chamados/services/vigia_fio.py:566`; `app/folha/tests/test_haiku_fora_da_folha.py:101`, `:129`, `:143`; `app/chamados/tests/test_o261e_f5_alcance.py:306`)

   **Sobrou uma referência de teste errada:** `_verbo_do_painel` do teste do vigia continua particionando `qs_na_fila()` e chamando isso de verbo do painel. Seus casos simples não expõem o defeito curado. (`app/chamados/tests/test_o261e_f5_alcance.py:281`, `:285`, `:287`)

2. **Igualdade com painel e badge**

   **Painel padrão, sem filtros, em estado estável: sim, o motor recebe o mesmo universo.** O painel monta a base sem filtros e particiona seus vivos; o juiz particiona `qs_vivos()` diretamente. `select_related`, prefetch, anotações e ordenação não introduzem outro recorte; a partição remove o prefetch antes de ler suas colunas. (`app/chamados/services/painel.py:96`, `:135`, `:198`, `:228`; `app/chamados/juizes.py:1158`; `app/chamados/services/verbos.py:44`)

   **Com filtros, pode divergir.** Contraexemplo deduzido: C normal; D urgente e vivo; pergunta respondida com destino D e pai C. Globalmente, D recebe a pergunta e C fica `decidir`. Filtrando urgência normal, D sai do conjunto e o crédito volta a C, que vira `validar`. O crédito prefere o destino somente quando ele pertence ao conjunto recebido. Ocorrência desse cenário em produção: `[nao verificado]`. (`app/chamados/services/painel.py:117`, `:228`; `app/chamados/services/raiox.py:54`, `:56`, `:140`; `app/chamados/catalogo/natureza.py:48`)

   **Empresas visíveis não restringem o painel padrão:** alimentam os seletores. O vigia restringe os dias a essas empresas, mas a alimentação continua particionando todos os vivos. (`app/chamados/services/painel.py:96`, `:127`, `:436`; `app/chamados/services/vigia_fio.py:534`; `app/chamados/juizes.py:1158`)

   **O badge não garante igualdade no instante atual:** normalmente devolve `PlacarNavbar.badge_chamados`; somente sem lavra usa a partição atual. Em outra exceção, devolve simplesmente `qs_na_fila().count()`. Além disso, mostra uma soma de `validar` e `decidir`, sem verbo individual por chamado. (`app/core/context_processors.py:68`, `:84`, `:88`, `:93`; `app/chamados/services/verbos.py:22`, `:25`)

   `registrado` continua vivo e pode receber crédito, embora não abra porta nem apareça na lista padrão. Silêncio vencido pode abrir porta com verbo `historico`; silêncio vigente fica fora da fila. Verbo `None` pode abrir porta para chamado aberto/em análise de auditoria, produzir fio `chamado` e aparecer no painel como `Registro`. (`app/chamados/catalogo/motor.py:24`, `:29`; `app/chamados/services/painel.py:187`, `:314`; `app/chamados/services/verbos.py:48`, `:125`; `app/chamados/juizes.py:1224`, `:1232`; `app/chamados/catalogo/natureza.py:44`, `:63`)

3. **Pertencimento a `qs_na_fila`**

   Com alimentação real e estado estável, **as duas portas continuam exigindo fila**: `fila_cel` e `fila_ids` nascem da mesma iteração de `qs_na_fila`; a primeira porta usa `fila_cel`, a segunda exige `anc in fila_ids`. Não encontrei passagem estável de âncora fora da fila. (`app/chamados/juizes.py:1150`, `:1153`, `:1223`, `:1227`)

   Uma âncora na fila pode legitimamente não abrir a porta da pergunta: ainda são necessárias pergunta viva, presença no app e resposta não vazia quando respondida. A âncora é o destino; o pai só serve quando não existe destino. (`app/chamados/juizes.py:1155`, `:1227`; `app/chamados/services/fio.py:116`)

   **Há uma fragilidade de consistência:** fila e mapa são lidos separadamente, sem revalidação da alimentação. Se o chamado deixar de ser vivo entre essas leituras, a porta da fila pode receber `None` pelo `.get()`, enquanto a porta da pergunta pode lançar `KeyError` pelo acesso direto. Ocorrência em produção: `[nao verificado]`. (`app/chamados/juizes.py:1150`, `:1158`, `:1222`, `:1224`, `:1228`)

4. **A chave `fila` pode nomear chamado oculto no painel padrão?**

   **Não encontrei esse caso em estado estável e sem filtros.** O juiz exige pertencimento à fila e remove `FORA_DA_LISTA_PADRAO`; o painel exige `COBRANCA` e remove a mesma constante. Paginação pode deixar o chamado fora da página corrente, mas não o elimina do conjunto. (`app/chamados/juizes.py:1150`, `:1232`; `app/chamados/services/painel.py:314`, `:326`, `:340`)

   Silêncio vencido conserva a porta, mas perde o número em `fila`; silêncio vigente sequer abre a porta. Isso está coberto pelo teste correspondente. (`app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:51`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:144`)

5. **Checklist das classes de reprovação**

   - **Segundo juiz:** a porta foi registrada e a folha lê seu `fio`. `pergunta_sem_acao_possivel` responde outra pergunta, conforme a ressalva posterior do corte; mantê-la não demonstra duplicação da porta do dia. (`app/core/juizes.py:253`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/raiox.py:151`; `app/docs/BACKLOG.md:778`)
   - **Segundo escritor:** os trechos novos de alimentação/porta escrevem mapas em memória; o command publica pelo carimbador existente. Não identifiquei nova escrita de estado de domínio nesses trechos. (`app/chamados/juizes.py:1148`, `:1167`, `:1233`; `app/chamados/management/commands/vigiar_fio_morto.py:206`)
   - **Relógio fora da borda:** o command captura `agora`, mas passa apenas `hoje`; `qs_na_fila` e `particionar_verbos` continuam lendo relógios próprios. Portanto a rodada não tem um instante único para silêncio/verbos. Os testes congelados não exercitam essa travessia. (`app/chamados/management/commands/vigiar_fio_morto.py:162`, `:167`; `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:35`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:28`)
   - **Derivação paralela:** a tupla de verbos ocultos saiu dos leitores de produção e ficou na constante compartilhada. A permissão do Resolver dia, porém, continua copiada em literal no vigia e no drawer; o teste verifica coincidência textual, sem transformar isso em fonte única. (`app/chamados/services/verbos.py:18`; `app/chamados/services/painel.py:327`; `app/colaboradores/services/ficha.py:160`; `app/chamados/services/vigia_fio.py:184`; `app/colaboradores/services/drawer.py:130`; `app/chamados/tests/test_o261e_f5_alcance.py:357`)
   - **Mordida da cura principal:** os novos casos distinguem a regressão para `particionar_verbos(qs_na_fila())`: nesse mutante C recebe a resposta e deixa de ser `decidir`. O teste da âncora silenciada distingue a troca indevida de `fila_ids` por `verbo`. Essa conclusão é estática; execução Django/RED nesta revisão: `[nao verificado]`. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:192`, `:213`, `:226`, `:236`)
   - **Selo fraco da ficha:** o mutante `verbo in FORA_DA_LISTA_PADRAO[:1]` passa nas três asserções AST e classifica `historico` como `registro`. Conferi essas expressões em memória: o teste exige presença do nome e ausência do literal, mas não exige uso integral da constante. (`app/colaboradores/services/ficha.py:160`; `app/colaboradores/tests/test_ficha_do_colab.py:108`, `:110`, `:111`, `:112`)
   - **Ordem de trava:** não identifiquei aquisição nova de trava nos trechos acrescentados; o command compartilha a lista e lê sua falha depois dos dois censos. (`app/chamados/juizes.py:1129`; `app/chamados/services/vigia_fio.py:187`; `app/chamados/management/commands/vigiar_fio_morto.py:166`)
   - **Pré-requisito:** o código consumidor e o juiz estão presentes no pouso fornecido, mas a documentação contradiz a ordem: a linha diz “ANTES da F5” e termina “depois da F5”. A sequência real de pousos, suite e opus não está demonstrada pelos anexos: `[nao verificado]`. (`app/docs/BACKLOG.md:777`; `app/docs/CORTES.md:400`)

6. **Custo**

   **Existe multiplicação comprovável no vigia:** para K pares empresa×competência, `censo_alcance` chama K vezes `fora_da_folha`, que alimenta o juiz; depois alimenta novamente. Havendo dias retidos, são **K+1 partições globais**, inclusive alimentações de lotes vazios. (`app/chamados/services/vigia_fio.py:546`, `:549`, `:565`; `app/folha/fora_da_folha.py:138`; `app/chamados/juizes.py:1146`, `:1158`)

   **Também há consultas por pergunta dentro da alimentação:** o carregamento inclui somente `disputa` e `celula`, mas C6 acessa `disputa.colaborador`, empresa e tranca. Fora de request, o memo pertence à instância do colaborador, permitindo repetir consultas. (`app/chamados/juizes.py:1155`, `:1008`, `:888`, `:896`, `:900`, `:919`; `app/ponto/services/fechamento.py:1438`)

   `_no_app` acrescenta consultas por colaborador; as perguntas do app carregam apenas `disputa`, enquanto o juiz acessa também `celula`, abrindo outro caminho de N+1. (`app/chamados/juizes.py:1163`, `:757`; `app/chamados/services/pergunta_no_app.py:42`)

   Não identifiquei chamada de produto por célula/request; os caminhos encontrados alimentam lotes em vigia, lavrador e pautas. Tempo e quantidade efetiva de consultas em produção: `[nao verificado]`. (`app/chamados/services/vigia_fio.py:565`; `app/folha/management/commands/lavrar_retrato_folha.py:54`; `app/ponto/services/pautas_dos_discordantes.py:74`; `app/folha/views_retrato.py:8`)

7. **Textos que o código não garante**

   - **“Consultas por lote, nunca por dia”** e **“UMA vez por rodada”** excedem as garantias, pelos acessos de C6 e pelas alimentações indiretas acima. (`app/chamados/juizes.py:1130`, `:888`; `app/chamados/services/vigia_fio.py:514`, `:549`; `app/folha/fora_da_folha.py:138`)
   - **“Mesmo universo do painel e do badge”** precisa distinguir painel sem filtros, painel filtrado e badge lavrado. (`app/chamados/juizes.py:1136`, `:1206`; `app/chamados/services/vigia_fio.py:173`; `app/core/context_processors.py:68`)
   - **“Pergunta sem crédito deixa a âncora em decidir”** não vale universalmente: auditoria pode dar `None`, geofence dá `validar`, outra pergunta pode fornecer crédito e silêncio pode sobrepor `historico`. (`app/chamados/juizes.py:1211`; `app/chamados/catalogo/natureza.py:44`, `:46`, `:48`; `app/chamados/services/verbos.py:51`)
   - **`DESTRAVA['admin']` promete resposta na mesa de disputa**, mas um chamado `gps_fora_geofence` da célula pode produzir `validar`/fio `admin` sem pergunta. O texto não acompanha esse contrato mais amplo. (`app/folha/fora_da_folha.py:41`; `app/chamados/catalogo/natureza.py:46`, `:59`; `app/chamados/juizes.py:1175`, `:1224`)
   - **“Ninguém está esperando ninguém”** também é excessivo: sem essas duas portas ainda pode haver decisão pendente, ausência aberta ou mesa do admin. (`app/folha/fora_da_folha.py:46`; `app/chamados/juizes.py:1214`; `app/chamados/services/vigia_fio.py:288`, `:291`, `:295`)
   - Restaram descrições antigas: grupo C como “chamado vivo sem pergunta viva” e folha julgando diretamente por `.viva`; o leitor atual recebe o fio da porta. (`app/folha/fora_da_folha.py:10`, `:67`; `app/chamados/services/vigia_fio.py:129`)

## 31/10 03:39 · sessao e426ca6c50df · - · - · origem=Ronald (tmux)

Custo confirmado por leitura estática:

- **Partição global repetida:** cada `alimentar_portas` particiona `qs_vivos()` inteiro, mesmo com lote vazio (`app/chamados/juizes.py:1146`, `:1158`). `censo_alcance` chama `fora_da_folha` por empresa×competência (`app/chamados/services/vigia_fio.py:546`), cada chamada alimenta (`app/folha/fora_da_folha.py:138`), e depois alimenta novamente (`app/chamados/services/vigia_fio.py:565`). São K+1 partições quando há dias retidos, sendo K os pares empresa×competência.
- **N+1 dentro da alimentação:** carrega apenas `disputa` e `celula` (`app/chamados/juizes.py:1155`), mas C6 acessa `disputa.colaborador` (`:888`) e `colab.empresa` (`:896`). Fora de request, o memo pertence à instância do colaborador (`:919`), podendo repetir também `periodo_trancado` (`:900`; `app/ponto/services/fechamento.py:1438`).
- **Consultas posteriores:** `_no_app` faz consulta do colaborador e das perguntas por colaborador (`app/chamados/juizes.py:1163`; `app/chamados/services/pergunta_no_app.py:42`). Estas últimas carregam só `disputa`, enquanto C6 acessa `celula` (`app/chamados/juizes.py:757`), introduzindo consultas por pergunta.
- **Não identifiquei chamador de produção por célula ou request** na busca dos símbolos; caminhos encontrados são vigia, lavrador e pautas. A tela declara ler o retrato gravado (`app/folha/views_retrato.py:8`); lavrador chama por empresa×competência (`app/folha/management/commands/lavrar_retrato_folha.py:54`, `:65`; `app/folha/retrato_folha.py:1157`); pautas alimenta por empresa (`app/ponto/services/pautas_dos_discordantes.py:66`, `:74`). Tempo e contagem observados em produção: `[nao verificado]`.

Textos/docstrings que excedem a garantia:

- **“Consultas por lote, nunca por dia”** (`app/chamados/juizes.py:1130`) não descreve os acessos de C6 e `_no_app` acima.
- **“Alimentado UMA vez por rodada”** (`app/chamados/services/vigia_fio.py:514`) ignora as alimentações indiretas por empresa×competência (`:549`; `app/folha/fora_da_folha.py:138`).
- **`DESTRAVA['admin']` promete validar resposta na disputa** (`app/folha/fora_da_folha.py:41`), mas `gps_fora_geofence` recebe `validar` independentemente de pergunta (`app/chamados/catalogo/natureza.py:29`, `:46`, `:59`); chamado desse módulo ligado à célula abre porta e produz fio `admin` (`app/chamados/juizes.py:1223`, `:1175`).
- **Pergunta sem crédito não garante âncora em `decidir`**, como afirma `app/chamados/juizes.py:1211`: auditoria pode resultar em `None`, geofence em `validar`, outra pergunta creditada em `cobrar`/`validar`, e silêncio em `historico` (`app/chamados/catalogo/natureza.py:44`; `app/chamados/services/verbos.py:47`).
- **`DESTRAVA['nada']` diz “ninguém está esperando ninguém”** (`app/folha/fora_da_folha.py:46`), embora ausência de porta ainda possa corresponder a decisão pendente, ausência aberta ou mesa do admin (`app/chamados/services/vigia_fio.py:288`, `:609`). O próprio juiz exclui essas outras entradas do conceito de porta (`app/chamados/juizes.py:1214`).
- **Prosa residual desatualizada:** grupo C ainda é descrito como “chamado vivo sem pergunta viva” (`app/folha/fora_da_folha.py:10`); vigia ainda diz que folha julga via `.viva` (`app/chamados/services/vigia_fio.py:129`), mas o leitor atual lê `porta['fio']` (`app/folha/fora_da_folha.py:67`).

## 31/10 03:38 · sessao df5e7c1b8947 · - · - · origem=Ronald (tmux)

Conclusões dos itens 2 e 4:

- **Sem filtros e no mesmo estado/instante, o motor é igual:** juiz usa `particionar_verbos(qs_vivos())`; painel usa `particionar_verbos(qs_vivos(_base=montar_qs_filtrado(...)))`. Sem filtros, as bases coincidem. `select_related`, `prefetch_related`, ordenação e anotações não acrescentam filtro; a partição remove prefetch e lê colunas. (`app/chamados/juizes.py:1158`; `app/chamados/services/painel.py:96-155,198,228`; `app/chamados/services/verbos.py:34-46`)

- **Filtros podem mudar o verbo do mesmo chamado:** C normal na fila; D urgente vivo; pergunta respondida com destino D e disputa.chamado=C. Global: C=`decidir`, D=`validar`. Com `urgencia=normal`, D sai do universo e o crédito volta para C, que vira `validar`. É consequência dos filtros antes da partição e da preferência pelo destino somente se pertencer ao conjunto recebido. O caso é deduzido do código; ocorrência em produção `[nao verificado]`. (`app/chamados/services/painel.py:117-130,228`; `app/chamados/services/raiox.py:54-56,133-149`; `app/chamados/catalogo/natureza.py:48-64`)

- **Empresas visíveis não filtram a lista padrão nem seu universo de verbos:** servem aos dropdowns. Empresa inativa/de diagnóstico pode entrar no universo global. O vigia, por sua vez, recorta seu universo de dias a empresas visíveis, sem limitar a partição global do juiz. (`app/colaboradores/queries.py:78-87`; `app/chamados/services/painel.py:96-132,436,542`; `app/chamados/services/vigia_fio.py:535-539`; `app/chamados/juizes.py:1158`)

- **O badge normalmente não particiona ao vivo:** devolve `PlacarNavbar.badge_chamados`; só sem lavra usa a partição atual, e em outra exceção devolve a quantidade em `qs_na_fila`. A lavra vem do painel sem filtros e soma apenas `validar`+`decidir`. Portanto igualdade temporal com juiz/painel não é garantida. (`app/core/context_processors.py:68-93,110-114`; `app/core/management/commands/lavrar_badge_navbar.py:39-41,72-74,91-93`; `app/chamados/services/verbos.py:22-27`)

- **Registrado/silêncio:** `registrado` permanece no universo vivo e pode receber crédito, mas sai tanto da porta quanto da lista padrão; silêncio futuro sai da fila, passado entra na fila mas recebe `historico`, depois removido de `fila` e da lista padrão. (`app/chamados/catalogo/motor.py:24-29`; `app/chamados/services/painel.py:155,186-188,314,326-329`; `app/chamados/services/verbos.py:48-51`; `app/chamados/juizes.py:1232`)

- **Verbo `None` não é ausência de porta:** chamado de auditoria aberto/em análise entra na fila, pode aparecer em `fila`, e produz fio `chamado`. Painel mostra `Registro`; resumo transforma `None` em `registro`; badge não o conta como admin. Isso é coerente com a implementação, mas não autoriza tratar todos os verbos de porta como ações. (`app/chamados/catalogo/natureza.py:18-23,44-45,63`; `app/chamados/juizes.py:1224,1232-1234`; `app/chamados/services/painel.py:381-386,472-484`; `app/chamados/services/verbos.py:125-135,22-27`)

- **Item 4:** em estado estável, não encontrei número de `fila` eliminado pelas regras da lista padrão sem filtros. `fila` exige cobrança e remove os mesmos verbos ocultos; painel exige cobrança e remove a mesma constante. Paginação e grupos inicialmente recolhidos podem impedir exibição imediata, sem eliminar o chamado do conjunto. Concorrência entre consultas/requests não tem snapshot garantido. (`app/chamados/juizes.py:1150-1158,1223,1232`; `app/chamados/services/painel.py:314,326-329,340-341,496-497`; `app/templates/chamados/partials/_lista_resumo.html:57-61`)

Falhas de prova/texto:

- O auxiliar de teste do vigia **continua chamando a fila de universo do painel**; seus casos simples não expõem a divergência curada. (`app/chamados/tests/test_o261e_f5_alcance.py:281-299`)
- Os novos testes do juiz comparam outro chamado ao mesmo motor global, sem exercitar `montar_contexto_painel` nem o badge real. Mordem a regressão para `qs_na_fila`, mas não provam a promessa ampla de tela/badge. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:87-92,192-224`)
- A afirmação “pergunta viva que o crédito não vê deixa o verbo da âncora em decidir” é excessiva: auditoria dá `None`; geofence dá `validar`; silêncio passado pode sobrepor `historico`; outras perguntas creditadas à âncora também alteram o verbo. (`app/chamados/juizes.py:1211`; `app/chamados/catalogo/natureza.py:44-51`; `app/chamados/services/verbos.py:48-51`)
- As docstrings que dizem “mesmo universo do painel e badge” precisam restringir painel sem filtros e distinguir o badge lavrado do fallback vivo. (`app/chamados/juizes.py:1135-1139,1206-1208`; `app/chamados/services/vigia_fio.py:172-173`; `app/core/context_processors.py:68-93`)

## 31/10 03:38 · sessao c5fa694cf9ae · - · - · origem=Ronald (tmux)

Contrato de `alimentar_portas` — escritor inicial único: `app/chamados/juizes.py:1148` a `:1159`; leitores por chave:

| Chave | Escrita | Leitura direta |
|---|---|---|
| `verbo` | `particionar_verbos(qs_vivos())['mapa']`, `juizes.py:1158` | `porta_alcancavel_do_dia`: `.get()` na fila, `:1224`; índice na âncora, `:1228`; filtro do número visível, `:1232` |
| `fila_ids` | inicializa/adiciona em `juizes.py:1148`, `:1151`; retorna `:1158` | pertinência da âncora, `juizes.py:1227`; `censo_alcance`, `vigia_fio.py:566`, usado para véu em `:619` |
| `fila_cel` | `juizes.py:1149`, `:1153`, `:1158` | `porta_alcancavel_do_dia`, `juizes.py:1223` |
| `vivas` | `juizes.py:1154` a `:1158` | porta, `juizes.py:1225`; vigia, `vigia_fio.py:571`, com usos em `:576`, `:595`, `:616` a `:645` |
| `colab` | `juizes.py:1159` | porta consulta colaborador da célula em `juizes.py:1227` |
| `no_app` | inicial vazio, `juizes.py:1159`; cache lazy `_no_app`, `:1167` | `_no_app`, `juizes.py:1163`, `:1168`; chamados em porta `:1227` e vigia `vigia_fio.py:580` |

Passadores do contrato, sem acesso direto a suas chaves: `folha/fora_da_folha.py:53`, `:58`, `:138`, `:190`; `ponto/services/pautas_dos_discordantes.py:74`, `:76`; teste de equivalência de lote em `chamados/tests/test_o261e_f5b_juiz_porta.py:187`, `:188`, `:189`. A chamada avulsa do juiz cria alimentação em `chamados/juizes.py:1222`.

Não encontrei leitor atual deste dict tomando as chaves de `verbo` como conjunto da fila: o antigo caso do vigia agora lê `fila_ids` (`app/chamados/services/vigia_fio.py:566`), e a porta usa `fila_ids` (`app/chamados/juizes.py:1227`). Não encontrei dict manual de alimentação em testes; os dicts manuais e mocks são **saída da porta**: `app/folha/tests/test_haiku_fora_da_folha.py:101`, `:129`, `:143`; `app/chamados/tests/test_o261e_f5_alcance.py:306`. O teste do lote usa o alimentador real (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:187`).

Há helper antigo que ainda chama o universo errado e se apresenta como painel: `app/chamados/tests/test_o261e_f5_alcance.py:281` a `:285` particiona `qs_na_fila()`; seus usos estão em `:293`, `:298`. O helper F5B foi corrigido para `qs_vivos()` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:87` a `:92`). Isso não é leitor de `alimentar_portas`, mas deixa uma referência de teste enganosa.

Pertinência da porta, com alimentação real e estado estável:

- Fila e âncora continuam provenientes de `qs_na_fila`: `fila_cel` e `fila_ids` nascem da mesma iteração (`app/chamados/juizes.py:1150` a `:1153`); a fila lê `fila_cel` (`:1223`), a pergunta exige `anc in fila_ids` (`:1227`). A âncora é destino, com fallback para chamado da disputa (`app/chamados/services/fio.py:116` a `:117`).
- Estar na fila é condição necessária da âncora, não suficiente da pergunta: ainda exige pergunta viva, resposta não vazia se respondida e presença no app (`app/chamados/juizes.py:1155` a `:1157`, `:1227`). Os testes cobrem resposta vazia e fora do app (`app/chamados/tests/test_o261e_f5_alcance.py:145`, `:194`).
- Silêncio vigente fica fora da fila; silêncio vencido pode estar nela, pois o predicado exclui somente `silenciado_ate > now` (`app/chamados/services/painel.py:186` a `:188`). O vencido conserva porta, mas perde número em `fila` se o verbo for `historico` (`app/chamados/juizes.py:1232`; teste `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`).
- `registrado` está em `VIVOS`, mas não em `COBRANCA`, portanto não entra em `qs_na_fila` (`app/chamados/catalogo/motor.py:24`, `:29`; `app/chamados/services/painel.py:187`), coberto em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:250` a `:257`.

Ressalva de consistência: fila e mapa de verbos são consultas separadas (`app/chamados/juizes.py:1150`, `:1158`), e `alimentacao` fornecida não é revalidada (`:1222`). Se um chamado deixar `VIVOS` entre essas consultas, a fila pode produzir verbo `None` pelo `.get()` (`:1224`), enquanto a mesma situação na porta da pergunta pode lançar `KeyError` pelo índice (`:1228`); ocorrência em produção `[nao verificado]`.

_886 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
