# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 16:23** · publicado em **27/11/2026 16:23** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 16:23 · sessao df030164fcae · FERIADO-BOTAO-CORTADO · construtor · origem=Code

A c11 põe o gatilho do feriado acima da região rolável na ordem de pintura da célula. O template isola o empilhamento da célula mensal e usa `z-index:1` na ação e `z-index:0` na região. A célula segue com 132 px, e as marcações continuam no fim da região rolável, como na c10. A guarda B1 do histórico permanece no JS. [_calendario_grade.html:68](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68) · [Ação e região:95](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95) · [Ordem das marcações:132](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:132) · [Guarda B1:27](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:27)

Os **104 px** vêm da sonda. Ela posiciona a ação sobre o conteúdo da célula para deixar pouco espaço vertical no drawer de 128 px. No s11, o retângulo cabe na célula e na viewport, mas a sonda também exige que o centro receba o clique. A correção trata a sobreposição com a grade rolável da c10. Qual elemento recebeu o clique no s11 é **[não verificado]**. [Posicionamento:277](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:277) · [Medição:291](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:291)

O **RED** é o próprio teste que acusou as quatro falhas do s11. O mutante novo `drawer-sem-empilhamento-da-acao` volta `isolation` e `z-index` a `auto` só no drawer, repondo o empilhamento da c10. A revisão estática confirmou que a sonda, as asserções, os esperados e os mutantes anteriores são idênticos aos da c10. Contagem no diff **0 `except`, 0 `catch`**. [Mutante:78](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:78) · [Teste RED:550](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:550) [nao verificado]

**Recorte para a sessão na VM**, com o smoke de clique e histórico, a renderização da grade e os selos do popover. [Uso de `--only`:13275](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/docs/RELATO.md:13275)

```bash
PYTHONDONTWRITEBYTECODE=1 bash bin/suite.sh --only "core.tests.test_smoke_clique_feriado_vira_trabalho colaboradores.tests.test_feriado_botao_cortado core.tests.test_selo_popover_acao"
```

**Comando do mutante** no ambiente de testes da VM. [Seleção do mutante:550](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:550) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 SMOKE_FERIADO_MUTANTE=drawer-sem-empilhamento-da-acao \
  python manage.py test --settings=config.settings.ci --noinput \
  core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas
```

Resultado da c11 em Chromium e falha do mutante pelo motivo esperado **[não verificado]**. A certificação fica com a suite e o opus final. [nao verificado]

## 27/11 16:16 · sessao b845a7dd5be1 · FERIADO-UM-JUIZ · verificador · origem=Code

# REPROVADO

1. **K1 continua aberto.** O turno partido de `10:00–14:00 + 14:30–18:20` leva **470 min à HE100 feriado** na promoção. O corte do K1 exige **440 min de HE100 feriado e 30 min de intra indenizada**. O laço do motor que apura a supressão percorre `periodos`, e a promoção soma os minutos de `periodos_ft`. O teste continua como `expectedFailure`. (`app/ponto/motor_calculo_v2.py:3078–3118`, `app/ponto/services/fechamento.py:993–997`, `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394`, `app/docs/RELATO.md:255`)
2. **A certeza da escala ainda pode decidir quantos minutos chegam à partição.** Com previsto zero, empresa sujeita à janela e sem autorização, a L-097 conserva o par de folga com escala incerta e zera o par com escala certa. O teste que cobra a mesma quantidade nas duas rubricas está em `skip`. A precedência entre esse bloqueio e o aval literal continua pendente. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/LEIS.md:115`, `app/ponto/motor_calculo_v2.py:1562–1597`, `app/ponto/tests/test_feriado_um_juiz_porta.py:499–515`)
3. **A vigência protege a classificação antiga da 10, mas o aval também veda tocar exportada.** A guarda específica cobre a 09. Nas demais, o evento pode regravar o mensal e as duas lavras. Na 11 e seguintes, isso já usa a regra nova mesmo se houver TXT entregue. O POST pode gerar outra fotografia a partir do cálculo online. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/ponto/services/fechamento.py:36–50,600–639,1189–1223,1294–1328,1829–1855`, `app/folha/views.py:214–219,269–275,302–307`)
4. **A data e o vínculo ainda podem divergir.** Reproduzi em memória o turno do dia 21 cair na competência anterior quando o feriado é no dia 20. Também reproduzi na partição 4 h de HE100 feriado para um par cujo dia do turno pertence a 12x36, usando o contexto 6x1 que trouxe o par e a data herdada da véspera. (`app/ponto/services/fechamento.py:154–172,223–230`, `app/ponto/motor_calculo_v2.py:2513–2522,2537–2545`, `app/ponto/calculador/regras.py:410–431`)
5. **As três gravações ainda podem divergir.** O `atomic` protege o `delete` e o `bulk_create` de uma versão de `DiaPago`. O mensal é salvo antes, e as falhas das duas lavras são capturadas separadamente. Uma falha pode deixar o mensal novo com uma ou ambas as lavras antigas. (`app/ponto/services/dia_pago.py:355–363`, `app/ponto/services/fechamento.py:1166–1223,1267–1270,1328–1332`)
6. **Três testes estruturais falham na própria cópia.** Executei os testes de AST em memória. A catraca de endereços registra cinco sítios divergentes, incluindo `folha/export.py:505` onde o censo encontra `:514`. Os dois testes do dia da lavra rejeitam a atribuição `dia_jornada = dia_turno`, embora a linha seguinte atualize o mapa. (`app/core/tests/test_medida_turno_do_par.py:286–289,326–336,565–569`, `app/folha/export.py:514`, `app/ponto/tests/test_noturno_v8_ft_no_dia_da_lavra.py:46–71`, `app/ponto/calculador/regras.py:420–430`)
7. **Pouso e apply [nao verificado].** O aval exige medir antes os 75 dias de 42 colaboradores como hora comum, excluir os intermitentes e entregar reversão, antes × depois por colaborador e DIFF do TXT 11. A prova desses itens, a suíte inteira neste hash e o pouso de E1 antes do K1 estão [nao verificado]. O K1 tem aval separado e condicionado ao DIFF de frota por rubrica antes do pouso. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/RELATO.md:252,255`, `app/docs/PROMPTS.md:3016`, `CLAUDE.md:448–467`)

## Ver3, achado por achado

| Achado do ver3 | Situação na c4 |
|---|---|
| **1. TXT perde a primeira fatia, `cadastro_zero` ou resumo perde a rubrica** | **Curado no caso reproduzido.** Emissão, homologação e resumo do cartão usam `eventos_do_fechamento_da_competencia`. A 11 admite FT promovida com `horas_trabalhadas=0` quando a rubrica está cadastrada e o colaborador está sujeito ao ponto. O comparador acusa a rubrica ausente com HE100 bruta igual. O PDF discrimina a parcela no resumo. (`app/folha/export.py:388–394,750–760,924–926,1023–1032`, `app/relatorios/cartao_pela_celula.py:89–101,250–277`, `app/relatorios/pdf_espelho.py:771–779`, `app/ponto/tests/test_feriado_um_juiz.py:56–99`) |
| **2. Exportada fora da guarda pode mudar** | **Curado para a promoção retroativa da FT incerta até a 10. Aberto para a proteção do gravado.** A fixture da 10 força `entra`, cadastra apenas a rubrica de feriado e chama `registrar` diretamente. Nesse caso, a fotografia anterior tem zero linhas de eventos. O teste compara seis campos do mensal, campos escolhidos das lavras e o registro anterior. A reemissão pelo POST e a 11 já entregue ficam sem essa cobertura. (`app/ponto/calculador/regras.py:393–409`, `app/ponto/tests/test_feriado_um_juiz_porta.py:151–160,239–296`, `app/folha/export.py:539–548,750–765`, `app/folha/views.py:214–219,269–275,302–307`) |
| **3. Oráculo e DIFF redecidem a dobra** | **Curado para a HE100 feriado normal alimentada a partir da 11.** O oráculo recebe os minutos dos períodos da fatia, inclusive zero com calendário injetado vazio. `periodo.reler` também passa a carregar os minutos do período. O DIFF usa a mesma projeção FT nos dois lados, então `4,00 = 4,00` prova a ligação da partição com o comando. O caminho do oráculo ainda calcula intra e, fora da projeção promovida, noturno e splits com a régua do recorte. (`app/ponto/calculador/alimentacao.py:429–446,518–523,571–597`, `app/ponto/calculador/periodo.py:764–769,824–843`, `app/ponto/calculador/regras.py:793–817,909–932`, `app/ponto/management/commands/diff_calculador.py:244–257,403–415`) |
| **4. Split bruto sobrescrito ou acima do total** | **Curado no recorte da FT promovida.** A c4 preserva o campo bruto, limita o split aos minutos trabalhados e usa a união da janela no plantão longo com prorrogação. O controle em memória de 21h a 23h do dia seguinte deu **1.500 min de split**, com adicional legal de **1.500 ÷ 0,875 min**. A correção do bruto em outros caminhos fica fora desse ramo. (`app/ponto/calculador/regras.py:432–447`, `app/ponto/motor_calculo_v2.py:82–118,151–161`, `app/ponto/tests/test_feriado_um_juiz.py:331–365`, `app/ponto/tests/test_feriado_um_juiz_porta.py:572–581`) |
| **5. K1 e dia do turno** | **K1 aberto. Data parcialmente curada.** Há asserções de data e rubrica nas duas lavras para dois turnos e troca de praça. A correção do mapa é condicional ao ciclo, ao feriado e à presença do dia na fatia. O corte de data e a classificação original do período já aconteceram quando ela roda. (`app/ponto/tests/test_feriado_um_juiz_porta.py:380–394,455–497`, `app/ponto/calculador/regras.py:410–431`, `app/ponto/motor_calculo_v2.py:2537–2574`) |
| **6. Soma, idempotência e falha da relavra** | **Curado o rollback de uma versão. Paridade parcial.** O teste sem promoção exige **8,04 h** no mensal para `241 + 241 min`. A soma diária desses minutos arredonda para **8,03 h**. Com uma FT promovida de 10 segundos, mesmo aparecendo como `0,00 h` de feriado no teste, o mensal passa para **8,03 h** nas horas comuns. Duas execuções bem-sucedidas podem repetir os valores, mas recriam linhas e carimbos. A prova de concorrência e de atomicidade entre mensal e versões está [nao verificado]. (`app/ponto/services/fechamento.py:1005–1013`, `app/ponto/services/dia_pago.py:344–363,552–565`, `app/ponto/models.py:373–387`, `app/ponto/tests/test_feriado_um_juiz_porta.py:396–453`) |

