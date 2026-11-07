# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 18:13** · publicado em **07/11/2026 18:17** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 18:13 · sessao 6364bb5ec91d · MOTOR-LE-O-TURNO-INSTRUMENTO · censo · origem=Code

As referências aos anexos contam linhas desde o início de cada bloco fornecido. A confirmação dos commits posteriores nesta cópia-base é [nao verificado]. [nao verificado]

1. **O1 — portas novas fora das funções fixas**

   **Pergunta:** “O instrumento cumpre o criterio LITERAL? Algum item conta algo fora de A1/B1/nomeados, ou deixa de contar?”; “Cite algum mutante do instrumento que passaria em todos os testes.” (`v1.q:7`, `v1.q:10`) [nao verificado]

   **Resposta:** diagnóstico das novas chamadas M1/M6: **ausente**. O trecho correlato trata outra forma de pareador novo: “Um `range(..., 2)` novo sobe contador e dispara RED.” (`v1.r:34`; `triagem_opus1.txt:2`)

   **Triagem:** descarte específico de M1/M6: **ausente**. A avaliação geral foi: “1 NAO PROCEDE no criterio (9/85, 9/89 reproduzidos); a garantia incompleta e os itens 2 e 4.” (`triagem_v1.txt:2`)

   **Classe (a):** omissão de contagem e mordida estavam perguntadas; o Codex examinou o incremento por `range` novo e deixou passar o incremento por chamada nova à autoridade. (`v1.q:7`, `v1.q:10`; `v1.r:34`; `triagem_opus1.txt:2`)

2. **O2 — leitor host e publicação no ESTADO ausentes**

   **Pergunta:** cobrança explícita dos itens contratuais “leitor `bin/*_estado.py` + linha no ESTADO”: **ausente**. Houve a pergunta correlata: “QUEM ESCREVE e QUEM LE cada valor que o diff grava ou le”. (`v1.q:15`; `triagem_opus1.txt:3`)

   **Resposta:** diagnóstico da entrega contratual faltante: **ausente**. O Codex registrou apenas: “Publicação efetiva desses valores em placar/HOST fora da suite: **[nao verificado]**.” (`v1.r:104`) [nao verificado]

   **Triagem:** descarte explícito da obrigação contratual: **ausente**. Sobre o inventário de leitores: “8 tabela recebida, sem achado.” (`triagem_v1.txt:9`)

   **Classe (d):** a obrigação específica de entregar a publicação host não foi perguntada nem diagnosticada; “publicação não verificada” não constitui esse achado. A restrição do construtor a `app/` não impedia a leitura pelo verificador. (`v1.q:15`; `v1.r:104`; `triagem_opus1.txt:3`; `CLAUDE.md:341`, `CLAUDE.md:351`)

3. **FILA — E2 troca quatro identidades B1 no rebase, conservando o total**

   **Pergunta:** sobre essa troca concreta: **ausente**. A pergunta semelhante era sobre outro registro: “COMPOSICAO_REGISTRADA congela a lista inteira? Uma troca de identidade com o mesmo tamanho e cobrada?” (`v2.q:6`)

   **Resposta:** identificação dos quatro B1 trocados: **ausente**. A proteção geral de identidade foi reconhecida: “A igualdade percorre todos os itens dos dois contadores: A1, B1 e os três nomeados, com identidade completa e listas inteiras.” (`v3.r:154`)

   **Triagem:** descarte anterior desse evento: **ausente**. O enquadramento final diz: “ao rebasear, 4 itens B1 trocam de identidade com total igual -- a E2 classifica, nao copia”. (`triagem_opus1.txt:4`)

   **Classe (c), quanto ao evento concreto:** faltam as quatro identidades e as árvores comparadas; a classe geral de troca de identidade já estava reconhecida, portanto não cabe afirmar modo-comum sobre ela. (`v3.r:154`; `triagem_opus1.txt:4`; identidades concretas: [nao verificado])

4. **FILA — LINHAS_REGISTRADAS absolutas**

   **Pergunta:** “Linha mudada por edicao NAO relacionada do motor reprova (custo esperado) e a mensagem diz o que fazer?” (`v3.q:7`)

   **Resposta:** “**PROCEDENTE o custo de deslocamento:** uma edição sem relação com o negócio reprova se deslocar ocorrências registradas.” (`v3.r:156`)

   **Triagem:** “custo de deslocamento e o esperado e a mensagem diz o que fazer.” O opus manteve: “LINHAS_REGISTRADAS absolutas (custo de vermelho falso, nunca verde falso)”. (`triagem_v3.txt:7`; `triagem_opus1.txt:4`) [nao verificado]

   **Classe (b):** o Codex viu o custo e a triagem o aceitou expressamente como esperado, sem tratá-lo como bloqueador. (`v3.r:156`; `triagem_v3.txt:7`)

5. **FILA — M2, M4 e M5 declarados no NAO_VE**

   **Pergunta:** referência individual a M2, M4 ou M5: **ausente**. O tema correlato estava presente: “Distinga o que o NAO_VE declara do que some calado.” (`v3.q:6`)

   **Resposta:** identificação individual desses mutantes: **ausente**. Sobre os limites examinados, o Codex escreveu: “**Não enumera especificamente `for-target`, `with-as` e chaves de `dict` como buracos do gap.**” (`v3.r:152`)

   **Triagem:** “Q5 PROCEDENTE (b)+(d) TEXTO”; a mesma linha registra que a perda da parcela continua cobrada pela catraca numérica e por `LINHAS_REGISTRADAS`. A vinculação disso aos nomes M2/M4/M5 não aparece; o opus fornece somente “M2/M4/M5 declarados no NAO_VE”. (`triagem_v3.txt:6`; `triagem_opus1.txt:4`)

   **Classes individuais — M2: [nao verificado]; M4: [nao verificado]; M5: [nao verificado].** Sem os enunciados dos três mutantes, não há base para escolher entre descarte de achado já visto, classe (b), e limitação fora do alcance, classe (c), nem para associá-los aos exemplos de v3(b). (`triagem_opus1.txt:4`; `v3.r:152`)

## 07/11 18:11 · sessao 3de4b0f69961 · - · - · origem=Ronald (tmux)

Minha consolidação mais defensável:

- **O1 — (a).** O tema estava em v1.q: “Algum item conta algo fora de A1/B1/nomeados, ou deixa de contar?” e “Cite algum mutante do instrumento que passaria em todos os testes.” A resposta avaliou “Pareador novo” como “Um `range(..., 2)` novo”, sem diagnosticar nova chamada ao pareador fora das funções fixas. Não há descarte desse achado específico na triagem; ela não o recebeu. `[nao verificado]` [nao verificado]
- **O2 — (d), com distinção expressa.** A entrega contratual do leitor host não foi perguntada explicitamente nem diagnosticada como faltante. A resposta “Publicação efetiva desses valores em placar/HOST fora da suite: **[nao verificado]**” registra uma incerteza, não o achado O2; portanto não basta para atribuir (b). “bin/ fora do alcance do construtor” também não demonstra impossibilidade do verificador. `[nao verificado]` [nao verificado]
- **E2, quatro identidades B1 no rebase — (c) para o evento concreto.** As quatro identidades e as árvores comparadas não constam do dado. v2.q(b) pergunta identidade da composição; v3.r(c) já reconhece “identidade completa” nos registros dos contadores. Não afirmar que ninguém viu a classe geral; não afirmar que o rebase ocorreu depois das verificações. `[nao verificado]`
- **LINHAS_REGISTRADAS absolutas — (b).** v3.q(c): “Linha mudada por edicao NAO relacionada do motor reprova (custo esperado) e a mensagem diz o que fazer?” v3.r(c): “PROCEDENTE o custo de deslocamento”. Triagem: “custo de deslocamento e o esperado e a mensagem diz o que fazer.” Foi visto e aceito como custo esperado, não falha bloqueadora. `[nao verificado]`
- **M2/M4/M5 — correspondência individual indeterminada.** O dado só informa “M2/M4/M5 declarados no NAO_VE”; não fornece suas definições. **(c) apenas conforme esse enquadramento do opus**, sem confirmar que sejam os escapes de v3(b). Mapear M2/M4/M5 a `for`, `with`, `setattr` etc. inventaria informação. `[nao verificado]`

Para O2, se usar (d), explique que o modo comum é **a ausência da entrega host como diagnóstico contratual**, apesar da menção à publicação não verificada. `[nao verificado]`

## 07/11 18:11 · sessao ee5afc1cc41e · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

O1 corrigido em [core/medida_turno_do_par.py:414](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:414): cada chamada resolvida vira uma porta, independentemente do nome da função; chamadas na composição ficam excluídas. A herança por gap continua separada em `core/medida_turno_do_par.py:422`.

Antes da edição, a medição encontrou somente:

| Chave | Linha | Sítio |
|---|---:|---|
| `ponto/motor_calculo_v2.py::turnos_via_autoridade` | 503 | `porta_parear_turnos` |
| `ponto/motor_calculo_v2.py::MotorBase._turnos_do_juiz` | 882 | `porta_turnos_de_batidas` |

Fontes: `ponto/motor_calculo_v2.py:503` e `:882`.

`DiaPago` está definido em `ponto/models.py:281`; a lavratura o importa em `ponto/services/dia_pago.py:163`. O escopo inclui também grade (`dia_pago.py:443` → `folha/export.py`), fechamento (`dia_pago.py:449` → `ponto/services/fechamento.py`) e janelas (`dia_pago.py:713` → `ponto/janelas.py`). A declaração completa está em `core/medida_turno_do_par.py:75`.

Resultados locais do runner de [core/tests/test_medida_turno_do_par.py:19](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:19):

- **Antes da cura:** M1 e M6 deram RED por `AssertionError: 10 != 9`; M7 passou. Casos em `core/tests/test_medida_turno_do_par.py:759`, `:762` e `:766`.
- **Depois:** original **rc=0, 35 testes OK**. M1/M6 exigem **10/86**; M7 preserva **9/85** (`core/tests/test_medida_turno_do_par.py:752` e `:780`).
- **13/13 mutantes: rc=1 por asserção, sem erros de execução**, usando os comandos de `core/tests/test_medida_turno_do_par.py:73`.
- **9/85, REGISTRO e LINHAS_REGISTRADAS preservados**, cobrados em `core/tests/test_medida_turno_do_par.py:552` e `:568`.

Para reproduzir sem arquivos auxiliares, em `app/`:

```bash
rodar_turno() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B - "$@" <<'PY'
import os, pathlib, shlex, sys
p = pathlib.Path('core/tests/test_medida_turno_do_par.py')
source = p.read_text()
runner = source.split("<<'PY'\n", 1)[1].split('\n  PY', 1)[0]
runner = '\n'.join(line[2:] for line in runner.splitlines())
mutant = sys.argv[1] if len(sys.argv) > 1 else ''
os.environ.pop('MUTANTE', None)
sys.argv = [str(p)]
if mutant:
    command = next(line for line in source.splitlines()
                   if line.startswith('  MUTANTE=' + mutant + ' '))
    os.environ['MUTANTE'] = mutant
    sys.argv.append(shlex.split(command)[-1])
exec(compile(runner, '<runner-docstring>', 'exec'))
PY
}

rodar_turno                       # original: rc=0
rodar_turno composicao            # rc=1
rodar_turno linhas                # rc=1
rodar_turno gap                   # rc=1
rodar_turno classificar           # rc=1
rodar_turno P1-linha-unica        # rc=1
rodar_turno P2-len1               # rc=1
rodar_turno Q1-lines10            # rc=1
rodar_turno Q2-len2               # rc=1
rodar_turno Q3-tuple              # rc=1
rodar_turno Q4-self               # rc=1
rodar_turno O1-funcoes-fixas      # rc=1: 10 != 9
rodar_turno O1-ancoras-motor      # rc=1: 10 != 9
rodar_turno O1-sem-composicao     # rc=1: 9 != 10
```

Não há campos novos na saída; os itens reutilizam `chave`, `criterio`, `sitio` e `linhas` (`core/medida_turno_do_par.py:420`).

| Dado acrescentado/reutilizado | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `_ARQUIVOS_DIAPAGO` | `core/medida_turno_do_par.py:75` | mesmo arquivo`:416` |
| `PORTAS_NOMEADAS` | `core/medida_turno_do_par.py:82` | mesmo arquivo`:414` |
| Item da porta: quatro campos existentes | `core/medida_turno_do_par.py:420` | `core/tests/test_medida_turno_do_par.py:753`, `:562`, `:569` |
| Contadores com essas portas | `core/medida_turno_do_par.py:435` | `core/tests/test_medida_turno_do_par.py:752` |

Certificação pelo Code e opus final: **[nao verificado]**.

## 07/11 17:34 · sessao 9e854daaf1c8 · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

Q1–Q4 acrescentados: [11 ocorrências](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:606), [três desconhecidas](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:635), [Tuple interna](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:654) e [outro receptor](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:685). Q5 corrigido no [NAO_VE](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:52) e no [docstring de `_gap`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:257).

A comparação AST antes/depois confirmou detector, catraca e registros intactos; execução e comparação ficaram somente em memória: [nao verificado].

Os resultados abaixo foram observados localmente, sem artefato histórico persistido nem certificação: [nao verificado]. Cada link aponta o comando exato; todos usam o heredoc do [runner](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:21).

| Execução / comando | Resultado |
|---|---|
| [Original](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:21) | rc=0; 28 testes OK | [nao verificado]
| [`MUTANTE=composicao`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:66) | rc=1; 1 falha por asserção |
| [`MUTANTE=linhas`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:67) | rc=1; 3 falhas por asserção |
| [`MUTANTE=gap`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:68) | rc=1; 9 falhas por asserção |
| [`MUTANTE=classificar`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:69) | rc=1; 1 falha por asserção |
| [`MUTANTE=P1-linha-unica`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:70) | rc=1; 1 falha por asserção |
| [`MUTANTE=P2-len1`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:71) | rc=1; 2 falhas por asserção |
| [`MUTANTE=Q1-lines10`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:72) | rc=1; falta a linha 12 |
| [`MUTANTE=Q2-len2`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:73) | rc=1; `LookupError not raised` |
| [`MUTANTE=Q3-tuple`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:74) | rc=1; asserção contra falsa herança |
| [`MUTANTE=Q4-self`](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:75) | rc=1; `LookupError not raised` |

## 07/11 17:19 · sessao 6f9c34fbaa6e · MOTOR-LE-O-TURNO-INSTRUMENTO · verificador · origem=Code

**As curas corrigem os contraexemplos da triagem, mas a proteção varia: P1 congela integralmente a árvore atual; P2/P3 têm regras gerais no código com cobertura ainda parcial; P4 mantém escapes; P5 remove o relato histórico, sem garantir documentação futura.** (`app/core/tests/test_medida_turno_do_par.py:546`, `app/core/medida_turno_do_par.py:259`, `app/core/medida_turno_do_par.py:286`, `app/core/medida_turno_do_par.py:316`, `app/core/tests/test_medida_turno_do_par.py:55`)

O histórico `133cd78a..35969e9d` não pôde ser conferido: a cópia não contém `.git`. O confronto abaixo usa a fonte atual e a triagem fornecida. **[nao verificado]**

Executei o original e cada mutante isoladamente, em memória: **original 28/28; P1–P5 28/28**. Conferi também as asserções HOST sobre cada fonte mutante. São os testes deste instrumento, pelo runner adaptado de `SimpleTestCase` para `unittest`, sem execução Django/produtiva; resultados sem artefato persistido: **[nao verificado]**. O procedimento está documentado em `app/core/tests/test_medida_turno_do_par.py:20`.

**P1 — fecha as linhas da árvore atual; não fecha a generalização para outras cardinalidades.** Agora há igualdade integral entre registro e publicação, incluindo A1, B1 e nomeados. O mutante anterior “usar `def` quando houver uma ocorrência” enfrenta essa igualdade. (`app/core/tests/test_medida_turno_do_par.py:227`, `app/core/tests/test_medida_turno_do_par.py:546`)

