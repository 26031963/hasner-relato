# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 14:05** · publicado em **13/11/2026 14:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 14:05 · sessao 97c1ef4ca10a · BUG-PAR-DESCONGELA · sonda · origem=Code

Não executei `sonda60.py`: o anexo contém a saída, sem o código da sonda. [nao verificado] O wrapper disponível escreve arquivos, contrariando a regra de somente leitura: [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/bin/sonda_leitura.sh:126).

Reprodução integral da saída fornecida, sem verificação em produção. [nao verificado]

```text
sonda_leitura: sonda60.py [4a6d0ca12cc5872c] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO PARIDADE 244 colabs 82
('carimbo', 'nulo') 60
('carimbo', 'via=chamado_encerrado') 12
('carimbo', 'via=declaracao') 4
('carimbo', 'via=paridade_recusada') 168
('comp', '07/2026') 4
('comp', '08/2026') 28
('comp', '09/2026') 85
('comp', '10/2026') 96
('comp', '11/2026') 31
('nulo', 'nao_validada', 'irma_ausente', 'disputa_aberta') 8
('nulo', 'nao_validada', 'irma_sem_carimbo_resp', 'disputa_aberta') 47
('nulo', 'nao_validada', 'irma_via=declaracao', 'disputa_aberta') 2
('nulo', 'nao_validada', 'irma_via=paridade_recusada', 'disputa_aberta') 3
('via=chamado_encerrado', 'validada', 'irma_via=chamado_encerrado', 'disputa_aberta') 3
('via=chamado_encerrado', 'validada', 'irma_via=chamado_encerrado', 'disputa_fechada') 8
('via=chamado_encerrado', 'validada', 'irma_via=declaracao', 'disputa_fechada') 1
('via=declaracao', 'validada', 'irma_sem_carimbo_semresp', 'disputa_aberta') 1
('via=declaracao', 'validada', 'irma_via=declaracao', 'disputa_aberta') 3
('via=paridade_recusada', 'validada', 'irma_ausente', 'disputa_aberta') 43
('via=paridade_recusada', 'validada', 'irma_ausente', 'disputa_fechada') 25
('via=paridade_recusada', 'validada', 'irma_sem_carimbo_resp', 'disputa_aberta') 5
('via=paridade_recusada', 'validada', 'irma_sem_carimbo_resp', 'disputa_fechada') 1
('via=paridade_recusada', 'validada', 'irma_sem_carimbo_semresp', 'disputa_aberta') 1
('via=paridade_recusada', 'validada', 'irma_via=admin_decidiu', 'disputa_fechada') 1
('via=paridade_recusada', 'validada', 'irma_via=chamado_encerrado', 'disputa_aberta') 2
('via=paridade_recusada', 'validada', 'irma_via=chamado_encerrado', 'disputa_fechada') 4
('via=paridade_recusada', 'validada', 'irma_via=competencia_trancada', 'disputa_aberta') 1
('via=paridade_recusada', 'validada', 'irma_via=completude_turno', 'disputa_aberta') 4
('via=paridade_recusada', 'validada', 'irma_via=completude_turno', 'disputa_fechada') 2
('via=paridade_recusada', 'validada', 'irma_via=declaracao', 'disputa_aberta') 14
('via=paridade_recusada', 'validada', 'irma_via=declaracao', 'disputa_fechada') 12
('via=paridade_recusada', 'validada', 'irma_via=duplicata', 'disputa_aberta') 2
('via=paridade_recusada', 'validada', 'irma_via=duplicata', 'disputa_fechada') 8
('via=paridade_recusada', 'validada', 'irma_via=juiz_aprovou', 'disputa_fechada') 1
('via=paridade_recusada', 'validada', 'irma_via=paridade_recusada', 'disputa_aberta') 22
('via=paridade_recusada', 'validada', 'irma_via=paridade_recusada', 'disputa_fechada') 20
COLABS_POR_CARIMBO {'via=paridade_recusada': 70, 'via=declaracao': 4, 'via=chamado_encerrado': 3, 'nulo': 18}
PKS {"('via=paridade_recusada', 'validada', 'irma_via=completude_turno', 'disputa_fechada')": [4248, 6228], "('via=paridade_recusada', 'validada', 'irma_via=completude_turno', 'disputa_aberta')": [4695, 6904, 7463, 9485], "('via=paridade_recusada', 'validada', 'irma_via=paridade_recusada', 'disputa_fechada')": [4797, 4798, 15994, 15995, 27136, 27137, 27667, 27668, 27727, 27728, 30009, 30010, 31342, 31343, 32645, 32646, 34259, 34400, 34719, 34749], "('via=paridade_recusada', 'validada', 'irma_via=chamado_encerrado', 'disputa_fechada')": [4800, 15482, 33308, 34130], "('via=paridade_recusada', 'validada', 'irma_via=duplicata', 'disputa_aberta')": [5040, 8778], "('via=paridade_recusada', 'validada', 'irma_ausente', 'disputa_fechada')": [5346, 6293, 10202, 12771, 13033, 14953, 27039, 27209, 27210, 27211, 27328, 27331, 27332, 27464, 27679, 33282, 33516, 34763, 36577, 37859, 38529, 39768, 40197, 46163, 49707], "('via=paridade_recusada', 'validada', 'irma_ausente', 'disputa_aberta')": [5367, 22627, 26782, 26963, 26984, 27109, 27110, 27112, 27160, 27427, 27431, 27433, 27435, 27437, 27439, 27449, 27452, 27461, 27680, 27690, 27798, 29469, 31373, 32649, 33034, 33097, 34853, 35676, 35727, 36077, 36567, 37961, 38289, 38647, 38674, 38950, 39759, 39766, 41733, 41988, 42762, 49608, 55681], "('via=paridade_recusada', 'validada', 'irma_via=duplicata', 'disputa_fechada')": [12770, 14356, 14359, 14363, 14365, 14368, 14960, 14963], "('via=paridade_recusada', 'validada', 'irma_via=juiz_aprovou', 'disputa_fechada')": [14874], "('via=paridade_recusada', 'validada', 'irma_via=declaracao', 'disputa_aberta')": [15014, 20890, 22586, 29419, 29929, 32358, 34303, 34792, 37398, 43728, 44107, 44525, 45737, 54942], "('via=paridade_recusada', 'validada', 'irma_via=competencia_trancada', 'disputa_aberta')": [15285], "('via=paridade_recusada', 'validada', 'irma_via=declaracao', 'disputa_fechada')": [15723, 16327, 22022, 22470, 26595, 31718, 31937, 31941, 35301, 35407, 37945, 38433], "('via=paridade_recusada', 'validada', 'irma_via=admin_decidiu', 'disputa_fechada')": [20960], "('via=paridade_recusada', 'validada', 'irma_via=chamado_encerrado', 'disputa_aberta')": [23137, 33760], "('via=paridade_recusada', 'validada', 'irma_sem_carimbo_semresp', 'disputa_aberta')": [23347], "('via=paridade_recusada', 'validada', 'irma_via=paridade_recusada', 'disputa_aberta')": [26588, 26589, 27450, 27451, 27560, 27561, 27673, 27674, 30011, 30012, 32914, 32915, 40850, 40851, 43255, 43256, 49154, 49155, 55437, 55438, 56214, 56260], "('via=paridade_recusada', 'validada', 'irma_sem_carimbo_resp', 'disputa_fechada')": [27135], "('via=paridade_recusada', 'validada', 'irma_sem_carimbo_resp', 'disputa_aberta')": [32716, 41705, 44362, 45399, 54689], "('via=declaracao', 'validada', 'irma_via=declaracao', 'disputa_aberta')": [32764, 53456, 54585], "('via=chamado_encerrado', 'validada', 'irma_via=chamado_encerrado', 'disputa_aberta')": [34744, 34745, 36609], "('nulo', 'nao_validada', 'irma_sem_carimbo_resp', 'disputa_aberta')": [36255, 41692, 41693, 45277, 45278, 45598, 45599, 47207, 47208, 49345, 49346, 49731, 49732, 50176, 50177, 51522, 51523, 51524, 51525, 51526, 51527, 51528, 51529, 51530, 51531, 51532, 51533, 51534, 51535, 51536, 51537, 51538, 51539, 51540, 51541, 51542, 51543, 51544, 51545, 51546, 51547, 51548, 51549, 51550, 51551, 55142, 55156], "('via=chamado_encerrado', 'validada', 'irma_via=chamado_encerrado', 'disputa_fechada')": [38260, 38261, 38268, 38269, 38863, 38864, 39564, 39565], "('via=chamado_encerrado', 'validada', 'irma_via=declaracao', 'disputa_fechada')": [38264], "('nulo', 'nao_validada', 'irma_ausente', 'disputa_aberta')": [39363, 46363, 47492, 48611, 49165, 49167, 49170, 56382], "('nulo', 'nao_validada', 'irma_via=declaracao', 'disputa_aberta')": [39872, 51739], "('nulo', 'nao_validada', 'irma_via=paridade_recusada', 'disputa_aberta')": [41706, 44363, 45400], "('via=declaracao', 'validada', 'irma_sem_carimbo_semresp', 'disputa_aberta')": [49126]}
CASO 51530/51531 [(51530, 'intervalo_saida', False, '', False, 'Pergunta #51530: batida vizinha #110812 '), (51531, 'intervalo_volta', False, '', False, 'Pergunta #51531: batida vizinha #110811 ')]
```

- Universo informado: 244 respostas, 82 colaboradores. [nao verificado]
- Carimbo nulo: 60; preenchido: 184 — `paridade_recusada`: 168, `chamado_encerrado`: 12, `declaracao`: 4. [nao verificado]
- Competências, no universo total: 07/2026: 4; 08/2026: 28; 09/2026: 85; 10/2026: 96; 11/2026: 31. [nao verificado]
- Distribuição por competência separada por carimbo/via: não apresentada. [nao verificado]
- 51530 (`intervalo_saida`) e 51531 (`intervalo_volta`): ambas no grupo `nulo`, `nao_validada`, `irma_sem_carimbo_resp`, `disputa_aberta`; batidas vizinhas 110812 e 110811, respectivamente. [nao verificado]

## 13/11 14:04 · sessao 3f6e6f46ae06 · DESLIGADO-NAO-TRAVA · verificador · origem=Code

**1. Achados (0) e (1): curados no código, com limites de apresentação e de prova.** (`app/folha/fora_da_folha.py:144`; `app/folha/retrato_folha.py:1193`)

- **(0), contador:** a fonte produz `desligados_retidos`; o censo agrega; o vigia publica no carimbo com esperado zero. O teste exige vazio normalmente e `[desligado.pk]` quando o export é simulado retendo desligado e ativo: detecta um contador artificialmente fixado em zero. (`app/folha/fora_da_folha.py:144`; `app/chamados/services/vigia_fio.py:589`; `app/chamados/management/commands/vigiar_fio_morto.py:101`; `app/chamados/management/commands/vigiar_fio_morto.py:246`; `app/folha/tests/test_lei_desligado_nao_trava.py:136`)
- **Limite do ESTADO:** o leitor genérico mostra a chave quando diverge do esperado. Com valor zero, não imprime nominalmente `desligados_retidos=0`; portanto, há transporte e exposição da violação, mas não exibição explícita do zero. (`bin/fio_estado.py:125`; `bin/fio_estado.py:191`; `bin/gerar_estado.py:260`)
- **(1), informativo:** `montar_linhas` obtém e grava `informativo`; o drawer lê a linha gravada e apresenta suas datas. O teste da lavra exige três datas para o desligado e vazio para ativo/afastado; o teste do template exige rótulo/datas e ausência do bloco com lista vazia ou chave ausente. (`app/folha/retrato_folha.py:1161`; `app/folha/retrato_folha.py:1193`; `app/colaboradores/views.py:984`; `app/templates/folha/_retrato_o_que_fazer.html:56`; `app/folha/tests/test_lei_desligado_nao_trava.py:117`; `app/folha/tests/test_lei_desligado_nao_trava.py:122`)

A cura do informativo alcança retratos novos ou relavrados: a leitura continua usando o JSON persistido. Relavra produtiva e execução desses testes nesta verificação: **[nao verificado]**. (`app/folha/retrato_folha.py:110`; `app/folha/management/commands/lavrar_retrato_folha.py:74`)

**2. Escritores, leitores e significado de `desligados_retidos`.**

| Etapa | Quem escreve/lê e significado |
|---|---|
| Fonte | Seleciona os colaboradores classificados por `furo_espelho`, deduplica pelo dict `_colabs` e produz **lista de ids únicos naquela empresa × competência**, perguntando a `esta_desligado`. (`app/folha/fora_da_folha.py:128`; `app/folha/fora_da_folha.py:131`; `app/folha/fora_da_folha.py:144`) |
| Agregador | Lê a lista e soma seu comprimento por empresa × competência. Produz **inteiro de ocorrências colaborador × empresa × competência**. (`app/chamados/services/vigia_fio.py:583`; `app/chamados/services/vigia_fio.py:589`) |
| Vigia | Lê o inteiro para stdout, copia para `censo` e publica também o bloco `alcance`; `_carimbar` persiste o JSON. (`app/chamados/management/commands/vigiar_fio_morto.py:120`; `app/chamados/management/commands/vigiar_fio_morto.py:212`; `app/chamados/management/commands/vigiar_fio_morto.py:246`; `app/chamados/management/commands/vigiar_fio_morto.py:248`) |
| ESTADO | Compara o escalar com o esperado do próprio carimbo e imprime divergências; `gerar_estado` delega a composição. (`bin/fio_estado.py:125`; `bin/fio_estado.py:191`; `bin/gerar_estado.py:260`) |

**O mesmo id em duas competências conta duas vezes.** Não há deduplicação global desse contador; a deduplicação por `(colaborador, data)` pertence à conta separada de dias. Não encontrei leitor convertendo dias em pessoas nessa cadeia. (`app/chamados/services/vigia_fio.py:589`; `app/chamados/services/vigia_fio.py:592`)

Assim, os consumidores são compatíveis com a agregação implementada, mas o resultado **não representa pessoas únicas da frota nem quantidade de dia-colab**. O comentário do agregador declara colaborador × competência; essa distinção deve acompanhar qualquer interpretação do número. (`app/chamados/services/vigia_fio.py:587`; `app/colaboradores/services/desligamento.py:18`)

**3. Informativo: autoridade correta; N+1 real.**

O helper não reconstrói dias ou regras: usa `janela_fechamento`, pergunta ao juiz `esta_desligado`, chama `folha.export.informativo_rescisao` e converte as datas para strings. A autoridade reutiliza o mesmo laço do retentor. (`app/folha/retrato_folha.py:1214`; `app/folha/retrato_folha.py:1217`; `app/folha/retrato_folha.py:1219`; `app/folha/export.py:1212`; `app/folha/export.py:1228`)

O custo incremental é **uma consulta de colaboradores + duas por desligado não isento cuja janela não colapse**: células e vínculos. Para uma folha de 750 pessoas, a fórmula é `1 + 2D`, sendo `D` os desligados elegíveis; se os 750 forem elegíveis, são **1.501 consultas adicionais**, além da fonte e das causas. Ativos/afastados pulam as consultas individuais. (`app/folha/retrato_folha.py:1216`; `app/folha/retrato_folha.py:1217`; `app/folha/export.py:1226`; `app/folha/export.py:1243`; `app/folha/export.py:1247`; `app/folha/export.py:1253`; `app/escala/alimentacao.py:186`; `app/escala/alimentacao.py:148`)

**O teste “UMA consulta” não comprova isso:** substitui o helper inteiro por mock e verifica uma chamada Python. Não mede SQL nem detecta seu N+1 interno. Latência real: **[nao verificado]**. (`app/folha/tests/test_retrato_folha_quadros.py:1345`; `app/folha/tests/test_retrato_folha_quadros.py:1349`; `app/folha/tests/test_retrato_folha_quadros.py:1350`)

O drawer mostra o bloco somente quando `informativo` contém dias. (`app/templates/folha/_retrato_o_que_fazer.html:56`; `app/templates/folha/_retrato_o_que_fazer.html:60`)

**4. Outros leitores: não encontrei quebra pela chave nova nos caminhos abaixo.**

| Leitor fora do delta | Comportamento |
|---|---|
| `fora_da_folha` | Extrai chaves nomeadas; não interpreta o dict inteiro como ids. (`app/folha/fora_da_folha.py:168`; `app/folha/fora_da_folha.py:170`) |
| `linhas_alcance` | Formata campos e subdicts específicos. (`app/chamados/services/vigia_fio.py:708`) |
| Backfill E4B | Consome `alcance_dias` e contadores específicos. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:239`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:412`) |
| `fio_estado` / `gerar_estado` | Iteram os esperados do carimbo e delegam apresentação; não somam todos os valores. (`bin/fio_estado.py:125`; `bin/gerar_estado.py:260`) |
| `pipeline_placar` | Lê métricas por nome; suas chamadas contemplam `fio_mudo_alvos` e `nunca_bateu_sem_adesao`, ignorando a nova. (`app/core/management/commands/pipeline_placar.py:92`; `app/core/management/commands/pipeline_placar.py:135`) |

As comparações integrais de `censo` e `esperado` identificadas no teste do vigia receberam a chave. Os testes de conservação de dias somam parcelas explícitas ou o subdict de rotas, sem incorporar o contador novo. (`app/chamados/tests/test_o261_vigia_fio.py:815`; `app/chamados/tests/test_o261_vigia_fio.py:828`; `app/chamados/tests/test_o261e_f5_alcance.py:104`; `app/chamados/tests/test_o261e_f5_alcance.py:392`)

**5. Classes de reprovação presentes no delta.**

- **Relógio fora da borda: presente no novo caminho.** O helper chama a autoridade sem `hoje`; o laço lê `localdate()` por desligado. O relógio já existia, mas o delta introduz esse novo consumidor sem transportar o instante capturado pelo lavrador. (`app/folha/retrato_folha.py:1219`; `app/folha/export.py:1238`; `app/folha/management/commands/lavrar_retrato_folha.py:48`; `app/folha/management/commands/lavrar_retrato_folha.py:65`)
- **Selo sem mordida para a propriedade anunciada: presente.** A asserção denominada “UMA consulta” testa apenas a chamada ao mock, deixando passar o N+1. Os testes funcionais de informativo e contador têm asserções próprias úteis. (`app/folha/tests/test_retrato_folha_quadros.py:1345`; `app/folha/tests/test_retrato_folha_quadros.py:1350`; `app/folha/tests/test_lei_desligado_nao_trava.py:117`; `app/folha/tests/test_lei_desligado_nao_trava.py:143`)
- **Segundo juiz, escritor fora da porta ou derivação paralela novos: não identificados nesses acréscimos.** Eles delegam aos juízes existentes, e a persistência permanece no lavrador. (`app/folha/fora_da_folha.py:143`; `app/folha/retrato_folha.py:1208`; `app/folha/retrato_folha.py:1219`; `app/folha/management/commands/lavrar_retrato_folha.py:74`)
- **Campo com dois significados:** há mudança explícita de lista de ids para contagem agregada entre funções, sem leitor incompatível encontrado; não deve ser apresentado como contagem de dias ou pessoas únicas. (`app/folha/fora_da_folha.py:146`; `app/chamados/services/vigia_fio.py:587`)

**6. Descartes da triagem.**

- **(3): descarte coletivo incorreto.** O Raio-X seleciona pessoas pelo juiz, mas recorta a demissão e reconstrói seus dias com `furos_por_dia`. Isso contraria a exigência de nenhum leitor de retido derivar por conta própria. O preview também usa quantidade de **motivos distintos** no lugar de dias. Esses problemas sobrevivem ao delta. (`app/folha/views.py:591`; `app/folha/views.py:612`; `app/folha/views.py:616`; `app/folha/views.py:398`; `app/folha/export.py:1256`; `app/folha/export.py:1298`)
- **`fechamento_lista`, separadamente:** lê a classificação e traduz o rótulo, preservando `motivo_juiz`; não constitui outra decisão de retenção. Seu fallback ambíguo para `furo_espelho` continua preexistente. (`app/ponto/services/fechamento_lista.py:34`; `app/ponto/services/fechamento_lista.py:36`)
- **(2): distinção técnica sustentada, cumprimento universal não demonstrado.** `turno_aberto` ainda impede aprovação/carimbo, mas a geração permite que a linha siga no TXT. Isso não prova retenção do TXT; também não elimina a ressalva sobre “nunca como trava”. Revogação de todo crivo de aprovação pelo aval: **[nao verificado]**. (`app/folha/export.py:833`; `app/ponto/views.py:1509`; `app/folha/views.py:216`; `app/folha/views.py:244`)
- **(3d): rótulo amplo confirmado.** A API chama todo `fora` de `retidos`, inclusive rescisão; lê o juiz e não cria retenção adicional. (`app/api/views_mensageria.py:283`; `app/api/views_mensageria.py:301`; `app/folha/export.py:866`)
- **(4): risco preexistente persiste.** O POST busca por pk e grava `afastado` pela porta, fazendo o juiz deixar de reconhecer desligamento. “Proibido tocar afastado” não demonstra que preservar um desligado nessa entrada seja proibido. Entretanto, após a troca, a pessoa sai da unidade literal por situação atual; não uso isso isoladamente como nova violação introduzida pelo delta. Ocorrência produtiva: **[nao verificado]**. (`app/ponto/views.py:2406`; `app/ponto/views.py:2435`; `app/colaboradores/services/desligamento.py:18`; `app/colaboradores/services/desligamento.py:24`)

