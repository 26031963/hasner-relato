# RELATO — esteira saas-hasner

_Estado de 17/09 21:45. Publico: so ids e contagens, nunca nome/CPF, nenhum codigo._

## PLACAR

| contador | valor | esperado | dono |
|---|---|---|---|
| parados_esperando_corte | 3 | fila do Ronald | Ronald |
| fatias_esperando_smoke | 15 | fila do Ronald | Ronald |
| contratos_estruturais | 8/22 | 22/22 | Code |
| juizes_por_varredura | 26 | 0 | Code |
| divergencia_grade_x_cartorio | 1.498 | 0 | cartorio |
| balao de chamados (Validar + Decidir) | 711 | — | admin |
| fila de trabalho (aberto + em analise) | 1.321 | — | admin/colab |
| competencias_pagas_sem_tranca | 0 | 0 | DP |
| chamados_em_competencia_trancada | 0 | 0 | sistema |
| sla_vencido_sem_aviso | 260 | 0 | supervisao/DP |
| furos_vetados_por_regua | 4 (3 vinculos) | 0 | DP/cadastro |
| deploys_agendados | 1 (qui 17/09 14:00) | — | Code |
| colabs_nao_certificados (09/2026) | 285 | 0 | Code |
| **colabs_sem_furo_no_periodo (09/2026, ate 16/09)** | **179/554** (no placar desde 17:31) | 554/554 | admin |
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

## FILA ESTRUTURAL (Ronald 12:3x: nunca para) -- `registro_chamado` 48 -> 0

Ordem: C5-EMISSORES (-6) -> C5-TELA (-2) -> C1-MSGDP (-1), com as fatias de tela da manha no meio (UIFIC3, UIFIC-FRONT, F7, PROPOSTA-EVIDENCIA, F7B, CAUDA-G) -> RESPOSTA-TARDIA (-1) -> INTEL-JUIZ (-2) -> C5-CANAL (-4) -> AGIR-POR-DONO (-3) -> WORKLIST-ATA -> fabrica por evento. Ao lado, com arquivos proprios: FABRICA-PELA-ATA (com o seu "!"), TABULEIRO.
- `registro_chamado`: 50 -> **48** (C5-AUDITOR, 11:01) · 12:59 UIFIC3 no ar (sem registro; a UF uma vez so e a escala como tipo e apelido -- os ajustes 1 e 2 do seu smoke de 00:2x) = 48. UIFIC-FRONT: a copia dos testes falhou (pasta nao recriada no script), corrigida e relancada. 13:30 **C5-EMISSORES NO AR** (fadb3646): seis emissores e fechadores de cobranca leem o dia pelo juiz = **42**. Proximo no registro: C5-TELA (-> 40). UIFIC-FRONT: um arquivo de teste estava com dono root (editado com sudo na madrugada) e a copia falhou sem mexer em nada; dono corrigido e cadeia relancada. 13:46 UIFIC-FRONT caiu na regua por um arquivo meu solto em docs (o comando de contraponto do SMOKE-150, lido como codigo novo); copia desfeita limpa, arquivo virou texto, fatia relancada. C5-TELA (-> 40) na regua desde 13:45; F7, PROPOSTA-EVIDENCIA, F7B, CAUDA-G e C1-MSGDP em sequencia atras. `registro_chamado` = **42**. 14:16 **C5-TELA NO AR** (cc280d68): o calendario do colaborador e a worklist do DP leem o dia do chamado pelo juiz = **40**. *Para a admin:* "o chamado aparece no dia certo do calendario e da lista do DP, mesmo quando foi aberto em outro dia." Proximo: F7-FROTA, PROPOSTA-EVIDENCIA, F7B, CAUDA-G, C1-MSGDP (-> 39); UIFIC-FRONT e, atras dela, GEO-PAINEL e SEM-FURO disputam a trava. 14:2x **UIFIC-FRONT no git** (d20c4d2e): o front do cabecalho do colaborador (painel do fio e ficha completa), com o seu smoke de 00:2x, entra no git; push e deploy em curso. `registro_chamado` = **40**. 15:48 **FURO-COBRANCA-MORTA-REABRE** (a6d222ed) e 17:2x **C1-MSGDP no ar** (ea9b0778): a mensagem rapida do DP reusa o chamado vivo, nao so o aberto -- **registro_chamado = 39**, a fila da manha fechada. 17:31 **SEM-FURO no ar** (d68e0366): o contador `colabs_sem_furo_no_periodo` passa a sair do placar, lido de prod -- **179/554** hoje. 18:32 **CARTORIO-SO-CHAMADO-DE-BATIDA NO AR** (e0cc7b5a, com o seu "!", deploy com ensaio da sombra). A guarda de baseline tinha barrado as 17:3x porque o `cartorio.py` mudou hoje em tres fatias: refeita contra a arvore nova (RED 3, suite 7.100). **O efeito nao e imediato e isso e da regra**: celula ja lavrada so muda quando e re-julgada. Como a impressao do dia passa a ser outra (a lista de chamados encolheu), o cartorio re-julga sozinho na varredura das 06:28 de amanha e os 21 saem da retencao. Para ver hoje, basta adiantar `processar_cartorio --apply` (idempotente, mas escreve: espera o aval do Ronald). 17:45 **CAUDA-TRILHA-DO-CARTAO no ar** (a9463665): cartao em lote e TXT gravam trilha (quem, quando, filtro, ids, quantos) e o cartao informacional ganha rodape com quem gerou e o periodo -- o buraco que apareceu hoje de manha esta fechado. 18:10 **TABULEIRO no ar** (d59d24df): todo cron declara o papel, o selo barra juiz fora da lista, e o placar mede `juizes_por_varredura` (26, a divida) e `divergencia_grade_x_cartorio` (1.498). O principio entrou no CLAUDE.md (secao 4a) com o seu nome. Na trava: CARTORIO-SO-CHAMADO-DE-BATIDA (com o "!"), CREDITO-NO-PROPRIO-DIA, CARTAO-PELA-CELULA, PROPOSTA-EVIDENCIA, TRANCA-DIZ-O-MESMO, TABULEIRO, CAUDA, F7B, CAUDA-G. 14:53 **F7-DIA-DA-FROTA no ar** (4ad1c94d): o copiloto responde quem faltou num dia, na frota, pela celula soberana. Antes dela, as 14:0x, subiu o deploy agendado `qui1709` (GEOFENCE-VALIDAR-E-RECUSAR, f5d67b52 -- raia dinheiro, janela aberta). Na fila da trava: GEO-PAINEL (na regua), SEM-FURO, PROPOSTA-EVIDENCIA.

- **RESPOSTA-TARDIA (estrutural, -1 no registro) -- PRECISA DE RECONSTRUCAO**: montada de manha, a cura nao casa mais com a arvore (a resposta do colaborador passou a validar a hora antes, e o selo antigo `test_z4_chamado_fechado_bloqueia_resposta` esperava 400 onde a fatia quer 200 com nota/Pauta). Nao e ajuste de linha: o ramo da resposta tardia tem que entrar antes da validacao da hora. Fica na fila estrutural, para refazer depois que a leva de hoje subir.

