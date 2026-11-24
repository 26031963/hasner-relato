# CODEX-ESTADO

> Ultima resposta processada: **24/11/2026 01:35** · publicado em **24/11/2026 01:37** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 24/11 01:35 · sessao 50bc8a92b541 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. Universo e empresa × competência

`sonda/anexos` conta as linhas da saída atual a partir de `sonda_leitura`, incluindo a linha em branco. `sonda_inv.r` conta as linhas do anexo anterior a partir do primeiro título.

**UNIVERSO** `pares_fechados 16316 invertidos 25 colabs 18 min_brutos 4370`. `sonda/anexos:3`

As linhas `COLAPSADO` registram os fechados com pontas efetivas iguais. O bloqueio do dia sem previsto produz essa igualdade. `sonda/anexos:4–6` · `app/ponto/motor_calculo_v2.py:1471–1483`

| Empresa | Competência | Fechados | Invertidos | Colabs | Minutos brutos | Fonte |
|:---|:---|---:|---:|---:|---:|:---|
| COLAPSADO | 10/2026 | 57 | 0 | 0 | 0 | `sonda/anexos:4` |
| COLAPSADO | 11/2026 | 77 | 0 | 0 | 0 | `sonda/anexos:5` |
| COLAPSADO | 12/2026 | 15 | 0 | 0 | 0 | `sonda/anexos:6` |
| 1 | 10/2026 | 12 | 0 | 0 | 0 | `sonda/anexos:7` |
| 1 | 11/2026 | 14 | 0 | 0 | 0 | `sonda/anexos:8` |
| 2 | 10/2026 | 6038 | 6 | 6 | 692 | `sonda/anexos:9` |
| 2 | 11/2026 | 6337 | 10 | 10 | 1846 | `sonda/anexos:10` |
| 2 | 12/2026 | 275 | 1 | 1 | 98 | `sonda/anexos:11` |
| 3 | 10/2026 | 1478 | 4 | 3 | 769 | `sonda/anexos:12` |
| 3 | 11/2026 | 1408 | 2 | 1 | 483 | `sonda/anexos:13` |
| 3 | 12/2026 | 61 | 0 | 0 | 0 | `sonda/anexos:14` |
| 4 | 10/2026 | 276 | 2 | 1 | 482 | `sonda/anexos:15` |
| 4 | 11/2026 | 258 | 0 | 0 | 0 | `sonda/anexos:16` |
| 4 | 12/2026 | 10 | 0 | 0 | 0 | `sonda/anexos:17` |

A saída fornecida termina em `rc_sombra=0`, sem traceback no trecho. `sonda/anexos:1–30`

## 2. Exemplos e ramo pelas pontas

A classificação indica quais pontas mudaram pela janela de HE. O motor aplica os recortes de entrada e saída separadamente e grava as pontas resultantes. `app/ponto/motor_calculo_v2.py:1691–1712,1731–1763,1834–1842`

| Colab | Comp. | Entrada bruta | Saída bruta | Entrada efetiva | Saída efetiva | `trab` | `fora` | `not_leg` | Ramo | Fonte |
|---:|:---|:---|:---|:---|:---|---:|---:|---:|:---|:---|
| 37 | 10/2026 | `2026-10-06 03:00:38` | `2026-10-06 07:01:01` | `2026-10-06 19:00:00` | `2026-10-06 07:00:00` | 0 | 960.4 | 0.0 | HE nas duas pontas | `sonda/anexos:18` · `sonda_inv.r:27` |
| 37 | 10/2026 | `2026-10-10 03:00:40` | `2026-10-10 07:01:50` | `2026-10-10 19:00:00` | `2026-10-10 07:00:00` | 0 | 961.2 | 0.0 | HE nas duas pontas | `sonda/anexos:19` · `sonda_inv.r:28` |
| 109 | 10/2026 | `2026-10-05 22:00:41` | `2026-10-05 22:10:41` | `2026-10-05 22:00:41` | `2026-10-05 22:00:00` | 0 | 10.7 | 0.0 | HE só na saída | `sonda/anexos:20` · `sonda_inv.r:34` |
| 113 | 11/2026 | `2026-10-31 03:00:00` | `2026-10-31 07:02:00` | `2026-10-31 19:00:00` | `2026-10-31 07:00:00` | 0 | 962.0 | 0.0 | HE nas duas pontas | `sonda/anexos:21` · `sonda_inv.r:36` |
| 113 | 11/2026 | `2026-11-02 03:00:00` | `2026-11-02 07:01:00` | `2026-11-02 19:00:00` | `2026-11-02 07:00:00` | 0 | 961.0 | 0.0 | HE nas duas pontas | `sonda/anexos:22` · `sonda_inv.r:37` |
| 174 | 10/2026 | `2026-09-24 02:32:11` | `2026-09-24 05:00:58` | `2026-09-24 21:00:00` | `2026-09-24 05:00:00` | 0 | 1108.8 | 0.0 | HE nas duas pontas | `sonda/anexos:23` |
| 174 | 11/2026 | `2026-10-25 02:32:15` | `2026-10-25 04:57:35` | `2026-10-25 21:00:00` | `2026-10-25 04:57:35` | 0 | 1107.7 | 0.0 | HE só na entrada | `sonda/anexos:24` |
| 200 | 11/2026 | `2026-11-03 02:00:33` | `2026-11-03 06:00:15` | `2026-11-03 22:00:00` | `2026-11-03 06:00:00` | 0 | 1199.7 | 0.0 | HE nas duas pontas | `sonda/anexos:25` |
| 218 | 10/2026 | `2026-10-16 03:15:06` | `2026-10-16 05:00:01` | `2026-10-16 21:00:00` | `2026-10-16 05:00:00` | 0 | 1064.9 | 0.0 | HE nas duas pontas | `sonda/anexos:26` |
| 218 | 11/2026 | `2026-11-15 03:17:11` | `2026-11-15 05:00:03` | `2026-11-15 21:00:00` | `2026-11-15 05:00:00` | 0 | 1062.9 | 0.0 | HE nas duas pontas | `sonda/anexos:27` |
| 218 | 12/2026 | `2026-11-22 03:22:03` | `2026-11-22 05:00:16` | `2026-11-22 21:00:00` | `2026-11-22 05:00:00` | 0 | 1058.2 | 0.0 | HE nas duas pontas | `sonda/anexos:28` |
| 296 | 11/2026 | `2026-11-03 00:15:19` | `2026-11-03 07:00:41` | `2026-11-03 19:00:00` | `2026-11-03 07:00:00` | 0 | 1125.4 | 0.0 | HE nas duas pontas | `sonda/anexos:29` |

## 3. Destino dos 4.370 minutos brutos

- **No próprio par invertido, zero em `minutos_trabalhados`.** O motor mede `saída efetiva − entrada efetiva` e grava `max(0, duração)`. `minutos_fora_janela` recebe a soma dos recortes, que pode superar a duração bruta do par. O teto de pontualidade lê esse campo separadamente. `sonda/anexos:3,18–29` · `app/ponto/motor_calculo_v2.py:2759–2787,2886–2895,3224–3258,1170–1196`
- **O crédito do aberto pula candidatos cuja entrada bruta já consta em um fechado de `resultado.periodos`, inclusive se o fechado tem zero trabalhado.** A guarda usa `resultado.periodos`, enquanto os fechados de folga ficam em `resultado.periodos_ft`. Para cada candidato aceito, o motor roda `calcular_periodo` e credita os campos retornados separadamente. Se o candidato também sai invertido, acrescenta zero ao trabalhado e pode acrescentar minutos ao `fora_janela`. `app/ponto/motor_calculo_v2.py:2492–2494,2513–2560,2759–2787,2886–2895,3224–3258`
- **Outro par contribui com seu próprio `minutos_trabalhados`.** A soma mensal e a lavratura leem esse campo dos períodos. `DiaPago.minutos_realizados` recebe o mapa da grade por dia. Se algum dos 4.370 minutos brutos aparece sob outro pareamento, no crédito de um candidato com outras pontas ou no realizado da grade, é [nao verificado]. `app/ponto/motor_calculo_v2.py:294–300,2549–2560` · `app/ponto/services/dia_pago.py:173–179,234–267,285–292`

## 24/11 01:31 · sessao bf4a511fb0a8 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. Universo e empresa × competência

As referências `sonda/anexos:N` numeram o trecho fornecido a partir de `sonda_leitura`, contando a linha em branco.

**UNIVERSO** `pares_fechados 16167 invertidos 174 colabs 117 min_brutos 81266`. `sonda/anexos:3`

| Empresa | Competência | Fechados | Invertidos | Colabs | Minutos brutos | Fonte |
|---:|:---|---:|---:|---:|---:|:---|
| 1 | 10/2026 | 12 | 2 | 1 | 968 | `sonda/anexos:4` |
| 1 | 11/2026 | 14 | 4 | 2 | 615 | `sonda/anexos:5` |
| 2 | 10/2026 | 6038 | 54 | 46 | 24762 | `sonda/anexos:6` |
| 2 | 11/2026 | 6337 | 74 | 59 | 34599 | `sonda/anexos:7` |
| 2 | 12/2026 | 275 | 14 | 14 | 7866 | `sonda/anexos:8` |
| 3 | 10/2026 | 1478 | 11 | 10 | 4920 | `sonda/anexos:9` |
| 3 | 11/2026 | 1408 | 9 | 7 | 4567 | `sonda/anexos:10` |
| 3 | 12/2026 | 61 | 1 | 1 | 720 | `sonda/anexos:11` |
| 4 | 10/2026 | 276 | 2 | 1 | 482 | `sonda/anexos:12` |
| 4 | 11/2026 | 258 | 2 | 2 | 1022 | `sonda/anexos:13` |
| 4 | 12/2026 | 10 | 1 | 1 | 746 | `sonda/anexos:14` |

As linhas somam **81267 minutos brutos**, um a mais que os **81266** do `UNIVERSO`. A origem da diferença é [nao verificado]. A saída fornecida termina em `rc_sombra=0`. O trecho da sonda não contém traceback. `sonda/anexos:1–27`

## 2. Exemplos e ramo das pontas efetivas

| Colab | Comp. | Entrada bruta | Saída bruta | Entrada efetiva | Saída efetiva | `trab` | `fora` | `not_leg` | Ramo pela leitura das pontas | Fonte |
|---:|:---|:---|:---|:---|:---|---:|---:|---:|:---|:---|
| 37 | 10/2026 | `2026-10-06 03:00:38` | `2026-10-06 07:01:01` | `2026-10-06 19:00:00` | `2026-10-06 07:00:00` | 0 | 960.4 | 0.0 | HE nas duas pontas | `sonda/anexos:15` |
| 37 | 10/2026 | `2026-10-10 03:00:40` | `2026-10-10 07:01:50` | `2026-10-10 19:00:00` | `2026-10-10 07:00:00` | 0 | 961.2 | 0.0 | HE nas duas pontas | `sonda/anexos:16` |
| 37 | 11/2026 | `2026-11-15 02:00:18` | `2026-11-15 07:01:13` | `2026-11-15 02:00:18` | `2026-11-15 02:00:18` | 0.0 | 300.9 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:17` |
| 50 | 12/2026 | `2026-11-22 07:00:48` | `2026-11-22 19:26:47` | `2026-11-22 07:00:48` | `2026-11-22 07:00:48` | 0.0 | 746.0 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:18` |
| 55 | 11/2026 | `2026-11-11 14:00:21` | `2026-11-11 15:08:32` | `2026-11-11 14:00:21` | `2026-11-11 14:00:21` | 0.0 | 68.2 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:19` |
| 61 | 11/2026 | `2026-11-08 07:02:51` | `2026-11-08 19:00:01` | `2026-11-08 07:02:51` | `2026-11-08 07:02:51` | 0.0 | 717.2 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:20` |
| 95 | 10/2026 | `2026-10-10 18:59:36` | `2026-10-11 06:55:15` | `2026-10-10 18:59:36` | `2026-10-10 18:59:36` | 0.0 | 655.6 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:21` |
| 109 | 10/2026 | `2026-10-05 22:00:41` | `2026-10-05 22:10:41` | `2026-10-05 22:00:41` | `2026-10-05 22:00:00` | 0 | 10.7 | 0.0 | HE só na saída | `sonda/anexos:22` |
| 110 | 11/2026 | `2026-11-13 01:59:07` | `2026-11-13 07:00:02` | `2026-11-13 01:59:07` | `2026-11-13 01:59:07` | 0.0 | 300.9 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:23` |
| 113 | 11/2026 | `2026-10-31 03:00:00` | `2026-10-31 07:02:00` | `2026-10-31 19:00:00` | `2026-10-31 07:00:00` | 0 | 962.0 | 0.0 | HE nas duas pontas | `sonda/anexos:24` |
| 113 | 11/2026 | `2026-11-02 03:00:00` | `2026-11-02 07:01:00` | `2026-11-02 19:00:00` | `2026-11-02 07:00:00` | 0 | 961.0 | 0.0 | HE nas duas pontas | `sonda/anexos:25` |
| 114 | 10/2026 | `2026-10-12 09:58:37` | `2026-10-12 17:10:11` | `2026-10-12 09:58:37` | `2026-10-12 09:58:37` | 0.0 | 431.6 | 0.0 | Bloqueio do dia sem previsto | `sonda/anexos:26` |

**HE nas duas pontas, colabs 37 e 113.** O ponto que ancora a entrada às 19h do mesmo dia civil é `_janela_do_dia`, que faz `dia = entrada.date()` e consulta `_md(dia)`. Em seguida, `entrada_efetiva` monta o marco com `marco_no_dia(entrada, hi)`, que troca a hora e preserva a data da batida. O motor calcula `data_jornada` em separado e o usa para a folga e a atribuição do período. O `data_jornada` efetivamente atribuído a cada exemplo é [nao verificado]. `sonda/anexos:15–16,24–25,47` · `app/ponto/motor_calculo_v2.py:1520–1542,1676–1712,2400–2408,2445–2461` · `app/ponto/janela_he.py:20–30,94–103`

**Condição do recorte.** A janela precisa estar ativa e vigente, o par precisa passar pela guarda de folga e pela L-084, e o dia consultado pela janela precisa estar sem decisão `autorizado`. A entrada é recortada quando a antecedência, descontada a extra declarada, supera o piso. Para a saída, a janela de saída precisa estar ativa, com marco final, e o excesso depois dele, descontada a extra declarada, também precisa superar o piso. O marco final nasce na data civil da saída e só avança um dia se o turno cruza meia-noite e o marco ficaria antes da **entrada bruta**. Com marcos de 19h e 07h, essa comparação mantém 07h no dia da batida de saída nos quatro exemplos. Os valores cadastrais de marcos, piso, extra declarada e decisão são [nao verificado]. `sonda/anexos:15–16,24–25` · `app/ponto/motor_calculo_v2.py:1389–1395,1520–1550,1641–1653,1691–1712,1731–1763` · `app/ponto/janela_he.py:41–51,73,94–103,145–153`

**Guarda de 180 min.** A L-084 dispensa a janela quando **as duas** distâncias absolutas superam 180 min. Com marcos 19h e 07h, as quatro entradas brutas estão de 15h59min20s a 16h do marco, e as saídas brutas estão de 1min01s a 2min do marco. Assim, a guarda **do par** deixa a janela passar. Há uma segunda guarda com as pontas externas do dia do juiz. As pontas e os marcos usados nessa segunda guarda são [nao verificado]. `sonda/anexos:15–16,24–25,47` · `app/ponto/motor_calculo_v2.py:1559–1571,1582–1606,1623–1629,1931–1949`

**HE só na saída, colab 109.** A entrada permanece `22:00:41` e `_saida_efetiva` recorta a saída para `22:00:00`, deixando-a 41 segundos antes da entrada. A condição é excesso de saída acima do piso após a extra declarada, com a janela de saída ativa, marco final disponível e sem autorização. O motivo específico pelo qual a entrada ficou intacta é [nao verificado]. `sonda/anexos:22` · `app/ponto/motor_calculo_v2.py:1691–1712,1731–1750` · `app/ponto/janela_he.py:145–153`

**Bloqueio do dia sem previsto, sete exemplos com pontas iguais.** A saída efetiva vira uma cópia da entrada efetiva, inclusive os segundos. Isso identifica o ramo na cópia, pois o recorte de saída da janela devolve um marco de minuto inteiro deslocado por extra declarada em minutos inteiros. `_dia_sem_previsto_fora_da_janela` exige trabalho previamente apurado, folga ou feriado, janela vigente, previsto igual a zero e ausência de autorização. Em folga com colaborador identificado, exige também escala certa. O ramo move o trabalho apurado para `minutos_fora_janela`, zera trabalho e noturno e iguala as pontas. Os valores das guardas por exemplo, inclusive se era folga ou feriado, são [nao verificado]. `sonda/anexos:17–21,23,26,49` · `app/ponto/motor_calculo_v2.py:1355–1362,1440–1483,2457–2461` · `app/ponto/janela_he.py:20–30,145–153`

## 3. Destino dos minutos brutos

- **No próprio par, a contribuição para `minutos_trabalhados` é zero.** Na inversão pela janela, os três corpos de `calcular_periodo` calculam a duração pelas pontas efetivas e gravam `max(0, duracao)`. No bloqueio do dia sem previsto, o motor zera o campo depois de calcular o par. `minutos_fora_janela` recebe as sobras do recorte ou o trabalho apurado antes do bloqueio. Na inversão estrita, as sobras podem somar mais que a duração bruta, como mostram os quatro recortes para `19:00 → 07:00`. A L-093 usa esse campo no teto de pontualidade. `sonda/anexos:15–16,24–25` · `app/ponto/motor_calculo_v2.py:1170–1196,1471–1483,2759–2787,2886–2895,3224–3258`

- **O crédito do aberto não repassa o bruto do fechado.** Ele monta candidatos a partir dos turnos do juiz. Se a entrada bruta já pertence a um fechado em `resultado.periodos`, pula o candidato mesmo quando o fechado tem `trab=0`. `_pagos_a` é formado só por `resultado.periodos`. Para um candidato que passa, o motor chama `calcular_periodo` e soma separadamente o `minutos_trabalhados` e o `minutos_fora_janela` retornados. Se esse recálculo também inverter as pontas, só pode carregar a parcela de fora da janela. A existência de candidato separado ou sobreposto nos casos da sonda é [nao verificado]. `sonda/anexos:15–26,39` · `app/ponto/motor_calculo_v2.py:2492–2494,2513–2560,2759–2787,2886–2895,3224–3258`

- **Outro par entra na soma pelo próprio valor apurado.** O total mensal soma `p.minutos_trabalhados`, e a lavra faz a soma por dia com esse mesmo campo. `DiaPago.minutos_realizados` vem por um mapa separado da grade. Os **81266** são o bruto publicado pela sonda. Quanto desse tempo aparece em outro par, no realizado da grade ou na folha lavrada é [nao verificado]. `sonda/anexos:3,15–26` · `app/ponto/motor_calculo_v2.py:294–300` · `app/ponto/services/dia_pago.py:173–179,237–267,285–292`

## 24/11 01:29 · sessao 2f08c8a501dd · E4C-F1-B2b-F0 · verificador · origem=Code

**1. NAO PROCEDE para o contrato inteiro.** As três correções pontuais funcionam, mas o rollback ainda deixa exceções descobertas. `app/chamados/services/fio.py:1508`, `app/chamados/models.py:3463`, `app/chamados/models.py:3509`

- **A — PROCEDE.** A lista deixa `StopIteration` subir da guarda. O gerador do diff a convertia em `RuntimeError`. Com campos já carregados, a ordem segue gravadas → empresa → veto. Num lote com dia diferido, a lista lê o dia da segunda pergunta antes de consultar a empresa da primeira, enquanto o gerador intercalava essas leituras. A caracterização registra contagens, e a comparação direta da ordem e da exceção com `327da9db7` fica [nao verificado]. `app/chamados/services/fio.py:1508`, `app/chamados/services/fio.py:1522`, `app/chamados/services/fio.py:1544`, `app/chamados/juizes.py:339`, `app/chamados/models.py:3826`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:179`
- **2 — PROCEDE para a falha do julgador.** O `except` restaura carimbo, resposta, validação e os três campos da lavra. Em `RETIDA`, a porta restaura os campos da tentativa, mantém a lavra anterior e registra `RECUSA_RETIDA`. Ainda escapam do rollback uma falha em `texto_vazio`, no import de `veredito_lavrado` ou em `foto_da_lavra`, todos depois da mutação. Uma falha injetada na foto deixou o carimbo tentado. O rollback também descarta `_pv_memo` e o reset inicial de `recusa_do_carimbo` não restaura o valor anterior em caso de exceção. `app/chamados/models.py:3422`, `app/chamados/models.py:3435`, `app/chamados/models.py:3463`, `app/chamados/models.py:3482`, `app/chamados/veredito_lavrado.py:279`
- **4c — PROCEDE.** A identidade sobrescreve a recusa com `RECUSA_RETIDA` antes de tocar os campos do carimbo. Com o coletor desligado, o julgador continua retornando `False` após uma leitura do `ContextVar`, sem consulta. Em `carimbar`, a foto da lavra agora vem antes dessa leitura. `app/chamados/models.py:3509`, `app/chamados/models.py:3463`, `app/chamados/services/fio.py:1635`

**2. PROCEDE. Há um leitor indireto após `carimbar_identidade`.**

- **Escritores.** O atributo em memória tem default de classe `None`. `carimbar` o zera e escreve `RECUSA_FORA_DO_FATO`, `RECUSA_C6`, `RECUSA_PAR_ATO_COLUNA` ou `RECUSA_RETIDA`. `carimbar_identidade` escreve apenas `RECUSA_RETIDA`, nesse ramo. Um teste injeta `RECUSA_C6`. `app/chamados/models.py:3387`, `app/chamados/models.py:3422`, `app/chamados/models.py:3426`, `app/chamados/models.py:3451`, `app/chamados/models.py:3461`, `app/chamados/models.py:3470`, `app/chamados/models.py:3510`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:262`
- **Leitores no código de produção.** Quatro leituras vêm logo após `carimbar`. A quinta está em `_balde_da_recusa`. No caminho de colisão, `_materializar_uma` chama `_carimbar_recusa` no `finally`, e esta chama `carimbar_identidade(VIA_FATO_OCUPADO)`. O passivo então lê a mesma instância se a via não mudou e há erro. Se a identidade devolveu `RETIDA`, o leitor cai em `recusa:COLISAO`, pois só trata especialmente `RECUSA_PAR_ATO_COLUNA`. Ocorrência com o coletor ligado em produção [nao verificado]. `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:2093`, `app/chamados/services/materializacao.py:2350`, `app/chamados/services/materializacao.py:2356`, `app/chamados/services/materializacao.py:1792`, `app/chamados/services/materializacao.py:2116`, `app/chamados/services/passivo_validado.py:151`, `app/chamados/services/passivo_validado.py:585`, `app/chamados/services/passivo_validado.py:593`
- **Leitores nos testes.** F9b lê diretamente após a identidade. As demais leituras estão em `app/chamados/tests/test_e4c_b2b_f0_espinha.py:311`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:402`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:455`, `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:223`, `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:59`, `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:134`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:78` e `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:149`. F9b: `app/chamados/tests/test_e4c_b2b_f0_espinha.py:323`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:328`

