# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:455 AFIRMA ato sem PROVA ao lado -- ## E1 NO AR e provada em prod · O85 com RED evidenciado · push `c35dbe8c`
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 07:38:31.**

| | |
|---|---|
| `HEAD` local | `4d8b3ad4 PAREI: a fila do MODO 24H so tem item com trava nomeada -- espera o Ronald` |
| `origin/main` | `4d8b3ad4 PAREI: a fila do MODO 24H so tem item com trava nomeada -- espera o Ronald` |
| commits a subir (`ahead`) | **0** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
4d8b3ad4 PAREI: a fila do MODO 24H so tem item com trava nomeada -- espera o Ronald
0161a378 [O86] corrijo minha propria frase de dez minutos: nao e "uma causa so", sao seis formas e cinco ja tem obra
6adad96d [O86] classe C fechada em taxonomia, e a O86 vira a maior obra aberta (~900 h)
6a4ed51b RELATO: resumo da noite fechado com a O66, a O91 e a licao do heredoc
d070cf9d [TICKETS] placar e rodape com o push 512030da -- quarta vez que a O90 barra um push hoje
```

## `git status --short`

```
 M app/docs/RELATO.md
?? esteira.pausada
```

## PLACAR ESPELHO-VERDADE

| etapa | feitos | itens do programa |
|---|---:|---|
| **E1** PREVISAO INTEGRA | **2/4 = 50%** | selos de frota = 0: vinculo com fim<inicio; 12x36 com 3+ trabalha seguidos; dia de colab ativo sem previsao ou |
| **E3** MOTOR PELO JUIZ | **4/8 = 50%** | o motor le periodos do juiz da batida e jornada do juiz do previsto; DIFF no RELATO + `!` |
| **E4** LEITORES NO MESMO NUMERO | **2/5 = 40%** | selo tela == PDF == fechamento == TXT na frota, 0 divergencia |
| **E5** FECHAMENTO ONLINE | **0/2 = 0%** | fechamento = LEITURA; `recalcular` deixa de existir; so atos persistem |
| **E6** CERTIFICACAO POR ORACULO INDEPENDENTE | **2/6 = 33%** | 0 divergencia nao explicada + 0 dia sem previsao + 0 periodo fora do juiz |

| etapa | item | estado | prova |
|---|---|---|---|
| E1 | nascer com data_fim < data_inicio e RECUSADO pelo banco (nao so pelo servico | FEITO | corte 27/09 02:0x: CHECK `ec_vigencia_fim_nunca_antes_do_inicio` NOT VALID (escala/0041); selo `escala.tests.test_vigencia_constraint_e_o_juiz` (15 ca |
| E1 | o passivo de vigencia impossivel que ja existe -- lista que so encolhe | em curso | MEDIDO 27/09 02:2x: 53 vinculos; 43 CONGELADOS por cruzarem competencia exportada (L-092) e 10 livres para o propositor com `!` caso a caso. logs/e1_v |
| E1 | celula de trabalho sem previsao valida | em curso | medido: 4 colabs com zero vinculo E zero celula (col924/391/43/942, ~221 h) |
| E1 | qual vinculo vale no dia tem UM juiz (CelulaDia.escala_geradora) | FEITO | O69 aplicada em 09: 654,74 h; `escala/alimentacao.py::vinculo_do_dia`; selo `ponto.tests.test_vinculo_do_dia_pela_celula` (8 casos) |
| E3 | o MARCO manda, nunca o tipo gravado | FEITO | selo `ponto.tests.test_e3_completa_o_marco_manda` + `test_selo_motor_nao_pareia_pelo_tipo_gravado`; aplicada em 09 |
| E3 | jornada do dia vem de minutos_previstos_do_dia, nunca de minutos_jornada | FEITO | HAIKU `jornada_de_fonte_lixo` (lavrar_jornada_lixo, 11 acessos em 4 arquivos) |
| E3 | os leitores de turno tambem perguntam ao juiz da batida (O65) | FEITO | O65 aplicada: dinheiro ZERO, turnos abertos 806 -> 744; selo `SeloLeitorDeTurnoTambemLeAAtaTest` |
| E3 | a jornada pertence ao dia de INICIO, nas tres derivacoes (O76) | FEITO | O76: `MotorBase._dia_do_turno` passa a servir o MotorBase; RED col382 DSR perdido=1 -> ok=1; aplicada 27/09 |
| E3 | o dia que a ata nao explica nao e pago pelo plano B em silencio (O68b) | em curso | medido na frota de 09: 1.484 dia-colab (19,4% do que o motor julga), 723 por dia sem ata + 761 dos 28 colabs de cadastro partido falso |
| E3 | colab com turno aberto em massa (o motor nao fecha o par, a tela tambem nao  | **FALTA** | medido: col788 63 turnos abertos, col923 46, col880 11 |
| E3 | os 30 separados do corte (b) seguem no motor VELHO no gravado de 09 | em curso | medido: 30 colabs restaurados (logs/apply_modo24h_antes.json); duas causas -- familia 100%/dobra de feriado (17, pergunta de LEI aberta) e DSR/reflexo |
| E3 | no turno partido sem intervalo declarado, a volta da pausa nao e atraso (O73 | **FALTA** | RED col81 (te#189 16:00-00:00): "Atraso: entrada as 18:29 (previsto 16:00)" na volta do intervalo; 6 templates `turno_partido` com intervalo_modo=dura |
| E4 | o cartao PDF desenha o mesmo que a tela | FEITO | `pdf_x_espelho_divergentes` = 0 em 199 colabs (sombra, pos-O69) |
| E4 | o cartao e o TXT no mesmo numero | FEITO | `cartao_x_txt_divergentes` = 0 na competencia 09 |
| E4 | o topo do cartao e a SOMA das linhas | **FALTA** | MEDIDO: topo x coluna diverge (col515 11,13 x 92,30 h). 27/09 02:1x: e o MESMO defeito que a classe C da O83 -- o TOPO concorda com a FOLHA (11,00 h)  |
| E4 | colunas Atraso e Saida antecipada lendo a folha (O51b) | **FALTA** | (sem prova) |
| E4 | o tipo de escala exibido sai da DEFINICAO, nao do rotulo gravado | em curso | O74: `rotulo_do_desenho` + filtro `desenho_do_turno`; ficha no ar, lista de tipos espera o deploy |
| E5 | a 09 lida da celula, sem gravado envelhecendo | **FALTA** | (sem prova) |
| E5 | competencia exportada nao muda o gravado (L-092) | **FALTA** | SEM SELO -- obra O80. MEDIDO o risco: 18 exports em prod, incluindo emp2 08/2026 com 352 linhas entregues |
| E6 | calculador independente do motor, dia a dia | FEITO | `/tmp/e6b.py` roda e publica CSV; metodo VALIDADO em 27/09 (erro real no campo comparado, medido em 1,0 h contra vao de 87 h) |
| E6 | ZERO divergencia nao explicada entre oraculo e espelho | **FALTA** | MEDIDO o contrario em 27/09 ~01:10: 644 de 7.536 dias (8,5%) fora de 10 min -- 257 acima de 60 min, 233 entre 10 e 60, 77 com espelho zero e trabalho  |
| E6 | o oraculo compara contra espelho INTEGRO, nao contra o builder | **FALTA** | MEDIDO: 235 dos colabs caem no builder (espelho.py:352-361). Causa = O81 (3.986 dias de ata agregada em 06/07/08 arrastando 09 via espelho.py:585) |
| E6 | dia de batida impar nao fica fora da certificacao em silencio | **FALTA** | MEDIDO: 473 dias saem da comparacao por batida impar -- o oraculo nao os julga e ninguem mais responde por eles |
| E6 | o piso de horas que o oraculo acusa tem causa por colab | em curso | CLASSIFICADO em 27/09 02:1x (O83), com a regua trabalhadas+folga: 84 colabs / 1.602,2 h em 4 classes (B 27/733,7 · C 32/376,4 · D 21/274,3 · A 4/217,7 |
| E6 | hora de folga trabalhada sem escala certa entra em horas_trabalhadas | FEITO | aplicada em 09: +431,90 h em 11 colabs, HE em zero, 0 saem do TXT; reversao logs/apply_folga_antes.json |

**E6 -- calculador independente (`/tmp/e6b.py`), ultima rodada 27/09/2026 ~01:10:** **91.5% dos dias BATEM** (ate 10 min) em 7536 dias comparados; 160 colabs com divergencia.

## painel da sessao (ultimas 40 linhas)

```
● Background command "Publicar a correcao e empurrar" completed (exit code 0)

