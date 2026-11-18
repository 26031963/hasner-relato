# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 08:29** · publicado em **18/11/2026 08:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 08:29 · sessao e54c3fff74ff · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-RESIDUO · verificador · origem=Code

**Resultado: PROCEDENTE em pontos que impedem aprovar o diff como cumprimento integral do aval.** Os principais são o limite literal dos meses da foto, o significado misto dos intervalos do vigia e a falta de prova dos 13 eventos. Também há custo por vínculo e por célula; seu impacto real está **[não verificado]**. [app/escala/folgas.py:619](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:619), [app/ponto/services/vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:30), [app/ponto/services/cadastro_futuro.py:35](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:35).

A cópia disponível não contém metadados Git nem o arquivo do censo indicado; a identidade do HEAD e a comparação com o pai dependem, portanto, do hash e do patch fornecidos **[não verificado]**. Não executei testes, mutantes nem medições: seus resultados e logs estão **[não verificado]**. [nao verificado]

## 1. Diff versus avais e proibições

O alcance comum seleciona células `gerada` com `data > hoje`, sem teto, e a relavra usa a porta com `estender_horizonte=False`. Isso implementa a parte central do pedido para os chamadores que efetivamente chegam ao serviço. [app/ponto/services/cadastro_futuro.py:8](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:8), [app/ponto/services/cadastro_futuro.py:35](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:35).

| Proibição ou limite | Veredito sobre o patch fornecido | Evidência ou contraexemplo |
|---|---|---|
| Tocar o gerador | **NÃO PROCEDENTE** | O patch não altera o corpo do gerador; o vigia continua chamando `montar_dna` e o comando continua chamando `medir` e `lavrar`. [app/ponto/services/vigia_cadastro_futuro.py:27](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:27), [app/ponto/management/commands/gerar_celulas.py:337](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/management/commands/gerar_celulas.py:337). |
| Tocar corpo ou janela da foto | **NÃO PROCEDENTE**, quanto ao corpo da função proibida | A mudança está no seletor que entrega meses; `_gerar_foto_folgas_no_ato` continua recebendo `datas_foto` e gravando a foto mensal. O alcance entregue a ela, porém, tem a ressalva da última linha desta tabela. [app/escala/folgas.py:166](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:166), [app/escala/folgas.py:623](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:623). |
| Tocar o cron `lavrar_foto_folgas` | **NÃO PROCEDENTE** | O patch fornecido não altera o cron, cujo seletor próprio permanece no código. [app/escala/management/commands/lavrar_foto_folgas.py:75](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/management/commands/lavrar_foto_folgas.py:75). |
| Tocar motor | **NÃO PROCEDENTE** | Não há arquivo de motor no patch fornecido; a relavra chama a porta existente. [app/ponto/services/cadastro_futuro.py:50](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:50). |
| Mudar leitor | **NÃO PROCEDENTE** quanto a editar o código de leitor; **PROCEDENTE** quanto à mudança de sentido recebido | `ultimo_lavrado` continua lendo `detalhe`, mas esse detalhe passa a conter contagens sobre outro universo. [app/ponto/services/vigia_cadastro_futuro.py:41](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:41), [app/ponto/services/vigia_cadastro_futuro.py:117](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:117). |
| Juiz novo | **NÃO PROCEDENTE** no patch | O novo helper seleciona linhas; a escolha do vínculo continua delegada a `vinculo_do_dia` ou `vinculos_dos_dias`. [app/ponto/services/cadastro_futuro.py:8](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:8), [app/ponto/services/cadastro_futuro.py:44](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:44), [app/ponto/services/cadastro_futuro.py:73](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:73). |
| Apply em lote do passivo | **NÃO PROCEDENTE** no patch; execução externa **[não verificado]** | O serviço relavra no curso dos eventos e não apresenta um chamador de lote de passivo no patch fornecido. [app/ponto/services/cadastro_futuro.py:20](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:20). |
| Criar célula | **NÃO PROCEDENTE** no caminho novo | O seletor parte de células existentes e chama a porta para cada dia selecionado com `estender_horizonte=False`. [app/ponto/services/cadastro_futuro.py:14](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:14), [app/ponto/services/cadastro_futuro.py:43](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:43), [app/ponto/services/cadastro_futuro.py:50](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:50). |
| Tocar os dois testes da torneira | **NÃO PROCEDENTE** | Eles não constam dos arquivos do patch fornecido. Resultado deles após o diff **[não verificado]**. | [nao verificado]
| Mudar escopo dos meses da foto | **PROCEDENTE no literal do limite do aval 08:04** | A primeira consulta inclui **qualquer origem** dentro da janela, e a segunda acrescenta geradas sem teto. Contraexemplo: só existe uma célula futura *editada* em novembro, dentro da janela: novembro ainda recebe foto sem célula gerada futura naquele mês. O teste novo declara esse comportamento. [app/escala/folgas.py:623](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:623), [app/ponto/tests/test_residuo_alem_da_competencia.py:99](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_residuo_alem_da_competencia.py:99). É preservação do comportamento anterior dentro da janela, mas o aval fornecido diz “só mês que TEM célula gerada futura”. |

Medição prévia do vigia, dry publicado, mutante com log, suite e demais condições de pouso: **[não verificado]**. [nao verificado]

## 2. Escritores, leitores e significado

- **`CelulaDia.origem`, `data`, `escala_geradora` e `dna`.** O modelo define a data da célula, origem gerada/editada, geradora como proveniência e DNA como snapshot; a porta automática escreve os campos e a porta humana pode marcar origem editada. O alcance lê origem e data; o vigia lê também geradora e DNA para comparar com o vínculo do dia e o cadastro. [app/escala/models.py:1681](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/models.py:1681), [app/ponto/portas/celula.py:251](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/portas/celula.py:251), [app/ponto/portas/celula.py:632](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/portas/celula.py:632), [app/ponto/services/vigia_cadastro_futuro.py:54](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:54).

- **`FolgaDia`.** É foto por vínculo e data. O escritor da foto apaga e recria registros de calendário preservando os manuais; há também lançamento manual. O cálculo do vínculo e a precedência leem folgas. O sincronizador escolhe meses a partir de dias selecionados e reconcilia os dias futuros de cada mês, não apenas os dias que têm célula. [app/escala/models.py:1582](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/models.py:1582), [app/escala/folgas.py:180](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:180), [app/escala/folgas.py:329](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:329), [app/escala/models.py:1249](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/models.py:1249), [app/ponto/precedencia.py:206](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/precedencia.py:206), [app/escala/folgas.py:632](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:632).

- **Dict de `medir`. PROCEDENTE: os campos não descrevem um único intervalo.** `ini` e `fim` são o intervalo pedido; para não geradas, o filtro usa esse intervalo inclusivo e `data > hoje`; para geradas, ignora *ambos* os limites e usa todas as datas `> hoje`. `fim_do_alcance` é o máximo das geradas, ou `None`. Contraexemplo: com hoje em 18/11 e intervalo pedido 1–20/12, uma gerada de 25/11 entra, mas uma editada de 25/11 não entra. [app/ponto/services/vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:30), [app/ponto/services/cadastro_futuro.py:14](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:14), [app/ponto/services/vigia_cadastro_futuro.py:88](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:88).

- **Contadores e selos.** `universo`, baldes e selo de DNA contam geradas; `universo_julgada` e selo de julgada contam a união mista descrita acima. Assim, a descrição “TODA futura” do denominador de julgada não corresponde a todas as editadas futuras fora do intervalo pedido. Os demais campos particionam ou qualificam os casos conforme os ramos do vigia. [app/ponto/services/vigia_cadastro_futuro.py:37](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:37), [app/ponto/services/vigia_cadastro_futuro.py:48](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:48), [app/ponto/services/vigia_cadastro_futuro.py:96](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:96).

- **Snapshot e leitores. PROCEDENTE: número e sentido podem mudar.** `lavrar` grava `data_ini=medida['ini']`, numerador, denominador e o dict completo em `detalhe`; uma gerada anterior a `data_ini`, como no contraexemplo, pode compor o número. `ultimo_lavrado` devolve o detalhe e a API do copiloto o expõe. Uma divergência em gerada fora da janela aumenta o número em relação ao universo antigo; o valor de produção **[não verificado]**. Leitura específica por haiku, placar e outros selos **[não verificado]**. [app/ponto/services/vigia_cadastro_futuro.py:105](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:105), [app/ponto/services/vigia_cadastro_futuro.py:117](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:117), [app/api/views_mensageria.py:643](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/api/views_mensageria.py:643).

O serviço novo exclui hoje estritamente; a porta geral e o cron também têm caminhos para o próprio dia de hoje. Isso é diferença declarada de alcance, não prova isolada de erro. [app/ponto/services/cadastro_futuro.py:14](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:14), [app/ponto/management/commands/gerar_celulas.py:303](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/management/commands/gerar_celulas.py:303).

## 3. Propriedade dos 13 eventos

**Cumprimento para os 13: [não verificado].** O item `(e)` do censo não está disponível nesta cópia, portanto não é possível identificar os 13 com segurança nem declarar uma lista exaustiva de falhas.

Nos caminhos identificáveis, folga lançada, vigência, âncora, vínculo, template e feriado chegam ao serviço sem teto, sujeitos às guardas de cada caminho. [app/escala/folgas.py:290](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:290), [app/escala/signals.py:80](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/signals.py:80), [app/escala/signals.py:131](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/signals.py:131), [app/colaboradores/services/vinculo.py:335](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/colaboradores/services/vinculo.py:335), [app/escala/services/cadastro_tipo.py:370](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/services/cadastro_tipo.py:370), [app/core/signals.py:28](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/core/signals.py:28).

O seletor de foto ainda lê `_janela_default`, mas acrescenta à união as geradas sem teto; essa leitura remanescente **não demonstra, por si, um evento gerado distante perdido**. Não identifiquei outro seletor produtivo `_janela_default` que prove uma falha entre os eventos; a exaustividade permanece **[não verificado]**. [app/escala/folgas.py:619](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:619).

## 4. Classes de reprovação

- **Segundo juiz ou segundo escritor fora da porta — NÃO PROCEDENTE no diff examinado.** O helper seleciona e delega; a escrita de célula passa por `regenerar_celulas_vinculo`. [app/ponto/services/cadastro_futuro.py:8](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:8), [app/ponto/services/cadastro_futuro.py:50](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:50).

- **Relógio fora da borda — NÃO PROCEDENTE no código novo examinado.** Os serviços exigem `hoje`; por exemplo, o salvamento de calendário lê uma data e a passa aos vínculos. Outros caminhos não auditados integralmente **[não verificado]**. [app/ponto/services/cadastro_futuro.py:12](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:12), [app/escala/folgas.py:597](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:597).

- **Derivação paralela — PROCEDENTE quanto ao conjunto de meses.** A foto mantém o seletor próprio de todas as origens dentro da janela e o une ao alcance novo. Contraexemplo: a editada sem gerada no mês ainda determina foto, como no item 1. Não é um segundo juiz de vínculo. [app/escala/folgas.py:619](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:619).

- **Selo/RED sem mordida — PROCEDENTE parcialmente.** A caracterização de foto passa por coincidência de mês; os testes novos mais fortes não provam todos os disparos reais. Detalhes no item 6. [app/ponto/tests/test_cadastro_futuro_sem_banco.py:645](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:645), [app/ponto/tests/test_residuo_alem_da_competencia.py:51](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_residuo_alem_da_competencia.py:51).

- **Travas — deadlock ou trava a menos: [não verificado].** As duas consultas da foto usam `select_for_update`; a segunda pode reencontrar linhas já travadas, o que não prova falta de trava. O modelo declara ordenação por colaborador e data, e a consulta da relavra ordena por data; não há aqui demonstração suficiente de inversão efetiva de ordem ou deadlock. Os dublês sem banco não exercitam locks. [app/escala/folgas.py:620](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/folgas.py:620), [app/escala/models.py:1830](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/models.py:1830), [app/ponto/services/cadastro_futuro.py:43](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:43), [app/ponto/tests/test_cadastro_futuro_sem_banco.py:170](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:170).

- **Campo com dois significados — PROCEDENTE.** `ini`/`fim` e `data_ini` continuam rotulando o intervalo pedido, enquanto os números de geradas podem incluir datas fora dele; o denominador de julgada mistura dois recortes. Contraexemplo no item 2. [app/ponto/services/vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:30), [app/ponto/services/vigia_cadastro_futuro.py:110](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/vigia_cadastro_futuro.py:110).

## 5. Custo e teto

**PROCEDENTE: há trabalho por vínculo e por célula.** Template percorre os vínculos do tipo; cada um consulta e trava suas células e chama a porta uma vez por dia. Cada chamada à porta consulta exportações e células. Assim, um vínculo com muitas células futuras multiplica chamadas e consultas. [app/escala/services/cadastro_tipo.py:370](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/services/cadastro_tipo.py:370), [app/ponto/services/cadastro_futuro.py:35](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:35), [app/ponto/services/cadastro_futuro.py:49](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:49), [app/ponto/portas/celula.py:218](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/portas/celula.py:218).

**Feriado: NÃO PROCEDENTE a premissa de que necessariamente percorre oito meses por vínculo.** Ele filtra pelas datas de feriado alteradas, agrupa as células por geradora e chama o mesmo serviço com `datas=dias`; ainda assim, paga seleção por vínculo e porta por célula selecionada. [app/ponto/services/cadastro_futuro.py:64](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/services/cadastro_futuro.py:64).

Os caminhos podem rodar sincronamente em request: o signal de feriado no save e a propagação de template no fluxo de salvamento. A configuração da UI é `gthread`, `timeout=30`; isso não demonstra prazo máximo de 30 segundos por request. Tempo e número de consultas antes/depois **[não verificado]**. [app/core/signals.py:28](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/core/signals.py:28), [app/escala/views_wizard.py:95](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/escala/views_wizard.py:95), [app/gunicorn.conf.py:17](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/gunicorn.conf.py:17), [app/holerite/views.py:595](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/holerite/views.py:595).

## 6. Testes [nao verificado]

- **PROCEDENTE: coincidência de data na caracterização da foto.** `FIM + 1` é 21/11, mas o dublê do calendário devolve 13/11; ambos são novembro. O assert de mês e o de existência podem passar sem provar folga no dia distante nem foto de mês posterior à competência. [app/ponto/tests/test_cadastro_futuro_sem_banco.py:20](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:20), [app/ponto/tests/test_cadastro_futuro_sem_banco.py:585](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:585), [app/ponto/tests/test_cadastro_futuro_sem_banco.py:645](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/ponto/tests/test_cadastro_futuro_sem_banco.py:645).

