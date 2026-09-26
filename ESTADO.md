# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 19:20:45.**

| | |
|---|---|
| `HEAD` local | `cb4152ef [O67] PAREI no item (6): 85 colabs, 3.268h56 -- e os meus DOIS medidores estavam cegos ao RED` |
| `origin/main` | `af7a6425 [O65] a cauda: cartao_x_txt=0 e 09 estavel, mas NAO declaro liberada -- pdf_x_espelho nao tem medidor` |
| commits a subir (`ahead`) | **4** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
cb4152ef [O67] PAREI no item (6): 85 colabs, 3.268h56 -- e os meus DOIS medidores estavam cegos ao RED
e571febd [O67] 09 CONDICIONAL: as tres medicoes de frota com a lista -- 203 no TXT, dos quais 9 com ajuste
6cf370e6 [PDF-X-ESPELHO-MEDIDOR] 09/2026 LIBERADA -- e o medidor que faltava agora existe em codigo
3046eef0 [BECO-FIO-FECHADO-CELULA-ABERTA] O66 medida: celula_furo_sem_canal = 68, e nenhum fechador consulta a celula
af7a6425 [O65] a cauda: cartao_x_txt=0 e 09 estavel, mas NAO declaro liberada -- pdf_x_espelho nao tem medidor
```

## `git status --short`

```
 M app/config/crons_duracao.json
 M app/docs/RELATO.md
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  │ colab  │ diferença │  topo  │ coluna │             causa             │
  ├────────┼───────────┼────────┼────────┼───────────────────────────────┤
  │ col924 │ 132h40    │ 0h00   │ 132h40 │ zero vínculo na janela        │
  ├────────┼───────────┼────────┼────────┼───────────────────────────────┤
  │ col866 │ 84h03     │ 239h57 │ 155h54 │ vínculo SOBREPOSTO (conta 2x) │
  ├────────┼───────────┼────────┼────────┼───────────────────────────────┤
  │ col391 │ 31h57     │ 0h00   │ 31h57  │ zero vínculo na janela        │
  ├────────┼───────────┼────────┼────────┼───────────────────────────────┤
  │ col43  │ 29h20     │ 0h00   │ 29h20  │ zero vínculo na janela        │
  ├────────┼───────────┼────────┼────────┼───────────────────────────────┤
  │ col942 │ 28h09     │ 0h00   │ 28h09  │ zero vínculo na janela        │
  ├────────┼───────────┼────────┼────────┼───────────────────────────────┤
  │ col935 │ 11h02     │ 77h04  │ 88h06  │ troca de vínculo no meio      │
  ├────────┼───────────┼────────┼────────┼───────────────────────────────┤
  │ col400 │ 8h38      │ 156h56 │ 148h18 │ sem causa dessas              │
  ├────────┼───────────┼────────┼────────┼───────────────────────────────┤
  │ col245 │ 7h01      │ 13h59  │ 21h00  │ sem causa dessas              │
  ├────────┼───────────┼────────┼────────┼───────────────────────────────┤
  │ col648 │ 1h29      │ 182h38 │ 181h09 │ troca de vínculo no meio      │
  └────────┴───────────┴────────┴────────┴───────────────────────────────┘

  9 no TXT somando 334h20; os outros 16 (867h35) já estão retidos e não foram pagos.

  (2) Sua hipótese se confirma exatamente na assinatura que você previu: dos 9 com gravado MAIOR que o espelho, 5 têm vínculo sobreposto — col277, col899, col866, col107, col736.
  "Gravado maior que o espelho tem essa cara" estava certo. Mais 8 com troca de vínculo no meio da competência.

  (3) Os de gravado 0h00: 7 colabs, todos com FM existindo — nenhum é "nunca calculado". E 4 têm zero vínculo ativo. Entre os que entram no TXT, quatro (col924, col391, col43,
  col942) têm zero vínculo na janela: o fechamento calculou zero porque não achou vínculo, e o espelho vê as horas porque pega o último vínculo mesmo inativo. Duas regras
  diferentes para "qual vínculo vale" — e é essa a raiz das duas pontas (o zero e o dobrado).

  Nada tocado. O que espera seu ! é a cura dessa raiz, e ela tem endereço: o fechamento exige vínculo ativo e o espelho toma o último; enquanto as duas regras conviverem, o topo e
  a coluna vão divergir por construção.

✻ Baked for 18m 57s · done 7:20 PM
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · ← for agents
  ⧉  index
```
