# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 12:32** · publicado em **12/11/2026 12:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 12:32 · sessao 8db6fd841a74 · AUDITORIA-DE-TRAVAS · leitura · origem=Code

As contas batem com a medida fornecida; há furos na semântica do prazo e na cobertura do selo. (`contrato_item6.txt:3–6`; `medida_6_8.r:9–11`; `bin/codex_pergunta.sh:348–408`; `bin/tests/test_codex_pergunta.sh:447–548`)

Nos anexos, conto linhas desde a primeira linha de conteúdo fornecida.

1. **Sítios de tempo no wrapper**

| Sítio | Hoje | Efeito da tabela por papel com cap |
|---|---|---|
| Documentação e atribuição | Segundos, default `900`; `CODEX_TIMEOUT` entra diretamente em `TIMEOUT`. | Muda a origem do valor; o contrato deixa ambíguo o default quando a variável está ausente/vazia. (`bin/codex_pergunta.sh:6`, `:83`; `contrato_item6.txt:3–4`) |
| Validação do papel | Aceita exatamente leitura, censo, verificador, sonda e construtor; demais saem com rc 2. | Esses cinco papéis precisam estar cobertos pela tabela. (`bin/codex_pergunta.sh:81–85`; `contrato_item6.txt:3–4`) |
| Vaga do construtor | `flock -n`: recusa imediata com rc 2. | Não usa `TIMEOUT`; permanece imediato. (`bin/codex_pergunta.sh:239–240`) |
| Gate e slots | Gate bloqueante sem prazo; busca slots imediatamente e repete espera de cinco segundos enquanto não consegue. | A tabela não limita essa espera acumulada. `CODEX_TETO` controla quantidade de slots, não segundos. (`bin/codex_pergunta.sh:84`, `:348–364`) |
| Diagnóstico de espera | Mede desde antes do gate até obter o slot. | Continua medindo espera efetiva; não é o teto da tabela. (`bin/codex_pergunta.sh:350–365`) |
| Trava da resposta | `flock -w "$TIMEOUT"`; mensagem usa `${TIMEOUT}s`; falha sai com rc 3. | Espera e mensagem passam a usar o valor efetivo limitado pelo papel. (`bin/codex_pergunta.sh:374–376`) |
| Execução construtora | `timeout -k 30 "$TIMEOUT" codex …`. | Usa o valor limitado, mantendo os 30 segundos adicionais de escalada. (`bin/codex_pergunta.sh:401–405`) |
| Execução dos quatro papéis de leitura | `timeout "$TIMEOUT" codex …`. | Verificador recebe seu teto; leitura/censo/sonda recebem o teto comum previsto. (`bin/codex_pergunta.sh:408–409`; `contrato_item6.txt:3`) |
| Duração e retorno | `fim` grava papel, rc e duração desde `T0C`; captura o rc da execução, com possíveis substituições posteriores. | Duração observada muda, mas seu significado permanece; o valor efetivo de `TIMEOUT` não é registrado. (`bin/codex_pergunta.sh:252–260`, `:411`, `:433–455`) |

2. **Furos e limites do contrato**

- **Teto da execução não é prazo total do lançador.** Há fila sem limite acumulado; depois, a trava da resposta pode consumir um `TIMEOUT` inteiro e o Codex recebe outro inteiro. Preparação, revalidação e inventário ficam fora do timeout do Codex. Portanto, “até ~32 min para cair” não fecha como limite total do wrapper. (`bin/codex_pergunta.sh:295–319`, `:348–376`, `:391–420`; `aval_respostas_1211_1230.txt:3`)

- **“Estourou = rc 124” precisa de escopo.** Vencer a espera da resposta produz rc **3**. No construtor, recusa do inventário também vence qualquer retorno anterior com rc **3**. (`contrato_item6.txt:4`; `bin/codex_pergunta.sh:376`, `:417–433`)

- **Default não está fechado.** Conservar `${CODEX_TIMEOUT:-900}` e apenas aplicar cap deixa construtor e leitura/censo/sonda em **900** quando não houver override; usar o teto como default produz **1930/1191**. O contrato especifica os tetos e o rebaixamento, mas não explicita essa escolha. (`bin/codex_pergunta.sh:83`; `contrato_item6.txt:3–4`)

- **Papéis atuais estão cobertos pelo texto**, desde que leitura, censo e sonda tenham entradas correspondentes a 1191. Não há papel atual omitido na enumeração; a recusa de papel desconhecido já existe. A futura tabela e sua recusa por ausência de entrada permanecem **[não verificado]**. (`bin/codex_pergunta.sh:85`; `contrato_item6.txt:3–4`)

- **ESMERIL-SEMÂNTICA:** os leitores demonstrados recebem o mesmo valor apresentado como segundos, mas com significados distintos: espera da trava e duração permitida ao processo. Já `duracao_s` inclui preparação e espera; ultrapassar o teto nessa medida não prova estouro do Codex. Leitor externo que suponha outra unidade: **[não verificado]**. O domínio aceito para override — zero, negativos, frações, sufixos ou texto — também não está definido no contrato; hoje o valor é repassado diretamente. (`bin/codex_pergunta.sh:83`, `:252–260`, `:348–408`; `contrato_item6.txt:4`)

- **Há uso documentado acima do teto:** PC2-FIO-MUDO registra tentativa com `CODEX_TIMEOUT=2400`, em papel **censo**; pelo contrato seria rebaixado a **1191**. Dependência atual desse prazo ou necessidade comprovada dos 2400 segundos: **[não verificado]**. O construtor documentado com 1800 fica abaixo de 1930. (`app/docs/RELATO.md:7756`, `:7782`, `:425`; `contrato_item6.txt:3`)

- **A premissa “selo hoje fixa 900” não corresponde à cópia.** O `900` do selo conta worktrees falsas. O falso captura argumentos do **Codex**, não o argumento de `timeout`; os casos e mutantes atuais não cobrem cap. Assim, `3600 → 1191` e mutante sem cap vermelho são exigências futuras, ainda **[não verificado]** por execução. (`bin/tests/test_codex_pergunta.sh:101–111`, `:322–328`, `:447–548`; `contrato_item6.txt:6`)

- **Registro disponível não equivale ao contador contratado.** `fim` contém papel, rc e duração; o contador existente por papel conta linhas `ini`, qualquer rc. A meta de até um rc 124 por papel em 24 horas permanece **[não verificado]** pelo dado fornecido. (`bin/codex_pergunta.sh:253–260`; `bin/codex_limite_estado.py:71–79`; `contrato_item6.txt:5`)

O outro `900`, em `encadeado.sh`, limita espera por carimbos e sai com rc 2; sua manutenção está expressamente autorizada no aval. (`bin/encadeado.sh:71`, `:84–96`; `aval_respostas_1211_1230.txt:3`)

3. **Conferência dos números**

| Papel/grupo medido | p95 + 300 s | Contrato |
|---|---:|---:|
| Construtor | 1630 + 300 = **1930 s** | Confere |
| Verificador | 594 + 300 = **894 s** | Confere |
| Leitura/censo/sonda | 891 + 300 = **1191 s** | Confere |

Fontes das três contas: `medida_6_8.r:9–11`; margem e tetos: `contrato_item6.txt:3`.

A terceira janela contém **20 chamadas do grupo**, não 20 de cada papel: p95 individual de leitura, censo e sonda é **[não verificado]**. Os números conferem para a duração registrada; sua capacidade de evitar futuros rc 124 é **[não verificado]**. (`medida_6_8.r:5–11`; `bin/codex_pergunta.sh:252–260`, `:348–408`)

## 12/11 12:31 · sessao 796afec6fbfb · - · - · origem=Ronald (tmux)

Leitura concluída dos dois arquivos; sem execução de teste ou alteração. [não verificado] [nao verificado]

- **“Selo hoje fixa 900”: falso nesta cópia.** O único `900` do selo é a quantidade de worktrees falsas no caso da lista grande, sem relação com segundos (`bin/tests/test_codex_pergunta.sh:322–328`). O selo captura argv/stdin do `codex` falso, não o argumento de `timeout` (`bin/tests/test_codex_pergunta.sh:101–111`); casos e mutantes atuais não cobrem timeout/cap (`bin/tests/test_codex_pergunta.sh:447–452`, `:474–548`). O caso `3600 → 1191` é exigência futura do anexo, não prova existente (`contrato_item6.txt:6`, numeração do texto fornecido).

- **Prazo global de ~32 min não resulta desses usos.** Há gate bloqueante sem prazo (`bin/codex_pergunta.sh:351`), loop de slots com tentativas de cinco segundos sem limite total (`:353–363`), depois espera da resposta de até `TIMEOUT` (`:376`) e posteriormente outro `TIMEOUT` inteiro para Codex (`:401`, `:408`). Preparação/revalidação/inventário também ficam fora do timeout do Codex (`:295–319`, `:391–399`, `:419–427`). Logo, o teto de 1930 s limita o processo chamado naquela linha; não limita toda a espera do lançador (`:401–411`, `:455`).

- **“Estourou = rc 124 como hoje” precisa de escopo.** Espera vencida da resposta devolve rc 3, não 124 (`bin/codex_pergunta.sh:376`). Inventário recusado vence qualquer rc anterior e também devolve 3 (`:417`, `:433`). O ramo construtor ainda tem tolerância de kill de 30 s (`:401`); 1930 s não é o instante do kill forçado.

- **Default permanece ambíguo no contrato.** Hoje `TIMEOUT=${CODEX_TIMEOUT:-900}` (`bin/codex_pergunta.sh:83`). Aplicar somente `min(default900, teto)` deixa construtor/leitura/censo/sonda em 900 quando o ambiente não define valor; usar o teto da tabela como default muda esse comportamento. O anexo especifica teto e cap, mas não escreve explicitamente a regra para ambiente ausente/vazio (`contrato_item6.txt:3–4`, numeração do texto fornecido).

- **Domínio do override também fica aberto.** Hoje o valor chega sem validação ao `flock` e ao `timeout`, enquanto o diagnóstico o apresenta como segundos (`bin/codex_pergunta.sh:83`, `:376`, `:401`, `:408`). O contrato não define aceitação/rejeição de zero, negativos, frações, sufixos ou texto (`contrato_item6.txt:4`). Portanto, um cap exclusivamente numérico não fecha sozinho a semântica desse campo.

- **Cinco papéis atuais, três classes de teto.** A whitelist contém exatamente leitura/censo/verificador/sonda/construtor e já recusa os demais com rc 2 (`bin/codex_pergunta.sh:85`). O contrato cobre todos somente se `leitura/censo/sonda` significar três chaves mapeadas para 1191, não uma chave literal contendo barras (`contrato_item6.txt:3`). O ramo de execução também agrupa todos os quatro papéis de leitura, embora verificador tenha teto próprio (`bin/codex_pergunta.sh:391–409`).

- **Medida existente não registra timeout efetivo.** A linha `fim` registra papel, rc e duração (`bin/codex_pergunta.sh:253–260`); a duração começa em `T0C`, antes da preparação e das esperas (`:252`, `:295–319`, `:348–376`). O sidecar `.rc` do construtor registra id/rc/fim, sem papel/duração/timeout (`:261–263`). Assim, duração acima do teto não demonstra estouro do `codex exec`; o contador por rc 124 também não inclui espera vencida da resposta nem timeout substituído por recusa de inventário (`:376`, `:433`).

- **Aritmética bate:** 1630+300=1930, 594+300=894, 891+300=1191 (`contrato_item6.txt:3`; `medida_6_8.r:9–11`, numeração do texto fornecido). A terceira medida é do **grupo** leitura/censo/sonda; p95 individual de cada um desses papéis permanece [não verificado]. A medida não demonstra que esses tetos evitarão rc 124 futuros; o próprio intervalo medido inclui preparação/esperas (`bin/codex_pergunta.sh:252`, `:260`, `:348–376`).

## 12/11 12:30 · sessao c1edd9f2c0a0 · LASTRO-FAIXA-SEM-DEFAULT · censo · origem=Code

Há **três casamentos ativos por HH:MM**: classificação de órfãs, exclusão de anomalias e supressão de alertas de turno. Nesta cópia estão em `app/relatorios/pdf_espelho.py:479`, `app/relatorios/pdf_espelho.py:679` e `app/relatorios/pdf_espelho.py:923`. **Colisões efetivamente ocorridas em produção: [nao verificado].**

L-037 determina que o PDF desenhe os dias da tela; L-038 determina que leia o mesmo juiz, sem regra própria de dia (`app/docs/LEIS.md:85`, `app/docs/LEIS.md:86`).

**1. Censo manual**

Nas tabelas, **PERDE-A-DATA** identifica uma operação que permite confundir registros de datas diferentes; não afirma ocorrência em produção. A possibilidade de D/D+1 na mesma linha decorre de todas as batidas reclamadas pelo turno receberem `t.data_turno`, inclusive quando a saída tem outra data civil (`app/ponto/turnos.py:2953`, `app/ponto/turnos.py:322`).

Os produtores comuns aos casamentos são:

| Dado | Produtor e transporte |
|---|---|
| `dia['batidas']` | A tela agrupa pelo dia respondido pelo juiz, ordena por timestamp e publica a lista (`app/ponto/services/espelho.py:580`, `app/ponto/services/espelho.py:582`, `app/ponto/services/espelho.py:665`, `app/ponto/services/espelho.py:776`). O PDF recebe a tela em `app/relatorios/pdf_espelho.py:474` e recorta os dias em `app/relatorios/pdf_espelho.py:562`. |
| `orfas_grade` | A ata transporta strings de horas; o leitor as recupera e anexa ao dia (`app/ponto/services/cartorio.py:736`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:390`, `app/escala/services/leitor_celula.py:420`). A tela seleciona a grade por data, adapta e publica as órfãs (`app/ponto/services/espelho.py:531`, `app/ponto/services/espelho.py:684`, `app/ponto/services/espelho.py:699`, `app/escala/utils.py:1113`, `app/ponto/services/espelho.py:854`). |
| Órfãs pelo builder | Batidas não consumidas são atribuídas pela data civil e convertidas para HH:MM (`app/escala/utils.py:1844`, `app/escala/utils.py:1856`, `app/escala/utils.py:1858`). |
| `dia['turnos']` / `pp.entrada` | O adaptador conserva o timestamp de entrada; o período recebe a entrada localizada completa (`app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:43`, `app/ponto/motor_calculo_v2.py:2751`). A tela associa entrada → pk → dia do juiz e publica os períodos nessa linha (`app/ponto/services/espelho.py:458`, `app/ponto/services/espelho.py:461`, `app/ponto/services/espelho.py:463`, `app/ponto/services/espelho.py:777`). |
| `anomalias_mes` | O adaptador escreve mensagens com **DD/MM HH:MM**, tanto para saída órfã como para entrada aberta (`app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:42`). O motor instala as anomalias no resultado, que o PDF publica (`app/ponto/motor_calculo_v2.py:2354`, `app/ponto/motor_calculo_v2.py:2357`, `app/relatorios/pdf_espelho.py:607`). |

O inventário dos casamentos e das coleções que os sustentam é:

| Local / função | Operandos, chave ou elemento; origem | Classificação e alcance entre dias |
|---|---|---|
| `app/relatorios/pdf_espelho.py:475` — `_coletar_dados_espelho` | Dict: chave `d['data']`; valor `set(d.get('orfas_grade') or [])`. Ambos vêm da tela recebida em `app/relatorios/pdf_espelho.py:474`; produtores acima. | **SEGURA apenas a chave externa**, que particiona por dia da linha. O set interno contém somente horários; essa partição não garante data civil única para as batidas confrontadas em `app/relatorios/pdf_espelho.py:479` (`app/ponto/turnos.py:2953`). |
| `app/relatorios/pdf_espelho.py:479` — `_coletar_dados_espelho` | Esquerda: `localtime(b.timestamp).strftime('%H:%M')`, com `b` de `d['batidas']` em `app/relatorios/pdf_espelho.py:478`. Direita: `_orfas_por_data[d['data']]`, produzido em `app/relatorios/pdf_espelho.py:475`. Operação: `in`. | **PERDE-A-DATA.** Compara hora da batida com strings da grade. A linha pode conter D e D+1; não há comparação da data civil de cada item (`app/ponto/turnos.py:2953`, `app/ponto/turnos.py:322`). |
| `app/relatorios/pdf_espelho.py:675` — `_build_story` | Elemento/chave do set `_hh_esp_periodo`: HH:MM de cada batida `orfa`, varrendo **todos os dias**. Dias vêm de `app/relatorios/pdf_espelho.py:604`; a marca `orfa` nasce em `app/relatorios/pdf_espelho.py:568`. | **PERDE-A-DATA.** O conjunto reúne horários do período inteiro e elimina qualquer distinção de data (`app/relatorios/pdf_espelho.py:676`, `app/relatorios/pdf_espelho.py:677`). |
| `app/relatorios/pdf_espelho.py:679` — `_build_story` | Esquerda: `_hh`, do set produzido em `app/relatorios/pdf_espelho.py:675`. Direita: `str(a)`, de `dados['anomalias_mes']`, publicado em `app/relatorios/pdf_espelho.py:607`. Operação: substring `in`; resultado negado pelo filtro. | **PERDE-A-DATA.** Uma órfã pode excluir uma anomalia de outro dia com o mesmo HH:MM. O produtor fornece DD/MM, mas esse filtro não o compara (`app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:42`). |
| `app/relatorios/pdf_espelho.py:919` — `_build_story` | Elemento/chave do set `_hh_espurias`: HH:MM de `b.timestamp`, selecionando `b.orfa` na linha. Marca produzida em `app/relatorios/pdf_espelho.py:568`; batidas publicadas em `app/ponto/services/espelho.py:776`. | **PERDE-A-DATA.** Mantém a linha, mas descarta a data civil dos instantes usados na próxima comparação (`app/relatorios/pdf_espelho.py:920`, `app/ponto/turnos.py:2953`). |
| `app/relatorios/pdf_espelho.py:923` — `_build_story` | Esquerda: `_abre_em`, HH:MM de `pp.entrada`, produzido em `app/relatorios/pdf_espelho.py:922`. Direita: `_hh_espurias`, produzido em `app/relatorios/pdf_espelho.py:919`. `pp` vem de `dia['turnos']` em `app/relatorios/pdf_espelho.py:921`; produtores acima. | **PERDE-A-DATA.** Igualdade de relógio basta para executar `continue`, suprimindo tanto “turno aberto” como seus alertas; não exige igualdade dos instantes nem das datas civis (`app/relatorios/pdf_espelho.py:928`, `app/relatorios/pdf_espelho.py:929`, `app/relatorios/pdf_espelho.py:931`). Colisão concreta nessa linha: [nao verificado]. |

A guarda `_entrada_espuria` **já compara instantes completos** nesta cópia: `periodo.entrada in orfas_instantes`. Entretanto, o conjunto recebido foi selecionado pelo casamento HH:MM de `:479`; preservar o timestamp depois não corrige uma seleção anterior indevida (`app/relatorios/pdf_espelho.py:178`, `app/relatorios/pdf_espelho.py:478`, `app/relatorios/pdf_espelho.py:479`, `app/relatorios/pdf_espelho.py:516`).

As demais ocorrências de relógio são **SO-EXIBICAO**. Nas formatações abaixo, o segundo operando é o formato literal ou o compositor de texto, não outro registro:

| Local / função | Operand​os e produtores | Classificação |
|---|---|---|
| `app/relatorios/pdf_espelho.py:691`, `app/relatorios/pdf_espelho.py:692` — `_build_story` | `tipo_esc.hora_inicio` e `tipo_esc.hora_fim` × `'%H:%M'`. São `TimeField`; escala selecionada em `app/relatorios/pdf_espelho.py:306`, tipo obtido em `app/relatorios/pdf_espelho.py:307`, transportado em `app/relatorios/pdf_espelho.py:605` e lido em `app/relatorios/pdf_espelho.py:689` (`app/escala/models.py:50`, `app/escala/models.py:51`). | **SO-EXIBICAO**, cabeçalho (`app/relatorios/pdf_espelho.py:699`). |
| `app/relatorios/pdf_espelho.py:840` — `_build_story` | Timestamp das batidas selecionadas em `app/relatorios/pdf_espelho.py:835` × `'%H:%M'`; produtor da lista na tabela anterior. | **SO-EXIBICAO**, entregue a `Marcacoes` (`app/relatorios/pdf_espelho.py:852`). |
| `app/relatorios/pdf_espelho.py:844` — `_build_story` | Timestamp das órfãs selecionadas em `app/relatorios/pdf_espelho.py:836` × `'%H:%M'`. | **SO-EXIBICAO**, texto cinza (`app/relatorios/pdf_espelho.py:855`). |
| `app/relatorios/pdf_espelho.py:866` — `_build_story` | Duas formatações: `_te.hora_inicio` e `_te.hora_fim` × `'%H:%M'`. `_te` vem de `dados['tipo_escala']` em `app/relatorios/pdf_espelho.py:862`; origem em `app/relatorios/pdf_espelho.py:307`. | **SO-EXIBICAO**, coluna Horário (`app/relatorios/pdf_espelho.py:937`). |
| `app/relatorios/pdf_espelho.py:914` — `_build_story` | Timestamp de `_bp`, iterado de `dia['batidas']` em `app/relatorios/pdf_espelho.py:910`, × `'%H:%M'`; combinado com a frase de procedência. | **SO-EXIBICAO**, observação (`app/relatorios/pdf_espelho.py:915`, `app/relatorios/pdf_espelho.py:933`). |
| `app/relatorios/pdf_espelho.py:139`, `app/relatorios/pdf_espelho.py:143`, `app/relatorios/pdf_espelho.py:165` — `Marcacoes._quebrar` / `draw` | `hhmm` recebido por `self.pares` em `app/relatorios/pdf_espelho.py:128`, a partir de `app/relatorios/pdf_espelho.py:840` e `app/relatorios/pdf_espelho.py:852`; usado em medição, transporte e desenho. | **SO-EXIBICAO.** A comparação derivada `usado + ESPACO + w > largura` compara largura gráfica com largura disponível, não identidades de horários (`app/relatorios/pdf_espelho.py:140`). |
| `app/relatorios/pdf_espelho.py:849` — `_build_story` | `_h_isc` × texto `'(i) %s'`; strings produzidas de `c['hora']` em `app/ponto/services/espelho.py:364` e publicadas em `app/ponto/services/espelho.py:871`. | **SO-EXIBICAO**, texto cinza (`app/relatorios/pdf_espelho.py:855`). |
| `app/relatorios/pdf_espelho.py:901` — `_build_story` | Texto de `marco` recebido da tela × compositor da observação. A tela transporta `hora_marco` da grade, sem o PDF fazer casamento (`app/escala/utils.py:1111`, `app/ponto/services/espelho.py:387`). | **SO-EXIBICAO**, observação (`app/relatorios/pdf_espelho.py:900`). |
| `app/relatorios/pdf_espelho.py:680`, `app/relatorios/pdf_espelho.py:932`, `app/relatorios/pdf_espelho.py:933` — `_build_story` | Texto de anomalia `[:40]`, alerta `[:60]` e observação composta `[:120]`; fontes em `app/relatorios/pdf_espelho.py:678`, `app/relatorios/pdf_espelho.py:931` e `app/relatorios/pdf_espelho.py:875`. | **SO-EXIBICAO**: truncamento de textos que podem conter horários; não extraem HH:MM para casamento. |

Há também candidatos nominais que **não são hora do dia**: `_h` compara duração com `0`, minutos com `60` e minutos com `0`; o banco compara saldo com `0`. São decisões de exibição de quantidades (`app/relatorios/pdf_espelho.py:629`, `app/relatorios/pdf_espelho.py:633`, `app/relatorios/pdf_espelho.py:635`, `app/relatorios/pdf_espelho.py:774`).

As ordenações encontradas usam datas de vigência ou datas dos dias, não relógio (`app/relatorios/pdf_espelho.py:325`, `app/relatorios/pdf_espelho.py:363`, `app/relatorios/pdf_espelho.py:461`, `app/relatorios/pdf_espelho.py:462`, `app/relatorios/pdf_espelho.py:563`, `app/relatorios/pdf_espelho.py:578`). Os carimbos de emissão preservam data completa (`app/relatorios/pdf_espelho.py:1006`, `app/relatorios/pdf_espelho.py:1053`). Não encontrei regex, chamada `.time()` ou slicing específico de string HH:MM no PDF; ausência além desta inspeção: [nao verificado].

**2. Script AST**

O script abaixo apenas lê o PDF e imprime candidatos. Propaga dependências por atribuições e iteráveis de comprehensions, alcançando `_hh` em `:679` e `_abre_em` em `:923`. A ponte externa `orfas_grade` é declarada explicitamente porque seu produtor fica fora do PDF (`app/relatorios/pdf_espelho.py:675`, `app/relatorios/pdf_espelho.py:679`, `app/relatorios/pdf_espelho.py:922`, `app/relatorios/pdf_espelho.py:923`, `app/ponto/services/espelho.py:854`).

```python
import ast
from collections import defaultdict
from pathlib import Path

path = Path("app/relatorios/pdf_espelho.py")
source = path.read_text(encoding="utf-8")
tree = ast.parse(source, filename=str(path))
scope = {}


def index(node, owner="<module>"):
    if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef)):
        owner += "." + node.name
    scope[node] = owner
    for child in ast.iter_child_nodes(node):
        index(child, owner)


