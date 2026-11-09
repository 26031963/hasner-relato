# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 05:24** · publicado em **09/11/2026 05:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 05:24 · sessao bbeb3f354882 · PAR-NAO-ADJACENTE · verificador · origem=Code

**A v4 cura a inversão entre intervalo e pausa curta. A aplicação sobre atas já carimbadas e o RED integrado continuam sem certificação.** A cura está em [escala/utils.py:455](/home/ronald/codex_ws/d361dfee6b22a10d4dd4298b47e3912f3f91b4ae/app/escala/utils.py:455); o pulo por impressão igual continua em `app/ponto/services/cartorio.py:1358–1364`.

Executei em memória os 17 testes das duas classes sem banco, com adaptação de timezone: passaram. Isso não certifica os testes de integração; execução completa em Django: **[nao verificado]**. As classes estão em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:50,218,313`.

1. **Achados do opus e cura na v4**

| Achado | Resultado |
|---|---|
| **1 — pausa curta veta o intervalo que desconta dinheiro** | **Curado na transcrição:** a pausa sobreposta sai antes da formação de `_pontas_par`, deixando o intervalo externo passar. Cura: `app/escala/utils.py:455–459,475–479`. Mordida real: teste aninhado curto em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182–194`; mordida por consultas simuladas, nos dois cruzamentos: `:287–310`. |
| **2 — sonda conta geometria, não impacto em ata/dinheiro** | **Continua aberto.** A v4 acrescenta asserções de papéis, alinhamento e minutos, mas elas não medem a frota: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:88–105,182–194,397–410`. Os números da sonda e seu impacto efetivo: **[nao verificado]**. |
| **3 — alcance do deploy e rota de aplicação** | **Não curado pelo delta.** A declaração continua na versão 4, e célula em dia continua pulada sem `forcar`: `app/ponto/services/cartorio.py:132,1348–1364`. A porta aceita `--apply` e `--forcar`: `app/ponto/management/commands/processar_cartorio.py:27–29,65`. “Zero células” exige qualificar “já carimbadas, com os demais insumos iguais”; alcance efetivo e situação da competência: **[nao verificado]**. |
| **4 — RED falsamente rotulado e builder sem RED comprovado** | Os três prefixos indevidos foram retirados: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:235,243,249`. O builder ainda tem o nome RED e asserções relevantes em `:397–410`; sua execução vermelha contra a base em Django permanece **[nao verificado]**. |
| **5 — “aninhado fica como hoje” e interno não adjacente** | A afirmação foi delimitada, e `_conflitos` restringe intervalos conflitantes ao casamento adjacente: `app/escala/utils.py:441–443,461–465`. O caso real preserva o interno adjacente (`teste:166–180`); os mocks agora preservam a resposta antiga de ambos soltos (`teste:266–285`). Esses testes caracterizam uma resposta; não resolvem a lei aberta entre dois intervalos. |

A cura **não recupera automaticamente a ata original de 660 minutos**. A ata já virada descreve o intervalo externo de duas horas; a transcrição passa a respeitar esse intervalo. O cenário externo produz 599 minutos, enquanto a fixture com intervalo `21:01→22:00` preserva 660: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:88–105,367–370,389–410`. Uma relavra integrada da **ata já virada**, seguida de outra relavra para comprovar convergência desse cenário específico: **[nao verificado]**.

O mutante `_n = 1` ganhou mordida específica no terceiro par: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142–164`. O mérito restante das duas triagens resumidas no anexo, sem sua fundamentação completa: **[nao verificado]**.

2. **Fronteira inclusiva e casos de borda**

Os dois leitores dividem **a mesma lista de pares marcados** pelo piso: intervalo com duração `>= 900 s`; pausa com duração `< 900 s`. Eles próprios não excluem sobreposição: `app/ponto/turnos.py:499,544–546,565–566`.

Considerando intervalo `I=[20:00,22:00]`:

| Pausa curta | Pareador real | Transcrição v4 |
|---|---|---|
| **Só toca antes:** `[19:58,20:00]` | Os dois pares exigiriam uma ponta compartilhada ou dois objetos no mesmo timestamp. O produtor não reutiliza identidades; o modelo declara unicidade por colaborador/timestamp: `app/ponto/turnos.py:811–822,883–885`; `app/ponto/models.py:106`. | Se consultas simuladas fornecerem ambos, elimina a pausa pelo `ini <= z`: `app/escala/utils.py:456–457`. |
| **Só toca depois:** `[22:00,22:02]` | Mesma restrição de identidade/unicidade: `app/ponto/turnos.py:815–822,883–885`; `app/ponto/models.py:106`. | Se fornecidos ambos, elimina a pausa pelo `a <= fim`: `app/escala/utils.py:456–457`. |
| **Contida:** `[21:01,21:02]` | **Produzível:** externo declarado por `Xi`; interno formado pelos vizinhos residuais. As consultas devolvem, respectivamente, externo e interno: `app/ponto/turnos.py:811–816,819–883,545–546,565–566`. | Elimina a pausa curta e mantém o externo; caso real em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182–194`. |
| **Cruza uma ponta:** `[19:59,20:01]` ou `[21:59,22:01]` | Com timestamps únicos, **não nasce desse produtor**: declarados são pareados sequencialmente; o guloso só casa vizinhos físicos e não pode usar ponta declarada: `app/ponto/turnos.py:811–822,883–885`. | Se fornecidos por mock, elimina a pausa e mantém o intervalo: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:287–310`. |

**A mesma batida não é ponta de dois pares produzidos:** as identidades declaradas são reservadas, e o guloso avança duas posições ao emitir. Na transcrição, `_fins` também impede reutilizar o fechamento como outra abertura: `app/ponto/turnos.py:815–822,883–885`; `app/escala/utils.py:468–478`.

O teste de timestamps repetidos demonstra reutilização de **índice `k`**, mas não contém pares; não demonstra uma batida em dois pares. Além disso, essa duplicação contraria a unicidade declarada: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249–255`; `app/ponto/models.py:106`. A constraint efetivamente instalada no banco de produção: **[nao verificado]**.

3. **Quem produz dois intervalos sobrepostos; há perda contra a base?**

**Existe entrada real:** intervalo externo declarado mais intervalo interno guloso, ambos acima do piso. O teste real produz `12:00→13:00` e `12:10→12:30`: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:166–176`. A origem são as duas passadas de `_pares_marcados`: `app/ponto/turnos.py:811–816,819–885`.

Nesse arranjo:

- O externo encontra as pontas internas e é vetado: `app/escala/utils.py:475–476`.
- O interno guloso é fisicamente adjacente e passa por `_conflitos`: `app/escala/utils.py:464–465`; a conservação é verificada em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:175–180`.

**Dois intervalos conflitantes podem ficar ambos soltos nos mocks**, mas esses exemplos também não tinham par adjacente na base reconstruída pelo diff fornecido: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266–285`. Comparação direta com o objeto Git `b57ce0a1`: **[nao verificado]**.

No domínio com timestamps únicos, **não há perda de intervalo adjacente causada por `_conflitos`**: um par adjacente antigo satisfaz `_pos(z)==_pos(a)+1`, e o teste de pontas internas percorre um intervalo vazio. São precisamente as guardas atuais de `app/escala/utils.py:465,475–476`.

Existência e frequência desses arranjos na frota: **[nao verificado]**.

4. **Quem escreve e quem lê**

| Valor | Escritor → leitor |
|---|---|
| `Batida.timestamp`, `tipo` | Registro em `app/ponto/registro_batida.py:152–158` → montagem de `locs` em `app/escala/utils.py:1332` e ordenação do turno em `:451`. |
| Papel da ata `E/S/X/Xi` | Mapa escrito em `app/ponto/turnos.py:1804–1806,1827–1828` → `_papel_da_ata` em `:904–909`; marcas aplicadas em `:1315–1325`. |
| Pares e classificação pelo piso | `_pares_marcados`, `app/ponto/turnos.py:811–885` → divisão em `:545–546,565–566` → transcrição em `app/escala/utils.py:455–458`; realizado lê intervalos em `app/ponto/turnos.py:756–759`. |
| `turno` | Consulta/indexação por `data_turno` em `app/escala/utils.py:1590–1592`; seleção em `:1602–1605` → matcher em `:1710–1712` → helper em `:605`. |
| `locs`, `consumed` | `locs` montado e deduplicado em `app/escala/utils.py:1332–1348`; consumo inicial em `:1349` → helper lê em `:448–453`; matcher escreve consumo em `:631–632`. |
| `_idx`, `_bats`, `_ks` | Construídos em `app/escala/utils.py:448–452` → guarda em `:453`, emissão em `:466–483`. `_idx` conserva o primeiro `k` de cada timestamp: `:450`. |
| `_intervalos`, `_pausas`, `_pares` | Construídos em `app/escala/utils.py:455–458` → pontas, conflitos e mapa inicial em `:459–465`. |
| `_pontas_par`, `_pos`, `_conflitos`, `_ini_par` | Escritos em `app/escala/utils.py:459–465` → consultados no reconhecimento dos pares em `:473–476`. |
| `_n`, `_fins`, `out`, `(k,papel)` | Inicializados e emitidos em `app/escala/utils.py:466–483` → três leitores lógicos abaixo. |
| Associação marco→`k` | Alinhador escreve em `app/escala/utils.py:567–591` → matcher transforma em células/órfãs em `:625–632`. |
| Células, órfãs e ata | Células em `app/escala/utils.py:628`; órfãs em `:630`; lâmpadas/ata em `app/ponto/services/cartorio.py:677–711` → lavra em `:1800–1802` → persistência em `app/ponto/portas/celula.py:1027–1035`. |
| Minutos realizados | Juiz calcula em `app/ponto/turnos.py:756–771` → builder consulta em `app/escala/utils.py:2054–2067`. |

Há **um chamador produtivo direto** de `_papeis_do_turno`: `app/escala/utils.py:605`. Seus **três leitores lógicos** são:

- Guarda L‑084: `app/escala/utils.py:614–620`.
- Alinhador, incluindo índice `k`, número e lado do par: `app/escala/utils.py:622,533–540`.
- Conservação de órfãs e consumo: `app/escala/utils.py:629–632`.

Há também três chamadas de teste: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:76,233,407`.

Ata, Zona 5 e juiz da volta não são três chamadores diretos desse helper: os caminhos sem turno tentam `_papeis_por_alternancia`. As portas estão em `app/ponto/motor_calculo_v2.py:3062`, `app/escala/utils.py:427,605–607`.

5. **Classes que já reprovaram obras**

| Classe | Avaliação |
|---|---|
| **Segundo juiz** | Existe seleção local adicional, portanto “transcrição” não significa copiar todos os pares: `app/escala/utils.py:455–476`. O registro separa a autoridade de intervalo da autoridade de alinhamento: `app/core/juizes.py:200–202`. O diff altera esta última; não cria outro cálculo de minutos. |
| **Derivação paralela de fonte única** | A soma continua no juiz, consultando `intervalos_do_turno`: `app/ponto/turnos.py:756–771`; o builder lê essa resposta em `app/escala/utils.py:2054–2067`. No aninhado acima do piso, a projeção ainda não transporta ambos os intervalos: comportamento preservado em `teste:166–180`, enquanto o realizado soma os intervalos em `turnos.py:758–759`. |
| **Relógio fora da borda** | Não há nova leitura de `now()`/data corrente no trecho alterado; há conversão dos timestamps recebidos: `app/escala/utils.py:437–483`. |
| **RED/selo sem mordida** | Os três rótulos falsos foram retirados; os mutantes relevantes têm mordida descrita abaixo. RED integrado do builder permanece **[nao verificado]**. Referências: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142,182,235,243,249,266,287,397`. |
| **Campo com dois significados** | O veto de contato é inclusivo, enquanto o impedimento por pontas internas é estrito: são predicados distintos em `app/escala/utils.py:457,463,475–476`. O helper não deriva dia civil; o builder seleciona pelo `data_turno`: `:1591–1605`. A linha civil `:1829` já existia e apenas mudou de posição no diff fornecido. |

6. **Mutantes sobreviventes**

| Mutante | Ensaio dos 17 casos em memória | Mordida |
|---|---|---|
| `_pausas`: trocar ambos os `<=` por `<` | **Sobrevive** | Não há fixture de toque; expressão em `app/escala/utils.py:457`. |
| `_conflitos`: trocar ambos os `<=` por `<` | **Sobrevive** | Não há fixture de toque; expressão em `app/escala/utils.py:463`. |
| Remover filtro de `_pausas` | **Morre** | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182,287`. |
| Remover restrição de `_conflitos` | **Morre** | Somente o mock aninhado com interno não adjacente: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266`. |
| `_n += 1` → `_n = 1` | **Morre** | Terceiro par: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142–164`. |

Para **TODOS os testes do arquivo**, a sobrevivência dos comparadores estritos é sustentada pela inspeção das fixtures, sem execução completa: os casos adicionais não apresentam toque entre pares; o anexo declara `21:01:59→22:00`. Referências: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:379–407,416,441,455,466,477,524,535,619,638,670`. Resultado completo em Django: **[nao verificado]**.

Esses sobreviventes podem ser **equivalentes no domínio que respeita a unicidade declarada**, pois contato exigiria compartilhar batida ou timestamp. Não constituem, isoladamente, prova de cobertura insuficiente nesse domínio: `app/ponto/turnos.py:811–822,883–885`; `app/ponto/models.py:106`.

7. **Catraca `LINHAS_REGISTRADAS`**

**Sim: os +8 do delta apontam às mesmas expressões.** São **+8 de v3 para v4**; o diff total fornecido indica **+9 contra a base**. Destinos registrados em `app/core/tests/test_medida_turno_do_par.py:247–249,281–282`; conferência direta pelo Git: **[nao verificado]**.

| v3 → v4 | Expressão preservada em `app/escala/utils.py` |
|---|---|
| 497 → **505** | `enumerate(zip(_ks[1:-1:2], _ks[2:-1:2]))` |
| 521 → **529** | `list(range(1, _M - 2, 2))` |
| 797 → **805** | `_idx_por_data.get(locs[_k][0].date())` |
| 1481 → **1489** | `min(_l[0].date() ...)` |
| 1501 → **1509** | busca com `_locs_i[_j][0].date() >= floor` |
| 1511 → **1519** | diferença `_locs_i[0][0].date() - _pre_ini` |
| 1524 → **1532** | `_locs_i[_anc_ix][0].date()` |
| 1535 → **1543** | filtro `_locs_i[k][0].date() == d` |
| 1821 → **1829** | `_idx_por_data.get(locs[_k][0].date())` |

A igualdade das linhas reais com `LINHAS_REGISTRADAS` passou no ensaio com `unittest`, substituindo a base Django; a asserção está em `app/core/tests/test_medida_turno_do_par.py:534–538`. Essa catraca verifica localização/identidade das ocorrências; não certifica a regra de conflito nem o impacto financeiro: `:528–538`.

## 09/11 05:22 · sessao 11864eaa6042 · - · - · origem=Ronald (tmux)

- **Toque numa ponta:** o filtro v4 excluiria a pausa, pois usa interseção inclusiva (`app/escala/utils.py:456-457`). O pareador de batidas persistidas válidas não produz esse toque: declarações consomem identidades (`app/ponto/turnos.py:811-822`), o guloso consome dois vizinhos (`app/ponto/turnos.py:819`, `app/ponto/turnos.py:883-885`), e duas batidas do mesmo colaborador não podem ter o mesmo timestamp (`app/ponto/models.py:105-106`).

- **Pausa contida:** existe no pareador real: intervalo externo declarado `Xi`, pausa interna de duas batidas residuais vizinhas (`app/ponto/turnos.py:1316-1325`, `app/ponto/turnos.py:811-816`, `app/ponto/turnos.py:819-883`). As consultas separam intervalo `>=900s` e pausa `<900s` (`app/ponto/turnos.py:544-546`, `app/ponto/turnos.py:565-566`). A v4 elimina a pausa e transporta o externo; o teste real cobra papéis e alinhamento (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182-194`).

- **Pausa cruzando estritamente:** não é saída desse produtor com timestamps únicos: declarados são pares consecutivos ordenados; o guloso só casa vizinhos e não pode consumir uma ponta já declarada (`app/ponto/turnos.py:811-822`, `app/ponto/turnos.py:883-885`). O teste cruzado fornece as consultas por mock e declara essa limitação (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:229-233`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:287-310`).

- **Dois intervalos sobrepostos existem:** externo declarado `12:00→13:00`, interno guloso adjacente `12:10→12:30`; a fixture usa o pareador real e verifica ambos (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:166-176`). A v4 veta o externo, mantém o interno adjacente e preserva o alinhamento caracterizado do HEAD (`app/escala/utils.py:462-476`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:175-180`).

- **Ambos descartados:** ocorre nos mocks de dois intervalos não adjacentes; os testes caracterizam que o HEAD também não tinha nenhum desses pares (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266-285`). No produtor real, um intervalo aninhado residual é fisicamente adjacente e passa pela exceção de `_conflitos` (`app/ponto/turnos.py:819`, `app/ponto/turnos.py:883-885`, `app/escala/utils.py:464-465`).

