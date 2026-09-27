# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 00:01:01.**

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
 M app/docs/LEIS.md
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
?? app/ponto/tests/test_selo_l086_feriado_previsto_nao_dobra.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```

  Ran 1 shell command

● Monitor(remedicao dos 20 com os 26 campos apos o selo da L-086)
  ⎿  Monitor started · task br7751v0p · timeout 1500s
  ⎿  Allowed by auto mode classifier

● A remedição ainda está rodando — publico quando o número existir. Monitor armado.

  Enquanto ela corre, o que já está fechado da sua lista de prioridades até segunda:

  O78 — feito. Selo da L-086 verde com 6 casos, e ele achou quatro sítios pagando dobra de feriado, não os dois que eu havia curado lendo o código. Os dois extras estavam em
  MotorTurnoPartido e MotorComercial, por dia em vez de por período. Sem o selo, a lei estaria "vigente" com metade dos sítios violando-a.

  Falta da sua lista: a segunda metade do (1), que é esta remedição; o O80, o selo da L-092; e o O51b.

  Sobre o O80, já sei a forma do teste que morde, e quero adiantar porque é a lei mais perigosa sem selo: recalcular competência com ExportacaoDominio confirmada tem de recusar, e
  o par é competência sem export recalculando normal. O risco que ela cobre não é hipotético — esta noite eu recalculei a 09 pelo menos seis vezes, e se ela estivesse exportada o
  gravado teria mudado por baixo de um TXT já entregue ao DP, sem nada no código reclamando.

  As 12 perguntas sem lei ficam listadas no RELATO para o seu corte, com fatos_do_dia e eh_feriado no topo como você pediu — as duas são leitura diária do motor, o que as torna as
  mais caras de deixar sem vocabulário.

✻ Churned for 23s · done 12:00 AM · 2 shells, 1 monitor still running

❯ segue pro O80 enquanto a remedicao roda

● O80. Primeiro os fatos: o que é "exportada", e se já existe trava:

● O que e exportada e se ha trava no recalculo
  ⎿  $ grep -n "class ExportacaoDominio" -A 22 folha/models.py | grep -nE "= models|def |confirmad|status" | head -14; echo "=== trancar_periodo ==="; sed -n '451,480p'
     ponto/services/fechamento.py; echo "=== o recalculo tem alguma trava? ==="; grep -n "trancado\|bloqueado\|ExportacaoDominio" ponto/serv…

✢ Whirring… (15s · ↓ 481 tokens · thinking)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells, 1 monitor · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