- **LICAO DA NOITE (17/09 19:2x) -- espera que desiste calada**: a F7B ficou **2 horas parada** e com ela a CAUDA-G, a classe 6, a CARTAO-PELA-CELULA e a TRANCA-DIZ-O-MESMO. Duas causas: (1) o arquivo de sinal da PROPOSTA-EVIDENCIA **sumiu** numa corrida de esteiras (duas rodadas da mesma fatia vivas ao mesmo tempo apagando os arquivos uma da outra) -- curado com trava por fatia (`/tmp/esteira_<fatia>.lock`) e o sinal reposto com a verdade conferida no git (PROPEV-FIM + push a1d568e1); (2) a espera tinha **teto de 2 h** e, ao estourar, a esteira morria em silencio -- teto removido em 15 esteiras. Mesma familia da LICAO-PGREP: espera que desiste sem avisar e fila parada sem ninguem ver.

- **FILA PARADA 18:30-21:29 (erro do Code, curado)**: nenhuma fatia subiu por 3 h. Causa: as 19:2x eu reescrevi os `esteira.sh` para tirar o teto de espera, **no mesmo arquivo, com a F7B rodando**; o bash le o script aos pedacos, caiu no meio de uma linha e a esteira morreu de erro de sintaxe. As tres cadeias prontas (CARTAO-PELA-CELULA, CREDITO-NO-PROPRIO-DIA, TRANCA-DIZ-O-MESMO) ficaram esperando a F7B, que ja nao existia. A F7B voltou as 21:29; a CAUDA-G vai cair do mesmo jeito quando sair da espera e sera relancada. Licao gravada: script em execucao so se troca por arquivo novo + `os.replace`, nunca no lugar.

## FILA BO (admin, Ronald, Fernando) -- medir em segundo plano; so bug provado de codigo entra, ATRAS da estrutural em curso

- **GEO-PAINEL** (bug, Ronald 12:2x) -- o pino de GPS do painel situacional parou de abrir o mapa em **05/09, commit 1c109378** ("Censo de app ganha a coluna ... estado do aparelho"): a coluna "app" entrou antes de "push" e o GPS foi da posicao 7 para a 8; o script `geo-colab-map.js` achava a coluna pela POSICAO (`cellIndex 7`) e o clique passou a cair fora, calado. O mapa nao tem URL propria (abre por JS) e o "Ver no mapa" do fio ja usa a mesma engine -- fonte unica, so o gatilho do painel morreu. Cura: a celula ganha nome (`data-col="geo"`) e o script procura o nome. **Na esteira, atras da estrutural em curso; sobe para o seu smoke nas duas cascas e so entra no git depois dele.**
- **12:48 LOGIN-APP NO AR**: login do app (cabecalho `X-Hasner-Client`) com conta sem colaborador responde 403 `conta_sem_colaborador`; `admin_em_mobile` deixou de dar 500. *Para o Fernando:* "teste com uma conta de colaborador; a de gestao agora recebe 403 conta_sem_colaborador no login -- mostre erro_msg e erro_dica".
- **13:18 DEPLOY-ESPERA-CRON NO AR** (f1da40f5): o reinicio do deploy espera o cron em curso (ate 10 min) e a raia TELA recarrega a copia pelo HUP gracioso -- deploy nao derruba mais cron.
- **Conta de teste iOS por empresa** -- decisao do Ronald (ver "Contratos do app").
- **GEO-PAINEL**: parada desde 12:35 sem aviso (import sobrando no teste novo barrou o ruff); curada e relancada 13:46, atras da estrutural.
- **CARTORIO-SO-CHAMADO-DE-BATIDA (raia DINHEIRO, janela aberta) -- autopsia dos 41 retidos, 15:0x**: o supra-juiz ja le so os modulos que FALAM DE BATIDA (`FURO_MODULES`, HX-CSF-MODULO/T6.2 fatia 1); o **cartorio lia TODO chamado do dia**. Um aviso leve (`batida_fora_escala_leve`, status `registrado`, sem pergunta e sem marco), um `par_relampago` ou um `turno_aberto_24h` deixavam o dia de marcos TODOS ACESOS com veredito `cobrado` -- e o export retem por veredito que acusa. **Medido em prod: 136 celulas em 88 colaboradores na competencia 09/2026 (101 por aviso leve, 28 por turno_aberto_24h, 7 por relampago); 22 colaboradores (17 emp 2, 5 emp 3) estao retidos SO por isso.** Nenhum furo real foi calado por esses chamados (0 celulas). Os outros 19 dos 41 sao desacordo por doutrina, nao bug: `fato_em_ausencia` (trabalho em dia coberto retem de proposito), `nunca_bateu` (BUG 24) e `indefinida`. Fica em separado, para medir depois: 297, 830 e 866 com `discordante/REALIZADO_ZERO_COM_TURNO` e as lampadas acesas. **Cura**: o cartorio ganha `chamados_que_falam_de_batida` e passa a ler o mesmo vocabulario. RED vermelho na arvore do ar e GREEN na cura; DIFF por par na sombra em curso (criterio de aceite: RETIDOS caem ~22 e o TXT ganha as linhas deles, nada mais muda). **Como e dinheiro e o DIFF nao e zero, o push espera o seu "!".** Achado separado: `turno_aberto_24h` NAO esta em `FURO_MODULES` e fala de batida (vira a pergunta `orfao_14h`) -- a tupla tem um buraco, que toca supra-juiz, reconciliar_grade, gerar_celulas e regeneracao; nao entrou nesta fatia.
- **BO admin 17/09 -- colab 863 (mat 1723, emp 2), "corrigi a escala para 19-07 e nada mudou" (medido 16:0x)**
  - **A correcao nao chegou ao sistema.** A trilha das 15:48 de hoje diz: `criar EscalaColaborador ... Vinculo escala: 07:00 14:00 15:00 19:00 a partir de 2026-08-21` -- ela reaplicou a MESMA escala diurna. Por isso o cabecalho segue 07:00-19:00: e o que esta cadastrado. Os dois vinculos apontam para o tipo `PAI-12x36.16` (07-19), que e template de **16 vinculos ativos** -- editar o horario nele mexeria em todo mundo.
  - **A regeneracao RODOU**: `regenerar_celulas_vinculo` as 15:48, 31 celulas de 21/08 a 20/09 (com o mesmo DNA 07-19). Entao **nao ha bug de "mudar escala nao regera celula"**.
  - **O estrago hoje**: a entrada real das 19:25 acende a lampada de SAIDA das 19:00 e a madrugada (02:00, 02:54, 06:55) cai no dia seguinte como orfa, em dia que a escala diz folga. No mes o cartao mostra **10,44 h trabalhadas e 181,56 h de "saida antecipada"**, e ele esta **retido no TXT** (espelho_cobrado + furo).
  - **Ensaio na sombra (tudo desfeito)**: com o tipo 19:00-07:00 **e** a fase caindo no dia da entrada, o turno de 04/09 19:25 -> 05/09 06:54 fecha em **UMA celula**, com as quatro lampadas acesas e zero orfas (veredito concorde). Trocar so o horario, sem a fase, nao resolve -- provado tambem. **A classe "noturno perde a madrugada" NAO se confirma neste caso: e cadastro.**
  - **Pauta 203 (supervisao)** com o passo exato. *Para a admin:* "a escala salva hoje voltou a ser 07-19; aplique um 12x36 noturno (19:00-07:00, com a pausa real) valendo desde 21/08, com a fase caindo em 04/09 -- no ensaio o turno fecha inteiro e os furos somem. Enquanto isso nao for feito, a folha dele fica retida."
