# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 14:41** · publicado em **13/11/2026 14:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 14:41 · sessao bfb9aff3c2f8 · HJ-092 · sonda · origem=Code

**A saída classifica 133 como `t1_certo`, 15 como `t2_certo` e deixa 469 indeterminadas. “Certo” aqui é o rótulo da sonda, condicionado ao teste de alternância.** `[nao verificado]` [nao verificado]

Os números abaixo derivam exclusivamente do dado colado, por isso levam `[nao verificado]`. Não localizei `sonda_classe.py` na cópia; seu corpo não consta do texto fornecido, portanto não pude conferir sua implementação nem o hash informado. `[nao verificado]`

**1. Contagem por competência**

Nas colunas de ocupação: P = preenchido, V = vazio, ? = `None`. `[nao verificado]`

| Competência | Total | t1 certo | t2 certo | Sem plantio vivo | Nenhum alterna | Ambos alternam | Outro marco P / V / ? | Fonte |
|---|---:|---:|---:|---:|---:|---:|---|---|
| 2026-08 | 55 | 3 | 1 | 48 | 3 | 0 | 31 / 24 / 0 | `[nao verificado]` |
| 2026-09 | 198 | 44 | 8 | 96 | 50 | 0 | 100 / 77 / 21 | `[nao verificado]` |
| 2026-10 | 221 | 74 | 4 | 98 | 43 | 2 | 81 / 92 / 48 | `[nao verificado]` |
| 2026-11 | 143 | 12 | 2 | 124 | 4 | 1 | 13 / 53 / 77 | `[nao verificado]` |
| **Total** | **617** | **133** | **15** | **366** | **100** | **3** | **225 / 246 / 146** | `[nao verificado]` |

O total “não dá para dizer” é **51 / 146 / 143 / 129**, respectivamente, somando **469**. As classes fecham exatamente as 617 entradas, e o cabeçalho declara `ERROS 0`. `[nao verificado]`

Sobre **627 versus 617**, a saída permite confirmar somente o universo declarado de 617 nesta execução e a diferença aritmética de 10. Ela não identifica os dez `pergunta_id` adicionais nem demonstra quais logs estavam ausentes; portanto não permite reconciliar os dois universos ou classificar os dez restantes. `[nao verificado]`

**2. Limitações do critério e alcance**

Na cópia, `t1` vem do casamento por motivo em `marcos_faltantes`, passando `tipo=None`; `t2` usa a resposta, escolhe o marco mais próximo por distância circular e aceita distância até 240 minutos. Na divergência, retorna `t1`. `app/chamados/juizes.py:549`, `app/chamados/juizes.py:553`, `app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:280`, `app/chamados/services/materializacao.py:296`, `app/chamados/services/materializacao.py:310`, `app/chamados/services/declaracao_texto.py:25`.

| Fraqueza | Onde pode errar | Quantos a saída permite delimitar |
|---|---|---|
| **Janela fixa** | Pode cortar vizinhos relevantes ou incluir outro turno. O materializador pode substituir a hora respondida pela hora do marco de intervalo e ajustar o instante por overnight. `app/chamados/services/materializacao.py:412`, `app/chamados/services/materializacao.py:426`. | **251** têm plantio vivo e entram potencialmente na comparação: **148** decididos e **103** ambíguos. Quantos sofreram corte ou contaminação da janela: `[nao verificado]`. |
| **Alternância como prova** | Alternar não demonstra identidade do marco, horário correto ou jornada correta. O pareador considera intervalo e papel informado pela ATA, que pode prevalecer sobre o tipo gravado. `app/ponto/turnos.py:1195`, `app/ponto/turnos.py:1201`. | Os **148** rótulos conclusivos dependem dessa heurística; **100** não alternam em nenhuma hipótese e **3** alternam nas duas. Quantos dos 148 estão efetivamente errados: `[nao verificado]`. |
| **Grade lida hoje** | O leitor consulta a célula/ATA disponível, sem parâmetro de instante histórico. Isso não reconstrói necessariamente o marco que originou `t2` no ato. `app/escala/services/leitor_celula.py:350`, `app/escala/services/leitor_celula.py:362`, `app/escala/services/leitor_celula.py:379`. | Até **617** reconstruções de marco/ocupação ficam expostas; **471** receberam booleano de ocupação e **146** receberam `None`. Quantas grades mudaram: `[nao verificado]`. |
| **Resposta do log × atual** | A derivação lê `p.resposta_colab`, e o log registra esse valor naquela chamada. O modelo admite respostas posteriores que superam anteriores. `app/chamados/services/materializacao.py:280`, `app/chamados/services/materializacao.py:308`, `app/chamados/models.py:3501`. | Até **617** comparações históricas ficam expostas se a sonda recomputa pela resposta atual. A saída não informa quantas respostas mudaram. Há **29** plantados diferentes de `t1`, mas isso não identifica a causa. `[nao verificado]` |
| **Exigir plantio vivo vinculado** | Ausência de vínculo não prova ausência histórica de plantio: `pergunta_origem` aceita nulo e seu contrato inclui plantios anteriores a 06/09. `app/ponto/models.py:40`. | Limita os **366 `sem_plantio_vivo`**; quantos representam ausência real, vínculo antigo ausente ou retratação: `[nao verificado]`. |
| **Ocupação histórica aproximada** | Sem o script, não se conferem os filtros temporais e de validade usados no teste dos 30 minutos. O modelo distingue instante da marca, criação e retratação. `app/ponto/models.py:23`, `app/ponto/models.py:67`, `app/ponto/models.py:79`. | **225 preenchidos, 246 vazios, 146 desconhecidos** são resultados do indicador. Quantos reproduzem incorretamente a ocupação no ato: `[nao verificado]`. |

Os alcances acima se sobrepõem; não são quantidades de erros demonstrados nem devem ser somados. `[nao verificado]`

**3. Cruzamento com ocupação**

| Classe da sonda | Total | Outro preenchido | Outro vazio | Desconhecido | Fonte |
|---|---:|---:|---:|---:|---|
| `t1_certo` | 133 | **92** | 41 | 0 | `[nao verificado]` |
| `t2_certo` | 15 | 4 | **11** | 0 | `[nao verificado]` |

Entre `t2_certo`, **11/15 = 73,3%** tinham o outro marco vazio. Entre `t1_certo`, **92/133 = 69,2%** tinham o outro preenchido. Isso sustenta uma associação na amostra, condicionada ao critério da sonda. `[nao verificado]`

A candidata **“a grade só vale contra marco ainda vazio” não é sustentada como condição universal**: existem quatro `t2_certo` com outro marco preenchido — **pk26279, pk34263, pk39767 e pk44524**. Além disso, há **41 `t1_certo` com outro vazio**, portanto vazio sozinho não determina que `t2` esteja certo. `[nao verificado]`

O código atual também não exige marco vazio para selecionar `t2`: a lista de marcos contém tipo/hora de todas as lâmpadas, e a seleção filtra hora/tipo, proximidade e tolerância. `app/escala/services/leitor_celula.py:148`, `app/chamados/services/materializacao.py:293`, `app/chamados/services/materializacao.py:299`.

**4. col824 — pk52752 / pk52753 / pk53024**

| Pergunta | Entrada histórica fornecida | Saída fornecida | Fonte |
|---|---|---|---|
| **pk52752** | `CASO 52752 None`; sem valores disponíveis de `t1`/`t2`. | Não aparece nas listas de classificação. Isso não prova inexistência da pergunta nem ausência em outros logs. | `[nao verificado]` |
| **pk52753** | `intervalo_volta`, resposta **06:00**, **t1=E / t2=S**. | Plantado vivo **E**, outro marco **vazio**, `nao_da_para_dizer(nenhum_alterna)`. O plantado coincide com `t1`, mas a sonda não confirma nenhum dos dois. | `[nao verificado]` |
| **pk53024** | `orfao_14h`, resposta **01:01**, **t1=S / t2=E**. | Sem plantio vivo vinculado, ocupação **None**, `nao_da_para_dizer(sem_plantio_vivo)`. Não permite decidir entre `t1` e `t2`. | `[nao verificado]` |

## 13/11 14:40 · sessao f4074b0f4b47 · - · - · origem=Ronald (tmux)

Cálculo exclusivamente das tuplas fornecidas; sem acesso à definição do último campo. `[nao verificado]`

| Competência | t1 certo: preenchido | t1 certo: vazio | t1: desconhecido | t2 certo: preenchido | t2 certo: vazio | t2: desconhecido |
|---|---:|---:|---:|---:|---:|---:|
| 2026-08 | 2 | 1 | 0 | 0 | 1 | 0 |
| 2026-09 | 31 | 13 | 0 | 1 | 7 | 0 |
| 2026-10 | 51 | 23 | 0 | 3 | 1 | 0 |
| 2026-11 | 8 | 4 | 0 | 0 | 2 | 0 |
| Total | 92 | 41 | 0 | 4 | 11 | 0 |

Toda a tabela deriva da saída fornecida. `[nao verificado]`

Os quatro `t2_certo` com outro marco preenchido são pk26279, pk34263, pk39767 e pk44524. `[nao verificado]`

Há 29 plantados diferentes de t1: 16 em 2026-09 e 13 em 2026-10. A divisão por classe é: `[nao verificado]`

- `t1_certo`: 3 — pk31938, pk31942, pk34270. `[nao verificado]`
- `t2_certo`: 4 — pk22626, pk25140, pk27739, pk44524. `[nao verificado]`
- `nenhum_alterna`: 22 — pk20883, pk23194, pk24950, pk25441, pk26108, pk26298, pk26885, pk29236, pk29930, pk33586, pk33606, pk34460, pk35300, pk37621, pk39998, pk40362, pk40757, pk41275, pk41356, pk41656, pk41978, pk43672. `[nao verificado]`
- `ambos_alternam`: 0. `[nao verificado]`

O último campo é maior que 1 em 26 tuplas: 8 `sem_plantio_vivo`, 7 `nenhum_alterna`, 11 `t1_certo`, nenhuma nas outras classes; sua soma em todas as 617 tuplas é 648. Não é possível atribuir esse campo a repetições de log sem o script. `[nao verificado]`

## 13/11 14:40 · sessao dfa9ae20f57a · - · - · origem=Ronald (tmux)

Não localizei `sonda_classe.py` na cópia; o hash e a implementação exata da sonda não puderam ser conferidos. A análise do critério depende da descrição e da saída fornecidas. `[nao verificado]`

| Fraqueza | Evidência e alcance |
|---|---|
| Janela fixa pode cortar turno noturno ou incluir outro turno | O materializador pode ajustar o instante por overnight; intervalo também pode usar hora do marco em vez da resposta. `app/chamados/services/materializacao.py:413`, `app/chamados/services/materializacao.py:426`. Potencialmente **251** perguntas com plantio vivo entram na comparação de sequências: **148** decididas e **103** ambíguas; quantidade efetivamente prejudicada pela janela não consta. `[nao verificado]` |
| Alternância não prova qual marco ocorreu | O pareador distingue intervalo e papel informado pela ATA; o papel pode prevalecer sobre `Batida.tipo`. `app/ponto/turnos.py:1195`, `app/ponto/turnos.py:1201`. As **148** classificações conclusivas são conclusões da heurística; os **100 nenhum_alterna** e **3 ambos_alternam** expõem suas abstenções. Número de conclusões erradas: não mensurável pela saída. `[nao verificado]` |
| Grade atual não reconstrói a grade do ato | O leitor consulta `CelulaDia` por colaborador/data, sem parâmetro de instante histórico. `app/escala/services/leitor_celula.py:350`, `app/escala/services/leitor_celula.py:362`. Potencialmente **617** casos expostos; quantidade de grades efetivamente alteradas não consta. Os **146 None** na ocupação não comprovam alteração de grade. `[nao verificado]` |
| Resposta atual pode diferir da registrada no log | A derivação lê `p.resposta_colab`; o log grava a resposta vigente daquela chamada. `app/chamados/services/materializacao.py:280`, `app/chamados/services/materializacao.py:308`. O modelo reconhece respostas anteriores superadas. `app/chamados/models.py:3501`. Potencialmente **617** expostos; quantidade efetivamente alterada não consta. `[nao verificado]` |
| Ausência de plantio vivo vinculado não prova ausência histórica de plantio | `pergunta_origem` aceita nulo; seu próprio contrato inclui plantio anterior a 06/09 entre os registros sem vínculo. `app/ponto/models.py:40`. Isso limita a interpretação dos **366 sem_plantio_vivo**. `[nao verificado]` |
| Ocupação atual não basta para provar ocupação histórica | Há campos distintos de instante da batida, criação e retratação. `app/ponto/models.py:23`, `app/ponto/models.py:67`, `app/ponto/models.py:79`. Sem o script, não se confere como esses tempos e o raio de 30 minutos foram empregados. A saída possui **225 True**, **246 False**, **146 None**; erros efetivos não são contáveis. `[nao verificado]` |

O cálculo de `t2` não filtra marco vazio: seleciona entre marcos com hora e tipo válidos, escolhe o mais próximo e aplica a tolerância. `app/chamados/services/materializacao.py:293`. A tolerância é 240 minutos. `app/chamados/services/declaracao_texto.py:25`. Quando ambos divergem, o retorno favorece `t1`. `app/chamados/services/materializacao.py:310`.

Para `col824`, `CASO 52752 None` só mostra ausência de entrada naquele mapeamento fornecido; não prova inexistência da pergunta nem ausência em todos os logs. `pk52753`: entrada histórica `intervalo_volta`, resposta `06:00`, `t1=E`, `t2=S`; saída com plantado `E`, ocupação `False`, classificação `nenhum_alterna`. `pk53024`: entrada histórica `orfao_14h`, resposta `01:01`, `t1=S`, `t2=E`; saída sem plantio vivo e ocupação `None`. `[nao verificado]`

## 13/11 14:38 · sessao 27bbcb37f3da · HJ-092 · sonda · origem=Code

**(a)** Na ATA fornecida da célula **128589 / col824**, **nenhuma lâmpada acesa tem `tipo_real` diferente do marco**: 17:00=`E/E`, 22:00=`S/S`, 23:00=`E/E`. Às 05:00, o marco é `S`, mas está apagado, com `tipo_real=null`; isso não demonstra inversão. A oposição mencionada no laudo anterior não aparece nesta ATA. [nao verificado]

A origem de `tipo_real` é:

- A grade monta `locs` com **`b.tipo`**, o tipo gravado da batida (`app/escala/utils.py:1323`).
- O alinhamento passa esse valor para `_celula_do_marco`, tanto no ramo por papel quanto no ramo por custo (`app/escala/utils.py:619`, `app/escala/utils.py:631`, `app/escala/utils.py:661`).
- A célula guarda separadamente `tipo/papel=tipo_m` e `tipo_real=tipo_real` (`app/escala/utils.py:326–327`).
- O cartório copia `x['tipo_real']` para a lâmpada; não o substitui pelo papel (`app/ponto/services/cartorio.py:711–713`).

Portanto, **`tipo_real` representa o tipo gravado da batida casada**, enquanto `tipo/papel` representa o marco. No anexo, 22:00=`S` coincide com **pk152080**, e 23:00=`E` coincide com **pk152081**. [nao verificado]

**(b)** A sequência e o pareamento **efetivamente exibido em `TURNO_SEM_MARCOS`** são:

| pk | Hora local | Tipo gravado | Resultado exibido |
|---|---|---|---|
| 143310 | 01/11 12:56:03 | E | Turno aberto. [nao verificado] |
| 143421 | 01/11 17:00:04 | E | Turno aberto. [nao verificado] |
| 143443 | 01/11 18:00:05 | E | Entrada do turno fechado. [nao verificado] |
| 152080 | 01/11 22:00:00 | S | Interna: saída para intervalo. [nao verificado] |
| 152081 | 01/11 23:00:00 | E | Interna: volta do intervalo. [nao verificado] |
| 143650 | 02/11 01:01:06 | S | Fecha o turno iniciado em pk143443. [nao verificado] |

**Sim: 17:00 e 18:00 já são entradas gravadas; 18:00 inicia o único turno fechado exibido.** [nao verificado] Uma `E` sucessiva preserva o turno anterior aberto e inicia outro (`app/ponto/turnos.py:1417–1420`). Sem marcos, o par `S22:00→E23:00` é reconhecido como intervalo pela existência da `S` posterior (`app/ponto/turnos.py:393–420`).

Para a célula, a grade chama `turnos_do_colab` e entrega seus turnos a `realizado_do_dia` (`app/escala/utils.py:1898`, `app/escala/utils.py:2045–2049`). O espelho também chama esse wrapper (`app/ponto/services/espelho.py:614`, `app/ponto/services/espelho.py:675–676`). O wrapper chama `parear_turnos` com papel da ATA, marcos e DNA diário (`app/ponto/turnos.py:1939`, `app/ponto/turnos.py:1992–1994`).

Com a ATA fornecida, o marco final apagado impede **esta célula** de emitir papel ao pareador; sem papel, vale o tipo gravado (`app/ponto/turnos.py:1682–1683`, `app/ponto/turnos.py:1382`). Com os marcos 22:00/23:00, essas batidas são absorvidas como intervalo (`app/ponto/turnos.py:1356–1376`).

