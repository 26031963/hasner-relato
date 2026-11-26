# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 09:11** · publicado em **26/11/2026 09:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 09:11 · sessao 0de906d0edc9 · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · verificador · origem=Code

**Parecer 5a — reprovo a c4c.** O K1 segue aberto. O K2 fecha a comparação em horas com duas casas, mas a nova soma pode reduzir HE100 de quem não trabalhou em feriado. O K3 preserva o veredito dos dois ramos nos testes de fronteira. A catraca de linhas falhou em três entradas. `app/ponto/motor_calculo_v2.py:3092–3129` · `app/ponto/services/fechamento.py:976–981` · `app/folha/porta_export.py:340–365,506–514` · `app/core/tests/test_medida_turno_do_par.py:562–566`.

Verifiquei a cópia e o diff em somente leitura. Executei em memória o motor do caso K1, as contas de K2, os testes de fronteira de K3 e a catraca por AST. Objetos Git históricos, suíte Django completa e efeito na frota ou no holerite de produção **[não verificado]**. [nao verificado]

## 1. Os três bloqueios da c4b/v1

| Achado | Situação na c4c |
|---|---|
| **K1 — aberto** | Reproduzi `10:00–14:00` + `14:30–18:20`. As duas vias recebem **470 min trabalhados**. A regular grava **30 min de intra e 440 min de HE100 feriado**. A via `periodos_ft` entrega **zero de intra** e promove **470 min de HE100 feriado**. A função de dobra é a mesma e recebe insumos diferentes. `app/ponto/motor_calculo_v2.py:2550–2599,3004–3024,3070–3086,3092–3129,3219–3225` · `app/ponto/services/dia_pago.py:151–170`. |
| **K2 — curado na unidade alternativa do brief** | Para `30,3 + 33,6 min`, FM e soma do DiaPago passam a **1,07 h** nas três HE100. Se a soma da lavra cai para **1,06 h**, o comparador acusa com tolerância zero. A igualdade é de **horas já arredondadas a duas casas**. Uma alteração de `33,6` para `33,9 min` continua dando `1,07 h` dos dois lados. A mudança fora do escopo de feriado é um bloqueio separado, demonstrado no item 5. `app/ponto/services/dia_pago.py:173–197,617–632` · `app/ponto/services/fechamento.py:976–981` · `app/folha/porta_export.py:506–514` · `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:351–387`. |
| **K3 — curado** | Feriado em dobra usa `sum` e dia comum mantém a acumulação incremental da c4b. Rodei os dois testes em memória. No feriado, `331,3` com teto publicado `330,3` é absolvido e `331,4` é acusado. O mutante que leva a acumulação incremental ao feriado acusa `331,3`. O mutante inverso absolve indevidamente o dia comum de `170,3`. `app/folha/porta_export.py:340–365` · `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:421–445` · `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:55–58`. |

**K1 exige autorização no motor.** O ponto de entrada é `motor_calculo_v2.py:3092–3095`, alimentado em `3070–3072` e com escrita em `3127–3129`. O juiz `intervalos_do_turno` entrega **qual vão foi pausa**, e `_intra_minima` entrega **o mínimo**. O cálculo do suprimido do partido, com teste de identidade de turno, duração e tolerância, está no laço. A promoção só recebe o suprimido já escrito no par. O helper do oráculo tem outro contrato, com guarda própria de `trab_min > 360`. A cura admissível é fazer a mesma apuração do motor alcançar os pares elegíveis em `periodos_ft`, ou extrair essa apuração e fazer o motor delegar. As duas opções tocam a zona inviolável. `CLAUDE.md:17–22` · `app/docs/LEIS.md:113` · `app/core/juizes.py:215–223` · `app/ponto/motor_calculo_v2.py:1910–1936,3070–3129` · `app/ponto/calculador/periodo.py:313–337` · `app/ponto/services/dia_pago.py:151–170`.

## 2. Quem escreve e quem lê

| Campo ou valor | Escrita e leitura |
|---|---|
| **Insumos do par, em minutos** | Os motores calculam trabalhado e intra indenizada. `_carimbar_segmento` grava pontas efetivas e intervalos. `_escrever_noturnas` grava relógio e minutos legais no motor v2. `apurar_por_fatia` reúne o mapa de dia e captura a prorrogação do motor de cada par FT. A promoção lê dia, flag, pontas registradas, intervalos carimbados, trabalhado e intra. O adicional noturno lê os **minutos legais**. `app/ponto/motor_calculo_v2.py:1039–1061,1939–1947,2884–2917,3013–3024,3121–3129,3359–3378` · `app/ponto/services/fechamento.py:302–335` · `app/ponto/services/dia_pago.py:137–169,247–252,289–291`. |
| **HE100 regular e promovida, em minutos** | O motor escreve `minutos_extra`, `minutos_extra_100`, `minutos_extra_100_feriado`, `hora_extra_100` e `minutos_extra_100_noturna` na dobra regular. `ResultadoMes`, `he100_por_dia` e o vigia leem os campos pertinentes. Na promoção, `dobra_da_folga_feriado` produz um mapa `(HE100, HE100 noturna)` por par, sem alterar o par. O fechamento produz esse mapa uma vez e o entrega à lavra. `app/ponto/motor_calculo_v2.py:323–344,727–763,1885–1901` · `app/ponto/services/dia_pago.py:137–192` · `app/ponto/services/fechamento.py:962–981,1217–1235` · `app/folha/porta_export.py:340–345`. |
| **FechamentoMensal e DiaPago, em horas** | `he100_por_dia` distribui os minutos já apurados em horas por dia. `lavrar` grava as três HE100 como `FloatField`, sem arredondar a rubrica do dia. O fechamento usa a mesma distribuição, agrega por `he100_da_competencia` e grava as três HE100 com duas casas. `horas_extras` soma HE50 à HE100 mensal antes de arredondar. `detalhe.folga_feriado_min` registra **trabalhado bruto promovido**, em minutos, e `pago_do_dia` o converte em horas de trabalho exibidas. `app/ponto/services/dia_pago.py:173–197,312–322,358–359,402–427,959–967` · `app/ponto/services/fechamento.py:976–983,1154–1187` · `app/ponto/models.py:325–337,412–425`. |
| **Comparador e vigia** | `soma_da_competencia` lê as linhas `motor`, inclusive o ajuste, e devolve os totais. `medir` cruza com o FM gravado e publica as diferenças. O vigia de dupla rubrica lê os pares regulares do espelho, agrupa pela data civil da entrada e usa a marca de HE100 feriado do motor. No dia comum consulta previsto e intervalo declarado. `porta_export.lavrar` grava os resultados no carimbo. A dupla rubrica entra em `falhas`. A divergência FM × DiaPago é publicada fora desse total. `app/ponto/services/dia_pago.py:617–632` · `app/folha/porta_export.py:340–365,441–448,506–523,588–603,685–700,725–735,806–831`. |

**Segundo escritor das HE100 pagas fora da porta — ausente no censo.** O FM tem escrita e restauração no serviço de fechamento. A lavra usa a mesma porta para `motor` e `oraculo`, com versões distintas. O retrato da exportada tem escrita própria de grade e ajuste, além de criar linhas com zeros. Existe reescrita de campos de par em `periodo.reler`, declarada como caminho de ensaio. `app/core/portas.py:503` · `app/ponto/services/fechamento.py:1154–1187,1283–1315,1377–1412` · `app/ponto/services/dia_pago.py:389–427,552–612` · `app/ponto/calculador/periodo.py:543–548,754–775,831–850`.

## 3. Classes de reprovação e mordida

- **Segundo juiz — ausente no caminho novo, presente em sítios anteriores.** A promoção consulta `feriado_vira_folga` e usa o dia do mapa. O vigia ainda decide o agrupamento pela data civil e o TXT ainda decide se suprime o evento 12x36 pela régua do fim da competência. Este último sítio consta dos pendentes do registro vivo. O `getattr(..., None)` novo estende à partição do FM a degradação civil já existente em `_dia_de` quando falta o mapa. Alcance dessa ausência na frota **[não verificado]**. `app/docs/JUIZES.md:29–32,114–119` · `app/core/juizes.py:215–223,310–315,495–498` · `app/ponto/services/fechamento.py:943–966` · `app/ponto/services/dia_pago.py:124–134` · `app/folha/porta_export.py:590–601` · `app/folha/export.py:357–380,420–423`.
- **Segundo escritor novo e relógio novo fora da borda — ausentes.** O diff mantém as portas de escrita, lê instantes das batidas e usa a flag da fatia. `medir` já captura o instante na entrada do ato, e `concluir` passa ao carimbo a data da tentativa. O diff acrescenta zero `except`. `app/ponto/services/fechamento.py:302–335,974–981,1154–1207` · `app/ponto/services/dia_pago.py:151–197,402–427` · `app/folha/porta_export.py:368–373,1293–1314` · diff anexo dos cinco arquivos.
- **Derivação paralela — presente em parte.** A distribuição da dobra é compartilhada. A promoção e a escrita do motor ainda montam separadamente a chamada ao split e aplicam o mesmo teto à sua parcela noturna. O vigia deriva seu teto a partir dos insumos. O oráculo ainda usa o trabalhado bruto da folga e pode substituir HE100 total e feriado na própria versão. O extrato por intervalo mantém outra agregação das mesmas horas lavradas. `app/ponto/motor_calculo_v2.py:1063–1078,1885–1901` · `app/ponto/services/dia_pago.py:165–169,389–397,851–858` · `app/folha/porta_export.py:355–364` · `app/ponto/calculador/regras.py:679–688` · `app/relatorios/views.py:523–530`.
- **Unidade e significado — há duas fronteiras relevantes.** HE100 noturna é subconjunto da HE100 em minutos de relógio. `horas_noturnas` é adicional, derivado de minutos legais. A lavra guarda horas sem arredondamento diário, e o mensal reconverte essas horas para somar e arredonda uma vez. `_TOL_H` também é aplicado numericamente aos campos de grade `minutos_*`, onde `0,015` significa **minuto**. Na data, a lavra usa o dia da jornada e o vigia usa a entrada civil, inclusive no filtro 20→21. `app/ponto/motor_calculo_v2.py:319–344,1039–1061` · `app/ponto/models.py:325–351` · `app/ponto/services/dia_pago.py:23–25,173–197` · `app/folha/porta_export.py:245,506–514,590–601` · `app/ponto/janelas.py:10–33`.

**Selos.** As 12 substituições têm âncora única e compilam. A prova de mordida abaixo é aritmética ou execução em memória, conforme indicado. A execução de todos os labels pelo runner Django e dos REDs históricos via Git **[não verificado]**. `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:25–99`.

| Mutante ou RED | Mordida |
|---|---|
| `mensal-le-par`, `lavra-le-par` | A asserção do Opus exige 420 min de HE100 noturna em cada lado. Ler o split residual do par FT troca 420 por 360 no lado mutado. `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:25–33` · `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:222–256`. |
| `sem-noturno`, `dia-civil`, `sem-limite`, `relogio` | Reproduzi nos helpers. FN2 cai de `120/120` para `120/0` nos dois primeiros. FD6 passa de `100/100` para `100/120`. G0.6 troca `270/240` por `270/270`. Pares de números em minutos de HE100/HE100 noturna. `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:34–44` · `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:302–321`. |
| `ultima-fatia`, `sem-intervalos` | Reproduzi 660 min noturnos no controle, 540 usando a flag final para todos e 720 retirando a pausa. `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:45–48` · `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:323–349`. |
| `k2-soma-em-horas`, `k2-tolerancia-he100` | A soma em horas dá `1,06` no controle que exige `1,07`. Repor `0,015 h` absolve a diferença de `0,01 h` provocada pelo teste. **Sobrevive uma implementação errada que arredonde cada dia só na soma mensal da lavra**. Nesse fixture, ela dá `0,51 + 0,56 = 1,07` no controle e `0,51 + 0,55 = 1,06` após alterar a linha. `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:49–54` · `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:360–387`. |
| `k3-acumulacao-no-feriado`, `soma-reordenada`, `red-k1` | Os dois mutantes K3 falharam nas respectivas asserções em memória. K1 é um RED explícito, com método `verificar_...` fora da descoberta normal e sem mutante próprio. Na reprodução, a primeira igualdade a falhar é `470 == 440`. O V1 existente acusa `540/480` na via regular e o vigia exclui a promoção do seu universo. `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:19–23,55–58,80–99` · `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:390–445,483–521` · `app/folha/porta_export.py:590–603`. |

## 4. O `+1 min` do K3

**É um limiar técnico preexistente no código, mantido pela c4c.** O `if _he > _teto_he + 1` vale para ambos os ramos. O registro da obra e o histórico da dupla rubrica descrevem o invariante e o teto. Um aval **LEI/CORTE** para a margem numérica de `+1 min` fica **[não verificado]**. É distinto de `0,015 h` do comparador. `app/folha/porta_export.py:245,327–338,355–365` · `app/docs/BACKLOG.md:1243,1991` · diff anexo de `app/folha/porta_export.py:337–358`.

## 5. Censo adicional e valores que mudam

- **Espelho, PDF, calendário e app.** As rubricas das linhas diárias vêm do DiaPago. O total HE100 é exibido e seu subconjunto noturno fica sem coluna própria. Espelho e PDF ainda chamam `apurar_por_fatia` e somam pares para o resumo do motor, inclusive no período livre. No resumo de competência, `folha_manda` usa o fechamento online ou a soma da lavra quando disponível. O app usa também `horas_do_dia`, que pode mostrar o trabalhado do motor na ausência de linha. `app/ponto/services/dia_pago.py:970–994` · `app/ponto/services/espelho.py:77–117,120–136,823–831,1198–1202,1315–1320` · `app/relatorios/pdf_espelho.py:417–423,494–503,624–628,844–859,901–914` · `app/relatorios/cartao_pela_celula.py:320–346` · `app/colaboradores/services/calendario.py:684–698` · `app/api/views.py:1450–1457`.
- **TXT, painel, ranking e holerite.** O clique **Gerar TXT reapura pela porta do fechamento em modo leitura** e injeta os objetos na montagem. Sem injeção, a montagem lê FM gravado, como no PDF de validação. O TXT deriva HE100 normal por `total − feriado` e emite o adicional de `horas_noturnas`. O painel lê `horas_extras` online. O ranking lê HE100 total e feriado, e a foto de auditoria lê os campos concretos. O holerite serve o PDF armazenado. `app/folha/views.py:209–214,264–270` · `app/folha/export.py:420–448,714–739` · `app/folha/validacao_pdf.py:58–68` · `app/ponto/services/fechamento.py:1778–1780` · `app/ponto/views.py:1018–1036` · `app/templates/ponto/partials/painel_fechamento.html:33–34` · `app/folha/services/ranking_he.py:137–152,206–229` · `app/chamados/services/foto_e4b.py:324–333` · `app/holerite/views.py:1445–1459`.
- **Outros leitores da soma e da porta.** `soma_da_competencia` também alimenta `linha_x_badge` e o resumo de cartão com fonte `lavratura`. O extrato parcial, a Gestão de HE e a Pauta de autorização leem `soma_do_periodo`, que segue com `SUM` de horas. O comando `selo_leitores_no_mesmo_numero` lê e publica ambos os contadores, o cron chama esse comando e o clique do TXT consome o carimbo. `app/folha/porta_export.py:251–282,1293–1314,1392–1414` · `app/relatorios/cartao_pela_celula.py:141–179` · `app/relatorios/views.py:481–487,523–531` · `app/ponto/services/gestao_he.py:255–279` · `app/ponto/services/autorizacao_he_periodo.py:172–189` · `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:127–131,218–240` · `app/config/crons.py:1810–1814` · `app/folha/views.py:124–135`.
- **Há mudança sem feriado, reproduzida.** Dois dias comuns com **10,0 e 31,1 min de HE100 normal**, e 120 min de HE50 em cada dia, passam de **0,69 para 0,68 h de HE100** e de **4,69 para 4,68 h de `horas_extras`**. A c4b soma os minutos dos pares antes de dividir. A c4c passa pelas horas diárias e reconverte para minutos, atravessando o limiar de arredondamento. HE50, adicional noturno e intra têm fórmulas próprias de escrita no FM. O evento de HE100 normal do TXT pode refletir a queda. `app/ponto/motor_calculo_v2.py:323–332,3273–3282` · `app/ponto/services/dia_pago.py:173–197` · `app/ponto/services/fechamento.py:932–934,976–981,1021` · `app/folha/export.py:420–443` · diff anexo do fechamento.
- **Além disso, a soma de leitura fora de HE100 mudou.** Saiu o `SUM` do banco e entrou `+=` em Python sobre linhas sem ordem explícita. Em memória, dez parcelas de `0,10 h` e uma de `0,445 h` dão `1,44` ou `1,45 h` conforme a ordem. Isso alcança os números publicados do comparador, `linha_x_badge` e o resumo com fonte `lavratura`. Diferença efetiva contra o `SUM` anterior em produção **[não verificado]**. `app/ponto/models.py:382–392` · `app/ponto/services/dia_pago.py:623–632` · `app/folha/porta_export.py:251–282,441–448,498–514` · `app/relatorios/cartao_pela_celula.py:141–185` · diff anexo de `soma_da_competencia`.

## 6. Casos fora do golden

| Caso | Resultado |
|---|---|
| **12x36 noturno** | Executei o par `19:00–07:00`, pausa `01:00–02:00`, prorrogação desligada e hora reduzida afastada. Com feriado na cabeça e cláusula de dobra habilitada, dá **660 min trabalhados, 660 de HE100 feriado, 360 de HE100 noturna e zero de intra**. Na régua simples, HE100 feriado é **zero**, com 360 min de adicional no exemplo. O ciclo 12x36 fica fora de `feriado_vira_folga`. `app/ponto/motor_calculo_v2.py:2884–2955,3743,3813–3824` · `app/core/regua_cct.py:466–479` · `app/escala/models.py:1393–1406`. |
| **Virada 20→21** | Com corte 21, o plantão `20/11 19:00 → 21/11 07:00` pertence a novembro. Com dobra habilitada, feriado só em **20/11** dá os 660 min do exemplo. Feriado só em **21/11** dá zero de HE100 feriado nesse plantão. Um plantão que começa em 21/11 pertence a dezembro. O selo existente prova atribuição única de trabalhadas. Em dois pares com volta na madrugada, o vigia ainda pode separar ou descartar a cauda. `app/ponto/janelas.py:10–33` · `app/ponto/motor_calculo_v2.py:953–973,2505–2537,2921–2939` · `app/ponto/tests/test_dupla_contagem_dia20.py:30–44` · `app/folha/porta_export.py:590–601`. |
| **Vários feriados na competência** | Três dias com **30,2 min** dão **1,51 h** com arredondamento mensal e **1,50 h** com arredondamento diário. Outro caso reproduzido em memória pega agrupamento indevido por mês: dia A noturno de 420 min com `S=60`, dia B diurno de 480 min com `S=0`. Por dia dá **840 de HE100 e 360 de HE100 noturna** no mês. Misturar os dias conserva 840, mas sobe a noturna para **420**. Os testes de dois dias atuais usam `S=0` e deixam passar essa mistura. `app/ponto/services/dia_pago.py:151–197` · `app/ponto/motor_calculo_v2.py:735–763` · `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:323–372`. |
| **Intermitente** | O controle de `21/04 07:00–19:00` exige **12 h trabalhadas, HE100 zero e HE100 feriado zero**, inclusive com `feriado_em_dobra=True`. O motor injeta o conjunto de feriados vazio e o vínculo intermitente passa fora da promoção. `app/ponto/tests/test_s6_intermitente_feriado.py:54–56,98–99,115–142` · `app/ponto/motor_calculo_v2.py:1019–1023,2466–2470,2550–2558,3689–3701` · `app/escala/models.py:1400–1406`. |

## 7. Bloqueia × fila

- **Bloqueia — K1.** A mesma jornada dá 440 ou 470 min de HE100, e a via promovida perde 30 min de intra. O ponto para autorização é o laço do motor em `app/ponto/motor_calculo_v2.py:3092–3129`.
- **Bloqueia — escopo de K2.** Há queda reproduzível de `0,01 h` de HE100 normal e de extras totais sem feriado, e a mudança da soma de leitura alcança também as demais rubricas. `app/ponto/services/dia_pago.py:173–197,623–632` · `app/ponto/services/fechamento.py:976–981`.
- **Bloqueia — catraca vermelha.** Linhas registradas → medidas: `medir` **586→594**, `_dia_de` **132→134**, `_so_a_data` **1592→1588**. A catraca de identidades passou em memória. `app/core/tests/test_medida_turno_do_par.py:287,323,329–331,546–566`.
- **Fila — vigia por data civil e sem `periodos_ft`, cálculo do oráculo, filtro 12x36 do TXT e G0.6 split × relógio `240/270`.** `app/folha/porta_export.py:590–603` · `app/ponto/calculador/regras.py:679–688` · `app/folha/export.py:357–380,420–423` · `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:318–321`.

## 26/11 09:09 · sessao 944a5788be74 · O30-F3-CONSTRUIR · verificador · origem=Code

**O30-F3-CONSTRUIR c2 — BLOQUEIA.** A troca direta do anexo aprovado pela filha foi fechada, mas a aprovação em grupo ainda pode travar filha → raiz, e `rederivar_fim` pode gravar minutos e fim calculados sobre uma versão antiga da ausência. A máscara da sombra deixa os seis campos passarem. `app/ponto/services/ausencia.py:283–297,1115–1134,1860–1872`, `app/ponto/views.py:2786–2815`, `app/ponto/models.py:843`, `bin/sombra_regras_pessoais.py:48–78,109,160–164`.

Os dez arquivos alterados da cópia conferem com os blobs finais do diff anexo. Rodei os 21 testes sem banco e 23 casos de mutação em leitura. Hash de commit pelo Git, REDs contra a c1 original, integração com banco, tempo do PDF real e ocorrências em produção [nao verificado]. `app/ponto/tests/mutantes_atestado_f3.py:15–31,194–235`, `app/ponto/tests/test_atestado_f3_contrato.py:87–202,205–470`, `app/ponto/tests/test_atestado_f3.py:330–351`.

## 1. B1 a B4

| Bloqueio | Veredito e prova |
|---|---|
| **B1 — raiz, trava e dinheiro** | **Parcial.** Anexar e reabrir gravam arquivo e dados na mesma referência imediata. A guarda, sob trava, recusa trocar pela filha o arquivo existente da referência aprovada. Editar relê a linha antes de derivar. A aprovação em grupo e o comando H06 ainda permitem ordem inversa. `app/ponto/services/ausencia.py:283–297,697–740,1115–1134,1405–1434`, `app/ponto/views.py:2786–2815`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137–139,165–170`. |
| **B2 — escrita e prova** | **Curado nos três pontos do v1.** O `save` real de `rederivar_fim` enumera as colunas e exclui os seis, o arquivo e a FK. A trilha dos seis propaga falha dentro da transação. O teste do histórico exige a chamada e a linha devolvida. Os mutantes do `save` integral, da trilha facultativa e da perda de delegação falharam. A lacuna geral do selo está no item 3. `app/ponto/services/ausencia.py:300–321,629–632,1865–1868`, `app/core/observ.py:94–108,136–140`, `app/ponto/tests/test_atestado_f3_contrato.py:137–184`, `app/ponto/tests/mutantes_atestado_f3.py:84–92,164–176`. |
| **B3 — lote** | **Parcial.** A seção consulta `estado_inicial`, o limite incide depois do filtro de tipos médicos, e arquivo sumido ou imagem recusada pelos erros de Pillow tratados vira “Anexo indisponível”. O diff adiciona **0** cláusulas `except` e remove **1** na reabertura. Há `suppress` para falhas do Pillow e desaparecimento entre `exists` e `open`, além do `suppress` do 409. O teste de tempo do PDF com 20 fotos existe. Resultado do tempo [nao verificado]. `app/ponto/services/atestados_lote.py:28–31,154–193,228–233`, `app/ponto/views_atestado.py:42–46,119–127`, `app/ponto/services/ausencia.py:1402–1440`, `app/ponto/tests/test_atestado_f3.py:330–351`. |
| **B4 — privacidade** | **Parcial.** O admin retira os seis da lista exibida e a rota normal de mídia exige ação e vínculo por caminho no schema ativo. A trilha de upload ainda guarda o nome do arquivo, o storage é compartilhado e os seis seguem fora da máscara da sombra. `app/ponto/admin.py:38–53`, `app/core/media_protegida.py:10–25,50–56`, `app/ponto/services/ausencia.py:812–821,1122–1128`, `Caddyfile:10–17`, `bin/sombra_regras_pessoais.py:48–78,109,160–164`. |

## 2. Quem escreve e quem lê

**Para cada um dos seis**, o formulário valida, `criar_ausencia` inclui no `INSERT`, e reabrir, editar e anexar chamam `_gravar_dados_atestado` sobre a referência imediata ou sobre a própria linha, se ela não tiver referência. O helper lê os campos para comparar e preservar os omitidos. O POST do drawer recebe os seis. App e tela legada de correção chamam as portas sem passá-los. A 0079 tem seis `AddField`, quatro com padrão vazio e dois anuláveis. `app/ponto/forms.py:13–22`, `app/ponto/views_atestado.py:26–43`, `app/ponto/services/ausencia.py:262–321,503–504,553–563,629–632,738–740,1115–1120,1405–1415`, `app/api/views.py:1824–1831,1852–1858,3140–3146`, `app/ponto/views.py:2973–2982`, `app/ponto/migrations/0079_ausencia_atestado_dados.py:12–24`.

| Campo | Leitores do valor |
|---|---|
| `atestado_medico` | Drawer, CSV e PDF. `app/templates/ponto/partials/painel_ausencia.html:99,123`, `app/ponto/services/atestados_lote.py:22–24,116–150,219–223`. |
| `atestado_crm` | Drawer, CSV e PDF. `app/templates/ponto/partials/painel_ausencia.html:102,124`, `app/ponto/services/atestados_lote.py:22–24,116–150,219–223`. |
| `atestado_crm_uf` | Drawer, CSV e PDF. `app/templates/ponto/partials/painel_ausencia.html:105,124`, `app/ponto/services/atestados_lote.py:22–24,116–150,219–223`. |
| `atestado_cid` | Drawer, CSV e PDF. `app/templates/ponto/partials/painel_ausencia.html:108,125`, `app/ponto/services/atestados_lote.py:22–24,116–150,219–223`. |
| `atestado_emissao` | Drawer, CSV e PDF, como data civil. `app/templates/ponto/partials/painel_ausencia.html:111,126`, `app/ponto/services/atestados_lote.py:116–120,140–150,219–223`. |
| `atestado_dias` | Drawer, CSV e PDF, preservando zero. `app/templates/ponto/partials/painel_ausencia.html:114,127`, `app/ponto/services/atestados_lote.py:116–150,219–223`. |

- **Leituras indiretas dos seis.** O admin exclui os seis da renderização. E4b lê todos os campos concretos e pode gravá-los no JSON de reversão. H06 também lê todos para comparar o antes e o depois, e seu snapshot persistido enumera outros campos. O CSV anual acumulado lê tipo, período e dias ocupados. `app/ponto/admin.py:38–53`, `app/chamados/services/foto_e4b.py:247–252,284–288`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:59–67,355–373`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:94–95,120–123,165–177`, `app/relatorios/views.py:275–321`.
- **Escritores do comprovante.** Criar insere o arquivo. Anexar e `_reabrir` o gravam na referência imediata sob trava. `reverter_rejeicao(documento=...)` é outra entrada para `_reabrir`, o mesmo escritor físico da reabertura. A view de reversão chama sem upload. Excluir a referência aciona `SET_NULL` na filha. `app/ponto/services/ausencia.py:553–563,629–632,1115–1128,1380–1384,1405–1424`, `app/ponto/views.py:2753–2756`, `app/ponto/models.py:815–819`.
- **Leitores do comprovante.** Drawer, entrega protegida, gate e rejulgamento seguem a referência imediata. Histórico/lote selecionam somente raízes. CSV leva indicador, PDF leva imagem ou link protegido, ZIP leva bytes originais, e a mídia busca pelo caminho exato. A listagem prefere o arquivo próprio quando preenchido. A fila do app, o reenvio, a abertura de chamado e a troca de plantão ainda consultam o arquivo da própria linha. Uma filha sem arquivo próprio pode assim aparecer no app como devendo upload enquanto o drawer abre o anexo da raiz. `app/ponto/views.py:2100–2109,2144–2162`, `app/ponto/services/ausencia.py:764–769,830–860,876–904`, `app/ponto/services/dia_decidido.py:624–630,671–687`, `app/ponto/services/atestados_lote.py:134–150,171–193,219–225,247–266`, `app/core/media_protegida.py:18–23`, `app/templates/ponto/ausencias.html:168–171`, `app/api/views.py:1053–1067,1852–1858`, `app/ponto/portas/celula.py:719–746`.

## 3. Classes que já reprovaram

| Classe | Veredito |
|---|---|
| **Segundo juiz** | **Ausente na seção do lote.** Ela e o drawer consultam o juiz registrado `estado_inicial`. **Presente o predicado cru no gate de aprovação**, que usa `TIPOS_EXIGEM_DOC` para o requisito documental dentro de sua regra de confirmação. `app/core/juizes.py:259–264`, `app/ponto/services/atestados_lote.py:28–31,70–86`, `app/ponto/models.py:723–729`, `app/ponto/services/ausencia.py:842–860,1007–1025`. |
| **Segundo escritor dos seis** | **Ausente nos caminhos de produção identificados.** A rederivação deixou de regravá-los. A 0079 inicializa o schema. `app/ponto/services/ausencia.py:309–321,553–563,629–632,1865–1868`, `app/ponto/tests/test_atestado_f3_contrato.py:137–156`, `app/ponto/migrations/0079_ausencia_atestado_dados.py:12–24`. |
| **Relógio fora da borda** | **Presente.** `mes=` ou `ano=` vazio passa pelo `setdefault` e faz o serviço usar `date.today()` do host. Reabrir também lê `localdate()` para o prazo dentro da porta. `app/ponto/views_atestado.py:103–117`, `app/ponto/services/atestados_lote.py:34–45`, `app/ponto/services/ausencia.py:1432–1434`. |
| **Derivação paralela** | **Ausente na montagem do histórico e da seção final.** O lote delega a `linha_do_tempo` e o PDF usa `previa`. **Presente se o recorte significa vigência.** Um `afastamento_inss` aberto antes de `de` fica fora do SQL do lote e entra no juiz de vigência. `app/ponto/services/atestados_lote.py:48–75,228–232`, `app/ponto/turnos.py:3207–3231`, `app/ponto/catalogo/ausencias.py:92–96`. |
| **Ordem de trava** | **Presente a inversão.** Editar, anexar e reabrir travam referência imediata → linha solicitada. A aprovação em grupo escreve na ordem padrão `-data`, numa única transação. Uma filha mais recente pode ficar travada enquanto a aprovação espera a raiz, e um anexo concorrente pode segurar a raiz enquanto espera a filha. H06 também pré-trava a linha antes de chamar a edição, que então pede a referência. O lote de exportação só lê. O lote de aprovação chama `decidir` por item, sem impor ordem de árvore. Deadlock ocorrido [nao verificado]. `app/ponto/services/ausencia.py:283–288,697–698,1067–1069,1115–1116,1184–1193,1405–1406`, `app/ponto/models.py:843`, `app/ponto/views.py:2392–2403,2786–2815`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137–139,165–170`, `app/ponto/views_atestado.py:119–142`. |
| **Campo com dois significados** | **Ausente nos seis.** `atestado_dias` é dias concedidos no papel, apartado de dias e minutos da ausência. **Presente no legado:** `data_fim=NULL` significa dia único ou vigência aberta conforme o tipo, e “tem documento” pode significar arquivo físico da filha ou arquivo acessível na referência, conforme o leitor. `app/ponto/models.py:787–795,898–932`, `app/ponto/turnos.py:3207–3226`, `app/api/views.py:1059–1067`, `app/ponto/views.py:2146–2155`. |

