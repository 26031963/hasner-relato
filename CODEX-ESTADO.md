# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 11:32** · publicado em **28/11/2026 11:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 11:32 · sessao 5209f6acefbe · E4C-F5-INSTANTE · leitura · origem=Code

## 1. Selo `core.observ` com outro import de `core`

As âncoras abaixo têm contagem **1** na a2 reconstruída em memória. `V` e `R` indicam os vereditos na cópia fornecida, na a2 reconstruída e com a cura. A equivalência Git da cópia com `486a1ffc4`, a execução de `486a1ffc4~1`, a suíte Django, o Ruff e a conferência pela sessão estão [nao verificado]. Rodei os métodos selecionados com `unittest.TestCase` e os analisadores reais. Os vermelhos foram `AssertionError`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:78–112` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165` · `app/chamados/tests/test_e4c_f5_c4_red.py:1557–1562` · `app/ruff.toml:4–6`

Em `_dicts_da_trilha`, substitua o bloco `else` de dois versos da a2 pelo trecho abaixo. Um `import core.X` sem alias mantém o vínculo com o pacote `core`. Um alias explícito continua sujeito à regra de religamento. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–109`

```diff
-                else:
+                elif not (isinstance(n, ast.Import) and not a.asname and a.name.startswith('core.')):
                     religados.add(nome)
```

Insira antes de `class SeloEscritorUnicoDoCarimbo(SimpleTestCase):`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:263`

```python
def _fonte_trilha_com_dois_imports_core(campo):
    return (
        'import core.observ\n'
        'import core.models\n'
        'def f():\n'
        f'    core.observ.evento(depois=dict({campo}=None))\n'
    )


```

No par, substitua o fecho do import da a2. `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30`

```diff
     _dicts_da_trilha,
+    _fonte_trilha_com_dois_imports_core,
 )
```

No carimbo, insira antes de `def test_MORDE_dict_em_variavel_defaults_construtor_e_fields_em_variavel(self):`. No par, insira antes de `def test_MORDE_repasse_de_param_na_classe_do_modelo(self):`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:333` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:269`

```python
    def test_CONTROLE_trilha_com_dois_imports_core(self):
        self.assertEqual(escritas_do_carimbo(_fonte_trilha_com_dois_imports_core('materializada_em')), [])

```

```python
    def test_CONTROLE_trilha_com_dois_imports_core(self):
        self.assertEqual(escritas_do_par(_fonte_trilha_com_dois_imports_core('respondida_em')), [])

```

Nos dois selos, `test_CONTROLE_trilha_com_dois_imports_core` dá **486 V · base V esperada [nao verificado] · a2 R · cura V**. O mutante **M2-core** produz um achado onde cada teste exige `[]`. Original e mutado, contagem **1** na cura. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165`

```diff
-                elif not (isinstance(n, ast.Import) and not a.asname and a.name.startswith('core.')):
+                else:
                     religados.add(nome)
```

## 2. Ligações em campos `str` nos dois selos

Em `_dicts_da_trilha`, insira imediatamente antes de `    if 'dict' in religados:` da a2. Incluí os três campos `name` de parâmetros de tipo. A imagem da aplicação usa Python 3.13. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` · `app/Dockerfile:1`

```python
        elif isinstance(n, (ast.ExceptHandler, ast.MatchAs, ast.MatchStar)) and n.name is not None:
            religados.add(n.name)
        elif isinstance(n, ast.MatchMapping) and n.rest is not None:
            religados.add(n.rest)
        elif isinstance(n, (ast.Global, ast.Nonlocal)):
            religados.update(n.names)
        elif isinstance(n, (ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)):
            religados.add(n.name)
```

Insira antes de `def _fonte_trilha_com_dois_imports_core(campo):`, introduzido no item 1. No par, insira `    _casos_trilha_ligacao_em_campo_str,` imediatamente depois de `    _casos_trilha_evento_religado,` do import da a2. O `nonlocal` usa um import legítimo na função externa, de modo que retirar a regra de `Nonlocal` derruba o teste mesmo com a regra de parâmetros ativa. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112,263` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30`

```python
def _casos_trilha_ligacao_em_campo_str(campo):
    for nome in ('evento', 'dict'):
        inicio = (
            'from core.observ import evento\n'
            f'def g():\n    evento(depois=dict({campo}=None))\n'
        )
        chamada = f'evento(depois=dict({campo}=None))'
        casos = (
            ('ExceptHandler.name', f'def f():\n    try:\n        pass\n    except E as {nome}:\n', 8, '        '),
            ('MatchAs.name', f'def f(x):\n    match x:\n        case {nome}:\n', 7, '            '),
            ('MatchStar.name', f'def f(x):\n    match x:\n        case [*{nome}]:\n', 7, '            '),
            ('MatchMapping.rest', f'def f(x):\n    match x:\n        case {{**{nome}}}:\n', 7, '            '),
            ('Global.names', f'def f():\n    global {nome}\n', 6, '    '),
            ('Nonlocal.names',
             f'def f():\n    from core.observ import evento as {nome}\n'
             f'    def interna():\n        nonlocal {nome}\n', 8, '        '),
            ('TypeVar.name', f'def f[{nome}]():\n', 5, '    '),
            ('ParamSpec.name', f'def f[**{nome}]():\n', 5, '    '),
            ('TypeVarTuple.name', f'def f[*{nome}]():\n', 5, '    '),
        )
        for caso, corpo, linha, recuo in casos:
            yield caso, nome, inicio + corpo + recuo + chamada + '\n', linha


```

Insira os métodos nas mesmas âncoras de classe do item 1, carimbo e par respectivamente. Cada teste exige o achado no sítio afetado e em `g`, antes do religamento. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:222–225,333` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165,269`

```python
    def test_RED_trilha_acusa_ligacao_em_campo_str(self):
        for caso, nome, fonte, linha in _casos_trilha_ligacao_em_campo_str('materializada_em'):
            achados = escritas_do_carimbo(fonte)
            alvo = 'f.interna' if caso == 'Nonlocal.names' else 'f'
            for funcao, numero in ((alvo, linha), ('g', 3)):
                with self.subTest(caso=caso, nome=nome, funcao=funcao):
                    self.assertIn((funcao, numero, 'dict(materializada_em=)'), achados)

```

```python
    def test_RED_trilha_acusa_ligacao_em_campo_str(self):
        for caso, nome, fonte, linha in _casos_trilha_ligacao_em_campo_str('respondida_em'):
            achados = escritas_do_par(fonte)
            alvo = 'f.interna' if caso == 'Nonlocal.names' else 'f'
            for funcao, numero in ((alvo, linha), ('g', 3)):
                with self.subTest(caso=caso, nome=nome, funcao=funcao):
                    self.assertIn((funcao, numero, 'dict(respondida_em=)'), achados)

```

Nos dois selos, `test_RED_trilha_acusa_ligacao_em_campo_str` dá **486 R · base R esperada [nao verificado] · a2 R · cura V**. Cada mutante abaixo tem original de contagem **1** na cura e derruba quatro asserções por selo. `\n` indica uma quebra literal. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112,222–225` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165`

| Mutante | Original exato | Mutado exato |
|---|---|---|
| `ExceptHandler.name` | `(ast.ExceptHandler, ast.MatchAs, ast.MatchStar)` | `(ast.MatchAs, ast.MatchStar)` |
| `MatchAs.name` | `(ast.ExceptHandler, ast.MatchAs, ast.MatchStar)` | `(ast.ExceptHandler, ast.MatchStar)` |
| `MatchStar.name` | `(ast.ExceptHandler, ast.MatchAs, ast.MatchStar)` | `(ast.ExceptHandler, ast.MatchAs)` |
| `MatchMapping.rest` | `"        elif isinstance(n, ast.MatchMapping) and n.rest is not None:\n            religados.add(n.rest)\n"` | `""` |
| `Global.names` | `(ast.Global, ast.Nonlocal)` | `ast.Nonlocal` |
| `Nonlocal.names` | `(ast.Global, ast.Nonlocal)` | `ast.Global` |
| `TypeVar.name` | `(ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)` | `(ast.ParamSpec, ast.TypeVarTuple)` |
| `ParamSpec.name` | `(ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)` | `(ast.TypeVar, ast.TypeVarTuple)` |
| `TypeVarTuple.name` | `(ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)` | `(ast.TypeVar, ast.ParamSpec)` |

Atualize os dois mutantes da a2 cujas âncoras mudaram. `M2-import-global` mata `test_RED_trilha_acusa_import_local_homonimo` e `M2-sem-parametro` mata esse teste e `test_RED_trilha_acusa_dict_religado`. Em ambos os selos, esses dois testes dão **486 R · base R esperada [nao verificado] · a2 V · cura V**. `M2-grafia-dict` conserva o bloco exato da a2 e mata também o novo RED nos casos de `dict`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112,222–225` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165`

```diff
@@ M2-import-global
                 elif not (isinstance(n, ast.Import) and not a.asname and a.name.startswith('core.')):
-                    religados.add(nome)
+                    pass
```

`M2-sem-parametro` troca `"        elif isinstance(n, ast.arg):\n            religados.add(n.arg)\n"` por `""`, com contagem **1**. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:94–99`

## 3. Censo com uma tabela de ligações e registro de projeção não resolvida

Cada escopo passa a ter uma tabela `ligacoes`. A coleta registra uma vez os vínculos sintáticos. A inferência legada de modelo escreve a origem nas mesmas entradas, e a projeção lê os vínculos lexicais dessas entradas. A resolução segue atribuições diretas, aliases e desempacotamento literal. Para `for` e `with`, a ligação local encobre a externa e o valor projetado fica indeterminado. Projeção indeterminada gera `save` com `entidade=None`, no balde existente, e `projecao=?` na expressão. Isso inclui `projecao=None` explícita. O delta de produção acrescenta zero `except`. `app/core/censo_escritas.py:200–217,300–377,421–485,561–565` · `app/chamados/models.py:4789–4803`

Em `app/core/censo_escritas.py`, insira `from collections import deque` imediatamente depois de `import os`. Insira o bloco abaixo imediatamente antes de `class _Varredor(ast.NodeVisitor):`. Cada âncora tem contagem **1**. `app/core/censo_escritas.py:58–59,199–211`

```python
_NOS_DE_ESCOPO = (
    ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef, ast.Lambda,
    ast.ListComp, ast.SetComp, ast.DictComp, ast.GeneratorExp,
)


class _Ligacao:
    def __init__(self):
        self.vinculos = []  # (ordem, local, valor da projecao, evidencia do modelo)
        self.campos = set()
        self.origem = None


```

No `__init__` de `_Varredor`, substitua o intervalo de `        self._funcao = []` até antes de `        # E3-CENSO-APELIDO` por esta linha. Substitua depois o intervalo de `    # -- escopos` até antes de `    def visit_ClassDef(self, no):` pelo bloco completo. Cada delimitador tem contagem **1**. `app/core/censo_escritas.py:209–217,300–319`

```python
        self._escopos = []  # pilha de (no do escopo, ligacoes)
```

```python
    # -- escopos
    def _ligar_alvo(self, ligacoes, alvo, valor, origem, local, ordem):
        if isinstance(alvo, ast.Name):
            ligacoes.setdefault(alvo.id, _Ligacao()).vinculos.append((ordem, local, valor, origem))
        elif isinstance(alvo, ast.Starred):
            self._ligar_alvo(ligacoes, alvo.value, None, None, local, ordem)
        elif isinstance(alvo, (ast.Tuple, ast.List)):
            elts = ()
            if (isinstance(valor, (ast.Tuple, ast.List)) and len(alvo.elts) == len(valor.elts)
                    and not any(isinstance(e, ast.Starred) for e in (*alvo.elts, *valor.elts))):
                elts = valor.elts
            for i, item in enumerate(alvo.elts):
                evid = origem if i == 0 and isinstance(alvo, ast.Tuple) and isinstance(item, ast.Name) else None
                self._ligar_alvo(ligacoes, item, elts[i] if elts else None, evid, local, ordem)

    def _colher_ligacoes(self, no, inferir):
        # A origem do modelo usa a ordem em largura e pode ler escopos filhos,
        # como antes. A tupla usa apenas os vinculos lexicais do escopo.
        ligacoes = {}
        fila = deque((sub, True, True) for sub in ast.iter_child_nodes(no))
        ordem = 0
        while fila:
            sub, local, direta = fila.popleft()
            ordem += 1
            if not local and not inferir:
                continue
            filhos_locais = local and not isinstance(sub, _NOS_DE_ESCOPO)
            fila.extend((filho, filhos_locais, direta and isinstance(sub, ast.Expr))
                        for filho in ast.iter_child_nodes(sub))
            alvos, valor, origem = (), None, None
            if isinstance(sub, ast.Assign):
                alvos, valor, origem = sub.targets, sub.value, sub.value
                for alvo in alvos:
                    if (inferir and isinstance(alvo, ast.Attribute) and isinstance(alvo.value, ast.Name)
                            and alvo.attr in self.unicos):
                        ligacoes.setdefault(alvo.value.id, _Ligacao()).campos.add(self.unicos[alvo.attr])
            elif isinstance(sub, ast.AnnAssign) and sub.value is None:
                if local and isinstance(sub.target, ast.Name):
                    self._ligar_alvo(ligacoes, sub.target, sub, None, local, ordem)
            elif isinstance(sub, (ast.AnnAssign, ast.NamedExpr)):
                alvos, valor, origem = (sub.target,), sub.value, sub.value
            elif isinstance(sub, ast.AugAssign):
                alvos, origem = (sub.target,), sub.value
            elif isinstance(sub, (ast.For, ast.AsyncFor, ast.comprehension)):
                alvos, origem = (sub.target,), sub.iter
            elif isinstance(sub, ast.withitem) and sub.optional_vars is not None:
                alvos, origem = (sub.optional_vars,), sub.context_expr
            elif isinstance(sub, (ast.Delete, ast.TypeAlias)):
                alvos = sub.targets if isinstance(sub, ast.Delete) else (sub.name,)
            for alvo in alvos:
                self._ligar_alvo(ligacoes, alvo, valor if direta else None, origem, local, ordem)
            if not local:
                continue
            nomes = ()
            if isinstance(sub, (ast.Import, ast.ImportFrom)):
                nomes = (a.asname or a.name.split('.')[0] for a in sub.names)
            elif isinstance(sub, (ast.Global, ast.Nonlocal)):
                nomes = sub.names
            elif isinstance(sub, ast.arg):
                nomes = (sub.arg,)
            elif isinstance(sub, ast.MatchMapping):
                nomes = (sub.rest,) if sub.rest is not None else ()
            elif isinstance(sub, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef, ast.ExceptHandler,
                                  ast.MatchAs, ast.MatchStar, ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)):
                nomes = (sub.name,) if sub.name is not None else ()
            for nome in nomes:
                ligacoes.setdefault(nome, _Ligacao()).vinculos.append((ordem, True, None, None))
        return ligacoes

    def _inferir_amarras(self, ligacoes):
        # Ponto fixo sobre os mesmos vinculos: qs = X.objects.filter(); for b in qs: b.save().
        evidencias = sorted(
            ((ordem, nome, valor) for nome, ligacao in ligacoes.items()
             for ordem, _, _, valor in ligacao.vinculos if valor is not None),
            key=lambda item: item[0],
        )
        mudou = True
        while mudou:
            mudou = False
            for _, nome, valor in evidencias:
                ligacao = ligacoes[nome]
                if ligacao.origem:
                    continue
                origem = self._origem(valor)
                if not origem and isinstance(valor, ast.Name) and valor.id in ligacoes:
                    origem = ligacoes[valor.id].origem
                    origem = None if origem == '__nova__' else origem
                if origem:
                    ligacao.origem = origem
                    mudou = True
        for ligacao in ligacoes.values():
            if not ligacao.origem and len(ligacao.campos) == 1:
                ligacao.origem = ('campo', next(iter(ligacao.campos)))

    def _visitar_escopo(self, no, inferir=False):
        ligacoes = self._colher_ligacoes(no, inferir)
        if inferir:
            self._inferir_amarras(ligacoes)
        self._escopos.append((no, ligacoes))
        self.generic_visit(no)
        self._escopos.pop()

    visit_Module = visit_Lambda = _visitar_escopo
    visit_ListComp = visit_SetComp = visit_DictComp = visit_GeneratorExp = _visitar_escopo

```

Em `visit_ClassDef`, aplique a primeira troca abaixo. Em `_visit_funcao`, substitua o intervalo de `        self._imports_locais.append(locais)` até antes de `        self._imports_locais.pop()` pelas duas linhas indicadas. Aplique também as trocas em `_no_registrar_da_decisao` e `_variavel`. Cada bloco ou delimitador tem contagem **1**. `app/core/censo_escritas.py:315–377,435–440,460–474`

```diff
@@ visit_ClassDef
-        self._visitar_escopo_da_projecao(no)
+        self._visitar_escopo(no)
         self._classe.pop()
@@ _no_registrar_da_decisao
-        if len(self._escopos_da_projecao) != 3:
+        if len(self._escopos) != 3:
             return False
-        _, (classe, _), (metodo, _) = self._escopos_da_projecao
+        _, (classe, _), (metodo, _) = self._escopos
@@ _variavel
-            for amarras in reversed(self._funcao):
-                origem = amarras.get(rec.id)
+            for _, ligacoes in reversed(self._escopos):
+                ligacao = ligacoes.get(rec.id)
+                origem = ligacao.origem if ligacao is not None else None
```

```python
        self._imports_locais.append(locais)
        self._visitar_escopo(no, inferir=True)
```

Substitua o intervalo de `    def _projecao(self, no, valor):` até antes de `        inst, campos = valor.elts` por este bloco. Cada delimitador tem contagem **1**. `app/core/censo_escritas.py:442–454`

```python
    def _valor_da_projecao(self, valor):
        limite = len(self._escopos) - 1
        while isinstance(valor, (ast.Name, ast.NamedExpr)):
            if isinstance(valor, ast.NamedExpr):
                valor = valor.value
                continue
            for i in range(limite, -1, -1):
                escopo, ligacoes = self._escopos[i]
                if isinstance(escopo, ast.ClassDef) and i != limite:
                    continue
                ligacao = ligacoes.get(valor.id)
                if ligacao is not None:
                    locais = [v for _, local, v, _ in ligacao.vinculos if local]
                    if locais:
                        break
            else:
                return None
            locais = [v for v in locais if not isinstance(v, ast.AnnAssign)]
            if len(locais) != 1 or locais[0] is None:
                return None
            ligado = locais[0]
            if (ligado.end_lineno, ligado.end_col_offset) > (valor.lineno, valor.col_offset):
                return None
            valor, limite = ligado, i
        return valor

    def _projecao(self, no, valor):
        """`projecao=(inst, campos)` = `inst.save(update_fields=campos)` neste sitio (`ESCRITA_POR_PROJECAO`)."""
        valor = self._valor_da_projecao(valor)
        if (not isinstance(valor, ast.Tuple) or len(valor.elts) != 2
                or any(isinstance(e, ast.Starred) for e in valor.elts)):
            self._anotar(no, None, 'save', '%s(projecao=?) [projecao]' % _texto(no.func))
            return
```

Insira os dois REDs em `CensoLeAProjecaoTest`, antes de `    def test_os_dois_saves_da_porta_ficam_visiveis(self):`, contagem **1**. `for` e `with` incluem uma tupla externa válida para que apagar a ligação local produza uma classificação errada e derrube a asserção. `app/chamados/tests/test_e4c_f5_c4_red.py:1557–1562,1576` · `app/core/censo_escritas.py:308–313,333–356,442–454`

```python
    def test_RED_projecao_resolve_ligacoes_lexicais(self):
        inicio = 'from chamados.models import DecisaoPergunta as DP\n'
        tupla = "(p, ['respon

[truncado em 20000 de 29418 caracteres]

## 28/11 11:30 · sessao 20970be2aa1e · C7-TELA-ADMIN-ENCERRA · leitura · origem=Code

A porta atual **encerra a disputa inteira**. Ela pode carimbar, no mesmo ato, perguntas que a célula ainda sustenta e perguntas que perderam o marco previsto. Para a obra que pede encerramento seletivo, falta uma decisão de escopo antes de ligar o botão. `app/chamados/models.py:2122`, `app/chamados/models.py:2184`, `app/chamados/services/disputa_emissao.py:1110`, `app/chamados/services/disputa_emissao.py:1167`

### 1. Porta e caso 40588

- `admin_encerra(user, motivo)` aplica `strip()`, rejeita motivo vazio com `ValueError` e chama `fechar` com `resolucao=texto` e `motivo_fechamento='admin_encerra'`. Para uma disputa aberta, `fechar` exige `user`. A permissão de staff/admin precisa ser aplicada na borda HTTP. `app/chamados/models.py:2094`, `app/chamados/models.py:2210`, `app/chamados/services/acoes_disputa.py:284`
- A guarda lê **todas** as perguntas da disputa. Uma resposta com instante e texto, sem validação nem materialização e fora de dia trancado, barra o ato inteiro. O `ValueError` nomeia as perguntas e manda validar ou recusar as respostas na mesa. A porta relê a disputa e repete a guarda sob trava. `app/chamados/juizes.py:1644`, `app/chamados/models.py:2041`, `app/chamados/models.py:2122`, `app/chamados/models.py:2151`, `app/chamados/models.py:2163`
- Para cada pergunta que o C6 considera viva, a porta tenta gravar `via_resolucao='admin_decidiu'`, `materializada_em` e `materializada_por`. Quando o carimbo grava, registra `DecisaoPergunta.SILENCIO` com autor, instante do carimbo, foto da resposta e motivo limitado a 240 caracteres, além de `pergunta_encerrada_com_a_disputa`. Depois grava `fechada_em`, `fechada_por`, `resolucao` e `motivo_fechamento` na disputa, emite `disputa_fechada` e retorna `True`. Os eventos de `LogAuditoria` são best-effort. O carimbo de identidade tem guardas próprias e pode devolver `False`, caso em que aquele `SILENCIO` e evento da pergunta são pulados. `app/chamados/models.py:2132`, `app/chamados/models.py:2184`, `app/chamados/models.py:2199`, `app/chamados/models.py:3372`, `app/chamados/models.py:3544`, `app/core/observ.py:94`, `app/core/observ.py:133`
- Mesma disputa, mesmo autor, mesmo motivo após `strip()` e mesmo código de fechamento devolvem `False` sem nova escrita ou trilha. Entrada diferente sobre disputa já fechada levanta `ValueError`. Se o fechamento concorrente só aparece na releitura sob a trava, devolve `False` sem regravar. `app/chamados/models.py:2094`, `app/chamados/models.py:2168`, `app/chamados/tests/test_e4_expand_orm.py:64`
- **61520/61521** entram no carimbo se estiverem vivas pelo C6 nas respectivas disputas abertas no instante do ato, se nenhuma irmã barrar o fechamento e se o carimbo gravar. A sonda do enunciado informa 88 vivas na comp. 11, 68 da emp2 e 20 da emp3, e descreve a emissão às 12:36 e a regeneração da célula 122468 às 12:42. PKs das disputas, lista integral de irmãs e estado atual das guardas **[nao verificado]**. `app/chamados/models.py:2122`, `app/chamados/models.py:2175`, `app/chamados/models.py:2184`
- **Disputa mista é possível.** O emissor reaproveita uma disputa aberta do colaborador e agrega nela perguntas de intervalo e de outros marcos ou chamados. O laço de encerramento usa o C6 sobre a disputa inteira. A falta de intervalo no DNA atual, por si só, não seleciona o que será carimbado. `app/chamados/services/disputa_emissao.py:1110`, `app/chamados/services/disputa_emissao.py:1167`, `app/chamados/services/disputa_emissao.py:1180`, `app/chamados/juizes.py:1551`, `app/chamados/models.py:2184`

**lei:** O aval permite encerrar a disputa inteira, inclusive uma irmã viva ainda sustentada pela célula, ou a irmã sustentada deve permanecer aberta? Se deve permanecer, qual porta existente o aval autoriza para o ato seletivo? `admin_encerra` recebe a disputa e o motivo, sem alvo por pergunta. `app/chamados/models.py:2184`, `app/chamados/models.py:2210`

### 2. Chamado 40588 e retenção do dia

- **Chamado.** O fechamento da disputa dispara `reconciliar()` para o chamado-pai vinculado. O chamado-destino da pergunta pode ser outro. O motor, a premissa, a conversa do pai e as perguntas vivas participam da decisão de fechar o chamado. O carimbo também emite evento da célula quando ela é localizada. O consumidor consulta o lastro de marcos **acesos**, e o cartório tem um ramo próprio para marcos **inexistentes** que também passa por `reconciliar()`. Se 40588 é pai ou destino e se de fato fecha após o clique **[nao verificado]**. `app/chamados/signals.py:19`, `app/chamados/signals.py:244`, `app/chamados/models.py:691`, `app/chamados/models.py:3600`, `app/chamados/services/fio.py:97`, `app/ponto/services/lastro.py:394`, `app/ponto/services/cartorio.py:1600`
- **Dia e folha.** `motivos_retencao_celula` lê o veredito e os protestos da `CelulaDia`. Considera dia passado sem decisão, vínculo e janela, isenção de ponto, desligamento, protestos integralmente não cobráveis e a exceção de furo só de pausa. Mudança de estado do chamado pode provocar nova lavra pelo cartório. `classificar_export` decide o TXT para a competência inteira, lê a retenção do espelho e aplica os demais crivos. Veredito e protestos da 122468 após o ato, outros dias retidos e resultado do TXT **[nao verificado]**. `app/folha/export.py:774`, `app/folha/export.py:984`, `app/folha/export.py:1189`, `app/folha/export.py:1221`, `app/folha/export.py:1267`, `app/folha/export.py:1292`, `app/folha/export.py:1329`, `app/chamados/signals.py:189`

**Texto ao admin** — “A disputa foi encerrada e o motivo ficou registrado. Confira no espelho se o dia ainda tem pendência para a folha.” `app/chamados/models.py:2199`, `app/folha/export.py:1192`

### 3. Mesa atual e desenho mínimo

| Casca | Caminho na cópia |
|---|---|
| `base.html` | A mesa de validação está na Central, `/chamados/painel/`. O painel carrega `/chamados/modal/<colaborador_id>/`, `modal_fio_colaborador`, com `chamados/partials/modal_fio.html`, onde ficam Validar e Rejeitar. A Central liga também à ficha completa `/chamados/colaborador/<colaborador_id>/`, `fio_colaborador`, com `chamados/fio_colaborador.html`. Há ainda ações no detalhe `/chamados/local/<chamado_id>/`, `detalhe_chamado_local`. `app/config/urls.py:28`, `app/chamados/urls.py:14`, `app/chamados/urls.py:21`, `app/chamados/urls.py:39`, `app/chamados/views.py:698`, `app/chamados/views.py:928`, `app/templates/chamados/painel_gestao.html:1`, `app/templates/chamados/painel_gestao.html:490`, `app/templates/chamados/painel_gestao.html:528`, `app/templates/chamados/partials/modal_fio.html:306`, `app/templates/chamados/detalhe_local.html:310` |
| `base_app.html` | `/chamados/meu-atendimento/`, `meu_atendimento`, renderiza `chamados/meu_atendimento.html` com `base_tpl`. É o fio do colaborador logado e passa `is_admin_ti=False`. Os templates do painel e do fio administrativo fixam `base.html`. Na cópia, a mesa administrativa fica nessa casca. `app/chamados/urls.py:16`, `app/chamados/views.py:559`, `app/chamados/views.py:633`, `app/chamados/views.py:650`, `app/templates/chamados/meu_atendimento.html:1`, `app/templates/chamados/fio_colaborador.html:1`, `app/templates/chamados/painel_gestao.html:1`, `app/core/context_processors.py:194` |

**Desenho proposto, condicionado ao aval de disputa inteira.** Link “Disputas” na ficha completa para uma página administrativa do colaborador, com um formulário simples por disputa aberta. A página usa `base_tpl`, identifica a disputa e mostra suas perguntas antes do envio. Isso cobre também disputas que compartilham um chamado-destino, pois o cartão do fio elege uma única disputa por chamado. Campo `motivo` com rótulo e `required`, botão **Encerrar disputa com motivo**, `{% csrf_token %}` e POST dedicado com `disputa_pk` explícito. GET e POST exigem sessão e `is_staff` ou grupo `admin_ti`, seguindo a ação administrativa vizinha. O POST carrega a disputa pelo PK e pelo colaborador da rota e chama a porta uma vez, inclusive no reenvio de disputa fechada. `app/chamados/services/fio.py:136`, `app/chamados/views.py:708`, `app/chamados/services/acoes_disputa.py:274`, `app/core/context_processors.py:194`, `app/config/settings/base.py:59`, `app/chamados/models.py:2094`

**lei:** O veto a `except` abrange a tradução de `ValueError` na borda HTTP? A porta levanta `ValueError` para motivo vazio, resposta sem veredito e fechamento anterior divergente. `resposta_acao` e `resposta_render` recebem uma mensagem pronta. O precedente da reabertura administrativa captura `ValueError` ao redor da chamada. Com o veto literal, a exibição da recusa precisa de um contrato autorizado antes de implementar o POST. `app/chamados/models.py:2094`, `app/chamados/models.py:2124`, `app/chamados/models.py:2214`, `app/core/respostas.py:20`, `app/core/respostas.py:74`, `app/chamados/services/acoes_disputa.py:299`

### 4. Censo

- **`admin_encerra`** tem zero chamadores diretos de produção na busca estática da cópia. Há chamadas de teste para motivo vazio, carimbo e trilha, repetição, corrida e cascata. `app/docs/BACKLOG.md:1640`, `app/chamados/tests/test_c7_red_minimos.py:250`, `app/chamados/tests/test_e4_expand_orm.py:64`, `app/chamados/tests/test_f5_contrato_v1_guarda_sob_trava.py:198`, `app/chamados/tests/test_guarda_disputa_status.py:69`
- **`DisputaSupervisao.fechar`**, chamadas diretas de produção. No modelo, `admin_encerra` e `remover_motivo`. Nas exceções, desligamento e competência trancada. No fluxo ordinário, becos, emissão e saída registrada, materialização de ausência e validação, três caminhos do reconciliador, foto recebida, entrada sincronizada e o comando de perguntas com fato. `app/chamados/models.py:2217`, `app/chamados/models.py:2342`, `app/colaboradores/services/desligamento.py:167`, `app/chamados/services/competencia_trancada.py:232`, `app/chamados/services/becos.py:61`, `app/chamados/services/disputa_emissao.py:1298`, `app/chamados/services/disputa_emissao.py:2224`, `app/chamados/services/materializacao.py:1159`, `app/chamados/services/materializacao.py:2160`, `app/chamados/reconciliador.py:415`, `app/chamados/reconciliador.py:719`, `app/chamados/reconciliador.py:1314`, `app/api/foto_recebida.py:126`, `app/ponto/services/entrada_sincronizada.py:60`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:89`
- **`motivo_fechamento`** é lido pelo próprio `fechar` na idempotência e pelo contador `disputa_vazia`, que filtra `FECHOU_VAZIA`. O inventário da sombra classifica a coluna como código de choices. Nenhum leitor de template apareceu na busca estática. `app/chamados/models.py:2094`, `app/chamados/services/vigia_fio.py:736`, `bin/sombra_regras_pessoais.py:175`
- **Selos de front.** Modal nasce oculto e preenchido, alcançável pelas duas cascas. A varredura de templates verifica também que o overlay tem um único dono. O selo de popover varre todos os `.html` e pega `<details>` de ação com lado fixo sem os atributos do componente. O smoke Chromium das duas cascas mede overlay e console, e há selo de toast nas duas. O smoke B8 procura POST em teste por heurística, cujo padrão atual de escrita não inclui `.admin_encerra(`. O smoke de clique desta obra precisa exercitar o botão explicitamente. `app/core/tests/test_selo_modal_nao_nasce_vazio.py:40`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:98`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:121`, `app/core/tests/test_selo_popover_acao.py:46`, `app/core/tests/test_selo_popover_acao.py:63`, `app/core/tests/test_selo_popover_acao.py:97`, `app/core/tests/test_smoke_chromium.py:172`, `app/core/tests/test_toast_le_o_motivo.py:100`, `app/core/tests/test_contract_smoke_porta.py:22`, `app/core/tests/test_contract_smoke_porta.py:64`, `app/docs/GRADE-MESTRA.md:650`

### 5. Arquivos em voo

- **Zero interseção com o anexo fornecido** para a proposta de quatro arquivos. Editar `app/chamados/urls.py` e `app/templates/chamados/fio_colaborador.html`. Criar `app/chamados/views_disputa_admin.py` e `app/templates/chamados/disputa_admin.html`. A Central já chega à ficha completa pelo botão “Ficha completa”. `app/chamados/urls.py:21`, `app/templates/chamados/fio_colaborador.html:1`, `app/templates/chamados/painel_gestao.html:257`, `app/templates/chamados/painel_gestao.html:490`
- Editar a porta no modelo conflita com as listas `486a1ffc4` e `c9fded10a` do anexo. Plugar o botão diretamente pelo modal, seu roteador ou seus cards toca `views.py`, `services/fio.py`, `modal_fio.html`, `_pergunta_card.html` ou `_card_atendimento.html`, listados em `f300431af`. Ponta atual das ramas e hash Git da cópia **[nao verificado]**. `app/chamados/models.py:2210`, `app/chamados/views.py:870`, `app/chamados/services/fio.py:657`, `app/templates/chamados/partials/modal_fio.html:306`, `app/templates/chamados/detalhe_local.html:310`, `app/templates/chamados/meu_atendimento.html:127`

### 6. RED para o Opus

