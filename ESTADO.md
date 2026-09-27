# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 22:57:54.**

| | |
|---|---|
| `HEAD` local | `4f82c385 [O71+O68] o cadastro que mente: NENHUM dos 37 "turno_partido" e partido, e o maior vao e 70 min` |
| `origin/main` | `d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga` |
| commits a subir (`ahead`) | **1** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
4f82c385 [O71+O68] o cadastro que mente: NENHUM dos 37 "turno_partido" e partido, e o maior vao e 70 min
d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga
a497ed37 [O69 VINCULO-DO-DIA] o espelho e o cartao perguntam a celula: a regra propria tinha QUATRO copias
068e830b TICKETS: linhas da O69 (aplicada) e da O67 (medida) que a regua cobrou
a92ae9c4 [O69 VINCULO-DO-DIA] APLICADA em 09: 654,74 h, e os sobrepostos cairam de 116h para 7 minutos
```

## `git status --short`

```
 M CLAUDE.md
 M app/config/crons_duracao.json
 M app/docs/BACKLOG.md
 M app/docs/CORTES.md
 M app/docs/PENDENTES_RONALD.json
 M app/docs/PROMPTS.md
 M app/docs/RELATO.md
 M app/escala/services/cadastro_tipo.py
 M app/ponto/management/commands/diff_reclassificar_partido.py
 M app/ponto/management/commands/plano_b_no_dinheiro.py
 M app/ponto/motor_calculo_v2.py
 M app/relatorios/management/commands/vinculo_do_dia_divergentes.py
?? app/ponto/management/commands/dna_x_batida_real.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
✻ Churned for 1m 6s · done 10:56 PM · 2 shells, 1 monitor still running

❯ remede as duas colunas com a O72 curada

  Read 1 file

● A suíte está lendo a árvore que eu acabei de mudar com a O72 — cometi o mesmo erro de método outra vez, e o veredito dela não vale. Vou relançá-la depois. As medições rodam no
  banco de prod, não no de teste, então podem ir agora:

  Ran 1 shell command

● As duas colunas estão rodando em sequência sobre o banco de prod, com a O72 curada.

  Antes que o resultado chegue, registro uma coisa sobre mim que já aconteceu duas vezes nesta madrugada: editei .py enquanto uma suíte lia a árvore montada. A primeira vez eu
  matei o run e refiz; agora repeti. A LEI-AKITA 10 existe exatamente contra isso, e a razão prática é que o verde daquela suíte não prova nada sobre nenhuma das duas versões da
  árvore — nem a que ela começou lendo, nem a que está lá agora.

  Não é falta de conhecer a regra: é que medir e codar ao mesmo tempo, com o banco de teste ocupado por minutos, cria a tentação de aproveitar a espera. A disciplina certa é usar
  a espera para medir (que roda em outro banco) e não para editar. Vou relançar a suíte depois das medições e antes do apply — o portão do ! inclui árvore verde, e eu não vou
  declarar verde com base num run que leu duas árvores.

  O que espero das duas colunas, para você comparar com o que vier:

  A coluna 2 deve encolher em relação aos 110 dia-colab / −671,88 h de antes, porque o col704 e os casos da mesma classe saem da lista CADASTRO x REALIDADE — o delta deles deixa
  de ser impossível e volta a caber no julgamento normal. Se ela não encolher, a minha cura não pegou, e aí o problema é outro.

  A coluna 1 deve ficar próxima dos números que já medi para a reclassificação (inconsistencias −50, turnos_abertos −30, noturnas +135,61 h, intra +100,17 h), mas agora sem os
  +253,42 h de saída antecipada — que era o artefato que a sua lei mata.

✻ Cooked for 35s · done 10:57 PM · 2 shells, 1 monitor still running

● Background command "Mover a guarda e rodar a suite de escala inteira" completed (exit code 0)

✻ Gesticulating…
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells, 1 monitor · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