**(c)** **Sim, a reconstituição fecha em 240 + 121 = 361:**

- **240 min:** pk143443 **18:00 → 22:00** pk152080. [nao verificado]
- **121 min:** pk152081 **23:00 → 01:01 do dia seguinte** pk143650. [nao verificado]

O código calcula **um turno bruto menos a pausa**: `421 − 60 = 361`; elimina segundos antes das diferenças e desconta o intervalo reconhecido (`app/ponto/turnos.py:605–606`, `app/ponto/turnos.py:740–741`, `app/ponto/turnos.py:756–771`). Os turnos abertos sem par fechado não acrescentam minutos (`app/ponto/turnos.py:735–739`).

A sonda não imprime os turnos efetivos de `turnos_do_colab` com toda a alimentação da janela. Para comprovar essa execução específica, faltam seus pares/pks e o mapa efetivo de papéis, incluindo eventuais batidas e células vizinhas. [nao verificado]

**(d) BATIDA — lista do admin:** há entradas consecutivas pk143310/pk143421/pk143443 e nenhuma oposição nas lâmpadas acesas da ATA apresentada [nao verificado]; `tipo_real` preserva o tipo gravado e cada nova `E` abre outro turno (`app/escala/utils.py:1323`, `app/escala/utils.py:619`, `app/ponto/turnos.py:1417–1420`).

## 13/11 14:37 · sessao c02396b340e5 · - · - · origem=Ronald (tmux)

A conta é **361 = 240 + 121** na reconstituição das seis BAT fornecidas: **pk143443 18:00 → pk152080 22:00 = 240 min** e **pk152081 23:00 → pk143650 01:01 do dia seguinte = 121 min**. Os vínculos pk/hora vêm exclusivamente do anexo, sem arquivo rastreável `[nao verificado]`; a regra trunca segundos (`app/ponto/turnos.py:605`), calcula bruto entrada→fim (`app/ponto/turnos.py:740`) e subtrai pausa marcada (`app/ponto/turnos.py:756`, `app/ponto/turnos.py:767`).

O cálculo efetivamente implementado equivale a **421 − 60 = 361**: turno 18:00→01:01, pausa S22:00→E23:00; pausas marcadas S→E são reconhecidas em `app/ponto/turnos.py:875` e `app/ponto/turnos.py:880`, com piso de 15 minutos em `app/ponto/turnos.py:545`. A aplicação aos horários do anexo é reconstituição `[nao verificado]`.

Na sequência apresentada, E12:56 e E17:00 ficam em turnos abertos quando chega a próxima E; E18:00 passa a ser a entrada do turno fechado pela S01:01 (`app/ponto/turnos.py:1417`, `app/ponto/turnos.py:1419`, `app/ponto/turnos.py:1491`). Os abertos sem fim fechado não somam minutos (`app/ponto/turnos.py:735`, `app/ponto/turnos.py:738`). Com marcos 22:00/23:00, essas duas batidas são absorvidas como intervalo interno (`app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1364`, `app/ponto/turnos.py:1371`).

A ATA fornecida tem saída final apagada `[nao verificado]`; nessa condição, esta célula não emite papel para substituir o tipo gravado: o mapa exige dia com entrada e saída, sem `em_aberto` (`app/ponto/turnos.py:1682`). Sem papel, o pareador usa `tipo` (`app/ponto/turnos.py:1382`).

A grade chama `turnos_do_colab` com a ATA montada em memória (`app/escala/utils.py:1886`, `app/escala/utils.py:1898`), pede o realizado a partir desses turnos (`app/escala/utils.py:2045`) e publica o resultado (`app/escala/utils.py:2058`). O espelho também pede turnos (`app/ponto/services/espelho.py:614`) e calcula realizado pela mesma autoridade (`app/ponto/services/espelho.py:290`, `app/ponto/services/espelho.py:675`, `app/ponto/services/espelho.py:783`).

**Limite:** a saída imprime `TURNO_SEM_MARCOS`, não os turnos efetivos de `turnos_do_colab` com toda a alimentação da janela; faltam esses turnos, o mapa de papel e eventuais BAT/células do pad para comprovar a execução concreta da produção `[nao verificado]`. O caminho efetivo recebe papel, marcos de template e DNA em `app/ponto/turnos.py:1939` e `app/ponto/turnos.py:1992`.

## 13/11 14:37 · sessao fffaf9e35fce · - · - · origem=Ronald (tmux)

- **Sequência fornecida:** `143310 01/11 12:56:03 E → 143421 17:00:04 E → 143443 18:00:05 E → 152080 22:00:00 S → 152081 23:00:00 E → 143650 02/11 01:01:06 S`. A ocorrência e os campos em produção provêm exclusivamente do dado fornecido, sem arquivo local correspondente. `[nao verificado]`

- **Pareamento exibido em `TURNO_SEM_MARCOS`:** `143310` aberto; `143421` aberto; `143443 → 143650` fechado, contendo `152080/152081` como intervalo interno. `[nao verificado]` O código explica esse resultado: sem marcos, `_marca_intra_duracao` reconhece `S → E` em até três horas quando há `S` posterior provando continuidade (`app/ponto/turnos.py:393-420`); cada `E` sucessiva preserva o turno anterior aberto e inicia outro (`app/ponto/turnos.py:1417-1420`).

- **Entrada 17:00/18:00:** ambas são entradas gravadas na saída fornecida; `143421` às 17:00 permanece aberta e `143443` às 18:00 abre o único turno fechado exibido. `[nao verificado]` Não houve necessidade de converter uma batida `S` em `E`: o pareador usa o papel da ATA quando disponível e, na ausência dele, usa `b.tipo` (`app/ponto/turnos.py:1382`).

- **Caminho real da célula/espelho:** a grade chama `turnos_do_colab` (`app/escala/utils.py:1898`); esse wrapper obtém o mapa de papéis da ATA (`app/ponto/turnos.py:1939`) e chama `parear_turnos` com papel, marcos de intervalo, bordas e DNA diário (`app/ponto/turnos.py:1992-1994`). O espelho chama o mesmo wrapper (`app/ponto/services/espelho.py:611-614`), passa seus turnos à autoridade dos minutos (`app/ponto/services/espelho.py:675-676`) e publica `_real_dia.minutos` (`app/ponto/services/espelho.py:783`). O cartório chega à grade por `montar_grade_prevista_periodo` (`app/ponto/services/cartorio.py:1175`).

- **Resultado esperado com DNA/ATA fornecidos:** o marco final apagado impede que essa ATA forneça papel ao pareador, pois `papel_por_minuto_da_ata` se abstém quando o dia está aberto ou falta entrada/saída (`app/ponto/turnos.py:1682-1683`). Os marcos 22:00/23:00 são absorvidos como intervalo por proximidade aos marcos, permanecendo internos ao turno (`app/ponto/turnos.py:1356-1376`); portanto a reprodução pela sequência e DNA fornecidos mantém `143443 → 143650`, sem converter 17:00/18:00. O anexo não imprime o resultado específico de `turnos_do_colab` com toda a janela e suas células vizinhas; a identidade desse resultado em produção permanece `[nao verificado]`.

- **361 minutos:** são compatíveis com um único turno `143443 18:00 → 143650 01:01`, descontado o intervalo `152080 22:00 → 152081 23:00`: `421 − 60 = 361`, equivalente a `240` (`143443 → 152080`) mais `121` (`152081 → 143650`). A autoridade trunca os horários para o minuto antes da diferença (`app/ponto/turnos.py:740-741`), desconta os intervalos reconhecidos (`app/ponto/turnos.py:756-767`) e soma o líquido (`app/ponto/turnos.py:770-771`); a grade publica esse resultado (`app/escala/utils.py:2045-2058`). **Não são dois turnos fechados independentes.** `17:00 → 22:00` daria 300 minutos e não explica a parcela 240 pela mesma regra (`app/ponto/turnos.py:740-741`).

## 13/11 14:22 · sessao d07877763393 · E3-FORMA-SQL · leitura · origem=Code

1. **Não dá para atribuir cada parcela dos +79 a uma mudança específica.** O raspado fornece diferenças líquidas, sem identificar os dias que entraram e saíram [nao verificado].

   | Recorte no anexo | Aumento | Mudança causadora / dono L-099 |
   |---|---:|---|
   | Competência `[0]` / `[1]` | +16 / +63 | [nao verificado] |
   | Empresa×competência `[2]` | +12 | [nao verificado] |
   | Empresa×competência `[3]` | +52 | [nao verificado] |
   | Empresa×competência `[4]` | +4 | [nao verificado] |
   | Empresa×competência `[5]` | +10 | [nao verificado] |
   | Empresa×competência `[7]` | +1 | [nao verificado] |

   As chaves empresa/competência desses itens não aparecem no raspado; não cabe identificá-las pelos índices [nao verificado].

   A mudança principal é **pedida** pelo aval E3-LEITORES: no diff, `porta_alcancavel_do_dia(...)` vira `porta_da_tarefa(carga.tarefa(...))`; implementação em [vigia_fio.py:640](app/chamados/services/vigia_fio.py:640). A tarefa escolhe portas pelas ações exigíveis em [tarefa_do_dia.py:547](app/chamados/services/tarefa_do_dia.py:547). Mas ausência de porta ainda atravessa os baldes legais antes de incrementar o contador — [vigia_fio.py:662](app/chamados/services/vigia_fio.py:662), [vigia_fio.py:704](app/chamados/services/vigia_fio.py:704). Portanto, **não está demonstrado que cada parcela seja exclusivamente efeito pedido** [nao verificado].

   Há outras mudanças a isolar: relógio repassado ao C6, diff `pergunta_viva(p)` → `pergunta_viva(p, agora=agora)`, em [juizes.py:1603](app/chamados/juizes.py:1603); trim SQL em [models.py:3397](app/chamados/models.py:3397); e exclusão de veredito `''` da absolvição, diff `+ ~Q(celula__veredito='')`, em [models.py:3408](app/chamados/models.py:3408). Essa última é distinta do trim autorizado; participação nos +79 e autorização específica [nao verificado].

   As identidades agregadas acompanharem o total não fornece decomposição causal; `_alcance` reutiliza o mesmo censo em [backfill_fio_morto_e4b.py:78](app/chamados/management/commands/backfill_fio_morto_e4b.py:78). **Dono de cada parcela: [nao verificado]**; faltam os dias e insumos para o juiz que distingue CADASTRO, BATIDA e ESTRUTURA em [e6_oraculo.py:98](app/ponto/management/commands/e6_oraculo.py:98).

2. **O LEITOR compara observações diferentes; também existe mudança semântica real no código. Não é simples mudança de lugar da função.** A cópia conserva `alimentar_portas` em [vigia_fio.py:608](app/chamados/services/vigia_fio.py:608) e acrescenta a leitura da tarefa em [vigia_fio.py:641](app/chamados/services/vigia_fio.py:641); o trecho removido no diff consultava C17 diretamente.

   Pelo script anexado, `alimentar_portas` observa identidade célula/colaborador e perguntas/fila; `carga.tarefa` observa colaborador/dia e retorno completo. Assim, faixas, hashes e classes desses dois registros não são comparação da mesma resposta [nao verificado]. As 34 classes não demonstram 34 comportamentos novos [nao verificado].

   **Os 2387 recebem a mesma resposta? Não dá para dizer** [nao verificado]. Falta parear por colaborador/dia e comparar uma projeção comum de porta e o resultado final do leitor, incluindo exceções. O comparador local já confronta C17 e tarefa por dia em [diff_e3_sombra.py:125](app/chamados/management/commands/diff_e3_sombra.py:125), mas esse resultado não foi apresentado [nao verificado]. **Dono da comparabilidade do instrumento: ESTRUTURA; donos dos dias: [nao verificado].**

3. **Não dá para explicar numericamente +9626/+2 exceções nem atribuir o pico inteiro ao diff** [nao verificado]. O script soma exceções externas e internas e exercita a nova recusa por pergunta; essa recusa lança `RespondidaSemTexto` em [fio.py:1602](app/chamados/services/fio.py:1602). Também há exceções engolidas pelo fio em [fio.py:349](app/chamados/services/fio.py:349) e [fio.py:375](app/chamados/services/fio.py:375). Faltam contagens por leitor, classe e unidade para separar recusas esperadas de falhas [nao verificado].

   **Existe custo adicional de memória no caminho real:** o diff acrescenta a carga em [vigia_fio.py:640](app/chamados/services/vigia_fio.py:640), mantendo alimentação/perguntas anteriores em [vigia_fio.py:608](app/chamados/services/vigia_fio.py:608). A carga conserva perguntas, decisões e pares colaboradores×dias — [tarefa_do_dia.py:376](app/chamados/services/tarefa_do_dia.py:376), [tarefa_do_dia.py:391](app/chamados/services/tarefa_do_dia.py:391), [tarefa_do_dia.py:423](app/chamados/services/tarefa_do_dia.py:423). O comando de produção chama esse caminho em [vigiar_fio_morto.py:176](app/chamados/management/commands/vigiar_fio_morto.py:176).

   Isso demonstra ampliação de alocação, mas **1233 MB por processo, vazamento ou OOM em produção: [nao verificado]**. Falta perfil por consumidor, sem retenção da sonda, incluindo concorrência. **Dono: ESTRUTURA.**

4. **O zero apresentado cobre o LEITOR `respondida_sem_texto`, não toda a frota de leitores** [nao verificado]. A função real verifica carimbo e chama `texto_vazio`, que aplica `(t or '').strip()` — [juizes.py:1518](app/chamados/juizes.py:1518), [juizes.py:1512](app/chamados/juizes.py:1512). O script programa essa chamada por pergunta global; não houve alteração desse predicado no diff fornecido [nao verificado].

   Esse zero não comprova equivalência das faces SQL, de `q_vivas` ou dos consumidores da tarefa, cujos caminhos são distintos — [juizes.py:1538](app/chamados/juizes.py:1538), [models.py:3397](app/chamados/models.py:3397). Como `rodar` também registra `EXC`, faltam as exceções desse LEITOR para afirmar 49257 retornos booleanos bem-sucedidos [nao verificado]. **Dono da interpretação/cobertura: ESTRUTURA.**

Há ainda uma lacuna de instrumento: o script textual anexado não calcula memória, identidades, classes ou blocos por faixa/bitmap no formato apresentado; falta a versão efetiva ou transformação intermediária [nao verificado].

VEREDITO DE IMPACTO: nao da para dizer em parcelas dos +79, equivalência dos 2387 e atribuição das exceções/pico [nao verificado].

## 13/11 14:21 · sessao 977450ce877d · - · - · origem=Ronald (tmux)

Q3 — Não é possível explicar numericamente +9.626/+2 exceções nem atribuir os MB ao diff com os anexos; faltam contagens por leitor/classe/unidade e perfil de memória isolado [nao verificado].

- A sonda textual soma tanto exceções externas de `rodar` quanto internas de dois wrappers; a recusa nova é chamada para cada pergunta e pode lançar `RespondidaSemTexto` (`app/chamados/services/fio.py:1602`). Portanto “exceções registradas” inclui recusa de domínio, e não equivale automaticamente a falhas novas de produção; quanto dos incrementos vem disso é [nao verificado]. O diff que adiciona esse caminho é `+def recusa_respondida_sem_texto` e os chamadores nas portas de escrita; cópia `app/chamados/models.py:3472`, `app/chamados/models.py:3508`.
- Há exceções internas engolidas no fio: explicador em `app/chamados/services/fio.py:346`/`:349` e aplicação em `:373`/`:375`; os totais não revelam qual delas ocorreu [nao verificado].
- O texto fornecido não implementa `memoria/pico_mb` nem o formato observado de blocos por faixa/bitmap: esses resultados pressupõem instrumento ou pós-processamento adicional cuja implementação é [nao verificado]. Isso limita qualquer atribuição do pico.

Há custo adicional real no caminho de produção, não apenas na instrumentação:

- O diff troca C17 por `carga_da_tarefa` em `app/chamados/services/vigia_fio.py:640` e conserva simultaneamente a alimentação e perguntas anteriores (`:608`, `:618`). O cron real chama esse caminho em `app/chamados/management/commands/vigiar_fio_morto.py:176`.
- A carga mantém cadastro (`app/chamados/services/tarefa_do_dia.py:372`), perguntas completas (`:376`/`:382`), decisões (`:391`/`:394`), pares colaboradores×todos os dias da janela (`:423`/`:429`) e caches por dia (`:503`, `:508`, `:514`). A classe herdada mantém células/escalas (`app/ponto/turnos.py:2978`, `:2986`) e, quando necessário, batidas/turnos (`:3016`, `:3021`, `:3036`).
- Outros consumidores novos carregam esses objetos: folha (`app/folha/fora_da_folha.py:178`), furo (`app/chamados/services/furo_sem_canal.py:91`), beco, que conserva cargas por janela (`app/escala/services/cadastro_realidade.py:492`).

