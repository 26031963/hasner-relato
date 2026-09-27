# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 26/09/2026 22:00:02.**

| | |
|---|---|
| `HEAD` local | `d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga` |
| `origin/main` | `d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga` |
| commits a subir (`ahead`) | **0** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga
a497ed37 [O69 VINCULO-DO-DIA] o espelho e o cartao perguntam a celula: a regra propria tinha QUATRO copias
068e830b TICKETS: linhas da O69 (aplicada) e da O67 (medida) que a regua cobrou
a92ae9c4 [O69 VINCULO-DO-DIA] APLICADA em 09: 654,74 h, e os sobrepostos cairam de 116h para 7 minutos
9b67131e [O69 VINCULO-DO-DIA] o fechamento pergunta a CELULA qual vinculo vale no dia -- sobreposicao para de dobrar
```

## `git status --short`

```
 M app/config/crons_duracao.json
 M app/docs/BACKLOG.md
 M app/docs/PENDENTES_RONALD.json
 M app/docs/PROMPTS.md
 M app/docs/RELATO.md
?? app/ponto/management/commands/celula_veredito_velho.py
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```
  Três conclusões que só existem porque a segunda corrida foi feita:

  A rejulga de 09 move dinheiro zero. Os 158 vereditos e as 5.081 lavras de ata não mudam um centavo do gravado.

  Os +142,14 h são deriva pura — o FechamentoMensal de 09 está atrasado em relação ao motor de HEAD, e qualquer recálculo o traz ao presente, com O71 ou sem ela, hoje ou segunda.
  É o passivo que o handoff já registrava como "67 de 603 divergem".

  O efeito próprio da O71 no TXT são +2 colabs: col830 e col56, que saíam por furo_espelho. Os outros quatro (col964–967) entram pelo recálculo — não tinham FechamentoMensal e ele
  criou um.

  O que espera o seu ! é a deriva de +142,14 h / +83,33 h de HE. Ela não está em aval nenhum: o seu ! de critério cobre a parte (a) da O68, e este número não é dela.

  A O71 não está travada e eu sigo com ela: dinheiro gravado zero, ninguém sai, sua condição fechada. Fica dito apenas que o ganho dela no TXT só se materializa quando houver
  recálculo — e nesse instante a deriva vem no mesmo ato, porque é o mesmo ato.

  Sobre o contador: 158, contra as 1.504 da minha primeira versão. Não publiquei aquele número porque a matriz se denunciava — 822 nunca_bateu → concorde, e nunca_bateu não existe
  no _MAPA_VEREDITO. Eu havia derivado o veredito dos códigos e chamado aquilo de "o juiz de hoje", reproduzindo metade do juiz: a lei que eu tinha citado na docstring do próprio
  medidor. A versão que vale intercepta o escritor único.

✻ Worked for 1m 17s · done 9:55 PM · 2 shells still running

❯ aplica a O71 em prod (dinheiro zero) e segue pra O68

● Antes de aplicar: a sombra é de 04:15 e os applies da E3 e da O69 foram depois disso, então os números de prod podem não ser os mesmos. Rodo o ensaio em prod e só então aplico:

  Ran 1 shell command

● Ensaio em prod rodando (dentro de atomic() com raise — o mesmo padrão do apply da O69). Não toco em .py enquanto ele lê a árvore montada.

  O --aplicar que escrevi tem uma trava embutida que vale explicitar: ele desliga o recálculo à força. Se alguém (eu, amanhã, com menos contexto) passar --aplicar esperando ver o
  TXT mudar, o comando responde que o recálculo está desligado e por quê — porque o dinheiro gravado só fica em zero enquanto não há recálculo, e juntar os dois atos aplicaria sem
  aval os +142,14 h que o aval não cobre. E ele exige --antes, o snapshot das 17.332 células, que é a frase de reversão: sem arquivo, não aplica.

✻ Worked for 47s · done 9:56 PM · 2 shells still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · ← for agents · ↓ to manage
  ⧉  index
```
