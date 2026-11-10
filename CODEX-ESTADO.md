# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 22:58** · publicado em **09/11/2026 23:01** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 22:58 · sessao b8554cce33ca · FIO-E0-V4-E4A-2 · construtor · origem=Code

Alterado somente [test_e4a2_disparar_e_vigia.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-225337/app/chamados/tests/test_e4a2_disparar_e_vigia.py:61): lista fechada SQL, execução dos callbacks dentro da captura e mutante local `on_commit` (`:61`, `:320`, `:351`).

- **RED antes:** 24 falhas nos casos novos; reprodução com o coletor antigo em memória falha em `WITH … UPDATE` (`:241`, `:256`).
- **Depois:** `Ran 3 tests — OK`, incluindo rejeição dos seis mutantes do coletor (`:264`).
- **Não rodaram:** os três testes ORM, incluindo comando com/sem alarme e mutantes UPDATE síncrono/on_commit (`:304`, `:333`, `:351`). Execução e resultados desses caminhos: **[nao verificado]**. Django/freezegun indisponíveis no Python consultado; nenhuma instalação realizada. [nao verificado]

<details>
<summary>Diff aplicado</summary> [nao verificado]

```diff
--- a/app/chamados/tests/test_e4a2_disparar_e_vigia.py
+++ b/app/chamados/tests/test_e4a2_disparar_e_vigia.py
@@ -10,7 +10,7 @@
 from unittest.mock import Mock
 
 from django.core.management import call_command
-from django.db import connection
+from django.db import connection, transaction
 from django.test import SimpleTestCase, TestCase
 from django.test.utils import CaptureQueriesContext
 from freezegun import freeze_time
@@ -59,8 +59,17 @@
 
 
 def escritas_sql(consultas):
-    """DML de qualquer tabela; SAVEPOINT/RELEASE e leituras nao entram."""
-    return [q['sql'] for q in consultas if re.match(r'^\s*(INSERT|UPDATE|DELETE)\b', q['sql'], re.I)]
+    """Lista fechada de leitura/controle; SQL desconhecido conta como escrita."""
+    escritas = []
+    for consulta in consultas:
+        sql = re.sub(r'\A(?:\s+|--[^\r\n]*(?:\r\n?|\n|$)|/\*.*?\*/)*', '', consulta['sql'], flags=re.S)
+        leitura = re.match(r'^(SELECT|SAVEPOINT|RELEASE\s+SAVEPOINT|ROLLBACK\s+TO\s+SAVEPOINT|SET|SHOW)\b',
+                           sql, re.I)
+        if re.match(r'^WITH\b', sql, re.I):
+            leitura = not re.search(r'\b(INSERT|UPDATE|DELETE|MERGE)\b', sql, re.I)
+        if not leitura:
+            escritas.append(consulta['sql'])
+    return escritas
 
 
 class DispararEVigiaFonteTest(SimpleTestCase):
@@ -219,10 +228,63 @@
 
 
 class DispararEVigiaSQLTest(SimpleTestCase):
+    def _verificar_coletor(self, coletor):
+        leituras = [
+            'SELECT 1', ' select 1', 'SAVEPOINT "s1"', 'RELEASE SAVEPOINT "s1"',
+            'ROLLBACK TO SAVEPOINT "s1"', 'SET TIME ZONE UTC', 'SHOW TIME ZONE',
+            'release\n savepoint "s1"', 'rollback\tto\n savepoint "s1"',
+            '-- c\n /* outro */ SELECT 1', '/* c */ -- outro\r\n SHOW TIME ZONE',
+            'WITH x AS (SELECT 1) SELECT * FROM x',
+        ]
+        escritas = [
+            'INSERT INTO tabela VALUES (1)', '\n update tabela SET campo=1', '\tDELETE FROM tabela',
+            'WITH x AS (SELECT 1) UPDATE tabela SET campo=1',
+            'with x as (select 1) insert into tabela select * from x',
+            'WITH x AS (DELETE FROM tabela RETURNING *) SELECT * FROM x',
+            'WITH x AS (SELECT 1) MERGE INTO tabela USING x ON false WHEN MATCHED THEN DO NOTHING',
+            '/* c */ UPDATE tabela SET campo=1',
+            '-- c\n /* outro */ DELETE FROM tabela',
+            'TRUNCATE tabela', 'COPY tabela FROM STDIN', 'DO $$ BEGIN END $$', 'CALL rotina()',
+            'BEGIN', 'COMMIT', 'ROLLBACK', 'RELEASE outra_coisa', 'ROLLBACK TO outra_coisa', [nao verificado]
+            'SELECTED 1', 'SAVEPOINTED s1', 'RELEASE SAVEPOINTED s1',
+            'ROLLBACK TO SAVEPOINTED s1', 'SETTING x', 'SHOWCASE x',
+            '', ' -- so comentario', '/* comentario sem fim',
+        ]
+        for sql in leituras + escritas:
+            self.assertEqual(coletor([{'sql': sql}]), [sql] if sql in escritas else [], sql)
+
+    def test_MORDE_WITH_comentarios_e_SQL_desconhecido(self):
+        self._verificar_coletor(escritas_sql)
+
     def test_coletor_reconhece_DML_e_ignora_transacao_e_leitura(self):
         dml = ['INSERT INTO tabela VALUES (1)', '\n update tabela SET campo=1', '\tDELETE FROM tabela']
         leituras = ['SELECT 1', 'SAVEPOINT "s1"', 'RELEASE SAVEPOINT "s1"', 'ROLLBACK TO SAVEPOINT "s1"']
         self.assertEqual(escritas_sql([{'sql': sql} for sql in leituras + dml]), dml)
+
+    def test_mutantes_do_coletor_sao_rejeitados(self):
+        fonte = Path(__file__).read_text(encoding='utf-8')
+        no = next(n for n in ast.parse(fonte).body if isinstance(n, ast.FunctionDef) and n.name == 'escritas_sql')
+        fonte = ast.get_source_segment(fonte, no)
+        mutantes = {
+            'lista_aberta': ('if not leitura:',
+                             "if not leitura and re.match(r'^(INSERT|UPDATE|DELETE)\\b', sql, re.I):"),
+            'WITH_escritor_liberado': ("leitura = not re.search(r'\\b(INSERT|UPDATE|DELETE|MERGE)\\b', sql, re.I)",
+                                      'leitura = True'),
+            'MERGE_esquecido': ('INSERT|UPDATE|DELETE|MERGE', 'INSERT|UPDATE|DELETE'),
+            'comentarios_nao_removidos':
+                ("sql = re.sub(r'\\A(?:\\s+|--[^\\r\\n]*(?:\\r\\n?|\\n|$)|/\\*.*?\\*/)*', '', consulta['sql'], flags=re.S)",
+                 "sql = consulta['sql'].lstrip()"),
+            'prefixo_sem_fronteira': ('|SHOW)\\b', '|SHOW)'),
+            'ignora_minusculas': ('re.I', '0'),
+        }
+        self._verificar_coletor(escritas_sql)
+        for nome, (antes, depois) in mutantes.items():
+            with self.subTest(mutante=nome):
+                self.assertIn(antes, fonte)
+                ns = {'re': re}
+                exec(compile(fonte.replace(antes, depois), '<mutante_sql_%s>' % nome, 'exec'), ns)
+                with self.assertRaises(AssertionError):
+                    self._verificar_coletor(ns['escritas_sql'])
 
 
 class DispararEVigiaBancoTest(TestCase):
@@ -256,12 +318,13 @@
                 opts = dict(ini=dia_celula.isoformat(), fim=self.dia.isoformat(), alarme=alarme,
                             stdout=out, stderr=StringIO())
                 with CaptureQueriesContext(connection) as consultas:
-                    if alarme:
-                        with self.assertRaises(SystemExit) as cm:
-                            call_command('disparar_perguntas_competencia', **opts)
-                        self.assertEqual(cm.exception.code, 2)
-                    else:
-                        self.assertIsNone(call_command('disparar_perguntas_competencia', **opts))
+                    with self.captureOnCommitCallbacks(execute=True):
+                        if alarme:
+                            with self.assertRaises(SystemExit) as cm:
+                                call_command('disparar_perguntas_competencia', **opts)
+                            self.assertEqual(cm.exception.code, 2)
+                        else:
+                            self.assertIsNone(call_command('disparar_perguntas_competencia', **opts))
                 self.assertEqual(escritas_sql(consultas), [], out.getvalue())
                 self.assertIn('VIGIA FIO MUDO: 1', out.getvalue())
                 self.assertIn('FABRICA-POR-EVENTO: o julgamento deixou 1 ', out.getvalue())
@@ -277,12 +340,34 @@
             return ChamadoColaborador.objects.filter(pk=ch.pk).update(titulo='mutante')
 
         with CaptureQueriesContext(connection) as consultas:
-            self.assertEqual(mutante(), 1)
+            with self.captureOnCommitCallbacks(execute=True):
+                self.assertEqual(mutante(), 1)
         dml = escritas_sql(consultas)
         self.assertGreaterEqual(len(dml), 1)
         self.assertEqual(self._contagens(), antes)
         with self.assertRaises(AssertionError):
             self.assertEqual(dml, [], 'o mesmo veto a DML deve rejeitar o mutante local')
+
+    def test_MORDE_o_mesmo_coletor_acusa_UPDATE_em_on_commit(self):
+        from chamados.models import ChamadoColaborador
+        from chamados.tests.test_selo_emissor_fio_mudo import EmissorPedeDoChamadoMudoTest
+        _, ch = EmissorPedeDoChamadoMudoTest._mudo(self, 'E4A2 MUTANTE ON COMMIT') [nao verificado]
+        antes = self._contagens()
+
+        def mutante():
+            transaction.on_commit(lambda: ChamadoColaborador.objects.filter(pk=ch.pk).update(titulo='mutante on_commit'))
+
+        with CaptureQueriesContext(connection) as consultas:
+            with self.captureOnCommitCallbacks(execute=True) as callbacks:
+                mutante()
+                self.assertEqual(escritas_sql(consultas), [], 'a escrita deve estar adiada ate a saida do contexto')
+        self.assertEqual(len(callbacks), 1)
+        dml = escritas_sql(consultas)
+        self.assertGreaterEqual(len(dml), 1)
+        self.assertEqual(ChamadoColaborador.objects.get(pk=ch.pk).titulo, 'mutante on_commit')
+        self.assertEqual(self._contagens(), antes)
+        with self.assertRaises(AssertionError):
+            self.assertEqual(dml, [], 'o mesmo veto a DML deve rejeitar o mutante adiado')
```

</details>

Comando executado para reproduzir RED e depois os três testes puros, incluindo todos os seis mutantes (`:256`, `:264`): [nao verificado]

```bash
sql_test() {
  MODO="$1" PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
import os
import re
import unittest
from pathlib import Path

p = Path('chamados/tests/test_e4a2_disparar_e_vigia.py')
nodes = [n for n in ast.parse(p.read_text()).body
         if isinstance(n, (ast.FunctionDef, ast.ClassDef))
         and n.name in ('escritas_sql', 'DispararEVigiaSQLTest')]
ns = dict(ast=ast, re=re, Path=Path, __file__=str(p.resolve()),
          SimpleTestCase=unittest.TestCase)
exec(compile(ast.Module(body=nodes, type_ignores=[]), str(p), 'exec'), ns)
cls = ns['DispararEVigiaSQLTest']
if os.environ['MODO'] == 'red':
    ns['escritas_sql'] = lambda qs: [q['sql'] for q in qs
        if re.match(r'^\s*(INSERT|UPDATE|DELETE)\b', q['sql'], re.I)]
    suite = unittest.TestSuite([cls('test_MORDE_WITH_comentarios_e_SQL_desconhecido')]) [nao verificado]
else:
    suite = unittest.defaultTestLoader.loadTestsFromTestCase(cls) [nao verificado]
raise SystemExit(not unittest.TextTestRunner(verbosity=2).run(suite).wasSuccessful()) [nao verificado]
PY
}
sql_test red
sql_test green
```

Saída da reprodução (`:256`, `:264`):

```text
RED:   Ran 1 test — FAILED (failures=1)
GREEN: Ran 3 tests — OK
```

Comandos dos testes ORM pendentes, para o ambiente autorizado da suite (`:304`, `:333`, `:351`); **não executados**: [nao verificado]

```bash
teste_banco=chamados.tests.test_e4a2_disparar_e_vigia.DispararEVigiaBancoTest

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_banco.test_fio_mudo_conta_sem_escrever_com_e_sem_alarme" --verbosity 2
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_banco.test_MORDE_o_mesmo_coletor_acusa_UPDATE_sem_mudar_contagens" --verbosity 2
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_banco.test_MORDE_o_mesmo_coletor_acusa_UPDATE_em_on_commit" --verbosity 2
```

## 09/11 22:44 · sessao 9e0b7679dda1 · FIO-E0-V4-E4A-2 · verificador · origem=Code

As curas locais estão presentes; o **RED continua parcial**, e o relógio único está garantido na borda, mas não em todas as funções chamadas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`; `app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/escala/servico_jornada.py:368`).

Identidade Git, ancestralidade e diffs exatos contra as duas bases: **[nao verificado]**, pois a cópia não tem `.git`. Os 140 testes informados não foram reexecutados; banco, logs e condição efetiva de pouso: **[nao verificado]**. [nao verificado]

**1. Itens destinados à volta 2**

| Item | Veredito e evidência |
|---|---|
| **1** | **CONFORME.** Ambas as asserções exigem zero, preservando as autoridades e acrescentando o histórico da retirada (`app/escala/tests/test_termometro_regua_prazo.py:12`; `app/escala/tests/test_termometro_regua_prazo.py:19`; `app/ponto/tests/test_selfie_ausente_juiz.py:144`; `app/ponto/tests/test_selfie_ausente_juiz.py:148`). |
| **4** | **PROCEDENTE — cura parcial.** AST ampliado e captura da chamada completa estão presentes; permanecem escapes por SQL não reconhecido e callbacks adiados. Correção mínima: ampliar a classificação SQL e executar callbacks dentro da captura, com mutantes correspondentes (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`). |
| **5** | **CONFORME.** O ciclo acusador compara o total de `PerguntaDisputa` antes/depois, além de ausência de vivas, contador igual a um e rc 2 (`app/chamados/tests/test_selo_emissor_fio_mudo.py:269`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:273`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:274`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:275`). |
| **6a** | **CONFORME.** A docstring aponta `consumir_eventos_celula --semear` e esclarece o consumo posterior; o consumidor realmente retorna depois de semear (`app/chamados/management/commands/disparar_perguntas_competencia.py:8`; `app/chamados/management/commands/consumir_eventos_celula.py:28`). |
| **7** | **CONFORME na cura delimitada; parcial no caminho completo.** `_viva(p, hoje=hoje)` está corrigido. `_ja_perguntou → fio_vivo` continua sem esse argumento; existe ainda releitura de instante em `classificar_falta`. Correção mínima da invariável global: encaminhar dia/instante também por esses caminhos (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/management/commands/disparar_perguntas_competencia.py:125`; `app/chamados/juizes.py:1434`; `app/escala/servico_jornada.py:368`). |
| **8a** | **CONFORME na legenda prevista; PROCEDENTE no leitor operacional.** O log explica pergunta **OU Pauta DP** e existência do fato sem validação humana; o placar conserva o rótulo “perguntas”. Correção mínima: esclarecer a unidade também na saída do placar e na linha complementar `VIGIA`, preservando o prefixo raspado (`app/chamados/management/commands/disparar_perguntas_competencia.py:140`; `app/chamados/management/commands/disparar_perguntas_competencia.py:157`; `app/chamados/management/commands/disparar_perguntas_competencia.py:160`; `bin/placar_code.sh:390`). |
| **8b** | **CONFORME no comentário.** Agora declara janela GLOBAL, sem empresa, corte 21, coerente com a chamada e o default (`app/chamados/management/commands/disparar_perguntas_competencia.py:28`; `app/chamados/management/commands/disparar_perguntas_competencia.py:42`; `app/ponto/janelas.py:50`). Inscrição da fila nominal prometida: **[nao verificado]**. |
| **8c** | **CONFORME na borda.** Uma captura produz `agora`, sua conversão produz `hoje`, e a janela recebe esse mesmo dia; a garantia não se estende integralmente às dependências (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:42`; `app/escala/servico_jornada.py:368`). |
| **8d** | **CONFORME.** O literal completo está na saída e é exigido pelo teste (`app/chamados/management/commands/disparar_perguntas_competencia.py:136`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:194`). |

Os dois comentários de sessão também foram ajustados para passado (`app/config/crons.py:2044`; `app/config/crons.py:2046`; `app/config/crons.py:2353`). Introdução de defeito funcional novo pelas curas: **[nao verificado]**; os limites concretos identificados estão discriminados abaixo.

**2. Quem escreve e quem lê**

| Valor | Produtor e consumidores |
|---|---|
| **`agora`** | Produzido por `timezone.now()` na borda. Recebido por `_ontem_em_janela`, fábrica da célula, barreira parcial e teto do FIO MUDO (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:79`; `app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/management/commands/disparar_perguntas_competencia.py:106`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`). |
| **`hoje`** | Produzido por `localdate(agora)`. Alimenta competência, ontem/piso/exclusão de hoje, fábrica, teto e vitalidade direta (`app/chamados/management/commands/disparar_perguntas_competencia.py:39`; `app/chamados/management/commands/disparar_perguntas_competencia.py:42`; `app/chamados/management/commands/disparar_perguntas_competencia.py:69`; `app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`). |
| **`hoje=` de `pergunta_viva`** | `models` reexporta o juiz; o argumento entra na chave do memo, desce ao cálculo, à janela viva e à testemunha de materialização. O outro escritor explícito desse argumento é a porta de relavra (`app/chamados/models.py:4083`; `app/chamados/juizes.py:852`; `app/chamados/juizes.py:856`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1406`; `app/chamados/services/relavra_via.py:242`). |
| **Instante na barreira parcial** | `parcial_so_lista` encaminha `agora` a `houve_fato_novo`; este consulta carimbos e compara os marcos nos dois extremos. O fallback `now()` existe, mas não é usado quando recebe o instante da borda (`app/ponto/services/cartorio.py:400`; `app/ponto/services/cartorio.py:345`; `app/ponto/services/cartorio.py:348`; `app/ponto/services/cartorio.py:417`). |
| **Instante no teto** | A fábrica encaminha para `marcos_vencidos`, que delega a `marcos_com_teto`; este compara o instante recebido, sem reler o relógio (`app/chamados/services/fabrica_da_celula.py:26`; `app/ponto/services/cartorio.py:843`; `app/ponto/services/cartorio.py:858`). O fallback `localtime()` da fábrica não é tomado com `agora` fornecido (`app/chamados/services/fabrica_da_celula.py:89`). |
| **Instante em ontem** | `_ontem_em_janela` compara instantes e encaminha `agora − 2 tetos` a `dia_encerrado` (`app/ponto/services/vigia_de_hora.py:52`; `app/ponto/services/vigia_de_hora.py:58`). |

**PROCEDENTE — ainda há duas famílias de releitura.**

- `_ja_perguntou → fio_vivo → pergunta_viva(p)` omite `hoje`, permitindo `date.today()` na janela viva. A preexistência desse caminho consta do anexo; confirmação independente contra a base: **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1419`; `app/chamados/juizes.py:1434`; `app/chamados/juizes.py:577`).
- Mesmo `_viva(..., hoje=hoje)` pode alcançar `classificar_falta`, diretamente ou pela testemunha materializada; essa função não recebe instante e consulta `_tz.now()` para o teto do turno. Portanto `hoje=` controla a janela civil, não todo o tempo usado pelo juiz (`app/chamados/juizes.py:1393`; `app/chamados/juizes.py:681`; `app/chamados/juizes.py:770`; `app/escala/servico_jornada.py:270`; `app/escala/servico_jornada.py:368`).

