# ESTADO -- o repo e a sessao agora

_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 01:38:51.**

| | |
|---|---|
| `HEAD` local | `fb20381f [O82] o portao do deploy e cego entre 00:00 e 04:00, e a divergencia por IDADE parece divergencia de SCHEMA` |
| `origin/main` | `c6c3785b [E6-PISO+O76+L-084/L-086] a hora de folga que sumia volta (+431,90 h em 11), e o dia do turno passa a sair do juiz` |
| commits a subir (`ahead`) | **3** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
fb20381f [O82] o portao do deploy e cego entre 00:00 e 04:00, e a divergencia por IDADE parece divergencia de SCHEMA
5836683c [L-084-DOC] a docstring dizia OU onde o codigo diz E, e o handoff dizia "deploy em curso" onde ele foi recusado
2d71f717 [PLACAR+O81] o placar deixa de inflar a E6, e um dia de julho arrasta o espelho de setembro
c6c3785b [E6-PISO+O76+L-084/L-086] a hora de folga que sumia volta (+431,90 h em 11), e o dia do turno passa a sair do juiz
c060e70a [O68b+L-084..L-092] o corte pelo VAO no ar, 8 leis de dominio indexadas, e o MODO 24H separando por colab
```

## `git status --short`

```
?? esteira.pausada
```

## PLACAR ESPELHO-VERDADE

| etapa | feitos | itens do programa |
|---|---:|---|
| **E1** PREVISAO INTEGRA | **1/3 = 33%** | selos de frota = 0: vinculo com fim<inicio; 12x36 com 3+ trabalha seguidos; dia de colab ativo sem previsao ou |
| **E3** MOTOR PELO JUIZ | **4/8 = 50%** | o motor le periodos do juiz da batida e jornada do juiz do previsto; DIFF no RELATO + `!` |
| **E4** LEITORES NO MESMO NUMERO | **2/5 = 40%** | selo tela == PDF == fechamento == TXT na frota, 0 divergencia |
| **E5** FECHAMENTO ONLINE | **0/2 = 0%** | fechamento = LEITURA; `recalcular` deixa de existir; so atos persistem |
| **E6** CERTIFICACAO POR ORACULO INDEPENDENTE | **2/6 = 33%** | 0 divergencia nao explicada + 0 dia sem previsao + 0 periodo fora do juiz |

| etapa | item | estado | prova |
|---|---|---|---|
| E1 | vinculo com data_fim < data_inicio (escritor unico + saneamento) | **espera Ronald** | medido: 51 colabs com vigencia impossivel (lavrar_vigencia_impossivel); restauracao pelo propositor espera o `!` |
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
| E4 | o topo do cartao e a SOMA das linhas | **FALTA** | MEDIDO o contrario: topo x coluna diverge em 7 casos conferidos (col515 11,13 x 92,30 h). Item (6) do [nome], obra aberta |
| E4 | colunas Atraso e Saida antecipada lendo a folha (O51b) | **FALTA** | (sem prova) |
| E4 | o tipo de escala exibido sai da DEFINICAO, nao do rotulo gravado | em curso | O74: `rotulo_do_desenho` + filtro `desenho_do_turno`; ficha no ar, lista de tipos espera o deploy |
| E5 | a 09 lida da celula, sem gravado envelhecendo | **FALTA** | (sem prova) |
| E5 | competencia exportada nao muda o gravado (L-092) | **FALTA** | SEM SELO -- obra O80. MEDIDO o risco: 18 exports em prod, incluindo emp2 08/2026 com 352 linhas entregues |
| E6 | calculador independente do motor, dia a dia | FEITO | `/tmp/e6b.py` roda e publica CSV; metodo VALIDADO em 27/09 (erro real no campo comparado, medido em 1,0 h contra vao de 87 h) |
| E6 | ZERO divergencia nao explicada entre oraculo e espelho | **FALTA** | MEDIDO o contrario em 27/09 ~01:10: 644 de 7.536 dias (8,5%) fora de 10 min -- 257 acima de 60 min, 233 entre 10 e 60, 77 com espelho zero e trabalho  |
| E6 | o oraculo compara contra espelho INTEGRO, nao contra o builder | **FALTA** | MEDIDO: 235 dos colabs caem no builder (espelho.py:352-361). Causa = O81 (3.986 dias de ata agregada em 06/07/08 arrastando 09 via espelho.py:585) |
| E6 | dia de batida impar nao fica fora da certificacao em silencio | **FALTA** | MEDIDO: 473 dias saem da comparacao por batida impar -- o oraculo nao os julga e ninguem mais responde por eles |
| E6 | o piso de horas que o oraculo acusa tem causa por colab | em curso | medido: 185 colabs / 2.713,8 h contra `minutos_realizados` (campo CAPADO, superestima). Item (3) do E6-PISO -- classificar por causa e cruzar com os 2 |
| E6 | hora de folga trabalhada sem escala certa entra em horas_trabalhadas | FEITO | aplicada em 09: +431,90 h em 11 colabs, HE em zero, 0 saem do TXT; reversao logs/apply_folga_antes.json |

**E6 -- calculador independente (`/tmp/e6b.py`), ultima rodada 27/09/2026 ~01:10:** **91.5% dos dias BATEM** (ate 10 min) em 7536 dias comparados; 160 colabs com divergencia.

## painel da sessao (ultimas 40 linhas)

```
  (1) C -- o espelho ve e o fechamento nao: 33 colabs, 442 h. RED col882 (77 h) e col515 (39 h). Classe nova: mesma fonte, dois numeros. Autopsia dia a dia (dia do espelho x
  periodo do fechamento), causa por arquivo:linha, cura na origem, DIFF pelo criterio do MODO 24H.
  (2) D -- nem o espelho ve plantao completo: 20 colabs, 209 h. RED col300 02/09 (17:53-06:03), col904 22/08 (18:50-06:57), col85 21/08 (18:56-07:02). Mesma receita.
  (3) B -- turno aberto 5+: 27 colabs, 734 h (col788 63 turnos abertos, col923 46, col880 11). E a O68b: vira a prioridade dela, com estes REDs.
  (4) A -- fechamento zero: col924, col391, col43, col942 (218 h). Cadastro: sem vinculo nem celula. Nao e cura de codigo; entram na lista de ajuste ao DP e na lista da
  supervisora.
  LISTA DE AJUSTE AO DP (09): os 9 colabs das classes A, C e D que estao no TXT, com as horas de cada um, no topo do RELATO, se a cura nao sair antes do export. Nenhuma lei nova.

