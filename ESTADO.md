# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 12:57:16.**

| | |
|---|---|
| `HEAD` local | `6944da89 [CARD4-E-RESCISAO] as duas obras nascem LIGADAS -- o dossie LE o registro, nao reemite o PDF` |
| `origin/main` | `6944da89 [CARD4-E-RESCISAO] as duas obras nascem LIGADAS -- o dossie LE o registro, nao reemite o PDF` |
| commits a subir (`ahead`) | **0** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
6944da89 [CARD4-E-RESCISAO] as duas obras nascem LIGADAS -- o dossie LE o registro, nao reemite o PDF
be72e9d5 [FABRICANTE-DESLIGADO] o censo que eu declarei fechado no commit anterior NAO estava -- e a lista agora e selo
9b033fa2 [FABRICANTE-DESLIGADO] o alarme para de contar a fabrica e passa a contar a SESSAO (item 8)
4c081b17 [PRE-PUSH-TESTA-O-COMMIT] o symlink que so o HOST via -- e o handoff antes do compact
83bf14e9 [PRE-PUSH-TESTA-O-COMMIT] o comentario que EXECUTOU e matou o push com rc=125
```

## `git status --short`

```
 M app/api/urls.py
 M app/api/views_mensageria.py
 M app/config/crons_duracao.json
 M app/docs/HAIKU-DENTES.md
 M bin/gerar_estado.py
 M mensageria/nucleo/core_client.py
 M mensageria/nucleo/ferramentas.py
?? app/api/tests/test_selo_helper_nao_tem_decorador_de_view.py
?? app/inteligencia/management/commands/lavrar_fabrica.py
?? bin/fabrica_estado.py
?? esteira.pausada
?? mensageria/nucleo/tests/test_fabrica_ligada.py
```

## painel da sessao (ultimas 40 linhas)

```

● Sem guarda no sítio da chamada. Medindo se isso aparece em prod:

  Ran 5 shell commands

● Provado no container de produção: a linha 271 explode com TypeError. Qualquer request no caminho por empresa dá 500, e o Haiku engole isso como core_indisponivel — fica cego sem
  dizer por quê. Curando na origem:

  Ran 1 shell command

● Selo que morde, por AST (não por regex, que leria minha própria prosa):

  Ran 1 shell command

● Agora o endpoint que o Haiku consulta, lendo os dois snapshots:

  Ran 2 shell commands

● Passo 4 — o lado da mensageria: cliente, bloco e a costura no contexto:

  Ran 1 shell command

● Passo 5 — o golden "a fábrica está ligada?", com o caso que morde:

  Ran 5 shell commands

● Rodando a cadeia inteira de ponta a ponta:

  Rodar api e inteligencia · 2s
  ⎿  $ cd /home/ronald/saas-hasner && source bin/recursos.sh >/dev/null 2>&1 && teste_envfile >/dev/null && ESTEIRA_QUEM=selo bash bin/trava_teste.sh -- docker run --rm
     $TESTE_DOCKER --network saas-hasner_default --env-file /home/ronald/saas-hasner/.env --env-file "$TESTE_ENVFILE" -e PYTHONDONTWRITEBYTE…

✽ Misting… (1h 3m 24s · ↓ 110.6k tokens)
  ⎿  Tip: Use /clear to start fresh when switching topics and free up context
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · esc to interrupt · ← for agents
  ⧉  index
```