- **NÃO PROCEDENTE: essa lacuna deixa a foto distante inteiramente sem teste.** O teste com banco usa dezembro e verifica `FolgaDia` no dia distante e `cel.trabalha=False`. Sua execução **[não 

[truncado em 20000 de 21867 caracteres]

## 18/11 08:27 · sessao 8d91d47ae5dc · - · - · origem=Ronald (tmux)

Enviados ao agente principal os achados sobre custo e o contraexemplo de troca de vínculo com calendário padrão: a foto é gerada apenas para o mês civil atual e o seguinte, enquanto a relavra alcança células posteriores ([vinculo.py:151](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/colaboradores/services/vinculo.py:151), [vinculo.py:335](/home/ronald/codex_ws/9cfb1dbbb41c429ad39d359554b8e5ad3ad467e1/app/colaboradores/services/vinculo.py:335)).

O censo indicado não foi encontrado na cópia; a verificação exaustiva dos 13 eventos permanece [não verificado].

## 18/11 08:27 · sessao c4ef979324bd · - · - · origem=Ronald (tmux)

Os achados sobre campos, foto, vigia e leitores do snapshot foram enviados ao agente principal, com referências ao HEAD e contraexemplo para o intervalo misto.

## 18/11 08:27 · sessao 0fae26c8a5a4 · - · - · origem=Ronald (tmux)

Achados sobre testes e travas enviados ao agente principal com referências de arquivo e linha. [nao verificado]

## 18/11 08:15 · sessao 176d33e7aeeb · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-RESIDUO · construtor · origem=Code

A implementação e os testes estão escritos na cópia. **A cura ainda não está certificada:** não executei RED, GREEN, mutantes ou suíte, conforme a instrução; a sintaxe dos quatro arquivos passou por análise com `ast.parse`. O HEAD da cópia permanece `dd489c627bb0a8d5f8782945c7e240e6621c98ed`; não criei commit, conforme a proibição. [cadastro_futuro.py:8](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:8), [test_residuo_alem_da_competencia.py:20](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/tests/test_residuo_alem_da_competencia.py:20)

Alterações:

- [cadastro_futuro.py::alcance_celulas_geradas](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:8), `regenerar_futuras_do_cadastro` e `regenerar_futuras_por_feriado`: alcance comum de células geradas existentes, posteriores a hoje, sem teto de competência. A relavra continua com `estender_horizonte=False`.
- [folgas.py::sincronizar_foto_futura_do_calendario](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:605): mantém qualquer origem dentro da janela e seleciona geradas além dela; os meses continuam saindo dos dias selecionados.
- [vigia_cadastro_futuro.py::medir](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/vigia_cadastro_futuro.py:24): mede todo o alcance gerado e preserva as não geradas no intervalo anteriormente solicitado. Os limites retornados são ampliados para abranger as células medidas.
- [test_residuo_alem_da_competencia.py::ResiduoAlemDaCompetenciaTest](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/tests/test_residuo_alem_da_competencia.py:20): casos para troca e encerramento de vínculo, folga manual, calendário, controle de dono, editadas, vigia e feriado. A execução e a capacidade de morder dos casos estão **[não verificado]**.

**ESMERIL-SEMÂNTICA.** `hoje` é dia civil exclusivo (`data > hoje`); `desde`, quando fornecido, é inclusivo. A janela da foto usa os dois extremos inclusivos e a vigência do vínculo também inclui início e fim. [cadastro_futuro.py:14](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:14), [cadastro_futuro.py:38](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:38), [folgas.py:620](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:620), [alimentacao.py:214](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/alimentacao.py:214)

O seletor lê `colaborador_id`, `data` e `origem` da célula; a porta existente cria células e escreve `trabalha`, `escala_geradora`, `dna` e a trilha de regeneração, enquanto a juíza recebe o carimbo para decidir o vínculo. A foto lê a definição do calendário e escreve ou remove `FolgaDia` por vínculo, data e origem de calendário; a folga manual tem porta própria. Os escritores completos de cadastro desses campos são **[não verificado]**. [cadastro_futuro.py:14](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:14), [celula.py:289](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/portas/celula.py:289), [alimentacao.py:228](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/alimentacao.py:228), [folgas.py:43](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:43), [folgas.py:180](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:180), [folgas.py:335](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/folgas.py:335)

O custo cresce com as células futuras adicionais: feriado consulta as células nas datas recebidas e chama a relavra por vínculo encontrado; template percorre os vínculos do tipo e a relavra percorre os dias selecionados. Quantidades de consultas e tempos antes/depois estão **[não verificado]**. [cadastro_futuro.py:68](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/ponto/services/cadastro_futuro.py:68), [cadastro_tipo.py:370](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-residuo-081057/app/escala/services/cadastro_tipo.py:370)

O mutante principal, **“teto de volta”**, é este patch, para aplicação temporária pela sessão de testes; não foi aplicado aqui: [nao verificado]

```diff
--- a/app/ponto/services/cadastro_futuro.py
+++ b/app/ponto/services/cadastro_futuro.py
@@
     if hoje is None:
         raise ValueError('alcance_celulas_geradas: hoje e obrigatorio')
-    qs = CelulaDia.objects.filter(origem='gerada', data__gt=hoje)
+    from ponto.management.commands.gerar_celulas import _janela_default
+    ini, fim = _janela_default(hoje)
+    qs = CelulaDia.objects.filter(
+        origem='gerada', data__gt=hoje, data__range=(ini, fim))
```

Comandos propostos para a sessão, no ambiente de testes configurado; resultados e configuração estão **[não verificado]**: [nao verificado]

```bash
# Novos casos: repetir na base para RED, na cura para GREEN e com cada mutante.
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_residuo_alem_da_competencia

# Torneira, com a data que coloca hoje+3 além do fim informado.
PYTHONDONTWRITEBYTECODE=1 python3 -c "from freezegun import freeze_time; import manage; import sys; sys.argv=['manage.py','test','escala.tests.test_torneira_folgas']; f=freeze_time('2026-11-18 15:00:00'); f.start(); manage.main()"
```

Para os demais mutantes, aplicar **um de cada vez** e executar o primeiro comando acima:

1. Em `sincronizar_foto_futura_do_calendario`, retirar `| Q(pk__in=geradas.values('pk'))`: deve atingir o teste de foto distante. [nao verificado]
2. Em `regenerar_futuras_por_feriado`, voltar a limitar `datas` ao `fim` de `_janela_default`: deve atingir o teste de feriado distante. [nao verificado]
3. Em `medir`, substituir a seleção das geradas pelo filtro `data__range=(ini, fim)`: deve atingir o teste do vigia. [nao verificado]

A queda esperada desses mutantes, inclusive a dos dois testes da torneira pelo teto, permanece **[não verificado]**. [nao verificado]

O dado raspado informa, antes da cura, **112 divergências de cadastro além do fim**, em 3.865 células geradas, e 112 no alcance novo total; não houve nova medição aqui **[não verificado na fonte]**. Permanecem incompletos: logs RED/GREEN/mutantes e suíte, medições de custo, dry do passivo, medição posterior e revisão final. O censo e as sondas pedidos não estavam disponíveis nos caminhos da cópia consultados; seu conteúdo além dos anexos fornecidos é **[não verificado]**. [nao verificado]

Mensagem proposta para o commit futuro, sem executá-lo: [nao verificado]

```text
Alcança células futuras existentes nos eventos de cadastro

LEI-AKITA: origem=ponto/services/cadastro_futuro.py (alcance do seletor) e escala/folgas.py (meses da foto pelo mesmo alcance), testemunha=vinculo_do_dia e a porta da celula, RED=test_torneira_folgas (2) + troca de vinculo, folga e foto alem do fim, quem-mais-le=censo residuo_alem_da_competencia.r item (e), juizes novos=0, padrao=projecao reconstruivel

Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
```

## 18/11 08:07 · sessao a4ee9aacdb54 · RETOMA-POS-POUSOS-1811 · verificador · origem=Code

| Item | Pergunta | Resultado e motivo |
|---|---|---|
| **1 — P2** | **1. Premissas** | **PROCEDE — há dependência de sítio classe (ii).** Os leitores que prendiam a P2 estão nesta cópia com o comportamento B2: `sem_veredito_entre` chama o juiz; `estado` lê `respondida`; e a property delega ao juiz. O universo da ressuscitação usa `q_nao_respondida` e preserva a exclusão de texto sem instante e sem validação. ([models.py:1530](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1530), [models.py:1541](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1541), [models.py:1931](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1931), [models.py:3162](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3162), [models.py:3674](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3674)). Entretanto, a escrita da ressuscitação chama `descarimbar_em_lote` em **A8**, sítio que a matriz fornecida lista como classe (ii); o censo fornecido diz que a escrita compensadora P2 opera nesse universo. A aplicação e o efeito da P2 rebaseada estão **[não verificado]**. ([models.py:1599](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1599), [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637); anexos fornecidos `censo.r` e `MATRIZ.md`, arquivo:linha **[não verificado]**). |
| **1 — P2** | **2. Colisão** | **PROCEDE — a retomada precisa ser devolvida no ramo escrito pelo próprio aval.** A exceção para antecipar a P2 dependia de censo independente; o censo fornecido concluiu dependência. O aval atual manda devolver se houver dependência de sítio classe (ii)/B2b, condição encontrada em A8. A continuação sem resolver essa condição não está autorizada pelo literal. (Anexos fornecidos `libera_b3_f2p2_1758.txt`, item 2, `aval.txt`, item 1, e `MATRIZ.md`, A8; arquivo:linha **[não verificado]**; [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637)). |
| **1 — P2** | **3. Literal** | **PROCEDE — “pouso da B2” não fecha automaticamente com B2a.** O corte anterior condicionava o pouso da B2 a três resultados; o aval posterior dividiu a obra, dispensou os 39 sítios para **B2a** e proibiu declarar a B2 fechada. O relato de B2a no ar e os resultados efetivos das condições de pouso são **[não verificado]**. A contagem da corrida de `respondida_em`, seu universo desde o reload e denominador foram definidos no complemento; N1/N2 e seus valores de produção são **[não verificado]**. (Anexos fornecidos `corte_b2_cerca_no_escritor_1711.txt`, item 6, `b2_escopo_e_torneira_constroi_1811_0148.txt`, item 1, e `complemento1_libera_b3_1808.txt`; arquivo:linha **[não verificado]**). | [nao verificado]
| **1 — P2** | **4. Exportada, dinheiro ou `!`** | **CONFERE quanto ao escopo declarado; efeito efetivo [não verificado].** O aval manda preservar condições anteriores de pouso, inclusive crons drenados e parada se as medidas indicadas forem maiores que zero; não apresenta autorização nova para alterar exportada ou dinheiro. Como a P2 não está aplicada nesta cópia e não há dados de execução, seus efeitos efetivos são **[não verificado]**. (Anexos fornecidos `aval.txt`, item 1, e `libera_b3_f2p2_1758.txt`, item 3; arquivo:linha **[não verificado]**). | [nao verificado]
| **2 — resíduo** | **1. Premissas** | **PROCEDE — o seletor atual tem um detalhe que o novo limite precisa corrigir.** Os dois serviços existem; `cadastro_futuro` seleciona futuras **geradas** existentes, enquanto o seletor da foto seleciona células futuras sem filtrar `origem='gerada'`. Portanto, simplesmente ampliar o teto atual da foto não garante o limite novo de meses com célula gerada. Os meses realmente saem dos dias selecionados; a foto alcança do amanhã ao fim de cada mês selecionado. ([cadastro_futuro.py:26](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cadastro_futuro.py:26), [folgas.py:619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:619), [folgas.py:627](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:627)). `_gerar_foto_folgas_no_ato` é chamado diretamente por `gerar_foto_folgas` e pelo sincronizador; o comando `lavrar_foto_folgas` chama `gerar_foto_folgas`, e está configurado como cron às 05:47. Outros chamadores do comando são **[não verificado]**. ([folgas.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:150), [folgas.py:639](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:639), [lavrar_foto_folgas.py:170](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/management/commands/lavrar_foto_folgas.py:170), [crons.py:425](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:425)). |
| **2 — resíduo** | **2. Colisão** | **CONFERE — o texto novo responde expressamente às duas devoluções.** Ele autoriza ampliar os meses entregues à foto, conservando corpo e janela do escritor e do cron; aceita também o dreno por evento, inclusive passivo alcançado. A relavra em lote do restante continua condicionada a dry publicado e `!`. O restante das condições de 01:48 continua vigente. (Anexos fornecidos `aval.txt`, item 2, `devolvido_0150.txt` e `b2_escopo_e_torneira_constroi_1811_0148.txt`, item 3; arquivo:linha **[não verificado]**). | [nao verificado]
| **2 — resíduo** | **3. Literal** | **PROCEDE — “a mesma medida 24 h depois” não está reproduzível com o dado entregue.** O texto dá três números de referência com unidades e universos diferentes: 389 células/12 colaboradores, 26 dias de folga/16 colaboradores e 287 células/8 desligados. Não foram fornecidos os scripts ou saídas das três sondas, suas consultas, denominadores completos, regras de deduplicação ou o instante de cada medida; a repetição exata e a comparabilidade dos universos após 24 horas ficam **[não verificado]**. O `!` permanece para o **lote do que sobrar**, não para os eventos aceitos; o que efetivamente sobrará é **[não verificado]**. A janela atual é a união das competências correntes das empresas ativas, portanto seu fim não é necessariamente o da empresa do colaborador. (Anexos fornecidos `aval.txt`, item 2, e `b2_escopo_e_torneira_constroi_1811_0148.txt`, item 3; arquivo:linha **[não verificado]**; [gerar_celulas.py:290](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/gerar_celulas.py:290)). |
| **2 — resíduo** | **4. Exportada, dinheiro ou `!`** | **CONFERE quanto ao `!` declarado; efeitos reais [não verificado].** O dreno por evento sem `!`, inclusive sobre passivo alcançado, foi explicitamente aceito. A porta atual barra células em competência exportada e pode alterar `trabalha`, geradora e DNA das geradas alcançadas; a mudança proposta ainda não existe nesta cópia. A foto ampliada também grava ou apaga `FolgaDia` nas datas futuras dos meses selecionados, e seu efeito financeiro concreto é **[não verificado]**. (Anexo fornecido `aval.txt`, item 2(B), arquivo:linha **[não verificado]**; [celula.py:284](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:284), [celula.py:297](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:297), [folgas.py:180](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:180)). |

**Quem escreve e quem lê além da competência**, no alcance identificado pelo código e pelo censo fornecido:

| Dado | Escritores | Leitores |
|---|---|---|
| **FolgaDia** | A foto de calendário reconcilia e grava registros; há também lançamento manual, lote e reparentamento. A relação completa de escritores é **[não verificado]**. ([folgas.py:166](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:166), [folgas.py:329](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:329), [folgas.py:405](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:405), [folgas.py:513](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/folgas.py:513)). | `folgas_lancadas_do_periodo` alimenta a precedência; o cálculo da escala também consulta a foto do vínculo, e o Plano de Folgas lê a folga gravada. ([precedencia.py:195](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/precedencia.py:195), [models.py:1249](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/models.py:1249), [views_plano_folgas.py:60](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/views_plano_folgas.py:60)). |
| **CelulaDia** | A porta `regenerar_celulas_vinculo` cria ou reescreve geradas; `editar_dia` cria célula humana; `gerar_celulas_periodo` cria ausentes. A seleção atual de eventos de cadastro limita a relavra futura à janela corrente. ([celula.py:289](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:289), [celula.py:631](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:631), [celula.py:883](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/portas/celula.py:883), [cadastro_futuro.py:26](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cadastro_futuro.py:26)). | `eh_dia_trabalho` lê o booleano da célula, usado pela Minha Escala; calendário/status pode chegar à folga pela precedência. O auditor C5 cruza FolgaDia e célula sem teto futuro indicado no censo. ([models.py:1092](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/models.py:1092), [views.py:38](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/views.py:38), [status_dia.py:296](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/status_dia.py:296), [auditar_invariantes_chamados.py:1536](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1536)). |

A identificação desta cópia como o hash informado e o estado de produção B2a/S6 estão **[não verificado]**: a cópia consultada não dispõe de metadados Git, e não houve acesso a banco ou logs.

## 18/11 07:46 · sessao 756f54b454dc · E4C-F1-B2b · leitura · origem=Code

| Resultado da leitura | Situação |
|---|---|
| Censo exaustivo de escritas sem guarda | **[não verificado]**. Localizei os caminhos abaixo, inclusive uma migração que a leitura anterior não havia identificado. Falta resolver todos os aliases e chamadas indiretas no universo pesquisado; portanto, não afirmo zero para os demais caminhos. |
| Matriz dos 39 sítios | **[não verificado]**. O anexo disponibilizado é identificado como volta 8 e contém 55 linhas, inclusive linhas sobrepostas e linhas com partição própria. Não consegui estabelecer com segurança a correspondência individual com os 39 da v7 nem concluir todas as cadeias e custos. A matriz abaixo contém somente os sítios cuja evidência nesta árvore consegui conferir. A referência da própria árvore aos 39 está em [fio.py:1397](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1397). |
| Escritores e leitores completos dos cinco campos | **[não verificado]**. A matriz de campos traz achados confirmados, mas não é uma lista completa; faltou rastrear todos os usos ORM, relações, aliases e consumidores. |
| Identificação do sítio para contar | A guarda atual recebe somente `perguntas`; o estado da cerca contém `vetadas`, `memo` e `empresa`, sem identificador do chamador. Assim, hoje não há dado geral na assinatura ou nesse estado para nomear o sítio sem inspecionar a pilha. [fio.py:1389](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1389), [fio.py:1445](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1445). |
| Identidade da árvore | **[não verificado]**: o diretório disponibilizado não contém metadados Git para confirmar independentemente o hash informado. |

| Busca declarada para o censo | Resultado e limite |
|---|---|
| Em `app/`, buscas textuais por `PerguntaDisputa`, `perguntadisputa`, relações como `perguntas_que_resolvem`, `.update(`, `bulk_update`, `bulk_create`, `.create(`, `.raw(`, `cursor`, SQL `UPDATE`/`INSERT`, construtores e `_state.adding`; inspeção manual dos resultados relevantes. | A busca direta encontrou o lote de lavras, o lote guardado e a migração por modelo histórico abaixo. **Não prova exaustividade**: atribuição por alias, SQL montado dinamicamente, managers relacionados e objetos passados entre funções requerem rastreamento adicional. O próprio código documenta essa limitação para `qs.update()`: [models.py:3401](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3401). |

| Escrita localizada | Campos e chamador | Passa pela guarda? |
|---|---|---|
| [veredito_lavrado.py:250](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:250), `bulk_update` | `veredito_lavrado`, `julgado_em`, `janela_expira_em`; comando [backfill_veredito_lavrado.py:143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_veredito_lavrado.py:143), inclusive sobra em linha 146. Campos: [veredito_lavrado.py:237](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:237). | **Não**: o corpo do lote chama `bulk_update` diretamente, sem a guarda. |
| [0051_decisao_pergunta.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:25), `PD.objects.update` | `colaborador_id`, derivado da disputa; chamada pela operação de migração [0051_decisao_pergunta.py:101](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:101). | **Não**: `PD` é modelo histórico obtido por `apps.get_model`, e a operação usa `update`. Se essa migração já rodou no ambiente de produção: **[não verificado]**. [0051_decisao_pergunta.py:22](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:22). |
| [0051_decisao_pergunta.py:27](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:27), `PD.objects.filter(...).update` | `instancia_marco`; mesma operação de migração. [0051_decisao_pergunta.py:22](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:22), [0051_decisao_pergunta.py:101](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:101). | **Não**, pelo mesmo caminho de modelo histórico e `update`. |
| [models.py:3135](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3135), `save(force_insert=True)` | Inserção dos campos da instância, incluindo disputa, motivo, destino, célula, conversa, dia e `**campos`; há entradas de emissão e declaração humana em [disputa_emissao.py:453](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:453) e [declaracao_texto.py:377](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/declaracao_texto.py:377). Outros chamadores: **[não verificado]**. | **Não**: `save` chama a guarda somente se `not self._state.adding`. [models.py:3562](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3562). |
| [models.py:3405](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3405), `update` de descarimbo | Zera `materializada_em`, `materializada_por`, `via_resolucao` e grava `**outros`; exemplos de chamadores em [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637) e [disputa_emissao.py:1649](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1649). | **Sim**, explicitamente antes do `update`; fora da cerca a chamada retorna sem julgar. [models.py:3398](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3398), [fio.py:1402](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1402). |
| Demais `update`, `bulk_update`, `bulk_create`, SQL cru, inserções por outros managers e aliases | **[não verificado]**: não foi demonstrado zero de chamadores nem completado o rastreamento necessário para listar todos. | **[não verificado]**. |

| Regra comum para ler a matriz de sítios | Evidência |
|---|---|
| **Porta** significa chegada à guarda, não julgamento atual: fora da cerca ela retorna antes de consultar. Dentro, compara dia em memória e dia gravado para perguntas que não são conversa; `None` no conjunto é indecidível e a guarda trata o resultado como veto. [fio.py:1402](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1402), [fio.py:1409](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1409), [fio.py:1358](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1358). |
| **Hipótese de recusa** exige uma implementação que crie estado válido fora da cerca. Retirar somente o `return` não basta: o restante acessa `estado['empresa']`, `estado['memo']` e `estado['vetadas']`. A guarda lança antes do `atomic` interno do `save`. [fio.py:1420](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1420), [models.py:3562](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3562). |
| **Custo**: passando a julgar, a forma atual consulta as linhas gravadas e pode consultar empresa e competência; o custo exato por caminho e quais tetos falhariam são **[não verificado]**, pois dependem da implementação e de medição. Há teto com folga zero para `reconciliar (gate da premissa, FECHA com dia e celula)`, e o selo compara consultas medidas ao teto. [fio.py:1409](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1409), [test_selo_chokepoint_escrita.py:134](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_selo_chokepoint_escrita.py:134), [test_selo_chokepoint_escrita.py:287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_selo_chokepoint_escrita.py:287). |

| Sítio conferido nesta árvore | Porta e sinal acima dela | Se julgar sempre e recusar |
|---|---|---|
| **A5** relavra de destino: [models.py:307](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:307) | Passa pelo `save` da lavra; acima há `self.pk` do chamado e o laço das perguntas de destino. [models.py:309](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:309), [veredito_lavrado.py:275](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:275). | O savepoint da relavra é desfeito e `engolir` trata a falha; a transição anterior está no atomic externo. Efeitos adicionais de receivers e teto atingido: **[não verificado]**. [models.py:280](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:280), [models.py:307](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:307). |
| **A8** ressuscitar: [models.py:1637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1637) | Passa pela guarda de lote; o chamado e o conjunto `alvo` estão acima dela. [models.py:1606](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1606), [models.py:3398](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3398). | Se a exceção propagar, o atomic de [models.py:1613](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1613) desfaz as decisões anteriores da unidade e impede descarimbo, despertar e reabertura subsequentes. Capturas externas e custo: **[não verificado]**. |
| **B1a** absolvida por célula: [reconciliador.py:639](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:639) | Passa por `carimbar` e `save`; o laço usa usuário de sistema. [reconciliador.py:628](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:628), [models.py:3222](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3222). | O atomic por pergunta é desfeito; o `except` registra e segue o laço. Outros efeitos e custo: **[não verificado]**. [reconciliador.py:636](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:636), [reconciliador.py:645](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:645). |
| **B3a** órfã pela lâmpada: [reconciliador.py:1192](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1192) | Passa por `encerrar_fantasma` e `carimbar`; há usuário e contexto do reconciliador acima. [models.py:3419](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3419), [reconciliador.py:1186](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1186). | A exceção no carimbo desfaz o atomic da pergunta e impede o evento seguinte; propagação até a entrada e custo: **[não verificado]**. [reconciliador.py:1191](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1191). |
| **B4** reaponte: [fio.py:974](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:974) | O `save` passa; a função recebe `p`, `chamado` e `data_evento`, sem origem ou usuário. Também é chamada por declaração humana, portanto sua presença isolada não identifica sistema. [fio.py:853](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:853), [declaracao_texto.py:371](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/declaracao_texto.py:371). | A recusa impede o `save`; efeitos e atomic de cada chamador, inclusive perdedoras e eventual data atribuída em memória: **[não verificado]**. |
| **B6** lote de lavras: [veredito_lavrado.py:250](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/veredito_lavrado.py:250) | **Não passa** pela guarda atual. O comando chamador é sinal de execução de sistema. [backfill_veredito_lavrado.py:143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_veredito_lavrado.py:143). | Uma mudança apenas na guarda de `save` não o alcança; custo e recusa para um eventual julgamento do lote: **[não verificado]**. |
| **B7** relavra de via: [relavra_via.py:127](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/relavra_via.py:127) | Passa; `ator` e `origem` existem acima, mas não são parâmetros da guarda. [relavra_via.py:106](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/relavra_via.py:106). | A exceção interrompe o laço se não for capturada acima. Atomic externo, reversão dos itens anteriores e custo em cada entrada: **[não verificado]**. |
| **P1** reuso: [models.py:3124](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3124) | Passa no `save` de instância existente; há entradas tanto de emissão quanto de declaração humana. [disputa_emissao.py:453](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:453), [declaracao_texto.py:377](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/declaracao_texto.py:377). | O savepoint do contrato de abertura é relevante, mas a propagação e o efeito integral por entrada são **[não verificado]**. [models.py:2919](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2919). |
| **P11** pendência: [materializacao.py:1353](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1353) | Passa; `_materializar_uma` recebe disputa, pergunta e usuário, e o laço de materialização tem atomic por pergunta. [materializacao.py:1833](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1833), [materializacao.py:2098](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2098). | A exceção pode entrar no tratamento da materialização e provocar nova tentativa de gravar recusa no `finally`; o resultado completo dessa segunda recusa e o custo são **[não verificado]**. [materializacao.py:1978](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1978), [materializacao.py:1990](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1990). |
| **P15** carimbo do plantio: [materializacao.py:1968](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1968) | Passa pelo `save` de `carimbar`; o contexto inclui `request_user` e o atomic do plantio. [materializacao.py:1911](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1911). | A exceção desfaz o atomic do plantio; o tratamento acrescenta erro e o `finally` tenta carimbar a recusa. O efeito da segunda exceção e o custo: **[não verificado]**. [materializacao.py:1978](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1978), [materializacao.py:1990](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1990). |
| **X1 / A9c** S144: [materializacao.py:2167](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2167) | Passa; o signal é disparado pelo fechamento de disputa e recebe `fechada_por`, que pode ter origem humana ou de sistema. [signals.py:150](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:150), [signals.py:165](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:165). | O signal captura a exceção e interrompe sua materialização; **sob cerca**, a anotação faz o abridor desfazer o ato inteiro. Fora dela, com uma hipotética guarda independente, o efeito completo da transação e dos carimbos anteriores é **[não verificado]**. [signals.py:166](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:166), [fio.py:1449](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1449). |

| Campo | Escritores e leitores confirmados; lacuna |
|---|---|
| `data_evento` | `gravar_dia_do_fato` atribui o dia em memória; nascimento usa essa função antes do insert; a guarda lê instância e linha gravada. [fio.py:849](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:849), [models.py:3132](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3132), [fio.py:1409](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1409). Para fato, o juiz o define como **dia do turno**; para conversa, o mesmo campo guarda **dia civil** e o juiz retorna `None`. [juizes.py:338](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:338). Lista completa e divergências adicionais: **[não verificado]**. |
| `conversa` | `abrir` passa `False` e `abrir_conversa` passa `True`; esta usa `localdate()`. A guarda discrimina tanto a instância quanto o valor gravado. [models.py:2902](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2902), [models.py:2914](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2914), [fio.py:1414](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1414). Lista completa: **[não verificado]**. |
| `chamado_destino` | Reaponte e reuso atribuem o campo; a relavra filtra perguntas por destino. [fio.py:974](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:974), [models.py:3100](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3100), [models.py:309](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:309). Lista completa: **[não verificado]**. |
| `materializada_em` | O carimbo atribui instante; descarimbo em lote o zera; S144 seleciona as perguntas em que está nulo. [models.py:3216](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3216), [models.py:3405](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3405), [materializacao.py:2154](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2154). Lista completa: **[não verificado]**. |
| `disputa.colaborador` | A guarda lê `q.disputa.colaborador_id` para buscar a empresa; a migração histórica copia esse identificador para a coluna desnormalizada da pergunta. [fio.py:1420](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1420), [0051_decisao_pergunta.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/migrations/0051_decisao_pergunta.py:25). Todos os escritores/leitores da FK da disputa e possíveis divergências: **[não verificado]**. |
| Fuso | A configuração declara `America/Sao_Paulo` e `USE_TZ=True`; o dia da conversa nasce de `localdate()`. Conversões específicas de todos os leitores de instante e eventuais diferenças de fuso: **[não verificado]**. [base.py

[truncado em 20000 de 21334 caracteres]

## 18/11 07:05 · sessao b3c6012a4dc8 · S6-INTERMITENTE-FERIADO-CURA-A · sonda · origem=Code

[Não verificado] Saída da sonda conforme o dado fornecido:

```text
sonda_leitura: script.py [a2fb8b54fbba6929] no cpuset 4-7, lendo sombra como leitor (LEITURA)

MOVEU emp 2 cod 1401 pk 365 [('intermitente', 'intermitente', '2026-07-21', 'None')]
MOVEU emp 2 cod 1995 pk 830 [('intermitente', 'intermitente', '2026-07-21', 'None')]
MOVEU emp 3 cod 197 pk 146 [('intermitente', 'intermitente', '2026-06-24', 'None')]
MOVEU emp 3 cod 450 pk 85 [('intermitente', 'intermitente', '2026-07-22', 'None')]
FIM
rc_sombra=0
```

## 18/11 07:01 · sessao ea69b279ba00 · R1-REMEDE-1811 · censo · origem=Code

**O código não mostra uma diferença de universo, filtro ou unidade de contagem que, por si só, explique 26 × 33.** Se ambas as execuções percorreram os mesmos colaboradores e produziram os mesmos julgamentos, a soma das seis partes e a contagem das linhas `ESTRUTURA` do CSV devem coincidir. A causa concreta dos sete dias de diferença é **[não verificado]**. [app/ponto/management/commands/e6_oraculo.py:314](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:314), [app/ponto/management/commands/e6_oraculo.py:378](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:378), [app/ponto/management/commands/e6_oraculo.py:401](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:401)

- **Universo.** O comando seleciona empresas ativas e colaboradores com `FechamentoMensal` da competência; só aplica o filtro de entrada no TXT quando recebe `so_txt`. Os dois scripts fornecidos chamam sem esse filtro. A sonda intercepta a seleção e a divide por `pk % 6`; se as seis partes cobrem os resíduos e terminam, essa divisão cobre o mesmo universo de colaboradores. [app/ponto/management/commands/e6_oraculo.py:303](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:303); anexo da sonda, `filtrar_colaboradores` e chamada de `e6_oraculo` **[linha não verificada]**; anexo da lista, chamada de `e6_oraculo` **[linha não verificada]**.
- **Filtro de dias.** O mesmo laço percorre a janela da empresa e exclui, entre outros, dias sem linha de espelho, isentos ou com ausência, sem minutos disponíveis, sem trabalho em ambos e diferenças explicadas pela janela declarada. Diferenças de até 10 minutos também não entram como divergência. A opção CSV não altera esses filtros. [app/ponto/management/commands/e6_oraculo.py:331](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:331), [app/ponto/management/commands/e6_oraculo.py:342](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:342), [app/ponto/management/commands/e6_oraculo.py:365](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:365), [app/ponto/management/commands/e6_oraculo.py:442](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:442).
- **Contagem.** Para cada divergência julgada, o comando incrementa uma vez `T['dono_…']`, inclui o colaborador no conjunto do dono e acrescenta uma linha a `div` com esse mesmo dono. O CSV grava `div`; portanto, nesse caminho, uma linha `ESTRUTURA` corresponde a um incremento `ESTRUTURA`. Colaboradores são contados distintamente por conjunto, não por linha. [app/ponto/management/commands/e6_oraculo.py:378](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:378), [app/ponto/management/commands/e6_oraculo.py:401](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:401), [app/ponto/management/commands/e6_oraculo.py:433](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:433), [app/ponto/management/commands/e6_oraculo.py:442](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:442).
- **Caminhos de execução.** A sonda copia os acumuladores do próprio `handle`, mas também instala interceptações, limpa cache e envolve chamadas do espelho em transações com rollback; a lista chama o comando inteiro com CSV. Além disso, cada chamada do comando captura seu próprio instante e deriva dele `HOJE`; o mesmo dump, por si só, não comprova instantes ou resultados intermediários iguais. Se alguma dessas diferenças afetou esta medição, **[não verificado]**. Anexo da sonda, `copiar_acumuladores`, `limpar` e `observar_espelho` **[linha não verificada]**; [app/ponto/management/commands/e6_oraculo.py:284](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:284), [app/ponto/management/commands/e6_oraculo.py:316](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/ponto/management/commands/e6_oraculo.py:316).

**Autoridade:** a tabela e o CSV são duas saídas do mesmo julgamento do `e6_oraculo`; o código não estabelece que uma delas prevaleça sobre a outra quando vêm de execuções diferentes. O R1 em `placar_estrutural.py` declara como fonte medida arquivos `logs/r1remede/r1_dono_*.txt` e identifica a tabela do comando como fonte; o registro exibido no arquivo descreve uma medição anterior em seis partes, não uma leitura direta do CSV desta lista. [app/core/placar_estrutural.py:123](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/core/placar_estrutural.py:123), [app/core/placar_estrutural.py:130](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/core/placar_estrutural.py:130), [app/core/placar_estrutural.py:133](/home/ronald/codex_ws/4307c01e60f6a10ea878f26948f0f137894cca57/app/core/placar_estrutural.py:133).

A saída `AUTORIDADE` da execução da lista e os resultados individuais das seis partes não foram fornecidos; assim, não é possível conferir o contador da tabela contra o CSV na própria execução, nem reconciliar os colaboradores e dias entre execuções. **[não verificado]** Os scripts constam no texto fornecido, mas seus arquivos não estão nesta cópia; por isso não pude verificar seus números de linha. **[não verificado]**

## 18/11 05:55 · sessao b3b854808243 · R1-REMEDE-1811 · sonda · origem=Code

Transcrição do trecho `CAB` até `FIM_LISTA`, conforme o dado fornecido. Arquivo e linhas de origem: [não verificado].

```text
CAB emp|colab|dia|dono_da_divergencia|esp_min|piso_min|teto_min|dif|classe|paridade|dono_da_paridade|veredito
ESTRUTURA 3|110|2026-11-12|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|112|2026-10-21|ESTRUTURA|546|838|838|292|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|112|2026-10-22|ESTRUTURA|541|835|835|294|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|113|2026-10-30|ESTRUTURA|242|0|0|-242|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|113|2026-11-01|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|114|2026-11-08|ESTRUTURA|439|429|499|-10|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 3|114|2026-11-15|ESTRUTURA|499|440|499|-59|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 3|130|2026-10-29|ESTRUTURA|335|672|730|337|diverge_acima_60|par|1|trabalhou |n_batidas=4
ESTRUTURA 2|200|2026-10-29|ESTRUTURA|180|0|0|-180|e6_zero_esp_trabalho|par|3|em_aberto |n_batidas=0
ESTRUTURA 2|200|2026-11-02|ESTRUTURA|180|0|0|-180|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|218|2026-11-14|ESTRUTURA|103|0|0|-103|e6_zero_esp_trabalho|par|3|em_aberto |n_batidas=0
ESTRUTURA 2|220|2026-11-10|ESTRUTURA|337|430|486|93|diverge_acima_60|par|1|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-09|ESTRUTURA|551|597|610|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-10|ESTRUTURA|551|598|611|47|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-11|ESTRUTURA|550|598|611|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-13|ESTRUTURA|551|597|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-17|ESTRUTURA|553|598|612|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|296|2026-11-02|ESTRUTURA|345|0|0|-345|e6_zero_esp_trabalho|par|0|trabalhou |n_batidas=0
ESTRUTURA 2|334|2026-11-16|ESTRUTURA|209|591|683|382|diverge_acima_60|par|3|folga |n_batidas=4
ESTRUTURA 4|37|2026-11-14|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|382|2026-11-10|ESTRUTURA|418|0|0|-418|e6_zero_esp_trabalho|par|3|trabalhou |n_batidas=0
ESTRUTURA 2|516|2026-10-23|ESTRUTURA|361|0|0|-361|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|727|2026-11-10|ESTRUTURA|307|0|0|-307|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|841|2026-11-16|ESTRUTURA|178|0|0|-178|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|874|2026-10-22|ESTRUTURA|667|383|383|-284|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 2|884|2026-11-08|ESTRUTURA|679|699|739|20|diverge_10_60|par|4|rejeitado |n_batidas=4
ESTRUTURA 3|909|2026-11-11|ESTRUTURA|831|245|374|-586|diverge_acima_60/FORA|IMPAR|CORTE_do_oraculo|em_aberto |n_batidas=4
ESTRUTURA 2|922|2026-11-01|ESTRUTURA|1382|68|68|-1314|diverge_acima_60|par|4|trabalhou |n_batidas=2
ESTRUTURA 2|922|2026-11-04|ESTRUTURA|676|458|458|-218|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 1|950|2026-11-11|ESTRUTURA|480|240|240|-240|diverge_acima_60|par|4|trabalhou |n_batidas=2
ESTRUTURA 2|989|2026-10-23|ESTRUTURA|721|567|567|-154|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|996|2026-10-28|ESTRUTURA|0|580|580|580|esp_zero_e6_trabalho|par|1|folga |n_batidas=2
ESTRUTURA 3|996|2026-11-10|ESTRUTURA|443|1026|1116|583|diverge_acima_60|par|5|em_aberto |n_batidas=6
FIM_LISTA
```

## 18/11 05:50 · sessao d3575bb42232 · R1-REMEDE-1811 · sonda · origem=Code

As seis linhas `SOMA` terminam em `True`. Não há `INCOMPLETO` na saída fornecida. A verificação em arquivo é [não verificado].

```text
sonda_leitura: script.py [455c4e7660bc3a87] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 6 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 100
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 99
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 199 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 199 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 199 minutos_total 2.6 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1298, 'bate_ate_10min': 1258, 'esp_zero_e6_trabalho': 5, 'e6_zero_esp_trabalho': 2, 'diverge_10_60': 1, 'diverge_acima_60': 32, 'dia_batida_impar': 55, 'impar_que_BATE': 27, 'impar_que_DIVERGE': 28, 'impar_dentro_da_faixa': 25, 'impar_FORA_da_faixa': 3, 'impar_no_conjunto_do_dia': 28, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1396, 'dia_isento_ou_ausencia': 277, 'dia_sem_trabalho_ambos': 11, 'esp_desconta_janela_declarada': 18, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.9% de 1298 dias comparados
[3] ESTRUTURA     2 dia-colab       8.2 h    1 colabs
[3] CADASTRO      8 dia-colab      40.5 h    2 colabs
[3] BATIDA       30 dia-colab     127.8 h   17 colabs
[4] TOTAL divergentes 40
[5] colabs com divergencia: 20
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=2|horas=8.15|colabs=1
PARCIAL|10/2026|CADASTRO|dias=8|horas=40.4628|colabs=2
PARCIAL|10/2026|BATIDA|dias=30|horas=127.75368121722221|colabs=17
PARCIAL_TOTAL|10/2026|dias=40|horas=176.36648121722221|colabs=20
ESTIMATIVA processados 100 universo 199 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 125 universo 199 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 199 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 199 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1206, 'bate_ate_10min': 1163, 'esp_zero_e6_trabalho': 3, 'e6_zero_esp_trabalho': 4, 'diverge_10_60': 2, 'diverge_acima_60': 34, 'dia_batida_impar': 64, 'impar_que_BATE': 33, 'impar_que_DIVERGE': 31, 'impar_dentro_da_faixa': 26, 'impar_FORA_da_faixa': 5, 'impar_no_conjunto_do_dia': 30, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 198, 'dia_sem_lavratura_no_espelho': 1343, 'dia_isento_ou_ausencia': 288, 'dia_sem_trabalho_ambos': 23, 'esp_desconta_janela_declarada': 11, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.4% de 1206 dias comparados
[3] ESTRUTURA     3 dia-colab      10.6 h    2 colabs
[3] CADASTRO      7 dia-colab      65.8 h    5 colabs
[3] BATIDA       33 dia-colab     166.7 h   22 colabs
[4] TOTAL divergentes 43
[5] colabs com divergencia: 26
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=3|horas=10.610499166666667|colabs=2
PARCIAL|11/2026|CADASTRO|dias=7|horas=65.7971625|colabs=5
PARCIAL|11/2026|BATIDA|dias=33|horas=166.71064350722222|colabs=22
PARCIAL_TOTAL|11/2026|dias=43|horas=243.118305173888887|colabs=26
TEMPO_SEGUNDOS 162.1
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 241905
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 80
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1406
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 4830
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 58
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 10
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6629
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3848
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3848
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 626
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 180
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 455936
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 544
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3097
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 183
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 13056
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 93
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4326
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 189
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 4
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 21916
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 4418
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 39962
TRATADA_LOCAL app/ponto/turnos.py:2653 GeneratorExit 4
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 17580
SOMA 10/2026 40   <- tem de ser igual a soma das classes (40) True
SOMA 10/2026 HORAS 176.36648121722221 True
SOMA 10/2026 COLABS_UNIAO 20 True
SOMA 11/2026 43   <- tem de ser igual a soma das classes (43) True
SOMA 11/2026 HORAS 243.118305173888887 True
SOMA 11/2026 COLABS_UNIAO 26 True
rc_sombra=0
```

## 18/11 05:46 · sessao 6801b2f64f9b · R1-REMEDE-1811 · sonda · origem=Code

Transcrição da saída fornecida — origem e conteúdo independentes não verificados:

```text
sonda_leitura: script.py [972aacf9a2766647] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 5 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 107
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 103
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 210 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 210 minutos_total 2.9 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1285, 'bate_ate_10min': 1215, 'esp_zero_e6_trabalho': 9, 'e6_zero_esp_trabalho': 3, 'diverge_10_60': 3, 'diverge_acima_60': 55, 'dia_batida_impar': 78, 'impar_que_BATE': 29, 'impar_que_DIVERGE': 49, 'impar_dentro_da_faixa': 42, 'impar_FORA_da_faixa': 7, 'impar_no_conjunto_do_dia': 49, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1533, 'dia_isento_ou_ausencia': 275, 'dia_sem_trabalho_ambos': 45, 'esp_desconta_janela_declarada': 72, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.6% de 1285 dias comparados
[3] ESTRUTURA     3 dia-colab      29.2 h    3 colabs
[3] CADASTRO     19 dia-colab      52.3 h    8 colabs
[3] BATIDA       48 dia-colab     197.3 h   26 colabs
[4] TOTAL divergentes 70
[5] colabs com divergencia: 29
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=29.23270916666667|colabs=3
PARCIAL|10/2026|CADASTRO|dias=19|horas=52.325450786111105|colabs=8
PARCIAL|10/2026|BATIDA|dias=48|horas=197.29829774694443|colabs=26
PARCIAL_TOTAL|10/2026|dias=70|horas=278.856457699722205|colabs=29
ESTIMATIVA processados 125 universo 210 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 210 minutos_total 3.1 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1236, 'bate_ate_10min': 1146, 'esp_zero_e6_trabalho': 5, 'e6_zero_esp_trabalho': 4, 'diverge_10_60': 6, 'diverge_acima_60': 75, 'dia_batida_impar': 98, 'impar_que_BATE': 37, 'impar_que_DIVERGE': 61, 'impar_dentro_da_faixa': 52, 'impar_FORA_da_faixa': 9, 'impar_no_conjunto_do_dia': 61, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 206, 'dia_sem_lavratura_no_espelho': 1402, 'dia_isento_ou_ausencia': 227, 'dia_sem_trabalho_ambos': 38, 'esp_desconta_janela_declarada': 84, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 92.7% de 1236 dias comparados
[3] ESTRUTURA     9 dia-colab      60.5 h    7 colabs
[3] CADASTRO     31 dia-colab     104.1 h   11 colabs
[3] BATIDA       50 dia-colab     255.3 h   25 colabs
[4] TOTAL divergentes 90
[5] colabs com divergencia: 35
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=9|horas=60.54144059916667|colabs=7
PARCIAL|11/2026|CADASTRO|dias=31|horas=104.14722180555555|colabs=11
PARCIAL|11/2026|BATIDA|dias=50|horas=255.3167716316666|colabs=25
PARCIAL_TOTAL|11/2026|dias=90|horas=420.00543403638882|colabs=35
TEMPO_SEGUNDOS 184.8
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 278619
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 34
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1955
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 8224
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 80
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 14
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 8043
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 5767
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 5767
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 350
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 193
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 505992
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 861
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3482
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 218
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 14496
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 171
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4379
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 199
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 28919
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 3793
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 45361
TRATADA_LOCAL app/ponto/turnos.py:2653 GeneratorExit 4
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 21691
SOMA 10/2026 70   <- tem de ser igual a soma das classes (70) True
SOMA 10/2026 HORAS 278.856457699722205 True
SOMA 10/2026 COLABS_UNIAO 29 True
SOMA 11/2026 90   <- tem de ser igual a soma das classes (90) True
SOMA 11/2026 HORAS 420.00543403638882 True
SOMA 11/2026 COLABS_UNIAO 35 True
rc_sombra=0
```

As seis linhas **SOMA** estão marcadas como `True` no dado fornecido. Não aparece `INCOMPLETO` na saída fornecida. A execução e sua origem não foram verificadas independentemente. [não verificado]

## 18/11 05:42 · sessao b42c501a0c3b · R1-REMEDE-1811 · sonda · origem=Code

Saída da sonda, conforme o dado fornecido [nao verificado]:

```text
sonda_leitura: script.py [22b7b39d0d46bdd1] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 4 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 96
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 94
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 190 minutos_total 2.6 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 190 minutos_total 2.6 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1235, 'bate_ate_10min': 1188, 'esp_zero_e6_trabalho': 6, 'e6_zero_esp_trabalho': 1, 'diverge_10_60': 5, 'diverge_acima_60': 35, 'dia_batida_impar': 54, 'impar_que_BATE': 21, 'impar_que_DIVERGE': 33, 'impar_dentro_da_faixa': 29, 'impar_FORA_da_faixa': 4, 'impar_no_conjunto_do_dia': 32, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1275, 'dia_isento_ou_ausencia': 315, 'dia_sem_trabalho_ambos': 18, 'esp_desconta_janela_declarada': 37, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.2% de 1235 dias comparados
[3] ESTRUTURA     3 dia-colab      15.6 h    3 colabs
[3] CADASTRO     23 dia-colab     107.6 h    8 colabs
[3] BATIDA       21 dia-colab      97.0 h   16 colabs
[4] TOTAL divergentes 47
[5] colabs com divergencia: 25
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=15.566319135833336|colabs=3
PARCIAL|10/2026|CADASTRO|dias=23|horas=107.60888681222221|colabs=8
PARCIAL|10/2026|BATIDA|dias=21|horas=97.03832052916665|colabs=16
PARCIAL_TOTAL|10/2026|dias=47|horas=220.213526477222196|colabs=25
ESTIMATIVA processados 100 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 125 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 190 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 190 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1069, 'bate_ate_10min': 1036, 'esp_zero_e6_trabalho': 8, 'e6_zero_esp_trabalho': 0, 'diverge_10_60': 0, 'diverge_acima_60': 25, 'dia_batida_impar': 54, 'impar_que_BATE': 29, 'impar_que_DIVERGE': 25, 'impar_dentro_da_faixa': 21, 'impar_FORA_da_faixa': 4, 'impar_no_conjunto_do_dia': 24, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 188, 'dia_sem_lavratura_no_espelho': 1319, 'dia_isento_ou_ausencia': 280, 'dia_sem_trabalho_ambos': 27, 'esp_desconta_janela_declarada': 31, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.9% de 1069 dias comparados
[3] ESTRUTURA     1 dia-colab       9.8 h    1 colabs
[3] CADASTRO      4 dia-colab      16.4 h    2 colabs
[3] BATIDA       28 dia-colab     170.3 h   13 colabs
[4] TOTAL divergentes 33
[5] colabs com divergencia: 15
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=1|horas=9.770812222222222|colabs=1
PARCIAL|11/2026|CADASTRO|dias=4|horas=16.358279375833334|colabs=2
PARCIAL|11/2026|BATIDA|dias=28|horas=170.27799270138888|colabs=13
PARCIAL_TOTAL|11/2026|dias=33|horas=196.407084299444436|colabs=15
TEMPO_SEGUNDOS 162.6
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 244200
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 605
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1215
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6259
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 50
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 1
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 36
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 7089
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 8
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 3
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3868
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3868
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 344
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 171
TRATADA_LOCAL app/ponto/juiz_batida.py:191 GeneratorExit 1246
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 453258
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 653
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3025
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 192
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 13120
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 109
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4097
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 180
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 25134
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 3863
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 40470
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 19410
SOMA 10/2026 47   <- tem de ser igual a soma das classes (47) True
SOMA 10/2026 HORAS 220.213526477222196 True
SOMA 10/2026 COLABS_UNIAO 25 True
SOMA 11/2026 33   <- tem de ser igual a soma das classes (33) True
SOMA 11/2026 HORAS 196.407084299444436 True
SOMA 11/2026 COLABS_UNIAO 15 True
rc_sombra=0
```

As seis linhas **SOMA** estão marcadas como `True`. Não há ocorrência de `INCOMPLETO` na saída fornecida. [nao verificado]

## 18/11 05:39 · sessao 9e4f22a34972 · R1-REMEDE-1811 · sonda · origem=Code

Todas as seis linhas `SOMA` terminam em `True`. Não há `INCOMPLETO` no trecho fornecido. [nao verificado]

Abaixo está a saída fornecida para a parte 3 de 6, sem resumo; ela não tem arquivo e linha identificados no material recebido. [nao verificado]

```text
sonda_leitura: script.py [c3a2fcb8e125469d] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 3 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 102
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 100
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 202 minutos_total 3.3 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 202 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 202 minutos_total 2.8 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1236, 'bate_ate_10min': 1169, 'esp_zero_e6_trabalho': 12, 'e6_zero_esp_trabalho': 6, 'diverge_10_60': 2, 'diverge_acima_60': 47, 'dia_batida_impar': 65, 'impar_que_BATE': 23, 'impar_que_DIVERGE': 42, 'impar_dentro_da_faixa': 33, 'impar_FORA_da_faixa': 9, 'impar_no_conjunto_do_dia': 42, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1421, 'dia_isento_ou_ausencia': 342, 'dia_sem_trabalho_ambos': 22, 'esp_desconta_janela_declarada': 39, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.6% de 1236 dias comparados
[3] ESTRUTURA     3 dia-colab      15.2 h    3 colabs
[3] CADASTRO      7 dia-colab      35.9 h    4 colabs
[3] BATIDA       57 dia-colab     297.7 h   24 colabs
[4] TOTAL divergentes 67
[5] colabs com divergencia: 29
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=3|horas=15.209666666666665|colabs=3
PARCIAL|10/2026|CADASTRO|dias=7|horas=35.88209174|colabs=4
PARCIAL|10/2026|BATIDA|dias=57|horas=297.66026132194446|colabs=24
PARCIAL_TOTAL|10/2026|dias=67|horas=348.752019728611125|colabs=29
ESTIMATIVA processados 125 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 202 minutos_total 2.9 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1158, 'bate_ate_10min': 1099, 'esp_zero_e6_trabalho': 6, 'e6_zero_esp_trabalho': 7, 'diverge_10_60': 3, 'diverge_acima_60': 43, 'dia_batida_impar': 58, 'impar_que_BATE': 20, 'impar_que_DIVERGE': 38, 'impar_dentro_da_faixa': 30, 'impar_FORA_da_faixa': 8, 'impar_no_conjunto_do_dia': 37, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 200, 'dia_sem_lavratura_no_espelho': 1448, 'dia_isento_ou_ausencia': 217, 'dia_sem_trabalho_ambos': 24, 'esp_desconta_janela_declarada': 53, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.9% de 1158 dias comparados
[3] ESTRUTURA     7 dia-colab      21.8 h    6 colabs
[3] CADASTRO     10 dia-colab      35.9 h    6 colabs
[3] BATIDA       42 dia-colab     209.6 h   22 colabs
[4] TOTAL divergentes 59
[5] colabs com divergencia: 28
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=7|horas=21.8227|colabs=6
PARCIAL|11/2026|CADASTRO|dias=10|horas=35.92035111111112|colabs=6
PARCIAL|11/2026|BATIDA|dias=42|horas=209.5975820575|colabs=22
PARCIAL_TOTAL|11/2026|dias=59|horas=267.34063316861112|colabs=28
TEMPO_SEGUNDOS 172.0
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 261678
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 278
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1738
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6017
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 41
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6974
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 4738
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 4738
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 620
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 183
TRATADA_LOCAL app/ponto/juiz_batida.py:191 GeneratorExit 17
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 488349
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 550
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3269
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 176
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 13326
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 111
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4282
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 193
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 4
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 24759
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 4929
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 41466
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 18839
SOMA 10/2026 67   <- tem de ser igual a soma das classes (67) True
SOMA 10/2026 HORAS 348.752019728611125 True
SOMA 10/2026 COLABS_UNIAO 29 True
SOMA 11/2026 59   <- tem de ser igual a soma das classes (59) True
SOMA 11/2026 HORAS 267.34063316861112 True
SOMA 11/2026 COLABS_UNIAO 28 True
rc_sombra=0
```

## 18/11 05:35 · sessao b6b30e7671b2 · R1-REMEDE-1811 · sonda · origem=Code

As seis linhas `SOMA` terminam em `True`. Não aparece `INCOMPLETO` na saída fornecida. [não verificado: o dado fornecido não tem arquivo:linha]

```text
sonda_leitura: script.py [8f462593598e8306] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 2 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 109
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 102
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 211 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 211 minutos_total 2.9 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1268, 'bate_ate_10min': 1227, 'esp_zero_e6_trabalho': 2, 'e6_zero_esp_trabalho': 4, 'diverge_10_60': 7, 'diverge_acima_60': 28, 'dia_batida_impar': 63, 'impar_que_BATE': 33, 'impar_que_DIVERGE': 30, 'impar_dentro_da_faixa': 24, 'impar_FORA_da_faixa': 6, 'impar_no_conjunto_do_dia': 30, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1563, 'dia_isento_ou_ausencia': 353, 'dia_sem_trabalho_ambos': 24, 'esp_desconta_janela_declarada': 62, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.8% de 1268 dias comparados
[3] ESTRUTURA     5 dia-colab      11.1 h    3 colabs
[3] CADASTRO      8 dia-colab      20.4 h    6 colabs
[3] BATIDA       28 dia-colab     136.0 h   18 colabs
[4] TOTAL divergentes 41
[5] colabs com divergencia: 23
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=5|horas=11.084600277777778|colabs=3
PARCIAL|10/2026|CADASTRO|dias=8|horas=20.397520712222224|colabs=6
PARCIAL|10/2026|BATIDA|dias=28|horas=136.0465618775|colabs=18
PARCIAL_TOTAL|10/2026|dias=41|horas=167.528682867500002|colabs=23
ESTIMATIVA processados 125 universo 211 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 211 minutos_total 3.1 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 211 minutos_total 3.0 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1161, 'bate_ate_10min': 1098, 'esp_zero_e6_trabalho': 3, 'e6_zero_esp_trabalho': 4, 'diverge_10_60': 3, 'diverge_acima_60': 53, 'dia_batida_impar': 85, 'impar_que_BATE': 31, 'impar_que_DIVERGE': 54, 'impar_dentro_da_faixa': 49, 'impar_FORA_da_faixa': 5, 'impar_no_conjunto_do_dia': 53, 'impar_SO_POR_CORTE_do_oraculo': 1, 'dia_sem_linha_no_espelho': 204, 'dia_sem_lavratura_no_espelho': 1492, 'dia_isento_ou_ausencia': 220, 'dia_sem_trabalho_ambos': 27, 'esp_desconta_janela_declarada': 58, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 94.6% de 1161 dias comparados
[3] ESTRUTURA     3 dia-colab      12.1 h    3 colabs
[3] CADASTRO     16 dia-colab      52.5 h    7 colabs
[3] BATIDA       44 dia-colab     212.9 h   22 colabs
[4] TOTAL divergentes 63
[5] colabs com divergencia: 29
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=3|horas=12.1|colabs=3
PARCIAL|11/2026|CADASTRO|dias=16|horas=52.48679821444445|colabs=7
PARCIAL|11/2026|BATIDA|dias=44|horas=212.917450995|colabs=22
PARCIAL_TOTAL|11/2026|dias=63|horas=277.50424920944445|colabs=29
TEMPO_SEGUNDOS 179.9
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 277290
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 53
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1589
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6877
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 61
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:1687 GeneratorExit 8
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6800
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 6
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 4975
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 4975
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 617
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 187
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 503275
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 665
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3196
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 224
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 13694
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 117
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4084
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 200
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 2
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 26995
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 6185
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 41986
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 20189
SOMA 10/2026 41   <- tem de ser igual a soma das classes (41) True
SOMA 10/2026 HORAS 167.528682867500002 True
SOMA 10/2026 COLABS_UNIAO 23 True
SOMA 11/2026 63   <- tem de ser igual a soma das classes (63) True
SOMA 11/2026 HORAS 277.50424920944445 True
SOMA 11/2026 COLABS_UNIAO 29 True
rc_sombra=0
```

## 18/11 05:31 · sessao dcd0bcbc6659 · R1-REMEDE-1811 · sonda · origem=Code

Saída da sonda, parte 1 de 6, conforme o dado fornecido pelo usuário:

```text
sonda_leitura: script.py [27c01d2784902123] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE 1 PARTES 6
[0] UNIVERSO 10/2026 624
UNIVERSO_PARTE 10/2026 110
JANELA 10/2026 2026-09-21 2026-10-20
[0] UNIVERSO 11/2026 597
UNIVERSO_PARTE 11/2026 99
JANELA 11/2026 2026-10-21 2026-11-20
ESTIMATIVA processados 25 universo 209 minutos_total 2.6 ate_40_min True ate_15_min True
ESTIMATIVA processados 50 universo 209 minutos_total 2.9 ate_40_min True ate_15_min True
ESTIMATIVA processados 75 universo 209 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 100 universo 209 minutos_total 2.6 ate_40_min True ate_15_min True
COMPETENCIA 10/2026
[1] {'dias_comparados': 1181, 'bate_ate_10min': 1141, 'esp_zero_e6_trabalho': 1, 'e6_zero_esp_trabalho': 3, 'diverge_10_60': 11, 'diverge_acima_60': 25, 'dia_batida_impar': 33, 'impar_que_BATE': 15, 'impar_que_DIVERGE': 18, 'impar_dentro_da_faixa': 15, 'impar_FORA_da_faixa': 3, 'impar_no_conjunto_do_dia': 18, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 0, 'dia_sem_lavratura_no_espelho': 1726, 'dia_isento_ou_ausencia': 316, 'dia_sem_trabalho_ambos': 34, 'esp_desconta_janela_declarada': 43, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 96.6% de 1181 dias comparados
[3] ESTRUTURA    10 dia-colab       9.1 h    3 colabs
[3] CADASTRO     10 dia-colab      38.4 h    3 colabs
[3] BATIDA       20 dia-colab     110.2 h   13 colabs
[4] TOTAL divergentes 40
[5] colabs com divergencia: 17
erros_no_espelho 0
PARCIAL|10/2026|ESTRUTURA|dias=10|horas=9.116283817777775|colabs=3
PARCIAL|10/2026|CADASTRO|dias=10|horas=38.355213611111104|colabs=3
PARCIAL|10/2026|BATIDA|dias=20|horas=110.21011656222223|colabs=13
PARCIAL_TOTAL|10/2026|dias=40|horas=157.681613991111109|colabs=17
ESTIMATIVA processados 125 universo 209 minutos_total 2.6 ate_40_min True ate_15_min True
ESTIMATIVA processados 150 universo 209 minutos_total 2.7 ate_40_min True ate_15_min True
ESTIMATIVA processados 175 universo 209 minutos_total 2.8 ate_40_min True ate_15_min True
ESTIMATIVA processados 200 universo 209 minutos_total 2.7 ate_40_min True ate_15_min True
COMPETENCIA 11/2026
[1] {'dias_comparados': 1106, 'bate_ate_10min': 1039, 'esp_zero_e6_trabalho': 14, 'e6_zero_esp_trabalho': 6, 'diverge_10_60': 10, 'diverge_acima_60': 37, 'dia_batida_impar': 55, 'impar_que_BATE': 20, 'impar_que_DIVERGE': 35, 'impar_dentro_da_faixa': 25, 'impar_FORA_da_faixa': 10, 'impar_no_conjunto_do_dia': 35, 'impar_SO_POR_CORTE_do_oraculo': 0, 'dia_sem_linha_no_espelho': 198, 'dia_sem_lavratura_no_espelho': 1426, 'dia_isento_ou_ausencia': 261, 'dia_sem_trabalho_ambos': 40, 'esp_desconta_janela_declarada': 38, 'esp_lido_pelo_pago_h': 0}
[2] BATE (ate 10 min): 93.9% de 1106 dias comparados
[3] ESTRUTURA    10 dia-colab      30.4 h    4 colabs
[3] CADASTRO     14 dia-colab      30.4 h    4 colabs
[3] BATIDA       43 dia-colab     284.7 h   17 colabs
[4] TOTAL divergentes 67
[5] colabs com divergencia: 21
erros_no_espelho 0
PARCIAL|11/2026|ESTRUTURA|dias=10|horas=30.40733995027778|colabs=4
PARCIAL|11/2026|CADASTRO|dias=14|horas=30.370979816666665|colabs=4
PARCIAL|11/2026|BATIDA|dias=43|horas=284.70216034805554|colabs=17
PARCIAL_TOTAL|11/2026|dias=67|horas=345.480480114999985|colabs=21
TEMPO_SEGUNDOS 165.8
TRATADA_LOCAL app/escala/alimentacao.py:279 GeneratorExit 253969
TRATADA_LOCAL app/escala/models.py:1282 GeneratorExit 76
TRATADA_LOCAL app/escala/services/fase_12x36.py:114 GeneratorExit 1161
TRATADA_LOCAL app/escala/services/leitor_celula.py:25 ValueError 6244
TRATADA_LOCAL app/escala/services/leitor_celula.py:81 GeneratorExit 52
TRATADA_LOCAL app/escala/utils.py:1500 GeneratorExit 2
TRATADA_LOCAL app/escala/utils.py:1991 GeneratorExit 6196
TRATADA_LOCAL app/escala/utils.py:2033 GeneratorExit 3
TRATADA_LOCAL app/escala/utils.py:452 GeneratorExit 4
TRATADA_LOCAL app/escala/utils.py:524 GeneratorExit 3608
TRATADA_LOCAL app/escala/utils.py:525 GeneratorExit 3608
TRATADA_LOCAL app/escala/utils.py:888 GeneratorExit 623
TRATADA_LOCAL app/folha/causas.py:58 GeneratorExit 181
TRATADA_LOCAL app/ponto/juiz_batida.py:75 ValueError 469726
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:1131 GeneratorExit 513
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:2604 GeneratorExit 3021
TRATADA_LOCAL app/ponto/motor_calculo_v2.py:677 GeneratorExit 178
TRATADA_LOCAL app/ponto/services/dia_pago.py:892 GeneratorExit 12424
TRATADA_LOCAL app/ponto/services/espelho.py:473 GeneratorExit 124
TRATADA_LOCAL app/ponto/services/espelho.py:657 GeneratorExit 4109
TRATADA_LOCAL app/ponto/services/fechamento.py:1044 GeneratorExit 192
TRATADA_LOCAL app/ponto/services/fechamento.py:1045 _GradeSemCelula 5
TRATADA_LOCAL app/ponto/turnos.py:1295 GeneratorExit 23456
TRATADA_LOCAL app/ponto/turnos.py:1656 GeneratorExit 4859
TRATADA_LOCAL app/ponto/turnos.py:1699 GeneratorExit 38424
TRATADA_LOCAL app/ponto/turnos.py:2653 GeneratorExit 16
TRATADA_LOCAL app/ponto/turnos.py:679 GeneratorExit 16981
SOMA 10/2026 40   <- tem de ser igual a soma das classes (40) True
SOMA 10/2026 HORAS 157.681613991111109 True
SOMA 10/2026 COLABS_UNIAO 17 True
SOMA 11/2026 67   <- tem de ser igual a soma das classes (67) True
SOMA 11/2026 HORAS 345.480480114999985 True
SOMA 11/2026 COLABS_UNIAO 21 True
rc_sombra=0
```

As seis linhas `SOMA` exibem `True`. A palavra `INCOMPLETO` não aparece na saída fornecida. A saída original e a execução não foram verificadas independentemente. [não verificado]

## 18/11 01:56 · sessao 5b93856c6c89 · E4C-F1-B2b · leitura · origem=Code

A guarda atual não recebe um sinal geral de “humano” ou “sistema”: recebe perguntas e consulta apenas o estado da cerca. Fora dela retorna antes de consultar; dentro, usa o estado criado pelo abridor para acumular empresa, memo e vetos ([fio.py:1389](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1389), [fio.py:1402](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1402), [fio.py:1445](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1445)). **A matriz abaixo é parcial**: não concluí o rastreamento independente dos 55 sítios nem o censo exaustivo de escritores e leitores. Os pontos não demonstrados estão marcados como [não verificado].

## F0 e efeito de julgar sempre

| Questão | Leitura |
|---|---|
| Motivo escrito de F0 | O comentário diz que, fora de ato cercado, as portas não consultam para que desligamento e portas do admin sigam como estavam; associa isso ao aval item 4 e à medição prévia do desligamento. O mesmo comentário explica que a cerca passou a observar a escrita efetiva porque a previsão anterior tinha sobrealcance e subalcance ([fio.py:1371](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1371), [fio.py:1379](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1379)). |
| Remover apenas o `return` | Não implementa “julgar sempre”: fora da cerca, `estado` é `None`, mas o restante acessa `estado['empresa']`, `estado['memo']` e `estado['vetadas']`; uma pergunta com dias a julgar chega a esse acesso ([fio.py:1402](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1402), [fio.py:1418](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1418), [fio.py:1421](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1421)). |
| Recusa hipotética | Se também houver estado válido e a semântica atual de veto for aplicada fora da cerca, dia indecidível ou entregue causa exceção; não há ali partição por pergunta. O efeito no ato depende de cada chamador e de seus `atomic` e capturas ([fio.py:1358](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1358), [fio.py:1424](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1424)). |
| Custo sem recusa | Cada chamada que prosseguir até a guarda faz a consulta das perguntas gravadas; pode ainda consultar empresa e competência. Hoje o retorno fora da cerca precede essas consultas ([fio.py:1402](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1402), [fio.py:1409](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1409), [fio.py:1420](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1420)). O selo mede caminhos de `registrar_batida` e cobra tetos; há cenários com folga zero. Quanto uma implementação concreta acrescentaria e quais testes falhariam: **[não verificado]** ([test_selo_chokepoint_escrita.py:110](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/ponto/tests/test_selo_chokepoint_escrita.py:110), [test_selo_chokepoint_escrita.py:196](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [test_selo_chokepoint_escrita.py:288](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/ponto/tests/test_selo_chokepoint_escrita.py:288)). |

## Matriz: seis portas humanas do ACHADO

“Passa” significa que a escrita chega à chamada da guarda; **não significa que seja julgada hoje fora da cerca**. A coluna de recusa descreve a consequência condicional de aplicar ali o veto atual com estado válido; o efeito completo de cada cadeia está [não verificado] ([models.py:3555](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3555), [fio.py:1402](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1402)).

| Sítio | Classe | Passa pela porta? | Sinal disponível acima da porta | Quebra por recusa, se julgar sempre |
|---|---|---|---|---|
| P8 `recusar_pergunta` | ACHADO humano | Sim, no `save`; a reabertura usa também a porta de lote ([validacao.py:920](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:920), [models.py:3398](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3398)). | `autor`, `request`, motivo e ato `recusa` na função; não são argumentos da guarda ([validacao.py:858](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:858), [validacao.py:925](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:925)). | O veto no primeiro `save` impede a gravação do motivo e a reabertura subsequente dentro desse atomic ([validacao.py:920](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:920)). |
| P9 `conferir_data_do_fato` | ACHADO humano | Sim, `save` dos campos de conferência ([validacao.py:996](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:996)). | `autor`, `request`, justificativa; `autor` é atribuído a `data_conferida_por` antes do save ([validacao.py:957](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:957), [validacao.py:996](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:996)). | O veto impede a conferência e a trilha posterior daquela execução ([validacao.py:998](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:998)). |
| P16 `responder_pergunta_disputa_core` | ACHADO humano | Sim, no `save` da resposta; a cadeia posterior de ausência deve ser analisada separadamente ([disputa_emissao.py:1517](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/disputa_emissao.py:1517), [disputa_emissao.py:1553](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/disputa_emissao.py:1553)). | A função recebe `user`; a guarda não o recebe ([disputa_emissao.py:1439](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/disputa_emissao.py:1439), [fio.py:1389](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1389)). | O veto no `save` impede a resposta e os passos posteriores da função ([disputa_emissao.py:1522](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/disputa_emissao.py:1522)). |
| X4 `_aceitar_a_folga` | ACHADO humano | Sim, nas gravações de pergunta verificadas no caminho de aceite ([validacao.py:519](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:519), [models.py:3183](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3183)). | `autor` e `request`; o aceite é chamado antes da regra comum de entrega da validação ([validacao.py:470](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:470), [validacao.py:711](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:711), [validacao.py:740](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:740)). | No ramo “dia já é folga”, a recusa em uma dessas gravações interrompe o atomic do aceite; demais ramos **[não verificado]** ([validacao.py:511](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:511)). |
| A9b `admin_encerra` | ACHADO humano | Sim, no fechamento das perguntas vivas por `fechar_por_admin` ([models.py:2007](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:2007), [models.py:3183](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3183)). | `user`, motivo obrigatório e constante `FECHOU_ADMIN_ENCERRA` na pilha; não chegam como argumentos à guarda ([models.py:2032](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:2032)). | Exceção propagada no laço desfaz perguntas e fechamento da disputa no atomic; o código documenta essa unidade ([models.py:1994](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:1994), [models.py:2002](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:2002)). |
| X11 `declarar_hora_colab` | ACHADO humano | Sim, quando o reaponte ou a resposta efetivamente gravam ([declaracao_texto.py:365](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/declaracao_texto.py:365), [declaracao_texto.py:420](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/declaracao_texto.py:420)). | `user` e `request` na declaração; **o reaponte mostrado recebe pergunta, chamado e data, sem `user` ou origem** ([declaracao_texto.py:307](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/declaracao_texto.py:307), [declaracao_texto.py:371](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/declaracao_texto.py:371)). | Uma recusa no reaponte impede a costura; uma recusa no save da resposta impede a declaração dessa resposta. Propagação e rollback completos **[não verificado]** ([declaracao_texto.py:371](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/declaracao_texto.py:371), [declaracao_texto.py:420](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/declaracao_texto.py:420)). |

`user` ou `autor` na pilha, por si só, não prova que **cada escrita descendente** seja humana: o dado fornecido inclui atos de sistema disparados por atos humanos [não verificado independentemente]. Na assinatura atual da guarda, não existe parâmetro de origem; a presença de `_CERCA` identifica somente o escopo cercado ([fio.py:1381](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1381), [fio.py:1389](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1389)).

## Escritas que contornam a guarda

| Escrita | Campo e chamador de produção | Passa pela guarda? |
|---|---|---|
| B6 `bulk_update` | `veredito_lavrado`, `julgado_em`, `janela_expira_em`; chamado pelo comando de backfill ([veredito_lavrado.py:237](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/veredito_lavrado.py:237), [veredito_lavrado.py:250](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/veredito_lavrado.py:250), [backfill_veredito_lavrado.py:143](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/management/commands/backfill_veredito_lavrado.py:143)). | Não há chamada da guarda nessa função ([veredito_lavrado.py:240](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/veredito_lavrado.py:240)). |
| `descarimbar_em_lote` / `update` | Zera `materializada_em`, `materializada_por`, `via_resolucao` e grava `outros`; o próprio método chama a guarda antes do `update` ([models.py:3371](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3371), [models.py:3398](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3398), [models.py:3404](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3404)). | **Passa**, embora o `update` em si não chame `save`; fora da cerca a guarda retorna sem julgar ([models.py:3398](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3398), [fio.py:1402](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1402)). |
| `save(update_fields=...)` | P8 e P9 são exemplos ([validacao.py:922](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:922), [validacao.py:998](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/validacao.py:998)). | **Não pula essa guarda** em instância existente: a chamada antecede a análise de `update_fields`. Outro receiver pode filtrar `update_fields`, mas isso é distinto da guarda ([models.py:3562](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3562), [signals.py:257](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/signals.py:257)). |
| Inserção por `save` | A guarda só é chamada quando `_state.adding` é falso ([models.py:3562](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3562)). | Não passa; o dado fornecido declara inserção fora do alcance da cerca [não verificado independentemente quanto ao aval]. |
| Outros `update`, `bulk_create`, SQL cru e aliases de queryset | Censo completo, campos e existência ou zero de chamadores de produção **[não verificado]**. | **[não verificado]**. |

## Campos lidos pela guarda

A premissa precisa de um ajuste: a guarda lê diretamente `conversa` e `data_evento` da instância e da linha gravada; para empresa, percorre `disputa.colaborador_id`. **Ela não lê diretamente `chamado_destino` nem `materializada_em`** no corpo apresentado ([fio.py:1409](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1409), [fio.py:1412](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1412), [fio.py:1420](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1420)).

| Campo | Escritores e leitores localizados |
|---|---|
| `data_evento` | Escrita pela função de atribuição do dia e na criação/reuso; leitura pela guarda e por `_dia_de_turno_da_pergunta` ([fio.py:849](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:849), [models.py:2889](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:2889), [juizes.py:338](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/juizes.py:338), [fio.py:1409](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1409)). Lista completa **[não verificado]**. |
| `conversa` | `abrir` e `abrir_conversa` passam respectivamente falso e verdadeiro; há atribuição no caminho de reuso. A guarda lê tanto o valor da instância quanto o gravado ([models.py:2902](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:2902), [models.py:2914](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:2914), [models.py:3003](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3003), [fio.py:1414](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:1414)). Lista completa **[não verificado]**. |
| `chamado_destino` | Escritores localizados incluem reaponte e reuso; leitores completos **[não verificado]** ([fio.py:974](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/services/fio.py:974), [models.py:3100](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3100)). |
| `materializada_em` | Escrita no carimbo e zerada no descarimbo em lote; `fechar_por_admin` a lê na idempotência. Outros escritores e leitores **[não verificado]** ([models.py:3218](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3218), [models.py:3404](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3404), [models.py:3174](/home/ronald/codex_ws/c26d0941fcbef3265e7c5d35a6d1a354c59e6525/app/chamados/models.py:3174)). |

A matriz sítio a sítio restante da tabela fornecida, inclusive os efeitos por recusa em cada cadeia de sistema, permanece **[não verificado]**. O diretório disponibilizado não contém metadados Git; a confirmação independente do hash informado também permanece **[não verificado]**.

## 18/11 01:51 · sessao 8ee488ab7ddf · E4C-F1-B2 · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1 e item 3.**

Os anexos foram fornecidos como texto raspado, sem numeração de linhas. Ao citá-los, identifico o anexo e o item, mas a referência `arquivo:linha` correspondente fica **[não verificado]**. A cópia não contém metadados Git; portanto, o hash, a base e a afirmação de que os três arquivos do item 3 não foram alterados pela B2 ficam **[não verificado]**.

## Item 1 — DEVOLVER

**P1 — Premissas conferem parcialmente.** Existem os quatro abridores de cerca: retratação, HIST, beco e resolver ([reconciliador.py:553](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/reconciliador.py:553), [reconciliador.py:1701](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/reconciliador.py:1701), [becos.py:66](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/services/becos.py:66), [disputa_emissao.py:1912](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/services/disputa_emissao.py:1912)). A guarda está no `save` de pergunta existente e retorna sem consultar fora de cerca ([models.py:3562](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/models.py:3562), [fio.py:1396](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/services/fio.py:1396)).

Existem a partição do backfill por grupo, o veto e a reversão da migração por grupo, e o pré-veto do rechaveamento ([backfill_decisoes_pergunta.py:253](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/management/commands/backfill_decisoes_pergunta.py:253), [migrar_data_evento_dia_do_turno.py:308](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:308), [migrar_data_evento_dia_do_turno.py:508](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:508), [fio.py:1105](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/services/fio.py:1105)). O R4b já espera recusa e preservação do persistido; os tetos 30, 62, 224 e 288 estão declarados ([test_o261_c_viva_sem_chamado_do_dia.py:312](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:312), [test_selo_chokepoint_escrita.py:124](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/tests/test_selo_chokepoint_escrita.py:124), [test_selo_chokepoint_escrita.py:134](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/tests/test_selo_chokepoint_escrita.py:134), [test_selo_chokepoint_escrita.py:196](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [test_selo_chokepoint_escrita.py:210](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/tests/test_selo_chokepoint_escrita.py:210)). Execução e resultado desses testes nesta revisão: **[não verificado]**.

A docstring de `vigiar_escrita_de_perguntas` já diz que fora de ato cercado não consulta, mas não explicita a classe (ii) pendente; a de `executar_sob_cerca` descreve o ato cercado sem declarar essa limitação ([fio.py:1389](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/services/fio.py:1389), [fio.py:1425](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/services/fio.py:1425)). O R4b não nomeia o ramo HIST no nome nem na docstring, embora o cabeçalho do arquivo associe o cenário a HIST; esse mesmo cabeçalho ainda descreve a caracterização antiga, de costura permitida ([test_o261_c_viva_sem_chamado_do_dia.py:30](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:30), [test_o261_c_viva_sem_chamado_do_dia.py:312](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:312)).

Qual teste ou selo **presente nesta cópia** afirma cobertura total: **[não verificado]**. A tabela raspada nomeia `test_selo_b2_cerca_15_sitios.py`, mas esse arquivo não foi encontrado na cópia; sua presença e seu conteúdo aqui ficam **[não verificado]**. O teste de cerca presente descreve atos cercados e seus casos, não uma prova dos 39 sítios externos ([test_e4c_b2_cerca_retratar.py:4](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/tests/test_e4c_b2_cerca_retratar.py:4), [test_e4c_b2_cerca_retratar.py:502](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/tests/test_e4c_b2_cerca_retratar.py:502)).

**P2 — Colisão e exceção.** A regra raspada CERCA-B2-REGRAS-2 exige julgamento por item para atos de sistema da classe (ii) fora de cerca; o item 1 do aval raspado dispensa nominalmente os 39 sítios da tabela v7 **como condição do pouso da B2a**. A exceção cobre literalmente essa condição, não declara implementada a regra nos 39 sítios nem fecha a B2. Referências dos anexos: `cerca_b2_regras2_1845.txt`, item 2(ii), e `aval.txt`, item 1 — linhas **[não verificado]**. O histórico e a aplicação integral da regra de véspera: **[não verificado]**. [nao verificado]

**P3 — Condições ainda não fecham na árvore atual.** As duas docstrings ainda não dizem explicitamente que a classe (ii) fica para B2b; o R4b ainda não identifica HIST no nome ou na docstring, como o próprio aval exige ([fio.py:1389](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/services/fio.py:1389), [fio.py:1425](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/services/fio.py:1425), [test_o261_c_viva_sem_chamado_do_dia.py:312](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:312); `aval.txt`, item 1(a)-(b), linhas **[não verificado]**). TICKETS, BACKLOG, placar e execução do mutante solicitado: **[não verificado]**. O item 1 aceita expressamente a exceção da torneira para sua suíte; a exigência sem exceção pertence ao pronto do item 3, portanto não transforma, por si só, o pouso anterior da B2a em suíte sem exceção (`aval.txt`, itens 1 e 3, linhas **[não verificado]**).

**P4 — Exportada, dinheiro e `!`.** A unidade declarada é justamente a escrita de pergunta em competência entregue, e a cerca visa impedir a alteração nos atos cercados ([fio.py:1384](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/chamados/services/fio.py:1384); `aval.txt`, item 1, linhas **[não verificado]**). A alteração de gravado exportado exige `!` pela regra da cópia ([CLAUDE.md:265](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/CLAUDE.md:265)). Efeito concreto em dinheiro ou gravação exportada fora do declarado: **[não verificado]**.

## Item 3 — DEVOLVER

**P1 — O teto existe, com qualificação.** Os dois seletores de cadastro e o da sincronização filtram pela janela obtida de `_janela_default`; o vigia recebe `ini` e `fim` e filtra por eles, sendo chamado com a janela default no fluxo padrão ([cadastro_futuro.py:26](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/services/cadastro_futuro.py:26), [cadastro_futuro.py:57](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/services/cadastro_futuro.py:57), [folgas.py:619](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/escala/folgas.py:619), [vigia_cadastro_futuro.py:24](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/services/vigia_cadastro_futuro.py:24), [gerar_celulas.py:344](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/management/commands/gerar_celulas.py:344)). Esse fim é o **maior fim das competências correntes das empresas ativas**, não necessariamente o fim da empresa do vínculo ([gerar_celulas.py:290](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/management/commands/gerar_celulas.py:290)).

`estender_horizonte=False` existe na chamada por dia, assim como `substitui_dono=False` e o filtro que preserva outro dono quando a opção não é ativada ([cadastro_futuro.py:9](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/services/cadastro_futuro.py:9), [cadastro_futuro.py:37](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/services/cadastro_futuro.py:37), [cadastro_futuro.py:43](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/services/cadastro_futuro.py:43)). O censo raspado enumera 13 chamadores/eventos, mas **não encontrei um cadastro único de “13 eventos” no código**; existência de tal cadastro: **[não verificado]**. Há chamadores concretos, por exemplo alteração de vigência e execução de vínculo ([signals.py:80](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/escala/signals.py:80), [vinculo.py:335](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/colaboradores/services/vinculo.py:335)).

**P2 — Colisões.** O novo aval raspado substitui expressamente o “registrar, não construir” anterior; o texto integral do aval de 16/11 e suas demais condições: **[não verificado]** (`aval.txt`, item 3, linhas **[não verificado]**). A exceção nominal dos 39 sítios pertence ao item 1 e não dispensa, literalmente, as condições do item 3 (`aval.txt`, itens 1 e 3, linhas **[não verificado]**). O item 3 também condiciona pouso na janela de véspera à confirmação; a definição independente e integral dessa janela: **[não verificado]**. [nao verificado]

**P3 — Há condições que não fecham no literal.**

- A FONTE inclui `sincronizar_foto_futura_do_calendario`, cujo seletor limitado está na própria função; ela também materializa a foto mensal. A proibição de “tocar … foto de folgas” entra em tensão literal com alterar esse sítio para que os três leiam o novo alcance ([folgas.py:605](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/escala/folgas.py:605), [folgas.py:619](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/escala/folgas.py:619), [folgas.py:639](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/escala/folgas.py:639); `aval.txt`, item 3, linhas **[não verificado]**).
- “Não muda quem cria” pode descrever os criadores de células, mas não significa preservar a geradora gravada: a porta existente pode trocar `escala_geradora` ou limpá-la ([celula.py:251](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/portas/celula.py:251), [celula.py:297](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/portas/celula.py:297)). O próprio RED solicitado exige passagem ao novo dono (`aval.txt`, item 3, linhas **[não verificado]**).
- “Suite inteira verde de verdade, sem exceção” é condição posterior do item 3, enquanto o item 1 aceita a exceção da torneira. A ordem é possível como sequência, mas o verde da B2a com exceção **não satisfaz sozinho** o pronto do item 3 (`aval.txt`, itens 1 e 3, linhas **[não verificado]**). A situação efetiva da suíte: **[não verificado]**. [nao verificado]

**P4 — Sim, há alcance de exportada, dinheiro e dado de vínculo que precisa constar na leitura.** Uma data futura além da janela corrente pode pertencer a uma competência já exportada: a janela é calculada pela data e a enumeração de exportações não impõe limite temporal ([janelas.py:24](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/janelas.py:24), [gerar_celulas.py:170](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/management/commands/gerar_celulas.py:170)). A porta ordinária consulta as competências protegidas e pula esses dias ([celula.py:218](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/portas/celula.py:218), [celula.py:284](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/portas/celula.py:284)). Existência de caso exportado entre as células da sonda: **[não verificado]**.

A relavra pode gravar `escala_geradora` e DNA com vínculo; a própria autoridade de vínculo do dia trata a geradora da célula como carimbo ([celula.py:305](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/portas/celula.py:305), [alimentacao.py:228](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/escala/alimentacao.py:228)). **Logo, a relavra alcança dado de vínculo**, enquanto a regra escrita reserva dado de ESCALA ou VÍNCULO a `!`; o aval declara apenas DRY para o passivo e exige `!` posterior para seu apply ([CLAUDE.md:265](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/CLAUDE.md:265); `aval.txt`, item 3, linhas **[não verificado]**). O alcance ordinário futuro dos eventos, após o pouso, não está limitado ao DRY do passivo no texto fornecido — essa é a colisão que devolvo.

Além disso, a porta pode disparar recálculo financeiro por competência quando `recalculo` está ativo e há dias tocados ([celula.py:483](/home/ronald/codex_ws/85e90e266f731208ea2dbfec06af249f66a240af/app/ponto/portas/celula.py:483)). Valor ou movimento financeiro efetivo nos dados raspados: **[não verificado]**.

## 18/11 01:18 · sessao 2a3378709e24 · E4C-F1-B2 · verificador · origem=Code

**1 — PROCEDE.** P1: Os tetos atuais são 60, 28, 220 e 280; o teste do gate compara deltas, com limites 3 e 9, sem teto absoluto 1012 ([teste:124](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/ponto/tests/test_selo_chokepoint_escrita.py:124), [teste:134](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/ponto/tests/test_selo_chokepoint_escrita.py:134), [teste:196](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [teste:210](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/ponto/tests/test_selo_chokepoint_escrita.py:210), [teste:501](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/ponto/tests/test_selo_chokepoint_escrita.py:501)). O `atomic` envolve o ato; aninhado, usa o escopo externo ([fio.py:1425](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/chamados/services/fio.py:1425)). P2: a exceção nominal cobre literalmente o custo do par sem pergunta; a preservação do ato inteiro acompanha CERCA itens 2 e 5 [não verificado: anexos fornecidos sem linhas]. P3: `assertLessEqual` não exige igualdade; a medição exata e seu log são uma condição adicional do aval, ainda [não verificado] ([teste:287](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/ponto/tests/test_selo_chokepoint_escrita.py:287)). P4: alteração proposta restrita aos tetos e justificativas; toque em exportada, dinheiro ou `!` fora do declarado [não verificado].

**2 — NÃO PROCEDE.** P1: Os dois testes existem com os nomes exatos ([teste:68](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/escala/tests/test_torneira_folgas.py:68), [teste:87](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/escala/tests/test_torneira_folgas.py:87)). P2: a exceção declara nominalmente as duas regras de suíte e limita testes e prazo [não verificado: aval fornecido sem linhas]. P3: o log fornecido contém **apenas dois testes**, sem identificar data ou commit; não comprova o conjunto de falhas da suíte inteira do MAIN, nem que `4307c01e6` seja a base da obra [não verificado]. P4: o item altera o critério de aceite, sem propor alteração dos testes; efeito fora do declarado [não verificado].

**3 — NÃO PROCEDE.** P1: A linha de destino existe e já descreve o resíduo além da competência ([BACKLOG.md:2705](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/docs/BACKLOG.md:2705)). P2: colisão com os avais citados [não verificado]. P3: a sonda fornecida é um retrato de 18/11, com células e contagens; não demonstra que a suíte fica vermelha nos três últimos dias de **toda** competência [não verificado]. P4: o item determina registro, não cura; toque fora do declarado [não verificado].

**4 — PROCEDE.** P1: A cópia registra que faltava o smoke no pouso ([RELATO.md:6](/home/ronald/codex_ws/8adeb6cf64e2e14dfd7666186e5057c3e3f98e9c/app/docs/RELATO.md:6)); a realização e os detalhes do smoke constam do relato fornecido, sem confirmação independente [não verificado]. P2: colisão com os avais citados [não verificado]. P3: nenhuma incompatibilidade literal demonstrada; a correspondência do relato com a tela permanece [não verificado]. P4: o item manda registrar uma observação sem botão acionado; alteração de exportada, dinheiro ou `!` fora do declarado [não verificado].

## 18/11 01:09 · sessao 82e1997623f9 · E4C-F1-B2 · verificador · origem=Code

**Veredito geral: NÃO PROCEDE como validação completa do delta.** Há testes com asserções úteis sobre estado persistido, e o mutante AST de S06 é detectado. Permanecem, porém, uma fixture cujo dia diverge do contexto, dependência de relógio real, recusa que pode passar por outro motivo e um caminho de costura fora da cerca que R4b não exercita. A execução dos testes com banco e dos mutantes do veto ficou **[não verificado]**. [nao verificado]

A cópia recebida não contém metadados Git; por isso, a identidade do commit e a completude do diff fornecido não puderam ser confirmadas **[não verificado]**. [nao verificado]

1. **Dias das fixtures — NÃO PROCEDE integralmente.**

   O juiz `_dia_de_turno_da_pergunta` devolve o próprio `data_evento` para pergunta que não é conversa; a vigia consulta esse valor projetado e o valor anteriormente gravado. Assim, concordância com ambos não demonstra, por si só, que o dia escolhido corresponde ao turno real. [app/chamados/juizes.py:338](app/chamados/juizes.py#L338); [app/chamados/services/fio.py:1389](app/chamados/services/fio.py#L1389).

   - **O168:** grava `_DIA = 26/05/2026`, também presente no contexto; o teste de cascata cria uma batida nesse dia. A equivalência para todos os casos da classe e a derivação por um turno real não foram executadas **[não verificado]**. [app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:29](app/chamados/tests/test_o168_pai_com_conversa_no_gate.py#L29); [app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:42](app/chamados/tests/test_o168_pai_com_conversa_no_gate.py#L42); [app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:132](app/chamados/tests/test_o168_pai_com_conversa_no_gate.py#L132).
   - **PEP, inclusive as duas irmãs:** grava 26/05/2026, igual ao contexto da disputa; a classe congela o relógio. Isso é coerência com o cenário, não uma prova independente da geometria do turno. [app/chamados/tests/test_pep_reentrada.py:32](app/chamados/tests/test_pep_reentrada.py#L32); [app/chamados/tests/test_pep_reentrada.py:44](app/chamados/tests/test_pep_reentrada.py#L44); [app/chamados/tests/test_pep_reentrada.py:263](app/chamados/tests/test_pep_reentrada.py#L263); [app/chamados/tests/test_pep_reentrada.py:295](app/chamados/tests/test_pep_reentrada.py#L295).
   - **Caso 06:** grava `self.ontem`, também usado no contexto, no chamado histórico e na célula; o valor depende de `localdate()` real. [app/chamados/tests/test_reconciliar_pergunta_orfa.py:27](app/chamados/tests/test_reconciliar_pergunta_orfa.py#L27); [app/chamados/tests/test_reconciliar_pergunta_orfa.py:114](app/chamados/tests/test_reconciliar_pergunta_orfa.py#L114).
   - **S128 de resposta não fantasma:** grava **20/05/2026** e uma célula dessa data, mas a disputa usa como contexto `timezone.now() - 5 dias`. Fora de uma data específica de execução, contexto e dia gravado divergem; o juiz e a vigia ainda aceitarão o dia gravado como projeção. Não há evidência de que 20/05 seja o dia de turno do fato representado pelo contexto móvel **[não verificado]**. [app/chamados/tests/test_resposta_nao_e_fantasma.py:38](app/chamados/tests/test_resposta_nao_e_fantasma.py#L38); [app/chamados/tests/test_resposta_nao_e_fantasma.py:45](app/chamados/tests/test_resposta_nao_e_fantasma.py#L45); [app/chamados/tests/test_resposta_nao_e_fantasma.py:120](app/chamados/tests/test_resposta_nao_e_fantasma.py#L120).

   Em produção, `abrir` exige dia de turno; nascimento, costura de linha sem dia e rechaveamento passam por `gravar_dia_do_fato`. Conversa usa o mesmo campo para **dia civil**, e o juiz a exclui da leitura de dia de turno. [app/chamados/models.py:2890](app/chamados/models.py#L2890); [app/chamados/models.py:2907](app/chamados/models.py#L2907); [app/chamados/models.py:3131](app/chamados/models.py#L3131); [app/chamados/services/fio.py:839](app/chamados/services/fio.py#L839); [app/chamados/services/fio.py:916](app/chamados/services/fio.py#L916); [app/chamados/services/fio.py:1086](app/chamados/services/fio.py#L1086).

2. **Gêmeos de recusa — NÃO PROCEDE como prova completa de mordida.**

   Os quatro gêmeos de dia nulo executam um `save` real dentro da cerca e comparam a linha persistida inteira. Como o veto retorna `None` para dia nulo e a vigia só permite o resultado literalmente `False`, as asserções de lista de empresas vetadas distinguem, pela leitura do código, tanto `veto_de_competencia -> False` quanto uma alteração que permita especificamente `None`. A execução desses mutantes está **[não verificado]**. [app/chamados/services/fio.py:1358](app/chamados/services/fio.py#L1358); [app/chamados/services/fio.py:1418](app/chamados/services/fio.py#L1418); [app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:108](app/chamados/tests/test_o168_pai_com_conversa_no_gate.py#L108); [app/chamados/tests/test_pep_reentrada.py:230](app/chamados/tests/test_pep_reentrada.py#L230); [app/chamados/tests/test_reconciliar_pergunta_orfa.py:96](app/chamados/tests/test_reconciliar_pergunta_orfa.py#L96); [app/chamados/tests/test_resposta_nao_e_fantasma.py:102](app/chamados/tests/test_resposta_nao_e_fantasma.py#L102).

   Uma exceção diferente **antes** de registrar empresa vetada sobe, em vez de satisfazer o teste; depois de registrada uma empresa, `executar_sob_cerca` captura qualquer exceção e devolve a recusa. [app/chamados/services/fio.py:1438](app/chamados/services/fio.py#L1438).

   O gêmeo HIST exportado verifica objeto recarregado, porém aceita qualquer `False` anterior à escrita: não afirma o motivo da recusa nem demonstra, nesse ramo exportado, que a escrita foi alcançada. O controle aberto força pergunta morta, enquanto o exportado não força esse mesmo estado. Portanto, sua mordida específica no mutante do veto é **[não verificado]**. [app/chamados/tests/test_e1_fila_v2_2_integracao.py:97](app/chamados/tests/test_e1_fila_v2_2_integracao.py#L97); [app/chamados/reconciliador.py:1689](app/chamados/reconciliador.py#L1689).

   Os casos exportados têm dia definido: eles não testam o mutante que altera apenas o tratamento de `None`. R4b também aceita `False` por outro motivo anterior à costura; sua mordida no mutante do veto é **[não verificado]**. [app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:109](app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py#L109); [app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:312](app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py#L312).

3. **Intenções originais — PROCEDE quanto às asserções preservadas, com limites de cenário.**

   - **Ordem de travas:** o caso aberto exige costura real e compara as linhas acessadas com o topo e a ordem de aquisição; a pergunta morta e outras decisões são impostas por mocks, portanto a prova é daquela costura sob essas premissas. [app/chamados/tests/test_e1_fila_v2_2_integracao.py:76](app/chamados/tests/test_e1_fila_v2_2_integracao.py#L76); [app/chamados/tests/test_e1_fila_v2_2_integracao.py:100](app/chamados/tests/test_e1_fila_v2_2_integracao.py#L100).
   - **Carimbo fora de voo:** o controle exige uma pergunta resolvida e carimbada; o caso em voo exige P sem carimbo e irmã persistida com carimbo. [app/chamados/tests/test_pep_reentrada.py:249](app/chamados/tests/test_pep_reentrada.py#L249); [app/chamados/tests/test_pep_reentrada.py:269](app/chamados/tests/test_pep_reentrada.py#L269).
   - **S128:** permanece o `refresh_from_db()` seguido de asserção de carimbo, mas a divergência de datas apontada no item 1 limita o cenário. [app/chamados/tests/test_resposta_nao_e_fantasma.py:120](app/chamados/tests/test_resposta_nao_e_fantasma.py#L120).
   - **Costura histórica 06:** ainda exige contagem 1, destino histórico e carimbo persistidos; agora a pergunta já chega datada, de modo que o teste não prova mais a datação de pergunta sem dia durante a costura. [app/chamados/tests/test_reconciliar_pergunta_orfa.py:124](app/chamados/tests/test_reconciliar_pergunta_orfa.py#L124); [app/chamados/tests/test_reconciliar_pergunta_orfa.py:128](app/chamados/tests/test_reconciliar_pergunta_orfa.py#L128).
   - **Fecho do pai O168:** os asserts continuam exigindo disputa fechada, pai fora dos vivos e ausência de retenção; há também cenário que exige pai vivo e retenção quando existe outra resposta sem veredito. [app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:126](app/chamados/tests/test_o168_pai_com_conversa_no_gate.py#L126); [app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:168](app/chamados/tests/test_o168_pai_com_conversa_no_gate.py#L168).

4. **R4b invertido — PROCEDE para o persistido observado; NÃO PROCEDE como cobertura de todos os escritores.**

   O teste recarrega a pergunta e exige destino e carimbo nulos, igualdade integral da linha da pergunta e do chamado encerrado, e mesmo conjunto de chamados do colaborador. [app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:316](app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py#L316).

   A costura HIST fica dentro de `executar_sob_cerca`, mas o ramo em que `_derivar_chamado_destino` encontra chamado chama `reapontar_resolvedora` diretamente, sem abrir cerca naquele trecho. A vigia é no-op fora de uma cerca aberta. R4b constrói o caso de encerrado histórico e não prova o que ocorre nesse outro ramo quando não houver cerca externa. [app/chamados/reconciliador.py:1601](app/chamados/reconciliador.py#L1601); [app/chamados/reconciliador.py:1690](app/chamados/reconciliador.py#L1690); [app/chamados/reconciliador.py:1707](app/chamados/reconciliador.py#L1707); [app/chamados/services/fio.py:1396](app/chamados/services/fio.py#L1396).

   O texto fornecido condiciona a inversão à contagem de perguntas vivas sem chamado em competência entregue; essa contagem e o conteúdo da resposta posterior citada na docstring não foram fornecidos **[não verificado]**. [app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:312](app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py#L312).

5. **S06 — PROCEDE para o sítio e o mutante AST; há mudança pontual no conjunto aceito.**

   A busca por função e texto encontra a chamada em `_ato_da_retratacao`, linha 567. Executei, sem escrita de bytecode, o localizador e o censo AST sobre o mutante cadastrado: ele encontrou a violação `reconciliar` nesse sítio. A execução ORM do mutante está **[não verificado]**. [app/chamados/tests/e4b_c12_sitios.json:43](app/chamados/tests/e4b_c12_sitios.json#L43); [app/chamados/tests/e4b_c12_verificar.py:84](app/chamados/tests/e4b_c12_verificar.py#L84); [app/chamados/reconciliador.py:563](app/chamados/reconciliador.py#L563).

   As duas alterações em `CORPO` mudam identidades exatas de arquivo, função, linha e forma aceitas pelo censo: passam a aceitar as novas linhas e deixam de aceitar as antigas. Não constituem uma permissão geral para outros sítios, mas qualquer chamada que coincida exatamente com uma dessas novas tuplas é excluída das violações; a regra de correspondência permanece a mesma. [app/chamados/tests/e4b_c12_ast.py:16](app/chamados/tests/e4b_c12_ast.py#L16); [app/chamados/tests/e4b_c12_ast.py:221](app/chamados/tests/e4b_c12_ast.py#L221).

6. **Classes já reprovadas — NÃO PROCEDE integralmente.**

   - **RED sem mordida:** a mordida dos gêmeos de dia nulo decorre das asserções e do fluxo lido, mas a execução dos mutantes está **[não verificado]**; HIST exportado e R4b admitem recusa anterior à escrita, como no item 2. [app/chamados/tests/test_e1_fila_v2_2_integracao.py:111](app/chamados/tests/test_e1_fila_v2_2_integracao.py#L111); [app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:320](app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py#L320).
   - **Dois significados:** `data_evento` continua significando dia de turno para fato e dia civil para conversa, discriminados por `conversa`; não se deve tratar a igualdade com o juiz como validação independente do dia. [app/chamados/models.py:2896](app/chamados/models.py#L2896); [app/chamados/models.py:2907](app/chamados/models.py#L2907); [app/chamados/juizes.py:338](app/chamados/juizes.py#L338).
   - **Mock versus comportamento:** os controles PEP consultam persistência além do espião; no laço sem credor o juiz de vida é mockado, limite declarado pelo próprio teste. O caso de travas também força decisões com mocks, mas afirma costura persistida. [app/chamados/tests/test_pep_reentrada.py:282](app/chamados/tests/test_pep_reentrada.py#L282); [app/chamados/tests/test_pep_reentrada.py:310](app/chamados/tests/test_pep_reentrada.py#L310); [app/chamados/tests/test_e1_fila_v2_2_integracao.py:100](app/chamados/tests/test_e1_fila_v2_2_integracao.py#L100).
   - **Relógio real:** caso 06 usa `localdate()` e o teste S128 usa `timezone.now()` para o contexto, sem data fixa nesses trechos. [app/chamados/tests/test_reconciliar_pergunta_orfa.py:27](app/chamados/tests/test_reconciliar_pergunta_orfa.py#L27); [app/chamados/tests/test_resposta_nao_e_fantasma.py:38](app/chamados/tests/test_resposta_nao_e_fantasma.py#L38).

Quanto aos campos restantes: `materializada_em` é atribuído pela porta de carimbo e pode ser limpo em lote; o juiz de vida e a materialização o leem. `chamado_destino` é escrito na costura e no nascimento/reuso, e é lido, entre outros, pelo juiz e pelo materializador. `status_local` é atribuído e salvo pelo caminho de transição do chamado; a guarda da costura lê o estado persistido do destino. Estes são os caminhos relevantes verificados, não um censo exaustivo de todos os leitores e escritores **[não verificado]**. [app/chamados/models.py:3192](app/chamados/models.py#L3192); [app/chamados/models.py:3216](app/chamados/models.py#L3216); [app/chamados/models.py:3404](app/chamados/models.py#L3404); [app/chamados/juizes.py:826](app/chamados/juizes.py#L826); [app/chamados/services/materializacao.py:608](app/chamados/services/materializacao.py#L608); [app/chamados/services/fio.py:974](app/chamados/services/fio.py#L974); [app/chamados/models.py:3132](app/chamados/models.py#L3132); [app/chamados/models.py:228](app/chamados/models.py#L228); [app/chamados/models.py:296](app/chamados/models.py#L296); [app/chamados/services/fio.py:805](app/chamados/services/fio.py#L805).

_6784 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