● Background command "Esperar o push da correcao" completed (exit code 0)

  Ran 1 shell command

● Tudo pousado (0161a378). A fila tratável está fechada e o que resta tem trava nomeada — escrevo a linha que a regra exige e publico:

  Pushed to main

● A madrugada fechou. origin/main = 4d8b3ad4, ahead 0, RELATO publicado.

  Seis itens da sua fila saíram, cada um com smoke próprio em prod — não só suíte verde: as sete curas (01:53), E1 com migration (02:30), O85 (03:31), O73 (04:01), O87 (04:28),
  O66 (05:08). Cinco deploys, todos provados na casca servida.

  Dinheiro: +157,72 h a 5 colabs, em dois atos, com os outros 25 campos em zero e 0 entram/saem do TXT nos dois. A lista do DP caiu para 8 colabs / 248,2 h.

  Onze obras nasceram medidas (O82–O91). As duas descobertas que valem mais que as horas:

  - O89 — o motor mutaciona os objetos Batida que recebe. Provado em três rodadas com entrada idêntica, e o laço de fatias faz isso em 76 de 607 colabs. Foi o smoke comparando
    folha × espelho que me levou até lá, por 4 h de diferença num colab.
  - Classe C fechada em taxonomia: 5 colabs eram buraco de vínculo (curado), 23 são "o motor não fecha o par" com motor == folha ao centavo, e 4 são gravado velho. Isto é: a folha
    não é o problema — o que falta está a montante dela.

  Sete correções de números meus, das quais duas derrubaram coisa que eu já havia publicado — inclusive a de dez minutos antes do fim, quando eu escrevi "~900 h numa causa só" e a
  medição seguinte mostrou seis formas, cinco delas já com obra. O total não mudou; mudou o que fazer com ele.

  A parada que importa: aplicar a L-084 descontaria 286,12 h de 24 pessoas, e o col820 deveria 12h21 num dia em que trabalhou 2h19. Está no topo do PENDENTES_RONALD.json com a
  tabela — uma frase sua resolve a forma do corte.

  O que resta na fila tem trava nomeada, item por item, no topo do RELATO: ! de lei (L-084, feriado), ! de desenho (O89), ! de infra (O91 — install ativaria um escritor), L-009
  para os passivos congelados, e o seu smoke de clique para a O51b, que é tela e PDF e eu não deixei meio pronta.

✻ Churned for 35m 54s · done 6:05 AM · 1 shell still running
                                                                                                                                           new task? /clear to save 729.7k tokens
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ corte Ronald: a saida antecipada nunca passa de previsto menos trabalhado. !
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · ← for agents · ↓ to manage
  ⧉  index
```