● O70 está cumprida no dado: 17.332 de 17.332 células da janela 09 têm escala_geradora (o 6.848 era de antes da O69), e nenhuma célula nascida de 20/09 para cá nasce sem ela —
  falta só o selo e a saída do fallback. Registro isso e entro na ordem nova, pela classe (1) C:

  Ran 1 shell command

● Achado forte, e não é periodos_ft (zero nos dois). O motor, rodado agora, dá 175,73 h para o col882 contra 98,68 h gravadas; e o col515 tem três números — topo 11,13 h, coluna
  92,30 h, gravado 55,75 h. Testo a hipótese decisiva: o gravado está velho?

  Ran 2 shell commands

● O gravado não está velho: todos os 607 fechamentos de 09 foram atualizados às 00:50-00:51 de hoje, pelo recálculo do apply da folga. Então o mesmo código, no mesmo minuto,
  produziu 98,68 h na folha e 175,73 h na tela — a classe C é exatamente "mesma fonte, dois números". Leio o somador da folha:

  Ran 1 shell command

● Antes de acusar o sistema, testo se a régua da medição está certa — foi assim que o minutos_realizados capado me enganou antes. Imprimo os 26 campos do col882:

  Conferir se as horas estao em outro campo
  ⎿  $ cat > /tmp/c882_campos.py <<'PYEOF'
     from django.apps import apps
     G = lambda n: next(m for m in apps.get_models() if m.__name__ == n)
     FM = G('FechamentoMensal')
     CAMPOS = ('horas_trabalhadas','horas_noturnas','horas_extras','horas_extras_50','horas_extras_50_noturna',
     'horas_extras_100','horas_extr…

✢ Unfurling… (23m 33s · ↓ 62.2k tokens · thinking some more)
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · esc to interrupt · ← for agents · ↓ to manage
  ⧉  index
```