**3. PROCEDE.** Os dois testes mordem os defeitos nomeados. O primeiro abre a cerca, injeta `StopIteration` no auxiliar chamado pela guarda e exige essa classe. O segundo injeta a falha do julgador e compara instante, autor e via. Com o julgador na posição antiga, a exceção subiria com o trio tentado ainda na instância. Esse segundo teste cobre o trio. A cobertura de validação e lavra está nos testes anteriores. F9b também falharia sem a nova atribuição, embora parta de `None` em vez de semear uma recusa anterior. Execução da suíte Django [nao verificado]. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:221`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:234`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:314`, `app/chamados/models.py:3435`, `app/chamados/models.py:3465`, `app/chamados/tests/test_o261e_f2_fila.py:186`

**4. PROCEDE. E e 4b merecem ressalva na triagem.**

- **E.** O helper `engolir` já existia, mas a captura da falha **na leitura** é nova no ramo `alvo`. No legado, `.first()` pode propagar. No item, a falha de leitura vira rastro e `False` após o savepoint. O censo da cópia aponta zero chamadores diretos com `alvo`. `app/chamados/reconciliador.py:505`, `app/chamados/reconciliador.py:525`, `app/core/observ.py:86`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:638`
- **4b.** A fixture herdada lê o relógio real antes do `freeze_time` dos métodos. `setUpTestData` cria o vínculo, cujo `post_save` chama `invalidar_previsto`, que avalia `timezone.now()`. Efeito nas asserções [nao verificado]. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:116`, `app/chamados/tests/_vinculo_do_dia.py:25`, `app/escala/signals.py:137`, `app/ponto/services/fechamento.py:1607`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:223`
- **3, E′, 4a e F — SEM ACHADO adicional.** `A,B,A` segue o contrato de registrar mudanças. A corrida continua possível entre leitura e escrita e já está anotada para a ativação. O ramo aceita `modelo` genérico, com dois modelos exercitados e exigência de lista fechada [nao verificado]. O ponto cego de `getattr` e `**kwargs` é real, com uso efetivo [nao verificado]. As tags de M1 e CERCA são compartilhadas dentro de cada caso, e o teste exige `AssertionError` com a tag esperada. O resultado de **H** consta do anexo para `4346eddae`. Confirmação independente e resultado no delta [nao verificado]. `app/chamados/reconciliador.py:468`, `app/chamados/reconciliador.py:519`, `app/chamados/reconciliador.py:525`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:473`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:521`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:538`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:583`

## 24/11 01:29 · sessao 68876d82a44d · - · - · origem=Ronald (tmux)

**4. PROCEDE em E e 4b, com alcance limitado.**

- **E — PROCEDE.** O ramo de item introduz uma captura na leitura de deduplicação. A leitura sem `alvo` pode propagar a exceção. Com `alvo`, `engolir` captura, registra e devolve `False`. O best-effort de `evento` já cobria a escrita. Autorização literal do corte para essa captura e efeito em produção [nao verificado]. `app/chamados/reconciliador.py:505-532`, `app/core/observ.py:73-105`.
- **4b — PROCEDE.** A fixture herdada cria um vínculo antes do `freeze_time(T0)`. O `post_save` desse vínculo chama `invalidar_previsto`, que avalia `timezone.now()`. Os corpos dos testes novos estão congelados. Dependência das asserções do horário real [nao verificado]. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:116-128`, `app/chamados/tests/_vinculo_do_dia.py:25-26`, `app/escala/signals.py:137-146`, `app/ponto/services/fechamento.py:1607-1613`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:102,217-244`.
- **3 — SEM ACHADO adicional.** `A,B,A` dá três linhas por construção, conforme a regra de gravar as mudanças. A corrida continua possível porque a leitura precede o insert sem trava ou unicidade nessa chave. O censo local mostrou os seis chamadores diretos e nenhum passa `alvo`. Ocorrência concorrente em produção [nao verificado]. `app/chamados/reconciliador.py:466-482,525-531`, `app/core/models.py:285-316`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:620-640`.
- **E′ — SEM ACHADO.** O modelo do item é genérico, e o próprio teste usa `PerguntaDisputa` e `ChamadoColaborador`. Uma lista obrigatória de modelos [nao verificado]. `app/chamados/reconciliador.py:516-531`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:473-488`.
- **4a — SEM ACHADO adicional.** Confirmei em memória os dois pontos cegos. `getattr` com nome literal passa despercebido e o desempacotamento de `alvo` aparece como lista vazia de argumentos nomeados. Uso efetivo dessas formas para ativar a fatia [nao verificado]. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:570-595,614-640`.
- **F — NAO PROCEDE como vacuidade.** Cada tag admite mais de uma asserção, porém o executor exige `AssertionError` com a tag no caso designado. Ligando o coletor em `M1`, a porta devolve uma recusa falsy e a primeira asserção falha. O mutante `CERCA` remove a guarda e provoca o veto esperado pelo caso. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:55-61,91-93,341-354,410-422,521-545`, `app/chamados/reconciliador.py:408-409`.
- **H — SEM ACHADO.** O anexo informa o resultado para `4346eddae`. A execução histórica e o resultado da suíte no delta ficam [nao verificado].

## 24/11 01:28 · sessao fe9f81ea416b · - · - · origem=Ronald (tmux)

**2. PROCEDE.** `recusa_do_carimbo` é atributo em memória com default de classe. `carimbar` escreve `None`, `RECUSA_FORA_DO_FATO`, `RECUSA_C6`, `RECUSA_PAR_ATO_COLUNA` ou `RECUSA_RETIDA`. `carimbar_identidade` só escreve `RECUSA_RETIDA` no ramo retido. `app/chamados/models.py:3380`, `:3387`, `:3422`, `:3426`, `:3451`, `:3461`, `:3470`, `:3509–3521`.

- Leituras de produção após `carimbar`: `app/chamados/services/materializacao.py:1105`, `:2093`, `:2350`, `:2356`.
- Leitura indireta de produção após `carimbar_identidade`: `app/chamados/services/passivo_validado.py:151`. O laço entrega a mesma instância e a mesma lista de erros a `_materializar_uma` e `_balde_da_recusa`. Se houve colisão, `_materializar_uma` chama `_carimbar_recusa` no `finally`, que chama `carimbar_identidade(VIA_FATO_OCUPADO)`. Com `RETIDA` a via fica igual e o erro permanece. `_balde_da_recusa` lê o valor, compara com `RECUSA_PAR_ATO_COLUNA` e devolve a classificação do erro de colisão. Ocorrência com coletor ativo em produção [nao verificado]. `app/chamados/services/passivo_validado.py:147–154`, `:577–593`; `app/chamados/services/materializacao.py:1625–1629`, `:1782–1793`, `:2034–2042`, `:2106–2116`; `app/chamados/models.py:3509–3511`.
- Leitura direta após identidade no teste F9b: `app/chamados/tests/test_e4c_b2b_f0_espinha.py:323`, `:328`. Demais leituras de teste: `app/chamados/tests/test_e4c_b2b_f0_espinha.py:311`, `:402`; `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:455`; `app/chamados/tests/test_e4c_f5_volta3_ausencia.py:223`; `app/chamados/tests/test_e4c_f5_guarda_par_s143.py:59`; `app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:134`; `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:78`; `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:149`. A injeção de `RECUSA_C6` em teste fica em `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:262`.

## 24/11 01:27 · sessao 86c298e2de0f · - · - · origem=Ronald (tmux)

**3. PROCEDE.** Os dois testes novos falham com os defeitos que nomeiam, por inspeção. O primeiro liga a cerca, injeta `StopIteration` no próprio auxiliar importado pela guarda e exige essa classe. Com o `yield` do diff anexado, a exceção escaparia como `RuntimeError`. [test_e4c_b2b_f0_espinha.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/tests/test_e4c_b2b_f0_espinha.py:221), [fio.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/services/fio.py:1506)

O segundo injeta `RuntimeError` em `reter_se_vetada` e exige que instante, autor e via voltem aos valores anteriores. Na posição antiga do julgador, o erro escapava depois da mutação e antes da restauração. O caso cobre o trio do carimbo. A cobertura da lavra e da validação está nos testes anteriores. [test_e4c_b2b_f0_espinha.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/tests/test_e4c_b2b_f0_espinha.py:234), [models.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/models.py:3432), [models.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/models.py:3465), [test_o261e_f2_fila.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/tests/test_o261e_f2_fila.py:186)

O F9b também falharia sem a atribuição nova. Ele parte de `None` e exige `RECUSA_RETIDA`. [test_e4c_b2b_f0_espinha.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/tests/test_e4c_b2b_f0_espinha.py:314), [models.py](/home/ronald/codex_ws/81c096c32aef53d72411b8327c8aace96c6fc429/app/chamados/models.py:3386) A execução dos testes nesta cópia está [nao verificado].

## 24/11 01:26 · sessao 4c298ef0e2f4 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. Contagem por classe

As 17 classes somam **402 = C1**. Os **320** casos de `iguais_so_fechados` aparecem nas três subclasses com `motor_agora=0`. Para as demais classes, a saída não informa esse indicador. `sonda/anexos:3–20`

As referências a `sonda/anexos` contam a partir de `sonda_leitura`, a primeira linha do dado raspado. `sonda/anexos:1`

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | `sonda/anexos:4` |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | `sonda/anexos:5` |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `sonda/anexos:6` |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `sonda/anexos:7` |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | `sonda/anexos:8` |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | `sonda/anexos:9` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | `sonda/anexos:10` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | `sonda/anexos:11` |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | `sonda/anexos:12` |
| `entrada_repetida+virada_ou_outro_dia` | 1 | `sonda/anexos:13` |
| `iguais_com_aberto` | 62 | `sonda/anexos:14` |
| `iguais_so_fechados\|bruto5-30\|motor_agora=0` | 55 | `sonda/anexos:15` |
| `iguais_so_fechados\|bruto<=5\|motor_agora=0` | 208 | `sonda/anexos:16` |
| `iguais_so_fechados\|bruto>30\|motor_agora=0` | 57 | `sonda/anexos:17` |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | `sonda/anexos:18` |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | `sonda/anexos:19` |
| `virada_ou_outro_dia` | 3 | `sonda/anexos:20` |
| **Soma** | **402** | `sonda/anexos:3–20` |

**Traceback.** Nenhum traceback consta no trecho fornecido. A última linha da saída é `rc_sombra=0`. `sonda/anexos:1–125`

## 2. Exemplos `fech` e corte da faixa noturna

A ordem de cada par é `[entrada, saida, efetivo_entrada, efetivo_saida, min_trab, min_fora_janela]`. O corte é avaliado contra a faixa noturna que o par bruto alcançava. O motor calcula o noturno pelas pontas efetivas e exige interseção positiva. `sonda/anexos:73–84` · [motor_calculo_v2.py:89](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:89) · [motor_calculo_v2.py:2792](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:2792)

### `iguais_so_fechados|bruto5-30|motor_agora=0`

| Colab / dia | Bruto / leg | Par | Cortou a faixa? | Fonte |
|---|---:|---|---|---|
| 108 / 2026-10-19 | 10.5 / 0.0 | `["2026-10-19 10:00:31", "2026-10-19 22:10:29", "2026-10-19 10:00:31", "2026-10-19 22:00:00", 657.9, 10.5]` | Sim, saída efetiva = 22:00 | `sonda/anexos:73` |
| 108 / 2026-10-21 | 6.3 / 0.0 | `["2026-10-21 09:59:32", "2026-10-21 22:06:16", "2026-10-21 10:00:00", "2026-10-21 22:00:00", 659.9, 6.7]` | Sim, saída efetiva = 22:00 | `sonda/anexos:74` |
| 108 / 2026-10-25 | 17.6 / 0.0 | `["2026-10-25 10:00:52", "2026-10-25 22:17:37", "2026-10-25 10:00:52", "2026-10-25 22:00:00", 658.1, 17.6]` | Sim, saída efetiva = 22:00 | `sonda/anexos:75` |
| 108 / 2026-11-16 | 15.6 / 0.0 | `["2026-11-16 09:59:13", "2026-11-16 22:15:37", "2026-11-16 10:00:00", "2026-11-16 22:00:00", 659.7, 16.4]` | Sim, saída efetiva = 22:00 | `sonda/anexos:76` |

### `iguais_so_fechados|bruto<=5|motor_agora=0`

| Colab / dia | Bruto / leg | Par | Cortou a faixa? | Fonte |
|---|---:|---|---|---|
| 60 / 2026-10-31 | 3.5 / 0.0 | `["2026-10-31 09:52:32", "2026-10-31 22:03:32", "2026-10-31 09:52:32", "2026-10-31 19:00:00", 486.3, 183.5]` | Sim, saída efetiva 19:00 < 22:00 | `sonda/anexos:77` |
| 108 / 2026-10-01 | 0.7 / 0.0 | `["2026-10-01 09:59:41", "2026-10-01 22:00:39", "2026-10-01 10:00:00", "2026-10-01 22:00:00", 659.3, 1.0]` | Sim, saída efetiva = 22:00 | `sonda/anexos:78` |
| 108 / 2026-10-03 | 0.5 / 0.0 | `["2026-10-03 09:56:13", "2026-10-03 22:00:27", "2026-10-03 10:00:00", "2026-10-03 22:00:00", 658.2, 4.2]` | Sim, saída efetiva = 22:00 | `sonda/anexos:79` |
| 108 / 2026-10-05 | 0.9 / 0.0 | `["2026-10-05 09:56:07", "2026-10-05 22:00:54", "2026-10-05 10:00:00", "2026-10-05 22:00:00", 660.5, 4.8]` | Sim, saída efetiva = 22:00 | `sonda/anexos:80` |

### `iguais_so_fechados|bruto>30|motor_agora=0`

| Colab / dia | Bruto / leg | Par | Cortou a faixa? | Fonte |
|---|---:|---|---|---|
| 37 / 2026-10-05 | 119.4 / 0.0 | `["2026-10-06 03:00:38", "2026-10-06 07:01:01", "2026-10-06 19:00:00", "2026-10-06 07:00:00", 0, 960.4]` | Sim, entrada efetiva 19:00 > 05:00 e posterior à saída efetiva de 07:00 | `sonda/anexos:81` |
| 37 / 2026-10-09 | 119.3 / 0.0 | `["2026-10-10 03:00:40", "2026-10-10 07:01:50", "2026-10-10 19:00:00", "2026-10-10 07:00:00", 0, 961.2]` | Sim, entrada efetiva 19:00 > 05:00 e posterior à saída efetiva de 07:00 | `sonda/anexos:82` |
| 113 / 2026-10-30 | 120.0 / 0.0 | `["2026-10-31 03:00:00", "2026-10-31 07:02:00", "2026-10-31 19:00:00", "2026-10-31 07:00:00", 0, 962.0]` | Sim, entrada efetiva 19:00 > 05:00 e posterior à saída efetiva de 07:00 | `sonda/anexos:83` |
| 113 / 2026-11-01 | 120.0 / 0.0 | `["2026-11-02 03:00:00", "2026-11-02 07:01:00", "2026-11-02 19:00:00", "2026-11-02 07:00:00", 0, 961.0]` | Sim, entrada efetiva 19:00 > 05:00 e posterior à saída efetiva de 07:00 | `sonda/anexos:84` |

Nos oito primeiros exemplos, o trecho efetivo termina às 22h ou antes. Nos quatro de `bruto>30`, o par efetivo está invertido, e `minutos_noturnos` retorna zero quando a entrada é igual ou posterior à saída. A regra ou configuração que produziu cada recorte na sombra fica **[nao verificado]**. `sonda/anexos:73–84` · [motor_calculo_v2.py:89](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/motor_calculo_v2.py:89)

## 3. `motor_agora>0` com `DiaPago.horas_noturnas=0`

**Só caracteriza uma lavra desatualizada frente ao recálculo se o valor positivo for o que o fechamento de hoje entregaria a `lavrar` para a mesma linha.** O fechamento passa os períodos e o mapa `dia_da_jornada` à lavra da versão `motor`. `lavrar` soma `minutos_noturnos_legais / 60` no dia atribuído, tanto nos períodos normais quanto nas partições de folga trabalhada, e grava a rubrica em `DiaPago.horas_noturnas`. Se a soma hoje entregue à mesma linha fosse positiva, o zero persistido divergiriria do que seria lavrado hoje. [fechamento.py:1200](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/fechamento.py:1200) · [dia_pago.py:126](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:126) · [dia_pago.py:173](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:173) · [dia_pago.py:223](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:223) · [dia_pago.py:345](/home/ronald/codex_ws/327da9db714e53557f69cd1800d673fe8c99c768/app/ponto/services/dia_pago.py:345)

Nesta saída, o zero do C1 vem do `DiaPago` lavrado, e os 320 `iguais_so_fechados` também têm zero no recálculo da sonda. A equivalência entre o recálculo da sonda e os períodos que o fechamento entregaria à mesma linha, a data e a régua da lavra original, e a causa de uma eventual divergência ficam **[nao verificado]**. O código de `sonda_c1c.py` e o vínculo da cópia com o hash informado também ficam **[nao verificado]**. `causa_c1.r:3–5` · `sonda/anexos:1,15–17`

## 24/11 01:23 · sessao bc2e125c7f87 · NOTURNO-NAO-APURADO · sonda · origem=Code

## 1. C1 e contagem por classe

**C1 = 402.** As referências a `sonda/anexos` numeram a saída colada a partir de `C1 402` como linha 1. `sonda/anexos:1`

| Classe | Contagem | Fonte |
|---|---:|---|
| `aberto_ou_saida_orfa` | 1 | `sonda/anexos:2` |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | `sonda/anexos:3` |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `sonda/anexos:4` |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | `sonda/anexos:5` |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | `sonda/anexos:6` |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | `sonda/anexos:7` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | `sonda/anexos:8` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | `sonda/anexos:9` |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | `sonda/anexos:10` |
| `entrada_repetida+virada_ou_outro_dia` | 1 | `sonda/anexos:11` |
| `iguais_com_aberto` | 62 | `sonda/anexos:12` |
| `iguais_so_fechados` | 320 | `sonda/anexos:13` |
| `iguais_sem_par` | 0, por saldo | `sonda/anexos:1–16` |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | `sonda/anexos:14` |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | `sonda/anexos:15` |
| `virada_ou_outro_dia` | 3 | `sonda/anexos:16` |

As demais classes somam 20. A soma fecha em **20 + 62 + 320 + 0 = 402**. A linha de `iguais_sem_par` está ausente da listagem, e seu saldo é `402 − (20 + 62 + 320) = 0`. `sonda/anexos:1–16`

A saída fornecida termina em `rc_sombra=0` e não contém traceback para transcrever. `sonda/anexos:1–89`

## 2. Cinco exemplos de `iguais_so_fechados`

Em cada exemplo, usei o único par publicado, igual no motor e no juiz. A conta mede a interseção das pontas brutas com a faixa das 22h do dia indicado às 05h do dia seguinte, sem prorrogação e sem desconto de pausas. Os cinco pares intersectam a faixa. `sonda/anexos:76–80` · `app/ponto/motor_calculo_v2.py:99–118`

### `37` · `2026-10-05`

Batidas transcritas por data. `sonda/anexos:76`

```text
2026-10-03  19:00:08 E
2026-10-04  02:00:42 S  03:00:46 E  07:00:00 S
2026-10-06  02:00:15 S  03:00:38 E  07:01:01 S
2026-10-07  19:21:03 E
```

Motor e juiz `2026-10-06 03:00:38 → 2026-10-06 07:01:01`. **Sim.** Na madrugada de 06/10, `05:00:00 − 03:00:38 = 1h59min22s`. `sonda/anexos:76` · `app/ponto/motor_calculo_v2.py:99–118`

### `37` · `2026-10-09`

Batidas transcritas por data. `sonda/anexos:77`

```text
2026-10-07  19:21:03 E
2026-10-08  02:00:01 S  03:00:25 E  07:00:25 S
2026-10-10  02:00:04 S  03:00:40 E  07:01:50 S
2026-10-11  19:00:04 E
```

Motor e juiz `2026-10-10 03:00:40 → 2026-10-10 07:01:50`. **Sim.** Na madrugada de 10/10, `05:00:00 − 03:00:40 = 1h59min20s`. `sonda/anexos:77` · `app/ponto/motor_calculo_v2.py:99–118`

### `60` · `2026-10-31`

Batidas transcritas por data. `sonda/anexos:78`

```text
2026-10-29  06:56:19 E  11:02:34 S  12:02:20 E  19:00:25 S
2026-10-31  09:52:32 E  12:02:15 S  13:03:23 E  22:03:32 S
2026-11-02  10:05:20 E  15:20:23 S  16:20:32 E  22:01:25 S
```

Motor e juiz `2026-10-31 09:52:32 → 2026-10-31 22:03:32`. **Sim.** Em 31/10, `22:03:32 − 22:00:00 = 3min32s`. `sonda/anexos:78` · `app/ponto/motor_calculo_v2.py:99–118`

### `108` · `2026-10-01`

Batidas transcritas por data. `sonda/anexos:79`

```text
2026-10-01  09:59:41 E  15:00:22 S  16:01:05 E  22:00:39 S
2026-10-03  09:56:13 E  15:00:34 S  16:02:19 E  22:00:27 S
```

Motor e juiz `2026-10-01 09:59:41 → 2026-10-01 22:00:39`. **Sim.** Em 01/10, `22:00:39 − 22:00:00 = 39s`. `sonda/anexos:79` · `app/ponto/motor_calculo_v2.py:99–118`

### `108` · `2026-10-03`

Batidas transcritas por data. `sonda/anexos:80`

```text
2026-10-01  09:59:41 E  15:00:22 S  16:01:05 E  22:00:39 S
2026-10-03  09:56:13 E  15:00:34 S  16:02:19 E  22:00:27 S
2026-10-05  09:56:07 E  15:01:46 S  16:01:15 E  22:00:54 S
```

Motor e juiz `2026-10-03 09:56:13 → 2026-10-03 22:00:27`. **Sim.** Em 03/10, `22:00:27 − 22:00:00 = 27s`. `sonda/anexos:80` · `app/ponto/motor_calculo_v2.py:99–118`

## 3. Sítios candidatos por exemplo

A janela de HE teria de retirar toda a interseção ou produzir `saída efetiva <= entrada efetiva`. Se a janela deixar interseção positiva, `calcular_noturnas` só a zera por desconto de pausas quando elas cobrem todo o noturno efetivo, inclusive a prorrogação quando aplicável. `app/ponto/motor_calculo_v2.py:89–118,162–196,983–1005,1689–1763`

| Colab e dia | Condição que explicaria zero no cálculo do par |
|---|---|
| `37` · `2026-10-05` | **Janela de HE na entrada.** Deslocar a entrada efetiva de 06/10 para `05:00:00` ou depois retiraria a faixa de `1h59min22s`. Um recorte só da saída teria de chegar à entrada ou antes dela para zerar. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `03:00:38–05:00:00` e, se houver prorrogação, também `05:00:00–07:01:01`. `sonda/anexos:76` · `app/ponto/motor_calculo_v2.py:89–118,983–987,1689–1750` |
| `37` · `2026-10-09` | **Janela de HE na entrada.** Deslocar a entrada efetiva de 10/10 para `05:00:00` ou depois retiraria a faixa de `1h59min20s`. Um recorte só da saída teria de chegar à entrada ou antes dela para zerar. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `03:00:40–05:00:00` e, se houver prorrogação, também `05:00:00–07:01:50`. `sonda/anexos:77` · `app/ponto/motor_calculo_v2.py:89–118,983–987,1689–1750` |
| `60` · `2026-10-31` | **Janela de HE na saída.** Uma saída efetiva às `22:00:00` ou antes eliminaria os `3min32s`. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `22:00:00–22:03:32`. `sonda/anexos:78` · `app/ponto/motor_calculo_v2.py:99–118,983–987,1731–1750` |
| `108` · `2026-10-01` | **Janela de HE na saída.** Uma saída efetiva às `22:00:00` ou antes eliminaria os `39s`. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `22:00:00–22:00:39`. `sonda/anexos:79` · `app/ponto/motor_calculo_v2.py:99–118,983–987,1731–1750` |
| `108` · `2026-10-03` | **Janela de HE na saída.** Uma saída efetiva às `22:00:00` ou antes eliminaria os `27s`. Com as pontas brutas mantidas, pausas julgadas teriam de cobrir `22:00:00–22:00:27`. `sonda/anexos:80` · `app/ponto/motor_calculo_v2.py:99–118,983–987,1731–1750` |