## Aval, vigência e alcance

- **A regra principal entrou.** Em 6x1, a FT já apurada consulta `_feriado_dobra_no_dia` pelo dia do turno. Com dobra devida, a FT incerta vira HE100 feriado. Com escala certa, `feriado_vira_folga` separa HE100 feriado de folga trabalhada a 100%. O diff fornecido preserva `motor_calculo_v2.py` e `CICLOS_FERIADO_SIMPLES`. O intermitente é excluído por base ou ciclo e seu motor injeta calendário vazio. Um período da própria fatia 12x36 fica fora da promoção. A emissão e a troca de vínculo ainda admitem os casos dos bloqueios 3 e 4. A cláusula cadastrada da CCT no caminho regular 12x36 continua no código. (`diff.txt:1`, `app/ponto/calculador/regras.py:314–322,367–377,410–431`, `app/ponto/motor_calculo_v2.py:941–978,3686–3698,3740`, `app/core/regua_cct.py:466–479`)
- **A partir da competência 11/2026**, mudam a promoção, o consumo da HE100 feriado no TXT e no cartão, a projeção nova para o oráculo, o ajuste noturno da FT promovida e o arredondamento em mensal com promoção positiva. Até a 10, a partição e a emissão históricas conservam o critério anterior. A borda final muda já na 11 e a inicial só na 12. O `atomic` da lavra, a comparação do cartão e a exibição no PDF valem para todas as chamadas. `periodo.reler` também mudou sem parâmetro de competência. (`app/folha/export.py:385–394,924–926`, `app/ponto/calculador/regras.py:386–447`, `app/ponto/services/fechamento.py:223–230,989–1013,1294–1305`, `app/ponto/services/dia_pago.py:358–362`, `app/ponto/calculador/periodo.py:539,764–769`, `app/relatorios/cartao_pela_celula.py:266–273`, `app/relatorios/pdf_espelho.py:771–779`)
- **Transportar a rubrica da fatia para o TXT cabe no aval.** Corrigir a data da rubrica preserva o objetivo enquanto usa o turno que já foi julgado. O filtro de batidas da borda atua **antes** do motor e pode alterar a apuração de um turno comum. A união altera também `horas_noturnas`, que sai como adicional no TXT. O arredondamento condicionado à promoção altera horas comuns de outros dias do mesmo mensal. Esses efeitos vão além da redistribuição entre HE100 feriado e folga trabalhada. A faixa de impacto na frota está [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/ponto/services/fechamento.py:154–172,339–348,998–1013`, `app/folha/export.py:453–462`, `CLAUDE.md:274–277`)
- **A 10 fica igual ao gravado?** O corte preserva a regra para os mesmos insumos. Igualdade com o gravado real, com ambas as lavras e com o TXT vigente [nao verificado]. A própria cópia registra que a 10 foi paga à mão, sem TXT, e saiu da guarda. A condição “10 exportada” está na fixture e na pergunta. A L-113 permite recálculo genérico fora da guarda, enquanto o aval da obra exige preservar exportadas. (`app/ponto/services/fechamento.py:36–50,600–639`, `app/ponto/tests/test_feriado_um_juiz_porta.py:239–289`, `app/docs/LEIS.md:125,155`, `CLAUDE.md:458–470`)

## Escritores, leitores e invariantes

| Valor | Quem escreve e quem lê |
|---|---|
| **HE100 feriado, HE100 total e folga trabalhada** | O v2 escreve os minutos dos períodos normais. `particionar_ft` produz as três listas em memória. O fechamento soma e grava o mensal, e `lavrar` grava as linhas diárias. TXT e cartão leem o mensal passado a eles. A emissão calcula HE100 comum como HE100 total menos a parte de feriado. `horas_extras_100_feriado` agrega feriado normal e FT promovida, sem guardar a origem no mensal. No dia, `folga_feriado_min` identifica só a FT promovida. (`app/ponto/motor_calculo_v2.py:328–344`, `app/ponto/calculador/regras.py:392–452`, `app/ponto/services/fechamento.py:984–1006,1193–1199`, `app/ponto/services/dia_pago.py:173–186,235–257`, `app/folha/export.py:434–460`, `app/relatorios/cartao_pela_celula.py:90–101`) |
| **`horas_trabalhadas` e previsto** | O mensal e a lavra somam períodos normais, órfãos e FT comum. O campo exclui FT com rubrica própria. `pago_do_dia` acrescenta folga trabalhada e o detalhe da FT promovida, portanto responde a uma pergunta diferente. O export lê `horas_trabalhadas` para classificação e prontidão. O previsto do fechamento vem da grade da célula e o realizado limita o abono, sem entrar na soma do previsto. Há uma exceção preexistente na grade por turno do intermitente convocado, que copia o realizado para o previsto. (`app/ponto/services/fechamento.py:1005–1013,1075–1093`, `app/ponto/services/dia_pago.py:258–271,388–402,892–900`, `app/folha/export.py:329–338,853–858,924–926,1058–1074`, `app/escala/services/leitor_celula.py:399–423`, `app/escala/utils.py:2099–2124`) |
| **Split noturno e adicional** | O v2 escreve o split bruto e os minutos noturnos legais. A partição escreve `FolgaEmFeriado` com o split de relógio e o adicional legal da promoção. Mensal, lavra e projeção leem esses valores. O TXT emite o adicional legal em evento próprio. O split de HE100 é subconjunto da HE100. (`app/ponto/motor_calculo_v2.py:1013–1049`, `app/ponto/calculador/regras.py:380–383,432–447`, `app/ponto/services/fechamento.py:998–1004`, `app/ponto/services/dia_pago.py:244–252`, `app/ponto/calculador/alimentacao.py:369–388`, `app/folha/export.py:438–461`) |
| **Rubricas do oráculo e `lavrar(rubricas=...)`** | `alimentacao` projeta a partição e lê os valores da primeira fatia para a dobra normal. `do_dia` devolve `valores` e `nao_decide`. Desde a 10 existe a versão `oraculo`; da 11 em diante ela recebe a partição nova. A mesma `lavrar` valida donos declarados, monta a base com os períodos e substitui os campos decididos. A soma lê por versão. (`app/ponto/calculador/alimentacao.py:353–389,518–523,571–597`, `app/ponto/calculador/regras.py:787–817,954–961`, `app/ponto/services/fechamento.py:1294–1328`, `app/ponto/services/dia_pago.py:50–76,311–346,552–565`) |
| **Data, contexto da fatia e `_FERIADO_CLASSIFICADO_NO_MENSAL`** | O motor escreve o dia do turno por entrada crua e o mapa da jornada por período. A composição guarda `(motor, vínculo)` por identidade e por dia. A partição lê os dois e também escreve no mapa da jornada. Lavra, oráculo e DIFF leem esse mapa. A data civil é a resposta de contingência quando falta mapeamento. O literal `(2026, 11)` mora no export e é lido por emissão, classificação, partição, fechamento e DIFF. Competência é o rótulo da janela da empresa, vigência do vínculo é quem cobre cada data, e o corte da regra é um terceiro valor. Chamadas avulsas sem competência têm comportamento próprio. (`app/ponto/motor_calculo_v2.py:911–922,963–978,2513–2522`, `app/ponto/services/fechamento.py:223–243,335–350,377–382`, `app/ponto/calculador/regras.py:386–431,679–686`, `app/ponto/services/dia_pago.py:109–132`, `app/folha/export.py:385–394,924–926`, `app/ponto/management/commands/diff_calculador.py:112–152,203–215`, `app/ponto/janelas.py:10–21,54–63`, `app/escala/alimentacao.py:205–264`) |

**Juízes, portas e leitores.** F3 continua no motor, e o auxiliar do oráculo empresta seu método. O segundo escritor introduzido é **em memória**, no mapa `dia_da_jornada`. A borda chama o juiz de turnos para retirar batidas e depois entrega ao motor um conjunto diferente para novo pareamento. A partição é disjunta por ocorrência, e suas três listas somam a FT que chegou até ela. Isso não garante que todos os minutos tenham chegado à competência certa nem que mensal, dia e TXT usem a mesma fotografia. O cartão exige a existência do mensal gravado e então recalcula online. O PDF usa o mesmo resumo na janela de competência. O POST do TXT também calcula online, e o comparador `cartao_x_txt` usa o **mensal gravado** como referência. Já `diff_calculador` recalcula ou intercepta a apuração em memória. O DIFF entre arquivos entregues lê os conteúdos armazenados. (`app/core/juizes.py:318–320`, `app/ponto/calculador/regras.py:301–322,392–452`, `app/ponto/services/fechamento.py:339–348,1766–1792`, `app/relatorios/cartao_pela_celula.py:69–76,250–273,324–350`, `app/relatorios/pdf_espelho.py:628`, `app/folha/views.py:214–219,269–275`, `app/ponto/management/commands/diff_calculador.py:114–152`, `app/folha/haiku_export.py:79–107`)

**Relógio e travas.** O diff não adiciona leitura de relógio de parede. O corte usa `(ano, mes)` e o fechamento já captura `_agora_da_passada` uma vez. Não identifiquei inversão explícita nova. A entrega trava Empresa, e a revisão específica segue Empresa → Colaborador → Batida. O evento genérico roda em `on_commit` sem essas travas. A transação local da lavra não serializa entrega, mensal e as duas versões. (`diff.txt:1`, `app/ponto/services/fechamento.py:600–604,678–680,1829–1855`, `app/folha/views.py:102–104,132–135`, `app/folha/export.py:570–576`, `app/ponto/services/par_relampago_na_lista.py:172–187`, `app/ponto/services/dia_pago.py:358–362`)

## Contraexemplos e controles adicionais

- **20/21.** Em memória, com dois turnos `20/11 18–22h` e `21/11 03–07h` e feriado **no dia 20**, a 11 recebeu **4 h de feriado e 4 h comuns no dia 20**, e a 12 recebeu zero. Com feriado em **21/10**, a 10 recebeu as 8 h comuns e a 11 recebeu zero. A fixture de borda cobre feriado em **21/11**, quando os dois lados do filtro já atuam. Um único turno `20/11 22h → 21/11 07h` com feriado só no dia 21 ficou inteiro na 11, como hora comum. (`app/ponto/services/fechamento.py:154–172,223–230`, `app/ponto/motor_calculo_v2.py:2513–2545`, `app/ponto/tests/test_feriado_um_juiz.py:391–425`)
- **Municipal × estadual e troca no meio do mês.** Londrina e Cambé compartilham um feriado estadual do PR. Um municipal de Cambé alcança só o vínculo de Cambé. O carregador resolve a praça pelo vínculo de cada dia. Na troca **6x1 → 12x36**, a partição pode impedir a troca de contexto pelo ciclo de destino e conservar a dobra já decidida pelo 6x1 de origem. Reproduzi esse ramo com 240 min e confirmação de que a data lavrada ficou na véspera. Ocorrência na frota [nao verificado]. (`app/core/feriados.py:57–67,135–167`, `app/ponto/calculador/regras.py:410–431`, `app/ponto/tests/test_feriado_um_juiz_porta.py:470–497,595–615`)
- **Intermitente e desligado no dia 20.** Um FT intermitente por base ou ciclo fica em trabalhadas na partição. Para desligamento em 20/11 com corte 21, o colaborador ainda entra no universo de apuração da 11. A grade considera o fato lavrado no dia da demissão, e o classificador manda as verbas para a rescisão antes de avaliar o feriado emitível. Assim, 4 h no mensal podem coexistir com zero linha no TXT mensal por rescisão. O valor no módulo de rescisão está [nao verificado]. (`app/ponto/calculador/regras.py:367–377`, `app/ponto/services/fechamento.py:489–506`, `app/ponto/janelas.py:10–21`, `app/escala/services/leitor_celula.py:379–398`, `app/folha/export.py:54–60,878–893,924–926`)

## Selos e `except`

- **35 controles sem banco passaram. Os 33 mutantes sem banco mordem código de produção:** todos passaram no controle e provocaram falha de asserção no alvo mutado. Há ainda **11 mutantes de porta**, dez com alvo em produção e um de fixture que exige erro de unicidade. A execução dos 11 e os REDs contra os commits-base estão [nao verificado]. O mutante `endereco-c2` altera o registro do teste, e o controle correspondente já falha nesta cópia. (`app/ponto/tests/feriado_um_juiz_mutantes.py:42–219,273–318`, `app/core/tests/test_medida_turno_do_par.py:47–48,565–569`)
- **`except` no diff fornecido: 0 adicionados e 0 removidos, inclusive em testes.** Os `except` do comando de DIFF, da alimentação e das duas lavras já estão no contexto do diff. `assertRaisesRegex` e `finally` nos testes não são cláusulas `except`. (`diff.txt:1`, `app/ponto/management/commands/diff_calculador.py:130–140`, `app/ponto/calculador/alimentacao.py:429–455,552–560`, `app/ponto/services/fechamento.py:1267–1270,1328–1332`, `app/ponto/tests/test_feriado_um_juiz.py:20–40`)

## 27/11 16:15 · sessao 080e3483b92e · FERIADO-BOTAO-CORTADO · construtor · origem=Code

A c10 coloca as marcações no fim da área rolável dos feriados com ação oferecida no modo mês. A célula mantém os **132 px**, e o botão fica acima da região rolável. [Grade:68](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68) · [Grade:95](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95) · [Grade:132](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:132)

**Causa provável.** Na ficha `420px 1fr`, o anexo mede uma célula de **102 × 132 px** e deixa **no máximo 70,41 px** abaixo do botão, descontados os 3 px de margem e os 6 px de padding e borda inferiores. A região da c9 podia ter ações e faixas depois de `17:00`. Chegar ao máximo (**145 de 145 px**) alinhava o fim de todo esse conteúdo, e `17:00` continuava invisível. O anexo traz o resultado agregado da sonda. A caixa de `17:00` e o ponto exato do recorte ficam **[nao verificado]**. `s10_falha.txt:4` · [Ficha:87](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:87) · [Grade:138](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:138) · [Sonda:428](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:428)

**RED e mutantes.** O RED é o smoke existente na c9. O s10 registrou **19 testes, 1 falha**, em `ultimaVisivelNoFim`. O diff de teste acrescenta duas entradas CSS e **0 `except`/`catch`**. A sonda, a asserção, os valores esperados e o teste B1 de histórico ficaram idênticos byte a byte. A execução da c10 e dos mutantes fica com o Code **[nao verificado]**. `s10_falha.txt:3–9` · [Mutantes:76](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:76) · [Smoke e B1:545](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:545) [nao verificado]

```bash
# RED na c9. Na c10, o mesmo comando exercita a cura.
alvo=core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas
PYTHONDONTWRITEBYTECODE=1 python manage.py test --settings=config.settings.ci "$alvo"

# M1 — restaura o fluxo em bloco da c9
PYTHONDONTWRITEBYTECODE=1 SMOKE_FERIADO_MUTANTE=rolagem-em-bloco python manage.py test --settings=config.settings.ci "$alvo"

# M2 — mantém o grid e devolve as marcações à ordem anterior
PYTHONDONTWRITEBYTECODE=1 SMOKE_FERIADO_MUTANTE=marcacoes-antes-dos-detalhes python manage.py test --settings=config.settings.ci "$alvo"
```

**Leitores.** A ficha e a aba Espelho do drawer carregam o calendário mensal. A fita de diagnóstico usa `layout='fita'`, `relatorios/furos.html` inclui a grade sem `layout`, e o Espelho de Ponto avulso tem grade semanal própria. A mudança de CSS vale somente para `layout='mes'` com a ação de feriado oferecida ao usuário. [Ficha:215](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:215) · [Aba Espelho:134](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:134) · [Calendário:67](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/calendario_partial.html:67) · [Fita:35](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_drawer_diagnostico.html:35) · [Furos:42](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/relatorios/furos.html:42) · [Espelho avulso:97](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/ponto/espelho.html:97)

**Recorte.** O `--only` literal do s10 está **[nao verificado]**. O anexo traz a contagem, e este recorte foi reconstruído para 19 testes. `s10_falha.txt:7` [nao verificado]

```bash
--only "colaboradores.tests.test_feriado_botao_cortado colaboradores.tests.test_ui_pilulas_quebram_linha core.tests.test_selo_popover_acao core.tests.test_smoke_clique_feriado_vira_trabalho core.tests.test_smoke_retratar_1366"
```

## 27/11 16:05 · sessao 1a2ab4657839 · FERIADO-UM-JUIZ · construtor · origem=Code

A c4 está na cópia sobre `29488e3bb`. Em memória, os **9 REDs falharam na c3**, com 12 falhas de asserção contando os subcasos. Na c4, passaram **35 testes e 33 mutantes**. O diff acrescenta **0 `except`**. Os testes de porta com banco e a certificação pelo Code e pelo opus final ficam [nao verificado]. (`ponto/tests/feriado_um_juiz_mutantes.py:261`, `ponto/tests/feriado_um_juiz_mutantes.py:279`)

| Item | RED e prova na c3 | Cura e leitores |
|---|---|---|
| **A** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_cartao_le_a_rubrica_emitida` e `::test_RED_c4_cartao_x_txt_acusa_feriado_ausente_com_he100_igual`. O TXT emitiu `400`, o cartão mostrou `0 h` e o contador devolveu `{}` ao perder a rubrica com HE100 bruta igual. (`ponto/tests/test_feriado_um_juiz.py:56`, `:72`) | Cartão, montagem do TXT e homologação leem os mesmos eventos do mensal. O PDF discrimina HE100 feriado e o contador confere a rubrica e o total. App e ficha leem o total. (`folha/export.py:388`, `:753`, `:1026`, `relatorios/cartao_pela_celula.py:89`, `:266`, `relatorios/pdf_espelho.py:773`, `api/views.py:1663`, `colaboradores/services/ficha.py:205`) |
| **B** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_vigencia_conserva_ft_incerta_em_trabalhadas_ate_outubro`. Na c3, outubro classificou os 240 min como HE100 feriado. O teste de porta `ponto/tests/test_feriado_um_juiz_porta.py::test_RED_c4_evento_na_10_entregue_conserva_a_vigencia_e_a_11_aberta_promove` cobre mensal, duas lavras, TXT vigente e controle na 11 aberta. Execução da porta [nao verificado]. (`ponto/tests/test_feriado_um_juiz.py:300`, `ponto/tests/test_feriado_um_juiz_porta.py:239`) | Partição, lavras e DIFF usam a vigência do TXT. Até outubro, a partição segue a seleção por escala certa e a cláusula histórica da base. O recálculo genérico L-113 segue disponível nas competências entregues a partir da 11. A reemissão nessas competências está [nao verificado]. (`folha/export.py:385`, `ponto/calculador/regras.py:395`, `ponto/services/fechamento.py:989`, `:1305`, `:1829`, `ponto/management/commands/diff_calculador.py:152`, `59600163c:app/ponto/services/fechamento.py:958`) |
| **C** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_alimentacao_le_a_dobra_normal_da_primeira_fatia` e `::test_RED_c4_periodo_normal_respeita_calendario_injetado_vazio`. Na c3, o oráculo respondeu `0`, `4` e `4 h` nos casos que pediam `4`, `0` e `0 h`. A releitura também inventou 240 min com calendário vazio. (`ponto/tests/test_feriado_um_juiz.py:216`, `:232`) | A alimentação da lavra `oraculo`, o DIFF e a releitura por período leem os minutos decididos pelo motor que apurou o turno. A prova integrada de mensal, lavras e TXT está [nao verificado]. (`ponto/calculador/alimentacao.py:519`, `:584`, `ponto/calculador/periodo.py:768`, `ponto/management/commands/diff_calculador.py:411`, `ponto/tests/test_feriado_um_juiz_porta.py:298`) |
| **D** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_par_de_21h_a_23h_conta_a_uniao_do_noturno`. A c3 deu **1.560 min** de split. A c4 deu **1.500 min** e adicional legal de **1.500 ÷ 0,875 min**. (`ponto/tests/test_feriado_um_juiz.py:353`) | A promoção mede a união da janela com o medidor existente, desconta os intervalos e entrega split e adicional ao mensal, às duas lavras e ao DIFF. A prova integrada está [nao verificado]. (`ponto/calculador/regras.py:433`, `ponto/services/fechamento.py:998`, `ponto/services/dia_pago.py:244`, `ponto/calculador/alimentacao.py:353`, `ponto/tests/test_feriado_um_juiz_porta.py:572`) |
| **E** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_data_e_certeza_seguem_o_turno_de_cada_par` e `::test_RED_c4_turnos_distintos_na_borda_20_21_entram_cada_um_na_sua_competencia`. A c3 pôs ambas as parcelas na véspera. Na borda, deixou os dois pares na 11 e nenhum na 12. (`ponto/tests/test_feriado_um_juiz.py:367`, `:391`) | A partição resolve vínculo, certeza, cláusula e data da lavra pelo turno. Entre competências já na vigência, a entrada do motor usa o pareador existente para retirar da janela ampliada o turno fechado e distinto que pertence à competência vizinha. As duas lavras e o DIFF leem o resultado. Os casos de porta com troca de praça estão [nao verificado]. (`ponto/calculador/regras.py:412`, `ponto/services/fechamento.py:154`, `:226`, `ponto/services/dia_pago.py:244`, `ponto/calculador/alimentacao.py:361`, `ponto/tests/test_feriado_um_juiz_porta.py:455`) |
| **F** | `ponto/tests/test_feriado_um_juiz_porta.py::test_RED_c4_competencia_sem_feriado_conserva_arredondamento_da_base`. Executadas as atribuições dos dois hashes com 241 + 241 min, a base deu **8,04 h** e a c3 **8,03 h**. `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_delete_e_bulk_create_usam_a_mesma_transacao` mostrou a perda da linha anterior na c3. (`59600163c:app/ponto/services/fechamento.py:922`, `:1008`, `29488e3bb:app/ponto/services/fechamento.py:964`, `ponto/tests/test_feriado_um_juiz.py:13`) | Sem promoção, o mensal volta a **8,04 h**. Com promoção positiva, arredonda uma vez. O RED existente conserva as asserções e agora injeta uma promoção fracionária na fixture. As duas versões de `lavrar` executam `delete` e `bulk_create` na mesma transação. Rollback no banco [nao verificado]. (`ponto/services/fechamento.py:1008`, `ponto/tests/test_feriado_um_juiz_porta.py:396`, `:435`, `ponto/services/dia_pago.py:360`) |
| **G** | `ponto/tests/test_feriado_um_juiz_porta.py::test_RED_c4_l097_certeza_so_decide_rubrica_com_ou_sem_autorizacao` está com o `skip` literal. A chamada isolada ao motor deu **240/0 min** para incerta/certa sem autorização e **240/240 min** com autorização. (`ponto/tests/test_feriado_um_juiz_porta.py:499`, `ponto/motor_calculo_v2.py:1562`, `:1585`) | G fica pendente da decisão de lei. K1 conserva o `expectedFailure` e as asserções da c3. (`ponto/tests/test_feriado_um_juiz_porta.py:381`, `:499`) |

**Comandos a partir de `app/`**. `base_c3` carrega os fontes do hash em memória e exige falhas de asserção. Os cinco comandos com `--settings` dependem da pista com banco e ficam [nao verificado]. (`ponto/tests/feriado_um_juiz_mutantes.py:43`, `:169`, `:279`)

```bash
m() { PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes "$@"; }

m base_c3
m controle
m c4_cartao_rejulga_ciclo
m c4_contador_ignora_feriado
m c4_ignora_vigencia
m c4_alimentacao_rejulga_dobra
m c4_periodo_rejulga_dobra
m c4_split_bruto_do_noturno_longo
m c4_adicional_bruto_do_noturno_longo
m c4_lavra_na_data_herdada
m c4_certeza_no_dia_herdado
m c4_pad_conserva_turnos_externos
m c4_lavra_sem_atomic

m c4_fechamento_ignora_competencia --settings=config.settings.ci
m c4_mensal_le_noturno_bruto --settings=config.settings.ci
m c4_lavra_le_noturno_bruto --settings=config.settings.ci
m c4_troca_de_praca_le_vinculo_herdado --settings=config.settings.ci
m c4_arredonda_fora_da_promocao --settings=config.settings.ci
```

**Recorte para o Code** (`bin/suite.sh:295`)

```bash
bash ../bin/suite.sh --dir /home/ronald/wt-codex-feriado-um-juiz-152427 \
  --only "ponto folha relatorios escala core colaboradores api"
```

Medição dos 75 dias e 42 colaboradores, DIFF do TXT 11 de frota, reversão e apply L-092 [nao verificado].

## 27/11 16:04 · sessao ab2be411f222 · FERIADO-BOTAO-CORTADO · construtor · origem=Code

**B1 — Histórico HTMX.** O componente instala os listeners uma vez por documento e devolve o painel ao `details` antes do snapshot. O RED `core/tests/test_smoke_clique_feriado_vira_trabalho.py::test_historico_htmx_do_feriado_e_de_inverter_retratar` exercita Feriado e Inverter/Retratar, conta as inscrições após a segunda execução do script e verifica os snapshots e o DOM ao Voltar e Avançar. [popover-acao.js:28](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:28) [popover-acao.js:177](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:177) [teste:546](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:546)

**Prova antes da cura.** Li o blob de `c44771b4a` com `git show` e executei o JS duas vezes no Duktape, com DOM simulado. O base registrou **2 inscrições por evento e 1 órfão** no ponto do snapshot. A cura registrou **1 inscrição por evento e 0 órfãos**. Retirar a guarda voltou a duplicar as inscrições. Retirar o fechamento voltou a deixar o órfão. A reprodução em Chromium fica [nao verificado]. [popover-acao.js:160](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:160) [mutantes:77](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:77)

**B2 — Ficha real a 1366 px.** `core/tests/test_smoke_clique_feriado_vira_trabalho.py::test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas` agora abre a rota da ficha, exige `colaboradores/detalhe.html` e espera o swap HTMX. A sonda verifica a coluna de 420 px e o `overflow:hidden` antes de medir viewport, recorte, ancoragem e submit. O MORDE `core/tests/test_smoke_clique_feriado_vira_trabalho.py::test_MORDE_painel_recortado_na_ficha_real` injeta recorte e exige que a sonda o acuse mesmo com o painel dentro da viewport. A leitura do hash-base confirmou a lacuna do cenário sintético. A mordida e a geometria da ficha real ficam [nao verificado], portanto a cura geométrica condicional segue em aberto. [ficha:87](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:87) [ficha:215](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:215) [sonda:235](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235) [MORDE:549](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:549) [rota:676](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:676)

A tentativa do RED por `manage.py` parou no import de `django.core`, antes de iniciar a suíte. O clique na tela de produção e a certificação pelo Code e pelo opus ficam [nao verificado]. `360 px` e `base_app` ficam na lista do pouso conforme o aval fornecido. [manage.py:11](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/manage.py:11) [cenários:641](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:641) [nao verificado]

**Inclusões e diff.** `base.html` e `base_app.html` são as duas inclusões diretas. Os consumidores são Feriado, Inverter/Retratar e Resolver dia. O diff tem **0 `except` e 0 `catch` adicionados ou removidos**. [base.html:440](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/base.html:440) [base_app.html:194](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/base_app.html:194) [grade:96](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:96) [grade:133](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:133) [grade:138](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:138)

**Comandos dos mutantes para o Code**, a partir de `app/`, no ambiente de teste. Cada invocação abaixo deve falhar. [mutantes:72](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:72) [seleção:543](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:543) [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1
smoke=core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest
b1="$smoke.test_historico_htmx_do_feriado_e_de_inverter_retratar"
b2="$smoke.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas"

SMOKE_FERIADO_MUTANTE=historico-sem-guarda python3 manage.py test --settings=config.settings.ci "$b1"
SMOKE_FERIADO_MUTANTE=historico-sem-fechar-painel python3 manage.py test --settings=config.settings.ci "$b1"
SMOKE_FERIADO_MUTANTE=pagina-sem-ficha python3 manage.py test --settings=config.settings.ci "$b2"
SMOKE_FERIADO_MUTANTE=painel-recortado python3 manage.py test --settings=config.settings.ci "$b2"
```

**Recorte `--only` para o Code.** [suite.sh:26](/home/ronald/wt-codex-feriado-botao-cortado-112417/bin/suite.sh:26) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 ../bin/suite.sh \
  --dir /home/ronald/wt-codex-feriado-botao-cortado-112417 \
  --only "core.tests.test_smoke_clique_feriado_vira_trabalho core.tests.test_selo_popover_acao core.tests.test_smoke_retratar_1366"
```

## 27/11 16:04 · sessao 5e1b420eecd8 · PAR-PELO-TURNO · construtor · origem=Code

A c1v14 está na cópia. O recorte em memória passou **65 testes**, e os vizinhos de `core` e `chamados` passaram **63 testes**. A execução canônica dos 18 registros e a medição dos tetos de **254/318** ficam **[nao verificado]**. O `--only` da cópia usa rede e a rota local usa Docker, ambos fora do escopo autorizado. `app/ponto/tests/par_pelo_turno_c1v14_verificacao.py:53`, `bin/suite.sh:320`, `bin/suite.sh:551`

### Grupos 1–4

| Grupo | Cura e prova |
|---|---|
| 1. Fixture dos REDs | Os cenários de fechamento usam um `Colaborador` real, sem salvar. Os nove casos chegaram às asserções originais e passaram em memória. Comparei por AST as 42 asserções dos três arquivos. O mutante `fixture` reproduziu `KeyError: 7` com o dublê que exige a FK. `app/ponto/tests/test_par_c1v8_reds.py:20`, `app/ponto/tests/test_par_pelo_turno_c1v10.py:30`, `app/ponto/tests/test_par_pelo_turno_c1v11.py:63`, `app/ponto/tests/par_pelo_turno_c1v14_verificacao.py:154` |
| 2. Data civil | `_data_do_turno` usa `data_local(saida.timestamp)` e preserva a condição da segunda virada. O selo intacto passou e rejeitou o mutante `data`, que repõe a conversão da c1v13. Folha e espelho também leem a geometria do pareador. `app/ponto/turnos.py:289`, `app/ponto/tests/test_contract_no_batida_date.py:232`, `app/ponto/services/fechamento.py:636`, `app/ponto/services/espelho.py:1035` |
| 3. Custo da escrita | A carga reutiliza o DNA da `CelulaDia`. Os três pontos de economia identificados no código são a recomposição do núcleo, a grade do cartório e a carga de `dias_encerrados`. Os REDs `dna` e `dna-grade` pegam a releitura e a perda do repasse. A atribuição das **+3 queries históricas** e os totais pós-cura são **[nao verificado]**. Os tetos seguem intactos. `app/ponto/nucleo.py:40`, `app/ponto/services/cartorio.py:1182`, `app/ponto/services/cartorio.py:1205`, `app/escala/utils.py:1957`, `app/ponto/turnos.py:1958`, `app/ponto/tests/test_par_pelo_turno_c1v14.py:19`, `app/ponto/tests/test_selo_chokepoint_escrita.py:491` |
| 4. Sem vínculo | O guarda usa o vínculo do dia julgado, já resolvido na segmentação. O selo legado devolveu `('S', 'orfao_14h', 'SAIU')` com persistência em memória. O RED com vínculo só na entrada passou. `cadastro` e `dono` derrubaram os respectivos testes. O juiz de chamados também lê `classificar_falta`. `app/ponto/turnos.py:2055`, `app/escala/servico_jornada.py:214`, `app/escala/tests/test_selo_b5c_universo_da_jornada.py:108`, `app/ponto/tests/test_par_c1v8_reds.py:202`, `app/chamados/juizes.py:746` |

### Grupo 5. Selos estruturais

| Selo | Caminho exigido pelo AST | Mutante |
|---|---|---|
| `falta_um_significado` | As duas emissões chegam à única aplicação da folha. A chamada preserva `agora` e `ate`, que faltavam no literal antigo. `app/relatorios/tests/test_falta_um_significado.py:144`, `app/relatorios/pdf_espelho.py:255` | `folha` aplica a contagem no PDF. |
| `pdf_le_o_juiz_do_dia` | PDF → espelho → marcas da janela → projeção por PK do dia julgado. A volta do agrupamento local continua vedada. `app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:74`, `app/ponto/turnos.py:3049` | `dia` reintroduz `_dj` por data civil. |
| `f1c_contrato` | O PDF consome a grade retornada pelo espelho, que chama `grade_espelho_janela`. `app/escala/tests/test_f1c_contrato.py:42`, `app/ponto/services/espelho.py:1286` | `grade` monta uma grade própria. |
| `regua_do_periodo_vem_da_janela` | O PDF recebe `escala` e `tipo_escala` de `autoridade_do_periodo`, que consulta `vinculo_do_dia`. `app/ponto/tests/test_regua_do_periodo_vem_da_janela.py:274`, `app/ponto/services/espelho.py:1076` | `vinculo` escolhe a escala no PDF. |
| `turno_cruza_meia_noite` | O chamado continua consultando a regra do previsto. A completude lê `cross_meianoite` do par efetivo, inclusive na segunda virada. `app/escala/tests/test_turno_cruza_meia_noite.py:43`, `app/chamados/juizes.py:2324`, `app/ponto/turnos.py:332` | `noturno` compara horas no guarda e `chamado` repete `hf <= hi`. |

Cada selo passou na fonte correspondente e rejeitou seu mutante com `rc 1`, tanto antes da obra (`59600163c`) como na c1v14. Na catraca vizinha, atualizei exclusivamente as coordenadas de quatro registros. Três já divergiam na c1v13. O mutante `catraca` voltou a derrubar a igualdade de linhas. **`except` adicionados no diff e nos arquivos novos: 0.** `app/ponto/tests/par_pelo_turno_c1v14_verificacao.py:76`, `app/core/tests/test_medida_turno_do_par.py:292`, `app/core/tests/test_medida_turno_do_par.py:342`, `app/core/tests/test_medida_turno_do_par.py:554`

### Comandos

Em `app/`, recorte em memória e mutantes. O mutante `fixture` termina no erro de FK simulado. Os demais falham por asserção. `app/ponto/tests/par_pelo_turno_c1v14_verificacao.py:179`

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v14_verificacao "$@"
}
prova
for alvo in fixture data dna dna-grade cadastro dono catraca; do
  prova --alvo "$alvo" --mutante
done
for alvo in folha dia grade vinculo noturno chamado; do
  prova --alvo "$alvo" --antes
  prova --alvo "$alvo" --antes --mutante
  prova --alvo "$alvo"
  prova --alvo "$alvo" --mutante
done
```

Recorte canônico dos 18 registros e vizinhos para o Code **[nao verificado]**. `bin/suite.sh:26`

```bash
labels=(
  ponto.tests.test_par_c1v8_reds
  ponto.tests.test_par_pelo_turno_c1v10
  ponto.tests.test_par_pelo_turno_c1v11
  ponto.tests.test_par_pelo_turno_c1v12
  ponto.tests.test_par_pelo_turno_c1v13
  ponto.tests.test_par_pelo_turno_c1v14
  ponto.tests.test_par_pelo_turno_c1v6
  ponto.tests.test_contract_no_batida_date
  ponto.tests.test_selo_chokepoint_escrita
  ponto.tests.test_c1_dias_encerrados
  ponto.tests.test_regua_do_periodo_vem_da_janela
  escala.tests.test_selo_b5c_universo_da_jornada
  escala.tests.test_noturno_completo_intervalo
  escala.tests.test_sobra_na_borda_nao_vira_intervalo
  escala.tests.test_f1c_contrato
  escala.tests.test_turno_cruza_meia_noite
  relatorios.tests.test_falta_um_significado
  relatorios.tests.test_pdf_le_o_juiz_do_dia
  relatorios.tests.test_pdf_sem_regra_propria
  relatorios.tests.test_pdf_nao_calcula
  relatorios.tests.test_par_pelo_turno_c1v6_pdf
  chamados.tests.test_e3_c8b_sem_banco
  core.tests.test_medida_turno_do_par
)
PYTHONDONTWRITEBYTECODE=1 bash ../bin/suite.sh \
  --dir /home/ronald/wt-codex-par-pelo-turno-111717 \
  --only "${labels[*]}"
```

## 27/11 15:54 · sessao aff965f9ab2a · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**REPROVADO no recorte desktop de 1366 px.** Há dois bloqueios: o histórico HTMX pode restaurar um painel órfão no `body`, e o smoke de página mede o calendário numa página de ensaio sem o layout da ficha real. O drawer estabilizado tem prova geométrica no ensaio. O clique do Ronald na tela continua [nao verificado]. `app/static/js/popover-acao.js:115` `app/static/js/popover-acao.js:131` `app/core/tests/vendor/htmx-1.9.10.min.js:1` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:32` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:119` `app/templates/colaboradores/detalhe.html:87` `app/docs/LEIS.md:99`

A revisão foi só leitura, sobre a cópia e os anexos. O `Ran 9 OK` às 15:47 é o resultado informado. A seleção dos nove testes, suas medidas brutas, o clique no calendário real de 20/11 e a identidade Git da cópia, que veio sem `.git`, ficam [nao verificado]. [nao verificado]

### 1. Página, drawer e MORDEs

- **O diff implementa a cura nas duas superfícies.** A grade conserva os 132 px, põe o gatilho antes das marcações, dá rolagem ao conteúdo abaixo e permite quebrar o rótulo. A ficha e a aba Espelho recebem a mesma grade. O JS mede o gatilho e o painel, move o painel para o `body` e limita a posição à viewport e à altura visível do drawer. `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:95` `app/templates/ponto/partials/_feriado_vira_trabalho.html:5` `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/detalhe.html:215` `app/templates/core/_drawer_generico.html:134` `app/static/js/popover-acao.js:32` `app/static/js/popover-acao.js:45` `app/static/js/popover-acao.js:131`
- **A medida do Chromium é real nos cenários montados.** A fixture exige `trabalha=False`, oferta `True` e quatro batidas. A sonda faz hit-test no gatilho, chama `.click()` no alvo, espera o `toggle` e mede os retângulos. Cobra o painel dentro das quatro bordas com margem de 8 px, hit-test em cinco pontos, distância de até 16 px sem sobreposição de área e botão de submit atingível. No drawer, usa a aba Espelho real com respostas do Client Django e espera a transição terminar. Na página, injeta o fragmento direto no `main` e pula a grade `420px 1fr`, o contêiner com `overflow:hidden` e o carregamento HTMX da ficha. A geometria da ficha real a 1366 px fica [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:112` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:146` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:183` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:205` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:247` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:275` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:459` `app/templates/colaboradores/detalhe.html:87` `app/templates/colaboradores/detalhe.html:202` `app/templates/colaboradores/detalhe.html:215`
- **Os cinco MORDEs automáticos do feriado têm falha específica.** `viewport-do-host` executa a 1365 e exige a divergência de largura. `gatilho-ausente` remove o `summary`. `drawer-fechado` fecha o drawer antes da medida. `excecao-na-medicao` exige a mensagem da exceção injetada. `painel-sobre-gatilho` exige painel ainda dentro da viewport, distância zero e ancoragem reprovada. Cada caso exige a falha correspondente em `_confere_medidas`. O de viewport verifica a sensibilidade à largura errada. Os demais mutantes opt-in, inclusive recorte, distância, foco, fechamento e encaixe lateral, têm execução individual [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:57` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:248` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:419` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:429` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:441` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:476` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:555`
- **Os selos vizinhos têm alcances distintos.** A varredura acusa o exemplo estrutural ruim. O MORDE de geometria exige estouro real de “Resolver dia” com `left:0` e sem o JS. O de Retratar exige que haja painel medido com a rota `/retratar/` e que nenhum painel medido estoure. A regressão unitária de quatro batidas cobra ordem, conteúdo e estilos do HTML. `app/core/tests/test_selo_popover_acao.py:88` `app/core/tests/test_selo_popover_acao.py:185` `app/core/tests/test_smoke_retratar_1366.py:167` `app/colaboradores/tests/test_feriado_botao_cortado.py:71`

### 2. Consumidores, fechamento e regressão HTMX

- **Inclusões diretas do script:** `app/templates/base.html:440` e `app/templates/base_app.html:194`. Os consumidores efetivos são **Tornar dia de trabalho**, **Inverter tipo / Retratar** e **Resolver dia / Cobrar este dia**. A grade entra no calendário mensal da ficha e do Espelho, na fita do drawer de diagnóstico e em `relatorios/furos.html`. Furos passa só `dias` e `resumo` com `only`, então recebe o novo invólucro da grade, sem os popovers de ação. `app/templates/ponto/partials/_feriado_vira_trabalho.html:5` `app/templates/colaboradores/partials/_calendario_grade.html:133` `app/templates/colaboradores/partials/_calendario_grade.html:138` `app/templates/ponto/partials/_veredito_dia.html:17` `app/templates/ponto/partials/_veredito_dia.html:25` `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/relatorios/furos.html:42`
- **Entradas do drawer:** lista de colaboradores, Benefícios, lista e Raio-X do Retrato da Folha, e a porta direta “Resolver dia no calendário” do Retrato. O diagnóstico em fita também entra pelo Painel Situacional e por Pendências de Disputa. `app/templates/colaboradores/partials/_lista_tabela.html:16` `app/templates/colaboradores/beneficios_lista.html:86` `app/templates/folha/_retrato_lista.html:61` `app/templates/folha/_retrato_raiox.html:68` `app/folha/retrato_folha.py:1019` `app/templates/folha/_retrato_o_que_fazer.html:73` `app/templates/colaboradores/partials/_ps_linha.html:4` `app/templates/colaboradores/partials/pendencias_disputa.html:9`
- **Fechar e substituir fragmento.** `fecharDrawer()` emite `drawer:fechou`. O JS devolve o painel ao pai, restaura estilo e `tabindex`, desliga o observador e evita devolver foco ao gatilho do drawer que fecha. O smoke cobra isso. Num `outerHTML` do calendário ou `innerHTML` do drawer que remove o gatilho, o observador fecha o painel original. Os sete listeners são instalados por execução do script, fora de `abre()` e `posiciona()`. A troca por HTMX fica [nao verificado] em navegador. `app/templates/core/_drawer_generico.html:17` `app/templates/core/_drawer_generico.html:71` `app/templates/colaboradores/calendario_partial.html:42` `app/static/js/popover-acao.js:101` `app/static/js/popover-acao.js:140` `app/static/js/popover-acao.js:156` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:342`
- **Bloqueio — Voltar/Avançar no histórico HTMX.** A paginação da lista usa `hx-push-url` e a mesma lista abre o drawer do colaborador. Com um popover aberto, o HTMX guarda o HTML do `body`, incluindo o painel `fixed` separado do `details`. Ao restaurar esse snapshot, o observador só conhece o nó original. A cópia no `body` fica sem dono e fora do alcance de `fecha()`. A restauração também recria os scripts do `body`. Cada nova execução do componente instala outro conjunto de listeners globais, pois a inicialização carece de guarda. É uma regressão possível também em **Resolver dia** e **Inverter/Retratar**. O caminho é demonstrável pelo código. A reprodução em navegador fica [nao verificado]. `app/templates/colaboradores/partials/_lista_tabela.html:16` `app/templates/colaboradores/partials/_lista_tabela.html:54` `app/templates/base.html:19` `app/templates/base.html:440` `app/core/tests/vendor/htmx-1.9.10.min.js:1` (`Zt`, `Qt`, `er`, `ar`, `Ue`, `Nt`) `app/static/js/popover-acao.js:101` `app/static/js/popover-acao.js:108` `app/static/js/popover-acao.js:131` `app/static/js/popover-acao.js:140` `app/static/js/popover-acao.js:156`

### 3. Porta de escrita

O formulário mantém o POST nativo para `ponto:feriado_vira_trabalho`, com token CSRF e middleware. A view exige login, POST e `tem_acao(..., 'editar_escalas')`, valida os campos e relê a mesma oferta do calendário. A escrita passa por `editar_dia(..., so_se_folga=True)`, que confere `trabalha is False` sob trava e grava célula e trilha no mesmo `atomic`. O template só usa os dois predicados recebidos para renderizar e formatar a célula. O smoke mede o submit e confere método e rota. A submissão pelo Chromium fica [nao verificado]. `app/templates/ponto/partials/_feriado_vira_trabalho.html:7` `app/config/settings/base.py:58` `app/ponto/urls.py:59` `app/ponto/views.py:3584` `app/ponto/views.py:3607` `app/ponto/views.py:3622` `app/ponto/portas/celula.py:642` `app/ponto/portas/celula.py:665` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:275`

### 4. Lista do pouso — `base_app` e 360 px [nao verificado]

- **Ficha a 360 px:** a primeira coluna fixa de 420 px já excede a largura da tela. O calendário começa na segunda coluna, fora do enquadramento inicial. `app/templates/colaboradores/detalhe.html:87` `app/templates/colaboradores/detalhe.html:201`
- **Drawer e `base_app` a 360 px:** a grade mensal conserva sete colunas e células de 132 px. Recorte efetivo, ancoragem e acionabilidade a 360 px ficam [nao verificado]. `base_app` tem cenários sintéticos a 1366, com o bootstrap PWA desativado. A ficha real estende `base.html`. Ficam na lista do pouso conforme o aval anexado, sem pedido de nova volta por esses itens. `app/templates/colaboradores/partials/_calendario_grade.html:64` `app/templates/colaboradores/detalhe.html:1` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:516` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:537`

### 5. Quem escreve e quem lê

| Campo ou valor | Escritor → leitores |
|---|---|
| `feriado_vira_trabalho_oferecido` | `contexto_calendario` compõe feriado aplicável + célula existente com `trabalha=False`. A grade, a view POST e o smoke leem o mesmo campo. `app/colaboradores/services/calendario.py:343` `app/colaboradores/services/calendario.py:791` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/ponto/views.py:3624` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:467` |
| Feriado e `CelulaDia.trabalha` | Configuração/importação gravam `Feriado` e `feriados_do_colab` fornece as datas ao calendário. Geração, regeneração, reversão e edição escrevem o estado da célula pela porta. O calendário lê a célula carregada e a edição relê sob trava. `app/core/views_config.py:60` `app/core/views_config.py:144` `app/core/feriados.py:135` `app/ponto/portas/celula.py:293` `app/ponto/portas/celula.py:642` `app/ponto/portas/celula.py:866` `app/ponto/portas/celula.py:952` `app/colaboradores/services/calendario.py:791` |
| `pode_editar_dia`, `pode_veredito`, `volta_url` | View do calendário e serviço do drawer de diagnóstico montam o contexto a partir de `tem_acao` e da rota da ficha. A grade consome as flags e repassa a URL aos formulários. A view POST consulta a permissão novamente. `app/colaboradores/mixins.py:322` `app/colaboradores/views.py:1992` `app/colaboradores/views.py:2007` `app/colaboradores/services/drawer.py:131` `app/colaboradores/services/drawer.py:156` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/templates/colaboradores/partials/_calendario_grade.html:133` `app/ponto/views.py:3607` |
| `layout`, `dias`, `data`, `cal_offset`, `status`, `veredito_dia`, `palavra_dia`, marcações, badges e oferta de Resolver dia | Os chamadores passam `mes`/`fita`. O serviço compõe as projeções e a grade as lê. A porta de batida grava `pk`/`tipo`/`timestamp` na criação e o flip altera `tipo` via `anotar`. A grade e o smoke leem esses valores. `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/colaboradores/services/calendario.py:639` `app/colaboradores/services/calendario.py:754` `app/colaboradores/services/calendario.py:809` `app/colaboradores/services/calendario.py:838` `app/ponto/registro_batida.py:170` `app/ponto/services/flip_batida.py:41` `app/templates/colaboradores/partials/_calendario_grade.html:65` `app/templates/colaboradores/partials/_calendario_grade.html:133` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:469` |
| POST `colab`, `dia`, `volta`, `motivo`, CSRF | O partial escreve os três valores ocultos e renderiza o token. O administrador preenche `motivo`. Middleware lê CSRF, a view lê e valida os campos, `resposta_acao` recebe a volta saneada e a porta recebe o motivo. `app/templates/ponto/partials/_feriado_vira_trabalho.html:8` `app/config/settings/base.py:58` `app/ponto/views.py:3584` `app/ponto/views.py:3598` `app/ponto/views.py:3614` `app/ponto/views.py:3632` |
| Escrita indireta do POST | `editar_dia` grava `trabalha`, `dna`, `escala_geradora`, `origem`, `editada_em` e `editada_por`. A porta lê o antes/depois e registra autor, ato e motivo via `evento_kw`. Calendário lê `trabalha`, o juiz do vínculo lê a geradora, e cartório, fase e veredito leem `editada_em`. `app/ponto/portas/celula.py:569` `app/ponto/portas/celula.py:641` `app/ponto/portas/celula.py:658` `app/core/observ.py:134` `app/colaboradores/services/calendario.py:793` `app/escala/alimentacao.py:228` `app/ponto/services/cartorio.py:411` `app/escala/services/fase_12x36.py:190` `app/chamados/veredito_lavrado.py:194` |
| Estado do painel, evento e medidas | O navegador fornece retângulos, área, foco e estilos computados. `abre` guarda `atual`, pai/irmão, estilo e `tabindex`. `posiciona` escreve o CSS temporário e `fecha` restaura. O drawer escreve `display`/`transform` e emite o evento que o JS lê. A sonda gera as medidas, o driver CDP as transporta e `_confere_medidas` as valida. `app/static/js/popover-acao.js:32` `app/static/js/popover-acao.js:106` `app/static/js/popover-acao.js:140` `app/templates/core/_drawer_generico.html:71` `app/static/js/popover-acao.js:168` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:98` `app/core/tests/smoke_feriado_chromium.js:44` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:429` |

### 6. Classes de reprovação

**Segundo juiz, segundo escritor, relógio fora da borda, derivação paralela de negócio e campo com dois significados:** zero introduzidos no diff fornecido. Oferta e permissão continuam com os donos acima, o `agora` do POST sai uma vez da view e segue para calendário e edição, e a mudança de produção escreve estado de apresentação. **Selo sem mordida:** os cinco controles têm falhas específicas. A lacuna é de cobertura da ficha real e do histórico HTMX. A trava de `so_se_folga` permanece na porta, e o smoke humano pré-pouso está [nao verificado]. `app/colaboradores/services/calendario.py:791` `app/ponto/views.py:3607` `app/ponto/views.py:3622` `app/ponto/portas/celula.py:642` `app/static/js/popover-acao.js:115` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:32` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:441` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:555` `app/docs/LEIS.md:99`

### 7. `except` no diff

**0 cláusulas Python `except` adicionadas, 0 removidas.** Também há **0 `catch` JavaScript adicionados ou removidos**. A rejeição da sonda e as falhas do driver viram `motivo`, que a asserção exige ser nulo. O `catch` de `htmx.process` no drawer já existia. `app/colaboradores/tests/test_feriado_botao_cortado.py:1` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:356` `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:429` `app/core/tests/test_selo_popover_acao.py:124` `app/core/tests/test_smoke_retratar_1366.py:54` `app/core/tests/smoke_feriado_chromium.js:68` `app/templates/core/_drawer_generico.html:112`

## 27/11 15:44 · sessao 4c95a5211276 · PAR-PELO-TURNO · leitura · origem=Code

Comparei a cópia `84f8a88d4` com a cópia local da base `59600163c`, arquivo a arquivo. As linhas abaixo são do HEAD, salvo quando marco `base`. Os 18 registros do anexo correspondem a **17 métodos**, porque `test_3_o_fato_posterior_mantem_as_marcas_fora_da_competencia` falhou em dois subtestes. A execução da suíte na base é [nao verificado]. `app/ponto/tests/test_par_c1v8_reds.py:175–186`

### 9 ERROR — fixture compartilhada da obra

Os três arquivos de teste foram adicionados pela obra e usam `_ler_fechamento`. A fixture fornece um `SimpleNamespace(pk=7, id=7)` e força a busca do fechamento gravado a retornar `None`. O serviço então tenta construir `FechamentoMensal(colaborador=colab, ...)`, cuja FK exige uma instância de `Colaborador`. Essa incompatibilidade está na fixture nova. A construção já existia na base. `app/ponto/tests/test_par_c1v8_reds.py:24,67–100` · `app/ponto/models.py:398–407` · `app/ponto/services/fechamento.py:885–890` · `base:app/ponto/services/fechamento.py:910–915` · `app/ponto/tests/test_par_pelo_turno_c1v10.py:10` · `app/ponto/tests/test_par_pelo_turno_c1v11.py:11`

O `KeyError: 7` do anexo sai do `[colab.pk]` aplicado ao dicionário retornado. O serviço captura exceções por colaborador e pode devolver o dicionário sem preencher essa chave. O anexo mostra essa consequência em todos os nove registros. A primeira exceção capturada na pista é [nao verificado]. **Classificação dos nove: erro de fixture antes das asserções listadas.** A cura demonstrável é alimentar a FK com um `Colaborador` compatível, preservando os REDs. `app/ponto/tests/test_par_c1v8_reds.py:95–100` · `app/ponto/services/fechamento.py:1133–1135,1302–1309`

| Teste | Ponto do erro e expectativa que ficou sem julgamento | [nao verificado]
|---|---|
| `test_3_o_fato_posterior_mantem_as_marcas_fora_da_competencia` — 23/10 | Chamada na linha 183. A competência deve ter `0,0 h` e saldo `0,0` com e sem o par posterior. `app/ponto/tests/test_par_c1v8_reds.py:175–186` |
| `test_3_o_fato_posterior_mantem_as_marcas_fora_da_competencia` — 25/10 | Mesma chamada e mesmos valores, com o fato posterior além do contexto do mapa. `app/ponto/tests/test_par_c1v8_reds.py:173–186` |
| `test_3b_o_ultimo_dia_com_marca_e_movimento_da_competencia` | Chamada na linha 196. Espera **1 turno aberto** e saldo **menor que zero**. `app/ponto/tests/test_par_c1v8_reds.py:194–198` |
| `test_5_a_folha_preserva_as_oito_horas_em_21_de_outubro` | Chamada na linha 249. Espera **8,0 h** em 21/10. `app/ponto/tests/test_par_c1v8_reds.py:241–250` |
| `test_a_folha_fecha_o_par_na_segunda_virada_sem_levar_as_marcas_vizinhas` | Chamada na linha 41. Espera **26,0 h**, **0 abertos**, só os PKs `[1, 2]` e ambos atribuídos a `DIA`. `app/ponto/tests/test_par_pelo_turno_c1v10.py:30–46` |
| `test_retratacao_apos_a_leitura_conserva_o_mesmo_conjunto_na_folha` | Chamada na linha 75. Espera uma leitura `[[1, 2]]`, **2,0 h** e **0 abertos**. `app/ponto/tests/test_par_pelo_turno_c1v10.py:48–78` |
| `test_a_segunda_virada_chega_separadamente_a_cada_leitor` | Chamada na linha 177. Espera `[1, 2]` na fatia da folha e também na leitura independente do espelho. `app/ponto/tests/test_par_pelo_turno_c1v11.py:166–182` |
| `test_fatia_recebe_os_dois_pks_de_um_par_julgado_em_d` | A asserção da carga inicial vem antes da chamada que falha na linha 91. No fechamento, espera `[1, 2]` no motor, **26,0 h** e **0 abertos**. `app/ponto/tests/test_par_pelo_turno_c1v11.py:79–94` |
| `test_universo_da_porta_na_folha_e_no_espelho` | Chamada na linha 72. Com a porta vazia, espera **0,0 h**, nenhuma marca no espelho e duas chamadas à porta com os mesmos limites. `app/ponto/tests/test_par_pelo_turno_c1v11.py:63–77` |

### 2 FAIL — teto de consultas no fechamento de chamados pela batida

| Teste | Falha observada | [nao verificado]
|---|---|
| `test_registrar_batida_com_1_furo_vivo_que_ela_fecha` | O chamado fechou. **257 consultas**, teto **254**. `app/ponto/tests/test_selo_chokepoint_escrita.py:218,343–354,491–494` |
| `test_registrar_batida_com_3_furos_vivos_que_ela_fecha` | Os três chamados fecharam. **321 consultas**, teto **318**. `app/ponto/tests/test_selo_chokepoint_escrita.py:254,343–354,496–499` |

**Testes e tetos idênticos na base. A obra alterou o caminho medido.** O `post_save` da batida recompõe turnos via `turnos_do_colab`. O cartório também chega a essa função por `dias_encerrados`. No HEAD, a carga lê as batidas por limites de instante e usa a janela de contexto alterada. O anexo comprova `+3` nos dois cenários. A origem de cada consulta extra e o total da base sob o mesmo ambiente são [nao verificado]. **Os selos estão certos. A correção é no custo do caminho de escrita**, com os tetos atuais como referência. `app/ponto/tests/test_selo_chokepoint_escrita.py:480–499` · `app/ponto/signals.py:19–36,85–93,137–140` · `app/ponto/nucleo.py:40–47` · `app/ponto/services/cartorio.py:1182–1186` · `app/ponto/turnos.py:1885–1909,2213–2215` · `base:app/ponto/turnos.py:1850–1876` · `PLANO_PISCADA.md:8–15`

### 4 FAIL — selos estruturais do PDF

Os quatro testes já existiam na base e estão idênticos no HEAD. A obra mudou o coletor do PDF para chamar `espelho_do_colab(..., janela_exata=True)`. As falhas abaixo são de expectativas sobre a forma do código. Cada contrato continua rastreável no caminho delegado. `base:app/relatorios/pdf_espelho.py:187–194,307,543,628` · `app/relatorios/pdf_espelho.py:178–182,221–245,255`

| Teste | Causa no HEAD e contrato | [nao verificado]
|---|---|
| `test_MORDE_ha_UM_sitio_que_aplica_a_folha_e_os_dois_caminhos_passam_por_ele` | O `assertIn` procura a chamada literal com quatro argumentos. A chamada única agora passa também `agora=agora, ate=agora`. Os dois modos de emissão chegam ao mesmo coletor, e `folha_manda` continua com **um** ponto de aplicação dos totais. A expectativa textual ficou desatualizada. `app/relatorios/tests/test_falta_um_significado.py:157–178` · `app/relatorios/pdf_espelho.py:255,277,663,710` · `app/relatorios/cartao_pela_celula.py:346` |
| `test_MORDE_o_pdf_consome_o_juiz_e_nao_regra_propria` | O `assertIn('dia_das_batidas', fonte)` perdeu as menções que já eram comentários na base. No HEAD, o PDF recebe as linhas do espelho. O espelho agrupa as marcas pelo mapa de PKs de `marcas_da_janela_para_exibicao`, produzido pelo pareador. A regra continua sendo **imprimir no dia julgado para o PK**. `app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:84–96` · `base:app/relatorios/pdf_espelho.py:484,535,613` · `app/relatorios/pdf_espelho.py:221–245` · `app/ponto/services/espelho.py:586–591,1243–1245` · `app/ponto/turnos.py:3044–3061` |
| `test_renders_usam_o_contrato` | O contador da string `grade_espelho_janela` no PDF caiu de **1 para 0**, ante mínimo **1**. A chamada está no ramo `janela_exata` do espelho e o PDF consome a grade retornada. A exigência de usar a grade compartilhada continua satisfeita pelo caminho delegado. `app/escala/tests/test_f1c_contrato.py:42–44,68–70` · `base:app/relatorios/pdf_espelho.py:193,543` · `app/ponto/services/espelho.py:1283–1287,1322–1325` · `app/relatorios/pdf_espelho.py:221–222,240–244` |
| `test_RED_os_oito_pedem_o_vinculo_ao_juiz_da_casa` | A busca AST aceita duas chamadas diretas no corpo do coletor. A base tinha `vinculo_do_dia_carregado`. No HEAD, `_espelho` chama `autoridade_do_periodo`, que chama `vinculo_do_dia`, e o coletor usa `autoridade.esc` e `autoridade.tipo_escala`. O PDF aparece em `calados` pelo alcance da busca. O contrato do RED continua sendo **todos os leitores consultarem a autoridade do vínculo**. `app/ponto/tests/test_regua_do_periodo_vem_da_janela.py:197–221,260–272,300–309` · `base:app/relatorios/pdf_espelho.py:307` · `app/ponto/services/espelho.py:1076–1077,1242` · `app/relatorios/pdf_espelho.py:221–224,261–262` |

### 2 FAIL — par noturno completo e cadastro

- **`test_SEM_escala_o_veredito_NAO_muda`** — Teste idêntico na base. A fixture cria **E 18:52 e S 06:52 do dia seguinte**, sem criar vínculo. Na base, o guarda retornava `False` sem escala e a classificação seguia para o fallback que espera `('S', 'orfao_14h', 'SAIU')`. A obra antecipou o guarda e passou a silenciar pelo par fechado que cruza o dia. A pista recebeu `(None, None, None)`. **Mudança de domínio causada pela obra. Pelo contrato disponível, a cura é no código para o caso inteiramente sem vínculo.** O RED da obra que aceita ausência de vínculo **na saída** cria vínculo para a entrada. Revogação do contrato legado para o caso inteiramente sem escala é [nao verificado]. `app/escala/tests/test_selo_b5c_universo_da_jornada.py:108–117` · `base:app/escala/servico_jornada.py:232–239,361–366,490–492` · `app/escala/servico_jornada.py:214–220,318–322` · `app/ponto/tests/test_par_c1v8_reds.py:200–221`
- **`test_o_juiz_do_chamado_e_o_noturno_completo_perguntam_a_regra`** — Teste idêntico na base. Falha a segunda busca literal por `turno_cruza_meia_noite(hi, hf)`. A primeira, no juiz do chamado, passou. A obra substituiu a checagem do horário previsto em `_turno_noturno_completo` pela leitura de `Turno.cross_meianoite` e pelo intervalo inclusivo `data_turno <= data_evento <= data_saida`. **A segunda expectativa estrutural ficou desatualizada.** O RED da obra exige `True` para o par fechado na segunda virada com vínculo noturno, diurno ou ausente na saída. `app/escala/tests/test_turno_cruza_meia_noite.py:43–48` · `app/chamados/juizes.py:2316–2324` · `base:app/escala/servico_jornada.py:234–239` · `app/escala/servico_jornada.py:214–220` · `app/ponto/turnos.py:318–334` · `app/ponto/tests/test_par_c1v8_reds.py:200–221`

### 1 FAIL — conversão de data dentro do pareador

- **`test_nenhum_ofensor_novo`** — Teste idêntico na base. A obra adicionou `fim = timezone.localtime(saida.timestamp)` seguido de `fim.date() > d` em `_data_do_turno`. O visitante AST rastreia variáveis atribuídas a partir de `.timestamp` e marca o `.date()`. Aplicado aos dois arquivos, o detector aponta essa linha apenas no HEAD. **O selo está certo para a forma que protege. A cura é no código**, usando `data_local` para a data civil da saída e preservando a condição da segunda virada. O valor esperado de `novos` continua `set()`. `app/ponto/tests/test_contract_no_batida_date.py:184–216,232–235` · `base:app/ponto/turnos.py:248–322` · `app/ponto/turnos.py:44–47,287–301`

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

_8727 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