- **Perda versus HEAD por `_conflitos`, com timestamps únicos:** não. Um par aceito pelo HEAD tinha fechamento em `i+1` (trecho HEAD do anexo `diff_v4.diff`, sem arquivo numerado: [não verificado]); portanto passa pela exceção adjacente e tem faixa interna vazia na v4 (`app/escala/utils.py:464-476`). Existência desses arranjos na frota: [não verificado].

- **Mesma batida como ponta de dois pares:** não no produtor: os declarados são retirados da passada gulosa, que avança dois elementos após emitir (`app/ponto/turnos.py:815-822`, `app/ponto/turnos.py:883-885`). No transcritor, `_fins` pula a ponta final quando a varredura chega nela (`app/escala/utils.py:468-478`). A fixture de timestamps repetidos reutiliza `k`, mas não contém pares (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249-255`).

Executei também entradas sintéticas com timestamps exatamente repetidos, por extração AST e shim de timezone; elas ficam fora da unicidade do modelo (`app/ponto/models.py:106`). Na entrada `E08,S12,E13,S13,E13:02,S17`, o HEAD reconstruído conserva a pausa e a v4 descarta ambos; essa comparação é execução exploratória sem Django instalado, não prova de regressão em produção [não verificado].

## 09/11 05:21 · sessao fae75912abde · - · - · origem=Ronald (tmux)

6. Verificação em memória: os 17 testes das classes `ParNaoAdjacente` e `PapeisDoTurnoLimites` passaram com os corpos reais extraídos e adaptação mínima de timezone; execução Django dos 36 testes completos: [não verificado]. As duas classes estão em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:50` e `:218`; os testes restantes dependem de `TestCase` e fixtures ORM em `:313`, `:317`, `:325`.

| Mutante | Resultado nos 17 | Teste que morde | [nao verificado]
|---|---|---|
| Filtro `_pausas`: ambos `<=` → `<` | Sobrevive | Nenhuma fixture de toque; comparador em `app/escala/utils.py:456` |
| `_conflitos`: ambos `<=` → `<` | Sobrevive | Nenhuma fixture de toque; comparador em `app/escala/utils.py:462` |
| Remover filtro `_pausas` | Morre | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182`, `:287` |
| Remover filtro de `_conflitos` | Morre | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266` |
| `_n += 1` → `_n = 1` | Morre | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142` |

A inspeção estática dos demais testes também não apresenta toque entre pares: as fixtures usuais têm no máximo quatro batidas no dia, deixando no máximo duas internas depois da exclusão das bordas; o anexo declara `21:01:59→22:00` e suas duas extras ficam soltas. Isso sustenta sobrevivência por análise, sem afirmar execução dos 36. `app/ponto/turnos.py:802`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:379`, `:401`, `:405`, `:416`, `:441`, `:455`, `:466`, `:477`, `:524`, `:535`, `:619`, `:635`, `:670`.

A sobrevivência dos comparadores estritos não basta para concluir falta de cobertura no domínio persistido: o produtor não reutiliza a mesma batida em dois pares, e a unicidade impede duas batidas do colaborador no mesmo timestamp. `app/ponto/turnos.py:811`, `:820`, `:883`; `app/ponto/models.py:105`.

7. **Sim: os +8 do delta preservam os textos das ocorrências.** Os nove destinos atuais correspondem às mesmas expressões, deslocadas de v3 para v4; contra a base do diff total, o deslocamento é **+9**, não +8. `app/core/tests/test_medida_turno_do_par.py:247`, `:248`, `:249`, `:281`, `:282`; `delta_v4.diff` e `diff_v4.diff` fornecidos [não verificado por git].

| v3 → v4 | Texto atual |
|---|---|
| 497 → 505 | `for _n, (_ks_s, _ks_e) in enumerate(zip(...))` |
| 521 → 529 | `_pausas = list(range(1, _M - 2, 2))` |
| 797 → 805 | `_idx = _idx_por_data.get(locs[_k][0].date())` |
| 1481 → 1489 | `_d_min_locs = min(_l[0].date() ...)` |
| 1501 → 1509 | `_ix_janela = next(...date() >= floor)` |
| 1511 → 1519 | `_mudos = (...date() - _pre_ini).days` |
| 1524 → 1532 | `_d_anc = _locs_i[_anc_ix][0].date() ...` |
| 1535 → 1543 | `if (not _cons_i[k]) and ...date() == d]` |
| 1821 → 1829 | `_idx = _idx_por_data.get(locs[_k][0].date())` |

Destinos: `app/escala/utils.py:505`, `:529`, `:805`, `:1489`, `:1509`, `:1519`, `:1532`, `:1543`, `:1829`. O teste real da igualdade de `LINHAS_REGISTRADAS` passou em unittest puro, substituindo somente a importação de `SimpleTestCase`. `app/core/tests/test_medida_turno_do_par.py:534`.

## 09/11 05:19 · sessao 81ffe87c376b · - · - · origem=Ronald (tmux)

**4. Escritores e leitores dos valores**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `Batida.timestamp`, `tipo` | `app/ponto/registro_batida.py:152`, `:157` | pareador; construção de `locs` em `app/escala/utils.py:1332`; transcrição ordena timestamps em `:451` |
| `papel_por_minuto` (`E/S/X/Xi`) | `app/ponto/turnos.py:1804`–`:1806`; declaração `Xi` no final desse bloco | `_papel_da_ata`, `app/ponto/turnos.py:904`–`:909` |
| marcas `_papel_ata`, `_intra_dur`, `_intra_ata`, `_intra_declarado` | `app/ponto/turnos.py:1315`–`:1325` | `_pares_marcados`: declarados primeiro em `:811`–`:816`; restantes em `:819`–`:884` |
| `intervalos_do_turno` / `pausas_abaixo_do_piso` | `_pares_marcados`, dividido pelo piso: `app/ponto/turnos.py:545`–`:546`, `:565`–`:566` | `_papeis_do_turno`, `app/escala/utils.py:455`–`:458`; dinheiro lê intervalos em `app/ponto/turnos.py:756`–`:759` |
| `turno` | builder consulta `turnos_do_colab` em `app/escala/utils.py:1590`; indexa pelo `data_turno` em `:1591`–`:1592`; seleciona em `:1602`–`:1605` | `_match_marcos(turno=...)`, `app/escala/utils.py:1710`–`:1712`, depois `_papeis_do_turno` em `:605` |
| `locs` | `app/escala/utils.py:1332`, deduplicação `:1335`–`:1348` | índice timestamp→primeiro `k` em `:448`–`:452`; alinhamento em `:531`–`:540` |
| `consumed` | inicializado `False` em `app/escala/utils.py:1349`; escrito pelo matcher em `:631`–`:632`; outros caminhos `:673`, `:1584`, `:1844` | guarda de transcrição em `:453`; seleção e conservação em `:638`, `:1827` |
| `_intervalos`, `_pausas`, `_pares`, `_pontas_par`, `_pos`, `_conflitos`, `_ini_par` | exclusivamente `app/escala/utils.py:455`–`:465` | reconhecimento de ponta/impedimento no laço `:469`–`:478` |
| `_n`, `_fins`, `out`, `(k,papel)` | `app/escala/utils.py:466`–`:483` | leitores descritos abaixo |
| `_ass` (marco→`k`) | `_alinhar_por_papel`, `app/escala/utils.py:567`–`:591` | células e órfãs em `:625`–`:630` |
| células `tipo/papel/hora/delta/status/tipo_real` | `_celula_do_marco`, `app/escala/utils.py:325`–`:327`; chamada `:628` | projeção por marco em `:1714`–`:1717`; montagem do dia em `:1816`–`:1820`; lâmpadas `app/ponto/services/cartorio.py:677`–`:679` |
| ata e minuto | ata de trabalho em memória `app/escala/utils.py:1896`–`:1907`; minuto pedido ao juiz em `:2054`–`:2067`; `ata_do_dia` escreve campos em `app/ponto/services/cartorio.py:701`–`:711` | cartório encaminha à lavra em `:1800`–`:1802`; porta atribui/persiste em `app/ponto/portas/celula.py:1027`–`:1035` |

Há **um chamador produtivo direto**, `app/escala/utils.py:605`, e três chamadas de teste, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:76`, `:233`, `:407`. Os **três leitores lógicos do retorno** são: guarda L‑084 (`app/escala/utils.py:614`–`:620`), alinhador (`:622`, decodificação de `k/papel/n` em `:533`–`:540`) e conservação de órfãs/consumo (`:629`–`:632`). 

Ata, Zona 5 e `cumpriu_a_volta` não são três chamadas diretas dessa função: Zona 5 chama `_match_marcos` **sem turno** em `app/ponto/motor_calculo_v2.py:3062`; `marco_ocupado_pela_batida` faz o mesmo em `app/escala/utils.py:427`; nesse formato o matcher tenta `_papeis_por_alternancia`, `app/escala/utils.py:605`–`:607`.

**5. Classes de defeitos**

- **Seleção local adicional:** `_papeis_do_turno` não transcreve incondicionalmente: elimina pausas sobrepostas e veta intervalos em conflito não adjacentes (`app/escala/utils.py:455`–`:465`, `:475`–`:476`). O registro atribui “o vão foi intervalo?” a `intervalos_do_turno` e “qual marco ocupa?” a `_match_marcos` (`app/core/juizes.py:200`–`:202`). A regra nova cabe no alinhamento apenas se entendida como preservação do HEAD; como decisão sobre existência do intervalo, seria segundo julgamento. A autorização para decidir conflito acima do piso permanece aberta no anexo `[não verificado no arquivo da cópia]`.
- **Derivação de dinheiro:** o trecho alterado não soma minutos; o realizado continua vindo do juiz (`app/escala/utils.py:2054`–`:2067`), que desconta `intervalos_do_turno` (`app/ponto/turnos.py:756`–`:770`). Consequência indireta pela próxima ata existe no fluxo `app/escala/utils.py:1896`–`:1907`; efeito em produção `[não verificado]`.
- **Relógio:** o trecho modificado usa conversão de timestamps, sem novo `now()`/`localdate()` (`app/escala/utils.py:437`–`:483`). O builder recebe `agora` e deriva `ref` dele (`:1286`, `:1299`).
- **Fronteiras:** veto de toque inclusivo em `app/escala/utils.py:457`, `:463`; interior estrito em `:475`–`:476`. São critérios diferentes explícitos, mas a justificativa de lei específica para toque permanece `[não verificado]`.
- **Dia civil/dia do turno:** a transcrição usa instantes absolutos e não introduz `.date()` (`app/escala/utils.py:437`–`:483`); seleção usa `data_turno` (`:1591`–`:1592`), marcos encadeiam virada (`:345`–`:348`). A conservação já arquiva sobras não consumidas por dia civil (`:1829`); esse texto não foi alterado, apenas sua linha registrada.

## 09/11 05:15 · sessao c7cd8e1c6f6d · PAR-NAO-ADJACENTE · construtor · origem=Code

Corrigido: pausas abaixo do piso que disputam pontas com intervalos são retiradas do pareamento; suas batidas ficam `sol`. O conflito entre dois intervalos conserva o casamento adjacente atribuído ao b57ce0a1. [escala/utils.py:455](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:455)

Hunks entregues:

- Docstring e transcrição da prioridade, sem a afirmação falsa sobre aninhados. [utils.py:438](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:438)
- Teste aninhado reescrito: exige o par 20:00/22:00 e verifica `_alinhar_por_papel`, incluindo [nome]. [teste:182](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182) [nao verificado]
- Cruzamento testado nos dois sentidos, com consultas mockadas explicitamente. [teste:287](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:287) [nao verificado]
- Antigo teste:230 **reescrito**, caracterizando ambos os intervalos não adjacentes como soltos. [teste:266](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266) [nao verificado]
- `LINHAS_REGISTRADAS`: somente deslocamento de **+8 linhas**, com motivo registrado. [core/tests/test_medida_turno_do_par.py:247](/home/ronald/wt-codex-par-nao-adjacente-031500/app/core/tests/test_medida_turno_do_par.py:247)

Para cada teste novo ou alterado, estes são os resultados **esperados** no b57ce0a1; execução nessa revisão: **[nao verificado]**.

| Teste/hunk | Esperado | Por quê | [nao verificado]
|---|---|---|
| [Duas soltas:89](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:89), comentário corrigido | Falha | Exige par não adjacente. |
| [Três pares:142](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142) | Falha | Exige três pares não adjacentes, numerados 0/1/2. |
| [Dois intervalos, interno adjacente:166](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:166), docstring corrigida | Passa | Mantém o interno adjacente. |
| [Aninhado abaixo do piso:182](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182) | Falha | Exige o externo, anteriormente perdido. |
| [Pausa curta separada:196](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:196) | Passa | Ambos os pares são adjacentes e separados. |
| [Sem par:235](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:235) | Passa | Caracterização; removido `RED`. |
| [Par adjacente:243](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:243) | Passa | Caracterização; removido `RED`. |
| [Timestamp repetido:249](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249) | Passa | Caracterização; removido `RED`. |
| [Dois intervalos aninhados:266](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266) | Passa | Nenhum par é adjacente. |
| [Dois intervalos cruzados:277](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:277) | Passa | Nenhum par é adjacente. |
| [Pausa curta cruzada:287](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:287) | Falha | Exige preservar e alinhar o intervalo atravessado. |

Quem escreve e quem lê:

| Papel | Escritor | Leitor |
|---|---|---|
| `('par', n, S/E)` | `_papeis_do_turno` [477](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:477); `_papeis_por_alternancia` [506](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:506) | `_alinhar_por_papel`: agrupa por número/lado [538](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:538), senta o par [570](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:570). |
| `sol` | `_papeis_do_turno` [481](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:481) | `_alinhar_por_papel` [540](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:540), encaixe posterior [573](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:573). |
| `ent` / `sai` | `_papeis_do_turno` [466](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:466), [482](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:482); alternância [504](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:504), [507](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:507) | `_alinhar_por_papel` [533](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:533), [534](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:534). |

`_match_marcos` encaminha os papéis ao alinhador e percorre todos para órfãs/consumo. [utils.py:622](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:622), [630](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:630)

Ensaio reduzido: **17/17**; os oito mutantes terminaram com **rc=1 por asserção**. Comandos e limitações estão no [ensaio:3](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/ensaio_par_nao_adjacente.py:3):

```bash
export PYTHONDONTWRITEBYTECODE=1
ensaio_par=escala/tests/ensaio_par_nao_adjacente.py
python3 -B "$ensaio_par"
MUTANTE=sem_prioridade python3 -B "$ensaio_par"
MUTANTE=so_aninhado python3 -B "$ensaio_par"
MUTANTE=so_inicio python3 -B "$ensaio_par"
MUTANTE=so_fim python3 -B "$ensaio_par"
MUTANTE=apaga_pausas python3 -B "$ensaio_par"
MUTANTE=lei_nova_dois_intervalos python3 -B "$ensaio_par"
MUTANTE=numero_zero python3 -B "$ensaio_par"
MUTANTE=numero_um python3 -B "$ensaio_par"
python3 -B "$ensaio_par" --py-compile
```

`py_compile` passou nos quatro arquivos, interceptando escrita de bytecode. [ensaio:37](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/ensaio_par_nao_adjacente.py:37) Suite Django e builder: **[nao verificado]**; esta entrega não declara certificação. [nao verificado]

## 09/11 05:10 · sessao 781c071dafd8 · PAR-NAO-ADJACENTE · censo · origem=Code

Conto as linhas dos anexos a partir da primeira linha após `== anexo: ... ==`, incluindo linhas em branco. No `relato_par.txt`, uso os números já impressos.

1. **(a) — O tema estava presente; o pre-verificador descreveu o mecanismo, mas não apontou a contradição ata × intervalo descontado.** (`preverif_v1.q:11-15`; `preverif_v1.r:11`; `preverif_v2.r:31`)

   **Pergunta:** “QUEM ESCREVE e QUEM LE: papel 'par'/'sol' produzido aqui; `minutos_realizados` da ata; intervalos transportados ao motor. Todos supoem o mesmo significado”. (`preverif_v1.q:11-12`)

   **Resposta:** “O externo continua recusado; o interno continua aceito. Os testes fixam exatamente isso.” Depois, tratou como mutação reprovável: “Retirar `pausas_abaixo_do_piso` da composição de `_pares`”. (`preverif_v1.r:11`; `preverif_v2.r:31`) [nao verificado]

   A distinção também foi percebida: “o papel `par` inclui pausas abaixo do piso; o adaptador transporta somente intervalos de pelo menos 900 segundos”. Faltou relacioná-la ao veto do intervalo externo naquele teste. (`preverif_v1.r:48`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:162-169`)

   O descarte de “builder não relido da persistência” não documenta descarte dessa contradição específica. (`relato_par.txt:12`; `relato_par.txt:15`)

