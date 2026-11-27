# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 15:35** · publicado em **27/11/2026 15:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 15:35 · sessao 0c713d148d50 · INVENTARIO-PELA-REGRA · construtor · origem=Code

Corrigi o instrumento do AUS-05. `_EmissorMudo._espelhar_marcos` tem a assinatura do emissor real e retorna `False`, o valor do caminho sem escrita. A comparação com o HEAD confirmou que os 35 casos C2 receberam somente mudanças de docstring, preservando as asserções de ESC-12, AUS-03 e FOL-14. [app/folha/tests/_celulas.py:11] [app/ponto/management/commands/detectar_ausencias.py:544] [app/ponto/management/commands/detectar_ausencias.py:558]

A bateria ficou com **50 casos etiquetados**. CMP-06 a CMP-11 e CMP-15 têm `@unittest.skip("lei: ...")` e duas respostas candidatas. As frentes da tabela são escolhas técnicas de leitura, sujeitas à revisão do Code. A menção a intermitente não veio acompanhada de cadastro, batidas e gabarito entre os 15 casos do L4. [app/folha/tests/_inventario_c3_mutantes.py:74] [app/folha/tests/test_inventario_competencia.py:247] [app/folha/tests/test_inventario_competencia.py:408] [nao verificado]

O RED isolado do AUS-05 reproduziu o `AttributeError` ao retirar o método em memória. O RED do predicado de rescisão apontou CMP-12 em 01/2027, CMP-13 em 01/2027 e CMP-14 em 12/2026. Para o movimento normal, os testes cobram matrícula, competência e valor em centésimos de hora. O L4 não informa o código dessa rubrica, e o escritor atual não oferece o evento de horas normais. A execução Django do recorte fica com Code e opus final. [app/folha/tests/_inventario_c3_mutantes.py:45] [app/folha/tests/_inventario_c3_mutantes.py:146] [app/folha/tests/test_inventario_competencia.py:132] [app/folha/export.py:439] [nao verificado] [nao verificado]

### Diff por arquivo

| Arquivo | Linhas `+/-` | Mudança |
|---|---:|---|
| [folha/tests/_celulas.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/_celulas.py:11) | +3/−0 | Método mudo |
| [ponto/tests/test_inventario_escala.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:180) | +36/−12 | 12 docstrings |
| [ponto/tests/test_inventario_ausencia.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:31) | +27/−9 | 9 docstrings |
| [folha/tests/test_inventario_folha.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:130) | +42/−14 | 14 docstrings |
| [folha/tests/test_inventario_competencia.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_competencia.py:1) | +444/−0 | 15 casos CMP |
| [folha/tests/_inventario_c3_mutantes.py](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/_inventario_c3_mutantes.py:1) | +185/−0 | Ensaios em memória |

**`except` no diff: 0**, contando inclusões e remoções nos seis arquivos acima.

### RED e mutantes

Na pasta `app`, esta função invoca os ensaios sem ORM. [app/folha/tests/_inventario_c3_mutantes.py:159]

```bash
c3() { PYTHONDONTWRITEBYTECODE=1 python3 folha/tests/_inventario_c3_mutantes.py "$1"; }
```

| Comando | Resultado observado | Referência |
|---|---|---|
| `c3 controle` | rc 0 | `app/folha/tests/_inventario_c3_mutantes.py:174` |
| `c3 red-emissor` | rc 1, método ausente na chamada do cartório | `app/folha/tests/_inventario_c3_mutantes.py:49` |
| `c3 sem-palavra-chave` | rc 1, rejeita `reaberto=` | `app/folha/tests/_inventario_c3_mutantes.py:51` |
| `c3 retorno-true` | rc 1, sinaliza escrita indevida | `app/folha/tests/_inventario_c3_mutantes.py:54` |
| `c3 sem-etiqueta` | rc 1, acusa a etiqueta ausente em CMP-06 | `app/folha/tests/_inventario_c3_mutantes.py:84` |
| `c3 sem-skip` | rc 1, acusa o `skip` ausente em CMP-06 | `app/folha/tests/_inventario_c3_mutantes.py:96` |
| `c3 corte-trocado` | rc 1, acusa 20/11 atribuído a dezembro | `app/folha/tests/_inventario_c3_mutantes.py:118` |
| `c3 movimento-outra-competencia` | rc 1, acusa movimento na competência errada | `app/folha/tests/_inventario_c3_mutantes.py:139` |
| `c3 movimento-minutos-a-menos` | rc 1, encontra 1100 centésimos onde espera 1200 | `app/folha/tests/_inventario_c3_mutantes.py:140` |
| `c3 movimento-em-feriado` | rc 1, rejeita o valor na rubrica de feriado | `app/folha/tests/_inventario_c3_mutantes.py:141` |
| `c3 red-rescisao` | rc 1, acusa as três exclusões pelo predicado real | `app/folha/tests/_inventario_c3_mutantes.py:146` |

### Recorte para o Code

Na raiz da cópia, com os quatro módulos dentro de `--only`. Execução neste turno [nao verificado]. [app/docs/RELATO.md:10801]

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --only "ponto.tests.test_inventario_escala ponto.tests.test_inventario_ausencia folha.tests.test_inventario_folha folha.tests.test_inventario_competencia"
```

### Etiquetas

| Classe | Família | Frente | Nascente |
|---|---|---|---|
| ESC-01 | escala | espelho | `app/escala/utils.py:2284` |
| ESC-02 | escala | espelho | `app/escala/utils.py:2284` |
| ESC-03 | escala | espelho | `app/escala/alimentacao.py:228` |
| ESC-04 | escala | espelho | `app/ponto/turnos.py:2971` |
| ESC-05 | escala | espelho | `app/ponto/motor_calculo_v2.py:1049` |
| ESC-06 | escala | espelho | `app/ponto/motor_calculo_v2.py:961` |
| ESC-07 | escala | espelho | `app/ponto/precedencia.py:259` |
| ESC-10 | escala | espelho | `app/ponto/motor_calculo_v2.py:3535` |
| ESC-11 | escala | espelho | `app/ponto/motor_calculo_v2.py:3538` |
| ESC-12 | escala | espelho | `app/ponto/services/dia_pago.py:239` |
| ESC-13 | escala | espelho | `app/ponto/services/dia_pago.py:250` |
| ESC-15 | escala | espelho | `app/ponto/services/espelho.py:1200` |
| AUS-01 | ausencia_ferias | espelho | `app/ponto/motor_calculo_v2.py:3596` |
| AUS-02 | ausencia_ferias | espelho | `app/ponto/services/dia_pago.py:396` |
| AUS-03 | ausencia_ferias | espelho | `app/escala/utils.py:1262` |
| AUS-04 | ausencia_ferias | espelho | `app/ponto/services/dia_pago.py:193` |
| AUS-05 | ausencia_ferias | espelho | `app/ponto/services/dia_pago.py:346` |
| AUS-06 | ausencia_ferias | ferias | `app/ferias/services.py:504` |
| AUS-07 | ausencia_ferias | ferias | `app/folha/export.py:90` |
| AUS-08 | ausencia_ferias | ferias | `app/ferias/services.py:49` |
| AUS-10 | ausencia_ferias | espelho | `app/ponto/services/dia_pago.py:396` |
| FOL-01 | folha_export | txt | `app/folha/export.py:439` |
| FOL-02 | folha_export | txt | `app/folha/export.py:441` |
| FOL-03 | folha_export | txt | `app/folha/export.py:446` |
| FOL-04 | folha_export | txt | `app/folha/export.py:443` |
| FOL-05 | folha_export | txt | `app/folha/export.py:447` |
| FOL-06 | folha_export | espelho | `app/ponto/motor_calculo_v2.py:3603` |
| FOL-07 | folha_export | txt | `app/folha/export.py:215` |
| FOL-08 | folha_export | txt | `app/folha/export.py:752` |
| FOL-09 | folha_export | txt | `app/folha/export.py:399` |
| FOL-10 | folha_export | txt | `app/folha/export.py:739` |
| FOL-11 | folha_export | txt | `app/ponto/janelas.py:15` |
| FOL-12 | folha_export | txt | `app/ponto/services/dia_pago.py:109` |
| FOL-13 | folha_export | txt | `app/folha/export.py:525` |
| FOL-14 | folha_export | txt | `app/folha/views.py:277` |
| CMP-01 | escala | espelho | `app/ponto/janelas.py:15` |
| CMP-02 | escala | txt | `app/folha/export.py:443` |
| CMP-03 | escala | txt | `app/ponto/services/fechamento.py:979` |
| CMP-04 | escala | espelho | `app/ponto/motor_calculo_v2.py:3603` |
| CMP-05 | escala | espelho | `app/ponto/motor_calculo_v2.py:3811` |
| CMP-06 | escala | txt | `app/core/regua_cct.py:477` |
| CMP-07 | escala | txt | `app/ponto/motor_calculo_v2.py:961` |
| CMP-08 | escala | txt | `app/ponto/motor_calculo_v2.py:961` |
| CMP-09 | escala | espelho | `app/ponto/motor_calculo_v2.py:3593` |
| CMP-10 | escala | espelho | `app/ponto/motor_calculo_v2.py:3593` |
| CMP-11 | escala | espelho | `app/ponto/motor_calculo_v2.py:3593` |
| CMP-12 | folha_export | txt | `app/folha/export.py:875` |
| CMP-13 | folha_export | txt | `app/folha/export.py:875` |
| CMP-14 | folha_export | txt | `app/folha/export.py:875` |
| CMP-15 | ausencia_ferias | ferias | `app/folha/export.py:90` |

## 27/11 15:22 · sessao 92a042f4758d · FERIADO-UM-JUIZ · verificador · origem=Code

**REPROVADO.** A c2/c3 corrige a emissão da HE100 de feriado no TXT da 11 quando o vínculo termina em 12x36. Ainda há quatro bloqueios de dinheiro: K1 continua pagando a dobra sobre 470 minutos, o recálculo genérico pode mudar exportadas fora da guarda, o resumo do cartão pode perder as 4 h que o TXT emite e a lavra `oraculo` ainda pode divergir em feriado de trabalho normal após troca de vínculo. (`app/folha/export.py:744–749`, `app/ponto/services/fechamento.py:564–600,948–965`, `app/ponto/tests/test_feriado_um_juiz_porta.py:272–286`, `app/relatorios/cartao_pela_celula.py:89–101`, `app/ponto/calculador/alimentacao.py:423–446,559–577`)

## Aval literal e alcance

- **A promoção principal está implementada.** Para cada período de folga trabalhada, `particionar_ft` usa o motor que apurou a fatia e consulta `_feriado_dobra_no_dia` pelo dia do turno do par. Em 6x1, com dobra devida, escala incerta vai para HE100 feriado. Com escala certa, `feriado_vira_folga=True` vai para HE100 feriado e `False` conserva folga trabalhada, também a 100%. Sem dobra, a partição manda a folga certa para a rubrica de folga e a incerta para trabalhadas. O corte por ciclo mantém 12x36 fora da promoção, e o corte por base ou ciclo exclui intermitentes. O diff preserva o arquivo do motor, o conjunto `CICLOS_FERIADO_SIMPLES` e a cláusula de CCT já usada no caminho regular do 12x36. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `diff.txt:1`, `app/ponto/calculador/regras.py:366–414`, `app/ponto/motor_calculo_v2.py:941–978,3740,3764–3767,3810–3822`, `app/core/regua_cct.py:473–479`, `app/folha/export.py:445–452`)
- **“Escala certa só escolhe a rubrica” vale dentro da partição.** Antes dela, a L-097 ainda pode zerar uma FT certa sem autorização, com previsto zero e empresa na janela. Com os mesmos 240 min de entrada, o bloqueio preservou 240 min para escala incerta e zerou o caso de escala certa na reprodução em memória. A incidência nos 75 dias é [nao verificado]. **RED:** passar a mesma FT 6x1 pela porta, com e sem autorização, variando apenas a certeza da escala. Se o aval exigir a mesma quantidade também antes da partição, há uma decisão de lei sobre a L-097. (`app/ponto/motor_calculo_v2.py:1562–1597,2558–2574`, `app/ponto/calculador/regras.py:389–409`, `CLAUDE.md:274–283`)
- **O motor por fatia já existia.** O acréscimo de `apurar_por_fatia` é guardar `(motor, vínculo)` por identidade do período FT. Esse vínculo com a origem cabe no aval. Também cabe ajustar a emissão e a homologação da 11 para transportar a rubrica já classificada. Há deltas além do literal: o arredondamento muda `horas_trabalhadas` em mês sem feriado, a alimentação da lavra `oraculo` muda também em dias normais de intermitentes e a promoção do fechamento alcança competências anteriores à 11. O TXT lê a partir de 11 qualquer HE100 feriado positiva do mensal, inclusive em um mensal 12x36 sem informação de proveniência dessa parcela. O comportamento inscrito no motor para 12x36 permanece, mas a camada de emissão mudou. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/ponto/services/fechamento.py:299–342,948–970,1246–1276`, `app/ponto/calculador/alimentacao.py:456,529–570`, `app/folha/export.py:383–385,425–428,744–749,1023–1028`, `CLAUDE.md:274–277`)

## Laudo c1, achado por achado

