# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 06:17** · publicado em **27/11/2026 06:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 06:17 · sessao 25a38f481b30 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

1. **Sem decisão.** Pela RAMO2-PARTIDO-2611-1812 (a) e pela L-097(b), o Partido 19:00–07:00 tem vão de 00:00 a 03:00 e o segundo par termina efetivamente no marco de 07:00. São **240 min trabalhados**, **30 min fora da janela** e **240 min noturnos** de 03:00 a 07:00 com a prorrogação da fixture. A HE do dia é `300 + 240 − 540 = 0`. A conta coincide com a saída registrada no anexo `tqc.log`. `docs/LEIS.md:115`, `ponto/motor_calculo_v2.py:3106`, [ponto/tests/test_ramo2b_turno_que_cruza.py:141](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_turno_que_cruza.py:141)

2. **Autorizado.** A conta concorda com o teste. A janela mantém a saída às 07:30, com 270 min trabalhados, 270 noturnos e zero fora da janela. `300 + 270 − 540 = 30` min de HE50 no último par, com HE100 zero. `ponto/motor_calculo_v2.py:1906`, `ponto/motor_calculo_v2.py:3402`, [ponto/tests/test_ramo2b_turno_que_cruza.py:154](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_turno_que_cruza.py:154)

3. **`acusa=False`.** `_TPART` fixa o marco em 07:00 na linha 60. O cadastro da empresa da fixture fixa `teto_saida` em 60 min na linha 179. A regra acusa acima do teto. Como `30 < 60`, a saída fica abaixo dele. `ponto/tests/test_ramo2b_turno_que_cruza.py:60`, `ponto/tests/test_ramo2b_turno_que_cruza.py:179`, `ponto/janela_he.py:200`

### Diff

```diff
--- a/app/ponto/tests/test_ramo2b_turno_que_cruza.py
+++ b/app/ponto/tests/test_ramo2b_turno_que_cruza.py
@@ -141,8 +141,8 @@
-    # TURNO PARTIDO que cruza (19-07, vao 00-03 > 2 h = partido de verdade): par 03:00 -> 07:30 de D+1, 30 min depois
-    # do marco 07:00. Conta 270; noturno com prorrogacao (jornada comum, Sumula 60 II): 03:00 -> 07:30 = 270. HE do
-    # DIA pela base recortada: 300 + 240 = 540 contra 540 -> 0.
+    # RAMO2-PARTIDO-2611-1812 (a) e L-097(b): no Partido 19-07 (vao 00-03), sem decisao, o par 03:00 -> 07:30
+    # de D+1 recorta no marco 07:00: 240 trabalhados, 30 fora da janela e 240 noturnos (03:00 -> 07:00 com
+    # prorrogacao). HE do dia: 300 + 240 = 540 contra a jornada de 540 -> 0.
     'P_CRUZA_PARTIDO_SEM_DECISAO': (
         _TPART, _cels(_TPART, _D),
         [(_D, '19:00:00', 'E'), (_D1, '00:00:00', 'S'), (_D1, '03:00:00', 'E'), (_D1, '07:30:00', 'S')], _D, None,
         '2026-11-06 03:00',
-        ('2026-11-06 03:00', '2026-11-06 07:30', 270.0, 0.0, 270.0, 0.0, 0.0), False),
+        ('2026-11-06 03:00', '2026-11-06 07:00', 240.0, 30.0, 240.0, 0.0, 0.0), False),
```

### RED e mutantes

Reproduzi em memória a tupla do anexo pelo `test_RED_partido_cruza_sem_decisao`, substituindo `_roda` pela observação registrada. A expectativa corrigida passou. A tupla antiga e os quatro mutantes individuais falharam na comparação da tupla. Suite Django após o diff e certificação do Code e do Opus final [nao verificado]. `ponto/tests/test_ramo2b_turno_que_cruza.py:230`, `ponto/tests/test_ramo2b_turno_que_cruza.py:279`

```bash
tqc_replay() {
  PYTHONDONTWRITEBYTECODE=1 python3 -S - "$1" <<'PY'
import ast
import sys
import unittest
from datetime import datetime
from pathlib import Path
from types import SimpleNamespace

nome = 'P_CRUZA_PARTIDO_SEM_DECISAO'
modo = sys.argv[1]
arquivo = Path('ponto/tests/test_ramo2b_turno_que_cruza.py')
arvore = ast.parse(arquivo.read_text())
arvore.body = [n for n in arvore.body if not isinstance(n, ast.ImportFrom)]
arvore.body[-1].decorator_list = []
escopo = dict(TestCase=unittest.TestCase, timezone=SimpleNamespace(localtime=lambda dt: dt))
exec(compile(arvore, str(arquivo), 'exec'), escopo)

casos = escopo['CASOS']
caso = casos[nome]
esperado = list(caso[-2])
antigos = {1: '2026-11-06 07:30', 2: 270.0, 3: 0.0, 4: 270.0}
for i in antigos if modo == 'red' else [] if modo == 'controle' else [int(modo)]:
    esperado[i] = antigos[i]
casos[nome] = (*caso[:-2], tuple(esperado), caso[-1])

e, s, batida = map(datetime.fromisoformat,
                   ('2026-11-06 03:00', '2026-11-06 07:00', '2026-11-06 07:30'))
par = SimpleNamespace(entrada=e, saida=batida, efetivo_entrada=e, efetivo_saida=s,
                      minutos_trabalhados=240, minutos_fora_janela=30, minutos_noturnos_relogio=240,
                      minutos_extra_50=0, minutos_extra_100=0)
motor = SimpleNamespace(_dia_turno_juiz_calc={e: '2026-11-05'}, _cadastro_x_realidade=[])
teste = escopo['Ramo2bTurnoQueCruzaTest']('test_RED_partido_cruza_sem_decisao') [nao verificado]
teste._roda = lambda nome: (motor, None, [par]) [nao verificado]
resultado = teste.run() [nao verificado]
assert not resultado.errors, resultado.errors
assert resultado.wasSuccessful() == (modo == 'controle'), resultado.failures
for _, falha in resultado.failures:
    assert 'Tuples differ' in falha and nome in falha, falha
print(modo, 'PASSOU' if resultado.wasSuccessful() else 'REJEITADO na comparação da tupla') [nao verificado]
PY
}

tqc_replay controle  # expectativa corrigida
tqc_replay red       # tupla antiga
tqc_replay 1         # saída efetiva 07:30
tqc_replay 2         # trabalhado 270
tqc_replay 3         # fora da janela 0
tqc_replay 4         # noturno 270
```

## 27/11 06:11 · sessao 4f8a76333721 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

## Aval e main

**A leitura da c2p é defensável, mas o texto permite outra leitura.** A opção (a) responde a uma parada sobre **pausa interna**. A expressão “motor do Partido intocado”, junto do recorte de pouso para 12x36 e Comercial, sustenta preservar também o tratamento anterior da cauda. A leitura mais estreita adia apenas a grade de pausa interna e mantém a emenda geral da L-097(b), que diz que “nenhum par perde trabalhado”. A opção (a) delimita o pouso, mas deixa implícita a consequência para `minutos_fora_janela` no Partido. [`PROMPTS.md:3087`](/home/ronald/codex_ws/c19bf0db27a334753ab8fe9cf17fad0b5c9b585e/app/docs/PROMPTS.md:3087) · [`LEIS.md:115`](/home/ronald/codex_ws/c19bf0db27a334753ab8fe9cf17fad0b5c9b585e/app/docs/LEIS.md:115)

**Na cópia, o Partido põe a cauda não autorizada em `minutos_fora_janela` quando a janela se aplica.** O `False` da classe leva ao retorno `(marco, fora, marco)`, e o calculador grava `fora`. `Autorizado` passa antes desse ramo e mantém a saída batida. O comentário e dois controles descrevem o recorte como comportamento do main. Para **`59600163c` especificamente**, a cópia veio sem histórico Git e o anexo traz apenas o delta de testes. A confirmação direta desse commit fica **[nao verificado]**. `motor_calculo_v2.py:1900–1920,3106,3131–3146` · `test_ramo2b_c2h_saida.py:147–166` · `test_ramo2b_c2l_red_motor.py:51–61`

## 1. Contas

| Caso | Conta refeita |
|---|---|
| **Partido `SEMANA_FLOAT`** | Segunda, 20:07:19 → 05:00:00 = `31961/60 = 532,683333… min`. Até a batida de 05:36:39 são `34160/60 = 569,333333… min`. Cauda = `2199/60 = 36,65 min`. Na cópia, **trabalhado = base = 532,683333…**, com **36,65 fora**. As bases de segunda a sexta são `31961`, `32277`, `31873`, `32374` e `32915` segundos. Somam `2690 min`, exatamente `2640 + 5 × 10`. **HE semanal zero**, sem alerta. `test_ramo2b_c2_base_da_he.py:79–97,213–236` · `motor_calculo_v2.py:3193–3195,3410–3439` |
| **Partido 6x1 `legais`** | P1 = `221 min`. P2 até 04:00 = `180 min`, todos noturnos. De 04:00 a 05:00 ficam `60 min` fora. Base de relógio = `401`. Hora reduzida = `180 × 8/7 = 1440/7`, com ganho de `180/7`. Previsto = `600 − 180 = 420`. Vão gozado = `180`, suprimido = `0`, limite = `min(420, 480) + 0 = 420`. Excedente = `47/7 = 6,714285… min`, dentro dos 10. **HE zero**. Incluir apenas o ganho noturno da cauda elevaria o excedente a `107/7 = 15,285714… min`, integralmente em HE50. A exceção da cl. 38-d é do 12x36. `test_ramo2b_c2_base_da_he.py:64–65,100–104,378–403` · `motor_calculo_v2.py:44–51,1993–2011,3237–3252,3375–3404` · `LEIS.md:84` · `regua_cct.py:467–472` |
| **12x36 e Comercial** | 19:07:19 → 07:36:39 = `44960/60 = 749,3333333333334 min` trabalhados. Até 07:00 são `42761/60 = 712,6833333333333 min` de base. Os `36,65 min` da cauda entram no trabalhado e **fora = 0**. Subtrair em ponto flutuante dá `712,6833333333334`, um ULP acima da medição direta. O teste acerta ao exigir igualdade exata. `test_ramo2b_c2_base_da_he.py:437–463` · `motor_calculo_v2.py:1918–1920,2985–3028,3473–3517` |

## 2. Caminho do novo teste [nao verificado]

O teste chama os `calcular_periodo` reais e percorre o recorte, a medição e a gravação da base. O patch entrega pronta a parte que o mensal precisa resolver: pareamento e intervalos, dia do turno, marcos do vínculo, folga, vigência, L-084, extra declarada e decisão do admin. Um marco de D+1 errado na janela real ficaria invisível nesse teste. `test_ramo2b_c2_base_da_he.py:443–463` · `fechamento.py:297–311` · `motor_calculo_v2.py:1682–1722,1742–1810,2500–2511,2560–2568,2661–2675` · `janela_he.py:41–61` [nao verificado]

Há duas diferenças relevantes. O 12x36 com horário cadastrado usa `Motor12x36ComEscala`, que chama o mesmo cálculo e depois relê a entrada efetiva para pontualidade. O Comercial mensal zera a HE feita por par e a apura de novo por dia, usando o previsto do dia da jornada quando cruza. O teste também injeta `660` sem fornecer o intervalo descontado do previsto. Ele prova os três números da asserção. A mesma igualdade bit a bit pelo mensal fica **[nao verificado]**. Os dois testes reescritos do Partido já chamam `calcular_mes`. `motor_calculo_v2.py:1464–1492,3580–3602,3672–3685,3810–3827,3927–3928` · `test_ramo2b_c2_base_da_he.py:160–176,455–463` [nao verificado]

## 3. Escritores e leitores

| Sítio | O que faz |
|---|---|
| **Motor** | Os três `calcular_periodo` gravam trabalhado e fora. A tripla da base é gravada por `_escrever_base_da_he`. No Partido recortado, a saída efetiva já é o marco, a tripla fica vazia e `_trabalhado_da_base_da_he` devolve o trabalhado. No 12x36 e Comercial com cauda incorporada, o leitor devolve a base explícita. O Comercial mensal usa a base para HE e o trabalhado integral para banco. O crédito do **dia aberto** e o bloqueio de HE100 também ajustam os campos pela regra do motor. `motor_calculo_v2.py:286–299,1628–1635,1941–1971,2768–2786,3025–3028,3141–3146,3193–3195,3382–3393,3514–3519,3591–3594` |
| **`dia_pago.py`** | Soma trabalhado, noturno e HE já calculados, pelo dia do mapa quando fornecido. A cauda excluída do trabalhado do Partido fica fora dessa soma. `dia_pago.py:126–132,173–186` |
| **`periodo.py::reler`** | Mede e **reescreve** trabalhado pela geometria efetiva. Lê a tripla para HE e dobra, e soma trabalhado com fora para o teto de pontualidade. No caminho de par duplicado, zera também `minutos_fora_janela`. A geometria efetiva do Partido já chega cortada. `periodo.py:182–187,344–366,743–769,807–813,843–859` |
| **`folha/porta_export.py`** | Compara o trabalhado com a lavratura e lê a base pela mesma função do motor. A pendência vem do espelho. O teto de conferência quando há HE **e** intra usa base de relógio e previsto líquido. Ele não reproduz o ganho noturno nem `min(previsto, 480)` do Partido `legais`. `porta_export.py:106–122,181–201,342–365` · `motor_calculo_v2.py:3382–3404` |
| **`credito_parcial.py`** | Fornece ausências parciais e créditos. Dos períodos, lê atraso e agrupa pela data civil da entrada, que pode divergir do dia da jornada. O teto do motor usa `trabalhado + fora`, então a cauda do Partido continua contando como presença para a L-093. `credito_parcial.py:35–50,62–102` · `motor_calculo_v2.py:1319–1321,1356–1357` |

**Há um leitor que usa a ponta errada no Partido.** Quando a tripla está vazia, `_aplicar_he_noturna` usa a saída **bruta**. Rodei essa função em memória com 01:00 → 05:30, saída efetiva às 04:00, HE50 de `180/7` e prorrogação desligada. Com a tripla vazia, saiu **0 min de HE50 noturna**. Com o fim da base às 04:00, saiu **25,7142857 min**. O teste `PARTIDO_LEGAIS` espera HE zero, então não alcança essa divergência. Impacto em produção **[nao verificado]**. `motor_calculo_v2.py:464–485,1094–1109,1967–1971,3131–3146` · `test_ramo2b_c2_base_da_he.py:396–399`

## 4. Teste que contradiz [nao verificado]

**`test_RED_partido_cruza_sem_decisao` ainda exige a cauda dentro.** Para P2 de 03:00 a 07:30, com marco às 07:00, exige saída efetiva **07:30**, **270 trabalhados**, **0 fora** e **270 noturnos**. Pelo ramo da cópia, a expectativa é **07:00**, **240 trabalhados**, **30 fora** e **240 noturnos** com a prorrogação dessa fixture. O vizinho `Autorizado` continua coerente ao exigir os 270. O caso dos 11 minutos também é controle distinto, pois o juiz põe o par em D+1, fora da janela do turno anterior. A execução da suíte fica **[nao verificado]**. [`test_ramo2b_turno_que_cruza.py:141`](/home/ronald/codex_ws/c19bf0db27a334753ab8fe9cf17fad0b5c9b585e/app/ponto/tests/test_ramo2b_turno_que_cruza.py:141) · `test_ramo2b_turno_que_cruza.py:227–230,279–283` · `test_ramo2b_c2_base_da_he.py:425–434` · `motor_calculo_v2.py:1900–1920,3106,3141–3146`

## 5. Checklist

- **Segundo juiz e derivação paralela.** O delta só muda testes. Na cópia, `reler` rederiva o trabalhado e a versão oráculo do fechamento mede a base novamente pelos segmentos efetivos. O patch do novo teste fornece a resposta da janela. `test_ramo2b_c2_base_da_he.py:437–463` · `periodo.py:753–769` · `alimentacao.py:168–178,469–474,545–548` · `fechamento.py:1289–1305` [nao verificado]
- **Segundo escritor.** A varredura AST executada em leitura encontrou só `_escrever_base_da_he` para a tripla. Para trabalhado e fora há as reescritas do motor e de `reler` descritas acima. `test_ramo2b_c2_base_da_he.py:644–677` · `motor_calculo_v2.py:1954–1958` · `periodo.py:743–767`
- **Relógio solto.** Os testes mensais do Partido têm `freeze_time`. O teste direto usa instantes fixos, janela injetada e feriados vazios. `test_ramo2b_c2_base_da_he.py:124–125,437–458` [nao verificado]
- **Selo com mordida.** O anexo mostra **5/5 mutantes mortos**, com oito falhas de asserção. O resultado da suíte limpa e o efeito nas competências 11/12 e no TXT ficam **[nao verificado]**. `mut4.log:6,29,51,61,75,84,93,104` [nao verificado]
- **Campo com dois significados.** `minutos_trabalhados` é o contado após a janela. A presença para o teto soma trabalhado e fora. Uma tripla `None` significa que a base numérica coincide com o trabalhado, inclusive quando houve cauda física recortada. O comentário “par sem cauda” é impreciso nesse caso e a leitura da saída bruta no split noturno mostra a consequência. `motor_calculo_v2.py:228–246,286–290,1096–1105,1319–1321,1963–1969`

## Veredito — REPROVADO

1. O selo mensal legado do Partido sem decisão exige o oposto da c2p para a mesma regra de saída. `test_ramo2b_turno_que_cruza.py:144–148,279–280` · `test_ramo2b_c2_base_da_he.py:222–236`
2. O split de HE noturna do Partido pode usar a cauda excluída para posicionar HE devida antes do marco. A c2p cobre o ganho na base, mas o caso com HE positiva segue sem selo nessa volta. `motor_calculo_v2.py:1094–1109,1967–1971` · `test_ramo2b_c2_base_da_he.py:378–403`
3. A c2p apresenta o destino da cauda como conclusão direta do aval. O texto sustenta preservar o Partido, mas a equivalência com o comportamento de `59600163c` permanece **[nao verificado]**. `PROMPTS.md:3087` · `LEIS.md:115` · `test_ramo2b_c2_base_da_he.py:4–6`

## 27/11 05:56 · sessao 527ddadb4b26 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Corrigi as expectativas dos dois casos do Partido e acrescentei um RED com segundos para o 12x36 e o Comercial. O `diff --stat` registra apenas o arquivo de testes autorizado. [Testes revisados](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:222) [nao verificado]

