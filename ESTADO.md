# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:85 AFIRMA ato sem PROVA ao lado -- MODO 24H (aval Ronald 27/09 03:4x). **APLICADOS: 4 colabs**, movimento so nos campos do item
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 00:55:31.**

| | |
|---|---|
| `HEAD` local | `75d4e150 [E6-PISO+O76+L-084/L-086] a hora de folga que sumia volta (+431,90 h em 11), e o dia do turno passa a sair do juiz` |
| `origin/main` | `d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga` |
| commits a subir (`ahead`) | **3** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
75d4e150 [E6-PISO+O76+L-084/L-086] a hora de folga que sumia volta (+431,90 h em 11), e o dia do turno passa a sair do juiz
c060e70a [O68b+L-084..L-092] o corte pelo VAO no ar, 8 leis de dominio indexadas, e o MODO 24H separando por colab
4f82c385 [O71+O68] o cadastro que mente: NENHUM dos 37 "turno_partido" e partido, e o maior vao e 70 min
d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga
a497ed37 [O69 VINCULO-DO-DIA] o espelho e o cartao perguntam a celula: a regra propria tinha QUATRO copias
```

## `git status --short`

```
 M app/config/crons_duracao.json
?? esteira.pausada
```

## painel da sessao (ultimas 40 linhas)

```

● Monitor "apply da cura da folga em 09" stream ended

  Ran 1 shell command

● Background command "Aplicar a cura da folga e conferir no banco" completed (exit code 0)

● Aplicado: 11 colabs, +431,90 h. No banco: col278 de 91,42 → 181,69 h, col203 de 98,96 → 183,61 h, col877 com 134,71 h. E o col865 preservou as 24,46 h de folga com escala certa,
  provando que a cura acrescenta o ramo em vez de trocar o critério. Zero entram ou saem do TXT; 07 e 08 intactos.

  Agora push até pousar:

  Ran 3 shell commands

● Commit 75d4e150, push51 em voo com a suíte completa sobre a árvore do commit. Vou até pousar — se cair, volto com a causa em uma linha e reempurro, sem pegar item novo no meio.

  O que esse commit carrega, e o que muda para as pessoas:

  +431,90 h voltam para 11 colaboradores. col278 de 91,42 para 181,69 h. col203 de 98,96 para 183,61 h. col877 ganha 134,71 h que estavam em campo nenhum. E a HE ficou em zero — o
  adicional de 100% que o corte nega não veio de graça junto.

  O col382 deixa de ter falta no 20/09 e recupera a semana de DSR. Ele entrou dez minutos depois da meia-noite e o sistema o registrava como ausente.

  A L-084 errada sai do ar. Desde 23:35 ela deixava de descontar atraso de quem chega mais de 3 h tarde com a saída na hora — o oposto do que a lei quer.

  Duas coisas que eu curei em mim antes de aplicar, e que ficam no commit porque valem mais que o número:

  O p not in _ft_certa comparava @dataclass por valor. Dois períodos com os mesmos campos se confundiriam, e o silêncio caía do lado de não pagar. Se eu tivesse aplicado antes de
  olhar, teria criado um bug novo dentro da cura de um bug de hora não paga.

  E eu criei um segundo mapa do dia do turno antes de procurar o primeiro — que existia, com a docstring dizendo "só o dia sai daqui". Passei a noite arrancando segundas regras de
  quatro lugares e quase plantei uma.

✻ Cooked for 1m 27s · done 12:53 AM · 2 shells still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ DEPLOY JA com smoke quando pousar
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · ← for agents · ↓ to manage
  ⧉  index
```