O teste novo exercita fragmentos da borda e a chamada de `_viva` com `Mock`; não executa esse caminho interno do juiz. Correção mínima probatória: acrescentar cenário com o juiz real e relógio que avance após a captura (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:125`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:145`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:147`).

**CONFORME nos consumidores diretos quanto a UTC/local.** O dia vem de conversão local, com São Paulo configurado; os consumidores de `agora` inspecionados comparam instantes conscientes de fuso, sem extrair hora civil crua (`app/chamados/management/commands/disparar_perguntas_competencia.py:39`; `app/config/settings/base.py:93`; `app/config/settings/base.py:95`; `app/ponto/services/vigia_de_hora.py:55`; `app/ponto/services/cartorio.py:858`). Igualdade entre o dia do processo usado por `date.today()` e o dia Django em produção: **[nao verificado]**.

As janelas têm significados distintos, explicitamente no código:

| Universo | Limites |
|---|---|
| FIO MUDO | `[ini, fim]`, inclusivo (`app/chamados/management/commands/disparar_perguntas_competencia.py:93`). |
| Célula | `[_piso, fim]`, exclui hoje; competência corrente pode baixar o piso até anteontem e excluir ontem ainda atendido pelo vigia de hora (`app/chamados/management/commands/disparar_perguntas_competencia.py:70`; `app/chamados/management/commands/disparar_perguntas_competencia.py:71`; `app/chamados/management/commands/disparar_perguntas_competencia.py:79`). |
| Janela viva da pergunta | Da véspera do início da competência anterior até o fim da corrente, inclusivamente (`app/chamados/juizes.py:597`; `app/chamados/juizes.py:603`). |
| Fato novo | `(desde, até]`; batidas candidatas em `[data−1, data+2)` (`app/ponto/services/cartorio.py:348`; `app/ponto/services/cartorio.py:350`; `app/ponto/services/cartorio.py:352`). |
| Marco vencido | Inclui igualdade: `marco + teto <= agora` (`app/ponto/services/cartorio.py:858`). |

**PROCEDENTE — apresentação da janela permanece incompleta:** imprime somente `ini/fim`, sem o piso e as exclusões efetivas da célula. Correção mínima: explicitar também esse universo (`app/chamados/management/commands/disparar_perguntas_competencia.py:153`; `app/chamados/management/commands/disparar_perguntas_competencia.py:70`).

| Saída | Quem escreve / quem lê |
|---|---|
| **VIGIA FIO MUDO** | Escreve `len(_mudos)`, após seleção de um motivo por passagem e dedup da chave. Leem os testes de relatório, selo, isolamento por colaborador, célula e teto (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`; `app/chamados/management/commands/disparar_perguntas_competencia.py:133`; `app/chamados/management/commands/disparar_perguntas_competencia.py:136`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:194`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:274`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:136`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:437`). Parser operacional específico: **[nao verificado]**. |
| **FIO DECIDIDO** | Escreve o contador incrementado por existência de linha no domínio do fato, sem filtro de validação humana. Leem o selo e o teste sintético do relatório (`app/chamados/management/commands/disparar_perguntas_competencia.py:130`; `app/chamados/management/commands/disparar_perguntas_competencia.py:140`; `app/chamados/models.py:2753`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:155`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:202`). Parser operacional específico: **[nao verificado]**. |
| **LEGENDA** | Escrita pelo comando; exigida literalmente pelo teste de relatório (`app/chamados/management/commands/disparar_perguntas_competencia.py:157`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:199`). Leitura operacional dessa linha: **[nao verificado]**. |
| **FABRICA-POR-EVENTO** | Conta motivos, inclusive falta de Pauta DP. O placar raspa somente o inteiro; testes raspam/exigem o mesmo prefixo, e o selo shell permite literalmente esse `sed` e arquivo (`app/chamados/services/fabrica_da_celula.py:211`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`; `bin/placar_code.sh:388`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:74`; `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:116`; `app/chamados/tests/test_fabrica_por_evento.py:83`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:127`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:196`; `bin/tests/test_placar_conta_o_retido_do_lastro.sh:500`). |

**PROCEDENTE — os leitores não compartilham integralmente a semântica.** O placar descarta a legenda e publica `perguntas_que_o_julgamento_nao_fez`; os registros operacionais também dizem “perguntas”, embora a unidade possa ser pauta. Correção mínima: esclarecer a unidade nesses leitores, sem alterar o prefixo contratado (`bin/placar_code.sh:388`; `bin/placar_code.sh:390`; `app/docs/RELATO.md:27`; `app/docs/BACKLOG.md:1872`; `app/chamados/services/fabrica_da_celula.py:211`).

O frescor do log usa meia-noite do host, independentemente do dia capturado pelo comando; equivalência dos fusos em produção: **[nao verificado]** (`bin/placar_code.sh:389`; `app/chamados/management/commands/disparar_perguntas_competencia.py:39`). O vigia de execução usa rc, aceitando 0/2 para carimbo; não interpreta essas legendas (`bin/cron_run.sh:22`; `bin/cron_run.sh:32`).

**3. Mordida do RED SQL**

**CONFORME para UPDATE ORM síncrono.** A captura envolve `call_command` inteiro, com e sem alarme, exigindo um alvo em cada caminho. O mutante atualiza exatamente uma linha usando `.objects` e o mesmo `connection`, encontra DML mesmo com contagens inalteradas e demonstra a rejeição pelo veto (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:253`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:266`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:267`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:276`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:280`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:284`).

Esse mutante prova o **coletor**, separadamente; não é uma mutação injetada no `handle`. Para escrita ORM comum na mesma execução síncrona, a configuração CI possui somente `default`; outra conexão/thread efetivamente usada nesta execução: **[nao verificado]** (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:13`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:261`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:276`; `app/config/settings/ci.py:6`).

**PROCEDENTE — callback adiado escapa.** O teste é `TestCase` e não executa callbacks dentro da captura; a própria cópia documenta que eles ficam pendentes e que isso já ocultou defeito. Há sinais reais com escrita agendada, embora a leitura atual não acione seus `post_save` (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:228`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`; `app/ponto/tests/test_selo_chokepoint_escrita.py:197`; `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:17`; `app/chamados/signals.py:227`; `app/chamados/signals.py:238`).

Um mutante `transaction.on_commit(lambda: evento(...))` não encontra nome proibido na catraca, e a escrita de `evento` só ocorreria depois; sua aprovação pela suíte inteira é **[nao verificado]**. Correção mínima: executar `captureOnCommitCallbacks(execute=True)` dentro da captura SQL e acrescentar esse mutante (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:258`; `app/core/observ.py:134`).

| Classe SQL | Resultado do filtro |
|---|---|
| INSERT/UPDATE/DELETE simples, SQL bruto e minúsculas | **Reconhece**, desde que sejam o primeiro token após espaços (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:223`). |
| `bulk_create` emitindo INSERT | **Reconhece pelo SQL**; também está proibido no AST (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:41`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`). |
| DML precedido por `WITH` ou comentário | **Não reconhece. PROCEDENTE.** Correção mínima: classificar esses formatos ou rejeitar SQL não reconhecido como leitura/controle permitido (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`). |
| `SELECT … FOR UPDATE` | **Ignora.** Não altera linha por si, mas a prova também não garante ausência de travas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`). |
| SAVEPOINT/RELEASE/ROLLBACK TO | **Ignora intencionalmente**; DML comum dentro do savepoint continua reconhecível (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:62`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:224`). |
| TRUNCATE/DO/CALL | **Não reconhece** pelo padrão atual; efeitos de escrita desses formatos ficam fora da garantia (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`). |

**4. Asserções novas passam vazias?**

**CONFORME — zero juízes é uma asserção efetiva de cadastro.** É igualdade numérica sobre a função que percorre as entradas e filtra por papel, acompanhada das autoridades específicas. Não é um laço que deixa de executar; também não prova sozinho o comportamento dos comandos (`app/config/crons.py:2356`; `app/escala/tests/test_termometro_regua_prazo.py:12`; `app/escala/tests/test_termometro_regua_prazo.py:19`; `app/ponto/tests/test_selfie_ausente_juiz.py:144`; `app/ponto/tests/test_selfie_ausente_juiz.py:148`). A existência de exatamente uma entrada do disparar, com alarme e sem apply, recebe prova própria (`app/chamados/tests/test_selo_emissor_fio_mudo.py:194`).

**CONFORME — o total prende a tabela pertinente e um alvo real.** O ciclo cria chamado/célula dentro da janela, executa o comando e exige FIO MUDO 1/rc 2; a comparação total impede aceitar uma pergunta nova simplesmente porque nasceu morta (`app/chamados/tests/test_selo_emissor_fio_mudo.py:255`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:260`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:269`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:273`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:274`). UPDATE ou criação seguida de exclusão podem conservar o total; por isso essa prova complementa o RED SQL (`app/chamados/tests/test_selo_emissor_fio_mudo.py:273`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:265`).

**5. Classes, restringindo a atribuição ao delta**

A delimitação Git exata do delta permanece **[nao verificado]**; não atribuo como novidade os remanescentes que o próprio anexo declarou preexistentes.

