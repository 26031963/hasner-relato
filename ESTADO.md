# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 23:50:45.**

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
 M app/config/crons.py
 M app/config/crons_duracao.json
 M app/core/juizes.py
 M app/core/templatetags/hasner_filters.py
 M app/docs/ARQUITETURA.mmd
 M app/docs/BACKLOG.md
 M app/docs/CORTES.md
 M app/docs/PENDENTES_RONALD.json
 M app/docs/PROMPTS.md
 M app/docs/RELATO.md
 M app/escala/services/cadastro_tipo.py
 M app/escala/servico_jornada.py
 M app/escala/tests/test_jm_derivado.py
 M app/ponto/management/commands/diff_reclassificar_partido.py
 M app/ponto/management/commands/plano_b_no_dinheiro.py
 M app/ponto/motor_calculo_v2.py
 M app/relatorios/management/commands/vinculo_do_dia_divergentes.py
 M bin/relato.sh
?? app/ponto/management/commands/aplicar_09_corte_b.py
?? app/ponto/management/commands/desvio_o68b.py
?? app/ponto/management/commands/dna_x_batida_real.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```

● Os dois sítios já usam e.date() — o dia da entrada, que é o dia de início. Então o que falta é a condição "dia NÃO previsto". Vejo como o DSR monta datas_trabalho:

  Ran 2 shell commands

● datas_trabalho já usa o dia da entrada. Então achei a mecânica do DSR: sob MotorTurnoPartido, a cauda depois da meia-noite era um período próprio com entrada em 20/09 — e
  contava como trabalho naquele dia. Com o motor contínuo ela se funde ao 19/09, e o 20/09 fica vazio. Implemento o 100% e meço se o 20/09 é falta real:

  Ran 2 shell commands

● A medição do 20/09 separa três casos, e o terceiro é um bug novo:

  ┌────────┬────────────────────────────────────────────────────────┬────────────────────────────────────────────────────────────────────────────────┐
  │ colab  │                    batidas no 20/09                    │                                    leitura                                     │
  ├────────┼────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────┤
  │ col491 │ 01:17S 02:17E 06:50S — só a cauda do plantão do dia 19 │ falta real: não há entrada para o plantão do dia 20. DSR perdido é correto     │
  ├────────┼────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────┤
  │ col343 │ cauda + 20/09 23:00E                                   │ começou o plantão do dia 20 na hora → DSR ok. Caso de controle                 │
  ├────────┼────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────┤
  │ col382 │ 21/09 00:00E 03:56S 05:18E 07:51S                      │ trabalhou, mas entrou 10 min depois do marco 23:50 — e a entrada caiu em 21/09 │
  └────────┴────────────────────────────────────────────────────────┴────────────────────────────────────────────────────────────────────────────────┘

  O col382 não tem falta: o plantão dele do dia 20 começou às 00:00 do dia 21, dez minutos atrasado. Como o motor atribui o dia pela data civil da entrada, o período foi para
  21/09 — fora da janela — e o 20/09 virou falta. É a mesma família da O72, agora na entrada em vez da saída.

  Running 1 shell command…
  ⎿  $ python3 - <<'PYEOF'
     p='docs/BACKLOG.md'
     s=open(p).read()
     anc="<!-- OBRAS:FIM -->"
     item=("| O76 | **DIA DO PLANTAO VEM DO MARCO, NAO DA DATA CIVIL DA ENTRADA** (P7.1; achado 27/09 02:0x na "
     "autopsia do corte do feriado) -- `MotorBase.calcular_mes` faz `data_jornada = localizar(entrada).date()`. …

· Catapulting… (2m 39s · ↓ 9.8k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
