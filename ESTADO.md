# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:435 AFIRMA ato sem PROVA ao lado -- ### APLICADO 01/10 19:44, e a PROVA
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 01/10/2026 20:25:21.**

| | |
|---|---|
| `HEAD` local | `61720fac S5b, os dois passos medidos: a guarda e CEGA no calculador por falta de dado, e a dobra MORREU` |
| `origin/main` | `c07a13c5 NO AR as 19:26 com smoke em prod -- e a ressalva: quem prova a guarda e o col890, nao os outros dois` |
| commits a subir (`ahead`) | **10** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **78** (baseline divergiu 40, nunca lancada 33, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
61720fac S5b, os dois passos medidos: a guarda e CEGA no calculador por falta de dado, e a dobra MORREU
edea15bc CORRECAO minha, e a tabela que decide: a troca ainda cria +129,70 h de atraso em 283 colabs
48a202f0 A tabela honesta da S5b: 3 rubricas 100% CEGAS, trabalhadas divergindo em 445 de 464, e o `!` NAO se pede
d1ddc677 O DIFF da S5b parou de somar a propria cegueira: noturnas sai de -5.786,47 h para 30 colabs CEGOS
02da1822 col369 DECIDIDO por ele: NAO trocar para o 1296 -- a pausa real e 12:09-14:04, nao 11:00-12:00
```

## `git status --short`

```
 M app/docs/BACKLOG.md
 M app/docs/HANDOFF-SESSAO.md
 M app/docs/PENDENTES_RONALD.json
 M bin/handoff_sessao.sh
 M bin/relato.sh
?? app/docs/AVAIS.md
?? bin/gerar_avais.py
?? bin/keepalive.sh
?? bin/tests/test_avais_na_mesa.sh
?? cortes.alarme.pausado
?? esteira.pausada
?? fatias_agendadas/abono-no-ar/esteira.out
?? fatias_agendadas/abono-no-ar/esteira_agendada.out
?? fatias_agendadas/abono-no-ar/fatia.done
?? fatias_agendadas/abono-no-ar/msg_commit.txt
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
  linha no AVAIS com a frase pronta e voce PASSA AO PROXIMO no mesmo turno. So devolve turno com a lista inteira travada. Um push por marco; vizinhos do
  sitio tocado antes da suite cheia.
  1. S5b: (a) horas_atraso +129,70 h em 22 -- medir quantos sao turno ABERTO ou par nulo; hipotese: o calculador nao le a guarda que entrou no motor as
  19:26 (_aplicar_teto_pontualidade); se for, IMPORTAR a guarda. (b) trabalhadas +107,23 h em 27 e folga +31,48 h em 3: abrir por causa, 3 casos a mao,
  dizer quem esta certo e se a contagem em dobro morreu. Tabela final -> AVAIS, espera o ! da troca.
  2. AVAIS-NA-MESA (ordem de 18:4x): gerar e publicar AVAIS.md.
  3. GESTAO-HE-CALENDARIO-CONTROLE, parte da main: view do laco (N atos por decidir_he), cadastro he_limite_decisao_min com migration, leitor mandando
  abaixo_limite, url no contexto; merge da raia + deploy no mesmo ato -> smoke meu no AVAIS.
  4. UI-RESPOSTA-DIZ-O-QUE-E (ordens de 17:4x e 18:3x): sinal_min no juiz, as 4 frases, Reabrir com aviso, "30 min" lido da fonte.
  5. SUITE-UMA-VEZ-POR-ARVORE (ordem de 20:0x).
  6. So LEITURA, publicar e nao aplicar: censo FOLHA-ZERO-COM-BATIDA na frota; DIFF emp3=CLT da 09 e da 10 por rubrica e por colab.
  7. Pautas, sem escrita de dinheiro: col369 (folga nao declarada no 1313), col650 (data de fim da ausencia 3186), col935 (admissao 05 ou 07/09), lista
  dos 51 da fase 12x36 para a supervisao.
  8. Fila 1 do BACKLOG na ordem do hook (O21 em diante).
     Segue a fila; PAREI so com a lista inteira travada !

  Ran 8 shell commands

● Monitor event: "DIFF de folha do vinculo 1296 na sombra (2a tentativa)"

● Fechar a celula e rodar os vizinhos
  ⎿  $ cd /home/ronald/saas-hasner && python3 - <<'PYEOF'
     import io, sys
     sys.path.insert(0,'bin'); import hook_stop_fila1 as H
     P='app/docs/BACKLOG.md'
     t=io.open(P,encoding='utf-8').read()
     A='| AVAIS-NA-MESA |'
     i=t.find(A); fim=t.index('\n', i); c=t[i:fim].split('|')
     novo = (' **FECHADA 01/10 21:3x.** `b…

* Discombobulating… (10m 12s · ↓ 34.8k tokens)
                                                                                                                  ✔ Update installed · Restart to update
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell, 3 monitors · esc to interrupt · ← for agents · ↓ to manage

  ● main
  ◯ general-purpose  Inspecting RELATO.md header lines                                                                         18m 34s · ↓ 235.2k tokens
  ⧉  index
```