Assim, existe ampliação estrutural de alocação no caminho real; afirmar “1233 MB por processo de produção”, vazamento ou OOM seria [nao verificado]. Para medir: cada consumidor real isolado com/sem perfil/registro canônico da sonda, mesmas entradas, RSS e heap por fase, cardinalidades/janela, retenção após retorno e concorrência/orçamento do worker [nao verificado]. Dono da implementação/instrumento/custo: **ESTRUTURA**; não há evidência para deslocar esse problema a CADASTRO/BATIDA [nao verificado quanto às ocorrências individuais].

## 13/11 14:21 · sessao 6b4d651b1e6b · - · - · origem=Ronald (tmux)

Q2: **Há mudança de instrumento e mudança real de comportamento; o anexo não permite dizer que os 2.387 registros receberam a mesma resposta.**

- O observador base `alimentar_portas` registra `[pk_celula, cid]` e somente `perguntas/fila`; o observador cópia `carga.tarefa` registra `[cid,dia]` e a tarefa inteira — conforme `perfil` do script anexado, sem arquivo local numerado [nao verificado].
- A função não apenas mudou de lugar: o vigia ainda chama `alimentar_portas` em [app/chamados/services/vigia_fio.py:608](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/vigia_fio.py:608) e passou a consultar `carga.tarefa`/`porta_da_tarefa` em [app/chamados/services/vigia_fio.py:640](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/vigia_fio.py:640). O diff anexo substitui ali C17 por tarefa.
- O C17 considera chamado da célula na fila ou pergunta viva no app ancorada na fila ([app/chamados/juizes.py:1677](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1677)); a tarefa escolhe portas a partir das ações exigíveis ([app/chamados/services/tarefa_do_dia.py:547](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/services/tarefa_do_dia.py:547)). Há mudança semântica implementada.
- As 34 classes refletem campos disponíveis no retorno completo da tarefa; sua ausência no registro base não comprova que inexistiam naquele comportamento [nao verificado]. O mesmo `n_total` conta observações, sem demonstrar correspondência entre unidades [nao verificado].
- Falta comparar, pela mesma chave `(empresa, competência, colaborador, dia)`, a saída final de cada leitor e uma projeção comum de porta (`rota/fila/verbos/fio`), preservando exceções e ramo. O instrumento local já compara C17 e tarefa no mesmo dia em [app/chamados/management/commands/diff_e3_sombra.py:125](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/management/commands/diff_e3_sombra.py:125), mas seu resultado não foi fornecido [nao verificado].
- **Dono:** a divergência de instrumento é ESTRUTURA; os donos L-099 dos dias não podem ser determinados sem insumos do juiz [app/ponto/management/commands/e6_oraculo.py:98](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/ponto/management/commands/e6_oraculo.py:98).

Q4: **O zero fornecido cobre somente o leitor chamado `respondida_sem_texto`, em 49.257 observações; não comprova paridade de toda a frota de leitores** [nao verificado: resumo fornecido].

- A função real testa `respondida_em is not None` e chama `texto_vazio`, cujo teste é `(t or '').strip()` ([app/chamados/juizes.py:1518](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d533ec2348b4d4/app/chamados/juizes.py:1518), caminho correto abaixo; [app/chamados/juizes.py:1512](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1512)).
- No script anexado, esse leitor é chamado diretamente, uma vez por pergunta global, em instância independente. As demais funções são registradas sob outros nomes [nao verificado: script sem arquivo local numerado].
- Portanto esse zero não cobre `q_respondida_sem_texto`, `q_respondida_com_texto` ([app/chamados/juizes.py:1531](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1531), [app/chamados/juizes.py:1541](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/juizes.py:1541)), nem `q_vivas` ([app/chamados/models.py:3396](/home/ronald/codex_ws/91bed16941419bc8c2f1a97097d533ec2348b4d4/app/chamados/models.py:3396)), nem os leitores de porta.
- `rodar` também registra `("EXC", classe)` como valor. Sem o conteúdo de `excecoes_por_classe` desse leitor, zero diferenças não comprova 49.257 retornos booleanos bem-sucedidos [nao verificado].
- **Dono:** ESTRUTURA para escopo/interpretação da medição; dono L-099 de dias individuais [nao verificado].

Observação para consolidação: o script textual anexado não produz o formato de `diferencas.txt`: imprime `blocos` inteiro e lotes sequenciais; não contém cálculo de `faixa`, bitmap, `classes`, `identidades_vigia` nem pico de memória. Falta a versão efetivamente executada ou a transformação intermediária para verificar o significado exato desses campos [nao verificado].

## 13/11 14:20 · sessao 4f6c776a50bd · - · - · origem=Ronald (tmux)

- O aumento líquido é **+79**, dividido em **+16 e +63** nos índices das competências; por empresa×competência, **+12, +52, +4, +10 e +1**. O anexo omite as chaves dos respectivos itens: identificar empresas/competências e causalidade individual é **[nao verificado]**.
- A troca concreta é `porta_alcancavel_do_dia(...)` → `porta_da_tarefa(carga.tarefa(...))`, no hunk `vigia_fio.py @@ -632`; cópia: `app/chamados/services/vigia_fio.py:639-641`. A tarefa só publica portas vinculadas às ações exigíveis: `app/chamados/services/tarefa_do_dia.py:547-565`. Porta publicada escapa do contador em `app/chamados/services/vigia_fio.py:648-655`; ausência de porta ainda passa pelos baldes de lei em `:660-668`; apenas o restante entra em `fio_morto_alcance`, `:704-708`.
- O aval E3-LEITORES pede essa troca, mas o raspado não mostra quais transições produziram cada parcela, nem entradas/saídas compensatórias: chamar os **79** de efeito exclusivamente pedido é **[nao verificado]**.
- Outros deltas capazes de influenciar a leitura: relógio chega ao C6 em `app/chamados/juizes.py:1603` (hunk `@@ -1570`); `q_vivas` lê a face SQL do texto em `app/chamados/models.py:3396-3397` e passa a excluir veredito `''` da absolvição em `:3408` (hunk `@@ -3346`). Essa segunda alteração é distinta do trim da resposta autorizado em aval.txt; efeito numérico dela **[nao verificado]**.
- Dono L-099 **não pode ser inferido desses agregados**. O juiz precisa dos insumos por dia e decide CADASTRO/BATIDA/ESTRUTURA em `app/ponto/management/commands/e6_oraculo.py:98-110`. `sem_porta` tampouco significa automaticamente ESTRUTURA: a retirada de BATIDA/CADASTRO exige presença na lista, `app/chamados/services/vigia_fio.py:222-228`, `:313-315`.
- As `identidades_vigia` acompanharam o total, mas provam somente as igualdades agregadas apresentadas; não demonstram mesma resposta por dia nem atribuição causal **[nao verificado]**.
- Medida faltante: comparação por `(colaborador, dia)` com rota/porta anterior e posterior, causa/balde, decisão da tarefa, recorte empresa×competência e retorno do juiz L-099; contrafactuais que isolem troca da porta, relógio e mudanças SQL. O próprio comparador existente enumera pares e compara projeções em `app/chamados/management/commands/diff_e3_sombra.py:123-139`, mas seu resultado por essas parcelas não foi fornecido **[nao verificado]**.

## 13/11 14:13 · sessao b3b8c64d7738 · BUG-PAR-DESCONGELA · sonda · origem=Code

A sonda falhou: `sonda168.py nao existe` (saída da tentativa: [nao verificado]); a verificação encerra com código 1 em `bin/sonda_leitura.sh:71`.

Saída integral da tentativa [nao verificado]:

```text
sonda_leitura: sonda168.py nao existe
```

Saída integral fornecida no pedido; todo o bloco é [nao verificado], sem reexecução:

```text
sonda_leitura: sonda168.py [033fac1559a4a6d2] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO PARIDADE 244 ERROS_LAMPADA 0
GRUPO ('chamado_encerrado', 'antes_18_10', 'apagado') respostas 11 colabs 2
GRUPO ('chamado_encerrado', 'antes_18_10', 'nao_da_para_dizer') respostas 1 colabs 1
GRUPO ('declaracao', 'desde_18_10', 'aceso') respostas 4 colabs 4
GRUPO ('nulo', '-', 'aceso') respostas 2 colabs 1
GRUPO ('nulo', '-', 'apagado') respostas 57 colabs 16
GRUPO ('nulo', '-', 'nao_da_para_dizer') respostas 1 colabs 1
GRUPO ('paridade_recusada', 'antes_18_10', 'aceso') respostas 41 colabs 27
GRUPO ('paridade_recusada', 'antes_18_10', 'apagado') respostas 87 colabs 39
GRUPO ('paridade_recusada', 'antes_18_10', 'nao_da_para_dizer') respostas 16 colabs 6
GRUPO ('paridade_recusada', 'desde_18_10', 'aceso') respostas 24 colabs 14
SOMA_POR_VIA {'paridade_recusada': 168, 'declaracao': 4, 'chamado_encerrado': 12, 'nulo': 60}
SEM_CONTA (via fora de nulo/paridade_recusada) 16
   {'pk': 32764, 'col': 824, 'motivo': 'intervalo_saida', 'data': '2026-09-17', 'via': 'declaracao', 'carimbo': '2026-11-11', 'validada': True, 'lampada': 'aceso', 'celula': 20260, 'disputa_fechada': False, 'irma': (32765, 'declaracao', True)}
   {'pk': 34744, 'col': 879, 'motivo': 'intervalo_saida', 'data': '2026-09-21', 'via': 'chamado_encerrado', 'carimbo': '2026-09-22', 'validada': True, 'lampada': 'apagado', 'celula': 108293, 'disputa_fechada': False, 'irma': (34745, 'chamado_encerrado', True)}
   {'pk': 34745, 'col': 879, 'motivo': 'intervalo_volta', 'data': '2026-09-21', 'via': 'chamado_encerrado', 'carimbo': '2026-09-22', 'validada': True, 'lampada': 'apagado', 'celula': 108293, 'disputa_fechada': False, 'irma': (34744, 'chamado_encerrado', True)}
   {'pk': 36609, 'col': 821, 'motivo': 'intervalo_saida', 'data': '2026-09-26', 'via': 'chamado_encerrado', 'carimbo': '2026-09-26', 'validada': True, 'lampada': 'nao_da_para_dizer', 'celula': 115598, 'disputa_fechada': False, 'irma': (43762, 'chamado_encerrado', False)}
   {'pk': 38260, 'col': 515, 'motivo': 'intervalo_saida', 'data': '2026-09-25', 'via': 'chamado_encerrado', 'carimbo': '2026-10-06', 'validada': True, 'lampada': 'apagado', 'celula': 105607, 'disputa_fechada': True, 'irma': (38261, 'chamado_encerrado', True)}
   {'pk': 38261, 'col': 515, 'motivo': 'intervalo_volta', 'data': '2026-09-25', 'via': 'chamado_encerrado', 'carimbo': '2026-10-06', 'validada': True, 'lampada': 'apagado', 'celula': 105607, 'disputa_fechada': True, 'irma': (38260, 'chamado_encerrado', True)}
   {'pk': 38264, 'col': 515, 'motivo': 'intervalo_saida', 'data': '2026-09-27', 'via': 'chamado_encerrado', 'carimbo': '2026-10-06', 'validada': True, 'lampada': 'apagado', 'celula': 105609, 'disputa_fechada': True, 'irma': (38265, 'declaracao', True)}
   {'pk': 38268, 'col': 515, 'motivo': 'intervalo_saida', 'data': '2026-09-29', 'via': 'chamado_encerrado', 'carimbo': '2026-10-06', 'validada': True, 'lampada': 'apagado', 'celula': 105611, 'disputa_fechada': True, 'irma': (38269, 'chamado_encerrado', True)}
   {'pk': 38269, 'col': 515, 'motivo': 'intervalo_volta', 'data': '2026-09-29', 'via': 'chamado_encerrado', 'carimbo': '2026-10-06', 'validada': True, 'lampada': 'apagado', 'celula': 105611, 'disputa_fechada': True, 'irma': (38268, 'chamado_encerrado', True)}
   {'pk': 38863, 'col': 515, 'motivo': 'intervalo_saida', 'data': '2026-10-01', 'via': 'chamado_encerrado', 'carimbo': '2026-10-06', 'validada': True, 'lampada': 'apagado', 'celula': 105613, 'disputa_fechada': True, 'irma': (38864, 'chamado_encerrado', True)}
   {'pk': 38864, 'col': 515, 'motivo': 'intervalo_volta', 'data': '2026-10-01', 'via': 'chamado_encerrado', 'carimbo': '2026-10-06', 'validada': True, 'lampada': 'apagado', 'celula': 105613, 'disputa_fechada': True, 'irma': (38863, 'chamado_encerrado', True)}
   {'pk': 39564, 'col': 515, 'motivo': 'intervalo_saida', 'data': '2026-10-03', 'via': 'chamado_encerrado', 'carimbo': '2026-10-06', 'validada': True, 'lampada': 'apagado', 'celula': 105615, 'disputa_fechada': True, 'irma': (39565, 'chamado_encerrado', True)}
   {'pk': 39565, 'col': 515, 'motivo': 'intervalo_volta', 'data': '2026-10-03', 'via': 'chamado_encerrado', 'carimbo': '2026-10-06', 'validada': True, 'lampada': 'apagado', 'celula': 105615, 'disputa_fechada': True, 'irma': (39564, 'chamado_encerrado', True)}
   {'pk': 49126, 'col': 516, 'motivo': 'intervalo_volta', 'data': '2026-10-22', 'via': 'declaracao', 'carimbo': '2026-11-03', 'validada': True, 'lampada': 'aceso', 'celula': 137166, 'disputa_fechada': False, 'irma': (51350, 'nulo', False)}
   {'pk': 53456, 'col': 993, 'motivo': 'intervalo_saida', 'data': '2026-10-16', 'via': 'declaracao', 'carimbo': '2026-11-03', 'validada': True, 'lampada': 'aceso', 'celula': 124768, 'disputa_fechada': False, 'irma': (53435, 'declaracao', True)}
   {'pk': 54585, 'col': 909, 'motivo': 'intervalo_saida', 'data': '2026-11-05', 'via': 'declaracao', 'carimbo': '2026-11-06', 'validada': True, 'lampada': 'aceso', 'celula': 123856, 'disputa_fechada': False, 'irma': (54586, 'declaracao', True)}
PKS {"('paridade_recusada', 'antes_18_10', 'apagado')": [4695, 4800, 5040, 5367, 7463, 8778, 9485, 12771, 13033, 14953, 15014, 20890, 20960, 22586, 22627, 26588, 26589, 26782, 26963, 26984, 27039, 27109, 27110, 27112, 27135, 27136, 27137, 27160, 27209, 27210, 27211, 27328, 27331, 27332, 27450, 27451, 27452, 27461, 27464, 27560, 27561, 27667, 27668, 27673, 27674, 27679, 27680, 27690, 27727, 27728, 27798, 29469, 30009, 30010, 30011, 30012, 31342, 31343, 31373, 31718, 32645, 32646, 32649, 32914, 32915, 33034, 33516, 34130, 34719, 34763, 36077, 36567, 37859, 38289, 38433, 38529, 38647, 38950, 39759, 40197, 40850, 40851, 41705, 41988, 42762, 43255, 43256], "('paridade_recusada', 'antes_18_10', 'nao_da_para_dizer')": [4797, 4798, 5346, 6228, 12770, 14356, 14359, 14363, 14365, 14368, 14960, 14963, 16327, 22470, 23137, 27439]}
COLABS {"('paridade_recusada', 'antes_18_10', 'apagado')": [72, 82, 107, 163, 248, 252, 277, 334, 338, 340, 363, 366, 435, 438, 444, 460, 468, 489, 515, 518, 627, 723, 769, 787, 814, 825, 841, 843, 857, 859, 865, 876, 879, 889, 913, 926, 940, 941, 944], "('paridade_recusada', 'antes_18_10', 'nao_da_para_dizer')": [98, 104, 171, 334, 749, 881]}
```

1. `paridade_recusada`, `antes_18_10`: aceso 41 respostas/27 colabs; apagado 87/39; nao_da_para_dizer 16/6. [nao verificado]
2. `paridade_recusada`, `desde_18_10`: aceso 24 respostas/14 colabs; nenhum grupo exibido para apagado ou nao_da_para_dizer. [nao verificado]
3. `SEM_CONTA`: 16 respostas; todas com `validada=True`. Abaixo: `pk/col: via, lampada, irma=(pk,via,validada)`; D=`declaracao`, C=`chamado_encerrado`, N=`nulo`. [nao verificado]
4. `32764/824: D, aceso, (32765,D,True)`. [nao verificado]
5. `34744/879: C, apagado, (34745,C,True)`; `34745/879: C, apagado, (34744,C,True)`. [nao verificado]
6. `36609/821: C, nao_da_para_dizer, (43762,C,False)`. [nao verificado]
7. `38260/515: C, apagado, (38261,C,True)`; `38261/515: C, apagado, (38260,C,True)`. [nao verificado]
8. `38264/515: C, apagado, (38265,D,True)`; `38268/515: C, apagado, (38269,C,True)`; `38269/515: C, apagado, (38268,C,True)`. [nao verificado]
9. `38863/515: C, apagado, (38864,C,True)`; `38864/515: C, apagado, (38863,C,True)`. [nao verificado]
10. `39564/515: C, apagado, (39565,C,True)`; `39565/515: C, apagado, (39564,C,True)`. [nao verificado]
11. `49126/516: D, aceso, (51350,N,False)`; `53456/993: D, aceso, (53435,D,True)`. [nao verificado]
12. `54585/909: D, aceso, (54586,D,True)`. [nao verificado]