| Classe | Veredito |
|---|---|
| **Segundo juiz** | **Não identificado — CONFORME no trecho inspecionado.** O comando consulta as autoridades compartilhadas de fábrica, vitalidade, parcial, teto e domínio do fato (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/management/commands/disparar_perguntas_competencia.py:106`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`; `app/chamados/management/commands/disparar_perguntas_competencia.py:130`). |
| **Segundo escritor fora da porta** | **Não identificado no comando atual.** Após consultas, altera coleções locais e imprime; garantia universal indireta: **[nao verificado]** (`app/chamados/management/commands/disparar_perguntas_competencia.py:133`; `app/chamados/management/commands/disparar_perguntas_competencia.py:135`; `app/chamados/management/commands/disparar_perguntas_competencia.py:164`). |
| **Relógio fora da borda** | **Sim no caminho chamado; introdução pelo delta [nao verificado].** A borda foi corrigida, mas persistem `date.today()` e `_tz.now()` internos (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/juizes.py:577`; `app/escala/servico_jornada.py:368`). |
| **Derivação paralela de fonte única** | **Não identificada — CONFORME para janela/teto.** Delega às autoridades existentes (`app/chamados/manag

[truncado em 20000 de 21207 caracteres]

## 09/11 22:42 · sessao 5c4bd2bc64d4 · - · - · origem=Ronald (tmux)

**Itens 1 e 5 — CONFORME.** As duas contagens agora exigem zero e preservam as asserções de autoridade (`app/escala/tests/test_termometro_regua_prazo.py:12`, `:19`; `app/ponto/tests/test_selfie_ausente_juiz.py:144`, `:148`). Não são laços sobre universo vazio: comparam numericamente o resultado da função que percorre as entradas cadastradas e filtra pelo papel (`app/config/crons.py:1787`, `:1798`, `:2356`). Isso sela classificação; não prova sozinho comportamento de cada cron (`app/escala/tests/test_termometro_regua_prazo.py:19`; `app/config/crons.py:2356`).

A contagem total do ciclo prende a tabela pertinente, `PerguntaDisputa`, antes/depois, além de ausência de vivas, alvo contado igual a um e rc 2 (`app/chamados/tests/test_selo_emissor_fio_mudo.py:269`, `:271`, `:273`, `:274`, `:275`). O fixture cria chamado/célula, e o comando visita esse universo; portanto não depende de ausência de alvo (`app/chamados/tests/test_selo_emissor_fio_mudo.py:255`, `:260`; `app/chamados/management/commands/disparar_perguntas_competencia.py:93`). Não detecta atualização que conserve contagem, nem criação seguida de exclusão: é garantia complementar ao RED SQL (`app/chamados/tests/test_selo_emissor_fio_mudo.py:273`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:265`).

**Item 4 — CONFORME quanto ao UPDATE ORM síncrono; PROCEDENTE quanto à abrangência declarada.** A catraca AST ganhou as portas previstas e métodos ORM, incluindo `bulk_create`; seus mutantes exercitam esses nomes (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`, `:84`, `:87`). A captura cerca `call_command` inteiro, com/sem alarme, e exige um alvo em cada caminho (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:253`, `:258`, `:261`, `:264`, `:266`, `:267`).

O mutante usa `ChamadoColaborador.objects.filter(...).update(...)`, exige uma linha atualizada, captura com o mesmo `connection`, verifica DML presente e contagens iguais e demonstra que a asserção “DML vazio” rejeita o resultado (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:276`, `:279`, `:280`, `:281`, `:283`, `:284`). Prova a mordida do coletor nesse UPDATE; não injeta o mutante no `handle` (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:276`; `:261`).

**Conexão/caminho atual:** a captura importa `django.db.connection`, e os três modelos são `models.Model`; as operações verificadas usam `.objects` sem alias explícito (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:13`, `:238`, `:277`; `app/chamados/models.py:27`, `:1710`, `:2406`). CI declara uma conexão `default` (`app/config/settings/ci.py:6`). A chamada é direta e síncrona, sem despacho para thread no teste; `threading.local()` do cartório é armazenamento de contexto, não criação de thread (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:261`; `app/ponto/services/cartorio.py:32`, `:34`). O caminho atual consulta e imprime; a fábrica consultada retorna motivos, e o dedup retorna vitalidade (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`, `:125`, `:130`, `:135`; `app/chamados/services/fabrica_da_celula.py:213`, `:216`; `app/chamados/services/disputa_emissao.py:1412`, `:1419`). Execução efetiva em outra conexão/thread: **[nao verificado]**.

**Escape por `on_commit` — PROCEDENTE.** O teste é `TestCase` e não executa callbacks pendentes dentro da captura (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:228`, `:258`, `:265`). A própria cópia explica que callbacks ficam em `connection.run_on_commit` no `TestCase` e que essa diferença já ocultou defeito (`app/ponto/tests/test_selo_chokepoint_escrita.py:197`; `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:17`). Há signal real que agenda escrita posterior, embora o comando atual não chame a porta de escrita que o acordaria (`app/chamados/signals.py:227`, `:233`, `:238`; `app/chamados/management/commands/disparar_perguntas_competencia.py:130`). Correção mínima: executar `captureOnCommitCallbacks(execute=True)` **dentro** de `CaptureQueriesContext` e acrescentar o mutante deferido; o escritor `evento` insere em `LogAuditoria` (`app/core/observ.py:133`, `:134`).

**Filtro SQL — PROCEDENTE parcial.** Só reconhece comandos cujo primeiro token, após espaços, seja INSERT/UPDATE/DELETE, ignorando caixa (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:61`, `:63`).

- `bulk_create` emitindo INSERT, SQL bruto simples e minúsculas: cobertos pelo padrão; minúsculas já têm caso explícito (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`, `:223`).
- `SAVEPOINT`, `RELEASE` e `ROLLBACK TO SAVEPOINT`: ignorados deliberadamente; não são DML (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:62`, `:224`).
- `WITH ... UPDATE ... RETURNING ... SELECT`, comentário antes do UPDATE, `TRUNCATE`, `DO`/`CALL`: não reconhecidos pelo padrão; `cursor`/`execute` também não são vetados no AST (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`, `:63`).
- `SELECT ... FOR UPDATE`: ignorado; não altera linha por si, mas o RED também não sela ausência de travas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`).

Extraí e executei somente `escritas_sql` via AST em `python3 -B`, sem Django/DB: confirmou reconhecimento de UPDATE minúsculo/INSERT e rejeição ausente para CTE com UPDATE, comentário, SELECT FOR UPDATE, TRUNCATE e SAVEPOINT; fundamento reproduzível na expressão de `app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`. Correção mínima: rejeitar SQL não classificado como leitura/controle permitido e testar CTE, comentários e callback deferido (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`, `:222`, `:258`).

**Classes:** segundo escritor no comando atual, **não identificado** no caminho inspecionado; garantia universal **[nao verificado]** (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`, `:125`, `:130`, `:135`). Selo/RED sem mordida suficiente, **sim**, pelas duas lacunas concretas acima, sem negar a mordida comprovada do UPDATE síncrono (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:63`, `:258`, `:276`).

## 09/11 22:41 · sessao 4056e2502796 · - · - · origem=Ronald (tmux)

Curas e limites:

- **6a CONFORME:** a docstring indica `consumir_eventos_celula --semear` e esclarece “semeia e retorna; consumo na rodada seguinte” (`app/chamados/management/commands/disparar_perguntas_competencia.py:8`). O consumidor confirma o retorno antes de consumir (`app/chamados/management/commands/consumir_eventos_celula.py:28`; `app/chamados/management/commands/consumir_eventos_celula.py:31`).
- **7 CONFORME na correção prevista, parcial no callgraph inteiro:** `_viva(p, hoje=hoje)` recebe agora o dia da borda (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`). Continua `_ja_perguntou` sem dia/instante (`app/chamados/management/commands/disparar_perguntas_competencia.py:125`), que chama `fio_vivo` (`app/chamados/services/disputa_emissao.py:1419`), que chama `pergunta_viva(p)` sem hoje (`app/chamados/juizes.py:1434`), permitindo `date.today()` (`app/chamados/juizes.py:577`). Preexistência desses trechos foi declarada no anexo fornecido; comprovação Git independente [nao verificado].
- **Relógio adicional não mencionado na triagem:** mesmo a chamada corrigida `_viva(..., hoje=hoje)` pode alcançar `classificar_falta` (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1393`). Essa função não recebe instante (`app/escala/servico_jornada.py:270`) e relê `_tz.now()` no teto do turno (`app/escala/servico_jornada.py:368`). Também há chamadas por `_pergunta_materializada_de_fato` (`app/chamados/juizes.py:681`; `app/chamados/juizes.py:770`). Correção mínima para a invariável global: propagar `agora` a esse leitor e `hoje` ao caminho `fio_vivo`; introdução desse defeito pelo delta [nao verificado].
- **8b CONFORME:** comentário agora diz explicitamente janela GLOBAL, sem empresa, corte 21 (`app/chamados/management/commands/disparar_perguntas_competencia.py:28`). A implementação chama `janela_atual(hoje)` sem empresa (`app/chamados/management/commands/disparar_perguntas_competencia.py:42`); o default é efetivamente 21 (`app/ponto/janelas.py:50`; `app/ponto/janelas.py:63`). Linha de backlog na cópia [nao verificado].
- **8c CONFORME na borda:** um único `timezone.now()` produz `agora`; `hoje=timezone.localdate(agora)`; competência recebe esse mesmo hoje (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:42`). Garantia em todas as funções chamadas continua parcial pelos relógios acima (`app/chamados/juizes.py:577`; `app/escala/servico_jornada.py:368`).

Mapa de escritores/leitores:

| Valor | Escritor | Leitores e significado |
|---|---|---|
| `agora` | Borda do comando (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`) | `_ontem_em_janela` recebe com hoje (`app/chamados/management/commands/disparar_perguntas_competencia.py:79`); fábrica recebe ambos (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`); parcial recebe instante (`app/chamados/management/commands/disparar_perguntas_competencia.py:106`); teto do fio recebe ambos (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`). |
| `hoje` | Conversão local do mesmo instante (`app/chamados/management/commands/disparar_perguntas_competencia.py:39`) | Competência (`app/chamados/management/commands/disparar_perguntas_competencia.py:42`), ontem/piso/filtro `<hoje` (`app/chamados/management/commands/disparar_perguntas_competencia.py:69`), fábrica/teto (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`), vitalidade direta (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`). |
| `hoje=` de pergunta_viva | Chamador acima; padrão `None` (`app/chamados/juizes.py:839`) | Integra chave do memo (`app/chamados/juizes.py:852`; `app/chamados/juizes.py:864`), é repassado ao cálculo (`app/chamados/juizes.py:856`), controla janela viva (`app/chamados/juizes.py:1389`) e testemunha materializada (`app/chamados/juizes.py:1406`). |
| `hoje` dentro da janela viva | Argumento ou `date.today()` independente (`app/chamados/juizes.py:577`) | Chama janela corrente e anterior (`app/chamados/juizes.py:578`; `app/chamados/juizes.py:597`), aceita inclusivamente da véspera do início anterior ao fim corrente (`app/chamados/juizes.py:603`). |

Os consumidores diretos de `agora` toleram representação UTC aware: comparam instantes e subtraem durações, sem extrair `.date()`, `.hour` ou `.time()` cru nesses caminhos (`app/ponto/services/vigia_de_hora.py:52`; `app/ponto/services/vigia_de_hora.py:58`; `app/ponto/services/cartorio.py:858`; `app/ponto/turnos.py:2116`; `app/ponto/turnos.py:2124`). A parcial encaminha o instante a `houve_fato_novo`, que usa intervalo `(desde, ate]` e compara carimbos (`app/ponto/services/cartorio.py:400`; `app/ponto/services/cartorio.py:345`; `app/ponto/services/cartorio.py:348`; `app/ponto/services/cartorio.py:365`). Seu teste de marco novo usa `marcos_vencidos` nos dois instantes recebidos (`app/ponto/services/cartorio.py:417`). Nenhum defeito novo por representar `agora` em UTC foi encontrado nesses consumidores (`app/ponto/services/cartorio.py:858`; `app/ponto/turnos.py:2124`).

A configuração local declara São Paulo e `USE_TZ=True` (`app/config/settings/base.py:93`; `app/config/settings/base.py:95`). O fallback `date.today()` é o dia do processo, enquanto a borda usa o fuso corrente Django; igualdade desses dias no host de produção [nao verificado] (`app/chamados/juizes.py:577`; `app/chamados/management/commands/disparar_perguntas_competencia.py:39`).

As janelas não são idênticas por desenho: fio mudo usa `[ini,fim]` (`app/chamados/management/commands/disparar_perguntas_competencia.py:93`); célula usa `[_piso,fim]` e exclui hoje, podendo estender piso até anteontem e excluir ontem ainda atendido pela vigia de hora (`app/chamados/management/commands/disparar_perguntas_competencia.py:70`; `app/chamados/management/commands/disparar_perguntas_competencia.py:79`). O teto de marco inclui igualdade em `marco+30min <= agora` (`app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:858`). `_ontem_em_janela` inclui marco vencendo exatamente à meia-noite (`app/ponto/services/vigia_de_hora.py:53`). A linha `janela` ainda imprime somente a solicitada; não explicita o piso/exclusões da célula (`app/chamados/management/commands/disparar_perguntas_competencia.py:153`). Correção mínima de apresentação: imprimir também o universo efetivo da célula (`app/chamados/management/commands/disparar_perguntas_competencia.py:70`).

O teste novo de relógio prova encaminhamento da borda e `hoje=` por fragmentos com mocks, incluindo o cenário UTC/local em dias diferentes (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:125`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:148`). Não executa a função real `pergunta_viva` nessa prova: avalia sua chamada com `Mock`; portanto não detecta as releituras internas acima (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:145`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:147`).

Derivação paralela de janela/teto: **não identificada no trecho inspecionado**; o comando usa as autoridades existentes (`app/chamados/management/commands/disparar_perguntas_competencia.py:42`; `app/chamados/management/commands/disparar_perguntas_competencia.py:81`; `app/chamados/services/fabrica_da_celula.py:26`; `app/ponto/services/cartorio.py:843`). Sem Git, restrição dessa conclusão estritamente aos hunks do delta [nao verificado].

## 09/11 22:41 · sessao df6029862e30 · - · - · origem=Ronald (tmux)

**8a — CONFORME à cura local prevista.** A legenda declara pergunta **OU Pauta DP** e a barreira FIO DECIDIDO declara somente existência da linha, sem validação humana; as duas mudanças estão presas por asserções literais (`app/chamados/management/commands/disparar_perguntas_competencia.py:140`, `app/chamados/management/commands/disparar_perguntas_competencia.py:157`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:199`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:202`). O domínio consultado realmente não exige validação humana: usa colaborador, motivo, data, instância, `conversa=False` e exclusão das vias fora do fato (`app/chamados/models.py:2753`).

**8d — CONFORME.** A linha contém o literal determinado, incluindo “dia ACUSADO pela lei” e “este lote nao as escreve”; o teste exige a linha inteira (`app/chamados/management/commands/disparar_perguntas_competencia.py:136`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:194`).

**Quem escreve / quem lê as saídas:**

| Valor | Escritor e significado | Leitores encontrados |
|---|---|---|
| `VIGIA FIO MUDO` | `len(_mudos)`, uma tupla elegível colaborador/dia/motivo por chamado visitado, com dedup global da chave; não conta todas as perguntas potencialmente faltantes de cada chamado (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`, `app/chamados/management/commands/disparar_perguntas_competencia.py:133`, `app/chamados/management/commands/disparar_perguntas_competencia.py:136`). | Testes do selo: `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `:127`, `:156`, `:173`, `:210`, `:274`, `:284`; colab: `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`; célula: `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:136`, `:155`, `:164`; teto: `app/chamados/tests/test_pergunta_do_dia_em_curso.py:432`, `:437`; RED: `app/chamados/tests/test_e4a2_disparar_e_vigia.py:194`, `:266`. Parser operacional específico em `app/` ou `bin/`: **[nao verificado]**. |
| `FIO DECIDIDO` | `_bar['fato_ja_existe']`, incrementado após `linhas_dos_fatos(...).exists()` (`app/chamados/management/commands/disparar_perguntas_competencia.py:130`, `:140`). | Selo com linha materializada e validada: `app/chamados/tests/test_selo_emissor_fio_mudo.py:149`, `:155`; relatório sintético com contador zero: `app/chamados/tests/test_e4a2_disparar_e_vigia.py:184`, `:202`. Parser operacional específico: **[nao verificado]**. |
| `LEGENDA` | Explica unidades mistas e significado da barreira (`app/chamados/management/commands/disparar_perguntas_competencia.py:157`). | Asserção literal do relatório: `app/chamados/tests/test_e4a2_disparar_e_vigia.py:199`. Parser operacional específico: **[nao verificado]**. |
| `FABRICA-POR-EVENTO` | `len(deixou)` conta motivos faltantes; no limbo exportado a falta é de Pauta DP (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`, `:154`; `app/chamados/services/fabrica_da_celula.py:205`, `:214`). | Parser operacional raspa somente o inteiro em `bin/placar_code.sh:388`; exige log modificado desde meia-noite do host em `:389`; reemite o campo `perguntas_que_o_julgamento_nao_fez` em `:390`. Regex de testes: `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:74`, `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:116`. Asserções: `app/chamados/tests/test_fabrica_por_evento.py:83`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:127`, `:133`, `:161`, `:392`, `:396`, `app/chamados/tests/test_e4a2_disparar_e_vigia.py:196`, `:267`. Contrato do sed e do arquivo: `bin/tests/test_placar_conta_o_retido_do_lastro.sh:500`, `:501`, `:507`. |

**PROCEDENTE — a semântica não se propaga completamente ao leitor operacional.** A legenda esclarece o log, mas o placar descarta essa legenda e conserva campo nomeado “perguntas” para quantidade que inclui motivos de Pauta DP (`app/chamados/services/fabrica_da_celula.py:211`, `bin/placar_code.sh:388`, `bin/placar_code.sh:390`). A linha complementar `VIGIA` também continua chamando tudo de pergunta (`app/chamados/management/commands/disparar_perguntas_competencia.py:160`). Correção mínima: conservar o prefixo consumido e esclarecer no rótulo operacional e nessa linha que a unidade é “motivos pendentes: pergunta ou pauta”. Atribuição deste resíduo como novidade do delta: **[nao verificado]**; a triagem já reconhecia a unidade mista.

**Fluxo do log — CONFORME no código.** Cadastro associa o comando a `fio_mudo.log`; o encadeado transporta o caminho e redireciona stdout e stderr; o teste prende log, `--alarme` e ausência de `--apply` (`app/config/crons.py:1521`, `bin/encadeado.sh:58`, `bin/encadeado.sh:64`, `bin/encadeado.sh:102`, `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:25`). O vigia de falhas do runner interpreta **rc**, não estas linhas; 0/2 recebem carimbo e não abrem falha de execução (`bin/cron_run.sh:22`, `bin/cron_run.sh:32`). Existência/frescor desse log em produção: **[nao verificado]**.

**Docs operacionais continuam descrevendo o contador como perguntas**, inclusive o crescimento histórico, sem a unidade Pauta DP (`app/docs/RELATO.md:27`, `app/docs/BACKLOG.md:1872`). O registro da condição de pouso continua exigindo zero do emissor com C11 no ar; não é prova de medição realizada (`app/docs/RELATO.md:26`, `app/docs/BACKLOG.md:1871`).

**Classes no trecho inspecionado:**

- **Segundo juiz — não identificado:** primeiro universo delega integralmente à regra compartilhada; segundo usa ata/teto, parcial, vitalidade e domínio do fato existentes (`app/chamados/management/commands/disparar_perguntas_competencia.py:81`, `:100`, `:106`, `:110`, `:125`, `:130`; `app/chamados/services/fabrica_da_celula.py:209`).
- **Segundo escritor — não identificado no comando:** coleções locais e saída após consultas; emissor real segue em função distinta, que chama os escritores existentes (`app/chamados/management/commands/disparar_perguntas_competencia.py:133`, `:164`; `app/chamados/services/fabrica_da_celula.py:133`, `:145`). Garantia universal indireta cabe à inspeção RED/árvore de chamadas: **[nao verificado]** por este explorer.
- **ESMERIL-SEMANTICA — cura local conforme; resíduo operacional procedente**, conforme campo “perguntas” descrito acima (`app/chamados/management/commands/disparar_perguntas_competencia.py:157`, `bin/placar_code.sh:390`).

**Itens 2/3 da sessão permanecem visíveis, sem confundir com curas volta 2:** exceção de disparar continua em `bin/tests/test_papel_prazo_nao_deriva.sh:303`, e o teste a reprova fora da tupla em `:325`; placar ainda trata ausência de membros impressos como falha em `bin/placar_code.sh:209`. A triagem explicitamente os destinou à sessão, não ao construtor volta 2 [dado fornecido; sem arquivo local].

**Linhas de fila prometidas:** inscrição de `E4A2-RELOGIO-FIO-VIVO` e `E4A2-JANELA-POR-EMPRESA` no BACKLOG desta cópia **[nao verificado]**; o registro geral da obra está em `app/docs/BACKLOG.md:1871`, e o comentário somente anuncia a fila de janela por empresa em `app/chamados/management/commands/disparar_perguntas_competencia.py:29`.

## 09/11 22:24 · sessao b40cd15d984c · FIO-E0-V4-E4A-2 · construtor · origem=Code

Alterados cinco arquivos, nos itens da volta 2:

- [disparar_perguntas_competencia.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/chamados/management/commands/disparar_perguntas_competencia.py:36): único `agora/hoje`, janela usando esse dia e `_viva(..., hoje=hoje)`. Reparo documentado em `:8`, janela global em `:28`, texto literal em `:136` e legenda em `:157`. A linha FABRICA permaneceu idêntica em bytes (`:154`).
- [test_e4a2_disparar_e_vigia.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/chamados/tests/test_e4a2_disparar_e_vigia.py:39): AST ampliado; captura DML da chamada inteira com dois alvos distintos, com/sem alarme (`:242`); mesmo coletor aplicado ao mutante ORM `UPDATE` (`:270`). [nao verificado]
- [test_selo_emissor_fio_mudo.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/chamados/tests/test_selo_emissor_fio_mudo.py:266): mantém “nenhuma viva” e exige contagem total antes/depois igual.
- [test_termometro_regua_prazo.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/escala/tests/test_termometro_regua_prazo.py:19) e [test_selfie_ausente_juiz.py](/home/ronald/wt-codex-fio-e0-v4-e4a-2-221508/app/ponto/tests/test_selfie_ausente_juiz.py:148): exigem zero juízes, com o histórico solicitado.

RED antes: o teste do relatório falhou exigindo a linha literal do brief: `FAILED (failures=1)`. Depois, os nove corpos de testes executados por AST/unittest passaram: `Ran 9 tests ... OK`. Essa execução inclui os mutantes de escrita/opções (`test_e4a2_disparar_e_vigia.py:74`), relógio (`:153`) e relatório/alarme (`:211`). [nao verificado]

Comando usado para esses testes e seus mutantes, sem Django: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
import unittest
from pathlib import Path

p = Path('chamados/tests/test_e4a2_disparar_e_vigia.py')
a = ast.parse(p.read_text())
a.body = [
    n for n in a.body
    if not (
        isinstance(n, ast.ImportFrom)
        and (n.module.startswith('django') or n.module == 'freezegun')
    )
    and not (
        isinstance(n, ast.ClassDef)
        and any(isinstance(b, ast.Name) and b.id == 'TestCase' for b in n.bases)
    )
]
ns = {'__file__': str(p.resolve()), 'SimpleTestCase': unittest.TestCase}
exec(compile(a, str(p), 'exec'), ns)
suite = unittest.TestSuite( [nao verificado]
    unittest.defaultTestLoader.loadTestsFromTestCase(c)
    for c in ns.values()
    if isinstance(c, type)
    and c is not unittest.TestCase
    and issubclass(c, unittest.TestCase)
)
r = unittest.TextTestRunner(verbosity=2).run(suite) [nao verificado]
raise SystemExit(not r.wasSuccessful())
PY
```

Os testes reais pelo Django, a captura ORM, a mordida do `UPDATE` e a suíte completa ficaram **[nao verificado]**. A tentativa dos `SimpleTestCase` parou em `No module named 'django'`. O ruff também ficou **[nao verificado]**: a tentativa retornou `No module named ruff`. [nao verificado]

Comandos pendentes para o Code, incluindo o mutante ORM:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4a2_disparar_e_vigia \
  chamados.tests.test_selo_emissor_fio_mudo \
  escala.tests.test_termometro_regua_prazo \
  ponto.tests.test_selfie_ausente_juiz

PYTHONDONTWRITEBYTECODE=1 python3 -m ruff check --no-cache \
  chamados/management/commands/disparar_perguntas_competencia.py \
  chamados/tests/test_e4a2_disparar_e_vigia.py \
  chamados/tests/test_selo_emissor_fio_mudo.py \
  escala/tests/test_termometro_regua_prazo.py \
  ponto/tests/test_selfie_ausente_juiz.py
```

## 09/11 22:10 · sessao 636b02405ee3 · FIO-E0-V4-E4A-2 · verificador · origem=Code

[nao verificado] — identidade Git, ancestralidade, diff integral e relação exata dos arquivos tocados: a cópia fornecida não contém `.git`. Execução da suíte e condição de pouso em produção: [nao verificado]. [nao verificado]

1. **QUEM ESCREVE / QUEM LÊ — CONFORME para rc e dependência; PROCEDENTE nas consequências da classificação.**

| Valor | Definição/produtor | Leitores e significado |
|---|---|---|
| `JUIZES_POR_VARREDURA` | Tupla vazia em `app/config/crons.py:2032` | Alimenta papéis em `app/config/crons.py:2164`; exceção estrutural em `app/core/contratos_estruturais.py:264`; igualdade no tabuleiro em `app/core/tests/test_contract_tabuleiro.py:26`; selo de host em `bin/tests/test_papel_prazo_nao_deriva.sh:325` e `bin/tests/test_papel_prazo_nao_deriva.sh:328`. |
| `PAPEL_DO_CRON` | Disparar declarado `vigia` em `app/config/crons.py:2168` | Consulta por `papel()` em `app/config/crons.py:2344`; juízes derivados em `app/config/crons.py:2356`; tabuleiro em `app/core/management/commands/gerar_diagrama.py:242`; subgrafo VIGIAS em `app/core/management/commands/gerar_diagrama.py:397`; selo de prazo em `bin/tests/test_papel_prazo_nao_deriva.sh:247`. |
| Juízes derivados | Compreensão sobre todas as entradas em `app/config/crons.py:2356` | Medição padrão em `app/chamados/services/vigia_loop.py:82`; placar em `bin/placar_code.sh:484`, incluído no TOTAL em `bin/placar_code.sh:555`; desenho em `app/core/management/commands/gerar_diagrama.py:244`. |
| Argv ENC | `--comp-corrente --alarme` em `app/config/crons.py:1521` | Transportado em `app/config/crons.py:1850`; serializado em `app/core/management/commands/crons.py:40`; executado em `bin/encadeado.sh:64` e `bin/encadeado.sh:102`; selado em `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26` e `app/chamados/tests/test_selo_emissor_fio_mudo.py:193`. |
| Rc | Com alarme, qualquer universo pendente produz 2; sem alarme termina normalmente em `app/chamados/management/commands/disparar_perguntas_competencia.py:160` | Runner captura/grava em `bin/cron_run.sh:11` e `bin/cron_run.sh:13`; aceita 0/2 para carimbo em `bin/cron_run.sh:22` e recuperação em `bin/cron_run.sh:32`; corrente acumula rc em `bin/encadeado.sh:65`; saúde aceita 0/2 em `app/core/services/crons_quebrados.py:46`; pipeline distingue alarme em `app/core/management/commands/pipeline_placar.py:75`. |
| `FABRICA-POR-EVENTO` | Quantidade de motivos pendentes e pares colaborador/dia em `app/chamados/management/commands/disparar_perguntas_competencia.py:153` | Parser operacional em `bin/placar_code.sh:388`; parsers de testes em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:74` e `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:116`; asserções em `app/chamados/tests/test_fabrica_por_evento.py:83`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:127` e `app/chamados/tests/test_e4a2_disparar_e_vigia.py:115`. |
| `VIGIA FIO MUDO` | `len(_mudos)` em `app/chamados/management/commands/disparar_perguntas_competencia.py:135` | Testes em `app/chamados/tests/test_selo_emissor_fio_mudo.py:173`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:437`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:136` e `app/chamados/tests/test_e4a2_disparar_e_vigia.py:114`. Leitor operacional específico: [nao verificado]. |
| `ALVOS` e distribuição | Mesmo conjunto `_mudos`, sem limite, em `app/chamados/management/commands/disparar_perguntas_competencia.py:147` e `app/chamados/management/commands/disparar_perguntas_competencia.py:157` | Testes em `app/chamados/tests/test_selo_emissor_fio_mudo.py:181`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`, `app/chamados/tests/test_fabrica_pela_ata.py:51`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106` e `app/chamados/tests/test_e4a2_disparar_e_vigia.py:116`. Leitor operacional específico: [nao verificado]. |
| `[BARRADO]` | Quatro contadores em `app/chamados/management/commands/disparar_perguntas_competencia.py:138` | A barreira parcial é conferida em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:174`; o teste de relatório injeta barreiras zeradas em `app/chamados/tests/test_e4a2_disparar_e_vigia.py:105`. Parser operacional da linha: [nao verificado]. |

**CONFORME — rc 2 libera o dependente.** O runner cria o arquivo de fim também em rc 2; a corrente espera sua existência. `reconciliar_perguntas_orfas` depende desse gatilho e mede seu próprio universo, sem interpretar os contadores do disparar. “Fim” significa conclusão, não ausência de pendências (`bin/cron_run.sh:19`; `bin/cron_run.sh:22`; `bin/encadeado.sh:89`; `app/config/crons.py:1526`; `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29`).

**PROCEDENTE — não há uniformidade completa.** A classificação vazia produz incompatibilidades nos testes e falso diagnóstico no placar, detalhados abaixo. O pipeline também continua calculando esperados somente de `CRONS`, deixando ENCs fora desse universo (`app/escala/tests/test_termometro_regua_prazo.py:19`; `bin/placar_code.sh:209`; `app/core/management/commands/pipeline_placar.py:40`; `app/core/management/commands/pipeline_placar.py:108`).

2. **TUPLA VAZIA / VEREDITOS — PROCEDENTE.**

- **Duas asserções ainda exigem exatamente 1 juiz.** Correção mínima: exigir zero, mantendo as verificações das respectivas autoridades (`app/escala/tests/test_termometro_regua_prazo.py:19`; `app/ponto/tests/test_selfie_ausente_juiz.py:148`).
- **O selo de host rejeita a exceção remanescente.** A exceção do disparar permanece, mas sua ausência na lista acrescenta falha. Apagá-la isoladamente também quebra o caso que exige sua isenção. Correção mínima: depois de comprovada a condição de saída, retirar a exceção e adaptar sua prova específica, preservando o caso que acusa outro cron (`bin/tests/test_papel_prazo_nao_deriva.sh:303`; `bin/tests/test_papel_prazo_nao_deriva.sh:325`; `bin/tests/test_papel_prazo_nao_deriva.sh:388`; `bin/tests/test_papel_prazo_nao_deriva.sh:390`).
- **Vazio saudável vira “vigia falhou”.** `escritas_por_cron()` devolve `universo=0, crons={}`; o shell imprime somente membros e interpreta ausência de linhas como falha. Correção mínima: emitir representação explícita do universo vazio, distinguindo-o de erro de execução (`app/chamados/services/vigia_loop.py:93`; `app/chamados/services/vigia_loop.py:98`; `bin/placar_code.sh:207`; `bin/placar_code.sh:209`).
- **Disparar desaparece da medição padrão de escritas.** Isso decorre da seleção pelos juízes vivos; não comprova ausência de escrita sem trilha. Correção mínima: preservar uma medição explícita do comando durante a verificação da retirada (`app/chamados/services/vigia_loop.py:64`; `app/chamados/services/vigia_loop.py:80`).

**CONFORME — a célula chamado × “um juiz por pergunta” não muda de cor.** `C()` tem `verde=False`; essa célula não o sobrescreve; `verdes()` conta flags, não listas vazias. O aval contrário à reclassificação para produzir verde permanece, assim como a frase 19/20 do R6 (`app/core/contratos_estruturais.py:62`; `app/core/contratos_estruturais.py:262`; `app/core/contratos_estruturais.py:270`; `app/core/contratos_estruturais.py:1055`; `app/core/placar_estrutural.py:440`).

**CONFORME — os leitores estruturais principais toleram vazio.** A derivação usa `sorted` e compreensão; o desenho usa `len` e laço; o selo compara listas e verifica exceções, sem indexar um primeiro juiz (`app/config/crons.py:2356`; `app/core/management/commands/gerar_diagrama.py:256`; `app/core/management/commands/gerar_diagrama.py:264`; `app/core/tests/test_contract_tabuleiro.py:26`; `app/core/tests/test_selo_contratos_estruturais.py:81`).

Os demais testes de retirada conferem ausência de nomes específicos e continuam compatíveis com vazio: `app/chamados/tests/test_jv3_reconciliar_vira_vigia.py:243`; `app/chamados/tests/test_jv1_feriado_vira_vigia.py:200`; `app/chamados/tests/test_varredura_isentos_evento.py:98`; `app/chamados/tests/test_varredura_feriado_evento.py:106`; `app/chamados/tests/test_varredura_becos_evento_pergunta.py:97`; `app/ponto/tests/test_varredura_lote_prazo.py:38`; `app/ponto/tests/test_varredura_fantasmas_evento.py:61`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:277`; `app/ponto/tests/test_vigia_de_hora.py:234`; `app/ponto/tests/test_vigia_intervalo_ausente.py:27`; `app/ponto/tests/test_par_relampago_pelo_turno.py:80`; `app/ponto/tests/test_orfas_vigia.py:115`; `app/ponto/tests/test_varredura_cluster_evento.py:90`; `app/ponto/tests/test_vigia_reconciliar_vinculo.py:26`; `app/ponto/tests/test_c2_vinculo_vira_lista.py:114`; `app/ponto/tests/test_varredura_classe_b_ausencia.py:35`. O teste da medição padrão exige igualdade com a lista viva, inclusive vazia (`app/chamados/tests/test_escritas_por_cron.py:65`).

3. **ESCRITA REMANESCENTE / CHAMADORES — CONFORME no caminho de domínio inspecionado; garantia universal [nao verificado].**

O comando conserva consultas e impressão; termina após o alarme. O parser oferece somente `ini`, `fim`, `comp-corrente`, `alarme` e `colab`; o ENC operacional já passa `--comp-corrente --alarme` (`app/chamados/management/commands/disparar_perguntas_competencia.py:21`; `app/chamados/management/commands/disparar_perguntas_competencia.py:70`; `app/chamados/management/commands/disparar_perguntas_competencia.py:92`; `app/chamados/management/commands/disparar_perguntas_competencia.py:160`; `app/config/crons.py:1521`).

Há efeitos indiretos de **observabilidade**: `_ata_do_dia` pode chamar `acender_sem_ata` ou `registrar_engolido`; estes fazem logging e, no segundo caso, incrementam contador em memória. A escrita ORM de auditoria pertence a outra função, `evento()` (`app/chamados/services/disputa_emissao.py:895`; `app/chamados/services/disputa_emissao.py:908`; `app/chamados/services/declaracao_texto.py:63`; `app/core/observ.py:73`; `app/core/observ.py:94`; `app/core/observ.py:134`).

Outro chamador executável passando as opções removidas, escrita de domínio por importação ou sinal alcançado nessa execução: **[nao verificado]**. As opções antigas presentes na base incorporada são texto histórico; há também comentário hipotético antigo, não chamada executável (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:165`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:218`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:5`).

4. **RED NOVO — PROCEDENTE: mordida parcial.**

O AST proíbe somente `perguntar`, `_rodar`, `atomic`, `RuntimeError` e as três opções. Os mutantes de escrita testados repetem essa lista (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:27`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:55`).

Ele aceita nomes que efetivamente escrevem neste domínio: `criar_ou_atualizar_disputa`, `gerar_perguntas_disputa`, `PerguntaDisputa.abrir`, `.save()`, `.create()`, `.update()` e `reabrir_sistema`. As portas reais geram perguntas, inserem linhas e salvam reabertura (`app/chamados/services/disputa_emissao.py:662`; `app/chamados/services/disputa_emissao.py:444`; `app/chamados/models.py:3019`; `app/chamados/models.py:2177`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:27`).

Um mutante concreto é inserir, antes de `_mudos.append`, `ChamadoColaborador.objects.filter(pk=ch.pk).update(titulo='mutante')`. O AST não acusa; o teste de banco compara apenas contagens; o teste de relatório executa somente o sufixo iniciado no primeiro `stdout.write`. Assim, essas verificações não observam tal alteração de estado (`app/chamados/management/commands/disparar_perguntas_competencia.py:133`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:24`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:94`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:140`). Aprovação desse mutante pela suíte inteira: **[nao verificado]**.

**Correção mínima:** interceptar SQL durante a chamada completa e rejeitar DML, mantendo as contagens; ampliar a catraca para as portas reais identificadas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:146`; `app/chamados/services/disputa_emissao.py:444`; `app/chamados/models.py:3019`).

**CONFORME — parser e alarme têm mordida específica.** Há rejeição das três opções, combinações dos dois universos com/sem alarme e mutações de condição, rc e prefixo (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:76`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:98`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:123`).

5. **INVERSÃO DOS TESTES — CONFORME em vários contratos; PROCEDENTE numa garantia enfraquecida.** [nao verificado]

Relação exata dos arquivos tocados: **[nao verificado]**. Os contratos atuais relacionados são:

| Teste | Garantia atual | [nao verificado]
|---|---|
| `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:78` | Célula: zero perguntas, contador exato e rc 2. FIO MUDO: zero e rc 2 em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:132`; retratada conserva uma linha em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:156`; parcial mantém zero e rc 0 em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:172`. |
| `app/chamados/tests/test_selo_emissor_fio_mudo.py:184` | Exatamente um ENC, alarme e ausência de apply. Sem chave conserva contagem em `app/chamados/tests/test_selo_emissor_fio_mudo.py:205`. |
| `app/chamados/tests/test_pergunta_do_dia_em_curso.py:435` | Marco vencido contado e nenhuma pergunta criada; o helper não verifica rc explicitamente em `app/chamados/tests/test_pergunta_do_dia_em_curso.py:422`. |
| `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19` | Preserva formato de seis posições, log e argv; exige alarme e rejeita apply. |
| `app/ponto/tests/test_varredura_lote_prazo.py:62` | Exige exatamente zero juízes e papel vigia. |
| `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:34` | Conserva contagem de perguntas; confere alvos e distribuição em `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`. |
| `app/chamados/tests/test_fabrica_por_evento.py:76` | Conserva contagem e exige contador da fábrica. |
| `app/chamados/tests/test_fabrica_pela_ata.py:33` | Conserva contagem antes de devolver o relatório. |
| `app/chamados/tests/test_e4a2_disparar_e_vigia.py:146` | Conserva quatro contagens e verifica rc com/sem alarme; limitação quanto a UPDATE descrita no item 4. |

**PROCEDENTE — “nenhuma viva” não equivale a “nenhuma criada”.** No ciclo acusador, a nova asserção aceita uma pergunta criada já morta. Correção mínima: acrescentar comparação de contagem total antes/depois, além de ausência de vivas e rc 2 (`app/chamados/tests/test_selo_emissor_fio_mudo.py:266`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:270`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:272`).

6. **LEGIBILIDADE SITUACIONAL — PROCEDENTE.**

**CONFORME parcialmente:** o docstring identifica vigia, dois universos, rc e emissor por evento; o comentário aponta a fábrica. Isso atende parte da fronteira “quem manda aqui” (`app/chamados/management/commands/disparar_perguntas_competencia.py:1`; `app/chamados/management/commands/disparar_perguntas_competencia.py:13`; `CLAUDE.md:41`; `CLAUDE.md:168`).

**PROCEDENTE — falta a ponte de reparo do estoque.** O comentário transforma estoque pendente em diagnóstico, mas não aponta a porta existente. Correção mínima: indicar `consumir_eventos_celula --semear` e esclarecer que a semeadura retorna antes do consumo, exigindo consumo posterior (`app/chamados/management/commands/disparar_perguntas_competencia.py:83`; `app/chamados/management/commands/consumir_eventos_celula.py:14`; `app/chamados/management/commands/consumir_eventos_celula.py:28`).

**PROCEDENTE — comentários contraditórios.** O cadastro anuncia a saída para vigia e, logo depois, afirma que o FIO MUDO segue escrevendo; a docstring do contador ainda apresenta disparar como emissor. Correção mínima: marcar explicitamente esses trechos como situação anterior ou atualizar sua descrição (`app/config/crons.py:2044`; `app/config/crons.py:2046`; `app/config/crons.py:2353`).

A condição legal de saída continua preservada no corte e no selo; não foi apagada desses lugares. Perda de outra história normativa sem cópia equivalente: **[nao verificado]** (`app/docs/CORTES.md:305`; `bin/tests/test_papel_prazo_nao_deriva.sh:307`).

7. **CLASSES QUE JÁ REPROVARAM OBRA.**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando registrado | **CONFORME no trecho inspecionado:** consulta regra compartilhada da fábrica, `parcial_so_lista`, pergunta viva e domínio do fato (`app/chamados/management/commands/disparar_perguntas_competencia.py:80`; `app/chamados/management/commands/disparar_perguntas_competencia.py:99`; `app/chamados/management/commands/disparar_perguntas_competencia.py:105`; `app/chamados/management/commands/disparar_perguntas_competencia.py:129`). |
| Segundo escritor fora da porta | **CONFORME no caminho de domínio inspecionado**, com limite probatório do RED apontado no item 4 (`app/chamados/management/commands/disparar_perguntas_competencia.py:133`; `app/chamados/management/commands/disparar_perguntas_competencia.py:160`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:27`). |
| Relógio fora da borda | **PROCEDENTE:** `_viva(p)` não recebe `_hoje`; pode alcançar `date.today()`. `_ja_perguntou` também chega a `fio_vivo`, que não injeta hoje. Correção mínima: propagar o dia capturado na borda pelos leitores (`app/chamados/management/commands/disparar_perguntas_competencia.py:99`; `app/chamados/management/commands/disparar_perguntas_competencia.py:124`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:577`; `app/chamados/services/disputa_emissao.py:1419`; `app/chamados/juizes.py:1434`). |
| Derivação paralela de fonte única | **CONFORME no teto e janela:** chama funções existentes; há problemas de parâmetros/semântica abaixo (`app/chamados/management/commands/disparar_perguntas_competencia.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:109`). |
| Selo/RED sem mordida suficiente | **PROCEDENTE**, para escritores alternativos e UPDATE (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:27`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:143`). |
| Ordem de trava | **[nao verificado]** quanto à execução e concorrência. O comando inspecionado faz seleção e relatório, sem novo bloco transacional (`app/chamados/management/commands/disparar_perguntas_competencia.py:70`; `app/chamados/management/commands/disparar_perguntas_competencia.py:134`). |
| Pouso antes do pré-requisito | **[nao verificado]** quanto ao pouso efetivo. A condição exige medição do FIO MUDO e C11 pousado; o registro exige medição com C11 no ar. A chamada ao escritor no consumidor prova código local, não implantação. Correção mínima: apresentar essa evidência antes de liberar pouso (`app/docs/CORTES.md:305`; `app/docs/RELATO.md:26`; `app/chamados/reconciliador.py:1387`). |
| Campo com dois significados | **PROCEDENTE**, nos casos seguintes (`app/chamados/management/commands/disparar_perguntas_competencia.py:139`; `app/chamados/management/commands/disparar_perguntas_competencia.py:153`; `app/chamados/services/fabrica_da_celula.py:211`). |

**PROCEDENTE — unidade:** `FABRICA-POR-EVENTO` diz “perguntas”, mas pode contar falta de Pauta DP; `FIO DECIDIDO` decorre de existência de linha do fato, sem exigir validação humana. Correção mínima: explicar as unidades e ajustar a legenda da barreira, preservando o prefixo consumido pelo placar (`app/chamados/services/fabrica_da_celula.py:211`; `app/chamados/management/commands/disparar_perguntas_competencia.py:129`; `app/chamados/management/commands/disparar_perguntas_competencia.py:139`; `app/chamados/models.py:2753`; `bin/placar_code.sh:388`).

**PROCEDENTE — corte por empresa:** o comentário promete corte por empresa, mas `janela_atual` é chamada sem empresa; nesse caso o corte é 21. Correção mínima: tornar o universo explicitamente global ou calcular a janela por empresa usando a mesma autoridade (`app/chamados/management/commands/disparar_perguntas_competen

[truncado em 20000 de 21489 caracteres]

## 09/11 22:08 · sessao d88a5eaae8fc · - · - · origem=Ronald (tmux)

3. **CONFORME quanto à retirada da escrita de domínio:** o comando termina após consultas, contadores e eventual `SystemExit(2)`; as opções restantes são `ini/fim/comp-corrente/alarme/colab` (`app/chamados/management/commands/disparar_perguntas_competencia.py:21-32,70-163`). Os helpers examinados consultam a ata, o juiz e o domínio do fato; o escritor continua separado em `perguntar_pela_celula` (`app/chamados/services/fabrica_da_celula.py:15-54,67-130,151-190,205-236`).

- A chamada indireta `_ata_do_dia` pode produzir observabilidade: `acender_sem_ata` só chama `logging.warning`; `registrar_engolido` modifica contador em memória e chama `logging.error`. Nenhuma delas chama o escritor de `LogAuditoria`, que é outra função (`app/chamados/services/disputa_emissao.py:884-910`; `app/chamados/services/declaracao_texto.py:63-72`; `app/core/observ.py:73-82,94-138`).
- Os signals inspecionados são registrados no `ready` e respondem a `pre_save/post_save`; o comando não executa essas operações. Ausência universal de efeitos em toda dependência de inicialização Django: **[nao verificado]** (`app/chamados/apps.py:8-10`; `app/chamados/signals.py:178-188,227-250`).
- O cron real passa somente `--comp-corrente --alarme` (`app/config/crons.py:1521`). As chamadas inspecionadas não passam opções removidas (`app/chamados/tests/test_selo_emissor_fio_mudo.py:58-65,260-261,292-293`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:61-66`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:34-41`; `app/chamados/tests/test_fabrica_por_evento.py:76-84`; `app/chamados/tests/test_fabrica_pela_ata.py:34-41`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:198-199`; `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:84-90`).
- Strings antigas restantes são história/base de teste, não chamadores atuais: BASE embutida conserva as três opções; TICKETS relata atos passados (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:165-166,218-223`; `app/docs/TICKETS.md:1116,1120`). O `--apply` de `test_cobparc5...:294` é encaminhado a outro comando, `emitir_furo_retroativo` (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:278-294`).

6. **PROCEDENTE — documentação situacional incompleta/contraditória.**

- O novo docstring identifica vigia, finalidade e escritor por evento; os comentários apontam a fábrica e preservam as guardas relevantes (`app/chamados/management/commands/disparar_perguntas_competencia.py:1-15,54-67,83-85,102-109,127-129`).
- Falta uma ponte para o reparo do estoque no texto do comando; a porta existente é `consumir_eventos_celula --semear`, que apenas semeia e retorna — consumir exige execução posterior normal (`app/chamados/management/commands/disparar_perguntas_competencia.py:1-19,83-85`; `app/chamados/management/commands/consumir_eventos_celula.py:14-15,25-31`). Correção mínima: citar essa sequência no docstring.
- O comentário novo anuncia VIGIA, mas a continuação ainda afirma “segue ESCREVENDO — não há evento”; a função de contagem também afirma que disparar emite pergunta (`app/config/crons.py:2044-2047,2348-2354`). Correção mínima: atualizar as frases ao estado novo, conservando a história como passado.
- A história de teto por instante e emissão por evento continua na fábrica; a exceção/condição de saída continua no corte. Não demonstrei lei viva apagada sem outra localização (`app/chamados/services/fabrica_da_celula.py:1-11`; `app/docs/CORTES.md:305`; BASE histórica: `app/chamados/tests/test_e4a2_disparar_e_vigia.py:174-190`).

7. Classes:

- **Segundo juiz registrado: CONFORME no trecho examinado.** Seleção chama as autoridades existentes de motivos, vivo, parcial e domínio do fato (`app/chamados/management/commands/disparar_perguntas_competencia.py:15,99,105,109,124,129`).
- **Segundo escritor: CONFORME estaticamente**, com limite de inicialização acima (`app/chamados/management/commands/disparar_perguntas_competencia.py:70-163`; `app/chamados/services/fabrica_da_celula.py:133-147,177-190`).
- **Relógio fora da borda: PROCEDENTE, preexistente.** O comando chama `_viva(p)` sem `_hoje`; o juiz pode alcançar `_dentro_da_janela_viva`, cujo fallback é `date.today()`. `_ja_perguntou` também chega a `fio_vivo` sem transportar o dia (`app/chamados/management/commands/disparar_perguntas_competencia.py:99,124`; `app/chamados/juizes.py:1388-1389,569-578,1425-1434`; `app/chamados/services/disputa_emissao.py:1412-1419`). Correção mínima: propagar o dia capturado por ambas as cadeias. Há ainda duas leituras independentes de relógio na própria borda, antes/depois dos imports (`app/chamados/management/commands/disparar_perguntas_competencia.py:38,49-51`).
- **Derivação paralela de fonte única: CONFORME quanto a teto/motivos**, delegados à fábrica e ao cartório (`app/chamados/management/commands/disparar_perguntas_competencia.py:15,80,109`; `app/chamados/services/fabrica_da_celula.py:15-29`; `app/ponto/services/cartorio.py:831-859`).
- **Ordem de trava: CONFORME no comando**, que não abre transação/trava no corpo atual; efeitos concorrentes concretos: **[nao verificado]** (`app/chamados/management/commands/disparar_perguntas_competencia.py:34-163`).
- **Pouso antes do pré-requisito: [nao verificado], não liberado.** O corte exige medição e C11 pousado; o RELATO concretiza “`--apply` achar 0 com C11 no ar”. O dado fornecido não prova essa rodada (`app/docs/CORTES.md:305`; `app/docs/RELATO.md:26`). A implementação do consumidor/C11 na cópia não comprova implantação (`app/chamados/reconciliador.py:1384-1387`).
- **Semântica de janela: PROCEDENTE, preexistente.** `--comp-corrente` chama `janela_atual` sem empresa, portanto usa corte default 21 e aplica uma janela conjunta, embora o comentário diga incluir corte por empresa (`app/chamados/management/commands/disparar_perguntas_competencia.py:24-27,38,70,92`; `app/ponto/janelas.py:36-51,60-63`). Correção mínima: explicitar universo de corte padrão ou calcular janela por empresa conforme contrato autorizado.
- **Inclusividade e dia do turno: CONFORME no recorte explícito.** INI/FIM são inclusivos; o primeiro universo estende o piso e exclui hoje/ontem ainda coberto pelo vigia; o segundo usa a data da célula; vencimento é por instante e usa o turno encadeado (`app/chamados/management/commands/disparar_perguntas_competencia.py:68-80,92,101,109`; `app/ponto/services/cartorio.py:831-859`). A linha “janela” imprime a solicitada, sem revelar o piso ampliado do primeiro universo (`app/chamados/management/commands/disparar_perguntas_competencia.py:69-70,152`).
- **Unidade do contador: PROCEDENTE, preexistente.** `FABRICA-POR-EVENTO` diz “perguntas”, mas o helper também retorna motivos de Pauta DP ausente no limbo exportado (`app/chamados/management/commands/disparar_perguntas_competencia.py:153-156`; `app/chamados/services/fabrica_da_celula.py:205-216`). `[BARRADO] FIO DECIDIDO` conta existência de linha do fato, sem provar decisão humana (`app/chamados/management/commands/disparar_perguntas_competencia.py:127-140`). Correção mínima: corrigir as legendas preservando o prefixo consumido pelo placar (`bin/placar_code.sh:388-390`).

## 09/11 22:07 · sessao e643b06f0f26 · - · - · origem=Ronald (tmux)

4. **PROCEDENTE — RED tem mordida parcial.** O verificador proíbe somente `perguntar`, `_rodar`, `atomic`, `RuntimeError` e três opções; não proíbe outros escritores (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:24-44`). As mutações testadas repetem precisamente essa lista (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:55-74`).

   Os nomes ignorados alcançam escrita real: `criar_ou_atualizar_disputa` agrega/cria e gera perguntas (`app/chamados/services/disputa_emissao.py:662-682`); `gerar_perguntas_disputa` chama `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:444-452`); a porta insere por `.save(force_insert=True)` (`app/chamados/models.py:3013-3019`); `reabrir_sistema` salva campos (`app/chamados/models.py:2163-2177`).

   Experimento em memória: extrair `violacoes` por AST e fornecer mutantes com esses nomes, `.save()`, `.create()`, `.reabrir_sistema()` ou `.update()` retornou conjunto vazio. Reprodução independente desse resultado na suíte completa: **[nao verificado]**. [nao verificado]

   Mutante concreto: inserir imediatamente antes de `_mudos.append(...)` uma chamada `ChamadoColaborador.objects.filter(pk=ch.pk).update(titulo='mutante')`; o verificador aceita, e a seleção oferece `ch` nesse ponto (`app/chamados/management/commands/disparar_perguntas_competencia.py:96-133`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:24-44`). O novo teste de banco compara somente quantidades de quatro tabelas, deixando alteração de estado sem mudança de cardinalidade invisível (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:140-162`). O teste de relatório executa apenas o sufixo iniciado no primeiro `stdout.write`, portanto também ignora essa inserção anterior (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:91-109`).

   **Correção mínima:** durante a chamada completa, interceptar SQL e reprovar qualquer DML; ampliar a catraca AST para os escritores canônicos efetivos. Contagens iguais devem continuar como garantia complementar (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:140-162`).

   **CONFORME — alarme e parser possuem mordida explícita.** Há combinações célula/FIO MUDO com e sem chave, rc esperado, mensagem e seis mutantes de relatório/alarme; há rejeição das três opções removidas (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:76-85,98-130`). O teste contra `FONTE_BASE` usa fonte incorporado; autenticidade dessa reprodução perante o commit original: **[nao verificado]** (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:51-53,165`).

5. **CONFORME, quanto aos contratos visíveis — não viraram apenas texto.** Lista exata dos arquivos tocados pelo commit: **[nao verificado]**; os contratos atuais relacionados são: [nao verificado]

| Arquivo | Força atual |
|---|---|
| `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py` | Célula: zero perguntas, contador exato e rc 2 (`:78-83`). FIO MUDO concorde: zero e rc 2 (`:132-139`). Retratada: conserva exatamente uma linha e rc 2 (`:141-157`). Limbo/parcial: zero e rc apropriado (`:159-176`). |
| `app/chamados/tests/test_selo_emissor_fio_mudo.py` | Exatamente um ENC, com alarme e sem apply (`:184-198`). Sem chave: contagem de perguntas igual, além do texto (`:205-213`). Ciclo: nenhuma pergunta viva e rc 2; lei calada: contagem igual e rc 0 (`:266-282`). |
| `app/chamados/tests/test_pergunta_do_dia_em_curso.py` | Marco vencido é contado e perguntas continuam inexistentes (`:435-438`). Helper roda sem alarme e não captura/verifica rc explicitamente (`:422-428`). |
| `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py` | Mantém seis posições, log absoluto, serialização exata; exige alarme e proíbe apply (`:19-42`). |
| `app/ponto/tests/test_varredura_lote_prazo.py` | Igualdade exata `len(...) == 0`, mais papel `vigia` (`:62-63`). |
| `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py` | Helper exige contagem de perguntas igual; casos exigem distribuição e cardinalidade dos alvos (`:34-54`). |
| `app/chamados/tests/test_fabrica_por_evento.py` | Contador da fábrica e contagem de perguntas igual (`:82-84`). |
| `app/chamados/tests/test_fabrica_pela_ata.py` | Helper exige contagem igual antes de devolver relatório (`:34-41`). |
| `app/chamados/tests/test_e4a2_disparar_e_vigia.py` | Novo teste verifica quatro contagens e rc com/sem alarme, mas não detecta UPDATE; ressalva procedente do item 4 (`:140-162`). |

**Ressalva localizada:** “nenhuma viva” no ciclo permite, isoladamente, criar pergunta já morta; não equivale a contagem total igual (`app/chamados/tests/test_selo_emissor_fio_mudo.py:266-272`). Correção mínima: também comparar contagens totais antes/depois nesse cenário (`app/chamados/tests/test_selo_emissor_fio_mudo.py:266-272`).

## 09/11 22:06 · sessao 2e3d14447032 · - · - · origem=Ronald (tmux)

Achados de bloqueio:

- **PROCEDENTE — dois testes ainda exigem 1 juiz.** A lista agora vazia é incompatível com `assertEqual(len(...), 1)` em `app/escala/tests/test_termometro_regua_prazo.py:19` e `app/ponto/tests/test_selfie_ausente_juiz.py:148`. Correção mínima: atualizar ambos para zero, preservando as asserções de autoridade e ausência do cron.
- **PROCEDENTE — selo de host fica vermelho.** A exceção de `disparar_perguntas_competencia` permanece em `bin/tests/test_papel_prazo_nao_deriva.sh:303`; a condição de saída permanece em `:307`; o selo rejeita exceção cujo cron saiu da lista em `:325-326` e devolve erro em `:413`. Correção mínima: retirar a exceção após comprovar a condição, ajustar também a prova específica que exige essa exceção em `:388-389`.
- **PROCEDENTE — lista vazia vira falso “vigia falhou”.** `escritas_por_cron()` retorna `universo=0, crons={}` para lista vazia, pela construção em `app/chamados/services/vigia_loop.py:80-99`. O placar imprime apenas entradas de `crons`, e seu `grep` sem linhas aciona `escritas_por_cron=? (vigia falhou)` em `bin/placar_code.sh:204-209`. Correção mínima: representar explicitamente universo vazio/zero sem diagnosticar falha.

Mapa de valores:

| Valor | Escritor/definição | Leitores |
|---|---|---|
| `JUIZES_POR_VARREDURA` | Tupla em `app/config/crons.py:2032`; disparar só aparece como comentário histórico em `:2044` | Construção de papéis em `:2164`; exceção da matriz em `app/core/contratos_estruturais.py:264`; testes de igualdade e verde em `app/core/tests/test_contract_tabuleiro.py:26,43-44`; host em `bin/tests/test_papel_prazo_nao_deriva.sh:325,328` |
| `PAPEL_DO_CRON` | Vigia declarado em `app/config/crons.py:2168` | `papel()` em `:2344`; juízes derivados em `:2356`; gerador de tabuleiro em `app/core/management/commands/gerar_diagrama.py:242`; subgrafo de vigias em `:397`; testes em `app/core/tests/test_contract_tabuleiro.py:16,19,26`; classificação de prazo no host em `bin/tests/test_papel_prazo_nao_deriva.sh:247` |
| `juizes_por_varredura()` | Deriva por papel sobre todas as entradas em `app/config/crons.py:2356` | `escritas_por_cron` em `app/chamados/services/vigia_loop.py:81-82`; placar em `bin/placar_code.sh:484-486`, soma ao TOTAL em `:555-556`; diagrama em `app/core/management/commands/gerar_diagrama.py:244,256,264`; contagens fixas conflitantes nos dois testes acima |
| Argv ENC | `--comp-corrente --alarme` em `app/config/crons.py:1521` | `corrente()` transporta args em `:1850`; comando serializa em `app/core/management/commands/crons.py:40-41`; shell recebe e executa em `bin/encadeado.sh:58-64,102-104`; testes em `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26-27` e `app/chamados/tests/test_selo_emissor_fio_mudo.py:193-198` |
| Rc 0/2 | Sem alarme termina normalmente; com alarme sai 2 por qualquer universo em `app/chamados/management/commands/disparar_perguntas_competencia.py:160-163` | Runner grava em `bin/cron_run.sh:11-13`, carimba fim em `:22-25`, aceita como recuperado em `:32-35`; corrente continua acumulando rc em `bin/encadeado.sh:64-68`; saúde aceita 0/2 em `app/core/services/crons_quebrados.py:16,46`; pipeline distingue alarme em `app/core/management/commands/pipeline_placar.py:75-79` |
| `FABRICA-POR-EVENTO` | `len(deixou)` e pares distintos em `app/chamados/management/commands/disparar_perguntas_competencia.py:153-155` | Parser operacional em `bin/placar_code.sh:388-390`; parsers de teste em `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:74` e `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:116`; asserções em `app/chamados/tests/test_fabrica_por_evento.py:83` e `app/chamados/tests/test_pergunta_do_dia_em_curso.py:127,133,161,392,396` |
| `VIGIA FIO MUDO` | `len(_mudos)` em `app/chamados/management/commands/disparar_perguntas_competencia.py:135-136` | Testes em `app/chamados/tests/test_selo_emissor_fio_mudo.py:84,127,156,173,210,271,281`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:48`; teste por substring em `app/chamados/tests/test_pergunta_do_dia_em_curso.py:432,437`; leitores operacionais específicos encontrados: [nao verificado] |
| `ALVOS` / distribuição | `app/chamados/management/commands/disparar_perguntas_competencia.py:147-148,157-158` | Testes em `app/chamados/tests/test_selo_emissor_fio_mudo.py:75-76,113-114,181-182`, `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49-54`, `app/chamados/tests/test_fabrica_pela_ata.py:51,64,71`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106`; leitor operacional específico: [nao verificado] |
| `[BARRADO]` | Quatro barreiras em `app/chamados/management/commands/disparar_perguntas_competencia.py:138-142` | Leitor operacional/parser específico: [nao verificado] |

**CONFORME — dependente e rc mantêm contrato.** `reconciliar_perguntas_orfas` é dependente pelo gatilho em `app/config/crons.py:1526`; o shell espera existência do arquivo de fim em `bin/encadeado.sh:84-94`; rc 2 também produz esse arquivo em `bin/cron_run.sh:22-25`. Seu comando só mede seu próprio universo, sem ler stdout ou rc do disparar, em `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29-36`. Portanto a ampliação do motivo de rc 2 é compatível com esses leitores; fim significa conclusão, não zero pendências (`bin/cron_run.sh:19-25`).

**CONFORME — célula não muda de cor.** O construtor `C()` usa `verde=False` por padrão em `app/core/contratos_estruturais.py:62`; a célula chamado/juiz não passa outro valor em `:262-270`. O numerador conta flags, não tamanho de exceções, em `:1053-1055`. O aval proibindo verde por reclassificação permanece em `:270`, e a cópia R6 ainda declara 19/20 em `app/core/placar_estrutural.py:440`. Esvaziar a tupla não a torna verde (`app/core/contratos_estruturais.py:62,262-270,1055`).

**PROCEDENTE documental — comentário contraditório.** A declaração diz que disparar saiu e virou vigia em `app/config/crons.py:2044`, mas a continuação diz que “segue ESCREVENDO” e não tem evento em `:2046`. A docstring de `juizes_por_varredura()` também apresenta disparar como emissor em `:2353`. Correção mínima: tornar explícito que esses trechos descrevem o passado.

**CONFORME com limitação de representação — geradores.** O tabuleiro usa todas as entradas e aceita lista vazia por `len` e laço em `app/core/management/commands/gerar_diagrama.py:242-265`; o subgrafo VIGIAS usa só `CRONS` em `:396-397`, portanto não passa a mostrar esse ENC individualmente. A função de lista usa compreensão e `sorted`, sem índice nem `max`, em `app/config/crons.py:2356`; os defeitos de vazio localizados estão nas asserções e no placar citados.

Execução dos testes/selos e comprovação da condição de pouso em produção: **[nao verificado]**. [nao verificado]

## 09/11 22:03 · sessao 3d32616f2541 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Cura aplicada: o comando conta os dois universos, remove as opções de escrita e sai 2 somente com `--alarme` e pendência. [comando:21](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/management/commands/disparar_perguntas_competencia.py:21), [saída:160](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/management/commands/disparar_perguntas_competencia.py:160) [nao verificado]

Arquivos tocados e motivo:

| Arquivo | Alteração |
|---|---|
| [disparar_perguntas_competencia.py:134](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/management/commands/disparar_perguntas_competencia.py:134) | Retirada do escritor e do rollback; diagnóstico `VIGIA FIO MUDO`. |
| [test_e4a2_disparar_e_vigia.py:24](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_e4a2_disparar_e_vigia.py:24) | RED, base integral, mutantes, parser e teste ORM das quatro contagens. | [nao verificado]
| [e4a2_provas.py:34](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/e4a2_provas.py:34) | Execução das provas de fonte sem Django. |
| [test_selo_emissor_fio_mudo.py:205](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_selo_emissor_fio_mudo.py:205) | Contagem sem criação; cron e ciclo com alarme. |
| [test_jv4a_disparar_vigia_da_celula.py:132](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:132) | FIO MUDO e limbo contam; retratada é preparada pelo evento. |
| [test_jv4a_elo_encadeado_tem_log.py:26](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:26) | Argv exige alarme e rejeita apply. |
| [test_pergunta_do_dia_em_curso.py:434](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_pergunta_do_dia_em_curso.py:434) | Marco vencido é contado sem pergunta. |
| [test_cobparc5_parcial_sem_periodo_so_lista.py:198](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:198) | Chamada com alarme. |
| [test_colab_nao_vaza_fio_mudo.py:35](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:35) | Recorte por colaborador e contagem persistida igual. |
| [test_fabrica_pela_ata.py:34](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_fabrica_pela_ata.py:34) | Ata conta sem criação pelo lote. |
| [test_fabrica_por_evento.py:76](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_fabrica_por_evento.py:76) | Conserva escritor do evento; lote não altera contagem. |
| [test_selo_disparo_nao_cobra_desligado.py:88](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:88) | Chamada com alarme. |
| [test_varredura_lote_prazo.py:62](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/ponto/tests/test_varredura_lote_prazo.py:62) | Exige zero juízes e papel vigia. |

O RED anterior à cura falhou nos sete itens proibidos. Depois, passaram **17 provas de fonte via unittest**; os **14 mutantes** foram rejeitados com rc 1. A base integral permanece no teste. [RED:48](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_e4a2_disparar_e_vigia.py:48), [base:165](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/test_e4a2_disparar_e_vigia.py:165), [runner:80](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/chamados/tests/e4a2_provas.py:80) [nao verificado]

Comandos reproduzíveis do RED, provas posteriores e de cada mutante:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4a2_provas base
# rc 1: sete violações

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4a2_provas selos
# Ran 17 tests — OK

for m in import_perguntar chamada_perguntar rodar atomic runtime_error \
         opcao_apply opcao_com_push opcao_limite alarme_and \
         alarme_sem_chave alarme_so_celula alarme_so_mudo rc_zero emite
do
  PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4a2_provas mutante "$m"
done
# rc 1 para cada mutante
```

Ruff local retornou `All checks passed!` nos 13 arquivos, com `check --no-cache` e as regras de [ruff.toml:1](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/ruff.toml:1).

A bateria Django abaixo foi tentada, mas nenhum módulo iniciou: faltou Django no import de [manage.py:11](/home/ronald/wt-codex-fio-e0-v4-e4a-2-215132/app/manage.py:11). Portanto, execução ORM e serialização via `call_command` ficaram **[nao verificado]**.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_selo_emissor_fio_mudo \
  chamados.tests.test_jv4a_elo_encadeado_tem_log \
  chamados.tests.test_jv4a_disparar_vigia_da_celula \
  chamados.tests.test_pergunta_do_dia_em_curso \
  chamados.tests.test_cobparc5_parcial_sem_periodo_so_lista \
  chamados.tests.test_colab_nao_vaza_fio_mudo \
  chamados.tests.test_fabrica_pela_ata \
  chamados.tests.test_fabrica_por_evento \
  chamados.tests.test_selo_disparo_nao_cobra_desligado \
  chamados.tests.test_e4a2_disparar_e_vigia \
  core.tests.test_contract_tabuleiro \
  ponto.tests.test_varredura_lote_prazo \
  --settings=config.settings.ci --verbosity=2
```

O brief integral não foi localizado; aderência a decisões ausentes dele: **[nao verificado]**. Certificação pela suite Code e pelo opus final: **[nao verificado]**. [nao verificado]

## 09/11 21:56 · sessao f21c98f8a5d3 · E4A1-C6-X-PASSADO-SEM-TURNO · sonda · origem=Code

```text
sonda_leitura: sonda1.py [2c1dcdffa761bf36] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora_local 2026-11-09T21:53:44.006061-03:00
DONO aberta universo 2 classes {'desfaz_pelo_julgamento': 2}
  exemplos(pk_pergunta, colab, empresa, dia) []
DONO exportada universo 187 classes {'sem_celula': 1, 'dia_sem_trabalho': 6, 'colab_nao_ativo': 132, 'desfaz_pelo_julgamento': 48}
  exemplos(pk_pergunta, colab, empresa, dia) []
ERROS 0
```

**1.** A saída acima é a fornecida no pedido. A tentativa de executar `python3 -B sonda1.py` falhou por arquivo inexistente; o fonte da sonda não foi disponibilizado na cópia. Portanto, a identidade das funções, da ordem e dos argumentos da sonda está **[nao verificado]**.

No código, a sequência é:

- Resolver `agora` e `hoje`, usando respectivamente `timezone.now()` e `timezone.localdate(agora)` quando ausentes: `app/chamados/services/fabrica_da_celula.py:160–161`.
- Avaliar, por curto-circuito: `cel.data < hoje`, situação do **colaborador da célula** igual a `'ativo'`, e `not dia_sem_trabalho(cel)`: `app/chamados/services/fabrica_da_celula.py:172`.
- Chamar `vencidos_no_instante(cel.colaborador, cel, hoje, agora)`; se retornar `None`, chamar `pauta_cadastro_sem_turno(cel)` e retornar `[]`, antes da coleta e do desfazimento: `app/chamados/services/fabrica_da_celula.py:173–180`.
- Dentro de `vencidos_no_instante`, chamar `marcos_vencidos(colab, cel, cel.data, agora)`; retornar `None` somente quando `venc` e `pend` estão vazios e a data é passada: `app/chamados/services/fabrica_da_celula.py:25–29`.

Há diferença entre os universos do vigia e da coleta do julgamento: o vigia filtra carimbo desfazível, disputa aberta e C6 vivo; a coleta do julgamento restringe também ao colaborador da célula, `data_evento=cel.data` e `conversa=False`, passando antes pelas guardas de `motivos_a_perguntar`. Referências: `app/chamados/services/vigia_fio.py:806–818`; `app/chamados/services/fabrica_da_celula.py:85–98`, `app/chamados/services/fabrica_da_celula.py:121–129`.

O vigia chama `pergunta_viva(p)` sem `hoje` explícito; a porta chamada pelo julgamento usa `pergunta_viva(p, hoje=hoje)`. Referências: `app/chamados/services/vigia_fio.py:813`; `app/chamados/services/relavra_via.py:242`. Os argumentos usados pela sonda estão **[nao verificado]**.

**2.** Sim, o vigia devolve **PKs de `PerguntaDisputa`**, acrescentando `p.pk` às listas por dono: `app/chamados/services/vigia_fio.py:804–818`.

`p.celula` é uma FK armazenada e anulável, separada de `data_evento`; não é uma busca automática pela célula dessa data. O filtro do vigia não exige igualdade entre `p.celula.data` e `p.data_evento`. Logo, esse universo pode admitir vínculo com outro dia ou ausência de vínculo. Referências: `app/chamados/models.py:2414–2425`; `app/chamados/services/vigia_fio.py:806–807`.

Já a coleta do julgamento encontra a pergunta pelo **`data_evento`**, sem exigir que sua FK aponte para a célula julgada: `app/chamados/services/fabrica_da_celula.py:121–129`. Portanto, não há garantia, nesse leitor, de que `p.celula` seja a célula cujo julgamento coletaria a pergunta.

A saída fornecida mostra **1 `sem_celula`, em `exportada`**, e nenhuma ocorrência dessa classe em `aberta`. Sem o fonte da sonda, o significado exato desse rótulo — FK nula ou ausência da célula procurada por outra chave — está **[nao verificado]**.

**3.** Para perguntas que permaneçam vivas pelo C6, os leitores incluem:

- **C6 e propriedades:** o C6 pode retornar vivo apesar do carimbo por resposta, célula acusando, classificação da falta ou ausência de lastro; `.viva` delega ao C6 e `.estado` consulta o C6 antes de tratar o carimbo como resolvido. Referências: `app/chamados/juizes.py:1341–1353`, `app/chamados/juizes.py:1372–1406`; `app/chamados/models.py:3041–3047`, `app/chamados/models.py:3375`.
- **Telas do colaborador:** `perguntas_do_app` filtra pelo C6; `consulta_do_app` separa pendentes e histórico; a API serve essas listas. Referências: `app/chamados/services/pergunta_no_app.py:44–74`; `app/api/views.py:2560–2562`, `app/api/views.py:2589–2601`.
- **Telas administrativas:** o relatório classifica pelas perguntas vivas; o banner do drawer lê `perguntas_vivas_de`. Referências: `app/chamados/relatorios_disputa.py:18–23`; `app/colaboradores/services/drawer.py:77–82`.
- **Contadores:** dashboard e badge contam as pendentes de `consulta_do_app`; a materialização conta `perguntas_vivas_de`; o vigia conta os carimbos vivos por dono e imprime `MATERIALIZADA_VIVA`. Referências: `app/core/views.py:252–253`; `app/api/views_core.py:313–316`; `app/chamados/services/materializacao.py:1313–1314`; `app/chamados/services/vigia_fio.py:847–851`; `app/chamados/management/commands/vigiar_fio_morto.py:198–199`.

A saída fornecida não apresenta a classe `PASSADO_SEM_TURNO_nao_desfaz` nem exemplos dela; a presença de casos concretos dessa classe nessas telas e contadores está **[nao verificado]**.

## 09/11 21:48 · sessao a038309c1c18 · MODO-COMUM-PASSADO-SEM-TURNO · construtor · origem=Code

Alterado somente [chamados/services/fabrica_da_celula.py:13](chamados/services/fabrica_da_celula.py:13): inscrição na docstring, com entradas, saídas e invariantes citando as funções. Nenhum código executável mudou.

RED antes da alteração, no [selo existente](core/tests/test_selo_mapa_contrato.py:41):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest core.tests.test_selo_mapa_contrato.SeloMapaContratoTest.test_MORDE_bolha_nova_sem_contrato_e_vermelha -v
```

```text
AssertionError: Lists differ: ['chamados/services/fabrica_da_celula.py'] != []
FAILED (failures=1)
```

Após a alteração:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -W ignore::ResourceWarning -m unittest core.tests.test_selo_mapa_contrato -v
```

```text
Ran 4 tests
OK
```

[Parser real](core/mapa/contrato.py:180):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from core.mapa import contrato
r = 'chamados/services/fabrica_da_celula.py'
f, t = contrato.sem_contrato()
print('arquivo_nas_faltas:', r in f)
print('campos_faltando:', contrato.faltando(contrato.do_arquivo(r)))
print('sem_contrato:', len(f), 'universo:', t)
assert r not in f
PY
```

```text
arquivo_nas_faltas: False
campos_faltando: []
sem_contrato: 38 universo: 69
```

Ruff no arquivo alterado:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache chamados/services/fabrica_da_celula.py
```

```text
All checks passed!
```

Mutantes dos quatro campos, em memória, exercitando o [mesmo selo](core/tests/test_selo_mapa_contrato.py:41):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -W ignore::ResourceWarning - <<'PY'
import ast, io, unittest
from unittest.mock import patch
from core.mapa import contrato

rel = 'chamados/services/fabrica_da_celula.py'
original = contrato._arvore
for campo in ('bolha', 'entra', 'sai', 'invariante'):
    arvore = original(rel)
    arvore.body[0].value.value = ast.get_docstring(arvore).replace(
        '@' + campo + ' ', '@removido ')
    with patch.object(contrato, '_arvore',
                      side_effect=lambda r: arvore if r == rel else original(r)):
        suite = unittest.defaultTestLoader.loadTestsFromName( [nao verificado]
            'core.tests.test_selo_mapa_contrato.SeloMapaContratoTest.'
            'test_MORDE_bolha_nova_sem_contrato_e_vermelha')
        resultado = unittest.TextTestRunner(stream=io.StringIO()).run(suite) [nao verificado]
    assert len(resultado.failures) == 1 and not resultado.errors
    assert rel in resultado.failures[0][1]
    print('MORTO @' + campo + ': FAILED (failures=1)')
PY
```

```text
MORTO @bolha: FAILED (failures=1)
MORTO @entra: FAILED (failures=1)
MORTO @sai: FAILED (failures=1)
MORTO @invariante: FAILED (failures=1)
```

Suite inteira: [não verificado]. [nao verificado]

## 09/11 21:38 · sessao 638745cb9017 · FIO-E0-V4-E4A-2 · censo · origem=Code

A cópia ainda contém o FIO MUDO escritor, inclusive um dry que executa as portas e depois faz rollback. Retirar apenas `--apply` do cron não transforma o comando em leitor. A lista tem **1 juiz por varredura**; a matriz declara **19/20 verdes**, e esvaziar a lista não altera automaticamente esse numerador. (`app/chamados/management/commands/disparar_perguntas_competencia.py:270-317`; `app/config/crons.py:2032-2048`; `app/core/contratos_estruturais.py:262-270,1053-1064`)

A identidade Git do HEAD, a ancestralidade da E4a-1 e a implantação em produção são **[nao verificado]**. Há evidência local de EXPAND/C11 aplicado, mas o dado fornecido não comprova uma rodada posterior à implantação com **FIO MUDO 0**. (`app/chamados/models.py:3017-3019`; `app/chamados/reconciliador.py:1384-1387`; prova da rodada: **[nao verificado]**)

1. **Ramos do comando, escritas e saída**

O primeiro universo, das células, apenas acumula `deixou`: células passadas da janela, com extensão até anteontem em `--comp-corrente`, excluindo ontem enquanto ainda pertence à janela do vigia de hora. Ele consulta `motivos_que_o_evento_deixou`; não chama o escritor nesse laço. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`)

O **FIO MUDO** é o segundo universo: chamado vivo ligado a célula na janela, sem pergunta viva entre suas resolvedoras. Exclui parcial só lista, ausência de motivo vencido pela ata, colaborador não ativo, `_ja_perguntou` e existência de linha do fato; deduplica por colaborador/dia/motivo. O primeiro motivo elegível de cada passagem entra em `_mudos`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141-175,215-219`)

| Condição | Comportamento |
|---|---|
| `--alarme`, inclusive combinado com `--apply` | Retorna antes do escritor. Sai 2 se houver pendência da célula ou FIO MUDO; se ambos existirem, o diagnóstico emitido é o da célula. (`app/chamados/management/commands/disparar_perguntas_competencia.py:247-260`) |
| Sem alvos FIO MUDO | Não executa `_rodar`. Com `--apply`, ainda alarma a pendência da célula; sem chave, apenas retorna. (`app/chamados/management/commands/disparar_perguntas_competencia.py:261-264`) |
| Com alvos e `--apply` | Executa `_rodar` em `transaction.atomic()`. Silencia push por padrão; `--com-push` dispensa esse silenciamento. Confirma a transação e só depois pode sair 2 pelo vigia da célula. (`app/chamados/management/commands/disparar_perguntas_competencia.py:289-304`) |
| Com alvos, sem `--apply` e sem `--alarme` | Executa o mesmo `_rodar`, com push silenciado, dentro de atomic; imprime deltas e força `RuntimeError('rollback')`. Portanto há chamadas de escrita e execução de suas consequências transacionais, seguidas de rollback. (`app/chamados/management/commands/disparar_perguntas_competencia.py:306-317`) |

A porta chamada é `fabrica_da_celula.perguntar(colab, dia, [motivo], origem='disparo_competencia', origem_carimbo='emissor fio mudo')`. Ela tenta desfazer carimbo sem lastro, cria/agrega disputa e gera perguntas. (`app/chamados/management/commands/disparar_perguntas_competencia.py:270-283`; `app/chamados/services/fabrica_da_celula.py:133-148`)

| Efeito alcançável pela porta | O que grava e por onde |
|---|---|
| Descarimbar pergunta sem lastro | Relê sob trava, exige pergunta viva pelo C6, chama `descarimbar`, zera `materializada_em`, `materializada_por` e `via_resolucao`, registra trilha. Pode reabrir disputa fechada. (`app/chamados/services/relavra_via.py:219-261`; `app/chamados/models.py:3203-3212`) |
| Reabrir disputa | Limpa `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; salva e anexa ocorrência ao chamado-pai. (`app/chamados/models.py:2163-2180`) |
| Agregar disputa existente | Acrescenta `motivos`; atualiza contexto com data, `qtd` e `ultima_em`, pela `_abrir_ou_agregar_disputa`. (`app/chamados/services/disputa_emissao.py:535-565,662-667`) |
| Criar disputa/pai | Abre ou reutiliza chamado `disputa_supervisao`, com contexto, urgência e SLA; pode anexar ocorrência de reuso; cria `DisputaSupervisao` e trilha `disputa_aberta`. (`app/chamados/services/disputa_emissao.py:568-599`) |
| Criar pergunta | `gerar_perguntas_disputa` chama `PerguntaDisputa.abrir`; a inserção grava identidade/dia, célula, disputa, motivo, destino e conteúdo recebido, registra assinaturas projetadas e trilha `pergunta_emitida`. (`app/chamados/services/disputa_emissao.py:444-452`; `app/chamados/models.py:3013-3027`) |
| Reutilizar/costurar pergunta | A porta pode trocar célula/destino, lavrar novamente e salvar; acorda a célula anterior. Pergunta nova órfã de motivo do dia pode passar por `reatar_pergunta_orfa`, com savepoint próprio. (`app/chamados/models.py:2962-2983,2997-3007`; `app/chamados/services/disputa_emissao.py:453-469`) |
| Notificar | Disputa nova ou motivo novo pode enviar push quando não silenciado. (`app/chamados/services/disputa_emissao.py:669-687,1748-1772`) |

O ramo `[LASTRO]` merece ressalva: em estado coerente e estável, uma pergunta materializada pertencente ao fato já tende a barrar `_mudos` antes de chegar ao descarimbo. Não é possível declará-lo absolutamente morto: seleção e escrita são consultas separadas, e seus filtros diferem — identidade usa coluna `colaborador` e instância; descarimbo busca por `disputa__colaborador`, motivo e dia. Ocorrência concreta de concorrência/inconsistência: **[nao verificado]**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:215-219`; `app/chamados/models.py:2753-2755`; `app/chamados/services/fabrica_da_celula.py:142-144`)

Se nenhum ramo escrevesse, o corpo atual ainda oferece estes diagnósticos de leitura; quais serão preservados na versão futura depende da decisão de interface. (`app/chamados/management/commands/disparar_perguntas_competencia.py:220-258`)

| Texto atual | Número/recorte |
|---|---|
| `[EMITE] FIO MUDO: N — chamado vivo, dia ACUSADO pela lei, sem uma pergunta viva` | `len(_mudos)`, antes de `--limite`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:220-222`) |
| `[BARRADO] desligado N … · ja tem pergunta viva N · FIO DECIDIDO N … · PARCIAL NA LISTA DO ADMIN N …` | Quatro contadores de barreiras do laço FIO MUDO. (`app/chamados/management/commands/disparar_perguntas_competencia.py:223-228`) |
| `ALVOS POR EMPRESA E PORTA: empE fio_mudo=N · …` ou `nenhum` | Distribuição de `_mudos`, antes do limite. (`app/chamados/management/commands/disparar_perguntas_competencia.py:229-234`) |
| `janela INI -> FIM` | Janela solicitada/calculada. (`app/chamados/management/commands/disparar_perguntas_competencia.py:239`) |
| `FABRICA-POR-EVENTO: o julgamento deixou N pergunta(s) do caminho da celula sem emitir, em C celula(s) (esperado 0; VIGIA -- este lote nao as escreve)` | Motivos em `deixou` e pares distintos colaborador/dia. (`app/chamados/management/commands/disparar_perguntas_competencia.py:240-242`) |
| `VIGIA: o evento deixou N pergunta(s) por emitir (esperado 0)` | Mesmo `len(deixou)`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:243`) |
| `ALVOS: N perguntas em C colaboradores (fio mudo)` | `falta`, depois de `--limite`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:235,244-245`) |

