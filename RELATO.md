# RELATO — esteira saas-hasner

_Estado de 17/09 07:21. Publico: so ids e contagens, nunca nome/CPF, nenhum codigo._

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

- **O cron `termometro_regua` das 06:00 QUEBROU em producao**: o retratador tenta fechar a disputa 5082, que tem 4 respostas do colaborador sem veredito; a guarda recusa (certo) e a recusa derruba o comando inteiro (errado). Os crons que dependem dele nesta manha precisam de conferencia.
- **O ensaio da sombra das 04:15 reproduziu a mesma quebra** (status FALHOU, 1 erro) -- e sem ensaio OK nenhum deploy passa hoje. Por isso a fila da noite (10 fatias) NAO subiu: todas desistiram as 06:50 sem mexer em nada. Pelo mesmo motivo o deploy agendado da geofence das 14:00 tambem nao passaria.
- **Proposta**: cura do bug (a recusa do fechamento vira "fica aberta", sem derrubar o comando) com teste vermelho; como o ensaio so volta a passar com a cura no ar, subir essa fatia com `deploy.sh --sem-sombra` e refazer a sombra em seguida -- **precisa do aval do Ronald**. Depois disso a fila da noite e a geofence voltam a andar.

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