**Veredito: REPROVA.** Bloqueantes: novo caminho com relógio interno; N+1 acompanhado de teste que anuncia uma garantia de consultas que não verifica; e descarte do censo apesar da derivação paralela sobrevivente no Raio-X. (`app/folha/retrato_folha.py:1219`; `app/folha/export.py:1238`; `app/folha/tests/test_retrato_folha_quadros.py:1350`; `app/folha/views.py:616`)

Suíte integral, opus, pouso e medição produtiva de competência 11 com contador zero: **[nao verificado]**. [nao verificado]

## 13/11 14:03 · sessao 3167efda7c55 · - · - · origem=Ronald (tmux)

**2. Escritores, leitores e unidade**

| Etapa | Referência | Significado |
|---|---|---|
| Seleção dos retidos por furo | `app/folha/fora_da_folha.py:119-131` | Usa resultado de `classificar_export`; carrega colaboradores por ids de `furo_espelho`. |
| Produção de `desligados_retidos` | `app/folha/fora_da_folha.py:143-146` | **Lista de ids únicos por empresa × competência**, filtrados pelo juiz `esta_desligado`. O dict `_colabs` elimina repetição do mesmo id nessa chamada. |
| Agregação | `app/chamados/services/vigia_fio.py:583-589` | **Inteiro: soma das quantidades por empresa × competência**. Mesmo id em duas competências soma duas vezes; não há deduplicação global. |
| Dias, separadamente | `app/chamados/services/vigia_fio.py:590-593` | Deduplicados por `(colab_id, data)`; não alimentam o contador novo. |
| Impressão do vigia | `app/chamados/management/commands/vigiar_fio_morto.py:212-213` | Imprime inteiro e esperado 0. |
| Publicação | `app/chamados/management/commands/vigiar_fio_morto.py:237-248` | Grava escalar no `censo`, esperado 0 e bloco `alcance`. |
| Escrita física do carimbo | `app/chamados/management/commands/vigiar_fio_morto.py:120-139` | `_carimbar` escreve JSON e substitui arquivo. |
| Leitura pelo ESTADO | `bin/fio_estado.py:122-126`; `bin/fio_estado.py:191-194`; `bin/gerar_estado.py:260-261` | Compara campos declarados em `esperado` e apresenta divergências. |

**Não há leitor encontrado que conte dias como pessoas. Há repetição intencional do id entre competências**, pois a soma representa ocorrências de colaborador por empresa × competência, não pessoas únicas na frota. (`app/chamados/services/vigia_fio.py:583-593`)

**Ressalva importante do achado (0): zero não aparece nominalmente na linha publicada do ESTADO.** Apenas valores divergentes entram em `fora_do_esperado` e são exibidos; `desligados_retidos=0` fica no carimbo, sem exposição explícita nessa linha. (`bin/fio_estado.py:125-126`; `bin/fio_estado.py:191-194`)

Conferido executando somente em memória as funções reais de `bin/fio_estado.py`: carimbo sintético com 0 produz `fora={}` e não mostra o nome; com 1 mostra `desligados_retidos` como divergência. É confirmação do comportamento do leitor, não medição produtiva. (`bin/fio_estado.py:121`; `bin/fio_estado.py:165`; produção: `[nao verificado]`)

**4. Leitores fora do diff**

Não encontrei quebra por chave nova nestes leitores: cada um acessa subcampos nomeados, sem interpretar todas as chaves como ids ou somar todos os valores. (`app/folha/fora_da_folha.py:168-171`; `app/chamados/services/vigia_fio.py:708-728`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78-79`; `bin/fio_estado.py:122-126`)

| Leitor | Comportamento relevante |
|---|---|
| `fora_da_folha` | Extrai exclusivamente as chaves antigas; não repassa a chave nova. (`app/folha/fora_da_folha.py:168-171`; `app/folha/fora_da_folha.py:201-207`) |
| `linhas_alcance` | Formata campos explícitos; itera somente subdicts de detalhe/papel. (`app/chamados/services/vigia_fio.py:708-728`) |
| Backfill | Filtra `alcance_dias`; consulta universo e fio morto explicitamente. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:78-79`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:239`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:412`) |
| `fio_estado` | Valida esquema por campos; itera `esperado.items()`, aceitando a chave nova. (`bin/fio_estado.py:79-94`; `bin/fio_estado.py:122-126`) |
| `gerar_estado` | Delega leitura e composição. (`bin/gerar_estado.py:260-261`) |
| **`pipeline_placar`**, leitor adicional do mesmo carimbo | Lê `registro['censo'][nome]`/`esperado[nome]`; os chamados ativos são somente `fio_mudo_alvos` e `nunca_bateu_sem_adesao`. Portanto ignora o contador novo e não quebra. (`app/core/management/commands/pipeline_placar.py:92-106`; `app/core/management/commands/pipeline_placar.py:135-136`) |

Nos testes, os únicos comparadores de dict inteiro identificados para `reg['censo']` e `reg['esperado']` já ganharam a chave no delta. (`app/chamados/tests/test_o261_vigia_fio.py:815-832`)