Mutante sobrevivente: substituir exatamente este fragmento em `app/core/medida_turno_do_par.py:389`:

```python
'criterio': criterion, 'linhas': lines
```

por:

```python
'criterio': criterion, 'linhas': lines[:10]
```

O maior registro atual tem dez ocorrências; a fixture específica cobra duas. Assim, o mutante preserva a árvore registrada e trunca uma função futura com onze ocorrências. (`app/core/tests/test_medida_turno_do_par.py:325`, `app/core/tests/test_medida_turno_do_par.py:587`)

Teste que deveria pegá-lo, ampliando `test_MORDE_linhas_a1_b1_publicam_ocorrencia_e_nao_def`, em `app/core/tests/test_medida_turno_do_par.py:570`:

```python
sources = _mini()
sources['novo.py'] = 'def novo(entrada):\n' + '    entrada.date()\n' * 11
result = m.censo_fontes(sources)
item = next(i for i in result['derivacoes_de_dia']['itens']
            if i['chave'] == 'novo.py::novo')
self.assertEqual(list(range(2, 13)), item['linhas'])
```

**P2 — o código fecha a classe “qualquer quantidade de desconhecidas”; a suíte acrescentou somente o caso de duas.** A diferença de conjuntos continua correta, mas ainda pode regredir para uma guarda limitada sem os testes perceberem. (`app/core/medida_turno_do_par.py:316`, `app/core/tests/test_medida_turno_do_par.py:616`)

Mutante sobrevivente: substituir a guarda em `app/core/medida_turno_do_par.py:316` por:

```python
if 0 < len(inherited - recognized) <= 2:
```

Teste que deveria pegá-lo, estendendo `app/core/tests/test_medida_turno_do_par.py:616`:

```python
sources = _mini()
sources[m._MOTOR] += '        data_jornada = _prev_dj\n' * 3
with self.assertRaisesRegex(LookupError, r'linhas \[12, 13, 14\]'):
    m.censo_fontes(sources)
```

O original aponta as três linhas; o mutante deixa passar e mantém somente a ocorrência reconhecida da fixture. Resultado observado em memória: **[nao verificado]**.

**P3 — fecha o casamento recursivo de Tuple/List sem `starred` na implementação; a suíte não protege todas essas formas.** O falso cruzamento original foi corrigido por associação posicional recursiva. A recursão ensaiada usa lista interna; falta a variante com tupla interna. (`app/core/medida_turno_do_par.py:259`, `app/core/tests/test_medida_turno_do_par.py:626`)

Mutante sobrevivente: substituir a linha de `app/core/medida_turno_do_par.py:265` por:

```python
yield from ([(left, right)] if isinstance(left, ast.Tuple) else bindings(left, right))
```

Teste que deveria pegá-lo, ampliando `app/core/tests/test_medida_turno_do_par.py:626`:

```python
sources = _mini()
sources[m._MOTOR] += (
    '        outro, (data_jornada, ultimo) = (_prev_dj, (x, _prev_dj))\n'
)
result = m.censo_fontes(sources)
self.assertEqual([11], result['sitios_nomeados'][-1]['linhas'])
```

Aqui `data_jornada` recebe `x`. O original aceita; o mutante mistura as posições da tupla interna e lança `LookupError`. Resultado observado em memória: **[nao verificado]**.

**P4 — amplia a detecção, mas não fecha a classe de fontes, destinos e bindings.** Alias transitivo, atributo de origem e subscrito literal de destino ganharam implementação e testes; outros bindings continuam ausentes. (`app/core/medida_turno_do_par.py:269`, `app/core/medida_turno_do_par.py:279`, `app/core/medida_turno_do_par.py:286`, `app/core/tests/test_medida_turno_do_par.py:654`)

Mutante sobrevivente: substituir o ramo de `Attribute` em `app/core/medida_turno_do_par.py:281` por:

```python
or (isinstance(n, ast.Attribute) and n.attr == '_prev_dj' and isinstance(n.value, ast.Name) and n.value.id == 'self')
```

A cobertura de origem por atributo usa `self._prev_dj`. Falta outro receptor. (`app/core/tests/test_medida_turno_do_par.py:657`, `app/core/tests/test_medida_turno_do_par.py:659`, `app/core/tests/test_medida_turno_do_par.py:662`)

Teste que deveria pegá-lo: [nao verificado]

```python
sources = _mini()
sources[m._MOTOR] += '        data_jornada = estado._prev_dj\n'
with self.assertRaisesRegex(LookupError, r'linhas \[12\]'):
    m.censo_fontes(sources)
```

**P5 — fecha o texto histórico apontado, não uma classe executável.** O docstring atual prescreve execuções que “devem” falhar; não conserva o antigo relato de autoria, duração e execuções concluídas. (`app/core/tests/test_medida_turno_do_par.py:55`, `app/core/tests/test_medida_turno_do_par.py:64`)

Para cumprir literalmente o pedido de mutante **do instrumento**, acrescente após a frase de `app/core/medida_turno_do_par.py:19` esta linha, ainda dentro do docstring:

```text
Registro da execucao: suite Django oficial aprovada em producao. [nao verificado]
```

Isso sobrevive porque as verificações HOST conferem execução/imports, sem autenticar essa alegação documental. A aprovação produtiva escrita pelo mutante é **[nao verificado]**. (`app/core/tests/test_medida_turno_do_par.py:826`)

Um teste documental como o seguinte pegaria **esse texto**, sem provar a veracidade geral da documentação: [nao verificado]

```python
self.assertNotIn('Registro da execucao:', m.__doc__)
```

A limitação é precisamente essa: P5 exige revisão de alegações e proveniência; um teste literal fecha apenas a frase escolhida. Autenticação geral dessas alegações pela suíte: **[nao verificado]**. [nao verificado]

**a) PROCEDENTE como possibilidade de contaminação; NAO PROCEDE como falha demonstrada no motor da cópia.** `aliases` só cresce, inclui `data_jornada` após a herança reconhecida e não elimina marcas após sobrescritas. Ordem, ramos e alcance são limitações expressamente declaradas. (`app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:277`, `app/core/medida_turno_do_par.py:290`)

Estes exemplos, acrescentados após a herança reconhecida, podem acusar herança na última atribuição sem que ela carregue `_prev_dj`; isso decorre de `source` consultar o conjunto acumulado. (`app/core/medida_turno_do_par.py:279`, `app/core/medida_turno_do_par.py:300`)

```python
data_jornada = x
data_jornada = data_jornada
```

```python
anterior = _prev_dj
anterior = x
data_jornada = anterior
```

```python
if cond:
    anterior = _prev_dj
else:
    anterior = x
    data_jornada = anterior
```

**Sim: `[2408]` é a única atribuição reconhecida no motor da cópia.** A origem inicial é `data_entrada`; a atribuição direta sob `_gap_h` está em `:2408`; depois ocorre `_prev_dj = data_jornada`. `recognized` exige literalmente `data_jornada = _prev_dj`, sem consultar os aliases. (`app/ponto/motor_calculo_v2.py:2403`, `app/ponto/motor_calculo_v2.py:2408`, `app/ponto/motor_calculo_v2.py:2409`, `app/core/medida_turno_do_par.py:312`)

A contaminação pode causar `LookupError` no **censo**; `_gap` recebe AST e é chamado pela instrumentação. Não há demonstração de parcela errada ou exceção no cálculo produtivo. (`app/core/medida_turno_do_par.py:329`, `app/core/medida_turno_do_par.py:399`; produção: **[nao verificado]**)

**b) PROCEDENTE — permanecem escapes, e nem todos estão declarados especificamente.**

| Forma | Veredito e comportamento | Evidência |
|---|---|---|
| `data_jornada, *outro = (x, _prev_dj)` | **NAO PROCEDE como escape:** acusa conservadoramente, embora o valor herdado vá para `outro`. | `app/core/medida_turno_do_par.py:259`; `app/core/tests/test_medida_turno_do_par.py:645` |
| `for data_jornada in [_prev_dj]: pass` | **PROCEDENTE:** `For` não entra em `assignments`. | `app/core/medida_turno_do_par.py:286` |
| `with cm(_prev_dj) as data_jornada: pass` | **PROCEDENTE:** `With` não entra em `assignments`. | `app/core/medida_turno_do_par.py:286` |
| `data_jornada = copiar(_prev_dj)` | **NAO PROCEDE como escape:** encontra a fonte dentro da chamada; já ensaiado. | `app/core/medida_turno_do_par.py:279`; `app/core/tests/test_medida_turno_do_par.py:599` |
| `setattr(self, 'data_jornada', _prev_dj)` | **PROCEDENTE:** chamada isolada não é atribuição selecionada. | `app/core/medida_turno_do_par.py:286` |
| `global`/`nonlocal` com atribuição direta no corpo examinado | **NAO PROCEDE como escape automático:** a declaração não esconde o `Assign`; alcance é ignorado. | `app/core/medida_turno_do_par.py:34`; `app/core/medida_turno_do_par.py:286` |
| Escritor em função aninhada usando `global`/`nonlocal` | **PROCEDENTE:** `_own` não percorre seu corpo; a chamada não propaga efeitos para `_gap`. | `app/core/medida_turno_do_par.py:70`; `app/core/medida_turno_do_par.py:284` |
| `dados = {'data_jornada': _prev_dj}` | **PROCEDENTE como destino omitido:** chave de `Dict` não é destino reconhecido. Pode marcar `dados` para usos posteriores. | `app/core/medida_turno_do_par.py:269`; `app/core/medida_turno_do_par.py:295` |
| `dados[chave] = _prev_dj` | **PROCEDENTE:** somente chave literal `'data_jornada'` é reconhecida. | `app/core/medida_turno_do_par.py:274` |

`NAO_VE` declara fluxo/alcance, origem interprocedural, chamadas dinâmicas e conservadorismo no desempacotamento. **Não enumera especificamente `for-target`, `with-as` e chaves de `dict` como buracos do gap.** (`app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:35`, `app/core/medida_turno_do_par.py:36`, `app/core/medida_turno_do_par.py:52`)

**c) NAO PROCEDE cobertura parcial de `LINHAS_REGISTRADAS`.** A igualdade percorre todos os itens dos dois contadores: A1, B1 e os três nomeados, com identidade completa e listas inteiras. (`app/core/tests/test_medida_turno_do_par.py:227`, `app/core/tests/test_medida_turno_do_par.py:355`, `app/core/tests/test_medida_turno_do_par.py:546`)

**PROCEDENTE o custo de deslocamento:** uma edição sem relação com o negócio reprova se deslocar ocorrências registradas. A mensagem é **“linhas mudaram: registrar no MESMO commit”**: informa a ação, embora não nomeie `LINHAS_REGISTRADAS`. Não é toda edição alheia que reprova; precisa alterar o retrato comparado. (`app/core/tests/test_medida_turno_do_par.py:17`, `app/core/tests/test_medida_turno_do_par.py:550`)

**d) NAO PROCEDE a permanência do antigo relato histórico; PROCEDENTE uma promessa comportamental excessiva.** “Alias dentro do if exige LookupError” não cobre, por exemplo, alias introduzido por `for`; a seleção não inclui esse binding. (`app/core/medida_turno_do_par.py:255`, `app/core/medida_turno_do_par.py:286`)

A reescrita abaixo pode retirar a parcela gap sem lançar a exceção prometida; a catraca numérica ainda cobra a queda quando aplicada. (`app/core/medida_turno_do_par.py:286`, `app/core/medida_turno_do_par.py:401`, `app/core/tests/test_medida_turno_do_par.py:497`)

```python
if 0 <= _gap_h <= 6:
    for anterior in [_prev_dj]:
        data_jornada = anterior
```

**FILA:** as referências históricas continuam qualificadas como informadas/anexadas. Sua proveniência e a contemporaneidade exigida por “MESMO commit” não são comprovadas pela suíte; o próprio texto explicita “sem consultar git”. (`app/core/tests/test_medida_turno_do_par.py:2`, `app/core/tests/test_medida_turno_do_par.py:5`, `app/core/tests/test_medida_turno_do_par.py:18`)

**e) Classes que já reprovaram a obra.**

| Classe | Veredito nesta revisão | Evidência |
|---|---|---|
| Segundo juiz de negócio | **NAO PROCEDE no instrumento:** identifica estruturas AST e publica medidas; não decide a jornada. | `app/core/medida_turno_do_par.py:329`, `app/core/medida_turno_do_par.py:416` |
| Segundo escritor fora da porta | **NAO PROCEDE no instrumento:** lê fontes e retorna estruturas. | `app/core/medida_turno_do_par.py:427` |
| Relógio fora da borda | **NAO PROCEDE no instrumento:** imports limitados a `ast`/`os`, conferidos pelo teste HOST. | `app/core/medida_turno_do_par.py:21`, `app/core/tests/test_medida_turno_do_par.py:834` |
| Derivação paralela de fonte única | **FILA:** portas e herança permanecem no motor; são objeto da medição. | `app/ponto/motor_calculo_v2.py:503`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:2408` |
| Selo/RED sem mordida | **PROCEDENTE quanto à cobertura incompleta**, pelos mutantes; **NAO PROCEDE como ausência total de mordida:** incrementos novos são cobrados. | `app/core/tests/test_medida_turno_do_par.py:690`, `app/core/tests/test_medida_turno_do_par.py:700`; sobrevivência: **[nao verificado]** |
| Campo com dois significados | **NAO PROCEDE como divergência de unidade demonstrada:** linhas representam ocorrências, contadores representam funções mais sítios. **FILA** para o rótulo “sem teto ou catraca” de `CLASSIFICAR`, cuja identidade pode reprovar por falta de registro. | `app/core/medida_turno_do_par.py:10`, `app/core/tests/test_medida_turno_do_par.py:12`, `app/core/tests/test_medida_turno_do_par.py:552` |

**f) QUEM ESCREVE / QUEM LÊ.** Inventário dos mecanismos relacionados às curas; exaustividade perante o delta histórico: **[nao verificado]**.

| Valor | Quem escreve | Quem lê |
|---|---|---|
| Pares posicionais de `bindings` | `app/core/medida_turno_do_par.py:264`, `:267` | Construção de atribuições em `app/core/medida_turno_do_par.py:289` |
| Resultado de `destination` | `app/core/medida_turno_do_par.py:270` | Seleção de herança em `app/core/medida_turno_do_par.py:300` |
| `aliases` | Semente em `app/core/medida_turno_do_par.py:277`; expansão em `:295` | `source` em `app/core/medida_turno_do_par.py:280`; convergência em `:296` |
| `assignments` | `app/core/medida_turno_do_par.py:289` | Propagação em `app/core/medida_turno_do_par.py:292`; seleção em `:299` |
| `inherited` | `app/core/medida_turno_do_par.py:301` | Guarda/diagnóstico em `app/core/medida_turno_do_par.py:316`, `:319` |
| `recognized` | `app/core/medida_turno_do_par.py:315` | Guarda e retorno em `app/core/medida_turno_do_par.py:316`, `:320` |
| Linhas lexicais A1/B1 | `app/core/medida_turno_do_par.py:194`, `:217`, `:389` | Contadores em `app/core/medida_turno_do_par.py:406`; igualdade em `app/core/tests/test_medida_turno_do_par.py:547` |
| Linhas nomeadas | `app/core/medida_turno_do_par.py:399`, `:403` | Contadores em `app/core/medida_turno_do_par.py:406`; igualdade em `app/core/tests/test_medida_turno_do_par.py:549` |
| `LINHAS_REGISTRADAS` | `app/core/tests/test_medida_turno_do_par.py:227` | `app/core/tests/test_medida_turno_do_par.py:547` |
| `NAO_VE` | `app/core/medida_turno_do_par.py:31` | Publicação em `app/core/medida_turno_do_par.py:408`, `:423`; igualdade em `app/core/tests/test_medida_turno_do_par.py:543` |
| Fontes/expectativas dos novos ensaios | `app/core/tests/test_medida_turno_do_par.py:618`, `:627`, `:643`, `:655`, `:677` | Chamadas e asserções em `app/core/tests/test_medida_turno_do_par.py:621`, `:637`, `:651`, `:670`, `:679` |
| Instruções de execução documental | `app/core/tests/test_medida_turno_do_par.py:55` | Autenticador automático de execução histórica: **[nao verificado]** |

## 07/11 17:14 · sessao a57c29d65425 · - · - · origem=Ronald (tmux)

**P1: fecha as linhas da árvore atual, não toda cardinalidade futura.** A igualdade compara todos os itens de ambos contadores, incluindo A1, B1 e os três nomeados; não verifica somente presença. (`app/core/tests/test_medida_turno_do_par.py:227`, `app/core/tests/test_medida_turno_do_par.py:355`, `app/core/tests/test_medida_turno_do_par.py:546`)

Mutante exato em `app/core/medida_turno_do_par.py:389`:

```python
# antes
'criterio': criterion, 'linhas': lines