| Caso | Asserção |
|---|---|
| Acesso e CSRF | POST de usuário autenticado sem staff/admin recebe `403`, com zero chamadas à porta. POST autorizado sem token recebe `403` com CSRF habilitado no cliente de teste. O endpoint de escrita só aceita POST. `app/chamados/services/acoes_disputa.py:284`, `app/chamados/views.py:960`, `app/config/settings/base.py:59` |
| Motivo vazio e resposta sem veredito | Enviar campo ausente, vazio e só com espaços. Ver a recusa, com zero fechamento, carimbo, decisões novas e eventos do ato. Repetir com uma irmã respondida sem veredito e conferir que o texto da porta aparece e identifica a pergunta. `app/chamados/models.py:2041`, `app/chamados/models.py:2124`, `app/chamados/models.py:2214`, `app/chamados/tests/test_c7_red_minimos.py:250` |
| POST válido e repetição | Espionar a porta real e provar uma chamada por POST com a disputa e o usuário corretos. No primeiro, verificar campos da disputa, carimbo, `SILENCIO` e eventos. Repetir com o mesmo ator e motivo, passando de novo pela porta, e provar `False`, mesmos carimbos e mesmas contagens. `app/chamados/models.py:2094`, `app/chamados/models.py:2184`, `app/chamados/tests/test_c7_red_minimos.py:261`, `app/chamados/tests/test_e4_expand_orm.py:64` |
| Duas cascas | Renderizar e submeter como admin em sessão comum e em sessão `via_app`. Conferir a casca efetiva, o mesmo formulário e o feedback. Colaborador comum fica sem a ação. `app/core/tests/test_casca_telas_colab.py:35`, `app/core/context_processors.py:194`, `app/templates/base.html:334`, `app/templates/base_app.html:92` |
| Nenhuma regra na view | Reusar os AST C7 que verificam juiz paralelo e escrita fora da entidade. No RED da nova view, provar a delegação única e vedar decisão sobre DNA, C6 ou retenção, escrita de estado e relógio. O AST existente de exceções verifica captura ampla que engole defeito, com alcance menor que o veto literal desta obra. `app/chamados/tests/test_c7_red_minimos.py:525`, `app/chamados/tests/test_c7_red_minimos.py:563`, `app/chamados/tests/test_c7_red_minimos.py:593`, `app/core/tests/test_contract_excecoes_engolidas.py:64` |

### 7. Brief para o construtor

Após os dois cortes de `lei:`, implementar somente código de `app/`, nos quatro caminhos da seção 5. Na ficha completa, acrescentar o acesso administrativo à lista de disputas do colaborador. Na página com `base_tpl`, identificar o alcance do ato e enviar `motivo` por POST com CSRF e PK explícito. A view fina autentica, autoriza, busca a disputa e chama `disputa.admin_encerra(request.user, motivo)` uma vez. Exibir a recusa pelo contrato autorizado e tratar `False` como nenhuma alteração. A trilha vem da porta. **Proibido** regra na view ou no template, `except`, relógio solto e juiz novo. Testes ficam com o Opus. `app/chamados/views.py:698`, `app/core/context_processors.py:194`, `app/chamados/models.py:2094`, `app/chamados/models.py:2184`, `app/chamados/models.py:2210`, `app/chamados/tests/test_c7_red_minimos.py:593`

Auditoria somente por leitura da cópia e do anexo. Execução dos REDs, smoke e efeitos em produção **[nao verificado]**.

## 28/11 11:24 · sessao 108fb1930b64 · E4D-CONTRATO-ROTA-B · leitura · origem=Code

**Caso 20: VERMELHO esperado na igualdade de atos e foto de Q, nos dois horários.** A recusa entra por `validacao.recusar_pergunta` em autocommit, depois da absorção da âncora e antes de abrir a transação de Q. Com o veto por `raise _Reprovada('recusa_humana_vigente')`, a transação de Q pode ser revertida e a recusa continua gravada. `app/chamados/services/materializacao.py:1047–1084,1124–1149` · `app/chamados/services/validacao.py:974–1000`

Conferi em memória as âncoras do texto, compilei os quatro módulos de teste resultantes e conferi os alvos do catálogo. O ambiente falhou ao importar `django.core.management`, e a cópia está sem metadados Git. Execução ORM/HTTP no HEAD, identidade Git e Ruff [nao verificado]. `app/manage.py:7–18` · `app/config/settings_test.py:1–9`

## 1. Recusa commitada antes da transação de Q [nao verificado]

lei: caso 20, grupo 6. A recusa humana vigente prevalece em Q. A passagem preserva o ato de recusa e a foto resultante, sem acrescentar `ABSORCAO` em Q. A escolha de uma identidade persistente de passagem segue aberta no material fornecido. O código já distingue os carimbos da própria passagem por PK em memória, pois duas portas podem carimbar no mesmo instante. `matriz.md:30,79` e `r2.txt:8` conforme o anexo · `app/docs/PROMPTS.md:2950` · `app/chamados/services/materializacao.py:1064–1067`

Em `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py`, substitua a linha `from datetime import datetime, time, timedelta` pelas três primeiras linhas abaixo. Substitua também `from django.db import IntegrityError` pela última linha. Cada âncora ocorre uma vez. `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:2–6`

```python
from contextlib import contextmanager
from datetime import datetime, time, timedelta
from sys import _getframe
```

```python
from django.db import IntegrityError, transaction
```

Insira o helper imediatamente antes de `class PassagemDaAusenciaTest(BaseCortes):` (1). Ele delega toda abertura ao `atomic` real. O callback roda na chamada que vai **criar** o contexto da candidata, antes de entrar nele. A comparação usa o objeto de código em execução, inclusive quando a contraprova troca `__code__`. `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:32` · `app/chamados/services/materializacao.py:1074–1080` · `app/chamados/tests/e4d_matriz_mutantes.py:14–37`

```python
@contextmanager
def antes_do_atomic_da_candidata(numero, interpor):
    atomic_real = transaction.atomic
    codigo = M._materializar_ausencia_de_resposta.__code__
    chamadas = 0

    def atomic_observado(*args, **kwargs):
        nonlocal chamadas
        if _getframe(1).f_code is codigo:
            chamadas += 1
            if chamadas == numero:
                interpor()
        return atomic_real(*args, **kwargs)

    with patch.object(transaction, 'atomic', new=atomic_observado):
        yield
    if chamadas < numero:
        raise AssertionError((numero, chamadas, 'atomic da candidata não alcançado'))


```

## 2. Caso 20 com a asserção da regra em primeiro lugar

Em `app/chamados/tests/test_e4d_matriz_reds.py`, substitua os dois imports abaixo, cada qual com uma ocorrência. `app/chamados/tests/test_e4d_matriz_reds.py:29,38`

```python
# Original
from django.test import RequestFactory, SimpleTestCase
from chamados.tests.cortes_f2_f9_apoio import AGORA, DIA, BaseCortes

# Mutado, cada import no seu sítio
from django.test import RequestFactory, SimpleTestCase, TransactionTestCase
from chamados.tests import test_e4c_f5_a1c_passagem_da_ausencia as a1c
from chamados.tests.cortes_f2_f9_apoio import AGORA, DIA, BaseCortes, criar_empresa_e_colaborador
```

Retire o intervalo que começa em `    def test_caso_20_recusa_para_so_q_e_preserva_irma_ja_absorvida(self):` e termina imediatamente antes de `    def test_caso_24_ancora_fora_deixa_dependente_sem_cobranca(self):` (1 de cada). Insira a classe abaixo antes de `class MatrizAusenciaTest(ApoioMatriz):` (1). O intervalo retirado inclui a exigência de uma assinatura `pedido_id` para passagens de ausência. `app/chamados/tests/test_e4d_matriz_reds.py:1320,1440–1516`

```python
class Caso20RecusaCommitadaTest(TransactionTestCase):
    _escape = a1c.PassagemDaAusenciaTest._escape
    _irma_da_celula = a1c.PassagemDaAusenciaTest._irma_da_celula
    _irma_com_luz = a1c.PassagemDaAusenciaTest._irma_com_luz
    _responder = a1c.PassagemDaAusenciaTest._responder
    _ausencia = a1c.PassagemDaAusenciaTest._ausencia
    atos = staticmethod(ApoioMatriz.atos)

    def setUp(self):
        super().setUp()
        self.relogio = self.enterContext(freeze_time(T0))
        self.enterContext(calado())
        self.push = self.enterContext(patch('chamados.fcm_utils.push_fcm_colaborador', autospec=True)) [nao verificado]
        _, self.colab = criar_empresa_e_colaborador()
        self.admin = User.objects.create_user(username='admin_recusa_commitada')
        self.usuario = User.objects.create_user(username='colab_recusa_commitada')
        self.colab.usuario = self.usuario
        self.colab.save(update_fields=['usuario'])
        self.disputa = DisputaSupervisao.objects.create(colaborador=self.colab)

    def foto(self, p):
        return P.objects.values_list(*RVM).get(pk=p.pk)

    def preparar(self):
        e = self._escape('hora')
        q = self._irma_com_luz()
        self._responder(q, '19:02')
        P.objects.create(
            disputa=self.disputa, celula_id=q.celula_id, data_evento=q.data_evento,
            tipo_resposta='hora', motivo='intervalo_volta', pergunta='A que horas voltou?',
        )
        q.refresh_from_db()
        self.assertEqual(q.celula.veredito, 'concorde')
        self.assertIs(juizes.fato_do_marco_no_espelho(q), True)
        self.assertEqual(self.atos(q), [DP.RESPOSTA])
        self.assertEqual(self.foto(q), (T0, '19:02', None, None, None, None, ''))
        self.relogio.move_to(T1)
        return e, q

    def test_caso_20_recusa_humana_prevalece_em_q_no_empate(self):
        self.verificar_recusa_na_passagem(T1)

    def test_caso_20_recusa_humana_prevalece_em_q_depois_do_relogio(self):
        self.verificar_recusa_na_passagem(T2)

    def verificar_recusa_na_passagem(self, instante):
        e, q = self.preparar()
        motivo = 'Confirme a hora'

        def recusar_q():
            self.assertTrue(connection.get_autocommit())
            self.assertFalse(connection.in_atomic_block)
            self.assertEqual(timezone.now(), T1)
            anterior = e.decisoes.get(ato=DP.ABSORCAO)
            self.assertEqual(
                (anterior.ato_em, anterior.absorcao_escape_id, anterior.absorcao_ausencia_id),
                (T1, e.pk, self._ausencia().pk),
            )
            self.assertEqual(self.atos(q), [DP.RESPOSTA])
            self.relogio.move_to(instante)
            r = validacao.recusar_pergunta(q.pk, self.admin, motivo)
            self.assertEqual((r['ok'], r['estado']), (True, 'recusada'))
            self.assertTrue(connection.get_autocommit())
            self.assertFalse(connection.in_atomic_block)
            recusa = q.decisoes.get(ato=DP.RECUSA)
            self.assertEqual(
                (recusa.ato_em, recusa.ato_por_id, recusa.ato_motivo, recusa.ato_texto),
                (instante, self.admin.pk, motivo, '19:02'),
            )
            self.assertEqual(self.foto(q), VAZIA)

        with a1c.antes_do_atomic_da_candidata(2, recusar_q):
            self._responder(e, 'ATESTADO')

        trilha = list(LogAuditoria.objects.filter(
            acao=materializacao.ACAO_IRMA_VIVA_NA_AUSENCIA,
            modelo='PerguntaDisputa', objeto_id=q.pk,
        ).values_list('valor_depois', flat=True))
        self.assertEqual(
            (self.atos(q), self.foto(q)), ([DP.RESPOSTA, DP.RECUSA], VAZIA),
            f'Regra 20: atos e foto de Q. Trilha da passagem: {trilha!r}',
        )
        self.push.assert_called_once() [nao verificado]

    def test_caso_20_sem_recusa_absorve_q(self):
        e, q = self.preparar()
        self._responder(e, 'ATESTADO')
        self.assertEqual(
            list(q.decisoes.filter(ato=DP.ABSORCAO).values_list(
                'ato_em', 'absorcao_escape_id', 'absorcao_ausencia_id',
            )),
            [(T1, e.pk, self._ausencia().pk)],
            'Controle 20: vínculo de Q sem recusa',
        )
        self.assertTrue(DP.absorcao_vigente(P.objects.get(pk=q.pk)))
        self.push.assert_not_called() [nao verificado]


```

A ordem por `id`, com a âncora promovida pela passagem, coloca Q na segunda tentativa. O teste mostra a absorção da âncora antes da recusa, prova autocommit dos dois lados da porta humana e deixa `agora` da passagem em `T1`. A segunda irmã permanece no universo da fixture. `app/chamados/models.py:3931–3934` · `app/chamados/services/materializacao.py:1047–1084`

A resposta de `19:02` concorda com a luz de `19:00` no controle. Depois da recusa, a composta perdeu a resposta e a foto ficou vazia. O carimbo tentativo de Q, com o marco aceso, permite ao C6 encerrar Q na base. Isso deixa o caminho até `ABSORCAO` exposto ao teste. A trilha real entra na mensagem do asserto e o retorno `recusadas` é descartado pelo chamador de produção. `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:50–64` · `app/chamados/juizes.py:1428–1473,1512–1533,1544–1567` · `app/chamados/models.py:3474–3491,4450–4456` · `app/chamados/services/materializacao.py:1091–1149` · `app/core/observ.py:124–137` · `app/chamados/services/disputa_emissao.py:1595–1600`

## 3. Cura de referência e mutantes B1/B2/B3/B0

**Ensaio da cura.** Em `app/chamados/models.py`, insira antes da âncora composta `    @classmethod\n    def composta(cls, p):` (1). O leitor de referência devolve o ato vigente para que B2 consiga aplicar o relógio errado ao próprio ato. A seleção entre `RECUSA`, `RESPOSTA` e `REABERTURA` segue a ordem da sequência do ato. `app/chamados/models.py:4450–4456,4486–4515`

```python
    @classmethod
    def recusa_humana_vigente(cls, p):
        ultimo = max(
            (d for d in p.decisoes.all() if d.ato in (cls.RECUSA, cls.RESPOSTA, cls.REABERTURA)),
            key=lambda d: (d.ato_em, d.pk),
            default=None,
        )
        return ultimo if ultimo is not None and ultimo.ato == cls.RECUSA else None

```

Em `app/chamados/services/materializacao.py`, insira antes de `                elif pergunta_respondida(leitura) and respondida_em_t1 is None:` (1). O nome do motivo abaixo exercita o caminho `_Reprovada`. A igualdade da regra usa a recusa humana gravada e a foto de Q, independentemente do motivo de auditoria adotado pela cura. `app/chamados/services/materializacao.py:1080,1094–1108,1136–1149`

```python
                elif (recusa := DecisaoPergunta.recusa_humana_vigente(q)) is not None:
                    raise _Reprovada('recusa_humana_vigente')
```

A mesma interface aceita leitura direta pelo banco com `p.decisoes.filter(ato__in=(cls.RECUSA, cls.RESPOSTA, cls.REABERTURA)).order_by('-ato_em', '-pk').first()` no lugar de `max(...)`. Os três mutantes atuam na decisão da passagem. B1 desliga o veto, B2 recorta o ato pelo relógio e B3 lê a recusa da âncora em lugar da de Q. Os pares abaixo ancoram a cura de referência e servem para ambas as formas do leitor, com qualquer texto de motivo. Uma cura que organize a decisão em outra expressão precisa apontar o mutante à expressão correspondente. `app/chamados/models.py:4375–4378,4486–4515,4527–4550` · `app/chamados/services/materializacao.py:1080–1108`

Em `app/chamados/tests/e4d_matriz_mutantes.py`, insira antes de `        '24_sem_veto_de_grupo': (materializacao._materializar_ausencia_de_resposta, (` (1). Cada par contém o original e o mutado exatos. Os originais de B1–B3 têm contagem 1 após a cura de referência. B0 já tem contagem 1 na cópia. `app/chamados/tests/e4d_matriz_mutantes.py:14–37,68–70` · `app/chamados/services/materializacao.py:1100,1128–1137`

```python
        '20_omite_recusa': (materializacao._materializar_ausencia_de_resposta, (
            ('                elif (recusa := DecisaoPergunta.recusa_humana_vigente(q)) is not None:',
             '                elif False:'),
        )),
        '20_corta_recusa_pelo_relogio': (materializacao._materializar_ausencia_de_resposta, (
            ('                elif (recusa := DecisaoPergunta.recusa_humana_vigente(q)) is not None:',
             '                elif ((recusa := DecisaoPergunta.recusa_humana_vigente(q)) is not None\n'
             '                      and recusa.ato_em <= agora):'),
        )),
        '20_esconde_recusa': (materializacao._materializar_ausencia_de_resposta, (
            ('DecisaoPergunta.recusa_humana_vigente(q)', 'DecisaoPergunta.recusa_humana_vigente(_escape)'),
        )),
        '20_veta_q_sem_recusa': (materializacao._materializar_ausencia_de_resposta, (
            ('                        _carimbou = q.pk',
             "                        if q.motivo == 'orfao_14h':\n"
             "                            raise _Reprovada('q_vetada_no_controle')\n"
             '                        _carimbou = q.pk'),
        )),
```

| Teste | Base limpa `f300431af` com o texto | Com B.3 | Sob mutação |
|---|---|---|---|
| Caso 20, empate | VERMELHO em `Regra 20: atos e foto de Q` | VERDE | B1 e B3 VERMELHOS no mesmo asserto. B2 VERDE, pois `ato_em == agora`. `app/chamados/services/materializacao.py:1047,1080–1133` |
| Caso 20, recusa posterior ao relógio | VERMELHO no mesmo asserto | VERDE | B1, B2 e B3 VERMELHOS no mesmo asserto. B2 exclui `T2` da decisão tomada com `agora=T1`. `app/chamados/services/materializacao.py:1047,1080–1133` |
| Caso 20, controle sem recusa | VERDE | VERDE | B0 VERMELHO em `Controle 20: vínculo de Q sem recusa`. A lista de vínculos fica vazia após o rollback. `app/chamados/services/materializacao.py:1128–1140` |

Esses são vereditos esperados. A contraprova começa pelo controle limpo, então B1–B3 param nesse controle na base vermelha. Com a cura, o braço mutado precisa falhar pelo asserto indicado e a contraprova completa deve ficar verde. Execução [nao verificado]. `app/chamados/tests/e4d_matriz_mutantes.py:585–612`

LEI-AKITA: origem=`app/chamados/services/materializacao.py:1080–1108`, testemunha=`app/chamados/models.py:4450–4515`, RED=`Caso20RecusaCommitadaTest`, quem-mais-le=`app/chamados/services/disputa_emissao.py:1595–1600` e leitores de H4 em `:1508–1512,1621–1628`, juízes novos=`recusa_humana_vigente`, grupo 6, padrão=Single Source of Truth. RED ORM e itens dependentes de execução [nao verificado]. `CLAUDE.md:16–39,62–68` · `app/chamados/services/materializacao.py:1143–1149` · `app/chamados/models.py:4527–4550`

## 4. Inversão do teste H4 do Opus [nao verificado]

A expectativa antiga mandava absorver Q mesmo com a recusa gravada e, por consequência, exigia que H4 vetasse a próxima resposta. A inversão exige a recusa preservada, a ausência de absorção e o sucesso da próxima resposta pela entrada real. O teste vizinho que começa com uma absorção confirmada exige o veto H4 nesse outro estado. `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:351–380` · `app/chamados/services/disputa_emissao.py:1508–1512,1621–1628`

Em `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py`, substitua a âncora composta `from django.db import transaction\nfrom django.test import RequestFactory\n` (1) pelo bloco de imports. Substitua de `    def test_RED_recusa_datada_depois_mas_inserida_antes_da_absorcao_recusa(self):` até antes de `    def _ato_posterior_com_carimbo_de_pe(self, ato, instante_do_ato, instante_da_resposta):` (1 de cada). O teste usa a transação externa do `TestCase` e comprova que a gravação da recusa ocorre antes do savepoint da candidata. `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:10–11,114–129,364–382` · `app/chamados/tests/cortes_f2_f9_apoio.py:47–51` · `app/chamados/services/materializacao.py:1074–1080`

```python
from django.db import connection, transaction
from django.test import RequestFactory
from django.utils import timezone
```

```python
    def test_RED_recusa_datada_depois_mas_inserida_antes_da_absorcao_prevalece(self):
        irma, escape = self._irma_com_luz(), self._escape()
        self._responder(irma)
        self.relogio.move_to(T1)
        savepoints = tuple(connection.savepoint_ids)

        def recusar():
            self.assertEqual(tuple(connection.savepoint_ids), savepoints)
            self.assertEqual(timezone.now(), T1)
            self.relogio.move_to(T2)
            r = validacao.recusar_pergunta(irma.pk, self.admin, 'Confirme a hora')
            self.assertEqual((r['ok'], r['estado']), (True, 'recusada'))
            self.assertEqual(tuple(connection.savepoint_ids), savepoints)

        with (patch('chamados.fcm_utils.push_fcm_colaborador', autospec=True) as push, [nao verificado]
              a1c.antes_do_atomic_da_candidata(1, recusar)):
            self._responder(escape, 'ATESTADO')

        fresca = self._ler(irma.pk)
        self.assertEqual(
            list(fresca.decisoes.order_by('pk').values_list('ato', 'ato_em')),
            [(D.RESPOSTA, T0), (D.RECUSA, T2)],
        )
        self.assertEqual(
            (fresca.respondida_em, fresca.resposta_colab, fresca.validada_em, fresca.validada_por_id,
             fresca.materializada_em, fresca.materializada_por_id, fresca.via_resolucao),
            (None, '', None, None, None, None, ''),
        )
        self.assertFalse(D.absorcao_vigente(fresca))
        self.relogio.move_to(T3)
        self.assertEqual(self._entrada(fresca), (True, None))
        self._assert_resposta(irma, instante=T3)
        self.assertEqual(
            list(irma.decisoes.order_by('pk').values_list('ato', 'ato_em')),
            [(D.RESPOSTA, T0), (D.RECUSA, T2), (D.RESPOSTA, T3)],
        )
        push.assert_called_once() [nao verificado]


```

**Veredito esperado.** A expectativa antiga é verde na base. O texto invertido é vermelho na base por `ABSORCAO` extra na primeira igualdade, verde com B.3 e vermelho nessa mesma igualdade sob B1, B2 ou B3. O mesmo par original/mutado da seção 3 se aplica. Execução [nao verificado]. `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:364–380` · `app/chamados/services/materializacao.py:1100–1149`

## 5. B.4 aplicável junto do texto de teste [nao verificado]

Em `app/chamados/tests/e4d_matriz_mutantes.py`, retire de `    if nome == '20_ignora_recusa_no_empate':` até antes de `    if nome in {'25_absorcao_so_por_coluna', '26_devolve_sem_vinculo'}:`. Substitua de `M20IgnoraRecusaNoEmpate = a(` até antes de `M26DevolveSemVinculo = a(` pelo bloco abaixo. Cada delimitação ocorre uma vez. `app/chamados/tests/e4d_matriz_mutantes.py:383–405,665–670`

```python
b = partial(contraprova, matriz.Caso20RecusaCommitadaTest)
M20OmiteRecusa = b('M20OmiteRecusa', '20_omite_recusa',
                   'test_caso_20_recusa_humana_prevalece_em_q_no_empate', falha_em='Regra 20: atos e foto de Q')
M20CortaRecusaPeloRelogio = b('M20CortaRecusaPeloRelogio', '20_corta_recusa_pelo_relogio',
                             'test_caso_20_recusa_humana_prevalece_em_q_depois_do_relogio',
                             falha_em='Regra 20: atos e foto de Q')
M20EscondeRecusa = b('M20EscondeRecusa', '20_esconde_recusa',
                     'test_caso_20_recusa_humana_prevalece_em_q_no_empate', falha_em='Regra 20: atos e foto de Q')
M20VetaQSemRecusa = b('M20VetaQSemRecusa', '20_veta_q_sem_recusa',
                     'test_caso_20_sem_recusa_absorve_q', falha_em='Controle 20: vínculo de Q sem recusa')
```

Substitua `def contraprova(base, classe, nome, teste, *, leitor=None):` (1) pela primeira linha abaixo. Substitua a linha da checagem de `mutacao_aplicada` (1) pelo segundo bloco. Isso exige o marcador da asserção da regra além da falha do próprio teste. A classe externa do executor usa `unittest.TestCase`, de modo que a execução interna de `TransactionTestCase` pode fazer commits. `app/chamados/tests/e4d_matriz_mutantes

[truncado em 20000 de 45338 caracteres]

## 28/11 11:01 · sessao 364dc2fa8f54 · E4C-F5-INSTANTE · leitura · origem=Code

# E4C-F5-INSTANTE · lote A · volta a2

Na cópia, os três testes da cura6c ficaram **verdes**, o RED novo do censo ficou **vermelho** e os quatro REDs dos selos ficaram **vermelhos**. Com os diffs abaixo em memória, os oito ficaram verdes. Cada mutante foi aplicado isoladamente e derrubou por asserção os testes indicados na tabela. A execução usou `unittest.TestCase` e os analisadores AST da cópia. Identidade Git, execução de `486a1ffc4~1`, suíte Django e Ruff [nao verificado]. `app/core/censo_escritas.py:300–319,421–454,475–485,506–522` · `app/chamados/tests/test_o261e_f2_porta_carimbo.py:78–112,136–156,222–232` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165`

Nos blocos de mutação, retire o primeiro caractere de cada linha e junte as linhas de cada lado com `\n`, sem quebra final. Confira `fonte.count(original) == 1` e aplique `fonte.replace(original, mutado, 1)`. `censo_base` designa a troca apenas de `app/core/censo_escritas.py` pela revisão histórica pedida. Os resultados históricos abaixo são esperados [nao verificado].

## 1. Censo da projeção

### `test_RED_registrar_com_projecao_por_variavel_e_acusado`

Insira em `CensoLeAProjecaoTest`, antes de `test_os_dois_saves_da_porta_ficam_visiveis`, contagem **1**. **Hash verde. `censo_base` esperado vermelho [nao verificado].** `app/chamados/tests/test_e4c_f5_c4_red.py:1551–1562,1576` · `app/core/censo_escritas.py:442–454`

```python
    def test_RED_registrar_com_projecao_por_variavel_e_acusado(self):
        fonte = (
            'from chamados.models import DecisaoPergunta as DP\n'
            'def f(p, linhas):\n'
            "    proj = (p, ['respondida_em'])\n"
            '    DP.registrar(linhas, projecao=proj)\n'
        )
        self.assertEqual(self._varrer(fonte), [
            (4, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
        ])
```

**`M1-tupla`**, contagem **1**. A chamada continua com a chave `registrar`, mas perde o achado porque `proj` deixa de ser resolvida. `app/core/censo_escritas.py:426–429,444–447`

```diff
-        if isinstance(valor, ast.Name):
-            _, atribuicoes = self._escopos_da_projecao[-1]
-            valor = atribuicoes.get(valor.id)
-        if not isinstance(valor, ast.Tuple) or len(valor.elts) != 2:
+        if not isinstance(valor, ast.Tuple) or len(valor.elts) != 2:
```

### `test_RED_salvar_a_projecao_direto_fora_da_porta_e_acusado`

Insira em `CensoLeAProjecaoTest`, antes de `test_os_dois_saves_da_porta_ficam_visiveis`, contagem **1**. **Hash verde. `censo_base` esperado vermelho [nao verificado].** Cada subteste varre sua própria fonte e exige a tupla inteira. Assim os dois casos da linha 4 exercitam caminhos diferentes sem compartilhar a mesma chave `(linha, entidade)` numa varredura. `app/chamados/tests/test_e4c_f5_c4_red.py:1557–1562,1576` · `app/core/censo_escritas.py:421–440,506–522`

```python
    def test_RED_salvar_a_projecao_direto_fora_da_porta_e_acusado(self):
        casos = (
            ('funcao avulsa',
             'from chamados.models import DecisaoPergunta as DP\n'
             'def registrar(p):\n'
             "    DP._salvar_a_projecao(p, ['respondida_em'])\n", 3, 'DP'),
            ('registrar de outra classe',
             'from chamados.models import DecisaoPergunta as DP\n'
             'class Outra:\n'
             '    def registrar(self, p):\n'
             "        DP._salvar_a_projecao(p, ['respondida_em'])\n", 4, 'DP'),
            ('outro classmethod de DecisaoPergunta',
             'class DecisaoPergunta:\n'
             '    @classmethod\n'
             '    def outro(cls, p):\n'
             "        cls._salvar_a_projecao(p, ['respondida_em'])\n", 4, 'cls'),
        )
        for caso, fonte, linha, receptor in casos:
            with self.subTest(caso=caso):
                self.assertEqual(self._varrer(fonte), [
                    (linha, 'PerguntaDisputa', 'save', f'{receptor}._salvar_a_projecao() [projecao]'),
                ])
```

**`M1-ignora-helper`**, contagem **1**. Os três subtestes perdem o achado. `app/core/censo_escritas.py:432`

```diff
-                self._anotar(no, 'PerguntaDisputa', 'save', '%s() [projecao]' % _texto(f))
+                pass
```

**`M1-nome-registrar`**, contagem **1**. Caem a função avulsa e `Outra.registrar`. `app/core/censo_escritas.py:436–440`

```diff
-        if len(self._escopos_da_projecao) != 3:
-            return False
-        _, (classe, _), (metodo, _) = self._escopos_da_projecao
-        return (isinstance(classe, ast.ClassDef) and classe.name == 'DecisaoPergunta'
-                and isinstance(metodo, ast.FunctionDef) and metodo.name == 'registrar')
+        metodo, _ = self._escopos_da_projecao[-1]
+        return isinstance(metodo, ast.FunctionDef) and metodo.name == 'registrar'
```

**`X-sem-nome-classe`**, contagem **1**. `Outra.registrar` ganha a dispensa e perde o achado da linha 4. `app/core/censo_escritas.py:438–440`

```diff
-        return (isinstance(classe, ast.ClassDef) and classe.name == 'DecisaoPergunta'
+        return (isinstance(classe, ast.ClassDef)
```

**`X-sem-cls`**, contagem **1**. `DecisaoPergunta.outro` perde o achado de `cls._salvar_a_projecao` da linha 4. `app/core/censo_escritas.py:422–432`

```diff
-            if (op == '_salvar_a_projecao' and (dono or classe_de_cls) == 'DecisaoPergunta'
+            if (op == '_salvar_a_projecao' and dono == 'DecisaoPergunta'
```

### `test_os_dois_saves_da_porta_ficam_visiveis`

Substitua o método de mesmo nome em `CensoLeAProjecaoTest`, contagem **1**. **Hash verde. `censo_base` esperado vermelho pelo controle externo [nao verificado].** O teste lê todos os `save` da classe, incluindo os atribuídos a uma entidade, e preserva cada achado com `append`. Acrescentei ao texto lido um controle externo, em linha própria, para exigir que a dispensa interna conviva com a detecção do helper fora da porta. `app/chamados/tests/test_e4c_f5_c4_red.py:1557–1562,1576–1591` · `app/chamados/models.py:4780–4786,4797–4803,4821–4824` · `app/core/censo_escritas.py:421–440,506–522`

```python
    def test_os_dois_saves_da_porta_ficam_visiveis(self):
        import ast

        with open(os.path.join(self.APP, 'chamados/models.py'), encoding='utf-8') as arquivo:
            fonte = arquivo.read().rstrip('\n')
        classe, = (
            no for no in ast.parse(fonte).body
            if isinstance(no, ast.ClassDef) and no.name == 'DecisaoPergunta'
        )
        metodos = [no for no in ast.walk(classe) if isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef))]
        linha_externa = len(fonte.splitlines()) + 3
        controle = (
            '\n\ndef _controle_fora_da_porta(p):\n'
            "    DecisaoPergunta._salvar_a_projecao(p, ['respondida_em'])\n"
        )
        achados = self._varrer(fonte + controle)
        contado = {}
        for linha, entidade, op, expr in achados:
            if op != 'save' or not classe.lineno <= linha <= classe.end_lineno:
                continue
            dono = min(
                (no for no in metodos if no.lineno <= linha <= no.end_lineno),
                key=lambda no: no.end_lineno - no.lineno,
                default=classe,
            )
            contado.setdefault(dono.name, []).append((entidade, expr))
        self.assertEqual(contado, {
            '_salvar_a_projecao': [(None, 'instancia.save()')],
            'registrar': [(None, 'projecao[0].save()')],
        })
        self.assertIn(
            (linha_externa, 'PerguntaDisputa', 'save', 'DecisaoPergunta._salvar_a_projecao() [projecao]'),
            achados,
        )
```

O controle externo mata também **`M1-ignora-helper`**, com a troca de contagem **1** acima. `app/core/censo_escritas.py:432`

**`M1-reclassifica`**, contagem **1**. Os dois saves internos passam a trazer `'PerguntaDisputa'` no lugar de `None`. `app/core/censo_escritas.py:484–485`

```diff
-        if not silencioso:
-            self._anotar(no, None, op, '%s.%s()' % (_texto(rec), op))
+        if not silencioso:
+            entidade = 'PerguntaDisputa' if op == 'save' and self._classe == ['DecisaoPergunta'] else None
+            self._anotar(no, entidade, op, '%s.%s()' % (_texto(rec), op))
```

**`M1-terceiro-save`**, contagem **1**. O `registrar` real ganha um terceiro save no total da classe, atribuído a `PerguntaDisputa`. `app/core/censo_escritas.py:430–432` · `app/chamados/models.py:4823–4824`

```diff
-            if (op == '_salvar_a_projecao' and (dono or classe_de_cls) == 'DecisaoPergunta'
-                    and not self._no_registrar_da_decisao()):
+            if op == '_salvar_a_projecao' and (dono or classe_de_cls) == 'DecisaoPergunta':
```