index(tree)
env = defaultdict(set)
# Ponte externa comprovada: espelho.py:854 -> escala/utils.py:1113.
bridge = {"orfas_grade"}


def hourly(name):
    name = name.lower()
    return "hora" in name or "hhmm" in name or name == "hr"


def seed(node, name):
    return {f"{node.lineno}:{name}"}


def deps(node):
    if node is None:
        return set()
    if isinstance(node, ast.Name):
        return env[(scope[node], node.id)] | (
            seed(node, node.id) if hourly(node.id) else set()
        )
    if isinstance(node, ast.Constant):
        return set()
    if isinstance(node, ast.Attribute):
        return deps(node.value) | (
            seed(node, node.attr) if hourly(node.attr) else set()
        )
    if isinstance(node, ast.Subscript):
        key = node.slice
        extra = set()
        if isinstance(key, ast.Constant) and isinstance(key.value, str):
            if hourly(key.value) or key.value in bridge:
                extra = seed(node, key.value)
        return deps(node.value) | deps(key) | extra
    if isinstance(node, ast.Call):
        extra = set()
        if isinstance(node.func, ast.Attribute):
            if node.func.attr == "strftime":
                for arg in node.args:
                    if isinstance(arg, ast.Constant):
                        if isinstance(arg.value, str) and "%H" in arg.value:
                            extra |= seed(node, "strftime " + repr(arg.value))
            if node.func.attr == "get" and node.args:
                key = node.args[0]
                if isinstance(key, ast.Constant) and isinstance(key.value, str):
                    if hourly(key.value) or key.value in bridge:
                        extra |= seed(node, key.value)
        return extra | union(
            [node.func, *node.args, *(k.value for k in node.keywords)]
        )
    if isinstance(node, (ast.ListComp, ast.SetComp, ast.GeneratorExp)):
        # Não confundir dependência da seleção com identidade do elemento.
        return deps(node.elt)
    if isinstance(node, ast.DictComp):
        return deps(node.key) | deps(node.value)
    if isinstance(node, ast.IfExp):
        return deps(node.body) | deps(node.orelse)
    return union(ast.iter_child_nodes(node))


def union(nodes):
    result = set()
    for node in nodes:
        result |= deps(node)
    return result


def bind(target, value):
    if isinstance(target, ast.Name):
        env[(scope[target], target.id)].update(value)
    elif isinstance(target, (ast.Tuple, ast.List)):
        for element in target.elts:
            bind(element, value)
    elif isinstance(target, ast.Starred):
        bind(target.value, value)


while True:
    before = {key: frozenset(value) for key, value in env.items()}
    for node in ast.walk(tree):
        if isinstance(node, ast.Assign):
            for target in node.targets:
                bind(target, deps(node.value))
        elif isinstance(node, (ast.AnnAssign, ast.AugAssign)):
            bind(node.target, deps(node.value))
        elif isinstance(node, (ast.For, ast.AsyncFor, ast.comprehension)):
            bind(node.target, deps(node.iter))
    if before == {key: frozenset(value) for key, value in env.items()}:
        break


def emit(kind, node, *parts):
    if not any(deps(expr) for label, expr in parts):
        return
    print(f"{path}:{node.lineno}:{node.col_offset} {scope[node]} {kind}")
    for label, expr in parts:
        print(
            " ", label, "=", ast.unparse(expr),
            "| fontes:", ", ".join(sorted(deps(expr))) or "-"
        )


nodes = sorted(
    ast.walk(tree),
    key=lambda n: (getattr(n, "lineno", 0), getattr(n, "col_offset", 0)),
)
for node in nodes:
    if isinstance(node, ast.Compare):
        left = node.left
        for op, right in zip(node.ops, node.comparators):
            # In/NotIn são operadores de Compare, não expressões separadas.
            emit(
                "Compare/" + type(op).__name__, node,
                ("esq", left), ("dir", right),
            )
            left = right
    elif isinstance(node, ast.Subscript):
        emit("Subscript", node, ("base", node.value), ("indice", node.slice))
    elif isinstance(node, ast.Dict):
        for key, value in zip(node.keys, node.values):
            if key is not None:
                emit("dict-key/value", node, ("chave", key), ("valor", value))
    elif isinstance(node, ast.DictComp):
        emit(
            "dict-key/value", node,
            ("chave", node.key), ("valor", node.value),
        )
    elif isinstance(node, (ast.SetComp, ast.Set)):
        parts = (
            (("elemento", node.elt),)
            if isinstance(node, ast.SetComp)
            else tuple(("elemento", e) for e in node.elts)
        )
        emit("set-element/key", node, *parts)
```

Limites: é análise conservadora, sem tipos nem execução. Não resolve chamadas entre funções/arquivos, mutações de containers, escrita em atributos, formatos construídos dinamicamente ou identidades escondidas em aliases externos. Une reatribuições e trata comprehensions no escopo da função; pode superestimar dependências. Completude geral: [nao verificado].

Dois falsos positivos são esperados pelo critério solicitado: nomes contendo `hora` incluem durações; `%H` também aparece em carimbos que conservam data. O script lista candidatos, não determina sozinho PERDE-A-DATA/SEGURA (`app/relatorios/pdf_espelho.py:629`, `app/relatorios/pdf_espelho.py:774`, `app/relatorios/pdf_espelho.py:1006`, `app/relatorios/pdf_espelho.py:1053`).

**3. Tela e adaptador**

| Leitor / operação | Casa com data? |
|---|---|
| Períodos por entrada | **Sim, pelo instante completo:** `resultados_por_entrada[p.entrada] = p` (`app/ponto/services/espelho.py:425`). |
| Turnos abertos | **Sim, pelo instante completo:** guarda `p.entrada` e consulta `_p.entrada in _abertos_ts` (`app/ponto/services/espelho.py:427`, `app/ponto/services/espelho.py:476`). |
| Período → dia da tela | **Sim:** timestamp → pk → data do juiz; depois indexa por essa data (`app/ponto/services/espelho.py:458`, `app/ponto/services/espelho.py:461`, `app/ponto/services/espelho.py:463`). |
| Batida → dia / ordenação | **Sim:** agrupa pela data do juiz e ordena pelo timestamp completo (`app/ponto/services/espelho.py:580`, `app/ponto/services/espelho.py:582`, `app/ponto/services/espelho.py:665`). |
| Anomalia → dia inconsistente | **Sim, por `date`:** extrai DD/MM, tenta os anos da janela/navegação, aceita uma data dentro da janela e consulta `data in _datas_anomalia` (`app/ponto/services/espelho.py:250`, `app/ponto/services/espelho.py:252`, `app/ponto/services/espelho.py:258`, `app/ponto/services/espelho.py:261`, `app/ponto/services/espelho.py:668`). A mensagem original não tem ano (`app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:42`). |
| Grade / `orfas_grade` | Seleciona a **grade por data**, mas não reconstrói identidade de batida por HH:MM: repassa as órfãs da grade (`app/ponto/services/espelho.py:684`, `app/ponto/services/espelho.py:699`, `app/escala/utils.py:1113`, `app/ponto/services/espelho.py:854`). |
| Renderização da tela | Apenas imprime `c.hora`, órfãs e, no fallback, timestamp formatado. Não executa os três casamentos do PDF (`app/templates/ponto/espelho.html:237`, `app/templates/ponto/espelho.html:240`, `app/templates/ponto/espelho.html:241`, `app/templates/ponto/espelho.html:246`). |
| `adaptador_motor.periodos_de_turnos` | **Preserva instantes:** períodos usam `(e_ts, s_ts)` e intervalos usam `e_ts` como chave. As mensagens incluem DD/MM HH:MM (`app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:38`, `app/ponto/adaptador_motor.py:42`, `app/ponto/adaptador_motor.py:43`, `app/ponto/adaptador_motor.py:44`). |

Existem casamentos `(tipo, hora)` nos adaptadores da grade, delimitados pelo objeto do dia: chamados já selecionados por `data`, e remapeamento cujo dict é recriado para cada dia. Eles operam sobre marcos/células, sem provar identidade de uma batida por data civil (`app/ponto/services/espelho.py:700`, `app/escala/utils.py:1099`, `app/escala/services/leitor_celula.py:284`, `app/escala/services/leitor_celula.py:294`, `app/escala/services/leitor_celula.py:297`).

Portanto, a restrição existente é **dia 

[truncado em 20000 de 20172 caracteres]

## 12/11 12:30 · sessao d079e99aa35c · - · - · origem=Ronald (tmux)

- Leitores de `TIMEOUT` no wrapper: atribuição em `bin/codex_pergunta.sh:83`; espera da resposta e diagnóstico em `:376`; execução construtora em `:401`; demais papéis em `:408`. Os dois comandos recebem o mesmo valor, mas começam esperas distintas; o gate não tem prazo (`:351`) e o laço de slots repete tentativas de cinco segundos sem teto acumulado (`:353–363`).

- O antecedente documental acima do teto é PC2-FIO-MUDO: `CODEX_TIMEOUT=2400` (`app/docs/RELATO.md:7782`), cujo papel é `censo` (`:7756`); com o contrato passaria a1191. Dependência atual desse prazo ou duração necessária: **[não verificado]**. O construtor documentado com1800 fica abaixo1930 (`app/docs/RELATO.md:425`).

- Nos lançadores de teste, as atribuições explícitas são20: `bin/tests/test_codex_limite.sh:24` e `bin/tests/test_disjuntor.sh:1213`. Há `timeout900` externo numa cadeia de teste do parser, sem prova de lançador operacional (`bin/tests/test_disjuntor.sh:563`). Arquivos operacionais `lanca*.sh` e ambiente efetivo da sessão: **[não verificado]**.

- Não encontrei consumidor externo do `TIMEOUT` do wrapper na busca integral da cópia, incluindo ocultos/ignorados e excluindo `.git`; todos os leitores encontrados estão nas linhas acima. A divergência semântica comprovada é a duração registrada: nasce antes da preparação (`bin/codex_pergunta.sh:252`) e termina no trap (`:260`), enquanto `TIMEOUT` limita separadamente o lock e o `codex exec` (`:376,:401,:408`). O valor efetivo não aparece no registro `fim` (`:259–260`).

- O contrato “estourou=124” não cobre toda espera: o lock sai3 (`bin/codex_pergunta.sh:376`); no construtor, inventário recusado também vence o retorno com3 (`:417,:420–425,:433`).

- Nesta cópia, `test_codex_pergunta.sh` **não fixa timeout900**: a literal900 corresponde à fabricação de falsos gitdirs (`bin/tests/test_codex_pergunta.sh:324`); `_roda` não atribui `CODEX_TIMEOUT` (`:124–128`). Nenhuma mordida de cap está demonstrada pelo selo atual (`:124–128`; cap: **[não verificado]**).

## 12/11 12:29 · sessao 223e35d7f556 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · sonda · origem=Code

Contando `sonda_leitura:` como linha 1 do dado fornecido:

1. **N = 4 vínculos, 4 colaboradores distintos, 143 células e 51 células futuras.** Linha 4 do dado: `S VINCULOS_GERADORA 4 COLABS 4 CELULAS 143 CELULAS_FUTURAS 51`.

   | Vínculo × colaborador | Células | Futuras | Fonte |
   |---|---:|---:|---|
   | vinc965 × col305 | 92 | 0 | dado:5 |
   | vinc1351 × col476 | 31 | 31 | dado:6 |
   | vinc1353 × col203 | 10 | 10 | dado:7 |
   | vinc1473 × col789 | 10 | 10 | dado:8 |

2. Para **cada um dos quatro**, num dia dentro de `[data_inicio, ∞)`:

   - **`vinculo_do_dia` devolve o próprio vínculo**, quando a célula daquele dia o nomeia como geradora e ele está carregado em `escalas`: esse ramo só verifica os limites de datas; não exige `ativa=True`. Se a geradora faltar na lista, devolve `None`. ([alimentacao.py:269](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/escala/alimentacao.py:269))
   - **`vinculo_cobre_a_data` devolve `False`**: desativado e sem `data_fim` faz `tem_vigencia_declarada` retornar `False`, mesmo dentro desse intervalo. ([alimentacao.py:196](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/escala/alimentacao.py:196), [alimentacao.py:214](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/escala/alimentacao.py:214))

   Portanto, **os 4 vínculos compõem o N de linhas de CADASTRO previsto pelo aval**, quando o vigia passar a ler esse juiz; vinc965 também entra na medida, embora tenha zero células futuras. (dado:4–8; anexo `aval_respostas_1211_1230.txt`, item 1)

3. **Não há erro aritmético aparente:** as quatro linhas somam 143 células e 51 futuras, com quatro vínculos e quatro colaboradores distintos. (dado:4–8) **Erros internos da sonda, completude da consulta e término sem erro: [não verificado]** — o código da sonda e um status de saída não constam do dado apresentado.

## 12/11 12:27 · sessao 8e9e87c033ca · - · - · origem=Ronald (tmux)

Censo do PDF: **três casamentos ativos perdem a data**, com seis pontos de construção/consulta de coleções de apoio. A colisão efetiva em produção é [nao verificado]; a possibilidade decorre do código abaixo. A lei exige ler os dias da tela e o mesmo juiz: `app/docs/LEIS.md:85-86`.

| Local e função | Operandos e produtores | Classificação |
|---|---|---|
| `app/relatorios/pdf_espelho.py:475`, `_coletar_dados_espelho` | Chave `d['data']`; valor `set(d.get('orfas_grade') or [])`. `d` vem de `_espelho(...)` em `:474`; `orfas_grade` é publicado em `app/ponto/services/espelho.py:854`, calculado em `:699-700`, repassando `gd['orfas']` por `app/escala/utils.py:1113`. | **SEGURA somente na chave externa de data**; o set interno conserva apenas HH:MM. Não garante o dia civil de cada batida (`app/ponto/turnos.py:2952-2955`). |
| `app/relatorios/pdf_espelho.py:479`, `_coletar_dados_espelho` | Esquerda: `localtime(b.timestamp).strftime('%H:%M')`, com `b` vindo de `d['batidas']` em `:478`. Direita: `_orfas_por_data[d['data']]`, construído em `:475`. | **PERDE-A-DATA**: restringe à linha do turno, que pode conter batidas de D e D+1; todas as batidas de um turno recebem `t.data_turno` em `app/ponto/turnos.py:2952-2953`; a saída pode ter data civil distinta em `:322`. O matching não distingue esses dias nem segundos. |
| `app/relatorios/pdf_espelho.py:675-677`, `_build_story` | Chave de set: HH:MM de `b.timestamp`, para toda batida marcada `orfa` de todos os dias. `dados['dias']` vem do recorte em `:562` e retorno em `:604`; `b.orfa` é atribuído em `:568`. | **PERDE-A-DATA**: o set reúne o período inteiro sem data. |
| `app/relatorios/pdf_espelho.py:679`, `_build_story` | `_hh` do set em `:675`; `str(a)` de `dados['anomalias_mes']`, publicado de `resultado.anomalias` em `:607`. | **PERDE-A-DATA**: substring HH:MM de órfã de qualquer dia elimina anomalia de outro dia. Não é regex; é `in` textual. Anomalias incluem data no produtor `app/ponto/adaptador_motor.py:33-35` e `:40-42`, mas o PDF não a compara. |
| `app/relatorios/pdf_espelho.py:919-920`, `_build_story` | Chave de set: `localtime(b.timestamp).strftime('%H:%M')`, de batidas órfãs da linha atual. Marca produzida em `:568`; linha produzida em `app/ponto/services/espelho.py:774-777`. | **PERDE-A-DATA**: set da linha perde o instante; linha do turno admite D+1 (`app/ponto/turnos.py:2952-2953`). |
| `app/relatorios/pdf_espelho.py:923`, `_build_story` | `_abre_em`, HH:MM de `pp.entrada` em `:922`; `_hh_espurias`, set de `:919`. `pp` vem de `dia['turnos']` em `:921`; tela associa períodos por instante→pk→dia em `app/ponto/services/espelho.py:454-463`, publica `turnos_dia` em `:777`. | **PERDE-A-DATA**: uma órfã de D+1 com mesmo HH:MM da entrada de D pode suprimir `turno aberto` e todos os alertas em `:923-932`. Ocorrência concreta [nao verificado]. |

A cadeia normal de órfãs perde o instante antes de chegar ao PDF: `app/ponto/services/cartorio.py:736` serializa `dia['orfas']` como strings; `app/escala/services/leitor_celula.py:187` lê essas strings, `:390` cria a linha com data, `:420` anexa as órfãs; a tela chama esse leitor em `app/ponto/services/espelho.py:524-525` e indexa a data da linha em `:529-532`. O caminho builder também produz HH:MM sem data em `app/escala/utils.py:1856-1858`, depois de atribuir a órfã pela data civil em `:1844`.

Operações próximas que **preservam o instante**: `_entrada_espuria` compara `periodo.entrada in orfas_instantes` em `app/relatorios/pdf_espelho.py:178`; o set recebe `b.timestamp` em `:478`; a classificação usa `_b.timestamp in _orfas_instantes` em `:568`; os turnos abertos guardam `p.entrada` em `:518` e consultam entrada completa em `:576`. São comparações estruturalmente seguras quanto à data, mas o conteúdo de `_orfas_instantes` já depende do casamento defeituoso em `:479`.

Formatação **SO-EXIBICAO**, sem casar registros:

- `app/relatorios/pdf_espelho.py:691-692`: início/fim da escala do cabeçalho; `tipo_escala` nasce em `:307`, retorna em `:605` e é lido em `:689`.
- `app/relatorios/pdf_espelho.py:840-841`: horários das batidas não órfãs selecionadas em `:835`; entrega ao Flowable em `:852`.
- `app/relatorios/pdf_espelho.py:844-845`: horários das órfãs selecionadas em `:836`, usados no texto cinza.
- `app/relatorios/pdf_espelho.py:866`: início/fim de `dados['tipo_escala']`, lido em `:862`.
- `app/relatorios/pdf_espelho.py:914-915`: horário da batida como prefixo da procedência, vindo de `dia['batidas']` em `:910`.
- `app/relatorios/pdf_espelho.py:139` e `:165`: `hhmm` recebido em `Marcacoes.pares` (`:128`, chamado em `:852`) é medido e desenhado. A comparação de `:140` é de largura gráfica, não de horários.
- `app/relatorios/pdf_espelho.py:849`: strings de `celulas_isencao` são impressas; o PDF não as usa para casamento.

Os slices de `app/relatorios/pdf_espelho.py:680`, `:932` e `:933` truncam texto de anomalias/alertas/observações; **SO-EXIBICAO**, posteriores ao casamento. `:763` fatia badges, não horários. As ordenações são por datas (`:325`, `:363`, `:461-462`, `:563`, `:578`), não HH:MM. Não há regex, criação/consulta de objeto `time` nem slicing de string de hora no PDF lido (`app/relatorios/pdf_espelho.py:1-1073`).

`_h` opera durações, não hora do dia: comparações com zero em `app/relatorios/pdf_espelho.py:629`, com 60 em `:633` e com zero em `:635`; **SO-EXIBICAO**. Similarmente `saldo_banco_horas != 0` em `:774` compara quantidade, não relógio. Os carimbos em `:1006` e `:1053` incluem data completa.

## 12/11 12:27 · sessao 41ddbe2848a5 · - · - · origem=Ronald (tmux)

Para o script, as armadilhas comprovadas nesta cópia são:

- `ast.In` aparece em `Compare.ops`, sem linha própria; a ocorrência deve ser reportada como `Compare`, mostrando os dois operandos. As três comparações relevantes estão em `app/relatorios/pdf_espelho.py:479`, `app/relatorios/pdf_espelho.py:679` e `app/relatorios/pdf_espelho.py:923`.
- `_hh` exige propagação do iterável da comprehension: o conjunto nasce de `strftime` em `app/relatorios/pdf_espelho.py:675` e alimenta `for _hh in _hh_esp_periodo` em `app/relatorios/pdf_espelho.py:679`.
- `_abre_em` exige propagação pela atribuição de `app/relatorios/pdf_espelho.py:922` até a comparação em `app/relatorios/pdf_espelho.py:923`.
- Um `SetComp` deve herdar a derivação de `elt`, sem herdar automaticamente os testes `ifs`: `app/relatorios/pdf_espelho.py:477` produz timestamps completos; sua guarda perde a data em `app/relatorios/pdf_espelho.py:479`.
- Um `DictComp` deve analisar chave e valor separadamente: em `app/relatorios/pdf_espelho.py:475`, a chave é `d['data']`, embora o valor contenha horários.
- A regra nominal `"hora"` inclui durações e produz candidatos sem relação com data: `horas == 0` em `app/relatorios/pdf_espelho.py:629` e saldo do banco em `app/relatorios/pdf_espelho.py:774`.
- Buscar `"%H"` como substring também captura formatos com data preservada em `app/relatorios/pdf_espelho.py:1006` e `app/relatorios/pdf_espelho.py:1053`.
- AST local não resolve produtores interarquivo, tipos em execução, contratos do dia do turno nem ocorrência real em produção: [nao verificado].

Censo direto de `strftime` sem data: `app/relatorios/pdf_espelho.py:479`, `:675`, `:691`, `:692`, `:840`, `:844`, `:866` (duas chamadas), `:914`, `:919` e `:922`. A única chamada `.sort` usa data em `app/relatorios/pdf_espelho.py:563`; não encontrei chamada `.time()` ou regex no arquivo, mediante inspeção AST integral [nao verificado: evidência negativa sem linha única].

Os snippets somente leram o arquivo e escreveram em stdout. Nesta cópia, usar `python3`; o executável `python` não foi encontrado [nao verificado: condição do ambiente, sem arquivo:linha].

## 12/11 12:27 · sessao 16f591caa689 · - · - · origem=Ronald (tmux)

Leis: L-037 manda PDF desenhar `dias` da tela e L-038 proíbe regra própria de dia no PDF (`app/docs/LEIS.md:85-86`).

A tela preserva data/instante nos cruzamentos principais:

| Campo/operação | Produtor e uso | Conclusão |
|---|---|---|
| Período por entrada | `resultados_por_entrada[p.entrada] = p`, `app/ponto/services/espelho.py:424-425` | Chave é datetime completo, não HH:MM. |
| Turno aberto | `_abertos_ts.add(p.entrada)`, `app/ponto/services/espelho.py:426-427`; teste `_p.entrada in _abertos_ts`, `:476` | Compara instantes completos. |
| Batida → linha do dia | timestamp → pk em `app/ponto/services/espelho.py:455-458`; pk → dia do juiz em `:460-463`; batida agrupada por dia em `:579-582` | Casa com data pelo juiz. |
| Ordem das batidas | `sorted(..., key=lambda b: b.timestamp)`, `app/ponto/services/espelho.py:665` | Ordena datetime completo. |
| Anomalia → dia | parser DD/MM, anos candidatos e filtro da janela em `app/ponto/services/espelho.py:250-263`; consumo `data in _datas_anomalia` em `:668` | Casa por `date`, não HH:MM. Ano recuperado pela janela; mensagem sem ano em `app/ponto/adaptador_motor.py:35,42`. |
| Grade do dia | leitura e mapa `_grade_por_data[_d]` em `app/ponto/services/espelho.py:524-532`; acesso `_grade_por_data.get(data)` em `:684` | Seleção por data do dia/turno. |
| Órfãs apresentadas | `montar_realizado(_gd, ..., _ch_por_data.get(data))`, `app/ponto/services/espelho.py:699-700`; retorna `list(gd.get('orfas') or [])`, `app/escala/utils.py:1113`; publica `orfas_grade`, `app/ponto/services/espelho.py:854` | Lista HH:MM já pertencente à grade; tela não a cruza novamente com timestamp da batida. |
| Renderização órfã | `for o in dia.orfas_grade`, imprime `o`, `app/templates/ponto/espelho.html:240-241` | Só exibição. |
| Renderização marcação | imprime `c.hora`, `app/templates/ponto/espelho.html:232-237`; fallback `b.timestamp|date:"H:i"`, `:244-246` | Só exibição. |

Cadeia de produção de `orfas_grade`: ata grava strings de horas em `app/ponto/services/cartorio.py:736`; leitor repassa essas strings em `app/escala/services/leitor_celula.py:187-190`; anexa a uma grade cuja data é `_cur` em `:390` e cujas órfãs vêm de `_orfas_da_ata(ata)` em `:420`; tela indexa a grade por data em `app/ponto/services/espelho.py:528-532`; adaptador retorna as órfãs sem remontagem em `app/escala/utils.py:1113`; tela publica em `app/ponto/services/espelho.py:854`.

**Limite essencial:** “mesma linha da tela” não implica “mesma data civil”. O juiz põe todas as batidas reclamadas pelo turno em `t.data_turno` (`app/ponto/turnos.py:2949-2953`); as demais recebem a data civil (`:2954-2955`). Assim, uma linha do turno pode conter D e D+1; a saída do cross-meia-noite de `fim` pode morar em `fim+1` (`:2907-2908`). Logo o HH:MM da lista de órfãs não é identificador inequívoco de uma batida civil dessa linha (`app/escala/utils.py:1113`; `app/ponto/turnos.py:2953`).

O adaptador preserva datas nos dados estruturados: `e_ts = t.entrada.timestamp`, `s_ts = t.saida.timestamp`, períodos `(e_ts,s_ts)` e intervalos chaveados por `e_ts` (`app/ponto/adaptador_motor.py:37-44`). Textos de anomalia também incluem DD/MM HH:MM: órfã em `:33-35`, aberta em `:40-42`. Motor consome o adaptador em `app/ponto/motor_calculo_v2.py:530`, instala anomalias em `:2354-2357`, e seus períodos guardam `localizar(entrada)` completo, por exemplo em `:2751`, `:2878`, `:3216`.

Há comparações HH:MM dentro de adaptadores da grade, porém delimitadas pelo objeto do dia: chamado com mesmo tipo/hora em `app/escala/utils.py:1093-1103`, alimentado com `_ch_por_data.get(data)` pela tela em `app/ponto/services/espelho.py:699-700`; remapeamento de células `(tipo,hora)` reinicializa `por_chave` para cada `d` em `app/escala/services/leitor_celula.py:284-298`. Elas não provam identidade de uma batida por data civil; operam sobre marcos/células do dia indicado nesses laços.

## 12/11 12:19 · sessao 856fc304b1e9 · AUDITORIA-DE-TRAVAS-CORTE · verificador · origem=Code

REPROVADO

O delta cura o truncamento e reforça as transições dos casos 5 e 8. Persistem as divergências com o aval literal e a compatibilidade descoberta da cota antiga; além disso, um ramo do caso 6 ainda aceita aviso ausente. (`bin/disjuntor.py:218`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1185`; `bin/tests/test_disjuntor.sh:1222`, `bin/tests/test_disjuntor.sh:1250`, `bin/tests/test_disjuntor.sh:1306`)