| Caso | Antes | Depois |
|---|---|---|
| `SEMANA_FLOAT`, Partido | Trabalhava `34160/60 = 569,333…` min, com base de `31961/60 = 532,683…` min. | Trabalhado e base medem `05:00:00 − 20:07:19 = 31961/60` min. Fora da janela ficam `(34160 − 31961)/60 = 36,65` min, medidos de `05:00:00` a `05:36:39`. [Asserção](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:222) |
| `PARTIDO_LEGAIS`, 6x1 | Esperava `(221, 240)` trabalhados, 240 noturnos de relógio no P2 e HE zero. | Trabalhado `(221, 180)`, com 60 min fora no P2. A cl. 38-d é do 12x36. No Partido, 180 min noturnos dão `180 × 60/52,5 = 1440/7` min legais, com ganho de `180/7`. O vão gozado de 180 min dá suprimido zero. O excedente é `221 + 180 + 180/7 − min(420, 480) = 47/7` min, dentro da tolerância de 10, portanto **HE zero**. [Conta e asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:378) · [Cl. 38-d](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/docs/LEIS.md:84) |
| 12x36 e Comercial que cruzam | A asserção de trabalhado bruto estava no caso do Partido. | O novo RED usa os marcos da fixture de 19h–07h e injeta a janela. A batida `19:07:19 → 07:36:39` dá `44960/60` min trabalhados, zero fora e base de `42761/60` min. Reconstruir a base por `44960/60 − 2199/60` dá um float diferente nos dois motores. [RED](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:438) |

A sonda Python sem banco reproduziu as duas falhas antigas. Com cadastro e fuso fornecidos em memória, os três casos revisados passaram e os cinco mutantes abaixo falharam por asserção. Execução com as fixtures Django e certificação pelo Code e pelo Opus final [nao verificado]. [Partido semanal](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:222) · [Partido legais](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:378) · [12x36 e Comercial](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:438)

```text
$ git -C /home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946 diff --stat
 app/ponto/tests/test_ramo2b_c2_base_da_he.py | 87 ++++++++++++++++++++--------
 1 file changed, 63 insertions(+), 24 deletions(-)
```

**Recorte para a sessão**, a partir de `app` e com `DJANGO_SETTINGS_MODULE` apontando para as configurações de teste. [Entrada do Django](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/manage.py:7) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  --settings="${DJANGO_SETTINGS_MODULE:?defina o settings de teste da sessao}" \ [nao verificado]
  ponto.tests.test_ramo2b_c2_base_da_he \
  ponto.tests.test_ramo2b_c2l_red_motor \
  ponto.tests.test_ramo2b_c2h_saida
```

**Comandos dos mutantes**, na mesma sessão. As substituições usam o executor em memória da cópia. [Executor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2l_mutantes.py:12)

```bash
muta_c2p() {
  DJANGO_SETTINGS_MODULE="${DJANGO_SETTINGS_MODULE:?defina o settings de teste da sessao}" \ [nao verificado]
  PYTHONDONTWRITEBYTECODE=1 python3 - "$1" <<'PY'
import sys
from unittest.mock import patch

import django
from django.conf import settings
from django.test.utils import get_runner

django.setup()

from ponto import motor_calculo_v2 as m
from ponto.tests.ramo2b_c2l_mutantes import _mutar

raiz = 'ponto.tests.test_ramo2b_c2_base_da_he.'
partido = raiz + 'Ramo2bC2BaseDaHeTest.'
semana = partido + 'test_RED_partido_semana_float_trabalha_ate_o_marco_e_classifica_a_cauda_fora'
legais = partido + 'test_RED_partido_legais_ganho_noturno_da_cauda_fora_da_base'
cruza = raiz + 'BaseDaHeMedidaNoMarcoTest.test_RED_12x36_e_comercial_trabalham_o_bruto_e_medem_a_base_no_marco'
campo = 'CAUDA_DO_TURNO_QUE_CRUZA_NO_TRABALHADO'
ganho = m._ganho_noturno_da_base_da_he

variantes = {
    'partido_inclui_cauda': (patch.object(m.MotorTurnoPartido, campo, True), [semana, legais]),
    'cruza_corta_cauda': (patch.object(m.MotorBase, campo, False), [cruza]),
    'base_por_subtracao': (patch.object(m.MotorBase, '_gravar_base_da_he', _mutar(
        m.MotorBase._gravar_base_da_he, 'periodo, base_min, s_he, float(_leg)',
        'periodo, periodo.minutos_trabalhados - (s - s_he).total_seconds() / 60, s_he, float(_leg)',
        metodo=True)), [cruza]),
    'partido_noturno_bruto': (patch.object(m.MotorTurnoPartido, 'calcular_periodo', _mutar(
        m.MotorTurnoPartido.calcular_periodo, 'self._escrever_noturnas(periodo, e, s, intervalos)',
        'self._escrever_noturnas(periodo, e, periodo.saida, intervalos)', metodo=True)), [legais]),
    'partido_he_inclui_ganho_da_cauda': (patch.object(
        m, '_ganho_noturno_da_base_da_he',
        side_effect=lambda p: ganho(p) + p.minutos_fora_janela / 7), [legais]),
}

mutacao, alvos = variantes[sys.argv[1]]
with mutacao:
    falhas = get_runner(settings)(verbosity=2).run_tests(alvos)
raise SystemExit(int(bool(falhas)))
PY
}

muta_c2p partido_inclui_cauda
muta_c2p cruza_corta_cauda
muta_c2p base_por_subtracao
muta_c2p partido_noturno_bruto
muta_c2p partido_he_inclui_ganho_da_cauda
```

## 27/11 05:43 · sessao 68521b7293d6 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

# REPROVADO

A c12 substitui a lista manual por uma regra geral que protege os testes **existentes na base**. A regra impede as edições apontadas no placar e no diagrama, mas deixa leitores operacionais descobrirem, lerem e executarem testes novos. O cruzamento com o brief comprova presença textual do caminho. A autorização do caminho pelo brief e o `!` do motor continuam sem vínculo executável demonstrado. `bin/codex_pergunta.sh:232–241,555–565` · `bin/regua.sh:138–145,165–169` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:143–163` · `CLAUDE.md:145–146,274–277`

## 1. Achados da c11 e situação na c12

| Achado | Classificação e resultado |
|---|---|
| Edição do BUG116, do A16 e dos testes reservados | **Procedente. Cura de classe para testes presentes na base.** Saiu `RESERVADOS`. A mesma guarda recusa a edição ou remoção de qualquer teste existente que caiba na forma permitida. Dos sete nomes antigos, seis continuam recusados por existência. O sétimo, o antigo teste da cerca, pode nascer de novo. A matriz trocou esse caso por `bin/cerca_schema.py`, que já cai nas proibições de host. `bin/codex_pergunta.sh:167–177,236–241` · `bin/tests/test_codex_lista_positiva.sh:97–100,108–109,125–137,151–152` · `mapa_delta_c12.md:2–10` |
| Seleção dinâmica pelo vigia e pela régua | **Procedente. Classe aberta.** A decisão do wrapper usa presença na base, enquanto os consumidores usam a árvore que montaram para rodar e os IDs encontrados nas saídas. O caminho antigo do teste padrão de reversão fica protegido. A seleção por ID continua aberta a testes novos. A interpolação de IDs em `sh -c` e a execução direta de Docker no reteste também permanecem como achados independentes. `bin/codex_pergunta.sh:236–241` · `app/core/esteira_vigia.py:725–762,1301–1317,1352–1363` · `bin/vigia_arvore.sh:75–88` |
| Flag escolhida pelo chamador, vínculo com o brief e `!` do motor | **Procedente. Cura de classe para menção literal, parcial para autorização.** Cada flag é checada pela mesma regra. A escolha do modo continua no argv, e a presença do caminho em texto não comprova o `!`. A referência de L-126 aos testes apagados continua como questão de desenho prevista no brief. `bin/codex_pergunta.sh:89–100,498,555–565` · `app/docs/LEIS.md:147` · `CLAUDE.md:145–146,274–277` · `brief_c12.md:18–19,30–31` |
| Matriz, helper e lacunas do mapa | **Procedente. Cura de classe no helper, cobertura parcial no mapa.** A mesma asserção agora cobra caminho e resultado em todos os chamadores de `_inv_recusa`. Permanecem, entre outros, a fixture de homônimo com `crons_pkg` em vez de `crons`, as lacunas para `base.py`, `signals.py`, `gunicorn.core.conf.py`, `quarentena_lista.py`, o artefato do diagrama e os casos cruzados com caminhos autorizados. A rodada `SO_CASOS` continua saindo com `3` quando tem zero falhas e termina antes dos mutantes. `bin/tests/_codex_construtor_cenario.sh:88–100` · `bin/tests/test_codex_lista_positiva.sh:85–142,165–190` · `app/core/management/commands/crons.py:23` · `bin/selo_reciclagem.sh:40–46` |
| Interseções de models e motor autorizados | **Procedente. Continuam abertas.** `tenants/models.py` nomeado continua alcançando o provisionamento por `auto_create_schema`. A migration histórica continua importando `colaboradores.models`. `core/models.py` continua na janela de auth. O controle do motor prova aceitação e `TOCADO`, sem consultar retorno ou carimbo do elo. Efeitos de alterações específicas em produção [nao verificado]. `bin/codex_pergunta.sh:234–241,555–563` · `app/tenants/models.py:14–21` · `app/tenants/services.py:1–13` · `app/colaboradores/migrations/0004_colaborador_data_demissao_alter_colaborador_cpf_and_more.py:3,25` · `bin/auth_sitios.txt:17` · `bin/tests/test_codex_lista_positiva.sh:165–177` · `bin/cron_run.sh:11–25` |
| `api`, deploy e custódia da cerca | **Procedente. Cura de origem herdada da c11.** O SaaS inclui `api`, a pergunta entrega os labels locais ao Django e M2 cobra código `1` com a operação esperada. A fonte fica em `bin/`, e o deploy publica conforme o retorno do primeiro `shell -c`. O portão de migrations pendentes consulta outra pergunta. A variante com `MIGRATION_MODULES` e as execuções efetivas em produção ficam [nao verificado]. `app/config/settings/saas.py:47–53` · `bin/cerca_schema.py:18–36` · `bin/deploy.sh:185–217` · `bin/tests/test_cerca_schema.sh:70–98` |
| Referências ao wrapper, item C | **Procedente. Cura parcial.** Conferi em memória o verificador atual e as sete substituições. A cópia passou e as sete falharam pela regra esperada. Seguem duas referências numéricas em `PENDENTE`, a extração em runtime pelo delimitador `PROIBIDOS = [` e a possibilidade de trocar uma citação por outra âncora válida sem acusar a troca de destino naquele sítio. `bin/tests/test_ref_codex_pergunta.sh:24–96,106–124` · `bin/retencao_disco.py:152–154` · `bin/tests/test_rc_sem_colher.sh:14` · `bin/disjuntor.py:2025–2041` |
| Pacote `models/` e despacho, item D | **Procedente. Cura de classe herdada da c11.** O wrapper define a regex, o disjuntor lê essa mesma regex e o selo mantém o caso de brief contendo apenas o pacote e o mutante que retira a tag. Em brief misto, um caminho elegível satisfaz o `any` da admissão do agente. `bin/codex_pergunta.sh:170–175` · `bin/disjuntor.py:1963,2025–2041,2104–2115` · `bin/tests/test_agente_no_modelo.sh:140–146,289–293,360–362` |
| Selos verdes, HUP e rodada em produção | **Procedente como pendência de evidência.** A régua chama os selos de host e o selo de HUP copia a cerca real para sua fixture. Os anexos trazem as matrizes. Os nove selos completos no hash, os logs dos REDs históricos, o Opus final e a execução em produção ficam [nao verificado]. `bin/regua.sh:138–145` · `bin/tests/test_deploy_reload_gracioso.sh:32–61,109–125` · `brief_c12.md:32–35` |

## 2. Contraexemplo de teste novo [nao verificado]

**Procedente.** Com `app/core/models.py` na flag e na pergunta, a criação de `app/core/tests/test_model_sem_migration_pendente.py` passa. O anexo registra `rc 0` e ausência na base. Conferi a ausência na cópia e reproduzi em memória o predicado do inventário com essa entrada ausente. Um `unittest.TestCase` comum nesse módulo entra na descoberta de `core` pela régua. O selo de host de RELATO já abre o arquivo pelo glob `app/*/tests/*.py`, antes mesmo de verificar seu conteúdo. Depois de incluído no `HEAD` da cópia, o mesmo arquivo entra como label em `suite.sh --volta`, cujo seletor lê o fonte por AST. `matriz_sem_flag.txt:7–10` · `bin/codex_pergunta.sh:164–166,203–209,236–241,555–565` · `bin/regua.sh:23,138–145,165–169` · `app/core/runner_quarentena.py:72–85` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:143–163` · `bin/vizinhos_da_volta.py:25–45,92–95,116–129` · `bin/suite.sh:198–216,441–445`

- **Procedente para os dois leitores fixos na base dada.** O placar executa a classe do BUG116 por nome e seu contrato pula `tests/` na varredura. O bloco A16 abre um caminho fixo e lê duas constantes por AST. A regra impede editar os dois arquivos existentes. O antigo nome da cerca volta ao universo de testes, enquanto o deploy carrega o corpo de `bin/cerca_schema.py`. `bin/placar_code.sh:137–144` · `app/core/tests/test_bug116_credencial_deixa_trilha.py:156–170` · `app/core/management/commands/gerar_diagrama.py:132–157` · `bin/deploy.sh:194–209`
- **Procedente para o vigia, conforme a árvore.** Se um teste novo falhar na cura, `_isolar` já pode selecionar seu ID, mas o módulo está ausente de `orig/app` daquela fatia. O resultado do reteste ainda entra na classificação `fatia` ou `arvore`. Depois do pouso o módulo pode integrar o `orig` seguinte. O vigia da árvore também retesta IDs na árvore viva, e a reversão os usa se o módulo continuar presente na árvore pós-revert. Acionamento concreto em produção [nao verificado]. `bin/molde_fatia/rodar.sh:9–14,43–51` · `app/core/esteira_vigia.py:725–762,1301–1317,1352–1363` · `bin/vigia_arvore.sh:101–106,128–149`

## 3. Cruzamento flag × brief

**Procedente como checagem literal.** Para cada valor de `--lista-positiva`, o wrapper valida a forma, exige que `HASH:caminho` seja `blob` e executa `grep -qF -- "$_p" "$PERGUNTA_R"`. `PERGUNTA_R` é o `realpath` do argumento posicional `<arquivo_pergunta>`. Só `grep` com `rc 0` passa. A recusa sai antes do Codex, com diagnóstico textual e `CODEX_RESULTADO rc=2 motivo=PRE_CHECK`. `bin/codex_pergunta.sh:89–104,353–360,431–433,555–565,809–817`

**Procedente a lacuna de autorização.** Em memória, passaram `<!-- app/core/models.py -->`, `./app/core/models.py` e `app/core/models.py.bak`. O caminho literal pode ser prefixo de uma menção maior, estar em comentário ou aparecer numa instrução de proibição. Já uma menção só em outro anexo fica fora desse `grep`, embora o censo leia anexos e pergunta, e o prompt receba ambos. A validação também é uma leitura anterior ao hash de admissão. O hash posterior e a revalidação conferem o arquivo, enquanto a montagem do prompt relê o caminho e não guarda os bytes do `grep`. Alteração concorrente efetiva e argv da obra real [nao verificado]. `bin/codex_pergunta.sh:306–319,498,558–576,657–689,791–804` · `bin/disjuntor.py:2693–2729`

## 4. O que os selos provam

| Selo | Classificação e alcance |
|---|---|
| **F1 / F1-M** | **Procedente para o payload, duvidoso para execução pelo deploy.** `_f1` avalia as atribuições extraídas do deploy e executa o payload silencioso no shell Django do selo. Cobra `0` na casa e `1` com M1. A ausência da chamada deixa a definição e elimina o efeito esperado. A linha que despacha Docker é reconhecida por texto e fica fora do trecho executado pelo selo. F1 aceita qualquer `rc 1` no M1 e F1-M aceita qualquer falha de `_f1`. O M1 independente é mais específico porque cobra também a operação. `bin/tests/test_cerca_schema.sh:46–56,70–89,100–129` · `bin/deploy.sh:197–209` |
| **30 / 30-M** | **Procedente para estrutura, duvidoso para execução.** A cópia do deploy chega ao Docker falso. O AST exige `def` de topo e chamada direta posterior. Em memória, o predicado deu `0` para o payload atual, `1` com só a definição e `0` com saída antecipada antes da chamada. O falso devolve seu rc configurado independentemente do AST. O 30-M aceita contagem zero sem conferir `RC=0` nem que o payload mutado chegou à casca. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97–111,134–152,283–295` |
| **F2** | **Procedente.** Saiu `\|\| true`. `grep` com `1` é verde, `0` acusa ocorrência e qualquer outro rc é vermelho. O alcance é textual, para a grafia buscada em `.py`. Prova de custódia por AST ou de todas as formas de definição [nao verificado]. `bin/tests/test_cerca_schema.sh:131–142` |
| **`_inv_recusa`** | **Procedente.** Cobra `PROIBIDO`, campo 2 igual ao caminho, motivo por substring no campo 3, rc, `.rc`, ausência de `.id` e prefixo `CODEX_RESULTADO rc=3 `. O primeiro mutante troca o caminho **esperado** pelo helper. O segundo apaga o marcador da saída. São provas do leitor da asserção. O marcador é aceito sem conferir `motivo=INVENTARIO` ou unicidade. `bin/tests/_codex_construtor_cenario.sh:88–100` · `bin/tests/test_codex_lista_positiva.sh:193–202` |
| **Sete entradas de `p_teste_existe`** | **Procedente para a guarda, com um limite.** São sete casos aplicados à mesma mutação. Sem a guarda, as seis entradas de edição perdem a recusa ou seu motivo. O caso de remoção ainda recusa por `pos_apagado` e morde pela ausência de `teste ja existe na base`. Assim, seu RED prova o motivo específico e a remoção já era recusada. O contador geral aceita qualquer `FALHA` emitida pelo caso. `bin/codex_pergunta.sh:236–241` · `bin/tests/test_codex_lista_positiva.sh:108–109,125–137,151–152,208–214,232–249` |
| **Mutante `pos_brief`** | **Procedente.** Troca `_recusa2` por `true` no sítio da guarda. A fixture usa caminho válido e existente com pergunta sem o literal. Cobra `rc 2`, diagnóstico, marcador e ausência da captura do Codex falso. Sua prova termina na ausência total da menção. `bin/tests/test_codex_lista_positiva.sh:62–68,159–164,221–249` · `bin/tests/_codex_construtor_cenario.sh:44–54,74–86` |