# depois
'criterio': criterion, 'linhas': lines[:10]
```

Executado em memória: **28/28 passam**; contraprova com onze chamadas publica dez linhas. Resultado sem artefato persistido: **[nao verificado documentalmente]**. O maior registro atual tem dez ocorrências e a fixture específica cobra duas. (`app/core/tests/test_medida_turno_do_par.py:325`, `app/core/tests/test_medida_turno_do_par.py:587`)

Teste que deveria matá-lo, ampliando `test_MORDE_linhas_a1_b1_publicam_ocorrencia_e_nao_def` em `app/core/tests/test_medida_turno_do_par.py:570`:

```python
sources = _mini()
sources['novo.py'] = 'def novo(entrada):\n' + '    entrada.date()\n' * 11
result = m.censo_fontes(sources)
item = next(i for i in result['derivacoes_de_dia']['itens']
            if i['chave'] == 'novo.py::novo')
self.assertEqual(list(range(2, 13)), item['linhas'])
```

**c) NAO PROCEDE cobertura parcial.** A1 consta em `:227-232`, B1 em `:236-354`, os nomeados nos dois contadores em `:233-235` e `:355-357`; a compreensão não filtra critérios e compara o registro inteiro. (`app/core/tests/test_medida_turno_do_par.py:227`, `app/core/tests/test_medida_turno_do_par.py:355`, `app/core/tests/test_medida_turno_do_par.py:546`)

**c) PROCEDENTE custo de deslocamento.** Uma edição alheia ao negócio que acrescente linhas antes dos sítios registrados muda a igualdade e reprova; o próprio contrato anuncia essa cobrança. A mensagem diz **“linhas mudaram: registrar no MESMO commit”**: informa a ação, embora não nomeie `LINHAS_REGISTRADAS`. (`app/core/tests/test_medida_turno_do_par.py:17`, `app/core/tests/test_medida_turno_do_par.py:550`)

**P5: fecha a ocorrência documental apontada.** O antigo relato de autoria/data/duração/resultados externos foi removido; agora o texto prescreve seis execuções e afirma que **devem** falhar. Não declara que foram executadas. (`app/core/tests/test_medida_turno_do_par.py:55`, `app/core/tests/test_medida_turno_do_par.py:64`)

**d) NAO PROCEDE permanência daquele “Registro da execução”. FILA quanto à história externa.** Permanecem base informada e referência histórica anexada, cuja proveniência a suíte não certifica; são referências qualificadas, não resultados apresentados como medidos pela sessão atual. (`app/core/tests/test_medida_turno_do_par.py:2`, `app/core/tests/test_medida_turno_do_par.py:5`; proveniência: **[nao verificado]**)

Não existe mutante lógico do instrumento que seja contraexemplo pertinente à retirada de um relato do docstring dos testes; isso seria outra propriedade. Reintroduzir história no docstring sobreviveria porque não há leitor que autentique documentação, mas não demonstraria falha da correção executável de P1–P4. (`app/core/tests/test_medida_turno_do_par.py:2`, `app/core/tests/test_medida_turno_do_par.py:826`; autenticação documental: **[nao verificado]**)

**f), parcela sob minha revisão:**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `LINHAS_REGISTRADAS` | Constante registrada no teste | Igualdade de todos os itens | [nao verificado]
| | `app/core/tests/test_medida_turno_do_par.py:227` | `app/core/tests/test_medida_turno_do_par.py:547` |
| Linhas lexicais A1/B1 | `_scan`, depois publicação de `criteria` | Contadores e igualdade real |
| | `app/core/medida_turno_do_par.py:194`, `app/core/medida_turno_do_par.py:217`, `app/core/medida_turno_do_par.py:389` | `app/core/medida_turno_do_par.py:406`, `app/core/tests/test_medida_turno_do_par.py:549` |
| Linhas nomeadas | `_gap` ou chamadas resolvidas, depois `named` | Contadores e igualdade real |
| | `app/core/medida_turno_do_par.py:399`, `app/core/medida_turno_do_par.py:403` | `app/core/medida_turno_do_par.py:406`, `app/core/tests/test_medida_turno_do_par.py:549` |
| Fixture de linhas deslocadas | Fonte `novo.py`, prefixo de 0/40 linhas | Comparação exata dos sítios |
| | `app/core/tests/test_medida_turno_do_par.py:572`, `app/core/tests/test_medida_turno_do_par.py:584` | `app/core/tests/test_medida_turno_do_par.py:592` |
| Instruções RED externo | Texto do docstring | Execução humana; autenticador automático não identificado |
| | `app/core/tests/test_medida_turno_do_par.py:57` | **[nao verificado]** |

## 07/11 17:14 · sessao f6a7f5a9c8d2 · - · - · origem=Ronald (tmux)

**a) PROCEDENTE como limitação declarada; NAO PROCEDE como falha demonstrada no motor corrente.** O ponto fixo inclui `data_jornada` após a atribuição reconhecida e nunca remove aliases; ignora ordem, sobrescritas e ramos. Assim, exemplos abaixo levantam falso positivo, embora a última atribuição não herde `_prev_dj`. (`app/core/medida_turno_do_par.py:277`, `app/core/medida_turno_do_par.py:290`, `app/core/medida_turno_do_par.py:300`; declaração em `app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:55`)

```python
# Acrescentados após a forma reconhecida:
data_jornada = x
data_jornada = data_jornada
```

```python
anterior = _prev_dj
anterior = x
data_jornada = anterior
```

```python
if cond:
    anterior = _prev_dj
else:
    anterior = x
    data_jornada = anterior
```

Executei `_gap` sobre a AST viva de `MotorBase.calcular_mes` e obtive `[2408]`; resultado sem artefato persistido: **[nao verificado documentalmente]**. A fonte possui exatamente uma atribuição direta `data_jornada = _prev_dj`, sob comparação de `_gap_h`; `recognized` exige essa forma literal e não consulta aliases. A contaminação não cria outra parcela reconhecida no motor disponível. (`app/ponto/motor_calculo_v2.py:2404`, `app/ponto/motor_calculo_v2.py:2407`, `app/ponto/motor_calculo_v2.py:2408`, `app/ponto/motor_calculo_v2.py:2409`; `app/core/medida_turno_do_par.py:312`)

**b) PROCEDENTE quanto a escapes restantes.**

| Forma | Resultado estrutural | Evidência |
|---|---|---|
| `data_jornada, *outro = (x, _prev_dj)` | Capturada conservadoramente, mesmo sem herança para `data_jornada` | `app/core/medida_turno_do_par.py:259`, `app/core/tests/test_medida_turno_do_par.py:645` |
| `for data_jornada in [_prev_dj]: pass` | Some: `For` não entra em `assignments` | `app/core/medida_turno_do_par.py:286` |
| `with cm(_prev_dj) as data_jornada: pass` | Some: `With` não entra em `assignments` | `app/core/medida_turno_do_par.py:286` |
| `data_jornada = copiar(_prev_dj)` | Capturada; já cobrada | `app/core/medida_turno_do_par.py:279`, `app/core/tests/test_medida_turno_do_par.py:599` |
| `setattr(self, 'data_jornada', _prev_dj)` | Some: chamada não constitui atribuição selecionada | `app/core/medida_turno_do_par.py:286` |
| `global`/`nonlocal` + atribuição direta | Declaração não altera seleção da atribuição; alcance não é resolvido | `app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:286` |
| `dados = {'data_jornada': _prev_dj}` | Some como destino nomeado; chave de `Dict` não é destino reconhecido | `app/core/medida_turno_do_par.py:269` |
| `dados[chave] = _prev_dj` | Some se `chave` é variável, ainda que valha `'data_jornada'` | `app/core/medida_turno_do_par.py:274` |

`NAO_VE` declara fluxo/alcance, origem interprocedural, chamadas dinâmicas e conservadorismo sem casamento posicional. Não enumera expressamente os buracos `for`/`with`/chave de dicionário do gap. A promessa “outras reescritas” do docstring excede o detector mostrado. (`app/core/medida_turno_do_par.py:34`, `app/core/medida_turno_do_par.py:35`, `app/core/medida_turno_do_par.py:36`, `app/core/medida_turno_do_par.py:55`, `app/core/medida_turno_do_par.py:255`)

Mutantes candidatos enviados para validação integral pelo pai:

```python
# P2: substitui a guarda em :316
if 0 < len(inherited - recognized) <= 2:
```

Deveria reprovar com **três** heranças desconhecidas; o teste atual usa duas. (`app/core/tests/test_medida_turno_do_par.py:616`)

```python
# P3: acrescenta ao if de bindings em :260
and len(target.elts) <= 2
```

Deveria reprovar com casamento correto de três posições, por exemplo `data_jornada, outro, ultimo = (x, _prev_dj, y)`. A fixture atual cobre apenas pares. (`app/core/tests/test_medida_turno_do_par.py:626`)

```python
# P4: substitui o ramo Attribute de source em :281
or (isinstance(n, ast.Attribute) and n.attr == '_prev_dj'
    and isinstance(n.value, ast.Name) and n.value.id == 'self')
```

Deveria reprovar exigindo `LookupError` para `data_jornada = estado._prev_dj`; a cobertura existente usa `self._prev_dj`. (`app/core/tests/test_medida_turno_do_par.py:657`, `app/core/tests/test_medida_turno_do_par.py:659`, `app/core/tests/test_medida_turno_do_par.py:662`)

## 07/11 17:10 · sessao 7114a41c4856 · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

Curas implementadas:

- **P1:** linhas reais A1/B1 e nomeadas congeladas por igualdade, com mensagem “registrar no MESMO commit” (`core/tests/test_medida_turno_do_par.py:227`, `:546`).
- **P2:** duas heranças desconhecidas exigem `LookupError` listando `[12, 13]` (`core/tests/test_medida_turno_do_par.py:616`).
- **P3/P4:** casamento posicional recursivo, fallback conservador, fontes Name/Attribute, aliases transitivos e destino Subscript (`core/medida_turno_do_par.py:259`). Alias **dentro** do `if _gap_h` exige `LookupError`, conforme declaração (`:255`) e teste (`core/tests/test_medida_turno_do_par.py:675`).
- **P5:** retirado o bloco histórico; mantidos runner e seis comandos de mutação (`core/tests/test_medida_turno_do_par.py:20`).

Os registros mantêm **9/85** e a parcela atual do gap em **[2408]** (`core/tests/test_medida_turno_do_par.py:77`, `:235`).

Resultados observados em memória nesta sessão; comprovação persistida das execuções: **[nao verificado]**. Cada link aponta o comando, usando o mesmo heredoc do runner:

| Execução | Resultado | Comando |
|---|---|---|
| Original | **28/28 OK, rc=0** | [runner](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:21) |
| composição | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:58) |
| linhas | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:59) |
| gap | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:60) |
| classificar | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:61) |
| P1-linha-unica | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:62) |
| P2-len1 | **RED, rc=1** | [comando](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:63) |

Quem escreve e quem lê os novos registros e intermediários:

| Campo | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `LINHAS_REGISTRADAS` | Registro literal, `core/tests/test_medida_turno_do_par.py:227` | Igualdade real, mesmo arquivo `:547` |
| `aliases` | Inicialização e propagação, `core/medida_turno_do_par.py:277`, `:295` | Detector de fonte e ponto fixo, mesmo arquivo `:280`, `:296` |
| `assignments` / pares posicionais | Coleta e `bindings`, `core/medida_turno_do_par.py:259`, `:289` | Propagação e seleção de heranças, mesmo arquivo `:292`, `:299` |
| `inherited` ampliado | Seleção, `core/medida_turno_do_par.py:301` | Guarda e diagnóstico, mesmo arquivo `:316`, `:319` |

Certificação pelo Code/opus final: **[nao verificado]**.

## 07/11 17:03 · sessao 0601c3de7f56 · MOTOR-LE-O-TURNO-INSTRUMENTO · verificador · origem=Code

**As curas 2 e 5b fecham os defeitos específicos apontados; 3 e 4 ainda deixam classes abertas.** A composição ganhou comparação integral, `CLASSIFICAR` ganhou leitor, mas encontrei mutantes sobreviventes para linhas e guarda de herança. (`app/core/tests/test_medida_turno_do_par.py:368`, `app/core/tests/test_medida_turno_do_par.py:415`, `app/core/tests/test_medida_turno_do_par.py:432`, `app/core/tests/test_medida_turno_do_par.py:456`)

A comparação histórica `c2058690..133cd78a` permanece **[nao verificado]**: a cópia não disponibiliza `.git`. As conclusões abaixo confrontam a fonte disponível com a triagem fornecida. **[nao verificado quanto ao delta histórico]**

Executei em memória, sem Django e com bytecode desabilitado: original **22/22 OK**; cada mutante abaixo, separadamente, **22/22 OK**. O procedimento corresponde ao runner documentado; os resultados desta sessão não têm artefato persistido com arquivo:linha: **[nao verificado documentalmente]**. (`app/core/tests/test_medida_turno_do_par.py:18`, `app/core/tests/test_medida_turno_do_par.py:24`)

| Item procedente da v1 | Veredito da cura |
|---|---|
| **2 — composição expansível** | **Fecha a classe específica “membros mudam sem registro” na suite:** compara a lista inteira da medida real. Não prova legitimidade semântica dos corpos já excluídos. (`app/core/tests/test_medida_turno_do_par.py:368`, `app/core/tests/test_medida_turno_do_par.py:372`; `app/core/medida_turno_do_par.py:343`) |
| **3 — linhas falsas** | **Fecha o mutante anterior, não a classe:** precisão cobrada numa fixture com duas ocorrências por função; na medida real, apenas presença de linhas. (`app/core/tests/test_medida_turno_do_par.py:437`, `app/core/tests/test_medida_turno_do_par.py:441`, `app/core/tests/test_medida_turno_do_par.py:454`, `app/core/tests/test_medida_turno_do_par.py:412`) |
| **4 — herança desaparece por forma** | **Amplia a proteção, sem fechar a classe:** procura determinados alvos e o nome literal `_prev_dj`, sem propagar aliases nem casar posições de tuplas. (`app/core/medida_turno_do_par.py:255`, `app/core/medida_turno_do_par.py:261`) |
| **5b — `CLASSIFICAR` sem leitor** | **Fecha o defeito “leitor zero”:** igualdade integral entre registro e publicação calculada. (`app/core/tests/test_medida_turno_do_par.py:226`, `app/core/tests/test_medida_turno_do_par.py:415`) |

**Mutante sobrevivente do item 3**, na publicação lexical em `app/core/medida_turno_do_par.py:351`:

```python
# original
'criterio': criterion, 'linhas': lines