Referências aos anexos contam desde a primeira linha de conteúdo. Execução local do selo e confirmação do intervalo pelo histórico Git: [nao verificado]. As mordidas abaixo são as do log fornecido.

**1. Destino de cada achado procedente da volta 2**

| Achado | O delta cura? | O selo morde? |
|---|---|---|
| Aviso truncado em nomes longos | **Sim.** DISJUNTOR/SOBE recebem `inteiro=True`; somente a linha livre final do SOBE fica limitada a 200 caracteres. (`bin/disjuntor.py:153`, `bin/disjuntor.py:218`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:1081`) | **Sim:** `B3-aviso-cortado`, `B3-sobe-cortado` e `B3-sobe-linha-sem-limite` caem no caso 6. (`item2v3_mutantes.log:30`, `item2v3_mutantes.log:45`, `item2v3_mutantes.log:60`) |
| Testemunha positiva da medida `lib['auto']` | Já estava curado; permanece exigindo trilha positiva, igualdade com o medidor e apresentação correspondente. (`bin/tests/test_disjuntor.sh:1284`, `bin/tests/test_disjuntor.sh:1292`) | Mutante continua declarado; sua execução nesta volta: [nao verificado]. (`bin/tests/test_disjuntor.sh:2036`) |
| Contrato FALHA × SOBE | Já estava conciliado: contrato pede SOBE e código produz SOBE. (`contrato_item2.txt:4`; `bin/disjuntor.py:1060`) | Caso 2 exige exatamente um SOBE; mutante específico FALHA/SOBE não consta entre os onze do item. (`bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Cota e medida com nomes distintos | Mantido: `auto_usada` governa a cota; `lib['auto']` conta eventos em 24 h. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:2029`, `bin/disjuntor.py:2033`, `bin/disjuntor.py:2058`) | Há verificações de ambas; a compatibilidade com a chave antiga continua sem caso/mutante específico. (`bin/tests/test_disjuntor.sh:1179`, `bin/tests/test_disjuntor.sh:1292`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Cota herdada em `r['auto']` ignorada | **Não.** O predicado continua lendo somente `auto_usada`; a migração existente trata o limiar, sem converter aquela chave. Existência desse estado em produção: [nao verificado]. (`bin/disjuntor.py:815`, `bin/disjuntor.py:832`, `bin/disjuntor.py:1185`) | Não há mordida demonstrada para esse caminho. (`bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Precedência das portas antigas | Já estava curado; mantém ordenação estável antes de AUTO. (`bin/disjuntor.py:1050`) | Caso 8 foi reforçado; mutante de precedência continua declarado, mas sua execução nesta volta: [nao verificado]. (`bin/tests/test_disjuntor.sh:1306`, `bin/tests/test_disjuntor.sh:2037`) |
| Disparo não condicionado literalmente a DESENHO | **Não.** Continua disparando pelo número de reprovações/voltas e retornando PARADA antes de examinar liberações. A classe só entra na porta posterior. (`aval_auditoria_de_travas_corte_1211.txt:4`; `bin/disjuntor.py:1031`, `bin/disjuntor.py:1040`, `bin/disjuntor.py:1074` [correção: validação da classe em `bin/disjuntor.py:774`]) | Não há mutante declarado que cobre essa divergência entre aval e contrato. (`contrato_item2.txt:2`; `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Segunda ocorrência pode sair pela porta antiga sem autoria Ronald | **Não.** ERRO-BOBO exige motivo, sem conferir autor; a cota restringe apenas AUTO. Isso preserva o contrato, mas mantém a divergência com o aval literal. (`contrato_item2.txt:2`; `aval_auditoria_de_travas_corte_1211.txt:4`; `bin/disjuntor.py:768`, `bin/disjuntor.py:1057`) | O caso 5 exige que essa porta continue liberando com cota gasta. (`bin/tests/test_disjuntor.sh:1227`, `bin/tests/test_disjuntor.sh:1230`) |
| Caso 5 podia passar sem provar liberação | **Sim.** Agora exige estado previamente disparado, LIBERA do tipo correto acrescentada e preservação da cota. (`bin/tests/test_disjuntor.sh:1220`, `bin/tests/test_disjuntor.sh:1228`, `bin/tests/test_disjuntor.sh:1232`) | **Sim:** `B3-c5-solta-sem-libera` passa a derrubar também o caso 5. Antes já derrubava o caso 6; não era sobrevivente da suíte inteira. (`item2v3_mutantes.log:67`, `item2v3_mutantes.log:73`) |
| Caso 8 podia passar sem segundo disparo/liberação final | **Sim.** Exige segundo disparo, evento AUTO e cota unitária finais. (`bin/tests/test_disjuntor.sh:1300`, `bin/tests/test_disjuntor.sh:1306`) | **Sim:** `B3-c8-sem-2o-disparo` passa de rc=0 para rc=8. (`item2v3_mutantes.log:81`, `item2v3_mutantes.log:89`) |
| Relógio fora da borda | **Não; permanece herdado.** Há `time.time()` dentro de `decide_b3` e `datetime.now()` dentro de `trilha`. (`bin/disjuntor.py:999`, `bin/disjuntor.py:217`) | Os cinco mutantes novos não atacam isso. (`bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2049`) |

**2. Quem escreve e quem lê; detalhe maior que 300**

| Campo/valor | Quem escreve/produz | Quem lê |
|---|---|---|
| Parâmetro `inteiro` | Padrão `False` na assinatura; `_anuncia` fornece o resultado de `ev in B3_AVISO_INTEIRO`. O espião encaminha `**kw`. (`bin/disjuntor.py:210`, `bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:1077`) | `trilha` escolhe corte em 300 ou ausência desse corte. O parâmetro não é persistido na linha. (`bin/disjuntor.py:217`, `bin/disjuntor.py:218`) |
| `B3_AVISO_INTEIRO` | Constante `('DISJUNTOR', 'SOBE')`. (`bin/disjuntor.py:153`) | `_anuncia`; os mutantes retiram o encaminhamento ou SOBE da tupla. (`bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2042`) |
| `B3_SOBE_LINHA` | Constante `200`. (`bin/disjuntor.py:154`) | Montagem do SOBE e assert do sufixo limitado; mutante remove o corte. (`bin/disjuntor.py:1062`; `bin/tests/test_disjuntor.sh:1278`, `bin/tests/test_disjuntor.sh:2043`) |
| Detalhe DISJUNTOR B3 | `decide_b3` monta obra, motivo e recado; `_anuncia → trilha` grava após persistência, normalizando espaços. (`bin/disjuntor.py:1036`, `bin/disjuntor.py:1038`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:217`) | `ler()` conta pelo prefixo; selo procura o aviso completo e observa evento/trava. (`bin/disjuntor.py:2059`; `bin/tests/test_disjuntor.sh:1234`, `bin/tests/test_disjuntor.sh:1265`, `bin/tests/test_disjuntor.sh:1086`) |
| Detalhe SOBE B3 | `decide_b3` monta obra, SHA, cota, instrução e linha limitada ao final; passa pela mesma porta de anúncio. (`bin/disjuntor.py:1060`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:1081`) | `conta()` e listas `sobe/sobe2` do selo. `ler()` não contabiliza SOBE. (`bin/tests/test_disjuntor.sh:1165`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1276`; `bin/disjuntor.py:2035`) |

**Os leitores identificados em `bin/` continuam compatíveis com o detalhe ampliado:**

- `ler()` percorre linhas completas, filtra pelo token do evento e conta DISJUNTOR usando `p[3]`; o sufixo longo não muda essa posição. (`bin/disjuntor.py:2039`, `bin/disjuntor.py:2041`, `bin/disjuntor.py:2060`)
- `_censo_24h()` também percorre linhas completas; seu regex ancorado descarta DISJUNTOR/SOBE. (`bin/disjuntor.py:2105`, `bin/disjuntor.py:2147`, `bin/disjuntor.py:2149`)
- ESTADO e CLI usam `ler/linha`. A referência do hook ao caminho apenas o exclui como prova de espera. (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`; `bin/disjuntor.py:2403`; `bin/hook_stop_fila1.py:666`)
- Os selos usam leitura integral por linha, `grep` ou `tail`, sem teto de 300. Isso inclui o selo de censo fora do diff. (`bin/tests/test_disjuntor.sh:96`, `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1236`; `bin/tests/test_censo_antes_da_vaga.sh:158`, `bin/tests/test_censo_antes_da_vaga.sh:525`)

**3. Segunda AUTO, DESENHO, rearme e reinício**

- **Segunda AUTO sozinha, com `auto_usada` preservada: não libera.** Registra SHA recusado em `auto_sobe`, deduplica SOBE e continua sem consumir a linha. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:1063`, `bin/disjuntor.py:1069`)
- **Com estado antigo apenas em `auto`: pode aceitar outra AUTO.** Ausência de `auto_usada` é lida como zero, conforme o próprio contrato. Ocorrência em produção: [nao verificado]. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:1065`; `contrato_item2.txt:4`)
- **`classe=DESENHO` não libera pela porta nova.** Um motivo dizendo “erro de desenho” sob rótulo FORMA pode passar: o reconhecedor valida o rótulo e motivo alfanumérico. Classificação real em produção: [nao verificado]. (`bin/disjuntor.py:148`, `bin/disjuntor.py:774`)
- **Rearme e reinício com JSON preservado não zeram a cota.** Rearme altera outros campos; o carregamento seguinte recupera o objeto persistido. Arquivo ausente inicia `{}`. (`bin/disjuntor.py:1148`, `bin/disjuntor.py:1150`, `bin/disjuntor.py:377`, `bin/disjuntor.py:380`)
- **Uma linha antiga ERRO-BOBO pode liberar com cota gasta**, sem comprovar autoria Ronald; isso não é uma segunda AUTO aceita. (`bin/disjuntor.py:768`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1057`)

**4. Espião do caso 5 de `b3_v4`**

**Sim, preserva a prova anterior:** tenta adquirir a trava por outro descritor, registra se estava ocupada e exige presença de DISJUNTOR/LIBERA/REARMA antes do `all(...)`. `**kw` é encaminhado intacto à função original. Não prova SOBE nem verifica diretamente o valor de `inteiro`. (`bin/tests/test_disjuntor.sh:1067`, `bin/tests/test_disjuntor.sh:1071`, `bin/tests/test_disjuntor.sh:1077`, `bin/tests/test_disjuntor.sh:1086`, `bin/tests/test_disjuntor.sh:1087`)

**5. Assert vazio e mordida dos mutantes**

**Casos 5 e 8:** os vazios apontados foram curados pelas testemunhas de transição descritas acima. (`bin/tests/test_disjuntor.sh:1222`, `bin/tests/test_disjuntor.sh:1228`, `bin/tests/test_disjuntor.sh:1306`)

**Caso 6: ainda há ramo vazio.** O retorno de `dispara('OBRA-F')` é ignorado; `ultima()` pode retornar `''`. Nesse caso, “`AUTO ja usada` ausente” passa, e `mensagem_b3` ainda ensina AUTOLIBERA sem exigir registro da obra. Portanto esse ramo aceita ausência de disparo/aviso F. As provas novas de textos longos exigem eventos positivos e não passam integralmente vazias. (`bin/tests/test_disjuntor.sh:1240`, `bin/tests/test_disjuntor.sh:1245`, `bin/tests/test_disjuntor.sh:1250`, `bin/tests/test_disjuntor.sh:1265`, `bin/tests/test_disjuntor.sh:1278`; `bin/disjuntor.py:1202`)

| Mutante novo | Mordida no log final |
|---|---|
| `B3-aviso-cortado` | Caso 6, rc=6. (`item2v3_mutantes.log:30`, `item2v3_mutantes.log:31`) |
| `B3-sobe-cortado` | Caso 6, rc=6. (`item2v3_mutantes.log:45`, `item2v3_mutantes.log:46`) |
| `B3-sobe-linha-sem-limite` | Caso 6, rc=6. (`item2v3_mutantes.log:60`, `item2v3_mutantes.log:61`) |
| `B3-c5-solta-sem-libera` | Casos 5/6, rc=5. (`item2v3_mutantes.log:73`, `item2v3_mutantes.log:74`) |
| `B3-c8-sem-2o-disparo` | Caso 8, rc=8. (`item2v3_mutantes.log:88`, `item2v3_mutantes.log:89`) |

**Nenhum dos cinco novos sobrevive no log; o controle passa.** A execução nesta volta dos seis antigos e de `b3_v4`: [nao verificado]. (`item2v3_mutantes.log:15`, `item2v3_mutantes.log:16`; `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2039`)

**Contrato:** continuam oito casos numerados, agora com mais cenários internos; há onze mutantes do item. “8 casos” permanece correto; “6 mutantes” virou divergência documental relevante para rastrear cobertura, sem alterar por si só a regra operacional. (`contrato_item2.txt:5`; `bin/tests/test_disjuntor.sh:1180`, `bin/tests/test_disjuntor.sh:1307`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`)

**6. Classes de reprovação**

| Classe | Resultado |
|---|---|
| Segundo escritor fora da porta | Não identificado para a cota operacional: acréscimo específico em `decide_b3`, persistência por `_com_estado`. (`bin/disjuntor.py:1065`, `bin/disjuntor.py:1089`, `bin/disjuntor.py:390`) |
| Relógio fora da borda | **Presente, herdado:** fallback em `decide_b3`; wrapper chega sem instante explícito; `trilha` captura seu próprio horário. (`bin/disjuntor.py:999`, `bin/disjuntor.py:1172`, `bin/disjuntor.py:217`; `bin/codex_pergunta.sh:378`) |
| Selo sem mordida | Nenhum sobrevivente entre os cinco novos demonstrados; permanece a checagem vazia de aviso F. Mordida atual dos seis antigos: [nao verificado]. (`item2v3_mutantes.log:30`, `item2v3_mutantes.log:45`, `item2v3_mutantes.log:60`, `item2v3_mutantes.log:73`, `item2v3_mutantes.log:88`; `bin/tests/test_disjuntor.sh:1250`) |
| Campo com dois significados | Não nos campos atuais: cota e medida permanecem distintas. A chave antiga continua sem conversão. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:2033`, `bin/disjuntor.py:2058`, `bin/disjuntor.py:815`) |
| Ordem de trava | Não identifiquei inversão introduzida: gravação e anúncio permanecem sob a trava do estado; wrapper/triagem tomam resposta antes de estado. (`bin/disjuntor.py:370`, `bin/disjuntor.py:391`, `bin/disjuntor.py:402`, `bin/disjuntor.py:405`, `bin/disjuntor.py:908`, `bin/disjuntor.py:940`; `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`) |

## 12/11 12:17 · sessao 9aa9d65cb110 · - · - · origem=Ronald (tmux)

Os achados persistentes da volta 2 **não foram curados por este delta**: o disparo continua condicionado apenas ao número de reprovações/voltas, e retorna PARADA nessa chamada; a classe só é examinada posteriormente na porta AUTOLIBERA. (`bin/disjuntor.py:1031`, `bin/disjuntor.py:1040`, `bin/disjuntor.py:774`)

