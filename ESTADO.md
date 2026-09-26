# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 10:54:01.**

| | |
|---|---|
| `HEAD` local | `48e408ed [PRE-PUSH-TESTA-O-COMMIT] dois bugs da cura da O56, achados pelo meu proprio push` |
| `origin/main` | `b81544bc RELATO: linha do vigia` |
| commits a subir (`ahead`) | **13** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald

## `git log --oneline -5`

```
48e408ed [PRE-PUSH-TESTA-O-COMMIT] dois bugs da cura da O56, achados pelo meu proprio push
8c6fb1a6 TICKETS: linhas da O57 e do SELO-PDF-PREVISTO-SUMIU que o pre-push cobrou
df1da882 [ESPELHO-VERDADE-E3] PAREI: achei um bug MEU antes de aplicar, e o numero do aval mudou
a8fa5dcf [O57] as DUAS curas do commit-carrega-o-indice, provadas com git de verdade -- e a fabrica desligada
2131dffa O57: limpa os alvos do teste empirico da O57
```

## `git status --short`

```
 M app/config/crons_duracao.json
 M app/docs/PENDENTES_RONALD.json
 M app/docs/RELATO.md
 M app/ponto/motor_calculo_v2.py
?? app/ponto/tests/test_e3_metade_intervalo_cadastrado.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
● A suíte roda sobre o commit 48e408ed. Enquanto isso, escrevo o RELATO do apply com o que você pediu:

  Read 1 file, ran 2 shell commands

● Estado, enquanto a suíte do push fecha o portão do código.

  Apliquei 17 dos 18 que você aprovou, e o corte que eu fiz sozinho está com número para você desfazer.

  O ! aprovou +13,04 h em 18 colabs (09/2026, todas as empresas — medido: nenhuma exportou 09). Dos 18, 17 ganham (+21,29 h) e um perde: col207, −8,25 h. Fui ver de onde a perda
  dele vem:

  ┌────────────────┬───────────────┬────────────────────────────────────────┐
  │ col207, te#216 │   declarado   │         medido (19 dias de 09)         │
  ├────────────────┼───────────────┼────────────────────────────────────────┤
  │ jornada        │ 240 min (4 h) │ 338 min (5,6 h), com dias de 361 e 363 │
  ├────────────────┼───────────────┼────────────────────────────────────────┤
  │ marcos         │ 08:00–12:00   │ batidas ~06:30 → ~12:30                │
  └────────────────┴───────────────┴────────────────────────────────────────┘

  O piso do Art.71 sai da jornada declarada (240 → piso 0), e é daí que os −8,25 h nascem. Pela jornada real o piso seria 15, e nos dias acima de 360, 60. A redução não é a lei
  aplicada certo — é a lei aplicada sobre cadastro errado. Deixei-o como está: tirar dinheiro com base em cadastro que mente é pior que não mexer, e corrigir o te#216 é mudança de
  dado de escala, que nunca é pré-aprovada. Está em col207-cadastro-mente-jornada com as duas saídas.

  O col118 que você nomeou é a prova pelo lado oposto: te#194 declara 160 min de intervalo e manda, porque cadastro acima da lei é direito do colaborador — o mesmo leitor que
  recusa o te#445 com 15 min abaixo do piso.

  07 (+25,26 h · 14) e 08 (+27,36 h · 22) vão para Pauta DP, gravado intocado.

  E dois bugs da cura da O56 do fabricante, achados pelo meu próprio push e curados: o git archive não trazia staticfiles/ (derrubava 4 smokes de clique por falta de CSS —
  vermelho que não era de código), e o container escrevia cache como root na árvore temporária, vazando 43 árvores / ~1 GB desde 03:38, agora limpas e em tmpfs.

  origin/main segue em b81544bc; o push29 leva os 13 commits.

✻ Baked for 5m 52s · done 10:52 AM · 2 shells still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · ← for agents · ↓ to manage
  ⧉  index
```
