# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 00:06:31.**

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
● Selo da L-086 corrigida: 10 casos, verde. E ele achou um quinto sítio — o texto do motivo dizia "folga semanal" por weekday() in dias_folga cru, o que poderia rotular assim um
  dia que o juiz considera previsto. Rótulo contradizendo o cálculo é a testemunha mentindo em prosa, então passou pelo mesmo juízo.

  O que o corte mudou, e o que eu tinha errado:

  Feriado saiu da L-086. Volta a seguir só feriado_em_dobra do ciclo, com o dia sendo o de início. Minha primeira versão exigia "dia não previsto" e generalizava o parecer do
  12x36 para todo plantão — e medido na frota aquilo tirava −642,43 h de 64 colaboradores. Deixei isso escrito na docstring e num caso de teste próprio
  (test_MORDE_a_versao_ANTIGA_desta_lei_ficaria_VERMELHA), para que ninguém "conserte o teste" em vez da regra se reintroduzir a versão errada.

  Domingo entrou. _folga_pelo_dia_previsto: o folga_dia_semana do template não vence a escala, e o dia é o de início — turno que começa sábado 22:00 e sai domingo 06:00 não é
  domingo trabalhado.

  Os pares que mordem, em ambas: plantão que começa no feriado dobra × o que começa na véspera e entra pelo feriado não dobra; domingo previsto não é folga × domingo não previsto
  segue folga — este último importa porque horas_folga_trabalhada é dinheiro, e uma versão que devolvesse sempre False apagaria folga trabalhada de todo mundo.

  Agora remeço os 20 e a frota. O esperado, pela sua ordem: HE 100% noturna dos 20 perto de zero, e dobra de feriado da frota intacta — salvo o que muda por atribuição do dia de
  início.

✻ Brewed for 2m 35s · done 12:06 AM · 2 shells still running

❯ remede os 20 e a frota

  Ran 1 shell command

● Remedição em ensaio, com os 26 campos e o col81 fora.

  Os três números que decidem, e o que cada desvio significa:

  horas_extras_100_feriado perto de zero. Se a dobra da frota voltou intacta, o campo não se move — era +667,11 antes da minha versão errada e tem de voltar a ficar parado. Se
  ainda cair centenas de horas, sobrou resíduo da regra que você revogou.

  horas_extras_100_noturna perto de zero. Esse é o alvo original dos 20. Ele vem de turnos_abertos fechando, não de feriado, então deve permanecer curado.

  horas_folga_trabalhada. Campo novo em risco: a regra de domingo agora pode tirar folga trabalhada de quem tem domingo previsto — e isso é dinheiro saindo. Antes era +1,76 h em 1
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