- **COLAB 863 (mat 1723) -- ESCALA APLICADA 21:3x (ordem do Ronald), pela porta do vinculo, com trilha**: `PAI-12x36.104` (19:00-07:00, pausa 01:00-02:00 -- a pausa real dele varia de 00:46 a 03:19, centro em 01-02) valendo desde 21/08, fase em 04/09. Ensaiado antes na sombra pela MESMA porta. **Resultado**: 25 dos 27 dias viraram concorde; 04/09 e 16/09 fecham com as quatro lampadas acesas e zero orfas; os dias de folga ficaram limpos. Sobram 2 dias `cobrado` (04 e 12/09) por cobrancas antigas da epoca 07-19, que o cartorio reconcilia na proxima passada.
  - **Falta a segunda metade -- as ETIQUETAS**: o motor pareia pelo TIPO da batida, e o aplicativo gravou **33 etiquetas trocadas em 11 plantoes -- exatamente 3 por plantao**: a entrada das 19h esta certa, mas a saida para a pausa foi gravada "E", a volta "S" e a saida final "E". Por isso o cartao ainda mostra 0 h (o motor ve "entrada sem saida" em todo turno). A causa e a escala errada: o app sugere o proximo tipo pelos marcos, que eram 07-19. **Com a escala certa, as batidas daqui em diante devem vir certas.** O corretor automatico (`flip_automatico`, 07:12) so pega 3 desses dias (trabalha pela fila do tripwire). **A folha dele segue retida -- nenhum numero errado sai.**
  - **Espera o "!" do Ronald**: corrigir as 33 etiquetas pela porta `flip_tipo` (escritor unico, trilha por batida), com a ata como juiz. Lista completa de ids no scratchpad `bo1723/etiquetas.py`.
- **CARTAO-PELA-CELULA (corte Ronald 17/09, "mesmo banco, dois resultados") -- raia TELA, lancada 16:03**: o cartao-ponto passa a LER a folha: morre o NUMERO por conta propria (mes civil, dia futuro, plantao em folga somado). A grade (`grade_espelho_janela`) SEGUE -- ela e contrato de render declarado (selo F1c) e e a mesma que o cartorio usa para julgar; e dela que saem a espuria e o credito do Art. 62 que o documento desenha. Tentei mata-la e tres selos ficaram vermelhos: o corte certo e no numero, nao no desenho. Periodo = a COMPETENCIA da empresa; o cartao para em ONTEM; os totais por rubrica saem de `eventos_do_fechamento` -- a MESMA funcao que escreve o TXT --; orfa e credito do Art. 62 vem da ATA. O dia a dia segue no papel como EVIDENCIA: onde o motor e a folha discordam (plantao em folga, turno antes da admissao), o dia aparece e o total segue o que se paga. **Mata as classes 1, 2, 4 e a parte de tela da 3.** Contador novo no placar: `cartao_x_txt_divergentes` (esperado 0, dono Code, conta no total). RED vermelho (4) e GREEN limpo.
- **BO admin 17/09 (print) -- TRANCA-DIZ-O-MESMO (fatia de tela lancada 15:51)**
  - **(1) a frase**: validar pergunta de competencia fechada respondia *"a folha desse mes ja foi APROVADA... reabra o fechamento do mes na tela de folha"* -- caminho que nao e da admin e que contradiz a tela do dia, que desde a TRANCA-TELA diz *"competencia MM/AAAA fechada -- decisao pelo DP (abra uma Pauta)"*. Agora e UMA frase e UMA fonte (`aviso_da_tranca`), que aceita competencia desconhecida sem inventar numero; a acao vira abrir Pauta para o DP. **Fica na fila (front, precisa do seu smoke)**: o botao "Abrir Pauta DP" dentro do proprio aviso -- hoje o botao so existe no chip do calendario.
  - **(2) a pergunta #27648**: ela e do dia **31/08**, que pertence a competencia **09/2026 -- ABERTA**. O que esta aprovado e o fechamento do mes CIVIL de agosto, e e isso que a tela mostra. A tranca das 12:18 nao tinha nada a fazer com ela. Medida da borda: **241 perguntas vivas** de dias 21-31 com mes civil aprovado e competencia aberta (mesma familia da classe 4: mes civil x competencia).
  - **(4) CALENDARIO-UNICO (corte Ronald 16:0x, entrou nesta fatia)**: o unico gate de "mes fechado" passa a ser a COMPETENCIA. A lei (`ponto/janelas.competencia_fechada`) lia o FechamentoMensal do MES CIVIL -- por isso o dia 31/08 batia no fechamento de agosto (aprovado) e barrava, com a competencia 09 aberta. **As 241 perguntas de 21-31 destravam com a cura, sem passivo.** O painel da inteligencia (6 sitios, ja declarados no registro) passa a ler `fechamentos_da_competencia_corrente`, e o pendente sai do registro. Selo de arvore: leitor de fechamento por mes civil fora da lei = vermelho.
  - **(3) achado de contador (BUG)**: **12 perguntas seguem vivas com o dia em competencia TRANCADA** (4 chamados da empresa 4, competencia 07/2026) e o contador `chamados_em_competencia_trancada` diz **0**. Elas pendem de chamados **resgatados depois do fechamento**, que a tranca deixa com o admin de proposito -- mas o contador nao os via. Cura na fatia: `medir` ganha `vivas_apesar_da_tranca` e o contador soma isso em `ficam_com_o_admin`. RED vermelho (4) e GREEN limpo. **Nada a aplicar em dado: nao e passivo, e contador.**
- **CLASSIFICACAO-554 (Ronald 15:3x; sombra, so leitura) -- FECHADA 15:4x**: cartao (competencia 21/08-20/09, o modo em que a admin recebe) x TXT simulado x celula, nos 554 da certificacao. **Nenhuma coluna ficou fora das classes conhecidas.** Saida: `app/docs/smoke150/classes_554.csv` (nome mascarado) e `classes_554_nomes.csv` (nomes reais, para o Ronald; fora do git).
  - **Quem entra no TXT hoje: 129 de 554.** Fora: 409 por furo no espelho, 13 por rescisao (modulo proprio), 2 por ferias com batida, 1 sem codigo Dominio. Para os retidos nao ha coluna a comparar -- e por isso que a fatia CARTORIO-SO-CHAMADO-DE-BATIDA (21 saem da retencao) vem primeiro.
  - **Entre os 129 que entram**: AN sem diferenca 127 · atraso sem diferenca 121 · HE sem diferenca 70 · intrajornada sem diferenca 56.
  - **Classes com diferenca**: faltas de dia futuro (classe 1) **105**; isento Art. 62 (classe 2) **14 em faltas**, 1 em HE e 1 em intrajornada; plantao em dia de folga (classe 3) **4 em intrajornada, 2 em atraso, 1 em HE e 1 em AN**; turno antes da admissao (classe 5) **1 em AN**; falta apurada sem lancamento **2**.
  - **Rotulos onde os dois lados concordam** (nao e divergencia, e o numero que ja sai assim): bug A **57 em HE**; T4/T8a **6 em atraso**; intrajornada por lei (Art. 71 par. 4) **68**, dos quais **58 com a escala sem pausa cadastrada** -- ou seja, a classe de CADASTRO mais comum da casa.
