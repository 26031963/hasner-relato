# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:568 AFIRMA ato sem PROVA ao lado -- ## O14 NO AR: o app dos ~750 parou de inventar turno aberto -- **1.349 avisos FALSOS a menos, 0 batida perdida**
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 01/10/2026 18:44:31.**

| | |
|---|---|
| `HEAD` local | `0ba9872f Lei (1) respondida: o recorte do T8 vale e a literal esta descartada; as duas linhas de lei saem do topo` |
| `origin/main` | `c965b638 O +114,00 h de atraso aberto por causa: 8 dos 10 maiores sao turno ABERTO ou par nulo` |
| commits a subir (`ahead`) | **2** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **78** (baseline divergiu 40, nunca lancada 33, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
0ba9872f Lei (1) respondida: o recorte do T8 vale e a literal esta descartada; as duas linhas de lei saem do topo
867fc8a1 ACHADO DE PROD: 7 colaboradores ATIVOS bateram ponto na 10 e tem fechamento ZERO
c965b638 O +114,00 h de atraso aberto por causa: 8 dos 10 maiores sao turno ABERTO ou par nulo
92a1c4e6 A segunda causa tambem estava escrita na casa: a L-084 julga ANTES da janela. Noturnas -47,80 -> +0,67 h
5e42de55 A causa da folga era UMA LINHA: o nucleo adivinhava o dia. Folga +1.035,67 -> +23,87 h, trabalhadas -1.231 -> -219
```

## `git status --short`

```
 M app/docs/PROMPTS.md
 M app/docs/RELATO.md
 M app/ponto/motor_calculo_v2.py
?? app/ponto/tests/test_pontualidade_em_turno_aberto.py
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
  de delta_min, mesma funcao, juiz novo = 0; sobe pela main junto do item 5. DOIS AJUSTES: (1) a faixa "atraso/saida antecipada admitidos" comeca na
  tolerancia com que a FOLHA julga (TOLERANCIA_CONFORMIDADE_MIN, a mesma de _envelope_lote_min), nao em 5 -- a frase nunca promete desconto que a folha
  nao faz (RED col290 +7 min); (2) no regime em que o motor nao julga pontualidade (lei 2), a frase diz so "Validar grava HH:MM no espelho", sem "vai
  para a folha". Fila 2: nao corta a S5b. Segue a fila.

❯ aval Ronald 01/10 18:3x GESTAO-HE-CALENDARIO-CONTROLE: OK no desenho da 2a volta -- celula com numero (▲ antes, ▼ depois), tres estados por led + risco
  + fundo, clicar MARCA e a barra unica confirma com motivo pela porta decidir_he, limite de decisao por cadastro (default 10 min, so de tela), ciencia
  no padrao. Construir na raia wt-ui; a migration do cadastro sobe pela main. Fila 2: nao corta a S5b. Merge so com o meu smoke. Segue a fila !

❯ aval Ronald 01/10 18:4x -- AVAIS-NA-MESA, infra de sessao, nenhuma lei de negocio; nao corta a S5b.
  1. bin/gerar_avais.py le app/docs/PENDENTES_RONALD.json e escreve app/docs/AVAIS.md: SO itens ABERTOS, uma linha cada -- id · tipo (! | lei |
  ok-desenho | smoke) · desde · 1 frase com o numero · a FRASE PRONTA para eu colar (aval Ronald ...). Ordem: o que trava a fila 1 primeiro.
  2. Todo item do JSON ganha estado (aberto | respondido | sem-motivo) e respondido_em; ao receber meu aval, o item FECHA no mesmo commit. Proposta
  "antes do codigo" e smoke pendente tambem sao itens.
  3. bin/relato.sh publica AVAIS.md no hasner-relato como arquivo proprio, fora da retencao por PROVA.
  4. O painel escreve "AVAIS NA MESA: N" a cada fim de marco.
     RESULTADOS OBSERVAVEIS: (1) AVAIS.md publicado com os itens de hoje (GESTAO-HE-CALENDARIO-CONTROLE, UI-RESPOSTA-DIZ-O-QUE-E, troca S5b, col369
  vinculo 1296); (2) item respondido some na proxima geracao; (3) selo de host: item sem estado = vermelho, item aberto sem frase pronta = vermelho.
     PROIBIDO: apagar historia do JSON (estado muda, texto fica); triar os 163 antigos (isso e do chat).
     CUSTO: 1 campo a mais por item e fechar o item ao receber o aval.
     Segue a fila.

● Atualizar as tres celulas de estado do BACKLOG
  ⎿  $ cd /home/ronald/saas-hasner && python3 - <<'PYEOF'
     # -*- coding: utf-8 -*-
     import io, sys
     sys.path.insert(0,'bin'); import hook_stop_fila1 as H
     P='app/docs/BACKLOG.md'
     t=io.open(P,encoding='utf-8').read()
     antes_prox = (H._proximo_da_fila() or 'nenhum').split(' -- ')[0]
     def troca_estado(ancora, no…

✢ Fermenting… (1h 38m 21s · ↓ 154.0k tokens)
  ⎿  Tip: Use /clear to start fresh when switching topics and free up context
                                                                                                                  ✔ Update installed · Restart to update
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on (shift+tab to cycle) · esc to interrupt · ← for agents
  ⧉  index
```
