# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 15:42:24.**

| | |
|---|---|
| `HEAD` local | `54a06418 [ESPELHO-VERDADE-E3-COMPLETA] a AMOSTRA achou um bug meu: o DIFF que publiquei estava INFLADO` |
| `origin/main` | `aa7b4b42 [ESPELHO-VERDADE-E3] PAREI: a Pauta DP de 07/08 mudou de numero E DE SINAL -- e o porque e a L-082` |
| commits a subir (`ahead`) | **2** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
54a06418 [ESPELHO-VERDADE-E3-COMPLETA] a AMOSTRA achou um bug meu: o DIFF que publiquei estava INFLADO
06311b07 [ESPELHO-VERDADE-E3-COMPLETA] PAREI: o col369 era uma CLASSE -- 164 colabs com turno aberto pelo tipo gravado
aa7b4b42 [ESPELHO-VERDADE-E3] PAREI: a Pauta DP de 07/08 mudou de numero E DE SINAL -- e o porque e a L-082
c8378cac [HAIKU-A-FABRICA-ESTA-LIGADA] a pergunta ganha DADO -- e sem snapshot a resposta e "nao sei", nunca "ligada"
6944da89 [CARD4-E-RESCISAO] as duas obras nascem LIGADAS -- o dossie LE o registro, nao reemite o PDF
```

## `git status --short`

```
 M app/config/crons.py
 M app/config/crons_duracao.json
 M app/docs/ARQUITETURA.mmd
 M app/docs/RELATO.md
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  Ran 1 shell command

● Todos os 8 são empresa 2 e 12x36, e em prod têm FolgaDia=0. Na 12x36 a folga vem do ciclo, não da tabela — então comparo o mesmo dado na sombra:

  Ran 1 shell command

● Dado idêntico em prod e na sombra. Mesmo código, mesmo dado, resultado diferente — então a diferença está no settings. Conferindo:

  Searched for 1 pattern

● Settings não explica — ele só desliga saídas. Medindo os dois lados com a mesma chamada restrita, para ver quem muda:

  Ran 1 shell command

● Agora o quadro é decisivo e não é dado: na sombra o motor reproduz 84,65; em prod ele zera. Mesmo código, mesmo dado. E os 8 são todos 12x36 da empresa 2 — a CCT dos vigilantes.
  Conferindo o ambiente:

  Ran 2 shell commands

● Falta um dado que eu não comparei: o tipo das células. E prod regenerou células hoje às 05:50, depois da cópia da sombra:

  Ran 3 shell commands

● Dado idêntico, código idêntico. Falta perguntar qual motor cada banco escolhe:

  Comparar HEAD contra prod para os mesmos colabs · 2s
  ⎿  $ cd /tmp/claude-1001/pauta/saida && python3 -c "
     import json
     H=json.load(open('diff_head.json')); N=json.load(open('diff_novo.json'))
     g=H['comp']['09']['gravado']; mh=H['comp']['09']['motor']; mn=N['comp']['09']['motor']
     for cid in ('203','877','278'):
     print('col%s SOMBRA: gravado=%s motorHEAD=%s …

✽ Wrangling… (17m 28s · ↓ 29.9k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · esc to interrupt · ← for agents
  ⧉  index
```
