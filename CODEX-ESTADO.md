# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 03:50** · publicado em **09/11/2026 03:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 09/11 03:27 · sessao f1da0f00d814 · - · - · origem=Ronald (tmux)

- **Juiz novo/derivação:** o diff lê pares prontos de `intervalos_do_turno` e `pausas_abaixo_do_piso`; não reconstrói duração nem pareamento a partir de tipos. Acrescenta filtro de representabilidade no alinhamento: nenhuma ponta de outro par estritamente entre as pontas. A autoridade registrada continua `intervalos_do_turno` para “o vão foi intervalo?”. `app/escala/utils.py:453`, `app/escala/utils.py:466`, `app/core/juizes.py:202`.
- **Escritor:** o trecho produtivo alterado só devolve lista em memória. O teste novo grava `CelulaDia.ata` diretamente, como preparação da fixture, em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:226` e `:232`. A porta registrada de produção é `ponto/portas/celula.py`; `lavrar_veredito` atribui a ata e persiste pela operação. `app/core/portas.py:112`, `app/ponto/portas/celula.py:969`, `app/ponto/portas/celula.py:1027`.
- **Relógio no trecho produtivo alterado:** não lê “agora”; trabalha sobre timestamps recebidos e sobre o turno fechado. `app/escala/utils.py:444`, `:449`, `:453`. Isso não prova ausência de relógio nos caminhos das fixtures: criar batidas dispara sinais de núcleo e cartório. `app/ponto/signals.py:34`, `:131`.

Censo dos sítios de adjacência e classificação:

| Sítio | Classificação |
|---|---|
| `_papeis_por_alternancia` | **Mesma pergunta de alinhamento; outro contrato de entrada.** Recebe pontas dos períodos já alternadas `E,S,E,S`, exige ≥4 e envelope ≤26h, depois chama cada vão interno `S,E` de par. É o caminho sem `turno`, não alterado pelo diff. `app/escala/utils.py:477`, `:484`, `:496`, `:596`. |
| Zona 5 do motor | Alimenta esse caminho apenas com entradas/saídas dos períodos e sem `turno=`. Há adjacência das pontas nessa representação, não teste de vizinhança entre todas as batidas do juiz. Demonstrar o mesmo defeito por esse caminho fica **[nao verificado]**. `app/ponto/motor_calculo_v2.py:3060`, `:3062`. |
| `marco_ocupado_pela_batida` / `cumpriu_a_volta` | Também chama `_match_marcos` sem turno; par único fica no alinhamento usual porque a alternância exige quatro batidas. Mesma autoridade de alinhamento, contrato distinto. `app/escala/utils.py:424`, `:427`, `:485`. |
| `_alinhar_por_papel` | A consecutividade exigida é dos **marcos cadastrais** `S,E`, não das batidas reais. Outra condição; não reproduz o defeito. Reconstrói cada par pelo número e pelas letras S/E. `app/escala/utils.py:507`, `:517`, `:527`, `:561`. |
| `_pares_marcados` | Usa adjacência no ramo residual, mas é **produção da decisão do juiz**, não transcrição de decisão recebida. As pontas declaradas Xi são pareadas primeiro, independentemente de batidas internas; o restante usa vizinhas e marcas/tipos. Alterar isso responde outra pergunta e pode mudar minutos. `app/ponto/turnos.py:798`, `:811`, `:818`, `:875`. |
| `periodos_de_turnos` | **Transporte sem adjacência:** copia diretamente `intervalos_do_turno(t)`. `app/ponto/adaptador_motor.py:27`, `:44`. |
| `_intra_real` | **Leitor sem adjacência:** soma duração dos pares recebidos. `app/ponto/motor_calculo_v2.py:1909`. |
| `_periodos_pelo_marco` / `juiz_batida.periodos_do_dia` | Outro transporte para o motor: pausa vem das chaves declaradas no DNA/ata, não da vizinhança das batidas cruas. Recupera os instantes reais de cada ponta antes de transportar. `app/ponto/juiz_batida.py:280`, `app/ponto/motor_calculo_v2.py:713`. |
| `segmentos_do_motor` | **Leitor sem adjacência:** ordena intervalos recebidos e corta segmentos por eles. `app/ponto/calculador/alimentacao.py:129`. |
| Motor, vão entre períodos | Examina períodos consecutivos e consulta existência do par exato na lista do juiz. É outra representação, não cópia das batidas internas para papéis. `app/ponto/motor_calculo_v2.py:2978`, `:2983`, `:2987`. |
| `primeiro_furo_posicional` | Busca intervalo por duração entre marcas adjacentes para decidir furo posicional; **outra pergunta**, não transcrição do par recebido do juiz. `app/ponto/propositor.py:261`, `:282`, `:296`. |

Não encontrei, nos leitores diretos das duas funções de pares, outra restrição idêntica à removida de `_papeis_do_turno`; os transportes efetivos identificados estão acima. A afirmação universal de que nenhum outro caminho reproduz o defeito permanece **[nao verificado]**.

## 09/11 03:27 · sessao 12e7e733d8cb · - · - · origem=Ronald (tmux)

**Ordem e `k`.** O único chamador produtivo direto encontrado de `_papeis_do_turno` é `_match_marcos`; este calcula extremos por timestamp, chama o alinhador, emite órfãs na ordem recebida e marca consumo por índice. Não precisa que S/E de um par sejam tuplas vizinhas. (`app/escala/utils.py:596`, `:605`, `:613`, `:621`, `:622`).

O alinhador localiza entrada/saída pelo rótulo, reconstrói pares por número e posição S/E, ordena os números dos pares e ordena soltas pelo timestamp. Assim, `sorted(out,key=k)` não altera a associação nesse leitor; pode alterar a ordem textual das órfãs se `locs` não vier cronológico. (`app/escala/utils.py:524`, `:525`, `:526`, `:530`, `:564`, `:621`).

`k` pode repetir: `_idx.setdefault(timestamp,k)` aponta todos os objetos com timestamp exato igual ao primeiro índice; `_pos` aponta o mesmo timestamp ao último objeto. `k=None` impede o retorno de lista, pela guarda anterior ao `sorted`. (`app/escala/utils.py:446`, `:448`, `:450`, `:451`, `:456`, `:474`).

**Escritores/leitores e significados.**

- Papel transitório: `_papeis_do_turno` produz bordas, par e solta; `_papeis_por_alternancia` é outro produtor existente do mesmo formato, usado somente sem turno e com `derivar=True`. O alinhador lê ambos pelo contrato comum. (`app/escala/utils.py:453`, `:468`, `:472`, `:477`, `:495`, `:596`, `:597`, `:598`, `:613`).
- `'par'` inclui **intervalos e pausas abaixo do piso**; não significa automaticamente pausa descontável. O adaptador transporta ao motor somente `intervalos_do_turno`, cujo piso é `>=900` segundos. (`app/escala/utils.py:453`; `app/ponto/turnos.py:499`, `:545`, `:565`; `app/ponto/adaptador_motor.py:44`).
- Minuto da ata: builder pergunta `realizado_do_dia` com turnos do juiz e grava zero declarado quando `sem_turno`; `ata_do_dia` copia para inteiro; a persistência ocorre por `lavrar_veredito`, não em `_papeis_do_turno`. (`app/escala/utils.py:2045`, `:2058`; `app/ponto/services/cartorio.py:696`, `:706`, `:1802`; `app/ponto/portas/celula.py:969`, `:995`, `:1028`, `:1035`).
- Leitores diretos relevantes do minuto lavrado: `grade_da_celula` copia `ata['minutos_realizados']`; validação PDF soma por célula; `ata_sem_turno` recebe o número pela grade; vigia compara com o juiz. (`app/escala/services/leitor_celula.py:406`; `app/folha/validacao_pdf.py:44`, `:48`, `:52`; `app/ponto/services/dia_decidido.py:158`, `:163`, `:179`; `app/ponto/services/bordas_realizado.py:76`, `:87`).
- Transporte ao motor: `periodos_de_turnos` preserva timestamps de bordas e intervalos; `_periodos_via_autoridade` passa o trio ao caminho pelo marco; esse caminho também pode substituir intervalos pelos declarados pelo juiz da ata. Recupera os segundos do objeto real mediante índice por minuto e `setdefault`. (`app/ponto/adaptador_motor.py:37`, `:43`, `:44`; `app/ponto/motor_calculo_v2.py:530`, `:531`, `:593`, `:596`, `:600`, `:602`, `:609`, `:713`, `:721`).
- **Dia**: realizado filtra `t.data_turno==data`; mapa do motor lê `t.data_turno`; há fallback declarado para data civil quando o mapa não responde. Órfãs da saída podem ser arquivadas no dia seguinte quando o juiz diz `cross_meianoite`. Portanto dia do turno e dia civil coexistem com funções diferentes. (`app/ponto/turnos.py:719`; `app/ponto/motor_calculo_v2.py:884`, `:894`, `:685`, `:687`; `app/escala/utils.py:1963`, `:1971`, `:1972`, `:1973`).
- **Precisão**: papel aqui usa timestamp completo; realizado trunca **cada ponta** ao minuto antes de subtrair; motor soma segundos/60. A ata usa HH:MM e seu alimentador rejeita minutos com número de batidas diferente de um, enquanto `_instante_real` do motor usa o primeiro objeto no minuto. São contratos distintos, com perda de identidade no minuto já existente. (`app/escala/utils.py:450`, `:454`, `:456`; `app/ponto/turnos.py:605`, `:740`, `:758`, `:1604`, `:1613`, `:1656`; `app/ponto/motor_calculo_v2.py:600`, `:606`, `:609`, `:1911`).
- **Pontas**: seleção de pares declarados admite `z>=a`; piso inclui exatamente 900s; transcrição exige índices estritamente interiores; realizado inclui pausa com `z<=fim`; recorte do motor conserva apenas interseção de duração positiva. Não há soma de “+1 minuto” nas pontas. (`app/ponto/turnos.py:813`, `:545`, `:759`; `app/escala/utils.py:466`; `app/ponto/motor_calculo_v2.py:1886`, `:1887`, `:1911`).

**Outro caminho importante para a questão 6.** Zona 5 cria `locs` só com entradas/saídas de períodos e chama `_match_marcos` **sem** `turno`; passa por `_papeis_por_alternancia`, e não pela função alterada. Essa forma descreve períodos alternados, não transcrição de batidas extras do mesmo turno; classificá-la como a mesma falha só porque usa fatias adjacentes confundiria os operandos. (`app/ponto/motor_calculo_v2.py:3060`, `:3062`; `app/escala/utils.py:484`, `:489`, `:491`, `:496`, `:597`, `:598`).

## 09/11 03:24 · sessao e20ede192d9b · PAR-NAO-ADJACENTE · construtor · origem=Code

Cura aplicada, árvore modificada e sem commit. A única função de produção alterada é `_papeis_do_turno`; a comparação AST está registrada em [resultados:94](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:94). [nao verificado]

Há uma contradição no caso 1: **719 − 120 = 599** para pausa 20:00→22:00; **660** corresponde à pausa 21:01→22:00 do anexo. Como a pergunta ficou sem resposta, preservei os dois oráculos em testes separados; o teste do builder usa o anexo. [teste:88](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:88), [teste:203](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:203). [nao verificado]

O diff, por hunk:

| Arquivo:linha | Alteração |
|---|---|
| [utils.py:437](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:437) | Docstring registra a restrição para pares não adjacentes. |
| [utils.py:453](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:453) | Materializa os pares dos dois leitores e reúne **todas** as pontas, inclusive abaixo do piso. |
| [utils.py:466](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:466) | Troca a adjacência pela guarda abaixo; continua visitando o miolo e retorna as tuplas por `k`. |
| [testes:29](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:29) | Acrescenta imports para fixtures sem banco. | [nao verificado]
| [testes:49](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:49) | Acrescenta seis testes com pareador e extratores reais, sem mock do juiz. | [nao verificado]
| [testes:203](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:203) | Acrescenta fixture do builder e asserção de 660 minutos. | [nao verificado]
| [executor:1](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_sem_banco.py:1) | Novo executor limitado por AST e mutantes em memória. |
| [resultados:1](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:1) | Comandos, resultados e hashes dos fontes verificados. |

A guarda efetiva ficou assim em [utils.py:466](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:466):

```diff
- if _iz is not None and 0 < _iz < len(_bats) - 1 and _iz == _i + 1:
+ if (_iz is not None and 0 < _i < _iz < len(_bats) - 1
+         and not any(_bats[_j].timestamp in _pontas_par for _j in range(_i + 1, _iz))):
```

O caso 3 **é montável pelo pareador real**: o extrator entrega 12→13 e 12:10→12:30. HEAD e cura conservam exatamente:

```text
ent, sol, ('par', 0, 'S'), ('par', 0, 'E'), sol, sai
```

A caracterização passa nos dois; retirar a guarda faz esse teste falhar. Também há proteção para o par interno abaixo do piso. [teste:115](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:115), [resultados:14](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:14), [resultados:47](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:47). [nao verificado]

Quanto aos leitores e à semântica:

- O único chamador direto de produção das duas funções é `_match_marcos`. Ele usa os índices para células e consumo, sem exigir pontas adjacentes. [utils.py:596](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:596), [utils.py:613](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:613).
- O alinhador agrupa por número do par e ordena as soltas pelo instante. **Não depende da ordem das tuplas**; a reversão é testada. Contudo, a lista de órfãs segue essa ordem, por isso preservei `out` ordenado por `k`. [utils.py:530](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:530), [utils.py:564](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:564), [utils.py:621](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:621), [teste:139](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:139). [nao verificado]
- `par` continua sendo o par numerado S/E; a consecutividade exigida pelo alinhador pertence aos **marcos cadastrais**. `sol` continua sendo tratada depois, sem deslocar ocupantes. A exceção histórica do aninhado permanece caracterizada. [utils.py:507](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:507), [utils.py:564](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:564).
- Pela fonte comum, os demais caminhos são builder, single, diagnóstico, leitor de marco, Zona 5 e detector. Os caminhos sem `turno=` conservam sua seleção anterior. [utils.py:1701](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:1701), [utils.py:776](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:776), [utils.py:1650](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:1650), [utils.py:427](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:427), [motor:3062](/home/ronald/wt-codex-par-nao-adjacente-031500/app/ponto/motor_calculo_v2.py:3062), [detector:212](/home/ronald/wt-codex-par-nao-adjacente-031500/app/chamados/detector_anomalias.py:212).
- O censo estrutural referencia `_alinhar_por_papel`; preservei seu corpo e suas posições, assim como `_papeis_por_alternancia`. [censo:247](/home/ronald/wt-codex-par-nao-adjacente-031500/app/core/tests/test_medida_turno_do_par.py:247), [resultados:109](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:109).

A prova local limitada produziu **3 falhas no HEAD e 6 testes passando com a cura**. Os cinco mutantes falharam por asserção, respectivamente com 3, 2, 3, 3 e 1 falhas. [resultados:7](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:7), [resultados:18](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:18), [resultados:29](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:29). [nao verificado]

Comandos executados, a partir de `app/`, registrados nesse arquivo:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 escala/tests/par_nao_adjacente_sem_banco.py --head
PYTHONDONTWRITEBYTECODE=1 python3 escala/tests/par_nao_adjacente_sem_banco.py
PYTHONDONTWRITEBYTECODE=1 python3 escala/tests/par_nao_adjacente_sem_banco.py --mutante adjacencia
PYTHONDONTWRITEBYTECODE=1 python3 escala/tests/par_nao_adjacente_sem_banco.py --mutante aceita_aninhado
PYTHONDONTWRITEBYTECODE=1 python3 escala/tests/par_nao_adjacente_sem_banco.py --mutante pula_soltas
PYTHONDONTWRITEBYTECODE=1 python3 escala/tests/par_nao_adjacente_sem_banco.py --mutante sem_ordem
PYTHONDONTWRITEBYTECODE=1 python3 escala/tests/par_nao_adjacente_sem_banco.py --mutante ignora_pares_curtos
```