**Mutantes.** O arquivo define **30** substituições, mais `escritor_externo`, quatro REDs c1 e um RED `base`. Os 30 seletores substituíram uma ocorrência cada e compilaram. Rodei 22 substituições sem banco mais `escritor_externo`. Todos ficaram vermelhos. `reenvio_tenta_trocar_aprovado` ficou vermelho pela recusa lançada pela guarda, e `pdf_imagem_recusada` pela exceção de imagem que escapou. `pdf_sem_storage` morde a abertura indevida, pois a linha de indisponibilidade ainda sai pelo `suppress(FileNotFoundError)`. `app/ponto/tests/mutantes_atestado_f3.py:27–117,149–176,212–235`, `app/ponto/tests/test_atestado_f3_contrato.py:299–311,396–413`, `app/ponto/services/atestados_lote.py:175–193`.

Os oito de integração têm asserções correspondentes por inspeção. `editar` verifica 403, `cid_pdf` verifica ausência dos valores, `foto` verifica imagens, `link` verifica os URIs, `sem_documento` verifica os IDs, `empresa` verifica o recorte, `limite` verifica 400 antes da geração e `lote_ferias` verifica 200 sem férias. Execução [nao verificado]. `app/ponto/tests/mutantes_atestado_f3.py:38–54,97–98`, `app/ponto/tests/test_atestado_f3.py:178–301`, `app/config/settings/ci.py:22–26`.

**Ainda passa código errado no selo.** Confirmei em memória que `campos = None` seguido de `ausencia.save(update_fields=campos)` produz zero achados. O selo trata o argumento indireto como lista restritiva, embora o valor seja `None`. A mutação registrada `save_integral` cobre a forma literal real da c1. `app/ponto/tests/test_atestado_f3_contrato.py:54–84`, `app/ponto/tests/mutantes_atestado_f3.py:84–86,164–176`.

## 4. Privacidade

- **Valores estruturados.** Drawer e comprovante por ID exigem `ver_ausencias`, e o POST dos seis exige também `editar_ausencias`. Só `ver_relatorios` recebe CSV/PDF sem os seis, sem imagens e sem links, e recebe 403 no ZIP. O admin omite os seis, mas mantém `documento` na ficha de leitura nativa. A mídia exige a ação para entregar os bytes dos caminhos canônicos. O app tem entradas de upload próprio e devolve status, ID ou erro, sem os seis. `app/ponto/views.py:2094–2116,2138–2162`, `app/ponto/views_atestado.py:26–46,93–99,127–142`, `app/ponto/services/atestados_lote.py:134–150,219–225`, `app/ponto/admin.py:38–53`, `app/core/admin.py:68–75`, `app/core/media_protegida.py:18–23,50–56`, `app/api/views.py:1852–1895,3102–3114,3140–3166`.
- **Trilha e mensagens.** `dados_atestado_editados` grava ID e nomes dos campos alterados, **sem os valores**. `documento_anexado` grava os nomes anterior e novo, e a normalização mantém o basename. Se um CRM ou CID estiver no nome enviado, ele pode persistir no `LogAuditoria` e aparecer no nome do arquivo na ficha admin. A tela geral de auditoria, sob `ver_saude_sistema`, renderiza a descrição e deixa os JSONs fora. Os erros esperados do formulário retornam mensagens de validação, e a recusa de troca tem texto fixo. O log de recusa do app copia o texto do erro. Nomes médicos em arquivos reais, exposição efetiva em logs e conteúdo de exceções inesperadas [nao verificado]. `app/ponto/services/ausencia.py:291–306,812–821,1122–1128`, `app/core/observ.py:136–140`, `app/core/views.py:686–705`, `app/templates/core/log_auditoria.html:55–74`, `app/ponto/admin.py:48–53`, `app/ponto/forms.py:13–22`, `app/ponto/views_atestado.py:35–46`, `app/api/views.py:1898–1922`.
- **Sombra.** A máscara cobre o caminho de `documento` e deixa os seis valores intactos. Os seis nomes também ficam fora da regex do selo. Na sessão, inserir em `VAZIO` a linha `('ponto', 'Ausencia'): ('atestado_medico', 'atestado_crm', 'atestado_crm_uf', 'atestado_cid'),` e em `NULO` a linha `('ponto', 'Ausencia'): ('atestado_emissao', 'atestado_dias'),`. A cópia efetiva desses valores em um dump [nao verificado]. `bin/sombra_regras_pessoais.py:48–78,109,160–164`, `bin/sombra_mascara_pessoal.py:16–26`, `bin/sombra_selo_pessoal.py:23–42,55–60`, `bin/sombra.sh:244–275`.

## 5. Dinheiro e competência exportada

**Sim, há caminhos que mudam insumo financeiro.** Editar ainda grava tipo, período, dias e minutos, agora sobre a linha relida. O gate passou a usar o arquivo da referência e pode mudar a decisão de aprovar uma filha. O `UPDATE` só dos metadados fica fora desse cálculo. `app/ponto/services/ausencia.py:309–321,697–740,830–860`, `app/ponto/views.py:2392–2403,2777–2815`.

**O caso que bloqueia é a gravação parcial sobre instância velha.** `curar_ausencia_sem_fim --apply` carrega as candidatas antes de iterar. Se o DP mover uma ausência para uma data posterior nesse intervalo, `rederivar_fim` pode derivar o fim da data velha e gravá-lo mantendo a data nova, inclusive deixando `data_fim < data`. Também pode repor minutos antigos. A releitura vem depois do `save`. O comando e a porta carecem de cerca de competência entregue. H06 tem a sua própria cerca. O rateio lê fim e minutos, o fechamento soma o mapa e o passa ao `DiaPago`. O acordador invalida a projeção. Efeito sobre TXT já emitido ou registros reais [nao verificado]. `app/ponto/management/commands/curar_ausencia_sem_fim.py:29–50`, `app/ponto/services/ausencia.py:126–139,179–195,1860–1872`, `app/ponto/models.py:898–932`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:105–117,160–170`, `app/folha/export.py:113–143`, `app/ponto/services/fechamento.py:893–895,1224–1229,1672–1680`, `app/ponto/services/dia_pago.py:274–277`.

## 6. Contraexemplos fora do golden

- **App, aprovado + novo arquivo.** Reenvio de raiz aprovada com arquivo devolve `ja_recebido` e conserva o arquivo. Se a raiz aprovada estiver sem arquivo, o mesmo reenvio grava o novo, pois o ramo de duplicata testa apenas o `FileField` e a guarda do serviço só barra substituição via filha. No endpoint de anexo próprio, a verificação de estado vem antes da trava, então uma aprovação concorrente também pode deixar passar uma troca na própria raiz. Para filha aberta de referência aprovada com arquivo, o endpoint direto devolve 409 e o reenvio do lançamento devolve `ja_recebido`. `app/api/views.py:1824–1831,1852–1858,3102–3114,3140–3146`, `app/ponto/services/ausencia.py:295–297,451–460,1104–1134`.
- **Outra filha ainda aguardando.** Se duas filhas aguardam documento, anexar pela primeira move a raiz e aquela filha conforme seus estados. A segunda pode continuar em `aguardando_documento`, mesmo com o arquivo acessível. Seu reenvio pode descartar o upload ao ver a referência documentada e sair antes da transição. Aprovar o grupo continua sujeito à recusa dessa irmã. `app/ponto/services/ausencia.py:451–460,993–997,1049–1055,1104–1134`, `app/ponto/views.py:2786–2815`.
- **Filha de filha.** A FK aceita autorreferência sem restrição de profundidade no modelo. Em `C → B → A`, anexar por C grava em B e servir por C lê B. Servir por B lê A e o lote só seleciona A. A proteção de A e a ordem raiz real → descendente ficam fora do caminho por C. Cadeias existentes [nao verificado]. `app/ponto/models.py:815–819,843–856`, `app/ponto/services/ausencia.py:283–297,1115–1128`, `app/ponto/views.py:2146–2155`, `app/ponto/services/dia_decidido.py:624–630`.
- **Zero atestados.** Confirmei em memória prévia toda zerada, CSV só com cabeçalho e ZIP válido vazio. A view aceita zero e o código do PDF monta título, seção final e mensagem de recorte vazio. Renderização real do PDF [nao verificado]. `app/ponto/views_atestado.py:119–142`, `app/ponto/services/atestados_lote.py:57–61,78–94,123–151,215–234,243–249`.
- **HEIC/PDF corrompido.** Se conserva a assinatura, o PDF do lote emite o link protegido. A entrega e o ZIP devolvem os bytes armazenados. Se perde a assinatura, o PDF imprime “Formato de anexo não reconhecido”. “Anexo indisponível” cobre arquivo ausente e imagem reconhecida que o Pillow recusa nos erros tratados. `app/ponto/services/ausencia.py:788–809`, `app/ponto/services/atestados_lote.py:154–193,258–266`, `app/ponto/views.py:2150–2162`.
- **Mesmo PK em dois tenants.** A busca por PK usa o schema ativo e a mídia consulta o caminho registrado nesse schema. Mesmo PK, por si só, resolve a linha local. Os bytes usam storage sem namespace de tenant no caminho. Se dois schemas apontarem para o mesmo nome físico, ambos resolvem os mesmos bytes. Colisão real e teste com dois schemas [nao verificado]. `app/config/settings/saas.py:34–49,67–94,109–111`, `app/config/settings/base.py:101–102`, `app/ponto/models.py:777–781`, `app/ponto/views.py:2144–2155`, `app/core/media_protegida.py:18–23,53–56`, `Caddyfile:10–17`.

## 7. Bloqueios e fila

- **Bloqueia — raiz e trava.** Aprovação em grupo pode prender filha antes da raiz, H06 pré-trava a linha, e filha de filha escapa à raiz real. `app/ponto/views.py:2786–2815`, `app/ponto/models.py:815–819,843`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137–139,165–170`, `app/ponto/services/ausencia.py:283–288`.
- **Bloqueia — app.** Reenvio pode gravar em raiz já aprovada sem arquivo, e o anexo da referência pode deixar outra filha presa em espera. `app/api/views.py:1852–1858,3102–3114,3140–3146`, `app/ponto/services/ausencia.py:295–297,451–460,1104–1134`.
- **Bloqueia — dinheiro.** `rederivar_fim` pode combinar fim e minutos velhos com a linha atual em competência entregue. `app/ponto/services/ausencia.py:1860–1872`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29–50`, `app/folha/export.py:113–143`.
- **Bloqueia — privacidade.** A sessão precisa cobrir os seis na sombra. Nome clínico no arquivo continua alcançando o registro de auditoria e a ficha admin, e o isolamento dos bytes depende de caminhos físicos distintos. `bin/sombra_regras_pessoais.py:48–78,160–164`, `app/ponto/services/ausencia.py:812–821,1122–1128`, `app/ponto/admin.py:38–53`, `app/core/media_protegida.py:18–23,53–56`.
- **Bloqueia — prova pendente de B3.** Tempo do PDF real com 20 atestados e integração dos oito mutantes [nao verificado]. `app/ponto/tests/test_atestado_f3.py:330–351`, `app/ponto/tests/mutantes_atestado_f3.py:231–235`.
- **Fila — relógio, vigência e competência geral.** `mes/ano` vazios, fim nulo, corte geral e fuso de `decidido_em`. `app/ponto/views_atestado

[truncado em 20000 de 20581 caracteres]

## 26/11 08:59 · sessao 86ffe2b66411 · E1-PREVISAO-24H · construtor · origem=Code

**E1-PREVISAO-24H v5** — O filtro dos leitores e o horizonte da falta estão na cópia. Os enxertos do `MotorComercial` e de `faltas_periodo` estão no fim para aplicação pela sessão. O diff tem **0 `except` novos**. `escala/utils.py:486` `ponto/motor_calculo_v2.py:3404` `ferias/models.py:144` `scripts/test_e1_v5_variantes.py:104`

### Entrega e verificação

- **V1 comercial.** O teste agora exige `MotorComercial` e cobre os dois pares. Em memória, com os pares já apurados, `07:30–11:00` passou de **60/0** para **0/0** min de HE50/HE100. `07:00–12:00` passou de **120/30** para **0/0**. O controle válido ficou em **120/60**. `escala/tests/test_e1_v4_juiz_unico.py:152` `ponto/motor_calculo_v2.py:3404` `scripts/sonda_e1_comercial.py:28`
- **V2 leitores.** Assiduidade, crédito parcial, subtotal do PDF e palavra do cartão passam pelo mapa da previsão. O cartão retira o efeito de desconto da cobertura usada para compor a palavra. Férias conserva a contagem de registros pela data inicial. O teste cobra **30 dias** com cinco faltas válidas e uma inválida e **24 dias** ao acrescentar a falta da 09. `inteligencia/management/commands/calcular_scores.py:7` `ponto/services/credito_parcial.py:15` `ponto/services/efeito_ausencia.py:46` `ponto/services/dia_decidido.py:442` `ferias/tests.py:54`
- **V3, V4 e P5.** O teste de cadastro fixa `sem_celula`, mapa vazio e falta de 480 min. O filtro do overlay, dos minutos e da lista do TXT usa `versao_para` por data e empresa. O caso da 09 cobra os 480 min em leitura com exportação registrada. P5 cobra ausência da chave `falta_aprovada`, transição válido → inválido → corrigido e rateio pelo tamanho inteiro do lançamento que cruza o corte. `escala/tests/test_e1_v5_leitores.py:56` `escala/tests/test_e1_v5_leitores.py:84` `escala/utils.py:486` `escala/utils.py:1365` `folha/export.py:125` `folha/export.py:171` `escala/tests/test_e1_v4_juiz_unico.py:317` `escala/tests/test_e1_v4_juiz_unico.py:354` `escala/tests/test_e1_v4_juiz_unico.py:369`
- **V5 intermitente.** A sonda mede campos financeiros do fechamento e o TXT em memória para colaboradores com vínculos sobrepostos. Escolhi **09, 10 e 11/2026** como padrão. A primeira passada retira em memória o efeito de `responde_pelo_dia` nos leitores da v4. Uma diferença termina com código 1 para a sessão voltar à base. O par intermitente de mais de 24 h já está na fila. `scripts/sonda_e1_intermitente.py:27` `scripts/sonda_e1_intermitente.py:53` `scripts/sonda_e1_intermitente.py:69` `scripts/sonda_e1_intermitente.py:99` `docs/BACKLOG.md:2010`

A catraca rodou **35 testes em memória**, todos OK. A tentativa de RED Django parou no import de `django.core`, antes das asserções. **RED e mutantes integrados, as duas medições de frota, a suíte inteira e o aval final do Opus ficam [nao verificado].** `core/tests/test_medida_turno_do_par.py:29` `scripts/test_e1_v5_variantes.py:118` `docs/PISTA.md:102`

### Comandos

O executor injeta a base ou o mutante em memória. Rode os mutantes `comercial-*` e `ferias-base` após aplicar o respectivo enxerto. `scripts/test_e1_v5_variantes.py:32` `scripts/test_e1_v5_variantes.py:104`

```bash
e1() { PYTHONDONTWRITEBYTECODE=1 python scripts/test_e1_v5_variantes.py "$@"; }
```

| Item | Comando na base e label | Comandos dos mutantes |
|---|---|---|
| V1 | `e1 base escala.tests.test_e1_v4_juiz_unico.Item3SemHeSemDescontoTest` | `e1 comercial-marcos` · `e1 comercial-zero-vira-he` |
| V2 | `e1 base escala.tests.test_e1_v5_leitores.LeitoresDaFaltaTest ferias.tests.FaltaComPrevisaoInvalidaTest` | `e1 ferias-base` · `e1 score` · `e1 credito` · `e1 subtotal` · `e1 palavra` |
| V3 | `e1 base escala.tests.test_e1_v5_leitores.SemCelulaTest` · caracterização da base | `e1 sem-celula` · `e1 universo-sem-celula` |
| V4 | `e1 base escala.tests.test_e1_v5_leitores.LeitoresDaFaltaTest.test_MORDE_competencia_09_guardada_mantem_falta_de_480` | `e1 regra-1` |
| P5 | `e1 base escala.tests.test_e1_v4_juiz_unico.Item5FaltaAprovadaNoDiaInvalidoTest` · caracterizações da v4 | `e1 sem-filtro-minutos` · `e1 overlay` · `e1 datas` · `e1 rateio-recortado` |

`escala/tests/test_e1_v4_juiz_unico.py:149` `escala/tests/test_e1_v5_leitores.py:17` `ferias/tests.py:53` `scripts/test_e1_v5_variantes.py:32`

Rode as sondas **na sombra, com o bloco comercial por marcos ainda presente**. A primeira imprime o número de dias válidos com diferença, `colab/dia/HE50/HE100/dono` via L-099 e o total por rubrica dos inválidos que mudam. Sai com código 1 se houver diferença em dia válido. A segunda isola o item 2 da v4. [Sonda comercial](/home/ronald/wt-codex-e1-previsao-24h-083708/app/scripts/sonda_e1_comercial.py:89) [Sonda intermitente](/home/ronald/wt-codex-e1-previsao-24h-083708/app/scripts/sonda_e1_intermitente.py:69) `ponto/management/commands/e6_oraculo.py:182`

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py shell < scripts/sonda_e1_comercial.py
PYTHONDONTWRITEBYTECODE=1 python manage.py shell < scripts/sonda_e1_intermitente.py
```

### Catraca

| Chave | Velha → nova |
|---|---|
| `escala/utils.py::_alinhar_por_papel` A1 | `649 → 664` |
| `escala/utils.py::_papeis_por_alternancia` A1 | `616 → 631` |
| `escala/utils.py::montar_grade_prevista` B1 | `927 → 949` |
| `escala/utils.py::montar_grade_prevista_periodo_por_turno` B1 | `[1619, 1639, 1649, 1662, 1673, 1964] → [1666, 1686, 1696, 1709, 1720, 2015]` |
| `folha/export.py::dias_ferias_com_batida` B1 | `517 → 507` |
| `ponto/services/cartorio.py::_julgar_colab_corpo` B1 | `[1253, 1411] → [1254, 1412]` |
| `ponto/services/credito_parcial.py::dia_do_periodo` B1 | `36 → 38` |
| `ponto/services/espelho.py::autoridade_do_periodo` B1 | `1065 → 1067` |

`CLASSIFICAR` ganhou `scripts/sonda_e1_comercial.py::sondar` em B2 pelo dia local entregue ao oráculo. Após o enxerto do motor, o censo em memória pede também `MotorComercial.calcular_mes` B1 de `[3346, 3467, 3485, 3510, 3514, 3515]` para `[3346, 3446, 3464, 3489, 3493, 3494]`. `core/tests/test_medida_turno_do_par.py:252` `core/tests/test_medida_turno_do_par.py:307` `core/tests/test_medida_turno_do_par.py:474`

### V1 — MotorComercial, velho → novo

O enxerto usa `dia` e `self`. `_jornada_do_dia` lê L-024 via `_previsto_do_dia`, que usa `_previsto_min`, `_colab_do_calculo` para carregar o colaborador, `celulas_alimentadas` e `folgas_alimentadas`. **Custo pelo código:** zero consultas no acerto do memo. No fechamento, com célula alimentada e memo frio, são duas consultas por dia consultado, uma de colaborador e uma de vínculos. Sem célula, entram as leituras condicionais de folga e feriado. O espelho já preenche o memo em lote. A escolha reutiliza o contrato de prévia sem colaborador de `_jornada_do_dia`. A dobra de feriado passa antes desse bloco. `ponto/motor_calculo_v2.py:1244` `ponto/motor_calculo_v2.py:1279` `ponto/motor_calculo_v2.py:3370` `ponto/services/fechamento.py:779` `ponto/services/espelho.py:1192` `escala/utils.py:2395` `escala/utils.py:2446` `escala/models.py:1279`

`ponto/motor_calculo_v2.py:3404`

```diff
-            # P7 caso Vera col42 (04/08): limite diario = jornada do dia
-            # PELOS MARCOS do template (fonte unica marcos_do_dia; cobre 5x2
-            # com compensacao de sabado — 528min/dia = 44h/sem), fallback 480.
-            _lim_dia = self.JORNADA_DIARIA_MINUTOS
-            if tipo_escala is not None:
-                # F4-CELULA-M2: DNA congelado via EC.marcos_do_dia quando ha
-                # vinculo (fallback template embutido); sem EC = template vivo.
-                _md_m2 = getattr(escala_colaborador, 'marcos_do_dia', None) \
-                    if escala_colaborador is not None else None
-                if _md_m2 is None:
-                    _md_m2 = tipo_escala.marcos_do_dia
-                try:
-                    _hi, _hf, _hii, _hfi = _md_m2(dia)
-                except Exception:
-                    _hi = _hf = _hii = _hfi = None
-                # F1 TURNO sitio 11 (14/09): o limite linear (fim - inicio) so vale para turno que NAO cruza a meia-noite --
-                # pergunta da autoridade (turno de 24h cruza), nao da comparacao crua de fim contra inicio.
-                from escala.servico_jornada import turno_cruza_meia_noite
-                if _hi and _hf and not turno_cruza_meia_noite(_hi, _hf):
-                    _lim_dia = ((_hf.hour * 60 + _hf.minute)
-                                - (_hi.hour * 60 + _hi.minute))
-                    if _hii and _hfi:
-                        _lim_dia -= ((_hfi.hour * 60 + _hfi.minute)
-                                     - (_hii.hour * 60 + _hii.minute))
-            # BUG-HE-INTRA-DOBRADA: idem, no limite diario por marcos.
+            _lim_dia = self._jornada_do_dia(dia)
+            if not _lim_dia:
+                continue
+            # O previsto da grade e liquido; a intra indenizada integra o limite.
             extra_dia = trab_dia - self._limite_com_suprimido(_lim_dia, periodos_dia, dia)
```

### V2 — Férias, velho → novo

`ferias/models.py:144`

```diff
     @property
     def faltas_periodo(self):
         """Faltas injustificadas (Ausencia tipo 'falta' aprovada) na janela do periodo."""
+        from escala.utils import dias_recusados_para_ausencia
         from ponto.models import Ausencia
