# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 04:30** · publicado em **01/11/2026 04:30** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 04:30 · sessao 8f8c0ea15497 · - · - · origem=Ronald (tmux)

**NADA — defeito procedente nos testes revisados.** O teste novo usa três objetos distintos, duas listas e o mesmo dia explícito; exige exatamente os três intervalos e cinco segmentos, portanto não é vacuidade nem deixa sobreviver a coleta apenas do primeiro período. HE zerada não impede a coleta, que fica fora da condição de HE positiva. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:153`, `:164`, `:168`, `:170`; **A:** `app/ponto/calculador/alimentacao.py:105`; `app/ponto/calculador/nucleo.py:393`, `:400`.

**NADA — relógio capaz de mudar esses resultados localizado.** Há, sim, leitura indireta preexistente de `date.today()`: `rubricas_por_dia` chama `intervalo_cadastrado`, que consulta `pausas_do_dia(hoje)`. Entretanto, N6/N12 possuem uma pausa fixa, sem override semanal; o retorno relevante termina nos campos fixos `hii/hfi`. Referências: `app/ponto/calculador/alimentacao.py:395`, `:485`; `app/ponto/juiz_batida.py:125`, `:128`, `:135`; `app/escala/models.py:304`; **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:44`; `app/ponto/tests/test_o280_bateria_da_lei.py:1086`.

**Limite do teste unitário:** p3, com envelope de três horas e desconto de quinze minutos, não reproduz o desconto dos ramos atuais 12x36/comercial, cujo portão exige duração maior que seis horas; isso não invalida sua asserção sobre transporte dos campos recebidos. Não usá-lo como prova de que o motor produz essa fixture completa. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:164`; `app/ponto/motor_calculo_v2.py:2755`, `:2763`, `:3216`.

Os casos integrados exigem período único, intervalo presente e resultados numéricos não nulos; N12 exige também par único não cortado. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:72`, `:75`, `:125`, `:127`, `:130`.

Execução Django dos anexos nesta revisão: **[nao verificado]**.

## 01/11 04:30 · sessao b60f6e19db67 · - · - · origem=Ronald (tmux)

**NADA novo procedente no delta executável**, além das questões já triadas e da geometria de envelopes sob investigação. A coleta acrescentada alimenta auxiliares que recortam os intervalos ao envelope; nas noturnas, também unem sobreposições antes de medir. Referências: **A:** `app/ponto/calculador/alimentacao.py:107`, `:527`; `app/ponto/motor_calculo_v2.py:164`, `:169`, `:397`, `:399`.

A troca de `return` por `elif` preserva a prioridade da dobra sobre o excedente e libera os blocos existentes de split, noturnas e pontualidade. Não identifiquei efeito colateral novo nessa sequência. Referências: `app/ponto/calculador/regras.py:779`, `:795`, `:820`, `:840`, `:851`.

Execução integral da árvore proposta: **[nao verificado]**.

## 01/11 04:27 · sessao ea3df59c7a57 · V3-DOBRA-SEM-NOTURNA · verificador · origem=Code

**PROCEDENTE — falta cobertura para intervalos de vários períodos:** o mutante “usar só o primeiro período” sobrevive aos cenários novos, que usam um único período com pausa. **DUVIDA — a agregação por dia não prova equivalência com os intervalos próprios de cada período quando envelopes se sobrepõem.** Referências: anexo `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:72`, `:125`; anexo `app/ponto/calculador/alimentacao.py:107`; `app/ponto/calculador/regras.py:823`.

Nas referências abaixo, **A:** indica linhas do arquivo resultante apresentado no anexo; sem esse prefixo, são linhas atuais da cópia. A cópia ainda passa `intervalos=()`; a cura anterior da dobra já está nela. Referências: `app/ponto/calculador/alimentacao.py:514`; `app/ponto/calculador/regras.py:789`.

**1. Escritores e leitores**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `PeriodoCalculo.intervalos_do_motor` | Default `()` no dataclass; atribuição de produção em `_carimbar_segmento`, que copia os pares para tupla. `app/ponto/motor_calculo_v2.py:247`, `:1830`. | Dentro do motor: `_aplicar_he_noturna`. Fora: corte dos segmentos, recálculo do período e noturno por par; A acrescenta o transporte por dia. `app/ponto/motor_calculo_v2.py:1006`; `app/ponto/calculador/alimentacao.py:121`; `app/ponto/calculador/periodo.py:741`, `:751`; **A:** `app/ponto/calculador/alimentacao.py:107`. |
| `insumos['intervalos_por_dia']` | `segmentos_do_motor` inicializa o mapa e acumula os intervalos dos períodos fechados pela chave `_dia`. **A:** `app/ponto/calculador/alimentacao.py:65`, `:107`. | `insumos_do_calculador` repassa o dict; `rubricas_por_dia` exige a chave e entrega uma tupla ao julgamento daquele dia. `app/ponto/calculador/alimentacao.py:302`, `:305`; **A:** `app/ponto/calculador/alimentacao.py:457`, `:527`. |
| Parâmetro `do_dia(intervalos=...)` | Default `()` na assinatura; A passa o conteúdo do mapa. `app/ponto/calculador/regras.py:574`; **A:** `app/ponto/calculador/alimentacao.py:527`. | Exatamente três pontos: noturnas da folga, split da HE e noturnas do ramo normal. `app/ponto/calculador/regras.py:714`, `:829`, `:842`. |

Os três motores recortam os intervalos à janela efetiva antes do carimbo: 12x36, partido e comercial. Referências: `app/ponto/motor_calculo_v2.py:2752`, `:2879`, `:3212`.

`calcular_noturnas` **não lê diretamente o atributo**: recebe o argumento local também usado no carimbo, por `_escrever_noturnas`. Referências: `app/ponto/motor_calculo_v2.py:991`, `:2780`, `:2884`, `:3235`.

Há escritores de fixture em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:51`, `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:48` e `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:44`. Asserções existentes leem o campo em `app/ponto/tests/test_noturno_desconta_intervalo.py:150`, `app/ponto/tests/test_o272_intra_fora_da_janela.py:91`, `:102`, `:118` e `app/ponto/tests/test_s5b_porta_unica.py:145`; o teste novo acrescenta leituras em **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:73`, `:126`.

**2. Efeito em cada leitor; desconto duplo e formato**

| Leitor | Par cortado na pausa | Par não cortado |
|---|---|---|
| Noturnas da folga | Fica igual quando os segmentos excluem toda a pausa: a interseção é vazia. `app/ponto/calculador/regras.py:714`; `app/ponto/motor_calculo_v2.py:164`, `:167`. | Pode diminuir pela parcela noturna da pausa. `app/ponto/calculador/regras.py:85`, `:86`. |
| Noturnas do ramo normal, incluindo dobra | Mesmo comportamento da folga; a dobra segue até esse bloco após a cura anterior. `app/ponto/calculador/regras.py:795`, `:842`. | Pode diminuir pela mesma subtração. `app/ponto/calculador/regras.py:85`, `:86`. |
| Split HE50/HE100 | Pode mudar: `he_pontas` mantém o envelope, independentemente dos cortes em `pares`. `app/ponto/calculador/alimentacao.py:96`; `app/ponto/calculador/regras.py:823`. | Também pode mudar: intervalos alteram o recuo da cauda extra e o desconto dentro dela. `app/ponto/motor_calculo_v2.py:440`, `:442`. |

**PROCEDENTE — o split pode diminuir, permanecer igual ou aumentar.** Em execução isolada dos corpos das funções, com datas sintéticas em UTC−03:

- Envelope 21–07, HE100=540, pausa02–03: **540 → 480 minutos** de HE100 noturna. Referências: `app/ponto/motor_calculo_v2.py:440`, `:442`.
- Envelope 22–07, HE100=120, pausa05:30–06:30: **0 → 120 minutos**. O recuo passa das05 para04 e passa a satisfazer a condição de prorrogação. Referências: `app/ponto/motor_calculo_v2.py:395`, `:405`, `:110`.

**Não identifiquei desconto duplo no universo de segmentos que excluem a pausa.** As noturnas partem da duração noturna de cada segmento, não de `minutos_trabalhados`; o intervalo recortado ao segmento mede zero. Referências: `app/ponto/calculador/regras.py:85`, `:86`; `app/ponto/motor_calculo_v2.py:164`, `:167`.

No split, os minutos de HE determinam **quanto trabalho** recuar; depois se desconta a pausa do envelope encontrado. Essas duas operações não retiram duas vezes a pausa da mesma grandeza. Referências: `app/ponto/motor_calculo_v2.py:392`, `:400`, `:403`, `:440`, `:442`.

Pontualidade **não lê esse parâmetro**: recebe `pontualidade` e `intervalo_marcos`. Trabalhadas, intra, dobra e HE total usam `trab`, `pausa_min` e os demais argumentos próprios; portanto A não muda essas contas por meio de `intervalos`. Referências: `app/ponto/calculador/regras.py:725`, `:740`, `:760`, `:776`, `:852`.

Nenhum dos três leitores exige intervalos recortados ao **dia civil**. O formato consumido é uma sequência de pares de datetimes; os auxiliares localizam as pontas, ordenam e intersectam com o recorte da chamada. O desconto noturno ainda une intervalos sobrepostos antes de medir. Referências: `app/ponto/motor_calculo_v2.py:162`, `:164`, `:169`, `:395`, `:399`.

**3. É o mesmo insumo usado pelo motor no mesmo cálculo?**

Para um período fechado, A transporta os mesmos pares de datetimes carimbados pelo motor; ao agrupar vários períodos, passa uma lista agregada do dia, não o vínculo individual período→intervalos. Referências: `app/ponto/motor_calculo_v2.py:1830`; **A:** `app/ponto/calculador/alimentacao.py:107`, `:527`.

Há quatro distinções relevantes:

- **PROCEDENTE — pontas diferentes no split, preexistentes a A.** O motor usa `periodo.entrada/saida`; a porta publica `efetivo_entrada/efetivo_saida`. Logo, mesmo conteúdo de intervalos não garante o mesmo resultado. Referências: `app/ponto/motor_calculo_v2.py:1003`; `app/ponto/calculador/alimentacao.py:73`, `:96`. Contraprova sintética: HE100=390, pausa02–03, pontas cruas21–07 produzem390 minutos; pontas efetivas21–04:30 produzem330, pela mesma função. Referências: `app/ponto/motor_calculo_v2.py:440`, `:442`. Ocorrência desse exemplo em produção: **[nao verificado]**.

- **DUVIDA — intervalos alheios dentro do envelope.** Se períodos agrupados no mesmo dia tiverem envelopes disjuntos, o recorte elimina as pausas alheias. Se houver sobreposição, o auxiliar não distingue a origem da pausa. Referências: **A:** `app/ponto/calculador/alimentacao.py:107`; `app/ponto/calculador/regras.py:829`; `app/ponto/motor_calculo_v2.py:397`, `:399`. Contraprova sintética: envelope21–07, HE100=540, pausa própria02–03 dá480; acrescentar pausa alheia04–05 dá420 minutos. Referências: `app/ponto/motor_calculo_v2.py:395`, `:440`, `:442`. Existência dessa geometria no dado fornecido: **[nao verificado]**.

- **`periodos_ft` entram no transporte.** A porta percorre `periodos_do_resultado`, que reúne `periodos` e `periodos_ft`; a chave vem de `dia_da_jornada[id(p)]`, sem redatar pela data civil da pausa. Período sem mapa é contado e descartado. Referências: `app/ponto/calculador/nucleo.py:393`, `:400`; `app/ponto/calculador/alimentacao.py:63`, `:66`, `:68`, `:69`. Assim, períodos com entradas civis diferentes e o mesmo dia de jornada alimentam a mesma lista. Referências: `app/ponto/calculador/alimentacao.py:68`; **A:** `app/ponto/calculador/alimentacao.py:107`.

- **Turno aberto não publica esses intervalos.** Os motores retornam antes do carimbo/noturnas; a porta publica o aberto e continua antes da coleta nova. Referências: `app/ponto/motor_calculo_v2.py:2740`, `:2867`, `:3192`; `app/ponto/calculador/alimentacao.py:74`, `:82`. O crédito de pares fechados dentro de aberto copia trabalhado/fora da janela, sem copiar intervalos ou noturnas. Referências: `app/ponto/motor_calculo_v2.py:2541`, `:2547`.

O adicional normal do motor usa pontas efetivas, mas conta o **par inteiro**; a porta pode contar segmentos. Essa diferença permanece: pausa04:30–05:30 resulta em480 minutos por par e390 por segmentos, pois o segmento iniciado05:30 perde a prorrogação. É limitação anterior, não desconto duplo introduzido por A. Referências: `app/ponto/motor_calculo_v2.py:110`; `app/ponto/tests/test_o280_bateria_da_lei.py:4162`, `:4168`.

**4. Os testes mordem?** [nao verificado]

**Contra `intervalos=()`: sim, pelas asserções e pelo cálculo isolado.** O RED exige8h de HE100 noturna, enquanto a chamada sem intervalo produz9h; o gêmeo exige diferença de1h. O caso N12 inteiro exige300 minutos noturnos de relógio, enquanto sem intervalo seriam360. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:75`, `:89`, `:130`; `app/ponto/motor_calculo_v2.py:440`, `:442`; `app/ponto/calculador/regras.py:85`.

**PROCEDENTE — contra “intervalos só do primeiro período”: não.** Os casos com pausa explicitamente têm um período; a propriedade reutiliza esses cenários. Falta uma chamada com pelo menos dois períodos e pausas distintas para distinguir esse mutante. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:72`, `:92`, `:125`.

A propriedade “não descontar duas vezes” **alcança o universo cortado comum**: N6 tem base12x36, envelope600 minutos e pausa02–03; o motor desconta intra e a porta corta quando `intra_descontada>0`. Ela compara as noturnas da porta com as do motor, com/sem pausa e com/sem feriado. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:44`, `:68`, `:92`; `app/ponto/motor_calculo_v2.py:2755`, `:2763`; `app/ponto/calculador/alimentacao.py:117`, `:121`.

Essa propriedade não afirma explicitamente que saíram dois segmentos, nem cobre pausa atravessando05h, múltiplos períodos ou mistura com FT. A igualdade genérica porta=motor já tem a contraprova480/390 acima. Referências: **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:92`, `:97`; `app/ponto/tests/test_o280_bateria_da_lei.py:4168`.

Execução integral dos testes novos, mutantes pela porta Django e suíte da árvore proposta: **[nao verificado]**. [nao verificado]

**5. Checklist das classes**

| Classe | Resultado nesta obra |
|---|---|
| Segundo juiz duplicando registrado | **Ausente no incremento A:** o transporte alimenta os auxiliares existentes do motor. A autoridade de cálculo está indicada em `app/docs/JUIZES.md:30`; delegações em `app/ponto/calculador/regras.py:63`, `:821`, `:829`. |
| Segundo escritor fora da porta | **Ausente para `intervalos_do_motor`:** A acrescenta leitura; a atribuição continua em `_carimbar_segmento`. O mapa novo é montado na porta. `app/ponto/motor_calculo_v2.py:1830`; **A:** `app/ponto/calculador/alimentacao.py:107`. |
| Relógio lido fora da borda | **Ausente no incremento A:** as linhas novas transportam datetimes já recebidos; não consultam “agora”. **A:** `app/ponto/calculador/alimentacao.py:107`, `:527`. |
| Derivação paralela de fonte única | **Ausente no incremento A:** copia os intervalos e chama a mesma implementação de split/desconto. **A:** `app/ponto/calculador/alimentacao.py:107`; `app/ponto/calculador/regras.py:829`; `app/ponto/motor_calculo_v2.py:442`. |
| Selo/RED sem mordida | **Presente parcialmente — PROCEDENTE:** há mordida contra vazio, mas falta contra coleta incompleta de períodos. **A:** `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:75`, `:92`, `:125`. |
| Ordem de trava | **Ausente no incremento A:** as alterações executáveis mostradas montam/consultam mapas e passam argumentos, sem acrescentar aquisição de trava. **A:** `app/ponto/calculador/alimentacao.py:65`, `:107`, `:457`, `:527`. |
| Pouso antes do pré-requisito | **DUVIDA:** o código-base contém a cura anterior exigida para alcançar split/noturnas após dobra. `app/ponto/calculador/regras.py:789`, `:795`. Evidência de suíte verde, leitura final e ordem real de pouso: **[nao verificado]**; a exigência está em `CLAUDE.md:557`. |

**6. Compatibilidade dos consumidores do dict**

**Não encontrei quebra nos chamadores concretos da cópia.** Os dois de produção obtêm os insumos pela porta antes de chamar `rubricas_por_dia`: fechamento e comando de diff. Referências: `app/ponto/services/fechamento.py:1237`, `:1240`; `app/ponto/management/commands/diff_calculador.py:312`, `:313`, `:418`.

Os chamadores de teste também usam `insumos_do_calculador`, inclusive os que fabricam períodos: `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:111`, `:113`; `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:120`, `:122`; `app/ponto/tests/test_he100_dia_sem_previsto.py:271`, `:272`; `app/ponto/tests/test_o280_bateria_da_lei.py:2775`, `:2776`, `:2859`, `:2861`.

Os consumidores encontrados acessam campos específicos; não apareceu serialização ou comparação estrita do dict inteiro nesses caminhos. Referências: `app/ponto/management/commands/diff_calculador.py:317`, `:318`, `:319`, `:320`, `:322`, `:323`; `app/ponto/tests/test_he100_dia_sem_previsto.py:296`.

**DUVIDA — o contrato passa a exigir a chave nova.** Um chamador manual antigo sem ela receberia `KeyError`, pois A usa indexação obrigatória; tal chamador não foi localizado na cópia. Referência: **A:** `app/ponto/calculador/alimentacao.py:457`. Consumidores externos à cópia: **[nao verificado]**.

## 01/11 04:25 · sessao 91057ad29c11 · - · - · origem=Ronald (tmux)

Leitura da cópia base e efeito virtual do anexo A; a cópia ainda passa `intervalos=()` em `app/ponto/calculador/alimentacao.py:514`.

1. Todos os leitores de `intervalos` em `do_dia`:

| Leitor | Efeito de A |
|---|---|
| Noturnas no ramo folga, `app/ponto/calculador/regras.py:713` | Par já cortado: pausa não intersecta os segmentos e o valor fica igual. Par inteiro: desconta a parcela noturna da pausa e pode diminuir. |
| Split HE, uma chamada por ponta, `app/ponto/calculador/regras.py:823` | Muda mesmo quando os pares de trabalho foram cortados, porque as pontas HE conservam o envelope; `app/ponto/calculador/alimentacao.py:96`. |
| Noturnas no ramo normal, inclusive dobra após a cura anterior, `app/ponto/calculador/regras.py:841` | Mesmo comportamento das noturnas da folga; a dobra continua até esse bloco, `app/ponto/calculador/regras.py:779` e `app/ponto/calculador/regras.py:795`. |

Não há outro uso do parâmetro no corpo: intra usa `pausa_min`, trabalhadas usa `trab`, dobra usa `dia/trab/feriados/feriado_em_dobra`, pontualidade recebe `intervalo_marcos` e `pontualidade`, parâmetros distintos; `app/ponto/calculador/regras.py:725`, `app/ponto/calculador/regras.py:740`, `app/ponto/calculador/regras.py:760`, `app/ponto/calculador/regras.py:776`, `app/ponto/calculador/regras.py:852`.

2. **PROCEDENTE — A altera dois mecanismos no split, não apenas o desconto.** `he_noturna` usa os intervalos para recuar por minutos trabalhados e também para descontar a pausa da janela noturna; `app/ponto/motor_calculo_v2.py:395`, `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:442`. Portanto o resultado pode diminuir, permanecer igual ou aumentar; `app/ponto/motor_calculo_v2.py:401`, `app/ponto/motor_calculo_v2.py:405`, `app/ponto/motor_calculo_v2.py:441`.

Execução AST em memória, sem importar Django nem escrever arquivos:

- Envelope 21–07, HE100=540, pausa02–03: sem intervalos=540; com intervalos=480 minutos HE100 noturna; `app/ponto/motor_calculo_v2.py:440`.
- Envelope 22–07, HE100=120, pausa05:30–06:30: sem intervalos=0; com intervalos=120. O recuo passa de05 para04, habilitando prorrogação; `app/ponto/motor_calculo_v2.py:395`, `app/ponto/motor_calculo_v2.py:110`.

3. **Ausência de duplo desconto no universo de segmentos disjuntos.** As noturnas começam de `minutos_noturnos(segmento)`, não de `minutos_trabalhados`; a pausa é intersectada com cada segmento e interseções vazias são ignoradas; `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:86`, `app/ponto/motor_calculo_v2.py:164`, `app/ponto/motor_calculo_v2.py:167`. O desconto em `minutos_trabalhados` não é reutilizado como minuendo das noturnas; `app/ponto/calculador/regras.py:600`, `app/ponto/calculador/regras.py:841`. No split, a grandeza HE já líquida determina quanto trabalho recuar; depois retirar pausas do envelope identifica a parte trabalhada desse trecho, sem retirar novamente minutos de HE; `app/ponto/motor_calculo_v2.py:392`, `app/ponto/motor_calculo_v2.py:403`, `app/ponto/motor_calculo_v2.py:442`.

4. **Formato:** nenhum desses leitores exige recorte por dia civil. Recebem pares de datetimes e intersectam com o par/ponta da chamada; `app/ponto/motor_calculo_v2.py:164`, `app/ponto/motor_calculo_v2.py:397`, `app/ponto/motor_calculo_v2.py:399`. O desconto noturno também une pausas sobrepostas antes de medir, evitando desconto repetido dentro de uma chamada; `app/ponto/motor_calculo_v2.py:163`, `app/ponto/motor_calculo_v2.py:169`.

5. **PROCEDENTE — limite da propriedade nova:** o teste novo exercita pausa02–03 e compara as noturnas da porta com o motor, portanto alcança o universo cortado em que um segundo desconto seria possível; `anexo diffA_bd9.patch → app/ponto/tests/test_v3_dobra_split_noturno_porta.py:68`, `anexo diffA_bd9.patch → app/ponto/tests/test_v3_dobra_split_noturno_porta.py:90`. Contudo a igualdade porta=motor não vale genericamente para segmentos: a pausa04:30–05:30 deixa 480 por par e390 por segmentos, fenômeno já documentado e testado; `app/ponto/calculador/periodo.py:91`, `app/ponto/tests/test_o280_bateria_da_lei.py:4162`, `app/ponto/tests/test_o280_bateria_da_lei.py:4168`. A não muda isso: as pausas têm interseção zero com segmentos, enquanto o segmento iniciado05:30 não satisfaz a condição de prorrogação; `app/ponto/motor_calculo_v2.py:167`, `app/ponto/motor_calculo_v2.py:110`. É limitação anterior explicitamente declarada, não novo duplo desconto; `app/ponto/calculador/periodo.py:98`.

6. **DUVIDA — equivalência dos intervalos agregados por dia com os de cada período depende da geometria.** Cada ponta recebe todos os intervalos do dia, e o comentário afirma equivalência com os intervalos próprios do período; `app/ponto/calculador/regras.py:823`, `app/ponto/calculador/regras.py:826`. A interseção elimina intervalos alheios disjuntos, mas não distingue origem quando períodos se sobrepõem; `app/ponto/motor_calculo_v2.py:164`, `app/ponto/motor_calculo_v2.py:397`. Existência desse cenário no dado de produção: [nao verificado].

## 01/11 04:24 · sessao 3adcf1e3c14d · - · - · origem=Ronald (tmux)

Auditoria do motor, só leitura:

- **Escritor do campo:** default `()` no dataclass `PeriodoCalculo` (`app/ponto/motor_calculo_v2.py:247`); escrita de produção em `_carimbar_segmento`, convertendo cada par em tupla (`app/ponto/motor_calculo_v2.py:1830`). Os três motores recortam antes e carimbam: 12x36 (`app/ponto/motor_calculo_v2.py:2752`), partido (`app/ponto/motor_calculo_v2.py:2879`), comercial (`app/ponto/motor_calculo_v2.py:3212`). O recorte é à janela efetiva `[e,s]`, sem corte por dia civil (`app/ponto/motor_calculo_v2.py:1871`).
- **Leitor direto dentro do motor:** `_aplicar_he_noturna` entrega `periodo.intervalos_do_motor` a `he_noturna` (`app/ponto/motor_calculo_v2.py:1006`). **`calcular_noturnas` não lê o atributo:** recebe a variável local que também foi carimbada; chamadas 12x36/partido/comercial em `app/ponto/motor_calculo_v2.py:2780`, `app/ponto/motor_calculo_v2.py:2884`, `app/ponto/motor_calculo_v2.py:3235`. Escritor intermediário único `_escrever_noturnas` em `app/ponto/motor_calculo_v2.py:991`.
- **Leitores diretos externos de produção na cópia:** corte dos segmentos em `app/ponto/calculador/alimentacao.py:121`; recálculo do período em `app/ponto/calculador/periodo.py:741`; recálculo noturno por par em `app/ponto/calculador/periodo.py:751`. O leitor adicional de transporte está exclusivamente no anexo, `alimentacao.py:107` da árvore proposta.
- **Escritores em fixtures:** `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:51`, `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:48`, `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:44`. Asserções externas existentes leem em `app/ponto/tests/test_noturno_desconta_intervalo.py:150`, `app/ponto/tests/test_o272_intra_fora_da_janela.py:91`, `app/ponto/tests/test_o272_intra_fora_da_janela.py:102`, `app/ponto/tests/test_o272_intra_fora_da_janela.py:118`, `app/ponto/tests/test_s5b_porta_unica.py:145`.

**PROCEDENTE — “mesma tupla” não garante o mesmo cálculo.** `_aplicar_he_noturna` ancora em `periodo.entrada/saida`, crus (`app/ponto/motor_calculo_v2.py:1003`); a porta usa `efetivo_entrada/saida` (`app/ponto/calculador/alimentacao.py:73`) nas pontas de HE (`app/ponto/calculador/alimentacao.py:96`). Portanto mesmo conteúdo dos intervalos pode produzir splits diferentes quando a janela muda as pontas. O adicional normal do motor, em contrapartida, usa as pontas efetivas nas três chamadas citadas (`app/ponto/motor_calculo_v2.py:2780`, `app/ponto/motor_calculo_v2.py:2884`, `app/ponto/motor_calculo_v2.py:3235`).

**DUVIDA — agregação por dia perde a associação período → pausas.** A lista do anexo reúne todas as pausas do dia da jornada; cada ponta recebe todas. `he_noturna` recua sobre os intervalos e depois desconta a interseção (`app/ponto/motor_calculo_v2.py:440`), portanto equivalência exige que pausas de outros períodos não intersectem o envelope da ponta. Contraprova sintética executada em memória, com as funções por AST: envelope 21–07, HE100 540, pausa própria 02–03 → 480 noturnos; acrescentar pausa alheia 04–05 → 420; intervalos vazios → 540 (`app/ponto/motor_calculo_v2.py:395`, `app/ponto/motor_calculo_v2.py:440`). Ocorrência desse cruzamento em produção: **[nao verificado]**. A cópia documenta pares crus duplicados em objetos distintos, mas isso não prova pausas distintas entre cópias (`app/ponto/calculador/periodo.py:701`).

- **FT incluída:** `periodos_do_resultado` lê `periodos + periodos_ft` (`app/ponto/calculador/nucleo.py:393`, `app/ponto/calculador/nucleo.py:400`); a porta usa essa função (`app/ponto/calculador/alimentacao.py:66`). O motor registra o dia antes de classificar FT (`app/ponto/motor_calculo_v2.py:2448`) e põe o período em FT (`app/ponto/motor_calculo_v2.py:2480`); o override do partido também preserva dia de FT (`app/ponto/motor_calculo_v2.py:2943`).
- **Dia é da jornada:** chave vem de `resultado.dia_da_jornada[id(p)]`, não da data civil do intervalo (`app/ponto/calculador/alimentacao.py:63`, `app/ponto/calculador/alimentacao.py:68`); períodos sem mapa são descartados e contados (`app/ponto/calculador/alimentacao.py:69`). Intervalos de datas civis diferentes podem legítima e conjuntamente pertencer à jornada (`app/ponto/motor_calculo_v2.py:2930`).
- **Aberto:** os três motores retornam antes do carimbo e noturnas (`app/ponto/motor_calculo_v2.py:2740`, `app/ponto/motor_calculo_v2.py:2867`, `app/ponto/motor_calculo_v2.py:3192`); a porta publica aberto e sai antes de ler os intervalos (`app/ponto/calculador/alimentacao.py:74`). Há crédito de pares fechados no aberto, mas ele copia somente trabalhado/fora da janela, sem copiar intervalos ou noturnas (`app/ponto/motor_calculo_v2.py:2541`, `app/ponto/motor_calculo_v2.py:2547`).
- **Pausa dupla:** o adicional normal não deriva de `minutos_trabalhados`; calcula noturno bruto e subtrai pausa (`app/ponto/motor_calculo_v2.py:971`). No split, o recuo é em minutos de trabalho, pulando pausa, e só então tira a pausa do envelope obtido; isso evita o erro de descontar pausa de uma ponta já curta (`app/ponto/motor_calculo_v2.py:395`, `app/ponto/motor_calculo_v2.py:440`). O subtrator une intervalos sobrepostos antes de medir (`app/ponto/motor_calculo_v2.py:164`).

**Limite da cópia:** `intervalos_por_dia` e o novo teste não estão aplicados no filesystem; referências correspondentes precisam indicar o anexo. A base contém somente `he_pontas_por_dia` no dict de saída (`app/ponto/calculador/alimentacao.py:55`).

## 01/11 04:24 · sessao 297795115ced · - · - · origem=Ronald (tmux)

**PROCEDENTE — lacuna no mutante “só primeiro período”.** Os testes novos exercitam um período com pausa: o RED afirma `len(periodos)==1`; o teste N12 também afirma um período; a propriedade reutiliza os mesmos cenários. Um mutante que acumule somente a lista do primeiro período preserva esses resultados. Falta cenário com dois períodos e intervalos distintos, incluindo `periodos_ft` e agrupamento pelo mesmo dia de jornada. Referências do **anexo**: `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:72`, `:92`, `:125`; acumulador proposto: `app/ponto/calculador/alimentacao.py:107`.

**Mordida contra `intervalos=()` demonstrável por inspeção.** No RED, HE100=540 e envelope21–07 sem intervalos fazem `he_noturna` recuar até22 e contar540 minutos=9h, contra8h exigidas; o gêmeo também exige diferença de1h. O N12 inteiro exige300 minutos de relógio, enquanto sem intervalos recebe360. Referências: `app/ponto/motor_calculo_v2.py:432`, `:440`, `:442`; anexo `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:75`, `:89`, `:130`. Execução da suíte e dos mutantes: **[nao verificado]**.

**A propriedade cobre o universo de par cortado comum.** N6 tem base12x36 e envelope600min; o motor desconta pausa acima360 e publica `intra_descontada`; a porta corta os pares quando essa resposta é positiva. A propriedade compara as noturnas da porta com o motor, com/sem pausa e feriado verdadeiro/falso. Referências: anexo `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:44`, `:92`; cópia `app/ponto/motor_calculo_v2.py:2755`, `:2762`, `:2763`; `app/ponto/calculador/alimentacao.py:115`, `:117`, `:121`. Ela não afirma explicitamente dois segmentos; a guarda explícita de um par pertence ao cenário N12 não cortado. Referências do anexo: `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:97`, `:127`.

**Q6 — quebra ausente nos chamadores concretos encontrados.** Os dois chamadores de produção obtêm o mesmo dict pela porta antes de chamar `rubricas_por_dia`: `app/ponto/services/fechamento.py:1237`, `:1240`; `app/ponto/management/commands/diff_calculador.py:312`, `:313`, `:418`. A porta acrescenta marcos e pontualidade ao próprio dict retornado por `segmentos_do_motor`: `app/ponto/calculador/alimentacao.py:302`, `:303`, `:304`, `:305`.

Os chamadores de teste também usam a porta, inclusive com períodos falsos: `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:111`, `:113`; `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:120`, `:122`; `app/ponto/tests/test_he100_dia_sem_previsto.py:271`, `:272`; `app/ponto/tests/test_o280_bateria_da_lei.py:2775`, `:2776`, `:2859`, `:2861`. Não encontrei construção manual desse dict, serialização do dict inteiro ou igualdade estrita dele; consumidores identificados acessam campos específicos: `app/ponto/management/commands/diff_calculador.py:317`, `:318`, `:319`, `:320`, `:322`, `:323`; `app/ponto/tests/test_he100_dia_sem_previsto.py:296`. Consumidores externos à cópia: **[nao verificado]**.

**DUVIDA — contrato obrigatório ampliado.** O novo `insumos['intervalos_por_dia']` causa `KeyError` para qualquer chamador manual antigo sem essa chave; tal chamador não foi encontrado na cópia. Referência do anexo: `app/ponto/calculador/alimentacao.py:457`; contraponto aos chamadores existentes: `app/ponto/services/fechamento.py:1237`, `:1240`.

**Limite da prova:** a cópia local contém a chamada antiga `intervalos=()` em `app/ponto/calculador/alimentacao.py:514`; linhas novas acima se referem ao arquivo resultante mostrado pelo anexo, não a arquivo editado localmente.

## 01/11 04:11 · sessao 8ad9dce1139f · V3-DOBRA-SEM-NOTURNA · censo · origem=Code

**Achado central:** a porta de produção passa `intervalos=()`, mas entrega à HE pontas que ainda são envelopes do período. Assim, o split noturno da HE recebe intervalos no motor e nenhum intervalo pelo calculador. (`app/ponto/calculador/alimentacao.py:96`, `app/ponto/calculador/alimentacao.py:514`, `app/ponto/calculador/regras.py:828`, `app/ponto/motor_calculo_v2.py:1006`)

**1. Chamadores de `regras.do_dia`**

Os padrões são `intervalos=()` e `he_pontas=None`. Na tabela, “padrão” significa argumento omitido, inclusive nos dicionários encaminhados. (`app/ponto/calculador/regras.py:574`)

| Local de chamada | Origem de `intervalos` | Origem de `he_pontas` |
|---|---|---|
| `app/ponto/calculador/alimentacao.py:501` | `()` explícito em `:514`. | `_he_pontas.get(k)` em `:513`; mapa recebido em `insumos['he_pontas_por_dia']`, lido em `:444`. |
| `app/ponto/tests/test_calculador_regras.py:28,35,42,54,69,85,102,116,131,137,206,308` | Padrão. | Padrão. |
| `app/ponto/tests/test_s5b_regra_pontualidade.py:210,220,230,241,250` | Padrão; `_COMUM` não contém o argumento (`:206`). | Padrão; mesma origem (`:206`). |
| `app/ponto/tests/test_s5b_regra_pontualidade.py:312,321,329,348,364,397,407,415,433,445` | Padrão. | Padrão. |
| `app/ponto/tests/test_s5b_regra_pontualidade.py:483,534,549,557` | Padrão; `_COMUM` não contém o argumento (`:469`). | Padrão; mesma origem (`:469`). |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:209,215,240,281` | Padrão. | Padrão. |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:312` | Padrão. | `[_p1, _p2]` em `:318`; fixtures 08–13, HE50=60, e 18–23:30, HE50=90/HE100=30, construídas em `:301–304`. |
| `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:227` | Encaminha `**kw`; os dois chamadores fornecem somente `folga_e_feriado`, portanto padrão (`:228`, `:231`, `:236`). | Padrão pelo mesmo encaminhamento. |
| `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61` | `(iv,)` em `:63`; fixture 05:30–06:30, construída em `:57`. | `[ponta]` em `:65`; fixture 22–07, HE50=120/HE100=0, construída em `:60`. |
| `app/ponto/tests/test_v3_dobra_sem_noturna.py:57` | `**kw` contém `iv`, fixture 02–03 (`:42`, `:46`). | Se dobra: `(e,s,0,trab)`; senão, excedente dividido entre 120/restante quando supera dez minutos, ou `[]` (`:50–54`). |

Na produção, `insumos_do_calculador` chama `segmentos_do_motor`; este lê as pontas **efetivas** e os minutos de HE dos períodos, formando `(efetivo_entrada, efetivo_saida, minutos_extra_50, minutos_extra_100)`. (`app/ponto/calculador/alimentacao.py:302`, `app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:91–96`)

**2. Chamadores de `he_noturna`**

O padrão de `he_noturna` também é `intervalos=()`. (`app/ponto/motor_calculo_v2.py:409`)

