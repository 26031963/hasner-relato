# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:85 AFIRMA ato sem PROVA ao lado -- MODO 24H (aval Ronald 27/09 03:4x). **APLICADOS: 4 colabs**, movimento so nos campos do item
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 01:06:01.**

| | |
|---|---|
| `HEAD` local | `c6c3785b [E6-PISO+O76+L-084/L-086] a hora de folga que sumia volta (+431,90 h em 11), e o dia do turno passa a sair do juiz` |
| `origin/main` | `d39c0696 [O68 PLANO-B-MEDIDO] o plano B tem tamanho: 758 dia-colab, 11,0% do que o motor julga` |
| commits a subir (`ahead`) | **3** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
c6c3785b [E6-PISO+O76+L-084/L-086] a hora de folga que sumia volta (+431,90 h em 11), e o dia do turno passa a sair do juiz
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

## PLACAR ESPELHO-VERDADE

| etapa | feitos | itens do programa |
|---|---:|---|
| **E1** PREVISAO INTEGRA | **1/3 = 33%** | selos de frota = 0: vinculo com fim<inicio; 12x36 com 3+ trabalha seguidos; dia de colab ativo sem previsao ou |
| **E3** MOTOR PELO JUIZ | **4/4 = 100%** | o motor le periodos do juiz da batida e jornada do juiz do previsto; DIFF no RELATO + `!` |
| **E4** LEITORES NO MESMO NUMERO | **2/5 = 40%** | selo tela == PDF == fechamento == TXT na frota, 0 divergencia |
| **E5** FECHAMENTO ONLINE | **0/2 = 0%** | fechamento = LEITURA; `recalcular` deixa de existir; so atos persistem |
| **E6** CERTIFICACAO POR ORACULO INDEPENDENTE | **2/4 = 50%** | 0 divergencia nao explicada + 0 dia sem previsao + 0 periodo fora do juiz |

| etapa | item | estado | prova |
|---|---|---|---|
| E1 | vinculo com data_fim < data_inicio (escritor unico + saneamento) | **espera Ronald** | medido: 51 colabs com vigencia impossivel (lavrar_vigencia_impossivel); restauracao pelo propositor espera o `!` |
| E1 | celula de trabalho sem previsao valida | em curso | medido: 4 colabs com zero vinculo E zero celula (col924/391/43/942, ~221 h) |
| E1 | qual vinculo vale no dia tem UM juiz (CelulaDia.escala_geradora) | FEITO | O69 aplicada em 09: 654,74 h; `escala/alimentacao.py::vinculo_do_dia`; selo `ponto.tests.test_vinculo_do_dia_pela_celula` (8 casos) |
| E3 | o MARCO manda, nunca o tipo gravado | FEITO | selo `ponto.tests.test_e3_completa_o_marco_manda` + `test_selo_motor_nao_pareia_pelo_tipo_gravado`; aplicada em 09 |
| E3 | jornada do dia vem de minutos_previstos_do_dia, nunca de minutos_jornada | FEITO | HAIKU `jornada_de_fonte_lixo` (lavrar_jornada_lixo, 11 acessos em 4 arquivos) |
| E3 | os leitores de turno tambem perguntam ao juiz da batida (O65) | FEITO | O65 aplicada: dinheiro ZERO, turnos abertos 806 -> 744; selo `SeloLeitorDeTurnoTambemLeAAtaTest` |
| E3 | a jornada pertence ao dia de INICIO, nas tres derivacoes (O76) | FEITO | O76: `MotorBase._dia_do_turno` passa a servir o MotorBase; RED col382 DSR perdido=1 -> ok=1; aplicada 27/09 |
| E4 | o cartao PDF desenha o mesmo que a tela | FEITO | `pdf_x_espelho_divergentes` = 0 em 199 colabs (sombra, pos-O69) |
| E4 | o cartao e o TXT no mesmo numero | FEITO | `cartao_x_txt_divergentes` = 0 na competencia 09 |
| E4 | o topo do cartao e a SOMA das linhas | **FALTA** | MEDIDO o contrario: topo x coluna diverge em 7 casos conferidos (col515 11,13 x 92,30 h). Item (6) do [nome], obra aberta |
| E4 | colunas Atraso e Saida antecipada lendo a folha (O51b) | **FALTA** | (sem prova) |
| E4 | o tipo de escala exibido sai da DEFINICAO, nao do rotulo gravado | em curso | O74: `rotulo_do_desenho` + filtro `desenho_do_turno`; ficha no ar, lista de tipos espera o deploy |
| E5 | a 09 lida da celula, sem gravado envelhecendo | **FALTA** | (sem prova) |
| E5 | competencia exportada nao muda o gravado (L-092) | **FALTA** | SEM SELO -- obra O80. MEDIDO o risco: 18 exports em prod, incluindo emp2 08/2026 com 352 linhas entregues |
| E6 | calculador independente do motor, dia a dia | FEITO | `/tmp/e6b.py` roda e publica CSV; metodo VALIDADO em 27/09 (erro real no campo comparado, medido em 1,0 h contra vao de 87 h) |
| E6 | hora de folga trabalhada sem escala certa entra em horas_trabalhadas | FEITO | aplicada em 09: +431,90 h em 11 colabs, HE em zero, 0 saem do TXT; reversao logs/apply_folga_antes.json |
| E6 | dia que a ata nao explica nao e pago em silencio | em curso | medido: 1.484 dia-colab (19,4%), 723 por dia + 761 de 28 colabs de cadastro partido falso |
| E6 | colab com turno aberto em massa (o espelho tambem nao ve) | **FALTA** | medido: col788 63 turnos abertos, col923 46, col880 11 |