-        return Ausencia.objects.filter(
+        datas = list(Ausencia.objects.filter(
             colaborador=self.colaborador, tipo='falta', status='aprovada',
             data__gte=self.data_inicio, data__lte=self.data_fim,
-        ).count()
+        ).values_list('data', flat=True))
+        recusadas = dias_recusados_para_ausencia(self.colaborador, datas)
+        return sum(dia not in recusadas for dia in datas)
```

## 26/11 08:57 · sessao 8a8df3bfde0c · PORTA-CONFERE-TRAVA · censo · origem=Code

# PORTA-CONFERE-TRAVA — dossiê de leitura

Conferi as linhas na cópia `558de6bd4bba318bac4130a14936946e218f885d`. O procedimento da casa exporta o HEAD com `git archive`. A cópia está sem `.git`, portanto a identidade do commit pelo Git fica **[nao verificado]**. O dossiê segue em texto. A conferência da amostra pela sessão e a execução dos REDs ficam **[nao verificado]**. `CLAUDE.md:354`, `CLAUDE.md:412`

## 1. Mapa curto

**Os dois itens seguem abertos na cópia.** `DecisaoPergunta.registrar` já centraliza o `INSERT`, aceita lote vazio, normaliza o instante dos compensadores, invalida `decisoes` e `_pv_memo` da instância recebida e acorda as células. Ainda entra no `bulk_create` sem conferir os donos. C20 já tem juiz único, forma avulsa e corpo em lote. O corpo ainda lê `colaboradores_travados()` e usa essa informação para decidir se relê a Empresa. [app/chamados/models.py](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/chamados/models.py:4551), [app/chamados/juizes.py](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/chamados/juizes.py:1054), `app/core/juizes.py:311`

**Já existe um exemplo de entrega sob trava.** O E2, em `retratar_perguntas_absolvidas_por_celula`, confere C, relê perguntas com `disputa__colaborador__empresa` e abre `tranca_da_consulta` com essas instâncias. A afirmação do anexo de que o ramo do juiz é o único lugar que assegura a releitura em todas as portas envelheceu. A alegação de regressão em exatamente 42 portas fica **[nao verificado]**. `app/chamados/reconciliador.py:673`, `app/chamados/reconciliador.py:679`, `app/chamados/reconciliador.py:695`

**Por onde passa.** A escrita cruza `chamados` e `ponto`, por resposta, validação, correção, declaração, materialização e fecho do fio. A API da foto entra pela validação. As telas do fio entram por `processar_post_fio` e o app usa `perguntas_do_app`. O julgamento de célula e a cascata de chamado resolvido alcançam as mesmas guardas. C20 lê o corte de Empresa em `colaboradores`, a janela e K7 em `ponto`, a identidade de célula em `escala` e a trilha em `core`. `app/chamados/urls.py:17`, `app/chamados/urls.py:33`, `app/chamados/views.py:675`, `app/api/foto_recebida.py:93`, `app/chamados/services/pergunta_no_app.py:64`, `app/ponto/services/veredito_celula.py:193`, `app/chamados/signals.py:117`, `app/chamados/juizes.py:1078`, `app/chamados/juizes.py:1348`

**Crons e comandos.** O cadastro local inclui `processar_cartorio`, `lavrar_perguntas_stale --apply`, `disparar_perguntas_competencia --alarme`, `reconciliar_perguntas_orfas --alarme`, `fechar_becos_disputa --alarme` e `vigiar_fio_morto --alarme`. Os modos importam para o censo. Por exemplo, o vigia de órfãs observa o que o consumidor de `EventoCelula` encerra. `validar_classe_a --apply` e os backfills são entradas de comando distintas. Execuções, volumes e flags efetivas de produção **[nao verificado]**. `app/docs/CRONS.md:22`, `app/docs/CRONS.md:33`, `app/config/crons.py:390`, `app/config/crons.py:663`, `app/config/crons.py:1543`, `app/config/crons.py:1548`, `app/config/crons.py:1741`, `app/ponto/management/commands/reconciliar_perguntas_orfas.py:1`, `app/chamados/reconciliador.py:1518`, `app/chamados/management/commands/validar_classe_a.py:72`

**Quanto se usa.** As funções reais para medir invocações são `DecisaoPergunta.registrar` e `dias_trancados`. O dado fornecido traz código de testes e censo estático. Frequência efetiva de entrada, tamanho dos lotes reais, incidência de dono sem trava e quantidade de vereditos C20 em produção **[nao verificado]**. `ato_em` registra o relógio do ato, inclusive de legado e de compensação. Uma contagem por `ato_em` mede linhas persistidas nessa janela lógica. O contador existente `medir_frota` mede o residual de vitalidade C6 em competências trancadas. Seu balde `perguntas_lidas_trancadas` recebe qualquer pergunta que C6 julgue morta. `app/chamados/models.py:4307`, `app/chamados/models.py:4377`, `app/chamados/models.py:4522`, `app/chamados/models.py:4551`, `app/chamados/juizes.py:1054`, `app/docs/CORTES.md:609`, `app/chamados/services/competencia_trancada.py:144`, `app/chamados/services/competencia_trancada.py:331`

**Sonda para o envelope.** O corpo abaixo usa o tenant já selecionado pelo envelope, conta atos na janela lógica e chama o C20 real para os pares distintos das perguntas com ato nessa janela. Escolhi **sete dias locais completos** e blocos de **512** como parâmetros de medição. O C20 é medido na foto do banco no começo da transação. A saída contém só agregados. A sonda usa a role de leitura do envelope, põe a transação em `READ ONLY` e lê o PK de sistema com `.get`, pois a dependência padrão usa `get_or_create`. O envelope existente monta o código em leitura e tem operações próprias de credencial, temporário e trilha. Execução e resultado da sonda **[nao verificado]**. `bin/sonda_leitura.sh:4`, `bin/sonda_leitura.sh:191`, `bin/sonda_leitura.sh:216`, `bin/sonda_leitura.sh:235`, `bin/sonda_leitura.sh:248`, `app/config/settings/base.py:93`, `app/chamados/juizes.py:1249`, `app/chamados/juizes.py:1316`, `app/core/utils.py:18`

```python
import json
from datetime import datetime, time, timedelta
from itertools import islice

from django.contrib.auth.models import User
from django.db import connection, transaction
from django.db.models import Count, Exists, OuterRef
from django.utils import timezone

from chamados.juizes import dias_trancados, tranca_em_lote
from chamados.models import DecisaoPergunta, PerguntaDisputa
from colaboradores.models import Colaborador

DIAS = 7
BLOCO = 512

fuso = timezone.get_default_timezone()
hoje = timezone.localdate(timezone=fuso)
fim = datetime.combine(hoje, time.min, tzinfo=fuso)
inicio = datetime.combine(hoje - timedelta(days=DIAS), time.min, tzinfo=fuso)

with transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")

    atos = DecisaoPergunta.objects.filter(ato_em__gte=inicio, ato_em__lt=fim)
    totais = atos.aggregate(
        linhas=Count("pk"), perguntas=Count("pergunta_id", distinct=True)
    )
    por_ato = list(
        atos.values("ato")
        .annotate(linhas=Count("pk"), perguntas=Count("pergunta_id", distinct=True))
        .order_by("ato")
    )

    perguntas = PerguntaDisputa.objects.filter(
        Exists(atos.filter(pergunta_id=OuterRef("pk")))
    )
    pares_da_foto = (
        perguntas.filter(conversa=False, data_evento__isnull=False)
        .values("disputa__colaborador_id", "data_evento")
        .annotate(perguntas=Count("pk"))
        .order_by("disputa__colaborador_id", "data_evento")
        .values_list("disputa__colaborador_id", "data_evento", "perguntas")
        .iterator(chunk_size=BLOCO)
    )

    sistema_pk = User.objects.values_list("pk", flat=True).get(username="sistema")
    pares_lidos = pares_trancados = perguntas_lidas = perguntas_trancadas = 0

    with tranca_em_lote(sistema_pk=sistema_pk):
        while bloco := list(islice(pares_da_foto, BLOCO)):
            colabs = (
                Colaborador.objects.select_related("empresa")
                .only("pk", "empresa_id", "empresa__pk", "empresa__dia_inicio_competencia")
                .in_bulk({cid for cid, _, _ in bloco})
            )
            pares = [(colabs[cid], dia) for cid, dia, _ in bloco]
            for (_, _, n), trancado in zip(bloco, dias_trancados(pares)):
                pares_lidos += 1
                perguntas_lidas += n
                if trancado:
                    pares_trancados += 1
                    perguntas_trancadas += n

print(json.dumps({
    "ato_em_de": inicio.isoformat(),
    "ato_em_ate_exclusivo": fim.isoformat(),
    "atos_persistidos": totais,
    "por_ato": por_ato,
    "c20_na_foto_atual": {
        "pares_lidos": pares_lidos,
        "pares_trancados": pares_trancados,
        "perguntas_lidas": perguntas_lidas,
        "perguntas_trancadas": perguntas_trancadas,
        "perguntas_fora_por_conversa_ou_sem_dia": totais["perguntas"] - perguntas_lidas,
    },
}, ensure_ascii=False))
```

## 2. Censo de escritores e leitores

**Ato e trava.** O AST da aplicação, com resolução de imports apelidados, aliases de classe e receptores explícitos, deu **1 escritor ORM de produção, 17 chamadas a `registrar` em 16 funções**. As 17 descartam a lista retornada. O censo nativo confirmou o único `bulk_create` e o censo histórico da F5 registra a mesma cardinalidade. A migração `0056`, via `D = apps.get_model`, atualiza historicamente `resposta_mantem_validacao`. As FKs de autor usam `SET_NULL`. `app/core/censo_escritas.py:451`, `app/core/censo_escritas.py:469`, `app/chamados/tests/test_e1_entradas.py:185`, `app/chamados/models.py:4305`, `app/chamados/models.py:4562`, `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12`, `app/docs/RELATO.md:430`

| Chamadores de `registrar` em produção | Escrita e aquisição de C |
|---|---|
| Ressuscitar, fechar disputa, remover motivo, nascer | Atos em `app/chamados/models.py:1681`, `:1737`, `:2152`, `:2287`, `:3297`. Travas em `:1655`, `:2123`, `:2253`, `:3123`. |
| Responder, reabrir para correção, responder automaticamente, carimbar pela trilha | Atos em `app/chamados/services/disputa_emissao.py:1565`, `:1695`, `:1761`, `:1795`. Travas e conferências em `:1490`, `:1677`, `:1746`, `:1784`, incluindo o alias `_exigir`. |
| Carimbar validação, devolver recusada, devolver BUG-PAR-103, sanear L-130 | Atos em `app/chamados/services/validacao.py:176`, `:313`, `:390`, `:475`. `_carimbar_validacao` herda C dos chamadores. Aquisições em `:552`, `:584`, `:745`, `:287`, `:368`, `:465`. |
| Absorção e silêncio na célula | `app/chamados/services/materializacao.py:1075`, `:1125`. `app/ponto/services/veredito_celula.py:176`, `:200`. |
| Backfill de decisões e backfill E4b | `app/chamados/management/commands/backfill_decisoes_pergunta.py:247`, `:261`, `:358`. `app/chamados/management/commands/backfill_fio_morto_e4b.py:161`, `:164`. |

Há trava local ou herdada visível nas 16 funções. Isso deixa casos concretos para a conferência nova. O backfill E4b trava `colab.pk` e recebe `pids` separadamente. O backfill E1 agrupa pela coluna `PerguntaDisputa.colaborador_id`, derivada da disputa no nascimento. A ressuscitação trava o colaborador do chamado e escreve pelas perguntas da relação reversa. A conformidade de cada execução e de todo o lote fica **[nao verificado]** sem a suite. Na leitura estática dos testes apareceram **78 chamadas em 35 arquivos**, incluindo fixtures diretas por alias, como `DP.registrar` e `D.registrar`. Esse é o universo lido, e o total de recusas da suite fica **[nao verificado]**. `app/chamados/management/commands/backfill_fio_morto_e4b.py:161`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:228`, `app/chamados/models.py:1643`, `app/chamados/models.py:3848`, `app/chamados/tests/test_f5_contrato_v2_itens_3_5.py:41`, `app/chamados/tests/test_f5_contrato_v2_itens_3_5.py:59`, `app/chamados/tests/test_e4c_f5_voltaFINAL.py:18`

| Campo ou função | Escritores e leitores na fronteira da obra |
|---|---|
| Dono e dia da pergunta | FK canônica `pergunta → disputa → colaborador`. A coluna redundante do dono nasce em `_chave_de_identidade`, e o descarimbo resolve o dono por `disputa__colaborador_id`. `conversa` nasce nas portas de nascimento, `data_evento` passa por `gravar_dia_do_fato`, e C20 lê os dois por `_dia_de_turno_da_pergunta`. `app/chamados/models.py:1910`, `:2688`, `:3292`, `:3555`, `:3609`, `:3848`, `:4305`, `app/chamados/services/fio.py:945`, `:1034`, `:1052`, `:1067`, `:1192`, `app/chamados/juizes.py:339`, `:1041`, `:1241`. |
| Registro de C | `trava_na_ordem` é quem adquire e registra, pelo alias `_tdc`. O estado vive em `run_on_commit`. Leitores de `colaboradores_travados` são o escritor do registro, `exigir_colaboradores_travados` e C20. A estampa também lê esse registro. `foto_e4b` lê o sentinela ao tratar callbacks. `app/chamados/models.py:2939`, `app/chamados/services/trava_do_colaborador.py:37`, `:45`, `:54`, `:72`, `app/chamados/juizes.py:1084`, `:1163`, `app/chamados/services/foto_e4b.py:50`, `:67`. |
| Conferência de C | Sete chamadas de produção antes da obra. Descarimbo, reconciliador, foto E4b, resposta, reabertura, resposta automática e carimbo pela trilha. `app/chamados/models.py:3609`, `app/chamados/reconciliador.py:680`, `app/chamados/services/foto_e4b.py:225`, `app/chamados/services/disputa_emissao.py:1491`, `:1678`, `:1747`, `:1785`. |
| Campos dos atos | Escritor nas 17 entradas acima via `bulk_create`. `pergunta_id/ato/ato_em` são lidos pelo legado, compensação, backfill e I23. `ato/ato_em/pk/ato_texto/resposta_mantem_validacao` alimentam a leitura vigente e a face SQL aplicável. Autores, motivo e foto original aparecem no histórico. `da_ressuscitacao` tem predicado próprio. As FKs de absorção são lidas pela confirmação e pela tarefa. A foto E4b serializa todos os campos concretos. `app/chamados/models.py:3682`, `:3936`, `:4360`, `:4394`, `:4447`, `:4504`, `:4537`, `app/chamados/services/tarefa_do_dia.py:388`, `:537`, `app/chamados/services/materializacao.py:289`, `app/chamados/services/foto_e4b.py:247`, `:280`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:127`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1133`, `app/templates/chamados/partials/modal_fio.html:205`. |
| Cache, consulta e evento | `registrar` invalida `decisoes` e `_pv_memo` na instância que recebeu, e passa `celula_id`, `disputa__colaborador_id` e `data_evento` ao caminho do evento. `validacao` e `vigia_carimbo` também invalidam o cache de decisões. Os memos da tranca são escritos no juiz, com a consulta e o lote em `ContextVar`, e o repasse `memo_inicial` tem leitor/chamador em S143. `app/chamados/models.py:3555`, `:3561`, `:4568`, `app/chamados/services/validacao.py:297`, `app/chamados/services/vigia_carimbo.py:125`, `app/chamados/juizes.py:1167`, `:1182`, `:1221`, `:1249`, `app/chamados/services/disputa_emissao.py:2054`. |
| Empresa, corte e K7 | Criação/edição do corte pela tela → `estrutura`, e `EmpresaAdmin` permite editar o campo. Leitor do corte para competência é `corte_da_empresa`, com leituras diretas também na mensageria, no carregador de fechamento e no diagnóstico do backfill. C20 pergunta K7, que filtra `empresa_id/mes/ano/reaberto_em` e devolve a linha cujo `fechado_em` limita a trilha. Escrevem a linha `trancar_periodo` e `reabrir_periodo`. `app/colaboradores/views.py:515`, `:542`, `:557`, `app/colaboradores/admin.py:7`, `app/colaboradores/services/estrutura.py:13`, `:18`, `app/ponto/janelas.py:36`, `app/api/views_mensageria.py:726`, `:887`, `app/ponto/services/fechamento.py:456`, `:1424`, `:1456`, `:1488`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:189`, `app/chamados/juizes.py:1121`. |
| Trilha de toque | `LogAuditoria` é inserido por `core.observ.evento`. Para C20, os produtores da assinatura são cobrar, resolver e desarquivar. A lavra da célula também emite `veredito_celula`, e a assinatura distingue a lavra do toque. C20 lê ação, modelo, objeto, autor, instante, descrição e `valor_depois`, e liga as linhas pela célula do colaborador-dia e pelo chamado desse colaborador. `app/core/observ.py:94`, `:134`, `app/chamados/services/cobrar_dia.py:39`, `app/ponto/services/veredito_celula.py:361`, `app/ponto/portas/celula.py:1122`, `:1137`, `app/chamados/views_arquivar.py:77`, `app/chamados/services/arquivo.py:177`, `app/chamados/juizes.py:1352`, `:1377`, `:1389`, `:1403`, `:1414`. |

**Leitores diretos de C20.** `dias_trancados` é chamado pelo avulso, por `tranca_da_consulta` e por `CargaDaTarefa`. O avulso é chamado por C6, `sem_veredito_entre`, dois sítios do reconciliador e S143. `tranca_da_consulta` tem entradas no reconciliador, no app e em dois sítios de S143, incluindo o `partial`. `app/chamados/juizes.py:1041`, `:1242`, `:1502`, `app/chamados/services/tarefa_do_dia.py:555`, `app/chamados/models.py:2045`, `app/chamados/reconciliador.py:695`, `:789`, `:869`, `app/chamados/services/pergunta_no_app.py:64`, `app/chamados/services/disputa_emissao.py:2054`, `:2074`, `:2102`, `:2213`

**Leitores da sequência por fachada.** `leitura` tem consumidores em modelos, histórico, tarefa, vigia de texto, auditor e CLI de classe A. `composta` chega a C6, modelos, reconciliador, telas, API, serviços de ação/validação/materialização, cadastro, folha e cartório. O reverso `decisoes` é materializado diretamente no histórico, ressuscitação, leitura, materialização e tarefa. O `Prefetch` é também insumo dessas leituras. `app/chamados/models.py:1595`, `:1725`, `:1728`, `:2154`, `:2289`, `:3936`, `:3950`, `:4447`, `:4474`, `app/chamados/services/historico.py:98`, `app/chamados/services/tarefa_do_dia.py:429`, `:545`, `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/management/commands/auditar_invariantes_chamados.py:207`, `app/chamados/management/commands/validar_classe_a.py:39`, `app/chamados/juizes.py:1496`, `app/chamados/reconciliador.py:367`, `app/chamados/views.py:1276`, `app/api/views.py:2587`, `app/chamados/services/validacao.py:755`, `app/chamados/services/materializacao.py:289`, `:1088`, `app/escala/services/cadastro_realidade.py:550`, `app/folha/previa.py:143`, `app/ponto/services/cartorio.py:1697`

### Recontagem do anexo C20

Reapliquei em memória a aproximação nominal. O AST de `app/` fora de testes e migrações coleta definições e chamadas, resolve o nome final e aliases de `from … import … as …`, inclui funções locais no corpo léxico e propaga o fecho até `dias_trancados`. A reprodução deu **634 nomes e 839 definições candidatas**. Procurando chamadas posteriores à primeira trava direta ou aos três invólucros do anexo, saíram **45 candidatos**. Os **43 sítios antigos** foram localizados, e os dois adicionais têm cadeia concreta. O script original e a reprodução idêntica dos **654** ficam **[nao verificado]**. A aproximação por nome inclui homônimos, e a posição textual pode cruzar o fim de um `atomic`. `app/chamados/juizes.py:1054`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:239`, `app/chamados/management/commands/validar_classe_a.py:118`, `app/ponto/calculador/alimentacao.py:514`, `app/chamados/services/pergunta_no_app.py:228`, `app/chamados/services/declaracao_texto.py:736`

Abaixo, cada entrada segue **definição / aquisição → chamadas no fecho**, com linhas da cópia. Onde o arquivo se repete, os números seguintes pertencem ao mesmo arquivo. As setas reproduzem o filtro lexical. `app/chamados/models.py:2901`, `app/chamados/services/becos.py:53`, `app/chamados/services/becos.py:67`

| Arquivo | Sítios reconferidos |
|---|---|
| `app/api/foto_recebida.py:48` | `_validar_pelo_dono` 48/73 → 93 |
| `app/chamados/management/commands/backfill_fio_morto_e4b.py:140` | `_executar_dia` 140/163 → 166,169 · `handle` 282/362 → 362,365,384,390,397,404,407,435,440 · **adicional** `_travar_colaboradores` 239/244 → 252 |
| `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:30` | `handle` 30/78 → 99,105 |
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:46` | `_rodar` 46/112 → 118 |
| `app/chamados/management/commands/validar_classe_a.py:55` | **Adicional** `handle` 55/124 → 132 |
| `app/chamados/models.py:899` | `abrir` 899/971 → 988 · `ressuscitar_perguntas` 1602/1655 → 1676 · `fechar` 2055/2123 → 2138 · `remover_motivo` 2199/2253 → 2303 · `reabrir_por_colab` 2413/2469 → 2481 · `_nascer` 3098/3123 → 3258 |
| `app/chamados/reconciliador.py:558` | `retratar_com_motivo` 558/576 → 588,592 · `retratar_perguntas_absolvidas_por_celula` 629/679 → 695,696,699,718,719 · `encerrar_orfas_pela_lampada` 1236/1267 → 1272,1283,1299,1306,1307 |
| `app/chamados/services/acoes_disputa.py:105` | `corrigiveis_sob_a_trava` 105/112 → 116 · `acao_corrigir_resposta` 343/423 → 442,454 |
| `app/chamados/services/becos.py:38` | `fechar_beco` 38/53 → 58,60,61, dentro do callback chama

[truncado em 20000 de 40819 caracteres]

## 26/11 08:55 · sessao 371cb0235542 · E4C-F5-INSTANTE · censo · origem=Code

# E4C-F5-INSTANTE — dossiê de leitura e censo

## 1. Mapa curto

**A F5 já lê o instante vigente do ato. O comparador, o contador e o selo atuais cobrem a presença da validação.** `DecisaoPergunta.composta` entrega o instante e a validação de `leitura`, o texto da projeção e o carimbo da pergunta. `par_ato_coluna` compara dois booleanos de presença de `validada_em`. O BACKLOG pede a comparação do instante, contador esperado zero, selo, A/B em `ALARMA_SEMPRE` e recorte por PK nas entregues. `app/chamados/models.py:4434`, `app/chamados/models.py:4468`, `app/chamados/models.py:4479`, `app/chamados/services/vigia_ato_x_coluna.py:8`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:572`, `app/docs/BACKLOG.md:2155`

Conferi as linhas na cópia identificada pelo diretório `558de6bd4bba318bac4130a14936946e218f885d`. O procedimento da casa extrai a cópia com `git archive`. A confirmação independente do objeto Git está [nao verificado]. O dossiê fica nesta resposta, sem arquivos criados, editados ou apagados. `CLAUDE.md:354`, `CLAUDE.md:412`, `bin/codex_pergunta.sh:615`

**O que envelheceu**

| Registro | Resultado na cópia |
|---|---|
| `r.txt`, 42 DECIDE e 3 EXIBE | **48 DECIDE e 3 EXIBE**. Os seis sítios adicionais e o censo completo estão na seção 2. A espera da pre-opus pela pergunta do aval 1410 foi encerrada pelo 1440. O relato registra o pouso da F5 em 23/11. `app/docs/RELATO.md:311`, `app/docs/RELATO.md:864`, `app/docs/RELATO.md:865` |
| `r.txt`, 59 leituras da coluna | **59**, pela unidade do anexo, com troca de sítio. `_slot_esperado` passou à composta e `carimbar_resposta_pela_trilha` ganhou uma leitura crua sob trava. `app/chamados/juizes.py:371`, `app/chamados/juizes.py:380`, `app/chamados/services/disputa_emissao.py:1776`, `app/chamados/services/disputa_emissao.py:1787` |
| Q0.1, limpar a coluna sem compensar o ato | Ainda presente. A cura L-130 guarda a foto RESPOSTA e zera o par pela porta de lote. A auditoria registra a devolução. `app/chamados/services/validacao.py:450`, `app/chamados/services/validacao.py:472`, `app/chamados/services/validacao.py:485` |
| Q0.2, preencher o instante sem linha RESPOSTA | Ainda presente em `carimbar`. A leitura pode sintetizar a linha faltante por correspondência de PK, tipo e instante. `app/chamados/models.py:3435`, `app/chamados/models.py:3475`, `app/chamados/models.py:4399`, `app/chamados/models.py:4447` |
| Q0.3, decidir pela instância anterior à trava | O diagnóstico antigo envelheceu nos cinco caminhos nomeados. Automática, trilha, recusa, reabertura pelo colaborador e ressuscitação ganharam releitura ou interseção sob a trava. `app/docs/RELATO.md:873`, `app/chamados/services/disputa_emissao.py:1741`, `app/chamados/services/disputa_emissao.py:1781`, `app/chamados/services/validacao.py:974`, `app/chamados/models.py:2466`, `app/chamados/models.py:1676` |
| Achado 2a do verif2 | Continua visível. `perguntas_absorvidas_por` encontra o grupo pelo **texto atual** do escape e o carimbo comum. A devolução relê o mesmo seletor. A linha ABSORÇÃO já guarda os vínculos. A troca da fonte do grupo e os casos de escolha sem âncora ou âncora retirada estão atribuídos à E4d, item 93. `app/docs/RELATO.md:873`, `app/chamados/services/materializacao.py:1125`, `app/chamados/services/materializacao.py:1195`, `app/chamados/services/materializacao.py:1263`, `app/docs/BACKLOG.md:2160`, `app/docs/PROMPTS.md:2873` |
| `JUIZES.md` | Conferem as autoridades da validação, do K6, da tarefa, do C6/C21 e da retenção do pai. O ponteiro de `GRADE-FOLGA-JUIZ` envelheceu. O corte está na linha 504 do `CORTES.md`, e o comando do O168 ainda consulta a guarda na linha 261. Os números históricos do lookup como estoque atual estão [nao verificado]. `app/docs/JUIZES.md:38`, `app/docs/JUIZES.md:40`, `app/docs/JUIZES.md:81`, `app/docs/JUIZES.md:97`, `app/docs/JUIZES.md:102`, `app/docs/JUIZES.md:113`, `app/docs/JUIZES.md:136`, `app/core/juizes.py:297`, `app/core/juizes.py:312`, `app/docs/CORTES.md:504`, `app/ponto/management/commands/supra_juiz.py:261` |

**Travessia.** As entradas passam pela API do app, pelo fio web, pela validação inline e pela declaração. O core de `chamados` conecta resposta, validação, recusa, reabertura, absorção e materialização. Ficha, Cadastro × Realidade, prévia de folha, cartório e histórico da mensageria consomem o resultado. `app/api/urls.py:57`, `app/api/urls.py:92`, `app/api/urls.py:93`, `app/chamados/urls.py:17`, `app/chamados/urls.py:33`, `app/chamados/services/disputa_emissao.py:1471`, `app/chamados/services/disputa_emissao.py:1628`, `app/chamados/services/validacao.py:697`, `app/chamados/services/validacao.py:902`, `app/chamados/services/materializacao.py:2234`, `app/colaboradores/services/ficha.py:173`, `app/escala/services/cadastro_realidade.py:552`, `app/folha/previa.py:143`, `app/ponto/services/cartorio.py:1697`

