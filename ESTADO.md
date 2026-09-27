# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:160 AFIRMA ato sem PROVA ao lado -- MODO 24H (aval Ronald 27/09 03:4x). **APLICADOS: 4 colabs**, movimento so nos campos do item
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 01:17:01.**

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
  seria erro meu.

  (c) O cartório LAVROU — todas as 3.986 estão julgadas, com veredito (3.543 concorde, 261 fato_sem_previsao, 142 nunca_bateu). Então a hipótese "o cartório não lavrou" está
  descartada: ele lavrou o veredito e deixou a ata na forma agregada antiga, sem lâmpadas. É formato legado, não omissão — e a distribuição confirma: julho 1.396, agosto 2.582,
  setembro zero, o que datam a mudança de formato.

  E só 63 dos 3.986 têm trabalha=True — a esmagadora maioria é dia de folga, o que reduz muito o impacto em dinheiro, embora não o impacto na leitura do espelho.

  Corrijo a O81 no BACKLOG com esses números: a cura não é "o cartório lavrar" (ele lavrou), é rejulgar com --forcar para reescrever a ata no formato novo, e o contador do
  builder.

✻ Churned for 1m 29s · done 1:14 AM · 1 shell still running

● Background command "Emendar e empurrar o push52" completed (exit code 0)

  Ran 1 shell command

● push52 POUSOU: origin/main = c6c3785b, ahead = 0, 8.388 testes OK + control-plane 22 OK. Agora o deploy, que é o pendente urgente:

  Ran 1 shell command

❯ HANDOFF ANTES DO AUTO-COMPACT (2%): grava no topo do RELATO e no TICKETS: push52 (se pousou, o hash; se nao, a causa), DEPLOY JA pendente (a L-084 errada no ar desde 23:35),
  e6b em curso (publicar % de dias que batem, dias comparados, data e quantos colabs caem no builder), placar E3 4/7. FILA MODO 24H depois do deploy: os 30 separados (remedir
  apos O76 e L-086 corrigida) -> O68b plano B -> O70 geradora -> O66 -> O73 -> O51b. Regras que continuam valendo: aplica so o que esta no criterio, separa o resto numa lista
  unica no topo do RELATO; para so em pergunta de LEI ou item da L-009. Nenhuma lei nova.

● Já recebido 27/09 ~07:xx e gravado — mas três fatos mudaram desde então, então atualizo em vez de só apontar:

  Ran 1 shell command

● Background command "DEPLOY JA das sete curas" completed (exit code 0)

✢ Compacting conversation… (1m 43s · ↓ 2.3k tokens)
  ▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▱▱▱▱▱▱▱▱▱▱▱▱▱▱▱▱▱▱▱▱ 50%
                                                                                                                                                            0% until auto-compact
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
