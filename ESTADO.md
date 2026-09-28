# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:148 AFIRMA ato sem PROVA ao lado -- ## NO AR, e provado em prod
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 23:55:45.**

| | |
|---|---|
| `HEAD` local | `c32534cb [O96] o hook de Stop tinha TRES furos, e um deles o liberava por contador truncado -- e a fila 1 fecha o que fechou` |
| `origin/main` | `3b742106 [IMPAR] linha aberta no TICKETS: citei a fatia em commit e nao abri a linha no mesmo ato` |
| commits a subir (`ahead`) | **1** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
c32534cb [O96] o hook de Stop tinha TRES furos, e um deles o liberava por contador truncado -- e a fila 1 fecha o que fechou
3b742106 [IMPAR] linha aberta no TICKETS: citei a fatia em commit e nao abri a linha no mesmo ato
ce88575c [TETO] a absorcao de intervalo para de engolir batida do dia seguinte -- 1.750 min e 78 h viram 0 e 418
1c031592 [R2b] APLICADA no gravado de 09 com o seu `!`: 9 colabs, folga trabalhada -17,31 h -- dentro da faixa
c372449c [IMPAR][TETO] os dois casos do TETO medidos, com mecanismo NOMEADO -- e sao dois, nao um
```

## `git status --short`

```
M  app/ponto/services/espelho.py
A  app/ponto/tests/test_impar_em_aberto.py
?? cortes.alarme.pausado
?? esteira.pausada
```

## PLACAR ESPELHO-VERDADE

| etapa | feitos | itens do programa |
|---|---:|---|
| **E1** PREVISAO INTEGRA | **2/4 = 50%** | selos de frota = 0: vinculo com fim<inicio; 12x36 com 3+ trabalha seguidos; dia de colab ativo sem previsao ou |
| **E3** MOTOR PELO JUIZ | **7/10 = 70%** | o motor le periodos do juiz da batida e jornada do juiz do previsto; DIFF no RELATO + `!` |
| **E4** LEITORES NO MESMO NUMERO | **2/5 = 40%** | selo tela == PDF == fechamento == TXT na frota, 0 divergencia |
| **E5** FECHAMENTO ONLINE | **1/2 = 50%** | fechamento = LEITURA; `recalcular` deixa de existir; so atos persistem |
| **E6** CERTIFICACAO POR ORACULO INDEPENDENTE | **2/8 = 25%** | 0 divergencia nao explicada + 0 dia sem previsao + 0 periodo fora do juiz |

| etapa | item | estado | prova |
|---|---|---|---|
| E1 | nascer com data_fim < data_inicio e RECUSADO pelo banco (nao so pelo servico | FEITO | corte 27/09 02:0x: CHECK `ec_vigencia_fim_nunca_antes_do_inicio` NOT VALID (escala/0041); selo `escala.tests.test_vigencia_constraint_e_o_juiz` (15 ca |
| E1 | o passivo de vigencia impossivel que ja existe -- lista que so encolhe | em curso | O91 FECHADA por corte Ronald 27/09 10:3x ("nao instala"): o cron `lavrar_vigencia_impossivel` -- o contador desta porta -- segue DECLARADO e NAO insta |
| E1 | celula de trabalho sem previsao valida | em curso | medido: 4 colabs com zero vinculo E zero celula (col924/391/43/942, ~221 h) |
| E1 | qual vinculo vale no dia tem UM juiz (CelulaDia.escala_geradora) | FEITO | O69 aplicada em 09: 654,74 h; `escala/alimentacao.py::vinculo_do_dia`; selo `ponto.tests.test_vinculo_do_dia_pela_celula` (8 casos) |
| E3 | o MARCO manda, nunca o tipo gravado | FEITO | selo `ponto.tests.test_e3_completa_o_marco_manda` + `test_selo_motor_nao_pareia_pelo_tipo_gravado`; aplicada em 09 |
| E3 | jornada do dia vem de minutos_previstos_do_dia, nunca de minutos_jornada | FEITO | HAIKU `jornada_de_fonte_lixo` (lavrar_jornada_lixo, 11 acessos em 4 arquivos) |
| E3 | os leitores de turno tambem perguntam ao juiz da batida (O65) | FEITO | O65 aplicada: dinheiro ZERO, turnos abertos 806 -> 744; selo `SeloLeitorDeTurnoTambemLeAAtaTest` |
| E3 | a jornada pertence ao dia de INICIO, nas tres derivacoes (O76) | FEITO | O76: `MotorBase._dia_do_turno` passa a servir o MotorBase; RED col382 DSR perdido=1 -> ok=1; aplicada 27/09 |
| E3 | o dia que a ata nao explica nao e pago pelo plano B em silencio (O68b) | FEITO | O68b-PAPEL no ar 27/09 13:2x (smoke no worker servido). O motor passou a ser ALIMENTADO com o papel da ata (`turnos_via_autoridade`), e o vao da ata d |
| E3 | colab com turno aberto em massa (o motor nao fecha o par, a tela tambem nao  | em curso | CLASSIFICADO 27/09: dos 782 turnos abertos seguidos de outra ENTRADA, 236 tem as duas entradas a <= 14 h (197 curados pela O68b-PAPEL, 39 plano B), 10 |
| E3 | os 30 separados do corte (b) seguem no motor VELHO no gravado de 09 | em curso | medido: 30 colabs restaurados (logs/apply_modo24h_antes.json); duas causas -- familia 100%/dobra de feriado (17, pergunta de LEI aberta) e DSR/reflexo |
| E3 | a ata do intermitente (coluna posicional, sem marco) tambem da o papel (O84) | FEITO | O84 commitada 27/09 14:5x (`b93b05b0`), suite 8.452 OK. `periodos_do_dia` aprende a forma `·I1..·In` que `escala/utils.py:962` ja escreve COM o papel; |
| E3 | o motor nunca altera os objetos que recebe -- mesma entrada, mesmo resultado | FEITO | O89 no ar 27/09 10:02 com a L-093. `_copia_de_trabalho` em 4 sitios; o pareador carimbava `_intra_dur`/`_eco_flush` na Batida do chamador e o fechamen |
| E3 | no turno partido sem intervalo declarado, a volta da pausa nao e atraso (O73 | **FALTA** | RED col81 (te#189 16:00-00:00): "Atraso: entrada as 18:29 (previsto 16:00)" na volta do intervalo; 6 templates `turno_partido` com intervalo_modo=dura |
| E4 | o cartao PDF desenha o mesmo que a tela | FEITO | `pdf_x_espelho_divergentes` = 0 em 199 colabs (sombra, pos-O69) |
| E4 | o cartao e o TXT no mesmo numero | FEITO | `cartao_x_txt_divergentes` = 0 na competencia 09 |
| E4 | o topo do cartao e a SOMA das linhas | **FALTA** | MEDIDO: topo x coluna diverge (col515 11,13 x 92,30 h). 27/09 02:1x: e o MESMO defeito que a classe C da O83 -- o TOPO concorda com a FOLHA (11,00 h)  |
| E4 | colunas Atraso e Saida antecipada lendo a folha (O51b) | **FALTA** | (sem prova) |
| E4 | o tipo de escala exibido sai da DEFINICAO, nao do rotulo gravado | em curso | O74: `rotulo_do_desenho` + filtro `desenho_do_turno`; ficha no ar, lista de tipos espera o deploy |
| E5 | a 09 lida da celula, sem gravado envelhecendo | **FALTA** | (sem prova) |
| E5 | competencia exportada nao muda o gravado (L-092) | FEITO | O80 no ar 27/09 10:1x: `ponto/services/fechamento.py::CompetenciaExportada` + `empresas_exportadas_no_escopo` (le `marcos_da_competencia`), conferido  |
| E6 | calculador independente do motor, dia a dia | FEITO | `/tmp/e6b.py` roda e publica CSV; metodo VALIDADO em 27/09 (erro real no campo comparado, medido em 1,0 h contra vao de 87 h) |
| E6 | com batidas COMPLETAS, espelho e oraculo batem (divergencia aqui e bug nosso | em curso | MEDIDO 27/09 19:3x na FROTA, depois do apply: **91,4%** (6.877 de 7.521 dia-colab). Divergem 644 em 153 colabs -- 247 entre 10 e 60 min, 272 acima de  |
| E6 | dia de batidas IMPARES aparece EM ABERTO com o que falta, nunca com numero | **FALTA** | MEDIDO 27/09 19:3x: **30,9%** (146 de 473 dia-colab). **327 dias em 167 colabs mostram um NUMERO** onde devia estar "em aberto". Caso calibrado a mao: |
| E6 | a 09 so exporta colab certificado pelo oraculo | em curso | MEDIDO 27/09 19:3x: **189 de 203 colabs certificados**. 14 divergem em algum dia de batidas completas e saem para a lista de ajuste ate a causa ter no |
| E6 | o oraculo compara contra espelho INTEGRO, nao contra o builder | **FALTA** | MEDIDO: 235 dos colabs caem no builder (espelho.py:352-361). Causa = O81 (3.986 dias de ata agregada em 06/07/08 arrastando 09 via espelho.py:585) |
| E6 | dia de batida impar nao fica fora da certificacao em silencio | **FALTA** | MEDIDO: 473 dias saem da comparacao por batida impar -- o oraculo nao os julga e ninguem mais responde por eles |
| E6 | o piso de horas que o oraculo acusa tem causa por colab | em curso | CLASSIFICADO em 27/09 02:1x (O83): 84 colabs / 1.602,2 h em 4 classes (B 27/733,7 · C 32/376,4 · D 21/274,3 · A 4/217,7); 9 no TXT = 259,3 h. RODADA 2 |
| E6 | hora de folga trabalhada sem escala certa entra em horas_trabalhadas | FEITO | aplicada em 09: +431,90 h em 11 colabs, HE em zero, 0 saem do TXT; reversao logs/apply_folga_antes.json |

**E6 -- calculador independente (`/tmp/e6b.py`), ultima rodada 27/09/2026 ~01:10:** **91.5% dos dias BATEM** (ate 10 min) em 7536 dias comparados; 160 colabs com divergencia.

## painel da sessao (ultimas 40 linhas)

```
  Auditoria da árvore viva (sua ordem) — 7 crons */5 rodaram na janela 21:15:04–21:15:18, dentro dela. Zero escrita estrutural: célula 0, chamado 0, pergunta 0, fechamento 0,
  batida retratada 0. Saíram só 3 push (envio, não estado — e os 3 colabs seguem com turno aberto pelo juiz revertido) e o snapshot do placar, já sobrescrito às 21:25. Nada a
  reverter. Os escritores de dinheiro rodam 06:26–07:37, a 9 h da janela.

  R2b — curada pela sua lei (ata com hii E hfi acesos = o par É a pausa, teto não se aplica), selada, régua verde, no ar e aplicada no gravado com o seu !: 9 colabs,
  horas_folga_trabalhada −17,31 h em 3 (dentro dos −25,85 em 5 autorizados; a casa separou 4 pela família 100% e 1 pela CONDICAO_SAIDA_REAL). ENTRAM/SAEM do TXT = 0, 07 e 08 = 0,
  família 100% e banco intocados. Smoke em prod: col843 24/08 = 544 trabalhou, 25/08 = folga sem hora. Condição (d): 0 divergência nova no oráculo e 31 curadas (85,0% → 90,0%).

  TETO — os seus dois casos eram um mecanismo, não dois (corrigi o que publiquei): _dist_marco mede hora do dia e engolia batida de D+1 como pausa de um turno de D, silenciando o
  próprio teto de 26 h. col444 30/08 1.750 → 0 (turno segue aberto) e 31/08 0 → 287; col349 14/09 4.682 → 418. No ar, com smoke em prod.

  REGUA-MORTA, sua pergunta — nenhum push pulou o arsenal entre 25/09 10:34 e 27/09 22:24. O atalho exige REGUA_STATUS == "OK", o único escritor do carimbo é bin/regua.sh (que
  morria antes de escrever), e o carimbo ficou em FALHOU / 25/09 07:25:56. Os 199 commits da janela rodaram o arsenal inteiro. O custo foi tempo e a cegueira dos selos de host,
  não código sem teste.

  Suite pela régua — régua 8.497 testes em 467 s; o docker run manual sem --parallel levava 898 s. Pico de RAM em --parallel 2: 1.024 MiB de 2.048 (50%), CPU ~145% de 400%.
  --parallel 4 cabe (~1,7 GiB, 83%), folgado com --memory 3g; mas o cpuset de teste são 4 threads em 2 núcleos físicos, então espero 25–40%, não o dobro — rodo uma régua assim e
  publico os dois tempos se você quiser.

  Achados de carona ao cumprir a ordem da régua: ela estava morta desde 25/09 10:34, e com ela viva 4 selos de host mudos há 36 h acusaram — alarme de cortes virado juiz,
  fabricante sorteando item que espera você (O59/O66/O81, e O81 declarado duas vezes), hook virado cópia de 177 linhas num ciclo com o próprio hooks.sh, e o selo de LABELS vazio
  (comparava a fonte única consigo mesma).

  Próximo: IMPAR — espelho.py:513-514 passa a ler RealizadoDoDia.aberto, classes B e C primeiro.

✻ Brewed for 2h 38m 35s · done 11:41 PM · 1 shell still running

❯ segue a fila: IMPAR. E o hook liberou o fim do turno as 23:41 com fila em pe: se foi PAREI velho no topo do RELATO, limpar o PAREI resolvido ao fechar cada item. Segue.

  Regua da IMPAR fatia 1 · 1m 23s
  ⎿  $ cd /home/ronald/saas-hasner && git add app/ponto/services/espelho.py app/ponto/tests/test_impar_em_aberto.py && bash bin/regua.sh 2>&1 | tail -6 (1m 22s)
     (ctrl+b ctrl+b (twice) to run in background)

· Zigzagging… (12m 48s · ↓ 21.5k tokens)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