**Duvidoso como RED histórico.** F1-M e 30-M são mutações reconstruídas do deploy. O deploy já enviava a chamada na c11. A troca do caso `cerca_reds` por `cerca_bin` também muda a entrada e a autoridade sob teste. Esses resultados não substituem os logs `RED.*.0b3939987.log` pedidos pelo brief [nao verificado]. `bin/deploy.sh:194–209` · `bin/tests/test_cerca_schema.sh:120–129` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:287–295` · `bin/tests/test_codex_lista_positiva.sh:97–100` · `brief_c12.md:11–17,32–35`

## 5. Escritores, leitores e autoridades

| Dado ou artefato | Escritor, leitor e classificação |
|---|---|
| `HASH`, `arvore`, `pastas` | **Procedente.** O chamador fornece `REF`. O wrapper resolve o commit, exige `HEAD == HASH` e constrói `arvore` e `pastas` do `git ls-tree` da worktree ligada. O mesmo inventário usa esse mapa para comparar modo/blob, detectar remoção e classificar a existência do teste. `cat-file -t` consulta o mesmo commit para outra pergunta, a elegibilidade dos caminhos da flag. `bin/codex_pergunta.sh:100–117,186–225,236–241,555–568,791–797,835–840` |
| `POSITIVA`, `_POSRX`, `_POSV`, `POS`, `APPS`, `RESERVADOS` | **Procedente.** O chamador escreve as flags, Bash acumula, valida e serializa, o cabeçalho lê `_POSV` e o inventário final deriva `POS` e `APPS`. Já havia duas concessões no mesmo argumento, edição literal e autorização do app para testes. Sai o escritor da lista manual `RESERVADOS` e entra a consulta de existência no inventário. `bin/codex_pergunta.sh:89–95,161–171,236–241,555–565,670–676,835–836` |
| Pergunta, anexos, `_grc`, `insumos`, `CAB` | **Procedente.** O produtor da pergunta e dos anexos fornece arquivos ao wrapper. `grep` escreve o resultado em `_grc`, consumido pela validação. O censo lê pergunta e anexos e julga os briefs da obra. `_PROJ_PY` grava e confere os hashes. O wrapper monta `CAB`, e o executor o recebe como stdin. São leituras independentes de menção, censo, integridade e prompt. O produtor efetivo do brief da chamada real fica [nao verificado]. `bin/codex_pergunta.sh:89–104,306–319,498,558–576,657–691,798–817` · `bin/disjuntor.py:1872–1901,2693–2729` |
| `.inventario`, `.recusa`, `.rc`, `.id`, `CODEX_RESULTADO` | **Procedente.** O wrapper emite os artefatos. O helper agora lê o caminho e o motivo de `.recusa`, o `.rc`, a ausência de `.id` e o marcador em `$T/saida`. O coletor de `.rc` segue lendo o recibo do wrapper. O delta muda as asserções do selo, preservando a origem dos sinais. `bin/codex_pergunta.sh:353–360,591–608,767–773,835–860` · `bin/tests/_codex_construtor_cenario.sh:75–100` · `bin/rc_sem_colher.py:65–77` |
| Fixtures, `PQ`, `POS_DEF`, `RESV`, `MATRIZ`, `MATRIZ_BASE`, `SEM_FLAG`, `MUTANTES` | **Procedente.** O selo positivo escreve a pergunta de fixture e os casos. O cenário compartilhado lê `PQ` e passa o arquivo ao wrapper. `_pos` escreve o rc observado e o motivo esperado em `MATRIZ`, que o selo imprime e filtra para `MATRIZ_BASE`. `SEM_FLAG` recebe os rc do ramo comum. A tabela `MUTANTES` dirige alterações em cópias do wrapper e o selo lê os diagnósticos resultantes. O selo legado usa o mesmo helper. `bin/tests/test_codex_lista_positiva.sh:36–82,180–255` · `bin/tests/_codex_construtor_cenario.sh:1–11,28–36,74–100` · `bin/tests/test_codex_pergunta.sh:73–74,312–354,444–451` |
| Cerca, cópias de deploy, `payload_cerca.py`, `CERCA_EXECUTA`, `N_CERCA`, `dono`/`grc` e leitor de referências | **Procedente.** Deploy e selo leem `bin/cerca_schema.py`. F1 monta e roda seu payload, e 30 captura o último argumento do Docker falso em `payload_cerca.py`. O AST escreve `CERCA_EXECUTA` no log e o selo conta em `N_CERCA`. O rótulo passou a contar forma de chamada e sua mensagem ainda fala em origem do texto. F2 lê a saída e o rc do `grep`. O selo de referências lê os arquivos em bytes com decodificação `replace`. Há duas derivações de prova nos selos, F1 por execução e 30 por forma. Segundo juiz de drift ou segundo escritor de domínio acrescentado [nao verificado]. A pergunta continua delegada ao Django, conforme a regra de autoridade de `JUIZES.md`. `bin/deploy.sh:197–212` · `bin/cerca_schema.py:26–36` · `bin/tests/test_cerca_schema.sh:100–142` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:71–78,97–107,150–152,283–297` · `bin/tests/test_ref_codex_pergunta.sh:58–70` · `app/docs/JUIZES.md:7–15,29–34` · `CLAUDE.md:180–182` |

## 6. `except`, relógio e travas

- **Procedente.** No delta fornecido `c11 → c12`, contei **0 `except` acrescentados e 1 removido**. A remoção é o `except (UnicodeDecodeError, IsADirectoryError)` do selo de referências. O `except Exception` do inventário é anterior à c12. A contagem literal pedida para `1fc9ab682..HEAD` fica [nao verificado], pois a cópia veio sem metadados Git e o patch anexo começa na c11. `bin/tests/test_ref_codex_pergunta.sh:58–70` · `bin/codex_pergunta.sh:196–228` · `brief_c12.md:20–21`
- **Duvidoso como mudança de relógio ou inversão de trava.** O delta acrescenta execução de payload com timeout no selo e nenhuma consulta nova à hora. O `grep` entra com meta e lease já tomadas. O wrapper solta a meta antes de esperar slot e resposta, readquire para revalidar e solta antes do `GO`. O inventário final continua seguido da prova de vida da lease. `bin/tests/test_cerca_schema.sh:46–52,103–116` · `bin/codex_pergunta.sh:511–525,546–549,555–580,741–762,777–817,835–840`

## 7. Matriz sem flag

**Procedente como exposição anterior à c12.** O anexo registra **43/58** aceitos no universo c12 e **42/56** para o universo c11 reconstituído, incluindo o caso antigo medido à parte. A c12 alterou a guarda dentro de `if POS`. O modo comum segue pelas proibições gerais e pelas guardas específicas de motor e `models.py`. O mapa c10 já documentava `app/config/crons_duracao.json` como gravável pelo construtor comum. A exposição do JSON é consistente com a regra original documentada em L-126. A contagem exata e a árvore do commit imediatamente anterior ao CCM ficam [nao verificado]. `matriz_sem_flag.txt:1–11` · `mapa.md:28` · `bin/codex_pergunta.sh:171–184,232–242` · `bin/tests/test_codex_lista_positiva.sh:71–79,183–187` · `app/docs/LEIS.md:147`

**Procedente o alcance do JSON sobre a agenda.** `bin/crons_duracao.py` grava `dur_s`, `config/crons.py` incorpora os valores, e os horários de `sombra.sh` e `noturno.py` os consomem por `inicio_derivado`. A sonda comprova aceitação da fixture pelo wrapper. Pouso dessa alteração, crontab instalado e efeito operacional ficam [nao verificado]. `bin/crons_duracao.py:77–99` · `app/config/crons.py:146–160,245–278,350–352,594–595` · `bin/crons.sh:17–40` · `matriz_sem_flag.txt:7–10`

## 27/11 05:08 · sessao 96e803870767 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 27/11 03:57 · sessao 0684ef753980 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

# construtor.r

**Caso 30.** O cartão recente usa `DIA - timedelta(days=1)`, o que distingue a chave `pergunta_unica_por_marco` e preserva o motivo do alvo. [Fixture](chamados/tests/test_e4d_matriz_reds.py:1640) [Restrição](chamados/models.py:3944)  
A resposta recente continua um minuto depois da resposta do alvo. As asserções de SQL, API, modal e raio-x são as mesmas. [Asserções](chamados/tests/test_e4d_matriz_reds.py:1642)

Na base, a fila SQL exige `respondida_em` preenchido. A asserção esperada compara a fila que perde o alvo com `[p.pk, recente.pk]`. A falha após a correção fica **[nao verificado]** na suíte do Code. [Filtro SQL](chamados/juizes.py:1676) [Fila](chamados/juizes.py:1863) [Asserção](chamados/tests/test_e4d_matriz_reds.py:1647)

## Diff

| Arquivo | Inserções | Remoções |
|---|---:|---:|
| [e4d_matriz_mutantes.py](chamados/tests/e4d_matriz_mutantes.py:551) | 148 | 18 |
| [test_e4d_matriz_reds.py](chamados/tests/test_e4d_matriz_reds.py:1640) | 2 | 1 |
| **Total** | **150** | **19** |

`git diff --check` e a compilação em memória passaram. O diff acrescenta **0 `except`**. A comparação com a base confirmou que a única mudança nos REDs é a chamada da fixture do caso 30. [Fixture](chamados/tests/test_e4d_matriz_reds.py:1640) [Arnês](chamados/tests/e4d_matriz_mutantes.py:551)

## Seletores para o Code

O arnês expõe as **39 variantes da lista da fix5**, incluindo a da fixture. As outras 31 definições preexistentes ficaram fora do seletor desta volta. Cada classe roda controle, mutação com restauração registrada em `addCleanup` e novo controle. O conjunto de falhas é comparado por método e, no caso 30, por subteste de leitor. O ensaio sintético conferiu essa comparação e a restauração. As mordidas na suíte ficam **[nao verificado]**. [Arnês](chamados/tests/e4d_matriz_mutantes.py:551) [Classes](chamados/tests/e4d_matriz_mutantes.py:607) [Leitores](chamados/tests/e4d_matriz_mutantes.py:662)

RED do caso 30. [Teste](chamados/tests/test_e4d_matriz_reds.py:1617)

```bash
bin/suite.sh --only "chamados.tests.test_e4d_matriz_reds.MatrizRaizELeitoresTest.test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta"
```

Linha dos mutantes para `bin/suite.sh`. [Classes](chamados/tests/e4d_matriz_mutantes.py:607)

```bash
--only "chamados.tests.e4d_matriz_mutantes.M04ReverteOutraCompetencia chamados.tests.e4d_matriz_mutantes.M05SegundoEscritorM chamados.tests.e4d_matriz_mutantes.M06CarimboReescrito chamados.tests.e4d_matriz_mutantes.M07EscritorAstForaDaPorta chamados.tests.e4d_matriz_mutantes.M07FabricaConclusao chamados.tests.e4d_matriz_mutantes.M08RegravaExportada chamados.tests.e4d_matriz_mutantes.M09SegundoEscritorV chamados.tests.e4d_matriz_mutantes.M09MaterializaForaDaPorta chamados.tests.e4d_matriz_mutantes.M10SegundoEscritorR chamados.tests.e4d_matriz_mutantes.M10MaterializaForaDaPorta chamados.tests.e4d_matriz_mutantes.M11DescartaReaberturaVazia chamados.tests.e4d_matriz_mutantes.M11RetryGanhaNovoPedido chamados.tests.e4d_matriz_mutantes.M12AutorDaFotoSetNull chamados.tests.e4d_matriz_mutantes.M12OcultaAutoriaDesconhecida chamados.tests.e4d_matriz_mutantes.M18C6JulgaCompensacaoCedo chamados.tests.e4d_matriz_mutantes.M18I23ZeroFixo chamados.tests.e4d_matriz_mutantes.M18RegistrarAceitaCompensadorFuturo chamados.tests.e4d_matriz_mutantes.M20IgnoraRecusaNoEmpate chamados.tests.e4d_matriz_mutantes.M20ReusaPedidoDaAusencia chamados.tests.e4d_matriz_mutantes.M26DevolveSemVinculo chamados.tests.e4d_matriz_mutantes.M26PreservaSemEncaminhar chamados.tests.e4d_matriz_mutantes.M27JulgaInstanciaAntiga chamados.tests.e4d_matriz_mutantes.M27ReleSoACelula chamados.tests.e4d_matriz_mutantes.M27FantasmaForaDaPorta chamados.tests.e4d_matriz_mutantes.M28ViaSemCompensacao chamados.tests.e4d_matriz_mutantes.M28UsaPrimeiroEspelho chamados.tests.e4d_matriz_mutantes.M28ViaForaDaPorta chamados.tests.e4d_matriz_mutantes.M30FilaPeloCarimboCru chamados.tests.e4d_matriz_mutantes.M30ApiPeloParCru chamados.tests.e4d_matriz_mutantes.M30ModalPeloParCru chamados.tests.e4d_matriz_mutantes.M30RaioxPeloCarimboCru chamados.tests.e4d_matriz_mutantes.M31RaizForaDaPorta chamados.tests.e4d_matriz_mutantes.M31ChamadoForaDaPorta chamados.tests.e4d_matriz_mutantes.M32RaizForaDaPorta chamados.tests.e4d_matriz_mutantes.M32FilhaNasceComV chamados.tests.e4d_matriz_mutantes.M36IgnoraTranca chamados.tests.e4d_matriz_mutantes.M37DestranqueForaDaPorta chamados.tests.e4d_matriz_mutantes.M37RegravaRNoDestranque chamados.tests.e4d_matriz_mutantes.MFixtureOmiteEntradaCongelada"
```

## Censo de relógio

| Ocorrência | Cobertura |
|---|---|
| [`timezone.now()` nos REDs](chamados/tests/test_e4d_matriz_reds.py:185) | `freeze_time(T1)` no `setUp`. O teste isolado da fixture usa `freeze_time(T2)`. [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) [Fixture](chamados/tests/test_e4d_matriz_reds.py:385) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:130) | String de mutação que aponta para a leitura da fixture. [Fixture](chamados/tests/test_e4d_matriz_reds.py:185) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:302) | Variante fora do seletor da fix5. Seu alvo, caso 16, herda `freeze_time(T1)`. [Caso 16](chamados/tests/test_e4d_matriz_reds.py:663) [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:334) | Caso 18 sob `freeze_time(T1)`, com `freeze_time(T2)` no controle que aceita o ato. [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) [Controle T2](chamados/tests/test_e4d_matriz_reds.py:783) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:475) | Variante fora do seletor da fix5. Seu alvo, caso 31, herda `freeze_time(T1)`. [Caso 31](chamados/tests/test_e4d_matriz_reds.py:1684) [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:496) | Caso 32 sob `freeze_time(T1)` do `setUp`. [Caso 32](chamados/tests/test_e4d_matriz_reds.py:1718) [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) |
| `localdate()` e `date.today()` | Zero ocorrências nos dois arquivos. [REDs](chamados/tests/test_e4d_matriz_reds.py:1) [Mutantes](chamados/tests/e4d_matriz_mutantes.py:1) |

## 27/11 03:47 · sessao 501893001208 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

O diff da fix5 está nos dois arquivos autorizados. A compilação e o `git diff --check` passaram. Em ensaios isolados, o selo AST rejeitou a escrita injetada do caso 07 e o teste da fixture rejeitou a omissão de `entrada_em`. A suíte Django e as mordidas em execução ficam **[nao verificado]**. [REDs](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1) [Mutantes](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:1) [nao verificado]

## Diff e decisões

```bash
GIT_OPTIONAL_LOCKS=0 git diff -- \
  chamados/tests/test_e4d_matriz_reds.py \
  chamados/tests/e4d_matriz_mutantes.py
```

- O caso 14 e os casos 01/02/42/44/45 ficaram idênticos ao HEAD. `marco_do_estoque` também ficou intacto. Um assert incondicional ali anteciparia a falha dos casos 04/07/08/18 para a ausência da 0057 e esconderia os contratos desta volta. [Fixture de estoque](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:238) [Caso 14](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:644)
- Os REDs 04 e 08 propõem as interfaces `--dir-reversao`/`--reverter` e `--so-trilha-exportada`/`--aval`. O 04 usa um JSON por competência com foto e SHA-256. Essas interfaces e o formato do JSON são decisões do teste, com encaixe em produção **[nao verificado]**. O parser da base declara apenas `--aplicar`. [Comando atual](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/backfill_decisoes_pergunta.py:455) [Reversão](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:459) [Exportada](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:540) [nao verificado]
- Os nomes `pedido_id`, `autoria_desconhecida`, `pergunta_sem_absorcao_encaminhada_admin` e `perguntas_sem_absorcao_na_lista_do_admin` são contratos propostos pelos REDs. O selo lê `PORTAS` e exige uma lápide com entidade, via, arquivo, função e motivo no próprio registro. O scanner cobre escritas com via literal e a observação SQL cobre as rotas exercitadas. A cobertura exaustiva de escritores **[nao verificado]**. [Pedido](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:570) [Autoria](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:628) [Admin](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1579) [Selo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:305)
- O ensaio isolado revelou um `TypeError` ao desmontar a troca de `__code__` no runner. Corrigi a restauração em `finally` e conferi que a função original volta após o mutante. [Runner](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:15)

## Caso → o que exige → mutante → morde?

`NV` significa **[nao verificado] na suíte**. A coluna registra o resultado observado, e os comandos abaixo identificam cada variante. [nao verificado]

| Caso | O que exige | Mutante | Morde? |
|---|---|---|---|
| 04 | Foto e hash próprios. Reverter B primeiro preserva A, o ato preexistente e a vizinha. [RED:459](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:459) | Reverte a outra competência junto | NV |
| 05 | `ABSORCAO` e `VALIDACAO` físicas antes de V/M, no mesmo atomic, pelas duas entradas. [RED:1395](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1395) | Segundo escritor de M no módulo e no alias | NV |
| 06/07 | Carimbo de célula pela exceção com lápide. Backfill preserva a foto e mantém zero ato novo e zero evento. [06:898](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:898) [07:526](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:526) | Reescreve carimbo, injeta escritor AST, fabrica conclusão | AST sim, com lápide sintética em memória. Suíte NV | [nao verificado]
| 08 | Via autorizada insere R e V na trilha. Hashes do TXT e do gravado iguais, zero escrita de coluna e zero evento. [RED:540](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:540) | Escreve uma coluna da exportada | NV |
| 09/10 | Ato físico precede a escrita de V/R. Resposta com texto, M pela exceção com lápide. [09:918](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:918) [10:959](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:959) | Segundo escritor de V/R ou M fora da porta | NV |
| 11/12 | Mesmo pedido preserva as linhas, pedido distinto grava por pergunta. FK protege e autoria histórica desconhecida tem marca explícita. [11:570](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:570) [12:608](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:608) | Descarta vazia, dá chave nova ao retry, volta `SET_NULL` ou omite a marca | NV |
| 18 | Compensador futuro restaurado, horizonte comum e recusa de ato futuro no `registrar`. Controles do I23 incluem `REABERTURA` e `RECUSA` com recuo do relógio. [Restore:701](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:701) [Registrar:766](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:766) | C6 antecipa, I23 zera fixo ou `registrar` aceita futuro | NV |
| 20 | Recusa para só Q, preserva irmãs e distingue dois pedidos na mesma ausência. [RED:1440](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1440) | Ignora empate ou reutiliza pedido da ausência | NV |
| 26 | Preserva R/V/M e DP. Registra Q e a apresenta na lista do admin. [RED:1579](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1579) | Devolve sem vínculo ou preserva sem encaminhar | NV |
| 27/28 | Releitura sob trava, com dia e célula atuais no fantasma. No espelho, resultado revogado preserva a pergunta, e H4 derrubado ganha `REABERTURA`. M exige lápide. [27:1126](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1126) [28:1167](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1167) | Instância antiga, só FK fresca, primeiro espelho, omissão da compensação ou escrita fora da porta | NV |
| 30 | Texto `09:00` em ambas as fontes. Instante do ato na ordem SQL, API, modal e crédito do raio-x. [RED:1617](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1617) | Um leitor por vez usa o instante cru | NV |
| 31/32 | Nascimento da raiz e do chamado pelas portas declaradas. Filha vazia na abertura direta e na emissão. [31:1683](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1683) [32:1717](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1717) | Escrita de mesmo valor fora da porta ou nascimento com V | NV |
| 36/37 | Consulta fresca vê a tranca e preserva a irmã de outra competência. Destranca preserva R e aplica M pela exceção com lápide. [36:1793](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1793) [37:1829](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1829) | Ignora tranca, regrava R ou destranca fora da porta | NV |
| Item 9 | `fato()` explicita `entrada_em` pelo relógio congelado quando o campo existe e preserva o instante dado pelo caller. [RED:377](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:377) | Omite a entrada e deixa o default agir | Sim, com relógio simulado em memória. Suíte NV | [nao verificado]