Os textos ligados à execução do escritor são `[LASTRO] N carimbo(s)…`, `APLICADO perguntas=N disputas=N chamados=N[ (push silenciado)]`, `DRY perguntas=N disputas=N chamados=N` e `(dry-run: nada gravado)`. Os três deltas vêm de contagens globais antes/depois, não de um relatório por alvo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:266-268,284-287,296-317`)

Os leitores e transportadores localizados são:

- **Placar:** raspa a última ocorrência de `^FABRICA-POR-EVENTO: o julgamento deixou \([0-9]*\) .*` em `logs/fio_mudo.log`; exige mtime de hoje, senão publica `?`. Expõe `perguntas_que_o_julgamento_nao_fez=N`, esperado 0, fora do TOTAL. (`bin/placar_code.sh:388-390`)
- **Corrente/cron:** `crons corrente` fornece `log<TAB>cmd gatilho empresas teto args`; `encadeado.sh` passa `ENC_LOG` e redireciona stdout/stderr do elo para o log. Não raspa os contadores para decidir o próximo elo. (`app/core/management/commands/crons.py:37-42`; `bin/encadeado.sh:45-68,99-104`)
- **`logs/fim`:** o runner usa o rc, criando arquivo de fim em 0 **ou 2**; o dependente verifica a existência desse arquivo. O conteúdo numérico do stdout não participa. (`bin/cron_run.sh:11-25`; `bin/encadeado.sh:81-94`)
- **JSONL/saúde:** o runner grava `nome,emp,ini,fim,dur,exit`; `crons_quebrados` trata 0/2 como execução não quebrada, e `pipeline_placar` distingue rc 2 como alarme. Este último deriva esperados apenas de `CRONS`, enquanto disparar está em `ENCADEADOS`. (`bin/cron_run.sh:13`; `app/core/services/crons_quebrados.py:16,24-47`; `app/core/management/commands/pipeline_placar.py:38-49,60-80`; `app/config/crons.py:1521`)
- **Testes:** há parser do prefixo `FABRICA-POR-EVENTO` e asserções sobre FIO MUDO, distribuição por empresa e dry. (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:91-95,152-184`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:195-203,226-232`)
- **ESTADO:** lê `placar_estrutural.RESULTADOS`; o R6 contém uma cópia da frase `contratos_estruturais: 19/20 verdes`. Não é leitura do stdout do disparar nesse caminho. (`bin/gerar_estado.py:60-86,98-104`; `app/core/placar_estrutural.py:433-441`)
- **Hooks:** leitor direto de `[EMITE] FIO MUDO`, `APLICADO` ou `DRY` fora dos testes: **[nao verificado]**. O documento do verificador exige auditar escritas por cron e escritas sem trilha; os hooks de compactação chamam o handoff, que raspa a frase estrutural do R6. (`.claude/agents/verificador.md:17`; `.claude/settings.json:14-30`; `bin/handoff_sessao.sh:185-208`)

2. **Cron, gatilhos, conjuntos e números**

A entrada é `ENC`, com `gatilho='apurar_furos_diarios'`, `args='--comp-corrente --apply'`, `log='fio_mudo.log'`, estágio `cobranca`, teto 900 e dependências explícitas de `processar_cartorio` e `apurar_furos_diarios`. (`app/config/crons.py:1521-1523`)

O dependente direto por gatilho é `reconciliar_perguntas_orfas --alarme`. Depois vêm `lavrar_deslogados`, `lavrar_he_pendente` e os demais elos da corrente. O primeiro lê perguntas órfãs que o evento deveria ter encerrado; não cria a pergunta faltante do FIO MUDO. (`app/config/crons.py:1526-1527,1539-1541,1554-1556`; `app/docs/CRONS.md:39-48`; `app/ponto/management/commands/reconciliar_perguntas_orfas.py:20-36`)

Não existe uma declaração Python `VIGIAS` correspondente ao nome usado no item: os vigias são uma compreensão dentro de `PAPEL_DO_CRON`. O subgrafo gerado, esse sim, chama-se `VIGIAS`. (`app/config/crons.py:2164-2169`; `app/core/management/commands/gerar_diagrama.py:393-401`)

| Registro/conjunto | Hoje | Se a mudança for coerente |
|---|---|---|
| `JUIZES_POR_VARREDURA` | Único membro: disparar; comentário justifica permanência pelo FIO MUDO. | Tupla vazia. (`app/config/crons.py:2032-2048`) |
| `PAPEL_DO_CRON` | Disparar recebe `juiz` da compreensão da tupla. | Precisa receber `vigia` na compreensão correspondente. Só retirar o nome elimina sua declaração de papel. (`app/config/crons.py:2164-2169,2340-2345`) |
| Agenda | Continua em `ENCADEADOS`, com gatilho e dependências próprios. | Reclassificação de papel não exige, por si, sair da corrente. (`app/config/crons.py:1521-1527,1787-1798,2348-2357`) |
| `juizes_por_varredura()` | **1**, derivado de todas as entradas cujo papel é `juiz`. | **0**, após retirada e reclassificação. (`app/config/crons.py:2048,2348-2357`) |
| Diagrama do tabuleiro | Versionado com **40 vigias / 1 juiz** e nó do disparar ligado ao juiz. | A mesma reclassificação isolada produz **41 / 0**, pelo gerador que conta todas as entradas. (`app/docs/ARQUITETURA.mmd:209-219`; `app/core/management/commands/gerar_diagrama.py:242-265`) |

Quem lê ou conta essa classificação:

- `bin/placar_code.sh` publica `juizes_por_varredura=N`; o valor entra no TOTAL. (`bin/placar_code.sh:482-486,555-556`)
- `vigia_loop.escritas_por_cron()` usa por padrão os juízes vivos e consulta sete dias de `LogAuditoria`, ator `sistema:<cron>`; o placar publica uma linha por cron. Retirar o nome também o retira dessa medição padrão. (`app/chamados/services/vigia_loop.py:61-99`; `bin/placar_code.sh:201-209`)
- O selo do tabuleiro exige igualdade entre lista declarada e juízes derivados. (`app/core/tests/test_contract_tabuleiro.py:25-28`)
- O selo de delegação percorre `JUIZES_POR_VARREDURA` e rejeita exceção cujo cron já saiu dela. (`bin/tests/test_papel_prazo_nao_deriva.sh:302-328`)
- A matriz estrutural referencia a lista como exceção da célula chamado/juiz; o selo exige exceções vazias para aceitar verde. (`app/core/contratos_estruturais.py:262-270`; `app/core/tests/test_selo_contratos_estruturais.py:71-96`)

**19/20 → 20/20 não é consequência automática da lista 1 → 0.** `verdes()` conta flags declaradas. A célula chamado/juiz ainda usa `verde=False` e referencia também `PENDENTES_CHAMADO`, já vazio. Esvaziar a lista mantendo a matriz intacta conserva **19/20**; declarar a célula verde e cumprir seus selos permitiria **20/20**. O denominador permanece 20. (`app/core/contratos_estruturais.py:40-42,62-73,262-270,1038-1064`; `app/core/juizes.py:465-466,1355`)

Os leitores desse N/20 são TICKETS e a ferramenta de arquitetura; ESTADO/handoff recebem a cópia do R6, cuja igualdade com a frase do juiz é selada. (`app/core/placar_tickets.py:215-217`; `app/core/arquitetura_leitura.py:55-82`; `bin/gerar_estado.py:73-83`; `bin/handoff_sessao.sh:192-208`; `app/core/tests/test_selo_r6_frase_do_juiz.py:20-25`)

3. **Cobertura dos escritores: mesma porta não prova mesmo fato**

Há outro caminho executável para a fábrica: o consumidor chama `perguntar_por_eventos` depois das retratações/lastro e antes de marcar os eventos processados; relê as células **atuais das datas**, deduplicando datas do lote. Falha da fábrica mantém eventos pendentes com tentativa/erro. O consumidor está declarado a cada minuto; execução implantada: **[nao verificado]**. (`app/chamados/reconciliador.py:1384-1401`; `app/chamados/services/fabrica_da_celula.py:259-273`; `app/config/crons.py:394`)

| Fato/efeito | Cobertura alternativa comprovada na cópia | Limite |
|---|---|---|
| Pergunta faltante de célula elegível | Cartório/evento chama `perguntar_pela_celula`, que usa o mesmo `perguntar` do lote; cobre criação/agregação, perguntas, costura e trilhas descritas acima. (`app/chamados/services/fabrica_da_celula.py:177-190`) | Exige que a data seja alcançada e passe pelas guardas da fábrica. (`app/chamados/services/fabrica_da_celula.py:85-98,259-273`) |
| Carimbo sem lastro | Julgamento coleta carimbadas e chama a mesma porta de descarimbo. (`app/chamados/services/fabrica_da_celula.py:121-129,177-180`) | Esse passe usa `reabrir_fechada=False`; o lote usa o default `True`. Portanto a cobertura de reabertura de disputa fechada não é idêntica. (`app/chamados/services/fabrica_da_celula.py:142-144,180`; `app/chamados/services/relavra_via.py:187,233-239`) |
| Nascimento de chamado de turno | `abrir`/`criar` emitem evento quando pertencem aos módulos de turno e têm célula resolvida. (`app/chamados/models.py:971-980,1093-1099`) | FIO MUDO aceita qualquer chamado vivo com célula; não aplica esse filtro de módulo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`) |
| Retorno de chamado a VIVOS | Signal emite evento antes da guarda de reentrada do cartório. (`app/chamados/signals.py:194-210`) | Emissão é best-effort com savepoint/`engolir`: pode haver retorno sem evento. Nesse caso, não há garantia desse reparo pelo consumidor. (`app/chamados/signals.py:194-207`) |
| Mudança relevante/perda de pergunta | Save relevante acorda células; ressuscitação em lote e reuso também emitem. (`app/chamados/signals.py:241-257`; `app/chamados/models.py:1563-1568,2997-3007`) | Depende das portas/gatilhos executados; estoque já existente não é automaticamente um evento novo. (`app/chamados/management/commands/consumir_eventos_celula.py:28-31`; `app/chamados/reconciliador.py:1278-1310`) |
| Estoque antigo | Existe `consumir_eventos_celula --semear`. (`app/chamados/management/commands/consumir_eventos_celula.py:14-15,28-30`) | É opção explícita; o ramo normal não semeia. (`app/chamados/management/commands/consumir_eventos_celula.py:28-31`) |
| Push opcional do lote | A fábrica do evento sempre silencia push. (`app/chamados/services/fabrica_da_celula.py:188-190`) | Não cobre o modo opcional `--com-push` do disparar. (`app/chamados/management/commands/disparar_perguntas_competencia.py:289-295`) |