| Local de chamada | Intervalos recebidos e origem |
|---|---|
| `MotorBase._aplicar_he_noturna`, `app/ponto/motor_calculo_v2.py:1002` | `periodo.intervalos_do_motor`, passado em `:1006`; escrito por `_carimbar_segmento` em `:1830`. |
| `regras.do_dia`, alias `_he_not`, `app/ponto/calculador/regras.py:828` | Repassa seu argumento `intervalos` em `:829`. Na produção, recebe `()` de `app/ponto/calculador/alimentacao.py:514`; nos testes, recebe os valores da tabela anterior. |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:111` | Omitido → padrão; percorre a grade sintética declarada em `:90–98`. |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:122` | Omitido → padrão; compreensão sobre a mesma grade (`:122–123`). |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:307` | Omitido → padrão; percorre `_p1` e `_p2`, construídos em `:301–304`. |
| `app/ponto/tests/test_noturno_desconta_intervalo.py:249` | Variável local `intervalos`, fixture 23:30–00:30, escrita em `:248` e passada em `:250`. |
| `app/ponto/tests/test_noturno_desconta_intervalo.py:263` | Omitido → padrão. |
| `app/ponto/tests/test_noturno_desconta_intervalo.py:265` | Tupla explícita da fixture 23–23:30 (`:267`). |

A cadeia produtiva de formação e escrita dos intervalos é:

- `periodos_de_turnos` escreve o mapa por entrada usando `intervalos_do_turno(t)`. (`app/ponto/adaptador_motor.py:27`, `app/ponto/adaptador_motor.py:44`)
- `_periodos_via_autoridade` usa esse adaptador e passa o resultado a `_periodos_pelo_marco`. Este pode substituir os intervalos pela resposta de `juiz_batida.periodos_do_dia`, reconstruindo os instantes pelas batidas e escrevendo `intervalos[ent]`. (`app/ponto/motor_calculo_v2.py:509–531`, `app/ponto/motor_calculo_v2.py:616`, `app/ponto/motor_calculo_v2.py:687–721`)
- `calcular_mes` entrega a lista correspondente à entrada a `calcular_periodo`; os três ramos recortam pela janela efetiva e carimbam o campo. (`app/ponto/motor_calculo_v2.py:2342`, `app/ponto/motor_calculo_v2.py:2446–2447`, `app/ponto/motor_calculo_v2.py:2752–2753`, `app/ponto/motor_calculo_v2.py:2879–2880`, `app/ponto/motor_calculo_v2.py:3212–3213`)

Nos testes que exercitam a casca real por `_MotorFalso`, o campo homônimo é escrito pela fixture `_PeriodoFalso.__init__`; o método real é vinculado ao falso. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:51`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:59`)

**3. Escritor, leitores e autoridade de `intervalos_do_motor`**

O campo nasce como `()`. Seu escritor produtivo é `MotorBase._carimbar_segmento`, que copia a lista recebida para uma tupla. (`app/ponto/motor_calculo_v2.py:247`, `app/ponto/motor_calculo_v2.py:1822–1830`)

| Leitor produtivo direto | Uso |
|---|---|
| `app/ponto/motor_calculo_v2.py:1006` | `_aplicar_he_noturna` entrega o campo ao split. |
| `app/ponto/calculador/alimentacao.py:121` | `segmentos_do_motor` usa os intervalos para cortar os pares. |
| `app/ponto/calculador/periodo.py:741` | `reler` entrega os intervalos a `periodo_do_dia`. |
| `app/ponto/calculador/periodo.py:751` | `reler` entrega os intervalos a `noturno_no_par`. |

Os testes leem diretamente o campo em `app/ponto/tests/test_s5b_porta_unica.py:145`, `app/ponto/tests/test_noturno_desconta_intervalo.py:150` e `app/ponto/tests/test_o272_intra_fora_da_janela.py:91,102,118`; também verificam sua existência por `hasattr` em `app/ponto/tests/test_s5b_porta_unica.py:140–141`.

Há escritas de campo homônimo em objetos falsos, além do escritor produtivo: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:51`, `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:44` e `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:48`.

**Há autoridade registrada para as duas perguntas:**

| Pergunta registrada | Autoridade |
|---|---|
| “o vão entre batidas foi intervalo?” | `ponto/turnos.py::intervalos_do_turno`. (`app/core/juizes.py:197`) |
| “que períodos e que intervalo teve este dia?” | `ponto/juiz_batida.py::periodos_do_dia`. (`app/core/juizes.py:1159`) |

`JUIZES.md` aponta `core/juizes.py` como registro vivo. O comentário do motor que anuncia apenas “DOIS leitores” está desatualizado diante dos quatro locais acima. (`app/docs/JUIZES.md:11–12`, `app/ponto/motor_calculo_v2.py:1843–1848`)

**4. Valor passado e commit de origem** [nao verificado]

Hoje é **`intervalos=()`**, explicitamente. A linha entrou em `rubricas_por_dia` no commit **`a80bf914084157a8bc81dfeb58e6dbdf035b6994`**, de **02/10/2026, 15:38:42 −0300**, conforme o histórico da linha; naquela revisão estava na linha 348. (`app/ponto/calculador/alimentacao.py:514`; `a80bf914:app/ponto/calculador/alimentacao.py:348`)

A mensagem é **“[S5b] As rubricas entram na porta por MOVIMENTO, e a frota prova que nada mudou”**. O comentário introduzido no arquivo explica que a montagem foi movida do chamador, preservando as linhas e trocando locais por parâmetros, após cinco tentativas de reescrita que alteraram números. (`a80bf914:app/ponto/calculador/alimentacao.py:256–260`; `app/ponto/calculador/alimentacao.py:346–350`)

O motivo específico do vazio aparece no comentário **removido do DIFF por esse commit**: os pares já seriam segmentos de trabalho, com o intervalo fora deles; passar os intervalos novamente descontaria duas vezes. O comentário reservava o parâmetro para quem entregasse o envelope do turno. (`a80bf914^:app/ponto/management/commands/diff_calculador.py:423–428`)

Essa justificativa descreve os pares segmentados; as pontas atuais da HE são montadas separadamente, como envelopes. (`app/ponto/calculador/alimentacao.py:91–96`, `app/ponto/calculador/alimentacao.py:115–146`)

**5. O que `pares_por_dia` e `he_pontas_por_dia` guardam**

- **`he_pontas_por_dia`: envelope efetivo do período**, sem corte nos intervalos. A inclusão acontece antes da rejeição de par invertido/colapsado; períodos fechados sem HE deixam uma lista vazia conhecida. (`app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:91–111`)
- **`pares_por_dia`: segmentos**, mas somente corta quando `intra_descontada > 0`. Com desconto zero, acrescenta o envelope inteiro. (`app/ponto/calculador/alimentacao.py:115–125`, `app/ponto/calculador/alimentacao.py:145–146`)
- Mesmo com desconto positivo, cada intervalo só corta se **`_ini < _a <= s`**. Portanto, ignora intervalo que começa na entrada efetiva, antes/no cursor já avançado ou depois da saída. Diferença entre o corte e `intra_descontada` gera contador, sem correção dos segmentos. (`app/ponto/calculador/alimentacao.py:122–144`)

Há situações concretas em que existem intervalos carimbados sem desconto: no 12x36, duração até 360 minutos; no comercial, duração até 240 minutos; no cálculo por período do turno partido, a duração permanece inteira sem atribuição de `intra_descontada`. O campo começa em zero. (`app/ponto/motor_calculo_v2.py:252`, `app/ponto/motor_calculo_v2.py:2752–2763`, `app/ponto/motor_calculo_v2.py:2879–2884`, `app/ponto/motor_calculo_v2.py:3212–3219`)

Sem dia, sem carimbo, turno aberto ou par invertido/colapsado, o fluxo sai antes de produzir esses segmentos fechados. (`app/ponto/calculador/alimentacao.py:68–82`, `app/ponto/calculador/alimentacao.py:97–111`)

**6. Desconto por rubrica e divergências**

Aqui, “calculador” significa o caminho **`rubricas_por_dia → regras.do_dia`**. A releitura por `periodo.reler` entrega o campo de intervalos diretamente às suas contas. (`app/ponto/calculador/alimentacao.py:501–514`, `app/ponto/calculador/periodo.py:741`, `app/ponto/calculador/periodo.py:751`)

| Rubrica | Motor | Calculador pela porta atual |
|---|---|---|
| `horas_noturnas` | Subtrai a parte noturna dos intervalos e depois aplica o fator da hora, independentemente de `intra_descontada`. (`app/ponto/motor_calculo_v2.py:971–980`) | Recebe `intervalos=()`: a subtração explícita tira zero. A pausa só sai pela geometria dos pares, quando houve corte. (`app/ponto/calculador/alimentacao.py:514`, `app/ponto/calculador/regras.py:82–89`, `app/ponto/calculador/alimentacao.py:115–125`) |
| `horas_extras_50_noturna` | Recua pela quantidade de **trabalho**, pulando pausas; desconta a parte noturna dos intervalos e rateia por HE50/HE total. (`app/ponto/motor_calculo_v2.py:392–406`, `app/ponto/motor_calculo_v2.py:440–450`) | Usa envelope e intervalos vazios: não pula nem desconta pausas no split. (`app/ponto/calculador/alimentacao.py:96`, `app/ponto/calculador/alimentacao.py:514`, `app/ponto/calculador/regras.py:823–833`) |
| `horas_extras_100_noturna` | Mesma conta, rateada por HE100/HE total. (`app/ponto/motor_calculo_v2.py:440–450`) | Mesma ausência de intervalos; soma os resultados e divide por 60. (`app/ponto/calculador/regras.py:823–835`) |

Exemplos sintéticos abaixo neutralizam outra diferença: pontas cruas e efetivas são iguais. Para facilitar a comparação, usam fator noturno **1** e prorrogação habilitada; o fator é parametrizado pelo cadastro. (`app/ponto/motor_calculo_v2.py:978–980`)

| Turno e pausa | Motor | Calculador atual | Base da diferença |
|---|---|---|---|
| **11→00**, pausa **22:30→23:30**; trabalho=720 min; HE50=120 e HE100=120 min | `horas_noturnas=1h`; HE50 noturna=`0,5h`; HE100 noturna=`0,5h`. | `horas_noturnas=1h`; HE50 noturna=`1h`; HE100 noturna=`1h`. | Na cauda extra, o motor exclui a hora de pausa; o calculador mede a cauda sem intervalos. (`app/ponto/motor_calculo_v2.py:440–450`, `app/ponto/calculador/regras.py:828–835`, `app/ponto/calculador/alimentacao.py:514`) |
| **Dobra 21→07**, pausa **02→03**; HE100=540 min | Noturnas=`8h`; HE100 noturna=`8h`. | Noturnas=`8h`; HE100 noturna=`9h`. | O recuo sem pausas começa às 22h e conta nove horas; com pausas, o trecho trabalhado contém oito horas noturnas. (`app/ponto/motor_calculo_v2.py:392–450`, `app/ponto/calculador/alimentacao.py:514`; configuração equivalente da dobra em `app/ponto/tests/test_v3_dobra_sem_noturna.py:86–99`) |
| **12x36 22→03**, pausa **00→01**; `intra_descontada=0` | Noturnas=`4h`. | Noturnas=`5h`. | O motor desconta a pausa do noturno; a porta mantém o envelope porque a duração não supera seis horas. (`app/ponto/motor_calculo_v2.py:971–974`, `app/ponto/motor_calculo_v2.py:2755–2763`, `app/ponto/calculador/alimentacao.py:115–125`) |
| **22→07**, pausa **04:30→05:30**; desconto=60 min | Noturnas=`8h`. | Noturnas=`6,5h`. | Mesmo cortando, o segmento 05:30→07 perde a prorrogação: cada segmento chama `minutos_noturnos` separadamente, e esse segundo segmento não toca 00–05. (`app/ponto/calculador/regras.py:82–86`, `app/ponto/motor_calculo_v2.py:105–111`) |

Se a hora reduzida estiver ativa, os valores de `horas_noturnas` desses exemplos são multiplicados por `60/52,5`; o split da HE permanece em minutos de relógio convertidos para horas. (`app/ponto/motor_calculo_v2.py:974–980`, `app/ponto/calculador/regras.py:833–835`)

Além disso, o split do motor usa `periodo.entrada/saida`, enquanto a porta monta suas pontas pelas entradas/saídas efetivas. Os exemplos acima excluem essa diferença de janela. (`app/ponto/motor_calculo_v2.py:1003`, `app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:96`)

**7. Leitores das saídas de `rubricas_por_dia`**

| Destino | Leitura efetiva |
|---|---|
| **Lavratura** | `fechamento` chama a porta e entrega o resultado como `rubricas=` a `lavrar`, com `versao='oraculo'`, desde competência 10/2026. (`app/ponto/services/fechamento.py:1233`, `app/ponto/services/fechamento.py:1239`, `app/ponto/services/fechamento.py:1256`, `app/ponto/services/fechamento.py:1262`) |
| **Validação e persistência** | `_DONOS_DECLARADOS` lê `valores` e `nao_decide`; `lavrar` substitui os campos, registra as rubricas não decididas e persiste `DiaPago`. (`app/ponto/services/dia_pago.py:61–63`, `app/ponto/services/dia_pago.py:320–331`, `app/ponto/services/dia_pago.py:343–358`) |
| **DIFF** | Chama a porta, lê cada dia, `detalhe`, `valores` e `nao_decide`; soma e compara com motor/gravado. (`app/ponto/management/commands/diff_calculador.py:417`, `app/ponto/management/commands/diff_calculador.py:469`, `app/ponto/management/commands/diff_calculador.py:485`, `app/ponto/management/commands/diff_calculador.py:501–548`) |
| **Tela do espelho** | Consulta `DiaPago` pela versão padrão **`motor`**, e monta `dia.rubricas`; portanto esse caminho não consome a lavra `oraculo` da porta. (`app/ponto/services/espelho.py:421`, `app/ponto/services/espelho.py:824`, `app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:854`) |
| **Calendário** | Usa `linhas_do_periodo` sem trocar o padrão `motor`, e transforma a linha em rubricas. (`app/colaboradores/services/calendario.py:370`, `app/colaboradores/services/calendario.py:784–785`, `app/ponto/services/dia_pago.py:831`) |
| **PDF** | Lê `dia['rubricas']` montado pelo espelho, seguindo a mesma lavra `motor`. (`app/relatorios/pdf_espelho.py:906–910`, `app/ponto/services/espelho.py:421`, `app/ponto/services/espelho.py:824`) |
| **Totais do cartão / extrato** | Usam as somas com versão padrão `motor`. (`app/relatorios/cartao_pela_celula.py:143`, `app/ponto/services/dia_pago.py:548`, `app/ponto/services/dia_pago.py:557`, `app/relatorios/views.py:487`, `app/ponto/services/dia_pago.py:750`, `app/ponto/services/dia_pago.py:782`) |
| **Export TXT** | Lê `FechamentoMensal` e `eventos_do_fechamento`; a consulta diária da porta de coerência usa explicitamente `versao='motor'`. (`app/folha/export.py:698–713`, `app/folha/porta_export.py:588–589`) |

Os testes que chamam e leem a saída da porta são: [nao verificado]

- `app/ponto/tests/test_he100_dia_sem_previsto.py:272` → leituras em `:281`, `:286`, `:292`.
- `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:121` → leituras em `:154–155`.
- `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:112` → leituras em `:126–130`.
- `app/ponto/tests/test_o280_bateria_da_lei.py:2776` → leituras em `:2780–2781`; chamada em `:2861` → leituras em `:2864–2872`.

## 01/11 04:09 · sessao 1d52753ed6cd · - · - · origem=Ronald (tmux)

Q5 — `segmentos_do_motor`:

- `he_pontas_por_dia` guarda **envelope efetivo do período**, `(e, s, extra50, extra100)`, sem cortar intervalos: lê `efetivo_entrada/saida` em `app/ponto/calculador/alimentacao.py:73`, cria lista para período fechado e acrescenta a tupla quando há HE em `app/ponto/calculador/alimentacao.py:91-96`. Essa inclusão precede inclusive a rejeição de `s <= e`, feita em `app/ponto/calculador/alimentacao.py:102-111`.
- `pares_por_dia` guarda segmentos cortados **somente quando** `intra_descontada > 0`; com desconto zero, acrescenta o envelope inteiro. Evidência: `app/ponto/calculador/alimentacao.py:115-125`, escrita em `app/ponto/calculador/alimentacao.py:145-146`.
- Mesmo com desconto positivo, cada intervalo só corta se `_ini < _a <= s`: intervalo começando na entrada efetiva (`_a == e`), começando antes/no cursor já avançado, ou começando depois de `s` é ignorado; o cursor avança ao fim do intervalo aceito. Evidência: `app/ponto/calculador/alimentacao.py:116-125`. Divergência entre minutos cortados e `intra_descontada` apenas incrementa contador; não corrige segmentos: `app/ponto/calculador/alimentacao.py:126-144`.
- Sem dia, sem carimbo, turno aberto e par invertido/colapsado saem antes desse corte: `app/ponto/calculador/alimentacao.py:68-82`, `app/ponto/calculador/alimentacao.py:97-111`.
- Um motivo concreto de intervalo existente com desconto zero: 12x36 só desconta da duração quando o envelope supera 360 min (`app/ponto/motor_calculo_v2.py:2754-2763`); comercial só quando supera 240 min (`app/ponto/motor_calculo_v2.py:3214-3219`); turno partido carimba intervalos mas usa duração inteira, sem escrever `intra_descontada` (`app/ponto/motor_calculo_v2.py:2879-2884`). O campo começa em zero: `app/ponto/motor_calculo_v2.py:252`.

Q6 — Comparação atual:

| Rubrica | Motor | Calculador na porta atual |
|---|---|---|
| `horas_noturnas` | `calcular_noturnas` subtrai os minutos noturnos dos intervalos, depois aplica fator da hora; independe de `intra_descontada`: `app/ponto/motor_calculo_v2.py:971-980`. Três ramos entregam intervalos ao escritor: `app/ponto/motor_calculo_v2.py:2780`, `app/ponto/motor_calculo_v2.py:2884`, `app/ponto/motor_calculo_v2.py:3235`. | Porta passa `intervalos=()` (`app/ponto/calculador/alimentacao.py:514`); assim a subtração explícita de `noturnas_do_dia` não tira nada (`app/ponto/calculador/regras.py:82-89`). O desconto vem apenas da geometria cortada em `pares_por_dia` (`app/ponto/calculador/alimentacao.py:115-146`). |
| `horas_extras_50_noturna` | Entrega `periodo.intervalos_do_motor` para `he_noturna`: `app/ponto/motor_calculo_v2.py:1002-1006`; recua pelos minutos trabalhados, pulando pausas (`app/ponto/motor_calculo_v2.py:392-406`), subtrai a parcela noturna dos intervalos e rateia por extra50/total (`app/ponto/motor_calculo_v2.py:440-450`). | Recebe envelope em `he_pontas` (`app/ponto/calculador/alimentacao.py:91-96`), e o mesmo `intervalos=()` da porta (`app/ponto/calculador/alimentacao.py:514`) chega ao split (`app/ponto/calculador/regras.py:823-829`): não pula nem desconta pausas. |
| `horas_extras_100_noturna` | Mesmo cálculo compartilhado, rateio por extra100/total: `app/ponto/motor_calculo_v2.py:440-450`. | Mesma ausência de intervalos; resultado acumulado e convertido em horas: `app/ponto/calculador/regras.py:823-835`. |