`py_compile` passou nos três fontes, interceptando somente a gravação para não criar bytecode; comando completo em [resultados:85](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:85). [nao verificado]

**Builder e testes Django existentes: [nao verificado].** Não executei a suíte, conforme a restrição de banco; comando pendente em [resultados:4](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/par_nao_adjacente_resultados.txt:4): [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_juiz_do_par_ata_casa_pelo_turno
```

## 09/11 03:14 · sessao 8538307186c7 · SELOS-CANON-I1 · verificador · origem=Code

1. **OBSERVACAO — P1..P7: a volta 2 não cura todos integralmente.**

| Item | Resultado da conferência |
|---|---|
| **P1** | **Parcial.** Wrapper passa pelo censo antes da vaga; Agent sem obra é negado. Porém `export` herdado é negado mesmo com censo válido, pois o hook exige obra escrita na invocação. `bin/codex_pergunta.sh:230`; `bin/disjuntor.py:1691`; `bin/disjuntor.py:1713`. |
| **P2** | **Descoberta literal curada:** existe glob `logs/<OBRA>/brief*.md`, unido aos citados. A costura com briefs citados em pasta diferente continua quebrada no wrapper, que entrega texto vazio ao juiz. `bin/disjuntor.py:1536`; `bin/disjuntor.py:1570`; `bin/disjuntor.py:2087`. |
| **P3** | **Curado só na forma.** Escreve pela porta existente, mas em ALARMES; o Stop procura a primeira linha útil do painel. A idempotência também não é atômica. `bin/disjuntor.py:258`; `bin/disjuntor.py:269`; `app/core/esteira_vigia.py:863`; `bin/hook_stop_fila1.py:407`. |
| **P4** | **Parcial.** BACKLOG ausente e exceções dentro de `_censo_nega` negam; JSON ilegível ainda termina com rc 0 e sem `deny`. `bin/disjuntor.py:1544`; `bin/disjuntor.py:1590`; `bin/disjuntor.py:1751`; `bin/disjuntor.py:1790`. |
| **P5** | **Contraexemplos originais curados:** examina todas as perguntas e ancora o corte. O positivo exige stdout vazio, mas não verifica rc 0 do processo. `bin/disjuntor.py:1454`; `bin/disjuntor.py:1512`; `bin/tests/test_censo_antes_da_vaga.sh:124`; `bin/tests/test_censo_antes_da_vaga.sh:139`. |
| **P6** | **Parcial.** Há união dos intervalos e adiamento do OK do Agent. O OK do wrapper continua anterior à vaga e é contado como abertura efetiva. `bin/disjuntor.py:1924`; `bin/disjuntor.py:1662`; `bin/codex_pergunta.sh:230`; `bin/disjuntor.py:1988`. |
| **P7** | **Reconhecedor compartilhado; semântica incompleta.** Stop, handoff e alarme usam a mesma função, mas selecionam lugares diferentes do RELATO. `bin/hook_stop_fila1.py:716`; `bin/handoff_sessao.sh:128`; `bin/alarme_sessao_ociosa.py:124`; `bin/alarme_sessao_ociosa.py:137`. |

2. **PROCEDENTE-candidato — classificação dos pontos abertos 1..9.**

| Ponto | Classificação e fundamento |
|---|---|
| **1** | **Defeito:** PAREI entra abaixo de ALARMES, preservando o painel anterior; não satisfaz o leitor do Stop/handoff. Ausência de hora é comprovada; permanência pela dieta é **[nao verificado]**. `app/core/esteira_vigia.py:829`; `app/core/esteira_vigia.py:867`; `bin/disjuntor.py:1457`; `bin/hook_stop_fila1.py:408`. |
| **2** | **Defeito:** existência é consultada antes do lock; a inserção travada não deduplica. Dois chamadores podem inserir duas vezes. Além disso, uma linha histórica em qualquer lugar impede republicação posterior. `bin/disjuntor.py:258`; `bin/disjuntor.py:261`; `app/core/esteira_vigia.py:848`; `app/core/esteira_vigia.py:867`. |
| **3** | **Defeito de costura:** o PreToolUse aceita brief citado em pasta diferente; o wrapper perde essa citação e exige o glob literal. Portanto uma abertura pode passar no hook e ser recusada pela CLI com o mesmo brief conforme. Que as pastas atuais de produção sejam slugs é **[nao verificado]**. `bin/tests/test_censo_antes_da_vaga.sh:166`; `bin/disjuntor.py:1536`; `bin/disjuntor.py:2087`. |
| **4** | **Defeito de autoridade:** a vaga é do repositório vivo, mas o julgamento usa BACKLOG/briefs da raiz do script. Cópia desatualizada sem a obra retorna FORA/rc 0. Divergência efetiva em produção: **[nao verificado]**. `bin/codex_pergunta.sh:55`; `bin/codex_pergunta.sh:76`; `bin/codex_pergunta.sh:230`; `bin/disjuntor.py:1544`; `bin/disjuntor.py:2083`. |
| **5** | **Defeito no contrato anunciado da chave:** “existindo, nenhum disjuntor age” não corresponde à CLI, que não consulta `desligado()`. A diferença está declarada, mas aceite específico desse desvio é **[nao verificado]**. `bin/disjuntor.py:35`; `bin/disjuntor.py:1756`; `bin/disjuntor.py:2069`. |
| **6** | **Endurecimento declarado, com regressões concretas:** negar construtor sem obra é intencional; negar `export` com censo válido e IDs anteriormente admitidos é incompatibilidade adicional. `bin/disjuntor.py:1713`; `bin/disjuntor.py:1691`; `bin/codex_pergunta.sh:188`; `bin/disjuntor.py:2075`. |
| **7** | **Defeito na cobertura alegada:** abertura fora do wrapper não é automaticamente contada; sem `ini` no chamadas.log ela desaparece da medida. A rota direta também é proibida pela conduta existente. `bin/disjuntor.py:1972`; `bin/disjuntor.py:1975`; `bin/codex_pergunta.sh:288`; `CLAUDE.md:348`. |
| **8** | **Risco de compatibilidade declarado da forma restritiva:** exige `? cortada por Ronald <token>` no fim; texto posterior invalida. Não é outro escape; é redução das formas aceitas. Aceitação de formatos reais afetados: **[nao verificado]**. `bin/disjuntor.py:1454`; `bin/disjuntor.py:1513`. |
| **9** | **Fora do aval funcional de I1; ajuste coerente do selo vizinho.** A mutação continua retirando `bash|sh`; `env` passou a ramo separado. Verde efetivo nesta auditoria: **[nao verificado]**. `bin/disjuntor.py:1288`; `bin/tests/test_disjuntor.sh:1741`. |

**Chamadores expostos ao ponto 6:** agentes construtores em cópia, inclusive a rota documental `wt-ui`; wrapper com obra herdada por `export`; wrapper alcançado por script ou `bash -c`. Os dois primeiros podem receber negativa diretamente no hook; os últimos passam pela porta acrescentada ao wrapper. `CLAUDE.md:306`; `CLAUDE.md:309`; `bin/disjuntor.py:1691`; `bin/disjuntor.py:1711`; `bin/codex_pergunta.sh:230`.

**Lista de processos/prompts individualmente vivos: [nao verificado].** Os exemplos de `export` e Agent sem obra nos anexos são fixtures, não identificação de chamadores de produção. `bin/tests/test_censo_antes_da_vaga.sh:174`; `bin/tests/test_censo_antes_da_vaga.sh:178`.

O predicado exato do Agent é:

- `tool_name == 'Agent'`;
- tipo fora de `{'verificador','Explore','Plan','claude-code-guide','statusline-setup'}`;
- prompt casando `COPIA_RE = /[^\s"'\\:]*/wt-[A-Za-z0-9._-]+`;
- obra obtida pela primeira ocorrência válida entre as âncoras `obra[s] <ID>` e `leitura … da <ID>`, esta limitada a 80 caracteres sem quebra. `bin/disjuntor.py:102`; `bin/disjuntor.py:104`; `bin/disjuntor.py:115`; `bin/disjuntor.py:545`; `bin/disjuntor.py:1711`.

O ID validado é `(?:[A-Z][A-Z0-9]*(?:-[A-Za-z0-9]+)+|[A-Z]+\d+[A-Z0-9]*)`. A busca ignora caixa, mas a validação não; cauda opcional participa da identidade dos IDs sem hífen. Reconhecimento sintático não significa pertencer à fila: isso é consultado depois. `bin/disjuntor.py:108`; `bin/disjuntor.py:110`; `bin/disjuntor.py:562`; `bin/disjuntor.py:570`; `bin/disjuntor.py:1567`.

**A sessão principal não tem isenção ao despachar Agent ou wrapper:** a saída antecipada por ausência de `agent_id` exclui expressamente essas chamadas. Que esta sessão tenha emitido uma chamada afetada é **[nao verificado]**. `bin/disjuntor.py:1673`.

3. **PROCEDENTE-candidato — fail-closed ainda tem buracos; há negativas espúrias permanentes.**

JSON truncado levanta em `json.loads`, mantendo `payload={}`. O classificador devolve lista vazia; o fallback pelo texto cru só existe quando o próprio classificador levanta. O caminho termina com rc 0, stdout vazio e “ferramenta liberada”. Esse resultado foi reproduzido em memória. `bin/disjuntor.py:1748`; `bin/disjuntor.py:1751`; `bin/disjuntor.py:1594`; `bin/disjuntor.py:1611`; `bin/disjuntor.py:1790`.

BACKLOG legível, mas sem marcador OBRAS, também vira fila vazia: `_itens_andaveis` retorna `[]`, e o censo libera como fora da fila. Ausência de estrutura não é distinguida de universo realmente vazio. `bin/hook_stop_fila1.py:225`; `bin/disjuntor.py:1546`; `bin/disjuntor.py:1567`; `bin/disjuntor.py:2083`.

Falhas de importação anteriores a `main_pretool` não estão protegidas pelo seu `try`; settings chama Python diretamente. **Que o harness real abra após traceback/rc 1 é [nao verificado]**: seu consumidor de PreToolUse não está demonstrado pela cópia. `bin/disjuntor.py:59`; `bin/disjuntor.py:1747`; `.claude/settings.json:68`.

No inverso, há duas recusas independentes da qualidade do brief:

- `export CODEX_OBRA=… CODEX_PAPEL=construtor; wrapper --construtor …` continua sem obra para o hook, mesmo com censo conforme. `bin/disjuntor.py:1385`; `bin/disjuntor.py:1691`.
- IDs como `ABC` e `09-TURNO-ABERTO-EXPOSTA` passam no contrato anterior do wrapper e falham no novo `main_censo`. Não se resolvem corrigindo o brief. Chamadas atuais desses IDs: **[nao verificado]**. `bin/codex_pergunta.sh:188`; `bin/disjuntor.py:108`; `bin/disjuntor.py:2075`.

4. **PROCEDENTE-candidato — ESMERIL-SEMANTICA do PAREI: mesma porta, lugar incompatível.**

Sim: negativa comum e PAREI carregam `esteira_vigia.py` e chamam `no_relato`. Não foi acrescentado um escritor independente nesse caminho. `bin/disjuntor.py:195`; `bin/disjuntor.py:240`; `bin/disjuntor.py:267`; `bin/disjuntor.py:269`.

A string escrita corresponde à forma aceita pelo reconhecedor; o problema é onde ela fica. `bin/disjuntor.py:1457`; `bin/hook_stop_fila1.py:700`.

| Consumidor | Lugar efetivamente lido |
|---|---|
| **Escritor** | Insere dentro de ALARMES, preservando o painel anterior. `app/core/esteira_vigia.py:829`; `app/core/esteira_vigia.py:867`. |
| **Stop** | Primeira linha útil após o cabeçalho. `bin/hook_stop_fila1.py:407`; `bin/hook_stop_fila1.py:438`; `bin/hook_stop_fila1.py:715`. |
| **Handoff** | Coleta até o primeiro `##`; prova a linha devolvida pelo Stop. `bin/handoff_sessao.sh:102`; `bin/handoff_sessao.sh:128`. |
| **Alarme** | Procura nas primeiras 40 linhas. Pode encontrar ALARMES enquanto Stop/handoff não encontram. `bin/alarme_sessao_ociosa.py:124`; `bin/alarme_sessao_ociosa.py:137`. |

A contraprova em memória usando o fixture do próprio selo colocou PAREI na linha 7, enquanto `_linha_do_topo()` devolveu `texto neutro do painel`. O selo apenas conta a ocorrência no arquivo inteiro; o teste do Stop fabrica PAREI diretamente no topo. Falta a prova integrada negativa → escritor → leitor. `bin/tests/test_censo_antes_da_vaga.sh:145`; `bin/tests/test_censo_antes_da_vaga.sh:202`; `bin/tests/test_hook_esperando_com_prova.sh:62`.

5. **OBSERVACAO — `#G:censo` atual recusa qualquer rc não zero antes da vaga.**

A expressão é `python3 -I … --censo "$OBRA" || _recusa2 …`; `_recusa2` termina com rc 2. Portanto **erro Python com traceback/rc 1 recusa**, assim como rc 2, 127 ou outro não zero. O `flock` vem depois. `bin/codex_pergunta.sh:183`; `bin/codex_pergunta.sh:230`; `bin/codex_pergunta.sh:236`.

O selo mantém a vaga ocupada e exige que o motivo seja censo, exercitando essa ordem. `bin/tests/test_codex_pergunta.sh:210`.

6. **PROCEDENTE-candidato — existem mutantes simples não distinguidos pelos casos apresentados.**

As contraprovas abaixo foram exercitadas em memória; **sobrevivência à execução integral dos selos: [nao verificado]**. A previsão de sobrevivência decorre dos casos indicados. `bin/tests/test_censo_antes_da_vaga.sh:149`; `bin/tests/test_censo_antes_da_vaga.sh:186`; `bin/tests/test_hook_esperando_com_prova.sh:233`; `bin/tests/test_codex_pergunta.sh:85`.

| Alvo / mutação | Por que os casos não distinguem; defeito introduzido |
|---|---|
| **`_censo_nega`: `+ ok` → `+ ok[-1:]`** | O caso com vários citados põe o ruim por último. Continua negando ali, mas libera **ruim seguido de bom**, ignorando o primeiro. `bin/disjuntor.py:1570`; `bin/tests/test_censo_antes_da_vaga.sh:149`. |
| **`_briefs_da_obra`: glob ordenado → glob ordenado `[-1:]`** | O cenário múltiplo põe `brief_v2.md` ruim por último. Falta a inversão: primeiro ruim, último bom. `bin/disjuntor.py:1536`; `bin/tests/test_censo_antes_da_vaga.sh:181`; `bin/tests/test_censo_antes_da_vaga.sh:186`. |
| **`prova_parei_censo`: não reconhecido retorna `''`, em vez de `None`** | Os casos novos chamam diretamente a prova apenas com linhas de censo. O handoff passaria a declarar espera provada para painel comum, pois trata `''` como sucesso. `bin/hook_stop_fila1.py:698`; `bin/handoff_sessao.sh:128`; `bin/tests/test_hook_esperando_com_prova.sh:237`. |
| **`#G:censo`: recusar somente rc 1** | O fake recusa exclusivamente com rc 1. O mutante mantém esse caso e libera rc 2/127. `bin/tests/test_codex_pergunta.sh:85`; `bin/tests/test_codex_pergunta.sh:210`; `bin/tests/test_codex_pergunta.sh:525`. |

Também falta assert numérico do contador `ok`: os asserts numéricos cobrem abertos, negados e horas. `bin/tests/test_censo_antes_da_vaga.sh:246`.

7. **PROCEDENTE-candidato — trilhas e medidas ainda atribuem significados diferentes ao mesmo evento.**

| Linha | Quem escreve | Quem lê / significado |
|---|---|---|
| **CENSO OK** | CLI por `_censo_nega`; Agent após `_pretool` permitir. `bin/disjuntor.py:1581`; `bin/disjuntor.py:1664`. | `_censo_24h` aumenta `ok`, encerra espera e apresenta “aberturas que passaram”. `bin/disjuntor.py:1961`; `bin/disjuntor.py:1963`; `bin/disjuntor.py:1988`. |
| **CENSO FORA** | `main_censo`. `bin/disjuntor.py:2084`. | Registra par obra/PID como cobertura suficiente; não aumenta `ok` nem encerra espera anterior. `bin/disjuntor.py:1955`; `bin/disjuntor.py:1960`. |
| **NEGA: linha real `NEGOU DISJUNTOR CENSO`** | PreToolUse normal, recuperação de exceção e CLI. `bin/disjuntor.py:1763`; `bin/disjuntor.py:1774`; `bin/disjuntor.py:2089`. | Conta negativas; abre intervalo somente se reconhecer `construtor de <obra> sem CENSO`. Negativas sem obra não abrem intervalo. `bin/disjuntor.py:1949`. |
| **CENSO DESLIGADO** | `_censo_desligado`, para Agent. `bin/disjuntor.py:1742`. | Soma em `abertos_sem`. `bin/disjuntor.py:1958`; `bin/disjuntor.py:1977`. |
| **PAREI censo** | `_parei_censo_no_relato` pela porta do vigia. `bin/disjuntor.py:269`. | Stop, handoff e alarme, com os recortes divergentes do achado 4. `bin/hook_stop_fila1.py:716`; `bin/handoff_sessao.sh:128`; `bin/alarme_sessao_ociosa.py:137`. |
| **`chamadas.log` / `ini`** | Wrapper, antes da chamada efetiva. `bin/codex_pergunta.sh:288`. | `codex_limite_estado._linhas_chamadas`, depois `_censo_24h`. `bin/codex_limite_estado.py:49`; `bin/disjuntor.py:1972`. |

O **OK da CLI significa brief aprovado**, pois ocorre antes da vaga. O leitor o transforma em **construtor aberto**. CLI avulsa ou recusa posterior por vaga ocupada aumenta aberturas e encerra espera sem abertura. `bin/disjuntor.py:2087`; `bin/codex_pergunta.sh:230`; `bin/codex_pergunta.sh:236`; `bin/disjuntor.py:1961`.

A correlação aceita `(obra,pid)` de toda a história, sem contemporaneidade ou hash. Um PID reutilizado pode esconder abertura sem aprovação contemporânea; a chave original do chamadas.log dispõe também de hash. Ocorrência real: **[nao verificado]**. `bin/disjuntor.py:1957`; `bin/disjuntor.py:1975`; `bin/codex_limite_estado.py:63`.

A união corrige sobreposição, mas mede intervalos NEGOU→OK, sem consultar ocupação da vaga; não prova “horas de vaga ociosa”. `bin/disjuntor.py:1924`; `bin/disjuntor.py:1953`; `bin/disjuntor.py:1968`.

8. **OBSERVACAO — classes estruturais presentes nesta fatia.**

- **Segundo juiz:** não apareceu clone do juiz de forma; há divisão da autoridade por raiz entre censo e vaga. `bin/disjuntor.py:1575`; `bin/codex_pergunta.sh:76`; `bin/codex_pergunta.sh:230`.
- **Segundo escritor fora da porta:** não neste caminho; a violação é leitura de idempotência fora da trava. `bin/disjuntor.py:261`; `bin/disjuntor.py:269`; `app/core/esteira_vigia.py:848`.
- **Borda temporal:** correlação com PID histórico sem limite de tempo, comprovada no leitor; ocorrência produtiva **[nao verificado]**. `bin/disjuntor.py:1957`; `bin/disjuntor.py:1975`.
- **Derivação paralela:** recortes independentes decidem quais linhas representam a parada atual. `bin/hook_stop_fila1.py:407`; `bin/handoff_sessao.sh:102`; `bin/alarme_sessao_ociosa.py:124`.
- **Selo sem mordida:** falta ligação escritor→Stop e faltam as inversões/rcs dos mutantes acima. `bin/tests/test_censo_antes_da_vaga.sh:202`; `bin/tests/test_hook_esperando_com_prova.sh:62`; `bin/tests/test_codex_pergunta.sh:85`.
- **Ordem de trava:** censo antes da vaga está correto; deduplicação antes do lock permanece incorreta. `bin/codex_pergunta.sh:230`; `bin/codex_pergunta.sh:236`; `bin/disjuntor.py:261`.
- **Campo com dois significados:** **presente em CENSO OK**, aprovação do brief no escritor e abertura efetiva no leitor. `bin/disjuntor.py:1581`; `bin/disjuntor.py:1988`.

Os verdes relatados pelo construtor não foram reexecutados nesta auditoria; execução integral e comportamento de produção permanecem **[nao verificado]**.

## 09/11 03:13 · sessao 4f60167f1d5f · PAR-NAO-ADJACENTE · censo · origem=Code

**1. A adjacência não tem justificativa de lei demonstrada**

O contrato registrado manda transportar **o par do juiz inteiro**, acomodar a saída na última ponta e tratar soltas pelo marco mais próximo livre. Não exige pontas consecutivas entre as batidas; essa condição aparece na implementação. (`app/docs/RELATO.md:1710`, `app/escala/utils.py:467`, `app/escala/utils.py:507`, `app/escala/utils.py:511`)

O pouso `b3bf529e`, em 29/10, está registrado. A introdução precisa da condição em `9da200c8` e a razão histórica dessa escolha ficam **[nao verificado]**: esta cópia não contém o histórico Git consultável. (`app/docs/RELATO.md:1657`)

Os casos pedidos não demonstram dependência da adjacência:

| Caso | O que protege |
|---|---|
| **col345** | A entrada às 15:00 cumpriu a volta `hfi`; o período de trabalho é 15:00→18:56. Não apresenta pausa com solta interposta. (`app/ponto/tests/test_calculador_portoes_de_marco.py:15`, `app/ponto/tests/test_calculador_portoes_de_marco.py:56`) |
| **col60/O271** | E12:00→S19:06: cabeça atrasada em 300 minutos, mesmo estando sobre `hii`. As contraprovas distinguem entrada em `hfi`. (`app/ponto/tests/test_o271_pontualidade_no_comercial.py:177`, `app/ponto/tests/test_o271_pontualidade_no_comercial.py:192`) |
| **[nome]** | Cluster de quatro batidas não pode preencher marcos distantes; a proteção é o filtro final, independente de `_iz == _i + 1`. (`app/ponto/tests/test_pareador_cluster_marco.py:53`, `app/escala/utils.py:572`) |
| **Nominais da obra** | A, controle, convergência e noturno usam quatro batidas, com pontas internas adjacentes; B/C/D/E/F exercitam bordas e soltas. (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:104`, `:129`, `:143`, `:154`, `:165`, `:212`, `:223`, `:326`, `:358`) |

Por inspeção, esses exemplos preservam a elegibilidade dos pares ao trocar a comparação por `>`; resultado de executar a alteração contra a bateria: **[nao verificado]**. Há precedente explícito protegendo o **par declarado contra o adjacente guloso**, embora a sobra daquele teste esteja antes do par. (`app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:701`)

**O contraexemplo à troca isolada é o aninhamento descrito abaixo**, permitido pela construção atual do extrator. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`)

**2. Arranjos que precisam ser distinguidos**

Os leitores públicos repartem os mesmos pares marcados pelo piso: `intervalos_do_turno` entrega os de pelo menos 15 minutos; `pausas_abaixo_do_piso`, o restante. A adjacência não é garantia conjunta deles: a passagem declarada casa suas pontas primeiro. (`app/ponto/turnos.py:545`, `app/ponto/turnos.py:565`, `app/ponto/turnos.py:811`)

| Arranjo | Resultado da leitura |
|---|---|
| **Uma solta entre duas pontas declaradas** | O par declarado permanece inteiro; a batida sem outro papel é `sol`, tratada depois. Rejeitá-lo por adjacência contradiz esse contrato. (`app/ponto/turnos.py:811`, `app/escala/utils.py:440`, `app/escala/utils.py:507`, `app/escala/utils.py:511`) |
| **Pares cruzados** | Com instantes distintos e lista cronológica, os dois laços atuais não os produzem: declarados são casados sucessivamente; os demais usam vizinhos sem pontas declaradas. (`app/ponto/turnos.py:1217`, `app/ponto/turnos.py:812`, `app/ponto/turnos.py:819`, `app/ponto/turnos.py:820`) |
| **Pares aninhados** | São possíveis: a primeira passagem reserva apenas as pontas declaradas; um par adjacente inteiramente dentro delas pode sobreviver à segunda passagem. (`app/ponto/turnos.py:815`, `app/ponto/turnos.py:820`, `app/ponto/turnos.py:880`) |
| **Volta igual à saída verdadeira** | As bordas reais são excluídas dos pares por identidade. Deve permanecer o limite interior, além de `iz > i`; a última batida continua `sai`. (`app/ponto/turnos.py:802`, `app/ponto/turnos.py:811`, `app/ponto/turnos.py:880`, `app/escala/utils.py:473`) |
| **“Solta” que é ponta de outro par** | Ela tem papel de par. Não se pode pular todo o trecho até a volta e apagar essas pontas. (`app/escala/utils.py:440`, `app/escala/utils.py:507`) |
| **Duas pausas disjuntas com solta entre elas** | Transportam-se os dois pares em ordem; depois a solta tenta seu marco mais próximo, livre e compatível com a cronologia. (`app/escala/utils.py:539`, `app/escala/utils.py:564`) |

**Contraexemplo concreto, deduzido do código:** ata/DNA `08/12/13/17`; batidas `E08, S12, S12:10, E12:30, E13, S17`. As extras podem receber `X`; 12/13 recebem `Xi`; o pareador absorve as internas. O extrator pode devolver **12→13 e 12:10→12:30**. Ocorrência desse arranjo na frota: **[nao verificado]**. (`app/ponto/turnos.py:1761`, `app/ponto/turnos.py:1787`, `app/ponto/turnos.py:1804`, `app/ponto/turnos.py:1827`, `app/ponto/turnos.py:1315`, `app/ponto/turnos.py:1371`, `app/ponto/turnos.py:811`, `app/ponto/turnos.py:880`)

Com apenas uma pausa cadastral, aceitar ambos faz o alinhador retornar `None` e seguir para a DP; com duas, ele pode distribuir externo e interno em pausas sucessivas, invertendo a cronologia entre os pares. (`app/escala/utils.py:530`, `app/escala/utils.py:535`, `app/escala/utils.py:561`, `app/escala/utils.py:613`, `app/escala/utils.py:625`)

A **L-085** conserva o dia de início da jornada; a **L-087** impede decidir papel pelo tipo gravado; **[nome]** ainda pode retirar associações no filtro final. Nenhuma prioridade explícita para pausas aninhadas foi demonstrada: **[nao verificado]**. (`app/docs/LEIS.md:117`, `app/docs/LEIS.md:119`, `app/escala/utils.py:578`)

**3. Cura mínima na fonte e derivações paralelas**

Para pares **internos, disjuntos e com pontas identificáveis**, a mudança local é reconhecer:

```python
0 < _i < _iz < len(_bats) - 1
```

em lugar da exigência de vizinhança. Conservam-se as pontas e a associação S/E entregues pelo juiz; não se procura outro parceiro. Isso é desenho proposto, ainda não implementado nem testado. (`app/escala/utils.py:454`, `app/escala/utils.py:467`; execução **[nao verificado]**)

O percurso deve continuar visitando as batidas entre as pontas. As que não pertencem a nenhum par recebem `sol`; as pontas de outros pares conservam seu papel. O alinhador já acomoda bordas e pares antes de examinar soltas, que não deslocam ocupantes; o filtro [nome] continua por último. (`app/escala/utils.py:461`, `app/escala/utils.py:468`, `app/escala/utils.py:472`, `app/escala/utils.py:558`, `app/escala/utils.py:564`, `app/escala/utils.py:578`)

**Não considero segura a cura geral como uma troca isolada de operador.** É preciso conferir o conjunto completo de pares antes de transcrevê-lo. Para sobreposição, o desenho sem fallback é explicitar o conflito, sem escolher vencedor no adaptador nem acrescentar queda para a DP. Esse tratamento é proposta; sua implementação e comportamento ficam **[nao verificado]**. A necessidade decorre do aninhamento permitido e da capacidade limitada do alinhador. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`, `app/escala/utils.py:535`)

Há outras adjacências a distinguir:

- **`_papeis_por_alternancia`** deriva pausas dos vizinhos S/E dos períodos. É a derivação paralela de papel usada sem `turno=`, exigindo pelo menos quatro batidas alternadas e duração até 26 horas. Não consulta os pares completos do turno. (`app/escala/utils.py:477`, `app/escala/utils.py:489`, `app/escala/utils.py:496`, `app/escala/utils.py:597`)
- **`_pares_marcados`** mantém adjacência própria para os pares residuais, depois dos declarados; essa é parte da autoridade existente, não deve ser refeita no adaptador. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`)
- **`_pausa_fechada_carimbada`** também percorre vizinhos, mas responde à figura da L-115, inclusive onde o extrator de pausas corretamente devolve vazio. Não é substituível mecanicamente por `intervalos_do_turno`. (`app/ponto/turnos.py:912`, `app/ponto/turnos.py:915`, `app/ponto/turnos.py:924`, `app/ponto/turnos.py:1005`)
- **`_borda_posicional`** procura um único vão interior adjacente somente quando nenhum turno tem pausa marcada; é outra pergunta, com exclusão explícita da autoridade já presente. (`app/ponto/turnos.py:654`, `app/ponto/turnos.py:679`, `app/ponto/turnos.py:697`)

**Pergunta de lei 1:** quando o extrator entrega uma pausa declarada externa e outra pausa inteiramente dentro dela, qual resultado deve valer: somente a declarada, ambas com tratamento explícito da sobreposição, ou turno conflitante? O código admite a figura; a regra de desempate permanece **[nao verificado]**. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`)

**4. Leitores e dinheiro**

| Caminho | Leitura e consequência |
|---|---|
| **Ata** | O builder passa `turno=` ao alinhador; monta cópias das células com as novas lâmpadas/órfãs, repareia e calcula o realizado. O cartório serializa esse número. É o caminho diretamente atingido por `_papeis_do_turno`. (`app/escala/utils.py:1701`, `app/escala/utils.py:1886`, `app/escala/utils.py:1898`, `app/escala/utils.py:2045`, `app/ponto/services/cartorio.py:706`) |
| **Zona 5** | Chama `_match_marcos` sem turno, sobre os períodos; identifica P1/P2 e aplica atraso/antecipada. Usa a derivação por alternância quando elegível. (`app/ponto/motor_calculo_v2.py:3060`, `app/ponto/motor_calculo_v2.py:3062`, `app/ponto/motor_calculo_v2.py:3077`, `app/escala/utils.py:597`) |
| **`marco_ocupado_pela_batida`** | Chama o mesmo alinhador sem turno e transcreve rótulo/delta; identifica a batida por HH:MM. (`app/escala/utils.py:427`, `app/escala/utils.py:429`) |
| **`regua_defesa.cumpriu_a_volta_do_intervalo`** | Consulta esse leitor com um período e exige `hfi` dentro da tolerância. Com duas batidas, permanece na DP; motor e calculador usam a resposta. (`app/escala/regua_defesa.py:489`, `app/escala/regua_defesa.py:493`, `app/escala/utils.py:489`, `app/ponto/motor_calculo_v2.py:2117`, `app/ponto/calculador/regras.py:251`) |
| **Outros chamadores diretos** | Builder single, passagem de diagnóstico e detector de anomalias desabilitam a derivação. (`app/escala/utils.py:776`, `app/escala/utils.py:1650`, `app/chamados/detector_anomalias.py:212`) |

Assim, mudar `_papeis_do_turno` não muda diretamente todos os chamadores. O efeito financeiro também pode chegar **pela ata**, cujo papel alimenta o pareador do motor e os intervalos transportados pelo adaptador. (`app/escala/utils.py:596`, `app/ponto/motor_calculo_v2.py:495`, `app/ponto/motor_calculo_v2.py:503`, `app/ponto/adaptador_motor.py:44`)

Os números envolvidos têm contas distintas:

- **Realizado da ata:** minutos HH:MM entre bordas, menos os intervalos reconhecidos. (`app/ponto/turnos.py:740`, `app/ponto/turnos.py:756`, `app/ponto/turnos.py:758`, `app/ponto/turnos.py:767`)
- **Pagamento e intrajornada:** no 12x36, o motor recorta a janela, desconta a pausa real e calcula supressão/tolerância; `DiaPago` recebe trabalhadas e intra indenizada convertidas em horas. (`app/ponto/motor_calculo_v2.py:2759`, `app/ponto/motor_calculo_v2.py:2764`, `app/ponto/motor_calculo_v2.py:2771`, `app/ponto/motor_calculo_v2.py:2784`, `app/ponto/services/dia_pago.py:178`, `app/ponto/services/dia_pago.py:186`)
- **Leitores derivados:** leitor da célula, PDF, causas, classificação, calendário e comparação ata/juiz consomem o realizado; fechamento e percentual ainda aplicam teto diário pelo previsto. (`app/escala/services/leitor_celula.py:406`, `app/folha/validacao_pdf.py:54`, `app/folha/causas.py:85`, `app/ponto/supra_juiz.py:224`, `app/colaboradores/services/calendario.py:359`, `app/ponto/services/bordas_realizado.py:87`, `app/ponto/services/dia_pago.py:394`, `app/ponto/services/fechamento.py:1064`, `app/folha/export.py:1096`)

**No dado raspado fornecido**, gravado→rejulgado:

| Campo | Antes → depois |
|---|---|
| Ata `minutos_realizados` | **660 → 599 min**, diferença −61. **[nao verificado]** |
| Motor `intra_descontada` | **58,0130167 → 119,6599667 min**, diferença +61,64695. **[nao verificado]** |
| Intrajornada indenizada | **0 → 0**. **[nao verificado]** |
| Pagamento lavrado exibido | **10,87 h → 10,87 h**, rubricas exibidas iguais. **[nao verificado]** |

Esses rótulos indicam ausência de arquivo:linha local para o anexo. O segundo `minutos_trabalhados` foi raspado; novo pagamento lavrado, impacto em reais e impacto financeiro da frota permanecem **[nao verificado]**. O espelho distingue realizado calculado de pagamento lavrado. (`app/ponto/services/espelho.py:783`, `app/ponto/services/espelho.py:799`, `app/ponto/services/espelho.py:811`)

**5. Sonda de impacto — somente texto**

A sonda usa `CargaDaJanela`, que carrega `batidas_apuraveis` e chama o mesmo corpo de `turnos_do_colab` usado pela ata. Conta turnos fechados com célula existente, sem julgar ou lavrar. (`app/ponto/turnos.py:3009`, `app/ponto/turnos.py:3022`, `app/escala/utils.py:1581`)

O agrupamento usa **21..20**, conforme solicitado, pela função existente; empresa significa o FK atual do colaborador, sem reconstrução histórica. (`app/ponto/janelas.py:24`, `app/colaboradores/models.py:321`) O banco sombra mantém o schema `juliani`. (`app/config/settings/sombra.py:13`, `app/config/settings/sombra.py:29`)

Texto para shell Django inicializado; execução e resultados: **[nao verificado]**.

```python
import json
from collections import defaultdict
from datetime import date

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from ponto.janelas import periodo_apuracao_atual
from ponto.turnos import (
    CargaDaJanela,
    intervalos_do_turno,
    pausas_abaixo_do_piso,
)

NAME = connection.settings_dict["NAME"]
assert NAME == "sombra"

class _Rollback(Exception):
    pass

saida = None

try:
    with schema_context("juliani"):
        with transaction.atomic():
            with connection.cursor() as cursor:
                cursor.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cursor.execute("SELECT current_database()")
                assert cursor.fetchone()[0] == "sombra"

            inicio = date(2026, 9, 21)
            hoje = timezone.localdate()
            assert hoje >= inicio

            # Todos, sem filtro de situação.
            # app/colaboradores/models.py:321
            colabs = list(
                Colaborador.objects.all().only("id", "empresa_id").order_by("pk")
            )

            # Mesmo corpo do pareador, universo apurável e pad oficiais.
            # app/ponto/turnos.py:2953,3009,3022
            carga = CargaDaJanela(colabs, inicio, hoje)
            grupos = defaultdict(list)
            examinados = 0
            fechados_sem_celula = 0

            for colab in colabs:
                for turno in carga.turnos(colab):
                    if (
                        turno.aberto
                        or turno.entrada is None
                        or turno.saida is None
                    ):
                        continue

                    dia = turno.data_turno
                    assert dia is not None and inicio <= dia <= hoje

                    if carga.celula(colab.pk, dia) is None:
                        fechados_sem_celula += 1
                        continue

                    examinados += 1

                    # Os dois leitores repartem os pares do juiz pelo piso.
                    # app/ponto/turnos.py:545,565
                    pares = (
                        list(intervalos_do_turno(turno))
                        + list(pausas_abaixo_do_piso(turno))
                    )
                    if not pares:
                        continue

                    # Só mede posições; não procura nem escolhe parceiros.
                    # Mesma ordenação de app/escala/utils.py:450.
                    bats = sorted(turno.batidas, key=lambda b: b.timestamp)
                    pos = {b.timestamp: i for i, b in enumerate(bats)}

                    # Sem resultado parcial baseado em índice ambíguo.
                    if len(pos) != len(bats):
                        raise RuntimeError(
                            f"timestamp ambiguo colab_pk={colab.pk} data={dia}"
                        )
                    if any(a not in pos or z not in pos for a, z in pares):
                        raise RuntimeError(
                            f"ponta ausente colab_pk={colab.pk} data={dia}"
                        )

                    if not any(pos[z] > pos[a] + 1 for a, z in pares):
                        continue

                    # Corte 21 explicitamente solicitado.
                    # app/ponto/janelas.py:24
                    comp_ini, comp_fim = periodo_apuracao_atual(dia, 21)
                    grupos[(comp_ini, comp_fim, colab.empresa_id)].append(
                        (colab.pk, dia.isoformat())
                    )

            linhas = []
            for (ci, cf, empresa_pk), ocorrencias in sorted(grupos.items()):
                linhas.append({
                    "competencia": cf.strftime("%Y-%m"),
                    "janela": [ci.isoformat(), cf.isoformat()],
                    "empresa_pk": empresa_pk,
                    "n_turnos": len(ocorrencias),
                    "n_colabs": len({pk for pk, _ in ocorrencias}),
                    # Uma entrada por turno afetado; pode repetir pk/data.
                    "colab_pk_data": sorted(ocorrencias),
                })

            saida = {
                "inicio": inicio.isoformat(),
                "hoje": hoje.isoformat(),
                "turnos_fechados_com_celula": examinados,
                "turnos_fechados_sem_celula": fechados_sem_celula,
                "turnos_afetados": sum(len(v) for v in grupos.values()),
                "grupos_afetados": linhas,
            }

            # Rollback também no término normal.
            raise _Rollback()

except _Rollback:
    pass

print(json.dumps(saida, ensure_ascii=False, separators=(",", ":")))
```

A contagem mede presença de par não adjacente; não estima mudança de ata ou dinheiro, que também depende das guardas e do alinhamento subsequente. (`app/escala/utils.py:600`, `app/escala/utils.py:613`, `app/escala/utils.py:578`)

perguntas de lei: 1

## 09/11 03:10 · sessao 67cbec47612d · - · - · origem=Ronald (tmux)

Q2/Q3 — leitura concluída, sem execução.

- **A adjacência não é garantia do juiz.** `_pares_marcados` casa primeiro as pontas declaradas, podendo haver batidas entre elas; depois examina pares adjacentes remanescentes. Os dois leitores públicos apenas repartem esse resultado pelo piso. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`, `app/ponto/turnos.py:545`, `app/ponto/turnos.py:565`)

