# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 10:05:45.**

| | |
|---|---|
| `HEAD` local | `2131dffa O57: limpa os alvos do teste empirico da O57` |
| `origin/main` | `b81544bc RELATO: linha do vigia` |
| commits a subir (`ahead`) | **9** |

## `git log --oneline -5`

```
2131dffa O57: limpa os alvos do teste empirico da O57
9ae97d41 O57 teste A: pathspec com delecao alheia no indice
fe379ed6 O57 teste: commit com pathspec enquanto ha delecao alheia no indice
e37398b8 O57: alvo temporario do teste empirico
66e82905 RELATO: o DIFF da E3 METADE (+39,41 h) e o que ela NAO cura -- e a lei L-081
```

## `git status --short`

```
 M app/config/crons_duracao.json
?? 0
?? bin/commit_so_o_declarado.sh
?? bin/gerar_sessao.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
● Encontrado: hasner-fabricante.timer (a cada 30 min, próximo às 10:30). Desligando com a trilha exata que você deu:

  Ran 1 shell command

● Fabricante DESLIGADO: pausa com dono e timer disabled+inactive, fora da lista de disparos. Agora a cura da O57 — os sítios que commitam e o pre-commit:

  Ran 1 shell command

● Só um sítio da casa commita no repo principal por script (relato.sh, e num repo separado). Mas os commits "linha do vigia" e "placar e rodape" existem — achando quem os faz, que
  é o item (2):

  Ran 1 shell command

● Achei os sítios. Escrevendo o tripwire da O57 — e vou medir uma subtileza em vez de supor: com git commit -- <paths>, o que o hook vê é o índice temporário ou o real?

  Ran 1 shell command

● Tenho uma deleção staged — a forma exata do dano. Medindo se git commit -- <paths> a carrega:

  Ran 1 shell command

● Provado: com git commit -- <paths> o commit levou só A _o57_outro.txt, e a deleção alheia ficou no índice, intocada. A cura (a) basta contra o mecanismo. Agora testo se o hook
  vê o índice temporário ou o real:

  Ran 1 shell command

● O hook vê o índice temporário — as duas curas coexistem sem falso positivo. Agora o teste que morde: commit sem pathspec com a deleção alheia no índice:

  O caso que MORDE, ao vivo
  ⎿  $ cd /home/ronald/saas-hasner && echo "outra linha" >> app/docs/_o57_outro.txt && git add app/docs/_o57_outro.txt && echo "indice:"; git diff --cached --name-status | sed
     's/^/ /'; echo "=== TESTE B: commit SEM pathspec (deve RECUSAR) ==="; git commit -q -m "O57 teste B: sem pathspec, com delecao a…

· Transmogrifying… (1h 8m 7s · ↓ 76.6k tokens)
  ⎿  Tip: Use /clear to start fresh when switching topics and free up context
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · esc to interrupt · ← for agents
  ⧉  index
```
