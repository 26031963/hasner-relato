# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 01:53** · publicado em **27/11/2026 01:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 01:53 · sessao bb8490509145 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

## A1–A4

| Achado | Contraprova e alcance |
|---|---|
| **A1 — a cura do `DECL_60` é parcial.** | No caso do anexo, 19h–07h, previsto 660, E19h, pausa julgada 07h20–07h40, S08h20, a regra dá **780 trabalhados, base 720, pausa descontada 20, intra 60, HE 0**. A c2l dá esses números na releitura. O mutante `intra_integral` volta a dar **intra 40 e HE50 20**, `rc=1`. Já o mutante que grava `_base_he[id(p)] = 0` passa, `rc=0`. O teste confere os 720 **injetados** em `p.minutos_base_da_he`, e a HE esperada de zero aceita a base interna zerada. Procede como **RED sem mordida sobre a base usada no excesso**. `app/ponto/tests/test_ramo2b_c2h_saida.py:196–209` · `app/ponto/tests/ramo2b_c2l_mutantes.py:33–36,67–70` · `app/ponto/calculador/periodo.py:752–759,833–837` · `app/docs/PROMPTS.md:3071`. |
| **A1 — a releitura ainda perde pausa na borda da entrada.** | 12x36, 19h–07h, previsto 660, pausa cadastrada de 60, E18h40, S18h50, E19h20, S08h20. Entregando a pausa já julgada a `calcular_periodo`, a janela corta a entrada às 19h e conserva **19h–19h20** como pausa. Regra e motor dão **780 trabalhados, base 700, pausa 20, intra 40, HE 0**. `reler` descarta a pausa cujo início coincide com a entrada efetiva e dá **800 trabalhados, base interna 720, pausa 0, intra 60, HE 0**. O campo público `minutos_base_da_he` continua em **700** no mesmo objeto. Procede contra **L-032, L-031 e L-097(b)** e demonstra a derivação paralela. Formação desse par pelas batidas cruas **[nao verificado]**. `app/ponto/motor_calculo_v2.py:2074–2082,2964–2999` · `app/ponto/calculador/periodo.py:304–310,747–759,835–837` · `app/ponto/calculador/alimentacao.py:136–146` · `app/docs/LEIS.md:79–80,115`. |
| **A2 — o repasse funciona quando o chamador entrega o mapa.** | 12x36, 19h–07h, previsto 660, E19h50, S00h, E01h, S07h20, parcial 19h20–19h50. São 630 trabalhados, base 610 e atraso bruto 50. A regra abate 30 e dá **20**. A releitura com mapa dá **20**, sem mapa dá **30** pelo teto. `descarta_parcial` cai em **30 × 20**, `rc=1`. As costuras antigas que julgam pontualidade continuam omitindo o mapa. Procede contra o **corte do abono** nesses caminhos de ensaio. `app/ponto/tests/test_ramo2b_c2h_saida.py:211–228` · `app/ponto/calculador/periodo.py:474–477,792–826` · `app/ponto/tests/test_o280_bateria_da_lei.py:3967–3968,4638–4640,5477–5483` · `app/docs/PROMPTS.md:3031`. |
| **A3 — o selo agora pega a intra zerada, mas aceita o Partido alterado.** | Partido 19h–07h, vão cadastrado 00h–03h, previsto 540, E19h, S00h20, E03h, S07h30. O mensal real recebe vão de 160 e grava **20 de intra**. `zera_intra_partido` dá **0** e derruba os dois subcasos, `rc=1`. Com S07h30, o controle aceita tanto os **560 trabalhados + 30 fora** do main quanto os **590 trabalhados + 0 fora** da cópia, porque soma as duas grandezas. A lacuna original do mensal foi coberta. A proteção do Partido intocado depende do B1, que está vermelho. Procede contra o **aval (a)**. `app/ponto/tests/test_ramo2b_c2h_saida.py:32–51,147–163` · `app/ponto/tests/ramo2b_c2l_mutantes.py:40–43` · `app/ponto/motor_calculo_v2.py:1893–1905,3103–3118,3184–3227` · `app/docs/PROMPTS.md:3087`. |
| **A4 — o mutante de chamada morta cai. A borda da competência continua errada.** | O teste entrega a `medir` um período de 05/11, E19h–S08h20, base e previsto 720, intra 60, declarado 0 e HE injetada 80. A regra dá teto **0** e **1 diagnóstico**, que entra em `falhas_proprias` e `falhas`. Com `testemunha_morta`, a lista fica vazia, `rc=1`. O mesmo selo fica verde enquanto o caso de 20/11 abaixo perde o diagnóstico pelo dia civil. A lacuna original da chamada morta foi coberta. O defeito de data procede contra **L-085/L-002**. `app/folha/tests/test_porta_do_export.py:25–77` · `app/ponto/tests/ramo2b_c2l_mutantes.py:44–46` · `app/folha/porta_export.py:340–362,585–591,713–721,788–790` · `app/docs/LEIS.md:60,117`. |

Outro desvio de **A2**, mesmo com o mapa presente. Comercial com jornada de D às 23h50–07h50, pausa 02h–03h, previsto 420 e D+1 de folga. Batidas E00h02, S02h, E03h, S07h50 em D+1, atribuídas a D. A parcial de D, 23h55–00h02, cobre 7 dos 12 minutos de atraso. Com 408 trabalhados, a regra dá **5**. Na chamada aos juízes em memória, `reler` deu **5** com mapa e **12** sem mapa. O motor deu **0** porque consulta os marcos de `e.date()` e retorna ao encontrar a folga de D+1. Procede contra **L-084/L-085** e o corte do abono. Atribuição automática dessas batidas a D no mensal **[nao verificado]**. `app/ponto/motor_calculo_v2.py:2241–2260,1242–1275,1304–1308` · `app/ponto/calculador/alimentacao.py:278–295` · `app/ponto/calculador/periodo.py:797–803` · `app/docs/LEIS.md:111,116–117` · `app/docs/PROMPTS.md:3031`.

## Números por regime

Valores em minutos, em dia comum. Nos casos com pausa entregue ao par, a comparação começa depois de a pausa ter sido julgada. `app/ponto/motor_calculo_v2.py:2964–2974,3445–3463`.

| Cadastro e batidas | Regra e resultado da cópia |
|---|---|
| **12x36, `DECL_60`**. 19h–07h, previsto 660, E19h, pausa julgada 07h20–07h40, S08h20. | **Regra, motor e `reler` nesse caso:** trabalhado **780**, base **720**, intra **60**, HE **0**. A pausa reduz o trabalhado integral em 20 e o gozo dentro da base é zero. O teto da HE é `660 + 60 = 720`. A L-084 mantém o julgamento porque as duas pontas não estão ambas a mais de 3h. `app/docs/LEIS.md:79–80,115–116` · `app/docs/PROMPTS.md:3071` · `app/ponto/motor_calculo_v2.py:2968–2999,3052–3068` · `app/ponto/tests/test_ramo2b_c2h_saida.py:196–209`. |
| **Comercial, célula congelada 19h–07h**, pausa declarada 00h–01h, previsto 660. Mesmo par do `DECL_60`. | **Regra e releitura:** trabalhado **780**, base **720**, intra **60**, HE **0**. Com esse par entregue, o mensal usa **480** como previsto para a célula que cruza. O teto vira **540** e a conta dá **180 HE**, repartidas pelo código em **120 HE50 + 60 HE100**. No RED de batidas, com E19h50, S00h, E01h, S07h20, a regra dá **630 / 610 / 0 / 0**, e o mensal dá **630 / 610 / 0 / 130**. Procede contra **L-024**. `app/escala/utils.py:2284,2322–2332,2368–2374` · `app/ponto/motor_calculo_v2.py:1978–1985,3430,3464–3485,3555–3560,3617–3653` · `app/ponto/tests/test_ramo2b_c2l_red_motor.py:116–127` · `app/docs/LEIS.md:75`. |
| **Partido, régua relógio**, 19h–07h, vão 00h–03h, previsto 540, E19h50, S00h, E03h, S07h20, parcial 19h20–19h50. | **L-097(b) em geral:** trabalhado **510**, base **490**, intra **0**, HE **0**, atraso **20**. **Aval (a) nesta obra e main:** trabalhado **490**, fora **20**, atraso **20**, intra e HE **0**. **Cópia:** trabalhado **510**, fora **0**, atraso **20**, intra e HE **0**. O desvio é de **escopo do aval**. No outro caso do Partido, E19h/S00h20/E03h/S07h30, o mensal dá **base 560, intra 20, HE 0**. Se esse resultado for passado a `reler`, a v3 mede cada par isolado, perde o vão julgado e dá **base 560, intra 0, HE50 20**. A divergência é de ensaio e também ocorre com S07h00. `app/docs/PROMPTS.md:3087` · `app/docs/LEIS.md:79–80,115` · `app/ponto/motor_calculo_v2.py:3103–3118,3184–3227,3347–3365` · `app/ponto/calculador/periodo.py:334–362,747–759,833–865` · `app/ponto/tests/test_ramo2b_c2l_red_motor.py:51–61`. |

## Escritores, leitores e significado

| Valor | Escrita e leitura na cópia |
|---|---|
| `minutos_intrajornada_indenizada` | **Escrevem:** o construtor inicializa e a rotina de bloqueio zera. 12x36 e Comercial escrevem por par, Partido escreve no segundo par do vão julgado, `periodo_do_dia` produz `DiaV3` e `reler` sobrescreve o `PeriodoCalculo` em memória, inclusive no zeramento de duplicados. `app/ponto/motor_calculo_v2.py:247,1612–1620,2997,3222–3224,3483` · `app/ponto/calculador/periodo.py:183,360–362,739–759`. **Leem em minutos:** `_limite_com_suprimido`, `reler`, a testemunha do export e o PDF para decidir se exibe a coluna. **Convertem para horas:** `ResultadoMes`, `DiaPago`, resumo do espelho e `diff_calculador`. A lavratura usa o mapa da jornada. A testemunha usa a data civil da entrada. `app/ponto/motor_calculo_v2.py:391–398,1978` · `app/ponto/calculador/periodo.py:836` · `app/folha/porta_export.py:340–351` · `app/relatorios/pdf_espelho.py:844–859` · `app/ponto/services/dia_pago.py:173–186,223–225` · `app/ponto/services/espelho.py:104` · `app/ponto/management/commands/diff_calculador.py:244–254`. |
| `_base_he` e `minutos_base_da_he` | O **mapa local** nasce por dia em `reler`, recebe minutos por `id(p)` e só a soma de trabalho o lê para excesso e dobra. O **campo do par** é escrito pela porta da tripla e lido por `_trabalhado_da_base_da_he`, que usa o trabalhado integral quando o campo é `None`. Esse leitor serve ao R3, Partido, Comercial e export. O caso da pausa na borda dá **720 internos × 700 gravados no campo**. `app/ponto/calculador/periodo.py:633,752–756,833–849` · `app/ponto/motor_calculo_v2.py:280–285,1927–1956,2747–2765,3167,3216,3320–3324,3560,3585–3589` · `app/folha/porta_export.py:339–351`. |
| `saida_da_base_da_he` | A janela calcula o **instante** do corte, já com extra declarada, e `_janela_do_par` o limita ao par efetivo. `_gravar_base_da_he` manda gravar pela porta da tripla. O bloqueio limpa pela mesma porta. O construtor gerado aceita escrever o campo diretamente. Leem **R3**, `segmentos_do_motor` para a geometria e a ponta da HE noturna, e `reler`. O corte mede duração até o instante. Se a pausa começa no marco, contribui **0** para a base. Se termina no marco, entra o trecho anterior. O R3 usa a mesma tripla para **soma de pares fechados** num portador aberto, com o instante do último par creditado que tinha cauda. Ocorrência dessa combinação no dado fornecido **[nao verificado]**. `app/ponto/janela_he.py:192–200` · `app/ponto/motor_calculo_v2.py:244–246,1612–1620,1893–1956,2747–2765` · `app/ponto/calculador/alimentacao.py:106–115,168–179` · `app/ponto/calculador/periodo.py:650–653,752–756`. |
| `parciais_por_dia` | A porta do DP escreve `hora_ini/hora_fim`. `parciais_com_ponta_por_dia` monta `{date: [Ausencia]}` por `a.data`. Motor e `rubricas_por_dia` carregam dessa fonte. Em `reler`, o único chamador que fornece o mapa é o teste novo, que o monta à mão. Também omitem o mapa as chamadas da bateria sem `marcos`, nas quais a pontualidade fica fora da releitura. O juiz usa minutos desde 00h do dia da jornada, com madrugada em **1440+**. O marco previsto usa bordas inclusivas. A batida usa direção, com **S no início** e **E no fim** fora. O abatimento mede fatias **`[k,k+1)`** e usa o período, enquanto `Ausencia.minutos` participa da classificação como parcial. `app/ponto/services/ausencia.py:1039–1058,1099–1104` · `app/ponto/services/credito_parcial.py:15–32,54–70` · `app/ponto/motor_calculo_v2.py:481–505,1326–1343` · `app/ponto/calculador/alimentacao.py:535–540,563–566` · `app/ponto/tests/test_ramo2b_c2h_saida.py:222–224` · `app/ponto/tests/test_o280_bateria_da_lei.py:3565,3665–3667,3900,3967,4306,4456,4638,5481–5483` · `app/ponto/turnos.py:2622–2639,2653–2711`. |

## REDs do motor

| RED | Regra, falha e mordida |
|---|---|
| **B1** | O Partido do caso acima dá **490/20/20/0** no main e **510/0/20/0** na cópia, na ordem trabalhado, fora, atraso, HE. Morde o mensal e pede o **aval (a)**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:51–61` · `app/ponto/motor_calculo_v2.py:1893–1905,3103–3118` · `app/docs/PROMPTS.md:3087`. |
| **B2** | 19h–07h, S08h20, pausa julgada 07h20–07h40. Em 12x36, E01h10, regra **base 350 / trabalho 410 / pausa 20 / HE 0**, código **350/430/0/0**. No Comercial, E03h10, regra **230/290/20/0**, código **230/310/0/0**. Morde a perda de desconto depois de a pausa ter sido julgada. A formação automática do par **[nao verificado]**. Procede contra **L-032/L-097(b)**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:63–85` · `app/ponto/motor_calculo_v2.py:2968–2999,3449–3485` · `app/docs/LEIS.md:80,115`. |
| **B3** | Par 19h–08h20, trabalhado 800. O construtor aceita `(720, None, None)` e o leitor numérico vê **720**, enquanto o geométrico conserva **800**. Morde a recusa ausente no construtor. O RED cobre a forma com só `minutos_base_da_he` preenchido. Procede contra **L-007**. Chamador de produção que passe tripla parcial **[nao verificado]**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:87–94` · `app/ponto/motor_calculo_v2.py:244–246,280–285,1927–1943` · `app/ponto/calculador/alimentacao.py:168–174` · `app/docs/LEIS.md:65`. |
| **B4** | Comercial 5x2, cinco turnos 22h–06h, o último até 06h30, HE 0. Pela fórmula semanal escrita no motor e pela L-097(b), **2.430 − 2.640 = −210**. O código reaproveita a base de HE como trabalhado semanal e dá **2.400 − 2.640 = −240**. Morde a troca de significado de `trab_por_dia`. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:96–110` · `app/ponto/motor_calculo_v2.py:3558–3560,3655–3685` · `app/docs/LEIS.md:115`. |
| **Herdado — limite Comercial** | O caso de 630 trabalhados e base 610 pede **HE 0** pela célula de 660. A última asserção morde **120 HE50 + 10 HE100**, na cópia e no main executado. Procede contra **L-024**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:116–127` · `app/ponto/motor_calculo_v2.py:3617–3653` · `59600163c:app/ponto/motor_calculo_v2.py:3504` · `app/docs/LEIS.md:75`. |
| **Herdado — HE noturna** | 12x36, 16h–04h, extra declarada 60, sem prorrogação pós-05h, E16h20/S05h30. A HE50 total é **40**. A regra situa os 40 em **04h20–05h**. A saída crua desloca a faixa para **04h50–05h30** e o motor conta só **10 noturnos**, na cópia e no main executado. Morde o split, contra **L-035/L-097(b)**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:129–144` · `app/ponto/motor_calculo_v2.py:1084–1094` · `59600163c:app/ponto/motor_calculo_v2.py:1051` · `app/docs/LEIS.md:83,115`. |
| **Herdado — export** | Competência fecha em 20/11. Turno de 20/11, 23h50–07h50, pausa 02h–03h sem gozo, E00h02/S08h20 em 21/11. Com HE injetada **30**, intra **60**, base **468**, previsto **420**, o teto é **0** e deve sair **1 diagnóstico**. Sai **0** na cópia e no main executado. O RED morde `medir` pelo descarte na borda, contra **L-085/L-002**. A produção da HE indevida pelo motor nesse caso **[nao verificado]**. `app/ponto/tests/test_ramo2b_c2l_red_motor.py:146–160` · `app/folha/porta_export.py:340–362,587–591` · `59600163c:app/folha/porta_export.py:544` · `app/docs/LEIS.md:60,117`. |

## Travas e alcance

- **Juiz e escritor.** A2 delega ao mesmo juiz de pontualidade. Em A1, a releitura de ensaio deriva a base em paralelo à resposta registrada do motor e reescreve o mesmo atributo de intra em memória. Os **720 × 700** da borda mostram o efeito. A testemunha do export também responde pelo dia civil quando já tem acesso ao resultado com dia da jornada. B3 prova a escrita da tripla fora da porta pelo construtor, embora a catraca só encontre a porta na varredura de sítios de produção. `app/core/juizes.py:406,1214–1216` · `app/ponto/calculador/regras.py:112–127,274–285` · `app/ponto/calculador/periodo.py:543–548,747–759` · `app/folha/porta_export.py:344,587–591` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:605–638` · `app/docs/LEIS.md:60,65`.
- **Escopo da entrega.** O hunk c2l acrescenta **0 `except`**. Na porta, o instante continua capturado em `medir` e repassado aos leitores. Inversão de ordem de trava, certificação, pouso prematuro e efeito no gravado **[nao verificado]**. O caminho genérico por evento pode recalcular fora da guarda explícita da 09/2026. Isso dá alcance estático ao desvio do Partido e não comprova dinheiro regravado. `app/folha/porta_export.py:389–394,529,549,693` · `app/docs/PROMPTS.md:3065,3105–3106` · `CLAUDE.md:421–431` · `app/ponto/services/fechamento.py:36–50,559–595,1822–1848` · `app/docs/LEIS.md:155–156`.
- **Prova executada.** Rodei em memória, com substitutos para Django e as fronteiras de I/O, **13 testes, 8 falhas de asserção**, e o recorte de A deu **6 testes verdes**. Os quatro mutantes fornecidos deram `rc=1`. Os três herdados estão numa única classe, em desacordo com o brief que pediu uma classe por RED. A execução Django de `Ran 99`, o DIFF de frota e os números de produção ficam **[nao verificado]**. `app/ponto/tests/ramo2b_c2l_mutantes.py:50–70` · `app/ponto/tests/test_ramo2b_c2l_red_motor.py:50–160` · `app/folha/tests/test_porta_do_export.py:57–77`.

**REPROVADO** — A1 perde pausa na borda e deixa o teste verde com base zerada, A2 deixa chamadores sem a parcial e diverge do motor na virada, A3 aceita o Partido fora do aval, B1–B4 seguem procedentes, e os três herdados mordem os defeitos de limite Comercial, ponta da HE noturna e dia civil no export. [nao verificado]

## 27/11 01:43 · sessao 5872738329b7 · O30-F3-CONSTRUIR · leitura · origem=Code

O saldo informado é de **+11 registros e +1 documento**. A empresa 2 teve +7 registros e +1 documento, a 3 teve +2 e a 4 teve +2. Os totais e o ZIP × CSV 1 a 1 dentro de cada execução deixam em aberto quais PKs entraram, se algum saiu e qual explica o saldo de documentos. [nao verificado]

### O que faz uma raiz entrar na F3

- A F3 seleciona raízes com `documento_referencia_id IS NULL`. Para a requisição da sonda, a view restringe o lote à empresa pedida, aos tipos médicos do catálogo e ao status `aprovada`. A linha do tempo devolve o próprio registro raiz e informa a presença de documento com `bool(a.documento)`. [app/ponto/services/dia_decidido.py:624](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:624) [app/relatorios/views.py:382](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:382) [app/ponto/services/dia_decidido.py:679](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:679)
- A F3 aplica `data <= fim` e aceita qualquer uma destas condições: `data >= ini`, `data_fim >= ini` ou `data_fim IS NULL` com `tipo in Ausencia.TIPOS_EM_ABERTO`. O filtro sem a terceira condição está reproduzido no mutante local, que aceita `data_fim >= ini` ou, com fim nulo, `data >= ini`. **O caso que distingue os dois é a raiz com `data < 2026-10-21`, `data_fim = null` e `tipo_em_aberto = true`**, satisfeitos os demais filtros. O teste da F3 exige essa inclusão para um `afastamento_inss` e também testa o fim fechado exatamente na borda inicial. [app/ponto/services/atestados_lote.py:35](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:35) [app/ponto/turnos.py:2842](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/turnos.py:2842) [app/ponto/tests/mutantes_atestado_f3_c14.py:45](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/tests/mutantes_atestado_f3_c14.py:45) [app/ponto/tests/test_atestado_f3.py:294](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/tests/test_atestado_f3.py:294)
- O código literal do MAIN nos pontos indicados (`atestados_lote.py:46`, `dia_decidido.py:645` e `:675`) fica **[nao verificado]**. A comparação acima usa o predicado reproduzido no mutante da cópia. [app/ponto/tests/mutantes_atestado_f3_c14.py:45](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/tests/mutantes_atestado_f3_c14.py:45)

**Regra aplicável.** O registro do aval `O30-F3-CONSTRUIR-V2-2511` diz que o V2 altera a vez da obra. O corte `AUSENCIAS-DRAWER-E-LOTE` veda montagem própria da janela, e a **L-002** exige que os leitores usem a mesma autoridade. O relato da c15 registra a adoção de `_q_cobertura`, cujo conjunto de tipos em aberto vem do cadastro de fins indeterminados. Para uma raiz com a assinatura acima, a inclusão é comportamento deliberado e testado da F3. O enquadramento de cada PK adicional e do documento adicional fica **[nao verificado]**. [app/docs/PROMPTS.md:3001](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/docs/PROMPTS.md:3001) [app/docs/CORTES.md:349](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/docs/CORTES.md:349) [app/docs/LEIS.md:60](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/docs/LEIS.md:60) [app/docs/RELATO.md:13620](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/docs/RELATO.md:13620) [app/ponto/catalogo/ausencias.py:430](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/catalogo/ausencias.py:430) [app/ponto/tests/test_atestado_f3.py:294](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/tests/test_atestado_f3.py:294)

### Script

O script usa as linhas da view real, ordena por `(empresa, pk)` e imprime o JSONL após sair do `atomic` por rollback. Mantém as guardas do anexo, a 0079 condicional e o storage em memória. `dias_corridos` fica disponível para conferir a apresentação da linha, que usa esse campo quando `data_fim` é nulo. Validei a sintaxe e exercitei os dois caminhos da 0079 com objetos simulados. Execução com Django e PostgreSQL nas duas árvores **[nao verificado]**. [app/relatorios/views.py:404](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:404) [app/ponto/services/dia_decidido.py:679](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:679) [app/ponto/migrations/0079_ausencia_atestado_dados.py:4](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/migrations/0079_ausencia_atestado_dados.py:4)

```python
import json
from datetime import date
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.base import ContentFile
from django.core.files.storage import Storage
from django.db import connection, transaction
from django.db.migrations.executor import MigrationExecutor
from django.template.response import TemplateResponse
from django.test import RequestFactory