# mutante
'criterio': criterion,
'linhas': lines if len(lines) > 1 else [node.lineno]
```

A fixture mantém suas duas ocorrências; funções com ocorrência única passam a publicar o `def`. Isso erra **A1 real**, apontando `_alinhar_por_papel:502` em vez do `range:520`, e **B1 real**, apontando `_dia_da_batida:47` em vez de `localdate:50`. (`app/core/tests/test_medida_turno_do_par.py:449`, `app/core/tests/test_medida_turno_do_par.py:450`; `app/escala/utils.py:502`, `app/escala/utils.py:520`; `app/chamados/juizes.py:47`, `app/chamados/juizes.py:50`)

**Mutante sobrevivente do item 4**, na guarda em `app/core/medida_turno_do_par.py:278`:

```python
# original
if inherited - recognized:

# mutante
if len(inherited - recognized) == 1:
```

Os nove subcasos atuais têm exatamente **uma atribuição desconhecida** por variante, inclusive os casos que também conservam uma reconhecida. Falta cobrar duas desconhecidas. (`app/core/tests/test_medida_turno_do_par.py:458`, `app/core/tests/test_medida_turno_do_par.py:459`, `app/core/tests/test_medida_turno_do_par.py:468`, `app/core/tests/test_medida_turno_do_par.py:469`)

Com esse mutante, renomear `_gap_h` e duplicar `data_jornada = _prev_dj` na fixture devolveu normalmente **2/2**, retirando a parcela gap; resultado em memória sem artefato persistido: **[nao verificado documentalmente]**. A parcela depende das linhas devolvidas por `_gap`. (`app/core/medida_turno_do_par.py:361`, `app/core/medida_turno_do_par.py:363`)

**a) Guarda de `_gap`: classificação desdobrada.**

- **NAO PROCEDE — falso positivo demonstrado na fonte atual:** a herança presente está sob a comparação reconhecida e corresponde à atribuição simples esperada. Execução produtiva: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:2407`, `app/ponto/motor_calculo_v2.py:2408`; `app/core/medida_turno_do_par.py:274`)
- **NAO PROCEDE — escape das formas diretas ensaiadas:** tupla com `_prev_dj`, atributo **de destino**, walrus e `augassign` são submetidos à exigência de `LookupError`. (`app/core/tests/test_medida_turno_do_par.py:462`, `app/core/tests/test_medida_turno_do_par.py:463`, `app/core/tests/test_medida_turno_do_par.py:464`, `app/core/tests/test_medida_turno_do_par.py:465`, `app/core/tests/test_medida_turno_do_par.py:475`)
- **PROCEDENTE — falso estrutural possível:** `data_jornada, outro = (data_entrada, _prev_dj)` acusa herança, embora `_prev_dj` alimente `outro`. A guarda cruza quaisquer alvos compatíveis com qualquer ocorrência desse nome no RHS. (`app/core/medida_turno_do_par.py:255`, `app/core/medida_turno_do_par.py:259`, `app/core/medida_turno_do_par.py:261`)
- **PROCEDENTE — escapes restantes:** `anterior = _prev_dj; data_jornada = anterior`, `data_jornada = self._prev_dj` e `dados['data_jornada'] = _prev_dj` escapam à seleção. Respectivamente: falta propagação, origem é `Attribute`, destino é `Subscript`. (`app/core/medida_turno_do_par.py:256`, `app/core/medida_turno_do_par.py:261`)

O desaparecimento isolado da parcela ainda enfrenta a catraca numérica; falha do detector não equivale automaticamente à aprovação da suite. (`app/core/tests/test_medida_turno_do_par.py:359`, `app/core/tests/test_medida_turno_do_par.py:364`)

**b) NAO PROCEDE — composição congelada apenas por tamanho.** A igualdade cobra todos os membros e sua ordem; troca de identidade mantendo tamanho é rejeitada. **FILA:** conteúdo, uso compartilhado e legitimidade de cada função já composta continuam sem prova semântica; a exclusão permanece integral por função. (`app/core/tests/test_medida_turno_do_par.py:368`; `app/core/medida_turno_do_par.py:343`)

**c) PROCEDENTE — cobertura exata limitada à fixture.** O teste cobre A1/B1 sintéticos, inclusive deslocamento +40. Na medida real, `REGISTRO` compara chave/critério/sítio, sem linhas, e depois exige somente listas não vazias. (`app/core/tests/test_medida_turno_do_par.py:444`, `app/core/tests/test_medida_turno_do_par.py:454`, `app/core/tests/test_medida_turno_do_par.py:407`, `app/core/tests/test_medida_turno_do_par.py:412`)

**d) PROCEDENTE — existem fatos documentais que a suite não prova.** Autoria “do construtor”, data, duração `30.656s` e resultados das quatro execuções externas são afirmações históricas, não conclusões verificadas pelas asserções. Sua falsidade também está **[nao verificado]**. (`app/core/tests/test_medida_turno_do_par.py:58`, `app/core/tests/test_medida_turno_do_par.py:59`, `app/core/tests/test_medida_turno_do_par.py:60`)

“MESMO commit” é exigência textual: os testes comparam a árvore corrente, sem demonstrar contemporaneidade no histórico. **NAO PROCEDE** atribuir ao docstring uma certificação Django oficial: ela está explicitamente marcada `[nao verificado]`. (`app/core/tests/test_medida_turno_do_par.py:364`, `app/core/tests/test_medida_turno_do_par.py:368`, `app/core/tests/test_medida_turno_do_par.py:415`, `app/core/tests/test_medida_turno_do_par.py:64`)

**e) Classes que já reprovaram a obra.**