A divergência de universo é comprovável: a fábrica barra `dia_sem_trabalho`, futuro e veredito que não acusa, e encaminha limbo entregue para Pauta DP. O FIO MUDO não consulta essas mesmas guardas: trabalha com chamado vivo, ata, teto e seus próprios filtros. Há teste que fixa célula `concorde` com FIO MUDO ainda escrevendo. (`app/chamados/services/fabrica_da_celula.py:85-98,185-190`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141-175,215-219`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:152-159`)

Assim, ficam sem **substituto integral demonstrado**: o reparo por varredura de alvo cujo evento se perdeu, de estoque não alcançado e de alvo aceito pelo FIO MUDO mas recusado pela fábrica. Existência e quantidade desses casos em produção: **[nao verificado]**. (`app/chamados/signals.py:194-207`; `app/chamados/services/fabrica_da_celula.py:85-98,185-190`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141-219`)

O reconciliador pode reatar **pergunta órfã existente**; isso não equivale a criar linha ausente. A criação faltante no consumidor aparece justamente pela nova chamada à fábrica. (`app/chamados/reconciliador.py:1384-1387,1432-1441,1533-1556`)

CADASTRO sem turno e limbo entregue já têm escritores próprios na fábrica; **não são escritas atuais do disparar**. CADASTRO produz pauta por colaborador sob trava; limbo entregue produz pauta por dia/motivo. (`app/chamados/services/fabrica_da_celula.py:172-186,222-255,324-343`; `app/chamados/management/commands/disparar_perguntas_competencia.py:282-283`)

A tranca atual também não substitui emissão faltante: encerra chamado/disputa, lê a morte da pergunta pelo C6 e roda com cartório calado. `trancar_periodo` não contém o bloco C11 do anexo; chama `disparar_no_ato` em best-effort. (`app/chamados/services/competencia_trancada.py:8-16,177-205`; `app/ponto/services/fechamento.py:1400-1429`)

4. **Testes atuais, selos e situação do PARQUE** [nao verificado]

Resultado de execução dos testes nesta auditoria: **[nao verificado]**. Os contratos abaixo foram identificados no fonte. [nao verificado]

| Contrato atual fixado | Teste | [nao verificado]
|---|---|
| Célula com pendência: apply não cria pergunta, contador 1, rc 2 | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:98-103` |
| FIO MUDO: apply cria 1, contador da célula 0, rc 0 | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:152-159` |
| Retratada permite renascer; limbo pode escrever FIO MUDO e terminar rc 2 | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:161-184` |
| Parcial só lista: zero perguntas e rc 0 | `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:186-194` |
| Cron e argv da corrente contêm `--apply` | `app/chamados/tests/test_selo_emissor_fio_mudo.py:215-219`; `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19-27` |
| Dry não deixa pergunta persistida e imprime aviso de rollback | `app/chamados/tests/test_selo_emissor_fio_mudo.py:226-232` |
| Um ciclo `comp_corrente/apply` devolve pergunta viva ao chamado mudo | `app/chamados/tests/test_selo_emissor_fio_mudo.py:273-298` |
| Há exatamente 1 juiz por varredura | `app/ponto/tests/test_varredura_lote_prazo.py:47-62` |
| Hoje não conta no vigia; depois passa a contar; FIO MUDO respeita marco vencido | `app/chamados/tests/test_pergunta_do_dia_em_curso.py:125-136,422-

[truncado em 20000 de 30906 caracteres]

## 09/11 21:34 · sessao a72b1f524879 · - · - · origem=Ronald (tmux)

Resultado da análise estática, somente leitura; execução real do cron, saldo FIO MUDO, banco e logs em produção: [nao verificado].

- **A escrita do disparar é o caminho FIO MUDO.** Seleciona chamados `status_local__in=VIVOS` com célula na janela; não restringe `MODULOS_DE_TURNO`. Exclui chamado com pergunta viva, parcial só lista, colaborador não ativo, `_ja_perguntou`, fato já existente; toma o primeiro motivo pela ata ainda não visto. A seleção termina em `_mudos.append(...)`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `:148`, `:154`, `:158`, `:169`, `:172`, `:215`, `:219`.)
- **O mesmo `_rodar()` escreve com e sem `--apply`.** Com apply, confirma atomic; sem apply, executa a fábrica sob atomic e força `RuntimeError('rollback')`. `--alarme` retorna antes desse trecho. (`app/chamados/management/commands/disparar_perguntas_competencia.py:253`, `:270`, `:282`, `:289`, `:306`.)

**Efeitos da porta compartilhada e cobertura alternativa**

| Efeito chamado por `_rodar` | Escritor/porta real e cobertura |
|---|---|
| Desfazer carimbo sem lastro | `perguntar` busca perguntas materializadas do motivo/dia e chama `desfazer_carimbo_sem_lastro`. Esta relê sob trava, exige C6 vivo, chama `descarimbar`, registra evento e pode reabrir disputa. O cartório/C11 também o faz por `perguntar_pela_celula`. (`app/chamados/services/fabrica_da_celula.py:142`, `:177`; `app/chamados/services/relavra_via.py:219`, `:242`, `:246`, `:248`, `:259`.) |
| Limpar campos do carimbo | `descarimbar` zera `via_resolucao`, `materializada_em`, `materializada_por`; recusa linha fora do fato. O `save` relevante também aciona outbox. (`app/chamados/models.py:3203`, `:3208`, `:3210`; `app/chamados/signals.py:251`.) |
| Reabrir disputa | `reabrir_sistema` limpa `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; grava ocorrência no chamado da disputa. `perguntar_pela_celula`, no passe específico de carimbadas, usa `reabrir_fechada=False`; o `perguntar` usado pelo fio mudo deixa o default `True`. (`app/chamados/models.py:2163`, `:2173`, `:2177`, `:2180`; `app/chamados/services/fabrica_da_celula.py:180`; `app/chamados/services/relavra_via.py:187`.) |
| Criar/agregar disputa e contexto | `perguntar` chama `criar_ou_atualizar_disputa`; esta chama `_abrir_ou_agregar_disputa`, que agrega `motivos`, faz merge do contexto com `qtd`/`ultima_em`, ou abre/reusa chamado-pai e cria disputa. É a mesma porta chamada pelo evento da célula. (`app/chamados/services/fabrica_da_celula.py:145`, `:190`; `app/chamados/services/disputa_emissao.py:535`, `:548`, `:565`, `:568`, `:587`, `:662`.) |
| Notas e trilha de emissão | No reuso do pai anexa ocorrência; no nascimento da disputa registra `disputa_aberta`; no nascimento da pergunta registra `pergunta_emitida`. Logo retirar o cron também retira os textos com origem `disparo_competencia`, embora a emissão por célula registre sua própria origem `julgamento_celula`. (`app/chamados/services/disputa_emissao.py:584`, `:595`; `app/chamados/models.py:3020`; `app/chamados/management/commands/disparar_perguntas_competencia.py:282`; `app/chamados/services/fabrica_da_celula.py:190`.) |
| Criar pergunta do fato | A emissão chama `PerguntaDisputa.abrir`; a porta insere com `gravar_dia_do_fato`, `save(force_insert=True)` e registra atos projetados recebidos. O cartório/C11 atravessa a mesma fábrica. (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/models.py:3013`, `:3017`, `:3019`; `app/chamados/services/fabrica_da_celula.py:145`.) |
| Costurar/reapontar pergunta existente | No reuso da identidade, `_nascer` pode atualizar `chamado_destino` e lavra, salvar e emitir evento da origem anterior. Na emissão nova, se ficar órfã e o motivo for do dia sem fechar, chama `reatar_pergunta_orfa` sob savepoint; falha fica órfã com log. Essa porta pode derivar destino, costurar histórico, lavrar pergunta morta ou emitir/reabrir `batida_ausente`, respeitando gates. (`app/chamados/models.py:2981`, `:2997`, `:3003`, `:3006`; `app/chamados/services/disputa_emissao.py:459`; `app/chamados/reconciliador.py:1533`, `:1556`, `:1619`, `:1642`, `:1655`, `:910`.) |
| Push | Apply com `--com-push` pode enviar push para disputa nova/motivo novo; dry e apply padrão silenciam. O evento da célula sempre silencia. Assim o modo opcional `--com-push` não tem equivalente nesse evento. (`app/chamados/management/commands/disparar_perguntas_competencia.py:289`, `:307`; `app/chamados/services/disputa_emissao.py:669`, `:686`, `:1748`; `app/chamados/services/fabrica_da_celula.py:188`.) |

**O ramo LASTRO parece residual, mas não deve ser chamado de morto absoluto.** Em estado coerente e estável, a pergunta materializada que poderia ser descarimbada já faz `linhas_dos_fatos(...).exists()` barrar o alvo; as linhas fora do fato que passam nesse filtro são inelegíveis ao descarimbo. Contudo seleção e escrita são consultas separadas; o filtro da identidade usa coluna `colaborador` e `instancia_marco`, enquanto a busca do descarimbo usa `disputa__colaborador` e omite instância. Concorrência ou inconsistência concreta: [nao verificado]. (`app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `app/chamados/models.py:2753`; `app/chamados/services/fabrica_da_celula.py:142`; `app/chamados/services/relavra_via.py:181`; `app/chamados/models.py:2510`.)

**C11 está no caminho executável da cópia.** O consumidor chama `perguntar_por_eventos` depois de retratações e lastro e antes de marcar eventos processados; falha da fábrica entra em `falhas` e mantém eventos pendentes com tentativa/erro. A fábrica deduplica datas e relê células atuais do colaborador, portanto não confia no ID antigo do evento. O cron contínuo do consumidor está declarado a cada minuto. (`app/chamados/reconciliador.py:1384`, `:1388`, `:1392`; `app/chamados/services/fabrica_da_celula.py:259`; `app/config/crons.py:394`.)

**Há gatilhos para nascimento, retorno e perda de pergunta, com garantias diferentes.**

- Nascimento por `ChamadoColaborador.abrir`/`criar`: insere evento para `MODULOS_DE_TURNO` com célula resolvida, no ato transacional. (`app/chamados/models.py:971`, `:977`, `:1093`.)
- Retorno do chamado a `VIVOS`: emite antes da guarda `cartorio.em_julgamento()`, porém é **best-effort**; falha do INSERT é engolida sob savepoint e o chamado permanece reaberto. Portanto há caminho permitido de reabertura sem evento que o apply da varredura ainda poderia reparar. (`app/chamados/signals.py:188`, `:194`, `:201`, `:205`, `:210`.)
- Alteração relevante de pergunta: o receiver emite por `CAMPOS_QUE_ACORDAM`; a falha sobe e o `save` é atomic. Ressuscitação em lote e reuso da identidade acordam células explicitamente. (`app/chamados/signals.py:251`; `app/chamados/models.py:3413`, `:1563`, `:2997`.)
- Reapontamento acorda origem, e teste exige captura antes e evento depois do save. (`app/chamados/tests/test_e4_c20.py:117`.)
- Estoque antigo pode ser semeado por `consumir_eventos_celula --semear`, mas isso é opção explícita, não o ramo normal do cron. Semeia chamados vivos de turno e órfãs que o evento encerraria. (`app/chamados/management/commands/consumir_eventos_celula.py:28`; `app/chamados/reconciliador.py:1278`, `:1289`, `:1302`.)

**A cobertura não é o MESMO universo do fio mudo.** O evento passa por `dia_sem_trabalho`, veredito que acusa, data não futura e encaminhamento do limbo entregue à Pauta DP; o fio mudo usa motivos pela ata e seus filtros próprios, sem essas mesmas guardas explícitas. Além disso, seu universo aceita qualquer chamado vivo ligado à célula, enquanto os gatilhos de nascimento/retorno exigem módulo de turno. Retirar apply elimina a varredura reparadora desses casos; se há casos reais: [nao verificado]. (`app/chamados/services/fabrica_da_celula.py:85`, `:87`, `:93`, `:185`; `app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `:158`; `app/chamados/signals.py:201`; `app/chamados/models.py:977`.)

**CADASTRO e limbo entregue já têm outro escritor; não são escritas atuais do disparar.** A fábrica, no passado sem turno, abre pauta de cadastro por colaborador sob trava, com dedup de pauta viva ou fechada cobrindo aquele dia; o limbo entregue abre pauta DP por motivo/dia, deduplicando inclusive histórico feito/cancelado. O comando atual só observa motivos deixados e emite perguntas do fio mudo. (`app/chamados/services/fabrica_da_celula.py:172`, `:288`, `:324`, `:185`, `:222`, `:239`; `app/chamados/management/commands/disparar_perguntas_competencia.py:112`, `:282`.)

**A TRANCA proposta não está no corpo atual.** `trancar_periodo` faz get-or-create/retranca e chama `disparar_no_ato` sob `engolir`; não contém o bloco de emissão C11 por célula do anexo. O serviço atual da tranca encerra chamados/disputas e encaminha respostas ao DP; declara que não escreve nas perguntas e roda com cartório calado. Portanto não substitui a emissão de pergunta faltante pelo fio mudo, nem garante hoje evento para recalcular a fila no ato da tranca. (`app/ponto/services/fechamento.py:1400`, `:1408`, `:1420`; `app/chamados/services/competencia_trancada.py:8`, `:30`, `:177`, `:196`, `:207`.)

**O reconciliador não cria pergunta ausente por conta própria fora da fábrica.** A rotina `reconciliar_chamados_colab` retrata perguntas, reata perguntas órfãs já existentes e retrata premissa morta; criar pergunta perdida agora aparece na chamada à fábrica dentro do consumidor. Reatar uma pergunta existente não cobre um fato que não tem linha de pergunta. (`app/chamados/reconciliador.py:1432`, `:1441`, `:1533`, `:1384`.)

## 09/11 21:34 · sessao bf6e4c8791c8 · - · - · origem=Ronald (tmux)

**Cron, conjuntos e leitores**

- Entrada efetiva da cópia: `ENC('disparar_perguntas_competencia', gatilho='apurar_furos_diarios', args='--comp-corrente --apply', log='fio_mudo.log', estagio='cobranca', teto_s=900, depende=('processar_cartorio', 'apurar_furos_diarios'))` — `app/config/crons.py:1521-1523`.
- Dependente direto como gatilho: `reconciliar_perguntas_orfas --alarme`, log `perguntas_orfas.log`, estágio `reconciliacao`, teto 900 — `app/config/crons.py:1526-1527`. Próximo elo é `lavrar_deslogados --apply`, gatilho `reconciliar_perguntas_orfas`, dependência declarada de `apurar_furos_diarios` — `app/config/crons.py:1539-1541`.
- `ENC` transforma dependência omitida em `(gatilho,)`; portanto reconciliar órfãs depende do disparar também no campo `depende` — `app/config/crons.py:53-54`.
- `ENCADEADOS` continua parte do universo de crons; `todas_as_entradas()` concatena `CRONS + ENCADEADOS` — `app/config/crons.py:1787-1798`.
- `JUIZES_POR_VARREDURA` contém somente disparar; comentário diz que permanece pelo FIO MUDO e que a célula já virou vigia — `app/config/crons.py:2032-2048`, fechamento da tupla em `2098`.
- Não há conjunto Python chamado `VIGIAS` na declaração observada: o grupo de vigias é a compreensão dentro de `PAPEL_DO_CRON`, começando com `reconciliar_grade`; disparar recebe `juiz` pela compreensão anterior — `app/config/crons.py:2164-2169`. `PAPEIS` aceita oito valores — `app/config/crons.py:2004`.
- Mudança correspondente: remover da tupla de juízes e inserir na compreensão de vigias; não há razão estrutural para sair de `ENCADEADOS` ou mudar `gatilho`/`depende` — evidência da independência entre agenda/papel em `app/config/crons.py:2340-2357`. Necessidade de mudar os gatilhos por negócio: **[nao verificado]**.
- Contagem de juízes: `juizes_por_varredura()` filtra `todas_as_entradas()` por `papel(e) == 'juiz'`, não simplesmente `len(JUIZES_POR_VARREDURA)` — `app/config/crons.py:2348-2357`. Hoje 1; após reclassificação coerente, 0 — mesma função e único nome `app/config/crons.py:2048`.
- `bin/placar_code.sh` importa a função e imprime `juizes_por_varredura=N (config/crons.py, papel juiz; esperado 0; dono=Code)` — `bin/placar_code.sh:482-486`. Esse valor entra em `TOTAL`; `?` equivale a 1 — `bin/placar_code.sh:555-556`.
- `escritas_por_cron()` usa, por default, a lista viva de juízes; mede exclusivamente `LogAuditoria` do ator `sistema:<cron>` nos últimos sete dias — `app/chamados/services/vigia_loop.py:61-99`. Tirar disparar da lista também o retira desse universo de medição; não prova ausência de escrita — `app/chamados/services/vigia_loop.py:64-66`, `80-84`.
- O placar publica uma linha por membro dessa medição, formato `escritas_por_cron[<nome>]=N (objetos ..., regravados ..., top3 ...; ... esperado 0 ...)` — `bin/placar_code.sh:201-209`. O verificador pede explicitamente conferir escrita sem trilha separadamente — `.claude/agents/verificador.md:17`.

**Matriz: não há 20/20 automático**

- Célula de chamado/juiz declara duas exceções: `config.crons.JUIZES_POR_VARREDURA` e `core.juizes.PENDENTES_CHAMADO`; não declara `verde=True`, logo usa default False — `app/core/contratos_estruturais.py:62`, `262-270`.
- `PENDENTES['chamado']` já está vazio; alias `PENDENTES_CHAMADO` aponta para ele — `app/core/juizes.py:465-466`, `1355`.
- A matriz tem 18 células com `verde=True`, GLOBAL está verde; portanto 19/20. A frase atual também declara 19/20 — contagem definida em `app/core/contratos_estruturais.py:1053-1064`, GLOBAL em `81-84`, frase em `app/core/placar_estrutural.py:440-441`.
- Esvaziar a lista, sozinho, mantém **19/20**, pois `verdes()` conta flags declaradas; 20/20 exige declarar essa célula verde e cumprir os selos — `app/core/contratos_estruturais.py:1053-1055`; `app/core/tests/test_selo_contratos_estruturais.py:71-96`.
- O denominador é calculado e continua 20, não muda pela remoção do juiz — `app/core/contratos_estruturais.py:1038-1046`.
- Leitores do N/20: TICKETS gera tabela com `ce.verdes()` e `ce.total()` — `app/core/placar_tickets.py:215-217`; ferramenta HAIKU retorna número, teto e células faltantes — `app/core/arquitetura_leitura.py:55-82`.
- ESTADO lê `core/placar_estrutural.py::RESULTADOS`, não stdout do disparar — `bin/gerar_estado.py:60-86`, publicação `388`. R6 contém cópia literal da frase 19/20 — `app/core/placar_estrutural.py:440-441`; selo exige igualdade dessa frase e declaradas com juiz real — `app/core/tests/test_selo_r6_frase_do_juiz.py:20-25`.
- Handoff também lê o dado R6 e raspa `"contratos_estruturais: (\d+)/(\d+) verdes"` — `bin/handoff_sessao.sh:185-208`.
- Afirmação de que hooks leem diretamente stdout/FIO MUDO: **[nao verificado]**. O arquivo `.claude/agents/verificador.md:17` exige auditoria do contador e escritas, mas não é parser da saída.

**Transporte de stdout, rc e carimbos**

- `corrente()` retorna `(cmd, gatilho, empresas, teto_s, args, caminho_do_log(log))` — `app/config/crons.py:1824-1851`.
- `manage.py crons corrente` serializa `log<TAB>cmd gatilho empresas teto args` — `app/core/management/commands/crons.py:37-42`.
- `bin/encadeado.sh` lê a serialização, passa `ENC_LOG`, acompanha rc e guarda pior rc — `bin/encadeado.sh:45-68`.
- Um elo só roda depois da existência dos arquivos `logs/fim/<gatilho>.<empresa>.<dia>`; teto ausente gera rc 2 — `bin/encadeado.sh:74-94`.
- Saída e erro do disparar são redirecionados para seu log declarado pelo envelope — `bin/encadeado.sh:99-104`.
- `cron_run.sh` captura rc, escreve `{nome,emp,ini,fim,dur,exit}` no JSONL e cria carimbo em rc **0 ou 2**; rc 2 significa alarme de negócio, não falha de execução — `bin/cron_run.sh:11-25`.
- Outros rc abrem pauta `cron_quebrado`; 0/2 recuperam pauta anterior — `bin/cron_run.sh:27-38`. Assim reconciliar órfãs continua recebendo gatilho mesmo com alarme 2 do disparar — `app/config/crons.py:1526`; `bin/cron_run.sh:22-25`; `bin/encadeado.sh:89`.
- Parser direto do contador da célula: `sed '^FABRICA-POR-EVENTO: o julgamento deixou \([0-9]*\) .*'` sobre `logs/fio_mudo.log`, última ocorrência; se arquivo não mudou hoje, usa `?` — `bin/placar_code.sh:388-389`.
- Publicação correspondente é `perguntas_que_o_julgamento_nao_fez=N`, esperado 0, fora do TOTAL — `bin/placar_code.sh:390`.
- Parser do número `[EMITE] FIO MUDO:` fora dos testes: **[nao verificado]**. [nao verificado]

**Selos e atualização de conjuntos**

- Tabuleiro exige igualdade entre juízes derivados e tupla declarada — `app/core/tests/test_contract_tabuleiro.py:25-28`. Exige célula de chamado não verde enquanto tupla não vazia — `41-44`.
- Selo do teto exige `total() == 20`, sete famílias, três contratos e exclusão de chamado/escala sem cadastro — `app/core/tests/test_selo_contratos_estruturais.py:33-49`.
- Selos da matriz exigem teste existente e TODAS as exceções vazias para cada verde; exceção cheia proíbe verde — `app/core/tests/test_selo_contratos_estruturais.py:71-96`.
- **Outro arquivo obrigatoriamente impactado**: a exceção de delegação do disparar no selo de host tem motivo/condição de saída explícitos; condição atual é “sai quando o fio mudo for medido e o C11 pousar” — `bin/tests/test_papel_prazo_nao_deriva.sh:302-308`. O selo falha se essa exceção continuar depois que o cron sair de `JUIZES_POR_VARREDURA` — `322-326`.

**Documentação e gerador**

- CRONS não chama literalmente disparar de escritor: mostra seu lugar na corrente — `app/docs/CRONS.md:39-40`; avisa que código é fonte única e documento é consulta — `7-16`. Ausência de outra descrição de escritor: **[nao verificado]**.
- JUIZES não contém ocorrência de disparar na cópia pesquisada: descrição dele como escritor ali é **[nao verificado]**. Documento identifica a fonte viva e função de lookup — `app/docs/JUIZES.md:7-16`.
- CLAUDE não contém ocorrência literal de disparar: descrição nominal dele como escritor é **[nao verificado]**. Regra geral é “emissão, pergunta e chamado nascem do sinal da célula”, cron de varredura é vigia, e juízes são dívida — `CLAUDE.md:166-170`. Fonte dos crons e corrente por arquivo — `236-242`.
- ARQUITETURA mostra dependências do disparar — `app/docs/ARQUITETURA.mmd:105-106`, `131`; declara **40 vigias, 1 juiz** e nó do disparar ligado a juiz — `209-219`.
- Gerador do tabuleiro lê todas as entradas e papel; após reclassificação produziria **41 vigias, 0 juízes**, sem nó `j_disparar...` — `app/core/management/commands/gerar_diagrama.py:233-265`.
- **Descompasso adicional**: `bloco_vigias()` lê apenas `CRONS`, não `ENCADEADOS`; portanto disparar, mesmo reclassificado, não ganharia nó no subgrafo VIGIAS por esse gerador — `app/core/management/commands/gerar_diagrama.py:393-401`. O bloco de tabuleiro conta universo completo, mas este subgrafo exclui encadeados — mesmas linhas e `242-243`.
- A cópia literal R6 envelheceu parcialmente em sua explicação: ainda menciona 24 juízes, apesar de a tupla atual conter 1 — `app/core/placar_estrutural.py:529-534`; `app/config/crons.py:2048`. Não implica erro na frase atual 19/20, cujo selo é separado — `app/core/tests/test_selo_r6_frase_do_juiz.py:20-25`.

**Limites**

- Não houve arquivo AGENTS.md encontrado no inventário da cópia; inexistência fora desse inventário: **[nao verificado]**.
- HEAD real/ancestralidade E4a-1 e operação em produção: **[nao verificado]**; cópia não contém repositório Git acessível no cwd.
- Nenhum dado entregue prova FIO MUDO 0 em produção nem C11 implantado; a condição de saída do selo continua exigindo ambos — `bin/tests/test_papel_prazo_nao_deriva.sh:307`; prova de atendimento: **[nao verificado]**.

## 09/11 21:34 · sessao 7e8ee26731c1 · - · - · origem=Ronald (tmux)

**Testes que fixam a situação atual** [nao verificado]

- A célula é vigia mesmo com `apply=True`: nenhuma pergunta gravada, contador 1, rc 2. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:98-103`.
- FIO MUDO continua escritor: `apply=True`, nasce 1 pergunta, contador da célula 0, rc 0. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:152-159`.
- Pergunta retratada não cala o FIO MUDO: nasce outra pergunta. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:161-175`.
- Limbo pode contar pendência da célula **e** escrever FIO MUDO antes de sair rc 2. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:177-184`.
- Parcial sem período fica na lista do admin, zero perguntas, rc 0. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:186-194`.
- Cron obrigatório com `--apply`: `app/chamados/tests/test_selo_emissor_fio_mudo.py:215-219`; argv encadeado também obrigatório com `--apply`: `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19-27`.
- Dry deixa contagem persistente de perguntas inalterada e imprime “dry-run: nada gravado”. `app/chamados/tests/test_selo_emissor_fio_mudo.py:226-232`. Porém o código executa o escritor em transação e provoca rollback; não é ausência de chamadas de escrita. `app/chamados/management/commands/disparar_perguntas_competencia.py:306-317`.
- O selo do ciclo exige que o chamado mudo tenha pergunta viva depois de `comp_corrente=True, apply=True`. `app/chamados/tests/test_selo_emissor_fio_mudo.py:273-298`.
- Hoje o número de juízes é fixado em **1** pelo teste. `app/ponto/tests/test_varredura_lote_prazo.py:47-62`.
- Exceção de delegação nomeia disparar e saída “fio mudo medido e C11 pousar”; removê-lo da lista sem apagar a exceção torna o script vermelho. `bin/tests/test_papel_prazo_nao_deriva.sh:302-307`, `bin/tests/test_papel_prazo_nao_deriva.sh:322-326`.

**Selos de vigia e estrutural**

- O parser de teste raspa `^FABRICA-POR-EVENTO: o julgamento deixou (\d+) `. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:91-95`.
- Limbo com pendência exige contador 1 e rc 2; folga exige 0 e rc 0; célula já perguntada exige 0 e rc 0. `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:106-124`.
- Hoje fica fora da conta; amanhã entra e exige rc 2 sem escrever. `app/chamados/tests/test_pergunta_do_dia_em_curso.py:125-136`.
- FIO MUDO de hoje só pergunta marco vencido. `app/chamados/tests/test_pergunta_do_dia_em_curso.py:422-438`.
- Teto estrutural é `total()==20`; o selo **não** exige `verdes()==20`. `app/core/tests/test_selo_contratos_estruturais.py:33-49`, `app/core/tests/test_selo_contratos_estruturais.py:209-212`.
- Verde exige todas as exceções vazias; exceção cheia impede verde. `app/core/tests/test_selo_contratos_estruturais.py:81-96`.
- Lista e papéis derivados precisam coincidir; célula “chamado × um juiz por pergunta” não pode ficar verde com `JUIZES_POR_VARREDURA` cheio. `app/core/tests/test_contract_tabuleiro.py:25-28`, `app/core/tests/test_contract_tabuleiro.py:41-44`.

**Comparação com E4A2_PARQUE fornecido**

O texto do anexo não tem numeração de arquivo verificável nesta cópia [nao verificado]. O helper atual tem somente `FONTES_MUTADAS` e `fonte_real`, sem `com_hunk`. `app/chamados/tests/e4_catracas.py:1-9`.

| Item anexado | Situação comprovável |
|---|---|
| `HunkRegistroTest...H4` | Não é um teste atual; a direção proposta contraria os selos atuais que exigem `--apply` no cron e no encadeado. `app/chamados/tests/test_selo_emissor_fio_mudo.py:215-219`; `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19-27`. O teste atual da lista exige 1. `app/ponto/tests/test_varredura_lote_prazo.py:62`. |
| `test_vigia_alarma_todos_e_cadastro_conta_colab_nao_dias` | Não corresponde ao vigia atual: alarme trata apenas `deixou` e FIO MUDO; não inclui CADASTRO nem sua mensagem/contador. `app/chamados/management/commands/disparar_perguntas_competencia.py:237-264`. O contador CADASTRO existe em teste separado da fábrica, com nome/número/esperado/dono. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51-56`. |
| `test_varredura_nao_contem_porta_de_escrita_nem_dry_escritor` | A condição proposta não bate: `_rodar` chama `perguntar`; apply usa atomic e dry também executa `_rodar` em atomic. `app/chamados/management/commands/disparar_perguntas_competencia.py:270-317`. |
| `test_C11_tranca_proposta_2x_eventos_e_erro_antes_dispatch` | Proposta ainda não aplicada: `trancar_periodo` atual não emite eventos nem envolve a operação em atomic; dispatch segue em `engolir`, com intenção best-effort declarada. `app/ponto/services/fechamento.py:1400-1429`. |
| `test_hunk_A05_falha_registrar_reverte_save_e_memoria` | Proposta fora do comportamento atual: `carimbar` salva, mas não chama `DecisaoPergunta.registrar`. `app/chamados/models.py:3128-3157`. |
| `test_A05_efeito_e_hunk_assinatura_conservam_projecao_original` + helper `carimbar` | Comparação via `com_hunk` ficou obsoleta como instrumento; helper não existe hoje. `app/chamados/tests/e4_catracas.py:1-9`. Regra atual ainda preenche resposta/validação conforme argumento, sem ato DP neste corpo. `app/chamados/models.py:3130-3157`. |
| ORM `test_A05_efeito_puro_conserva_RV` | O arquivo atual declara explicitamente A05 fora da E4a-1. `app/chamados/tests/test_e4_expand_orm.py:1-5`. O caso de `resposta=None` condiz com o corpo atual, que só preenche RV quando `resposta` é verdadeiro; execução ORM desse caso [nao verificado]. `app/chamados/models.py:3128-3157`. |
| `HunksTest` com A05/TRANCA/C11/A22/H2 | Harness `com_hunk` obsoleto; os testes atuais leem fontes aplicados via `fonte_real`. `app/chamados/tests/test_e4_expand.py:15-16`. C11 reversão, ressuscitação e reuso têm testes contra código real. `app/chamados/tests/test_e4_expand.py:284-349`. H2 tem teste de atomicidade e falha do evento. `app/chamados/tests/test_e4_expand.py:351-376`. |

**Evidência da E4a-1 na cópia**

- Testes atuais identificam explicitamente E4a-1/P1 (origem do reuso em memória), P2 (relógio propagado) e P3 (pauta CADASTRO única e contador). `app/chamados/tests/test_e4_expand.py:335-349`; `app/chamados/tests/test_e4a_relogio_injetado.py:70-87`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:1-12`.
- O teste do lote diz expressamente que H4 pertence à E4a-2 e que o lote atual somente conta perguntas da célula. `app/chamados/tests/test_fabrica_pela_ata.py:63-68`.
- Confirmação de commit/HEAD git [nao verificado]. Nenhum teste foi executado [nao verificado]. [nao verificado]

## 09/11 21:24 · sessao 5ff88b8dbfe8 · MODO-COMUM-PASSADO-SEM-TURNO · construtor · origem=Code

Arquivos entregues:

- [Selo P3/P4/P6 e casos MORDE](chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:160).
- [Fábrica: alteração somente na docstring](chamados/services/fabrica_da_celula.py:292).
- [Executor dos mutantes, com restauração em `finally`](chamados/tests/mutantes_juiz_pauta_cadastro_sem_turno.py:112).
- [Comandos e saídas completos](chamados/tests/evidencia_modo_comum_passado_sem_turno.txt:7).

Resultado isolado: **14 testes OK**, RED contra a base com **22 falhas**, **23/23 mutantes RC=1** por asserções. [Evidência](chamados/tests/evidencia_modo_comum_passado_sem_turno.txt:765).

**Ruff 0.17.4: `All checks passed!`** [Saída](chamados/tests/evidencia_modo_comum_passado_sem_turno.txt:805).

Suíte Django/ORM: **[nao verificado]**, pois Django está indisponível neste ambiente. A execução isolada não substitui a certificação do Code e do opus final. [Limites](chamados/tests/evidencia_modo_comum_passado_sem_turno.txt:828).

## 09/11 21:15 · sessao d4c533f18057 · MODO-COMUM-PASSADO-SEM-TURNO · verificador · origem=Code

**Há PROCEDENTES: emissão sem previsão, entrada alternativa de pauta, proteção insuficiente da ordem, teste incompleto do registro e explicação incorreta do fechamento.** As evidências e curas mínimas estão abaixo. Correspondência independente da cópia com `49015265a`, ocorrências reais e resultado da suíte no container: **[nao verificado]**.

1. **QUEM ESCREVE / QUEM LÊ — CONFORME nos campos; PROCEDENTE na explicação de “entregue”.**

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| Nova chave/valor de `JUIZES['chamado']` | Declaração em `app/core/juizes.py:293`. | Gerador do MAPA: `bin/gerar_mapa_juizes.py:40`; teste novo: `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`. |
| `PAUTA_SEM_TURNO` / prefixo do texto | Constante em `app/chamados/services/fabrica_da_celula.py:276`; escritor específico incorpora ao texto em `app/chamados/services/fabrica_da_celula.py:342`. | `_pautas_cadastro` interpreta o prefixo; juiz e contador usam esse seletor. `app/chamados/services/fabrica_da_celula.py:285`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:315`. |
| `Pauta.texto`, âncora e destinatário | A fábrica fornece DP, `ANCORA_COLAB` e `str(cel.colaborador_id)`; a porta comum persiste. É **pessoa**, sem vínculo nem dia na chave. `app/chamados/services/fabrica_da_celula.py:339`, `app/chamados/services/fabrica_da_celula.py:340`; `app/pautas/models.py:30`; `app/pautas/services.py:180`. | O seletor exige cabeça, DP, âncora colab e prefixo; o juiz restringe pela pessoa. `app/chamados/services/fabrica_da_celula.py:284`, `app/chamados/services/fabrica_da_celula.py:297`. |
| `feito_em` | `marcar_feita` e `marcar_feita_pelo_sistema`, com instante `timezone.now()`. `app/pautas/services.py:246`, `app/pautas/services.py:247`, `app/pautas/services.py:271`, `app/pautas/services.py:273`. | `vivas()` exclui feitas; o juiz histórico converte o instante para data local. `app/pautas/models.py:57`; `app/chamados/services/fabrica_da_celula.py:301`, `app/chamados/services/fabrica_da_celula.py:304`. |
| `cancelado_em` | `cancelar`, ato do autor, grava instante. `app/pautas/services.py:288`, `app/pautas/services.py:294`. | `vivas()` exclui canceladas; o juiz aceita cancelamento como fechamento e o prioriza sobre `feito_em`. `app/pautas/models.py:64`; `app/chamados/services/fabrica_da_celula.py:303`. |
| `VFM\|CADASTRO_SEM_TURNO` e carimbo JSON | Vigia lê o contador, imprime número/esperado/dono e grava número e esperado no JSON. `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:223`, `app/chamados/management/commands/vigiar_fio_morto.py:229`. | `bin/fio_estado.py:104` lê o JSON e `bin/fio_estado.py:125` compara censo com esperado; `bin/gerar_estado.py:260` incorpora a apresentação. |

Os leitores genéricos transportam esses campos sem decidir “entregue CADASTRO”: aba de pautas, API de mensageria e listagem da pessoa. A listagem da pessoa inclui histórico; a aba padrão usa vivas. São universos declaradamente diferentes, sem segundo significado para a âncora. `app/pautas/services.py:323`, `app/pautas/services.py:433`; `app/pautas/views.py:40`; `app/api/views_mensageria.py:1329`, `app/api/views_mensageria.py:1334`.

**PROCEDENTE — “o DP já olhou” não decorre do predicado.** O juiz aceita cancelamento pelo autor e não consulta `lido_em`; a docstring afirma que o DP olhou. Cura mínima: descrever “já encaminhada e posteriormente fechada/cancelada”, sem prometer leitura ou resolução pelo DP. Alterar a cobertura do cancelamento exigiria decisão normativa específica. `app/chamados/services/fabrica_da_celula.py:292`, `app/chamados/services/fabrica_da_celula.py:301`; `app/pautas/services.py:288`, `app/pautas/services.py:294`.

**CONFORME — instante versus data:** o armazenamento é de instantes; a cobertura usa `localdate(fechou) >= dia`, sendo `dia=cel.data`. A configuração base é São Paulo; fuso ativo efetivo em produção: **[nao verificado]**. `app/chamados/services/fabrica_da_celula.py:304`, `app/chamados/services/fabrica_da_celula.py:337`; `app/config/settings/base.py:93`, `app/config/settings/base.py:95`.

2. **O selo morde — CONFORME para usos diretos; PROCEDENTE na ordem e no registro.**

| Regra | O que pega | O que escapa |
|---|---|---|
| **a. Marcador/literal fora dos dois sítios** | Nome, import com alias, atributo direto e literal completo. Importar o módulo inteiro e acessar diretamente o atributo **é pego**; outra constante com o mesmo literal completo também. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:111`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:115`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:120`. | `getattr`, texto composto por soma, regex sem o literal completo e SQL parametrizado com texto composto não são resolvidos pelo varredor. Uso atual dessas técnicas para esse marcador: **[nao verificado]**. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:120`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:124`. |
| **b. Seletor fora do juiz/contador** | Chamadas diretas e aliases de `from … import`. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:61`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:146`. | Referência intermediária, como `f = modulo._pautas_cadastro; f()`, e reflexão. Uso atual com essa finalidade: **[nao verificado]**. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:47`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:124`. |
| **c. Escritor fora de `perguntar_pela_celula`** | Chamadas diretas ao escritor específico. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:148`. | Mesmas indireções de função; além disso, o **POST humano já existente** pode produzir o mesmo universo sem chamar o escritor específico. `app/pautas/views.py:76`, `app/pautas/views.py:80`, `app/pautas/views.py:81`; `app/pautas/services.py:180`. |
| **d. Trava → juiz → escrever em `atomic`** | Inversões diretas, ausência ou repetição das chamadas e os casos negativos incluídos. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:174`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:257`. | Não valida argumentos, identidade do `atomic` nem interrupções do fluxo entre os passos. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`. |
| **e. Registro** | Presença do endereço entre os valores da família. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`. | Chave errada e segundo registro do mesmo endereço passam. Cura mínima: exigir chave exata e unicidade. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:195`; `app/core/juizes.py:293`. |

**PROCEDENTE — ordem com falsa garantia.** Na execução em memória, os **11 testes novos passaram**, individualmente, com: juiz sem `cel.data`; trava de outro colaborador; `falso.atomic()`; retorno antes do bloco; retorno após a trava; comparação de fechamento invertida; contador fixado em zero. Os pontos permissivos são `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:170`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:216`.

Cura mínima: conferir argumentos da trava/juiz, identidade de `transaction.atomic` e sequência executável. Fechamento e contador precisam da proteção comportamental já existente; esses mutantes **não demonstram** que a suíte inteira passaria. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`.

**PROCEDENTE — brecha alcançável hoje pela entrada humana.** Staff autorizado pode enviar duas vezes marcador + DP + mesma âncora colab: a porta comum cria duas cabeças sem a trava/deduplicação específica, enquanto o contador `distinct` continua mostrando uma pessoa. Isso é segunda origem da pauta específica, usando a porta comum; ocorrência real: **[nao verificado]**. Cura mínima: reservar o prefixo ao fluxo específico na entrada comum, declarar essa validação no contrato do selo e testar a recusa pela entrada humana. `app/pautas/views.py:16`, `app/pautas/views.py:76`; `app/pautas/services.py:153`, `app/pautas/services.py:180`; `app/chamados/services/fabrica_da_celula.py:320`.

3. **MESMA PERGUNTA / SEGUNDO JUIZ — CONFORME no chamador atual.**

A chave registrada corresponde ao predicado efetivamente usado: cabeça DP de cadastro da pessoa, viva ou fechada em data local igual/posterior ao dia consultado. O chamador passa exatamente `(cel.colaborador_id, cel.data)`. A assinatura permite `dia=None`, caso em que responde apenas sobre vivas; essa forma não é a chamada da fábrica. `app/core/juizes.py:293`; `app/chamados/services/fabrica_da_celula.py:288`, `app/chamados/services/fabrica_da_celula.py:297`, `app/chamados/services/fabrica_da_celula.py:299`, `app/chamados/services/fabrica_da_celula.py:304`, `app/chamados/services/fabrica_da_celula.py:337`.

O juiz não verifica sozinho “passado sem turno”; essa pré-condição vem do chamador. Logo, o contrato depende desse caminho, atualmente presente. `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`.

**Nenhum segundo juiz registrado da mesma entrega foi identificado.** `pergunta_viva`, `dia_sem_trabalho`, `tarefa_do_dia` e `dia_da_pergunta_trancado` têm outras perguntas; o contador mede somente pessoas com pendência viva. `app/core/juizes.py:278`, `app/core/juizes.py:290`, `app/core/juizes.py:291`, `app/core/juizes.py:292`; `app/chamados/services/fabrica_da_celula.py:320`.

4. **OBRIGAÇÕES POSITIVAS — cumprimento e proteção.**

| Obrigação | Código da cópia | Selo novo / cura mínima |
|---|---|---|
| **i. Passado sem turno não pergunta ao colab** | **CONFORME na fábrica**: encaminha cadastro e retorna. **CONFORME no fio mudo quanto ao silêncio**: helper devolve vazio e o ramo pula. **PROCEDENTE em `disputa_emissao`**, explicado abaixo. `app/chamados/services/fabrica_da_celula.py:175`, `app/chamados/services/fabrica_da_celula.py:38`; `app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:160`. | O novo selo não testa essa obrigação nos emissores. Falta caso negativo completo para `disputa_emissao` e caso específico do fio mudo; falta também detectar cadastro não entregue quando esses caminhos apenas calam. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`; `app/chamados/services/fabrica_da_celula.py:213`. |
| **ii. Uma pendência por colab** | **CONFORME para escritores cooperantes**: `atomic`, trava da pessoa, seleção e escrita; a trava usa `SELECT FOR UPDATE`. Duas conexões reais: **[nao verificado]**. A entrada humana rompe a garantia global descrita no item 2. `app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:337`; `app/chamados/models.py:2666`. | Testes anteriores cobrem dois dias e repetição sequencial; não equivalem a dois processos concorrentes. Falta ensaio com conexões distintas e proteção dos argumentos no AST. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:51`; `app/chamados/tests/test_e4_c20.py:95`, `app/chamados/tests/test_e4_c20.py:105`. |
| **iii. Nome/número/esperado/dono** | **CONFORME**: nome `cadastro_sem_turno`, número de pessoas distintas com pauta viva, esperado `0`, dono `CADASTRO`. Nome aparece também no rótulo VFM/chave JSON; número/esperado/dono aparecem na linha textual. O JSON não transporta dono. `app/chamados/services/fabrica_da_celula.py:320`, `app/chamados/services/fabrica_da_celula.py:321`; `app/chamados/management/commands/vigiar_fio_morto.py:202`, `app/chamados/management/commands/vigiar_fio_morto.py:229`. | O novo selo exige chamada ao seletor, sem conferir os quatro valores nem o consumo pelo vigia. O contrato dos quatro campos já tem teste anterior. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:217`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`. |
| **iv. Fechada não reabre em laço** | **CONFORME** para dia anterior ou igual à data local do fechamento. Dia posterior permite nova pauta. `app/chamados/services/fabrica_da_celula.py:304`, `app/chamados/services/fabrica_da_celula.py:337`. | Proteção comportamental anterior cobre feita, cancelada, dia coberto e posterior; o AST novo não protege essa semântica. Falta caso na fronteira UTC/data local. `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:67`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:80`, `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:90`. |
| **v. Vigia só alarma, nunca julga** | **CONFORME no acompanhamento CADASTRO**: lê, imprime e carimba; esse contador deliberadamente não dispara alarme operacional. O modo `--alarme` de disparar retorna antes da escrita. `app/chamados/management/commands/vigiar_fio_morto.py:179`, `app/chamados/management/commands/vigiar_fio_morto.py:202`; `app/chamados/management/commands/disparar_perguntas_competencia.py:253`. | O novo selo não proíbe julgamento/escrita no vigia; exigir que seu arquivo seja varrido não protege esse papel. O fio mudo escritor está declarado separadamente como exceção de prazo. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:206`; `app/config/crons.py:2044`. |

**PROCEDENTE — caminho de emissão sem previsão.** `_tem_base` aceita batidas; `classificar_falta` pode manter `esperada=[]` e devolver motivo por batida unilateral; `_ata_do_dia` permite continuar sem célula/escala; o agrupador chama a abertura da pergunta. Portanto existe caminho de código que contraria a proibição fornecida, sem demonstrar ocorrência em produção. `app/chamados/services/disputa_emissao.py:814`; `app/escala/servico_jornada.py:396`, `app/escala/servico_jornada.py:444`, `app/escala/servico_jornada.py:446`; `app/chamados/services/disputa_emissao.py:906`, `app/chamados/services/disputa_emissao.py:1234`, `app/chamados/services/disputa_emissao.py:1241`.

Cura mínima: fazer o classificador canônico respeitar a ausência de previsão e encaminhar a pendência pelo fluxo de evento/porta autorizado, com teste do emissor completo. A fila existente chama esse caso de “falta lei”, mas a exceção “sem turno **com batida**” não está no aval fornecido: autorização dessa exceção **[nao verificado]**. `app/docs/BACKLOG.md:1876`; `app/docs/PROMPTS.md:2004`.

Quanto ao **dia posterior ao fechamento**: a implementação e os testes adotam uma nova pendência viva para uma nova ocorrência. Isso é compatível com “uma pendência viva por colab”; autorização literal específica dessa política no aval raspado: **[nao verificado]**. `app/chamados/services/fabrica_da_celula.py:295`; `app/chamados/tests/test_e4a_pauta_cadastro_contador.py:58`.

5. **CLASSES QUE JÁ REPROVARAM OBRA.**

| Classe | Resultado |
|---|---|
| Segundo juiz que duplica registrado | **NÃO identificado para entrega CADASTRO.** `app/core/juizes.py:293`; `app/chamados/services/fabrica_da_celula.py:288`. |
| Segundo escritor fora da porta | **NÃO demonstrado como escritor ORM externo; SIM, segunda origem que contorna a deduplicação específica**, pelo POST. `app/pautas/views.py:76`; `app/pautas/services.py:180`. |
| Relógio fora da borda | **NÃO no novo juiz**: converte instante persistido. **SIM, preexistente no classificador do emissor**, que lê agora internamente. `app/chamados/services/fabrica_da_celula.py:304`; `app/escala/servico_jornada.py:368`. |
| Derivação paralela de fonte única | **SIM para a política de emissão**: a fábrica cala sem previsão; o classificador ainda acusa unilateral sem sequência prevista. Não é segundo juiz de entrega. `app/chamados/services/fabrica_da_celula.py:38`; `app/escala/servico_jornada.py:396`, `app/escala/servico_jornada.py:444`. |
| Selo/RED sem mordida | **SIM, parcialmente**: ordem e registro aceitam os mutantes descritos; usos diretos têm mordida. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:224`. |
| Ordem de trava | **NÃO no escritor atual; SIM na proteção incompleta do selo.** `app/chamados/services/fabrica_da_celula.py:334`, `app/chamados/services/fabrica_da_celula.py:337`; `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`. |
| Pouso antes do pré-requisito | **[nao verificado]**: cópia e relato não certificam execução/deploy; a exigência está em `CLAUDE.md:374`. | [nao verificado]
| Campo com dois significados | **NÃO identificado nos tipos pessoa/data/instante; SIM na explicação de “entregue = DP olhou”.** `app/chamados/services/fabrica_da_celula.py:292`; `app/pautas/models.py:94`. |

6. **VARREDURA DO FILESYSTEM — pode ler não versionado ou falhar; container efetivo [nao verificado].**

O selo percorre o filesystem, sem inventário Git, incluindo `static`, `staticfiles`, `media` e `logs`. Na montagem da cópia, `staticfiles` vem do host e é ignorado pelo Git; logs/media usam tmpfs. Portanto um `.py` não versionado nesses locais pode entrar na certificação. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:26`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:37`; `.gitignore:12`, `.gitignore:13`; `bin/arvore_do_push.sh:55`, `bin/arvore_do_push.sh:81`.

Leitura sem UTF-8, arquivo ilegível ou Python inválido pode interromper o teste; diretório inacessível pode ser omitido pelo `os.walk` sem `onerror`. Não há tratamento nesses pontos. Falha efetiva no container: **[nao verificado]**. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:37`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:132`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:204`.

A medição local em memória varreu **760 arquivos em aproximadamente 1,63 s**; os 11 testes levaram aproximadamente **1,67 s**. Esse custo local é pequeno; tempo no container e crescimento dos diretórios: **[nao verificado]**. O percurso também enumera arquivos que não serão analisados como Python. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:37`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`.

**PROCEDENTES finais:**

- Emissão sem previsão ainda alcançável em `disputa_emissao`; corrigir o predicado canônico e proteger o caminho completo. `app/escala/servico_jornada.py:444`; `app/chamados/services/disputa_emissao.py:1241`.
- Entrada humana produz o marcador sem deduplicação específica; reservar essa espécie ao fluxo autorizado. `app/pautas/views.py:76`; `app/pautas/services.py:180`.
- Selo de ordem aceita trava/juiz/atomic incorretos e fluxo interrompido; validar identidade, argumentos e sequência executável. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:179`, `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:182`.
- Teste de registro aceita pergunta errada e duplicação; exigir chave exata e unicidade. `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:197`.
- Explicação “DP já olhou” excede o predicado; corrigir a documentação do fechamento/cancelamento. `app/chamados/services/fabrica_da_celula.py:292`; `app/pautas/services.py:288`.

## 09/11 21:13 · sessao d6f821d9f1e7 · - · - · origem=Ronald (tmux)

**PROCEDENTE — ordem selada apenas pela forma.** Em memória, os 11 testes novos ficaram verdes para cada mutante: juiz sem `cel.data`, trava de outro colaborador, `falso.atomic()`, retorno antes do bloco, retorno após a trava, fechamento `>=` trocado por `<=` e contador fixado em zero. `_ordem_ok` não confere argumentos ou identidade das funções e ignora instruções intermediárias que interrompem o fluxo. Cura mínima: conferir argumentos, vínculo ao `transaction.atomic` e sequência executável; proteger fechamento/contador com testes semânticos. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:170`, `:179`, `:182`, `:216`; `app/chamados/services/fabrica_da_celula.py:304`, `:320`, `:335`, `:337`)

**PROCEDENTE — registro incompleto.** Chave errada mantendo o valor e segundo registro com o mesmo valor passam no teste de registro. Cura mínima: exigir a chave exata e unicidade do endereço nessa família. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:195`; `app/core/juizes.py:293`)

**CONFORME, com limites — o selo morde usos diretos.** Nome/import alias/atributo direto/literal completo são detectados; importar módulo e chamar seu atributo também é detectado, assim como outra constante contendo o mesmo literal. Entretanto, `getattr`, referência intermediária `f = modulo._pautas_cadastro; f()`, equivalente do escritor, `Q` com prefixo composto por `+`, regex sem literal completo e SQL com parâmetro composto escapam. Esses exemplos são mutantes hipotéticos, sem ocorrência encontrada no código atual. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:47`, `:61`, `:111`, `:120`, `:124`, `:138`)

**Caminho atual alcançável — entrada humana pode produzir o marcador.** POST de staff aceita texto, destinatário e âncora arbitrários e chama a porta genérica sem deduplicação específica/trava por colaborador. Entrada adequada pode criar outra pauta CADASTRO sem passar por `pauta_cadastro_sem_turno`; ocorrência real é `[nao verificado]`. Isso continua usando a porta genérica de Pauta, portanto não comprova escritor ORM fora dela. (`app/config/urls.py:36`; `app/pautas/urls.py:9`; `app/pautas/views.py:16`, `:72`, `:76`; `app/pautas/services.py:153`, `:180`)

**Varredura — pode incluir arquivo não versionado.** Percorre filesystem, sem consultar Git; `staticfiles` do host é montado no container como diretório real, embora ignorado no Git. Logs/media ficam em tmpfs. `.py` inválido, ilegível ou sem UTF-8 pode falhar em leitura/parse; diretório inacessível pode ser omitido pelo `os.walk` sem `onerror`. Falha efetiva nesse container: `[nao verificado]`. Cura mínima: universo de fontes versionadas/materializado para o selo, distinguindo arquivos gerados. (`app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:26`, `:36`, `:132`, `:204`; `.gitignore:13`; `bin/arvore_do_push.sh:55`, `:81`)

**Custo local:** varredura de 760 arquivos sem erros/violações em aproximadamente 1,63 s; os 11 testes em aproximadamente 1,67 s, usando `python3 -B`, AST e `unittest.TestCase` em memória. Tempo/resultado no container Django: `[nao verificado]`. O método que determina esse custo está em `app/chamados/tests/test_juiz_pauta_cadastro_sem_turno.py:199`.

**Limite importante:** os mutantes passaram apenas pelo selo novo. Testes funcionais anteriores já cobrem contador, fechamento e dia posterior; não se pode concluir que a suíte inteira os aceitaria. (`app/chamados/tests/test_e4a_pauta_cadastro_contador.py:55`, `:61`, `:76`, `:84`, `:88`)

_3251 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