2. **(c) — Fora do alcance quanto à medição dos sete turnos de produção e ao impacto em dinheiro/ata.** O registro fornecido dizia expressamente “quantos sao aninhados: nao medido”. (`relato_par.txt:17`)

   **Pergunta:** o brief forneceu “Frota desde 21/09: 7 turnos de 12.498”; a pergunta ao pre-verificador foi estrutural: “para todo arranjo de batidas e pares do juiz [...] Liste os arranjos que mudam e os que nao mudam”. (`construtor_v1.q:8`; `preverif_v1.q:5-8`)

   **Resposta:** “O arranjo pode mudar: o externo permanece vetado, mas o interno passa se atravessar somente soltas.” Sobre dinheiro, delimitou: “Isso não demonstra igualdade com o resultado financeiro do motor.” Nenhum desses trechos constitui classificação ou medição financeira dos sete turnos. (`preverif_v1.r:12`; `preverif_v1.r:53`; `relato_par.txt:17`)

3. **(e) — NÃO PERGUNTADO: alcance no deploy e declaração da rota do apply.** Os três questionários do pre-verificador não pedem examinar `REGRA_VERSOES`, `--forcar` ou escolher/declarar essa rota. (`preverif_v1.q:5-18`; `preverif_v2.q:3-9`; `preverif_v3.q:3-7`) [nao verificado]

   **Pergunta mais próxima:** “Ha outros sitios com a MESMA exigencia de adjacencia que transcrevem o par do juiz para a ata/motor”. Isso pergunta sobre caminhos do defeito, sem perguntar sobre alcance de implantação. (`preverif_v1.q:17-18`)

   **Resposta específica sobre deploy/apply:** não há trecho correspondente a transcrever nos anexos de resposta. A falta de declaração aparece no laudo: “Rota do apply nao declarada”; depois, o RELATO manda “declarar rota”. (`opus_final_1.txt:4`; `relato_par.txt:3`) [nao verificado]