## 13/11 14:05 · sessao 97c1ef4ca10a · BUG-PAR-DESCONGELA · sonda · origem=Code

Não executei `sonda60.py`: o anexo contém a saída, sem o código da sonda. [nao verificado] O wrapper disponível escreve arquivos, contrariando a regra de somente leitura: [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/bin/sonda_leitura.sh:126).

Reprodução integral da saída fornecida, sem verificação em produção. [nao verificado]

```text
sonda_leitura: sonda60.py [4a6d0ca12cc5872c] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO PARIDADE 244 colabs 82
('carimbo', 'nulo') 60
('carimbo', 'via=chamado_encerrado') 12
('carimbo', 'via=declaracao') 4
('carimbo', 'via=paridade_recusada') 168
('comp', '07/2026') 4
('comp', '08/2026') 28
('comp', '09/2026') 85
('comp', '10/2026') 96
('comp', '11/2026') 31
('nulo', 'nao_validada', 'irma_ausente', 'disputa_aberta') 8
('nulo', 'nao_validada', 'irma_sem_carimbo_resp', 'disputa_aberta') 47
('nulo', 'nao_validada', 'irma_via=declaracao', 'disputa_aberta') 2
('nulo', 'nao_validada', 'irma_via=paridade_recusada', 'disputa_aberta') 3
('via=chamado_encerrado', 'validada', 'irma_via=chamado_encerrado', 'disputa_aberta') 3
('via=chamado_encerrado', 'validada', 'irma_via=chamado_encerrado', 'disputa_fechada') 8
('via=chamado_encerrado', 'validada', 'irma_via=declaracao', 'disputa_fechada') 1
('via=declaracao', 'validada', 'irma_sem_carimbo_semresp', 'disputa_aberta') 1
('via=declaracao', 'validada', 'irma_via=declaracao', 'disputa_aberta') 3
('via=paridade_recusada', 'validada', 'irma_ausente', 'disputa_aberta') 43
('via=paridade_recusada', 'validada', 'irma_ausente', 'disputa_fechada') 25
('via=paridade_recusada', 'validada', 'irma_sem_carimbo_resp', 'disputa_aberta') 5
('via=paridade_recusada', 'validada', 'irma_sem_carimbo_resp', 'disputa_fechada') 1
('via=paridade_recusada', 'validada', 'irma_sem_carimbo_semresp', 'disputa_aberta') 1
('via=paridade_recusada', 'validada', 'irma_via=admin_decidiu', 'disputa_fechada') 1
('via=paridade_recusada', 'validada', 'irma_via=chamado_encerrado', 'disputa_aberta') 2
('via=paridade_recusada', 'validada', 'irma_via=chamado_encerrado', 'disputa_fechada') 4
('via=paridade_recusada', 'validada', 'irma_via=competencia_trancada', 'disputa_aberta') 1
('via=paridade_recusada', 'validada', 'irma_via=completude_turno', 'disputa_aberta') 4
('via=paridade_recusada', 'validada', 'irma_via=completude_turno', 'disputa_fechada') 2
('via=paridade_recusada', 'validada', 'irma_via=declaracao', 'disputa_aberta') 14
('via=paridade_recusada', 'validada', 'irma_via=declaracao', 'disputa_fechada') 12
('via=paridade_recusada', 'validada', 'irma_via=duplicata', 'disputa_aberta') 2
('via=paridade_recusada', 'validada', 'irma_via=duplicata', 'disputa_fechada') 8
('via=paridade_recusada', 'validada', 'irma_via=juiz_aprovou', 'disputa_fechada') 1
('via=paridade_recusada', 'validada', 'irma_via=paridade_recusada', 'disputa_aberta') 22
('via=paridade_recusada', 'validada', 'irma_via=paridade_recusada', 'disputa_fechada') 20
COLABS_POR_CARIMBO {'via=paridade_recusada': 70, 'via=declaracao': 4, 'via=chamado_encerrado': 3, 'nulo': 18}
PKS {"('via=paridade_recusada', 'validada', 'irma_via=completude_turno', 'disputa_fechada')": [4248, 6228], "('via=paridade_recusada', 'validada', 'irma_via=completude_turno', 'disputa_aberta')": [4695, 6904, 7463, 9485], "('via=paridade_recusada', 'validada', 'irma_via=paridade_recusada', 'disputa_fechada')": [4797, 4798, 15994, 15995, 27136, 27137, 27667, 27668, 27727, 27728, 30009, 30010, 31342, 31343, 32645, 32646, 34259, 34400, 34719, 34749], "('via=paridade_recusada', 'validada', 'irma_via=chamado_encerrado', 'disputa_fechada')": [4800, 15482, 33308, 34130], "('via=paridade_recusada', 'validada', 'irma_via=duplicata', 'disputa_aberta')": [5040, 8778], "('via=paridade_recusada', 'validada', 'irma_ausente', 'disputa_fechada')": [5346, 6293, 10202, 12771, 13033, 14953, 27039, 27209, 27210, 27211, 27328, 27331, 27332, 27464, 27679, 33282, 33516, 34763, 36577, 37859, 38529, 39768, 40197, 46163, 49707], "('via=paridade_recusada', 'validada', 'irma_ausente', 'disputa_aberta')": [5367, 22627, 26782, 26963, 26984, 27109, 27110, 27112, 27160, 27427, 27431, 27433, 27435, 27437, 27439, 27449, 27452, 27461, 27680, 27690, 27798, 29469, 31373, 32649, 33034, 33097, 34853, 35676, 35727, 36077, 36567, 37961, 38289, 38647, 38674, 38950, 39759, 39766, 41733, 41988, 42762, 49608, 55681], "('via=paridade_recusada', 'validada', 'irma_via=duplicata', 'disputa_fechada')": [12770, 14356, 14359, 14363, 14365, 14368, 14960, 14963], "('via=paridade_recusada', 'validada', 'irma_via=juiz_aprovou', 'disputa_fechada')": [14874], "('via=paridade_recusada', 'validada', 'irma_via=declaracao', 'disputa_aberta')": [15014, 20890, 22586, 29419, 29929, 32358, 34303, 34792, 37398, 43728, 44107, 44525, 45737, 54942], "('via=paridade_recusada', 'validada', 'irma_via=competencia_trancada', 'disputa_aberta')": [15285], "('via=paridade_recusada', 'validada', 'irma_via=declaracao', 'disputa_fechada')": [15723, 16327, 22022, 22470, 26595, 31718, 31937, 31941, 35301, 35407, 37945, 38433], "('via=paridade_recusada', 'validada', 'irma_via=admin_decidiu', 'disputa_fechada')": [20960], "('via=paridade_recusada', 'validada', 'irma_via=chamado_encerrado', 'disputa_aberta')": [23137, 33760], "('via=paridade_recusada', 'validada', 'irma_sem_carimbo_semresp', 'disputa_aberta')": [23347], "('via=paridade_recusada', 'validada', 'irma_via=paridade_recusada', 'disputa_aberta')": [26588, 26589, 27450, 27451, 27560, 27561, 27673, 27674, 30011, 30012, 32914, 32915, 40850, 40851, 43255, 43256, 49154, 49155, 55437, 55438, 56214, 56260], "('via=paridade_recusada', 'validada', 'irma_sem_carimbo_resp', 'disputa_fechada')": [27135], "('via=paridade_recusada', 'validada', 'irma_sem_carimbo_resp', 'disputa_aberta')": [32716, 41705, 44362, 45399, 54689], "('via=declaracao', 'validada', 'irma_via=declaracao', 'disputa_aberta')": [32764, 53456, 54585], "('via=chamado_encerrado', 'validada', 'irma_via=chamado_encerrado', 'disputa_aberta')": [34744, 34745, 36609], "('nulo', 'nao_validada', 'irma_sem_carimbo_resp', 'disputa_aberta')": [36255, 41692, 41693, 45277, 45278, 45598, 45599, 47207, 47208, 49345, 49346, 49731, 49732, 50176, 50177, 51522, 51523, 51524, 51525, 51526, 51527, 51528, 51529, 51530, 51531, 51532, 51533, 51534, 51535, 51536, 51537, 51538, 51539, 51540, 51541, 51542, 51543, 51544, 51545, 51546, 51547, 51548, 51549, 51550, 51551, 55142, 55156], "('via=chamado_encerrado', 'validada', 'irma_via=chamado_encerrado', 'disputa_fechada')": [38260, 38261, 38268, 38269, 38863, 38864, 39564, 39565], "('via=chamado_encerrado', 'validada', 'irma_via=declaracao', 'disputa_fechada')": [38264], "('nulo', 'nao_validada', 'irma_ausente', 'disputa_aberta')": [39363, 46363, 47492, 48611, 49165, 49167, 49170, 56382], "('nulo', 'nao_validada', 'irma_via=declaracao', 'disputa_aberta')": [39872, 51739], "('nulo', 'nao_validada', 'irma_via=paridade_recusada', 'disputa_aberta')": [41706, 44363, 45400], "('via=declaracao', 'validada', 'irma_sem_carimbo_semresp', 'disputa_aberta')": [49126]}
CASO 51530/51531 [(51530, 'intervalo_saida', False, '', False, 'Pergunta #51530: batida vizinha #110812 '), (51531, 'intervalo_volta', False, '', False, 'Pergunta #51531: batida vizinha #110811 ')]
```

- Universo informado: 244 respostas, 82 colaboradores. [nao verificado]
- Carimbo nulo: 60; preenchido: 184 — `paridade_recusada`: 168, `chamado_encerrado`: 12, `declaracao`: 4. [nao verificado]
- Competências, no universo total: 07/2026: 4; 08/2026: 28; 09/2026: 85; 10/2026: 96; 11/2026: 31. [nao verificado]
- Distribuição por competência separada por carimbo/via: não apresentada. [nao verificado]
- 51530 (`intervalo_saida`) e 51531 (`intervalo_volta`): ambas no grupo `nulo`, `nao_validada`, `irma_sem_carimbo_resp`, `disputa_aberta`; batidas vizinhas 110812 e 110811, respectivamente. [nao verificado]

## 13/11 14:04 · sessao 3f6e6f46ae06 · DESLIGADO-NAO-TRAVA · verificador · origem=Code

**1. Achados (0) e (1): curados no código, com limites de apresentação e de prova.** (`app/folha/fora_da_folha.py:144`; `app/folha/retrato_folha.py:1193`)

- **(0), contador:** a fonte produz `desligados_retidos`; o censo agrega; o vigia publica no carimbo com esperado zero. O teste exige vazio normalmente e `[desligado.pk]` quando o export é simulado retendo desligado e ativo: detecta um contador artificialmente fixado em zero. (`app/folha/fora_da_folha.py:144`; `app/chamados/services/vigia_fio.py:589`; `app/chamados/management/commands/vigiar_fio_morto.py:101`; `app/chamados/management/commands/vigiar_fio_morto.py:246`; `app/folha/tests/test_lei_desligado_nao_trava.py:136`)
- **Limite do ESTADO:** o leitor genérico mostra a chave quando diverge do esperado. Com valor zero, não imprime nominalmente `desligados_retidos=0`; portanto, há transporte e exposição da violação, mas não exibição explícita do zero. (`bin/fio_estado.py:125`; `bin/fio_estado.py:191`; `bin/gerar_estado.py:260`)
- **(1), informativo:** `montar_linhas` obtém e grava `informativo`; o drawer lê a linha gravada e apresenta suas datas. O teste da lavra exige três datas para o desligado e vazio para ativo/afastado; o teste do template exige rótulo/datas e ausência do bloco com lista vazia ou chave ausente. (`app/folha/retrato_folha.py:1161`; `app/folha/retrato_folha.py:1193`; `app/colaboradores/views.py:984`; `app/templates/folha/_retrato_o_que_fazer.html:56`; `app/folha/tests/test_lei_desligado_nao_trava.py:117`; `app/folha/tests/test_lei_desligado_nao_trava.py:122`)

A cura do informativo alcança retratos novos ou relavrados: a leitura continua usando o JSON persistido. Relavra produtiva e execução desses testes nesta verificação: **[nao verificado]**. (`app/folha/retrato_folha.py:110`; `app/folha/management/commands/lavrar_retrato_folha.py:74`)

**2. Escritores, leitores e significado de `desligados_retidos`.**

| Etapa | Quem escreve/lê e significado |
|---|---|
| Fonte | Seleciona os colaboradores classificados por `furo_espelho`, deduplica pelo dict `_colabs` e produz **lista de ids únicos naquela empresa × competência**, perguntando a `esta_desligado`. (`app/folha/fora_da_folha.py:128`; `app/folha/fora_da_folha.py:131`; `app/folha/fora_da_folha.py:144`) |
| Agregador | Lê a lista e soma seu comprimento por empresa × competência. Produz **inteiro de ocorrências colaborador × empresa × competência**. (`app/chamados/services/vigia_fio.py:583`; `app/chamados/services/vigia_fio.py:589`) |
| Vigia | Lê o inteiro para stdout, copia para `censo` e publica também o bloco `alcance`; `_carimbar` persiste o JSON. (`app/chamados/management/commands/vigiar_fio_morto.py:120`; `app/chamados/management/commands/vigiar_fio_morto.py:212`; `app/chamados/management/commands/vigiar_fio_morto.py:246`; `app/chamados/management/commands/vigiar_fio_morto.py:248`) |
| ESTADO | Compara o escalar com o esperado do próprio carimbo e imprime divergências; `gerar_estado` delega a composição. (`bin/fio_estado.py:125`; `bin/fio_estado.py:191`; `bin/gerar_estado.py:260`) |

**O mesmo id em duas competências conta duas vezes.** Não há deduplicação global desse contador; a deduplicação por `(colaborador, data)` pertence à conta separada de dias. Não encontrei leitor convertendo dias em pessoas nessa cadeia. (`app/chamados/services/vigia_fio.py:589`; `app/chamados/services/vigia_fio.py:592`)

Assim, os consumidores são compatíveis com a agregação implementada, mas o resultado **não representa pessoas únicas da frota nem quantidade de dia-colab**. O comentário do agregador declara colaborador × competência; essa distinção deve acompanhar qualquer interpretação do número. (`app/chamados/services/vigia_fio.py:587`; `app/colaboradores/services/desligamento.py:18`)

**3. Informativo: autoridade correta; N+1 real.**

O helper não reconstrói dias ou regras: usa `janela_fechamento`, pergunta ao juiz `esta_desligado`, chama `folha.export.informativo_rescisao` e converte as datas para strings. A autoridade reutiliza o mesmo laço do retentor. (`app/folha/retrato_folha.py:1214`; `app/folha/retrato_folha.py:1217`; `app/folha/retrato_folha.py:1219`; `app/folha/export.py:1212`; `app/folha/export.py:1228`)

O custo incremental é **uma consulta de colaboradores + duas por desligado não isento cuja janela não colapse**: células e vínculos. Para uma folha de 750 pessoas, a fórmula é `1 + 2D`, sendo `D` os desligados elegíveis; se os 750 forem elegíveis, são **1.501 consultas adicionais**, além da fonte e das causas. Ativos/afastados pulam as consultas individuais. (`app/folha/retrato_folha.py:1216`; `app/folha/retrato_folha.py:1217`; `app/folha/export.py:1226`; `app/folha/export.py:1243`; `app/folha/export.py:1247`; `app/folha/export.py:1253`; `app/escala/alimentacao.py:186`; `app/escala/alimentacao.py:148`)

**O teste “UMA consulta” não comprova isso:** substitui o helper inteiro por mock e verifica uma chamada Python. Não mede SQL nem detecta seu N+1 interno. Latência real: **[nao verificado]**. (`app/folha/tests/test_retrato_folha_quadros.py:1345`; `app/folha/tests/test_retrato_folha_quadros.py:1349`; `app/folha/tests/test_retrato_folha_quadros.py:1350`)

O drawer mostra o bloco somente quando `informativo` contém dias. (`app/templates/folha/_retrato_o_que_fazer.html:56`; `app/templates/folha/_retrato_o_que_fazer.html:60`)

**4. Outros leitores: não encontrei quebra pela chave nova nos caminhos abaixo.**

| Leitor fora do delta | Comportamento |
|---|---|
| `fora_da_folha` | Extrai chaves nomeadas; não interpreta o dict inteiro como ids. (`app/folha/fora_da_folha.py:168`; `app/folha/fora_da_folha.py:170`) |
| `linhas_alcance` | Formata campos e subdicts específicos. (`app/chamados/services/vigia_fio.py:708`) |
| Backfill E4B | Consome `alcance_dias` e contadores específicos. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:239`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:412`) |
| `fio_estado` / `gerar_estado` | Iteram os esperados do carimbo e delegam apresentação; não somam todos os valores. (`bin/fio_estado.py:125`; `bin/gerar_estado.py:260`) |
| `pipeline_placar` | Lê métricas por nome; suas chamadas contemplam `fio_mudo_alvos` e `nunca_bateu_sem_adesao`, ignorando a nova. (`app/core/management/commands/pipeline_placar.py:92`; `app/core/management/commands/pipeline_placar.py:135`) |

