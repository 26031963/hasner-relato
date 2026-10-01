# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:24 AFIRMA ato sem PROVA ao lado -- funcao, o simbolo existe no modulo **na versao que esta NO AR**? O commit no ar sai de `logs/deploy.stamp`,
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 01/10/2026 09:43:31.**

| | |
|---|---|
| `HEAD` local | `7a2e4afd RELATO item 3: o O108 nao sobe pela metade, e quem disse isso foram tres selos dele` |
| `origin/main` | `2b7a8537 O placar do topo do TICKETS volta a bater com o mundo` |
| commits a subir (`ahead`) | **15** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
7a2e4afd RELATO item 3: o O108 nao sobe pela metade, e quem disse isso foram tres selos dele
f6851e50 RELATO: o bug em prod com a minha frase errada corrigida no lugar onde foi escrita, a HE no ar e a lei do S5b
71f16546 A Gestao de HE esta NO AR e o que falta e o smoke dele
843f74e8 MERGE da Gestao de HE completa: listagem, aba Autorizar, lote de "Nao", busca, filtros, totais, PDF e o atalho da Central
0251070a Item 1: a autopsia do col881 esta no topo do RELATO, e ela desmente a minha propria leitura de ontem
```

## `git status --short`

```
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
  O bug em prod, na sua ordem:
  1. Deploy 08:52 — conferi antes que o disco era mudança NULA (as duas extrações são byte a byte, 3.717 selos; o único comportamento novo era celulas_do_periodo recusar escalar,
     e os 10 chamadores passam lista). A cura do O108 não subiu nisso.
  2. Smoke 21/08–20/09 pela função real (sem POST em porta de prod): col61 → 1 gerado, 5.450 bytes · col929 → 1 gerado, 4.749 bytes.
  3. Selo, não promessa: bin/import_tardio_contra_o_ar.py pergunta, para cada from <módulo local> import <s> escrito dentro de função, se o símbolo existe na versão que está no ar
     (commit de logs/deploy.stamp). 4.117 imports tardios, 0 acusados, e ele morde com a sua história: força o ar para a216cd89^ e exige que acuse pdf_espelho.py. O número caiu
     por cura do instrumento (270 → 10 → 1 → 0: submódulo, desempacotamento, PEP 562), nunca por afrouxamento.

  Ele achou um 500 vivo que não é janela: chamados/services/lembrete_app.py não existe (nem no disco nem no git) e api/views_mensageria.py:2125 o importa —
  api_mensageria_lembrete_app dá 500 desde 19/09. Declarado em DEFEITOS_CONHECIDOS (censo que só encolhe). Escrever o módulo ou remover a rota é seu.

  Item 1 — autópsia do col881, e ela desmente o que eu disse às 19:3x: não é falta nem 02/09 (li o layout com 4 decimais; tem 2). A rubrica é 0200, de 10,00 h → 5,00 h, e são dois
  colabs: o col864 ganha 7,29 h. O gravado do col881 não foi tocado (atualizado_em 28/09 00:45, e o único escritor com update_fields nesse modelo não mexe em número e põe status
  aberto, que não é o dele). Quem moveu foi o emissor — e o col864 prova pelo outro lado: gravado atualizado 19:57, dez minutos antes do TXT, já com 7,29, e o TXT daquela hora não
  emitiu a linha. A pergunta que separa: na rubrica 0200 o Domínio espera as horas de folga trabalhada (5,00) ou o dobro (10,00)? Não substituí nada. E o bloco de FALTA não é o
  que moveu (sai de datas_falta, que lê ausência lançada), então não curei ali — quando você responder, entram juntas a falta pelo gravado e o TXT carimbando o commit que o gerou.

  Item 2 — S5b: lei registrada (BUG-144), fecha a pergunta que eu havia publicado e confirma a conta à mão em 3 de 3. O DIFF por rubrica é o próximo ato na sombra, e o comando já
  existe e já nasceu "por rubrica".

  Item 3 — O108: escrevi a cura e NÃO a subi. A suite deu 5.467 testes com 3 vermelhos, todos de test_palavra_do_dia.py: o topo ganhou 23 e 24/07 que a linha não marca, e a tela
  diz "Em aberto" no 22/07 onde o cartão diz ''. É o invariante CARTÃO=ESPELHO (seu corte de 23/09) dizendo que enriquecer só a tela faz os dois divergirem pelo outro lado —
  meia-correção. O ato completo é o que a própria célula escreveu: enriquecer o espelho e o cartão passar a ler daqui, que é o O9. A cura está commitada na raia (9d68ca04), fora
  da árvore viva.

● Ran 1 stop hook
  ⎿  Stop hook error: siga: S5b-CALCULADOR-ESCREVE -- o calculador na forma do oraculo passa a ESCREVER o DiaPago, da competencia 10 em diante (aval Ronald 30/09 13

● Ler os argumentos do diff por rubrica
  ⎿  $ cd /home/ronald/saas-hasner && grep -n "add_argument" app/ponto/management/commands/diff_calculador.py | head -12; echo "=== casa no crons.py? ==="; grep -c
     "diff_calculador" app/config/crons.py

✢ Pouncing… (2h 10m 55s · ↓ 217.6k tokens)
  ⎿  Tip: Use /clear to start fresh when switching topics and free up context
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 3 shells · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