## Item 10 — conferência do verificador v2

| Caso | Veredito e motivo |
|---|---|
| 09 | **PROCEDENTE.** A rota carimba V sem registrar `VALIDACAO`. O recorte anterior só conferia o ato no primeiro ramo e deixava M sem selo. [Resolvedor:2118](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:2118) [RED:918](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:918) |
| 10 | **PROCEDENTE.** O ramo carimba R e emite `fecho_sem_credor`; esse log não é uma `RESPOSTA` física. A via de M também precisava de selo. [Resolvedor:2192](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:2192) [RED:959](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:959) |
| 11 | **PROCEDENTE.** A assinatura atual não recebe identidade do pedido e o filtro do lote exclui vazias. Uma chamada isolada não prova idempotência. [Porta:1628](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:1628) [Filtro:1695](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:1695) |
| 12 | **PROCEDENTE.** As quatro FKs já eram isoladas no teste. O modelo ainda usa `SET_NULL` e o teste anterior deixava passar autoria histórica nula sem marca. [Projeção:3804](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/models.py:3804) [Trilha:4347](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/models.py:4347) [RED:628](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:628) | [nao verificado]
| 20 | **PROCEDENTE.** A materialização reaproveita a ausência do mesmo tipo. Conferir só a FK da ausência deixa passar uma chave fixa para pedidos distintos. [Materialização:1037](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/materializacao.py:1037) [RED:1451](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1451) |
| 26 | **PROCEDENTE.** A devolução retorna cedo sem itens e reabre os selecionados pelo caminho atual. Só conferir a foto deixava passar a omissão do registro e da lista. [Materialização:1239](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/materializacao.py:1239) [RED:1579](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1579) |
| 27 | **PROCEDENTE como lacuna do teste.** O reconciliador relê a pergunta sob trava. A fixture anterior distinguia apenas a FK, enquanto o juiz também usa o dia. [Releitura:1265](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/reconciliador.py:1265) [Juiz:1182](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/reconciliador.py:1182) | [nao verificado]
| 28 | **PROCEDENTE.** `declaracao` fica fora de `SEM_HISTORIA`. O novo controle elegível com H4 chega à etapa que precisa de trava e compensação, e o espelho que muda de resposta exige releitura efetiva. [Seletor:31](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/encerrar_perguntas_com_fato.py:31) [RED:1167](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1167) |
| 36 | **PROCEDENTE como lacuna do teste.** `_pv_memo` depende da instância e do instante. A consulta nova isola a regra da tranca. A causa dinâmica da falha da pista antiga fica **[nao verificado]**. [Memo:931](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/juizes.py:931) [RED:1793](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1793) | [nao verificado]

## Comandos para a pista do Code

O runner recebe a variante por `E4D_MUTANTE`. Os comandos usam a configuração CI de PostgreSQL e os seletores dos REDs. [Runner](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:547) [Configuração CI](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/config/settings/ci.py:6)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4d_matriz_reds

m() {
  PYTHONDONTWRITEBYTECODE=1 E4D_MUTANTE="$1" python3 manage.py test \
    --settings=config.settings.ci \
    --testrunner=chamados.tests.e4d_matriz_mutantes.RunnerMutanteE4D \
    "chamados.tests.test_e4d_matriz_reds.$2"
}

h() { m "$1" "MatrizHistoricoTest.$2"; }
p() { m "$1" "MatrizPortasTest.$2"; }
a() { m "$1" "MatrizAusenciaTest.$2"; }
r() { m "$1" "MatrizRaizELeitoresTest.$2"; }

h 04_reverte_a_outra_competencia test_caso_04_aplica_fotos_separadas_por_competencia
a 05_segundo_escritor_m test_caso_05_absorcao_e_validacao_antes_de_v_m_no_mesmo_atomic
p 06_carimbo_reescrito test_caso_06_celula_absolve_so_com_carimbo
h 07_escritor_ast_fora_da_porta test_caso_07_backfill_celula_absolveu_fica_como_estava
h 07_fabrica_conclusao test_caso_07_backfill_celula_absolveu_fica_como_estava
h 08_regrava_exportada test_caso_08_exportada_veta_apply_comum_e_preserva_txt_e_gravado
p 09_segundo_escritor_v test_caso_09_resolvedor_validar_registra_antes_de_v
p 09_m_fora_da_porta test_caso_09_resolvedor_validar_registra_antes_de_v
p 10_segundo_escritor_r test_caso_10_resolvedor_responder_registra_so_com_texto
p 10_m_fora_da_porta test_caso_10_resolvedor_responder_registra_so_com_texto
h 11_descarta_reabertura_vazia test_caso_11_reabertura_vazia_grava_ato_sem_projetar
h 11_retry_ganha_novo_pedido test_caso_11_reabertura_vazia_grava_ato_sem_projetar
h 12_autor_da_foto_set_null test_caso_12_proteger_cada_referencia_de_autoria
h 12_oculta_autoria_desconhecida test_caso_12_backfill_marca_autoria_desconhecida
h 18_c6_julga_compensacao_cedo test_caso_18_estoque_futuro_compensa_no_horizonte_comum
h 18_i23_zero_fixo test_caso_18_estoque_futuro_compensa_no_horizonte_comum
h 18_registrar_aceita_compensador_futuro test_caso_18_registrar_recusa_compensador_futuro
a 20_ignora_recusa_no_empate test_caso_20_recusa_para_so_q_e_preserva_irma_ja_absorvida
a 20_reusa_pedido_da_ausencia test_caso_20_pedidos_distintos_na_mesma_ausencia
a 26_devolve_sem_vinculo test_caso_26_sem_absorcao_preserva_legado
a 26_preserva_sem_encaminhar test_caso_26_sem_absorcao_preserva_legado
p 27_julga_instancia_antiga test_caso_27_fantasma_trava_rele_julga_e_escreve
p 27_rele_so_a_celula test_caso_27_fantasma_trava_rele_julga_e_escreve
p 27_fantasma_fora_da_porta test_caso_27_fantasma_trava_rele_julga_e_escreve
p 28_via_sem_compensacao test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4
p 28_usa_o_primeiro_espelho test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4
p 28_via_fora_da_porta test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4
r 30_fila_pelo_carimbo_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 30_api_pelo_par_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 30_modal_pelo_par_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 30_raiox_pelo_carimbo_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 31_raiz_fora_da_porta test_caso_31_nascimento_da_raiz_e_do_chamado
r 31_chamado_fora_da_porta test_caso_31_nascimento_da_raiz_e_do_chamado
r 32_raiz_fora_da_porta test_caso_32_nascimento_da_filha_com_rvm_vazios
r 32_filha_nasce_com_v test_caso_32_nascimento_da_filha_com_rvm_vazios
r 36_ignora_tranca test_caso_36_tranca_retirada_da_fila_por_leitura
r 37_destranque_fora_da_porta test_caso_37_destranca_passivo_preserva_resposta
r 37_regrava_r_no_destranque test_caso_37_destranca_passivo_preserva_resposta
m fixture_omite_entrada_congelada FixtureMatrizTest.test_fato_explica_entrada_quando_o_campo_existe
```

## Perguntas de lei

1. **Caso 14.** Para o meio-par histórico de L-130, qual ato prevalece entre `SANEAMENTO` e `REABERTURA`? O teste continua congelado. `opus.md:19` · [RED:644](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:644) [nao verificado]
2. **Caso 18.** O zero para compensadores de restore deve sair no próprio I23 ou em um contador separado? O RED usa I23 e preserva o controle da `REABERTURA` legítima que fica adiantada após recuo do relógio. [I23 atual](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/auditar_invariantes_chamados.py:1122) [Caracterização F5](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319)
3. **Caso 28.** A presença de H4 vigente e de fato novo confirmado sob trava torna elegível uma pergunta com via `declaracao`? O RED preserva o cenário original e também exercita `legado_sem_lastro`, que já entra no seletor. [Seletor:36](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/encerrar_perguntas_com_fato.py:36) [RED:1167](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1167)

## 27/11 03:46 · sessao d6cd1e928312 · E4C-F5-INSTANTE · construtor · origem=Code

Apliquei o diff nos dois arquivos autorizados. A compilação estática passou, e a comparação com o conteúdo inicial preservou os REDs nomeados e os skips. A suíte e as mordidas em PostgreSQL ficam para o Code. [test_e4c_f5_c4_red.py:492](chamados/tests/test_e4c_f5_c4_red.py:492), [e4c_f5_c4red2_mutantes.py:490](chamados/tests/e4c_f5_c4red2_mutantes.py:490). Execução em PostgreSQL [nao verificado].

1. **Escritores.** O campo declara `db_default=Now()`. O controle lê `statement_timestamp()` antes e depois de cada escritor, exige que `entrada_em` caia no intervalo inclusivo e mantém `entrada_em != MARCO`. M33 grava no marco e M34 grava um instante fixo anterior. A fonte instalada de `django/db/models/functions/datetime.py::Now.as_postgresql` estava indisponível nesta cópia, então a emissão SQL dessa instalação fica [nao verificado]. O comando abaixo imprime a fonte e a linha no ambiente do Code. [chamados/models.py:4406](chamados/models.py:4406), [requirements.txt:13](requirements.txt:13), [test_e4c_f5_c4_red.py:499](chamados/tests/test_e4c_f5_c4_red.py:499), [e4c_f5_c4red2_mutantes.py:623](chamados/tests/e4c_f5_c4red2_mutantes.py:623).
2. **Item 13.** `competencia_fechada` barra `aprovado` e `exportado`. A testemunha da F6 mostra `bloqueada_competencia`. A porta `validar_pergunta` devolve `(False, 'nao_gravou')`, conforme o teste já existente da F6, que também exige a trilha de devolução. O controle mantém o sucesso em FM aberto. Em FM exportado, exige a tupla exata, o motivo `COMPETENCIA_APROVADA` e a preservação das respostas, validações e carimbos do par, das batidas e do TXT. M32 injeta o retorno K6 `bloqueada_competencia_entregue`, com mordida esperada só em `fm='exportado'`. [ponto/janelas.py:87](ponto/janelas.py:87), [materializacao.py:943](chamados/services/materializacao.py:943), [validacao.py:869](chamados/services/validacao.py:869), [validacao.py:243](chamados/services/validacao.py:243), [test_bug_par_volta.py:240](chamados/tests/test_bug_par_volta.py:240), [test_e4c_f5_c4_red.py:902](chamados/tests/test_e4c_f5_c4_red.py:902), [e4c_f5_c4red2_mutantes.py:603](chamados/tests/e4c_f5_c4red2_mutantes.py:603).
3. **M21.** O anexo mostra as duas rodadas no conjunto observado e só o teste-pai no esperado. A cópia confirma que `vistos` é comparado dentro de `subTest`. Essa é a asserção certa, pois trocar `<=` por `<` deixa a linha empatada como `ato` nas duas rodadas. O conjunto esperado agora contém exatamente `rodada=1` e `rodada=2`. [0058_decisao_entrada_do_ato_historico.py:20](chamados/migrations/0058_decisao_entrada_do_ato_historico.py:20), [test_e4c_f5_c4_red.py:1062](chamados/tests/test_e4c_f5_c4_red.py:1062), [e4c_f5_c4red2_mutantes.py:490](chamados/tests/e4c_f5_c4red2_mutantes.py:490).

### Comandos para o Code

```bash
PYTHONDONTWRITEBYTECODE=1 python -c 'import inspect; from django.db.models.functions.datetime import Now; f = Now.as_postgresql; print(inspect.getsourcefile(f), inspect.getsourcelines(f)[1]); print(inspect.getsource(f))'

PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_e4c_f5_c4_red

mut=chamados.tests.e4c_f5_c4red2_mutantes
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M21Empate0058.test_mordida"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M32FMExportadoBarra.test_mordida"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M33EntradaNoMarco.test_mordida"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M34EntradaFixa.test_mordida"
```

### Diff

```diff
--- a/chamados/tests/test_e4c_f5_c4_red.py
+++ b/chamados/tests/test_e4c_f5_c4_red.py
@@ -500,7 +500,9 @@
                 p = self._historica(respondida_em=RESPOSTA_EM, resposta_colab='07:13')
                 with connection.cursor() as cursor:
-                    cursor.execute('SELECT transaction_timestamp()')
-                    instante_db, = cursor.fetchone()
-                self._escrever(p, escritor)
+                    cursor.execute('SELECT statement_timestamp()')
+                    antes_db, = cursor.fetchone()
+                    self._escrever(p, escritor)
+                    cursor.execute('SELECT statement_timestamp()')
+                    depois_db, = cursor.fetchone()
                 p.refresh_from_db()
                 atos = list(p.decisoes.order_by('ato_em', 'pk'))
@@ -517,5 +519,6 @@
                 self.assertEqual(D.divergencia_ato_x_coluna(p, MARCO), [])
                 self.assertNotEqual(novo.entrada_em, MARCO, 'ato novo entra fora do marco')
-                self.assertEqual(novo.entrada_em, instante_db, 'ato novo entra no instante da transação')
+                self.assertGreaterEqual(novo.entrada_em, antes_db, 'entrada do ato respeita o limite anterior à escrita')
+                self.assertLessEqual(novo.entrada_em, depois_db, 'entrada do ato respeita o limite posterior à escrita')
                 transaction.set_rollback(True)
 
@@ -898,20 +901,36 @@
 
     def test_item13_validacao_livre_com_txt_invalidado(self):
-        """O TXT invalidado sem substituto libera o par em FM aberto e exportado."""
+        """Sem TXT vigente, FM aberto libera o par e FM exportado aciona a F6."""
+        from chamados.catalogo.recusa import traduzir_recusa
+
         txt = self._entregar()
         with freeze_time(ITEM13 + HORA):
             txt.invalidar(motivo='Entrega cancelada na fixture', por=self.admin)
         self.assertFalse(competencia_entregue(self.empresa.pk, self.mes, self.ano))
-        batidas, arquivo_anterior, _ = self._gravado()
+        perguntas = (self.saida, self.volta)
+        campos_do_par = PAR + CARIMBO
+        par_antes = [valores(p, campos_do_par) for p in perguntas]
         for status in ('aberto', 'exportado'):
             with self.subTest(fm=status), transaction.atomic():
                 FechamentoMensal.objects.filter(pk=self.fm.pk).update(status=status)
+                gravado = self._gravado()
                 with freeze_time(ITEM13 + 2 * HORA):
                     r = validacao.validar_pergunta(self.saida.pk, self.admin, confirmar_implausivel=True)
-                self.assertTrue(r['ok'], 'TXT invalidado libera a validação')
-                self._assert_par_gravado()
-                self.assertEqual(self._gravado()[1], arquivo_anterior)
-                self.assertEqual(list(Batida.objects.filter(pk__in=self.chao).order_by('pk').values()), batidas)
-                for p in (self.saida, self.volta):
+                if status == 'aberto':
+                    batidas, arquivo_anterior, _ = gravado
+                    self.assertTrue(r['ok'], 'TXT invalidado libera a validação')
+                    self._assert_par_gravado()
+                    self.assertEqual(self._gravado()[1], arquivo_anterior)
+                    self.assertEqual(list(Batida.objects.filter(pk__in=self.chao).order_by('pk').values()), batidas)
+                else:
+                    self.assertEqual((r['ok'], r['estado']), (False, 'nao_gravou'),
+                                     'FM exportado sem TXT vigente recusa pela F6')
+                    for p in perguntas:
+                        p.refresh_from_db()
+                    self.assertEqual(traduzir_recusa(self.saida.recusa_motivo)[0], 'COMPETENCIA_APROVADA')
+                    self.assertEqual([valores(p, campos_do_par) for p in perguntas], par_antes,
+                                     'F6 preserva respostas, validações e carimbos do par')
+                    self.assertEqual(self._gravado(), gravado, 'F6 preserva batidas e TXT')
+                for p in perguntas:
                     self.assertEqual(D.divergencia_ato_x_coluna(p, None), [])
                 transaction.set_rollback(True)