4. **O achado reúne duas classes: (a) para o rótulo RED falso; (c) para a execução contra o HEAD no Django.** (`preverif_v1.q:14-15`; `preverif_v2.r:19`; `preverif_v2.r:23`; `preverif_v2.r:38`)

   **Pergunta sobre RED:** “selo/RED sem mordida (cada teste novo falha no HEAD? qual mutante simples sobrevive?)”; na v2: “Os testes novos mordem? Para cada um, qual mutacao do codigo o reprova.” (`preverif_v1.q:14-15`; `preverif_v2.q:6`) [nao verificado]

   **Resposta sobre os três testes:** “Os três testes novos fixam as listas antigas para ausência de par, par adjacente e timestamps repetidos”. Reconheceu a preservação, mas não apontou o prefixo RED indevido, presente nos três nomes da cópia. (`preverif_v2.r:19`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:199`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:207`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:213`)

   **Resposta sobre execução:** “Isso é um ensaio reduzido; execução integral Django e integração com banco: **[nao verificado]**”; para o builder: “execução desse teste: **[nao verificado]**”. Essa limitação foi declarada, não passou despercebida. (`preverif_v2.r:23`; `preverif_v2.r:38`) [nao verificado]

5. **(b) quanto à objeção aos testes como caracterização; com ressalva: a descoberta da mudança do interno foi acolhida como PROCEDENTE, não descartada.** (`relato_par.txt:15`) [nao verificado]

   **Pergunta:** “o diff muda algum resultado ALEM do caso [...]? Liste os arranjos [...] (adjacente, aninhado, cruzado [...] )”. (`preverif_v1.q:5-8`)

   **Resposta:** “Portanto ‘aninhado não muda’ não vale sem qualificar o interior.” (`preverif_v1.r:12`)

   **Triagem e motivo:** “PROCEDENTES [...] aninhado com interno nao adjacente muda (interno vira par) -> teste + docstring”; separadamente, descartou “aninhados sao caracterizacao (nao RED)”. (`relato_par.txt:15`) [nao verificado]

   O encaminhamento tornou essa distinção explícita: “Isso e a cura aplicada ao par interno, nao o caso aninhado; mantenha o comportamento, mas escreva o teste que o fixa”. A resposta seguinte considerou “Contrato corrigido e caso acrescentado”. Assim, o comportamento questionado pelo opus foi visto e deliberadamente fixado após a triagem. (`construtor_v2.q:14-17`; `preverif_v2.r:11`) [nao verificado]

## 09/11 04:08 · sessao e275d0649b92 · SELOS-CANON-I1 · verificador · origem=Code

1. **OBSERVACAO — a volta 3 tem curas reais, mas não encerra todos os PROCEDENTES da triagem v2.**

| PROCEDENTE | Resultado |
|---|---|
| **P1/achado 4: PAREI no lugar errado** | **Curado localmente:** publica na primeira linha útil e relê pelo Stop. **Integração incompleta:** desloca o painel anterior e pode esconder a linha B3. `bin/disjuntor.py:299`; `bin/disjuntor.py:301`; `app/core/esteira_vigia.py:888`; `bin/hook_stop_fila1.py:442`. |
| **P2: existência/idempotência/trava** | **Curado dentro da porta:** leitura, comparação com painel vigente e gravação ficam sob a mesma trava; histórico inferior não impede republicação. Escritores externos continuam podendo perder essa atualização. `app/core/esteira_vigia.py:878`; `app/core/esteira_vigia.py:883`; `app/core/esteira_vigia.py:891`; `bin/gerar_cortes.py:136`. |
| **P3: briefs diferentes entre bocas** | **Parcial:** há coletor compartilhado, mas a CLI lê somente o conteúdo da pergunta; dos anexos recebe apenas caminhos. Referências internas aos anexos ficam ausentes do julgamento. `bin/disjuntor.py:1573`; `bin/disjuntor.py:2152`; `bin/disjuntor.py:2154`; `bin/codex_pergunta.sh:315`. |
| **P4: raiz errada** | **Curado para BACKLOG/briefs do julgamento:** wrapper passa a raiz viva. **Costura da medida incompleta:** aprovação e `ini` podem nascer em raízes diferentes. `bin/codex_pergunta.sh:234`; `bin/disjuntor.py:2157`; `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:293`. |
| **P5: chave ignorada pela CLI** | **Curado:** consulta `desligado(raiz)`, registra DESLIGADO e retorna zero. `bin/disjuntor.py:2147`; `bin/disjuntor.py:2148`; `bin/disjuntor.py:2150`. |
| **P6/achado 3: dois julgamentos, IDs e fail-closed** | **Parcial:** divisão por via implementada; IDs anteriores do wrapper aceitos; BACKLOG sem marcador e JSON truncado literal negados. A guarda ainda depende da representação textual do JSON e não cobre `SyntaxError` anterior à instalação. `bin/disjuntor.py:1731`; `bin/disjuntor.py:1752`; `bin/disjuntor.py:2144`; `bin/disjuntor.py:1586`; `bin/disjuntor.py:1657`; `bin/disjuntor.py:82`. |
| **Achado 6: quatro mutantes anteriores** | **Casos discriminantes acrescentados:** ruim antes do bom, último do glob, painel neutro e rc 2. Há novos sobreviventes no item 8. Verde integral: **[nao verificado]**. `bin/tests/test_censo_antes_da_vaga.sh:138`; `bin/tests/test_censo_antes_da_vaga.sh:147`; `bin/tests/test_censo_antes_da_vaga.sh:303`; `bin/tests/test_codex_pergunta.sh:230`. |
| **Achado 7: aprovação confundida com abertura** | **Parcial:** separa APROVADO e correlaciona obra+PID+hash12 em 600 s. Entretanto, `ini` ainda precede recusas que impedem chamar o Codex; “briefs aprovados” também conta eventos, não arquivos. `bin/disjuntor.py:2026`; `bin/disjuntor.py:2028`; `bin/codex_pergunta.sh:292`; `bin/codex_pergunta.sh:399`; `bin/disjuntor.py:2007`. |

2. **PROCEDENTE-candidato — a cura do topo mascara contratos anteriores de posição.**

`no_topo` acrescenta uma linha acima do painel vigente, preservando todas as anteriores. Contraprova em memória com as funções reais: `[ESPERANDO, ERRO-BOBO B3]` torna-se `[PAREI censo, ESPERANDO, ERRO-BOBO B3]`. O Stop passa a julgar o censo; a liberação B3 cai para a terceira linha útil e desaparece do seu recorte. `app/core/esteira_vigia.py:888`; `bin/hook_stop_fila1.py:411`; `bin/hook_stop_fila1.py:442`; `bin/disjuntor.py:759`; `bin/disjuntor.py:1003`.

A segunda linha **não é alternativa de ESPERANDO/PAREI para o Stop**: ela existe para B3. Assim, uma espera anteriormente comprovável pode deixar de liberar o Stop quando o novo censo não tiver wrapper vivo. `bin/hook_stop_fila1.py:425`; `bin/hook_stop_fila1.py:746`; `bin/hook_stop_fila1.py:749`; `bin/hook_stop_fila1.py:751`.

| Leitor | Posição e pressuposto |
|---|---|
| `_linha_do_topo` | Primeira útil representa o painel vigente. `bin/hook_stop_fila1.py:411`. |
| `_esperando_com_prova` | Primeira útil deve declarar ESPERANDO ou PAREI censo comprovado; não busca a segunda. `bin/hook_stop_fila1.py:746`; `bin/hook_stop_fila1.py:751`. |
| `_parei_do_em_curso` → `_relato_parou` | Primeira útil deve declarar PAREI com `!`, respeitar exclusões e referir o item pertinente. `bin/hook_stop_fila1.py:1198`; `bin/hook_stop_fila1.py:1213`; `bin/hook_stop_fila1.py:1218`; `bin/hook_stop_fila1.py:1249`. |
| Orçamento/CLEAR e recuperação | Consultam a primeira útil para reconhecer CLEAR. `bin/hook_stop_fila1.py:1002`; `bin/hook_stop_fila1.py:1119`. |
| B4/COTA | Recebe a primeira útil para decidir COTA. `bin/hook_stop_fila1.py:1138`. |
| B3 | Fotografa as duas primeiras úteis e procura nelas DESENHO/ERRO-BOBO. `bin/disjuntor.py:759`; `bin/disjuntor.py:1003`. |
| Confirmação da publicação censo | Compara somente a primeira útil com o PAREI recém-escrito. `bin/disjuntor.py:301`; `bin/disjuntor.py:302`. |
| Handoff | Prova censo pela primeira útil; seu inventário textual percorre até o primeiro `##` e exibe quatro perguntas. `bin/handoff_sessao.sh:102`; `bin/handoff_sessao.sh:110`; `bin/handoff_sessao.sh:128`. |
| Alarme | Procura PAREI nas primeiras 40 linhas físicas; exige prova somente para censo. Pode reconhecer uma parada deslocada que o Stop ignora. `bin/alarme_sessao_ociosa.py:124`; `bin/alarme_sessao_ociosa.py:137`; `bin/alarme_sessao_ociosa.py:140`. |

O selo exige que o painel neutro desça à segunda posição, mas não combina ESPERANDO/PAREI anterior com linha B3. Portanto, prova a inserção e aceita o deslocamento defeituoso. `bin/tests/test_censo_antes_da_vaga.sh:299`; `bin/tests/test_censo_antes_da_vaga.sh:307`; `bin/tests/test_censo_antes_da_vaga.sh:309`.

3. **PROCEDENTE-candidato — permanecem escritores do RELATO fora da trava compartilhada.**

O censo escreve por `no_topo`; alarmes do disjuntor e do vigia usam `no_relato`. Essas portas compartilham trava e gravação por temporário/replace. Stop e B3 consomem o painel; o Stop preserva explicitamente a retirada da antiga escrita automática de PAREI. `bin/disjuntor.py:299`; `bin/disjuntor.py:269`; `app/core/esteira_vigia.py:1603`; `app/core/esteira_vigia.py:843`; `app/core/esteira_vigia.py:891`; `app/core/esteira_vigia.py:926`; `bin/hook_stop_fila1.py:1379`.

Porém, `gerar_cortes` e `deploy_agendado` leem e regravam o RELATO inteiro diretamente. O intercalamento “leitura antiga → publicação censo → regravação antiga” perde o novo PAREI, mesmo quando o escritor externo modifica outra seção. É residual declarado, não curado pela trava nova. Ocorrência produtiva: **[nao verificado]**. `bin/gerar_cortes.py:117`; `bin/gerar_cortes.py:136`; `bin/deploy_agendado.sh:37`; `bin/deploy_agendado.sh:44`; `app/core/esteira_vigia.py:839`.

4. **PROCEDENTE-candidato — o coletor compartilhado ainda recebe universos diferentes e aplica confinamentos diferentes.**

Na CLI, `partes = list(anexos)` acrescenta os **nomes** dos anexos; somente `texto_de` é aberto. No despacho, o wrapper concatena o **conteúdo** dos anexos. Um envelope contendo referência a outro brief ruim pode chegar ao construtor sem essa referência passar pelo censo. O selo cobre anexo cujo próprio caminho é brief, não essa referência interna. `bin/disjuntor.py:2152`; `bin/disjuntor.py:2154`; `bin/disjuntor.py:2156`; `bin/codex_pergunta.sh:315`; `bin/codex_pergunta.sh:344`; `bin/tests/test_censo_antes_da_vaga.sh:138`.

Além disso, referências citadas são resolvidas e conferidas contra `RAIZ/logs`; o glob da obra é resolvido por `realpath` e unido sem a mesma conferência. Um link no brief ou na pasta da obra pode entregar arquivo externo ao juiz, embora a mesma referência externa citada diretamente seja recusada. Presença desses links em produção: **[nao verificado]**. `bin/disjuntor.py:1549`; `bin/disjuntor.py:1554`; `bin/disjuntor.py:1566`; `bin/disjuntor.py:1574`.

5. **OBSERVACAO — um julgamento por via existe no caminho normal; a rota externa continua aberta tecnicamente e proibida por conduta.**

Agent construtor reconhecido passa pelo PreToolUse; Codex construtor pelo wrapper. O hook deixou de julgar o censo da chamada Bash ao wrapper. Não aparece, nesses caminhos normais, a mesma abertura julgada pelos dois mecanismos; a recuperação excepcional pode chamar novamente `_censo_nega` para Agent. `bin/disjuntor.py:1748`; `bin/disjuntor.py:1752`; `bin/disjuntor.py:1731`; `bin/codex_pergunta.sh:234`; `bin/disjuntor.py:1662`.

Uma chamada direta a `codex exec`, sem wrapper, não alcança essas portas de censo: na sessão principal, um Bash sem `codex_pergunta` sai antecipadamente. Essa rota permanece proibida pela conduta, conforme o descarte P7; ocorrência produtiva: **[nao verificado]**. `bin/disjuntor.py:1716`; `CLAUDE.md:348`.

A sessão principal **não tem isenção** ao despachar Agent ou wrapper. `CODEX_PAPEL=leitura`, sem `--construtor`, fica fora do censo novo; leitura com essa flag recebe a recusa de incompatibilidade papel/flag. Uma negativa espúria nova para a chamada concreta desta sessão: **[nao verificado]**. `bin/disjuntor.py:1716`; `bin/codex_pergunta.sh:184`; `bin/codex_pergunta.sh:185`.

6. **PROCEDENTE-candidato — fail-closed ainda depende da grafia do JSON; identidade do Agent não cabe integralmente nas portas seguintes.**

A guarda de import e o fallback cru procuram literalmente `"Agent"` e `/wt-`. JSON equivalente com `"\u0041gent"` e `\/wt-` não satisfaz essas substrings. Em memória, a guarda emitiu `deny` para a representação literal e nenhum `deny` para a equivalente escapada. Que o consumidor real abra após traceback/rc sem negativa é **[nao verificado]**: a configuração apresentada apenas executa Python. `bin/disjuntor.py:72`; `bin/disjuntor.py:1657`; `.claude/settings.json:68`.

`SyntaxError` no próprio módulo precede a instalação da guarda. Já no wrapper, todo rc não zero do Python impede chegar à vaga. `bin/disjuntor.py:62`; `bin/disjuntor.py:82`; `bin/codex_pergunta.sh:234`; `bin/codex_pergunta.sh:240`.

IDs como `ABC` foram compatibilizados na CLI, mas continuam ilegíveis para o extrator do Agent; isso já fazia parte da definição anterior dessa via, portanto regressão nova aí é **[nao verificado]**. Há também incompatibilidade interna demonstrável: `_obra_de` admite `O9 (C)`, enquanto glob e publicação PAREI rejeitam espaço/parênteses. Uma negativa dessa identidade pode não publicar PAREI. Obra atual assim na fila: **[nao verificado]**. `bin/disjuntor.py:132`; `bin/disjuntor.py:594`; `bin/disjuntor.py:602`; `bin/disjuntor.py:1563`; `bin/disjuntor.py:285`; `bin/disjuntor.py:2144`.

7. **PROCEDENTE-candidato — a medida ainda mistura tentativa e abertura, pode separar suas fontes e não mede ocupação da vaga.**

O wrapper grava `ini` antes da raspagem, da trava da resposta e da revalidação. Essas etapas podem terminar sem chamar Codex; mesmo assim, `_censo_24h` conta o `ini` casado como abertura. A chamada efetiva só ocorre depois. Portanto, o antigo defeito semântico não desapareceu integralmente. `bin/codex_pergunta.sh:292`; `bin/codex_pergunta.sh:309`; `bin/codex_pergunta.sh:318`; `bin/codex_pergunta.sh:376`; `bin/codex_pergunta.sh:399`; `bin/codex_pergunta.sh:401`; `bin/disjuntor.py:2035`.

O censo grava na raiz viva; `ini` usa `LOGD`, por padrão sob a raiz do script, e aceita outros diretórios sob `R/logs`. O leitor procura exclusivamente `<raiz>/logs/codex/chamadas.log`. Wrapper de cópia ou `CODEX_LOGD` alternativo pode separar aprovação e `ini`, fazendo a abertura desaparecer ou parecer sem aprovação. Ocorrência produtiva: **[nao verificado]**. `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:193`; `bin/codex_pergunta.sh:234`; `bin/codex_pergunta.sh:293`; `bin/codex_limite_estado.py:22`; `bin/codex_limite_estado.py:52`.

O novo rótulo das horas descreve corretamente NEGOU→APROVADO, mas o cálculo não consulta ocupação da vaga: a medida literal de “vaga ociosa” continua sem prova. Também falta limitar eventos a `t <= agora`; um APROVADO Agent uma hora no futuro contou em memória como aprovado e abertura. `bin/disjuntor.py:2003`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2009`; `bin/disjuntor.py:2038`; `bin/disjuntor.py:2041`.

8. **PROCEDENTE-candidato — há novos mutantes simples não distinguidos pelos casos.**

| Alvo/mutante | Lacuna e contraprova |
|---|---|
| **`_briefs_do_censo`: `return todos, fora` → `return todos, []`** | O caso externo existente continua negado por ausência de qualquer brief. Falta **bom local + referência externa**: original nega; mutante libera. `bin/disjuntor.py:1575`; `bin/disjuntor.py:1614`; `bin/tests/test_censo_antes_da_vaga.sh:214`. |
| **`_censo_24h`: retirar `k not in usados and`** | Preserva todos os números da fixture: a reutilização nela ocorre fora dos 600 s. Com uma aprovação e dois `ini` da mesma identidade em 10 s, original dá com=1/sem=1; mutante com=2/sem=0. `bin/disjuntor.py:2027`; `bin/disjuntor.py:2033`; `bin/tests/test_censo_antes_da_vaga.sh:394`; `bin/tests/test_censo_antes_da_vaga.sh:397`. |
| **`no_topo`: retornar `False` quando `i is None`** | Casos usam painel neutro existente. Com RELATO contendo apenas cabeçalho, original publica PAREI; mutante não publica. Contraprova executada em memória. `app/core/esteira_vigia.py:885`; `bin/tests/test_censo_antes_da_vaga.sh:299`; `bin/tests/test_censo_antes_da_vaga.sh:322`. |
| **`_guarda_import`: retirar a condição da chave desligada** | Os casos de import não combinam import quebrado com chave desligada. O mutante passaria esses cenários, mas negaria quando deveria respeitar a chave. `bin/disjuntor.py:72`; `bin/tests/test_censo_antes_da_vaga.sh:345`; `bin/tests/test_censo_antes_da_vaga.sh:352`. |

As contraprovas acima foram verificadas em memória; sobrevivência à **execução integral** dos selos: **[nao verificado]**. Os mutantes anteriores de último citado/glob, neutro e rc 1 exclusivo agora têm casos específicos. `bin/tests/test_censo_antes_da_vaga.sh:457`; `bin/tests/test_censo_antes_da_vaga.sh:458`; `bin/tests/test_censo_antes_da_vaga.sh:459`; `bin/tests/test_codex_pergunta.sh:545`.

O positivo do PreToolUse ainda verifica somente stdout vazio, sem exigir rc zero. Assim, o selo continua podendo chamar de passagem limpa uma falha silenciosa. `bin/tests/test_censo_antes_da_vaga.sh:186`; `bin/tests/test_censo_antes_da_vaga.sh:201`.

9. **OBSERVACAO — escritores, leitores e unidades das trilhas.**

| Linha/fato | Quem escreve | Quem lê e significado |
|---|---|---|
| **CENSO APROVADO** | `_censo_nega`; Agent publica depois de `_pretool` permitir. `bin/disjuntor.py:1623`; `bin/disjuntor.py:1707`. | `_censo_24h` conta aprovações, fecha espera e correlaciona wrapper com `ini`; Agent é tratado como abertura. `bin/disjuntor.py:2004`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2009`; `bin/disjuntor.py:2035`. |
| **CENSO FORA** | `main_censo`. `bin/disjuntor.py:2162`. | Casamento isenta o `ini` de com/sem; não encerra espera anterior. `bin/disjuntor.py:2006`; `bin/disjuntor.py:2034`. |
| **NEGA: real `NEGOU DISJUNTOR CENSO`** | PreToolUse normal, recuperação e CLI. `bin/disjuntor.py:1801`; `bin/disjuntor.py:1812`; `bin/disjuntor.py:2167`. | Conta negativas; abre intervalo por obra somente quando reconhece `construtor de … sem CENSO`. `bin/disjuntor.py:1995`; `bin/disjuntor.py:1998`. |
| **CENSO DESLIGADO** | Agent e CLI. `bin/disjuntor.py:1779`; `bin/disjuntor.py:2148`. | Agent soma diretamente em sem; wrapper precisa casar com `ini`. `bin/disjuntor.py:2005`; `bin/disjuntor.py:2036`. |
| **PAREI censo** | `_parei_censo_no_relato` → `no_topo`. `bin/disjuntor.py:299`. | Stop, handoff e alarme compartilham reconhecedor, porém selecionam recortes diferentes. `bin/hook_stop_fila1.py:746`; `bin/handoff_sessao.sh:128`; `bin/alarme_sessao_ociosa.py:124`. |
| **`chamadas.log` / `ini`** | Wrapper, antes da chamada efetiva. `bin/codex_pergunta.sh:292`. | Parser cria chave **(obra,hash,PID)**; censo reordena corretamente para **(obra,PID,hash)**. Também alimenta contagem por papel, últimas chamadas e procedência. `bin/codex_limite_estado.py:63`; `bin/disjuntor.py:2026`; `bin/codex_limite_estado.py:71`; `bin/codex_limite_estado.py:82`; `bin/codex_limite_estado.py:92`. |

A correlação atual usa hash de 12 caracteres, PID, obra, consumo único e janela anterior de 600 segundos; as horas são união dos intervalos dividida por 3600. Não encontrei troca de PID por hash nessa indexação. `bin/disjuntor.py:2002`; `bin/disjuntor.py:2026`; `bin/disjuntor.py:2028`; `bin/disjuntor.py:2033`; `bin/disjuntor.py:2038`.

Persistem duas diferenças de significado: **“briefs aprovados” conta linhas APROVADO**, embora uma linha possa listar vários arquivos; **`ini` é tentativa iniciada no wrapper**, mas os leitores o apresentam como chamada/abertura acontecida. `bin/disjuntor.py:1621`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2054`; `bin/codex_limite_estado.py:72`; `bin/codex_pergunta.sh:399`.

`CENSO OK` legado deixou de ser reconhecido: um `ini` recente coberto apenas por OK antigo aparece sem aprovação. Existência dessas linhas na janela produtiva: **[nao verificado]**. `bin/disjuntor.py:1946`; `bin/disjuntor.py:2029`.

10. **OBSERVACAO — classes estruturais ainda presentes.**

| Classe | Situação |
|---|---|
| **Segundo juiz** | Não apareceu clone do juiz de forma; divisão normal por via está implementada. Há seleção paralela do que representa parada vigente. `bin/disjuntor.py:1617`; `bin/disjuntor.py:1731`; `bin/handoff_sessao.sh:102`; `bin/alarme_sessao_ociosa.py:124`. |
| **Segundo escritor fora da porta** | Presente e capaz de sobrescrever publicação concorrente. `bin/gerar_cortes.py:136`; `bin/deploy_agendado.sh:44`. |
| **Relógio fora da borda** | Janela não exclui futuro; escritor da trilha captura seu próprio `datetime.now()`. `bin/disjuntor.py:173`; `bin/disjuntor.py:2003`; `bin/disjuntor.py:2024`. |
| **Derivação paralela** | Stop: primeira; B3: duas; handoff: bloco; alarme: 40 físicas. `bin/hook_stop_fila1.py:411`; `bin/hook_stop_fila1.py:442`; `bin/handoff_sessao.sh:102`; `bin/alarme_sessao_ociosa.py:124`. |
| **Selo sem mordida** | Interação painel/B3 e mutantes do item 8 não são distinguidos. `bin/tests/test_censo_antes_da_vaga.sh:299`; `bin/tests/test_censo_antes_da_vaga.sh:394`. |
| **Ordem de trava** | Censo antes da vaga e idempotência sob trava estão corretos; escritores externos não participam. `bin/codex_pergunta.sh:234`; `bin/codex_pergunta.sh:240`; `app/core/esteira_vigia.py:891`; `bin/gerar_cortes.py:136`. |
| **Campo com dois significados** | Continua em `ini`/abertura e evento APROVADO/quantidade de briefs. `bin/codex_pergunta.sh:292`; `bin/disjuntor.py:1972`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2054`. |

## 09/11 04:05 · sessao 992b267357f7 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — aprovação + `ini` ainda não prova abertura.** O wrapper escreve `ini` antes de raspagem, espera por slot, trava da resposta e revalidação; essas etapas podem recusar sem chamar Codex. O leitor conta esse `ini` como abertura de fato. `bin/codex_pergunta.sh:292`; `bin/codex_pergunta.sh:309`; `bin/codex_pergunta.sh:318`; `bin/codex_pergunta.sh:376`; `bin/codex_pergunta.sh:399`; `bin/codex_pergunta.sh:401`; `bin/disjuntor.py:1972`; `bin/disjuntor.py:2023`. Ocorrência produtiva: **[nao verificado]**.

2. **PROCEDENTE-candidato — trilha e `ini` podem nascer em raízes diferentes e desaparecer da medida.** O censo recebe a raiz viva; `ini` usa `CODEX_LOGD`, cujo padrão é a raiz do script e cuja guarda permite outros diretórios sob `R/logs`. A medida lê exclusivamente `<raiz>/logs/codex/chamadas.log`. `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:193`; `bin/codex_pergunta.sh:234`; `bin/codex_pergunta.sh:293`; `bin/codex_limite_estado.py:22`; `bin/codex_limite_estado.py:52`; `bin/disjuntor.py:2018`. O selo verifica argv e `ini` separadamente da árvore da medida. `bin/tests/test_codex_pergunta.sh:222`; `bin/tests/test_censo_antes_da_vaga.sh:369`.

3. **PROCEDENTE-candidato — sobrevivente de consumo único.** Remover `k not in usados and` de `bin/disjuntor.py:2027` preservou, em memória, todos os números da fixture: sem=7, com=2, aprovados=4, negados=2, horas=1,9972 → texto `2.0`. A fixture reutiliza a identidade somente 55 minutos depois, quando a janela de 600 s já exclui a aprovação. `bin/tests/test_censo_antes_da_vaga.sh:383`; `bin/tests/test_censo_antes_da_vaga.sh:394`; `bin/tests/test_censo_antes_da_vaga.sh:397`. Na contraprova com uma aprovação e dois `ini` da mesma identidade em 10 s: original com=1/sem=1; mutante com=2/sem=0. Código: `bin/disjuntor.py:2027`; `bin/disjuntor.py:2033`. Execução integral dos selos: **[nao verificado]**.

4. **PROCEDENTE-candidato — sobrevivente do coletor.** Alterar `return todos, fora` para `return todos, []` preserva a negativa do único caso externo: ele não possui brief local e continua negando por ausência de briefs. `bin/disjuntor.py:1575`; `bin/disjuntor.py:1614`; `bin/tests/test_censo_antes_da_vaga.sh:214`. Em memória, bom local + referência externa: original nega; mutante libera. Não há essa combinação nos casos mostrados. `bin/tests/test_censo_antes_da_vaga.sh:208`; `bin/tests/test_censo_antes_da_vaga.sh:216`. Execução integral: **[nao verificado]**.

5. **PROCEDENTE-candidato — guarda de import depende da representação literal do JSON.** JSON válido com `tool_name:"\u0041gent"` e `/wt-` representado como `\/wt-` decodifica para o mesmo payload construtor, mas não satisfaz as substrings da guarda. Em memória, payload literal emitiu `deny`; equivalente escapado não emitiu. `bin/disjuntor.py:72`. O fallback de JSON ilegível repete a dependência literal. `bin/disjuntor.py:1662`. Que o harness abra após traceback/rc sem deny: **[nao verificado]**; a configuração apenas chama Python diretamente. `.claude/settings.json:68`.

6. **OBSERVACAO — outro mutante da guarda não está distinguido.** Remover a condição da chave desligada em `bin/disjuntor.py:72` mantém os casos de import apresentados, mas negaria Agent com import quebrado mesmo com a chave desligada. Os casos cobrem Agent quebrado, Bash quebrado e Agent conforme; não chave desligada + import quebrado. `bin/tests/test_censo_antes_da_vaga.sh:345`; `bin/tests/test_censo_antes_da_vaga.sh:352`. Sobrevivência integral: **[nao verificado]**.

7. **PROCEDENTE-candidato — janela não limita o futuro.** A medida usa apenas `t >= corte`, sem `t <= agora`, tanto nos eventos quanto nos `ini`. Um APROVADO Agent uma hora no futuro foi contado em memória como aprovado=1/com=1. `bin/disjuntor.py:1996`; `bin/disjuntor.py:2003`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2024`. Existência de carimbos futuros produtivos: **[nao verificado]**.

8. **OBSERVACAO — unidade de “briefs aprovados” é evento.** Uma linha pode listar vários briefs, mas a medida soma um por linha APROVADO; o rótulo diz briefs aprovados. `bin/disjuntor.py:1621`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2054`. Duas aprovações do mesmo arquivo somam duas; uma aprovação de dois arquivos soma uma. `bin/disjuntor.py:1621`; `bin/disjuntor.py:2007`.

9. **OBSERVACAO — compatibilidade da trilha antiga perdida.** `CENSO OK` não é reconhecido pela regex nova. Um `ini` recente coberto apenas por OK legado conta sem aprovação; negativas antigas não fecham por esse OK. `bin/disjuntor.py:1946`; `bin/disjuntor.py:2006`; `bin/disjuntor.py:2029`. Existência de linhas antigas na janela produtiva: **[nao verificado]**.

10. **OBSERVACAO — inventário de leitores de `ini`.** O escritor é o wrapper; o parser único constrói chave `(obra,hash,pid)`. `bin/codex_pergunta.sh:292`; `bin/codex_limite_estado.py:49`; `bin/codex_limite_estado.py:63`. Seus consumidores são contagem por papel, últimas chamadas, procedência por `fim.thread_id`, tokens por papel e censo; os últimos dois cruzam fatos diferentes. `bin/codex_limite_estado.py:71`; `bin/codex_limite_estado.py:82`; `bin/codex_limite_estado.py:92`; `bin/codex_limite_estado.py:185`; `bin/disjuntor.py:2018`. `gerar_codex_estado` usa a procedência para rotular origem=Code. `bin/gerar_codex_estado.py:153`.

11. **OBSERVACAO — trilhas atuais.** APROVADO: `_censo_nega`, com publicação adiada no Agent; FORA: `main_censo`; DESLIGADO: `main_censo` e `_censo_desligado`; negativa real chama-se `NEGOU DISJUNTOR CENSO`, sem evento literal CENSO NEGA. `bin/disjuntor.py:1623`; `bin/disjuntor.py:1707`; `bin/disjuntor.py:2162`; `bin/disjuntor.py:2148`; `bin/disjuntor.py:1779`; `bin/disjuntor.py:1801`; `bin/disjuntor.py:1812`; `bin/disjuntor.py:2167`. O escritor comum carimba `datetime.now()` e trunca o detalhe em 300 caracteres; a medida interpreta carimbo local, conta linhas, correlaciona obra+pid+hash12 em até 600 s e calcula união NEGOU→APROVADO dividida por 3600. `bin/disjuntor.py:168`; `bin/disjuntor.py:173`; `bin/disjuntor.py:1949`; `bin/disjuntor.py:1990`; `bin/disjuntor.py:2026`; `bin/disjuntor.py:2038`.

## 09/11 04:04 · sessao 66abcd07f52f · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — P3 ainda não reúne todo o texto entregue ao construtor.** `main_censo` recebe os nomes dos anexos, lê somente a pergunta e une ambos; não lê conteúdo dos anexos. O despacho efetivo concatena o conteúdo de todos eles. Um anexo geral contendo referência a `logs/outra/brief_ruim.md` chega ao construtor, mas essa referência não entra no coletor, salvo coincidência com o glob da obra. Contraprova em memória confirmou que o texto entregue ao juiz contém apenas `/raiz/logs/e3/envelope.txt\n<pergunta>`. `bin/disjuntor.py:2152`; `bin/disjuntor.py:2154`; `bin/disjuntor.py:2156`; `bin/codex_pergunta.sh:315`; `bin/codex_pergunta.sh:344`. O selo cobre anexos cujo próprio nome é brief, não referências internas: `bin/tests/test_censo_antes_da_vaga.sh:138`.

2. **PROCEDENTE-candidato — P4 corrige autoridade do julgamento, mas divide as trilhas entre raízes.** O wrapper de uma cópia passa a raiz do repositório vivo ao censo; `main_censo` grava ali. Entretanto, `LOGD` continua, por padrão, sob a raiz do script, e o `ini` é gravado nesse `LOGD`. Uma cópia/worktree pode deixar aprovação e abertura em árvores distintas; `_censo_24h(raiz)` procura ambos numa só árvore. `bin/codex_pergunta.sh:55`; `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:234`; `bin/codex_pergunta.sh:292`; `bin/disjuntor.py:2167`; `bin/disjuntor.py:1982`; `bin/disjuntor.py:2018`. A rota de cópia é expressamente suportada: `bin/codex_pergunta.sh:237`. Ocorrência produtiva: **[nao verificado]**.

3. **PROCEDENTE-candidato — fail-closed depende da representação literal do JSON.** `_censo_na_excecao` procura o trecho cru `"Agent"`; `_guarda_import` repete esse teste. JSON truncado com `"tool_name":"Ag\u0065nt"` e cópia `/wt-` não produz negativa nesse fallback. Contraprova em memória: retorno `None`; com `"Agent"` literal, retorna negativa. `bin/disjuntor.py:72`; `bin/disjuntor.py:1657`; `bin/disjuntor.py:1807`; `bin/disjuntor.py:1828`. Comportamento do consumidor real diante de payload truncado/erro Python: **[nao verificado]**. `SyntaxError` no próprio arquivo também precede a instalação da guarda: `bin/disjuntor.py:62`; `bin/disjuntor.py:82`.

4. **OBSERVACAO — um julgamento de censo por via está implementado no caminho normal.** Agent passa pelo hook; Codex construtor passa pelo wrapper. O hook deixou de julgar censo da chamada Bash ao wrapper. `bin/disjuntor.py:1731`; `bin/disjuntor.py:1748`; `bin/disjuntor.py:1752`; `bin/codex_pergunta.sh:184`; `bin/codex_pergunta.sh:234`. Não encontrei, nesses caminhos normais, a mesma abertura julgada pelos dois mecanismos de censo; o ramo excepcional pode reexecutar `_censo_nega` após falha do processamento do Agent. `bin/disjuntor.py:1661`.

5. **OBSERVACAO — compatibilidade de IDs continua diferente por via.** CLI agora admite `ABC` e `09-TURNO-ABERTO-EXPOSTA`; o extrator de Agent continua sem reconhecê-los, e Agent em cópia recebe negativa permanente “sem obra”, independentemente do brief. Contraprova em memória: `_obra_de` retorna `None` para ambos. `bin/disjuntor.py:132`; `bin/disjuntor.py:144`; `bin/disjuntor.py:594`; `bin/disjuntor.py:1751`; `bin/disjuntor.py:2144`; `bin/codex_pergunta.sh:188`. Isso já integrava a definição anterior do Agent; uma regressão nova desses IDs nessa via é **[nao verificado]**.

6. **PROCEDENTE-candidato — identidade com cauda continua sem suporte integral.** `_obra_de("obra O9 (C)…")` retorna `O9 (C)`, mas `_briefs_da_obra` e `_parei_censo_no_relato` rejeitam espaço/parênteses pelo `OBRA_CENSO_ID`. Logo o glob literal da obra desaparece e uma negativa dessa identidade não publica o PAREI censo; citar brief diretamente pode resolver o primeiro problema, não o segundo. `bin/disjuntor.py:137`; `bin/disjuntor.py:285`; `bin/disjuntor.py:588`; `bin/disjuntor.py:602`; `bin/disjuntor.py:1563`. Existência atual de obra assim na fila: **[nao verificado]**.

7. **OBSERVACAO — sessão principal e wrapper de leitura.** Ausência de `agent_id` não isenta a sessão principal ao despachar Agent ou comando contendo `codex_pergunta`. `bin/disjuntor.py:1716`. Wrapper com `CODEX_PAPEL=leitura`, sem `--construtor`, não passa pelo novo censo; com essa flag recebe a recusa anterior de incompatibilidade papel/flag. `bin/codex_pergunta.sh:184`; `bin/codex_pergunta.sh:185`; `bin/codex_pergunta.sh:407`. As chamadas concretas desta sessão e negativas produtivas não constam do dado: **[nao verificado]**.

8. **PROCEDENTE-candidato — confinamento dos briefs tem duas regras.** Referências citadas são resolvidas e conferidas contra `RAIZ/logs`; briefs descobertos por glob são resolvidos por `realpath` e unidos sem a mesma conferência. Um link em `logs/<obra>/brief*.md` ou na pasta da obra pode alimentar o juiz com arquivo externo; a referência externa citada diretamente seria recusada. `bin/disjuntor.py:1549`; `bin/disjuntor.py:1554`; `bin/disjuntor.py:1566`; `bin/disjuntor.py:1574`. A presença real desses links é **[nao verificado]**.

Nenhum arquivo foi criado/editado/apagado; verificações executadas somente em memória com `python3 -B`. Execução integral dos selos: **[nao verificado]**.

## 09/11 04:03 · sessao 399ce1c11505 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — PAREI censo mascara o painel anterior e pode esconder a liberação do B3.** `no_topo` insere acima da primeira linha útil, sem substituir/remover o painel anterior (`app/core/esteira_vigia.py:886`; `app/core/esteira_vigia.py:889`). Contraexemplo reproduzido exclusivamente em memória, usando as funções reais extraídas por AST:

   ```text
   antes:  [ESPERANDO: suite | pid=123, ERRO-BOBO OBRA-E3: defeito corrigido] [nao verificado]
   depois: [PAREI: censo-OBRA-E3 | espera Codex, ESPERANDO: suite | pid=123] [nao verificado]
   ```

   A linha B3 permanece no arquivo, na terceira posição útil, invisível para `_bloco_do_topo`, que para em duas (`bin/hook_stop_fila1.py:425`; `bin/hook_stop_fila1.py:442`). O Stop julga exclusivamente a primeira linha para ESPERANDO/censo e não procura o ESPERANDO deslocado à segunda (`bin/hook_stop_fila1.py:746`; `bin/hook_stop_fila1.py:751`). Sem wrapper vivo para o novo censo, a espera antiga comprovável fica mascarada (`bin/hook_stop_fila1.py:732`; `bin/hook_stop_fila1.py:749`). Efeito produtivo: **[nao verificado]**.

2. **OBSERVACAO — inventário dos leitores de primeira/segunda linha.**

   | Leitor | Recorte e pressuposto |
   |---|---|
   | `_linha_do_topo` | Devolve somente `b[0]`: primeira útil é o painel vigente. `bin/hook_stop_fila1.py:410`; `bin/hook_stop_fila1.py:411`. |
   | `_esperando_com_prova` | Primeira linha deve ser PAREI censo literal ou ESPERANDO com produtor/prova. Segunda linha não participa. `bin/hook_stop_fila1.py:746`; `bin/hook_stop_fila1.py:752`. |
   | `_parei_do_em_curso` | Primeira linha deve começar PAREI, conter `!`, passar exclusões CLEAR/COTA/SONDA e referir fila pertinente. `bin/hook_stop_fila1.py:1198`; `bin/hook_stop_fila1.py:1201`; `bin/hook_stop_fila1.py:1213`; `bin/hook_stop_fila1.py:1218`; `bin/hook_stop_fila1.py:1223`. |
   | `_orcamento` | Primeira linha decide o reconhecimento do PAREI CLEAR. `bin/hook_stop_fila1.py:1002`; `bin/hook_stop_fila1.py:1004`. |
   | `_orcamento_contido`, recuperação | Primeira linha deve ser CLEAR e handoff recente para recuperação excepcional. `bin/hook_stop_fila1.py:1119`. |
   | `_disjuntor_contido('b4')` | Entrega primeira linha para `b4_stop`; COTA deve ocupar essa posição. `bin/hook_stop_fila1.py:1138`; `bin/disjuntor.py:1290`. |
   | B3 `_topo_consistente` → `linhas_libera_b3` | Fotografa duas primeiras úteis e busca DESENHO/ERRO-BOBO nelas. `bin/disjuntor.py:759`; `bin/disjuntor.py:1003`. |
   | Handoff, prova censo | Consulta primeira linha pelo Stop; compartilha reconhecimento, não o inventário textual abaixo. `bin/handoff_sessao.sh:128`. |
   | Handoff, inventário textual | Varre bloco inteiro até primeiro `## `, coleta PAREI/lei/!, exibe quatro primeiros. Pode exibir parada que Stop não considera vigente. `bin/handoff_sessao.sh:102`; `bin/handoff_sessao.sh:109`; `bin/handoff_sessao.sh:111`. |
   | Alarme `_tem_parei` | Varre primeiras quarenta linhas físicas, aceita qualquer PAREI genérico e exige prova apenas do censo. Pode aceitar parada antiga deslocada abaixo do painel vigente. `bin/alarme_sessao_ociosa.py:124`; `bin/alarme_sessao_ociosa.py:134`; `bin/alarme_sessao_ociosa.py:137`; `bin/alarme_sessao_ociosa.py:140`. |
   | Confirmação do escritor censo | Relê `_bloco_do_topo(...)[0]` e compara a linha recém-publicada; isso prova localização, não compatibilidade com painel/B3 deslocados. `bin/disjuntor.py:301`; `bin/disjuntor.py:302`. |

3. **OBSERVACAO — escritores e travas.** Censo chama a porta nova (`bin/disjuntor.py:299`); alarmes do disjuntor usam `no_relato` (`bin/disjuntor.py:269`). Ambas delegam à mesma trava de diretório e gravação por temporário/replace (`app/core/esteira_vigia.py:842`; `app/core/esteira_vigia.py:891`; `app/core/esteira_vigia.py:853`; `app/core/esteira_vigia.py:921`; `app/core/esteira_vigia.py:926`). O vigia escreve alarmes por `no_relato`, em ALARMES, sem julgar painel linha 1/2 (`app/core/esteira_vigia.py:898`; `app/core/esteira_vigia.py:1603`). `vigia_arvore.sh` também delega à porta (`bin/vigia_arvore.sh:166`; `bin/vigia_arvore.sh:170`).

   O Stop atual não escreve PAREI: seu comentário preserva a remoção desse comportamento (`bin/hook_stop_fila1.py:1379`; `bin/hook_stop_fila1.py:1387`). Escrita manual efetiva do painel nesta produção: **[nao verificado]**.

4. **PROCEDENTE-candidato — segundo escritor fora da trava continua existente e pode restaurar fotografia antiga do topo.** `gerar_cortes.escrever` lê RELATO inteiro e depois regrava inteiro diretamente (`bin/gerar_cortes.py:117`; `bin/gerar_cortes.py:136`). `deploy_agendado.relato` faz o mesmo (`bin/deploy_agendado.sh:37`; `bin/deploy_agendado.sh:44`). Nenhum desses trechos toma a trava da porta; um intercalamento leitura antiga → censo publica PAREI → regravação antiga perde o PAREI novo, embora o escritor estivesse alterando outra seção (`app/core/esteira_vigia.py:850`; `app/core/esteira_vigia.py:853`). Ocorrência produtiva: **[nao verificado]**. Residual já declarado no próprio contrato de `no_relato`: `app/core/esteira_vigia.py:839`.

5. **PROCEDENTE-candidato — selo de topo prova inserção, mas não o contrato compartilhado das posições.** Os cenários de `casos_topo` começam com painel neutro, exigem censo em primeiro e painel neutro em segundo (`bin/tests/test_censo_antes_da_vaga.sh:299`; `bin/tests/test_censo_antes_da_vaga.sh:307`; `bin/tests/test_censo_antes_da_vaga.sh:309`). Não há ali ESPERANDO/PAREI útil anterior combinado com linha B3; o teste, portanto, aceita precisamente o deslocamento que esconde o B3 do leitor limitado a duas linhas (`bin/tests/test_censo_antes_da_vaga.sh:297`; `bin/tests/test_censo_antes_da_vaga.sh:328`; `bin/hook_stop_fila1.py:442`). Execução integral dos selos: **[nao verificado]**.

6. **OBSERVACAO — P1/P2 têm cura real local, mas não encerram o defeito de integração do topo.** A idempotência compara o painel vigente sob a trava; histórico inferior não impede republicação (`app/core/esteira_vigia.py:882`; `app/core/esteira_vigia.py:883`; `app/core/esteira_vigia.py:891`). A posição publicada é verificada pelo leitor do Stop (`bin/disjuntor.py:301`). Entretanto, recortes paralelos de Stop/B3, handoff e alarme continuam atribuindo vigência a posições diferentes (`bin/hook_stop_fila1.py:411`; `bin/hook_stop_fila1.py:442`; `bin/handoff_sessao.sh:102`; `bin/alarme_sessao_ociosa.py:124`). Classes presentes: **derivação paralela**, **segundo escritor fora da porta** e **selo sem mordida sobre a interação painel/B3**, nas referências dos achados 1, 2, 4 e 5.

## 09/11 03:50 · sessao 3bc64869901f · PAR-NAO-ADJACENTE · verificador · origem=Code

1. **Sim, `_n += 0` está morto pelos dois testes novos.** A asserção direta exige `('par', 1, 'S')` no índice 4 e `('par', 1, 'E')` no índice 6; o mutante devolve número zero. A asserção do alinhador também falha: exige `{0:0, 1:1, 2:3, 3:4, 4:6, 5:7}`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:128`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:140`; `app/escala/utils.py:471`.

2. **Sim, o pareador real produz os dois pares com essa alimentação.** O helper cria as batidas, monta o mapa por instante local truncado no minuto e chama `parear_turnos`, sem mock dessas funções. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:57`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:116`.

   O rastreio é:

   - **08h abre; 18h fecha.** A entrada está a 120 minutos do marco de intervalo mais próximo, fora da absorção de 90 minutos; a saída também está fora. Seguem pelos ramos normais de abertura e fechamento. `app/ponto/turnos.py:1356`; `app/ponto/turnos.py:1402`; `app/ponto/turnos.py:1491`.
   - **As seis batidas internas permanecem no turno.** `X` e `Xi` recebem `_intra_dur` e `_intra_ata`; somente `Xi` recebe `_intra_declarado`. O ramo `_iv` acrescenta essas batidas e continua, sem abrir ou fechar turno pelo tipo gravado. `app/ponto/turnos.py:1310`; `app/ponto/turnos.py:1316`; `app/ponto/turnos.py:1324`; `app/ponto/turnos.py:1371`.
   - **`_pares_marcados` encontra declaradas `[10h, 11h, 14h, 15h]`**, nessa ordem, e casa posições alternadas: `10→11` e `14→15`. Essa passagem não exige tipos gravados `S→E`. Marca as quatro pontas como usadas; depois, cada adjacência restante toca uma dessas pontas e é ignorada. `app/ponto/turnos.py:811`; `app/ponto/turnos.py:812`; `app/ponto/turnos.py:820`.
   - **Ambos entram em `intervalos_do_turno`:** duram 60 minutos, acima do piso de 15. Na transcrição, cada par atravessa somente uma `X`, sem ponta de outro par; recebe respectivamente números 0 e 1. `app/ponto/turnos.py:499`; `app/ponto/turnos.py:545`; `app/escala/utils.py:456`; `app/escala/utils.py:467`; `app/escala/utils.py:471`.

   **O oráculo confere com a regra implementada para o mapa fornecido:** o segundo par é declarado nos instantes 14–15h, embora os marcos de intervalo passados ao pareador sejam 10–11h. O carimbo `Xi` precede a inferência por proximidade do marco. O alinhador recebe separadamente as duas pausas cadastrais e verifica o encaixe completo. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:122`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:137`; `app/ponto/turnos.py:1316`.

   Esses testes **fornecem o mapa manualmente**; sua produção pelo leitor real de uma ata persistida, para esse arranjo: **[nao verificado]**. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:62`.

3. **Sobra um sobrevivente simples: `_n += 1` → `_n = 1`.** Todos os testes de `ParNaoAdjacente` continuam passando: nenhum exige um terceiro par aceito. Com dois pares, ambos os algoritmos emitem `0,1`; com três, o original emite `0,1,2`, enquanto o mutante emite `0,1,1`. Portanto, a lacuna diminuiu, mas permanece na numeração do terceiro par. `app/escala/utils.py:459`; `app/escala/utils.py:469`; `app/escala/utils.py:471`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:89`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:125`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:171`.

   No ensaio reduzido em memória dos **13 corpos das duas classes sem banco**, com funções reais extraídas e infraestrutura Django/timezone substituída: original **13/13**, `_n += 0` **11/13**, `_n = 1` **13/13**, tanto em ordem normal quanto inversa. As duas falhas foram exatamente nas asserções acima. Execução integral Django: **[nao verificado]**. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:50`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:128`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:140`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182`.

4. **Nos dois testes do delta v3:** data fixa; objetos novos por chamada; sem nome/CPF; sem leitura do relógio corrente ou estado compartilhado entre testes no caminho examinado. A ordem cronológica das batidas compõe a geometria do caso; não exige executar um teste antes do outro. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:43`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:59`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:125`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:132`; `app/ponto/turnos.py:1217`.

   **No diff total, permanece a ressalva da integração:** `ParNaoAdjacenteNaAta` herda uma fixture que cria nome e CPF; a criação da escala alcança `localdate()` por signal, e as batidas alcançam cartório com relógio corrente. Influência efetiva disso nas asserções: **[nao verificado]**. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:250`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:305`; `app/ponto/signals.py:63`; `app/ponto/signals.py:93`; `app/ponto/signals.py:135`; `app/ponto/services/cartorio.py:1965`; `app/ponto/services/cartorio.py:1989`.

## 09/11 03:49 · sessao 7ccf7d4cd575 · - · - · origem=Ronald (tmux)

Ensaio reduzido executado em memória, sem Django/banco e sem escrever arquivos: funções reais extraídas da cópia, timezone substituído por `ZoneInfo('America/Sao_Paulo')` e classes adaptadas a `unittest.TestCase`; execução integral Django: [nao verificado]. `app/ponto/turnos.py:1192`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:50`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182`.

| Variante | Ordem normal | Ordem inversa |
|---|---:|---:|
| Original | 13/13 | 13/13 |
| `_n += 0` | 11/13 | 11/13 |
| `_n = 1` | 13/13 | 13/13 |

Os 13 corpos são os oito testes de `ParNaoAdjacente` e cinco de `PapeisDoTurnoLimites`; as alterações foram aplicadas somente à string compilada em memória. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:89`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:171`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:199`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:230`; `app/escala/utils.py:471`.

`_n += 0` falha exatamente nas asserções das linhas **128** e **140**. A primeira encontra o segundo par com número `0`; a segunda recebe `{0:0, 1:2, 3:4, 4:6, 5:7}`, pois a solta `10:20` ocupa o marco `10:00` após a colisão dos números. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:128`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:140`; `app/escala/utils.py:530`; `app/escala/utils.py:565`.

O sobrevivente `_n = 1` permanece confirmado: nenhum corpo exige um terceiro par aceito; nesse terceiro par ele repetiria `1`, enquanto o original emitiria `2`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:116`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:129`; `app/escala/utils.py:469`; `app/escala/utils.py:471`.

## 09/11 03:48 · sessao 7ccf7d4cd575 · - · - · origem=Ronald (tmux)

Sobra o mutante `_n += 1` → `_n = 1`: a classe `ParNaoAdjacente` cobre no máximo dois pares aceitos, cujos números continuam sendo `0,1`; os testes novos exigem exatamente isso. `app/escala/utils.py:471`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:125`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:132`.

Ele não é equivalente: com três pares disjuntos não adjacentes, por exemplo índices `1→3`, `4→6`, `7→9`, emite números `0,1,1`, enquanto o original emite `0,1,2`. Confirmei o contraexemplo executando a função extraída por AST em memória, com consultas de pares substituídas; execução integral Django: [nao verificado]. `app/escala/utils.py:459`; `app/escala/utils.py:469`; `app/escala/utils.py:471`.

Também sobra o mutante `range(_i + 1, _iz)` → `range(_i + 1, _iz - 1)`: os casos aninhados da classe conservam outra ponta intermediária antes da última posição examinada, portanto continuam vetando o externo. `app/escala/utils.py:468`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:149`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:164`.

Contraexemplo desse segundo mutante: pares cruzados `1→4` e `3→5`; original veta ambos, mutante aceita ambos porque deixa de examinar as pontas imediatamente anteriores aos fechamentos. Confirmei com pares fornecidos em memória; produção desses pares pelo pareador real: [nao verificado]. `app/escala/utils.py:467`; `app/escala/utils.py:468`.

## 09/11 03:46 · sessao 9b78e3bf994b · PAR-NAO-ADJACENTE · construtor · origem=Code

Acrescentei os dois testes com pareador real e oráculos literais: [nao verificado]

- Papéis exatos dos pares `0` e `1`: [teste:125](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:125). [nao verificado]
- Cada ponta alinhada à sua pausa: [teste:132](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:132). [nao verificado]

No ensaio reduzido abaixo, ambos passaram no original e **ambos mataram `_n += 0`**, respectivamente nas asserções das linhas 128 e 140. `py_compile` passou sem gravar bytecode. Execução integral Django: **[nao verificado]**; suíte não executada. [nao verificado]

Comando executado para compilação, original e mutante, inteiramente em memória:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
import datetime
import py_compile
import sys
import types
import unittest
from pathlib import Path
from unittest.mock import patch
from zoneinfo import ZoneInfo

ROOT = Path.cwd()
TEST = ROOT / 'escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py'
with patch('py_compile.importlib._bootstrap_external._write_atomic') as write:
    py_compile.compile(str(TEST), cfile='verificacao-em-memoria.pyc', doraise=True)
    assert write.call_count == 1
print('py_compile: OK, escrita de bytecode interceptada')


def module(name):
    obj = types.ModuleType(name)
    sys.modules[name] = obj
    return obj


def extract(path, predicate, namespace):
    tree = ast.parse(path.read_text(), filename=str(path))
    tree.body = [node for node in tree.body if predicate(node)]
    exec(compile(tree, str(path), 'exec'), namespace)


# Somente infraestrutura substituida; pareador e leitores vem da copia.
tz = ZoneInfo('America/Sao_Paulo')
timezone = module('django.utils.timezone')
timezone.make_aware = lambda dt: dt.replace(tzinfo=tz)
timezone.localtime = lambda dt: dt.astimezone(tz)
timezone.localdate = lambda dt: dt.astimezone(tz).date()
module('django').utils = module('django.utils')
sys.modules['django.utils'].timezone = timezone
module('escala').utils = module('escala.utils')
utils = sys.modules['escala.utils']
utils.__dict__.update(datetime=datetime, timezone=timezone)
module('ponto').turnos = module('ponto.turnos')
turnos = sys.modules['ponto.turnos']
module('escala.servico_jornada')
extract(ROOT / 'escala/servico_jornada.py',
        lambda n: isinstance(n, ast.FunctionDef) and n.name == 'turno_cruza_meia_noite',
        sys.modules['escala.servico_jornada'].__dict__)
module('ponto.management')
module('ponto.management.commands')
eco = module('ponto.management.commands.detectar_par_relampago')
extract(ROOT / 'ponto/management/commands/detectar_par_relampago.py',
        lambda n: isinstance(n, ast.Assign) and any(isinstance(t, ast.Name) and
                  t.id == 'DURACAO_ECO_SEG' for t in n.targets), eco.__dict__)
extract(ROOT / 'ponto/turnos.py',
        lambda n: n.lineno < 1504 and not (isinstance(n, ast.ImportFrom) and
                                          n.module == 'core.dia_do_turno'), turnos.__dict__)
utils_path = ROOT / 'escala/utils.py'
utils_source = utils_path.read_text()
functions = {'_papeis_do_turno', '_alinhar_por_papel', 'encadear_marcos'}
extract(utils_path, lambda n: isinstance(n, ast.FunctionDef) and n.name in functions,
        utils.__dict__)
ns = dict(datetime=datetime, SimpleNamespace=types.SimpleNamespace,
          SimpleTestCase=unittest.TestCase, timezone=timezone)
extract(TEST, lambda n: (isinstance(n, ast.Assign) and any(isinstance(t, ast.Name) and
                         t.id == 'DIA' for t in n.targets)) or
        (isinstance(n, (ast.FunctionDef, ast.ClassDef)) and
         n.name in {'_aw', 'ParNaoAdjacente'}), ns)
tests = [
    'test_RED_dois_pares_nao_adjacentes_recebem_numeros_distintos',
    'test_RED_alinhador_senta_dois_pares_nao_adjacentes_nas_duas_pausas',
]
for name in tests:
    case = ns['ParNaoAdjacente'](name)
    getattr(case, name)()
    print('ORIGINAL PASS:', name)

assert utils_source.count('_n += 1') == 1
mutant = ast.parse(utils_source.replace('_n += 1', '_n += 0'), filename=str(utils_path))
mutant.body = [n for n in mutant.body if isinstance(n, ast.FunctionDef) and
               n.name == '_papeis_do_turno']
exec(compile(mutant, str(utils_path), 'exec'), utils.__dict__)
for name in tests:
    case = ns['ParNaoAdjacente'](name)
    try:
        getattr(case, name)()
    except AssertionError as error:
        print('MUTANTE MORTO:', name)
        print(str(error).splitlines()[0])
    else:
        raise AssertionError('MUTANTE SOBREVIVEU: ' + name)
PY
```

## 09/11 03:42 · sessao b891029b1740 · PAR-NAO-ADJACENTE · verificador · origem=Code

Os três pontos da v1 estão tratados na v2. Permanece uma lacuna de cobertura: o mutante que deixa todos os pares com número zero sobrevive aos testes novos sem banco. `app/escala/utils.py:475`; `app/escala/utils.py:467`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:203`; `app/escala/utils.py:471`.

A cópia disponibilizada não contém repositório Git acessível; identidade com o HASH e execução do HEAD histórico `b57ce0a1`: **[nao verificado]**. A comparação abaixo considera o algoritmo anterior mostrado no diff fornecido e o código presente. `app/escala/utils.py:437`.

1. **Fechamento dos achados da v1**

| Achado | Situação na v2 |
|---|---|
| Ordenação por `k` alterava listas sem par novo | **Corrigido.** As triplas carregam a posição em `_bats`; o retorno ordena por essa posição e depois remove esse índice auxiliar. Os testes incluem `locs` desordenado e timestamp repetido. `app/escala/utils.py:458`; `app/escala/utils.py:475`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:172`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:186`. |
| Mutante `_iz <= N−1` sobrevivia | **Cobertura acrescentada.** A condição continua estrita; o teste exige apenas `ent`, `sol`, `sai` quando o par termina na última batida. `app/escala/utils.py:467`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:194`. |
| “Aninhado não muda” ignorava interno não adjacente | **Contrato corrigido e caso acrescentado.** Externo vetado; interno não adjacente atravessando solta vira par. Esse caso usa pares mockados: sua produção pelo pareador real continua **[nao verificado]**. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:156`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:203`. |

2. **Preservação quando nenhum par é não adjacente**

**Sim, por comparação dos algoritmos fornecidos; não encontrei contraexemplo nessa condição.** Para um par adjacente, `_iz == _i + 1`, o intervalo de índices examinado pelo veto é vazio. A aceitação continua igual à anterior; o fechamento emitido antecipadamente continua imediatamente seguinte à abertura, portanto ordenar pela posição em `_bats` conserva a lista antiga. As guardas de borda permanecem estritas. `app/escala/utils.py:461`; `app/escala/utils.py:467`; `app/escala/utils.py:468`; `app/escala/utils.py:475`.

Isso também conserva as peculiaridades anteriores: ordenação estável de `batidas`, primeiro `k` de cada timestamp em `locs`, última posição daquele timestamp em `_pos`, possíveis `k` repetidos e ausência de deduplicação do retorno. `app/escala/utils.py:449`; `app/escala/utils.py:450`; `app/escala/utils.py:457`; `app/escala/utils.py:475`.

Os três testes novos fixam as listas antigas para ausência de par, par adjacente e timestamps repetidos com `locs` desordenado. Não constituem, isoladamente, prova exaustiva; a conclusão geral vem do raciocínio acima. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:172`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:180`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:186`.

3. **Mordida dos testes** [nao verificado]

Executei os **11 corpos novos sem banco em memória**, extraindo funções da cópia e substituindo a infraestrutura Django/timezone: todos passaram, e as mutações abaixo foram reprovadas pelos testes indicados. Isso é um ensaio reduzido; execução integral Django e integração com banco: **[nao verificado]**. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:50`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:155`.

| Teste novo | Mutação que o reprova | [nao verificado]
|---|---|
| Duas soltas, linha 89 | Repor exigência `_iz == _i + 1`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:95`; `app/escala/utils.py:467`. |
| Anexo 660, linha 99 | Repor adjacência. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:103`; `app/escala/utils.py:467`. |
| Uma solta, linha 107 | Repor adjacência. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:113`; `app/escala/utils.py:467`. |
| Aninhado com interno adjacente, linha 116 | Remover veto de pontas intermediárias: o externo passa indevidamente. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:128`; `app/escala/utils.py:468`. |
| Aninhado abaixo do piso, linha 135 | Retirar `pausas_abaixo_do_piso` da composição de `_pares`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:141`; `app/escala/utils.py:454`. |
| Alinhador independente da ordem, linha 144 | Trocar busca pelo rótulo `ent` por `papeis[0][0]`. A lista invertida revela o erro. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:151`; `app/escala/utils.py:525`. |
| Sem par, desordenado, linha 172 | Ordenar triplas por `k`, usando `_p[1]`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:177`; `app/escala/utils.py:475`. |
| Adjacente, desordenado, linha 180 | Mesma ordenação por `k`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:183`; `app/escala/utils.py:475`. |
| Timestamp repetido, linha 186 | Substituir `setdefault` por atribuição, conservando o último `k`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:191`; `app/escala/utils.py:449`. |
| Fechamento na última, linha 194 | Trocar `< N−1` por `<= N−1`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:199`; `app/escala/utils.py:467`. |
| Interno não adjacente, linha 203 | Repor adjacência. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:210`; `app/escala/utils.py:467`. |
| Builder, linha 299 | Repor adjacência viola a asserção direta dos papéis, por análise estática; execução desse teste: **[nao verificado]**. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:307`; `app/escala/utils.py:467`. |

**Mutante sobrevivente:** `_n += 1` → `_n += 0` passou nos 11 corpos reduzidos. Nenhum caso novo exige dois pares aceitos com números distintos; o teste de integração também só espera `par 0`. Com dois pares, esse mutante faz o alinhador sobrescrever as pontas no mesmo agrupamento. Sobrevivência na suíte integral: **[nao verificado]**. `app/escala/utils.py:471`; `app/escala/utils.py:530`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:128`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:210`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:307`.

O teste do builder verifica seu dicionário, **não uma ata relavrada e relida da persistência**. A ata preparada manualmente contém lâmpadas e órfãs, sem `minutos_realizados`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:291`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:305`.

4. **Escritores, leitores e significados**

| Retorno/campo | Escrita e leitura identificadas |
|---|---|
| Lista `(k, papel)` | `_papeis_do_turno` produz; `_match_marcos` recebe; `_alinhar_por_papel` interpreta rótulos e agrupa por número/letra. Não exige tuplas S/E consecutivas. `app/escala/utils.py:475`; `app/escala/utils.py:597`; `app/escala/utils.py:525`; `app/escala/utils.py:530`. |
| Órfãs e consumo | `_match_marcos` emite órfãs na ordem da lista e marca `consumed[k]`. Logo, a ordem do retorno é observável. `app/escala/utils.py:622`; `app/escala/utils.py:624`. |
| Células, papel e tipo real | O casamento passa separadamente tipo do marco e tipo gravado; `_celula_do_marco` mantém `tipo`/`papel` separados de `tipo_real`. O propositor compara explicitamente esses significados. `app/escala/utils.py:620`; `app/escala/utils.py:326`; `app/ponto/propositor.py:66`. |
| Ata | Cartório transporta células para lâmpadas e copia órfãs/minutos; chama `lavrar_veredito(save=False)`; a porta atribui a ata, e `carimbar` persiste os campos pela mesma porta. `app/ponto/services/cartorio.py:679`; `app/ponto/services/cartorio.py:706`; `app/ponto/services/cartorio.py:711`; `app/ponto/services/cartorio.py:1802`; `app/ponto/services/cartorio.py:1822`; `app/ponto/portas/celula.py:1028`; `app/ponto/portas/celula.py:1067`; `app/ponto/portas/celula.py:900`. |
| `minutos_realizados` | Builder consulta `realizado_do_dia`; leitores incluem grade, validação PDF, comparação ata×juiz e diagnóstico do espelho. `app/escala/utils.py:2046`; `app/escala/utils.py:2059`; `app/escala/services/leitor_celula.py:406`; `app/folha/validacao_pdf.py:48`; `app/ponto/services/bordas_realizado.py:87`; `app/ponto/services/espelho.py:891`. |

Há uma distinção que deve permanecer: papel `par` inclui pausas abaixo do piso; o adaptador envia ao motor somente `intervalos_do_turno`. Esses conjuntos não são intercambiáveis, mas isso não demonstra leitor novo com interpretação errada. `app/escala/utils.py:454`; `app/ponto/adaptador_motor.py:44`; `app/ponto/turnos.py:545`; `app/ponto/turnos.py:565`.

5. **Classes de reprovação e relógio**

- **Segundo juiz / derivação paralela:** a alteração consulta os pares existentes e decide sua representabilidade, sem redescobri-los por tipo ou duração. Continua omitindo o externo aninhado. O produtor por alternância já existe e só entra sem `turno`. `app/escala/utils.py:454`; `app/escala/utils.py:468`; `app/escala/utils.py:478`; `app/escala/utils.py:598`.
- **Segundo escritor:** nenhum novo escritor produtivo aparece no trecho; há escrita direta de `ata` na preparação do teste. `app/escala/utils.py:475`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:291`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:297`.
- **Selo sem mordida:** cada teste tem mutação discriminada acima, mas falta cobertura de numeração com dois pares aceitos e de releitura após persistência. `app/escala/utils.py:471`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:210`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:305`.
- **Campo com dois significados:** os leitores examinados distinguem papel de tipo gravado e par geométrico de intervalo descontável. Ausência universal de outros conflitos: **[nao verificado]**. `app/escala/utils.py:326`; `app/ponto/propositor.py:66`; `app/ponto/adaptador_motor.py:44`.

**Não há `timezone.now()` nem `timezone.localdate()` chamado no corpo de teste novo.** A data é fixa e o builder recebe `DEPOIS_DE_TUDO`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:43`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:89`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:172`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:299`; `app/escala/tests/instante.py:12`.

**A fixture de integração ainda alcança relógio corrente indiretamente:** criação da escala → signal → `localdate()`; criação das batidas → signal → cartório sem `agora` → `localdate()`/`now()`. Influência efetiva nos asserts: **[nao verificado]**. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:233`; `app/ponto/signals.py:72`; `app/ponto/signals.py:63`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:286`; `app/ponto/signals.py:135`; `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1965`; `app/ponto/services/cartorio.py:1989`.

## 09/11 03:39 · sessao f3ebd92d8e31 · - · - · origem=Ronald (tmux)

Os 12 testes novos têm uma mutação identificável que viola suas asserções; isso é análise estática, sem execução Django dos mutantes. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:89`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:299`.

| Teste, identificado pela linha | Mutação que o reprova | [nao verificado]
|---|---|
| Duas soltas, :89 | Repor adjacência `_iz == _i + 1` na condição. `app/escala/utils.py:467`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:95` |
| Anexo 660, :99 | Repor adjacência. `app/escala/utils.py:467`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:103` |
| Uma solta, :107 | Repor adjacência. `app/escala/utils.py:467`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:113` |
| Aninhado interno adjacente, :116 | Remover veto de pontas intermediárias: externo passa a ser emitido. `app/escala/utils.py:468`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:128` |
| Aninhado abaixo do piso, :135 | Retirar `pausas_abaixo_do_piso` de `_pares`: interno desaparece e externo fica elegível. `app/escala/utils.py:454`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:141` |
| Alinhador independente da ordem, :144 | Trocar busca de entrada por `_ent = papeis[0][0]`: lista invertida revela dependência posicional. `app/escala/utils.py:525`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:151` |
| Sem par e locs desordenado, :172 | Ordenar triplas por `k`, `_p[1]`, em vez de posição `_p[0]`. `app/escala/utils.py:475`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:177` |
| Adjacente e locs desordenado, :180 | Mesma ordenação por `k`. `app/escala/utils.py:475`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:183` |
| Timestamp repetido, :186 | Trocar `setdefault` por atribuição, conservando último `k`. `app/escala/utils.py:449`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:191` |
| Fechamento na última, :194 | Trocar `< len(_bats)-1` por `<= len(_bats)-1`. `app/escala/utils.py:467`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:199` |
| Aninhado interno não adjacente, :203 | Repor adjacência. `app/escala/utils.py:467`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:210` |
| Builder, :299 | Repor adjacência: asserção direta dos papéis já reprova. `app/escala/utils.py:467`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:307` |

**Mutante simples sobrevivente aos testes novos, por inspeção:** `_n += 1` → `_n += 0`. Nenhum novo caso espera mais de um par aceito; todos os pares esperados têm número zero. Execução integral deste mutante: `[nao verificado]`. `app/escala/utils.py:471`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:95`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:103`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:113`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:128`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:141`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:150`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:183`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:210`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:307`.

**Relógio:** nenhum corpo de teste novo chama `timezone.now()` ou `timezone.localdate()`. Os helpers usam `DIA` fixo, e o builder recebe `DEPOIS_DE_TUDO`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:43`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:57`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:161`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249`; `app/escala/tests/instante.py:12`.

A fixture de integração alcança relógio indiretamente: criação da escala → signal → `localdate()`; criação das batidas → signal → cartório sem `agora` → `localdate()`/`now()`. Influência concreta nas asserções: `[nao verificado]`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:233`; `app/ponto/signals.py:72`; `app/ponto/signals.py:63`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:286`; `app/ponto/signals.py:131`; `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1965`; `app/ponto/services/cartorio.py:1989`.

O teste de integração verifica o dicionário do builder, não relê `minutos_realizados` persistido pela porta; sua ata é preparada com escrita direta na fixture. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:291`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:297`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:305`.

## 09/11 03:39 · sessao 58456d9ce782 · - · - · origem=Ronald (tmux)

- `_papeis_do_turno` escreve apenas a lista transitória `(k,papel)`; `_match_marcos` é o único chamador produtivo direto encontrado. O alinhador agrupa pares por número e letra; não exige tuplas consecutivas. `_match_marcos` usa timestamps para extremos, preserva a ordem recebida nas órfãs e marca `consumed[k]`. `app/escala/utils.py:475`; `app/escala/utils.py:597`; `app/escala/utils.py:525`; `app/escala/utils.py:530`; `app/escala/utils.py:607`; `app/escala/utils.py:622`; `app/escala/utils.py:624`.

- Existe produtor alternativo preexistente, `_papeis_por_alternancia`, para períodos alternados; só é chamado quando `turno is None`. Não é uma nova derivação introduzida pela obra. `app/escala/utils.py:478`; `app/escala/utils.py:490`; `app/escala/utils.py:497`; `app/escala/utils.py:598`.

- A alteração lê as duas consultas do juiz e filtra representabilidade; não refaz a decisão por duração ou tipo. As autoridades registradas são `_match_marcos` para ocupação e `intervalos_do_turno` para intervalo. O filtro continua podendo omitir pares, portanto “transcrição de todos os pares” seria excessivo. `app/escala/utils.py:454`; `app/escala/utils.py:467`; `app/core/juizes.py:200`; `app/core/juizes.py:202`.

- Campos afetados indiretamente: `_match_marcos` produz células e órfãs; células conservam `tipo_real` vindo de `locs`, distinguindo-o do papel `S/E` do par. `lampadas_do_dia` transporta o tipo real. Não encontrei leitor produtivo que interprete o papel do par como o tipo gravado; essa universalidade fora dos pontos pesquisados é `[nao verificado]`. `app/escala/utils.py:620`; `app/escala/utils.py:622`; `app/escala/utils.py:327`; `app/ponto/services/cartorio.py:679`.

- Minutos seguem outra cadeia: builder consulta `realizado_do_dia`, `ata_do_dia` transporta, cartório entrega à porta, porta atribui e persiste. Não há novo escritor produtivo nesse trecho. A integração nova escreve a ata diretamente na preparação da fixture. `app/escala/utils.py:2046`; `app/escala/utils.py:2059`; `app/ponto/services/cartorio.py:706`; `app/ponto/services/cartorio.py:1802`; `app/ponto/portas/celula.py:1028`; `app/ponto/portas/celula.py:1035`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:291`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:297`.

- Leitores do minuto: grade da célula, conferência PDF, residual ata×juiz e diagnóstico `ata_sem_turno`. `app/escala/services/leitor_celula.py:406`; `app/folha/validacao_pdf.py:48`; `app/ponto/services/bordas_realizado.py:87`; `app/ponto/services/espelho.py:891`.

- Distinção explícita de significado: `par` inclui pausas abaixo do piso; intervalos enviados ao motor e descontados do realizado vêm somente de `intervalos_do_turno`, com piso de 900 segundos. Isso não demonstra conflito novo entre leitores. `app/escala/utils.py:454`; `app/ponto/turnos.py:499`; `app/ponto/turnos.py:545`; `app/ponto/turnos.py:565`; `app/ponto/adaptador_motor.py:44`; `app/ponto/turnos.py:756`.

- O teste de integração consulta o builder e o retorno transitório; não relê uma nova ata persistida pela porta com `minutos_realizados=660`. A ata semeada manualmente contém lâmpadas e órfãs. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:291`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:304`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:305`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:309`.

## 09/11 03:37 · sessao 3bbc841fcd90 · PAR-NAO-ADJACENTE · construtor · origem=Code

Aplicado em [escala/utils.py:458](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:458): cada item carrega sua posição em `_bats`; o retorno ordena por essa posição e remove-a: [nao verificado]

```diff
-    out = [(_ks[0], 'ent')]
+    out = [(0, _ks[0], 'ent')]
…
-            out += [(_ks[_i], ('par', _n, 'S')), (_ks[_iz], ('par', _n, 'E'))]
+            out += [(_i, _ks[_i], ('par', _n, 'S')), (_iz, _ks[_iz], ('par', _n, 'E'))]
…
-            out.append((_ks[_i], 'sol'))
-    out.append((_ks[-1], 'sai'))
-    return sorted(out, key=lambda _p: _p[0])
+            out.append((_i, _ks[_i], 'sol'))
+    out.append((len(_bats) - 1, _ks[-1], 'sai'))
+    return [(_k, _papel) for _i, _k, _papel in sorted(out, key=lambda _p: _p[0])]
```

A linha em branco do docstring voltou em [utils.py:439](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:439). O teste aninhado recebeu nome e docstring qualificados: externo vetado igual ao HEAD; interno segue a regra do par não adjacente. [teste:116](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:116) [nao verificado]

`_match_marcos` lê o retorno, passa-o ao alinhador e emite órfãs na ordem recebida. O alinhador identifica bordas pelos rótulos e pares pelo número/S/E. [utils.py:597](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:597), [utils.py:622](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:622), [utils.py:525](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:525)

Sem par não adjacente, as emissões já seguem as posições crescentes de `_bats`; ordenar por essas posições preserva essa lista, inclusive a ordem estável dos timestamps iguais e os `k` repetidos. [utils.py:450](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:450), [utils.py:461](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:461)

Os cinco testes novos têm oráculos manuais e mock declarado das consultas do juiz: [nao verificado]

| Caso | O que reprova |
|---|---|
| Sem par, `locs` desordenado | Ordenação por `k`. [teste:172](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:172) | [nao verificado]
| Par adjacente, `locs` desordenado | Regressão da lista do HEAD. [teste:180](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:180) | [nao verificado]
| Timestamp repetido | Reordenação ou perda das ocorrências repetidas. [teste:186](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:186) | [nao verificado]
| Fechamento na última batida | Mutante `<` → `<=`: emite `par/E` e `sai` no mesmo índice. [teste:194](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:194) | [nao verificado]
| Aninhado com interno não adjacente | Interno reduzido a `sol/sol`, externo aceito ou fechamento fora da ordem. [teste:203](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:203) | [nao verificado]

Comandos reproduzíveis pelo [verificador em memória:45](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/verificar_par_nao_adjacente_memoria.py:45):

```bash
export PYTHONDONTWRITEBYTECODE=1
verificador_par=escala/tests/verificar_par_nao_adjacente_memoria.py
python3 "$verificador_par" --modo atual
python3 "$verificador_par" --modo volta1
python3 "$verificador_par" --modo pre-cura
python3 "$verificador_par" --modo mutante-por-k
python3 "$verificador_par" --modo mutante-sem-ordenar
python3 "$verificador_par" --modo mutante-ultima
python3 "$verificador_par" --compilar
```

Resultados reduzidos: atual **5/5**; volta 1 reprova os três casos de ordem; pré-cura preserva esses três e reprova o interno não adjacente. Os três mutantes morreram nas falhas esperadas. [verificador:93](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/verificar_par_nao_adjacente_memoria.py:93)

`py_compile` passou nos três arquivos tocados, interceptando apenas a gravação de bytecode. [verificador:102](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/verificar_par_nao_adjacente_memoria.py:102) Suite não executada; execução integral Django e certificação: **[nao verificado]**. [nao verificado]

## 09/11 03:32 · sessao 7ca8d208fcb4 · PAR-NAO-ADJACENTE · verificador · origem=Code

A cópia contém a implementação proposta e os testes novos. A comparação abaixo usa o comportamento anterior descrito no diff; identidade do checkout com o HASH e execução do HEAD histórico: **[nao verificado]**. Referências da versão presente: `app/escala/utils.py:437`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:49`.

**1) Quais arranjos mudam**