O cron `vigiar_fio_morto --alarme --alarme-passivo` já está encadeado depois de `vigiar_pausa_export`, com dependências de cartório e órfãs. O comando imprime A/B e grava as chaves com esperado zero no carimbo de frota. O ESTADO lê esse esperado. A decisão de saída 2 usa `gritou`, calculado antes de A/B entrarem no mapa do carimbo. Incluir as chaves só na tupla faria `d[k]` procurar valores ausentes. O teste legado afirma carimbo A/B positivo com `gritou=[]`. `app/config/crons.py:1741`, `app/docs/CRONS.md:39`, `app/chamados/management/commands/vigiar_fio_morto.py:87`, `app/chamados/management/commands/vigiar_fio_morto.py:199`, `app/chamados/management/commands/vigiar_fio_morto.py:211`, `app/chamados/management/commands/vigiar_fio_morto.py:234`, `app/chamados/management/commands/vigiar_fio_morto.py:281`, `app/chamados/management/commands/vigiar_fio_morto.py:303`, `bin/fio_estado.py:121`, `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:78`

**Quanto se usa.** A função real para medir a incidência no estado vigente é `DecisaoPergunta.composta(p).respondida_em`, com `DecisaoPergunta.leitura(p)` como redução da sequência. Para o A/B já em uso, a função real é `vigia_ato_x_coluna.contar`, que chama `par_ato_coluna`. O censo mede sítios estáticos. Frequência de chamadas em produção, execução atual do cron e incidência atual no banco estão [nao verificado]. `app/chamados/models.py:4434`, `app/chamados/models.py:4468`, `app/chamados/services/vigia_ato_x_coluna.py:8`

## 2. Censo de escritores e leitores

Usei AST para localizar chamadas, aliases, atribuições, argumentos de ORM e referências ao campo por string. Conferi os usos em SQL, templates, tuplas e projeções. A unidade é o sítio final, como no anexo. O escopo é código operacional da cópia, com testes e migrações citados quando provam um contrato. Clientes externos, SQL executado fora da cópia e chamadas montadas dinamicamente estão [nao verificado]. Exemplos dos aliases resolvidos são `_pergunta_respondida`, `_q_resp`, `_q_nao_resp`, `_PD`, `D` e `contar_ato_x_coluna`. `app/api/views.py:2584`, `app/colaboradores/services/calendario.py:499`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1628`, `app/chamados/services/disputa_emissao.py:1485`, `app/chamados/services/tarefa_do_dia.py:150`, `app/chamados/management/commands/vigiar_fio_morto.py:79`

**Escritores de `PerguntaDisputa.respondida_em`**

| Origem | Escrita e trilha |
|---|---|
| Nascimento | `_nascer` verifica o par de entrada e, se nasce com projeção preenchida, registra a foto no mesmo ato. `app/chamados/models.py:3117`, `app/chamados/models.py:3292`, `app/chamados/models.py:3297` |
| Resposta humana | `responder_pergunta_disputa_core`, via `_PD`, grava a coluna e a linha RESPOSTA sob a trava. As bordas passam pela API, web e declaração. `app/chamados/services/disputa_emissao.py:1485`, `app/chamados/services/disputa_emissao.py:1530`, `app/chamados/services/disputa_emissao.py:1565`, `app/api/views.py:2653`, `app/chamados/views.py:1279`, `app/chamados/services/disputa_emissao.py:1624` |
| Automática e pela trilha | `responder_automaticamente` e `carimbar_resposta_pela_trilha` gravam coluna mais RESPOSTA após releitura. Foto recebida e cura do limbo são entradas identificadas. `app/chamados/services/disputa_emissao.py:1757`, `app/chamados/services/disputa_emissao.py:1787`, `app/api/foto_recebida.py:83`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:68` |
| Carimbo | `carimbar` completa a coluna se recebeu modo de resposta, há texto e falta instante. A escrita está no `save`; o ramo de absorção registra um ato ABSORÇÃO. `app/chamados/models.py:3435`, `app/chamados/models.py:3475`, `app/chamados/services/disputa_emissao.py:2118`, `app/chamados/services/disputa_emissao.py:2192`, `app/chamados/services/materializacao.py:1104`, `app/chamados/services/materializacao.py:1125`, `app/chamados/services/materializacao.py:2091` |
| Limpeza | `descarimbar_em_lote` é a escrita física comum a ressuscitação, reabertura/recusa e cura L-130. O `save` confere o par persistido. A simulação e o desfazimento de `carimbar` também atribuem em memória. `app/chamados/models.py:3619`, `app/chamados/models.py:1725`, `app/chamados/services/disputa_emissao.py:1695`, `app/chamados/services/disputa_emissao.py:1714`, `app/chamados/services/validacao.py:476`, `app/chamados/models.py:3801`, `app/chamados/models.py:3840`, `app/chamados/models.py:1584`, `app/chamados/models.py:3486` |

**Sequência e campos de apoio.** `DecisaoPergunta.registrar` é a porta de inserção de `ato`, `ato_em`, texto e fotos. Faz `bulk_create` e descarta o prefetch e o memo do C6 da instância recebida. A revisão por AST tem selo próprio. As origens dos atos relevantes são RESPOSTA dos escritores acima e da foto, VALIDAÇÃO pela porta ou foto, DEVOLUÇÃO em `validacao`, RECUSA/REABERTURA em `disputa_emissao`, REABERTURA marcada na ressuscitação, SILÊNCIO no modelo, veredito e backfill, RETRATAÇÃO no modelo e ABSORÇÃO em materialização. `app/chamados/models.py:4256`, `app/chamados/models.py:4305`, `app/chamados/models.py:4366`, `app/chamados/models.py:4401`, `app/chamados/models.py:4406`, `app/chamados/models.py:4551`, `app/chamados/models.py:4568`, `app/chamados/tests/test_e1_entradas.py:185`, `app/chamados/models.py:1737`, `app/chamados/models.py:2152`, `app/chamados/models.py:2287`, `app/chamados/services/validacao.py:176`, `app/chamados/services/validacao.py:313`, `app/chamados/services/validacao.py:390`, `app/chamados/services/disputa_emissao.py:1695`, `app/chamados/services/materializacao.py:1125`, `app/ponto/services/veredito_celula.py:200`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:358`

Os leitores diretos de tipo/instante incluem a simulação e foto da ressuscitação, face SQL da validação, respostas anteriores, síntese, redução da sequência, H4, relógio dos compensadores, tarefa do dia, BUG-PAR, auditoria I23 e fotos de acervo. A flag de preservação da validação tem leitores no redutor e na face SQL e migração histórica própria. `app/chamados/models.py:1595`, `app/chamados/models.py:1725`, `app/chamados/models.py:3682`, `app/chamados/models.py:3936`, `app/chamados/models.py:4394`, `app/chamados/models.py:4431`, `app/chamados/models.py:4447`, `app/chamados/models.py:4504`, `app/chamados/models.py:4537`, `app/chamados/services/tarefa_do_dia.py:150`, `app/chamados/services/tarefa_do_dia.py:388`, `app/chamados/services/tarefa_do_dia.py:538`, `app/chamados/services/materializacao.py:288`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:127`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1132`, `app/chamados/services/foto_e4b.py:280`, `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:12`

No recorte, `data_evento` nasce no modelo e muda pelas portas de reapontar/rechavear. `conversa` nasce pela porta própria. `materializada_em` e `via_resolucao` passam pelo escritor do carimbo, suas portas de identidade/descarimbo e o lote. A via também tem os caminhos de encerrar por fato no espelho e relavrar. K6 lê vigência e invalidação de `ExportacaoDominio`, escritas por `registrar` e `invalidar`. O corte da empresa vem do cadastro. `app/chamados/models.py:3049`, `app/chamados/models.py:3074`, `app/chamados/models.py:3292`, `app/chamados/models.py:3389`, `app/chamados/models.py:3512`, `app/chamados/models.py:3533`, `app/chamados/models.py:3619`, `app/chamados/models.py:3623`, `app/chamados/services/fio.py:945`, `app/chamados/services/fio.py:1034`, `app/chamados/services/fio.py:1192`, `app/chamados/services/relavra_via.py:125`, `app/folha/models.py:143`, `app/folha/models.py:174`, `app/folha/export.py:588`, `app/colaboradores/views.py:518`

**Instante devolvido por `composta` — 48 DECIDE.** São 44 chamadas ao C21 com a composta e quatro usos finais que chegam ao instante por serialização, veto temporal, espera de texto ou comparação da foto inteira. A única serialização contada como DECIDE por dúvida é a da linha 2581. As linhas 2590 e 2646 decidem no servidor. O consumo no APK está [nao verificado]. `app/chamados/juizes.py:374`, `app/chamados/services/fio.py:225`, `app/chamados/services/passivo_validado.py:364`, `app/chamados/services/passivo_validado.py:458`, `app/api/views.py:2581`, `app/api/views.py:2590`, `app/api/views.py:2646`

| Grupo | Sítios atuais |
|---|---|
| API, views e inline | `app/api/views.py:2581`, `:2590`, `:2646`; `app/chamados/views.py:1276`; `app/chamados/views_cobrar.py:172`, `:180`, **`:223`** |
| Juízes, modelos e reconciliador | `app/chamados/juizes.py:374`, **`:380`**, `:1516`; `app/chamados/models.py:2044`, `:3328`, `:3331`, `:3926`; `app/chamados/reconciliador.py:367`, `:1206` |
| Ações, declaração e fio | `app/chamados/services/acoes_disputa.py:169`; `app/chamados/services/becos.py:32`; `app/chamados/services/declaracao_texto.py:445`, `:723`; `app/chamados/services/disputa_emissao.py:1941`, `:2112`; `app/chamados/services/fio.py:225`, `:814`, `:1390`, `:1403` |
| Materialização, passivo e validação | `app/chamados/services/materializacao.py:154` **novo**, `:200`, `:272`, `:304`, `:900`, `:1096`, **`:1122`**, `:1393`; `app/chamados/services/passivo_validado.py:458` **novo**; `app/chamados/services/validacao.py:756`, `:955`, **`:1293`** |
| App, vigia e comandos | `app/chamados/services/pergunta_no_app.py:88`; `app/chamados/services/vigia_fio.py:658`; `app/chamados/management/commands/auditar_invariantes_chamados.py:179`, `:598`, `:1685`; `app/chamados/management/commands/dedup_perguntas_fato.py:76` |
| Outros apps | `app/colaboradores/services/ficha.py:173`; `app/escala/services/cadastro_realidade.py:552`; `app/folha/previa.py:143`; `app/ponto/services/cartorio.py:1697` |

**Três EXIBE.** A composta data a auditoria da absorção. `leitura` direta data a foto da reabertura e os eventos da cronologia. O acesso da tarefa à `leitura` usa `validada_em` nesse ponto e o vigia de texto usa `resposta_colab`. `app/chamados/services/materializacao.py:1143`, `app/core/observ.py:134`, `app/chamados/models.py:1733`, `app/chamados/models.py:3928`, `app/templates/chamados/partials/modal_fio.html:212`, `app/chamados/services/historico.py:106`, `app/chamados/services/historico.py:185`, `app/api/views_mensageria.py:2177`, `app/api/views_mensageria.py:2199`, `app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:429`, `app/chamados/services/vigia_texto_resposta.py:23`

**`PerguntaDisputa.respondida_em` crua — 35 DECIDE e 24 EXIBE.** A classificação abaixo mantém a unidade do anexo. Os dois JSONs do APK e o campo derivado de idade da fila do copiloto conservam a classificação DECIDE por dúvida sobre o consumidor. `app/api/views.py:1165`, `app/api/views.py:2663`, `app/api/views_mensageria.py:572`, `mensageria/nucleo/ferramentas.py:128`

| Classe | Sítios atuais |
|---|---|
| DECIDE, núcleo | `app/chamados/models.py:1568`, `:3433`, `:3437`, `:3605`, `:3840`, `:3841`, `:4390`, `:4399`, `:4400`, `:4401`; `app/chamados/juizes.py:900`, `:1629`, `:1636`, `:1648`, `:1668`, `:1676`, `:1687`, `:1862`; `app/chamados/veredito_lavrado.py:207`, `:208` |
| DECIDE, serviços e bordas | `app/chamados/services/disputa_emissao.py:1688`, `:1700`, `:1776`, `:1787`, `:1959`; `app/chamados/services/foto_e4b.py:251`; `app/chamados/services/painel.py:67`; `app/chamados/services/raiox.py:138`; `app/chamados/services/tarefa_do_dia.py:419`, `:434`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:47`, `:71`; `app/api/views.py:1165`, `:2663`; `app/api/views_mensageria.py:572` |
| EXIBE, serviços e API | `app/chamados/services/disputa_emissao.py:2184`, `:2201`; `app/chamados/services/validacao.py:470`, `:993`, `:1348`; `app/chamados/services/materializacao.py:1242`; `app/api/views_mensageria.py:571`; `app/chamados/services/foto_e4b.py:180`; `app/chamados/services/passivo_validado.py:88`; `app/chamados/services/verbos.py:337`, `:338`, `:344`, `:347`; `app/chamados/services/vigia_fio.py:774` |
| EXIBE, templates, comandos e relatório | `app/templates/chamados/partials/modal_fio.html:220`; `app/templates/chamados/partials/_pergunta_card.html:32`; `app/templates/chamados/partials/_card_atendimento.html:106`, `:143`, `:394`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1116`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:57`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:58`; `bin/censo_folha.py:28`, `:29` |

As faces SQL `q_respondida_sem_texto`, `q_respondida_com_texto` e `q_nao_respondida` entram na contagem por expressão. As aplicações e os carregamentos auxiliares ficam explícitos aqui. A chamada antiga no seletor de irmãs de materialização virou uso da composta. `app/chamados/juizes.py:1668`, `:1676`, `:1687`, `:1853`, `:1860`, `app/chamados/models.py:1552`, `:1682`, `:3732`, `app/chamados/canal.py:66`, `app/chamados/views.py:1413`, `app/chamados/services/acoes_disputa.py:126`, `app/chamados/services/disputa_emissao.py:1832`, `app/chamados/services/materializacao.py:154`, `:2228`, `app/chamados/services/verbos.py:335`, `app/colaboradores/services/calendario.py:505`, `app/ponto/management/commands/reconciliar_grade.py:461`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:37`, `:97`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1600`, `:1631`, `app/chamados/management/commands/bug_par_103.py:73`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:61`, `:105`, `:540`

## 3. Leis herdadas

| Lei ou corte | Consequência para a obra |
|---|---|
| **L-001, L-002, L-003, L-007** | Cura na origem, uma autoridade lida pela testemunha e pelo contador, medida pela função real, escrita canônica com trilha de antes/depois e motivo. `app/docs/LEIS.md:59`, `:60`, `:61`, `:65` |
| **L-130**, `CORTE-JUIZ-RESPONDIDA-JUIZ-NASCE`, `F5-L1-VALE-L130-1811` | C21 exige instante e texto após trim. A porta recusa as duas metades ilegais. O estoque vivo de instante sem texto volta a pendente com trilha, com autorização própria para as entregues enumeradas. Validadas e materializadas ficam. `app/docs/LEIS.md:151`, `app/docs/CORTES.md:562`, `:605` |
| `CORTE-F2-RESSUSCITAR-GRAVA-ATO` e `CORTE-RESSUSCITAR-TEM-DONO` | Ressuscitação limpa resposta e validação com ato compensador marcado. Em aberta, dono ADMIN com cobrança caso a caso. Em entregue ou paga fora, lista textual ao DP. `app/docs/CORTES.md:588`, `:589`, `:590` |
| `F5-CONTRATO-V2-2211-1610` e `RESPOSTA-DESENHO-F5-7A-2311-1350` | Compensadores usam o máximo do relógio pedido e do último ato da mesma pergunta, com desempate por PK. RESPOSTA conserva o instante recebido. O C6 da guarda decide sobre o estado pré-ato. `app/docs/PROMPTS.md:2789`, `app/docs/CORTES.md:635` |
| Texto corrente, limbo e absorção rejeitada | O corte de texto corrente vale para leitores e SILÊNCIO. A cura do limbo mede a projeção por decisão expressa. A absorção rejeitada volta a pendente, com a troca da origem do grupo registrada na E4d. `app/docs/CORTES.md:587`, `:606`, `:608`, `:611`, `app/docs/BACKLOG.md:2160` |
| **L-092, L-113 e DINHEIRO 2**, K6 | O recorte po

[truncado em 20000 de 38052 caracteres]

## 26/11 08:53 · sessao 2805847f50f4 · - · - · origem=Ronald (tmux)

O C20 ainda lê os donos travados diretamente e a estampa de aquisição indiretamente. A releitura de `Empresa` permanece no juiz, e o memo pode compartilhar o corte entre instâncias distintas da mesma empresa. `app/chamados/juizes.py:1084`, `:1098`, `:1107`, `:1112`, `:1163`, `:1199`.

Reconferi as 43 entradas históricas na cópia e remeti as linhas atualizadas. O AST por nome final com apelidos de import, excluindo testes e migrações, encontrou 634 nomes e 839 definições candidatas. A seleção lexical produziu 45 candidatos com os invólucros do anexo e 55 ao incluir outros invólucros de trava confirmados. São números de triagem. Um nome igual em receptores diferentes pode produzir conexão falsa, como `regras.do_dia` e `pergunta_no_app.do_dia`. A reprodução idêntica dos 654 é [nao verificado]. `app/docs/BACKLOG.md:1985`, `app/ponto/calculador/alimentacao.py:514`, `app/chamados/services/pergunta_no_app.py:228`.

Confirmei duas entradas ausentes da lista histórica. O backfill inventaria perguntas ressuscitáveis depois de adquirir C e chega ao C20 por C6. O comando manual `validar_classe_a` invoca a validação sob o `atomic` externo e pode chegar ao C20 pela cauda da materialização. `app/chamados/management/commands/backfill_fio_morto_e4b.py:244`, `:252`, `:221`, `app/chamados/services/disputa_emissao.py:1917`, `app/chamados/models.py:1526`, `:1599`, `app/chamados/management/commands/validar_classe_a.py:118`, `:124`, `:132`, `app/chamados/services/validacao.py:830`, `app/chamados/services/materializacao.py:2137`, `:2293`.

O teste A6 vigente exige que o juiz receba a instância velha e veja o corte novo sob trava. Isso diverge do resultado pedido no i6a. O controle positivo i6 do HEAD faz a releitura explícita, e os dois REDs do anexo ficaram fora do commit. O i6b fornecido usa duas cópias carregadas antes da mudança, portanto falta o caso com cortes efetivamente distintos da mesma empresa. `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:365`, `:372`, `:379`, `app/chamados/tests/test_f5_v6_guarda_sob_trava.py:247`, `:263`, `:270`, `app/chamados/juizes.py:1086`, `:1112`.

Os tetos da cópia continuam 254 e 318. O caminho sem autenticação explícita no memo do C20 pode chamar `get_or_create` para o usuário de sistema. Para uma sonda de incidência baseada no C20 real, `tranca_em_lote(sistema_pk=...)` abastece o PK obtido por `SELECT` e evita esse caminho. A contagem persistida de `DecisaoPergunta` mede atos por timestamp lógico e não entradas de `registrar`, chamadas de C20 ou recusas sem trava. `app/ponto/tests/test_selo_chokepoint_escrita.py:218`, `:254`, `app/chamados/juizes.py:1249`, `:1308`, `:1316`, `app/core/utils.py:18`, `app/chamados/models.py:4307`, `:4514`, `:4551`.

## 26/11 08:53 · sessao 2dbe81d1b28d · - · - · origem=Ronald (tmux)

Censo estático concluído e encaminhado. A cópia contém 17 chamadas runtime a `DecisaoPergunta.registrar` em 16 funções, 78 chamadas diretas em testes e apoio, e um único `bulk_create` runtime do modelo. A porta registra, normaliza compensações, invalida caches e acorda células. A confirmação do dono ainda falta. `app/chamados/models.py:4551–4577`; `app/chamados/tests/test_e1_entradas.py:185–222`; `app/chamados/tests/test_f5_contrato_v2_itens_3_5.py:41,59–121`.

Os chamadores runtime têm aquisição visível nas rotas nominais. O backfill E4B mostra por que o teste precisa conferir os donos efetivos. A seleção recebe PKs, a trava usa o colaborador informado separadamente e o registro usa as perguntas selecionadas. O backfill E1 depende da identidade espelhada na pergunta, definida pela FK da disputa no nascimento e no preenchimento histórico. `app/chamados/management/commands/backfill_fio_morto_e4b.py:85–103,161–164`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:228–247,358`; `app/chamados/models.py:3848–3858`; `app/chamados/migrations/0051_decisao_pergunta.py:22–25`.

O C20 consulta os donos travados diretamente e a estampa indiretamente. O cache compartilhado por empresa pode misturar fotografias dos pares. O teste F5 que exige releitura com instância velha transfere sua prova para a porta que entrega a instância fresca sob a nova regra. `app/chamados/juizes.py:1082–1126,1171,1199`; `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:365–379`; `app/docs/CORTES.md:639`.

A sonda proposta deve usar uma conexão leitora já provisionada. O script existente altera credencial, catálogo e arquivos locais. A contagem por `ato_em` mede o relógio do ato, com compensação para os casos previstos, e a frequência efetiva de invocação permanece [nao verificado]. `bin/sonda_leitura.sh:121–164,216–218,245–250`; `app/chamados/models.py:4307,4514–4546`; `app/docs/CORTES.md:609`.

## 26/11 08:51 · sessao 7e2316ccdda5 · - · - · origem=Ronald (tmux)

- A regra de escrita vem de `E1-ORDEM-POR-TRANSACAO`. O dono da disputa deve estar travado na transação antes da escrita, e os donos de um lote são travados por PK crescente. A porta existente de descarimbo resolve os donos por `disputa__colaborador_id` e consulta o registro. `app/docs/PROMPTS.md:1971`, `app/chamados/tests/test_e1_v13_ordem.py:1-11,59-73`, `app/chamados/models.py:3607-3610,3848-3858`.
- `RETOMA-E-LIBERA-F5-2311-0905` e `RESPOSTA-LEI-C20-PURO-2311-1450` já decidem a alteração de `registrar`, a entrega da instância ao C20 e a retirada da consulta do C20 ao registro. O pouso da F5 está registrado. Nenhuma pergunta bloqueante de lei encontrada no escopo literal. `app/docs/PROMPTS.md:2813`, `app/docs/CORTES.md:639`, `app/docs/RELATO.md:311`.
- Leis herdadas relevantes: L-002 autoridade única, L-003 sonda pela função real, L-007 escritor canônico, L-030 batida de chão protegida, L-051 histórico e reversão por toque, L-090 competência pelo corte da empresa, L-110 matriz contra a regra, L-117 moratória, L-121 projeções no pouso, L-123 padrão, L-130 par de resposta. O C20 julga tranca viva sem toque humano posterior no mesmo par e preserva lote único por empresa × competência. `app/docs/LEIS.md:60-61,65,78,92,123,136,138,142,144,151`, `app/docs/CORTES.md:415,507`. Padrões literais `Policy Enforcement Point`, `Single Writer Principle` e `Identity Map`. `app/docs/GLOSSARIO.md:12,31,33`.
- O RED A6 atual obriga o juiz a ler novamente a Empresa da instância velha. A prova da lei nova precisa subir à porta real. Os dois i6 anexados usam instâncias inicialmente velhas e não provam que duas fotos diferentes da mesma Empresa permanecem distintas nos dois sentidos da ordem. O memo de empresa atual é por `empresa_id`, e C20 consulta a estampa indiretamente pelo memo e pela consulta. `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:361-381`, `app/chamados/juizes.py:1035-1038,1086-1122,1155-1205,1238-1242`.
- O risco de transação na sonda é verificável. A tranca pode chamar `get_user_sistema()`, que usa `get_or_create`. Transação PostgreSQL `READ ONLY`, obtenção prévia com `.get` e `tranca_em_lote(sistema_pk=...)` sustentam a garantia de leitura. O contador existente `medir_frota` mede o residual do C6, e `perguntas_lidas_trancadas` conta qualquer pergunta que C6 julgou morta. Contar o C20 com a função `dias_trancados`. `app/chamados/juizes.py:1054,1248-1259,1316-1322`, `app/core/utils.py:6-18`, `app/chamados/services/competencia_trancada.py:144-154,331-357`.
- O núcleo obrigatório fica em `chamados/models.py` e `chamados/juizes.py`, fora das listas das cinco obras pedidas. `ponto/services/fechamento.py` é dependência de leitura em comum com E1, PAR e FERIADO. `ponto/turnos.py` é dependência da carga do C19 em comum com PAR. Outros arquivos que uma suíte encontre seguem [nao verificado]. `app/chamados/models.py:4551`, `app/chamados/juizes.py:1054,1079,1125`, `app/ponto/services/fechamento.py:1488`, `app/ponto/turnos.py:3024`, `app/chamados/services/tarefa_do_dia.py:497-499`, `app/docs/RELATO.md:3-6,17,13243,13254`.
- O resumo C20 de JUIZES confere. A referência GRADE-FOLGA-JUIZ a `CORTES.md:416` envelheceu para `:504`. As linhas “SEM juiz” não acompanham os registros `_F6`, `_F7`, `_K1`, `_K4`, `_K7`. O C17 está limitado em chamadas ao diff, mas continua implementado e registrado. Os tetos 254/318 conferem como constantes, e a medição dos anexos segue [nao verificado]. `app/docs/JUIZES.md:90,97-101,117-122,135`, `app/core/juizes.py:308,311,321,323,335,338,350`, `app/chamados/juizes.py:1801`, `app/chamados/management/commands/diff_e3_sombra.py:144`, `app/ponto/tests/test_selo_chokepoint_escrita.py:218,254`.

## 26/11 08:50 · sessao 2b9febd4514d · - · - · origem=Ronald (tmux)

O censo AST encontrou uma porta de inserção de `DecisaoPergunta` e três origens que zeram a projeção da resposta pela porta de lote. O carimbo pode preencher `respondida_em` sem inserir `RESPOSTA`, e a cura L-130 pode preservar a linha histórica `RESPOSTA` enquanto zera a coluna sem ato compensador. As duas formas seguem presentes na cópia. `app/chamados/models.py:3435–3443,3475,3619–3621,4551–4562`, `app/chamados/services/disputa_emissao.py:1661,1695–1714`, `app/chamados/services/validacao.py:472–489`

As janelas antigas de leitura fora da trava foram revistas nos cinco caminhos apontados no Q0. O achado 2a permanece na identificação do grupo de absorção pelo texto atual da projeção. A lei determina que uma absorção rejeitada volta a pendente, mas a nova fonte de identificação desse grupo está [nao verificado]. `app/docs/RELATO.md:873`, `app/docs/CORTES.md:606,611`, `app/chamados/services/disputa_emissao.py:1741–1756,1781–1792`, `app/chamados/services/validacao.py:967–990`, `app/chamados/models.py:1676–1679,2459–2484`, `app/chamados/services/materializacao.py:1192–1208`

O contador e o selo ato × coluna atuais cobrem validade de `validada_em`. O teste do vigia exige ausência de alarme A/B. O teste dos dois sentidos de meia-escrita cobre o par instante + texto, com escopo distinto do pareamento ato × instante. `app/chamados/models.py:4479–4485`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:572–584`, `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:55–69,78–97`, `app/chamados/tests/test_contract_par_resposta.py:139–149`

