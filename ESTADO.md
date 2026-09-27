# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:245 AFIRMA ato sem PROVA ao lado -- ## E1 NO AR e provada em prod · O85 com RED evidenciado · push `c35dbe8c`
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 03:42:09.**

| | |
|---|---|
| `HEAD` local | `95d74797 [O73] a volta da pausa deixa de ser atraso -- e o apply da L-084 PAROU com 286 h de desconto na mesa` |
| `origin/main` | `9746080f [O89] o item do PENDENTES nasce com o tipo do vocabulario: ! (o selo cobrou e esta certo)` |
| commits a subir (`ahead`) | **1** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
95d74797 [O73] a volta da pausa deixa de ser atraso -- e o apply da L-084 PAROU com 286 h de desconto na mesa
9746080f [O89] o item do PENDENTES nasce com o tipo do vocabulario: ! (o selo cobrou e esta certo)
ea91c992 [TICKETS] rodape com o push que pousou; a O90 ganha o terceiro caso
56475e40 [TICKETS] a linha da vigencia passa a ter o ID E1, que e a tag citada nos commits
ffa4079e [TICKETS] linhas de O66 O73 O87 O88 O89 O90
```

## `git status --short`

```
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

● 19 colabs, horas_atraso +5,95 h e horas_saida_antecipada +221,21 h — ou seja ~11,6 h de desconto por pessoa. O critério do MODO 24H diz "aplica", mas o próprio motor avisa
  (:2049) que a L-084 mascarava esse efeito, e a O88 mostrou que a ata assigna a batida mais próxima e órfana o resto. Não desconto 221 h de 19 pessoas sem saber o que são. Meço
  quem são:

  Ran 1 shell command

● Agora está claro, e confirma a cautela. Os piores casos são absurdos dimensionais:

  ┌────────┬─────────────┬───────┬────────────────────┬────────────────────────────┐
  │ colab  │  cadastro   │  dia  │     trabalhou      │ "saída antecipada" cobrada │
  ├────────┼─────────────┼───────┼────────────────────┼────────────────────────────┤
  │ col820 │ 19:00–07:00 │ 01/09 │ 16:19→18:38 = 2h19 │ 741 min = 12h21            │
  ├────────┼─────────────┼───────┼────────────────────┼────────────────────────────┤
  │ col769 │ 10:00–22:00 │ 18/09 │ 07:02→10:12 = 3h10 │ 708 min = 11h48            │
  ├────────┼─────────────┼───────┼────────────────────┼────────────────────────────┤
  │ col872 │ 07:00–19:00 │ 07/09 │ 09:00→12:08 = 3h08 │ 492 min                    │
  └────────┴─────────────┴───────┴────────────────────┴────────────────────────────┘

  Nesses dias a entrada está a 161–178 min do marco — logo abaixo dos 180 da L-084 — e a saída a 700+ min. Uma ponta escapa por minutos e o dia inteiro é cobrado:

● Commitar a O73 e a parada da L-084
  ⎿  $ cd /home/ronald/saas-hasner && python3 - <<'PYEOF'
     import json
     p='app/docs/PENDENTES_RONALD.json'; d=json.load(open(p))
     alvo = d if isinstance(d, list) else d.setdefault('itens', [])
     item = {
     "id": "L-084-FORMA-DO-CORTE",
     "tipo": "corte",
     "quando": "27/09 03:5x",
     "o_que": ("L-084: alem das duas p…

✶ Transmogrifying… (47m 52s · ↓ 81.8k tokens)
  ⎿  Tip: Use /clear to start fresh when switching topics and free up context
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 1 shell · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