**E6 -- calculador independente:** _sem rodada registrada_ (`logs/e6_placar.json` ausente). Sem rodada, a etapa E6 nao tem piso medido.

## painel da sessao (ultimas 40 linhas)

```

  E o ramo certo do elif é sem continue: feriado previsto significa horas simples, então o dia segue para o limite diário e a HE normal como qualquer dia de trabalho. Deixei isso
  escrito, porque a diferença entre os dois ramos é exatamente o que eu confundi.

  Agora emendo o commit e empurro — o push51 falhou por essas duas, e a L-084 errada continua no ar.

  Ran 1 shell command

● push52 em voo, com a suíte inteira sobre a árvore do commit. Vou até pousar.

  Sobre o item (2) do seu prompt — você estava certo e eu já corrigi: o E3 não estava 100%, estava com o meu recorte. Acrescentei os itens de dinheiro abertos que faltavam, com o
  número medido como prova:

  ┌───────────────────────────────┬───────┬──────────┬──────────────────────────────────────────────┐
  │             item              │ etapa │  estado  │                    medido                    │
  ├───────────────────────────────┼───────┼──────────┼──────────────────────────────────────────────┤
  │ plano B (O68b)                │ E3    │ FALTA    │ 1.484 dia-colab (19,4%)                      │
  ├───────────────────────────────┼───────┼──────────┼──────────────────────────────────────────────┤
  │ volta do intervalo (O73)      │ E3    │ FALTA    │ 6 templates, 9 colabs, +44,42 h no col81     │
  ├───────────────────────────────┼───────┼──────────┼──────────────────────────────────────────────┤
  │ o gravado recebeu o corte (b) │ E3    │ em curso │ 4 aplicados, 30 separados                    │
  ├───────────────────────────────┼───────┼──────────┼──────────────────────────────────────────────┤
  │ geradora em toda célula (O70) │ E1    │ FALTA    │ 6.848 dia-colab                              │
  ├───────────────────────────────┼───────┼──────────┼──────────────────────────────────────────────┤
  │ célula com canal (O66)        │ E4    │ FALTA    │ 68 sem canal, 509 chamados fechados acusando │
  └───────────────────────────────┴───────┴──────────┴──────────────────────────────────────────────┘

  O E3 caiu de 100% para 4/7 = 57%, que é a verdade. E a sua frase — "a lista de itens sai do BACKLOG e do RELATO, não do recorte de quem escreve o placar; item de dinheiro aberto
  nunca fica fora" — é a regra que faltava no arquivo, porque sem ela o placar mede a honestidade de quem o preenche em vez do estado do programa.

  Falta ainda, na sua ordem: o DEPLOY JÁ com smoke assim que o push pousar, e rodar o e6b para persistir o percentual do calculador — hoje a linha do E6 diz "sem rodada registrada
  — sem rodada, a etapa E6 não tem piso medido", o que é honesto mas é um buraco no placar.

✻ Churned for 5m 18s · done 1:05 AM · 2 shells still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ roda o e6b e persiste o numero
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · ← for agents · ↓ to manage
  ⧉  index
```
