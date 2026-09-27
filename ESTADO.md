# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 22:36:31.**

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
 M app/config/crons_duracao.json
 M app/docs/RELATO.md
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  chamou atenção:

  Searched for 1 pattern

● Duas pistas fortes: saldo_banco_horas é calculado numa única linha (1685) e horas_saida_antecipada em 1879/1396. Vejo em que classe cada linha mora — se forem exclusivas de
  certos motores, a explicação é outra:

  Ran 1 shell command

❯ Pista para a autopsia do col382 (saida antecipada ~4 h por dia) e do banco -305 h: conferir, dia a dia, o marco de SAIDA que o motor usa depois da troca de regime -- se o
  template "partido" tem a saida do 2o bloco depois da meia-noite e o motor passa a compara-la na data da entrada, a saida antecipada e artefato de data, nao fato. Mostrar 3 dias
  do col382: marco de saida usado, batida de saida real, minutos descontados. Nenhuma lei nova.

● A AST já derruba metade da minha hipótese e confirma a outra:

  ┌─────────────────────────────────┬─────────────────────────────────────────┐
  │             cálculo             │                 mora em                 │
  ├─────────────────────────────────┼─────────────────────────────────────────┤
  │ saldo_banco_horas (único sítio) │ MotorComercial                          │
  ├─────────────────────────────────┼─────────────────────────────────────────┤
  │ minutos_saida_antecipada        │ MotorTurnoPartido e Motor12x36ComEscala │
  └─────────────────────────────────┴─────────────────────────────────────────┘

  Então o banco não existia para quem era turno_partido — os −305,37 h são o campo passando a ser calculado, não gente perdendo hora. Já a saída antecipada existe nos dois, e é aí
  que a sua pista entra. Leio o cálculo:

  Ran 2 shell commands

● Motor12x36ComEscala trata o cross-meia-noite: se hora_fim < hora_inicio, data_ref = dia do início previsto + 1. Então a data do marco parece certa — mas o alerta do motor
  carrega o marco que ele usou, e é isso que vou ler em vez de deduzir:

● Running 1 shell command…

✢ Fiddle-faddling… (1m 41s · ↓ 6.3k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
