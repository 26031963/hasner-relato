# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 23:55:45.**

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
  │ colab  │       DSR novo       │                                                                é legítimo?                                                                 │
  ├────────┼──────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ col491 │ perdido=1, falta     │ sim — ele não tem entrada no 20/09. O motor velho contava a cauda do plantão do dia 19 como trabalho do dia 20, e isso é exatamente o que  │
  │        │ 20/09                │ o seu corte proíbe                                                                                                                         │
  ├────────┼──────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ col343 │ ok=1                 │ sim — entrou 20/09 23:00, na hora                                                                                                          │
  ├────────┼──────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ col382 │ perdido=1, falta     │ não — ele trabalhou. Entrou 21/09 00:00, dez minutos depois do marco 23:50, e o período migrou para fora da janela                         │
  │        │ 20/09                │                                                                                                                                            │
  └────────┴──────────────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

  Então, pela sua ressalva "salvo falta real": col491 e col343 estão conformes; col382 é bug, e é a mesma família da O72 — agora na entrada em vez da saída. Registrei como O76,
  com a causa nomeada: a pergunta "a que turno pertence a batida?" tem juiz (turnos_do_colab, core/juizes.py:151), e o motor a responde por conta própria fazendo data_jornada =
  entrada.date(); o periodos_de_turnos descarta o data_turno que o juiz já calculou.

  Não vou remedir os 20 e aplicar ainda, e o motivo é o seu próprio critério: com o col382 nessa condição, a remedição traria uma semana de DSR perdida por bug, não por falta — e
  isso é campo fora do critério outra vez. A O76 vem antes. Se você preferir aplicar os 20 aceitando o col382 como conhecido-e-listado, eu faço — mas não escolho isso sozinho,
  porque é uma semana de descanso de uma pessoa que trabalhou.

✻ Worked for 2m 54s · done 11:51 PM · 1 shell still running

❯ corte Ronald (leis de dominio): as 8 regras abaixo entram no LEIS.md como L-NNN, cada uma com dono (arquivo::funcao que a aplica) e selo que morde; onde nao houver selo, a
  linha diz "SEM SELO" e vira obra no BACKLOG. (1) jornada pertence ao dia de inicio, inclusive domingo, feriado, semana e DSR; (2) plantao previsto nunca e HE 100%; (3) batida =
  instante contra marco, o tipo gravado nao decide; (4) falta tem um significado so, furo e "em aberto"; (5) atestado so tem efeito em dia de trabalho; (6) competencia 21-20,
  nunca mes civil; (7) a definicao da escala decide o regime, o rotulo e texto; (8) competencia exportada nao muda o gravado, diferenca vira Pauta DP. Depois disso, censo: regra
  de dominio que o codigo aplica e nao tem L-NNN = lista para mim. !

● Ordem grande e de vocabulário. Primeiro o formato do LEIS.md e o último número:

● Selos existentes por tema das 8 leis
  ⎿  $ echo "1 dia de inicio:"; ls ponto/tests/ | grep -iE "dia20|dupla_contagem|dia_da_jornada" ; echo "3 instante x tipo:"; ls ponto/tests/ | grep -iE
     "tipo_gravado|marco_manda|juiz_batida"; echo "4 falta/furo:"; ls ponto/tests/ escala/tests/ relatorios/tests/ 2>/dev/null | grep -iE "falta|furo" | hea…

✶ Booping… (1m 18s · ↓ 3.9k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
