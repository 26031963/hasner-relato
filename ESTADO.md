# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 00:12:01.**

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
✻ Crunched for 1m 8s · done 12:09 AM · 2 shells still running

❯ MODO 24H (aval Ronald, ate domingo 24:00): fila na ordem: L-086 corrigida + os 20 -> O80 trava -> O68b aposentar plano B nos ~720 dias -> O70 geradora -> O66 fechadores -> O51b
  -> E5 fechamento online (fatia 1: a 09 lida da celula, sem gravado envelhecendo). Criterio de apply para TODO item de dinheiro: aplica sem parar os colabs cujo movimento esta
  so nos campos do item, com os 26 campos medidos contra o GRAVADO e ninguem saindo do TXT; colab fora disso fica SEPARADO (nao aplicado) numa lista unica no topo do RELATO; o
  item segue. So PARA de verdade em pergunta de LEI (como a do feriado) ou item da L-009. Pergunta de desenho: mede, propoe, segue. Commit, push e DEPLOY JA entre itens. Nenhuma
  lei nova. !

● MODO 24H entendido, e o critério muda a mecânica: por colaborador, não por fatia. Implemento a separação automática:

  Ran 1 shell command

● MODO 24H implementado. O critério agora é por colaborador, e a separação é automática — o item não para por causa de um colab.

  Os campos do item, e o que ficou de fora com o motivo no código:

  Entram: horas_trabalhadas, horas_noturnas, horas_intra_indenizada, turnos_abertos, inconsistencias, horas_saida_antecipada, horas_atraso, horas_folga_trabalhada.

  Ficam fora, e cada um por uma razão diferente:

  horas_extras* — a dobra de plantão que começa no feriado é pergunta de lei aberta (+110,62 h em 17 colabs), e você determinou que pergunta de lei para. Esses 17 ficam separados
  até a sua frase.

  semanas_dsr_* e horas_reflexo_dsr — o DSR se move por dois motivos distintos: falta real (col491, legítimo) e o bug da O76 (col382, entrada 10 min depois da meia-noite jogando o
  plantão fora da janela). Enquanto a O76 não cura, não consigo distinguir os dois automaticamente, então quem tem DSR movido fica separado.

  saldo_banco_horas — fora do escopo do seu ! de ontem, restaurado sempre.

  A separação restaura o colab inteiro, não o campo: campo a campo deixaria a pessoa em metade motor novo e metade velho, um estado que nunca existiu.

  Rodando. Quando voltar, aplico os que passarem, publico a lista única dos separados no topo do RELATO, e sigo para o O80 — commit, push e DEPLOY JÁ entre os itens, como você
  definiu.

✻ Cogitated for 1m 0s · done 12:10 AM · 2 shells still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ aplica os que passarem e publica os separados
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · ← for agents · ↓ to manage
  ⧉  index
```
