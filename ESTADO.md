# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 01:22:31.**

| | |
|---|---|
| `HEAD` local | `c6c3785b [E6-PISO+O76+L-084/L-086] a hora de folga que sumia volta (+431,90 h em 11), e o dia do turno passa a sair do juiz` |
| `origin/main` | `c6c3785b [E6-PISO+O76+L-084/L-086] a hora de folga que sumia volta (+431,90 h em 11), e o dia do turno passa a sair do juiz` |
| commits a subir (`ahead`) | **0** |

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
 M app/docs/BACKLOG.md
 M app/docs/LEIS.md
 M app/docs/PROMPTS.md
 M app/docs/RELATO.md
 M app/docs/TICKETS.md
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

**E6 -- calculador independente (`/tmp/e6b.py`), ultima rodada 27/09/2026 07:xx:** **91.5% dos dias BATEM** (ate 10 min) em 7536 dias comparados; 160 colabs com divergencia.

## painel da sessao (ultimas 40 linhas)

```
● Confirmado no vivo: ponto/services/espelho.py:352-361 — o try envolve grade_da_celula(colaborador, **data_ini, data_fim_mes**) do período INTEIRO, então um dia de ata agregada
  derruba o espelho do mês todo para o builder. E isso desmente o meu próprio recorte: a competência 09 começa em 21/08, então dias de agosto estão DENTRO dela. Vou medir quantos
  dos 3.986 caem na janela de 09:

  Ran 1 shell command

❯ Depois do compact: rele o topo do RELATO e o TICKETS. Primeiro o SMOKE do deploy das sete curas, provado no worker servido (hora dos workers x hora dos arquivos): atraso de
  4h10 com saida no horario voltando a descontar (L-084 corrigida) e col382 sem falta no 20/09 (O76). Depois segue a FILA do MODO 24H do handoff. Nenhuma lei nova.

❯ Depois do compact: (1) SMOKE do deploy das sete curas no worker servido (atraso de 4h10 com saida no horario descontando; col382 sem falta no 20/09). (2) PLACAR: o plano B
  (O68b, 1.484 dias) e os turnos abertos em massa (col788, col923, col880) sao do MOTOR -> etapa E3, nao E6; os 30 separados do corte (b) e a O73 tambem sao E3. A E3 publicada
  tem que sair com esses itens e o % verdadeiro. A data da rodada da E6 e ~01:10 de 27/09, nao 07:xx. (3) RELATO.md:160 "APLICADOS: 4 colabs" ganha a linha PROVA e o RELATO volta
  a publicar. Depois segue a fila do MODO 24H. Nenhuma lei nova.

● Dois pedidos registrados. Antes de tudo, o fato que muda a ordem: o deploy das sete curas FALHOU — não por bug, por lei: o carimbo da sombra era de 26/09 e agora é 27/09 01:19,
  então bin/deploy.sh recusou por falta de ensaio de hoje. Já lancei --refazer && --bloco (não usei --sem-sombra: atalho é L-009). Enquanto roda, meço o que o worker realmente
  serve:

  Ran 1 shell command

● Evidência fechada, e ela confirma a urgência: o container roda em UTC — os workers subiram 02:35 UTC = 23:35 -03 de 26/09, e os arquivos das curas têm mtime 00:48 e 01:04 de
  27/09, depois disso. As sete curas não estão sendo servidas; o smoke no worker servido só pode ser feito depois do deploy, que espera o ensaio. Enquanto a sombra roda, faço os
  itens (2) e (3):

  Searched for 2 patterns, ran 3 shell commands

● O elo que faltava está no vivo: ponto/services/espelho.py:585 — vis_ini = min(piso_visual(colaborador, hoje), apur_ini) e a grade vai de vis_ini até hoje. Então os dias de ata
  agregada de 07/08, embora fora da competência 09, estão dentro do intervalo do espelho: um único dia deles derruba o mês todo para o builder. Gravo a O81 medida e corrijo os
  horários que eu escrevi 6 h adiantados:

● Ver o json do e6 e como o placar e renderizado
  ⎿  $ cd /home/ronald/saas-hasner && cat logs/e6_placar.json; echo; sed -n 40,80p bin/gerar_estado.py

· Unfurling… (7m 13s · ↓ 16.0k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