from ponto.catalogo import ausencias as catalogo
from ponto.models import Ausencia
from relatorios import views


USUARIO_PK = None
EMPRESAS = (2, 3, 4)
JANELA_ESPERADA = (date(2026, 10, 21), date(2026, 11, 20))
MIGRACAO = ("ponto", "0079_ausencia_atestado_dados")


class DesfazerEnsaio(Exception):
    pass


class MidiaEmMemoria(Storage):
    def _open(self, name, mode="rb"):
        if not name or mode != "rb":
            raise FileNotFoundError("Caminho vazio ou modo de abertura inválido.")
        return ContentFile(b"sonda de atestados em lote\n", name=name)


def preparar_0079():
    campos = {
        campo.column
        for campo in Ausencia._meta.local_fields
        if campo.name.startswith("atestado_")
    }
    if not campos:
        return

    with connection.cursor() as cursor:
        cursor.execute(
            """
            SELECT column_name
            FROM information_schema.columns
            WHERE table_schema = current_schema() AND table_name = %s
            """,
            [Ausencia._meta.db_table],
        )
        presentes = {nome for (nome,) in cursor.fetchall()}

    faltam = campos - presentes
    if not faltam:
        return
    if faltam != campos:
        raise RuntimeError("As colunas da 0079 estão parcialmente presentes.")

    executor = MigrationExecutor(connection)
    migration = executor.loader.graph.nodes[MIGRACAO]
    if executor.migration_plan([MIGRACAO]) != [(migration, False)]:
        raise RuntimeError("O plano pendente precisa conter apenas a aplicação da 0079.")

    estado_anterior = executor.loader.project_state([MIGRACAO], at_end=False)
    with connection.schema_editor(atomic=False) as editor:
        migration.apply(estado_anterior, editor)


def usuario_da_view():
    usuarios = get_user_model().objects.filter(is_active=True)
    if USUARIO_PK is None:
        usuario = usuarios.filter(is_superuser=True).order_by("pk").first()
    else:
        usuario = usuarios.filter(pk=USUARIO_PK).first()
    if usuario is None:
        raise RuntimeError("Usuário ativo da contraprova indisponível.")
    return usuario


def medir_empresa(empresa, usuario):
    request = RequestFactory().get(
        "/relatorios/atestados/lote/",
        {"empresa": empresa, "mes": 11, "ano": 2026},
    )
    request.user = usuario

    with patch.object(views, "render", new=TemplateResponse):
        resposta = views.atestados_lote(request)

    if not isinstance(resposta, TemplateResponse) or resposta.status_code != 200:
        raise RuntimeError("A view não autorizou a consulta do lote.")

    contexto = resposta.context_data
    if "previa" not in contexto:
        raise RuntimeError("Empresa indisponível no filtro da view.")
    if (contexto["ini"], contexto["fim"]) != JANELA_ESPERADA:
        raise RuntimeError("A janela da empresa diverge da competência da contraprova.")

    tipos_em_aberto = set(Ausencia.TIPOS_EM_ABERTO)
    tipos_medicos = set(catalogo.TIPOS_MEDICOS)

    for linha in sorted(contexto["linhas"], key=lambda item: item["ausencia"].pk):
        a = linha["ausencia"]
        yield {
            "empresa": empresa,
            "pk": a.pk,
            "data": a.data.isoformat(),
            "data_fim": a.data_fim.isoformat() if a.data_fim is not None else None,
            "tipo": a.tipo,
            "status": a.status,
            "tem_documento": bool(linha["documento"]),
            "empresa_do_colaborador": a.colaborador.empresa_id,
            "documento_referencia_id": a.documento_referencia_id,
            "tipo_medico": a.tipo in tipos_medicos,
            "tipo_em_aberto": a.tipo in tipos_em_aberto,
            "dias_corridos": a.dias_corridos,
        }


def medir():
    if (
        settings.SETTINGS_MODULE != "config.settings.sombra"
        or connection.vendor != "postgresql"
        or connection.settings_dict["HOST"] != "sombra_db"
        or not connection.features.can_rollback_ddl
    ):
        raise RuntimeError("A medição exige PostgreSQL da sombra com DDL reversível.")

    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        if cursor.fetchone() != ("sombra", "juliani"):
            raise RuntimeError("Banco ou schema diferente do alvo da contraprova.")

    preparar_0079()
    usuario = usuario_da_view()

    campo_documento = Ausencia._meta.get_field("documento")
    with patch.object(campo_documento, "storage", new=MidiaEmMemoria()):
        return [linha for empresa in EMPRESAS for linha in medir_empresa(empresa, usuario)]


try:
    with transaction.atomic(durable=True):
        resultados = medir()
        raise DesfazerEnsaio
except DesfazerEnsaio:
    for resultado in resultados:
        print(json.dumps(resultado, sort_keys=True))
```

## 27/11 01:43 · sessao ba76a83b5eee · PAR-PELO-TURNO · construtor · origem=Code

Corrigi as duas regressões em `ponto/turnos.py`. A conferência por AST deu **`except no diff = 0`**. `ponto/turnos.py:1871` · `ponto/turnos.py:3066` · `ponto/tests/test_par_pelo_turno_bordas.py:13`

- **Col297. O teste guarda a regra correta.** A L-084 e a O73 regem pontualidade. No mapa por PK, a marca reclamada recebe o dia do turno e a marca sem turno recebe o dia local. Na base, o recorte descartava as marcas posteriores a `fim` antes dessa atribuição. A cura devolve **21/09** para as duas marcas da madrugada. O espelho filtra pelo dia atribuído, então elas ficam fora do cartão encerrado em **20/09**. `docs/LEIS.md:116` · `docs/TICKETS.md:308` · `relatorios/tests/test_pdf_sem_regra_propria.py:111` · `f822620f2:app/ponto/turnos.py:3061` · `ponto/turnos.py:3066` · `ponto/turnos.py:3077` · `ponto/services/espelho.py:588`
- **Ata.** Na base, o carregador intermediário chamava o corpo no lugar de `turnos_do_colab`, que é a chamada direta exigida pelo selo. Agora a casca carrega os insumos e chama `_turnos_do_colab_puro` diretamente. O corpo lê a ata e passa `papel_por_minuto` a `parear_turnos`. `f822620f2:app/ponto/turnos.py:1871` · `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:187` · `ponto/turnos.py:1871` · `ponto/turnos.py:1969` · `ponto/turnos.py:2023`

### RED e mutantes

Ensaios em memória, com cargas simuladas. Cada modo executou o RED do mapa e o selo existente da ata. `ponto/tests/test_par_pelo_turno_bordas.py:34` · `ponto/tests/test_par_pelo_turno_bordas.py:79`

| Modo | Saída |
|---|---|
| `base` | `Ran 2 tests`, `FAILED (failures=3)`, rc 1. Faltaram os dois PKs de 21/09 na casca e no lote, e o selo da ata falhou. `ponto/tests/test_par_pelo_turno_bordas.py:45` · `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:187` |
| `cura` | `Ran 2 tests`, `OK`, rc 0. `ponto/tests/test_par_pelo_turno_bordas.py:90` |
| `mapa_sem_borda_direita` | `FAILED (failures=2)`, rc 1. Falharam as duas asserções do mapa. `ponto/tests/test_par_pelo_turno_bordas.py:45` |
| `ata_por_chamada_indireta` | `FAILED (failures=1)`, rc 1. O selo acusou a perda da chamada direta. `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:187` |

Comandos para o Code em `app/`. O executor monta as fontes em memória. `ponto/tests/test_par_pelo_turno_bordas.py:54` · `ponto/tests/test_par_pelo_turno_bordas.py:97`

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci
python3 -m ponto.tests.test_par_pelo_turno_bordas base
python3 -m ponto.tests.test_par_pelo_turno_bordas cura
python3 -m ponto.tests.test_par_pelo_turno_bordas mapa_sem_borda_direita
python3 -m ponto.tests.test_par_pelo_turno_bordas ata_por_chamada_indireta
```

### Recorte solicitado

O runner Django parou na importação com `ModuleNotFoundError: No module named 'django.core'`. Em memória, o módulo do selo deu `Ran 8 tests / OK`. No módulo do PDF passaram os três selos e os dois casos do juiz, col297 e col788. Passaram também os quatro alvos unitários da c1v5a e o controle de paridade entre janelas, `Ran 5 tests / OK`. `manage.py:11` · `ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:63` · `relatorios/tests/test_pdf_sem_regra_propria.py:92` · `relatorios/tests/test_pdf_sem_regra_propria.py:127` · `relatorios/tests/test_pdf_sem_regra_propria.py:147` · `ponto/tests/test_par_pelo_turno.py:133` · `ponto/tests/test_par_pelo_turno.py:984`

Os dois casos integrados do cartão e o quinto alvo da c1v5a, de folha e espelho, ficam **[nao verificado]**. `relatorios/tests/test_pdf_sem_regra_propria.py:100` · `relatorios/tests/test_pdf_sem_regra_propria.py:135` · `ponto/tests/test_par_pelo_turno.py:636`

## 27/11 01:42 · sessao 46266bc6bb7d · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**RAMO2-B c2l entregue na base `7f954d921`.** A mudança de produção está na releitura v3. Em memória, A passou em 6 testes, os quatro mutantes retornaram `rc=1` por asserção, e B registrou 7 testes com 8 falhas de asserção porque B2 verifica dois motores. O diff, incluindo os arquivos novos, tem **0 `except` adicionados**. A execução usou `unittest` com dependências de Django substituídas em memória. Suíte Code e certificação pelo opus final [nao verificado]. [periodo.py:747](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:747) · [ramo2b_c2l_mutantes.py:60](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2l_mutantes.py:60) · [test_ramo2b_c2l_red_motor.py:50](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:50)

| Achado | Arquivo e linha | RED observado | GREEN de A |
|---|---|---|---|
| A1 · `DECL_60` | [periodo.py:747](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:747) · [teste:197](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:197) | Trabalhado 780, base 720, intra 40, HE50 20 | Trabalhado 780, base 720, intra 60, HE 0 | [nao verificado]
| A2 · Parcial no 12x36 | [periodo.py:798](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:798) · [teste:211](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:211) | Com parcial, atraso 30 | Com parcial, atraso 20. Sem parcial, 30 | [nao verificado]
| A3 · Partido mensal | [motor_calculo_v2.py:3216](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3216) · [teste:147](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:147) | Os controles anteriores passavam com o mutante. Os novos falham nos dois casos com intra 0 | Intra 20 para saídas às 07h30 e 07h00 | [nao verificado]
| A4 · Testemunha da porta | [porta_export.py:587](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/porta_export.py:587) · [teste:58](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/tests/test_porta_do_export.py:58) | Com a chamada sob `if False`, `medir` devolve `[]` | HE 80 gera 1 diagnóstico, com teto 0 e `falhas=1` | [nao verificado]
| B1 · Partido intocado | [teste:51](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:51) | 510 trabalhados e 0 fora. Esperado 490 trabalhados e 20 fora. `main` deu 490 e 20 | | [nao verificado]
| B2 · Pausa julgada com base curta | [teste:63](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:63) | 12x36 deu 430 trabalhados e pausa 0, esperado 410 e 20. Comercial deu 310 e 0, esperado 290 e 20 | | [nao verificado]
| B3 · Tripla parcial | [teste:87](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:87) | `AssertionError: ValueError not raised` no construtor | | [nao verificado]
| B4 · Saldo do Comercial | [teste:96](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:96) | Trabalhado 2.430, HE 0, saldo −240. Esperado −210 | | [nao verificado]
| Herdado · Limite do Comercial | [teste:116](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:116) | Grade 660, mensal paga HE50 120 e HE100 10. Esperado 0 e 0 | | [nao verificado]
| Herdado · HE noturna | [teste:129](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:129) | Dos 40 min de HE50, 10 saem noturnos. Esperado 40 | | [nao verificado]
| Herdado · Dia civil no export | [teste:146](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2l_red_motor.py:146) | 0 diagnósticos. Esperado 1 para 20/11, com teto 0 | | [nao verificado]

**Herdados no `main`** — a execução das funções de `59600163c` via `git show` devolveu os mesmos números nos três casos, respectivamente `120/10`, `10` e `0` diagnósticos. `59600163c:app/ponto/motor_calculo_v2.py:3504` · `59600163c:app/ponto/motor_calculo_v2.py:1051` · `59600163c:app/folha/porta_export.py:544`

**Escritores e leitores**

- **Base, intra e HE.** O motor grava o marco em `saida_da_base_da_he`. `reler` lê o marco e mede base e intra indenizada no mesmo trecho. O excesso lê os dois valores e `reler` escreve HE total, HE50 e HE100. `ResultadoMes` soma as faixas e a intra indenizada. [motor_calculo_v2.py:1927](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1927) · [periodo.py:752](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:752) · [periodo.py:835](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:835) · [periodo.py:863](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:863) · [motor_calculo_v2.py:352](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:352) · [motor_calculo_v2.py:391](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:391)
- **Parciais e pontualidade.** `credito_parcial` fornece o mapa por dia. O chamador de `reler` o entrega pelo argumento opcional `parciais_por_dia`. O juiz existente recebe as parciais, `reler` grava atraso e saída antecipada, e `ResultadoMes` soma ambos. [credito_parcial.py:62](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/services/credito_parcial.py:62) · [periodo.py:543](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:543) · [periodo.py:474](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:474) · [periodo.py:825](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/periodo.py:825) · [motor_calculo_v2.py:375](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:375)

