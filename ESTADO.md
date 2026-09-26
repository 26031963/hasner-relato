# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 16:09:01.**

| | |
|---|---|
| `HEAD` local | `dd0d7d68 [ESPELHO-VERDADE-E3-COMPLETA] PAREI no apply: o dinheiro bate inteiro, e a SOMBRA mentiu uma vez hoje` |
| `origin/main` | `aa7b4b42 [ESPELHO-VERDADE-E3] PAREI: a Pauta DP de 07/08 mudou de numero E DE SINAL -- e o porque e a L-082` |
| commits a subir (`ahead`) | **3** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
dd0d7d68 [ESPELHO-VERDADE-E3-COMPLETA] PAREI no apply: o dinheiro bate inteiro, e a SOMBRA mentiu uma vez hoje
54a06418 [ESPELHO-VERDADE-E3-COMPLETA] a AMOSTRA achou um bug meu: o DIFF que publiquei estava INFLADO
06311b07 [ESPELHO-VERDADE-E3-COMPLETA] PAREI: o col369 era uma CLASSE -- 164 colabs com turno aberto pelo tipo gravado
aa7b4b42 [ESPELHO-VERDADE-E3] PAREI: a Pauta DP de 07/08 mudou de numero E DE SINAL -- e o porque e a L-082
c8378cac [HAIKU-A-FABRICA-ESTA-LIGADA] a pergunta ganha DADO -- e sem snapshot a resposta e "nao sei", nunca "ligada"
```

## `git status --short`

```
 M app/config/crons_duracao.json
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  isso que evitou gravar contra número errado.

  O dinheiro bate inteiro. Nenhum campo de dinheiro, de nenhum colab, divergiu do esperado. Sobraram 606 divergências de previsto_em — carimbo de quando a conta rodou, que não
  pode bater por construção; isso é defeito do meu critério, que devia tê-lo excluído por nome. E uma real: inconsistencias do col369, prod calcula 10, a sombra calculou 14.
  Determinístico nos dois, duas corridas cada, com dado provado idêntico. Não é dinheiro, e não está explicado.

  O achado vale mais que a fatia. A primeira conferência acusou 8 colabs com horas_folga_trabalhada = 0 em prod contra até 134,71 h na sombra. Eliminei uma variável por vez — dado
  (vínculos, FolgaDia, células com trabalha/origem/regeneradas, batidas, ausências, vereditos), motor e parâmetros de CCT, imagem (mesmo sha), Python, libs, settings, bytecode
  com -B: tudo idêntico. A causa saiu instrumentando builtins.open:

  fechamento.py:231 lê logs/esmeril_espelho.json — porque o corte CLASSE3-FOLGA-100 diz que folga trabalhada paga 100% só com a escala certa no dia, e quem diz isso é a lavra do
  esmeril, um arquivo. A sombra roda com --tmpfs /app/logs: o arquivo não existe lá, a lavra vem vazia, ninguém é excluído e a folga é paga. Prod está certo; meu esperado estava
  errado. Montei a lavra e as 8 divergências sumiram (col203 foi de 84,65 para 0,00).

  Duas consequências, que abri como O63: todo ensaio de folha na sombra superestimou folga trabalhada desde aquele corte — é desenho do ensaio, não erro de hoje; e um campo de
  dinheiro depende de arquivo do host, falhando para o lado caro (sem o arquivo, todo mundo tem "escala certa").

  A prova dos 26 campos está no RELATO. Em prod: intra −33,33 h (49 colabs), trabalhadas +390,25 h (81), turnos_abertos −141 (50 colabs, e não sobe em nenhum), horas_falta zero.
  Diferem dos números do aval porque aqueles foram medidos contra o gravado da sombra e estes contra o de prod — a deriva que você aceitou.

  Uma linha sua destrava: o col369 com inconsistencias 10 em vez de 14 entra? É contador, não dinheiro, e a fonte do 14 é o ambiente que hoje se provou infiel — não vou decidir
  por você qual dos dois vale. Com o ok, aplico na hora; o ensaio já está escrito e reverte sozinho se algo sair do lugar.

  Também curei os 5 vermelhos que a suíte completa achou, todos meus (papel lavra do cron novo, horário movido para 07:31, ARQUITETURA.mmd regenerado). Falta empurrar dd0d7d68 e
  dois anteriores.

✻ Cogitated for 43m 17s · done 4:08 PM

❯ aval Ronald: E3 completa APLICA em 09/2026 pelo ensaio de hoje (603/603). previsto_em sai do criterio (carimbo). col369 inconsistencias = valor de PROD (10); a sombra e infiel
  (O63). Esperado em prod: intra -33,33 h (49 colabs), trabalhadas +390,25 h (81), turnos_abertos -141 (50, nao sobe em nenhum), horas_falta zero; a diferenca para o aval
  anterior e a deriva aceita. PROVA pos-apply igual ao ensaio, campo a campo. Em seguida O63 (P7.1): dinheiro nunca depende de arquivo do host -- a lavra do esmeril que decide a
  folga 100% passa a vir do banco; sem lavra = PAREI com alarme, nunca paga todo mundo nem ninguem em silencio; o ensaio na sombra passa a montar a lavra. !

✢ Billowing… (7s · thinking)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · esc to interrupt · ← for agents
  ⧉  index
```