- **NAO PROCEDE nos dois arquivos examinados:** segundo juiz de negócio, segundo escritor persistente ou relógio fora da borda. O instrumento lê fontes, analisa AST e retorna estruturas; seus imports são `ast`/`os`. (`app/core/medida_turno_do_par.py:21`, `app/core/medida_turno_do_par.py:378`, `app/core/medida_turno_do_par.py:389`)
- **FILA — derivação paralela já existente:** as portas e a herança continuam no motor, medidas positivamente; não foram curadas por esta instrumentação. (`app/ponto/motor_calculo_v2.py:503`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:2408`; `app/core/medida_turno_do_par.py:59`)
- **PROCEDENTE quanto à garantia incompleta do RED:** os dois mutantes acima sobrevivem à cobertura atual. **NAO PROCEDE** chamar toda a mordida de decorativa: novos pareador e derivador lexical têm incremento e rejeição exigidos. (`app/core/tests/test_medida_turno_do_par.py:432`, `app/core/tests/test_medida_turno_do_par.py:456`, `app/core/tests/test_medida_turno_do_par.py:490`, `app/core/tests/test_medida_turno_do_par.py:498`)
- **FILA de nomenclatura:** `CLASSIFICAR` é chamado “sem teto ou catraca”, mas sua identidade pode reprovar a suite; o próprio texto esclarece que a cobrança é de registro. Divergência de unidade entre escritor e leitor: **NAO PROCEDE** na comparação mostrada. (`app/core/tests/test_medida_turno_do_par.py:12`, `app/core/tests/test_medida_turno_do_par.py:14`, `app/core/tests/test_medida_turno_do_par.py:415`)

**f) QUEM ESCREVE / QUEM LÊ os valores envolvidos nas curas.**

| Valor | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `inherited` | Seleção de atribuições, `app/core/medida_turno_do_par.py:263` | Diferença e diagnóstico, `app/core/medida_turno_do_par.py:278` |
| `recognized` | Reconhecimento sob `_gap_h`, `app/core/medida_turno_do_par.py:277` | Guarda e linhas retornadas, `app/core/medida_turno_do_par.py:278`, `app/core/medida_turno_do_par.py:282` |
| Linhas/erro do gap | Retorno ou exceção, `app/core/medida_turno_do_par.py:279` | Construção do nomeado, `app/core/medida_turno_do_par.py:361`; teste de exceção, `app/core/tests/test_medida_turno_do_par.py:475` |
| `composed` | Fechamento, `app/core/medida_turno_do_par.py:335` | Exclusão, `app/core/medida_turno_do_par.py:343`; guarda de nomeado absorvido, `app/core/medida_turno_do_par.py:358` |
| `composicao` / `COMPOSICAO_REGISTRADA` | Publicação, `app/core/medida_turno_do_par.py:384`; registro, `app/core/tests/test_medida_turno_do_par.py:79` | Igualdade integral, `app/core/tests/test_medida_turno_do_par.py:368` |
| `a_classificar_pela_regra` / `CLASSIFICAR` | Cálculo, `app/core/medida_turno_do_par.py:377`; registro, `app/core/tests/test_medida_turno_do_par.py:226` | Igualdade integral, `app/core/tests/test_medida_turno_do_par.py:415` |
| Linhas A1/B1 | Ocorrências AST, `app/core/medida_turno_do_par.py:191`, `app/core/medida_turno_do_par.py:214`; publicação, `app/core/medida_turno_do_par.py:351` | Presença na medida real, `app/core/tests/test_medida_turno_do_par.py:412`; exatidão na fixture, `app/core/tests/test_medida_turno_do_par.py:454` |
| Fonte com chamada morta / variantes | Injeção em memória, `app/core/tests/test_medida_turno_do_par.py:389`, `app/core/tests/test_medida_turno_do_par.py:458` | `censo`/`censo_fontes`, `app/core/tests/test_medida_turno_do_par.py:395`, `app/core/tests/test_medida_turno_do_par.py:476` |
| “Registro da execução” documental | Texto estático, `app/core/tests/test_medida_turno_do_par.py:58` | Leitor automático que comprove sua proveniência: **[nao verificado]** |

## 07/11 17:02 · sessao 93157a80dbed · - · - · origem=Ronald (tmux)

**Item 3 — PROCEDENTE; cura fecha o mutante anterior, não a classe.** A comparação exata de A1/B1 cobre somente `novo.py` sintético, com **duas ocorrências por função**, nos deslocamentos 0/+40. Na medida real, o registro compara identidade sem linhas e só exige linhas não vazias. (`app/core/tests/test_medida_turno_do_par.py:432`, `app/core/tests/test_medida_turno_do_par.py:448`, `app/core/tests/test_medida_turno_do_par.py:454`, `app/core/tests/test_medida_turno_do_par.py:407`, `app/core/tests/test_medida_turno_do_par.py:412`)

Mutante sobrevivente no instrumento, substituindo a publicação em `app/core/medida_turno_do_par.py:351`:

```python
'criterio': criterion,
'linhas': lines if len(lines) > 1 else [node.lineno]
```

A fixture preserva suas duas ocorrências; uma função com uma ocorrência publica o `def`. Exemplos reais: `_alinhar_por_papel` publica 502 em vez do `range` em 520; `_dia_da_batida` publica 47 em vez de `localdate` em 50. (`app/core/tests/test_medida_turno_do_par.py:437`, `app/core/tests/test_medida_turno_do_par.py:441`, `app/escala/utils.py:502`, `app/escala/utils.py:520`, `app/chamados/juizes.py:47`, `app/chamados/juizes.py:50`)

Executei a suite inteira com o runner em memória, `python3 -B`, trocando somente `SimpleTestCase` por `unittest.TestCase`: baseline **22 testes OK**, mutante acima **22 testes OK**, sem arquivos persistidos; resultado desta execução não dispõe de arquivo:linha e deve ser marcado **[nao verificado]** se a exigência de referência for aplicada também à evidência de ferramenta. A infraestrutura Django oficial permanece **[nao verificado]**. [nao verificado]

**d — PROCEDENTE quanto a fatos que a suite não prova; falsidade não demonstrada.** O docstring afirma uma execução histórica “do construtor”, data, duração de 30.656s e quatro resultados externos RED. São declarações documentais; os testes não comprovam autoria, data, duração nem proveniência dessa execução. (`app/core/tests/test_medida_turno_do_par.py:58`, `app/core/tests/test_medida_turno_do_par.py:59`, `app/core/tests/test_medida_turno_do_par.py:60`, `app/core/tests/test_medida_turno_do_par.py:63`)

O runner é fornecido, e a certificação Code/opus e suite oficial são explicitamente `[nao verificado]`; portanto não há ali alegação de certificação oficial. (`app/core/tests/test_medida_turno_do_par.py:18`, `app/core/tests/test_medida_turno_do_par.py:64`)

“MESMO commit” é uma exigência textual: as asserções comparam valores e listas da árvore corrente; não consultam histórico nem demonstram que atualização e código pertencem ao mesmo commit. (`app/core/tests/test_medida_turno_do_par.py:364`, `app/core/tests/test_medida_turno_do_par.py:368`, `app/core/tests/test_medida_turno_do_par.py:407`, `app/core/tests/test_medida_turno_do_par.py:415`)

**f — linhas novas/endurecidas.** `_scan` escreve linhas das ocorrências A1 e B1; `censo_fontes` as publica; o teste real lê apenas presença e a fixture lê igualdade exata. (`app/core/medida_turno_do_par.py:191`, `app/core/medida_turno_do_par.py:214`, `app/core/medida_turno_do_par.py:350`, `app/core/tests/test_medida_turno_do_par.py:412`, `app/core/tests/test_medida_turno_do_par.py:454`)

## 07/11 17:01 · sessao b4fd11f2739c · - · - · origem=Ronald (tmux)

**Item 4: a cura fecha as formas diretas ensaiadas, mas não a classe inteira.** A guarda procura destino `Name/Attribute` chamado `data_jornada` e ocorrência literal `Name('_prev_dj')` na expressão; não acompanha aliases nem associa elementos de desempacotamento. (`app/core/medida_turno_do_par.py:253`, `app/core/medida_turno_do_par.py:256`, `app/core/medida_turno_do_par.py:261`)

- **NAO PROCEDE — falso na cópia atual:** `_gap` retorna `[2408]`; a atribuição real está dentro da comparação reconhecida de `_gap_h`. Isso verifica a fonte, não uma execução produtiva com banco/log. (`app/ponto/motor_calculo_v2.py:2405`, `app/ponto/motor_calculo_v2.py:2407`, `app/ponto/motor_calculo_v2.py:2408`; execução produtiva: [nao verificado])
- **NAO PROCEDE — escape das formas diretas testadas:** tupla, atributo de destino, walrus e augassign contendo `_prev_dj` literal lançam `LookupError`, conforme exigido pelos respectivos casos. (`app/core/tests/test_medida_turno_do_par.py:459`, `app/core/tests/test_medida_turno_do_par.py:462`, `app/core/tests/test_medida_turno_do_par.py:463`, `app/core/tests/test_medida_turno_do_par.py:464`, `app/core/tests/test_medida_turno_do_par.py:465`, `app/core/tests/test_medida_turno_do_par.py:475`)
- **PROCEDENTE — falso estrutural:** `data_jornada, outro = (data_entrada, _prev_dj)` dispara a guarda embora `data_jornada` receba `data_entrada`: qualquer destino compatível é cruzado com qualquer ocorrência de `_prev_dj` no RHS. (`app/core/medida_turno_do_par.py:255`, `app/core/medida_turno_do_par.py:259`, `app/core/medida_turno_do_par.py:261`)
- **PROCEDENTE — escapes restantes:** `data_jornada = self._prev_dj`, `dados['data_jornada'] = _prev_dj` e `anterior = _prev_dj; data_jornada = anterior` retornam `[]`: o primeiro perde o nome literal de origem, o segundo perde o tipo permitido de destino e o terceiro exige propagação inexistente nessa guarda. (`app/core/medida_turno_do_par.py:256`, `app/core/medida_turno_do_par.py:261`, `app/core/medida_turno_do_par.py:282`)

**Mutante sobrevivente do instrumento**, na guarda: (`app/core/medida_turno_do_par.py:278`)

```python
# antes
if inherited - recognized:
# mutante
if len(inherited - recognized) == 1:
```

Executei os **22 métodos**, em memória, com `SimpleTestCase` substituído por `unittest.TestCase`: **0 falhas, 0 erros**; resultado da execução sem artefato persistido: **[nao verificado]**. O runner corresponde à substituição documentada. (`app/core/tests/test_medida_turno_do_par.py:24`)

Com esse mutante, renomear `_gap_h` e duplicar a herança na fixture retorna normalmente **2/2**, retirando a parcela nomeada; resultado em memória: **[nao verificado]**. Os nove subcasos atuais têm exatamente uma atribuição não reconhecida, inclusive aqueles que acrescentam outra atribuição reconhecida; falta um caso com duas desconhecidas. (`app/core/tests/test_medida_turno_do_par.py:458`, `app/core/tests/test_medida_turno_do_par.py:468`, `app/core/tests/test_medida_turno_do_par.py:469`, `app/core/tests/test_medida_turno_do_par.py:475`; publicação da parcela: `app/core/medida_turno_do_par.py:361`, `app/core/medida_turno_do_par.py:363`)

## 07/11 17:01 · sessao 2a612d8fe253 · - · - · origem=Ronald (tmux)

- **2 — cura fecha a classe indicada na triagem:** o teste compara os 47 membros da lista inteira, por identidade e ordem; não compara só tamanho. Troca de um membro mantendo 47 foi rejeitada em memória por `_composicao`. A lista nasce em `app/core/tests/test_medida_turno_do_par.py:79`; igualdade em `app/core/tests/test_medida_turno_do_par.py:368`; aplicação à medida real em `app/core/tests/test_medida_turno_do_par.py:372`.
- **Limite semântico — FILA:** continua existindo exclusão integral de qualquer função alcançada pelo fechamento, inclusive por chamada morta. Congelar os membros detecta mudança da lista; não prova que cada corpo ou uso compartilhado pertence à autoridade. O fechamento está em `app/core/medida_turno_do_par.py:330`; exclusão em `app/core/medida_turno_do_par.py:343`; o teste deliberadamente aceita exclusão transitiva em `app/core/tests/test_medida_turno_do_par.py:597`.
- **b — NAO PROCEDE:** acusação de que troca de identidade com mesmo tamanho não seria cobrada. A igualdade integral cobra isso diretamente. `app/core/tests/test_medida_turno_do_par.py:368`.
- **5b — cura fecha a classe “leitor zero”:** `CLASSIFICAR` agora tem leitor que exige igualdade integral com a publicação calculada. Seu escritor permanece o registro estático em `app/core/tests/test_medida_turno_do_par.py:226`; leitor em `app/core/tests/test_medida_turno_do_par.py:415`. O cálculo é escrito por `difference`/`classify` em `app/core/medida_turno_do_par.py:373` e publicado em `app/core/medida_turno_do_par.py:381`.
- **FILA de nomenclatura:** o docstring chama `CLASSIFICAR` de “sem teto ou catraca”, embora sua igualdade possa reprovar alterações; as linhas seguintes explicam que é cobrança de registro, sem promover candidatos aos contadores. `app/core/tests/test_medida_turno_do_par.py:12`; `app/core/tests/test_medida_turno_do_par.py:14`; `app/core/tests/test_medida_turno_do_par.py:415`.
- **Quem escreve/lê composição:** fechamento escreve `composed` em `app/core/medida_turno_do_par.py:335`; exclusão lê em `app/core/medida_turno_do_par.py:343`; guarda de nomeado absorvido lê em `app/core/medida_turno_do_par.py:358`; publicação escreve `composicao` em `app/core/medida_turno_do_par.py:384`; teste lê em `app/core/tests/test_medida_turno_do_par.py:368`.
- **Quem escreve/lê expectativa:** `COMPOSICAO_REGISTRADA` é escrito em `app/core/tests/test_medida_turno_do_par.py:79` e lido em `app/core/tests/test_medida_turno_do_par.py:368`; o instrumento não importa esse registro, pois seus únicos imports são `ast` e `os`. `app/core/medida_turno_do_par.py:21`.

Mutante do instrumento sobrevivente específico dessas duas curas: **[nao verificado]**. Não executei a suite inteira; executei a prova direta da troca de identidade 47→47. **[nao verificado quanto à suite inteira]** [nao verificado]

## 07/11 16:58 · sessao d959e2a6a2ad · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

Entrega para certificação, com as quatro curas:

| Cura/teste | Mutante que derruba | [nao verificado]
|---|---|
| [Composição congelada + MORDE](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:423) | Chamada nova sob `if False` no juiz; exige registro no MESMO commit. | [nao verificado]
| [Linhas exatas A1/B1](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:432) | `'linhas': [node.lineno]`, publicando o `def`. |
| [Gap](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:456) | Renomear `_gap_h`; remover a guarda faz o teste falhar. | [nao verificado]
| [Registro CLASSIFICAR](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:414) | Trocar identidade mantendo a quantidade. |

Execução em memória: **22 testes OK; quatro mutantes externos RED, rc=1**. [Registro](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:58) [nao verificado]

[Runner da suite](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:18) e comandos individuais: [composição](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:52), [linhas](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:53), [gap](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:54), [CLASSIFICAR](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:55). [nao verificado]

Herança não reconhecida agora lança `LookupError` com “rever detector”; destino transitivo ignorado está declarado em `NAO_VE`. [Guarda](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:278), [limitação](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:39).

Certificação Code/opus e execução Django oficial: **[nao verificado]**.

## 07/11 16:49 · sessao bc9f57e9f38b · MOTOR-LE-O-TURNO-INSTRUMENTO · verificador · origem=Code

**Há achados PROCEDENTES na exclusão por composição e na cobertura dos testes.** A medição atual reproduz 9/85, mas isso não prova que toda derivação exigida continuará contada após mudanças de forma. (`app/core/medida_turno_do_par.py:324`, `app/core/medida_turno_do_par.py:350`; `app/core/tests/test_medida_turno_do_par.py:31`)

A comparação histórica `ebe80893..c2058690` está **[nao verificado]**: esta cópia não disponibiliza o histórico Git.

1. **Critério literal — contagem atual correta; garantia incompleta.**

   **NAO PROCEDE** a acusação de promover A2/B2 ao contador: a composição dos contadores é exatamente A1 + nomeados e B1 + nomeados; as diferenças amplas são publicadas separadamente. Reproduzi A1=6, A2=15, B1=82, B2=171; nomeados=3; contadores=9/85; diferenças=9/89. (`app/core/medida_turno_do_par.py:348`, `app/core/medida_turno_do_par.py:358`, `app/core/medida_turno_do_par.py:360`)

   Os três nomeados correspondem às chamadas em `app/ponto/motor_calculo_v2.py:503` e `app/ponto/motor_calculo_v2.py:882`, e à herança em `app/ponto/motor_calculo_v2.py:2408`; entram positivamente nos dois contadores. (`app/core/medida_turno_do_par.py:57`, `app/core/medida_turno_do_par.py:345`, `app/core/medida_turno_do_par.py:349`)

   **PROCEDENTE:** existem maneiras de deixar de contar sem remover a derivação: composição expansível, descrita no item 2, e reconhecimento frágil do gap, descrito no item 4. (`app/core/medida_turno_do_par.py:253`, `app/core/medida_turno_do_par.py:324`)

2. **Allowlist e composição — distinguir duas coisas.**

   **NAO PROCEDE** chamar `REGISTRO` ou `SITIOS_NOMEADOS` de allowlist de funções: o primeiro é esperado da suite; o segundo acrescenta parcelas. Nenhum deles dispensa nominalmente uma função produtiva da varredura. (`app/core/tests/test_medida_turno_do_par.py:296`; `app/core/medida_turno_do_par.py:335`, `app/core/medida_turno_do_par.py:349`)

   Existem exclusões nominais de **universo**: diretórios em `_FORA`, diretórios ocultos e arquivos `test_*.py`; elas correspondem ao molde. (`app/core/medida_turno_do_par.py:26`, `app/core/medida_turno_do_par.py:225`, `app/core/medida_turno_do_par.py:227`; `app/core/medida_leitor_do_dia.py:247`, `app/core/medida_leitor_do_dia.py:249`)

   **PROCEDENTE — a composição funciona como isenção estrutural expansível.** Qualquer chamada AST alcançada pelo fechamento inclui sua função em `composed`; depois essa função inteira fica fora dos quatro critérios. Não há verificação de execução, exclusividade de uso ou responsabilidade pela pergunta do juiz. (`app/core/medida_turno_do_par.py:311`, `app/core/medida_turno_do_par.py:318`, `app/core/medida_turno_do_par.py:324`)

   Prova em memória: uma função externa contendo `entrada.date()` conta B1=1; acrescentar ao juiz apenas `if False: leitor(None)` faz B1=0, sem retirar a derivação externa. O algoritmo percorre essa chamada morta e aplica a exclusão integral. (`app/core/medida_turno_do_par.py:65`, `app/core/medida_turno_do_par.py:318`, `app/core/medida_turno_do_par.py:324`)

   Excluir a implementação do próprio juiz é compatível com “fora do juiz”; equiparar **todo chamado transitivo**, inclusive morto e compartilhado, à implementação autorizada é uma extensão do critério. Autorização explícita dessa extensão no aval fornecido: **[nao verificado]**. A suite consagra a exclusão transitiva, mas não demonstra sua legitimidade semântica. (`app/core/tests/test_medida_turno_do_par.py:416`, `app/core/tests/test_medida_turno_do_par.py:429`)

3. **MORDE — há mordida local, mas existe mutante sobrevivente.**

   **NAO PROCEDE** dizer que todos são decorativos: os testes de pareador e dia novos verificam incremento e rejeição por `SUBIU`. (`app/core/tests/test_medida_turno_do_par.py:322`, `app/core/tests/test_medida_turno_do_par.py:325`, `app/core/tests/test_medida_turno_do_par.py:330`, `app/core/tests/test_medida_turno_do_par.py:333`)

   A cobertura efetiva dos 13 `test_MORDE` é esta:

   | Teste | O que efetivamente cobra | [nao verificado]
   |---|---|
   | Candidato amplo | Publicação de novos A2/B2 sem subir os contadores. `app/core/tests/test_medida_turno_do_par.py:310` |
   | Pareador novo | Um `range(..., 2)` novo sobe contador e dispara RED. `app/core/tests/test_medida_turno_do_par.py:318` |
   | Dia novo | Alias de `entrada` seguido de `.date()` sobe contador e dispara RED. `app/core/tests/test_medida_turno_do_par.py:328` |
   | Número curado | Diminui diretamente o resultado; testa `_catraca`, sem curar fonte nem exercitar detector. `app/core/tests/test_medida_turno_do_par.py:336` |
   | Âncora renomeada | Renomear cada função nomeada provoca exceção. `app/core/tests/test_medida_turno_do_par.py:344` |
   | Sintaxe inválida | Exige `SyntaxError` e identificação do arquivo. `app/core/tests/test_medida_turno_do_par.py:353` |
   | Leitura/travessia | Testa erro de `open` e raiz inexistente; não injeta falha intermediária de travessia ou encoding. `app/core/tests/test_medida_turno_do_par.py:358` |
   | Linhas/alias | Desloca fonte e verifica linhas **dos nomeados**; usa os aliases já presentes na fixture. `app/core/tests/test_medida_turno_do_par.py:374` |
   | Cura dos nomeados | Remove corpos, conserva âncoras e exige zero. `app/core/tests/test_medida_turno_do_par.py:383` |
   | Critérios lexicais | Exemplos de `zip`, `append`, `localdate` e `.date()` civil. `app/core/tests/test_medida_turno_do_par.py:394` |
   | Composição | Exige excluir auxiliar chamado e contar função independente do mesmo arquivo. `app/core/tests/test_medida_turno_do_par.py:416` |
   | Aninhamento | Exige atribuir B1 à função interna. `app/core/tests/test_medida_turno_do_par.py:433` |
   | Autoridade ausente | Remove arquivos inteiros e exige exceção. `app/core/tests/test_medida_turno_do_par.py:441` |

   **PROCEDENTE — mutante do instrumento que passa todos:** em `app/core/medida_turno_do_par.py:332`, substituir `'linhas': lines` por `'linhas': [node.lineno]`. Ele publica a linha do `def` em lugar da ocorrência: por exemplo, aponta `app/escala/utils.py:502`, embora o `range` esteja em `app/escala/utils.py:520`. A suite exige apenas linhas não vazias para A1/B1; a comparação precisa de deslocamento cobre os nomeados. (`app/core/tests/test_medida_turno_do_par.py:301`, `app/core/tests/test_medida_turno_do_par.py:380`)

   Esse mutante passou nos **18 métodos**, executados em memória com `SimpleTestCase` substituído por `unittest.TestCase`. A execução pela infraestrutura Django oficial está **[nao verificado]**. (`app/core/tests/test_medida_turno_do_par.py:26`, `app/core/tests/test_medida_turno_do_par.py:257`)

4. **Falha silenciosa — erros de arquivo propagam; detecção incompleta pode retornar normalmente.**

   **NAO PROCEDE** para erros reportados por `os.walk`, leitura/encoding, `SyntaxError`, autoridade ausente, raízes ausentes e funções âncora ausentes: os caminhos propagam exceção ou lançam `LookupError`. (`app/core/medida_turno_do_par.py:221`, `app/core/medida_turno_do_par.py:272`, `app/core/medida_turno_do_par.py:273`, `app/core/medida_turno_do_par.py:290`, `app/core/medida_turno_do_par.py:337`, `app/core/medida_turno_do_par.py:374`)

   **PROCEDENTE para contagem parcial por forma:** renomeei somente `_gap_h` para `_intervalo_h` na fixture, mantendo a mesma herança. O censo retornou normalmente e ambos os contadores caíram de 3 para 2. O detector exige aquele identificador e, sem correspondência, simplesmente não acrescenta o nomeado. (`app/core/medida_turno_do_par.py:253`, `app/core/medida_turno_do_par.py:344`; fixture em `app/core/tests/test_medida_turno_do_par.py:249`)

   A catraca de números atuais acusaria essa queda; portanto o resultado parcial do **instrumento** não equivale a passagem silenciosa pela **suite**. (`app/core/tests/test_medida_turno_do_par.py:269`)

   **FILA:** destinos transitivos sem registro são ignorados, sem distinguir dependência local ausente de chamada externa não resolvida; somente as raízes têm presença exigida. (`app/core/medida_turno_do_par.py:290`, `app/core/medida_turno_do_par.py:314`)

5. **ESMERIL-SEMANTICA — nome ambíguo, sem leitor mostrado usando outra unidade.**

   **NAO PROCEDE** uma divergência demonstrada entre escritor e leitor: `numero` mede parcelas; `itens` contém essas parcelas; `funcoes` projeta suas chaves **com repetição**; `linhas` contém ocorrências AST dentro da parcela. A suite compara essas mesmas relações. (`app/core/medida_turno_do_par.py:348`, `app/core/medida_turno_do_par.py:351`; `app/core/tests/test_medida_turno_do_par.py:294`, `app/core/tests/test_medida_turno_do_par.py:295`)

   **FILA de nomenclatura:** `funcoes` não é conjunto de funções únicas. Uma função B1 que também contém um nomeado aparece duas vezes, multiplicidade declarada pelo instrumento. Nenhum consumidor mostrado interpreta essa lista como conjunto único. (`app/core/medida_turno_do_par.py:10`, `app/core/medida_turno_do_par.py:351`; `app/core/tests/test_medida_turno_do_par.py:295`)

6. **Universo — nenhum outro desvio encontrado frente a `producao`.**

   **NAO PROCEDE:** reproduzi listas idênticas, com 755 arquivos, entre os dois `producao`. As regras de seleção correspondem e a suite compara as listas completas. (`app/core/medida_turno_do_par.py:224`; `app/core/medida_leitor_do_dia.py:246`; `app/core/tests/test_medida_turno_do_par.py:449`)

   `e6_piso_autopsia` não é excluído por `producao` do molde: sua exclusão pertence ao estágio `NAO_E_LEITOR` daquele outro detector. Portanto sua inclusão aqui não constitui desvio de `producao`. (`app/core/medida_leitor_do_dia.py:240`, `app/core/medida_leitor_do_dia.py:282`; `app/core/tests/test_medida_turno_do_par.py:452`)

   A atribuição histórica da diferença 83→82 exclusivamente a `bin/` consta da documentação da obra; recomposição independente do censo antigo: **[nao verificado]**. (`app/core/medida_turno_do_par.py:5`, `app/core/tests/test_medida_turno_do_par.py:4`)

7. **Juiz novo / segundo escritor / relógio / derivação paralela.**

   **NAO PROCEDE nos dois arquivos examinados:** o instrumento importa `ast`/`os`, lê fontes e devolve estruturas em memória; não escreve dados de negócio nem consulta relógio. (`app/core/medida_turno_do_par.py:21`, `app/core/medida_turno_do_par.py:348`, `app/core/medida_turno_do_par.py:374`)

   **FILA de negócio:** as portas do motor e a herança continuam presentes, agora medidas como RED nomeado. A receita `turnos_de_batidas` copia batidas, prepara parâmetros e chama o pareador; o gap continua escrevendo `data_jornada`. (`app/ponto/turnos.py:1515`, `app/ponto/turnos.py:1533`; `app/ponto/motor_calculo_v2.py:2408`; `app/core/medida_turno_do_par.py:57`)

8. **QUEM ESCREVE / QUEM LÊ os valores públicos e insumos.**

   A autoridade de negócio vem do registro vivo; `JUIZES.md` se declara lookup. O instrumento lê esse registro, não o Markdown. (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`; `app/core/juizes.py:196`; `app/core/medida_turno_do_par.py:232`)

   | Valor | Quem escreve/fornece | Quem lê |
   |---|---|---|
   | Fontes e árvores AST | Leitura em `app/core/medida_turno_do_par.py:374`; parse em `app/core/medida_turno_do_par.py:272` | Indexação em `app/core/medida_turno_do_par.py:277` |
   | Autoridade | `JUIZES`, `app/core/juizes.py:196` | `_autoridade`, `app/core/medida_turno_do_par.py:232`; raízes em `app/core/medida_turno_do_par.py:275` |
   | Composição | Fechamento em `app/core/medida_turno_do_par.py:311`; publicação em `app/core/medida_turno_do_par.py:365` | Exclusão em `app/core/medida_turno_do_par.py:324`; teste em `app/core/tests/test_medida_turno_do_par.py:429` |
   | `chave`, `criterio`, `linhas` lexicais | `app/core/medida_turno_do_par.py:331` | Montagem em `app/core/medida_turno_do_par.py:349`; validação em `app/core/tests/test_medida_turno_do_par.py:296` |
   | `sitio`, nomeados e suas `linhas` | `app/core/medida_turno_do_par.py:342` | Ambos os contadores em `app/core/medida_turno_do_par.py:349`; testes em `app/core/tests/test_medida_turno_do_par.py:365` |
   | `numero` | `len(items)`, `app/core/medida_turno_do_par.py:350` | `_catraca`, `app/core/tests/test_medida_turno_do_par.py:266` |
   | `itens` | Lexicais + nomeados, `app/core/medida_turno_do_par.py:349` | Consistência/identidade em `app/core/tests/test_medida_turno_do_par.py:294` |
   | `funcoes` | Projeção com multiplicidade, `app/core/medida_turno_do_par.py:351` | Comparação em `app/core/tests/test_medida_turno_do_par.py:295` |
   | `a_classificar_pela_regra` | Diferença de chaves em `app/core/medida_turno_do_par.py:354` | Testes em `app/core/tests/test_medida_turno_do_par.py:303` |
   | `criterios` | Tamanhos das listas, `app/core/medida_turno_do_par.py:363` | Testes em `app/core/tests/test_medida_turno_do_par.py:288` |
   | `universo_arquivos`, `ilegiveis` | `len(trees)` e constante `[]`, `app/core/medida_turno_do_par.py:366` | Testes em `app/core/tests/test_medida_turno_do_par.py:290` |
   | `nao_ve` | Constante em `app/core/medida_turno_do_par.py:31` | Comparação em `app/core/tests/test_medida_turno_do_par.py:299` |
   | `esperado_no_fim` | Constante zero em `app/core/medida_turno_do_par.py:352` | Teste em `app/core/tests/test_medida_turno_do_par.py:300`; a catraca atual lê outro teto em `app/core/tests/test_medida_turno_do_par.py:265` |
   | Rótulo `contador` | `app/core/medida_turno_do_par.py:350`, `app/core/medida_turno_do_par.py:360` | Consumidor externo **[nao verificado]** |
   | Esperados numéricos e `REGISTRO` | `app/core/tests/test_medida_turno_do_par.py:31` | Cat­raca e identidade em `app/core/tests/test_medida_turno_do_par.py:265`, `app/core/tests/test_medida_turno_do_par.py:296` |
   | `CLASSIFICAR` estático | `app/core/tests/test_medida_turno_do_par.py:131` | Nenhum leitor localizado; os testes leem a classificação calculada em `app/core/tests/test_medida_turno_do_par.py:305` |

   Publicação efetiva desses valores em placar/HOST fora da suite: **[nao verificado]**. [nao verificado]