A condição nova usa **índices e igualdade de timestamps completos**. Para uma abertura no índice `i`, escolhe o fechamento pelo último índice daquele timestamp; exige `0 < i < iz < N−1` e nenhuma batida intermediária cujo timestamp pertença ao conjunto de pontas de **qualquer** par, incluindo pausas abaixo do piso. Não verifica o tipo gravado das pontas. `app/escala/utils.py:453`; `app/escala/utils.py:456`; `app/escala/utils.py:464`; `app/escala/utils.py:466`.

| Arranjo | Comparação com o comportamento anterior |
|---|---|
| Par interior adjacente, isoladamente | Continua aceito: não há índice intermediário para vetá-lo. `app/escala/utils.py:466` |
| Par interior não adjacente, atravessando somente batidas sem timestamp de ponta | Passa de pontas `sol` para `par`; as intermediárias continuam `sol`. `app/escala/utils.py:467`; `app/escala/utils.py:468`; `app/escala/utils.py:472` |
| Aninhado com par interno adjacente | O externo continua recusado; o interno continua aceito. Os testes fixam exatamente isso. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:115`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:130` |
| Aninhado cujo par interno também é não adjacente | **O arranjo pode mudar:** o externo permanece vetado, mas o interno passa se atravessar somente soltas. Portanto “aninhado não muda” não vale sem qualificar o interior. `app/escala/utils.py:466`; `app/escala/utils.py:467` |
| Cruzado, com quatro pontas distintas: `a<c<z<w` | Ambos continuam recusados: cada vão contém uma ponta do outro. A ocorrência desse formato na saída real do pareador não está demonstrada pelos anexos: **[nao verificado]**. `app/escala/utils.py:455`; `app/escala/utils.py:467` |
| Par começando no primeiro índice ou terminando no último | Continua sem transcrição como par: a primeira batida não entra no laço; o fechamento deve ser estritamente interior. `app/escala/utils.py:457`; `app/escala/utils.py:460`; `app/escala/utils.py:466`; `app/escala/utils.py:473` |
| Timestamp de uma batida do turno ausente de `locs`, ou índice já consumido | Continua retornando `None`, antes de examinar os pares. A presença é por timestamp, não por identidade do objeto ou tipo. `app/escala/utils.py:446`; `app/escala/utils.py:450`; `app/escala/utils.py:451` |
| Ponta declarada pelo juiz ausente de `turno.batidas` | Sem posição para o fechamento, não transcreve esse par. Isso não é uma guarda nova. `app/escala/utils.py:456`; `app/escala/utils.py:465`; `app/escala/utils.py:466` |
| `locs` fora de ordem cronológica | **Pode mudar a lista mesmo sem nenhum par novo:** agora o retorno ordena por `k`, não pela cronologia de `_bats`. `app/escala/utils.py:449`; `app/escala/utils.py:474` |