Para cada um dos cinco, a aplicação da janela, os marcos, as pontas efetivas, as pausas julgadas e a prorrogação estão **[nao verificado]**. O cálculo preserva frações de minuto e a lavra soma os minutos legais divididos por 60 em um campo `FloatField`. Os `39s` e `27s`, por si sós, dão valor positivo nesse caminho. `app/ponto/motor_calculo_v2.py:112–118,983–992` · `app/ponto/services/dia_pago.py:173–179` · `app/ponto/models.py:327`

Dois sítios posteriores também são candidatos para cada exemplo. **Bloqueio do dia sem previsto** exigiria trabalho apurado, folga ou feriado, janela vigente, previsto igual a zero, ausência de autorização e, em folga com colaborador identificado, escala certa. O bloqueio zera os dois campos de noturno. As guardas e o antes/depois de cada exemplo estão **[nao verificado]**. `app/ponto/motor_calculo_v2.py:1440–1483,2457–2461`

**Fatia ou atribuição na lavra** exigiria verificar se o par entrou no resultado usado pelo fechamento e em que data foi somado. Nos dois exemplos do `37`, a data civil da entrada é o dia seguinte ao `dia` publicado. Na lavra normal, o mapa da jornada pode atribuir o par ao dia publicado, com a data civil da entrada como fallback. No O85, o fechamento agrupa pela data civil da entrada e encaminha só minutos trabalhados. O mapa efetivo, a aplicação do O85 e a correspondência entre o motor vivo da sonda e o fechamento que gerou o zero estão **[nao verificado]**. `sonda/anexos:76–80` · `app/ponto/services/espelho.py:1198–1227` · `app/ponto/motor_calculo_v2.py:2399–2432` · `app/ponto/services/fechamento.py:792–796,838–843,1201–1217` · `app/ponto/services/dia_pago.py:126–132,269–272`

## 24/11 01:20 · sessao d6484a37d1f5 · NOTURNO-NAO-APURADO · leitura · origem=Code

## C1 e alcance da sonda

**Definição em `logs/PAR-PELO-TURNO/g3/g3.py`: [nao verificado].** O arquivo não está disponível na cópia. O predicado observável em `mapa.r` seleciona dias de **01/10 a 22/11/2026** com `DiaPago` do tipo `dia`, versão `motor`, na competência determinada pelo fim da janela, com `minutos_realizados > 0` e `horas_noturnas == 0`. Exige também ao menos uma batida apurável a partir das 22h do dia ou antes das 05h do dia seguinte. Uma batida isolada satisfaz esse último filtro. `mapa.r §5, bloco Python:21–23,57–83` · `app/ponto/turnos.py:2896–2904`

**382 dos 402 dias** tiveram o mesmo multiconjunto de pontas brutas por dia, inclusive `null`. A comparação lê `p.entrada/p.saida` do motor vivo pelo espelho e `t.entrada/t.saida` de `turnos_do_colab`. O zero usado para selecionar C1 vem do `DiaPago` já lavrado. A correspondência entre essa lavra e as rubricas do recálculo da sonda está **[nao verificado]**. `sonda/anexos:1–15,64` · `mapa.r §5, bloco Python:57–83,86–119` · `app/ponto/services/espelho.py:1198–1227`

## (a) Dia com turno aberto

**O período com `saida is None` sai de `calcular_periodo` com zero noturno.** Os três ramos marcam `turno_aberto=True` e retornam antes da janela de HE e de `_escrever_noturnas`. Os dois campos de noturno ficam no zero inicial. Isso explica o zero **do período aberto** de **28 / 2026-11-22**, cuja única ponta publicada pelos dois lados é `19:02:43 → null`. Por que a batida de `23/11 02:02:18` não fechou esse par está **[nao verificado]**. `sonda/anexos:64` · `app/ponto/motor_calculo_v2.py:208–215,2750–2765,2785–2792,2877–2897,3215–3225,3256–3260`

**Um trecho fechado aproveitado no crédito do aberto pode ter noturno calculado e descartado.** `calcular_mes` procura abertos em `resultado.periodos` e usa os turnos do seu juiz interno para o mesmo dia. Com borda posicional, só candidata a cauda se o início da pausa coincide com `_fim_fechado`; sem borda posicional, candidata `entrada → _fim_fechado` de um turno interno com saída `None`, quando esse fim existe. Pula o candidato se a entrada já está em `_pagos_a` ou se `saída <= entrada`. Nos demais, `calcular_periodo` produz `_pc`, mas o crédito só lê `_pc.minutos_trabalhados` e `_pc.minutos_fora_janela`; se algum dos totais for positivo, escreve esses dois campos no primeiro aberto do dia. **É nessa transferência que o noturno de `_pc` fica sem chegar à lavra.** Para **28 / 2026-11-22**, a sonda mostra o aberto, mas a presença de candidato no juiz interno e o valor de `_pc` estão **[nao verificado]**. Um período fechado que já entrou nas listas do resultado segue pela soma normal da lavra. `sonda/anexos:64` · `app/ponto/motor_calculo_v2.py:867–882,2492–2494,2513–2564` · `app/ponto/turnos.py:609–622` · `app/ponto/services/dia_pago.py:173–179,223–265`

## (b) Dia fechado

O trecho fornecido da sonda mostra **28 / 2026-11-22** como exemplo de `iguais`, com saída `null`. Um exemplo **fechado** da classe `iguais`, com as pontas efetivas e o resultado por rubrica, está **[nao verificado]**. Os casos divergentes abaixo servem para localizar a condição que teria de ser observada. `sonda/anexos:52–76` · `mapa.r §5, bloco Python:86–119`

| Sítio | Condição exata | Exemplo da sonda |
|---|---|---|
| **Janela de HE, antes de escrever o noturno** | Com janela vigente e aplicável, sem autorização do dia, a ponta é recortada se o excesso além do marco, descontada a extra declarada, supera o piso. A saída exige também `saida_ativa` e marco final. A guarda L-084 dispensa a janela quando **ambas** as pontas do par ou do dia distam mais de 180 min dos marcos. O noturno usa as **pontas efetivas**. Se o trecho efetivo perde toda a faixa noturna ou fica com `s <= e`, a rubrica dá zero, embora as pontas brutas continuem publicadas. `app/ponto/motor_calculo_v2.py:1372–1395,1520–1571,1582–1653,1691–1763,1931–1949,2750–2765,2785–2792` · `app/ponto/janela_he.py:41–51,73,94–103,145–153` | **820 / 2026-10-29** tem o par `19:07:16 → 22:01:55` nos dois lados e um aberto adicional em J. Para esse par, se a saída efetiva cair às 22h, o recorte elimina os 1min55s depois das 22h. Marco, decisão de HE e ponta efetiva **[nao verificado]**. `sonda/anexos:55` |
| **Faixa noturna e pausas, dentro de `calcular_noturnas`** | `minutos_noturnos` retorna zero para `e >= s`, duração acima de 26h ou ausência de interseção positiva com 22h–05h. A extensão após 05h exige a opção de prorrogação, interseção com 00h–05h e fim após 05h. Do total, a função subtrai a cobertura noturna das pausas julgadas, recortadas ao par e unidas. Se o desconto cobre todo o noturno, o resultado também é zero. `app/ponto/motor_calculo_v2.py:82–118,162–196,983–1005,1881–1889` | Par fechado igual com faixa efetiva, pausas e prorrogação observadas **[nao verificado]**. `sonda/anexos:52–76` |
| **Bloqueio do dia sem previsto, depois de calcular o par** | `_dia_sem_previsto_fora_da_janela` exige entrada, trabalho apurado, folga ou feriado, janela da empresa vigente, previsto exatamente `0` e ausência de autorização. Em folga, com colaborador identificado, exige escala certa. Transfere o trabalho para `minutos_fora_janela`, zera os dois campos de noturno e colapsa `efetivo_saida` sobre `efetivo_entrada` quando esta existe. `app/ponto/motor_calculo_v2.py:1440–1483,2457–2461` | Dia da sonda com essas guardas e o antes/depois do período **[nao verificado]**. `sonda/anexos:52–76` |
| **Período ausente no dia da lavra** | O motor descarta o par antes de `calcular_periodo` se o dia atribuído cair fora da fatia. A lavra soma na data do mapa do motor, com data local da entrada na falta da chave. No caminho O85, se há vínculo na competência e uma data com batida ficou fora de todas as fatias, o fechamento calcula os órfãos à parte e encaminha só os minutos trabalhados para a linha do dia. `app/ponto/motor_calculo_v2.py:2399–2432` · `app/ponto/services/fechamento.py:792–796,817–843` · `app/ponto/services/dia_pago.py:126–132,173–179,269–272` | **174 / 2026-11-01** tem `M=[]` e um par em J atribuído a 01/11. É divergente. O destino do par em M, a aplicação do O85 e um caso equivalente em `iguais` estão **[nao verificado]**. `sonda/anexos:62` |

## Quem escreve e quem lê

| Campo | Escritor e leitor |
|---|---|
| `entrada`, `saida`, `data_turno` e pausas | `parear_turnos` monta o `Turno`; `turnos_do_colab` o entrega por `data_turno`. No caminho M, `periodos_de_turnos` converte as pontas e as pausas por entrada, e `_periodos_pelo_marco` pode substituí-las pelas da ATA. `calcular_mes` lê o trio e chama `calcular_periodo`, que grava as pontas brutas em `PeriodoCalculo`. `app/ponto/turnos.py:312–324,1997–2002` · `app/ponto/adaptador_motor.py:13–45` · `app/ponto/motor_calculo_v2.py:530–531,684–724,2350–2356,2457–2459,2750–2755` |
| `efetivo_entrada`, `efetivo_saida`, `intervalos_do_motor` e minutos noturnos | `_janela_do_par` devolve as pontas efetivas; `_carimbar_segmento` grava as pontas e as pausas recortadas. `_escrever_noturnas` recebe esses mesmos valores e escreve os minutos de relógio e legais. `segmentos_do_motor` lê os carimbos no caminho do calculador. `app/ponto/motor_calculo_v2.py:1752–1763,1834–1842,2759–2765,2792,983–1005` · `app/ponto/calculador/alimentacao.py:76–108` |
| `dia_da_jornada` e `DiaPago.horas_noturnas` | O motor publica o mapa por `id(periodo)` e `apurar_por_fatia` o mescla. O fechamento passa à lavra as listas e o mapa. `lavrar` lê `minutos_noturnos_legais`, soma `/60` no dia atribuído, inclusive nas três partições de folga trabalhada, e grava a linha `motor`. A chamada com `rubricas=` grava a versão `oraculo`. O calendário lê a linha `motor` pelo leitor de `DiaPago`. `app/ponto/motor_calculo_v2.py:1068–1075,2457–2461` · `app/ponto/services/fechamento.py:311–316,1200–1227,1277–1295` · `app/ponto/services/dia_pago.py:173–179,223–265,335–360,833–856,924` · `app/colaboradores/services/calendario.py:319,689` |
| `DiaPago.minutos_realizados` | `realizado_do_dia` lê os turnos e pode somar trechos fechados de um aberto. A grade escreve o resultado e o cartório o leva à ATA. O fechamento lê a grade da célula, `por_dia_da_grade` aplica `min(realizado, previsto)` nos dias de trabalho ou ausência e `lavrar` grava o mapa recebido. O predicado de C1 lê esse campo junto de `horas_noturnas`, que vem dos períodos. `app/ponto/turnos.py:719–743,756–771,774–795` · `app/escala/utils.py:2052–2071` · `app/ponto/services/cartorio.py:748–758,1898–1901` · `app/escala/services/leitor_celula.py:377–379,418–423` · `app/ponto/services/fechamento.py:1013–1014,1060–1064,1212–1217` · `app/ponto/services/dia_pago.py:173–179,285–292,386–402` · `mapa.r §5, bloco Python:57–65` |

## 24/11 01:16 · sessao 348924c27255 · NOTURNO-NAO-APURADO · sonda · origem=Code

| Conferência | Resultado | Fonte |
|---|---|---|
| C1 total | **402 dias** | `sonda/anexos:1` |
| Soma das classes | **382 iguais + 20 com pontas divergentes = 402** | `sonda/anexos:2–15` |
| Comparação com o g3 | **Bate com os 402** de `frota.out`. Os totais por empresa e por competência também batem. | `sonda/anexos:1–51` · `frota.out:4–23`, citado em `mapa.r` §5 · `app/docs/BACKLOG.md:1981` |
| Execução | `rc_sombra=0`. A saída anexada termina sem traceback. | `sonda/anexos:1–77` |

As referências `sonda/anexos` numeram as linhas da saída colada a partir de `C1 402` como linha 1.

### Classe, empresa e competência

`—` indica combinação sem linha impressa. A empresa 29 tem total `0` na referência do g3 e não aparece na quebra da sonda. `frota.out:4–23` · `sonda/anexos:16–34`

| Classe | Total | Emp. 1 | Emp. 2 | Emp. 3 | Emp. 4 | 10/2026 | 11/2026 | 12/2026 | Fonte `sonda/anexos` |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---|
| `aberto_ou_saida_orfa` | 1 | — | 1 | — | — | — | 1 | — | 2, 17, 39 |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 1 | — | 1 | — | — | 1 | — | — | 3, 18, 35 |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | — | 1 | — | — | — | 1 | — | 4, 19, 40 |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 1 | — | 1 | — | — | — | 1 | — | 5, 20, 41 |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 2 | — | 2 | — | — | 1 | 1 | — | 6, 21, 36, 42 |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 2 | 1 | 1 | — | — | — | 2 | — | 7, 16, 22, 43 |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 1 | — | 1 | — | — | 1 | — | — | 8, 23, 37 |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 1 | — | 1 | — | — | — | 1 | — | 9, 24, 44 |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 1 | — | 1 | — | — | — | 1 | — | 10, 25, 45 |
| `entrada_repetida+virada_ou_outro_dia` | 1 | — | 1 | — | — | — | 1 | — | 11, 26, 46 |
| `iguais` | 382 | — | 264 | 113 | 5 | 143 | 204 | 35 | 12, 27, 30, 33, 38, 47, 51 |
| `saida_apos_marco+aberto_ou_saida_orfa` | 1 | — | — | 1 | — | — | 1 | — | 13, 31, 48 |
| `saida_apos_marco+virada_ou_outro_dia` | 4 | — | 2 | 1 | 1 | — | 4 | — | 14, 28, 32, 34, 49 |
| `virada_ou_outro_dia` | 3 | — | 3 | — | — | — | 3 | — | 15, 29, 50 |
| **Soma** | **402** | **1** | **280** | **115** | **6** | **146** | **221** | **35** | 2–51 |

### Um exemplo por classe

Nas tabelas, as datas abreviadas estão em `MM-DD` de 2026. `M` é o motor, `J` é `turnos_do_colab`, `null` é uma entrada sem saída e `[]` é a lista vazia de pares no dia. `sonda/anexos:52–76` · `app/ponto/services/espelho.py:1198–1227` · `app/ponto/turnos.py:1997–2002`