### `test_RED_registrar_com_projecao_anotada_ou_nomeada_e_acusado`

Insira em `CensoLeAProjecaoTest`, antes de `test_os_dois_saves_da_porta_ficam_visiveis`, contagem **1**. **Hash vermelho** nos casos anotado com valor, `:=` antes da chamada e `:=` no argumento. **`censo_base` esperado vermelho [nao verificado]. Com a cura abaixo, verde.** A anotação sem valor testa a preservação de uma tupla já atribuída. `app/chamados/tests/test_e4c_f5_c4_red.py:1557–1562,1576` · `app/core/censo_escritas.py:308–313,426–429,442–454`

```python
    def test_RED_registrar_com_projecao_anotada_ou_nomeada_e_acusado(self):
        tupla = "(p, ['respondida_em'])"
        cabecalho = 'from chamados.models import DecisaoPergunta as DP\ndef f(p, linhas):\n'
        casos = (
            ('anotada', f'    proj: tuple = {tupla}\n    DP.registrar(linhas, projecao=proj)\n', 4),
            ('anotacao sem valor preserva a tupla',
             f'    proj = {tupla}\n    proj: tuple\n    DP.registrar(linhas, projecao=proj)\n', 5),
            ('nomeada antes da chamada',
             f'    (proj := {tupla})\n    DP.registrar(linhas, projecao=proj)\n', 4),
            ('nomeada no argumento', f'    DP.registrar(linhas, projecao=(proj := {tupla}))\n', 3),
        )
        for caso, corpo, linha in casos:
            with self.subTest(caso=caso):
                self.assertEqual(self._varrer(cabecalho + corpo), [
                    (linha, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
                ])
```

#### Diff de produção

As duas âncoras têm contagem **1** no hash, em `visit_Assign` e no início de `_projecao`. O primeiro trecho registra os vínculos no escopo corrente. O segundo lê o valor de `:=` quando a expressão é o próprio argumento de projeção. `app/core/censo_escritas.py:300–315,426–433,442–454` · `CLAUDE.md:28–31`

```diff
--- a/app/core/censo_escritas.py
+++ b/app/core/censo_escritas.py
@@ -308,8 +308,14 @@
     def visit_Assign(self, no):
         self.generic_visit(no)
+        if no.value is None:
+            return
         _, atribuicoes = self._escopos_da_projecao[-1]
-        for alvo in no.targets:
+        alvos = no.targets if isinstance(no, ast.Assign) else (no.target,)
+        for alvo in alvos:
             if isinstance(alvo, ast.Name):
                 atribuicoes[alvo.id] = no.value
+
+    visit_AnnAssign = visit_Assign
+    visit_NamedExpr = visit_Assign
 
     def visit_ClassDef(self, no):
@@ -442,4 +448,6 @@
     def _projecao(self, no, valor):
         """`projecao=(inst, campos)` = `inst.save(update_fields=campos)` neste sitio (`ESCRITA_POR_PROJECAO`)."""
+        if isinstance(valor, ast.NamedExpr):
+            valor = valor.value
         if isinstance(valor, ast.Name):
             _, atribuicoes = self._escopos_da_projecao[-1]
```

O RED novo mata também **`M1-tupla`** na cópia curada. As âncoras a seguir são do arquivo com apenas o diff de produção aplicado. `app/core/censo_escritas.py:308–319,450–455` após o diff.

**`M1-sem-AnnAssign`**, contagem **1**. Cai a anotação com valor. `app/core/censo_escritas.py:318` após o diff.

```diff
-    visit_AnnAssign = visit_Assign
-    visit_NamedExpr = visit_Assign
+    visit_NamedExpr = visit_Assign
```

**`M1-sem-NamedExpr`**, contagem **1**. Cai o `:=` anterior à chamada. `app/core/censo_escritas.py:318` após o diff.

```diff
-    visit_AnnAssign = visit_Assign
-    visit_NamedExpr = visit_Assign
+    visit_AnnAssign = visit_Assign
```

**`M1-sem-NamedExpr-no-argumento`**, contagem **1**. Cai o `:=` no argumento. `app/core/censo_escritas.py:450` após o diff.

```diff
-        if isinstance(valor, ast.NamedExpr):
-            valor = valor.value
-        if isinstance(valor, ast.Name):
-            _, atribuicoes = self._escopos_da_projecao[-1]
+        if isinstance(valor, ast.Name):
+            _, atribuicoes = self._escopos_da_projecao[-1]
```

**`M1-apaga-anotacao-anterior`**, contagem **1**. A anotação sem valor passa a sobrescrever a tupla e o achado some. `app/core/censo_escritas.py:309` após o diff.

```diff
-        self.generic_visit(no)
-        if no.value is None:
-            return
-        _, atribuicoes = self._escopos_da_projecao[-1]
+        self.generic_visit(no)
+        _, atribuicoes = self._escopos_da_projecao[-1]
```

## 2. Selos por arquivo

As fixtures colocam `g` na linha 3, antes do religamento. Cada teste exige por `assertIn` o achado no sítio afetado e em `g`. O `dict` religado alcança todo o arquivo, e cada nome de trilha religado perde sua dispensa em todos os usos desse nome no arquivo. É a mesma abrangência do controle existente com import legítimo e `def evento`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:78–112,309–331` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30,163`

Insira as duas fixtures no nível do módulo, antes da declaração `class SeloEscritorUnicoDoCarimbo(SimpleTestCase):`, contagem **1**. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:263`

```python
def _casos_trilha_dict_religado(campo):
    chamada = f'evento(depois=dict({campo}=None))\n'
    inicio = f'from core.observ import evento\ndef g():\n    {chamada}'
    return (
        ('modulo', inicio + f'dict = gravar\ndef f():\n    {chamada}', ('f', 6)),
        ('closure', inicio + 'def f():\n    dict = gravar\n    def interna():\n'
         + f'        {chamada}', ('f.interna', 7)),
        ('parametro', inicio + f'def f(dict):\n    {chamada}', ('f', 5)),
    )


def _casos_trilha_evento_religado(campo):
    chamada = f'evento(depois=dict({campo}=None))\n'
    inicio = f'from core.observ import evento\ndef g():\n    {chamada}'
    chamada_ev = f'ev(depois=dict({campo}=None))\n'
    inicio_ev = f'from core.observ import evento as ev\ndef g():\n    {chamada_ev}'
    chamada_observ = f'observ.evento(depois=dict({campo}=None))\n'
    inicio_observ = f'from core import observ\ndef g():\n    {chamada_observ}'
    return (
        ('import local', inicio + 'def f():\n    import core.outro as evento\n'
         + f'    {chamada}', ('f', 6)),
        ('from local com alias', inicio_ev + 'def f():\n    from core.outro import escritor as ev\n'
         + f'    {chamada_ev}', ('f', 6)),
        ('modulo observ', inicio_observ + 'def f():\n    from core.outro import observ\n'
         + f'    {chamada_observ}', ('f', 6)),
        ('parametro', inicio + f'def f(evento):\n    {chamada}', ('f', 5)),
    )
```

No módulo do par, estenda o import compartilhado, contagem **1**. `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30`

```diff
-from chamados.tests.test_o261e_f2_porta_carimbo import APP, _atribuicoes, _dicts_da_trilha
+from chamados.tests.test_o261e_f2_porta_carimbo import (
+    APP,
+    _atribuicoes,
+    _casos_trilha_dict_religado,
+    _casos_trilha_evento_religado,
+    _dicts_da_trilha,
+)
```

### Carimbo · `test_RED_trilha_acusa_dict_religado`

Insira em `SeloEscritorUnicoDoCarimbo`, antes de `test_MORDE_dict_em_variavel_defaults_construtor_e_fields_em_variavel`, contagem **1**. **Hash vermelho. `censo_base` esperado vermelho. Cura do selo verde.** Mata `M2-grafia-dict` e `M2-sem-parametro`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112,154–156,222–225,263,333`

```python
    def test_RED_trilha_acusa_dict_religado(self):
        for caso, fonte, alvo in _casos_trilha_dict_religado('materializada_em'):
            achados = escritas_do_carimbo(fonte)
            for nome, linha in (alvo, ('g', 3)):
                with self.subTest(caso=caso, nome=nome):
                    self.assertIn((nome, linha, 'dict(materializada_em=)'), achados)
```

### Carimbo · `test_RED_trilha_acusa_import_local_homonimo`

Insira em `SeloEscritorUnicoDoCarimbo`, antes de `test_MORDE_dict_em_variavel_defaults_construtor_e_fields_em_variavel`, contagem **1**. **Hash vermelho. `censo_base` esperado vermelho. Cura do selo verde.** Mata `M2-import-global` e `M2-sem-parametro`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112,154–156,222–225,263,333`

```python
    def test_RED_trilha_acusa_import_local_homonimo(self):
        for caso, fonte, alvo in _casos_trilha_evento_religado('materializada_em'):
            achados = escritas_do_carimbo(fonte)
            for nome, linha in (alvo, ('g', 3)):
                with self.subTest(caso=caso, nome=nome):
                    self.assertIn((nome, linha, 'dict(materializada_em=)'), achados)
```

### Par · `test_RED_trilha_acusa_dict_religado`

Insira em `SeloEscritorUnicoDoParMordeTest`, antes de `test_MORDE_repasse_de_param_na_classe_do_modelo`, contagem **1**. **Hash vermelho. `censo_base` esperado vermelho. Cura do selo verde.** Mata `M2-grafia-dict` e `M2-sem-parametro`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30,144–165,247,269`

```python
    def test_RED_trilha_acusa_dict_religado(self):
        for caso, fonte, alvo in _casos_trilha_dict_religado('respondida_em'):
            achados = escritas_do_par(fonte)
            for nome, linha in (alvo, ('g', 3)):
                with self.subTest(caso=caso, nome=nome):
                    self.assertIn((nome, linha, 'dict(respondida_em=)'), achados)
```

### Par · `test_RED_trilha_acusa_import_local_homonimo`

Insira em `SeloEscritorUnicoDoParMordeTest`, antes de `test_MORDE_repasse_de_param_na_classe_do_modelo`, contagem **1**. **Hash vermelho. `censo_base` esperado vermelho. Cura do selo verde.** Mata `M2-import-global` e `M2-sem-parametro`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30,144–165,247,269`

```python
    def test_RED_trilha_acusa_import_local_homonimo(self):
        for caso, fonte, alvo in _casos_trilha_evento_religado('respondida_em'):
            achados = escritas_do_par(fonte)
            for nome, linha in (alvo, ('g', 3)):
                with self.subTest(caso=caso, nome=nome):
                    self.assertIn((nome, linha, 'dict(respondida_em=)'), achados)
```

### Texto de referência da cura do selo para o Opus

Âncora de contagem **1** na cópia, dentro de `_dicts_da_trilha`. Com este texto, passaram os quatro REDs e os dois controles existentes de trilha legítima. Os três originais dos mutantes M2 têm contagem **0** na cópia e **1** neste texto curado. As linhas dos mutantes abaixo contam o diff aplicado antes das inserções de fixtures e testes. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:78–112,309–331` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:289–299`

```diff
--- a/app/chamados/tests/test_o261e_f2_porta_carimbo.py
+++ b/app/chamados/tests/test_o261e_f2_porta_carimbo.py
@@ -85,15 +85,27 @@
     funcs, mods, religados, core_inteiro = set(), set(), set(), False
     for n in ast.walk(arv):
