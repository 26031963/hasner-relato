# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 10:48:31.**

| | |
|---|---|
| `HEAD` local | `8c6fb1a6 TICKETS: linhas da O57 e do SELO-PDF-PREVISTO-SUMIU que o pre-push cobrou` |
| `origin/main` | `b81544bc RELATO: linha do vigia` |
| commits a subir (`ahead`) | **12** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald

## `git log --oneline -5`

```
8c6fb1a6 TICKETS: linhas da O57 e do SELO-PDF-PREVISTO-SUMIU que o pre-push cobrou
df1da882 [ESPELHO-VERDADE-E3] PAREI: achei um bug MEU antes de aplicar, e o numero do aval mudou
a8fa5dcf [O57] as DUAS curas do commit-carrega-o-indice, provadas com git de verdade -- e a fabrica desligada
2131dffa O57: limpa os alvos do teste empirico da O57
9ae97d41 O57 teste A: pathspec com delecao alheia no indice
```

## `git status --short`

```
 M app/config/crons_duracao.json
 M app/ponto/motor_calculo_v2.py
?? app/ponto/tests/test_e3_metade_intervalo_cadastrado.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```

  Também fechado neste bloco: O57 curada com as duas curas provadas com git de verdade (pathspec + tripwire que recusa deleção não declarada, 6 casos de selo, o MORDE sendo o
  índice literal do b4eacf03); fabricante DESLIGADO com dono e condição de saída, timer disabled, e fabricante: DESLIGADO no ESTADO.md; SESSAO.md publicado (583 ações, sem saída
  de comando, selo medindo 7 documentos); e o .gitignore que escondia a app do cartão-PDF.

✻ Baked for 1h 40m 34s · done 10:38 AM

❯ aval Ronald: E3 metade v2 (max(cadastrado, piso)) aplica SO 09/2026, todas as empresas: +13,04 h, 18 colabs. 07 (+25,26 h, 14) e 08 (+27,36 h, 22) NAO mudam o gravado em
  empresa nenhuma: viram Pauta DP por empresa. RELATO com col207 e col118 nomeados e o hash de origin/main. !

● O ! no número certo. Restaurando a cura v2 na árvore:

  Ran 1 shell command

● Cura v2 na árvore e provada. Mas o push28 falhou, e a causa é um bug na cura da O56 do fabricante — o container escreve .ruff_cache como root na árvore temporária e o rm -rf não
  consegue limpar:

  Ran 1 shell command

● Duas coisas distintas: as 6 falhas são o portão, e os rm são vazamento de disco depois. Vendo as falhas:

  Ran 1 shell command

● Duas causas, e as duas são bugs na cura da O56 do fabricante — que está me bloqueando. Medindo antes de curar:

  Ran 1 shell command

● Confirmado: staticfiles/ é gitignored, então o git archive não o traz e os smokes de clique perdem CSS/JS; e o container escreve .ruff_cache/.hypothesis como root, o que vaza 43
  árvores (~1 GB). Curando as duas:

● Ler os dois sitios a curar
  ⎿  $ cd /home/ronald/saas-hasner && sed -n 25,40p bin/arvore_do_push.sh; echo "=== o container do pre-push ==="; sed -n 100,110p bin/pre-push.sh

✢ Zigzagging… (1m 25s · ↓ 5.0k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · esc to interrupt · ← for agents
  ⧉  index
```