## 26/11 08:50 · sessao c9c0d0c592d3 · - · - · origem=Ronald (tmux)

Censo estático da cópia, pela unidade do anexo, fechou em **48 DECIDE e 3 EXIBE** para o instante vigente. Os seis DECIDE ausentes no anexo são `app/chamados/juizes.py:380`, `app/chamados/views_cobrar.py:223`, `app/chamados/services/materializacao.py:154` e `:1122`, `app/chamados/services/validacao.py:1293` e `app/chamados/services/passivo_validado.py:458`. As três exibições estão em `app/chamados/services/materializacao.py:1143`, `app/chamados/models.py:1733` e `app/chamados/services/historico.py:106`. O uso da serialização pelo APK está [nao verificado] (`app/api/views.py:2581`).

A coluna crua conserva **59 sítios, 35 DECIDE e 24 EXIBE**, pela unidade do anexo. `_slot_esperado` passou a ler a composta e `carimbar_resposta_pela_trilha` ganhou uma releitura crua sob trava. Registrei à parte quatro carregamentos auxiliares de `.only()`. `app/chamados/juizes.py:371–380`, `app/chamados/services/disputa_emissao.py:1776` e `:1787`, `app/chamados/models.py:1682`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:61`, `:105` e `:540`.

A Q0 histórica ainda tem os caminhos de limpeza sem compensação e de carimbo sem RESPOSTA persistida. As portas automática e pela trilha já revalidam suas guardas sob trava. O achado 2a segue reproduzível por leitura: a busca do grupo consulta o texto atual, enquanto o ato ABSORCAO já grava os vínculos. `app/chamados/services/validacao.py:450–476`, `app/chamados/models.py:3437–3439` e `:3475`, `app/chamados/services/disputa_emissao.py:1741–1756` e `:1781–1792`, `app/chamados/services/materializacao.py:1125–1129`, `:1199–1208`, `:1237` e `:1263`.

## 26/11 08:50 · sessao 289e7fc19d15 · - · - · origem=Ronald (tmux)

O achado 2a continua no código. `perguntas_absorvidas_por` encontra o grupo pela resposta atual da pergunta de hora. A devolução relê o mesmo seletor sob a trava. O BACKLOG atribui à E4d o uso da linha ABSORÇÃO como fonte do grupo e registra os dois casos de regressão, escolha sem âncora e âncora retirada. A lei de texto corrente continua vigente para os leitores e o SILÊNCIO. `app/docs/RELATO.md:873`, `app/chamados/services/materializacao.py:1175`, `app/chamados/services/materializacao.py:1195`, `app/chamados/services/materializacao.py:1263`, `app/docs/BACKLOG.md:2160`, `app/docs/CORTES.md:587`

A Q0 mudou. A limpeza L-130 ainda guarda foto RESPOSTA e limpa o par sem compensador. `carimbar` ainda pode preencher o instante sem registrar linha RESPOSTA, e `leitura` sintetiza a linha faltante. A automática e o carimbo pela trilha já releem a guarda sob a trava. A trilha também confronta o texto relido com o texto testemunhado. Recusa e reabertura pelo colaborador já conferem novamente a elegibilidade sob a trava. `app/chamados/services/validacao.py:472`, `app/chamados/services/validacao.py:974`, `app/chamados/models.py:3436`, `app/chamados/models.py:4390`, `app/chamados/models.py:4447`, `app/chamados/services/disputa_emissao.py:1741`, `app/chamados/services/disputa_emissao.py:1781`, `app/chamados/models.py:2466`

O item 13 pede o recorte geral **por PK** nas portas de validação, recusa e materialização em competências entregues. O A/B histórico e a amostra por porta são evidências diferentes. A sonda só de leitura pode estabelecer o universo pelo dia do fato e K6, enquanto o teste pelo escritor exige banco de teste. Dia do fato ausente fica explícito. Aplicar L-113 e a guarda vigente à expectativa da folha. `app/docs/BACKLOG.md:2155`, `app/docs/RELATO.md:694`, `app/docs/RELATO.md:703`, `app/docs/RELATO.md:745`, `app/chamados/services/materializacao.py:802`, `app/folha/export.py:556`, `app/docs/LEIS.md:155`, `app/ponto/services/fechamento.py:36`

Leis principais para citar: L-001/002/003 (origem, autoridade única, medição pela função real), L-007 (escritor, tripwire e trilha), L-099 (separar dado e estrutura), L-117 (só corte literal resolve lei), L-123 (padrão nomeado), L-128 (relógio injetado), L-130 (par, recusa simétrica e estoque específico), L-114 (RED certifica, invariante de frota com meta zero), L-113 (competências entregues). Cortes complementares: C21 após trim e sua face SQL, ressuscitação compensadora que preserva a fila, C6 pré-ato, comando do limbo pela projeção e exceção declarada do carimbo pela trilha na lavra. `app/docs/LEIS.md:59`, `app/docs/LEIS.md:65`, `app/docs/LEIS.md:122`, `app/docs/LEIS.md:138`, `app/docs/LEIS.md:144`, `app/docs/LEIS.md:150`, `app/docs/LEIS.md:154`, `app/docs/LEIS.md:156`, `app/docs/CORTES.md:562`, `app/docs/CORTES.md:588`, `app/docs/CORTES.md:590`, `app/docs/CORTES.md:608`, `app/docs/CORTES.md:610`, `app/docs/CORTES.md:635`. O DOSSIES herda portas Q1–Q6, RED real por porta, idempotência e selo de escrituras. `app/docs/DOSSIES.md:43`, `app/docs/DOSSIES.md:51`, `app/docs/DOSSIES.md:59`

Padrão **single source of truth / um juiz por pergunta**, com invariante no escritor (**make illegal states unrepresentable**) e selo **Architecture Conformance Test**. `app/docs/PLANO-PADROES.md:49`, `app/docs/GLOSSARIO.md:15`

Matriz de regra para consolidar:

- Par vazio leva a pendente, par completo leva a respondida, e ambas as metades são recusadas. RED existente `test_RED_porta_recusa_as_duas_metades`. `app/docs/LEIS.md:151`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:92`
- Nova resposta humana conserva o instante recebido e desfaz a validação. Automática e trilha conservam a validação. REDs de controle existentes `test_resposta_nova_depois_da_validacao_volta_ao_admin`, `test_resposta_automatica_depois_da_validacao_conserva_a_validacao`, `test_resposta_pela_trilha_e_validacao`. `app/docs/PROMPTS.md:2789`, `app/chamados/models.py:4411`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:239`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:259`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:295`
- Recusa e reabertura limpam o par, devolução da validação o conserva, e silêncio/retratação/absorção isolados o conservam. Compensadores usam o máximo por pergunta e desempate por PK. A ressuscitação tem marca e mantém a fila. REDs `test_resposta_e_recusa_zeram_o_par_na_leitura`, `test_resposta_validacao_devolucao_reabertura`, `test_c3a_REABERTURA_nasce_no_ultimo_ato`, `test_c3b_DEVOLUCAO_nasce_no_ultimo_ato`, `test_c3c_RECUSA_nasce_no_ultimo_ato`. `app/docs/CORTES.md:588`, `app/docs/PROMPTS.md:2789`, `app/chamados/models.py:4411`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:165`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:215`, `app/chamados/tests/test_f5_contrato_v2_itens_3_5.py:79`
- Q0.1 `test_RED_l130_zerar_coluna_nao_deixa_resposta_vigente` e Q0.2 `test_RED_carimbo_com_texto_nao_grava_instante_sem_ato` são nomes propostos para provar o invariante sem aceitar igualdade obtida por síntese. O teste `test_RED_instantes_presentes_e_distintos_contam_divergencia` distingue igualdade de instantes de igualdade de presença. Os REDs de corrida existentes `test_MORDE_resposta_automatica_rele_depois_da_trava` e `test_MORDE_trilha_rele_depois_da_trava` provam a guarda operacional. `app/docs/BACKLOG.md:2155`, `app/chamados/models.py:4390`, `app/chamados/tests/test_f5_contrato_v1_guarda_sob_trava.py:137`
- O RED proposto `test_RED_ato_x_coluna_a_e_b_alarmam_sem_alarme_passivo` precisa provar código 2 com `--alarme`. Hoje as chaves só entram no carimbo depois de `gritou`, e o teste legado afirma ausência de alarme. `app/docs/PROMPTS.md:2710`, `app/chamados/management/commands/vigiar_fio_morto.py:87`, `app/chamados/management/commands/vigiar_fio_morto.py:199`, `app/chamados/management/commands/vigiar_fio_morto.py:211`, `app/chamados/management/commands/vigiar_fio_morto.py:284`, `app/chamados/management/commands/vigiar_fio_morto.py:303`, `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:78`

Perguntas restantes: (1) qual tratamento dos 16.886 de projeção sem linha e 71 de linha persistida com coluna vazia conta para o esperado zero ou para passivo à parte, e qual destino autorizado para o estoque. (2) se o item 13 exige quais campos e efeitos por PK, além dos agregados A/B já registrados, a especificação exata do formato não apareceu. O 2a tem destino na E4d e não requer pergunta de lei sobre troca do seletor nesta fatia. `app/docs/BACKLOG.md:2155`, `app/docs/BACKLOG.md:2160`, `app/docs/RELATO.md:694`, `app/docs/RELATO.md:703`, `app/docs/RELATO.md:865`

## 26/11 08:48 · sessao 1cac04485aca · RAZAO-DA-SOBRA · leitura · origem=Code

- **Competência e schema.** O envelope seleciona `juliani`. A sonda aplica `janela_fechamento(11, 2026, empresa)` a todas as empresas pelo FK `Colaborador.empresa`. Com corte 21, a janela inclusiva é 21/10 a 20/11. O controle de 24/11 é validado antes dos totais e só entra neles se o corte da empresa incluir a data. `bin/sonda_leitura.sh:235–243`, `app/colaboradores/models.py:46`, `app/colaboradores/models.py:321`, `app/ponto/janelas.py:10–21`, `app/ponto/janelas.py:36–57`.
- **Órfãs gravadas.** `CelulaDia.colaborador`, `data`, `dna` e `ata` dão a identidade e os dados do dia. `cartorio.ata_do_dia` produz `ata['orfas']` e `ata['n_orfas']`. O cartório passa o JSON a `CelulaDia.lavrar_veredito`, e a porta grava a ata. O universo inclui atas antigas com só `n_orfas`; a comparação de hora exige o `HH:MM` gravado. `app/escala/models.py:1692–1712`, `app/escala/models.py:1787–1791`, `app/escala/models.py:1832–1846`, `app/ponto/services/cartorio.py:749–766`, `app/ponto/services/cartorio.py:1906–1932`, `app/ponto/portas/celula.py:1090–1104`, `app/ponto/portas/celula.py:1127–1135`, `app/ponto/portas/celula.py:1218–1238`, `app/escala/services/leitor_celula.py:180–190`.
- **Dispensa e borda.** A grade usa a ausência aprovada que cobre o dia para a folha. `_marcar_ausencias` chama `periodo_da_parcial_no_dia` para dispensar um marco previsto ainda faltante. O cartório grava `lampadas[].tipo`, `hora`, `acesa`, `luz` e `dispensada`, além de `cobertura_parcial` e `periodo_parcial`. A sonda exige a dispensa lavrada com o tipo da mesma ausência e chama o juiz inclusivo com o deslocamento do marco, inclusive após meia-noite. No controle, também chama `batida_dentro_da_parcial` com a direção da batida apurável das 15:54. Em (a), os critérios são a igualdade de hora e a dispensa do marco S. `app/ponto/models.py:22–24`, `app/ponto/models.py:67`, `app/ponto/models.py:743–771`, `app/ponto/models.py:809`, `app/ponto/turnos.py:2528–2537`, `app/ponto/turnos.py:2555–2576`, `app/ponto/turnos.py:2633–2691`, `app/ponto/turnos.py:2921–2948`, `app/escala/utils.py:330–384`, `app/escala/utils.py:1229–1233`, `app/escala/utils.py:1272–1295`, `app/escala/models.py:1663–1690`, `app/escala/models.py:1109–1144`, `app/escala/models.py:1177–1219`, `app/escala/servico_jornada.py:15–66`, `app/ponto/services/cartorio.py:611–615`, `app/ponto/services/cartorio.py:730–745`, `app/ponto/services/cartorio.py:782–790`.
- **Pergunta ou chamado aberto.** Em (b), contei a célula uma vez quando há pergunta de fato viva pelo juiz `pergunta_viva` ou chamado direto com `status_local='aberto'`, o estado do contador **Abertos** da casa. O vínculo é `celula_id`. O ID exibido vem do chamado direto aberto ou, pela pergunta viva, do destino ou da disputa. A autoria pela órfã específica é [nao verificado]. `app/chamados/models.py:39–64`, `app/chamados/models.py:1923–1929`, `app/chamados/models.py:2685–2707`, `app/chamados/models.py:3752–3763`, `app/chamados/models.py:3773–3780`, `app/chamados/models.py:4305`, `app/chamados/juizes.py:911–952`, `app/chamados/services/painel.py:212–222`.
- **Falhas e verificação.** A sonda aceita `SELECT` e propaga falhas de consulta e erros encaminhados pelo juiz a `registrar_engolido`. Conferi a sintaxe e exercitei em memória o controle e a falha por dispensa ausente. Execução e totais em produção [nao verificado]. `app/chamados/juizes.py:1564–1588`, `app/core/observ.py:73–82`, `bin/sonda_leitura.sh:235–251`.

```python
from datetime import date, datetime, time, timedelta
from itertools import islice
from unittest.mock import patch

from django.db import connection
from django.db.models import Q
from django.utils import timezone

from chamados.juizes import pergunta_viva
from chamados.models import ChamadoColaborador, PerguntaDisputa
from colaboradores.models import Empresa
from core import observ
from escala.models import CelulaDia
from escala.servico_jornada import escala_vigente
from escala.utils import cadeia_do_dia, deslocamentos_na_cadeia, encadear_marcos
from ponto.janelas import janela_fechamento
from ponto.turnos import (
    ausencia_cobre,
    batida_dentro_da_parcial,
    batidas_apuraveis,
    periodo_da_parcial_no_dia,
)

COLAB_CONTROLE = 306
DIA_CONTROLE = date(2026, 11, 24)
HORA_CONTROLE = "15:54"
MARCO_CONTROLE = "19:00"
CAMPOS = ("id", "colaborador_id", "colaborador__empresa_id", "data", "ata", "dna")
SEM_DESLOCAMENTO = "carimbo S sem deslocamento confirmado dentro do período"


class FalhaDeLeitura(BaseException):
    pass


def somente_select(execute, sql, params, many, context):
    if many or not sql.lstrip().upper().startswith("SELECT "):
        raise FalhaDeLeitura("A sonda tentou executar uma instrução fora de SELECT.")
    try:
        return execute(sql, params, many, context)
    except Exception as erro:
        raise FalhaDeLeitura("Falha ao executar SELECT na sonda.") from erro


def propagar_erro(_logger, operacao, erro, **_contexto):
    raise FalhaDeLeitura(f"Falha na leitura da autoridade da casa ({operacao}).") from erro


def avaliar(cel, orfas):
    ata = cel["ata"] or {}
    if not ata.get("cobertura_parcial"):
        return None, "ata sem carimbo de cobertura parcial"
    if not orfas:
        return None, "ata sem HH:MM de órfã"

    cid, dia = cel["colaborador_id"], cel["data"]
    ausencia = ausencia_cobre(cid, dia, para="folha")
    if ausencia is None:
        return None, "ausência aprovada não encontrada"
    if ausencia.hora_ini is None or ausencia.hora_fim is None:
        return None, "ausência aprovada sem início ou fim do período"
    inicio = ausencia.hora_ini.strftime("%H:%M")
    if inicio not in orfas:
        return None, f"início {inicio} ausente da lista de órfãs"

    lampadas = ata.get("lampadas") or []
    candidatas = [
        i for i, lampada in enumerate(lampadas)
        if lampada.get("tipo") == "S"
        and lampada.get("dispensada") == ausencia.tipo
        and lampada.get("acesa") is True
        and lampada.get("luz") is None
    ]
    if not candidatas:
        return None, "ata sem marco S dispensado pelo tipo da ausência aprovada"

    dna = (cel["dna"] or {}).get("marcos") or {}
    if dna.get("hi") and dna.get("hf"):
        seq = [
            (tipo, time.fromisoformat(hora))
            for tipo, _, hora in CelulaDia.marcos_presentes(dna)
        ]
        cadeia = encadear_marcos(dia, seq)
    else:
        escala = escala_vigente(cid, dia)
        cadeia = (
            cadeia_do_dia(dia, escala.marcos_do_dia(dia), escala.pausas_do_dia(dia))
            if escala else []
        )

    offsets = deslocamentos_na_cadeia(dia, cadeia, lampadas)
    dispensados = {
        lampadas[i]["hora"] for i in candidatas
        if i in offsets and periodo_da_parcial_no_dia(ausencia, [offsets[i]]) == "dentro"
    }
    if not dispensados:
        return None, SEM_DESLOCAMENTO
    return (ausencia, inicio, dispensados), None


def validar_controle(cel):
    if cel is None:
        raise RuntimeError("Controle 306/2026-11-24: CelulaDia ausente.")
    ata = cel["ata"] or {}
    if ata.get("n_orfas") != 1 or ata.get("orfas") != [HORA_CONTROLE]:
        raise RuntimeError("Controle 306/2026-11-24: a ata não confirma n_orfas=1 e órfã 15:54.")

    resultado, motivo = avaliar(cel, ata["orfas"])
    if resultado is None:
        raise RuntimeError(f"Controle 306/2026-11-24 fora de (a): {motivo}.")
    ausencia, inicio, dispensados = resultado
    if (
        ausencia.tipo != "declaracao" or inicio != HORA_CONTROLE
        or ausencia.hora_fim.strftime("%H:%M") != MARCO_CONTROLE
        or MARCO_CONTROLE not in dispensados
    ):
        raise RuntimeError(
            "Controle 306/2026-11-24: declaração 15:54–19:00 ou dispensa S 19:00 ausente."
        )

    minuto = timezone.make_aware(datetime.combine(DIA_CONTROLE, time(15, 54)))
    direcoes = set(
        batidas_apuraveis(COLAB_CONTROLE, ini=minuto)
        .filter(timestamp__lt=minuto + timedelta(minutes=1))
        .values_list("tipo", flat=True)
    )
    if direcoes != {"S"}:
        raise RuntimeError(
            f"Controle 306/2026-11-24: saída 15:54 sem direção inequívoca ({sorted(direcoes)})."
        )
    posicao = batida_dentro_da_parcial(ausencia, [(HORA_CONTROLE, next(iter(direcoes)))])
    if posicao != "fora" or ata.get("periodo_parcial") != "fora":
        raise RuntimeError(
            f"Controle 306/2026-11-24: esperado 'fora', juiz={posicao}, "
            f"ata={ata.get('periodo_parcial')}."
        )
    return inicio


def chamados_e_perguntas(ids, agora):
    relacionados = {}
    chamados = (
        ChamadoColaborador.objects.filter(celula_id__in=ids, status_local="aberto")
        .order_by("pk").values_list("celula_id", "pk")
    )
    for celula_id, chamado_id in chamados:
        relacionados.setdefault(celula_id, chamado_id)

    perguntas = (
        PerguntaDisputa.objects.filter(celula_id__in=ids - relacionados.keys(), conversa=False)
        .select_related("celula", "colaborador", "disputa__colaborador", "chamado_destino")
        .prefetch_related("decisoes").order_by("pk")
    )
    for pergunta in perguntas:
        if pergunta.celula_id not in relacionados and pergunta_viva(pergunta, agora=agora):
            relacionados[pergunta.celula_id] = (
                pergunta.chamado_destino_id or pergunta.disputa.chamado_id
            )
    return relacionados


def sondar():
    if connection.schema_name != "juliani":
        raise RuntimeError("Execute no schema juliani.")
    agora = timezone.now()
    base = CelulaDia.objects.values(*CAMPOS)
    controle = base.filter(colaborador_id=COLAB_CONTROLE, data=DIA_CONTROLE).first()
    inicio_controle = validar_controle(controle)
    controle_id = controle["id"]

    filtro = Q(pk__in=[])
    for empresa in Empresa.objects.only("pk", "dia_inicio_competencia"):
        ini, fim = janela_fechamento(11, 2026, empresa)
        filtro |= Q(colaborador__empresa_id=empresa.pk, data__range=(ini, fim))

    universo = 0
    sem_hora = 0
    sem_deslocamento = 0
    casos = {}
    celulas = (
        base.filter(filtro).exclude(ata__isnull=True)
        .order_by("colaborador_id", "data", "pk")
    )
    for cel in celulas.iterator():
        ata = cel["ata"] or {}
        orfas = ata.get("orfas") or []
        if not orfas and not (ata.get("n_orfas") or 0):
            continue
        universo += 1
        if not orfas:
            sem_hora += 1
            continue
        if cel["id"] == controle_id:
            casos[controle_id] = (COLAB_CONTROLE, DIA_CONTROLE, HORA_CONTROLE, inicio_controle)
            continue
        resultado, motivo = avaliar(cel, orfas)
        if resultado is None:
            if motivo == SEM_DESLOCAMENTO:
                sem_deslocamento += 1
            continue
        _, inicio, _ = resultado
        casos[cel["id"]] = (cel["colaborador_id"], cel["data"], inicio, inicio)

    relacionados = chamados_e_perguntas(set(casos) | {controle_id}, agora)
    print(f"universo={universo}")
    print(f"N_a={len(casos)}")
    print(f"N_b={sum(pk in relacionados for pk in casos)}")
    if sem_hora or sem_deslocamento:
        print(
            f"[nao verificado] universo_sem_hora={sem_hora} "
            f"candidatas_sem_deslocamento_confirmado={sem_deslocamento}"
        )
    print(f"controle_a=sim n_orfas=1 integra_competencia_11={controle_id in casos}")
    print("colab_id/dia/orfa/inicio_parcial/chamado_id")
    print(
        f"{COLAB_CONTROLE}/{DIA_CONTROLE}/{HORA_CONTROLE}/{inicio_controle}/"
        f"{relacionados.get(controle_id) or '-'}"
    )
    amostra = ((pk, linha) for pk, linha in casos.items() if pk != controle_id)
    for pk, (cid, dia, orfa, inicio) in islice(amostra, 19):
        print(f"{cid}/{dia}/{orfa}/{inicio}/{relacionados.get(pk) or '-'}")


with connection.execute_wrapper(somente_select), patch.object(
    observ, "registrar_engolido", propagar_erro
):
    sondar()
```

## 26/11 08:44 · sessao f958b0f973ed · DIA-DA-BATIDA-JUIZ-UNICO · verificador · origem=Code

**Veredito da F1 — bloqueia.** A borda de um dia produz tanto falso positivo quanto falso negativo no fechamento da adesão. A matriz também passa a responder `18/20`, enquanto o R6 publica `19/20` e seu selo falha. `app/colaboradores/services/pos_vinculo.py:38`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3010`, `app/core/tests/test_selo_r6_frase_do_juiz.py:20`

A revisão foi somente leitura. Usei os arquivos da cópia e o diff anexo. O hash Git exato, a suíte Django com banco e a incidência dos cenários em produção ficam [nao verificado]. [nao verificado]

### 1. ESMERIL-SEMANTICA

| Campo ou valor | Quem escreve, quem lê e qual é o significado |
|---|---|
| `ChamadoColaborador.contexto_json['periodo']` | `sincronizar_adesao` calcula a competência por `janela_atual(hoje, empresa)`, consulta `CelulaDia.data` no intervalo inclusivo e envia `[ini.isoformat(), fim.isoformat()]` a `ChamadoColaborador.abrir`. A porta persiste os defaults no nascimento e devolve o pai existente sem renovar `periodo`. O catálogo lê `data_inicio` como data do chamado. O leitor específico dos dois limites é `fechar_adesao_com_batida`. Na base, a comparação era com o dia civil da marca; na F1, é com o dia devolvido pelo mapa. Ambos incluem as duas pontas. `app/chamados/services/adesao.py:26`, `app/chamados/services/adesao.py:55`, `app/chamados/services/adesao.py:75`, `app/chamados/models.py:972`, `app/chamados/models.py:1037`, `app/chamados/catalogo/modulos.py:126`, `app/chamados/catalogo/modulos.py:219`, `app/ponto/janelas.py:10`, `app/colaboradores/services/pos_vinculo.py:41` (base no diff anexo), `app/colaboradores/services/pos_vinculo.py:38` |
| Mapa `dia_das_batidas` | `_dia_das_batidas_puro` escreve em memória `{PK: dia}`. Um turno retornado imprime `t.data_turno`. Se nenhum turno **retornado** reivindicar a PK, o mapa publica a data civil local. A função avulsa e `CargaDaJanela` usam o mesmo corpo. Leem o mapa a adesão, o espelho, as duas rotas da API, o painel operacional, o diagnóstico de férias, o vigia de plantio e o adaptador unitário `dia_do_turno_da_batida`. A adesão usa só os valores do mapa, sem saber qual dos dois ramos produziu cada valor. `app/ponto/turnos.py:2976`, `app/ponto/turnos.py:2992`, `app/ponto/turnos.py:3010`, `app/ponto/turnos.py:3113`, `app/colaboradores/services/pos_vinculo.py:43`, `app/ponto/services/espelho.py:589`, `app/api/views.py:1461`, `app/api/views.py:2046`, `app/core/services/painel_op.py:330`, `app/ferias/management/commands/listar_ferias_com_batida.py:54`, `app/ponto/management/commands/vigia_plantio_overnight.py:87` |
| `Turno.data_turno` | `_mk` grava a saída de `_data_do_turno` na `namedtuple`. A referência é a entrada ou, para saída órfã, a própria saída. A geometria pode atribuir a referência à véspera conforme seus marcos e envelope. `turnos_do_colab` retém os turnos pelo dia inclusivo do segmento, o mapa copia o campo e o núcleo o materializa em `TurnoMaterializado`. `data_local` usa o fuso ativo do Django, com `America/Sao_Paulo` como configuração base e `USE_TZ=True`. O `periodo` armazena datas sem fuso. `app/ponto/turnos.py:43`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:312`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3017`, `app/ponto/nucleo.py:41`, `app/config/settings/base.py:93` |
| `ChamadoColaborador.status_local` | No nascimento, `abrir` toma o estado do motor. Neste fechamento, o serviço seleciona `VIVOS`, chama `transicionar_com_pauta` com `pai.reconciliar`, e o gate relê o estado sob a trava canônica. O motor permite `RECONCILIAR → resolvido`; `_gravar_estado` persiste o estado e a trilha. Só depois do retorno verdadeiro o serviço anexa a nota e incrementa o contador. A nota usa a porta de `FollowupChamado`. O painel lê `COBRANCA`, que tem universo menor que `VIVOS`. `app/chamados/models.py:999`, `app/colaboradores/services/pos_vinculo.py:37`, `app/chamados/services/fecho_runtime.py:146`, `app/chamados/models.py:661`, `app/chamados/models.py:761`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:109`, `app/chamados/models.py:292`, `app/chamados/services/nota.py:75`, `app/chamados/services/painel.py:189` |
| Célula da `MATRIZ` | A declaração em `core/contratos_estruturais.py` escreve `verde=False`, mantém o teste S3 e acrescenta `DIVIDA_DIA_DA_BATIDA` às exceções. `celula`, `verdes` e `declaradas` leem essa declaração. O selo geral recusa verde com exceção cheia, e o selo F1 exige a referência ao registro. `app/core/contratos_estruturais.py:225`, `app/core/contratos_estruturais.py:1052`, `app/core/tests/test_selo_contratos_estruturais.py:81`, `app/ponto/tests/test_contract_no_batida_date.py:651` |

Há dois sentidos operacionais para a mesma adesão. O fluxo reativo exige batida no período gravado quando há dois limites. `apurar_furos_diarios` chama `sincronizar_adesao(fechar=True)`, que aceita uma batida apurável **em qualquer data** e usa o mesmo gate de encerramento. A diferença está na decisão de tentar fechar. `app/colaboradores/services/pos_vinculo.py:38`, `app/escala/management/commands/apurar_furos_diarios.py:15`, `app/chamados/services/adesao.py:143`, `app/config/crons.py:1531`

### 2. Borda e contraprovas

Para um chamado `[D,D]`, a F1 pede turnos de `D−1` a `D+1`. O pad interno carrega marcas de `D−3` a `D+2`, mas cada segmento só devolve turnos cujo `data_turno` pertence ao segmento. A marca pode estar **carregada e pareada**, perder seu turno no recorte e voltar ao mapa com a data civil. Reproduzi os dois casos abaixo em memória com o pareador e o montador de mapa da árvore, sem banco. `app/colaboradores/services/pos_vinculo.py:43`, `app/ponto/turnos.py:1834`, `app/ponto/turnos.py:1977`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3010`