| Achado da c1 | Situação em c2/c3 e RED |
|---|---|
| **1. TXT perde a primeira fatia ou retém em `cadastro_zero`.** | **Curado na montagem e homologação da 11 em diante.** Há testes para 6x1 → 12x36 e para só 4 h de FT promovida, com previsto e `horas_trabalhadas` zerados e rubrica homologada. Eles passam por classificação, montagem e serialização, com fechamento gravado e calculado em leitura. **Procedente no resumo:** reproduzimos em memória TXT `[400]`, HE100 bruta `4,00` e `total_extras=0,00` no cartão. O app e a ficha repassam esse total. O contador `cartao_x_txt` compara HE100 bruta, deixando a rubrica feriado e `total_extras` fora da comparação. **RED:** no mesmo caso, exigir 4 h no resumo e na linha 0200, e fazer o contador acusar supressão da rubrica com HE100 bruta igual. (`app/folha/export.py:744–757,921–923,995–1000,1023–1037`, `app/ponto/tests/test_feriado_um_juiz_porta.py:164–177,215–227`, `app/relatorios/cartao_pela_celula.py:89–101,249–274,320–346`, `app/api/views.py:1660–1669`, `app/colaboradores/services/ficha.py:198–205`) |
| **2. Exportada fora da guarda pode mudar.** | **Procedente.** A guarda específica cobre setembro de 2026. Fora dela, a L-113 permite o recálculo genérico sob suas condições. O aval desta fatia veda tocar exportada. O evento retroativo continua chegando ao mensal e às duas lavras com a classificação nova. O POST do TXT usa o fechamento calculado em leitura, inclusive onde a guarda impede escrita do recálculo, e pode registrar uma nova fotografia. **RED:** entregar uma competência fora da guarda, disparar evento retroativo e exigir igualdade do mensal, das duas lavras e do TXT vigente. Exercitar também a reemissão na 09 guardada e na 11 entregue, com a 11 aberta como controle. Alteração efetiva em produção [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/LEIS.md:156`, `app/ponto/services/fechamento.py:36–50,564–600,948–965,1118–1185,1246–1276,1780–1806`, `app/folha/views.py:214–219,269–275,302–307`, `app/folha/models.py:173–188`) |
| **3. Oráculo e DIFF redecidem feriado.** | **Curado para FT projetada e para o intermitente no caminho alimentado.** F3 continua no motor. O auxiliar avulso empresta o próprio método, e a lavra FT recebe a partição. O DIFF usa a mesma projeção nos dois lados, portanto `4,00 = 4,00` prova a ligação, enquanto a classificação é cobrada pelos testes da porta. **Procedente fora de FT:** o oráculo ainda recompõe a dobra por dia, usa o flag de um motor montado no início com o template de referência do fim e pode cair na consulta nacional. O caminho de `periodo.py` também recompõe com os insumos que recebeu. Em memória, um par normal 6x1 deu 240 min no motor e zero em `do_dia` com o flag falso do 12x36 final. **RED:** feriado em dia normal da primeira fatia num 6x1 → 12x36 e no sentido inverso, com igualdade do mensal, das lavras e do TXT. Cobrir também calendário oficial injetado vazio. (`app/core/juizes.py:318–320`, `app/ponto/calculador/regras.py:297–310,360–363,727–748,818–829`, `app/ponto/calculador/alimentacao.py:423–446,559–577`, `app/ponto/calculador/periodo.py:828–845`, `app/ponto/management/commands/diff_calculador.py:110–148,231–253,399–409`, `app/ponto/tests/test_feriado_um_juiz_porta.py:229–270`) |
| **4. Segundo escritor do split e split acima do total.** | **Curada a sobrescrita do período bruto e imposto o teto da promoção.** O split segue numa parcela própria, mensal e lavras a leem, e o noturno legal do dia promovido vem dos períodos apurados. **Procedente a exatidão do noturno longo:** reproduzimos 21h → 23h do dia seguinte, com prorrogação. A promoção retorna 1.560 min de split, embora a união da janela seja 1.500 min. A sobreposição compensa a hora diurna inicial, e o adicional legal também carrega o excesso. O teste atual começa às 22h e cobra o teto. **RED:** exigir 1.500 min de split e `1.500/0,875` min de adicional legal no caso de 21h. Incidência na frota [nao verificado]. (`app/ponto/calculador/regras.py:379–414`, `app/ponto/calculador/alimentacao.py:361–387`, `app/ponto/services/dia_pago.py:223–225,244–257`, `app/ponto/motor_calculo_v2.py:82–118,151–161,1027–1049`, `app/ponto/tests/test_feriado_um_juiz.py:196–216`, `app/ponto/tests/test_feriado_um_juiz_porta.py:242–248,343–351`) |
| **5. K1 e dia do turno.** | **K1 procedente.** A c3 conservou a asserção e marcou a falha como esperada. **Dia parcialmente curado:** o juiz de dobra agora recebe o dia do turno do par. A certeza, a cláusula da rubrica e a linha lavrada ainda usam a jornada herdada, que pode ser a véspera com intervalo de até 6 h entre turnos distintos. O teste mensal soma a competência inteira e aceita as 4 h mesmo se caírem na data errada. **RED:** nos pares 18h–22h na véspera e 03h–07h no feriado, exigir 4 h comuns na primeira linha e 4 h de feriado na segunda, em ambas as versões. Repetir com praça e certeza diferentes, e com a virada na borda da competência. (`app/ponto/calculador/regras.py:390–409`, `app/ponto/motor_calculo_v2.py:963–978,2513–2522,2558–2574`, `app/ponto/services/dia_pago.py:126–132,244–257`, `app/ponto/tests/test_feriado_um_juiz.py:176–194`, `app/ponto/tests/test_feriado_um_juiz_porta.py:272–286,298–324`) |
| **6. Soma, idempotência e falha da relavra.** | **Curado o caso 241 + 241 min:** agora soma e arredonda uma vez, chegando a 8,03 h em vez de 8,04 h. O teste repete a apuração e confere as somas das duas versões. Esse próprio RED comprova delta fora de feriado. **Procedente a falha parcial:** o mensal é salvo antes das lavras, cada erro é capturado separadamente e `lavrar` exclui as linhas antes de recriá-las. A repetição bem-sucedida conserva números no cenário testado e recria as linhas. **REDs:** cobrar paridade com a base num mês sem feriado para demonstrar o delta de escopo. Injetar falha no `bulk_create` depois do `delete` e exigir a preservação do conjunto anterior. Intercalar duas relavras e, em outro teste, primeira entrega e evento retroativo, conferindo mensal, versões e TXT vigente. (`app/ponto/services/fechamento.py:963–965,1118–1222,1246–1283,1780–1806`, `app/ponto/services/dia_pago.py:344–359,549–562`, `app/ponto/tests/test_feriado_um_juiz_porta.py:288–296`, `app/ponto/models.py:380–387`) |

Os controles pedidos de **véspera de um mesmo turno**, **municipal versus nacional**, **folga com abono** e **BO-B(3)** estão no código. O partido da véspera espera 7 h comuns e zero feriado. O municipal só alcança a praça pertinente, com nacionais nas duas praças, e o teste espera 12 h de feriado mais 4 h comuns. A folga promovida conserva 4 h e abono zero, e a ausência num dia com 480 min previstos e 240 realizados abona 240. BO-B(3) com escala certa e `feriado_vira_folga=False` conserva 4 h em folga trabalhada. O teste da virada com **dois turnos distintos** ainda precisa cobrar a data de cada linha, conforme o item 5. (`app/core/feriados.py:57–67,135–167`, `app/ponto/services/dia_pago.py:386–401`, `app/ponto/tests/test_feriado_um_juiz_porta.py:110–136,298–324,366–409`)

## Quem escreve e quem lê

| Campo ou valor | Escrita e leitura |
|---|---|
| **Períodos, minutos, calendário e dias** | O v2 escreve `minutos_trabalhados`, pontas efetivas, intervalos, extras brutas, `periodos`/`periodos_ft` e o mapa da jornada. O carregador de feriados resolve o município do vínculo de cada data. `apurar_por_fatia` compõe os mapas e grava em memória o contexto por identidade, lido por `particionar_ft` no fechamento e no DIFF. Escala certa lê LIMBO e A2/A8. `feriado_vira_folga` lê o cadastro `trabalha_em_feriado`, ciclo e calendário da praça. Há três valores distintos: dia civil da entrada, dia do turno do juiz e dia da jornada lavrada. Os dois últimos podem divergir por herança. (`app/ponto/motor_calculo_v2.py:963–978,2513–2522,2558–2607,3353–3389`, `app/core/feriados.py:135–167`, `app/ponto/services/fechamento.py:314–346,672–673,950–952`, `app/ponto/management/commands/diff_calculador.py:137–148`, `app/escala/services/escala_certa.py:19–51`, `app/escala/models.py:1384–1406`, `app/ponto/services/dia_pago.py:126–132`) |
| **Trabalhadas, folga, HE50, HE100 e noturno** | O fechamento escreve `horas_trabalhadas` com períodos normais, órfãos e FT comum. FT certa vai para `horas_folga_trabalhada`. FT promovida entra em `horas_extras_100_feriado`, na HE100 total e nas extras totais. O split da promoção é somado à HE100 noturna. `lavrar` lê os mesmos períodos e listas para a versão `motor`. O TXT lê o mensal, calcula HE100 comum como total menos feriado, emite folga e feriado por rubrica e emite o adicional noturno à parte. `horas_trabalhadas` também alimenta classificação, travas de aprovação, prontidão e resumo. (`app/ponto/motor_calculo_v2.py:294–344`, `app/ponto/services/fechamento.py:941–965,1124–1155,1180–1188`, `app/ponto/services/dia_pago.py:173–186,223–271`, `app/folha/export.py:425–457,850–851,921–923,1059–1063`, `app/relatorios/cartao_pela_celula.py:93–120`) |
| **`rubricas_ft`, `lavrar(rubricas=...)` e leitores diários** | `alimentacao` projeta a partição por dia e recompõe as contribuições dos períodos normais naquele dia. `do_dia` lê a projeção e ainda apura intra e outras rubricas conforme o ramo. O fechamento passa `rubricas=` à mesma `lavrar` para a versão `oraculo`, desde 10/2026. A porta valida os donos declarados na chamada e substitui os campos fornecidos sobre a base já montada. `soma_da_competencia` agrega por versão. Para horas pagas do dia, `pago_do_dia` soma trabalhadas, folga e `detalhe.folga_feriado_min`. O detalhe é só da FT promovida. Assim a HE100 feriado de um período normal, cujos minutos já estão em trabalhadas, entra uma vez nas horas pagas. Espelho, app, calendário e testemunha do export leem essas linhas ou suas projeções. (`app/ponto/calculador/alimentacao.py:353–388,559–582`, `app/ponto/calculador/regras.py:747–778,914–918`, `app/ponto/services/fechamento.py:1246–1278`, `app/ponto/services/dia_pago.py:50–76,244–257,311–359,549–562,889–927`, `app/ponto/services/espelho.py:820–831`, `app/api/views.py:1527–1555`, `app/colaboradores/services/calendario.py:685–689`, `app/folha/porta_export.py:113–124`) |

A partição é **disjunta por ocorrência**: cada período FT entra em uma lista. Na rubrica do TXT, feriado e HE100 comum separam o total, e o split noturno é subconjunto. Isso sustenta a conservação no golden e no caso de 241 + 241 min. A igualdade **mensal = soma de cada lavra** permanece aberta nos contraexemplos de K1 e de feriado normal com troca de vínculo. A igualdade numérica também pode conviver com uma data errada na lavra. (`app/ponto/calculador/regras.py:389–414`, `app/ponto/services/fechamento.py:948–965`, `app/ponto/services/dia_pago.py:244–267,549–562`, `app/folha/export.py:425–452`, `app/ponto/tests/test_feriado_um_juiz_porta.py:69–92,272–308`)

## Selos, K1 e pouso [nao verificado]

- **Classes estruturais.** A promoção chama F3 e produz um valor em memória, sem novo escritor persistente. A derivação paralela que sobra é a alimentação do feriado normal do oráculo, descrita no item 3. O diff introduz zero leituras de relógio e zero inversões de trava. A corrida preexistente continua relevante para o dinheiro novo: a entrega trava a empresa, a revisão específica usa Empresa → Colaborador → Batida e consulta K6, e o recálculo genérico por evento roda em `on_commit` sem essa serialização. (`diff.txt:1`, `app/ponto/calculador/regras.py:389–414,818–829`, `app/ponto/services/dia_pago.py:344–359`, `app/folha/views.py:103,132–135`, `app/folha/export.py:561–567`, `app/ponto/services/par_relampago_na_lista.py:172–187`, `app/ponto/services/fechamento.py:1780–1834`)
- **Os mutantes têm mordida no código.** Na cópia, sem gravação, rodamos **26 controles e 22 mutantes sem banco**. Os controles passaram e cada mutante provocou falha de asserção na função alterada em memória. Dos seis mutantes adicionais de porta, cinco alteram produção e um altera a fixture para provocar erro de unicidade. A execução dos seis e o RED contra o commit-base são [nao verificado]. O selo de endereço `endereco-c2` morde o registro de linha, com escopo de numeração. A cópia é um `git archive` e o modo `base` requer `git show`. (`app/ponto/tests/feriado_um_juiz_mutantes.py:38–143,169–223`, `app/core/tests/test_medida_turno_do_par.py:47–48,565–569`, `CLAUDE.md:375–379`)
- **O `expectedFailure` de K1 deixa passar dinheiro errado.** O teste força a rota `MotorTurnoPartido` para `10:00–14:00 + 14:30–18:20` em feriado de folga e cobra 440 min de HE100 feriado, 440 de HE100 total e 30 min de intra no mensal e nas duas somas. A reprodução em memória deu 470 min de HE100 na promoção, zero intra no motor e 30 min de intra no `do_dia` com pausa de 30 e mínimo de 60. O laço de intra entre pares percorre `resultado.periodos`, e a FT fica em `periodos_ft`. O agregado do job informado não identifica qual asserção falhou. **RED:** tornar esse teste obrigatório sob o `!` separado do K1 e cobrar as três rubricas nas duas lavras, no mensal e na emissão com rubricas configuradas. O registro da dependência põe E1 antes de K1 e condiciona o pouso à prova de frota da 11 por rubrica. Pouso efetivo e ocorrência produtiva desse caso [nao verificado]. (`app/ponto/tests/test_feriado_um_juiz_porta.py:138–148,272–286`, `app/ponto/motor_calculo_v2.py:3044–3061,3078–3118`, `app/ponto/services/fechamento.py:953–960,985`, `app/ponto/calculador/regras.py:768–778`, `app/docs/RELATO.md:252,255`, `app/docs/PROMPTS.md:3016`)
- **Dos 40, a obra acrescenta um.** K1 é o único `expectedFailure` adicionado pelo diff. O censo do recorte na cópia encontrou 25 marcas diretas, incluindo K1, e 15 marcas temporárias de cron com prazo até 27/11. Isso coincide com o total informado para `1127142942_29488e3b`, `Ran 16274`, `OK`, 32 skips e 40 falhas esperadas. Identidades no log do job [nao verificado]. K1 tem tag e item de fila, mas a marca é incondicional. Prazo e aval para silenciar K1 [nao verificado]. (`diff.txt:1`, `app/ponto/tests/test_feriado_um_juiz_porta.py:272–286`, `app/core/tests/marca_cron15.py:27–65`, `app/docs/BACKLOG.md:2005`, `CLAUDE.md:197–201,274–277`)

**Falta para o pouso e o apply.** Fechar os REDs procedentes e o pré-requisito K1. Certificar a suíte **inteira** no hash que vai pousar e fazer o opus final de mão única depois dela. O recorte verde informado cobre apenas os rótulos listados. Para a 11, comprovar que está aberta, a medição anterior dos **75 dias/42 colaboradores** como hora comum e a exclusão dos intermitentes. Antes do apply, gerar reversão por colaborador em `logs/` e guardar os hashes das exportadas vigentes e os valores gravados que a obra pode alcançar. No mesmo marco, publicar a prova de **frota** depois do apply, com antes × depois por colaborador, horas e rubrica, DIFF do TXT 11, hashes antes/depois das exportadas e trilha em `PENDENTES` com reversão. O corte separado do K1 exige ainda DIFF de frota por rubrica **antes** do pouso e `PAREI` se houver mudança fora de feriado de folga. Essas evidências operacionais, o pouso e o apply são [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/PROMPTS.md:3016`, `CLAUDE.md:129,260–287,295–299,421–431,448–467`)

## 27/11 15:21 · sessao ce7b3625b60e · - · - · origem=Ronald (tmux)

**REPROVADO.** K1 continua no caminho de produção. A promoção soma 470 minutos brutos de HE100 feriado no turno partido. A apuração de intra percorre apenas `resultado.periodos`, de modo que o caso de folga fica fora do laço. O `expectedFailure` mascara essa falha na pista. É o único `expectedFailure` adicionado pelo diff, e a identidade dos outros 39 no job fica [nao verificado]. **RED:** exigir 440 min de HE100 e 30 min de intra no mensal e nas duas lavras, com o teste normal. `app/ponto/motor_calculo_v2.py:3059–3118`, `app/ponto/services/fechamento.py:953–960,985`, `app/ponto/tests/test_feriado_um_juiz_porta.py:138–147,272–287`, `app/docs/RELATO.md:252–255`, `app/docs/PROMPTS.md:3016`.

- **F3 e segundo juiz.** A partição consulta o método registrado do motor e reaproveita a instância da fatia. O oráculo e o DIFF passam a consumir a partição da FT. Dias sem projeção, porém, julgam o feriado de novo com a régua de um motor montado com o template final e a data inicial, além de manterem fallback nacional. Em memória, um período real de MotorComercial 6x1 deu 240 min de HE100 feriado. O mesmo par em `do_dia`, alimentado com flag falso do vínculo final, deu 0 h. **RED:** 6x1 → 12x36 e sequência inversa, com feriado em dia normal, exigir mensal e ambas as lavras iguais. `app/core/juizes.py:320`, `app/ponto/motor_calculo_v2.py:941–978,1000–1011,3380–3388`, `app/ponto/calculador/regras.py:297–310,360–363,727–748,818–829`, `app/ponto/calculador/alimentacao.py:423–446,559–577`, `app/ponto/management/commands/diff_calculador.py:110–148,231–253,399–409`.
- **Data do turno e data da lavra.** A c2 corrige a data usada para decidir a dobra. A data usada para lavrar e escolher rubrica pode continuar na véspera pela herança de até seis horas. O RED de dois turnos distintos hoje soma o mês. **RED:** separar turnos `18–22` e `03–07`, exigir nas duas lavras a HE100 no dia do segundo turno e testar a borda da competência e escalas certas divergentes. `app/ponto/motor_calculo_v2.py:963–978,2513–2522,2558–2574,3067–3075`, `app/ponto/calculador/regras.py:389–401`, `app/ponto/services/dia_pago.py:126–132,244–257`, `app/ponto/tests/test_feriado_um_juiz.py:176–194`, `app/ponto/tests/test_feriado_um_juiz_porta.py:299–313`.
- **Partição e noturno.** A partição tem três destinos disjuntos por ocorrência, reaproveita o split válido, calcula o faltante pela função do motor e limita o split da FT promovida à HE100. O valor bruto do período fica preservado. O noturno legal deriva da fatia nas projeções com promoção. O acesso direto a `motor.prorrogacao_noturna_pos5h` contorna a fronteira declarada da v3 e passa pelo filtro atual do selo, que procura `getattr` com default. **RED estrutural:** reconhecer acesso ao flag no motor desempacotado de `contexto_por_periodo` e exigir a porta declarada. `app/ponto/calculador/regras.py:379–414`, `app/ponto/calculador/nucleo.py:314–350`, `app/ponto/calculador/alimentacao.py:361–387`, `app/ponto/services/dia_pago.py:244–257`, `app/ponto/tests/test_s5b_porta_unica.py:273–292,393–400`.
- **Escopo e controles.** Já havia um motor por fatia, e o mapa novo preserva seu contexto para a FT. BO-B(3) certa com `feriado_vira_folga=False` permanece em folga trabalhada, e há controles de mudança de praça, nacional versus municipal, folga com abono e abono residual. O v2 preserva a exceção CCT existente no dia normal 12x36, e a promoção da FT exclui o ciclo. Intermitente por base ou por ciclo fica fora no caminho de produção. L-097 continua capaz de zerar os minutos antes da partição para folga com escala certa e sem autorização. Reproduzi em memória os mesmos 240 min de FT 6x1 saindo com 0 min quando certa e 240 min quando incerta sob a guarda L-097. **RED:** controle com e sem autorização na janela da guarda, conforme a delimitação do aval. `app/ponto/services/fechamento.py:299–321`, `app/ponto/calculador/regras.py:366–401`, `app/ponto/calculador/alimentacao.py:529–573`, `app/core/regua_cct.py:473–479`, `app/ponto/motor_calculo_v2.py:1562–1597,3694–3698,3740,3764–3767`, `app/ponto/tests/test_feriado_um_juiz_porta.py:101–113,253–270,315–329,366–409`.
- **Idempotência e soma.** O arredondamento único cura o caso 241 + 241, inclusive em folga comum, que ultrapassa o universo literal do feriado. O teste cobre repetição sequencial dos valores. O mensal segue gravado antes das lavras, e a substituição por versão ainda faz `delete` seguido de `bulk_create`. **RED:** injetar falha entre eles e checar conservação do conjunto anterior, depois intercalar evento retroativo e exportação. `app/ponto/services/fechamento.py:963–965,1118–1172,1180–1222,1246–1283`, `app/ponto/services/dia_pago.py:344–359`, `app/ponto/tests/test_feriado_um_juiz_porta.py:289–298`.
- **Mordida.** Rodei sem escrita e sem banco 26 controles verdes e 22 mutações de produção com controle verde e depois falha de uma asserção. A mutação da fixture espera erro de unicidade e altera só o teste. Mutantes de porta e RED contra a base [nao verificado]. No DIFF, as duas colunas de FT leem a mesma projeção, e `> 0,02` deixa diferenças de um centésimo fora da contagem de divergências. `app/ponto/tests/feriado_um_juiz_mutantes.py:37–140,187–224`, `app/ponto/management/commands/diff_calculador.py:231–253,399–409,503–527,546–572`.

## 27/11 15:20 · sessao 71639322d2dc · FERIADO-BOTAO-CORTADO · construtor · origem=Code

Alterei apenas [core/tests/test_smoke_clique_feriado_vira_trabalho.py](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:516).

1. Mantive os nove cenários de 1366 px nas duas cascas, incluindo lateral, sem espaço e batida. [Cenários desktop](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:517)
2. **Lista do pouso** — saíram os quatro casos de 360 px (`base.html` e `base_app.html`, página e drawer), `esperado_360`, o retorno antecipado da sonda, `test_MORDE_painel_fora_da_viewport` e `painel-fora-360`. Ficou um comentário com o aval `BOTAO-POUSA-DESKTOP-2711-1510`. [Comentário do aval](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:516) [nao verificado]
3. `test_MORDE_painel_sobre_o_gatilho` seleciona a página de `base.html` a 1366 px. A regex exige painel dentro da viewport, distância zero e `painelEncostado: False`. O deslocamento do mutante foi preservado. [Mutação](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:252) · [Regex](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:453) · [Filtro](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:526)
4. Ajustei também `viewport-do-host`, que dependia da diferença entre 360 e 1366 px. Agora ele mede a 1365 px, e cada cenário confere a largura pedida. [Mutação de largura](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:419) · [Largura esperada](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:535)

Conferi a sintaxe, o diff e as asserções RED com medidas sintéticas. **A mordida real de `painel-sobre-gatilho` a 1366 px e o resultado da suíte no Chromium ficam [nao verificado].** [Asserções do smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:429) [nao verificado]

**Comandos para o Code**, a partir de `app/`. Os dois comandos com mutante devem produzir RED no teste principal. [Entrada do smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:438) · [Configuração de CI](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/config/settings/ci.py:6) [nao verificado]

```bash
alvo=core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest
principal="$alvo.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas"

smoke_feriado() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci \
    python3 -B manage.py test "$1" --noinput
}

SMOKE_FERIADO_MUTANTE= smoke_feriado "$alvo"
SMOKE_FERIADO_MUTANTE=painel-sobre-gatilho smoke_feriado "$principal"
SMOKE_FERIADO_MUTANTE=viewport-do-host smoke_feriado "$principal"
```

## 27/11 15:20 · sessao c8163c86514d · - · - · origem=Ronald (tmux)

**K1 continua procedente.** O RED exige 440 min de HE100 feriado e 30 min de intra no mensal e nas duas lavras. Em memória, com os dois períodos de folga injetados no laço do partido, obtive 470 min de HE100 e zero de intra no motor. O oráculo, alimentado com os mesmos 470 min e a pausa de 30 min, devolveu 470 min de HE100 e 30 de intra. A marca de falha esperada permite que esse RED falhe com o job verde. A causa individual registrada no job [nao verificado]. (`app/ponto/tests/test_feriado_um_juiz_porta.py:148–158,272–286`; `app/ponto/motor_calculo_v2.py:2993–3013,3044–3061,3081–3118`; `app/ponto/calculador/regras.py:384–409,768–778`; `app/ponto/services/fechamento.py:953–957,985`)

**Uma das 40 marcas veio da obra.** O censo AST do recorte encontrou 25 marcações diretas, incluindo a K1, e 15 do prazo de cron. O diff fornecido acrescenta a K1. As identidades executadas no job [nao verificado]. A K1 tem tag e uma condição que gera sucesso inesperado, mas o decorador tem vigência indefinida, enquanto a regra local exige prazo, tripwire e item de fila. (`app/ponto/tests/test_feriado_um_juiz_porta.py:272–286`; `app/core/tests/marca_cron15.py:27,32–65`; `app/docs/BACKLOG.md:2005`; `CLAUDE.md:201,274–277`)

**Mutantes.** Executei 26 controles sem banco e 22 mutantes de produção. Todos os controles passaram e cada mutante provocou a falha de asserção prevista. Também confirmei o mutante `endereco-c2`, que derruba a catraca quando se restaura a linha 500 no registro esperado, em vez de 505. Os seis mutantes de porta são cinco de produção e um da fixture. Execução deles e RED contra o commit-base [nao verificado]. (`app/ponto/tests/feriado_um_juiz_mutantes.py:38–143,169–223`; `app/core/tests/test_medida_turno_do_par.py:47–48,565–569`; `CLAUDE.md:375–379`)

**Dois REDs adicionais.**

