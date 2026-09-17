# RELATO — esteira saas-hasner

_Estado de 17/09 07:53. Publico: so ids e contagens, nunca nome/CPF, nenhum codigo._

## PLACAR

| contador | valor | esperado | dono |
|---|---|---|---|
| parados_esperando_corte | 3 | fila do Ronald | Ronald |
| fatias_esperando_smoke | 15 | fila do Ronald | Ronald |
| contratos_estruturais | 8/22 | 22/22 | Code |
| balao de chamados (Validar + Decidir) | 711 | — | admin |
| fila de trabalho (aberto + em analise) | 1.321 | — | admin/colab |
| competencias_pagas_sem_tranca | 0 | 0 | DP |
| chamados_em_competencia_trancada | 0 | 0 | sistema |
| sla_vencido_sem_aviso | 260 | 0 | supervisao/DP |
| furos_vetados_por_regua | 4 (3 vinculos) | 0 | DP/cadastro |
| deploys_agendados | 1 (qui 17/09 14:00) | — | Code |
| colabs_nao_certificados (09/2026) | 285 | 0 | Code |
| chamados_vivos_sem_pergunta_no_app | 130 (46 admin, 84 sistema) | 0 | admin/sistema |

## CERTIFICACAO 09/2026 (fechada 18:09; so leitura, na sombra, nenhum deploy) -- LER ANTES DAS 08:00

_Dados de prod das 04:00 de 16/09. Turnos comparados ate 14/09 (o dump pega o noturno de 15/09 pela metade); celula x
folha ate 15/09. Certificado = a celula do dia (a), a folha que o TXT escreveria por rubrica (b) e o espelho e o cartao
PDF (c) concordam turno a turno, e celula x folha concorda no total do periodo._

### N/N por empresa

| empresa | colabs | certificados | so classes conhecidas | alguma classe fora das conhecidas |
|---|---|---|---|---|
| 2 | 414 | **210/414** | 79 | 125 |
| 3 | 120 | **50/120** | 37 | 33 |
| 4 | 20 | **9/20** | 2 | 9 |
| total | 554 | **269/554** | 118 | 167 |

Um colaborador pode estar em mais de uma classe. Adicional noturno reduzido x relogio e rotulo, nao divergencia (nao conta).

### Classes

| classe | o que o admin vai ver | natureza | estado | Pauta DP | emp 2 | emp 3 | emp 4 |
|---|---|---|---|---|---|---|---|
| bug a | HE do 12x36 sem a tolerancia de 10 min (bug A) | codigo (motor) | conhecida; cura depois do export de 09 | 93/94/95 | 97 | 52 | 4 |
| t4 t8a | saida antecipada que e intervalo (T4/T8a) | codigo (leitura do marco) | conhecida | 83/84/85 | 32 | 9 | 2 |
| intervalo nao batido celula x folha | intervalo nao batido: folha = jornada corrida + intervalo indenizado; celula = pausa | lei (Art. 71 par. 4) + dado (a batida que falta) | espera a resposta do colaborador (perguntado em 16/09) | 150-168 | 94 | 26 | 8 |
| espelho so com vinculo ativo | o espelho comeca no vinculo de hoje: os dias do vinculo anterior (troca ou encerramento na competencia) nao aparecem na tela, e a folha os paga; vale a folha | codigo (tela) | PARADO; **RED confirmado** (0 h na tela x 9,07 h na folha) | 150-168 | 11 | 6 | 0 |
| celula noturna perde a madrugada | celula do noturno conta so ate a meia-noite: % de pronto cai para ~40% e pode reter | a confirmar (o RED simples nao reproduziu; o mecanismo de prod segue em apuracao) | PARADO | 150-168 | 12 | 1 | 1 |
| borda espelho conta continuacao da vespera | espelho soma a madrugada do dia 21 que e da jornada do dia 20; vale a folha | codigo (tela) | PARADO; **RED confirmado** (3 h na tela x 0 na folha) | 150-168 | 8 | 0 | 1 |
| turno longo 16h | turno de 16h ou mais (batida faltando ou resposta no dia errado); retidos fora do TXT | dado (batidas/respostas) | PARADO; conferir antes de liberar | 150-168 | 4 | 0 | 0 |
| celula conta mais que a folha | celula do dia conta mais que a folha | a apurar | PARADO | 150-168 | 5 | 0 | 0 |
| celula x folha diferenca pequena | diferenca pequena celula x folha (menos de 10 h no periodo) | a apurar | PARADO | 150-168 | 8 | 1 | 0 |
| pdf x espelho turno | cartao PDF mostra turno que a tela nao mostra | codigo (tela x PDF) | PARADO | 150-168 | 1 | 0 | 0 |

Natureza: **codigo** = o sistema mostra ou calcula diferente em dois lugares (cura em fatia, depois do congelamento);
**dado** = batida ou resposta que falta ou esta no dia errado (DP/supervisao conferem); **lei** = regra de pagamento
aplicada enquanto a resposta nao chega.

**Pautas para o admin ler antes das 08:00**: DP 150 a 168 (uma por classe e empresa, com os ids), e as ja abertas
93/94/95 (bug A) e 83/84/85 (T4).

### Natureza apurada das classes "a apurar" (noite de 16/09, do registro da rodada; so leitura, sem cura)

| classe | ids | natureza | o que e | teste vermelho |
|---|---|---|---|---|
| turno longo 16h | 174, 922 | dado | jornadas reais de ~16 h batidas (05:05-21:10, 06:55-23:10); folha e tela concordam -- conferir se e dobra autorizada | -- |
| turno longo 16h | 243 | dado | saida no dia seguinte a mesma hora da entrada (24 h): batida ou resposta no dia errado | -- |
| turno longo 16h | 556 | codigo sobre dado | a folha pareia 18:56 com 18:56 do dia seguinte (24 h) e a tela deixa o turno aberto: dois pareadores sobre uma batida faltando | escrito; nao reproduziu com fixture simples |
| celula conta mais | 258, 283, 655 | codigo | em dia de AUSENCIA parcial a celula nao desconta o intervalo e a folha desconta (60 min exatos por dia); 655 tambem tem um turno aberto retido (dado) | -- |
| celula conta mais | 235, 382 | codigo (provavel) | noturno partido (21:00-00:00 / 01:00-05:00): a celula conta a metade depois da meia-noite que nao foi batida (+240 min por dia) -- mesma familia da "celula noturna" | escrito; nao reproduziu com fixture simples |
| diferenca pequena | 191, 880, 238, 145 | lei | pausa prevista e nao batida: a folha conta a jornada corrida (60 min a mais por dia) sem marcar intrajornada indenizada; a celula desconta -- mesmo tema da classe do intervalo | **CONFIRMADO** (12x36: celula 10 h x folha 11 h) |
| diferenca pequena | 259, 736 | codigo | dia de ausencia parcial: folha e celula contam diferente (a folha a mais) | -- |
| diferenca pequena | 200, 784, 903 | dado | turno aberto retido: a celula conta, a folha nao | -- |
| pdf x tela | 935 | codigo | o cartao PDF lista o noturno 05/09 18:52 -> 06/09 06:58 e a tela nao | escrito; nao reproduziu com fixture simples |

Testes vermelhos da certificacao (fora da arvore): 7 escritos -- 3 confirmados (espelho so com vinculo ativo, borda do dia 21, pausa nao batida no 12x36), 4 sem reproduzir (celula noturna, turno de 24 h, noturno so no PDF, noturno partido): o mecanismo de producao segue em apuracao.

### Ids por classe

- **bug a** -- emp 2 (97): 190 195 197 237 247 249 251 253 254 255 276 277 280 281 284 287 300 320 332 335 340 342 346 347 351 360 364 385 388 389 394 396 398 399 400 407 411 414 415 424 425 432 442 443 454 455 456 464 466 473 475 478 482 496 511 516 523 551 552 556 557 558 566 570 576 579 612 625 626 627 654 658 668 673 696 718 719 737 739 760 820 835 838 841 852 854 860 861 871 887 888 891 901 904 908 930 938; emp 3 (52): 49 56 59 60 61 62 63 70 78 79 80 84 90 91 92 94 97 98 100 101 109 111 115 123 126 134 138 139 141 150 154 157 159 166 168 169 170 171 173 639 643 741 744 747 750 752 755 761 866 873 876 925; emp 4 (4): 27 28 29 40
- **t4 t8a** -- emp 2 (32): 174 193 196 212 217 219 231 238 296 306 418 435 450 489 570 696 707 820 821 824 827 841 843 846 847 861 865 874 889 890 913 920; emp 3 (9): 99 104 109 119 145 515 639 769 876; emp 4 (2): 50 624
- **intervalo nao batido celula x folha** -- emp 2 (94): 192 193 202 211 243 247 248 263 277 296 301 306 320 331 340 366 390 406 415 435 444 446 450 452 465 466 468 469 476 492 503 511 516 518 532 549 564 575 583 592 600 612 617 618 627 631 634 696 697 698 699 709 723 727 739 787 789 820 821 824 825 827 833 838 840 841 843 846 847 852 853 861 862 869 872 874 878 884 888 889 890 893 899 902 904 906 907 915 919 920 923 926 930 944; emp 3 (26): 59 60 72 82 98 99 104 107 109 110 111 119 125 150 154 171 502 638 746 769 829 873 876 909 911 925; emp 4 (8): 28 40 41 42 50 510 624 712
- **espelho so com vinculo ativo** -- emp 2 (11): 331 367 375 441 465 584 707 736 789 853 859; emp 3 (6): 91 115 168 515 743 866
- **celula noturna perde a madrugada** -- emp 2 (12): 193 196 219 231 331 418 446 489 707 841 843 865; emp 3 (1): 119; emp 4 (1): 857
- **borda espelho conta continuacao da vespera** -- emp 2 (8): 196 200 218 231 296 343 843 865; emp 4 (1): 857
- **turno longo 16h** -- emp 2 (4): 174 243 556 922
- **celula conta mais que a folha** -- emp 2 (5): 235 258 283 382 655
- **celula x folha diferenca pequena** -- emp 2 (8): 191 200 238 259 736 784 880 903; emp 3 (1): 145
- **pdf x espelho turno** -- emp 2 (1): 935