-        if isinstance(n, ast.ImportFrom) and n.module == 'core.observ' and not n.level:
-            funcs |= {a.asname or a.name for a in n.names if a.name == 'evento'}
-        elif isinstance(n, ast.ImportFrom) and n.module == 'core' and not n.level:
-            mods |= {a.asname or a.name for a in n.names if a.name == 'observ'}
-        elif isinstance(n, ast.Import):
-            mods |= {a.asname for a in n.names if a.name == 'core.observ' and a.asname}
-            core_inteiro |= any(a.nam

[truncado em 20000 de 24071 caracteres]

## 28/11 10:58 · sessao 393a45b4bcf1 · FDS-FERIAS-COM-BATIDA-PORTA · sonda · origem=Code

A sonda informa **475 colaboradores e 1.025 dias-colaborador retidos**, com **228 colaboradores com dia retido**. A matriz fecha os **1.025 dias**. A contagem declarada também fecha os **475 colaboradores**, mas os PKs impressos identificam **474**. A diferença está na linha `(sem motivo)`, que declara **217** colaboradores e lista **216** PKs distintos. `sonda:4–5,8–49`

As referências `sonda:N` contam as linhas do bloco raspado desde `sonda_leitura`, incluindo a linha em branco. As referências `r.md:N` contam desde o primeiro título do anexo.

## Matriz de portas oferecidas hoje

| Motivo | Escopo | Porta | Dias | Colabs | Saída |
|---|---|---|---:|---:|---|
| `(sem motivo)` | sem dia | sem porta | 0 | 217 declarados | `sonda:5` |
| `codigo_dominio_invalido` | sem dia | Cadastro do colaborador | 0 | 1 | `sonda:6` |
| `codigo_dominio_invalido` | sem dia | Validar no fio | 0 | 1 | `sonda:7` |
| `espelho_cobrado` | dia | Resolver dia no calendário | 7 | 5 | `sonda:8` |
| `espelho_cobrado` | dia | sem porta datada | 420 | 110 | `sonda:9` |
| `espelho_cobrado` | linha | Cobrar no fio | 339 | 75 | `sonda:10` |
| `espelho_cobrado` | linha | Decidir ausência | 5 | 3 | `sonda:11` |
| `espelho_cobrado` | linha | Decidir no fio | 3 | 3 | `sonda:12` |
| `espelho_cobrado` | linha | Validar no fio | 49 | 23 | `sonda:13` |
| `espelho_cobrado` | linha | Vínculo da escala | 194 | 45 | `sonda:14` |
| `espelho_cobranca_indevida` | dia | sem porta datada | 8 | 5 | `sonda:15` |
| `espelho_cobranca_indevida` | linha | Cobrar no fio | 1 | 1 | `sonda:16` |
| `espelho_cobranca_indevida` | linha | Validar no fio | 6 | 3 | `sonda:17` |
| `espelho_cobranca_indevida` | linha | Vínculo da escala | 1 | 1 | `sonda:18` |
| `espelho_discordante` | dia | Resolver dia no calendário | 7 | 4 | `sonda:19` |
| `espelho_discordante` | dia | sem porta datada | 5 | 5 | `sonda:20` |
| `espelho_discordante` | linha | Cobrar no fio | 4 | 4 | `sonda:21` |
| `espelho_discordante` | linha | Decidir no fio | 1 | 1 | `sonda:22` |
| `espelho_discordante` | linha | Vínculo da escala | 3 | 3 | `sonda:23` |
| `espelho_fato_em_ausencia` | dia | Resolver dia no calendário | 7 | 6 | `sonda:24` |
| `espelho_fato_em_ausencia` | dia | sem porta datada | 6 | 4 | `sonda:25` |
| `espelho_fato_em_ausencia` | linha | Decidir no fio | 3 | 1 | `sonda:26` |
| `espelho_fato_em_ausencia` | linha | Validar no fio | 3 | 3 | `sonda:27` |
| `espelho_fato_em_ausencia` | linha | Vínculo da escala | 2 | 2 | `sonda:28` |
| `espelho_furo` | dia | Resolver dia no calendário | 8 | 7 | `sonda:29` |
| `espelho_furo` | dia | sem porta datada | 200 | 102 | `sonda:30` |
| `espelho_furo` | linha | Cobrar no fio | 138 | 69 | `sonda:31` |
| `espelho_furo` | linha | Decidir ausência | 2 | 2 | `sonda:32` |
| `espelho_furo` | linha | Decidir no fio | 15 | 8 | `sonda:33` |
| `espelho_furo` | linha | Validar no fio | 35 | 19 | `sonda:34` |
| `espelho_furo` | linha | Vínculo da escala | 94 | 42 | `sonda:35` |
| `espelho_indefinida` | dia | Resolver dia no calendário | 1 | 1 | `sonda:36` |
| `espelho_indefinida` | dia | sem porta datada | 228 | 54 | `sonda:37` |
| `espelho_indefinida` | linha | Cobrar no fio | 208 | 45 | `sonda:38` |
| `espelho_indefinida` | linha | Validar no fio | 3 | 2 | `sonda:39` |
| `espelho_indefinida` | linha | Vínculo da escala | 200 | 44 | `sonda:40` |
| `espelho_nunca_bateu` | dia | Resolver dia no calendário | 61 | 9 | `sonda:41` |
| `espelho_nunca_bateu` | dia | sem porta datada | 52 | 8 | `sonda:42` |
| `espelho_nunca_bateu` | linha | Decidir no fio | 8 | 1 | `sonda:43` |
| `espelho_nunca_bateu` | linha | Vínculo da escala | 60 | 4 | `sonda:44` |
| `ferias_com_batida` | dia | sem porta datada | 15 | 2 | `sonda:45` |
| `ferias_com_batida` | linha | Validar no fio | 15 | 2 | `sonda:46` |
| `rescisao_modulo_proprio` | sem dia | sem porta | 0 | 28 | `sonda:47` |
| `sem_codigo_dominio` | sem dia | Cadastro do colaborador | 0 | 1 | `sonda:48` |
| `sem_codigo_dominio` | sem dia | Validar no fio | 0 | 1 | `sonda:49` |

- **Dias.** No escopo `dia`, **91 com porta datada + 934 sem porta datada = 1.025**. As portas de `linha` podem repetir os mesmos pares de colaborador e data. `sonda:4,8–45` · `r.md:53`
- **Colaboradores.** A união dos PKs no escopo `dia` dá **228**. No escopo `sem dia`, os contadores declaram **217 + 1 + 28 + 1 = 247**, após contar uma só vez cada PK com dois botões. Isso fecha **228 + 247 = 475** pelos contadores. Pelas listas, são **246** PKs sem dia, sem interseção com os **228** com dia, totalizando **474** PKs visíveis. A identidade do PK faltante é [nao verificado]. `sonda:4–9,15,19–20,24–25,29–30,36–37,41–42,45,47–49`

## Casos do aval

As datas vêm do aval e do material de apoio. A sonda identifica a oferta por PK dentro de cada motivo. `sonda:36–40,45–46` · `r.md:30–33`

| Caso | Porta que aparece hoje | Evidência |
|---|---|---|
| `col344` · 07/11 | **Validar no fio**, no escopo `linha`. Sem porta datada para `ferias_com_batida`. | `sonda:45–46` · [RELATO.md:15107](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/RELATO.md:15107) |
| `col327` · 10/11 | **Nenhuma porta na saída.** O PK aparece em `sem porta datada` e está ausente das portas de `linha` de `espelho_indefinida`. | `sonda:36–40` · [RELATO.md:14897](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/RELATO.md:14897) |
| `col97` · ímpares de 01 a 19/11 | **Nenhuma porta na saída.** O PK aparece em `sem porta datada` e está ausente das portas de `linha` de `espelho_indefinida`. | `sonda:36–40` · [RELATO.md:14899](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/RELATO.md:14899) |
| `col476` · 25/10 | **Vínculo da escala**, no escopo `linha`. Sem porta datada para `espelho_indefinida`. | `sonda:36–40` · [RELATO.md:14898](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/RELATO.md:14898) |

O anexo descreve **Conferir ausência** para `col327` e **Vínculo da escala** datado para `col97` pelo OS6c. Essas ofertas estão ausentes da saída. Na cópia, `portas_da_linha` retorna após traduzir ações, fio e tipo, e a montagem copia `motivos_por_dia` sem projetar destinos causais. `r.md:23,31–32` · `sonda:36–40` · [retrato_folha.py:1078–1110](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/folha/retrato_folha.py:1078) · [fora_da_folha.py:218–255](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/folha/fora_da_folha.py:218)

## Dias sem a porta que trata a causa

Os valores exatos abaixo medem a ausência do destino indicado no anexo. Onde o anexo admite mais de uma correção, contei como candidata toda porta oferecida que poderia servir e usei isso para calcular o mínimo sem porta. O vínculo entre o botão e a causa de cada dia permanece [nao verificado] nesses casos. `r.md:7–14,39–49` · `sonda:8–46`

| Motivo | Dias retidos | Dias sem porta causal pelo anexo | Base |
|---|---:|---|---|
| `espelho_cobrado` | 427 | **Pelo menos 30.** Calendário e fio cobrem no máximo 397 dias, descontada a sobreposição comprovada. Total exato [nao verificado]. | `sonda:8–14` · `r.md:12` |
| `espelho_cobranca_indevida` | 8 | **8** sem **Conferir chamado** pelo ID do chamado. | `sonda:15–18` · `r.md:11,41` |
| `espelho_discordante` | 12 | **Pelo menos 5** sem calendário. Nos 7 com calendário, a saída não identifica a causa da divergência. Total exato [nao verificado]. | `sonda:19–23` · `r.md:9` |
| `espelho_fato_em_ausencia` | 13 | **Pelo menos 6** sem calendário ou painel da ausência. Nos 7 com calendário, a saída não identifica qual registro precisa de correção. Total exato [nao verificado]. | `sonda:24–28` · `r.md:14` |
| `espelho_furo` | 208 | **Pelo menos 13.** Calendário e fio cobrem no máximo 195 dias, descontada ao menos uma sobreposição. Total exato [nao verificado]. | `sonda:29–35` · `r.md:8` |
| `espelho_indefinida` | 229 | **Pelo menos 29.** A saída não traz painel da ausência nem editor da agenda. Os 200 dias com **Vínculo da escala** de linha são candidatos quando a causa é vigência ou fase. Total exato [nao verificado]. | `sonda:36–40` · `r.md:10,42,48` |
| `espelho_nunca_bateu` | 113 | **113** sem **Cadastro x Realidade, C5**, o destino de triagem do anexo. | `sonda:41–44` · `r.md:13,41` |
| `ferias_com_batida` | 15 | **15** sem **Férias do colaborador** e sem **Calendário do dia para retratar a marcação**. | `sonda:45–46` · `r.md:16,41` |
| `sem_celula` | 0 | **0**. O motivo não aparece na saída e os demais fecham o total de dias. | `sonda:4,8–46` · `r.md:7` |

**`espelho_indefinida` por protesto:** os totais de `PARCIAL_SEM_PERIODO`, `FASE_NAO_DECLARADA_COM_PREVISTO` e `PREVISTO_COM_DIA_INDEFINIDO` são [nao verificado]. O código agrupa esses protestos em `indefinida`, e a saída agrega por motivo, escopo e porta, com uma lista de PKs por célula. O rótulo **Vínculo da escala** não identifica qual protesto foi lavrado. `sonda:36–40` · `r.md:42,53` · [models.py:1626](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/escala/models.py:1626) · [export.py:1310–1322](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/folha/export.py:1310)

## 28/11 10:55 · sessao 9ef8957d8ad1 · INTERVALO-SEM-PREVISTO · sonda · origem=Code

As referências `sonda.py:N` apontam para as linhas da saída raspada fornecida, contando desde `sonda_leitura` e incluindo a linha em branco.

## Competência 11 na sombra

A sonda conta as perguntas vivas pelo juiz C6 e procura a célula pelo colaborador e dia do turno. Em célula encontrada, `sem_intervalo` significa que o leitor do DNA encontrou zero pares completos de pausa. `sonda.py:3–6` · `app/chamados/juizes.py:911–938` · `app/escala/utils.py:111–146`

| Empresa | Vivas | Em célula sem intervalo previsto | Sem célula | Em célula com intervalo, por diferença | Saída |
|---|---:|---:|---:|---:|---|
| 2 | 1.978 | 68 | 0 | 1.910 | `sonda.py:3` |
| 3 | 403 | 20 | 0 | 383 | `sonda.py:4` |
| 4 | 22 | 0 | 0 | 22 | `sonda.py:5` |
| **Total** | **2.403** | **88** | **0** | **2.315** | `sonda.py:3–6` |

As empresas somam **1.978 + 403 + 22 = 2.403** vivas, **68 + 20 + 0 = 88** sem intervalo previsto e **0 + 0 + 0 = 0** sem célula. As partes do universo também fecham em **2.315 + 88 + 0 = 2.403**. `sonda.py:3–6`

## Caso literal

- O chamado **40588** está `aberto`, com origem `batida_ausente`. No dump, a célula **122468** de **20/11/2026** tem entrada `hi` às **07:00** e saída `hf` às **19:00**, sem intervalo previsto. O veredito é `cobrado`, com `protestos=[]`, **720 min** previstos e **715 min** realizados. `sonda.py:7–9`
- O `dna_anterior` tem entrada `hi` às **07:00**, saída para intervalo `hii` às **11:00**, volta `hfi` às **12:00** e saída `hf` às **19:00**. O contador registra **3 regenerações**. O campo guarda a versão imediatamente anterior à última regeneração. `sonda.py:8` · `app/escala/models.py:1725–1741`

| Pergunta | Motivo | Estado no dump | Emissão | Saída |
|---|---|---|---|---|
| 61520 | `intervalo_saida` | `pendente`, viva; sem carimbos de resposta, validação ou materialização; via vazia | `2026-11-23 12:36:16.395304+00:00` | `sonda.py:10,12` |
| 61521 | `intervalo_volta` | `pendente`, viva; sem carimbos de resposta, validação ou materialização; via vazia | `2026-11-23 12:36:16.420428+00:00` | `sonda.py:11,13` |

As duas apontam para a célula **122468**, na disputa **7979**, com chamado-pai **37670** e destino **40588**. **Sim, a última regeneração foi posterior às duas emissões.** O carimbo é `2026-11-23 12:42:47.819535+00:00`. `sonda.py:8,10–13`

A retenção do colaborador retornou um único dia, **20/11/2026**, e um único motivo, `espelho_cobrado`, na célula **122468**, com veredito `cobrado`. O retentor deriva esse motivo do veredito lavrado, inclusive com a lista de protestos vazia neste caso. `sonda.py:7,14` · `app/folha/export.py:1301–1326`

## Origem dos horários

**A fonte usada no nascimento das perguntas está [nao verificado].** A linha `marcos_dna_anterior` comprova que **11:00/12:00** estavam no DNA imediatamente anterior à última regeneração. As linhas `emissao` comprovam a ordem dos eventos. O código grava motivo, dia, disputa e célula no evento `pergunta_emitida`, e os textos do catálogo interpolam a data. O ramo executado e o conteúdo da ata no instante da emissão estão [nao verificado]. `sonda.py:8,12–13` · `app/chamados/models.py:3330–3344` · `app/chamados/catalogo/perguntas.py:123–138`

- **DNA e ata.** A fábrica deriva os motivos dos marcos faltantes na ata da célula. A presença do par no `dna_anterior` é compatível com esse caminho. `sonda.py:8` · `app/chamados/services/fabrica_da_celula.py:52–74` · `app/chamados/services/declaracao_texto.py:87–104,122–144`
- **Contexto do chamado.** O fio lê a hora prevista em `chamado_destino.contexto_json['marcos_faltantes']`. Na regeneração, `orfa_regeneracao.marcos_na_emissao` copia horas desse contexto. A linha `chamado` mostra origem e status. Os valores do contexto desse chamado e seu uso na emissão estão [nao verificado]. `sonda.py:9–11` · `app/chamados/services/fio.py:446–457` · `app/chamados/juizes.py:572–601` · `app/ponto/management/commands/gerar_celulas.py:255–264`
- **`TipoEscala`.** O código consulta a escala ativa como fallback na validação da hora. O valor que esse fallback devolveria no caso está [nao verificado]. `app/chamados/juizes.py:1982–2008,2030–2048`

Identidade da cópia com o hash informado [nao verificado].

## 28/11 10:52 · sessao 9963d98a4a05 · E4D-CONTRATO-ROTA-B · leitura · origem=Code

# r.md — E4D-CONTRATO-ROTA-B, cura4

lei: `matriz.md:30` e `r2.txt:8`, grupo 6 — “A recusa humana prevalece em Q”, sem nova `ABSORCAO` dessa passagem sobre Q. Identidade da passagem — identificador próprio ou instante registrado — em aberto (`matriz.md:79`, `r2.txt:8`, conforme os anexos).

**Na cópia indicada como `f300431af`**, o selo A com o texto abaixo deu **VERDE** em memória e acusou A1–A4 e X1. O caso 20 com recusa é **VERMELHO esperado** no código atual. O controle sem recusa, C1 e C3 são **VERDE esperado**. Conferi as âncoras e compilei os trechos Python em memória. Identidade Git, Ruff e execução ORM/HTTP [nao verificado]. `app/chamados/tests/test_e4c_f5_selo_catraca.py:332–356` · `app/chamados/tests/test_e4d_matriz_reds.py:1440–1514,1616–1617` · `app/chamados/services/materializacao.py:1074–1133` · `app/templates/chamados/partials/_pergunta_card.html:20–44` · `app/api/views.py:2653–2666`

## A — Catraca do fio

A varredura usa `ast.walk` para conferir **todo** `Name(Load)` de `ALVO_FIO`, inclusive em argumento, retorno, valor default e decorador. Só a identidade do nó em `Call.func` autoriza tratar a referência como chamada direta. O mesmo critério cobre acesso por atributo, e o import com `as` recebe diagnóstico próprio de referência. Uma chamada direta com `leitura` explícita precisa da prova de origem pelo visitante. As duas chamadas de produção que omitem `leitura` usam o default `None`, conferido separadamente por A4. `app/chamados/tests/test_e4c_f5_selo_catraca.py:76–85,214–235,304–349` · `app/chamados/services/fio.py:210–226,442–446` · `app/chamados/services/cobranca.py:227–242`

### A.1 — `app/chamados/tests/test_e4c_f5_selo_catraca.py`

Substitua a âncora `from chamados.tests.e4c_f5_voltaE_mutantes import fonte_mutada` (1). `app/chamados/tests/test_e4c_f5_selo_catraca.py:9`

```python
from chamados.tests.e4c_f5_voltaE_mutantes import fonte_mutada
from core.censo_escritas import _e_codigo_de_producao
```

Substitua a âncora composta abaixo (1). `app/chamados/tests/test_e4c_f5_selo_catraca.py:68–70`

```python
class Catraca(ast.NodeVisitor):
    def __init__(self):
        self.vinculos = {}
```

Por este texto. `app/chamados/tests/test_e4c_f5_selo_catraca.py:68–74`

```python
class Catraca(ast.NodeVisitor):
    def __init__(self, *, parametros_compostos=None):
        self.parametros_compostos = parametros_compostos or {}
        self.vinculos = {}
```

Substitua a âncora composta abaixo (1). `app/chamados/tests/test_e4c_f5_selo_catraca.py:321–324`

```python
        antes = self.vinculos.copy()
        for arg in ast.walk(no.args):
            if isinstance(arg, ast.arg):
                self.vinculos.pop(arg.arg, None)
```

Por este texto. `app/chamados/tests/test_e4c_f5_selo_catraca.py:321–329`

```python
        antes = self.vinculos.copy()
        compostos = self.parametros_compostos.get(no, ())
        for arg in ast.walk(no.args):
            if isinstance(arg, ast.arg):
                self.vinculos.pop(arg.arg, None)
                if arg.arg in compostos:
                    self.vinculos[arg.arg] = VAZIO._replace(composta=True)
```

Substitua de `def _vermelhos(funcao):` até antes de `    def _julgar(self, corpo):` (1 ocorrência de cada âncora). Os dois testes usam a mesma varredura. O `test_MORDE_*` aplica individualmente cada mutante à fonte real, exige a fonte limpa verde e confere o arquivo e o tipo de cada diagnóstico. `app/chamados/tests/test_e4c_f5_selo_catraca.py:332–351` · `app/chamados/tests/e4c_f5_voltaE_mutantes.py:80–91` · `CLAUDE.md:195`

```python
ALVO_FIO = 'espera_o_texto_do_colaborador'
ARQUIVO_FIO = 'chamados/services/fio.py'


def _referencia_ao_fio(no):
    if isinstance(no, ast.Name):
        return no.id == ALVO_FIO and isinstance(no.ctx, ast.Load)
    return isinstance(no, ast.Attribute) and no.attr == ALVO_FIO and isinstance(no.ctx, ast.Load)


def _nulo(no):
    return isinstance(no, ast.Constant) and no.value is None


class CatracaChamadasDoFio(Catraca):
    def __init__(self):
        super().__init__()
        self.leituras_compostas = set()

    def _composta(self, no):
        if isinstance(no, ast.Name):
            return super()._composta(no)
        return (
            _metodo(no) == 'composta'
            and isinstance(no.func.value, ast.Name)
            and no.func.value.id == 'DecisaoPergunta'
        )

    def visit_Call(self, no):
        if _referencia_ao_fio(no.func):
            self.leituras_compostas.update(
                kw.value for kw in no.keywords if kw.arg == 'leitura' and self._composta(kw.value)
            )
        super().visit_Call(no)


def _vermelhos_das_chamadas_do_fio(fonte):
    catraca = CatracaChamadasDoFio()
    catraca.visit(fonte)
    nos = tuple(ast.walk(fonte))
    diretas = {no.func: no for no in nos if isinstance(no, ast.Call) and _referencia_ao_fio(no.func)}
    for no in nos:
        if isinstance(no, ast.alias) and no.name == ALVO_FIO and no.asname not in (None, ALVO_FIO):
            yield no.lineno, 'referencia'
        if not _referencia_ao_fio(no):
            continue
        chamada = diretas.get(no)
        if chamada is None:
            yield no.lineno, 'referencia'
        elif (len(chamada.args) > 1 or any(isinstance(arg, ast.Starred) for arg in chamada.args)
              or any(kw.arg is None or (kw.arg == 'leitura' and not _nulo(kw.value)
                                       and kw.value not in catraca.leituras_compostas)
                     for kw in chamada.keywords)):
            yield no.lineno, 'leitura'


def _vermelhos(funcao, *, parametros_compostos=()):
    catraca = Catraca(parametros_compostos={funcao: parametros_compostos})
    catraca.visit(funcao)
    return catraca.vermelhos


class SeloCatracaTest(SimpleTestCase):
    def _vermelhos_da_producao(self, *, mutante=None):
        vermelhos = []
        fontes = {}
        for caminho in sorted(RAIZ.rglob('*.py')):
            arquivo = caminho.relative_to(RAIZ).as_posix()
            if not _e_codigo_de_producao(arquivo):
                continue
            texto = fonte_mutada(arquivo, caminho.read_text(encoding='utf-8'), nome=mutante)
            if ALVO_FIO in texto or arquivo in FUNCOES:
                fontes[arquivo] = ast.parse(texto, filename=arquivo)
            if ALVO_FIO in texto:
                vermelhos.extend(
                    f'{arquivo}:{linha} {ALVO_FIO} ({tipo})'
                    for linha, tipo in sorted(set(_vermelhos_das_chamadas_do_fio(fontes[arquivo])))
                )
        for arquivo, nomes in FUNCOES.items():
            for nome in nomes:
                locais = [no for no in fontes[arquivo].body if isinstance(no, ast.FunctionDef) and no.name == nome]
                self.assertEqual(len(locais), 1, (arquivo, nome))
                funcao, = locais
                compostos = ()
                if (arquivo, nome) == (ARQUIVO_FIO, ALVO_FIO):
                    defaults = dict(zip((arg.arg for arg in funcao.args.kwonlyargs), funcao.args.kw_defaults))
                    if not _nulo(defaults.get('leitura')):
                        vermelhos.append(f'{arquivo}:{funcao.lineno} {nome} (default de leitura)')
                    compostos = ('leitura',)
                vermelhos.extend(
                    f'{arquivo}:{linha} {nome} ({tipo})'
                    for linha, tipo in sorted(_vermelhos(funcao, parametros_compostos=compostos))
                )
        return vermelhos

    def test_producao_sem_leituras_cruas(self):
        self.assertEqual(self._vermelhos_da_producao(), [])

    def test_MORDE_mutantes_do_fio_na_fonte_real(self):
        self.assertEqual(self._vermelhos_da_producao(mutante=''), [])
        casos = {'A1': 'leitura', 'A2': 'respondida_em', 'A3': 'respondida_em',
                 'A4': 'default de leitura', 'X1': 'referencia'}
        for nome, tipo in casos.items():
            with self.subTest(mutante=nome):
                vermelhos = self._vermelhos_da_producao(mutante=nome)
                self.assertEqual(len(vermelhos), 1, vermelhos)
                self.assertTrue(vermelhos[0].startswith(f'{ARQUIVO_FIO}:'), vermelhos)
                self.assertTrue(vermelhos[0].endswith(f' {ALVO_FIO} ({tipo})'), vermelhos)

```

### A.2 — `app/chamados/tests/e4c_f5_voltaE_mutantes.py`

Substitua a âncora composta `"}\n\n\ndef fonte_mutada(arquivo, fonte):\n    nome = os.environ.get(SELETOR)"` (1) pelo bloco abaixo. A aplicação continua exigindo `count == 1` para cada original. `app/chamados/tests/e4c_f5_voltaE_mutantes.py:77–91`

```python
    'A1': (
        'chamados/services/fio.py',
        'espera_o_texto_do_colaborador(_p, leitura=leitura)',
        'espera_o_texto_do_colaborador(_p, leitura=_p)',
    ),
    'A2': (
        'chamados/services/fio.py',
        '    if leitura is None:\n        leitura = DecisaoPergunta.composta(p)',
        '    if leitura is None:\n        leitura = DecisaoPergunta.composta(p)\n'
        '    else:\n        leitura = p',
    ),
    'A3': (
        'chamados/services/fio.py',
        '    if leitura is None:\n        leitura = DecisaoPergunta.composta(p)',
        '    if leitura is None:\n        leitura = p',
    ),
    'A4': (
        'chamados/services/fio.py',
        'def espera_o_texto_do_colaborador(p, *, leitura=None):',
        'def espera_o_texto_do_colaborador(p, *, leitura=False):',
    ),
    'X1': (
        'chamados/services/fio.py',
        '_p.resposta_enviada = not espera_o_texto_do_colaborador(_p, leitura=leitura)',
        '_esp = espera_o_texto_do_colaborador; _p.resposta_enviada = not _esp(_p, leitura=_p)',
    ),
}


def fonte_mutada(arquivo, fonte, *, nome=None):
    if nome is None:
        nome = os.environ.get(SELETOR)
```

**Veredito medido em memória.** Os dez métodos do selo passaram com o executor `unittest.TestCase` no lugar da classe-base Django. `test_producao_sem_leituras_cruas` fica **VERDE** com a fonte limpa. Cada mutação dá **VERMELHO** no mesmo verificador chamado pelo `test_MORDE_*`, que por isso fica **VERDE**. Diagnósticos observados em `app/chamados/services/fio.py`: A1 `:445 (leitura)`, A2 `:228 (respondida_em)` na fonte mutada, A3 `:226 (respondida_em)`, A4 `:213 (default de leitura)` e X1 `:445 (referencia)`. `app/chamados/tests/test_e4c_f5_selo_catraca.py:332–356` · `app/chamados/services/fio.py:213–226,444–446`

## B — Caso 20

A fixture usa a âncora **de hora**, Q respondida, célula **concorde** e lâmpada **acesa**. A segunda irmã continua na fixture. Antes da releitura de Q, o teste comprova a `ABSORCAO` da primeira irmã e usa a porta real para registrar a `RECUSA` de Q. O veredito final é sobre Q. A política de compensar a irmã anterior ou de interromper a seguinte segue em aberto no grupo 6. `app/chamados/tests/test_e4d_matriz_reds.py:1330–1379,1440–1514` · `app/chamados/services/validacao.py:974–1000` · `r2.txt:8`, grupo 6, anexo

### B.1 — Teste da regra e controle [nao verificado]

Em `app/chamados/tests/test_e4d_matriz_reds.py`, substitua de `    def test_caso_20_recusa_para_so_q_e_preserva_irma_ja_absorvida(self):` até antes de `    def test_caso_24_ancora_fora_deixa_dependente_sem_cobranca(self):` (1 ocorrência de cada âncora). O bloco inclui a retirada de `pedidos_distintos` conforme a linha `lei:` no topo. `app/chamados/tests/test_e4d_matriz_reds.py:1440–1516`

```python
    def test_caso_20_recusa_humana_prevalece_em_q(self):
        for instante in (T1, T2):
            with self.subTest(recusa_em=instante), transaction.atomic():
                self.verificar_recusa_na_passagem(instante)
                transaction.set_rollback(True)

    def test_caso_20_sem_recusa_absorve_q(self):
        self.verificar_recusa_na_passagem(None)

    def verificar_recusa_na_passagem(self, instante):
        e = self.escape('hora')
        q = self.irma_da_celula(respondida=True)
        self.cartao(disputa=self.raiz, dia=self.DIA_PASSAGEM, motivo='intervalo_volta',
                    celula_id=q.celula_id)
        motivo = 'Confirme a hora'

        def antes_de_q():
            anterior = e.decisoes.get(ato=DP.ABSORCAO)
            self.assertEqual(
                (anterior.ato_em, anterior.absorcao_escape_id, anterior.absorcao_ausencia_id),
                (T1, e.pk, self.atestado().pk),
            )
            self.assertTrue(DP.absorcao_vigente(self.alvo(e).get()))
            self.assertFalse(q.decisoes.filter(ato=DP.ABSORCAO).exists())
            if instante is not None:
                with freeze_time(instante):
                    recusa = validacao.recusar_pergunta(q.pk, self.admin, motivo)
                self.assertEqual((recusa['ok'], recusa['estado']), (True, 'recusada'))
                ato = q.decisoes.get(ato=DP.RECUSA)
                self.assertEqual((ato.ato_em, ato.ato_por_id, ato.ato_motivo),
                                 (instante, self.admin.pk, motivo))
                self.assertEqual(self.foto(q), VAZIA)

        recusadas = self.passagem(e, antes_de_q, na_irma=2)
        if instante is not None:
            self.assertIn(q.pk, recusadas)
            self.assertEqual(self.atos(q), [DP.RESPOSTA, DP.RECUSA])
            self.assertEqual(self.foto(q), VAZIA)
        else:
            self.assertNotIn(q.pk, recusadas)
            self.assertFalse(q.decisoes.filter(ato=DP.RECUSA).exists())
            absorcao = q.decisoes.get(ato=DP.ABSORCAO)
            self.assertEqual(
                (absorcao.ato_em, absorcao.absorcao_escape_id, absorcao.absorcao_ausencia_id),
                (T1, e.pk, self.atestado().pk),
            )
            self.assertTrue(DP.absorcao_vigente(self.alvo(q).get()))

```

- **Com recusa, VERMELHO esperado em `f300431af`.** A primeira falha esperada é `assertIn(q.pk, recusadas)`. O laudo registra `[] != [6]` no vizinho com a mesma célula. O código atual relê Q, testa resposta posterior, verifica carimbo e chama `carimbar`. A lâmpada acesa permite que o C6 dê Q por encerrada com o carimbo aplicado. A passagem então insere `ABSORCAO`. Execução do texto novo [nao verificado]. `verificador.md:2–6`, anexo · `app/chamados/services/materializacao.py:1080–1140` · `app/chamados/models.py:3474–3491` · `app/chamados/juizes.py:1544–1555`
- **Sem recusa, VERDE esperado em `f300431af`.** A interposição observa a mesma primeira irmã absorvida e deixa Q respondida. Q deve ter uma `ABSORCAO` vigente em `T1`, ligada ao mesmo escape e ao mesmo atestado. Execução [nao verificado]. `app/chamados/tests/test_e4d_matriz_reds.py:1330–1379,1401–1438` · `app/chamados/services/materializacao.py:1106–1133` · `app/chamados/models.py:4527–4550`

### B.2 — `:577` e o caso 11

Retire a exigência de assinatura de `:577` **e** as chamadas com `pedido_id` do caso 11. O grupo 3 decide que uma nova reabertura admitida deixa ato sem efeito funcional e que um retry conserva estado e trilha. Fica em aberto como distinguir as duas entradas. O teste executável abaixo fixa a primeira parte. As asserções de retry e de novo pedido de `:591–606` ficam como especificação para a entrada que vier a ser decidida. `r2.txt:5`, grupo 3, anexo · `app/chamados/tests/test_e4d_matriz_reds.py:570–608` · `app/chamados/services/disputa_emissao.py:1631,1698–1717`

No mesmo arquivo, retire `from inspect import signature` (1). Substitua de `    def test_caso_11_reabertura_vazia_grava_ato_sem_projetar(self):` até antes de `    def test_caso_12_proteger_cada_referencia_de_autoria(self):` (1 de cada). **VERMELHO esperado** em `f300431af` pela lista de atos vazia. O filtro de `:1713–1715` exclui a pendente vazia. Execução [nao verificado]. `app/chamados/tests/test_e4d_matriz_reds.py:10,570–608` · `app/chamados/services/disputa_emissao.py:1664–1669,1698–1717`

```python
    def test_caso_11_reabertura_vazia_grava_ato_sem_projetar(self):
        self.celula()
        p = self.cartao()
        outra = self.cartao(motivo='intervalo_volta')
        eventos = self.eventos()
        alvo = self.alvo(p) | self.alvo(outra)

        with escritas() as operacoes:
            disputa_emissao.reabrir_para_correcao(alvo, por=self.admin, motivo='Conferir hora')

        for q in (p, outra):
            self.assertEqual(list(q.decisoes.values_list(
                'ato', 'ato_em', 'ato_por_id', 'ato_motivo', 'ato_texto',
                'respondida_original_em', 'validada_original_em', 'validada_original_por_id')),
                [(DP.REABERTURA, T1, self.admin.pk, 'Conferir hora', '', None, None, None)])
            self.assertEqual(self.foto(q), VAZIA)
        for campo in RVM:
            self.assertEqual(posicoes_da_coluna(operacoes, campo), [])
        self.assertEqual(self.eventos(), eventos)

```

### B.3 — Brief para o construtor

Na passagem de `materializacao.py`, use Q da releitura de `:1080`, com o dono já travado em `:1075–1079` e a sequência pré-carregada de `:1059`. Antes de `q.carimbar` em `:1108`, consulte a vigência da `RECUSA` nessa sequência. Centralize a leitura do ato junto aos leitores de `DecisaoPergunta`. A composta já perdeu o tipo do ato que limpou a resposta. Para a vigência da recusa, proponho usar o último entre `RECUSA`, `RESPOSTA` e `REABERTURA`, pela ordem `(ato_em, pk)` da sequência de resposta. Use as linhas já visíveis na releitura sob a trava, inclusive a `RECUSA` gravada em `T2` após a captura do relógio da passagem em `T1`. A porta humana é a escritora de `ato='recusa'` e o campo de autoria admite `SET_NULL`. `app/chamados/services/materializacao.py:1047–1059,1074–1109` · `app/chamados/models.py:4344–4349,4455–4456,4486–4515` · `app/chamados/services/validacao.py:989–1000`

Quando a recusa estiver vigente, use o caminho de retorno e trilha de `:1139–1149` para Q, com `motivo='recusa_humana_vigente'` e uma descrição explícita. Assim o ramo deixa Q em `recusadas`, protege seu destino na lógica existente e impede tanto o carimbo quanto a inserção de `ABSORCAO` para Q. As decisões sobre compensação das irmãs, interrupção da passagem e identidade persistente continuam no grupo 6. `app/chamados/services/materializacao.py:1106–1159` · `r2.txt:8`, grupo 6, anexo

Censo dos consumidores na cópia:

- O chamador de produção em `disputa_emissao` dispara a passagem após a resposta de escape e descarta `recusadas`. O reexport em `models` expõe a função. O helper de teste intercepta e lê a lista. `app/chamados/services/disputa_emissao.py:12,1595–1600` · `app/chamados/models.py:4755` · `app/chamados/tests/test_e4d_matriz_reds.py:1361–1380`
- A própria passagem usa `carimbadas_aqui` para reencontrar as âncoras sob a trava e `perguntas_absorvidas_por` para conferir recuperação. A devolução por ausência rejeitada lê esse mesmo agrupador antes e sob a trava. O consumidor de ausência agenda a devolução no commit. A passagem também publica `irma_viva_na_ausencia` na trilha de auditoria. `app/chamados/services/materializacao.py:1064–1084,1115–1149,1179–1212,1241–1274` · `app/ponto/signals.py:255–277`
- A entrada de resposta lê os dois vínculos do ato via H4. Isso repercute na API, na resposta web e nas duas entradas de declaração de hora. A tarefa do dia também lê o ato e o status de `absorcao_ausencia`. `app/chamados/models.py:4527–4550,4766` · `app/chamados/services/disputa_emissao.py:1508–1512,1621–1628` · `app/api/views.py:2653–2658` · `app/chamados/views.py:1277–1279` · `app/chamados/services/declaracao_texto.py:447,727–729` · `app/chamados/services/tarefa_do_dia.py:165–192,374–403`
- O teste H4 de `:364–380` exige a absorção **depois** da `RECUSA` já gravada. Atualize essa expectativa no pouso da cura. Os testes de H4 cujo ponto de partida é uma absorção vigente continuam cobrindo seu próprio veto. `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:351–385`

### B.4 — Mutantes e catálogo

Em `app/chamados/tests/e4d_matriz_mutantes.py`, insira antes da âncora `        '24_sem_veto_de_grupo': (materializacao._materializar_ausencia_de_resposta, (` (1). Cada par abaixo é o original e o mutado exatos, com original de contagem 1 no arquivo de produção. `app/chamados/tests/e4d_ma

[truncado em 20000 de 38400 caracteres]

## 28/11 10:52 · sessao ccfe968d6055 · E4C-F5-INSTANTE · leitura · origem=Code

# r.md — E4C-F5-INSTANTE · cura6-lote-b

**Resultado esperado na cópia informada como `c9fded10a`: 6 VERDES, com alterações em testes e nas tabelas dos executores.** As seis substituições casaram uma vez e compilaram em memória. H2, C11 e aplicação passaram com a fonte original e caíram nas asserções indicadas com seus mutantes. Conferi contador, absorção e carimbo com os corpos de produção e persistência instrumentada. Execução ORM, Ruff, identidade Git e produção ficam **[nao verificado]**. `app/chamados/models.py:3503,4843` · `app/ponto/portas/celula.py:873` · `app/chamados/services/materializacao.py:1133` · `app/chamados/services/vigia_ato_x_coluna.py:29` · `app/chamados/tests/e4_verificar.py:59` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:106`

Nos cinco mutantes já cadastrados, troque `antes` e `depois` nas entradas pelo nome. Os caminhos em `arquivo` são relativos a `app/`. `app/chamados/tests/e4_verificar.py:16,59` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:15,106`

## 3-H2

Em `EventosAplicadosTest`, substitua o método ancorado por `def test_H2_ato_e_evento_no_mesmo_atomic_inclusive_falha(self):`. Use os imports existentes de `N`, `funcao` e `fonte_real`, o helper `modulo` e a fixture `ExpandTest` já vinculada à classe. `app/chamados/tests/test_e4_expand.py:11,15,28,34,78,277,396`

```python
    def test_H2_ato_e_evento_no_mesmo_atomic_inclusive_falha(self):
        banco = self.b
        pergunta = self.pergunta()
        celula = (1, pergunta.disputa.colaborador_id, pergunta.data_evento)

        def filtrar(*, pk__in):
            self.assertEqual(pk__in, {pergunta.pk})
            return N(values_list=lambda *campos: [celula])

        def bulk(linhas):
            self.assertGreater(banco.profundidade, 0)
            banco.gravacoes.extend(linhas)
            return linhas

        def emitir(linhas):
            self.assertGreater(banco.profundidade, 0)
            self.assertEqual(len(banco.gravacoes), 1)
            banco.eventos.extend(linhas)

        def falhar(linhas):
            emitir(linhas)
            raise RuntimeError('outbox indisponivel')

        class DP(N):
            ATO_NOVO, ESTOQUE = 'ato', 'estoque'
            ATOS_QUE_COMPENSAM = ('reabertura', 'devolucao', 'recusa')
            objects = N(bulk_create=bulk)
            _lote_de_estoque = classmethod(funcao(MODELS, '_lote_de_estoque'))
            _no_ultimo_ato = classmethod(funcao(MODELS, '_no_ultimo_ato'))

        self.env['DecisaoPergunta'] = DP
        self.modules['django.db.models'] = modulo('django.db.models', Max=object())
        self.pd.objects = N(filter=filtrar)
        self.pd.linhas_do_evento = funcao(MODELS, 'linhas_do_evento')
        self.pd.acordar_celulas = emitir
        fonte = fonte_real(MODELS)
        lote = [dict(pergunta=pergunta, ato='resposta', ato_em=AGORA)]

        self.executar(MODELS, 'registrar', DP, lote, fonte=fonte)
        self.assertEqual(len(banco.gravacoes), 1)
        self.assertEqual(banco.eventos, [celula])

        banco.gravacoes.clear()
        banco.eventos.clear()
        self.pd.acordar_celulas = falhar
        with self.assertRaisesRegex(RuntimeError, 'outbox indisponivel'):
            self.executar(MODELS, 'registrar', DP, lote, fonte=fonte)
        self.assertEqual((banco.gravacoes, banco.eventos), ([], []))
```

**`M-H2-evento` · 1 ocorrência.** Cai em `self.assertEqual(banco.eventos, [celula])`, com `[]`. **VERDE esperado.** `app/chamados/models.py:4828,4841` · `app/chamados/tests/e4_verificar.py:33`

```python
arquivo = 'chamados/models.py'
antes = '                PerguntaDisputa.acordar_celulas(eventos)'
depois = '                pass  # sem evento'
```

## 3-C11

Em `EventosAplicadosTest`, mantenha o método ancorado por `def test_C11_reversao_emite_dentro_do_atomic_do_DNA(self):`. Os imports existentes são `ast` e `fonte_real`. A fixture é a fonte de `reverter_regeneracao_dia`. `app/chamados/tests/test_e4_expand.py:6,16,277,313` · `app/ponto/portas/celula.py:803`

```python
    def test_C11_reversao_emite_dentro_do_atomic_do_DNA(self):
        path = 'ponto/portas/celula.py'
        source = fonte_real(path)
        fn = next(n for n in ast.walk(ast.parse(source))
                  if isinstance(n, ast.FunctionDef) and n.name == 'reverter_regeneracao_dia')
        blocos = [n for n in ast.walk(fn) if isinstance(n, ast.With)
                  and any('transaction.atomic' in ast.unparse(i.context_expr) for i in n.items)]
        self.assertEqual(len(blocos), 1)
        calls = [ast.unparse(n) for n in blocos[0].body if isinstance(n, ast.Expr)]
        self.assertIn('emitir_evento_celula(cel)', calls)
```

**`M-C11-reversao` · 1 ocorrência.** Cai em `self.assertIn('emitir_evento_celula(cel)', calls)`. **VERDE esperado.** `app/ponto/portas/celula.py:854,868,873` · `app/chamados/tests/e4_verificar.py:21`

```python
arquivo = 'ponto/portas/celula.py'
antes = '        emitir_evento_celula(cel)\n        _hoje = hoje'
depois = '        pass  # sem evento\n        _hoje = hoje'
```

## 3-absorção

Em `AusenciaVolta3Test`, substitua o método ancorado por `def test_RED_coluna_nula_e_ato_validado_sem_texto_desfaz_a_absorcao(self):`. Use `D`, `registrar_na_fixture`, `T0` e a fixture existente, com relógio congelado. A chamada explicita `FALTA_DIA`. `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:10,20,23,26,37,196` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:86`

```python
    def test_RED_coluna_nula_e_ato_validado_sem_texto_desfaz_a_absorcao(self):
        escape, irma = self._escape(), self._irma_da_celula(acesa=True)
        registrar_na_fixture([dict(pergunta=irma, ato=D.VALIDACAO, ato_em=T0, ato_por=self.admin)])
        self.assertEqual(D.par_ato_coluna(irma), (True, False))
        self.assertFalse(D.instantes_do_par_divergem(irma))

        recusadas, _ = self._passagem(escape, resposta='FALTA_DIA')

        irma.refresh_from_db()
        self.assertEqual(recusadas, [irma.pk])
        self._sem_absorcao(irma)
        self.assertEqual(D.par_ato_coluna(irma), (True, False))
        self.assertEqual(self._trilha(irma).get().valor_depois['motivo'], 'par_ato_coluna_divergente')
```

**`absorcao_sem_comparador` · 1 ocorrência.** Retira o braço de presença. A primeira asserção que deve cair é `self.assertEqual(recusadas, [irma.pk])`, com `[]`. **VERDE esperado.** `app/chamados/services/materializacao.py:1133,1149,1160` · `app/chamados/models.py:4683` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:31`

```python
arquivo = 'chamados/services/materializacao.py'
antes = 'validada_pelo_ato != validada_na_coluna'
depois = 'False'
```

## 3-carimbo

Em `GuardaParS143Test`, mantenha o método ancorado por `def test_RED_destino_recusa_b_persistido_e_a_mesa_recusa_o_mesmo_pk(self):`. Use os imports do módulo e a fixture existente sob `@freeze_time(AGORA)`. `_afirmar_recusa_sem_escrita` observa o `carimbar` real e confere a foto persistida e os PKs dos atos. `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:2,24,30,53,70,78,91`

```python
    def test_RED_destino_recusa_b_persistido_e_a_mesa_recusa_o_mesmo_pk(self):
        pai, destino = self.chamado(), self.chamado()
        pergunta = self._resposta_com_validacao_devolvida_e_coluna_antiga(pai=pai, destino=destino)
        Batida.objects.bulk_create([Batida(
            colaborador=self.colab, timestamp=timezone.make_aware(datetime.combine(DIA, time(19))),
            tipo='S', origem='app', aprovada=True,
        )])
        self.assertTrue(juizes._pergunta_materializada_de_fato(pergunta), 'premissa: batida real no slot de saída')
        antes = self._estado_gravado(pergunta)
        self._afirmar_recusa_sem_escrita(pergunta, destino, 'validar', antes)
        pai.refresh_from_db()
        self.assertEqual(pai.status_local, 'em_analise')

        request = RequestFactory().post('/', {
            'acao': 'recusar_pergunta', 'chamado_id': destino.pk, 'pergunta_id': pergunta.pk,
            'recusa_motivo': 'Esclarecer a hora declarada',
        })
        request.user = self.admin
        with patch.object(fio, 'contexto_parcial_fio', return_value={}), \
                patch('core.respostas.resposta_render', side_effect=lambda *args, **kw: kw['tipo']):
            resultado = fio.processar_post_fio(request, self.colab, lambda tipo, msg: tipo)
        self.assertEqual(resultado, 'sucesso')
        pergunta.refresh_from_db()
        self.assertEqual((pergunta.estado, pergunta.respondida_em, pergunta.resposta_colab),
                         (PerguntaDisputa.PENDENTE, None, ''))
        self.assertEqual(D.par_ato_coluna(pergunta), (False, False))
        self.assertEqual(pergunta.decisoes.get(ato=D.RECUSA).ato_por_id, self.admin.pk)
```

**`tirar_guarda` · 1 ocorrência.** Retira o braço de presença. A primeira asserção que deve cair é a de `tentativas`, que espera `[('validar', False, PerguntaDisputa.RECUSA_PAR_ATO_COLUNA)]`. **VERDE esperado.** `app/chamados/models.py:3497,3503,3522` · `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:78` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:65`

```python
arquivo = 'chamados/models.py'
antes = 'DecisaoPergunta.par_ato_coluna(self) == (False, True)'
depois = 'False'
```

## 3-contador

Em `ContadorAtoXColunaTest`, insira antes de `def test_recorte_do_chamador_define_o_universo(self):`. Use os imports existentes e a fixture `BaseCortes`. As duas perguntas têm colunas gravadas. A primeira tem dois campos `b` novos e a segunda tem um `b` anterior ao marco, classificado pelo comparador real como indeterminado. `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:17,25,71,77` · `app/chamados/tests/cortes_f2_f9_apoio.py:40,58,77` · `app/chamados/models.py:4625,4638`

```python
    def test_contador_ignora_b(self):
        from datetime import timedelta

        nova = self.pergunta(respondida_em=RESPOSTA_EM, validada_em=VALIDACAO_EM, validada_por=self.admin)
        antiga = self.pergunta(respondida_em=MARCO_DA_FIXTURE - timedelta(seconds=1))
        perguntas = PerguntaDisputa.objects.filter(pk__in=(nova.pk, antiga.pk))

        resultado = contar(perguntas)

        self.assertEqual((resultado['b']['numero'], resultado['indeterminado']['b']), (1, 1))
```

**`contador_ignora_b` · 1 ocorrência.** Cai na comparação com `(1, 1)`, com resultado `(0, 0)`. **VERDE esperado.** `app/chamados/services/vigia_ato_x_coluna.py:23,29` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:95`

```python
arquivo = 'chamados/services/vigia_ato_x_coluna.py'
antes = '            (novo if eh_novo else indeterminado)[classe] += 1'
depois = "            if classe != 'b':\n                (novo if eh_novo else indeterminado)[classe] += 1"
```

## 3-aplicação

Insira em `FontesAplicadosTest`, antes de `def test_fontes_aplicados_compilam_sem_escrever_modelos(self):`. Usei o módulo existente `test_e4_expand.py` como ponto de colagem para o teste do WIP. A fixture lê a fonte pelos helpers da cópia e cobre as cinco entradas corrigidas. `app/chamados/tests/test_e4_expand.py:270` · `app/chamados/tests/e4_catracas.py:5` · `app/chamados/tests/e4_verificar.py:59` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:106`

```python
    def test_ancoras(self):
        from textwrap import dedent

        from chamados.tests.e4_verificar import MUTANTES as E4, mutacao
        from chamados.tests.e4c_f5_volta3_mutantes import MUTANTES as VOLTA3, substituir

        for nome in ('M-H2-evento', 'M-C11-reversao'):
            with self.subTest(mutante=nome):
                arquivo, antes, depois, _ = E4[nome]
                fonte = fonte_real(arquivo)
                self.assertEqual(fonte.count(antes), 1, nome)
                with mutacao(nome):
                    alterada = fonte_real(arquivo)
                    self.assertNotEqual(alterada, fonte)
                    self.assertEqual(alterada, fonte.replace(antes, depois, 1))
                    compile(alterada, arquivo, 'exec')
                self.assertEqual(fonte_real(arquivo), fonte)

        for nome in ('absorcao_sem_comparador', 'tirar_guarda', 'contador_ignora_b'):
            with self.subTest(mutante=nome):
                modulo_alvo, alvo, antes, depois = VOLTA3[nome]
                arquivo = modulo_alvo.replace('.', '/') + '.py'
                fonte = fonte_real(arquivo)
                self.assertEqual(fonte.count(antes), 1, nome)
                no = ast.parse(fonte, filename=arquivo)
                for parte in alvo.split('.'):
                    no = next(n for n in no.body if getattr(n, 'name', None) == parte)
                corpo = dedent(''.join(fonte.splitlines(keepends=True)[no.lineno - 1:no.end_lineno]))
                self.assertEqual(corpo.count(antes), 1, nome)
                alterado = substituir(nome, corpo)
                self.assertNotEqual(alterado, corpo)
                self.assertEqual(alterado, corpo.replace(antes, depois, 1))
                compile(alterado, arquivo, 'exec')
```

**`ancora_c11_parenteses` · 1 ocorrência.** Com as cinco entradas corrigidas, injete a fonte mutada em `FONTES_MUTADAS[arquivo]` via `patch.dict` ao rodar `test_ancoras`. Os parênteses extras preservam o AST da chamada. O subteste `M-C11-reversao` cai em `self.assertEqual(fonte.count(antes), 1, nome)`, com `0 != 1`, antes de chamar `mutacao`. **VERDE esperado.** `app/ponto/portas/celula.py:873` · `app/chamados/tests/e4_catracas.py:5,8` · `app/chamados/tests/test_e4_expand.py:313`

```python
arquivo = 'ponto/portas/celula.py'
antes = '        emitir_evento_celula(cel)\n        _hoje = hoje'
depois = '        emitir_evento_celula((cel))\n        _hoje = hoje'
```

| Item | Teste | Mutantes | Esperado | [nao verificado]
|---|---|---|---|
| 3-H2 | `test_H2_ato_e_evento_no_mesmo_atomic_inclusive_falha` | `M-H2-evento` | VERDE · `app/chamados/models.py:4843` |
| 3-C11 | `test_C11_reversao_emite_dentro_do_atomic_do_DNA` | `M-C11-reversao` | VERDE · `app/ponto/portas/celula.py:873` |
| 3-absorção | `test_RED_coluna_nula_e_ato_validado_sem_texto_desfaz_a_absorcao` | `absorcao_sem_comparador` | VERDE · `app/chamados/services/materializacao.py:1135` |
| 3-carimbo | `test_RED_destino_recusa_b_persistido_e_a_mesa_recusa_o_mesmo_pk` | `tirar_guarda` | VERDE · `app/chamados/models.py:3503` |
| 3-contador | `test_contador_ignora_b` | `contador_ignora_b` | VERDE · `app/chamados/services/vigia_ato_x_coluna.py:29` |
| 3-aplicação | `test_ancoras` | `ancora_c11_parenteses` | VERDE · `app/chamados/tests/e4_verificar.py:63` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:108` |

**Total: 6 mutantes**, cinco recadastrados e um estrutural para a aplicação. `app/chamados/tests/e4_verificar.py:21,33` · `app/chamados/tests/e4c_f5_volta3_mutantes.py:31,65,95` · `app/ponto/portas/celula.py:873`

## 28/11 10:51 · sessao fa9413bbde70 · INTERVALO-SEM-PREVISTO · leitura · origem=Code

## 2. Chamado #40588 — porta na tela e retenção

**A tela carece de um botão para encerrar as perguntas mudas #61520 e #61521 com motivo.** A operação existe no modelo, `DisputaSupervisao.admin_encerra(self, user, motivo)`. Ela encerra a disputa inteira, carimba as perguntas mudas vivas como `admin_decidiu` e registra usuário, instante e motivo em `DecisaoPergunta.SILENCIO`. Uma resposta sem veredito veta a operação. A ligação do botão **Encerrar com motivo** à tela continua registrada em `C7-TELA-ADMIN-ENCERRA`. `app/chamados/models.py:2122–2146,2184–2217` · `app/docs/BACKLOG.md:1640`

- **Rejeitar resposta** aparece no fio em `/chamados/modal/<colaborador_id>/`, na view `modal_fio_colaborador`, template `modal_fio.html`. O detalhe `/chamados/local/<chamado_id>/`, view `detalhe_chamado_local`, usa o botão **Rejeitar** em `_pergunta_card.html`. Ambos exigem uma resposta existente e motivo. O ato registra a recusa e **devolve a pergunta ao colaborador**. Para uma pergunta já pendente, a porta devolve `ja_pendente`. `app/config/urls.py:28` · `app/chamados/urls.py:14,39` · `app/chamados/views.py:359,506–519,870,928` · `app/templates/chamados/partials/modal_fio.html:217,314–326` · `app/templates/chamados/partials/_pergunta_card.html:29,42,117–129` · `app/chamados/services/validacao.py:948–958,995–1028`
- **Regularizar disputa e fechar** fica atrás da condição de todas as perguntas respondidas no fio. **Resolver dia** é uma decisão sobre o dia inteiro, via `POST /ponto/celula/veredito/`, view `veredito_celula_view`, template `_veredito_dia.html`. A medição registrada do #40588 encontrou perguntas sem resposta e **Resolver dia** indisponível porque o estado do dia não pedia decisão. O encaminhamento registrado é **CHAMADO-PRESO-CLASSE**. `app/templates/chamados/partials/modal_fio.html:354–389` · `app/ponto/urls.py:58` · `app/ponto/views.py:3517–3546` · `app/templates/ponto/partials/_veredito_dia.html:6–25` · `app/colaboradores/services/calendario.py:94–104` · `app/docs/RELATO.md:595–596,603`

**O que segura 20/11.** A leitura de 24/11 registrou `cobrado` via cartório, todas as lâmpadas existentes acesas, `marcos_inexistentes=True` e `marcos_acesos=None`. O cartório conta o `fio_vivo` dos chamados vivos e pode lavrar `cobrado` quando o chamado ainda protesta. Ele tenta reconciliar a cobrança por marco extinto, mas a guarda do chamado veta o fecho se sobra pergunta viva ancorada sem destino para reatar. `app/docs/RELATO.md:596,599` · `app/ponto/services/lastro.py:98–115,153–181` · `app/chamados/juizes.py:1609–1619` · `app/ponto/services/cartorio.py:1418–1423,1498–1508,1540–1545,1600–1617` · `app/chamados/models.py:429–450`

O TXT lê o **veredito lavrado** em `motivos_retencao_celula`, que pode devolver `espelho_cobrado`. Se as duas forem as últimas perguntas do fio que sustenta a cobrança e a relavra ficar sem outro protesto, o dia vira `concorde` e sai da retenção por espelho. O Retrato usa o mesmo retentor, lê o fio para identificar de quem depende a pendência e mostra a foto da próxima lavra. Outros dias da competência ainda podem segurar a linha no TXT. A situação atual em `juliani` está [nao verificado]. `app/escala/models.py:1780–1786` · `app/folha/export.py:984–995,1292–1335` · `app/ponto/tests/test_o261_cobrado_le_o_fio.py:126–150` · `app/folha/fora_da_folha.py:115–122,146–165,220–238` · `app/folha/views_retrato.py:194–205`

**Texto para o admin**

1. Abra o **Painel de Chamados** em `/chamados/painel/?colab_id=549`. Clique na linha **#40588** e confira no fio as cobranças intermediárias de 20/11, referências **#61520** e **#61521**. `app/templates/chamados/painel_gestao.html:451–461` · `app/templates/chamados/partials/_lista_chamados.html:42` · `app/templates/chamados/partials/modal_fio.html:76–86,157–162` · `app/docs/RELATO.md:596` · vínculo exato das duas PKs [nao verificado].
2. Abra o **Espelho** em `/ponto/espelho/?colab=549&mes=11&ano=2026` e confira a linha de **20/11**. `app/config/urls.py:29` · `app/ponto/urls.py:10` · `app/ponto/views.py:98–126`
3. Registre o caso para **CHAMADO-PRESO-CLASSE** com este motivo. “Chamado **#40588**, célula **#122468**, perguntas **#61520** e **#61521**. O previsto de 20/11 é **07:00–19:00, sem intervalo**. As cobranças de **11:00 e 12:00** referem-se a marcos ausentes da célula atual. Após a correção, confira se 20/11 saiu dos dias retidos no Retrato.” `app/docs/RELATO.md:595–603` · `app/docs/BACKLOG.md:835–836` · horários e PKs informados no pedido [nao verificado].

## 1. Emissor e fonte — censo de nascimento

O modelo é `PerguntaDisputa`. O censo estático encontrou **quatro chamadas de produção** a `PerguntaDisputa.abrir` para perguntas datadas. Todas passam por `_sob_contrato` e `_nascer`, que faz a inserção. A porta de conversa também chega a `_nascer`, com dia civil e sem marco de turno. `app/chamados/models.py:2717–2746,3088–3133,3330–3344` · `app/chamados/services/disputa_emissao.py:453,851` · `app/chamados/services/declaracao_texto.py:404,702`

| Caminho | Fonte do intervalo |
|---|---|
| `gerar_perguntas_disputa` | Recebe motivo e dia do envelope e abre sem conferir se o motivo corresponde a uma pausa da célula. No fluxo normal, a **fábrica da célula** já escolheu o motivo pela ata, via `_ata_do_dia` → `_marcos_faltantes` → `grade_da_celula`. O teto temporal também lê os marcos do vínculo do dia via `EC.marcos_do_dia`. `app/chamados/services/disputa_emissao.py:300–325,353–418,451–461,901–927` · `app/chamados/services/fabrica_da_celula.py:35–74,160–174` · `app/ponto/services/cartorio.py:884–912` · `app/escala/servico_jornada.py:69–85` |
| `_criar_pergunta_se_couber`, no repasse de órfãos | O ramo legado `categoria == 'intervalo_ausente'` confia em `chamado.contexto_json['marcos_faltantes']`. Usa **tipo e data** dessa lista para abrir `intervalo_saida` ou `intervalo_volta`, sem conferir o DNA ou a ata do dia. **Esse ramo contorna S133.** O repasse de `batida_ausente` com marco nomeado e os complementos dos outros ramos leem os faltantes da ata. `app/chamados/services/disputa_emissao.py:736–758,837–898,1088–1095,1149–1192,1223–1233,1254–1264` · `CLAUDE.md:152–177` |
| `declarar_hora_colab` e `aplicar_declaracao` | Os outros dois locais de abertura conferem a declaração contra `_marcos_faltantes`, que lê a ata da célula. Podem criar a pergunta correspondente para registrar a declaração. `app/chamados/services/declaracao_texto.py:87–144,339–346,401–405,643–657,683–703` |

O detector `detectar_intervalo_ausente` lê DNA e ata e roda como vigia. `reconciliar_grade` tem caminho que grava **chamado**, agendado sem `--apply`. A reabertura administrativa abre uma pergunta de **conversa**, e `classificar_falta` devolve motivos das pontas do turno. Esses caminhos explicam as referências a intervalo fora dos quatro locais de abertura datada. `app/ponto/management/commands/detectar_intervalo_ausente.py:29–88,93–109,150–158` · `app/ponto/management/commands/reconciliar_grade.py:488–521` · `app/config/crons.py:460–462,491` · `app/chamados/services/acoes_disputa.py:289–309` · `app/escala/servico_jornada.py:459–492`

**Para o literal**, o #40588 está documentado como `batida_ausente` com `orfa_regeneracao`. Essa marca guarda os horários cobrados antes da reescrita da célula. O catálogo da pergunta interpola a **data**. O fio busca a **hora prevista** em `marcos_faltantes` do chamado-destino. A validação tem ainda um fallback pelo `TipoEscala` ativo, que é um leitor de hora. O ramo que efetivamente inseriu #61520/#61521 e a origem exata de 11:00/12:00 na emissão estão [nao verificado]. Os caminhos de nascimento encontrados usam ata ou contexto de cobrança, sem regra fixa de seis horas. `app/docs/PROMPTS.md:2875` · `app/docs/RELATO.md:599` · `app/ponto/management/commands/gerar_celulas.py:232–264` · `app/chamados/catalogo/perguntas.py:123–138` · `app/chamados/services/fio.py:446–457` · `app/chamados/juizes.py:572–629,1982–2010`

## 3. Sonda Django — somente leitura

Texto para colar no shell Django já aberto com acesso leitor ao schema `juliani`. `vivas` conta o universo de cada empresa pelo juiz C6. `sem_intervalo` exige célula localizada pelo colaborador e dia do turno, com zero pares completos no DNA. `sem_celula` conta separadamente as vivas cujo dia ficou sem célula. A sonda usa a janela inclusiva pedida e consulta as duas pausas pelo leitor da célula. `bin/sonda_leitura.sh:235–240` · `app/chamados/tests/cef_c3_sonda.py:16–26` · `app/chamados/juizes.py:339–343,911,1875` · `app/escala/utils.py:111–146` · `app/escala/models.py:1843–1847`

```python
from datetime import date

from django.db import connection, transaction

from chamados.juizes import MOTIVOS_DE_INTERVALO, pergunta_viva
from chamados.models import ChamadoColaborador, DecisaoPergunta, PerguntaDisputa
from core.models import LogAuditoria
from escala.models import CelulaDia
from escala.utils import pausas_de_celulas
from folha.export import motivos_retencao_celula


def sondar():
    if connection.schema_name != "juliani" or connection.in_atomic_block:
        raise RuntimeError("Use um shell do schema juliani fora de uma transação.")

    empresas = (2, 3, 4)
    ini, fim = date(2026, 10, 21), date(2026, 11, 20)
    pks = (61520, 61521)
    contagem = {e: {"vivas": 0, "sem_intervalo": 0, "sem_celula": 0}
                for e in empresas}

    with transaction.atomic(), connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        celulas = {(c.colaborador_id, c.data): c for c in CelulaDia.objects.filter(
            colaborador__empresa_id__in=empresas, data__range=(ini, fim))}
        perguntas = PerguntaDisputa.objects.select_related(
            "disputa__colaborador__empresa", "celula", "chamado_destino"
        ).prefetch_related("decisoes")
        universo = perguntas.filter(
            conversa=False, motivo__in=MOTIVOS_DE_INTERVALO,
            data_evento__range=(ini, fim), disputa__colaborador__empresa_id__in=empresas,
        )

        for p in universo.order_by("pk").iterator(chunk_size=500):
            if not pergunta_viva(p):
                continue
            colab = p.disputa.colaborador
            dia = p.data_evento
            chave = (colab.pk, dia)
            c = celulas.get(chave)
            linha = contagem[colab.empresa_id]
            linha["vivas"] += 1
            if c is None:
                linha["sem_celula"] += 1
            elif not pausas_de_celulas(colab.pk, dia, dia, celulas={chave: c}).get(dia):
                linha["sem_intervalo"] += 1

        for empresa in empresas:
            print("empresa", empresa, contagem[empresa])
        print("total", {k: sum(contagem[e][k] for e in empresas)
                        for k in ("vivas", "sem_intervalo", "sem_celula")})

        caso = CelulaDia.objects.select_related("colaborador__empresa").get(pk=122468)
        ata = caso.ata or {}
        print("celula", caso.pk, "colab", caso.colaborador_id, "dia", caso.data,
              "marcos", CelulaDia.marcos_presentes((caso.dna or {}).get("marcos")),
              "veredito", caso.veredito, "protestos", caso.protestos,
              "previsto", ata.get("minutos_previstos"),
              "realizado", ata.get("minutos_realizados"))
        print("regenerada_em", caso.regenerada_em, "regeneracoes", caso.regeneracoes,
              "marcos_dna_anterior",
              CelulaDia.marcos_presentes((caso.dna_anterior or {}).get("marcos")))

        ch = ChamadoColaborador.objects.get(pk=40588)
        print("chamado", ch.pk, ch.modulo_origem, ch.status_local)
        achadas = perguntas.in_bulk(pks)
        for pk in pks:
            p = achadas.get(pk)
            if p is None:
                print("pergunta", pk, "ausente")
                continue
            ato = DecisaoPergunta.composta(p)
            print("pergunta", p.pk, "colab", p.colaborador_id, "dia", p.data_evento,
                  "motivo", p.motivo, "celula", p.celula_id, "disputa", p.disputa_id,
                  "chamado_pai", p.disputa.chamado_id, "destino", p.chamado_destino_id,
                  "estado", p.estado, "viva", pergunta_viva(p),
                  "respondida_em", ato.respondida_em, "validada_em", ato.validada_em,
                  "materializada_em", ato.materializada_em, "via", ato.via_resolucao)

        for pk, quando in LogAuditoria.objects.filter(
                acao="pergunta_emitida", modelo="PerguntaDisputa", objeto_id__in=pks
        ).order_by("criado_em").values_list("objeto_id", "criado_em"):
            print("emissao", pk, quando)

        motivos, dias = motivos_retencao_celula(caso.colaborador, ini, fim, com_dias=True)
        print("retencao_da_competencia", motivos, dias)


sondar()
```

**Assinaturas da aplicação**

| Chamada | Assinatura |
|---|---|
| Juiz C6 | `pergunta_viva(p, hoje=None, *, carga=None, agora=None)` · `app/chamados/juizes.py:911` |
| Pausas pelo DNA | `pausas_de_celulas(colab_id, ini, fim, celulas=None)` · `app/escala/utils.py:111` |
| Marcos para exibição | `CelulaDia.marcos_presentes(cls, marcos)` · `app/escala/models.py:1683` |
| Rótulo e carimbos | `PerguntaDisputa.estado(self)` é propriedade, acessada como `p.estado`. `DecisaoPergunta.composta(cls, p)` lê a resposta e os carimbos. · `app/chamados/models.py:3350–3370,4507–4515` |
| Retenção | `motivos_retencao_celula(colab, ini, fim, hoje=None, com_dias=False)` · `app/folha/export.py:1192` |

As chamadas de infraestrutura são `atomic(using=None, savepoint=True, durable=False)`, `connection.cursor()` e `execute(sql, params=None)`. As chamadas de `QuerySet`, na forma vinculada, são `filter(*args, **kwargs)`, `select_related(*fields)`, `prefetch_related(*lookups)`, `order_by(*field_names)`, `iterator(chunk_size=None)`, `get(*args, **kwargs)`, `in_bulk(id_list=None, *, field_name="pk")` e `values_list(*fields, flat=False, named=False)`. As assinaturas do pacote instalado estão [nao verificado]. `app/requirements.txt:13–16` · `app/chamados/tests/cef_c3_sonda.py:16–26`

A sintaxe foi compilada em memória. A execução no Django, os totais e a identidade da cópia com o HEAD de `main` estão [nao verificado]. O DNA anterior da saída representa somente a versão imediatamente anterior à última regeneração. `app/escala/models.py:1715–1741`

## 4. Cura do emissor — desenho

- Em `criar_disputa_a_partir_de_chamados_orfaos`, o ramo `intervalo_ausente` deve resolver o dia pelo `dia_do_fato` e consumir os **marcos intermediários existentes no DNA e apagados na ata desse dia**, pela leitura que já alimenta `_ata_do_dia` e `_complementar_marcos_faltantes`. Para a concordância entre DNA e ata, já existe `periodos_do_dia(celula, tipo_escala)`, com os papéis por marco. Uma célula sem pausa fornece zero marcos intermediários. Ata ausente ou desalinhada pede abstenção e sinalização pela porta existente. O contexto do chamado permanece como trilha da cobrança. `app/chamados/services/disputa_emissao.py:864–927,1149–1192` · `app/chamados/juizes.py:89` · `app/ponto/juiz_batida.py:153–180,199–208,230–252` · `app/ponto/management/commands/gerar_celulas.py:232–264`
- A mesma autoridade já atende a **fábrica por evento**, o **repasse de `batida_ausente` pela ata** e as **declarações do colaborador e do admin**. O **cartório do furo parcial** já cruza marcos apagados com `EC.marcos_do_dia` e `EC.pausas_do_dia`, incluindo o segundo intervalo. `gerar_perguntas_disputa` deve receber os motivos de intervalo já apurados por essa fonte. A cobrança histórica do #40588 tem o encaminhamento próprio já registrado, **CHAMADO-PRESO-CLASSE**. `app/chamados/services/fabrica_da_celula.py:52–74,160–174` · `app/chamados/services/disputa_emissao.py:403–418,1182–1192` · `app/chamados/services/declaracao_texto.py:343,643–657` · `app/ponto/services/cartorio.py:1967–2004` · `app/docs/RELATO.md:595–603`

**`padrao=Single Source of Truth / Aggregate Root`** — `CelulaDia` governa o previsto, e a ata fornece o estado de cada marco. O desenho toca `disputa_emissao.py`, arquivo compartilhado com a E4C. `CLAUDE.md:152–177` · `app/docs/GLOSSARIO.md:11` · `app/docs/PLANO-PADROES.md:53–67` · `app/docs/RELATO.md:2765`

**Perguntas de lei — 0.** S133 e a seção 4a já escolhem a fonte da emissão, e o encaminhamento do caso literal já está registrado. `CLAUDE.md:152–177,278–283` · `app/docs/RELATO.md:595–603`

## 28/11 10:50 · sessao ad72913ba694 · FDS-FERIAS-COM-BATIDA-PORTA · leitura · origem=Code

## 1. Motivos e telas existentes

`motivos_retencao_celula` devolve **oito códigos possíveis**. Com `com_dias=True`, retorna `(motivos, dias)`, com `data`, `motivo`, `veredito` e `celula_id` por dia retido. O resultado vazio é `[]` ou `([], [])`, conforme o modo. Se faltam tanto a célula quanto um vínculo que cubra o dia, o laço pula a data. Uma célula existente com veredito acusador pode reter mesmo depois do fim do vínculo. `app/folha/export.py:1250–1287,1333–1384` · `app/escala/models.py:1780–1781`

| Motivo diário | Tela existente e alcance |
|---|---|
| `sem_celula` | **Calendário**, `colaboradores:calendario_ponto` → `calendario_ponto` → `colaboradores/calendario_partial.html`. Abre só o dia com `cid` e `?modo=custom&data_ini=AAAA-MM-DD&data_fim=AAAA-MM-DD`. Pode oferecer **Resolver dia** quando o status e as demais guardas permitem. O escritor lavra o veredito humano quando existe célula. Tela genérica para gerar ou relavrar a célula faltante [nao verificado]. `app/colaboradores/urls.py:24` · `app/colaboradores/views.py:1986–2020` · `app/colaboradores/services/calendario.py:94–104,117–122,188–241` · `app/ponto/services/veredito_celula.py:351–363` |
| `espelho_furo` | **Calendário** para conferir ou corrigir marcação e, quando oferecido, **Resolver dia**. **Fio**, `chamados:modal_fio` → `modal_fio_colaborador` → `chamados/partials/modal_fio.html`, quando há ação publicada no fio. O calendário aceita `cid` e data, o modal abre por colaborador e carrega seu fio inteiro. `app/escala/models.py:1622–1623` · `app/colaboradores/urls.py:24` · `app/colaboradores/services/calendario.py:227–241` · `app/templates/colaboradores/partials/_calendario_grade.html:130–140` · `app/chamados/urls.py:39` · `app/chamados/views.py:870–890,928–957` |
| `espelho_discordante` | **Calendário do dia** para a divergência de marcação. O mesmo veredito também cobre realizado inflado e realizado zero com turno. Uma tela única que corrija qualquer uma dessas causas [nao verificado]. `app/escala/models.py:1627` · `app/colaboradores/urls.py:24` · `app/colaboradores/views.py:1986–2020` · `app/colaboradores/services/calendario.py:227–241` · `app/templates/colaboradores/partials/_calendario_grade.html:130–140` |
| `espelho_indefinida` | **Parcial sem período:** `ponto:painel_ausencia` → `painel_ausencia` → `ponto/partials/painel_ausencia.html`, direto pelo `aid`. **Vigência ou fase da escala:** `colaboradores:painel_vinculo` → `painel_vinculo` → `colaboradores/partials/painel_vinculo.html`, por `cid`, com início de vigência e controle de fase. O painel abre o vínculo, sem seleção de dia na URL. **Dia da agenda:** `colaboradores:agenda_edit_dia` → `agenda_edit_dia_view` → `colaboradores/partials/agenda_edit_dia.html`, direto por `cid` e data ISO. A possibilidade de edição depende do estado da agenda. `app/escala/models.py:1626` · `app/ponto/urls.py:72` · `app/ponto/views.py:2095–2119` · `app/colaboradores/urls.py:12–19` · `app/colaboradores/views.py:263–295,307–320,2461–2509` · `app/templates/colaboradores/partials/painel_vinculo.html:136–176` |
| `espelho_cobranca_indevida` | **Conferir chamado**, `chamados:painel_chamado_turno` → `painel_chamado_turno` → `chamados/partials/painel_chamado_turno.html`, direto por `chamado_id`. O painel mostra o registro e oferece resposta e mudança de status. A reconciliação e a nova lavra que podem retirar a retenção passam pelo cartório. Tela universal de correção para o caso sem chamado associado [nao verificado]. `app/chamados/urls.py:15` · `app/chamados/views.py:347–356` · `app/templates/chamados/partials/painel_chamado_turno.html:64–87` · `app/ponto/services/cartorio.py:1618–1629` |
| `espelho_cobrado` | **Fio do colaborador** para acompanhar a cobrança ou agir sobre resposta ou decisão disponível. **Calendário do dia** quando a autoridade publica **Resolver dia**. Se a rota publicada é `porta_app`, a ação exigível é do colaborador. O modal aceita colaborador, e o calendário aceita colaborador e data. `app/chamados/urls.py:39` · `app/chamados/views.py:870–890,928–957` · `app/chamados/services/tarefa_do_dia.py:752–810` · `app/colaboradores/urls.py:24` · `app/colaboradores/views.py:1986–2020` · `app/colaboradores/services/calendario.py:94–104,227–241` |
| `espelho_nunca_bateu` | A porta de triagem já definida é **Cadastro x Realidade, C5**, `escala:cadastro_x_realidade` → `cadastro_x_realidade` → `escala/cadastro_x_realidade.html`. A lista tem ocorrências por dia e aponta para `colaboradores:detalhe` → `detalhe_colaborador` → `colaboradores/detalhe.html`, por `cid`, para tratar adesão ao app ou explicar o dia por ausência. A URL da C5 abre a lista. O formulário `ponto:lancar_ausencia` → `lancar_ausencia` → `ponto/lancar_ausencia.html` recebe colaborador e data no POST. `app/escala/urls.py:14` · `app/escala/views.py:512–537` · `app/escala/services/cadastro_realidade.py:19,72–80,139–175,235–243,757–759` · `app/colaboradores/urls.py:23` · `app/colaboradores/views.py:477–487` · `app/ponto/urls.py:57` · `app/ponto/views.py:2501–2512,2631–2635` |
| `espelho_fato_em_ausencia` | **Painel da ausência**, direto pelo `aid`, se o lançamento precisa de correção. **Calendário do dia**, por `cid` e data, para examinar a marcação e retratá-la se foi indevida. O calendário já liga o dia ao registro de ausência por `?registro=<aid>`. A escolha depende de qual registro está errado. `app/ponto/supra_juiz.py:393–418` · `app/ponto/urls.py:53,62,72,77–78` · `app/ponto/views.py:2095–2119,2930–2945,3453–3464` · `app/templates/colaboradores/partials/_calendario_grade.html:101–104,130–135` · `app/templates/ponto/ausencias.html:200–211` |

No caminho do Retrato, **`furo_espelho` é o motivo agregado**. O export guarda nele os códigos da célula em `motivos_espelho`. `fora_da_folha` obtém os pares por dia com `com_dias=True`, e `montar_linhas` os copia como `motivos_por_dia`. **`ferias_com_batida` vem de `dias_ferias_com_batida`**, antes desse ramo, e a linha recebe as datas do item do export. As telas existentes são **Férias do colaborador**, `ferias:secao_ferias_colab` → `secao_ferias_colab` → `ferias/_secao_colab.html`, por `cid`, com ações sobre o agendamento, e **Calendário do dia**, por `cid` e data, para retratação da marcação. `app/folha/export.py:464–505,950–958,1042–1047,1387–1393` · `app/folha/fora_da_folha.py:115–122,164–183,239–247` · `app/folha/retrato_folha.py:1285–1320` · `app/ferias/urls.py:14,18–22` · `app/ferias/views.py:832–861` · `app/templates/ferias/_secao_colab.html:146–166` · `app/colaboradores/urls.py:24`

Os demais motivos de linha são `sem_codigo_dominio` e `codigo_dominio_invalido`, corrigíveis em `colaboradores:editar_colaborador` → `editar_colaborador` → `colaboradores/editar.html`, por `cid`; `cadastro_zero`, com vínculo/escala como destino quando falta cadastro do período; `nao_homologado`, com `folha:lista_rubricas` → `lista_rubricas` → `folha/rubricas_quadro.html` para corrigir o vínculo evento–rubrica, por empresa; e `rescisao_modulo_proprio`, encaminhado ao módulo de rescisão do Domínio. Rota local para resolver a rescisão do Domínio [nao verificado]. O item que entra tem `motivo=None`, gravado como `''` na linha. `app/folha/export.py:905–958,1020–1055` · `app/folha/retrato_folha.py:1283–1303` · `app/colaboradores/urls.py:50` · `app/colaboradores/views.py:1267–1298,1394–1400` · `app/templates/colaboradores/editar.html:62` · `app/escala/services/cadastro_realidade.py:746–752` · `app/folha/urls.py:8` · `app/folha/views.py:29–81`

## 2. Portas oferecidas hoje e efeito do c1

- **Ação publicada, fio e tipo.** `portas_da_linha` começa pelas ações. `validar` e `decidir` abrem o modal do fio, `decidir_ausencia` abre o painel com `?somente_decisao=1`, e `resolver_dia` abre o calendário com `ano`, `mes` e `dia`. Nesse último link o calendário cai no modo mensal, que usa mês e ano. O `dia` gravado na ação, por si só, **não recorta o calendário para um dia**. Sem ação traduzida, entram `Validar no fio`, `Cobrar no fio` ou `Abrir no fio`, conforme o fio. O tipo é complementar. `escala`, `folga` e `sem_escala` dão **Vínculo da escala**. `cadastro` dá **Cadastro do colaborador**. `nao_bate`, `falta`, `ferias` e `sem_causa` ficam sem link de tipo. `app/folha/retrato_folha.py:1023–1055,1077–1116` · `app/colaboradores/services/calendario.py:194–241`
- **Destino diário do OS6c.** Com `com_motivos_por_dia=True`, dia com `celula_id` e sem rota publicada pela tarefa, `espelho_cobranca_indevida` dá **Conferir chamado** para cada ID com verbo `cobrar` ligado à data. `espelho_indefinida` dá **Conferir ausência** se a cobertura de cobrança devolveu um registro na data, e **Vínculo da escala** caso contrário. Os outros seis códigos ficam sem destino diário por esse ramo. `PORTA_DO_DESTINO_RETENCAO` converte `tela` em rota. O botão recebe a data e o rótulo do motivo, e o `href` leva somente o ID do destino. Como essa etapa acrescenta links depois da deduplicação dos links agregados, o vínculo pode aparecer uma vez como link da linha e novamente para cada dia. `app/folha/fora_da_folha.py:125–140,239–247,275–278` · `app/folha/retrato_folha.py:1057–1061,1104–1126` · `app/templates/folha/_retrato_o_que_fazer.html:67–76`
- **O c1 acrescenta um retorno antecipado para `ferias_com_batida`.** Se há datas, abre **Calendário do dia (retratar marcação)**, com `modo=custom` e início e fim iguais à *primeira* data. Sempre acrescenta **Férias do colaborador (corrigir lançamento)**. A sobreposição está no ponto de apresentação, `portas_da_linha`. O c1 lê o motivo agregado e sai antes de ações, fio e tipo. O OS6c lê o motivo diário, acrescenta os destinos depois desses ramos e depende da falta de rota da tarefa. Na cópia atual, férias com batida cai no fio `admin` e oferece **Validar no fio**, porque `ferias` tem porta de tipo vazia. `c1.diff:1078–1093` · `app/folha/retrato_folha.py:247–253,385–424,1023–1046,1084–1126,1291–1320` · `app/docs/RELATO.md:15107`

## 3. Os quatro casos

| Caso | Motivo recebido e oferta hoje | Porta pelo aval |
|---|---|---|
| **col344 · 07/11** | O motivo exibido é **`ferias_com_batida`**, do classificador do export. O retorno independente de `motivos_retencao_celula` para essa data [nao verificado]. Hoje, **Validar no fio**. `app/docs/RELATO.md:15107` · `app/folha/export.py:950–958` · `app/folha/retrato_folha.py:408–409,1023–1045,1107–1116` | **Férias do colaborador**, `/ferias/344/secao/`, para a decisão sobre o lançamento. O c1 também oferece o calendário de 07/11 para a hipótese de marcação indevida. `app/config/urls.py:26–32` · `app/ferias/urls.py:14` · `app/templates/ferias/_secao_colab.html:146–166` · `c1.diff:1078–1093` |
| **col327 · 10/11** | **`espelho_indefinida`**. No caminho fornecido, sem rota da tarefa, **Conferir ausência**, `/ponto/ausencias/5515/painel/`. `PARCIAL_SEM_PERIODO` é um dos protestos que lavram `indefinida`. `app/docs/RELATO.md:14897` · `app/escala/models.py:1622–1627` · `app/folha/export.py:1379–1382` · `app/folha/fora_da_folha.py:134–139,244–247` · `app/ponto/urls.py:72` | **O mesmo painel, no registro 5515, com “Salvar periodo”**. O formulário aparece para ausência parcial aprovada e envia `hora_ini` e `hora_fim` ao POST `ponto:editar_periodo_ausencia`. O formulário é nativo, e o POST exige `editar_ausencias`. A condição do formulário de período é independente de `pode_editar`, flag usada pela edição geral do painel. A abertura real com as permissões do operador [nao verificado]. `app/templates/ponto/partials/painel_ausencia.html:145–176,310–327` · `app/ponto/views.py:2107–2119,2835–2850` · `app/ponto/urls.py:68` |
| **col97 · ímpares de 01 a 19/11** | **`espelho_indefinida`**, em **10 dias** no dado fornecido. Hoje o OS6c oferece **Vínculo da escala** por dia, `/colaboradores/vincular/97/painel/`. `app/docs/RELATO.md:14899` · `app/folha/export.py:1351–1359,1379–1382` · `app/folha/fora_da_folha.py:134–139,244–247` · `app/colaboradores/urls.py:12` | **Vínculo da escala**, para corrigir vigência ou fase no cadastro. O painel já traz início de vigência e o controle de fase. Resultado após corrigir e relavrar [nao verificado]. `app/docs/RELATO.md:14899` · `app/templates/colaboradores/partials/painel_vinculo.html:136–176` · `app/colaboradores/views.py:393–405,429–448` |
| **col476 · 25/10** | **`espelho_indefinida`**. Hoje o OS6c cai em **Vínculo da escala**. No caso fornecido, `escala_suspeita` fica fora dos status que oferecem **Resolver dia**, e as perguntas vivas também fecham a guarda de último recurso. `app/docs/RELATO.md:14898` · `app/folha/export.py:1379–1382` · `app/folha/fora_da_folha.py:134–139,244–247` · `app/colaboradores/services/calendario.py:94–104,117–122` | **Editor do dia da agenda**, `colaboradores:agenda_edit_dia`, `/colaboradores/vincular/476/agenda/2026-10-25/editar/`. A URL abre o dia. A view exige `editar_agenda` e libera edição para rascunho ou dia sem `AgendaDia`. O modelo descreve a agenda como informativa para escalas flexíveis. Estado da agenda e efeito sobre a retenção deste caso [nao verificado]. `app/config/urls.py:26` · `app/colaboradores/urls.py:19` · `app/colaboradores/views.py:2461–2509` · `app/escala/models.py:1410–1422` |

**Lei do col327.** O texto do aval `COBERTURA-PARCIAL-PERIODO` está em `CORTES.md` e no registro correspondente de `CORTES.json`. `LEIS.md` indexa o corte e traz a L-006, de cadastro pela UI. Parcial sem horas declaradas é cadastro do DP e produz `PARCIAL_SEM_PERIODO`. O botão **Salvar periodo** chama a mesma porta de decisão da ausência aprovada, que grava o período. `app/docs/CORTES.md:462` · `app/docs/CORTES.json:1095–1099` · `app/docs/LEIS.md:64,354` · `app/ponto/supra_juiz.py:393–402` · `app/ponto/views.py:2835–2850` · `app/ponto/services/ausencia.py:1099–1111`

## 4. Desenho e censo de leitores

**Proposta.** Uma tabela `PORTA_POR_MOTIVO` em `folha/fora_da_folha.py`, junto da projeção de destinos. A chave é o motivo já emitido. A tabela produz os descritores de porta para a lavra, com data, motivo, rota e argumentos. `montar_linhas` persiste os descritores, `portas_da_linha` monta os `href`s e o template desenha os botões. Durante a transição, o renderer continua aceitando os descritores OS6c de retratos já gravados, `{data, motivo, tela, id}`. Ações de tarefa continuam disponíveis no lugar em que são publicadas. Nos dias com destino causal identificado, o drawer usa esse destino para responder à retenção, inclusive se houver rota de tarefa. `app/folha/fora_da_folha.py:125–140,239–258` · `app/folha/retrato_folha.py:1084–1126,1272–1322` · `app/templates/folha/_retrato_o_que_fazer.html:67–76`

- **Férias com batida** entra nessa tabela pelo item e pelas datas já entregues pelo export. A seção de férias é compartilhada pelo colaborador. Cada dia pode ter seu próprio link de calendário, usando o recorte do c1. **Cobrança indevida** reaproveita os IDs e verbos que a carga já associou ao dia. **Nunca bateu** tem a C5 como destino de triagem já definido. `app/folha/export.py:950–958` · `app/folha/retrato_folha.py:1291–1296` · `c1.diff:1078–1093` · `app/folha/fora_da_folha.py:129–133` · `app/chamados/services/tarefa_do_dia.py:738–751` · `app/escala/services/cadastro_realidade.py:72–80`
- **Indefinida** precisa de mais contexto dentro da entrada da tabela. O mesmo código cobre `PARCIAL_SEM_PERIODO`, `FASE_NAO_DECLARADA_COM_PREVISTO` e `PREVISTO_COM_DIA_INDEFINIDO`. A projeção pode ler os protestos **já lavrados**, em lote pelos `celula_id` recebidos, e usar a cobertura existente para obter o `aid` da parcial. Para distinguir falta de vínculo de ajuste pontual, pode ler a resposta da autoridade `vinculo_do_dia`, com a célula e os vínculos já carregados. Um registro de ausência presente na data, sozinho, é evidência insuficiente de parcial sem período. Para a agenda, reutilizar também o carregador que ativa o JavaScript do editor. O drawer genérico insere o HTML com `innerHTML`, enquanto o modal de agenda instala explicitamente os scripts do editor. `app/escala/models.py:1622–1627,1759–1763` · `app/folha/export.py:1345–1348` · `app/ponto/turnos.py:3157–3170` · `app/escala/alimentacao.py:228–277` · `app/templates/core/_drawer_generico.html:19–35` · `app/templates/colaboradores/partials/agenda_modal.html:220–240`
- **Leitores em produção na cópia.** `painel_colaborador` lê a linha do retrato gravado, chama `portas_da_linha` e passa o resultado a `_retrato_o_que_fazer.html`. É o leitor que efetivamente consome `destinos_retencao`. `o_que_segura` é o outro chamador direto de `portas_da_linha`. Passa uma projeção diária com `colab`, `dono`, `tipo='falta'` e `acoes`, hoje sem `destinos_retencao`, e retorna o resultado em `dias[].portas`. A lavra copia o campo e persiste o JSON de linhas. Outros leitores diretos fora da cópia [nao verificado]. `app/colaboradores/views.py:993–1017` · `app/templates/colaboradores/partials/painel_colaborador.html:26` · `app/templates/folha/_retrato_o_que_fazer.html:67–76` · `app/folha/fora_da_folha.py:310–327` · `app/folha/retrato_folha.py:1117–1126,1317–1320` · `app/folha/management/commands/lavrar_retrato_folha.py:63–79`
- **PDF, lista, Raio-X e TXT.** O PDF do Retrato, a lista e o Raio-X leem `motivos_diarios`, que usa `motivos_por_dia` para exibir texto. O TXT monta linhas a partir de `classificar_export` e `montar_linhas_export`, assim como o PDF de validação do TXT. O PDF de retidos também usa `classificar_export`. Esses caminhos de exportação ficam fora do consumo de `portas_da_linha` e `destinos_retencao`. `app/folha/retrato_folha.py:99–119` · `app/folha/views_retrato.py:279–295,437–449,532–539` · `app/folha/views.py:220,276–289,885–886` · `app/folha/export.py:757–789` · `app/folha/validacao_pdf.py:65–67`

**Perguntas de lei.** Para abrir as quatro telas pedidas, **0**. Para generalizar a promessa de que o link resolve a causa, ficam duas perguntas. `app/folha/fora_da_folha.py:125–140` · `app/escala/models.py:1410–1422,1622–1631`

1. **Dia indefinido sem parcial nem problema de vigência/fase identificado.** Qual evidência já publicada autoriza o editor da agenda como porta causal, e qual ato do DP vale para corrigir o dia quando a agenda é apenas informativa? O aval indica a tela de col476. O efeito dela sobre o retentor [nao verificado]. `app/escala/models.py:1410–1422,1626` · `app/docs/BACKLOG.md:1196,1643` · `app/colaboradores/views.py:2461–2509`
2. **Outros dias sem destino causal demonstrado.** Qual encaminhamento o DP quer quando falta lavra ou quando o mesmo motivo admite correções incompatíveis e os dados gravados são insuficientes para escolher? Uma porta única por código para esses casos [nao verificado]. `app/folha/export.py:1350–1384` · `app/escala/models.py:1622–1631` · `app/folha/fora_da_folha.py:125–140`

## 5. Sonda de leitura

O universo é a montagem **em memória** de todos os itens da competência. Para `furo_espelho`, a sonda chama o retentor e confere os pares contra a linha montada. Para férias, usa as datas do item já copiadas pela montagem. A matriz conta pares únicos `(pk, data)` e colaboradores distintos em cada célula. `dia` indica um botão datado pelo OS6c ou uma ação com `dia_do_turno` publicado cujo link saiu no drawer. `linha` indica os links agregados oferecidos à pessoa. `sem dia` preserva a contagem de pessoas das linhas sem data, com zero dias. Um dia pode aparecer em mais de uma porta, por isso o total do universo é contado à parte. `app/folha/fora_da_folha.py:164–183,239–258` · `app/folha/retrato_folha.py:1084–1126,1233–1265,1272–1322` · `app/chamados/services/tarefa_do_dia.py:813–826`

**Assinaturas exatas das funções da aplicação chamadas**

| Função | Assinatura |
|---|---|
| Montagem do Retrato | `montar_linhas(empresa, mes, ano, *, agora, hoje=None)` · `app/folha/retrato_folha.py:1233` |
| Renderização de portas | `portas_da_linha(linha)` · `app/folha/retrato_folha.py:1064` |
| Retentor diário | `motivos_retencao_celula(colab, ini, fim, hoje=None, com_dias=False)` · `app/folha/export.py:1250` |
| Janela da competência | `janela_fechamento(mes, ano, empresa=None)` · 

[truncado em 20000 de 25052 caracteres]

## 28/11 10:50 · sessao 1b0c83935ae0 · E4C-F5-INSTANTE · leitura · origem=Code

# r.md · E4C-F5-INSTANTE OS6c

**Verificação em memória:** 7/7 testes de regra verdes em `c9fded10a`, 7/7 vermelhos em `f6ba2bc4e` e 12/12 mutantes Python detectados. Ruff passou com `app/ruff.toml`. O render pelo Django e os dois mutantes de template seguem [nao verificado]. `app/folha/fora_da_folha.py:239` · `app/folha/retrato_folha.py:1117` · `app/ruff.toml:5` · `app/templates/folha/_retrato_o_que_fazer.html:74`

A fixture entrega os fatos sintéticos à lavra real com `save=False`. A execução da reconciliação, da edição da ausência e da troca de fase pelas telas segue [nao verificado]. `app/ponto/portas/celula.py:1127` · `app/ponto/services/cartorio.py:1618` · `app/ponto/views.py:2930` · `app/templates/colaboradores/partials/painel_vinculo.html:157`

## `app/folha/tests/test_e4c_os6_destinos_retencao.py`

```python
"""OS6c: destinos administrativos por dia retido no Retrato."""
import re
from datetime import date, datetime, timedelta, timezone
from types import SimpleNamespace
from unittest.mock import Mock, patch

from django.template import Context, engines
from django.test import SimpleTestCase, override_settings
from django.urls import reverse
from django.utils.html import escapejs

from chamados.services.tarefa_do_dia import _portas_da_tarefa
from colaboradores.models import Colaborador
from colaboradores.services.calendario import _pode_ultimo_recurso, resolver_dia_oferecido
from escala.models import CelulaDia, EscalaColaborador
from folha.retrato_folha import montar_linhas, portas_da_linha
from ponto.models import FechamentoMensal
from ponto.status_dia import status_da_lavra
from ponto.turnos import _dia_das_batidas_puro

HOJE = date(2026, 11, 28)
AGORA = datetime(2026, 11, 28, 15, tzinfo=timezone.utc)
EMPRESA = SimpleNamespace(pk=1, dia_inicio_competencia=21)
INDEFINIDO = 'Dia indefinido no espelho'
VINCULO = 'colaboradores:painel_vinculo'
TRABALHO = {'tipo_dia': 'trabalho', 'tem_batida': True, 'previsto_min': 660, 'isento': False}
FOLGA = {'tipo_dia': 'folga', 'tem_batida': False, 'previsto_min': 0, 'isento': False}


def _colab(pk, demissao=None):
    return SimpleNamespace(
        pk=pk, id=pk, nome='', empresa=EMPRESA, codigo_dominio=str(pk),
        data_admissao=None, data_demissao=demissao,
        situacao='desligado' if demissao else 'ativo',
    )


def _celula(data, veredito=None):
    return CelulaDia(pk=data.toordinal(), data=data, veredito=veredito, protestos=[])


def _relavrar(celula, fatos):
    with patch('core.observ.evento'):
        celula.lavrar_veredito(via='cartorio', codigos=[], agora=AGORA, save=False, fatos_do_status=fatos)


def _tarefa(celula, *, verbos=None, perguntas=(), pendentes=(), resolver=False):
    verbos = verbos or {}
    tarefa = {
        'celula_id': celula.pk, 'nao_sei': False,
        'instancias': [
            {'pergunta_id': pk, 'acoes': [{'acao': 'responder', 'de': 'colaborador', 'porta': None}]}
            for pk in perguntas
        ],
    }
    _portas_da_tarefa(
        tarefa, {'fila_ids': set(verbos), 'fila_cel': {celula.pk: verbos}, 'verbo': verbos},
        pendentes_app=frozenset(pendentes), resolver_oferecido=resolver,
    )
    return tarefa


def _lavrar(colab, celulas, tarefas, *, ausencias=None):
    fechamento = SimpleNamespace(
        colaborador=colab, colaborador_id=colab.pk, mes=11, ano=2026,
        horas_trabalhadas=8, turnos_abertos=0, inconsistencias=[],
    )
    fechamentos = Mock()
    fechamentos.select_related.return_value.order_by.return_value = [fechamento]
    fechamentos.values_list.return_value = []
    escalas = Mock()
    escalas.values_list.return_value = []
    coberturas = {(colab.pk, 'cobranca'): ausencias or {}}
    carga = SimpleNamespace(
        tarefa=lambda c, dia: tarefas[c.pk, dia],
        cobertura=lambda c, modo: coberturas[c.pk, modo],
    )
    with (
        patch.object(FechamentoMensal.objects, 'filter', return_value=fechamentos),
        patch.object(EscalaColaborador.objects, 'filter', return_value=escalas),
        patch.object(Colaborador.objects, 'filter', side_effect=lambda *, pk__in: Mock(
            select_related=Mock(return_value=[colab] if colab.pk in pk__in else []),
        )),
        patch.object(CelulaDia.objects, 'filter', return_value=celulas),
        patch('escala.alimentacao.escalas_do_periodo', return_value=[]),
        patch('folha.export.codigos_homologados', return_value=set()),
        patch('folha.export._rubricas_map', return_value={}),
        patch('folha.export.dias_ferias_com_batida', return_value={}),
        patch('folha.fora_da_folha._carga', return_value=carga),
        patch('django.utils.timezone.localdate', return_value=HOJE),
    ):
        linha, = montar_linhas(EMPRESA, 11, 2026, agora=AGORA, hoje=HOJE)
    return linha


def _portas_diarias(linha):
    return [(p['data'], p['motivo'], p['href']) for p in portas_da_linha(linha) if p.get('data')]


def _vinculo(pk):
    return reverse(VINCULO, args=[pk])


class DestinosRetencaoTest(SimpleTestCase):
    def test_cobranca_residual_abre_chamado(self):
        colab = _colab(246)
        cel = _celula(date(2026, 11, 1), 'cobranca_indevida')
        self.assertEqual(status_da_lavra(veredito=cel.veredito, **TRABALHO)[0], 'ok')
        tarefa = _tarefa(cel, verbos={36424: 'cobrar', 36425: None})
        self.assertIsNone(tarefa['rota'])
        linha = _lavrar(colab, [cel], {(colab.pk, cel.data): tarefa})
        self.assertEqual(_portas_diarias(linha), [
            (str(cel.data), 'Cobrança indevida', reverse('chamados:painel_chamado_turno', args=[36424])),
        ])
        _relavrar(cel, TRABALHO)
        self.assertEqual((cel.veredito, cel.status_dia), ('concorde', 'ok'))
        nova = _lavrar(colab, [cel], {})
        self.assertNotIn(str(cel.data), nova['datas'] or [])
        self.assertEqual(_portas_diarias(nova), [])

    def test_indefinida_abre_ausencia_da_data(self):
        colab = _colab(327)
        cel = _celula(date(2026, 11, 10), 'indefinida')
        tarefa = _tarefa(cel)
        self.assertIsNone(tarefa['rota'])
        ausencias = {date(2026, 11, 9): SimpleNamespace(pk=5514), cel.data: SimpleNamespace(pk=5515)}
        linha = _lavrar(colab, [cel], {(colab.pk, cel.data): tarefa}, ausencias=ausencias)
        self.assertEqual(_portas_diarias(linha), [
            (str(cel.data), INDEFINIDO, reverse('ponto:painel_ausencia', args=[5515])),
        ])
        _relavrar(cel, TRABALHO)
        self.assertEqual((cel.veredito, cel.status_dia), ('concorde', 'ok'))
        nova = _lavrar(colab, [cel], {})
        self.assertNotIn(str(cel.data), nova['datas'] or [])
        self.assertEqual(_portas_diarias(nova), [])

    def test_perguntas_fora_do_app_deixam_destino_no_vinculo(self):
        colab = _colab(476)
        cel = _celula(date(2026, 10, 25), 'indefinida')
        oferta = resolver_dia_oferecido({
            'data': cel.data, 'status': 'escala_suspeita',
            'pode_ultimo_recurso': _pode_ultimo_recurso(cel.data, date(2026, 11, 21), pergunta_viva=True),
        }, HOJE)
        self.assertFalse(oferta)
        tarefa = _tarefa(cel, perguntas=(31, 32, 33, 34), pendentes=(35,), resolver=oferta)
        self.assertIsNone(tarefa['rota'])
        linha = _lavrar(colab, [cel], {(colab.pk, cel.data): tarefa})
        self.assertEqual(_portas_diarias(linha), [(str(cel.data), INDEFINIDO, _vinculo(476))])

    def test_dez_datas_mantem_links_para_o_mesmo_vinculo(self):
        colab = _colab(97)
        datas = [date(2026, 11, dia) for dia in range(1, 20, 2)]
        celulas = [_celula(data, 'indefinida') for data in datas]
        tarefas = {(colab.pk, cel.data): _tarefa(cel) for cel in celulas}
        linha = _lavrar(colab, celulas, tarefas)
        self.assertCountEqual(_portas_diarias(linha), [
            (str(data), INDEFINIDO, _vinculo(97)) for data in datas
        ])
        for cel in celulas:
            _relavrar(cel, FOLGA)
        fuso = timezone(timedelta(hours=-3))
        for dia, hora_saida in ((2, (6, 0)), (18, (5, 58))):
            inicio = date(2026, 11, dia)
            entrada = SimpleNamespace(pk=1, timestamp=datetime(2026, 11, dia, 17, 56, tzinfo=fuso))
            saida = SimpleNamespace(pk=2, timestamp=datetime(2026, 11, dia + 1, *hora_saida, tzinfo=fuso))
            turno = SimpleNamespace(data_turno=inicio, batidas=[entrada, saida])
            por_batida = _dia_das_batidas_puro([turno], [entrada, saida])
            self.assertEqual(por_batida, {entrada.pk: inicio, saida.pk: inicio})
            cel = _celula(por_batida[saida.pk])
            _relavrar(cel, TRABALHO)
            self.assertEqual((cel.data, cel.veredito, cel.status_dia), (inicio, 'concorde', 'ok'))
            celulas.append(cel)
        nova = _lavrar(colab, celulas, {})
        self.assertFalse(nova['datas'])
        self.assertEqual(_portas_diarias(nova), [])

    def test_rota_da_tarefa_exclui_so_o_dia_alcancavel(self):
        colab = _colab(701)
        sem_app = _celula(date(2026, 11, 3), 'indefinida')
        com_app = _celula(date(2026, 11, 4), 'indefinida')
        sem_rota = _tarefa(sem_app, resolver=None)
        com_rota = _tarefa(com_app, perguntas=(31,), pendentes=(31,))
        self.assertEqual((sem_rota['rota'], sem_rota['nao_sei']), (None, True))
        self.assertEqual((com_rota['rota'], com_rota['fio']), ('porta_app', 'colab'))
        linha = _lavrar(colab, [sem_app, com_app], {
            (colab.pk, sem_app.data): sem_rota, (colab.pk, com_app.data): com_rota,
        })
        self.assertCountEqual(linha['datas'], [str(sem_app.data), str(com_app.data)])
        self.assertNotIn(str(com_app.data), [d['data'] for d in linha.get('destinos_retencao', ())])
        self.assertEqual(_portas_diarias(linha), [(str(sem_app.data), INDEFINIDO, _vinculo(701))])

    def test_desligado_so_recebe_informativo(self):
        cel = _celula(date(2026, 11, 5), 'indefinida')
        desligado = _colab(702, demissao=date(2026, 11, 6))
        linha = _lavrar(desligado, [cel], {})
        self.assertEqual((linha['status'], linha['motivo']), ('fora', 'rescisao_modulo_proprio'))
        self.assertEqual(linha['informativo'], [str(cel.data)])
        self.assertNotIn(str(cel.data), linha['datas'] or [])
        self.assertFalse(linha.get('destinos_retencao'))
        self.assertEqual(_portas_diarias(linha), [])
        ativo = _colab(703)
        controle = _lavrar(ativo, [cel], {(ativo.pk, cel.data): _tarefa(cel)})
        self.assertEqual(_portas_diarias(controle), [(str(cel.data), INDEFINIDO, _vinculo(703))])

    def test_legada_preserva_o_link_agregado(self):
        legada = {'colab': 704, 'status': 'fora', 'motivo': 'furo_espelho', 'dono': 'ninguem', 'tipo': 'escala'}
        self.assertEqual([p['href'] for p in portas_da_linha(legada)], [_vinculo(704)])
        nova = dict(legada, destinos_retencao=[
            {'data': '2026-11-07', 'motivo': 'espelho_indefinida', 'tela': 'vinculo', 'id': 704},
        ])
        self.assertEqual(_portas_diarias(nova), [('2026-11-07', INDEFINIDO, _vinculo(704))])

    @override_settings(STORAGES={
        'default': {'BACKEND': 'django.core.files.storage.FileSystemStorage'},
        'staticfiles': {'BACKEND': 'django.contrib.staticfiles.storage.StaticFilesStorage'},
    })
    @patch('comunicados.services.texto_cobranca', return_value=('', ''))
    def test_partial_renderiza_o_link_diario_nas_duas_cascas(self, _texto_push):
        data = '2026-11-07'
        linha = {'colab': 705, 'status': 'fora', 'motivo': 'furo_espelho', 'dono': 'ninguem', 'tipo': 'escala'}
        nova = dict(linha, destinos_retencao=[
            {'data': data, 'motivo': 'espelho_indefinida', 'tela': 'vinculo', 'id': 705},
        ])
        molde = engines['django'].engine.from_string(
            '{% extends casca %}{% block content %}'
            "{% include 'folha/_retrato_o_que_fazer.html' %}{% endblock %}"
        )
        alvo = f"'{escapejs(_vinculo(705))}'"
        for casca in ('base.html', 'base_app.html'):
            with self.subTest(casca=casca):
                for registro in (linha, nova):
                    html = molde.render(Context({
                        'casca': casca, 'retrato_linha': registro, 'retrato_competencia': '11/2026',
                        'retrato_portas': portas_da_linha(registro),
                    }))
                    self.assertRegex(html, r'window\.abrirDrawer\s*=\s*function')
                    links = re.findall(r'<a\b[^>]*onclick="([^"]*)"[^>]*>([^<]*)</a>', html)
                    textos = [texto for clique, texto in links if 'window.abrirDrawer(' in clique and alvo in clique]
                    self.assertTrue(textos)
                    self.assertEqual(any(data in t and INDEFINIDO in t for t in textos), registro is nova, textos)
```

## Mutantes literais e asserções

Cada trecho original abaixo tem uma ocorrência na árvore. M1 e M3 incluem contexto para tornar a substituição literal única. `app/folha/fora_da_folha.py:133` · `app/chamados/services/tarefa_do_dia.py:752`

- **M1** `return [('chamado', pk) for pk, verbo in verbos.items() if verbo == 'cobrar']` → `return [('chamado', pk) for pk, verbo in verbos.items() if verbo == 'decidir']`. Cai a igualdade que exige o link do chamado `36424`. `app/folha/fora_da_folha.py:133`
- **M2** `return [('ausencia', ausencia.pk)]` → `return [('ausencia', colab.pk)]`. Cai a igualdade que exige o painel da ausência `5515`. `app/folha/fora_da_folha.py:138`
- **M3** A troca abaixo faz cair `assertIsNone(tarefa['rota'])` no caso das quatro perguntas fora das pendentes. `app/chamados/services/tarefa_do_dia.py:752`

  Original

  ```python
      colab_tem_porta = any(i['pergunta_id'] in pendentes_app and
          any(a['acao'] == RESPONDER and a['de'] == COLABORADOR for a in i['acoes'])
  ```

  Mutado

  ```python
      colab_tem_porta = any(i['pergunta_id'] is not None and
          any(a['acao'] == RESPONDER and a['de'] == COLABORADOR for a in i['acoes'])
  ```

- **M4** `for _destino in _destinos:` → `for _destino in _destinos[:1]:`. Cai `assertCountEqual` dos dez links diários. `app/folha/retrato_folha.py:1122`
- **M5** `if com_motivos_por_dia and not (_p or {}).get('rota'):` → `if com_motivos_por_dia:`. Cai `assertNotIn` da data que tem `porta_app`. `app/folha/fora_da_folha.py:245`
- **M6** `'informativo': _info.get(_cid, []),` → `'informativo': _c.get('destinos_retencao', []),`. Cai a igualdade do `informativo` com `['2026-11-05']`. `app/folha/retrato_folha.py:1315`
- **M7** `_destinos = linha.get('destinos_retencao')` → `_destinos = linha['destinos_retencao']`. A leitura da linha legada levanta `KeyError` ao avaliar a asserção do link agregado. `app/folha/retrato_folha.py:1117`
- **M8** `if _c.get('destinos_retencao'):` → `if _c.get('destino_retencao'):`. Caem as igualdades que exigem links diários após `montar_linhas`. `app/folha/retrato_folha.py:1319`
- **M9** `args=[_destino['id']]` → `args=[_cid]`. Caem as igualdades dos URLs do chamado `36424` e da ausência `5515`. `app/folha/retrato_folha.py:1124`
- **M10** `{% if p.data %}{{ p.data }} · {{ p.motivo }} · {% endif %}{{ p.rotulo }}` → `{{ p.rotulo }}`. Deve cair a asserção de data e motivo na âncora da linha nova. `app/templates/folha/_retrato_o_que_fazer.html:75`
- **M11** `_linha['destinos_retencao'] = _destinos_retencao` → `_linha['destinos_retencao'] = []`. Caem as igualdades que exigem links diários após `montar_linhas`. `app/folha/fora_da_folha.py:278`
- **M12** `return [('vinculo', colab.pk)]` → `return ()`. Caem as igualdades que exigem links diários para o vínculo. `app/folha/fora_da_folha.py:139`
- **M13** `{{ p.href|escapejs }}` → `{{ p.rotulo|escapejs }}`. Deve cair `assertTrue(textos)` porque a âncora perde o URL esperado na chamada do drawer. `app/templates/folha/_retrato_o_que_fazer.html:74`
- **M14** `if com_motivos_por_dia and not (_p or {}).get('rota'):` → `if com_motivos_por_dia and not _p:`. Cai a igualdade que exige o link do dia com `rota=None` e `nao_sei=True`. `app/folha/fora_da_folha.py:245` · `app/chamados/services/tarefa_do_dia.py:823`

## teste | mutantes | esperado [nao verificado]

| teste | mutantes | esperado | [nao verificado]
|---|---|---|
| `test_cobranca_residual_abre_chamado` | M1, M7, M8, M9, M11 | VERDE `c9fded10a` / VERMELHO `f6ba2bc4e` · `app/folha/fora_da_folha.py:133` |
| `test_indefinida_abre_ausencia_da_data` | M2, M7, M8, M9, M11 | VERDE / VERMELHO · `app/folha/fora_da_folha.py:138` |
| `test_perguntas_fora_do_app_deixam_destino_no_vinculo` | M3, M8, M11, M12 | VERDE / VERMELHO · `app/chamados/services/tarefa_do_dia.py:752` |
| `test_dez_datas_mantem_links_para_o_mesmo_vinculo` | M4, M7, M8, M11, M12 | VERDE / VERMELHO · `app/folha/retrato_folha.py:1122` |
| `test_rota_da_tarefa_exclui_so_o_dia_alcancavel` | M5, M8, M11, M12, M14 | VERDE / VERMELHO · `app/folha/fora_da_folha.py:245` |
| `test_desligado_so_recebe_informativo` | M6, M7, M8, M11, M12 | VERDE / VERMELHO · `app/folha/retrato_folha.py:1315` |
| `test_legada_preserva_o_link_agregado` | M7 | VERDE / VERMELHO · `app/folha/retrato_folha.py:1117` |
| `test_partial_renderiza_o_link_diario_nas_duas_cascas` | M7, M10, M13 | VERDE / VERMELHO [nao verificado] · `app/templates/base.html:399` · `app/templates/base_app.html:154` · `app/templates/folha/_retrato_o_que_fazer.html:74` |

**Total: 14 mutantes**, M1–M14 nos trechos citados acima.

## 28/11 10:49 · sessao e5758e844b5e · - · - · origem=Ronald (tmux)

**Emissor e fonte.** O único nascimento de `PerguntaDisputa` passa por `abrir`/`abrir_conversa` → `_sob_contrato` → `_nascer` → `save(force_insert=True)`. As perguntas datadas de intervalo podem chegar por três caminhos. `app/chamados/models.py:3088–3139,3330–3344`

1. `gerar_perguntas_disputa` recebe motivo e dia do envelope. Ele não valida a presença de um marco intermediário antes de abrir. Na fábrica normal, porém, quem escolhe o motivo é a ata soberana, via `motivos_pela_ata` → `_ata_do_dia` → `_marcos_faltantes` → `grade_da_celula`. O teto temporal consulta `EscalaColaborador.marcos_do_dia` pelo vínculo do dia. `app/chamados/services/disputa_emissao.py:300–325,353–418,451–461,901–927` · `app/chamados/services/fabrica_da_celula.py:35–74,160–175,217–219` · `app/ponto/services/cartorio.py:884–912` · `app/escala/servico_jornada.py:69–85`
2. No repasse de órfãos, o ramo `intervalo_ausente` lê `contexto_json.marcos_faltantes`, mapeia `S` para `intervalo_saida` e `E` para `intervalo_volta` e abre sem consultar a ata. É o nascimento que toma um snapshot lateral como autoridade para o dia, contrariando S133. Os ramos de complemento e de `batida_ausente` datada leem os faltantes da ata. `app/chamados/services/disputa_emissao.py:837–898,1088–1095,1149–1190,1223–1233,1254–1264` · `CLAUDE.md:152–161`
3. `declarar_hora_colab` e `aplicar_declaracao` reconhecem os marcos faltantes pelo mesmo leitor antes de eventualmente abrirem pergunta para a declaração. Reabertura administrativa pode carregar um motivo de intervalo, mas abre conversa sem marco. O detector dedicado é vigia e o comando `reconciliar_grade` emite somente chamado sob `--apply`, hoje ausente do cron. `app/chamados/services/declaracao_texto.py:339–346,401–405,643–657,691–703` · `app/chamados/services/acoes_disputa.py:289–309` · `app/ponto/management/commands/detectar_intervalo_ausente.py:29–88,93–109,150–158` · `app/ponto/management/commands/reconciliar_grade.py:488–532` · `app/config/crons.py:460–462,489–491`

**Prova do literal.** O chamado #40588 foi registrado como `batida_ausente`, com todos os marcos atuais acesos, `cobrado` e marca `orfa_regeneracao`. Essa marca guarda o que o chamado cobrava antes da reescrita do DNA. Os horários apresentados podem vir do contexto desse chamado, porque a pergunta de intervalo do catálogo carrega o dia e a tela extrai a hora do chamado pelo motivo. O ramo de nascimento específico das perguntas #61520/#61521 requer ler `LogAuditoria.pergunta_emitida` e comparar seu instante ao DNA anterior/regenerado [nao verificado]. A Defesa-1 tem um fallback paralelo pelo `TipoEscala` ativo, mas esse é leitor de hora, sem criação de pergunta. `app/docs/RELATO.md:595–599,603` · `app/ponto/management/commands/gerar_celulas.py:232–264` · `app/chamados/catalogo/perguntas.py:123–138` · `app/chamados/juizes.py:572–629,1982–2010,2030–2047,2157–2175` · `app/chamados/services/fio.py:446–457` · `app/chamados/models.py:3337–3347` · `app/core/observ.py:22–32,94–123`

**Porta.** `DisputaSupervisao.admin_encerra(self, user, motivo)` fecha a disputa inteira e suas perguntas mudas com usuário, motivo e ato `SILENCIO`. O censo não localizou uma chamada desse método por URL, view ou template. Na tela, `Rejeitar` devolve a resposta ao colaborador e recusa pergunta muda, `Regularizar disputa` barra pendências sem resposta e a oferta `Resolver dia` foi medida como falsa para #40588. A regra registrada encaminha #40588 pela classe de regeneração. `app/chamados/models.py:2122–2145,2184–2217` · `app/chamados/urls.py:8–55` · `app/templates/chamados/partials/_pergunta_card.html:117–125` · `app/chamados/services/validacao.py:948–959,995–1016` · `app/chamados/services/acoes_disputa.py:156–170` · `app/docs/RELATO.md:595–603,2863` · `app/docs/BACKLOG.md:835–836`

**Retenção.** A pergunta viva pode manter o chamado e, pelo fio, o veredito `cobrado`. O TXT retém pelo veredito já lavrado, inclusive `espelho_cobrado`. Tirar o fio remove essa causa de `cobrado` no próximo julgamento, desde que inexista outro protesto ou fio, e a foto do Retrato depende de nova lavra. O cartório já tenta fechar por marco extinto e passa pelo gate que recusa a pergunta viva. O caminho `COBRANCA_SEM_FURO` exige realizado ≥ previsto, portanto não decorre de 715/720 [nao verificado quanto aos valores do caso]. `app/chamados/models.py:429–447` · `app/ponto/services/cartorio.py:1423,1498–1508,1520–1545,1600–1617` · `app/ponto/supra_juiz.py:476–484` · `app/folha/export.py:1301–1322` · `app/folha/fora_da_folha.py:120–165` · `app/folha/retrato_folha.py:761,1256–1258`

**Cura.** O ramo dedicado em `disputa_emissao.py:1149–1170` deve receber o dia do turno e ler `_ata_do_dia`/`_marcos_faltantes`, emitindo apenas o marco intermediário existente e apagado no próprio dia. A mesma leitura já atende a fábrica, o repasse pela ata, a declaração do colaborador e a proposta administrativa. `CelulaDia.marcos_presentes(cls, marcos)` lê a representação própria da célula, incluindo duas pausas, e `EC.pausas_do_dia(self, data, celulas=None)` reconhece pares completos do DNA. A correção realiza **Single Source of Truth / Aggregate Root**, conforme L-123. `app/chamados/services/disputa_emissao.py:864–927,1149–1190` · `app/chamados/services/declaracao_texto.py:87–144,343,643–657` · `app/chamados/services/fabrica_da_celula.py:52–74` · `app/escala/models.py:1177–1219,1663–1690` · `app/docs/GLOSSARIO.md:11` · `app/docs/PLANO-PADROES.md:53–67`

## 28/11 10:47 · sessao 299eff72d329 · - · - · origem=Ronald (tmux)

A sonda pode usar `pausas_de_celulas(colab_id, ini, fim, celulas=None)` sobre a célula carregada por `(colaborador_id, data_evento)`. Esse leitor consulta os dois pares do DNA e não consulta template quando recebe o mapa. O retorno sem a data, com célula confirmada, corresponde a zero pausas declaradas pela lei da R4. `EscalaColaborador.marcos_do_dia(self, data: datetime.date, celulas=None)` retorna quatro posições e pode consultar o template quando o DNA está vazio. [app/escala/utils.py:111](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/escala/utils.py:111) [app/escala/models.py:1109](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/escala/models.py:1109) [app/escala/models.py:1193](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/escala/models.py:1193)

O universo são os motivos `MOTIVOS_DE_INTERVALO`, com `conversa=False`, pelo dia do turno em `data_evento` e pelo vínculo da disputa ao colaborador. O estado vivo é `pergunta_viva(p, hoje=None, *, carga=None, agora=None)`. Para o caso literal, `CelulaDia.marcos_presentes(cls, marcos)` sobre `dna.marcos`, `PerguntaDisputa.estado(self)` e `DecisaoPergunta.composta(cls, p)` mostram marcos, rótulo e carimbos sem expor o texto da resposta. [app/chamados/juizes.py:339](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/juizes.py:339) [app/chamados/juizes.py:911](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/juizes.py:911) [app/chamados/juizes.py:1875](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/juizes.py:1875) [app/escala/models.py:1683](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/escala/models.py:1683) [app/chamados/models.py:3351](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/models.py:3351) [app/chamados/models.py:4507](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/models.py:4507)

O esqueleto completo foi transmitido ao agente principal. A proteção usa transação PostgreSQL `REPEATABLE READ, READ ONLY`, seguindo a sonda CEF. Os resultados de produção e o HEAD permanecem [nao verificado]. [app/chamados/tests/cef_c3_sonda.py:16](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/tests/cef_c3_sonda.py:16)

## 28/11 10:47 · sessao 594be9fb352e · - · - · origem=Ronald (tmux)

**#40588 não tem clique de encerramento aplicável na árvore entregue.** O serviço `DisputaSupervisao.admin_encerra(self, user, motivo)` encerra as perguntas mudas da disputa, com usuário, horário e motivo, e barra resposta sem veredito. A ligação à tela está registrada como pendente em `C7-TELA-ADMIN-ENCERRA`. O **Rejeitar resposta** devolve a pergunta ao colaborador e recusa quando ela já está pendente. **Regularizar disputa e fechar** exige resposta às perguntas vivas. **Resolver dia** fecha o fio mediante decisão de mérito sobre o dia, mas a medição registrada do #40588 diz que o D4 não ofereceu esse botão. [chamados/models.py:2122](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/models.py:2122), [chamados/models.py:2184](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/models.py:2184), [BACKLOG.md:1640](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/BACKLOG.md:1640), [chamados/services/validacao.py:955](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/services/validacao.py:955), [modal_fio.html:363](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/templates/chamados/partials/modal_fio.html:363), [RELATO.md:596](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/RELATO.md:596).

O cartório usa `fio_vivo` nas perguntas que resolvem chamados vivos. Sem outros protestos, um chamado com fio vivo que ainda protesta produz `cobrado`. Marco inexistente na ata resulta em `marcos_acesos=None`, mesmo com todas as lâmpadas que existem acesas. O lastro autoriza a premissa de fechamento, mas a guarda do chamado barra o encerramento se deixar pergunta viva ancorada. Se as duas perguntas forem as últimas desse fio e não houver outro protesto, a próxima relavra resulta em `concorde` e o dia deixa de reter por `espelho_cobrado`. O Retrato precisa da nova lavra. Outras retenções na competência continuam possíveis, e o estado atual no banco está [não verificado]. [ponto/services/cartorio.py:1418](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/ponto/services/cartorio.py:1418), [ponto/services/cartorio.py:1498](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/ponto/services/cartorio.py:1498), [ponto/services/cartorio.py:1544](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/ponto/services/cartorio.py:1544), [ponto/services/lastro.py:170](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/ponto/services/lastro.py:170), [chamados/models.py:1352](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/models.py:1352), [chamados/models.py:429](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/models.py:429), [folha/export.py:1301](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/folha/export.py:1301), [folha/fora_da_folha.py:115](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/folha/fora_da_folha.py:115), [lavrar_retrato_folha.py:65](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/folha/management/commands/lavrar_retrato_folha.py:65).

Texto para o admin: “Abra o Painel de Chamados em `/chamados/painel/?colab_id=549`, clique no chamado **#40588** e confira no fio as cobranças de 11:00 e 12:00, perguntas **#61520** e **#61521**. Confira 20/11 no Espelho em `/ponto/espelho/?colab=549&mes=11&ano=2026`. Encaminhe o chamado com este motivo: ‘A célula **#122468** prevê 07:00–19:00 sem intervalo. As cobranças de 11:00 e 12:00 precisam ser retiradas pela correção CHAMADO-PRESO-CLASSE. Depois da relavra, conferir se 20/11 saiu dos dias que seguram o Retrato.’” [painel_gestao.html:451](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/templates/chamados/painel_gestao.html:451), [_lista_chamados.html:42](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/templates/chamados/partials/_lista_chamados.html:42), [ponto/views.py:98](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/ponto/views.py:98), [RELATO.md:595](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/RELATO.md:595). IDs e horários da célula vêm do enunciado [não verificado].

## 28/11 10:45 · sessao 6fc1c6f8c2b1 · E4C-F5-INSTANTE · leitura · origem=Code

# r.md

Os sete métodos abaixo estão prontos para colar nas classes indicadas. Os três do censo passam na cópia, e os quatro dos selos falham com `[]` na asserção. A prova em memória executou os métodos com `unittest.TestCase` e os analisadores AST da cópia. Suíte Django, Ruff e identidade Git [nao verificado]. `app/chamados/tests/test_e4c_f5_c4_red.py:1551–1591` · `app/core/censo_escritas.py:421–454` · `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165`

Nos diffs, retire o primeiro caractere de cada linha e junte as linhas de cada lado com `\n`, sem acrescentar `\n` ao fim. Aplique cada troca isoladamente com `fonte.replace(original, mutado, 1)`, após conferir `fonte.count(original) == 1`. Os cinco mutantes do censo foram conferidos na cópia. Os dois dos selos foram conferidos sobre a base de contraprova ao fim do item 2. `app/core/censo_escritas.py:430–485` · `app/chamados/tests/test_o261e_f2_porta_carimbo.py:110–112`

## 1 · Variável

Cole em `CensoLeAProjecaoTest`, imediatamente antes de `def test_os_dois_saves_da_porta_ficam_visiveis(self):`. Usa `SimpleTestCase` e a fixture `_varrer` existentes, sem imports novos. **VERDE**. `app/chamados/tests/test_e4c_f5_c4_red.py:21,1551–1562,1576`

```python
    def test_RED_registrar_com_projecao_por_variavel_e_acusado(self):
        fonte = (
            'from chamados.models import DecisaoPergunta as DP\n'
            'def f(p, linhas):\n'
            "    proj = (p, ['respondida_em'])\n"
            '    DP.registrar(linhas, projecao=proj)\n'
        )
        self.assertEqual(self._varrer(fonte), [(4, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]')])
```

**`M1-tupla`** · `app/core/censo_escritas.py:444–447` · âncora **1**. Cai o `assertEqual` porque o achado da linha 4 vira `[]`. `app/core/censo_escritas.py:442–454`

```diff
-        if isinstance(valor, ast.Name):
-            _, atribuicoes = self._escopos_da_projecao[-1]
-            valor = atribuicoes.get(valor.id)
-        if not isinstance(valor, ast.Tuple) or len(valor.elts) != 2:
+        if not isinstance(valor, ast.Tuple) or len(valor.elts) != 2:
```

## 1 · Helper

Cole em `CensoLeAProjecaoTest`, depois do método do item anterior e antes de `def test_os_dois_saves_da_porta_ficam_visiveis(self):`. Usa a mesma fixture, sem imports novos. **VERDE**. `app/chamados/tests/test_e4c_f5_c4_red.py:21,1551–1562,1576` · `app/core/censo_escritas.py:430–440`

```python
    def test_RED_salvar_a_projecao_direto_fora_da_porta_e_acusado(self):
        fonte = (
            'from chamados.models import DecisaoPergunta as DP\n'
            'def registrar(p):\n'
            "    DP._salvar_a_projecao(p, ['respondida_em'])\n"
        )
        self.assertEqual(
            self._varrer(fonte),
            [(3, 'PerguntaDisputa', 'save', 'DP._salvar_a_projecao() [projecao]')],
        )
```

**`M1-ignora-helper`** · `app/core/censo_escritas.py:432` · âncora **1**. Cai o `assertEqual` porque o achado da linha 3 vira `[]`. `app/core/censo_escritas.py:430–433`

```diff
-                self._anotar(no, 'PerguntaDisputa', 'save', '%s() [projecao]' % _texto(f))
+                pass
```

**`M1-nome-registrar`** · `app/core/censo_escritas.py:436–440` · âncora **1**. Cai o mesmo `assertEqual` porque a função avulsa chamada `registrar` ganha a dispensa e o censo devolve `[]`. `app/core/censo_escritas.py:430–440`

```diff
-        if len(self._escopos_da_projecao) != 3:
-            return False
-        _, (classe, _), (metodo, _) = self._escopos_da_projecao
-        return (isinstance(classe, ast.ClassDef) and classe.name == 'DecisaoPergunta'
-                and isinstance(metodo, ast.FunctionDef) and metodo.name == 'registrar')
+        metodo, _ = self._escopos_da_projecao[-1]
+        return isinstance(metodo, ast.FunctionDef) and metodo.name == 'registrar'
```

## 1 · Controle interno

Substitua o método inteiro ancorado em `def test_os_dois_saves_da_porta_ficam_visiveis(self):`, em `CensoLeAProjecaoTest`. O teste lê o `models.py` inteiro com a fixture `_varrer` e confere todos os saves da classe `DecisaoPergunta`, inclusive os atribuídos. Usa `os` do módulo e importa `ast` no método. **VERDE**. `app/chamados/tests/test_e4c_f5_c4_red.py:5,1551–1562,1576–1591` · `app/chamados/models.py:4780–4786,4797–4803,4821–4824`

```python
    def test_os_dois_saves_da_porta_ficam_visiveis(self):
        import ast

        with open(os.path.join(self.APP, 'chamados/models.py'), encoding='utf-8') as arquivo:
            fonte = arquivo.read()
        classe, = (
            no for no in ast.parse(fonte).body
            if isinstance(no, ast.ClassDef) and no.name == 'DecisaoPergunta'
        )
        metodos = [no for no in ast.walk(classe) if isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef))]
        contado = {}
        for linha, entidade, op, expr in self._varrer(fonte):
            if op != 'save' or not classe.lineno <= linha <= classe.end_lineno:
                continue
            dono = min(
                (no for no in metodos if no.lineno <= linha <= no.end_lineno),
                key=lambda no: no.end_lineno - no.lineno,
                default=classe,
            )
            contado.setdefault(dono.name, []).append((entidade, expr))
        self.assertEqual(contado, {
            '_salvar_a_projecao': [(None, 'instancia.save()')],
            'registrar': [(None, 'projecao[0].save()')],
        })
```

**`M1-reclassifica`** · `app/core/censo_escritas.py:484–485` · âncora **1**. Cai `assertEqual(contado, ...)` porque os dois pares passam a trazer `'PerguntaDisputa'` no lugar de `None`. `app/core/censo_escritas.py:475–485`

```diff
-        if not silencioso:
-            self._anotar(no, None, op, '%s.%s()' % (_texto(rec), op))
+        if not silencioso:
+            entidade = 'PerguntaDisputa' if op == 'save' and self._classe == ['DecisaoPergunta'] else None
+            self._anotar(no, entidade, op, '%s.%s()' % (_texto(rec), op))
```

**`M1-terceiro-save`** · `app/core/censo_escritas.py:430–431` · âncora **1**. Cai `assertEqual(contado, ...)` porque `registrar` ganha `('PerguntaDisputa', 'cls._salvar_a_projecao() [projecao]')`. `app/core/censo_escritas.py:430–432` · `app/chamados/models.py:4823–4824`

```diff
-            if (op == '_salvar_a_projecao' and (dono or classe_de_cls) == 'DecisaoPergunta'
-                    and not self._no_registrar_da_decisao()):
+            if op == '_salvar_a_projecao' and (dono or classe_de_cls) == 'DecisaoPergunta':
```

## 2 · `dict` religado

**Carimbo.** Cole em `SeloEscritorUnicoDoCarimbo`, antes de `def test_MORDE_dict_em_variavel_defaults_construtor_e_fields_em_variavel(self):`. Usa `SimpleTestCase` e `escritas_do_carimbo` do módulo, sem imports novos. **VERMELHO**. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:30,110–112,156,222–225,263,333`

```python
    def test_RED_trilha_acusa_dict_religado(self):
        fonte = (
            'from core.observ import evento\n'
            'def f(pk):\n'
            '    dict = PerguntaDisputa.objects.filter(pk=pk).update\n'
            '    evento(depois=dict(materializada_em=None))\n'
            'def g():\n'
            '    evento(depois=dict(materializada_em=None))\n'
        )
        self.assertEqual(escritas_do_carimbo(fonte), [('f', 4, 'dict(materializada_em=)')])
```

**Par.** Cole em `SeloEscritorUnicoDoParMordeTest`, antes de `def test_MORDE_repasse_de_param_na_classe_do_modelo(self):`. Usa `SimpleTestCase` e `escritas_do_par` do módulo, sem imports novos. **VERMELHO**. `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:28–30,144–165,247,269`

```python
    def test_RED_trilha_acusa_dict_religado(self):
        fonte = (
            'from core.observ import evento\n'
            'def f(pk):\n'
            '    dict = PerguntaDisputa.objects.filter(pk=pk).update\n'
            '    evento(depois=dict(respondida_em=None))\n'
            'def g():\n'
            '    evento(depois=dict(respondida_em=None))\n'
        )
        self.assertEqual(escritas_do_par(fonte), [('f', 4, 'dict(respondida_em=)')])
```

**`M2-grafia-dict`** · alvo compartilhado em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:110–112` · âncora **1** na base de contraprova abaixo. Caem os dois `assertEqual` com `[]`. A cópia já dispensa pela grafia de `dict`. O alvo fica em `tests/` e é importado pelo selo do par. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:110–112,156` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30,163`

```diff
-            if isinstance(a, ast.Call) and getattr(a.func, 'id', None) == 'dict'
-            and (escopos[c], 'dict') not in nomes_locais}
+            if isinstance(a, ast.Call) and getattr(a.func, 'id', None) == 'dict'}
```

## 2 · Import homônimo

**Carimbo.** Cole em `SeloEscritorUnicoDoCarimbo`, depois do método anterior e antes de `def test_MORDE_dict_em_variavel_defaults_construtor_e_fields_em_variavel(self):`. Mesmos imports. **VERMELHO**. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:30,85–112,156,263,333`

```python
    def test_RED_trilha_acusa_import_local_homonimo(self):
        fonte = (
            'from core.observ import evento\n'
            'def f():\n'
            '    from core.outro import evento\n'
            '    evento(depois=dict(materializada_em=None))\n'
            'def g():\n'
            '    evento(depois=dict(materializada_em=None))\n'
        )
        self.assertEqual(escritas_do_carimbo(fonte), [('f', 4, 'dict(materializada_em=)')])
```

**Par.** Cole em `SeloEscritorUnicoDoParMordeTest`, depois do método anterior e antes de `def test_MORDE_repasse_de_param_na_classe_do_modelo(self):`. Mesmos imports. **VERMELHO**. `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:28–30,144–165,247,269`

```python
    def test_RED_trilha_acusa_import_local_homonimo(self):
        fonte = (
            'from core.observ import evento as ev\n'
            'def f():\n'
            '    from core.outro import escritor as ev\n'
            '    ev(depois=dict(respondida_em=None))\n'
            'def g():\n'
            '    ev(depois=dict(respondida_em=None))\n'
        )
        self.assertEqual(escritas_do_par(fonte), [('f', 4, 'dict(respondida_em=)')])
```

**`M2-import-global`** · mesmo alvo compartilhado · âncora **1** na base de contraprova. Caem os dois `assertEqual` com `[]` porque o nome local herda a dispensa do conjunto global. A cópia já apresenta esse efeito. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30,163`

```diff
-    return {id(a) for c in ast.walk(arv) if isinstance(c, ast.Call) and _eh_evento(c.func)
-            and (escopos[c], getattr(c.func, 'id', None)) not in imports_alheios
+    return {id(a) for c in ast.walk(arv) if isinstance(c, ast.Call) and _eh_evento(c.func)
```

**Base de contraprova dos dois mutantes.** Aplique este diff em memória em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:110–112`, com âncora **1** na cópia. A base passou nos quatro métodos novos e nos dois controles existentes de trilha legítima. Cada mutante derrubou por asserção apenas os dois métodos do seu item. A base foi verificada nesse recorte. Cobertura geral da resolução de nomes [nao verificado]. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:78–112,309–331` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:289–299`

```diff
-    return {id(a) for c in ast.walk(arv) if isinstance(c, ast.Call) and _eh_evento(c.func)
-            for a in (*c.args, *(k.value for k in c.keywords))
-            if isinstance(a, ast.Call) and getattr(a.func, 'id', None) == 'dict'}
+    escopos = {}
+
+    def _indexar(no, escopo):
+        if isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef, ast.Lambda)):
+            escopo = no
+        escopos[no] = escopo
+        for filho in ast.iter_child_nodes(no):
+            _indexar(filho, escopo)
+
+    _indexar(arv, arv)
+    nomes_locais = {(escopos[n], n.id) for n in escopos if isinstance(n, ast.Name) and isinstance(n.ctx, ast.Store)}
+    imports_alheios = {(escopos[n], a.asname or a.name)
+                       for n in escopos if isinstance(n, ast.ImportFrom) and (n.module != 'core.observ' or n.level)
+                       for a in n.names}
+    return {id(a) for c in ast.walk(arv) if isinstance(c, ast.Call) and _eh_evento(c.func)
+            and (escopos[c], getattr(c.func, 'id', None)) not in imports_alheios
+            for a in (*c.args, *(k.value for k in c.keywords))
+            if isinstance(a, ast.Call) and getattr(a.func, 'id', None) == 'dict'
+            and (escopos[c], 'dict') not in nomes_locais}
```

| Item | Teste | Mutantes | Esperado na cópia | [nao verificado]
|---|---|---|---|
| 1 · Variável | `test_RED_registrar_com_projecao_por_variavel_e_acusado` | `M1-tupla` | **VERDE** · `app/core/censo_escritas.py:442–454` |
| 1 · Helper | `test_RED_salvar_a_projecao_direto_fora_da_porta_e_acusado` | `M1-ignora-helper`, `M1-nome-registrar` | **VERDE** · `app/core/censo_escritas.py:430–440` |
| 1 · Controle interno | `test_os_dois_saves_da_porta_ficam_visiveis` | `M1-reclassifica`, `M1-terceiro-save` | **VERDE** · `app/core/censo_escritas.py:430–440,475–485` |
| 2 · `dict` religado | `test_RED_trilha_acusa_dict_religado` nos dois selos | `M2-grafia-dict` | **VERMELHO** · `app/chamados/tests/test_o261e_f2_porta_carimbo.py:110–112` |
| 2 · Import homônimo | `test_RED_trilha_acusa_import_local_homonimo` nos dois selos | `M2-import-global` | **VERMELHO** · `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` |

**Total de mutantes distintos: 7.** `app/core/censo_escritas.py:430–485` · `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112`

## 28/11 10:37 · sessao f785008300d3 · E4C-F5-INSTANTE · construtor · origem=Code

O drawer do Retrato agora mostra **data, motivo da retenção e link de conferência** para os dias OS6 com destino conhecido. A rota da tarefa continua sendo lida de `porta_da_tarefa`. A oferta de «Resolver dia» continua no calendário. `folha/fora_da_folha.py:70–73,239–248` · `templates/folha/_retrato_o_que_fazer.html:67–76` · `colaboradores/services/calendario.py:94–104`

### Diff e leitores

- [`fora_da_folha.py:125`](/home/ronald/wt-codex-e4c-f5-instante-102848/app/folha/fora_da_folha.py:125) publica `destinos_retencao` com `data`, `motivo`, `tela` e `id`. Para cobrança indevida, usa os IDs que a carga da tarefa associa ao dia com verbo `cobrar`. Para dia indefinido, usa a ausência que a autoridade de cobertura devolve para a data, quando existe, ou o painel do vínculo. `folha/fora_da_folha.py:125–140,239–247,275–278` · `chamados/services/tarefa_do_dia.py:738–751,809–810` · `ponto/turnos.py:3157–3170`
- [`retrato_folha.py:1319`](/home/ronald/wt-codex-e4c-f5-instante-102848/app/folha/retrato_folha.py:1319) copia o campo para a linha lavrada. `portas_da_linha` lê os quatro valores e entrega o link ao drawer do colaborador. O outro chamador, `o_que_segura`, passa a projeção anterior. O PDF continua lendo `motivos_por_dia` pelo leitor existente. `folha/retrato_folha.py:99–118,1117–1126` · `colaboradores/views.py:1004–1017` · `folha/fora_da_folha.py:317–322` · `folha/views_retrato.py:441–445`
- [`_retrato_o_que_fazer.html:73`](/home/ronald/wt-codex-e4c-f5-instante-102848/app/templates/folha/_retrato_o_que_fazer.html:73) exibe o dia e a causa junto ao botão existente. Para col476, o link leva ao vínculo. A obra `RAIZ-DA-DISPUTA-17-SEM-PORTA` está registrada no backlog. Rota de tela para essa obra [nao verificado]. `docs/RELATO.md:14898` · `docs/BACKLOG.md:2048`

### Medida na cópia

Comparei a base `f6ba2bc4e` e o código editado com os mesmos insumos em memória, usando os corpos reais do retentor, da classificação, da montagem do TXT e dos contadores. Nas duas versões, foram **14 dias retidos**, os mesmos pares `status/motivo` em **7 itens**, **45 bytes idênticos de TXT sintético** e os mesmos contadores e pílulas. O total foi de **1 entra, 5 bloqueados, 1 rescisão**. Banco, renderização Django, Retrato pk 10 e certificação pela suíte [nao verificado]. `folha/export.py:525,757,832,1250` · `folha/retrato_folha.py:147,882,1365`

### REDs para o Opus

Cada mutante abaixo é uma substituição literal com **uma ocorrência** na cópia. Execução dos REDs e dos mutantes [nao verificado].

| Caso | O que conferir | Mutante literal |
|---|---|---|
| **col246 · 01/11** | Com jornada 09–15–16–21, batidas 10:04/15:04/16:06/22:08 e chamado residual 36424, conferir `ok`, retenção `espelho_cobranca_indevida`, `rota=None` e link para o painel do chamado **36424**. No cenário de reconciliação, conferir a nova lavra antes de verificar a saída da retenção. `docs/RELATO.md:14896` · `ponto/services/cartorio.py:1618–1629` · `chamados/urls.py:15` | `if verbo == 'cobrar'` → `if verbo == 'decidir'` em `folha/fora_da_folha.py:133`. |
| **col327 · 10/11** | Com atestado 5515 e quatro marcos acesos, conferir retenção `espelho_indefinida`, `rota=None` e painel da ausência **5515** com a edição sujeita à permissão da tela. Após correção válida pela porta de ausência, conferir `concorde/ok` e saída da retenção em uma fixture sem outro protesto. `docs/RELATO.md:14897` · `ponto/views.py:2095–2116,2930` · `ponto/status_dia.py:161–162` | `return [('ausencia', ausencia.pk)]` → `return [('ausencia', colab.pk)]` em `folha/fora_da_folha.py:138`. |
| **col476 · 25/10** | Com batidas só em 24/10 e quatro perguntas vivas fora das pendentes do app, conferir retenção em **25/10**, `rota=None`, oferta de «Resolver dia» fechada e link para o vínculo. `docs/RELATO.md:14898` · `chamados/services/pergunta_no_app.py:80–92` · `colaboradores/services/calendario.py:94–122` | `colab_tem_porta = any(i['pergunta_id'] in pendentes_app and` → `colab_tem_porta = any(i['pergunta_id'] is not None and` em `chamados/services/tarefa_do_dia.py:752`. |
| **col97 · ímpares de 01 a 19/11** | Conferir os **10** dias, cada qual com motivo e link para o vínculo. Após corrigir a fase na fixture, conferir nova lavra por dia de turno, incluindo os pares 02→03 e 18→19, e a retenção resultante por data. `docs/RELATO.md:14899` · `templates/colaboradores/partials/painel_vinculo.html:157` · `folha/export.py:1250–1267` | `for _destino in _destinos:` → `for _destino in _destinos[:1]:` em `folha/retrato_folha.py:1122`. |
| **Pergunta alcançável** | Usar um dia retido por `espelho_indefinida` com pergunta realmente pendente no app. Conferir `porta_app`, fio `colab` e ausência de `destinos_retencao` para o dia. `chamados/services/tarefa_do_dia.py:752–754,777–780` · `folha/fora_da_folha.py:244–247` | `if com_motivos_por_dia and not (_p or {}).get('rota'):` → `if com_motivos_por_dia:` em `folha/fora_da_folha.py:245`. |
| **Desligado** | Usar um dia anterior à saída que apareceria como retenção para um ativo. Conferir a data em `informativo`, sua ausência de `datas` e de `destinos_retencao`, e o item `fora/rescisao_modulo_proprio`. `folha/export.py:928–936,1279–1302` · `folha/retrato_folha.py:1312–1320` | `'informativo': _info.get(_cid, []),` → `'informativo': _c.get('destinos_retencao', []),` em `folha/retrato_folha.py:1315`. |
| **Linha legada** | Abrir uma linha lavrada sem `destinos_retencao`, com `tipo='escala'` e fio `ninguem`. Conferir que o painel do vínculo continua acessível pelo link agregado e que o drawer abre. `folha/retrato_folha.py:1023–1039,1107–1119` · `templates/folha/_retrato_o_que_fazer.html:67–76` | `_destinos = linha.get('destinos_retencao')` → `_destinos = linha['destinos_retencao']` em `folha/retrato_folha.py:1117`. |

## 28/11 10:36 · sessao b60736d02a75 · FDS-FERIAS-COM-BATIDA-PORTA · leitura · origem=Code

## Arquivo `app/folha/tests/test_retrato_portas_ferias_codex.py`

```python
from urllib.parse import parse_qs

from django.test import SimpleTestCase
from django.urls import reverse

from folha.retrato_folha import portas_da_linha


class PortasFeriasComBatidaTest(SimpleTestCase):
    def assert_href_com_consulta(self, href, rota, colab, consulta_esperada):
        caminho, separador, consulta = href.partition('?')
        self.assertEqual(caminho, reverse(rota, args=[colab]))
        self.assertEqual(separador, '?')
        self.assertEqual(
            parse_qs(consulta, keep_blank_values=True, strict_parsing=True),
            consulta_esperada,
        )

    def test_calendario_do_primeiro_dia_gravado_antes_da_secao_de_ferias(self):
        casos = (
            (731, ['2025-12-30', '2026-01-04'], '2025', '12', 'ferias'),
            (842, ['2026-02-07', '2026-01-29'], '2026', '2', 'ferias'),
            (619, ['2024-02-29'], '2024', '2', 'cadastro'),
        )
        for colab, datas, ano, mes, tipo in casos:
            with self.subTest(colab=colab, datas=datas, tipo=tipo):
                linha = {
                    'colab': colab,
                    'status': 'fora',
                    'motivo': 'ferias_com_batida',
                    'tipo': tipo,
                    'dono': 'admin',
                    'acoes': [],
                    'datas': datas.copy(),
                }

                portas = portas_da_linha(linha)

                self.assertEqual(len(portas), 2)
                calendario, ferias = portas
                self.assertTrue(calendario['rotulo'].strip())
                self.assertTrue(ferias['rotulo'].strip())
                self.assert_href_com_consulta(
                    calendario['href'],
                    'colaboradores:calendario_ponto',
                    colab,
                    {
                        'ano': [ano],
                        'mes': [mes],
                        'modo': ['custom'],
                        'data_ini': [datas[0]],
                        'data_fim': [datas[0]],
                    },
                )
                self.assertEqual(
                    ferias['href'],
                    reverse('ferias:secao_ferias_colab', args=[colab]),
                )
                self.assertEqual(linha['datas'], datas)

    def test_sem_datas_oferece_so_a_secao_de_ferias(self):
        colab = 953
        for campos_de_data in ({}, {'datas': None}, {'datas': []}):
            with self.subTest(campos_de_data=campos_de_data):
                portas = portas_da_linha({
                    'colab': colab,
                    'status': 'fora',
                    'motivo': 'ferias_com_batida',
                    'tipo': 'ferias',
                    'dono': 'admin',
                    'acoes': [],
                    **campos_de_data,
                })

                self.assertEqual(
                    [porta['href'] for porta in portas],
                    [reverse('ferias:secao_ferias_colab', args=[colab])],
                )
                self.assertTrue(portas[0]['rotulo'].strip())

    def test_outro_motivo_preserva_a_porta_da_acao_publicada(self):
        colab = 164
        for tipo in ('falta', 'ferias'):
            with self.subTest(tipo=tipo):
                portas = portas_da_linha({
                    'colab': colab,
                    'status': 'fora',
                    'motivo': 'furo_espelho',
                    'tipo': tipo,
                    'dono': 'admin',
                    'datas': ['2025-12-30', '2026-01-04'],
                    'acoes': [{'acao': 'resolver_dia', 'dia_do_turno': '2026-01-04'}],
                })

                self.assertEqual(len(portas), 1)
                self.assertEqual(portas[0]['rotulo'], 'Resolver dia no calendário')
                self.assert_href_com_consulta(
                    portas[0]['href'],
                    'colaboradores:calendario_ponto',
                    colab,
                    {'ano': ['2026'], 'mes': ['1'], 'dia': ['4']},
                )

    def test_outros_motivos_sem_acoes_preservam_as_portas_anteriores(self):
        colab = 275
        fio = reverse('chamados:modal_fio', args=[colab])
        escala = reverse('colaboradores:painel_vinculo', args=[colab])
        cadastro = reverse('colaboradores:painel_colaborador', args=[colab])
        casos = (
            (
                {'motivo': 'furo_espelho', 'tipo': 'escala', 'dono': 'colab',
                 'datas': ['2026-02-07']},
                [
                    {'rotulo': 'Cobrar no fio', 'href': fio},
                    {'rotulo': 'Vínculo da escala', 'href': escala},
                ],
            ),
            (
                {'motivo': 'furo_espelho', 'tipo': 'falta', 'dono': 'ninguem',
                 'datas': ['2026-02-07']},
                [],
            ),
            (
                {'motivo': 'sem_codigo_dominio', 'tipo': 'cadastro', 'dono': 'admin'},
                [
                    {'rotulo': 'Validar no fio', 'href': fio},
                    {'rotulo': 'Cadastro do colaborador', 'href': cadastro},
                ],
            ),
        )
        for campos, esperadas in casos:
            with self.subTest(campos=campos):
                linha = {'colab': colab, 'status': 'fora', 'acoes': [], **campos}
                self.assertEqual(portas_da_linha(linha), esperadas)
```

## Regra fixada por teste [nao verificado]

- `test_calendario_do_primeiro_dia_gravado_antes_da_secao_de_ferias` — “oferece as DUAS portas existentes” e “dias lidos do campo gravado (testemunha le)”. O teste exige calendário do colaborador primeiro, com `ano` e `mes` do primeiro dia gravado e recorte `custom` de um só dia, seguido da seção de férias do mesmo ID. A ordem e o recorte estão explícitos na cópia. `brief_c1.md:8` e `brief_c1.md:9` (anexo), `app/folha/retrato_folha.py:1078–1093`, `app/colaboradores/services/calendario.py:227–235`.
- `test_sem_datas_oferece_so_a_secao_de_ferias` — “CADASTRO = secao Ferias do colaborador” e “dias lidos do campo gravado”. Com `datas` ausente, `None` ou `[]`, a seção de férias é a única porta. `brief_c1.md:8` e `brief_c1.md:9` (anexo), `app/folha/retrato_folha.py:1080–1093`, `app/ferias/urls.py:14`.
- `test_outro_motivo_preserva_a_porta_da_acao_publicada` — “demais motivos inalterados”. A ação `resolver_dia` continua apontando para o dia de turno publicado, inclusive no caso sintético em que o campo `tipo` contém `ferias`. `brief_c1.md:9` (anexo), `app/folha/retrato_folha.py:1053`, `app/folha/retrato_folha.py:1095–1117`.
- `test_outros_motivos_sem_acoes_preservam_as_portas_anteriores` — “demais motivos inalterados”. Os controles preservam cobrança seguida de vínculo da escala, nenhuma porta para falta sem ação disponível, e validação seguida de cadastro para falta de código Domínio. `brief_c1.md:9` (anexo), `app/folha/retrato_folha.py:1023–1045`, `app/folha/retrato_folha.py:1118–1128`.

Usei datas fora de ordem e combinações divergentes de `motivo` e `tipo` como entradas sintéticas para distinguir a primeira data gravada da menor data e o motivo do tipo. O produtor ordena as datas e deriva o tipo das férias com batida. Ocorrência dessas combinações em produção [não verificado]. `app/folha/export.py:498–504`, `app/folha/retrato_folha.py:247–253`, `app/folha/retrato_folha.py:413–424`.

## Veredito esperado

- **VERMELHO sem a cura.** Removendo em memória apenas o bloco `ferias_com_batida`, os seis subcasos de férias falharam e os controles passaram. `app/folha/retrato_folha.py:1078–1093`, `app/folha/retrato_folha.py:1118–1128`.
- **VERDE em `de15021a9`.** Os quatro métodos passaram na cópia fornecida, em memória, com `unittest` e resolução de URL simulada a partir das rotas declaradas. Execução pelo runner Django e confirmação do hash via Git [não verificado]. `app/folha/retrato_folha.py:1058–1128`, `app/config/urls.py:23–32`, `app/colaboradores/urls.py:12`, `app/colaboradores/urls.py:24`, `app/colaboradores/urls.py:48`, `app/ferias/urls.py:14`, `app/chamados/urls.py:39`.

## Mutantes derrubados em memória

- Retirar uma das portas, inverter a ordem, esvaziar o rótulo do calendário, congelar o ID ou apontar direto para a retratação da batida ou para a edição do agendamento. `app/folha/retrato_folha.py:1083–1093`, `app/ponto/urls.py:62`, `app/ferias/urls.py:21`.
- Ler o campo do export, escolher o último ou o menor dia, ordenar a lista gravada, fixar o mês, abrir todo o intervalo, usar o parâmetro `dia` de `resolver_dia` ou omitir o recorte. `app/folha/retrato_folha.py:1079–1088`, `app/folha/retrato_folha.py:1106–1111`, `app/folha/retrato_folha.py:1297–1305`.
- Inventar um dia sem datas, falhar quando a chave está ausente, exigir mais de um dia para oferecer o calendário ou condicionar a seção de férias à presença de datas. `app/folha/retrato_folha.py:1080–1093`.
- Acionar as portas pelo `tipo` ou pela presença de datas, ou deixar o caso de férias cair no fluxo geral e ganhar uma porta extra. `app/folha/retrato_folha.py:1078–1095`, `app/folha/retrato_folha.py:1118–1128`.

## 28/11 10:27 · sessao 3d06f7064149 · E4D-CONTRATO-ROTA-B · leitura · origem=Code

# r.md — E4D-CONTRATO-ROTA-B, cura4

## A — O fio está certo. A catraca precisa seguir a origem de `leitura`

`montar_chamados_fio` é o único chamador de produção que passa `leitura`. Ele calcula `DecisaoPergunta.composta(_p)` e entrega o resultado à função na linha seguinte. `aguarda_o_colaborador` e `cobranca._calados` omitem o argumento e entram no ramo que calcula a composta localmente. A catraca atual apaga os vínculos dos parâmetros ao entrar na função e combina os ramos por interseção. Isso explica o falso vermelho em `fio.py:226`. `app/chamados/services/fio.py:199–226,442–446` · `app/chamados/services/cobranca.py:221–242` · `app/chamados/tests/test_e4c_f5_selo_catraca.py:237–250,321–327`

**Arquivo** `app/chamados/tests/test_e4c_f5_selo_catraca.py`. A catraca passa a provar a origem nos chamadores diretos antes de aceitar o parâmetro na função. As duas verificações usam a fonte que o teste recebeu, inclusive quando ela foi mutada. `app/chamados/tests/test_e4c_f5_selo_catraca.py:332–349` · `app/chamados/tests/e4c_f5_voltaE_mutantes.py:80–91` · `app/core/censo_escritas.py:66–82`

Na âncora `from chamados.tests.e4c_f5_voltaE_mutantes import fonte_mutada`, cole estes imports. `app/chamados/tests/test_e4c_f5_selo_catraca.py:9`

```python
from chamados.tests.e4c_f5_voltaE_mutantes import fonte_mutada
from core.censo_escritas import _e_codigo_de_producao
```

Substitua de `class Catraca(ast.NodeVisitor):` até `        self.vinculos = {}`, inclusive. `app/chamados/tests/test_e4c_f5_selo_catraca.py:68–70`

```python
class Catraca(ast.NodeVisitor):
    def __init__(self, *, parametros_compostos=None):
        self.parametros_compostos = parametros_compostos or {}
        self.vinculos = {}
```

Em `visit_FunctionDef`, substitua o trecho que começa em `        antes = self.vinculos.copy()` imediatamente antes de `        for arg in ast.walk(no.args):` e termina antes de `    visit_AsyncFunctionDef = visit_FunctionDef`. `app/chamados/tests/test_e4c_f5_selo_catraca.py:321–329`

```python
        antes = self.vinculos.copy()
        compostos = self.parametros_compostos.get(no, ())
        for arg in ast.walk(no.args):
            if isinstance(arg, ast.arg):
                self.vinculos.pop(arg.arg, None)
                if arg.arg in compostos:
                    self.vinculos[arg.arg] = VAZIO._replace(composta=True)
        for passo in no.body:
            self.visit(passo)
        self.vinculos = antes
```

Substitua de `def _vermelhos(funcao):` até antes de `    def _julgar(self, corpo):`. `app/chamados/tests/test_e4c_f5_selo_catraca.py:332–351`

```python
ALVO_FIO = 'espera_o_texto_do_colaborador'


class CatracaChamadasDoFio(Catraca):
    def __init__(self):
        super().__init__()
        self.chamadas_cruas = set()

    def _composta(self, no):
        if isinstance(no, ast.Name):
            return super()._composta(no)
        return (
            _metodo(no) == 'composta'
            and isinstance(no.func.value, ast.Name)
            and no.func.value.id == 'DecisaoPergunta'
        )

    def visit_Call(self, no):
        chamada_do_fio = (
            isinstance(no.func, ast.Name) and no.func.id == ALVO_FIO
        ) or _metodo(no) == ALVO_FIO
        if chamada_do_fio:
            argumentos_opacos = (
                len(no.args) > 1
                or any(isinstance(arg, ast.Starred) for arg in no.args)
                or any(kw.arg is None for kw in no.keywords)
            )
            leitura_crua = any(
                kw.arg == 'leitura'
                and not (isinstance(kw.value, ast.Constant) and kw.value.value is None)
                and not self._composta(kw.value)
                for kw in no.keywords
            )
            if argumentos_opacos or leitura_crua:
                self.chamadas_cruas.add(no.lineno)
        super().visit_Call(no)


def _vermelhos_das_chamadas_do_fio():
    vermelhos = []
    for caminho in sorted(RAIZ.rglob('*.py')):
        arquivo = caminho.relative_to(RAIZ).as_posix()
        if not _e_codigo_de_producao(arquivo):
            continue
        texto = fonte_mutada(arquivo, caminho.read_text(encoding='utf-8'))
        if ALVO_FIO in texto:
            catraca = CatracaChamadasDoFio()
            catraca.visit(ast.parse(texto, filename=arquivo))
            vermelhos.extend(f'{arquivo}:{linha} {ALVO_FIO} (leitura)'
                             for linha in sorted(catraca.chamadas_cruas))
    return vermelhos


def _vermelhos(funcao, *, parametros_compostos=()):
    catraca = Catraca(parametros_compostos={funcao: parametros_compostos})
    catraca.visit(funcao)
    return catraca.vermelhos


class SeloCatracaTest(SimpleTestCase):
    def test_producao_sem_leituras_cruas(self):
        self.assertEqual(_vermelhos_das_chamadas_do_fio(), [])
        vermelhos = []
        for arquivo, nomes in FUNCOES.items():
            fonte = ast.parse(fonte_mutada(arquivo, (RAIZ / arquivo).read_text(encoding='utf-8')),
                              filename=arquivo)
            for nome in nomes:
                locais = [no for no in fonte.body if isinstance(no, ast.FunctionDef) and no.name == nome]
                self.assertEqual(len(locais), 1, (arquivo, nome))
                compostos = ('leitura',) if (arquivo, nome) == ('chamados/services/fio.py', ALVO_FIO) else ()
                vermelhos.extend(
                    f'{arquivo}:{linha} {nome} ({tipo})'
                    for linha, tipo in sorted(_vermelhos(locais[0], parametros_compostos=compostos))
                )
        self.assertEqual(vermelhos, [])

```

**Mutantes A.** Cada tupla traz `id`, arquivo de produção, original e mutado. Aplique cada tupla independentemente. A1 cai em `self.assertEqual(_vermelhos_das_chamadas_do_fio(), [])` na chamada da linha 445. A2 e A3 caem em `self.assertEqual(vermelhos, [])` na leitura de `respondida_em`, respectivamente na linha 228 da fonte mutada e na linha 226. Rodei os nove métodos do selo em memória com `unittest.TestCase`. A fonte limpa passou e os três mutantes falharam nas assertivas indicadas. `app/chamados/services/fio.py:224–226,444–446` · `app/chamados/tests/test_e4c_f5_selo_catraca.py:338–447`

```python
[
    (
        'A1', 'app/chamados/services/fio.py',
        'espera_o_texto_do_colaborador(_p, leitura=leitura)',
        'espera_o_texto_do_colaborador(_p, leitura=_p)',
    ),
    (
        'A2', 'app/chamados/services/fio.py',
        '    if leitura is None:\n        leitura = DecisaoPergunta.composta(p)',
        '    if leitura is None:\n        leitura = DecisaoPergunta.composta(p)\n'
        '    else:\n        leitura = p',
    ),
    (
        'A3', 'app/chamados/services/fio.py',
        '    if leitura is None:\n        leitura = DecisaoPergunta.composta(p)',
        '    if leitura is None:\n        leitura = p',
    ),
]
```

## B — Caso 20 testa a prevalência da recusa humana em Q

O teste atual exige `pedido_id` nas assinaturas do núcleo de resposta e da reabertura, e exige que dois identificadores distintos fiquem nas linhas `ABSORCAO` da mesma ausência. O recibo HTTP de sucesso devolve `status`, `pergunta_id` e `respondida_em`. `pergunta_id` é o PK da pergunta e `respondida_em` vem do ato de resposta gravado pelo pedido. `app/chamados/tests/test_e4d_matriz_reds.py:1451–1478` · `app/api/views.py:2653–2666` · `app/chamados/services/disputa_emissao.py:1564–1571,1618–1631`

A regra decidida é “A recusa humana prevalece em Q”. O grupo 6 deixa em aberto a extensão da interrupção às irmãs e a identidade da passagem. O teste abaixo intercala uma recusa real depois de a passagem selecionar Q e antes de relê-la sob a trava. Verifica a recusa de Q, a autoria e a projeção pendente. A fixture dá a Q um marco apagado e uma célula que acusa. O cenário vizinho de `:1440` exige também um desfecho para as irmãs e continua fora do veredito deste teste. `logs/E4D-MATRIZ/r2.txt`, grupo 6, caso 20, conforme anexo, linha original [nao verificado] · `app/docs/PROMPTS.md:2873` · `app/chamados/tests/test_e4d_matriz_reds.py:1356–1379,1440–1449,1480–1514` · `app/chamados/services/materializacao.py:1047–1058,1067–1109` · `app/chamados/juizes.py:1544–1555`

**Arquivo** `app/chamados/tests/test_e4d_matriz_reds.py`. Substitua de `    def test_caso_20_pedidos_distintos_na_mesma_ausencia(self):` até antes de `    def verificar_recusa_na_passagem(self, instante):`. `app/chamados/tests/test_e4d_matriz_reds.py:1451–1480`

```python
    def test_caso_20_recusa_humana_prevalece_em_q(self):
        e = self.escape()
        celula = self.celula(
            self.DIA_PASSAGEM, trabalha=True, origem='gerada', dna={'marcos': {'hf': '19:00'}},
        )
        celula.lavrar_veredito(
            via='cartorio', veredito='furo', agora=T0,
            ata={'lampadas': [{'tipo': 'S', 'hora': '19:00', 'acesa': False}]},
        )
        q = self.respondida(disputa=self.raiz, dia=self.DIA_PASSAGEM, celula=celula)
        motivo = 'Confirme a hora de saída'

        def recusar_q():
            resultado = validacao.recusar_pergunta(q.pk, self.admin, motivo)
            self.assertEqual((resultado['ok'], resultado['estado']), (True, 'recusada'))

        recusadas = self.passagem(e, recusar_q)
        self.atestado()
        self.assertEqual(recusadas, [q.pk])
        self.assertEqual(self.atos(q), [DP.RESPOSTA, DP.RECUSA])
        self.assertEqual(self.foto(q), VAZIA)
        recusa = q.decisoes.get(ato=DP.RECUSA)
        self.assertEqual((recusa.ato_em, recusa.ato_por_id, recusa.ato_motivo), (T1, self.admin.pk, motivo))

```

**Mutante B1.** O original ocorre uma vez. Ao aceitar o carimbo de ausência apesar do C6, a passagem inclui Q na absorção. A primeira assertiva que deve cair é `self.assertEqual(recusadas, [q.pk])`. Execução ORM [nao verificado]. `app/chamados/models.py:3474–3501` · `app/chamados/services/materializacao.py:1106–1140`

```python
[
    (
        'B1', 'app/chamados/models.py',
        'viva = pergunta_viva(self)',
        'viva = pergunta_viva(self) and via != self.VIA_AUSENCIA_DIA',
    ),
]
```

**Contraprova já registrada na matriz.** O seletor antigo chama a porta com `pedido_id` e referencia o nome do teste substituído. Em `app/chamados/tests/e4d_matriz_mutantes.py`, substitua de `    if nome == '20_reusa_pedido_da_ausencia':` até antes de `    if nome in {'25_absorcao_so_por_coluna', '26_devolve_sem_vinculo'}:`. `app/chamados/tests/e4d_matriz_mutantes.py:394–405`

```python
    if nome == '20_absorve_q_apesar_da_recusa':
        return trocar_codigo(P.carimbar, (
            ('viva = pergunta_viva(self)',
             'viva = pergunta_viva(self) and via != self.VIA_AUSENCIA_DIA'),
        ))

```

No mesmo arquivo, substitua a declaração de `M20ReusaPedidoDaAusencia = a(`, até antes de `M26DevolveSemVinculo = a(`. `app/chamados/tests/e4d_matriz_mutantes.py:665–670`

```python
M20RecusaPrevaleceEmQ = a('M20RecusaPrevaleceEmQ', '20_absorve_q_apesar_da_recusa',
                         'test_caso_20_recusa_humana_prevalece_em_q')
```

## C1 — Cartão renderizado pelo GET e pelo HTMX

O GET filtra a lista pelo C6. O POST HTMX de recusa sem motivo usa a porta real, devolve o parcial pelo PK e preserva a fixture. Isso permite conferir pelo HTMX a hora de validação quando o ato ainda está vigente. Depois da `DEVOLUCAO`, as duas vias têm de mostrar o cartão com a hora da resposta vigente, sem opacidade nem selo de validação, e com as ações disponíveis. `app/chamados/services/acoes_chamado.py:337–358` · `app/templates/chamados/detalhe_local.html:295–311` · `app/chamados/views.py:394–408,506–519,550–552` · `app/chamados/services/acoes_disputa.py:94–102` · `app/chamados/services/validacao.py:934–937` · `app/templates/chamados/partials/_pergunta_card.html:20–44,118–127`

**Arquivo** `app/chamados/tests/test_e4d_matriz_reds.py`. Insira antes de `    def test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta(self):`, dentro de `MatrizRaizELeitoresTest`. `app/chamados/tests/test_e4d_matriz_reds.py:1616–1617`

```python
    def test_cura4_cartao_exibe_composta_no_get_e_no_htmx(self):
        from django.urls import reverse

        from chamados.tests._ato_da_fixture import registrar_na_fixture

        gestora = User.objects.create_superuser(username='gestora_cura4_cartao')
        self.client.force_login(gestora)
        chamado = self.pai_de_disputa()
        disputa = self.disputa(chamado=chamado)
        resposta_em = T0 + timedelta(minutes=15)
        validacao_residual = T0 + timedelta(minutes=30)
        validacao_vigente = T0 + timedelta(minutes=35)
        devolucao_em = T0 + timedelta(minutes=45)
        texto = 'Conferi o registro'
        p = self.cartao(
            disputa=disputa, motivo='foto_ausente', tipo='texto',
            respondida_em=T0, resposta_colab=texto,
            validada_em=validacao_residual, validada_por_id=gestora.pk,
        )
        registrar_na_fixture([
            dict(pergunta=p, ato=DP.RESPOSTA, ato_em=T0, ato_texto=texto),
            dict(pergunta=p, ato=DP.RESPOSTA, ato_em=resposta_em, ato_texto=texto),
            dict(pergunta=p, ato=DP.VALIDACAO, ato_em=validacao_residual, ato_por=gestora),
            dict(pergunta=p, ato=DP.VALIDACAO, ato_em=validacao_vigente, ato_por=gestora),
        ])
        url = reverse('chamados:detalhe_local', args=[chamado.pk])
        template = 'chamados/partials/_pergunta_card.html'
        raiz = f'<div id="pergunta-{p.pk}"'

        def htmx():
            retorno = self.client.post(
                url, {'acao': 'recusar_pergunta', 'pergunta_id': p.pk},
                HTTP_HX_REQUEST='true',
            )
            self.assertEqual(retorno.status_code, 200)
            self.assertTemplateUsed(retorno, template)
            self.assertTrue(retorno.content.decode().lstrip().startswith(raiz))
            self.assertEqual(json.loads(retorno.headers['HX-Trigger'])['showToast']['tipo'], 'erro')
            return retorno

        with timezone.override('UTC'):
            selo = htmx()
            self.assertContains(selo, f'✓ validada {validacao_vigente:%d/%m %H:%M}</span>')
            registrar_na_fixture([
                dict(pergunta=p, ato=DP.DEVOLUCAO, ato_em=devolucao_em, ato_por=gestora),
            ])
            for via, renderizar in (('GET', lambda: self.client.get(url)), ('HTMX', htmx)):
                with self.subTest(via=via):
                    retorno = renderizar()
                    self.assertTemplateUsed(retorno, template)
                    self.assertContains(retorno, raiz, count=1)
                    html = retorno.content.decode()
                    abertura = html.split(raiz, 1)[1].split('>', 1)[0]
                    self.assertNotIn('opacity:.55;', abertura)
                    self.assertNotContains(retorno, '✓ validada ')
                    self.assertContains(retorno, '⚠ aguardando validacao')
                    self.assertContains(retorno, f'— {resposta_em:%d/%m %H:%M}</span>')
                    self.assertContains(retorno, 'name="acao" value="recusar_pergunta"')

        p.refresh_from_db()
        self.assertEqual((p.respondida_em, p.validada_em), (T0, validacao_residual))
        self.assertEqual(list(p.decisoes.order_by('pk').values_list('ato', flat=True)),
                         [DP.RESPOSTA, DP.RESPOSTA, DP.VALIDACAO, DP.VALIDACAO, DP.DEVOLUCAO])

```

**Mutantes C1.** Cada original ocorre uma vez. C1.1 deve cair na ausência de `opacity:.55;`. C1.2 deve cair na ausência de `✓ validada ` após a devolução. C1.3 deve cair na hora do selo vigente, no controle HTMX anterior à devolução. C1.4 deve cair na hora exibida da resposta. C1.5 deve cair na presença do botão `recusar_pergunta`. Execução Django [nao verificado]. `app/templates/chamados/partials/_pergunta_card.html:21–33,43–44,118–127` · `app/chamados/models.py:4486–4515`

```python
[
    (
        'C1.1', 'app/templates/chamados/partials/_pergunta_card.html',
        '{% if resposta.validada_em %}opacity:.55;',
        '{% if perg.validada_em %}opacity:.55;',
    ),
    (
        'C1.2', 'app/templates/chamados/partials/_pergunta_card.html',
        '{% if resposta.validada_em %}<span style="color:#16a34a;',
        '{% if perg.validada_em %}<span style="color:#16a34a;',
    ),
    (
        'C1.3', 'app/templates/chamados/partials/_pergunta_card.html',
        '{{ resposta.validada_em|date:"d/m H:i" }}',
        '{{ perg.validada_em|date:"d/m H:i" }}',
    ),
    (
        'C1.4', 'app/templates/chamados/partials/_pergunta_card.html',
        '{{ resposta.respondida_em|date:"d/m H:i" }}',
        '{{ perg.respondida_em|date:"d/m H:i" }}',
    ),
    (
        'C1.5', 'app/templates/chamados/partials/_pergunta_card.html',
        '{% if not resposta.validada_em and pode_agir %}',
        '{% if not perg.validada_em and pode_agir %}',
    ),
]
```

## C3 — Recibo do ato deste POST

O teste envolve o registrador real para agendar uma segunda resposta no `on_commit` da primeira. O callback confere que saiu do bloco atômico e usa a porta real para gravar em `T2`. A resposta legada entra no mesmo lote que promove a projeção e grava o pedido em `T1`. Assim o teste distingue o primeiro ato do lote (`T0`), o ato do pedido (`T1`) e a resposta vigente quando a API monta o recibo (`T2`). `app/chamados/models.py:4429–4447,4590–4623` · `app/chamados/services/disputa_emissao.py:1485–1503,1523–1534,1564–1571,1618–1628` · `app/api/views.py:2653–2666`

**Arquivo** `app/chamados/tests/test_e4d_matriz_reds.py`. Substitua a âncora `from django.test import RequestFactory, SimpleTestCase` pelo import abaixo. Depois insira a classe antes de `class MatrizRaizELeitoresTest(ApoioMatriz):`. `app/chamados/tests/test_e4d_matriz_reds.py:29,1616`

```python
from django.test import RequestFactory, SimpleTestCase, TransactionTestCase
```

```python
class ReciboCura4Test(TransactionTestCase):
    def test_cura4_recibo_do_post_preserva_o_ato_deste_pedido(self):
        from rest_framework.test import APIRequestFactory, force_authenticate

        from api.views import api_disputa_responder
        from chamados.tests.cortes_f2_f9_apoio import criar_empresa_e_colaborador

        with freeze_time(T1), calado():
            _, colab = criar_empresa_e_colaborador()
            titular = User.objects.create_user(username='titular_cura4_recibo')
            colab.usuario = titular
            colab.save(update_fields=['usuario'])
            disputa = DisputaSupervisao.objects.create(colaborador=colab)
            p = P.objects.create(
                disputa=disputa, motivo='foto_ausente', pergunta='Confirme o registro',
                tipo_resposta='texto', data_evento=DIA, conversa=True,
                resposta_colab='Resposta legada', respondida_em=T0,
            )
            texto_pedido = 'Resposta deste pedido'
            texto_posterior = 'Resposta de outro pedido'
            registrar = DP.registrar

            def gravar_resposta_posterior():
                self.assertFalse(connection.in_atomic_block)
                with freeze_time(T2):
                    atual = P.objects.get(pk=p.pk)
                    resultado = disputa_emissao.responder_pergunta_disputa_core_da_entrada(
                        atual, texto_posterior, titular, True, exige_viva=True,
                    )
                self.assertEqual(resultado, (True, None))

            def registrar_e_agendar(linhas):
                atos = registrar(linhas)
                if any(ato.pergunta_id == p.pk and ato.ato == DP.RESPOSTA
                       and ato.ato_texto == texto_pedido for ato in atos):
                    self.assertTrue(connection.in_atomic_block)
                    transaction.on_commit(gravar_resposta_posterior)
                return atos

            request = APIRequestFactory().post(
                f'/api/disputa/responder/{p.pk}/', {'resposta': texto_pedido}, format='json',
            )
            force_authenticate(request, user=titular)
            with patch.object(DP, 'registrar', new=staticmethod(regist

[truncado em 20000 de 23414 caracteres]

## 28/11 10:25 · sessao 7c32debc918d · FDS-FERIAS-COM-BATIDA-PORTA · leitura · origem=Code

## 1. Onde a linha nasce

- **Motivo.** `classificar_export` chama `dias_ferias_com_batida` e, quando recebe dias para o colaborador, carimba `status='fora'`, `motivo='ferias_com_batida'` e `dias_ferias_com_batida`. A lista contém datas civis locais distintas, ordenadas. `app/folha/export.py:491–505`, `:801`, `:892–900`.
- **`datas` e `dias`.** `montar_linhas` recebe o item por `fora_da_folha(com_itens=True)`, copia `dias_ferias_com_batida` para `datas` em ISO e grava `dias=len(datas)`. O comando `lavrar_retrato_folha --apply` persiste essas linhas. `app/folha/fora_da_folha.py:256–262`, `app/folha/retrato_folha.py:1274–1305`, `app/folha/management/commands/lavrar_retrato_folha.py:65–79`.
- **`acoes=[]`.** Quem atribui é `montar_linhas`, com `_c.get('acoes') or []`. `_c` fica vazio para esta linha porque `fora_da_folha` só carrega tarefas e monta `colabs` para os retidos por `furo_espelho`. `app/folha/fora_da_folha.py:150–165`, `:203–236`, `:242–261`, `app/folha/retrato_folha.py:1284–1289`, `:1310–1313`.

## 2. Quem tem a batida e quem dá o dia do turno

**A batida concreta está em mãos no laço de `dias_ferias_com_batida`.** Ali `b` vem de `batidas_apuraveis`, e o teste usa `localdate(b.timestamp)` contra as faixas de férias. O laço devolve somente o conjunto de datas civis. Quando `classificar_export` escreve o motivo e quando `montar_linhas` escreve a linha, ambos recebem só essas datas. O caminho de férias passa fora da carga de tarefas. `app/folha/export.py:491–505`, `:892–900`, `app/folha/fora_da_folha.py:203–213`, `app/folha/retrato_folha.py:1284–1311`.

**A autoridade é `ponto/turnos.py`.** O pareador atribui `Turno.data_turno` por `_data_do_turno`. `dia_das_batidas` projeta esse valor por PK, e `dia_do_turno_da_batida` é o acesso unitário. Para batida que nenhum turno reclama, a projeção devolve o dia civil. O Calendário `modo=custom` lê o intervalo pedido, agrupa os turnos da mesma autoridade por `t.data_turno` e desenha as marcações de `t.batidas`. Assim, o dia devolvido pela projeção e a presença da marcação na grade são verificações distintas. `app/ponto/turnos.py:285–324`, `:2971–2978`, `:3014–3025`, `:3034–3045`, `:3112–3140`, `app/colaboradores/services/calendario.py:227–253`, `:641–654`, `app/templates/colaboradores/partials/_calendario_grade.html:124–134`.

**`resolver_dia` já recebe um dia de turno como entrada.** A tarefa consulta a oferta do Calendário para esse dia, copia o dia para a porta publicada e o Retrato lê `dia_do_turno` da ação. A oferta de `resolver_dia` tem predicado próprio na tela. `app/chamados/services/tarefa_do_dia.py:214–215`, `:702–717`, `:781–790`, `:809–822`, `:830–840`, `app/colaboradores/services/calendario.py:97–104`, `app/folha/retrato_folha.py:1106–1111`.

## 3. Dá para publicar sem alterar `export.py`?

**Sim, com uma releitura das batidas. Só com o item atual, o dia do turno é irrecuperável.** Em `montar_linhas`, dá para usar as datas civis já certificadas pelo Export, reler `batidas_apuraveis` nesses dias e consultar `dia_das_batidas` por PK. Isso reaproveita o veredito de férias e o pareador. Para publicar uma porta que exiba a marcação, a evidência é o PK em `t.batidas` no dia indicado. `app/folha/export.py:498–505`, `:892–900`, `app/folha/retrato_folha.py:1284–1311`, `app/ponto/turnos.py:2941–2968`, `:3034–3045`, `app/colaboradores/services/calendario.py:641–654`.

**A fonte do conflito é o laço em `export.py:499–502`. A fonte do dia do turno é `turnos.py:318` e `:3042`.** Para transportar a evidência **da mesma passagem** que decidiu a retenção, o ponto é o laço do Export, antes de descartar o PK. O Retrato é o ponto de publicação da linha. O c3 fornecido só consome `acoes[].dia_do_turno` de `resolver_dia`. Com o `acoes=[]` produzido hoje, seu ramo oferece apenas Férias. `app/folha/export.py:498–505`, `app/ponto/turnos.py:312–324`, `:3034–3045`, `app/folha/retrato_folha.py:1299–1313`, anexo `c3.diff`, `retrato_folha.py:1049–1069` na versão proposta.

## 4. Quem lê e o que pode mudar

| Consumidor | Efeito |
|---|---|
| **Drawer** | Mostra `datas` literalmente. Na cópia, o ramo de férias abre o Calendário pela primeira data civil e acrescenta Férias, retornando antes da leitura genérica de `acoes`. “O que destrava” vem do fio gravado, que é `admin` para esse motivo. O outro chamador de `portas_da_linha`, `o_que_segura`, passa uma linha própria com `tipo='falta'`. `app/colaboradores/views.py:1004–1007`, `app/templates/folha/_retrato_o_que_fazer.html:43–47`, `app/folha/retrato_folha.py:406–410`, `:1078–1098`, `:1183–1184`, `app/folha/fora_da_folha.py:293–298`. |
| **Contadores e PDF do Retrato** | Publicar apenas o dado de navegação preserva as contagens. Trocar `datas` pode deslocar dias para dentro ou para fora do recorte, mudando `dias`, a faixa de saldo e os números das pílulas. O PDF usa o mesmo recorte e imprime `dias`. O total por situação conta linhas pelo balde de `status` e `motivo`. `app/folha/retrato_folha.py:122–154`, `:622–623`, `:872–876`, `:1403–1415`, `app/folha/views_retrato.py:230–232`, `:394–396`, `:450–457`. |
| **Painel do Export, PDF de Retidos e TXT** | Painel e PDF contam `dias_ferias_com_batida` no item do Export. O TXT seleciona os itens com `status='entra'`, então acrescentar o dia do turno como dado de navegação preserva o TXT. Mudar o cruzamento civil que decide `status` mexeria no critério de entrada. `app/folha/views.py:425–430`, `:884–886`, `app/folha/retidos_pdf.py:88–95`, `app/folha/export.py:723–736`, `:892–900`. |

## 5. Chamado 37604 e col976

- **Chamado 37604, J.A., competência 11, 07/11 — cruzamento da meia-noite [nao verificado].** O fixture de 07/11 injeta a data civil no retorno de `classificar_export`. O teste de porta também injeta a data na linha. Ambos exercitam a publicação ou o link, sem montar um par desse chamado. `app/folha/tests/test_retrato_por_que_por_dia.py:345–356`, `app/folha/tests/test_retrato_portas_ferias_com_batida.py:60–65`.
- **col976, 07–20/11 — cruzamento da meia-noite [nao verificado].** O dado fornecido não traz entrada, saída nem associação por PK. O fixture que prova a regra geral usa outro caso, com entrada dia 4 às 19h e saída dia 5 às 7h, ambas atribuídas ao dia 4. `app/ponto/tests/test_continuacao_turno_aberto.py:35–41`.

**Sonda exata, só leitura.** No tenant dos casos, resolver o colaborador do 37604 pelo identificador do chamado (`pk` se local, `ticket_id_glpi` se GLPI). Para cada colaborador, obter `dias_ferias_com_batida(colab.empresa, 11, 2026, [colab])` e recortar aos dias pedidos. Para cada dia civil `D` retornado, ler `batidas_apuraveis` do primeiro ao último instante local de `D`. Obter `turnos_do_colab(colab, D−1, D+1)`, passar esses turnos a `dia_das_batidas` e casar cada batida pelo PK em `t.batidas`. Emitir por batida a data civil, o `data_turno`, os timestamps locais de `entrada` e `saida` e `t.cross_meianoite`. Se faltar uma ponta, registrar o par incompleto. O booleano do código exige ambas as pontas e compara a data local da saída com `data_turno`. `app/chamados/models.py:50–51`, `app/chamados/views.py:359–363`, `app/folha/export.py:486–505`, `app/ponto/turnos.py:312–324`, `:1834–1852`, `:2941–2968`, `:2996–3004`, `:3034–3045`.

O comando existente `listar_ferias_com_batida` usa `Ausencia(tipo='ferias')` e marca `plantao_da_vespera` quando **todas** as batidas do dia são atribuídas à véspera. A sonda acima mede o universo do Retrato, que vem de agendamentos, e verifica o par de cada batida. `app/ferias/management/commands/listar_ferias_com_batida.py:4–11`, `:41–65`, `app/folha/export.py:484–505`.

## 28/11 10:20 · sessao 1cfdeddb2690 · E4C-F5-INSTANTE · construtor · origem=Code

A cura ficou em `app/core/censo_escritas.py`. O censo resolve `projecao=proj` quando `proj` recebeu uma tupla de dois elementos por atribuição simples anterior no mesmo escopo. O achado usa a linha da chamada e a mesma inferência da tupla literal. `DP._salvar_a_projecao(...)` gera um `save` de `PerguntaDisputa` fora de `DecisaoPergunta.registrar`, inclusive em função avulsa chamada `registrar`. `DP` é resolvido pela tabela de símbolos existente. `app/core/censo_escritas.py:221`, `app/core/censo_escritas.py:301`, `app/core/censo_escritas.py:422`, `app/core/censo_escritas.py:435`, `app/core/censo_escritas.py:442`

**Leitores e medição**

- O comando `censo_escritas` usa `entidade` e `arquivo` para filtrar e `expr` para exibir. A `tabela` também lê `op` para contar `delete`. Na cópia, o total ficou em **543 → 543**, com **137 → 137** sem entidade e **68 → 68** na família `chamado`. A comparação dos achados completos deu zero adições e zero remoções. `app/core/management/commands/censo_escritas.py:23`, `app/core/censo_escritas.py:525`, `app/core/censo_escritas.py:561`
- O gate de chamados filtra por `entidade` e `arquivo` e usa `expr` no diagnóstico. A contagem de sítios fora da porta ficou em **0 → 0**. Os dois `save()` físicos da porta continuaram visíveis com `entidade=None`. `app/chamados/tests/test_chokepoint_chamado_gate.py:316`, `app/chamados/tests/test_chokepoint_chamado_gate.py:349`, `app/chamados/models.py:4784`, `app/chamados/models.py:4802`

**REDs e mutantes para o Opus**

| Caso | Asserção | Mutante |
|---|---|---|
| Projeção por variável com `as DP` | Exatamente um `(4, 'PerguntaDisputa', 'save')` na chamada. Manter os controles da tupla literal e do `registrar` sem projeção. | Voltar a exigir `ast.Tuple` diretamente no argumento. A lista fica vazia. `app/core/censo_escritas.py:427`, `app/core/censo_escritas.py:442`, `app/chamados/tests/test_e4c_f5_c4_red.py:1564` |
| Helper direto em função avulsa `registrar` | Exatamente um `(3, 'PerguntaDisputa', 'save')` para `DP._salvar_a_projecao(p, ['respondida_em'])`. | Omitir o reconhecimento do helper ou dispensar pelo nome `registrar` sozinho. Em ambos os casos, a lista fica vazia. `app/core/censo_escritas.py:430`, `app/core/censo_escritas.py:435` |
| Controle dos dois saves da porta | Conferir todos os achados `save` da classe, com ou sem entidade. Exigir exatamente `instancia.save()` e `projecao[0].save()`, ambos com `entidade=None`. | Reclassificar um dos saves físicos ou contar a delegação interna como terceiro `save`. A igualdade da lista falha. `app/chamados/models.py:4784`, `app/chamados/models.py:4802`, `app/chamados/models.py:4824` |

**Script para o Code**

Execute a partir de `app/`. O script compara o censo da base com o da cópia atual e aplica o filtro do gate, cuja allowlist é vazia. `app/core/censo_escritas.py:525`, `app/chamados/tests/test_chokepoint_chamado_gate.py:56`, `app/chamados/tests/test_chokepoint_chamado_gate.py:316`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from collections import Counter
from subprocess import check_output
from core import censo_escritas as atual, portas

caminho = 'app/core/censo_escritas.py'
ref = '63680b06e97b766895151a423f771215528cd721'
base = {}
fonte = check_output(['git', 'show', f'{ref}:{caminho}'], text=True)
exec(compile(fonte, caminho, 'exec'), base)

def medir(rotulo, varrer, tabela):
    achados = varrer('.')
    familias, _, numeros = tabela(achados)
    gate = Counter(x['entidade'] for x in achados
                   if x['entidade'] and portas.familia_de(x['entidade']) == 'chamado'
                   and x['arquivo'] not in (portas.porta_de(x['entidade']) or ()))
    campos = ('arquivo', 'linha', 'entidade', 'op', 'expr')
    print(rotulo, len(achados), numeros, familias['chamado']['sitios'], dict(gate))
    return Counter(tuple(x[k] for k in campos) for x in achados)

antes = medir('antes', base['varrer_arvore'], base['tabela'])
depois = medir('depois', atual.varrer_arvore, atual.tabela)
print('removidos', antes - depois, 'adicionados', depois - antes)
PY
```

Suite, execução dos mutantes e produção [nao verificado]. [nao verificado]

_8904 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