- **BO admin 17/09 -- "valido e nada acontece" (colab 769, medido 15:2x, so leitura)**: a admin corrigiu a resposta para 11:13 e validou as 15:19; a trilha diz o motivo exato -- *"batida vizinha #97036 (S 10/09 19:03) tem o mesmo tipo do plantio (S) - par quebrado; nao materializado (guarda de paridade)"*. A pergunta irma (volta do intervalo) segue viva, entao o chamado nao anda. **A raiz e DADO/ESCALA**: a escala dele e 12x36 das 10:00 as 22:00 (pausa 13-14), mas ele bate ~06:55 e sai ~19:00. A ata mostra o marco das 10:00 aceso por uma batida de tipo S (10:20) e o das 22:00 por 18:58; as batidas de 06:5x ficam orfas. Por isso o calendario mostra numeros enormes: no mes, **150 min de atraso e 1.943 min (32 h) de saida antecipada**; em 08/09 atraso 77 + antecipada 882, em 10/09 74 + 881, em 12/09 antecipada 180. **Pauta 202 (supervisao)** pergunta as duas coisas: o horario real e 07-19 (trocar a escala) e/ou os tipos das batidas estao trocados no app. **P7.1 DE TELA (bug provado, entra na fila de tela)**: o aviso "nao vai gravar" aparece SEM o motivo e SEM porta de acao -- o motivo existe na trilha, mas a admin nao o ve nem tem botao para corrigir o tipo da batida vizinha. Regra do Ronald: todo aviso de "nao gravou" diz o motivo E o que fazer.
- **CREDITO-NO-PROPRIO-DIA -- DIFF por par fechado (sombra, 15:3x)**: **so 2 colaboradores mudam** -- col567 atraso 0 -> 0,60 h e col104 0,37 -> 0,38 h (os 36,8 min que o credito de outro dia apagava). **TXT: 244 linhas nos dois lados, sha identico nas 3 empresas** (os dois estao retidos hoje, entao nenhuma linha muda). Primeira versao da cura somava tambem os turnos em dia de folga e mexia em 13 colaboradores: corrigida para tirar SO o credito, mantendo a base do motor.
- **CARTORIO-SO-CHAMADO-DE-BATIDA -- DIFF por par fechado (sombra, 15:1x)**: celulas `cobrado` **217 -> 84** (as 84 que sobram tem chamado de batida vivo, cobranca de verdade); retidos entre os alvos **98 -> 77**, com **21 saindo e nenhum entrando** (80, 100, 159, 284, 306, 342, 388, 399, 418, 424, 472, 473, 668, 719, 744, 761, 844, 854, 887, 889, 918); TXT da empresa 2 **92 -> 108 colaboradores e 164 -> 208 linhas**, empresa 3 **38 -> 43 e 71 -> 85**, empresa 4 igual; total **244 -> 302 linhas**. Nada mais se move. RED vermelho (3 erros) na arvore do ar, GREEN limpo na cura, suite completa verde. **A cadeia esta montada e PARADA esperando o seu "!"** (raia dinheiro, deploy com ensaio da sombra, nunca --sem-sombra).
- **CREDITO-NO-PROPRIO-DIA (classe 6, raia DINHEIRO, atras da CARTORIO-SO-CHAMADO-DE-BATIDA -- corte Ronald 15:1x)**: a declaracao parcial paga passa a abater **so o atraso do proprio dia**, e o que sobra nao vira saldo nem cruza dia. Juiz unico novo (`ponto/services/credito_parcial.py`) lido pelo fechamento (folha) e pelo coletor do espelho (cartao) -- era exatamente o ponto em que os dois divergiam. **MEDIDO em prod**: 18 colaboradores tem declaracao parcial paga na competencia 09/2026 e em **2 deles o credito cruza dia, apagando 36,8 min de atraso** (col567 36 min, col104 1 min). RED vermelho (3 falhas) e GREEN limpo; DIFF por par na sombra em curso.
- **CAUDA-TRILHA-DO-CARTAO (raia TELA, lancada 15:08)**: o cartao em lote e o TXT passam a gravar trilha (quem, quando, filtro, ids, quantos) e o cartao **informacional** -- o que a maioria recebe -- ganha rodape com quem gerou, o periodo e a versao do sistema; o auditavel ganha as mesmas duas informacoes. Selo com o caso que morde: dois filtros = duas trilhas diferentes, e sem usuario o rodape nao inventa nome. RED vermelho conferido na arvore do ar.
- **AVALIACAO ADMIN -- 5 exemplos (Ronald 14:2x; so leitura; competencia 09/2026 ate 16/09; TXT simulado na sombra das 04:15 numa transacao que volta, cartao e celula por dia)** -- ex1 = colab 30 (emp 4), ex2 = 567 (emp 2), ex3 = 576 (emp 2), ex4 = 51 (emp 4), ex5 = 712 (emp 4). **Nenhuma HE ou intrajornada sem batida nem lei (classe d = 0).**
  - **30** (12x36 noturno 19-07, escala sem pausa, bate pausa 02-03): TXT HE 0, intra 0, AN 77,98 h = cartao. AN 6:00 por noite: janela 22-05 menos a pausa das 02-03; a CCT da empresa 4 afasta a hora reduzida no 12x36 e nao prorroga depois das 5h (relogio = impresso) -> (c) regra da CCT. HE 50%: nao existe no cartao nem no TXT. *Admin:* "noite de 6 h de adicional porque a pausa das 2 as 3 cai dentro da janela noturna; nao ha hora extra neste mes."
  - **567** (12x36 diurno 08-20, pausa 12-13 batida todo dia): TXT e cartao com HE 0, intra 0, AN 0. A queixa nao se reproduz no cartao nem no TXT -- falta saber em que tela a admin viu. *Admin:* "no cartao e no arquivo da folha nao ha hora extra nem adicional noturno neste mes; em que tela apareceu?"
  - **576** (12x36 noturno 19-07, escala **sem pausa cadastrada**, bate so entrada e saida): HE 50% de 0:02 a 0:05 por plantao (0,67 h) = excedente de 12 h sem a tolerancia de 10 min -> **(a) bug A** (Pautas 93-95). Intra 1:00 por plantao (12 h) = **(b) Art. 71 par. 4**: a escala nao tem pausa e o colab nao bate pausa. AN 9:00 no relogio (22-05 + prorrogacao 05-07, Sum. 60 II ligada na CCT da empresa 2) = 10:18 reduzido, impresso e no TXT -> (c) rotulo. *Admin:* "os minutos de HE sao o bug A (sai depois do fechamento); a 1 h de intrajornada e lei porque nao ha pausa registrada nem cadastrada; adicional de 10h18 = 9 h de relogio com a hora reduzida."
  - **51** (6x1 07-16, escala **sem pausa, jornada 9 h**, colab bate pausa 12-13 todo dia): HE 50% 1:01 em 25/08 (saida 18:00) e 0:13 em 12/09 (sabado, saida 11:20, passou dos 10 min) -> batida + lei. Intra 0:23 em 14/09 (pausa de 37 min) -> **(b)** Art. 71 par. 4, pausa curta batida. **Cadastro**: a escala sem pausa e com 9 h de jornada esconde 1 h de HE em 25/08 (com a pausa cadastrada seriam 2:00). **Feriado 07/09 trabalhado (4 h) com celula de folga**: o cartao mostra 4 h de HE 100%, o TXT nao -> **classe 3** do smoke (plantao em folga), dinheiro para o DP. Em 24/08, 09/09 e 16/09 as batidas vieram E E S S (tipo trocado): o dia conta so 4 h -> dado.
  - **712** (5x2 07-17, pausa 11:00-12:12): HE 50% + 100% = 9,78 h = TXT; vem de 03/09 (07:01-20:56) e 04/09 (07:01-19:28) sem pausa batida -- ate 2 h por dia a 50%, o resto a 100% pela CCT -- mais 4 dias com 12 a 22 min de excedente (passou dos 10 min). Intra 2:00 = esses mesmos 2 dias sem pausa batida (escala COM pausa) -> **(b)** o colab nao bateu a pausa. AN 1:08 no cartao em 29/08 (batidas de madrugada em dia de folga) e 0 no TXT -> **classe 3**. *Admin:* "a intrajornada sao os dias 03 e 04/09, jornadas de 14 h e 12 h sem pausa batida; a HE vem desses dois dias e de 4 saidas mais de 10 min depois do horario."
  - **PROVA DO PDF DA ADMIN (Ronald 14:3x)**: a admin gera os cartoes por `POST /relatorios/espelho/lote/` (16/09 15:30; 17/09 08:36, 08:52, 09:51, 12:24-12:29). O log nao guarda os parametros. **Nenhum cartao auditavel foi registrado desde 02/09**, e **394 dos 556 ativos tem disputa aberta** (567, 51 e 712 entre eles) -- para esses o lote cai no cartao informacional, sempre na competencia 21/08-20/09. Reproduzido na sombra como o mesmo usuario (652), nos dois modos do formulario ("Datas livres" 21/08-20/09 e "Mes fechado" 09/2026): `app/docs/smoke150/admin/` (2 PDFs + `comparativo_5.json`). Coluna a coluna (cartao x TXT simulado x celula):
    - **30**: cartao = TXT em trabalhadas (142,95), HE (0), AN (77,98, relogio = impresso pela CCT) e intra (0); so difere nas **2 faltas de 18 e 20/09 (classe 1, dia futuro)**. Celula: entra no TXT hoje. **Nao ha HE 50% no cartao.**
    - **567**: cartao **sem HE e sem AN** nos dois modos; mostra **Atraso 0h36** (06/09 0h11, 12/09 0h25) e 2 faltas futuras (classe 1). TXT: atraso 0. **Classe 6 (nova, dinheiro)**: a declaracao parcial paga de 10/09 (190 min) abate no fechamento o atraso do MES inteiro -- credito de um dia apagando atraso de outros dias; o cartao mostra o atraso. A HE/AN que a admin citou nao sai deste cartao. Celula: **retida** hoje (furo em 31/08, cobranca viva em 12/09, trabalho em dia coberto em 10/09) -- nao entra no TXT.
    - **576**: competencia = TXT (HE 0,67; AN 123,54 = 108,10 de relogio; intra 12; falta 11/09); a mais so as faltas futuras de 17 e 19/09 (classe 1). Em "Mes fechado" sai o **mes civil 01-30/09** (classe 4): HE 0,40, AN 72,07, intra 7, 8 faltas (5 delas depois de hoje). Celula: entra no TXT.
    - **51**: HE 50% 1,25 e intra 0,38 = TXT; **HE 100% 3,99 (feriado 07/09 em celula de folga) so no cartao = classe 3**; 3 faltas futuras (classe 1). Celula: retida (furo 25/08).
    - **712**: HE 50%+100% 9,78 = TXT; intra 2,00 = TXT; **AN 1,13 (29/08, folga) so no cartao = classe 3**; faltas 31/08 e 01/09 (cobranca viva, o TXT retem ate decidir) + 17-18/09 (classe 1). Celula: retida.
  - **REPRODUCAO EM PROD (so leitura, 14:5x)**: a view do lote foi chamada como o usuario 652 dentro de transacao `READ ONLY`, com a gravacao do registro auditavel interceptada -- nenhuma escrita. Os PDFs de prod estao em `app/docs/smoke150/admin/` (`prod_cartoes_5_*.pdf`, `comparativo_5_prod.json`, `comparativo_5_sombra.json`). **Os parametros do POST dela (ids, modo, datas) NAO existem em lugar nenhum**: o access log guarda so a rota (08:36 u28, 08:52 u652 com 8,6 s depois de 197 buscas por nome, 09:51 u28, 12:2x u653) e nao ha LogAuditoria do cartao. E o que a CAUDA pede.
    - **prod x sombra das 04:15 (os 5)**: so o colab 30 mudou -- trabalhadas 142,95 -> 153,95 e AN 77,98 -> 83,98, porque a **saida de 16/09 entrou depois do dump** (o turno estava aberto as 04:15). Nos colabs 51 e 712 sumiu a "falta" de 17/09: eles bateram hoje. Os demais numeros sao identicos.
    - **classe 4 na pratica**: com "Mes fechado", quem NAO tem disputa aberta sai no mes civil (30 e 576: 01-30/09) e quem tem sai na competencia (567, 51, 712: 21/08-20/09) -- o mesmo lote, dois periodos. Em prod o colab 30 passou a sair no mes civil (AN 48,00 e 7 faltas) porque o turno dele fechou; na sombra ele ainda saia na competencia. **394 dos 556 ativos tem disputa aberta**, entao a maioria dos cartoes sai na competencia.
    - **prod x sombra nos 146 do smoke** (mesmos ids, mesma view, `prod_cartoes_146.json` e `sombra_cartoes_146.json`): na competencia so muda o que o dia de hoje trouxe -- 30 colabs perderam a "falta" de 17/09 (bateram hoje) e 3 ganharam horas (turno de 16/09 que fechou depois do dump; colab 76 tambem ganhou 8 h de AN e 1 h de intra). **Nenhuma diferenca de regra entre os dois bancos.** Em "Mes fechado", 106 dos 146 saem no mes civil em prod contra 92 na sombra: 14 trocaram de periodo porque a disputa deles abriu ou fechou nesse meio-tempo -- toda a diferenca de HE/AN/intra dessa coluna e efeito do FILTRO (classe 4), nao de dado.
    - **567 (a pergunta direta): o PDF dela NAO tem HE nem AN**, nem em prod nem na sombra, nos dois modos. Conferido tambem na competencia 08/2026 (HE 0, AN 0, intra 1,00), na 07/2026 (sem movimento) e no mes civil de 08 e de 09: **em nenhum periodo aparece HE ou AN para ela**. O que o cartao dela mostra de diferente da folha e o **Atraso de 0h36** (classe 6) e as faltas de dias futuros (classe 1). Se a admin viu HE/AN, foi em outra tela ou em outro colaborador -- preciso do print ou do nome.
  - *Admin (em lingua dela):* "o cartao da Alessandra (567) nao tem hora extra nem adicional noturno; os 36 min de atraso nao estao indo para a folha porque a declaracao do dia 10 esta abatendo -- vamos corrigir; as faltas de 17 a 20/09 no cartao sao dias que ainda nao aconteceram."
  - **Pautas a escrever** (fila BO): cadastro da escala do colab 51 (pausa + jornada); DP: 03-04/09 do colab 712 (houve pausa?) e o feriado do 51 (classe 3).
