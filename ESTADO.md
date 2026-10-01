# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:486 AFIRMA ato sem PROVA ao lado -- ### APLICADO 01/10 19:44, e a PROVA
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 01/10/2026 20:35:45.**

| | |
|---|---|
| `HEAD` local | `060dd4a2 AVAIS NA MESA no ar: 3 abertos com a frase pronta, 4 respondidos que ja somem, 163 sem triagem` |
| `origin/main` | `c07a13c5 NO AR as 19:26 com smoke em prod -- e a ressalva: quem prova a guarda e o col890, nao os outros dois` |
| commits a subir (`ahead`) | **11** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **78** (baseline divergiu 40, nunca lancada 33, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
060dd4a2 AVAIS NA MESA no ar: 3 abertos com a frase pronta, 4 respondidos que ja somem, 163 sem triagem
61720fac S5b, os dois passos medidos: a guarda e CEGA no calculador por falta de dado, e a dobra MORREU
edea15bc CORRECAO minha, e a tabela que decide: a troca ainda cria +129,70 h de atraso em 283 colabs
48a202f0 A tabela honesta da S5b: 3 rubricas 100% CEGAS, trabalhadas divergindo em 445 de 464, e o `!` NAO se pede
d1ddc677 O DIFF da S5b parou de somar a propria cegueira: noturnas sai de -5.786,47 h para 30 colabs CEGOS
```

## `git status --short`

```
 M app/docs/BACKLOG.md
 M app/docs/RELATO.md
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
  ├──────────────────────────────────────┼─────┼─────┼─────┼───────┤
  │ fechamento ZERO e situação desligado │ 3   │ 2   │ 1   │ 6     │
  ├──────────────────────────────────────┼─────┼─────┼─────┼───────┤
  │ SEM linha de fechamento              │ 0   │ 1   │ 0   │ 1     │
  ├──────────────────────────────────────┼─────┼─────┼─────┼───────┤
  │ (com batida apurável na janela)      │ 516 │ 528 │ 472 │       │
  └──────────────────────────────────────┴─────┴─────┴─────┴───────┘

  42 em 1.516 colab-mês com batida — 2,8%. O achado de prod das 16:0x está aqui dentro e confere: na 10 são 7 ativos com vínculo e 4 sem vínculo, os 11
  que eu publiquei. Os que mais doem por volume: col43 nas competências 09 (24 batidas) e 10 (38), sempre sem vínculo que cubra a janela; col924 com 41
  batidas na 10; col882 com 23. Gente batendo ponto todo dia cuja folha não existe porque o cadastro não a alcança. Nada aplicado — e a cura não é uma: as
  quatro causas pedem quatro curas, e duas são dado de cadastro, que a L-009 põe sob o seu !.

  Item (7) — as quatro Pautas escritas, pelo escritor único, assinadas como sistema, cada uma com âncora: 908 supervisão/colab:369 (qual é o dia de folga
  — o 1313 é 6x1 e não declara folga, e os dois candidatos, sexta 25/09 e segunda 28/09, estão ambos sem batida), 909 DP/colab:650 (data de fim da
  ausência #3186), 910 DP/colab:935 (batidas em 05-06/09 contra admissão em 07/09, 9,11 h de adicional noturno), 911 supervisão/colab:221 (os 51 da fase
  12x36, nenhuma proposta se aplica). Idempotentes e provado na hora: a segunda chamada respondeu já existe — nada criado nas quatro.

  Item (2) — AVAIS-NA-MESA já estava no ar, com os 3 abertos e a frase pronta.

  Sobre o item (6b), o DIFF emp3=CLT: encontrei a medição de 25/09 no JSON de pendentes (08 movia 87 de 128 colabs, +979,40 h de noturno; 09 movia 48 de
  123, +1.279,27 h), mas ela é de antes de uma dúzia de curas e não serve para o seu ! final. O DIFF novo precisa alternar Empresa.regime_trabalhista na
  sombra com rollback — nunca em prod, que é a lei do incidente de 27/08 — e a sombra usa os mesmos núcleos da suíte que está rodando. Entra assim que ela
  fechar, junto do deploy que o selo test_import_tardio_contra_o_ar está cobrando.

✻ Waiting for 1 background agent to finish

› Message from @a6628ec7e07379c89 (ctrl+o to expand)

· Zesting… (9s · thinking)
  ⎿  Tip: Run /ultrareview for a cloud-based multi-agent review that finds and verifies bugs in your branch — 3 free reviews left
                                                                                                                  ✔ Update installed · Restart to update
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell, 3 monitors · esc to interrupt · ← for agents · ↓ to manage

  ● main
  ◯ general-purpose  Stopping the pista-waiting background task                                                                28m 58s · ↓ 287.4k tokens
  ⧉  index
```