| Cenário sintético, com `D = 25/10` | Resultado |
|---|---|
| **Falso positivo.** Sem marcos, `E 23/10 23:55` e `S 25/10 00:05`. São 24h10, dentro do teto de 26h desse ramo. | A borda F1 descarta o turno de 23/10 e publica a saída como **25/10**. Com dois dias de borda, a mesma PK fica em **23/10**. Para um pai apto de 25/10, a F1 fecha indevidamente. `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1492`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3010`, `app/colaboradores/services/pos_vinculo.py:43` |
| **Falso negativo além do golden.** Vínculo 19h–07h sem DNA. Par `E 21/10 16:00` e `S 21/10 18:00`, seguido de uma `S` às 07h em cada dia de 22 a 25/10. | A borda F1 perde o par de 21/10 na carga. As saídas da série recuam para a véspera e a PK de 25/10 sai como **24/10**. Com dois dias de borda, o par fechado de 21/10 impede o primeiro recuo e a sequência mantém a PK final em **25/10**. O filtro civil antigo encontrava a marca de 25/10 e, neste cenário, concordava com o juiz mais bem alimentado. `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1253`, `app/ponto/turnos.py:1295`, `app/ponto/turnos.py:1977`, `app/colaboradores/services/pos_vinculo.py:41` (base no diff anexo), `app/colaboradores/services/pos_vinculo.py:43` |

Na virada comum, `E` na véspera e `S` às 07h ficam no dia da entrada, como cobrem os goldens. Saída sem par também pode receber a véspera se couber no envelope. Sem uma reivindicação no resultado, vale o dia civil, e a F1 aceita essa marca sozinha. Retratada fica fora das duas cargas. Se a **entrada** do primeiro cenário for retratada, a saída viva pode passar a ser órfã de 25/10, alterando o dia da PK que sobrou. `app/ponto/tests/test_x1_folga_sem_adesao.py:456`, `app/ponto/tests/test_x1_folga_sem_adesao.py:492`, `app/ponto/turnos.py:293`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:2979`, `app/ponto/turnos.py:3010`

### 3. Consultas e caminho quente

O receiver de adesão é `post_save` de `Batida`, sem guarda de `created` ou `raw` nesse receiver. Pega criação e atualização por `save`, inclusive a retratação que usa `save(update_fields=...)`. `bulk_create` fica fora desse gatilho. Há também a chamada direta pelo pós-vínculo. `app/ponto/signals.py:206`, `app/ponto/signals.py:232`, `app/ponto/registro_batida.py:272`, `app/ponto/registro_batida.py:287`, `app/ponto/tests/test_x1_folga_sem_adesao.py:439`, `app/colaboradores/services/pos_vinculo.py:95`

| Consultas para encontrar evidência, antes do gate | Base | F1 |
|---|---:|---:|
| Buscar os pais vivos | 1 | 1 |
| Cada pai sem dois limites | 1 `EXISTS` | 1 `EXISTS` |
| Cada pai com dois limites válidos | 1 `EXISTS` | 5 leituras fixas, mais até 3 condicionais |

As cinco são batidas do pareador, células, vínculos candidatos, vínculo da ata e segunda leitura de batidas para o mapa. As condicionais são geradoras faltantes, DNA e `MAX(timestamp)` de batida apurável. Assim, um pai com período e sem transição custa **2 → 6–9 consultas**. O laço por pai já existia e agora repete cargas inteiras, inclusive para pais com janelas idênticas. O pareador processa as marcas em memória, com nova varredura da lista por segmento. Gate, locks, nota, receivers decorrentes e custo total medido ficam [nao verificado]. `app/colaboradores/services/pos_vinculo.py:37`, `app/colaboradores/services/pos_vinculo.py:41` (base no diff anexo), `app/ponto/turnos.py:1889`, `app/ponto/turnos.py:1538`, `app/ponto/turnos.py:1593`, `app/ponto/turnos.py:1977`, `app/ponto/turnos.py:2976`, `app/escala/alimentacao.py:148`, `app/escala/utils.py:99`

### 4. Catraca

- **Os mutantes publicados mordem.** Rodei os dez métodos com `unittest` no lugar de `SimpleTestCase`. O censo encontrou **252 ocorrências**, sendo **102 J, 94 P e 56 E**. Repor as duas chaves da adesão em memória apontou as duas como novas. A remoção de uma ocorrência em `core/cell.py` acusou o registro obsoleto. `app/ponto/tests/test_contract_no_batida_date.py:506`, `app/ponto/tests/test_contract_no_batida_date.py:546`, `app/ponto/tests/test_contract_no_batida_date.py:564`, `app/ponto/tests/test_contract_no_batida_date.py:639`
- **O registro pode crescer com tudo verde.** Injetei outra consulta civil e a catraca acusou a falta. Cadastrei o mesmo sítio como `J` em memória e passaram tanto a catraca geral quanto o mutante específico da adesão. O selo compara chaves da árvore atual com chaves do registro atual. Classe, dono e motivo têm validação de formato. Um `J` também pode virar `P` sem alteração no código de produção, porque a contagem deriva do rótulo manual. `app/ponto/tests/test_contract_no_batida_date.py:514`, `app/ponto/tests/test_contract_no_batida_date.py:524`, `app/ponto/tests/test_contract_no_batida_date.py:532`, `app/ponto/tests/test_contract_no_batida_date.py:651`
- **Cobertura AST.** Funciona com apelido estático de módulo ou função, import dentro da função, `__init__.py` e import relativo direto, inclusive `from ..turnos import data_local as dia`. Testei em memória. Escaparam `localdate(valor)` com o timestamp recebido em parâmetro sem marca lexical, import por reexportação em outro módulo e lookup construído por `''.join(...)`. `app/core/medida_turno_do_par.py:100`, `app/core/medida_turno_do_par.py:131`, `app/core/medida_turno_do_par.py:231`, `app/ponto/tests/test_contract_no_batida_date.py:26`, `app/ponto/tests/test_contract_no_batida_date.py:51`, `app/ponto/tests/test_contract_no_batida_date.py:109`

### 5. Matriz, placares e registro de juízes

Haiku e `tickets_placar` consultam a matriz viva e mostram **18/20**, com **20 declaradas**. O R6 usa a frase gravada de **19/20**. `gerar_estado.py` renderiza o R6 pelo placar estrutural, e o handoff extrai dele a fração. Rodei o selo do R6 em memória e ele falhou na igualdade da frase. O teste de teto do Haiku exige denominador 20 e toma o numerador da matriz. `app/core/arquitetura_leitura.py:55`, `app/core/placar_tickets.py:215`, `app/core/placar_tickets.py:312`, `app/core/placar_estrutural.py:436`, `bin/gerar_estado.py:73`, `bin/handoff_sessao.sh:205`, `app/core/tests/test_selo_r6_frase_do_juiz.py:20`, `app/core/tests/test_haiku_contratos_estruturais.py:23`

O registro vivo segue apontando `_T1` para `turnos_do_colab`. A F1 chama a projeção do próprio juiz e não registra autoridade nova. O selo S3 de zero ainda conta `PENDENTES_TURNO`, enquanto a matriz recebe os `J` por outra exceção. Portanto, zero nesse selo e vermelho na matriz são resultados possíveis na mesma árvore. `app/docs/JUIZES.md:11`, `app/core/juizes.py:215`, `app/core/juizes.py:414`, `app/ponto/tests/test_contract_juiz_turno.py:80`, `app/core/contratos_estruturais.py:225`

### 6. Classes que já reprovaram obras

**Presentes:** duas respostas para a premissa da adesão entre cron e fluxo reativo, mistura de dia civil e dia do turno no mapa recortado, e brecha de admissão na catraca. Há ainda uma nova saída por exceção: com `periodo` de comprimento dois contendo, por exemplo, `null`, `fromisoformat` levanta antes do gate. No signal, o `except` existente encerra aquela tentativa inteira, inclusive o restante do laço. No pós-vínculo, a chamada deixa a exceção subir. Estoque real desse formato [nao verificado]. `app/chamados/services/adesao.py:143`, `app/ponto/turnos.py:3010`, `app/ponto/tests/test_contract_no_batida_date.py:524`, `app/colaboradores/services/pos_vinculo.py:37`, `app/colaboradores/services/pos_vinculo.py:95`, `app/ponto/signals.py:232`

A F1 reaproveita o escritor de estado e a ordem **Colaborador → Disputa → Chamado → Pergunta**. A decisão acrescentada usa datas do contexto e timestamps dos fatos, sem ler o relógio corrente. O ramo sem dois limites continua com o universo da vida, e a projeção civil do mapa já existia. O efeito novo é usá-la como evidência de fechamento. `app/colaboradores/services/pos_vinculo.py:38`, `app/chamados/models.py:292`, `app/chamados/models.py:761`, `app/ponto/turnos.py:45`, `app/ponto/turnos.py:3010`

### 7. Leis dos sítios

- **L-085 e L-099:** o caso de 24h10 faz a cauda do plantão contar como presença em outra jornada, e o mesmo conjunto de fatos dá dias diferentes para a mesma PK conforme o recorte. `app/docs/LEIS.md:117`, `app/docs/LEIS.md:122`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3010`
- **L-002 e L-061:** a F1 chama a geometria registrada, sem copiar o pareador. A decisão de fechamento, porém, é refeita a partir da carga, e o cron de adesão conserva uma segunda regra de presença que pode encerrar o mesmo pai. A emissão já lê a lâmpada `CelulaDia.veredito`. `app/docs/LEIS.md:60`, `app/docs/LEIS.md:95`, `app/chamados/services/adesao.py:26`, `app/chamados/services/adesao.py:143`, `app/colaboradores/services/pos_vinculo.py:43`
- **L-007, L-024, L-080, L-100 e L-117:** o escritor de transição e sua trilha permanecem na porta, os marcos vêm do carregamento do juiz, a conversão continua no fuso configurado, o teto continua 20 e o diff não cria lei. **L-111:** a varredura inclui funções sem chamador quando reconhece a forma. Os pontos cegos da AST limitam essa garantia. O vermelho enquanto houver `J` está previsto para esta obra. `app/docs/LEIS.md:65`, `app/docs/LEIS.md:75`, `app/docs/LEIS.md:107`, `app/docs/LEIS.md:126`, `app/docs/LEIS.md:137`, `app/docs/LEIS.md:138`, `app/docs/BACKLOG.md:1858`, `app/chamados/models.py:292`, `app/ponto/turnos.py:1914`, `app/core/contratos_estruturais.py:1041`, `app/ponto/tests/test_contract_no_batida_date.py:546`

### Achados para o veredito

1. **[bloqueia] Dia da PK depende da borda.** Com borda de um dia, a saída do par de 24h10 fecha o pai errado; na sequência de saídas órfãs, o pai certo fica aberto. `app/colaboradores/services/pos_vinculo.py:43`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3010`
2. **[bloqueia] Selo R6 vermelho.** A asserção compara `19/20` gravado com `18/20` calculado. O ESTADO e o handoff carregam a cópia antiga. `app/core/placar_estrutural.py:443`, `app/core/tests/test_selo_r6_frase_do_juiz.py:24`, `bin/gerar_estado.py:73`, `bin/handoff_sessao.sh:205`
3. **[fila] Cron pode desfazer a distinção da F1.** Com uma única batida apurável fora da competência do pai, o evento mantém o pai vivo e a execução de `apurar_furos_diarios` pode fechá-lo pelo estoque. A regra do cron é anterior à fatia. `app/colaboradores/services/pos_vinculo.py:38`, `app/chamados/services/adesao.py:143`, `app/escala/management/commands/apurar_furos_diarios.py:17`
4. **[fila] Catraca admite dívida nova com registro simultâneo.** Uma nova consulta civil cadastrada como `J` entrou sem falhar nem a comparação geral nem o mutante da adesão. `app/ponto/tests/test_contract_no_batida_date.py:524`, `app/ponto/tests/test_contract_no_batida_date.py:546`, `app/ponto/tests/test_contract_no_batida_date.py:639`
5. **[fila] Custo por pai no signal.** Um pai que fica vivo por falta de evidência na janela troca um `EXISTS` por 5–8 leituras a cada `post_save`, além da consulta dos pais. Latência e frequência em produção [nao verificado]. `app/ponto/signals.py:206`, `app/ponto/signals.py:232`, `app/colaboradores/services/pos_vinculo.py:37`, `app/ponto/turnos.py:1889`, `app/ponto/turnos.py:2976`
6. **[nota] Dois limites exigem ISO válido.** Um limite inválido pode interromper a avaliação antes dos demais pais. O emissor conhecido grava o formato esperado; ocorrência no legado [nao verificado]. `app/chamados/services/adesao.py:86`, `app/colaboradores/services/pos_vinculo.py:37`, `app/ponto/signals.py:232`

## 26/11 08:42 · sessao a20deb4d80b3 · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · construtor · origem=Code

Implementei **K2 e K3**. **K1 segue bloqueado na zona inviolável do motor.** [fechamento.py:973](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/services/fechamento.py:973) · [porta_export.py:340](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:340) · [motor_calculo_v2.py:3092](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/motor_calculo_v2.py:3092)

- **K1 — equivalência das vias.** O RED com o motor real obteve **440 min na dobra regular e 470 min na promoção**. O ponto para autorização do Ronald é `motor_calculo_v2.py:3092–3095`. A conta do suprimido percorre apenas `resultado.periodos` e grava no segundo par em `:3127`. Para reutilizar essa mesma conta, o motor precisa aplicá-la aos pares de folga antes de entregar o resultado. O RED roda por comando explícito. O mutante fica pendente com a cura. [Teste:391](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:391) · [Motor:3092](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/motor_calculo_v2.py:3092) · [Motor:3127](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/motor_calculo_v2.py:3127) [nao verificado]
- **K2 — igualdade exata.** O RED reproduziu **1,07 h × 1,06 h**. FM e DiaPago agora usam a mesma distribuição diária e a mesma soma em minutos antes do arredondamento mensal. A comparação é exata no centésimo para as três rubricas HE100, nas vias regular e promovida. Em memória, ambas deram **1,07 h**, e o comparador acusou as três rubricas ao alterar a lavra para **1,06 h**. Os dois mutantes reproduziram a divergência de soma e a omissão do comparador, respectivamente. [Teste:351](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:351) · [DiaPago:173](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/services/dia_pago.py:173) · [Comparador:510](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:510) [nao verificado]
- **K3 — vigia no limite.** O RED acusava **331,3 min de HE para teto de 330,3 min**. O feriado em dobra voltou à soma da base e passa no limite de **+1 min**. O controle com **331,4 min** acusa. O mutante que reaplica a acumulação incremental ao feriado ressuscitou o falso positivo, e o mutante do dia comum suprimiu a acusação esperada nesse ramo. [Vigia:340](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:340) · [Testes:421](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:421) [nao verificado]

**Comandos para o Code**, com `DJANGO_SETTINGS_MODULE` do ensaio já configurado. [Runner:80](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:80)

```bash
obra() { PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.mutantes_feriado_sem_limite_do_suprimido "$1"; }

obra red-k1
obra red-k2
obra k2-soma-em-horas
obra k2-tolerancia-he100
obra red-k3
obra k3-acumulacao-no-feriado
obra soma-reordenada
obra mensal-le-par
obra lavra-le-par
```

**Catraca**, linha registrada → linha atual, conferida pelo detector. [Registro:287](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/core/tests/test_medida_turno_do_par.py:287)

| Sítio | Linha |
|---|---:|
| [porta_export.py `medir`](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:594) | 586 → 594 |
| [dia_pago.py `_dia_de`](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/services/dia_pago.py:134) | 132 → 134 |
| [fechamento.py `_so_a_data`](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/services/fechamento.py:1588) | 1592 → 1588 |

Ficaram na fila o agrupamento civil e a cobertura de `periodos_ft` pelo vigia, o oráculo, a regra do TXT para 12x36 e G0.6. [Vigia:591](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:591) · [Oráculo:679](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/calculador/regras.py:679) · [TXT:420](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/export.py:420) · [G0.6:323](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:323)

Conferi os casos em memória, com ORM simulado no K2. A suíte Django, a certificação e o efeito sobre competências já lavradas ficam **[nao verificado]**. [nao verificado]

## 26/11 01:19 · sessao 859e73b64e12 · O30-F3-CONSTRUIR · construtor · origem=Code

**O30-F3 c2 entregue na cópia sobre `52821ca5b`.** Os 21 testes sem banco passaram. Os quatro REDs reproduziram as falhas na c1 com `rc=1`, e os 23 mutantes executados localmente falharam como esperado. O diff adiciona zero ocorrências de `except`. A suíte do Code, a certificação do opus, a integração com PostgreSQL e o comportamento em produção ficam [nao verificado]. `ponto/tests/test_atestado_f3_contrato.py:87`, `ponto/tests/mutantes_atestado_f3.py:27`, `ponto/services/ausencia.py:1401`, `ponto/services/atestados_lote.py:160`

| Bloqueio | Correção e decisão |
|---|---|
| **B1** | Criar/reabrir, editar e anexar travam raiz → filha. A edição relê a linha antes de derivar minutos e estado. Arquivo e metadados ficam na referência que a entrega abre. A troca de um arquivo aprovado exige atuar na própria raiz. O reenvio do app preserva o comprovante já recebido. `ponto/services/ausencia.py:283`, `:452`, `:502`, `:684`, `:842`, `:1086`, `:1387`, `ponto/views_atestado.py:42`, `ponto/tests/test_atestado_f3.py:106` |
| **B2** | `rederivar_fim` grava só os campos derivados e o carimbo de atualização. A falha da trilha dos seis campos aborta a transação. O selo detecta o `save()` integral real, e a prova do histórico verifica a chamada à função compartilhada. `ponto/services/ausencia.py:300`, `:1860`, `core/observ.py:94`, `ponto/tests/test_atestado_f3_contrato.py:134`, `:158`, `:171`, `ponto/tests/test_atestados_lote.py:13` |
| **B3** | “Sem documento” consulta `estado_inicial`. O teto conta os tipos que o catálogo marca como médicos. Arquivo sumido ou foto recusada pelo Pillow rende “Anexo indisponível”. HEIC e PDF recebem o link protegido. Na sonda com seleção simulada, um atestado e 21 férias deram HTTP 400 na c1 e no mutante, e HTTP 200 na c2, nos três formatos. `core/juizes.py:263`, `ponto/services/atestados_lote.py:28`, `:154`, `:171`, `ponto/views_atestado.py:119`, `ponto/tests/test_atestado_f3.py:288` |
| **B4** | O admin oculta os seis campos de todos os perfis. A mídia normaliza o caminho e exige `ver_ausencias` e vínculo com uma ausência no schema da requisição. `ponto/admin.py:39`, `core/media_protegida.py:8`, `ponto/tests/test_atestado_f3_contrato.py:411` |

**Comandos de reprodução**, a partir de `app/`. Os seletores e testes-alvo estão em `ponto/tests/mutantes_atestado_f3.py:27`.

```bash
f3() { PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 "$1"; }

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_atestado_f3_contrato

f3 c1_b1
f3 filha_substitui_aprovado
f3 reabrir_separa_arquivo
f3 trava_invertida
f3 editar_sem_releitura
f3 anexar_ignora_filha
f3 reabrir_le_filha
f3 gate_le_filha
f3 reenvio_regrava_upload
f3 reenvio_tenta_trocar_aprovado

f3 c1_b2
f3 save_integral
f3 trilha_facultativa
f3 evento_engole_obrigatorio
f3 historico_sem_delegacao

f3 c1_b3
f3 lote_flag_crua
f3 lote_ferias
f3 pdf_sem_storage
f3 pdf_imagem_recusada

f3 c1_b4
f3 admin_expoe_seis
f3 midia_staff
f3 midia_sem_normalizacao
f3 midia_sem_vinculo
```

**PDF com 20 atestados** [nao verificado]. O comando de medição parou ao importar `django.core`. O tempo do PDF real e o `f3 lote_ferias` com o ORM ficam para a régua do Code. `manage.py:11`, `ponto/tests/test_atestado_f3.py:288`, `:330`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_atestado_f3.AtestadoF3MedidaTest --settings=config.settings.ci
```

**Ficou fora.** Relógio na borda, semântica de vigência, competência geral, fuso de `decidido_em`, JSON de reversão E4b e efeito da trilha na reversão H06. `ponto/services/atestados_lote.py:34`, `:48`, `:149`, `ponto/views_atestado.py:103`, `chamados/management/commands/backfill_fio_morto_e4b.py:355`, `ponto/management/commands/rederivar_minutos_h06_comp11.py:152`

**Máscara para a sessão.** Acrescentar em `VAZIO` e `NULO`, respectivamente. As duas regras gravam `''` e `None`. `bin/sombra_regras_pessoais.py:49`, `:70`, `bin/sombra_mascara_pessoal.py:17`, `ponto/models.py:787`

```python
    ('ponto', 'Ausencia'): ('atestado_medico', 'atestado_crm', 'atestado_crm_uf', 'atestado_cid'),
    ('ponto', 'Ausencia'): ('atestado_emissao', 'atestado_dias'),
```

## 26/11 01:13 · sessao 0fff049b4d8a · DIA-DA-BATIDA-JUIZ-UNICO · construtor · origem=Code

A adesão pós-vínculo agora usa o dia que `dia_das_batidas` atribui a cada PK. A consulta inclui um dia em cada ponta e aplica os dois limites inclusivos do chamado ao resultado do juiz. Sem dois limites, verifica se há batida apurável na vida do colaborador. O fechamento continua por `transicionar_com_pauta` e `pai.reconciliar`. [colaboradores/services/pos_vinculo.py:37] [ponto/turnos.py:2972]

Dei baixa nas duas chaves ORM da linha 41 da base. A catraca agora compara arquivo, função, expressão AST e ocorrência, com classe, motivo e dono por sítio. A varredura estática passou com **252 sítios registrados, 102 classificados como J**. A célula `turno/marcos × um juiz por pergunta` está vermelha e a dívida J integra o selo que bloqueia o verde. Conferi **zero novos `except`** nos quatro trechos alterados, incluindo testes. [ponto/tests/test_contract_no_batida_date.py:145] [ponto/tests/test_contract_no_batida_date.py:507] [ponto/tests/test_contract_no_batida_date.py:546] [core/contratos_estruturais.py:225] [core/tests/test_selo_contratos_estruturais.py:81] [nao verificado]

### RED e GREEN

Executei o comando RED antes de alterar o serviço e o comando GREEN depois. Ambos pararam ao importar `django.core`, antes da conexão. **RED e GREEN em banco de testes, suíte do Code e confirmação independente do hash** [nao verificado]. [manage.py:11] [nao verificado]

Em memória, o pareador e o mapa reais atribuíram as duas PKs a **25/10** nos pares **19:00–07:00** e **23:58–00:03**, com 43.200 e 300 segundos brutos. Com persistência e porta simuladas, a seleção civil fechou o pai da folga nos dois casos e a cura devolveu `0` e o manteve vivo. O teste ORM também exige o controle de 25/10 com gate apto, uma só nota após a repetição, exclusão da retratada e o ramo sem dois limites. [ponto/turnos.py:1192] [ponto/turnos.py:3010] [ponto/tests/test_x1_folga_sem_adesao.py:439] [nao verificado]

Comandos para o envelope do Code, a partir de `app/`. [config/settings/ci.py:6] [ponto/tests/test_x1_folga_sem_adesao.py:473]

```bash
A=ponto.tests.test_x1_folga_sem_adesao.FecharAdesaoDiaDoTurnoTest
C=ponto.tests.test_contract_no_batida_date.ContractNoBatidaDateTest
t() { PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci "$@"; }

# RED na base com o teste novo aplicado [nao verificado]
t "$A.test_1900_a_0700_nao_fecha_adesao_da_folga" \
  "$A.test_2358_a_0003_nao_fecha_adesao_da_folga"

# GREEN com a cura
t ponto.tests.test_x1_folga_sem_adesao "$C" \
  core.tests.test_selo_contratos_estruturais colaboradores.tests.test_pos_vinculo