| Classe | `colab_id` / dia | Batidas do exemplo | Pares de M e J | Fonte |
|---|---|---|---|---|
| `aberto_ou_saida_orfa` | 827 / 2026-11-20 | 11-18: 11:28:46 E, 17:00:12 S, 18:00:34 E, 23:31:23 S<br>11-20: 11:27:05 E, 17:00:04 S, 22:50:34 E<br>11-22: 10:55:35 E, 17:00:09 S, 18:00:04 E, 22:50:36 S | **M** 11-20 11:27:05 → 22:50:34<br>**J** 11-20 11:27:05 → 17:00:04<br>**Órfã J** 11-20 22:50:34 | `sonda/anexos:52` |
| `aberto_ou_saida_orfa+quantidade_de_blocos` | 599 / 2026-10-20 | 10-18: 11:28:35 E, 17:04:43 S, 23:30:26 E<br>10-20: 11:27:45 E, 17:02:08 S, 18:02:22 E, 23:31:41 S<br>10-22: 11:28:10 E, 18:11:24 S, 19:13:19 E, 23:30:19 S | **M** 10-20 11:27:45 → 17:02:08, 18:02:22 → `null`<br>**J** 10-20 11:27:45 → 17:02:08, 18:02:22 → 23:31:41 | `sonda/anexos:53` |
| `aberto_ou_saida_orfa+virada_ou_outro_dia` | 820 / 2026-10-28 | 10-26: 18:40:19 E, 19:46:56 S<br>10-27: 00:13:17 S, 18:09:41 E<br>10-28: 00:21:05 S, 19:06:19 E, 23:28:34 S<br>10-29: 00:43:33 E, 18:24:01 S, 19:07:16 E, 22:01:55 S<br>10-30: 23:28:07 E | **M** 10-28 19:06:19 → 23:28:34, 10-29 00:43:33 → `null`<br>**J** 10-28 19:06:19 → 23:28:34 | `sonda/anexos:54` |
| `batida_orfa_na_celula+aberto_ou_saida_orfa+virada_ou_outro_dia` | 820 / 2026-10-29 | 10-27: 00:13:17 S, 18:09:41 E<br>10-28: 00:21:05 S, 19:06:19 E, 23:28:34 S<br>10-29: 00:43:33 E, 18:24:01 S, 19:07:16 E, 22:01:55 S<br>10-30: 23:28:07 E | **M** 10-29 19:07:16 → 22:01:55<br>**J** 10-29 00:43:33 → `null`, 19:07:16 → 22:01:55<br>**Órfã J** 10-29 18:24:01 | `sonda/anexos:55` |
| `entrada_repetida+aberto_ou_saida_orfa+virada_ou_outro_dia` | 820 / 2026-11-03 | 11-02: 18:24:39 E<br>11-03: 16:53:08 E, 21:20:13 S<br>11-04: 00:32:23 E<br>11-05: 06:14:23 E | **M** 11-03 16:53:08 → 21:20:13, 11-04 00:32:23 → `null`<br>**J** 11-03 16:53:08 → 21:20:13 | `sonda/anexos:57` |
| `entrada_repetida+batida_orfa_na_celula+aberto_ou_saida_orfa` | 788 / 2026-11-09 | 11-08: 22:07:28 E<br>11-09: 04:11:16 E, 05:07:10 S, 05:58:41 E, 15:41:28 S, 18:04:19 E, 19:35:14 S, 22:59:46 S<br>11-10: 15:06:53 E, 18:39:07 S, 19:26:48 E, 22:59:39 S<br>11-11: 12:12:22 E, 18:06:09 S, 18:58:46 E, 20:00:01 S | **M** 11-09 15:41:28 → 22:59:46<br>**J** 11-09 04:11:16 → `null`, 15:41:28 → 22:59:46 | `sonda/anexos:58` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa` | 788 / 2026-10-12 | 10-10: 01:27:12 E, 03:49:20 S, 04:46:51 E, 05:59:35 S<br>10-11: 21:57:27 E<br>10-12: 03:36:36 E, 04:31:38 S, 06:00:17 E, 17:25:50 S, 21:14:21 S, 22:08:35 E, 23:01:26 S<br>10-13: 12:05:09 E, 17:56:34 S, 18:54:49 E, 20:00:46 S<br>10-14: 22:01:37 E | **M** 10-12 17:25:50 → 23:01:26<br>**J** 10-12 03:36:36 → 04:31:38, 06:00:17 → `null`<br>**Órfã J** 10-12 23:01:26 | `sonda/anexos:60` |
| `entrada_repetida+saida_apos_marco+aberto_ou_saida_orfa+virada_ou_outro_dia+quantidade_de_blocos` | 174 / 2026-10-26 | 10-24: 03:00:17 S, 04:00:48 S, 05:12:40 E<br>10-25: 02:32:15 E, 03:31:49 S, 04:57:35 S<br>10-26: 02:27:46 E<br>10-27: 02:39:56 E, 03:40:39 S, 05:01:23 S, 21:08:54 E<br>10-28: 02:40:52 S, 03:43:33 E, 05:00:25 S, 21:05:53 E | **M** 10-26 02:27:46 → `null`<br>**J** 10-27 02:39:56 → 05:01:23 | `sonda/anexos:61` |
| `entrada_repetida+saida_apos_marco+virada_ou_outro_dia` | 174 / 2026-11-01 | 10-30: 21:04:53 E<br>10-31: 02:45:30 S, 03:48:53 E, 05:07:37 S, 20:58:58 E, 21:00:09 E<br>11-01: 02:59:23 S, 04:01:26 S, 05:08:03 E<br>11-02: 02:47:25 E, 03:47:24 S, 05:09:11 S, 21:15:43 E<br>11-03: 02:49:17 S, 03:50:09 E, 04:56:52 S, 20:56:51 E | **M** `[]`<br>**J** 11-02 02:47:25 → 05:09:11 | `sonda/anexos:62` |
| `entrada_repetida+virada_ou_outro_dia` | 174 / 2026-10-24 | 10-22: 02:45:42 S, 03:45:28 E, 05:00:35 S<br>10-23: 21:12:34 E<br>10-24: 03:00:17 S, 04:00:48 S, 05:12:40 E<br>10-25: 02:32:15 E, 03:31:49 S, 04:57:35 S<br>10-26: 02:27:46 E | **M** `[]`<br>**J** 10-25 02:32:15 → 04:57:35 | `sonda/anexos:63` |
| `iguais` | 28 / 2026-11-22 | 11-20: 19:04:15 E<br>11-21: 02:03:02 S, 03:00:19 E, 07:03:23 S<br>11-22: 19:02:43 E<br>11-23: 02:02:18 S, 03:00:13 E | **M e J** 11-22 19:02:43 → `null` | `sonda/anexos:64` |
| `saida_apos_marco+aberto_ou_saida_orfa` | 769 / 2026-11-03 | 11-03: 09:52:23 E, 16:03:00 S, 22:09:25 E | **M** 11-03 09:52:23 → 22:09:25<br>**J** 11-03 09:52:23 → `null`, 16:03:00 → 22:09:25 | `sonda/anexos:69` |
| `saida_apos_marco+virada_ou_outro_dia` | 37 / 2026-11-14 | 11-12: 19:00:51 E<br>11-13: 02:00:02 S, 03:00:51 E, 07:01:25 S<br>11-15: 02:00:18 S, 03:00:30 E, 07:01:13 S<br>11-16: 19:00:34 E | **M** `[]`<br>**J** 11-15 02:00:18 → 07:01:13 | `sonda/anexos:70` |
| `virada_ou_outro_dia` | 922 / 2026-10-26 | 10-24: 07:00:00 S, 23:08:05 E<br>10-25: 07:13:10 S, 23:06:15 E<br>10-26: 03:20:43 S, 07:00:45 E, 23:10:55 S<br>10-27: 03:35:27 E, 04:40:29 S, 23:07:46 E<br>10-28: 03:51:01 S, 07:02:09 E, 23:06:23 S | **M** `[]`<br>**J** 10-26 07:00:45 → 23:10:55 | `sonda/anexos:74` |

### Regra de pareamento em cada exemplo

| Exemplo | Regra da seção 2 e alcance da evidência |
|---|---|
| **827 · 20/11** | **Papel e saída órfã.** M usa `22:50:34 E` como saída, enquanto J a devolve como saída órfã depois de fechar em `17:00:04`. No núcleo de J, o papel da ATA pode prevalecer sobre o tipo gravado, e uma saída pode ficar sem entrada. Em M, a ATA pode fornecer as pontas e o núcleo também pode fechar pela posição. O ramo que produziu a ponta de M está **[nao verificado]**. `sonda/anexos:52` · `app/ponto/turnos.py:1377–1382,1491–1502,1065–1075` · `app/ponto/motor_calculo_v2.py:632–645,669–688` |
| **599 · 20/10** | **Aberto e blocos.** J fecha o segundo bloco em `23:31:41`, e M devolve `null` para a mesma entrada. São dois pares em cada lado, com um fechado em M e dois em J. M passa os limites do regime ao núcleo. J passa o provedor da última batida, que pode permitir fechar uma pausa sem volta sob as guardas do núcleo. Qual regra fechou o bloco em J e por que M o deixou aberto estão **[nao verificado]**. `sonda/anexos:53` · `app/ponto/motor_calculo_v2.py:475–479,494–506` · `app/ponto/turnos.py:1170–1188,1997–2000` |
| **820 · 28/10** | **Dia da jornada.** O mesmo aberto de `29/10 00:43:33` aparece em 28/10 no M e em 29/10 no J. Em M, o aberto começa dentro das seis horas seguintes à saída do par fechado mostrado em 28/10, condição compatível com herdar o dia. J publica pelo `data_turno`. O ramo efetivo de atribuição em M está **[nao verificado]**. `sonda/anexos:54–55` · `app/ponto/motor_calculo_v2.py:2399–2423` · `app/ponto/turnos.py:312–324,1997–2000` |
| **820 · 29/10** | **Dia da jornada e órfã.** J conserva em 29/10 a entrada aberta que M mostrou no dia anterior e lista `18:24:01` como saída órfã. O núcleo pode separar uma saída do aberto pelo teto de fechamento ou produzir uma órfã quando não há entrada corrente. O ramo usado e qual ponta coincidiu com a lista de órfãs da célula estão **[nao verificado]**. `sonda/anexos:54–55` · `app/ponto/turnos.py:1430–1456,1487–1498,1997–2000` |
| **820 · 03/11** | **Dia da jornada e E repetida.** M põe o aberto de `04/11 00:32:23` no dia 03/11. O intervalo desde a saída de `21:20:13` cabe na regra de herança de M. As E consecutivas de 04/11 e 05/11 estão além do corte de deduplicação de M. O destino desse aberto em J e o ramo efetivo de M estão **[nao verificado]**. `sonda/anexos:57` · `app/ponto/motor_calculo_v2.py:70,736,897–912,2399–2423` |
| **788 · 09/11** | **Papel e aberto.** Os dois usam `15:41:28 S` como entrada do mesmo par. A diferença é o aberto de `04:11:16` em J. O núcleo pode consumir uma batida como miolo pelo papel da ATA, e M pode substituir períodos do dia pelas pontas da ATA. O papel efetivo da batida de `04:11:16` e o motivo de ela não figurar no M estão **[nao verificado]**. `sonda/anexos:58` · `app/ponto/turnos.py:1310–1326,1371–1401` · `app/ponto/motor_calculo_v2.py:632–645,669–688` |
| **788 · 12/10** | **Papel, aberto e órfã.** M usa `17:25:50 S` como entrada e fecha com `23:01:26`, saída que J devolve órfã. J também mantém `06:00:17` aberta. São resultados compatíveis com papéis diferentes para as batidas ou com substituição do par em M. O ramo efetivo e o valor do marco final estão **[nao verificado]**. `sonda/anexos:60` · `app/ponto/turnos.py:1377–1421,1487–1498` · `app/ponto/motor_calculo_v2.py:632–645,669–688` |
| **174 · 26/10** | **Dia da jornada e blocos fechados.** J atribui a 26/10 um par cujas duas pontas são de 27/10, conforme a regra que permite atribuir a madrugada à véspera. M mostra um aberto de 26/10. A quantidade de fechados é zero em M e um em J. Os marcos usados e a razão de M manter o aberto estão **[nao verificado]**. `sonda/anexos:61` · `app/ponto/turnos.py:285–324,1997–2000` |
| **174 · 01/11** | **Dia da jornada.** J atribui o par de 02/11 a 01/11 pela regra de `data_turno` da véspera. A lista de M está vazia no dia consultado. O destino do par em M e o valor do marco final comparado pela sonda estão **[nao verificado]**. `sonda/anexos:62` · `app/ponto/turnos.py:285–324,1997–2000` · `app/ponto/motor_calculo_v2.py:2399–2432` |
| **174 · 24/10** | **Dia da jornada.** J atribui o par de 25/10 a 24/10 pela regra da véspera. As E consecutivas de `24/10 05:12:40` e `25/10 02:32:15` estão além do corte de deduplicação de M. O tratamento dessas entradas e o destino do par em M estão **[nao verificado]**. `sonda/anexos:63` · `app/ponto/turnos.py:285–324,1997–2000` · `app/ponto/motor_calculo_v2.py:70,736,897–912` |
| **28 · 22/11** | **Aberto nos dois lados.** M e J preservam a mesma entrada com saída `null`. A razão de as batidas da madrugada seguinte não fecharem esse par está **[nao verificado]**. `sonda/anexos:64` · `app/ponto/adaptador_motor.py:24–44` · `app/ponto/turnos.py:1497–1502` |
| **769 · 03/11** | **Papel da batida e ponta bruta.** Os dois usam `22:09:25 E` como saída. J usa `16:03:00 S` como entrada desse par e mantém a primeira entrada aberta. A regra de papel da ATA permite usar o papel do marco sobre o tipo gravado. A ponta `22:09:25` é a única saída dos pares divergentes, portanto é a que dispara `saida_apos_marco` no teste da sonda. O marco exato e a saída efetiva da rubrica estão **[nao verificado]**. `sonda/anexos:69` · `mapa.r` §5, função `classe` · `app/ponto/turnos.py:1377–1421` · `app/ponto/motor_calculo_v2.py:2750–2765` |
| **37 · 14/11** | **Dia da jornada e papel.** J atribui a 14/11 o par de 15/11, com `02:00:18 S` no papel de entrada. A regra da véspera explica o dia, e a leitura do papel permite abrir com uma batida gravada como S. O destino do par em M e o valor do marco final estão **[nao verificado]**. `sonda/anexos:70` · `app/ponto/turnos.py:285–324,1377–1421,1997–2000` |
| **922 · 26/10** | **Mesmo par em dia vizinho.** As duas pontas de J são de 26/10. Pelo teste de `virada_ou_outro_dia` do script anexado, com `M=[]` no dia, o rótulo implica que o mesmo par aparece em M em 25/10 ou 27/10. M pode atribuir o dia pelo mapa interno ou por herança do período anterior. Qual dos dois dias recebeu o par e qual ramo o atribuiu estão **[nao verificado]**. `sonda/anexos:74` · `mapa.r` §5, função `classe` · `app/ponto/motor_calculo_v2.py:884–895,2399–2423` |

### O que `iguais` diz sobre C1

| Pergunta | Resposta | Fonte |
|---|---|---|
| Há dias `iguais` em C1? | **Sim, 382 dos 402.** Nesses dias, os multiconjuntos de pontas brutas por dia coincidiram, incluindo entradas com saída `null`. | `sonda/anexos:1,12,64–68` · `mapa.r` §5, função `classe` |
| A divergência de pareamento explica C1? | **A divergência de pontas medida pela sonda não explica C1 inteiro.** Ela aparece em 20 dias. A causa do noturno zerado nos 382 dias `iguais` e o efeito da divergência sobre o noturno nos outros 20 estão **[nao verificado]**. | `sonda/anexos:1–15` · `app/ponto/motor_calculo_v2.py:2750–2765` · `app/ponto/services/dia_pago.py:173–179,285–292` |

## 24/11 01:13 · sessao 152ff60dfb62 · E4C-F1-B2b-F0 · verificador · origem=Code

## E4C-F1-B2b-F0 · verificação da volta 1

**A guarda não é equivalente para toda entrada.** Reproduzi em memória um `StopIteration` que a versão anterior propagava e que agora sai como `RuntimeError`. O censo do app encontrou zero ativação do coletor e zero chamada com `alvo` em produção. A incidência em produção é [nao verificado]. `app/chamados/services/fio.py:1505–1516`, `app/chamados/services/fio.py:1541–1553`, `app/chamados/services/fio.py:1593–1642`, `app/chamados/reconciliador.py:483–487`

Os quatro blobs da cópia e os três blobs anteriores, reconstruídos em memória, bateram com os prefixos do diff anexo. A cópia veio sem `.git`. O parentesco entre os commits e a execução da suíte com ORM e banco são [nao verificado]. [nao verificado]

### A — NAO PROCEDE · equivalência para toda entrada

- No caminho normal, a cerca desligada retorna antes de consumir a entrada. Com cerca, o primeiro avanço do gerador esgota o iterável uma vez, faz a mesma leitura em lote das perguntas gravadas e alterna, por pergunta, o cálculo dos dias e a resolução da empresa. A pergunta sem PK fica fora do `IN` e ainda fornece o dia em memória. Cada versão da pergunta marcada como conversa deixa de fornecer **seu** dia. Assim, uma conversa em memória ainda pode levar o dia de fato de uma linha gravada como não conversa. `app/chamados/services/fio.py:1505–1525`, `app/chamados/services/fio.py:1541–1548`
- A guarda continua memoizando empresa por colaborador em `estado['empresa']`, usando o mesmo `estado['memo']` no veto e consultando empresas na ordem da primeira ocorrência. Ordena as empresas vetadas antes de atualizar `estado['vetadas']` e levantar `EscritaEmCompetenciaEntregue`. Confirmei em memória a mesma sequência de leituras para iterador consumível, conversa, pergunta sem PK e duas empresas. Uma exceção comum no meio conserva as empresas já memoizadas e interrompe antes da fase do veto em ambas as versões. `app/chamados/services/fio.py:1507–1525`, `app/chamados/services/fio.py:1544–1553`, `app/chamados/services/vigia_fio.py:801–811`
- A diferença aparece quando a obtenção de um dia levanta `StopIteration`. No corpo anterior ele escapava diretamente. Dentro do gerador vira `RuntimeError('generator raised StopIteration')`. O esgotamento normal da entrada continua funcionando. Também mudou o ponto do import de `Colaborador`, agora adiado até a primeira pergunta com dias. `app/chamados/services/fio.py:1505–1524`, `app/chamados/services/fio.py:1545–1548`

### B — PROCEDE · gravação com o coletor desligado

O censo de todo `app`, incluindo imports com apelido, encontrou `_RETIDAS.set/reset` apenas no coletor e nenhum ativador em produção. `reter_se_vetada` retorna `False` após `_RETIDAS.get()`, antes de ler `_CERCA`, os dias ou o banco. Os caminhos normais conservam os retornos, os `update_fields`, as consultas e as recusas anteriores. Os dois imports de `fio` são locais aos métodos. `fio` importa `ChamadoColaborador` no módulo, mas o diff não acrescentou dependência circular no carregamento dos módulos. `app/chamados/services/fio.py:187–194`, `app/chamados/services/fio.py:1593–1609`, `app/chamados/services/fio.py:1627–1642`, `app/chamados/models.py:3422–3479`, `app/chamados/models.py:3501–3518`

Há um caminho de erro novo em `carimbar`. O import local e o julgador ficam depois da mutação em memória e fora dos blocos que a desfazem. Simulei a falha do julgador e a instância ficou com instante, autor e via tentados. Com o coletor desligado, o julgador não faz leituras que possam falhar dessa forma. A posição do import também afeta uma eventual falha de carregamento. `app/chamados/models.py:3430–3447`, `app/chamados/models.py:3453–3478`, `app/chamados/services/fio.py:1632–1639`

### C — PROCEDE · escritores, leitores e alcance

| Valor | Quem escreve, quem lê e até onde chega |
|---|---|
| `recusa_do_carimbo` | `carimbar` zera e escreve `fora_do_fato`, `c6`, `par_ato_coluna` ou `retida`. `carimbar_identidade` não zera nem escreve o atributo, inclusive ao devolver `RETIDA`. `materializacao.py:1105` lê o motivo como dado e o passa à descrição e ao JSON da trilha em `:1139–1142`. Os leitores em `:2093` e `:2350–2356` distinguem apenas `par_ato_coluna`. `passivo_validado.py:151–154` também distingue apenas esse valor. Hoje nenhum deles recebe `'retida'` pela produção da cópia. `app/chamados/models.py:3382–3387`, `app/chamados/models.py:3422–3467`, `app/chamados/models.py:3501–3518`, `app/chamados/services/fio.py:1593–1609`, `app/chamados/services/fio.py:1632–1634` |
| Retorno de `carimbar` | Os consumidores diretos usam valor lógico. São `app/chamados/reconciliador.py:408`, `:702–707`, `app/chamados/services/disputa_emissao.py:2118`, `:2192` e `app/chamados/services/materializacao.py:1104`, `:2091`, `:2348`. `encerrar_fantasma` repassa o objeto em `app/chamados/models.py:3639`, e o consumidor usa `if not` em `app/chamados/reconciliador.py:1286`. |
| Retorno de `carimbar_identidade` | `fechar_por_admin` repassa o objeto em `app/chamados/models.py:3347`. Seus consumidores usam valor lógico em `app/chamados/models.py:2148` e `app/ponto/services/veredito_celula.py:195`, ou descartam em `app/chamados/services/validacao.py:556`, `:596`. Também descartam o retorno diretamente `app/chamados/models.py:2292`, `app/chamados/services/materializacao.py:1567`, `:1792` e `app/chamados/services/fio.py:1685`. Este último devolve `True` mesmo se a identidade não gravar. |
| `RETIDA` | `fio` cria a sentinela e as duas portas a devolvem com o coletor ligado. Ela é falsy e difere de `False` tanto por identidade quanto por igualdade. Não encontrei consumidor de produção das duas portas que compare com `is False`, `== False` ou persista ou serialize o retorno. Os dois repasses acima poderiam transportar o próprio objeto se alcançados com o coletor ligado. Hoje o retorno novo não nasce por esses caminhos. `app/chamados/services/fio.py:1612–1642`, `app/chamados/models.py:3463–3467`, `app/chamados/models.py:3507–3508`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:252–257` |
| `LogAuditoria` com a ação nova | `_registrar_retencao_de_item` monta a chave e o JSON, lê o último `valor_depois` e pede a escrita a `core.observ.evento`. Esse é o único leitor específico encontrado. A tela de auditoria pode exibir uma linha já existente. A foto E4B pode incluí-la por modelo e PK. O vigia de cron pode contá-la se ator e data estiverem no filtro. Nenhum chamador atual chega ao escritor da ação. Existência de linha em produção é [nao verificado]. `app/chamados/reconciliador.py:512–532`, `app/core/observ.py:94–139`, `app/core/views.py:705–736`, `app/templates/core/log_auditoria.html:55–74`, `app/chamados/services/foto_e4b.py:310–323`, `app/chamados/services/vigia_loop.py:84–97` |
| `_CERCA` e `_RETIDAS` | `executar_sob_cerca` cria e restaura `_CERCA`, lê a presença para reentrada e lê `vetadas` para desfazer. A guarda lê o contexto, preenche `empresa` e `vetadas`, e passa `memo` à consulta de competência, que o lê e preenche. `reter_se_vetada` só lê `_CERCA` quando `_RETIDAS` já está ligado. `coletor_de_retidas` cria, expõe, reutiliza e restaura `_RETIDAS`. O julgador lê e preenche os dois caches e acrescenta `(pk, alvo)` a `retidas`. Não há leitor de produção do conjunto `retidas`. `app/chamados/services/fio.py:1519–1525`, `:1541–1553`, `:1567–1584`, `:1593–1609`, `:1632–1642`, `app/chamados/services/vigia_fio.py:801–811` |

### D — NAO PROCEDE · “corpo executado byte a byte”

A assinatura, a docstring e o teste `if alvo is not None` são novos. Os imports, o desempacotamento e todo o bloco legado de escolha da ação, leitura, deduplicação e escrita são textualmente iguais ao pré-diff. O teste adicional só compara o default com `None`. Os seis sítios abaixo passam exatamente dois argumentos posicionais, todos sem `alvo`. `app/chamados/reconciliador.py:466–509`

- `encerrar_com_retencao` — `app/chamados/reconciliador.py:544`
- `retratar_com_motivo`, retenção anterior à tentativa e veto da cerca — `app/chamados/reconciliador.py:593`, `app/chamados/reconciliador.py:598`
- `reatar_pergunta_orfa`, ramo HIST — `app/chamados/reconciliador.py:1798`
- `fechar_beco` — `app/chamados/services/becos.py:71`
- `resolver_perguntas_por_chamado_resolvido` — `app/chamados/services/disputa_emissao.py:2011`

### E — NAO PROCEDE · idempotência irrestrita

- A repetição sequencial deduplica contra a **última** linha da chave `(acao, modelo, objeto_id)`. O comparador usa motivo e PKs ordenados e sem repetição. A sequência de conteúdos A, B, A grava três linhas. Duas chamadas concorrentes podem ler a mesma última linha e inserir ambas. O `atomic` envolve a leitura e escrita, mas o código não trava a chave e o modelo não declara unicidade para ela. `app/chamados/reconciliador.py:519–532`, `app/core/models.py:295–316`
- `engolir` acrescenta uma captura efetiva de `Exception` no caminho de leitura. A falha registra `ERROR` e termina em `False`, sem segunda leitura, escrita substituta ou outro critério de decisão. Pela L-001, não identifiquei fallback de negócio. A autorização nominal para essa captura específica é [nao verificado]. O savepoint próprio sai antes de `engolir` capturar a exceção, preservando um `atomic` externo saudável na mesma conexão. O teste injeta `RuntimeError` no `filter`, antes de o banco executar a consulta. A prova com uma falha real de SQL é [nao verificado]. `app/docs/LEIS.md:59`, `app/docs/PROMPTS.md:2754`, `app/chamados/reconciliador.py:525–532`, `app/core/observ.py:73–91`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:461–469`
- A ação nova é distinta das três ações legadas e não encontrei outro escritor com o mesmo literal. O ramo aceita qualquer `modelo` em `alvo`. Se alguém o chamar com `('Batida', pk)`, por exemplo, a linha pode afetar os leitores que tomam a última ação daquela batida sem filtrar o verbo. Esse cenário não tem chamador na cópia. `app/chamados/reconciliador.py:488–531`, `app/ponto/services/retratar_humano.py:35–53`, `:66–77`

### F — NAO PROCEDE · selo completo e relógio inteiramente fixo

O mecanismo exige `AssertionError` com a regex esperada. Um erro de compilação, uma falha de banco ou uma pré-condição sem a tag não contam como morte. Os trechos de substituição textual ocorrem uma vez cada. A tabela indica a primeira asserção que deve falhar pelo código. A execução dos nove mutantes com ORM é [nao verificado]. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:37–95`, `:489–516`

| Mutante | Asserção esperada |
|---|---|
| `m1_coletor_no_b2a` | `r is True`. A tag `[M1]` também aceita falha do carimbo ou da contagem de trilha. O patch envolve o método inteiro, embora o cenário invoque o B2a real. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:55–61`, `:385–390` |
| `m2_coletor_sem_reset` | Estado final após sair do coletor, `[M2:coletor]`. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:415–417` |
| `m2_dias_gravam` | Foto após consumir os dias, `[M2:dias]`. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:399–414` |
| `m2_julgador_mexe_na_cerca` | Foto após chamar o julgador desligado, `[M2:julgador]`. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:418–420` |
| `m4_sem_pks_na_chave` | A terceira chamada deve gravar quando muda um PK. A regex exige `pks diferentes`. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:446–448`, `:494` |
| `f9_porta_sem_ponto` | Identidade com `fio.RETIDA`. A regex exige a mensagem dessa asserção. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:277`, `:495` |
| `f9b_porta_sem_ponto` | Identidade com `fio.RETIDA` na porta de identidade. A regex exige a mensagem dessa asserção. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:293`, `:496` |
| `f9_julga_sob_cerca` | `r is False`. `[CERCA]` também aceita falha por ter anotado no coletor. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:321–322`, `:497` |
| `f9_none_nao_veta` | `r is True` para dia indecidível, `[NONE]`. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:298–307`, `:498` |

- Os dois testes AST passaram em memória. Confirmei que o detector vê `from .services.fio import … as c`, `import chamados.services.fio` seguido do atributo e `from chamados import reconciliador as r` seguido de `r.registrar_retencao(...)`. Escapam `getattr` por string, import isolado sem uso, `from . import nome` com reexportação, acesso direto a `_RETIDAS` e `alvo` por `**kwargs`. O selo também não verifica a aridade posicional e pode contar atributo homônimo de outro objeto. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:531–580`, `:588–625`
- Os corpos dos cenários de banco congelam o relógio. O `setUpTestData` herdado roda fora desse congelamento e cria `Colaborador`, cujo `criado_em` e `atualizado_em` leem relógio pelo ORM. A caracterização da guarda ordena os dias na espia, usa uma empresa e não testa a exceção de A. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:101–137`, `:181–204`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:115–128`, `app/colaboradores/models.py:380–381`

### G — PROCEDE · há lacunas de selo e de contrato

- **Segundo juiz, segundo escritor, derivação paralela e ordem de trava — SEM ACHADO.** Os dois caminhos usam a mesma extração de dias e consultam o mesmo veto, que delega à composição com K6. As gravações seguem pelas portas dos modelos e por `core.observ.evento`. O ramo novo abre savepoint, sem acrescentar lock à ordem global. O diff lista três arquivos de produção. `app/chamados/services/fio.py:1465–1475`, `:1496–1516`, `:1545–1553`, `:1632–1642`, `app/core/juizes.py:342`, `app/chamados/models.py:2901–2922`, `:3463–3479`, `:3507–3518`, `app/chamados/reconciliador.py:525–531`
- **Relógio e selo.** Sem leitura nova de relógio na lógica de produção do diff. O relógio solto da fixture e as formas que o selo ignora estão em F. `app/chamados/models.py:3430`, `:3512`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:115–128`, `app/chamados/tests/test_e4c_b2b_f0_espinha.py:538–580`
- **Pré-requisito.** A F5 consta como no ar em 23/11. A C7 consta como pousada em 19/11 e sua releitura sob trava está na cópia. O despacho posterior ao aval original permite construir C7 e exige rebase e RED das duas obras para quem pousar depois. O pouso da F0 e a execução dos dois REDs são [nao verificado]. `app/docs/PROMPTS.md:2636–2638`, `app/docs/RELATO.md:14`, `:1766`, `:1832`, `app/chamados/reconciliador.py:584–598`
- **Campo e significado.** `data_evento` já tinha os sentidos de dia do turno e dia civil da conversa, separados pelo discriminador. A lacuna nova é que `carimbar_identidade` pode devolver `RETIDA` mantendo `recusa_do_carimbo` de uma tentativa anterior na mesma instância. A docstring de `carimbar` ainda promete `bool` e também documenta a sentinela. `app/chamados/models.py:3067–3075`, `:3395–3419`, `:3422`, `:3463–3467`, `:3490–3518`

### H — SEM ACHADO · vizinho tornado vermelho pelo diff

O mapa de juízes passou. Passaram também as verificações de fonte executadas para a porta do carimbo, as catracas E1 e os selos E4-C20 e E4B-C12. A catraca da porta compara **funções** chamadoras de `_desfazer_em_memoria`, por isso a chamada adicional dentro de `carimbar` conserva o conjunto. Não localizei catraca vizinha que congele a lista exata de chamadores de `veto_de_competencia` ou de `_dia_de_turno_da_pergunta`. O delta que ela veria seria um chamador adicional do veto e a transferência da chamada do dia para `dias_das_perguntas`. A suíte completa é [nao verificado]. `bin/tests/test_mapa_juizes.sh:21–47`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:373–382`, `:470–503`, `app/chamados/tests/test_e1_v7_catracas.py:359`, `app/chamados/tests/test_e4_c20.py:30–40`, `:117–125`, `app/chamados/tests/test_e4b_c12_base_sitio_mudou.py:52–57`, `app/chamados/services/fio.py:1513`, `:1550`, `:1639`

### Achados por gravidade