- **Aninhamento é estruturalmente alcançável.** Exemplo por inspeção: DNA/ata `08/12/13/17`; batidas `E08,S12,S12:10,E12:30,E13,S17`. As duas extras podem passar como `_resto` de duas batidas, virar `X`; 12/13 viram `Xi`. O pareador carimba e absorve todas as quatro internas, fechando às 17. O juiz devolve `(12,13)` e `(12:10,12:30)`: reserva apenas as pontas declaradas, deixando as duas extras disponíveis ao segundo laço. Ocorrência real desse exemplo: **[não verificado]**. (`app/ponto/turnos.py:1761`, `app/ponto/turnos.py:1787`, `app/ponto/turnos.py:1804`, `app/ponto/turnos.py:1827`, `app/ponto/turnos.py:1315`, `app/ponto/turnos.py:1371`, `app/ponto/turnos.py:1491`, `app/ponto/turnos.py:811`, `app/ponto/turnos.py:820`, `app/ponto/turnos.py:880`)

- **Só `==` → `>` não basta para aquele arranjo.** Com uma pausa cadastral, os dois pares fazem `_alinhar_por_papel` devolver `None`, caindo no alinhamento antigo; com duas pausas, o par externo pode ocupar a primeira e o interno a segunda, produzindo sequência temporal inversa entre a volta externa e a ida interna. O alinhador não valida a ordem cronológica entre pares; distribui-os pela ordem numérica recebida. (`app/escala/utils.py:530`, `app/escala/utils.py:535`, `app/escala/utils.py:551`, `app/escala/utils.py:561`, `app/escala/utils.py:613`, `app/escala/utils.py:625`)