```

### Mutantes

Os oito casos de mutação da catraca passaram em memória com `unittest.TestCase`. Forçar a célula a verde fez falhar a asserção de dívida. [ponto/tests/test_contract_no_batida_date.py:554] [ponto/tests/test_contract_no_batida_date.py:651] [nao verificado]

```bash
t "$C.test_mutante_novo_em_funcao_registrada"
t "$C.test_mutante_registro_obsoleto_na_mesma_funcao"
t "$C.test_mutante_localdate_terminal_com_apelido_de_modulo"
t "$C.test_mutante_localdate_importado_e_renomeado_em_escopo_local"
t "$C.test_mutante_localtime_e_instante_em_variavel_derivada"
t "$C.test_mutante_data_local_com_apelido"
t "$C.test_mutante_lookup_relacional_em_dict_e_keyword"
t "$C.test_mutante_retorno_das_duas_chaves_da_adesao"
```

Os cinco mutantes do serviço foram exercitados com persistência simulada. Os comandos abaixo aplicam cada mutação em memória e devem falhar na asserção indicada quando rodados pelo Code. O resultado ORM dos mutantes está [nao verificado]. [colaboradores/services/pos_vinculo.py:39] [ponto/tests/test_x1_folga_sem_adesao.py:469]

```bash
mutar() {
  MUTANTE_ADESAO="$1" DJANGO_SETTINGS_MODULE=config.settings.ci \
    PYTHONDONTWRITEBYTECODE=1 python3 -B - "$2" <<'PY'
import os
import sys
from pathlib import Path

import django
from django.core.management import call_command

django.setup()
mutante = os.environ['MUTANTE_ADESAO']

if mutante == 'verde': [nao verificado]
    from core.contratos_estruturais import celula
    celula('turno/marcos', 'um juiz por pergunta')['verde'] = True [nao verificado]
else:
    from colaboradores.services import pos_vinculo as pos
    filtro = 'any(inicio <= dia <= fim for dia in dias.values())'
    antes, depois = {
        'civil': (filtro, 'batidas_apuraveis(colaborador).filter('
                          'timestamp__date__gte=per[0], timestamp__date__lte=per[1]).exists()'),
        'sem_vespera': ('inicio - um, fim + um', 'inicio, fim + um'),
        'sem_recorte': (filtro, 'bool(dias)'),
        'limites_exclusivos': ('inicio <= dia <= fim', 'inicio < dia < fim'),
        'sem_vida': ('tem_batida = batidas_apuraveis(colaborador).exists()', 'tem_batida = False'),
    }[mutante]
    fonte = Path(pos.__file__).read_text(encoding='utf-8')
    assert fonte.count(antes) == 1
    exec(compile(fonte.replace(antes, depois), pos.__file__, 'exec'), vars(pos))

call_command('test', sys.argv[1])
PY
}
mutar civil "$A.test_1900_a_0700_nao_fecha_adesao_da_folga"
mutar sem_vespera "$A.test_2358_a_0003_nao_fecha_adesao_da_folga"
mutar sem_recorte "$A.test_1900_a_0700_nao_fecha_adesao_da_folga"
mutar limites_exclusivos "$A.test_dia_do_trabalho_fecha_uma_vez"
mutar sem_vida "$A.test_sem_dois_limites_a_presenca_e_na_vida"
mutar verde "$C.test_celula_vermelha_enquanto_houver_divida" [nao verificado]
```

### Sonda de leitura da competência 10/2026

O recorte usa o corte de cada empresa. A sonda conta PKs e dias-colaborador distintos quando o dia civil ou o dia atribuído cai na competência. Para cada pai vivo com janela sobreposta ou com uma marca nesse recorte, compara a decisão sobre o **período integral gravado** e usa `reconciliaria()` para estimar transições. O snapshot manual evita o ramo de trava que o gate usa dentro de `atomic()`. **Contagens e transições da competência 10** [nao verificado]. [ponto/janelas.py:54] [colaboradores/services/pos_vinculo.py:37] [chamados/models.py:678] [chamados/models.py:761] [docs/BACKLOG.md:1858]

```bash
# Envelope do Code em app/, com SONDA_SCHEMA, SONDA_DB_USUARIO e SONDA_DB_SENHA.
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import json
import os
from collections import Counter
from datetime import date, timedelta

import django
from django.conf import settings
from config.settings import base

shared = ['django_tenants', 'tenants', *(
    app for app in base.INSTALLED_APPS if app.startswith(('django.contrib.', 'rest_framework'))
)]
tenant = [
    app for app in base.INSTALLED_APPS
    if app not in shared or app in {'django.contrib.auth', 'django.contrib.contenttypes'}
] + ['api']
cfg = {k: getattr(base, k) for k in dir(base) if k.isupper()}
cfg.update(
    DEBUG=False, LOGGING_CONFIG=None,
    SHARED_APPS=shared, TENANT_APPS=tenant,
    INSTALLED_APPS=list(dict.fromkeys(shared + tenant)),
    DATABASE_ROUTERS=['django_tenants.routers.TenantSyncRouter'],
    TENANT_MODEL='tenants.Cliente', TENANT_DOMAIN_MODEL='tenants.Domain',
    EMAIL_BACKEND='django.core.mail.backends.dummy.EmailBackend',
    VAPID_PUBLIC_KEY='', VAPID_PRIVATE_KEY='', GLPI_APP_TOKEN='',
    DATABASES={'default': {
        'ENGINE': 'django_tenants.postgresql_backend',
        'HOST': 'sombra_db', 'PORT': '5432', 'NAME': 'sombra',
        'USER': os.environ['SONDA_DB_USUARIO'],
        'PASSWORD': os.environ['SONDA_DB_SENHA'],
        'OPTIONS': {'options': '-c default_transaction_read_only=on'},
    }},
)
settings.configure(**cfg)
django.setup()

from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context
from chamados.catalogo.motor import VIVOS
from chamados.models import ChamadoColaborador
from ponto.janelas import janela_fechamento
from ponto.turnos import batidas_apuraveis, dia_das_batidas
from tenants.models import Cliente

UM = timedelta(days=1)
grupos = ('antes', 'depois', 'saem', 'entram', 'muda_dia', 'sem_dia')
pks = {g: set() for g in grupos}
civis = {g: set() for g in grupos}
jornadas = {g: set() for g in grupos}
transicoes = Counter(dict.fromkeys(('ambos', 'so_antes', 'so_depois', 'nenhum'), 0))
pais = Counter(dict.fromkeys(('no_recorte', 'sem_dois_limites', 'gate_recusou'), 0))

schema = os.environ['SONDA_SCHEMA']
with connection.cursor() as cur:
    cur.execute(
        'SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY'
    )
connection.set_autocommit(False)
try:
    with connection.cursor() as cur:
        cur.execute("SELECT current_database(), current_setting('transaction_read_only'), "
                    "current_setting('transaction_isolation')")
        if cur.fetchone() != ('sombra', 'on', 'repeatable read') or connection.in_atomic_block:
            raise RuntimeError('snapshot de leitura da sombra indisponível')
    if schema == 'public' or not Cliente.objects.filter(schema_name=schema).exists():
        raise ValueError('schema de ensaio inválido')

    with schema_context(schema), timezone.override(settings.TIME_ZONE):
        estoque = ChamadoColaborador.objects.filter(
            modulo_origem='sem_previsao_escala', subcategoria='nunca_bateu',
            status_local__in=VIVOS,
        ).select_related('colaborador__empresa')
        for pai in estoque:
            periodo = (pai.contexto_json or {}).get('periodo') or []
            if len(periodo) != 2:
                pais['sem_dois_limites'] += 1
                continue
            ini, fim = map(date.fromisoformat, periodo)
            colab = pai.colaborador
            a, z = janela_fechamento(10, 2026, colab.empresa)
            vivas = batidas_apuraveis(colab)
            antes = set(vivas.filter(timestamp__date__gte=ini, timestamp__date__lte=fim)
                        .values_list('pk', flat=True))
            mapa = dia_das_batidas(colab, ini - UM, fim + UM)
            depois = {pk for pk, dia in mapa.items() if ini <= dia <= fim}

            no_recorte = max(ini, a) <= min(fim, z)
            for pk, ts in vivas.filter(pk__in=antes | depois).values_list('pk', 'timestamp'):
                civil, dia = timezone.localdate(ts), mapa.get(pk)
                if not (a <= civil <= z or (dia is not None and a <= dia <= z)):
                    continue
                no_recorte = True
                em_antes, em_depois = pk in antes, pk in depois
                for grupo, conta in (
                    ('antes', em_antes), ('depois', em_depois),
                    ('saem', em_antes and not em_depois),
                    ('entram', em_depois and not em_antes),
                    ('muda_dia', dia is not None and civil != dia),
                    ('sem_dia', dia is None),
                ):
                    if conta:
                        pks[grupo].add(pk)
                        civis[grupo].add((colab.pk, civil))
                        if dia is not None:
                            jornadas[grupo].add((colab.pk, dia))
            if no_recorte:
                pais['no_recorte'] += 1
                candidato = bool(antes or depois)
                apto = candidato and pai.reconciliaria()
                pais['gate_recusou'] += int(candidato and not apto)
                assinatura = (bool(antes) and apto, bool(depois) and apto)
                transicoes[{(True, True): 'ambos', (True, False): 'so_antes',
                            (False, True): 'so_depois', (False, False): 'nenhum'}[assinatura]] += 1
finally:
    connection.rollback()
    connection.close()

print(json.dumps({
    'competencia': '10/2026',
    'pais': pais,
    'transicoes_por_pai': transicoes,
    'transicoes_antes': transicoes['ambos'] + transicoes['so_antes'],
    'transicoes_depois': transicoes['ambos'] + transicoes['so_depois'],
    'marcas': {
        g: {'batidas': len(pks[g]), 'dias_colab_civil': len(civis[g]),
            'dias_colab_jornada': len(jornadas[g]),
            'dias_colab_uniao': len(civis[g] | jornadas[g])}
        for g in grupos
    },
}, ensure_ascii=False, sort_keys=True))
PY
```

## 26/11 01:03 · sessao 4737c85695ab · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · verificador · origem=Code

**Parecer 5a — eu não daria o aval à c4b.** A promoção passa a usar o mesmo cálculo da dobra para os pares que recebe. O motor ainda entrega suprimido diferente nas duas vias do turno partido. Também reproduzi em memória duas divergências de fronteira: a igualdade mensal com a soma dos dias falha por 0,01 h e o ajuste do vigia do dia comum muda uma acusação em feriado. `app/ponto/motor_calculo_v2.py:3020–3024,3092–3129`; `app/ponto/services/fechamento.py:975–984` (c4b); `app/ponto/services/dia_pago.py:282–297` (c4b); `app/folha/porta_export.py:340–358` (c4b).

Usei a cópia e o diff fornecido, em somente leitura. Executei as contas puras em memória com Python 3.12. A execução Django, a comparação com os objetos Git `67dd8957e` e `160173bb1` e os efeitos em produção ficam **[não verificado]**. As referências marcadas **c4b** usam a numeração do anexo.

## 1. Quem escreve e quem lê

| Campo ou valor | Escritor e leitores |
|---|---|
| **Trabalhado, suprimido e limite comum**, em minutos | 12x36 e comercial lavram trabalhado e suprimido por par. O turno partido escreve o suprimido entre pares no fechamento do motor. `_limite_com_suprimido` lê o suprimido e devolve ao limite comum só até o intervalo declarado. A dobra lê o suprimido inteiro, descontado até o trabalhado do dia. `app/ponto/motor_calculo_v2.py:1840–1858,2884–2910,3092–3129,3359–3378,746–763`. |
| **HE100, HE100 feriado e HE100 noturna do motor**, em minutos | `_dobra_sem_o_suprimido` escreve `minutos_extra`, `minutos_extra_100`, `minutos_extra_100_feriado`, `hora_extra_100` e `minutos_extra_100_noturna` nos pares. `_aplicar_he_noturna` escreve o split intermediário e o split da HE comum. `ResultadoMes` e `lavrar` leem as rubricas dos pares regulares. `app/ponto/motor_calculo_v2.py:298–344,1063–1078,1885–1908`; `app/ponto/services/dia_pago.py:199–212`. |
| **Split da promoção e flag de prorrogação** | `get_motor_cct` monta a régua e o motor guarda `prorrogacao_noturna_pos5h`. `apurar_por_fatia` captura a flag do motor que julgou cada par de folga. A porta lê a flag por `id(p)`, as pontas registradas, o trabalhado, o suprimido e os intervalos carimbados pelo motor. Devolve `(HE100, HE100 noturna)` em minutos. Mensal e lavra recebem o mesmo mapa, sem nova consulta à CCT. `app/core/regua_cct.py:449–479`; `app/ponto/motor_calculo_v2.py:797–801,1939–1947`; `app/ponto/services/fechamento.py:302–334,975–984,1222–1227,1299–1304` (c4b); `app/ponto/services/dia_pago.py:135–168` (c4b). |
| **FechamentoMensal e DiaPago** | O fechamento soma a promoção em `horas_extras`, `horas_extras_100`, `horas_extras_100_feriado` e `horas_extras_100_noturna`, em horas com duas casas. `lavrar` grava as três HE100 como horas `FloatField`. `detalhe.folga_feriado_min` guarda o **trabalhado bruto da promoção** em minutos e `pago_do_dia` o converte em horas para tela, PDF, app e conferência da porta. A partição tira os promovidos de `horas_folga_trabalhada` e de `ft_sem_escala`. `app/ponto/services/fechamento.py:961–1001` (c4b), `1154–1186`; `app/ponto/services/dia_pago.py:282–298` (c4b), `374–389,921–927`; `app/ponto/models.py:325–337,412–425`; `app/folha/porta_export.py:106–124`. |
| **`porta_export`** | `fechamento_x_soma_dias_pagos` lê o mensal e agrega a lavra por campo, inclusive os três HE100. `minuto_em_duas_rubricas` lê HE, intra, trabalhado e a marca de dobra nos pares regulares. No ramo comum consulta previsto e intervalo declarado. `medir` acrescenta colab e dia e publica a lista. Os limiares são `0,015 h` para FM × DiaPago e `+1 min` para dupla rubrica. `app/ponto/services/dia_pago.py:34–44,579–592`; `app/folha/porta_export.py:245,431–438,496–512,579–592,715–724`; `app/folha/porta_export.py:340–358` (c4b). |

**Segundo escritor fora da porta — ausente nas rubricas auditadas.** `restaurar_fechamento` aceita valores de restauração pela porta do mensal. O retrato de exportada atualiza os três campos de grade. O oráculo tem derivação própria e grava outra `versao` pela mesma `lavrar`. `app/ponto/services/fechamento.py:1375–1414,1260–1313`; `app/ponto/services/dia_pago.py:341–389,434–467,544–574`; `app/ponto/models.py:319–323`.

## 2. Classes de reprovação e selos

- **Segundo juiz registrado — ausente no diff.** A promoção usa `_dia_de`, que prioriza o mapa do motor, e o predicado existente `feriado_vira_folga`. Já existem decisões paralelas no vigia por data civil e na supressão de feriado do TXT para 12x36. Esta última está registrada em `PENDENTES`. `app/docs/JUIZES.md:114–119`; `app/core/juizes.py:310–315,495–498`; `app/ponto/services/dia_pago.py:109–132`; `app/ponto/services/fechamento.py:963–969` (c4b); `app/folha/porta_export.py:579–590`; `app/folha/export.py:420–423`.
- **Relógio fora da borda — ausente no trecho novo.** A flag vem do motor da fatia. `_dia_de` usa o instante da entrada como degradação quando falta o mapa. As leituras de `now()` do fechamento já existiam. O diff acrescenta zero `except`. `app/ponto/services/fechamento.py:302–334` (c4b), `623–625,1154–1206`; `app/ponto/services/dia_pago.py:126–132`.
- **Derivação paralela — presente em parte.** A função de janela e a distribuição do suprimido têm um corpo cada no motor. A promoção replica o `min(noturna_cheia, HE100_do_par)` e monta a chamada de split com HE50 zero. O motor faz o mesmo teto em sua escrita e a casca dele recebe também o campo HE50 do par. O oráculo continua com outra derivação, usando o trabalhado da folga na HE100. `app/ponto/motor_calculo_v2.py:409–451,727–763,1070–1078,1885–1901`; `app/ponto/services/dia_pago.py:149–168` (c4b); `app/ponto/calculador/regras.py:679–712`.
- **Dois significados — unidades distintas, fronteira de dia divergente no vigia.** `folga_feriado_min` é trabalhado bruto, HE100 é líquida do suprimido e HE100 noturna é um subconjunto em minutos de relógio. `horas_noturnas` usa minutos legais. Com corte 21, entram os dias de jornada de 21 a 20, **ambos inclusivos**. À meia-noite a janela noturna reparte duração em trechos contíguos sem duplicar o instante, e o feriado da dobra pertence ao dia do início do turno. O vigia agrupa pela data da entrada e pode separar a cauda ou descartá-la na borda da competência. `app/ponto/models.py:319–337`; `app/ponto/services/dia_pago.py:249–251,283–297` (c4b); `app/ponto/janelas.py:10–33`; `app/ponto/motor_calculo_v2.py:89–118,953–989,1039–1061,2505–2537`; `app/folha/porta_export.py:579–592`.

**Mordida dos mutantes.** A tabela indica o efeito na asserção-alvo. Conferi a aritmética isolada. A execução dos nove processos Django fica **[não verificado]**. `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:22–84` (c4b).

| Mutante | O que a asserção pega |
|---|---|
| `mensal-le-par`, `lavra-le-par` | Opus cai de 420 para 360 min de HE100 noturna no mensal ou na lavra, respectivamente. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:239–255` (c4b). |
| `sem-noturno`, `dia-civil` | FN2 dá `120/0` em vez de `120/120` de HE100/noturna. O segundo mutante perde o diurno disponível do par com entrada no dia civil seguinte. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:289–303` (c4b). |
| `sem-limite`, `relogio` | FD6 dá `100/120` em vez de `100/100`. G0.6 dá `270/270` em vez de `270/240`. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:300–318` (c4b). |
| `ultima-fatia`, `sem-intervalos` | A primeira fatia perde 120 min noturnos pela flag errada. A segunda ganha 60 min ao ignorar a pausa. O mensal esperado de 11 h noturnas vira 9 h ou 12 h, respectivamente. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:319–345` (c4b). |
| `soma-reordenada` | Reproduzido. Com HE `170,3`, a soma incremental acusa e o `sum` compensado devolve `None`. O mesmo hunk muda também o ramo **feriado**: trabalhados `(120; 120,2; 120,1)`, HE `(120; 120,1; 91,2)` e intra `30` no último par. A base devolve `None`, a c4b acusa `331,3` contra teto publicado `330,3`. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:348–361` (c4b); `app/folha/porta_export.py:340–355`, `340–358` (c4b). |

O V1 verifica que o espelho foi consultado, que a chamada mutada recebeu `noturno=` e que a acusação contém `540/480`. O controle exige HE100 feriado de 8 h e lista vazia. A assinatura `_antiga(periodos, noturno=None)` **já está na cópia**. O diff acrescenta `autospec` e a prova da chamada. O V1 cobre a dobra regular. Uma promoção errada que sobe o mesmo valor a FM e DiaPago pode passar pelos dois vigias, pois o comparador vê igualdade e a dupla rubrica só visita `periodos`. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:247–281,417–439` (último intervalo c4b); `app/folha/porta_export.py:496–512,579–592`.

**Lacunas que deixam código errado passar.** A bateria nova aceitaria agrupar todos os promovidos da competência num único lote: os casos com suprimido usam um dia e o teste de duas fatias usa `S=0` nos dois dias. A maioria dos REDs substitui `calcular_mes` e injeta `S` pronto, o que deixa passar a diferença real do turno partido abaixo. Contra a lógica da base, Opus, FN2, FD6 e três pares discriminam a cura, enquanto G0.6 e duas fatias com `S=0` são controles. O runner `base` devolve só o resultado global da suíte, sem conferir a causa de cada falha. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:239–345` (c4b); `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:63–84` (c4b).

## 3. O noturno do dia muda?

**O adicional noturno do dia fica igual com os mesmos pares julgados.** O motor continua preenchendo `minutos_noturnos_relogio` e `minutos_noturnos_legais` pelas pontas efetivas. FM e DiaPago continuam lendo o legal em `horas_noturnas`. O diff move a **HE100 noturna** da promoção. Quando acabam os diurnos, a HE100 noturna pode cair em relação à dobra cheia. Num par `22:00–05:00`, com 420 min trabalhados e `S=60`, ficam 360 min de HE100 noturna, 420 min de relógio e 480 min legais com o fator padrão. `app/ponto/motor_calculo_v2.py:44–46,319–321,740–763,1039–1061`; `app/ponto/services/fechamento.py:927`; `app/ponto/services/dia_pago.py:249–251,149–168` (porta em c4b).

O G0 anexo fixa **HE100 noturna / relógio**, em minutos, em `300/300`, `180/180`, `360/360`, `120/120 + 240/240`, `270/270` e `240/270`. No sexto caso, a saída registrada `05:30` faz o split recuar 270 min até `01:00`. As pontas efetivas do relógio são `00:30–05:00`. No F11c, o cálculo conserva 9,00 h de HE100, 1,00 h de HE100 noturna e 1,14 h de adicional noturno. Execução dos testes completos **[não verificado]**. `app/ponto/tests/test_feriado_dobra_noturna_g0.py:44–95` (c4b); `app/ponto/motor_calculo_v2.py:392–406,435–451,1039–1061,1068–1076`; `app/ponto/tests/test_o280_bateria_da_lei.py:1801–1823`.

## 4. Censo de leitores e efeito

- **Chamadores diretos das interfaces alteradas.** O fechamento é o único chamador de produção da porta da promoção e entrega o mapa às duas chamadas de `lavrar`. Espelho e PDF também chamam `apurar_por_fatia`, leem o resultado e os carimbos e ignoram a flag nova. O resultado bruto que recebem fica igual. `app/ponto/services/fechamento.py:975–984,1222–1227,1299–1304` (c4b); `app/ponto/services/espelho.py:1199–1202`; `app/relatorios/pdf_espelho.py:417–423`.
- **Espelho, PDF, calendário, app e cartão.** As rubricas diárias exibidas vêm de `DiaPago`, via `rubricas_do_dia` e `pago_do_dia`. A projeção inclui HE100 total e adicional noturno, sem coluna de HE100 noturna. Espelho e PDF ainda apuram o motor para geometria e resumo de períodos livres. O cartão de competência também consulta o fechamento online. `app/ponto/services/dia_pago.py:921–957`; `app/ponto/services/espelho.py:823–831,1200–1216,1315–1320`; `app/relatorios/pdf_espelho.py:844–856,901–914`; `app/colaboradores/services/calendario.py:688–698`; `app/api/views.py:1450–1457`; `app/relatorios/cartao_pela_celula.py:70–101,142–179`.
- **TXT e holerite.** O TXT lê FM e emite o adicional por `horas_noturnas`. A HE100 noturna fica sem evento próprio. Há uma decisão de emissão própria para 12x36, por ciclo e régua do fim da competência, enquanto o motor lê a régua do começo de cada fatia. O holerite entrega o PDF armazenado. Alteração de valor no holerite externo **[não verificado]**. `app/folha/export.py:341–380,420–448,714–739`; `app/ponto/services/fechamento.py:733–750`; `app/holerite/views.py:1445–1459`.
- **Painel e leitores online.** `fechamento_lido` reapura pela mesma porta em modo leitura. O painel mostra total de extras e adicional noturno, e o ranking mostra HE100 e HE100 feriado. Nenhum desses recortes exibe a parcela que muda no Opus. A foto de auditoria lê todos os campos concretos e pode registrar a mudança. O comparador da porta lê explicitamente a parcela alterada e pode continuar com lista vazia quando os dois lados mudam juntos. `app/ponto/services/fechamento.py:1752–1778`; `app/ponto/views.py:1018–1036`; `app/templates/ponto/partials/painel_fechamento.html:33–34`; `app/folha/services/ranking_he.py:137–152,206–229`; `app/chamados/services/foto_e4b.py:324–333`; `app/folha/porta_export.py:496–512`.

## 5. Casos fora do golden

| Caso | Resultado pela cópia e pelo diff |
|---|---|
| **12x36 noturno que cruza a meia-noite** | Feriado no início do plantão `19:00–07:00`, pausa `01:00–02:00`, prorrogação desligada e hora reduzida afastada. São 660 min trabalhados e 360 noturnos. Com dobra habilitada, HE100/noturna `660/360`. Com a régua simples, HE100 **feriado** `0`. O ciclo 12x36 está fora da promoção `feriado_vira_folga`. `app/ponto/motor_calculo_v2.py:2884–2955,3743,3813–3822`; `app/core/regua_cct.py:467–479`; `app/escala/models.py:28–34,1400–1406`. |
| **Feriado no dia 20 ou 21** | Corte 21, plantão `20/11 19:00 → 21/11 07:00`. Com dobra habilitada e feriado só em **20/11**, a dobra inteira pertence a novembro. Com feriado só em **21/11**, esse plantão dá zero HE100 feriado. Um plantão que comece em **21/11** pertence a dezembro. O selo existente verifica a atribuição única do 20→21 para horas trabalhadas. `app/ponto/janelas.py:10–33`; `app/ponto/motor_calculo_v2.py:953–973,2529–2537`; `app/ponto/tests/test_dupla_contagem_dia20.py:30–44`. |
| **Suprimido parcial no turno partido** | Use o desenho já testado `10:00–14:00` e `14:30–18:20`, com mínimo de 60 min e os dois pares reconhecidos como no selo. Em feriado regular em dobra, o laço grava `S=30`, portanto `470−30 = 440` min de HE100. Se o vínculo folga no feriado e os dois pares chegam em `periodos_ft`, o mesmo laço os omite e o cálculo por par do partido não escreve `S`. A promoção recebe `S=0` e paga **470 min**. A divergência está na alimentação da porta. `app/ponto/tests/test_motor_partido_intervalo_pelo_juiz.py:23–39,62–63`; `app/ponto/motor_calculo_v2.py:2550–2599,3004–3024,3092–3129,3219–3225`; `app/ponto/services/fechamento.py:947–984` (c4b). |
| **Intermitente** | O controle existente usa `21/04`, `07:00–19:00`. São 12 h trabalhadas, HE100 e HE100 feriado zero, inclusive com `feriado_em_dobra=True`. O motor injeta conjunto de feriados vazio e o vínculo intermitente passa fora da classificação de folga trabalhada. `app/ponto/tests/test_s6_intermitente_feriado.py:54–56,115–142`; `app/ponto/motor_calculo_v2.py:1019–1023,2466–2470,2550–2558,3689–3701`. |
| **FM × soma DiaPago com segundos** | Dois feriados promovidos com `S=0`, cada um começando às 22:00, com saídas às `22:30:18` e `22:33:36`. Para HE100 e HE100 noturna, arredondar `sum([30,3; 33,6])/60` dá **1,07 h** e somar as horas lavradas e arredondar dá **1,06 h**, reproduzido em memória. A tolerância de `0,015 h` deixa a diferença passar. `app/ponto/services/fechamento.py:975–984` (c4b); `app/ponto/services/dia_pago.py:282–292` (c4b), `579–592`; `app/folha/porta_export.py:245,496–512`. |

## 6. Bloqueia × fila

- **Bloqueia a equivalência das vias reais** — o turno partido com pausa parcial paga 440 min na dobra regular e 470 min na promoção quando o motor envia os pares a `periodos_ft`. `app/ponto/motor_calculo_v2.py:2597–2599,3092–3129,3219–3225`; `app/ponto/services/fechamento.py:975–984` (c4b).
- **Bloqueia C2(g) como igualdade exata** — o exemplo com segundos dá FM `1,07 h` e soma `1,06 h`, com o comparador verde. `app/ponto/services/fechamento.py:975–984` (c4b); `app/ponto/services/dia_pago.py:579–592`; `app/folha/porta_export.py:245,496–512`.
- **Bloqueia o ajuste do vigia no escopo de dia comum** — a ordem nova também muda o veredito de feriado no limite de 1 min. `app/folha/porta_export.py:340–355`, `340–358` (c4b).
- **Fila** — vigia por data da entrada e sem `periodos_ft`, oráculo com outra derivação, TXT com regra de 12x36 no fim da competência e divergência G0.6 de split × relógio. `app/folha/porta_export.py:579–592`; `app/ponto/calculador/regras.py:679–712`; `app/folha/export.py:341–380,420–423`; `app/ponto/motor_calculo_v2.py:392–406,1039–1076`.
- **Catraca** — o anexo desloca `medir` 583→586, `_so_a_data` 1586→1592, `apurar_por_fatia` 213→215 e `recalcular_fechamento_mes` `[664,666,672,692,763,802,848,860]`→`[669,671,677,697,768,807,853,865]`. Execução **[não verificado]**. `app/core/tests/test_medida_turno_do_par.py:287,329–331,563`.

## 26/11 01:00 · sessao ae5d8bfb59d4 · O30-F3-CONSTRUIR · verificador · origem=Code

