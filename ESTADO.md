# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 22:52:42.**

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
 M app/docs/CORTES.md
 M app/docs/PENDENTES_RONALD.json
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
● Monitor(selos e suite de escala apos a L-084)
  ⎿  Monitor started · task bxg4l1cu8 · timeout 900s

❯ No ensaio antes do apply: duas colunas separadas -- (1) efeito da reclassificacao nos 23; (2) efeito da L-084 no RESTO da frota de 09 (colabs e dias em que atraso/saida
  antecipada deixam de descontar), com a lista. Ninguem sai do TXT; se sair, PAREI. Nenhuma lei nova.

● A sua ordem muda o desenho do ensaio, e com razão: a L-084 vale para a frota inteira, não só para os 30 reclassificados. Se eu medisse junto, o número da reclassificação viria
  inflado pelo efeito da lei em 123 colabs. Separo em duas corridas:

  Ran 1 shell command

● Duas corridas, como você separou:

  Coluna 2 rodando agora — --sem-reclassificar: recalcula 09 com a guarda da L-084 no ar mas sem trocar nenhum tipo_base. O delta contra o gravado é o efeito da lei sozinha, na
  frota inteira.

  Coluna 1 depois — reclassifica sobre esse estado, e o delta entre as duas corridas é o efeito da reclassificação nos 23, limpo.

  E a lista que você pediu vem da fonte certa: o motor agora expõe dias_cadastro_x_realidade, mas recalcular_fechamento_mes não a persiste — então eu a colho das instâncias de
  motor que o próprio fechamento criou, com colab, dia, horário cadastrado, horário real e os dois deltas. Contar pela variação de horas_atraso no FechamentoMensal daria o total
  sem dizer quem, e "quem" é metade do que o DP precisa para tratar o cadastro.

  Uma coisa que vou conferir com atenção nesse resultado, porque é o risco real da L-084: ela para de descontar, então horas_atraso e horas_saida_antecipada caem e
  horas_trabalhadas pode subir. Se alguém estava dentro do TXT por ter pouca inconsistência e a lei mexer nisso, ele pode sair — e a sua regra é literal: um único colab saindo =
  PAREI.

  O apply já está escrito com as suas quatro condições, incluindo o saldo_banco_horas restaurado ao valor anterior e a conferência de 07 e 08 no banco, depois do commit — não por
  inferência de que "não recalculei, logo não mudou".

✻ Worked for 1m 29s · done 10:52 PM · 2 shells, 1 monitor still running

● Background command "Usar a autoridade e rodar os selos mais a suite de escala" completed (exit code 0)

* Whirlpooling…
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells, 1 monitor · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