- **Cruzamento não nasce dos dois laços sobre uma lista cronológica com instantes distintos:** declarados são emparelhados sucessivamente; os outros têm pontas adjacentes, sem usar pontas declaradas. Aninhamento permanece possível porque não se reserva o interior. O pareador ordena a entrada por timestamp. (`app/ponto/turnos.py:1217`, `app/ponto/turnos.py:812`, `app/ponto/turnos.py:819`, `app/ponto/turnos.py:820`)

- **Volta que é saída verdadeira não é pausa do juiz:** as bordas são excluídas por identidade na passagem declarada e na gulosa. Portanto a cura deve preservar o limite interior `0 < i < iz < len(bats)-1`; a última batida continua `sai`, sentada no último marco. (`app/ponto/turnos.py:802`, `app/ponto/turnos.py:811`, `app/ponto/turnos.py:880`, `app/escala/utils.py:467`, `app/escala/utils.py:473`, `app/escala/utils.py:559`)

- **Solta entre pontas:** só recebe `sol` se não pertence a nenhum par. Não se deve “pular até a volta”, pois isso apagaria os papéis de outras pontas interiores. O contrato manda transportar todos os pares e depois tratar as soltas, sem mover borda/par; cada solta tenta o marco mais próximo, livre e compatível com a ordem. O cluster guard [nome] ainda roda por último e pode retirar uma associação já feita. (`app/escala/utils.py:440`, `app/escala/utils.py:507`, `app/escala/utils.py:511`, `app/escala/utils.py:564`, `app/escala/utils.py:578`)