--- a/chamados/tests/e4c_f5_c4red2_mutantes.py
+++ b/chamados/tests/e4c_f5_c4red2_mutantes.py
@@ -490,5 +490,6 @@
 M21Empate0058 = contraprova(
     'M21Empate0058', red.MarcoDa0058Test, 'test_0058_empate_e_reexecucao_sobre_update_ja_aplicado',
-    '0058 inclui a entrada no instante do marco', recorte_0058_estrito)
+    '0058 inclui a entrada no instante do marco', recorte_0058_estrito,
+    subcasos=(dict(rodada=1), dict(rodada=2)))
 
 M22SoValidacaoNaIrma = contraprova(
@@ -616,3 +617,27 @@
 M32FMExportadoBarra = contraprova(
     'M32FMExportadoBarra', red.Item13PorPKTest, 'test_item13_validacao_livre_com_txt_invalidado',
-    'TXT invalidado libera a validação', fm_exportado_barra_validacao, subcasos=(dict(fm='exportado'),))
+    'FM exportado sem TXT vigente recusa pela F6', fm_exportado_barra_validacao,
+    subcasos=(dict(fm='exportado'),))
+
+
+def entrada_em_fixa(instante):
+    bulk_create = D.objects.bulk_create
+
+    def gravar(linhas, *args, **kwargs):
+        linhas = list(linhas)
+        for linha in linhas:
+            if linha.entrada_origem == D.ATO_NOVO:
+                linha.entrada_em = instante
+        return bulk_create(linhas, *args, **kwargs)
+
+    return patch.object(D.objects, 'bulk_create', new=gravar)
+
+
+M33EntradaNoMarco = contraprova(
+    'M33EntradaNoMarco', red.EstoqueDoParTest, 'test_escritores_gravam_o_novo_ato_e_a_coluna_no_mesmo_instante',
+    'ato novo entra fora do marco', lambda: entrada_em_fixa(red.MARCO),
+    subcasos=tuple(dict(escritor=e) for e in ('carimbar', 'validar', 'responder')))
+M34EntradaFixa = contraprova(
+    'M34EntradaFixa', red.EstoqueDoParTest, 'test_escritores_gravam_o_novo_ato_e_a_coluna_no_mesmo_instante',
+    'entrada do ato respeita o limite anterior à escrita', lambda: entrada_em_fixa(red.MARCO - red.HORA),
+    subcasos=tuple(dict(escritor=e) for e in ('carimbar', 'validar', 'responder')))
```

## 27/11 03:44 · sessao aacf2bb78d5c · - · - · origem=Ronald (tmux)

```text
/bin/bash: line 4: /home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/INTRUSO_COPIA.txt: Read-only file system
/bin/bash: line 4: /home/ronald/saas-hasner/INTRUSO_VIVA.txt: Read-only file system
/bin/bash: line 4: /tmp/INTRUSO_TMP.txt: Read-only file system
```

## 27/11 03:44 · sessao 9dfe567edf08 · - · - · origem=Ronald (tmux)

ok

## 27/11 03:37 · sessao 55eda9ce28c0 · E4C-F5-INSTANTE · construtor · origem=Code

Apliquei a c4red5 nos dois arquivos de teste. O diff passa em `git diff --check`, os dois módulos compilam em memória e as asserções do item 10 são idênticas às do HEAD. O ensaio isolado do arnês aceitou o conjunto exato e rejeitou a falta de um subcaso. [REDs](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:301) · [Arnês](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:26) [nao verificado]

**PRONTO 9(a–c) [nao verificado].** Com a restrição de acesso ao banco, ficam pendentes o log dos REDs no HEAD, o verde da c1 e os ciclos verde → mordida → verde na pista do Code. [Configuração da pista](/home/ronald/wt-codex-e4c-f5-instante-012038/app/config/settings/ci.py:6) [nao verificado]

| Item | Teste | Mutante | Morde? | [nao verificado]
|---|---|---|---|
| 1 | Foto da recusa dentro das transações do [estoque](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:338) e do [worker](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:801) | [M30 × 2, M27](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:569) | [nao verificado] |
| 2 | PK e ato nos REDs de [fronteira, reprojeção e conversa](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:355). Mensagens integrais na [elegibilidade](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:434). | [M07, M09, M11](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:440) | [nao verificado] |
| 3 | Três escritores sem trava prévia da Empresa. O ato novo confere autor, motivo e instante da transação. [Controle vivo e skips A/B](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:464) | [M26](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:503) | Estacionado pela lei A |
| 4 | Dois PKs com B novo, um PK de estoque e skip da lei 1. [Comparador e leitores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:613) | [M12a, M12b, M13](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:460) | [nao verificado] |
| 5 | Alarme e placar em subcasos próprios. [Asserções](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:673) | [M12a no contador, M12b nas duas saídas](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:460) | [nao verificado] |
| 6 | Identidade da candidata e instante futuro na coluna em memória. [Horizonte](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:233) | [M31](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:597) | [nao verificado] |
| 7 | [TXT invalidado com FM aberto e exportado](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:899) e [devolução via `validar_pergunta` após recusa por paridade](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:986) | [M32](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:616), [M29](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:535) | [nao verificado] |
| 8 | [Conjunto exato por `str(teste)`](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:51). A ordem das travas fica como [pré-condição da fixture](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:790). | [M07 exige dois subcasos, M11 exige sete](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:440). M14 e M25 removidos. | Conjunto exato conferido no ensaio isolado. Mordidas [nao verificado]. | [nao verificado]
| 9 | REDs, c1 e controles antes e depois de cada mutação. [Ciclo do arnês](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:46) | Comandos abaixo | [nao verificado] |
| 10 | Validação futura semeada como estoque restaurado. [Asserções da tarefa](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:301) | [M28](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:517) | [nao verificado] |

**Diff e comandos para a pista do Code**

```bash
git --no-optional-locks --no-pager diff -- \
  chamados/tests/test_e4c_f5_c4_red.py \
  chamados/tests/e4c_f5_c4red2_mutantes.py

export PYTHONDONTWRITEBYTECODE=1
python3 manage.py test --settings=config.settings.ci -v 2 \
  chamados.tests.test_e4c_f5_c4_red
python3 manage.py test --settings=config.settings.ci -v 2 \
  chamados.tests.test_e4c_f5_instante_c1

mordida() {
  python3 manage.py test --settings=config.settings.ci -v 2 \
    "chamados.tests.e4c_f5_c4red2_mutantes.$1.test_mordida"
}

# PRONTO 9(c), mais M32 no controle de TXT invalidado
for mutante in \
  M08QualquerResposta M11Elegibilidade M12aVigia M12bAlarme \
  M13Dry M21Empate0058 M24RelogioAposEspera M27K6LidoAntesDaTrava \
  M28CompensadorCru M29DevolucaoApagaResposta \
  M30EstoqueSemAtomic M30WorkerSemAtomic M31FotoRelida M32FMExportadoBarra
do
  mordida "$mutante"
done
```

Na cura, a mesma função recebe `M01DuasValidacoes`, `M02IgnoraResposta`, `M03FotoDaLavra`, `M04ValidacaoFutura`, `M07ValidacaoNoMarco`, `M09Conversa`, `M10Proveniencia`, `M15SoFM`, `M16TrilhaVazia`, `M17AlteraTXT`, `M18AlteraBatida`, `M19RecusaFutura`, `M20EmissaoDupla`, `M22SoValidacaoNaIrma` e `M23SoCandidata`. `mordida M26FotoViraAto` fica para depois da decisão da lei A. [Labels das contraprovas](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:425)

## 27/11 03:16 · sessao 196363bf4456 · E4D-CONTRATO-ROTA-B · verificador · origem=Code

A volta 2 ainda deixa passar uma implementação sem reversão por competência, sem a via autorizada da exportada e sem lista de exceções com lápides. Também aceita `REABERTURA` onde a regra exige `SANEAMENTO`. `app/chamados/tests/test_e4d_matriz_reds.py:328–400,438–455,655–671,1324–1362,1453–1474` · `app/docs/PROMPTS.md:2873,2950`

**Referências.** `T` = `app/chamados/tests/test_e4d_matriz_reds.py`. `M` = `app/chamados/tests/e4d_matriz_mutantes.py`. `C4` = `app/chamados/tests/test_e4c_f5_c4_red.py@48b24c39c`. `J` = `app/docs/JUIZES.md`. O cotejo das cópias mostrou diferenças somente em `T` e `M`. A cópia E4D é uma exportação sem `.git`, portanto a identidade do HEAD via Git fica [nao verificado]. Reexecução da pista, execução dos mutantes e dados de produção [nao verificado].

## 1. Procedentes da volta 1

### Os 14 motivos errados, um por um

| Caso | Situação e contraexemplo |
|---|---|
| **04** | **Curado mudando o que o caso pede.** Saiu o símbolo inventado e entrou o comando real, mas agora só se testa aplicação. Um apply que agrupe a prévia por competência e grave tudo sem foto, hash ou reversão por competência passa. `T:328–360` · `app/docs/PROMPTS.md:2873,2950` |
| **06** | **Curado mudando o que o caso pede.** Cobra M e zero ato. Uma escrita direta de M, uma vez por campo, sem entrada na lista e sem lápide passa. `T:655–671,232–236` · `app/docs/PROMPTS.md:2873` |
| **07** | **Curado mudando o que o caso pede.** Agora chama o comando efetivo e observa a ausência de ato, escrita e evento. O comando pode simplesmente ignorar a via e passar com a exceção ainda sem lápide. `T:362–373` · `app/docs/PROMPTS.md:2873` |
| **08** | **Curado mudando o que o caso pede.** A foto e os hashes vêm dos registros reais, mas o teste exige zero DP pela via comum. Uma implementação sem via autorizada passa, embora o caso peça deixar R/V/M, TXT e gravado intactos **e inserir a trilha** sob a autorização específica. `T:185–190,375–400` · `app/docs/PROMPTS.md:2873,2950` |
| **18 · contador** | **Curado quanto ao medidor.** A leitura agora é do I23 real, com controle positivo. Ainda passa uma implementação que isenta toda `REABERTURA` futura e conta `RECUSA` futura. A caracterização F5 exige contar uma `REABERTURA` futura após recuo do relógio. `T:499–503,536–543` · `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134` · `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319–325` |
| **21** | **Curado quanto à fixture.** O carimbo interposto passa pela trava real. O controle de sucesso ainda muda o motivo para `intervalo_volta`. Uma recusa que devolve sempre `ja_validada` para `orfao_14h`, após adquirir a trava, passa. `T:123,650–653,800–815` · `app/chamados/tests/_ato_da_fixture.py:16–27,47–51` |
| **23** | **Curado quanto à fixture e ao alvo do patch.** O alvo agora é o módulo usado pelo import local. Uma correção que recusa sempre `orfao_14h` depois da trava e aceita `orfao_22h` passa. A validação do controle positivo é simulada. `T:123,843–863` · `app/chamados/services/acoes_disputa.py:373–376` |
| **28** | **Não curado.** O controle `legado_sem_lastro` entra. A pergunta principal em `declaracao` segue excluída pelo seletor, antes da releitura e da compensação. Corrigir trava e releitura para todo item admitido ainda deixa o RED cair em `fato_materializado`. Alargar o seletor pode deixar passar uma decisão tomada pelo primeiro espelho, pois o mock responde `True` antes e depois da trava. `T:902–943` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44,66–77` |
| **30 · SQL** | **Curado mudando o que o caso pede.** Foi retirada a consulta artificial, mas a fila real só é conferida por PK. Usar o ato para admitir P e devolver ou ordenar pelas colunas cruas passa. A fixture também mudou de dois pares completos divergentes para `NULL/08:00` contra `10:05/09:00`. `T:1288–1300` · `app/chamados/juizes.py:1863–1868` |
| **30 · modal** | **Curado quanto à fixture.** O modal real chega ao par cru. Ainda passa um leitor que usa a DP somente quando a coluna de instante está nula. Ele falharia no par completo divergente pedido pelo caso. `T:208–220,1288–1293,1310–1311` · `app/templates/chamados/partials/modal_fio.html:217–220` · `app/docs/PROMPTS.md:2873` |
| **31** | **Curado mudando o que o caso pede.** Cobre os nascimentos automático e manual, mas perdeu o selo. Um segundo escritor de D ou do chamado que grave os mesmos valores, fora da lista, passa. `T:1324–1351` · `app/docs/PROMPTS.md:2873,2950` |
| **32** | **Curado mudando o que o caso pede.** A cobrança indevida de `tipo_resposta` na lista saiu. A abertura direta prova R/V/M vazios e zero DP nessa porta. A emissão pode contornar `P.abrir` e nascer com V sem derrubar o 32. O selo dos escritores de D e chamado também saiu do 31. `T:1324–1362` · `app/docs/PROMPTS.md:2873,2950` |
| **37** | **Curado mudando o que o caso pede.** Preserva R, observa M e protege a vizinha. Um descarimbo que faça as mesmas alterações fora da exceção listada passa. `T:1453–1474` · `app/docs/PROMPTS.md:2873,2950` |
| **42** | **Curado mudando o que o caso pede.** Saiu `marcar_estoque` e entrou uma resposta real que muda a projeção. Ficaram de fora a migração, a marca, a hora real da entrada nova e a reconstrução do que se sabia em T1. Congelar o marco no valor da primeira resposta passa sem guardar instante de entrada algum. `T:545–564` · `C4:948–979` · `app/docs/PROMPTS.md:2974` |

### Mordida, guardas e caso 20

- **Escritor único e lápides — não curado.** O vínculo entre `INSERT`, ato e PK melhorou. O 05 também compara o savepoint de V/M com o dos atos. A presença de `registrar` na pilha ainda aceita dois escritores chamados por ele, e o observador perde um `UPDATE` que alcance P por outro predicado que não `id =/IN`. No sentido inverso, um projetor único chamado depois de `registrar`, dentro do mesmo atomic, falha pela forma da pilha. O censo estático devolveu um `bulk_create` atribuído a DP, mas descarta os achados sem entidade. Por exemplo, a chamada genérica `Model.save(pergunta.decisoes.model(...))` escapa desse filtro. Nenhum desses mecanismos recupera as lápides retiradas. `T:72–109,222–236,488–493,1123–1134` · `app/core/censo_escritas.py:364–430,469–491` · `app/chamados/models.py:4609`
- **Mutantes — não curado.** No 05, `M` troca o atributo de `materializacao`, enquanto a resposta chama o alias já importado em `disputa_emissao`. No 30/modal, `M` instala uma função de dois parâmetros e `T` chama com `request=req`. O primeiro pode deixar a escrita extra de fora. O segundo produz `TypeError` antes de comparar o par. No 31, fechar D após nascer prova a asserção de estado, mas nada diz sobre um escritor fora da lista que preserve esse estado. Mordida em execução [nao verificado]. `M:89–99,124–143,303–315,328–332` · `app/chamados/services/disputa_emissao.py:12,1596` · `T:215,1075–1078,1119–1120,1327–1334`
- **20 — curado quanto à referência e ao empate.** A docstring nomeia a caracterização invertida, o teste antigo permanece e a nova matriz verifica empate e posterioridade. A identidade do pedido saiu. Reusar uma chave fixa por ausência passa mesmo que dois pedidos distintos devam ser separados. `T:1144–1189` · `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:364–380` · `app/docs/PROMPTS.md:2950`

| Guarda | Situação da lacuna da volta 1 |
|---|---|
| **13** | **Curada.** O snapshot integral de DP pega a alteração de texto ou motivo que escapava ao teste por tipo e autor. `T:181–183,731–748` | [nao verificado]
| **15** | **Parcial.** O controle aberto elimina a recusa incondicional. A entrega é interposta antes da aquisição da trava, então uma releitura feita após a interposição e ainda fora da trava pode passar. `T:239–252,750–776` |
| **19** | **Curada para o contraexemplo anterior.** A automática que sempre recusa agora falha no controle em branco. Uma guarda especializada no motivo do caso principal ainda passa, pois o controle usa outro motivo. `T:123,778–798` |
| **21, 23** | **Parciais.** Fixtures reparadas e caminhos positivos adicionados, com os escapes por motivo descritos acima. `T:650–653,800–815,843–863` |
| **22** | **Curada para o contraexemplo anterior.** O controle do mesmo motivo precisa validar e plantar. Como ele usa outro dia, uma recusa fixa para o dia inicial ainda passa. `T:123,817–841` |
| **24** | **Curada quanto à ordem e ao carimbo de E.** A ordem literal saiu e a foto de E passou a ser exigida. Ainda se exige exatamente o conjunto `{E,Q}`. Uma passagem que preserve E e produza os mesmos efeitos sobre Q, mas reporte só Q na lista, falha. `T:1191–1218` | [nao verificado]
| **27** | **Curada para a instância antiga.** A FK muda antes da trava, o juiz simulado decide por essa FK e o novo mutante conserva a releitura real, mas entrega a instância antiga ao juiz. Ainda se pode usar uma releitura atualizada só da FK e julgar outros campos obsoletos. A lápide de `fantasma` segue sem selo. `T:865–900` · `M:65–69` · `app/chamados/reconciliador.py:1265–1298` |
| **36** | **Parcial.** A irmã viva em outra competência e o pai aberto entraram. A asserção que cai reutiliza a instância e o mesmo `agora`. A chave de `_pv_memo` não contém a tranca. Uma consulta fresca pode dar `False` enquanto a instância anterior devolve `True`. A causa dinâmica exata da ocorrência [nao verificado]. `T:1424–1451` · `app/chamados/juizes.py:889–908,931–952` · `app/ponto/services/fechamento.py:1431–1453` |
| **43** | **Curada para a reescrita anterior.** O retry avança o relógio e acusa `UPDATE` de P, DP ou D mesmo com os mesmos valores. A contagem de eventos ainda deixa passar a troca de um evento por outro com o mesmo total. `T:173–174,1507–1527` |
| **46** | **Curada para a ata de fixture.** Depois do plantio, a ata recarregada precisa trazer 480 realizados. Plantar sem relavrar deixa a ata inicial em zero e falha. A mordida do mutante que corta o signal [nao verificado]. `T:1545–1570` · `M:79–85` · `app/ponto/signals.py:85–93,136–140` |

## 2. As 51 ocorrências da pista de 03:06

A lista anexa contém **51 falhas em 29 métodos**, com `errors=0` no placar fornecido. `T` declara 45 métodos numerados. Resultado individual dos 16 métodos sem ocorrência e placar dos mutantes [nao verificado]. A tabela classifica a primeira asserção que caiu, inclusive quando ela só cobre uma parte do caso.