**Exemplos calculados em memória com AST das funções reais**, via `python3 -B`, sem importar Django nem escrever arquivos. As hipóteses são pontas cruas = efetivas, fator 1 e prorrogação habilitada; resultados decorrem de `app/ponto/motor_calculo_v2.py:82-118`, `app/ponto/motor_calculo_v2.py:162-196`, `app/ponto/motor_calculo_v2.py:392-451` e montagem em `app/ponto/calculador/alimentacao.py:115-146`:

| Turno / intervalo | Motor | Calculador |
|---|---|---|
| Dia normal **11→00**, pausa **22:30→23:30**, trabalho 720 min, HE50=120 e HE100=120 min | AN=60 min; HE50not=30 min; HE100not=30 min | AN=60 min; HE50not=60 min; HE100not=60 min |
| Dobra **21→07**, pausa **02→03**, HE100=540 min | AN=480 min; HE100not=480 min | AN=480 min; HE100not=540 min |
| 12x36 curto **22→03**, pausa **00→01**, `intra_descontada=0` | AN=240 min | AN=300 min, porque o par não é cortado |
| **22→07**, pausa **04:30→05:30**, `intra_descontada=60` | AN=480 min | AN=390 min: segmento **05:30→07** perde a prorrogação, porque `minutos_noturnos` decide a extensão a partir da entrada de cada segmento (`app/ponto/motor_calculo_v2.py:105-111`; chamadas independentes em `app/ponto/calculador/regras.py:82-86`) |

Além do intervalo, há diferença de pontas no split: motor usa `periodo.entrada/saida` (`app/ponto/motor_calculo_v2.py:1003`), campos documentados como crus (`app/ponto/motor_calculo_v2.py:239-246`); calculador recebe as efetivas (`app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:96`). Os exemplos acima neutralizam essa outra diferença.

## 01/11 04:08 · sessao 78ee2ae67a93 · - · - · origem=Ronald (tmux)

Censo Q3:

- Escritor produtivo de `PeriodoCalculo.intervalos_do_motor`: `MotorBase._carimbar_segmento`, que copia `intervalos` para tupla; o campo nasce com `()` na dataclass. Os três ramos recortam a lista pela janela antes de carimbar. (`app/ponto/motor_calculo_v2.py:247`, `app/ponto/motor_calculo_v2.py:1822`, `app/ponto/motor_calculo_v2.py:1830`, `app/ponto/motor_calculo_v2.py:2752`, `app/ponto/motor_calculo_v2.py:2879`, `app/ponto/motor_calculo_v2.py:3212`)
- Leitores produtivos diretos: `_aplicar_he_noturna` entrega o campo a `he_noturna`; `segmentos_do_motor` percorre a lista; `periodo.reler` entrega a lista a `periodo_do_dia` e a `noturno_no_par`. São quatro locais em três funções. (`app/ponto/motor_calculo_v2.py:1006`, `app/ponto/calculador/alimentacao.py:121`, `app/ponto/calculador/periodo.py:741`, `app/ponto/calculador/periodo.py:751`)
- Leitores diretos em testes: `test_s5b_porta_unica.py:145` e acesso por `hasattr` em `:140-141`; `test_noturno_desconta_intervalo.py:150`; `test_o272_intra_fora_da_janela.py:91`, `:102`, `:118`, todos sob `app/ponto/tests/`. [nao verificado]
- Escritores de campo homônimo em objetos falsos de testes: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:51`, `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:44`, `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:48`.
- Autoridades registradas: pergunta “o vão entre batidas foi intervalo?” → `ponto/turnos.py::intervalos_do_turno`; pergunta “que períodos e que intervalo teve este dia?” → `ponto/juiz_batida.py::periodos_do_dia`. O registro também distingue a pergunta de valor do PAR → `MotorBase.calcular_periodo`. (`app/core/juizes.py:197`, `app/core/juizes.py:1159`, `app/core/juizes.py:1160`)
- `JUIZES.md` remete ao registro vivo `core/juizes.py` e lista geometria/turno de modo geral; a entrada nominal para intervalo está no registro Python. (`app/docs/JUIZES.md:11`, `app/docs/JUIZES.md:20`)

Censo Q7:

| Destino | Leitura efetiva e evidências |
|---|---|
| Lavratura | `fechamento` chama `rubricas_por_dia`, passa o resultado como `rubricas=` a `lavrar` e escolhe `versao='oraculo'`, apenas desde 10/2026. (`app/ponto/services/fechamento.py:1233`, `:1239`, `:1256`, `:1262`) |
| Escritor das linhas | `lavrar` percorre `Rubricas.valores`, substitui campos, registra `nao_decide` no detalhe e persiste `DiaPago`. (`app/ponto/services/dia_pago.py:310`, `:320`, `:322`, `:328`, `:329`, `:343`, `:358`) |
| DIFF | Chama a porta, busca cada dia, lê `detalhe`, `valores` e `nao_decide`, soma e compara com o motor/gravado. (`app/ponto/management/commands/diff_calculador.py:417`, `:469`, `:485`, `:501`, `:517`, `:519`, `:535`, `:547`) |
| Tela do espelho | Lê `DiaPago` pela versão padrão `motor`; portanto não lê a saída `oraculo` da porta. A consulta é `espelho.py:421`, o padrão/filtro ficam em `dia_pago.py:831`/`:854`, o dicionário em `espelho.py:824`, e a exibição em `templates/ponto/espelho.html:271-278`, todos sob `app/`. |
| Calendário | Chama `linhas_do_periodo` sem sobrescrever a versão padrão `motor`, e transforma a linha em rubricas. (`app/colaboradores/services/calendario.py:370`, `:784`, `:785`; `app/ponto/services/dia_pago.py:831`, `:854`) |
| PDF | Lê `dia['rubricas']` montado pelo espelho, portanto também segue a linha `motor`. (`app/relatorios/pdf_espelho.py:906`, `:909`; `app/ponto/services/espelho.py:421`, `:824`; `app/ponto/services/dia_pago.py:831`) |
| Totais do cartão | Usa `soma_da_competencia` sem mudar `versao='motor'`. (`app/relatorios/cartao_pela_celula.py:143`; `app/ponto/services/dia_pago.py:548`, `:557`) |
| Extrato parcial | Usa `soma_do_periodo` sem mudar `versao='motor'`. (`app/relatorios/views.py:487`; `app/ponto/services/dia_pago.py:750`, `:782`) |
| Export TXT | Lê `FechamentoMensal`, chama `eventos_do_fechamento` e emite `fech.horas_noturnas` como adicional noturno; não lê `rubricas_por_dia`. A porta de coerência consulta `DiaPago` versão `motor`. (`app/folha/export.py:698`, `:700`, `:711`, `:436`; `app/folha/porta_export.py:588`, `:589`) |

Testes que chamam e leem resultados da porta: [nao verificado]

- `app/ponto/tests/test_he100_dia_sem_previsto.py:272` → leitura em `:281`, `:286`, `:292`.
- `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:121` → leitura em `:154`, `:155`.
- `app/ponto/tests/test_s5b_vinculo_do_dia_efeito.py:112` → leitura em `:126`, `:127`, `:129`, `:130`.
- `app/ponto/tests/test_o280_bateria_da_lei.py:2776` → leitura em `:2780`, `:2781`; segunda chamada em `:2861` → leitura em `:2864-2872`.

Os testes de ligação/AST inspecionam a existência da função e as chamadas, sem ler sua saída: `app/ponto/tests/test_s5b_porta_unica.py:52`, `:75`, `:210`, `:232`; `app/ponto/tests/test_s5b_troca_ligada.py:43`, `:47`.

## 01/11 04:08 · sessao 082d38a8e46a · - · - · origem=Ronald (tmux)

Censo estático: **40 locais de chamada de `regras.do_dia`: 1 produção + 39 testes**; **8 locais de chamada de `he_noturna`: 2 produção + 6 testes**. As listas abaixo individualizam os locais; não executei testes. [não verificado: execução] [nao verificado]

**`regras.do_dia`: valores padrão**

Quando omitidos, `intervalos=()` e `he_pontas=None`, conforme `app/ponto/calculador/regras.py:572-574`.

| Chamador | `intervalos` | `he_pontas` |
|---|---|---|
| `app/ponto/calculador/alimentacao.py:501` | Tupla vazia **explícita**, `app/ponto/calculador/alimentacao.py:514` | `_he_pontas.get(k)`, `app/ponto/calculador/alimentacao.py:513`; mapa vem de `insumos.get('he_pontas_por_dia') or {}`, `app/ponto/calculador/alimentacao.py:444`. `insumos_do_calculador` usa `segmentos_do_motor`, `app/ponto/calculador/alimentacao.py:302`; este monta lista com `(efetivo_entrada, efetivo_saida, minutos_extra_50, minutos_extra_100)`, `app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:91-96`. |
| `app/ponto/tests/test_calculador_regras.py:28,35,42,54,69,85,102,116,131,137,206,308` | Omitido → `()` | Omitido → `None` |
| `app/ponto/tests/test_s5b_regra_pontualidade.py:210,220,230,241,250` | Omitido inclusive nos `**_COMUM`; `_COMUM` não tem esses campos, `app/ponto/tests/test_s5b_regra_pontualidade.py:206-207` | Omitido → `None` |
| `app/ponto/tests/test_s5b_regra_pontualidade.py:312,321,329,348,364,397,407,415,433,445` | Omitido → `()` | Omitido → `None` |
| `app/ponto/tests/test_s5b_regra_pontualidade.py:483,534,549,557` | Omitido inclusive nos `**_COMUM`; `_COMUM` não tem esses campos, `app/ponto/tests/test_s5b_regra_pontualidade.py:469-470` | Omitido → `None` |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:209,215,240,281` | Omitido → `()` | Omitido → `None` |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:312` | Omitido → `()` | `[_p1,_p2]`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:318`; fixtures criadas em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:301-304`: 08–13 com HE50=60; 18–23:30 com HE50=90, HE100=30. |
| `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:227` | Encaminha `**kw`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:228`; chamadas fornecem somente `folga_e_feriado`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:231,236`; portanto default `()` | Mesmo encaminhamento, portanto default `None` |
| `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61` | `(iv,)`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:63`; `iv` é fixture 05:30–06:30 do dia seguinte, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:56-57` | `[ponta]`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:65`; ponta fixture 22–07, HE50=120, HE100=0, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:60` |
| `app/ponto/tests/test_v3_dobra_sem_noturna.py:57` | `**kw` contém `intervalos=iv`, `app/ponto/tests/test_v3_dobra_sem_noturna.py:44-46`; `iv` fixture 08/09 02–03, `app/ponto/tests/test_v3_dobra_sem_noturna.py:42` | `**kw`: se `dobra`, `(e,s,0,trab)`, `app/ponto/tests/test_v3_dobra_sem_noturna.py:50-51`; senão excedente `max(trab-previsto,0)`, dividido 120/restante se >10, senão `[]`, `app/ponto/tests/test_v3_dobra_sem_noturna.py:52-54`. |

**`he_noturna`: todos os locais**

| Chamador | Intervalos recebidos / escritor |
|---|---|
| `MotorBase._aplicar_he_noturna`, `app/ponto/motor_calculo_v2.py:1002` | Passa `periodo.intervalos_do_motor`, `app/ponto/motor_calculo_v2.py:1006`. Escritor de produção: `_carimbar_segmento`, `app/ponto/motor_calculo_v2.py:1822-1830`. |
| `regras.do_dia`, alias `_he_not`, `app/ponto/calculador/regras.py:821,828` | Passa argumento `intervalos` recebido por `do_dia`, `app/ponto/calculador/regras.py:829`; em produção chega `()` explícito, `app/ponto/calculador/alimentacao.py:514`; nos testes vale tabela anterior. |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:111` | Omite → `()` padrão, `app/ponto/motor_calculo_v2.py:409`; parâmetros vêm da `GRADE`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:90-98,107-112`. |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:122` | Omite → `()` padrão; comprehension sobre mesma `GRADE`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:122-123`. |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:307` | Omite → `()` padrão; duas pontas de fixture `_p1,_p2`, `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:301-308`. |
| `app/ponto/tests/test_noturno_desconta_intervalo.py:249` | Variável local `intervalos`, fixture 23:30–00:30, `app/ponto/tests/test_noturno_desconta_intervalo.py:247-250`. |
| `app/ponto/tests/test_noturno_desconta_intervalo.py:263` | Omite → `()` padrão, `app/ponto/tests/test_noturno_desconta_intervalo.py:263-264`. |
| `app/ponto/tests/test_noturno_desconta_intervalo.py:265` | Tupla explícita de fixture 23–23:30, `app/ponto/tests/test_noturno_desconta_intervalo.py:265-267`. |

**Cadeia de origem dos intervalos entregues pelo motor**

- `periodos_de_turnos` escreve `intervalos[e_ts]=intervalos_do_turno(t)`: `app/ponto/adaptador_motor.py:27,44`. `_periodos_via_autoridade` chama esse adaptador e depois `_periodos_pelo_marco`: `app/ponto/motor_calculo_v2.py:509-531`.
- `_periodos_pelo_marco` consulta `juiz_batida.periodos_do_dia`: `app/ponto/motor_calculo_v2.py:577,616`; pode remover os intervalos do dia e reescrevê-los com pausas julgadas pela ata: `app/ponto/motor_calculo_v2.py:687,689-721`.
- `calcular_mes` recebe o mapa de `montar_periodos` e entrega a lista pela entrada a `calcular_periodo`: `app/ponto/motor_calculo_v2.py:2342,2446-2447`.
- Os três ramos de cálculo recortam pela janela efetiva antes de chamar o escritor do campo: `app/ponto/motor_calculo_v2.py:2752-2753,2879-2880,3212-3213`; `_carimbar_segmento` grava a tupla: `app/ponto/motor_calculo_v2.py:1830`.
- Nos testes que chamam a casca real via `_MotorFalso`, o escritor de fixture é `_PeriodoFalso.__init__`, que grava `tuple(intervalos or ())`: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:42-51`; binding da casca real: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:59`.

## 01/11 03:52 · sessao a060f571054d · V3-DOBRA-SEM-NOTURNA · verificador · origem=Code

**1. Os esperados estão corretos; a cobertura dos mutantes exige uma ressalva em `extra_min`.** O texto exato dos mutantes da volta 1 não foi fornecido: sua identidade literal está [não verificado].

As contas abaixo usam o cadastro explícito do teste: intervalo de 60 minutos, previsto de 480, prorrogação após 05h ligada e hora reduzida preservada (`app/ponto/tests/test_v3_dobra_sem_noturna.py:40`, `app/ponto/tests/test_v3_dobra_sem_noturna.py:46`).

| Caso | Conta pela regra declarada | Esperados |
|---|---|---|
| 22h–06h | Trabalho: 480−60=420 min. Art.73 + Súm.60 II: noite 22h–06h menos intervalo =420 min; hora reduzida: 420÷52,5=8 h. A dobra coloca os 420 min em HE100; todos são noturnos. | HE100=HE100-feriado=7 h; HE100-noturna=7 h; noturnas legais=8 h; HE50 e HE50-noturna=0. Corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:12`, `:64`, `:74`). |
| Gêmeo sem dobra, 22h–06h | Mesmo trabalho e mesma noite; 420<480, portanto excedente zero. | Noturnas=8 h; HE100 e HE100-noturna=0. Corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:78`, `:81`). |
| 21h–07h | Trabalho: 600−60=540 min. Excedente: 540−480=60 min. Noite prorrogada: 22h–07h=540, menos intervalo=480 min. Hora reduzida: 480÷52,5=9,142857 h, ou 548,571429 min legais. | HE100=HE100-feriado=9 h; HE50=0; HE100-noturna=8 h; HE50-noturna=0; `extra_min`=60. Gêmeo sem dobra: HE50=1 h. Todos corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:88`, `:92`, `:96`, `:99`, `:102`). |
| Entrada 22h20, marco 22h | Trabalho: 460−60=400 min. L-084: desvios 20 e 0, portanto não recusa os marcos. Tolerância da casa: 20>10, cobra 20 integralmente. L-093: teto 480−400=80, não corta. | Atraso=20÷60 h; antecipada=0. Corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:108`, `:111`, `:114`; `app/ponto/motor_calculo_v2.py:1934`, `:1994`, `:1176`). |
| Saída 05h30, marco 06h | Trabalho: 450−60=390 min. L-084: desvios 0 e 30, portanto julga. Tolerância: 30>10, cobra 30 integralmente. L-093: teto 480−390=90, não corta. | Antecipada=30÷60=0,5 h; atraso=0. Corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:119`, `:122`, `:126`; `app/ponto/motor_calculo_v2.py:1934`, `:1996`, `:1176`). |

A Súm.264 aparece no cadastro como **split em horas de relógio**, subconjunto da HE; o fator reduzido é aplicado à rubrica `horas_noturnas`. Por isso 8 h de HE100-noturna e 9,142857 h noturnas legais no caso longo são compatíveis (`app/ponto/motor_calculo_v2.py:342`, `app/ponto/calculador/regras.py:835`, `app/ponto/calculador/regras.py:88`).

Sobre os três reforços:

- **HE100-feriado=9:** detecta um mutante que altere essa parcela no caso com excedente, mantendo eventualmente HE100 total correta (`app/ponto/tests/test_v3_dobra_sem_noturna.py:93`).
- **`extra_min`=60:** detecta resultado final errado, mas **não mata uma mutação restrita à atribuição do ramo de dobra**: a escrita em `regras.py:787` agora é sobrescrita pela escrita em `regras.py:813`. Esse mutante sobrevive sem alterar a saída observável (`app/ponto/tests/test_v3_dobra_sem_noturna.py:96`; `app/ponto/calculador/regras.py:787`, `:813`).
- **Antecipada de 30 minutos:** detecta zeramento apenas dessa rubrica na dobra; o gêmeo precisa valer 0,5 h e a dobra precisa igualá-lo (`app/ponto/tests/test_v3_dobra_sem_noturna.py:122`, `:123`).

**2. As triagens estão corretas quanto à origem, mas “preexistente” não basta para justificar enfileiramento pelas regras locais.** `CLAUDE.md:30` determina curar bug comprovado no caminho, sem enfileirar.

- **Tolerância:** comentário corrigido para os 10 minutos efetivamente usados; não altera os esperados de 20 e 30 minutos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:106`; `app/ponto/motor_calculo_v2.py:60`, `:1994`).
- **Intervalos:** origem preexistente em `alimentacao.py:514`, mas o diff passa a expô-la também no split da dobra. Para o mesmo caso longo, omitindo o intervalo, o recuo de 540 minutos desde 07h chega a 22h e devolve **9 h**, contra **8 h** com o intervalo informado. Portanto, não foi criado pelo diff, mas compromete o resultado desse caminho integrado (`app/ponto/calculador/alimentacao.py:514`; `app/ponto/calculador/regras.py:828`; `app/ponto/motor_calculo_v2.py:395`, `:440`).
- **Crédito:** a substituição já existia. O crédito é abatido em `dia_pago.py:190`, mas a rubrica do calculador substitui o valor em `:328`; a regra do dia deliberadamente não abate crédito. Após a cura, chega atraso bruto onde antes chegava zero. Origem preexistente; descartá-lo apenas por isso contraria a regra local acima (`app/ponto/services/dia_pago.py:190`, `:328`; `app/ponto/calculador/regras.py:184`, `:855`).
- **Folga-feriado:** triagem correta. Esse ramo afirma HE100/feriado, declara os splits em `nao_decide` e retorna antes do trecho alterado; não é o mesmo zero afirmado (`app/ponto/calculador/regras.py:682`, `:704`, `:709`, `:735`).