- **Duas pausas disjuntas com solta entre elas:** transcrever ambos os pares na ordem e manter a batida intermediária como `sol`; a distribuição busca a menor distância total entre pares e pausas disponíveis. L-085 mantém o dia da jornada; L-087 impede decidir o papel pelo tipo gravado; nenhuma das duas fornece prioridade para pares sobrepostos. (`app/escala/utils.py:539`, `app/escala/utils.py:553`, `app/escala/utils.py:564`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:119`; prioridade legal para sobrepostos **[não verificado]**)

- **Desenho mínimo:** em `_papeis_do_turno`, reconhecer pontas interiores não adjacentes por `_iz > _i`, conferir primeiro o conjunto completo de pontas e só então atribuir `sol` às demais. Preservar a associação S/E do juiz e o alinhador existente. A cura geral precisa tornar explícito o conflito de sobreposição, sem selecionar vencedor nem acrescentar queda para DP. O código atual não possui esse tratamento. (`app/escala/utils.py:454`, `app/escala/utils.py:467`, `app/escala/utils.py:472`, `app/escala/utils.py:613`; proposta de tratamento explícito **[não verificado]**)

- **Derivação paralela:** `_papeis_por_alternancia` reconstrói pausas por S/E adjacentes na forma `E S E S …`; só é usada sem `turno`, com `derivar=True`, ao menos quatro batidas, tipos alternados e duração até 26h. É a outra derivação de papel, pertinente à Zona 5. (`app/escala/utils.py:477`, `app/escala/utils.py:489`, `app/escala/utils.py:496`, `app/escala/utils.py:596`)

- **Outra adjacência, pergunta diferente:** `_pausa_fechada_carimbada` usa `zip(bats,bats[1:])`, duas pontas carimbadas e piso/teto; alimenta `_saida_pela_posicao_entre_turnos`. Sua docstring distingue figura de dinheiro e declara casos em que `_pares_marcados` corretamente não entrega pausa; substituir essa função por `intervalos_do_turno` apagaria essa diferença de contrato. (`app/ponto/turnos.py:912`, `app/ponto/turnos.py:915`, `app/ponto/turnos.py:924`, `app/ponto/turnos.py:1005`)

**Decisão de lei candidata:** quando o juiz devolve pausa declarada externa e outra pausa inteiramente dentro dela, qual é a pausa válida — conservar ambas, privilegiar a declarada ou considerar o turno conflitante? O código permite a figura; a regra localizada não resolve o conflito. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`, `app/escala/utils.py:507`; resposta legal **[não verificado]**)