- **SMOKE-PORTAS-150 v2 -- criterio unico (Ronald 13:5x) -- FECHADO 14:05** (so leitura, na sombra das 04:15, nenhum deploy)
  - **Criterio**: colab sem nenhum marco apagado na ata de 21/08 a 16/09; dia de folga, ausencia ou feriado (pelo juiz do dia) conta limpo; pergunta viva, troca de vinculo ou de escala nao excluem. Universo = os 554 da certificacao de 09.
  - **Sem furo no periodo: 146/554 na sombra das 04:15** (emp 2 92/414, emp 3 44/120, emp 4 10/20) -- **178/554 em prod as 13:5x** (emp 2 114/414, emp 3 51/120, emp 4 13/20; o cartorio seguiu julgando depois do dump). Todos os 146 entraram na amostra (teto 150); estratos e ids em `selecao.json`.
  - Portas do admin na ordem dele (Recalcular -> Gerar TXT -> Cartoes em lote), como gestor, numa transacao que volta: exportacoes 0 -> 1 -> 0, aprovados 0 -> 92/38/8 -> 0, trilha 498.104 -> 498.105 -> 498.104; fechamentos 09 antes 0, depois 0.
  - **Certificados pela porta: 7/146.** Tirando so os dias que ainda nao chegaram (17-20/09): **84/146**.
  - **Retidos com a celula limpa: 41** -- o juiz do export retem por furo no espelho um colab sem marco apagado (ids em `classes_por_colab.json`). Os dois juizes discordam. Autopsia na fila BO.
  - **Classe 1 -- falta em dia futuro** (78): o cartao conta como falta 2-3 dias de 17-20/09; o TXT nao. Teto temporal. Codigo (cartao).
  - **Classe 2 -- isento Art. 62** (14: 41, 128, 142, 181, 186, 216, 225, 234, 257, 291, 379, 427, 440, 642): TXT zerado (certo), cartao com o mes inteiro de falta (e HE no 41). Codigo (cartao).
  - **Classe 3 -- plantao em dia de folga da celula** (6: 251, 282, 325, 454, 478, 512): o cartao soma o turno, a folha nao, e ninguem paga HE dele. Codigo + **pergunta de dinheiro para o DP**.
  - **Classe 4 -- cartao em mes/ano mistura periodos** (mes civil para quem nao tem pendencia, competencia para quem tem). Codigo (cartao).
  - **Classe 5 (nova, 1 caso) -- turno antes da admissao**: colab 935 admitido em 07/09 tem uma noite em 05/09 no cartao (9,11 h de noturno) que a folha nao paga. Dado (batida antes do vinculo) -> Pauta DP.
  - Classes conhecidas (bug A, T4/T8a, AN reduzido): nenhuma diferenca cartao x TXT atribuida a elas.
  - **Proximo (fila BO, atras da estrutural):** RED das classes 1, 2 e 4 (cartao; raia TELA); autopsia dos 41 retidos; Pautas DP das classes 3 e 5.
  - **Arquivos** em `app/docs/smoke150/` (nomes mascarados da sombra, ids reais; fora do git): TXT dos 146, cartoes em lote nos dois modos, `classes_por_colab.json`, `selecao.json`, `resultado.json` (dia a dia de cada divergencia), `contraponto.py.txt` + `contraponto_comandos.txt` (146 linhas; so leitura). A v1 (91 colabs, criterio antigo) fica no scratchpad.
  - **Contador no placar -- fatia SEM-FURO** (na esteira, atras da estrutural): `colabs_sem_furo_no_periodo=N/554`, dono admin, fora do total do Code; mesma funcao desta selecao, universo da certificacao lavrada, ate ontem.
  - *Para a admin:* "estamos conferindo o cartao-ponto contra o arquivo da folha antes do fechamento; ate o dia 20 o cartao pode mostrar como falta dias que ainda nao aconteceram, e o cartao dos isentos mostra faltas que a folha nao manda -- ignore os dois por enquanto."