## 07/11 16:47 · sessao 2bf3f03528c4 · - · - · origem=Ronald (tmux)

Achados consolidados, somente leitura:

- **PROCEDENTE — exclusão estrutural por composição pode baixar contador sem curar derivação.** O fechamento percorre toda chamada AST, inclusive sob `if False`, e exclui inteiramente a função alcançada da medição: `app/core/medida_turno_do_par.py:311`, `:318`, `:324`. Prova em memória: `externo.py::leitor(entrada): return entrada.date()` contou B1=1; acrescentar somente `if False: leitor(None)` ao juiz produziu B1=0, mantendo a derivação. A insensibilidade a ramos está declarada em `:34`. O teste explicitamente consagra a exclusão transitiva de um auxiliar: `app/core/tests/test_medida_turno_do_par.py:416`, `:429`, `:430`. Autorização literal do aval para fechamento transitivo irrestrito: **[não verificado]**.

- **NÃO PROCEDE — outra allowlist nominal encontrada.** Os nomes positivos dos três sítios adicionam parcelas, não dispensam funções: `app/core/medida_turno_do_par.py:57`, `:335`, `:345`, `:349`. A exclusão de funções ocorre por composição em `:324`. As exclusões de diretórios e prefixo `test_` pertencem ao universo: `:26`, `:225`, `:227`.

- **NÃO PROCEDE — desvio adicional frente a `producao` do molde nesta cópia.** Execução em memória encontrou listas idênticas, com 755 arquivos. As regras correspondem: novo em `app/core/medida_turno_do_par.py:224`, `:225`, `:227`; molde em `app/core/medida_leitor_do_dia.py:246`, `:247`, `:249`. O teste compara as listas completas: `app/core/tests/test_medida_turno_do_par.py:449`. A exclusão de `e6_piso_autopsia` do molde pertence a outro estágio (`NAO_E_LEITOR`), não à sua `producao`: `app/core/medida_leitor_do_dia.py:278`, `:282`, `:377`.

- **NÃO PROCEDE — leitura, encoding, SyntaxError ou raízes ausentes virarem zero silencioso.** `os.walk` recebe callback que relança erro (`app/core/medida_turno_do_par.py:221`, `:224`); leitura UTF-8-SIG não captura exceção (`:374`); `ast.parse` também não captura (`:272`); autoridade, raízes e âncoras faltantes geram `LookupError` (`:273`, `:290`, `:337`). Os testes cobrem leitura, raiz inexistente, SyntaxError e autoridade/composição ausente: `app/core/tests/test_medida_turno_do_par.py:353`, `:358`, `:441`.

- **FILA — composição incompleta pode ser apresentada normalmente.** Destino de chamada sem registro é descartado silenciosamente: `app/core/medida_turno_do_par.py:314`. Prova em memória: `parear_turnos(): return sumida()` manteve resultado normal. Portanto exige as duas raízes, não todas as dependências transitivas: `:290`. Chamadas não resolvidas constam entre limitações declaradas em `:39`; integridade completa do grafo não é comprovada.

- Medição reproduzida: A1=6, A2=15, B1=82, B2=171; contadores 9/85, pelos produtores em `app/core/medida_turno_do_par.py:349`, `:350`, `:363`. Parcelas nomeadas apontam para `app/ponto/motor_calculo_v2.py:503`, `:882`, `:2408`. Os únicos consumidores localizados do novo instrumento são os testes, incluindo leitura dos campos em `app/core/tests/test_medida_turno_do_par.py:286`, `:303`, `:365`; integração externa/HOST publicada: **[não verificado]**.

## 07/11 16:47 · sessao 75130e94b46d · - · - · origem=Ronald (tmux)

Achados para consolidação, somente pela cópia:

- **PROCEDENTE: composição pode isentar código que não executa no juiz.** O fechamento percorre qualquer `ast.Call`, sem avaliar alcance, e exclui integralmente toda função alcançada (`app/core/medida_turno_do_par.py:311-325`). Confirmei em memória com `_mini` (`app/core/tests/test_medida_turno_do_par.py:235`): uma função `novo(entrada,xs)` com `range(0,len(xs),2)` e `entrada.date()` produz A1/A2/B1/B2=1/1/1/1; adicionar `from novo import novo; if False: novo()` a `turnos_do_colab` faz tudo virar zero. A suite exige a exclusão transitiva de helper, mas não testa alcance/semântica (`app/core/tests/test_medida_turno_do_par.py:416-431`). O aval colado não explicita autorização desse fechamento irrestrito [não verificado].

- **NÃO PROCEDE, para os dois arquivos novos: juiz de negócio, segundo escritor persistente ou relógio novo.** Instrumento importa apenas `ast`/`os` (`app/core/medida_turno_do_par.py:21-22`), lê arquivos (`:374-375`) e devolve dicionários em memória (`:348-367`). O teste restringe imports a esses dois módulos (`app/core/tests/test_medida_turno_do_par.py:454-464`). A comparação histórica do diff continua [não verificado], sem `.git`.

- **FILA: derivações paralelas já existentes são os RED nomeados.** `turnos_via_autoridade` chama `parear_turnos` com parâmetros próprios (`app/ponto/motor_calculo_v2.py:503-506`); `_turnos_do_juiz` chama `turnos_de_batidas` (`:882`), cuja receita independente copia batidas, marca ecos e chama o pareador (`app/ponto/turnos.py:1515-1533`). O gap escreve `data_jornada = _prev_dj` (`app/ponto/motor_calculo_v2.py:2405-2409`); essa data governa inclusão na competência (`:2429-2432`). São os três itens positivos declarados pelo instrumento (`app/core/medida_turno_do_par.py:57-60`).

- **Autoridade:** o registro vivo atribui a pergunta do turno da batida a `ponto/turnos.py::turnos_do_colab` (`app/core/juizes.py:195-196`). `JUIZES.md` se declara lookup e remete ao vivo (`app/docs/JUIZES.md:7-15`); lista geometria em `turnos.py` (`:20-25`) e cálculo no motor (`:29`). O escritor de `Turno.data_turno` usa `_data_do_turno` e constrói `Turno` (`app/ponto/turnos.py:310-322`).

Inventário de saída/consumo:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| fontes / árvores | `censo` lê fonte, instrumento `:373-375`; parse `:272` | `censo_fontes`, instrumento `:277-289` |
| autoridade | `JUIZES`, `app/core/juizes.py:195-196` | `_autoridade`, instrumento `:232-244`, raiz `:275` |
| `criterio`, `chave`, `linhas` lexicais | instrumento `:329-332` | montagem `:349-351`; testes `:296-301`, `:307-308` | [nao verificado]
| `sitio`, itens nomeados, `linhas` | instrumento `:335-346` | ambos contadores `:349`; testes `:365-381` | [nao verificado]
| `numero` | `len(itens)`, instrumento `:350` | catraca testes `:264-270`; consistência `:294` | [nao verificado]
| `itens` | lexical + nomeados, instrumento `:349-350` | testes `:294-308` | [nao verificado]
| `funcoes` | lista de chaves com multiplicidade, instrumento `:351` | testes `:295`, `:324`, `:332`, `:379`, `:430-431` | [nao verificado]
| classificação | diferença por chave, instrumento `:354-358`, `:362` | testes `:303-316`, `:411-412` | [nao verificado]
| `criterios` | tamanho das listas, instrumento `:363` | testes `:288-289`, `:305`, `:410` | [nao verificado]
| `composicao` | instrumento `:365` | teste `:429`; exclusão interna usa conjunto `:324-325` | [nao verificado]
| `universo_arquivos` | `len(trees)`, instrumento `:366` | teste só exige `>100`, `:291` | [nao verificado]
| `ilegiveis` | constante `[]`, instrumento `:366` | teste `:290` | [nao verificado]
| `nao_ve` | constante instrumento `:31-53`, saída `:351`, `:366` | teste compara saída interna `:299`; leitor externo [não verificado] | [nao verificado]
| `esperado_no_fim` | constante 0, instrumento `:352` | teste `:300`; não é teto executado pela catraca `:264-270` | [nao verificado]
| `contador` | rótulos instrumento `:350`, `:360-361` | leitor externo [não verificado] |
| registro esperado | testes `:31-129` | testes `:264-270`, `:288-298` | [nao verificado]
| `CLASSIFICAR` | testes `:131-228` | nenhuma referência adicional localizada na cópia [não verificado] | [nao verificado]

`funcoes` tem significado de parcelas, não conjunto de funções únicas: a documentação declara multiplicidade (`app/core/medida_turno_do_par.py:10-13`) e o leitor compara essa mesma lista (`app/core/tests/test_medida_turno_do_par.py:295`). `linhas` mede sítios internos, enquanto `numero` mede funções por critério mais sítios nomeados (`app/core/medida_turno_do_par.py:48-49`, `:349-351`); não há consumidor mostrado confundindo essas unidades.

## 07/11 16:47 · sessao 321d2aab3083 · - · - · origem=Ronald (tmux)

**PROCEDENTE — há mutante sobrevivente na proveniência.** Em `app/core/medida_turno_do_par.py:332`, trocar:

```python
'criterio': criterion, 'linhas': lines
```

por:

```python
'criterio': criterion, 'linhas': [node.lineno]
```

mantém os totais e identidades, mas publica a linha da definição em lugar das ocorrências A1/B1. Exemplo: `_alinhar_por_papel` passa a apontar `app/escala/utils.py:502`, embora o `range(..., 2)` esteja em `app/escala/utils.py:520`. O teste exige somente lista não vazia (`app/core/tests/test_medida_turno_do_par.py:301`); a verificação de deslocamento das linhas cobre apenas os nomeados (`app/core/tests/test_medida_turno_do_par.py:380`).

Executei todos os **18 métodos** com esse mutante em memória: **18 passaram, zero falhas/erros**. Como Django está ausente, substituí somente `django.test.SimpleTestCase` por `unittest.TestCase`; execução pela infraestrutura Django oficial **[nao verificado]**. [nao verificado]

Os 13 `test_MORDE` têm estas mordidas delimitadas, sem provar completude do instrumento:

| Teste, linha no arquivo de testes | O que efetivamente cobra | [nao verificado]
|---|---|
| `:310` candidato amplo | Novos A2/B2 publicados e totais preservados (`:314–316`). | [nao verificado]
| `:318` pareador novo | Um `range` novo aumenta contador e provoca `SUBIU` (`:322–326`). |
| `:328` dia novo | `copia = entrada; copia.date()` aumenta contador e provoca `SUBIU` (`:330–334`). |
| `:336` número curado | **Altera diretamente o resultado**, diminuindo `numero`; cobra `_catraca`, sem curar fonte nem exercitar detector (`:339–342`). |
| `:344` renomear âncora | Renomeia cada função nomeada e exige `LookupError` (`:349–351`). |
| `:353` sintaxe inválida | Cobra `SyntaxError` e nome do arquivo (`:354–356`). |
| `:358` leitura/travessia | Cobra erro de `open` e raiz inexistente; não injeta `UnicodeDecodeError` nem erro no meio de `os.walk` (`:359–363`). |
| `:374` linhas/alias | Usa aliases já presentes no `_mini`; desloca fonte e verifica linhas **nomeadas** (`:375–381`; aliases em `:243`, `:247`). |
| `:383` cura dos nomeados | Preserva âncoras, remove seus corpos e exige zero (`:385–392`). |
| `:394` critérios lexicais | Cobre um `zip`, um `append`, um `localdate` e um `.date()` civil (`:397–414`). |
| `:416` composição | Cobra helper importado excluído e função independente do mesmo arquivo contada (`:419–431`). |
| `:433` aninhamento | Cobra que a função interna responda pelo B1 (`:435–439`). |
| `:441` autoridade ausente | Remove dois arquivos inteiros e exige erro (`:442–447`). |