**Mutantes** — comandos a partir de `app`, no ambiente Django da suíte. Todos retornaram `rc=1` em memória. `zera_intra_partido` derrubou os dois controles de A3. [ramo2b_c2l_mutantes.py:33](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2l_mutantes.py:33) [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci
python3 -m ponto.tests.ramo2b_c2l_mutantes intra_integral
python3 -m ponto.tests.ramo2b_c2l_mutantes descarta_parcial
python3 -m ponto.tests.ramo2b_c2l_mutantes zera_intra_partido
python3 -m ponto.tests.ramo2b_c2l_mutantes testemunha_morta
```

**Recortes para a sessão** — A com vizinhos e B com os REDs intencionais. [bin/suite.sh:25](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/bin/suite.sh:25) [nao verificado]

```bash
RAMO2_WT=/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946
PYTHONDONTWRITEBYTECODE=1 "$RAMO2_WT/bin/suite.sh" --dir "$RAMO2_WT" --only \ [nao verificado]
  "ponto.tests.test_ramo2b_c2h_saida ponto.tests.test_ramo2b_c2_base_da_he folha.tests.test_porta_do_export core.tests.test_medida_turno_do_par"

PYTHONDONTWRITEBYTECODE=1 "$RAMO2_WT/bin/suite.sh" --dir "$RAMO2_WT" --only \ [nao verificado]
  "ponto.tests.test_ramo2b_c2l_red_motor"
```

## 27/11 01:38 · sessao 9e97b3f580bf · O30-F3-CONSTRUIR · leitura · origem=Code

**O `--rodar` grava na sombra antes de chamar o script.** Ele repõe dois `MetricaSnapshot` e grava logs no host. O rollback do Python cobre a 0079 e a execução do relatório. Para cumprir zero escrita persistente na execução inteira, a sessão precisa chamar diretamente o mesmo container e `tenant_command`, com a saída em stdout. [`bin/sombra.sh:136`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/bin/sombra.sh:136), [`bin/sombra.sh:151`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/bin/sombra.sh:151), [`bin/sombra.sh:182`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/bin/sombra.sh:182), [`bin/sombra.sh:483`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/bin/sombra.sh:483)

1. **Chamadas reais.** A view é `relatorios.views.atestados_lote`, na linha 346 da F3 e 345 do main `59600163c`. Ambas exigem usuário autenticado com `ver_relatorios`, `ver_folha` e `ver_ausencias`. A view seleciona uma empresa visível, usa `janela()` e `linhas()` com os tipos médicos do catálogo e, por padrão, só `aprovada`. O script invoca a view real para obter as linhas e a prévia, passa competência 11/2026 e verifica as duas datas. A view usa `request.user`. O PK do usuário histórico ficou [nao verificado]. `USUARIO_PK` permite fixá-lo nas duas execuções. O padrão do script é o superusuário ativo de menor PK. [F3 `app/relatorios/views.py:20`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:20), [F3 `app/relatorios/views.py:345`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:345), [F3 `app/relatorios/views.py:382`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/relatorios/views.py:382), [main `app/relatorios/views.py:344`](/home/ronald/saas-hasner/app/relatorios/views.py:344), [main `app/relatorios/views.py:376`](/home/ronald/saas-hasner/app/relatorios/views.py:376)

   | | F3 | Main |
   |---|---|---|
   | Prévia | `previa(linhas)` — `app/ponto/services/atestados_lote.py:64`, `app/relatorios/views.py:404` | `previa(linhas)` — `app/ponto/services/atestados_lote.py:69`, `app/relatorios/views.py:395` |
   | CSV | `csv_bytes(linhas, pode_ver_anexo=True)` — `app/ponto/services/atestados_lote.py:109`, `app/relatorios/views.py:392` | `csv_bytes(linhas)` — `app/ponto/services/atestados_lote.py:107`, `app/relatorios/views.py:386` |
   | ZIP | `zip_bytes(linhas)` — `app/ponto/services/atestados_lote.py:235`, `app/relatorios/views.py:398` | `zip_bytes(linhas)` — `app/ponto/services/atestados_lote.py:135`, `app/relatorios/views.py:389` |

2. **Registro e documento.** Nas duas versões, um registro é um lançamento raiz selecionado, contado uma vez na prévia e exportado como uma linha de dados do CSV. `com_documento` conta essas linhas quando `bool(ausencia.documento)` é verdadeiro. O inteiro devolvido pelo ZIP conta entradas gravadas após a leitura do anexo. Uma falha de leitura acrescenta uma entrada `.FALTANDO.txt` sem incrementar esse inteiro. A F3 aplica `_q_cobertura` antes de montar a linha do tempo, incluindo o caso de tipos de intervalo aberto. O main monta a linha do tempo por colaborador e usa `data_fim or data` no filtro posterior. [F3 `app/ponto/services/dia_decidido.py:624`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:624), [F3 `app/ponto/services/dia_decidido.py:679`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/dia_decidido.py:679), [F3 `app/ponto/services/atestados_lote.py:35`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:35), [F3 `app/ponto/turnos.py:2842`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/turnos.py:2842), [main `app/ponto/services/atestados_lote.py:46`](/home/ronald/saas-hasner/app/ponto/services/atestados_lote.py:46), [main `app/ponto/services/dia_decidido.py:645`](/home/ronald/saas-hasner/app/ponto/services/dia_decidido.py:645), [main `app/ponto/services/dia_decidido.py:675`](/home/ronald/saas-hasner/app/ponto/services/dia_decidido.py:675)

3. **Mídia vazia.** O ZIP precisa abrir e ler o arquivo. O nome vem da matrícula, data inicial, tipo e extensão de `documento.name`. Com matrícula ausente, o ZIP usa o PK do colaborador e o CSV deixa a matrícula em branco. O script troca apenas o storage do `FileField` durante a medição. Cada caminho não vazio carregado do banco recebe bytes fixos em memória. Isso mede quantas entradas o ZIP real produz com leitura bem-sucedida e quais nomes ele emite. A disponibilidade dos arquivos originais em produção ficou [nao verificado]. [F3 `app/ponto/services/atestados_lote.py:83`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:83), [F3 `app/ponto/services/atestados_lote.py:124`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:124), [F3 `app/ponto/services/atestados_lote.py:247`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:247), [main `app/ponto/services/atestados_lote.py:88`](/home/ronald/saas-hasner/app/ponto/services/atestados_lote.py:88), [main `app/ponto/services/atestados_lote.py:147`](/home/ronald/saas-hasner/app/ponto/services/atestados_lote.py:147)

4. **0079.** Na F3, o queryset carrega instâncias completas de `Ausencia`, então o `SELECT` já precisa das seis colunas. O CSV autorizado também lê os seis atributos. Se faltarem todas as colunas, o script exige que o plano pendente seja apenas a 0079 e executa as operações dessa migration pelo `schema_editor`, sem inserir no histórico de migrations. A única captura é a sentinela fora do `atomic()`. O main segue pelo mesmo script sem aplicar a 0079. [F3 `app/ponto/services/atestados_lote.py:35`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:35), [F3 `app/ponto/services/atestados_lote.py:102`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:102), [F3 `app/ponto/models.py:787`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/models.py:787), [F3 `app/ponto/migrations/0079_ausencia_atestado_dados.py:8`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/migrations/0079_ausencia_atestado_dados.py:8)

5. **Saída.** O casamento usa apenas as linhas `tem_documento=sim` do CSV e remove só a extensão final do nome do ZIP. Uma chave ambígua ou um sufixo de colisão aparece na conferência. O SHA-256 usa todas as chaves do CSV em ordem lexicográfica, incluindo repetições. `contagens_prod_conferem` compara com os números informados. A execução do script ficou [nao verificado]. [F3 `app/ponto/services/atestados_lote.py:121`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:121), [F3 `app/ponto/services/atestados_lote.py:251`](/home/ronald/codex_ws/ac4ad27cea9defd9ab744d97dc6db899cd6835b6/app/ponto/services/atestados_lote.py:251), [`logs/O30-F3-CONSTRUIR/smoke_sombra/q.txt:10`](/home/ronald/saas-hasner/logs/O30-F3-CONSTRUIR/smoke_sombra/q.txt:10)

```python
import csv
import hashlib
import json
import warnings
from collections import Counter
from datetime import date, datetime
from inspect import signature
from io import BytesIO, StringIO
from os.path import splitext
from unittest.mock import patch
from zipfile import ZipFile

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.base import ContentFile
from django.core.files.storage import Storage
from django.db import connection, transaction
from django.db.migrations.executor import MigrationExecutor
from django.template.response import TemplateResponse
from django.test import RequestFactory

from ponto.models import Ausencia
from ponto.services import atestados_lote as lote
from relatorios import views


USUARIO_PK = None
ESPERADO_PROD = {2: (119, 116), 3: (7, 7), 4: (4, 4)}
JANELA_ESPERADA = (date(2026, 10, 21), date(2026, 11, 20))
MIGRACAO = ("ponto", "0079_ausencia_atestado_dados")

CSV_COM_PERMISSAO = "pode_ver_anexo" in signature(lote.csv_bytes).parameters


class DesfazerEnsaio(Exception):
    pass


class MidiaEmMemoria(Storage):
    def _open(self, name, mode="rb"):
        if not name or mode != "rb":
            raise FileNotFoundError("Caminho vazio ou modo de abertura inválido.")
        return ContentFile(b"sonda de atestados em lote\n", name=name)


def preparar_0079():
    campos = {
        campo.column
        for campo in Ausencia._meta.local_fields
        if campo.name.startswith("atestado_")
    }
    if not campos:
        return

    with connection.cursor() as cursor:
        cursor.execute(
            """
            SELECT column_name
            FROM information_schema.columns
            WHERE table_schema = current_schema() AND table_name = %s
            """,
            [Ausencia._meta.db_table],
        )
        presentes = {nome for (nome,) in cursor.fetchall()}

    faltam = campos - presentes
    if not faltam:
        return
    if faltam != campos:
        raise RuntimeError("As colunas da 0079 estão parcialmente presentes.")

    executor = MigrationExecutor(connection)
    migration = executor.loader.graph.nodes[MIGRACAO]
    if executor.migration_plan([MIGRACAO]) != [(migration, False)]:
        raise RuntimeError("O plano pendente precisa conter apenas a aplicação da 0079.")

    estado_anterior = executor.loader.project_state([MIGRACAO], at_end=False)
    with connection.schema_editor(atomic=False) as editor:
        migration.apply(estado_anterior, editor)


def usuario_da_view():
    usuarios = get_user_model().objects.filter(is_active=True)
    if USUARIO_PK is None:
        usuario = usuarios.filter(is_superuser=True).order_by("pk").first()
    else:
        usuario = usuarios.filter(pk=USUARIO_PK).first()
    if usuario is None:
        raise RuntimeError("Usuário ativo da contraprova indisponível.")
    return usuario


def contar_repeticoes(contador):
    return sum(quantidade - 1 for quantidade in contador.values())


def medir_empresa(empresa, usuario):
    request = RequestFactory().get(
        "/relatorios/atestados/lote/",
        {"empresa": empresa, "mes": 11, "ano": 2026},
    )
    request.user = usuario

    # Captura o contexto da view real sem renderizar o HTML.
    with patch.object(views, "render", new=TemplateResponse):
        resposta = views.atestados_lote(request)

    if not isinstance(resposta, TemplateResponse) or resposta.status_code != 200:
        raise RuntimeError("A view não autorizou a consulta do lote.")

    contexto = resposta.context_data
    if "previa" not in contexto:
        raise RuntimeError("Empresa indisponível no filtro da view.")
    if (contexto["ini"], contexto["fim"]) != JANELA_ESPERADA:
        raise RuntimeError("A janela da empresa diverge da competência da contraprova.")

    linhas = contexto["linhas"]
    previa = contexto["previa"]
    if CSV_COM_PERMISSAO:
        csv_real = lote.csv_bytes(linhas, pode_ver_anexo=True)
    else:
        csv_real = lote.csv_bytes(linhas)

    # O aviso da biblioteca inclui o nome. A duplicata é contada abaixo.
    with warnings.catch_warnings():
        warnings.filterwarnings("ignore", message=r"^Duplicate name:", category=UserWarning)
        zip_real, documentos_zip = lote.zip_bytes(linhas)

    leitor = csv.DictReader(StringIO(csv_real.decode("utf-8-sig"), newline=""), delimiter=";")
    chaves_csv = []
    chaves_docs_csv = Counter()
    chaves_vazias_csv = 0
    chaves_vazias_docs_csv = 0

    for linha in leitor:
        matricula, data_csv, tipo = linha["matricula"], linha["data"], linha["tipo"]
        data_iso = (
            datetime.strptime(data_csv, "%d/%m/%Y").date().isoformat()
            if data_csv.strip() else ""
        )
        chave = f"{matricula}_{data_iso}_{tipo}"
        vazia = not all(parte.strip() for parte in (matricula, data_iso, tipo))
        chaves_csv.append(chave)
        chaves_vazias_csv += vazia

        if linha["tem_documento"] == "sim":
            chaves_docs_csv[chave] += 1
            chaves_vazias_docs_csv += vazia

    with ZipFile(BytesIO(zip_real)) as arquivo_zip:
        nomes = arquivo_zip.namelist()

    nomes_vazios = sum(not nome.strip() for nome in nomes)
    nomes_duplicados = contar_repeticoes(Counter(nomes))
    chaves_zip = Counter(splitext(nome)[0] for nome in nomes)
    chaves_duplicadas_zip = contar_repeticoes(chaves_zip)
    chaves_duplicadas_docs_csv = contar_repeticoes(chaves_docs_csv)
    marcadores_faltando = sum(nome.endswith(".FALTANDO.txt") for nome in nomes)
    documentos_csv = sum(chaves_docs_csv.values())

    registros_conferem = previa["total"] == len(chaves_csv)
    documentos_conferem = (
        previa["com_documento"] == documentos_csv == documentos_zip == len(nomes)
        and marcadores_faltando == 0
    )
    zip_csv_1_a_1 = (
        registros_conferem
        and documentos_conferem
        and chaves_docs_csv == chaves_zip
        and chaves_vazias_docs_csv == chaves_duplicadas_docs_csv == 0
        and nomes_vazios == nomes_duplicados == chaves_duplicadas_zip == 0
    )
    chaves_serializadas = json.dumps(
        sorted(chaves_csv), ensure_ascii=False, separators=(",", ":")
    ).encode("utf-8")

    return {
        "empresa": empresa,
        "registros_previa": previa["total"],
        "registros_csv": len(chaves_csv),
        "documentos_previa": previa["com_documento"],
        "documentos_csv": documentos_csv,
        "documentos_zip": documentos_zip,
        "entradas_zip": len(nomes),
        "marcadores_faltando_zip": marcadores_faltando,
        "nomes_vazios_zip": nomes_vazios,
        "nomes_duplicados_zip": nomes_duplicados,
        "chaves_duplicadas_zip": chaves_duplicadas_zip,
        "chaves_vazias_csv": chaves_vazias_csv,
        "chaves_duplicadas_csv": contar_repeticoes(Counter(chaves_csv)),
        "chaves_vazias_csv_com_documento": chaves_vazias_docs_csv,
        "chaves_duplicadas_csv_com_documento": chaves_duplicadas_docs_csv,
        "registros_previa_csv_conferem": registros_conferem,
        "documentos_previa_csv_zip_conferem": documentos_conferem,
        "zip_csv_1_a_1": zip_csv_1_a_1,
        "contagens_prod_conferem": (
            registros_conferem
            and documentos_conferem
            and (previa["total"], documentos_zip) == ESPERADO_PROD[empresa]
        ),
        "sha256_chaves_csv": hashlib.sha256(chaves_serializadas).hexdigest(),
    }


def medir():
    if (
        settings.SETTINGS_MODULE != "config.settings.sombra"
        or connection.vendor != "postgresql"
        or connection.settings_dict["HOST"] != "sombra_db"
        or not connection.features.can_rollback_ddl
    ):
        raise RuntimeError("A medição exige PostgreSQL da sombra com DDL reversível.")

    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        if cursor.fetchone() != ("sombra", "juliani"):
            raise RuntimeError("Banco ou schema diferente do alvo da contraprova.")

    preparar_0079()
    usuario = usuario_da_view()

    campo_documento = Ausencia._meta.get_field("documento")
    with patch.object(campo_documento, "storage", new=MidiaEmMemoria()):
        return [medir_empresa(empresa, usuario) for empresa in ESPERADO_PROD]


try:
    with transaction.atomic(durable=True):
        resultados = medir()
        raise DesfazerEnsaio
except DesfazerEnsaio:
    for resultado in resultados:
        print(json.dumps(resultado, sort_keys=True))
```

## 27/11 01:31 · sessao d67b8bc0162f · E4C-F5-INSTANTE · construtor · origem=Code

Escrevi 12 testes em [test_e4c_f5_c4_red.py](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1). Cada caso começa pela regra, pelo cadastro e pelo resultado esperado. A alteração ficou no arquivo novo, com zero `except` novos. [nao verificado]

A execução do módulo parou no import, antes das fixtures. O Python local devolveu `ModuleNotFoundError: No module named 'django.contrib'`, rc 1. A suíte da cópia usa PostgreSQL. Em memória, executei dois métodos do arquivo novo e oito recortes das funções reais da cópia. A saída foi `Ran 10 tests`, `FAILED (failures=10)`, rc 1, todas por `AssertionError`. As três provas de integração indicadas na tabela seguem **[nao verificado]**. `app/chamados/tests/test_e4c_f5_c4_red.py:9`, `app/config/settings/ci.py:6`

| Achado | Teste em `test_e4c_f5_c4_red.py` | Saída RED | Cura esperada | [nao verificado]
|---|---:|---|---|
| Carimbo aceita validação vigente em T2 com coluna em T1 | `:36` | `(True, None) != (False, 'par_ato_coluna')` | `chamados/models.py::DecisaoPergunta.par_ato_coluna` e `PerguntaDisputa.carimbar` devem conferir o instante vigente. `models.py:3499`, `models.py:4646` |
| Absorção aceita o mesmo par divergente | `:93` | **[nao verificado]**. O teste exige recusa por PK, trilha e ausência de `ABSORCAO`. | `chamados/services/materializacao.py::_materializar_ausencia_de_resposta` deve usar a mesma comparação por instante. `materializacao.py:1108`, `materializacao.py:1115` |
| Falha ao ler a foto vaza o carimbo em memória | `:59` | Instante, autor `7` e via `juiz_aprovou` ficaram na instância. | `chamados/models.py::PerguntaDisputa.carimbar` deve ler a foto antes de alterar a instância ou incluir essa leitura na proteção existente. `models.py:3465`, `models.py:3473`, `models.py:3490` |
| `ato_em` pendente escapa ao horizonte da tarefa | `:117` | O horizonte de 12h viu a validação pendente de 13h. | `chamados/models.py::DecisaoPergunta.leitura` e `chamados/services/tarefa_do_dia.py::_fatos_das_perguntas` devem limitar o candidato ao contexto do carimbo e respeitar o horizonte da tarefa. `models.py:4549`, `tarefa_do_dia.py:416` |
| Coluna retrodatada em disputa nova conta como estoque | `:160` | `[('b', 'respondida', False)] != [('b', 'respondida', True)]` | `chamados/models.py::DecisaoPergunta.divergencia_ato_x_coluna` deve usar a abertura posterior da disputa como prova de que a pergunta nasceu depois do marco. `models.py:1921`, `models.py:4615` |
| Porta aceita estoque no marco e depois | `:173` | `ValueError not raised` nos dois instantes. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve admitir somente o campo classificado como `b` de estoque. `models.py:4615`, `models.py:4800` |
| Porta aceita foto de campo `a` com ato vigente | `:183` | `ValueError not raised`. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve recusar a foto e preservar a autoridade do ato vigente para a reprojeção. `models.py:4611`, `models.py:4801` |
| Porta aceita dia civil de conversa como dia do fato | `:196` | `ValueError not raised`. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve excluir conversa sem competência de fato e consultar o K6 existente para as elegíveis. `models.py:3191`, `models.py:4813`, `backfill_decisoes_pergunta.py:74` |
| Payload de estoque atribui a foto ao carimbo | `:205` | `ValueError not raised`. | `chamados/models.py::DecisaoPergunta._conferir_estoque` deve recusar o motivo adulterado. `models.py:4469`, `models.py:4781`, `models.py:4785` |
| Foto histórica ganha origem de ato novo | `:215` | `'ato' != 'estoque'` no payload com o default do modelo. | `chamados/models.py::PerguntaDisputa.carimbar` e `DecisaoPergunta.registrar` devem preservar a proveniência da foto e a entrada própria da validação nova. `models.py:3473`, `models.py:3526`, `models.py:4392` |
| Entrega pode intercalar a leitura do K6 e o `INSERT` | `:234` | **[nao verificado]**. A prova usa duas conexões e a trava real de entrega. | A entrada do estoque em `chamados/models.py::DecisaoPergunta.registrar` deve serializar com `folha/export.py::travar_entrega`, respeitando Empresa antes de Colaborador. `models.py:4737`, `models.py:4742`, `folha/export.py:556` |
| Item 13 por PK com entrega real pelo K6 | `:298` | **[nao verificado]**. O teste verifica as três portas, atos, colunas, trilha e hash do mês do fato. | `chamados/services/validacao.py::validar_pergunta` e `chamados/services/materializacao.py::_materializar_uma` devem registrar o bloqueio com autor e PK. A recusa serve de controle com a trilha já emitida pela porta. `validacao.py:792`, `materializacao.py:1981`, `validacao.py:1017` |

**Achados sem RED adicional**

- Regravação retrodatada ou limpeza em pergunta já existente no marco pode produzir a mesma foto persistida que um passivo legítimo. O teste usa a abertura da disputa como evidência apenas no caso em que ela resolve a ambiguidade. As fixtures antigas que pretendem simular estoque também precisam representar uma disputa anterior ao marco se a cura usar essa evidência. `app/chamados/models.py:4600`, `app/chamados/tests/test_e4c_f5_instante_c1.py:83`
- A 0058 já recorta `entrada_em <= marco`, com teste na função real. Ordenação dos relógios no deploy e recuperação de timestamps sobrescritos seguem **[nao verificado]**. `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16`, `app/chamados/tests/test_e4c_f5_instante_c1.py:370`
- O censo de `RESPOSTA`, a devolução L-130, a colisão Q0.2 e a saída DRY já têm casos próprios. A contagem e a lista de PKs reais, o efeito monetário e o DIFF de frota seguem **[nao verificado]**. `app/chamados/tests/test_f2_resposta_mantem_validacao.py:20`, `app/chamados/tests/test_e4c_f5_instante_c1.py:234`, `app/chamados/tests/test_e4c_f5_instante_c1.py:340`, `app/chamados/tests/test_e4c_f5_instante_c1.py:142`, `app/docs/LEIS.md:155`
- O relógio da passagem da ausência tem um contrato próprio de horizonte, e o `max` dos atos compensadores está previsto no contrato V2. A janela de candidato foi testada. Uma mudança na data de `ABSORCAO`, o contrato geral de ato futuro e a escolha do escritor semântico da validação automática requerem conciliar os contratos citados. `app/chamados/services/materializacao.py:1047`, `app/chamados/services/materializacao.py:1100`, `app/chamados/models.py:4691`, `app/docs/BACKLOG.md:2171`, `app/docs/JUIZES.md:38`, `app/chamados/models.py:3484`
- Vigia e DRY duplicam a consolidação por PK, com a mesma regra observável no código atual. Extrair a consolidação por si só não dá um RED de comportamento. `app/chamados/services/vigia_ato_x_coluna.py:24`, `app/chamados/management/commands/estoque_ato_x_coluna.py:44`

**Comando para a sessão**

```bash
bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-instante-012038 \
  --only "chamados.tests.test_e4c_f5_c4_red"
```

Para os mutantes, após o verde, reintroduzir o comportamento da primeira coluna uma vez por caso e rodar o seletor correspondente. A aplicação e a execução dos mutantes seguem **[nao verificado]**. A certificação fica com o Code e o opus final. `app/docs/LEIS.md:147`

```bash
red() {
  bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-instante-012038 \
    --only "chamados.tests.test_e4c_f5_c4_red.$1"
}

red PortaDoCarimboTest.test_RED_carimbo_recusa_validacao_vigente_em_outro_instante
red AbsorcaoDoParTest.test_RED_absorcao_preserva_irma_com_validacao_em_instante_divergente
red PortaDoCarimboTest.test_RED_falha_da_foto_preserva_o_carimbo_em_memoria
red HorizonteDoCarimboTest.test_RED_candidato_futuro_nao_existe_no_horizonte_da_tarefa
red EstoqueDoParTest.test_RED_coluna_retrodatada_em_disputa_nova_conta_como_b_novo
red EstoqueDoParTest.test_RED_porta_estoque_recusa_instante_no_marco_ou_depois
red EstoqueDoParTest.test_RED_porta_estoque_recusa_a_com_ato_vigente
red EstoqueDoParTest.test_RED_porta_estoque_recusa_dia_civil_de_conversa
red EstoqueDoParTest.test_RED_porta_estoque_nao_grava_motivo_de_ato_do_carimbo
red EstoqueDoParTest.test_RED_foto_anterior_carimbada_hoje_conserva_origem_historica
red EstoqueConcorrenteComEntregaTest.test_RED_entrega_nao_intercala_k6_e_insert_do_estoque
red Item13PorPKTest.test_RED_item13_portas_publicas_em_entrega_vigente_com_trilha_por_pk
```

## 27/11 01:29 · sessao de4cf674062e · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

**REPROVADO.** A releitura v3 paga **20 min de HE50 indevida** no caso da pausa na cauda. O diff também altera o motor do Partido, que o aval (a) mandou manter intocado. A catraca c2k reproduziu as três verificações em memória. [ponto/calculador/periodo.py:745](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/ponto/calculador/periodo.py:745) · [app/docs/PROMPTS.md:3087](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/docs/PROMPTS.md:3087) · [core/tests/test_medida_turno_do_par.py:548](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/core/tests/test_medida_turno_do_par.py:548)

### Parcial e cauda

Valores em minutos. Nos três casos, a janela está ligada, com piso 0, teto 60, cauda pendente e dia comum. A parcial abate 30 do atraso bruto de 50 **antes** do teto. As duas pontas distam 50 e 20 dos marcos, de modo que a L-084 mantém o julgamento. A base vai até o marco, o trabalhado inclui a cauda e o teto é 30. `app/docs/LEIS.md:79–80,111,115–116` · `app/docs/PROMPTS.md:3031` · `app/ponto/motor_calculo_v2.py:1242–1307` · `app/ponto/tests/test_ramo2b_c2h_saida.py:133–161`

| Achado | Arquivo:linha | Contraexemplo e números | Procede? |
|---|---|---|---|
| **12x36 — o caso da c2j cobre só `do_dia`** | `app/ponto/tests/test_ramo2b_c2h_saida.py:133–161` · `app/ponto/calculador/regras.py:614–626,785–808,882–886` | Cadastro 19h–07h, previsto 660, pausa 00h–01h. Batidas E19h50, S00h, E01h, S07h20. Parcial 19h20–19h50. **Regra e `do_dia` executado em memória:** trabalhado 630, base 610, intra 0, atraso 20, antecipada 0, HE 0. Sem parcial, atraso 30. O teste injeta a base, a pausa e a tupla de pontualidade prontas. | **Não procede** como erro de `do_dia` nesse caso. **Procede** como lacuna do teste ponta a ponta. |
| **Comercial — o mensal troca o previsto da célula por 480** | `app/escala/utils.py:2284,2322–2328` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:62–66` · `app/ponto/motor_calculo_v2.py:3430,3555–3568,3617–3653` | Mesmo caso, em tipo Comercial diurno com célula congelada 19h–07h e pausa 00h–01h. **Regra e `do_dia`:** trabalhado 630, base 610, intra 0, atraso 20, HE 0. **Mensal:** o limite fica em 480 porque a célula cruza. `610 − 480 = 130`, sendo **120 HE50 + 10 HE100**. Executei o override com o período e o dia fornecidos. | **Procede contra L-024**, `app/docs/LEIS.md:75`. O limite fixo é herdado. A grade injeta 660 e deixa o mensal fora da prova, `app/ponto/tests/test_ramo2b_c2h_saida.py:62–77,91–113`. |
| **Partido — mudança de dinheiro fora do aval (a)** | `app/docs/PROMPTS.md:3087` · `app/ponto/motor_calculo_v2.py:1888–1905,3103–3118,3164–3167,3216–3227,3319–3326,3353–3365` | Partido real 19h–07h, vão 00h–03h, previsto 540, régua relógio. E19h50, S00h, E03h, S07h20. Parcial 19h20–19h50. **L-097(b) e caminho atual:** trabalhado 510, base 490, intra 0, atraso 20, HE 0. **Com o Partido preservado como no lado antigo do diff:** trabalhado 490, fora 20, atraso 20, HE 0. O diff também troca a base usada no cálculo mensal da intra, dobra e ganho noturno. | **Procede contra o escopo do aval (a).** Os 20 a mais de trabalhado correspondem à L-097(b). O bloqueio é aplicar a mudança ao Partido nesta entrega. |

### Achados no diff e nos leitores

| Achado | Arquivo:linha | Contraexemplo e números | Procede? |
|---|---|---|---|
| **A releitura v3 calcula HE com dois recortes incompatíveis** | `app/ponto/calculador/periodo.py:745–759,832–836,862–864` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:301–338` | Caso `DECL_60`: 19h–07h, previsto 660, pausa cadastrada 00h–01h, E19h, pausa interna julgada 07h20–07h40, S08h20. **Regra:** trabalhado 780, base 720, intra 60, HE 0. **`reler` executado:** 780, base 720, intra **40**, HE50 **20**. A segunda leitura fica só com o trabalhado recortado. A intra gravada usa a pausa do par integral e baixa o limite de 720 para 700. | **Procede contra L-031, L-032 e L-097(b)** e contra a resposta específica, `app/docs/PROMPTS.md:3071`. Regressão no caminho de ensaio, `app/ponto/calculador/periodo.py:543–548`. Efeito no gravado [nao verificado]. |
| **Pausa julgada vira trabalhado quando a base é curta** | `app/ponto/motor_calculo_v2.py:2968–2999,3449–3485` · `app/ponto/calculador/alimentacao.py:133–147` | 12x36 19h–07h, E01h10, S08h20, pausa 07h20–07h40 entregue dentro do par e dia atribuído a D. Base 350, bruto 430. **Regra:** trabalhado 410, pausa 20, HE 0. **Motor:** 430, pausa descontada 0, HE 0. No Comercial, E03h10 com o mesmo fim e a mesma pausa dá base 230 e **310 em vez de 290** trabalhados. O desconto do trabalhado integral ficou dentro do gatilho da duração **da base**. | **Procede contra L-032 e L-097(b)** na porta `calcular_periodo`, executada com a pausa julgada. Formação automática desses pares pela sequência de batidas [nao verificado]. |
| **A releitura v3 perde a parcial** | `app/ponto/calculador/periodo.py:434–435,464–477,797–825` · `app/ponto/calculador/alimentacao.py:535–566` | No 12x36 da primeira tabela, o leitor de ensaio chama o juiz de pontualidade sem a parcial. Calcula `min(50, 660 − 630) = 30`. A regra e `do_dia` dão **20**. | **Procede contra o corte do abono**, `app/docs/PROMPTS.md:3031`. O teto L-093 é respeitado. O repasse faltava no caminho de ensaio anterior ao diff. |
| **HE noturna no motor ainda termina na batida crua** | `app/ponto/motor_calculo_v2.py:458–479,1084–1094,3052–3072` · `app/ponto/calculador/alimentacao.py:106–115` · `app/ponto/calculador/regras.py:850–865` | 12x36 16h–04h, extra declarada de 60 desloca o corte para 05h, sem prorrogação pós-05h. E16h20, S05h30, previsto 720. Base 760, trabalhado 790, HE50 40. **Na base:** 04h20–05h, **40 noturnos**. **Na ponta crua do motor:** 04h50–05h30, **10 noturnos**. A mesma função, executada com as duas pontas, devolveu 40 e 10. | **Procede contra L-035 e L-097(b)**, `app/docs/LEIS.md:83,115`. O motor já lia a batida crua. O oráculo mantém o fim recortado. |
| **Export mede o dia civil, mesmo com o dia do turno disponível** | `app/folha/porta_export.py:340–362,584–591` · `app/ponto/services/dia_pago.py:109–132,174` | Competência termina em 20/11. Turno de 20/11, 23h50–07h50, pausa prevista 02h–03h, previsto 420. E00h02, S08h20 em 21/11, pausa sem gozo. Base 468, trabalhado 498, intra 60. Injetando HE indevida de 30 na testemunha, o teto da jornada é **0** e deveria sair **1** diagnóstico. A função retorna **0** porque descarta a entrada de 21/11. | **Procede contra L-085 e L-002**, `app/docs/LEIS.md:60,117`. O agrupamento civil é herdado. A extração o manteve. |
| **A troca da função muda a medição do export** | `app/folha/porta_export.py:348–362,587–591,713–723` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:717–746` | No par que a testemunha consegue agrupar, base 720, trabalhado 800, previsto 720, intra 60 e declarado 0. Para uma HE indevida de 80, o teto antigo era **80**, com **0** diagnósticos. O teto novo é **0**, com **1** diagnóstico. A entrada da testemunha continua vindo de `esp['resultado'].periodos`. Com intra 0, a mesma HE passa por essa testemunha, que só mede HE junto com intra. A pendência tem outra entrada no portão. | **Procede** a mudança do valor lido. **Não procede** atribuí-la à troca de identidade do registro AST. O alcance da prova é a testemunha HE × intra, `app/folha/porta_export.py:354–355,548,706–723`. |
| **`trab_por_dia` do Comercial virou base de HE e continua alimentando saldo** | `app/ponto/motor_calculo_v2.py:3558–3560,3655–3685` | Semana de cinco dias com célula 22h–06h, base 480 por dia e uma cauda pendente de 30. Soma trabalhada 2.430, HE 0. Pela fórmula do saldo sobre o trabalhado, `2.430 − 2.640 = −210`. Pelo mapa da base, o código calcula **−240**. | **Procede** como campo com dois significados, à luz da separação da L-097(b), `app/docs/LEIS.md:115`. O saldo já excluía esses 30 quando o trabalhado antigo vinha recortado. |
| **A tripla aceita escrita fora do tripwire** | `app/ponto/motor_calculo_v2.py:207–211,244–246,280–285,1927–1943` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:627–638,705–714` | Par 19h–08h20, trabalhado 800. O construtor gerado aceita `minutos_base_da_he=720` sozinho e produz `(720, None, None)`. O leitor numérico vê **720**. O leitor geométrico, sem fim da base, conserva o par de **800**. A porta rejeita a tripla parcial, o construtor a aceita. | **Procede contra L-007**, `app/docs/LEIS.md:65`. Chamador de produção que passe a tripla ao construtor [nao verificado]. |
| **Os dois controles do Partido deixam o cálculo que deveriam vigiar fora do teste** | `app/ponto/tests/test_ramo2b_c2h_saida.py:62–77,123–131` · `app/escala/servico_jornada.py:130–164` · `app/ponto/motor_calculo_v2.py:3184–3227` | Os controles forçam a classe com vão de 60 e chamam `calcular_periodo`. Num Partido real 19h–07h com vão cadastrado 00h–03h, batidas 19h, 00h20, 03h, 07h30, o mensal deve indenizar **20** pelo vão de 160. Um mutante que zere esses 20 deixa os dois controles intactos. | **Procede como selo sem mordida sobre o mensal**, `CLAUDE.md:28–29`. Há outros testes do caminho real do Partido, `app/ponto/tests/test_ramo2b_turno_que_cruza.py:151–164,216–225`. |

### Quem escreve e quem lê

| Campo ou valor | Escrita e leitura na cópia | Semântica que importa |
|---|---|---|
| `PeriodoCalculo.minutos_base_da_he` | 12x36, Partido e Comercial medem por par e passam a `_gravar_base_da_he`, que chama `_escrever_base_da_he`. O bloqueio de HE100 limpa e o crédito R3 agrega pela mesma porta. `_trabalhado_da_base_da_he` serve ao R3, às bases mensais do Partido e do Comercial, e à testemunha do export. `app/ponto/motor_calculo_v2.py:280–285,1612–1620,1927–1956,2747–2765,2998–2999,3113–3116,3167,3216,3320–3325,3484–3485,3560,3585–3589` · `app/folha/porta_export.py:337–351` | Minutos de relógio por par. `None` lê `minutos_trabalhados`. No R3, a escrita passa a representar a **soma de pares fechados** num portador aberto. `app/ponto/motor_calculo_v2.py:2755–2765` |
| `saida_da_base_da_he` | A janela produz o instante, `_janela_do_par` o limita ao par efetivo e a porta grava. Leem o R3, o split e a segmentação da alimentação, e a releitura v3. `app/ponto/motor_calculo_v2.py:1893–1924,1942,2751–2752` · `app/ponto/calculador/alimentacao.py:114–115,168–179` · `app/ponto/calculador/periodo.py:749–756` | Instante de fim da base, já deslocado pela extra declarada. Par todo na cauda usa a entrada como limite e tem base 0. No R3, o instante vem do último par creditado com cauda. Alcance real desse caso R3 [nao verificado]. `app/ponto/janela_he.py:192–200` · `app/ponto/motor_calculo_v2.py:1921–1923,2758–2765` |
| `ganho_noturno_da_base_da_he` | `_gravar_base_da_he` calcula legais menos relógio até o fim da base. A porta grava. `_ganho_noturno_da_base_da_he` alimenta a régua `legais` do Partido. R3 fornece zero. `app/ponto/motor_calculo_v2.py:287–293,1943–1956,2762–2765,3354–3362` | Minutos de ganho da hora reduzida. `None` usa legais menos relógio do par. Zero explícito no R3 é o valor do portador aberto. Mesmas referências. |
| Argumentos `minutos_base_da_he` e `pausa_base_da_he_min` de `do_dia` | `segmentos_do_motor` recorta pelo instante, `minutos_do_oraculo` soma segmentos e vãos, e `rubricas_por_dia` passa ambos por dia da jornada. `do_dia` usa a dupla no piso, na intra e na HE/dobra. Fechamento e `diff_calculador` consomem a saída. `app/ponto/calculador/alimentacao.py:78–88,165–179,469–474,545–548` · `app/ponto/calculador/nucleo.py:133–154` · `app/ponto/calculador/regras.py:614–626,772–808` · `app/ponto/services/fechamento.py:1291–1319` · `app/ponto/management/commands/diff_calculador.py:414–424` | Minutos **do dia**, derivados de novo a partir do instante. O núcleo mede todo vão entre segmentos do mesmo dia. Uma pausa que começa no marco dá 0 à base. Uma que termina no marco inclui o tempo anterior. A sentinela de duração zero conserva o fim do vão. A terceira derivação, em `reler`, é a que diverge em **40/20 contra 60/0**. `app/ponto/calculador/alimentacao.py:173–179` · `app/ponto/calculador/periodo.py:745–759,834–836` |
| `parciais` de `do_dia` | O DP grava as pontas pela porta de ausência. `credito_parcial` seleciona ausências pagas com período. A alimentação passa a lista do dia a `do_dia`, e o contexto a entrega ao teto do motor. O motor também carrega a lista pela mesma fonte. `app/ponto/services/ausencia.py:1039–1058,1099–1104` · `app/ponto/services/credito_parcial.py:15–32,54–70` · `app/ponto/calculador/alimentacao.py:535–566` · `app/ponto/calculador/regras.py:117–127,204–205,280–282,882–886` · `app/ponto/motor_calculo_v2.py:1326–1343` | Lista de ausências, com leitura das horas declaradas. `minutos` participa da classificação como parcial. O abate mede a cobertura da ponta em fatias `[k,k+1)` desde 00h do dia da jornada. A virada vale 1.440+. O juiz do **marco previsto** inclui as duas bordas. O juiz da **batida** usa a direção, com S no início e E no fim fora. `app/ponto/turnos.py:2622–2639,2653–2711` · `app/ponto/motor_calculo_v2.py:481–505,1242–1275` |

### Catraca, travas e alcance

| Achado | Arquivo:linha | Contraprova | Procede? |
|---|---|---|---|
| **C2k — censo real** | `app/core/tests/test_medida_turno_do_par.py:194,250–260,288,304–313,348–350,425,548–577` · `app/core/medida_turno_do_par.py:426–444` | Rodei os três testes da c2k com `unittest` em memória. **3 passaram.** Censo de 782 arquivos: A1/A2 **6/15**, B1/B2 **78/192**, totais **9** pareadores e **81** derivações. Os sítios nomeados são **558, 937 e 2601**. O B1 da folha acompanha `he_acima_do_teto_por_dia:344`. O `.date()` que fica em `medir:391` deriva o dia do relógio capturado. `CLASSIFICAR` recebe o resultado de `B2 − B1` e o teste exige igualdade. | **Não procede** lista de exceção nova ou critério amplo no contador. A atribuição de cada mudança ao commit individual [nao verificado]. A cópia entregue não tem metadados Git [nao verificado]. |
| **`except`, fallback e asserções** | `app/ponto/motor_calculo_v2.py:280–293,1935–1943` · `app/ponto/calculador/alimentacao.py:472–474,547–548` · `app/folha/tests/test_porta_do_export.py:122–145` · `app/ponto/tests/test_contract_juiz_batida.py:382–412` | **0 `except` novos** e nenhum teste inteiro apagado nos hunks anexados. Há `raise ValueError` para dados incompletos na porta. Há fallback de ausência dos novos campos para o par integral e de chave ausente no mapa para zero. A asserção do export passou a procurar texto em duas funções. Uma chamada morta sob `if False` satisfaz o `assertIn`, e o RED de 80 chama só o helper. O AST da impressão do motor exige uma ocorrência em cada classe, com a limitação a `max` literal declarada no próprio teste. | **Procede** a lacuna de integração da testemunha. O fallback da tripla tem o contraexemplo **720/800** acima. Execução da suíte Django completa [nao verificado]. |
| **Relógio, ordem de trava e pré-requisito** | `app/folha/porta_export.py:366–391,529,549,693,1082–1083,1181–1188` · `app/folha/export.py:555–562` · `app/docs/PROMPTS.md:3065,3105` · `CLAUDE.md:421–430` | `medir` captura um instante e o repassa ao espelho, PDF e comando. Os hunks da testemunha operam sobre períodos, datas e cadastros já alimentados. A ordem escrita exige ABONO antes da RAMO2 e certificação antes do pouso. | **Não procede** leitura nova do relógio ou inversão de trava nos hunks exibidos. Pouso efetivo, certificação e ordem real dos pré-requisitos [nao verificado]. |
| **Competência exportada** | `app/ponto/services/fechamento.py:36–50,559–595,1215–1216,1822–1848` · `app/folha/views.py:214–216` · `app/docs/LEIS.md:155–156` | O evento pode atingir o escritor genérico com o motor alterado. A guarda explícita cobre 09/2026 nas empresas que a exportaram. O TXT também tem caminho de recálculo em leitura. Assim, o diff demonstra alcance potencial fora da guarda, inclusive para um caso cujo trabalhado passe de 490 a 510. | **Procede como alcance estático.** Competência exportada efetivamente tocada, antes/depois e hash do gravado [nao verificado]. |

## 27/11 01:29 · sessao fb00a5f2b756 · - · - · origem=Ronald (tmux)

**REPROVADO.** A releitura v3 dá 40 min de intra e 20 min de HE50 onde a regra dá 60 de intra e zero de HE, e o split noturno do motor usa a saída crua em vez do fim da base. `app/docs/LEIS.md:79–80,83,115`, `app/ponto/calculador/periodo.py:745–759,834–836`, `app/ponto/motor_calculo_v2.py:1084–1094`.

| Achado | Arquivo:linha | Contraexemplo | Procedência |
|---|---|---|---|
| A v3 usa a pausa da cauda para reduzir intra e criar HE | `app/ponto/calculador/periodo.py:745–759,834–836`, `app/ponto/tests/test_ramo2b_c2_base_da_he.py:301–334` | 12x36 19–07, previsto 660, pausa declarada 00–01, E19/S08:20, pausa julgada 07:20–07:40 no par. Regra dá trabalho 780, intra 60, HE0. Sonda de `reler` em memória dá 780/40/20. | Procede contra L-031/L-032/L-097. A base recortada é usada na HE e descartada para apurar intra. `app/docs/LEIS.md:79–80,115`. |
| O split noturno do motor usa a saída crua | `app/ponto/motor_calculo_v2.py:458–479,1084–1094`, `app/ponto/calculador/alimentacao.py:106–115` | 12x36 CLT 16–04, extra declarada 60, E16:20/S05:30. Sonda de `calcular_periodo` dá base 760, trabalho 790, HE50 40. Os 40 de HE até 05:00 são noturnos. Motor marca 10, oráculo ancora no marco e marca 40. | Procede contra L-035/L-097 e entre leitores. A leitura crua do motor sobrevive ao diff. `app/docs/LEIS.md:83,115`. |
| Comercial mantém previsto paralelo de 480 no caminho mensal | `app/ponto/motor_calculo_v2.py:3430,3617–3652`, `app/escala/utils.py:2284,2322–2345` | Template 5x2 Comercial diurno, célula 19–07 com pausa 00–01, E19:50/S00/E01/S07:20, parcial 19:20–19:50. Previsto 660, base 610, trabalho 630, atraso 20 e HE0 pela regra. O laço diário isolado escreve HE50 120 + HE100 10. | Procede contra a fonte única L-024. O limite incorreto já estava no código de base do diff. `app/docs/LEIS.md:75`. |
| `reler` não fornece parcial ao teto que empresta | `app/ponto/calculador/periodo.py:434–477,797–802`, `app/ponto/tests/test_ramo2b_c2h_saida.py:133–151` | Caso acima no 12x36. Regra e `do_dia` abatem 30 do atraso bruto de 50 e cobram 20. V3 de ensaio cobra 30 pelo teto `660−630`; nenhuma parcial entra. | Procede contra o corte de abater a ponta antes do teto. O repasse faltava no código-base da v3 de ensaio. `app/ponto/motor_calculo_v2.py:1242–1275,1288–1306`. |
| Pausa julgada vira trabalho quando a base fica abaixo do gatilho | `app/ponto/motor_calculo_v2.py:2968–2988,2998,3449–3471,3484` | Janela 19–07, fim 08:20, pausa interna julgada 07:20–07:40. 12x36 entra 01:10, base 350, trabalho líquido 410, API dá 430. Comercial entra 03:10, base 230, trabalho 290, API dá 310. Sondas diretas com pausa já entregue à porta. O pareamento automático desses quatro eventos [não verificado]. | Procede no contrato da porta contra L-032/L-097(b). `app/docs/LEIS.md:80,115`. |
| A testemunha usa dia civil e perde o último par no corte da competência | `app/folha/porta_export.py:340–359,585–591`, `app/ponto/motor_calculo_v2.py:3200–3224,3374–3381` | Partido dia 20, 19–07, vão 00–03, E19/S00:20, E03/S07:30 no dia 21. Os dois são do dia 20. Previsto 540, base 560, intra 20, teto HE0. Se a testemunha recebe 30 de HE indevida e intra 20 no último par, deve acusar um dia. A função descarta esse par pelo dia civil 21 e retorna zero linhas. | Procede contra L-085/L-097. O corte civil já existia em `medir`. `app/docs/LEIS.md:115,117`. |
| Os testes c2h/c2j deixam escapar as portas mensais e de alimentação | `app/ponto/tests/test_ramo2b_c2h_saida.py:62–77,91–113,123–161`, `app/ponto/motor_calculo_v2.py:3184–3224,3890–3920` | O teste parcial passa base, pausa e lista diretamente a `do_dia`. As outras grades chamam `calcular_periodo`, esvaziam `pont_por_dia` e injetam o previsto. Os dois controles Partido usam vão 60 que, se usado na fábrica com os quatro marcos, escolhe o motor contínuo 12x36. A indenização do Partido mora em `calcular_mes`. | Procede como selo sem mordida para esses caminhos. `CLAUDE.md:28–29,45–49`. |

Para o Partido de fato, 19–07 com vão 00–03, E19:50/S00 e E03/S07:20, a parcial 19:20–19:50 cobre 30 dos 50 minutos de atraso. Pela regra e pelo caminho estático da régua de relógio, previsto 540, trabalho 510, base 490, atraso 20, intra 0, HE 0. O Partido consta alterado no diff acumulado em `calcular_periodo` e `calcular_mes`. A autoria exata por commit [não verificado]. `app/ponto/motor_calculo_v2.py:3103–3118,3164–3167,3200–3224,3295–3302,3347–3381,3416`.

## 27/11 01:28 · sessao 69677b25d33b · - · - · origem=Ronald (tmux)

**REPROVADO.** A releitura v3 usa o intervalo da cauda para reduzir a indenização e combinar esse valor com a base de HE recortada. No 12x36 de 19h–07h, previsto líquido de 660 min, intervalo cadastrado 00h–01h e batidas E19h/S07h20/E07h40/S08h20, a regra dá 780 min trabalhados, 60 de intra e zero HE. Executei as funções da cópia em memória e obtive 780, 40 e 20. O caso usa a pausa entregue diretamente ao período, conforme a fixture. A divergência procede contra a L-031, L-032, L-097 e o corte da pausa na cauda. [periodo.py:745](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/ponto/calculador/periodo.py:745) [periodo.py:834](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/ponto/calculador/periodo.py:834) [PROMPTS.md:3071](/home/ronald/codex_ws/7f954d9211ae5382b1aae074c32c1fe4683e0fd6/app/docs/PROMPTS.md:3071)

- **Parcial na releitura v3.** No caso c2j E19h50/S00h/E01h/S07h20, parcial aprovada 19h20–19h50, previsto 660 e trabalho 630, a regra dá atraso 20. A releitura deixa de alimentar a parcial e limita o atraso bruto de 50 a 30. A omissão procede contra o aval do abono. O módulo declara zero leitores em produção. `app/ponto/calculador/periodo.py:8–20,434–435,474–476,797–825` · `app/docs/PROMPTS.md:3031`.
- **Exportação, dia errado.** No dia final 20/11, jornada 23h50–07h50, intervalo 02h–03h, E00h02/S08h20 de 21/11, previsto 420, base 468, trabalho 498, intra 60, uma HE indevida injetada de 30 min devia produzir um alerta pelo teto zero. Executei a testemunha em memória e obtive zero alertas pela data civil e um pelo dia do turno. Procede contra a L-085. A data civil é herdada. Com intra zero a testemunha retorna vazio ainda que haja 30 HE acima de teto zero. Esse segundo limite pertence ao teste HE×intra e não demonstra falha no contador de pendências. `app/folha/porta_export.py:344–359,548,587–591` · `app/docs/LEIS.md:117`.
- **Split da HE noturna.** No 12x36 16h–04h com 15 min de extra declarada depois, E16h/S05h30, janela ativa e prorrogação noturna desligada, a HE50 de 15 min termina às 04h15. A função real dá 15 HE noturna com a ponta da base e zero com a batida 05h30. Motor passa a batida, oráculo passa a ponta. O motor já usava o fim cru. O diff torna explícita a divergência dos leitores. `app/ponto/motor_calculo_v2.py:437–479,1084–1090,3052–3072` · `app/ponto/calculador/alimentacao.py:106–115` · `app/ponto/calculador/regras.py:845–865` · `app/docs/LEIS.md:83,115`.
- **Tripla.** A porta explícita está em `app/ponto/motor_calculo_v2.py:1927–1943` e recebe gravação em `:1945–1956`, reset em `:1613–1621` e crédito em `:2742–2765`. O construtor gerado aceita `(720, None, None)` em `:207–208,244–246`, verificado em memória. Os três chamadores de produto encontrados fornecem entrada e saída em `:2956,3095,3437`. O reset mantém o sentido de trabalho zero. O crédito aberto grava soma de pares e instante do último com cauda no mesmo campo que no par fechado indica o ponto final desse par. A ocorrência em fluxo real está [nao verificado]. O próprio teste declara ausência de fixture em `app/ponto/tests/test_ramo2b_c2_base_da_he.py:361–366`.
- **Derivação.** O motor guarda a base por par. Folha consulta o valor em `motor_calculo_v2.py:280–284` / `folha/porta_export.py:339–351`. Oráculo recalcula minutos e pausas do dia com o instante, em `ponto/calculador/alimentacao.py:168–178,469–474,545–548` e `ponto/calculador/nucleo.py:134–153`. A releitura v3 calcula de novo em `ponto/calculador/periodo.py:749–756`. Pontualidade recebe trabalho integral em `alimentacao.py:535–544` e chama a autoridade da L-093 em `regras.py:274–285,879–888`. Parcial vem de `credito_parcial.py:15–32,54–70`; limites inclusivos de marcos e direção das batidas em `turnos.py:2653–2711`, fatia semiaberta `[k,k+1)` em `motor_calculo_v2.py:481–505`.
- **Partido.** O diff contra main muda `calcular_periodo` e `calcular_mes` em `app/ponto/motor_calculo_v2.py:3094–3117,3164–3167,3213–3217,3317–3326,3356–3362`. `app/docs/PROMPTS.md:3087` registra «motor do Partido intocado». Os dois controles usam vão de 60 e `calcular_periodo` direto, em `app/ponto/tests/test_ramo2b_c2h_saida.py:64–67,121–131`. Uma quebra na intra mensal em `app/ponto/motor_calculo_v2.py:3187–3227` deixaria os controles verdes. A atribuição histórica de cada hunk a um commit está [nao verificado].
- **Competência exportada.** O helper da folha recebe `esp['resultado'].periodos` e afeta `falhas`; não muda o objeto de exportação diretamente. O motor alterado entra no recálculo por evento, que escreve em competência entregue fora da guarda especial da 09, de acordo com a L-113 e a trilha existente. Não foi verificada regravação em produção. `app/folha/porta_export.py:587–591,713–721` · `app/ponto/services/fechamento.py:36–50,559–590,1161–1215,1822–1848` · `app/docs/LEIS.md:155`.

## 27/11 01:24 · sessao e1354a6c81d9 · PAR-PELO-TURNO · construtor · origem=Code

## Item 1 — Dia por PK e carga

- **RED na base `794ab3332`.** Nos ensaios em memória, `Ran 5 tests`, `FAILED (failures=5)`, `rc 1`. Os PKs 20 e 21 recebiam dia **17/10** em `[17,21]` e **20/10** em `[20,20]`. A folha, o espelho e a fatia entregavam só `[20]` para o par de borda, que exigia `[20, 21]`. `app/ponto/tests/test_par_pelo_turno.py:195,225,636`
- **Cura na origem.** `_marca_intra_duracao` e `parear_turnos` aplicam as 26 h também à absorção de marcas. `_turnos_do_colab_puro` julga a véspera que uma entrada pode ter herdado. Folha, fatia e espelho passam a carregar pelo alcance de `janela_com_pad`. `app/ponto/turnos.py:390,1381,1844,1989,2989` · `app/ponto/services/fechamento.py:262,638,811` · `app/ponto/services/espelho.py:1032`
- **GREEN local.** O par dos PKs 21 e 22 fica em **20/10** nas três janelas do selo. O PK 20 fica no dia julgado para a órfã, **20/10** ou **19/10** conforme o DNA da véspera, com a mesma resposta nas janelas que contêm a marca. O `Motor12x36` recebeu `[20, 21]` e fechou `20/10 23:30 → 22/10 01:30`, **26 h**, no dia **20/10**. `Ran 57 tests`, `OK`, em memória. Reposicionei as marcas de dois testes de fuso nas novas bordas da janela. `app/ponto/tests/test_par_pelo_turno.py:195,225,253` · `app/ponto/tests/test_composicao_de_fatias_e_total.py:306,316`
- **Mutantes.** Os cinco mutantes unitários do item deram `rc 1` por asserção. Nos dois mutantes de carga, a sonda das funções voltou de `[20, 21]` para `[20]` em cada leitor. A integração Django está **[nao verificado]**. A chamada local parou no import com `ModuleNotFoundError: No module named 'django.test'`. **`except` novos: 0**, por comparação AST dos cinco arquivos alterados. `app/ponto/tests/test_par_pelo_turno.py:7,636,808,868,920`

## Item 5 — Laudo `col155/08-11`

Cabe **pela regra da série HJ-026**. O ensaio sintético de `17:29 → 06:31` fecha no dia **08/11**, com C9 de **782 min** contra teto de **732 min**. O selo passou na base e na cópia. O mutante `laudo_hj026_sem_c9` deu `rc 1`, com `[]` onde o selo exigia a C9. Pontas e PKs reais, papéis intermediários, matriz dos 29 casos e sombra por rubrica **[nao verificado]**. `app/docs/BACKLOG.md:745,2027` · `app/ponto/tests/test_par_pelo_turno.py:80,825`

**LEI-AKITA — padrao=Single Source of Truth.** O pareador julga turno e dia, e `janela_com_pad` fornece o alcance das cargas. `app/core/juizes.py:216` · `app/ponto/turnos.py:300,1844,3055`

| Campo ou valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `_intra_dur`, `Turno.batidas`, `Turno.data_turno`, dia por PK | A pré-passagem carimba, `parear_turnos` compõe, `_mk` publica e `_dia_das_batidas_puro` projeta. `app/ponto/turnos.py:300,419,1381,3055` | Pós-fecho, consulta individual e espelho. `app/ponto/turnos.py:1109,3036` · `app/ponto/services/espelho.py:465` |
| Limites e lista de batidas apuráveis | `janela_com_pad` fornece as datas e `batidas_apuraveis_na_janela` consulta pelo universo de apuração. `app/ponto/turnos.py:1844,2959,2989` | Casca, lote, folha, fatia e espelho. `app/ponto/turnos.py:1882,3094` · `app/ponto/services/fechamento.py:262,638,811` · `app/ponto/services/espelho.py:1032` |
| Células e feriados da borda | Cargas de folha e espelho usam o alcance do pareador. `app/ponto/services/fechamento.py:609,619` · `app/ponto/services/espelho.py:1036,1056` | Motor da folha e do espelho. `app/ponto/services/fechamento.py:744,752` · `app/ponto/services/espelho.py:1121,1139` |

### Comandos para o Code

A partir de `app/`. O modo `base_c1v5a` monta as fontes da base em memória. A suite com `--dir` usa a pista remota, cuja execução fica **[nao verificado]**. `app/ponto/tests/test_par_pelo_turno.py:946,980` · `bin/suite.sh:319,326`

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci

python -m ponto.tests.test_par_pelo_turno base_c1v5a

for mutante in \
  pausa_inferida_fora_do_envelope miolo_carimbado_fora_do_envelope \
  contexto_sem_vespera_da_entrada carga_so_uma_virada \
  fatia_so_uma_virada folha_so_uma_virada espelho_so_uma_virada \
  laudo_hj026_sem_c9
do
  python -m ponto.tests.test_par_pelo_turno "$mutante"
done

../bin/suite.sh --dir /home/ronald/wt-codex-par-pelo-turno-010806 \
  --only "ponto.tests.test_par_pelo_turno ponto.tests.test_composicao_de_fatias_e_total ponto.tests.test_janela_da_autoridade_e_a_da_folha ponto.tests.test_dupla_contagem_dia20 ponto.tests.test_col129_par_separado_pelo_teto ponto.tests.test_col129_teto_vira_lista ponto.tests.test_contract_no_batida_date ponto.tests.test_contract_folha_le_a_autoridade ponto.tests.test_tripwire_b5j_universo_de_apuracao ponto.tests.test_fechamento_escritor_guardas"
```

## 27/11 01:19 · sessao e5b196ca57e5 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Ajustei os três registros da catraca em `core/tests/test_medida_turno_do_par.py`. **GREEN local: 35 de 35 testes em memória**, usando `unittest.TestCase` no lugar de `django.test.SimpleTestCase`. A suíte do Code e o Opus final estão [nao verificado]. `core/tests/test_medida_turno_do_par.py:28`, `:194`, `:251`, `:425`

As três divergências vieram da obra no merge `3b594ce1f`. No main `59600163c`, os três registros coincidiam com o censo da própria revisão. `core/medida_turno_do_par.py:336`, `:448`

| Falha | Causa | Cura | GREEN local |
|---|---|---|---|
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` | O merge deslocou ocorrências do motor, incluindo `909 → 937` e `2521 → 2601`, e moveu a ocorrência da folha de `medir:544` no main para `he_acima_do_teto_por_dia:344`. `ponto/motor_calculo_v2.py:937`, `:2601`; `folha/porta_export.py:344` | Atualizei as linhas medidas nas duas listas e registrei a obra em um comentário de uma linha. `core/tests/test_medida_turno_do_par.py:251`, `:288`, `:304`, `:348` | Passou. `core/tests/test_medida_turno_do_par.py:565` |
| `test_CATRACA_numeros_exatos_e_registro_no_mesmo_commit` | A chamada `_p.entrada.date()` passou de `medir` para `he_acima_do_teto_por_dia`, trocando a identidade B1. `folha/porta_export.py:344`, `:587`; `core/medida_turno_do_par.py:217` | Registrei a troca de identidade. Os contadores conferem em **9 pareadores e 81 derivações de dia**. `core/tests/test_medida_turno_do_par.py:108`, `:194` | Passou. `core/tests/test_medida_turno_do_par.py:558` |
| `test_a_classificar_publicado_sem_entrar_como_criterio_amplo` | Em `medir`, a data do ato ainda é medida como B2. Com a saída da ocorrência B1, `medir` aparece em `B2_menos_B1`. `folha/porta_export.py:391`; `core/medida_turno_do_par.py:223`, `:432` | Registrei `medir` em `CLASSIFICAR['B2_menos_B1']`. `core/tests/test_medida_turno_do_par.py:425` | Passou. `core/tests/test_medida_turno_do_par.py:571` |

**`except` novos: 0.** A comparação AST encontrou o mesmo `except` antes e depois. O diff ficou restrito aos três registros e ao comentário no arquivo de teste. `core/tests/test_medida_turno_do_par.py:194`, `:251`, `:425`, `:668`

### RED e mutantes

A partir de `app/`, a função abaixo restaura em memória os valores da base `03bc04e5b`. O RED reproduz as três falhas. Cada mutante restaura um único registro e derruba somente a asserção correspondente. `core/tests/test_medida_turno_do_par.py:558`, `:565`, `:571`

```bash
catraca() {
  GIT_OPTIONAL_LOCKS=0 PYTHONDONTWRITEBYTECODE=1 python3 -B - "$1" <<'PY'
import ast
import pathlib
import subprocess
import sys
import types
import unittest

path = pathlib.Path('core/tests/test_medida_turno_do_par.py')
suite = types.ModuleType('suite_turno') [nao verificado]
suite.__file__ = str(path.resolve()) [nao verificado]
source = path.read_text().replace('from django.test import SimpleTestCase',
                                  'from unittest import TestCase as SimpleTestCase')
exec(compile(source, str(path), 'exec'), suite.__dict__) [nao verificado]
mode = sys.argv[1]
records = ('LINHAS_REGISTRADAS', 'REGISTRO', 'CLASSIFICAR')
assert mode in ('green', 'red', *records)
if mode != 'green':
    baseline = subprocess.check_output(
        ['git', 'show', f'03bc04e5b:app/{path}'], text=True)
    selected = records if mode == 'red' else (mode,)
    for node in ast.parse(baseline).body:
        if isinstance(node, ast.Assign) and isinstance(node.targets[0], ast.Name):
            name = node.targets[0].id
            if name in selected:
                setattr(suite, name, ast.literal_eval(node.value)) [nao verificado]
result = unittest.TextTestRunner(verbosity=1).run(
    unittest.defaultTestLoader.loadTestsFromModule(suite)) [nao verificado]
raise SystemExit(not result.wasSuccessful())
PY
}
```

| Comando | Resultado local |
|---|---|
| `catraca green` | rc 0, 35 testes passaram. `core/tests/test_medida_turno_do_par.py:503` |
| `catraca red` | rc 1, três falhas de asserção. `core/tests/test_medida_turno_do_par.py:558`, `:565`, `:571` |
| `catraca LINHAS_REGISTRADAS` | rc 1, somente a asserção de linhas. `core/tests/test_medida_turno_do_par.py:565` |
| `catraca REGISTRO` | rc 1, somente a asserção de identidade. `core/tests/test_medida_turno_do_par.py:558` |
| `catraca CLASSIFICAR` | rc 1, somente a asserção de publicação. `core/tests/test_medida_turno_do_par.py:571` |

### Recorte do Code

Comando do anexo c2j, a partir da raiz da cópia. Execução [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh \
  --dir /home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946 \
  --only "ponto.tests.test_ramo2b_c2_base_da_he ponto.tests.test_ramo2b_c2h_saida ponto.tests.test_ramo2b_turno_que_cruza ponto.tests.test_contract_juiz_batida ponto.tests.test_janela_he_saida ponto.tests.test_o146_extra_declarada_da_escala ponto.tests.test_o280_bateria_da_lei core.tests.test_medida_turno_do_par folha.tests.test_porta_do_export"
```

**LEI-AKITA** `padrao=juiz unico` · `juizes novos=0` · `docs/LEIS.md:60` · `core/tests/test_medida_turno_do_par.py:251`

## 27/11 01:06 · sessao 42b6cdad486f · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Na base `3b594ce1f`, `do_dia` já devolve o número esperado para parcial com cauda pendente. O caso mede **610 min de base da HE, 630 min trabalhados, 20 min de atraso e zero de saída antecipada, intra indenizada e HE em dia comum**. Com feriado em dobra, são **610 min de HE100**. O cálculo de produção ficou como veio no merge. [Regra escrita](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/docs/LEIS.md:79) · [L-097 emendada](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/docs/LEIS.md:115) · [Teste](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:133)

`rubricas_por_dia` carrega as parciais pagas com período declarado e passa a lista do dia a `do_dia`. A base e a HE são apuradas primeiro. O abatimento é posterior, em `pontualidade_do_dia`, pelo teto emprestado do motor. A parcial cobre 30 dos 50 min de atraso bruto. Os 20 restantes passam pelo teto da L-093, de `660 − 630 = 30 min`. [Chamador](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/alimentacao.py:535) · [Base e dobra](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/regras.py:797) · [Leitor de `parciais`](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/calculador/regras.py:879) · [Abatimento](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1270) · [Teto](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1304)

| Item | RED | GREEN local |
|---|---|---|
| 1. Catraca | Os dois registros `[530]` divergiam do AST `[558]`. | Os dois registros pedidos conferem. Na catraca integral, **3 de 35 testes falham**. Restam 2 divergências em `pareadores` e 13 em `derivacoes_de_dia`, incluindo a troca de identidade de `medir` para `he_acima_do_teto_por_dia`. A troca também afeta `CLASSIFICAR`. [Registros](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:251) · [Asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:547) · [Exportação](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/porta_export.py:344) | [nao verificado]
| 2. Parcial com cauda | `ignora_parciais` dá **30 min de atraso** onde a regra pede **20**. | Os três cenários passam na base do merge. [Teste](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:133) · [Mutação](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2h_mutantes.py:12) | [nao verificado]
| 3. Bateria de saída | Com a alimentação de `deed2fe72`, 8 subcasos falham por asserção. No cruzamento, o gozo é **0** em vez de **20 min**. | Oito casos em cada motor, 12x36 e Comercial, mais os dois controles do Partido previstos em `RAMO2-PARTIDO-2611-1812 (a)`. Com os três cenários do item 2, são **21 subcasos verdes em memória**. [Grade](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:32) · [Execução e controles](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:115) |
| 4. Vizinhos | As três falhas da catraca persistem após o ajuste restrito aos dois registros. [Asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:547) | Os selos do escritor e do contrato da batida passaram em memória, **22 testes**. Suíte Django do Code e Opus final [nao verificado]. [Escritor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:622) · [Contrato](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_contract_juiz_batida.py:159) | [nao verificado]

### Comandos

Medição da catraca a partir de `app/`. Resultado `pareadores [558]` e `derivacoes_de_dia [558]`. [Censo](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/medida_turno_do_par.py:448) · [Chamada medida](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:558)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
from core.medida_turno_do_par import censo

medida = censo('.')
for grupo in ('pareadores', 'derivacoes_de_dia'):
    print(grupo, next(i['linhas'] for i in medida[grupo]['itens']
                      if i['chave'] == 'ponto/motor_calculo_v2.py::turnos_via_autoridade'
                      and i.get('sitio') == 'porta_parear_turnos'))
PY
```

Para a sessão rodar o recorte Django a partir da raiz da cópia. [Rota de suite da cópia](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/bin/suite.sh:26) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh \
  --dir /home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946 \
  --only "ponto.tests.test_ramo2b_c2_base_da_he ponto.tests.test_ramo2b_c2h_saida ponto.tests.test_ramo2b_turno_que_cruza ponto.tests.test_contract_juiz_batida ponto.tests.test_janela_he_saida ponto.tests.test_o146_extra_declarada_da_escala ponto.tests.test_o280_bateria_da_lei core.tests.test_medida_turno_do_par folha.tests.test_porta_do_export"
```

Para reproduzir os mutantes a partir de `app/` no ambiente Django da sessão, defina a função abaixo. As saídas da tabela foram medidas pelo mesmo executor em `unittest`, com dublês de infraestrutura em memória e zero erros de execução. [Executor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2h_mutantes.py:82)

```bash
ramo2() { PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.ramo2b_c2h_mutantes "$1"; }
```

| Comando | Saída local |
|---|---|
| `ramo2 green` | rc 0, 3 testes, 21 subcasos | [nao verificado]
| `ramo2 ignora_parciais` | rc 1, 2 falhas, atraso 30 em vez de 20 |
| `ramo2 teto_le_base_da_he` | rc 1, 1 falha, atraso do controle 50 em vez de 30 |
| `ramo2 abono_compoe_base_da_he` | rc 1, 1 falha, HE100 de 640 em vez de 610 |
| `ramo2 dobra_inclui_cauda` | rc 1, 1 falha, HE100 de 630 em vez de 610 |
| `ramo2 red_deed2fe72` | rc 1, 8 falhas, gozo 0 em vez de 20 no cruzamento |
| `ramo2 sem_ponta` | rc 1, 8 falhas, gozo 0 em vez de 20 no cruzamento |
| `ramo2 copy_setattr` | rc 1, 8 falhas, base 1 em vez de 700 no cruzamento |
| `ramo2 teto_max` | rc 1, 16 falhas, trabalhado 800 em vez de 760 no cruzamento |
| `ramo2 inclui_cauda` | rc 1, 5 falhas, base 760 em vez de 680 na pausa antes do marco |
| `ramo2 perde_fim_no_marco` | rc 1, 2 falhas, gozo 0 em vez de 20 na pausa que termina no marco |

### Hand-back

A contagem AST deu **0 `except` novos** nos três arquivos alterados. O diff do motor contra `138b516f2` tem **97 inserções em três blocos** e é idêntico ao diff do merge `3b594ce1f` contra a mesma referência. [Catraca](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_medida_turno_do_par.py:251) · [Bateria](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2h_saida.py:123) · [Mutantes](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2h_mutantes.py:12) · [Motor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:481)

```bash
GIT_OPTIONAL_LOCKS=0 git diff 138b516f2 -- ponto/motor_calculo_v2.py
GIT_OPTIONAL_LOCKS=0 git diff --exit-code 3b594ce1f -- ponto/motor_calculo_v2.py
```

Corpo proposto para o commit. [nao verificado]

```text
RAMO2-B c2j: cobre parcial com cauda e registra linha da catraca

OBRA: RAMO2-B-EM-TURNO-QUE-CRUZA
BASE: 3b594ce1f4bd2bfa4a2049c149ecb254643ff526

Registra porta_parear_turnos em 558 nos dois contadores.
Fixa pela regra 610 min de base, 630 trabalhados e 20 de atraso
com parcial. A grade cobre 12x36 e Comercial, com os dois controles
do Partido pelo aval RAMO2-PARTIDO-2611-1812 (a).

LEI-AKITA:
origem=docs/LEIS.md:79-80,111,115; docs/PROMPTS.md:3031,3071,3087
escrita=ponto/motor_calculo_v2.py:1926-1963
testemunha=ponto/calculador/nucleo.py:146-153, segmentos e batidas
RED=ponto/tests/test_ramo2b_c2h_saida.py:133, mutante ignora_parciais
RED da grade=ponto/tests/test_ramo2b_c2h_saida.py:115, alimentacao de deed2fe72
quem-mais-le=ponto/calculador/alimentacao.py:106-117,168-178,469-475,535-566
  (rubricas_por_dia -> ponto/calculador/regras.py:614-626,775-807,879-888);
  ponto/services/fechamento.py:1291-1300;
  ponto/management/commands/diff_calculador.py:414-424;
  ponto/calculador/periodo.py:745-759; folha/porta_export.py:339-358
padrao=juiz unico
juizes novos=0

fora do marco: PAUSA-NA-BORDA-DO-PAR, PAUSA-INTERNA-NO-PARTIDO

Catraca integral: tres falhas medidas em memoria fora dos dois
registros ajustados (linhas, identidade e CLASSIFICAR).
```

## 27/11 00:12 · sessao b7c6fd3c7578 · V6-FURO-OUTPUT · leitura · origem=Code

# V6-FURO-OUTPUT

HEAD conferido em `59600163ccc9cffb8f3ae148beede7cd7866ae86`. A leitura Bash do `.output` registrado no RELATO hoje retorna zero alvos. Confirmei em memória que três chamadas `cat` desse mesmo caminho deixam o contador vazio. [.git/refs/heads/main:1](/home/ronald/saas-hasner/.git/refs/heads/main:1) · [app/docs/RELATO.md:3041](/home/ronald/saas-hasner/app/docs/RELATO.md:3041) · [bin/disjuntor.py:2142](/home/ronald/saas-hasner/bin/disjuntor.py:2142)

## 1. Mapa do reconhecimento

| Entrada | O que conta hoje |
|---|---|
| `tail`, `cat`, `grep`, `ls` | Argumentos terminados em `.rc` ou `.log`, em qualquer diretório. Para `ls`, também conta um argumento com `logs` como componente do caminho. A análise usa o basename do comando, descarta opções e pula os redirecionamentos que reconhece. [bin/disjuntor.py:1985](/home/ronald/saas-hasner/bin/disjuntor.py:1985) · [bin/disjuntor.py:2131](/home/ronald/saas-hasner/bin/disjuntor.py:2131) · [bin/disjuntor.py:2142](/home/ronald/saas-hasner/bin/disjuntor.py:2142) |
| `head`, `wc` | Ficam fora da lista de comandos de consulta. [bin/disjuntor.py:1985](/home/ronald/saas-hasner/bin/disjuntor.py:1985) · [bin/disjuntor.py:2142](/home/ronald/saas-hasner/bin/disjuntor.py:2142) |
| `vercod`, `vercode`, `vercodex`, `python* …/vercodex.py` | Geram a chave textual `vercodex ` seguida dos argumentos, excluído `vercodex.py`. [bin/disjuntor.py:1986](/home/ronald/saas-hasner/bin/disjuntor.py:1986) · [bin/disjuntor.py:2139](/home/ronald/saas-hasner/bin/disjuntor.py:2139) |
| `ssh pista estado …` | Gera a chave textual `ssh pista estado ` seguida dos argumentos após `estado`. `ssh pista espera` e os laços de espera reconhecidos retornam lista vazia. [bin/disjuntor.py:2120](/home/ronald/saas-hasner/bin/disjuntor.py:2120) · [bin/disjuntor.py:2132](/home/ronald/saas-hasner/bin/disjuntor.py:2132) |

O **PreToolUse recebe `Read`** pelo `matcher` vazio, e a guarda geral lê `tool_input.file_path`. O **polling** roda só para `Bash` sem `agent_id`, usando `tool_input.command`. Uma mudança apenas em `_alvos_de_consulta` alcança esse ramo Bash. [.claude/settings.json:62](/home/ronald/saas-hasner/.claude/settings.json:62) · [bin/disjuntor.py:222](/home/ronald/saas-hasner/bin/disjuntor.py:222) · [bin/disjuntor.py:2217](/home/ronald/saas-hasner/bin/disjuntor.py:2217)

## 2. Chamadores, memória e identidade

- **Chamada única em produção:** `main_pretool` → `pretool` → `_pretool` → `espera_nega(cmd) or polling_nega(...)` → `_alvos_de_consulta(cmd, cwd)`. Uma negativa de `espera_nega` encerra a avaliação antes do polling. [bin/disjuntor.py:2303](/home/ronald/saas-hasner/bin/disjuntor.py:2303) · [bin/disjuntor.py:2207](/home/ronald/saas-hasner/bin/disjuntor.py:2207) · [bin/disjuntor.py:2225](/home/ronald/saas-hasner/bin/disjuntor.py:2225) · [bin/disjuntor.py:2177](/home/ronald/saas-hasner/bin/disjuntor.py:2177)
- **Memória:** `logs/sessao/disjuntor_estado.json`, sob `HOOK_STOP_RAIZ` ou a raiz derivada do script. `_com_estado` usa `flock` e grava por `os.replace`. `poll` guarda listas de timestamps por `sessão + "\t" + alvo`. A sessão vem de `session_id`, depois `transcript_path`, depois `"-"`. Com duas consultas aceitas há **menos de 600 s**, a terceira é recusada. A tentativa recusada não acrescenta timestamp. O teto da espera é **300 s**. [bin/disjuntor.py:122](/home/ronald/saas-hasner/bin/disjuntor.py:122) · [bin/disjuntor.py:340](/home/ronald/saas-hasner/bin/disjuntor.py:340) · [bin/disjuntor.py:374](/home/ronald/saas-hasner/bin/disjuntor.py:374) · [bin/disjuntor.py:1953](/home/ronald/saas-hasner/bin/disjuntor.py:1953) · [bin/disjuntor.py:1962](/home/ronald/saas-hasner/bin/disjuntor.py:1962) · [bin/disjuntor.py:2183](/home/ronald/saas-hasner/bin/disjuntor.py:2183)
- **Caminho:** usa `normpath` sobre o caminho absoluto ou sobre `join(cwd, relativo)`. Sem `cwd`, o relativo continua relativo. A função usa o `cwd` do payload, sem aplicar `cd` do comando nem resolver symlinks. Cada alvo normalizado conta uma vez por chamada. [bin/disjuntor.py:2116](/home/ronald/saas-hasner/bin/disjuntor.py:2116) · [bin/disjuntor.py:2151](/home/ronald/saas-hasner/bin/disjuntor.py:2151) · [bin/disjuntor.py:2180](/home/ronald/saas-hasner/bin/disjuntor.py:2180)

## 3. Forma do `.output`

O recorte para o aval é casar **o caminho normalizado inteiro** com `/tmp/claude-[0-9]+/[^/]+/[^/]+/tasks/[^/]+\.output` usando `fullmatch`. Isso exige UID decimal, um componente de projeto, um de sessão e o arquivo diretamente em `tasks/`. A árvore documenta um caminho real nessa forma e usa `/tmp/claude-%d` para o diretório do Code. `esteira.out`, `wrapper.out` e um `*.output` comum do repo ficam fora desse recorte. [bin/disjuntor.py:2153](/home/ronald/saas-hasner/bin/disjuntor.py:2153) · [app/docs/RELATO.md:3041](/home/ronald/saas-hasner/app/docs/RELATO.md:3041) · [bin/retencao_disco.py:638](/home/ronald/saas-hasner/bin/retencao_disco.py:638) · [fatias_agendadas/o135-teto20/esteira.sh:26](/home/ronald/saas-hasner/fatias_agendadas/o135-teto20/esteira.sh:26) · [bin/tests/prova_codex_pergunta_real.sh:85](/home/ronald/saas-hasner/bin/tests/prova_codex_pergunta_real.sh:85)

## 4. Onde nasce o RED

`bin/tests/test_disjuntor.sh` tem o helper `_pre`, mas nenhum caso de `polling_nega`. O vizinho do RED é **`polling()` em `bin/tests/test_agente_no_modelo.sh:213`**. Ele cobre primeira e segunda aceitas, terceira negada, relativo e absoluto como o mesmo alvo, bordas de 599 e 600 s, alvos e sessões distintos, `ssh pista estado`, `vercodex`, `ls logs/`, subagente e espera única. `divergencia()` também verifica o limiar e a janela, e o mutante `polling-desligado` exercita a ligação ao hook. [bin/tests/test_disjuntor.sh:103](/home/ronald/saas-hasner/bin/tests/test_disjuntor.sh:103) · [bin/tests/test_disjuntor.sh:1621](/home/ronald/saas-hasner/bin/tests/test_disjuntor.sh:1621) · [bin/tests/test_agente_no_modelo.sh:213](/home/ronald/saas-hasner/bin/tests/test_agente_no_modelo.sh:213) · [bin/tests/test_agente_no_modelo.sh:297](/home/ronald/saas-hasner/bin/tests/test_agente_no_modelo.sh:297) · [bin/tests/test_agente_no_modelo.sh:345](/home/ronald/saas-hasner/bin/tests/test_agente_no_modelo.sh:345)

## 5. Espera única

**A inclusão desse alvo no reconhecedor não recusa a espera única.** `ESPERA_UNICA` chama `rc_sem_colher.py`, que lê o RELATO e o conteúdo dos `.rc` selecionados, e usa `find` sobre `*.rc`. Seu laço já retorna zero alvos no extrator. Conferi também `logs/espera_unica.sh` em disco, fora do HEAD e ignorado pelo Git. Ele chama o mesmo juiz e usa `head` apenas nos arquivos `*.rc` devolvidos pelo `find`. Nenhuma das duas formas abre arquivo de tarefa `.output`. [bin/disjuntor.py:1967](/home/ronald/saas-hasner/bin/disjuntor.py:1967) · [bin/disjuntor.py:2120](/home/ronald/saas-hasner/bin/disjuntor.py:2120) · [bin/rc_sem_colher.py:71](/home/ronald/saas-hasner/bin/rc_sem_colher.py:71) · [bin/rc_sem_colher.py:101](/home/ronald/saas-hasner/bin/rc_sem_colher.py:101) · [.gitignore:53](/home/ronald/saas-hasner/.gitignore:53) · [logs/espera_unica.sh:5](/home/ronald/saas-hasner/logs/espera_unica.sh:5) · [logs/espera_unica.sh:9](/home/ronald/saas-hasner/logs/espera_unica.sh:9)

## 27/11 00:08 · sessao 99ce13081ea2 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

**APROVADO na revisão de código da volta 10.** O achado 1 foi curado na origem. O descarte do achado 4 procede para a admissão do construtor Opus prevista em RAIAS item 3. `app/core/tests/test_model_sem_migration_pendente.py:65–70` · `app/tenants/test_model_sem_migration_pendente.py:25–39` · `bin/deploy.sh:194–196` · `CLAUDE.md:325–330`

### 1. Mordida dos REDs

O RED de campo, usado para `folha` e `tenants`, e o RED de model novo em `api` agora invocam `pergunta_sem_migration_pendente`, a mesma função do deploy. Ambos exigem `SystemExit(1)` e a operação específica na saída. `app/core/tests/test_model_sem_migration_pendente.py:56–80` · `app/tenants/test_model_sem_migration_pendente.py:25–42` · `bin/deploy.sh:194–196`

| Mutante na pergunta | REDs de campo (`folha` e `tenants`) | RED de `api` |
|---|---|---|
| Retirar só `api` das labels enviadas ao Django | Passam. Cada um cobra uma alteração no próprio app. | Reprova. O Django deixa de receber a label necessária para detectar a migration inicial de `CercaMordeApi`. `app/core/tests/test_model_sem_migration_pendente.py:24–31,40–48,79–80` · `app/tenants/test_model_sem_migration_pendente.py:25–42` |
| Retirar `--check`, mantendo `--dry-run` | Reprovam por ausência de `SystemExit`. | Reprova pela mesma asserção. `app/core/tests/test_model_sem_migration_pendente.py:48,65–70` · `app/tenants/test_model_sem_migration_pendente.py:33–39` |
| Engolir o `SystemExit` do Django | Reprovam por ausência de `SystemExit`. | Reprova pela mesma asserção. `app/core/tests/test_model_sem_migration_pendente.py:48,65–70` · `app/tenants/test_model_sem_migration_pendente.py:33–39` |

A execução em memória com dublê do Django deu esses resultados. Execução com Django real nesta cópia [nao verificado].

### 2. Achado 4 da volta 9

**Descarte procedente para a admissão.** A redação da c10 exige `wt-*` e que `ARQUIVOS:` toque pelo menos um caminho de `migrations`, `core/juizes.py`, `crons.py` ou `bin/`. A lista vigente já exclui motor e `models.py`. O `any(...)` implementa essa condição, e o selo espera admissão de `views.py` junto com um caminho da lista. `CLAUDE.md:325–330` · `bin/disjuntor.py:1961,2102–2113` · `bin/tests/test_agente_no_modelo.sh:127–143`

O escopo de escrita continua regido por CODEX item 2 e L-126. Motor e `models.py` sem migration cabem ao Codex construtor, com Opus na verificação. Escrita efetiva pelo Opus fora desse escopo [nao verificado]. `CLAUDE.md:402–406` · `app/docs/LEIS.md:147` · `aval.txt:1`

### 3. Quem escreve e quem lê

| Sítio | Escritor e leitores |
|---|---|
| **Pergunta** | `labels_locais()` produz a lista a partir do registro Django. A pergunta a entrega a `call_command`, que produz o resultado do check. Leem a pergunta `checa_sem_migration_pendente`, os REDs de campo e de `api`, e o deploy nas chamadas de decisão e diagnóstico. O Bash grava `DRIFT` pelo status e o lê na guarda. `app/core/tests/test_model_sem_migration_pendente.py:24–53,65–66` · `app/tenants/test_model_sem_migration_pendente.py:33–34` · `bin/deploy.sh:194–206` |
| **RED de campo** | `morde_campo_sem_migration` altera e restaura `max_length` em memória. O Django lê o model alterado pela pergunta comum. O teste lê o código de saída e a operação `Alter field`. Os casos são `RubricaDominio.evento` e `Cliente.nome`. `app/core/tests/test_model_sem_migration_pendente.py:56–80` · `app/tenants/test_model_sem_migration_pendente.py:41–42` |
| **RED de `api`** | O teste registra `CercaMordeApi` e o remove em `finally`, limpando o cache. O Django lê o model registrado pela pergunta comum. O teste lê o código de saída e `Create model CercaMordeApi`. `app/tenants/test_model_sem_migration_pendente.py:25–39` |

Na cópia, a única chamada Python a `call_command('makemigrations', …, '--check')` está dentro da própria pergunta. O ramo `*makemigrations*--check*` é um padrão do Docker falso. `app/core/tests/test_model_sem_migration_pendente.py:48` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:85–103`

### 4. Classes do delta

**Selo sem mordida:** curado para os três mutantes pedidos pelo conjunto dos REDs Python. O selo Bash simula o status pelo texto do comando. **Derivação paralela:** as duas invocações avulsas foram substituídas pela pergunta comum. **`except` novo e campo com dois significados:** o delta troca as chamadas e o import, preservando os `finally` e o significado dos valores testados. `delta.patch:1–34` · `app/core/tests/test_model_sem_migration_pendente.py:34–70` · `app/tenants/test_model_sem_migration_pendente.py:25–42` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–97`

Selos de host no hash, Opus final e prova das 03:30 são evidências da sessão [nao verificado]. `aval.txt:2`

## 27/11 00:02 · sessao cd30e8986317 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

**REPROVADO — volta 9, cópia `148372677`.** A c9 fecha o falso verde causado pela lista parcial no Bash e corrige o destino da obra de motor na L-126. O RED obrigatório de `api` ainda contorna a função usada pelo deploy. Selos verdes no hash, Opus final aprovado e execução real das 03:30 registrada no RELATO seguem [nao verificado]. `bin/deploy.sh:194–205` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `app/docs/LEIS.md:147` · `aval.txt:2`

## Achados da volta 8 e do Opus

| Achado | Situação na c9 e contraexemplo |
|---|---|
| **1. Schema sem label e falso verde por stdout parcial** | **Curado na origem do Bash.** A c9 passa a lista diretamente do registro Django ao `makemigrations`, no mesmo processo. O cenário da v8 em que sai `core` antes de um timeout agora recusa pelo status do comando. **Procedente no RED:** se a função nova omitir `api` ou perder `--check`, os testes que injetam drift continuam chamando `call_command` diretamente e o Docker falso continua respondendo pelo nome da função. `bin/deploy.sh:194–205` · `app/core/tests/test_model_sem_migration_pendente.py:40–48,56–70` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–97` |
| **2. Selos de host verdes no hash** | **Procedente, prova em aberto.** Os casos e mutantes existem no código. Um verde anterior à c9 ainda deixa a revisão atual sem prova. RED/GREEN integral do deploy, wrapper e agente no hash pedido [nao verificado]. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:244–262` · `bin/tests/test_codex_pergunta.sh:658` · `bin/tests/test_agente_no_modelo.sh:127–143,286–294` · `aval.txt:2` |
| **3. Portão na casca real** | **Procedente, prova em aberto.** O cron das 03:30 e o reload da cópia, inclusive ao desfazer, chegam ao portão quando as guardas anteriores deixam passar. Os casos 18 e 20 simulam migration existente pendente. Um deploy normal verde no falso ainda deixaria sem prova os settings efetivos e a execução das 03:30. Linha da prova no RELATO [nao verificado]. `app/config/crons.py:326–327` · `bin/molde_fatia/cadeia.sh:25–35,46–49` · `bin/deploy.sh:126–205` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:216–226` · `aval.txt:2` |
| **4. Duplo destino após duas reprovações** | **Curado na L-126.** Motor ou `models.py` fica com o construtor Codex e ganha postmortem. O repasse ao Opus vale só para o trabalho vedado ao Codex. **Procedente na admissão de escopo misto:** o disjuntor admite o brief se **qualquer** caminho casar com uma das quatro tags. Um brief com motor e migration pode passar nessa regra. O selo já espera admissão com `views.py` mais um caminho vedado. Edição efetiva pelo Opus fora do seu escopo [nao verificado]. `app/docs/LEIS.md:147` · `bin/disjuntor.py:1961,2023–2039,2102–2113` · `bin/tests/test_agente_no_modelo.sh:127–143` |
| **5. Pacote `models/` liberado** | **Curado na origem do inventário.** A regex barra o pacote e seus descendentes. O inventário também acrescenta `/` ao caminho de uma pasta nova vazia. O selo cria `folha/models/__init__.py` e tem mutante que retira a guarda. Um model declarado em `schema.py` e importado por `models.py` fica fora da regra de caminho e entra na cerca de schema se for registrado sob uma label elegível. `bin/codex_pergunta.sh:162–175,202–224` · `bin/tests/test_codex_pergunta.sh:153–162,409–412,658` · `app/core/tests/test_model_sem_migration_pendente.py:24–48` |

## `DRIFT=0` e diagnóstico

**Pelos caminhos de falha perguntados, o portão fecha.** A única atribuição de `DRIFT=0` fica no ramo bem-sucedido de `timeout -k 10 180 docker exec … shell -c`. A decisão descarta stdout e stderr e roda sem pipeline. Ausência de `core` levanta `SystemExit(1)`. Falha de import, falha do Docker ou timeout que volte com status não zero cai em `DRIFT=1`. O Bash confia integralmente no status externo. Se o processo acabar prematuramente com status zero, por exemplo por um `SystemExit(0)` antes da chamada, ou se um executor falso devolver zero, o Bash aceita. Um caminho desse tipo nos imports atuais da casca [nao verificado]. `app/manage.py:8–18` · `app/core/tests/test_model_sem_migration_pendente.py:34–48` · `bin/deploy.sh:194–201` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–103`

O script declara `set -u`. Num modo de publicação com `DRIFT=1`, a segunda chamada executa com `verbosity=1` e `stdout=sys.stdout`. Ela imprime `labels: …` e a saída do Django, como as operações propostas, ou `labels da casa sem core: …`. Erros do import e do Docker podem aparecer em stderr. O Bash junta os fluxos, retém as últimas 20 linhas e indenta. Essa segunda resposta fica só no diagnóstico, mesmo se voltar verde. Com `errexit` e `pipefail` juntos, o pipeline pode encerrar o script antes da mensagem de `falhou`, ainda com recusa. `--conferir` imprime `DRIFT=1`, pula a reconsulta e pode sair com rc 0 ao chegar ao fim do modo. `bin/deploy.sh:52,92,194–206,260–262` · `app/core/tests/test_model_sem_migration_pendente.py:40–48` · `app/tenants/test_model_sem_migration_pendente.py:34–40`

**Quem lê.** O cron encaminha a saída para `logs/reload_diario.log` e seu envelope registra o rc. A cadeia captura a saída em `reload_copia.out` e `reload_desfeito.out`, mostra o fim do arquivo e testa o rc na ida. O integrador captura ambos os fluxos. Ele descarta o rc do reload preliminar e testa o do deploy final antes de marcar a fatia no ar. No selo, 25 e 27 leem a mensagem de recusa em `SAIDA`. `app/config/crons.py:20,326–327,1900–1907` · `bin/cron_run.sh:11–13` · `bin/molde_fatia/cadeia.sh:32–33,48–49` · `app/core/integrador_lote.py:242–248,638–639,753–765` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123–126,245–255`

## Universo de labels

- **`ci`** herda os 12 apps da casa de `base`, sem `api` e `tenants`. A régua seleciona testes de `api`, mas isso não instala o app no registro. **`teste_motor`** herda `saas`, instala os 14 apps da casa e é usado pelo pre-push para `tenants`. **`saas`** reúne `SHARED_APPS` e `TENANT_APPS`, com `tenants` no primeiro e `api` no segundo. `manage.py` usa `saas` como default e preserva `DJANGO_SETTINGS_MODULE` já definido. O deploy não passa `--settings`. Um ambiente com `ci` pode completar a pergunta com `core` e deixar `api` de fora. Valor efetivo em produção [nao verificado]. `app/config/settings/base.py:4–30` · `app/config/settings/ci.py:1` · `app/config/settings/saas.py:22–53` · `app/config/settings/teste_motor.py:5–13` · `bin/regua.sh:23,165–169` · `bin/pre-push.sh:198–199` · `app/manage.py:9` · `bin/deploy.sh:194–196`
- **Sem `models.py` ou fora de `BASE_DIR`.** A presença de `models.py` não participa do filtro. A função seleciona todo `AppConfig` instalado cujo `path` textual começa com `BASE_DIR/`. Um app fora desse prefixo pode ficar de fora enquanto `core` continua na lista. Um model que nunca foi importado nem registrado fica invisível mesmo com a label selecionada. `mensageria/nucleo` pertence a outro settings e tem, no deploy, um `migrate --check` separado para migrations já existentes. `app/core/tests/test_model_sem_migration_pendente.py:24–48` · `app/tenants/test_model_sem_migration_pendente.py:27–38` · `mensageria/config/settings.py:1–10` · `bin/deploy.sh:213–225`
- **`api` sem pacote `migrations`.** Com a label explícita e um model concreto registrado, a cerca espera que o Django proponha a migration inicial e saia 1. O teste registra `CercaMordeApi` sob `Meta.app_label='api'` e exige `Create model CercaMordeApi`. A asserção demonstra o contrato que o teste cobra. Sua execução com Django real neste hash e a passagem desse RED pela função nova seguem [nao verificado]. `app/core/tests/test_model_sem_migration_pendente.py:24–29,40–48` · `app/tenants/test_model_sem_migration_pendente.py:25–40`

## Mordida dos casos 25–28

| Caso | Mutante que a asserção pega |
|---|---|
| **25** | Ignorar drift no deploy normal ou consultar só depois do apply. Há migration existente pendente e o caso exige recusa, zero migrate, zero HUP e mensagem de drift. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:244–247` |
| **26** | Pular o portão em `--sem-migrate`. O caso injeta drift nesse modo e exige recusa, zero migrate e zero HUP. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:249–251` |
| **27** | Voltar ao `makemigrations --check` direto, sem a pergunta da casa. O ramo direto do falso ignora `FAKE_DRIFT_API`, então o mutante passa pelo portão e a asserção falha. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–97,253–255` |
| **28** | Dar verde por `No changes detected` apesar de rc 1. O falso imprime essa linha antes de falhar, e a asserção exige recusa. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:91–96,257–259` |

**Mutantes que sobrevivem.** O falso aceita qualquer linha que contenha `pergunta_sem_migration_pendente`, inclusive uma chamada quebrada que só mencione o nome. Os quatro casos injetam o mesmo rc 1, então também passam com um portão que barra exclusivamente rc 1 e dá verde em 124, 125 ou 137. Omitir só `api` **dentro da função** escapa porque o falso não executa Python e o RED de `api` usa outra chamada. Pular o drift somente em `--reload-agendado` ou `--reload-copia` também fica fora dos casos de drift. Os casos 18 e 20 ainda recusariam pelo portão posterior de migrations existentes. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–103,216–226,244–259` · `app/tenants/test_model_sem_migration_pendente.py:33–35` · `bin/deploy.sh:207–237`

Em leitura, conferi a sintaxe Bash e Python e executei em memória o trecho do portão com o corpo do Docker falso. O original recusou rc 1, 124, 125 e 137. Os mutantes da chamada quebrada e da recusa exclusiva de rc 1 mantiveram os resultados esperados pelos casos 25–28 no trecho. A execução integral dos selos e o vínculo criptográfico da cópia ao hash, sem metadados Git disponíveis, seguem [nao verificado]. `bin/deploy.sh:194–205` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:66–97,244–259`

## L-126, escritores, leitores e classes

**A regra vigente converge.** ZONAS INVIOLÁVEIS e PARADA item 2 exigem o `!` específico do motor. CODEX item 2 e a L-126 dão a construção autorizada ao Codex, com Opus verificador único, DIFF da competência na sombra, pista, pouso e HOM. A L-126 agora conserva esse papel após a segunda reprovação. A redação divergente permanece em `PISTA.md`: só leitura, proibição de escrever e construção de negócio pelo Opus. O cabeçalho da própria PISTA dá precedência ao CODEX consolidado. `RELATO.md:3410` documenta a leitura antiga num episódio anterior. `aval.txt:1` · `CLAUDE.md:145–146,274–277,398–406` · `app/docs/LEIS.md:147` · `app/docs/PISTA.md:8–14,80–99,112,143–152`

| Função ou valor | Quem escreve e quem lê |
|---|---|
| `BASE_DIR`, registro de apps e `labels_locais()` | `base.py` define a raiz, os settings declaram os apps e o Django fornece o registro. `labels_locais()` produz a lista ordenada. Leem a função a pergunta nova, a asserção de `checa`, os REDs de campo e o RED de `api`. `app/config/settings/base.py:4–30` · `app/config/settings/saas.py:22–53` · `app/core/tests/test_model_sem_migration_pendente.py:24–66` · `app/tenants/test_model_sem_migration_pendente.py:25–35` |
| `pergunta_sem_migration_pendente`, `verbosity`, `stdout` e status | As cercas de core e tenants chamam `checa`, que chama a pergunta com os defaults. O deploy declara `PERGUNTA_MIGR` e `PERGUNTA_MIGR_V` e as entrega a `shell -c`. A pergunta passa as labels e as opções ao Django, que responde pelo resultado do check. O Bash decide e escreve `DRIFT`, lido pelo `echo` e pela guarda de recusa. `app/core/tests/test_model_sem_migration_pendente.py:34–53,73–80` · `app/tenants/test_model_sem_migration_pendente.py:15–23` · `bin/deploy.sh:194–206` |
| Fixtures e contadores do selo | `roda` fornece as flags `FAKE_*` ao processo do teste. O Docker falso lê as flags e produz status, stdout e chamadas registradas. `roda` captura `SAIDA` e `RC` e extrai `N_MIG` e `N_HUP`. As asserções leem esses valores. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:82–109,119–126,244–259` |
| Permissão de construção e trilha | `PROIBIDOS` nasce no wrapper e é lido pelo inventário na entrada, revalidação e saída. `proibidos_do_wrapper()` lê as quatro regex selecionadas por `OPUS_CONSTROI_TAGS`. `agente_nega()` as aplica ao brief. O wrapper escreve `chamadas.log` e `codex_limite_estado.py` o lê. Chamadas reais da obra e teto Opus [nao verificado]. `bin/codex_pergunta.sh:162–175,224,541,560–570,608,761,806` · `bin/disjuntor.py:1961,2023–2039,2102–2113` · `bin/codex_limite_estado.py:46–57` |

- **Segundo juiz e derivação paralela:** o Django continua sendo a autoridade de schema. A c9 elimina o recorte de stdout no Bash. A asserção em `checa` repete a leitura do mesmo registro. Já os REDs repetem a **invocação** de `makemigrations` fora da pergunta nova, o que explica a lacuna de cobertura. `app/core/tests/test_model_sem_migration_pendente.py:24–70` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/deploy.sh:194–200`
- **Segundo escritor fora da porta:** o delta acrescenta consulta com `--dry-run`. As mutações de campo e do registro de models são de teste e têm limpeza em `finally`. O ponto procedente de admissão é o brief misto do Opus. `app/core/tests/test_model_sem_migration_pendente.py:48,60–68` · `app/tenants/test_model_sem_migration_pendente.py:28–38` · `bin/disjuntor.py:2102–2113`
- **Relógio, ordem, semântica e `except`:** os timeouts ficam na borda Bash e o horário continua no cron. O drift é consultado antes de migrate, restart e HUP. `DRIFT=0` tem uma única atribuição após sucesso. `DRIFT=1` agrupa divergência e falha de consulta de forma explícita. O delta não introduz `except` na pergunta. O bloqueio remanescente de **selo/RED sem mordida** está no caminho real da função, como mostram os mutantes acima. `app/config/crons.py:326–327` · `bin/deploy.sh:194–205,338–339,361–382` · `app/core/tests/test_model_sem_migration_pendente.py:34–48` · `app/tenants/test_model_sem_migration_pendente.py:33–40`

## 26/11 23:56 · sessao f9374a425f1a · O30-F3-CONSTRUIR · verificador · origem=Code

**REPROVADO — O30-F3-CONSTRUIR, volta 11.** A junção deixou uma única rota de lote. Dois achados sustentam a reprovação: a edição comum pode sobrescrever os seis campos sem passar pela gravação auditada, e a rota reunificada perdeu o filtro universal e a seleção de colaborador previstos para a F3. O caso pontual de C foi corrigido, mas o selo ainda aceita a perda da prova de persistência. D fica na lista pelo aval. `app/relatorios/urls.py:16`, `app/ponto/services/ausencia.py:311`, `app/ponto/services/ausencia.py:757`, `app/docs/BACKLOG.md:2384`, `app/relatorios/views.py:352`, `app/ponto/tests/test_atestado_f3_contrato.py:936`, `app/docs/PROMPTS.md:3103`

Conferi os 33 blobs de destino do `diff.diff` contra a cópia. Todos coincidem, e o teste removido está ausente. A identidade do commit e a ancestralidade em relação a `59600163c` ficam [nao verificado]. `diff.diff:1`, `app/ponto/urls.py:73`, `app/templates/relatorios/atestados_lote.html:47`, `bin/sombra_regras_pessoais.py:167`

### 1. Aval e junção do lote

- **Entregue no código.** A rota do main serve prévia, CSV, PDF e ZIP sobre o mesmo recorte. O PDF embute JPEG/PNG e dá link protegido para PDF, HEIC e demais formatos. A seção «Sem documento» usa a mesma lista da prévia. Os seis campos entram no formulário e nas exportações PDF/CSV. O mapa curto está registrado. `app/relatorios/views.py:382`, `app/ponto/services/atestados_lote.py:64`, `app/ponto/services/atestados_lote.py:119`, `app/ponto/services/atestados_lote.py:159`, `app/ponto/services/atestados_lote.py:203`, `app/ponto/services/atestados_lote.py:217`, `app/templates/ponto/partials/painel_ausencia.html:95`, `app/docs/RELATO.md:13589`
- **Falta parte do aval.** O backlog da F3 pede filtro universal, período, colaborador e empresa. A tela reunificada oferece empresa, período e status binário (`Aprovadas`/`Todos`). Em memória, `q` e `colaborador` não alteraram a seleção. O tipo também fica fixado em `TIPOS_MEDICOS`, embora o desenho original previsse seleção de tipo. A prévia ficou na página do relatório, onde o desenho original previa um drawer. A junção autoriza preservar a porta do main. Uma dispensa explícita desses requisitos fica [nao verificado]. `app/docs/BACKLOG.md:1445`, `app/docs/BACKLOG.md:2384`, `app/docs/RELATO.md:13620`, `app/relatorios/views.py:352`, `app/relatorios/views.py:382`, `app/templates/relatorios/atestados_lote.html:1`
- **L-111.** A busca na cópia encontrou uma view, uma rota e um template de lote. Os dois atalhos apontam para `relatorios:atestados_lote`. `lote_atestados` aparece nos testes de remoção e no mutante que o reintroduz. O POST `anexar_atestado` é a porta do formulário individual. A antiga função de anexo, sua rota e seu pendente têm lápide. `app/relatorios/views.py:346`, `app/relatorios/urls.py:16`, `app/templates/relatorios/atestados_lote.html:9`, `app/templates/ponto/ausencias.html:42`, `app/templates/relatorios/atestados.html:12`, `app/relatorios/tests/test_atestados_lote_contrato.py:86`, `app/ponto/tests/mutantes_atestado_f3.py:159`, `app/ponto/urls.py:73`, `app/ponto/views.py:2165`, `app/core/juizes.py:1098`, `app/docs/LEIS.md:137`

### 2. Achados C e D

| Achado | Resultado |
|---|---|
| **C — caso pontual corrigido, selo parcial** | A fixture usa `aguardando_documento`, relê a linha e confirma estado e ausência de arquivo. O caso aprovado envia o POST ao `action` extraído do HTML, verifica o `302`, relê `aprovada` e confere os bytes. Os três mutantes da v10 caíram por asserção. Em duas alterações adicionais em memória, o selo passou com `pendente` em outra fixture da F3 e passou quando mantive o POST e retirei as verificações de persistência. Os dois testes vizinhos com `pendente` continuam na cópia. `app/ponto/models.py:690`, `app/ponto/tests/test_atestado_f3.py:238`, `app/ponto/tests/test_atestado_f3.py:257`, `app/ponto/tests/test_atestado_f3.py:294`, `app/ponto/tests/test_atestado_f3_contrato.py:947`, `app/ponto/tests/test_atestado_f3_contrato.py:966`, `app/ponto/tests/test_anexar_ui_colab.py:34`, `app/ponto/tests/test_gate_anexo_desconta.py:23` |
| **D — dois `xfail` nomeados, caminhos da v10 preservados** | A seleção explícita deu duas falhas esperadas. Desativei os marcadores em memória e obtive um `FAIL`, zero `ERROR`, na asserção final de cada caso. No primeiro, `criar_ausencia` já retornou quando o chamador desfaz a linha, e o arquivo sobra. No segundo, a referência aparece entre `exists()` e `storage.delete()`, e o arquivo é apagado. O aval deixa ambos na lista e registra a retenção futura. Ocorrência em produção [nao verificado]. `app/ponto/tests/mutantes_atestado_f3_c14.py:98`, `app/ponto/tests/mutantes_atestado_f3_c14.py:129`, `app/ponto/tests/mutantes_atestado_f3_c14.py:131`, `app/ponto/tests/mutantes_atestado_f3_c14.py:151`, `app/ponto/services/ausencia.py:263`, `app/ponto/services/ausencia.py:668`, `app/docs/PROMPTS.md:3103` |

**c19.** O selo normaliza os espaços do texto extraído antes de conferir os seis valores. O exportador segue emitindo os mesmos dados. A execução desse selo com PDF real fica [nao verificado]. `app/relatorios/tests/test_atestados_lote_tela.py:229`, `app/ponto/services/atestados_lote.py:203`

### 3. Quem escreve e quem lê

Os seis campos têm dois pontos explícitos de persistência no serviço. `criar_ausencia` inclui os valores validados no INSERT. `_gravar_dados_atestado` relê os seis campos e a referência sob trava, faz o UPDATE e grava trilha obrigatória. Chegam ao helper `anexar_documento`, `editar_ausencia_registro` e `_reabrir`. O POST do painel passa apenas as chaves presentes. Os formulários comuns de lançar e editar ausência passam os campos que já tratavam. `app/ponto/services/ausencia.py:288`, `app/ponto/services/ausencia.py:311`, `app/ponto/services/ausencia.py:421`, `app/ponto/services/ausencia.py:525`, `app/ponto/services/ausencia.py:581`, `app/ponto/services/ausencia.py:650`, `app/ponto/services/ausencia.py:757`, `app/ponto/services/ausencia.py:1129`, `app/ponto/services/ausencia.py:1428`, `app/ponto/views_atestado.py:31`, `app/ponto/views_atestado.py:43`, `app/ponto/views.py:2550`, `app/ponto/views.py:2949`

| Campo | Migration, modelo e leitores |
|---|---|
| `atestado_medico` | Texto inicial vazio. O painel lê na linha e PDF/CSV leem a mesma coluna. A sombra aplica a regra `TEXTO`. `app/ponto/migrations/0079_ausencia_atestado_dados.py:13`, `app/ponto/models.py:787`, `app/templates/ponto/partials/painel_ausencia.html:100`, `app/ponto/services/atestados_lote.py:102`, `bin/sombra_regras_pessoais.py:112` |
| `atestado_crm` | Texto inicial vazio. Painel, PDF e CSV leem a coluna. A sombra grava vazio. `app/ponto/migrations/0079_ausencia_atestado_dados.py:15`, `app/ponto/models.py:789`, `app/templates/ponto/partials/painel_ausencia.html:103`, `app/ponto/services/atestados_lote.py:102`, `bin/sombra_regras_pessoais.py:56` |
| `atestado_crm_uf` | Texto inicial vazio. O formulário valida duas letras maiúsculas. Painel, PDF e CSV leem a coluna. A sombra grava vazio. `app/ponto/migrations/0079_ausencia_atestado_dados.py:17`, `app/ponto/models.py:790`, `app/ponto/forms.py:16`, `app/templates/ponto/partials/painel_ausencia.html:106`, `app/ponto/services/atestados_lote.py:102`, `bin/sombra_regras_pessoais.py:56` |
| `atestado_cid` | Texto inicial vazio. Painel, PDF e CSV leem a coluna. A sombra grava vazio. O admin Django omite os seis campos. `app/ponto/migrations/0079_ausencia_atestado_dados.py:19`, `app/ponto/models.py:791`, `app/templates/ponto/partials/painel_ausencia.html:109`, `app/ponto/services/atestados_lote.py:102`, `bin/sombra_regras_pessoais.py:56`, `app/ponto/admin.py:50` |
| `atestado_emissao` | Data inicial `NULL`. O painel formata para o input de data. PDF e CSV formatam a coluna como `dd/mm/aaaa`. A sombra grava `NULL`. `app/ponto/migrations/0079_ausencia_atestado_dados.py:21`, `app/ponto/models.py:793`, `app/templates/ponto/partials/painel_ausencia.html:112`, `app/ponto/services/atestados_lote.py:104`, `bin/sombra_regras_pessoais.py:80` |
| `atestado_dias` | Inteiro inicial `NULL`, aceita zero. Painel, PDF e CSV leem os dias declarados no papel. A duração do lançamento vem de `data`, `data_fim` e `dias_corridos`. A sombra grava `NULL`. `app/ponto/migrations/0079_ausencia_atestado_dados.py:23`, `app/ponto/models.py:794`, `app/ponto/forms.py:22`, `app/templates/ponto/partials/painel_ausencia.html:115`, `app/ponto/services/atestados_lote.py:203`, `app/ponto/services/dia_decidido.py:678`, `bin/sombra_regras_pessoais.py:80` |

**O escritor único efetivo falha.** Quando a edição comum omite os campos médicos, `_gravar_dados_atestado` retorna cedo e a edição faz `ausencia.save()` integral. Simulei a chamada real do serviço com uma instância antiga e um CID mais novo no registro persistido. A edição repôs o CID antigo sem trilha dos seis. `rederivar_fim` também faz `save()` integral. O censo de menções declara que deixa essa classe fora. Na sombra, a escrita deliberada é do mascarador, por `update` e `bulk_update`, e o selo lê o resultado. `app/ponto/services/ausencia.py:311`, `app/ponto/services/ausencia.py:757`, `app/ponto/services/ausencia.py:1880`, `app/ponto/models.py:932`, `app/ponto/tests/test_atestado_f3_contrato.py:176`, `bin/sombra_mascara_pessoal.py:16`, `bin/sombra_mascara_pessoal.py:44`, `bin/sombra_selo_pessoal.py:37`

| Demais valores do diff | Origem, escrita e leitura |
|---|---|
| `empresa`, `mes`, `ano`, `de`, `ate`, `status`, `formato` | GET → view. A empresa vem de `empresas_visiveis`, a janela explícita de `janela_fechamento`, e tipos médicos do catálogo vigente. A consulta lê colaborador, tipo, status, vigência e `documento_referencia` nula. A view escreve apenas a resposta e seu nome de download. `app/relatorios/views.py:352`, `app/relatorios/views.py:382`, `app/relatorios/views.py:400`, `app/colaboradores/queries.py:78`, `app/ponto/catalogo/ausencias.py:467`, `app/ponto/services/atestados_lote.py:20`, `app/ponto/services/atestados_lote.py:35`, `app/ponto/services/dia_decidido.py:624` |
| Matrícula, nome, razão social, datas, dias, tipo, rótulo, status, minutos, presença e exigência de documento, decisor e instante | Cadastro do colaborador/empresa e linha gravada de `Ausencia`. O histórico fornece dias, rótulo, status, minutos, presença, `quem` e `quando`. A exigência consulta `estado_inicial`. Prévia, CSV e PDF leem esses valores. O ZIP lê matrícula ou PK, data, tipo, nome e bytes do anexo. `app/ponto/services/dia_decidido.py:558`, `app/ponto/services/dia_decidido.py:659`, `app/ponto/services/atestados_lote.py:14`, `app/ponto/services/atestados_lote.py:71`, `app/ponto/services/atestados_lote.py:83`, `app/ponto/services/atestados_lote.py:121`, `app/ponto/services/atestados_lote.py:188`, `app/ponto/services/atestados_lote.py:247` |
| `documento`, `status`, `prazo_documento`, `descricao`, `situacao` e compensação | A criação grava documento, estado inicial, prazo e descrição. Anexo e reabertura escrevem a chave do arquivo. `mudar_estado` grava status e limpa prazo conforme a transição. A reabertura pode acrescentar descrição e renovar prazo. O INSS escreve `situacao` pela porta do colaborador. A compensação lê `_committed`, nome, storage, banco e presença de referência antes de decidir a exclusão no ramo explícito. `app/ponto/services/ausencia.py:567`, `app/ponto/services/ausencia.py:638`, `app/ponto/services/ausencia.py:1075`, `app/ponto/services/ausencia.py:1133`, `app/ponto/services/ausencia.py:1430`, `app/ponto/services/ausencia.py:250`, `app/ponto/views.py:2566` |
| Trilha, recusa e projeção | `core.observ.evento` grava ator, IP, ação, objeto, descrição e antes/depois. Para os seis campos, a trilha leva os nomes das chaves. O anexo registra o nome anterior e o novo. A recusa usa o registrador de porta crítica. A projeção compara o insumo da folha e pode invalidar o carimbo de `FechamentoMensal`. `app/core/observ.py:111`, `app/core/observ.py:136`, `app/ponto/services/ausencia.py:339`, `app/ponto/services/ausencia.py:1140`, `app/ponto/views_atestado.py:34`, `app/core/recusa_critica.py:70`, `app/ponto/services/ausencia.py:128`, `app/ponto/services/fechamento.py:1676` |

### 4. Contraexemplos e classes da régua

- **Fonte única, com divergência na filha legada.** O histórico F2 e o lote chamam `linha_do_tempo`. PDF e CSV leem os seis atributos da própria raiz selecionada, que são os mesmos exibidos ao abrir essa raiz no painel. Cobertura, exigência de documento e relógio do lote têm as autoridades indicadas. Na filha legada, porém, o painel exibe o anexo da raiz e oferece «Substituir anexo», enquanto o POST grava o arquivo na filha. O teste confirma a gravação na filha. Reabrir o painel, baixar pela rota protegida ou exportar a raiz continua mostrando o arquivo anterior. `app/ponto/views.py:2042`, `app/ponto/services/atestados_lote.py:35`, `app/ponto/services/atestados_lote.py:56`, `app/ponto/services/atestados_lote.py:102`, `app/ponto/models.py:723`, `app/core/juizes.py:264`, `app/templates/ponto/partials/painel_ausencia.html:71`, `app/templates/ponto/partials/painel_ausencia.html:89`, `app/ponto/views.py:2146`, `app/ponto/tests/test_atestado_f3.py:169`, `app/ponto/services/dia_decidido.py:628`
- **Estado e ordem de trava.** O INSS trava o colaborador antes de gravar a ausência. O UPDATE dos seis trava `Ausencia`, mas relê apenas a referência e os campos médicos. Em memória, um POST que havia carregado `aguardando_documento` antes de uma rejeição concorrente gravou `aguardando_decisao` por cima da rejeição. A trava dos seis deixa a decisão de estado apoiada na instância antiga. Uma inversão de ordem ou um deadlock em banco fica [nao verificado]. `app/ponto/views.py:2547`, `app/colaboradores/services/colaborador.py:84`, `app/ponto/services/ausencia.py:318`, `app/ponto/services/ausencia.py:1068`, `app/ponto/services/ausencia.py:1144`
- **Formato.** JPEG/PNG com decode recusado pelo tratador viram `ANEXO ILEGIVEL` e incrementam o rodapé. Se `storage.exists()` responder verdadeiro e a abertura seguinte falhar, o erro sai antes do tratador de imagem e interrompe o PDF. Arquivo de bytes desconhecidos ainda ganha link. No ZIP, um HEIC legado salvo como `.jpg` sai com os bytes originais e a extensão `.jpg`, pois o nome vem da chave gravada. O tipo real rege o link de download e a normalização de novos uploads. `app/ponto/services/atestados_lote.py:91`, `app/ponto/services/atestados_lote.py:145`, `app/ponto/services/atestados_lote.py:166`, `app/ponto/services/atestados_lote.py:208`, `app/ponto/services/atestados_lote.py:226`, `app/ponto/services/atestados_lote.py:258`, `app/ponto/services/ausencia.py:833`, `app/ponto/views.py:2153`
- **Permissão.** O superusuário passa pelo RBAC. O lote exige `ver_relatorios`, `ver_folha` e `ver_ausencias`. O POST individual exige `ver_ausencias` e `editar_ausencias`. O colaborador comum tem a API da própria ausência aberta, que recebe apenas o documento. Dentro do mesmo tenant, a empresa do setor do operador não limita essas ações: um DP com as permissões alcança outra empresa ativa e visível pelo lote, e painel, download e POST individual buscam a ausência por PK. Isolamento entre tenants e as permissões efetivas em produção [nao verificado]. `app/colaboradores/mixins.py:322`, `app/colaboradores/mixins.py:364`, `app/relatorios/views.py:20`, `app/relatorios/views.py:345`, `app/colaboradores/queries.py:78`, `app/ponto/views_atestado.py:19`, `app/ponto/views.py:2098`, `app/ponto/views.py:2144`, `app/api/views.py:3094`, `app/api/views.py:3140`
- **Competência.** Em memória, com corte 12 e data 25/09/2026, o filtro vazio devolveu 12/08–11/09. `janela_atual` devolveu 12/09–11/10. Um intervalo livre invertido também chegou à consulta, onde uma ausência longa pode satisfazer os dois limites. Os dois desvios já estavam na lista da mínima. O relógio agora é lido uma vez na view. A competência entregue pode ser consultada e exportada pelo relatório, que lê o cadastro atual. A edição apenas médica grava os campos e a trilha, sem acionar o juiz K6 ou reescrever o TXT. Estoque já exportado em produção [nao verificado]. `app/relatorios/views.py:370`, `app/ponto/turnos.py:2842`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`, `app/docs/RELATO.md:13666`, `app/ponto/services/ausencia.py:1129`, `app/folha/export.py:565`
- **`except` no diff inteiro.** **1 novo**, em `_foto_no_pdf`, cobrindo abertura, carregamento e transposição da foto, conforme QUATRO-PAREIS. **2 removidos**, um da antiga view de anexo e um de `_reabrir`. **Saldo −1.** O `except` de `anexar_documento` já existia e passou a circundar um bloco maior. Os dois do ZIP são contexto, e o do mutante é texto. **`bin/`: 0 novos e 0 removidos.** `app/docs/PROMPTS.md:3078`, `app/ponto/services/atestados_lote.py:145`, `app/ponto/services/atestados_lote.py:261`, `app/ponto/services/ausencia.py:1147`, `diff.diff:1`, `app/ponto/tests/mutantes_atestado_f3.py:119`, `bin/sombra_regras_pessoais.py:55`
- **Selos e pré-requisito de pouso.** Rodei 70 testes sem banco: 67 passaram, 2 `xfail` de D e 1 erro por falta de `PIL`. Nos dez mutantes da junção, os controles passaram, nove mutantes caíram por asserção e um por `ValueError` no caso de mês inválido. O registro da mínima documenta o pouso anterior e sua suíte. Suíte integral, integração HTTP real, migration aplicada e certificado de pouso deste hash [nao verificado]. `app/ponto/tests/test_atestado_f3_contrato.py:1067`, `app/ponto/tests/mutantes_atestado_f3.py:132`, `app/relatorios/tests/test_atestados_lote_contrato.py:76`, `app/ponto/services/atestados_lote.py:29`, `app/docs/RELATO.md:13683`, `CLAUDE.md:129`

**Lista do veredito.** Bloqueiam a perda de atualização dos campos auditados, a transição sobre estado obsoleto e os filtros do aval ausentes. Ficam registrados C parcial, D autorizado em lista, o anexo invisível da filha, as bordas de arquivo e período, e o alcance entre empresas dentro do tenant. A prova integral de pouso deste hash fica [nao verificado]. `app/ponto/services/ausencia.py:757`, `app/ponto/services/ausencia.py:1144`, `app/docs/BACKLOG.md:2384`, `app/ponto/tests/test_atestado_f3_contrato.py:966`, `app/docs/PROMPTS.md:3103`, `app/ponto/tests/test_atestado_f3.py:169`, `app/ponto/services/atestados_lote.py:167`, `app/docs/RELATO.md:13666`, `app/relatorios/views.py:352`

## 26/11 23:52 · sessao 8a6a096f36d6 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

**REPROVADO — `a32fe226f`.** O deploy ainda pode dar `DRIFT=0` com uma lista incompleta de apps. Os selos 27 e 28 não cobrem esse caso. A L-126 ainda dá dois destinos para uma obra de motor reprovada duas vezes. O verde dos selos no hash e a execução do portão na casca real seguem [nao verificado]. [bin/deploy.sh:191](/home/ronald/codex_ws/a32fe226fc7d61dc356a7a510739fc993e6ef894/bin/deploy.sh:191) · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:248` · `app/docs/LEIS.md:147`

## 1. Os cinco achados do Opus

| Achado | Cura na origem e contraexemplo |
|---|---|
| **1. Schema sem label** | **Parcial.** O registro de apps do Django agora alimenta as duas cercas e o deploy. O teste de `tenants` registra `CercaMordeApi` sob `api` e exige rc 1 com `Create model CercaMordeApi`. No deploy, se a última linha do shell for só `core`, a consulta pode sair verde com um model novo e independente em `api`. `app/core/tests/test_model_sem_migration_pendente.py:24–37` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/deploy.sh:191–200` |
| **2. Selos de host sem verde no hash** | **Em aberto.** Há casos 25–28 no selo de deploy e um mutante de `p_models_pkg` no selo do wrapper. O selo do agente cobra a leitura das quatro tags e a recusa de Opus para `models.py` e motor. Um verde anterior à mudança ainda poderia ser apresentado como prova do hash atual. RED/GREEN dos 25–28, verde do wrapper e verde do agente vinculados a `a32fe226f` [nao verificado]. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:239–257` · `bin/tests/test_codex_pergunta.sh:658` · `bin/tests/test_agente_no_modelo.sh:127–143,286–288` |
| **3. Portão na produção** | **Em aberto.** O código alcança o portão no `--reload-agendado` das 03:30 e no `--reload-copia`, inclusive na chamada de desfazer a cópia. Os modos 18 e 20 do selo simulam migration pendente, e os casos 27 e 28 executam o modo normal com Docker falso. Execução do portão novo com Django e settings efetivos da casca real [nao verificado]. `app/config/crons.py:326` · `bin/molde_fatia/cadeia.sh:32,48` · `bin/deploy.sh:63–82,191–208` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:81–98,211–221,248–254` |
| **4. Aval e papel do construtor** | **Parcial.** LEIS e ZONAS INVIOLÁVEIS agora dizem que, dado o `!` da obra, Codex constrói o motor e Opus verifica. A mesma L-126 ainda manda entregar a construção ao Opus na segunda reprovação, sem ressalvar motor ou `models.py`. Numa segunda reprovação de uma obra só de motor, a lei manda trocar o construtor e o disjuntor recusa esse despacho. `app/docs/LEIS.md:147` · `CLAUDE.md:146,274–277,402–406` · `bin/disjuntor.py:1961,2102–2112` |
| **5. Pacote `models/` liberado** | **Curado no inventário de caminhos.** A regex barra descendentes de `models/`, inclusive a criação de uma pasta vazia, que o inventário registra com `/` no fim. O selo cria `folha/models/__init__.py`, exige rc 3 com o motivo e tem mutante que retira a guarda. Um model declarado num módulo de outro nome e importado por `models.py` fica fora dessa proibição de caminho. `bin/codex_pergunta.sh:162–175,202–224,231–233` · `bin/tests/test_codex_pergunta.sh:154–162,409–412,658` |

## 2. Alcance da cerca de schema

- **`ci`** herda a lista de `base` e tem 12 apps da casa instalados. `api` aparece na seleção de **testes** da régua, mas fica fora do registro de apps de `ci`. **`teste_motor`** herda `saas` e acrescenta ao universo de `ci` `api` e `tenants`. O pre-push seleciona a suíte `tenants` nesse settings, e o teste pergunta por todas as labels locais instaladas. **`saas`** é o default declarado por `manage.py` e WSGI. O deploy não fixa `--settings`, então o ambiente da casca pode determinar outro valor. Valor efetivo em produção [nao verificado]. `app/config/settings/base.py:4–30` · `app/config/settings/ci.py:1` · `bin/regua.sh:23,168–169` · `app/config/settings/teste_motor.py:5–13` · `app/config/settings/saas.py:22–53` · `bin/pre-push.sh:198–199` · `app/manage.py:9` · `app/config/wsgi.py:14` · `bin/deploy.sh:191–197`
- **Apps omitidos.** Entre os apps declarados nesses três settings, não encontrei app da casa instalado que fique fora pelo caminho. A regra compara o prefixo textual de `AppConfig.path` com `BASE_DIR/`. Um app da casa instalado com `path` fora desse prefixo fica de fora. Há um caso concreto de outro projeto da casa, `mensageria/nucleo`, instalado no settings próprio. Seu portão no deploy checa migrations **já existentes** pendentes de aplicação. `app/core/tests/test_model_sem_migration_pendente.py:24–31` · `app/config/settings/base.py:4–30` · `app/config/settings/saas.py:22–53` · `mensageria/config/settings.py:2–10` · `mensageria/nucleo/models.py:4` · `bin/deploy.sh:215–227`
- **`Meta.app_label` e `MIGRATION_MODULES`.** Se o model foi importado e a label de destino é local e instalada, a declaração em outro módulo continua no universo. O teste de `api` cobre exatamente o registro explícito. Uma label não instalada ou excluída pelo caminho fica fora da lista. Para um model gerenciado com `MIGRATION_MODULES[app] = None`, o comportamento esperado com label explícita é propor a migration inicial, conforme o comentário da cerca. Esse caso específico segue [nao verificado]. Um pacote alternativo de migrations pode deixar o estado em dia, enquanto o inventário só veda o componente de caminho literal `migrations`. `app/core/tests/test_model_sem_migration_pendente.py:24–37` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/codex_pergunta.sh:162–175`
- **Abstrato, proxy e unmanaged.** A pergunta enviada é sobre estado de migrations. Uma base abstrata sem descendente concreto não tem operação própria. Um proxy ou unmanaged novo pode gerar migration de estado sem criar tabela. Um contraexemplo é acrescentar campo a um unmanaged já existente, cuja tabela externa a cerca não inspeciona. Esses casos de autodetecção seguem [nao verificado] no Django desta cópia. `app/core/tests/test_model_sem_migration_pendente.py:34–54` · `app/requirements.txt:13`

## 3. `DRIFT=0` e a mordida dos selos

| Cenário | Resultado do portão | Casos 27 e 28 |
|---|---|---|
| **Warning na última linha** | Sem ` core `, dá `1`. Com ` core `, a linha passa à próxima etapa e é dividida em argumentos. Prosa com labels inválidas tende a dar `1`; se a última linha contiver só um subconjunto válido, como `core`, pode dar `0` com drift em `api`. Como a expansão também aceita opções, uma saída que inclua uma opção de saída antecipada como `--version` pode terminar sem autodetecção [nao verificado] no Django da casca. `bin/deploy.sh:191–200` | **Nenhum dos dois** simula stdout com prosa, lista parcial ou opção. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–92,248–254` |
| **Timeout** | Na consulta de labels, o status do primeiro comando se perde atrás de `tail` e a atribuição segue sem teste de rc. Se já saiu `core`, um segundo comando bem-sucedido pode dar `0`. Sem stdout útil, dá `1`. Timeout no próprio `makemigrations` dá `1`. `bin/deploy.sh:52,191–200` | O 28 cobre apenas o efeito de **stdout vazio**, com rc **0** na consulta. Nenhum simula timeout ou saída antes da falha. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90–92,252–254` |
| **Casca fora** | Se a primeira chamada falha sem stdout, dá `1`. Se a casca cai entre as chamadas e o `makemigrations` falha, dá `1`. `--conferir` pode imprimir `DRIFT=1` e ainda terminar com rc 0, por desenho. `bin/deploy.sh:191–208,262–265` | O 28 cobre ausência de labels. Nenhum simula rc do Docker indisponível nem a queda entre chamadas. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:81–98,248–254` |

O **27** morde a perda de `api` na lista limpa do stub e exige recusa, zero migrate, zero HUP e o motivo. O **28** morde a aceitação de lista vazia e exige rc não zero, zero migrate e zero HUP. Rodei em memória o trecho do Bash com respostas simuladas. Lista parcial e timeout após imprimir `core` deram `DRIFT=0`. Timeout sem stdout, casca fora sem stdout e timeout do check deram `DRIFT=1`. Conferi também a sintaxe dos scripts e dos dois módulos Python, em leitura. Execução integral dos selos, Django real e produção [nao verificado]. `bin/deploy.sh:191–202` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:65–70,90–92,248–254`

## 4. Quem escreve e quem lê

- **`labels_locais()`** deriva a lista ordenada de `settings.BASE_DIR` e do registro de `AppConfig` do Django. Chamam diretamente `checa_sem_migration_pendente`, `morde_campo_sem_migration`, o teste de `api` duas vezes e o `shell -c` do deploy. As cercas normais de core e tenants chamam o mesmo `checa`. `app/core/tests/test_model_sem_migration_pendente.py:24–37,40–50,59–64` · `app/tenants/test_model_sem_migration_pendente.py:22–43` · `bin/deploy.sh:191–192`
- **`LABELS_CASA`** é escrita uma vez pelo Bash com a última linha do stdout. Leem a variável o `case`, o `makemigrations` que decide e a impressão e reconsulta de diagnóstico. `bin/deploy.sh:191–205`
- **`p_models_pkg`** é declarada em `PROIBIDOS` e aplicada por `re.search` aos caminhos tocados. O mesmo inventário roda no pre-check, na revalidação e no fim. O disjuntor extrai as regex do wrapper, mas seleciona quatro tags, sem `p_models_pkg`. Uma obra cujo único caminho é um pacote `models/` fica recusada nas duas portas de construtor. O inventário barra descendentes, inclusive `__init__.py`, e admite um `models.py` regular dentro de `app/`, com qualquer conteúdo que as demais cercas aceitem. Também admite módulos com outros nomes e um arquivo regular chamado `models` sem barra final. `bin/codex_pergunta.sh:162–175,209–233,541,761,806,815–823` · `bin/disjuntor.py:1961,2023–2039,2102–2112`

## 5. Terceiro sítio da regra antiga

PARADA item 2, seção 4 e a cláusula nova da L-126 concordam sobre o `!` específico para tocar o motor. O despacho registra a sequência do aval, incluindo DIFF na sombra antes de pista e pouso. **`PISTA.md` conserva o sentido antigo** em três passagens: proíbe Codex de escrever, reserva código de produção ao Code e atribui construção de negócio ao Opus. O próprio cabeçalho de `PISTA.md` dá precedência ao CODEX consolidado. `RELATO.md` ainda registra a leitura antiga de “item que pede `!`” num episódio anterior. `CLAUDE.md:146,274–277,402–406` · `app/docs/LEIS.md:147` · `app/docs/PISTA.md:8–14,83–99,143–152` · `app/docs/RELATO.md:3410`

## 6. Classes de reprovação no delta

- **Selo sem mordida e valor com dois significados.** `DRIFT=0` primeiro quer dizer “a última linha contém `core`” e depois “o check saiu 0”. A distinção deixa passar uma pergunta parcial, que os selos não exercitam. A dupla atribuição do papel do Opus na L-126 é a outra ambiguidade do delta. `bin/deploy.sh:193–202` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:248–254` · `app/docs/LEIS.md:147`
- **Derivação paralela.** As três cercas compartilham a função, e o shell reinterpreta a lista com um critério mais fraco. Já existiam outros critérios para “app da casa” em `describe` e no censo de logging, com diferenças de caminho. Nenhum deles é outra cerca de migrations. `bin/deploy.sh:191–197` · `app/core/tests/test_model_sem_migration_pendente.py:24–37` · `app/core/management/commands/describe.py:15–19,32–38` · `app/ponto/tests/test_logging_de_todo_app.py:26–36`
- **Segundo juiz, segundo escritor, relógio, ordem e `except`.** O delta mantém o autodetector do Django como juiz de schema. A escrita direta nova é no registro global de models **do teste**, com limpeza em `finally`. Os timeouts ficam na borda do deploy. O portão vem antes de migrate e HUP, sem inversão de trava nem novo `except` no delta. O falso verde permite avançar sem cumprir o pré-requisito de schema. Na cadeia, a ordem reload antes da régua já existia. Pouso efetivo com esse desvio [nao verificado]. `app/core/tests/test_model_sem_migration_pendente.py:34–37` · `app/tenants/test_model_sem_migration_pendente.py:25–40` · `bin/deploy.sh:191–208,340–341,384` · `bin/molde_fatia/cadeia.sh:48–55`

## 26/11 23:45 · sessao 579083d18fc0 · CODEX-CONSTROI-MOTOR · censo · origem=Code

**Quem perguntou.** A v2 pediu contraexemplos para qualquer mudança de schema, a v4 perguntou pela cobertura de todos os apps do deploy e a v5 pediu a conferência da L-126 cláusula por cláusula. As perguntas sobre os selos cobraram mordida e ordem. A v7 ficou restrita à troca do nome na L-126 e aos seus leitores. `voltas.txt/v2/P:9`, `voltas.txt/v4/P:12`, `voltas.txt/v4/P:14`, `voltas.txt/v5/P:4`, `voltas.txt/v7/P:3`. [nao verificado]

**Quem triou.** A v2 descartou a proibição antiga do motor na L-126, e a v3 registrou a emenda. O conflito residual com `!` ficou sem tratamento explícito. A ampliação para `models/` já estava na resposta da v1 e ficou fora da triagem seguinte. `voltas.txt/v1/R:10`, `voltas.txt/v2/P:6`, `voltas.txt/v3/P:7`, `opus.md:5`.

As referências `P` e `R` contam as linhas dentro de cada bloco da transcrição colada. A cópia local ainda contém `p_models` e, no trecho do deploy, a checagem de migrations pendentes. A identidade da cópia com o HEAD final é [nao verificado]. `bin/codex_pergunta.sh:167`, `bin/deploy.sh:185`.

| Achado | Classe | Trecho literal da pergunta | Trecho literal da resposta | Motivo |
|---|---|---|---|---|
| 1. Cerca de schema sem label | **(a)** | “Liste contra-exemplos: mudanca de model que gera migration e o teste nao ve” (`voltas.txt/v2/P:9`). “O portao novo do deploy.sh pega model sem migration em TODO app instalado em prod” (`voltas.txt/v4/P:12`). | “O portão cobre o Django do `saas_core`, inclusive `tenants`, quando a casca usa `config.settings.saas`. O comando roda sem rótulo de app e sem `--settings`.” `voltas.txt/v4/R:3`. | O tema foi perguntado. A resposta tomou a lista de apps instalados como cobertura e deixou passar o caso de `api` sem pacote de migrations, que o Opus atribuiu a `ask_initial`. `app/config/settings/saas.py:48`, `opus.md:2`. |
| 2. Selos de host sem prova verde no HEAD | **(c)** | “Os selos mordem? Os casos c_motor_livre/c_models_livre e o laco novo do test_agente_no_modelo passariam com o wrapper/disjuntor do HEAD?” `voltas.txt/v1/P:6`. | “O selo de deploy VERDE foi relatado pela sessão.” `voltas.txt/v6/R:1`. | A pergunta cobrava o comportamento dos selos. Vincular os verdes ao HEAD e comprovar o RED/GREEN dos casos 25/26 exigia os registros de execução. A prova no HEAD fica [nao verificado]. `opus.md:3`, `CLAUDE.md:427`. | [nao verificado]
| 3. Portão sem execução comprovada em produção | **(c)** | “Ha caminho de publicacao que o pule (--sem-migrate, --reload-copia, --reload-agendado, --restart, --conferir, HUP das 03:30 em config/crons.py)?” `voltas.txt/v4/P:12`. | “A declaração chama `deploy.sh --reload-agendado`, passando pela mesma guarda. O crontab instalado é **[nao verificado]**.” `voltas.txt/v4/R:12`. | O código mostra as chamadas do cron e da cópia. A execução efetiva do portão em produção fica [nao verificado]. `app/config/crons.py:326`, `bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:48`, `CLAUDE.md:418`, `opus.md:4`. |
| 4. Dois sentidos de `!` na L-126 | **(a)** | “A L-126 agora diz o aval sem omitir nem acrescentar (clausula a clausula)?” `voltas.txt/v5/P:4`. | “A ordem da fila está no bloco geral, e a condição de `PAREI` entrou na emenda.” `voltas.txt/v5/R:20`. | A resposta conferiu `PAREI` e deixou passar o efeito da proibição geral de “item que pede `!`” sobre o motor, que a PARADA 2 ainda lista entre os itens que exigem `!`. O descarte da v2 tratava da proibição antiga, posteriormente emendada. `app/docs/LEIS.md:147`, `CLAUDE.md:146`, `CLAUDE.md:274`, `CLAUDE.md:276`, `voltas.txt/v2/P:6`, `voltas.txt/v3/P:7`, `opus.md:5`. |
| 5. Remoção da guarda libera `models/` | **(b)** | “O diff faz o que o aval diz, e so isso?” `voltas.txt/v1/P:3`. | “A remoção de `p_models` libera também a antiga categoria `models/`.” `voltas.txt/v1/R:10`. | O Codex viu a ampliação. A triagem seguinte deixou o ponto de fora, o que sustenta **(b) por omissão**. Descarte formal e motivo ficam [nao verificado]. `bin/codex_pergunta.sh:167`, `voltas.txt/v2/P:4`, `voltas.txt/v2/P:6`, `opus.md:6`. |

**Contagem em cinco achados:** **(a) 2**, **(b) 1 por omissão na triagem**, **(c) 2**, **(d) 0**, **(e) 0**. `opus.md:2–6`, `CLAUDE.md:432–440`.

## 26/11 23:39 · sessao 6cccd208fc28 · O30-F3-CONSTRUIR · construtor · origem=Code

**Causa (b)**

1. `_dados_para_exportacao` preserva o `0`, e `pdf_bytes` exclui apenas `''` e `None`. [atestados_lote.py:102](/home/ronald/wt-codex-o30-f3-construir-225036/app/ponto/services/atestados_lote.py:102) · [atestados_lote.py:205](/home/ronald/wt-codex-o30-f3-construir-225036/app/ponto/services/atestados_lote.py:205)
2. O PDF põe os seis campos no mesmo parágrafo. O texto extraído em `green_c18_falha.txt` contém `Dias do\natestado: 0`. [atestados_lote.py:207](/home/ronald/wt-codex-o30-f3-construir-225036/app/ponto/services/atestados_lote.py:207)
3. `_texto` conserva a quebra, e o `assertIn` da c18 exige a sequência literal com espaço. [test_atestados_lote_tela.py:37](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:37) · [test_atestados_lote_tela.py:233](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:233)

**Diff** em [test_atestados_lote_tela.py:232](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:232)

```diff
         texto = _texto(PdfReader(BytesIO(resposta_pdf.content)))
+        texto = ' '.join(texto.split())
         for trecho in ('Médico: Dra. Exemplo', 'CRM: 765', 'UF do CRM: PR', 'CID: Z99.8',
```

O teste ainda exige os seis pares de rótulo e valor no texto extraído. A normalização só altera espaços em branco. Suprimir o campo inteiro ou só o `0` faz a asserção de `Dias do atestado: 0` falhar. [test_atestados_lote_tela.py:231](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:231) [nao verificado]

**RED e mutantes** em memória, com o trecho extraído do anexo e a asserção do arquivo. A execução integrada pelo Code fica [nao verificado]. [test_atestados_lote_tela.py:232](/home/ronald/wt-codex-o30-f3-construir-225036/app/relatorios/tests/test_atestados_lote_tela.py:232)

```bash
sonda() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B - "$1" <<'PY'
import sys
from pathlib import Path
from textwrap import dedent
from unittest import TestCase

modo = sys.argv[1]
fonte = Path('relatorios/tests/test_atestados_lote_tela.py').read_text()
inicio = fonte.index("        texto = ' '.join(texto.split())")
fim = fonte.index('\n\n', inicio)
checagem = dedent(fonte[inicio:fim])
texto = ('Médico: Dra. Exemplo · CRM: 765 · UF do CRM: PR · CID: Z99.8 · '
         'Emissão: 12/08/2026 · Dias do\natestado: 0\n')
if modo == 'red':
    checagem = checagem.split('\n', 1)[1]
elif modo == 'junta_sem_espaco':
    checagem = checagem.replace("' '.join", "''.join", 1)
elif modo == 'sem_campo':
    texto = texto.replace(' · Dias do\natestado: 0', '')
elif modo == 'sem_zero':
    texto = texto.replace('atestado: 0', 'atestado:')
exec(checagem, {'self': TestCase(), 'texto': texto})
PY
}

sonda red                # rc=1, reproduz a falha da c18
sonda corrigido          # rc=0
sonda junta_sem_espaco   # rc=1
sonda sem_campo          # rc=1
sonda sem_zero           # rc=1
```

## 26/11 23:24 · sessao 3ac36de352b7 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

1. **O item (3) está curado.** A única troca é o nome da HOM pelo trecho `[homologador; nome trocado pelo papel, dado pessoal]`. Os outros **825 caracteres** da citação coincidem, caractere a caractere, com o aval raspado e com o campo `frase` do JSON. Reverter só essa troca em memória reproduziu o hash abreviado do blob anterior indicado no patch. `aval.txt:1`, `delta.patch:2`, `delta.patch:9`, `delta.patch:10`, `app/docs/LEIS.md:147`, `app/docs/CORTES.json:2475`.

2. **Ainda aparece “Ronald”**, como autor do corte já citado na linha 147 e nas linhas de contexto. O delta introduz zero nomes de pessoa, e o nome da HOM saiu da linha 147. “Claude” designa o agente verificador. No ensaio em memória com cadastro sintético, o nome da HOM passava pela raspagem na versão anterior e já estava ausente na c7. `delta.patch:6`, `delta.patch:9`, `delta.patch:10`, `app/docs/LEIS.md:147`, `CLAUDE.md:402`, `bin/nome_no_texto.py:78`, `bin/nome_no_texto.py:115`.

3. **O descarte como fila procede nesta obra.** A ocorrência em `CORTES.*` está fora do hunqe da c7. O publicador inclui `LEIS.md` e `RELATO.md`, e a tabela `SEUS CORTES` leva ao `RELATO` apenas corte, hora, idade, estado e fatia. A frase integral fica no registro de cortes. A presença anterior à obra inteira no histórico Git e a publicação efetiva no remoto ficam [nao verificado]. `delta.patch:1`, `app/docs/CORTES.json:2475`, `app/docs/CORTES.md:651`, `bin/gerar_cortes.py:87`, `bin/gerar_cortes.py:105`, `bin/gerar_cortes.py:118`, `bin/relato.sh:125`, `bin/relato.sh:158`.

4. **Quem escreve e quem lê.**
   - **Origem da L-126.** Ronald dá o aval, `CORTES.json` guarda a frase e `gerar_cortes.py` lê o JSON e escreve `CORTES.md`. A autoria da edição da linha 147 fica [nao verificado]. `app/docs/CORTES.json:2475`, `bin/gerar_cortes.py:31`, `bin/gerar_cortes.py:114`, `app/docs/LEIS.md:147`.
   - **Publicação.** `relato.sh` entrega `LEIS.md` a `raspar_publico.py`, que lê, mascara, confere e escreve a cópia. `relato.sh` faz o commit e o push. O selo de publicação lê os arquivos rastreados no destino. `bin/relato.sh:125`, `bin/relato.sh:131`, `bin/relato.sh:156`, `bin/raspar_publico.py:28`, `bin/raspar_publico.py:40`, `bin/tests/test_publicacao_sem_nome.sh:63`, `bin/tests/test_publicacao_sem_nome.sh:72`.
   - **Selos.** O índice lê o vínculo com o corte e a estrutura da tabela. O pre-push chama o selo que lê `PROTEGE`, cuja célula ficou idêntica. Na cópia, o índice retornou `RED (1)` por outros cortes sem linha. Esta emenda está mapeada na linha 423. `app/docs/LEIS.md:147`, `app/docs/LEIS.md:423`, `bin/tests/test_leis_indice.sh:25`, `bin/tests/test_leis_indice.sh:49`, `bin/tests/test_leis_indice.sh:67`, `bin/pre-push.sh:16`, `bin/tests/test_lei_protege_sitio.sh:77`.

**APROVADO — (3) curado, citação preservada fora da troca declarada, zero nomes novos e `CORTES.*` em fila nesta obra.** `delta.patch:9`, `delta.patch:10`, `app/docs/LEIS.md:147`, `bin/gerar_cortes.py:87`.

_8535 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