A exclusividade “segunda ocorrência só Ronald” também permanece falsa pela porta antiga: `ERRO-BOBO OBRA-A: motivo` exige apenas motivo alfanumérico, sem autoria; essas candidatas precedem AUTO e a cota só restringe `tipo == 'auto'`. Isso é preservação expressa da porta antiga pelo contrato, mas continua divergindo da leitura literal do aval já apontada na volta 2. (`bin/disjuntor.py:768`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1057`; `contrato_item2.txt:2`; `aval_auditoria_de_travas_corte_1211.txt:4`)

Para a pergunta 3:

- **Estado atual com `auto_usada`: segunda AUTOLIBERA sozinha não libera.** Acrescenta SHA a `auto_sobe` uma vez, anuncia SOBE e continua sem consumir SHA; o único acréscimo a `auto_usada` fica depois dessa guarda. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:1063`, `bin/disjuntor.py:1065`, `bin/disjuntor.py:1069`)
- **DESENHO pela porta nova:** rótulo `classe=DESENHO` é recusado porque as classes aceitas são FORMA/ERRO-BOBO; o texto “erro de desenho” sob rótulo FORMA pode passar, pois o reconhecedor valida rótulo e presença de letra/dígito, não a natureza do defeito. Classificação real em produção: `[nao verificado]`. (`bin/disjuntor.py:148`, `bin/disjuntor.py:774`)
- **Rearme não zera cota:** altera apenas `rearmado`, `reprovados`, `codex`, `disparado`. A decisão subsequente continua lendo `auto_usada`. (`bin/disjuntor.py:1148`, `bin/disjuntor.py:1150`, `bin/disjuntor.py:1185`)
- **Reinício com JSON preservado não zera cota:** a transição carrega JSON existente e o grava inteiro; carregamento de consulta faz o mesmo. Arquivo ausente inicia `{}`; isto é perda/ausência de estado, não efeito necessário do reinício. (`bin/disjuntor.py:377`, `bin/disjuntor.py:380`, `bin/disjuntor.py:390`, `bin/disjuntor.py:411`, `bin/disjuntor.py:414`)
- **Compatibilidade herdada continua descoberta:** estado com `auto=['sha-antigo']` sem `auto_usada` é lido como cota livre, podendo aceitar nova AUTO; o predicado lê só `auto_usada`, a migração existente só trata limiar/reprovações e não converte `auto`. O contrato explicitamente chama ausência de `auto_usada` de zero; existência de tal estado em produção: `[nao verificado]`. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:815`, `bin/disjuntor.py:832`, `bin/disjuntor.py:1065`; `contrato_item2.txt:4`)

Classes da pergunta 6:

| Classe | Evidência |
|---|---|
| Segundo escritor da cota fora da porta | Não identificado no código operacional pesquisado: acréscimo específico só em `decide_b3`; persistência genérica por `_com_estado`. Os escritores adicionais em `bin/tests` são fixtures. (`bin/disjuntor.py:1065`, `bin/disjuntor.py:1089`, `bin/disjuntor.py:390`; `bin/tests/test_disjuntor.sh:1153`) |
| Relógio fora da borda | Persiste, herdado: `decide_b3` chama `time.time()` se não recebe instante; wrapper chama `b3_parada → estado_b3` sem instante. `trilha` também lê `datetime.now()`. Stop e PreToolUse fornecem instante capturado na borda. (`bin/disjuntor.py:999`, `bin/disjuntor.py:1172`, `bin/disjuntor.py:217`; `bin/codex_pergunta.sh:378`; `bin/hook_stop_fila1.py:1198`, `bin/hook_stop_fila1.py:1204`; `bin/disjuntor.py:1951`, `bin/disjuntor.py:1834`) |
| Campo com dois significados | Nos nomes atuais não: cota cumulativa `auto_usada` versus medida de LIBERA(auto) em 24h `lib['auto']`; compatibilidade da chave antiga segue sem conversão. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:2029`, `bin/disjuntor.py:2033`, `bin/disjuntor.py:2056`) |
| Ordem de trava | Não identifiquei inversão introduzida: estado é travado antes de ler/mutar/gravar/anunciar; anúncio ocorre antes de fechar FD. Wrapper e triagem tomam resposta antes de estado. (`bin/disjuntor.py:370`, `bin/disjuntor.py:385`, `bin/disjuntor.py:391`, `bin/disjuntor.py:402`, `bin/disjuntor.py:405`; `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`; `bin/disjuntor.py:908`, `bin/disjuntor.py:940`) |

Cura efetiva do aviso truncado: `_anuncia` passa `inteiro=True` para DISJUNTOR/SOBE; `trilha` deixa de aplicar corte de 300 nesses chamados; SOBE preserva instrução antes da linha livre, e somente essa linha fica limitada a 200. (`bin/disjuntor.py:153`, `bin/disjuntor.py:154`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:218`, `bin/disjuntor.py:1060`, `bin/disjuntor.py:1062`)

## 12/11 12:17 · sessao 5f42930059da · - · - · origem=Ronald (tmux)

- **Espião `b3_v4` caso 5 preservado.** `**kw` segue intacto para `orig`; a tentativa de `flock(LOCK_EX|LOCK_NB)` em descritor separado continua detectando trava ocupada. O assert exige presença dos três eventos antes de `all(...)`, impedindo `all([])`. Prova DISJUNTOR/LIBERA/REARMA sob trava; SOBE não está no filtro. (`bin/tests/test_disjuntor.sh:1067`, `bin/tests/test_disjuntor.sh:1071`, `bin/tests/test_disjuntor.sh:1077`, `bin/tests/test_disjuntor.sh:1086`, `bin/tests/test_disjuntor.sh:1087`)

- **Caso 5 corrigido:** controles exigem estado previamente disparado, incremento LIBERA do tipo certo e cota apropriada; terceiro controle exige preservação exata da cota. Lista obrigatoriamente contém três resultados. (`bin/tests/test_disjuntor.sh:1220`, `bin/tests/test_disjuntor.sh:1222`, `bin/tests/test_disjuntor.sh:1228`, `bin/tests/test_disjuntor.sh:1232`)

- **Caso 8 corrigido:** segundo disparo deve retornar verdadeiro, AUTO final deve produzir evento e cota unitária. (`bin/tests/test_disjuntor.sh:1300`, `bin/tests/test_disjuntor.sh:1303`, `bin/tests/test_disjuntor.sh:1306`)

- **Caso 6 tem ramo residual vazio:** ignora retorno de `dispara('OBRA-F')`; `ultima` pode devolver `''`, e o assert aceita ausência de aviso porque só exige ausência da marca de cota gasta, além da instrução em `mensagem_b3`, que funciona mesmo sem registro da obra. Assim pode passar sem disparo/aviso F. As novas provas longas, entretanto, exigem disparos e SOBE positivos; não passam integralmente vazias. (`bin/tests/test_disjuntor.sh:1240`, `bin/tests/test_disjuntor.sh:1245`, `bin/tests/test_disjuntor.sh:1250`, `bin/disjuntor.py:1202`, `bin/disjuntor.py:1205`, `bin/tests/test_disjuntor.sh:1265`, `bin/tests/test_disjuntor.sh:1278`)

- **Inventário:** seguem oito casos numerados; há onze mutantes item 2, seis antigos e cinco novos. Logo “8 casos” permanece correto; “6 mutantes” está desatualizado, divergência documental sem alteração das regras operacionais. (`bin/tests/test_disjuntor.sh:1180`, `bin/tests/test_disjuntor.sh:1307`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2039`, `bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2049`; `contrato_item2.txt:5`)

- **Mordidas:** o log anexado mostra os cinco novos mutantes derrubados — três no caso 6, um no 5 e um no 8 — e controle rc=0; execução atual dos seis antigos e de `b3_v4`: `[nao verificado]`. O harness exige âncora única, controle sem mutação e falha se o caso aceitar a mutação. (`bin/tests/test_disjuntor.sh:1715`, `bin/tests/test_disjuntor.sh:1719`, `bin/tests/test_disjuntor.sh:1723`, `bin/tests/test_disjuntor.sh:1741`; localizações exatas no log anexado: `[nao verificado]`)

## 12/11 12:17 · sessao ad20865149a1 · - · - · origem=Ronald (tmux)