**O30-F3-CONSTRUIR c1 — bloqueia.** A cura permite que o app substitua o anexo de uma raiz já aprovada por meio de uma filha aberta. Também introduz uma ordem de trava inversa entre reabertura e edição e deixa um `save()` integral capaz de sobrescrever os seis campos. `app/api/views.py:3102–3114,3138–3146`, `app/ponto/services/ausencia.py:1745–1757` (base), `app/ponto/services/ausencia.py:491–496,728–734,1104–1119` (diff).

Conferi a cópia em leitura e usei as linhas novas do diff fornecido. Hash por Git, `mapa.r` separado, execução da suíte completa e ocorrências em produção [nao verificado]. [nao verificado]

## 1. Quem escreve e quem lê

| Campo ou valor | Escritores e leitores |
|---|---|
| `atestado_medico`, `atestado_crm`, `atestado_crm_uf`, `atestado_cid`, `atestado_emissao`, `atestado_dias` | Para **cada um dos seis**, o POST lê o campo presente, o formulário valida, `criar_ausencia` inclui no `INSERT` e o helper `_gravar_dados_atestado` grava pela criação/reabertura, edição e anexo. O helper lê os valores atuais dos seis para detectar mudanças e preservar os omitidos. Drawer, CSV e PDF leem os seis. **Segundo escritor físico presente:** `rederivar_fim` faz `save()` integral com a instância que o comando carregou antes, podendo recolocar os seis valores antigos mesmo sem nomear nenhum campo. A migration inicializa os campos. `app/ponto/views_atestado.py:33–39`, `app/ponto/forms.py:12–21`, `app/ponto/services/ausencia.py:267–311,491–496,548–552,728–734`, `app/templates/ponto/partials/painel_ausencia.html:89–130`, `app/ponto/services/atestados_lote.py:121–155,189–207` (diff), `app/ponto/services/ausencia.py:1745–1753`, `app/ponto/models.py:787–795,892–932`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29–50`, `app/ponto/migrations/0079_ausencia_atestado_dados.py:12–24` (base). |
| Leituras indiretas dos seis | O Django admin lê todos os campos concretos. A foto E4b lê todos e o comando pode copiá-los ao JSON de reversão. O reparo H06 também lê todos, para conferir quais colunas mudaram. `app/ponto/admin.py:37–48`, `app/core/admin.py:46–75`, `app/chamados/services/foto_e4b.py:247–252,284–288`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:59–67,355–372`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:94–95,166–177` (base). |
| `documento`, `documento_referencia_id` e raiz | `criar_ausencia` insere o arquivo e `anexar_documento` passa a substituir o da raiz. Fora das três portas, `_reabrir` também grava `documento` quando chamado por `reverter_rejeicao(documento=...)`, e o `save()` integral de `rederivar_fim` regrava arquivo e FK. `SET_NULL` limpa a FK ao apagar a raiz. No diff, a atribuição explícita da FK que aparece é de teste. Leem a raiz o drawer, a entrega protegida, a lista, a aprovação/rejeição em grupo e o histórico/lote. A API de reenvio, a fila do app, o gate de aprovação e a troca de plantão ainda leem o arquivo da linha individual. `app/ponto/services/ausencia.py:483–497,754–779,1249–1316,1745–1753`, `app/ponto/models.py:777–781,815–819`, `app/ponto/views.py:2100–2109,2146–2155,2786–2815,2862–2866`, `app/templates/ponto/ausencias.html:168–171`, `app/api/views.py:1053–1067,1852–1858`, `app/ponto/portas/celula.py:719–746` (base), `app/ponto/services/ausencia.py:282–287,1104–1119`, `app/ponto/services/dia_decidido.py:624–630`, `app/ponto/tests/test_atestado_f3.py:77–99` (diff). |
| Trilha | `_trilha_dados_atestado` emite `dados_atestado_editados` com os nomes dos campos e o ramo de upload emite `documento_anexado` com os nomes dos arquivos. Ambos persistem por `core.observ.evento`. A tela de auditoria lê ação e descrição sob `ver_saude_sistema`. O H06 lê qualquer evento posterior da ausência e pode barrar a reversão de minutos após uma edição só de metadados. `evento` é best-effort e os chamadores ignoram o retorno, portanto a gravação pode confirmar sem a trilha. `app/ponto/services/ausencia.py:289–311,623–627,1110–1116` (diff), `app/core/observ.py:94–105,124–139`, `app/core/views.py:686–710,733–738`, `app/templates/core/log_auditoria.html:55–74`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152–159` (base). |
| `pode_ver_anexo` e `LIMITE_LOTE` | A view produz a flag com `tem_acao(ver_ausencias)`. CSV/PDF recebem a flag com default `False`. A view e o template também a leem para o ZIP. `LIMITE_LOTE = 20` tem uma definição de produção. A view lê até 21 **lançamentos-raiz de qualquer tipo ou status** e recusa os três formatos acima de 20. O teste substitui a constante por 1. Os testes de medida usam 20. Repeti em memória o ensaio ZIP do anexo com o serviço da base, que o diff preserva. Deu **0,374 s no total, 0,019 s/anexo** para 20 arquivos de 1 MB. Tempo do PDF e tempo em produção [nao verificado]. `app/ponto/views_atestado.py:88–95,115–131`, `app/ponto/services/atestados_lote.py:24,53–65,129–155,175–207`, `app/templates/relatorios/atestados_lote.html:20–25`, `app/ponto/tests/test_atestado_f3.py:221–250`, `app/ponto/tests/test_atestado_f3_contrato.py:164–186` (diff), `app/ponto/services/atestados_lote.py:135–173` (base). |

## 2. Classes que já reprovaram obra

| Classe | Veredito |
|---|---|
| Segundo juiz | **Presente para a exigência de documento na saída do lote.** O lote herda a flag crua do catálogo. O juiz registrado é `estado_inicial`, que isenta férias, e o drawer já o consulta. Assim, férias sem arquivo podem aparecer em “Sem documento” no lote e ficar fora da cobrança do drawer. Permissão, janela de competência e assinatura do arquivo continuam delegadas aos respectivos pontos existentes. `app/core/juizes.py:259–264`, `app/ponto/models.py:723–729`, `app/ponto/services/ausencia.py:894–898,926–944,709–729`, `app/ponto/catalogo/ausencias.py:101`, `app/ponto/janelas.py:54–57` (base), `app/ponto/services/atestados_lote.py:32–50,73–75,196–213`, `app/ponto/views_atestado.py:24–26,88–95` (diff). |
| Segundo escritor fora da porta | **Presente.** `rederivar_fim` grava a linha inteira. O selo exclui justamente o módulo que o contém e ainda exige menção textual a algum dos seis campos para examinar os demais arquivos. `app/ponto/services/ausencia.py:1745–1757` (base), `app/ponto/tests/test_atestado_f3_contrato.py:56–75,126–143` (diff). |
| Relógio fora da borda | **Presente, herdado e alcançável.** A view lê `timezone.localdate()`, mas `mes=` ou `ano=` explícito em branco passa pelo `setdefault` e chega vazio a `janela()`. O serviço usa `datetime.date.today()` do host. Em virada de mês com datas locais diferentes, pode selecionar outra competência. Fuso efetivo do host [nao verificado]. `app/ponto/views_atestado.py:49–50,98–112`, `app/ponto/services/atestados_lote.py:39–50` (diff), `app/config/settings/base.py:92–95` (base). |
| Derivação paralela | **Ausente** para `previa()['faltando']` e para o dia decidido. PDF chama `previa`, a tela lê a mesma chave e a montagem continua em `linha_do_tempo`. **Presente no recorte SQL se a pergunta for vigência.** O filtro trata todo `data_fim=NULL` como dia inicial e omite `afastamento_inss` aberto antes de `de`. `ausencias_vigentes` inclui esse caso. A exclusão já existia no lote. `app/ponto/services/atestados_lote.py:53–75,210–216`, `app/ponto/services/dia_decidido.py:624–672`, `app/templates/relatorios/atestados_lote.html:31–40` (diff), `app/ponto/services/atestados_lote.py:54–63`, `app/ponto/turnos.py:3207–3231`, `app/ponto/catalogo/ausencias.py:92–95` (base). |
| Selo/RED sem mordida | **Presente.** Por inspeção, `gravacao`, `cid_csv`, `editar`, `cid_pdf`, `foto`, `sem_documento`, `empresa` e `limite` alteram valores ou ramos com asserção correspondente. `link` só altera o texto, embora o teste também confira o URI. `escritor_externo` injeta um `update` literal por alias, caso que o AST detecta, e deixa escapar o `save()` integral real. O RED `base` cobre só assinatura das portas e CSV. Executei isoladamente o selo legado do histórico em memória: passou com a função do diff e também com o corpo trocado por `return []`, pois o nome exigido está no docstring. Execução dos mutantes anexos [nao verificado]. `app/ponto/tests/mutantes_atestado_f3.py:21–42,75–110`, `app/ponto/tests/test_atestado_f3_contrato.py:78–158`, `app/ponto/tests/test_atestado_f3.py:118–231`, `app/ponto/services/atestados_lote.py:68–75` (diff), `app/ponto/tests/test_atestados_lote.py:116–124` (base). |
| Ordem de trava | **Presente.** Na reabertura da filha rejeitada com dados, criação trava raiz → filha. Na edição da filha com dados, o `save` trava filha → raiz. As duas ordens permitem um ciclo entre transações. Deadlock observado [nao verificado]. `app/ponto/services/ausencia.py:282–287,491–496,728–734` (diff), `app/ponto/services/ausencia.py:392–399,1306–1327` (base). |
| ESMERIL-SEMÂNTICA | **Emissão e dias do atestado têm unidade própria.** Emissão é data civil opcional, inclusive futura, sem fuso e fora do filtro do lote. `atestado_dias` aceita 0 e fica separado de `dias_corridos` e de minutos. O período livre usa datas civis inclusivas nas duas pontas. **Ambiguidades presentes** em fim nulo, competência geral e teto: fim nulo pode ser dia único ou aberto, competência geral usa corte 21 quando empresas podem ter cortes distintos, e o teto conta lançamentos mesmo sem anexo. `decidido_em` é o instante da decisão/criação e o CSV já o formatava sem conversão explícita de fuso. `app/ponto/models.py:685–689,750–763,787–795,834–838`, `app/ponto/janelas.py:10–21,36–57`, `app/ponto/services/dia_decidido.py:558–567`, `app/ponto/services/atestados_lote.py:131` (base), `app/ponto/forms.py:19–21`, `app/ponto/views_atestado.py:56–63,107–119`, `app/ponto/services/atestados_lote.py:53–65,121–125,151–155`, `app/ponto/tests/test_atestado_f3.py:111–116` (diff). |

## 3. Permissões

- **Drawer POST:** exige `editar_ausencias` e `ver_ausencias`, com 403 na falta de qualquer uma. **Drawer GET e leitura do anexo:** exigem `ver_ausencias`, com 404 para o usuário autenticado sem a ação. As buscas por PK e o salto à raiz carecem de recorte por empresa. `app/ponto/views_atestado.py:22–40` (diff), `app/ponto/views.py:2094–2119,2137–2162` (base).
- **Lote:** sem as duas ações de leitura, responde 403. Só `ver_relatorios` permite CSV/PDF sem os seis campos, imagens e links, e recebe 403 no ZIP. Os seletores e a busca partem de todas as empresas e colaboradores do schema. A UI admite inativas e diagnósticas que `empresas_visiveis()` exclui dos seletores. O contrato desse helper ressalva os exports. O ORM SaaS tem isolamento por schema configurado. Execução entre tenants e política de acesso por empresa em produção [nao verificado]. `app/ponto/views_atestado.py:45–48,69–95,107–131`, `app/ponto/services/atestados_lote.py:129–155,189–207` (diff), `app/colaboradores/queries.py:78–87`, `app/config/settings/saas.py:34–41,67–94` (base).
- **CID fora das três portas:** staff autenticado com só `ver_relatorios` pode ler CID **contido no comprovante** pela rota de mídia, se conhecer o caminho do arquivo. A rota autoriza `is_staff` e entrega do storage compartilhado sem conferir ação, empresa ou schema do arquivo. O **campo estruturado** também fica visível no Django admin a staff com a permissão nativa adicional `ponto.view_ausencia`. Acessos reais [nao verificado]. `app/config/urls.py:24,38`, `app/core/media_protegida.py:6–17,38–49`, `Caddyfile:10–17`, `app/config/settings/saas.py:109–111`, `app/ponto/models.py:777–792`, `app/ponto/admin.py:37–48`, `app/core/admin.py:31–34,46–75` (base).
- **Sombra:** o restore copia a tabela, mas os seis campos estão fora do inventário de máscara. Conferi em memória que nenhum dos seis nomes casa com a regex do selo. Um dump com CID preenchido pode chegar à sombra com o valor cru e passar pelo selo. Dados efetivamente copiados [nao verificado]. `bin/sombra.sh:244–275`, `bin/sombra_regras_pessoais.py:48–81,109,160–164`, `bin/sombra_selo_pessoal.py:23–36` (base).

## 4. Dia pago, abono e falta

- **`dia_decidido.py`: nenhum efeito direto identificado.** A mudança seleciona raízes para o histórico e permite pular a leitura do tipo do arquivo. Leitores da mudança são o drawer e o lote. Espelho e calendário continuam lendo `decididos_do_periodo` e seu cartão, fora do hunk. `app/ponto/services/dia_decidido.py:624–672`, `app/ponto/services/atestados_lote.py:68–75` (diff), `app/ponto/views.py:2035–2043`, `app/ponto/services/dia_decidido.py:570–621`, `app/ponto/services/espelho.py:661`, `app/colaboradores/services/calendario.py:563` (base).
- **`ausencia.py`: os seis campos ficam fora do insumo da folha, mas a mudança nas portas pode mover dinheiro.** O `UPDATE` só dos seis não dispara `post_save`. O upload ainda passa pelo `save` e pelo cartório, e a ida entre os dois estados de espera, por si, fica fora da folha. No intervalo legado, a filha pode continuar devendo documento e derrubar a aprovação do grupo, impedindo o abono. Na edição, uma instância carregada como pendente pode ser aprovada por outro ato antes do `save(update_fields=...)`. Se o editor então mudar o tipo para `falta` com minutos omitidos, o modelo decide pela instância pendente, deixa zero, e a gravação parcial preserva o status aprovado do banco. O rateio da folha, o fechamento e `DiaPago` leem esse zero. Ocorrência em produção [nao verificado]. `app/ponto/services/ausencia.py:298–311,725–734,1104–1119` (diff), `app/ponto/services/ausencia.py:115–139,914–917,968–994`, `app/ponto/views.py:2786–2815`, `app/ponto/signals.py:143–147,250–275`, `app/ponto/turnos.py:2528–2537`, `app/ponto/models.py:892–911`, `app/folha/export.py:116–143`, `app/ponto/services/fechamento.py:893–895,1224–1229`, `app/ponto/services/dia_pago.py:274–292` (base).

## 5. Contraexemplos fora do golden

1. **Raiz aprovada com anexo e filha sem anexo próprio.** O DP cancela e reverte só a filha. A reabertura olha o arquivo da filha e a coloca em espera. O app aceita o upload pela filha aberta, mas o serviço do diff troca o arquivo da raiz aprovada. Em outra entrada, `criar_ausencia` reabrindo a filha com CID e arquivo grava o CID na raiz e o arquivo na filha, que a entrega protegida não abre. `app/ponto/views.py:2753–2763,2897–2918,2146–2155`, `app/ponto/services/ausencia.py:1296–1327`, `app/api/views.py:3102–3114,3138–3146` (base), `app/ponto/services/ausencia.py:491–496,1104–1119` (diff).
2. **Um arquivo sumido derruba o PDF inteiro.** Basta um `FileField` preenchido cujo arquivo não esteja no storage. Também basta um JPEG truncado que passe pelos bytes de assinatura e falhe no Pillow. O ZIP já prevê arquivo sumido e o golden usa fotos válidas. `app/ponto/services/atestados_lote.py:159–172,196–204`, `app/ponto/tests/test_atestado_f3.py:169–191` (diff), `app/ponto/services/ausencia.py:709–729`, `app/ponto/services/atestados_lote.py:156–164` (base).
3. **Um atestado e 21 dias de férias do mesmo colaborador já recusam CSV, PDF e ZIP.** O espelho de férias cria uma raiz por dia, e o teto é aplicado antes de distinguir tipo ou presença de arquivo. `app/ponto/services/ausencia.py:1379–1394` (base), `app/ponto/views_atestado.py:115–121`, `app/ponto/services/atestados_lote.py:53–65` (diff).

## 6. Bloqueios e fila

- **Bloqueia — privacidade:** alinhar a leitura dos seis e dos comprovantes nas portas, na mídia e no admin, e cobrir o CID na máscara da sombra. `app/core/media_protegida.py:6–17,42–48`, `app/core/admin.py:46–75`, `bin/sombra_regras_pessoais.py:48–81,160–164` (base).
- **Bloqueia — raiz, trava e dinheiro:** eliminar a substituição via filha aberta, a divisão arquivo/metadados, a inversão de trava e a derivação financeira sobre instância desatualizada. `app/ponto/services/ausencia.py:491–496,725–734,1104–1119` (diff).
- **Bloqueia — escrita e prova:** fechar o `save()` integral fora das três portas, assegurar a trilha exigida e fazer o selo morder a escrita implícita e a perda da delegação do histórico. `app/ponto/services/ausencia.py:1745–1757`, `app/core/observ.py:94–105`, `app/ponto/tests/test_atestados_lote.py:116–124` (base), `app/ponto/tests/test_atestado_f3_contrato.py:126–143` (diff).
- **Bloqueia — lote:** alinhar a exigência de documento e o universo do teto, cobrir arquivo indisponível e comprovar o tempo do PDF. `app/core/juizes.py:263` (base), `app/ponto/views_atestado.py:115–124`, `app/ponto/services/atestados_lote.py:32–36,196–213`, `app/ponto/tests/test_atestado_f3.py:232–250` (diff).
- **Fila — semântica herdada:** fixar a leitura de relógio na borda e explicitar se o período recorta lançamentos ou vigência, qual competência vale no geral e em que fuso sai `decidido_em`. `app/ponto/services/atestados_lote.py:32–43,54–63,131`, `app/ponto/turnos.py:3207–3231`, `app/ponto/janelas.py:36–57` (base), `app/ponto/views_atestado.py:98–112` (diff).
- **Fila — auditoria operacional:** revisar a cópia dos seis no JSON de reversão E4b e o efeito da trilha médica na reversão H06. `app/chamados/management/commands/backfill_fio_morto_e4b.py:355–372`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152–159` (base).

## 26/11 00:58 · sessao 33edb894e08e · - · - · origem=Ronald (tmux)

- **Portas e permissão.** Drawer POST exige `editar_ausencias` e `ver_ausencias` e responde 403. Drawer GET e arquivo protegido exigem `ver_ausencias` e respondem 404. As três resolvem o registro por PK e seguem a referência sem recorte de empresa. No lote, só `ver_relatorios` permite tela/CSV/PDF sem os seis campos, imagens ou links. ZIP exige `ver_ausencias`. `app/ponto/views_atestado.py:22-40,88-95,119-131` (diff), `app/ponto/views.py:2094-2119,2137-2162`, `app/ponto/services/atestados_lote.py:130-154,189-207` (diff).
- **Empresa e tenant.** A tela e os filtros da nova view admitem empresas inativas e de diagnóstico, excluídas do dropdown canônico. A própria fixture usa duas empresas de diagnóstico. O ORM SaaS está configurado para consultar o schema do tenant. Isolamento executado [nao verificado]. `app/ponto/views_atestado.py:45-48,69-83,107-118` (diff), `app/colaboradores/queries.py:78-87`, `app/ponto/tests/test_atestado_f3.py:50-55,207-220` (diff), `app/config/settings/saas.py:34-41,67-94`.
- **CID fora da porta.** `/media/` autoriza `is_staff` e encaminha bytes cujo caminho seja conhecido, sem checar `ver_ausencias`, empresa ou propriedade do arquivo. Pode entregar CID presente no anexo a staff com apenas `ver_relatorios`. O Django admin é outro leitor dos seis campos, por permissão nativa de visualizar `Ausencia`. Atribuições e ocorrências reais [nao verificado]. `app/config/urls.py:24,38`, `app/core/media_protegida.py:6-18,38-49`, `Caddyfile:10-17`, `app/ponto/admin.py:37-48`, `app/core/admin.py:46-75`.
- **Contraexemplo de autorização introduzido pelo diff.** Raiz aprovada com comprovante e filha aberta. É possível obter esse estado cancelando e revertendo só a filha, cuja reabertura lê o documento local vazio. O app aprova a operação pela filha aberta, mas o novo serviço substitui o comprovante da raiz aprovada. `app/ponto/views.py:2740-2765,2897-2918`, `app/ponto/services/ausencia.py:1296-1327`, `app/api/views.py:3083-3114,3135-3146`, `app/ponto/services/ausencia.py:283-288,1105-1118` (diff).
- **Escrita e trava.** `rederivar_fim` é segundo escritor implícito dos seis campos e do documento por `save()` integral, com chamador no comando de cura. O selo exclui todo o módulo. Reabertura com dados trava raiz→filha e edição com dados trava filha→raiz, com possibilidade de deadlock. `_reabrir` ainda grava o arquivo na filha quando os dados foram gravados na raiz. `app/ponto/services/ausencia.py:1249-1282,1306-1327,1745-1753`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-31,42-50`, `app/ponto/services/ausencia.py:491-496,728-734` (diff), `app/ponto/tests/test_atestado_f3_contrato.py:126-143` (diff).
- **Dois juízes.** O novo SQL repete vigência e omite `afastamento_inss` aberto que começou antes da janela. `_exige_documento` lê o booleano cru do catálogo, enquanto `estado_inicial` isenta férias. O PDF reutiliza `previa()['faltando']`, herdando a classificação divergente. `app/ponto/services/atestados_lote.py:32-36,53-75,212-213` (diff), `app/ponto/turnos.py:3207-3231`, `app/core/juizes.py:259-264`, `app/ponto/catalogo/ausencias.py:92-93,101`, `app/ponto/services/ausencia.py:898,926-952`.
- **Relógio e limite.** Campo `mes=` ou `ano=` em branco aciona relógio do host dentro de `janela()`, embora a view já tenha lido `localdate()`. O limite conta lançamentos-raiz de todos os tipos, então 21 faltas sem atestado já recusam os três formatos. Tempos reais [nao verificado]. `app/ponto/views_atestado.py:49-50,98-121` (diff), `app/ponto/services/atestados_lote.py:39-65` (diff), `app/config/settings/base.py:92-95`.

## 26/11 00:58 · sessao 60e59a9f1ff5 · - · - · origem=Ronald (tmux)

- **Escritor efetivo fora das três portas.** `rederivar_fim` faz `save()` completo, inclusive dos seis campos, do arquivo e da referência. O comando carrega os objetos antes de gravá-los, permitindo sobrescrita com valores antigos após uma edição concorrente. O selo ignora `ausencia.py` inteiro e exige menção ao nome de um campo, então deixa esse caminho passar. [app/ponto/services/ausencia.py:1745–1753], [app/ponto/models.py:892–932], [app/ponto/management/commands/curar_ausencia_sem_fim.py:29–50], [diff app/ponto/tests/test_atestado_f3_contrato.py:56–75,121–138].
- **Arquivo e raiz.** Criação insere `documento`, e o novo `anexar_documento` atualiza a raiz. `_reabrir` também escreve o arquivo e é alcançável por `reverter_rejeicao(documento=...)`. Ao reabrir uma filha rejeitada com arquivo e CID, a cura grava CID na raiz e `_reabrir` grava o arquivo na filha. Drawer, leitura do arquivo e lote consultam a raiz. A referência pode virar `NULL` por exclusão da raiz via `SET_NULL`. [app/ponto/services/ausencia.py:483–497,1249–1310], [diff app/ponto/services/ausencia.py:492–496,1107–1119], [app/ponto/views.py:2100–2109,2146–2155], [diff app/ponto/services/dia_decidido.py:624–630], [app/ponto/models.py:815–818].
- **Ordem de trava invertida.** `editar_ausencia_registro(filha, **dados)` atualiza e trava a filha antes da raiz. `criar_ausencia` com metadados trava primeiro a raiz da filha rejeitada e depois atualiza a filha em `_reabrir`. As duas ordens formam um ciclo concorrente possível. Ocorrência real [nao verificado]. [diff app/ponto/services/ausencia.py:282–285,492–495,728–734], [app/ponto/services/ausencia.py:988,1307–1309,1320–1322].
- **Leitores laterais dos seis campos.** O Django admin expõe todos os campos a quem tiver a sua permissão de visualização, independente de `ver_ausencias`. A foto da E4b lê todos os campos e pode copiá-los para o JSON de reversão. O reparo H06 lê os seis na conferência genérica de que apenas minutos/carimbo mudaram. Audiência dos arquivos e concessões de produção [nao verificado]. [app/ponto/admin.py:37–48], [app/core/admin.py:31–34,46–75], [app/chamados/services/foto_e4b.py:247–252,284–288], [app/chamados/management/commands/backfill_fio_morto_e4b.py:59–67,355–372], [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:94–95,166–177].
- **Trilha e efeito indireto em minutos.** O evento médico registra nomes de campos sem conteúdo em `core.observ.evento`, com savepoint best-effort. A UI de auditoria mostra ação e descrição sob `ver_saude_sistema`. O reparo H06 lê qualquer evento posterior de `Ausencia`, portanto só editar dados médicos pode impedir a reversão de minutos depois de um apply. [diff app/ponto/services/ausencia.py:289–312], [app/core/observ.py:94–139], [app/core/views.py:686–705], [app/templates/core/log_auditoria.html:55–74], [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152–159].
- **Leitura do CID no arquivo por outro caminho.** A mídia protegida aceita qualquer `is_staff` autenticado e encaminha o caminho ao Caddy, sem checar `ver_ausencias` ou vincular o arquivo à empresa/schema. Um staff com apenas `ver_relatorios` pode ler CID contido no próprio comprovante se conhecer seu caminho. A rota de mídia não devolve o campo estruturado `atestado_cid`. Acesso observado em produção [nao verificado]. [app/config/urls.py:38], [app/core/media_protegida.py:6–17,42–48], [Caddyfile:7–17], [app/ponto/models.py:777–791].
- **Aprovação legada pode mudar.** A API valida que a filha está aberta e pertence ao colaborador. O novo serviço muda o status da raiz e deixa a filha em `aguardando_documento`. A aprovação em grupo recusa esse estado e reverte a transação, impedindo o abono nesse caso. [app/api/views.py:3102–3114,3138–3146], [diff app/ponto/services/ausencia.py:1107–1119], [app/ponto/services/ausencia.py:914–916,968–974], [app/ponto/views.py:2786–2815].
- **Juiz e limite.** O lote herda a leitura da flag bruta do catálogo onde o juiz A4 exclui férias. Um único colaborador com um atestado e 21 dias de férias no recorte ultrapassa o teto, pois férias são criadas dia a dia e a view conta todas as raízes. [app/core/juizes.py:258–264], [app/ponto/services/atestados_lote.py:15–29,76–84], [app/ponto/services/ausencia.py:894–898,926–944,1379–1394], [diff app/ponto/views_atestado.py:115–120].

_8306 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
