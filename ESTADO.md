# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:85 AFIRMA ato sem PROVA ao lado -- MODO 24H (aval Ronald 27/09 03:4x). **APLICADOS: 4 colabs**, movimento so nos campos do item
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 00:44:31.**

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
 M app/docs/RELATO.md
 M app/ponto/management/commands/aplicar_09_corte_b.py
 M app/ponto/management/commands/desvio_o68b.py
 M app/ponto/management/commands/diff_reclassificar_partido.py
 M app/ponto/motor_calculo_v2.py
 M app/ponto/services/fechamento.py
 M app/ponto/tests/test_motor_dna_m3.py
?? app/ponto/management/commands/e6_piso_autopsia.py
?? app/ponto/management/commands/folga_que_sumiu.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  │ col877                         │               134,71 h │        0,00 │ 134,71 h │       13 │ —      │
  ├────────────────────────────────┼────────────────────────┼─────────────┼──────────┼──────────┼────────┤
  │ col278                         │                90,27 h │        0,00 │  90,27 h │       15 │ —      │
  ├────────────────────────────────┼────────────────────────┼─────────────┼──────────┼──────────┼────────┤
  │ col203                         │                84,65 h │        0,00 │  84,65 h │       12 │ —      │
  ├────────────────────────────────┼────────────────────────┼─────────────┼──────────┼──────────┼────────┤
  │ col932                         │                49,59 h │        0,00 │  49,59 h │        7 │ —      │
  ├────────────────────────────────┼────────────────────────┼─────────────┼──────────┼──────────┼────────┤
  │ col936                         │                37,98 h │        0,00 │  37,98 h │        5 │ —      │
  ├────────────────────────────────┼────────────────────────┼─────────────┼──────────┼──────────┼────────┤
  │ col511, col622, col369, col788 │                      — │        0,00 │   27,4 h │        — │ —      │
  ├────────────────────────────────┼────────────────────────┼─────────────┼──────────┼──────────┼────────┤
  │ col245                         │                 7,03 h │        0,00 │   7,03 h │        1 │ SIM    │
  ├────────────────────────────────┼────────────────────────┼─────────────┼──────────┼──────────┼────────┤
  │ col865                         │                26,22 h │       24,46 │   1,76 h │        4 │ —      │
  └────────────────────────────────┴────────────────────────┴─────────────┴──────────┴──────────┴────────┘

  11 colabs, 433,4 h desaparecidas — das quais 7,0 h em 1 colab no TXT de 09 (col245).

  Três coisas que esse número diz:

  O col877 com 134,71 h é o mesmo número que aparece no comentário do fechamento.py sobre o apply da E3: "8 colabs, ate 134,71 h de diferenca entre o ensaio (sem o arquivo) e prod
  (com ele)". É o mesmo colaborador, o mesmo valor — o caso que gerou a O63 é também a maior vítima deste bug, e ninguém tinha notado porque a O63 tratou a lavra faltando, não a
  hora sumindo quando a lavra diz "errada".

  O col865 é a prova do mecanismo: 26,22 h de folga trabalhada, 24,46 h reconhecidas e 1,76 h sumidas. Ele tem dias com escala certa e dias com escala errada — e só os errados
  desaparecem. Isso descarta "o campo nunca soma nada" e confirma que o filtro _certa é o que corta.

  E o impacto no TXT de 09 é pequeno: 7,0 h em um colab. Os 433 h estão quase todos em gente que já está retida por outro motivo. Isso muda a urgência, não a gravidade — 90 h de
  uma pessoa não pagas é 90 h, mesmo que ela esteja fora do TXT por furo de espelho.

  Vou implementar a cura que você definiu — sem escala certa, a hora entra em horas_trabalhadas sem o 100% — e medir o DIFF pelo critério do MODO 24H.

✻ Cogitated for 20s · done 12:44 AM · 1 shell still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ aplica a cura e mede o DIFF
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · ← for agents · ↓ to manage
  ⧉  index
```
