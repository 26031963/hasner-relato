# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 23:08:10.**

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
 M app/escala/tests/test_jm_derivado.py
 M app/ponto/management/commands/diff_reclassificar_partido.py
 M app/ponto/management/commands/plano_b_no_dinheiro.py
 M app/ponto/motor_calculo_v2.py
 M app/relatorios/management/commands/vinculo_do_dia_divergentes.py
?? app/ponto/management/commands/dna_x_batida_real.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  usar turno_partido. Ou seja: o cadastro que você identificou como errado é, hoje, o único que o sistema aceita para plantão noturno contínuo.

  Isso reposiciona a O68 inteira. Reclassificar 32 templates para 12x36 pode passar por essa validação, mas a minha correção de fixture para comercial num turno 21:00–05:00 é
  inválida por regra do modelo — e o mais importante: significa que a casa já sabia que comercial quebra turno que cruza, e resolveu isso rotulando de "partido", o que criou os
  761 dias fora do juiz.

  Não aplico. Duas razões, e nenhuma é a suíte por si: a condição literal que você deu não fechou, e o alvo da reclassificação precisa ser revisto contra uma validação do modelo
  que eu desconhecia quando propus o mapeamento.

✻ Brewed for 1m 12s · done 11:05 PM · 1 shell, 2 monitors still running

❯ (1) A porta nova (recusa "partido" com vao ate 2 h) NAO vai ao ar enquanto a base do plantao noturno continuo nao for decidida: junto com a validacao antiga (comercial nao
  cruza a meia-noite) ela impede cadastrar 5x2/6x1 noturno. Garantir que nao esta na arvore servida antes do reload das 03:30; se estiver, tirar com trilha.
  (2) MEDIR, sem mudar nada: o que TipoEscala.tipo_base comanda alem de MOTOR_POR_BASE (gerador de celulas/ciclo, tela, relatorios, export) -- arquivo:linha. E se
  Motor12x36ComEscala serve para plantao continuo que cruza a meia-noite fora do ciclo 12x36.
  (3) Com isso, trazer as opcoes para eu decidir: (a) base de 12x36 para esses 37; (b) o motor escolhido pelo vao entre blocos, sem mexer no cadastro; (c) o motor comercial
  aprender a nao partir o turno na meia-noite. Reclassificacao em 09 fica suspensa ate o corte. Seguem sem esperar os itens de dinheiro zero (O70, O51b). Nenhuma lei nova.

● Item (1) é urgente e vou primeiro. A guarda está na árvore viva e o reload das 03:30 a poria no ar:

  Ran 1 shell command

● Correção importante do meu próprio método: docker exec ... python -c abre processo novo e lê o disco, não o worker servido. Isso responde "está na árvore", não "está no ar". O
  que vale é a hora: arquivo escrito ~22:5x, workers de pé desde 21:39 — a guarda não está servida, e o reload das 03:30 a poria. Tiro agora, com trilha:

  Testar a regra pausada direto na funcao
  ⎿  $ python3 - <<'PYEOF'
     p='escala/tests/test_jm_derivado.py'
     s=open(p).read()
     old=''' def test_MORDE_a_porta_recusa_partido_com_vao_de_intrajornada(self):
     """`!` Ronald 26/09: vao de ate 2 h nao e turno partido. Sem este caso, a guarda poderia
     sumir da porta e os tres testes acima seguiriam verdes --…

✽ Undulating… (1m 41s · ↓ 5.6k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell, 2 monitors · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
