# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 17:55:45.**

| | |
|---|---|
| `HEAD` local | `2fe6ce39 RELATO: a prontidao do export de 09 DEPOIS do apply -- 202 entram, 402 retidos, 337 por furo_espelho` |
| `origin/main` | `2fe6ce39 RELATO: a prontidao do export de 09 DEPOIS do apply -- 202 entram, 402 retidos, 337 por furo_espelho` |
| commits a subir (`ahead`) | **0** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
2fe6ce39 RELATO: a prontidao do export de 09 DEPOIS do apply -- 202 entram, 402 retidos, 337 por furo_espelho
858fe169 TICKETS: linha da DEPLOY-JA que a regua cobrou (fatia citada em fbd909bc sem linha)
fbd909bc [DEPLOY-JA] deploy feito e smoke em prod: o PDF diz 420 min, e o rotulo da tela vem de outro juiz
84ed1467 [ESPELHO-VERDADE-E3] a TESTEMUNHA passa a ler a mesma autoridade da folha (tela do espelho e PDF do cartao)
9e608c51 [ESMERIL-LAVRA-NO-BANCO] O63: dinheiro para de depender de arquivo do host, e sem lavra o recalculo RECUSA
```

## `git status --short`

```
 M app/config/crons_duracao.json
 M app/core/tests/test_selo_performance.py
 M app/docs/BACKLOG.md
 M app/docs/PROMPTS.md
 M app/docs/RELATO.md
 M app/docs/TICKETS.md
 M app/ponto/tests/test_selo_chokepoint_escrita.py
 M app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py
 M app/ponto/turnos.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  │               leitor               │                                       o que muda                                       │
  ├────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────┤
  │ dinheiro                           │ ZERO — nenhum dos 27 campos de nenhum dos 607 colabs se move, rollback provado 607/607 │
  ├────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────┤
  │ turnos abertos                     │ 806 → 744: 62 morrem, nenhum nasce; colabs com turno aberto 219 → 208                  │
  ├────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────┤
  │ chamados/perguntas de turno aberto │ os mesmos 62 deixam de nascer                                                          │
  ├────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────┤
  │ rótulos                            │ universo alcançado: 118 dias com tipo divergente do marco, em 77 colabs                │
  └────────────────────────────────────┴────────────────────────────────────────────────────────────────────────────────────────┘

  Dinheiro zero era a sua condição para aplicar sem nova parada.

  Dois erros meus no caminho, os dois pegos por medição:

  A primeira versão media 806 → 845 — os turnos abertos subiam 39 ao mesmo tempo que 11 colabs saíam da lista. Ata parcial relabela umas batidas e não outras, e isso quebra pares
  que estavam de pé. Curei com a mesma guarda do motor: a ata só dá o papel do dia quando explica o dia inteiro.

  E antes disso a cura ficou muda: a ata entrega instante_luz naive local, a batida vem aware em UTC, e eu comparei chave naive com busca aware — nenhuma casou. O col736 seguia
  com dois turnos abertos como se nada tivesse mudado. Chave que nunca casa é pior que cura ausente, porque parece aplicada.

  parear_turnos segue puro — recebe o papel como alimentação; quem lê a célula é o chamador, e o leitor é o mesmo juiz do motor. Nenhum juiz novo nasceu. O selo da E3 se estendeu:
  chamador com o colaborador na mão que não alimenta o papel fica vermelho.

  O preço, declarado: dois tetos de performance subiram com a justificativa escrita que o próprio selo exige (C8) — registrar_batida 22→23 (eliminei uma das duas queries passando
  as batidas já carregadas) e o calendário 25→27 (duas, porque a tela pergunta a dois leitores de turno). Tentei não pagar no calendário: turnos_do_colab aceita celulas=, mas o
  mapa nasce depois do laço no fluxo atual — fica como dívida com endereço, escrita no próprio selo.

  A O66 (BECO-FIO-FECHADO-CELULA-ABERTA) está registrada e entra quando a O65 fechar com deploy e smoke. Já comecei o censo dos fechadores: as vias chamado_encerrado,
  celula_absolveu e completude_turno vivem em chamados/juizes.py:844-879 e chamados/reconciliador.py:643; quem escreve validada_em está em
  chamados/services/materializacao.py:430,939 e disputa_emissao.py:1361. A Q2-JA "conversa parada" aparece em arquivar_competencia_encerrada.py e celula_carimbada.py — é onde vou
  conferir contra o DECIDIR-642.

✻ Churned for 32m 34s · done 5:49 PM · 1 shell still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ deploy e smoke quando a suite fechar, depois O66
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · ← for agents · ↓ to manage
  ⧉  index
```
