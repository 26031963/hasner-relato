# ESTADO -- o repo e a sessao agora

> **RELATO retido:** /home/ronald/saas-hasner/app/docs/RELATO.md:237 AFIRMA ato sem PROVA ao lado -- divergencias**. APLICADOS **148**, SEPARADOS **31**.
>
> O RELATO nao entrou neste ciclo porque uma afirmacao de ato
> (APLICADO / no ar / feito) esta sem a linha `PROVA:` ao lado. O ESTADO e a SESSAO abaixo
> seguem valendo -- eles descrevem, nao afirmam ato.


_Gerado por `bin/relato.sh` (timer de 5 min). **Sem nome e sem CPF**: passa pela mesma raspagem do
RELATO, com a lista de nomes do banco como autoridade._

**Medido em 27/09/2026 15:02:01.**

| | |
|---|---|
| `HEAD` local | `e2753835 [O95] a pausa DECLARADA absorve quem nao voltou -- achada na autopsia da O94, medida e revertida hoje` |
| `origin/main` | `73322ebd [O68b-PAPEL] no ar com smoke no worker servido, e onde o Ronald clica` |
| commits a subir (`ahead`) | **6** |

## Fabrica

**fabricante: DESLIGADO** -- dono `Ronald`, desde `2026-09-26 10:01:34`.

- por que: 19-26/09: portas 149->149, noites 0-2 fatias, 28 de 44 caidos, O57
- condicao de saida: criterio do estrutural fechado + corte Ronald
- pacotes caidos: **72** (baseline divergiu 42, nunca lancada 25, o processo morreu sem escrever fim 2, vermelho da ARVORE no lote 2, copia falhou 1)

## `git log --oneline -5`

