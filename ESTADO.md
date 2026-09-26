# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 19:36:31.**

| | |
|---|---|
| `HEAD` local | `fac5f59c [O69 VINCULO-DO-DIA] o MEDIR ANTES respondido: os 4 de zero vinculo NAO sao da E1, mas 3 dos 25 sao` |
| `origin/main` | `af7a6425 [O65] a cauda: cartao_x_txt=0 e 09 estavel, mas NAO declaro liberada -- pdf_x_espelho nao tem medidor` |
| commits a subir (`ahead`) | **5** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
fac5f59c [O69 VINCULO-DO-DIA] o MEDIR ANTES respondido: os 4 de zero vinculo NAO sao da E1, mas 3 dos 25 sao
cb4152ef [O67] PAREI no item (6): 85 colabs, 3.268h56 -- e os meus DOIS medidores estavam cegos ao RED
e571febd [O67] 09 CONDICIONAL: as tres medicoes de frota com a lista -- 203 no TXT, dos quais 9 com ajuste
6cf370e6 [PDF-X-ESPELHO-MEDIDOR] 09/2026 LIBERADA -- e o medidor que faltava agora existe em codigo
3046eef0 [BECO-FIO-FECHADO-CELULA-ABERTA] O66 medida: celula_furo_sem_canal = 68, e nenhum fechador consulta a celula
```

## `git status --short`

```
 M app/config/crons_duracao.json
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```

  (1) O dinheiro são nove, somando 334h20:

  ┌─────────────────────────┬───────────────────────┬────────┬────────┬──────────────────────────┐
  │          colab          │       diferença       │  topo  │ coluna │          causa           │
  ├─────────────────────────┼───────────────────────┼────────┼────────┼──────────────────────────┤
  │ col924                  │ 132h40                │ 0h00   │ 132h40 │ zero vínculo na janela   │
  ├─────────────────────────┼───────────────────────┼────────┼────────┼──────────────────────────┤
  │ col866                  │ 84h03                 │ 239h57 │ 155h54 │ vínculo sobreposto       │
  ├─────────────────────────┼───────────────────────┼────────┼────────┼──────────────────────────┤
  │ col391 · col43 · col942 │ 31h57 · 29h20 · 28h09 │ 0h00   │ —      │ zero vínculo na janela   │
  ├─────────────────────────┼───────────────────────┼────────┼────────┼──────────────────────────┤
  │ col935 · col648         │ 11h02 · 1h29          │ —      │ —      │ troca de vínculo no meio │
  ├─────────────────────────┼───────────────────────┼────────┼────────┼──────────────────────────┤
  │ col400 · col245         │ 8h38 · 7h01           │ —      │ —      │ sem causa dessas         │
  └─────────────────────────┴───────────────────────┴────────┴────────┴──────────────────────────┘

  Os outros 16 (867h35) estão retidos — não foram pagos.

  (2) Sua hipótese, confirmada na assinatura que você previu: dos 9 com gravado maior que o espelho, 5 têm vínculo sobreposto — col277, col899, col866, col107, col736. Mais 8 com
  troca de vínculo no meio.

  (3) O MEDIR ANTES: os 4 de zero vínculo não estão no passivo da E1 (53 registros em 51 colaboradores) — interseção vazia, raiz própria. Mas col277, col736 e col866 estão, e são
  três dos cinco sobrepostos: vigência impossível e sobreposição andam juntas neles, e a restauração vai pelo propositor com seu !, como você previu.

  E eu quase respondi errado aqui: a primeira leitura do censo deu "53 registros em 1 colaborador" porque vigencias_impossiveis devolve tuplas (pk, colaborador_id, ini, fim,
  ativa) e eu extraí com .get('colaborador_id') — None em todas, colapsando o conjunto. O "não" só passou a valer depois de eu ler o contrato da função.

  A O69 está registrada com a raiz nomeada: enquanto o fechamento exigir vínculo ativo e o espelho pegar o último, as duas pontas (o zero e o dobrado) são produzidas por
  construção. O juiz único que você definiu — CelulaDia.escala_geradora — resolve as duas de uma vez.

  Nada tocado em dinheiro. O próximo passo dela é o DIFF na sombra com os 25 e a lista dos 9, que é o que vai ao seu !.

✻ Baked for 2m 46s · done 7:29 PM
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ roda o DIFF na sombra dos 25 com a lista dos 9
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · ← for agents
  ⧉  index
```
