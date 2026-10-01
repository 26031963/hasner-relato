# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:644 AFIRMA ato sem PROVA ao lado -- ## O14 NO AR: o app dos ~750 parou de inventar turno aberto -- **1.349 avisos FALSOS a menos, 0 batida perdida**
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 01/10/2026 19:31:01.**

| | |
|---|---|
| `HEAD` local | `c07a13c5 NO AR as 19:26 com smoke em prod -- e a ressalva: quem prova a guarda e o col890, nao os outros dois` |
| `origin/main` | `c965b638 O +114,00 h de atraso aberto por causa: 8 dos 10 maiores sao turno ABERTO ou par nulo` |
| commits a subir (`ahead`) | **6** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **78** (baseline divergiu 40, nunca lancada 33, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
c07a13c5 NO AR as 19:26 com smoke em prod -- e a ressalva: quem prova a guarda e o col890, nao os outros dois
f794d405 A REGUA ESTAVA VERMELHA e nao era pelo codigo: dois selos de host, e o vigia cego para um id
3cbfd495 A guarda lia p.entrada direto e derrubou 20 testes: periodo de selo e SimpleNamespace
19e17c5f PONTUALIDADE-EM-TURNO-ABERTO: 7 campo-colab descem, 0 sobe. So fato encerrado se julga
0ba9872f Lei (1) respondida: o recorte do T8 vale e a literal esta descartada; as duas linhas de lei saem do topo
```

## `git status --short`

```
 M app/docs/HANDOFF-SESSAO.md
 M app/docs/PROMPTS.md
?? bin/keepalive.sh
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
    o hook. Agora o selo importa o hook — raspar a fonte para não copiar vocabulário é copiar por outro meio.
  - test_hook_teto_nao_conta_espera rodava contra a árvore viva, e o hook libera quando o RELATO declara PAREI. Com um PAREI legítimo no topo — o estado
    normal de uma fila esperando o ! — ele acusava "o teto gastou espera" sem exercitar o teto. Agora monta a própria árvore por HOOK_STOP_RAIZ, como os
    irmãos já faziam.
  - E o vigia era cego para a linha que você autorizou: _linha_de_item_re() exigia [A-Z] no primeiro caractere, então | 09-TURNO-ABERTO-EXPOSTA | não era
    pulado por estado — não existia para ele. Mesmo buraco da lápide de 28/09, com outra forma.

  51 selos de host, 0 vermelhos. Tentei curar isso pelo lado errado antes (fiz o hook preferir o marcador ORDEM-VIVA-TOPO) e revertí no mesmo turno: o
  selo do congelado já exige que o 1º aberto seja o declarado, e um segundo mecanismo para a mesma pergunta é juiz paralelo. A ordem mora na ordem da
  tabela; o marcador a confirma — e agora ele aponta 09-TURNO-ABERTO-EXPOSTA, que é a sua sequência de 19:0x.

  Correção do número da 09, antes do apply: são 48,26 h em 6 dia-colab, não 61,30 em 14. A conta tomou três versões e as duas primeiras estavam erradas
  por motivos diferentes — a primeira por forma (batidas em número ímpar: 169 dia-colab), a segunda porque autoridade_do_periodo busca batida só até
  meio-dia do dia 20: os oito "turnos abertos" caíam todos em 20/09 e nenhum estava aberto — col905 bateu 19:31S, col441 23:05S, col704 23:12S. O que de
  fato está exposto: col820 (07, 15 e 17/09, 11,00 h cada), col890 14/09 10,64, col60 07/09 4,45, col922 13/09 0,17.

  Isso destapou um achado próprio, medido na frota: 109 colabs têm a saída do dia 20 fora da janela da tela (324 batidas em 172 colabs), então o espelho e
  o PDF de uma competência já paga mostram o último dia como turno aberto enquanto a folha o pagou fechado. A lição já estava escrita três vezes na casa,
  sempre no chamador (porta_export.py:66, calendario.py:296, e agora eu) — e o +1 dia no chamador é o band-aid. Registrado como
  JANELA-DA-AUTORIDADE-PERDE-O-DIA-20, depois da S5b, com a nota de que a cura não é copiar o +1 dia para dentro: aquele alarga a competência junto, e a
  folha alarga só a busca.

✻ Churned for 15m 14s · done 7:27 PM · 2 shells, 1 monitor still running

❯ aval Ronald 01/10 19:3x col369 VINCULO: o vinculo 1296 (ARCOS - PSR 6x1, folga na SEXTA) vale de 22/09 em diante; corrigir a vigencia impossivel dele
  (22/09..18/09) e absorver o 1313 pela porta absorver_vigencias_posteriores, com trilha -- nunca update solto. Efeito esperado, ja medido: -4 furos
  (sextas 25/09, 02/10, 09/10, 16/10), ZERO hora movida. Se o DIFF mover qualquer hora, PAREI com a tabela. Entra depois do deploy da pontualidade; nao
  corta a S5b !

  Ran 8 shell commands

● Monitor event: "veredito final da suite (OK/FAILED/Ran e falhas nomeadas)"

✢ Wibbling… (1m 57s · ↓ 7.0k tokens · thinking)
                                                                                                                  ✔ Update installed · Restart to update
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells, 1 monitor · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