1. **Média · guarda ativa.** A extração muda a exceção que escapa para uma classe de entrada. Alcance desse caso em produção [nao verificado]. `app/chamados/services/fio.py:1505–1516`, `:1545`
2. **Média · antes de ligar o coletor.** Uma falha do julgador pode deixar a instância com carimbo tentado. Há chamadores de identidade que descartam o retorno e um que informa sucesso mesmo sem gravação. `app/chamados/models.py:3430–3478`, `:2292`, `app/chamados/services/fio.py:1685–1689`
3. **Média · ramo `alvo` desligado.** A deduplicação cobre a última linha em sequência e admite duplicata em corrida. `app/chamados/reconciliador.py:525–532`, `app/core/models.py:309–316`
4. **Baixa · prova e contrato.** O selo admite ativações indiretas, a fixture herda relógio solto, e F9b permite que o motivo em memória fique velho. `app/chamados/tests/test_e4c_b2b_f0_espinha.py:538–608`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:115–128`, `app/chamados/models.py:3501–3518`

## 24/11 01:12 · sessao 6f267943b93b · NOTURNO-NAO-APURADO · leitura · origem=Code

## 1. Pareadores

A cópia tem **um núcleo de geometria**, `parear_turnos`, e **três formas relevantes de alimentá-lo**. O motor ainda pode substituir os pares que recebeu por pontas lidas da ATA. `_match_marcos` responde qual batida ocupa cada marco. Seu resultado chega ao pareamento por meio das luzes da ATA e dos papéis derivados delas. `app/ponto/turnos.py:1192–1217,1631–1638,1940–1944,1997–2002` · `app/ponto/motor_calculo_v2.py:453–506,509–531,611–714` · `app/escala/utils.py:595–676`

| Pergunta | Função de produção | Entrada, regra e saída | Quem lê |
|---|---|---|---|
| Que turno as batidas formam? | `parear_turnos` e `_mk` | Recebem batidas do chamador e ordenam por instante. Usam papel da ATA quando disponível, tipo gravado onde ele falta, marcos do DNA ou template, absorção de miolo e tetos. Uma nova E pode deixar a anterior aberta. Uma S pode fechar, ficar órfã ou ser absorvida. `Turno` carrega as batidas, as pontas, `data_turno`, `aberto` e `cross_meianoite`. `app/ponto/turnos.py:248–324,1192–1251,1304–1502` | As portas de geometria abaixo. |
| Que turno pertence ao dia do colaborador? **J** | `turnos_do_colab`, `_turnos_do_colab_puro`, `CargaDaJanela.turnos` | Batidas com `retratada_em IS NULL`, janela de dois dias antes e um depois. Segmenta pelo vínculo do dia, marca ecos de flush, lê os papéis da ATA e passa os marcos e o provedor da última batida apurável ao núcleo. Entrega turnos filtrados por `data_turno`. A porta em lote usa o mesmo corpo. `app/ponto/turnos.py:1834–1920,1923–2002,3017–3061` | Grade e cartório, realizado do espelho, núcleo materializado, consultas de dia encerrado e de par completo. `app/escala/utils.py:1590–1607,1909,2052–2072` · `app/ponto/services/cartorio.py:1180–1203` · `app/ponto/services/espelho.py:628–631,689` · `app/ponto/nucleo.py:40–70` · `app/ponto/turnos.py:2098–2105,2185–2219` |
| Que períodos vão para o cálculo? **M** | `turnos_via_autoridade`, `_periodos_via_autoridade`, `_periodos_pelo_marco` e os `montar_periodos` de `Motor12x36`, `MotorTurnoPartido`, `MotorComercial` e `MotorPlantao` | Fechamento e espelho alimentam `batidas_apuraveis`. `calcular_mes` copia e deduplica as E/S. A geometria usa `AUT_*`, template, DNA e, se houver célula alimentada, papel da ATA. `periodos_de_turnos` traduz para `(entrada, saída ou None)`, anomalias e pausas por entrada. `_periodos_pelo_marco` pode substituir os períodos do dia pelas pontas da ATA, localizadas nas batidas reais. Partido conserva a montagem por blocos. `app/ponto/services/fechamento.py:624–633,724–755` · `app/ponto/services/espelho.py:1055–1058,1147–1169` · `app/ponto/motor_calculo_v2.py:453–506,509–714,897–912,2346–2356,2739–2748,2871–2875,3211–3213,3530–3532` · `app/ponto/adaptador_motor.py:13–45` | `calcular_mes` passa cada par a `calcular_periodo`. Fechamento e espelho recebem `periodos` e `periodos_ft`. `selecao_periodo.montar_periodos_de` é outra entrada, com lista ordenada e `get_motor`, usada pelo extrato parcial para contar abertos. `app/ponto/motor_calculo_v2.py:2423–2461,2492–2494` · `app/ponto/services/fechamento.py:286–316,1200–1227` · `app/ponto/services/espelho.py:1198–1227` · `app/ponto/selecao_periodo.py:18–40` · `app/relatorios/views.py:504–514` |
| Que turno o motor consulta para dia e crédito do aberto? **I** | `MotorBase._turnos_do_juiz` → `turnos_de_batidas` | Recebe as batidas do motor, aplica a mesma deduplicação, copia, limpa os carimbos de miolo e eco e volta a marcar ecos. Usa template e mapa de DNA quando cabível. Essa chamada ao núcleo usa os valores padrão e omite papel da ATA e provedor da última batida. Entrega `Turno`. `app/ponto/motor_calculo_v2.py:867–895,2346–2349` · `app/ponto/turnos.py:1192–1194,1517–1535` | Mapa de dia do motor, crédito do aberto e agrupamento de Partido. `_dias_batidos`, `_horario_mediano` e `propostas` também chamam `turnos_de_batidas` com seus próprios insumos. `app/ponto/motor_calculo_v2.py:2396–2408,2513–2555,2921–2927` · `app/escala/services/propositor.py:109–118,139–155,522` |
| Que marco foi cumprido e por qual batida? | `_match_marcos`, `_alinhar`, `_alinhar_por_papel`, `_papeis_do_turno`, `_papeis_por_alternancia` | Trabalham com instantes locais, marcos encadeados e controle de consumo. Com papéis utilizáveis, assentam saída no último marco e pausas inteiras, em ordem e por custo. No caminho por distância, recortam candidatos pela janela externa, usam alinhamento monotônico e aplicam a guarda de cluster. Devolvem células e órfãs. O builder por turno deduplica mesmo tipo abaixo de 10 min e pode ler um turno fechado de J. No ramo intermitente há atribuição posicional E/S, com âncora e janela de 20 h. `app/escala/utils.py:216–254,330–350,437–676,1333–1348,1488–1586,1590–1607,1712–1714` | `montar_grade_prevista`, `_montar_grade_prevista_periodo_legacy`, `montar_grade_prevista_periodo_por_turno`, `marco_ocupado_pela_batida`, detector de batida fora da escala e Zona 5 do motor. O cartório filtra retratadas na entrada. A grade recebe a lista do chamador. `app/escala/utils.py:387–427,787,981–1027,2095–2101` · `app/ponto/services/cartorio.py:1184–1203` · `app/chamados/detector_anomalias.py:206–212` · `app/ponto/motor_calculo_v2.py:3052–3078` |
| Quais pontas e pausas a ATA descreve? **A** | `ponto.juiz_batida.periodos_do_dia`, `_instante_da_luz` e `papel_por_minuto_da_ata` | Leem luzes e DNA congelado. Conferem os marcos, leem `hi/hf` ou primeira/última luz posicional e devolvem pontas, pausas, papéis, órfãs, `em_aberto` e `desalinhado`. A luz tem precisão de minuto. O mapa de papéis só endereça um instante quando há batida apurável identificável e aplica as guardas de intervalo e órfãs. `app/ponto/juiz_batida.py:86–104,153–174,232–295,325–345` · `app/ponto/turnos.py:1565–1576,1599–1638,1680–1831` | M pode substituir o par por essa leitura. M e J também recebem os papéis. `_jornada_que_mente` usa o par para a média de Cadastro x Realidade. `_furo_so_da_pausa` lê as luzes e o estado das pontas para retenção na exportação. `app/ponto/motor_calculo_v2.py:494–506,611–714` · `app/ponto/turnos.py:1940–1944` · `app/escala/services/cadastro_realidade.py:215–243` · `app/folha/export.py:1116,1148–1155,1173` |
| Que trecho fechado existe apesar de um turno aberto? | `_fim_fechado`, `_borda_posicional`, `_fechar_aberto_com_saida_seguinte`, `_fechar_aberto_na_pausa_sem_volta` | `_fim_fechado` lê a saída ou acha a última S após E entre as batidas do próprio turno. `_borda_posicional` pode usar a última batida após uma pausa fechada sob as guardas da L-115. As duas pós-passagens do núcleo procuram uma saída **existente**, sujeita a teto, posição e, no caso da pausa sem volta, evidência de batida posterior. `app/ponto/turnos.py:609–622,638–704,943–1104,1106–1189,1499–1502` | `realizado_dos_turnos` usa as bordas para o realizado. `MotorBase.calcular_mes` usa I, `_fim_fechado` e `_borda_posicional` para os pares candidatos ao crédito. A pausa é lida por `_pares_marcados` e `intervalos_do_turno` com piso de 15 min. `app/ponto/turnos.py:502–546,725–766,798–888` · `app/ponto/motor_calculo_v2.py:2513–2564` |
| Há um turno aberto **vivo agora**? | `_turno_aberto_calc`, `_turno_aberto_puro`, `turno_aberto_de`, `turnos_abertos_de`, `turno_aberto_vivo` | Batidas não retratadas de hoje e dos dois dias anteriores, vínculo vigente, marcos, ecos e ATA. O núcleo monta turnos e o leitor de vivo aplica expiração. `app/ponto/turnos.py:2005–2036,2222–2241,2244–2309,2312–2381` | Presença, app e painel. `app/ponto/turnos.py:2300–2306` · `app/colaboradores/services/situacional.py:169` · `app/core/services/painel_op.py:96` |

Há outras entradas de produção para o mesmo núcleo. A falta de marcos, a janela escolhida e a falta de papel da ATA nessas entradas mudam a pergunta efetivamente feita ao núcleo. `app/ponto/turnos.py:1192–1215,1229–1251`

| Entrada de produção | Universo e uso da saída |
|---|---|
| `fila_por_causa.particionar`, `semana_do_posto._bateram`, `meio_periodo.observado_meio_periodo`, `diagnostico_escala.diagnostico_escala`, `lastro.julgar` | Chamam o núcleo sem marcos explícitos. Usam `batidas_apuraveis` ou filtro direto de retratada nula, com a janela de cada serviço. Alimentam, respectivamente, fila, semana do posto, observação de meio período, diagnóstico por dia e lastro. `app/chamados/services/fila_por_causa.py:187–207` · `app/escala/services/semana_do_posto.py:106–125` · `app/escala/services/meio_periodo.py:142–173` · `app/ponto/services/diagnostico_escala.py:126–151` · `app/ponto/services/lastro.py:257–280` |
| `detectar_bloqueio_turno_orfao`, `triagem_bater_ponto`, `_completude` de `reconciliar_fantasmas`, `pares_abaixo_do_piso` | Os três primeiros consultam histórico apurável. A triagem passa marcos do dia/template e `_completude` passa os do dia investigado. O censo de piso lê marcações não retratadas da janela, passa bordas e inspeciona pausas do turno. Os resultados servem a bloqueio, triagem, reconciliação e auditoria de pausa. `app/ponto/utils_turno_orfao.py:9–40` · `app/ponto/services/triagem_batida.py:148–194` · `app/ponto/management/commands/reconciliar_fantasmas.py:72–106` · `app/ponto/services/piso_intervalo.py:18–54` |
| `contexto_drawer` | Dentro de cada turno de J, volta às batidas e soma E→S pelo tipo gravado. Nova E sobrescreve a entrada pendente. Produz o número de presença de sete dias do drawer. `app/colaboradores/services/drawer.py:41–57,142–149` · `app/colaboradores/views.py:2992` |
| `diagnostico_regua` | Lê apuráveis, atribui a madrugada pela régua de início menos duas horas e usa primeira/última marca para medianas de pontas. Serve ao diagnóstico de cadastro e relatório de pendências. `app/escala/regua_defesa.py:118–124,151–175` · `app/ponto/services/diagnostico_escala.py:183` · `app/relatorios/pendencias.py:80` |
| `minutos_do_oraculo`, `e6_piso_autopsia.Command.handle`, `vigiar_motor_autoridade.Command.handle` | O oráculo sem `pares=` agrupa instantes por sequência e soma pares adjacentes. A autópsia lê não retratadas, deduplica por tempo, corta a sequência por gaps e deixa o grupo ímpar fora da soma. O vigia consome `turnos_via_autoridade` e volta a casar de dois em dois as marcas internas para sua métrica de pausa. A chamada do calculador de rubricas com `pares=` recebe períodos já produzidos pelo motor. `app/ponto/calculador/nucleo.py:119–141,160–254` · `app/ponto/management/commands/e6_oraculo.py:148–153` · `app/ponto/management/commands/e6_piso_autopsia.py:68–95` · `app/ponto/management/commands/vigiar_motor_autoridade.py:54–89` · `app/ponto/calculador/alimentacao.py:35–76,449` |
| `varrer_pares_embutidos` | Forma candidatos de marcações aprovadas adjacentes, com tipos alternados e limite de duração. Consulta `par_completo` sobre turno e encerramento para decidir elegibilidade à retratação. A saída é um candidato por dia para o comando. `app/ponto/management/commands/detectar_par_relampago.py:275–314,392` · `app/ponto/turnos.py:2185–2219` |

**Dos pares aos leitores.** `calcular_periodo` recebe as pontas, aplica a janela e escreve as rubricas. `DiaPago.lavrar` recebe os períodos e o mapa do motor para as rubricas, inclusive noturno, e recebe `minutos_realizados` pelo mapa da grade. O TXT monta eventos de `FechamentoMensal`. A retenção de furo tem a leitura específica da ATA indicada acima. `app/ponto/motor_calculo_v2.py:2457–2461,2750–2792` · `app/ponto/services/dia_pago.py:173–186,223–240,285–292,364–399` · `app/ponto/services/fechamento.py:918–924,1200–1217` · `app/folha/export.py:715–745,1148–1155`

**Da saída aos relatos.** `janela_he.marcar_pontas_fora` lê as extremidades da grade. `pontas_do_relato` lê `dias_cadastro_x_realidade` publicado pelo motor e coloca no máximo uma ponta por sentido no espelho, incluindo a que já veio da grade. Gestão de HE e portão leem essa projeção. `cxr_na_lista.medir` usa o relato do motor via espelho, filtra pela competência e deduplica o item inteiro. `_cxr_do_motor` formata cada item da lavra como C12. Esses leitores recebem pontas ou itens de relato. `app/ponto/janela_he.py:191–231,267–331` · `app/ponto/services/espelho.py:734–756` · `app/ponto/services/he_pendente_lavrado.py:55–68,124–144` · `app/folha/porta_export.py:172–188` · `app/ponto/services/cxr_na_lista.py:76–110,142–152` · `app/escala/services/cadastro_realidade.py:360–379,949–952`

## 2. Onde divergem

| Classe | Condição verificável para os caminhos que respondem pelo par |
|---|---|
| **Entrada repetida** | **M × J:** M elimina E/S consecutivas de mesmo tipo abaixo de três minutos. J marca eco de flush, com condições sobre `criado_em` e o instante da batida. Sem absorção nem eco, o núcleo abre na segunda E e deixa a primeira aberta. **M × I:** ambos deduplicam, mas I marca eco e M pode receber papel de ATA. **A × geometria:** a grade tem dedup abaixo de dez minutos e pode assentar uma E e deixar a outra órfã. O drawer usa a última E pendente dentro do turno recebido. `app/ponto/motor_calculo_v2.py:70,736,897–912` · `app/ponto/turnos.py:344–375,1398–1421,1517–1535,1940–1944` · `app/escala/utils.py:635–676,1333–1348` · `app/colaboradores/services/drawer.py:47–54` |
| **Saída após o marco** | **A × M/J/I:** uma S pode estar fora da janela de candidatos do alinhamento por distância e ainda fechar no núcleo. A grade pode recebê-la pelo papel do turno fechado ou na reconciliação de ponta órfã com célula `missing`, sujeita às guardas. **M × J/I:** o par bruto pode coincidir e a saída efetiva da rubrica ser o marco. São dois campos, `saida` e `efetivo_saida`. `app/escala/utils.py:568–592,635–643,1909–1937,1985–2051` · `app/ponto/turnos.py:1430–1456,1487–1502` · `app/ponto/motor_calculo_v2.py:1714–1763,1834–1842,2750–2765` |
| **Batida fora da célula ou com papel diferente** | **A × núcleo:** luz faltante ou ATA desalinhada pode deixar A sem o par que a sequência fecha. **M × J:** J carrega a ATA no seu envelope. M só a alimenta quando tem células e pode substituir o par inteiro se encontrar as pontas reais e a contagem de batidas dentro do par couber nas luzes. **I × M/J:** I omite esse mapa de papéis. No núcleo, `X/Xi` pode consumir uma batida como miolo onde o tipo gravado abriria ou fecharia. `app/ponto/juiz_batida.py:232–271` · `app/ponto/motor_calculo_v2.py:494–506,611–688,867–882` · `app/ponto/turnos.py:1310–1326,1371–1401,1533–1535,1940–1944` |
| **Aberto / sem par** | **J × M/I:** J passa a última batida apurável do colaborador. Uma S marcada como pausa e sem volta pode fechar se houver batida posterior além do recorte local, respeitado o teto. M e I fazem essa passagem com as batidas da chamada. O adaptador de M devolve S sem entrada como anomalia e E sem saída como período aberto. O realizado e o crédito podem achar trecho fechado dentro de um aberto. `app/ponto/turnos.py:1106–1189,1497–1502,1533–1535,1903–1908,1997–2000,725–759` · `app/ponto/motor_calculo_v2.py:503–506,2513–2555` · `app/ponto/adaptador_motor.py:24–44` |
| **Virada / outro dia** | J agrupa por `data_turno` pelo envelope da véspera e janela por vínculo. I usa as batidas já carregadas no motor. M pode encadear pelo fim do período anterior em até seis horas ou usar data civil na ausência de entrada no mapa. A encadeia os marcos e reconstrói a data da luz. As pontas podem ser idênticas e aparecer em dias diferentes. `app/ponto/turnos.py:248–324,1834–1869,1961–2002` · `app/ponto/motor_calculo_v2.py:884–895,2396–2432,2921–2942` · `app/ponto/juiz_batida.py:86–104,210–246` |
| **Pausa / quantidade de blocos** | **M × I/J/A:** Partido passa teto de duração zero para pausa e remove os marcos de intervalo da alimentação geométrica. Plantão passa 8 h de pausa e 26 h de continuidade. I e J chamam o núcleo com os padrões de 3 h e 14 h. J e M podem consumir uma pausa declarada por `Xi`, e M pode ainda substituir pelos intervalos da ATA quando o regime permite. Os limites de continuidade, o teto de fechamento e o piso de pausa gozada têm funções distintas. `app/ponto/motor_calculo_v2.py:467–506,558–570,689–714,728–730,2860–2861,3512–3513` · `app/ponto/turnos.py:502–546,1192–1199,1316–1326,1430–1447,1535,1997–1999` |

**Q-B, 06:00 / 06:30 / 19:00.**

1. **Entrada do motor.** O anexo informa `06:00 E, 06:30 E, 19:00 S`. O dedup de M preserva as duas E a 30 minutos. No ramo sequencial sem carimbo que as absorva, `06:00` abre, `06:30` torna `06:00` aberto e abre outro turno, e `19:00` fecha o segundo se passar as guardas. Os papéis, `criado_em`, flags de intervalo, DNA e teto aplicados ao caso estão **[nao verificado]**. `qb_linha.txt:1` · `app/ponto/motor_calculo_v2.py:897–912` · `app/ponto/turnos.py:1304–1421,1430–1502`
2. **Caminho do juiz.** O anexo informa `06:30→20:00` como par do juiz. Com apenas os três instantes listados, o núcleo só pode devolver uma saída que esteja entre as batidas recebidas. As pós-passagens também usam batidas existentes. `_match_marcos` entrega a hora de uma marca atribuída e a ATA pode guardar uma luz `20:00`, mas a substituição por A em M exige localizar a batida correspondente. A origem de `20:00` e a chamada que efetivamente o entregou estão **[nao verificado]**. `qb_linha.txt:1` · `app/ponto/turnos.py:1035–1104,1170–1188,1487–1502` · `app/escala/utils.py:625–631,666–676` · `app/ponto/juiz_batida.py:240–264` · `app/ponto/motor_calculo_v2.py:596–609,636–645`
3. **Janela e publicação.** O item Q1 do `verif6.r` usa os pares `06:30→19:00` e `06:30→20:00`, com marco `08:00–17:00`. Os dois excessos de saída são `120` e `180` minutos. Com a janela nas condições do caso, o motor publica a saída real, o marco cru e os minutos excedentes. O anexo da volta 6 registra os dois itens e uma cauda adicional de 60 minutos fora da janela. Na cópia, o laço do aberto pula a entrada já paga inteira em `_pagos_a`, de modo que esse resultado da volta 6 não é reproduzível por esse ramo da cópia para a mesma entrada. `verif6.r:13` · `qb_linha.txt:1` · `app/ponto/janela_he.py:117–153` · `app/ponto/motor_calculo_v2.py:1669–1674,1731–1750,2525–2555`
4. **Dois itens, uma saída exibida.** Os itens `19:00/120` e `20:00/180` são distintos na deduplicação inteira da CxR. Na Gestão de HE, se a grade já marcou uma saída, o tradutor pula o sentido. Se a ponta estiver `missing`, o primeiro item do dia que passa as guardas ocupa o sentido `depois` e o próximo é pulado. O anexo informa a saída exibida `19:00/120`. Qual dos dois ramos da tela ocorreu no nominal está **[nao verificado]**. `qb_linha.txt:1` · `app/ponto/services/cxr_na_lista.py:81–110` · `app/escala/services/cadastro_realidade.py:360–379` · `app/ponto/janela_he.py:267–295,311–331`

**O “420 × 480”.** Há duas ocorrências relevantes no material da volta 6, ambas depois de escolhidas as pontas. `verif6.r:11,21–26` · `triagem_v6.txt:1–5`

| Caso | Caminho do fechado e caminho do candidato |
|---|---|
| **Q1, noturno após 05:00** | Fechado em `05:30→06:30`, em `periodos_ft`, leva `60` trabalhados e `0` noturno. Candidato `23:00→07:00` com prorrogação tem `480` minutos noturnos de relógio. O crédito da volta 6 fica com `420` **trabalhados** e `480` **noturnos de relógio**, ante `420/420`. Os `60` já trabalhados no fechado são descontados do trabalho do candidato. Como o fechado sozinho não trabalhou no segmento `00:00–05:00`, esses `60` não são cobertura noturna do fechado. O fator `60/52,5` dá `548,57` minutos legais na régua do teste. `verif6.r:11` · `app/ponto/motor_calculo_v2.py:46,82–118,969–1005` |
| **Q2, pausas sobrepostas** | Fechado e candidato têm as mesmas pontas `19:00→03:00`. O fechado tem duas pausas injetadas, `23:00→00:00` e `23:15→23:45`. A soma de pausas tira `90` de `480` e o fechado conta `390`. O leitor de cobertura da volta 6 tira a união de `60` e reconstrói `420`. O candidato sem pausa vale `480` integralmente, ganha `60` no crédito e a soma sai `450`. A triagem identifica reconstrução paralela do que já foi pago. `verif6.r:21–26` · `triagem_v6.txt:1–5` · `app/ponto/turnos.py:542–546,798–888` · `app/ponto/motor_calculo_v2.py:2771–2785` |

No Q1, a evidência fornecida começa com o fechado e o candidato já montado

[truncado em 20000 de 39707 caracteres]

## 24/11 00:55 · sessao 568f2d7f75e9 · NOTURNO-NAO-APURADO · verificador · origem=Code

**REPROVA — DENTRO do contrato da CAUDA e da L-097.** A volta 6 corrige os três exemplos literais de minutos da volta 5. Ainda duplica **30 minutos fora da janela** quando o recorte de saída inverte o par e pode creditar **60 minutos noturnos** quando o desconto do trabalhado faz a L-097 retornar antes do bloqueio. Reproduzi os dois casos em memória, com os pares e o cadastro injetados e as funções de cálculo da cópia. `app/ponto/motor_calculo_v2.py:266–281,1548–1565,2752–2787`

Os três arquivos alterados conferem com os hashes de arquivo do diff fornecido. Identidade dos commits, suíte Django, mutantes e incidência em produção **[nao verificado]**. [nao verificado]

## Q1 — Achados literais da volta 5

| Achado | Resultado da volta 6 |
|---|---|
| **DENTRO — envelope bruto do fechado** | Fechado **17h30–18h30**, marco **19h**, candidato **17h–23h**. O fechado grava **90 fora**. O aberto cai de **60 para 30 fora**, com **240 trabalhados e 60 noturnos**. O total fora cai de **150 para 120**. O RED agora confere o fechado, o aberto e a soma. `app/ponto/motor_calculo_v2.py:273–278,2726–2734,2755–2766` · `app/ponto/tests/test_noturno_credito_turno_aberto.py:440–466` |
| **DENTRO — pausa no fechado** | Fechado **19h–03h**, pausa **23h–00h**, candidato **19h–04h** sem pausa. O fechado leva **420 trabalhados / 240 noturnos**. O aberto sobe de **60/60 para 120/120**. Totais **540/360**. O RED usa a pausa literal. `app/ponto/motor_calculo_v2.py:268–281,2755–2764` · `app/ponto/tests/test_noturno_credito_turno_aberto.py:998–1027` |
| **DENTRO — noturno após 05h** | Fechado **05h30–06h30** com **60 trabalhados / 0 noturno**, candidato **23h–07h** com prorrogação. O aberto leva **420 trabalhados / 480 noturnos de relógio**, ante **420/420**. São **548,57 minutos legais** na régua do teste. O fechado está em `periodos_ft`. `app/ponto/motor_calculo_v2.py:99–118,2726–2731,2758–2762` · `app/ponto/tests/test_noturno_credito_turno_aberto.py:934–963` |
| **DENTRO — flag após exceção** | No caso em que `calcular_periodo` levanta durante a sobreposição, `RuntimeError` propaga e o `finally` deixa a flag em `False` e o dia em `None`. O RED exige a exceção e a flag desarmada. `app/ponto/motor_calculo_v2.py:2744–2750` · `app/ponto/tests/test_noturno_credito_turno_aberto.py:496–516` |
| **DENTRO — relato repetido e fato novo** | Com marco **08h–17h**, fechado **06h30–19h** e candidato **06h30–20h**, saem **uma chegada 06h30/90** e **duas saídas, 19h/120 e 20h/180**. A cauda acrescenta **60 fora**. No caso L-084 com fechado **10h30–14h** e candidato **04h30–13h**, fica **um** relato `l084_do_dia`, com as bordas **04h30–22h**. No reuso do motor, o piso acumula **1 → 2** quando o segundo mês de cálculo encontra o fato pela primeira vez no candidato. `app/ponto/motor_calculo_v2.py:1722–1736,1782–1810,1963–1967,2131–2149` · `app/ponto/tests/test_noturno_credito_turno_aberto.py:743–775,877–900` |

Os REDs da cauda comercial **180**, do 12x36 diurno **240 de 420**, da cauda pós-05h **90 / 102,86** e da virada autorizada/bloqueada continuam presentes. **DENTRO.** `app/ponto/tests/test_noturno_credito_turno_aberto.py:251–310,417–438,675–695,817–852`

## Q2 — O que o fechado contou

`W`, `F` e `N` são reconstruídos no leitor. Os três `calcular_periodo` concretos carimbam pontas efetivas e intervalos antes da apuração das rubricas, inclusive em período sem HE. O retorno anterior ao carimbo é o de saída ausente, marcado `turno_aberto` e excluído da cobertura. Intermitente herda 12x36, plantão delega ao 12x36 e 12x36 com escala chama `super()`. A virada conserva os instantes completos. Pausa julgada fora do horário literal chega em `intervalos_do_motor` após o recorte. `intra_descontada=0` significa desconto zero em `W`. `N` desconta a pausa julgada mesmo nesse caso. O ramo novo que devolve três listas vazias por carimbo ausente existe, mas um fechado concreto que chegue ali com minutos pagos é **[nao verificado]**. `app/ponto/motor_calculo_v2.py:266–281,330–337,1973–1981,2020–2028,2726–2729,2978–3020,3105–3125,3443–3488,3762–3785,3797–3809` · `app/ponto/turnos.py:798–852`

| Achado | Resultado da sonda em memória |
|---|---|
| **DENTRO — 2× fora da janela pelo recorte de saída** | Marco **08h–17h**, janela de saída ligada, fechado **17h30–18h30**, candidato **16h–19h**. O fechado grava **90 fora** de **17h a 18h30**. Como a saída efetiva é **17h** e a entrada efetiva **17h30**, o `max` da reconstrução deixa só **60** em `F`. O aberto leva **60 fora**. Total **150 para 120**, com **17h–17h30** contado duas vezes. `app/ponto/janela_he.py:143–153` · `app/ponto/motor_calculo_v2.py:273–278,2757–2766,3114–3124` |
| **DENTRO — 60 noturnos passam pela L-097** | 12x36 fechado em **D 23h–D+1 04h**, pausa julgada **01h–02h**. Por ter **300 minutos brutos**, o fechado grava **300 trabalhados / 240 noturnos**. O candidato **D+1 00h–04h** sem pausa, em feriado com previsto zero, janela ligada e sem autorização, teria **240/240** antes da guarda. A cobertura zera os **240 trabalhados** antes da L-097. A guarda retorna na primeira condição e o aberto leva **0 trabalhado / 0 fora / 60 noturno**, sem pendência. A mesma guarda chamada sobre o par integral devolve **0 / 240 / 0**. `app/ponto/motor_calculo_v2.py:1548–1565,1579–1596,2755–2787,2987–3020` |
| **DENTRO — faltam 30 trabalhados na soma com pausas sobrepostas** | Fechado **19h–03h**, com pausas julgadas **23h–00h** e **23h15–23h45**. Com marcas distintas injetadas, `intervalos_do_turno` devolve ambas. `_intra_real` soma **90** e o fechado grava **390 trabalhados**. `W` tira a união de **60** e mede **420**. Para candidato **19h–03h** sem pausa, o aberto leva **60** e o total fica **450 para 480**. A divergência do leitor é **DENTRO**. Alterar o julgamento das pausas do fechado é **FORA**. `app/ponto/turnos.py:542–546,798–852` · `app/ponto/motor_calculo_v2.py:217–231,268–272,2048–2051,2755–2764,2993–3013` |
| **DENTRO — `N` perde a multiplicidade do valor que lê** | Com prorrogação, fechado **D 23h–D+1 23h** grava **1.500 noturnos**, enquanto `N` mede **1.440**. O sítio original soma **D+1 22h–23h** na prorrogação e na janela das 22h. Candidato **D 22h–D+1 23h** grava **1.560**. O aberto leva **120 noturnos**, e a soma chega a **1.620**, 60 acima até do valor integral do candidato. A divergência da cobertura é **DENTRO**. Mudar a régua original de períodos longos é **FORA**. `app/ponto/motor_calculo_v2.py:93–118,189–214,279–281,1070–1074,1100–1113,2758–2762` |

O delta também toma `W ∪ F` como cobertura de trabalho e de fora, e `N ∪ F` como cobertura de noturno. Assim, **`F` comum também vence um `W` ou `N` do candidato**. **FORA**, como regra geral de pagamento a decidir. Quando os pares recebem vereditos diferentes, um recorte ordinário registrado em `F` deve impedir o crédito de trabalho ou adicional reconhecido no candidato? Aval específico para essa precedência **[nao verificado]**. `app/ponto/motor_calculo_v2.py:313–319,2724–2731,2760–2762` · `app/docs/LEIS.md:115`

## Q3 — Registro e dia do relato

- **DENTRO — uma chave, datas com sentidos diferentes.** `_relato_ja_publicado_no_dia` registra e consulta `(tipo, _dia_do_par_em_calculo, *conteudo)`. O laço regular fornece `data_jornada`, que pode ser civil por ausência no mapa ou herdada do período anterior pelo intervalo de até 6h. O crédito fornece `_d_a`, o dia do grupo do aberto, e seleciona turnos com `t.data_turno == _d_a`. No relato publicado, as pontas da janela usam o dia civil da entrada, `_janela_do_dia` publica `_dt_b` na L-084 e `aplicar_tolerancia` publica o dia do mapa, que pode ser `None`. O piso consulta previsto em `quando` e registra pela jornada em curso. `app/ponto/motor_calculo_v2.py:971–982,1634–1637,1690–1692,1725–1736,1754–1789,1805–1810,1963–1967,2131–2149,2548–2557,2607–2612,2667–2678,2744–2750`
- **DENTRO — fato de outra data civil pode sumir.** Na sonda, fechado **D 23h–D+1 09h** e candidato **D+1 00h–09h** receberam o mesmo dia de turno **D**, com marco de saída **07h**. Os dois fatos são **09h/120**, mas a janela do fechado publica **D** e a do candidato publicaria **D+1**. Saiu só o de **D**. A chave omite o `dia` do próprio item e a lavra usa esse campo para atribuir data e competência. `app/ponto/motor_calculo_v2.py:1636–1637,1754–1758,1782–1789,1805–1810,1880–1887` · `app/ponto/services/cxr_na_lista.py:81–110`
- **DENTRO — exceção coberta para o par, aninhamento sem restauração.** O `finally` limpa o dia no laço regular e limpa dia e flag no crédito. O set parcial fica na instância até a próxima entrada em `calcular_mes`. Com dia `None`, a chave deixa passar o relato. Um `calcular_mes` aninhado na mesma instância substituiria set, lista e contexto externos, pois a entrada reinicializa e os `finally` atribuem valores fixos. Caminho concreto de reentrada em produção **[nao verificado]**. `_piso_sem_previsto_dias` perdeu seu único leitor funcional. `_piso_sem_previsto` segue acumulado e lido pelos testes. `app/ponto/motor_calculo_v2.py:935–937,1782–1789,1963–1967,2425–2432,2607–2612,2744–2750` · `app/ponto/tests/test_e3_metade_intervalo_cadastrado.py:101–103` · `app/ponto/tests/test_noturno_credito_turno_aberto.py:877–900`
- **FORA — contexto emprestado do calculador.** `_ContextoPontualidade` tem o método emprestado e omite o registro. O `getattr` deixa cada ocorrência L-084 chegar ao `append`. Dois pares que disparem a mesma acusação podem produzir dois itens nessa lista, comportamento já presente fora do crédito. Durante o crédito, o motor inicializa o registro antes dos pares. `app/ponto/calculador/regras.py:112–120,195,264–275` · `app/ponto/motor_calculo_v2.py:2127–2149,2425–2431`

## Q4 — Soma e teto fora do crédito

**FORA do crédito — igualdade matemática, diferença no `float`.** A união, o corte por hora local e o teto `min(total, minutos_noturnos(...))` são os mesmos. A junção de trechos adjacentes muda a ordem da soma. No recorte **22h–05h**, com pausa **22:00:01–00:00:08**, a volta 5 dá **120,11666666666667** e a 6 dá **120,11666666666666**. O teto é **420** nas duas. Efeito em valor lavrado **[nao verificado]**. `app/ponto/motor_calculo_v2.py:162–214` · `app/ponto/services/fechamento.py:941`

Os chamadores diretos são `he_noturna` e `MotorBase._noturno_dos_intervalos`. Pelo primeiro passam o split do motor e o split do calculador. Pelo segundo passam `calcular_noturnas` e `noturnas_do_dia` via `_ContextoNoturno`. Há chamadas diretas na bateria P5. `app/ponto/motor_calculo_v2.py:527,1070–1074,1122–1126,2053–2067` · `app/ponto/calculador/regras.py:60–86,824–833` · `app/ponto/calculador/periodo.py:488–512` · `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:105–108,140–142,194–197,409,471–476`

## Q5 — Classes de reprovação e guarda

- **DENTRO — derivação paralela e dois sentidos de cobertura.** `F` e `W` têm uma montagem no leitor do fechado e outra no candidato. A saída invertida mostra a diferença entre a rubrica gravada e o que o leitor afirma que ela cobriu. O delta também usa `F` tanto como marca de fora quanto como veto de adicional. O selo AST de 11 ajudantes verifica classe e quantidade de métodos. Os REDs novos têm asserções numéricas, mas seus exemplos de `F`, pausa e `N` deixam passar os contraexemplos da Q2. `app/ponto/motor_calculo_v2.py:266–281,2730–2731,2755–2766` · `app/ponto/tests/test_contract_juiz_batida.py:132–143,324–342` · `app/ponto/tests/test_noturno_credito_turno_aberto.py:117–131,440–493,934–1027`
- **DENTRO — autoridade, escrita e fallback.** A geometria continua vindo dos leitores do juiz de turno, o valor do par passa por `calcular_periodo` e a atribuição direta das duas rubricas noturnas fica em `_escrever_noturnas`. O zeramento dinâmico da L-097 e as atribuições residuais de trabalhado e fora já aparecem na base do diff. No delta, o tratamento de exceção é `finally`; o `except` da publicação já existia. O relógio usado pelos trechos vem dos timestamps. O fallback novo é cobertura vazia quando falta ponta efetiva, com efeito financeiro em fechado **[nao verificado]**. `app/core/juizes.py:1193–1208` · `app/ponto/motor_calculo_v2.py:197–208,266–281,1100–1113,1579–1591,1805–1812,2672–2701,2744–2766`
- **DENTRO — entrada da guarda de 06–08.** As chaves entram no ramo existente e a entrega é decidida por K6. **FORA — ordem de saída herdada da volta 5.** Para 06–08, o texto manda retirar a chave antes da REGEN. Para 09, exige DIFF, reversão e Pauta antes da retirada no commit do apply. O delta fornecido conserva essa diferença. Aval para mudar a ordem **[nao verificado]**. `app/ponto/services/fechamento.py:34–63,583–596,1538–1560` · `app/ponto/services/dia_pago.py:449–460` · `app/ponto/management/commands/recalcular_fechamento.py:123–126`

## Q6 — Escritores e leitores

| Campo ou função | Escritores e leitores |
|---|---|
| `entrada`, `saida`, `turno_aberto`, `periodos`, `periodos_ft`, mapas e intervalos de entrada | Adaptador e montagem entregam pares e pausas. Os três cálculos criam as pontas e a marca de aberto. `_dia_do_turno` monta o mapa e `calcular_mes` guarda o mapa, distribui as listas e lê ambos no crédito. `_borda_posicional`, `_fim_fechado` e `intervalos_do_turno` fornecem a geometria. Espelho e lavra leem os períodos. `app/ponto/adaptador_motor.py:30–45` · `app/ponto/motor_calculo_v2.py:769–808,971–982,2544–2548,2645–2647,2667–2701,2727–2734,2978–2983,3105–3110,3443–3448` · `app/ponto/turnos.py:502–546,609–622,638–693` · `app/ponto/services/espelho.py:81–103` · `app/ponto/services/dia_pago.py:173–184,223–240` |
| `efetivo_entrada`, `efetivo_saida`, `intervalos_do_motor`, `intra_descontada` | `_carimbar_segmento` grava os três primeiros; a L-097 pode colapsar a saída. 12x36 e comercial gravam o desconto. O crédito, o split e o calculador leem os carimbos. O recálculo por período do calculador também escreve seu `intra_descontada`. `app/ponto/motor_calculo_v2.py:1122–1126,1590–1591,1973–1981,266–280,2732–2734,2752–2759,3003,3472` · `app/ponto/calculador/alimentacao.py:83–138` · `app/ponto/calculador/periodo.py:647–650,744–756` |
| Helpers de trechos e `prorrogacao_noturna_pos5h` | `trechos_noturnos_dos_intervalos` gera as peças para o somador, `trechos_contados_do_periodo` e o candidato. `trechos_menos` gera diferenças; `trechos_dentro` lê a diferença para intersectar; `trechos_minutos` mede as interseções no crédito. O construtor grava a prorrogação recebida da régua ou resolvida pelo ciclo. Leem a porta noturna, o desconto de pausa, o split e o crédito. `_confere_contados` lê os três trechos no teste. `app/ponto/motor_calculo_v2.py:162–281,843–847,1070–1074,1122–1126,2066–2067,2726–2734,2755–2762` · `app/core/regua_cct.py:449–466` · `app/ponto/tests/test_noturno_credito_turno_aberto.py:113–131` |
| Minutos trabalhados, fora e noturnos; alertas e contadores | Os três cálculos gravam trabalhado e fora e chamam a porta noturna. A L-097 transfere e zera rubricas, grava alerta e pendência. `calcular_mes` ajusta o candidato, soma e escreve o portador, manda os pares com descontos à porta noturna e publica alerta e contadores. Leem o teto L-093, o resumo mensal, o split por dia, o fechamento, a lavra, a exportação e o espelho. `app/ponto/motor_calculo_v2.py:380–406,1081–1113,1294,1579–1596,2664–2666,2741,2763–2792,2947–2948,3013–3020,3121–3125,3364–3369,3484–3488` · `app/ponto/services/fechamento.py:941` · `app/ponto/services/dia_pago.py:173–184,223–240` · `app/folha/porta_export.py:105,544` · `app/ponto/services/espelho.py:95–103,757–763` |
| Registro, dia, flag, piso e listas de relato | O construtor e `calcular_mes` iniciam o contexto; os dois laços põem o dia e o crédito arma a flag. O helper lê e grava a chave. Chamam a janela L-084, as acusações de chegada/saída, a pontualidade e o piso. A janela também grava `_l084_do_dia_acusado` e `_janela_fora_por_l084`. O motor publica a lista; fechamento, espelho, tradução da janela, lavra e comandos de diff a leem. `app/ponto/motor_calculo_v2.py:935–937,1674–1679,1715–1736,1782–1812,1847–1849,1885–1887,1963–1967,2127–2149,2425–2431,2451,2546,2607–2612,2744–2750,2947` · `app/ponto/services/fechamento.py:115–117` · `app/ponto/services/espelho.py:754–756,1296` · `app/ponto/janela_he.py:277–315` · `app/ponto/services/cxr_na_lista.py:79–110` · `app/ponto/management/commands/diff_janela_he.py:70` · `app/ponto/management/commands/diff_reclassificar_partido.py:188` |
| Guarda, metadados e trilha | `fechamento.py` define as chaves e os metadados. Recálculo e retrato leem a guarda e escrevem a trilha de bloqueio. Recálculo e CLI divulgam `saida`; `barrados_pela_guarda` conta eventos. O selo lê os metadados. `app/ponto/services/fechamento.py:45–71,583–598` · `app/ponto/services/dia_pago.py:449–460` · `app/ponto/management/commands/recalcular_fechamento.py:123–126` · `app/ponto/tests/test_noturno_item1_guarda_exportada.py:94–98,127–135` |

## 24/11 00:43 · sessao f73cc7ae2c55 · RESOLVER-PASSA-NA-FRENTE · sonda · origem=Code

```text
sonda_leitura: sonda_fk2.py [def639c0d9aeadc2] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO respondidas q_vivas conversa=False: 1491 erros_c6: 0
morta_c6 145
('fk_do_dia', 'cel_dia_nao_admin') 1336
('sem_fk', 'cel_dia_nao_admin') 10
```

O dado fornecido registra `erros_c6: 0`. Execução da sonda [nao verificado].

- `morta_c6` — As 145 estão no grupo das mortas. O degrau que encerrou cada uma é [nao verificado].
- `('fk_do_dia', 'cel_dia_nao_admin')` — **Degrau 1, resposta do colaborador.** Sem decisão de admin na célula vinculada, a pergunta segue viva quando `_resposta_concorda_com_a_luz(p)` retorna `False` ou `None`. O retorno individual é [nao verificado]. [app/chamados/models.py:3689](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/models.py:3689), [app/chamados/juizes.py:1511](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1511), [app/chamados/juizes.py:1524](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1524).
- `('sem_fk', 'cel_dia_nao_admin')` — **Degrau 1, resposta do colaborador.** Sem `p.celula`, a leitura do marco retorna `None`, a concordância retorna `None` e a pergunta segue viva. [app/chamados/juizes.py:993](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:993), [app/chamados/juizes.py:1435](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1435), [app/chamados/juizes.py:1524](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1524).

## 24/11 00:40 · sessao af828c423230 · RESOLVER-PASSA-NA-FRENTE · sonda · origem=Code

## 1. Saída da sonda

Transcrição do dado fornecido. `erros_c6` é **zero**. Execução em produção [nao verificado].

```text
sonda_leitura: sonda_semfk.py [dcb9515faf345b5d] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO q_vivas conversa=False: 16148 erros_c6: 0
morta_c6 3675
('respondida', 'cel_do_dia_existe') 10
('respondida', 'fk') 1336
('sem_resposta', 'cel_do_dia_existe') 19
('sem_resposta', 'fk') 11108
```

## 2. Resultado no C6 depois do Resolver dia

O Resolver dia busca a célula por colaborador e data. Filtra as perguntas pelo C6 antes de carimbar `admin_decidiu` junto com `materializada_em`. Em seguida, se encontrou a célula, lavra o veredito com via `admin`. A gravação do carimbo deixa a FK como estava. [app/ponto/services/veredito_celula.py:30](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/ponto/services/veredito_celula.py:30) · [app/ponto/services/veredito_celula.py:193](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/ponto/services/veredito_celula.py:193) · [app/chamados/models.py:3347](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/models.py:3347) · [app/chamados/models.py:3496](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/models.py:3496) · [app/ponto/services/veredito_celula.py:352](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/ponto/services/veredito_celula.py:352)

| Grupo | Resultado numa nova leitura do C6 |
|---|---|
| `('respondida', 'cel_do_dia_existe')` | **Viva**, se o C6 reconhecer a resposta e a FK continuar nula. O C6 procura o veredito em `p.celula`. Com a FK nula, a checagem do marco devolve `None` e o ramo da resposta retorna `True` antes de chegar ao carimbo. [app/chamados/juizes.py:1511](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1511) · [app/chamados/juizes.py:1524](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1524) · [app/chamados/juizes.py:1435](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1435) · [app/chamados/juizes.py:993](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:993) |
| `('respondida', 'fk')` | **Morta** se a FK aponta para a célula que recebeu o veredito `admin`. O veredito faz o C6 passar pelo ramo da resposta e chegar ao encerramento por identidade. A correspondência entre a FK e a célula lavrada para cada registro está [nao verificado]. [app/chamados/juizes.py:1511](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1511) · [app/chamados/juizes.py:1524](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1524) · [app/chamados/juizes.py:1536](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1536) |
| `('sem_resposta', 'cel_do_dia_existe')` | **Morta** se o C6 a lê como sem resposta. `admin_decidiu` com `materializada_em` encerra por identidade, mesmo com FK nula. [app/chamados/juizes.py:1516](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1516) · [app/chamados/juizes.py:1536](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1536) · [app/chamados/models.py:2782](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/models.py:2782) |
| `('sem_resposta', 'fk')` | **Morta** se o C6 a lê como sem resposta. O encerramento por identidade vem antes da avaliação do veredito da célula. [app/chamados/juizes.py:1536](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1536) · [app/chamados/juizes.py:1546](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1546) |

A mesma regra vale no caso sem FK e sem célula no dia, se o carimbo for gravado. Com resposta reconhecida pelo C6, fica **viva**. Sem resposta, fica **morta**. Contagens explícitas desses grupos na sonda [nao verificado]. [app/chamados/juizes.py:1511](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1511) · [app/chamados/juizes.py:1524](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1524) · [app/chamados/juizes.py:1536](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1536)

A sonda agrupa pela coluna `respondida_em` e pelo texto via SQL. O C6 lê o instante da sequência de decisões e o texto da coluna. A saída traz as contagens da leitura inicial. A correspondência desses estados por registro e o resultado após o Resolver dia em produção ficam [nao verificado]. [app/chamados/juizes.py:1671](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1671) · [app/chamados/juizes.py:1494](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/juizes.py:1494) · [app/chamados/models.py:4449](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/chamados/models.py:4449)

## 24/11 00:32 · sessao b9c5fc6eacb3 · A-TELA-DO-DIA-LE-UM-JUIZ · sonda · origem=Code

## 1. RESUMO

As referências `DADO RASPADO:n` numeram as linhas JSON do texto fornecido, a partir do primeiro `d1`. A linha `RESUMO` é a 404. As contagens abaixo são as da sonda. [DADO RASPADO:404]

| Chave | Valor |
|---|---|
| `RESUMO` | `true` |
| `agora` | `2026-11-24 00:15:56.989532-03:00` |
| `colaboradores` | 662 |
| `dias_com_celula` | 18.395 |
| `janelas` | `2026-10-21` a `2026-11-20` — 662 colaboradores |
| `ausencia_varios_dias` | `dias` = 9, `rotulo_difere_do_rateio` = 9 |

**`cruz_texto_aberto_x_status_pede_decisao_x_oferta`** [DADO RASPADO:404]

| Texto aberto | Status pede decisão | Oferta | Dias |
|---|---|---|---:|
| `false` | `false` | `false` | 15.996 |
| `true` | `true` | `true` | 1.096 |
| `false` | `true` | `true` | 575 |
| `true` | `false` | `false` | 394 |
| `false` | `true` | `false` | 213 |
| `true` | `true` | `false` | 121 |

**`D1_texto_aberto_status_nao_pede_por_status_e_ramo`** [DADO RASPADO:404]

| Status | Ramo | Dias |
|---|---|---:|
| `escala_suspeita` | `furo_apurado` | 244 |
| `ok` | `turno_aberto` | 123 |
| `ok` | `saida_orfa` | 18 |
| `escala_suspeita` | `saida_orfa` | 5 |
| `ok` | `furo_apurado` | 3 |
| `folga_trabalhada` | `turno_aberto` | 1 |

**`D1_fonte_do_d4`** [DADO RASPADO:404]

| Status | Fonte | Terceiro valor da tupla | Dias |
|---|---|---|---:|
| `escala_suspeita` | `lavrado` | `true` | 249 |
| `ok` | `lavrado` | `true` | 144 |
| `folga_trabalhada` | `lavrado` | `true` | 1 |

**`D1_veredito_da_celula`** [DADO RASPADO:404]

| Status | Veredito da célula | Via | Dias |
|---|---|---|---:|
| `escala_suspeita` | `indefinida` | `cartorio` | 249 |
| `ok` | `concorde` | `cartorio` | 144 |
| `folga_trabalhada` | `concorde` | `cartorio` | 1 |

**`D2_status_pede_texto_nao_aberto`** [DADO RASPADO:404]

| Status | `veredito_dia` | Dias |
|---|---|---:|
| `irregular` | `trabalhou` | 641 |
| `falta` | `descontado` | 73 |
| `irregular` | `rejeitado` | 32 |
| `irregular` | `abonado` | 29 |
| `falta` | `abonado` | 5 |
| `irregular` | `folga` | 5 |
| `irregular` | `feriado` | 1 |
| `sem_certidao` | `trabalhou` | 1 |
| `irregular` | `descontado` | 1 |

**`cruz_completo_veredito_status_oferta_ultimo_recurso`** [DADO RASPADO:404]

| `veredito_dia` | Status | Oferta | Último recurso | Dias |
|---|---|---|---|---:|
| `trabalhou` | `ok` | `false` | `true` | 7.079 |
| `folga` | `folga` | `false` | `true` | 5.453 |
| `em_aberto` | `irregular` | `true` | `true` | 1.071 |
| `feriado` | `folga` | `false` | `true` | 889 |
| `trabalhou` | `folga` | `false` | `true` | 879 |
| `abonado` | `ausencia` | `false` | `true` | 743 |
| `suprimido` | `ausencia` | `false` | `true` | 437 |
| `trabalhou` | `irregular` | `true` | `true` | 435 |
| `em_aberto` | `escala_suspeita` | `false` | `true` | 232 |
| `trabalhou` | `irregular` | `false` | `false` | 206 |
| `folga` | `folga_trabalhada` | `false` | `true` | 190 |
| `em_aberto` | `ok` | `false` | `true` | 133 |
| `em_aberto` | `irregular` | `false` | `false` | 121 |
| `feriado` | `folga_trabalhada` | `false` | `true` | 109 |
| `descontado` | `falta` | `true` | `true` | 73 |
| `trabalhou` | `ok` | `false` | `false` | 57 |
| `trabalhou` | `escala_suspeita` | `false` | `true` | 44 |
| `rejeitado` | `irregular` | `true` | `true` | 31 |
| `folga` | `ok` | `false` | `true` | 27 |
| `abonado` | `irregular` | `true` | `true` | 24 |
| `em_aberto` | `sem_certidao` | `true` | `true` | 23 |
| `folga` | `folga` | `false` | `false` | 18 |
| `em_aberto` | `escala_suspeita` | `false` | `false` | 17 |
| `rejeitado` | `folga` | `false` | `true` | 15 |
| `feriado` | `ok` | `false` | `true` | 12 |
| `abonado` | `ausencia` | `false` | `false` | 12 |
| `em_aberto` | `ok` | `false` | `false` | 11 |
| `rejeitado` | `ok` | `false` | `true` | 8 |
| `abonado` | `ok` | `false` | `true` | 6 |
| `abonado` | `falta` | `true` | `true` | 5 |
| `abonado` | `irregular` | `false` | `false` | 5 |
| `folga` | `irregular` | `true` | `true` | 5 |
| `abonado` | `escala_suspeita` | `false` | `true` | 4 |
| `descontado` | `ausencia` | `false` | `true` | 4 |
| `descontado` | `folga` | `false` | `true` | 2 |
| `em_aberto` | `aberto` | `true` | `true` | 2 |
| `rejeitado` | `folga_trabalhada` | `false` | `true` | 2 |
| `rejeitado` | `irregular` | `false` | `false` | 1 |
| `trabalhou` | `folga_trabalhada` | `false` | `true` | 1 |
| `rejeitado` | `ok` | `false` | `false` | 1 |
| `feriado` | `irregular` | `true` | `true` | 1 |
| `trabalhou` | `sem_certidao` | `true` | `true` | 1 |
| `rejeitado` | `escala_suspeita` | `false` | `false` | 1 |
| `descontado` | `irregular` | `false` | `false` | 1 |
| `rejeitado` | `escala_suspeita` | `false` | `true` | 1 |
| `descontado` | `ok` | `false` | `true` | 1 |
| `em_aberto` | `folga_trabalhada` | `false` | `true` | 1 |
| `feriado` | `folga` | `false` | `false` | 1 |

## 2. Grupos D1 e caminho até a lavra

A palavra “Em aberto” vem da união dos turnos com `aberto=True`, das saídas sem entrada e dos furos apurados pela autoridade do período, com os recortes de `datas_em_aberto_de`. O D4 consulta o relógio e lê `CelulaDia.status_dia` quando o relógio deixa passar. Os 394 D1 têm fonte `lavrado`. Seus três status ficam fora de `STATUS_QUE_PEDEM_DECISAO`, o que derruba a oferta mesmo quando `pode_ultimo_recurso=true`. [app/colaboradores/services/calendario.py:94–104,603–616,748–757] [app/ponto/services/dia_decidido.py:346–355,419–430] [app/ponto/status_dia.py:273–289] [DADO RASPADO:404]

No caminho principal, o cartório monta a grade com batidas apuráveis e passa ao supra-juiz a contagem de células da grade preenchidas mais órfãs da grade, os dados de cadastro, a cobertura, as cobranças e o encerramento. `ata_n_missing` é a contagem gravada na ata, em geral de lâmpadas apagadas. O `_n_missing` do classificador é contado nas células da grade que ele recebe. Uma saída órfã do pareador é um turno com `entrada=None` e saída presente, por isso pode aparecer com `ata_n_orfas=0`. [app/ponto/services/cartorio.py:1198–1207,1452–1458,1499–1510,748–765] [app/ponto/supra_juiz.py:317–319,527–537] [app/ponto/turnos.py:234–245]

O cartório passa os códigos à lavra e grava a ata e o status; `[]` deriva `concorde`, salvo a variante `cobrado` quando há fio vivo que ainda protesta. O código `PREVISTO_COM_DIA_INDEFINIDO` deriva `indefinida`. Na regra de status, `concorde` com fato de batida chega a `ok` ou, se o tipo da grade é `folga`, a `folga_trabalhada`. `indefinida` chega a `escala_suspeita` quando as precedências de ausência integral, falta decidida ou folga humana aplicáveis não vencem. [app/ponto/services/cartorio.py:1513–1538,1898–1922] [app/ponto/portas/celula.py:1013–1020,1052–1057,1104–1111] [app/escala/models.py:1622–1643] [app/ponto/status_dia.py:139–162,185–192]

| Status | Ramo | `cel_veredito` | Protestos | `ata_n_missing` | Dias | Padrão da sonda e caminho compatível |
|---|---|---|---|---:|---:|---|
| `ok` | `turno_aberto` | `concorde` | Vazios | 0 | 39 | Há entrada sem saída no pareamento, às vezes junto de par fechado. O turno aberto mantém o texto. Na lavra, a batida afasta o furo integral, e o furo parcial exige marco `missing` na grade julgada. Ex. col56 e col81. [DADO RASPADO:1,7] [app/ponto/supra_juiz.py:326–329,429–439,450–463] |
| `ok` | `turno_aberto` | `concorde` | Vazios | 1 | 40 | Há uma ou mais entradas sem saída no pareamento, com uma falta lavrada. A falta só vira `FURO_PARCIAL` se o ramo satisfizer, entre outras guardas, encerramento e ausência de `sem_ancora`. A guarda efetiva de cada lavra é [nao verificado]. Ex. col60 e col254. [DADO RASPADO:2,66] [app/ponto/services/cartorio.py:1452–1458,1499–1510] [app/ponto/supra_juiz.py:393–439,450–463] |
| `ok` | `turno_aberto` | `concorde` | Vazios | 2 | 17 | Mesmo caminho com duas faltas. Há linhas com trecho fechado e entrada solta, e linhas com um único turno aberto no pareador. Ex. col446. [DADO RASPADO:116] [app/ponto/supra_juiz.py:326–329,393–439,450–463] |
| `ok` | `turno_aberto` | `concorde` | Vazios | 3 | 25 | Mesmo caminho com três faltas. Uma entrada sozinha, como em col204, pode valer como batida para o status com zero minuto realizado. O ramo de parcial com batida e zero realizado ainda exige encerramento, previsto e as demais guardas. [DADO RASPADO:47] [app/ponto/supra_juiz.py:450–463] [app/ponto/status_dia.py:185–192] |
| `ok` | `turno_aberto` | `concorde` | Vazios | 4 | 2 | As duas linhas de col592 têm trecho fechado, entrada aberta, quatro faltas e três órfãs de grade. Com realizado positivo, essas órfãs não acionam o protesto de realizado zero. Se a grade julgada também tinha `missing`, o parcial dependeria do encerramento e de `sem_ancora`, entre outras guardas. A guarda efetiva é [nao verificado]. [DADO RASPADO:186,191] [app/ponto/supra_juiz.py:392–439,527–537] |
| `ok` | `saida_orfa` | `concorde` | Vazios | 0 | 15 | Uma saída sem entrada, sozinha ou junto de par fechado, abre o texto. A saída observada pode casar na grade ou coexistir com realizado positivo. O protesto por órfã da grade exige previsto positivo e realizado zero. Ex. col129 e col407. [DADO RASPADO:25–26,114] [app/ponto/supra_juiz.py:326–329,527–537] [app/ponto/status_dia.py:185–192] |
| `ok` | `saida_orfa` | `concorde` | Vazios | 2 | 1 | Col988 tem um par fechado, uma saída avulsa e 157 min. O positivo afasta o protesto por órfãs com realizado zero. O parcial ainda exige as guardas de grade e encerramento. A guarda efetiva é [nao verificado]. [DADO RASPADO:368] [app/ponto/supra_juiz.py:392–439,527–537] |
| `ok` | `saida_orfa` | `concorde` | Vazios | 3 | 2 | Col988 tem par fechado mais saída avulsa. Col964 tem só `S 11:00`, zero min e zero órfãs na ata, padrão compatível com saída casada a um marco sem formar par. Para esse segundo padrão, o parcial sem realizado ainda exige as guardas de 450–463. O casamento e a guarda efetivos são [nao verificado]. [DADO RASPADO:333,367] [app/ponto/supra_juiz.py:392–439,450–463,527–537] |
| `ok` | `furo_apurado` | `concorde` | Vazios | 1 | 1 | Col922 tem listas vazias na tela, zero min e falta de entrada às 23:00. Com previsto positivo, o furo integral só acusa se todas as guardas passarem. Para `ok` sem `tem_batida`, a regra tem a saída de isenção. O fato de batida da grade e a isenção efetivos são [nao verificado]. [DADO RASPADO:295] [app/ponto/supra_juiz.py:230–237,326–329] [app/ponto/status_dia.py:147–162,185–192] |
| `ok` | `furo_apurado` | `concorde` | Vazios | 2 | 1 | Col174 tem par fechado na madrugada e 145 min. O motor registra furo, enquanto o supra-juiz, com realizado positivo, avalia o ramo de fato e só protesta por parcial sob suas guardas. A guarda efetiva é [nao verificado]. [DADO RASPADO:34] [app/colaboradores/services/calendario.py:602–606] [app/ponto/supra_juiz.py:326–329,392–439] |
| `ok` | `furo_apurado` | `concorde` | Vazios | 4 | 1 | Col174 tem par fechado de manhã, 63 min, duas órfãs na ata e quatro faltas. O realizado positivo afasta o furo integral e o protesto de órfã com zero realizado. O ramo de parcial é o mesmo do grupo anterior. A guarda efetiva é [nao verificado]. [DADO RASPADO:41] [app/ponto/supra_juiz.py:326–329,392–439,527–537] |
| `escala_suspeita` | `furo_apurado` | `indefinida` | Não vazios | 2 | 52 | Sem batida ou turno na tela, previsto positivo e realizado zero. É o padrão do furo integral: se o dia de cobrança está encerrado, sem batida da grade, sem fio vivo e sem cobertura integral, o ramo pode emitir `PREVISTO_COM_DIA_INDEFINIDO` por `sem_vinculo` ou pelo caminho de `sem_ancora`, conforme a precedência. Qual condição de cadastro valeu em cada linha é [nao verificado]. [DADO RASPADO:8,80] [app/ponto/supra_juiz.py:250–265,326–372] |
| `escala_suspeita` | `furo_apurado` | `indefinida` | Não vazios | 4 | 192 | Mesmo caminho de furo integral, com quatro faltas lavradas. Ex. col245. Qual condição de cadastro valeu em cada linha é [nao verificado]. [DADO RASPADO:62] [app/ponto/services/cartorio.py:1168–1183,1499–1510] [app/ponto/supra_juiz.py:326–372] |
| `escala_suspeita` | `saida_orfa` | `indefinida` | Não vazios | 2 | 4 | Em três linhas de col155, só aparece uma saída de madrugada, com previsto 492, zero realizado e zero órfãs na ata. Se essa saída ficou fora da contagem da grade do dia julgado, cabe o ramo de furo integral. Em col741, a ata tem previsto zero e uma órfã. Para esse padrão existe a via de célula de trabalho sem vínculo válido, inclusive pelo ramo que usa a data civil. Os insumos efetivos de cadastro e `n_bat` são [nao verificado]. [DADO RASPADO:28–29,32,224] [app/ponto/services/cartorio.py:1452–1454,1499–1510] [app/ponto/supra_juiz.py:206–214,326–372,392–422,464–469] |
| `escala_suspeita` | `saida_orfa` | `indefinida` | Não vazios | 4 | 1 | Col992 tem só uma saída de manhã, com previsto 420, zero realizado e zero órfãs na ata. A saída abre o texto pelo pareador. O padrão é compatível com o furo integral se a grade do dia julgou zero batidas. O `n_bat` e o motivo de cadastro efetivos são [nao verificado]. [DADO RASPADO:375] [app/ponto/services/cartorio.py:1447–1454,1499–1510] [app/ponto/supra_juiz.py:326–372] |
| `folga_trabalhada` | `turno_aberto` | `concorde` | Vazios | 0 | 1 | Col969 tem uma entrada, uma órfã na ata e zero min previstos e realizados. Sem previsto, os ramos de furo e de órfã com previsto positivo ficam fora. Com tipo de grade `folga` e fato de batida, D2 grava `folga_trabalhada`. O tipo de grade histórico é [nao verificado]. [DADO RASPADO:346] [app/ponto/supra_juiz.py:326–329,392–393,450–451,527–537] [app/ponto/status_dia.py:157–162,185–192] |

Os 249 protestos não vazios das linhas D1 contêm apenas `PREVISTO_COM_DIA_INDEFINIDO`. O ramo `nunca_bateu` da cópia passa `codigos=[]` à lavra, portanto o caminho que produz esse protesto persistido é o principal. As contagens das 15 combinações acima somam os 394 D1 do resumo. [DADO RASPADO:1–403,404] [app/ponto/services/cartorio.py:1296–1325,1898–1906]

## 3. `julgada_em` em relação à expiração

A regra da aberta viva usa **fim da janela prevista do template + 4 horas**, ancorado em `data_turno`. Se o fim cruza a meia-noite, cai no dia seguinte. A aberta só deixa de estar viva quando `agora > expiração`. O encerramento consulta também o fim previsto pelos marcos da célula. O vigia do cartório pode julgar até os marcos vencidos, com teto próprio de 30 minutos. `julgada_em` é o `agora` gravado pelo carimbo. [app/ponto/turnos.py:2005–2035,2113–2130] [app/ponto/selecao_periodo.py:151–170] [app/ponto/services/cartorio.py:880–883,900–911,1391–1411,1505–1510] [app/ponto/portas/celula.py:1217–1237]

A sonda traz horários de alguns marcos faltantes, horas de batidas e minutos previstos. O fim do **template vigente no ato de cada lavra**, a expiração efetiva e a modalidade de julgamento são [nao verificado]. Na tabela, quando o texto identifica a saída final, a comparação usa essa hora **se ela coincidia com o fim do template**. Para os grupos sem aberta geométrica, `fim + 4h` serve como comparação de horários. A regra de encerramento, sem aberta viva, verifica o fim previsto sem somar a margem. [DADO RASPADO:1–403] [app/colaboradores/services/calendario.py:395–408] [app/ponto/turnos.py:2021–2035,2122–2130]

| Grupo (`status` / `ramo` / `ata_n_missing`) | O que os horários permitem dizer |
|---|---|
| `ok` / `turno_aberto` / 0 | **Posterior** em casos distantes como col56, dia 25/10 e julgamento em 02/11, se havia janela finita ancorada naquele dia. Há carimbo no próprio dia, como col936 às 19:01, com fim previsto [nao verificado]. [DADO RASPADO:1,315] |
| `ok` / `turno_aberto` / 1 | **Há exemplos dos dois lados.** Col254, 06/11, saída prevista exibida às 19:00, referência às 23:00 e carimbo às 19:20, **anterior**. Col1024, 13/11, saída às 21:20, referência em 14/11 às 01:20 e carimbo às 21:22 daquele dia, **posterior**. [DADO RASPADO:66,398] |
| `ok` / `turno_aberto` / 2 | **Posterior** no exemplo col446, dia 01/11 com saída às 23:00, referência em 02/11 às 03:00 e carimbo às 23:01. Para linhas que só nomeiam entradas faltantes, o fim exato é [nao verificado]. [DADO RASPADO:116] |
| `ok` / `turno_aberto` / 3 | **Posterior** no exemplo col204, dia 18/11 com saída final às 15:20, referência às 19:20 e carimbo em 19/11 às 15:21. O fim exato das linhas que omitem a saída final é [nao verificado]. [DADO RASPADO:47] |
| `ok` / `turno_aberto` / 4 | **Posterior** nas duas linhas de col592 pela saída final exibida de 23:00. As referências são 06/11 e 20/11 às 03:00, com carimbos às 16:28 e 06:35 nesses dias. [DADO RASPADO:186,191] |
| `ok` / `saida_orfa` / 0 | **Posterior** em casos como col407, dia 23/10 e carimbo em 06/11, se havia janela finita ancorada naquele dia. Em col129 nos dias 09 e 16/11, o carimbo é no mesmo segundo exibido para o fechamento observado do par. O fim **previsto** nesses casos é [nao verificado]. [DADO RASPADO:25–26,114] |
| `ok` / `saida_orfa` / 2 | **Posterior** para col988, dia 06/11 com saída final exibida às 17:00, referência às 21:00 e carimbo em 10/11 às 11:52. [DADO RASPADO:368] |
| `ok` / `saida_orfa` / 3 | **Posterior** nas duas linhas pela saída final exibida. Col964, dia 23/10, referência às 19:00 e carimbo em 02/11. Col988, dia 05/11, referência às 21:00 e carimbo em 10/11. [DADO RASPADO:333,367] |
| `ok` / `furo_apurado` / 1 | **[nao verificado]** para col922. A linha mostra a entrada prevista de 08/11 às 23:00 e o carimbo de 09/11 às 23:07. A hora de saída prevista é [nao verificado]. [DADO RASPADO:295] |
| `ok` / `furo_apurado` / 2 | **Posterior** se havia janela finita ancorada em 24/10. O carimbo é de 02/11 e a saída de 02:00 citada no texto é a do primeiro bloco faltante. O fim exato é [nao verificado]. [DADO RASPADO:34] |
| `ok` / `furo_apurado` / 4 | **Posterior** pela saída final exibida. Col174, dia 15/11, saída em 16/11 às 05:00, referência às 09:00 e carimbo às 22:08. [DADO RASPADO:41] |
| `escala_suspeita` / `furo_apurado` / 2 | **Posterior** onde a saída final está explícita. Col81, dia 15/11, saída em 16/11 às 00:00, referência às 04:00 e carimbo às 06:29. O fim exato das linhas com “Em aberto” sem horário é [nao verificado]. [DADO RASPADO:1–403,8] |
| `escala_suspeita` / `furo_apurado` / 4 | **Posterior** onde a saída final está explícita. Col245, dia 03/11, saída em 04/11 às 00:00, referência às 04:00 e carimbo às 23:53. O fim exato das linhas com “Em aberto” sem horário é [nao verificado]. [DADO RASPADO:1–403,62] |
| `escala_suspeita` / `saida_orfa` / 2 | **Posterior** pela saída final exibida. Em col155, dia 02/11, a referência é 03/11 às 09:42 e o carimbo é às 21:29. Em col741, a referência textual é 03/11 às 22:00 e o carimbo é em 04/11 às 06:29, com `ata_min_prev=0`. A janela efetiva de col741 é [nao verificado]. [DADO RASPADO:28–29,32,224] |
| `escala_suspeita` / `saida_orfa` / 4 | **Posterior** pela saída final exibida. Col992, dia 04/11, saída em 05/11 às 07:00, referência às 11:00 e carimbo em 13/11 às 12:45. [DADO RASPADO:375] |
| `folga_trabalhada` / `turno_aberto` / 0 | **[nao verificado]** no instante da lavra. Col969 tem zero minuto previsto na ata, sem hora de fim exibida, e carimbo em 16/11 às 07:57:03.977280, logo após a entrada exibida de 07:57:02. [DADO RASPADO:346] |

Para os grupos `turno_aberto`, há ainda uma conclusão **no instante da leitura da sonda**. A tela e o D4 usam os turnos da mesma carga. Com uma aberta ainda viva nessa leitura, o D4 devolveria `aberto` com fonte `relogio`. A fonte `lavrado` mostra que a aberta examinada já havia passado da expiração **na leitura**, inclusive em `folga_trabalhada`. A posição do carimbo histórico em relação à janela efetiva continua [nao verificado] onde faltam os insumos acima. [app/colaboradores/services/calendario.py:246–253,635–640] [app/ponto/turnos.py:2005–2018,2121–2126,3063–3067] [app/ponto/status_dia.py:273–289] [DADO RASPADO:404]

## 24/11 00:24 · sessao fcf72d029d65 · LATERAIS-EM-PARALELO · leitura · origem=Code

**CASO-JV.** Uma batida gravada como `E` pode ser a saída do turno da véspera. O pareador pode usá-la como `Turno.saida` e o espelho do app ainda devolver `tipo: E` para a mesma batida. Se isso aconteceu com a batida das 06:57, ou se ela deveria fechar aquele plantão, fica **[nao verificado]**. [app/ponto/turnos.py:1096](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/ponto/turnos.py:1096) · [app/api/views.py:1467](/home/ronald/codex_ws/051e1b5b422bf27369862542f02b95e7829626c5/app/api/views.py:1467)

## Q1. Quem decide e quem pode reescrever

- **Na gravação, o chamador decide.** `registrar_batida` cria o registro com o `tipo` recebido. Antes de criar, pode devolver uma batida viva do mesmo tipo em ±60 s ou a batida já gravada na mesma chave colaborador/instante, inclusive de tipo oposto. A colisão com uma batida próxima de tipo oposto recusa a criação automática. Com `autoridade_humana=True`, a porta pode criar e devolver o aviso de colisão. `app/ponto/registro_batida.py:118`, `:130`, `:144`, `:162`.
- **Na API de batida, o servidor pode substituir o E/S enviado.** Primeiro há deduplicação em ±5 min pelo tipo enviado. Para uma nova batida a até 300 s do instante efetivo, vale `proximo_tipo_de` naquele instante. No fluxo comum, sem turno vivo dá `E`, com turno vivo fora da pausa dá `S`, e em pausa compara os marcos de fim da pausa e de fim de turno. No intermitente, alterna pelo último tipo vivo nas 14 h anteriores. Acima de 300 s, a API consulta os marcos do template do vínculo do **dia civil** da batida. Se só um tipo casa em até 30 min, usa esse tipo. Se ambos ou nenhum casam, conserva o do cliente. `app/api/views.py:420`, `:506`, `:528`, `:569`, `:582`, `:654` · `app/api/views_core.py:555`, `:645` · `app/ponto/turnos.py:2441`, `:2483`, `:2491`.
- **No formulário web, vale o E/S do POST.** Uma `E` com turno aberto pode pedir confirmação. O ramo de gravação repassa o tipo recebido. Ajustes e materializações também fornecem seu próprio tipo na criação. `app/ponto/views.py:287`, `:335`, `:365` · `app/ponto/services/triagem_batida.py:698` · `app/ponto/services/veredito_celula.py:326` · `app/chamados/services/materializacao.py:1580`, `:1697`.
- **Depois da gravação, `flip_tipo` inverte E↔S, registra trilha e recompõe os turnos.** Chamam essa porta o POST com permissão `regularizar_batida`, o veredito administrativo `trabalhou` após simulação e `flip_automatico --apply`. A porta exige autor ou request e recusa batida retratada, tipo fora de E/S ou competência fechada. `app/ponto/services/flip_batida.py:10`, `:28`, `:34` · `app/ponto/registro_batida.py:255` · `app/ponto/views.py:3439` · `app/ponto/services/veredito_celula.py:266`, `:298`, `:317` · `app/ponto/management/commands/flip_automatico.py:51`.
- **A regularização externa tem uma escrita direta.** Com o lote confirmado pelo colaborador e sem disputa aberta, um item `corrigir` pendente com batida-alvo grava `tipo` e `timestamp` na própria batida e registra `corrigir_batida`. `hora_saida` escolhe `S`. Na sua ausência, `hora_entrada` escolhe `E`. `app/chamados/services/regularizacao_ext.py:52`, `:122`, `:143`, `:154`, `:348`.

## Q2. Matriz do pareador

`parear_turnos` ordena por timestamp. Na decisão de borda, usa o papel da ata por minuto quando disponível e recorre a `Batida.tipo` quando a ata se abstém. Depois do laço, ainda pode fechar um turno pela posição da batida. `app/ponto/turnos.py:1217`, `:1310`, `:1377`, `:1497`.

| Condição para a batida gravada como `E` | Resultado |
|---|---|
| A ata atribui `S` e a batida chega ao ramo de borda | Fecha a entrada aberta se a duração passar de 60 s e couber no teto. Sem entrada disponível, acima do teto ou com par de até 60 s, sai órfã nesse ramo. `app/ponto/turnos.py:1421`, `:1430`, `:1447`, `:1487` · `app/ponto/management/commands/detectar_par_relampago.py:49`. |
| A ata atribui `E` e a batida chega ao ramo de borda | Abre turno próprio e fica inelegível para a fusão posicional. `app/ponto/turnos.py:997`, `:1417`. |
| A ata atribui `X`/`Xi`, ou a batida é absorvida como intervalo, eco ou volta de pausa deslocada | Fica interna ao turno quando há um aberto. Essa batida não forma borda. `app/ponto/turnos.py:1315`, `:1356`, `:1371`, `:1398`. |
| A ata se abstém e a `E` vira a única batida do próximo turno, ainda sem saída | No primeiro passe, abre turno próprio. No passe final, pode virar `saida` do anterior. A fusão conserva o `data_turno` do anterior e o tipo gravado da batida, e retira da lista o turno de uma batida. `app/ponto/turnos.py:1382`, `:1417`, `:1059`, `:1096`. |
| Falta uma entrada anterior apta, ou falha uma guarda da fusão | A `E` fica como entrada, se chegou ao ramo de borda. Uma `S` órfã posterior ou uma `S` de pausa sem volta podem fechar o aberto por caminhos próprios. `app/ponto/turnos.py:1417`, `:1059`, `:1170`. |

**Guardas da fusão posicional.** O turno anterior precisa ter entrada, nenhuma saída e os dois marcos de borda no mapa do dia. Precisa haver uma pausa fechada e carimbada de 15 min a 3 h, com a candidata estritamente após a volta. A candidata tem de estar mais perto do fim previsto (`hf`) encadeado a partir da entrada que da instância mais próxima do início concorrente (`hi`). Empate veta. Sem `hi` concorrente, basta essa guarda de proximidade não vetar a candidata. A duração total precisa passar de 60 s e caber no previsto + 4 h. Se as bordas coincidem, vale `cont_max_s`, cujo padrão é 14 h. A suíte contém um caso em que quatro `E` fecham pela última e casos em que a fusão é recusada. `app/ponto/turnos.py:66`, `:912`, `:991`, `:1008`, `:1084`, `:1192` · `app/ponto/tests/test_saida_pela_posicao_entre_turnos.py:103`, `:229`, `:260`, `:286`.

## Q3. Quem lê qual resposta

| Leitor | Fonte que usa |
|---|---|
| **Espelho web e PDF** | O dia da batida vem de `dia_das_batidas`, que usa `data_turno` do pareamento para as batidas que o turno reclama. O realizado usa `turnos_do_colab`. A grade web desenha o **tipo do marco** da grade da célula e conserva `tipo_real` para sinalizar divergência. A listagem bruta, o fallback sem grade e os símbolos de marcação do PDF leem `b.tipo`. `app/ponto/services/espelho.py:464`, `:532`, `:588`, `:625`, `:689` · `app/ponto/turnos.py:2966` · `app/escala/utils.py:1113` · `app/templates/ponto/espelho.html:79`, `:233`, `:248` · `app/relatorios/pdf_espelho.py:881`. |
| **Motor de cálculo v2** | Usa o **tipo gravado** para filtrar e deduplicar. Monta períodos e intervalos pelo **pareador**, com marcos por dia quando disponíveis e papel da ata quando recebe as células. Pode substituir o período pelo de uma ata completa e coerente com as batidas. Se a ata não sustenta o período, ou se o regime é turno partido, mantém a geometria. `app/ponto/motor_calculo_v2.py:467`, `:494`, `:530`, `:558`, `:632`, `:669`, `:688`, `:897`. |
| **Chamados e perguntas** | O reconciliador de turno aberto lê **`TurnoMaterializado`**, que grava as pontas julgadas pelo pareamento, e aplica um veto se os dias estiverem posicionalmente completos. A emissão de órfão na virada e a prova de completude da pergunta sem hora leem `turnos_do_colab`. Perguntas de marco também consultam a lâmpada da ata e o veredito no juiz de vitalidade. Já a prova de batida na janela do slot exige o **tipo gravado**, assim como o fallback de lastro quando falta a lâmpada. `app/ponto/nucleo.py:40` · `app/chamados/reconciliador.py:96`, `:140`, `:400` · `app/chamados/services/disputa_emissao.py:721` · `app/chamados/juizes.py:240`, `:426`, `:860`, `:1539` · `app/ponto/services/lastro.py:289`, `:307`. |
| **App** | `/api/espelho/v2/` devolve **`b.tipo`** em cada batida, usa o **pareamento** para o dia e `turno_aberto`, e lê o status do dia lavrado. `/api/me/` usa o tipo gravado das batidas do dia civil para `hora_entrada`, e os juízes `turno_aberto_de` e `proximo_tipo_de` para o estado e a próxima batida. O juiz de turno **ao vivo** recebe a ata, mas chama o pareador sem `marcos_por_dia`, então a fusão posicional da matriz fica indisponível ali. O GET de justificativas usa o dia do juiz, mas ainda declara aberto por `len(E) > len(S)` dos tipos gravados. A lista de perguntas do app vem do juiz de vitalidade. `app/api/views.py:296`, `:354`, `:1443`, `:1467`, `:1512`, `:2041`, `:2054`, `:2549` · `app/ponto/turnos.py:991`, `:1990`, `:2295` · `app/chamados/services/pergunta_no_app.py:45`. |

**No caso das 06:57**, o relato informa o horário e a gravação como `E`. A origem da batida, a sequência apurável, os marcos e a ata daquele dia, eventual reescrita e o efeito em chamado ou pergunta ficam **[nao verificado]**.

## 24/11 00:23 · sessao f4cd520e2c59 · - · - · origem=Ronald (tmux)

- **Espelho web.** A linha da batida segue o dia atribuído por `dia_das_batidas`, que lê o pareamento. Os minutos realizados vêm de `realizado_do_dia` sobre os turnos do juiz. A grade normalmente lê as lâmpadas da ata, desenha o tipo do marco e mantém o tipo gravado em `tipo_real` para sinalizar divergência. Sem grade realizada, o template desenha `b.tipo`. `app/ponto/services/espelho.py:464–474,520–550,586–591,625–631,688–706,799–802`, `app/ponto/turnos.py:2932–2938,2966–2977`, `app/escala/utils.py:1089–1121`, `app/templates/ponto/espelho.html:233–249`.
- **Espelho PDF.** Reaproveita os dias do espelho e desenha o símbolo das marcações por `b.tipo`. Uma batida pode aparecer na linha da véspera, atribuída pelo juiz, com símbolo de entrada gravada. `app/relatorios/pdf_espelho.py:596–609,873–894`.
- **Motor v2.** Filtra e deduplica por `Batida.tipo` antes de parear. A montagem de períodos usa `parear_turnos` com marcos e, quando alimentado, papel da ata. O adaptador lê as bordas e pausas do `Turno`. Uma ata coerente e completa pode substituir o período geométrico com os timestamps originais, salvo no turno partido. O mapa do dia passa separadamente por `turnos_de_batidas` sobre cópias. `app/ponto/motor_calculo_v2.py:453–506,509–531,558–577,596–645,660–688,867–912,2346–2355,2739–2748`, `app/ponto/adaptador_motor.py:26–45`, `app/ponto/turnos.py:1517–1535`.
- **Chamados e perguntas.** A premissa de turno aberto lê `TurnoMaterializado` e aplica veto se o dia estiver posicionalmente completo. Já a integridade de par-relâmpago usa o `tipo` gravado. A pergunta de marco consulta a lâmpada e o veredito da célula, com guardas adicionais. O fim de uma entrada sem saída no núcleo emite evento de célula. `app/chamados/reconciliador.py:96–115,204–224,263–296,363–416`, `app/chamados/juizes.py:990–1020,1539–1575`, `app/ponto/nucleo.py:47–66`.
- **App.** `/api/espelho/v2/` devolve `tipo: b.tipo`, agrupa pelas datas do juiz e calcula `turno_aberto` dos turnos pareados. O status do dia vem do D4. `/api/me/` usa o tipo gravado para achar a primeira entrada do dia civil e `turno_aberto_de` e `proximo_tipo_de` para os outros campos. `app/api/views.py:296–309,354–357,1443–1473,1493–1516`.

## 24/11 00:17 · sessao e051b537137b · LATERAIS-EM-PARALELO · leitura · origem=Code

## Q1 — Quem decide e quem pode reescrever o tipo

**Na gravação, o chamador escolhe o `E` ou `S`.** `registrar_batida` recebe `tipo` e o passa ao `Batida.objects.create`. A porta pode devolver uma batida existente por deduplicação ou recusar uma colisão de tipo oposto. A consulta aos marcos nessa colisão decide se a gravação pode seguir com o tipo recebido. `app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:118`, `app/ponto/registro_batida.py:137`, `app/ponto/registro_batida.py:144`, `app/ponto/registro_batida.py:162`

- **App pelas duas APIs.** O `tipo` enviado precisa ser `E` ou `S`. Se a hora efetiva estiver a até cinco minutos do relógio do servidor, vale `proximo_tipo_de(colab, ts_efetivo)`. No ramo comum, sem turno aberto vivo sai `E`, com turno aberto fora do intervalo sai `S`, e durante o intervalo a função compara os marcos de retorno e de fim do turno. Para intermitente, a função alterna a partir da última batida viva nas 14 horas anteriores. `app/api/views.py:420`, `app/api/views.py:569`, `app/api/views_core.py:406`, `app/api/views_core.py:555`, `app/ponto/turnos.py:2480`, `app/ponto/turnos.py:2491`
- **App com mais de cinco minutos de diferença.** A API usa o vínculo da data local e chama `tipo_por_marcos` com os marcos do *template* daquele dia. Havendo um único tipo de marco a até 30 minutos, grava esse tipo. Sem vínculo, sem marco compatível ou com os dois tipos na janela, mantém o tipo enviado. `app/api/views.py:578`, `app/api/views.py:654`, `app/api/views_core.py:564`, `app/api/views_core.py:645`, `app/ponto/turnos.py:2441`
- **Formulário web e lançamentos institucionais.** O formulário web repassa o `E` ou `S` do POST. Na materialização de disputa, o motivo da pergunta fornece o tipo. Na aprovação de justificativa de saída, o serviço passa `S`. `app/ponto/views.py:288`, `app/ponto/views.py:365`, `app/ponto/services/triagem_batida.py:700`, `app/chamados/services/materializacao.py:636`, `app/chamados/services/materializacao.py:1581`, `app/ponto/services/justificativa.py:188`

**Depois de gravada, há dois caminhos de reescrita no código da cópia.**

- **`flip_tipo`** inverte `E ↔ S` via `anotar`, registra a trilha e recompõe os turnos. Exige `request` ou autor e recusa batida retratada, tipo fora de `E/S` ou competência aprovada ou exportada. Chamam essa porta o POST de inversão com permissão `regularizar_batida`, o veredito `trabalhou` quando a simulação seleciona a inversão e o comando `flip_automatico --apply` para itens com veredito `auto`. O cron da cópia está configurado com `--competencia --apply`. `app/ponto/services/flip_batida.py:10`, `app/ponto/registro_batida.py:255`, `app/ponto/janelas.py:75`, `app/ponto/views.py:3439`, `app/ponto/services/veredito_celula.py:265`, `app/ponto/services/veredito_celula.py:317`, `app/ponto/management/commands/flip_automatico.py:37`, `app/ponto/management/commands/flip_automatico.py:51`, `app/config/crons.py:707`
- **Regularização externa, item `corrigir`.** Após a confirmação do lote pelo colaborador, o materializador escolhe `S` se o item tem `hora_saida`, ou `E` se tem `hora_entrada`, e salva diretamente `tipo` e `timestamp` na batida-alvo, com trilha `corrigir_batida`. Esse caminho passa por `alvo.save` diretamente. `app/chamados/services/regularizacao_ext.py:52`, `app/chamados/services/regularizacao_ext.py:125`, `app/chamados/services/regularizacao_ext.py:143`, `app/chamados/services/regularizacao_ext.py:154`, `app/chamados/services/regularizacao_ext.py:160`, `app/chamados/services/regularizacao_ext.py:348`

**CASO-JV.** O enunciado informa `E` às 06:57. A rota usada, o tipo enviado, o estado do turno naquele instante, eventual reescrita e o papel de saída da véspera estão [nao verificado].

## Q4 — Sonda de frota

A unidade é **uma batida apurável por PK**, no intervalo inclusivo de datas locais e em toda a frota do schema selecionado. O script compara o tipo persistido com as pontas finais devolvidas por `CargaDaJanela` e com as pontas das pausas reconhecidas pelo próprio pareador. A janela de pareamento inclui dias vizinhos para alcançar turnos que cruzam a meia-noite. Batidas sem papel binário ou com papéis conflitantes têm contadores próprios. Onde o pareador recorre ao tipo gravado, a concordância pode vir desse próprio fallback. `app/ponto/turnos.py:798`, `app/ponto/turnos.py:1382`, `app/ponto/turnos.py:1499`, `app/ponto/turnos.py:1834`, `app/ponto/turnos.py:2877`, `app/ponto/turnos.py:3044`

O bloco é para um shell Django já aberto no schema desejado, fora de uma transação, com datas de exemplo na última linha. A consulta usa uma transação PostgreSQL somente leitura e imprime apenas totais. A agregação foi checada em memória. Execução com Django/PostgreSQL e totais de produção [nao verificado]. `app/config/settings/saas.py:83`

```python
from collections import defaultdict
from datetime import date
from json import dumps

