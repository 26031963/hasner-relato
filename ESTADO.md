# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 23:45:45.**

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
  20 colabs revertidos aos 26 campos do snapshot. Prova: 0 campos ainda divergentes, 0 mudanças de status no TXT. Os outros 31 colabs movidos seguem com o apply — eles moveram só
  campos do critério.

  Revertí o colab inteiro, não os quatro campos: campo a campo deixaria a pessoa num estado que nunca existiu, metade motor novo e metade velho. O ganho aprovado desses 20 voltou
  junto, e isso é consequência da sua ordem, não efeito colateral — o aval nomeava campos, e nesses 20 o ato saiu do aval.

  Os piores eram concentrados: col491 ganhava 13,99 h de HE 100% noturna e passava de 5 para 1 semana de DSR OK; col343 de 5 para 2; col382 ganhava 14,04 h e ia de 3 para 1.

  A causa é minha, sem atenuante. Meu DIFF imprimia 10 campos, e as duas corridas de comparação — a (a) e a (b) — usaram a mesma lista curta. Então esses quatro campos sempre se
  moveram e eu nunca os mostrei: quando escrevi "o que move é o medido", o medido era menor que o que move.

  É a mesma classe do incidente que criou a L-082 doze horas antes — "o DIFF motor-x-motor dava +12,29 h e escondia os 10 campos". Repeti a forma no mesmo dia, com outro corte e
  outra lista curta. Por isso registrei a O75: a cura não pode ser disciplina minha, tem de ser default do medidor — imprimir os 26 campos sempre, lista curta só como exceção
  declarada, com selo.

  Agora, na sua sequência: cartao_x_txt e pdf_x_espelho de 09.

✻ Crunched for 2m 6s · done 11:43 PM · 1 shell still running

❯ OS 20 REVERTIDOS (O68b): autopsia do col491, col343 e col382, dia a dia: (1) a HE 100% noturna nova -- quais dias, se e domingo ou feriado trabalhado dentro da escala dele, e
  qual regra a produz; (2) cada semana de DSR que vira "perdido": qual atraso a derruba, se o atraso esta dentro da regua das 3 h e se o cadastro descreve o dia. Tabela por
  colab. Ate o meu corte, os 20 ficam no motor velho no gravado e entram na lista de ajuste ao DP com os dois numeros (velho e novo). Nenhuma lei nova.

● Preciso saber o que derruba a semana antes de tabular. Leio a regra do DSR:

  Searched for 1 pattern, ran 1 shell command

● Achado que derruba a premissa da sua pergunta (2), e é mais sério do que atraso: dsr_ok = not faltas_injustificadas — atraso não entra na conta do DSR. Quem derruba a semana é
  dia previsto sem trabalho e sem justificativa. Confiro os campos do período para montar a tabela:

  Campos do PeriodoCalculo para a tabela
  ⎿  $ sed -n '130,163p' ponto/motor_calculo_v2.py | grep -nE "minutos_|he_|extra|noturn|data" | head -20

· Nucleating… (53s · ↓ 3.0k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