**NÃO PROCEDE** dizer que os selos são inteiramente decorativos: há incremento e rejeição explícita em `app/core/tests/test_medida_turno_do_par.py:322` e `:325`, assim como em `:330` e `:333`. **PROCEDENTE** dizer que não asseguram a fidelidade das linhas publicadas, pelo sobrevivente acima (`app/core/medida_turno_do_par.py:332`; testes `:301`, `:380`).

## 07/11 16:34 · sessao 076a0a8f57fe · FIO-E0-V4-E2 · verificador · origem=Code

**Há dois bloqueios para a equivalência universal:** o delta passa a compartilhar uma Empresa mutável entre FKs e antecipa leituras que N chamadas unitárias podem dispensar. São consequências do código; incidência em produção e resultado da suíte completa: [nao verificado]. Referências: [juizes.py:977](app/chamados/juizes.py:977), [juizes.py:997](app/chamados/juizes.py:997).

1. **Empresa compartilhada no cache das FKs — bloqueante, introduzido no delta.**

   `in_bulk` fornece um objeto por PK, e `set_cached_value` instala **esse mesmo objeto** em cada colaborador frio daquela empresa, fora do request compartilhado. Portanto, depois do lote, dois colaboradores distintos podem ter `a.empresa is b.empresa`. Alterar somente `a.empresa.dia_inicio_competencia` também altera o que `b.empresa` responde. N chamadas unitárias com instâncias frias independentes não compartilham esse objeto entre chamadas. O próprio teste novo considera legítima a alteração do corte somente em memória. [juizes.py:987](app/chamados/juizes.py:987), [juizes.py:997](app/chamados/juizes.py:997), [test_e2_tarefa_do_dia.py:1224](app/chamados/tests/test_e2_tarefa_do_dia.py:1224).

   **Quem escreve:** o juiz escreve o cache da FK em `997` e o memo em `995`, `998` e `1000`. **Quem lê depois:** o teste lê diretamente `c.empresa`; existe também leitor de produção após `pergunta_viva`, em `vigia_fio`. Esse leitor de produção já usa `select_related`, portanto sua existência não prova incidência desse defeito ali. [juizes.py:995](app/chamados/juizes.py:995), [test_e2_tarefa_do_dia.py:1252](app/chamados/tests/test_e2_tarefa_do_dia.py:1252), [vigia_fio.py:807](app/chamados/services/vigia_fio.py:807), [vigia_fio.py:813](app/chamados/services/vigia_fio.py:813).

2. **Leitura antecipada quebra lote = N unitárias — bloqueante para a promessa “toda combinação”.**

   Contraexemplo com request inicialmente vazio: primeiro par com objeto não-modelo que possui `empresa`; segundo par com outro objeto não-modelo, mesmo `empresa_id`, mas sem atributo `empresa`. O lote lê ambos antes de preencher o memo e lança `AttributeError` no segundo. Nas unitárias, a primeira preenche o memo do request; a segunda encontra a chave e dispensa a leitura do atributo, podendo responder normalmente. Isso foi reproduzido em memória com as funções extraídas e dependências simuladas; execução com ORM real: [nao verificado]. O caminho está em [juizes.py:943](app/chamados/juizes.py:943), [juizes.py:977](app/chamados/juizes.py:977), [juizes.py:993](app/chamados/juizes.py:993), [juizes.py:1038](app/chamados/juizes.py:1038).

   Para objetos ordinários, atributos completos e estado estável, o delta resolve os casos apresentados de FK quente, FK fria e `empresa=None`; isso não estabelece equivalência universal de exceções nem dos objetos deixados nos caches. [juizes.py:942](app/chamados/juizes.py:942), [juizes.py:994](app/chamados/juizes.py:994).

3. **A Empresa efetiva tem precedência de memo — limite residual, não regressão nova de v2.**

   A seleção efetiva por par é esta. [juizes.py:978](app/chamados/juizes.py:978), [juizes.py:993](app/chamados/juizes.py:993):

   | Situação | Empresa que decide a janela |
   |---|---|
   | Chave já no memo | A Empresa do memo, mesmo que a FK carregue outra |
   | Sem memo, FK quente | A Empresa daquela FK |
   | Sem memo, não-modelo | Seu atributo `empresa`, inclusive `None` |
   | Sem memo, FK fria encontrada | O objeto retornado pelo `in_bulk` |
   | PK ausente do `in_bulk` | A leitura direta `c.empresa`, inclusive sua exceção |

   Referências dos ramos: [juizes.py:942](app/chamados/juizes.py:942), [juizes.py:943](app/chamados/juizes.py:943), [juizes.py:995](app/chamados/juizes.py:995), [juizes.py:997](app/chamados/juizes.py:997), [juizes.py:1000](app/chamados/juizes.py:1000).

   No request, o memo pertence ao request e a chave de Empresa contém somente `empresa_id`. Assim, dois colaboradores com cortes em memória 21/22 podem ser julgados pelo primeiro corte. **As unitárias no mesmo request também fazem isso**: não seria correto apresentar esse caso, sozinho, como divergência lote/unitárias. Empresas com IDs diferentes têm chaves distintas. [juizes.py:992](app/chamados/juizes.py:992), [juizes.py:1038](app/chamados/juizes.py:1038).

   Em `CargaDaTarefa`, os pares usam os colaboradores recarregados em `_cadastro`; a Empresa quente do objeto entregue pelo chamador não é necessariamente a usada pelo juiz, e o aquecimento ocorre nessa instância interna. [tarefa_do_dia.py:317](app/chamados/services/tarefa_do_dia.py:317), [tarefa_do_dia.py:366](app/chamados/services/tarefa_do_dia.py:366).

4. **Queries: constância condicionada — não é bloqueio novo por si só.**

   O desenho faz uma chamada `in_bulk`, uma consulta por chave nova **empresa × competência**, mais as etapas em lote do toque. Portanto, não é constante para qualquer crescimento de colaboradores/dias: novas empresas ou competências aumentam as consultas de tranca. Os testes comparam uma empresa e dias da mesma competência. [juizes.py:987](app/chamados/juizes.py:987), [juizes.py:1002](app/chamados/juizes.py:1002), [fechamento.py:1463](app/ponto/services/fechamento.py:1463), [test_e2_tarefa_do_dia.py:1146](app/chamados/tests/test_e2_tarefa_do_dia.py:1146).

   **Há caminhos de acesso individual:** se o `in_bulk` omitir a Empresa e a leitura direta conseguir resolvê-la, `1000` pode consultar por colaborador; se a FK estiver realmente quebrada, a chamada falha no primeiro acesso correspondente. Além disso, no request compartilhado, depois que o primeiro colaborador preenche o memo, os demais pulam `set_cached_value`: leituras posteriores dessas FKs podem custar uma consulta por colaborador ainda frio. Não atribuo esse N+1 à execução de `.tarefa()` sem demonstrar tal leitura nela. [juizes.py:993](app/chamados/juizes.py:993), [juizes.py:997](app/chamados/juizes.py:997), [juizes.py:1000](app/chamados/juizes.py:1000).

5. **Testes novos: não identifiquei vácuo — não bloqueante; cobertura incompleta.** [nao verificado]

   O teste dos cortes exige explicitamente `[True, False]`; o não-modelo exige ausência de SQL de Empresa; o aquecimento usa colaborador recém-buscado e exige zero consultas no acesso seguinte. Eles verificam efeitos relevantes, mas não cobrem compartilhamento entre duas FKs frias nem o memo compartilhado de request. [test_e2_tarefa_do_dia.py:1230](app/chamados/tests/test_e2_tarefa_do_dia.py:1230), [test_e2_tarefa_do_dia.py:1239](app/chamados/tests/test_e2_tarefa_do_dia.py:1239), [test_e2_tarefa_do_dia.py:1249](app/chamados/tests/test_e2_tarefa_do_dia.py:1249).

   Nos mutantes novos do Caso10, o detector encontra a infração no próprio mutante: `getattr` pela regra de chamada e `**dict` pela chave constante. A verificação AST isolada encontrou a fonte-base limpa e ambos os mutantes detectados; portanto, eles não passam apoiados numa infração prévia. [test_e2_tarefa_do_dia.py:1278](app/chamados/tests/test_e2_tarefa_do_dia.py:1278), [test_e2_tarefa_do_dia.py:1282](app/chamados/tests/test_e2_tarefa_do_dia.py:1282), [test_e2_tarefa_do_dia.py:1311](app/chamados/tests/test_e2_tarefa_do_dia.py:1311).

6. **Os três descartados — nenhum demonstra bloqueio novo do delta, mas duas justificativas precisam de ressalva.**

   - **Linhas de “Cobrar” versus `exists()`:** mesma contagem de consultas não significa mesmo volume. Não há `LIMIT`; o SQL usa o menor `desde` do lote e pode trazer outros dias e linhas anteriores à tranca específica de um item. A comparação individual preserva o resultado, mas a justificativa de volume estritamente limitado é excessiva. Impacto real de volume: [nao verificado]. [juizes.py:1151](app/chamados/juizes.py:1151), [juizes.py:1159](app/chamados/juizes.py:1159), [juizes.py:1166](app/chamados/juizes.py:1166).
   - **`self._trancado` como retrato da carga:** o descarte é coerente com a materialização na construção. Porém, “nunca entre requests” não é imposto pelo código: a reutilização verifica igualdade de `agora`, sem vínculo com request ou atualização da tranca. [tarefa_do_dia.py:367](app/chamados/services/tarefa_do_dia.py:367), [tarefa_do_dia.py:402](app/chamados/services/tarefa_do_dia.py:402), [tarefa_do_dia.py:420](app/chamados/services/tarefa_do_dia.py:420).
   - **Trancado sem célula:** descarte sustentado pelo código. A tranca retorna sem ações antes do ramo `sem_celula`, conforme a regra registrada no próprio teste. [tarefa_do_dia.py:187](app/chamados/services/tarefa_do_dia.py:187), [tarefa_do_dia.py:196](app/chamados/services/tarefa_do_dia.py:196), [test_e2_tarefa_do_dia.py:1048](app/chamados/tests/test_e2_tarefa_do_dia.py:1048).

## 07/11 16:32 · sessao bffeb73bbc60 · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

(a) Escritos:

- [Instrumento puro AST](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:1).
- [Selo SimpleTestCase e inventários](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:1).

(b) Medição registrada nesta cópia:

| Contador | Número | Lista integral `arquivo::funcao` |
|---|---:|---|
| Pareadores | **9 = A1(6) + 3 nomeados** | [REGISTRO](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:33) |
| Derivações de dia | **85 = B1(82) + 3 nomeados** | [REGISTRO](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:42) |

“A classificar pela regra”: **A2−A1=9; B2−B1=89**, com [listas integrais publicadas](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:131). Candidatos novos dessas listas não travam a catraca ([caso explícito](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:310)). [nao verificado]

Referência fornecida `ffbda473`: **6/15/83/172**; cópia medida no universo `app/`: **6/15/82/171**. A diferença corresponde ao recorte que exclui `bin/`; a base informada está registrada no [instrumento](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:4). HEAD independente: [nao verificado].

(c) [`NAO_VE` integral](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:31), publicado junto de cada número: homônimos e ausência de tipos; fluxo e origem interprocedural; chamadas dinâmicas, `getattr` e aliases; limites da composição estática; código fora do universo; formas sintáticas não reconhecidas; novos sítios dentro de função já contada; reescritas dos sítios nomeados. [nao verificado]

(d) Localização por função AST:

- `turnos_via_autoridade`: chamada resolvida para `ponto.turnos.parear_turnos`.
- `MotorBase._turnos_do_juiz`: chamada resolvida para `ponto.turnos.turnos_de_batidas`.
- `MotorBase.calcular_mes`: atribuição `data_jornada = _prev_dj` sob comparação de `_gap_h`.

As [âncoras](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:57) não dependem de linhas. Renomear uma função âncora provoca [erro explícito](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:334).

(e) Executei **18 casos no HOST**, substituindo `SimpleTestCase` por `unittest.TestCase` somente em memória, sem falhas. Desativar A1, B1, proteção das âncoras ou erro de parse produziu RED esperado. Certificação pela suite Django: **[nao verificado]**; a tentativa parou por ausência de `django`. [nao verificado]

(f) Comando da suite e comandos individuais dos quatro mutantes exigidos ([casos](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:318)): [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test core.tests.test_medida_turno_do_par

turno_selo=core.tests.test_medida_turno_do_par.CatracaTurnoDoParTest

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test "$turno_selo.test_MORDE_pareador_novo_sobe_contador_1_e_catraca_fica_RED"

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test "$turno_selo.test_MORDE_dia_novo_sobe_contador_2_e_catraca_fica_RED"

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test "$turno_selo.test_MORDE_renomear_cada_funcao_nomeada_falha_explicitamente"

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test "$turno_selo.test_MORDE_arquivo_invalido_e_RED_mesmo_fora_do_escopo_do_motor"
```

## 07/11 16:20 · sessao 504cc4b15e21 · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: a cura permanece parcial. PROCEDENTES as classes (1), (2) e (4); NAO PROCEDE a permanência dos defeitos enumerados nas classes (3) e (5). Persistem falhas de alcance dos selos e uma cegueira nova no I22.** (`app/chamados/services/fio.py:1081`; `app/chamados/models.py:2019`; `app/chamados/juizes.py:932`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:202`; `app/chamados/services/fio.py:1172`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1046`)

Identidade independente do commit/pai, ocorrência dos contraexemplos em produção, reprodução em banco, RED→GREEN histórico e suíte inteira `rc=0`: **[nao verificado]**. [nao verificado]

**1. Classe (1) — PROCEDENTE: as portas ainda podem escolher canônicas diferentes, por divergência de universo.**

O contraexemplo anterior do horizonte foi corrigido: `aplicar` escolhe a canônica antes de recortar as perdedoras que pode escrever. **NAO PROCEDE sua permanência nessa forma.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:226`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:227`)

Entretanto, `universo_do_grupo` não é idempotente. Contraexemplo estático: mesmo colaborador, motivo e dia no horizonte; `L` sem episódio, muda e menor PK; `A` do episódio α, muda; `B` do episódio β, respondida. `do_fato(α)` entrega `[L,A]`, e `abrir(α)` segura `L`. Episódios distintos são fatos distintos. (`app/chamados/models.py:2565`; `app/chamados/models.py:2567`; `app/chamados/models.py:2691`; `app/chamados/models.py:2692`; `app/chamados/services/fio.py:1240`)

O agrupador também entrega `[L,A]`, mas `aplicar` expande esse grupo novamente. Ao visitar `L`, sem episódio, `universo_do_grupo` chama o ramo de `do_fato` por dia, que inclui `B`. O apply passa a escolher `B` e retrata `A`: **a expansão de uma linha legada uniu episódios diferentes**. (`app/chamados/services/fio.py:1120`; `app/chamados/services/fio.py:1126`; `app/chamados/services/fio.py:1079`; `app/chamados/services/fio.py:1081`; `app/chamados/models.py:2571`; `app/chamados/models.py:2598`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:221`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`)

Assim, DRY e apply podem divergir; abrir e migração usam diretamente `do_fato`, enquanto backfill/dedup aplicam a expansão adicional. **NAO PROCEDE segundo implementador da preferência resposta/menor PK localizado; PROCEDENTE derivação paralela do universo entregue ao juiz.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:359`; `app/chamados/management/commands/dedup_perguntas_fato.py:70`; `app/chamados/services/fio.py:1248`; `app/chamados/services/fio.py:1255`; `app/chamados/services/fio.py:984`)