| Caso · ocorrências | A asserção caiu pelo motivo certo? |
|---|---|
| **05 · 1** | **Sim.** Falta `VALIDACAO` física de Q. O caminho carimba V/M e registra `ABSORCAO`. `T:1123–1124` · `app/chamados/services/materializacao.py:1107–1132` |
| **20 · 2** | **Sim.** Tanto no empate quanto em T2, a recusa humana foi aceita, mas Q ficou fora de `recusadas`. Identidade do pedido segue sem teste. `T:1150–1153,1162–1189` | [nao verificado]
| **25 · 1** | **Sim.** M fica de pé apesar da `ABSORCAO` vigente. O seletor atual exige a âncora que essa entrada dispensa. `T:1223–1247` · `app/chamados/services/materializacao.py:1195–1212` |
| **26 · 1** | **Sim, para preservar o legado.** A rejeição muda R/V/M. Uma correção que deixe tudo intacto e nunca encaminhe Q ao admin passa. `T:1254–1272` · `app/chamados/services/materializacao.py:1235–1279` · `app/docs/PROMPTS.md:2873` |
| **01, 02 · 2** | **Sim, pela falta do ato.** O comando calcula a prévia de R/V, mas seu apply chama a porta de colisões e silêncio. As ocorrências ainda não chegam ao zero evento, e ambas aceitam ato novo com proveniência errada. `T:264–295` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:486–516,628–631` |
| **03 · 2** | **Sim.** No vigente, a foto oferece retroativamente uma validação. No controle compensado, falta a validação nova de `09:00` após o apply. A segunda ocorrência prova a ausência do apply nesse controle. `T:311–326` · `app/chamados/models.py:4437–4447` |
| **04 · 1** | **Não para a reversão.** Falta `VALIDACAO` no apply. Uma aplicação irreversível pode ficar verde. `T:328–360` | [nao verificado]
| **11 · 1** | **Sim, pelo ato vazio ausente.** A assinatura atual aceita a chamada e a porta não gera linha. Uma porta que insira outra linha a cada repetição passa, pois há uma única chamada. `T:402–416` · `app/chamados/services/disputa_emissao.py:1695–1715` |
| **12 · 4** | **Sim, uma por FK.** `validada_por`, `materializada_por`, `ato_por` e `validada_original_por` estão isoladas e cada exclusão deveria ser protegida. Manter autoria histórica nula sem marca ainda passa. `T:418–436` · `app/chamados/models.py:3804–3829,4347–4353` |
| **14 · 1** | **Não.** Faltou a `REABERTURA` que o teste passou a exigir. A implementação correta com `SANEAMENTO`, lei em campo próprio e referência à resposta antiga também falha nessa asserção. A resposta mais nova da fixture só é criada depois do saneamento. `T:438–455` · `app/chamados/services/validacao.py:450–455,472–489` · `app/docs/PROMPTS.md:2873,2950` |
| **16 · 1** | **Sim.** A porta aceita a resposta futura. Se a exceção viesse depois de um `INSERT` bem-sucedido e revertido, o observador ainda o registraria. A ordem da projeção durante uma tentativa revertida fica sem verificação equivalente. `T:53–65,457–470` · `app/chamados/models.py:4598–4623` |
| **17 · 3** | **Sim.** `save` aceita DP futura com e sem `force_insert`, e a terceira falha mostra os dois `INSERT`s. O selo AST vem depois e essa pista não o alcança. `T:472–493` · `app/chamados/models.py:4405–4408` |
| **18 · C6 e modal · 2** | **Sim.** O C6 dá `False` em T1 e o modal não apresenta o par que ainda deveria valer. A tarefa, que devolve uma razão, não é a asserção `False != True`. `T:522–534` · `app/chamados/juizes.py:1499–1521` · `app/chamados/services/tarefa_do_dia.py:389–429` |
| **18 · I23 · 1** | **Depende da identidade do contador.** A pista mede a inclusão do compensador futuro pelo I23 real. O I23 vigente foi definido para incluí-lo. O brief exige `ato_futuro=0` no restore, mas atribuir esse contrato novo ao I23 [nao verificado]. Isentar toda `REABERTURA` passa no novo teste e quebra a caracterização F5. `T:536–543` · `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134` · `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319–325` |
| **42 · 1** | **Sim, pelo marco.** O leitor troca `10:00` por `10:30` depois da resposta retrodatada. A asserção de lavra vem depois e a ocorrência não prova entrada ou migração. `T:545–564` · `app/chamados/veredito_lavrado.py:197–209` |
| **44 · 1** | **Não pelo retry.** A primeira aplicação já deixou zero DP. A segunda nem foi alcançada. O teste repete o comando que recalcula o acervo e pode passar com uma porta que duplique o mesmo payload quando o recebe novamente. `T:199–200,566–587` | [nao verificado]
| **45 · autor, lastro, texto novo e FK nula · 4** | **Sim.** Erros de contagem A/B, coerentes com o vigia atual de presença. Um vigia que só compare diferenças observadas de autor e texto ainda passa sem detectar uma diferença exclusivamente de instante. `T:593–632` · `app/chamados/services/vigia_ato_x_coluna.py:8–25` |
| **45 · texto de estoque, absorvida histórica e quatro reconciliadas · 6** | **Interface C4.** As seis comparam `None` à chave `estoque` exigida. Elas não demonstram o valor histórico medido. Devolver zeros fixos na chave elimina cinco ocorrências e ainda perde a divergência do texto histórico. Alimentar um PK de cada vez também deixa escapar erros de precedência em acervo misto. `T:613–640` · `app/chamados/services/vigia_ato_x_coluna.py:21–25` · `C4:559–572,603–607` |
| **09, 10, 29 · 3** | **Sim.** Faltam `VALIDACAO`, `RESPOSTA` e `REABERTURA` físicas, respectivamente. As asserções posteriores de autoria, via e ordem ainda ficam atrás desses REDs. `T:683–700,712–723,957–964` · `app/chamados/services/disputa_emissao.py:2112–2122,2189–2202` · `app/chamados/services/relavra_via.py:230–261` |
| **28 · 1** | **Não para a trava ou compensação.** A pergunta ficou em `declaracao` porque nem entrou no seletor. A exigência de elegibilidade pode justificar um RED próprio. `T:906–943` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44` |
| **39, 40, 41 · 4** | **Sim.** Caem pela ordem física de V, `recusa_motivo` e R, esta última nos dois retornos de plantio recusado. Um segundo escritor de `validada_por` ou `resposta_colab` em ramo não observado ainda escapa, e `RESULTADO` fica depois do RED do 41. `T:974–1035` · `app/chamados/services/validacao.py:164–178,995–1000` · `app/chamados/services/disputa_emissao.py:1559–1567` |
| **30 · SQL, API, modal, raio-x · 4** | **Sim para os observáveis exercitados.** SQL perde o PK, API e modal expõem a coluna e raio-x põe o chamado no grupo pendente. Só API e modal comparam o par exibido. SQL pode conservar a ordenação crua, e raio-x pode creditar por qualquer resposta histórica, inclusive uma já compensada. `T:1298–1314` · `app/chamados/juizes.py:1863–1868` · `app/api/views.py:1161–1165` · `app/templates/chamados/partials/modal_fio.html:217–220` · `app/chamados/services/raiox.py:135–158` |
| **34, 38 · 2** | **Sim.** `SILENCIO` entra depois de M. A verificação da cascata do pai está depois da primeira asserção. Um pai fechado por `UPDATE` direto ainda satisfaria a checagem de status. `T:1364–1381,1476–1505` · `app/chamados/models.py:2184–2207` · `app/chamados/signals.py:20–46` |
| **35 · 1** | **Sim.** O PK eleito difere do esperado. A vizinha de outro chamado e o empate já estão no cenário. Como o primeiro assert cai, as preservações posteriores ficam [nao verificado]. `T:1383–1418` · `app/chamados/services/disputa_emissao.py:539–542,582–605` |
| **36 · 1** | **Inconclusiva como RED da regra.** A ocorrência é no C6 após trancar, mas o teste admite o contraexemplo do memo descrito acima. A pista não mostra o resultado em P recarregada em nova instância. `T:1434–1447` · `app/chamados/juizes.py:889–908,931–952` |

## 3. Fix4 e o cartão

O **fix4 em si** preserva o que 18 e 30 pedem. O 18 passou a ligar a disputa e a pergunta ao cartão que o montador real usa. O 30 já tinha esses vínculos. `contexto_parcial_fio` monta chamados, disputa e permissões que o template consulta. A mudança de regra do 30 veio no recorte do teste, que injeta `NULL/08:00` e tira a asserção C6. `T:208–220,505–515,1283–1314` · `app/chamados/services/fio.py:251–254,305–362,573–603` · `app/templates/chamados/partials/modal_fio.html:123,157–160`

**Cartão ausente não passa em nenhum dos dois casos.** O `partition` produz `''`, mas a pergunta tem texto e `assertIn` falha antes que o helper devolva `(None, None)`. A lacuna é **dentro do cartão**. Uma irmã usa o mesmo texto, o helper pega a primeira `Resposta:` e não identifica a pergunta pelo PK. Em T2 do 18, a pergunta alvo pode sumir e uma irmã muda de mesmo título satisfazer o teste. Em T1 ou no 30, a resposta esperada pode vir da irmã errada. `T:123–127,216–220,532–534,1310–1311` · `app/templates/chamados/partials/modal_fio.html:157–160,217–220`

O 18 também perdeu a prova de que os três leitores enxergam o **mesmo par**. O C6 compara só um booleano e a tarefa só a razão. Trocar `08:00` por `09:00` ainda diverge da luz `19:00` e pode conservar o booleano, enquanto a tarefa pode manter `aguarda_validacao` com ação ou dono errado. `T:507–509,522–534` · `app/chamados/juizes.py:1428–1454,1521–1533` · `app/chamados/services/tarefa_do_dia.py:165–170` · `J:81–95`

## 4. Conflitos com c4red4

| Recorte da volta 1 | Situação na volta 2 |
|---|---|
| **01/02/44 · entrada e evento** | **Curado mudando o que os casos cobrem.** O backfill pelo comando não se opõe ao evento da resposta pública. As asserções de origem e entrada inicial sumiram. Gravar `entrada_em=ato_em` no primeiro apply e conservá-lo no retry passa em E4D. C4 exige `ESTOQUE/MARCO` na foto histórica e `ATO_NOVO` com um evento para a resposta pública. `T:264–295,566–587` · `C4:478–504` |
| **02/04/08/44 · admissibilidade** | **Curado mudando o que os casos cobrem.** 02 ficou antes do marco. 04 usa resposta vigente correspondente, sem a divergência que C4 manda reprojetar. 08 te

[truncado em 20000 de 31735 caracteres]

## 27/11 03:15 · sessao fc98a769dbc1 · E4C-F5-INSTANTE · verificador · origem=Code

Os 28 registros de `falhas.txt` chegam a asserções de comportamento. Os contraexemplos que reprovam a c4red4 estão na prova de atomicidade, na checagem das mensagens de estoque, no alcance dos três escritores e no arnês. Em particular, o teste de estoque pode obter a foto intacta porque **a transação da fixture** desfez a escrita que a porta deveria ter desfeito. `falhas.txt:1` `falhas.txt:55` `test_e4c_f5_c4_red.py:326` `test_e4c_f5_c4_red.py:332` `e4c_f5_c4red2_mutantes.py:48` [nao verificado]

Referências dos testes da cópia: [REDs c4red4](/home/ronald/codex_ws/48b24c39c6bd6686d0e0f066a77187d4caddd34c/app/chamados/tests/test_e4c_f5_c4_red.py:1), [mutantes](/home/ronald/codex_ws/48b24c39c6bd6686d0e0f066a77187d4caddd34c/app/chamados/tests/e4c_f5_c4red2_mutantes.py:1) e [c1](/home/ronald/codex_ws/48b24c39c6bd6686d0e0f066a77187d4caddd34c/app/chamados/tests/test_e4c_f5_instante_c1.py:1). Os dois blobs de teste conferem com `diff.txt`. O SHA da árvore inteira, a identidade da c1 entre commits e o verde integral da c1 ficam [nao verificado]. O anexo lista as falhas, sem o resumo completo da pista. `diff.txt:1` `falhas.txt:1` `falhas.txt:55`

## 1. Os sete itens do opus 5d

| Item | Estado e contraexemplo |
|---|---|
| **1. Falso zero** | **Fechado** para os três controles históricos e a separação da escrita retroativa. O teste vivo de precedência só fornece A novo e B de estoque. A presença de B novo no alarme e no DRY ficou no skip, embora seja possível produzi-la sem a lei. A razão do skip cita “item 3”; o estacionamento é o item 1. `opus.md:19` `test_e4c_f5_c4_red.py:551` `test_e4c_f5_c4_red.py:559` `test_e4c_f5_c4_red.py:574` `test_e4c_f5_c4_red.py:616` | [nao verificado]
| **2. Nascimento** | **Fechado.** Saiu `b_nascida`. A c1 ainda exige B de estoque quando a pergunta nasce em 13/11 com instante de resposta anterior ao marco de 01/11. Reintroduzir “nasceu após o marco ⇒ novo” volta a quebrar esse caso. `test_e4c_f5_c4_red.py:579` `test_e4c_f5_c4_red.py:592` `test_e4c_f5_instante_c1.py:49` `test_e4c_f5_instante_c1.py:83` |
| **3. Concorrência** | **Aberto na prova de “não grava”.** O mesmo worker espera no PostgreSQL, segue após o commit e exige a mensagem exata de `ENTREGUE`. A ordem Empresa → Colaborador está no chamador da fixture, sem troca da guarda e sem `lock_timeout`. Entretanto, `assertRaisesRegex` envolve `_registrar_estoque` por fora de seu `atomic`. Uma porta que insira e só depois lance o `ValueError` correto passa com a foto intacta graças ao rollback do chamador. `test_e4c_f5_c4_red.py:650` `test_e4c_f5_c4_red.py:671` `test_e4c_f5_c4_red.py:689` `test_e4c_f5_c4_red.py:737` `app/folha/export.py:556` |
| **4. Resposta futura** | **Fechado.** A comparação usa a razão e as ações da face pública por pergunta, e M05 saiu. O estímulo ainda depende de `foto_da_lavra`. Uma cura que mova a foto para antes da montagem da candidata pode preservar a face pública e quebrar o ponto de observação. `test_e4c_f5_c4_red.py:226` `test_e4c_f5_c4_red.py:276` `app/chamados/services/tarefa_do_dia.py:461` `e4c_f5_c4red2_mutantes.py:430` |
| **5. Carimbo** | **Fechado mudando o que a regra pede.** O teste exige uma única saída e o de competência entregue exige `ENTREGUE`, foto integral persistida e `validada_em` em memória. A exigência nova é que **cada uma das três portas**, já com Empresa travada pela fixture, promova o passivo a estoque na primeira escrita. Q0.2 amarra a linha da coluna que **o carimbo escreve** à mesma transação e ao mesmo instante. O próprio código reserva o ato de estoque a aval específico e rejeita lote misto. A autorização para essa promoção automática pelas três portas fica [nao verificado]. Contraexemplo de cobertura: uma porta que só consiga promovê-lo com a Empresa pré-travada passa nesse teste e pode falhar na chamada normal. O novo ato também pode ter `entrada_em` ou autor incorretos e passar. `test_e4c_f5_c4_red.py:444` `test_e4c_f5_c4_red.py:467` `test_e4c_f5_c4_red.py:478` `app/chamados/models.py:3521` `app/chamados/models.py:4392` `app/chamados/models.py:4761` |
| **6. `suppress` e mensagens** | **Fechado mudando o que a regra pede.** Sai o `suppress`, e o caso de proveniência exige mensagem exata. O teste troca o critério anterior de fidelidade da foto por rejeição obrigatória. Um registrador que descarte o motivo falso e grave a foto canônica satisfaz a fidelidade da L-007, mas falha no teste novo. Além disso, os outros subcasos de elegibilidade continuam aceitando fragmentos como `duplicada`, `reproduz a coluna` e `ENTREGUE`. `opus.md:11` `opus.md:28` `app/docs/LEIS.md:65` `test_e4c_f5_c4_red.py:379` `test_e4c_f5_c4_red.py:409` `test_e4c_f5_c4_red.py:426` `app/chamados/models.py:4803` |
| **7. Arnês** | **Aberto.** Há instâncias novas, controle inicial, mutação, controle no `finally`, e savepoints nos subcasos de M11. M25/M26 têm contextos delimitados e M12/M13 usam patch de atributo. Ainda há mutantes que atingem só a fixture ou cujo veredito termina antes do efeito anunciado. A prova diferencial dos 27 mutantes sobre a cura fica [nao verificado]. `e4c_f5_c4red2_mutantes.py:26` `e4c_f5_c4red2_mutantes.py:200` `e4c_f5_c4red2_mutantes.py:230` `e4c_f5_c4red2_mutantes.py:371` `e4c_f5_c4red2_mutantes.py:395` `test_e4c_f5_c4_red.py:433` |

### Procedentes da c4red3, um a um

| Achado anterior | Estado na c4red4 |
|---|---|
| Par do carimbo | **Fechado.** Os quatro subcasos chegam à recusa nomeada, incluindo só uma validação e resposta divergente sem validação. `test_e4c_f5_c4_red.py:93` `test_e4c_f5_c4_red.py:116` |
| Duas falhas de foto | **Fechado.** A exceção injetada é esperada e o vermelho vem da foto e da leitura que vazaram. `test_e4c_f5_c4_red.py:126` `test_e4c_f5_c4_red.py:137` `falhas.txt:53` |
| Absorção | **Fechado** quanto ao par antes e depois da escrita. Há dois vermelhos por PK e um controle separado para T1 com o relógio movido a T2. Os motivos agora são literais distintos para a recusa da porta e para a reconferência depois do carimbo. `test_e4c_f5_c4_red.py:169` `test_e4c_f5_c4_red.py:188` `test_e4c_f5_c4_red.py:200` `test_e4c_f5_c4_red.py:214` |
| Horizonte | **Fechado.** Cai a decisão pública da validação candidata. A resposta futura passa a controle público, como no item 4. `test_e4c_f5_c4_red.py:259` `test_e4c_f5_c4_red.py:290` `falhas.txt:31` |
| Falso zero e colisão por nascimento | **Fechado** para a colisão. **Aberto** para a prova viva de B novo no alarme e DRY. `test_e4c_f5_instante_c1.py:83` `test_e4c_f5_c4_red.py:559` `test_e4c_f5_c4_red.py:592` |
| Fronteira e classe A | **Fechado** para recusar os quatro payloads e admitir a resposta compensada de outro instante. **Aberto** se “encaminha à reprojeção” significa entregá-la a um consumidor: o teste aceita o texto `reproje` e a foto intacta. A c1 prova a listagem do DRY com outra fixture. `test_e4c_f5_c4_red.py:345` `test_e4c_f5_c4_red.py:369` `test_e4c_f5_c4_red.py:379` `test_e4c_f5_instante_c1.py:142` | [nao verificado]
| Conversa e proveniência | Conversa **fechado**. Proveniência **fechado mudando o que a regra pede**, pela recusa obrigatória descrita no item 6. Os dois motivos são subcasos de uma única linha, de modo que o teste também não demonstra rejeição de lote com uma linha válida antes da inválida. `test_e4c_f5_c4_red.py:393` `test_e4c_f5_c4_red.py:409` | [nao verificado]
| Competência entregue, origem e emissão | **Fechado** para a exigência explícita de `ENTREGUE`, preservação do carimbo e emissão observada. **Aberto** para a entrada e a autoria do ato novo. A linha nova é fatiada nos quatro primeiros campos, e a autoria nem é selecionada na consulta. `test_e4c_f5_c4_red.py:444` `test_e4c_f5_c4_red.py:478` `test_e4c_f5_c4_red.py:504` |
| Colisão c1 × travas | **Fechado.** A c1 chama `registrar` diretamente e exige `ColaboradorNaoTravado`. A c4red4 fornece as duas travas no chamador. A consequência é que a c4red4 prova esse chamador de teste. `test_e4c_f5_instante_c1.py:173` `test_e4c_f5_c4_red.py:650` `app/chamados/models.py:4737` |
| Colisão c1 × recusa íntegra e segunda tentativa | **Fechado** para o conflito anterior. Ambos exigem sucesso de primeira no cenário aberto. Resta a diferença de pré-condições: a c1 fornece só a trava do dono e seu marco de fixture de janeiro faz a resposta de outubro cair em **B novo** pela regra atual. A c4 fornece Empresa e um passivo anterior ao marco. O verde desses dois caminhos sobre a mesma cura fica [nao verificado]. `test_e4c_f5_instante_c1.py:317` `cortes_f2_f9_apoio.py:31` `cortes_f2_f9_apoio.py:40` `cortes_f2_f9_apoio.py:61` `test_e4c_f5_c4_red.py:44` `test_e4c_f5_c4_red.py:462` `app/chamados/models.py:4615` |
| K6 antes da trava | **Fechado** para o defeito de testar duas tentativas diferentes. M27 agora captura K6 antes da espera e devolve a foto antiga à consulta posterior. A ausência de escrita própria da porta continua **aberta** pelo rollback da fixture. `e4c_f5_c4red2_mutantes.py:402` `test_e4c_f5_c4_red.py:742` `app/chamados/models.py:4799` |
| Item 13 | **Aberto** nos dois pontos antigos. O inverso usa TXT invalidado com **FM ainda aberto**. A devolução ainda começa com validação pela fixture e chama o helper diretamente. Um defeito na ligação `validar_pergunta` → devolução passa pelo controle M29. Outro contraexemplo é alterar `aprovado_em` do FM: esse campo fica fora do hash que a foto compara. `test_e4c_f5_c4_red.py:775` `test_e4c_f5_c4_red.py:790` `test_e4c_f5_c4_red.py:837` `test_e4c_f5_c4_red.py:918` `app/chamados/services/validacao.py:835` `app/ponto/portas/regen_exportada.py:37` |
| 0058 e L-130 | **Fechado** para empate, `+1 µs`, reexecução do recorte correto e a REABERTURA marcada da c1. **Aberto** para reparar uma marcação ampla já gravada: uma linha pós-marco previamente sobrescrita com `entrada_em=MARCO` é indistinguível, para a 0058, de uma linha corretamente marcada. `test_e4c_f5_c4_red.py:953` `test_e4c_f5_c4_red.py:969` `test_e4c_f5_instante_c1.py:234` `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16` |

## 2. As 28 falhas do anexo

A numeração segue a ordem de `falhas.txt`. Nos casos `ValueError not raised`, o vermelho é a aceitação indevida pela porta. A asserção posterior de foto intacta ainda está por executar nesses subcasos. `test_e4c_f5_c4_red.py:332` `test_e4c_f5_c4_red.py:451` `falhas.txt:5`