### 08/2026 (paga): nao certificavel -- Pauta DP

- emp 2: o TXT entregue (exportacao de 01/09) bate com o recibo do Dominio em 2 de 92 colaboradores. HE 50% (60 colabs)
  e faltas parciais (45) do TXT nao aparecem em recibo nenhum; adicional noturno diverge em 41, intrajornada em 27. Tudo
  o que bate e numero redondo: o TXT parece nao ter sido importado, ou o Dominio recalculou (natureza: **dado/processo**).
- emp 3 e emp 4: nao ha exportacao de 08/2026 no sistema (a ultima e de 07), e os recibos trazem horas (**processo**).
- cadastro: dois colaboradores com o mesmo codigo Dominio (883 e 885); cabecalho do recibo diferente do cadastro em 885
  (emp 2), 142 e 624 (emp 4) (**dado**).

### Amostra do admin

93 cartoes (emp 2: 39, emp 3: 34, emp 4: 20), escolhidos pelo sistema entre certificados e classes, sem CPF e sem
matricula, com a folha "regras aplicadas". Entregue em privado ao Ronald.

## BLOQUEIO DA MANHA (17/09 07:2x) -- bug, P7.1

- **Crons da manha (conferido 07:3x nos logs):** `reconciliar_grade` 06:20 RODOU (sombra, sem escrita: divergencia grade x cartorio = 1.498, esperado 0, dono cartorio). `processar_cartorio` 06:28 RODOU nas 3 empresas: julgadas 360 / 817 / 38, emitidos 2 / 11 / 1, furo parcial classificado 45 / 182 / 7 (so emite quando o dia nao tem chamado vivo), 18 dias de ontem abstidos por ata nao lavrada. Chamados criados entre 06:00 e 07:00: 27 -- **16 furos de ONTEM** (15 batida ausente + 1 turno aberto), 1 disputa de ontem, o resto de outros dias/modulos. `detectar_ausencias` roda a cada 5 min (07:25: 101 avaliados, 0 abertos).
- **Pautas 150-174: vivas e NAO lidas, todas para o DP** (aba Pautas do DP). A amostra dos 93 cartoes foi entregue so ao Ronald, em privado, em 16/09 -- nao esta no sistema.
- **Por que a fila nao subiu de 00:17 a 04:15:** depois da meia-noite o deploy exige o ensaio da sombra de HOJE, e o carimbo era de 16/09 -- o ensaio so se refaz as 04:15, e 03:40-04:45 ja e janela sem deploy. Eu disse "depois da meia-noite" sem conferir essa regra; as 04:15 o ensaio FALHOU (este bug) e a espera virou o dia inteiro.
- **Vigia de crons (pedido Ronald 07:3x), pronto, sobe logo depois da cura:** cron de producao que morre (exit diferente de 0 e de 2) abre na hora UMA Pauta de sistema para TI e acende `crons_quebrados` no placar (esperado 0, conta no total); a pauta fecha sozinha quando o cron volta a terminar bem. A quebra de hoje ja se recuperou: sem pauta retroativa.
- **Ordem da manha (refeita 08:12):** TERMO-FECHAR no ar 07:51 -> **sombra refeita e VERDE 08:09** (carimbo 17/09 OK, bloco 56/56, erro 0) -> a geofence das 14:00 (`qui1709`) volta a valer. `manha17` adiantada para 08:20, so raia TELA: TEXTO-FURO (P7.1), CRON-VIGIA, C5-AUDITOR, UIFIC3, UIFIC-FRONT, C5-EMISSORES, C5-TELA, F7, PROPOSTA-EVIDENCIA, F7B, CAUDA-G, C1-MSGDP. AUS-ESCRITA-1 saiu da fila: e raia DINHEIRO (escrita de ausencia) e espera a janela.
- **O cron `termometro_regua` das 06:00 QUEBROU em producao**: o retratador tenta fechar a disputa 5082, que tem 4 respostas do colaborador sem veredito; a guarda recusa (certo) e a recusa derruba o comando inteiro (errado). Os crons que dependem dele nesta manha precisam de conferencia.
- **O ensaio da sombra das 04:15 reproduziu a mesma quebra** (status FALHOU, 1 erro) -- e sem ensaio OK nenhum deploy passa hoje. Por isso a fila da noite (10 fatias) NAO subiu: todas desistiram as 06:50 sem mexer em nada. Pelo mesmo motivo o deploy agendado da geofence das 14:00 tambem nao passaria.
- **CURA NO AR 07:51** (TERMO-FECHAR, `deploy --sem-sombra` com aval do Ronald): a vassoura do chamado resolvido deixa aberta a disputa com resposta sem veredito e o comando segue (teste vermelho antes; suite 6.990 verde). **O `termometro_regua` de hoje rodou de novo pelo envelope do cron as 07:51 e terminou bem** (exit 0, 76 s; 91 avisos de cadastro tocados, 26 retratados); a disputa 5082 segue aberta esperando o veredito do admin. Sombra sendo refeita agora; em seguida `manha17` as 08:40.

## DUAS RAIAS (corte Ronald 17/09 08:1x)

- **10:01 FABRICA-SEM-FIO-MUDO NO AR** (28070726): o cron das 06:38 pergunta tambem pelo caminho da celula, com a trava C7 nos dois caminhos. **Crontab reinstalado a mao as 10:01** (87 linhas, igual ao codigo): a cadeia parou antes do install porque o `crons.sh check` sai 1 quando diverge -- a diferenca era so a linha da fabrica; sem isso o cron de amanha quebraria com a chave velha. *Para a admin:* "a partir de amanha as perguntas dos dias com furo chegam todo dia de manha".
- **Corte Claude 09:2x, aplicado nos scripts:** (1) **fila sem teto** -- a cadeia espera a vez o tempo que for (a trava da regua tambem); a guarda continua sendo a conferencia da arvore na hora de copiar. (2) **raia TELA = UMA passada de regua**: a 1a passada acusava o teste novo da propria fatia como "fora do git", ficava FALHOU e a 2a passada rodava a suite inteira de novo (21 min na TEXTO-FURO); agora os testes novos entram no indice por intencao antes da regua, ela fica verde na 1a e as passadas pos-commit viram carimbo. Vale para as fatias que ainda nao comecaram a cadeia; as 4 que ja esperavam a vez (TRANCA-TELA, FABRICA-SEM-FIO-MUDO, VALIDAR-DIZ-O-SEU, COBRAR-DIA) seguem na versao antiga -- reinicia-las foi barrado pela permissao (ver abaixo). (3) **#22929 e #22930 -> Pautas DP 178 e 179** (ancoradas no chamado, mesma classe dos 46 "marco apagado"): nao se repergunta; o DP decide o dia no proprio chamado.
- **CRON-VIGIA:** vermelha as 09:26 -- o command novo `cron_quebrado` nao estava declarado fora do pipeline (contrato B6). Cura no construir; como agora mexe em `config/crons.py` (o mesmo da FABRICA-SEM-FIO-MUDO), a 2a rodada espera a FABRICA subir, e refaz os testes sozinha se a arvore andar antes da copia (ate 3 vezes).
- **09:23 TEXTO-FURO NO AR** (b355ca25): chamado de furo de dia passado passa a dizer "dd/mm -- marco HH:MM (volta do intervalo) sem batida", sem "atraso atual". Os ja abertos com o texto antigo (513) ficam -- passivo com DRY e aval. Contador `chamados_vivos_sem_pergunta_no_app` = 141 depois do deploy (138 as 09:16; os novos furos do dia entram e saem pelo ciclo). *Para a admin:* "o texto dos chamados novos de dias passados agora diz o dia e o marco; os antigos seguem com o texto velho".
- **A `manha17` NAO disparou as 08:20**: a linha do cron tinha 1.168 caracteres (12 caminhos de fatia) e o cron ignora linha de mais de 1.000, sem aviso. A de 08:40 teria caido igual; a `qui1709` (2 fatias, ~290) cabe e segue valendo. Cura do agendador (lista de fatias em arquivo, linha curta) vai para a fatia RAIAS. A fila da manha espera o disparo manual. Correm sozinhas, na frente: AUS-SALVAR-ERRO (lancada 08:23, 3 vermelhos antes, suite 6.994 verde) -> TRANCA-TELA -> SOLIC-FLAG.
- **TELA** = regua + publica, sem ensaio da sombra, sobe a qualquer hora (fora das janelas 23:20-00:00 e 03:40-04:45). **DINHEIRO** = ensaio + DIFF de folha + janela. Fatia de tela nunca espera fatia de dinheiro; cabeca travada para so a raia de dinheiro.
- **Ja valendo na fila da manha:** as 12 fatias de tela publicam com `deploy --sem-sombra "raia TELA <fatia>"` (o motivo fica na trilha do deploy) e cada uma so espera a anterior TERMINAR -- a dependencia entre elas e guardada pela conferencia da arvore e pelas contagens de cada fatia, nao por marcador. A raia de dinheiro hoje: `qui1709` 14:00 (geofence, furo-cobranca) e AUS-ESCRITA-1 (espera a janela).
- **Contador `fatias_prontas_paradas`** (esperado 0): fatia com testes verdes e sem cadeia terminada ha mais de 30 min; acima de 0 por 30 min = Pauta de sistema. Agora: **0 na raia tela** (todas na `manha17`). Ainda NAO esta no placar: entra na fatia RAIAS (porta `deploy --tela` que recusa arquivo de dinheiro + o contador), na fila logo depois da manha17.

## P7.1 AGORA (Ronald 17/09 08:3x) -- cobranca de ontem, resolver dia, cobrar este dia