As comparações integrais de `censo` e `esperado` identificadas no teste do vigia receberam a chave. Os testes de conservação de dias somam parcelas explícitas ou o subdict de rotas, sem incorporar o contador novo. (`app/chamados/tests/test_o261_vigia_fio.py:815`; `app/chamados/tests/test_o261_vigia_fio.py:828`; `app/chamados/tests/test_o261e_f5_alcance.py:104`; `app/chamados/tests/test_o261e_f5_alcance.py:392`)

**5. Classes de reprovação presentes no delta.**

- **Relógio fora da borda: presente no novo caminho.** O helper chama a autoridade sem `hoje`; o laço lê `localdate()` por desligado. O relógio já existia, mas o delta introduz esse novo consumidor sem transportar o instante capturado pelo lavrador. (`app/folha/retrato_folha.py:1219`; `app/folha/export.py:1238`; `app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/management/commands/lavrar_retrato_folha.py:65`)
- **Selo sem mordida para a propriedade anunciada: presente.** A asserção denominada “UMA consulta” testa apenas a chamada ao mock, deixando passar o N+1. Os testes funcionais de informativo e contador têm asserções próprias úteis. (`app/folha/tests/test_retrato_folha_quadros.py:1345`; `app/folha/tests/test_retrato_folha_quadros.py:1350`; `app/folha/tests/test_lei_desligado_nao_trava.py:117`; `app/folha/tests/test_lei_desligado_nao_trava.py:143`)
- **Segundo juiz, escritor fora da porta ou derivação paralela novos: não identificados nesses acréscimos.** Eles delegam aos juízes existentes, e a persistência permanece no lavrador. (`app/folha/fora_da_folha.py:143`; `app/folha/retrato_folha.py:1208`; `app/folha/retrato_folha.py:1219`; `app/folha/management/commands/lavrar_retrato_folha.py:74`)
- **Campo com dois significados:** há mudança explícita de lista de ids para contagem agregada entre funções, sem leitor incompatível encontrado; não deve ser apresentado como contagem de dias ou pessoas únicas. (`app/folha/fora_da_folha.py:146`; `app/chamados/services/vigia_fio.py:587`)

**6. Descartes da triagem.**

- **(3): descarte coletivo incorreto.** O Raio-X seleciona pessoas pelo juiz, mas recorta a demissão e reconstrói seus dias com `furos_por_dia`. Isso contraria a exigência de nenhum leitor de retido derivar por conta própria. O preview também usa quantidade de **motivos distintos** no lugar de dias. Esses problemas sobrevivem ao delta. (`app/folha/views.py:591`; `app/folha/views.py:612`; `app/folha/views.py:616`; `app/folha/views.py:398`; `app/folha/export.py:1256`; `app/folha/export.py:1298`)
- **`fechamento_lista`, separadamente:** lê a classificação e traduz o rótulo, preservando `motivo_juiz`; não constitui outra decisão de retenção. Seu fallback ambíguo para `furo_espelho` continua preexistente. (`app/ponto/services/fechamento_lista.py:34`; `app/ponto/services/fechamento_lista.py:36`)
- **(2): distinção técnica sustentada, cumprimento universal não demonstrado.** `turno_aberto` ainda impede aprovação/carimbo, mas a geração permite que a linha siga no TXT. Isso não prova retenção do TXT; também não elimina a ressalva sobre “nunca como trava”. Revogação de todo crivo de aprovação pelo aval: **[nao verificado]**. (`app/folha/export.py:833`; `app/ponto/views.py:1509`; `app/folha/views.py:216`; `app/folha/views.py:244`)
- **(3d): rótulo amplo confirmado.** A API chama todo `fora` de `retidos`, inclusive rescisão; lê o juiz e não cria retenção adicional. (`app/api/views_mensageria.py:283`; `app/api/views_mensageria.py:301`; `app/folha/export.py:866`)
- **(4): risco preexistente persiste.** O POST busca por pk e grava `afastado` pela porta, fazendo o juiz deixar de reconhecer desligamento. “Proibido tocar afastado” não demonstra que preservar um desligado nessa entrada seja proibido. Entretanto, após a troca, a pessoa sai da unidade literal por situação atual; não uso isso isoladamente como nova violação introduzida pelo delta. Ocorrência produtiva: **[nao verificado]**. (`app/ponto/views.py:2406`; `app/ponto/views.py:2435`; `app/colaboradores/services/desligamento.py:18`; `app/colaboradores/services/desligamento.py:24`)

**Veredito: REPROVA.** Bloqueantes: novo caminho com relógio interno; N+1 acompanhado de teste que anuncia uma garantia de consultas que não verifica; e descarte do censo apesar da derivação paralela sobrevivente no Raio-X. (`app/folha/retrato_folha.py:1219`; `app/folha/export.py:1238`; `app/folha/tests/test_retrato_folha_quadros.py:1350`; `app/folha/views.py:616`)

Suíte integral, opus, pouso e medição produtiva de competência 11 com contador zero: **[nao verificado]**. [nao verificado]

## 13/11 14:03 · sessao 3167efda7c55 · - · - · origem=Ronald (tmux)

**2. Escritores, leitores e unidade**

| Etapa | Referência | Significado |
|---|---|---|
| Seleção dos retidos por furo | `app/folha/fora_da_folha.py:119-131` | Usa resultado de `classificar_export`; carrega colaboradores por ids de `furo_espelho`. |
| Produção de `desligados_retidos` | `app/folha/fora_da_folha.py:143-146` | **Lista de ids únicos por empresa × competência**, filtrados pelo juiz `esta_desligado`. O dict `_colabs` elimina repetição do mesmo id nessa chamada. |
| Agregação | `app/chamados/services/vigia_fio.py:583-589` | **Inteiro: soma das quantidades por empresa × competência**. Mesmo id em duas competências soma duas vezes; não há deduplicação global. |
| Dias, separadamente | `app/chamados/services/vigia_fio.py:590-593` | Deduplicados por `(colab_id, data)`; não alimentam o contador novo. |
| Impressão do vigia | `app/chamados/management/commands/vigiar_fio_morto.py:212-213` | Imprime inteiro e esperado 0. |
| Publicação | `app/chamados/management/commands/vigiar_fio_morto.py:237-248` | Grava escalar no `censo`, esperado 0 e bloco `alcance`. |
| Escrita física do carimbo | `app/chamados/management/commands/vigiar_fio_morto.py:120-139` | `_carimbar` escreve JSON e substitui arquivo. |
| Leitura pelo ESTADO | `bin/fio_estado.py:122-126`; `bin/fio_estado.py:191-194`; `bin/gerar_estado.py:260-261` | Compara campos declarados em `esperado` e apresenta divergências. |

**Não há leitor encontrado que conte dias como pessoas. Há repetição intencional do id entre competências**, pois a soma representa ocorrências de colaborador por empresa × competência, não pessoas únicas na frota. (`app/chamados/services/vigia_fio.py:583-593`)

**Ressalva importante do achado (0): zero não aparece nominalmente na linha publicada do ESTADO.** Apenas valores divergentes entram em `fora_do_esperado` e são exibidos; `desligados_retidos=0` fica no carimbo, sem exposição explícita nessa linha. (`bin/fio_estado.py:125-126`; `bin/fio_estado.py:191-194`)

Conferido executando somente em memória as funções reais de `bin/fio_estado.py`: carimbo sintético com 0 produz `fora={}` e não mostra o nome; com 1 mostra `desligados_retidos` como divergência. É confirmação do comportamento do leitor, não medição produtiva. (`bin/fio_estado.py:121`; `bin/fio_estado.py:165`; produção: `[nao verificado]`)

**4. Leitores fora do diff**

Não encontrei quebra por chave nova nestes leitores: cada um acessa subcampos nomeados, sem interpretar todas as chaves como ids ou somar todos os valores. (`app/folha/fora_da_folha.py:168-171`; `app/chamados/services/vigia_fio.py:708-728`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78-79`; `bin/fio_estado.py:122-126`)

| Leitor | Comportamento relevante |
|---|---|
| `fora_da_folha` | Extrai exclusivamente as chaves antigas; não repassa a chave nova. (`app/folha/fora_da_folha.py:168-171`; `app/folha/fora_da_folha.py:201-207`) |
| `linhas_alcance` | Formata campos explícitos; itera somente subdicts de detalhe/papel. (`app/chamados/services/vigia_fio.py:708-728`) |
| Backfill | Filtra `alcance_dias`; consulta universo e fio morto explicitamente. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78-79`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:239`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:412`) |
| `fio_estado` | Valida esquema por campos; itera `esperado.items()`, aceitando a chave nova. (`bin/fio_estado.py:79-94`; `bin/fio_estado.py:122-126`) |
| `gerar_estado` | Delega leitura e composição. (`bin/gerar_estado.py:260-261`) |
| **`pipeline_placar`**, leitor adicional do mesmo carimbo | Lê `registro['censo'][nome]`/`esperado[nome]`; os chamados ativos são somente `fio_mudo_alvos` e `nunca_bateu_sem_adesao`. Portanto ignora o contador novo e não quebra. (`app/core/management/commands/pipeline_placar.py:92-106`; `app/core/management/commands/pipeline_placar.py:135-136`) |

Nos testes, os únicos comparadores de dict inteiro identificados para `reg['censo']` e `reg['esperado']` já ganharam a chave no delta. (`app/chamados/tests/test_o261_vigia_fio.py:815-832`)