Há ressalvas específicas para repetições:

- Mesmo `HH:MM`, mas segundos diferentes, permanece distinguível aqui; a chave não é truncada ao minuto. `app/escala/utils.py:448`; `app/escala/utils.py:450`; `app/escala/utils.py:456`.
- Timestamp completo repetido colapsa identidades: `_idx` conserva o **primeiro** `k`; `_pos` conserva a **última** posição; `_ini_par` conserva o último fechamento para uma abertura repetida. Essa combinação já decorre dos mapas usados pela função. `app/escala/utils.py:448`; `app/escala/utils.py:454`; `app/escala/utils.py:456`.
- Uma duplicata intermediária de uma ponta também veta o vão: o filtro compara timestamps, sem distinguir “esta ponta” de “outro objeto no mesmo instante”. Duplicatas de soltas podem atravessar e produzir `k` repetido no retorno. `app/escala/utils.py:450`; `app/escala/utils.py:455`; `app/escala/utils.py:467`; `app/escala/utils.py:472`.
- Duas batidas adjacentes com timestamp idêntico podem receber `S` e `E` do mesmo par e o mesmo `k`: a condição exige ordem dos **índices**, não duração positiva. O juiz admite pares com `z.timestamp >= a.timestamp`. `app/escala/utils.py:466`; `app/escala/utils.py:468`; `app/ponto/turnos.py:813`.
- `_fins` também é um conjunto de timestamps: aceitar um fechamento faz o laço pular qualquer abertura posterior com aquele timestamp. Portanto pontas compartilhadas não devem ser tratadas como pares independentes. `app/escala/utils.py:459`; `app/escala/utils.py:462`; `app/escala/utils.py:469`.

