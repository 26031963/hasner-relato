# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 11:12** · publicado em **19/11/2026 11:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 11:12 · sessao a30249330466 · ESCALA-NAO-DESCREVE · leitura · origem=Code

**Resultado principal:** o núcleo pode produzir uma amostra por data da primeira batida de cada bloco, mas essa chave não garante um turno por dia nem um turno fechado. Blocos distintos que começam no mesmo dia são concatenados em `horas[k]`, enquanto `impares` registra se **algum bloco** dessa chave teve contagem ímpar. [app/ponto/calculador/nucleo.py:215] [app/ponto/calculador/nucleo.py:236]

A análise abaixo usa o ramo sem `pares` e sem `envelopes`. A cópia não possui metadados Git acessíveis no diretório informado; que seja o HEAD do hash declarado é **[não verificado]**.

## 1. Casos do brief e cinco vínculos

Para ler a tabela: `D` é a data local da primeira batida do bloco e `k = str(D)`. “Entrada, saída, marcos” significa o que sairia **se o detector passasse a consumir essa lista** como primeira, última e comprimento; o detector atual continua agrupando por data civil. [app/ponto/calculador/nucleo.py:236] [app/escala/detector_proposta.py:59]

| Caso do brief | Resultado do núcleo e leitura hipotética |
|---|---|
| **(a) 12x36, 19→07** | Para um turno isolado com exatamente `D 19:00` e `D+1 07:00`: `horas[k]=['19:00','07:00']`, `k ∉ impares`; entrada `19:00`, saída `07:00`, 2 marcos. Datas, bordas da janela e eventuais outras batidas da amostra: **[não verificado]**. [app/ponto/calculador/nucleo.py:215] [app/ponto/calculador/nucleo.py:236] |
| **(b) mesmo horário, 4 batidas** | **Condicional:** se as quatro forem `19:00, I1, I2, 07:00`, em ordem, e nenhum corte ocorrer, `horas[k]=['19:00', I1, I2, '07:00']`, `k ∉ impares`; entrada `19:00`, saída `07:00`, 4 marcos. Faltam horários e datas de `I1` e `I2`; um gap de pelo menos 8 h após quantidade par pode separar o bloco. Resultado exato: **[não verificado]**. [app/ponto/calculador/nucleo.py:186] |
| **(c) diurno 07→19** | Para um turno isolado com somente as duas pontas: `horas[k]=['07:00','19:00']`, `k ∉ impares`; entrada `07:00`, saída `19:00`, 2 marcos. O brief não informa se há batidas intermediárias, datas ou outras batidas: resultado da amostra completa **[não verificado]**. [app/ponto/calculador/nucleo.py:236] |
| **(d) 6x1, 22→06** | Para um turno isolado com somente as pontas: `horas[k]=['22:00','06:00']`, `k ∉ impares`; entrada `22:00`, saída `06:00`, 2 marcos. O gap de exatamente 8 h com uma batida na mão não dispara a condição de paridade. Intermediárias, datas e composição da janela: **[não verificado]**. [app/ponto/calculador/nucleo.py:215] |
| **(e) aberto / solta** | Faltam instantes, datas e tipos das batidas. Para uma única batida isolada `H`, o núcleo devolve `horas[k]=[H]`, `k ∈ impares`, e a leitura hipotética seria `H→H`, 1 marco. Com outras batidas próximas, pode juntá-las; não é possível reproduzir os 11,4 h apenas pelo dado fornecido. **[não verificado]**. [app/ponto/calculador/nucleo.py:186] [app/ponto/calculador/nucleo.py:236] |
| **(f) troca de regime** | Faltam a sequência, as datas, a quantidade de batidas e o momento da troca. Portanto `horas`, `impares`, pontas e marcos por chave são **[não verificado]**; `07→19` no brief é o resultado agregado informado para o detector atual, não a sequência necessária para executar o núcleo. [app/escala/detector_proposta.py:70] |

Para **845:146, 1547:220, 1485:451, 1597:489 e 1604:934**, o dado raspado informa classificações de cadastro e proposta, mas não fornece as batidas individuais, datas ou limites efetivos da amostra. Assim, os retornos por chave dos cinco são **[não verificado]**. Os exemplos da tabela não devem ser atribuídos a esses vínculos. O núcleo depende dos instantes efetivamente recebidos e os ordena antes de cortar. [app/ponto/calculador/nucleo.py:45] [app/ponto/calculador/nucleo.py:109]

## 2. Outro corte sem consultar escala

- **`parear_turnos`**, chamado sem alimentar marcos, mapa de dias ou papéis, recebe objetos com `timestamp` e `tipo`, ordena e usa `E`/`S`; não consulta banco por si. No modo sem bordas, pode reconhecer `S→E` como pausa por duração quando há `S` posterior como prova de continuação. Uma `E` com outra aberta pode deixar a anterior aberta; uma `S` sem entrada pode virar órfã. Ele devolve objetos `Turno`, com `aberto`, e não um conjunto de paridade como o núcleo. Sem bordas da véspera, a data é a data local da referência; com alimentação, pode atribuir à véspera. O fechamento sem jornada declarada usa envelope de 26 h nesse caminho, em vez do teto geral de vão de 14 h do núcleo. [app/ponto/turnos.py:1192] [app/ponto/turnos.py:385] [app/ponto/turnos.py:248] [app/ponto/turnos.py:312] [app/ponto/turnos.py:1417] [app/ponto/turnos.py:1430]

- **`Command.handle` de `e6_piso_autopsia`** contém outro corte sequencial embutido, não uma função reutilizável separada: antes remove batidas a menos de 10 minutos; corta por gap ≥8 h com contagem par ou gap >14 h, sem o teto adicional de vão total do núcleo. A chave é a data da primeira batida; turno ímpar marca a data e é inteiramente excluído da soma, enquanto o núcleo soma seus pares fechados. O comando completo também consulta espelho e previsto; o trecho de corte citado não lê escala para decidir o corte. [app/ponto/management/commands/e6_piso_autopsia.py:65] [app/ponto/management/commands/e6_piso_autopsia.py:75] [app/ponto/management/commands/e6_piso_autopsia.py:89]

Exaustividade de outras implementações ou trechos de corte na casa: **[não verificado]**. O registro de censo do próprio repositório identifica também outras funções de pareamento, mas isso, por si só, não demonstra que cortem turno sem insumo de escala. [app/core/tests/test_medida_turno_do_par.py:151]

## 3. Padrões que expõem o corte

Os exemplos são sequências **ilustrativas**, não batidas atribuídas aos cinco vínculos. A coluna “detector com a fonte trocada” significa primeira e última posição e tamanho de `horas[k]`; isso não é o comportamento atual de agrupamento civil. [app/escala/detector_proposta.py:59] [app/escala/detector_proposta.py:70]

| Padrão | Sequência e retorno do núcleo | Detector com a fonte trocada |
|---|---|---|
| **Dois turnos iniciados em D** | `D 00:00, 02:00; D 10:00, 12:00`: o gap de 8 h corta após duas batidas, mas ambos os blocos têm a mesma chave. `horas[k]=['00:00','02:00','10:00','12:00']`; `k ∉ impares`; `envelope[k]=240` min, soma dos dois vãos. [app/ponto/calculador/nucleo.py:215] [app/ponto/calculador/nucleo.py:236] | `00:00→12:00`, **4 marcos**: os blocos não aparecem separados. [app/ponto/calculador/nucleo.py:238] |
| **Intervalo de 8 h ou mais** | `D 07:00, 09:00, 17:00, 18:00`: o gap de 8 h após duas batidas corta; como os dois blocos começam em D, `horas[k]=['07:00','09:00','17:00','18:00']`, sem `k` em `impares`; `envelope[k]=180` min. O corte exige contagem par para o limiar de 8 h. [app/ponto/calculador/nucleo.py:215] | `07:00→18:00`, **4 marcos**, apesar de o núcleo ter separado os blocos. [app/ponto/calculador/nucleo.py:236] |
| **Dobra** | `D 07:00, D+1 07:00`, apenas duas pontas: o gap e o vão são 24 h, então surgem dois blocos unitários, chaves `D` e `D+1`; cada lista é `['07:00']` e ambas as chaves estão em `impares`. [app/ponto/calculador/nucleo.py:215] [app/ponto/calculador/nucleo.py:243] | Em cada chave, `07:00→07:00`, **1 marco**. Uma dobra com batidas intermediárias pode produzir outra partição; depende dos instantes. **[não verificado]**. [app/ponto/calculador/nucleo.py:186] |
| **Mais de 14 h** | O exemplo documentado `D 06:52, D 21:34` é cortado em dois blocos unitários; como ambos começam em D, `horas[k]=['06:52','21:34']`, `k ∈ impares`, `envelope[k]=0` e `k ∈ cortes`. [app/ponto/calculador/nucleo.py:15] [app/ponto/calculador/nucleo.py:215] | `06:52→21:34`, **2 marcos**: a concatenação esconde a divisão se o consumidor só olhar a lista. [app/ponto/calculador/nucleo.py:236] |
| **Batida esquecida** | Na sequência documentada `D 07:00, 12:00, 17:00; D+1 06:00, 17:00`, o vão até `D+1 06:00` excede 14 h: `horas[D]=['07:00','12:00','17:00']`, `D ∈ impares` e `D ∈ cortes`; `horas[D+1]=['06:00','17:00']`, sem essa chave em `impares`. [app/ponto/calculador/nucleo.py:192] [app/ponto/calculador/nucleo.py:215] | Para D, `07:00→17:00`, **3 marcos**; para D+1, `06:00→17:00`, **2 marcos**. A posição da batida esquecida muda o resultado. [app/ponto/calculador/nucleo.py:236] |

Mesmo quando a lista concatenada tem comprimento par, `impares` pode conter a chave por causa de blocos ímpares individuais, como no exemplo de 14 h 42. [app/ponto/calculador/nucleo.py:236]

## 4. Importação, independência e tripwire

Um leitor de `escala/` importar `ponto.calculador.nucleo` **não cria, pelo código examinado, um ciclo direto de imports**: o módulo do núcleo importa biblioteca padrão e não importa `escala`; `e6_oraculo` já importa a mesma função e chama `minutos_do_oraculo(ts)` sem envelopes. Um novo leitor, isoladamente, não altera essa chamada nem a função. Ciclos indiretos introduzidos por uma implementação ainda não escrita: **[não verificado]**. [app/ponto/calculador/nucleo.py:22] [app/ponto/management/commands/e6_oraculo.py:42] [app/ponto/management/commands/e6_oraculo.py:149]

A independência relevante para essa chamada é que ela continua sem alimentar envelopes de escala. Há teste comportamental que exige o corte nesse modo, e um teste AST que procura derivação aritmética duplicada no núcleo e no comando. **Não localizei, na busca feita, um selo que congele todos os importadores do núcleo ou proíba por AST qualquer leitura de escala dentro dele; exaustividade dessa busca é [não verificado].** O teste citado de unicidade aritmética não é uma lista de importadores. [app/ponto/tests/test_calculador_regras.py:259] [app/ponto/tests/test_e6_oraculo_julga_impar.py:110] [nao verificado]

O tripwire de data verifica padrões AST de `.date()` associados a `timestamp` e tem allowlist para o detector. Se a alteração remover a última ocorrência detectada no detector, o teste de entrada obsoleta exigirá retirar essa entrada da allowlist; isso depende do código final. O visitante também tem limites sintáticos: uma chave derivada dentro do núcleo a partir de `t.date()` não é, por esse fato, prova de correção semântica do dia do turno. [app/ponto/tests/test_contract_no_batida_date.py:18] [app/ponto/tests/test_contract_no_batida_date.py:186] [app/ponto/tests/test_contract_no_batida_date.py:239] [app/ponto/calculador/nucleo.py:236] [nao verificado]

## 5. Consultas e local de execução

**Hoje, por chamada de `propor_escala`:**

- **1 consulta de batidas** ao consumir o queryset de `batidas_apuraveis` em `medir_padrao`. [app/escala/detector_proposta.py:53] [app/ponto/turnos.py:2891]
- **Mais 1 consulta de templates** quando há pelo menos 5 dias e se chega ao laço de `_casa_template`; com menos de 5 dias, retorna antes. Portanto, **1 ou 2 consultas**, sem somar consultas dos chamadores para obter colaborador ou vínculo. [app/escala/detector_proposta.py:99] [app/escala/detector_proposta.py:122]

Uma troca que reutilize a **mesma consulta de batidas**, converta os instantes locais e chame o núcleo em memória acrescentaria **0 consultas pelo núcleo**: nesse ramo a função ordena e opera sobre os dados recebidos. O total continuaria **1 ou 2**, condicionado à implementação e à mesma carga; buscar bordas adicionais ou outros insumos tem custo **[não verificado]** até existir implementação. [app/ponto/calculador/nucleo.py:109] [app/ponto/calculador/nucleo.py:157]

Na **Cadastro x Realidade**, a view chama `lista`, que lê `proposta` da lavra e passa o valor ao botão; esse caminho não recalcula a proposta no clique de leitura da tela. A lavra chama `anomalias_da_frota`, que chama o detector para colaboradores com assinatura. O detector também roda em outros caminhos: coleta do PDF de propostas, comando de limbo e `escala_auto`. [app/escala/views.py:512] [app/escala/services/cadastro_realidade.py:960] [app/ponto/services/esmeril_espelho.py:239] [app/ponto/services/esmeril_espelho.py:321] [app/escala/proposta_pdf.py:43] [app/escala/management/commands/propor_escala_limbo.py:35] [app/escala/services/escala_auto.py:117]

## 19/11 11:08 · sessao df35418f7377 · ESCALA-NAO-DESCREVE · censo · origem=Code

O caminho de **Cadastro x realidade usa a lavra do `MetricaSnapshot`** para a coluna “Destino sugerido”; `botao_da_proposta` não chama o detector. A ausência da chave estruturada `proposta` numa lavra não implica ausência da frase `destino`: a tela lê as duas chaves separadamente. Isso torna compatível, no código, uma frase visível com a falta do botão baseado na proposta estruturada. [cadastro_realidade.py:960](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:960), [cadastro_realidade.py:982](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:982), [cadastro_x_realidade.html:32](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/escala/cadastro_x_realidade.html:32).

**Limite do censo:** não consegui provar exaustividade para referências calculadas dinamicamente, fluxo arbitrário de dicionários ou a frase exata pedida. Abaixo estão os sítios identificados e o script efetivamente executado; ele é uma busca estática de candidatos, não uma prova de ausência de toda referência dinâmica. Também **NAO ACHEI** confirmação do hash da cópia: `git rev-parse HEAD` respondeu que o diretório não é um repositório Git.

## (a) Chamadores e leitores encontrados

**Produção.**

- `medir_padrao`: chamada por `propor_escala` em [detector_proposta.py:124](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:124).
- `_casa_template`: chamada por `propor_escala` em [detector_proposta.py:130](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:130).
- `propor_escala`: quatro chamadas encontradas, todas com import local: `coletar` em [proposta_pdf.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/proposta_pdf.py:43); `handle` em [propor_escala_limbo.py:22](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/management/commands/propor_escala_limbo.py:22); `propostas_do_vinculo` em [escala_auto.py:117](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/escala_auto.py:117); e `anomalias_da_frota` em [esmeril_espelho.py:215](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:215).

**Testes.** As chamadas diretas encontradas estão em [test_detector_proposta.py:64](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_detector_proposta.py:64), :82, :94, :105, :119, :130, :157 e :180. Os testes substituem a função por caminho textual em [test_escala_nao_descreve.py:83](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_escala_nao_descreve.py:83) e :110; há inspeção textual em [test_detector_proposta.py:189](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_detector_proposta.py:189) e referências de contrato em [test_medida_turno_do_par.py:180](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/core/tests/test_medida_turno_do_par.py:180) e :277.

**Alias, `getattr`, templates e JS.** NAO ACHEI chamada adicional por esses mecanismos na busca textual `rg -n 'medir_padrao|propor_escala|_casa_template' --glob '*.{py,html,js}' app` e na busca AST abaixo. Isso não exclui nome construído em tempo de execução. O template lê dados derivados, como `x.destino` e `x.proposta.codigo`, em [cadastro_x_realidade.html:32](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/escala/cadastro_x_realidade.html:32).

### Chaves e leitores identificados

Na tabela, **D** = [detector_proposta.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:53), **PDF** = [proposta_pdf.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/proposta_pdf.py:53), **CLI** = [propor_escala_limbo.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/management/commands/propor_escala_limbo.py:22), **Auto** = [escala_auto.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/escala_auto.py:117), **Esmeril** = [esmeril_espelho.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:241) e **Teste** = [test_detector_proposta.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_detector_proposta.py:64). Os números nas células são linhas desses arquivos.

| Chave | Nascimento | Leitores identificados |
|---|---|---|
| `padrao` | D:127, 132, 157 | PDF:77,121; CLI:41; Auto:122,124; Teste:65–67,83,95,106,159,161 | [nao verificado]
| `proposta` | D:127,132,158 | PDF:75,130–131; CLI:39,50–53; Auto:121; Esmeril:243–244,261–263; Teste:69–70,85,97,108,120,158,160 | [nao verificado]
| `confianca` | D:127,132,157 | PDF:76; CLI:38,53,57; Auto:122; Esmeril:247,264; Teste:68,84,96,107 | [nao verificado]
| `porque` | D:128,133,157 | PDF:138; CLI:54,57; Auto:124; Esmeril:262; Teste:68–69,84,98,121 | [nao verificado]
| `padrao.dias` | D:67,87 | D:125; PDF:77,124; CLI:47; Auto:122,124; Teste:95,106,159,161 | [nao verificado]
| `padrao.marcos_moda` | D:67,88 | D:105,136,145,153; PDF:124; CLI:47; Teste:65,83 | [nao verificado]
| `padrao.marcos_freq` | D:89 | D:137,145,153. Não nasce no retorno sem dados de D:67–69; o ramo de poucos dias retorna antes da leitura em D:125–129 |
| `padrao.entrada` | D:67,90 | D:102,104,135; PDF:125; CLI:47; Teste:66 | [nao verificado]
| `padrao.saida` | D:67,91 | D:102,104,135; PDF:125; CLI:47; Teste:67 | [nao verificado]
| `padrao.duracao_h` | D:68,92 | PDF:126; CLI:48 |
| `padrao.dispersao_entrada_min` | D:68,93 | D:141,146,155; PDF:127; CLI:48 |
| `padrao.dows` | D:68,94 | PDF:128; CLI:49 |
| `padrao.padrao_dias` | D:69,95 | PDF:128; CLI:49 |
| `proposta.tipo_escala_id` | D:158 | Auto:122,125; Esmeril:246; a cópia lavrada é lida em [cadastro_realidade.py:824](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:824), :843–849 e [regua_defesa.py:307](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:307) |
| `proposta.codigo` | D:158 | PDF:130; CLI:52; Auto:123; Esmeril:246,264; Teste:70,85; cópia lavrada em [cadastro_x_realidade.html:32](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/escala/cadastro_x_realidade.html:32) e [views.py:505](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/views.py:505) |
| `proposta.descricao` | D:159 | Auto:123; Esmeril:264 |
| `proposta.ciclo` | D:159 | PDF:131; CLI:53 |
| `proposta.horario` | D:160 | PDF:130; CLI:52; Auto:123; Esmeril:264 |

Os testes da lavra estruturada e do botão também leem as **cópias** das chaves, por exemplo em [test_escala_nao_descreve.py:98](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_escala_nao_descreve.py:98) e :155–165. A tabela registra os leitores identificados, não prova todos os possíveis leitores indiretos.

### Script AST executado

Foi executado via `python3 -B` com o texto abaixo na entrada padrão; não foi criado arquivo. Ele percorre os `.py`, identifica imports locais e apelidos dos três nomes, chamadas e strings com esses nomes; para chaves, imprime acessos literais nos arquivos previamente identificados. A busca de templates e JS foi complementar, por `rg`.

```python
import ast, pathlib

names = {'medir_padrao', 'propor_escala', '_casa_template'}
keys = {
    'padrao', 'proposta', 'confianca', 'porque', 'dias',
    'marcos_moda', 'marcos_freq', 'entrada', 'saida',
    'duracao_h', 'dispersao_entrada_min', 'dows', 'padrao_dias',
    'tipo_escala_id', 'codigo', 'descricao', 'ciclo', 'horario',
}
key_files = {
    'app/escala/detector_proposta.py',
    'app/escala/proposta_pdf.py',
    'app/escala/management/commands/propor_escala_limbo.py',
    'app/escala/services/escala_auto.py',
    'app/ponto/services/esmeril_espelho.py',
    'app/escala/tests/test_detector_proposta.py',
}

for path in pathlib.Path('.').rglob('*.py'):
    try:
        tree = ast.parse(path.read_bytes(), filename=str(path))
    except (SyntaxError, UnicodeError, OSError) as e:
        print('ERRO', path, e)
        continue

    aliases = set(names)
    for n in ast.walk(tree):
        if isinstance(n, ast.ImportFrom) and n.module == 'escala.detector_proposta':
            for a in n.names:
                if a.name in names:
                    aliases.add(a.asname or a.name)
                    print(f'{path}:{n.lineno}: import {a.name} as {a.asname or a.name}')

    for n in ast.walk(tree):
        if isinstance(n, ast.Call):
            f = n.func
            if (isinstance(f, ast.Name) and f.id in aliases
                    or isinstance(f, ast.Attribute) and f.attr in names):
                print(f'{path}:{n.lineno}: chamada {ast.unparse(f)}')
        if (isinstance(n, ast.Constant) and isinstance(n.value, str)
                and any(x in n.value for x in names)):
            print(f'{path}:{n.lineno}: string {n.value[:100]!r}')

    if str(path) in key_files:
        for n in ast.walk(tree):
            if (isinstance(n, ast.Subscript) and isinstance(n.ctx, ast.Load)
                    and isinstance(n.slice, ast.Constant)
                    and n.slice.value in keys):
                print(f'{path}:{n.lineno}: chave {ast.unparse(n)}')
            if (isinstance(n, ast.Call) and isinstance(n.func, ast.Attribute)
                    and n.func.attr == 'get' and n.args
                    and isinstance(n.args[0], ast.Constant)
                    and n.args[0].value in keys):
                print(f'{path}:{n.lineno}: get {ast.unparse(n)}')
```

## (b) Tela e frase

1. A rota chama `cadastro_x_realidade`; a view chama `lista` e renderiza o template: [urls.py:14](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/urls.py:14), [views.py:512](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/views.py:512).
2. `lista` chama `ler_lavra`, lê `destino` e `proposta`, coloca ambos na linha e consulta o botão: [cadastro_realidade.py:960](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:960), :966–972 e :982–987. `ler_lavra` seleciona o snapshot `esmeril_espelho` e devolve `detalhe`: [esmeril_espelho.py:359](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:359).
3. Na produção da lavra, `anomalias_da_frota` chama o detector para pessoas com assinatura, transforma o retorno em frase com `_destino` e guarda separadamente a proposta estruturada: [esmeril_espelho.py:233](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:233), :241–247 e :259–264. `lavrar` chama o produtor e grava o resultado no snapshot: [esmeril_espelho.py:321](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:321), :342–355.
4. O template mostra `x.destino`; o botão depende de `x.botao_proposta`: [cadastro_x_realidade.html:24](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/escala/cadastro_x_realidade.html:24), :32. O POST relê a lavra antes de aplicar: [views.py:493](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/views.py:493).

**Frase pedida:** NAO ACHEI o formato literal “entradas próximas … % de … dias” na cópia com `rg -n -i 'entradas|pr[oó]ximas' app --glob '*.py' --glob '*.html' --glob '*.js'` nem com a busca por `entrada real` e `% de dias` em `app/escala`, `app/templates/escala` e `app/ponto/services`.

Há, contudo, uma frase **parecida** na assinatura C7: `diagnostico_regua` produz `real = 'E~HH:MM (N% de N dias)'` em [regua_defesa.py:167](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:167); o diagnóstico é copiado para a lavra do termômetro em [regua_na_lista.py:29](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/regua_na_lista.py:29), :42–64, e `_termometro` o formata como detalhe C7 em [cadastro_realidade.py:315](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:315). Não consegui provar que essa seja a frase observada para col451.

Para essa frase semelhante e a coluna de destino, são **medidas distintas**:

- A régua usa, por padrão, `ref - 30 dias` até `ref`, com possível reavaliação de 14 dias: [regua_defesa.py:100](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:100), :118–129 e :181–192. O Esmeril chama o detector de `hoje - 60 dias` até ontem: [esmeril_espelho.py:241](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:241).
- A régua desloca para a véspera batidas anteriores ao início declarado menos duas horas e exige ao menos duas por grupo: [regua_defesa.py:151](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:151). O detector agrupa por data civil local: [detector_proposta.py:57](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:57).
- Ambos consultam `batidas_apuraveis`, mas cada um deriva seu agrupamento; a régua declara que não usa o turno pareado, e o detector documenta a circularidade com o cadastro: [regua_defesa.py:118](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:118), [detector_proposta.py:15](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:15).