| Nº | Efeito que derrubou a asserção |
|---|---|
| 1–2 | **1** Par adulterado depois do carimbo: irmã ausente das recusadas. **2** Só a resposta diverge: irmã também ausente. `falhas.txt:1` `test_e4c_f5_c4_red.py:171` `test_e4c_f5_c4_red.py:200` |
| 3 | Carimbo com competência entregue deixa de emitir a recusa. A fixture registra o TXT para a competência retornada pelo dia do fato. `falhas.txt:5` `test_e4c_f5_c4_red.py:338` `test_e4c_f5_c4_red.py:451` |
| 4–6 | **4** Carimbar, **5** validar, **6** responder: cada caminho chega com duas linhas e falha na tupla histórica. Em todos os três, o código monta a foto sem marcar `ESTOQUE` e sem fornecer o marco. `falhas.txt:7` `test_e4c_f5_c4_red.py:480` `app/chamados/models.py:3473` `app/chamados/models.py:3526` `app/chamados/services/validacao.py:170` `app/chamados/services/disputa_emissao.py:1522` `app/chamados/services/disputa_emissao.py:1565` |
| 7 | Classe A com ato vigente divergente entra sem a recusa de reprojeção. `falhas.txt:13` `test_e4c_f5_c4_red.py:373` |
| 8–9 | **8** Motivo falsamente atribuído ao carimbo e **9** motivo atribuído ao DP entram sem recusa. A chave da comparação omite `ato_motivo`. É efeito da política de recusa escolhida pelo teste. `falhas.txt:15` `test_e4c_f5_c4_red.py:411` `app/chamados/models.py:4785` |
| 10 | Conversa com dia civil entra sem recusa: a consulta de competência usa `data_evento` preenchido. `falhas.txt:19` `test_e4c_f5_c4_red.py:400` `app/chamados/services/materializacao.py:803` |
| 11–14 | **11** Resposta no marco, **12** validação no marco, **13** resposta depois, **14** validação depois. Os quatro chegam à ausência da recusa depois de o comparador apontar B novo. A ordem dos atos vem do laço; o identificador literal de cada ato foi truncado no anexo. `falhas.txt:21` `falhas.txt:27` `test_e4c_f5_c4_red.py:349` `test_e4c_f5_c4_red.py:364` |
| 15 | Uma linha de resposta e dois eventos da célula. O `save` dispara o outbox e `registrar` também acorda a célula. `falhas.txt:29` `test_e4c_f5_c4_red.py:502` `app/chamados/signals.py:252` `app/chamados/models.py:4755` |
| 16 | Validação futura candidata muda a decisão pública de `aguarda_validacao` para `devolvida_ao_colaborador`. A vista corta a coluna futura, mas delega `_atos_do_carimbo` à candidata; `leitura` anexa esse pendente sem teto. Isso ativa a leitura da recusa anterior pela cauda de respostas que mantém validação. `falhas.txt:31` `test_e4c_f5_c4_red.py:263` `app/chamados/services/tarefa_do_dia.py:160` `app/chamados/services/tarefa_do_dia.py:416` `app/chamados/models.py:4549` |
| 17–20 | **17** PK 37 na materialização, **18** PK 38 na materialização, **19** PK 37 no passivo, **20** PK 38 no passivo. As recusas e a preservação passam antes da asserção que encontra a trilha vazia. `falhas.txt:33` `falhas.txt:39` `test_e4c_f5_c4_red.py:863` `test_e4c_f5_c4_red.py:877` |
| 21–22 | **21** PK 39, validação vetada no cenário da recusa, **22** PK 41, TXT vigente com FM aberto. Falta foto de auditoria após o veto. O nº 21 diz respeito à **validação vetada**; a asserção do instante da `RECUSA` fica mais adiante. `falhas.txt:41` `test_e4c_f5_c4_red.py:829` `test_e4c_f5_c4_red.py:893` `test_e4c_f5_c4_red.py:906` |
| 23–26 | **23** Validação única, **24** duas validações, **25** resposta divergente com validação, **26** resposta divergente sem validação. Todos obtêm `(True, None)`; a guarda vigente compara presença de validação. `falhas.txt:45` `falhas.txt:51` `test_e4c_f5_c4_red.py:93` `app/chamados/models.py:3499` `app/chamados/models.py:4646` |
| 27–28 | **27** Falha na foto da projeção, **28** falha na foto da lavra. Ambas chegam à comparação da instância e da leitura após a exceção esperada. As duas chamadas de foto ficam fora do bloco que desfaz a escrita em memória. `falhas.txt:53` `test_e4c_f5_c4_red.py:126` `test_e4c_f5_c4_red.py:141` `app/chamados/models.py:3473` `app/chamados/models.py:3510` |

**O instante do run é efeito do código.** A primeira data mostrada na tupla truncada é `ato_em=18/10 09:00`, igual dos dois lados. O marco esperado é `entrada_em=20/10 11:00`, no último campo. A linha histórica entra pelo caminho padrão `origem=ato`, com `entrada_em=Now()` do banco. O `freeze_time` controla o relógio Python. A representação `FakeDatetime` no diff não explica a desigualdade. O valor literal completo de `entrada_em` nas três falhas fica [nao verificado]. `falhas.txt:7` `test_e4c_f5_c4_red.py:44` `test_e4c_f5_c4_red.py:69` `test_e4c_f5_c4_red.py:478` `cortes_f2_f9_apoio.py:31` `app/chamados/models.py:4405` `app/chamados/models.py:4495` `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16`

## 3. Arnês

- **M14 e M25 mordem a fixture.** M14 inverte duas chamadas escritas no teste. M25 antepõe a trava do dono à entrega da fixture. Um chamador real que adquira as travas em outra ordem fica fora da prova. M27, por sua vez, atinge o K6 atual porque `_conferir_estoque` importa `competencia_do_fato` na hora da chamada. Uma cura que passe a usar um alias já importado pode deixar o patch de M27 sem efeito. `e4c_f5_c4red2_mutantes.py:230` `e4c_f5_c4red2_mutantes.py:371` `e4c_f5_c4red2_mutantes.py:402` `test_e4c_f5_c4_red.py:650` `app/chamados/models.py:4799`
- **M12 não chega ao alarme sob mutação.** O patch também troca o alias do comando, mas a primeira comparação de `contar` derruba o método antes de executar `vigiar_fio_morto`. M13 chega à comparação do quadro. Ambos dependem de um trecho literal de fonte para montar; uma refatoração equivalente pode produzir erro de montagem antes da comparação de negócio. `e4c_f5_c4red2_mutantes.py:200` `e4c_f5_c4red2_mutantes.py:214` `e4c_f5_c4red2_mutantes.py:221` `test_e4c_f5_c4_red.py:605` `test_e4c_f5_c4_red.py:611` `test_e4c_f5_c4_red.py:627`
- **M23 tem `finally`, com risco de reaplicar a adulteração.** A lista guarda o instante lido *depois* da escrita adulterada. Se um savepoint interno já reverteu aquela tentativa, o `finally` pode escrever esse instante de volta sobre a linha que sobreviveu. O rollback do `TestCase` envolve o ensaio inteiro, e o controle posterior começa com outra fixture. Isso não demonstra a restauração da mesma linha. M11 e M26 têm isolamento por subteste; M25/M26 têm desmontagem dos patches sob contexto. `e4c_f5_c4red2_mutantes.py:35` `e4c_f5_c4red2_mutantes.py:56` `e4c_f5_c4red2_mutantes.py:333` `e4c_f5_c4red2_mutantes.py:346` `e4c_f5_c4red2_mutantes.py:371` `e4c_f5_c4red2_mutantes.py:395` `test_e4c_f5_c4_red.py:433` `test_e4c_f5_c4_red.py:463` `app/chamados/services/materializacao.py:1074`
- **O veredito identifica texto de asserção, sem identificar o subcaso ou a gravação.** M07 afeta as duas validações, mas um único vermelho com a mensagem compartilhada basta. M11 aceita qualquer subconjunto dos casos com o mesmo rótulo. M20 conta eventos, sem fixar se a duplicata vem do `post_save` ou de `registrar`. E qualquer RED vermelho na base para no controle inicial, antes de montar o mutante. A pista fornecida não evidencia nenhum ciclo verde → mordida → verde. `e4c_f5_c4red2_mutantes.py:48` `e4c_f5_c4red2_mutantes.py:146` `e4c_f5_c4red2_mutantes.py:289` `test_e4c_f5_c4_red.py:365` `test_e4c_f5_c4_red.py:433` `test_e4c_f5_c4_red.py:504` `falhas.txt:1` [nao verificado]

## 4. O que pode sair do skip

**B novo no alarme e no DRY independe da lei.** Contraexemplo de cobertura: resposta válida sem linha com `respondida_em=MARCO`, ao lado de outra anterior ao marco. O comparador já separa os dois, a c1 já prova B novo no contador e o comando inclui B positivo no alarme. Outra fixture independente pode combinar, no mesmo PK, resposta anterior ao marco e validação no marco sem linhas, cobrando que B novo prevaleça uma vez. Pela inspeção, esses são controles verdes esperados. Falha atual dessa publicação [nao verificado]. O RED independente de **aceitar** o B novo como estoque já está vivo nos quatro casos de fronteira. `test_e4c_f5_c4_red.py:345` `test_e4c_f5_c4_red.py:572` `test_e4c_f5_c4_red.py:616` `test_e4c_f5_instante_c1.py:102` `app/chamados/models.py:4615` `app/chamados/services/vigia_ato_x_coluna.py:24` `app/chamados/management/commands/vigiar_fio_morto.py:213` `app/chamados/management/commands/estoque_ato_x_coluna.py:44`

O que depende da lei é marcar como nova a **escrita** retroativa ou a limpeza sem ato. `b_antiga`, depois da escrita, e `estoque` preservam os mesmos insumos relevantes para o comparador, embora o skip espere `True` e `False`. A data de abertura é da disputa, e a coluna não guarda sua entrada. O texto da linha `lei:` mencionada no enunciado não consta no topo do `RELATO.md` da cópia; o registro externo fica [nao verificado]. `test_e4c_f5_c4_red.py:516` `test_e4c_f5_c4_red.py:583` `test_e4c_f5_c4_red.py:597` `app/chamados/models.py:1921` `app/chamados/models.py:4600` `app/docs/RELATO.md:1`

## 5. Quem escreve e quem lê

| Campos e saídas | Escrita e leitura |
|---|---|
| `respondida_em`, `resposta_colab`, `validada_em`, `validada_por` | Resposta humana, automática e pela trilha escrevem a resposta. Validação e carimbo escrevem a assinatura; nova resposta, devolução, recusa/reabertura e L-130 limpam os pares conforme o ato. Leem o comparador real, `leitura` com passivo, C6, C21, a face SQL de validação e `verbos` pelo dia civil. `app/chamados/services/disputa_emissao.py:1529` `app/chamados/services/disputa_emissao.py:1661` `app/chamados/services/disputa_emissao.py:1759` `app/chamados/services/disputa_emissao.py:1793` `app/chamados/services/validacao.py:171` `app/chamados/services/validacao.py:307` `app/chamados/services/validacao.py:485` `app/chamados/models.py:3474` `app/chamados/models.py:3726` `app/chamados/models.py:4530` `app/chamados/models.py:4589` `app/chamados/juize

[truncado em 20000 de 25432 caracteres]

## 27/11 03:15 · sessao f80556b9c6d3 · - · - · origem=Ronald (tmux)

As 51 falhas anexadas correspondem a 29 métodos. Classifiquei 39 como asserções pertinentes ao contrato ou a uma de suas partes, 11 como falhas por outro motivo ou sem evidência da condição alegada e uma dependente de o I23 ser o contador que a regra pretende mudar. Resultados individuais dos outros 16 métodos e execução dos mutantes [nao verificado]. `app/chamados/tests/test_e4d_matriz_reds.py:263–1570`

- **39 pertinentes.** 01, 02 e 03×2 exercitam criação ou supressão esperada de atos, embora os erros de 01/02 e do controle 03 antecedam a verificação de eventos. 11 demonstra falta da reabertura inicial. 12×4 isola cada FK. 16 e 17×3 demonstram falta de veto à inserção. 18/C6 e 18/modal divergem do horizonte em T1. 42 isola a mudança da lavra em sua leitura de marcos, com a projeção já alterada. 45×4 demonstra divergência de autor, lastro, texto novo e FK nula. Também pertinentes 05, 09, 10, 20×2, 25, 26, 29, 30×4, 34, 35, 38, 39, 40 e 41×2. `app/chamados/tests/test_e4d_matriz_reds.py:264–326,402–436,457–564,589–632,673–723,945–1036,1114–1189,1220–1314,1364–1418,1476–1505`
- **11 falhas por outro motivo ou insuficientes para o contrato alegado.** 04 mede aplicação e deixou reversão/foto/hash por competência de fora. 14 exige `REABERTURA` no lugar de `SANEAMENTO`. 44 falha na primeira chamada e a segunda enumeraria um novo acervo, condição que permite passar com a porta ainda duplicando o mesmo payload. 28 cai no seletor que exclui `declaracao` antes da releitura. 36 reaproveita a mesma instância e o mesmo `agora` antes e depois de trancar. `_pv_memo` pode retornar o `True` anterior mesmo com uma consulta nova funcionando. A causa dinâmica fica [nao verificado]. As seis falhas restantes do 45 param na chave `estoque` ausente, incluindo quatro zeros reconciliados. Elas evidenciam a forma de retorno incompatível com c4. O valor histórico do texto, quando a chave vier, continua por demonstrar. `app/chamados/tests/test_e4d_matriz_reds.py:328–360,438–455,566–587,613–640,903–943,1434–1447` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44` · `app/chamados/juizes.py:899–952` · `app/chamados/services/vigia_ato_x_coluna.py:21–25` · `e4c_c4_red_48b24c39c.py:603–608`
- **18/I23.** O contador vigente conta explicitamente compensadores adiantados pelo último ato. A falha `1 != 0` mede esse comportamento. Que o I23 seja a interface do `ato_futuro` da E4D [nao verificado]. Isentar toda `REABERTURA` e contar só `RECUSA` passa no teste com seu controle atual. `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134` · `app/chamados/tests/test_e4d_matriz_reds.py:495–520,536–543`
- **Modal.** O cartão ausente produz `''`, mas o teste exige o texto não vazio da pergunta antes de aceitar ausência de resposta. O recorte é do cartão inteiro e a busca pega a primeira resposta. Uma pergunta homônima no mesmo cartão pode satisfazer a presença e esconder a falta da pergunta observada em T2. No 30, a pista mostra o bloco real exibindo `08:00` sem data, consistente com a leitura crua. O mutante do modal recebe `request=req` e tem assinatura de dois argumentos, portanto pode morrer com `TypeError` antes da asserção. `app/chamados/tests/test_e4d_matriz_reds.py:123–127,208–220,522–534,1288–1314` · `app/templates/chamados/partials/modal_fio.html:157–160,217–220` · `app/chamados/tests/e4d_matriz_mutantes.py:303–315`
- **Mordida.** Em análise AST pura e sem escrita, o censo atribuiu o `bulk_create` com alias `DP` e deixou `Model.save(pergunta.decisoes.model(...))` sem entidade. O teste filtra fora essa segunda forma. O observador SQL só reconhece o PK em predicados específicos de `id`; uma regravação pelos campos de disputa ou célula pode escapar. A pilha contendo `registrar` também admite dois escritores executados sob a mesma chamada. `app/core/censo_escritas.py:364–430,469–484` · `app/chamados/tests/test_e4d_matriz_reds.py:92–109,222–236,488–493,1129–1137`

## 27/11 03:14 · sessao a22e65db9a41 · - · - · origem=Ronald (tmux)

Censo estático entregue ao agente principal. O registro vivo identifica C6, C7, C19, C20, C21, K3, K6 e K7. A tabela de consulta explicita que validação e coluna `validada_em` têm atribuições diferentes. O código atual grava V diretamente tanto na validação quanto no carimbo chamado pelo resolvedor. A porta `registrar` grava DP e acorda a célula. `app/docs/JUIZES.md:7–15,38–43,81–109` · `app/core/juizes.py:293–313,336–352` · `app/chamados/services/validacao.py:164–178` · `app/chamados/services/disputa_emissao.py:2112–2122` · `app/chamados/models.py:3473–3514,4590–4623`

O selo AST aponta uma escrita de DP em `models.py:4609` e deixa escapar chamadas sem modelo atribuído. A própria fixture demonstra outro caminho físico, `Model.save(instancia, force_insert=True)`. O selo SQL depende do formato do filtro por PK e da presença de `registrar` na pilha. Uma atualização da mesma linha selecionada por outra chave, ou dois escritores chamados por `registrar`, pode escapar. `app/core/censo_escritas.py:364–430,469–491` · `app/chamados/tests/test_e4d_matriz_reds.py:92–109,137–141,222–236,488–493`

Dois mutantes têm defeito verificável por leitura. O do 05 troca um atributo de `materializacao` que a emissão já importou por nome. O do modal 30 recebe dois argumentos, enquanto o teste passou a mandar `request=req`. A mordida em execução fica [nao verificado]. `app/chamados/services/disputa_emissao.py:12,1596` · `app/chamados/tests/e4d_matriz_mutantes.py:125–126,141–143,303–315` · `app/chamados/tests/test_e4d_matriz_reds.py:208–215,1075–1078,1114–1120`

## 27/11 03:12 · sessao d01a7dafcf06 · - · - · origem=Ronald (tmux)