Assim, além da ampliação pretendida, há mudança de ordenação e possível renumeração dos pares posteriores, pois `_n` cresce para cada par aceito. Uma afirmação de preservação de **toda a lista** exige considerar esses efeitos. `app/escala/utils.py:468`; `app/escala/utils.py:470`; `app/escala/utils.py:474`.

**2) Ordem do retorno e leitores**

O chamador produtivo direto identificado é `_match_marcos`. Ele usa a lista para localizar extremos por timestamp, alinhar, emitir órfãs e marcar consumo; não lê pares como duas tuplas obrigatoriamente consecutivas. `app/escala/utils.py:596`; `app/escala/utils.py:605`; `app/escala/utils.py:613`; `app/escala/utils.py:621`; `app/escala/utils.py:622`.

O alinhador encontra `ent`/`sai` pelos rótulos, agrupa pontas pelo número do par e pelas letras `S`/`E`, ordena os números e ordena soltas pelo timestamp. Portanto esse leitor não depende da antiga proximidade das tuplas S/E. **A ordem das órfãs**, porém, segue a lista recebida. `app/escala/utils.py:524`; `app/escala/utils.py:526`; `app/escala/utils.py:530`; `app/escala/utils.py:564`; `app/escala/utils.py:621`.

`k` pode repetir nas colisões descritas acima; `k=None` impede o retorno da lista pela guarda anterior. O `sorted` não deduplica nem corrige colisões. `app/escala/utils.py:448`; `app/escala/utils.py:451`; `app/escala/utils.py:474`.