- **(2) cobranca de ontem -- a admin:** "os crons de hoje RODARAM; dos 71 furos de 16/09 em cobranca, 48 estao no app do colaborador e 20 NAO chegaram (fabrica de perguntas), 3 por outra causa". Medido no placar dos crons: `termometro_regua` 06:00 quebrou (exit 1) e NAO parou os seguintes -- `reconciliar_grade` 06:20 exit 0, `processar_cartorio` 06:28/06:30/06:32 exit 0, `apurar_furos_diarios` 06:37 exit 0, `disparar_perguntas_competencia` 06:38 exit 0 (42 perguntas), `reconciliar_perguntas_orfas` 06:41 exit 0. Cada cron ja e linha propria do crontab. Contador `chamados_vivos_sem_pergunta_no_app` = **146** (fabrica_nao_rodou 22, marco apagado 44, pergunta viva em outro chamado 31, pergunta do chamado ja encerrada 28, turno fechado na virada 10, coberto por ausencia 8, sem dia 3).
- **Os 20 de 16/09 sem pergunta:** #22474, #22851-#22855, #22857-#22860, #22887, #22891, #22922, #22924-#22927, #22929, #22930, #22939 (18 criados antes da fabrica das 06:38). Causa medida no DRY de um deles (colab 59, #22851): ele cai no caminho da CELULA da fabrica, e o cron diario roda com `--so-fio-mudo`, que por desenho (corte 06/09) deixa esse caminho de fora -- hoje sao ~841 alvos fora. Rodar de novo o cron nao muda nada (idempotente, e o mesmo corte). Emitir os 20 = rodar o caminho da celula so para eles: e escrita de classe -> **DRY + contagem, `--apply` espera o "!"**. A sonda que confirmaria o vinculo celula de cada um foi barrada pela permissao do modo auto; fica para a retomada.
- **Achado:** `escalonar_documentos_ausencia` das 07:51 morreu com exit 137 -- foi derrubado pelo restart do deploy da TERMO-FECHAR, que caiu no mesmo minuto. Nao rodou de novo hoje (so roda 07:51). Deploy em cima de cron agendado = cron quebrado; entra no vigia (CRON-VIGIA) e na fila.
- **(2) EMITIDOS com o "!" (09:0x):** dos 20, **8 tinham motivo pela lei** e receberam pergunta agora (#22851, #22853, #22854, #22855, #22858, #22859, #22860, #22939; 7 ja ligadas ao chamado, a do colab 125 viva no app pela disputa). Contador `chamados_vivos_sem_pergunta_no_app` **146 -> 139**. Os outros 12 NAO saem pelo caminho da celula: 10 tem a ata acusando o marco mas a lei (`classificar_falta`) sem motivo (#22474, #22852, #22857, #22887, #22891, #22922, #22924-#22927) e 2 ja tem a pergunta do mesmo fato morta como fantasma (#22929, #22930 -- a trava de pergunta duplicada nao deixa nascer irma). *Para a admin:* "8 dos furos de ontem ja estao no app; 12 ainda nao -- estao na fila do sistema, nao precisam de acao sua".
- **(4) FABRICA-SEM-FIO-MUDO** pronta na esteira, parada no "!" do lote. DRY (escrita interceptada, comando real): **802 perguntas em 141 colaboradores, todas pelo caminho da celula** -- emp2 726, emp3 75, emp4 1; 526 de dias de setembro, 276 de 21-31/08; motivos: saida sem entrada 770, turno sem saida 32; fio mudo 0. Achado no caminho: a trava C7 (competencia exportada nao se re-pergunta) so existia no fio mudo -- a fatia a poe tambem no caminho da celula. O crontab e reinstalado no mesmo ato, e so se a unica diferenca for a linha da fabrica. **O lote so nasce com o "!" separado.** Obs.: os 10 casos de "lei sem motivo, ata acusando" continuam fora mesmo assim -- a fabrica pergunta pela lei, nao pela ata; tornar a ata a fonte e corte seu.
- **(4) LOTE APLICADO com o "!" (09:11-09:16, sem push):** conferido antes -- nenhum dia da competencia corrente exportado em nenhuma empresa (a trava C7 ainda nao estava no caminho da celula no codigo do ar). Resultado: **801 perguntas em 141 colaboradores, 31 disputas e 32 chamados novos** (o DRY dava 802). Contador `chamados_vivos_sem_pergunta_no_app` 138 -> 138: o lote serviu celulas que acusam e nao tinham pergunta; os que o contador ve seguem por outras causas (entre eles os 10 de 16/09 que so a FABRICA-PELA-ATA alcanca). A fatia FABRICA-SEM-FIO-MUDO foi liberada para subir e reinstalar o crontab. *Para a admin:* "801 perguntas novas foram para o app dos colaboradores, sem notificacao; as respostas chegam na fila de validacao".
- **(1) RESOLVER DIA EM SETEMBRO -- bug de codigo, cura na esteira (VALIDAR-DIZ-O-SEU).** Nao e o botao "Resolver dia" (so 2 usos em 5 dias): e o **validar** dentro do chamado. Caso: colab 438, 16/09 09:19-09:27, dias 31/08, 02, 04, 06, 08, 10 e 12/09 -- 13 validacoes. **O que a admin leu:** "Validacao registrada, mas o ponto NAO foi gravado. A folha desse mes ja foi APROVADA. O sistema nao altera ponto de competencia fechada sem que alguem reabra explicitamente. Reabra o fechamento do mes na tela de folha e valide de novo." **O que aconteceu:** o ponto de setembro FOI gravado (batida criada, conferido em 3); o erro era de 3 perguntas irmas de 29/08 da mesma disputa (#22396, #22494, #22495; competencia 08 aprovada) -- conferido na sombra, em transacao desfeita. Em 48h: 22 registros "nao gravou" em 8 colaboradores. Cura: o resultado passa a ser o da propria pergunta, e as irmas travadas viram aviso com os ids. *Para a admin:* "os dias de setembro que voce validou do colab 438 foram gravados; o aviso era de 29/08, que esta em agosto (fechado) -- isso e com o DP".
- **(3a) COBRAR-DIA na esteira.** Botao "Cobrar este dia (pergunta no app)" dentro da caixa do "Resolver dia": o dia passa pelo cartorio (rejulgado com a ata de agora, emissor canonico, chamado do dia) e, se o chamado ficou sem pergunta, pela fabrica de sempre, com aviso ao colaborador; trilha "manual por <admin>". Recusa em lingua de admin: dia que nao terminou, competencia trancada ("decisao pelo DP"), veto da regua, ata sem furo. **Liberado pelo Ronald para subir antes das 14:00** ("libera cobrar", 09:2x); sobe assim que os testes fecharem e a cadeia tiver vaga. Esperando o seu smoke depois de subir. *Para a admin:* "no calendario, em Resolver dia, o botao Cobrar este dia pede ao colaborador pelo app quando o sistema nao cobrou".
- **(3b) FORM-CATALOGO na esteira (corte Claude 09:1x)** -- sobe depois da SOLIC-FLAG (as duas mexem na mesma view). O catalogo de modulos virou a fonte: 7 areas por departamento dono (supervisao, dp, rh, cadastro, ti, seguranca, hasner), todas com "Outros"; `abrivel_por_humano` marca os seis modulos. Leitura do corte: so `beneficio` casa 1:1 com uma categoria de quem abre para si (dp -> Beneficios, grava `beneficio`); os outros cinco falam de OUTRA pessoa e viraram o "Tipo" do "abrir no fio de uma pessoa" (ausencia, esclarecimento de um dia, regularizacao externa, agenda do dia, mensagem do DP -- padrao); tipo de um dia so pede o "Dia do fato". Esperando o seu smoke depois de subir. *Para a admin:* "em Nova solicitacao ha RH, Cadastro e Suporte Hasner, todos com Outros; em Colaborador escolha o tipo (e o dia, quando pedir)".
- **FABRICA-PELA-ATA pronta (corte Claude 09:1x)** -- espera a FABRICA-SEM-FIO-MUDO subir; depois roda os testes, mede o lote NA SOMBRA (arvore do ar contra a nova, mesma copia) e para no **seu "!" proprio**. Cada marco faltante da ata vira o seu motivo; sem ata lavrada o dia espera; sem escala vigente a lei segue respondendo.
- **Esteira P7.1:** AUS-SALVAR-ERRO em cadeia (regua), TRANCA-TELA e SOLIC-FLAG na fila atras dela. A `manha17` segue esperando o disparo manual.

## PRINCIPIO TABULEIRO (Ronald 17/09 09:5x)

A lampada (marco da ata) comunica o proprio estado por evento; cron e VIGIA, nunca juiz -- cada cron de varredura vira contador "esperado 0" e so alarma; emissao, pergunta e chamado nascem do sinal da celula, na hora. A familia chamado fecha quando: nenhum emissor por varredura, fabrica por evento, telas leem a lampada.

- **Censo confirmado lendo cada command (10:1x): 26 crons julgam por varredura** -- os 23 de antes menos nenhum, mais processar_alertas_turno e processar_alertas_avancados (reabrem turno e abrem chamado a cada 5/30 min) e escalonar_documentos_ausencia (muda o estado da ausencia). Papel proprio **agenda** para o que executa na data um ato ja decidido por gente (efetivar_desligamentos_agendados, sincronizar_status_ferias); **lavra** para tabela de exibicao (placar, badge, scores, perguntas_stale); reconciliar_grade e **vigia** (nao escreve; a divergencia 1.498 vira contador proprio).
- **Contador `juizes_por_varredura`** (esperado 0; hoje 23): fatia PLACAR-VARREDURA -- cada cron declara o seu papel (`juiz` | `vigia` | `alerta` | `infra`) em `config/crons.py` e o placar conta os `juiz`. Entra depois da FABRICA-SEM-FIO-MUDO e da CRON-VIGIA (mesmo arquivo e mesmo placar). Cada juiz que migra para o sinal da celula vira `vigia` com contador proprio e baixa 1.
- Leitura do principio nas fatias ja na fila: a FABRICA-SEM-FIO-MUDO e a FABRICA-PELA-ATA ainda sao varredura (o lote diario); a fabrica por evento (a pergunta nasce no julgamento da celula) e a fatia seguinte da familia.
- **PEDRA TABULEIRO (Ronald 09:5x) -- plano, em construcao:**
  - **Fatia TABULEIRO -- PRONTA (10:1x), esperando a vez** (depois da fila da manha e da CRON-VIGIA, que mexem em `config/crons.py` e no placar):
    - cada cron declara o papel (juiz/vigia/alerta/infra) em `config/crons.py`, com a lista `JUIZES_POR_VARREDURA` que so encolhe;
    - selo novo: todo cron tem papel, e juiz fora da lista = vermelho no commit;
    - a celula "chamado x um juiz por pergunta" da matriz passa a exigir essa lista vazia -- quando zerar, e a 9a verde (hoje 8/22);
    - placar: `juizes_por_varredura` (23) e `divergencia_grade_x_cartorio` (1.498 no log das 06:20);
    - o diagrama gerado ganha o bloco "lampada avisa, cron vigia" (escala -> celula/ata -> evento do cartorio -> consumidores; cada cron pintado como juiz ou vigia pelo papel declarado);
    - 5 linhas do principio no CLAUDE.md e no PRIMER.
  - **Fatia TABULEIRO-HAIKU** em seguida: a pergunta "o sistema ainda vasculha?" no copiloto, lendo os dois contadores.
  - **Leitura minha:** o HANDOFF.md e o contrato da API do app; o principio vai no CLAUDE.md e no PRIMER, que sao os documentos de arquitetura.

## LAMPADAS (medido 17/09 09:5x, so leitura)

1. **A ata guarda estado POR MARCO: sim.** `CelulaDia.ata['lampadas']` (escrita por `ponto/services/cartorio.py::ata_do_dia`), uma por marco do DNA: `tipo`, `hora` prevista, `acesa` (**True = acesa, False = apagada, None = nao sei** -- fato sem hora atribuivel), `luz` (hora real) e `tipo_real`; o isento do art. 62 leva `isencao: True` (e o "desligado": acesa por credito, nao por batida). Nao existe um quarto estado "desligada" gravado. Hoje 101.202 celulas tem ata, todas com lampadas; na competencia corrente, 15.091 atas e 0 agregadas (sem lampada).
2. **Telas que desenham o dia POR MARCO lendo a ata: o espelho do admin** (`ponto/services/espelho.py` -> `leitor_celula.grade_da_celula` -> `templates/ponto/espelho.html`, uma etiqueta por marco, faltante com "?") e o endpoint do copiloto `dia-do-colab` (le `lampadas` direto; nao e tela). **So POR DIA:** calendario do colaborador (um status por dia + a lista das batidas cruas, nao dos marcos), espelho do app/PWA (`api_espelho_v2`: status "alerta/ok" + batidas cruas), worklist de inconsistencias. **O fio/modal do chamado nao desenha o dia.**
3. **Marco com derivador proprio (`lampadas_sem_ata`, esperado 0): 1 tela + 1 degradacao.** A worklist (`ponto/worklist.py` -> `reconciliar_fantasmas._completude`, com `marcos_do_dia` + `parear_turnos`) monta "E.. S.." por conta propria; o espelho cai no construtor antigo (`grade_espelho_janela`) quando a ata e agregada -- 0 casos hoje. A fila de flips e o "Resolver dia" usam `grade_dia`, mas sao acao/linha por batida, nao desenho de marco. O contador NAO existe no placar.
4. **Col709, 14/09** (veredito furo): ata = entrada 07:00 **acesa** (07:02) · saida intervalo 13:00 **acesa** (13:20) · volta 14:12 **APAGADA** · saida 17:00 **acesa** (17:02); batidas do dia 07:02 E, 13:20 S, 17:02 S. Pelo codigo: o espelho do admin mostra as quatro, com "14:12 ?"; o calendario mostra o dia numa cor so e as tres batidas (a volta faltante nao aparece); o app mostra "alerta" e as tres batidas; o fio nao mostra o dia. (Nao renderizei as telas.)
5. **Conclusao: o tabuleiro existe como DADO e so em UMA tela** (espelho do admin). Calendario, app, fio e worklist nao mostram por marco -- para o colaborador e para quem trabalha pelo calendario/fio, o tabuleiro nao existe como UI.

## BO ADMIN 17/09 07:38 (lista da Juliani) -- TRIAGEM, so leitura, ids

**P7.1 (bug, na frente da fila):**
- **(A) texto do furo de dia passado** -- CODIGO, dono Code. O chamado de furo aberto para um dia que ja passou diz "batida de entrada / atraso atual N min" (o texto do furo de HOJE). Vivos com o texto errado: **513**, **25 abertos hoje**; ex.: #22988, #22984, #22983, #22982, #22980, #22968, #22947, #22941, #22940, #22938, #22936, #22932. **Cura TEXTO-FURO pronta** (RED 2 vermelhos na arvore anterior; suite 6.993 verde): dia passado passa a dizer "dd/mm -- marco HH:MM (volta do intervalo) sem batida", sem linha de atraso. **Primeira da `manha17` (08:20).** Os 513 ja abertos = passivo de texto: DRY + contagem depois do deploy, `--apply` espera o "!".
- **(B) aba "Solicitacoes" / "+ Nova solicitacao" no app** -- a aba entrou no commit `1c98ec4c` (13/09 16:38); o botao "+ Nova solicitacao" vem do port original. Corte Claude: chave por empresa, padrao DESLIGADO (some o botao e os links; a aba so aparece para quem ja tem solicitacao). Fatia SOLIC-FLAG na esteira (raia tela, depois da TRANCA-TELA). Os "Justificar" do ponto e do painel sao outra porta e ficam como estao. Com a chave desligada, quem ja tem solicitacao continua vendo a aba. *Para ligar numa empresa:* chave `app_solicitacoes` = 1.
- **(C) colabs 696 e 786, dia 20/08 -- admin nao consegue marcar trabalhou/falta** -- DESENHO (tranca). As duas celulas sao da competencia 08/2026, que esta TRANCADA; a admin ve "Nao foi possivel resolver -- 20/08: ... Competencia 08/2026 trancada". Fatia TRANCA-TELA na esteira (raia tela, logo depois da AUS-SALVAR-ERRO; as duas mexem na mesma view): o dia de competencia trancada mostra "competencia fechada -- decisao pelo DP (Pauta)" com o botao que abre a Pauta. *Resposta para ela:* "agosto esta fechado; mudar dia de agosto e decisao do DP -- abra uma Pauta pelo botao do dia".

- **(D) ausencia #4280 (colab 928, "Saida antecipada - sem abono", 15/09) -- "Salvar alteracoes" nao salva** -- CODIGO (tela), P7.1. Medido: 1 edicao salvou as 08:05 (atestado -> saida antecipada, com trilha); os 4 cliques seguintes (08:07, 08:08, 08:08, 08:10) foram RECUSADOS pelo servidor e a tela jogou a recusa fora -- ela nunca le a resposta, so reabre o painel com os valores antigos. O corpo desses POSTs nao fica no log, entao a mensagem exata daqueles 4 nao e recuperavel; **a recusa de agora e: "Sobrepoe ausencia existente no periodo."** -- as 08:12 nasceu a #4289 (falta injustificada aprovada, 15/09, pelo chamado #22228) no mesmo dia. O tipo NAO exige minutos (0 = dia inteiro) nem documento (fora da lista que exige); a tela nao mostra nada disso antes do clique. Obs.: a #4280 segue com status "aguardando documento", herdado de quando era atestado -- a edicao troca o tipo e nao rejulga o status (Pauta, raia dinheiro). **Cura AUS-SALVAR-ERRO na esteira (raia tela, na frente):** recusa abaixo do botao, campo culpado em vermelho, painel parado; a recusa de sobreposicao nomeia a outra ausencia. *Resposta para ela:* "o dia 15/09 ja tem a falta injustificada #4289 -- as duas nao podem ficar no mesmo dia: cancele ou corrija a #4289 e depois salve a #4280".

**MEDIR:**

| # | caso (ids) | causa | dono | o que a admin ve |
|---|---|---|---|---|
| 1 | espelho que nao abre inteiro (user 211 / colab 439) | servidor entrega a pagina inteira hoje (200 em 0,3 s); os erros 500 foram em 14/09, ja curados | navegador dela | pagina cortada -- precisa de print + qual navegador |
| 2 | mapa do GPS no chamado | botao "Ver no mapa" existe e 235/235 chamados vivos tem coordenada; o mapa depende do servico de mapas no navegador | Code (conferir o console do navegador) | botao sem mapa -- precisa de print do erro |
| 3 | colab 835, intrajornada indenizada | 12x36 07-19 com pausa de 60 min cadastrada e nenhuma batida de pausa: o motor paga os 60 min de intervalo nao gozado todo dia desde 01/09 (lei, art. 71 par. 4) | DP (cadastro) / colab (bater a pausa) | a indenizacao e correta pelo que foi batido |
| 4 | "esqueci a senha" | nao existe fluxo de recuperacao | **Pauta produto** | -- |
| 5 | chamados do feriado 07/09 no 6x1 | 65 vivos abertos antes de 16/09 14:52 e 7 depois (15:42-15:44, passivo do furo com trilha): #22788, #22763, #22727, #22684, #22559, #22545, #22533 -- todos em vinculo marcado "trabalha em feriado" | DP (cadastro, Pautas 140-149) | cobranca de feriado porque o cadastro diz que trabalha |
| 6 | colab 885, 03/09 amarelo | 6x1 sem pausa no cadastro e as batidas da pausa com tipo invertido (entrada 13:02 / saida 14:01): realizado 179 de 540 min | dado + cadastro (DP) | dia incompleto |
| 7 | supervisor com 2o vinculo intermitente | -- | **Pauta desenho** | -- |
| 8 | colab 920, 02/09 ainda aberto | #20019 e o chamado da disputa 4460, com 3 respostas sem veredito (inclui 02/09) | DESENHO; admin da o veredito | fica aberto ate os 3 vereditos |
| 9 | colab 49, 12/09 sem chamado | o cartorio julga todo dia, mas a regua veta a emissao (tambem 23/08); o placar conta na porta "veto" | supervisao (cadastro, Pauta 92) | sem chamado -- por veto |
| 10 | colab 193, horario quebrado todo dia | 12x36 18-06 com pausa 01:00 no cadastro; batida ausente todo dia (7 resolvidos em 14 dias); nenhuma proposta de escala | dado/cadastro, supervisao | um chamado por dia |
| 11 | colab 789, horas 00 | turnos de 14/09 e 15/09 abertos (entrada sem saida) = 0 h | dado (colab/supervisao) | 0 h ate fechar os turnos |
| 12 | colab 358, 10/09 "sem chamado" | o chamado existe: #21323 (em analise, marco 08:00) | -- | esta no fio do colab |
| 13 | colab 422, licenca-maternidade | licenca 25/04-22/08 terminou; situacao ativa; o servidor nao bloqueia (ha 1 questionario pendente "Responder DP"); ela abre o app todo dia e nunca envia batida | app (Fernando) / questionario | "ponto bloqueado" vem do app ou do questionario -- precisa de print |

**Respostas de 1 linha (desenho):** (4) recuperacao de senha ainda nao existe -- Pauta de produto. (7) segundo vinculo intermitente para supervisor -- Pauta de desenho (hoje o sistema trata como vinculo comum). (8) o dia 02/09 fecha quando a admin der o veredito das 3 respostas do questionario desse chamado. (C) agosto esta trancado: dia de agosto e decisao do DP, pela Pauta.

## BLOCOS DA NOITE (non-stop ate qui 17/09 14:00)

Uma cadeia por vez, espera por arquivo de sinal. Nada em folha, ata, batida ou veredito ate qui 14:00.

- **Esteira parada ate ~04:50 por regra**: depois da meia-noite o deploy exige o ensaio da sombra de HOJE, que so se refaz as 04:15, e 03:40-04:45 e janela sem deploy. As fatias seguem prontas e encadeadas; nada foi contornado.
- **bloco a (familia chamado, registro 50 -> 0)** -- em andamento. Na esteira: C5-AUDITOR (50 -> 48), C5-EMISSORES (-> 42; contador `registro_chamado` no placar), C5-TELA (-> 40), C1-MSGDP (-> 39). PARADO-CORTE de negocio (medido): responder chamado resolvido/cancelado/superado; score de incidentes (790 x 2.009 chamados, 324 de 444 scores mudam); contador "chamados abertos" da inteligencia; quem pode agir no chamado (gerir_chamados ignora a area). PARADO-CORTE: emitir_furo_retroativo e o sinal que rejulga a celula (dinheiro, qui 14:00); canal partido, selo de data divergente do fio e mapa_divergencia (o juiz mudaria o universo -- medido); ponto/views (campo do formulario).
- **bloco b fechado: Pautas DP 169-174 = 46 chamados.** A porta de 1 clique (validar de novo / apagar marco) fica PARADO-CORTE ate qui 14:00: validar de novo planta batida e apagar marco mexe na ata.
- **bloco d** -- na esteira, completo: F7-DIA-DA-FROTA ("quem faltou no dia X?" pela celula; golden +1) e F7B (legenda do calendario com selo preso a tela; o dia de hoje na ficha do copiloto, pela mesma funcao da ponte do dia; golden +1).
- **bloco e fechado: testes vermelhos da certificacao = 3 confirmados de 7** (natureza das 4 classes "a apurar" na secao CERTIFICACAO).
- **bloco f (familia ausencia, 49)** -- na esteira: AUS-ESCRITA-1 (o PWA segue o juiz do documento; 49 -> 47). PARADO-CORTE: AusenciaForm sem consumidor.
- **bloco c** -- na esteira: PROPOSTA-EVIDENCIA (a faixa da proposta traz os 28 dias de evidencia e marca o chamado que ela resolve; o front espera smoke; o Aplicar nao foi tocado).
- **bloco g** -- na esteira: CAUDA-G (LICOES.md; o tripwire do modo app sem a excecao morta). PREVIA-DO-HOLERITE segue na fila.
- **Retomada agendada**: `noite17` as 04:50 (cron de disparo unico), em sequencia -- UIFIC3, C5-EMISSORES, C5-TELA, F7, AUS-ESCRITA-1, PROPOSTA-EVIDENCIA, F7B, CAUDA-G, C1-MSGDP (previsao: ate ~11h, antes do deploy da geofence das 14:00). Cada fatia escreve aqui em DEPLOYS AGENDADOS quando termina.

## DEPLOYS AGENDADOS

- 17/09 09:26 deploy agendado manha17, fatia cronvigia: rc=1 -- NAO LANCADA: rodar cronvigia: GREEN parcial vermelho 09:26
- 17/09 09:23 deploy agendado manha17, fatia textofuro: rc=0 -- TEXTOFURO-FIM
- 17/09 06:50 deploy agendado noite17, fatia c1msg: rc=1 -- NAO LANCADA: a CAUDA-G nao terminou no ar
- 17/09 06:50 deploy agendado noite17, fatia caudag: rc=1 -- NAO LANCADA: a F7B nao terminou no ar
- 17/09 06:50 deploy agendado noite17, fatia f7b: rc=1 -- NAO LANCADA: a PROPOSTA-EVIDENCIA nao terminou no ar
- 17/09 06:50 deploy agendado noite17, fatia propev: rc=1 -- NAO LANCADA: a AUS-ESCRITA-1 nao terminou no ar
- 17/09 06:50 deploy agendado noite17, fatia aus1: rc=1 -- NAO LANCADA: a F7 nao terminou no ar
- 17/09 06:50 deploy agendado noite17, fatia f7frota: rc=1 -- NAO LANCADA: a C5-TELA nao terminou no ar
- 17/09 06:50 deploy agendado noite17, fatia c5tela: rc=1 -- NAO LANCADA: a C5-EMISSORES nao terminou no ar
- 17/09 06:50 deploy agendado noite17, fatia c5emi: rc=1 -- NAO LANCADA: a UIFIC-FRONT nao terminou no ar
- 17/09 06:50 deploy agendado noite17, fatia uifront: rc=1 -- NAO LANCADA: a UIFIC3 nao terminou no ar
- 17/09 06:50 deploy agendado noite17, fatia uific3: rc=1 -- NAO LANCADA: a C5-AUDITOR nao terminou no ar
Gate temporal agora e do SISTEMA (cron de 1 disparo na VM), nao de processo do Code: os dois esperadores de quinta morreram com a sessao e foram reagendados.

- **qui 17/09 14:00** — rotulo qui1709: GEOFENCE-VALIDAR-E-RECUSAR, depois FURO-COBRANCA-MORTA-REABRE, em sequencia. Cada uma: testes → DIFF de folha → deploy → DRY do passivo. Ao fim grava o arquivo de sinal, escreve uma linha aqui por fatia e remove o proprio agendamento. Os dois --apply seguem esperando o "!".

## NO AR HOJE (16/09) — 27 fatias

| hora | fatia | o que mudou |
|---|---|---|
| 09:1x | P7.1-MARCO-SEM-HORA (bug) | marco de intermitente sem hora derrubava o auditor diario; 7 perguntas, nenhuma mudou de estado. Auditoria re-rodada em prod: alarme de negocio (13 invariantes), nao mais erro |
| 09:34 | SLA-PELA-FILA | o alerta de prazo ve a fila inteira. Sem rajada: passivo de 1.203 vencidos virou Pauta DP 89/90/91 |
| 09:5x | COMPETENCIAS-PAGAS-SEM-TRANCA | contador de mes pago sem tranca (hoje 0) |
| 10:27 | VETO-QUE-CAI-REJULGA (bug) | furo vetado pela regua deixa de ficar preso quando o veto cai; porta para desfazer a confirmacao de "cadastro errado" |
| 10:55 | TRANCA-SEM-CADASTRO (bug) | a tranca nao encerra aviso de cadastro nem revisao de desligamento; o emissor nao reabre dia trancado |
| 11:12 | HAIKU-BUSCA-PESSOA | busca de pessoa por nome parcial, sem acento e sem ordem; zero resultado = "nao achei", nunca "em dia" |
| 11:35 | ADESAO-AGRUPA-SO-VIVO | o agrupamento da adesao nao reagrupa chamado encerrado |
| 11:56 | CALENDARIO-UM-JUIZ (bug) | tela e rodape leem um juiz; "extra" virou selo HE (so acima de 10 min); dia de folga com batida = "Trabalhou na folga" |
| 12:14 | BALAO-DO-ADMIN | o balao mostra Validar + Decidir (739); antes mostrava 836 "toques" |
| ~12:30 | C1-EMISSORES-VIVOS | aceite de escala e pedido de autorizacao perguntam ao motor quem esta vivo; registro da familia chamado 54 -> 52 |
| 13:0x | FILA-ROTEIA-CATALOGO (bug) | chamado sem dia por natureza sai da gaveta "carimbar o dia": revisao de vinculo e aviso de cadastro vao para "revisao de cadastro" (supervisao, um toque por colaborador); modulo sem dia pela lei tem gaveta propria |
| 13:3x | VALIDACAO-RESPEITA-TRANCA (bug) | validar nao planta batida em competencia trancada; a disputa que nao fecha nao derruba quem validou; o lote classe A nao lista dia trancado |
| 14:18 | FICHA-DO-COLAB-NO-CORE | uma porta para o copiloto, o fio e a ficha completa: vinculo em uma linha, lotacao, app, horas (do espelho), pendencias (das pilulas), proposta de escala, acoes pelo que mais resolve; telefone so na tela |
| 14:37 | PRECEDENCIA-VINCULO-ENCERRADO (bug) | a precedencia do dia enxerga o vinculo encerrado pela troca de escala (colab 152, 07/09: agora "trabalho", previsto); 47 vinculos, 435 dias na competencia; DIFF de folha 0 |
| 14:52 | FERIADO-PADRAO-POR-ESCALA (corte Ronald) | vinculo novo nasce pelo tipo de escala: 5x2 e 6x1 comercial folgam no feriado, 12x36 e escala corrida trabalham; o posto que declarou "opera em feriado" vence. Os existentes nao mudaram (Pautas DP 140-149). Contador vinculos_5x2_6x1_trabalha_feriado = 183 hoje, 0 declarados (nenhum posto marcou "opera em feriado" ainda; quem zera sao as Pautas 140-149); DIFF de folha 0 |
| 15:23 | FURO-PARCIAL-SEM-COBRANCA (bug) | o furo parcial vira cobranca tambem no julgamento na hora (a chave do cron saiu; crontab reinstalado e igual ao codigo); o chamado do dia e o espelho da celula (um por colab-dia, reabre nomeando o marco); retratacao e emissao leem a ata do julgamento corrente. Marco nao vencido, dia trancado e veto nao cobram. DIFF de folha 0 |
| 15:3x | APP-FALTA-NO-TETO | no app nativo, o dia de hoje sem batida so vira "falta" depois do fim do turno previsto (antes, as 14h ja era falta); so leitor |
| 15:47 | CANAL-DE-PUSH | "tem canal de push?" tem um juiz so, em core (cobranca e holerite importam dele); contador juizes_discordam_push = 0 |
| 16:18 | FUROS-SEM-COBRANCA-PORTAS | o contador do furo sem cobranca so conta o que o emissor pode cobrar; **furos_sem_cobranca_viva = 0** (portas fora do total: veto da celula 16, coberto por quem superou 1, decidido pelo admin 3, veto da regua 3) |
| 17:12 | CONTRATO-TROCAR-SENHA (pedido Fernando) | contrato da troca de senha escrito do codigo e travado por teste; /api/me passa a dizer se a troca e obrigatoria; contador colabs_com_troca_de_senha_pendente = 38 (emp 2: 36, emp 3: 2); copiloto responde "quantos colabs estao com senha provisoria?" |
| 18:3x | PERGUNTA-NO-APP (bug) | o chamado de furo de dia passado pergunta pela ata do seu dia; app, contador e copiloto leem a mesma selecao; dia_do_colab diz se a pergunta chegou e quem reteve. Contador chamados_vivos_sem_pergunta_no_app = 515; **passivo aplicado 18:28** (aval Ronald, sem push): 433 chamados em 144 colabs; contador 515 -> **129** |

Todas com suite verde, regua e deploy OK; as de dinheiro com DIFF de folha 0.

**Em curso** (fila da cadeia):
- **GEOFENCE-VALIDAR-E-RECUSAR** (BO Ronald): **PRONTA 16:17** -- teste vermelho na arvore anterior (7 falhas + 2 erros), suite verde (6.930), DIFF de folha 0 (TXT 0, retidos 0), esmeril limpo. **Deploy armado para qui 17/09 14:00** (dinheiro: a recusa retrata batida); o DRY dos 231 registrados sai no deploy e o --apply espera o "!". Antes do deploy, os testes rodam de novo (o crons.py mudou depois do ensaio).
- **FURO-COBRANCA-MORTA-REABRE** (corte Ronald: 117): a regra do chamado do dia vale para o dia inteiro com cobranca encerrada. **PRONTA 16:26** (vermelho 3; suite 6.928; DIFF de folha 0). Deploy tambem qui 14:00; o DRY do passivo sai no deploy.
  - Achado no ensaio: pela lei E1 (03/09), chamado do dia FECHADO pelo admin tambem renasce quando o furo segue. Mantido; o contador foi alinhado a isso.
| 20:2x | GATE-DE-DEPLOY (infra, sem dinheiro) | deploy com hora marcada vive em agendamento de disparo unico na VM, nao em processo da sessao; arquivo de sinal + linha aqui ao terminar; contador deploys_agendados no placar |
| 21:1x | PERGUNTA-NO-APP-CAUSAS + CERTIFICACAO-LINHA-HAIKU (tela) | retencao do chamado com nome e dono, contador por causa; "a folha de 09 esta certificada?" no copiloto; colabs_nao_certificados = 285 no placar |
| 21:4x | C5-FABRICA (familia chamado) | a fabrica de perguntas le o dia do chamado pelo juiz (8 chamados de ausencia) |
| 22:0x | UI-FIO-CABECALHO (tela; front espera smoke) | a ficha do core devolve o cabecalho pronto; o copiloto le a ficha |
| 22:3x | UI4-REABRIR (bug) | "Reabrir questionario" no card do furo volta a cobrar/reabrir (4 cliques de 4 caiam em "nenhuma disputa") |
| 22:35 | UI-FIO-CABECALHO front (restart ui, corte Ronald) | cabecalho compacto no painel do fio e na ficha completa -- aguardando smoke |
| 23:0x | UI-FIO-CABECALHO formato (tela) | texto do cabecalho como no print; app pelo rotulo |

## ATOS EM PROD HOJE (com aval)

- **Tranca pela porta** (emp 2/08, 3/07, 3/08, 4/08), com a lista dos "de fora" na trilha. Encerramento aplicado: 1.795 + 65 + 323 + 113. Regra: mes pago ou exportado tranca pela porta; quem ficou de fora nao barra.
- **Reparo da tranca**: 72 avisos de cadastro e 38 revisoes de desligamento reabertos; 48 gemeos superados no original. Segunda passada 0.
- **SLA**: marco de corte gravado; Pautas DP 89/90/91.
- **Pautas escritas**: 92 (supervisao, colab 49), 93/94/95 (DP, HE 12x36), **39 Pautas de posto** (supervisao, colaboradores sem notificacao do app).

## E0 BALDES — os 739 do balao

571 causas raiz; as 20 maiores cobrem so 119 (16%) — a cauda e por colaborador.

| porta que fecha | chamados | acoes |
|---|---|---|
| pergunta ao colab / declarar dia | 161 | 154 |
| validar um a um | 122 | 100 |
| fila de suporte (solicitacoes) | 120 | 84 |
| "sem dia" (ver abaixo) | 92 | 56 |
| corrigir cadastro da escala | 67 | 61 |
| medir (sem causa) | 29 | 28 |
| validar lote classe A | 28 | **1** |
| proposta de escala — wizard / Aplicar | 28 / 26 | 10 / 12 |
| conferir adesao | 26 | 26 |
| lastro (feriado do posto) | 15 | 14 |
| plano de folgas | 14 | 14 |
| pergunta ao colab (conversa) | 11 | 11 |

- **Lote classe A — FEITO** (aval Ronald 16/09, assinado pelo sistema, trilha "classe A, criterio declarado, aval Ronald 16/09"):
  - 12:57: 16 validadas; o lote parou em dois defeitos (validacao em mes trancado e disputa que nao fecha). As 5 batidas plantadas em mes trancado foram RETRATADAS; cura no ar 13:3x.
  - 13:34 (retomada): 22 validadas, 4 recusadas pela guarda de paridade (batida vizinha do mesmo tipo); 38 perguntas de mes trancado ficaram de fora; 0 batida em mes trancado; trilha completada em 9 que estavam sem ela.
  - Antes (12:57) x depois (13:34): respostas pendentes 174 -> 128; classe A 46 -> 0 (4 ficam, todas em mes trancado); chamados vivos 1.921 -> 1.890; balao 742 -> 711; na retomada 22 chamados passaram a resolvido.
  - Pauta de conferencia para o admin: 5 escritas (DP, por empresa). Retratacao possivel pela porta de retratar batida.
- **Criterio revisado da classe A** (casa qualquer marco do mesmo tipo +-10 min, sem contradicao no dia), sobre os 124 "um a um": 57 perguntas entrariam, so 3 chamados fechariam inteiros. Nada aplicado.
- **Os 92 "sem dia"**: roteados pelo catalogo (fatia no ar 13:0x).

**BALDE 0 — com o colaborador** (competencia corrente): 365 colaboradores, 1.965 perguntas sem resposta, 1.457 furos sem justificativa, 8.470 h em jogo (teto bruto). Idade mediana 21 dias. 63 sem notificacao do app, em 39 postos — Pautas de posto escritas. R$ nao medivel (sistema sem salario). Pauta PREVIA-DO-HOLERITE (em horas) na fila F7.

## BO QUESTIONARIO VAZIO (colab 709, medido 17:2x, so leitura)

- **Colab 709**: 6 chamados vivos de furo. A pergunta aparece no app em 3 dias (03, 08 e 09/09, criadas pelo passivo das 15:41). Nos outros 3 o questionario vem vazio:
  - 04/09: a fabrica de perguntas o dava por resolvido, porque as perguntas de outro marco ja tinham virado batida;
  - 14/09: a batida de origem saia do dia 15, e o dia julgado era outro;
  - 15/09: o juiz da jornada nao via falta, e a ata (que acusa) nao era lida.
- **Frota**: 552 de 1.247 chamados vivos de furo sem pergunta no app (emp 2: 425, emp 3: 114, emp 4: 13). Causas: guarda de turno pareado 220, pergunta de outro marco ja materializada 110, origem de outro dia 99, jornada sem falta com a ata acusando 63, dia de hoje ainda sem ata 36, outros 34.
- **Cura no ar** (PERGUNTA-NO-APP). **Passivo aplicado 18:28** (aval Ronald, sem push, com trilha): 433 chamados em 144 colabs. Contador 515 -> 129. Sobram: 46 com pergunta do mesmo marco e dia ja encerrada e a ata ainda apagada (conferencia humana, a fabrica nao repergunta), 39 com a pergunta viva num outro chamado do mesmo dia (a pergunta esta no app), 34 fora da fabrica, 7 cobertos por ausencia, 3 sem dia.

- **Conferencia 20:3x (so leitura, pelo mesmo juiz do app)** — pergunta no app do colab 709:
  - **14/09: sim** (pergunta 27390, volta do intervalo);
  - **15/09: sim** (27391 saida do intervalo e 27392 saida);
  - **04/09: NAO.** O dia entrou nos 46 abaixo: as 4 perguntas foram respondidas e validadas em 10/09, mas a da volta do intervalo foi fechada como "chamado encerrado" sem virar batida. A ata segue acusando a volta e o chamado 20138 segue em analise. Vai ao admin (Pauta DP 169) e ao golden do dia 04/09.

## CAUDA DA NOITE (16/09)

- **Os 46 "pergunta encerrada, marco apagado" viraram Pauta DP 169 a 174** (por empresa e competencia, com os ids dos chamados e o dia): o colaborador ja respondeu a pergunta daquele marco, a ata segue acusando e o chamado segue vivo; o app nao pergunta de novo. Quem decide o dia e o admin.
  - **ACHADO para o Ronald numerar**: em 13 dos 46 a pergunta respondida e validada foi fechada como "chamado encerrado" com o chamado ainda vivo. Chamados: 18149, 18323, 19142, 19382, 20138, 20319, 20436, 20798, 21001, 21012, 21290, 21332, 21660.
- **Contador na hora (130)**: 46 pergunta encerrada com marco apagado (dono admin) · 38 pergunta viva em outro chamado · 36 fora da fabrica = 28 com pergunta do proprio chamado ja encerrada + 8 turnos que a saida do dia seguinte fechou (dono sistema) · 7 cobertos por ausencia · 3 sem dia.
- **NO AR 21:1x** (PERGUNTA-NO-APP-CAUSAS + CERTIFICACAO-LINHA-HAIKU, tela): cada causa com nome e dono no registro do chamado; contador por causa no placar (130 = 46 admin + 84 sistema); linha Haiku da certificacao: contador **colabs_nao_certificados = 285** (emp 2 210/414, emp 3 50/120, emp 4 9/20), o copiloto responde "a folha de 09 esta certificada?" pela lavra (sem lavra: "nao medida", nunca "sim"); golden +3 (certificacao; colab 709 em 15/09 e em 04/09).
- **Na esteira, em ordem** (uma cadeia por vez):
  1. **NO AR 21:4x** C5-FABRICA (familia chamado, cobranca): a fabrica de perguntas le o dia do chamado pelo juiz; medido antes, 8 chamados de ausencia recebiam pergunta de qualquer dia. Registro do chamado 52 -> 50.
  2. **UI-FIO-CABECALHO: smoke Ronald OK (17/09 00:2x) + 3 ajustes.** Ajuste 3 (a linha encostava no botao "Ficha completa" e ele quebrava; bloco com teto de largura, botao sem quebra, fonte 1 passo menor) **no ar 00:4x** (restart ui; 66 selos verdes; provado no processo do ui). Ajustes 1 ("6x1milano 6x1" vira "6x1 · milano") e 2 ("Londrina/PR/PR") sao codigo da ficha: sobem pelo deploy na fila das 04:50 (UIFIC3, primeira da fila) -- codigo so vai ao ar pelo deploy, e o ensaio de hoje so existe depois das 04:15. Em seguida a UIFIC-FRONT leva o front aprovado ao git (fatias esperando smoke: -1).
  2. **UI-FIO-CABECALHO -- "nao aparece" (conferido pelo Ronald 23:2x): O QUE ERA.** Medido 23:4x: o partial estava no container (mesmo arquivo do host) e o modal que o painel carrega (`/chamados/modal/<id>/`, aberto pelo Ronald as 23:27 e 23:30) o renderizava. Mas o nome que o admin ve fica no TOPO FIXO do painel, preenchido pelo JS, e o bloco vinha no CONTEUDO, que o JS rola ate o card clicado (ou ate o fim) -- o cabecalho saia de vista. Cura: o painel sobe o bloco para o topo fixo, abaixo do nome, tambem depois de cada acao (65 selos verdes na copia da arvore viva, sintaxe do JS conferida). **Cabecalho no ar, aguardando smoke (17/09 00:0x, `docker compose restart ui`).** Prova no processo do ui: o painel traz o espaco no topo fixo e o JS que sobe o bloco (na abertura e depois de cada acao); o modal do colab 248 (o que o Ronald abriu) traz o bloco com a linha "6x1 · 07:30-16:30 · pausa 12:00-13:00 · desde 21/07/2026". Ctrl+F5 no painel antes de conferir. De carona: "Londrina/PR/PR" (21 de 26 pracas ja trazem a UF) -- ajuste na esteira.
     Antes: (23:0x: o texto no formato do print -- escala numa linha com dias e desde dd/mm/aaaa, posto · praca/UF, "tel · app: PWA iOS, ultima batida dd/mm hh:mm", o vinculo de hoje em negrito na ficha completa; o rotulo da plataforma entrou junto) (22:35: partial aplicado na arvore e `docker compose restart ui`, corte Ronald; compacto, 3 linhas, a ficha completa com uma linha por vinculo; 13 selos verdes na copia da arvore viva antes do restart). Parte do git no ar desde 22:0x. UI-FIO-CABECALHO (tela): a ficha do core devolve as 3 linhas prontas (escala · horario · pausa · desde; posto · praca; telefone · app com a ultima batida) e uma linha por vinculo; o painel do fio e a ficha completa recebem a MESMA ficha; o copiloto passa a ler a ficha (a rota existia sem cliente) e responde "qual a escala e o posto de <nome>?". **O partial e os dois templates ficam fora do git, esperando o smoke do Ronald** (64 testes verdes com o front aplicado, teto de queries do modal incluido).
  3. **NO AR 22:3x** UI4-REABRIR (**bug**, P7.1): "Reabrir questionario nao aciona". Medido: 4 cliques de 4 hoje caiam em "Nenhuma disputa aberta" -- a disputa mora no chamado-container e o card clicado e o do furo; o botao aparece pelo juiz do fio e a view procurava outra coisa. Cura: a view pergunta ao mesmo juiz.
- **Familia chamado, o que fica medido e parado** (nao sobe hoje):
  - dia cru no sinal que rejulga a celula quando o chamado muda de estado: com o juiz, mais chamados disparam o cartorio -- espera o fim do congelamento (qui 14:00);
  - dia cru no auditor de invariantes: **corte 16/09 -- so modulos de cobranca de dia.** Na esteira (C5-AUDITOR, sobe depois da meia-noite): o alarme olha o mesmo universo do "ausencia em decisao" e le o dia pelo juiz; medido antes, 1 -> 0 (saiu um chamado que nao cobra dia); os outros dois alarmes nao mudam.
- **UI-5 (painel colapsado)**: ja no ar desde 13/09 (pacote do smoke que entrou no git): 1 linha + Validar/Rejeitar/Corrigir, o resto atras de "detalhes do caso".

## PASSIVO DO FURO SEM COBRANCA — APLICADO (aval Ronald 16/09, 15:41-15:44)

Pela porta do cron, com trilha "passivo furo sem cobranca, aval Ronald 16/09" em cada celula. O push de supervisao foi suprimido (dias retroativos); a pergunta ao colab segue o fluxo normal.

| | celulas | colabs |
|---|---|---|
| antes | 478 | 151 |
| cobradas | 457 | |
| 2o DRY | 20 | 5 |

- As 20 que sobraram o emissor, pela lei, nao cobra:
  - 16: celula que nao e dia de trabalho (vinculo intermitente);
  - 1: dia coberto por pedido de ausencia em analise;
  - 3: chamado do dia fechado pelo admin (decisao humana).
- FUROS-SEM-COBRANCA-PORTAS no ar 16:18: `furos_sem_cobranca_viva` = 0 no placar.
- Achado (fila): celula de intermitente (nao e dia de trabalho) lavrada como furo parcial.
- Colab 709: 14/09 e os outros 5 dias entraram em cobranca.

## BO GEOFENCE (medido 15:4x, so leitura)

1. **Emissao nao caiu.** Furo de geofence: 2 a 5 por dia nos ultimos 14 dias (48 chamados novos; ocorrencias novas vao para o chamado vivo do colab: 176 chamados com nota nova em 14 dias). O ultimo foi hoje as 13:12.
2. **Onde estao**: 231 vivos em "registrado", **sem verbo, fora da fila do admin**. A causa e o corte A3.1b (e201b0c2, 23/08: modulo de auditoria nasce "registrado"). A FILA-ROTEIA-CATALOGO de hoje so mexeu na revisao de vinculo (69 em Decidir, gaveta "revisao de cadastro"), nao no furo de geofence -- nao e bug da fatia de hoje. Autorizacoes do admin por semana: 55 (03/08), 22, 15, 5 (24/08), 0, 3.
3. **Porta do admin**: o fio tem "Aceitar" e "Negar e advertir", e o segundo TAMBEM autoriza a batida (so grava advertencia). Nao existe recusa. E em chamado "registrado" os dois botoes falham: a transicao manual registrado -> resolvido nao e permitida.
- **Corte (Claude)**: fatia GEOFENCE-VALIDAR-E-RECUSAR. O furo de geofence volta a ser cobranca do admin (nasce aberto, verbo Validar); porta de RECUSA com motivo obrigatorio (retrata a batida pela porta unica, trilha "fora do posto"); aceitar/advertir/recusar funcionam tambem no que esta "registrado". A batida continua nunca barrada na hora. A recusa tira batida da folha, entao e dinheiro: sobe quinta 14:00. Os 231 registrados passam para "em analise" por DRY + "!".

## CONTRATOS DO APP

_Para o Fernando (app iOS). A mesma coisa está em app/docs/HANDOFF.md, seção "Plataforma do app"._

### POST `/api/auth/trocar-senha/` (pedido Fernando, 16/09)

Escrito a partir do código e travado por um teste de contrato que confere cada ramo abaixo.

**1. Autenticação.** Header `Authorization: Bearer <access>` (o `access` do login). Sem header, ou token inválido: `401`
do framework, corpo `{"detail": "..."}` (token inválido traz também `"code": "token_not_valid"`) -- **não** traz
`erro_codigo`. O `X-Hasner-Client` não muda nada nesta rota.

**2. Corpo (JSON).** Os três campos são obrigatórios, texto:

| campo            | regra |
|------------------|-------|
| `senha_atual`    | a senha de agora (na senha provisória, é o CPF só com números) |
| `senha_nova`     | pelo menos 6 caracteres (é a única regra de complexidade) |
| `senha_confirma` | igual a `senha_nova` |

**3. Sucesso.** `200`:

```json
{"sucesso": true, "mensagem": "Senha alterada com sucesso.", "tokens": {"refresh": "...", "access": "..."}}
```

- **Guarde os tokens novos na hora.** A troca derruba **todos** os tokens anteriores da pessoa, inclusive o que fez
  esta chamada: access antigo → `401` com `"code": "credencial_trocada"` ("senha alterada; entre de novo"); refresh
  antigo → `401` `{"detail": "senha alterada; entre de novo"}`. Isso vale para os outros aparelhos dela também.
- A flag de troca obrigatória é zerada (`precisa_trocar_senha` passa a `false` no `/api/me/`).

**4. Erros.** Todos com `{"erro_codigo", "erro_msg", "erro_dica"}`. Conferidos **nesta ordem** (o primeiro que falha
responde):

| ordem | status | `erro_codigo`           | `erro_msg`                                        | `erro_dica` |
|-------|--------|-------------------------|---------------------------------------------------|-------------|
| 1     | 400    | `campos_vazios`         | Preencha todos os campos.                         | Senha atual, nova e confirmacao sao obrigatorias. |
| 2     | **401**| `senha_atual_incorreta` | Senha atual incorreta.                            | na senha provisória: "No primeiro acesso, a senha e o seu CPF (somente numeros)."; fora dela: "Verifique a senha atual e tente novamente." |
| 3     | 400    | `senha_curta`           | A nova senha deve ter pelo menos 6 caracteres.    | Use uma senha mais longa. |
| 4     | 400    | `senhas_nao_conferem`   | As senhas nao coincidem.                          | Digite a mesma senha nos dois campos. |
| 5     | 400    | `senha_igual_atual`     | A nova senha deve ser diferente da atual.         | Escolha uma senha diferente. |

**Atenção ao 401 do ramo 2:** é o mesmo status de token vencido. Diferencie pelo corpo: com `erro_codigo` é senha
atual errada (mostre a dica, **não** deslogue); sem `erro_codigo` (só `detail`) é token -- aí sim, renove ou volte ao
login. Os textos vêm sem acento, como estão no código.

**5. Efeitos colaterais.**
- Trilha: uma linha `senha_trocada_pelo_dono` no LogAuditoria (sem a senha).
- Sessões web da pessoa caem; tokens do app (todos os aparelhos) deixam de valer -- os outros aparelhos voltam ao login.
- Nenhum push é enviado.

**Como o app sabe que precisa trocar.**
- `POST /api/auth/login/` (em qualquer plataforma, `X-Hasner-Client: ios` inclusive): `"precisa_trocar_senha": true`
  quando a troca é obrigatória. **Quando não é, a chave não vem** (convenção do login desde S113; ausente = `false`).
  O login **não** é barrado: o token sai válido.
- `GET /api/me/` (**novo, 16/09**): `"precisa_trocar_senha": true|false`, sempre presente. Use ao reabrir o app com o
  token guardado, sem novo login.
- **O bloqueio é do app:** enquanto `precisa_trocar_senha` for `true`, o app mostra só a tela de troca.

## ONDE O DP TRANCA A COMPETENCIA (proximos meses)

- Tela **Fechamento** → mes/ano/empresa → botao **Aprovar** do lote. Nao existe botao so de "trancar".
- O botao so tranca quando ninguem fica de fora (conta vazia, turno aberto, 12x36 sem ancora). Mes ja pago com gente de fora: pedir a tranca pela porta.
- Ao trancar, o sistema encerra os chamados dos dias da competencia; avisos de cadastro e revisao de desligamento ficam.
- Ordem: aprovar → exportar TXT → trancar → publicar holerite.

## CASOS

- **colab 49**: furos 23/08 e 12/09 vetados (cadastro confirmado errado em 08/09). Corte: 24/08-04/09 foi cobertura. Espera a supervisao (Pauta 92).
- **colab 901, 15/09**: dia cumprido, calendario corrigido (ok, sem selo). 14/09 segue cobrado: a colaboradora contestou ("era folga") e espera validacao do admin.
- **colab 709, espelho app x admin** (so leitura, 14:0x): mesmos numeros. Competencia 21/08-20/09: 112,5 h trabalhadas, 3,8 h extras, 4 turnos abertos e 11 dias inconsistentes nos dois lados, que leem a mesma funcao. Nao voltou o BUG 139. Ela nao tem aparelho cadastrado e usa o app pela web, que abre a mesma tela do admin. Diferencas so de apresentacao: (a) no app nativo, a lista de dias vai de 01 a 30/09, mas os totais sao da competencia; (b) o app pinta "alerta" em qualquer atraso, e o admin so marca dia inconsistente; (c) no app nativo, o dia de hoje sem batida ja aparece como "falta" no meio do dia (teto temporal; nao atinge ela; vai para a fila).
- **colab 709, por dia** (so leitura, 14:2x): 11 dias inconsistentes e 4 turnos abertos, com **0 chamado vivo**. Ela recebe pela web: 28 respostas dela, a ultima hoje as 13:55; nao tem aparelho, entao nao ha push. Nas perguntas o canal funciona; o buraco e o emissor.
  - **bug (espera corte)**: o furo parcial (volta do intervalo ou saida sem batida, com o resto batido) nao vira cobranca em dois casos:
    - (a) o julgamento na hora (quando chega batida ou resposta) nao usa a chave `--furo-parcial` do cron, grava a impressao, e o cron das 06:28 pula a celula (colab 709, 14/09);
    - (b) o cartorio nao emite quando o dia ja tem QUALQUER chamado, inclusive resolvido. O chamado da entrada atrasada fecha as 07:2x e cala o furo da tarde (03, 04, 08, 09 e 15/09).
  - Frota, competencia ate 15/09: 839 celulas com furo; 332 sem chamado nenhum e 348 so com chamado encerrado, ou seja, 680 celulas sem cobranca viva em 188 colabs. No log do cron, o furo parcial emitiu 2 vezes em 15 rodadas.
- **Feriado abrindo chamado** (so leitura, 14:0x): 76 chamados vivos em dia de feriado (07/09 e 08/09), sendo 26 em 5x2, 24 em 6x1, 24 em 12x36 e 2 em personalizado. Em todos o vinculo esta marcado "trabalha em feriado"; a celula, a escala e a precedencia dizem "dia de trabalho", entao o emissor cobra pela regra. Nos 5x2 e 6x1 vigentes, 195 de 200 vinculos estao marcados assim (o padrao do sistema). Se eles folgam no feriado, e cadastro: **Pautas DP 140-149** escritas (lista por posto; aval Ronald). Achados: (1) colab 143 esta sem posto, entao nao enxerga feriado municipal (cadastro); (2) **bug**: a precedencia nao enxerga o vinculo ja encerrado por troca de escala (colab 152, 07/09: a celula diz trabalho e a precedencia diz feriado sem previsao). Na competencia sao 47 vinculos, 42 colabs e 435 dias, 271 deles de trabalho. Cura so na precedencia (aval Ronald): **no ar** (PRECEDENCIA-VINCULO-ENCERRADO).
- **Re-lavra 16/09**: medida fechada; 12 diferencas ficam para autopsia.

## PARADOS

- T4-MARCO-QUE-A-BATIDA-OCUPA — DIFF TXT=9 RETIDOS=47 → Pauta DP 83/84/85.
- TURNO-F1-S3 / P71-ANTECIPACAO-12X36 — corte Ronald.
- **CERTIFICACAO 09**: 8 classes fora das conhecidas (tabela acima), PARADAS ate o fim do congelamento; RED de cada uma na fila. **08/2026**: TXT x recibo nao bate (Pauta DP).
- **HE 12x36 sem a tolerancia de 10 min/dia** (bug) — cura so depois do export de 09. Pauta DP 93/94/95 (08/2026 paga: 36,3 h emp 2, 21,5 h emp 3, 1,0 h emp 4).

## FILA

- Copiloto: ferramenta dia_da_frota (data + filtros; celula soberana por veredito; mesma funcao do Raio-X; golden +1 da pergunta de 07/09, escala != 12x36) -- F7, pedido Ronald 16/09.

- Copiloto: legenda do calendario; dia do colaborador na ficha (a ficha mora no core).
- UI do fio do colaborador (cabecalho + proposta de escala com 28 dias de evidencia).
- PREVIA-DO-HOLERITE (app, em horas) + cobranca escalonada + metrica resposta_48h.
- Familia chamado: 52 sitios no registro. Achado: "tem canal de push?" tem dois juizes.

## ESPERANDO RONALD / DP / SUPERVISAO

- ~~Smoke do cabecalho~~ -- **feito pelo Ronald 00:2x** (a geofence de 14:00 nao leva mais o front de carona: ele entra no git antes, na UIFIC-FRONT).
- DP: Pautas 140-149 (feriado em 5x2/6x1; colab 143 sem posto), 89/90/91 (SLA), 93/94/95 (HE 12x36), 83/84/85 (T4); emp 2 06 e 07/2026 — folha fora do sistema?
- Supervisao: Pauta 92 (colab 49) e as 39 Pautas de posto (notificacao do app).
- Admin: validar a contestacao de 14/09 do colab 901.
- Ronald: "!" dos 231 geofence registrados (DRY no deploy de quinta); "!" do passivo da cobranca morta (DRY no deploy de quinta).
- CONGELAMENTO de dinheiro: hoje 18:00 → qui 17/09 14:00.