Os testes de soma usam parcelas explícitas ou subdicts, mantendo o contador novo fora das contas de dias. (`app/chamados/tests/test_o261e_f5_alcance.py:104-111`; `app/chamados/tests/test_o261e_f5_alcance.py:392`; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:194-197`)

Os demais consumidores diretos de testes acessam campos específicos. (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:77-79`; `app/chamados/tests/test_n1_alimentacao_unica.py:99-119`; `app/chamados/tests/test_e4_passivo.py:110-119`; `app/chamados/tests/test_e4_backfill.py:162`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:95-98`; `app/chamados/tests/test_o261_resposta_vazia.py:168-170`)

A fixture antiga do selo shell não inclui `desligados_retidos`; isso não a quebra, pois o leitor compara apenas os esperados existentes na própria fixture. Também não comprova cobertura específica do contador novo no ESTADO. (`bin/tests/test_fio_estado.sh:48-60`; `bin/tests/test_fio_estado.sh:74`; `bin/fio_estado.py:125-126`)

**Teste do contador que morde:** exige vazio no caminho normal e `[self.d.pk]` quando o export é mutado para reter desligado e ativo por furo; portanto não é um contador literal 0. (`app/folha/tests/test_lei_desligado_nao_trava.py:136-143`)

**Limite da cobertura:** teste do carimbo usa o valor normalmente devolvido pelo censo e compara esperado 0; não há, nesse trecho, cenário de `desligados_retidos > 0` atravessando censo → carimbo → ESTADO. (`app/chamados/tests/test_o261_vigia_fio.py:815-832`; execução integral: `[nao verificado]`)

## 13/11 14:02 · sessao c54425f805df · - · - · origem=Ronald (tmux)

**(3) O descarte coletivo contradiz a cláusula de fonte única do aval.** O Raio-X lê do juiz quem está em `furo_espelho`, mas deriva suas próprias datas: recorta por demissão e chama `furos_por_dia`, que retorna marcos apagados da ata, não dias do retentor. (`app/folha/views.py:591`; `app/folha/views.py:612`; `app/folha/views.py:616`; `app/relatorios/furos.py:186`; `app/relatorios/furos.py:202`)

O preview também não apresenta uma contagem de dias da autoridade: chama `len(motivos_espelho)` de `_dias` e imprime esse número; a autoridade acumula motivos em `set` e devolve motivos distintos. (`app/folha/views.py:398`; `app/folha/views.py:402`; `app/folha/export.py:1256`; `app/folha/export.py:1298`)

`fechamento_lista` é diferente: lê o resultado e traduz o rótulo, preservando `motivo_juiz`; não decide novamente quem retém. O fallback chama outros motivos de `furo_espelho`, o que é ambiguidade de apresentação preexistente. (`app/ponto/services/fechamento_lista.py:25`; `app/ponto/services/fechamento_lista.py:34`; `app/ponto/services/fechamento_lista.py:36`)

**(2) O descarte não demonstra cumprimento da frase “nunca como trava”.** `turno_aberto` continua sendo emitido para desligados e recusa aprovação individual, lote e carimbo automático. (`app/folha/export.py:833`; `app/folha/export.py:848`; `app/ponto/views.py:1509`; `app/ponto/views.py:1308`; `app/folha/views.py:244`)

Entretanto, isso não prova bloqueio de entrada no TXT: a geração separa `status='entra'` do carimbo e documenta que quem tem trava segue no TXT sem aprovação automática. É caminho preexistente fora do delta; interpretar o aval como revogação de todo crivo de aprovação exige assumir alcance adicional. (`app/folha/views.py:215`; `app/folha/views.py:216`; alcance adicional: `[nao verificado]`)

**(3d) O problema de rótulo existe, mas não é nova retenção pelo delta.** A API conta todo `status='fora'` como `retidos`, incluindo saída por rescisão; ela lê o juiz e não escreve ou recalcula o retentor. (`app/api/views_mensageria.py:283`; `app/api/views_mensageria.py:301`; `app/folha/export.py:866`)

**(4) O contraexemplo estático persiste; “PROIBIDO tocar afastado” não prova que preservar desligado seja proibido.** A lista da tela só oferece ativos/afastados, mas o POST busca qualquer `pk` e, para INSS, grava `afastado` pela porta legítima. (`app/ponto/views.py:2372`; `app/ponto/views.py:2406`; `app/ponto/views.py:2433`; `app/colaboradores/services/colaborador.py:55`)

Depois dessa escrita, `esta_desligado` devolve falso e o retentor deixa de aplicar a saída antecipada. Contudo, a unidade literal é a situação atual: após a troca já não é `situacao='desligado'`. Portanto, há risco preexistente e descarte insuficientemente fundamentado, mas não prova autônoma de violação da lei para alguém que ainda esteja na unidade definida. (`app/colaboradores/services/desligamento.py:18`; `app/colaboradores/services/desligamento.py:24`; `app/folha/export.py:1207`; execução produtiva: `[nao verificado]`)

**Para consolidar:** o bloqueante mais objetivo nos descartes é declarar limpo o censo apesar da derivação paralela do Raio-X; preview confunde motivos com dias. Esses caminhos não foram curados pelo delta fornecido. (`app/folha/views.py:616`; `app/folha/views.py:398`; presença da cura no delta: `[nao verificado]`)

## 13/11 14:02 · sessao dbda1d79cea4 · BUG-PAR · verificador · origem=Code

**BLOQUEANTE: o passivo já absolvido continua fora da recuperação, e a tela pode prometer um par que o clique recusa.** Fontes: `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:192`, `app/chamados/services/materializacao.py:566`, `app/chamados/services/validacao.py:391`.

1. **BLOQUEANTE — pergunta absolvida por paridade permanece invisível em quatro entradas.**

   Contraexemplo: saída S01:00 respondida, com `validada_em` e `materializada_em` preenchidos por `VIA_PARIDADE_RECUSADA`, sem batida criada; depois chega a resposta da volta E02:00. Essa absolvição sem batida existe expressamente em `app/chamados/services/materializacao.py:1010` e `app/chamados/services/materializacao.py:1050`.

   As quatro entradas que não a recuperam são:

   | Entrada | Exclusão |
   |---|---|
   | Contador | Exige `materializada_em IS NULL`: `app/chamados/services/materializacao.py:191`. |
   | Clique na própria saída | Retorna `ja_validada`: `app/chamados/services/validacao.py:433`. Se a validação estiver nula, ainda pula a materialização pelo carimbo preenchido: `app/chamados/services/validacao.py:505`. |
   | Clique na volta | A seleção da irmã exclui a saída carimbada: `app/chamados/services/materializacao.py:136`; `_validar_o_par` depende dessa seleção: `app/chamados/services/validacao.py:390`. |
   | Laço de materialização da disputa | Seleciona somente perguntas sem carimbo: `app/chamados/services/materializacao.py:1531`. |

   São **quatro entradas, não quatro causas independentes**. `_gravar_irma_do_par` compartilha a seleção da irmã; a devolução também exige carimbo nulo, portanto não constituem quinta e sexta entradas: `app/chamados/services/materializacao.py:1250`, `app/chamados/services/validacao.py:192`. Quantas das 60 respostas pertencem a essa classe: **[nao verificado]**.

2. **BLOQUEANTE — tela promete aplicação com irmã implausível; clique recusa.**

   Contraexemplo: saída respondida 01:00; volta respondida **14:00**, com marco fixo **02:00**, ainda sem validação. A projeção usa 02:00, porque o plantio de intervalo substitui a hora respondida pela hora do marco; a tela encontra o par válido e retorna `aplicavel`: `app/chamados/services/materializacao.py:511`, `app/chamados/services/materializacao.py:563`.

   O clique consulta a implausibilidade da resposta **14:00 contra 02:00**, abandona o par e tenta a saída isoladamente: `app/chamados/services/validacao.py:390`, `app/chamados/services/validacao.py:508`. O juiz detecta a diferença circular de 720 minutos: `app/chamados/juizes.py:1799`, `app/chamados/juizes.py:2016`. A testemunha, portanto, não representa todos os impedimentos da porta: `app/chamados/services/materializacao.py:143`, `app/chamados/services/validacao.py:391`.

3. **BLOQUEANTE — igualdade de `data_evento` não garante o mesmo dia do chamado.**

   Contraexemplo: as duas perguntas têm `data_evento=25/09`, mas o chamado que lista a volta aponta 26/09, sem `data_conferida_em`. A seleção aceita a irmã pela igualdade entre os campos das perguntas; a projeção não consulta a divergência com o chamado, e a tela promete o par: `app/chamados/services/materializacao.py:136`, `app/chamados/services/materializacao.py:143`, `app/chamados/services/materializacao.py:566`.

   Na escrita da irmã, `_materializar_uma` consulta `data_do_fato_de`, encontra a divergência e retorna sem materializar, com `DATA_EM_DISPUTA`: `app/chamados/services/materializacao.py:1353`. A comparação entre dia da pergunta e dia do chamado está em `app/chamados/services/fio.py:85`.

   **NAO-BLOQ para os outros atributos isoladamente:** irmã com outro `data_evento`, já materializada ou retratada fica excluída da projeção. Os filtros estão em `app/chamados/services/materializacao.py:137` e `app/chamados/services/materializacao.py:139`; `fora_do_fato` reconhece a via terminal em `app/chamados/models.py:3227`. Não há contraexemplo demonstrado desses três atributos, por si sós, produzindo promessa indevida de par: **[nao verificado]**.

4. **BLOQUEANTE — o aviso “colada” continua descrevendo distância de horas.**

   Contraexemplo já presente no teste: S01:00 isolada recusa pela S06:30, diferença de **5h30**; o teste exige recusa, mas não exige “falta a outra metade”: `app/chamados/tests/test_bug_par_projetado.py:151`.

   Toda recusa classificada como PARIDADE continua recebendo “marcação […] colada nesse horário”, embora a guarda procure vizinhas até 12 horas: `app/chamados/catalogo/recusa.py:26`, `app/chamados/services/materializacao.py:100`. O requisito textual do aval de dois atos não está realizado nesse caminho: `app/chamados/services/acoes_disputa.py:33`.

5. **BLOQUEANTE — o lote pode processar novamente a irmã que o par acabou de gravar.**

   Contraexemplo: o grupo do lote contém saída e volta já validadas. Ao processar a saída, `_gravar_irma_do_par` materializa primeiro a volta; depois o lote alcança o objeto antigo da volta e chama `_materializar_uma` novamente: `app/chamados/services/materializacao.py:1258`, `app/chamados/services/passivo_validado.py:520`.

   A segunda passagem pode reutilizar a batida, registrar outra auditoria e renovar o carimbo: `app/chamados/services/materializacao.py:980`, `app/chamados/services/materializacao.py:1077`, `app/chamados/services/materializacao.py:1439`. `carimbar` escolhe novo instante e salva novamente: `app/chamados/models.py:3150`, `app/chamados/models.py:3178`.

   O diff protegeu o laço da disputa contra exatamente essa repetição, mas essa proteção não está no laço do passivo: `app/chamados/services/materializacao.py:1538`, `app/chamados/services/passivo_validado.py:522`. Reprodução executada e quantidade de eventos adicionais: **[nao verificado]**.

6. **BLOQUEANTE — a ordem de trava não é uniforme com o lote; há interleaving de deadlock.**

   O admin toma Colaborador antes de validar e materializar; os chamados da irmã entram no conjunto ordenado de travas: `app/chamados/services/validacao.py:465`, `app/chamados/services/validacao.py:474`. A autoridade ordena Colaborador → Disputa → Chamado → Pergunta, com pks crescentes: `app/chamados/models.py:2650`, `app/chamados/models.py:2687`, `app/chamados/models.py:2709`.

   O lote entra diretamente na materialização sem essa trava inicial: `app/chamados/services/passivo_validado.py:374`, `app/chamados/services/passivo_validado.py:522`. A criação da batida dispara o núcleo, que apaga/recria `TurnoMaterializado`; depois o fechamento do chamado pode tomar Colaborador: `app/ponto/registro_batida.py:157`, `app/ponto/signals.py:34`, `app/ponto/nucleo.py:57`, `app/chamados/models.py:712`.

   Contraexemplo de concorrência: lote segura uma linha de `TurnoMaterializado`; admin segura Colaborador e espera aquela linha; lote tenta tomar Colaborador no fechamento. A inversão é estruturalmente possível nesses caminhos; ocorrência real e introdução histórica: **[nao verificado]**.

7. **NAO-BLOQ para rollback de banco demonstrável; notificação externa permanece uma lacuna.**

   No caso em que a saída é validada primeiro, a volta E02:00 grava antes da saída S01:00; existe, portanto, o estado intermediário E18:30–E02:00–S06:30. Núcleo, cartório e reconciliador executam entre as duas gravações: `app/chamados/services/materializacao.py:996`, `app/chamados/services/materializacao.py:1258`, `app/ponto/signals.py:34`, `app/ponto/signals.py:131`, `app/ponto/signals.py:206`.

   As escritas de banco estão dentro dos atomics; o outbox da pergunta é síncrono, os becos usam `on_commit`, e o realtime usa `pg_notify` na mesma conexão. Não encontrei nesses caminhos prova de rastro de banco sobrevivendo ao rollback do par: `app/chamados/services/validacao.py:395`, `app/chamados/services/materializacao.py:1255`, `app/chamados/signals.py:240`, `app/chamados/signals.py:259`, `app/ponto/realtime.py:40`.

   Existe, porém, caminho cartório → emissão de cobrança → push imediato → `webpush`, sem adiamento até commit nesses pontos: `app/ponto/services/cartorio.py:1829`, `app/ponto/management/commands/detectar_ausencias.py:777`, `app/chamados/push_utils.py:40`. Que o estado intermediário do caso fornecido efetivamente alcance essa emissão: **[nao verificado]**. O teste novo confere contagem de chamados/perguntas depois do sucesso; não prova ausência de push nem rollback após a primeira batida: `app/chamados/tests/test_bug_par_projetado.py:113`.

8. **BLOQUEANTE — duas entradas novas da allowlist ocultam o defeito de universo.**

   As exceções de `_gravar_irma_do_par` e `_validar_o_par` correspondem a guardas de escrita admitidas pelo próprio contrato: `app/chamados/tests/test_contract_leitores_vitalidade.py:8`, `app/chamados/tests/test_contract_leitores_vitalidade.py:108`, `app/chamados/tests/test_contract_leitores_vitalidade.py:113`.

   Já `metade_irma_do_intervalo` e o contador eliminam o passivo carimbado **antes** de consultar a autoridade. As duas exceções autorizam esses leitores, e o selo apenas remove endereços allowlisted da lista de ofensores; ele não verifica o significado do filtro: `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:192`, `app/chamados/tests/test_contract_leitores_vitalidade.py:109`, `app/chamados/tests/test_contract_leitores_vitalidade.py:331`.

9. **BLOQUEANTE de certificação — os dois ramos defensivos pedidos não estão exercitados pelos cenários novos.**

   Faltam cenários que forcem a rejeição por `not t_s < t_e` e a rechecagem ofensora depois de gravar a irmã: `app/chamados/services/materializacao.py:154`, `app/chamados/services/materializacao.py:997`. A fixture fixa os marcos em 01:00/02:00; os adversariais acrescentam vizinha ofensora ou retiram validação: `app/chamados/tests/test_bug_par_projetado.py:61`, `app/chamados/tests/test_bug_par_projetado.py:162`, `app/chamados/tests/test_bug_par_projetado.py:214`.

   Contraexemplos de certificação necessários: ordem projetada inválida deve impedir escrita; reprovação depois da primeira batida deve desfazer ambas, carimbos e efeitos transacionais. Execução dos mutantes, RED na base, GREEN e suite: **[nao verificado]**. [nao verificado]

O mapa de escrita/leitura dos campos centrais é este:

| Campo/valor | Quem escreve | Quem lê no percurso examinado |
|---|---|---|
| `validada_em` | `_carimbar_validacao`; devolução limpa; resposta nova limpa; carimbo simétrico também preenche. `app/chamados/services/validacao.py:136`, `app/chamados/services/validacao.py:201`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/models.py:3160`. | Guarda inicial, validação da irmã, materialização da irmã e laço. `app/chamados/services/validacao.py:433`, `app/chamados/services/validacao.py:391`, `app/chamados/services/materializacao.py:1251`, `app/chamados/services/materializacao.py:1533`. |
| `materializada_em`, autor e via | Atribuição canônica em `_escrever_carimbo`; persistência pelas portas `carimbar`/`carimbar_identidade`. `app/chamados/models.py:3123`, `app/chamados/models.py:3178`, `app/chamados/models.py:3216`. | Seleção da irmã, contador, tela, prova do par e laço. `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:192`, `app/chamados/services/materializacao.py:542`, `app/chamados/services/validacao.py:399`, `app/chamados/services/materializacao.py:1534`. |
| `VIA_PARIDADE_RECUSADA` | Ramo de recusa chama `carimbar_identidade`. `app/chamados/services/materializacao.py:1050`. | Identidade/vitalidade e testemunha da aplicação. `app/chamados/models.py:2531`, `app/chamados/models.py:3118`, `app/chamados/services/materializacao.py:546`. |
| Resposta e `respondida_em` | Escritor da resposta do colaborador. `app/chamados/services/disputa_emissao.py:1467`. | Seleção da irmã, contador e derivação do plantio. `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:199`, `app/chamados/services/materializacao.py:461`. |
| Motivo, tipo e dia da pergunta | Emissão; conferência pode alterar o dia. `app/chamados/services/disputa_emissao.py:834`, `app/chamados/services/validacao.py:664`. | Identificação da irmã e derivação compartilhada. `app/chamados/services/materializacao.py:133`, `app/chamados/services/materializacao.py:435`. |
| `recusa_motivo` / `pendencia_admin` | Porta da recusa e ramo de pendência. `app/chamados/services/materializacao.py:1209`, `app/chamados/services/materializacao.py:965`. | Contador e elegibilidade do par. `app/chamados/services/materializacao.py:193`, `app/chamados/services/materializacao.py:148`. |
| `estado_aplicacao_pergunta` | Retorno derivado, sem campo persistido. `app/chamados/services/materializacao.py:533`. | Modal/fio, contador e explicador. `app/chamados/services/fio.py:373`, `app/chamados/services/materializacao.py:201`, `app/chamados/services/explicador.py:100`. |
| `gravou` | Explicador acrescenta somente `False` para a recusa reconhecida. `app/chamados/services/explicador.py:101`. | Desenlace conta falsos; ausência deles mantém “Tudo validado e gravado”. `app/chamados/services/desenlace.py:65`. |
| `msg` | Service de validação. `app/chamados/services/validacao.py:563`. | Casca repassa, acrescentando avisos de irmãs. `app/chamados/services/acoes_disputa.py:62`. |
| Projeção `(ts, tipo)` | Contexto temporário definido/resetado ao materializar a irmã. `app/chamados/services/materializacao.py:1256`. | Guarda de paridade. `app/chamados/services/materializacao.py:990`. |

**O campo com dois significados continua sendo `materializada_em`:** sua descrição diz “batida criada + chamado fechado”, mas a recusa o preenche sem criar batida. O diff trata essa diferença na apresentação, porém continua usando o carimbo como exclusão do universo e como prova de conclusão: `app/chamados/models.py:3445`, `app/chamados/services/materializacao.py:1050`, `app/chamados/services/materializacao.py:192`, `app/chamados/services/validacao.py:399`.

Quanto aos avais e proibições:

| Item | Resultado da pré-verificação |
|---|---|
| Par projetado pelo escritor existente | Implementado no percurso novo, com as falhas acima. `app/chamados/services/materializacao.py:159`, `app/chamados/services/materializacao.py:1258`. |
| Paridade preservada; isolada continua recusada | Preservada no código e exigida pelo teste. `app/chamados/services/materializacao.py:1003`, `app/chamados/tests/test_bug_par_projetado.py:151`. |
| Não aceitar furo da véspera | A consulta continua restrita ao dia local do plantio. `app/chamados/services/materializacao.py:273`. |
| Central/modal não mentir | Correção parcial: explicador/desenlace reconhecem a absolvição sem batida; previsão de aplicação ainda diverge do clique. `app/chamados/services/explicador.py:101`, `app/chamados/services/desenlace.py:65`, `app/chamados/services/validacao.py:391`. |
| As 60 voltarem ao admin sem nova resposta | Não demonstrado; há exclusões estruturais do passivo. `app/chamados/services/materializacao.py:138`, `app/chamados/services/materializacao.py:192`. Resultado sobre as 60: **[nao verificado]**. |
| Contador 60 → 0, restantes legítimas listadas | O teste cobre recusa sem carimbo, não o passivo já absolvido. `app/chamados/tests/test_bug_par_projetado.py:181`. Medição da frota: **[nao verificado]**. |
| RED do caso e contraexemplo isolado | Cenários sintéticos presentes. `app/chamados/tests/test_bug_par_projetado.py:113`, `app/chamados/tests/test_bug_par_projetado.py:151`. Execução: **[nao verificado]**. |
| História da recusa, censo por competência/VALIDAR/DECIDIR, DRY integral, matriz e reaproveitamento dos mutantes | **[nao verificado]**. |
| Cópia nova sobre E3, ancestralidade, suite após E3, opus e pouso | **[nao verificado]**. | [nao verificado]
| Teto de produção | O diff fornecido toca cinco arquivos de produção: `app/chamados/services/acoes_disputa.py:62`, `app/chamados/services/desenlace.py:65`, `app/chamados/services/explicador.py:100`, `app/chamados/services/materializacao.py:86`, `app/chamados/services/validacao.py:379`. |
| Segundo motor ou alteração da resposta do colaborador | Não aparecem no percurso acrescentado; ele carimba validação e delega ao materializador. `app/chamados/services/validacao.py:397`, `app/chamados/services/materializacao.py:1258`. |

**NAO-BLOQ quanto a segundo escritor ou juiz duplicado demonstrado.** As duas funções novas delegam a `_materializar_uma`; o plantio efetivo continua em `registrar_batida`, e a vinculação passa por `anotar`: `app/chamados/services/validacao.py:398`, `app/chamados/services/materializacao.py:1258`, `app/chamados/services/materializacao.py:1052`, `app/chamados/services/materializacao.py:1073`. A guarda retroativa de vizinhança não é, pelo contrato examinado, a mesma pergunta do tipo sugerido por `proximo_tipo_de`: `app/chamados/services/materializacao.py:86`, `app/core/juizes.py:213`.

**O padrão Single Writer Principle é realizado na escrita da batida, mas não certifica a obra inteira:** o glossário o associa ao chokepoint existente; reutilizá-lo não elimina seleção incompleta, divergência tela/clique ou repetição do lote. Fontes: `app/docs/GLOSSARIO.md:12`, `app/chamados/services/materializacao.py:1052`, `app/chamados/services/materializacao.py:138`, `app/chamados/services/validacao.py:391`, `app/chamados/services/passivo_validado.py:522`. Relógio novo fora da borda ou nova derivação paralela não foram demonstrados nas funções acrescentadas, que reutilizam `_ts_tipo_do_plantio`: `app/chamados/services/materializacao.py:150`, `app/chamados/services/materializacao.py:172`.

## 13/11 14:02 · sessao 8555b616ccb4 · - · - · origem=Ronald (tmux)

- **Achado (1) curado no código para retratos novos/relavrados:** `montar_linhas` obtém o informativo e grava chave própria; a porta de gravação segue o lavrador existente. (`app/folha/retrato_folha.py:1161`; `app/folha/retrato_folha.py:1193`; `app/folha/management/commands/lavrar_retrato_folha.py:65`; `app/folha/management/commands/lavrar_retrato_folha.py:74`)
- **Testes que mordem:** o teste da lavra exige três datas no desligado e vazio no ativo/afastado; o teste do drawer exige rótulo e datas, e ausência do rótulo com lista vazia/chave ausente. (`app/folha/tests/test_lei_desligado_nao_trava.py:106`; `app/folha/tests/test_lei_desligado_nao_trava.py:117`; `app/folha/tests/test_lei_desligado_nao_trava.py:122`; `app/folha/tests/test_lei_desligado_nao_trava.py:131`)
- **Leitura da autoridade, sem derivação paralela:** o helper pergunta a `esta_desligado`, chama `folha.export.informativo_rescisao` e transforma as datas em strings; janela vem de `janela_fechamento`. A autoridade chama o mesmo `_dias_sem_decisao` do retentor. (`app/folha/retrato_folha.py:1208`; `app/folha/retrato_folha.py:1214`; `app/folha/retrato_folha.py:1217`; `app/folha/retrato_folha.py:1219`; `app/folha/export.py:1212`; `app/folha/export.py:1228`)
- **N+1 real:** custo incremental do helper = uma consulta de colaboradores mais duas por desligado não isento cuja janela não colapse: células + vínculos. Para 750 desses desligados, 1.501 consultas, além do custo anterior de montar a fonte e causas. Ativos/afastados não fazem as duas consultas individuais. (`app/folha/retrato_folha.py:1216`; `app/folha/retrato_folha.py:1217`; `app/folha/export.py:1226`; `app/folha/export.py:1243`; `app/folha/export.py:1247`; `app/folha/export.py:1253`; `app/escala/alimentacao.py:186`; `app/escala/alimentacao.py:148`)
- **Selo de custo sem mordida:** `test_retrato_folha_quadros` substitui o helper inteiro por mock e conta uma chamada Python, embora o comentário/asserção a chamem de “UMA consulta”. Não mede SQL nem impede o N+1 interno. (`app/folha/tests/test_retrato_folha_quadros.py:1345`; `app/folha/tests/test_retrato_folha_quadros.py:1349`; `app/folha/tests/test_retrato_folha_quadros.py:1350`)
- **Drawer só mostra com dia:** a view entrega a linha gravada, o painel inclui o partial e o bloco tem `if retrato_linha.informativo`; sem competência, permissão `ver_folha`, retrato ou linha, não há esse caminho de apresentação. (`app/colaboradores/views.py:977`; `app/colaboradores/views.py:984`; `app/colaboradores/views.py:995`; `app/templates/colaboradores/partials/painel_colaborador.html:26`; `app/templates/folha/_retrato_o_que_fazer.html:56`; `app/templates/folha/_retrato_o_que_fazer.html:60`)
- **Classe relógio:** o delta abre novo caminho para relógio interno: helper chama a autoridade sem `hoje`; esta passa `None` ao laço, que lê `localdate()`. O relógio já existia; a chamada pelo novo informativo do retrato é nova. (`app/folha/retrato_folha.py:1219`; `app/folha/export.py:1228`; `app/folha/export.py:1238`)
- **Limite de implantação:** leitura continua usando o JSON gravado; retratos antigos sem a chave não ganham o informativo até relavra. Relavra produtiva e execução dos testes: `[nao verificado]`. (`app/folha/retrato_folha.py:110`; `app/folha/retrato_folha.py:1190`)

## 13/11 14:00 · sessao ceeeba5ede72 · HJ-090 · verificador · origem=Code

**Pré-verificação: REPROVADA.** Permanecem corridas de saldo, escritores fora das travas, reversão insegura e contador ausente do ESTADO (`app/ferias/services.py:233`, `app/ferias/services.py:321`, `app/ferias/management/commands/realinhar_concedidos_ferias.py:72`, `bin/gerar_estado.py:178`).

As conclusões abaixo são estáticas. Execução dos contraexemplos, RED/GREEN, aplicação em produção e ancestralidade do hash: **[não verificado]**. Os totais raspados não demonstram quais registros cada implementação efetivamente alcança **[não verificado]**.

1. **BLOQUEANTE — a trava evita perda da soma, mas não impede consumir saldo já gasto.** A tela valida saldo antes da porta; `executar_agendamento` relê o período travado, porém só revalida duplicata, sem repetir saldo/fracionamento (`app/ferias/views.py:297`, `app/ferias/services.py:434`, `app/ferias/services.py:435`). O caminho com abono também valida antes do `atomic`; a venda isolada apenas incrementa sob trava (`app/ferias/services.py:233`, `app/ferias/services.py:237`, `app/ferias/services.py:720`).

   **Contraexemplos:** com direito/saldo 30, agendamento de 30 e venda de 5 passam nas respectivas fotos; as escritas serializadas terminam com concedidos 30, vendidos 5 e saldo −5. Duas vendas de 6 podem passar pelo teto inicial 10 e terminar em vendidos 12. Os juízes existem, mas são consultados antes da trava (`app/ferias/views.py:241`, `app/ferias/views.py:962`, `app/ferias/views.py:964`, `app/ferias/models.py:217`, `app/ferias/models.py:235`).

2. **BLOQUEANTE — ainda há escritores de status do período sem releitura travada.** `sincronizar_status_periodo` calcula e salva usando o objeto recebido; `garantir_periodo_aquisitivo` também lê uma foto e depois faz `update` (`app/ferias/services.py:307`, `app/ferias/services.py:315`, `app/ferias/services.py:321`, `app/ferias/services.py:344`, `app/ferias/services.py:348`).

   **Contraexemplo:** alerta carrega período adquirido, saldo 30, limite vencido; outro pedido agenda 30 e grava `concedido`; o alerta grava `vencido`, calculado pela foto anterior. Esse escritor é alcançado pelo cron e pelas consultas de saldo/seção (`app/ferias/management/commands/alertar_ferias_vencendo.py:32`, `app/ferias/views.py:116`, `app/ferias/views.py:781`).

3. **BLOQUEANTE — Ausencia de férias tem edição e rejeição independentes do agendamento.** As telas genéricas carregam qualquer Ausencia por PK e podem rejeitá-la ou alterar tipo/datas, sem passar pelas portas travadas de férias (`app/ponto/views.py:2728`, `app/ponto/views.py:2734`, `app/ponto/views.py:2761`, `app/ponto/views.py:2794`, `app/ponto/services/ausencia.py:498`, `app/ponto/services/ausencia.py:836`).

   **Contraexemplo:** agendamento vivo de 30 dias, concedidos 30; rejeitar uma Ausencia deixa agendamento e espelho discordantes. Outra sincronização não repara essa rejeição: `espelhar_ferias` inclui qualquer status em `existentes` e pula datas existentes (`app/ponto/services/ausencia.py:1182`, `app/ponto/services/ausencia.py:1200`). Mover a Ausencia de 10/11 para 01/12 também mantém a janela original do agendamento (`app/ponto/services/ausencia.py:499`, `app/ferias/models.py:309`).

4. **BLOQUEANTE — exclusão confere agendamentos vivos fora da porta.** A view conta antes; o serviço depois apaga todos os agendamentos e o período, sem conferir novamente sob trava (`app/ferias/views.py:848`, `app/ferias/services.py:674`, `app/ferias/services.py:675`).

   **Contraexemplo:** a contagem retorna zero; outro pedido cria um agendamento; a exclusão seguinte pode apagar esse agendamento recém-criado. O caminho usa `QuerySet.delete`, em vez do método de instância que limpa as Ausencias (`app/ferias/services.py:675`, `app/ferias/models.py:453`). Resultado desse entrelaçamento com FKs e signals em execução: **[não verificado]**.

5. **BLOQUEANTE — snapshot, apply e reversão não identificam a mesma alteração com segurança.**

   - **Foto anterior à trava:** o command fotografa antes de a porta reler e recalcular (`app/ferias/management/commands/realinhar_concedidos_ferias.py:72`, `app/ferias/management/commands/realinhar_concedidos_ferias.py:81`, `app/ferias/services.py:144`). **Contraexemplo:** fotografa 30→33; cancelamento concorrente deixa concedidos −3/soma 0; apply faz −3→0; snapshot continua 30→33 e a reversão pula porque 0≠33 (`app/ferias/services.py:538`, `app/ferias/services.py:145`, `app/ferias/services.py:167`).
   - **Sobrescrita do arquivo:** nome distingue somente segundos e abre com `w`, inclusive sem períodos para aplicar (`app/ferias/management/commands/realinhar_concedidos_ferias.py:71`, `app/ferias/management/commands/realinhar_concedidos_ferias.py:75`). **Contraexemplo:** dois applies no mesmo segundo; o segundo, já alinhado, substitui a reversão útil por `periodos=[]` (`app/ferias/management/commands/realinhar_concedidos_ferias.py:74`, `app/ferias/management/commands/realinhar_concedidos_ferias.py:76`).
   - **Retorno ao mesmo número:** reversão compara apenas `dias_concedidos`, sem versão ou identidade dos agendamentos (`app/ferias/services.py:167`). **Contraexemplo:** realinha 30→33; cancela o original→0; registra outro histórico de 33→33; snapshot antigo restaura 30, reabrindo a divergência e o cancelamento seguinte em −3 (`app/ferias/services.py:538`, `app/ferias/services.py:416`, `app/ferias/services.py:170`).

6. **BLOQUEANTE — trilha obrigatória pode falhar e a mudança confirmar.** Realinhamento e reversão ignoram o retorno de `evento`; esse escritor captura exceções, desfaz somente a auditoria em savepoint e retorna `None` (`app/ferias/services.py:153`, `app/ferias/services.py:173`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`).

   **Contraexemplo:** falha em `LogAuditoria.objects.create`; concedidos muda de 30 para 33 sem a linha antes/depois exigida. O erro é registrado pelo mecanismo de observabilidade, mas não impede a alteração (`app/core/observ.py:90`, `app/core/observ.py:134`).

7. **BLOQUEANTE — contador existe, mas não é lido pelo ESTADO; o tripwire pode retornar OK com divergência.** Os dois sentidos existem em `ferias_concedidos_x_soma`; o command imprime ambos, mas os exclui do total do alarme (`app/ferias/services.py:116`, `app/ferias/services.py:125`, `app/ferias/management/commands/tripwire_status_ferias.py:30`, `app/ferias/management/commands/tripwire_status_ferias.py:35`). A composição do ESTADO não incorpora esse contador (`bin/gerar_estado.py:178`, `bin/gerar_estado.py:304`, `bin/gerar_estado.py:370`).

   **Contraexemplo:** concedidos 30, soma 33, status `concedido` coerente. O tripwire imprime `soma_maior=1`, mas pode retornar “OK”/rc 0 porque não existe divergência de status (`app/ferias/management/commands/tripwire_status_ferias.py:39`, `app/ferias/management/commands/tripwire_status_ferias.py:45`).

8. **BLOQUEANTE — a lista proposta para os 40 dias consulta outro universo.** O command reutiliza a folha, cuja fonte é a janela de **AgendamentoFerias**; isso não demonstra equivalência com os dias medidos por **Ausencia** (`app/ferias/management/commands/listar_ferias_com_batida.py:47`, `app/folha/export.py:472`, `app/folha/export.py:491`).

   **Contraexemplo:** Ausencia de férias aprovada + batida no dia, sem agendamento correspondente: a consulta por Ausencia encontra, a lista nova omite. O inverso também pode existir: a materialização admite manter agendamento com dias de Ausencia recusados e encaminhados ao DP (`app/ponto/services/ausencia.py:1203`, `app/ponto/services/ausencia.py:1210`, `app/ponto/services/ausencia.py:1217`). Alcançar exatamente 40 dias, 12 colaboradores e os quatro dias da competência 11: **[não verificado]**.

9. **BLOQUEANTE — encaminhamento do histórico futuro perde o preenchimento fora de HTMX; aceitação dos mesmos 32 não está comprovada.** Só a ramificação HX transmite `prefill`; a convencional volta ao saldo sem datas (`app/ferias/views.py:213`, `app/ferias/views.py:217`, `app/ferias/views.py:219`, `app/ferias/views.py:138`).

   **Contraexemplo:** POST convencional de histórico 01–30/12 recebe recusa e retorna ao saldo, sem abrir o agendamento preenchido; esse saldo apenas oferece o link (`app/templates/ferias/saldo.html:46`). O RED cobre um caso sintético HTMX; o teste convencional só verifica que não nasceu agendamento (`app/ferias/test_hj090_um_juiz.py:271`, `app/ferias/test_hj090_um_juiz.py:287`). Resultado da simulação dos mesmos 32, motivos das recusas e eventual linha `lei:`: **[não verificado]**.

10. **BLOQUEANTE estrutural — o cálculo do saldo ainda tem duas implementações.** O registro aponta `PeriodoAquisitivo.dias_saldo`, mas `status_de_periodo` repete direito−concedidos−vendidos em vez de consultar essa property (`app/core/juizes.py:264`, `app/ferias/models.py:95`, `app/ferias/models.py:217`). A catraca exclui o arquivo inteiro, permitindo esse clone interno (`app/core/juizes.py:1458`).

    **Contraexemplo de manutenção:** alterar o juiz registrado muda o saldo mostrado, enquanto o status continua julgando pela fórmula antiga. Divergência numérica atual entre essas duas fórmulas para os dados fornecidos: **[não verificado]**.

11. **NAO-BLOQ — relógios internos e um command escritor sem `--apply` permanecem.** Serviços ainda leem `date.today()` para aviso/carimbo; o histórico recebe `hoje`, mas calcula status sem repassá-lo (`app/ferias/services.py:432`, `app/ferias/services.py:544`, `app/ferias/services.py:571`, `app/ferias/services.py:399`).

    **Contraexemplo:** atravessar meia-noite entre a captura de `hoje` e `status_por_data` permite recusa e status julgados por dias diferentes (`app/ferias/services.py:391`, `app/ferias/models.py:280`). Fuso configurado é São Paulo; erro UTC/local efetivo em produção: **[não verificado]** (`app/config/settings/base.py:93`). `alertar_ferias_vencendo` também pode gravar status/direito sem flag apply (`app/ferias/management/commands/alertar_ferias_vencendo.py:14`, `app/ferias/management/commands/alertar_ferias_vencendo.py:32`, `app/ferias/services.py:321`).

12. **NAO-BLOQ — leitores ainda ocultam déficit ou misturam reserva e gozo.** A API recebe soma apenas dos saldos positivos e uma lista que exclui períodos negativos (`app/ferias/situacao.py:296`, `app/ferias/situacao.py:297`, `app/api/views_mensageria.py:835`). **Contraexemplo:** saldos −3 e 30 produzem `saldo_dias=30`, omitindo o primeiro período nessa lista (`app/api/views_mensageria.py:836`).

    Concedidos também aparece como “Gozo”/“dias gozados”, embora seja incrementado ao reservar férias futuras; período negativo com status concedido recebe “Nada pendente” (`app/templates/ferias/_secao_colab.html:44`, `app/templates/ferias/_secao_colab.html:49`, `app/ferias/situacao.py:179`, `app/ferias/services.py:443`). As duas telas principais, porém, imprimem o saldo negativo sem corte (`app/templates/ferias/_secao_colab.html:50`, `app/templates/ferias/saldo.html:41`).

A conferência individual dos avais fica assim:

| Item | Verificação |
|---|---|
| **1221(1): quatro escritores com trava e atomic** | **Cumprido nesses quatro corpos:** histórico, executar, trocar e aprovar; **não cumprido como “todo escritor”**, pelos achados 2–4 (`app/ferias/services.py:393`, `app/ferias/services.py:433`, `app/ferias/services.py:563`, `app/ferias/services.py:609`). |
| **1221(2): saldo desce para `_trocar_janela`** | **Cumprido no código:** consulta ao juiz ocorre após travar agendamento e período; views traduzem a exceção (`app/ferias/services.py:564`, `app/ferias/services.py:565`, `app/ferias/views.py:509`, `app/ferias/views.py:694`). |
| **1221(3): porta, DRY, reversão, trilha, negativo e ESTADO** | **Parcial:** DRY padrão, porta e apresentação negativa existem; reversão/trilha falham nas condições descritas; contador não chega ao ESTADO (`app/ferias/management/commands/realinhar_concedidos_ferias.py:65`, `app/ferias/services.py:143`, `app/templates/ferias/_secao_colab.html:50`, `bin/gerar_estado.py:370`). |
| **1221(4): RED cancelar 33 em concedidos 30 pela tela** | **Não demonstrado pela tela:** os casos realinham/cancelam diretamente por serviço. RED anterior/GREEN atual: **[não verificado]** (`app/ferias/test_hj090_um_juiz.py:109`, `app/ferias/test_hj090_um_juiz.py:119`). |
| **1224(1): só soma>concedidos; inverso ao DP** | **Cumprido no algoritmo:** porta ignora soma≤concedidos; lista imprime os dois números; contador separa sentidos (`app/ferias/services.py:146`, `app/ferias/management/commands/listar_ferias_dp.py:57`, `app/ferias/services.py:125`). |
| **1224(2): 52/32 não movidos; listas; medição prévia** | Listagem padrão consulta os registros e imprime os campos pedidos; `--simular` admite somente sombra e reverte a transação (`app/ferias/management/commands/listar_ferias_dp.py:52`, `app/ferias/management/commands/listar_ferias_dp.py:62`, `app/ferias/management/commands/listar_ferias_dp.py:77`, `app/ferias/management/commands/listar_ferias_dp.py:110`). Discriminação medida dos 52 por origem×status no RELATO antes do construtor: **[não verificado]**. |
| **1224(3): recusa com destino preenchido e RED dos 32** | **Parcial:** recusa na porta e preenchimento HTMX existem; caminho convencional e comprovação dos 32 faltam, conforme achado 9 (`app/ferias/services.py:391`, `app/templates/ferias/_secao_colab.html:57`, `app/templates/ferias/_secao_colab.html:65`). |
| **1224(4): 40 dias, dono, véspera e quatro no topo** | **Parcial:** marca véspera e prioriza competência corrente; universo diverge. Dono pode sair `ESTRUTURA`, `None` ou erro, sem resolver o encaminhamento pedido (`app/ferias/management/commands/listar_ferias_com_batida.py:57`, `app/ferias/management/commands/listar_ferias_com_batida.py:64`, `app/ponto/management/commands/e6_oraculo.py:110`, `app/ponto/management/commands/e6_oraculo.py:193`). Classificação e presença dos quatro casos fornecidos: **[não verificado]**. |

O censo de escrita e leitura dos valores tocados pelo diff é:

| Campo/valor | Quem escreve | Quem lê/deriva |
|---|---|---|
| **PA.dias_concedidos** | Realinhar/reverter; histórico; agendar; cancelar DP/admin; trocar; aprovar (`app/ferias/services.py:149`, `:170`, `:416`, `:443`, `:511`, `:538`, `:569`, `:640`). | Saldo/status; contador; validações; edição de direito; telas e relatórios (`app/ferias/models.py:95`, `app/ferias/models.py:217`, `app/ferias/services.py:125`, `app/ferias/views.py:824`, `app/templates/ferias/saldo.html:37`, `app/relatorios/services.py:290`). |
| **PA.dias_vendidos** | Agendamento+abono; histórico; venda isolada (`app/ferias/services.py:244`, `app/ferias/services.py:417`, `app/ferias/services.py:721`). | Saldo, teto vendável, status e apresentação (`app/ferias/models.py:95`, `app/ferias/models.py:217`, `app/ferias/models.py:235`, `app/templates/ferias/_secao_colab.html:51`). |
| **PA.dias_saldo** | **Property, sem coluna gravada** (`app/ferias/models.py:216`). | Validadores; remarcar; situação; listas; alertas; APIs; IA; relatórios (`app/ferias/views.py:241`, `app/ferias/models.py:222`, `app/ferias/situacao.py:69`, `app/ferias/management/commands/listar_ferias_dp.py:68`, `app/ferias/management/commands/alertar_ferias_vencendo.py:36`, `app/api/views_mensageria.py:840`, `app/core/views_ia.py:492`, `app/relatorios/services.py:316`, `app/relatorios/views.py:370`). |
| **PA.status** | `aplicar_status` atribui; portas acima persistem; sincronização e garantia escrevem sem trava; criação recebe defaults (`app/ferias/models.py:212`, `app/ferias/services.py:321`, `app/ferias/services.py:341`, `app/ferias/services.py:348`). | `pode_agendar/pode_vender`, escolha da aprovação, listas DP e relatórios (`app/ferias/models.py:231`, `app/ferias/models.py:239`, `app/ferias/services.py:619`, `app/ferias/management/commands/listar_ferias_dp.py:67`, `app/relatorios/services.py:281`). |
| **AG.dias/datas** | Histórico, agendamento, aprovação e troca (`app/ferias/services.py:395`, `app/ferias/services.py:440`, `app/ferias/services.py:629`, `app/ferias/services.py:570`). | Soma canônica; devolução no cancelamento; saldo de remarcação; fracionamento; materialização; listas e folha (`app/ferias/services.py:106`, `app/ferias/services.py:511`, `app/ferias/models.py:222`, `app/ferias/models.py:360`, `app/ponto/services/ausencia.py:1195`, `app/folha/export.py:491`). |
| **AG.status** | Criações; cancelamentos; troca; cron (`app/ferias/models.py:316`, `app/ferias/services.py:399`, `app/ferias/services.py:514`, `app/ferias/services.py:546`, `app/ferias/services.py:575`, `app/ferias/services.py:743`). | Guardas travadas; soma; consultas por janela; materializador; telas/listas (`app/ferias/services.py:495`, `app/ferias/services.py:106`, `app/ferias/services.py:53`, `app/ponto/services/ausencia.py:1181`, `app/ferias/views.py:81`). |
| **Ausencia de férias** | Espelhamento cria/remove; cancelamento rejeita/remove; edição e decisão genéricas também alteram (`app/ponto/services/ausencia.py:1191`, `app/ponto/services/ausencia.py:1203`, `app/ponto/services/ausencia.py:1271`, `app/ponto/services/ausencia.py:1282`, `app/ponto/services/ausencia.py:509`, `app/ponto/services/ausencia.py:836`). | Cobertura e consumidores disparados pelos signals; o próprio espelhamento lê existentes (`app/ponto/turnos.py:2538`, `app/ponto/signals.py:143`, `app/ponto/signals.py:250`, `app/ponto/services/ausencia.py:1182`). |

Também permanecem superfícies públicas sem trava nos modelos: `PeriodoAquisitivo.save` só verifica sobreposição; `AgendamentoFerias.save` grava e espelha sem chamar `full_clean`; seu `delete` remove Ausencias, mas não devolve concedidos (`app/ferias/models.py:201`, `app/ferias/models.py:446`, `app/ferias/models.py:453`). Chamador de produção que invoque diretamente esse `delete` de instância fora das portas: **[não verificado]**. O admin de férias registra somente Afastamento e o admin de Ausencia é de leitura (`app/ferias/admin.py:4`, `app/ponto/admin.py:38`).

Quanto à ordem, os caminhos inspecionados de cancelamento/troca/confirmacão fazem **agendamento→período→Ausencia**; criadores travam período e criam agendamento novo; realinhar/reverter travam apenas período (`app/ferias/services.py:494`, `app/ferias/services.py:497`, `app/ferias/services.py:541`, `app/ferias/services.py:578`, `app/ferias/services.py:394`, `app/ferias/services.py:434`, `app/ferias/services.py:618`, `app/ferias/services.py:144`). O cron de status trava somente agendamento e depois espelha Ausencias (`app/ferias/services.py:740`, `app/ferias/services.py:744`, `app/ferias/models.py:450`). Caminho comprovado de período→trava de agendamento existente ou deadlock reproduzido: **[não verificado]**.

Os contraexemplos adicionais pedidos têm estes resultados estáticos:

| Caso | Resultado |
|---|---|
| **Cancelar depois do realinhamento** | Alinhado 33→cancelar 33 resulta concedidos 0; há assertiva desse serviço, sem POST nesse cenário (`app/ferias/services.py:538`, `app/ferias/test_hj090_um_juiz.py:119`). |
| **Venda+agendamento simultâneos** | Corrida permanece: validações anteriores à trava, conforme achado 1 (`app/ferias/views.py:297`, `app/ferias/views.py:962`, `app/ferias/services.py:434`). |
| **Período com abono** | Direito 30, vendidos 10, soma 23: realinhar grava concedidos 23 e mantém saldo −3; contador fica alinhado porque compara somente concedidos×soma (`app/ferias/services.py:149`, `app/ferias/models.py:217`, `app/ferias/services.py:125`). |
| **Histórico atravessa dois períodos** | Todo o intervalo é debitado no PA escolhido pelo início; não há repartição (`app/ferias/views.py:205`, `app/ferias/services.py:398`, `app/ferias/services.py:416`). Obrigação de repartir nos avais fornecidos: **[não verificado]**. |
| **Reversão duas vezes** | Imediatamente, a segunda é recusada porque concedidos já voltou ao valor anterior; retorno posterior ao mesmo número reabre a aplicação do snapshot antigo (`app/ferias/services.py:167`, `app/ferias/services.py:170`). |
| **Realinhar duas vezes** | Porta não grava novamente quando soma=concedidos; o comman

[truncado em 20000 de 20715 caracteres]

## 13/11 13:59 · sessao 92e95c273df1 · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — ordem de trava do lote diverge da porta admin; deadlock possível, ocorrência [nao verificado].** O admin trava Colaborador antes de validar/materializar (`app/chamados/services/validacao.py:474`, `:479`, `:507`). O lote abre atomic e chama diretamente o materializador, sem travar Colaborador no topo (`app/chamados/services/passivo_validado.py:374`, `:433`, `:517`, `:522`). O materializador cria Batida (`app/chamados/services/materializacao.py:1053`); seu signal recomputa TurnoMaterializado (`app/ponto/signals.py:34`) com DELETE/INSERT (`app/ponto/nucleo.py:57`, `:79`). Só depois reconcilia destino/clones (`app/chamados/services/materializacao.py:1100`, `:886`), cujo gate toma Colaborador (`app/chamados/models.py:708`, `:712`, `:2687`). Contraexemplo de concorrência: lote segura linha TurnoMaterializado; admin toma Colaborador e espera aquela linha no recompute; lote tenta tomar Colaborador no fechamento. É inversão herdada pelo caminho novo do par; sua introdução histórica [nao verificado].

2. **NAO-BLOQ quanto a segundo escritor de Batida.** `_gravar_irma_do_par` delega a `_materializar_uma` (`app/chamados/services/materializacao.py:1258`), que chama `_materializar_resposta_hora` (`:1384`), que usa `registrar_batida` (`:1052`, `:1053`). A criação efetiva continua em `app/ponto/registro_batida.py:157`. `_validar_o_par` usa o mesmo materializador (`app/chamados/services/validacao.py:399`). Não encontrei escrita alternativa nesse percurso.

3. **NAO-BLOQ para rastros de banco no rollback; efeitos externos exigem ressalva.** Existem signals síncronos entre as duas gravações: núcleo (`app/ponto/signals.py:34`), cartório (`:131`) e reconciliador (`:206`). Contudo, o par roda em savepoint (`app/chamados/services/validacao.py:396`), a irmã em atomic aninhado (`app/chamados/services/materializacao.py:1255`) e cada materialização também (`:1382`). O outbox da Pergunta é escrito sincronicamente no save (`app/chamados/signals.py:253`, `:259`); os becos são `on_commit` (`:235`, `:240`); recálculo também é `on_commit` (`app/ponto/services/fechamento.py:1824`). O broadcast utiliza `pg_notify` na mesma conexão Django (`app/ponto/realtime.py:40`, `:42`). Não há prova de persistência desses rastros após rollback no código examinado.

   Há, porém, um caminho externo sem `on_commit`: signal → cartório (`app/ponto/signals.py:135`) → emissor de cobrança (`app/ponto/services/cartorio.py:1791`, `:1829`) → push imediato (`app/ponto/management/commands/detectar_ausencias.py:777`, `:779`) → `webpush` (`app/chamados/push_utils.py:40`). Que o estado intermediário do caso fornecido alcance efetivamente essa emissão é **[nao verificado]**; não transformar o caminho alcançável em incidente provado. A fábrica de perguntas silencia seu próprio push (`app/chamados/services/fabrica_da_celula.py:218`).

4. **Integração incompleta com lote — candidato a bloqueante, efeito exato [nao verificado].** O skip de irmã já materializada foi acrescentado apenas ao laço da disputa (`app/chamados/services/materializacao.py:1538`). O lote mantém a iteração sobre todo o snapshot (`app/chamados/services/passivo_validado.py:520`) e chama `_materializar_uma` para cada objeto (`:522`), mesmo se a pergunta anterior acabou de puxar sua irmã. `_materializar_uma` não verifica o carimbo existente antes do plantio (`app/chamados/services/materializacao.py:1340`, `:1384`) e chama novamente `carimbar` (`:1439`). Contraexemplo: grupo contém saída e volta validadas; saída grava ambas; o lote chega ao objeto volta antigo e tenta processá-lo outra vez. Recarimbo/trilha e efeitos concretos devem ser conferidos contra a implementação de `carimbar`; não executei teste.

## 13/11 13:59 · sessao ccf968f16d21 · - · - · origem=Ronald (tmux)

**BLOQUEANTE — o passivo já absolvido não é recuperado por quatro entradas independentes.** Contraexemplo: saída S01:00 respondida e absolvida por `VIA_PARIDADE_RECUSADA`, com `materializada_em` e `validada_em` preenchidos; volta E02:00 respondida posteriormente. O contador exclui a saída (`materializacao.py:191-193`); validar a própria saída retorna `ja_validada` (`validacao.py:433-435`); validar a volta não encontra a saída como irmã (`materializacao.py:136-139`, usado por `validacao.py:390`); laço/lote exclui a saída (`materializacao.py:1531-1534`). `_gravar_irma_do_par` compartilha a seleção da irmã e não constitui quinta entrada (`materializacao.py:1250-1251`). A devolução também não desfaz o carimbo, mas é guarda em série do clique, não quinto caminho (`validacao.py:190-193`). Quantos dos 60 pertencem a essa classe: **[nao verificado]**.

**BLOQUEANTE — tela promete par com irmã implausível que o clique recusa.** Contraexemplo: golden com saída respondida 01:00 e volta respondida 14:00 para marco 02:00, ambas não materializadas; a derivação da volta usa o marco 02:00 (`materializacao.py:511-518`), tornando o par projetado válido, e tela devolve `aplicavel` (`materializacao.py:563-568`). O clique chama `motivo_implausivel_validacao(q)` e desiste do par (`validacao.py:390-392`); juiz compara resposta 14:00 com marco 02:00 e acusa 720 minutos (`juizes.py:1799-1824`, `juizes.py:2002-2022`). Depois tenta saída isolada (`validacao.py:508-509`).

**BLOQUEANTE — tela promete par com dia do chamado da irmã divergente; porta recusa.** Contraexemplo: perguntas com mesmo `data_evento` 25/09, porém chamado que lista a volta diz 26/09, sem `data_conferida_em`. Seleção considera somente igualdade entre `data_evento` das perguntas (`materializacao.py:136-139`); projeção deriva hora sem conferir a divergência (`materializacao.py:143-163`), tela promete o par (`materializacao.py:563-568`). A escrita da irmã consulta `data_do_fato_de`, grava recusa `DATA_EM_DISPUTA` e retorna sem materializar (`materializacao.py:1353-1376`); o juiz obtém dia do chamado e compara (`fio.py:77-88`).

**BLOQUEANTE condicional — `materializada_em` ainda vale por “encerrou sem batida” e “gravou”, permitindo sucesso falso.** Se uma pergunta absolvida tiver `validada_em=None`, o clique passa pelo carimbo (`validacao.py:479`), pula materialização por `materializada_em` preenchido (`validacao.py:505`), o laço também a exclui (`materializacao.py:1531-1534`) e pode retornar “ponto gravado” (`validacao.py:567-568`), repassado pela casca (`acoes_disputa.py:62-64`). Esse estado pode ser produzido pelo escritor de resposta, que zera validação (`disputa_emissao.py:1470-1485`) mas não descarimba via de identidade (`models.py:3353-3356`). A chegada desse estado em produção/tela normal: **[nao verificado]**. A via de paridade carimba sem criar batida (`materializacao.py:1050-1051`); o campo declara “batida criada + chamado fechado” (`models.py:3445-3448`).

**NAO-BLOQ — não há contraexemplo confirmado de tela aplicável causado apenas pelos outros três atributos pedidos.** Irmã já materializada é excluída (`materializacao.py:138`); outro `data_evento` é excluído (`materializacao.py:137`); irmã retratada é excluída por `fora_do_fato` (`materializacao.py:139`, `models.py:3227`, `models.py:2414`). Já materializada com batida real pode permitir aplicação isolada legítima; já materializada por identidade sem batida não é projetada (`materializacao.py:138`, `materializacao.py:555-568`).

**Allowlist:** os dois escritores usam guardas de escrita (`materializacao.py:1251`, `validacao.py:391-399`); as duas entradas de leitura excluem justamente o passivo antes de consultar autoridade (`materializacao.py:138`, `materializacao.py:192`). Portanto, a justificativa “carimbo como porta” não prova atendimento do universo autorizado; as exceções escondem esse defeito do selo (`test_contract_leitores_vitalidade.py:104-114`, `test_contract_leitores_vitalidade.py:330-333`).

Mapa dos campos:

- `validada_em`: `_carimbar_validacao` escreve e registra decisão (`validacao.py:135-143`); devolução limpa (`validacao.py:197-210`); resposta nova limpa (`disputa_emissao.py:1470-1482`); carimbo simétrico também preenche (`models.py:3160-3163`). Leem: bloqueio inicial (`validacao.py:433`), par (`validacao.py:391-396`), irmã já validada (`materializacao.py:1251`), laço (`materializacao.py:1533`).
- `materializada_em`/via: atribuição canônica em `_escrever_carimbo` (`models.py:3123-3127`), persistência em `carimbar` (`models.py:3154-3178`) ou `carimbar_identidade` (`models.py:3208-3216`); recusa de paridade chama esta última (`materializacao.py:1050`). Leem seleção da irmã, contador, tela, comprovação do par e laço (`materializacao.py:138`, `materializacao.py:192`, `materializacao.py:542-550`, `validacao.py:399`, `materializacao.py:1534-1540`).
- `estado_aplicacao_pergunta`: deriva e retorna valor sem persistência (`materializacao.py:533-574`); explicador lê (`explicador.py:99-102`), contador lê (`materializacao.py:201`).
- `gravou`: explicador escreve apenas `False` para via recusada (`explicador.py:100-102`); desenlace lê ausência de falsos como autorização para dizer “Tudo validado e gravado” (`desenlace.py:65-73`).
- `msg`: service escreve (`validacao.py:563-568`); casca lê e repassa (`acoes_disputa.py:62-64`).

## 13/11 13:58 · sessao 7f21cd136ec7 · - · - · origem=Ronald (tmux)

- **BLOQUEANTE — duas exceções do selo encobrem o passivo.** `metade_irma_do_intervalo` exige `materializada_em IS NULL` antes de julgar o par ([materializacao.py:138](app/chamados/services/materializacao.py#L138)); o contador repete a exclusão ([materializacao.py:192](app/chamados/services/materializacao.py#L192)). Uma pergunta encerrada por `VIA_PARIDADE_RECUSADA`, com esse carimbo preenchido, desaparece das duas entradas. A allowlist autoriza exatamente esses leitores ([test_contract_leitores_vitalidade.py:109](app/chamados/tests/test_contract_leitores_vitalidade.py#L109)); o selo apenas verifica se o endereço está autorizado, sem validar o significado ([test_contract_leitores_vitalidade.py:329](app/chamados/tests/test_contract_leitores_vitalidade.py#L329)).

- **BLOQUEANTE para cumprimento integral do aval de 09/11 — mensagem “colada” permanece falsa.** Toda recusa de paridade continua traduzida como marcação “colada” ([recusa.py:26](app/chamados/catalogo/recusa.py#L26)), enquanto a guarda procura até 12 horas ([materializacao.py:100](app/chamados/services/materializacao.py#L100)). Contraexemplo já escrito: S 01:00 isolada recusa pela S 06:30; o teste exige recusa, mas não exige “falta a outra metade” ([test_bug_par_projetado.py:151](app/chamados/tests/test_bug_par_projetado.py#L151)). A leitura de que DESCONGELA/1246 revogou expressamente esse requisito é [nao verificado].

- **BLOQUEANTE de certificação — os dois ramos defensivos não estão demonstrados pelos testes novos.** A ordem `t_s < t_e` é exigida ([materializacao.py:154](app/chamados/services/materializacao.py#L154)), e a rechecagem pode levantar `_Reprovada` após gravar a irmã ([materializacao.py:997](app/chamados/services/materializacao.py#L997)). A fixture nova fixa saída 01:00 e volta 02:00 ([test_bug_par_projetado.py:61](app/chamados/tests/test_bug_par_projetado.py#L61)); os adversariais adicionam vizinha ofensora ou retiram validação, sem provocar esses dois ramos ([test_bug_par_projetado.py:162](app/chamados/tests/test_bug_par_projetado.py#L162), [test_bug_par_projetado.py:214](app/chamados/tests/test_bug_par_projetado.py#L214)). Execução de mutantes e sobrevivência efetiva: [nao verificado].

- **NAO-BLOQ — segundo escritor de Batida não demonstrado.** `_gravar_irma_do_par` chama `_materializar_uma` ([materializacao.py:1258](app/chamados/services/materializacao.py#L1258)); `_validar_o_par` também ([validacao.py:398](app/chamados/services/validacao.py#L398)). O plantio passa por `registrar_batida` ([materializacao.py:1052](app/chamados/services/materializacao.py#L1052)), e a vinculação da pergunta passa por `anotar` ([materializacao.py:1073](app/chamados/services/materializacao.py#L1073)). Nesse aspecto, realiza o Single Writer descrito no glossário ([GLOSSARIO.md:12](app/docs/GLOSSARIO.md#L12)).

- **NAO-BLOQ — duplicação do juiz registrado não demonstrada.** O registro atribui a `proximo_tipo_de` a pergunta sobre o tipo sugerido da próxima batida ([juizes.py:213](app/core/juizes.py#L213)); sua implementação usa estado do turno e marcos ([turnos.py:2450](app/ponto/turnos.py#L2450)). `_vizinha_mesmo_tipo` verifica vizinhas cronológicas de uma inserção retroativa ([materializacao.py:86](app/chamados/services/materializacao.py#L86)). São perguntas diferentes nesses contratos; contraexemplo de duplicação: [nao verificado].

- **NAO-BLOQ — derivação e relógio novos fora da borda não demonstrados.** O par reutiliza `_ts_tipo_do_plantio` para ambas as metades ([materializacao.py:150](app/chamados/services/materializacao.py#L150), [materializacao.py:172](app/chamados/services/materializacao.py#L172)); esse é o derivador compartilhado pelo plantio e pela tela ([materializacao.py:435](app/chamados/services/materializacao.py#L435)). As funções novas examinadas não introduzem leitura de `now()` ([materializacao.py:130](app/chamados/services/materializacao.py#L130), [validacao.py:379](app/chamados/services/validacao.py#L379)).

## 13/11 13:57 · sessao 554bcdd26943 · - · - · origem=Ronald (tmux)

**BLOQUEANTE — snapshot não representa necessariamente o apply.** A foto é obtida sem trava e gravada antes de a porta reler os valores ([command:72](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/management/commands/realinhar_concedidos_ferias.py:72), [command:81](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/management/commands/realinhar_concedidos_ferias.py:81), [services:143](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:143)). Contraexemplo: PA concedidos30/soma33; snapshot registra30→33; cancelamento concorrente do agendamento33 deixa concedidos−3/soma0; apply realinha−3→0, mas snapshot permanece30→33; reversão compara0 com33 e pula ([services:537](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:537), [services:145](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:145), [services:167](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:167)). Execução real desse entrelaçamento: [não verificado].

**BLOQUEANTE — rodar apply duas vezes pode apagar a reversão da primeira.** O nome do snapshot distingue somente segundos, abre com `w` e grava mesmo quando a segunda execução já não encontra divergências ([command:65](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/management/commands/realinhar_concedidos_ferias.py:65), [command:71](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/management/commands/realinhar_concedidos_ferias.py:71), [command:75](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/management/commands/realinhar_concedidos_ferias.py:75)). Contraexemplo: primeiro apply grava PA30→33; segundo apply no mesmo segundo sobrescreve o JSON com `periodos=[]`; desaparece a reversão útil. Ocorrência real: [não verificado].

**BLOQUEANTE — reversão identifica uma alteração apenas pelo número de concedidos.** Não confere identidade dos agendamentos, versão nem consumo do snapshot ([services:165](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:165)). Contraexemplo: realinha30→33; cancela original33→0; registra novo histórico33→33; aplica snapshot antigo e obtém30 apesar de soma33; cancelar o novo histórico produz−3 novamente ([services:538](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:538), [services:416](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:416), [services:170](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:170)). Duas reversões imediatamente consecutivas ficam protegidas pela diferença antes/depois; a proteção não cobre retorno posterior ao mesmo valor ([services:167](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:167)). Execução desses cenários: [não verificado].

**BLOQUEANTE — trilha obrigatória pode falhar sem impedir realinhamento/reversão.** As portas ignoram o retorno de `evento`, cuja implementação captura erro e retorna `None`, com savepoint que preserva a escrita de negócio ([services:153](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:153), [services:173](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:173), [observ:105](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/core/observ.py:105), [observ:133](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/core/observ.py:133), [observ:139](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/core/observ.py:139)). Contraexemplo: `LogAuditoria.objects.create` falha; PA confirma30→33 sem linha antes/depois na auditoria. Falha real de auditoria: [não verificado].

**NAO-BLOQ — ordem de trava não mostrou ciclo concreto nos caminhos examinados.** Cancelamento, troca e confirmação fazem agendamento→período; histórico/agendamento/aprovação travam período e criam agendamento novo; realinhamento/reversão travam somente período e consultam soma sem `select_for_update` ([services:494](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:494), [services:394](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:394), [services:434](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:434), [services:618](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:618), [services:143](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:143)). Cron de status faz agendamento→Ausência, sem travar período ([services:740](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:740), [models:450](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/models.py:450)). Signals de Ausência chamam cartório síncrono e consumidor após commit; não identifiquei retorno à trava de agendamento/período nesses callbacks ([signals:143](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ponto/signals.py:143), [signals:250](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ponto/signals.py:250)). Deadlock efetivamente reproduzido: [não verificado].

**NAO-BLOQ — casos básicos têm assertivas úteis, mas não provam os adversariais acima.** Existem assertivas de realinhar duas vezes sem trilha duplicada, cancelar após realinhamento e recusar reversão quando concedidos ficou diferente; a reversão do command é exercitada uma vez ([teste:109](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/test_hj090_um_juiz.py:109), [teste:126](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/test_hj090_um_juiz.py:126), [teste:151](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/test_hj090_um_juiz.py:151)). Resultado de execução e evidência histórica RED: [não verificado].

**NAO-BLOQ — cruzar ciclos não divide o histórico.** A view escolhe o PA contendo o início e a porta carrega todo o intervalo nesse PA; soma, abono e cancelamento continuam nessa unidade ([views:205](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/views.py:205), [services:395](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:395), [services:416](/home/ronald/codex_ws/6b723726ebeb8083a71997160807fd0f937d621e/app/ferias/services.py:416)). Contraexemplo: gozo de20 dias iniciado dez dias antes da virada aquisitiva debita20 do primeiro PA. Obrigação de repartir entre PAs, ausente nos complementos recebidos: [não verificado].

## 13/11 13:56 · sessao e506e1f51428 · - · - · origem=Ronald (tmux)

Achados do censo, todos por leitura estática:

- **BLOQUEANTE — espelho de férias tem escritor independente.** `cancelar_ausencia` aceita qualquer Ausencia por PK, inclusive férias aprovada, e decide rejeição sem passar pelo agendamento/período (`app/ponto/views.py:2728`, `app/ponto/views.py:2734`); a transição é permitida (`app/ponto/services/ausencia.py:771`) e gravada (`app/ponto/services/ausencia.py:825`, `app/ponto/services/ausencia.py:836`). Contraexemplo: agendamento vivo de 30 dias, concedidos=30, cancelar uma das 30 Ausencias deixa férias no agendamento e rejeição no espelho. Nova sincronização não repara: inclui rejeitadas em `existentes` e pula qualquer data existente (`app/ponto/services/ausencia.py:1182`, `app/ponto/services/ausencia.py:1200`).

- **BLOQUEANTE — edição independente do espelho.** `editar_ausencia` carrega qualquer PK e chama serviço sem excluir férias (`app/ponto/views.py:2761`, `app/ponto/views.py:2794`). O serviço altera tipo/data/fim/duração e salva sem trava de agendamento/período (`app/ponto/services/ausencia.py:498`, `app/ponto/services/ausencia.py:509`). Contraexemplo: mover Ausencia de férias de 10/11 para 01/12 mantém janela e concedidos antigos. O veto de férias do Form existe, mas essa view não usa o Form (`app/ponto/forms.py:32`, `app/ponto/forms.py:65`).

- **BLOQUEANTE — nem todo escritor de status PA trava.** `sincronizar_status_periodo` decide e grava pela foto recebida (`app/ferias/services.py:307`, `app/ferias/services.py:315`, `app/ferias/services.py:321`); `garantir_periodo_aquisitivo` lê sem trava e faz update de status (`app/ferias/services.py:344`, `app/ferias/services.py:348`). Contraexemplo: cron lê PA adquirido/saldo30; agendamento concorrente consome30 e grava concedido; cron grava status derivado da foto de saldo30. Os chamadores incluem GET de saldo (`app/ferias/views.py:116`), seção (`app/ferias/views.py:781`) e alerta (`app/ferias/management/commands/alertar_ferias_vencendo.py:32`).

- **BLOQUEANTE — exclusão sem conferir vivos sob trava.** View confere agendamentos antes da porta (`app/ferias/views.py:848`); porta apaga todos por queryset e depois PA, sem revalidar (`app/ferias/services.py:674`, `app/ferias/services.py:675`). Contraexemplo: conferência retorna zero; outro request agenda; exclusão encontra e apaga o recém-criado, sem o `AgendamentoFerias.delete` que apaga Ausencias (`app/ferias/models.py:453`). Órfãs após o interleaving dependem do resultado de FKs/execução [não verificado].

- **BLOQUEANTE estrutural — saldo calculado duas vezes.** Juiz registrado: `PeriodoAquisitivo.dias_saldo` (`app/core/juizes.py:264`), fórmula em `app/ferias/models.py:217`; `status_de_periodo` repete direito−concedidos−vendidos (`app/ferias/models.py:95`). A catraca exclui o arquivo inteiro, portanto não percebe esse clone interno (`app/core/juizes.py:1458`). Exemplo: evolução do juiz de saldo altera saldo mostrado, enquanto status conserva a fórmula antiga.

- **NAO-BLOQ — command escreve sem `--apply`.** `alertar_ferias_vencendo` não declara argumento apply e chama sincronização que pode gravar (`app/ferias/management/commands/alertar_ferias_vencendo.py:14`, `app/ferias/management/commands/alertar_ferias_vencendo.py:32`; escrita `app/ferias/services.py:321`). Exemplo: executar apenas para alertar muda status/direito.

- **NAO-BLOQ — API omite déficit do saldo.** `situacao_de_ferias` soma e lista apenas PA com saldo positivo (`app/ferias/situacao.py:296`, `app/ferias/situacao.py:297`); API publica o total/lista (`app/api/views_mensageria.py:835`, `app/api/views_mensageria.py:836`). Exemplo: A=−3, B=30 produz saldo_dias=30 e omite A. As duas telas imprimem saldo negativo diretamente, sem corte (`app/templates/ferias/_secao_colab.html:50`, `app/templates/ferias/saldo.html:41`).

- **NAO-BLOQ — significado exibido não acompanha reserva futura.** Concedidos+vendidos vira “dias gozados” (`app/ferias/situacao.py:179`, `app/ferias/situacao.py:182`); seção chama concedidos de Gozo (`app/templates/ferias/_secao_colab.html:49`) e status concedido recebe verde “Nada pendente” mesmo com déficit (`app/templates/ferias/_secao_colab.html:44`). Exemplo: gozo futuro reservado33 recebe descrição de dias gozados e selo sem pendência.

Censo adicional de escritores/derivadores:

| Campo | Escritores / mecanismo |
|---|---|
| PA concedidos | realinhar/reverter `services.py:149`, `:170`; histórico `:416`; agendar `:443`; cancelar `:511`, `:538`; trocar janela `:569`; aprovar `:640` |
| PA vendidos | agendar+abono `services.py:244`; histórico `:417`; vender `:721` |
| PA saldo | property, não coluna: `models.py:216` |
| PA status | `aplicar_status` atribui `models.py:212`; persistências acima; sincronizar `services.py:321`; garantir `:348`; defaults criação `:341` |
| PA save | público, só guarda sobreposição e super.save, sem trava `models.py:201` |
| AG dias/datas/status | criação histórico `services.py:395`; criação agendar `:440`; troca `:570`; aprovação `:629`; cron status `:743`; cancelamentos em `:514`, `:543` |
| AG save/delete | save público guarda e salva, depois espelha; não full_clean nem trava `models.py:446`; delete apaga Ausencias e agendamento, sem devolver concedidos `models.py:453` |
| Ausencia férias | `espelhar_ferias`: cria aprovadas por dia `ponto/services/ausencia.py:1203`; remove inativas `:1191`; apagar janela `:1271`; rejeitar janela `:1282`; edição/decisão genéricas acima |

Principais leitores PA: saldo/teto/status nos validadores `app/ferias/views.py:241`, `:824`, `:964`; relatórios `app/relatorios/services.py:290`, `:316`, `app/relatorios/views.py:370`; IA `app/core/views_ia.py:262`, `:492`, `:503`, `:532`; APIs `app/api/views_mensageria.py:840`, `:954`, `:1103`; situação lê juiz `app/ferias/situacao.py:69`; pautas `app/ferias/management/commands/pautas_de_ferias.py:97`; alertas `app/ferias/management/commands/alertar_ferias_vencendo.py:36`.

Admin não abre os campos PA/AG: registro de férias é somente Afastamento (`app/ferias/admin.py:4`); AusenciaAdmin herda SoLeitura (`app/ponto/admin.py:38`). Cron de status declara apply (`app/config/crons.py:442`), mas escritor `alinhar_status_por_data` trava apenas AG, depois espelha Ausencias (`app/ferias/services.py:740`, `:744`; gancho `app/ferias/models.py:450`). Signals de Ausencia julgam consumidores (`app/ponto/signals.py:143`, `:150`); não foi encontrada escrita de PA/AG nesses signals [não verificado quanto a chamadas dinâmicas].

## 13/11 13:56 · sessao 3e4521b2b598 · - · - · origem=Ronald (tmux)

Achados para consolidação:

1. **BLOQUEANTE — contador fora do ESTADO e sem alarme efetivo.** `ferias_concedidos_x_soma` existe e separa os sentidos em `app/ferias/services.py:116-128`; `tripwire_status_ferias` o imprime em `app/ferias/management/commands/tripwire_status_ferias.py:35-38`, mas o exclui de `total` e retorna “OK” quando os status coincidem (`:30`, `:39-41`). `bin/gerar_estado.py:178-390` não lê esse contador, direta ou indiretamente pelos placares importados. Contraexemplo: concedidos=30, agendamento=33, status concedido coerente; tripwire pode imprimir soma_maior=1 e sair OK/rc0, sem publicação dessa divergência no ESTADO. Código do retorno: `tripwire_status_ferias.py:39-46`.

2. **BLOQUEANTE — lista dos 40 usa outro universo.** O command chama a autoridade da folha (`app/ferias/management/commands/listar_ferias_com_batida.py:47`), que cruza **AgendamentoFerias** com batidas (`app/folha/export.py:491-502`; `app/ferias/services.py:49-56`). O usuário identifica o universo medido por **Ausencia**; igualdade dos universos não foi provada. Contraexemplo: Ausencia férias aprovada em D + batida em D, sem AgendamentoFerias correspondente: sonda de Ausencia conta D e esta lista o omite. Inversamente, materialização pode recusar a Ausencia e manter o agendamento: `app/ponto/services/ausencia.py:1202-1217`. Portanto obter os mesmos 40/12 colaboradores/4 da competência 11 é **[não verificado]**, sem repetir a medição pela fonte original.

3. **BLOQUEANTE — encaminhamento futuro perde datas fora de HTMX.** Só a ramificação HX entrega `prefill` (`app/ferias/views.py:213-219`). Fora de HX, `_falha` redireciona ao saldo sem datas (`:135-138`); o saldo oferece link de agendamento (`app/templates/ferias/saldo.html:46-49`). Contraexemplo: POST convencional com gozo futuro 01–30/12 recebe recusa e volta ao saldo, sem abrir o agendamento preen­chido. A implementação HTMX abre e preenche os formulários (`app/templates/ferias/_secao_colab.html:57-68`).

4. **BLOQUEANTE de comprovação — RED dos mesmos 32 não demonstrado.** O teste cobre um único caso sintético HTMX (`app/ferias/test_hj090_um_juiz.py:266-285`); o teste sem HTMX só verifica que não nasceu agendamento (`:287-289`). O command `--simular` consulta `_validar_agendamento` para candidatos em transação revertida (`app/ferias/management/commands/listar_ferias_dp.py:98-115`), mas a saída para os 32 casos e eventual linha `lei:` são **[não verificado]**. Nenhum resultado HJ-090 localizado em `app/docs/RELATO.md`/`RELATO-ARQUIVO.md`; ausência da medição anterior ao construtor é **[não verificado]**.

5. **NAO-BLOQ isoladamente — lista preserva resposta do juiz, inclusive sem dono BATIDA/CADASTRO.** `InsumosDoDono.dono` pode devolver None sem lavra CXR (`app/ponto/management/commands/e6_oraculo.py:189-195`); o juiz pode devolver ESTRUTURA (`:106-110`). O command imprime ambos literalmente (`app/ferias/management/commands/listar_ferias_com_batida.py:59-66`). Contraexemplo: duas batidas, cadastro coerente, CXR não lavrada → None. Cumprimento de “cada um com dono BATIDA ou CADASTRO” para os 40 é **[não verificado]**; não se deve inventar um segundo juiz para coagir a resposta.

6. **NAO-BLOQ — separação da véspera e ordenação existem, cobertura medida falta.** O command usa batidas apuráveis, pergunta o dia das batidas e marca véspera quando todas pertencem ao dia anterior (`app/ferias/management/commands/listar_ferias_com_batida.py:53-57`). Competência corrente é ordenada primeiro (`:64`, `:71`). Contraexemplo adversarial ausente na bateria HJ090: única saída 07h de plantão iniciado na véspera; classificação real desse caso é **[não verificado]**.

7. **NAO-BLOQ — listas DP implementadas, recorte agregado solicitado não publicado.** Concedidos>soma sai com ambos os números (`app/ferias/management/commands/listar_ferias_dp.py:55-60`); POSTERIOR imprime colaborador/agendamento/PA usado/status/anteriores/saldo/origem (`:62-75`); futuro imprime colaborador/agendamento/PA/status/data do registro (`:77-84`). Origem deriva de texto livre de `obs` (`:26-32`), portanto pode ser ambígua: agendamento normal com observação contendo “gozo historico” entra como histórico. Medição dos 52 discriminados por origem × status antes do construtor é **[não verificado]**.

8. **NAO-BLOQ — RED do cancelamento tem mordida de serviço, não da tela pedida.** O teste inicia concedidos=30/agendamento=33, realinha, chama cancelamento de serviço e verifica zero (`app/ferias/test_hj090_um_juiz.py:97-120`). Não faz POST na tela nesse caso. O teste do novo histórico 33 também cancela diretamente pelo serviço (`:78-85`). Execução RED anterior/GREEN atual: **[não verificado]**.

Cumprimentos estáticos úteis:

- Saldo negativo é apresentado sem corte em `app/templates/ferias/_secao_colab.html:50` e `app/templates/ferias/saldo.html:41`; o teste afirma -3 em `app/ferias/test_hj090_um_juiz.py:82`.
- Realinhamento tem DRY padrão e só entra no apply com flag (`app/ferias/management/commands/realinhar_concedidos_ferias.py:57-67`); snapshot em logs antecede porta (`:69-81`), ressalvadas corridas que o parent está analisando.
- Listagem padrão do DP não grava; a exceção explícita `--simular` só admite banco chamado sombra e usa rollback (`app/ferias/management/commands/listar_ferias_dp.py:52-53`, `:98-111`).
- Juiz de férias com batidas da folha é reutilizado, sem clone aritmético no command (`app/ferias/management/commands/listar_ferias_com_batida.py:47`); isso não prova equivalência com o universo da sonda de Ausencia (`app/folha/export.py:491-502`).

## 13/11 13:29 · sessao 05c3d77a4e24 · DESLIGADO-NAO-TRAVA · verificador · origem=Code

**Pré-verificação: BLOQUEANTE. O contador `desligados_retidos`, esperado 0 e lido pelo ESTADO, não existe na cópia examinada.** `barrados_desligado` mede outra coisa: pares excluídos do comparador C5. A integração existente do ESTADO lê o carimbo de fio/C5. (`bin/gerar_estado.py:260`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `app/chamados/management/commands/disparar_perguntas_competencia.py:61`)

A identidade da cópia com o commit/base informados não pôde ser conferida por Git: a cópia não contém repositório `.git`. `[nao verificado]` [nao verificado]

**1. BLOQUEANTE — o informativo foi produzido, mas não integrado à apresentação do DP.**

`informativo_rescisao` usa o mesmo laço do retentor e aparece no retorno de `o_que_segura`. Entretanto, o lavrador do Retrato não grava esse informativo; a tela e o drawer continuam lendo os campos antigos do Retrato. Existe acesso manual pela ferramenta; não está implementada nesses caminhos a apresentação dos dias ao DP. (`app/folha/export.py:1228`; `app/folha/fora_da_folha.py:232`; `app/folha/retrato_folha.py:1177`; `app/folha/views_retrato.py:190`; `app/colaboradores/views.py:984`)

O teste comprova a chave no retorno da função, sem percorrer tela/PDF do DP. (`app/folha/tests/test_lei_desligado_nao_trava.py:100`)

**2. BLOQUEANTE — desligado ainda pode sofrer trava de aprovação por outro caminho.**

`classificar_export` acrescenta `turno_aberto` a `travas_aprovacao` sem excluir desligados. A aprovação individual recusa essa trava; o lote também exclui esses fechamentos. Portanto, zerar `_dias_que_retem` não zerou todas as travas da folha. (`app/folha/export.py:833`; `app/folha/export.py:848`; `app/ponto/views.py:1509`; `app/ponto/views.py:1308`)

O carimbo automático na geração também conserva esse crivo. Isso **não demonstra impedimento de emissão da linha no TXT**: a montagem aceita fechamentos `aberto` e `aprovado`, selecionados pelo resultado `entra`. (`app/folha/views.py:244`; `app/folha/export.py:687`; `app/folha/export.py:716`)

O teste novo verifica somente que o desligado não seja `('fora', 'furo_espelho')`; não exige ausência de `travas_aprovacao`. (`app/folha/tests/test_lei_desligado_nao_trava.py:89`)

**3. BLOQUEANTE — o requisito “nenhum leitor deriva por conta própria” não está cumprido.**

Há leitores fora do diff com respostas próprias:

| Leitor | Derivação sobrevivente |
|---|---|
| `raiox_export` | Seleciona os retidos pelo export, mas reconstrói os dias com `furos_por_dia`, recortando diretamente por `data_demissao`. (`app/folha/views.py:591`; `app/folha/views.py:612`; `app/folha/views.py:616`) |
| Preview do export | Apresenta como quantidade de dias o comprimento de `motivos_espelho`, que contém motivos, não datas. (`app/folha/views.py:398`; `app/folha/views.py:402`) |
| Lista de fechamento | Traduz qualquer motivo não previsto no mapa para `furo_espelho`; o motivo original fica separado em `motivo_juiz`. (`app/ponto/services/fechamento_lista.py:25`; `app/ponto/services/fechamento_lista.py:36`) |
| API de mensageria | Chama de `retidos` todos os itens `fora`, incluindo saída pelo módulo de rescisão. Esse número não é “dias desligados retidos”. (`app/api/views_mensageria.py:283`; `app/api/views_mensageria.py:301`; `app/folha/export.py:867`) |

O Raio-X depende previamente do balde `furo_espelho` do export; portanto, sua derivação paralela não prova, sozinha, que o desligado voltou a reter. Prova que o censo não pode ser declarado limpo. (`app/folha/views.py:589`; `app/folha/views.py:605`)

**4. BLOQUEANTE — INSS pode apagar o estado usado para reconhecer o desligado.**

A lista da tela oferece apenas ativos/afastados, mas o POST busca o colaborador somente por `pk`. Após lançar INSS, grava `situacao='afastado'`, sem preservar `desligado`. A escrita passa pela porta legítima: é outro escritor semântico do campo, não escritor fora da porta. (`app/ponto/views.py:2372`; `app/ponto/views.py:2406`; `app/ponto/views.py:2433`; `app/colaboradores/services/colaborador.py:32`; `app/colaboradores/services/colaborador.py:55`)

Depois dessa troca, `esta_desligado` responde falso, o retentor volta ao laço e a C5 deixa de excluir a pessoa. Contraexemplo estático: desligado com saída depois da competência, furos anteriores e INSS posterior aos furos; o lançamento pode devolver os furos à retenção. Execução desse cenário em banco/produção: `[nao verificado]`. (`app/colaboradores/services/desligamento.py:24`; `app/folha/export.py:1207`; `app/escala/services/cadastro_realidade.py:169`)

O controle novo de afastado apenas cria outra pessoa com `situacao='afastado'`; não cobre essa transição nem uma ausência INSS real. (`app/folha/tests/test_lei_desligado_nao_trava.py:50`; `app/folha/tests/test_lei_desligado_nao_trava.py:79`)

**5. BLOQUEANTE para declarar PRONTO — faltam as provas nominais e de produção.**

O teste usa três dias da competência 10 e declara que col718/col270/col373 serão medidos por sonda no pouso. Não reproduz os RED nominais de 8/3/1 dias da competência 11. (`app/folha/tests/test_lei_desligado_nao_trava.py:8`; `app/folha/tests/test_lei_desligado_nao_trava.py:26`)

RED/GREEN nominal, suíte completa, opus, pouso e medição “comp11: desligados retidos = 0” não constam do dado fornecido. `[nao verificado]` [nao verificado]

Os recortes executados em memória passaram: C5 **9 testes** e matriz **20 testes**; não foi salvo registro de execução. Os módulos carregam código real por AST e declaram execução sem banco/escrita. (`app/chamados/tests/test_e4a2_c5_memoria.py:1`; `app/chamados/tests/test_e4a2_matriz_memoria.py:1`; registro persistido: `[nao verificado]`)

**6. NAO-BLOQ — o mutante do retentor tem efeito, mas o FAIL exigido não foi demonstrado.**

O teste chamado “mutante” força `esta_desligado=False` e **espera que os dias voltem**. Ele passa com esse comportamento; não reaplica a asserção original esperando reprovação. A asserção normal de vazio mataria essa mutação, mas sua execução sobre o mutante permanece `[nao verificado]`. (`app/folha/tests/test_lei_desligado_nao_trava.py:135`; `app/folha/tests/test_lei_desligado_nao_trava.py:67`)

Já o harness C5 realmente executa a verificação original, modifica a fonte em memória e exige `AssertionError`. (`app/chamados/tests/test_e4a2_c5_memoria.py:211`)

O selo AST do retentor exige apenas encontrar uma chamada chamada `esta_desligado`; aceita chamada sem influência no retorno, inclusive em ramo morto. O selo C5 possui tratamento explícito para esse caso. (`app/folha/tests/test_lei_desligado_nao_trava.py:157`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:285`)

**7. NAO-BLOQ — relógio interno e lavra antiga continuam exigindo atenção.**

O laço extraído lê `localdate()` quando não recebe `hoje`; `_dias_que_retem` não passa esse argumento. A C5 também mantém relógio interno opcional. Logo, não há leitura exclusivamente na borda. (`app/folha/export.py:1238`; `app/folha/fora_da_folha.py:98`; `app/escala/services/cadastro_realidade.py:161`)

`julgar_desligamento`, descrito como puro, lê `date.today()` duas vezes. É problema preexistente; ocorrência de divergência temporal em produção: `[nao verificado]`. (`app/colaboradores/services/desligamento.py:3`; `app/colaboradores/services/desligamento.py:32`; `app/colaboradores/services/desligamento.py:52`)

Retratos antigos continuam devolvendo o JSON gravado, sem reaplicar a cura. O indicador de frescor compara com atualização de `FechamentoMensal`, sem considerar mudança de código ou situação. Relavratura no pouso: `[nao verificado]`. (`app/folha/retrato_folha.py:110`; `app/folha/retrato_folha.py:1298`)

**8. NAO-BLOQ — contraexemplos e limites do predicado.**

| Caso | Resultado sustentado pelo código |
|---|---|
| Desligado reativado | Reativação volta a ativo e apaga demissão. Furos históricos podem voltar ao retentor/C5. Isso decorre da unidade por situação atual; não há base para exigir isenção histórica após reativação. (`app/colaboradores/services/desligamento.py:227`; `app/folha/export.py:1207`; `app/escala/services/cadastro_realidade.py:166`) |
| Desligado com INSS já existente | Enquanto conservar `situacao='desligado'`, retorna vazio antes de examinar células/ausências. O lançamento posterior de INSS tem o risco do achado 4. (`app/folha/export.py:1207`; `app/ponto/views.py:2435`) |
| Dois vínculos/empresas | O estado é da pessoa; o juiz não recebe vínculo/empresa. O cadastro tem uma empresa, e a demissão fecha os vínculos ativos elegíveis. Caso produtivo e regra de desligamento parcial: `[nao verificado]`. (`app/colaboradores/models.py:321`; `app/colaboradores/services/desligamento.py:24`; `app/colaboradores/services/vinculo.py:863`) |
| Desligado sem data | O retentor retorna vazio; a C5 possui teste desse cadastro. Entretanto, o universo de fechamento exclui desligado sem data, podendo não produzir linha para o DP. (`app/folha/export.py:1207`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:126`; `app/ponto/services/fechamento.py:432`) |
| Desligamento agendado | Grava data/motivo, conserva situação e retorna. Portanto, ainda pode reter como ativo; isso está de acordo com a unidade literal por situação. (`app/colaboradores/services/desligamento.py:99`; `app/colaboradores/services/desligamento.py:103`; `app/colaboradores/services/desligamento.py:118`) |
| Dias após a saída | Com situação desligado, o retentor retorna vazio antes do laço. A trava agregada `turno_aberto` do achado 2 permanece; não oferece datas para determinar se veio de dia posterior à saída. (`app/folha/export.py:1207`; `app/folha/export.py:833`) |

Não encontrei juiz **registrado** concorrente para “está desligado?”. `afastado_hoje` responde outra pergunta; `sai_pela_rescisao` decide destino da competência por data. Há comparação crua duplicada no formatador de situação exibida, mas isso não comprova uma segunda autoridade registrada. (`app/core/juizes.py:263`; `app/core/juizes.py:267`; `app/folha/export.py:60`; `app/colaboradores/services/situacao_exibida.py:56`)

**9. NAO-BLOQ — mapa de escritores e leitores relevantes aos campos do diff.**

| Campo/valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `situacao` | Demissão e reativação; INSS; reversor de afastamento, pela porta. (`app/colaboradores/services/desligamento.py:121`; `:227`; `app/ponto/views.py:2435`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`) | Juiz novo; badge; universo de fechamento; guarda de vínculo. (`app/colaboradores/services/desligamento.py:24`; `app/colaboradores/services/situacao_exibida.py:56`; `app/ponto/services/fechamento.py:440`; `app/colaboradores/services/vinculo.py:91`) |
| Data de desligamento | O campo real é `data_demissao`; agendamento/demissão escrevem, reativação limpa. (`app/colaboradores/models.py:325`; `app/colaboradores/services/desligamento.py:103`; `:122`; `:228`) | Cron; destino rescisão; teto do laço; universo; Raio-X. (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; `app/folha/export.py:60`; `:1241`; `app/ponto/services/fechamento.py:432`; `app/folha/views.py:612`) |
| `o_que_segura` | Produz retorno em memória, incluindo o informativo. (`app/folha/fora_da_folha.py:235`) | Teste novo; o drawer produtivo lê o Retrato, não essa função. (`app/folha/tests/test_lei_desligado_nao_trava.py:100`; `app/colaboradores/views.py:984`) |
| `informativo_rescisao` | Produzido pelo laço compartilhado; transformado em chave de retorno. (`app/folha/export.py:1228`; `app/folha/fora_da_folha.py:233`) | `o_que_segura` e testes; integração ao DP faltante no achado 1. (`app/folha/fora_da_folha.py:234`; `app/folha/tests/test_lei_desligado_nao_trava.py:119`) |
| `retidos` | Há métricas diferentes: lista do gerador por `furo_espelho`; API por qualquer `fora`. (`app/folha/views.py:259`; `app/api/views_mensageria.py:301`) | Mensagem do gerador, trilha e resposta da API; não equivalem ao contador solicitado. (`app/folha/views.py:293`; `app/folha/views.py:304`; `app/api/views_mensageria.py:295`) |
| C5 `colabs_nunca_bateu` | Lê células e exclui pelo juiz; retorna mapa pessoa→datas. (`app/escala/services/cadastro_realidade.py:166`; `:169`; `:172`) | Formatador C5, contador do disparar e vigia de retido calado. (`app/escala/services/cadastro_realidade.py:179`; `app/chamados/management/commands/disparar_perguntas_competencia.py:56`; `app/folha/vigia_retido_calado.py:43`) |
| `barrados_desligado` | Conta pares únicos excluídos antes do filtro temporal. (`app/chamados/management/commands/disparar_perguntas_competencia.py:61`; `:72`) | Viaja no bloco informativo do carimbo; não entra como contador próprio em `censo/esperado`. (`app/chamados/management/commands/vigiar_fio_morto.py:233`; `:239`; `:242`) |

**10. NAO-BLOQ — balanço literal do aval, com as pendências acima.**

| Item | Cumprimento |
|---|---|
| Lei/unidade no retentor | **Cumprido no caminho central:** situação desligado retorna vazio, inclusive com `com_dias=True`. (`app/colaboradores/services/desligamento.py:24`; `app/folha/export.py:1207`) |
| `_dias_que_retem` e ramo do export iguais | **Cumprido por delegação:** ambos chegam a `motivos_retencao_celula`; o desligado sai antes de `_retem`. (`app/folha/fora_da_folha.py:98`; `app/folha/export.py:1307`; `app/folha/export.py:1207`) |
| Censo de leitores | **Incompleto**, pelas derivações do achado 3. (`app/folha/views.py:616`; `app/ponto/services/fechamento_lista.py:36`) |
| RED col718/270/373 | **Não demonstrado.** Fixture substituto comp10; sonda nominal `[nao verificado]`. (`app/folha/tests/test_lei_desligado_nao_trava.py:8`; `:26`) |
| Ativo com mesmo furo | **Há teste de controle.** Execução ORM `[nao verificado]`. (`app/folha/tests/test_lei_desligado_nao_trava.py:79`) |
| Afastado intocado | **Regra central preservada**, mas cobertura INSS é parcial e há o contraexemplo do achado 4. (`app/folha/export.py:1207`; `app/folha/tests/test_lei_desligado_nao_trava.py:50`; `app/ponto/views.py:2435`) |
| Contador no ESTADO | **Não cumprido: não existe `desligados_retidos` nesta cópia.** A métrica publicada é outra. (`app/chamados/management/commands/vigiar_fio_morto.py:233`; `bin/gerar_estado.py:260`) |
| Mutante → FAIL | **Parcial:** comportamento mutado demonstrado no teste; execução do FAIL do retentor `[nao verificado]`. (`app/folha/tests/test_lei_desligado_nao_trava.py:135`) |
| PROIBIDOS | Os novos caminhos leem células e retornam dados; não lançam falta. O selo de imutabilidade compara somente `CelulaDia`, sem selar Batida/Ausência. (`app/folha/export.py:1247`; `app/folha/export.py:1298`; `app/folha/tests/test_lei_desligado_nao_trava.py:128`) |
| Suíte + opus + pouso + comp11=0 | **Não demonstrado pelo material fornecido.** `[nao verificado]` | [nao verificado]

## 13/11 13:26 · sessao 21c2dc54bcdf · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — INSS pode apagar o estado que sustenta a isenção.** A tela só oferece ativos/afastados, mas o POST busca qualquer colaborador pelo `pk` e, se `tipo == 'afastamento_inss'`, grava `situacao='afastado'` sem preservar `desligado`: `app/ponto/views.py:2372`, `:2406`, `:2433`. A porta aceita essa escrita (`app/colaboradores/services/colaborador.py:32`, `:55`); portanto é **outro escritor semântico pela porta**, não escritor fora dela. Depois disso, `esta_desligado` responde falso (`app/colaboradores/services/desligamento.py:24`), o retentor volta ao laço (`app/folha/export.py:1207`) e a C5 volta a incluir os dias `nunca_bateu` (`app/escala/services/cadastro_realidade.py:169`). Cenário estático concreto: desligado com saída depois do fim da competência, furos antigos antes do INSS; lançar INSS posterior aos furos altera o estado e devolve os furos à retenção. Execução/banco/prod desse cenário: `[nao verificado]`. O teste só usa outro colaborador já marcado afastado, não a transição desligado→INSS (`app/folha/tests/test_lei_desligado_nao_trava.py:48`, `:79`).

2. **NAO-BLOQ para o novo predicado; problema temporal preexistente — relógio fora da borda.** `julgar_desligamento`, apresentado como puro, lê `date.today()` duas vezes (`app/colaboradores/services/desligamento.py:3`, `:32`, `:52`). A view só lê `timezone.localdate()` depois, ao chamar o executor (`app/colaboradores/views.py:1434`, `:1437`); o cron lê `hoje` local e não o passa ao juiz (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `:51`). Logo, a mesma operação pode usar datas distintas na borda e no julgamento; ocorrência real `[nao verificado]`. O laço extraído ainda lê relógio internamente (`app/folha/export.py:1238`) e a C5 também (`app/escala/services/cadastro_realidade.py:161`).

3. **NAO-BLOQ — não encontrei prova de juiz registrado duplicado.** O registro declara perguntas distintas para afastamento e desligamento (`app/core/juizes.py:263`, `:267`). `situacao_exibida.palavra` já repete a comparação crua para a precedência do badge (`app/colaboradores/services/situacao_exibida.py:56`), mas não achei ali uma autoridade registrada concorrente para a pergunta “desligado?”. `sai_pela_rescisao` pergunta sobre destino da competência e usa data, não situação (`app/folha/export.py:54`, `:60`); não equivale automaticamente ao novo juiz.

4. **NAO-BLOQ, contraexemplo com efeito histórico — reativação.** Reativar volta a `ativo`, apaga `data_demissao` e motivo e salva pela porta (`app/colaboradores/services/desligamento.py:220`, `:227`, `:231`). Não reativa escala automaticamente (`:223`). A partir daí, os furos históricos continuam na C5 (`app/escala/services/cadastro_realidade.py:166`) e podem voltar ao retentor (`app/folha/export.py:1207`); não há recorte histórico de períodos desligados nesse predicado (`app/colaboradores/services/desligamento.py:24`). A unidade literal “colaborador com situação desligado” pode autorizar esse efeito; chamar isso de violação da lei exigiria esclarecimento `[nao verificado]`. Os novos testes não cobrem a sequência de reativação `[nao verificado]`.

5. **NAO-BLOQ — aviso agendado e dia da saída.** Agendar grava data/motivo e retorna sem mudar situação (`app/colaboradores/services/desligamento.py:99`, `:102`, `:118`); a efetivação grava `desligado` (`:121`). O cron está configurado para 06:06 (`app/config/crons.py:440`). Antes da efetivação, inclusive no dia com data de saída vencida, o novo juiz ainda responde falso se a situação continua ativa (`app/colaboradores/services/desligamento.py:24`; seleção do cron em `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`). Isso segue a escolha explícita por estado atual, enquanto rescisão e teto do laço seguem data (`app/folha/export.py:60`, `:1241`). Estado real do cron/produção `[nao verificado]`.

6. **NAO-BLOQ — sem data.** O campo real é `data_demissao`; `situacao` e a data são campos independentes (`app/colaboradores/models.py:323`, `:325`). `esta_desligado` não exige data (`app/colaboradores/services/desligamento.py:24`), então o retentor devolve vazio para desligado sem data (`app/folha/export.py:1207`). Entretanto, o universo de fechamento exclui desligado sem data (`app/ponto/services/fechamento.py:432`, `:439`), podendo impedir que ele tenha linha nos leitores de fechamento. O novo teste C5 cobre falta de data (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:126`); cobertura equivalente de exportação/DP para cadastro sem fechamento `[nao verificado]`.

7. **NAO-BLOQ — dois vínculos/empresas não têm isolamento no estado.** Há um `Colaborador.empresa`, um estado e CPF único (`app/colaboradores/models.py:319`, `:321`, `:323`); vínculos apontam para colaborador e posto (`app/escala/models.py:872`, `:873`). A demissão fecha todos os vínculos ativos do colaborador (`app/colaboradores/services/vinculo.py:855`, `:863`) e o predicado isenta a pessoa inteira (`app/colaboradores/services/desligamento.py:24`), sem empresa ou vínculo como argumento. C5 agrupa pelo cadastro atual da empresa (`app/escala/services/cadastro_realidade.py:166`). Caso real de vínculo simultâneo noutra empresa e regra esperada para ele `[nao verificado]`.

Mapa de escrita/leitura relevante:

- **`situacao`**: escreve demissão/reativação (`app/colaboradores/services/desligamento.py:121`, `:227`), INSS (`app/ponto/views.py:2435`) e reversor de afastamento (`app/ponto/management/commands/reverter_situacao_afastado.py:73`), todos pela porta. Lê novo juiz (`app/colaboradores/services/desligamento.py:24`), badge (`app/colaboradores/services/situacao_exibida.py:56`), universo fechamento (`app/ponto/services/fechamento.py:439`), guarda de vínculo (`app/colaboradores/services/vinculo.py:91`), guarda de reativação ainda crua (`app/colaboradores/views.py:1417`). Admin é `SoLeitura` (`app/colaboradores/admin.py:48`).
- **`data_demissao`**: escreve agendamento/demissão/reativação (`app/colaboradores/services/desligamento.py:103`, `:122`, `:228`). Lê cron de efetivação (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`), destino rescisão (`app/folha/export.py:60`), teto do laço (`:1241`), universo fechamento (`app/ponto/services/fechamento.py:432`), corte na tela folha (`app/folha/views.py:612`), cadastro do dia (`app/ponto/supra_juiz.py:154`, `:194`).
- **C5**: produz `colabs_nunca_bateu` com juiz (`app/escala/services/cadastro_realidade.py:169`); formata `_nunca_bateu` (`:176`); lê contador (`app/chamados/management/commands/disparar_perguntas_competencia.py:56`).
- **`barrados_desligado`**: produzido como quantidade de pares únicos excluídos (`app/chamados/management/commands/disparar_perguntas_competencia.py:58`, `:62`, `:72`); é contador de exclusão, não prova de desligados ainda retidos (`:61`).

## 13/11 13:26 · sessao dbab1381c92f · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — não existe o contador `desligados_retidos` pedido no aval.** A busca literal em toda a cópia não encontrou definição nem leitor; o ESTADO conecta `fio_estado`, e o carimbo publica contadores de fio/C5, sem esse contador. `bin/gerar_estado.py:260`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `app/chamados/management/commands/vigiar_fio_morto.py:239`. Ausência literal no conjunto da cópia: `[nao verificado]` por arquivo:linha.

2. **BLOQUEANTE para declarar PRONTO — golden e medição de produção ausentes.** O teste diz que col718/col270/col373 serão medidos por sonda no pouso; seu fixture é genérico, com três dias da competência 10, não os 8/3/1 dias da competência 11. `app/folha/tests/test_lei_desligado_nao_trava.py:8`; `app/folha/tests/test_lei_desligado_nao_trava.py:26`. Saídas RED/GREEN nominais, suite completa, opus, pouso e comp11=0: `[nao verificado]`.

3. **NAO-BLOQ — `barrados_desligado` não substitui `desligados_retidos`.** Conta pares desligados excluídos do comparador C5, antes da filtragem pelo piso temporal; não consulta retenção da folha. `app/chamados/management/commands/disparar_perguntas_competencia.py:60`; `app/chamados/management/commands/disparar_perguntas_competencia.py:72`. Viaja no bloco informativo inteiro `nunca_bateu_sem_adesao` do carimbo, enquanto `censo/esperado` levam apenas `numero/esperado` do comparador. `app/chamados/management/commands/vigiar_fio_morto.py:233`; `app/chamados/management/commands/vigiar_fio_morto.py:242`. O ESTADO compara chaves do `esperado` e mostra divergências, sem ler `barrados_desligado` como contador próprio. `bin/fio_estado.py:121`; `bin/fio_estado.py:191`.

4. **NAO-BLOQ — mutante do retentor é testemunha positiva, não execução do FAIL exigido.** O teste denominado mutante força `esta_desligado=False` e afirma que os dias voltaram; não executa a asserção original esperando reprovação. `app/folha/tests/test_lei_desligado_nao_trava.py:135`. A asserção normal de vazio mataria esse mutante se executada sobre ele; portanto não há base para afirmar que o mutante é semanticamente inofensivo. `app/folha/tests/test_lei_desligado_nao_trava.py:67`. Execução ORM dessa mutação: `[nao verificado]`.

5. **NAO-BLOQ — cobertura INSS/PROIBIDOS é parcial.** O controle afastado apenas cria `situacao='afastado'`, sem afastamento INSS materializado. `app/folha/tests/test_lei_desligado_nao_trava.py:50`; `app/folha/tests/test_lei_desligado_nao_trava.py:79`. O teste “nada escrito” compara apenas `CelulaDia`; não sela Batida, Ausencia ou lançamento de falta. `app/folha/tests/test_lei_desligado_nao_trava.py:128`.

6. **NAO-BLOQ — selo AST do retentor aceita chamada em ramo morto.** Examina todos os nós e exige apenas o nome `esta_desligado`, sem verificar ramo morto ou influência no resultado. `app/folha/tests/test_lei_desligado_nao_trava.py:157`. O selo C5 possui tratamento/testemunha explícitos para ramo morto. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:285`.

Verificação executada sem banco/rede/escrita: C5 memória **9 OK** e matriz memória **20 OK**, incluindo seus catálogos de mutantes; registros de execução não salvos: `[nao verificado]`. Os módulos declaram execução em memória e carregamento das funções reais por AST. `app/chamados/tests/test_e4a2_c5_memoria.py:1`; `app/chamados/tests/test_e4a2_matriz_memoria.py:1`. O harness C5 realmente reaplica a asserção e exige `AssertionError` após mutação. `app/chamados/tests/test_e4a2_c5_memoria.py:211`.

## 13/11 13:26 · sessao 9647e3caf3e2 · - · - · origem=Ronald (tmux)

1. **BLOQUEANTE — ainda existe trava de aprovação para desligado.** `classificar_export` cria `turno_aberto` sem consultar `esta_desligado` e grava a resposta em `travas_aprovacao` (`app/folha/export.py:830-848`). O lote exclui o colaborador da aprovação (`app/ponto/views.py:1295-1312`), e a aprovação individual recusa explicitamente quem tem essa trava (`app/ponto/views.py:1505-1513`). A tela também usa essas travas para habilitar o botão (`app/ponto/views.py:1168-1171`). O carimbo automático do TXT mantém esse crivo (`app/folha/views.py:244-248`). **Precisão:** isso não impede necessariamente a linha no TXT: `montar_linhas_export` aceita fechamentos abertos e aprovados e seleciona pelo `status='entra'` (`app/folha/export.py:687`, `app/folha/export.py:714-726`).

2. **BLOQUEANTE — informativo não integrado à apresentação do DP.** A lista nova só é acrescentada ao retorno da ferramenta `o_que_segura` (`app/folha/fora_da_folha.py:232-238`). O escritor do Retrato continua gravando somente campos antigos, sem `informativo_rescisao` (`app/folha/retrato_folha.py:1177-1189`); a tela lê as linhas gravadas (`app/folha/views_retrato.py:190-196`). O drawer lê o mesmo Retrato e seus campos antigos (`app/colaboradores/views.py:977-983`). Acesso manual pela ferramenta existe; entrega efetiva do informativo ao DP por tela/PDF é **[nao verificado]**.

3. **BLOQUEANTE para o requisito literal de censo/juiz único — sobrevivem derivações paralelas.**
   - O painel de retenção pergunta quem está fora ao export (`app/folha/views.py:586-591`), mas reconstrói os dias via `furos_por_dia`, cortando pela `data_demissao` crua (`app/folha/views.py:605-619`), em vez de ler `_dias_que_retem`.
   - O preview apresenta como dias o tamanho da lista de motivos (`app/folha/views.py:398-402`).
   - Esses caminhos não recolocam por si o desligado no balde `furo_espelho`, pois dependem previamente do veredito do export (`app/folha/views.py:589-591`); entretanto não satisfazem “nenhum leitor deriva por conta própria”.

4. **NAO-BLOQ — leitores alinhados ao novo retentor.**
   - `_dias_que_retem` delega diretamente a `motivos_retencao_celula(com_dias=True)` (`app/folha/fora_da_folha.py:95-99`).
   - `dias_que_seguram` usa `classificar_export` e só busca dias dos itens `furo_espelho` (`app/folha/fora_da_folha.py:118-137`).
   - `fora_da_folha` conta os dias recebidos dessa autoridade (`app/folha/fora_da_folha.py:160-193`).
   - Prévia: `_cobre_a_retencao` consulta o retentor e usa suas datas (`app/folha/previa.py:169-173`).
   - Vigia de pausa: consulta o mesmo retentor (`app/folha/services/vigia_pausa.py:123-130`).
   - Vigia de fio: lê `dias_que_seguram` (`app/chamados/services/vigia_fio.py:583-587`).
   - Retidos do gerador: deriva a lista do resultado `status/motivo` do export (`app/folha/views.py:257-260`).
   - PDF de retidos: chama o mesmo `classificar_export` (`app/folha/views.py:858-859`).
   - Retrato novo: `montar_linhas` lê `fora_da_folha` e grava os campos resultantes (`app/folha/retrato_folha.py:1156-1189`); único escritor persistente nessa porta é `lavrar_retrato_folha --apply` (`app/folha/management/commands/lavrar_retrato_folha.py:65-76`).

5. **NAO-BLOQ — lavra antiga continua sendo lida sem reaplicar a cura.** O leitor de Retrato devolve as linhas JSON gravadas, sem consultar o estado atual do colaborador ou o novo retentor (`app/folha/retrato_folha.py:98-111`). A tela usa esse leitor (`app/folha/views_retrato.py:190-196`); o vigia de retidos calados também (`app/folha/vigia_retido_calado.py:45-55`). Portanto o pouso precisa incluir relavratura; ocorrência/execução dessa relavratura em produção é **[nao verificado]**. O indicador de frescor compara apenas o horário do Retrato com `FechamentoMensal.atualizado_em`, não com mudança de código ou de situação (`app/folha/retrato_folha.py:1295-1306`).

6. **BLOQUEANTE — contador `desligados_retidos` no ESTADO ausente.** A busca literal em toda a cópia não encontrou o identificador **[nao verificado por referência positiva: ausência]**. O gerador tem integração explícita para outros medidores, como fio morto (`bin/gerar_estado.py:253-261`), leitores do dia (`bin/gerar_estado.py:284-293`) e turno do par (`bin/gerar_estado.py:294-302`); não há medidor/integração equivalente para desligados no código pesquisado **[nao verificado por referência positiva: ausência]**.

## 13/11 12:59 · sessao c2c9fc70ece1 · HJ-090 · censo · origem=Code

**(A) Não há arquivo em comum nas listas fornecidas. Nenhuma das três listas contém migration de `ponto`.** Para citar os anexos colados, conto o primeiro caminho depois de cada marcador como linha 1.

| Obra | Interseção com os arquivos declarados da HJ-091 | Migration de `ponto` listada | Fonte fornecida |
|---|---|---|---|
| DESLIGADO | Vazia | Não | `arquivos_wt-desligado-112447.txt:1–14` |
| FERIADO | Vazia | Não | `arquivos_wt-feriado1609-110424.txt:1–2` |
| E3 | Vazia | Não | `arquivos_e3.txt:1–73` |

O painel efetivamente usado é `app/templates/ponto/partials/painel_ausencia.html`, renderizado por `painel_ausencia`. Seus includes são `_cabecalho_drawer.html` e `_btn_pdf.html`; este inclui `_icone_pdf.html`. Nenhum desses caminhos aparece nas três listas fornecidas. (`app/ponto/views.py:2079`; `app/templates/ponto/partials/painel_ausencia.html:24`, `:206`, `:244`; `app/templates/core/_btn_pdf.html:37`; anexos acima.)

`app/folha/export.py` aparece em DESLIGADO, mas o complemento declara expressamente que esse arquivo **não muda** na HJ-091. (`arquivos_wt-desligado-112447.txt:10`; `complemento_hj_091.txt:2`, numeração do texto fornecido.)

Assim, os anexos não demonstram conflito de arquivo ou de número de migration. O número da migration futura e mudanças omitidas das listas são **[nao verificado]**. Existe na cópia a migration `0077`, dependente de `0076`; isso não demonstra reserva do próximo número. (`app/ponto/migrations/0077_ausencia_minutos_help_text_grade.py:8`.)

**(B) Escritores de `Ausencia` que alcançam férias.**

“Derivada” abaixo designa a linha criada por `espelhar_ferias`, que grava `Ferias agendamento #<pk>` na descrição. O espelhador seleciona existentes por colaborador, tipo e janela, sem filtrar status ou conferir esse texto; qualquer dia encontrado é pulado. (`app/ponto/services/ausencia.py:1183`, `:1200`, `:1203`, `:1207`.)

`ABERTAS` significa `aguardando_documento` e `aguardando_decisao`. A máquina permite aprovação a partir de `aguardando_decisao`; `aguardando_documento` não tem transição direta para aprovada. (`app/ponto/services/ausencia.py:766`, `:768`.)

| Função | Escrita e alcance de férias derivada |
|---|---|
| `criar_ausencia` | Valida tipo pelo catálogo; cria e salva. Também encontra rejeitada do mesmo colaborador/tipo/data e chama `_reabrir`, sem conferir agendamento de origem. **Alcança derivada rejeitada por esse caminho.** (`app/ponto/services/ausencia.py:194`, `:254`, `:291`, `:385`, `:418`.) |
| `garantir_ausencia` | Delega ao criador com `se_existir='retornar'`; não acrescenta exclusão de férias. **Função genérica; seus chamadores de regularização/abandono fornecem `falta`.** (`app/ponto/services/ausencia.py:453`, `:461`; `app/ponto/services/regularizacao.py:118`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`.) |
| `editar_ausencia_registro` | Troca tipo, data, fim, dias, descrição não vazia e minutos; salva, invalida fechamento e rejulga estado quando muda o tipo. **Alcança derivada**, sem guarda de origem. (`app/ponto/services/ausencia.py:465`, `:474`, `:498`, `:509`, `:524`, `:528`.) |
| `rejulgar_status_pelo_tipo` | Só atua em estados abertos; calcula estado inicial, muda estado e eventualmente grava prazo. **Alcança férias abertas**; para férias, o estado inicial é `aguardando_decisao`, sem prazo documental. (`app/ponto/services/ausencia.py:534`, `:541`, `:546`, `:551`, `:794`.) |
| `mudar_estado` | Valida transição, grava status, limpa prazo quando cabível e salva os campos derivados. **Alcança derivada**, sem filtro de tipo/origem. (`app/ponto/services/ausencia.py:809`, `:818`, `:825`, `:836`.) |
| `decidir` | Aceita aprovada/rejeitada; chama `mudar_estado` e grava decisor, data, descrição e período quando fornecidos. **Alcança derivada.** (`app/ponto/services/ausencia.py:898`, `:905`, `:912`, `:914`, `:928`.) |
| `_validar_periodo` / `_aplicar_periodo` | Auxiliares de `decidir`: exigem ausência parcial e atribuem horas. **Não excluem férias por literal**, mas a escrita depende de `cobre_parte_do_dia`. (`app/ponto/services/ausencia.py:880`, `:888`, `:892`, `:909`, `:926`.) |
| `anexar_documento` | Grava documento; em `aguardando_documento`, muda para `aguardando_decisao`. **Alcança derivada**, sem filtro de tipo. (`app/ponto/services/ausencia.py:847`, `:858`, `:860`, `:874`.) |
| `reverter_rejeicao` | Exige rejeitada e motivo; delega a `_reabrir`. **Alcança derivada rejeitada.** (`app/ponto/services/ausencia.py:1072`, `:1098`, `:1101`, `:1103`.) |
| `_reabrir` | Pode gravar documento/descrição; muda ao estado inicial calculado pelo tipo/anexo e grava prazo quando necessário. **Alcança derivada**, sem conferir origem. (`app/ponto/services/ausencia.py:1107`, `:1120`, `:1131`, `:1141`, `:1142`, `:1149`.) |
| `escalonar_aguardando_documento` | Seleciona por estado e prazo, sem tipo; com `aplicar`, vencidas passam a `aguardando_decisao`. **Alcança férias que estejam nessa seleção.** (`app/ponto/services/ausencia.py:974`, `:993`, `:1002`.) |
| `espelhar_ferias` | Agendamento ativo: cria férias aprovadas nos dias sem linha existente. Inativo: apaga as férias selecionadas na janela. **Alcança derivadas e outras férias da mesma janela**, sem conferir origem. Não busca linhas deslocadas para fora dela. (`app/ponto/services/ausencia.py:1181`, `:1183`, `:1189`, `:1200`, `:1203`.) |
| `apagar_ausencias_de_ferias` | Apaga por colaborador, `tipo='ferias'` e data inicial dentro da janela; `exceto` preserva a janela indicada. **Alcança qualquer derivada selecionada**, sem origem. (`app/ponto/services/ausencia.py:1263`, `:1267`, `:1269`, `:1271`.) |
| `rejeitar_ausencias_de_ferias` | Faz `update` para rejeitada nas férias não rejeitadas da janela, com atualização do carimbo; não passa por `save`. **Alcança qualquer derivada selecionada.** (`app/ponto/services/ausencia.py:1274`, `:1282`.) |
| `apagar_ausencias_em_ensaio` | Exige banco de ensaio e apaga o queryset recebido. **Alcança derivada incluída nele.** (`app/ponto/services/ausencia.py:1287`, `:1289`, `:1291`.) |
| `rederivar_fim` | Salva novamente a ausência e recarrega. **Alcança derivada**, sem filtro próprio. (`app/ponto/services/ausencia.py:1294`, `:1297`.) |
| `Ausencia.save` | Carimba decisão; deriva dias pelo intervalo e fim pelos dias quando cabível. **Alcança derivada**, sem guarda de origem. (`app/ponto/models.py:868`, `:872`, `:894`, `:902`, `:908`.) |

O adaptador `_a4_decidir` das views delega a `decidir`; não acrescenta guarda para férias derivadas. (`app/ponto/views.py:82`, `:85`.)

**Portas HTTP de ausência.** As rotas de ponto recebem prefixo `/ponto/`; as da API, `/api/`. (`app/config/urls.py:29`, `:37`.)

| View / rota | Comportamento e alcance |
|---|---|
| `lancar_ausencia` — `ausencias/lancar/` | Tipo vem do POST; chama criador com `status='aprovada'`. **Cria férias sem agendamento e alcança reabertura pelo criador**; não seleciona derivada por ID. (`app/ponto/urls.py:57`; `app/ponto/views.py:2378`, `:2415`, `:2420`; `app/ponto/services/ausencia.py:254`, `:291`.) |
| `submeter_ausencia` — `ausencias/submeter/` | Valida catálogo/sobreposição e chama criador, sem status explícito nem agendamento. **Não exclui férias**; aplica os vetos e a reabertura do criador. (`app/ponto/urls.py:64`; `app/ponto/views.py:2509`, `:2525`, `:2536`.) |
| `editar_ausencia` — `ausencias/<aid>/editar/` | Busca só por PK e chama editor geral. **Edita derivada**, conservando férias ou trocando o tipo, sujeita às validações do editor. (`app/ponto/urls.py:76`; `app/ponto/views.py:2761`, `:2794`, `:2796`.) |
| `cancelar_ausencia` — `ausencias/<aid>/cancelar/` | Busca por PK; recusa apenas rejeitada; decide rejeitada e prefixa descrição. **Cancela derivada aprovada.** (`app/ponto/urls.py:75`; `app/ponto/views.py:2728`, `:2729`, `:2734`.) |
| `cancelar_ausencia_lote` — `ausencias/lote/cancelar/` | Para cada PK, exclui só rejeitada e decide rejeitada. **Cancela derivada aprovada.** (`app/ponto/urls.py:56`; `app/ponto/views.py:2345`, `:2351`.) |
| `rejeitar_ausencia` — `ausencias/<aid>/rejeitar/` | Seleciona ausência e raiz/irmãos documentais em estados abertos; rejeita cada um. **Alcança derivada aberta.** (`app/ponto/urls.py:74`; `app/ponto/views.py:2687`, `:2689`, `:2701`.) |
| `rejeitar_ausencia_lote` — `ausencias/lote/rejeitar/` | PK + estados abertos; rejeita e pode acrescentar motivo à descrição. **Alcança derivada aberta.** (`app/ponto/urls.py:55`; `app/ponto/views.py:2316`, `:2319`.) |
| `reverter_rejeicao_ausencia` — `ausencias/<aid>/reverter-rejeicao/` | Busca só por PK e chama reversor. **Reabre derivada rejeitada**, exigindo motivo. (`app/ponto/urls.py:68`; `app/ponto/views.py:2583`, `:2584`; `app/ponto/services/ausencia.py:1098`, `:1101`.) |
| `aprovar_ausencia` — `ausencias/<aid>/aprovar/` | Seleciona aberta e raiz/irmãos abertos; passa pelo gate e decide aprovação. **Não exclui derivada**; gate e máquina de estados limitam a aprovação. (`app/ponto/urls.py:65`; `app/ponto/views.py:2606`, `:2608`, `:2616`, `:2643`.) |
| `aprovar_ausencia_lote` — `ausencias/lote/aprovar/` | PK + estados abertos, gate e decisão. **Não exclui derivada**, com as mesmas limitações de aprovação. (`app/ponto/urls.py:54`; `app/ponto/views.py:2282`, `:2284`, `:2290`.) |
| `editar_periodo_ausencia` — `ausencias/<aid>/periodo/` | Busca aprovada por PK e envia horas ao decisor. **Sem exclusão literal de férias**; exige tipo parcial no catálogo vigente, dia único e minutos entre zero e o dia inteiro. O catálogo vigente de produção é **[nao verificado]**. (`app/ponto/urls.py:66`; `app/ponto/views.py:2670`, `:2673`; `app/ponto/services/ausencia.py:888`; `app/ponto/turnos.py:2600`.) |
| `anexar_documento_ausencia` — `ausencias/<aid>/anexar-documento/` | Busca por PK sem filtro de tipo/status e chama anexador. **Alcança derivada.** (`app/ponto/urls.py:71`; `app/ponto/views.py:2140`, `:2151`.) |
| `api_ausencias` — `ausencias/` | POST fornece tipo; criador recebe `devolver_mesmo_fato`. Reenvio pode anexar documento na existente. **Cria férias, reabre rejeitada e anexa na derivada existente**, conforme esses caminhos. (`app/api/urls.py:82`; `app/api/views.py:1705`, `:1805`, `:1811`, `:1833`, `:1838`; `app/ponto/services/ausencia.py:254`, `:291`.) |
| `api_ausencia_documento` — `ausencias/<aid>/documento/` | Busca PK + colaborador, exige estado aberto e chama anexador. **Alcança derivada aberta do próprio colaborador.** (`app/api/urls.py:83`; `app/api/views.py:3069`, `:3078`, `:3108`.) |
| `veredito_celula_view` — `celula/veredito/` | Encaminha `tipo_ausencia` recebido ao serviço `veredito_celula`; no ramo justificado ele cria ausência aprovada sem veto próprio a férias. **Cria férias sem agendamento; a reabertura segue o criador.** (`app/ponto/urls.py:58`; `app/ponto/views.py:3312`, `:3340`; `app/ponto/services/veredito_celula.py:211`, `:333`, `:337`.) |

O veto a férias de `AusenciaForm.clean_tipo` não protege lançar/submeter/editar: essas views chamam diretamente os serviços, que validam pelo catálogo. (`app/ponto/forms.py:65`; `app/ponto/views.py:2415`, `:2536`, `:2794`; `app/ponto/services/ausencia.py:194`, `:474`.)

Há ainda o escritor genérico `lancar_ausencia_porta`, chamado por `_aplicar_ausencia` através de `ia_perguntar → _aplicar_proposta`, em `/ia/perguntar/`. O escritor recebe tipo e cria aprovada; **o fluxo normal de prévia recusa férias antes de guardar a proposta**. (`app/ponto/services/checar_ausencia.py:43`, `:46`, `:10`, `:21`; `app/core/views_ia.py:931`, `:935`, `:985`, `:153`, `:874`; `app/core/urls.py:15`.)

**Escrita pelo agendamento e respectivas portas.**

`AgendamentoFerias.save → _sync_ausencia → espelhar_ferias` executa em save completo ou alteração de status/datas/período aquisitivo. `AgendamentoFerias.delete` chama o apagador de férias da janela inteira, sem conferir `#N`. **Ambos alcançam as derivadas selecionadas.** (`app/ferias/models.py:386`, `:392`, `:444`, `:446`, `:450`, `:453`, `:455`.)

As rotas desta tabela têm prefixo `/ferias/`. (`app/config/urls.py:32`.)

| Escritor | Efeito nas ausências | View/porta que chega nele |
|---|---|---|
| `registrar_gozo_historico` | Cria agendamento e salva, disparando espelho. (`app/ferias/services.py:270`, `:278`, `:298`.) | `criar_periodo_aquisitivo → registrar_gozo_historico`. (`app/ferias/views.py:124`, `:211`; `app/ferias/urls.py:15`.) |
| `executar_agendamento` | Cria agendamento, disparando espelho. (`app/ferias/services.py:306`, `:321`.) | `agendar_ferias` e `agendar_lote`, pelo wrapper `_executar_agendamento`. (`app/ferias/views.py:299`, `:636`, `:260`, `:264`; `app/ferias/urls.py:16`, `:10`.) |
| `agendar_ferias_porta` | Delega a `executar_agendamento`. (`app/ferias/services.py:128`, `:157`.) | `ia_perguntar → _aplicar_proposta → _aplicar_ferias → escritor registrado`. (`app/core/views_ia.py:874`, `:145`, `:668`, `:684`; `app/core/intake.py:34`; `app/core/urls.py:15`.) |
| `cancelar_agendamento_dp` | Salva cancelado; o espelho apaga férias da janela. (`app/ferias/services.py:372`, `:384`, `:385`; `app/ponto/services/ausencia.py:1189`.) | `cancelar_agendamento`. (`app/ferias/views.py:322`, `:330`; `app/ferias/urls.py:18`.) |
| `executar_cancelamento_admin` | Primeiro rejeita férias da janela; depois salva cancelado e o espelho as apaga. (`app/ferias/services.py:390`, `:411`, `:417`.) | `cancelar_agendamento_admin`, `corrigir_concluida_admin` e `cancelar_admin_massa`, pelo wrapper `_executar_cancelamento_admin`; correção de concluída passa `permitir_concluido=True`. (`app/ferias/views.py:370`, `:418`, `:575`, `:527`, `:531`; `app/ferias/urls.py:19`, `:20`, `:9`.) |
| `_trocar_janela` | Apaga trecho antigo fora da janela nova; salva datas/status e espelha a nova janela. (`app/ferias/services.py:422`, `:445`, `:450`.) | `editar_datas_admin → corrigir_datas_agendamento → _trocar_janela`; `editar_agendamento → remarcar_agendamento → _trocar_janela`. (`app/ferias/views.py:503`, `:688`; `app/ferias/services.py:457`, `:459`, `:462`, `:466`; `app/ferias/urls.py:21`, `:22`.) |
| `aprovar_solicitacao` | Havendo ciclo e saldo, cria/salva agendamento, disparando espelho. (`app/ferias/services.py:469`, `:483`, `:489`, `:493`, `:503`.) | `avaliar_solicitacao`, ação aprovar. (`app/ferias/views.py:733`, `:750`, `:753`; `app/ferias/urls.py:12`.) |
| `alinhar_status_por_data` | Relê sob trava; mudança de status de não cancelado dispara espelho. (`app/ferias/services.py:588`, `:600`, `:602`, `:604`.) | `sincronizar_status_ferias.Command.handle`, somente com `--apply`. (`app/ferias/management/commands/sincronizar_status_ferias.py:24`, `:44`, `:49`.) |

Pedir confirmação e registrar resposta do colaborador salvam somente campos de confirmação, que não disparam esse espelho. (`app/ferias/services.py:553`, `:571`, `:575`; `app/ferias/models.py:444`, `:450`.)

**Demais comandos, função genérica e escrita auxiliar.**

| Entrada/função | Comportamento e alcance |
|---|---|
| `curar_ausencia_sem_fim.Command.handle` | Seleciona não rejeitadas com dias > 1, fim nulo e tipo fora dos abertos; `--apply` chama `rederivar_fim`. **Não exclui férias derivadas.** (`app/ponto/management/commands/curar_ausencia_sem_fim.py:26`, `:29`, `:38`, `:46`.) |
| `rejulgar_status_por_tipo.Command.handle` | Seleciona `aguardando_documento`, sem tipo; `--apply` chama rejulgador. **Alcança derivadas nesse estado.** (`app/ponto/management/commands/rejulgar_status_por_tipo.py:19`, `:23`, `:30`.) |
| `escalonar_documentos_ausencia.Command.handle` | Encaminha `--apply` ao escalonador geral. **Alcance dado pela seleção de estado/prazo, sem exclusão de férias.** (`app/ponto/management/commands/escalonar_documentos_ausencia.py:24`, `:26`; `app/ponto/services/ausencia.py:993`.) |
| `zerar_dados_operacionais.Command.handle` | Com confirmação, envia todas as ausências ao apagador de ensaio. **Inclui derivadas em banco de ensaio.** (`app/ponto/management/commands/zerar_dados_operacionais.py:71`, `:89`; `app/ponto/services/ausencia.py:1289`.) |
| `rederivar_minutos_h06_comp11.tratar_linha` | Busca PK, confere identidade e chama editor geral, sem veto próprio de tipo. **Função genérica; o plano atual do comando contém somente atestado/falta.** (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:126`, `:138`, `:142`, `:169`, `:50`, `:244`, `:265`.) |
| `_materializar_ausencia_de_resposta` | Recebe tipo/status e chama criador sem veto próprio a férias. **O fluxo atual fornece somente falta/atestado**, pelo mapa de escapes. (`app/chamados/services/materializacao.py:502`, `:527`, `:603`; `app/chamados/services/disputa_emissao.py:1538`.) |
| Portas desse materializador | `responder_pergunta_disputa` e `api_disputa_responder` fornecem resposta ao core; o mapa acima restringe os tipos produzidos. (`app/chamados/views.py:1228`, `:1283`; `app/chamados/urls.py:17`; `app/api/views.py:2577`, `:2620`; `app/api/urls.py:93`.) |
| Código de módulo `sombra_mascara_pessoal.py` | Exige banco `sombra`; atualiza documento e descrição sem filtro de tipo. **Alcança derivadas na sombra**, fora das portas da aplicação. (`bin/sombra_mascara_pessoal.py:10`, `:22`, `:44`, `:62`; `bin/sombra_regras_pessoais.py:64`, `:109`.) |

Também existem escritores históricos nas migrations de dados, sem filtro que exclua férias:

- `0057._para_frente` transforma `pendente` em estado documental/decisório; `_para_tras` retorna os estados abertos para `pendente`. **Alcançam férias selecionadas.** (`app/ponto/migrations/0057_a4_backfill_pendente.py:21`, `:35`, `:43`, `:58`, `:63`.)
- `0058._para_frente` e `_para_tras` alteram prazo das ausências em `aguardando_documento`. **Alcançam férias selecionadas.** (`app/ponto/migrations/0058_a4_relogio_recomeca.py:29`, `:37`, `:42`, `:57`, `:59`, `:63`.)
- `0059._mover`, chamado por `_para_frente`/`_para_tras`, altera prazo por estado, sem tipo. **Alcança férias selecionadas.** (`app/ponto/migrations/0059_a4_d0_no_primeiro_dia_do_cron.py:26`, `:32`, `:36`, `:48`, `:53`.)

O Django admin de `Ausencia` está em somente leitura: adicionar, alterar e apagar retornam falso. (`app/ponto/admin.py:38`; `app/core/admin.py:59`, `:62`, `:65`.)

**Leitores que casam férias com agendamento pelo `#N`: nenhum localizado; inexistência universal [nao verificado].** Os comportamentos concretos encontrados são:

| Leitor | O que efetivamente usa |
|---|---|
| `espelhar_ferias` | Colaborador + tipo férias + janela; pula dia existente. O `#N` é **escrito**, sem ser usado para casar a existente. (`app/ponto/services/ausencia.py:1183`, `:1200`, `:1207`.) |
| `AgendamentoFerias._guard_sobreposicao` | Trata como `_minhas` todas as férias do colaborador na janela antiga; não compara descrição. (`app/ferias/models.py:427`, `:430`, `:435`.) |
| `painel_ausencia` / template | Agrupa por `documento_referencia`; mostra intervalo/data da ausência e descrição como texto. (`app/ponto/views.py:2073`, `:2076`, `:2079`; `app/templates/ponto/partials/painel_ausencia.html:50`, `:57`.) |
| `dias_ferias_com_batida` | Lê agendamentos por `ferias_na_janela` e cruza suas datas com batidas; não casa descrição de ausência. (`app/folha/export.py:484`, `:491`, `:499`.) |

Ocorrências, contagens e situação atual de produção além do texto fornecido permanecem **[nao verificado]**.

## 13/11 12:57 · sessao 0858a2d44305 · - · - · origem=Ronald (tmux)

As duas portas que fornecem resposta arbitrária ao ramo escape são:

- `responder_pergunta_disputa`: recebe `resposta` do POST e chama o core. `app/chamados/views.py:1228`, `:1265`, `:1283`; rota `app/chamados/urls.py:17`.
- `api_disputa_responder`: recebe `resposta` de `request.data` e chama o core. `app/api/views.py:2577`, `:2614`, `:2620`; rota `app/api/urls.py:93`.

O core consulta `ESCAPES_QUE_VIRAM_AUSENCIA` e chama `_materializar_ausencia_de_resposta`; o mapa atual contém somente falta e atestado. `app/chamados/services/disputa_emissao.py:1538`; `app/chamados/services/materializacao.py:603`.

Outros caminhos chegam ao core, mas fornecem apenas hora e não alcançam esse ramo:

- `aplicar_declaracao`: passa `p['hora']`, produzida pelo parser de horas. `app/chamados/services/declaracao_texto.py:28`, `:35`, `:624`, `:635`, `:693`. HTTP `detalhe_chamado_local` com ação `aplicar_declaracao_texto`: `app/chamados/views.py:359`, `:478`; rota `app/chamados/urls.py:14`.
- `acao_corrigir_resposta`: exige regex `HH:MM` e pergunta de hora antes de chamar o core. `app/chamados/services/acoes_disputa.py:302`, `:336`, `:349`, `:371`. Entradas: `detalhe_chamado_local` → `:536`; `fio_colaborador` → `app/chamados/views.py:674`; `modal_fio_colaborador` → `:954`; dispatcher `app/chamados/services/fio.py:485`, `:639`. Rotas `app/chamados/urls.py:14`, `:21`, `:39`.
- `declarar_hora_colab`: chama core com `hora`. `app/chamados/services/declaracao_texto.py:306`, `:419`; entrada `responder_chamado_colaborador`: `app/chamados/views.py:1041`, `:1089`.

`declarar_nao_trabalhou` não chama o core nem cria Ausência: atualiza contexto e anexa nota. `app/chamados/services/declaracao_texto.py:189`, `:208`, `:211`.

## 13/11 12:55 · sessao 0858a2d44305 · - · - · origem=Ronald (tmux)

Censo externo ao produtor e às views centrais:

| Função/porta | Comportamento e alcance |
|---|---|
| `api_ausencias` | Lê `tipo` do POST e chama `criar_ausencia`, sem exclusão de férias; pode criar ou reabrir a linha pelo produtor. Reenvio pode anexar documento na existente. `app/api/views.py:1671`, `:1705`, `:1805`, `:1833`. Rota `ausencias/`: `app/api/urls.py:82`. |
| `api_ausencia_documento` | Busca por PK+colaborador, sem filtro de tipo; exige estado aberto e chama `anexar_documento`. Alcança férias abertas. `app/api/views.py:3052`, `:3069`, `:3078`, `:3107`. Rota: `app/api/urls.py:83`. |
| `veredito_celula` | Para justificado usa `tipo_ausencia` recebido e cria aprovada, sem veto próprio a férias. `app/ponto/services/veredito_celula.py:211`, `:333`, `:337`. HTTP `veredito_celula_view` aceita o tipo do POST: `app/ponto/views.py:3312`, `:3340`; rota: `app/ponto/urls.py:58`. |
| `lancar_ausencia_porta` | Recebe qualquer tipo e cria aprovada; não veta férias. `app/ponto/services/checar_ausencia.py:43`. Chamada `_aplicar_ausencia`: `app/core/views_ia.py:971`, `:985`; despacho `:153`; HTTP `ia_perguntar`: `:859`, `:874`; rota `app/core/urls.py:15`. **O preview normal veta férias expressamente**: `app/ponto/services/checar_ausencia.py:10`, `:21`. |
| `Ausencia.save` | Sem veto à origem derivada: carimba decisão; deriva dias do range e fim a partir dos dias quando cabível. `app/ponto/models.py:868`, `:872`, `:894`, `:902`. |
| `curar_ausencia_sem_fim.Command.handle` | Seleciona não rejeitadas, dias>1, fim nulo e tipo fora de `EM_ABERTO`, sem excluir férias; `--apply` chama `rederivar_fim`. `app/ponto/management/commands/curar_ausencia_sem_fim.py:26`, `:29`, `:38`, `:46`. |
| `rejulgar_status_por_tipo.Command.handle` | Seleciona `aguardando_documento`, sem filtro de tipo; `--apply` chama `rejulgar_status_pelo_tipo`. Alcança férias nesse estado. `app/ponto/management/commands/rejulgar_status_por_tipo.py:19`, `:23`, `:30`. |
| `escalonar_documentos_ausencia.Command.handle` | Encaminha `--apply` ao escalonador geral, sem filtro próprio de tipo. `app/ponto/management/commands/escalonar_documentos_ausencia.py:24`. |
| `zerar_dados_operacionais.Command.handle` | Exige banco de ensaio; com `--confirmar`, manda `Ausencia.objects.all()` para exclusão. Alcança férias em ensaio. `app/ponto/management/commands/zerar_dados_operacionais.py:24`, `:27`, `:71`, `:89`. |
| `rederivar_minutos_h06_comp11.tratar_linha` | Busca por PK, compara identidade e usa editor geral sem veto próprio a férias. Porém o plano atual do comando usa lista fechada exclusivamente de atestado/falta. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:126`, `:138`, `:142`, `:169`, `:50`, `:244`, `:265`. |
| `_materializar_ausencia_de_resposta` | Função recebe tipo sem veto e cria pelo produtor; chamador runtime usa mapa somente falta/atestado, portanto o caminho atual não fornece férias. `app/chamados/services/materializacao.py:502`, `:527`, `:603`; chamador: `app/chamados/services/disputa_emissao.py:1425`, `:1538`. |

Exclusões concretas:

- `_declarar_ausencia_de_pergunta` recebe tipo de mapa exclusivamente falta/atestado: `app/chamados/services/materializacao.py:468`, `:475`; `app/chamados/models.py:3717`.
- Regularização e abandono provisório fixam `tipo='falta'`: `app/ponto/services/regularizacao.py:118`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`.
- `AusenciaAdmin` herda `SoLeitura`; add/change/delete retornam falso: `app/ponto/admin.py:38`; `app/core/admin.py:59`.
- `trocar_plantao` lê/trava Ausência e altera a célula pelo `editar_dia`, sem escrever Ausência: `app/ponto/portas/celula.py:670`, `:695`, `:713`.
- `decididos_do_periodo` e `linha_do_tempo` apenas leem e montam cartões: `app/ponto/services/dia_decidido.py:529`, `:550`, `:561`, `:566`, `:587`, `:599`.

Script auxiliar que também escreve:

- Código de módulo de `bin/sombra_mascara_pessoal.py` exige banco `sombra`, usa schema `juliani`, atualiza `documento` e faz `bulk_update` de `descricao`, sem filtro tipo; portanto alcança férias na sombra. `bin/sombra_mascara_pessoal.py:10`, `:16`, `:22`, `:44`, `:62`; inventário: `bin/sombra_regras_pessoais.py:64`, `:109`. Chamadas shell: `bin/sombra.sh:204`, `:272`.

_4603 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
