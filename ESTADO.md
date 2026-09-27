# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:21 AFIRMA ato sem PROVA ao lado -- MODO 24H (aval Ronald 27/09 03:4x). **APLICADOS: 4 colabs**, movimento so nos campos do item
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 00:39:01.**

| | |
|---|---|
| `HEAD` local | `c060e70a [O68b+L-084..L-092] o corte pelo VAO no ar, 8 leis de dominio indexadas, e o MODO 24H separando por colab` |
| `origin/main` | `d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga` |
| commits a subir (`ahead`) | **2** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
c060e70a [O68b+L-084..L-092] o corte pelo VAO no ar, 8 leis de dominio indexadas, e o MODO 24H separando por colab
4f82c385 [O71+O68] o cadastro que mente: NENHUM dos 37 "turno_partido" e partido, e o maior vao e 70 min
d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga
a497ed37 [O69 VINCULO-DO-DIA] o espelho e o cartao perguntam a celula: a regra propria tinha QUATRO copias
068e830b TICKETS: linhas da O69 (aplicada) e da O67 (medida) que a regua cobrou
```

## `git status --short`

```
 M CLAUDE.md
 M app/config/crons.py
 M app/config/crons_duracao.json
 M app/docs/LEIS.md
 M app/ponto/management/commands/aplicar_09_corte_b.py
 M app/ponto/management/commands/desvio_o68b.py
 M app/ponto/management/commands/diff_reclassificar_partido.py
 M app/ponto/motor_calculo_v2.py
 M app/ponto/services/fechamento.py
 M app/ponto/tests/test_motor_dna_m3.py
?? app/ponto/management/commands/e6_piso_autopsia.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
● O método está legível. O ponto crítico da validação é contra o que ele compara: minutos_realizados + horas_folga_trabalhada. Vejo o que esse campo guarda:

  Searched for 1 pattern, ran 1 shell command

● Achei o erro do método, e ele é estrutural. minutos_realizados não é "horas trabalhadas":

  fechamento.minutos_realizados = sum(
      min((d.get('minutos_realizados') or 0), (d.get('minutos_previstos') or 0))
      for d in _dt_trab if d.get('tipo_dia') == 'trabalho')

  Três coisas o tornam incomparável com batidas completas: ele é capado no previsto por dia ("cap POR DIA: HE nao tapa furo de outro dia"), só conta dias tipo_dia == 'trabalho', e
  o próprio modelo diz que ele existe para percentual de cumprimento, não para dinheiro. Escrevo a autópsia que mede isso:

  Ran 1 shell command

● Duas coisas: o padrão já é informativo, mas a minha sonda tem bug — tipo_dia vem None e prev vem 0 em todos, então a coluna cap_h está não medida, não zero. Vejo as chaves reais
  do dia:

  Ran 2 shell commands

● As chaves são previsto, folga, ausencia, feriado — não existe tipo_dia nem minutos_previstos no dict do espelho. Refaço a sonda perguntando ao juiz do previsto:

  Ran 1 shell command

● Agora os números são reais, e eles corrigem a minha própria crítica ao método:

● Os campos que explicam a diferenca
  ⎿  $ cd /home/ronald/saas-hasner && docker exec saas_core python manage.py tenant_command shell --schema=juliani --command "
     from ponto.models import FechamentoMensal
     print('colab | trabalhadas | folga_trab | min_real(h) | intra_ind | atraso | extras | previstos(h)')
     for pk in (278,203,788,880,923):
     f…

✢ Harmonizing… (3m 16s · ↓ 13.0k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