Os testes de soma usam parcelas explícitas ou subdicts, mantendo o contador novo fora das contas de dias. (`app/chamados/tests/test_o261e_f5_alcance.py:104-111`; `app/chamados/tests/test_o261e_f5_alcance.py:392`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:194-197`)

Os demais consumidores diretos de testes acessam campos específicos. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:77-79`; `app/chamados/tests/test_n1_alimentacao_unica.py:99-119`; `app/chamados/tests/test_e4_passivo.py:110-119`; `app/chamados/tests/test_e4_backfill.py:162`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:95-98`; `app/chamados/tests/test_o261_resposta_vazia.py:168-170`)

A fixture antiga do selo shell não inclui `desligados_retidos`; isso não a quebra, pois o leitor compara apenas os esperados existentes na própria fixture. Também não comprova cobertura específica do contador novo no ESTADO. (`bin/tests/test_fio_estado.sh:48-60`; `bin/tests/test_fio_estado.sh:74`; `bin/fio_estado.py:125-126`)

**Teste do contador que morde:** exige vazio no caminho normal e `[self.d.pk]` quando o export é mutado para reter desligado e ativo por furo; portanto não é um contador literal 0. (`app/folha/tests/test_lei_desligado_nao_trava.py:136-143`)

**Limite da cobertura:** teste do carimbo usa o valor normalmente devolvido pelo censo e compara esperado 0; não há, nesse trecho, cenário de `desligados_retidos > 0` atravessando censo → carimbo → ESTADO. (`app/chamados/tests/test_o261_vigia_fio.py:815-832`; execução integral: `[nao verificado]`)

## 13/11 14:02 · sessao c54425f805df · - · - · origem=Ronald (tmux)

**(3) O descarte coletivo contradiz a cláusula de fonte única do aval.** O Raio-X lê do juiz quem está em `furo_espelho`, mas deriva suas próprias datas: recorta por demissão e chama `furos_por_dia`, que retorna marcos apagados da ata, não dias do retentor. (`app/folha/views.py:591`; `app/folha/views.py:612`; `app/folha/views.py:616`; `app/relatorios/furos.py:186`; `app/relatorios/furos.py:202`)

O preview também não apresenta uma contagem de dias da autoridade: chama `len(motivos_espelho)` de `_dias` e imprime esse número; a autoridade acumula motivos em `set` e devolve motivos distintos. (`app/folha/views.py:398`; `app/folha/views.py:402`; `app/folha/export.py:1256`; `app/folha/export.py:1298`)

`fechamento_lista` é diferente: lê o resultado e traduz o rótulo, preservando `motivo_juiz`; não decide novamente quem retém. O fallback chama outros motivos de `furo_espelho`, o que é ambiguidade de apresentação preexistente. (`app/ponto/services/fechamento_lista.py:25`; `app/ponto/services/fechamento_lista.py:34`; `app/ponto/services/fechamento_lista.py:36`)

**(2) O descarte não demonstra cumprimento da frase “nunca como trava”.** `turno_aberto` continua sendo emitido para desligados e recusa aprovação individual, lote e carimbo automático. (`app/folha/export.py:833`; `app/folha/export.py:848`; `app/ponto/views.py:1509`; `app/ponto/views.py:1308`; `app/folha/views.py:244`)

Entretanto, isso não prova bloqueio de entrada no TXT: a geração separa `status='entra'` do carimbo e documenta que quem tem trava segue no TXT sem aprovação automática. É caminho preexistente fora do delta; interpretar o aval como revogação de todo crivo de aprovação exige assumir alcance adicional. (`app/folha/views.py:215`; `app/folha/views.py:216`; alcance adicional: `[nao verificado]`)

**(3d) O problema de rótulo existe, mas não é nova retenção pelo delta.** A API conta todo `status='fora'` como `retidos`, incluindo saída por rescisão; ela lê o juiz e não escreve ou recalcula o retentor. (`app/api/views_mensageria.py:283`; `app/api/views_mensageria.py:301`; `app/folha/export.py:866`)

**(4) O contraexemplo estático persiste; “PROIBIDO tocar afastado” não prova que preservar desligado seja proibido.** A lista da tela só oferece ativos/afastados, mas o POST busca qualquer `pk` e, para INSS, grava `afastado` pela porta legítima. (`app/ponto/views.py:2372`; `app/ponto/views.py:2406`; `app/ponto/views.py:2433`; `app/colaboradores/services/colaborador.py:55`)

Depois dessa escrita, `esta_desligado` devolve falso e o retentor deixa de aplicar a saída antecipada. Contudo, a unidade literal é a situação atual: após a troca já não é `situacao='desligado'`. Portanto, há risco preexistente e descarte insuficientemente fundamentado, mas não prova autônoma de violação da lei para alguém que ainda esteja na unidade definida. (`app/colaboradores/services/desligamento.py:18`; `app/colaboradores/services/desligamento.py:24`; `app/folha/export.py:1207`; execução produtiva: `[nao verificado]`)

**Para consolidar:** o bloqueante mais objetivo nos descartes é declarar limpo o censo apesar da derivação paralela do Raio-X; preview confunde motivos com dias. Esses caminhos não foram curados pelo delta fornecido. (`app/folha/views.py:616`; `app/folha/views.py:398`; presença da cura no delta: `[nao verificado]`)

## 13/11 14:02 · sessao dbda1d79cea4 · BUG-PAR · verificador · origem=Code

**BLOQUEANTE: o passivo já absolvido continua fora da recuperação, e a tela pode prometer um par que o clique recusa.** Fontes: `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:192`, `app/chamados/services/materializacao.py:566`, `app/chamados/services/validacao.py:391`.

1. **BLOQUEANTE — pergunta absolvida por paridade permanece invisível em quatro entradas.**

   Contraexemplo: saída S01:00 respondida, com `validada_em` e `materializada_em` preenchidos por `VIA_PARIDADE_RECUSADA`, sem batida criada; depois chega a resposta da volta E02:00. Essa absolvição sem batida existe expressamente em `app/chamados/services/materializacao.py:1010` e `app/chamados/services/materializacao.py:1050`.

   As quatro entradas que não a recuperam são:

   | Entrada | Exclusão |
   |---|---|
   | Contador | Exige `materializada_em IS NULL`: `app/chamados/services/materializacao.py:191`. |
   | Clique na própria saída | Retorna `ja_validada`: `app/chamados/services/validacao.py:433`. Se a validação estiver nula, ainda pula a materialização pelo carimbo preenchido: `app/chamados/services/validacao.py:505`. |
   | Clique na volta | A seleção da irmã exclui a saída carimbada: `app/chamados/services/materializacao.py:136`; `_validar_o_par` depende dessa seleção: `app/chamados/services/validacao.py:390`. |
   | Laço de materialização da disputa | Seleciona somente perguntas sem carimbo: `app/chamados/services/materializacao.py:1531`. |

   São **quatro entradas, não quatro causas independentes**. `_gravar_irma_do_par` compartilha a seleção da irmã; a devolução também exige carimbo nulo, portanto não constituem quinta e sexta entradas: `app/chamados/services/materializacao.py:1250`, `app/chamados/services/validacao.py:192`. Quantas das 60 respostas pertencem a essa classe: **[nao verificado]**.

2. **BLOQUEANTE — tela promete aplicação com irmã implausível; clique recusa.**

   Contraexemplo: saída respondida 01:00; volta respondida **14:00**, com marco fixo **02:00**, ainda sem validação. A projeção usa 02:00, porque o plantio de intervalo substitui a hora respondida pela hora do marco; a tela encontra o par válido e retorna `aplicavel`: `app/chamados/services/materializacao.py:511`, `app/chamados/services/materializacao.py:563`.

   O clique consulta a implausibilidade da resposta **14:00 contra 02:00**, abandona o par e tenta a saída isoladamente: `app/chamados/services/validacao.py:390`, `app/chamados/services/validacao.py:508`. O juiz detecta a diferença circular de 720 minutos: `app/chamados/juizes.py:1799`, `app/chamados/juizes.py:2016`. A testemunha, portanto, não representa todos os impedimentos da porta: `app/chamados/services/materializacao.py:143`, `app/chamados/services/validacao.py:391`.

3. **BLOQUEANTE — igualdade de `data_evento` não garante o mesmo dia do chamado.**

   Contraexemplo: as duas perguntas têm `data_evento=25/09`, mas o chamado que lista a volta aponta 26/09, sem `data_conferida_em`. A seleção aceita a irmã pela igualdade entre os campos das perguntas; a projeção não consulta a divergência com o chamado, e a tela promete o par: `app/chamados/services/materializacao.py:136`, `app/chamados/services/materializacao.py:143`, `app/chamados/services/materializacao.py:566`.

   Na escrita da irmã, `_materializar_uma` consulta `data_do_fato_de`, encontra a divergência e retorna sem materializar, com `DATA_EM_DISPUTA`: `app/chamados/services/materializacao.py:1353`. A comparação entre dia da pergunta e dia do chamado está em `app/chamados/services/fio.py:85`.

   **NAO-BLOQ para os outros atributos isoladamente:** irmã com outro `data_evento`, já materializada ou retratada fica excluída da projeção. Os filtros estão em `app/chamados/services/materializacao.py:137` e `app/chamados/services/materializacao.py:139`; `fora_do_fato` reconhece a via terminal em `app/chamados/models.py:3227`. Não há contraexemplo demonstrado desses três atributos, por si sós, produzindo promessa indevida de par: **[nao verificado]**.

4. **BLOQUEANTE — o aviso “colada” continua descrevendo distância de horas.**

   Contraexemplo já presente no teste: S01:00 isolada recusa pela S06:30, diferença de **5h30**; o teste exige recusa, mas não exige “falta a outra metade”: `app/chamados/tests/test_bug_par_projetado.py:151`.

   Toda recusa classificada como PARIDADE continua recebendo “marcação […] colada nesse horário”, embora a guarda procure vizinhas até 12 horas: `app/chamados/catalogo/recusa.py:26`, `app/chamados/services/materializacao.py:100`. O requisito textual do aval de dois atos não está realizado nesse caminho: `app/chamados/services/acoes_disputa.py:33`.

5. **BLOQUEANTE — o lote pode processar novamente a irmã que o par acabou de gravar.**

   Contraexemplo: o grupo do lote contém saída e volta já validadas. Ao processar a saída, `_gravar_irma_do_par` materializa primeiro a volta; depois o lote alcança o objeto antigo da volta e chama `_materializar_uma` novamente: `app/chamados/services/materializacao.py:1258`, `app/chamados/services/passivo_validado.py:520`.

   A segunda passagem pode reutilizar a batida, registrar outra auditoria e renovar o carimbo: `app/chamados/services/materializacao.py:980`, `app/chamados/services/materializacao.py:1077`, `app/chamados/services/materializacao.py:1439`. `carimbar` escolhe novo instante e salva novamente: `app/chamados/models.py:3150`, `app/chamados/models.py:3178`.

   O diff protegeu o laço da disputa contra exatamente essa repetição, mas essa proteção não está no laço do passivo: `app/chamados/services/materializacao.py:1538`, `app/chamados/services/passivo_validado.py:522`. Reprodução executada e quantidade de eventos adicionais: **[nao verificado]**.

6. **BLOQUEANTE — a ordem de trava não é uniforme com o lote; há interleaving de deadlock.**

   O admin toma Colaborador antes de validar e materializar; os chamados da irmã entram no conjunto ordenado de travas: `app/chamados/services/validacao.py:465`, `app/chamados/services/validacao.py:474`. A autoridade ordena Colaborador → Disputa → Chamado → Pergunta, com pks crescentes: `app/chamados/models.py:2650`, `app/chamados/models.py:2687`, `app/chamados/models.py:2709`.

   O lote entra diretamente na materialização sem essa trava inicial: `app/chamados/services/passivo_validado.py:374`, `app/chamados/services/passivo_validado.py:522`. A criação da batida dispara o núcleo, que apaga/recria `TurnoMaterializado`; depois o fechamento do chamado pode tomar Colaborador: `app/ponto/registro_batida.py:157`, `app/ponto/signals.py:34`, `app/ponto/nucleo.py:57`, `app/chamados/models.py:712`.

   Contraexemplo de concorrência: lote segura uma linha de `TurnoMaterializado`; admin segura Colaborador e espera aquela linha; lote tenta tomar Colaborador no fechamento. A inversão é estruturalmente possível nesses caminhos; ocorrência real e introdução histórica: **[nao verificado]**.

7. **NAO-BLOQ para rollback de banco demonstrável; notificação externa permanece uma lacuna.**

   No caso em que a saída é validada primeiro, a volta E02:00 grava antes da saída S01:00; existe, portanto, o estado intermediário E18:30–E02:00–S06:30. Núcleo, cartório e reconciliador executam entre as duas gravações: `app/chamados/services/materializacao.py:996`, `app/chamados/services/materializacao.py:1258`, `app/ponto/signals.py:34`, `app/ponto/signals.py:131`, `app/ponto/signals.py:206`.

   As escritas de banco estão dentro dos atomics; o outbox da pergunta é síncrono, os becos usam `on_commit`, e o realtime usa `pg_notify` na mesma conexão. Não encontrei nesses caminhos prova de rastro de banco sobrevivendo ao rollback do par: `app/chamados/services/validacao.py:395`, `app/chamados/services/materializacao.py:1255`, `app/chamados/signals.py:240`, `app/chamados/signals.py:259`, `app/ponto/realtime.py:40`.

   Existe, porém, caminho cartório → emissão de cobrança → push imediato → `webpush`, sem adiamento até commit nesses pontos: `app/ponto/services/cartorio.py:1829`, `app/ponto/management/commands/detectar_ausencias.py:777`, `app/chamados/push_utils.py:40`. Que o estado intermediário do caso fornecido efetivamente alcance essa emissão: **[nao verificado]**. O teste novo confere contagem de chamados/perguntas depois do sucesso; não prova ausência de push nem rollback após a primeira batida: `app/chamados/tests/test_bug_par_projetado.py:113`.

8. **BLOQUEANTE — duas entradas novas da allowlist ocultam o defeito de universo.**

   As exceções de `_gravar_irma_do_par` e `_validar_o_par` correspondem a guardas de escrita admitidas pelo próprio contrato: `app/chamados/tests/test_contract_leitores_vitalidade.py:8`, `app/chamados/tests/test_contract_leitores_vitalidade.py:108`, `app/chamados/tests/test_contract_leitores_vitalidade.py:113`.

   Já `metade_irma_do_intervalo` e o contador eliminam o passivo carimbado **antes** de consultar a autoridade. As duas exceções autorizam esses leitores, e o selo apenas remove endereços allowlisted da lista de ofensores; ele não verifica o significado do filtro: `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:192`, `app/chamados/tests/test_contract_leitores_vitalidade.py:109`, `app/chamados/tests/test_contract_leitores_vitalidade.py:331`.

9. **BLOQUEANTE de certificação — os dois ramos defensivos pedidos não estão exercitados pelos cenários novos.**

   Faltam cenários que forcem a rejeição por `not t_s < t_e` e a rechecagem ofensora depois de gravar a irmã: `app/chamados/services/materializacao.py:154`, `app/chamados/services/materializacao.py:997`. A fixture fixa os marcos em 01:00/02:00; os adversariais acrescentam vizinha ofensora ou retiram validação: `app/chamados/tests/test_bug_par_projetado.py:61`, `app/chamados/tests/test_bug_par_projetado.py:162`, `app/chamados/tests/test_bug_par_projetado.py:214`.

   Contraexemplos de certificação necessários: ordem projetada inválida deve impedir escrita; reprovação depois da primeira batida deve desfazer ambas, carimbos e efeitos transacionais. Execução dos mutantes, RED na base, GREEN e suite: **[nao verificado]**. [nao verificado]

O mapa de escrita/leitura dos campos centrais é este:

| Campo/valor | Quem escreve | Quem lê no percurso examinado |
|---|---|---|
| `validada_em` | `_carimbar_validacao`; devolução limpa; resposta nova limpa; carimbo simétrico também preenche. `app/chamados/services/validacao.py:136`, `app/chamados/services/validacao.py:201`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/models.py:3160`. | Guarda inicial, validação da irmã, materialização da irmã e laço. `app/chamados/services/validacao.py:433`, `app/chamados/services/validacao.py:391`, `app/chamados/services/materializacao.py:1251`, `app/chamados/services/materializacao.py:1533`. |
| `materializada_em`, autor e via | Atribuição canônica em `_escrever_carimbo`; persistência pelas portas `carimbar`/`carimbar_identidade`. `app/chamados/models.py:3123`, `app/chamados/models.py:3178`, `app/chamados/models.py:3216`. | Seleção da irmã, contador, tela, prova do par e laço. `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:192`, `app/chamados/services/materializacao.py:542`, `app/chamados/services/validacao.py:399`, `app/chamados/services/materializacao.py:1534`. |
| `VIA_PARIDADE_RECUSADA` | Ramo de recusa chama `carimbar_identidade`. `app/chamados/services/materializacao.py:1050`. | Identidade/vitalidade e testemunha da aplicação. `app/chamados/models.py:2531`, `app/chamados/models.py:3118`, `app/chamados/services/materializacao.py:546`. |
| Resposta e `respondida_em` | Escritor da resposta do colaborador. `app/chamados/services/disputa_emissao.py:1467`. | Seleção da irmã, contador e derivação do plantio. `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:199`, `app/chamados/services/materializacao.py:461`. |
| Motivo, tipo e dia da pergunta | Emissão; conferência pode alterar o dia. `app/chamados/services/disputa_emissao.py:834`, `app/chamados/services/validacao.py:664`. | Identificação da irmã e derivação compartilhada. `app/chamados/services/materializacao.py:133`, `app/chamados/services/materializacao.py:435`. |
| `recusa_motivo` / `pendencia_admin` | Porta da recusa e ramo de pendência. `app/chamados/services/materializacao.py:1209`, `app/chamados/services/materializacao.py:965`. | Contador e elegibilidade do par. `app/chamados/services/materializacao.py:193`, `app/chamados/services/materializacao.py:148`. |
| `estado_aplicacao_pergunta` | Retorno derivado, sem campo persistido. `app/chamados/services/materializacao.py:533`. | Modal/fio, contador e explicador. `app/chamados/services/fio.py:373`, `app/chamados/services/materializacao.py:201`, `app/chamados/services/explicador.py:100`. |
| `gravou` | Explicador acrescenta somente `False` para a recusa reconhecida. `app/chamados/services/explicador.py:101`. | Desenlace conta falsos; ausência deles mantém “Tudo validado e gravado”. `app/chamados/services/desenlace.py:65`. |
| `msg` | Service de validação. `app/chamados/services/validacao.py:563`. | Casca repassa, acrescentando avisos de irmãs. `app/chamados/services/acoes_disputa.py:62`. |
| Projeção `(ts, tipo)` | Contexto temporário definido/resetado ao materializar a irmã. `app/chamados/services/materializacao.py:1256`. | Guarda de paridade. `app/chamados/services/materializacao.py:990`. |

**O campo com dois significados continua sendo `materializada_em`:** sua descrição diz “batida criada + chamado fechado”, mas a recusa o preenche sem criar batida. O diff trata essa diferença na apresentação, porém continua usando o carimbo como exclusão do universo e como prova de conclusão: `app/chamados/models.py:3445`, `app/chamados/services/materializacao.py:1050`, `app/chamados/services/materializacao.py:192`, `app/chamados/services/validacao.py:399`.

Quanto aos avais e proibições:

| Item | Resultado da pré-verificação |
|---|---|
| Par projetado pelo escritor existente | Implementado no percurso novo, com as falhas acima. `app/chamados/services/materializacao.py:159`, `app/chamados/services/materializacao.py:1258`. |
| Paridade preservada; isolada continua recusada | Preservada no código e exigida pelo teste. `app/chamados/services/materializacao.py:1003`, `app/chamados/tests/test_bug_par_projetado.py:151`. |
| Não aceitar furo da véspera | A consulta continua restrita ao dia local do plantio. `app/chamados/services/materializacao.py:273`. |
| Central/modal não mentir | Correção parcial: explicador/desenlace reconhecem a absolvição sem batida; previsão de aplicação ainda diverge do clique. `app/chamados/services/explicador.py:101`, `app/chamados/services/desenlace.py:65`, `app/chamados/services/validacao.py:391`. |
| As 60 voltarem ao admin sem nova resposta | Não demonstrado; há exclusões estruturais do passivo. `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:192`. Resultado sobre as 60: **[nao verificado]**. |
| Contador 60 → 0, restantes legítimas listadas | O teste cobre recusa sem carimbo, não o passivo já absolvido. `app/chamados/tests/test_bug_par_projetado.py:181`. Medição da frota: **[nao verificado]**. |
| RED do caso e contraexemplo isolado | Cenários sintéticos presentes. `app/chamados/tests/test_bug_par_projetado.py:113`, `app/chamados/tests/test_bug_par_projetado.py:151`. Execução: **[nao verificado]**. |
| História da recusa, censo por competência/VALIDAR/DECIDIR, DRY integral, matriz e reaproveitamento dos mutantes | **[nao verificado]**. |
| Cópia nova sobre E3, ancestralidade, suite após E3, opus e pouso | **[nao verificado]**. | [nao verificado]
| Teto de produção | O diff fornecido toca cinco arquivos de produção: `app/chamados/services/acoes_disputa.py:62`, `app/chamados/services/desenlace.py:65`, `app/chamados/services/explicador.py:100`, `app/chamados/services/materializacao.py:86`, `app/chamados/services/validacao.py:379`. |
| Segundo motor ou alteração da resposta do colaborador | Não aparecem no percurso acrescentado; ele carimba validação e delega ao materializador. `app/chamados/services/validacao.py:397`, `app/chamados/services/materializacao.py:1258`. |

**NAO-BLOQ quanto a segundo escritor ou juiz duplicado demonstrado.** As duas funções novas delegam a `_materializar_uma`; o plantio efetivo continua em `registrar_batida`, e a vinculação passa por `anotar`: `app/chamados/services/validacao.py:398`, `app/chamados/services/materializacao.py:1258`, `app/chamados/services/materializacao.py:1052`, `app/chamados/services/materializacao.py:1073`. A guarda retroativa de vizinhança não é, pelo contrato examinado, a mesma pergunta do tipo sugerido por `proximo_tipo_de`: `app/chamados/services/materializacao.py:86`, `app/core/juizes.py:213`.

**O padrão Single Writer Principle é realizado na escrita da batida, mas não certifica a obra inteira:** o glossário o associa ao chokepoint existente; reutilizá-lo não elimina seleção incompleta, divergência tela/clique ou repetição do lote. Fontes: `app/docs/GLOSSARIO.md:12`, `app/chamados/services/materializacao.py:1052`, `app/chamados/services/materializacao.py:138`, `app/chamados/services/validacao.py:391`, `app/chamados/services/passivo_validado.py:522`. Relógio novo fora da borda ou nova derivação paralela não foram demonstrados nas funções acrescentadas, que reutilizam `_ts_tipo_do_plantio`: `app/chamados/services/materializacao.py:150`, `app/chamados/services/materializacao.py:172`.

## 13/11 14:02 · sessao 8555b616ccb4 · - · - · origem=Ronald (tmux)

- **Achado (1) curado no código para retratos novos/relavrados:** `montar_linhas` obtém o informativo e grava chave própria; a porta de gravação segue o lavrador existente. (`app/folha/retrato_folha.py:1161`; `app/folha/retrato_folha.py:1193`; `app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/management/commands/lavrar_retrato_folha.py:74`)
- **Testes que mordem:** o teste da lavra exige três datas no desligado e vazio no ativo/afastado; o teste do drawer exige rótulo e datas, e ausência do rótulo com lista vazia/chave ausente. (`app/folha/tests/test_lei_desligado_nao_trava.py:106`; `app/folha/tests/test_lei_desligado_nao_trava.py:117`; `app/folha/tests/test_lei_desligado_nao_trava.py:122`; `app/folha/tests/test_lei_desligado_nao_trava.py:131`)
- **Leitura da autoridade, sem derivação paralela:** o helper pergunta a `esta_desligado`, chama `folha.export.informativo_rescisao` e transforma as datas em strings; janela vem de `janela_fechamento`. A autoridade chama o mesmo `_dias_sem_decisao` do retentor. (`app/folha/retrato_folha.py:1208`; `app/folha/retrato_folha.py:1214`; `app/folha/retrato_folha.py:1217`; `app/folha/retrato_folha.py:1219`; `app/folha/export.py:1212`; `app/folha/export.py:1228`)
- **N+1 real:** custo incremental do helper = uma consulta de colaboradores mais duas por desligado não isento cuja janela não colapse: células + vínculos. Para 750 desses desligados, 1.501 consultas, além do custo anterior de montar a fonte e causas. Ativos/afastados não fazem as duas consultas individuais. (`app/folha/retrato_folha.py:1216`; `app/folha/retrato_folha.py:1217`; `app/folha/export.py:1226`; `app/folha/export.py:1243`; `app/folha/export.py:1247`; `app/folha/export.py:1253`; `app/escala/alimentacao.py:186`; `app/escala/alimentacao.py:148`)
- **Selo de custo sem mordida:** `test_retrato_folha_quadros` substitui o helper inteiro por mock e conta uma chamada Python, embora o comentário/asserção a chamem de “UMA consulta”. Não mede SQL nem impede o N+1 interno. (`app/folha/tests/test_retrato_folha_quadros.py:1345`; `app/folha/tests/test_retrato_folha_quadros.py:1349`; `app/folha/tests/test_retrato_folha_quadros.py:1350`)
- **Drawer só mostra com dia:** a view entrega a linha gravada, o painel inclui o partial e o bloco tem `if retrato_linha.informativo`; sem competência, permissão `ver_folha`, retrato ou linha, não há esse caminho de apresentação. (`app/colaboradores/views.py:977`; `app/colaboradores/views.py:984`; `app/colaboradores/views.py:995`; `app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/folha/_retrato_o_que_fazer.html:56`; `app/templates/folha/_retrato_o_que_fazer.html:60`)
- **Classe relógio:** o delta abre novo caminho para relógio interno: helper chama a autoridade sem `hoje`; esta passa `None` ao laço, que lê `localdate()`. O relógio já existia; a chamada pelo novo informativo do retrato é nova. (`app/folha/retrato_folha.py:1219`; `app/folha/export.py:1228`; `app/folha/export.py:1238`)
- **Limite de implantação:** leitura continua usando o JSON gravado; retratos antigos sem a chave não ganham o informativo até relavra. Relavra produtiva e execução dos testes: `[nao verificado]`. (`app/folha/retrato_folha.py:110`; `app/folha/retrato_folha.py:1190`)

## 13/11 14:00 · sessao ceeeba5ede72 · HJ-090 · verificador · origem=Code

**Pré-verificação: REPROVADA.** Permanecem corridas de saldo, escritores fora das travas, reversão insegura e contador ausente do ESTADO (`app/ferias/services.py:233`, `app/ferias/services.py:321`, `app/ferias/management/commands/realinhar_concedidos_ferias.py:72`, `bin/gerar_estado.py:178`).

As conclusões abaixo são estáticas. Execução dos contraexemplos, RED/GREEN, aplicação em produção e ancestralidade do hash: **[não verificado]**. Os totais raspados não demonstram quais registros cada implementação efetivamente alcança **[não verificado]**.

1. **BLOQUEANTE — a trava evita perda da soma, mas não impede consumir saldo já gasto.** A tela valida saldo antes da porta; `executar_agendamento` relê o período travado, porém só revalida duplicata, sem repetir saldo/fracionamento (`app/ferias/views.py:297`, `app/ferias/services.py:434`, `app/ferias/services.py:435`). O caminho com abono também valida antes do `atomic`; a venda isolada apenas incrementa sob trava (`app/ferias/services.py:233`, `app/ferias/services.py:237`, `app/ferias/services.py:720`).

   **Contraexemplos:** com direito/saldo 30, agendamento de 30 e venda de 5 passam nas respectivas fotos; as escritas serializadas terminam com concedidos 30, vendidos 5 e saldo −5. Duas vendas de 6 podem passar pelo teto inicial 10 e terminar em vendidos 12. Os juízes existem, mas são consultados antes da trava (`app/ferias/views.py:241`, `app/ferias/views.py:962`, `app/ferias/views.py:964`, `app/ferias/models.py:217`, `app/ferias/models.py:235`).

2. **BLOQUEANTE — ainda há escritores de status do período sem releitura travada.** `sincronizar_status_periodo` calcula e salva usando o objeto recebido; `garantir_periodo_aquisitivo` também lê uma foto e depois faz `update` (`app/ferias/services.py:307`, `app/ferias/services.py:315`, `app/ferias/services.py:321`, `app/ferias/services.py:344`, `app/ferias/services.py:348`).

   **Contraexemplo:** alerta carrega período adquirido, saldo 30, limite vencido; outro pedido agenda 30 e grava `concedido`; o alerta grava `vencido`, calculado pela foto anterior. Esse escritor é alcançado pelo cron e pelas consultas de saldo/seção (`app/ferias/management/commands/alertar_ferias_vencendo.py:32`, `app/ferias/views.py:116`, `app/ferias/views.py:781`).

3. **BLOQUEANTE — Ausencia de férias tem edição e rejeição independentes do agendamento.** As telas genéricas carregam qualquer Ausencia por PK e podem rejeitá-la ou alterar tipo/datas, sem passar pelas portas travadas de férias (`app/ponto/views.py:2728`, `app/ponto/views.py:2734`, `app/ponto/views.py:2761`, `app/ponto/views.py:2794`, `app/ponto/services/ausencia.py:498`, `app/ponto/services/ausencia.py:836`).

   **Contraexemplo:** agendamento vivo de 30 dias, concedidos 30; rejeitar uma Ausencia deixa agendamento e espelho discordantes. Outra sincronização não repara essa rejeição: `espelhar_ferias` inclui qualquer status em `existentes` e pula datas existentes (`app/ponto/services/ausencia.py:1182`, `app/ponto/services/ausencia.py:1200`). Mover a Ausencia de 10/11 para 01/12 também mantém a janela original do agendamento (`app/ponto/services/ausencia.py:499`, `app/ferias/models.py:309`).

4. **BLOQUEANTE — exclusão confere agendamentos vivos fora da porta.** A view conta antes; o serviço depois apaga todos os agendamentos e o período, sem conferir novamente sob trava (`app/ferias/views.py:848`, `app/ferias/services.py:674`, `app/ferias/services.py:675`).

   **Contraexemplo:** a contagem retorna zero; outro pedido cria um agendamento; a exclusão seguinte pode apagar esse agendamento recém-criado. O caminho usa `QuerySet.delete`, em vez do método de instância que limpa as Ausencias (`app/ferias/services.py:675`, `app/ferias/models.py:453`). Resultado desse entrelaçamento com FKs e signals em execução: **[não verificado]**.

5. **BLOQUEANTE — snapshot, apply e reversão não identificam a mesma alteração com segurança.**

   - **Foto anterior à trava:** o command fotografa antes de a porta reler e recalcular (`app/ferias/management/commands/realinhar_concedidos_ferias.py:72`, `app/ferias/management/commands/realinhar_concedidos_ferias.py:81`, `app/ferias/services.py:144`). **Contraexemplo:** fotografa 30→33; cancelamento concorrente deixa concedidos −3/soma 0; apply faz −3→0; snapshot continua 30→33 e a reversão pula porque 0≠33 (`app/ferias/services.py:538`, `app/ferias/services.py:145`, `app/ferias/services.py:167`).
   - **Sobrescrita do arquivo:** nome distingue somente segundos e abre com `w`, inclusive sem períodos para aplicar (`app/ferias/management/commands/realinhar_concedidos_ferias.py:71`, `app/ferias/management/commands/realinhar_concedidos_ferias.py:75`). **Contraexemplo:** dois applies no mesmo segundo; o segundo, já alinhado, substitui a reversão útil por `periodos=[]` (`app/ferias/management/commands/realinhar_concedidos_ferias.py:74`, `app/ferias/management/commands/realinhar_concedidos_ferias.py:76`).
   - **Retorno ao mesmo número:** reversão compara apenas `dias_concedidos`, sem versão ou identidade dos agendamentos (`app/ferias/services.py:167`). **Contraexemplo:** realinha 30→33; cancela o original→0; registra outro histórico de 33→33; snapshot antigo restaura 30, reabrindo a divergência e o cancelamento seguinte em −3 (`app/ferias/services.py:538`, `app/ferias/services.py:416`, `app/ferias/services.py:170`).

6. **BLOQUEANTE — trilha obrigatória pode falhar e a mudança confirmar.** Realinhamento e reversão ignoram o retorno de `evento`; esse escritor captura exceções, desfaz somente a auditoria em savepoint e retorna `None` (`app/ferias/services.py:153`, `app/ferias/services.py:173`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`).

   **Contraexemplo:** falha em `LogAuditoria.objects.create`; concedidos muda de 30 para 33 sem a linha antes/depois exigida. O erro é registrado pelo mecanismo de observabilidade, mas não impede a alteração (`app/core/observ.py:90`, `app/core/observ.py:134`).

7. **BLOQUEANTE — contador existe, mas não é lido pelo ESTADO; o tripwire pode retornar OK com divergência.** Os dois sentidos existem em `ferias_concedidos_x_soma`; o command imprime ambos, mas os exclui do total do alarme (`app/ferias/services.py:116`, `app/ferias/services.py:125`, `app/ferias/management/commands/tripwire_status_ferias.py:30`, `app/ferias/management/commands/tripwire_status_ferias.py:35`). A composição do ESTADO não incorpora esse contador (`bin/gerar_estado.py:178`, `bin/gerar_estado.py:304`, `bin/gerar_estado.py:370`).

   **Contraexemplo:** concedidos 30, soma 33, status `concedido` coerente. O tripwire imprime `soma_maior=1`, mas pode retornar “OK”/rc 0 porque não existe divergência de status (`app/ferias/management/commands/tripwire_status_ferias.py:39`, `app/ferias/management/commands/tripwire_status_ferias.py:45`).

8. **BLOQUEANTE — a lista proposta para os 40 dias consulta outro universo.** O command reutiliza a folha, cuja fonte é a janela de **AgendamentoFerias**; isso não demonstra equivalência com os dias medidos por **Ausencia** (`app/ferias/management/commands/listar_ferias_com_batida.py:47`, `app/folha/export.py:472`, `app/folha/export.py:491`).

   **Contraexemplo:** Ausencia de férias aprovada + batida no dia, sem agendamento correspondente: a consulta por Ausencia encontra, a lista nova omite. O inverso também pode existir: a materialização admite manter agendamento com dias de Ausencia recusados e encaminhados ao DP (`app/ponto/services/ausencia.py:1203`, `app/ponto/services/ausencia.py:1210`, `app/ponto/services/ausencia.py:1217`). Alcançar exatamente 40 dias, 12 colaboradores e os quatro dias da competência 11: **[não verificado]**.

9. **BLOQUEANTE — encaminhamento do histórico futuro perde o preenchimento fora de HTMX; aceitação dos mesmos 32 não está comprovada.** Só a ramificação HX transmite `prefill`; a convencional volta ao saldo sem datas (`app/ferias/views.py:213`, `app/ferias/views.py:217`, `app/ferias/views.py:219`, `app/ferias/views.py:138`).

   **Contraexemplo:** POST convencional de histórico 01–30/12 recebe recusa e retorna ao saldo, sem abrir o agendamento preenchido; esse saldo apenas oferece o link (`app/templates/ferias/saldo.html:46`). O RED cobre um caso sintético HTMX; o teste convencional só verifica que não nasceu agendamento (`app/ferias/test_hj090_um_juiz.py:271`, `app/ferias/test_hj090_um_juiz.py:287`). Resultado da simulação dos mesmos 32, motivos das recusas e eventual linha `lei:`: **[não verificado]**.

10. **BLOQUEANTE estrutural — o cálculo do saldo ainda tem duas implementações.** O registro aponta `PeriodoAquisitivo.dias_saldo`, mas `status_de_periodo` repete direito−concedidos−vendidos em vez de consultar essa property (`app/core/juizes.py:264`, `app/ferias/models.py:95`, `app/ferias/models.py:217`). A catraca exclui o arquivo inteiro, permitindo esse clone interno (`app/core/juizes.py:1458`).

    **Contraexemplo de manutenção:** alterar o juiz registrado muda o saldo mostrado, enquanto o status continua julgando pela fórmula antiga. Divergência numérica atual entre essas duas fórmulas para os dados fornecidos: **[não verificado]**.

11. **NAO-BLOQ — relógios internos e um command escritor sem `--apply` permanecem.** Serviços ainda leem `date.today()` para aviso/carimbo; o histórico recebe `hoje`, mas calcula status sem repassá-lo (`app/ferias/services.py:432`, `app/ferias/services.py:544`, `app/ferias/services.py:571`, `app/ferias/services.py:399`).

    **Contraexemplo:** atravessar meia-noite entre a captura de `hoje` e `status_por_data` permite recusa e status julgados por dias diferentes (`app/ferias/services.py:391`, `app/ferias/models.py:280`). Fuso configurado é São Paulo; erro UTC/local efetivo em produção: **[não verificado]** (`app/config/settings/base.py:93`). `alertar_ferias_vencendo` também pode gravar status/direito sem flag apply (`app/ferias/management/commands/alertar_ferias_vencendo.py:14`, `app/ferias/management/commands/alertar_ferias_vencendo.py:32`, `app/ferias/services.py:321`).

12. **NAO-BLOQ — leitores ainda ocultam déficit ou misturam reserva e gozo.** A API recebe soma apenas dos saldos positivos e uma lista que exclui períodos negativos (`app/ferias/situacao.py:296`, `app/ferias/situacao.py:297`, `app/api/views_mensageria.py:835`). **Contraexemplo:** saldos −3 e 30 produzem `saldo_dias=30`, omitindo o primeiro período nessa lista (`app/api/views_mensageria.py:836`).

    Concedidos também aparece como “Gozo”/“dias gozados”, embora seja incrementado ao reservar férias futuras; período negativo com status concedido recebe “Nada pendente” (`app/templates/ferias/_secao_colab.html:44`, `app/templates/ferias/_secao_colab.html:49`, `app/ferias/situacao.py:179`, `app/ferias/services.py:443`). As duas telas principais, porém, imprimem o saldo negativo sem corte (`app/templates/ferias/_secao_colab.html:50`, `app/templates/ferias/saldo.html:41`).

A conferência individual dos avais fica assim:

| Item | Verificação |
|---|---|
| **1221(1): quatro escritores com trava e atomic** | **Cumprido nesses quatro corpos:** histórico, executar, trocar e aprovar; **não cumprido como “todo escritor”**, pelos achados 2–4 (`app/ferias/services.py:393`, `app/ferias/services.py:433`, `app/ferias/services.py:563`, `app/ferias/services.py:609`). |
| **1221(2): saldo desce para `_trocar_janela`** | **Cumprido no código:** consulta ao juiz ocorre após travar agendamento e período; views traduzem a exceção (`app/ferias/services.py:564`, `app/ferias/services.py:565`, `app/ferias/views.py:509`, `app/ferias/views.py:694`). |
| **1221(3): porta, DRY, reversão, trilha, negativo e ESTADO** | **Parcial:** DRY padrão, porta e apresentação negativa existem; reversão/trilha falham nas condições descritas; contador não chega ao ESTADO (`app/ferias/management/commands/realinhar_concedidos_ferias.py:65`, `app/ferias/services.py:143`, `app/templates/ferias/_secao_colab.html:50`, `bin/gerar_estado.py:370`). |
| **1221(4): RED cancelar 33 em concedidos 30 pela tela** | **Não demonstrado pela tela:** os casos realinham/cancelam diretamente por serviço. RED anterior/GREEN atual: **[não verificado]** (`app/ferias/test_hj090_um_juiz.py:109`, `app/ferias/test_hj090_um_juiz.py:119`). |
| **1224(1): só soma>concedidos; inverso ao DP** | **Cumprido no algoritmo:** porta ignora soma≤concedidos; lista imprime os dois números; contador separa sentidos (`app/ferias/services.py:146`, `app/ferias/management/commands/listar_ferias_dp.py:57`, `app/ferias/services.py:125`). |
| **1224(2): 52/32 não movidos; listas; medição prévia** | Listagem padrão consulta os registros e imprime os campos pedidos; `--simular` admite somente sombra e reverte a transação (`app/ferias/management/commands/listar_ferias_dp.py:52`, `app/ferias/management/commands/listar_ferias_dp.py:62`, `app/ferias/management/commands/listar_ferias_dp.py:77`, `app/ferias/management/commands/listar_ferias_dp.py:110`). Discriminação medida dos 52 por origem×status no RELATO antes do construtor: **[não verificado]**. |
| **1224(3): recusa com destino preenchido e RED dos 32** | **Parcial:** recusa na porta e preenchimento HTMX existem; caminho convencional e comprovação dos 32 faltam, conforme achado 9 (`app/ferias/services.py:391`, `app/templates/ferias/_secao_colab.html:57`, `app/templates/ferias/_secao_colab.html:65`). |
| **1224(4): 40 dias, dono, véspera e quatro no topo** | **Parcial:** marca véspera e prioriza competência corrente; universo diverge. Dono pode sair `ESTRUTURA`, `None` ou erro, sem resolver o encaminhamento pedido (`app/ferias/management/commands/listar_ferias_com_batida.py:57`, `app/ferias/management/commands/listar_ferias_com_batida.py:64`, `app/ponto/management/commands/e6_oraculo.py:110`, `app/ponto/management/commands/e6_oraculo.py:193`). Classificação e presença dos quatro casos fornecidos: **[não verificado]**. |

O censo de escrita e leitura dos valores tocados pelo diff é:

| Campo/valor | Quem escreve | Quem lê/deriva |
|---|---|---|
| **PA.dias_concedidos** | Realinhar/reverter; histórico; agendar; cancelar DP/admin; trocar; aprovar (`app/ferias/services.py:149`, `:170`, `:416`, `:443`, `:511`, `:538`, `:569`, `:640`). | Saldo/status; contador; validações; edição de direito; telas e relatórios (`app/ferias/models.py:95`, `app/ferias/models.py:217`, `app/ferias/services.py:125`, `app/ferias/views.py:824`, `app/templates/ferias/saldo.html:37`, `app/relatorios/services.py:290`). |
| **PA.dias_vendidos** | Agendamento+abono; histórico; venda isolada (`app/ferias/services.py:244`, `app/ferias/services.py:417`, `app/ferias/services.py:721`). | Saldo, teto vendável, status e apresentação (`app/ferias/models.py:95`, `app/ferias/models.py:217`, `app/ferias/models.py:235`, `app/templates/ferias/_secao_colab.html:51`). |
| **PA.dias_saldo** | **Property, sem coluna gravada** (`app/ferias/models.py:216`). | Validadores; remarcar; situação; listas; alertas; APIs; IA; relatórios (`app/ferias/views.py:241`, `app/ferias/models.py:222`, `app/ferias/situacao.py:69`, `app/ferias/management/commands/listar_ferias_dp.py:68`, `app/ferias/management/commands/alertar_ferias_vencendo.py:36`, `app/api/views_mensageria.py:840`, `app/core/views_ia.py:492`, `app/relatorios/services.py:316`, `app/relatorios/views.py:370`). |
| **PA.status** | `aplicar_status` atribui; portas acima persistem; sincronização e garantia escrevem sem trava; criação recebe defaults (`app/ferias/models.py:212`, `app/ferias/services.py:321`, `app/ferias/services.py:341`, `app/ferias/services.py:348`). | `pode_agendar/pode_vender`, escolha da aprovação, listas DP e relatórios (`app/ferias/models.py:231`, `app/ferias/models.py:239`, `app/ferias/services.py:619`, `app/ferias/management/commands/listar_ferias_dp.py:67`, `app/relatorios/services.py:281`). |
| **AG.dias/datas** | Histórico, agendamento, aprovação e troca (`app/ferias/services.py:395`, `app/ferias/services.py:440`, `app/ferias/services.py:629`, `app/ferias/services.py:570`). | Soma canônica; devolução no cancelamento; saldo de remarcação; fracionamento; materialização; listas e folha (`app/ferias/services.py:106`, `app/ferias/services.py:511`, `app/ferias/models.py:222`, `app/ferias/models.py:360`, `app/ponto/services/ausencia.py:1195`, `app/folha/export.py:491`). |
| **AG.status** | Criações; cancelamentos; troca; cron (`app/ferias/models.py:316`, `app/ferias/services.py:399`, `app/ferias/services.py:514`, `app/ferias/services.py:546`, `app/ferias/services.py:575`, `app/ferias/services.py:743`). | Guardas travadas; soma; consultas por janela; materializador; telas/listas (`app/ferias/services.py:495`, `app/ferias/services.py:106`, `app/ferias/services.py:53`, `app/ponto/services/ausencia.py:1181`, `app/ferias/views.py:81`). |
| **Ausencia de férias** | Espelhamento cria/remove; cancelamento rejeita/remove; edição e decisão genéricas também alteram (`app/ponto/services/ausencia.py:1191`, `app/ponto/services/ausencia.py:1203`, `app/ponto/services/ausencia.py:1271`, `app/ponto/services/ausencia.py:1282`, `app/ponto/services/ausencia.py:509`, `app/ponto/services/ausencia.py:836`). | Cobertura e consumidores disparados pelos signals; o próprio espelhamento lê existentes (`app/ponto/turnos.py:2538`, `app/ponto/signals.py:143`, `app/ponto/signals.py:250`, `app/ponto/services/ausencia.py:1182`). |

Também permanecem superfícies públicas sem trava nos modelos: `PeriodoAquisitivo.save` só verifica sobreposição; `AgendamentoFerias.save` grava e espelha sem chamar `full_clean`; seu `delete` remove Ausencias, mas não devolve concedidos (`app/ferias/models.py:201`, `app/ferias/models.py:446`, `app/ferias/models.py:453`). Chamador de produção que invoque diretamente esse `delete` de instância fora das portas: **[não verificado]**. O admin de férias registra somente Afastamento e o admin de Ausencia é de leitura (`app/ferias/admin.py:4`, `app/ponto/admin.py:38`).

Quanto à ordem, os caminhos inspecionados de cancelamento/troca/confirmacão fazem **agendamento→período→Ausencia**; criadores travam período e criam agendamento novo; realinhar/reverter travam apenas período (`app/ferias/services.py:494`, `app/ferias/services.py:497`, `app/ferias/services.py:541`, `app/ferias/services.py:578`, `app/ferias/services.py:394`, `app/ferias/services.py:434`, `app/ferias/services.py:618`, `app/ferias/services.py:144`). O cron de status trava somente agendamento e depois espelha Ausencias (`app/ferias/services.py:740`, `app/ferias/services.py:744`, `app/ferias/models.py:450`). Caminho comprovado de período→trava de agendamento existente ou deadlock reproduzido: **[não verificado]**.

Os contraexemplos adicionais pedidos têm estes resultados estáticos:

| Caso | Resultado |
|---|---|
| **Cancelar depois do realinhamento** | Alinhado 33→cancelar 33 resulta concedidos 0; há assertiva desse serviço, sem POST nesse cenário (`app/ferias/services.py:538`, `app/ferias/test_hj090_um_juiz.py:119`). |
| **Venda+agendamento simultâneos** | Corrida permanece: validações anteriores à trava, conforme achado 1 (`app/ferias/views.py:297`, `app/ferias/views.py:962`, `app/ferias/services.py:434`). |
| **Período com abono** | Direito 30, vendidos 10, soma 23: realinhar grava concedidos 23 e mantém saldo −3; contador fica alinhado porque compara somente concedidos×soma (`app/ferias/services.py:149`, `app/ferias/models.py:217`, `app/ferias/services.py:125`). |
| **Histórico atravessa dois períodos** | Todo o intervalo é debitado no PA escolhido pelo início; não há repartição (`app/ferias/views.py:205`, `app/ferias/services.py:398`, `app/ferias/services.py:416`). Obrigação de repartir nos avais fornecidos: **[não verificado]**. |
| **Reversão duas vezes** | Imediatamente, a segunda é recusada porque concedidos já voltou ao valor anterior; retorno posterior ao mesmo número reabre a aplicação do snapshot antigo (`app/ferias/services.py:167`, `app/ferias/services.py:170`). |
| **Realinhar duas vezes** | Porta não grava novamente quando soma=concedidos; o comman

[truncado em 20000 de 20715 caracteres]

## 13/11 13:59 · sessao 92e95c273df1 · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — ordem de trava do lote diverge da porta admin; deadlock possível, ocorrência [nao verificado].** O admin trava Colaborador antes de validar/materializar (`app/chamados/services/validacao.py:474`, `:479`, `:507`). O lote abre atomic e chama diretamente o materializador, sem travar Colaborador no topo (`app/chamados/services/passivo_validado.py:374`, `:433`, `:517`, `:522`). O materializador cria Batida (`app/chamados/services/materializacao.py:1053`); seu signal recomputa TurnoMaterializado (`app/ponto/signals.py:34`) com DELETE/INSERT (`app/ponto/nucleo.py:57`, `:79`). Só depois reconcilia destino/clones (`app/chamados/services/materializacao.py:1100`, `:886`), cujo gate toma Colaborador (`app/chamados/models.py:708`, `:712`, `:2687`). Contraexemplo de concorrência: lote segura linha TurnoMaterializado; admin toma Colaborador e espera aquela linha no recompute; lote tenta tomar Colaborador no fechamento. É inversão herdada pelo caminho novo do par; sua introdução histórica [nao verificado].

2. **NAO-BLOQ quanto a segundo escritor de Batida.** `_gravar_irma_do_par` delega a `_materializar_uma` (`app/chamados/services/materializacao.py:1258`), que chama `_materializar_resposta_hora` (`:1384`), que usa `registrar_batida` (`:1052`, `:1053`). A criação efetiva continua em `app/ponto/registro_batida.py:157`. `_validar_o_par` usa o mesmo materializador (`app/chamados/services/validacao.py:399`). Não encontrei escrita alternativa nesse percurso.

3. **NAO-BLOQ para rastros de banco no rollback; efeitos externos exigem ressalva.** Existem signals síncronos entre as duas gravações: núcleo (`app/ponto/signals.py:34`), cartório (`:131`) e reconciliador (`:206`). Contudo, o par roda em savepoint (`app/chamados/services/validacao.py:396`), a irmã em atomic aninhado (`app/chamados/services/materializacao.py:1255`) e cada materialização também (`:1382`). O outbox da Pergunta é escrito sincronicamente no save (`app/chamados/signals.py:253`, `:259`); os becos são `on_commit` (`:235`, `:240`); recálculo também é `on_commit` (`app/ponto/services/fechamento.py:1824`). O broadcast utiliza `pg_notify` na mesma conexão Django (`app/ponto/realtime.py:40`, `:42`). Não há prova de persistência desses rastros após rollback no código examinado.

   Há, porém, um caminho externo sem `on_commit`: signal → cartório (`app/ponto/signals.py:135`) → emissor de cobrança (`app/ponto/services/cartorio.py:1791`, `:1829`) → push imediato (`app/ponto/management/commands/detectar_ausencias.py:777`, `:779`) → `webpush` (`app/chamados/push_utils.py:40`). Que o estado intermediário do caso fornecido alcance efetivamente essa emissão é **[nao verificado]**; não transformar o caminho alcançável em incidente provado. A fábrica de perguntas silencia seu próprio push (`app/chamados/services/fabrica_da_celula.py:218`).

4. **Integração incompleta com lote — candidato a bloqueante, efeito exato [nao verificado].** O skip de irmã já materializada foi acrescentado apenas ao laço da disputa (`app/chamados/services/materializacao.py:1538`). O lote mantém a iteração sobre todo o snapshot (`app/chamados/services/passivo_validado.py:520`) e chama `_materializar_uma` para cada objeto (`:522`), mesmo se a pergunta anterior acabou de puxar sua irmã. `_materializar_uma` não verifica o carimbo existente antes do plantio (`app/chamados/services/materializacao.py:1340`, `:1384`) e chama novamente `carimbar` (`:1439`). Contraexemplo: grupo contém saída e volta validadas; saída grava ambas; o lote chega ao objeto volta antigo e tenta processá-lo outra vez. Recarimbo/trilha e efeitos concretos devem ser conferidos contra a implementação de `carimbar`; não executei teste.

## 13/11 13:59 · sessao ccf968f16d21 · - · - · origem=Ronald (tmux)

**BLOQUEANTE — o passivo já absolvido não é recuperado por quatro entradas independentes.** Contraexemplo: saída S01:00 respondida e absolvida por `VIA_PARIDADE_RECUSADA`, com `materializada_em` e `validada_em` preenchidos; volta E02:00 respondida posteriormente. O contador exclui a saída (`materializacao.py:191-193`); validar a própria saída retorna `ja_validada` (`validacao.py:433-435`); validar a volta não encontra a saída como irmã (`materializacao.py:136-139`, usado por `validacao.py:390`); laço/lote exclui a saída (`materializacao.py:1531-1534`). `_gravar_irma_do_par` compartilha a seleção da irmã e não constitui quinta entrada (`materializacao.py:1250-1251`). A devolução também não desfaz o carimbo, mas é guarda em série do clique, não quinto caminho (`validacao.py:190-193`). Quantos dos 60 pertencem a essa classe: **[nao verificado]**.

**BLOQUEANTE — tela promete par com irmã implausível que o clique recusa.** Contraexemplo: golden com saída respondida 01:00 e volta respondida 14:00 para marco 02:00, ambas não materializadas; a derivação da volta usa o marco 02:00 (`materializacao.py:511-518`), tornando o par projetado válido, e tela devolve `aplicavel` (`materializacao.py:563-568`). O clique chama `motivo_implausivel_validacao(q)` e desiste do par (`validacao.py:390-392`); juiz compara resposta 14:00 com marco 02:00 e acusa 720 minutos (`juizes.py:1799-1824`, `juizes.py:2002-2022`). Depois tenta saída isolada (`validacao.py:508-509`).

**BLOQUEANTE — tela promete par com dia do chamado da irmã divergente; porta recusa.** Contraexemplo: perguntas com mesmo `data_evento` 25/09, porém chamado que lista a volta diz 26/09, sem `data_conferida_em`. Seleção considera somente igualdade entre `data_evento` das perguntas (`materializacao.py:136-139`); projeção deriva hora sem conferir a divergência (`materializacao.py:143-163`), tela promete o par (`materializacao.py:563-568`). A escrita da irmã consulta `data_do_fato_de`, grava recusa `DATA_EM_DISPUTA` e retorna sem materializar (`materializacao.py:1353-1376`); o juiz obtém dia do chamado e compara (`fio.py:77-88`).

**BLOQUEANTE condicional — `materializada_em` ainda vale por “encerrou sem batida” e “gravou”, permitindo sucesso falso.** Se uma pergunta absolvida tiver `validada_em=None`, o clique passa pelo carimbo (`validacao.py:479`), pula materialização por `materializada_em` preenchido (`validacao.py:505`), o laço também a exclui (`materializacao.py:1531-1534`) e pode retornar “ponto gravado” (`validacao.py:567-568`), repassado pela casca (`acoes_disputa.py:62-64`). Esse estado pode ser produzido pelo escritor de resposta, que zera validação (`disputa_emissao.py:1470-1485`) mas não descarimba via de identidade (`models.py:3353-3356`). A chegada desse estado em produção/tela normal: **[nao verificado]**. A via de paridade carimba sem criar batida (`materializacao.py:1050-1051`); o campo declara “batida criada + chamado fechado” (`models.py:3445-3448`).

**NAO-BLOQ — não há contraexemplo confirmado de tela aplicável causado apenas pelos outros três atributos pedidos.** Irmã já materializada é excluída (`materializacao.py:138`); outro `data_evento` é excluído (`materializacao.py:137`); irmã retratada é excluída por `fora_do_fato` (`materializacao.py:139`, `models.py:3227`, `models.py:2414`). Já materializada com batida real pode permitir aplicação isolada legítima; já materializada por identidade sem batida não é projetada (`materializacao.py:138`, `materializacao.py:555-568`).

**Allowlist:** os dois escritores usam guardas de escrita (`materializacao.py:1251`, `validacao.py:391-399`); as duas entradas de leitura excluem justamente o passivo antes de consultar autoridade (`materializacao.py:138`, `materializacao.py:192`). Portanto, a justificativa “carimbo como porta” não prova atendimento do universo autorizado; as exceções escondem esse defeito do selo (`test_contract_leitores_vitalidade.py:104-114`, `test_contract_leitores_vitalidade.py:330-333`).

Mapa dos campos:

- `validada_em`: `_carimbar_validacao` escreve e registra decisão (`validacao.py:135-143`); devolução limpa (`validacao.py:197-210`); resposta nova limpa (`disputa_emissao.py:1470-1482`); carimbo simétrico também preenche (`models.py:3160-3163`). Leem: bloqueio inicial (`validacao.py:433`), par (`validacao.py:391-396`), irmã já validada (`materializacao.py:1251`), laço (`materializacao.py:1533`).
- `materializada_em`/via: atribuição canônica em `_escrever_carimbo` (`models.py:3123-3127`), persistência em `carimbar` (`models.py:3154-3178`) ou `carimbar_identidade` (`models.py:3208-3216`); recusa de paridade chama esta última (`materializacao.py:1050`). Leem seleção da irmã, contador, tela, comprovação do par e laço (`materializacao.py:138`, `materializacao.py:192`, `materializacao.py:542-550`, `validacao.py:399`, `materializacao.py:1534-1540`).
- `estado_aplicacao_pergunta`: deriva e retorna valor sem persistência (`materializacao.py:533-574`); explicador lê (`explicador.py:99-102`), contador lê (`materializacao.py:201`).
- `gravou`: explicador escreve apenas `False` para via recusada (`explicador.py:100-102`); desenlace lê ausência de falsos como autorização para dizer “Tudo validado e gravado” (`desenlace.py:65-73`).
- `msg`: service escreve (`validacao.py:563-568`); casca lê e repassa (`acoes_disputa.py:62-64`).

_4614 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