## (c) Encaixe das decisões e leitores afetados

1. **Amostra com turno fechado:** encaixa na alimentação e agregação de `medir_padrao`, hoje em [detector_proposta.py:55](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:55). O juiz devolve `entrada`, `saida`, `batidas` e `aberto` em [turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43); `turnos_do_colab` e sua dependência de escalas/células estão em [turnos.py:1850](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1850) e :1892–1920. Leitores afetados: as chaves de `padrao` e os quatro consumidores de `propor_escala` da tabela.
2. **Turno iniciado na janela conta inteiro:** encaixa na mesma fronteira de amostragem em [detector_proposta.py:57](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:57); a porta de turnos já declara janela por `data_turno` e carga com margem em [turnos.py:1847](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1847). Leitores afetados: os mesmos consumidores da amostra.
3. **Menos de 80% no mesmo eixo implica abstenção:** encaixa antes do retorno de proposta em [detector_proposta.py:130](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:130). O `0.8` atual mede **frequência do número de marcos**, e sua falha hoje produz confiança média, em [detector_proposta.py:137](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:137) e :147–157. Além dos leitores da tabela, o PDF classifica qualquer ausência de proposta com dias suficientes como “sem template” em [proposta_pdf.py:74](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/proposta_pdf.py:74), :99–105.
4. **Desempate por `tipo_ciclo` × `padrao_dias`:** encaixa na escolha do melhor candidato em [detector_proposta.py:105](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:105), :107–119; `padrao_dias` nasce em :79–95. Mudam os leitores de `proposta` e seus campos listados acima.
5. **Excluir `hora_inicio == hora_fim`:** encaixa no filtro de candidatos em [detector_proposta.py:107](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:107). Mudam os mesmos leitores de `proposta`, inclusive frase e botão derivados da lavra em [esmeril_espelho.py:241](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:241) e [cadastro_realidade.py:971](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:971).

## 19/11 11:03 · sessao 0fc23b4a7ea9 · E4C-F5-CONSTROI · construtor · origem=Code

O teste solicitado foi acrescentado em [test_e4c_f5_contador_texto.py:40](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_contador_texto.py:40). `git diff --check` passou. Conforme solicitado, não executei o teste nem os mutantes; a mordida permanece **[não verificado]**. [nao verificado]

