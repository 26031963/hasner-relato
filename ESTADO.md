# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 01:24:03.**

| | |
|---|---|
| `HEAD` local | `d64f1a53 TICKETS: linha do ESTADO-PUBLICADO que o pre-push cobrou (fatia citada em commit sem linha na tabela)` |
| `origin/main` | `4ea0942b [ESPELHO-VERDADE-E0] o errexit tambem calava o push, e a minha mudanca "por coerencia" no export sai` |
| commits a subir (`ahead`) | **36** |

## `git log --oneline -5`

```
d64f1a53 TICKETS: linha do ESTADO-PUBLICADO que o pre-push cobrou (fatia citada em commit sem linha na tabela)
445e43fe RELATO: BUG-C (47 de 51 invisiveis), BUG-D (1.377 linhas indistinguiveis) e o cron que espera !
4e467cc9 [BUG-B] o leitor que o meu censo perdeu, e a parentese dupla
02f4146b [BUG-D] `modelo` diz de que tabela e o `objeto_id`, ou nao diz nada
6abe6223 [BUG-C] a lista de "cadastro x realidade" nao via a classe mais crua do cadastro
```

## `git status --short`

```
 M app/colaboradores/services/vinculo.py
 M app/escala/services/cadastro_realidade.py
```

## painel da sessao (ultimas 40 linhas)

```
● Relato publicado (145 substituições, 0 vazamento). Agora o item (7) — porta da E1. Medindo os três selos de frota em prod:

  Searched for 1 pattern, ran 1 shell command

● SELO 3 fechado (0). SELO 2 caiu de 15 para 1 colab — e é o col899, exatamente o vínculo que restaurei ontem. Medindo antes de concluir:

  Ran 1 shell command

● Achado: não é bug de fase — é a fronteira entre dois vínculos. Dias 21-24 vêm do ec1311 (6x1) e 25 do ec1310, e meu selo contava a corrida atravessando a troca. Remedindo pelo
  vínculo gerador, e de passo um censo do cadastro que mente:

  Ran 1 shell command

● SELO 2 = 0 medido corretamente — o "1" era falso positivo do meu próprio selo atravessando a troca de vínculo. E aparece um achado de cadastro: 50 dos 132 templates se declaram
  12x36/24x48 com jornada < 11h ou folga por dia-da-semana (16 vínculos ativos).

  Searched for 2 patterns, listed 1 directory, ran 7 shell commands

● O golden vive na stack da mensageria (nucleo/ não está neste repo); o que é meu aqui é o número que ela consome. E ele tem de sair da mesma derivação que a lista usa — então o
  censo vai morar junto da lei:

  Ran 2 shell commands

● Agora o contador de vigia que a tela e o copiloto leem, pelo mesmo censo:

  Ran 1 shell command

● O irmão vigencia_sem_trilha tem a mesma comparação escrita de novo (data_fim__lt=F(...)) — o que acabei de proibir. Ele passa a ler o censo:

  Running 1 shell command…
  ⎿  $ sed -n 60,110p escala/services/vigencia_sem_trilha.py

· Quantumizing… (38m 29s · ↓ 114.1k tokens)
  ⎿  Tip: Use /clear to start fresh when switching topics and free up context
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · esc to interrupt · ← for agents
  ⧉  index
```