- **Fix4 e modal.** O contexto real restaura seleção, vínculo com a disputa e permissão. O caso 18 passa a usar uma disputa com container, como o 30 já usava. A mudança monta a fixture necessária para ler o modal nos dois casos. O cartão ausente produz `''`, mas a pergunta tem texto e o `assertIn` falha antes da comparação. Ainda há seleção ambígua dentro do cartão. Duas perguntas usam `A que horas?` e o regex pega a primeira resposta, sem conferir o PK. Em T2, a pergunta alvo pode sumir e uma irmã muda com o mesmo título fazer o teste passar. `app/chamados/tests/test_e4d_matriz_reds.py:123–127,208–220,505–515,1286–1289` · `app/chamados/services/fio.py:251–254,305–323,334–362,573–603` · `app/templates/chamados/partials/modal_fio.html:76,123,157–160,217–220`.
- **Mutante do modal.** O fix4 passou `request=req` e o mutante substitui a função por `renderizar(template, contexto)`. Ele quebra com `TypeError` antes de testar o par. Execução do mutante [nao verificado]. `app/chamados/tests/test_e4d_matriz_reds.py:215` · `app/chamados/tests/e4d_matriz_mutantes.py:303–315`.
- **Caso 18.** As três falhas apontam para contrato após o fix4. C6 decide `False` cedo, o modal perde a resposta em T1 e I23 inclui o compensador adiantado por estoque. A cobertura ainda aceita C6 usando texto incorreto, pois 08:00 e 09:00 discordam da luz 19:00 e produzem o mesmo booleano. A tarefa pode devolver a razão certa com a ação errada. I23 pode excluir toda `REABERTURA` e passar, pois o controle positivo é `RECUSA`. Essa cura derruba a caracterização F5 que exige contar `REABERTURA` por recuo do relógio após uma validação nova. A distinção por proveniência permite coexistência. `app/chamados/tests/test_e4d_matriz_reds.py:505–543` · `app/chamados/juizes.py:1428–1454,1521–1533` · `app/chamados/services/tarefa_do_dia.py:165–170` · `app/chamados/management/commands/auditar_invariantes_chamados.py:1123–1134` · `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319–325`.
- **Caso 30.** API e modal caem ao exibir valores crus na face real. SQL e raio-x agora exercitam consumidores reais, mas cobram somente elegibilidade e classificação. O SQL pode retornar o PK certo ainda ordenando e entregando o par da coluna. O raio-x pode creditar por qualquer `RESPOSTA` histórica, mesmo superada. O cenário substituiu o par cru válido 10:00/08:00 por `NULL`/08:00 e tirou a asserção C6. Um leitor que só usa o ato quando a coluna de instante é nula passaria. `app/chamados/tests/test_e4d_matriz_reds.py:1288–1314` · `app/chamados/juizes.py:1863–1868` · `app/api/views.py:1163–1165` · `app/templates/chamados/partials/modal_fio.html:217–220` · `app/chamados/services/raiox.py:135–158` · `diff.txt` [nao verificado quanto à numeração absoluta do anexo].
- **Conflitos e lacunas C4.** O C4 anexado concorda com o horizonte da tarefa do 18, mas grava `VALIDACAO` futura como ato novo pelo helper no relógio anterior. A lei A7 recusa fato futuro e o RED16 só exercita `RESPOSTA`. No 14, o RED atual exige `REABERTURA` como a caracterização C1 da mesma revisão, em contradição com a lei de `SANEAMENTO` em choice próprio. No 30, retirar C6 removeu o conflito direto entre asserções com a caracterização da composta. Uma troca global da fonte do texto ainda derruba essa caracterização. `app/chamados/tests/test_e4c_f5_c4_red.py@48b24c39c:297–314` · `app/chamados/tests/test_e4d_matriz_reds.py:438–470,1298–1314` · `app/chamados/tests/test_e4c_f5_instante_c1.py@48b24c39c:234–248` · `app/chamados/tests/test_e4c_f5_opus_models.py:59–69,77–90` · `app/docs/PROMPTS.md:2873,2950`.
- **Estoque C4.** Os 01/02/04/44 atuais não exigem o instante de entrada e a marca do backfill. Antecipar os carimbos ao marco elimina a oposição à recusa do estoque novo, e retentar o comando é operação distinta de ter duplicata no mesmo payload. O 08 coincide com o veto comum C4 depois de suprimir a via autorizada. O 42 compara marcos e lavra, deixando migração e entrada sem prova, embora C4 prove a fronteira inclusiva e preserve `marco+1µs`. Se “hora da migração” for a execução da 0058, C4 guarda a hora da 0057 e preserva uma linha que já existia quando a 0058 executou. A identidade dessas duas horas [nao verificado]. Acrescentar SELECT direto em DP a `marcos_do_lote` quebra o selo F3. `app/chamados/tests/test_e4d_matriz_reds.py:264–295,328–359,375–399,545–587` · `app/chamados/tests/test_e4c_f5_c4_red.py@48b24c39c:345–376,436–442,478–504,953–978` · `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py@48b24c39c:9–20` · `app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:312–322` · `app/docs/PROMPTS.md:2974`.

## 27/11 03:04 · sessao 5e9d4a43e848 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

O helper agora renderiza o modal com `contexto_parcial_fio`, um `RequestFactory` e `self.admin`. A leitura fica restrita ao cartão `fio-ticket-<pk>`. O caso 18, único outro consumidor do helper, ganhou o vínculo entre disputa e chamado exigido pelo montador. As asserções continuam iguais. [test_e4d_matriz_reds.py:208](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:208) [test_e4d_matriz_reds.py:495](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:495) [test_e4d_matriz_reds.py:534](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:534)

A abertura do modal usa `modal_fio_colaborador` e `_resp_modal_full`. O re-render após reabrir usa `contexto_parcial_fio`, caminho escolhido para o helper. Ambos chamam `montar_chamados_fio(..., com_explicacao=True)` e renderizam o mesmo template. O montador ancora a pergunta do caso 30 pelo `chamado_destino`. [views.py:869](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/views.py:869) [views.py:890](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/views.py:890) [views.py:928](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/views.py:928) [views.py:1394](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/views.py:1394) [fio.py:117](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/fio.py:117) [fio.py:582](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/fio.py:582)

**RED do caso 30.** Pela leitura do código, a asserção do modal deve receber `('08:00', '·')` e esperar `('09:00', '· 20/10 10:05')`. O contexto real fornece `folha_julga_pontualidade`, e o template ainda imprime o texto e o instante das colunas alteradas pela fixture. A sintaxe e os três mutantes foram checados em memória. Resultado na suite serial [nao verificado]. [fio.py:599](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/fio.py:599) [modal_fio.html:217](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/templates/chamados/partials/modal_fio.html:217) [test_e4d_matriz_reds.py:1293](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1293) [test_e4d_matriz_reds.py:1311](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1311) [nao verificado]

### Diff

```diff
--- a/app/chamados/tests/test_e4d_matriz_reds.py
+++ b/app/chamados/tests/test_e4d_matriz_reds.py
@@ -208,10 +208,13 @@
     def resposta_no_modal(self, ch, p):
-        ch.disputa_obj = p.disputa
-        ch.perguntas_disputa = [p]
+        from chamados.services.fio import contexto_parcial_fio
+
+        req = RequestFactory().get('/')
+        req.user = self.admin
         with timezone.override('UTC'):
-            html = render_to_string('chamados/partials/modal_fio.html', {
-                'chamados': [ch], 'colaborador': self.colab, 'pode_agir_map': {ch.pk: True},
-            })
+            contexto = contexto_parcial_fio(req, self.colab)
+            html = render_to_string('chamados/partials/modal_fio.html', contexto, request=req)
+        inicio_cartao = '<div id="fio-ticket-'
+        html = html.partition(f'{inicio_cartao}{ch.pk}"')[2].partition(inicio_cartao)[0]
         self.assertIn(p.pergunta, html)
         bloco = re.search(r'<strong>Resposta:</strong>\s*([^<]*)\s*<span[^>]+>([^<]*)</span>', html)
         return (bloco[1].strip(), bloco[2].strip()) if bloco else (None, None)
@@ -502,9 +505,10 @@
         marco = self.marco_do_estoque()
         ch = self.chamado()
         cel = self.celula(trabalha=True, veredito='concorde', veredito_via='cartorio',
                           dna={'marcos': {'hf': '19:00'}},
                           ata={'lampadas': [{'tipo': 'S', 'hora': '19:00', 'acesa': True, 'luz': '19:00'}]})
-        p = self.cartao(destino=ch, respondida_em=T0, resposta_colab='08:00',
+        raiz = self.disputa(chamado=ch, motivos=['orfao_14h'])
+        p = self.cartao(disputa=raiz, destino=ch, respondida_em=T0, resposta_colab='08:00',
                         validada_em=T2, validada_por=self.admin, celula=cel)
         self.fato_restaurado(p, DP.RESPOSTA, marco, texto='08:00')
         estoque = self.fato_restaurado(p, DP.VALIDACAO, marco, em=T2, texto='08:00', autor=self.admin)
```

### Suite serial para o Code [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir /home/ronald/wt-codex-e4d-contrato-rota-b-174640 --only "chamados.tests.test_e4d_matriz_reds" --parallel 1
```

### Mutantes

Da raiz da cópia, aplique cada comando separadamente sobre o diff limpo. Na checagem em memória, M1 voltou a perder a chave, M2 leu a resposta do cartão vizinho e M3 perdeu o vínculo da fixture com o chamado. [test_e4d_matriz_reds.py:214](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:214) [test_e4d_matriz_reds.py:216](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:216) [test_e4d_matriz_reds.py:510](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:510)

```bash
f=app/chamados/tests/test_e4d_matriz_reds.py

# M1 — retirar a chave antes de renderizar
sed -i '214a\            del contexto["folha_julga_pontualidade"]' "$f"

# M2 — ler o HTML inteiro do fio
sed -i '216,217d' "$f"

# M3 — desvincular a disputa do caso 18
sed -i '510s/chamado=ch, //' "$f"
```

## 27/11 03:02 · sessao 6aa002de3fc5 · E4C-F5-INSTANTE · construtor · origem=Code

Apliquei o BRIEF c4red4 nos dois arquivos de teste. A conferência estática encontrou 15 REDs ativos, um skip e 27 seletores de mutantes. O diff passou em `git diff --check`, tem zero `except` e zero `suppress` adicionados e deixa o c1 intocado. As falhas no HEAD, o verde do c1 e as mordidas com PostgreSQL seguem **[nao verificado]**. [RED](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1) · [Arnés](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:26) · [c1](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_instante_c1.py:83) [nao verificado]

1. Estacionei o falso zero com o skip `lei: registro da escrita da coluna -- E4C c4red3 opus5d item 3`. Os casos históricos e a precedência de ato novo no vigia e no DRY ficaram em controles próprios. [Controles e skip](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:551)
2. Removi `b_nascida` e retirei M06 da bateria ativa pela mesma dependência da lei. [Acervo estacionado](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:575) · [Seletores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:421)
3. A concorrência ficou como controle do chamador, na ordem `travar_entrega → trava_na_ordem → registrar`. A fixture observa a espera com `Event` e `pg_locks`, sob prazo monotônico. M27 injeta a leitura de K6 antes da trava. [Fixture](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:650) · [M27](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:402)
4. A resposta futura virou controle da razão e da ação públicas da tarefa. M05 saiu. [Face pública](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:276) · [Seletores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:430)
5. Os escritores exigem uma foto `ESTOQUE` com `entrada_em=MARCO` e um ato novo. O carimbo em competência entregue exige `ValueError` com `ENTREGUE`, foto integral preservada, `validada_em` anterior e zero linhas gravadas. [Carimbo e escritores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:444)
6. A foto após resposta compensada é aplicada diretamente. Para o motivo adulterado, usei a mensagem integral de `_conferir_estoque` para linha que não reproduz a coluna. O HEAD ainda exclui `ato_motivo` da chave comparada. [Testes](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:379) · [Chave e mensagem](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4784) [nao verificado]
7. O arnés roda controle, mutação e controle após restaurar, com fixtures independentes. Só conta a falha na asserção nomeada. Os patches são locais ao caso, M23 restaura em `finally` e os cenários de inserção ficam em transações isoladas. Exercitei o arnés com `unittest` sintético, incluindo sobrevivente, asserção errada e restauração. [Arnés](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:26) · [M23](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:326) · [Isolamento por subteste](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:420)

**Comandos para o Code**, a partir de `app/`. O laço roda cada mutante por seu seletor, sobre a cura, com os dois controles. [Configuração de CI](/home/ronald/wt-codex-e4c-f5-instante-012038/app/config/settings/ci.py:6) · [Seletores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:421)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_c4_red --settings=config.settings.ci --verbosity=2

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_instante_c1 --settings=config.settings.ci --verbosity=2

for classe in $(sed -nE 's/^(M[0-9]+[[:alnum:]]+) = contraprova\($/\1/p' \
  chamados/tests/e4c_f5_c4red2_mutantes.py); do
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    "chamados.tests.e4c_f5_c4red2_mutantes.${classe}.test_mordida" \
    --settings=config.settings.ci --verbosity=2
done
```

## 27/11 02:58 · sessao 99acdd48db17 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**RAMO2-B c2n — três ajustes na cópia**

1. **Pausa na entrada efetiva.** `segmentos_do_motor` agora usa `_ini <= _a <= s`, como `segmentos_do_par`. No RED sobre `045d308c0`, 12x36 e Comercial deram 800 min trabalhados, 720 min de base, pausa 0 e divergência 1. Após a cura, os dois deram **800 − 20 = 780 min trabalhados**, **720 − 20 = 700 min de base**, pausa de 20 min e divergência 0. O teste também morde se o segmento de duração zero for removido, pois o núcleo usa sua ponta para medir a pausa. `app/ponto/calculador/alimentacao.py:143` · `app/ponto/calculador/periodo.py:310` · `app/ponto/calculador/nucleo.py:145–153` · `app/ponto/tests/test_ramo2b_c2h_saida.py:266–284`
2. **Porta da HE.** `_acusa` passa `{id(p): date(2026, 11, 5)}` para o próprio período da fixture. As três asserções existentes passaram no ensaio local. O mutante que omite o mapa reproduziu os três `TypeError`. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:723–750` · `app/folha/porta_export.py:319,347`
3. **Catraca.** O censo de `045d308c0` mediu **9 pareadores, 80 derivações de dia, A1=6, A2=15, B1=77, B2=191 e 3 sítios nomeados**. Registrei a saída de `he_acima_do_teto_por_dia` do B1 e as linhas medidas do motor. Em `unittest` local, o registro anterior teve 5 falhas em 35 testes e o atualizado passou os 35. `test_MORDE_dia_novo_sobe` continua exigindo `SUBIU` para a derivação nova. `app/core/medida_turno_do_par.py:392–452` · `app/core/tests/test_medida_turno_do_par.py:110–111,161,253–350,847–853`

**Escritores e leitores.** O motor grava as pontas e a pausa em `_carimbar_segmento`, o desconto nos dois `calcular_periodo` e o marco da base em `_escrever_base_da_he`. A alimentação publica os pares e o contador. O núcleo usa a ponta de duração zero para medir o vão, o cálculo noturno ignora esse segmento e o DIFF soma o contador. O mapa da jornada da fixture é escrito em `_acusa` e lido pela porta via `id(p)`. `app/ponto/motor_calculo_v2.py:1957,2048–2050,3015,3501` · `app/ponto/calculador/alimentacao.py:143–178,469–475,545–560` · `app/ponto/calculador/nucleo.py:145–153` · `app/ponto/calculador/regras.py:82–86` · `app/ponto/management/commands/diff_calculador.py:323–324` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:735–737` · `app/folha/porta_export.py:347`

**Quem mais chama `segmentos_do_motor`.** Em produção, `app/ponto/calculador/alimentacao.py:334`, via DIFF em `app/ponto/management/commands/diff_calculador.py:312` e lavratura em `app/ponto/services/fechamento.py:1293–1312`. Nos testes, `app/ponto/tests/test_ramo2b_c2h_saida.py:91,277`, `app/ponto/tests/test_ramo2b_c2_base_da_he.py:462,767` e `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:172`.

`LEI-AKITA origem=app/ponto/motor_calculo_v2.py:2048 testemunha=app/ponto/calculador/alimentacao.py:143 RED=app/ponto/tests/test_ramo2b_c2h_saida.py:266 quem-mais-le=app/ponto/calculador/alimentacao.py:334 juizes novos=0 padrao=fonte unica (app/docs/GLOSSARIO.md:11)`

**Mutantes** — resultados observados em memória com `unittest`. Os primeiros dois falharam em ambos os motores, a omissão do mapa gerou três erros e os dois registros antigos foram rejeitados. `app/ponto/tests/ramo2b_c2n_mutantes.py:14–60`

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci
python3 -B -m ponto.tests.ramo2b_c2n_mutantes ignora_pausa_na_entrada  # rc 1
python3 -B -m ponto.tests.ramo2b_c2n_mutantes apaga_segmento_vazio     # rc 1
python3 -B -m ponto.tests.ramo2b_c2n_mutantes omite_dia_da_jornada    # rc 2
python3 -B -m ponto.tests.ramo2b_c2n_mutantes contagem_anterior      # rc 1
python3 -B -m ponto.tests.ramo2b_c2n_mutantes linha_anterior         # rc 1
```

**Recorte para o Code.** Os ensaios locais usaram `unittest` com dependências Django substituídas em memória. Recorte Django e impacto em produção [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci --noinput \
  ponto.tests.test_o280_bateria_da_lei \
  ponto.tests.test_ramo2b_c2l_red_motor \
  ponto.tests.test_ramo2b_c2h_saida \
  ponto.tests.test_ramo2b_c2_base_da_he \
  folha.tests.test_porta_do_export \
  core.tests.test_medida_turno_do_par \
  ponto.tests.test_s5b_split_noturno_tem_um_dono \
  ponto.tests.test_calculador_regras
```

```text
$ GIT_OPTIONAL_LOCKS=0 git -C /home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946 status --porcelain
 M app/core/tests/test_medida_turno_do_par.py
 M app/ponto/calculador/alimentacao.py
 M app/ponto/tests/test_ramo2b_c2_base_da_he.py
 M app/ponto/tests/test_ramo2b_c2h_saida.py
?? app/ponto/tests/ramo2b_c2n_mutantes.py
```

## 27/11 02:56 · sessao 96e5d25defc9 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

A fix3 troca de `orfao_14h` para `intervalo_volta` apenas o motivo de `vizinha` no caso 26 e de `outra` no caso 30. As datas foram preservadas. O motivo distingue as chaves no índice `pergunta_unica_por_marco`. Comparei a AST com `774860a13` e confirmei que as asserções e o restante do módulo são idênticos. [Caso 26](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1261) · [Caso 30](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1292) · [Índice](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/migrations/0053_pergunta_unica_por_familia.py:59)

- **26.** A preservação dos campos e a ausência de novos atos independem do motivo trocado. O mesmo dia mantém `vizinha` no recorte da rejeição. Pela leitura da base, espero `FAIL` na comparação dos campos, pois a rejeição ainda seleciona `e` e `q` pelo grupo legado de disputa e carimbo. Execução [nao verificado]. [Asserções](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1267) · [Seleção legada](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/materializacao.py:1199)
- **30.** As asserções independem do motivo trocado. `outra` continua pendente no chamado vizinho, e o cartão cuja resposta os leitores comparam mantém o motivo e o dia originais. Pela leitura da base, espero `FAIL` já no subteste SQL, pois o filtro exige `respondida_em` preenchido e a fixture o deixa nulo. Execução [nao verificado]. [Fixture e asserções](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1285) · [Filtro SQL](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/juizes.py:1676)

**Recorte para o Code** [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir /home/ronald/wt-codex-e4d-contrato-rota-b-174640 --only "chamados.tests.test_e4d_matriz_reds"
```

**Mutantes existentes**, para o ambiente de testes do Code. A mordida de cada um fica [nao verificado]. [Mutante do 26](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:275) · [Mutantes do 30](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:288) · [Runner](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:393) [nao verificado]

```bash
mutante() {
  E4D_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$2" \
    --settings=config.settings_test \
    --testrunner=chamados.tests.e4d_matriz_mutantes.RunnerMutanteE4D
}
caso26=chamados.tests.test_e4d_matriz_reds.MatrizAusenciaTest.test_caso_26_sem_absorcao_preserva_legado
caso30=chamados.tests.test_e4d_matriz_reds.MatrizRaizELeitoresTest.test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta

mutante 26_devolve_sem_vinculo "$caso26"
mutante 30_fila_pelo_carimbo_cru "$caso30"
mutante 30_api_pelo_par_cru "$caso30"
mutante 30_modal_pelo_par_cru "$caso30"
mutante 30_raiox_pelo_carimbo_cru "$caso30"
```

_8574 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