Comando do caso positivo, para o certificador executar com banco de teste configurado ([ci.py:6](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/config/settings/ci.py:6)): [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_vigia_carimba_divergencia_de_texto --settings=config.settings.ci -v 2
```

Os comandos seguintes aplicam cada mutante diretamente ao arquivo de produção. Devem ser executados **separadamente em cópias descartáveis**, seguidos do comando de teste acima. [nao verificado]

**M_censo_zero** — espera-se falha na asserção do censo em [test_e4c_f5_contador_texto.py:52](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_contador_texto.py:52) **[não verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from pathlib import Path
p = Path('chamados/management/commands/vigiar_fio_morto.py')
s = p.read_text()
antes = '                              divergencia_texto_resposta=texto_divergente,'
depois = '                              divergencia_texto_resposta=0,'
assert s.count(antes) == 1
p.write_text(s.replace(antes, depois, 1))
PY
```

**M_fora_do_esperado** — espera-se `KeyError` na consulta ao esperado em [test_e4c_f5_contador_texto.py:53](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_contador_texto.py:53) **[não verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from pathlib import Path
p = Path('chamados/management/commands/vigiar_fio_morto.py')
s = p.read_text()
antes = '                           desligados_retidos=0, ferias_ausencia_sem_agendamento=0, ferias_dia_sem_ausencia=0,\n                           divergencia_texto_resposta=0)'
depois = '                           desligados_retidos=0, ferias_ausencia_sem_agendamento=0, ferias_dia_sem_ausencia=0)'
assert s.count(antes) == 1
p.write_text(s.replace(antes, depois, 1))
PY
```

Diff obtido por `git diff -- chamados/tests/test_e4c_f5_contador_texto.py`:

```diff
diff --git a/app/chamados/tests/test_e4c_f5_contador_texto.py b/app/chamados/tests/test_e4c_f5_contador_texto.py
index 0fdf23aa0..d0011c621 100644
--- a/app/chamados/tests/test_e4c_f5_contador_texto.py
+++ b/app/chamados/tests/test_e4c_f5_contador_texto.py
@@ -1,8 +1,15 @@
 """Contador de texto: divergencia real, universo por pergunta e custo em lote."""
+import io
+import json
+import os
+import tempfile
 from unittest.mock import patch
 
+from django.core.management import call_command
 from django.db import connection
+from django.test import override_settings
 from django.test.utils import CaptureQueriesContext
+from freezegun import freeze_time
 
 from chamados.models import DecisaoPergunta as D, PerguntaDisputa
 from chamados.services.vigia_texto_resposta import contar
@@ -30,6 +37,23 @@ class ContadorTextoTest(BaseCortes):
             D.registrar([dict(pergunta=outro, ato=D.RESPOSTA, ato_em=RESPOSTA_EM, ato_texto='ato')])
         self.assertEqual(contar(), (1, 3), 'dois atos na mesma pergunta: ainda UMA pergunta no universo')
 
+    @freeze_time('2026-11-13 12:00:00')
+    def test_vigia_carimba_divergencia_de_texto(self):
+        p = self.pergunta()
+        self.responder(p)
+        PerguntaDisputa.objects.filter(pk=p.pk).update(resposta_colab='projecao')
+        out = io.StringIO()
+        with tempfile.TemporaryDirectory() as pasta:
+            carimbo = os.path.join(pasta, 'fio_morto.json')
+            with override_settings(VIGIA_FIO_CARIMBO=carimbo):
+                call_command('vigiar_fio_morto', stdout=out)
+            with open(carimbo, encoding='utf-8') as arquivo:
+                reg = json.load(arquivo)
+        self.assertEqual(reg['censo']['divergencia_texto_resposta'], 1)
+        self.assertEqual(reg['esperado']['divergencia_texto_resposta'], 0)
+        self.assertIn('VFM|DIVERGENCIA_TEXTO_RESPOSTA=1 (esperado 0)', out.getvalue())
+        self.assertGreaterEqual(reg['divergencia_texto_resposta_universo'], 1)
+
     def medir(self, minimo):
         contar()  # aquece a primeira chamada fora da medida
         with patch.object(D, 'leitura', wraps=D.leitura) as leitor:
```

LEI-AKITA: padrao=uma pergunta com divergência real, comando real e conferência do carimbo e stdout; certificação e mordida dos mutantes **[não verificado]**.

## 19/11 10:58 · sessao f9b2b6327f63 · E4C-F5-CONSTROI · verificador · origem=Code

**BLOQUEIA para a volta 2b.** O contador chega ao ESTADO como “fora do esperado”, mas o código examinado não o promove a **PERGUNTA no topo**. Também faltam provas de mordida para o caminho positivo completo e para a retirada dos prefetches dos leitores. [bin/fio_estado.py:121](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/fio_estado.py:121), [bin/gerar_estado.py:312](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/gerar_estado.py:312), [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88)

A cópia contém os arquivos examinados, mas o diretório disponibilizado não é um repositório Git; portanto, a identidade do HEAD, a base e a integridade do diff em relação à árvore são **[não verificado]**. O resumo de runs foi fornecido pelo enunciado; os logs completos dos mutantes e o dado de produção não foram conferidos independentemente **[não verificado]**.

1. **NÃO BLOQUEIA — lei B nos sítios do diff.** `espera_o_texto_do_colaborador` pega o instante da leitura e substitui nela o texto pelo da **mesma pergunta** na projeção. Se a leitura estiver pendente, espera mesmo com texto corrente; se tiver instante e o texto corrente estiver vazio após trim, também espera. [app/chamados/services/fio.py:178](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/fio.py:178), [app/chamados/juizes.py:1526](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/juizes.py:1526)

   O histórico combina os instantes vigentes da leitura com o texto corrente da mesma pergunta: não emite evento de resposta ou validação cujo instante esteja vazio; para os demais, mostra o texto corrente, truncado, ou “sem resposta”. Materialização continua com instante da projeção. Assim, instante e texto podem representar **momentos diferentes**, embora não perguntas diferentes nesse laço. Isso corresponde à separação declarada no anexo de resposta B, que não está disponível como arquivo com linha **[não verificado]**. [app/chamados/services/historico.py:96](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/historico.py:96)

2. **NÃO BLOQUEIA — com limites na verificação dos escritores.** O mapa relevante é:

   - `respondida_em` e `resposta_colab`: a resposta humana grava ambos e registra o ato; a resposta automática também grava ambos, enquanto a cura pela trilha carimba o instante de texto já existente. A resposta humana limpa validação anterior. [app/chamados/services/disputa_emissao.py:1474](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/disputa_emissao.py:1474), [app/chamados/services/disputa_emissao.py:1671](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/disputa_emissao.py:1671), [app/chamados/services/disputa_emissao.py:1692](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/disputa_emissao.py:1692)
   - `validada_em`: `_carimbar_validacao` grava o instante e o ato com o mesmo valor; devolução pode limpá-lo e registrar outro ato. `carimbar` também pode preencher resposta e validação na projeção. [app/chamados/services/validacao.py:161](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/validacao.py:161), [app/chamados/services/validacao.py:262](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/validacao.py:262), [app/chamados/models.py:3299](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:3299)
   - `materializada_em`: a porta do modelo atribui o carimbo; o histórico o lê diretamente. [app/chamados/models.py:3269](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:3269), [app/chamados/services/historico.py:104](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/historico.py:104)
   - `decisoes` é escrita por `registrar`; `leitura` ordena atos por instante e PK, acrescenta passivo da projeção sem linha correspondente e aplica reabertura, recusa e devolução. Portanto, “leitura” não significa somente linhas físicas de atos. [app/chamados/models.py:4238](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:4238), [app/chamados/models.py:4295](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:4295), [app/chamados/models.py:4329](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:4329)
   - `divergencia_texto_resposta` é calculada pelo contador, escrita em `censo` no carimbo com esperado zero e lida genericamente pelo ESTADO. [app/chamados/services/vigia_texto_resposta.py:8](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/vigia_texto_resposta.py:8), [app/chamados/management/commands/vigiar_fio_morto.py:260](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/management/commands/vigiar_fio_morto.py:260), [bin/fio_estado.py:121](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/fio_estado.py:121)

   Os campos de resposta, validação e materialização são `DateTimeField`; o carimbo do vigia publica instante com offset e dia local separadamente. A uniformidade de fuso e significado em **todos** os escritores e leitores fora do diff permanece **[não verificado]**. [app/chamados/models.py:2634](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:2634), [app/chamados/models.py:3647](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:3647), [app/chamados/management/commands/vigiar_fio_morto.py:255](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/management/commands/vigiar_fio_morto.py:255)

3. **BLOQUEIA — saída exigida para divergência positiva.** O contador percorre cada pergunta do queryset recebido, inclui o passivo pela leitura e soma uma divergência por pergunta; no vigia, o universo é toda a frota ou o recorte de empresas. O teste cobre divergência, passivo, recorte e dois atos na mesma pergunta. O `0 em 51.563` de produção consta somente do dado fornecido **[não verificado]**. [app/chamados/services/vigia_texto_resposta.py:8](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/vigia_texto_resposta.py:8), [app/chamados/management/commands/vigiar_fio_morto.py:191](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/management/commands/vigiar_fio_morto.py:191), [app/chamados/tests/test_e4c_f5_contador_texto.py:19](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_contador_texto.py:19)

   A comparação de textos é um predicado novo de divergência para o vigia, expressamente solicitado pelo anexo de resposta; não é chamada ao juiz registrado de pergunta respondida, que julga instante mais texto não vazio. A autorização literal do anexo não tem arquivo:linha disponível **[não verificado]**. [app/chamados/services/vigia_texto_resposta.py:16](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/vigia_texto_resposta.py:16), [app/chamados/juizes.py:1544](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/juizes.py:1544)

   **Falha concreta:** com valor positivo, o leitor inclui a chave em “FORA DO ESPERADO” dentro da seção Fio morto; sua manchete continua sendo a de alcance. O gerador coloca essa seção abaixo de Fábrica, Raias e Obras interrompidas. Não há, nesse caminho, promoção a **PERGUNTA no topo**, como exige o texto fornecido. [bin/fio_estado.py:165](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/fio_estado.py:165), [bin/fio_estado.py:191](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/fio_estado.py:191), [bin/gerar_estado.py:312](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/gerar_estado.py:312)

4. **BLOQUEIA — prova de custo incompleta.** Os caminhos identificados carregam decisões em lote: cobrança, montagem do fio, histórico e contador; `coletar` carrega `perguntas__decisoes`, e `classificar` tem prefetch de contingência para suas perguntas. Não identifiquei N+1 novo nesses caminhos com o código atual. [app/chamados/services/cobranca.py:236](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/cobranca.py:236), [app/chamados/services/fio.py:262](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/fio.py:262), [app/chamados/services/historico.py:96](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/historico.py:96), [app/chamados/relatorios_disputa.py:11](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/relatorios_disputa.py:11)

   Os testes comparam uma e várias perguntas e exigem chamadas à leitura; o de `coletar` cria várias disputas, necessário para expor uma consulta de contingência por disputa. Contudo, o dado fornecido só relata mutante de custo para o **contador**; a mordida à retirada de cada prefetch dos demais chamadores não está demonstrada **[não verificado]**. No fio, a contagem global de chamadas a `leitura` tampouco identifica, por si só, qual consumidor as produziu. [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88), [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:121](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:121)

5. **BLOQUEIA — selo positivo de ponta a ponta ausente.** Os testes novos exercitam os leitores reais para divergência de instante, reabertura, texto da projeção e histórico; o contador tem caso positivo próprio. O resumo fornecido relata morte dos mutantes correspondentes, mas os logs completos e o conteúdo exato das mutações são **[não verificado]**. [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:42](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:42), [app/chamados/tests/test_e4c_f5_contador_texto.py:19](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_contador_texto.py:19)

   O teste vizinho de memória usa `(0, 0)` como mock: preserva sua prova específica sobre C5, mas não prova o novo contador. O teste O261 chama o contador real e conserva suas comparações anteriores, porém não demonstra publicação de divergência **positiva** nem sua chegada como PERGUNTA no topo. Assim, a igualdade com zero pode passar sem testar o caso que deve alarmar. [app/chamados/tests/test_e4a2_c5_memoria.py:118](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4a2_c5_memoria.py:118), [app/chamados/tests/test_o261_vigia_fio.py:825](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_o261_vigia_fio.py:825)

6. **BLOQUEIA — selo sem mordida suficiente**, pelos itens 4 e 5. O diff mistura intencionalmente duas fontes para estado e texto, e a leitura incorpora passivo; essas diferenças de significado são explícitas, mas a uniformidade de todos os escritores externos não foi esgotada **[não verificado]**. A resposta humana relê o relógio após a trava no caminho examinado; não concluo uma auditoria completa de relógios e ordem de trava de todos os escritores externos **[não verificado]**. Pré-requisitos e pouso real também são **[não verificado]**. [app/chamados/models.py:4295](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:4295), [app/chamados/services/disputa_emissao.py:1517](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/disputa_emissao.py:1517)

7. **NÃO BLOQUEIA — escopo do diff fornecido.** Os leitores alterados preservam o texto da projeção; o diff acrescenta prefetch, contador e publicação no carimbo, sem alteração apresentada em motor, folha, export, DiaPago, escala ou vínculo. Há escrita da **nova chave no carimbo existente**, autorizada pelo contador descrito no anexo; não identifiquei nova escrita de campos da pergunta no diff. A completude do diff frente ao Git permanece **[não verificado]**. [app/chamados/services/fio.py:188](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/fio.py:188), [app/chamados/services/historico.py:96](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/historico.py:96), [app/chamados/management/commands/vigiar_fio_morto.py:255](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/management/commands/vigiar_fio_morto.py:255)

BLOQUEIA: 4 -- itens 3, 4, 5 e 6

## 19/11 10:50 · sessao 7eb07578ea99 · DST-L2-JZ1 · leitura · origem=Code

A triagem não identifica corretamente todos os criadores nesta cópia. Os criadores diretos de `Justificativa` encontrados no código de aplicação são API, tela de ponto, cron de alertas e regularização; os endereços de benefícios, advertência e materialização citados criam outras entidades. A conclusão sobre a corrida depende também da transação externa: nas portas de aprovação examinadas, o colaborador já é travado antes de se chegar à retratação. [api/views.py:2011](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/api/views.py:2011), [ponto/views.py:1638](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1638), [processar_alertas_turno.py:394](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/management/commands/processar_alertas_turno.py:394), [regularizacao.py:91](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/regularizacao.py:91), [justificativa.py:139](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:139), [ponto/views.py:1795](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1795).

A matriz abaixo é uma **análise condicional**, não uma reprodução em banco. O código não estabelece que cada view ou o cron inteiro seja uma única transação; portanto, não é seguro atribuir um único “commit de T-cria” ao fluxo inteiro. Estado efetivo das constraints, seu instante de verificação, isolamento e esperas em produção: **[não verificado]**. Os settings exibem PostgreSQL, mas não provam o catálogo efetivo nem o isolamento da conexão de produção. [prod.py:9](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/config/settings/prod.py:9), [saas.py:83](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/config/settings/saas.py:83).

## Travas e escritor comuns à matriz

- **R**: após a primeira leitura de retenção, `retratar_com_motivo` abre `atomic` e pede `FOR UPDATE` no colaborador, nas disputas abertas e nos chamados pertinentes, inclusive o chamado alvo. Essa chamada não fornece IDs de perguntas; a função só as trava se receber IDs. Não usa `no_key=True`. Depois, o escritor consulta as justificativas candidatas com `SELECT FOR UPDATE`. [reconciliador.py:544](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:544), [models.py:2853](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:2853), [models.py:2875](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:2875), [justificativa.py:283](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:283).
- **P**: ao apontar uma justificativa, a porta abre `atomic`, pede `FOR UPDATE` no colaborador e no chamado, nessa ordem, e atualiza o ponteiro. A guarda de ponteiro nulo ocorre **antes** da trava. [vinculos.py:81](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/vinculos.py:81), [models.py:488](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:488).
- **FK**: sob a hipótese de constraints PostgreSQL efetivas e verificadas, a verificação da FK pede `FOR KEY SHARE` na linha referenciada. O `INSERT` de J3 refere colaborador e, quando preenchido, chamado; o `UPDATE` do ponteiro refere J3. `FOR KEY SHARE` conflita com o `FOR UPDATE` de R/P. O instante real da verificação — inclusive eventual postergação até o commit — é **[não verificado]**. As declarações estão em [ponto/models.py:136](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/models.py:136), [ponto/models.py:173](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/models.py:173) e [chamados/models.py:75](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:75).

O universo que decide a morte é a consulta de `pendentes_do_vinculo(..., travar=True)`: ela seleciona pendentes pela origem **ou** pelo ID do ponteiro presente na instância `ch`, exclui a justificativa em aprovação e ordena por PK. Em seguida, `rejeitar_justificativas_dos_chamados` grava `status='rejeitada'` e descrição. A retratação chama esse escritor com `portao_de_retencao=False`. [justificativa.py:279](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:279), [justificativa.py:337](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:337), [reconciliador.py:573](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:573).

## Matriz de intercalação

As células de resultado pressupõem: J3 é distinta de J1, está pendente e alcançável pelo predicado; não há irmã que retenha nem outra recusa da transição; ambas as transações terminam sem erro. Esses pressupostos são necessários: a retenção por justificativa alheia só é consultada dentro de `_APROVANDO`, e a própria transição pode ser recusada. [reconciliador.py:449](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:449), [reconciliador.py:560](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:560).

| Criador ou caminho | C cria e commita J3 **antes da leitura inicial de R** | R lê sem J3; C commita J3 **antes de R adquirir as travas** | R adquire as travas e executa o escritor **antes do commit de C** | [nao verificado]
|---|---|---|---|
| **API**, com `chamado_origem=ch`; depois pode executar P. [api/views.py:1974](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/api/views.py:1974), [api/views.py:2011](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/api/views.py:2011) | Em aprovação, a leitura vê J3 e R retorna retido: chamado permanece no estado anterior, J3 pendente; não chega às travas R. Fora de aprovação, não há essa retenção: se a transição passar, R trava J3 e a rejeita. | Com a leitura antiga vazia e J3 já confirmada pela origem, R adquire suas travas e o `SELECT FOR UPDATE` posterior alcança J3: chamado resolvido, J3 rejeitada. Esse cronograma exige que uma transação externa de R ainda **não** detenha trava conflitante. | J3 não confirmada não integra o snapshot do escritor: se R conclui, chamado resolvido e J3 não é rejeitada por esse ato. A verificação FK ou P pode fazer C esperar R; se C já detiver trava FK conflitante, R pode esperar C, impedindo esta ordem específica. Após R, uma criação já decidida com base em leitura anterior pode deixar J3 pendente ligada a chamado resolvido. Instante concreto da espera: **[não verificado]**. |
| **Tela de ponto**, com `chamado_origem=ch`; depois pode executar P. [ponto/views.py:1618](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1618), [ponto/views.py:1638](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1638) | Mesmo resultado da API, condicionado a a busca ter escolhido esse chamado. | Mesmo resultado da API: J3 alcançada pela origem e rejeitada, se C commitou nessa janela e R não estava já protegido por trava externa conflitante. | Mesmo resultado da API quanto a invisibilidade e espera FK/P; chamado resolvido e J3 pendente se os atos terminarem nessa ordem. |
| **Cron**, quando `get_or_create` efetivamente cria J3 com `chamado_origem=ch`; depois pode executar P. [processar_alertas_turno.py:392](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/management/commands/processar_alertas_turno.py:392) | Mesmo resultado da API. Se `get_or_create` encontra uma linha existente, não há nascimento de J3 nessa execução. | Mesmo resultado da API, para o commit da criação pela origem na janela indicada. | Mesmo resultado da API para o nascimento não confirmado. O método não declara um `atomic` envolvendo criação **e** P; tratar ambos como um único commit é **[não verificado]**. |
| **Regularização**, `get_or_create` sem origem e sem apontamento nesse bloco. [regularizacao.py:88](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/regularizacao.py:88) | Por esse criador isoladamente, J3 não entra no vínculo de `ch`; não produz retenção por J3 nem rejeição dela. R pode resolver; J3 fica pendente. | Mesmo resultado: o commit de J3, por si só, não a torna candidata. | Mesmo resultado quanto ao predicado. A regularização tem `atomic` próprio e depois chama `ch.analisar()` nos chamados encontrados; esperas adicionais e resultado integral desse fluxo dependem dos seus demais caminhos: **[não verificado]**. [regularizacao.py:126](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/regularizacao.py:126) |

Nas duas primeiras linhas, há também o ramo **atualiza J existente**: API e tela fazem `j.save()` e podem preencher uma origem antes nula; ele não é nascimento de J3 e tem travas de escrita sobre essa J. O resultado depende do vínculo e do status visíveis ao statement de R. [api/views.py:2000](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/api/views.py:2000), [ponto/views.py:1624](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1624).

Há uma limitação importante para a segunda coluna: na aprovação individual e no lote examinados, `travar_origem_antes` adquire a ordem global **antes** de travar J1 e de prosseguir com a aprovação. Para uma J3 do mesmo colaborador, uma FK efetiva e sua verificação podem, portanto, impedir que o criador confirme nessa janela. Não se pode afirmar a ocorrência dessa célula nesses caminhos sem demonstrar a ordem completa da transação externa e das constraints. [justificativa.py:110](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:110), [justificativa.py:140](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:140), [ponto/views.py:1795](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1795).

### Endereços da triagem que não são criadores de J3

| Endereço citado | O que o código desta cópia mostra |
|---|---|
| `colaboradores/views.py:2036` | Passa `chamado_origem` para `registrar_solicitacao_beneficio`, que cria `SolicitacaoBeneficio`, não `Justificativa`. [views.py:2031](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/colaboradores/views.py:2031), [beneficios.py:20](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/colaboradores/services/beneficios.py:20) |
| `chamados/services/acoes_chamado.py:212` | Campo da criação de `Advertencia`. [acoes_chamado.py:199](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/acoes_chamado.py:199) |
| `chamados/services/materializacao.py:989` e `:1025` | Argumentos de `criar_ausencia`, não criação de justificativa. [materializacao.py:983](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/materializacao.py:983), [materializacao.py:1019](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/materializacao.py:1019) |
| `chamados/services/cobertura.py:110` e `ponto/services/vinculos.py:87` | Esses caminhos não foram encontrados nesta cópia: **[não verificado]**. Existe `chamados/services/vinculos.py:87`, que aponta uma justificativa já recebida e toma P; não a cria. [vinculos.py:81](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/vinculos.py:81) |

## FKs e releitura proposta

Os dois campos são declarados como FKs Django, ambos anuláveis e com `on_delete=models.SET_NULL`: `ChamadoColaborador → Justificativa` para o ponteiro, e `Justificativa → ChamadoColaborador` para a origem. As migrations adicionam os mesmos campos sem declarar `db_constraint=False`. A existência e definição das constraints no banco efetivo são **[não verificadas]**; `on_delete` é a política declarada no Django, não prova, por si só, de uma ação SQL `ON DELETE SET NULL` no catálogo. [chamados/models.py:75](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:75), [migration 0010:15](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/migrations/0010_chamadocolaborador_justificativa_relacionada.py:15), [ponto/models.py:173](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/models.py:173), [migration 0020:15](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/migrations/0020_justificativa_chamado_origem.py:15).

Se a retenção for relida depois de R, em `READ COMMITTED`, uma J3 **já commitada e alcançável pela origem** no início dessa nova consulta será vista e, durante `_APROVANDO`, reterá o chamado. Isso fecha a célula da segunda coluna para esse vínculo, desde que a releitura ocorra de fato. O snapshot é por statement; a consulta posterior do escritor não herda o universo da leitura anterior. [reconciliador.py:453](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:453), [justificativa.py:283](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:283).

Essa releitura **não é uma garantia geral demonstrada pelo código**:

- Fora de `_APROVANDO`, ela não pergunta por justificativa alheia; uma J3 pendente já visível continua podendo ser rejeitada pelo escritor. [reconciliador.py:453](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:453), [reconciliador.py:580](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:580).
- Para vínculo **somente pelo ponteiro**, a consulta usa `ch.justificativa_relacionada_id` da instância recebida. `trava_na_ordem` devolve IDs e não atualiza essa instância. Se o ponteiro mudou antes da trava, reler apenas a função com a instância velha pode ignorá-lo; se o escritor mais tarde receber uma instância com ponteiro atualizado, poderá alcançá-lo. A ocorrência dessa sequência concreta no fluxo inteiro é **[não verificada]**. [justificativa.py:279](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:279), [models.py:2873](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:2873).
- Uma J3 não confirmada no instante da releitura não é vista por ela. Para J3 ligada por FK ao mesmo chamado ou colaborador, o `FOR UPDATE` de R e a verificação efetiva da FK podem impedir o commit concorrente até R terminar; não se deve presumir que ela consiga commitar entre a releitura e o escritor. O momento concreto de verificação da FK permanece **[não verificado]**. [models.py:2853](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:2853), [ponto/models.py:173](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/models.py:173).

## Espera nova ou deadlock

A própria releitura proposta chama `pendentes_do_vinculo` com `travar=False`: é uma leitura simples, portanto não acrescenta `SELECT FOR UPDATE` em J3. A espera nova identificável vem de **mover a leitura para depois da aquisição de R**: R pode esperar uma trava conflitante de C antes de ler, ou C pode esperar R na verificação de FK ou em P. O sentido depende da ordem e do momento efetivo da verificação. [justificativa.py:262](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:262), [reconciliador.py:556](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:556), [vinculos.py:86](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/vinculos.py:86).

**Não está demonstrado um deadlock novo causado pela releitura simples** com os criadores de J3 examinados. Um ciclo exige provar duas esperas simultâneas, inclusive quais versões de J são visíveis e quando as FKs são verificadas; esses dados não estão no anexo nem no catálogo do banco: **[não verificado]**. O código registra que outros caminhos, entre eles regularização, têm ordem própria e que pode haver ciclo em cenários mais amplos, mas isso não prova um deadlock específico desta alteração. [justificativa.py:101](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:101).

## 19/11 10:48 · sessao b95331667c03 · ESPELHO-VERDADE · leitura · origem=Code

**O campo `minutos_realizados` da linha do espelho é calculado a partir de turnos pedidos novamente por `turnos_do_colab` para a janela visual inteira.** Ele não é copiado do resultado do motor nem do campo de minutos da ata. A sonda informa 11 valores positivos com `horas_oraculo=[]` e sem par completo na lista que ela mostrou, mas não fornece o código da própria sonda, as janelas exatas nem os turnos carregados pela chamada interna do espelho. Portanto, **o par e a causa específica de cada um dos 11 resultados permanecem [não verificado]**. A observação dos valores vem do anexo fornecido, sem arquivo e linhas verificáveis na cópia: [não verificado].

### 1. De onde vem o dia creditado

1. `espelho_do_colab` determina `vis_ini` e chama `autoridade_do_periodo(colaborador, vis_ini, hoje)`; depois chama `montar_dias` com esse mesmo intervalo visual. [app/ponto/services/espelho.py:1241](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1241), [app/ponto/services/espelho.py:1278](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1278).
2. `montar_dias` pede `turnos_do_colab(colaborador, data_ini, data_fim_mes)` uma vez, percorre cada data da janela e passa essa data e os turnos carregados a `_realizado_do_dia_tela`. A chave `data` da linha recebe a data do laço. [app/ponto/services/espelho.py:623](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:623), [app/ponto/services/espelho.py:667](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:667), [app/ponto/services/espelho.py:778](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:778).
3. `realizado_do_dia` encaminha os turnos a `realizado_dos_turnos`, que seleciona **`t.data_turno == data`**. Portanto, o crédito vai à linha cuja data coincide com o `data_turno` atribuído pelo pareador. [app/ponto/turnos.py:774](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:774), [app/ponto/turnos.py:719](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:719).
4. Ao formar um `Turno`, `_mk` chama `_data_do_turno`: a referência inicial é a data local do `timestamp` da entrada ou, se não houver entrada, da saída. Conforme os marcos da véspera, a travessia de meia-noite e o envelope da jornada, pode atribuir a véspera. [app/ponto/turnos.py:285](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:285), [app/ponto/turnos.py:312](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:312).

O motor também é chamado nessa montagem, mas por uma trilha paralela: recebe batidas e células e calcula por fatias de vínculo, ou diretamente se não houver escalas; seu resultado é passado a `montar_dias`. Isso não altera a origem do campo `minutos_realizados` descrita acima. [app/ponto/services/espelho.py:1035](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1035), [app/ponto/services/espelho.py:1147](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1147), [app/ponto/services/espelho.py:1178](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1178).

### 2. De onde vêm as pontas que viram minutos

`turnos_do_colab` carrega batidas não retratadas na janela com dois dias de margem antes e um depois; carrega células, vínculos e DNA; divide a janela em segmentos por vínculo do dia; e chama `parear_turnos` com cópias das batidas do segmento, marcos de intervalo e de borda, marcos por dia, papéis vindos da ata e provedor da última batida. Retém os turnos cujo `data_turno` pertence ao segmento. [app/ponto/turnos.py:1834](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1834), [app/ponto/turnos.py:1867](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1867), [app/ponto/turnos.py:1940](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1940), [app/ponto/turnos.py:1961](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1961), [app/ponto/turnos.py:1977](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1977).

No cálculo usual, a entrada é `t.entrada`; o fim é `t.saida` ou, em turno aberto, uma saída anterior que feche par dentro das batidas do turno. Sem entrada, `_fim_fechado` não devolve fim. O cálculo trunca as pontas ao minuto, subtrai pausas reconhecidas ou, quando aplicável, a sobreposição com o intervalo declarado, e soma os líquidos. [app/ponto/turnos.py:609](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:609), [app/ponto/turnos.py:733](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:733).

Existe uma **exceção posicional** antes desse laço: se as guardas de `_borda_posicional` forem satisfeitas, o cálculo usa a entrada do único turno com fim fechado e a última batida dos turnos daquele dia, descontando um único vão interior elegível. Entre as guardas estão: um único turno com fim fechado, ausência de pausas marcadas, pelo menos quatro batidas e exatamente um vão interior elegível. A última batida pode pertencer a outro turno devolvido pelo pareador. A condição ter ocorrido em qualquer um dos 11 casos é [não verificado]. [app/ponto/turnos.py:638](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:638), [app/ponto/turnos.py:673](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:673), [app/ponto/turnos.py:725](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:725).

### 3. Onde muda a autoridade ou a janela

Para o campo do espelho, **não há um ponto em que a lista interna de `parear_turnos` seja simplesmente descartada**: `turnos_do_colab` devolve seus turnos, e `realizado_dos_turnos` os lê. Há, porém, a exceção posicional acima, que escolhe uma ponta entre as batidas de mais de um turno do dia. [app/ponto/turnos.py:1997](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1997), [app/ponto/turnos.py:719](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:719).

O oráculo tem outra derivação: o carregador E6 busca timestamps apuráveis entre `ini - 12h` e `fim + 1 dia + 14h` e chama `minutos_do_oraculo(ts)` **sem `pares`**. Nesse ramo, a função ordena os instantes, aplica cortes próprios e chaveia as horas pela data civil do primeiro instante de cada sequência. [app/ponto/management/commands/e6_oraculo.py:149](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:149), [app/ponto/calculador/nucleo.py:109](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:109), [app/ponto/calculador/nucleo.py:215](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:215), [app/ponto/calculador/nucleo.py:235](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:235).

O carregador E6 também tem uma chamada separada de `turnos_do_colab(c, ini-1 dia, fim+1 dia)`, mas **não é possível afirmar que essa foi a chamada da sonda nem quais `ini` e `fim` ela usou**: o código `sonda_v3.py` não foi fornecido no anexo e sua implementação não foi encontrada na cópia [não verificado]. [app/ponto/management/commands/e6_oraculo.py:156](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:156).

No motor há uma substituição concreta em outra trilha: `_periodos_via_autoridade` converte os turnos pareados e passa o resultado a `_periodos_pelo_marco` em [app/ponto/motor_calculo_v2.py:530](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:530). Esse caminho pode substituir períodos pela leitura da ata; retorna o trio original, por exemplo, para turno partido ou sem células alimentadas. Essa substituição **não é a atribuição do campo `minutos_realizados` da linha**. [app/ponto/motor_calculo_v2.py:534](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:534), [app/ponto/motor_calculo_v2.py:558](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:558), [app/ponto/services/espelho.py:797](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:797).

### 4. Caminhos existentes e os 11 casos

| Caminho | Condição e efeito |
|---|---|
| **Realizado da linha** | Sempre chama `_realizado_do_dia_tela` para a data iterada; dentro dele há o ramo posicional e o ramo usual descritos acima. [app/ponto/services/espelho.py:688](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:688), [app/ponto/turnos.py:725](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:725). |
| **Grade visual por célula ou builder** | A célula é lida a partir de `max(data_ini, apur_ini)`; o trecho anterior usa builder, e `AtaAgregadaSemMarcos` faz o período cair no builder. A grade preenche `realizado_grade`, enquanto o número vem de `_real_dia.minutos`. [app/ponto/services/espelho.py:532](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:532), [app/ponto/services/espelho.py:542](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:542), [app/ponto/services/espelho.py:551](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:551), [app/ponto/services/espelho.py:786](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:786). |
| **Motor: pareador e ata** | O motor passa os turnos ao adaptador e depois pode usar a ata para períodos; as guardas de cobertura ficam em `_periodos_pelo_marco`. [app/ponto/motor_calculo_v2.py:530](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:530), [app/ponto/motor_calculo_v2.py:549](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:549). |
| **Fallback de data civil no motor** | Se o mapa do juiz não contém a entrada do período, o motor usa a data local da entrada; em um caminho também existe herança do dia anterior para período subsequente com intervalo de até seis horas. Essas regras são do motor, não da seleção de `t.data_turno` para o relógio da linha. [app/ponto/motor_calculo_v2.py:2400](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:2400), [app/ponto/motor_calculo_v2.py:2940](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:2940). |

Para os 11, o **campo informado** segue o primeiro caminho pela atribuição em `montar_dias`; qual dos ramos interno usual ou posicional ocorreu em cada caso, quais turnos ele recebeu e qual caminho desenhou a grade são [não verificado]. [app/ponto/services/espelho.py:689](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:689), [app/ponto/services/espelho.py:797](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:797).

### 5. Escritores, leitores e significado dos campos

| Campo ou insumo | Escrita e leitura verificadas | Significado relevante |
|---|---|---|
| `Batida.timestamp`, `tipo`, `colaborador` | `registro_batida` cria a batida com esses campos; `turnos_do_colab` busca por colaborador e data do timestamp; o pareador ordena por timestamp e usa tipo e alimentação. [app/ponto/registro_batida.py:170](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/registro_batida.py:170), [app/ponto/turnos.py:1867](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1867), [app/ponto/turnos.py:1192](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1192). | `timestamp` é instante; sua data local é referência para o juiz, mas pode resultar em `data_turno` da véspera. O `tipo` gravado pode ser superado pelo papel da ata quando esta responde. [app/ponto/turnos.py:285](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:285), [app/ponto/turnos.py:1201](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1201). |
| `retratada_em` | A porta de retratação escreve o instante; a porta de desfazer pode limpar o campo. Tanto a consulta do espelho quanto `batidas_apuraveis` excluem retratadas. [app/ponto/registro_batida.py:294](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/registro_batida.py:294), [app/ponto/services/retratar_humano.py:207](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/retratar_humano.py:207), [app/ponto/turnos.py:1868](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1868), [app/ponto/turnos.py:2892](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2892). | É filtro do universo, não dia da jornada. [app/ponto/turnos.py:2872](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2872). |
| `CelulaDia.data`, `escala_geradora`, `dna.marcos` | A porta de célula cria e pode regenerar esses dados; `vinculo_do_dia` lê a geradora e sua vigência; `marcos_dna_periodo` lê os marcos congelados. [app/ponto/portas/celula.py:289](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/portas/celula.py:289), [app/ponto/portas/celula.py:305](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/portas/celula.py:305), [app/escala/alimentacao.py:269](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/alimentacao.py:269), [app/escala/utils.py:96](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:96). | A data identifica a célula do dia; seus marcos alimentam a geometria e a pausa. A escolha do vínculo pode retornar `None` se a geradora estiver fora da vigência; sem geradora, há fallback para vínculo que cobre a data. [app/escala/alimentacao.py:279](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/alimentacao.py:279), [app/escala/models.py:1135](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1135). |
| `CelulaDia.ata`: lâmpadas, luzes e órfãs | O cartório monta a ata a partir da grade e a entrega à porta de lavra; `periodos_do_dia` lê ata e DNA, e `papel_por_minuto_da_ata` transforma a resposta em papéis para o pareador, sujeitos às guardas de correspondência. [app/ponto/services/cartorio.py:749](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:749), [app/ponto/services/cartorio.py:1898](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:1898), [app/ponto/portas/celula.py:1091](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/portas/celula.py:1091), [app/ponto/juiz_batida.py:149](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/juiz_batida.py:149), [app/ponto/turnos.py:1631](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1631). | A luz tem precisão de hora e minuto; o leitor reconstrói instantes ancorados na data da célula e na ordem dos marcos. O motor procura a batida correspondente para recuperar o timestamp com segundos. [app/ponto/juiz_batida.py:206](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/juiz_batida.py:206), [app/ponto/motor_calculo_v2.py:580](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:580). |
| `Turno.data_turno` e `minutos_realizados` da linha | `_mk` constrói o `Turno` em memória; `realizado_dos_turnos` lê `data_turno`; `montar_dias` escreve no dicionário da linha o resultado calculado; o leitor E6 prefere esse campo quando não é `None`. [app/ponto/turnos.py:312](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:312), [app/ponto/turnos.py:719](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:719), [app/ponto/services/espelho.py:797](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:797), [app/ponto/management/commands/e6_oraculo.py:263](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:263). | `data_turno` é dia atribuído à jornada, não necessariamente data civil do instante. A ata também possui uma chave chamada `minutos_realizados`, escrita pelo cartório, mas ela não é a fonte direta desse campo da linha. [app/ponto/turnos.py:285](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:285), [app/ponto/services/cartorio.py:757](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:757), [app/ponto/services/espelho.py:797](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:797). |

A diferença de significado verificável é, portanto, principalmente **instante da batida → data civil local → dia atribuído ao turno**: o oráculo sem pares agrupa pela data civil do início de sua sequência, enquanto o espelho seleciona o `data_turno` produzido pelo juiz. Isso demonstra uma diferença de regra capaz de mudar a linha, mas **não demonstra que ela explica qualquer um dos 11 casos** [não verificado]. [app/ponto/calculador/nucleo.py:235](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:235), [app/ponto/turnos.py:285](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:285), [app/ponto/turnos.py:719](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:719).

A lista acima identifica escritores e leitores encontrados para os campos decisivos; **exaustividade de todos os escritores indiretos e históricos** [não verificado].

## 19/11 10:40 · sessao f15884b7e099 · ESCALA-NAO-DESCREVE · leitura · origem=Code

**Resultado principal:** `medir_padrao` usa a data civil de cada batida, enquanto a regra atribui a jornada ao dia de início do turno. Nos exemplos noturnos abaixo, isso altera a entrada, a saída ou ambas e pode produzir confiança alta para um horário que não corresponde ao turno observado. A escolha concreta de template em produção é **[não verificado]**: o catálogo não está no dado fornecido. (`app/escala/detector_proposta.py:18`, `:53–95`, `:99–119`; `app/docs/LEIS.md:117`)

O censo abaixo identifica os fluxos encontrados, mas **não sustenta a contagem exaustiva solicitada**. O AST executado encontra chamadas por nome e apelidos diretos e conta candidatos de chave; ele não prova o fluxo por reexportações, aliases atribuídos, serialização ou templates. A contagem total e por classe é, portanto, **[não verificado]**. Incluo o script efetivamente executado e as limitações ao fim do item 1.

## Item 1 — chamadas e leitores identificados

`medir_padrao` retorna o dicionário de padrão; `propor_escala` retorna `padrao`, `proposta`, `confianca` e `porque`. `_casa_template` retorna um objeto `TipoEscala` ou `None`, não um dicionário. (`app/escala/detector_proposta.py:53`, `:86`, `:99–119`, `:122–123`, `:157–160`)

### Chamadas diretas encontradas pelo AST

| Chamador | Chamada | Classe ou destino |
|---|---|---|
| `propor_escala` — `app/escala/detector_proposta.py:124` | `medir_padrao` | **DECIDE** com o padrão retornado. |
| `propor_escala` — `app/escala/detector_proposta.py:130` | `_casa_template` | **DECIDE** conforme retorna template ou `None`. |
| `coletar` — `app/escala/proposta_pdf.py:43–46` | `propor_escala` | Repassa o resultado ao PDF; não lê uma das chaves nesse sítio. |
| `Command.handle` — `app/escala/management/commands/propor_escala_limbo.py:22–49` | `propor_escala` | **DECIDE** filtro e placar; também exibe. |
| `propostas_do_vinculo` — `app/escala/services/escala_auto.py:117–125` | `propor_escala` | **DECIDE** se emite A2. |
| `anomalias_da_frota` — `app/ponto/services/esmeril_espelho.py:215–247` | `propor_escala` | **DECIDE** se projeta proposta para a lavra. |
| Testes — `app/escala/tests/test_detector_proposta.py:64`, `:82`, `:94`, `:105`, `:119`, `:130`, `:157`, `:180` | `propor_escala` | Chamadas de teste; a de `:130` descarta o retorno. Não as classifico como comportamento de produção. |

Os quatro chamadores externos de produção usam import local; o script também percorreu `__init__.py` e imports relativos como arquivos Python, mas não resolve uma cadeia arbitrária de reexportações. (`app/escala/proposta_pdf.py:45`, `app/escala/management/commands/propor_escala_limbo.py:24`, `app/escala/services/escala_auto.py:119`, `app/ponto/services/esmeril_espelho.py:222`; script abaixo)

### Leituras e propagação identificadas

Uma função pode aparecer em mais de uma classe quando há sítios distintos de decisão e exibição. `dias`, embora fora da lista de chaves pedida, aparece porque governa decisões do fluxo.

| Sítio e função | Chaves lidas e efeito | Classe |
|---|---|---|
| `app/escala/detector_proposta.py:102–105`, `_casa_template` | `entrada`, `saida`, `marcos_moda`: exige pontas e escolhe se quer intervalo; depois filtra horários e intervalo do template. | **DECIDE** |
| `app/escala/detector_proposta.py:125–155`, `propor_escala` | `dias`, `entrada`, `saida`, `marcos_moda`, `marcos_freq`, `dispersao_entrada_min`: decide amostra, ausência de template e confiança; algumas leituras também compõem `porque`. | **DECIDE** |
| `app/escala/proposta_pdf.py:74–80`, `gerar_pdf_propostas_bytes` | `proposta`, `confianca`, `padrao.dias`: separa as seções. | **DECIDE** |
| `app/escala/proposta_pdf.py:120–138`, mesma função | `marcos_moda`, `entrada`, `saida`, `duracao_h`, `dispersao_entrada_min`, `padrao_dias`, `dows`, `proposta`, `porque`: compõe as linhas do PDF. | **SÓ EXIBE** |
| `app/escala/management/commands/propor_escala_limbo.py:37–40`, `handle` | `confianca`, `proposta`: placar e filtro `--so-propostas`. | **DECIDE** |
| `app/escala/management/commands/propor_escala_limbo.py:41–57`, `handle` | `marcos_moda`, `entrada`, `saida`, `duracao_h`, `dispersao_entrada_min`, `padrao_dias`, `dows`, `proposta`, `confianca`, `porque`: saída do comando; `proposta` também escolhe o ramo textual. | **DECIDE** e **SÓ EXIBE**, em sítios diferentes |
| `app/escala/services/escala_auto.py:120–125`, `propostas_do_vinculo` | `proposta`, `confianca`, `padrao.dias`, `porque` e campos da proposta: a confiança alta e o destino diferente habilitam A2; `porque` vira evidência. | **DECIDE** |
| `app/ponto/services/esmeril_espelho.py:241–247`, `anomalias_da_frota` | `proposta`, `confianca`: só projeta proposta existente; guarda ID, código e confiança no retrato retornado. | **DECIDE** |
| `app/ponto/services/esmeril_espelho.py:259–264`, `_destino` | `proposta`, `porque`, `confianca`: escolhe a frase de destino e lê código, descrição e horário. | **DECIDE** |
| `app/ponto/services/esmeril_espelho.py:321–334`, `lavrar` | Persiste o retrato produzido por `anomalias_da_frota`, inclusive a projeção da proposta, no banco e em arquivo. | **GRAVA** |
| `app/escala/services/cadastro_realidade.py:819–827`, `ciclos_dos_destinos` | Lê `tipo_escala_id` da proposta projetada e consulta o ciclo atual dos destinos. | **DECIDE** |
| `app/escala/services/cadastro_realidade.py:830–851`, `botao_da_proposta` | Lê o ID projetado; decide nenhum botão, destino cíclico ou aplicação, considerando vínculo e assinaturas. | **DECIDE** |
| `app/escala/services/cadastro_realidade.py:960–987`, `lista` | Lê o mapa `proposta` da lavra, passa-o a `ciclos_dos_destinos` e `botao_da_proposta` e o coloca na linha. | **DECIDE** |
| `app/templates/escala/cadastro_x_realidade.html:32` | Exibe `x.proposta.codigo` e escolhe botão/link por `x.botao_proposta`. | **SÓ EXIBE** o código; **DECIDE** o ramo do botão |
| `app/escala/views.py:475–505`, `aplicar_proposta_da_lista` | Lê proposta e confiança da lavra, confere o predicado e entrega a proposta à porta de realocação. | **GRAVA**, condicionado à guarda e à porta |

O PDF de propostas existe como consumidor do resultado; a view antiga `propostas_limbo_pdf` redireciona para cadastro × realidade nesta cópia. (`app/escala/proposta_pdf.py:43–46`, `:53–80`; `app/escala/views.py:428`)

**Colisões verificadas:** `entrada` e `saida` em `cadastro_realidade` nas linhas `239–241` vêm de `periodos_do_dia`, não do detector; o `porque` de `validacao.py:1185–1192` pertence a outro dicionário; o propositor em `app/escala/services/propositor.py:136–155` deriva suas próprias pontas de turnos fechados. O fluxo indireto completo de validação e wizard a partir do detector é **[não verificado]**. (`app/escala/services/cadastro_realidade.py:223`, `:238–241`; `app/chamados/services/validacao.py:1185–1192`; `app/escala/services/propositor.py:136–155`)

### Script AST executado

Executado pela entrada padrão com `python3`, sem criar arquivo. Ele imprime chamadas por nome ou apelido direto, chama atributos de mesmo nome de *candidatos* e conta acessos literais por `[]` e `.get()`. **Não é um analisador de fluxo interprocedural:** as contagens de chave incluem colisões; não resolve reexportações nem lê HTML. A conferência dos sítios acima exigiu leitura adicional do código.

```python
import ast
from pathlib import Path
from collections import Counter

names = {'medir_padrao', 'propor_escala', '_casa_template'}
keys = {
    'entrada', 'saida', 'marcos_moda', 'marcos_freq', 'duracao_h',
    'dispersao_entrada_min', 'dows', 'padrao_dias', 'proposta',
    'confianca', 'porque',
}
counts = Counter()

for path in Path('app').rglob('*.py'):
    try:
        tree = ast.parse(path.read_text(encoding='utf-8'), filename=str(path))
    except (SyntaxError, UnicodeError):
        print('ERRO', path)
        continue

    aliases = {}
    for n in ast.walk(tree):
        if isinstance(n, ast.ImportFrom):
            for a in n.names:
                if a.name in names:
                    aliases[a.asname or a.name] = a.name
        elif isinstance(n, ast.Import):
            for a in n.names:
                aliases[a.asname or a.name.split('.')[0]] = a.name

    for n in ast.walk(tree):
        if isinstance(n, ast.Call):
            f = n.func
            if isinstance(f, ast.Name) and (
                f.id in names or f.id in aliases and aliases[f.id] in names
            ):
                print('CALL', path, n.lineno, aliases.get(f.id, f.id))
            elif isinstance(f, ast.Attribute) and f.attr in names:
                print('CALL_CANDIDATO', path, n.lineno, ast.unparse(f))

        if (
            isinstance(n, ast.Subscript)
            and isinstance(n.slice, ast.Constant)
            and n.slice.value in keys
        ):
            counts[n.slice.value] += 1

        if (
            isinstance(n, ast.Call)
            and isinstance(n.func, ast.Attribute)
            and n.func.attr == 'get'
            and n.args
            and isinstance(n.args[0], ast.Constant)
            and n.args[0].value in keys
        ):
            counts[n.args[0].value] += 1

print('CANDIDATOS_DE_CHAVES', dict(sorted(counts.items())))
```

## Item 2 — autoridades e derivações

**(a) Turno e dia da batida.** O registro vivo aponta `turnos_do_colab` para a geometria do turno; seus objetos expõem `data_turno`, `entrada`, `saida`, `batidas` e `aberto`. Para apresentação, `dia_das_batidas` responde também pelo dia de uma batida que nenhum turno reclamou: usa o dia civil em que aconteceu. (`app/core/juizes.py:215–216`; `app/ponto/turnos.py:1–16`, `:43`, `:2902–2909`)

**(b) Entrada e saída.** Para o **realizado**, as pontas vêm do `Turno` produzido pelo pareador. Para o **previsto**, `marcos_turno` encadeia os marcos da célula ou do template; `fim_turno` lê o fim dessa sequência. São perguntas distintas. (`app/ponto/turnos.py:1192–1217`; `app/escala/servico_jornada.py:69–101`; `app/core/juizes.py:220–224`)

`medir_padrao` **poderia chamar interfaces existentes**, sem modificar `parear_turnos` nem o motor: `turnos_de_batidas` alimenta o pareador e remove ecos; `turnos_do_colab` carrega também vínculos, marcos e células. Isso não prova que obteria a escala real sem circularidade: o próprio detector registra que o pareamento depende da escala que se pretende descobrir. (`app/ponto/turnos.py:1515–1535`, `:1850–1858`, `:1984–2000`; `app/escala/detector_proposta.py:18–23`)

- **Entrada sem saída:** o pareador pode retornar `Turno` com entrada, `saida=None` e `aberto=True`. O resultado concreto depende também de marcos e ata. (`app/ponto/turnos.py:312–324`, `:1497–1502`)
- **Batida solta:** não há uma única resposta só pelo horário. Uma `S` órfã pode virar turno sem entrada; uma `E` pode abrir turno; uma `X` sem turno corrente pode não ser reclamada. Para apresentação, a batida não reclamada recebe seu dia civil. (`app/ponto/turnos.py:1377–1397`, `:1402`, `:1495–1498`, `:2902–2909`)

**Derivações paralelas identificadas em `app/escala/`:**

- `detector_proposta.medir_padrao`: primeira e última batidas por data civil. (`app/escala/detector_proposta.py:59–74`)
- `regua_defesa.diagnostico_regua`: desloca madrugada conforme limiar e agrupa para obter pontas; `entradas_fora_do_inicio` toma a primeira por data civil. (`app/escala/regua_defesa.py:118–173`, `:506–531`)
- `utils._match_marcos`: usa papéis do turno quando disponíveis, mas pode derivá-los por alternância; a montagem da grade tem caminhos de alinhamento quando o turno não resolve. (`app/escala/utils.py:595–608`, `:1587–1607`)
- `escala_auto.propostas_do_vinculo`: lê turnos do juiz para entradas e deriva a pausa pela segunda e terceira batidas. (`app/escala/services/escala_auto.py:76–105`)
- `propositor._dias_batidos` e `_horario_mediano`: delegam a `turnos_de_batidas`; a segunda calcula a mediana das pontas dos fechados. (`app/escala/services/propositor.py:106–118`, `:136–155`)
- `semana_do_posto._bateram` e `meio_periodo.observado_meio_periodo` chamam o pareador; este último usa suas pontas. (`app/escala/services/semana_do_posto.py:90–122`; `app/escala/services/meio_periodo.py:120–169`, `:184`)

A lista exaustiva de **todas** as derivações em `app/escala/` é **[não verificado]**.

**Leis e selos localizados:** L-085 fixa o dia de início; L-080 referencia o selo AST que restringe agrupamento por data da batida; L-032 protege a interpretação do intervalo cadastrado; L-115 disciplina uma saída posicional após pausa fechada. O detector está na exceção declarada do selo de data, e o propositor tem selo próprio para consultar o juiz de turnos. O corte do juiz do par também foi localizado. (`app/docs/LEIS.md:80`, `:107`, `:117`, `:153`; `app/ponto/tests/test_contract_no_batida_date.py:29`; `app/escala/tests/test_propositor_turnos_pelo_juiz.py:1–5`; `app/docs/CORTES.md:417`)

## Item 5 — casos pela regra e pelo código

**Convenções dos exemplos:** janela civil local de **02 a 22/11/2026**, inclusive; batidas descritas são supostas apuráveis. Nos casos alternados, os turnos começam nos dias pares. A saída do turno iniciado em 22/11 ocorre em 23/11 e fica fora da consulta civil atual. Pela regra informada e pela L-085, a unidade esperada é o turno e seu dia é o do início. (`app/escala/detector_proposta.py:55–62`; `app/docs/LEIS.md:117`)

**Rastro comum do código:** agrupa por data local (`:60–62`), ordena (`:63–65`), toma primeira e última e suas modas (`:70–75`), calcula duração só em dias com mais de uma batida (`:76–77`) e classifica intervalos entre datas (`:78–95`). O casamento exige pontas dentro de 30 minutos e presença ou ausência de intervalo conforme a moda de marcas; **não testa o ciclo** (`:99–119`). Com pelo menos cinco dias, a proposta depende do catálogo; havendo casamento, a confiança depende de 15 dias, coerência de 80% e dispersão (`:122–157`). Todas as referências deste parágrafo são de `app/escala/detector_proposta.py`.

### (a) 12×36 noturno, duas batidas

**Batidas:** em 02, 04, …, 22/11, entrada 19:00 e saída 07:00 do dia seguinte.

**Pela regra:** 11 inícios de turno, **19:00→07:00**, padrão alternado, template esperado **12×36 noturno sem intervalo**. A existência e o ID desse template no catálogo são **[não verificado]**. (`app/docs/LEIS.md:117`)

**Hoje:** 21 datas civis com uma batida cada; entrada modal **19:00**, saída modal **19:00**, `marcos_moda=1`, frequência 21, duração `None`, dispersão 0 e padrão `semanal`. `_casa_template` procura **19:00→19:00 sem intervalo**, dentro da tolerância. Se existir casamento, confiança **alta**; caso contrário, proposta `None` e confiança `insuficiente`. (`app/escala/detector_proposta.py:60–95`, `:102–119`, `:130–146`)

### (b) Mesmo turno, quatro batidas

**Batidas:** em cada data par, 19:00; no dia seguinte, 01:00, 02:00 e 07:00.

**Pela regra:** **19:00→07:00**, pausa **01:00–02:00**, alternado, template esperado **12×36 noturno com intervalo**; existência e ID no catálogo **[não verificado]**. (`app/docs/LEIS.md:117`; `app/docs/LEIS.md:80`)

**Hoje:** 21 datas: 11 com uma marca e dez com três; entrada e saída modais **19:00→19:00**, `marcos_moda=1`, frequência 11, duração média **6 h**, dispersão 0 e padrão `semanal`. Procura **19:00→19:00 sem intervalo**. Se casar, confiança **média**, pois 11/21 não chega a 80%; senão, `None`/`insuficiente`. (`app/escala/detector_proposta.py:70–95`, `:102–119`, `:130–156`)

### (c) 12×36 diurno, controle

**Batidas:** nos dias pares, **07:00 e 19:00**.

**Pela regra:** **07:00→19:00**, alternado, template esperado **12×36 diurno sem intervalo**; existência e ID **[não verificado]**. (`app/docs/LEIS.md:117`)

**Hoje:** 11 dias, **07:00→19:00**, duas marcas em todos, duração **12 h**, dispersão 0 e `alternado`. Procura **07:00→19:00 sem intervalo**; se casar, confiança **média** por haver só 11 dias; senão, `None`/`insuficiente`. O horário medido permanece correto neste controle, mas o código não exige que o template escolhido seja 12×36. (`app/escala/detector_proposta.py:70–95`, `:99–119`, `:130–156`)

### (d) Noturno 6×1

**Batidas:** de segunda a sábado em cada uma das três semanas, entrada **22:00**, saída **06:00** do dia seguinte; folga de início aos domingos.

**Pela regra:** 18 turnos, **22:00→06:00**, padrão semanal **6×1**, template noturno correspondente; existência e ID **[não verificado]**. (`app/docs/LEIS.md:117`)

**Hoje:** 21 datas civis, entrada modal **06:00**, saída modal **22:00**, `marcos_moda=2`, frequência 15, duração média **16 h**, dispersão 0 e `semanal`. Procura **06:00→22:00 sem intervalo**. Se casar, confiança **média** porque 15/21 < 80%; senão, `None`/`insuficiente`. (`app/escala/detector_proposta.py:70–95`, `:102–119`, `:130–156`)

### (e) Turno aberto e batida solta no meio da janela

**Batidas:** as do controle (c), mais uma entrada aberta **21/11 às 07:00** e uma batida solta **21/11 às 12:00**, tomadas aqui como fatos independentes do exemplo.

**Pela regra:** os turnos completos continuam **07:00→19:00**, alternados; o aberto tem **07:00→saída desconhecida** e a solta não deve ser inventada como sua saída. Se o juiz classificaria essas duas batidas assim, sem mais tipo, ata e contexto, é **[não verificado]**. A política de agregação desses fatos ao propor um template também é **[não verificado]**. (`app/docs/LEIS.md:117`; `app/ponto/turnos.py:1377–1397`, `:1495–1502`)

**Hoje:** junta as duas batidas de 21/11 como se fossem pontas do mesmo dia: 12 dias, modal **07:00→19:00**, duas marcas em todos, duração média **11,4 h**, dispersão 0 e `alternado`. Procura **07:00→19:00 sem intervalo**; se casar, confiança **média** por 12 dias; senão, `None`/`insuficiente`. (`app/escala/detector_proposta.py:60–95`, `:102–119`, `:130–156`)

### (f) Troca de diurno para noturno

**Batidas:** em 02, 04, …, 12/11, **07:00 e 19:00**; em 14, 16, …, 22/11, **19:00 e 07:00 do dia seguinte**.

**Pela regra:** os turnos anteriores à troca são **07:00→19:00** e os posteriores **19:00→07:00**, ambos alternados; os templates esperados por trecho são os respectivos 12×36 diurno e noturno. A regra para escolher uma **única proposta para toda a janela**, e a existência dos templates, são **[não verificado]**. (`app/docs/LEIS.md:117`; `app/docs/LEIS.md:171`)

**Hoje:** 15 datas civis, modal **07:00→19:00**, `marcos_moda=1`, frequência 9, duração média **12 h**, dispersão 0 e padrão `semanal`. Procura **07:00→19:00 sem intervalo**; se casar, confiança **média** por variação de marcas; senão, `None`/`insuficiente`. (`app/escala/detector_proposta.py:60–95`, `:102–119`, `:130–156`)

**Perguntas de lei: N = [não verificado].** Foram identificadas quatro **candidatas**, sem prova de que nenhuma outra lei escrita as responda: tratamento de aberto/solta na amostra; saída além da borda da janela; segmentação ou abstenção na troca de regime; e desempate entre templates com horários iguais e ciclos diferentes. Foram consultados, entre outros, L-085, L-041, L-032, L-115 e os cortes END-L1/L2/L3; os trechos localizados não resolvem expressamente essas quatro decisões do detector. (`app/docs/LEIS.md:80`, `:117`, `:153`, `:171`; `app/docs/CORTES.md:551–553`)

## 19/11 10:39 · sessao f92271236998 · DST-L2-JZ1 · verificador · origem=Code

**Veredito: BLOQUEIA.** A v3 impede a rejeição de J3 quando ela aparece na leitura travada, mas não impede a retratação do chamado nessa janela; também passa a perguntar duas vezes ao juiz de irmãs no mesmo caminho, e o selo ainda aceita resultado lido sem participar da decisão. Há ainda um teste anterior cujo resultado esperado contradiz a nova regra. ([reconciliador.py:549](app/chamados/reconciliador.py:549), [reconciliador.py:578](app/chamados/reconciliador.py:578), [justificativa.py:347](app/ponto/services/justificativa.py:347), [teste C6:119](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:119), [teste anterior:336](app/ponto/tests/test_dst_l2_jz1_rejeita_justificativa.py:336))

### 1. Os dois bloqueios da v2

**Janela: parcialmente curada.** No ato de aprovar, o escritor agora aplica o portão mesmo com `portao_de_retencao=False` e passa a cada chamado os IDs derivados da própria leitura travada. Assim, uma J3 presente nessa leitura não é auto-rejeitada. ([justificativa.py:340](app/ponto/services/justificativa.py:340), [justificativa.py:347](app/ponto/services/justificativa.py:347), [justificativa.py:350](app/ponto/services/justificativa.py:350), [justificativa.py:354](app/ponto/services/justificativa.py:354))

**Contraexemplo:** `_retratar` não vê J3; ela passa a integrar o vínculo antes da leitura travada; `_ato_da_retratacao` já transicionou o chamado para `resolvido` quando o escritor a vê. J3 fica pendente, mas o chamado não fica retido, e a chamada de rejeição não devolve uma recusa que desfaça a transição. O teste novo chama o escritor diretamente e não verifica o estado do chamado nessa sequência. A possibilidade concreta dessa concorrência sob as travas e o isolamento de produção é **[não verificado]**. ([reconciliador.py:549](app/chamados/reconciliador.py:549), [reconciliador.py:578](app/chamados/reconciliador.py:578), [reconciliador.py:588](app/chamados/reconciliador.py:588), [reconciliador.py:590](app/chamados/reconciliador.py:590), [teste C6:72](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:72))

**Selo: parcialmente curado.** O delta rejeita atribuição cujo nome nunca aparece como leitura e identifica os acessos ORM `.objects`, `.filter`, `.exclude` e `.select_for_update`, inclusive no exemplo com alias e `**kwargs`. ([teste C6:118](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:118), [teste C6:125](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:125), [teste C6:138](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:138), [teste C6:183](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:183))

**Contraexemplo que passa pela análise estática:** `x = pendentes_do_vinculo([ch]); bool(x); return ch.justificativa_relacionada_id is not None`. `x` é lido, mas essa leitura não influencia o retorno; o atributo `_id` é permitido. O selo também não demonstra fluxo de dados até a decisão. Execução desse mutante é **[não verificado]**. ([teste C6:119](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:119), [teste C6:125](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:125), [teste C6:136](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:136))

### 2. Escritores, leitores e significados

| Item | Quem escreve ou fornece | Quem lê | Significado observado |
|---|---|---|---|
| `portao_de_retencao` | Padrão `True` na assinatura; `_ato_da_retratacao` passa `False`. ([justificativa.py:292](app/ponto/services/justificativa.py:292), [reconciliador.py:588](app/chamados/reconciliador.py:588)) | O `if` do escritor. ([justificativa.py:347](app/ponto/services/justificativa.py:347)) | `False` agora dispensa o portão **somente fora** do ato de aprovar. Portanto, o parâmetro sozinho não determina mais se o portão roda. ([justificativa.py:316](app/ponto/services/justificativa.py:316), [justificativa.py:347](app/ponto/services/justificativa.py:347)) |
| `_APROVANDO` | Default `None`; `no_ato_de_aprovar` faz `set(id)` e `reset(token)`. Os três pontos observados entram nesse contexto. ([reconciliador.py:426](app/chamados/reconciliador.py:426), [reconciliador.py:430](app/chamados/reconciliador.py:430), [justificativa.py:140](app/ponto/services/justificativa.py:140), [justificativa.py:467](app/ponto/services/justificativa.py:467), [views.py:1795](app/ponto/views.py:1795)) | Retenção, seleção do vínculo, portão do escritor e trilha. ([reconciliador.py:456](app/chamados/reconciliador.py:456), [justificativa.py:286](app/ponto/services/justificativa.py:286), [justificativa.py:347](app/ponto/services/justificativa.py:347), [reconciliador.py:496](app/chamados/reconciliador.py:496)) | ID da justificativa em aprovação; a seleção exclui esse ID. A trilha deduplica apenas pela justificativa pendente, logo seu `em_aprovacao` pode ser de tentativa anterior. ([justificativa.py:287](app/ponto/services/justificativa.py:287), [reconciliador.py:499](app/chamados/reconciliador.py:499)) |
| `alheias` | O escritor fornece `_lidas[pk]`; quando recebe `None`, a própria retenção consulta e monta a lista. ([justificativa.py:350](app/ponto/services/justificativa.py:350), [reconciliador.py:460](app/chamados/reconciliador.py:460)) | Retenção copia e testa os IDs, escolhendo o ponteiro se presente ou o primeiro ID. ([reconciliador.py:463](app/chamados/reconciliador.py:463)) | IDs pendentes ligados ao chamado, excluindo a justificativa em aprovação. Com parâmetro fornecido, a função confia no chamador; não valida o conteúdo. ([justificativa.py:283](app/ponto/services/justificativa.py:283), [justificativa.py:286](app/ponto/services/justificativa.py:286), [reconciliador.py:460](app/chamados/reconciliador.py:460)) |
| `donos` | O escritor transforma `donos_por_j` em `{pk da justificativa: chamados}`. ([justificativa.py:340](app/ponto/services/justificativa.py:340), [justificativa.py:344](app/ponto/services/justificativa.py:344)) | Montagem de `_com` e `_lidas`, veto e resultado `feitas`. ([justificativa.py:349](app/ponto/services/justificativa.py:349), [justificativa.py:354](app/ponto/services/justificativa.py:354), [justificativa.py:359](app/ponto/services/justificativa.py:359)) | Chamados do lote que alcançam cada justificativa por ponteiro ou origem; uma candidata compartilhada é preservada se qualquer dono estiver retido. ([justificativa.py:288](app/ponto/services/justificativa.py:288), [justificativa.py:354](app/ponto/services/justificativa.py:354)) |
| `retidos` | O escritor inicializa conjunto vazio e o substitui pelos PKs cujo portão retornou retenção. ([justificativa.py:345](app/ponto/services/justificativa.py:345), [justificativa.py:351](app/ponto/services/justificativa.py:351)) | O laço de rejeição. ([justificativa.py:353](app/ponto/services/justificativa.py:353)) | São chamados que **protegem candidatas contra rejeição**, não necessariamente chamados cuja transição foi impedida: o escritor roda depois do fecho. ([justificativa.py:311](app/ponto/services/justificativa.py:311), [reconciliador.py:578](app/chamados/reconciliador.py:578)) |
| `Justificativa.status` | Default `pendente`; há aprovação nas portas e no lote, rejeição automática e humana, e retorno a pendente na regularização. ([models.py:159](app/ponto/models.py:159), [justificativa.py:205](app/ponto/services/justificativa.py:205), [justificativa.py:508](app/ponto/services/justificativa.py:508), [views.py:1799](app/ponto/views.py:1799), [justificativa.py:356](app/ponto/services/justificativa.py:356), [justificativa.py:408](app/ponto/services/justificativa.py:408), [views.py:1878](app/ponto/views.py:1878), [regularizacao.py:105](app/ponto/services/regularizacao.py:105)) | A seleção do vínculo lê `pendente`; outros escritores também o testam. ([justificativa.py:283](app/ponto/services/justificativa.py:283), [entrada_sincronizada.py:40](app/ponto/services/entrada_sincronizada.py:40), [triagem_batida.py:607](app/ponto/services/triagem_batida.py:607)) | Nos trechos citados é estado da justificativa; concordância de **todos** os leitores e escritores, inclusive acessos indiretos, é **[não verificado]**. |

### 3. Consultas no caminho

Para um chamado que passa pela primeira retenção, cuja transição tem sucesso, sem contar SQL da própria transição, sinais, resolução, auditoria ou save:

| Situação | v2 | v3 |
|---|---|---|
| **No ato de aprovar, e a leitura travada encontra candidata** | Duas consultas ao vínculo: uma sem trava na retenção e uma travada no escritor; uma avaliação do juiz de irmãs. | As mesmas duas consultas ao vínculo; **duas** avaliações do juiz de irmãs. |
| **Fora do ato** | Uma consulta travada ao vínculo e uma avaliação do juiz de irmãs. | Igual à v2. |
| **No ato, sem candidata na leitura travada** | Duas consultas ao vínculo e uma avaliação do juiz de irmãs. | Igual à v2: o escritor retorna antes do segundo portão. |

A segunda avaliação é feita por `retencao_da_retratacao`, chamada pelo escritor após a primeira chamada de `retratar_com_motivo`. Cada avaliação de irmãs consulta as perguntas e pode fazer uma consulta adicional de prefetch de decisões quando há perguntas; portanto, no caso com candidata, o componente de leitura descrito é **3 + prefetch condicional na v2** e **4 + até dois prefetches condicionais na v3**, não um total SQL do ato completo. A contagem efetivamente executada e efeitos de caches ou sinais são **[não verificado]**. ([reconciliador.py:453](app/chamados/reconciliador.py:453), [reconciliador.py:549](app/chamados/reconciliador.py:549), [justificativa.py:340](app/ponto/services/justificativa.py:340), [justificativa.py:341](app/ponto/services/justificativa.py:341), [justificativa.py:351](app/ponto/services/justificativa.py:351), [juizes.py:2115](app/chamados/juizes.py:2115), [models.py:1999](app/chamados/models.py:1999))

O `alheias=` evita uma **terceira** consulta ao vínculo, mas não evita repetir a pergunta sobre irmãs. ([reconciliador.py:453](app/chamados/reconciliador.py:453), [reconciliador.py:460](app/chamados/reconciliador.py:460), [justificativa.py:351](app/ponto/services/justificativa.py:351))

### 4. Chamadores e comportamento fora da aprovação

Os chamadores de produção encontrados para o escritor são **`_ato_da_retratacao`**, com portão `False`, e os lotes **`_fech`** e **`_porm`** do cartório, com o padrão `True`. Fora do ato, o primeiro continua sem segundo portão; os outros continuam aplicando-o. Fora do ato, `retencao_da_retratacao` não usa `alheias`, de modo que a passagem da lista não altera sua decisão observada. ([reconciliador.py:588](app/chamados/reconciliador.py:588), [cartorio.py:1038](app/ponto/services/cartorio.py:1038), [cartorio.py:1585](app/ponto/services/cartorio.py:1585), [justificativa.py:347](app/ponto/services/justificativa.py:347), [reconciliador.py:456](app/chamados/reconciliador.py:456))

Os chamadores diretos de produção encontrados para a retenção são **`encerrar_com_retencao`**, **`retratar_com_motivo`** — usado por `_retratar` —, o **escritor de rejeição** e a **vigia `reconciliar_chamados`**. Fora da aprovação, não identifiquei mudança de decisão nesses sítios pelo delta. Chamadas dinâmicas não identificadas são **[não verificado]**. ([reconciliador.py:514](app/chamados/reconciliador.py:514), [reconciliador.py:528](app/chamados/reconciliador.py:528), [reconciliador.py:549](app/chamados/reconciliador.py:549), [justificativa.py:351](app/ponto/services/justificativa.py:351), [reconciliar_chamados.py:79](app/chamados/management/commands/reconciliar_chamados.py:79))

### 5. Classes de reprovação

- **Segundo juiz:** não vi predicado novo de irmãs; a retenção delega ao juiz existente. Entretanto, a v3 **executa o mesmo juiz duas vezes** no caminho descrito. ([reconciliador.py:452](app/chamados/reconciliador.py:452), [justificativa.py:351](app/ponto/services/justificativa.py:351))
- **Segundo escritor fora da porta:** há escritores automáticos diretos preexistentes em entrada sincronizada e triagem; o delta apresentado não os introduz. Sua admissibilidade pela regra da obra é **[não verificado]**. ([entrada_sincronizada.py:38](app/ponto/services/entrada_sincronizada.py:38), [triagem_batida.py:605](app/ponto/services/triagem_batida.py:605))
- **Relógio fora da borda:** não identifiquei leitura de relógio nas linhas novas de produção do delta; alcance transitivo completo é **[não verificado]**. ([reconciliador.py:460](app/chamados/reconciliador.py:460), [justificativa.py:347](app/ponto/services/justificativa.py:347))
- **Derivação paralela:** a implementação observada deriva `_lidas` de `donos`, que vem da função comum; o selo ainda permite o contraexemplo de resultado lido e ignorado descrito acima. ([justificativa.py:340](app/ponto/services/justificativa.py:340), [justificativa.py:350](app/ponto/services/justificativa.py:350), [teste C6:119](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:119))
- **RED:** por inspeção, sem a condição nova `or _APROVANDO.get() is not None`, o portão não roda com `False` e J3 é rejeitada; essa condição, junto da passagem de `_lidas`, faz o cenário novo preservar J3. O teste não simula as duas leituras nem verifica o chamado. Sua execução e o resultado efetivo do mutante são **[não verificado]**. ([justificativa.py:347](app/ponto/services/justificativa.py:347), [justificativa.py:351](app/ponto/services/justificativa.py:351), [teste C6:72](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:72))
- **Teste anterior conflitante:** `test_LEI_APROVANDO_controle_outra_pendente_morre_sem_portao` espera `rejeitada` com contexto de aprovação ativo e portão `False`; a v3 passa a reter essa outra pendente. Por inspeção, a expectativa conflita com a alteração; execução é **[não verificado]**. ([teste anterior:336](app/ponto/tests/test_dst_l2_jz1_rejeita_justificativa.py:336), [justificativa.py:347](app/ponto/services/justificativa.py:347), [justificativa.py:351](app/ponto/services/justificativa.py:351))
- **Ordem de trava:** o caminho toma a trava ordenada antes do ato, e o escritor seleciona justificativas com `select_for_update`; a nova consulta de irmãs ocorre depois dessa seleção. Não demonstrei inversão nova, nem ausência de inversão em todos os caminhos; efeitos em produção são **[não verificado]**. ([reconciliador.py:561](app/chamados/reconciliador.py:561), [justificativa.py:283](app/ponto/services/justificativa.py:283), [justificativa.py:340](app/ponto/services/justificativa.py:340), [justificativa.py:351](app/ponto/services/justificativa.py:351))
- **Campo com dois significados:** `retidos` no escritor significa proteção da justificativa, embora a retenção anterior signifique impedir a retratação; são efeitos diferentes no mesmo ato. `portao_de_retencao=False` também passa a depender do contexto para determinar a execução do portão. ([justificativa.py:311](app/ponto/services/justificativa.py:311), [justificativa.py:347](app/ponto/services/justificativa.py:347), [reconciliador.py:549](app/chamados/reconciliador.py:549), [reconciliador.py:578](app/chamados/reconciliador.py:578))

Não executei testes nem consultei banco ou logs; resultados de execução e estado de produção são **[não verificado]**. [nao verificado]

## 19/11 10:38 · sessao b9caceed74dc · ESCALA-NAO-DESCREVE · sonda · origem=Code

A tela usa `lista()` e recebe todas as empresas visíveis, sem um filtro de empresa específico: empresas ativas, exceto as de diagnóstico identificadas pelo prefixo de CNPJ. O usuário precisa ter `ver_escala` para acessar a view; a sonda reproduz o filtro de empresas, mas não executa essa verificação de permissão. A composição efetiva das empresas em produção é **[não verificado]**. [app/escala/views.py:522](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/views.py:522), [app/colaboradores/queries.py:78](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/colaboradores/queries.py:78)

A sonda abaixo usa a data local de execução para o cadastro atual. Para `padrao_dias`, mede novamente a janela de 60 dias anterior à data registrada em `lavrado_em`, pois a proposta guardada na lavra não contém esse campo. O resultado medido agora pode diferir do que seria obtido quando a lavra foi criada, se as batidas tiverem mudado. [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:241), [app/ponto/services/esmeril_espelho.py:246](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:246)

```python
import datetime
import logging
import re

from django.db import connection
from django.utils import timezone
from django.utils.dateparse import parse_datetime

from colaboradores.models import Colaborador
from colaboradores.queries import empresas_visiveis
from escala.alimentacao import q_vinculo_cobre_a_data, vinculo_do_dia
from escala.detector_proposta import medir_padrao
from escala.models import EscalaColaborador, TipoEscala
from escala.services.cadastro_realidade import lista
from escala.servico_jornada import turno_cruza_meia_noite
from inteligencia.models import MetricaSnapshot
from ponto.services.esmeril_espelho import CHAVE_LAVRA, ler_lavra


ERROS = 0
UNIVERSO = None
CONTADOS = 0
HOJE_ALVO = datetime.date(2026, 11, 19)


def erro(etapa, exc, colab_pk=None):
    """Identifica a excecao sem imprimir mensagens que possam conter dados pessoais."""
    global ERROS
    ERROS += 1
    tipo = re.sub(r"[^A-Za-z0-9_]", "", type(exc).__name__)
    tb = exc.__traceback__
    while tb is not None and tb.tb_next is not None:
        tb = tb.tb_next
    linha = tb.tb_lineno if tb is not None else "?"
    cid = colab_pk if isinstance(colab_pk, int) else "?"
    print("ERRO etapa=%s colab_pk=%s tipo=%s linha=%s" %
          (etapa, cid, tipo, linha))


class CapturaErrosLista(logging.Handler):
    """Captura as excecoes que lista() trata internamente."""
    def emit(self, record):
        if record.levelno >= logging.ERROR:
            if record.exc_info and record.exc_info[1] is not None:
                erro("lista_interno", record.exc_info[1])
            else:
                erro("lista_interno", RuntimeError())


def somente_select(execute, sql, params, many, context):
    # Fail closed: inclusive WITH e comandos de sessao sao recusados.
    if not sql.lstrip().upper().startswith("SELECT "):
        raise RuntimeError("SQL_NAO_SELECT_BLOQUEADO")
    return execute(sql, params, many, context)


def horario_lavra(detalhe):
    valor = detalhe.get("lavrado_em")
    if not valor:
        raise ValueError("LAVRADO_EM_AUSENTE")
    instante = parse_datetime(valor)
    if instante is None:
        raise ValueError("LAVRADO_EM_INVALIDO")
    if timezone.is_naive(instante):
        raise ValueError("LAVRADO_EM_SEM_FUSO")
    return timezone.localtime(instante)


def seleciona_vinculo(cid, dia):
    # Mesma alimentacao e mesma chamada feitas por _vinculos() da tela.
    candidatos = list(
        EscalaColaborador.objects.filter(
            q_vinculo_cobre_a_data(dia), colaborador_id=cid
        ).select_related("tipo_escala").order_by("pk")
    )
    return vinculo_do_dia(cid, dia, candidatos)


def cruza_template(template, dia):
    hi, hf, _, _ = template.marcos_do_dia(dia)
    if hi is None or hf is None:
        raise ValueError("MARCOS_INICIO_OU_FIM_AUSENTES")
    return turno_cruza_meia_noite(hi, hf)


def executar():
    global UNIVERSO, CONTADOS

    if getattr(connection, "schema_name", None) != "juliani":
        raise RuntimeError("SCHEMA_NAO_E_JULIANI")

    agora = timezone.now()
    hoje = timezone.localtime(agora).date()
    print("DATA_CADASTRO=%s" % hoje.isoformat())

    # Item 4: mesmo conjunto de snapshots lido por ler_lavra().
    print("ITEM_4")
    detalhe = None
    instante_lavra = None
    try:
        qs = MetricaSnapshot.objects.filter(
            chave=CHAVE_LAVRA, escopo="frota", escopo_id=None
        )
        n_snapshots = qs.count()
        ultimo = qs.order_by("-data_ref").first()
        print("SNAPSHOTS=%d" % n_snapshots)
        if ultimo is None:
            print("ULTIMA_LAVRA=AUSENTE")
        else:
            print(
                "ULTIMA_LAVRA snapshot_pk=%d data_ref=%s calculado_em=%s"
                % (
                    ultimo.pk,
                    ultimo.data_ref.isoformat(),
                    timezone.localtime(ultimo.calculado_em).isoformat(),
                )
            )
            if not isinstance(ultimo.detalhe, dict):
                raise ValueError("DETALHE_NAO_E_OBJETO")
            if "orfas" not in ultimo.detalhe:
                print("ULTIMA_LAVRA_ORFAS=AUSENTE")
            elif not isinstance(ultimo.detalhe["orfas"], dict):
                raise ValueError("ORFAS_NAO_E_MAPA")
            else:
                print("ULTIMA_LAVRA_LINHAS_ORFAS=%d" %
                      len(ultimo.detalhe["orfas"]))

        de_hoje = list(qs.filter(data_ref=HOJE_ALVO).order_by("pk"))
        print("LAVRAS_DATA_ALVO=%d" % len(de_hoje))
        for snapshot in de_hoje:
            try:
                if not isinstance(snapshot.detalhe, dict):
                    raise ValueError("DETALHE_NAO_E_OBJETO")
                presente = (
                    "orfas" in snapshot.detalhe
                    and snapshot.detalhe["orfas"] is not None
                )
                print("LAVRA_DATA_ALVO snapshot_pk=%d orfas_preenchido=%s" %
                      (snapshot.pk, "SIM" if presente else "NAO"))
            except Exception as exc:
                erro("lavra_data_alvo", exc)

        # Respeita tambem a guarda da lavra de producao na sombra.
        detalhe = ler_lavra()
        if detalhe is None:
            print("LAVRA_LIDA_PELA_TELA=AUSENTE")
        else:
            instante_lavra = horario_lavra(detalhe)
            print("LAVRADO_EM=%s" % instante_lavra.isoformat())
    except Exception as exc:
        erro("item_4", exc)

    # Item 3: a funcao real da tela monta o universo.
    print("ITEM_3")
    linhas = None
    logger = logging.getLogger("escala")
    captura = CapturaErrosLista()
    handlers_anteriores = logger.handlers[:]
    propaga_antes = logger.propagate
    nivel_antes = logger.level
    try:
        empresas = set(empresas_visiveis().values_list("pk", flat=True))
        print("EMPRESA_PKS=%s" % ",".join(str(pk) for pk in sorted(empresas)))
        logger.handlers = [captura]
        logger.propagate = False
        logger.setLevel(logging.ERROR)
        dados = lista(hoje=hoje, empresas=empresas, agora=agora)
        linhas = [] if dados is None else dados["linhas"]
        print("LINHAS_TELA=%d" % len(linhas))
    except Exception as exc:
        erro("lista", exc)
    finally:
        logger.handlers = handlers_anteriores
        logger.propagate = propaga_antes
        logger.setLevel(nivel_antes)

    if linhas is None:
        print("UNIVERSO_PROPOSTAS=NAO_VERIFICADO")
        return

    linhas_por_colab = {linha["colab_id"]: linha for linha in linhas}
    com_proposta = [linha for linha in linhas
                    if linha.get("proposta") is not None]
    UNIVERSO = len(com_proposta)
    tabela = {(False, False): 0, (False, True): 0,
              (True, False): 0, (True, True): 0}
    divergentes = []

    def processar(cid, proposta, fora):
        global CONTADOS
        vinculo_pk = None
        try:
            ec = seleciona_vinculo(cid, hoje)
            if ec is None:
                raise ValueError("SEM_VINCULO_VIGENTE")
            vinculo_pk = ec.pk
            if not fora:
                linha = linhas_por_colab[cid]
                if linha["template_id"] != ec.tipo_escala_id:
                    raise ValueError("TEMPLATE_DIVERGE_DA_LISTA")
            if not isinstance(proposta, dict):
                raise ValueError("PROPOSTA_AUSENTE_OU_INVALIDA")
            prop_pk = proposta.get("tipo_escala_id")
            if not isinstance(prop_pk, int):
                raise ValueError("PROPOSTA_SEM_TEMPLATE_PK")
            prop = TipoEscala.objects.get(pk=prop_pk)

            # Cadastro atual = template vivo do vinculo, para o dia local.
            cruza_cad = cruza_template(ec.tipo_escala, hoje)
            cruza_prop = cruza_template(prop, hoje)

            if instante_lavra is None:
                raise ValueError("DATA_DA_LAVRA_INDISPONIVEL")
            dia_lavra = instante_lavra.date()
            colab = Colaborador.objects.get(pk=cid)
            padrao = medir_padrao(
                colab,
                dia_lavra - datetime.timedelta(days=60),
                dia_lavra - datetime.timedelta(days=1),
            )["padrao_dias"]
            if padrao not in ("sem dados", "alternado", "semanal", "indefinido"):
                raise ValueError("PADRAO_INESPERADO")

            print(
                "LINHA vinculo_pk=%d colab_pk=%d ciclo_cad=%s "
                "cruza_cad=%s template_proposto_pk=%d ciclo_prop=%s "
                "cruza_prop=%s padrao_dias=%s %s"
                % (
                    ec.pk, cid, ec.tipo_escala.tipo_ciclo,
                    int(cruza_cad), prop.pk, prop.tipo_ciclo,
                    int(cruza_prop), padrao,
                    "FORA_DA_LISTA" if fora else "NA_LISTA",
                )
            )
            if not fora:
                tabela[(cruza_cad, cruza_prop)] += 1
                CONTADOS += 1
                if cruza_cad != cruza_prop:
                    divergentes.append((ec.pk, cid))
        except Exception as exc:
            erro("linha_fora" if fora else "linha", exc, cid)
            print("LINHA vinculo_pk=%s colab_pk=%d resultado=ERRO %s" %
                  (vinculo_pk if vinculo_pk is not None else "?", cid,
                   "FORA_DA_LISTA" if fora else "NA_LISTA"))

    for linha in com_proposta:
        try:
            processar(linha["colab_id"], linha["proposta"], False)
        except Exception as exc:
            erro("iteracao_lista", exc)

    # Os dois pedidos sao mostrados mesmo sem proposta ou fora da lista.
    for cid in (451, 898):
        if cid in linhas_por_colab:
            linha = linhas_por_colab[cid]
            if linha.get("proposta") is None:
                print("LINHA vinculo_pk=? colab_pk=%d proposta=AUSENTE NA_LISTA"
                      % cid)
            # Se tem proposta, ja foi impressa no laco acima.
        else:
            proposta = None
            if isinstance(detalhe, dict):
                propostas = detalhe.get("proposta")
                if isinstance(propostas, dict):
                    proposta = propostas.get(str(cid))
            processar(cid, proposta, True)

    print("TABELA_2X2 somente_contados=SIM")
    for cad in (False, True):
        for prop in (False, True):
            print("cad=%d prop=%d n=%d" % (int(cad), int(prop), tabela[(cad, prop)]))
    print("DIVERGENTES vinculo_pk:colab_pk=%s" %
          ",".join("%d:%d" % par for par in divergentes))


try:
    with connection.execute_wrapper(somente_select):
        executar()
except Exception as exc:
    erro("execucao", exc)
finally:
    print("ERROS=%d UNIVERSO=%s CONTADOS=%d" %
          (ERROS, UNIVERSO if UNIVERSO is not None else "NAO_VERIFICADO",
           CONTADOS))
```

**Como ler os resultados e limites:**

- `UNIVERSO` conta as linhas da tela com proposta não nula; `CONTADOS` conta as que puderam ser classificadas nas duas travessias e medidas. A tabela e os divergentes abrangem somente `CONTADOS`. Linhas extras de 451 e 898 não alteram a tabela. A lista é por colaborador e não expõe o PK do vínculo; a sonda recupera o escolhido usando a mesma consulta de candidatos e o mesmo juiz de `_vinculos()`. [app/escala/services/cadastro_realidade.py:753](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:753), [app/escala/services/cadastro_realidade.py:977](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:977)
- A travessia usa `turno_cruza_meia_noite(hi, hf)`, autoridade explícita para o par previsto: fim menor **ou igual** ao início cruza. Os horários são os do template vivo para o dia local, inclusive eventual override por dia da semana; não são o DNA congelado de uma célula histórica. [app/escala/servico_jornada.py:104](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/servico_jornada.py:104), [app/escala/models.py:326](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:326)
- `medir_padrao()` lê batidas apuráveis, converte seus instantes para hora local e classifica os intervalos entre dias com batida. [app/escala/detector_proposta.py:53](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:53)
- A carga é `MetricaSnapshot.detalhe["orfas"]`: `anomalias_da_frota()` a monta, `lavrar()` chama `gravar_lavra()`, e esta grava o snapshot; o comando `esmeril_espelho --apply` chama o escritor. O cron configurado é **08:30 diariamente**. A sonda chama somente o leitor e consulta o modelo; não chama esses escritores. [app/ponto/services/esmeril_espelho.py:215](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:215), [app/ponto/services/esmeril_espelho.py:321](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:321), [app/ponto/services/esmeril_espelho.py:342](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:342), [app/ponto/management/commands/esmeril_espelho.py:15](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/esmeril_espelho.py:15), [app/config/crons.py:640](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/config/crons.py:640)
- `calculado_em` é o instante do snapshot; `lavrado_em` é o instante registrado dentro do retrato. `LINHAS_ORFAS` é o tamanho do mapa medido, inclusive valores zero. Para 19/11, `orfas_preenchido=SIM` significa chave presente e não nula, inclusive mapa vazio. [app/inteligencia/models.py:95](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/inteligencia/models.py:95), [app/ponto/services/esmeril_espelho.py:224](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:224)
- A execução, os valores de produção e a existência da lavra de 19/11 são **[não verificado]**. A sonda informa o tipo e a linha de cada exceção, sem imprimir a mensagem livre, que poderia conter dados pessoais. `lista()` também captura algumas exceções internamente; a sonda intercepta as registradas no logger `escala` e as conta. [app/escala/services/cadastro_realidade.py:878](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:878)

## 19/11 10:38 · sessao e2029d6cccde · - · - · origem=Ronald (tmux)

Cálculos dos seis casos, condições de catálogo, textos de confiança e referências de lei enviados ao agente principal.

## 19/11 10:38 · sessao 4453b4917876 · - · - · origem=Ronald (tmux)

A investigação do item 2 foi enviada ao agente principal, com as autoridades, os casos de turno aberto e batida solta, as derivações em `app/escala/` e os contratos localizados.

## 19/11 10:38 · sessao c10d22884f27 · - · - · origem=Ronald (tmux)

Enviei ao agente principal o inventário dos chamadores e leitores identificados, o fluxo até a lavra e a lista, as colisões verificadas e o script AST executado em leitura.

A contagem total por classe permanece **[não verificado]**: o script executado inventaria imports, mas não resolve reexports, fluxo interprocedural nem templates; além disso, alguns chamadores apenas repassam ou descartam o retorno e não cabem claramente nas três classes solicitadas.

## 19/11 10:31 · sessao 4643c4b2b1cc · ESPELHO-VERDADE · leitura · origem=Code

O ponto central é que `InsumosDoDono` chama o oráculo **sem `pares`**. Assim, as `horas_do_dia` entregues ao juiz são as batidas agrupadas pela data da **primeira batida de cada turno formado pelo próprio oráculo**; não são uma lista agrupada necessariamente pelo dia de calendário de cada batida nem pelo dia da jornada vindo dos pares do motor ([e6_oraculo.py:149](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:149), [nucleo.py:186](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:186), [nucleo.py:236](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:236)).

**Referência do censo.** O diretório fornecido não contém metadados Git acessíveis: `git rev-parse HEAD` retornou “not a git repository”. Portanto, a identidade entre os arquivos lidos e o HEAD/hash informado é **[não verificado]**. A busca por chamadas textuais em arquivos Python encontrou os quatro sítios de aplicação abaixo; chamadas dinâmicas ou fora dos arquivos pesquisados permanecem **[não verificado]**.

### 1. Chamadas na aplicação

| Sítio | `pares` e origem de `ts` | Chave de dia |
|---|---|---|
| [e6_oraculo.py:153](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:153), `InsumosDoDono.__init__` | **Não passa `pares`**. `ts` vem dos timestamps de `batidas_apuraveis`, convertidos para a timezone corrente, entre `ini − 12h` e `fim + 1 dia + 14h`; a consulta usa limites inclusivos e exclui retratadas ([e6_oraculo.py:149](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:149), [turnos.py:2892](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2892)). | No ramo por sequência, `str(tu[0].date())`, para cada turno construído após os cortes ([nucleo.py:215](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:215), [nucleo.py:236](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:236)). |
| [diff_calculador.py:414](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/diff_calculador.py:414) | **Passa `pares=_pares`**. No padrão, `_pares=None`; com `--pares-da-autoridade`, recebe `_ins['pares']` de `insumos_do_calculador(c, aut.resultado, ...)`. `ts` vem de consulta direta a `Batida`, sem retratadas, ordenada, convertida para horário local; o limite superior é exclusivo ([diff_calculador.py:270](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/diff_calculador.py:270), [diff_calculador.py:287](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/diff_calculador.py:287), [diff_calculador.py:312](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/diff_calculador.py:312), [diff_calculador.py:325](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/diff_calculador.py:325)). | Com `None`, a primeira data do turno por sequência. Com os pares da porta, o terceiro elemento é o dia obtido de `resultado.dia_da_jornada[id(p)]`; períodos sem esse dia são pulados ([alimentacao.py:73](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/alimentacao.py:73), [alimentacao.py:133](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/alimentacao.py:133), [nucleo.py:134](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:134)). |
| [alimentacao.py:449](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/alimentacao.py:449), `rubricas_por_dia` | **Passa `pares`**, convertendo `insumos['pares']` em lista; `ts` é a lista do parâmetro `batidas_locais`, ou lista vazia se ausente. No DIFF, recebe o `ts` descrito acima; no fechamento, recebe os timestamps locais de `batidas`, carregadas por `batidas_apuraveis` ([alimentacao.py:353](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/alimentacao.py:353), [diff_calculador.py:417](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/diff_calculador.py:417), [fechamento.py:633](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/fechamento.py:633), [fechamento.py:1271](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/fechamento.py:1271)). | Para os insumos montados por `insumos_do_calculador`, os segmentos levam o dia do mapa `dia_da_jornada`; o núcleo usa esse terceiro elemento ([alimentacao.py:305](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/alimentacao.py:305), [alimentacao.py:73](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/alimentacao.py:73), [nucleo.py:141](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:141)). |
| [periodo.py:356](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/periodo.py:356), `periodo_do_dia` | **Passa `pares`** de `segmentos_do_par(entrada, saida, intervalos, dia)` e passa `ts=()`, vazio. Se entrada ou saída for `None`, retorna antes dessa chamada ([periodo.py:291](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/periodo.py:291), [periodo.py:353](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/periodo.py:353)). | Cada segmento recebe o `dia` passado à função; no caminho de `reler`, ele vem do mapa `resultado.dia_da_jornada`, por objeto de período ([periodo.py:596](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/periodo.py:596), [periodo.py:611](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/periodo.py:611), [periodo.py:744](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/periodo.py:744)). |

Nos dois ramos, `brutas` é uma contagem separada por `str(t.date())` de cada instante de `ts`. No ramo com pares, as demais saídas usam o terceiro elemento do par quando presente; se ausente ou `None`, usam `str(entrada.date())` ([nucleo.py:117](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:117), [nucleo.py:134](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:134), [nucleo.py:235](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:235)).

**Testes:** também há chamadas diretas com dados de teste nos arquivos [test_calculador_regras.py:165](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_calculador_regras.py:165), [test_e6_oraculo_julga_impar.py:44](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_e6_oraculo_julga_impar.py:44), [test_e6_oraculo_le_a_retratacao.py:34](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_e6_oraculo_le_a_retratacao.py:34) e [test_e6_turno_nao_cresce_sem_fim.py:72](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_e6_turno_nao_cresce_sem_fim.py:72). Elas exercitam sequência e, em alguns casos, pares de teste; a mesma regra de chave do núcleo se aplica ([test_calculador_regras.py:171](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_calculador_regras.py:171), [test_e6_turno_nao_cresce_sem_fim.py:160](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_e6_turno_nao_cresce_sem_fim.py:160), [nucleo.py:141](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:141), [nucleo.py:236](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:236)). Este parágrafo agrupa os testes por arquivo, não enumera cada chamada individual.

### 2. Caminho de `horas_do_dia` até o dono

1. `InsumosDoDono.__init__` consulta `batidas_apuraveis`, transforma os timestamps em horário local, chama `minutos_do_oraculo(ts)` e guarda `self.oraculo.horas` em `self.horas` ([e6_oraculo.py:128](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:128), [e6_oraculo.py:149](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:149)).
2. `minutos_do_oraculo` ordena `ts`, forma turnos pelos cortes de sequência e, para cada turno, acrescenta **todas as suas batidas formatadas como `HH:MM`** à lista sob `str(tu[0].date())` ([nucleo.py:109](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:109), [nucleo.py:186](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:186), [nucleo.py:236](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:236)).
3. `InsumosDoDono.dono(data)` faz `k = str(data)` e passa `self.horas.get(k, [])` como primeiro argumento de `dono_da_divergencia`: se não houver lista para a chave, passa `[]`. Pode chamar primeiro com CXR falso e, se necessário, chamar novamente com o resultado de `cxr()`; o argumento de horas é o mesmo ([e6_oraculo.py:182](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:182)).
4. O juiz usa a **paridade do comprimento** dessa lista no ramo de BATIDA, depois das guardas de cadastro e de `batida_fora_de_turno`; não interpreta os valores `HH:MM` nessa condição ([e6_oraculo.py:98](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:98)). No próprio comando, o objeto é criado com o espelho já obtido e `dono` é chamado com a data convertida da chave do dia ([e6_oraculo.py:328](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:328), [e6_oraculo.py:378](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:378)).

### 3. Limites da leitura

- **HEAD/hash:** correspondência com `f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33` **[não verificado]**; o Git não estava disponível nesse diretório.
- **Execução e dados reais:** não executei testes nem consultei banco; resultados por colaborador, frequência de chaves divergentes e impacto na classificação **[não verificado]**. [nao verificado]
- **Cobertura do censo:** os quatro sítios da tabela são as chamadas textuais de aplicação encontradas em Python; resolução dinâmica, código externo e o censo individualizado de cada chamada de teste **[não verificado]**. [nao verificado]
- **Demais insumos do dono:** o caminho acima acompanha `horas_do_dia`; não é uma auditoria completa da produção de `cadastro`, fase, CXR ou `batida_fora_de_turno`, embora esses insumos participem da chamada ([e6_oraculo.py:185](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:185)).

## 19/11 10:31 · sessao 1d64acfa6833 · DST-L2-JZ1 · verificador · origem=Code

**Veredito: BLOQUEIA**, por dois pontos que permanecem na c6: a retenção pode ser decidida com uma fotografia e a rejeição com outra; e o selo ainda aceita uma chamada cujo resultado não participa da decisão, permitindo uma derivação paralela. A triagem registra a janela como fila, mas não demonstra que ela deixou de contrariar a proteção pretendida. ([reconciliador.py:544](app/chamados/reconciliador.py:544), [reconciliador.py:581](app/chamados/reconciliador.py:581), [justificativa.py:337](app/ponto/services/justificativa.py:337), [teste C6:90](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:90))

### 1. Achados da volta 1

- **Janela entre retenção e rejeição: não curado; a justificativa da triagem procede apenas quanto ao comportamento anterior.** Antes da c6, a rejeição já abrangia justificativas pendentes por origem; a extração mantém esse universo. Mas agora a c6 promete reter o chamado quando houver justificativa alheia pendente no ato de aprovar: a consulta da retenção ocorre antes do `atomic`, e a rejeição consulta de novo com trava e sem portão. Uma justificativa que passe a integrar o vínculo entre essas leituras pode ser rejeitada apesar da proteção. A ocorrência, o isolamento e a frequência em produção são **[não verificado]**. ([justificativa.py:267](app/ponto/services/justificativa.py:267), [reconciliador.py:445](app/chamados/reconciliador.py:445), [reconciliador.py:544](app/chamados/reconciliador.py:544), [reconciliador.py:549](app/chamados/reconciliador.py:549), [reconciliador.py:581](app/chamados/reconciliador.py:581), [justificativa.py:337](app/ponto/services/justificativa.py:337))
- **Texto “aponta”: curado para novas gravações.** A frase agora admite ponteiro ou origem, conforme o vínculo que a consulta aceita. Registros históricos já gravados não são reescritos por essa mudança. ([reconciliador.py:458](app/chamados/reconciliador.py:458), [reconciliador.py:492](app/chamados/reconciliador.py:492))
- **Selo sem mordida: parcialmente curado, ainda não curado.** As quatro amostras declaradas são reconhecidas pela lógica do verificador, mas ela continua sem garantir uso efetivo do resultado. A execução dos testes é **[não verificado]**. ([teste C6:90](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:90), [teste C6:122](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:122))
- **Documentação de 37 pares: curada como descrição da alegação.** A docstring distingue 37 pares em uma foto e 46 = 46 em outra. As fotos, os hashes e a comparação de produção são **[não verificado]**. ([teste C6:4](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:4))

### 2. Mordida do selo

As quatro variantes do teste recebem ao menos uma violação: ausência de chamada aproveitada, atributo proibido, importação do modelo ou keyword do vínculo. Os dois sítios vivos têm chamada em expressão usada e não contêm os padrões proibidos, portanto passam pela análise estática apresentada; a execução é **[não verificado]**. ([teste C6:90](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:90), [teste C6:117](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:117), [teste C6:122](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:122), [reconciliador.py:457](app/chamados/reconciliador.py:457), [justificativa.py:337](app/ponto/services/justificativa.py:337))

Ainda escapa, por exemplo, `x = pendentes_do_vinculo(chs)` seguido de uma decisão independente que nunca usa `x`. O selo considera a atribuição suficiente; uma consulta paralela também pode esconder o vínculo em `filter(**d)` e usar um alias do modelo importado fora da função. Assim, ele não prova o contrato de um só predicado. ([teste C6:90](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:90), [teste C6:95](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:95))

### 3. Trilha de retenção

`registrar_retencao` monta a descrição e os campos `justificativa_pendente` e `em_aprovacao`; `core.observ.evento` grava a descrição em `LogAuditoria.descricao` e os campos em `valor_depois`. Antes de gravar, o próprio registrador lê o último `valor_depois` e compara apenas `justificativa_pendente`. ([reconciliador.py:465](app/chamados/reconciliador.py:465), [reconciliador.py:490](app/chamados/reconciliador.py:490), [core/observ.py:133](app/core/observ.py:133))

A tela de auditoria exibe a descrição, e sua busca filtra texto por `descricao__icontains`. Não encontrei leitor de código que exija literalmente a palavra antiga para essa ação; buscas humanas salvas, integrações externas e leitores dinâmicos são **[não verificado]**. Uma busca textual por “aponta” naturalmente não encontrará as novas descrições. ([core/views.py:701](app/core/views.py:701), [core/views.py:713](app/core/views.py:713), [log_auditoria.html:73](app/templates/core/log_auditoria.html:73))

Há uma limitação adicional da trilha: se a justificativa alheia for a mesma e `em_aprovacao` mudar, a comparação considera o evento repetido e não grava o novo ID. Portanto, o campo gravado descreve o ato da linha existente, não necessariamente a última tentativa. ([reconciliador.py:491](app/chamados/reconciliador.py:491), [reconciliador.py:494](app/chamados/reconciliador.py:494))

### 4. Significado dos campos lidos ou gravados

- **`Justificativa.status`:** o modelo inicia em `pendente`; a consulta seleciona esse estado, e a rejeição grava `rejeitada`. Os leitores e escritores citados na volta 1 tratam esses valores como estado da justificativa; não identifiquei segundo significado nos trechos examinados. Outros acessos indiretos são **[não verificado]**. ([models.py:159](app/ponto/models.py:159), [justificativa.py:283](app/ponto/services/justificativa.py:283), [justificativa.py:351](app/ponto/services/justificativa.py:351))
- **`chamado_origem` e `justificativa_relacionada_id`:** são relações distintas: a origem identifica o chamado que gerou a justificativa; o ponteiro identifica a justificativa que responde ao chamado. A consulta deliberadamente aceita **qualquer uma** das duas, sem atribuir a ambas o mesmo sentido. A preferência pelo ponteiro no detalhe também é explícita. ([models.py:173](app/ponto/models.py:173), [vinculos.py:81](app/chamados/services/vinculos.py:81), [justificativa.py:279](app/ponto/services/justificativa.py:279), [reconciliador.py:458](app/chamados/reconciliador.py:458))
- **`_APROVANDO` / `em_aprovacao`:** o contexto carrega o ID da justificativa cujo ato está em curso; o predicado a exclui e a trilha grava esse mesmo ID, sujeito à deduplicação acima. ([reconciliador.py:426](app/chamados/reconciliador.py:426), [reconciliador.py:430](app/chamados/reconciliador.py:430), [justificativa.py:286](app/ponto/services/justificativa.py:286), [reconciliador.py:491](app/chamados/reconciliador.py:491))
- **`justificativa_pendente`:** é o ID escolhido de uma justificativa alheia pendente ligada por ponteiro ou origem; o texto novo preserva esse significado. A chave sozinha não informa qual relação existia. ([reconciliador.py:445](app/chamados/reconciliador.py:445), [reconciliador.py:458](app/chamados/reconciliador.py:458), [reconciliador.py:491](app/chamados/reconciliador.py:491))
- **`Justificativa.descricao`:** a rejeição acrescenta uma anotação ao texto existente, sem substituir seu conteúdo. O diff também preserva a escolha de não atualizar `atualizado_em`; o próprio contrato documenta que leitores desse timestamp têm consequências distintas. ([justificativa.py:300](app/ponto/services/justificativa.py:300), [justificativa.py:351](app/ponto/services/justificativa.py:351))

A concordância de **todos** os consumidores, inclusive externos e dinâmicos, é **[não verificado]**.

### 5. Classes de reprovação

- **Segundo juiz / derivação paralela:** não identifiquei outro predicado nos dois sítios alterados: ambos chamam `pendentes_do_vinculo`. O selo, porém, não impediria toda futura duplicação. A classificação de outros contadores como duplicação proibida é **[não verificado]**. ([reconciliador.py:457](app/chamados/reconciliador.py:457), [justificativa.py:337](app/ponto/services/justificativa.py:337), [justificativa.py:359](app/ponto/services/justificativa.py:359), [teste C6:90](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:90))
- **Segundo escritor fora da porta:** existem escritores diretos preexistentes em entrada sincronizada e triagem; o diff fornecido não os introduz. Sua admissibilidade pela lei aplicável é **[não verificado]**. ([entrada_sincronizada.py:38](app/ponto/services/entrada_sincronizada.py:38), [triagem_batida.py:605](app/ponto/services/triagem_batida.py:605))
- **Relógio fora da borda:** não identificado nos trechos novos de produção examinados; alcance transitivo completo **[não verificado]**. ([reconciliador.py:453](app/chamados/reconciliador.py:453), [justificativa.py:262](app/ponto/services/justificativa.py:262))
- **Selo sem mordida completa:** presente, pela variante de resultado atribuído e ignorado. ([teste C6:90](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:90))
- **Ordem de trava:** a extração preserva a consulta travada da rejeição, mas a retenção anterior continua fora da transação e sujeita à janela descrita. O efeito das travas e do isolamento em produção é **[não verificado]**. ([justificativa.py:283](app/ponto/services/justificativa.py:283), [justificativa.py:337](app/ponto/services/justificativa.py:337), [reconciliador.py:544](app/chamados/reconciliador.py:544))
- **Campo com dois significados:** a afirmação incorreta de que toda justificativa alheia era apontada foi corrigida no texto novo. Não identifiquei novo duplo significado nos campos examinados; a deduplicação de `em_aprovacao` limita o que se pode inferir da última linha. ([reconciliador.py:491](app/chamados/reconciliador.py:491))

Não executei testes nem consultei banco ou logs; resultados de execução e estado de produção são **[não verificado]**. [nao verificado]

## 19/11 10:31 · sessao c09fa2cce16b · ESPELHO-VERDADE · sonda · origem=Code

O dado fornecido não contém as batidas nem os resultados necessários para preencher a tabela: as 14 linhas e as contagens só serão conhecidas após a execução. [não verificado]

Há uma distinção relevante para a coluna **b**: o script entrega ao pareador exatamente o universo e o recorte usados por `InsumosDoDono`. Essa chamada isolada não reproduz toda a alimentação do caminho do sistema: `turnos_do_colab` segmenta a janela e fornece marcos, papéis da ata e outros insumos. Portanto, a coluna **b** responde à comparação sobre as mesmas batidas do oráculo; ela **não certifica que o motor formou aqueles mesmos pares**. [e6_oraculo.py:149](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:149) [turnos.py:1923](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1923) [turnos.py:1977](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1977)

Outro limite da solicitação: **a sonda abaixo é entregue somente como texto**. A porta `bin/sonda_leitura.sh --sombra` também cria ou reescreve arquivos e administra credenciais e permissões; executá-la não satisfaria uma proibição absoluta de criar, editar ou apagar qualquer arquivo. [sonda_leitura.sh:115](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/bin/sonda_leitura.sh:115) [sonda_leitura.sh:153](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/bin/sonda_leitura.sh:153) [sonda_leitura.sh:216](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/bin/sonda_leitura.sh:216)

```python
# Sonda EV v3, item 2.
# Executar no envelope Django da sombra, tenant juliani.
# Somente IDs, datas, horas, resultados e classes de excecao.
import sys
sys.dont_write_bytecode = True

import logging

# Mesma guarda contra handlers de arquivo do anexo v2.
for _logger in [logging.getLogger()] + [
    x for x in logging.Logger.manager.loggerDict.values()
    if isinstance(x, logging.Logger)
]:
    for _handler in list(_logger.handlers):
        if isinstance(_handler, logging.FileHandler):
            _logger.removeHandler(_handler)

import datetime as dt
import json
import os

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from ponto import janelas
from ponto.management.commands import e6_oraculo as e6
from ponto.services.espelho import espelho_do_colab
from ponto.supra_juiz import cadastro_do_dia_carregado
from ponto.turnos import batidas_apuraveis, parear_turnos


# Competencia 2026-11 e empresa conforme o anexo v2.
DADOS = (
    (2, 200, "2026-10-29"),
    (2, 200, "2026-11-02"),
    (2, 218, "2026-11-14"),
    (2, 296, "2026-11-02"),
    (2, 382, "2026-11-10"),
    (2, 516, "2026-10-23"),
    (2, 727, "2026-11-10"),
    (2, 841, "2026-11-16"),
    (3, 110, "2026-11-12"),
    (3, 113, "2026-10-30"),
    (3, 113, "2026-11-01"),
    (4, 37,  "2026-11-14"),
    (3, 112, "2026-10-21"),
    (3, 112, "2026-10-22"),
)


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")

    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")

    especificacao = os.environ.get("TESTE_CPUSET", "4-7")
    permitidas = set()
    for parte in especificacao.split(","):
        limites = parte.strip().split("-")
        if len(limites) == 1:
            permitidas.add(int(limites[0]))
        elif len(limites) == 2:
            inicio, fim = map(int, limites)
            if inicio > fim:
                raise RuntimeError("cpuset_invalido")
            permitidas.update(range(inicio, fim + 1))
        else:
            raise RuntimeError("cpuset_invalido")

    afinidade = os.sched_getaffinity(0)
    if not afinidade or not afinidade.issubset(permitidas):
        raise RuntimeError("cpuset")

    with connection.cursor() as cursor:
        cursor.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY"
        )
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()

    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def instante_local(valor):
    if valor is None:
        return None
    if timezone.is_naive(valor):
        raise ValueError("timestamp_sem_timezone")
    return timezone.localtime(valor).isoformat()


def json_seguro(valor):
    return json.dumps(valor, ensure_ascii=True, separators=(",", ":"))


def preparar(empresa_id, pk, hoje, agora):
    empresa = Empresa.objects.get(pk=empresa_id)
    colab = Colaborador.objects.get(pk=pk, empresa_id=empresa_id)
    ini, fim = janelas.janela_fechamento(11, 2026, empresa)

    espelho = espelho_do_colab(colab, hoje, ini, fim, agora=agora)
    registros = {
        str(item.get("data"))[:10]: item
        for item in (espelho.get("dias") or [])
    }
    insumos = e6.InsumosDoDono(
        colab, ini, fim, hoje, espelho=espelho, agora=agora
    )

    # Exatamente os limites inclusivos usados em InsumosDoDono.
    tzc = timezone.get_current_timezone()
    lo = timezone.make_aware(
        dt.datetime.combine(ini, dt.time()) - dt.timedelta(hours=12), tzc
    )
    hi = timezone.make_aware(
        dt.datetime.combine(fim + dt.timedelta(days=1), dt.time())
        + dt.timedelta(hours=14),
        tzc,
    )
    batidas = list(batidas_apuraveis(colab, lo, hi))
    instantes = [timezone.localtime(b.timestamp) for b in batidas]

    # Confere que a segunda leitura produz as mesmas horas do oraculo.
    releitura = e6.minutos_do_oraculo(instantes)
    if dict(releitura.horas) != dict(insumos.oraculo.horas):
        raise RuntimeError("releitura_oraculo_diferente")

    # Chamada direta sobre as mesmas batidas, sem alimentacao de ata ou marcos.
    turnos = parear_turnos(batidas)

    # O proprio nucleo transforma os pares em horas por dia_da_jornada.
    # Ele ignora pares sem entrada; eles continuam visiveis em "pares".
    pares_nucleo = [
        (
            timezone.localtime(t.entrada.timestamp) if t.entrada else None,
            timezone.localtime(t.saida.timestamp) if t.saida else None,
            t.data_turno,
        )
        for t in turnos
    ]
    horas_pares = e6.minutos_do_oraculo(
        instantes, pares=pares_nucleo
    ).horas

    return colab, ini, fim, registros, insumos, turnos, horas_pares


def medir(colab, dia, registro, insumos, turnos, horas_pares):
    chave = str(dia)
    horas_a = list(insumos.oraculo.horas.get(chave, []))
    horas_b = list(horas_pares.get(chave, []))

    pares = [
        {
            "entrada": instante_local(t.entrada.timestamp) if t.entrada else None,
            "saida": instante_local(t.saida.timestamp) if t.saida else None,
            "dia_da_jornada": str(t.data_turno) if t.data_turno else None,
        }
        for t in turnos
        if t.data_turno == dia
    ]

    # Mesmos outros insumos que InsumosDoDono.dono; somente horas mudam.
    kw = dict(
        batida_fora_de_turno=e6.batida_fora_de_turno(
            colab, dia, insumos.turnos()
        ),
        fase_deslocada=chave in insumos.fase(),
        **cadastro_do_dia_carregado(colab.pk, dia),
    )
    cxr = chave in insumos.cxr()
    dono_a = e6.dono_da_divergencia(horas_a, cxr, **kw)
    dono_b = e6.dono_da_divergencia(horas_b, cxr, **kw)

    # Verifica a chamada direta contra a montagem oficial dos insumos.
    if dono_a != insumos.dono(dia):
        raise RuntimeError("dono_original_diferente")

    # Campo produzido pelo caminho de realizado do espelho.
    minutos_realizados = (
        None if registro is None else registro.get("minutos_realizados")
    )
    # Leitor do E6, inclusive o fallback para pago_h.
    minutos_e6, fonte_e6 = e6.minutos_do_espelho(registro)

    return {
        "horas_oraculo": horas_a,
        "pares": pares,
        "horas_derivadas_pares": horas_b,
        "dono_oraculo": dono_a,
        "dono_pares": dono_b,
        "mudou": dono_a != dono_b,
        "minutos_realizados_espelho": minutos_realizados,
        "minutos_leitor_e6": minutos_e6,
        "fonte_leitor_e6": fonte_e6,
    }


def main():
    casos = [(empresa, pk, dt.date.fromisoformat(dia))
             for empresa, pk, dia in DADOS]
    if len(casos) != 14 or len(set(casos)) != 14:
        print("ERRO etapa=UNIVERSO tipo=RuntimeError")
        print("CONTADOR=0 UNIVERSO=14 CONFERE=n")
        print("ERROS=1")
        return 1

    contador = 0
    erros = 0
    mudou = 0
    nao_mudou = 0
    etapa = "GUARDA"

    try:
        with transaction.atomic():
            guarda_sombra()
            agora = timezone.now()
            hoje = timezone.localtime(agora).date()
            cache = {}

            for empresa_id, pk, dia in casos:
                contador += 1
                try:
                    # Savepoint: erro de banco em um caso nao invalida os demais.
                    with transaction.atomic():
                        chave = (empresa_id, pk)
                        if chave not in cache:
                            preparado = preparar(empresa_id, pk, hoje, agora)
                        else:
                            preparado = cache[chave]

                        colab, ini, fim, registros, insumos, turnos, horas_b = preparado
                        if not ini <= dia <= fim:
                            raise RuntimeError("dia_fora_da_janela")
                        resultado = medir(
                            colab, dia, registros.get(str(dia)),
                            insumos, turnos, horas_b,
                        )

                    # Nao guardar uma carga cujo savepoint falhou.
                    cache[chave] = preparado
                    if resultado["mudou"]:
                        mudou += 1
                    else:
                        nao_mudou += 1
                    print(
                        "CASO colab_pk=%d data=%s resultado=%s"
                        % (pk, dia, json_seguro(resultado))
                    )
                except Exception as exc:
                    erros += 1
                    # Mensagens de excecao podem conter dados pessoais.
                    print(
                        "CASO colab_pk=%d data=%s ERRO tipo=%s"
                        % (pk, dia, type(exc).__name__)
                    )

            confere = (
                contador == 14
                and mudou + nao_mudou + erros == contador
            )
            print(
                "CONTAGEM dono_muda=%d dono_nao_muda=%d "
                "casos_com_erro=%d"
                % (mudou, nao_mudou, erros)
            )
            print(
                "CONTADOR=%d UNIVERSO=14 CONFERE=%s"
                % (contador, "s" if confere else "n")
            )
            if not confere:
                erros += 1

    except Exception as exc:
        erros += 1
        print("ERRO etapa=%s tipo=%s" % (etapa, type(exc).__name__))
        print(
            "CONTAGEM dono_muda=%d dono_nao_muda=%d casos_com_erro=%d"
            % (mudou, nao_mudou, contador - mudou - nao_mudou)
        )
        print("CONTADOR=%d UNIVERSO=14 CONFERE=n" % contador)

    print("ERROS=%d" % erros)
    print("FIM_SONDA_EV_V3")
    return 1 if erros else 0


sys.exit(main())
```

As colunas chamam ou leem estas autoridades:

- **Horas do oráculo:** `InsumosDoDono.oraculo.horas`, produzido por `minutos_do_oraculo` com `batidas_apuraveis` no intervalo de início menos 12 horas até o fim mais um dia e 14 horas, com limites inclusivos. [e6_oraculo.py:149](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:149) [turnos.py:2872](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2872)
- **Pares e dia:** `parear_turnos`, sem argumentos de marcos ou ata; `dia_da_jornada` é o campo `data_turno` devolvido. Para obter `horas_derivadas_pares`, a sonda chama o ramo `pares` de `minutos_do_oraculo`; esse ramo ignora uma saída órfã sem entrada, embora ela permaneça visível em `pares`. [turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43) [turnos.py:1192](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1192) [nucleo.py:119](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:119)
- **Donos:** duas chamadas reais a `dono_da_divergencia`, alterando apenas a lista de horas; a primeira ainda é conferida contra `InsumosDoDono.dono`. Os demais insumos seguem a montagem dessa função. [e6_oraculo.py:53](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:53) [e6_oraculo.py:182](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:182)
- **Espelho:** `minutos_realizados_espelho` lê o campo produzido pelo caminho `_realizado_do_dia_tela` do próprio espelho. Separadamente, `minutos_leitor_e6` chama `minutos_do_espelho`, que pode recorrer a `pago_h`; a fonte acompanha o valor. [espelho.py:267](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:267) [espelho.py:689](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:689) [espelho.py:797](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:797) [e6_oraculo.py:242](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:242)

A execução, as 14 linhas, as contagens e eventuais exceções permanecem **[não verificado]**. A comparação também não prova a causa de uma divergência nem reproduz o pareamento contextual completo do motor. [turnos.py:1977](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1977)

## 19/11 10:29 · sessao 800c94e98edf · ESPELHO-VERDADE · leitura · origem=Code

**Não é possível responder “sim” ou “não” para nenhum dos sete dias com os anexos fornecidos.** Falta, para cada um, o dado que mostre se há sobra da ata a classificar e se ela pertence ao balde pausa. [não verificado]

O critério disponível no brief é a **sobra**: ele descreve a pausa como uma razão pretendida para a instância de batida processada, sujeita à precedência de dia sem trabalho e de rajada; o carimbo de intervalo, por si só, não liga a hora da ata à instância. O brief diz ainda que essas razões são propostas, não retornos demonstrados do código atual. [não verificado — anexo `brief_cadeia20.md`, seções 1 e 2]

| Colaborador / data | No universo do censo, balde pausa? | Razão para o dia trazida pelo brief |
|---|---|---|
| 252 / 2026-11-09 | Não consta [não verificado] | Não consta [não verificado] |
| 252 / 2026-11-10 | Não consta [não verificado] | Não consta [não verificado] |
| 252 / 2026-11-11 | Não consta [não verificado] | Não consta [não verificado] |
| 252 / 2026-11-13 | Não consta [não verificado] | Não consta [não verificado] |
| 252 / 2026-11-17 | Não consta [não verificado] | Não consta [não verificado] |
| 884 / 2026-11-08 | Não consta [não verificado] | Não consta [não verificado] |
| 114 / 2026-11-08 | Não consta [não verificado] | Não consta [não verificado] |

A ausência desses dias na lista da `sonda4` não prova que estejam fora do censo de razões: aquela lista é de sobras **sem batida viva no mesmo minuto do dia civil da célula**, segundo a hipótese descrita no brief, e não uma lista de todas as sobras. [não verificado — anexos `brief_cadeia20.md`, seção 4, e `sondas_cadeia20.txt`]

## 19/11 10:28 · sessao 5da7167e2d20 · CADEIA-20-PASSO-1 · leitura · origem=Code

**A existência da batida perguntada permanece [não verificado].** O anexo traz a sonda anterior, mas não traz o resultado desta medição. A condição da premissa existe: quando `_iv` é verdadeiro, a batida só entra em `cur["batidas"]` se `cur` não for `None`; em seguida o laço continua. [app/ponto/turnos.py:1371] [não verificado]

No lote `processar_cartorio`, o cartório entrega a `julgar_colab` todas as células passadas do colaborador, ordenadas por data; o corpo usa a primeira e a última como `d0` e `d1`. Ele carrega `batidas_apuraveis` de `d0 − 1 dia`, inclusive, até `d1 + 3 dias`, exclusivo, e chama `montar_grade_prevista_periodo` com `ref=d1+1` e `dias=(d1-d0)+2`. [app/ponto/management/commands/processar_cartorio.py:59] [app/ponto/services/cartorio.py:1156] [app/ponto/services/cartorio.py:1185] [app/ponto/services/cartorio.py:1201]

Na grade por turno, `_ini_al = ref − dias`, portanto `d0 − 1`, e o limite final é `ref = d1 + 1`. A grade chama `turnos_do_colab` primeiro com as atas gravadas e depois com as atas desta montagem em memória; esta segunda resposta é usada no realizado e na reconciliação anterior à lavra. O wrapper aplica seu próprio *pad*, segmenta por vínculo e chama `parear_turnos`; ele não usa simplesmente a lista `bs` passada à grade. A ata lavrada recebe as horas de `dia["orfas"]`. [app/escala/utils.py:1317] [app/escala/utils.py:1590] [app/escala/utils.py:1891] [app/escala/utils.py:1909] [app/ponto/turnos.py:1850] [app/ponto/turnos.py:1867] [app/ponto/turnos.py:1977] [app/ponto/services/cartorio.py:749] [app/ponto/services/cartorio.py:1898]

A sonda abaixo intercepta **a segunda chamada real** de `turnos_do_colab` dentro da grade, sem reconstruir o pareamento. Para evitar chamar uma batida de sem papel quando ela pertence a um turno aberto ou a um turno com apenas uma borda, `em_par` conta qualquer batida presente em `Turno.batidas`; é, portanto, um critério conservador, mais amplo que as bordas de um par fechado. `em_par` e `em_sobra_da_ata` podem se sobrepor. [app/ponto/turnos.py:312] [app/ponto/turnos.py:1495]

O texto foi preparado para o envelope Django do anexo e **não foi executado nem testado**. [não verificado]

```python
from collections import Counter
from datetime import datetime, time, timedelta
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa
from escala import utils as escala_utils
from escala.models import CelulaDia
from ponto.janelas import janela_fechamento
import ponto.turnos as turnos


cont = Counter()
erros = 0
linhas_emitidas = 0
tz = timezone.get_current_timezone()


def inicio_local(dia):
    return timezone.make_aware(datetime.combine(dia, time.min), tz)


def horas_da_ata(cel):
    ata = cel.ata
    if not isinstance(ata, dict) or "orfas" not in ata:
        raise ValueError("ata sem lista de orfas")

    horas = ata["orfas"]
    if not isinstance(horas, list):
        raise ValueError("lista de orfas invalida")

    for hora in horas:
        if not isinstance(hora, str) or len(hora) != 5:
            raise ValueError("hora invalida")
        if datetime.strptime(hora, "%H:%M").strftime("%H:%M") != hora:
            raise ValueError("hora invalida")

    return set(horas)


def medir_colaborador(emp, colab, cels, ini, fim, agora):
    # Mesmo lote de celulas passadas de processar_cartorio.
    d0, d1 = cels[0].data, cels[-1].data
    alvos = [
        cel for cel in cels
        if ini <= cel.data <= fim and cel.veredito_em is not None
    ]
    if not alvos:
        return Counter(), []

    # Exclui a celula inteira da medicao se sua ata nao permite
    # responder se o minuto consta na lista de orfas.
    atas = {}
    locais = Counter()
    validas = []
    for cel in alvos:
        try:
            atas[cel.pk] = horas_da_ata(cel)
            validas.append(cel)
        except (TypeError, ValueError):
            locais["celulas_ata_indeterminada"] += 1

    if not validas:
        return locais, []

    j_ini = inicio_local(d0 - timedelta(days=1))
    j_fim = inicio_local(d1 + timedelta(days=3))
    bs = list(
        turnos.batidas_apuraveis(
            colab, j_ini, j_fim - timedelta(microseconds=1)
        )
    )
    ref = d1 + timedelta(days=1)
    dias = (d1 - d0).days + 2
    ini_turnos = ref - timedelta(days=dias)

    chamadas = []
    original = turnos.turnos_do_colab

    def capturar(*args, **kwargs):
        resultado = original(*args, **kwargs)
        chamadas.append((args, kwargs, resultado))
        return resultado

    # A propria grade executa as chamadas com seus insumos e atas.
    with patch.object(turnos, "turnos_do_colab", side_effect=capturar):
        escala_utils.montar_grade_prevista_periodo(
            colab, bs, dias=dias, ref=ref, agora=agora
        )

    # Falha fechada se o caminho da grade mudar.
    if len(chamadas) != 2:
        raise ValueError("numero inesperado de chamadas")
    args, kwargs, resultado = chamadas[1]
    if (
        len(args) != 3
        or args[0].pk != colab.pk
        or args[1] != ini_turnos
        or args[2] != ref
        or "celulas" not in kwargs
    ):
        raise ValueError("chamada inesperada")

    pks_em_turno = {
        batida.pk
        for turno in resultado
        for batida in turno.batidas
    }

    # As batidas medidas sao as apuraveis no dia civil de cada celula
    # lavrada; a carga multi-dia do pareador foi feita pela propria grade.
    por_data = {cel.data: cel for cel in validas}
    batidas = turnos.batidas_apuraveis(
        colab,
        inicio_local(min(por_data)),
        inicio_local(max(por_data) + timedelta(days=1))
        - timedelta(microseconds=1),
    )

    exemplos = []
    locais["celulas_medidas"] += len(validas)
    for batida in batidas:
        local = timezone.localtime(batida.timestamp, tz)
        cel = por_data.get(local.date())
        if cel is None:
            continue

        hora = local.strftime("%H:%M")
        em_par = batida.pk in pks_em_turno
        em_sobra = hora in atas[cel.pk]

        locais["batidas_no_universo"] += 1
        locais["em_par"] += int(em_par)
        locais["em_sobra_da_ata"] += int(em_sobra)
        if not em_par and not em_sobra:
            locais["sem_papel"] += 1
            if len(exemplos) < 20:
                exemplos.append(
                    (
                        emp.pk,
                        cel.pk,
                        colab.pk,
                        cel.data,
                        hora,
                        batida.pk,
                    )
                )

    return locais, exemplos


def sonda():
    global erros, linhas_emitidas

    agora = timezone.now()
    hoje = timezone.localtime(agora, tz).date()

    for emp in Empresa.objects.all().order_by("pk").iterator():
        try:
            with transaction.atomic():
                ini, fim = janela_fechamento(11, 2026, emp)
                ids = list(
                    CelulaDia.objects.filter(
                        colaborador__empresa_id=emp.pk,
                        data__gte=ini,
                        data__lte=fim,
                        data__lt=hoje,
                        veredito_em__isnull=False,
                    )
                    .order_by()
                    .values_list("colaborador_id", flat=True)
                    .distinct()
                )
        except Exception:
            erros += 1
            print("ERRO emp_pk=%s" % emp.pk)
            continue

        for col_pk in ids:
            try:
                # O savepoint permite continuar apos erro de banco,
                # sem invalidar a transacao externa de leitura.
                with transaction.atomic():
                    cels = list(
                        CelulaDia.objects.filter(
                            colaborador_id=col_pk,
                            data__lt=hoje,
                        )
                        .select_related("colaborador")
                        .order_by("data", "pk")
                    )
                    if not cels:
                        raise ValueError("sem celulas")

                    colab = cels[0].colaborador
                    locais, exemplos = medir_colaborador(
                        emp, colab, cels, ini, fim, agora
                    )

                cont.update(locais)
                for exemplo in exemplos:
                    if linhas_emitidas >= 20:
                        break
                    print(
                        "SEM_PAPEL emp_pk=%s cel_pk=%s col_pk=%s "
                        "data=%s hora=%s batida_pk=%s" % exemplo
                    )
                    linhas_emitidas += 1
            except Exception:
                erros += 1
                print("ERRO emp_pk=%s col_pk=%s" % (emp.pk, col_pk))


try:
    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION READ ONLY")
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ")
            cursor.execute("SELECT current_database(), current_schema()")
            banco, schema = cursor.fetchone()

        if banco != "sombra" or schema != "juliani":
            raise RuntimeError("banco ou schema incorreto")
        if not getattr(settings, "GRADE_POR_TURNO", False):
            raise RuntimeError("grade por turno desativada")

        sonda()
except Exception:
    erros += 1
    print("ERRO execucao")
finally:
    for chave in (
        "celulas_medidas",
        "celulas_ata_indeterminada",
        "batidas_no_universo",
        "em_par",
        "em_sobra_da_ata",
        "sem_papel",
    ):
        print("%s=%s" % (chave, cont[chave]))
    print("ERROS=%s" % erros)
    print("FIM_ETAPA_A")
```

O que a sonda não cobre:

- Ela compara **o pareamento calculado agora** com a **ata atualmente gravada**. Não prova qual foi o pareamento no instante da lavra: batidas, atas, escalas ou a janela do julgamento podem ter mudado desde então. O código também admite julgamento de célula isolada e uma segunda passada com subconjunto de células; a sonda adota a janela do lote atual. [app/ponto/services/cartorio.py:1049] [app/ponto/services/cartorio.py:2059] [app/ponto/management/commands/processar_cartorio.py:59]
- Uma ata sem lista `orfas` válida é contada como indeterminada e excluída da medição. A comparação por `HH:MM` não identifica individualmente duas batidas no mesmo minuto. [app/ponto/services/cartorio.py:764]
- O resultado `sem_papel` significa ausência dos **dois papéis medidos**. Ele não verifica se a batida aparece em uma lâmpada da ata ou participa de outro consumidor; tampouco demonstra, sozinho, ausência de todo papel no sistema. A ata também contém `lampadas`. [app/ponto/services/cartorio.py:754] [app/ponto/services/cartorio.py:757]
- Células futuras, sem `veredito_em`, sem célula no dia civil da batida, e colaboradores ou empresas com erro ficam fora do resultado; `ERROS` e `celulas_ata_indeterminada` precisam ser considerados ao interpretá-lo. [não verificado]

perguntas de lei: 0

## 19/11 10:26 · sessao b77dcde34fb5 · DST-L2-JZ1-C6 · verificador · origem=Code

# BLOQUEIA

Há dois bloqueios identificados no código: a nova retenção pode decidir com uma consulta sem trava e a rejeição consultar novamente depois, com resultado diferente; e a trilha continua afirmando que o chamado **aponta** para a justificativa mesmo quando a retenção a encontrou apenas por `chamado_origem`. O selo AST também não impede todas as formas de duplicar o predicado. ([reconciliador.py:458](app/chamados/reconciliador.py:458), [reconciliador.py:544](app/chamados/reconciliador.py:544), [reconciliador.py:581](app/chamados/reconciliador.py:581), [reconciliador.py:491](app/chamados/reconciliador.py:491), [teste C6:94](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:94))

O hash informado, os resultados dos logs e a comparação de produção não puderam ser verificados independentemente: a cópia não contém metadados Git acessíveis e os logs não foram fornecidos como arquivos. O dado raspado **relata** 129 testes verdes, três falhas no mutante e 46 pares iguais na foto D4; a prova exigida no aval menciona 37 pares, de modo que a correspondência entre os universos de 37 e 46 pares fica **[não verificado]**. A documentação do teste ainda diz 37 pares. ([teste C6:4](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:4))

## 1. Escritores e leitores

O censo abaixo cobre os acessos encontrados em código de aplicação; completude para acessos dinâmicos, SQL indireto e todos os templates é **[não verificado]**.

- **`Justificativa.status` — escritores:** default `pendente` no modelo; criação pendente pela API, formulário e cron; aprovação e rejeição nas portas individuais, materialização e lotes; rejeição automática no D3, entrada sincronizada e triagem; regularização pode restaurar `pendente`. Os dois escritores automáticos externos ao D3 são preexistentes. ([models.py:159](app/ponto/models.py:159), [api/views.py:2011](app/api/views.py:2011), [ponto/views.py:1638](app/ponto/views.py:1638), [processar_alertas_turno.py:394](app/ponto/management/commands/processar_alertas_turno.py:394), [justificativa.py:205](app/ponto/services/justificativa.py:205), [justificativa.py:351](app/ponto/services/justificativa.py:351), [justificativa.py:403](app/ponto/services/justificativa.py:403), [justificativa.py:503](app/ponto/services/justificativa.py:503), [ponto/views.py:1799](app/ponto/views.py:1799), [ponto/views.py:1878](app/ponto/views.py:1878), [entrada_sincronizada.py:38](app/ponto/services/entrada_sincronizada.py:38), [triagem_batida.py:605](app/ponto/services/triagem_batida.py:605), [regularizacao.py:91](app/ponto/services/regularizacao.py:91))

- **`Justificativa.status` — leitores encontrados:** predicado e portas de decisão; API e fila; contadores de ponto, chamados e core; cálculo de cobertura, fechamento e escala; cron de SLA; templates de atendimento, fila e painel. ([justificativa.py:33](app/ponto/services/justificativa.py:33), [justificativa.py:142](app/ponto/services/justificativa.py:142), [justificativa.py:283](app/ponto/services/justificativa.py:283), [api/views.py:2117](app/api/views.py:2117), [ponto/views.py:1713](app/ponto/views.py:1713), [ponto/views.py:1739](app/ponto/views.py:1739), [chamados/views.py:599](app/chamados/views.py:599), [core/views.py:205](app/core/views.py:205), [efeito_ausencia.py:41](app/ponto/services/efeito_ausencia.py:41), [fechamento.py:661](app/ponto/services/fechamento.py:661), [escala/utils.py:1208](app/escala/utils.py:1208), [alertar_justificativas_sla.py:19](app/ponto/management/commands/alertar_justificativas_sla.py:19), [meu_atendimento.html:89](app/templates/chamados/meu_atendimento.html:89), [fila_justificativas.html:112](app/templates/ponto/fila_justificativas.html:112), [painel_justificativa.html:47](app/templates/ponto/partials/painel_justificativa.html:47))

- **`Justificativa.chamado_origem` — escritores:** API e formulário, tanto na criação quanto no preenchimento de origem vazia; cron na criação; exclusão do chamado pode limpar a FK por `SET_NULL`. ([api/views.py:2007](app/api/views.py:2007), [api/views.py:2011](app/api/views.py:2011), [ponto/views.py:1634](app/ponto/views.py:1634), [ponto/views.py:1638](app/ponto/views.py:1638), [processar_alertas_turno.py:394](app/ponto/management/commands/processar_alertas_turno.py:394), [models.py:173](app/ponto/models.py:173))

- **`Justificativa.chamado_origem` — leitores encontrados:** encerramento e pré-trava da aprovação, predicado e mapeamento de donos, contador de pares, utilitário de chamado, painel e template. ([justificativa.py:33](app/ponto/services/justificativa.py:33), [justificativa.py:110](app/ponto/services/justificativa.py:110), [justificativa.py:279](app/ponto/services/justificativa.py:279), [justificativa.py:288](app/ponto/services/justificativa.py:288), [justificativa.py:376](app/ponto/services/justificativa.py:376), [chamados/utils.py:7](app/chamados/utils.py:7), [ponto/views.py:1686](app/ponto/views.py:1686), [painel_justificativa.html:128](app/templates/ponto/partials/painel_justificativa.html:128))

- **`ChamadoColaborador.justificativa_relacionada` — escritor encontrado:** porta `apontar_justificativa`; a FK também tem comportamento de exclusão definido no modelo. Chamadores incluem formulário, API e cron. ([vinculos.py:81](app/chamados/services/vinculos.py:81), [chamados/models.py:75](app/chamados/models.py:75), [ponto/views.py:1648](app/ponto/views.py:1648), [api/views.py:2019](app/api/views.py:2019), [processar_alertas_turno.py:409](app/ponto/management/commands/processar_alertas_turno.py:409))

- **Ponteiro — leitores encontrados:** guarda da própria porta, gates de chamado, predicado, escolha do detalhe, contador de pares e os dois escritores automáticos externos. ([vinculos.py:84](app/chamados/services/vinculos.py:84), [chamados/models.py:142](app/chamados/models.py:142), [chamados/models.py:1325](app/chamados/models.py:1325), [justificativa.py:280](app/ponto/services/justificativa.py:280), [justificativa.py:373](app/ponto/services/justificativa.py:373), [reconciliador.py:460](app/chamados/reconciliador.py:460), [entrada_sincronizada.py:38](app/ponto/services/entrada_sincronizada.py:38), [triagem_batida.py:605](app/ponto/services/triagem_batida.py:605))

- **`_APROVANDO`:** criado com default `None`; `no_ato_de_aprovar` faz `set` e `reset`; os blocos de aprovação individual, lote e materialização entram nesse contexto. Leem-no a retenção, o predicado e o registro da trilha. ([reconciliador.py:426](app/chamados/reconciliador.py:426), [reconciliador.py:430](app/chamados/reconciliador.py:430), [justificativa.py:140](app/ponto/services/justificativa.py:140), [ponto/views.py:1795](app/ponto/views.py:1795), [justificativa.py:462](app/ponto/services/justificativa.py:462), [reconciliador.py:453](app/chamados/reconciliador.py:453), [justificativa.py:286](app/ponto/services/justificativa.py:286), [reconciliador.py:491](app/chamados/reconciliador.py:491))

- **Funções do diff:** `pendentes_do_vinculo` é lida pela retenção e pela rejeição; a retenção é chamada pelo fechamento protegido, pela retratação, pelo portão da rejeição e pelo comando de reconciliação; a rejeição é chamada pela retratação e pelos dois lotes do cartório. ([justificativa.py:262](app/ponto/services/justificativa.py:262), [reconciliador.py:458](app/chamados/reconciliador.py:458), [justificativa.py:337](app/ponto/services/justificativa.py:337), [reconciliador.py:509](app/chamados/reconciliador.py:509), [reconciliador.py:544](app/chamados/reconciliador.py:544), [justificativa.py:346](app/ponto/services/justificativa.py:346), [reconciliar_chamados.py:79](app/chamados/management/commands/reconciliar_chamados.py:79), [reconciliador.py:581](app/chamados/reconciliador.py:581), [cartorio.py:1038](app/ponto/services/cartorio.py:1038), [cartorio.py:1585](app/ponto/services/cartorio.py:1585))

## 2. Equivalência do predicado

**Sim, para a mesma lista e a mesma fotografia dos dados**, a seleção extraída corresponde ao trecho anterior fornecido no diff: origem nos chamados **ou** PK apontada, somente `pendente`, excluindo `_APROVANDO`, sem duplicatas e ordenada por PK. Ambos removem `None` e retornam vazio sem consulta para lote vazio. A diferença de interface é que a função nova retorna um dicionário de justificativas para seus chamados; a rejeição antiga construía essa associação depois da consulta. ([justificativa.py:273](app/ponto/services/justificativa.py:273), [justificativa.py:279](app/ponto/services/justificativa.py:279), [justificativa.py:288](app/ponto/services/justificativa.py:288), [justificativa.py:327](app/ponto/services/justificativa.py:327))

A rejeição usa `travar=True`; a retenção usa o padrão `False`. Portanto, igualdade lógica de predicado não garante igualdade de resultado em momentos distintos. ([justificativa.py:284](app/ponto/services/justificativa.py:284), [justificativa.py:337](app/ponto/services/justificativa.py:337), [reconciliador.py:458](app/chamados/reconciliador.py:458))

## 3. Alcance da retenção

Fora do ato, a nova cláusula `justificativa_alheia` não é executada; a retenção por irmã órfã continua podendo atuar. Dentro do ato, a justificativa cuja PK está em `_APROVANDO` é excluída da consulta e, sob esse contexto corretamente estabelecido, não aparece como alheia. ([reconciliador.py:449](app/chamados/reconciliador.py:449), [reconciliador.py:453](app/chamados/reconciliador.py:453), [justificativa.py:286](app/ponto/services/justificativa.py:286))

## 4. Trava e leituras

Na rejeição, a mudança de `select_for_update().filter(...)` para `filter(...).select_for_update()` mantém a consulta preguiçosa: a avaliação continua ocorrendo na ordenação por PK antes do portão e das gravações. Não identifiquei mudança de ordem ou momento da trava **nesse trecho**; comportamento efetivo do banco em produção é **[não verificado]**. ([justificativa.py:283](app/ponto/services/justificativa.py:283), [justificativa.py:288](app/ponto/services/justificativa.py:288), [justificativa.py:337](app/ponto/services/justificativa.py:337), [justificativa.py:343](app/ponto/services/justificativa.py:343))

A retenção nova lê status e origem, que outros caminhos escrevem; o próprio ato de aprovação grava o status, e outros caminhos podem criar ou alterar a origem. Na retratação, a primeira retenção ocorre antes da transação e da trava explícita daquele caminho. ([reconciliador.py:544](app/chamados/reconciliador.py:544), [reconciliador.py:549](app/chamados/reconciliador.py:549), [reconciliador.py:558](app/chamados/reconciliador.py:558), [justificativa.py:205](app/ponto/services/justificativa.py:205), [api/views.py:2007](app/api/views.py:2007))

## 5. Segunda consulta e consequência

**Sim.** Na retratação durante uma aprovação, a retenção consulta sem trava; se não retiver, o código transiciona o chamado e a rejeição consulta novamente com trava, mesmo com `portao_de_retencao=False`. Se uma justificativa alheia pendente passar a integrar o vínculo entre as duas consultas, a primeira pode não vê-la e a segunda pode selecioná-la e rejeitá-la sem repetir a retenção. Isso é uma sequência possível pelo código; sua ocorrência, isolamento e frequência em produção são **[não verificado]**. ([reconciliador.py:544](app/chamados/reconciliador.py:544), [reconciliador.py:573](app/chamados/reconciliador.py:573), [reconciliador.py:581](app/chamados/reconciliador.py:581), [justificativa.py:337](app/ponto/services/justificativa.py:337), [justificativa.py:343](app/ponto/services/justificativa.py:343), [justificativa.py:351](app/ponto/services/justificativa.py:351))

No cartório, a ordem é inversa: consulta travada do lote e, para cada chamado candidato, outra consulta sem trava pela retenção. A seleção inicial não inclui linhas que passem a satisfazer o predicado depois dela; o efeito exato de concorrência depende das travas e do isolamento efetivos **[não verificado]**. ([justificativa.py:337](app/ponto/services/justificativa.py:337), [justificativa.py:345](app/ponto/services/justificativa.py:345), [reconciliador.py:458](app/chamados/reconciliador.py:458))

## 6. Classes de reprovação

- **Segundo juiz que duplica juiz registrado: ausente no delta examinado.** A função extraída é usada pelas duas portas; não encontrei registro de novo juiz para esse predicado na busca do registro vivo. Isso não prova ausência de toda derivação fora do censo **[não verificado]**. ([justificativa.py:262](app/ponto/services/justificativa.py:262), [justificativa.py:337](app/ponto/services/justificativa.py:337), [reconciliador.py:458](app/chamados/reconciliador.py:458), [core/juizes.py:316](app/core/juizes.py:316))

- **Segundo escritor fora da porta: presente no código preexistente.** Entrada sincronizada e triagem rejeitam diretamente a justificativa apontada; o lote de rejeição também escreve diretamente. A existência desses sítios não demonstra, por si só, que o diff os introduziu. ([entrada_sincronizada.py:38](app/ponto/services/entrada_sincronizada.py:38), [triagem_batida.py:605](app/ponto/services/triagem_batida.py:605), [ponto/views.py:1875](app/ponto/views.py:1875))

- **Relógio lido fora da borda: ausente nos trechos novos de produção examinados.** Comportamento de todos os caminhos transitivos **[não verificado]**. ([justificativa.py:262](app/ponto/services/justificativa.py:262), [reconciliador.py:453](app/chamados/reconciliador.py:453))

- **Derivação paralela de fonte única: ausente entre os dois leitores alterados**, que chamam a mesma função. Há, contudo, outra consulta de pares para contador com condições adicionais e finalidade própria; classificá-la como duplicação proibida exige a lei aplicável a esse contador **[não verificado]**. ([reconciliador.py:458](app/chamados/reconciliador.py:458), [justificativa.py:337](app/ponto/services/justificativa.py:337), [justificativa.py:373](app/ponto/services/justificativa.py:373))

- **Selo sem mordida completa: presente.** O teste principal exige uma chamada com o nome da função e proíbe algumas keywords e a forma direta `ch.justificativa_relacionada.status`. Uma chamada sem usar seu resultado, ou uma variante que leia o ponteiro por variável intermediária e derive o status fora da função, pode escapar dessas verificações. O teste do predicado antigo apenas confirma que encontra uma forma AST num texto artificial; ele não executa o selo principal contra todas as variantes. O RED funcional cobre o cenário J1/J3, mas a execução relatada no anexo é **[não verificado]**. ([teste C6:94](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:94), [teste C6:108](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:108), [teste C6:31](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:31))

- **Ordem de trava: sem mudança identificada na extração da rejeição; janela presente na retenção anterior à trava**, descrita nos itens 4 e 5. ([justificativa.py:283](app/ponto/services/justificativa.py:283), [reconciliador.py:544](app/chamados/reconciliador.py:544))

- **Pouso antes do pré-requisito: [não verificado].** O aval fornecido exige o pouso do COMBOIO-1911-F antes da construção; o dado fornecido não comprova a ordem real dos pousos. [nao verificado]

- **ESMERIL-SEMÂNTICA: presente.** `justificativa_pendente` agora pode identificar justificativa ligada só pela origem, enquanto a descrição da mesma trilha afirma que o chamado “aponta” para ela. A chave conserva o sentido de justificativa pendente; a frase atribui um vínculo de ponteiro que pode não existir. ([reconciliador.py:458](app/chamados/reconciliador.py:458), [reconciliador.py:491](app/chamados/reconciliador.py:491))

## 7. Teste do ponteiro [nao verificado]

**Sim, o teste é coerente para o caso que constrói:** J3 tem menor PK, mas J2 é o ponteiro pendente e alheio; o resultado esperado é J2. O código escolhe o ponteiro se ele estiver entre as alheias; caso contrário, escolhe a primeira da consulta ordenada por PK. ([teste C6:70](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:70), [reconciliador.py:458](app/chamados/reconciliador.py:458), [justificativa.py:288](app/ponto/services/justificativa.py:288))

Para esse caso específico, o detalhe também coincide com a trilha anterior, pois o predicado antigo olhava o ponteiro pendente alheio. A afirmação do teste de que “a trilha de antes não muda” não vale para o novo caso somente por origem: antes não havia essa retenção; agora há e a descrição pode afirmar incorretamente que há ponteiro. ([teste C6:71](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:71), [teste C6:108](app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:108), [reconciliador.py:491](app/chamados/reconciliador.py:491))

## 19/11 10:23 · sessao 46fa5858d7a2 · E4C-F5-CONSTROI · construtor · origem=Code

O contador está implementado em [vigia_texto_resposta.py](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/services/vigia_texto_resposta.py:7), com integração ao carimbo existente em [vigiar_fio_morto.py](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/management/commands/vigiar_fio_morto.py:194). O diff solicitado, incluindo a volta 1c, está em [E4C-F5-diff-final.patch](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/E4C-F5-diff-final.patch).

A entrega tem duas ressalvas:

- O leitor atual do ESTADO mostra o contador fora do esperado na seção do fio morto, mas a **PERGUNTA no topo** depende do patch externo abaixo ([fio_estado.py:191](/home/ronald/wt-codex-e4c-f5-constroi-090617/bin/fio_estado.py:191), [gerar_estado.py:261](/home/ronald/wt-codex-e4c-f5-constroi-090617/bin/gerar_estado.py:261)).
- Conforme a instrução de mudar somente os comentários, não alterei os leitores de texto. O código atual passa `leitura` para `respondida_sem_texto` e usa `leitura.resposta_colab` no histórico, apesar do comentário solicitado dizer projeção ([fio.py:190](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/services/fio.py:190), [historico.py:110](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/services/historico.py:110)).

O teste cobre igualdade, divergência após `update`, universo por pergunta com RESPOSTA e custo de uma versus várias perguntas ([teste:19](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_contador_texto.py:19), [teste:40](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_contador_texto.py:40)). A sintaxe dos cinco arquivos alterados foi analisada e `git diff --check` passou; banco e suíte não foram executados. Resultado dos testes e mutantes: **[não verificado]**. [nao verificado]

Comandos para execução posterior em banco de teste isolado, a partir de `app/`: [nao verificado]

```bash
# GREEN
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_contador_texto --settings=config.settings.ci --noinput

# RED / mutante: compara a leitura consigo mesma; test_divergencia_e_universo deve falhar
sed -i 's/!= p\.resposta_colab/!= DecisaoPergunta.leitura(p).resposta_colab/' chamados/services/vigia_texto_resposta.py
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_divergencia_e_universo --settings=config.settings.ci --noinput
sed -i 's/!= DecisaoPergunta\.leitura(p)\.resposta_colab/!= p.resposta_colab/' chamados/services/vigia_texto_resposta.py

# Mutante N+1: test_custo_uma_e_varias_perguntas deve falhar
sed -i "s/\.prefetch_related('decisoes')//" chamados/services/vigia_texto_resposta.py
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_custo_uma_e_varias_perguntas --settings=config.settings.ci --noinput
sed -i "s/perguntas.filter(Exists(respostas))$/perguntas.filter(Exists(respostas)).prefetch_related('decisoes')/" chamados/services/vigia_texto_resposta.py
```

Patch proposto para `bin/gerar_estado.py`, **não aplicado** por estar fora de `app/`. Ele usa a comparação já feita pelo leitor do carimbo, sem criar outro limiar ([fio_estado.py:145](/home/ronald/wt-codex-e4c-f5-constroi-090617/bin/fio_estado.py:145)):

```diff
@@
-    fio_txt = _linha_fio(_ler_fio(raiz))
+    _fio = _ler_fio(raiz)
+    fio_txt = _linha_fio(_fio)
+    _texto_fora = _fio.get('fora_do_esperado', {}).get('divergencia_texto_resposta')
+    pergunta_texto = (
+        '**PERGUNTA E4C-F5-CONTADOR-TEXTO:** divergencia ato x projecao: %s '
+        '(esperado %s; fonte: carimbo do vigia).\n\n'
+        % (_texto_fora['medido'], _texto_fora['esperado'])
+        if _texto_fora else ''
+    )
@@
 **Medido em %s.**
 
+%s\
 | | |
@@
-""" % (agora, head or '(sem head)', origem or '(sem origin/main)', ahead or '?',
+""" % (agora, pergunta_texto, head or '(sem head)', origem or '(sem origin/main)', ahead or '?',
```

Esse patch externo também está **[não verificado]**.

**LEI-AKITA:** origem=[PerguntaDisputa.resposta_colab e universo com ato RESPOSTA](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/services/vigia_texto_resposta.py:11); testemunha=DecisaoPergunta.leitura ([models.py:4295](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/models.py:4295)); RED=projeção alterada, divergência 1 ([teste:24](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_contador_texto.py:24)); quem-mais-le=vigiar_fio_morto e ESTADO pelo carimbo ([vigia:268](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/management/commands/vigiar_fio_morto.py:268), [leitor:144](/home/ronald/wt-codex-e4c-f5-constroi-090617/bin/fio_estado.py:144)); juizes novos=0; padrao=lampada ([GLOSSARIO:13](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/docs/GLOSSARIO.md:13)). [nao verificado]

## 19/11 10:19 · sessao 214707845bc3 · CADEIA-20-PASSO-1 · censo · origem=Code

# brief.md — CADEIA-20-PASSO-1, etapa A

**Estado: incompleto para aprovação do censo.** A ata guarda cada sobra como hora, sem PK nem data completa da batida; o pareador também pode consumir uma batida de intervalo sem devolvê-la dentro de um turno. A ligação de toda sobra a uma única batida viva, portanto, ainda precisa ser medida ([app/ponto/services/cartorio.py:749](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:749); [app/ponto/turnos.py:1371](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1371)).

Os anexos foram fornecidos no texto do pedido sem numeração de linhas. As referências a seus resultados abaixo ficam marcadas **[não verificado]**; não houve acesso ao banco ou aos logs. O arquivo `brief.md` não foi criado, conforme a regra de somente leitura.

## 1. Matriz: estado, autoridade, operação, persistência, retorno e prova

A persistência pretendida é comum às sete linhas: o cartório monta a ata e a entrega a `cel.lavrar_veredito(..., ata=_ata)`; `CelulaDia.ata` é o JSON da lavra. As seis razões e a marca, com esses nomes, são o resultado **proposto no aval fornecido**, não um retorno demonstrado do código atual ([app/ponto/services/cartorio.py:1898](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:1898); [app/escala/models.py:1772](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1772); aval fornecido: **[não verificado]**).

| Estado e retorno pretendido | Autoridade existente e operação | Entrega por batida? | Prova existente |
|---|---|---|---|
| Dia sem trabalho → `TRABALHO_EM_DIA_SEM_TRABALHO` | `dia_sem_trabalho(cel)` lê `cel.trabalha`, `ata.tipo_dia` e a exceção de cobertura parcial; retorna booleano. Não é uma leitura exclusiva de `tipo_dia`. | **Por dia.** Aplicar a resposta a cada sobra exige identificá-la. | [app/chamados/juizes.py:445](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/chamados/juizes.py:445); [app/escala/tests/test_orfa_classe.py:37](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_orfa_classe.py:37). |
| Rajada → `BATIDA_REPETIDA` | `classificar_orfas` agrupa horas consecutivas a até 15 minutos e devolve mapa hora → classe, quando há `missing`; o leitor de produção de cluster devolve uma assinatura **do dia**, contendo a lista inteira de órfãs e a contagem de lâmpadas apagadas. | **Por hora, com limite:** o classificador antigo identifica horas do grupo, mas não PKs e pode colidir em horas iguais. O leitor de produção responde só pelo dia. A equivalência integral à razão individual do aval está **[não verificada]**. | [app/escala/orfa_classe.py:26](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/orfa_classe.py:26); [app/ponto/services/cluster_espurio.py:15](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cluster_espurio.py:15); [app/escala/tests/test_orfa_classe.py:47](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_orfa_classe.py:47). |
| Pausa → `INTERVALO_FORA_DA_ESCALA` | `parear_turnos` lê ou carimba `is_intervalo`/`_intra_dur` na instância da batida. Os carimbos podem ter mais de uma origem. | **Na instância de batida processada**, inclusive quando ela não entra em turno; isso não fornece a ligação da hora da ata à instância. A origem da pausa não é separada pelo carimbo comum. | [app/ponto/turnos.py:1310](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1310); [app/ponto/turnos.py:1371](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1371); [app/ponto/tests/test_parear_intra_duracao.py:23](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_parear_intra_duracao.py:23). |
| Par fechado → `PERIODO_ADICIONAL` | `parear_turnos` retorna `Turno` com `entrada`, `saida` e `batidas`; a leitura das pontas permite identificar um par fechado. | **Por batida associada ao turno**, após a junção; não retorna a razão pronta para a hora da ata. | [app/ponto/turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43); [app/ponto/tests/test_turnos_borda_direita.py:36](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos_borda_direita.py:36). |
| Turno aberto → `TURNO_ABERTO` | `parear_turnos` retorna turno com entrada e sem saída, com campo `aberto`. | **Por batida associada ao turno**, após a junção; não retorna a razão pronta para a hora da ata. | [app/ponto/turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43); [app/ponto/turnos.py:1497](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1497); [app/ponto/tests/test_turnos.py:19](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos.py:19). |
| Saída sem entrada → `SAIDA_SEM_ENTRADA` | `parear_turnos` retorna turno sem entrada e com saída. | **Por batida associada ao turno**, após a junção; não retorna a razão pronta para a hora da ata. | [app/ponto/turnos.py:1495](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1495); [app/ponto/tests/test_turnos.py:37](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos.py:37). |
| Dia publicado em cadastro × realidade ou na lista → marca `ESCALA_NAO_DESCREVE` | Ler `ResultadoMes.dias_cadastro_x_realidade` ou as assinaturas **datadas** já presentes na lista; `dias_na_lista` devolve chaves `(colab_id, data)`. | **Por dia**, não por batida. A presença da pessoa numa assinatura sem data não identifica o dia. | [app/ponto/motor_calculo_v2.py:289](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:289); [app/ponto/services/cxr_na_lista.py:79](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cxr_na_lista.py:79); [app/escala/services/cadastro_realidade.py:1135](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:1135). |

Os testes citados exercitam as autoridades atuais; não são testes das seis razões propostas nem provam a junção completa. **[não verificado]** [nao verificado]

**Ligação da sobra.** A grade formata a sobra como `HH:MM`, e a ata grava essa string ([app/escala/utils.py:673](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:673); [app/ponto/services/cartorio.py:764](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:764)). A hora, sozinha, **não identifica uma batida**: pode não haver candidata viva ou pode haver mais de uma no minuto. Comparar por minuto é uma operação de junção a medir, e a data civil da batida também pode diferir da data do turno ([app/ponto/turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43); [app/ponto/tests/test_turnos_borda_direita.py:36](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos_borda_direita.py:36)).

## 2. Casos nominais pela regra — L-110

A precedência fornecida é: dia sem trabalho; depois rajada; depois papel do pareador. O anexo informa a classificação **reconstruída** e o papel do pareador para cada hora, mas não fornece o cadastro completo do dia nem a lista completa de batidas. Assim, abaixo há **candidatos condicionais**, não casos fechados para RED. A identificação `colN`, data, hora e cada resultado atribuído ao anexo estão **[não verificados]**. A exigência de casos pela regra antes do código está em [app/docs/LEIS.md:136](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/LEIS.md:136).

| Razão pretendida | Dois candidatos do anexo; cadastro e batidas disponíveis |
|---|---|
| `TRABALHO_EM_DIA_SEM_TRABALHO` | `col950 15/11 11:25` e `col950 15/11 16:59`: reconstrução `folga_desmentida`; pareador `PAR_FECHADO`. Pela precedência, **se confirmado o dia sem trabalho pela ata**, ambos recebem esta razão. Demais batidas e `tipo_dia` literal: **[não verificado]**. |
| `BATIDA_REPETIDA` | `col836 06/11 08:25` e `col836 06/11 08:38`: reconstrução `eco_cluster`; pareador `PAR_FECHADO`. **Se confirmado dia de trabalho e pertencimento à rajada**, recebem esta razão antes do par fechado. Cadastro literal e demais batidas: **[não verificado]**. |
| `INTERVALO_FORA_DA_ESCALA` | `col203 21/10 12:35` e `col204 17/11 14:44`: reconstrução `geometria`; pareador `NAO_MEDIDA_ORIGEM_PAUSA`; anexo também informa `INTRA_ATA_SIM`. **Se confirmados dia de trabalho e ausência de precedência de rajada**, recebem esta razão. Cadastro literal e demais batidas: **[não verificado]**. |
| `PERIODO_ADICIONAL` | `col174 15/11 05:30` e `col174 15/11 06:33`: reconstrução `geometria`; pareador `PAR_FECHADO`. **Se confirmados dia de trabalho e ausência de rajada**, recebem esta razão. Cadastro literal e demais batidas: **[não verificado]**. |
| `TURNO_ABERTO` | `col174 11/11 18:59` e `col220 11/11 06:00`: reconstrução `geometria`; pareador `TURNO_ABERTO`. **Se confirmados dia de trabalho e ausência de rajada**, recebem esta razão. Cadastro literal e demais batidas: **[não verificado]**. |
| `SAIDA_SEM_ENTRADA` | `col174 12/11 07:01` e `col300 21/10 06:00`: reconstrução `geometria`; pareador `SAIDA_SEM_ENTRADA`. **Se confirmados dia de trabalho e ausência de rajada**, recebem esta razão. Cadastro literal e demais batidas: **[não verificado]**. |

A classificação antiga trata `folga_desmentida`, `geometria` e `eco_cluster` segundo as entradas da grade, inclusive `tipo_dia` e `missing`; o texto raspado não demonstra, por si só, todos esses campos para cada caso ([app/escala/orfa_classe.py:53](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/orfa_classe.py:53)).

## 3. Leitores de órfãs

**Censo parcial: 25 entradas listadas = 25 contadas; o total de 49 não foi comprovado.** A unidade desta lista é um local de leitura ou passagem identificado, não uma contagem demonstrada de todos os consumidores finais. A cópia registra um censo AST anterior e sua remoção, mas nesse trecho não apresenta a lista nominal dos 49 ([app/docs/RELATO.md:10239](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/docs/RELATO.md:10239)). A classificação dos locais abaixo é preliminar onde indicada por **[não verificado]**.

**MOSTRA ou mede — 13 entradas**

1. [app/escala/services/leitor_celula.py:187](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/leitor_celula.py:187) — adapta ata e contagem.
2. [app/escala/utils.py:1054](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:1054) — passagem da grade.
3. [app/ponto/services/espelho.py:702](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:702) — prepara órfãs para o espelho.
4. [app/templates/ponto/espelho.html:151](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/ponto/espelho.html:151) — apresentação.
5. [app/colaboradores/services/calendario.py:406](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/colaboradores/services/calendario.py:406) — marcação no calendário.
6. [app/relatorios/pdf_espelho.py:477](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/relatorios/pdf_espelho.py:477) — apresentação em PDF.
7. [app/relatorios/cartao_pela_celula.py:288](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/relatorios/cartao_pela_celula.py:288) — leitura para cartão.
8. [app/ponto/juiz_batida.py:161](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/juiz_batida.py:161) — exposição intermediária; destino completo **[não verificado]**.
9. [app/ponto/services/esmeril_espelho.py:130](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:130) — medição.
10. [app/escala/services/furos_diarios.py:22](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/furos_diarios.py:22) — contagem.
11. [app/ponto/services/vigia_pareador_celula.py:74](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/vigia_pareador_celula.py:74) — vigia.
12. [app/ponto/management/commands/tripwire_celulas.py:264](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/tripwire_celulas.py:264) — verificação.
13. [app/ponto/services/bordas_realizado.py:49](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/bordas_realizado.py:49) — contador.

**DECIDE — 12 entradas**

1. [app/chamados/juizes.py:514](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/chamados/juizes.py:514) — guarda de furo.
2. [app/ponto/supra_juiz.py:392](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/supra_juiz.py:392).
3. [app/ponto/status_dia.py:188](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/status_dia.py:188).
4. [app/ponto/services/simular_dia.py:12](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/simular_dia.py:12).
5. [app/ponto/propositor.py:39](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/propositor.py:39).
6. [app/ponto/turnos.py:2679](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2679).
7. [app/chamados/services/materializacao.py:449](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/chamados/services/materializacao.py:449).
8. [app/ponto/services/cluster_espurio.py:29](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cluster_espurio.py:29) — assinatura de cluster.
9. [app/ponto/management/commands/detectar_par_relampago.py:140](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/detectar_par_relampago.py:140).
10. [app/ponto/management/commands/propor_reorganizacao.py:104](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/propor_reorganizacao.py:104).
11. [app/ponto/services/cartorio.py:858](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:858).
12. [app/escala/utils.py:1979](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:1979).

**PAGA — 0 entradas comprovadas nesta lista parcial.** Isso não significa que não haja leitores com efeito em dinheiro: a própria descrição do campo `ata` informa consumidores de folha; o caminho completo de cada leitura de órfãs até pagamento ainda precisa ser censado ([app/escala/models.py:1774](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1774)). `folha/causas.py` lê a informação, mas o destino de pagamento não foi demonstrado nesta revisão ([app/folha/causas.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/folha/causas.py:43)).

## 4. Sonda das sobras sem batida viva

A medida anterior informa **36**, mas a causa e a reprodução da medida estão **[não verificadas]**. A sonda abaixo usa a data civil da célula como hipótese operacional: a ata só guarda `HH:MM`, enquanto turnos podem atravessar meia-noite. Portanto, o universo impresso pode diferir dos 36 e **não prova**, sozinho, a atribuição correta das sobras noturnas ([app/ponto/services/cartorio.py:764](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:764); [app/ponto/tests/test_turnos_borda_direita.py:36](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/tests/test_turnos_borda_direita.py:36)).

A lavra usa `veredito_em`; batidas têm `criado_em` e `retratada_em`; a autoridade de batidas apuráveis exclui retratadas ([app/escala/models.py:1772](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1772); [app/ponto/models.py:63](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/models.py:63); [app/ponto/models.py:79](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/models.py:79); [app/ponto/turnos.py:2872](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2872)). O script foi preparado para Django shell com PostgreSQL em transação própria; execução no envelope da sombra **[não verificada]**.

```python
from collections import Counter
from datetime import datetime, time, timedelta

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Empresa
from escala.models import CelulaDia
from ponto.janelas import janela_fechamento
from ponto.models import Batida
from ponto.turnos import batidas_apuraveis

cont = Counter()
erros = 0
tz = timezone.get_current_timezone()


def minuto(ts):
    return timezone.localtime(ts, tz).strftime("%H:%M")


def estado(valor):
    if valor is None:
        return "indeterminada"
    return "sim" if valor else "nao"


def sonda():
    global erros

    for emp in Empresa.objects.all().order_by("pk").iterator():
        try:
            ini, fim = janela_fechamento(11, 2026, emp)
            celulas = (
                CelulaDia.objects.filter(
                    colaborador__empresa_id=emp.pk,
                    data__gte=ini,
                    data__lte=fim,
                )
                .select_related("colaborador")
                .order_by("pk")
            )

            for cel in celulas.iterator():
                try:
                    horas = (cel.ata or {}).get("orfas") or []
                    if not horas:
                        continue

                    inicio = timezone.make_aware(
                        datetime.combine(cel.data, time.min), tz
                    )
                    final = timezone.make_aware(
                        datetime.combine(cel.data + timedelta(days=1), time.min),
                        tz,
                    )

                    todas = list(
                        Batida.objects.filter(
                            colaborador_id=cel.colaborador_id,
                            timestamp__gte=inicio,
                            timestamp__lt=final,
                        ).only("pk", "timestamp", "criado_em", "retratada_em")
                    )
                    vivas = list(
                        batidas_apuraveis(cel.colaborador)
                        .filter(timestamp__gte=inicio, timestamp__lt=final)
                        .only("pk", "timestamp")
                    )
                    minutos_vivos = {minuto(b.timestamp) for b in vivas}
                    eventos = [
                        evento
                        for b in todas
                        for evento in (b.criado_em, b.retratada_em)
                        if evento is not None
                    ]
                    lavra = cel.veredito_em
                    velha = (
                        None if lavra is None
                        else any(evento > lavra for evento in eventos)
                    )

                    for hora in horas:
                        if not isinstance(hora, str) or len(hora) != 5:
                            raise ValueError("hora invalida")
                        datetime.strptime(hora, "%H:%M")

                        if hora in minutos_vivos:
                            continue

                        retratadas = [
                            b for b in todas
                            if b.retratada_em is not None
                            and minuto(b.timestamp) == hora
                        ]
                        depois = (
                            None if lavra is None
                            else any(b.retratada_em > lavra for b 

[truncado em 20000 de 23403 caracteres]

_7322 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