**3. Escritores e leitores depois do diff:**

| Campo | Escrita | Leitura |
|---|---|---|
| `horas_saida_antecipada` | Alimentador passa pontualidade; regra aplica tolerância/teto e grava horas em `valores` (`app/ponto/calculador/alimentacao.py:496`, `:518`; `app/ponto/calculador/regras.py:264`, `:271`, `:856`). | Testes (`app/ponto/tests/test_v3_dobra_sem_noturna.py:122`); diagnóstico/comparação (`app/ponto/management/commands/diff_calculador.py:476`, `:501`, `:535`); lavratura recebe as rubricas e substitui o campo do `DiaPago` (`app/ponto/services/fechamento.py:1256`; `app/ponto/services/dia_pago.py:322`, `:328`, `:343`). A apresentação lê a linha lavrada em `app/ponto/services/dia_pago.py:916`. |
| `detalhe['extra_min']` | Excedente calculado em `regras.py:769`, sujeito à tolerância em `:770`; escrito na dobra em `:787` e novamente em `:813` (`app/ponto/calculador/regras.py:769`, `:787`, `:813`). | Assert direto (`app/ponto/tests/test_v3_dobra_sem_noturna.py:96`); diagnóstico HE50 (`app/ponto/management/commands/diff_calculador.py:497`). **Esse leitor explícito não executa na dobra**, pois exige HE50>0, enquanto a dobra fixa HE50=0 (`app/ponto/management/commands/diff_calculador.py:480`; `app/ponto/calculador/regras.py:786`). O detalhe inteiro ainda pode aparecer na impressão de divergências (`app/ponto/management/commands/diff_calculador.py:546`). |

A lavratura não copia `Rubricas.detalhe`: copia valores e registra nomes das recusas. Logo, `extra_min` não é persistido por esse bloco (`app/ponto/services/dia_pago.py:322`, `:329`).

**4. Não identifiquei outro defeito funcional nas linhas alteradas:** o `elif` preserva a dobra e libera split, noturno e pontualidade. A ressalva nova de cobertura é a atribuição intermediária de `extra_min` mascarada pela escrita posterior (`app/ponto/calculador/regras.py:795`, `:813`, `:820`, `:840`, `:851`).

Execução da suíte, morte dos mutantes originais e impacto em produção: **[não verificado]**. [nao verificado]

## 01/11 03:50 · sessao 66a13e78b458 · - · - · origem=Ronald (tmux)

Fluxo após diff, com referências locais:

- `horas_saida_antecipada`: `do_dia` inicializa zero apenas se decidível (`app/ponto/calculador/regras.py:641`); a dobra continua adiante (`:779-795`); `pontualidade_do_dia` aplica tolerância (`:264`), teto (`:271-272`) e soma minutos (`:274`); `do_dia` grava horas em `valores` (`:856`) e minutos no detalhe (`:858`). Sem dados/lei, declara `nao_decide` (`:619-625`); sem `dia`, remove o valor (`:863-868`).
- Alimentação: organiza pontualidade (`app/ponto/calculador/alimentacao.py:496-500`) e passa `dia`, marcos e pares para `do_dia` (`:501-519`).
- Leitura para gravar: fechamento monta rubricas (`app/ponto/services/fechamento.py:1239-1244`) e entrega a `lavrar` (`:1245-1264`). A lavratura primeiro carrega antecipada do motor (`app/ponto/services/dia_pago.py:185`), depois lê genericamente `valores` e substitui pelo calculador (`:320-328`), construindo `DiaPago` (`:343-345`). Não há `do_motor` nessa chamada (`app/ponto/services/fechamento.py:1245-1264`).
- Leitura no diff: imprime antecipada explicitamente (`app/ponto/management/commands/diff_calculador.py:474-479`), põe `valores` em `c_dia` (`:501`) e compara cada rubrica decidida (`:527-542`). Os testes leem antecipada em `app/ponto/tests/test_v3_dobra_sem_noturna.py:114` e `:122-124`.
- Leitor de apresentação: `rubricas_da_linha` lê `linha.horas_saida_antecipada` (`app/ponto/services/dia_pago.py:916`). Atingimento efetivo dessa versão pelo leitor em produção: [não verificado].

`detalhe['extra_min']`:

- A fórmula é excedente, `trab - limite`, sujeito à tolerância (`app/ponto/calculador/regras.py:767-771`). A dobra escreve o detalhe em `:787`; depois do diff, `:813` escreve novamente **o mesmo valor**. Para o caso longo: 540 − 480 = 60 (`app/ponto/tests/test_v3_dobra_sem_noturna.py:86-96`).
- Leitor explícito: `app/ponto/management/commands/diff_calculador.py:497`, mas está sob `--abrir-he50` **e HE50 > 0** (`:480`). Como a dobra fixa HE50=0 (`app/ponto/calculador/regras.py:786`), esse leitor explícito **não executa para dobra**.
- Na dobra, o detalhe inteiro ainda pode ser impresso quando há divergência e `--colab` (`app/ponto/management/commands/diff_calculador.py:543-548`).
- O teste lê diretamente a chave (`app/ponto/tests/test_v3_dobra_sem_noturna.py:96`). `dia_pago.lavrar` não copia `Rubricas.detalhe`; copia valores e nomes de recusas (`app/ponto/services/dia_pago.py:320-331`).

Nuance dos mutantes: o assert `extra_min == 60` verifica o resultado final, mas **não mata um mutante restrito à atribuição em `regras.py:787`**, porque `:813` a sobrescreve. Se o mutante altera a variável `extra` ou a escrita final, o assert pode mordê-lo; quais eram exatamente os mutantes da volta 1: [não verificado].

## 01/11 03:50 · sessao aca67ccff62d · - · - · origem=Ronald (tmux)

A triagem técnica dos três defeitos está correta quanto à origem:

- **Intervalos:** `alimentacao.py:514` já passa `intervalos=()`. O diff torna o split alcançável na dobra (`regras.py:779`, `regras.py:820`), expondo essa alimentação defeituosa também nesse ramo. No caso longo, sem intervalo informado, o recuo de 540 minutos desde 07:00 chega a 22:00: 540 minutos noturnos = **9 h**. Com intervalo, recua até 21:00; janela 22–07 menos 02–03 = 480 minutos = **8 h** (`motor_calculo_v2.py:378`, `motor_calculo_v2.py:440`, `regras.py:828`). É origem preexistente, embora agora observável na dobra (`app/ponto/calculador/alimentacao.py:514`).

- **Crédito de atraso:** o abatimento ocorre em `dia_pago.py:190`, seguido pela substituição da rubrica em `dia_pago.py:328`; a regra do dia expressamente não abate crédito (`regras.py:184`). Antes, o retorno antigo já entregava atraso zero afirmado; agora entrega atraso bruto, portanto o problema permanece e passa a aparecer com esse valor (`regras.py:641`, `regras.py:855`; retorno antigo: anexo `cura2.diff`). O overwrite não foi criado pelo diff (`app/ponto/services/dia_pago.py:328`).

- **Folga-feriado:** paga HE100 e HE100-feriado em `regras.py:682`, declara os splits em `nao_decide` em `regras.py:704`–`regras.py:711` e retorna antes do ramo alterado (`regras.py:735`). Portanto, não apresenta o mesmo zero afirmado nem é alcançado pela cura (`app/ponto/calculador/regras.py:735`).

Há uma distinção de governança: a regra local manda curar bug comprovado no caminho, mesmo preexistente, e proíbe enfileirá-lo (`CLAUDE.md:30`). Logo, “não foi introduzido pelo diff” é correto; “pode necessariamente ficar para fila” não decorre disso (`CLAUDE.md:30`). Nesta revisão, prevalece a instrução expressa do usuário de somente leitura [não verificado: instrução sem arquivo].

Não identifiquei defeito novo nas linhas alteradas: o `elif` preserva a substituição da HE normal pela dobra e permite executar os blocos independentes de split, noturno e pontualidade (`app/ponto/calculador/regras.py:779`, `app/ponto/calculador/regras.py:795`, `app/ponto/calculador/regras.py:820`, `app/ponto/calculador/regras.py:840`, `app/ponto/calculador/regras.py:851`).

## 01/11 03:42 · sessao 5e70cae2048d · - · - · origem=Ronald (tmux)

**O `if/elif` corrige o fluxo local, mas a cura ainda deixa uma divergência na porta real:** no caso 21h–07h, o teste calcula **8h de HE100 noturna**, enquanto a alimentação existente, que entrega `intervalos=()`, produz **9h**. O teste novo contorna essa alimentação. (`app/ponto/calculador/alimentacao.py:96`, `app/ponto/calculador/alimentacao.py:514`; `app/ponto/motor_calculo_v2.py:440`; anexo `teste_novo.py:46`.)

As referências abaixo usam as linhas da **base**, antes do diff; as linhas do teste anexo são contadas desde seu cabeçalho. (`app/ponto/calculador/regras.py:789`; anexo `teste_novo.py:1`.)

**1. Resultado final de `do_dia` com `_dobra > 0`.**

`_dobra > 0` não determina sozinho noturnas e pontualidade: elas dependem dos respectivos insumos e das recusas montadas antes de `val`. Rubrica recusada fica **ausente de `valores`**, com causa em `nao_decide`. (`app/ponto/calculador/regras.py:602`, `app/ponto/calculador/regras.py:609`, `app/ponto/calculador/regras.py:618`, `app/ponto/calculador/regras.py:627`, `app/ponto/calculador/regras.py:641`.)

Na tabela, `T` é o trabalhado; `S`, o intervalo suprimido após piso/cadastro/tolerância; `N`, os minutos noturnos legais; `H50n/H100n`, as somas retornadas por `he_noturna`; `A/B`, atraso/antecipada após o julgamento e teto. (`app/ponto/calculador/regras.py:600`, `app/ponto/calculador/regras.py:756`, `app/ponto/calculador/regras.py:823`, `app/ponto/calculador/regras.py:836`, `app/ponto/calculador/regras.py:847`.)

| Rubrica de `RUBRICAS` | Resultado geral após o diff | Caso anexo 22h–06h, dobra |
|---|---|---|
| `horas_trabalhadas` | `T/60` | **7h** |
| `horas_intra_indenizada` | `S/60` | **0** |
| `horas_extras_50` | **0** | **0** |
| `horas_extras_100` | `_dobra/60 = T/60` | **7h** |
| `horas_extras_100_feriado` | `_dobra/60` | **7h** |
| `horas_extras_50_noturna` | `H50n/60`, ou `nao_decide` sem pontas/prorrogação | **0** |
| `horas_extras_100_noturna` | `H100n/60`, ou a mesma recusa | **7h** |
| `horas_folga_trabalhada` | **0** neste caminho; `nao_decide` se `escala_certa=None` | **0** |
| `horas_noturnas` | `N/60`, ou `nao_decide` sem pares/CCT completa | **8h** |
| `horas_atraso` | `A/60`, ou `nao_decide` por lei/insumo ausente | **`nao_decide`** |
| `horas_saida_antecipada` | `B/60`, ou a mesma recusa | **`nao_decide`** |
| `horas_falta` | `nao_decide`: fonte Ausência | `nao_decide` |
| `horas_reflexo_dsr` | `nao_decide`: apuração mensal | `nao_decide` |
| `saldo_banco_horas` | `nao_decide`: apuração mensal | `nao_decide` |

Fontes da tabela: enumeração e recusas permanentes em `app/ponto/calculador/regras.py:400`, `app/ponto/calculador/regras.py:426`; valores e guardas em `app/ponto/calculador/regras.py:609`, `app/ponto/calculador/regras.py:740`, `app/ponto/calculador/regras.py:763`, `app/ponto/calculador/regras.py:779`, `app/ponto/calculador/regras.py:815`, `app/ponto/calculador/regras.py:835`, `app/ponto/calculador/regras.py:846`. O caso padrão do teste não fornece pontualidade; somente o último teste a fornece. (Anexo `teste_novo.py:40`, `teste_novo.py:55`, `teste_novo.py:106`.)

**Não há sobrescrita indevida de HE50/HE100/HE100_feriado abaixo do antigo retorno:** o `elif` impede executar o excedente na dobra; os blocos seguintes escrevem splits, noturno e pontualidade. `det['extra_min']` é sobrescrito em `:808`, mas pela **mesma expressão** de `:787`: continua representando o excedente, não a jornada dobrada — **0** no caso padrão e **60min** no caso longo. (`app/ponto/calculador/regras.py:786`, `app/ponto/calculador/regras.py:787`, `app/ponto/calculador/regras.py:790`, `app/ponto/calculador/regras.py:808`, `app/ponto/calculador/regras.py:815`.)

Há, porém, erro alcançado no **split** quando os intervalos não chegam: o bloco usa `he_pontas` como recebido, sem reconstruir pausas nem obrigar HE50 noturna a zero por reconhecer dobra. A coerência dessas pontas é responsabilidade da alimentação. (`app/ponto/calculador/regras.py:818`, `app/ponto/calculador/regras.py:823`; `app/ponto/calculador/alimentacao.py:91`.)

**2. Quem escreve e quem lê os campos alterados.**

| Campo | Escritor agora alcançado na dobra | Fonte da regra |
|---|---|---|
| `horas_noturnas` | `do_dia`, legais/60 | `noturnas_do_dia`, janela e desconto do motor |
| `horas_extras_50_noturna` e `horas_extras_100_noturna` | `do_dia`, soma por período/60 | `motor_calculo_v2.he_noturna` |
| `horas_atraso` e `horas_saida_antecipada` | `do_dia`, minutos/60 | `pontualidade_do_dia`, métodos emprestados do motor |
| `noturno_relogio_min`, `noturno_legais_min` | `do_dia`, valores arredondados no detalhe | Resultado de `noturnas_do_dia` |
| `he_pontas_periodos` | `do_dia`, quantidade de pontas | `len(he_pontas)` |
| `atraso_min`, `saida_antecipada_min` | `do_dia`, minutos no detalhe | Resultado de pontualidade |
| `cadastro_x_realidade` | `do_dia`, somente se houver acusações | Lista produzida pela tolerância/L-084 |

Escritas: `app/ponto/calculador/regras.py:828`, `app/ponto/calculador/regras.py:831`, `app/ponto/calculador/regras.py:840`, `app/ponto/calculador/regras.py:850`, `app/ponto/calculador/regras.py:857`. Fontes: `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:112`, `app/ponto/calculador/regras.py:264`, `app/ponto/calculador/regras.py:271`, `app/ponto/calculador/regras.py:823`.

Os consumidores relevantes são:

- **`dia_pago.lavrar`:** primeiro monta valores dos períodos do motor; depois substitui os campos presentes em `Rubricas.valores`, exceto os explicitamente reservados em `do_motor`. Grava pela mesma porta `delete/bulk_create`. (`app/ponto/services/dia_pago.py:173`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:327`, `app/ponto/services/dia_pago.py:357`.)
- **Detalhes não persistem automaticamente:** `lavrar` lê `valores` e os nomes de `nao_decide`, mas **não copia `Rubricas.detalhe`**. Assim, esses novos `noturno_*`, `he_pontas_periodos`, `atraso_min` e `cadastro_x_realidade` não chegam à linha por essa passagem. (`app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:329`, `app/ponto/services/dia_pago.py:339`.)
- **`fechamento`:** o fechamento principal continua recebendo noturnas/splits e pontualidade do resultado do motor. O calculador alimenta a lavra **`versao='oraculo'`**, desde competência 10/2026. (`app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:969`, `app/ponto/services/fechamento.py:1233`, `app/ponto/services/fechamento.py:1256`, `app/ponto/services/fechamento.py:1262`.)
- **`diff_calculador`:** lê valores, exclui rubricas declaradas cegas da comparação e imprime o detalhe inteiro no diagnóstico por colaborador divergente. Também lê especificamente `extra_min` para explicar HE50. (`app/ponto/management/commands/diff_calculador.py:497`, `app/ponto/management/commands/diff_calculador.py:501`, `app/ponto/management/commands/diff_calculador.py:517`, `app/ponto/management/commands/diff_calculador.py:532`, `app/ponto/management/commands/diff_calculador.py:546`.)
- **`porta_export` e testemunhas:** a soma mensal chamada pela porta usa o padrão **`motor`**, comparando seus campos com o fechamento; a testemunha diária citada compara horas trabalhadas, sem conferir individualmente os cinco campos novos da dobra. Portanto, esses contadores não demonstram a correção da versão `oraculo`. (`app/ponto/services/dia_pago.py:548`, `app/folha/porta_export.py:388`, `app/folha/porta_export.py:451`, `app/folha/porta_export.py:101`, `app/folha/porta_export.py:115`.)
- **Tela/papel:** `rubricas_do_dia` lê noturnas, atraso e antecipada da linha lavrada. A lista de produção CADASTRO × REALIDADE lê **`resultado.dias_cadastro_x_realidade` do motor**, não o novo detalhe da `Rubricas`. (`app/ponto/services/dia_pago.py:915`, `app/ponto/services/dia_pago.py:922`; `app/ponto/services/espelho.py:745`, `app/ponto/services/espelho.py:752`.)

**Outro problema na ordem das escritas:** `lavrar` abate crédito parcial do atraso primeiro e depois pode substituir esse resultado pelo atraso bruto do calculador; `pontualidade_do_dia` deliberadamente não abate crédito. Com atraso de 20min e crédito de 20min no mesmo dia, essa sequência pode lavrar novamente **20min no oráculo**, após ter calculado zero. O teste novo não fornece crédito; ocorrência em produção **[nao verificado]**. (`app/ponto/services/dia_pago.py:190`, `app/ponto/services/dia_pago.py:193`, `app/ponto/services/dia_pago.py:328`; `app/ponto/calculador/regras.py:184`; anexo `teste_novo.py:106`.)

**3. Checklist das classes de reprovação.**

| Classe | Avaliação do diff |
|---|---|
| **(a) Segundo juiz** | **Não introduz.** Continua usando a tolerância registrada e a autoridade de dobra existente; os métodos são importados/emprestados. (`app/core/juizes.py:198`, `app/core/juizes.py:277`; `app/ponto/calculador/regras.py:112`, `app/ponto/calculador/regras.py:290`.) |
| **(b) Segundo escritor fora da porta** | **Não introduz persistência nova.** A montagem continua em `do_dia`; a gravação continua em `dia_pago.lavrar`. Há a sobrescrita do crédito descrita acima, dentro dessa porta. (`app/ponto/calculador/regras.py:865`; `app/ponto/services/dia_pago.py:328`, `app/ponto/services/dia_pago.py:357`.) |
| **(c) Relógio fora da borda** | **Não identificado no diff/teste.** `do_dia` recebe os instantes; o teste constrói setembro/2026 fixo. `make_aware` não consulta “agora”, embora dependa do fuso ativo. (`app/ponto/calculador/regras.py:572`; anexo `teste_novo.py:33`, `teste_novo.py:37`; lei em `app/docs/CORTES.md:369`.) |
| **(d) Derivação paralela** | **Não acrescenta fórmula paralela.** Reutiliza os três blocos existentes. Contudo, a porta não entrega os mesmos intervalos usados na chamada do teste. (`app/ponto/calculador/regras.py:823`, `app/ponto/calculador/regras.py:836`, `app/ponto/calculador/regras.py:847`; `app/ponto/calculador/alimentacao.py:514`.) |
| **(e) RED sem mordida** | **Morde o retorno antigo e a troca indevida de `elif` por `if`.** Não cobre toda a integração nem todos os campos; mutantes abaixo. (`app/ponto/calculador/regras.py:789`, `app/ponto/calculador/regras.py:791`; anexo `teste_novo.py:68`, `teste_novo.py:74`, `teste_novo.py:93`, `teste_novo.py:110`.) |
| **(f) Ordem de trava** | **Não altera aquisição de locks.** Preserva as recusas antes de inicializar valores e mantém dobra antes do excedente. A ordem problemática de crédito/substituição está na lavra existente. (`app/ponto/calculador/regras.py:618`, `app/ponto/calculador/regras.py:641`, `app/ponto/calculador/regras.py:779`; `app/ponto/services/dia_pago.py:193`, `app/ponto/services/dia_pago.py:328`.) |
| **(g) Pouso antes do pré-requisito** | Suite inteira verde, árvore certificada e leitura final exigidas; comprovação desses atos **[nao verificado]**. Este laudo não certifica pouso. (`CLAUDE.md:505`, `CLAUDE.md:557`, `CLAUDE.md:560`.) | [nao verificado]

Mutantes que **o teste novo não pega**: [nao verificado]

- Trocar a escrita comum por `det['extra_min'] = 0` — nenhum teste afirma esse detalhe. (`app/ponto/calculador/regras.py:808`; anexo `teste_novo.py:62`, `teste_novo.py:86`, `teste_novo.py:102`.)
- Fazer `horas_saida_antecipada` sempre zero — o único caso de pontualidade termina no marco e exige justamente zero. (`app/ponto/calculador/regras.py:851`; anexo `teste_novo.py:106`, `teste_novo.py:112`.)
- Corromper `horas_extras_100_feriado` somente quando houver excedente — o teste longo não verifica essa rubrica. (`app/ponto/calculador/regras.py:785`; anexo `teste_novo.py:92`.)
- Manter a perda dos intervalos na alimentação — o teste fornece o intervalo diretamente a `do_dia`. (`app/ponto/calculador/alimentacao.py:514`; anexo `teste_novo.py:46`.)

RED/GREEN efetivamente executados e suite completa: **[nao verificado]**. [nao verificado]

**4. Contas dos números esperados.**

Para as chamadas isoladas do anexo, os números conferem com a REGRA fornecida e com as funções: janela prorrogada, desconto do intervalo, fator somente nas noturnas legais e split da HE em minutos de relógio. (`app/ponto/motor_calculo_v2.py:46`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:448`; `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:88`.)

| Caso | Trabalhado | Noturno de relógio | Noturnas legais | HE100 / feriado | HE100 noturna |
|---|---:|---:|---:|---:|---:|
| 22h–06h; pausa 02h–03h | `480−60=420min` | `420+60−60=420min` | `420×60/52,5÷60 = 8h` | **7h / 7h** | **7h** |
| 21h–07h; mesma pausa | `600−60=540min` | `420+120−60=480min` | `480×60/52,5÷60 = 9,142857h` | **9h / 9h** | **8h** |
| 22h20–06h; mesma pausa | `460−60=400min` | `400min` | **7,619048h** | **6,666667h / 6,666667h** | **6,666667h** |

Fontes: cadastro do anexo `teste_novo.py:40`, `teste_novo.py:42`, `teste_novo.py:43`; dobra integral em `app/ponto/calculador/regras.py:330`, `app/ponto/calculador/regras.py:347`; contas noturnas e split em `app/ponto/calculador/regras.py:85`, `app/ponto/motor_calculo_v2.py:440`.

O recuo por trabalho explica o split: no primeiro caso, recua **180min de 06h até 03h**, pula a pausa e recua **240min até 22h**; no segundo, recua **240min até 03h**, pula a pausa e recua **300min até 21h**. (`app/ponto/motor_calculo_v2.py:392`, `app/ponto/motor_calculo_v2.py:400`, `app/ponto/motor_calculo_v2.py:404`.)

**Na porta real**, o segundo recuo recebe pausa vazia: `07h−540min=22h`; a janela 22h–07h produz **540min = 9h de HE100 noturna**. Isso demonstra a divergência com as 8h do teste; ocorrência desse cadastro/batidas em produção **[nao verificado]**. (`app/ponto/calculador/alimentacao.py:96`, `app/ponto/calculador/alimentacao.py:514`; `app/ponto/motor_calculo_v2.py:405`, `app/ponto/motor_calculo_v2.py:441`.)

No atraso, `22h20−22h=20min`; saída no marco dá zero; teto `480−400=80min`, portanto preserva os vinte minutos: **atraso = 1/3h**. (`app/ponto/motor_calculo_v2.py:1961`, `app/ponto/motor_calculo_v2.py:1994`; `app/ponto/calculador/regras.py:264`, `app/ponto/calculador/regras.py:271`; anexo `teste_novo.py:106`.)

**A justificativa de tolerância precisa ser corrigida:** o comentário do teste fala em 5min do art.58, mas o código aplica **10min por marcação**, por mandato de negócio explicitamente documentado como dobro do legal. O caso de 20min passa nos dois critérios; não distingue 5 de 10. (`app/ponto/motor_calculo_v2.py:52`, `app/ponto/motor_calculo_v2.py:60`, `app/ponto/motor_calculo_v2.py:1994`; anexo `teste_novo.py:104`.)

**5. Outros retornos e testes antigos.** [nao verificado]

Há um irmão relevante no próprio `do_dia`: **folga que é feriado**, com `_hora_feriado=True`. Esse subramo escreve HE100 e HE100_feriado, mantém os splits noturnos declarados em `nao_decide` e retorna antes do split comum. O comentário “se o pai não viaja” não descreve esse subramo, pois ali o pai HE100 viaja. (`app/ponto/calculador/regras.py:680`, `app/ponto/calculador/regras.py:682`, `app/ponto/calculador/regras.py:700`, `app/ponto/calculador/regras.py:704`, `app/ponto/calculador/regras.py:735`.)

Nesse retorno, **noturno geral já é calculado**; pontualidade fica excluída expressamente por ser folga sem marco. Há ainda o retorno da folga sem os insumos exigidos. Portanto, não é uma repetição exata dos cinco zeros afirmados da dobra. (`app/ponto/calculador/regras.py:697`, `app/ponto/calculador/regras.py:712`, `app/ponto/calculador/regras.py:737`.)

Em `periodo.py`, o retorno cedo de `periodo_do_dia` atende entrada/saída ausente, declarando turno aberto; não é um ramo de dobra que interrompe rubricas. (`app/ponto/calculador/periodo.py:351`.)

**Não encontrei teste que exigisse os cinco zeros antigos.** O teste de dobra existente verifica HE100, HE50 e a flag; o teste de feriado/split usa par diurno e verifica as metades da HE100. Ausência absoluta de qualquer outro teste desse tipo: **[nao verificado]**. (`app/ponto/tests/test_s5b_regra_pontualidade.py:309`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:207`.)

Há, contudo, **registro explícito do defeito no teste P5**: ele explica que retirou o feriado da fixture porque o retorno antecipado zerava noturnas/split e estava fora daquele aval. O teste passou a usar dia comum e cobrar noturnas nele. Isso é contorno documentado da lacuna. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:25`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:81`.)

**6. Lei que exclua pontualidade no feriado trabalhado em dobra.**

**Não localizei essa exclusão escrita.** As leis aplicáveis encontradas são L-084, julgamento contra marcos válidos, e L-093, teto pelo tempo efetivamente faltante. As exclusões explícitas encontradas são por família sem marcos e por folga trabalhada; elas não autorizam excluir todo feriado em dobra. Inexistência absoluta em todo o acervo: **[nao verificado]**. (`app/docs/LEIS.md:116`, `app/docs/LEIS.md:111`; `app/ponto/calculador/regras.py:487`, `app/ponto/calculador/regras.py:189`.)

Os quatro ramos de dobra do motor alteram HE/alertas, sem estabelecer isenção de pontualidade; no comercial, o fluxo segue ao split e ao julgamento de pontualidade. (`app/ponto/motor_calculo_v2.py:2797`, `app/ponto/motor_calculo_v2.py:3080`, `app/ponto/motor_calculo_v2.py:3243`, `app/ponto/motor_calculo_v2.py:3325`, `app/ponto/motor_calculo_v2.py:3269`, `app/ponto/motor_calculo_v2.py:3281`.)

## 01/11 03:40 · sessao 4e5de46601be · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Números do teste novo conferem no helper isolado.** `noturnas_do_dia` soma janela, desconta intervalo e aplica fator importado: `app/ponto/calculador/regras.py:80-89`; fator `60/52,5`: `app/ponto/motor_calculo_v2.py:46`. Para 22–06: `480−60=420` trabalhados/noturnos de relógio; `420×60/52,5=480` minutos legais, **8 h**; HE100 = **7 h**, split100 noturno = **7 h**. Para 21–07: `600−60=540` trabalhados; noturno `540−60=480`; legal `548,571428…` minutos = **9,142857 h**; HE100 **9 h**, split100 noturno **8 h**. A janela prorrogada é implementada em `app/ponto/motor_calculo_v2.py:99-116`; o split não aplica fator reduzido, apenas rateia minutos noturnos: `app/ponto/motor_calculo_v2.py:440-451`.

- **O recuo precisa considerar trabalho.** No primeiro caso, `_inicio_do_trecho_extra` recua 180 minutos de 06 até 03, pula intervalo 02–03 e recua os 240 restantes até 22; no segundo, recua 240 de 07 até 03, pula intervalo e recua 300 até 21. Código: `app/ponto/motor_calculo_v2.py:392-406`.

- **Atraso de 20 minutos confere, mas a justificativa “tolerância de 5” não descreve o código vivo.** O motor usa **10 minutos por marcação**, com mandato de negócio explicitamente registrado como dobro dos limites legais 5/10: `app/ponto/motor_calculo_v2.py:52-61`; comparação efetiva `>10`: `app/ponto/motor_calculo_v2.py:1994-1997`. A docstring ainda diz cinco: `app/ponto/motor_calculo_v2.py:1940`. Neste caso, 20 ultrapassa ambos, conta integralmente, saída tem diferença zero e teto `480−400=80` não corta os vinte: `app/ponto/calculador/regras.py:264-275`; teto descrito em `app/ponto/motor_calculo_v2.py:1012-1037`. O teste novo não distingue tolerância 5 de 10 [nao verificado: anexo sem linhas locais].

- **Há irmão relevante no ramo de folga que vira HE100 de feriado.** `regras.py` escreve HE100 e HE100_feriado quando `_hora_feriado`: `app/ponto/calculador/regras.py:680-684`; mantém os splits noturnos em `nao_decide`, pois a exceção do laço protege apenas os dois pais: `app/ponto/calculador/regras.py:704-711`; retorna antes do split comum: `app/ponto/calculador/regras.py:735`, `app/ponto/calculador/regras.py:815`. O comentário “se o pai não viaja” contradiz esse subramo onde HE100 viaja: `app/ponto/calculador/regras.py:700-703`. Ali o noturno geral é calculado antes do retorno: `app/ponto/calculador/regras.py:712-718`; pontualidade é excluída expressamente por ser folga sem marco: `app/ponto/calculador/regras.py:697-699`. Outro retorno de folga sem insumos: `app/ponto/calculador/regras.py:736-737`. Os demais retornos examinados não reproduzem o formato de dobra interrompendo rubricas; `periodo_do_dia` retorna para par aberto: `app/ponto/calculador/periodo.py:351-352`.

- **Não achei teste que exija os cinco zeros antigos da dobra.** O teste existente `RegraDobraDeFeriadoTest.test_MORDE_no_do_dia_a_dobra_SUBSTITUI_a_HE50_e_nao_soma` só verifica HE100=10, HE50=0 e flag, sem pares nem marcos: `app/ponto/tests/test_s5b_regra_pontualidade.py:309-318`. `FeriadoEOutraMetadeDaHE100Test.test_MORDE_feriado_em_dobra_x_excedente_dao_METADES_DIFERENTES` usa par diurno 08–17 e verifica somente HE100/feriado: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:207-233`. Ausência global de qualquer outro assert: [nao verificado].

- **Um teste antigo já registra o defeito e o contorna explicitamente.** O P5 explica que removeu o feriado da fixture porque o retorno antecipado produzia zero afirmado e aquele arquivo estava fora do aval: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:25-28`; usa calendário vazio: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61-66`; cobra noturnas=8 no dia comum: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:81-83`. Portanto há lacuna/contorno documentado, não evidência de selo que mande preservar os zeros.

- **Mutantes que o teste novo deixa escapar:** mudar `det['extra_min']` para `_dobra` na escrita comum (`app/ponto/calculador/regras.py:808`); zerar sempre saída antecipada (`app/ponto/calculador/regras.py:851`), pois o único caso verifica saída no marco; corromper HE100_feriado apenas quando houver excedente, pois o caso longo não confere essa rubrica [nao verificado: anexo sem linhas locais]. O caso longo pega a remoção do `elif`, e o caso de atraso pega retorno após noturnas que continue pulando pontualidade [nao verificado: anexo sem linhas locais].

- **Limite de integração:** o teste fornece par inteiro e intervalo explícito; o caminho real entrega `pares=_pares_por_dia` e `intervalos=()` em `app/ponto/calculador/alimentacao.py:512-515`. Já existe comentário explicando que segmentos após 05 podem perder prorrogação por essa forma de alimentação: `app/ponto/calculador/periodo.py:501-505`. O teste prova sua chamada isolada; equivalência com uma lavratura real continua [nao verificado].

## 01/11 03:39 · sessao 2f1486725273 · - · - · origem=Ronald (tmux)

Checklist pela leitura do diff:

- **(a) Segundo juiz: não introduz.** Noturnas já usam funções do motor (`app/ponto/calculador/regras.py:33`, `:63`, `:85`); pontualidade empresta os métodos registrados de `MotorBase` (`:112`–`:115`; registro em `app/core/juizes.py:198`); dobra usa a autoridade existente (`app/core/juizes.py:277`).
- **(b) Segundo escritor fora da porta: não introduz no diff.** O patch altera o fluxo da mesma montagem de `Rubricas`, permitindo alcançar as atribuições existentes; o retorno continua em `app/ponto/calculador/regras.py:865`. Persistência e leitores foram delegados ao outro explorador. [nao verificado]
- **(c) Relógio fora da borda: não identificado.** `do_dia` recebe data, pares e parâmetros nos argumentos (`app/ponto/calculador/regras.py:572`). O teste anexo constrói datas fixas; `make_aware` não é leitura do instante atual. A lei está em `app/docs/CORTES.md:369`. Execução do teste [nao verificado].
- **(d) Derivação paralela: o diff não acrescenta conta.** Reaproveita os blocos existentes e as funções importadas (`app/ponto/calculador/regras.py:815`, `:835`, `:846`).
- **(e) RED morde, por inspeção.** O `return` antigo (`app/ponto/calculador/regras.py:789`) impede obter os 8 h noturnos, 7 h de HE100 noturna e 20 min de atraso exigidos pelo anexo. O caso com excedente de 60 min também detecta usar `if` em lugar do `elif`: escreveria HE50=1 (`:790`–`:791`). RED/GREEN efetivamente executados [nao verificado]. Há mutantes sobreviventes: escrever sempre zero em `horas_saida_antecipada` (`:851`), ou em `det['extra_min']` (`:808`); o teste novo não exige antecipada positiva nem afirma esse detalhe. [nao verificado: teste disponível apenas no anexo]
- **(f) Ordem de trava: o patch não introduz transação nem aquisição de lock; altera a continuidade entre atribuições locais** (`app/ponto/calculador/regras.py:779`, `:815`, `:846`, `:865`).
- **(g) Pouso antes do pré-requisito: não pode ser certificado.** Exige suite inteira verde, depois opus final, sem alteração posterior da árvore certificada (`CLAUDE.md:505`, `:557`–`:561`). Evidências desses atos não foram fornecidas [nao verificado]. [nao verificado]

Sobre pontualidade em feriado com dobra: **não localizei exclusão escrita por esse motivo** nas leis consultadas; a inexistência absoluta é [nao verificado]. As regras escritas aplicáveis são L-084 e L-093 (`app/docs/LEIS.md:116`, `:111`). As isenções explícitas são por classe sem marcos (`app/ponto/calculador/regras.py:487`–`:498`) e por **folga trabalhada** (`:189`–`:192`, `:697`–`:711`), que não equivale automaticamente a feriado trabalhado em dobra.

Os quatro ramos de dobra do motor escrevem HE/alertas, sem zerar pontualidade (`app/ponto/motor_calculo_v2.py:2797`, `:3080`, `:3243`, `:3325`); o comercial segue ao split e à pontualidade depois da decisão de dobra (`:3269`, `:3281`).

## 01/11 03:38 · sessao 062333bad220 · - · - · origem=Ronald (tmux)

Mapa de escritores e leitores, com linhas da base:

| Campo | Escritor após a cura | Leitores e persistência |
|---|---|---|
| `horas_noturnas` | `do_dia` chama `noturnas_do_dia` e escreve legais/60; grava `noturno_relogio_min` e `noturno_legais_min` (`app/ponto/calculador/regras.py:835-842`). Sem pares ou CCT completa, fica em `nao_decide` (`:609-611`). | `dia_pago.lavrar` substitui valor genericamente (`app/ponto/services/dia_pago.py:320-328`); DIFF compara valores e respeita cegueira (`app/ponto/management/commands/diff_calculador.py:501-535`); tela/papel leem `rubricas_do_dia` (`app/ponto/services/dia_pago.py:899-924`). |
| `horas_extras_50_noturna`, `horas_extras_100_noturna` | `do_dia` soma `he_noturna` por `he_pontas`, escreve rubricas e `he_pontas_periodos` (`app/ponto/calculador/regras.py:815-831`). Sem pontas/prorrogação ficam em `nao_decide` (`:627-632`). | Mesma substituição genérica na lavra (`app/ponto/services/dia_pago.py:320-328`) e comparação do DIFF (`app/ponto/management/commands/diff_calculador.py:501-535`). São campos de `CAMPOS_DIA`, somados na competência (`app/ponto/services/dia_pago.py:35-37`, `:548-561`). |
| `horas_atraso`, `horas_saida_antecipada` | `do_dia` chama `pontualidade_do_dia` e escreve minutos/60 (`app/ponto/calculador/regras.py:846-853`). Lei da família ou falta de insumos mantém `nao_decide` (`:618-625`); sem dia remove valores e declara (`:858-863`). | Mesma lavra e DIFF; tela/papel leem atraso/antecipada (`app/ponto/services/dia_pago.py:915-916`). O export soma as duas pela função `rubrica_atraso_parcial` (`app/folha/export.py:372-388`, `:440-441`). |
| `noturno_*`, `he_pontas_periodos`, `atraso_min`, `saida_antecipada_min`, `cadastro_x_realidade` em `Rubricas.detalhe` | `do_dia` escreve nos blocos agora alcançados (`app/ponto/calculador/regras.py:831`, `:841-842`, `:852-857`). | DIFF imprime **detalhe inteiro** para colaborador divergente (`app/ponto/management/commands/diff_calculador.py:543-548`). A lavra **não copia `_r.detalhe`**: copia valores e apenas nomes de `nao_decide` (`app/ponto/services/dia_pago.py:320-331`); o detalhe persistido vem de `L['_detalhe']` (`:339-345`). Portanto não ligar automaticamente esse novo `cadastro_x_realidade` à lista de produção. |

A alimentação real vem de `alimentacao.rubricas_por_dia`, que fornece pares, pontas de HE, flags de CCT e pontualidade (`app/ponto/calculador/alimentacao.py:501-519`). As pontas de HE carregam extras **do período do motor**, deliberadamente (`:48-54`, `:91-96`).

O fechamento principal continua escrito pelo resultado do motor: noturnas/splits em `app/ponto/services/fechamento.py:893-899`, atraso com crédito e antecipada em `:962-970`. A versão `motor` de DiaPago recebe os períodos em `:1168-1205`. O calculador só alimenta a versão `oraculo`, desde competência 10/2026, pela mesma função `lavrar` (`:1233-1264`). Não nasce escritor novo: a persistência comum continua no delete/bulk_create (`app/ponto/services/dia_pago.py:357-358`).

As testemunhas da porta continuam lendo `motor`: `soma_da_competencia` tem esse default (`app/ponto/services/dia_pago.py:548-561`); `porta_export` a chama sem versão (`app/folha/porta_export.py:388`), compara todos os campos somados com fechamento (`:451-467`), compara badge de noturnas/atraso (`:245-276`) e consulta linhas com versão literal `motor` (`:588-589`). A testemunha diária `dias_fora_do_dia_pago` mede **horas trabalhadas**, por `pago_do_dia`, sem comparação individual de noturno/splits/pontualidade (`:101-118`).

A lista de produção CADASTRO x REALIDADE é `resultado.dias_cadastro_x_realidade`, escrita pelo motor (`app/ponto/motor_calculo_v2.py:2707`), lida no espelho (`app/ponto/services/espelho.py:745-752`, `:1264`) e pela testemunha de janela de HE (`app/ponto/janela_he.py:245-254`, `:277-282`). Ela tem forma/proveniência diferente do novo `Rubricas.detalhe['cadastro_x_realidade']` (`app/ponto/calculador/regras.py:854-857`).

**Achado fora do diff que o teste novo não cobre:** a porta real entrega `intervalos=()` (`app/ponto/calculador/alimentacao.py:514`), embora `he_pontas` guarde o envelope efetivo do período (`:73`, `:91-96`). Apenas os pares comuns são segmentados descontando pausa (`:115-146`). O teste anexo passa envelope e intervalo explícito, contornando essa alimentação [nao verificado: arquivo anexo não materializado].

Consequência demonstrável para o caso 21–07/trabalho 540/pausa 02–03: pela porta real, `he_noturna` recebe x100=540 e nenhum intervalo, recua de 07 para 22 (`app/ponto/motor_calculo_v2.py:392-406`), conta 540 minutos noturnos e devolve **9 h**, enquanto com intervalo recua até 21, conta 540 noturnos e desconta 60, devolvendo **8 h** (`:440-450`; janela em `:99-118`). Isso é um problema preexistente de alimentação que a cura passa a alcançar no ramo dobra; ocorrência em produção permanece [nao verificado].

## 01/11 03:30 · sessao 49226582207f · - · - · origem=Ronald (tmux)

Censo estático da cópia disponível. Correspondência com o commit Git `40988459`: **[nao verificado]**, pois não há metadados Git disponíveis.

**1. Chamadores e origem dos argumentos**

Encontrei **um chamador direto em produção**: `rubricas_por_dia`, que importa `regras` e chama `regras.do_dia`. (`app/ponto/calculador/alimentacao.py:367`, `app/ponto/calculador/alimentacao.py:501`)

| Argumento | Valor passado e origem |
|---|---|
| `feriados` | `_feriados`, construído com `set(feriados or ())` a partir do argumento da porta. (`app/ponto/calculador/alimentacao.py:396`, `app/ponto/calculador/alimentacao.py:509`) |
| `feriado_em_dobra` | `_fer_dobra`: `_cct.feriado_em_dobra`, ou `None` quando não há CCT lida. (`app/ponto/calculador/alimentacao.py:394`, `app/ponto/calculador/alimentacao.py:509`) |
| `prorrogacao_pos5h` | `_cct_pro`: `_cct.prorrogacao_pos5h`, ou `None`. (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:515`) |
| `hora_reduzida_afastada` | `_cct_hr`: `_cct.hora_reduzida_afastada`, ou `None`. (`app/ponto/calculador/alimentacao.py:393`, `app/ponto/calculador/alimentacao.py:515`) |
| `he_pontas` | `_he_pontas.get(k)`, sendo `_he_pontas = insumos.get('he_pontas_por_dia') or {}`. (`app/ponto/calculador/alimentacao.py:444`, `app/ponto/calculador/alimentacao.py:513`) |
| `pares` | `_pares_por_dia.get(k)`, vindo de `insumos['pares_por_dia']`. (`app/ponto/calculador/alimentacao.py:438`, `app/ponto/calculador/alimentacao.py:512`) |
| `pontualidade` | `_pont`: cópia da lista diária de `pont_por_dia`, ou `None`; na primeira tupla, substitui trabalhados pelo total diário do núcleo e fora da janela pelo total diário correspondente. (`app/ponto/calculador/alimentacao.py:439`, `app/ponto/calculador/alimentacao.py:496–500`, `app/ponto/calculador/alimentacao.py:518`) |

