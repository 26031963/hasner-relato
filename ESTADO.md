# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 20:06:01.**

| | |
|---|---|
| `HEAD` local | `f029d738 [O96] reapply aplicado, mutirao ENVIADO, e a R1 com origem nomeada depois de DUAS curas rejeitadas` |
| `origin/main` | `f029d738 [O96] reapply aplicado, mutirao ENVIADO, e a R1 com origem nomeada depois de DUAS curas rejeitadas` |
| commits a subir (`ahead`) | **0** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
f029d738 [O96] reapply aplicado, mutirao ENVIADO, e a R1 com origem nomeada depois de DUAS curas rejeitadas
0a22e208 [O96] o RELATO sai do retido, e entram as regressoes com numero + o censo O81 a/b/c
6d3e54b0 [O96] a pausa que a ATA marcou com `X` voltou a ser PAUSA -- regressao minha, de hoje, no gravado
eb87a7c2 [O95] apply da 09 FEITO, E6 rodada 3 na frota, e o placar da E6 redefinido pela regua da supervisora
5c8d4240 [O95] rodape e placar do TICKETS antes do push
```

## `git status --short`

```
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
  (1) Os baldes da O96 — "OUTRO = 0", então não sobra cura.

  ┌─────────────────────────────────────────────────────────────────┬────────┬─────────────────────────────────────┐
  │                              balde                              │ colabs │              pendente               │
  ├─────────────────────────────────────────────────────────────────┼────────┼─────────────────────────────────────┤
  │ gravado já igual à leitura                                      │    575 │                             +0,00 h │
  ├─────────────────────────────────────────────────────────────────┼────────┼─────────────────────────────────────┤
  │ separado do apply ou fora (col81 + os 3 da CONDICAO_SAIDA_REAL) │     29 │ +136,70 h congeladas por declaração │
  ├─────────────────────────────────────────────────────────────────┼────────┼─────────────────────────────────────┤
  │ fora do TXT                                                     │      0 │                             +0,00 h │
  ├─────────────────────────────────────────────────────────────────┼────────┼─────────────────────────────────────┤
  │ OUTRO                                                           │      0 │                             +0,00 h │
  └─────────────────────────────────────────────────────────────────┴────────┴─────────────────────────────────────┘

  A diferença entre os dois números tem duas partes: das 116,1 h do censo, só 50,9 h (11 colabs) foram criadas pela O68b-PAPEL — as outras 65,2 h são o problema antigo (par com
  gravado S,E falhando o carimbo mais acima), que a O96 não cura e segue em pé com nome. E dessas 50,9 h, a parte que estava em colab do TXT e não separado é exatamente os 7
  colabs / −18,76 h do reapply. Nenhum colab do TXT tem diferença pendente entre leitura e gravado.

  (2) R1: duas curas, as duas rejeitadas pela medição — e é isso que nomeia a origem. A primeira (o vão tem de dar conta do dia) não moveu o col920 e quebrou o col922 (404 →
  1.015). A segunda, a mais restritiva que existe (a ata corrige só o que o gravado contradiz — sequência que alterna perfeitamente não se corrige), também não moveu o col920 e
  levou o col922 a zero. As duas revertidas no ato.

  As duas falhas juntas provam que a R1 não está no leitor do papel: _reiv só repete a escolha do matcher. No col920 o matcher deu as 12:21 ao marco 12:00 — a volta do intervalo,
  tipo E — por distância de 21 min, que é a lei do cluster-guard. A origem é escala/utils.py::_match_marcos/_alinhar, zona com bateria de certificação própria. Não curo por
  wrapper no leitor (a LEI-AKITA 1 ficou provada na prática, duas vezes) nem mexo na grade agora: R1 fica com origem nomeada, 5 dias, +26,1 h, e a próxima tentativa começa por
  censo, como na O81.

  (3) Mutirão: 86 casos vão à mesa da supervisão às 12:00 de 28/09 se ninguém responder. Foram 159 chamados que receberam o relógio em 145 colabs; 73 já fecharam entre o envio e
  esta medição — gente respondendo, que é o efeito que o toque existe para ter.

  E o registro que eu preciso deixar explícito: HORAS_DE_PRAZO = 48 não foi tocada. Nasceu um prazo por ato, informado por quem manda, porque é o mesmo campo do relógio de SLA —
  encurtá-lo de 48h para ~12h para 158 pessoas é corte seu, e está no RELATO com essa frase.

✻ Cooked for 35m 11s · done 7:28 PM · 1 shell still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ censo do matcher: quantos dias da frota a escolha do marco inverte
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · ← for agents · ↓ to manage
  ⧉  index
```
