# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 17:24:01.**

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
 M app/docs/PROMPTS.md
 M app/ponto/turnos.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```

● _turno_aberto_calc é justamente quem alimenta chamado e pergunta de turno aberto — o que a ordem quer ver morrer. Alimentando:

  Ran 2 shell commands

● A chave vinha naive da ata e a busca aware — nunca casavam. Normalizando as duas para local naive:

  Ran 1 shell command

● 08:47-16:23 fechado — a geometria agora concorda com o motor. Medindo o RED da ordem: rótulo da tela = PDF = motor:

  Ran 1 shell command

● RED fechado: rótulo da tela = ok nos dois, e o PDF em 420 min. Agora o DIFF por leitor — ensaio em prod com rollback, que é o que decide se aplica:

  Ran 1 shell command

● ZERO campos se movem — nem dinheiro, nem contador. O FM já vinha do motor curado; a O65 só muda quem pergunta à geometria. Medindo esse lado, que é o que a ordem pede:

  Ran 1 shell command

● Surpresa que impede aplicar assim: 11 colabs deixam de ter turno aberto, mas o total sobe de 806 para 845. Ata parcial relabela umas batidas e não outras — o mesmo risco que o
  motor já tinha. Aplicando a mesma guarda:

  Ran 1 shell command

● Agora na direção certa e sem nascer nenhum: 806 → 744 turnos abertos, 62 morrem, e 11 colabs saem da lista. Reconfirmando que dinheiro segue ZERO:

● Reconfirmar dinheiro zero e o RED · 57s
  ⎿  $ cd /home/ronald/saas-hasner && docker exec saas_core python manage.py tenant_command shell --schema=juliani --command "exec(open('/tmp/o65.py').read())" 2>&1 | grep -E
     "ROLLBACK|processados|SOMA|nenhum|^[a-z_]+ +[-+]" | head -8; echo "=== RED dos dois casos ==="; docker exec saas_core python mana… (56s)
     (ctrl+b ctrl+b (twice) to run in background)

✢ Pouncing… (7m 21s · ↓ 17.9k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · esc to interrupt · ← for agents
  ⧉  index
```