Inventário e compatibilidade dos leitores, por inspeção somente leitura:

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `inteiro` | Padrão `False` na assinatura; `_anuncia` passa `ev in B3_AVISO_INTEIRO`. O espião do selo recebe e encaminha `**kw`. (`bin/disjuntor.py:210`, `bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:1067`, `bin/tests/test_disjuntor.sh:1077`) | `trilha` escolhe o limite `None` ou `300`; nenhum outro consumidor semântico encontrado. (`bin/disjuntor.py:218`) |
| `B3_AVISO_INTEIRO` | Tupla constante `('DISJUNTOR', 'SOBE')`. (`bin/disjuntor.py:153`) | `_anuncia` determina a opção de escrita; mutantes removem o encaminhamento ou o SOBE. (`bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2042`) |
| `B3_SOBE_LINHA` | Constante `200`. (`bin/disjuntor.py:154`) | Construção de SOBE corta somente o texto livre final; selo exige linha maior que o limite e sufixo cortado. (`bin/disjuntor.py:1062`; `bin/tests/test_disjuntor.sh:1278`, `bin/tests/test_disjuntor.sh:1279`, `bin/tests/test_disjuntor.sh:2043`) |
| Detalhe DISJUNTOR B3 | `decide_b3` produz prefixo, motivo e `_recado_auto`; `_anuncia` entrega a `trilha`, que normaliza espaços e grava uma linha. (`bin/disjuntor.py:1036`, `bin/disjuntor.py:1038`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:217`) | `ler`, auxiliares de teste `_tem`/`ultima`, espião B3 v4. (`bin/disjuntor.py:2059`; `bin/tests/test_disjuntor.sh:96`, `bin/tests/test_disjuntor.sh:1234`, `bin/tests/test_disjuntor.sh:1086`) |
| Detalhe SOBE | `decide_b3` produz obra, SHA, cota e instrução, seguidos de `ln[:200]`; mesma porta de anúncio/escrita. (`bin/disjuntor.py:1060`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:1081`) | Selo `conta` e listas `sobe`/`sobe2`; `ler` não enumera SOBE, `_censo_24h` o descarta. (`bin/tests/test_disjuntor.sh:1165`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1276`; `bin/disjuntor.py:2035`, `bin/disjuntor.py:2105`, `bin/disjuntor.py:2148`) |

**Todos os leitores diretos identificados em `bin/` suportam detalhe maior que 300:** a escrita continua em uma única linha; nenhuma leitura identificada limita bytes/caracteres a 300. (`bin/disjuntor.py:217`, `bin/disjuntor.py:2039`, `bin/disjuntor.py:2147`; `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1236`)

- `ler()` percorre linhas completas, separa tokens e usa data/hora/evento; para DISJUNTOR conta `p[3]`, portanto o sufixo ampliado não muda a leitura. SOBE não participa da enumeração. (`bin/disjuntor.py:2035`, `bin/disjuntor.py:2040`, `bin/disjuntor.py:2044`, `bin/disjuntor.py:2059`)
- `_censo_24h()` percorre linhas completas; o regex ancorado aceita apenas `CENSO ...` e `NEGOU DISJUNTOR CENSO:`, portanto ignora os dois eventos ampliados. (`bin/disjuntor.py:2105`, `bin/disjuntor.py:2146`, `bin/disjuntor.py:2148`)
- `gerar_estado.py` e CLI `--linha` consomem a derivação `ler/linha`, sem leitor alternativo de detalhe. (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`; `bin/disjuntor.py:2402`, `bin/disjuntor.py:2403`)
- Os leitores do selo disjuntor usam `grep`, `tail`, busca por substring ou leitura integral por linha; o espião observa evento/trava e encaminha o detalhe. (`bin/tests/test_disjuntor.sh:96`, `bin/tests/test_disjuntor.sh:97`, `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1236`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1276`, `bin/tests/test_disjuntor.sh:1285`, `bin/tests/test_disjuntor.sh:1705`)
- O outro selo que lê diretamente a trilha é `test_censo_antes_da_vaga.sh`: `grep`/`tail` procuram eventos CENSO; o filtro seleciona os eventos antes de `tail`. Não depende do comprimento do detalhe B3. (`bin/tests/test_censo_antes_da_vaga.sh:114`, `bin/tests/test_censo_antes_da_vaga.sh:158`, `bin/tests/test_censo_antes_da_vaga.sh:249`, `bin/tests/test_censo_antes_da_vaga.sh:275`, `bin/tests/test_censo_antes_da_vaga.sh:314`, `bin/tests/test_censo_antes_da_vaga.sh:525`, `bin/tests/test_censo_antes_da_vaga.sh:543`)
- A referência do hook a `disjuntor.log` apenas o inclui entre arquivos que não provam espera; não interpreta seu detalhe. (`bin/hook_stop_fila1.py:664`, `bin/hook_stop_fila1.py:666`)

**Espião do caso 5 B3 v4:** continua provando presença de DISJUNTOR/LIBERA/REARMA e que a trava estava ocupada em todas essas chamadas. A exigência de conjunto exato impede o `all([])`. `**kw` apenas preserva a assinatura nova e chega à função original; o teste não verifica diretamente o valor de `inteiro`, obrigação coberta pelos novos casos de texto longo. (`bin/tests/test_disjuntor.sh:1067`, `bin/tests/test_disjuntor.sh:1071`, `bin/tests/test_disjuntor.sh:1077`, `bin/tests/test_disjuntor.sh:1086`, `bin/tests/test_disjuntor.sh:1087`, `bin/tests/test_disjuntor.sh:1252`, `bin/tests/test_disjuntor.sh:1278`)

## 12/11 12:11 · sessao ccf3a0772d66 · SAIDA-NOTURNA-NO-DIA-CERTO · censo · origem=Code

Critério: censo estático dos caminhos operacionais, templates e comandos. Testes e cópias históricas não entram nas contagens; mecanismos genéricos e ferramentas da sombra aparecem separados. A identificação do hash e os estados de produção de #51467/#56641 são os fornecidos no texto; confirmação independente: `[nao verificado]`. [nao verificado]

**1. Escritores de `PerguntaDisputa.pergunta`**

São **oito expressões de interpolação de data: quatro `.format` e quatro `.replace`, em sete locais lógicos**. Os dois ramos de `texto_pergunta_com_data` contam como duas expressões, uma função (`app/chamados/catalogo/query.py:64,66`).

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/chamados/catalogo/query.py:64` | Substitui `{data}` pelo ISO convertido para `dd/mm/aaaa`. | Data de `contexto_disputa[motivo]['data']`, lida em `:58,59`; não deriva o dia civil do marco. Chamadores: `app/chamados/services/disputa_emissao.py:345,354`. |
| `app/chamados/catalogo/query.py:66` | Na exceção, substitui `{data}` pela string original. | Mesma origem; nesse ramo não valida nem converte a data. |
| `app/chamados/services/disputa_emissao.py:879` | Formata pergunta para cada marco faltante. | Usa `dia` da tupla de `_marcos_faltantes`, exigindo igualdade com `data_evento` em `:873,874`. A tupla usa **data da célula**, inclusive quando a célula é a da véspera: `app/chamados/services/declaracao_texto.py:109,139`. |
| `app/chamados/services/disputa_emissao.py:1151` | Formata pergunta de intervalo. | Usa `_m['data']` de `chamado.contexto_json.marcos_faltantes`, convertido em `:1143,1148`. Um produtor comprovado grava ali o **dia civil do marco encadeado**, potencialmente D+1: `app/ponto/management/commands/detectar_intervalo_ausente.py:77,78,82`. |
| `app/chamados/services/disputa_emissao.py:1208` | Formata pergunta do grupo com batida de origem. | Usa `data_evento` resolvido em `:1205`: **dia do turno da batida**, pelo resolvedor em `app/escala/servico_jornada.py:495,509`. |
| `app/chamados/services/disputa_emissao.py:1246` | Formata pergunta sem batida de origem. | Usa `_ddc(ch.contexto_json)`, em `:1232`. A hierarquia aceita `data_turno`, `data_batida`, data de `entrada_timestamp`, `pendentes` e `horario_previsto`: `app/chamados/data_evento.py:16,27,31,39`. Não tem uma única origem temporal. |
| `app/chamados/services/declaracao_texto.py:374` | Substitui `{data}` ao criar pergunta da declaração. | Usa `dia` escolhido pelo casamento da hora em `:341,345`; origem na data da célula de `_marcos_faltantes`, em `:109,139`. |
| `app/chamados/services/declaracao_texto.py:664` | Substitui `{data}` ao aplicar proposta textual. | Usa `p['dia']`; proposta deriva de `_marcos_faltantes` e `_casar_hora` em `:615,625,628,635`. |
| `app/chamados/services/acoes_disputa.py:265` | Formata confirmação administrativa com `% label_motivo`. | **Sem data no texto.** Cria conversa em `:262,268`; a conversa recebe dia civil da emissão em `app/chamados/models.py:2802`. |
| `app/chamados/services/disputa_emissao.py:1374` | Copia texto de catálogo da âncora ou literal de fallback. | **Sem interpolação de data.** Cria conversa `ancora_ausente`, tipo `data_hora`, em `:1369,1377`. |
| `app/chamados/services/disputa_emissao.py:440,447` | Encaminha `texto` para `abrir_conversa` ou `abrir`. | O texto já foi produzido em `:345,354`; a data do nascimento passa por outra derivação em `:371,424,451`. Não recompõe o texto depois disso. |
| `app/chamados/services/disputa_emissao.py:840` | `_criar_pergunta_se_couber` encaminha pergunta e data recebidas. | Não reformata. |
| `app/chamados/models.py:3015,3017,3018` | `_nascer` instancia com `**campos`, atribui a data e insere. | Persistência comum do texto recebido. No reuso retorna a pergunta existente em `:3009`, sem reescrever seu texto. |
| `app/chamados/catalogo/perguntas.py:49`; `app/chamados/models.py:3566` | Definem “Confirme o horário da saída no dia {data}”. | São **templates**, não formatadores. A referência raspada `models.py:3564-3565` não identifica o formatador nesta cópia. |
| `bin/sombra_regras_pessoais.py:85`; `bin/sombra_mascara_pessoal.py:54,55,57,62,64` | Na cópia sombra, lê, mascara e grava `pergunta` por `bulk_update`. | Escritor adicional fora da emissão do domínio; não projeta nenhuma data. Transformação definida em `bin/sombra_regras_pessoais.py:29,33`. |

**2. Leitores de `PerguntaDisputa.pergunta`**

Há **20 ocorrências explícitas de leitura do campo: 14 Python e seis templates**, discriminadas abaixo. Excluir `__str__` e o comando smoke produz 18; esse recorte deixa leitores de fora (`app/chamados/models.py:3547`; `app/chamados/management/commands/smoke_chamados.py:108`). Além dessas 20, existem leitura genérica, leitores da sombra e propagação indireta.

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/api/views.py:1155` | API de chamados copia `p.pergunta`. | Texto cru; data enviada separadamente em `:1153,1154`. |
| `app/api/views.py:2551` | API de perguntas do app monta `_item`. | Texto cru para pendentes/histórico. |
| `app/api/views_mensageria.py:564` | API da fila de validação copia pergunta. | Texto cru; `data_evento` separado em `:568`. |
| `app/ponto/services/dia_do_colab.py:69` | Consulta do dia entrega pergunta. | Texto cru; seleção por `data_evento=data` em `:65`. |
| `app/chamados/views_cobrar.py:291` | Monta amostra de validação em lote. | Texto cru. |
| `app/chamados/canal.py:81` | Monta pergunta muda com resposta no fio. | Texto cru; correlação por colaborador/data em `:71,79`. |
| `app/chamados/services/explicador.py:71` | Monta dict explicativo. | Conserva texto cru; estado, ação e dia ficam em campos separados. |
| `app/chamados/services/disputa_emissao.py:1549` | Copia pergunta para ocorrência de resposta. | Envolve texto cru com tag/resposta; grava em `:1555,1556`. |
| `app/chamados/services/validacao.py:599` | Copia pergunta para ocorrência de recusa. | Envolve texto cru com autor, resposta anterior e motivo. |
| `app/chamados/templatetags/chamados_extras.py:11` | Detecta pergunta “cega” com regex de data. | Inspeciona o texto gravado; não o recompõe. |
| `app/chamados/management/commands/auditar_disputas.py:74` | Procura data no texto. | Regex sobre o campo cru. |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:33` | Procura ocorrência correspondente. | Usa o texto cru como substring `Pergunta: ...`. |
| `app/chamados/management/commands/smoke_chamados.py:108` | Verifica `{data}` residual. | Leitura operacional do campo persistido. |
| `app/chamados/models.py:3547` | `__str__`. | Prefixa motivo e corta texto cru em 60 caracteres. |
| `app/templates/chamados/partials/_card_atendimento.html:93` | Cartão da resposta. | Exibe texto cru. |
| `app/templates/chamados/partials/_card_atendimento.html:175` | Cartão do histórico. | Exibe texto cru. |
| `app/templates/chamados/partials/_pergunta_card.html:28` | Cartão de supervisão/Central. | Exibe texto cru. |
| `app/templates/chamados/partials/modal_fio.html:160` | Modal do fio. | Exibe texto cru; data do fato separada em `:167`. |
| `app/templates/chamados/partials/_lote_preview.html:24` | Prévia de lote. | Texto cru com `truncatechars:60`. |
| `app/templates/colaboradores/partials/painel_colaborador.html:197` | Painel do colaborador, bloco “no app”. | Exibe texto cru. |
| `app/chamados/services/foto_e4b.py:251,272` | **Leitor genérico:** fotografa todos os campos concretos. | Inclui `pergunta` por `value_from_object`; não renderiza nem recompõe. |
| `bin/sombra_mascara_pessoal.py:54`; `bin/sombra_regras_pessoais.py:85` | **Sombra:** lê para mascarar. | Não é apresentação da pergunta. |
| `bin/sombra_selo_pessoal.py:74,77`; `bin/sombra_regras_pessoais.py:85` | **Sombra:** SQL confere se o texto foi mascarado. | Leitura adicional por regra dinâmica de campos. |
| `app/templates/chamados/partials/modal_fio.html:439` | **Indireto:** exibe dict do explicador. | Texto copiado por `app/chamados/services/explicador.py:71`, truncado. |
| `app/chamados/views_cobrar.py:180,186` | **Indireto:** repassa dict do canal. | Origem em `app/chamados/canal.py:81`. |
| `mensageria/nucleo/ferramentas.py:119` | **Indireto:** copia pergunta recebida da API. | Não recompõe. |
| `app/relatorios/pdf_chamados.py:145,147` | **PDF indireto:** imprime `fup.conteudo`. | Pode imprimir a pergunta copiada pelas ocorrências de resposta/recusa; limita conteúdo a 1.000 caracteres. Fontes dessas cópias: `app/chamados/services/disputa_emissao.py:1549,1556`; `app/chamados/services/validacao.py:598,600`. |
| `app/chamados/catalogo/query.py:45,52,64,66` | Existe `texto_pergunta_com_data`. | É interpolador do **catálogo no emissor**, não projeção comum dos leitores. Os acessos acima leem diretamente o campo ou sua cópia. |
| Renderizador específico do cliente nativo/PWA | Execução efetiva e eventual recomposição no cliente. | `[nao verificado]`; os pontos comprovados nesta cópia são APIs e templates acima. |

**3. `VIA_FATO_OCUPADO`: escritores, leitores e colisão**

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/chamados/models.py:2488` | Declara `'fato_ocupado'`. | Definição da constante. |
| `app/chamados/services/materializacao.py:1095,1097,1099,1105` | Único chamador operacional encontrado que solicita gravação dessa via. | Predicado exato: `p.materializada_em is None and codigo == 'COLISAO'`. **Não consulta o marco cobrado nem sua lâmpada.** |
| `app/chamados/catalogo/recusa.py:66,79,80,81,82,83` | Classifica `COLISAO` pela substring `colisao tipo-oposto`. | Classificação textual, sem comparação de marcos. |
| `app/chamados/models.py:3179,3186,3187,3189,3191` | `carimbar_identidade` persiste a solicitação. | Grava via, instante, autor e, quando fornecido, motivo da recusa. Não pergunta ao C6 nem limpa resposta/validação. |
| `app/ponto/registro_batida.py:94,138,139,145,150` | Produz a recusa original de colisão. | Existe batida **oposta, não retratada**, na janela inclusiva `timestamp ± g_op`, e a candidata não casa com marco próprio. Sem autoridade humana, recusa. Não recebe a pergunta nem consulta sua lâmpada. |
| `app/ponto/registro_batida.py:21,28,32` | Define a janela de colisão. | 300 segundos com escala/marcos no dia civil local da candidata; senão 60 segundos. |
| `app/ponto/registro_batida.py:35,42,49` | `_casa_marco_proprio` excepciona a colisão por geometria. | Procura algum marco previsto do mesmo tipo, no dia local ou véspera, a até cinco minutos. Portanto a origem não olha **somente** vizinhança, mas tampouco identifica o marco da `PerguntaDisputa`. |
| `app/chamados/models.py:2511` | Inclui a via na identidade. | Consumidores na tabela 4. |
| `app/chamados/models.py:2535` | Define descrição de apresentação. | “Fato já ocupado (batida existente no slot)”. |
| `app/chamados/models.py:3738`; `app/chamados/juizes.py:679,689,691` | Inclui em `VIAS_SEM_TESTEMUNHA`; juiz reconhece a família e a identidade carimbada. | Família distinta de `VIAS_DE_IDENTIDADE`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:47,48,460,746` | Família sem testemunha participa do piso I6 e de sua apresentação. | Leitura indireta da via por pertencimento ao conjunto. |
| `app/templates/chamados/partials/_card_atendimento.html:186` | Trata literalmente `fato_ocupado` ou `paridade_recusada`. | Exibe “já havia batida registrada”. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40,42` | Permite apenas vias vazia/legado/nula. | `fato_ocupado` fica fora desse recorte por filtro genérico. |
| `app/chamados/migrations/0040_a27_recusa_motivo.py:21`; `0041_b2_via_legado_sem_lastro.py:16`; `0042_hx_via_ausencia_dia.py:16`; `0046_via_celula_absolveu.py:16`; `0048_via_competencia_trancada.py:15`; `0051_decisao_pergunta.py:111` | Preservam o valor nos `choices` históricos. | Declaração de esquema; não são novos escritores de registros com essa via. |

**4. Leitores de `VIAS_DE_IDENTIDADE` e efeito de retirar somente `fato_ocupado`**

São **quatro acessos diretos à tupla e cinco consumidores operacionais das funções de acesso**, além da definição (`app/chamados/models.py:3090,3096,3119,3179,3348`; `app/chamados/juizes.py:689,1427`; `app/chamados/services/relavra_via.py:181`; `app/chamados/management/commands/auditar_invariantes_chamados.py:195`).

| Sítio arquivo:linha | O que faz | Observação: retirada somente da tupla |
|---|---|---|
| `app/chamados/models.py:2511` | Fonte: admin, duplicata, fato ocupado, paridade recusada, motivo retratado. | Passaria de cinco para quatro valores. |
| `app/chamados/models.py:3090` | `vias_de_identidade()` devolve a tupla. | Deixaria de devolver fato ocupado. |
| `app/chamados/models.py:3096` | `vias_que_encerram_por_identidade()` acrescenta competência trancada. | Deixaria de devolver fato ocupado; conservaria tranca. |
| `app/chamados/models.py:3119,3140` | `carimbar` rejeita vias da família de identidade. | Fato ocupado passaria a ser aceito por essa porta e submetido ao C6. |
| `app/chamados/models.py:3179,3180`; `app/chamados/services/materializacao.py:1105` | A porta atual exige identidade. | **O chamador atual de colisão passaria a levantar `ValueError` antes de gravar.** |
| `app/chamados/models.py:3348,3351,3362` | `q_vivas` exclui identidade materializada, condicionado aos demais predicados. | Deixaria de excluir fato ocupado por identidade; resposta, validação e célula continuariam condicionando o resultado. |
| `app/chamados/juizes.py:679,689,691,693,698,705` | Juiz do lastro absolve identidades carimbadas. | Deixaria de absolver sempre por identidade. Contudo a via continuaria em `VIAS_SEM_TESTEMUNHA`, por `app/chamados/models.py:3738`, e poderia absolver por ausência de dia, janela, classificação do dia ou exceção. Não passaria automaticamente a exigir a lâmpada específica. |
| `app/chamados/juizes.py:1427,1428` | C6 encerra identidade materializada. | Seguiria aos degraus de lâmpada, célula, lei e lastro em `:1435,1452,1454,1472`. **Validação continuaria matando antes**, em `:1391,1392`; resposta pode retornar antes em `:1407,1419`. |
| `app/chamados/services/relavra_via.py:181,236,242,246` | Filtro de carimbo desfazível exclui identidades. | Fato ocupado se tornaria elegível; só cairia após releitura e C6 vivo. Validação ainda pode impedir isso. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:195,202,206` | Contraprova F1T classifica o degrau da morte. | Deixaria de rotular “via de identidade”; se ainda morta, cairia em outra classificação; se viva, entraria no alarme. |

**5. `fato_do_marco_no_espelho`: predicado, universo, chamadas e D+1**

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/chamados/juizes.py:922,941,943` | Mapeia `saida_sem_entrada→E/hi`, `orfao_14h→S/hf`, `intervalo_saida→S/hii`, `intervalo_volta→E/hfi`. | Motivo sem mapeamento retorna `None`. |
| `app/chamados/juizes.py:944,948,965,966` | Lê `p.celula`, `cel.dna.marcos` e `cel.ata.lampadas`. | Não calcula timestamp, não lê resposta nem usa `data_evento` para conferir a identidade da célula. |
| `app/chamados/juizes.py:956,960` | Para intervalo, reúne as posições correspondentes de todas as pausas do DNA. | Cobra AND das pausas; não escolhe uma única pausa pela resposta. |
| `app/chamados/juizes.py:962,963,968,969,971` | Procura lâmpadas por `(hora,tipo)` e retorna `all(bool(acesa))`. | Sem hora ou faltando lâmpada: `None`. Lâmpada presente com `acesa=None`: **False**, pela conversão `bool`. |
| `app/chamados/models.py:1502,1504` | Ressuscitação não devolve pergunta de marco aceso. | Usa cópia com célula do julgamento em curso. |
| `app/chamados/services/materializacao.py:906,907,919,930` | Guarda de paridade consulta a lâmpada antes de carimbar. | `is not True` segue à recusa com motivo. Essa guarda não cobre a colisão de `:1099,1105`. |
| `app/chamados/juizes.py:1334`; `:1419` | `_resposta_concorda_com_a_luz` exige marco aceso. | C6 usa esse resultado no ramo de resposta. |
| `app/chamados/juizes.py:1435` | C6 encerra pergunta materializada com marco aceso. | Executa depois dos degraus anteriores. |
| `app/chamados/juizes.py:1452` | C6 também consulta o marco no ramo da célula. | Segundo sítio de chamada dentro do C6. |
| `app/chamados/reconciliador.py:1083,1085,1087,1097` | `veredito_orfa` consulta a lâmpada. | Exclui resposta com texto; acrescenta condições de vitalidade/teto. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:67,68,78,83` | Classifica luz True/False/None e seleciona luz False com C6 vivo. | Universo de fantasmas materializadas, não validadas. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32,40,43` | Seleciona marco aceso para recarimbar via. | Universo materializado, não validado e restrito por via. |
| `app/ponto/services/veredito_celula.py:47,56,60,66,67,68` | `_abs_marcos` encadeia entrada, pausas e saída; soma dias enquanto `dt <= prev`. | Entrada 19h e saída 7h resulta em saída **D+1**. |
| `app/ponto/services/veredito_celula.py:251,252,275,280,282,291` | Usa cadeia absoluta e preserva o instante do candidato faltante. | O candidato de saída pode ficar em D+1. |
| `app/ponto/services/cartorio.py:620,648,656` | Ata mantém lâmpadas das chaves do DNA por tipo/hora. | O juiz pode reconhecer `hf=07:00` na célula do turno D, embora o marco absoluto esteja em D+1. |
| `app/chamados/juizes.py:941,948,965,971`; `app/ponto/services/veredito_celula.py:66,68` | **Responde para a saída noturna pela lâmpada da célula do turno.** | Não recalcula nem certifica o dia civil da saída. Célula/lâmpada efetivamente gravadas de #51467/#56641: `[nao verificado]`. |

São **oito sítios de chamada, em sete funções e seis arquivos**, nas oito linhas de consumidores acima (`app/chamados/models.py:1504`; `app/chamados/services/materializacao.py:907`; `app/chamados/juizes.py:1334,1435,1452`; `app/chamados/reconciliador.py:1085`; comandos `reabrir_orfas_enterradas.py:78` e `encerrar_perguntas_com_fato.py:43`).

**6. Ramo de recusa “PENDENTE com motivo”**

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/chamados/services/materializacao.py:1099,1107,1108,1109` | **Existe:** fora do carimbo de colisão, se o motivo mudou, salva somente `recusa_motivo`. | Trunca motivo a 240 caracteres em `:1096`. Não limpa resposta, validação, materialização ou via. |
| `app/chamados/services/materializacao.py:1110,1111,1113` | Anexa ocorrência `recusa_materializacao` ao destino. | Só quando houve carimbo ou mudança de motivo e existe destino; inclui código/texto/ação traduzidos. |
| `app/chamados/models.py:3042,3044,3046,3049` | Calcula o rótulo de estado. | O ramo anterior **não garante o rótulo `PENDENTE`**: com validação pode continuar `VALIDADA`; com resposta, `RESPONDIDA`. |
| `app/chamados/services/validacao.py:188,190,192,199,202,205` | Lê motivo e devolve validação recusada. | Operação separada: para resposta de hora, sem materialização e com motivo, limpa validação, preserva resposta e registra `DEVOLUCAO`. Chamada em `:464`. |
| `app/chamados/services/materializacao.py:842,919,1219,1303`; `app/chamados/services/passivo_validado.py:545` | Chamadores de `_carimbar_recusa`. | Cobrem ausência de resposta, paridade sem luz, divergência de data, erro final da unidade e recusa de geometria do lote. |
| `app/chamados/services/materializacao.py:1095,1098`; `app/chamados/services/validacao.py:147,188,192,208,225,228,582,604`; `app/chamados/services/passivo_validado.py:66,83` | Leem o motivo para comparação, devolução, foto e trilha. | Leitores operacionais do campo persistido. |
| `app/chamados/services/explicador.py:90`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:186` | Apresentam o motivo ou o usam como explicação. | No modal, sua exibição está dentro do ramo `pendencia_admin`, em `:222`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:536,1075,1112` | Usa motivo vazio como condição dos universos 

[truncado em 20000 de 44505 caracteres]

## 12/11 12:06 · sessao c16ee12ee21d · - · - · origem=Ronald (tmux)

Censo P8 concluído em leitura; inventários completos de leitores por campo enviados nas quatro mensagens anteriores. A tabela abaixo consolida escritores e significado.

| Campo / sítio arquivo:linha | O que escreve | Observação |
|---|---|---|
| `data_evento` — `app/chamados/services/fio.py:801` | Única atribuição explícita: `linha.data_evento = dia_do_turno`. | Nascimento chama em `app/chamados/models.py:3017`, persistindo em `:3018`; costura chama em `app/chamados/services/fio.py:880`, com reversão em memória em `:898` e `:913`; rechaveamento chama em `:1038`, persistindo em `:1044`. |
| `data_evento` — `app/chamados/models.py:2790`, `:2802`; `app/chamados/services/fio.py:1631` | Porta do fato recebe dia do turno; porta da conversa fornece `localdate()`; discriminador `conversa` preserva o pedido. | **Não possui significado universal:** fato = dia do turno; conversa = dia civil de emissão, comprovado em `app/chamados/models.py:2802` e `app/chamados/services/fio.py:1632`. |
| `via_resolucao` — `app/chamados/models.py:3102` | Atribuição junto com instante e autor do carimbo. | Persistida por `carimbar` em `:3153`, por `carimbar_identidade` em `:3191` e por `descarimbar` em `:3213`. |
| `via_resolucao` — `app/chamados/models.py:3267`, `:3276`; `app/chamados/services/relavra_via.py:125` | Lote limpa `''`; encerramento por fato troca para `fato_materializado`; relavra substitui pela via derivada atual. | Últimas duas persistem em `app/chamados/models.py:3277` e `app/chamados/services/relavra_via.py:127`. Default vazio em `app/chamados/models.py:2563`. |
| `materializada_em` — `app/chamados/models.py:3100` | Instante do ato de carimbar; também `None` no descarimbo. | Mesmo conjunto de portas do carimbo acima; lote limpa em `app/chamados/models.py:3267`. |
| `materializada_em` — `app/chamados/models.py:3065`; `app/chamados/services/materializacao.py:1105` | Carimbo também nasce por decisão administrativa e por colisão recusada. | **Não comprova sozinho batida criada:** administrador chama identidade e colisão chama `fato_ocupado`. O help text “batida criada + chamado fechado” em `app/chamados/models.py:3396` é mais estreito que esses usos. |
| `respondida_em` — `app/chamados/services/disputa_emissao.py:1468`, `:1653`, `:1674` | Resposta corrente usa `now()`; resposta automática usa instante recebido; reconstrução pela trilha usa instante recebido. | Persistência em `:1509` **[nao verificado: confirmar linha exata do save normal]**, `:1655` e `:1676`. |
| `respondida_em` — `app/chamados/models.py:3133` | Carimbo pode preencher instante quando há texto e `resposta` pede responder/validar. | Não exige passagem pela atribuição normal de resposta: condição em `app/chamados/models.py:3131`. |
| `respondida_em` — `app/chamados/services/disputa_emissao.py:1593`, `:1632`; `app/chamados/models.py:1567` | Reabertura limpa; ressuscitação limpa. | Reabertura também limpa resposta; ressuscitação opera universo sem texto, filtrado em `app/chamados/models.py:1487`. |
| `validada_em` — `app/chamados/services/validacao.py:134`, `:199` | Validação usa `timezone.now()`; devolução limpa `None`. | Persistência em `:137` e `:202`; atos `VALIDACAO` e `DEVOLUCAO` em `:139` e `:205`. |
| `validada_em` — `app/chamados/models.py:3136`; `app/chamados/services/disputa_emissao.py:1480` | Carimbo pode preencher validação vazia quando `resposta='validar'`; resposta nova desfaz validação anterior. | Condições em `app/chamados/models.py:3135` e `app/chamados/services/disputa_emissao.py:1471`. |
| `validada_em` — `app/chamados/services/disputa_emissao.py:1593`, `:1632`; `app/chamados/models.py:1567` | Reabertura e ressuscitação limpam. | Não confundir com julgamento da célula: estado projetado usa validação e materialização separadamente em `app/chamados/models.py:3042`. |
| `resposta_colab` — `app/chamados/services/disputa_emissao.py:1467`, `:1652` | Resposta normal e resposta automática. | Automática não sobrescreve texto existente: guarda em `:1647`. |
| `resposta_colab` — `app/chamados/services/disputa_emissao.py:1593`, `:1632` | Reabertura limpa `''`. | Salva foto anterior em `DecisaoPergunta` em `:1615`, lendo texto e carimbos em `:1619`. |
| `resposta_colab` — `app/chamados/models.py:2448` | Default `''` no nascimento. | É texto polimórfico: hora, escape e identificadores de posto/escala; leitores diferenciados em `app/chamados/models.py:3527`, `:3535` e `app/chamados/services/materializacao.py:359`. |
| Memória, sem novo escritor persistente — `app/chamados/models.py:1508`, `:1509`, `:3164`, `:3165` | Limpa carimbos na cópia julgada; restaura instância após recusa/erro de `carimbar`. | Não contar como persistências independentes: cópia em `:1495`; função de restauração em `:3160`. |
| Fora do fluxo de produção: `bin/sombra_regras_pessoais.py:85` → `bin/sombra_mascara_pessoal.py:54`, `:57`, `:62`, `:64` | Lê e transforma `pergunta` e `resposta_colab`, persistindo por `bulk_update` na cópia sombra. | Leitor adicional SQL da máscara: `bin/sombra_selo_pessoal.py:74`, `:77`. |
| Todos os sete campos — `app/chamados/services/foto_e4b.py:251`, `:272` | Leitor genérico fotografa todos os campos concretos de `PerguntaDisputa`. | Deve entrar no censo mesmo sem referência literal ao nome de cada campo; inclusive `pergunta`. |

Complementos para consolidar a tabela P8:

- Leitores dinâmicos de instantes: `app/chamados/services/historico.py:98`, `:105`; leitor de reversão de materialização/via/resposta/validação: `app/chamados/services/passivo_validado.py:64`, `:66`, `:83`; agregador dos três instantes: `app/chamados/services/painel.py:66`.
- O instante efetivo de uma saída noturna não é a simples soma da data gravada com a hora: combinação inicial em `app/chamados/services/materializacao.py:419`, ajuste overnight em `:426`. A classificação de conflito já usa `_slot_esperado` em `app/chamados/services/validacao.py:872`.
- API expõe `data_evento` diretamente como ISO e rótulo em `app/api/views.py:1153`, `:1154`; template apresenta essa data sem recompor o dia civil do marco em `app/templates/chamados/partials/_card_atendimento.html:90`.
- Migração histórica com leitura de identidade: `app/chamados/migrations/0053_pergunta_unica_por_familia.py:32`, `:33`, `:40`; schema original de resposta/carimbos: `app/chamados/migrations/0014_perguntadisputa.py:24`, `:25`, `:26`.
- Auxiliar externo que lê respondida/validada: `bin/censo_folha.py:28`, `:29`; consumidor indireto do payload de pergunta/data: `mensageria/nucleo/ferramentas.py:119`, `:127`.
- Excluir homônimo de outro modelo: `app/chamados/services/regularizacao_ext.py:199`, `:220` refere-se a `RegularizacaoExterna`, não `PerguntaDisputa`.

## 12/11 12:03 · sessao d57e2b6a2b71 · - · - · origem=Ronald (tmux)

Contagem por expressão, sem testes/snapshots: **8 interpolações de data = 4 `.format` + 4 `.replace`; 7 locais lógicos**, porque `query.py:64,66` são os dois ramos da mesma função. Há ainda formatação administrativa sem data e cópia de catálogo para âncora (`app/chamados/services/acoes_disputa.py:265`; `app/chamados/services/disputa_emissao.py:1374`).

| Sítio arquivo:linha | O que faz | Observação / fonte da data |
|---|---|---|
| `app/chamados/catalogo/query.py:64` | `replace('{data}', ISO convertido para dd/mm/aaaa)` | Usa **contexto da disputa por motivo**, `contexto_disputa[motivo]['data']`, lido em `:58-59`; não consulta marco absoluto. Chamado no emissor em `disputa_emissao.py:345,354`. |
| `app/chamados/catalogo/query.py:66` | Ramo de exceção: `replace('{data}', data_str)` | Usa a mesma string crua do contexto; não valida turno/dia civil nesse ramo (`query.py:58-66`). |
| `app/chamados/services/disputa_emissao.py:879` | Formata texto de cada marco faltante | `dia` vem da tupla retornada por `_marcos_faltantes` (`:859-861,873`) ou `faltantes` recebido; exige `dia == data_evento` (`:874`). A tupla é ancorada na **data da célula**, inclusive a véspera, em `declaracao_texto.py:109,139`. |
| `app/chamados/services/disputa_emissao.py:1151` | Formata pergunta de intervalo a partir do chamado | `_de = date.fromisoformat(_m['data'])`, onde `_m` vem de `contexto_json.marcos_faltantes` (`:1143-1148`). Um produtor demonstrável põe ali o **dia civil do marco encadeado**: `app/ponto/management/commands/detectar_intervalo_ausente.py:77-84`. O emissor passa esse valor a `dia_do_turno_da_celula` e à criação (`disputa_emissao.py:1152-1155`); origem efetiva de cada registro de produção `[nao verificado]`. |
| `app/chamados/services/disputa_emissao.py:1208` | Formata pergunta do grupo com batida de origem | Usa `data_evento = resolver_data_evento(...batida_origem...)` (`:1205`). O resolvedor retorna **dia do turno da batida**, sem somar o dia civil da saída (`app/escala/servico_jornada.py:495-509`). |
| `app/chamados/services/disputa_emissao.py:1246` | Formata pergunta sem batida de origem | Usa `data_evento = _ddc(ch.contexto_json)` (`:1232`; import `:1042`). `_ddc` tenta `data_turno`, `data_batida`, `entrada_timestamp`, `pendentes`, `horario_previsto`: portanto aceita fontes com nomes e bases diferentes (`app/chamados/data_evento.py:16-39`). |
| `app/chamados/services/declaracao_texto.py:374` | Formata pergunta criada na declaração do colaborador | `dia` vem do marco faltante casado por `_casar_hora` (`:341-345`); `_marcos_faltantes` devolve data da célula em `:109,139`. Persiste pelo `abrir` em `:376-377`. |
| `app/chamados/services/declaracao_texto.py:664` | Formata pergunta criada ao aplicar declaração textual | Usa `p['dia']`, produzido por `propostas_do_chamado`: grade → `_marcos_faltantes` (`:615`) → `_casar_hora` (`:625-628`) → dict (`:635`); persiste em `:666-667`. |
| `app/chamados/services/acoes_disputa.py:265` | Formata confirmação de reabertura administrativa com `% label_motivo` | **Sem data no texto**; nasce como conversa (`:262-268`), cujo `data_evento` é `localdate()` (`app/chamados/models.py:2795-2803`). |
| `app/chamados/services/disputa_emissao.py:1374` | Copia pergunta de catálogo da âncora ou literal de fallback | **Sem data conhecida no texto**; cria conversa `ancora_ausente`, tipo `data_hora` (`:1369-1377`). |
| `app/chamados/services/disputa_emissao.py:440,447` | Entrega `texto` ao nascimento, por `abrir_conversa` ou `abrir` | Texto vem de `texto_pergunta_com_data` (`:345,354`); data passada ao nascimento pode ser derivada depois por outro caminho (`:371-424,451`). Logo texto e `data_evento` não são recalculados juntos nesse ponto. |
| `app/chamados/services/disputa_emissao.py:840` | Repasse de `pergunta` por `_criar_pergunta_se_couber` | Não reformata; encaminha texto e data recebidos ao `abrir` (`:823-845`). |
| `app/chamados/models.py:3015` e `:3018` | Instancia com `**campos` e salva inserção | Persistência comum dos textos recebidos; `abrir` e `abrir_conversa` chegam via `_sob_contrato/_nascer` (`:2790-2803,2820-2828`). O reuso retorna a existente sem reescrever texto (`:2998-3009`). |
| `app/chamados/catalogo/perguntas.py:49` e `app/chamados/models.py:3566` | Literais do template “Confirme o horário da saída no dia {data}” | São **fontes do template**, não formatação de `data_evento`. A referência raspada `models.py:3564-3565` não corresponde a um formatador nesta cópia (`models.py:3564-3566`). |

**Leitores diretos do campo persistido: 20 ocorrências = 14 Python + 6 templates.** Se excluir `__str__` e o comando smoke, restam 18; isso é um recorte possível, não a contagem de todos os leitores. Evidências nas vinte linhas abaixo.

| Sítio arquivo:linha | O que faz | Observação |
|---|---|---|
| `app/api/views.py:1155` | API de chamados fornece `p.pergunta` | Texto cru; data é fornecida separadamente em `:1153-1154`. |
| `app/api/views.py:2551` | API das perguntas do app, `_item` | Texto cru para pendentes/histórico (`:2519-2559`). |
| `app/api/views_mensageria.py:564` | API da fila de validação | Texto cru; `data_evento` separado em `:568`. |
| `app/ponto/services/dia_do_colab.py:69` | Consulta do dia entrega pergunta | Texto cru; seleciona pelo `data_evento=data` (`:64-71`). |
| `app/chamados/views_cobrar.py:291` | Amostra da validação em lote | Texto cru no dict de saída. |
| `app/chamados/canal.py:81` | Perguntas mudas com resposta no fio | Texto cru; correlaciona fio por colaborador e `data_evento` (`:71-84`). |
| `app/chamados/services/explicador.py:71` | Monta dict explicativo | Mantém texto cru; calcula estado/ação em outros campos (`:68-79`). |
| `app/chamados/services/disputa_emissao.py:1549` | Copia pergunta para nota de resposta | Envolve o texto cru com tag e resposta; salva ocorrência no fio (`:1555-1556`). |
| `app/chamados/services/validacao.py:599` | Copia pergunta para nota de recusa | Envolve texto cru com autor, resposta anterior e motivo (`:598-600`). |
| `app/chamados/templatetags/chamados_extras.py:11` | Detecta “pergunta cega” | Lê texto cru com regex de data; não o recompõe. |
| `app/chamados/management/commands/auditar_disputas.py:74` | Auditor procura data no texto | Regex no campo cru (`:69-83`). |
| `app/chamados/management/commands/curar_perguntas_em_limbo.py:33` | Procura nota correspondente | Usa texto cru como substring `Pergunta: ...`; não recompõe. |
| `app/chamados/management/commands/smoke_chamados.py:108` | Verifica placeholder residual | Lê pergunta persistida e acusa `{data}`; o `p_cat.pergunta` de `:97` é catálogo, não campo persistido (`:94-108`). |
| `app/chamados/models.py:3547` | `__str__` da pergunta | Prefixa motivo e corta texto cru em 60 caracteres. |
| `app/templates/chamados/partials/_card_atendimento.html:93` | Cartão de resposta do colaborador | Exibe `p.pergunta` cru; sem projeção de marco. |
| `app/templates/chamados/partials/_card_atendimento.html:175` | Cartão de pergunta no histórico | Exibe `p.pergunta` cru. |
| `app/templates/chamados/partials/_pergunta_card.html:28` | Cartão de supervisão/Central | Exibe `perg.pergunta` cru. |
| `app/templates/chamados/partials/modal_fio.html:160` | Pergunta no modal do fio | Exibe texto cru; data do fato é mostrada em bloco separado (`:167`). |
| `app/templates/chamados/partials/_lote_preview.html:24` | Prévia de lote | Campo cru com `truncatechars:60`; data em coluna separada. |
| `app/templates/colaboradores/partials/painel_colaborador.html:197` | Painel do colaborador, bloco “no app” | Exibe `p.pergunta` cru. |
| `app/templates/chamados/partials/modal_fio.html:439` | **Propagação indireta:** dict do explicador | Exibe texto copiado com truncamento, além do dia separado; origem direta em `explicador.py:71`. Não entra nas 20 leituras diretas. |
| `app/chamados/views_cobrar.py:186` | **Propagação indireta:** dict do canal | Repassa texto cru já copiado por `canal.py:81`; relação demonstrada em `views_cobrar.py:180-187`. |
| `mensageria/nucleo/ferramentas.py:119` | **Propagação indireta:** item recebido da API | Copia `i.get('pergunta')` para avaliação; não recompõe. |
| `app/relatorios/pdf_chamados.py:145` e `:147` | **PDF indireto:** imprime `fup.conteudo` | Pode carregar as cópias da pergunta feitas em `disputa_emissao.py:1549-1556` e `validacao.py:598-600`; corta conteúdo em 1.000 caracteres. Não lê `PerguntaDisputa.pergunta` diretamente. |
| `app/chamados/catalogo/query.py:45` | Interpolador existente de catálogo | Não é projeção universal dos leitores: recebe motivo/contexto e lê **catálogo** (`:52-66`); chamados encontrados no emissor `disputa_emissao.py:345,354` e auditor `auditar_disputas.py:191`. Os leitores diretos acima acessam o campo. |

Não apareceu escritor de `p.pergunta = ...` nem `update(pergunta=...)` fora dos testes na varredura da cópia; a prova positiva disponível é a cadeia de inserção por `**campos` em `app/chamados/models.py:3015-3018`; ausência absoluta por mecanismos dinâmicos `[nao verificado]`. PWA com renderizador próprio de `PerguntaDisputa.pergunta` não foi demonstrado; os pontos de app comprovados são APIs e templates citados, e a execução efetiva do cliente em produção é `[nao verificado]`.

## 12/11 12:03 · sessao 84c8983eaadc · - · - · origem=Ronald (tmux)

Censo somente leitura. Não encontrei `AGENTS.md` na árvore nem nos ancestrais pesquisados; regra adicional de arquivo: `[nao verificado]`.

**3. VIA_FATO_OCUPADO**

| sítio arquivo:linha | o que faz | observação |
|---|---|---|
| `app/chamados/models.py:2488` | Declara `VIA_FATO_OCUPADO='fato_ocupado'`. | Definição, não escrita de registro. |
| `app/chamados/services/materializacao.py:1095`, `:1097`, `:1099`, `:1105` | **Único chamador ativo encontrado que grava essa via**: relê carimbo/via/recusa, traduz texto, decide `_carimba = materializada_em is None and codigo == 'COLISAO'`, chama `carimbar_identidade`. | Não consulta motivo/marco/lâmpada da pergunta. |
| `app/chamados/catalogo/recusa.py:66`, `:77` | Tradutor classifica `COLISAO` quando o texto contém `colisao tipo-oposto`, sem distinguir maiúsculas. | `any(m in baixo...)` em `:81-83`; classificação textual. |
| `app/chamados/models.py:3179`, `:3186`, `:3187`, `:3189`, `:3191`; `:3098` | Porta valida a família, grava `materializada_em`, `materializada_por`, `via_resolucao`, e `recusa_motivo` quando fornecido. | Não pergunta ao C6 (`:3170-3177`). Não limpa resposta nem validação. |
| `app/ponto/registro_batida.py:138`, `:139`, `:145`, `:150` | Origem da mensagem: existe batida oposta não retratada na janela inclusiva `timestamp±g_op`, e a candidata **não** casa com marco previsto do próprio tipo; sem autoridade humana retorna recusa. | **Não é somente vizinhança**: há exceção por geometria prevista. Entretanto não consulta **o marco cobrado pela PerguntaDisputa**, nem a sua lâmpada. A assinatura recebe colaborador, timestamp, tipo, origem (`:94`). |
| `app/ponto/registro_batida.py:21` | `g_op=300s` quando há escala e marcos no dia civil local da timestamp; senão `60s`. | Código `:28-32`; o texto do catálogo fala genericamente “menos de 5 minutos” (`recusa.py:67`). |
| `app/ponto/registro_batida.py:35` | `_casa_marco_proprio`: verifica dia local e véspera; considera algum marco previsto do mesmo tipo a distância `<=5min`. | `:42-49`; usa `escala_vigente` e `marcos_turno`, sem PerguntaDisputa. |
| `app/chamados/models.py:2511` | Inclui a via em `VIAS_DE_IDENTIDADE`. | Leitores derivados na tabela 4. |
| `app/chamados/models.py:2535` | Choice/descrição da via. | Disponível a `get_via_resolucao_display`. |
| `app/chamados/models.py:3738` | Inclui em `VIAS_SEM_TESTEMUNHA`. | Família separada da identidade; retirar de uma não retira da outra. |
| `app/chamados/juizes.py:679`, `:689` | Juiz do lastro reconhece a família sem testemunha e absolve sempre as identidades carimbadas. | Se sair só da identidade, continua no ramo sem testemunha, mas passa por janela/classificação de falta (`:690-705`). |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:47`, `:48`, `:460`, `:746` | Família sem testemunha alimenta piso I6 e sua apresentação. | Fato ocupado permanece nesse piso se apenas a identidade mudar. |
| `app/templates/chamados/partials/_card_atendimento.html:186` | Caso literal `fato_ocupado` ou `paridade_recusada`: “ja havia batida registrada.” | Leitor de apresentação explícito. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`, `:42` | Só sobrescreve vias `('', 'legado_sem_lastro', None)`. | Fato ocupado é excluído pelo filtro genérico, embora citado apenas no comentário `:36`. |

Migrações `app/chamados/migrations/0040_a27_recusa_motivo.py:21`, `0041_b2_via_legado_sem_lastro.py:16`, `0042_hx_via_ausencia_dia.py:16`, `0046_via_celula_absolveu.py:16`, `0048_via_competencia_trancada.py:15`, `0051_decisao_pergunta.py:111` preservam o valor em **choices**, não constituem novos escritores desse valor em registros.

**4. VIAS_DE_IDENTIDADE — todos os consumidores ativos diretos e pelas duas funções da fonte**

| sítio arquivo:linha | o que faz | observação / efeito se fato ocupado sair apenas da tupla |
|---|---|---|
| `app/chamados/models.py:2511` | Fonte: admin, duplicata, fato ocupado, paridade recusada, motivo retratado. | Passa de cinco para quatro valores. |
| `app/chamados/models.py:3090` | `vias_de_identidade()` devolve a própria tupla. | Deixa de devolver fato ocupado. |
| `app/chamados/models.py:3096` | `vias_que_encerram_por_identidade()` acrescenta competência trancada. | Deixa de devolver fato ocupado; conserva tranca. |
| `app/chamados/models.py:3119` | `carimbar` recusa famílias que pertencem à outra porta. | Fato ocupado passa a poder entrar nesta porta e será submetido ao C6 (`:3140`). |
| `app/chamados/models.py:3179` | `carimbar_identidade` exige pertencimento. | **Chamador atual de `materializacao.py:1105` passa a levantar `ValueError`** (`:3180`) antes de gravar. |
| `app/chamados/models.py:3348`, `:3351`, `:3362` | `q_vivas` faz pré-filtro SQL: identidade materializada exclui perguntas sem resposta viva. | Fato ocupado deixa de excluir por identidade; validação, resposta e veredito da célula ainda condicionam o resultado (`:3349-3363`). |
| `app/chamados/juizes.py:689`, `:691` | `_pergunta_materializada_de_fato`: identidades carimbadas absolvem sem reclassificar o dia. | Fato ocupado deixa de absolver sempre. Continua em `VIAS_SEM_TESTEMUNHA` (`models.py:3738`): pode absolver por ausência de dia, fora da janela, falta não acusada ou exceção (`juizes.py:690-705`); não passa automaticamente a exigir a lâmpada específica. |
| `app/chamados/juizes.py:1427`, `:1428` | C6 mata identidade materializada no degrau posterior à resposta. | Fato ocupado segue para lâmpada (`:1434-1436`), veredito da célula (`:1438-1453`), lei do dia (`:1454-1462`) e lastro (`:1472-1479`). **Validada ainda morre antes de tudo** (`:1391-1392`); resposta não vazia também pode retornar antes (`:1407-1419`). |
| `app/chamados/services/relavra_via.py:181` | `q_carimbo_desfazivel` exclui identidades materializadas. | Fato ocupado torna-se elegível; só cai se a linha relida passar no filtro e o C6 a considerar viva (`:236-246`). Validada permanece morta no C6. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:195`, `:206` | `_contraprova_f1t` rotula o degrau pelo qual morreu. | Não classifica mais como “via de identidade”; se ainda morta e não validada, rotula “carimbo/celula”. Se viva, vira alarme (`:202-210`). |

São **quatro acessos diretos à tupla** (`models.py:3090`, `:3096`; `relavra_via.py:181`; auditor `:195`) e **cinco consumidores operacionais das funções de acesso** (`models.py:3119`, `:3179`, `:3348`; `juizes.py:689`, `:1427`). A própria definição está em `models.py:2511`.

**5. fato_do_marco_no_espelho**

| sítio arquivo:linha | o que faz | observação |
|---|---|---|
| `app/chamados/juizes.py:922` | Universo de motivos: `saida_sem_entrada→(E,hi)`, `orfao_14h→(S,hf)`, `intervalo_saida→(S,hii)`, `intervalo_volta→(E,hfi)`. | Outro motivo resulta `None` (`:941-943`). |
| `app/chamados/juizes.py:944`, `:948` | Usa exclusivamente `p.celula`, `cel.dna.marcos` e `cel.ata.lampadas`. | Não consulta banco diretamente, resposta, `data_evento`, timestamp, `veredito`, `_abs_marcos` ou batidas. FK pode ser carregada pelo ORM ao acessar `p.celula`. |
| `app/chamados/juizes.py:956`, `:960` | Para intervalo, seleciona todas as chaves existentes da mesma posição dos pares de pausa. | Pergunta de intervalo cobra AND das pausas, sem identificar uma única instância de pausa. |
| `app/chamados/juizes.py:962`, `:965`, `:968`, `:969`, `:971` | Faz lookup das lâmpadas por `(hora,tipo)`; `None` sem horas ou se faltar alguma lâmpada; retorna `all(bool(acesa))`. | `acesa=None` numa lâmpada presente vira **False**, porque usa `bool`, não `None`. Não valida identidade/data da célula contra a pergunta. |
| `app/chamados/models.py:1504` | `_universo_da_ressuscitacao` não ressuscita marco já aceso. | Usa cópia com a célula do julgamento em curso (`:1502`). |
| `app/chamados/services/materializacao.py:906`, `:907` | Guarda de **paridade** só carimba a recusa quando o marco está aceso. | `is not True` vai à recusa pendente; essa guarda não cobre COLISAO (`:919-930`, `:1099-1106`). |
| `app/chamados/juizes.py:1334` | `_resposta_concorda_com_a_luz` exige marco aceso antes de comparar resposta/luz e coerência do dia. | C6 o chama em `:1419`. |
| `app/chamados/juizes.py:1435` | C6: com materialização e marco aceso, mata antes do veredito do dia. | Chamado só depois dos degraus anteriores. |
| `app/chamados/juizes.py:1452` | C6: célula que não acusa com marcos pendentes ainda pode absolver se o marco da pergunta acendeu. | Segundo sítio de chamada dentro de `_pergunta_viva_calc`. |
| `app/chamados/reconciliador.py:1085` | `veredito_orfa` exige marco aceso; não trata resposta com texto (`:1083`) e depois exige C6 vivo e teto encerrado (`:1087-1097`). | Consumidor de órfãs. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:78` | Mede True/False/None das fantasmas e só considera luz False com C6 vivo (`:83`). | Recorte específico `VIA_FANTASMA`, materializada, não validada (`:67-68`). |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:43` | Seleciona perguntas materializadas não validadas e sem história para recarimbar marco aceso. | Via permitida em `:40-42`. |
| `app/ponto/services/veredito_celula.py:47`, `:56`, `:66`, `:67` | `_abs_marcos` encadeia entrada, pausas e saída, acrescentando dias enquanto o instante não for posterior ao anterior. | Entrada 19h/saída 7h produz S em D+1 pela regra `:67-68`. |
| `app/ponto/services/veredito_celula.py:251`, `:252`, `:275`, `:280` | Veredito usa cadeia absoluta, indexa por `(tipo,HH:MM)` e cria candidatos no instante absoluto faltante. | Instante D+1 preservado no candidato (`:282`, `:291`). |
| `app/ponto/services/cartorio.py:620`, `:648`, `:656` | Ata representa lâmpadas com chaves do DNA e casamento por tipo/hora. | Isso permite ao juiz acima ler `hf='07:00'` na célula do turno D, mesmo sendo saída D+1. |

Contagem das chamadas ativas do juiz: **8 sítios em 7 funções, 6 arquivos**: `models.py:1504`; `materializacao.py:907`; `juizes.py:1334`, `:1435`, `:1452`; `reconciliador.py:1085`; comandos `reabrir_orfas_enterradas.py:78`, `encerrar_perguntas_com_fato.py:43`.

Conclusão demonstrável para noturna: o juiz consegue responder pela **lâmpada de saída `hf` da célula do turno**, inclusive quando a cadeia absoluta a coloca em D+1; ele não recalcula nem confere o dia civil do marco (`juizes.py:941-971`; `veredito_celula.py:66-71`). A lâmpada e a célula corretas nos casos de produção 51467/56641: `[nao verificado]`.

## 12/11 12:01 · sessao 9a4a3a589e82 · MAPA-DO-FIO-MORTO · censo · origem=Code

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **d — SEM_ACAO** | 241 (`recorte.out:50`) | **Juiz:** `censo_alcance`: sem vivas ligadas à célula, colaborador na CxR e dia fora dela; rota `sem_porta`, papel `nenhuma porta` (`app/chamados/services/vigia_fio.py:610`, `app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:699`). **Escritor:** `d` não é campo persistido; a retenção lê veredito/protestos, escritos por `lavrar_veredito` (`app/folha/export.py:1241`, `app/ponto/portas/celula.py:974`). Ausência de viva não identifica nascimento, morte ou escritor histórico [nao verificado]. | **E3:** “vigia_fio leem SO tarefa_do_dia”; **E4B:** “backfill do fio morto (matriz_e4 C3, classes a-e) pela porta canonica” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1894`). Cobrem leitura/backfill; `tarefa_nada` não produz ato reparador no comando (`app/chamados/management/commands/backfill_fio_morto_e4b.py:117`). | **Decisão específica para retenção com tarefa vazia: não consta dos dois anexos da pergunta 2** (`avais_e3_leitores.txt:5`, `avais_e3_leitores.txt:18`; `censo.r`, seção 3 [nao verificado quanto à numeração física]). Que os **241** tenham tarefa vazia também não foi medido: `SEM_ACAO` é classificação da sonda, sem consulta a C19 (`sonda_mapa_fio.py:39`). |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **porta_fila — decidir — SEM_ACAO** | 15 (`recorte.out:36`) | **Rota:** C17 aceita chamado da célula na fila antes de exigir pergunta; censo copia rota e verbos (`app/chamados/juizes.py:1647`, `app/chamados/juizes.py:1657`, `app/chamados/services/vigia_fio.py:642`). **Papel:** `particionar_verbos → creditos_perguntas → verbo_do_chamado`; ausência de crédito pode resultar em `decidir` (`app/chamados/services/verbos.py:34`, `app/chamados/services/raiox.py:135`, `app/chamados/catalogo/natureza.py:61`). **Escritores:** status por `_gravar_estado`; mortes/carimbos pelas portas discriminadas na resposta 4 (`app/chamados/models.py:201`, `app/chamados/models.py:3153`). Autoria histórica individual [nao verificado]. | **E3:** “5 respondedores morrem com certidao” e leitores passam a C19 (`app/docs/BACKLOG.md:1873`). C19 já publica somente portas de ações exigíveis; fila residual não cria tarefa (`app/chamados/services/tarefa_do_dia.py:538`). **E4C/E4D:** “LEITORES DIVERGE + selo 2” / “parar projecao” (`app/docs/BACKLOG.md:1885`). | Os **15 ficam fora do comando E4B**, cujo recorte exige `sem_porta` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:79`). A migração E3 não demonstra a ação C19 nem a cura histórica de cada dia; isso não aparece no recorte [nao verificado]. Cobertura adicional específica por outro PRONTO [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **porta_fila — cobrar — SEM_ACAO** | 6 (`recorte.out:30`) | **Rota:** chamado da célula em `qs_na_fila`; **papel:** crédito de pergunta sem resposta resulta em `cobrar` (`app/chamados/juizes.py:1647`, `app/chamados/services/raiox.py:140`, `app/chamados/catalogo/natureza.py:50`, `app/chamados/catalogo/natureza.py:60`). Crédito usa `q_vivas`/lavra; app acrescenta C6 e disputa aberta: são predicados diferentes (`app/chamados/services/raiox.py:74`, `app/chamados/services/raiox.py:135`, `app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`). **Escritores:** nascimento `_nascer`, resposta/reabertura e estado do chamado (`app/chamados/models.py:3015`, `app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:228`). | **E3:** “fio, tela admin, API/app, badge/PWA […] leem SO tarefa_do_dia” (`app/docs/BACKLOG.md:1873`). **RAIZ:** “elo raiz<-filhos […] DisputaSupervisao nao reage” cobre a relação estrutural, sem identificar estes seis (`app/docs/BACKLOG.md:1904`). | **E4B exclui estes seis `porta_fila`** (`app/chamados/management/commands/backfill_fio_morto_e4b.py:79`). `app=0` não revela qual guarda excluiu cada pergunta; atribuir os seis a disputa fechada, identidade ou outro motivo é [nao verificado]. PRONTO específico que decida cada um desses estados [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **a — registrado — SEM_ACAO** | 2 (`recorte.out:47`) | **Juiz:** `censo_alcance` encontra chamado vivo fora da fila e publica o status como detalhe (`app/chamados/services/vigia_fio.py:664`, `app/chamados/services/vigia_fio.py:669`). `registrado` pertence a VIVOS, mas não a COBRANCA (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`). **Escritores:** nascimento `abrir/criar`, decidido pelo motor; mudanças por `_gravar_estado` (`app/chamados/models.py:934`, `app/chamados/models.py:972`, `app/chamados/models.py:1061`, `app/chamados/models.py:1096`, `app/chamados/models.py:228`). | **E4B:** “classes a-e”; **E3:** “vigia_fio leem SO tarefa_do_dia” (`app/docs/BACKLOG.md:1894`, `app/docs/BACKLOG.md:1873`). **E1:** “censo de toda porta do fato” cobre a estrutura das entradas, sem provar defeito nestes registros (`app/docs/BACKLOG.md:1859`). | O PRONTO não transforma `registrado` em ação exigível; o catálogo declara registro sem cobrança (`app/chamados/catalogo/motor.py:25`). A ação C19 e o ato E4B aplicável aos dois não foram fornecidos [nao verificado]. Eventual lacuna além desse escopo [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **e — pergunta_viva_sob_chamado_terminal — SEM_ACAO** | 2 (`recorte.out:52`) | **Juiz:** após as causas anteriores, viva com âncora terminal recebe esse detalhe (`app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:687`). Âncora é destino, senão chamado da disputa (`app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:117`). **Escritores:** estado terminal por `_gravar_estado`; vínculo por `_nascer/reapontar_resolvedora` (`app/chamados/models.py:228`, `app/chamados/models.py:3015`, `app/chamados/services/fio.py:880`). Sequência histórica concreta [nao verificado]. | **E4B:** “classes a-e”; cura de credor sem resposta existe em `reabrir_credor_de_pergunta_viva` (`app/docs/BACKLOG.md:1894`, `app/chamados/services/disputa_emissao.py:1805`). **E3:** leitura única; **RAIZ:** “elo raiz<-filhos” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1904`). | A cura do credor possui vetos de cascata, exportação, limbo e reabertura; não promete tornar a pergunta pendente no app (`app/chamados/services/disputa_emissao.py:1835`, `app/chamados/services/disputa_emissao.py:1839`, `app/chamados/services/disputa_emissao.py:1848`). Motivo de `app=0` e cobertura individual desses vetos [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **e — sem_pergunta_viva — SEM_ACAO** | 1 (`recorte.out:53`) | **Juiz:** sem vivas e sem enquadramento nas causas anteriores recebe esse detalhe (`app/chamados/services/vigia_fio.py:677`, `app/chamados/services/vigia_fio.py:685`). **Escritor compatível com a via raspada `fato_ocupado`:** recusa COLISAO do materializador → `carimbar_identidade` (`app/chamados/services/materializacao.py:1097`, `app/chamados/services/materializacao.py:1105`, `app/chamados/models.py:3191`). A via não prova autoria histórica [nao verificado]. | **E4B:** “classes a-e”; **E3:** leitura única; **E4C/E4D:** “LEITORES DIVERGE + selo 2” / “parar projecao” (`app/docs/BACKLOG.md:1894`, `app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1885`). | A via de identidade pode encerrar a pergunta sem resolver a exigibilidade do dia; C6 e C19 fazem leituras distintas (`app/chamados/juizes.py:1428`, `app/chamados/services/tarefa_do_dia.py:230`). Ação necessária, C19 concreto e cura desse dia não constam da medida [nao verificado]. Cobertura contratual adicional específica [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **e — COLAB_RESPONDE** | 48 (`recorte.out:51`) | **Juiz da causa:** viva sob âncora terminal (`app/chamados/services/vigia_fio.py:687`). **Juiz da ação da sonda:** existe pendência do app no mesmo `data_evento`; não significa reparação completa do dia (`sonda_mapa_fio.py:27`, `sonda_mapa_fio.py:41`). **Escritores:** estado terminal `_gravar_estado`; nascimento/costura da pergunta `_nascer/reapontar_resolvedora` (`app/chamados/models.py:228`, `app/chamados/models.py:3015`, `app/chamados/services/fio.py:880`). | **E4B:** “backfill do fio morto […] classes a-e”; reabre credor de pergunta viva sem resposta sob âncora fora de VIVOS (`app/docs/BACKLOG.md:1894`, `app/chamados/services/disputa_emissao.py:1821`, `app/chamados/services/disputa_emissao.py:1827`). **E3:** leitores únicos; **RAIZ:** “elo raiz<-filhos” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1904`). | **Cobertura parcial:** a cascata pode impedir a reabertura; o dado não contém os insumos dos vetos (`app/chamados/services/disputa_emissao.py:1835`, `app/chamados/services/disputa_emissao.py:1839`, `app/chamados/services/disputa_emissao.py:1848`). Não há prova de 48 curas nem de que a pergunta contada em `app` seja a mesma que determinou a causa [nao verificado]. Lacuna contratual adicional [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **c — ADMIN_VALIDA** | 15 (`recorte.out:48`) | **Juiz:** viva respondida sob âncora terminal; validação ausente decorre de C6 (`app/chamados/services/vigia_fio.py:671`, `app/chamados/services/vigia_fio.py:675`, `app/chamados/juizes.py:1391`). **Escritores:** resposta por `responder_pergunta_disputa_core`, automática, trilha ou complemento de `carimbar`; status por `_gravar_estado` (`app/chamados/services/disputa_emissao.py:1468`, `app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3133`, `app/chamados/models.py:228`). | **E4B:** “classes a-e”; **E3:** “tela admin […] leem SO tarefa_do_dia”; **RAIZ:** reação raiz/filhos (`app/docs/BACKLOG.md:1894`, `app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1904`). **E4C/E4D:** leitores/escritores da projeção (`app/docs/BACKLOG.md:1885`). | **A cura de reabrir credor exclui respondidas**, portanto não alcança diretamente a pergunta que caracteriza `c` (`app/chamados/services/disputa_emissao.py:1821`). Outros atos E4B dependem da resposta C19, ausente na medida (`app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:98`). PRONTO específico garantindo a passagem desses 15 a uma lista de validação [nao verificado]. |

| classe | n | juiz/escritor arquivo:linha | obra que cobre + trecho do PRONTO | o que fica descoberto |
|---|---:|---|---|---|
| **d — COLAB_RESPONDE** | 2 (`recorte.out:49`) | **Causa:** ausência de vivas por **FK célula** e dia fora da CxR (`app/chamados/services/vigia_fio.py:610`, `app/chamados/services/vigia_fio.py:677`). **Ação:** pendência por **data_evento**, em outro universo; app não exclui conversas (`sonda_mapa_fio.py:27`, `sonda_mapa_fio.py:30`, `app/chamados/services/pergunta_no_app.py:49`). **Escritores dos vínculos:** `_nascer` associa célula do fato; `rechavear_data_evento` muda dia/célula (`app/chamados/models.py:2897`, `app/chamados/models.py:3015`, `app/chamados/services/fio.py:1038`). | **E3:** API/app e vigia leem C19; **E1:** “censo de toda porta do fato”; **E4B:** “classes a-e” (`app/docs/BACKLOG.md:1873`, `app/docs/BACKLOG.md:1859`, `app/docs/BACKLOG.md:1894`). | A diferença de universos não prova erro de identidade, célula ou C6. Qual pergunta gera `app=1`, sua natureza e seu vínculo com a célula são [nao verificado]. Cobertura individual ou lacuna adicional não podem ser atribuídas pelos agregados [nao verificado]. |

**2. NAO.** O brief E3 fornece o procedimento V/T/X e exige decisões no censo; não escreve o que C19 deve mandar — nem que deva mandar nada — para o estado específico “folha retém e tarefa não manda nada”. O `censo.r` fornecido distingue nada/desconhecido/sem porta e conserva o universo, sem decidir esse estado (`avais_e3_leitores.txt:5`, `avais_e3_leitores.txt:18`; `censo.r`, seção 3, itens 4–5 [nao verificado quanto à numeração física]).

Os **241 são `SEM_ACAO` da sonda**, não uma medição de `tarefa_do_dia == nada`; essa classificação consulta validação, app, Resolver dia e rota (`recorte.out:50`, `sonda_mapa_fio.py:39`).

| 3. Tela candidata | View/template e função REAL que monta a lista | c — 15 | e — 48 |
|---|---|---|---|
| **Drawer administrativo — Mensagens** | `painel_colaborador` chama `projecao_do_app`; lista real: `perguntas_do_app → consulta_do_app → projecao_do_app`; template percorre pendências (`app/colaboradores/views.py:940`, `app/chamados/services/pergunta_no_app.py:45`, `app/chamados/services/pergunta_no_app.py:61`, `app/chamados/services/pergunta_no_app.py:78`, `app/templates/colaboradores/partials/painel_colaborador.html:193`). | **Exclui a pergunta respondida** de `pendentes`; ela vai ao histórico. Outra pendência do mesmo dia pode aparecer (`app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/pergunta_no_app.py:74`; ocorrência individual [nao verificado]). | **Inclui as pendências que produziram `app>0`**, sem filtro de status da âncora/silêncio (`sonda_mapa_fio.py:27`, `app/chamados/services/pergunta_no_app.py:71`). Mostra dia do turno para fatos; conversa recebe dia `None`. Identidade entre pendência do app e pergunta sob terminal [nao verificado] (`app/chamados/services/pergunta_no_app.py:106`). |
| **Modal administrativo do fio** | `modal_fio_colaborador → montar_chamados_fio(so_janela=False)`; template percorre cartões e perguntas (`app/chamados/views.py:889`, `app/chamados/services/fio.py:163`, `app/templates/chamados/partials/modal_fio.html:68`, `app/templates/chamados/partials/modal_fio.html:157`). | **O predicado admite:** todos os chamados do colaborador; perguntas de disputa aberta e vivas de disputa fechada. Terminal/silêncio não excluem (`app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:235`). Porém a enumeração exige disputa com chamado; esse FK não consta do recorte: inclusão individual [nao verificado] (`app/chamados/services/fio.py:184`). | Mesmo predicado: **admite pergunta sob terminal**, com a mesma ressalva do FK da disputa (`app/chamados/services/fio.py:184`, `app/chamados/services/fio.py:240`). Inclusão das 48 perguntas concretas [nao verificado]. |
| **Página administrativa Fio** | `fio_colaborador → montar_chamados_fio`; template renderiza cartões/followups (`app/chamados/views.py:677`, `app/chamados/services/fio.py:163`, `app/templates/chamados/fio_colaborador.html:153`, `app/templates/chamados/fio_colaborador.html:237`). | Inclui o **chamado terminal**, pois o carregamento filtra somente colaborador; esse template não possui a lista de perguntas do modal. Ver chamado não comprova ver a pergunta (`app/chamados/services/fio.py:177`, `app/templates/chamados/fio_colaborador.html:161`). | Mesmo comportamento (`app/chamados/services/fio.py:177`, `app/templates/chamados/fio_colaborador.html:161`). |
| **Central/painel de gestão** | View `painel_gestao → montar_contexto_painel`; templates `painel_gestao.html` e `_lista_chamados.html` (`app/chamados/views.py:727`, `app/chamados/views.py:740`, `app/chamados/views.py:767`, `app/chamados/services/painel.py:198`). | **Exclui a âncora terminal do padrão:** exige COBRANCA. Filtro explícito pelo status admite o chamado; “histórico” cobre resolvido/fechado, sem provar qual status ocorreu aqui (`app/chamados/services/painel.py:302`, `app/chamados/services/painel.py:306`, `app/chamados/services/painel.py:316`; status individual [nao verificado]). | Mesmo predicado: **fora da lista padrão**, acessível pelo filtro explícito correspondente (`app/chamados/services/painel.py:306`, `app/chamados/services/painel.py:316`). |
| **Pendências de disputa** | `pendencias_disputa → relatorios_disputa.coletar`; template de pendências (`app/colaboradores/views.py:2998`, `app/colaboradores/views.py:3001`, `app/chamados/relatorios_disputa.py:26`, `app/templates/colaboradores/partials/pendencias_disputa.html:6`). | **Condicional:** `coletar` exige disputa aberta; o recorte não fornece seu fechamento. Categoria depende de todas as vivas da disputa (`app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:40`). Inclusão individual [nao verificado]. | A disputa da **pendência do app** entra, pois o app exige disputa aberta. A tela mostra pessoa/disputa, não cada dia/pergunta (`app/chamados/services/pergunta_no_app.py:71`, `app/chamados/relatorios_disputa.py:40`, `app/templates/colaboradores/partials/pendencias_disputa.html:6`). Correspondência com a pergunta terminal [nao verificado]. |
| **Calendário do perfil** | `calendario_ponto → contexto_calendario`; grade `_calendario_grade.html` (`app/colaboradores/views.py:1949`, `app/colaboradores/services/calendario.py:159`, `app/templates/colaboradores/partials/_calendario_grade.html:67`). | **Inclui o dia na janela selecionada**, enumerada inclusive; não lista a pergunta. Ticket da âncora terminal fica fora da carga VIVOS; eventual pai vivo é outro vínculo (`app/colaboradores/services/calendario.py:437`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:603`, `app/colaboradores/services/calendario.py:721`). | Mesmo predicado: **dia visível na janela**, sem prova de exposição da pergunta terminal (`app/colaboradores/services/calendario.py:603`, `app/colaboradores/services/calendario.py:721`). |
| **Cadastro × Realidade** | View `cadastro_realidade → cadastro_realidade.lista`; ramo candidato C11 é `beco_em_lote` (`app/escala/views.py:476`, `app/escala/views.py:487`, `app/escala/services/cadastro_realidade.py:787`, `app/escala/services/cadastro_realidade.py:442`). | **Inclusão [nao verificado]:** C11 exige ativo, veredito `indefinida`, protesto específico, sem porta, viva sob terminal e folga não definida; o recorte não contém todos esses insumos (`app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:488`, `app/escala/services/cadastro_realidade.py:515`, `app/escala/services/cadastro_realidade.py:518`). | Mesma insuficiência: `sem_porta` com terminal **não basta** para inclusão (`app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:518`). |

| 4. Campo/estado | Quem ESCREVE | Quem LÊ | Mesmo significado e bordas |
|---|---|---|---|
| **`rota`** | Saída transitória de C17; censo copia ou atribui balde/`sem_porta`. C19 também produz sua própria rota, a partir das ações (`app/chamados/juizes.py:1657`, `app/chamados/services/vigia_fio.py:646`, `app/chamados/services/vigia_fio.py:699`, `app/chamados/services/tarefa_do_dia.py:553`). | Sonda; backfill seleciona `sem_porta`; folha lê C17 (`sonda_mapa_fio.py:32`, `sonda_mapa_fio.py:47`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:79`, `app/folha/fora_da_folha.py:67`). | **Não equivalentes:** C17 admite presença na fila; C19 exige porta de ação julgada. Não são campos persistidos da pergunta (`app/chamados/juizes.py:1647`, `app/chamados/services/tarefa_do_dia.py:541`, `app/chamados/services/tarefa_do_dia.py:546`). |
| **`causa`/`detalhe`** | `censo_alcance`, por precedência a→b→c→d→e; grava apenas a estrutura de retorno (`app/chamados/services/vigia_fio.py:667`, `app/chamados/services/vigia_fio.py:699`). | Sonda agrega somente `sem

[truncado em 20000 de 31536 caracteres]

## 12/11 12:00 · sessao 25cc736dbb89 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**Veredito geral: bloqueia declarar a c16 integralmente fechada.** A guarda do resumo foi corrigida e existe agora teste integrado com o recalculador real; porém a garantia literal “nunca turno aberto” sem célula continua sem preservação, e outro leitor da marca ainda elimina a data ao filtrar anomalias. (`app/relatorios/pdf_espelho.py:477`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`; `app/ponto/tests/test_lastro_c11_sem_banco.py:418`; `app/relatorios/pdf_espelho.py:675`)