- Com 4 h de HE100 feriado e 12x36 sem cláusula no fim da janela, reproduzi em memória 4 h no evento emitido e `total_extras=0` no cartão. A testemunha compara HE100 bruta e omite `total_extras`. RED com o TXT e o resumo real do mesmo 6x1 → 12x36, ambos exigindo 4 h. (`app/folha/export.py:425–448,744–749`; `app/relatorios/cartao_pela_celula.py:89–99,249–274,321–346`)
- No plantão de 26 h iniciado às 21h, reproduzi 1.560 min de split noturno promovido. A janela da própria regra cobre 1.500 min, das 22h às 23h de D+1. O minuto diurno inicial é compensado pela segunda contagem das 22h–23h de D+1. RED exige 1.500 min de split e adicional legal de `1.500/0,875`. O teste existente começa às 22h e verifica o limite superior. (`app/ponto/motor_calculo_v2.py:82–118,151–161,1027–1049`; `app/ponto/calculador/regras.py:402–409`; `app/ponto/tests/test_feriado_um_juiz.py:182–198`; `app/ponto/tests/test_feriado_um_juiz_porta.py:343–351`)

O K1 tem aval separado e condicional. O registro determina E1 antes de K1, exige DIFF de frota pré-pouso por rubrica na comp. 11 e PAREI se mudar rubrica fora de feriado de folga. A execução do pré-requisito e a evidência de pouso [nao verificado]. (`app/docs/RELATO.md:252–255`; `app/docs/PROMPTS.md:3016`)

## 27/11 15:19 · sessao 711d7f903f0c · - · - · origem=Ronald (tmux)

**REPROVADO.** A c2/c3 corrige a emissão e a homologação da primeira fatia na competência 11+, inclusive FT promovida como único trabalho. A montagem lê o mensal sem aplicar o ciclo final, e `cadastro_zero` libera o caso com rubrica configurada. Os REDs passam por classificador, montagem e serialização com mensal gravado e lido. (`app/folha/export.py:744–757,921–923,989–1000,1023–1037`; `app/ponto/tests/test_feriado_um_juiz_porta.py:164–177,215–227`)

**Procedentes nesta frente**

1. **Cartão e app ainda perdem o total de extras no 6x1 → 12x36.** Reproduzi em memória com as funções da cópia. Mensal com HE100 feriado de 4 h e ciclo final 12x36 sem cláusula produziu TXT `[400]`, `total_extras=0` no resumo e HE100 bruta de 4 h. O cálculo do resumo ainda passa o ciclo final, e o app e a ficha repassam essa chave. O PDF de badges emprega a HE100 bruta nesse caso. O contador `cartao_x_txt` compara contra o gravado pelo mesmo mapeamento antigo, usa a HE100 bruta e não observa a rubrica feriado nem `total_extras`. **RED:** estender `test_RED_txt_da_primeira_fatia_6x1` para comparar as 4 h da linha 0200 com o resumo e fazer o contador acusar uma rubrica de feriado suprimida com HE100 bruta igual. (`app/ponto/tests/test_feriado_um_juiz.py:11–39`; `app/ponto/tests/test_feriado_um_juiz_porta.py:215–219`; `app/relatorios/cartao_pela_celula.py:89–101,250–274,320–346`; `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51–75`; `app/api/views.py:1429–1433,1660–1669`; `app/colaboradores/services/ficha.py:198–205`; `app/relatorios/pdf_espelho.py:766–776`)

2. **Exportadas continuam expostas.** A partição entra em todo recálculo. A guarda só tem 09/2026, e nas outras exportadas a regra genérica registra o aviso e prossegue. O evento retroativo alcança mensal e lavras. O POST também pode gerar um novo TXT com o fechamento calculado em leitura mesmo na 09 guardada, pois a guarda não exclui leitura pura e o corte 11 atua na emissão, não na promoção. A L-113 permite a via genérica fora da guarda com condições. O aval desta obra veda tocar exportada. **RED:** em competência entregue fora da guarda, disparar evento retroativo e exigir mensal e duas lavras iguais. Em 09 guardada e 11 entregue, exercitar a via de geração comparando id, hash e conteúdo do TXT vigente. A 11 aberta deve promover no controle. Alteração efetiva de produção [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`; `app/docs/LEIS.md:156`; `app/ponto/services/fechamento.py:36–50,564–600,948–965,1118–1155,1172–1185,1246–1276,1780–1806`; `app/folha/views.py:214–219,269–275,302–307`; `app/folha/models.py:173–188`)

3. **O novo arredondamento muda o mensal fora do feriado.** O caso 241 min normais + 241 min de FT comum passa de 8,04 h para 8,03 h, com zero HE100 feriado. A correção da igualdade é verificável e o `horas_trabalhadas` gravado muda fora da folga em feriado prevista no aval. O TXT não emite `horas_trabalhadas` como rubrica, mas os critérios de aprovação e classificação e a prontidão leem esse campo. **RED:** testar a paridade com a base em dia comum, incluindo o caso de 241 + 241, e registrar o delta separadamente se houver aval para a correção. Efeito monetário de produção [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`; `app/ponto/services/fechamento.py:963–965`; `app/ponto/tests/test_feriado_um_juiz_porta.py:288–296`; `app/folha/export.py:444–457,850–851,921–923,1059–1063`)

**Travas e pouso.** POST e regerador usam a trava da empresa. A revisão específica ordena Empresa → Colaborador → Batida e consulta K6. O evento genérico via `on_commit` segue sem essa trava. O diff não introduz inversão, mas leva o novo valor a um caminho sem a serialização da entrega. **RED:** intercalar primeira entrega e evento retroativo e conferir valores, lavras e TXT vigente. (`app/folha/export.py:561–567`; `app/folha/views.py:103,132–135`; `app/folha/management/commands/regerar_txt_dominio.py:64–67`; `app/ponto/services/par_relampago_na_lista.py:172–187`; `app/ponto/services/fechamento.py:1780–1806,1812–1834`)