**3) Quem escreve, quem lê e qual significado**

| Conteúdo | Produção e leitura |
|---|---|
| Papéis transitórios `par`/`sol` | `_papeis_do_turno` produz; `_alinhar_por_papel` e `_match_marcos` leem. Existe outro produtor do mesmo formato, `_papeis_por_alternancia`, acionado sem `turno`. `app/escala/utils.py:468`; `app/escala/utils.py:472`; `app/escala/utils.py:477`; `app/escala/utils.py:596`; `app/escala/utils.py:613` |
| `minutos_realizados` destinado à ata | O builder consulta `realizado_do_dia`; `ata_do_dia` transporta o inteiro; o cartório entrega a ata a `lavrar_veredito`, cuja porta atribui e persiste. `app/escala/utils.py:2045`; `app/escala/utils.py:2058`; `app/ponto/services/cartorio.py:706`; `app/ponto/services/cartorio.py:1802`; `app/ponto/portas/celula.py:1028`; `app/ponto/portas/celula.py:1035` |
| Leitores do minuto lavrado | `grade_da_celula`, conferência PDF, comparação ata×juiz e diagnóstico `ata_sem_turno` no espelho. `app/escala/services/leitor_celula.py:406`; `app/folha/validacao_pdf.py:48`; `app/ponto/services/bordas_realizado.py:87`; `app/ponto/services/espelho.py:891` |
| Intervalos enviados ao motor | `periodos_de_turnos` copia `intervalos_do_turno`; o caminho da ata também transporta pausas declaradas, recuperando timestamps das batidas. `_intra_real` soma os intervalos recebidos. `app/ponto/adaptador_motor.py:44`; `app/ponto/motor_calculo_v2.py:713`; `app/ponto/motor_calculo_v2.py:721`; `app/ponto/motor_calculo_v2.py:1909` |

**Não há um significado único intercambiável entre esses conteúdos:**

- **Par geométrico versus descanso descontável:** o papel `par` inclui pausas abaixo do piso; o adaptador transporta somente intervalos de pelo menos 900 segundos. Confundir esses conjuntos alteraria a conta. `app/escala/utils.py:453`; `app/ponto/turnos.py:499`; `app/ponto/turnos.py:545`; `app/ponto/turnos.py:565`; `app/ponto/adaptador_motor.py:44`.
- **Dia:** o realizado usa `data_turno`; o motor mantém mapa para esse dia, mas há trechos com fallback explícito para a data civil da entrada. Não cabe substituir indiscriminadamente um pelo outro. `app/ponto/turnos.py:719`; `app/ponto/motor_calculo_v2.py:884`; `app/ponto/motor_calculo_v2.py:894`; `app/ponto/motor_calculo_v2.py:685`.
- **Precisão:** o realizado trunca cada ponta ao minuto antes de subtrair; o motor soma segundos/60. A ata endereça por minuto, e o motor recupera o primeiro objeto daquele minuto. O alimentador de papéis da ata, diferentemente, recusa luzes com quantidade de batidas diferente de um. `app/ponto/turnos.py:605`; `app/ponto/turnos.py:740`; `app/ponto/turnos.py:758`; `app/ponto/motor_calculo_v2.py:600`; `app/ponto/motor_calculo_v2.py:609`; `app/ponto/motor_calculo_v2.py:1911`; `app/ponto/turnos.py:1656`.
- **Pontas:** o juiz aceita igualdade temporal; a transcrição exige índices interiores; o realizado admite fechamento da pausa igual ao fim; o motor conserva interseções de duração positiva. As contas subtraem instantes, sem acrescentar uma unidade pelas pontas. `app/ponto/turnos.py:813`; `app/escala/utils.py:466`; `app/ponto/turnos.py:759`; `app/ponto/motor_calculo_v2.py:1886`; `app/ponto/motor_calculo_v2.py:1887`; `app/ponto/motor_calculo_v2.py:1911`.

Os **660** da fixture correspondem a `21:01→22:00`, após truncamento; `20:00→22:00` corresponde aos **599** do outro teste. Isso não demonstra igualdade com o resultado financeiro do motor. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:96`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:104`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:204`.

**4) Classes de reprovação**

- **Segundo juiz / derivação paralela:** o trecho alterado lê os pares das funções existentes e calcula sua representabilidade; não redescobre pares por tipo ou duração. As autoridades registradas continuam sendo `_match_marcos` para ocupação de marcos e `intervalos_do_turno` para intervalo. Entretanto, ele continua omitindo alguns pares do juiz, como o externo aninhado; não é transcrição irrestrita de todos os pares. `app/core/juizes.py:200`; `app/core/juizes.py:202`; `app/escala/utils.py:453`; `app/escala/utils.py:467`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:123`.
- **Segundo escritor:** a função produtiva só devolve uma lista. O teste novo de integração, contudo, atribui e salva `CelulaDia.ata` diretamente, fora da porta registrada — preparação de fixture, não novo escritor produtivo. `app/escala/utils.py:474`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:226`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:232`; `app/core/portas.py:112`.
- **Campo com dois significados:** nenhum campo persistido novo aparece no trecho alterado. O cuidado necessário é distinguir `par` geométrico, intervalo descontável e minuto de relógio, conforme os contratos acima. `app/escala/utils.py:453`; `app/escala/utils.py:474`; `app/ponto/adaptador_motor.py:44`; `app/ponto/turnos.py:605`.

Quanto ao RED, executei **os seis corpos puros em memória**, com funções extraídas do código e substituições de Django/timezone. Isso verifica esse ambiente reduzido; execução integral Django e HEAD histórico: **[nao verificado]**. Os corpos e as funções exercitadas estão em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:49`; `app/escala/utils.py:437`; `app/escala/utils.py:502`.

| Teste novo | Condição anterior reconstruída | Proposta, execução reduzida | [nao verificado]
|---|---|---|
| Duas soltas | Falha na asserção dos papéis | Passa. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:94` |
| Anexo 21:01→22 | Falha nos papéis | Passa. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:102` |
| Uma solta | Falha nos papéis | Passa. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:112` |
| Aninhado | Passa | Passa. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:115` |
| Aninhado abaixo do piso | Passa | Passa. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:130` |
| Ordem do alinhador | Passa | Passa. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:139` |
| Integração NaAta | **[nao verificado]** | **[nao verificado]**. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:234` |

Logo, **não é verdade que cada teste novo reprova a condição anterior**. Os três últimos testes puros preservam comportamentos. Além disso, o teste NaAta consulta o builder; não relê `minutos_realizados` persistido pela porta. A ata inserida manualmente nem contém essa chave. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:184`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:226`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:240`.

**Mutante sobrevivente na execução reduzida:** trocar `_iz < len(_bats)-1` por `_iz <= len(_bats)-1` passou nos seis testes. Permitiria emitir fechamento de par na última batida e também `sai`; os casos novos não exercitam esse limite. Sobrevivência na integração integral: **[nao verificado]**. `app/escala/utils.py:466`; `app/escala/utils.py:468`; `app/escala/utils.py:473`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:84`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:108`.

**5) Relógio, nome/CPF e ordem**

`ParNaoAdjacente` usa data fixa e objetos novos `SimpleNamespace`, sem nome/CPF e sem consulta explícita ao relógio nos seus corpos. Não há dependência explícita entre os seis testes. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:42`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:58`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:88`.

`ParNaoAdjacenteNaAta` **herda nome, CPF e username literais** da fixture; origem real desses identificadores: **[nao verificado]**. Seus argumentos diretos fixam `hoje` e `agora`. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:158`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:159`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:184`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:223`; `app/escala/tests/instante.py:12`.

Essa fixture, entretanto, **alcança relógio solto indiretamente**: criar a escala aciona recomputação com `timezone.localdate()`; criar batidas aciona o cartório sem `agora`, que consulta data/hora corrente. Influência efetiva nos asserts e independência integral da ordem: **[nao verificado]**. `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:168`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:221`; `app/ponto/signals.py:63`; `app/ponto/signals.py:72`; `app/ponto/signals.py:93`; `app/ponto/signals.py:131`; `app/ponto/services/cartorio.py:1965`; `app/ponto/services/cartorio.py:1989`.

**6) Outros sítios e classificação da pergunta**

| Sítio | Mesma pergunta ou outra? |
|---|---|
| `_papeis_por_alternancia` e Zona 5 | Mesma finalidade de alinhamento, **outro contrato de entrada**: pontas de períodos alternadas, sem `turno`, e não todas as batidas internas. O defeito idêntico por esse caminho está **[nao verificado]**. `app/escala/utils.py:489`; `app/escala/utils.py:491`; `app/escala/utils.py:496`; `app/escala/utils.py:597`; `app/ponto/motor_calculo_v2.py:3060`; `app/ponto/motor_calculo_v2.py:3062` |
| `marco_ocupado_pela_batida` | Também usa o alinhamento sem turno; um único período não satisfaz a guarda de quatro pontas da alternância. Outro contrato, sem demonstração do mesmo defeito. `app/escala/utils.py:427`; `app/escala/utils.py:489` |
| `_alinhar_por_papel` | Exige consecutividade dos **marcos cadastrais** S/E, não das batidas reais. Outra condição. `app/escala/utils.py:517`; `app/escala/utils.py:520`; `app/escala/utils.py:561` |
| `_pares_marcados` | Há adjacência no ramo residual, mas ele **produz a decisão do juiz**. Pontas declaradas são pareadas antes. Alterá-lo muda a pergunta de origem, potencialmente os minutos. `app/ponto/turnos.py:811`; `app/ponto/turnos.py:818`; `app/ponto/turnos.py:880` |
| `_borda_posicional` e `_pausa_fechada_carimbada` | Procuram figuras entre vizinhas para decidir borda/pausa carimbada; não transcrevem a lista de pares recebida. Outra pergunta. `app/ponto/turnos.py:679`; `app/ponto/turnos.py:697`; `app/ponto/turnos.py:912`; `app/ponto/turnos.py:924` |
| Adaptador, `_intra_real`, `segmentos_do_motor` | Transportam ou consomem intervalos sem exigir adjacência entre batidas cruas. `app/ponto/adaptador_motor.py:44`; `app/ponto/motor_calculo_v2.py:1910`; `app/ponto/calculador/alimentacao.py:133` |
| Caminho da ata para o motor | Lê pares pelas chaves declaradas da ata/DNA e recupera seus instantes. Não exige vizinhança entre batidas cruas. `app/ponto/juiz_batida.py:280`; `app/ponto/motor_calculo_v2.py:714` |
| Vão entre períodos no motor | Compara períodos consecutivos e consulta o par exato no juiz. Outra representação; não é a transcrição das batidas internas para papéis. `app/ponto/motor_calculo_v2.py:2978`; `app/ponto/motor_calculo_v2.py:2983`; `app/ponto/motor_calculo_v2.py:2987` |
| `primeiro_furo_posicional` | Deduz candidato por duração entre marcas adjacentes para responder sobre furo posicional. Outra pergunta, sem receber o par pronto do juiz. `app/ponto/propositor.py:282`; `app/ponto/propositor.py:296` |

## 09/11 03:28 · sessao 494b8856f826 · - · - · origem=Ronald (tmux)

Arquivo abreviado: `T = app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py`.

| Teste novo | Condição/retorno anteriores reconstruídos do diff | Cópia atual | [nao verificado]
|---|---|---|
| `test_RED_duas_soltas_entre_20_e_22` | Falha no papel, T:94 | Passa, T:88 |
| `test_RED_anexo_2101_22_preserva_660_minutos` | Falha no papel, T:102 | Passa, T:98 |
| `test_RED_uma_solta_entre_12_e_13` | Falha no papel, T:112 | Passa, T:106 |
| `test_aninhado_mantem_exatamente_os_papeis_do_HEAD` | Passa | Passa, T:115 |
| `test_aninhado_abaixo_do_piso_tambem_veta_o_par_externo` | Passa | Passa, T:130 |
| `test_alinhador_le_k_e_numero_do_par_sem_depender_da_ordem_das_tuplas` | Passa | Passa, T:139 |
| `test_RED_builder_preserva_o_par_e_os_660_minutos_do_anexo` | Execução integral [nao verificado]; a primeira asserção diretamente afetada é o papel, T:242 | Execução integral [nao verificado], T:234 |

**Limite da execução:** os seis testes puros foram executados em memória por AST, com funções reais de `turnos.py`, `_papeis_do_turno`, `_alinhar_por_papel`, `encadear_marcos` e os próprios testes; `SimpleTestCase` substituído por `unittest.TestCase`, timezone substituído por implementação `zoneinfo America/Sao_Paulo`. Isso verifica os corpos dos seis testes nesse ambiente reduzido; execução Django/HEAD histórico integral [nao verificado]. O código atual está em `app/escala/utils.py:437`; reconstrução anterior corresponde ao diff fornecido, com comparação de adjacência e retorno `out` em `diff_v1.diff`, hunk `@@ -464,14 +463,15 @@`.

**Selo com mordida limitada:** três testes novos passam também com condição anterior: os dois aninhados e o alinhador; portanto “cada teste novo falha no HEAD” é falso para os corpos verificados. Os números 599/660 são calculados pelo juiz sem depender da função alterada: T:89, T:96, T:99, T:104; a aritmética usa `intervalos_do_turno` em `app/ponto/turnos.py:756`. No teste de integração, premissa do par e número aparecem antes do papel, respectivamente T:238 e T:240; resultado integral no HEAD [nao verificado].

**Mutante sobrevivente:** trocar apenas `_iz < len(_bats) - 1` por `_iz <= len(_bats) - 1`, em `app/escala/utils.py:466`, passou nos seis corpos executados. Ele permite classificar a última batida como fim de par e ainda emitir `'sai'`, porque a emissão final continua em `app/escala/utils.py:473`. Os arranjos novos usam fins interiores, T:84, T:108, T:118; logo não exercitam essa guarda. Sobrevivência na integração integral [nao verificado], embora seu par também termine antes da borda, T:217.

**Relógio, identidade e ordem:**

- `ParNaoAdjacente` usa `SimpleNamespace` sem nome/CPF, data fixa e instantes explícitos: T:42, T:58, T:84. Cada chamada constrói novas batidas e novo mapa, T:58; não há dependência explícita entre testes nessa classe, T:88. [nao verificado]
- `ParNaoAdjacenteNaAta` herda fixture com username, CPF e nome literais, T:154–160. Origem real desses identificadores [nao verificado].
- As entradas diretas do julgamento estão fixadas: `hoje` em T:223, builder `agora=DEPOIS_DE_TUDO` em T:184; esse instante é fixo em `app/escala/tests/instante.py:12`.
- Ainda há relógio indireto: criar `EscalaColaborador` em T:168 aciona `app/ponto/signals.py:72`, cujo caminho usa `timezone.localdate()` em `app/ponto/signals.py:63`. Criar batidas em T:221 aciona `app/ponto/signals.py:131`, chama julgamento em `app/ponto/signals.py:93`, e `julgar_celula` consulta relógio em `app/ponto/services/cartorio.py:1965`, `:1986`, `:1989`. Os signals são importados em `app/ponto/apps.py:7`; não há freeze/desconexão nos novos testes, T:49 e T:212. Influência efetiva disso nos asserts e independência integral da ordem [nao verificado].
- Há escrita direta de `cel.ata` na fixture, fora da porta: T:226 e T:232. É uma escrita introduzida no teste; o diff de produção continua sendo somente a função de transcrição, `app/escala/utils.py:437`.

_3024 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