from django.db import connection, transaction
from ponto.turnos import CargaDaJanela, _pares_marcados, batidas_apuraveis, janela_com_pad


def sonda(inicio, fim):
    inicio, fim = date.fromisoformat(inicio), date.fromisoformat(fim)
    if inicio > fim:
        raise ValueError("Inicio posterior ao fim.")

    contexto_ini, contexto_fim = janela_com_pad(inicio, fim)
    placar = dict.fromkeys((
        "apuraveis", "concordantes", "divergentes",
        "E_gravado_S_pareado", "S_gravado_E_pareado",
        "sem_papel_ES", "papeis_conflitantes", "tipo_fora_de_ES",
    ), 0)

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")

        universo = batidas_apuraveis(None).filter(timestamp__date__range=(inicio, fim))
        ids = list(universo.order_by("colaborador_id")
                   .values_list("colaborador_id", flat=True).distinct())

        for offset in range(0, len(ids), 100):
            lote = ids[offset:offset + 100]
            gravadas = dict(universo.filter(colaborador_id__in=lote)
                            .order_by().values_list("pk", "tipo"))
            papeis = defaultdict(set)
            carga = CargaDaJanela(lote, contexto_ini, contexto_fim)

            for colaborador_id in lote:
                for turno in carga.turnos(colaborador_id):
                    for batida, papel in ((turno.entrada, "E"), (turno.saida, "S")):
                        if batida is not None and batida.pk in gravadas:
                            papeis[batida.pk].add(papel)

                    por_instante = {b.timestamp: b.pk for b in turno.batidas}
                    for ida, volta in _pares_marcados(turno):
                        for instante, papel in ((ida, "S"), (volta, "E")):
                            pk = por_instante[instante]
                            if pk in gravadas:
                                papeis[pk].add(papel)

            placar["apuraveis"] += len(gravadas)
            for pk, tipo in gravadas.items():
                atribuicoes = papeis.get(pk, ())
                if tipo not in ("E", "S"):
                    placar["tipo_fora_de_ES"] += 1
                elif not atribuicoes:
                    placar["sem_papel_ES"] += 1
                elif len(atribuicoes) > 1:
                    placar["papeis_conflitantes"] += 1
                elif tipo in atribuicoes:
                    placar["concordantes"] += 1
                else:
                    papel, = atribuicoes
                    placar["divergentes"] += 1
                    placar[f"{tipo}_gravado_{papel}_pareado"] += 1

    return placar