## 09/11 03:10 · sessao cd0a18573d10 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — P6 curado parcialmente; `OK` continua com dois significados.** O escritor registra aprovação do brief antes da vaga, mas o leitor chama isso de “aberturas que passaram”. Uma CLI avulsa ou uma chamada posteriormente recusada por vaga ocupada aumenta `ok` e encerra a espera sem abrir construtor. Fontes: `bin/disjuntor.py:1579`, `bin/disjuntor.py:2087`, `bin/codex_pergunta.sh:230`, `bin/codex_pergunta.sh:236`, `bin/disjuntor.py:1961`, `bin/disjuntor.py:1963`, `bin/disjuntor.py:1988`. Prova em memória, executando funções extraídas por AST: `NEGOU 11:00 + OK 11:30`, sem nenhum `ini`, devolveu `ok=1`, `ociosa_h=0.5`, `esperando=[]`; execução integral dos selos [nao verificado].

2. **PROCEDENTE-candidato — correlação da medida admite PID reutilizado.** Os pares `(obra,pid)` são recolhidos de toda a história, sem verificar contemporaneidade ou hash; a chamada é dispensada apenas por encontrar esse par. Fontes: `bin/disjuntor.py:1955`, `bin/disjuntor.py:1957`, `bin/disjuntor.py:1975`. O registro original dispõe também de hash: `bin/codex_limite_estado.py:63`. Prova em memória: `OK` de 01/10, obra/pid111, mais `ini` de 09/11 com outro hash/pid111, sem aprovação contemporânea, produziu `abertos_sem=0`; incidência real [nao verificado].

3. **PROCEDENTE-candidato — ponto aberto 7 não garante cobertura fora do wrapper.** A medida lê somente `ini` de `logs/codex/chamadas.log`; o próprio wrapper é seu escritor. Uma abertura direta sem esse registro não entra na conta; a exceção implementada é `Agent` com chave desligada. Fontes: `bin/codex_pergunta.sh:249`, `bin/codex_pergunta.sh:288`, `bin/codex_limite_estado.py:52`, `bin/disjuntor.py:1972`, `bin/disjuntor.py:1975`, `bin/disjuntor.py:1742`. Aberturas reais fora do wrapper [nao verificado].

4. **PROCEDENTE-candidato — P2 tem contenção diferente entre glob e citados.** Citados exigem que o caminho real fique sob `RAIZ/logs/<dir>`; arquivos descobertos pelo glob são convertidos por `realpath` sem a mesma verificação. Assim, condicionalmente a um symlink em `logs/<OBRA>/brief*.md` apontando para fora, o juiz recebe o destino externo já resolvido. Fontes: `bin/disjuntor.py:1520`, `bin/disjuntor.py:1525`, `bin/disjuntor.py:1536`, `bin/disjuntor.py:1570`, `bin/disjuntor.py:1575`. Existência desses symlinks [nao verificado].

5. **PROCEDENTE-candidato — mutante simples do glob escapa à cobertura apresentada.** Alterar o retorno de `_briefs_da_obra` para `sorted(glob.glob(...))[-1:]` conserva o cenário testado, cujo último brief é ruim, mas deixa passar um primeiro brief ruim seguido de último bom. Fontes: `bin/disjuntor.py:1536`, `bin/tests/test_censo_antes_da_vaga.sh:181`, `bin/tests/test_censo_antes_da_vaga.sh:186`, `bin/tests/test_censo_antes_da_vaga.sh:188`. Prova em memória executada: original/mutante negam bom+ruim; original nega ruim+bom; mutante libera ruim+bom. Sobrevivência integral aos selos [nao verificado], prevista pela cobertura lida.

6. **PROCEDENTE-candidato — selo do wrapper só exerce rc1.** Mutante de `#G:censo`: substituir `python ... || _recusa2 ...` por `python ...; [ "$?" -ne 1 ] || _recusa2 ...`. Ele mantém a recusa exercida pelo fake, mas libera rc2/127. Fontes: `bin/codex_pergunta.sh:230`, `bin/tests/test_codex_pergunta.sh:76`, `bin/tests/test_codex_pergunta.sh:85`, `bin/tests/test_codex_pergunta.sh:210`, `bin/tests/test_codex_pergunta.sh:525`. Prova shell em memória: original e mutante retornam 2 para rc1; para rc2/127 original retorna 2 e mutante retorna 0. Sobrevivência integral [nao verificado]. **O código atual recusa qualquer rc diferente de zero, inclusive traceback Python com rc1:** `bin/codex_pergunta.sh:230`.

7. **OBSERVACAO — P5 cura os contraexemplos declarados.** Todas as perguntas são examinadas, a forma do corte termina em `? cortada por Ronald <aval>`, e o positivo exige saída vazia. Fontes: `bin/disjuntor.py:1454`, `bin/disjuntor.py:1512`, `bin/tests/test_censo_antes_da_vaga.sh:90`, `bin/tests/test_censo_antes_da_vaga.sh:139`. O selo não verifica numericamente o contador `ok`; seus asserts numéricos cobrem somente abertos, negados e horas: `bin/tests/test_censo_antes_da_vaga.sh:246`. Portanto, previsão por inspeção: mutante `oks += 0` em `bin/disjuntor.py:1961` não é distinguido; execução integral [nao verificado].

8. **OBSERVACAO — escritores e leitores da trilha.** Todos os eventos usam `trilha`, que escreve `logs/sessao/disjuntor.log` com timestamp local e detalhe limitado a 300 caracteres: `bin/disjuntor.py:72`, `bin/disjuntor.py:139`, `bin/disjuntor.py:144`.
   - `CENSO OK`: `_censo_nega` na CLI ou `pretool` após suas decisões; leitores de medida em `_censo_24h`. Fontes: `bin/disjuntor.py:1581`, `bin/disjuntor.py:1664`, `bin/disjuntor.py:1960`.
   - `CENSO FORA`: `main_censo`; leitor incorpora o PID como aprovação suficiente, mas não encerra espera nem aumenta `ok`. Fontes: `bin/disjuntor.py:2084`, `bin/disjuntor.py:1955`, `bin/disjuntor.py:1960`.
   - `NEGOU DISJUNTOR CENSO`: `main_pretool`, seu tratamento de exceção e `main_censo`; leitor conta negativas e inicia espera apenas quando reconhece `construtor de <obra> sem CENSO`. Fontes: `bin/disjuntor.py:1763`, `bin/disjuntor.py:1774`, `bin/disjuntor.py:2089`, `bin/disjuntor.py:1949`.
   - `CENSO DESLIGADO`: `_censo_desligado`; leitor soma em `abertos_sem`. Fontes: `bin/disjuntor.py:1742`, `bin/disjuntor.py:1958`, `bin/disjuntor.py:1977`.
   - `PAREI censo`: escritor `_parei_censo_no_relato`; leitores compartilham `prova_parei_censo`, mas a análise de lugar/formato ficou com o agente `parei`. Fontes: `bin/disjuntor.py:247`, `bin/hook_stop_fila1.py:692`, `bin/alarme_sessao_ociosa.py:137`, `bin/handoff_sessao.sh:128`.

## 09/11 03:10 · sessao ede921335bff · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — P4 permanece parcial: JSON ilegível abre pelo protocolo do próprio hook.** Se `json.loads` levanta, `payload` continua `{}`; `_aberturas_construtor({})` devolve `[]`, portanto o fallback pelo texto cru nem é executado e `main_pretool` termina com rc 0, sem JSON `deny`. Reprodução em memória, sem escrita: entrada truncada `{"tool_name":"Agent","tool_input":{"prompt":"/home/ronald/wt-x"}` produziu rc 0 e stdout vazio. `bin/disjuntor.py:1748`, `bin/disjuntor.py:1751`, `bin/disjuntor.py:1594`, `bin/disjuntor.py:1611`, `bin/disjuntor.py:1614`, `bin/disjuntor.py:1790`.

2. **OBSERVACAO — exceção anterior ao julgamento também pode terminar sem negativa quando o censo é válido.** `_censo_na_excecao` apenas refaz o censo; se ele passa, retorna `None` e a exceção original termina em “ferramenta liberada”, rc 0. Isso é mais estreito que a promessa textual “exceção = nega”. `bin/disjuntor.py:1618`, `bin/disjuntor.py:1622`, `bin/disjuntor.py:1767`, `bin/disjuntor.py:1790`.

3. **OBSERVACAO — importações anteriores ao `try` não têm proteção local; abertura efetiva pelo harness é [nao verificado].** Os imports globais precedem `main_pretool`; a configuração chama Python diretamente, sem tradutor externo de rc para `deny`. Falha ali pode produzir traceback/rc 1 sem JSON de decisão, mas a implementação do harness que interpreta esse rc não está na cópia examinada. `bin/disjuntor.py:59`, `bin/disjuntor.py:1747`, `.claude/settings.json:68`; comportamento real do harness: **[nao verificado]**. O arquivo `eval/harness.py` executa perguntas HTTP do copiloto e não implementa o consumidor de PreToolUse. `eval/harness.py:14`, `eval/harness.py:52`.

4. **PROCEDENTE-candidato — export herdado é negado mesmo com censo conforme.** O classificador só lê atribuições da mesma invocação. Assim, `export CODEX_OBRA=OBRA-E3 CODEX_PAPEL=construtor; bin/codex_pergunta.sh --construtor ...` chega com `obra=''` e é recusado antes de consultar qualquer brief. O teste exercita essa forma somente sem brief; a restrição excede “export sem brief”. `bin/disjuntor.py:1371`, `bin/disjuntor.py:1385`, `bin/disjuntor.py:1690`, `bin/disjuntor.py:1692`, `bin/tests/test_censo_antes_da_vaga.sh:174`.

5. **PROCEDENTE-candidato — contrato de ID incompatível causa negativas permanentes para formas anteriormente aceitas pelo wrapper.** O wrapper admite `^[A-Z0-9-]+$`; `main_censo` exige `_OBRA_ID`, que começa por letra e exige hífen ou letras seguidas de dígitos. Exemplos `ABC` e `09-TURNO-ABERTO-EXPOSTA` passam no primeiro e falham no segundo independentemente do brief. `bin/codex_pergunta.sh:188`, `bin/disjuntor.py:108`, `bin/disjuntor.py:2075`. A existência de chamada atual dessas formas é **[nao verificado]**; o reconhecedor do BACKLOG admite IDs iniciados por dígito explicitamente. `bin/hook_stop_fila1.py:113`, `bin/hook_stop_fila1.py:120`.

6. **PROCEDENTE-candidato — ponto aberto 4: raiz de julgamento pode divergir da raiz da vaga.** `R` vem do arquivo do wrapper; `VIVO` é separado. O censo executa o disjuntor de `R`, cujo BACKLOG e glob também são de `R`, enquanto a vaga deriva do repositório vivo. Uma cópia com BACKLOG antigo sem aquela obra retorna `CENSO FORA`/rc 0, mesmo que o BACKLOG vivo exigisse censo. `bin/codex_pergunta.sh:55`, `bin/codex_pergunta.sh:56`, `bin/codex_pergunta.sh:76`, `bin/codex_pergunta.sh:230`, `bin/disjuntor.py:71`, `bin/disjuntor.py:1536`, `bin/disjuntor.py:1544`, `bin/disjuntor.py:2083`. Ocorrência dessa divergência em produção: **[nao verificado]**. O selo de cópia apenas fornece disjuntor falso e verifica a vaga ocupada; não confronta dois BACKLOGs. `bin/tests/test_codex_pergunta.sh:392`.