- **Os 4 furos de 16/09 que seguem fora (medido 12:5x, so leitura):** #22474 ja tem pergunta viva ligada (29908) -- nasceu depois da primeira leitura. **#22887, #22891, #22927 -- BUG PROVADO de codigo:** a ata acusa a volta do intervalo e a fabrica tira o motivo certo (`intervalo_volta`), mas o escritor (`gerar_perguntas_disputa`) so deduplica por DIA os motivos da lista `MOTIVOS_QUE_PRECISAM_CHAMADO_DESTINO_SET` (ancora_ausente, orfao_14h, saida_sem_entrada); os de INTERVALO caem no dedup "uma pergunta por disputa e motivo". Prova: colab 890 tem uma disputa so (3102, aberta) com seis perguntas de volta do intervalo de outros dias (08/2026) -- a de 16/09 nunca nasce; o DRY da fabrica para o dia da 6 alvos e 0 perguntas. **Fatia INTERVALO-DATADO** (fila BO, atras da estrutural): intervalo_saida e intervalo_volta deduplicam por dia, como os outros motivos datados; antes, medir quantos dias com o intervalo acusado estao sem pergunta por isso. **Medido 13:2x: 1.107 marcos de intervalo acusados sem pergunta na competencia corrente, 142 colaboradores** (emp 2: saida 488 + volta 498; emp 3: 55 + 56; emp 4: 5 + 5). Fatia montada (a lista dos auditores nao muda; a nova vale so para o dedup do escritor), em testes; **espera o seu "!"** -- o lote nasce no cron de amanha as 06:38, sem push.

## DUAS RAIAS (corte Ronald 17/09 08:1x)