```
e2753835 [O95] a pausa DECLARADA absorve quem nao voltou -- achada na autopsia da O94, medida e revertida hoje
c286981e [O94] o oraculo estava certo contra a minha leitura: o par do miolo com tipo INVERTIDO partia o turno
acfa037f [O93] dois selos da casa morderam a minha cura, e os dois tinham razao
e652f7c6 [O94] PAREI o apply de 09 outra vez: 14 colabs perdem 283,01 h de hora TOTAL, e nenhum aval nomeia isso
0a657eb0 [O93] sem turno anterior, o dia e o do INICIO -- e o ensaio de dinheiro passa a exigir a lavra de PROD
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
| **E6** CERTIFICACAO POR ORACULO INDEPENDENTE | **2/6 = 33%** | 0 divergencia nao explicada + 0 dia sem previsao + 0 periodo fora do juiz |

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
| E6 | ZERO divergencia nao explicada entre oraculo e espelho | **FALTA** | MEDIDO o contrario em 27/09 ~01:10: 644 de 7.536 dias (8,5%) fora de 10 min -- 257 acima de 60 min, 233 entre 10 e 60, 77 com espelho zero e trabalho  |
| E6 | o oraculo compara contra espelho INTEGRO, nao contra o builder | **FALTA** | MEDIDO: 235 dos colabs caem no builder (espelho.py:352-361). Causa = O81 (3.986 dias de ata agregada em 06/07/08 arrastando 09 via espelho.py:585) |
| E6 | dia de batida impar nao fica fora da certificacao em silencio | **FALTA** | MEDIDO: 473 dias saem da comparacao por batida impar -- o oraculo nao os julga e ninguem mais responde por eles |
| E6 | o piso de horas que o oraculo acusa tem causa por colab | em curso | CLASSIFICADO em 27/09 02:1x (O83): 84 colabs / 1.602,2 h em 4 classes (B 27/733,7 · C 32/376,4 · D 21/274,3 · A 4/217,7); 9 no TXT = 259,3 h. RODADA 2 |
| E6 | hora de folga trabalhada sem escala certa entra em horas_trabalhadas | FEITO | aplicada em 09: +431,90 h em 11 colabs, HE em zero, 0 saem do TXT; reversao logs/apply_folga_antes.json |

**E6 -- calculador independente (`/tmp/e6b.py`), ultima rodada 27/09/2026 ~01:10:** **91.5% dos dias BATEM** (ate 10 min) em 7536 dias comparados; 160 colabs com divergencia.

## painel da sessao (ultimas 40 linhas)

```
  A medição (49 turnos abertos nos 14): onde a ata reivindica a batida (36 casos) ela está a 0–20 min do marco hi — ali a ata está certa e o turno anterior realmente não tem
  saída. Nos outros 13 a coluna do marco de entrada é -: não é "errou o papel", é silêncio, e o pareador caía no tipo gravado.

  O seu oráculo estava apontando para algo real, e a sequência o entregou. No col297 a ata diz 22:59 = ENTRADA e 06:56 = SAÍDA — e o gravado diz E nas duas (inversão da O65).
  Entre elas ficam 02:57E 03:57S da ronda, que a ata não reivindica e cujo gravado está invertido: _marca_intra_duracao exige S depois E para reconhecer pausa, não reconhece, e o
  pareador leu as duas como borda — o turno de 8 h que a ata descreve inteiro ficava aberto valendo 0 h.

  Curado (mesma família da guarda 1): quando a ata descreve o turno pelas duas pontas, o par do miolo também é X, com o teto continuando no cadastro. col297 106,28 → 127,48 h
  (acima dos 125 h que o gravado paga), col922 137,56 → 158,17 h, col51/col904/col85 idênticos. A L-007 foi de −283,01 h em 14 colabs para −219,02 h em 12: devolveu 64 h. E eu não
  havia feito assim porque protegia o almoço do col51 — medido agora, quem o protege é o _marca_intra_duracao, e a minha guarda protegia por acidente e custava o col297.

  Aplicando o seu princípio ao que sobrou: 23 dos 44 casos TÊM saída real, então não podem virar cobrança. O mais limpo é o col727: template 12:00-20:00 com intervalo declarado
  16:00-17:00, trabalho real 07:00-11:00 + 12:00-15:00, e a saída das 15:00 fica a 60 min do marco de intervalo contra 180 da borda — é comida como "ida ao intervalo" e o turno
  espera uma volta que nunca vem. −38,02 h em 7 dias, e nos dias em que a ata existe o mesmo dia fecha certo. A assimetria tem nome: _marca_intra_duracao exige prova de
  continuidade; a absorção por marco não exige nada.

  A cura candidata (exigir volta em intra_max_s) recupera o col727 para 159,49 h — mas eu a reverti: ela também moveu col51 (+3,99 h) e col382 (−1,28 h) sem que eu saiba por quê,
  e absorção de pausa é caminho quente. Virou O95, sobe com DIFF de frota; não foi para o seu PENDENTES porque a medição que falta é minha, não a sua decisão.

  Suíte final rodando com a árvore parada — editei-a com a suíte rodando duas vezes hoje, e a primeira me custou um vermelho falso (inspect.getsource lendo linhas deslocadas).
  Quando fechar: push e deploy das curas commitadas. O apply de 09 segue segurado.

✻ Cooked for 21m 24s · done 2:54 PM · 2 shells still running

❯ quando a suite fechar, push e deploy

  Read 1 file

● Suíte ainda rodando (árvore parada, nada tocado). O esperador está armado — quando o Ran aparecer eu empurro e subo na sequência: push pela régua → bin/deploy.sh --sem-migrate →
  smoke no worker servido.

  Fila do push (6 commits desde 73322ebd): O84, O93 + lavra de prod + placar, os dois selos que morderam minha cura, O94/miolo invertido, O95.

✻ Churned for 24s · done 3:02 PM · 2 shells still running
                                                                                                                                           ✔ Update installed · Restart to update
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ⏵⏵ auto mode on · 2 shells · ← for agents · ↓ to manage
  ⧉  index
```