**1. Bloqueios da preverif c14**

| Bloqueio | Veredito | O contraexemplo ainda reproduz? |
|---|---|---|
| Órfã 10/11 07:02 suprime entrada legítima 11/11 07:02 | **Fecha nesse ponto.** | **Não** no resumo nem na marca. A seleção restringe o HH:MM à linha recebida e guarda o timestamp completo; guarda e marca consultam esse mesmo conjunto. O teste exige uma abertura em 11/11, preservando a órfã de 10/11. (`app/relatorios/pdf_espelho.py:475`; `:477`; `:178`; `:516`; `:568`; `app/ponto/tests/test_lastro_c14_sem_banco.py:118`) |
| Prova integrada de (d), com recalculador substituído | **Fecha por inspeção; execução ORM [nao verificado].** | O mutante `return {}` já não sobreviveria: `_lido()` chama o recalculador real e exige o colaborador no retorno. O simulador também é chamado realmente. A prova antiga continua parcial, agora declaradamente. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`; `:85`; `:89`; `app/ponto/tests/test_lastro_c14_sem_banco.py:393`) |
| Selo sem célula enfraquece “nunca turno aberto” | **Fecha a comparação com a tela; bloqueia afirmar preservação da garantia literal.** | O comportamento sem ata permanece: marca ausente e um turno aberto no harness. O novo selo compara contadores, mas deixou de exigir zero; a documentação restringiu “nunca” ao caso **com ata**. O resultado da fixture ORM de 16:19 permanece **[nao verificado]**. (`app/ponto/tests/test_lastro_c11_sem_banco.py:416`; `:418`; `app/relatorios/tests/test_cartao_marca_espuria.py:8`; `:135`; `:155`) |

**Há uma sobrevivência da classe “perder a data”, fora da guarda corrigida:** o cabeçalho reúne HH:MM de todas as órfãs e remove qualquer anomalia contendo esses horários. Com órfã em **10/11 07:02**, também elimina **“Entrada sem saída em 11/11 07:02.”** O formato dessa mensagem vem do adaptador produtivo. Isso bloqueia considerar a classe completamente fechada no escopo solicitado; ocorrência efetiva em produção: **[nao verificado]**. (`app/relatorios/pdf_espelho.py:675`; `:678`; `app/ponto/adaptador_motor.py:40`)

**2. Teste c16 — fecha a costura; tem limites explícitos** [nao verificado]

As três funções são importadas diretamente e chamadas sem substituição por `Mock`: `julgar_celula`, `recalcular_fechamento_mes` e `instantaneo`. O congelamento substitui o relógio, não essas funções. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:31`; `:34`; `:35`; `:46`; `:79`; `:83`; `:89`)

