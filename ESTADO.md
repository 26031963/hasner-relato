# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 15:21:01.**

| | |
|---|---|
| `HEAD` local | `06311b07 [ESPELHO-VERDADE-E3-COMPLETA] PAREI: o col369 era uma CLASSE -- 164 colabs com turno aberto pelo tipo gravado` |
| `origin/main` | `aa7b4b42 [ESPELHO-VERDADE-E3] PAREI: a Pauta DP de 07/08 mudou de numero E DE SINAL -- e o porque e a L-082` |
| commits a subir (`ahead`) | **1** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
06311b07 [ESPELHO-VERDADE-E3-COMPLETA] PAREI: o col369 era uma CLASSE -- 164 colabs com turno aberto pelo tipo gravado
aa7b4b42 [ESPELHO-VERDADE-E3] PAREI: a Pauta DP de 07/08 mudou de numero E DE SINAL -- e o porque e a L-082
c8378cac [HAIKU-A-FABRICA-ESTA-LIGADA] a pergunta ganha DADO -- e sem snapshot a resposta e "nao sei", nunca "ligada"
6944da89 [CARD4-E-RESCISAO] as duas obras nascem LIGADAS -- o dossie LE o registro, nao reemite o PDF
be72e9d5 [FABRICANTE-DESLIGADO] o censo que eu declarei fechado no commit anterior NAO estava -- e a lista agora e selo
```

## `git status --short`

```
 M app/config/crons.py
 M app/config/crons_duracao.json
 M app/docs/BACKLOG.md
 M app/docs/PROMPTS.md
 M app/docs/RELATO.md
 M app/ponto/motor_calculo_v2.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  dentro do período:

  Ran 2 shell commands

● Agora está limpo: zero dias suspeitos, e dois dos cinco colabs ficaram exatamente neutros — o "ganho" deles era todo o meu bug. Refazendo o DIFF completo, que estava inflado:

  Ran 2 shell commands

● Os números corrigidos são muito diferentes dos que publiquei — e a direção inverteu. Rodando as suítes e corrigindo o RELATO:

  Ran 2 shell commands

● Item (2) da ordem: a Pauta de 08 compara com o que foi pago, não com o nosso gravado. Medindo se existe recibo do Domínio no sistema:

  Searched for 1 pattern, ran 1 shell command

● Existe: ExportacaoDominio guarda cópia fiel do TXT entregue com hash. Medindo o que há para 08/2026:

  Ran 1 shell command

● Dois achados que mudam o item (2):

  Ran 1 shell command

● Cuidado: isso é a sombra, que pode ter raspado o campo. Conferindo em prod — só tamanho, sem conteúdo:

● Conferir o conteudo em prod, so tamanho
  ⎿  $ cd /home/ronald/saas-hasner && docker exec saas_core python manage.py tenant_command shell --schema=juliani --command "
     from folha.models import ExportacaoDominio as E
     from collections import Counter
     c = Counter(len(y.conteudo or '') for y in E.objects.all())
     print('PROD -- distribuicao do tamanh…

· Twisting… (21m 26s · ↓ 46.4k tokens · thinking)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
