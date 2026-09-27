# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 22:10:45.**

| | |
|---|---|
| `HEAD` local | `d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga` |
| `origin/main` | `d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga` |
| commits a subir (`ahead`) | **0** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga
a497ed37 [O69 VINCULO-DO-DIA] o espelho e o cartao perguntam a celula: a regra propria tinha QUATRO copias
068e830b TICKETS: linhas da O69 (aplicada) e da O67 (medida) que a regua cobrou
a92ae9c4 [O69 VINCULO-DO-DIA] APLICADA em 09: 654,74 h, e os sobrepostos cairam de 116h para 7 minutos
9b67131e [O69 VINCULO-DO-DIA] o fechamento pergunta a CELULA qual vinculo vale no dia -- sobreposicao para de dobrar
```

## `git status --short`

```
 M app/config/crons_duracao.json
 M app/docs/BACKLOG.md
 M app/docs/PENDENTES_RONALD.json
 M app/docs/PROMPTS.md
 M app/docs/RELATO.md
?? app/ponto/management/commands/celula_veredito_velho.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  ┌────────┬──────────────────────────────────────────────────────────────┬─────────────────────────────────┐
  │  quem  │                            efeito                            │       muda status no TXT?       │
  ├────────┼──────────────────────────────────────────────────────────────┼─────────────────────────────────┤
  │ col830 │ —                                                            │ ENTRA (era fora / furo_espelho) │
  ├────────┼──────────────────────────────────────────────────────────────┼─────────────────────────────────┤
  │ col56  │ —                                                            │ ENTRA (era fora / furo_espelho) │
  ├────────┼──────────────────────────────────────────────────────────────┼─────────────────────────────────┤
  │ col107 │ horas_trabalhadas −7,33 h, turnos_abertos +2                 │ não                             │
  ├────────┼──────────────────────────────────────────────────────────────┼─────────────────────────────────┤
  │ col499 │ horas_extras +1,00 h, trabalhadas +1,00 h, turnos_abertos −1 │ não                             │
  └────────┴──────────────────────────────────────────────────────────────┴─────────────────────────────────┘

  SAEM do TXT: 0 — essa metade do critério fechou. Mas os dois que entram não movem dinheiro, e os dois em que o dinheiro move não mudam de status.

  E o mais importante: é da rejulga, não é deriva, e isso está provado. O baseline de recálculo puro em prod move nenhum campo — o gravado de 09 já está em sincronia com o motor
  de HEAD depois dos applies de hoje. A causa é a ata relavrada: 5.078 lavras para 158 mudanças de veredito, porque a porta lavra a ata também e o motor lê a ata.

  Não apliquei. O critério nomeava os colabs do alvo, e dinheiro que se move em colaborador que a cura não visa é a forma exata que a L-082 nasceu proibindo — de manhã o ! era
  +12,29 h num campo e o gravado moveu +13,29 h com 10 campos fora do alvo. Aqui é menor e tem causa conhecida, mas é a mesma forma, então o número fica na mesa em vez de entrar
  por analogia.

  Registrei em PENDENTES_RONALD.json com o comando de reprodução e o de apply, caso o ! venha: --aplicar --antes logs/apply_o71_antes.json, revertível pelo snapshot das 17.332
  células.

  Uma observação que vale o ponteiro: col107 aparece pela segunda vez hoje — ele foi um dos dois colabs do DIFF da O69 (23/08, 374→349 min). Não afirmo que seja a mesma causa,
  porque não medi isso. Mas duas aparições no mesmo dia em fatias diferentes é padrão, não acidente.

  A esteira não para. Sigo para a O68, onde o seu ! de critério para a parte (a) já está dado.

✻ Sautéed for 1m 22s · done 10:10 PM · 1 shell still running

❯ segue pra O68

· Creating…
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