print(dumps(sonda("2026-10-01", "2026-10-31"), sort_keys=True))
```

## 24/11 00:17 · sessao 37dc27fdd841 · - · - · origem=Ronald (tmux)

`registrar_batida` grava o `tipo` recebido do chamador. Pode devolver uma batida existente pelo mesmo tipo em ±60 s ou pela chave colaborador/instante, inclusive se a chave já estiver gravada com o tipo oposto. Na colisão de tipo oposto, a porta recusa o automático e permite a criação com aviso se houver autoridade humana. `app/ponto/registro_batida.py:118–178`

- **API de batida.** O cliente envia E/S. Antes de classificar, a API deduplica pelo tipo enviado em ±5 min. Para nova batida com diferença de até 300 s entre chegada e instante efetivo, o servidor usa `proximo_tipo_de`. Acima de 300 s, consulta o vínculo do dia civil da batida e `TipoEscala.marcos_do_dia` desse dia. Se exatamente um tipo casa com marco a até 30 min, usa esse tipo. Se ambos ou nenhum casam, preserva o tipo do cliente. `app/api/views.py:420–427,506–509,528–545,569–600,654–668`, `app/api/views_core.py:406–413,492–495,514–531,555–586,645–659`, `app/ponto/turnos.py:2441–2464`
- **Regra ao vivo.** Sem turno vivo, sugere E. Com turno vivo fora do intervalo, S. Em intervalo, compara a proximidade do fim da pausa com a do fim do turno. Empate favorece E. Intermitente alterna pelo último tipo vivo nas 14 h anteriores. `app/ponto/turnos.py:2222–2241,2284–2297,2467–2525`
- **Formulário web.** Passa o E/S recebido por POST, com possível confirmação para E sobre turno aberto. Materialização e ajustes fornecem o próprio tipo. `app/ponto/views.py:287–292,335–349,365–371`, `app/ponto/services/triagem_batida.py:617–637,698–712`, `app/chamados/services/materializacao.py:1580–1588,1697–1710`, `app/chamados/services/regularizacao_ext.py:168–188`
- **Reescrita normal.** `flip_tipo` inverte E↔S por `anotar`, registra trilha e recompõe turnos. Recusa retratada, falta de autor/request, tipo inválido ou competência aprovada/exportada. Chamam a porta o POST com `regularizar_batida`, o veredito administrativo `trabalhou` após simulação e `flip_automatico --apply`. O automático usa a regra estrita − marco oposto ≤10 min e nenhum do próprio tipo ≤30 min − e conta com alternativa por slots da célula. `app/ponto/services/flip_batida.py:10–39`, `app/ponto/registro_batida.py:223–225,255–283`, `app/ponto/janelas.py:75–89`, `app/ponto/views.py:3438–3447,3491–3495`, `app/ponto/services/veredito_celula.py:266–272,298–325`, `app/ponto/turnos.py:2409–2438`, `app/ponto/services/flip_auto.py:207–245,285–313`, `app/ponto/management/commands/flip_automatico.py:51–68`, `app/config/crons.py:707`
- **Outra reescrita efetiva.** No lote de regularização externa confirmado pelo colaborador e sem disputa aberta, o item `corrigir` pendente com batida-alvo grava diretamente `tipo` e `timestamp`. `hora_saida` escolhe S. Na sua ausência, `hora_entrada` escolhe E. O fluxo registra `corrigir_batida`. `app/chamados/views_regularizacao.py:19–22,102–130,186–198,216–217`, `app/chamados/services/regularizacao_ext.py:52–66,122–130,143–162,348–357`

Qual caminho gravou as 06:57, quais marcos estavam disponíveis e se houve reescrita posterior ficam [nao verificado].

_8024 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