O oráculo é **misto, com âncora independente suficiente para esse caso**:

- **Pela regra:** cadastro 08–17 com intervalo 12–13, quatro batidas na segunda e ausência aprovada na terça; esperados literais de **960 previstos, dois dias, 480 abonados e 480 realizados**. Não são capturados da saída para formar o esperado. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:56`; `:66`; `:73`; `:107`)
- **Por invariância:** depois da relavra, compara seis campos e o retrato do retido com o estado anterior. A contraprova retira os metadados e exige perda de 480 no previsto e abono zero. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:116`; `:123`; `:130`; `:139`)
- **Limite:** a ata antiga continua sintetizada da nova; isso não executa o cartório histórico. `ausencia_label` é cobrado por presença, sem assert de conteúdo. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:95`; `:101`)

**Mutantes produtivos que fariam falhar, por inspeção:**

| Mutante | Mordida |
|---|---|
| Recalculador retorna `{}` | Falha no `assertIn`. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:85`) |
| Cartório perde os metadados da ausência | Falha na presença/tipo/id da ata nova. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:95`; `:97`; `:98`) |
| Cálculo ignora `ausencia_tipo` | Falha na contraprova que exige diferença de 480. A regra produtiva consulta esse campo. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:139`; `app/ponto/services/dia_pago.py:392`) |
| Simulador consulta fechamento gravado para os retidos | Nesta fixture, sem fechamento gravado, desaparece a prontidão exigida. (`app/folha/management/commands/simular_folha.py:76`; `:85`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:125`; `:128`) |

O comentário “MESMO objeto” excede o demonstrado pelo teste: `_lido()` e `_foto()` fazem chamadas independentes. A implementação do simulador transporta os objetos da sua própria chamada ao recalculador. (`app/ponto/tests/test_lastro_c16_recalculador_relavra.py:83`; `:89`; `:121`; `app/folha/management/commands/simular_folha.py:130`; `:152`)

**3. Selo cartão — fecha a comparação sem célula**

Compara **a mesma autoridade funcional**, consultada duas vezes: chama `espelho_do_colab` diretamente e chama o coletor PDF, que também consulta esse serviço, com o mesmo período e instante nesse cenário. Não compara a mesma instância de retorno. (`app/relatorios/tests/test_cartao_marca_espuria.py:75`; `:129`; `:132`; `app/relatorios/pdf_espelho.py:474`)

O contador esperado é lido diretamente do resumo da tela. Para a marca, o teste **deriva um booleano** por pertinência do HH:MM em `orfas_grade` daquele dia; portanto não lê um `batida_tela.orfa` pronto, mas também não reconstrói grade ou pareamento. (`app/relatorios/tests/test_cartao_marca_espuria.py:133`; `:140`; `:142`; `:144`)

**Fila:** isso prova a comparação de contador e marca sem célula, não a disposição visual completa. O positivo com ata verifica horário presente e ausência de “turno aberto”, mas não exige o asterisco nem a separação das marcações comuns. (`app/relatorios/tests/test_cartao_marca_espuria.py:109`; `:110`; `:112`; `:155`)

**4. Checklist das classes**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando autoridade | **Fecha quanto ao delta de classificação:** não reaparece grade própria no PDF; ele adapta `orfas_grade` recebido da tela. **Fila herdada:** PDF e tela continuam contando abertos com guardas diferentes. (`app/relatorios/pdf_espelho.py:474`; `:479`; `:516`; `app/ponto/services/espelho.py:436`) |
| Segundo escritor fora da porta | **Fecha no delta produtivo:** escreve somente atributo de apresentação `batida.orfa` e estruturas do retorno. A ata continua passando pela porta; os `update(ata=...)` novos são fixtures do teste. (`app/relatorios/pdf_espelho.py:568`; `app/ponto/portas/celula.py:1033`; `:1040`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:103`; `:137`) |
| Relógio fora da borda | **Fila herdada:** `agora` chega à tela, mas não a `folha_manda`, que consulta turno vivo e teto sem recebê-lo; ambos podem ler o relógio corrente. (`app/relatorios/pdf_espelho.py:474`; `:587`; `app/relatorios/cartao_pela_celula.py:398`; `:412`; `app/ponto/turnos.py:2290`; `app/relatorios/cartao_pela_celula.py:40`) |
| Derivação paralela de fonte única | **Bloqueia fechar a classe:** o filtro global de anomalias continua decidindo por coincidência textual de HH:MM. A contagem própria de abertos também permanece. (`app/relatorios/pdf_espelho.py:675`; `:679`; `:516`) |
| Selo/RED sem mordida | **Fecha para as operações novas examinadas:** há casos adversários de data, contador e marca, além da contraprova c16. Execução ORM e RED histórico: **[nao verificado]**. (`app/ponto/tests/test_lastro_c14_sem_banco.py:118`; `:129`; `app/ponto/tests/test_lastro_c16_recalculador_relavra.py:130`) |
| Campo com dois significados | **Fecha a perda de data na guarda; fila nos leitores restantes:** o desenho ainda reduz marcas a HH:MM para suprimir alertas da linha. `orfa=False` continua podendo significar falta de informação disponível, pois ata antiga admite `?` e ausência de célula devolve vazio. (`app/relatorios/pdf_espelho.py:178`; `:919`; `:923`; `app/escala/services/leitor_celula.py:190`; `:390`; `:394`) |