A cadeia dos flags é `get_motor_cct(c, ini, tipo_escala=te)` → `cct_do_motor(_mcct)` → três campos da CCT. Exceção na fábrica deixa `_mcct=None`; `cct_do_motor` também devolve `None` se qualquer um dos quatro atributos exigidos estiver ausente ou nulo. Essa leitura acontece antes do laço diário. (`app/ponto/calculador/alimentacao.py:378–394`, `app/ponto/calculador/nucleo.py:317`, `app/ponto/calculador/nucleo.py:344–351`)

Na fábrica, com a régua ativa, a prorrogação vem de `regua_para`; o afastamento da hora reduzida exige a cláusula cadastrada, ciclo `12x36` e trava desativada; a dobra pode ser explicitamente ligada pela cláusula de feriado do `12x36`. Sem valor explícito de dobra, `get_motor` resolve pelo ciclo, usando `CICLOS_FERIADO_SIMPLES={'12x36'}`. (`app/core/regua_cct.py:458–479`, `app/ponto/motor_calculo_v2.py:3587`, `app/ponto/motor_calculo_v2.py:3657–3668`)

`insumos_do_calculador` monta os três mapas: segmentos do motor, marcos e pontualidade. Os pares usam `efetivo_entrada/efetivo_saida`, separados pelos intervalos descontados; a pontualidade usa entradas/saídas cruas em horário local e marcos do vínculo, ancorados no dia da jornada. (`app/ponto/calculador/alimentacao.py:302–304`, `app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:115–146`, `app/ponto/calculador/alimentacao.py:184–189`, `app/ponto/calculador/alimentacao.py:251–289`)

Há dois chamadores de produção dessa porta:

- **Fechamento:** monta insumos e chama `rubricas_por_dia`; os feriados vêm de `feriados_de(lugar_do_colab(colab), _env_ini, _env_fim, carregados=_feriados_lote)`. (`app/ponto/services/fechamento.py:621–623`, `app/ponto/services/fechamento.py:1237–1244`)
- **DIFF:** chama a mesma porta com `aut.resultado`; passa `set(aut.feriados or ())`. A autoridade obtém esse calendário por `feriados_do_colab`, que delega a `feriados_de`. (`app/ponto/management/commands/diff_calculador.py:361`, `app/ponto/management/commands/diff_calculador.py:417–424`, `app/ponto/services/espelho.py:1034`, `app/ponto/services/espelho.py:146–147`)

**2. Fluxo de `do_dia`, de `_dobra` até o retorno**

Antes desse trecho, `valores` é inicializado com **`0.0` para toda rubrica que não esteja em `_nd`**. As cinco rubricas perguntadas pertencem a `RUBRICAS`. (`app/ponto/calculador/regras.py:420–421`, `app/ponto/calculador/regras.py:641`)

As condições iniciais dessas cinco são:

| Rubrica | Quando entra em `nao_decide`, ficando ausente de `valores` |
|---|---|
| `horas_noturnas` | `pares` vazio/ausente **ou** qualquer um dos flags `prorrogacao_pos5h` e `hora_reduzida_afastada` igual a `None`. (`app/ponto/calculador/regras.py:609–611`) |
| Ambas as HE noturnas | `he_pontas is None` **ou** `prorrogacao_pos5h is None`. Lista `[]` não aciona essa condição. (`app/ponto/calculador/regras.py:627–632`) |
| `horas_atraso` e `horas_saida_antecipada` | `pontualidade_fora_por_lei` verdadeiro: recebem essa causa e `pontualidade` vira `None`; senão, `pontualidade is None`: recebem `SEM_ENTRADA`. (`app/ponto/calculador/regras.py:618–625`) |

O fluxo posterior é:

1. `_dobra` começa em `0.0`. Só chama `dobra_de_feriado_do_dia` se calendário, flag de dobra e `dia` não forem `None`; registra `dobra_feriado_min`. O helper devolve o trabalhado integral quando positivo e o predicado do motor é verdadeiro; caso contrário, zero. (`app/ponto/calculador/regras.py:774–778`, `app/ponto/calculador/regras.py:344–347`)

2. **Se `_dobra > 0`:**
   - Escreve `horas_extras_100 = _dobra/60`, a parcela de feriado quando não declarada, `horas_extras_50 = 0.0`, os detalhes da substituição e **retorna imediatamente**. (`app/ponto/calculador/regras.py:779–789`)
   - Portanto, as cinco rubricas perguntadas conservam exatamente a inicialização: **`0.0` em `valores` quando seus insumos passaram pelas guardas; ausentes de `valores` e com causa em `nao_decide` quando não passaram**. Nenhuma das cinco é calculada nesse ramo. (`app/ponto/calculador/regras.py:609–641`, `app/ponto/calculador/regras.py:779–789`)

3. **Se `_dobra == 0`:**
   - Se `extra > 0`, escreve HE50 como `min(extra,120)/60`; escreve HE100 como `max(extra-120,0)/60` somente se essa rubrica não estiver em `_nd`. Registra os detalhes do excedente; sem extra positivo, esses valores conservam a inicialização. (`app/ponto/calculador/regras.py:790–808`)
   - Se `he_pontas` e prorrogação não forem `None`, chama `he_noturna` para cada `(entrada, saída, x50, x100)`, soma os resultados e escreve cada split dividido por 60. Com `he_pontas=[]`, escreve ambos como zero. Sem os insumos, conserva as causas iniciais. O flag de hora reduzida não participa dessa guarda. (`app/ponto/calculador/regras.py:815–831`)
   - Se existem pares e ambos os flags noturnos, escreve `horas_noturnas = _legais/60` e os detalhes relógio/legais. `_legais` é o noturno dos pares válidos, descontados os intervalos, limitado inferiormente a zero, multiplicado por `1.0` ou `FATOR_HORA_NOTURNA`. Sem esses insumos, conserva `nao_decide`. (`app/ponto/calculador/regras.py:80–89`, `app/ponto/calculador/regras.py:835–842`)
   - Se `pontualidade` e `dia` não forem `None`, chama `pontualidade_do_dia` e escreve atraso/antecipada, divididos por 60; registra detalhes e eventual `cadastro_x_realidade`. (`app/ponto/calculador/regras.py:846–857`)
   - Se existe `pontualidade`, mas falta `dia`, declara ambas em `nao_decide` com a causa específica de ausência de `dia=` e remove os zeros de `valores`. Se `pontualidade=None`, conserva as causas iniciais. (`app/ponto/calculador/regras.py:618–625`, `app/ponto/calculador/regras.py:858–863`)
   - Retorna `Rubricas(val, _nd, det)`. (`app/ponto/calculador/regras.py:865`)

**3. Motor v2 no feriado em dobra**

O predicado executável é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`; não contém teste de “dia previsto na escala”. (`app/ponto/motor_calculo_v2.py:929`)

Os **quatro sítios de dobra** têm estes comportamentos:

| Sítio | Noturnas ordinárias | Split de HE noturna |
|---|---|---|
| `Motor12x36.calcular_periodo` | Calcula antes da dobra; o ramo coloca a duração integral em HE100 sem alterar as noturnas. (`app/ponto/motor_calculo_v2.py:2780`, `app/ponto/motor_calculo_v2.py:2797–2805`) | Executa `_aplicar_he_noturna` depois do ramo, inclusive na dobra. (`app/ponto/motor_calculo_v2.py:2843`) |
| `MotorTurnoPartido.calcular_mes` | Os períodos já calcularam noturnas; a reinicialização mensal não as apaga. (`app/ponto/motor_calculo_v2.py:2884`, `app/ponto/motor_calculo_v2.py:2921–2927`) | Zera ambos os splits; atribui a HE100 integral ao último período e dá `continue`, pulando `_aplicar_he_noturna`. **Ambos ficam zero nesse caminho mensal.** (`app/ponto/motor_calculo_v2.py:2925–2926`, `app/ponto/motor_calculo_v2.py:3080–3089`, `app/ponto/motor_calculo_v2.py:3135`) |
| `MotorComercial.calcular_periodo` | Calcula antes da dobra, que não altera esses campos. (`app/ponto/motor_calculo_v2.py:3235`, `app/ponto/motor_calculo_v2.py:3243–3251`) | Executa `_aplicar_he_noturna` depois do ramo. (`app/ponto/motor_calculo_v2.py:3269`) |
| `MotorComercial.calcular_mes` | Preserva as noturnas ordinárias dos períodos; reinicializa extras e seus splits. (`app/ponto/motor_calculo_v2.py:3304–3313`) | Apaga os splits individuais; atribui HE100 integral ao último período e dá `continue`, pulando a nova aplicação. **Ambos ficam zero nesse caminho mensal.** (`app/ponto/motor_calculo_v2.py:3311–3312`, `app/ponto/motor_calculo_v2.py:3325–3340`, `app/ponto/motor_calculo_v2.py:3387`) |

Quando chamado, `_aplicar_he_noturna` entrega a `he_noturna` as pontas cruas do período, x50/x100, prorrogação e intervalos. A função mede o trecho extra no fim, desconta intervalos e reparte o noturno na proporção x50/x100; não aplica fator de hora reduzida. (`app/ponto/motor_calculo_v2.py:1000–1010`, `app/ponto/motor_calculo_v2.py:430–451`)

**Atraso/saída antecipada:** nenhum dos quatro ramos de dobra escreve ou zera esses campos. O comercial julga pontualidade após a HE noturna; o `12x36ComEscala` julga após o cálculo herdado; o partido julga antes do laço de HE/dobra. (`app/ponto/motor_calculo_v2.py:2797–2805`, `app/ponto/motor_calculo_v2.py:3080–3089`, `app/ponto/motor_calculo_v2.py:3243–3251`, `app/ponto/motor_calculo_v2.py:3325–3340`, `app/ponto/motor_calculo_v2.py:3281`, `app/ponto/motor_calculo_v2.py:3510–3521`, `app/ponto/motor_calculo_v2.py:3065–3068`)

A pontualidade ainda passa pelo teto diário: guardas de ausência inteira ou dia aberto/nulo podem zerar; com previsto conhecido, a soma fica limitada a `max(0, previsto − trabalhado_real)`, com rateio proporcional. (`app/ponto/motor_calculo_v2.py:1080–1093`, `app/ponto/motor_calculo_v2.py:1144–1184`)

Existe também um bloqueio separado da dobra: `_dia_sem_previsto_fora_da_janela` pode zerar trabalho, noturnas e HE/splits em folga/feriado sem previsto e sem autorização, conforme suas demais guardas. Portanto, a preservação das noturnas descrita na tabela é o comportamento **dos ramos de dobra**, não uma garantia contra esse bloqueio posterior. (`app/ponto/motor_calculo_v2.py:2446–2449`, `app/ponto/motor_calculo_v2.py:1436–1471`)

**4. Leitores das cinco rubricas no resultado de `do_dia`**

Em produção:

- **`dia_pago._DONOS_DECLARADOS`:** lê as chaves de `valores` e `nao_decide`; as cinco estão em `CAMPOS_DIA`. (`app/ponto/services/dia_pago.py:34–37`, `app/ponto/services/dia_pago.py:61–63`)
- **`dia_pago.lavrar`:** lê os valores e substitui o campo da linha diária, salvo proveniência explícita em `do_motor`; lê `nao_decide` para registrar `calculador_nao_decide`. O fechamento entrega a saída da porta em `rubricas=_rub_o`. (`app/ponto/services/dia_pago.py:320–331`, `app/ponto/services/fechamento.py:1239–1244`, `app/ponto/services/fechamento.py:1256`)
- **`diff_calculador`:** lê atraso e antecipada explicitamente para impressão; guarda o mapa completo, lê `nao_decide`, soma os valores e compara cada rubrica não declarada cega. O conjunto comparado inclui as rubricas fora do `NAO_DECIDE` do módulo. (`app/ponto/management/commands/diff_calculador.py:78`, `app/ponto/management/commands/diff_calculador.py:476–477`, `app/ponto/management/commands/diff_calculador.py:501`, `app/ponto/management/commands/diff_calculador.py:517–542`)

Em testes, encontrei estas leituras das cinco rubricas: [nao verificado]

| Arquivo | Leituras |
|---|---|
| `app/ponto/tests/test_calculador_regras.py` | Presença/ausência e causas: `:208–219`. |
| `app/ponto/tests/test_s5b_regra_pontualidade.py` | Atraso, antecipada e noturnas: `:211–253`; folga: `:400–412`; cobertura genérica: `:436–438`, `:452–454`; discriminador de pontualidade: `:485–493`, `:538–539`, `:551`, `:558`. |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py` | HE noturna ausente/declarada: `:288–290`; valores: `:319–320`. |
| `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py` | HE50 noturna e noturnas: `:69–82`. |
| `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py` | Atraso: `:154–159`. |
| `app/ponto/tests/test_o280_bateria_da_lei.py` | Declaração de atraso/antecipada: `:2780–2781`. |