- **12:37 FABRICA-PELA-ATA NO AR** (168073c1) e **lote aplicado com o seu "!"**, sem push: **957 perguntas, 8 disputas e 8 chamados**. Contador `chamados_vivos_sem_pergunta_no_app` 153 -> 141. Dos 10 furos de 16/09 que so a ata alcancava, **6 chegaram ao app** (#22852, #22857, #22922, #22924, #22925, #22926); **4 seguem fora** com a causa "fabrica nao rodou" (#22474, #22887, #22891, #22927) -- medicao na fila BO. *Para a admin:* "mais perguntas dos dias com furo foram para o app, sem notificacao".
- **12:24 FORM-CATALOGO NO AR**: "Nova solicitacao" do admin com 7 areas (supervisao, dp, rh, cadastro, ti, seguranca, hasner), todas com "Outros"; Beneficios grava o modulo `beneficio`; em "Colaborador" o tipo vem do catalogo (ausencia, esclarecimento de um dia, regularizacao externa, agenda do dia, mensagem do DP), com o dia do fato quando o tipo pede. **Esperando o seu smoke.** *Para a admin:* "em Nova solicitacao ha RH, Cadastro e Suporte Hasner; em Colaborador escolha o tipo e, quando pedir, o dia".
- **12:13 TRANCA-DE-VERDADE NO AR** (0ebf3fed) -- a regra vale para toda tranca daqui em diante (trancar fecha, reabrir devolve). **Passivo, DRY por empresa x competencia -- espera o seu "!":**

  | empresa | competencia | chamados | perguntas (respondidas -> Pauta DP) | disputas | acoes |
  |---|---|---|---|---|---|
  | 2 | 08/2026 | 108 | 158 (145) | 12 | 278 |
  | 3 | 07/2026 | 8 | 28 (26) | 2 | 38 |
  | 3 | 08/2026 | 54 | 35 (35) | 1 | 90 |
  | 4 | 07/2026 | 0 | 6 (6) | 1 | 7 |
  | 4 | 08/2026 | 4 | 10 (10) | 3 | 17 |

  Contador `chamados_em_competencia_trancada` = **430** (esperado 0 depois do apply).
  **12:18 APLICADO com o "!" (5 linhas):** 428 acoes (emp 2 08/2026: 276 de 278 -- as 2 que faltam ja tinham saido por tabela quando chegou a vez delas: a disputa fecha junto com o chamado-pai); **Pautas DP 187 (emp 2 08), 188 (emp 3 07), 189 (emp 3 08), 190 (emp 4 07), 191 (emp 4 08)** com os ids das respostas. **Contador = 0** (chamados, perguntas e decidir_pelo_dp). O balao se relavra no ciclo de 5 minutos. **Esperando o seu smoke no balao.** *Para a admin:* "os chamados de meses fechados sairam da fila; o que precisa de decisao esta nas Pautas do DP". Com o "!": cinco comandos, um por linha, cada um com a sua Pauta DP; nada toca ata, celula ou folha. `registro_chamado` = 48 (esta fatia nao mexe no registro).
- **11:59 CRON-VIGIA NO AR**: cron de producao que morre (saida diferente de 0 e de 2) abre na hora uma Pauta de sistema para TI e acende `crons_quebrados` no placar (esperado 0); a pauta fecha sozinha quando o cron volta a terminar bem.
- **12:0x -- o vigia ja acusou: `crons_quebrados=9`, e e bug provado.** Oito crons morreram com exit 137 (processo derrubado): sete as 12:00 (detectar_ausencias, processar_alertas_turno, processar_alertas_avancados, reconciliar_chamados, reconciliar_fantasmas, lavrar_badge_navbar, alertar_chamados_sla, reavaliar_ausencias_lancadas emp 2) e o escalonar_documentos_ausencia das 07:51 -- **todos no minuto de um deploy**: o reinicio do `saas_core` derruba o comando que estava rodando dentro dele. Os de 5/15/30 minutos se recuperam sozinhos na proxima corrida (e a Pauta de sistema fecha). **Causa de fundo, minha:** desde a raia TELA (08:1x) cada fatia reiniciava as cascas tres vezes (copia, desfazer, deploy) pelo `--sem-sombra`. **Fatia DEPLOY-ESPERA-CRON (bug, depois da TRANCA-DE-VERDADE e da LOGIN-APP; so host):** o envelope `cron_run.sh` marca o cron em curso e o restart do deploy espera ele terminar (ate 10 min); a raia TELA recarrega a copia pelo HUP gracioso (`DEPLOY_SEM_SOMBRA` no `--reload-copia`), que nao derruba comando. O contrato do deploy acusa as tres coisas na arvore de hoje (RED 3). As cadeias que ainda nao comecaram ja recarregam a copia pelo HUP; so o deploy final reinicia.
- **11:59 FABRICA-PELA-ATA -- copia desfeita, sem estrago:** a regua nao chegou a rodar ("template database test_juliani does not exist": o banco de testes sumiu no meio da criacao, colisao com outro processo). Relancada as 12:0x; o "!" segue valendo.
- **11:47 TRANCA-TELA NO AR**: dia de competencia trancada mostra "competencia MM/AAAA fechada -- decisao pelo DP (Pauta)" com o botao "Abrir Pauta DP", no lugar do Resolver dia; a recusa do Resolver dia diz o mesmo. **Esperando o seu smoke.** *Para a admin:* "dia de agosto (mes fechado) agora diz que a decisao e do DP e tem o botao da Pauta".
- **11:34 SOLIC-FLAG NO AR**: "Solicitacoes" e "+ Nova solicitacao" no app por empresa, desligados por padrao (chave `app_solicitacoes` = 1 liga); quem ja tem solicitacao ve a aba. **Esperando o seu smoke.** *Para a admin:* "o botao de nova solicitacao sumiu do app; quem ja tinha solicitacao continua vendo a aba".
- **ORDEM (Ronald 11:0x) -- nada novo na frente sem bug provado:** TRANCA-DE-VERDADE -> fila da manha (C5-AUDITOR, UIFIC3, UIFIC-FRONT, C5-EMISSORES, C5-TELA, F7, PROPOSTA-EVIDENCIA, F7B, CAUDA-G, C1-MSGDP) -> RESPOSTA-TARDIA -> INTEL-JUIZ -> C5-CANAL -> AGIR-POR-DONO -> WORKLIST-ATA -> fabrica por evento. **`registro_chamado` (alvo 0), a cada fatia:** 11:0x = **48** (C5-AUDITOR copiada, na regua; 50 antes dela) · 11:01 C5-AUDITOR NO AR (fcbfc867) = **48**. *Como a ordem e garantida:* a trava da cadeia nao e fila; a fila da manha restante e a FORM-CATALOGO so entram depois da TRANCA-DE-VERDADE (portao por arquivo). As quatro que ja esperavam a vez antes da ordem (SOLIC-FLAG, TRANCA-TELA, CRON-VIGIA, FABRICA-PELA-ATA) podem passar antes dela.
- **10:47 COBRAR-DIA NO AR** (liberado por voce): botao "Cobrar este dia (pergunta no app)" na caixa do Resolver dia. **Esperando o seu smoke.** *Para a admin:* "no calendario, em Resolver dia, o botao Cobrar este dia pede ao colaborador pelo app quando o sistema nao cobrou".
- **TRANCA-DE-VERDADE (P7.1, 10:5x) -- medido e em testes, na frente da fila:**
  - **Medido (so leitura):** competencias trancadas = 08/2026 (emp 2, 3, 4) e 07/2026 (emp 3, 4); emp 2 07/2026 e as 06/2026 NAO estao trancadas. Com dia nelas: **172 chamados vivos** (43 na fila: validar 24, decidir 12, cobrar 7; 38 sem verbo; 64 arquivados; 2 historico) e **249 perguntas vivas** (224 respondidas, 25 sem resposta).
  - **Por que o contador dizia ~0:** ele contava so o que a regra de 15/09 podia encerrar (hoje 6 chamados + 15 perguntas). A regra deixava a pergunta RESPONDIDA "com o admin" -- o juiz dava a resposta do colaborador como mais forte que qualquer carimbo -- e so via o dia pela chave do catalogo (566 chamados "sem dia" so na emp 2).
  - **A lei na fatia:** o juiz mata a pergunta de competencia trancada ANTES da resposta (sai do app, do balao, de Validar/Decidir; sem lastro, a resposta fica na trilha); a regra ve o dia pelo juiz e fecha tambem as respondidas; os ids delas viram UMA Pauta DP por empresa e competencia; reabrir a competencia devolve tudo com trilha; o contador soma tudo que esta vivo contra a lei. Ficam, nomeados: o resgatado pelo admin, o aviso de cadastro (corte 16/09) e a pergunta viva de outro dia.
  - **Depois do deploy:** DRY do passivo por empresa x competencia -> **"!" seu** -> apply; depois **smoke seu no balao**. *Para a admin (depois do apply):* "mes fechado nao aparece mais na fila; o que precisar de decisao esta nas Pautas do DP".
- **10:28 VALIDAR-DIZ-O-SEU NO AR** (17b25997): validar uma pergunta diz o resultado dela; pergunta irma travada de outro dia vira aviso a parte, com os ids. *Para a admin:* "ao validar um dia de setembro, se aparecer aviso de agosto, o seu dia foi gravado -- o aviso e das perguntas de 29/08 (agosto fechado, com o DP)".
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
  - **10:21 PRONTA, esperando o seu "!" -- DRY na sombra (copia de prod das 04:15, antes do lote de hoje):** arvore do ar = 807 perguntas em 141 colaboradores (o lote que ja foi aplicado as 09:16); **arvore nova (pela ata) = 2.821 alvos em 247 colaboradores, dos quais 1.654 perguntas nasceriam** (o resto ja tem a pergunta do mesmo fato) -- por empresa: emp2 2.498 + 4 fio mudo, emp3 309 + 3, emp4 7. Celulas que acusam: 1.833 pela lei x 6.035 pela ata (a ata acusa cada marco apagado, a lei so um motivo por dia). **Liquido sobre o que ja nasceu hoje: ~850 perguntas a mais, em ~106 colaboradores a mais.** Com o "!", a fatia sobe e o lote nasce no cron de amanha as 06:38 (sem push, como o de hoje) -- ou aplico na hora, se voce disser.
  - **10:56 "!" do Ronald (--apply sem push):** fatia liberada; o lote nasce na propria cadeia, logo depois do deploy, com o contador antes/depois.
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
  - **Corte Claude 10:2x -- duas fatias novas:** **WORKLIST-ATA** (familia chamado): a worklist de inconsistencias le a ata (`lampadas_sem_ata` -> 0), depois da C5-TELA, que mexe no mesmo arquivo. **TELA-MARCO** (fila de tela, depois da familia chamado): calendario, app e fio mostram o marco faltante com "?" pela ata, como o espelho do admin ja faz.
  - **10:1x -- familia chamado montada** (todas esperando a vez, em sequencia pelo registro): RESPOSTA-TARDIA, INTEL-JUIZ, **C5-CANAL** (canal partido, selo de data divergente do fio, mapa_divergencia e a justificativa leem o dia pelo juiz; o detector deixa de acusar leitura de formulario; universo antes/depois medido na sombra antes de subir), **AGIR-POR-DONO** (departamento dono do modulo; colab e cadastro -> supervisao, sistema -> TI; gestor geral age em tudo; lotes dizem quantos ficaram fora; 0 acoes das ultimas 48h seriam recusadas). Leitura do item (6): nao e formulario de abertura, e a justificativa do turno -- os modulos que ela liga vem do catalogo (`COBRANCAS_DO_TURNO`) e o dia do juiz.
  - **Ordem da familia chamado:** RESPOSTA-TARDIA -> INTEL-JUIZ -> C5-CANAL -> AGIR-POR-DONO -> WORKLIST-ATA; TABULEIRO e TABULEIRO-HAIKU correm ao lado (arquivos proprios); depois TELA-MARCO.
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

- 17/09 17:07 deploy agendado manha17, fatia c1msg: rc=2 -- C1M-FIM
- 17/09 16:11 deploy agendado manha17, fatia caudag: rc=0 -- copia DESFEITA e recarregada: deploy: OK -- migrations em dia, tres cascas reiniciadas juntas, tres rotas provadas.
- 17/09 15:51 deploy agendado qui1709, fatia fcm: rc=0 -- FCM-FIM
- 17/09 15:43 deploy agendado manha17, fatia f7b: rc=1 -- NAO LANCADA: rodar f7b fim 15:43
- 17/09 15:12 deploy agendado manha17, fatia propev: rc=1 -- NAO LANCADA: rodar propev fim 15:12
- 17/09 14:54 deploy agendado manha17, fatia f7frota: rc=0 -- F7F-FIM
- 17/09 14:41 deploy agendado qui1709, fatia gvr: rc=0 -- GVR-FIM
- 17/09 14:16 deploy agendado manha17, fatia c5tela: rc=0 -- C5T-FIM
- 17/09 13:30 deploy agendado manha17, fatia c5emi: rc=0 -- C5E-FIM
- 17/09 12:59 deploy agendado manha17, fatia uifront: rc=1 -- NAO LANCADA: copia falhou
- 17/09 12:59 deploy agendado manha17, fatia uific3: rc=0 -- UIFIC3-FIM
- 17/09 11:01 deploy agendado manha17, fatia c5aud: rc=0 -- C5A-FIM
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

### Login iOS com conta sem colaborador (BO Fernando 17/09) -- medido, cura na esteira (LOGIN-APP)

- **A conta:** a "conta de teste JSP" usada no iOS e o **usuario 656**, uma conta de **gestao** (superusuario, grupo gestor, supervisao em 4 setores) **sem colaborador ligado** -- sem situacao, empresa ou vinculo. Nao e conta de colaborador.
- **As 3 rotas:** login, device-register e bater resolvem o colaborador pelo MESMO caminho (`request.user.colaborador`) -- nao sao tres juizes. O login tem um ramo proprio para admin sem colaborador: devolve 200 com `colaborador.id = null` e `perfil = "admin"`, e so barra admin no celular pelo User-Agent -- o app iOS se apresenta como `HasnerWK/1 CFNetwork/... Darwin/...`, sem "iphone"/"mobile", entao passou.
- **Reproduzido na sombra** (mesmo token, transacao desfeita): device-register e bater dao 404 "Colaborador nao encontrado." com `X-Hasner-Client: ios`, sem o cabecalho e com `wv` -- **nao depende da plataforma**.
- **Cura (LOGIN-APP, na esteira logo apos a TRANCA-DE-VERDADE):** com o cabecalho `X-Hasner-Client`, o login de conta sem colaborador responde **403 `conta_sem_colaborador`** -- "Conta sem colaborador ativo nesta empresa." (+ `erro_dica`). As tres rotas passam a dizer a mesma coisa para a mesma conta. De carona: `admin_em_mobile` dava 500 (chave errada na tabela de erros). Contrato no HANDOFF.
- **Para o Fernando:** teste com uma conta de **colaborador** (usuario ligado a colaborador ativo com vinculo); a 656 e de gestao. Na tela de login, mostre `erro_msg` e `erro_dica`.
- **Conta de teste iOS por empresa -- espera decisao do Ronald:** um colaborador de teste em producao entra no fechamento e no TXT da folha. Opcoes: (a) usar um colaborador real que o DP indicar, por empresa; (b) criar colaborador de teste com uma marca que o tire do fechamento/TXT (fatia de dinheiro, depois da janela). Nada foi criado.

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