**5. Escritores e leitores dos campos do delta e de suas fontes**

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `orfas` da grade | Pareamento com marcos; publicação por dia e conservação das não consumidas. (`app/escala/utils.py:1725`; `:1782`; `:1835`; `:1858`) | Cartório e adaptador de apresentação. (`app/ponto/services/cartorio.py:736`; `app/escala/utils.py:1113`) |
| `ata.orfas` | Cartório monta; porta atribui e persiste. (`app/ponto/services/cartorio.py:736`; `app/ponto/portas/celula.py:1033`; `:1040`) | Leitor conserva horas ou devolve `?`, publicando `orfas`. (`app/escala/services/leitor_celula.py:187`; `:190`; `:420`) |
| `dias[].orfas_grade` | Adaptador → espelho; sem grade/marcos pode devolver vazio. (`app/escala/utils.py:1087`; `:1113`; `app/ponto/services/espelho.py:699`; `:854`) | HTML, mapa PDF e esperado do selo. (`app/templates/ponto/espelho.html:240`; `app/relatorios/pdf_espelho.py:475`; `app/relatorios/tests/test_cartao_marca_espuria.py:143`) |
| `dias`, `data`, `batidas` | Espelho publica linhas e batidas agrupadas pelo juiz. Reclamadas recebem dia do turno; restantes recebem dia civil. (`app/ponto/services/espelho.py:579`; `:663`; `:775`; `:776`; `app/ponto/turnos.py:2953`; `:2955`) | PDF indexa classificação, seleciona timestamps e recorta linhas. (`app/relatorios/pdf_espelho.py:475`; `:478`; `:562`) |
| `batida.timestamp` | Registro da batida grava o instante recebido. (`app/ponto/registro_batida.py:157`) | Adaptador de períodos, ponte de dia e PDF. (`app/ponto/adaptador_motor.py:37`; `app/ponto/services/espelho.py:458`; `app/relatorios/pdf_espelho.py:478`; `:568`) |
| `resultado.periodos`, `periodos_ft`; `periodo.entrada`, `turno_aberto` | Motor acrescenta períodos e marca aberto quando falta saída; entrada vem do timestamp transportado pelo adaptador. (`app/ponto/motor_calculo_v2.py:2492`; `:2494`; `:2751`; `:2753`; `app/ponto/adaptador_motor.py:37`) | Guarda PDF e conta da tela. (`app/relatorios/pdf_espelho.py:178`; `:515`; `:516`; `app/ponto/services/espelho.py:424`; `:436`) |
| `_orfas_por_data`, `_orfas_instantes` | PDF transforma o contrato recebido em mapa por linha e conjunto de instantes. (`app/relatorios/pdf_espelho.py:475`; `:477`) | Construção do conjunto, guarda e marca. (`app/relatorios/pdf_espelho.py:479`; `:178`; `:516`; `:568`) |
| `resumo.turnos_abertos` | PDF inicia e incrementa; tela conta separadamente. Na competência, folha pode sobrescrever com o fechamento. (`app/relatorios/pdf_espelho.py:253`; `:517`; `app/ponto/services/espelho.py:437`; `app/relatorios/cartao_pela_celula.py:112`; `:346`) | Retorno PDF, HTML e selo comparativo. (`app/relatorios/pdf_espelho.py:603`; `app/templates/ponto/espelho.html:90`; `app/relatorios/tests/test_cartao_marca_espuria.py:133`) |
| `_abertos_ts`, `dias[].turnos`, `datas_turno_aberto` | PDF guarda entradas contadas; cards vêm da ponte instante→pk→dia da tela; PDF publica datas das linhas correspondentes. (`app/relatorios/pdf_espelho.py:514`; `:518`; `:576`; `:578`; `app/ponto/services/espelho.py:461`; `:777`) | `folha_manda` e desenho dos alertas. (`app/relatorios/cartao_pela_celula.py:396`; `:418`; `app/relatorios/pdf_espelho.py:921`) |
| `batida.orfa` | PDF atribui em memória; calendário tem outro escritor transitório. (`app/relatorios/pdf_espelho.py:568`; `app/colaboradores/services/calendario.py:627`) | Filtro global de anomalias, separação, asterisco, alertas e legenda. (`app/relatorios/pdf_espelho.py:675`; `:835`; `:844`; `:919`; `:983`) |

**Ressalva de origem:** `orfas_grade` nem sempre vem da ata: antes de `apur_ini` e no tratamento de `AtaAgregadaSemMarcos`, a tela usa o builder. O PDF recebe esses caminhos pelo mesmo contrato da tela. (`app/ponto/services/espelho.py:493`; `:523`; `:536`; `:542`; `:565`)

## 12/11 11:57 · sessao b126c59da370 · - · - · origem=Ronald (tmux)

Censo do delta e dos produtores/leitores:

| Campo | Escritor/origem | Leitores |
|---|---|---|
| `ata.orfas` | `ata_do_dia` copia `dia.orfas` como strings; porta atribui ata e persiste com evento. (`app/ponto/services/cartorio.py:727`, `:736`; `app/ponto/portas/celula.py:1006`, `:1033`, `:1040`, `:899`) | `_orfas_da_ata` conserva horas; ata antiga vira `?`; `grade_da_celula` publica. (`app/escala/services/leitor_celula.py:180`, `:187`, `:190`, `:420`) |
| `orfas_grade` | Adaptador devolve `gd.orfas`, espelho publica; sem grade/marcos devolve vazio. (`app/escala/utils.py:1087`, `:1113`; `app/ponto/services/espelho.py:699`, `:702`, `:854`) | HTML e PDF. (`app/templates/ponto/espelho.html:151`, `:240`; `app/relatorios/pdf_espelho.py:475`) |
| `dias[].data` | Espelho percorre datas da janela e publica a linha. (`app/ponto/services/espelho.py:663`, `:775`) | Indexação de órfãs e recorte PDF; datas dos turnos abertos. (`app/relatorios/pdf_espelho.py:475`, `:479`, `:562`, `:577`) |
| `dias[].batidas` | Juiz atribui data por `pk`; espelho agrupa e publica. (`app/ponto/turnos.py:2945`, `:2953`, `:2955`; `app/ponto/services/espelho.py:579`, `:665`, `:776`) | PDF recupera timestamps classificados e atribui apresentação. (`app/relatorios/pdf_espelho.py:478`, `:567`) |
| `batida.timestamp` | Campo persistido da batida; adaptador transporta como entrada de período. (`app/ponto/models.py:23`; `app/ponto/adaptador_motor.py:37`, `:43`) | Ponte de dia, seleção de órfãs, marca e desenho. (`app/ponto/services/espelho.py:458`; `app/relatorios/pdf_espelho.py:478`, `:568`, `:840`) |
| `periodo.entrada`, `periodo.turno_aberto` | Motor cria entrada localizada e marca aberto quando saída é `None`; pareamento vem da autoridade. (`app/ponto/motor_calculo_v2.py:509`, `:530`, `:2751`, `:2753`, `:2878`, `:2880`, `:3216`, `:3218`) | Guarda do resumo PDF e conta da tela. (`app/relatorios/pdf_espelho.py:178`, `:516`; `app/ponto/services/espelho.py:426`, `:436`) |
| `_orfas_por_data`, `_orfas_instantes` | PDF adapta a classificação lida da tela, restringindo HH:MM à própria linha e armazenando timestamp completo. (`app/relatorios/pdf_espelho.py:475`, `:477`) | Mesma coleção alimenta resumo e `batida.orfa`. (`app/relatorios/pdf_espelho.py:178`, `:516`, `:568`) |
| `batida.orfa` | PDF atribui atributo transitório, sem `save`. Outro escritor transitório existe no calendário; não encontrei chamada desse serviço pelo PDF. (`app/relatorios/pdf_espelho.py:568`; `app/colaboradores/services/calendario.py:622`, `:627`) | Filtro de anomalias, separação/asterisco, alertas da linha e legenda. (`app/relatorios/pdf_espelho.py:675`, `:679`, `:835`, `:844`, `:919`, `:923`, `:983`) |
| `resumo.turnos_abertos` | PDF inicia em zero e conta com guarda; tela conta sem guarda; na competência a autoridade folha pode sobrescrever pelo campo do fechamento. (`app/relatorios/pdf_espelho.py:253`, `:516`, `:517`, `:587`; `app/ponto/services/espelho.py:436`, `:437`; `app/relatorios/cartao_pela_celula.py:112`, `:346`) | Resumo HTML e retorno PDF; datas abertas alimentam `folha_manda`. (`app/templates/ponto/espelho.html:90`, `:92`; `app/relatorios/pdf_espelho.py:603`; `app/relatorios/cartao_pela_celula.py:396`, `:418`) |
| `_abertos_ts`, `datas_turno_aberto`, `dias[].turnos` | PDF guarda entradas contadas; data vem dos cards publicados pela tela, que usa ponte instante→pk→juiz. (`app/relatorios/pdf_espelho.py:518`, `:576`, `:578`; `app/ponto/services/espelho.py:453`, `:461`, `:463`, `:666`, `:777`) | Compositor de abertos e alertas do desenho. (`app/relatorios/pdf_espelho.py:587`, `:597`, `:921`; `app/relatorios/cartao_pela_celula.py:418`) |

**Atenção ao alcance da origem:** `orfas_grade` nem sempre vem de ata: o trecho anterior a `apur_ini` usa builder, assim como o ramo `AtaAgregadaSemMarcos`. O PDF recebe ambos pelo contrato da tela. (`app/ponto/services/espelho.py:493`, `:523`, `:533`, `:536`, `:542`)

**Fecho específico da data:** órfã 10/11 07:02 já não contamina entrada 11/11 07:02 no resumo nem na marca, porque ambos leem timestamps completos recuperados dentro da linha correta. (`app/relatorios/pdf_espelho.py:475`, `:479`, `:178`, `:516`, `:568`)

**Fila herdada comprovável por leitura:** o desenho ainda apaga a data de marcas para filtrar anomalias globalmente. Uma órfã 10/11 07:02 elimina do cabeçalho qualquer anomalia de outro dia contendo “07:02”; ocorrência concreta de produção [nao verificado]. (`app/relatorios/pdf_espelho.py:675`, `:679`; formato das anomalias em `app/ponto/adaptador_motor.py:34`, `:41`)

**Fila semântica:** a classificação original só distingue strings HH:MM; atas antigas podem conter `?`, sem célula retorna vazio, e o adaptador sem marcos também descarta órfãs. Portanto `batida.orfa=False` continua significando ausência de marca disponível, sem provar casamento legítimo. (`app/escala/services/leitor_celula.py:190`, `:390`, `:394`; `app/escala/utils.py:1087`; `app/relatorios/pdf_espelho.py:568`)

**Fila herdada de relógio:** `agora` chega à tela, mas não a `folha_manda`; esta chama turno vivo e teto sem repassar o instante. (`app/relatorios/pdf_espelho.py:474`, `:587`; `app/relatorios/cartao_pela_celula.py:398`, `:412`; `app/ponto/turnos.py:2290`, `:2291`; `app/relatorios/cartao_pela_celula.py:40`)

## 12/11 11:56 · sessao d257aac0c7f0 · - · - · origem=Ronald (tmux)

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `respondida_em` | `responder_pergunta_disputa_core`: instante `now`, texto junto, salva e registra ato (`app/chamados/services/disputa_emissao.py:1462`, `:1467`, `:1468`, `:1508`, `:1511`). `responder_automaticamente`: instante recebido (`:1652`, `:1653`, `:1655`). `carimbar_resposta_pela_trilha`: instante da nota recebido (`:1661`, `:1665`, `:1674`, `:1676`). `carimbar`: completa nulo, apenas com texto, pelo instante do carimbo (`app/chamados/models.py:3125`, `:3131`, `:3133`). `_nascer` conserva `**campos` recebidos (`:3015`, `:3018`, `:3020`). Limpeza: `reabrir_para_correcao` (`app/chamados/services/disputa_emissao.py:1593`, `:1630`, `:1632`), `ressuscitar_perguntas` (`app/chamados/models.py:1567`). | DateTimeField: instante, não dia (`app/chamados/models.py:2449`). C6 lê projeção temporal, ignora `> agora`, inclui igualdade (`app/chamados/juizes.py:1375`, `:1380`, `:1389`, `:1407`). App usa cru `None` + disputa aberta (`app/chamados/services/pergunta_no_app.py:70`, `:71`). Crédito separa cru nulo/não nulo (`app/chamados/services/raiox.py:135`, `:139`, `:140`). Fio usa `all(p.respondida_em…)` (`app/chamados/services/fio.py:340`). Portanto instante tem tipo comum, mas presença de carimbo recebe significados diferentes de pendência nesses leitores. |
| `validada_em` | `_carimbar_validacao`: `timezone.now`, salva e registra ato (`app/chamados/services/validacao.py:127`, `:134`, `:137`, `:139`). `carimbar(...resposta='validar')`: completa nulo com instante comum se texto (`app/chamados/models.py:3131`, `:3135`, `:3136`, `:3153`). `_nascer` conserva recebido (`:3015`, `:3020`). Limpam: nova resposta (`app/chamados/services/disputa_emissao.py:1471`, `:1480`, `:1508`), devolução (`app/chamados/services/validacao.py:199`, `:202`), reabertura e ressuscitação acima. | DateTimeField (`app/chamados/models.py:2450`). C6 ignora validação posterior a `agora`; presente no instante mata a pergunta antes do marco (`app/chamados/juizes.py:1378`, `:1391`). C19 usa decisão `VALIDACAO` e lâmpada: validada sem efeito pede materializar (`app/chamados/services/tarefa_do_dia.py:141`, `:145`, `:146`); não equivale a morte/encerramento. C19 inclui atos `ato_em <= agora` (`:389`, `:391`) e passivo `<= agora` (`:398`, `:399`). |
| `silenciado_ate`, autor, motivo | Escritores físicos `arquivo.silenciar` (`app/chamados/services/arquivo.py:141`, `:166`, `:171`) e `arquivo.levantar` (`:177`, `:202`, `:210`). Chamadores: arquivar/desarquivar/lote (`app/chamados/views_arquivar.py:37`, `:77`, `:140`); ausência (`app/chamados/services/silencio_ausencia.py:73`, `:87`); isentos (`app/chamados/services/silencio_isentos.py:43`); comandos competência/prescritos/sem dia (`app/chamados/management/commands/arquivar_competencia_encerrada.py:288`; `arquivar_prescritos.py:94`; `arquivar_sem_dia_irrecuperavel.py:94`). | Instante de vencimento, ativo exclusivamente `> agora`; igualdade já não silencia (`app/chamados/models.py:508`, `:512`; `app/chamados/services/painel.py:188`, `:190`). Partição usa o mesmo `>` para ARQUIVADO, mas campo preenchido e vencido vira HISTORICO (`app/chamados/services/verbos.py:48`, `:51`), enquanto `qs_na_fila` volta a incluí-lo: classificações diferentes após vencimento. Silêncio não altera `status_local` (`app/chamados/services/arquivo.py:154`). |
| ato `SILENCIO` | `DisputaSupervisao.fechar` nas portas admin/desligamento chama `fechar_por_admin` e registra ato com instante do carimbo (`app/chamados/models.py:1919`, `:1922`, `:1925`, `:1926`). | C19 interpreta como decisão que encerra ações (`app/chamados/services/tarefa_do_dia.py:150`, `:151`). É ato de pergunta, distinto do véu temporal do chamado, que não muda estado (`app/chamados/services/arquivo.py:154`; `app/chamados/models.py:3842`, `:3844`). |
| `status_local` do chamado | Juiz motor, transição humana (`app/chamados/models.py:123`, `:124`); escrita persistente `_gravar_estado` (`:201`, `:228`, `:296`). Nascimento `abrir`: motor decide e `get_or_create` (`:934`, `:936`, `:939`, `:972`); `criar`: motor decide e INSERT (`:1061`, `:1065`, `:1096`). | Censo lê status das âncoras (`app/chamados/services/vigia_fio.py:618`) e usa TERMINAIS (`:675`, `:687`). Fila exige COBRANCA (`app/chamados/services/painel.py:189`). Catálogo distingue VIVOS, COBRANCA, ENCERRADOS e TERMINAIS (`app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`). C6/lastro pode ler ENCERRADOS, não todo TERMINAIS (`app/chamados/models.py:330`, `:332`). Status terminal não prova pergunta morta; app não filtra status do chamado (`app/chamados/services/pergunta_no_app.py:70`, `:71`). Status concreto dentro de TERMINAIS dos 48: [nao verificado]. |
| `via_resolucao` | `_escrever_carimbo` atribui em memória (`app/chamados/models.py:3098`, `:3102`); persistem `carimbar` (`:3153`), identidade (`:3191`), descarimbo (`:3213`), lote (`:3266`, `:3267`). Exceções físicas: `encerrar_por_fato_no_espelho` (`:3276`, `:3277`), `relavra_via.relavrar` (`app/chamados/services/relavra_via.py:125`, `:127`). | C6 lê via associada à materialização; carimbo posterior ao horizonte é retirado, identidade fora do fato permanece (`app/chamados/juizes.py:1376`, `:1382`, `:1384`); vias que vencem resposta (`:1402`) e demais identidades (`:1428`) têm degraus próprios. C19 lê decisões/efeitos/lavra, não resume tudo à via (`app/chamados/services/tarefa_do_dia.py:116`, `:133`, `:141`, `:150`). Via é vocabulário de encerramento (`app/chamados/models.py:2546`); a via isolada não prova qual escritor histórico agiu [nao verificado]. |
| Dia ao qual R/V/via se referem | Instantes acima registram o ato; o dia é outro campo (`app/chamados/models.py:2449`, `:2450`; `app/chamados/juizes.py:334`). | `_dia_de_turno_da_pergunta` usa `data_evento` como dia de turno para fato; conversa grava dia civil e devolve `None` ao juiz de turno (`app/chamados/juizes.py:334`, `:335`, `:338`). C6 janela inclui as duas pontas (`:618`). C19 carrega fatos `conversa=False`, intervalo de dias inclusivo `gte/lte` (`app/chamados/services/tarefa_do_dia.py:373`, `:375`, `:376`). |

| Via raspada | Juiz/escritor atual comprovado |
|---|---|
| `celula_absolveu` | `perguntas_absolvidas_pela_celula` → `retratar_perguntas_absolvidas_por_celula` → `carimbar` (`app/chamados/reconciliador.py:585`, `:596`; `app/chamados/models.py:3140`, `:3153`). |
| `fantasma` | `veredito_orfa` exige luz `is True`, viva e dia encerrado (`app/chamados/reconciliador.py:1083`, `:1085`, `:1087`, `:1089`, `:1091`); consumidor chama `encerrar_fantasma` (`:1148`) → `carimbar` (`app/chamados/models.py:3280`, `:3288`). |
| `fato_ocupado` | Recusa COLISAO, ainda não materializada → `carimbar_identidade` (`app/chamados/services/materializacao.py:1097`, `:1099`, `:1105`). |
| `declaracao` / `ausencia_dia` | Absorção escolhe por `declarou_hora` e carimba (`app/chamados/services/materializacao.py:548`, `:555`, `:567`); materialização principal escolhe via e carimba com validação/lavra (`:1283`, `:1284`). |
| `chamado_encerrado` | `resolver_perguntas_por_chamado_resolvido`: exige C6 morta, escolhe `via_pelo_fato` ou fallback, chama carimbar (`app/chamados/services/disputa_emissao.py:2048`, `:2058`, `:2059`, `:2060`); seleção final da via (`app/chamados/juizes.py:1757`). |
| Autoria histórica individual dessas linhas | As portas atuais acima não provam autor, instante nem caminho que produziu cada registro raspado [nao verificado]. |

_4237 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
