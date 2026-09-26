# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 15:47:51.**

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
  Ran 2 shell commands

● Falta um dado que eu não comparei: o tipo das células. E prod regenerou células hoje às 05:50, depois da cópia da sombra:

  Ran 3 shell commands

● Dado idêntico, código idêntico. Falta perguntar qual motor cada banco escolhe:

  Ran 2 shell commands

● A bisseção ficou clara: em prod, HEAD mantém 84,65; meu código zera. Na sombra, meu código mantém. Falta isolar se o diferenciador é o banco ou o container:

  Ran 3 shell commands

● Arquivo e mount idênticos. Há um except no meu código que engole erro e loga — conferindo se ele disparou em prod:

  Ran 1 shell command

● Percebi a diferença real entre as duas corridas: a primeira recalculou a frota inteira, as outras só 8 colabs. Testando se o resultado depende do escopo da chamada:

  Ran 1 shell command

● Vou comparar os dois ambientes de verdade — o container de prod tem 18 h de vida e a imagem :latest pode ter mudado:

  Ran 1 shell command

● Mesma imagem, mesmo Python. Então preciso saber se o resultado reproduz. Rodando o mesmo ensaio outra vez:

● Reproduzir o ensaio em prod · 59s
  ⎿  $ cd /home/ronald/saas-hasner && docker exec -e MODO=ensaio saas_core python manage.py tenant_command shell --schema=juliani --command "exec(open('/tmp/apply09.py').read())"
     2>&1 | grep -E "ROLLBACK|divergencia|horas_folga_trabalhada|horas_trabalhadas|col203|turnos_abertos" | head -10 (58s)
     (ctrl+b ctrl+b (twice) to run in background)

· Wrangling… (22m 56s · ↓ 42.2k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · esc to interrupt · ← for agents
  ⧉  index
```