**2. Classe (2) — PROCEDENTE pelas ordens residuais; NAO PROCEDE outro `select_for_update` explícito de produção localizado para as quatro entidades.**

As quatro aquisições explícitas estão em `trava_na_ordem`. Os dois caminhos apontados na volta anterior receberam antecipação das travas. (`app/chamados/models.py:2513`; `app/chamados/models.py:2520`; `app/chamados/models.py:2533`; `app/chamados/models.py:2537`; `app/chamados/models.py:290`; `app/chamados/services/materializacao.py:676`)

Persistem, porém:

- **Pergunta → Disputa:** `remover_motivo` abre atomic, carimba a Pergunta — com `save` real — e depois salva a Disputa, sem antecipar a ordem. Os receivers de reativação de Posto e calibração de âncora chegam diretamente a esse método. (`app/chamados/models.py:2011`; `app/chamados/models.py:2019`; `app/chamados/models.py:2911`; `app/chamados/models.py:2023`; `app/colaboradores/signals.py:71`; `app/colaboradores/signals.py:77`; `app/escala/signals.py:155`; `app/escala/signals.py:161`)
- **Chamado → Disputa por chamadas sucessivas ao próprio dono:** a aprovação de justificativa trava Colaborador e Chamados sem incluir as Disputas abertas. Depois, o fechamento da origem chama `retratar`, chega ao reconciliar e adquire as Disputas abertas. Um Chamado elegível ao fechamento e outra Disputa aberta da mesma pessoa bastam para alcançar essa ordem. (`app/ponto/services/justificativa.py:123`; `app/ponto/services/justificativa.py:134`; `app/ponto/services/justificativa.py:211`; `app/ponto/services/justificativa.py:39`; `app/ponto/services/justificativa.py:44`; `app/chamados/reconciliador.py:523`; `app/chamados/models.py:655`; `app/chamados/models.py:694`)

Portanto, centralizar a aquisição não garante a ordem global entre chamadas: uma chamada posterior pode adquirir linhas de tabela anterior ainda não travadas. (`app/ponto/services/justificativa.py:123`; `app/chamados/models.py:694`)

**3. Classe (3) — NAO PROCEDE a permanência dos defeitos anteriores enumerados.**

O atomic engloba o processamento; as travas precedem a conferência do horizonte empresarial, mérito e exportada. `_ent` agora é reiniciado por colaborador. O horizonte da escrita dos grupos e o veto dos `sem_ato` também são lidos nesse envelope. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:202`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:208`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:211`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:227`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:239`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:252`)

**NAO PROCEDE mistura de escrita persistida e recusa no retorno normal:** qualquer recusa levanta exceção dentro do atomic; o retorno posterior zera os contadores. `abaixo_do_horizonte` é informativo, e o dedup corretamente o exclui das recusas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:278`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `app/chamados/management/commands/dedup_perguntas_fato.py:91`)

**NAO PROCEDE segundo escritor do backfill localizado:** os commands preparam o DRY e encaminham a escrita para `aplicar`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:401`; `app/chamados/management/commands/dedup_perguntas_fato.py:89`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:256`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`)

Exclusão mútua contra entrega concorrente da competência: **[nao verificado]**; `competencia_entregue` faz leitura simples, o que não invalida, por si, a conferência literal exigida dentro do atomic. (`app/folha/export.py:581`)

**4. Classe (4) — PROCEDENTE: significado declarado, mas censo incompleto e leitores de decisão incompatíveis.**

Pergunta de fato usa **dia do turno**, com `conversa=False`; conversa usa **dia civil da emissão**, com `conversa=True`. O proxy e `resolvedora_valida` agora respeitam essa distinção. (`app/chamados/models.py:2631`; `app/chamados/models.py:2642`; `app/chamados/models.py:2673`; `app/chamados/juizes.py:1969`)

O censo lista `_dia_da_disputa`, `dias_do_fato`, prévia, copiloto e diagnóstico, mas conclui “Nenhum deles escreve”. Essa conclusão não considera os consumidores que decidem a escrita. (`app/chamados/services/fio.py:1034`)

Há provas concretas:

- **Leitor censado alimenta escrita:** `dias_do_fato` inclui datas das conversas; o arquivamento usa seu resultado para decidir pelo corte e depois silenciar o Chamado. Conversa encerrada não impede o caminho, pois a guarda procura pergunta viva. (`app/chamados/juizes.py:209`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:102`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:121`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:158`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:288`)
- **Escritor fora do censo:** `fabrica_da_celula.perguntar` seleciona por motivo/data sem distinguir conversa e chama a porta que pode descarimbar a pergunta. Os filtros posteriores também não excluem conversa. (`app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/services/relavra_via.py:179`; `app/chamados/services/relavra_via.py:234`; `app/chamados/services/relavra_via.py:244`)
- **Decisores fora do censo:** `dia_da_pergunta_trancado` transforma essa data em competência e pode matar a pergunta antes da resposta; `_dias_das_perguntas_vivas` transforma a data civil em fato usado por `competencia_do_chamado`. (`app/chamados/juizes.py:932`; `app/chamados/juizes.py:945`; `app/chamados/juizes.py:1141`; `app/chamados/juizes.py:2037`; `app/chamados/juizes.py:2058`; `app/chamados/services/competencia_trancada.py:92`)

Também falta o leitor de classificação `pergunta_no_app.retencao`, que pode tomar a conversa civil como pergunta do dia do furo. (`app/chamados/services/pergunta_no_app.py:92`)

**5. Classe (5) — NAO PROCEDE no leitor de “novas perguntas” localizado.**

Ele lê `data_evento` da Pergunta e compara com o dia local da aplicação da 0052. Ausência do registro ou dia nulo alarma; o escape dos fatos anteriores está declarado. Não usa `Disputa.aberta_em`. (`app/chamados/services/fio.py:1158`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1168`; `app/chamados/services/fio.py:1172`)

**6. Selos — PROCEDENTE insuficiência de alcance; NAO PROCEDE ausência geral de mordida.**

| Selo | Burla que permanece |
|---|---|
| A — trava | O detector reconhece SQL literal `FOR UPDATE`, mas a varredura só o chama se houver `select_for_update` no arquivo. Um arquivo contendo somente SQL cru passa. O mutante testa o detector isolado, sem esse filtro. (`app/chamados/tests/test_e1_v7_catracas.py:120`; `app/chamados/tests/test_e1_v7_catracas.py:238`; `app/chamados/tests/test_e1_v7_catracas.py:277`) |
| A — trava | `lock = qs.select_for_update; lock()` não é chamada de atributo nem o `getattr` reconhecido. (`app/chamados/tests/test_e1_v7_catracas.py:100`; `app/chamados/tests/test_e1_v7_catracas.py:116`) |
| B — canônica | Argumento nomeado recortado escapa: o detector examina somente `n.args[0]`. `min`, `sorted()[0]` e QuerySet filtrado continuam no limite declarado. (`app/chamados/tests/test_e1_v7_catracas.py:185`; `app/chamados/tests/test_e1_v7_catracas.py:26`) |
| C — escrita | `gravar = p.save; gravar()` escapa: há resolução de alias de importação, mas não dessa atribuição. Helper externo segue no limite declarado. (`app/chamados/tests/test_e1_v7_catracas.py:208`; `app/chamados/tests/test_e1_v7_catracas.py:217`; `app/chamados/tests/test_e1_v7_catracas.py:28`) |

A/B percorrem `app` com exclusões; C examina os dois commands. Esse alcance não equivale a uma garantia semântica sobre todo o repositório. (`app/chamados/tests/test_e1_v7_catracas.py:37`; `app/chamados/tests/test_e1_v7_catracas.py:43`; `app/chamados/tests/test_e1_v7_catracas.py:194`)

Os mutantes têm expectativas que mordem as formas sintéticas escolhidas, inclusive homônimos e aliases. **Não demonstram o caminho completo**, como mostra o SQL cru filtrado antes do detector. (`app/chamados/tests/test_e1_v7_catracas.py:270`; `app/chamados/tests/test_e1_v7_catracas.py:277`; `app/chamados/tests/test_e1_v7_catracas.py:340`; `app/chamados/tests/test_e1_v7_catracas.py:383`)

Quanto aos REDs v8:

- Canônica exige duas retratações, preservação de p1, restauração do índice e idempotência. Porém usa uma única batida e não chama efetivamente abrir/migração; não cobre a legada que conecta episódios distintos. (`app/chamados/tests/test_e1_v8.py:50`; `app/chamados/tests/test_e1_v8.py:65`; `app/chamados/tests/test_e1_v8.py:72`; `app/chamados/tests/test_e1_v8.py:77`)
- Ordem exige presença das tabelas e ausência de violações, mas o observador registra somente a primeira operação por **tabela**. Uma aquisição posterior de outra PK desaparece; uma consulta registrada também não prova que encontrou linha. (`app/chamados/tests/test_e1_v8.py:108`; `app/chamados/tests/test_e1_v8.py:121`; `app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:46`)
- Conversa verifica nascimento aceito e fato cruzado recusado isoladamente; não testa a composição do I22. (`app/chamados/tests/test_e1_v8.py:138`; `app/chamados/tests/test_e1_v8.py:148`)

Execução Django desses REDs e comprovação histórica da mordida: **[nao verificado]**.

**7. Escritores e leitores — PROCEDENTE incompatibilidade residual de datas e imprecisão de `abaixo_do_horizonte`.**

| Campo/estado | Quem escreve | Quem lê / significado |
|---|---|---|
| `via_resolucao` | `_escrever_carimbo`, pelas portas de carimbo/descarimbo; lote tem UPDATE; exceções existentes são `encerrar_por_fato_no_espelho` e `relavrar`. (`app/chamados/models.py:2818`; `app/chamados/models.py:2983`; `app/chamados/models.py:2992`; `app/chamados/services/relavra_via.py:123`) | C6, identidade e backfill leem via de resolução/identidade. Duplicata é terminal antes da resposta; a documentação corrigida concorda com isso. (`app/chamados/juizes.py:1148`; `app/chamados/models.py:2569`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/models.py:2893`) |
| “retratada” | É estado representado pela via: dedup grava `VIA_DUPLICATA`; remoção de motivo grava `VIA_MOTIVO_RETRATADO`. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2019`) | `fora_do_fato` exclui da identidade; C6 torna terminal quando carimbada. A resposta permanece histórica. (`app/chamados/models.py:2922`; `app/chamados/juizes.py:1148`; `app/chamados/models.py:2894`) |
| `data_evento` | Nascimento; preenchimento no reapontamento; rechaveamento. (`app/chamados/models.py:2739`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:948`) | Identidade/horizonte usam turno; leitores do item 4 ainda recebem dia civil sem distinguir. **Significado incompatível entre consumidores.** (`app/chamados/models.py:2599`; `app/chamados/services/fio.py:1043`; `app/chamados/juizes.py:932`) |
| `conversa` | As portas fixam `False`/`True`; `_nascer` persiste e agora transmite ao proxy. (`app/chamados/models.py:2631`; `app/chamados/models.py:2642`; `app/chamados/models.py:2673`; `app/chamados/models.py:2740`) | Identidade, índice e resolvedora usam “sem fato”. Os leitores do item 4 ignoram o discriminador. (`app/chamados/models.py:2599`; `app/chamados/models.py:3207`; `app/chamados/juizes.py:1969`) |
| `abaixo_do_horizonte` | `aplicar` acumula perdedoras fora de `q_do_horizonte`; retorno deduplica PKs. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:230`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`) | CLI chama de perdedoras do acervo abaixo; dedup corretamente não interpreta como recusa. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:419`; `app/chamados/management/commands/dedup_perguntas_fato.py:92`) |

O último significado é mais amplo que o anunciado: uma BATIDA perdedora com `data_evento=None` também entra em `abaixo_do_horizonte`. O agrupador permite esse NULL, embora o censo o distinga como `batida_sem_dia`. **Não escreve a linha, mas classifica “sem dia” como “abaixo”.** (`app/chamados/services/fio.py:1021`; `app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1111`; `app/chamados/services/fio.py:1148`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:230`)

**8. Reincidências — PROCEDENTES universo paralelo, ordem de trava, cobertura insuficiente e relógio não injetado; NAO PROCEDE segundo juiz de preferência ou segundo escritor do backfill demonstrado.** (`app/chamados/services/fio.py:1081`; `app/chamados/models.py:2023`; `app/chamados/tests/test_e1_v7_catracas.py:238`; `app/chamados/services/fio.py:978`; `app/chamados/management/commands/dedup_perguntas_fato.py:90`)

O relógio residual alcança **teste novo**: apesar de declarar todos os instantes literais/injetados, ele chama `ch.fechar` sem congelamento, chegando a `timezone.now()`. As novas fixtures também criam Disputa com `aberta_em=auto_now_add`. Portanto contar zero `now()` literal no diff não demonstra injeção. (`app/chamados/tests/test_e1_v8.py:12`; `app/chamados/tests/test_e1_v8.py:106`; `app/chamados/models.py:450`; `app/chamados/models.py:244`; `app/chamados/tests/test_e1_v8.py:104`; `app/chamados/models.py:1715`)

**NAO PROCEDE subida dos cinco TETOS sem frase C8:**

| Cenário | Teto / aumento | Referência |
|---|---:|---|
| Estado sem pergunta | 27 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:116` |
| Reconciliar fecha | 59 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:118` |
| Uma cobrança fecha | 208 / +4 | `app/ponto/tests/test_selo_chokepoint_escrita.py:161` |
| Três cobranças fecham | 262 / +8 | `app/ponto/tests/test_selo_chokepoint_escrita.py:162` |
| Três perguntas, sem cruzar | 10 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:193` |

A justificativa é compatível com **duas consultas adicionais por entrada**: Disputas e Chamados. Isso satisfaz a exigência documental da C8, mas não prova tempo ou contenção constantes: o conjunto de linhas bloqueadas cresce com Disputas abertas e Chamados vivos. Medições citadas nos comentários: **[nao verificado]**. (`PLANO_PISCADA.md:10`; `app/chamados/models.py:2518`; `app/chamados/models.py:2520`; `app/chamados/models.py:2528`; `app/chamados/models.py:2533`)

A diferença de mérito anteriormente levada a desenho permanece: migração retrata perdedoras sem o veto de `admin_decidiu` com resposta que o backfill aplica. (`app/chamados/services/fio.py:1265`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`)

**9. `resolvedora_valida` — PROCEDENTE cegueira do I22; NAO PROCEDE transformação automática da entrada factual em conversa ou cegueira equivalente no I8b.**

`abrir` força `conversa=False`, e o proxy preserva esse valor. A mudança não desvia essa porta para o retorno antecipado da conversa. (`app/chamados/models.py:2631`; `app/chamados/models.py:2673`; `app/chamados/juizes.py:1969`)

O contraexemplo do **I22 não exige flag corrompida**: Chamado vivo com perguntas de fato de outro turno e uma conversa legítima apontada para ele. A conversa agora devolve `True`; o `any(...)` fica verdadeiro e o I22 deixa de acusar que nenhuma pergunta **de fato** resolve aquele turno. (`app/chamados/models.py:2642`; `app/chamados/juizes.py:1969`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1043`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1046`)

O **I8b continua examinando cada pergunta viva individualmente**: a conversa irmã não impede que uma pergunta factual cruzada seja rejeitada pelo juiz. Os REDs novos cobrem a recusa individual, mas não a composição que mascara o I22. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891`; `app/chamados/management/commands/auditar_invariantes_chamados.py:896`; `app/chamados/tests/test_e1_v8.py:148`)

_2775 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
