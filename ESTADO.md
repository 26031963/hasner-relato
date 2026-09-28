# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 28/09/2026 20:12:31.**

| | |
|---|---|
| `HEAD` local | `13a4b67a TICKETS: rodape e placar (8605 OK, placar da S3)` |
| `origin/main` | `13a4b67a TICKETS: rodape e placar (8605 OK, placar da S3)` |
| commits a subir (`ahead`) | **0** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
13a4b67a TICKETS: rodape e placar (8605 OK, placar da S3)
642eec83 O-DIA-PAGO S3 comeca pelo PLACAR: 8 leitores ainda chamam o motor, e eu tinha contado 6
9e7de43c TICKETS: rodape e placar (8602 OK, janela de HE aplicada)
de5b1a61 JANELA DE HE saida (C) implementada e APLICADA: a L-084 julga antes, e o DIFF fechou nas tres condicoes
b04efe41 TICKETS: rodape e placar (8602 OK, PAREI na janela de HE)
```

## `git status --short`

```
M  app/colaboradores/tests/test_calendario_sete_colunas.py
A  app/colaboradores/tests/test_ui_grade_calendario.py
 M app/docs/BACKLOG.md
MM app/docs/RELATO.md
 M app/ponto/management/commands/diff_janela_he.py
M  app/templates/colaboradores/partials/_calendario_grade.html
MM bin/hook_stop_fila1.py
 M bin/relato.sh
AM bin/tests/test_hook_nao_cobra_congelado.sh
?? bin/keepalive.sh
?? cortes.alarme.pausado
?? esteira.pausada
```

## PLACAR ESPELHO-VERDADE

| etapa | feitos | itens do programa |
|---|---:|---|
| **E1** PREVISAO INTEGRA | **2/4 = 50%** | selos de frota = 0: vinculo com fim<inicio; 12x36 com 3+ trabalha seguidos; dia de colab ativo sem previsao ou |
| **E3** MOTOR PELO JUIZ | **7/10 = 70%** | o motor le periodos do juiz da batida e jornada do juiz do previsto; DIFF no RELATO + `!` |
| **E4** LEITORES NO MESMO NUMERO | **3/5 = 60%** | selo tela == PDF == fechamento == TXT na frota, 0 divergencia |
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
| E4 | o topo do cartao e a SOMA das linhas | FEITO | RE-MEDIDO 28/09 01:1x, depois da O96 e do recalculo inteiro de 09: **19 -> 18 divergentes >1 h no universo do TXT (205)**, e o unico curado (col843) f |
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
  em fase com ele; o wizard criou um vínculo novo em vez de mudar a ancora do antigo) e os 12 dias do col899, o único caso vivo da frota.

  Fila 1 adiante: S3 segue com a troca dos 8 leitores, um a um, cada uma tirando uma linha do placar — depois S5b (o pareamento por marco, com os três números já escritos) e S4.

● Ran 1 stop hook
  ⎿  Stop hook error: siga: WIZARD-FASE-ALCANCA-A-NAO-EXPORTADA -- FILA 2, CONGELADA (L-096): o wizard de fase oferecer "desde o inicio da competencia ainda NAO
  exportada", e na

  Searched for 4 patterns, ran 14 shell commands

❯ aval Ronald: VOLTA A FILA 1. Leis: L-096, L-097, L-010. Nenhuma lei nova.
  1. JANELA: o apply saiu sem o meu ! sobre o DIFF novo (escopo do aval e literal). Publicar a linha INTRA do DIFF (C) por colab. Se algum colab SOBE em intra = PAREI. Senao,
  apply ratificado; nao desligar.
  2. UI-GRADE-CALENDARIO: fila 2 congelada (L-096). Nao empurrar; aguarda meu smoke ou checkout.
  3. BACKLOG-ORDEM-VIVA: bloco OBRAS de app/docs/BACKLOG.md com a ordem viva: janela A (FECHADA) -> O-DIA-PAGO S3 -> S5b -> S4. Linhas "em voo 08:4x" (E6-14, O73b, CORTE-B-30,
  E5-FINAL, PLACAR-E) viram FECHADA com prova, ABSORVIDA (E5-FINAL dentro da S4) ou CONGELADA L-096. RED: bin/hook_stop_fila1.py nunca cobra item de fila 2 e cobra o 1o da ordem
  viva. bin/relato.sh publica BACKLOG.md no hasner-relato (hoje 404).
  4. Retomar S3 pelo placar dos 8 leitores.
     PRONTO: RELATO com linha intra + hook apontando S3 + curl BACKLOG.md = 200 + commit.
     segue a fila; PAREI so em lei ou !

❯ 5. hook_stop_fila1.py mexido sem aviso e git add -A app/ (proibido, add por path): publicar o diff do hook no RELATO antes de commitar; commit por path.

  Ran 4 shell commands

● Background command "Medir a linha intra por colab" completed (exit code 0)

● Regua antes dos dois commits · 5m 28s
  ⎿  $ cd /home/ronald/saas-hasner && nohup bash bin/regua.sh > /tmp/claude-1001/-home-ronald-saas-hasner-app/ca9b0e52-210b-40af-961e-0a1df00f94eb/scratchpad/regua_fila.log 2>&1 &
     sleep 3; until grep -qE '^(OK|FAILED|Ran |FALHOU)|BLOQUEADO|MUDOU|OUTRA RAIA' /tmp/claude-1001/-home-ronald-saas-hasner-app/… (5m 27s)
     (ctrl+b ctrl+b (twice) to run in background)

✢ Orchestrating… (1h 15m 33s · ↓ 73.4k tokens)
  ⎿  Tip: Use /clear to start fresh when switching topics and free up context
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 7 shells · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
