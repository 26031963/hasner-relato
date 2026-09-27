# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:21 AFIRMA ato sem PROVA ao lado -- MODO 24H (aval Ronald 27/09 03:4x). **APLICADOS: 4 colabs**, movimento so nos campos do item
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 00:28:31.**

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
 M app/config/crons_duracao.json
 M app/ponto/motor_calculo_v2.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```

  Searched for 1 pattern, ran 1 shell command

● _dia_do_turno já existe e a docstring diz "So o dia sai daqui" — e ele é usado no MotorTurnoPartido e no MotorComercial, mas não no MotorBase, que é onde o col382 roda. Eu criei
  um segundo mapa para a mesma pergunta. Removo o meu e uso o declarado:

  Ran 1 shell command

● O76 curada, e o RED fechou exatamente como a sua ordem previa:

  ┌────────┬────────────────────────┬────────────────────────┬───────────────────────────────────────────────────────────────────────────────────────┐
  │ colab  │         antes          │         agora          │                                        leitura                                        │
  ├────────┼────────────────────────┼────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────┤
  │ col382 │ perdido=1, falta 20/09 │ ok=1, sem falta        │ ele não faltou — entrou 10 min depois da meia-noite e o motor o jogava fora da janela │
  ├────────┼────────────────────────┼────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────┤
  │ col491 │ perdido=1, falta 20/09 │ perdido=1, falta 20/09 │ falta real — segue perdido, e está certo                                              │
  ├────────┼────────────────────────┼────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────┤
  │ col343 │ ok=1                   │ ok=1                   │ controle, inalterado                                                                  │
  └────────┴────────────────────────┴────────────────────────┴───────────────────────────────────────────────────────────────────────────────────────┘

  A cura foi menor e mais funda do que eu tinha escrito na primeira tentativa. Três coisas que valem registro:

  O mapa já existia. MotorBase._dia_do_turno, cuja docstring diz literalmente "So o dia sai daqui". O MotorTurnoPartido e o MotorComercial já o usavam; o MotorBase era o único dos
  três que derivava por conta própria — e é nele que roda o Motor12x36ComEscala, ou seja o plantão noturno, exatamente quem sofre o problema.

  Eu criei um segundo mapa antes de procurar o primeiro. Chaveado pelo timestamp cru, enquanto o declarado chaveia por localizar(...). Apaguei o meu. Um segundo mapa para a mesma
  pergunta é o que esta noite inteira arrancou de quatro lugares — e eu quase plantei outro.

  Havia uma terceira derivação, no DSR. datas_trabalho.add(localizar(p.entrada).date()) — e era ela que derrubava a semana do col382. Curar só o data_jornada não bastava; o laço
  do DSR tinha a sua própria conta. Agora ele lê o dia da jornada que classificou o período.

  Agora remeço os 30 pelo critério do MODO 24H e aplico os que passarem.

✻ Cooked for 3m 1s · done 12:25 AM · 2 shells still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ remede os 30 e aplica os que passarem
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · ← for agents · ↓ to manage
  ⧉  index
```