7. **OBSERVACAO — predicado exato de obra reconhecida no Agent.** A chamada entra no censo quando `tool_name == 'Agent'`, `subagent_type` não pertence a `{'verificador','Explore','Plan','claude-code-guide','statusline-setup'}` e o prompt casa `/[^\s"\'\\:]*/wt-[A-Za-z0-9._-]+`. `bin/disjuntor.py:102`, `bin/disjuntor.py:104`, `bin/disjuntor.py:1711`. `_obra_de` procura a primeira ocorrência válida entre `obra[s] <ID>` e `leitura ... da <ID>` — nesta segunda âncora, até 80 caracteres sem quebra —, com regex insensível à caixa, mas validação posterior sensível à caixa. O ID é `(?:[A-Z][A-Z0-9]*(?:-[A-Za-z0-9]+)+|[A-Z]+\d+[A-Z0-9]*)`; cauda opcional `(letra)` ou outro ID hifenizado integra a identidade somente quando o ID original não tem hífen. `bin/disjuntor.py:108`, `bin/disjuntor.py:110`, `bin/disjuntor.py:115`, `bin/disjuntor.py:545`, `bin/disjuntor.py:559`, `bin/disjuntor.py:567`.

8. **OBSERVACAO — chamadores expostos ao ponto 6, distinguindo rotas de ocorrências.**
   - Agentes construtores de cópia, inclusive a rota documental de UI em `wt-ui`, sem obra reconhecida no prompt: passam a negar. `CLAUDE.md:306`, `CLAUDE.md:309`, `bin/disjuntor.py:1711`.
   - Wrapper construtor com variáveis herdadas por `export`, sem `CODEX_OBRA` escrita na invocação: passa a negar no hook, independentemente do brief. `bin/disjuntor.py:1371`, `bin/disjuntor.py:1692`.
   - Wrapper chamado por script/`bash -c`: a via executada passa pelo novo `--censo`, ainda que o hook não consiga interpretar o shell. `bin/disjuntor.py:1281`, `bin/codex_pergunta.sh:230`.
   - A sessão principal **não é isenta** quando abre Agent ou chama wrapper: a saída antecipada por ausência de `agent_id` exclui expressamente essas ferramentas. `bin/disjuntor.py:1673`.
   - O tipo `verificador` declarado na configuração de agente é isento desse ramo de censo. `.claude/agents/verificador.md:2`, `bin/disjuntor.py:102`.
   
   Não há no dado fornecido nomes/transcripts de chamadas atualmente vivas que permitam identificar quais efetivamente cairão nessas condições: **[nao verificado]**. Os exemplos concretos de export e Agent sem obra são fixtures, não prova de produção. `bin/tests/test_censo_antes_da_vaga.sh:174`, `bin/tests/test_censo_antes_da_vaga.sh:178`.

9. **OBSERVACAO — Q5 está curada na expressão do wrapper.** `python3 -I ... --censo "$OBRA" || _recusa2` recusa qualquer rc diferente de zero; `_recusa2` termina com rc 2. Portanto Python com traceback/rc 1 recusa; o `flock` só ocorre depois. `bin/codex_pergunta.sh:183`, `bin/codex_pergunta.sh:230`, `bin/codex_pergunta.sh:236`. O selo mantém a vaga presa e exige recusa por censo, provando a ordem pretendida pelo teste. `bin/tests/test_codex_pergunta.sh:210`.

10. **OBSERVACAO — pontos abertos 5, 8 e 9.**
    - **5: risco declarado; desligamento comum não cobre a CLI.** `main_censo` não consulta `desligado`; o PreToolUse consulta. O texto da configuração enumera B1–B4/B4-CODEX/ANEXO-ENXUTO como escopo da chave, sem incluir censo. `bin/disjuntor.py:1756`, `bin/disjuntor.py:2069`, `bin/disjuntor_settings.json:3`. Exigência de que essa mesma chave desligue I1: **[nao verificado]**.
    - **8: restrição de formato declarada.** O juiz exige literalmente `? cortada por Ronald <token>` no fim; texto posterior deixa de contar, embora contenha a expressão de corte. `bin/disjuntor.py:1454`, `bin/disjuntor.py:1512`. A triagem fornecida pede ancoragem; autorização de todos os formatos rejeitados: **[nao verificado]**.
    - **9: ajuste coerente da âncora.** A mutação continua removendo `bash|sh` do ramo pertinente; `env` passou a ramo separado com atribuições. `bin/disjuntor.py:1288`, `bin/tests/test_disjuntor.sh:1741`. Execução verde do selo nesta auditoria: **[nao verificado]**.

## 09/11 03:10 · sessao 4358b47b3955 · FIO-E0-V4-E2 · construtor · origem=Code

Contrato E2 aplicado em `chamados/veredito_lavrado.py:119`, `:141`, `:213` e `chamados/management/commands/backfill_veredito_lavrado.py:111`. **Resta o ACHADO da catraca**, conforme a condição imposta: `instancias_do_turno` não monta universo pela porta (`chamados/services/tarefa_do_dia.py:88`). Suíte, testes RED e morte dos mutantes: **[nao verificado]**.

Alterações por hunk:

| Arquivo:linha atual | Alteração |
|---|---|
| [chamados/veredito_lavrado.py:95](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/veredito_lavrado.py:95) | `_hoje(agora)` obrigatório; somente converte o instante; rejeita `None`. |
| `chamados/veredito_lavrado.py:107` | `relogio_virou` exige `agora`; conserva as duas comparações em dia local. |
| `chamados/veredito_lavrado.py:119` | `expira_em(data_evento, hoje)` obrigatório, com `hoje` do tipo `date`; cálculo começa em `max(hoje, data_evento)` em `:141`. |
| `chamados/veredito_lavrado.py:213` | `esta_stale(p, marcos, agora)` obrigatório; conserva rejeição de lavra futura em `:224`. |
| `chamados/veredito_lavrado.py:273` | `lavrar` fornece o dia local do mesmo instante gravado em `julgado_em`. |
| [backfill_veredito_lavrado.py:87](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/management/commands/backfill_veredito_lavrado.py:87) | Importa o conversor puro; lê um instante em `:94`, converte em `:95` e passa `hoje` em `:111`. |
| [chamados/services/raiox.py:10](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/services/raiox.py:10) | Varredura exige `agora`; saiu a leitura interna do relógio. Marcos, lavra futura e comparação local usam esse horizonte em `:38`, `:41`, `:43`. |
| [lavrar_perguntas_stale.py:59](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/management/commands/lavrar_perguntas_stale.py:59) | Command lê o relógio e entrega o horizonte à varredura em `:60`. |
| [chamados/tests/test_e1_fila.py:352](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/tests/test_e1_fila.py:352) | Migração do chamador indireto de `expira_em`; mutantes atualizados em `:377`, `:384`; demais chamadas migradas em `:413`, `:427`. |
| [test_selo_concordancia_varredura_stale.py:56](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/tests/test_selo_concordancia_varredura_stale.py:56) | Um horizonte compartilhado pela varredura e pelo leitor da lavra. |
| [test_selo_get_nao_escreve.py:72](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/tests/test_selo_get_nao_escreve.py:72) | Três chamadas entregam o instante explicitamente: `:72`, `:102`, `:116`. |
| [escala/tests/test_signal_ancora.py:107](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/escala/tests/test_signal_ancora.py:107) | Atualizada a âncora do mutante de lavra futura; varredura recebe instante com relógio envenenado em `:119`. |
| `escala/tests/test_signal_ancora.py:281` | Acrescentados seis RED e seis métodos de mutação para contratos, conversão local e passagem dos argumentos pelos commands. |
| [core/tests/test_medida_turno_do_par.py:313](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/core/tests/test_medida_turno_do_par.py:313) | Dois motivos individuais e registro `[1167, 1319]`. |

Inventário dos chamadores alterados, antes → depois:

| Função | Arquivo:linha anterior → atual | Migração |
|---|---|---|
| `_hoje` | `chamados/veredito_lavrado.py:114 → :116` | Já passava `agora`; mantido. |
| `_hoje` | `chamados/veredito_lavrado.py:137 → removida` | `expira_em` usa diretamente `hoje`, agora em `:141`. |
| `_hoje` | nova em `chamados/veredito_lavrado.py:273` | Converte `_agora` da lavra. |
| `_hoje` | nova em `chamados/management/commands/backfill_veredito_lavrado.py:95` | Converte `agora` do command. |
| `expira_em` | `chamados/veredito_lavrado.py:267 → :273` | `agora=_agora` → `_hoje(_agora)`. |
| `expira_em` | `chamados/management/commands/backfill_veredito_lavrado.py:111 → :111` | `agora=agora` → `hoje`. |
| `expira_em`, indireto por `fn` | `chamados/tests/test_e1_fila.py:352 → :352` | `fn(DIA)` → `fn(DIA, timezone.localdate(timezone.now()))`. |
| `esta_stale` | `chamados/tests/test_e1_fila.py:412 → :413` | Acrescentado `marcos=None` explícito. |
| `relogio_virou` | `chamados/veredito_lavrado.py:222 → :228` | Já passava `agora`; mantido. |
| `relogio_virou` | `chamados/services/raiox.py:41 → :43` | Recebe agora o horizonte originado no command. |
| `pks_stale_por_varredura` | `chamados/management/commands/lavrar_perguntas_stale.py:58 → :60` | Acrescentado `agora`. |
| mesma | `chamados/tests/test_e1_fila.py:426 → :427` | Acrescentado `agora`. |
| mesma | `chamados/tests/test_selo_concordancia_varredura_stale.py:56 → :57` | Acrescentado o instante compartilhado. |
| mesma | `chamados/tests/test_selo_get_nao_escreve.py:72 → :72`, `:102 → :102`, `:116 → :116` | Relógio fornecido pelo teste chamador. |
| mesma, indireto por `fn` | `escala/tests/test_signal_ancora.py:121 → :121` | Acrescentado `self.AGORA`; relógio interno envenenado. |

Demais chamadas diretas, já compatíveis, antes → depois:

| Função | Chamadores preservados |
|---|---|
| `expira_em` | `chamados/tests/test_selo_lavrado_orfaos.py:60→60`, `:61→61`, `:85→85`, `:86→86`, `:97→97`; `chamados/tests/test_e2_tarefa_do_dia.py:686→686`. |
| `esta_stale` | `chamados/services/tarefa_do_dia.py:349→349`; `chamados/management/commands/auditar_invariantes_chamados.py:1167→1167`; `chamados/tests/test_e2_tarefa_do_dia.py:685→685`; `chamados/tests/test_porta_veredito_pergunta.py:270→270`, `:275→275`; `chamados/tests/test_selo_concordancia_varredura_stale.py:60→60`; `chamados/tests/test_selo_lavrado_orfaos.py:103→103`, `:115→115`, `:118→118`, `:128→128`, `:130→130`. |

As omissões em `chamados/tests/test_e2_tarefa_do_dia.py:696` e `:698` permanecem: são precisamente os testes que exigem `TypeError`. Os novos leitores indiretos dos contratos e das funções estão em `escala/tests/test_signal_ancora.py:294`, `:303`, `:317`, `:334`, `:353`, `:375`, `:408`.

Outros consumidores de `lavrar`: `chamados/models.py:312`, `:3007`, `:3150`; `chamados/services/fio.py:923`, `:1033`; `chamados/services/relavra_via.py:126`; `chamados/management/commands/lavrar_perguntas_stale.py:89`. Nos testes: `chamados/tests/test_e1_fila.py:372`; `chamados/tests/test_porta_veredito_pergunta.py:271`, `:286`, `:292`, `:318`, `:322`; `chamados/tests/test_selo_teto_placar.py:97`; `chamados/tests/test_profile_placar_rodinha.py:159`.

Consumidores dos commands: backfill em `chamados/tests/test_porta_veredito_pergunta.py:502`, `:509`; lavra stale em `chamados/tests/test_selo_cron_lavrar_stale.py:45` e `chamados/tests/test_selo_get_nao_escreve.py:115`; novos exercícios diretos em `escala/tests/test_signal_ancora.py:395`, `:423`.

Mantive `_dia_de_turno_da_pergunta` na tranca (`chamados/juizes.py:949`). Ele devolve `data_evento` para perguntas de fato e `None` para conversa (`chamados/juizes.py:332`). Os confrontos E2 consultados usam perguntas de fato (`chamados/tests/test_e2_tarefa_do_dia.py:1127`, `:1198`, `:1208`); passagem desses testes: **[nao verificado]**.