**5. O que os x50/x100 de `he_pontas` representam**

São lidos diretamente de `p.minutos_extra_50` e `p.minutos_extra_100`, com as pontas efetivas do período. Não são recalculados pelo excedente de `do_dia`. Dia visitado sem HE recebe `[]`; se existe x50 ou x100, acrescenta `(e,s,x50,x100)`. (`app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:91–96`)

**Quando o motor aplicou a dobra, esses números já refletem a dobra:** nos cálculos individuais, x100 recebe a duração integral; nos consolidados de partido/comercial, as extras dos períodos são zeradas e o último recebe todo o trabalhado do dia em x100. Assim, nos consolidados, a tupla do último período pode carregar a HE100 do dia inteiro; não há distribuição da dobra por cada período. (`app/ponto/motor_calculo_v2.py:2797–2800`, `app/ponto/motor_calculo_v2.py:3243–3246`, `app/ponto/motor_calculo_v2.py:2921–2926`, `app/ponto/motor_calculo_v2.py:3080–3089`, `app/ponto/motor_calculo_v2.py:3307–3312`, `app/ponto/motor_calculo_v2.py:3325–3340`)

Essa origem é comum ao fechamento e ao DIFF, pois ambos constroem `insumos_do_calculador` e chamam a mesma porta. (`app/ponto/services/fechamento.py:1237–1244`, `app/ponto/management/commands/diff_calculador.py:311–312`, `app/ponto/management/commands/diff_calculador.py:417–424`)

**6. Testes de `do_dia` com feriado em dobra** [nao verificado]

Encontrei estes casos que alcançam o ramo da dobra:

- **`test_MORDE_no_do_dia_a_dobra_SUBSTITUI_a_HE50_e_nao_soma`:** passa feriado e dobra explicitamente; afirma HE100=10h, HE50=0 e detalhe de substituição. Não afirma noturnas nem HE noturna. (`app/ponto/tests/test_s5b_regra_pontualidade.py:309–318`)
- **`test_MORDE_feriado_em_dobra_x_excedente_dao_METADES_DIFERENTES`:** afirma HE100 positiva, parcela de feriado igual à HE100 e distinção do excedente normal. Não afirma noturnas nem HE noturna. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:207–233`)
- **`test_C3b_flag_da_CCT_AUSENTE_nao_vira_o_default_do_leitor`:** alcança `do_dia` pela porta, injeta feriado e liga a dobra; verifica HE100, parcela de feriado e declaração quando falta cadastro. O resultado lido pelo teste não inclui noturnas ou HE noturna. (`app/ponto/tests/test_o280_bateria_da_lei.py:2858–2902`)

O teste de **folga/feriado** é outro caminho: passa `folga_e_feriado`, afirma folga=0 e HE100-feriado=8h, mas retorna antes de chegar a `_dobra`. (`app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:225–233`, `app/ponto/calculador/regras.py:735`)

**Não encontrei asserção de `horas_noturnas` ou HE noturna numa execução de `_dobra > 0`.** As asserções encontradas de split usam dia comum; o P5 também executa dia comum e apenas documenta, em comentário, o retorno precoce da dobra. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:312–320`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:25–28`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61–82`)

## 01/11 03:29 · sessao eea9e4aa971d · - · - · origem=Ronald (tmux)

Leitores do resultado de `do_dia` em produção:

- `app/ponto/services/dia_pago.py:61-63`: `_DONOS_DECLARADOS` lê as chaves de `Rubricas.valores` e `nao_decide`; as cinco rubricas pertencem a `CAMPOS_DIA` em `app/ponto/services/dia_pago.py:34-37`.
- `app/ponto/services/dia_pago.py:320-328`: `lavrar` lê os valores e os copia para a linha do dia; lê `nao_decide` para registrar o detalhe em `app/ponto/services/dia_pago.py:329-331`. A ligação com a saída real da porta está em `app/ponto/services/fechamento.py:1239-1244` e `app/ponto/services/fechamento.py:1256`.
- `app/ponto/management/commands/diff_calculador.py:476-477`: leitura explícita de saída antecipada e atraso. O comando guarda o mapa de valores em `app/ponto/management/commands/diff_calculador.py:501`, lê as rubricas cegas em `app/ponto/management/commands/diff_calculador.py:517`, soma valores em `app/ponto/management/commands/diff_calculador.py:519-520` e cegueira em `app/ponto/management/commands/diff_calculador.py:525-526`; compara valores somente das rubricas não cegas em `app/ponto/management/commands/diff_calculador.py:527-542`. A seleção inclui todas as rubricas do calculador fora de `NAO_DECIDE`: `app/ponto/management/commands/diff_calculador.py:78`.

Leitores em testes das cinco rubricas: [nao verificado]

- `test_calculador_regras.py`: ausência em `valores` e presença/causa em `nao_decide`: `app/ponto/tests/test_calculador_regras.py:208-219`.
- `test_s5b_regra_pontualidade.py`: atraso, antecipada e noturnas em `app/ponto/tests/test_s5b_regra_pontualidade.py:211-253`; noturnas de folga em `app/ponto/tests/test_s5b_regra_pontualidade.py:411-412`; atraso/antecipada de folga em `app/ponto/tests/test_s5b_regra_pontualidade.py:400-402`; cobertura de todas as rubricas em `app/ponto/tests/test_s5b_regra_pontualidade.py:436-438` e `app/ponto/tests/test_s5b_regra_pontualidade.py:452-454`; testes do discriminador de pontualidade em `app/ponto/tests/test_s5b_regra_pontualidade.py:485-493`, `app/ponto/tests/test_s5b_regra_pontualidade.py:538-539`, `app/ponto/tests/test_s5b_regra_pontualidade.py:551` e `app/ponto/tests/test_s5b_regra_pontualidade.py:558`.
- HE noturna ausente/cega em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:288-290`; HE noturna com valor em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:319-320`.
- HE50 noturna e noturnas do P5 em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:69-82`.
- Atraso em `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:154-159`; cegueira de atraso/antecipada em `app/ponto/tests/test_o280_bateria_da_lei.py:2780-2781`.

Testes que chegam ao feriado em dobra: [nao verificado]

1. `test_MORDE_no_do_dia_a_dobra_SUBSTITUI_a_HE50_e_nao_soma`: chamada explícita em `app/ponto/tests/test_s5b_regra_pontualidade.py:312-314`; afirma HE100 de 10h, HE50 zero e detalhe da substituição em `app/ponto/tests/test_s5b_regra_pontualidade.py:315-318`. Não afirma noturnas ou HE noturna nesse caso: `app/ponto/tests/test_s5b_regra_pontualidade.py:309-318`.
2. `test_MORDE_feriado_em_dobra_x_excedente_dao_METADES_DIFERENTES`: chamada explícita em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:209-214`; afirma HE100 positiva, igualdade da parcela de feriado ao total e distinção do dia normal em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:222-233`. Não afirma noturnas nesse caso: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:207-233`.
3. Indireto pela porta, `test_C3b_flag_da_CCT_AUSENTE_nao_vira_o_default_do_leitor`: passa feriado em `app/ponto/tests/test_o280_bateria_da_lei.py:2858` e `app/ponto/tests/test_o280_bateria_da_lei.py:2863`, liga a dobra em `app/ponto/tests/test_o280_bateria_da_lei.py:2875-2876` e executa em `app/ponto/tests/test_o280_bateria_da_lei.py:2883`; verifica HE100, parcela de feriado e declaração de ausência de cadastro em `app/ponto/tests/test_o280_bateria_da_lei.py:2887-2902`. Seu retorno lido não inclui noturnas: `app/ponto/tests/test_o280_bateria_da_lei.py:2864-2873`.
4. Caso relacionado, mas do ramo anterior de folga/feriado: `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:225-233`; usa `folga_e_feriado=True`, omite `feriado_em_dobra` e afirma folga zero/HE100 feriado 8h. Não é execução do ramo `_dobra > 0`, pois o ramo de folga retorna em `app/ponto/calculador/regras.py:735`.

Não encontrei asserção sobre `horas_noturnas` ou HE noturna **no ramo `_dobra > 0`**. Os testes que afirmam essas rubricas usam calendário vazio: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:315-320` e `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61-82`. O P5 documenta o retorno precoce do feriado em comentário em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:25-28`, mas executa dia comum em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66`.

## 01/11 03:29 · sessao 7d15870ce6a1 · - · - · origem=Ronald (tmux)

No arquivo `app/ponto/motor_calculo_v2.py`, `_feriado_dobra_no_dia(dia)` é exatamente `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`; não consulta “previsto na escala”, apesar dos comentários antigos em alguns sítios. `eh_feriado` consulta conjunto injetado e fallback `is_feriado`. Referências: `app/ponto/motor_calculo_v2.py:909-929`, `app/ponto/motor_calculo_v2.py:951-955`.

**Há quatro ramos que chamam `_feriado_dobra_no_dia`:**

| Sítio | Dobra | Adicional noturno ordinário | Split HE noturna |
|---|---|---|---|
| `Motor12x36.calcular_periodo` | Se duração positiva: `minutos_extra`, `minutos_extra_100`, `minutos_extra_100_feriado = duracao`; `hora_extra_100=True`. `app/ponto/motor_calculo_v2.py:2797-2805` | Calculado antes, por `_escrever_noturnas`, e não alterado pelo ramo. `app/ponto/motor_calculo_v2.py:2780`, `app/ponto/motor_calculo_v2.py:2797-2805` | `_aplicar_he_noturna(periodo)` é executado depois do if/elif/else, inclusive dobra. `app/ponto/motor_calculo_v2.py:2843` |
| `MotorTurnoPartido.calcular_mes` | HE integral do dia no último período: `minutos_extra`, `minutos_extra_100`, `minutos_extra_100_feriado=trab_dia`; flag verdadeira; `continue`. `app/ponto/motor_calculo_v2.py:3080-3089` | Cada período calcula noturnas antes, em `calcular_periodo`, e o recálculo mensal não as zera. `app/ponto/motor_calculo_v2.py:2884`, `app/ponto/motor_calculo_v2.py:2920-2927` | Mensal zera `minutos_extra_50_noturna` e `minutos_extra_100_noturna`; dobra dá `continue` antes da chamada em `:3135`. Portanto ambos permanecem zero nos `resultado.periodos` desse dia. `app/ponto/motor_calculo_v2.py:2925-2926`, `app/ponto/motor_calculo_v2.py:3089`, `app/ponto/motor_calculo_v2.py:3135`. Caminho semanal também exclui feriados: `app/ponto/motor_calculo_v2.py:3144-3146`. |
| `MotorComercial.calcular_periodo` | Duração inteira em HE100 e HE100-feriado; flag verdadeira. `app/ponto/motor_calculo_v2.py:3243-3251` | `_escrever_noturnas` antes do ramo; ramo não altera os campos. `app/ponto/motor_calculo_v2.py:3235`, `app/ponto/motor_calculo_v2.py:3243-3251` | Chama `_aplicar_he_noturna` após o ramo. `app/ponto/motor_calculo_v2.py:3269` |
| `MotorComercial.calcular_mes` | Último período recebe `trab_dia` nas três HE; flag verdadeira; `continue`. `app/ponto/motor_calculo_v2.py:3325-3340` | Mensal zera só extras e seus splits, preservando os campos noturnos ordinários calculados no período. `app/ponto/motor_calculo_v2.py:3304-3313` | O mensal apaga o split calculado individualmente (`:3311-3312`); dobra continua (`:3340`) antes de `_aplicar_he_noturna` (`:3387`). Resultado mensal desses períodos: ambos splits zero. |

**O cálculo ordinário de noturnas e o split são distintos.** `_escrever_noturnas` grava relógio e legais a partir de `calcular_noturnas`; esta calcula janela com `prorrogacao_noturna_pos5h`, desconta intervalos e multiplica pelo fator (1 se hora reduzida afastada; caso contrário `FATOR_HORA_NOTURNA`). Referências: `app/ponto/motor_calculo_v2.py:971-980`, `app/ponto/motor_calculo_v2.py:991-993`.

`_aplicar_he_noturna` passa entrada/saída brutas do período, extras 50/100, prorrogação e intervalos do motor para `he_noturna`, gravando somente resultados não zero. Referências: `app/ponto/motor_calculo_v2.py:1000-1010`. `he_noturna` recua da saída pelo total extra em minutos trabalhados, intersecta janela noturna, desconta intervalos e reparte em proporção de 50/100; não aplica fator de hora reduzida nessa função. Referências: `app/ponto/motor_calculo_v2.py:430-451`.

**Atraso/saída antecipada:** nenhum dos quatro ramos de dobra atribui/zera esses campos: somente escreve HE, flag e alerta; nos mensais o `continue` pula HE diária, depois de a pontualidade já ter sido julgada. Referências: `app/ponto/motor_calculo_v2.py:2797-2805`, `app/ponto/motor_calculo_v2.py:3080-3089`, `app/ponto/motor_calculo_v2.py:3243-3251`, `app/ponto/motor_calculo_v2.py:3325-3340`.

- Comercial chama `_julgar_pontualidade` após o cálculo da HE noturna, inclusive se passou pela dobra: `app/ponto/motor_calculo_v2.py:3269`, `app/ponto/motor_calculo_v2.py:3281`.
- `Motor12x36ComEscala` chama o cálculo do 12x36 e depois `_julgar_pontualidade`, exceto aberto ou sem escala; não há exceção feriado-dobra: `app/ponto/motor_calculo_v2.py:3510-3521`.
- Partido julga a Zona 5 antes do laço de HE/dobra, por `aplicar_tolerancia` e grava as duas rubricas: `app/ponto/motor_calculo_v2.py:3012-3023`, `app/ponto/motor_calculo_v2.py:3061-3068`.
- `_julgar_pontualidade` retorna sem escala/marco; havendo marcos, usa `aplicar_tolerancia` e grava atraso/antecipada. Referências: `app/ponto/motor_calculo_v2.py:2030-2055`, `app/ponto/motor_calculo_v2.py:2205-2214`.
- Teto mensal geral: ausência inteira zera (`:1080-1093`), dia aberto/nulo zera (`:1144-1157`), previsto inexistente (`None`) deixa como está (`:1158-1160`); caso contrário limita soma a `max(0, previsto - trabalhado_real)` e rateia proporcionalmente (`:1161-1184`). Chamadas: base `:2706`, partido `:3170`, comercial `:3472`, todas em `app/ponto/motor_calculo_v2.py`.

**Ressalva necessária para a frase “motor mantém noturnas”:** os ramos de dobra por si mantêm, mas existe bloqueio independente depois do cálculo do período. Base chama `_dia_sem_previsto_fora_da_janela` em `app/ponto/motor_calculo_v2.py:2446-2449`. Em folga ou feriado, se empresa tem janela, previsto é zero, HE não está autorizada e passam as guardas de escala certa, o método zera trabalhadas, noturnas e HE/splits, além de colapsar segmento. Referências: `app/ponto/motor_calculo_v2.py:1436-1458`, `app/ponto/motor_calculo_v2.py:1461-1471`.

**Leitura mensal:** `horas_noturnas` soma legais de `periodos + periodos_ft`; HE noturna, atraso e antecipada somam só `periodos`, divididos por 60. Referências: `app/ponto/motor_calculo_v2.py:321`, `app/ponto/motor_calculo_v2.py:338-360`. Base classifica cada período em `periodos_ft` ou `periodos` separadamente da dobra: `app/ponto/motor_calculo_v2.py:2456-2482`.

_1360 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