Faltam comprovar a linha de base dos 75 dias/42 colaboradores na 11 e exclusão dos intermitentes, DIFF por colaborador, horas e rubrica com TXT 11, reversão por snapshot em `logs/` antes, hash da exportada antes/depois, confirmação de que a 11 está aberta, apply, prova de **frota** depois no mesmo marco e PENDENTES com trilha e reversão. O recorte informado não demonstra a certificação da suíte inteira da árvore nem a leitura opus final exigida para mão única. Execução em produção [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`; `CLAUDE.md:260–265,274–277,295–299,347–351,421–431,451–467`) [nao verificado]

## 27/11 15:17 · sessao d62db9fabea1 · INVENTARIO-PELA-REGRA · leitura · origem=Code

# nascentes.md

Inventário do lote 2 entregue no chat, em leitura exclusiva. A identidade da cópia com o hash `59600163ccc9cffb8f3ae148beede7cd7866ae86` ficou [nao verificado]. As referências abaixo usam as linhas da cópia, que diferem das do anexo. Por exemplo, `ESMERIL-SEM-DIA-DO-TURNO` está em `app/docs/BACKLOG.md:2121`.

**Ainda vivo?** refere-se ao caminho de código citado. Ocorrências, estoques, volumes e execução em produção ficam [nao verificado] quando a sondagem fornecida não os demonstra. **Morre?** indica quanto da entrega desapareceria com a correção da origem, inclusive quando a cópia já mostra essa correção. O critério de origem é a autoridade de leitura ou a porta de escrita. `CLAUDE.md:20`, `CLAUDE.md:32`.

### Referências do placar

Na coluna `placar20`, **juiz** corresponde a `um juiz por pergunta` e **escritor** a `um escritor por entidade`. A indicação identifica o contrato aplicável à origem. As notas indicam quando o detalhe local fica fora do alcance do selo. `app/core/contratos_estruturais.py:55`, `app/core/contratos_estruturais.py:1038`.

| Família | Juiz | Escritor |
|---|---|---|
| batida | `app/core/contratos_estruturais.py:375` | `app/core/contratos_estruturais.py:91` |
| turno/marcos | `app/core/contratos_estruturais.py:225` | `app/core/contratos_estruturais.py:102` |
| célula/precedência | `app/core/contratos_estruturais.py:165` | `app/core/contratos_estruturais.py:113` |
| ausência/férias | `app/core/contratos_estruturais.py:247` | `app/core/contratos_estruturais.py:126` |
| escala | `app/core/contratos_estruturais.py:444` | `app/core/contratos_estruturais.py:139` |
| chamado | `app/core/contratos_estruturais.py:262` | `app/core/contratos_estruturais.py:496` |
| folha/export | `app/core/contratos_estruturais.py:353` | `app/core/contratos_estruturais.py:151` |

## Inventário

| id | nascente | sítio | morre? | ainda vivo? | placar20 | nota |
|---|---|---|---|---|---|---|
| ESMERIL-SEM-DIA-DO-TURNO | Dia do turno derivado fora do juiz | `app/ponto/turnos.py:3014`, `app/ponto/services/regularizacao.py:61`, `app/ponto/services/regularizacao.py:73` | sim | sim | turno/marcos · juiz | A regularização obtém o turno da autoridade e usa o dia civil da entrada na escrita. |
| TIPO-V5-NAO-BLOQUEANTES | Dia do turno derivado fora do juiz + Relógio do vigia de células relido no interior | `app/ponto/services/vigia_pareador_celula.py:109`, `app/ponto/management/commands/tripwire_celulas.py:85`, `app/ponto/management/commands/tripwire_celulas.py:247` | parcial por nascente | sim | turno/marcos · juiz para o dia, relógio fora do placar | A comparação civil e as leituras internas de tempo têm sítios distintos. |
| VIGIA-PAREADOR-DIA-CIVIL | Dia do turno derivado fora do juiz | `app/ponto/services/vigia_pareador_celula.py:97`, `app/ponto/services/vigia_pareador_celula.py:109` | sim | sim | turno/marcos · juiz | O vigia seleciona o turno por `data_turno` e procura a órfã na chave civil. |
| SAIDA-NOTURNA-NO-DIA-CERTO | Instante do marco noturno fora da cadeia + Colisão do plantio sem prova da lâmpada | `app/chamados/juizes.py:301`, `app/chamados/services/disputa_emissao.py:1222`, `app/chamados/services/disputa_emissao.py:1260`, `app/chamados/services/materializacao.py:1790` | parcial por nascente | sim nos sítios, caso [nao verificado] | turno/marcos · juiz, chamado · escritor | O texto usa o dia da cobrança. O outro ramo carimba colisão sem a prova usada no ramo de paridade. |
| [nome]-NOTURNO-DIA-CIVIL | Dia da pergunta gravado fora da porta do dia do turno | `app/chamados/models.py:3179`, `app/chamados/models.py:3192`, `app/chamados/services/fio.py:1963`, `app/docs/BACKLOG.md:1196` | parcial | não no nascimento com batida de origem, acervo [nao verificado] | chamado · escritor, turno/marcos · juiz | Identificação e retratação do estoque são atos próprios. |
| FICHA-LE-O-CARIMBO | Vínculo do dia sem a célula geradora | `app/colaboradores/services/ficha.py:126`, `app/colaboradores/services/ficha.py:129`, `app/escala/alimentacao.py:228` | parcial | sim | escala · juiz | A ficha consulta a autoridade sem alimentar o carimbo nesse trecho. O teto de custo continua sendo uma entrega. |
| ABSOLVIDA-PELA-FOLGA | Dia da pergunta gravado fora da porta do dia do turno | `app/chamados/services/fio.py:1139`, `app/chamados/services/fio.py:1963`, `app/chamados/reconciliador.py:373`, `app/docs/BACKLOG.md:2423` | parcial | não no nascimento com batida de origem, acervo [nao verificado] | chamado · escritor, turno/marcos · juiz | Absolvições antigas e exportadas ainda exigem decisão sobre o estoque. |
| BUG-PLANTIO-NOTURNO-MAIS-1 | Dia da pergunta gravado fora da porta do dia do turno | `app/chamados/models.py:3192`, `app/chamados/services/disputa_emissao.py:393`, `app/chamados/juizes.py:311`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:416` | parcial | não no ramo com origem, migração [nao verificado] | chamado · escritor, turno/marcos · juiz | A data do episódio tem um significado no nascimento. A migração do legado é separada. |
| CONTRATO-DE-ENTRADA-DO-FIO | Batida de origem fora do envelope da pergunta | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`, `app/chamados/services/disputa_emissao.py:300`, `app/chamados/models.py:3183`, `app/chamados/models.py:3914` | parcial | não no emissor conferido, alcance dos demais [nao verificado] | chamado · escritor | O emissor conferido entrega a batida do ato. |
| PLANTIO-SILENCIO-SEM-DIA | independente | `app/chamados/services/disputa_emissao.py:433`, `app/chamados/services/disputa_emissao.py:439` | não | sim | fora do placar | A saída sem dia já é decidida. A entrega é dar destino visível à recusa hoje registrada em log. |
| SELFIE-RETRO-SEM-ULTIMA-BATIDA | Batida de origem fora do envelope da pergunta | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`, `app/api/foto_recebida.py:38`, `app/api/foto_recebida.py:99` | parcial | não para o nascimento pelo emissor conferido, legado [nao verificado] | chamado · escritor | A recepção usa a origem do episódio. O caso legado sem origem tem tratamento próprio. |
| E4B-D2-ANCORA-RAIOX | Âncora do fio recalculada no Raio-X | `app/chamados/services/raiox.py:149`, `app/chamados/services/raiox.py:157`, `app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:117` | sim | sim | chamado · juiz | O diagnóstico troca o destino pelo pai conforme `VIVOS`. O fio tem sua própria resolução da âncora. |
| E4B-D3-ARQUIVAR-PRESCRITOS | independente | `app/config/crons.py:1477`, `app/chamados/management/commands/arquivar_prescritos.py:47` | não | não sei [nao verificado] | fora do placar | A obra mede pendências sem dono. A configuração marca o cron como desligado. |
| E4B-FILA-OPUS-C14 | Dia da pergunta lido cru no backfill | `app/chamados/management/commands/backfill_fio_morto_e4b.py:188`, `app/chamados/juizes.py:339`, `app/chamados/services/disputa_emissao.py:1955` | parcial | sim | chamado · juiz, C6/C20 | O reparador ainda abre a data diretamente. A fila também trata identidades e provas. |
| C6-RELOGIO-BORDA | Relógio da pergunta relido no interior | `app/chamados/juizes.py:934`, `app/chamados/juizes.py:1569`, `app/chamados/reconciliador.py:1210`, `app/chamados/reconciliador.py:1214` | sim | sim | fora do placar | O reconciliador já tem `agora`. Uma chamada a C6 ainda usa o relógio implícito. C6 é contrato de chamado, a propagação do instante é o resíduo. |
| CAL-DOIS-RELOGIOS | Relógio do calendário lido duas vezes | `app/colaboradores/services/calendario.py:192`, `app/colaboradores/services/calendario.py:279` | sim | não | fora do placar | O calendário e o retrato usam o mesmo `_agora`. |
| DETECTOR-SO-A-1A-PAUSA | Marcos de pausa truncados fora da sequência completa | `app/chamados/detector_anomalias.py:199`, `app/escala/servico_jornada.py:76`, `app/escala/models.py:1177` | parcial | sim | turno/marcos · juiz | A cadeia lida pelo detector leva uma pausa. A autoridade da célula expõe todas. O item do papel da batida é adicional. |
| F5-RELOGIO-DO-EXPORT | Relógio da retenção relido na classificação | `app/folha/fora_da_folha.py:137`, `app/folha/export.py:977`, `app/folha/export.py:1247` | sim | sim | fora do placar | O juiz da retenção integra folha/export. O resíduo é a classificação perder o `hoje` que a outra leitura já recebeu. |
| LASTRO-PARTE-2 | Fecho de cobrança pelo marco fora do evento da célula | `app/ponto/services/lastro.py:364`, `app/ponto/services/lastro.py:394`, `app/chamados/reconciliador.py:1533`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:49` | sim | não | chamado · juiz e escritor | O evento já reconcilia. O comando opera como vigia. |
| LASTRO-PARTE-2-FILA-OPUS | Lâmpada acesa tomada por batida | `app/ponto/services/lastro.py:327`, `app/ponto/services/lastro.py:340`, `app/ponto/services/diagnostico_escala.py:155`, `app/ponto/status_dia.py:186` | parcial | sim | célula/precedência · juiz | Ainda há leitores que usam o sinal ou a hora da lâmpada como prova. Gatilho, trilha e testes têm escopo adicional. |
| O127 | Marcos de pausa truncados fora da sequência completa | `app/ponto/services/flip_auto.py:201`, `app/ponto/services/flip_auto.py:218`, `app/escala/models.py:1177` | sim | sim | turno/marcos · juiz | O flip lê as pausas e ainda usa só a primeira ao montar o slot. |
| PAR-NAO-ADJACENTE | Papel do par descartado pela exigência de adjacência | `app/escala/utils.py:444`, `app/escala/utils.py:467`, `app/ponto/turnos.py:502`, `app/ponto/turnos.py:549` | sim | sim | turno/marcos · juiz | O alinhador exige que as pontas sejam vizinhas mesmo após o pareador reconhecê-las. |
| R1 | Papel do par fora da ata | `app/escala/utils.py:606`, `app/escala/utils.py:646`, `app/ponto/turnos.py:1565`, `app/docs/BACKLOG.md:1310` | parcial | não sei para o caso restante [nao verificado] | turno/marcos · juiz | O alinhamento conserva ramo por distância. A incidência dele no caso residual ficou [nao verificado]. |
| RELOGIO-NA-BORDA | Relógio do cadastro relido no interior | `app/colaboradores/services/vinculo.py:194`, `app/colaboradores/services/pos_vinculo.py:56`, `app/escala/signals.py:67`, `app/escala/signals.py:119` | sim | sim | fora do placar | A borda e os serviços internos consultam o tempo no mesmo fluxo de cadastro. |
| RELOGIO-NA-BORDA-OPUS-C5E | Relógio do cadastro relido no interior | `app/escala/management/commands/corrigir_vinculo_vigencia.py:48`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`, `app/colaboradores/services/vinculo.py:742`, `app/ponto/portas/celula.py:482` | parcial | sim para relógio | fora do placar | A porta da célula já recebe `hoje`. A fila também inclui transação e prova da catraca. |
| RETRATO-DOIS-RELOGIOS | Relógio da retenção relido na classificação | `app/folha/fora_da_folha.py:125`, `app/folha/fora_da_folha.py:137`, `app/folha/export.py:977`, `app/folha/export.py:1247` | sim | sim | fora do placar | O retrato lê os dias com o `hoje` da borda. A classificação da mesma competência pode usar outra data. |
| TETO-NA-LISTA-DOIS-RELOGIOS | Relógio do teto da lista relido na medição | `app/ponto/management/commands/lavrar_teto_do_dia.py:27`, `app/ponto/services/teto_na_lista.py:49`, `app/ponto/services/teto_na_lista.py:72` | sim | sim | fora do placar | O comando passa a data e a medição obtém o próprio instante para os insumos. |
| PASSIVO-VIVA-EM-DISPUTA-FECHADA | independente | `app/chamados/models.py:2047`, `app/chamados/models.py:2122`, `app/chamados/services/validacao.py:830`, `app/docs/BACKLOG.md:1210` | não | não sei [nao verificado] | fora do placar | É tratamento de acervo. A possibilidade de fechar e a vitalidade têm autoridades nas portas atuais. |
| ATA-X-TURNO-PAUSA | Papel do par fora da ata | `app/ponto/services/cartorio.py:1205`, `app/escala/utils.py:467`, `app/escala/utils.py:588`, `app/escala/utils.py:1748`, `app/ponto/turnos.py:1850` | parcial | sim para realinhamento residual, caso [nao verificado] | turno/marcos · juiz | O montador já lê o pareador. A guarda do marco mais próximo ainda pode retirar a batida de um marco. |
| POSICAO-TOTAL-ATE-A-DATA | Total do espelho até a data sem juiz | `app/ponto/services/espelho.py:1230`, `app/relatorios/cartao_pela_celula.py:296`, `app/ponto/services/dia_pago.py:752`, `app/docs/BACKLOG.md:1181` | sim | sim | fora do placar | Falta o total parcial com a semântica do resumo do espelho. |
| O103 | independente | `app/ponto/portas/celula.py:112`, `app/ponto/portas/celula.py:765`, `app/docs/BACKLOG.md:1382` | não | não no recorte de código, TXT [nao verificado] | fora do placar | O modo dia usa a mesma porta com horizonte limitado. A retificação do TXT é outro ato. |
| FECHAMENTO-ZERO-COM-BATIDA | independente | `app/folha/export.py:968`, `app/ponto/services/espelho.py:1007`, `app/docs/BACKLOG.md:1419` | não | não sei [nao verificado] | fora do placar | É encaminhamento de cadastro sem jornada descrita ou de batida sem par. |
| O238 | Testemunha do fechamento por carimbo e hash duplicado | `app/ponto/management/commands/carimbo_gravado.py:29`, `app/ponto/portas/regen_exportada.py:37`, `app/ponto/portas/regen_exportada.py:204`, `app/ponto/services/fechamento.py:1637` | parcial | sim | fora do placar | O instrumento define a própria foto para hash. O identificador da trilha é outro item. |
| O145 | Ponta de HE julgada pela grade | `app/ponto/janela_he.py:203`, `app/ponto/services/espelho.py:741`, `app/ponto/services/espelho.py:754`, `app/docs/BACKLOG.md:1302` | parcial | não na origem, resíduos [nao verificado] | fora do placar | O espelho já recebe a ponta órfã do motor. A pergunta específica de HE está fora do selo de geometria. |
| QUARENTENA-DO-COLAB | Quarentena de cobrança por dia sem autoridade e porta | `app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:188`, `app/chamados/models.py:527`, `app/docs/BACKLOG.md:900` | parcial | sim como lacuna | fora do placar | Há silêncio por chamado. O estado por pessoa e período pedido pela obra carece de representação e porta próprias. |
| BO-354 | Papel do par fora da ata | `app/chamados/detector_anomalias.py:31`, `app/ponto/utils_turno_orfao.py:20`, `app/ponto/utils_turno_orfao.py:31`, `app/ponto/turnos.py:1850` | parcial | sim no leitor, três casos [nao verificado] | turno/marcos · juiz | O emissor delega, mas o helper usa pareamento genérico sem marcos do vínculo. |
| CORTE-B-30 | independente | `app/ponto/management/commands/aplicar_09_corte_b.py:228`, `app/ponto/management/commands/aplicar_09_corte_b.py:239`, `app/docs/BACKLOG.md:1299` | não | não sei [nao verificado] | fora do placar | É lista de explicações do lote. O comando já declara as guardas de separação. |
| DESLIGAMENTO-NAO-CARIMBA-RESPONDIDA | Destino da respondida no desligamento sem ato próprio | `app/chamados/models.py:2044`, `app/chamados/models.py:2123`, `app/colaboradores/services/desligamento.py:165`, `app/docs/BACKLOG.md:1211` | não, solução substituída | não para o carimbo apontado | chamado · escritor | O caminho atual preserva a resposta. A demanda vigente está na sucessora. |
| DESLIGAR-LIQUIDA-TUDO | Destino da respondida no desligamento sem ato próprio | `app/colaboradores/services/desligamento.py:164`, `app/colaboradores/services/desligamento.py:187`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, `app/chamados/models.py:2123` | parcial | sim | chamado · escritor | Manual e cron convergem na porta. A guarda preserva a resposta, sem o ato específico de encaminhamento previsto na obra. |
| O210-COL417-COL931 | independente | `app/ponto/management/commands/recalcular_fechamento.py:110`, `app/ponto/services/fechamento.py:468`, `app/docs/BACKLOG.md:1213` | não | não sei [nao verificado] | fora do placar | É reapuração de dois gravados antigos pela porta existente. |
| O232 | Papel do par fora da ata | `app/escala/utils.py:1999`, `app/escala/utils.py:2053`, `app/escala/tests/test_saida_valida_cross_meia_noite.py:60`, `app/config/settings/base.py:165` | parcial | não no ramo padrão para o caso descrito | turno/marcos · juiz | O ramo usa a saída do turno fechado e a remove do dia seguinte. Impacto na frota [nao verificado]. |
| O73b | Volta de pausa julgada como nova entrada | `app/ponto/motor_calculo_v2.py:2777`, `app/ponto/motor_calculo_v2.py:2805`, `app/ponto/motor_calculo_v2.py:2822`, `app/ponto/tests/test_o73_volta_do_intervalo.py:60` | parcial | não na origem, gravado de 09 [nao verificado] | turno/marcos · juiz | O motor agrupa pelo dia da jornada e zera o atraso dos blocos internos. |
| POSICAO-PORTA-POR-MOTIVO | Porta da retenção por motivo sem projeção comum | `app/folha/fora_da_folha.py:278`, `app/folha/fora_da_folha.py:302`, `app/folha/retrato_folha.py:1024`, `app/chamados/services/tarefa_do_dia.py:809` | parcial | sim para motivo geral | chamado · juiz C19 para a ação do dia, motivo geral fora do placar | A linha diária recebe porta. O motivo geral é publicado sem a mesma projeção de ação. |
| SITUACAO-LEITOR-CRU | Desligamento derivado fora do juiz | `app/colaboradores/services/desligamento.py:23`, `app/ponto/services/cartorio.py:1803`, `app/ponto/services/cartorio.py:1868`, `app/ponto/services/vigia_de_hora.py:166`, `app/core/juizes.py:283` | sim | sim | ausência/férias · juiz | Consumidores ainda leem `situacao` diretamente. |
| COL882-O-PROXIMO-COL515 | independente | `app/ponto/supra_juiz.py:155`, `app/ponto/services/vinculo_na_lista.py:32`, `app/docs/BACKLOG.md:1274` | não | não sei [nao verificado] | fora do placar | É correção de cadastro para dias sem cobertura de vínculo. |
| O143 | independente | `app/ponto/supra_juiz.py:467`, `app/chamados/services/fabrica_da_celula.py:317`, `app/chamados/services/fabrica_da_celula.py:365`, `app/docs/BACKLOG.md:1535` | não | não sei para o encaminhamento com prazo [nao verificado] | fora do placar | A classe já tem julgamento e escritor de pauta. A entrega é fluxo administrativo. |
| BO-0611-05-DESLIGADO-FERIAS | independente | `app/api/views.py:394`, `app/api/views.py:405`, `app/docs/BACKLOG.md:2324` | não | não sei para o efeito financeiro pedido [nao verificado] | fora do placar | O comportamento observado é recusa do registro. A obra pede decisão sobre o tratamento financeiro. |
| E4-8-ROTULO-OK | independente | `app/ponto/status_dia.py:31`, `app/ponto/status_dia.py:95`, `app/ponto/portas/celula.py:1061`, `app/docs/BACKLOG.md:2518` | não | sim | fora do placar | A palavra `ok` ainda vem com o texto de que todas as batidas foram registradas. |
| EMIS-FALSE-ATA-TRABALHO | Lavra sem invariante entre `trabalha` e `tipo_dia` | `app/ponto/services/cartorio.py:757`, `app/ponto/services/cartorio.py:1470`, `app/escala/utils.py:1499`, `app/escala/utils.py:1857`, `app/chamados/juizes.py:503` | parcial | sim para a combinação no caminho intermitente, estoque [nao verificado] | célula/precedência · escritor, chamado · juiz C18 | A grade intermitente pode usar batida para classificar trabalho. A cobrança aplica seu juiz sobre ambos os insumos. |
| EMIS-PAUTA-DEDUP | Pauta do limbo sem unicidade atômica | `app/chamados/services/fabrica_da_celula.py:251`, `app/chamados/services/fabrica_da_celula.py:277`, `app/chamados/services/fabrica_da_celula.py:365`, `app/pautas/models.py:123` | sim | sim | fora do placar | Uma consulta anterior à escrita pode ser repetida por outro julgamento. A trava de outra pauta dá o contraste no próprio código. |
| F3-PORTA-E-CRON-PRAZO | independente | `app/config/crons.py:822`, `app/config/crons.py:837`, `app/config/crons.py:2040`, `app/ponto/services/espelho.py:1065`, `app/docs/BACKLOG.md:1931` | não | não sei quanto ao estoque [nao verificado] | fora do placar | É reencaminhamento e organização de tarefas. A cópia tem os crons e a injeção de feriados. |
| ISENTO-H10-MENORES | Isenção corrente lida fora do predicado | `app/core/constants.py:13`, `app/ponto/services/cartorio.py:954`, `app/ponto/services/cartorio.py:1303`, `app/chamados/veredito_lavrado.py:231`, `app/docs/BACKLOG.md:1077` | parcial | sim | fora

[truncado em 20000 de 88507 caracteres]

## 27/11 15:16 · sessao 4b6ad359cc31 · INVENTARIO-PELA-REGRA · leitura · origem=Code

# nascentes.md — INVENTARIO-PELA-REGRA v2, parte 3, lote 1

Inventário dos **179 IDs fornecidos**. São 158 linhas do bloco principal e 21 complementos que também vieram no anexo. Quando o ponteiro da GRADE estava deslocado, localizei a entrada pelo ID. Usei a cópia disponível em modo somente leitura. A correspondência da cópia com o hash `59600163ccc9cffb8f3ae148beede7cd7866ae86` fica [nao verificado]. `app/docs/GRADE-MESTRA.md:140`, `app/docs/GRADE-MESTRA.md:297`, `app/docs/GRADE-MESTRA.md:548`, `app/docs/GRADE-MESTRA.md:596`, `app/docs/GRADE-MESTRA.md:663`.

**Morre?** indica o alcance da cura da nascente sobre a entrega da obra. **Ainda vivo?** se refere ao ramo identificado no código da cópia. Passivo, incidência atual, tempos, deploy e smoke ficam [nao verificado] quando exigem banco, logs ou execução. Apliquei os critérios de juiz e escritor de `CLAUDE.md:14`. Os caminhos das tabelas são relativos a `app/`. [nao verificado]

**Placar 20.** `J` é **um juiz por pergunta**, `E` é **um escritor por entidade** e `P` é **parâmetro consumido ou sem efeito**. A referência é a célula declarada da matriz. Onde a célula cobre a pergunta-mãe, a nota identifica a lacuna específica do leitor ou da porta. `core/contratos_estruturais.py:55`, `core/contratos_estruturais.py:90`, `core/contratos_estruturais.py:1038`.

## Obras 1–54

| ID | Nascente | Sítio | Morre? | Ainda vivo? | Placar 20 | Nota |
|---|---|---|---|---|---|---|
| FERIADO-ESCALA | padrão de feriado ao vincular | `escala/models.py:905` + `colaboradores/services/vinculo.py:297` | parcial | não no vínculo novo | fora do placar | A porta usa o padrão por cargo, posto e tipo. Ele não reclassifica vínculos antigos. `escala/models.py:908` |
| HJ-092 | independente | `chamados/services/materializacao.py:539`, `:595`, `:603` | não | sim para o ramo de divergência | fora do placar | A entrega é encaminhar a decisão humana. O materializador já distingue marco cobrado e marco da ata. `docs/BACKLOG.md:797` |
| LANCAR-AUSENCIA-AFASTA-DESLIGADO | independente | `ponto/views.py:2542`, `:2569` + `colaboradores/services/colaborador.py:56` | não | não no POST lido | fora do placar | Guarda de elegibilidade. A situação é validada pela porta antes da escrita. |
| PILHA-DA-BATIDA-RECALCULO-INLINE | independente | `ponto/registro_batida.py:197` + `ponto/services/fechamento.py:1831`, `:1848` | não | sim | fora do placar | O recálculo pode ocorrer no caminho da requisição. A latência fica [nao verificado]. |
| O-BITEMPORAL-2 | independente | `docs/BACKLOG.md:814` + `escala/models.py:1473` | não | não sei [nao verificado] | fora do placar | Entrega transversal de histórico de vigência e de registro. A obra usa 20/20 como pré-requisito. |
| IA-DOMINIO-POR-FERRAMENTA | permissão por ferramenta sem juiz aplicado | `core/ia_acoes.py:22`, `:53`, `:69` + `core/views_ia.py:859` | sim | sim | fora do placar | O gate de entrada usa a união das ações. Falta aplicar a permissão da ferramenta à identidade que pede a resposta. |
| REJULGAR-COMP10-4-REVERTIDOS | independente | `docs/BACKLOG.md:858` | não | não sei [nao verificado] | fora do placar | Ato sobre o passivo e comparação de resultados. A causa residual fica [nao verificado]. |
| TROCA-DE-POSTO-SEM-VINCULO | troca de posto sem porta única do ato | `colaboradores/views.py:337`, `:1415` + `escala/alimentacao.py:228` | sim | sim | fora do placar | Os caminhos de posto alteram o cadastro. A autoridade do vínculo do dia continua separada. O escritor de EscalaColaborador é coberto por `core/contratos_estruturais.py:139`. |
| LAVRAR-HE-PENDENTE-ESCALA | independente | `ponto/services/fechamento.py:152`, `:293` + `escala/models.py:1109`, `:1221` | não | não para a recarga corrigida | fora do placar | Reuso da alimentação no cálculo. Custo dos leitores restantes [nao verificado]. |
| N1-ALIMENTACAO-UNICA | independente | `folha/fora_da_folha.py:62` + `chamados/services/vigia_fio.py:600`, `:618` | não | não sei [nao verificado] | fora do placar | Obra de carga compartilhada entre leitores. `docs/BACKLOG.md:890` |
| BO-ADMIN-2710 | independente | `docs/BACKLOG.md:893` | não | não sei [nao verificado] | fora do placar | Triagem histórica. A linha remete cada ocorrência à obra responsável. |
| TRILHA-DO-VINCULO-MENTE | trilha de edição do vínculo sem dono do ato | `colaboradores/services/vinculo.py:307`, `:356`, `:381` | parcial | não no núcleo | E escala · `core/contratos_estruturais.py:139` | A porta distingue criação de edição e registra as fotos. O residual RT-03 fica [nao verificado]. `docs/BACKLOG.md:894` |
| REGISTRADO-NAO-E-VIVO | independente | `chamados/catalogo/motor.py:24` + `chamados/services/painel.py:316` | não | não na lista padrão | fora do placar | A lista padrão distingue cobrança e histórico. O complemento é de fiscalização. `docs/BACKLOG.md:896` |
| SILENCIO-VENCIDO-VOLTA | fila de cobrança com segundo juiz | `chamados/services/painel.py:189`, `:316`, `:331` + `chamados/services/verbos.py:47` | sim | sim | fora do placar | `qs_na_fila` volta a incluir o silêncio vencido. A lista ainda o exclui como histórico. |
| FECHAR3-SOBREVIVENTE-NO-RECONCILIAR | sobrevivente do dedup decidido por ordem | `chamados/models.py:807`, `:827` + `chamados/juizes.py:2352` | sim | não | J chamado, C8 · `core/contratos_estruturais.py:262` | A porta protege o sobrevivente que o juiz designa. `core/juizes.py:300` |
| V-FASE-AS-OF-TRANSFERIDO | empresa do dia de fase lida pelo cadastro atual | `ponto/management/commands/e6_oraculo.py:178` + `escala/services/fase_12x36.py:113`, `:146` + `escala/alimentacao.py:228` | sim | sim | J escala, vínculo do dia · `core/contratos_estruturais.py:444` | A busca histórica recebe a empresa atual. A cobertura `as of` da lavra, por si, fica fora da célula do placar. |
| V-FASE-AS-OF-PARIDADE-UNIVERSO | previsão do censo de fase em dois universos | `ponto/management/commands/censo_fase_12x36.py:176`, `:203`, `:212` | sim | sim | J célula/precedência, previsto · `core/contratos_estruturais.py:165` | O cenário atual lê célula e folga. O proposto deriva do template. A paridade hipotética fica fora do selo específico. |
| V-FASE-AS-OF-CUSTO | independente | `escala/services/fase_12x36.py:97`, `:109`, `:132` | não | sim para a busca | fora do placar | Procura lavras até cobrir o universo ou esgotar o iterador. O custo efetivo fica [nao verificado]. |
| DEGRAU-HE-100-CADASTRO | degrau de HE100 reproduzido | `ponto/calculador/periodo.py:173`, `:376` + `ponto/calculador/regras.py:825` + `ponto/motor_calculo_v2.py:3270`, `:3405` | parcial | sim | fora do placar | O limiar 120 aparece em mais de um cálculo. O novo cadastro é parte própria da entrega. `docs/BACKLOG.md:1021` |
| V-FASE-AS-OF-SEM-CELULA | dia sem célula não tem carimbo de fato novo | `escala/services/fase_12x36.py:174`, `:193` + `ponto/services/cartorio.py:2078`, `:2095` | sim | sim | fora do placar | A reabertura depende de carimbos das células existentes. |
| V-FASE-AS-OF-ATRASO | independente | `config/crons.py:616`, `:637` + `escala/services/fase_12x36.py:188` | não | sim na dependência do cron | fora do placar | Coordenação e frescor. Falta dependência direta da lavra do cartório. O atraso efetivo fica [nao verificado]. |
| V-FASE-AS-OF-SOBREPOSTA | cobertura de lavra de fase em duas chaves | `escala/services/fase_12x36.py:113`, `:137`, `:177`, `:193` | sim | sim | fora do placar | A leitura reconhece cobertura por dia. A reabertura procura a tripla exata da janela. |
| V-FASE-AS-OF-INTRADIA | lavra de fase sem escritor de versões intradia | `escala/services/fase_12x36.py:85`, `:91`, `:107` | sim | sim | fora do placar | A escrita substitui o snapshot pela chave `data_ref` no mesmo dia. |
| PROPOSITOR-JANELA-LONGA | fase histórica lida da última lavra | `escala/services/propositor.py:416`, `:490` + `escala/services/fase_12x36.py:117`, `:197` | sim | sim | fora do placar | O propositor confronta uma janela arbitrária com os itens da última lavra. |
| FILA-POR-CAUSA-AS-OF | fase histórica lida da última lavra | `chamados/services/fila_por_causa.py:184`, `:214`, `:230` + `escala/services/fase_12x36.py:117`, `:197` | sim | sim | fora do placar | A fila agrupa pela identidade da última lavra. A autoridade `as of` responde por dia. |
| GRADE-09-PAUTA | independente | `docs/BACKLOG.md:1072` + `ponto/services/cartorio.py:2078` | não | não sei [nao verificado] | fora do placar | Medida e entrega ao DP da diferença do passivo. |
| NOREVERSE-UI-NO-CORE | independente | `pautas/services.py:416`, `:436` | não | não no sítio lido | fora do placar | A ponte de tela resolve a URL fora do ramo que já gravou. A verificação da corrente fica [nao verificado]. `docs/BACKLOG.md:1085` |
| ISENTO-FLAG-ATEMPORAL | isenção por dia sem autoridade temporal | `colaboradores/models.py:294` + `core/constants.py:13` + `ponto/services/cartorio.py:567`, `:1219`, `:1438` | sim | sim | fora do placar | A flag atual entra na impressão de dia antigo. O efeito no passivo fica [nao verificado]. |
| GRADE-FOLGA-C5 | folga lançada fora do juiz | `chamados/management/commands/auditar_invariantes_chamados.py:1559` + `ponto/precedencia.py:212`, `:222` | sim | sim | J célula/precedência · `core/contratos_estruturais.py:165` | A auditoria usa qualquer vínculo que cubra a data. O juiz distingue calendário do vínculo e folga pessoal. `core/juizes.py:212` |
| GRADE-FOLGA-GERADOR | folga lançada fora do juiz | `escala/models.py:1263`, `:1297`, `:1321` + `ponto/precedencia.py:212`, `:222` | sim | sim | J célula/precedência · `core/contratos_estruturais.py:165` | O gerador lê a foto do próprio vínculo e pode deixar de ver uma folga pessoal lançada no anterior. |
| P14-CONFIRMAR-IMPLAUSIVEL-SEM-MOTIVO | motivo de confirmação implausível sem guarda na porta | `chamados/services/validacao.py:697`, `:773`, `:787` + `chamados/services/declaracao_texto.py:738` | sim | sim | E chamado, lacuna na guarda do ato · `core/contratos_estruturais.py:496` | A confirmação booleana permite o ato sem exigir o motivo escrito. |
| PERGUNTA-VIVA-LOGA-PREVISTO-COMO-FALHA | independente | `chamados/juizes.py:360`, `:411` + `chamados/services/materializacao.py:595` | não | sim | fora do placar | Divergência prevista registrada como erro. Volume atual [nao verificado]. |
| JUIZ-UNICO-DA-ANCORA | âncora sugerida 12x36 sem juiz único | `escala/services/fase_12x36.py:27` + `escala/services/fase_na_tela.py:50` + `ponto/management/commands/censo_fase_12x36.py:203` + `escala/services/propositor.py:308`, `:333` | sim | sim | fora do placar | Censo, tela e propositor têm derivações próprias. O contrato da escala registra duas perguntas de vínculo. `core/juizes.py:1334`, `core/contratos_estruturais.py:444` |
| BO-[nome]-DESTRAVA | dono de cadastro não chega ao destrava da folha | `ponto/management/commands/e6_oraculo.py:53`, `:98` + `folha/fora_da_folha.py:275` + `folha/retrato_folha.py:1079` | parcial | sim para a lacuna da projeção | fora do placar | A projeção da porta não recebe o dono de cadastro do dia. Texto e caminho de tela fazem parte da obra. `docs/BACKLOG.md:1135` |
| FIO-DO-ADMIN-LAVRA-VELHA | independente | `chamados/services/vigia_fio.py:273` + `escala/services/cadastro_realidade.py:430` + `ponto/services/teto_na_lista.py:120` | não | sim | fora do placar | Falta de aviso de frescor nesses leitores. Incidência atual [nao verificado]. |
| AVISO-ATIVO-SEM-VINCULO-NO-DIA | aviso consulta existência de escala em vez do vínculo do dia | `inteligencia/management/commands/gerar_alertas.py:113` + `ponto/supra_juiz.py:196` + `escala/alimentacao.py:228` | parcial | sim no aviso lido | J escala, vínculo do dia · `core/contratos_estruturais.py:444` | A consulta de existência pode responder quando nenhuma escala cobre aquele dia. Emitir o aviso é parte própria da entrega. |
| REABRIR-DISPUTA-ADMIN-IGNORA-RETORNO | independente | `chamados/services/acoes_disputa.py:299`, `:340` + `chamados/models.py:1453` | não | sim | fora do placar | O chamador pode ignorar a recusa do escritor e emitir sucesso. |
| CXR-FALHA-CALADA | independente | `escala/services/cadastro_realidade.py:968`, `:1014`, `:1027` | não | sim | fora do placar | Alguns braços registram exceção e seguem sem indicar indisponibilidade na lista. |
| COL309-SETEMBRO | independente | `ponto/portas/celula.py:112`, `:214` + `docs/BACKLOG.md:1159` | não | não sei [nao verificado] | fora do placar | Ato nominal de regeneração por uma porta existente. O recálculo apareceu como complemento separado. |
| CAL-CUSTO-NO-MAPA-DE-FUROS | independente | `relatorios/furos.py:127`, `:141` | não | sim para a leitura por colaborador | fora do placar | Há leitura de grade por colaborador elegível. O custo fica [nao verificado]. |
| ENTRADA-BLOQUEADA-SEM-DATA-TURNO | nascimento de chamado sem dia do fato | `ponto/views.py:294`, `:320` + `chamados/catalogo/modulos.py:182`, `:219` | sim | sim | J chamado, C5 · `core/contratos_estruturais.py:262` | O contexto de nascimento carrega pendências e total. Falta o `data_turno` que o juiz procura. |
| POSICAO-FALTAS-E-AUSENCIAS | posição tipada de faltas e ausências sem juiz | `folha/export.py:169` + `ponto/turnos.py:2560`, `:2816` | sim | não sei [nao verificado] | fora do placar | Existem autoridades separadas de falta e cobertura. A composição pedida não apareceu nos sítios lidos. `docs/BACKLOG.md:1182` |
| POSICAO-FERIAS-ATE-A-DATA | férias proporcionais por data sem juiz | `ferias/situacao.py:138`, `:167` + `ferias/models.py:108` | sim | sim na interface lida | fora do placar | A consulta recebe o dia de referência e lista períodos. Falta apurar os avos até esse dia. `docs/BACKLOG.md:1183` |
| BO2910-ESTRUTURA | troca de posto sem porta única do ato + outras classes [nao verificado] | `colaboradores/views.py:337`, `:1415` + `chamados/reconciliador.py:335` + `ponto/services/ausencia.py:1099` + `colaboradores/services/desligamento.py:235` + `ponto/portas/celula.py:1015` | parcial | sim para o posto | fora do placar para o posto | O item 01 compartilha a origem. Os demais tratam módulo de chamado, período de ausência, vigência e veredito humano. A atribuição de cada ocorrência fica [nao verificado]. `docs/BACKLOG.md:1191` |
| GRADE-DEPENDE-DA-JANELA | montagem da grade decide o dia pela janela | `escala/utils.py:1322`, `:1384`, `:1824`, `:2138` + `ponto/services/cartorio.py:1202` | sim | sim para a dependência do insumo | J célula/precedência e J turno/marcos · `core/contratos_estruturais.py:165`, `:225` | A alocação de batidas depende dos limites da chamada. Reprodução do caso nominal e selo específico de invariância [nao verificado]. `docs/BACKLOG.md:1200` |
| PAUTAS-PREFILTRO-REPETIDO | independente | `ponto/management/commands/pautas_discordantes.py:99`, `:118` + `ponto/management/commands/e6_oraculo.py:182` | não | sim | fora do placar | As duas chamadas reutilizam o juiz do dono. A repetição é de trabalho. |
| OUTBOX-EVENTO-DUPLO | independente | `ponto/portas/celula.py:981`, `:1002` + `chamados/management/commands/consumir_eventos_celula.py:1` | não | não sei [nao verificado] | fora do placar | A porta emite por chamada. O chamador que a acionaria duas vezes no mesmo ato fica [nao verificado]. `docs/BACKLOG.md:1203` |
| JUSTIFICATIVA-ENCERRA-ORIGEM | fechamento da origem fora do ato da aprovação | `ponto/services/justificativa.py:16`, `:65`, `:86`, `:217` | parcial | não na porta lida | E chamado · `core/contratos_estruturais.py:496` | Aprovação chama a retratação no mesmo ato e conserva a guarda do chamado irmão. Retenções nominais [nao verificado]. |
| BO-ATESTADO-ANDROID-ERRO-AO-ENVIAR | independente | `api/views.py:1824`, `:1857` + `docs/BACKLOG.md:1223` | não | não para reenvio idêntico no servidor | fora do placar | Reenvio idêntico devolve a linha existente. Veto real tem ramo próprio. Comportamento do Android [nao verificado]. |
| REABRIR-NAO-DESTROI | reabertura sem escritor do fato anterior | `chamados/services/disputa_emissao.py:1628`, `:1659`, `:1695` | sim | não no caminho lido | E chamado · `core/contratos_estruturais.py:496` | A decisão registra a foto da resposta antes de descarimbar. |
| HE-DE-14H-NUM-DIA (col945 12/09) | teto do dia ignorado no pareamento | `ponto/turnos.py:66`, `:1427`, `:1456` + `docs/BACKLOG.md:1235` | parcial | não no ramo lido | J turno/marcos · `core/contratos_estruturais.py:225` | O pareador lê o teto e pode deixar a saída órfã. O caso e o gravado antigo ficam [nao verificado]. |
| BUG-ANCORA-NAO-REGENERA | alteração de âncora sem efeito na célula | `escala/signals.py:87`, `:113`, `:134` + `ponto/portas/celula.py:112` | parcial | não no sinal lido | fora do placar | O sinal chama a porta de regeneração. O contrato da célula cobre o escritor e a obra inclui o alcance do disparo. `core/contratos_estruturais.py:113` |
| O-HAIKU-INVESTIGA | independente | `api/views_mensageria.py:1935`, `:1981`, `:2021` + `docs/BACKLOG.md:1262` | não | não sei [nao verificado] | fora do placar | Entrega de consultas e propostas. Os leitores correlatos existem. O pacote e o juiz incremental pedidos ficam [nao verificado]. |
| O-JANELA-HE | janela de HE sem regra única para as duas pontas | `ponto/janela_he.py:107`, `:150` + `ponto/motor_calculo_v2.py:1812`, `:1854` + `ponto/calculador/regras.py:30` | parcial | não nos caminhos lidos | P folha/export · `core/contratos_estruturais.py:307`, `:320` | Motor e calculador leem a janela. Cadastro, ato administrativo e tela são entregas adicionais. |

## Obras 55–102

| ID | Nascente | Sítio | Morre? | Ainda vivo? | Placar 20 | Nota |
|---|---|---|---|---|---|---|
| CHAMADO-NASCE-EM-EXPORTADA | independente | `chamados/models.py:1062`, `:1092` + `folha/export.py:565` | não | não como defeito descrito | fora do placar | O gate vigente do nascimento usa tranca. A exportação vigente é outra pergunta. `docs/BACKLOG.md:1269` |
| COL369-TRES-RESTANTES | [nao verificado] | `ponto/turnos.py:502`, `:1565` + `ponto/services/dia_pago.py:364`, `:481` + `colaboradores/services/vinculo.py:550`, `:567` | não sei [nao verificado] | não sei [nao verificado] | [nao verificado] | São três ocorrências. Os sítios tratam pausa, campos gravados e guarda da vigência, sem reproduzir a atribuição nominal. `docs/BACKLOG.md:1288` |
| O-GEO-DECISAO | ato de geofence dividido entre autorizar e recusar | `chamados/services/fio.py:669`, `:682` + `chamados/services/acoes_chamado.py:171`, `:251`, `:277` + `ponto/presenca.py:29` | parcial | sim | fora do placar | Os ramos do ato são separados. A notificação de ausência de ping é outro componente. A escrita de Batida já tem seu contrato. `core/contratos_estruturais.py:91` |
| O146 | independente | `escala/models.py:78`, `:83` + `ponto/management/commands/gerar_celulas.py:128` + `ponto/motor_calculo_v2.py:1457` | não | não no motor | fora do placar | O motor consome a extra declarada no DNA. A linha do copiloto fica [nao verificado]. |
| O236 | independente | `folha/export.py:1185`, `:1221`, `:1240` + `docs/BACKLOG.md:1337` | não | não sei [nao verificado] | fora do placar | Censo por motivo de retenção. O export já tem autoridade para a classificação. |
| O121 | guarda do passado do vínculo concorrendo com a guarda da célula | `colaboradores/services/vinculo.py:69`, `:72`, `:330` + `ponto/portas/celula.py:218` + `folha/export.py:607` | parcial | sim no caminho geral | J folha/export, K9 · `core/contratos_estruturais.py:353` | A edição comum ainda passa pelo piso e pela guarda de TXT ou holerite. O piso S99 e o wizard têm escopo próprio. `docs/BACKLOG.md:1362` |
| O123 | independente | `colaboradores/views.py:3045` + `chamados/relatorios_disputa.py:47`, `:53` | não | não para a carga proposta | fora do placar | O coletor carrega perguntas, célula, destinos e decisões em lote. O custo residual fica [nao verificado]. |
| O118 | marcos do dia reconstruídos com uma só pausa | `escala/utils.py:27`, `:65`, `:1458`, `:1664` | parcial | não no ramo lido | J turno/marcos, T5 · `core/contratos_estruturais.py:225` | O alinhamento recebe todas as pausas do DNA. A prova de tela fica [nao verificado]

[truncado em 20000 de 75137 caracteres]

## 27/11 15:15 · sessao 878f619f0872 · INVENTARIO-PELA-REGRA · leitura · origem=Code

Conferi o HEAD `59600163ccc9cffb8f3ae148beede7cd7866ae86` e comparei os arquivos citados com os blobs desse commit. A sonda está abaixo. Os números da comp 11 e o cadastro atual da Sombra ficam **[nao verificado]** até a execução pelo envelope. [bin/codex_pergunta.sh:88](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:88) [bin/codex_pergunta.sh:611](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:611)

## A. R4

Para cada `Empresa` de PK 2, 3 e 4, a entrada de leitura é `folha.porta_export.medir(empresa, 11, 2026)`. A janela vem de `janela_fechamento(11, 2026, empresa)`. A sonda confere o intervalo de **21/10 a 20/11/2026** e imprime o marco lido do banco para cada empresa. [app/core/placar_estrutural.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/placar_estrutural.py:214) [app/folha/porta_export.py:319](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:319) [app/ponto/janelas.py:54](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/janelas.py:54) [app/ponto/services/fechamento.py:1577](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1577)

| Par | Função que mede |
|---|---|
| Tela × PDF | `medir` compara `espelho_do_colab(c, hoje, ini, fim, agora=ref)` e `_coletar_dados_espelho_mes(c, 11, 2026, agora=ref)`. Retorno `tela_x_pdf`. [app/folha/porta_export.py:482](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:482) [app/folha/porta_export.py:502](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:502) |
| Cartão × TXT | `relatorios.cartao_pela_celula.divergencia(c, 11, 2026, resumo)`, chamado por `cartao_x_txt_divergentes`. O retorno de `medir` é da frota da competência. Para obter os números por empresa, a sonda cruza os IDs divergentes emitidos pelo comando com `Colaborador.empresa_id`. [app/relatorios/cartao_pela_celula.py:216](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/relatorios/cartao_pela_celula.py:216) [app/relatorios/management/commands/cartao_x_txt_divergentes.py:68](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/relatorios/management/commands/cartao_x_txt_divergentes.py:68) [app/folha/porta_export.py:732](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:732) |
| Espelho × DiaPago | `dias_fora_do_dia_pago(c, 11, 2026, ini, fim, versao='motor', dp_por_dia=None)`. Retornos `espelho_x_dia_pago` e `espelho_x_dia_pago_nao_medido`. [app/folha/porta_export.py:26](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:26) [app/folha/porta_export.py:603](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:603) |
| Fechamento × soma do DiaPago | `_medir_dia_pago(c, fech, badge=None)`, interna a `medir`, usa `soma_da_competencia(c, 11, 2026, versao='motor')`. Retorno `fechamento_x_soma_dias_pagos`. O universo inclui quem tem fechamento ou lavratura, inclusive fora do TXT. [app/folha/porta_export.py:384](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:384) [app/folha/porta_export.py:457](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:457) [app/folha/porta_export.py:653](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:653) [app/ponto/services/dia_pago.py:550](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:550) |
| Topo × soma das linhas | `medir` compara `total_relogio` com a soma de `minutos_realizados` dos dias da janela. Retorno `topo_x_coluna`. [app/folha/porta_export.py:515](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:515) |
| App × tela | `AppLeATelaTest.test_MORDE_o_endpoint_nao_remonta_a_palavra_do_dia()` e `test_MORDE_o_endpoint_LE_o_espelho()`. É um selo AST do código, aplicável às três empresas. [app/api/tests/test_r4_par6_app_le_a_tela.py:53](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/api/tests/test_r4_par6_app_le_a_tela.py:53) |
| Linhas do DiaPago × badge | `linha_x_badge(c, 11, 2026, badge, soma=None)`, com `badge` igual ao resumo do espelho. Mede noturnas, atraso e trabalhadas mais folga quando a fonte do badge é `folha`. Os retornos distinguem divergência, exportada e motivo de ausência de medição. [app/folha/porta_export.py:251](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:251) [app/folha/porta_export.py:449](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:449) [app/folha/porta_export.py:717](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:717) |

`medir` é a entrada de leitura. O CLI `selo_leitores_no_mesmo_numero` vigia e assume tentativas, depois chama `concluir`, que grava o carimbo por `lavrar` em `MetricaSnapshot.objects.update_or_create`. A chave `calendario_x_espelho` é de compatibilidade e vem sempre vazia. [app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:76](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:76) [app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:197](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:197) [app/folha/porta_export.py:1279](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:1279) [app/folha/porta_export.py:785](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:785) [app/folha/porta_export.py:608](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:608)

## B. R5

**R5 so com escrita: fica fora (proibido medida nova)**. O `.sh` chama o Python fixado em **10/2026**. Cada passada aplica o cartório e grava o recálculo. A prova compara a foto após a primeira escrita com a foto após a segunda. O dry do cartório, sem `--apply`, julga a fila `data < hoje`. Duas leituras dessa fila observam o mesmo estado e deixam a transição exigida pelo R5 sem prova. [bin/r5_idempotencia_frota.sh:24](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/r5_idempotencia_frota.sh:24) [bin/r5_idempotencia_frota.py:26](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/r5_idempotencia_frota.py:26) [bin/r5_idempotencia_frota.py:104](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/r5_idempotencia_frota.py:104) [bin/r5_idempotencia_frota.py:124](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/r5_idempotencia_frota.py:124) [app/ponto/management/commands/processar_cartorio.py:25](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/processar_cartorio.py:25) [app/ponto/services/cartorio.py:1905](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/cartorio.py:1905) [app/ponto/services/fechamento.py:1168](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1168)

## C. Becos, órfãs e dias sem caminho

- **Beco de disputa.** `chamados.services.becos.classificar(disputa)` devolve `fechavel`, `barrada` ou `None`. O cron usa `--alarme`, com RC 2 se houver fecháveis. Até em dry o comando executa `get_user_sistema()`, que pode fazer `INSERT` via `get_or_create`. A sonda chama diretamente o classificador e usa `dia_do_fato(chamado)` para o recorte. Becos sem dia apurável aparecem em linha própria. [app/config/crons.py:663](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:663) [app/chamados/management/commands/fechar_becos_disputa.py:27](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/fechar_becos_disputa.py:27) [app/core/utils.py:18](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/utils.py:18) [app/chamados/services/becos.py:25](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/becos.py:25) [app/chamados/juizes.py:89](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/juizes.py:89)
- **Dia retido sem caminho.** `censo_alcance(empresas=[empresa], hoje=hoje, agora=ref, limite=0)` é o leitor do `vigiar_fio_morto` que devolve todos os `sem_porta`, mesmo com limite zero. A sonda conta por causa, detalhe e papel em `11/2026` e indica quando a competência está fora do universo do vigia. A execução de frota do CLI grava carimbo em disco. [app/config/crons.py:1746](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:1746) [app/chamados/services/vigia_fio.py:524](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/vigia_fio.py:524) [app/chamados/services/vigia_fio.py:622](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/vigia_fio.py:622) [app/chamados/management/commands/vigiar_fio_morto.py:131](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/vigiar_fio_morto.py:131)
- **Fio mudo e emissão que faltou.** `contador_fio_mudo(hoje=hoje, agora=ref, ini=ini, fim=fim, empresas=[eid])` mede motivos devidos sem pergunta viva. `motivos_que_o_evento_deixou(cel, hoje, ref, memo=memo)` mede o que faltou emitir na célula. Ambos são leitores. O `--alarme` do cron dá RC 2 pelo segundo contador. [app/config/crons.py:1548](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:1548) [app/chamados/management/commands/disparar_perguntas_competencia.py:77](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/disparar_perguntas_competencia.py:77) [app/chamados/management/commands/disparar_perguntas_competencia.py:221](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/disparar_perguntas_competencia.py:221) [app/chamados/services/fabrica_da_celula.py:234](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/fabrica_da_celula.py:234)
- **Órfãs e RC.** `orfas_pela_lampada_sem_evento(agora=ref, colaborador_ids=ids)` devolve os IDs que o evento devia ter encerrado. `reconciliar_perguntas_orfas` é leitura e `--alarme` dá RC 2 se houver achado. No `reconciliar_chamados`, `perguntas_orfas_reatavies(colab)` devolve um queryset do universo elegível, que fica fora da soma de alarme do RC. A sonda recorta os dois por `data_evento`. [app/config/crons.py:1551](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:1551) [app/chamados/reconciliador.py:1325](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1325) [app/ponto/management/commands/reconciliar_perguntas_orfas.py:24](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/reconciliar_perguntas_orfas.py:24) [app/chamados/reconciliador.py:1640](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1640) [app/chamados/management/commands/reconciliar_chamados.py:97](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/reconciliar_chamados.py:97)
- **E6.** `e6_oraculo --mes 11 --ano 2026 --hoje 2026-11-27` é leitura sem `--csv`. Mede divergência do espelho por dia-colaborador, com dono atribuído por `InsumosDoDono.dono(dia)`. O comando percorre a frota ativa com fechamento. A sonda usa as colunas `emp`, `dono_da_divergencia` e `classe` do CSV oficial em memória para imprimir o resultado por empresa. Se o comando reportar erro de espelho, a sonda levanta exceção. [app/ponto/management/commands/e6_oraculo.py:182](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:182) [app/ponto/management/commands/e6_oraculo.py:276](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:276) [app/ponto/management/commands/e6_oraculo.py:303](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:303) [app/ponto/management/commands/e6_oraculo.py:442](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:442)

## D. Layout do TXT e cadastro L-099

A view obtém os objetos com `recalcular_fechamento_mes(11, 2026, empresa_id=e.pk, somente_leitura=True, retornar_objeto=True)`. Passa esses mesmos objetos a `classificar_export` e a `montar_linhas_export(empresa, mes, ano, itens=itens, fechamentos=fechs)`. O primeiro valor devolvido pela montagem é `linhas`, que entra em `validar_layout_dominio(linhas)`. A aprovação em lote e o registro de `ExportacaoDominio` ficam no caminho da view. [app/folha/views.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:214) [app/folha/views.py:253](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:253) [app/folha/views.py:275](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:275) [app/folha/views.py:282](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:282) [app/folha/views.py:304](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:304)

A sonda avalia todo o universo da competência e valida o TXT montado para quem a regra admite. Quando há erro, remonta os blocos por colaborador com a mesma função, preserva o bloco da rubrica `8792` com seus registros `11` e confere que linhas e erros recompostos coincidem com os da empresa. Cada achado fica em **CADASTRO L-099**, com `colaborador_id`, regra e campo. A mensagem bruta do validador pode conter a linha inteira e fica em memória. [CLAUDE.md:183](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/CLAUDE.md:183) [app/folha/export.py:266](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:266) [app/folha/export.py:699](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:699) [app/folha/export.py:725](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:725)

## E. Feriado municipal da comp 12

A data localizada é **10/12/2026, Aniversário de Londrina/PR**. O teste cadastra esse feriado e uma leitura anterior da Sombra registra 10/12 como feriado por posto na comp 12. A presença no cadastro atual da Sombra está **[nao verificado]**. [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:39](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:39) [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62) [lista anterior da Sombra](/home/ronald/saas-hasner/logs/FERIADO-POR-POSTO/pouso_lista_v2/lista.out:50)

O cadastro é `core.models.Feriado`, tabela `core_feriado`. A consulta abaixo filtra `abrangencia='municipal'` e `data__range=(21/11/2026, 20/12/2026)` e ordena por UF, município e data. Por empresa, o cruzamento é com `Posto.praca` e `Posto.municipio`, usando `core.feriados.alcanca` para aplicar a regra de UF e município. [app/core/models.py:5](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/models.py:5) [bin/sombra_regras_pessoais.py:194](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sombra_regras_pessoais.py:194) [app/colaboradores/models.py:230](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/models.py:230) [app/core/feriados.py:57](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:57)

## F. Sonda Python

Para `bin/codex_pergunta.sh --sonda`, o caminho em `CODEX_SONDA_ENVELOPE` é o **adaptador de Sombra existente**, `/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh`. O padrão do wrapper passa só o arquivo Python e cai em produção. `SONDA_ARVORE` deve apontar para o `app/` da cópia conferida, pois é esse caminho que o envelope monta no contêiner. O envelope prepara role, grants e arquivos próprios, por isso esta entrega ficou em leitura de código e checagem em memória. [bin/codex_pergunta.sh:623](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:623) [adaptador da Sombra](/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh:3) [bin/sonda_leitura.sh:93](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:93) [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:126) [bin/sonda_leitura.sh:153](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:153) [bin/sonda_leitura.sh:232](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:232)

Fixei o relógio da medição em **27/11/2026 às 15:15, São Paulo**. O cabeçalho imprime também a hora real de execução. O `raise` final provoca rollback, e só essa sentinela é suprimida depois da saída do `atomic()`, para o wrapper aceitar o stdout. A sintaxe e exemplos dos rótulos do layout foram checados em memória. A execução Django permanece **[nao verificado]**. [app/ponto/services/fechamento.py:634](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:634) [app/chamados/tests/cef_c3_sonda.py:22](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/cef_c3_sonda.py:22) [bin/codex_pergunta.sh:629](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:629)

```python
import logging
import re
import sys
from collections import Counter, defaultdict
from contextlib import contextmanager, nullcontext, suppress
from csv import DictReader
from datetime import date, datetime, timezone as utc
from io import StringIO
from unittest.mock import patch
from zoneinfo import ZoneInfo

from django.contrib.auth.models import User
from django.core.management import call_command
from django.db import connection, transaction
from django.db.models import Exists, OuterRef
from django.utils import timezone

from api.tests.test_r4_par6_app_le_a_tela import AppLeATelaTest
from chamados.juizes import dia_do_fato, tranca_em_lote
from chamados.management.commands.disparar_perguntas_competencia import contador_fio_mudo
from chamados.models import DisputaSupervisao, PerguntaDisputa
from chamados.reconciliador import orfas_pela_lampada_sem_evento, perguntas_orfas_reatavies
from chamados.services import becos
from chamados.services.fabrica_da_celula import motivos_que_o_evento_deixou
from chamados.services.vigia_fio import censo_alcance
from colaboradores.models import Colaborador, Empresa, Posto
from core.feriados import alcanca
from core.models import Feriado
from escala.models import CelulaDia
from folha.export import classificar_export, montar_linhas_export, validar_layout_dominio
from folha.porta_export import medir
from ponto.janelas import janela_fechamento
from ponto.management.commands import e6_oraculo
from ponto.services.fechamento import (
    marco_da_competencia, recalcular_fechamento_mes, universo_da_competencia,
)

HEAD = "59600163ccc9cffb8f3ae148beede7cd7866ae86"
EMPRESAS = (2, 3, 4)
MES, ANO = 11, 2026
INI, FIM = date(2026, 10, 21), date(2026, 11, 20)
JANELA_12 = (date(2026, 11, 21), date(2026, 12, 20))
FUSO = ZoneInfo("America/Sao_Paulo")
REF = datetime(2026, 11, 27, 15, 15, tzinfo=FUSO)
HOJE = REF.date()


class F

[truncado em 20000 de 31363 caracteres]

## 27/11 15:11 · sessao 05e8003882ed · INVENTARIO-PELA-REGRA · construtor · origem=Code

Corrigi as cinco medições nos três módulos da bateria. A sintaxe dos módulos foi verificada. O recorte Django e os mutantes na pista do Code ficam [nao verificado].

### Diff por caso

- **ESC-11** cobra 630 min trabalhados e 150 min de extra total, com 120 em HE50 e 30 em HE100 de dia normal. O excedente é `HE total − teto = 30`, conforme o corte HE100-RUBRICA. [test_inventario_escala.py:327](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:327)
- **AUS-05** deixa o modelo derivar `instancia_marco` para `batida_ausente`. A pergunta segue aberta, com 480 min previstos, furo apurado e zero falta. [test_inventario_ausencia.py:103](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:103)
- **FOL-03** mapeia e homologa `9902` em `folga_trabalhada_diurna`. Confere os 240 min no fechamento antes de comparar o delta do TXT, que continua exigindo zero em HE50 e HE100 de feriado. [test_inventario_folha.py:56](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:56) · [test_inventario_folha.py:164](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:164)
- **FOL-13** usa 60 min de HE50 autorizada com rubrica homologada. Faz dois POSTs idênticos, exige dois downloads 200, confere a presença dos 60 min e compara bytes e totais por rubrica. [test_inventario_folha.py:53](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:53) · [test_inventario_folha.py:309](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:309)

**ESC-12, AUS-03 e FOL-14** estão idênticos à base. [test_inventario_escala.py:335](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:335) · [test_inventario_ausencia.py:61](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:61) · [test_inventario_folha.py:330](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:330)

### Recorte para o Code

Da raiz de `app`, na pista de teste do Code. O settings `ci` lê `DB_HOST` e `REGUA_DB` da pista. [config/settings/ci.py:6](/home/ronald/wt-codex-inventario-pela-regra-143921/app/config/settings/ci.py:6) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci --noinput \
  ponto.tests.test_inventario_escala \
  ponto.tests.test_inventario_ausencia \
  folha.tests.test_inventario_folha
```

### RED e mutantes

No RED isolado de **ESC-11**, as asserções aceitaram `120/30`. Transferir os 30 min para HE50 derrubou as duas asserções por faixa. Calcular `HE50 − teto` derrubou a do excedente com `0 != 30`. A execução integrada desses mutantes e dos demais fica [nao verificado]. [test_inventario_escala.py:328](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:328)

Os comandos abaixo aplicam uma substituição em memória por processo e rodam o caso correspondente. [ESC-11:327](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:327) · [AUS-05:103](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:103) · [FOL-03:56](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:56) · [FOL-13:309](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:309)

```bash
mutante() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 - "$@" <<'PY'
import importlib
import sys
from pathlib import Path

import django
from django.core.management import execute_from_command_line

rotulo, antes, depois = sys.argv[1:]
django.setup()
modulo = importlib.import_module(rotulo.rsplit('.', 2)[0])
arquivo = Path(modulo.__file__)
fonte = arquivo.read_text(encoding='utf-8')
assert fonte.count(antes) == 1, (arquivo, antes)
exec(compile(fonte.replace(antes, depois, 1), str(arquivo), 'exec'), vars(modulo))
execute_from_command_line(['manage.py', 'test', '--noinput', rotulo])
PY
}

esc11=ponto.tests.test_inventario_escala.InventarioEscalaTest.test_ESC_11_extra_integral_acima_de_duas_horas
aus05=ponto.tests.test_inventario_ausencia.InventarioAusenciaTest.test_AUS_05_uma_batida_com_pergunta_em_aberto
fol03=folha.tests.test_inventario_folha.InventarioFolhaTest.test_FOL_03_dsr_trabalhado_em_rubrica_propria
fol13=folha.tests.test_inventario_folha.InventarioFolhaTest.test_FOL_13_gerar_duas_vezes_mesmos_bytes_e_totais

# M1 — ESC-11: deslocar a HE100 para HE50, preservando o total.
mutante "$esc11" '        # HE100-RUBRICA' \
  $'        for periodo in resultado.periodos:\n            periodo.minutos_extra_50 += periodo.minutos_extra_100\n            periodo.minutos_extra_100 = 0\n        # HE100-RUBRICA'

# M2 — ESC-11: medir o excedente só pela HE50.
mutante "$esc11" 'extra_total - teto' 'resultado.horas_extra_50 * 60 - teto'

# M3 — AUS-05: forçar "fim" na pergunta genérica.
mutante "$aus05" $'            data_evento=data,\n        )' \
  $'            data_evento=data, instancia_marco=\'fim\',\n        )'

# M4 — FOL-03: voltar a mapear 9902 para o evento antigo.
mutante "$fol03" "'folga_trabalhada_diurna': DSR_TRABALHADO" \
  "'dsr_trabalhado_diurno': DSR_TRABALHADO"

# M5 — FOL-13: retirar da massa o evento exportável.
mutante "$fol13" \
  $'colab = self.massa_comercial({data: (\'08:00\', \'12:00\', \'13:00\', \'18:00\')})\n        self.autorizar(colab, data, 60)\n        self.fechar(colab, 11)' \
  $'colab = self.massa_comercial({data: COMERCIAL})\n        self.fechar(colab, 11)'

# M6 — FOL-13: alterar os bytes do segundo download após conferir o 200.
mutante "$fol13" '        self.assertEqual(primeiro.content, segundo.content)' \
  $'        segundo.content += b\'\\n\'\n        self.assertEqual(primeiro.content, segundo.content)'
```

## 27/11 15:10 · sessao 2e975728d169 · - · - · origem=Ronald (tmux)

Auditei os 40 itens de `RETRATO-DOIS-RELOGIOS` a `O320` e mais 22 de `EMPRESA-ID-ISDIGIT` ao fim do anexo. Enviei as linhas e os ajustes de consolidação ao agente principal. A cópia já tem o caminho da saída tardia de `O232` e a correção de pontualidade de `O73b`. [app/escala/utils.py:1999] [app/escala/tests/test_saida_valida_cross_meia_noite.py:80] [app/ponto/motor_calculo_v2.py:2822]

Estado de produção e identificação Git da cópia [nao verificado].

## 27/11 15:09 · sessao f553e73e9590 · - · - · origem=Ronald (tmux)

O HEAD de `/home/ronald/saas-hasner` é `59600163ccc9cffb8f3ae148beede7cd7866ae86`. Os arquivos A/B e os dois envelopes na cópia de leitura coincidem byte a byte com os blobs do commit. A resolução do hash pelo wrapper está em `bin/codex_pergunta.sh:116`.

**A.** Para cada `Empresa.objects.get(pk=eid)`, chame `folha.porta_export.medir(empresa, 11, 2026)`. A janela vem de `janela_fechamento(11, 2026, empresa)`, cujo resultado com corte 21 é `21/10/2026–20/11/2026`. O marco pode ser lido com `marco_da_competencia(eid, 11, 2026)`. `app/folha/porta_export.py:319`, `app/folha/porta_export.py:343`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:54`, `app/ponto/services/fechamento.py:1577`.

| Par | Função e resultado |
|---|---|
| Tela × PDF | `medir` compara `espelho_do_colab(c, hoje, ini, fim, agora=instante)` com `_coletar_dados_espelho_mes(c, 11, 2026, agora=instante)`. Chave `tela_x_pdf`. `app/folha/porta_export.py:482`, `app/folha/porta_export.py:502`, `app/folha/porta_export.py:699`. |
| Cartão × TXT | `medir` chama `cartao_x_txt_divergentes`. O comando mede todas as empresas ativas e chama `relatorios.cartao_pela_celula.divergencia(colab, 11, 2026, resumo)`. Para contagem por empresa, rode o comando com `ids=True` e `agora` ISO fixo, capture a saída em `StringIO` e associe os IDs `colN` à empresa pelo ORM. `app/folha/porta_export.py:670`, `app/folha/porta_export.py:732`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:20`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:68`, `app/relatorios/cartao_pela_celula.py:216`. |
| Espelho × DiaPago | `dias_fora_do_dia_pago(colab, 11, 2026, ini, fim, versao='motor', dp_por_dia=None)`, chamada por `medir`; chaves `espelho_x_dia_pago` e `espelho_x_dia_pago_nao_medido`. `app/folha/porta_export.py:26`, `app/folha/porta_export.py:603`, `app/folha/porta_export.py:703`. |
| Fechamento × soma do DiaPago | `_medir_dia_pago(c, fech, badge=None)`, interna a `medir`, usa `soma_da_competencia(c, 11, 2026, versao='motor')`. Chaves `fechamento_x_soma_dias_pagos` e `colabs_sem_lavratura`. `app/folha/porta_export.py:384`, `app/folha/porta_export.py:457`, `app/folha/porta_export.py:664`, `app/ponto/services/dia_pago.py:550`. |
| Topo × soma das linhas | `medir` compara `total_relogio` com a soma de `minutos_realizados` dos dias dentro da janela, chave `topo_x_coluna`. `app/folha/porta_export.py:515`, `app/folha/porta_export.py:699`. |
| App × tela | `AppLeATelaTest.test_MORDE_o_endpoint_nao_remonta_a_palavra_do_dia()` e `test_MORDE_o_endpoint_LE_o_espelho()` verificam a AST de `api_espelho_v2` para todos os recortes. `app/api/tests/test_r4_par6_app_le_a_tela.py:53`, `app/api/views.py:1361`. |
| Linhas × badge | `linha_x_badge(colab, 11, 2026, badge, soma=None)` usa o `resumo` do espelho e a soma das linhas. Chaves `linha_x_badge_divergentes`, `linha_x_badge_na_exportada` e `linha_x_badge_nao_medido`. `app/folha/porta_export.py:258`, `app/folha/porta_export.py:449`, `app/folha/porta_export.py:717`. |

`calendario_x_espelho` é uma chave de compatibilidade vazia, pois a pergunta foi incorporada a `espelho_x_dia_pago`. O selo CLI chama `vigiar_abertos`, `assumir` e `concluir`. `concluir` chama `lavrar`, que persiste `MetricaSnapshot` em `update_or_create`. A sonda usa apenas `medir` e os leitores. `app/folha/porta_export.py:608`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:76`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:197`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:219`, `app/folha/porta_export.py:785`, `app/folha/porta_export.py:1279`.

**B.** R5 so com escrita: fica fora (proibido medida nova). O script está fixado na comp 10. Executa duas passadas de `processar_cartorio(apply=True)` e `recalcular_fechamento_mes()`, com critério `foto3 == foto2`. O cartório carimba `CelulaDia` e pode emitir chamados. O recálculo grava `FechamentoMensal`. O dry do cartório omite `--apply` e lê a fila histórica `data < hoje`. O recálculo dispõe de `somente_leitura=True, lavrar_dias_pagos=False`. Os dois modos de leitura preservam o estado da primeira passagem e, por isso, não verificam o critério do R5. `bin/r5_idempotencia_frota.sh:24`, `bin/r5_idempotencia_frota.py:26`, `bin/r5_idempotencia_frota.py:104`, `bin/r5_idempotencia_frota.py:124`, `app/ponto/management/commands/processar_cartorio.py:25`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1883`, `app/ponto/services/cartorio.py:1905`, `app/ponto/services/fechamento.py:468`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/fechamento.py:1215`.

**F.** O wrapper chama um executável por `CODEX_SONDA_ENVELOPE`, com o caminho da sonda como único argumento. O executável existente `/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh:3` usa `exec` com `--sombra` e preserva o RC. `SONDA_ARVORE=<cópia conferida>/app` fixa os blobs montados. O Python deve capturar somente a exceção sentinela de rollback, fora do `atomic()`, pois RC diferente de zero faz o wrapper rejeitar a saída. O envelope faz escritas administrativas de role/GRANT e de arquivos durante a preparação. Eu apenas li o código. `bin/codex_pergunta.sh:627`, `bin/sonda_leitura.sh:66`, `bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:140`, `bin/sonda_leitura.sh:157`, `bin/sonda_leitura.sh:232`, `bin/sonda_leitura.sh:239`.

## 27/11 15:09 · sessao 3656640e4966 · - · - · origem=Ronald (tmux)

- **Becos de disputa.** O contador usa `chamados.services.becos.classificar(disputa)` e retorna `FECHAVEL`, `BARRADA` ou `None`. O comando chama `get_user_sistema()` mesmo em dry ou `--alarme`, e esse helper usa `get_or_create`. A sonda deve usar o classificador diretamente. O `--alarme` do cron sai com RC 2 para qualquer `FECHAVEL`. `--apply` chama a porta de escrita `fechar_beco`. [app/chamados/services/becos.py:25](app/chamados/services/becos.py#L25), [app/chamados/management/commands/fechar_becos_disputa.py:27](app/chamados/management/commands/fechar_becos_disputa.py#L27), [app/core/utils.py:18](app/core/utils.py#L18), [app/config/crons.py:663](app/config/crons.py#L663)
- **Órfãs da lâmpada.** `chamados.reconciliador.orfas_pela_lampada_sem_evento(agora=REF, colaborador_ids=ids)` devolve os IDs completos. O recorte da competência pode filtrar esses IDs por `PerguntaDisputa.data_evento`. O comando sem argumentos de alarme conta em dry e `--alarme` sai com RC 2 se houver achados. `perguntas_orfas_reatavies(colab)` mede separadamente o universo elegível e pode receber o filtro de data no queryset. [app/chamados/reconciliador.py:1325](app/chamados/reconciliador.py#L1325), [app/ponto/management/commands/reconciliar_perguntas_orfas.py:24](app/ponto/management/commands/reconciliar_perguntas_orfas.py#L24), [app/chamados/reconciliador.py:1640](app/chamados/reconciliador.py#L1640)
- **Fio sem caminho.** `censo_alcance(empresas=[empresa], hoje=HOJE, agora=REF, limite=0)` usa a tarefa publicada e retorna todos os `sem_porta` mesmo com limite zero, com competência, causa e detalhe em cada linha. A sonda pode contar os `sem_porta` da `11/2026` por causa e detalhe. O vigia mede empresas visíveis em operação e competências que `competencias_a_lavrar` seleciona. O comando de frota grava carimbo em disco. `contador_fio_mudo(hoje=HOJE, agora=REF, ini=INI, fim=FIM, empresas=[empresa.pk])` mede os motivos devidos sem pergunta viva. `motivos_que_o_evento_deixou(cel, HOJE, REF, memo=memo)` mede o que o julgamento da célula deixou de emitir. [app/chamados/services/vigia_fio.py:524](app/chamados/services/vigia_fio.py#L524), [app/chamados/services/vigia_fio.py:622](app/chamados/services/vigia_fio.py#L622), [app/chamados/management/commands/vigiar_fio_morto.py:131](app/chamados/management/commands/vigiar_fio_morto.py#L131), [app/chamados/management/commands/disparar_perguntas_competencia.py:77](app/chamados/management/commands/disparar_perguntas_competencia.py#L77), [app/chamados/services/fabrica_da_celula.py:234](app/chamados/services/fabrica_da_celula.py#L234)
- **E6.** O comando `e6_oraculo --mes 11 --ano 2026 --hoje 2026-11-27`, sem CSV, certifica o espelho da frota e exige `FechamentoMensal` para selecionar colaboradores. O comando captura falha do espelho e segue. Para apor a classe a um dia já achado pelo vigia, existe precedente em `RELATO`, com `InsumosDoDono(colab, INI, FIM, HOJE, agora=REF).dono(dia)` como leitor por colaborador. [app/ponto/management/commands/e6_oraculo.py:128](app/ponto/management/commands/e6_oraculo.py#L128), [app/ponto/management/commands/e6_oraculo.py:182](app/ponto/management/commands/e6_oraculo.py#L182), [app/ponto/management/commands/e6_oraculo.py:276](app/ponto/management/commands/e6_oraculo.py#L276), [app/docs/RELATO.md:5549](app/docs/RELATO.md#L5549)
- **RC de órfãs de obra.** `bin/rc_sem_colher.py::sem_colher(raiz, relato_txt, agora=None, min_idade=0)` lê logs de obras das últimas 24 h. RC 0 indica lista vazia, RC 3 indica achados e RC 1 indica falha de leitura. Sua unidade é obra e arquivo `.rc`. [bin/rc_sem_colher.py:2](bin/rc_sem_colher.py#L2), [bin/rc_sem_colher.py:50](bin/rc_sem_colher.py#L50)

O envelope padrão de `codex_pergunta.sh --sonda` chama `sonda_leitura.sh` sem `--sombra`. Há um `CODEX_SONDA_ENVELOPE` de sombra documentado no `RELATO`. A saída só segue se o envelope retorna zero, logo a sonda precisa suprimir exclusivamente a exceção deliberada de rollback **fora** do `atomic`. `SET TRANSACTION READ ONLY` antes de consultar e a pré-carga `tranca_em_lote(sistema_pk=User.objects.values_list('pk', flat=True).get(username='sistema'))` evitam o `get_or_create` nas leituras da tranca. [bin/codex_pergunta.sh:623](bin/codex_pergunta.sh#L623), [bin/sonda_leitura.sh:93](bin/sonda_leitura.sh#L93), [app/docs/RELATO.md:2735](app/docs/RELATO.md#L2735), [app/chamados/juizes.py:1254](app/chamados/juizes.py#L1254), [app/chamados/juizes.py:1321](app/chamados/juizes.py#L1321)

## 27/11 15:08 · sessao 83229a591dba · - · - · origem=Ronald (tmux)

Auditei as 25 obras do bloco inicial e as cinco verificações adicionais. Enviei ao agente principal as classificações, os sítios de código, os contratos e as ressalvas de acervo. A confirmação do hash e a situação atual em produção ficam [nao verificado].

## 27/11 15:08 · sessao 9e7bed9a78ac · INVENTARIO-PELA-REGRA · leitura · origem=Code

# INVENTÁRIO-PELA-REGRA

Gabarito de **12 casos CCT e 5 casos intermitentes**, pela L-110. Execução contra o HEAD e placar de vermelhos [nao verificado]. `app/docs/LEIS.md:136`

Os cadastros, vínculos, convocações e batidas abaixo são dados de teste [nao verificado]. As batidas estão em UTC−3. Nos plantões que cruzam a meia-noite, os minutos pertencem ao dia de início. `app/docs/LEIS.md:107` · `app/docs/LEIS.md:117`

A extração inicial exibiu comentários técnicos junto dos jurídicos em `app/core/regua_cct.py:56`. As referências dos casos apontam para as regras, cláusulas e pareceres usados no gabarito. O corte de 17/11 sobre intermitentes e seu texto em `logs/AVAIS-17*` ficaram [nao verificado]. Os casos INT usam a CLT e as leis da casa citadas em cada linha.

## CCT dos Vigilantes de Londrina

`Vigilância A` é a empresa fictícia. Em todos os casos CCT, a linha de `AplicacaoConvencao` está ativa e é única no nível empresa + praça, para o Sindicato nº 2. O vínculo 12x36 está vigente desde 01/11/2026. A convenção PR000251/2026 tem vigência documentada de 01/02/2026 a 31/01/2028. `CLAUDE.md:214` · `CLAUDE.md:218` · `app/core/regua_cct.py:234`

| ID | Família | Escala | CADASTRO | BATIDAS (UTC−3) | RESPOSTA DA REGRA | FONTE |
|---|---|---|---|---|---|---|
| CCT-01 | CCT · `regua_excedente` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026 | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | `lei: cláusula ou corte que defina se a régua do escrito e a 9ª diária se aplicam ao plantão regular 12x36 e quais minutos entram como HE nesse ciclo.` | `SERIE_T.md:1072` · parecer e decisão de 29–30/07, `app/core/regua_cct.py:376` e `app/core/regua_cct.py:378` |
| CCT-02 | CCT · `prorrogacao_noturna_pos5h` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Leitura restritiva registrada no cadastro | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | `lei: parecer ou corte que confirme se a cl. 10 exclui os minutos trabalhados entre 05h e 07h. O texto referido fecha a janela 22h–05h e é silente sobre a prorrogação.` | L-035, `app/docs/LEIS.md:83` · cl. 10, `CLAUDE.md:236` · registro da interpretação, `SERIE_T.md:1326` |
| CCT-03 | CCT · `hora_reduzida_afastada_12x36` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Janela 22h–05h fixada no caso | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | Normais **660 min**. Noturno 0025 **420 min de base**, com conversão **1:1** entre relógio e folha, a 20%. | Cl. 38-d, `app/core/regua_cct.py:129` e `app/core/regua_cct.py:136` · L-036, `app/docs/LEIS.md:84` · `SERIE_T.md:1074` |
| CCT-04 | CCT · `adicional_noturno_pct` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Janela 22h–05h fixada no caso | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | Normais **660 min**. Rubrica 0025 **420 min de base a 20%**, com adicional devido além do piso salarial. | Cl. 3 e 38-d, `app/core/regua_cct.py:136` e `app/core/regua_cct.py:146` · `SERIE_T.md:1075` |
| CCT-05 | CCT · `tolerancia_marcacao_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa prevista 20h30–21h30 | 18/11 E 19:06, S 20:30, E 21:30. 19/11 S 07:00 | `lei: cláusula que fixe a tolerância CCT por marcação, ou corte que mande usar o piso legal neste cadastro. A entrada isolada tem desvio de 6 min.` | `SERIE_T.md:1076` · corte sobre o piso da CLT, art. 58 § 1º, `app/docs/CORTES.md:521` |
| CCT-06 | CCT · `tolerancia_dia_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa prevista 20h30–21h30 | 18/11 E 19:04, S 20:30, E 21:34. 19/11 S 06:56 | `lei: cláusula que fixe o teto diário CCT, ou corte que mande usar o piso legal neste cadastro. Os três desvios de 4 min somam 12 min no mesmo plantão.` | `SERIE_T.md:1077` · corte sobre o piso da CLT, art. 58 § 1º, `app/docs/CORTES.md:521` · L-085, `app/docs/LEIS.md:117` |
| CCT-07 | CCT · `intrajornada_minima_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa prevista 20h30–21h30 e `intervalo_indenizavel=True` | 18/11 E 19:00, S 20:30, E 21:00. 19/11 S 07:00 | `lei: cláusula que fixe o mínimo convencional aplicável. Com 30 min de pausa gozada, esse mínimo decide quantos minutos suprimidos entram na rubrica de intrajornada indenizada.` | `SERIE_T.md:1078` · L-031, `app/docs/LEIS.md:79` · [CLT, art. 71, del5452compilado.htm:373 e :379](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art71) |
| CCT-08 | CCT · `compensacao_autorizada` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantões 18 e 26/11/2026. Prorrogação autorizada e compensação solicitada no segundo plantão | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:30. 26/11 E 19:00, S 20:30, E 21:30. 27/11 S 06:30 | `lei: cláusula que autorize compensar a prorrogação no 12x36 e defina se os 30 min podem ser quitados pela redução do segundo plantão.` | `SERIE_T.md:1079` · [CLT, art. 59 §§ 2º, 5º e 6º, del5452compilado.htm:326, :330 e :332](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |
| CCT-09 | CCT · `compensacao_janela` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantões 18/11/2026 e 18/01/2027. Compensação admitida como premissa para testar só o prazo | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:30. 18/01 E 19:00, S 20:30, E 21:30. 19/01 S 06:30 | `lei: cláusula que fixe a janela, o marco inicial e o vencimento do saldo. Isso decide se a redução em janeiro quita os 30 min de novembro ou se o saldo já virou HE a pagar.` | `SERIE_T.md:1080` · [CLT, art. 59 §§ 2º, 5º e 6º, del5452compilado.htm:326, :330 e :332](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |
| CCT-10 | CCT · `teto_prorrogacao_dia_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Prorrogação de 90 min determinada e confirmada pelo empregador | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 08:30 | `lei: cláusula que fixe o teto diário convencional no 12x36 e diga em que rubrica pagar o trabalho efetivo acima do teto, se houver.` | `SERIE_T.md:1081` · L-097, `app/docs/LEIS.md:115` · [CLT, arts. 59 e 59-A, del5452compilado.htm:323 e :333](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |
| CCT-11 | CCT · cl. 10, janela estrita | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Sem prorrogação após 05h por premissa expressa do caso | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | Normais **660 min**. Noturno 0025 **420 min de base a 20%**, sendo 120 min em 18/11 e 300 min em 19/11. De 05h a 07h, **0 min de adicional noturno**. O plantão é atribuído a 18/11. | Cl. 10, `CLAUDE.md:236` · cl. 38-d, `app/core/regua_cct.py:136` · L-085, `app/docs/LEIS.md:117` |
| CCT-12 | CCT · cl. 38-d, hora de 60 min | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa 01h–02h e janela 22h–05h fixada no caso | 18/11 E 19:00. 19/11 S 01:00, E 02:00, S 07:00 | Normais **660 min**. Noturno 0025 **360 min de base a 20%**. Dos 420 min da janela, 60 min são pausa. Cada 60 min de trabalho noturno gera 60 min de base. | Cl. 38-d e parecer de 01/09, `app/core/regua_cct.py:129`, `app/core/regua_cct.py:136` e `app/core/regua_cct.py:141` · L-036, `app/docs/LEIS.md:84` |

## Intermitente

`Serviços B` é a empresa fictícia. Os casos usam regime `clt`, praça Londrina, sem CCT ou `AplicacaoConvencao` aplicável, vínculo intermitente escrito desde 01/11/2026 e salário-hora pactuado conforme o piso aplicável. As convocações indicadas foram recebidas e aceitas na data registrada, às 08h. [CLT, art. 452-A, del5452compilado.htm:2116, :2117 e :2119](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a)

| ID | Família | Escala | CADASTRO | BATIDAS (UTC−3) | RESPOSTA DA REGRA | FONTE |
|---|---|---|---|---|---|---|
| INT-01 | Intermitente · convocação aceita e trabalhada | Personalizado sem intervalo, 08h–12h | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / chamada aceita 19/11 para 23/11/2026, 08h–12h | 23/11 E 08:00, S 12:00 | Normais **240 min**, HE **0 min**, falta **0 min**. Ao fim da prestação, recibo com remuneração, férias proporcionais + 1/3, 13º proporcional e DSR discriminados. | [CLT, arts. 71 § 1º e 452-A §§ 1º, 2º, 6º e 7º, del5452compilado.htm:375, :2117, :2119, :2125 e :2135](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) |
| INT-02 | Intermitente · dia sem convocação | Personalizado sem intervalo, sem jornada chamada no dia | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / 24/11/2026 sem convocação | 24/11 sem batidas | Dia de inatividade. Previsto **0 min**, normais **0 min**, HE **0 min**, falta **0 min**. Sem desconto de DSR por falta nesse dia. | L-088, `app/docs/LEIS.md:120` · [CLT, art. 452-A § 5º, del5452compilado.htm:2123](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) |
| INT-03 | Intermitente · convocação em feriado | Personalizado sem intervalo, 09h–13h | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / chamada avulsa aceita 17/11 para o feriado de teste de 20/11/2026, sem plantão regular na escala e sem folga compensatória | 20/11 E 09:00, S 13:00 | Feriado em dobro **240 min**, pagos a **2×** o valor da hora normal. DSR em parcela própria. | [Lei 605/49, art. 9º, l0605.htm:67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm#art9) · [Súmula 146, TST, IRR-308, p. 4](https://www.tst.jus.br/documents/d/guest/irr-308-_acordao-pdf) · L-086, `app/docs/LEIS.md:118` |
| INT-04 | Intermitente · HE acima do convocado | Personalizado sem intervalo, 08h–11h | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / chamada aceita 19/11 para 25/11/2026, 08h–11h. HE até 12h acordada e autorizada com registro | 25/11 E 08:00, S 12:00 | Normais **180 min**, HE50 **60 min**, falta **0 min**. | L-097, `app/docs/LEIS.md:115` · [CLT, arts. 59 § 1º e 452-A § 6º I e V, del5452compilado.htm:325, :2127 e :2134](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) |
| INT-05 | Intermitente · DSR proporcional | Personalizado sem intervalo, 08h–11h em dois dias | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / uma chamada aceita 19/11 para 23 e 24/11/2026. Ambos os dias cumpridos integralmente | 23/11 E 08:00, S 11:00. 24/11 E 08:00, S 11:00 | `lei: fórmula do DSR proporcional para uma convocação de dois dias, com base, divisor e arredondamento em minutos. O DSR é devido ao fim da prestação e vem discriminado no recibo.` | [CLT, art. 452-A §§ 6º IV e 7º, del5452compilado.htm:2125, :2132 e :2135](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) · [Lei 605/49, art. 7º b, l0605.htm:55](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm#art7) |

## Perguntas de lei

1. **CCT-01.** Qual cláusula ou corte define a incidência da régua do escrito e da 9ª diária sobre o 12x36 regular? `app/core/regua_cct.py:378`
2. **CCT-02.** Qual parecer ou corte confirma o tratamento de 05h–07h pela cl. 10? `CLAUDE.md:236` · `SERIE_T.md:1326`
3. **CCT-05 e CCT-06.** Quais são as tolerâncias convencionais por marcação e por plantão? Na ausência de cláusula própria, vale neste cadastro o par legal de 5 min por marcação e 10 min por dia? `SERIE_T.md:1076` · `SERIE_T.md:1077` · `app/docs/CORTES.md:521`
4. **CCT-07.** Qual mínimo de intrajornada da CCT rege a indenização do intervalo parcialmente gozado no 12x36? `SERIE_T.md:1078` · `app/docs/LEIS.md:79`
5. **CCT-08 e CCT-09.** Qual cláusula autoriza compensar prorrogação no 12x36 e qual é o prazo para quitar o saldo? `SERIE_T.md:1079` · `SERIE_T.md:1080`
6. **CCT-10.** Qual teto convencional rege a prorrogação diária no 12x36 e como se classifica, para pagamento, o trabalho efetivo acima dele? `SERIE_T.md:1081` · `app/docs/LEIS.md:115`
7. **INT-05.** Qual é a base, o divisor e o arredondamento do DSR proporcional em minutos por período de prestação? [CLT, art. 452-A § 6º IV, del5452compilado.htm:2132](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) · [Lei 605/49, art. 7º b, l0605.htm:55](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm#art7)
8. **INT-01 a INT-05.** Qual é o texto do corte de 17/11 sobre intermitentes e quais destes casos ele alcança? [nao verificado]

## 27/11 15:08 · sessao b24bb17059ed · - · - · origem=Ronald (tmux)

Auditei as 54 obras entre `FERIADO-ESCALA` e `O-JANELA-HE` e encaminhei as linhas ao agente principal, com sítios de código, estado no recorte e contrato aplicável. [GRADE-MESTRA.md](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/GRADE-MESTRA.md:140)

No agregado, `GRADE-FOLGA-C5` e `GRADE-FOLGA-GERADOR` compartilham a leitura de folga fora do juiz. `PROPOSITOR-JANELA-LONGA` e `FILA-POR-CAUSA-AS-OF` compartilham o uso da última lavra onde a consulta precisa da decisão por dia. `TROCA-DE-POSTO-SEM-VINCULO` e o item 01 de `BO2910-ESTRUTURA` compartilham a troca de posto sem uma porta única para o ato. `app/ponto/precedencia.py:212`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1561`, `app/escala/models.py:1321`, `app/escala/services/propositor.py:493`, `app/chamados/services/fila_por_causa.py:184`, `app/colaboradores/views.py:337` e `app/colaboradores/views.py:1415`.

## 27/11 15:08 · sessao 903d81c0f68d · INVENTARIO-PELA-REGRA · leitura · origem=Code

# INVENTARIO-PELA-REGRA

As competências usadas são **11/2026** de 21/10 a 20/11, **12/2026** de 21/11 a 20/12, **01/2027** de 21/12 a 20/01 e **02/2027** de 21/01 a 20/02. Os 15 casos são gabaritos de leitura, com asserções limitadas aos dias informados. A execução na pista e o HEAD estão [nao verificado]. [app/docs/LEIS.md:77](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:77) · [app/docs/LEIS.md:136](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:136)

**Dados comuns.** Cadastro e batidas são entradas sintéticas, independentes por caso. Onde constar 6×1 comercial, usei 08:00–17:00 com pausa 12:00–13:00 de segunda a sexta, sábado 08:00–12:00 e DSR domingo. Usei essa escala nos recortes anteriores à transição de dezembro da EC 141/2026. Nos plantões de dezembro e janeiro, o acordo coletivo de jornada e repouso regular é dado do teste. Os feriados locais são entradas de cadastro da praça Londrina/PR, cuja situação real está [nao verificado]. [app/docs/LEIS.md:64](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:64) · [EC 141/2026, arts. 6º e 10, linhas 34–52](https://www.planalto.gov.br/ccivil_03/constituicao/emendas/emc/emc141.htm)

**CMP-01**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo, mensalista. Feriado nacional de 20/11/2026 cadastrado. Sexta seria dia de trabalho e o posto dispensa o expediente no feriado.  
**BATIDAS (UTC−3)** 19/11 08:00 E, 12:00 S, 13:00 E, 17:00 S. 20/11 sem batidas. 21/11 08:00 E, 12:00 S.  
**RESPOSTA DA REGRA** 20/11 é feriado nacional e fecha a comp. **11/2026**. No dia, 0 min de presença exigida, 0 trabalhados, 0 de HE de feriado e 0 de falta. Remuneração do feriado preservada. O sábado 21/11 tem 240 min normais na comp. **12/2026**.  
**FONTE** `app/docs/LEIS.md:77` · [Lei 14.759/2023, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2023/lei/l14759.htm) · [Lei 605/1949, art. 8º, linha 64](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-02**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo. Cadastro dado de feriado municipal em 10/12/2026, praça Londrina/PR. Trabalho autorizado em dia ordinariamente útil, sem folga compensatória. DSR em 13/12.  
**BATIDAS (UTC−3)** 10/12 08:00 E, 12:00 S, 13:00 E, 17:00 S. 11/12 08:00 E, 12:00 S, 13:00 E, 17:00 S. 12/12 08:00 E, 12:00 S. 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. Em 10/12, 480 min trabalhados na base da rubrica **feriado em dobro**, remunerada a 200%. Em 11/12, 480 min normais. Em 12/12, 240 min normais. DSR de 13/12 preservado.  
**FONTE** `app/docs/LEIS.md:64` · `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · [Lei 605/1949, art. 9º, linha 67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [Súmula 146 do TST, linhas 1498–1502](https://www.tst.jus.br/documents/10157/cc23ec94-9e08-47e3-8ed6-2e662cee7690)

**CMP-03**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo. Mesmo feriado municipal dado de 10/12/2026. Trabalho autorizado, com folga compensatória integral formalizada e gozada em 11/12. DSR em 13/12.  
**BATIDAS (UTC−3)** 10/12 08:00 E, 12:00 S, 13:00 E, 17:00 S. 11/12 sem batidas. 12/12 08:00 E, 12:00 S. 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. Em 10/12, 480 min trabalhados e 0 min de dobra de feriado. Em 11/12, folga compensatória com 0 min trabalhados e 0 de falta. Em 12/12, 240 min normais. DSR de 13/12 preservado.  
**FONTE** `app/docs/LEIS.md:77` · [Lei 605/1949, arts. 6º e 9º, linhas 28 e 67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [Súmula 146 do TST, linhas 1498–1502](https://www.tst.jus.br/documents/10157/cc23ec94-9e08-47e3-8ed6-2e662cee7690)

**CMP-04**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo, mensalista. Cadastro dado de feriado estadual em 13/12/2026, aplicável à praça Londrina/PR. A data coincide com o DSR da escala. Semana cumprida.  
**BATIDAS (UTC−3)** 07, 08, 09, 10 e 11/12, cada dia 08:00 E, 12:00 S, 13:00 E, 17:00 S. 12/12 08:00 E, 12:00 S. 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. Em 13/12, 0 min previstos de presença, 0 trabalhados, 0 de HE de feriado e 0 de falta. Um DSR preservado no salário mensal, com 0 min de dobra pelo encontro das duas datas.  
**FONTE** `app/docs/LEIS.md:64` · `app/docs/LEIS.md:74` · `app/docs/LEIS.md:77` · [Lei 605/1949, arts. 1º e 6º–9º, linhas 15–67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-05**  
**Família** escala  
**Escala** 12×36 diurno, 07:00–19:00  
**CADASTRO** Admissão 01/10/2026, vínculo ativo. Feriado nacional de 25/12/2026 cadastrado. Foto escala 25/12 como plantão. Acordo coletivo válido e sem cláusula de dobra. Pausa 12:00–13:00 e limites de jornada cumpridos.  
**BATIDAS (UTC−3)** 24/12 sem batidas. 25/12 07:00 E, 12:00 S, 13:00 E, 19:00 S. 26/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **01/2027**. Em 25/12, 660 min previstos e normais trabalhados, 0 de HE50, 0 de dobra de feriado, 0 de adicional noturno, 0 de intrajornada indenizada. DSR e feriado abrangidos pela remuneração mensal pactuada. A Resolução TST 225/2025 cancelou a Súmula 444 por perda de eficácia a partir de 11/11/2017.  
**FONTE** `app/docs/LEIS.md:73` · `app/docs/LEIS.md:77` · `app/docs/LEIS.md:118` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm) · [CLT, art. 59-A, linhas 333–334](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Resolução TST 225/2025, art. 1º, XXIV, p. 3](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/92286/Resolu%C3%A7%C3%A3o%20n.%20225%2C%20de%2030%20de%20junho%20de%202025.pdf?sequence=3#page=3)

**CMP-06**  
**Família** escala  
**Escala** 12×36 noturno, 19:00–07:00, CCT Vigilantes Londrina  
**CADASTRO** Admissão 01/10/2026, vínculo ativo e aplicação da CCT cadastrada. Feriado nacional de 01/01/2027 cadastrado. Foto escala plantão iniciado em 01/01. Pausa 21:00–22:00.  
**BATIDAS (UTC−3)** 31/12 sem batidas. 01/01 19:00 E, 21:00 S, 22:00 E. 02/01 07:00 S.  
**RESPOSTA DA REGRA** `lei: cláusula vigente que conceda dobra ao 12×36 dos vigilantes e defina sua base`. Na comp. **01/2027**, são certos 660 min trabalhados e 420 min na rubrica 0025 a 20%, com hora de 60 min. São 240 min trabalhados na data civil do feriado. Para atribuição da jornada, a L-085 usa 01/01, dia de início.  
**FONTE** `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:118` · `CLAUDE.md:227` · `CLAUDE.md:236` · `app/core/regua_cct.py:473` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm) · [CLT, art. 59-A, linhas 333–334](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm)

**CMP-07**  
**Família** escala  
**Escala** 12×36 noturno, 19:00–07:00, CCT Vigilantes Londrina  
**CADASTRO** Admissão 01/10/2026, vínculo ativo e aplicação da CCT cadastrada. Natal de 25/12/2026 cadastrado. Foto escala plantão iniciado em 24/12. Pausa 21:00–22:00.  
**BATIDAS (UTC−3)** 24/12 19:00 E, 21:00 S, 22:00 E. 25/12 07:00 S.  
**RESPOSTA DA REGRA** `lei: cláusula de dobra e corte de incidência por data civil que resolva a divergência com a L-085`. Na comp. **01/2027**, são certos 660 min trabalhados e 420 min na rubrica 0025 a 20%. O Natal toca fisicamente 25/12 das 00:00 às 07:00, 420 min, dos quais 300 dentro da janela noturna. A L-085 atribui o plantão a 24/12.  
**FONTE** `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:118` · `CLAUDE.md:236` · `app/core/regua_cct.py:473` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm)

**CMP-08**  
**Família** escala  
**Escala** 12×36 noturno, 19:00–07:00, CCT Vigilantes Londrina  
**CADASTRO** Admissão 01/10/2026, vínculo ativo e aplicação da CCT cadastrada. Feriado de 01/01/2027 cadastrado. Foto escala plantão iniciado em 31/12. Pausa 21:00–22:00.  
**BATIDAS (UTC−3)** 31/12 19:00 E, 21:00 S, 22:00 E. 01/01 07:00 S.  
**RESPOSTA DA REGRA** `lei: cláusula de dobra e corte de incidência por data civil que resolva a divergência com a L-085`. Na comp. **01/2027**, são certos 660 min trabalhados e 420 min na rubrica 0025 a 20%. O Ano-Novo toca fisicamente 01/01 das 00:00 às 07:00, 420 min, dos quais 300 dentro da janela noturna. A L-085 atribui o plantão a 31/12. A virada do ano civil permanece na mesma competência.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:118` · `CLAUDE.md:236` · `app/core/regua_cct.py:473` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm)

**CMP-09**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão e início de vínculo em 05/12/2026. Horista. DSR em 06/12. Sem feriado cadastrado no recorte.  
**BATIDAS (UTC−3)** 04/12 sem batidas. 05/12 08:00 E, 12:00 S. 06/12 sem batidas.  
**RESPOSTA DA REGRA** `lei: fórmula e arredondamento do DSR proporcional da primeira semana do horista admitido no sábado`. Na comp. **12/2026**, antes de 05/12 são 0 min previstos, 0 de falta e 0 de DSR creditável. Em 05/12 são 240 min previstos e normais trabalhados. O primeiro DSR a apurar é 06/12, na mesma competência.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:157` · [Lei 605/1949, arts. 6º e 7º, linhas 28–61](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-10**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão e início de vínculo em 21/11/2026. Horista. Feriado nacional de 20/11 cadastrado. DSR em 22/11.  
**BATIDAS (UTC−3)** 20/11 sem batidas. 21/11 08:00 E, 12:00 S. 22/11 sem batidas.  
**RESPOSTA DA REGRA** `lei: fórmula e arredondamento do DSR proporcional do horista admitido no primeiro dia da competência`. Em **11/2026**, 0 min previstos, trabalhados, de falta ou de DSR e nenhum crédito do feriado de 20/11 para este vínculo. Em **12/2026**, 240 min previstos e normais trabalhados em 21/11. O primeiro DSR a apurar é 22/11.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:157` · [Lei 14.759/2023, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2023/lei/l14759.htm) · [Lei 605/1949, arts. 6º–8º, linhas 28–64](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-11**  
**Família** escala  
**Escala** personalizada sem intervalo, 08:00–12:00 de segunda a sexta  
**CADASTRO** Admissão e início de vínculo em 20/01/2027. Horista. Repousos remunerados em 23 e 24/01. Sem feriado cadastrado no recorte.  
**BATIDAS (UTC−3)** 19/01 sem batidas. 20, 21 e 22/01, cada dia 08:00 E, 12:00 S. 23 e 24/01 sem batidas.  
**RESPOSTA DA REGRA** `lei: fórmula e arredondamento dos dois DSR proporcionais quando a primeira semana atravessa o corte`. São 0 min previstos e de falta antes da admissão, 240 min previstos e normais em **01/2027** no dia 20, e 480 min previstos e normais em **02/2027** nos dias 21 e 22. Os dois repousos a apurar caem em **02/2027**.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:157` · [CLT, art. 71, § 1º](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Lei 605/1949, arts. 6º–7º, linhas 28–61](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [EC 141/2026, arts. 1º e 10, linhas 9–26 e 49–52](https://www.planalto.gov.br/ccivil_03/constituicao/emendas/emc/emc141.htm)

**CMP-12**  
**Família** folha/export  
**Escala** personalizada sem intervalo, 08:00–12:00 de segunda a sexta  
**CADASTRO** Admissão 01/10/2026. Desligamento efetivado em 20/01/2027, último dia do vínculo. Sem feriado no recorte. Há pendência de célula que normalmente reteria o TXT. Os demais requisitos da emissão estão satisfeitos.  
**BATIDAS (UTC−3)** 18, 19 e 20/01, cada dia 08:00 E, 12:00 S. 21/01 sem batidas.  
**RESPOSTA DA REGRA** **01/2027** inclui o dia 20. São 720 min previstos e normais no recorte de 18 a 20, dos quais 240 no último dia. O movimento entra no TXT de **01/2027** e a pendência do desligado gera zero motivos de retenção. Em 21/01, já em **02/2027**, são 0 min previstos, 0 de falta e nenhum movimento para este vínculo.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · `app/docs/CORTES.md:517`. A inclusão no TXT é asserção do aval informado. Seu literal nos documentos consultados está [nao verificado].

**CMP-13**  
**Família** folha/export  
**Escala** personalizada sem intervalo, 08:00–12:00 de segunda a sexta  
**CADASTRO** Admissão 01/10/2026. Desligamento efetivado em 21/12/2026, último dia do vínculo. Descansos em 19 e 20/12. Sem feriado no recorte. Demais requisitos de emissão satisfeitos.  
**BATIDAS (UTC−3)** 18/12 08:00 E, 12:00 S. 19 e 20/12 sem batidas. 21/12 08:00 E, 12:00 S. 22/12 sem batidas.  
**RESPOSTA DA REGRA** 240 min normais de 18/12 no TXT de **12/2026** e 240 min normais de 21/12 no TXT de **01/2027**. Zero motivos de retenção do desligado em ambas. A partir de 22/12, 0 min previstos e 0 de falta.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · `app/docs/CORTES.md:517`. A inclusão no TXT é asserção do aval informado. Seu literal nos documentos consultados está [nao verificado].

**CMP-14**  
**Família** folha/export  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026. Desligamento efetivado em 10/12/2026, último dia do vínculo. Sem feriado cadastrado neste recorte. Demais requisitos de emissão satisfeitos.  
**BATIDAS (UTC−3)** 07, 08, 09 e 10/12, cada dia 08:00 E, 12:00 S, 13:00 E, 17:00 S. 11, 12 e 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. São 1.920 min previstos e normais no recorte de 07 a 10/12, dos quais 480 no último dia. O movimento entra no TXT, com zero motivos de retenção do desligado. Em 11, 12 e 13/12, 0 min previstos, 0 de falta e nenhum DSR por data posterior ao fim do vínculo.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · `app/docs/CORTES.md:517` · `app/docs/DOSSIES.md:135`. A inclusão no TXT é asserção do aval informado. Seu literal nos documentos consultados está [nao verificado].

**CMP-15**  
**Família** ausência/férias  
**Escala** 6×1 comercial nos dias do recorte  
**CADASTRO** Admissão 01/10/2025, vínculo ativo, mensalista. Férias já concedidas de 15/11 a 14/12/2026, inclusive. Feriados nacionais de 15 e 20/11 cadastrados.  
**BATIDAS (UTC−3)** Cada dia de 15/11 a 14/12, sem batidas. 15/12 08:00 E, 12:00 S, 13:00 E, 17:00 S.  
**RESPOSTA DA REGRA** **11/2026** recebe 6 dias corridos de gozo, de 15 a 20/11. **12/2026** recebe 24, de 21/11 a 14/12. O dia 21/11 está em férias. Os DSR de 15, 22 e 29/11 e de 06 e 13/12 estão dentro do gozo. Durante o gozo são 0 min de trabalho, falta, HE ou DSR pago à parte. O retorno em 15/12 dá 480 min normais em **12/2026**. O terço mínimo incide sobre a remuneração dos 30 dias e é pago com ela até 13/11, data que cai em **11/2026**. `lei: competência e formato da rubrica de férias e terço no TXT`.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · [CLT, arts. 130, 142 e 145](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Constituição, art. 7º, XVII, linha 295](https://www.planalto.gov.br/ccivil_03/constituicao/constituicao.htm) · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm)

## Perguntas de lei

1. **Dobra do 12×36, CMP-05 a CMP-08.** Qual cláusula ou corte vigente concede dobra ao plantão previsto? Para o cadastro sem cláusula favorável, o aval confirma o piso do art. 59-A? `app/docs/LEIS.md:118` · `app/core/regua_cct.py:473` · [CLT, art. 59-A, linhas 333–334](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Resolução TST 225/2025, art. 1º, XXIV, p. 3](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/92286/Resolu%C3%A7%C3%A3o%20n.%20225%2C%20de%2030%20de%20junho%20de%202025.pdf?sequence=3#page=3)
2. **Virada do feriado, CMP-06 a CMP-08.** Havendo dobra, qual corte autoriza calcular a rubrica pelas horas da data civil, inclusive a sobreposição com a 0025, diante da atribuição pelo início na L-085? `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `CLAUDE.md:236`
3. **DSR da primeira semana, CMP-09 a CMP-11.** Qual base, divisor e arredondamento convertem o DSR proporcional do horista em minutos? Como aplicar a fórmula aos dois repousos quando a semana cruza o dia 20/21? `app/docs/LEIS.md:77` · [Lei 605/1949, arts. 6º–7º, linhas 28–61](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [EC 141/2026, arts. 1º e 10, linhas 9–26 e 49–52](https://www.planalto.gov.br/ccivil_03/constituicao/emendas/emc/emc141.htm)
4. **Férias e terço no TXT, CMP-15.** Qual regra atribui o pagamento no TXT quando o gozo atravessa o corte? Qual unidade a rubrica deve carregar? `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · [CLT, arts. 142 e 145](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm)

## 27/11 15:08 · sessao 9139767f560b · - · - · origem=Ronald (tmux)

Auditei as 50 obras de `PERGUNTA-VIVA-COM-CELULA-ABSOLVEU` a `F-346-FILA-OPUS` e enviei a tabela em três blocos para consolidação. Os endereços do anexo divergem da cópia em várias obras. Por exemplo, a primeira consta em `app/docs/BACKLOG.md:2451` e a última em `app/docs/BACKLOG.md:2508`. Estado de produção e confirmação do hash Git ficam como [nao verificado].

## 27/11 15:08 · sessao 39cd61c86174 · - · - · origem=Ronald (tmux)

**D.** A view chama `recalcular_fechamento_mes(11, 2026, empresa_id=e.pk, somente_leitura=True, retornar_objeto=True)`, usa os valores do dicionário como `fechamentos`, chama `classificar_export(e, 11, 2026, fechamentos=fechamentos)` e passa os mesmos objetos com `itens=itens` a `montar_linhas_export`. Em seguida chama `validar_layout_dominio(linhas)`. A montagem devolve `(linhas, entra_ids)` em memória. A aprovação e o registro do TXT ficam na view. [app/folha/views.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:214) [app/folha/views.py:258](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:258) [app/folha/views.py:275](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:275) [app/folha/views.py:282](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:282) [app/folha/views.py:304](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:304) [app/folha/export.py:699](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:699)

Para atribuir os erros, monte novamente com `[fechamento]` e `[item]` na ordem original e use `fechamento.colaborador_id`. Preserve cada bloco inteiro, pois o validador conta os registros 11 depois da rubrica 8792 conjuntamente. Confronte a concatenação das linhas e dos erros com a rodada da empresa. Publique somente a classificação da mensagem e seu campo, pois algumas mensagens embutem o registro. [app/folha/export.py:266](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:266) [app/folha/export.py:725](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:725) [app/folha/export.py:734](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:734)

Fixe o relógio da medição com `freeze_time` e capture a hora real do cabeçalho antes do congelamento. O fechamento lê o relógio internamente e captura exceções por colaborador. Compare as chaves retornadas com `universo_da_competencia` e falhe com os IDs ausentes, ou propague a exceção original no ponto de log. [app/requirements.txt:39](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/requirements.txt:39) [app/ponto/services/fechamento.py:574](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:574) [app/ponto/services/fechamento.py:636](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:636) [app/ponto/services/fechamento.py:1327](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1327)

**E.** O cadastro é `core.models.Feriado`, tabela `core_feriado`. Use `Feriado.objects.filter(abrangencia='municipal', data__range=(date(2026, 11, 21), date(2026, 12, 20))).order_by('uf', 'municipio', 'data', 'pk')`. Para cruzar por empresa e praça, leia os pares `Posto.praca` e `Posto.municipio` da empresa e use `core.feriados.alcanca(registro, posto.municipio)`, que normaliza UF e nome do município. [app/core/models.py:5](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/models.py:5) [bin/sombra_regras_pessoais.py:194](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sombra_regras_pessoais.py:194) [app/colaboradores/models.py:219](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/models.py:219) [app/core/feriados.py:57](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:57)

O cenário de teste identifica **10/12/2026, Aniversário de Londrina/PR**. A presença do registro na Sombra permanece [nao verificado]. [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:8](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:8) [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62)

**Envelope.** O adaptador existente encaminha a `--sombra`. Configure-o via `CODEX_SONDA_ENVELOPE` e a árvore fixada via `SONDA_ARVORE`. Capture exclusivamente a exceção sentinela após o `atomic` reverter, pois o wrapper descarta toda a saída quando o envelope termina com código diferente de zero. [Adaptador da Sombra](/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh:3) [bin/sonda_leitura.sh:226](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:226) [bin/codex_pergunta.sh:628](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:628)

## 27/11 15:07 · sessao d1951f170791 · - · - · origem=Ronald (tmux)

Auditei os 40 IDs atribuídos e encaminhei as linhas e os cruzamentos com o placar. Os sítios, a classificação e as ressalvas de cada obra foram enviados nos três blocos. A confirmação do hash da cópia e a incidência atual em produção permanecem [nao verificado].

_8719 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