Os registros deslocados continuam apontando para `_julgar_colab_corpo`: conversão da adesão em `ponto/services/cartorio.py:1167` e recorte das batidas em `:1319`. Os motivos individuais ficaram em `core/tests/test_medida_turno_do_par.py:313` e `:314`. Igualdade textual com o fonte histórico indisponível de `2ea1f4c3`: **[nao verificado]**.

**ACHADO catraca: chamados/services/tarefa_do_dia.py:88.** A função recebe marcos prontos e converte a data civil em `:99`; não chama `parear_turnos` nem monta universo pela porta. Não acrescentei sua entrada a `CLASSIFICAR` (`core/tests/test_medida_turno_do_par.py:344`).

Os sete testes do anexo ficam assim; passagem efetiva de cada um: **[nao verificado]**. [nao verificado]

| Teste | Motivo tratado ou pendência | [nao verificado]
|---|---|
| `test_a_tarefa_e_a_lavra_lidas_sem_relogio` — `chamados/tests/test_e2_tarefa_do_dia.py:667` | `date` entra diretamente em `expira_em`; `_hoje` somente converte instante explícito (`chamados/veredito_lavrado.py:104`, `:141`). |
| `test_a_VESPERA_e_LEI_o_ultimo_dia_da_competencia_vive_UMA_a_mais` — `chamados/tests/test_selo_lavrado_orfaos.py:69` | Corrigido o contrato de entrada; preservado o limite com `- timedelta(days=1)` (`chamados/veredito_lavrado.py:145`). |
| `test_expira_em_devolve_FIM_DE_COMPETENCIA_nao_prazo_fixo` — `chamados/tests/test_selo_lavrado_orfaos.py:42` | Corrigido o contrato de entrada; preservado avanço pelo fim da competência (`chamados/veredito_lavrado.py:142`, `:147`). |
| `test_esta_stale_exige_agora` — `chamados/tests/test_e2_tarefa_do_dia.py:692` | Assinaturas obrigatórias em `chamados/veredito_lavrado.py:119`, `:213`. |
| `test_tarefa_e_leitores_da_lavra_nao_leem_o_relogio` — `chamados/tests/test_e2_tarefa_do_dia.py:1009` | Removida a leitura interna de `_hoje`; leitura da lavra permanece na borda declarada (`chamados/veredito_lavrado.py:104`, `:271`). |
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` — agora `core/tests/test_medida_turno_do_par.py:535` | Registro ajustado em `core/tests/test_medida_turno_do_par.py:315`. |
| `test_a_classificar_publicado_sem_entrar_como_criterio_amplo` — agora `core/tests/test_medida_turno_do_par.py:541` | **Pendência mantida pelo ACHADO acima. Não afirmo que passa.** |

Comandos individuais para o Code, não executados aqui. Os seis RED e seis métodos MORDE novos estão em `escala/tests/test_signal_ancora.py:294`, `:303`, `:324`, `:328`, `:343`, `:347`, `:363`, `:367`, `:395`, `:399`, `:423`, `:427`:

```bash
r() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test \
    --settings=config.settings_test "$1"
}
c() { r "escala.tests.test_signal_ancora.RebaseE2ContratoRelogioTest.$1"; }

c test_RED_argumentos_obrigatorios
c test_MORDE_defaults_opcionais
c test_RED_hoje_so_converte_instante
c test_MORDE_hoje_le_relogio
c test_RED_expira_recebe_date_sem_relogio
c test_MORDE_expira_rele_relogio
c test_RED_relogio_virou_preserva_dois_dias_locais
c test_MORDE_dia_UTC_em_cada_lado
c test_RED_backfill_passa_date_do_mesmo_instante
c test_MORDE_backfill_instante_ou_segundo_relogio
c test_RED_command_stale_entrega_horizonte
c test_MORDE_command_omite_horizonte
```

Comandos dos exercícios existentes afetados pela migração (`escala/tests/test_signal_ancora.py:101`, `:105`, `:124`, `:128`, `:134`; `chamados/tests/test_e1_fila.py:354`, `:370`, `:375`, `:382`, `:409`, `:420`):

```bash
l() { r "escala.tests.test_signal_ancora.RebaseE2LeitoresTest.$1"; }

l test_RED_lavra_futura
l test_MORDE_lavra_futura
l test_RED_varredura_no_mesmo_horizonte
l test_MORDE_varredura_sem_corte
l test_MORDE_varredura_aceita_lavra_futura

r chamados.tests.test_e1_fila.F5RelogioDaBordaTest.test_RED_expira_em_le_o_relogio_da_borda
r chamados.tests.test_e1_fila.F5RelogioDaBordaTest.test_a_lavra_e_a_janela_leem_o_MESMO_instante
r chamados.tests.test_e1_fila.F5RelogioDaBordaTest.test_MORDE_duas_leituras_do_relogio_na_lavra
r chamados.tests.test_e1_fila.F5RelogioDaBordaTest.test_MORDE_a_volta_do_date_today
r chamados.tests.test_e1_fila.DiaLocalEstaStaleTest.test_RED_esta_stale_le_o_dia_local
r chamados.tests.test_e1_fila.DiaLocalVarreduraTest.test_RED_a_varredura_le_o_dia_local
```

`py_compile` concluiu com rc=0 nos nove arquivos alterados listados acima. Comando executado, com escrita de bytecode bloqueada:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import py_compile
import subprocess
from unittest.mock import patch

arquivos = subprocess.check_output(
    ['git', 'diff', '--name-only', '--relative'], text=True
).splitlines()

with patch('py_compile.os.makedirs'), \
     patch('py_compile.importlib._bootstrap_external._write_atomic'):
    for arquivo in arquivos:
        if arquivo.endswith('.py'):
            py_compile.compile(arquivo, doraise=True)
            print('OK:', arquivo)
PY
```

## 09/11 03:09 · sessao 0c12bed4fd26 · - · - · origem=Ronald (tmux)

Q4 — cadeia e efeitos, somente leitura:

- `_match_marcos` é o ponto comum de alinhamento; recebe papel de `_papeis_do_turno` quando há `turno=`, ou de `_papeis_por_alternancia` sem turno. O ramo de papéis gera células, órfãs e marca as batidas consumidas. (`app/escala/utils.py:585`, `app/escala/utils.py:596`, `app/escala/utils.py:613`, `app/escala/utils.py:620`)
- O builder por turno passa `turno=`; o single e o dry-run chamam com `derivar=False`. Assim, corrigir apenas `_papeis_do_turno` incide diretamente no builder por turno, enquanto esses outros caminhos conservam a DP. (`app/escala/utils.py:776`, `app/escala/utils.py:1650`, `app/escala/utils.py:1701`)
- Para apurar o realizado da montagem atual, o builder cria cópias em memória das células, entrega as lâmpadas e órfãs recém-montadas, chama `turnos_do_colab` com elas e depois `realizado_do_dia`; grava seu resultado no dict da grade. Essa é a passagem pela qual um novo alinhamento altera o intervalo reconhecido e o número da ata. (`app/escala/utils.py:1886`, `app/escala/utils.py:1892`, `app/escala/utils.py:1898`, `app/escala/utils.py:2045`, `app/escala/utils.py:2058`)
- O cartório carrega `batidas_apuraveis`, chama o builder e serializa lâmpadas, órfãs e `minutos_realizados` em `ata_do_dia`. (`app/ponto/services/cartorio.py:1121`, `app/ponto/services/cartorio.py:1126`, `app/ponto/services/cartorio.py:696`, `app/ponto/services/cartorio.py:706`)
- A Zona 5 do motor chama `_match_marcos` **sem** `turno=` sobre entradas/saídas dos períodos do dia, associa períodos aos marcos `hi`/`hfi` e aplica atraso/antecipada. Sua leitura direta usa `_papeis_por_alternancia`, portanto a troca da adjacência de `_papeis_do_turno` não basta, isoladamente, para afirmar mudança direta neste caminho. (`app/ponto/motor_calculo_v2.py:3060`, `app/ponto/motor_calculo_v2.py:3062`, `app/ponto/motor_calculo_v2.py:3064`, `app/ponto/motor_calculo_v2.py:3077`, `app/escala/utils.py:597`)
- `marco_ocupado_pela_batida` transcreve `_match_marcos`, sem turno, devolvendo rótulo/delta; o casamento com a batida usa HH:MM. (`app/escala/utils.py:395`, `app/escala/utils.py:427`, `app/escala/utils.py:429`)
- `regua_defesa.cumpriu_a_volta_do_intervalo` entrega apenas um período a esse leitor e exige `hfi` com delta dentro da tolerância; esse único período fica na DP, porque a derivação por alternância exige pelo menos quatro batidas. Motor e calculador consultam esse predicado para retirar o previsto de entrada de quem cumpriu a volta. (`app/escala/regua_defesa.py:489`, `app/escala/regua_defesa.py:491`, `app/escala/regua_defesa.py:493`, `app/escala/utils.py:485`, `app/escala/utils.py:489`, `app/ponto/motor_calculo_v2.py:2117`, `app/ponto/calculador/regras.py:251`)

Dinheiro e leitores:

- O realizado líquido subtrai `intervalos_do_turno`; a conversão para minutos acontece antes da subtração. (`app/ponto/turnos.py:740`, `app/ponto/turnos.py:756`, `app/ponto/turnos.py:758`, `app/ponto/turnos.py:767`)
- O motor também lê o papel da ata quando recebe células alimentadas, chama o mesmo pareador e transporta os intervalos pelo adaptador. Portanto o alinhamento pode chegar ao cálculo financeiro **pela ata**, além dos chamadores diretos de `_match_marcos`. (`app/ponto/motor_calculo_v2.py:495`, `app/ponto/motor_calculo_v2.py:500`, `app/ponto/motor_calculo_v2.py:503`, `app/ponto/motor_calculo_v2.py:530`, `app/ponto/adaptador_motor.py:44`)
- No 12x36, o motor recorta os intervalos na janela, calcula pausa real, subtrai da duração, calcula supressão com tolerância e grava minutos trabalhados/indenização. A tolerância de intrajornada é dez minutos na cópia. (`app/ponto/motor_calculo_v2.py:2764`, `app/ponto/motor_calculo_v2.py:2771`, `app/ponto/motor_calculo_v2.py:2774`, `app/ponto/motor_calculo_v2.py:2776`, `app/ponto/motor_calculo_v2.py:2784`, `app/ponto/motor_calculo_v2.py:2785`, `app/ponto/motor_calculo_v2.py:69`)
- `DiaPago` recebe horas trabalhadas e intrajornada indenizada dividindo os campos do período por 60. O espelho lê `realizado_h` da autoridade do realizado e `pago_h`/rubricas da lavratura, portanto os números podem divergir. (`app/ponto/services/dia_pago.py:178`, `app/ponto/services/dia_pago.py:186`, `app/ponto/services/espelho.py:783`, `app/ponto/services/espelho.py:799`, `app/ponto/services/espelho.py:811`, `app/ponto/services/espelho.py:813`)
- O leitor da célula transporta o realizado da ata; o PDF de validação soma e divide por 60. Causas, classificação, comparação ata/juiz e calendário também o leem. (`app/escala/services/leitor_celula.py:369`, `app/escala/services/leitor_celula.py:406`, `app/folha/validacao_pdf.py:48`, `app/folha/validacao_pdf.py:54`, `app/folha/causas.py:85`, `app/ponto/supra_juiz.py:224`, `app/ponto/services/bordas_realizado.py:87`, `app/colaboradores/services/calendario.py:359`)
- Fechamento e percentual usam realizado limitado ao previsto de cada dia; a mesma diferença pode desaparecer no teto ou afetar cumprimento/abono. (`app/ponto/services/dia_pago.py:394`, `app/ponto/services/dia_pago.py:395`, `app/ponto/services/fechamento.py:1061`, `app/ponto/services/fechamento.py:1064`, `app/folha/export.py:1096`, `app/folha/export.py:1103`)

No dado raspado, antes→rejulgado: ata **660→599 min**, pausa descontada pelo motor **58,0130167→119,6599667 min**; indenização intrajornada **0→0**; pagamento já lavrado **10,87 h→10,87 h**, com rubricas exibidas idênticas. São evidências do texto fornecido, sem arquivo local/linha: **[não verificado]**. O segundo `minutos_trabalhados` está raspado, e o dado não prova novo pagamento lavrado nem valor em reais: **[não verificado]**.

_3014 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
