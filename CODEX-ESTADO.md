# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 10:25** · publicado em **26/11/2026 10:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 10:25 · sessao 034edfafc5b6 · INTERVALO-PRE-ASSINALADO-CONTRATO · leitura · origem=Code

# Dossiê antes do construtor — INTERVALO-PRE-ASSINALADO-CONTRATO

**Base `558de6bd4bba318bac4130a14936946e218f885d`.** Conferi os 4.716 arquivos regulares da cópia contra os objetos Git desse commit. O aval vigente é o **INTERVALO-PRE-ASSINALADO-CONTRATO-V2**. A entrega é em leitura, no formato MÃO ÚNICA. `CLAUDE.md:412` · `app/docs/BACKLOG.md:2554` · `app/docs/RELATO.md:230`

## 1. Mapa

**O sistema faz parte da regra.** O motor já desconta a pausa batida que reconhece e indeniza o suprimido. No B1, quando a lista de pausas vem vazia, a soma do intervalo real dá zero. Nos ramos 12x36 e comercial que alcançam o cálculo de indenização, isso deixa a jornada sem desconto de pausa e pode indenizar o mínimo inteiro. Em HE ordinária, `_limite_com_suprimido` já evita a duplicação até o valor que o previsto descontou. No turno partido, a indenização é calculada entre dois períodos do mesmo dia e o ramo pula dias com menos de dois períodos. `app/ponto/motor_calculo_v2.py:1801–1819,1908–1928,2780–2801,2984–3024,3256–3275` · `app/docs/LEIS.md:193–194`

**Onde se decide “sem batida de intervalo”.** `calcular_mes` prepara o dia da jornada e chama `montar_periodos`. O caminho normal passa por `turnos_via_autoridade` → `parear_turnos` → `periodos_de_turnos`, que entrega os pares reconhecidos por `intervalos_do_turno`. Em seguida, `_periodos_pelo_marco` pode substituir o resultado pela ata. Nesse ramo, o motor só copia a pausa quando encontra as duas pontas e seus instantes nas batidas reais. A lista ausente chega como `()` a `calcular_periodo` e ainda passa pelo recorte da janela efetiva. Portanto, lista vazia também pode significar meia batida, par rejeitado pela geometria ou pausa fora da janela. `app/ponto/motor_calculo_v2.py:503–531,558–567,611–616,684–723,1897–1928,2362–2372,2474–2476` · `app/ponto/adaptador_motor.py:26–45` · `app/ponto/turnos.py:537–546`

**Onde se lê o cadastrado.** Há três respostas distintas no HEAD:

| Pergunta | Leitura atual |
|---|---|
| Qual é o mínimo usado na indenização? | `MotorBase.__init__` lê `intervalo_cadastrado(tipo_escala)` por fatia, sem passar o dia. `_intra_minima` combina esse número com o piso do previsto diário, com precedência para `parametros.intrajornada_minutos` quando fornecido. `app/ponto/motor_calculo_v2.py:831–865,1821–1847` |
| Quanto a grade S133 já descontou do previsto? | `_intervalo_descontado_no_dia` lê `escala.utils.intervalo_declarado_do_dia`. O caminho é célula → `vinculo_do_dia` → dia de trabalho → marcos `hi/hf/hii/hfi`. Esse leitor devolve só a primeira janela e devolve zero sem seus quatro marcos. Já `minutos_previstos_do_dia` percorre todas as pausas declaradas e tem fallback para os marcos. `app/ponto/motor_calculo_v2.py:1324–1352` · `app/escala/utils.py:2222–2248,2251–2281,2320–2363` · `app/escala/models.py:1135–1144,1177–1219` |
| O que a ata diz sobre a pausa daquele dia? | `periodos_do_dia` entrega a pausa batida ou um valor cadastrado quando falta uma ponta. Para `dna.intervalo_livre=True`, pede o valor com `celula.data`, sem produzir uma janela ou uma pergunta nesse ramo. A grade de realizado também já desconta a janela fixa que intersecta o turno fechado quando falta pausa reconhecida. `app/ponto/juiz_batida.py:273–336` · `app/ponto/turnos.py:756–795` |

**Quem chama.** No caminho de dinheiro, a tela **Recalcular** e o comando de fechamento chamam `recalcular_fechamento_mes` → `apurar_por_fatia` → `get_motor_cct`/`get_motor` → `calcular_mes`. O resultado alimenta `FechamentoMensal` e `DiaPago`; o TXT lê a rubrica gravada. O espelho compartilha `apurar_por_fatia` e atende admin, app e calendário. O PDF também tem chamada direta ao motor. São portas verificadas no código. Volume de chamadas por porta, distribuição por classe de motor e valores por rubrica em produção ficam **[nao verificado]**. A medida fornecida de 24/11 é **B1 207 dias / 74 colaboradores** e **B2 93 dias / 43 colaboradores**. `app/ponto/views.py:162,1280` · `app/ponto/management/commands/recalcular_fechamento.py:130` · `app/ponto/services/fechamento.py:306,755–775,1028,1224–1254` · `app/core/regua_cct.py:428–479` · `app/ponto/services/espelho.py:1198–1216` · `app/api/views.py:1361` · `app/colaboradores/services/calendario.py:602` · `app/relatorios/pdf_espelho.py:434` · `app/folha/export.py:448` · `app/docs/BACKLOG.md:2554`

## 2. Censo fechado dos sítios da fatia

**Critério.** AST nos 3.823 arquivos Python rastreados, com zero falhas de parse, cruzado com `git grep -n -w` no commit. O inventário abaixo cobre definições, chamadas diretas, aliases de importação, escritas e leituras de atributos, além de chaves literais nos sítios de integração. Testes são referência de contrato, fora da lista de chamadores de runtime. SQL externo, alterações por reflexão com nome calculado, estado do banco e execução real dos chamadores ficam **[nao verificado]**. Como o desenho ainda cabe ao construtor, o censo fecha os símbolos abaixo, sem presumir uma alteração nas portas de cadastro, cobrança ou persistência. `CLAUDE.md:412` · `app/docs/DOSSIES.md:42,61`

| Símbolo ou dado | Definição, escritores e leitores |
|---|---|
| `intervalo_cadastrado` | Definição em `app/ponto/juiz_batida.py:106`. Chamadas na própria ata `:274,317`, na inicialização do motor `app/ponto/motor_calculo_v2.py:832`, no calculador `app/ponto/calculador/alimentacao.py:407,498` e `app/ponto/calculador/periodo.py:520`, e na sonda `app/ponto/management/commands/diff_calculador.py:170`. |
| `intervalo_declarado_do_dia` e `_intervalo_desc_min` | Definição em `app/escala/utils.py:2251`. Leitores em `app/ponto/motor_calculo_v2.py:1344`, `app/ponto/services/espelho.py:1193`, `app/folha/porta_export.py:566`, `app/ponto/calculador/alimentacao.py:517` e `app/ponto/calculador/periodo.py:522`. O motor cria e preenche o memo em `app/ponto/motor_calculo_v2.py:1331–1352`; o espelho também o alimenta em `app/ponto/services/espelho.py:1180–1194`. `_limite_com_suprimido` é o único chamador de `_intervalo_descontado_no_dia` no censo. `app/ponto/motor_calculo_v2.py:1819` |
| Pausa no DNA e na ata | `montar_dna` produz marcos e `intervalo_livre`; as portas de geração, edição, reversão e regeneração gravam a célula. `EscalaColaborador.pausas_do_dia` lê os pares congelados. `periodos_do_dia` produz `intervalo` e `papeis`; chamadores estáticos no motor, em turnos, em Cadastro × Realidade e na exceção da folha. `app/ponto/management/commands/gerar_celulas.py:73–113` · `app/ponto/portas/celula.py:261,278,290–306,641–659,863–866,953–961` · `app/escala/models.py:1177–1219` · `app/ponto/juiz_batida.py:153,284–336` · `app/ponto/motor_calculo_v2.py:616` · `app/ponto/turnos.py:1633` · `app/escala/services/cadastro_realidade.py:299` · `app/folha/export.py:1150` |
| Campos de mínimo no motor | `__init__` escreve `intrajornada_cadastrada`, `intrajornada_fonte`, `intrajornada_minutos` e `_intra_do_perfil`. `_intra_minima` lê `intrajornada_cadastrada` e `_intra_do_perfil`; os três chamadores são 12x36, partido e comercial. O AST encontrou somente escritas no atributo `intrajornada_fonte` e nenhuma leitura externa de `MotorBase.intrajornada_minutos`. A flag `TipoEscala.intervalo_indenizavel` existe, participa da validação e é repassada pela ata. O motor não a lê. `app/ponto/motor_calculo_v2.py:822–865,1845–1847,2788,3014,3265` · `app/escala/models.py:108,574` · `app/ponto/juiz_batida.py:333` |
| Pares, `intervalos_do_motor` e `intra_descontada` | O adaptador e `_periodos_pelo_marco` escrevem o mapa de pausas por entrada. Os três corpos recortam e chamam `_carimbar_segmento`, escritor de `intervalos_do_motor`. `_intra_real` tem dois chamadores, 12x36 e comercial, que escrevem `intra_descontada`. Leem os pares o split de noturno e o calculador. O calculador também lê o desconto e tem escrita própria ao materializar sua projeção. `app/ponto/adaptador_motor.py:44` · `app/ponto/motor_calculo_v2.py:687–723,1035,1858,2780–2791,2910–2911,3256–3263` · `app/ponto/calculador/alimentacao.py:108,127,133` · `app/ponto/calculador/periodo.py:361,744–754` |
| `minutos_intrajornada_indenizada` | Default no período. Escritas do motor em 12x36, partido e comercial. Leituras diretas no total mensal, no limite de HE, no espelho, em `DiaPago`, no vigia de export e no DIFF. O PDF também lê pelo nome da rubrica. O calculador tem escrita própria e soma por dia. `app/ponto/motor_calculo_v2.py:235,370,1801,2800,3019–3020,3274` · `app/ponto/services/espelho.py:104` · `app/ponto/services/dia_pago.py:186,225` · `app/folha/porta_export.py:549` · `app/ponto/management/commands/diff_calculador.py:254` · `app/relatorios/pdf_espelho.py:847` · `app/ponto/calculador/periodo.py:362,747,821` |
| Jornada líquida, HE e alertas | `minutos_trabalhados` é escrito pelos três corpos, pelo caminho de crédito e na projeção do calculador. A soma, a HE, o fechamento, `DiaPago`, espelho e vigia leem o valor. `_limite_com_suprimido` tem quatro chamadas. O alerta de intra é escrito junto da indenização e lido pelo espelho e pelo PDF. `app/ponto/motor_calculo_v2.py:296–300,2575,2795–2801,2859,2912,3019–3024,3163,3269–3275,3304,3429` · `app/ponto/calculador/periodo.py:360,745` · `app/ponto/services/fechamento.py:862,978,990,1007` · `app/ponto/services/dia_pago.py:178,239–267` · `app/ponto/services/espelho.py:136,671` · `app/folha/porta_export.py:550` · `app/relatorios/pdf_espelho.py:975` |
| Cobrança por batida de intervalo | O cartório chama `perguntar_pela_celula`; a fábrica lê faltantes da ata, aplica vencimento e dedup, e chama `perguntar`. Os motivos são `intervalo_saida` e `intervalo_volta`. O comando `detectar_intervalo_ausente --alarme` é uma via de vigia, com leitura própria de faltantes. Zerar a rubrica no motor, por si só, não cala a fábrica. `app/ponto/services/cartorio.py:1061–1073` · `app/chamados/services/fabrica_da_celula.py:52–74,139–157,218–219` · `app/chamados/services/disputa_emissao.py:901–910` · `app/chamados/services/declaracao_texto.py:43–47,87–104` · `app/config/crons.py:491` · `app/ponto/management/commands/detectar_intervalo_ausente.py:96–108,120` |

## 3. Leis herdadas

- **L-031, L-032 e L-033.** A ficha de L-031 registra a regra histórica da flag e da pergunta, com a leitura da flag adiada. O V2 substitui o aval anterior desta obra, retira o cadastro novo e dá o resultado B1. L-032 protege a dedução do cadastrado e o reconhecimento da pausa dentro do envelope. L-033 mantém pagável o dia fechado cujo único furo é a pausa. O corte **LEI-DO-MIOLO-CEDE-A-LAMPADA** preserva a via da pergunta pela célula e descreve expressamente o caso com retorno batido e ida faltante. `app/docs/LEIS.md:79–81` · `app/docs/CORTES.md:416` · `app/docs/BACKLOG.md:1634,2554` · `app/folha/export.py:1150–1171`
- **L-020, L-024/S133, L-025, L-085 e L-087.** Protegem DNA congelado, grade como fonte do previsto, universo de batidas apuráveis, dia de início da jornada e papel da batida pela ata com instante do fato. O corte **JUIZ-ESCALA-NASCE** reafirma S133. O dossiê de célula e turno registra o precedente de ler o realizado e as pausas pela autoridade de turno. `app/docs/LEIS.md:71,75–76,117,119` · `app/docs/CORTES.md:367` · `app/docs/DOSSIES.md:73`
- **L-086, L-095, L-097 e L-099.** Protegem o feriado previsto, a lavra do dinheiro do dia, a janela de HE já emendada pela NOTURNO-A e a coerência entre leitores. A forma atual da janela mantém trabalhado e noturno após o marco de saída e trata a HE conforme a L-097. `app/docs/LEIS.md:113,115,118,122` · `app/docs/RELATO.md:143`
- **L-090, L-092, L-110 e DINHEIRO.** A competência vem da janela da empresa. Para esta obra, 09/10 ficam em Pauta DP; apply da 11 segue DINHEIRO item 1, com reversão, prova de preservação da exportada e DIFF de frota da intrajornada no RELATO. O corte **REFERENCIA-E-A-LEI** e os precedentes dos dossiês O146/O214 dão a forma do RED e do contrato de rubricas existentes. `app/docs/LEIS.md:123,125,136` · `app/docs/CORTES.md:383` · `app/docs/DOSSIES.md:182,195–205,229–241` · `CLAUDE.md:418–427` · `app/docs/BACKLOG.md:2554`

## 4. Contrato de entrada

| Item | Contrato |
|---|---|
| Fonte | Vínculo do dia e DNA da célula para o desenho daquele dia; batidas apuráveis e ata/geometria para o fato. O cadastro já tem janelas e modalidade por duração. `intervalo_duracao_min` tem default 60; um valor positivo isolado no template não distingue declaração explícita de default, nem prova pausa naquele dia. `app/escala/alimentacao.py:228` · `app/escala/models.py:52–56,108–110,281–308,1177–1219` · `app/ponto/turnos.py:2940–2948` · `app/ponto/juiz_batida.py:129–150,307–327` |
| Unidade | Um **dia de jornada por colaborador**, com pares medidos em minutos, preservando segundos das batidas. A rubrica gravada é em horas decimais. A competência é dada por `janela_fechamento` com o corte da empresa. `app/ponto/motor_calculo_v2.py:602–609,936–951,1925–1928,2440–2448,370` · `app/ponto/services/fechamento.py:641,1028` · `app/ponto/janelas.py:54–63` |
| Universo e exclusões | Da competência 11 em diante, para dia com intervalo cadastrado: B1 recebe dedução e intra zero; B2 mantém indenização do suprimido. Dia sem intervalo cadastrado é controle. Para 09/10, somente Pauta DP. O comando de pauta **da exportada** exige competência entregue, portanto sua aplicabilidade à 10 por empresa depende dessa condição; a guarda geral do HEAD registra a retirada da 10. `app/docs/BACKLOG.md:2554` · `app/ponto/services/fechamento.py:36–49` · `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:107–135` |
| Escala | A apuração existente agrega por fatia do vínculo e o espelho alimenta os memos por janela. O DIFF tem como referência operacional a função real de fechamento e deve cobrir a frota, além dos três REDs do aval. Denominador da sonda, sobreposição de colaboradores entre B1 e B2, composição por empresa/ciclo e impacto em horas ou reais ficam **[nao verificado]**. `app/ponto/services/fechamento.py:152,306,774–776` · `app/ponto/services/espelho.py:1167–1194` · `app/ponto/services/diff_frota.py:45,84–87` · `CLAUDE.md:423–426` · `app/docs/BACKLOG.md:2554` |

## 5. Matriz de transições — REDs propostos

Os horários abaixo são **fixtures propostas**. B1, B2 e o controle sem cadastro são os três casos mínimos do aval. Os demais fixam fronteiras já respondidas. `app/docs/BACKLOG.md:2554` · `app/docs/LEIS.md:136`

| Entrada | Resposta pela regra | Teste proposto | [nao verificado]
|---|---|---|
| **B1**, comp. 11, 07:00–19:00, pausa fixa 12:00–13:00 no dia, só entrada e saída final | 660 min trabalhados, 60 deduzidos, intra 0, sem HE pela pausa e sem cobrança de intervalo no alcance do aval. Ver §8.1 para a fábrica de perguntas. `app/docs/BACKLOG.md:2554` · `app/docs/LEIS.md:79–81` | `test_b1_sem_batidas_de_pausa_deduz_60_e_zera_intra` |
| **B2**, mesma jornada, pausa reconhecida 12:15–12:45 | 690 min trabalhados, 30 deduzidos, 30 indenizados e zero HE pelos mesmos 30 min. `app/docs/BACKLOG.md:2554` · `app/docs/LEIS.md:80` · `app/ponto/motor_calculo_v2.py:1801–1819` | `test_b2_pausa_curta_indeniza_30_sem_he_dobrada` |
| **Sem cadastro**, 12x36, 07:00–19:00, sem janela, duração 0 e só as pontas | 720 min trabalhados, desconto cadastral 0 e indenização ordinária de 60 pelo piso nesse cenário. `app/docs/BACKLOG.md:2554` · `app/escala/models.py:6` · `app/ponto/motor_calculo_v2.py:1840–1847,2782–2801` | `test_sem_intervalo_cadastrado_preserva_o_piso_e_o_trabalhado` |
| Pausa cadastrada de 60, batida reconhecida de 60 deslocada dentro do envelope | Deduz os 60 batidos, intra 0, sem HE pela pausa. `app/docs/LEIS.md:80` · `app/ponto/motor_calculo_v2.py:2780–2801` | `test_pausa_completa_no_envelope_continua_batida` |
| B2 com suprimido de apenas 5 min, em ramo sujeito à tolerância vigente | Deduz o gozado e mantém intra 0 pela tolerância de 10 min. `app/docs/BACKLOG.md:1686,2554` · `app/ponto/motor_calculo_v2.py:69,2787–2794,3261–3267` | `test_b2_preserva_tolerancia_do_suprimido` |
| Mesmo B1 em 09/10 e, no teste de borda, turno iniciado na última data da 10 com saída na madrugada da 11 | Gravados de 09/10 intactos, divergência encaminhada à Pauta DP. O turno de borda pertence à 10. `app/docs/BACKLOG.md:2554` · `app/docs/LEIS.md:117,123` · `app/ponto/motor_calculo_v2.py:2440–2448` | `test_b1_competencias_09_10_so_pauta_dp` e `test_virada_da_competencia_respeita_inicio_do_turno` |

## 6. Padrão nomeado

**Single Source of Truth / Aggregate Root — célula soberana / fonte única.** O desenho do dia vem da célula e do vínculo que a gerou. O cálculo consome esse desenho com o fato já julgado pela ata ou pela geometria. O padrão de prova de frota é **Shadow mode / Parallel run**, com o resultado da função real de fechamento. `app/docs/GLOSSARIO.md:11,22` · `app/docs/LEIS.md:71,75,136,144` · `app/ponto/services/diff_frota.py:45,84–87`

## 7. Arquivos tocados × obras em voo

**Escopo previsto para a construção:** `app/ponto/motor_calculo_v2.py` e os REDs em `app/ponto/tests/`. O caminho de teste é uma proposta. O `!` do motor é exclusivo deste item, e o aval exclui cadastro novo na UI. O pouso da NOTURNO-A consta do RELATO e o commit `0d1e5ddf5` é ancestral do HEAD. A ordem registrada para o motor põe FERIADO e PAR-PELO-TURNO V6 antes do INTERVALO V2. `CLAUDE.md:143,315,412` · `app/docs/BACKLOG.md:2020,2554` · `app/docs/PROMPTS.md:2943` · `app/docs/RELATO.md:143`

Comparei as cópias existentes com seu ancestral comum ao HEAD e li os dois arquivos, incluindo a alteração local do PAR c7. As linhas a seguir são do HEAD; os hashes identificam as cópias conferidas. `app/docs/RELATO.md:3,5–6,14,16–17`

| Obra e cópia conferida | `app/ponto/motor_calculo_v2.py` | `app/ponto/turnos.py` |
|---|---|---|
| **FERIADO-SEM-LIMITE-DO-SUPRIMIDO**, c4d `28eb07f5d` | **Colisão de arquivo e dependência de dados.** Na faixa pedida **3070–3129**, o diff troca o corpo da dobra em **3115–3123** e a mensagem em **3126**; há outro trecho logo adiante. A detecção da pausa e as escritas de intra ficam em outros trechos do HEAD. A dobra lê justamente `minutos_trabalhados` e `minutos_intrajornada_indenizada`, então há efeito conjunto mesmo onde os hunks não se cruzam. A c4e fica **[nao verificado]**. `app/ponto/motor_calculo_v2.py:689–723,2787–2801,3014–3024,3070–3133,3258–3275` · `app/docs/BACKLOG.md:1998` · `app/docs/RELATO.md:5,13251` | Sem hunk próprio da obra. A diferença direta contra o HEAD nessa cópia corresponde à base anterior à PARCIAL-BORDA. `app/docs/RELATO.md:13,13246–13252` · `app/ponto/turnos.py:2644–2729` |
| **E1-PREVISAO-24H**, v6 `f8d398282` | **Colisão de arquivo.** A v6 troca **3404–3428**, no limite diário do comercial, pela leitura do previsto S133. Esse limite alimenta o mesmo `_limite_com_suprimido` que lê o resultado da intra. A versão v4 descrita no RELATO preservava o motor; o diff da v6 já toca o arquivo. `app/ponto/motor_calculo_v2.py:3404–3429` · `app/docs/RELATO.md:16–17` | Sem diff nesse arquivo na cópia v6. A interface de interesse para esta fatia continua sendo previsto e intervalo declarado. `app/escala/utils.py:2251–2284` · `app/docs/BACKLOG.md:2006` |
| **PAR-RELAMPAGO-A-LISTA-DO-ADMIN**, `61c5323a0` + alterações locais c7 | Sem diff no motor inspecionado. `app/docs/RELATO.md:6,132` · `app/ponto/motor_calculo_v2.py:453–506` | **Arquivo de dependência com diff próprio.** Toca `turnos_do_colab`, reapresentação de batida, consulta de par completo e dia da batida. Colide por arquivo com DIA-DA-BATIDA; o escopo previsto do INTERVALO apenas consome a geometria. `app/ponto/turnos.py:1850–1920,2185–2218,2951–3001` · `app/docs/BACKLOG.md:2337` |
| **DIA-DA-BATIDA-JUIZ-UNICO**, `a4d3675c7` | Sem diff no motor inspecionado. O mapa do dia do par continua sendo um insumo do caminho de cálculo. `app/ponto/motor_calculo_v2.py:936–951,2362–2365` · `app/docs/RELATO.md:3` | **Arquivo de dependência com diff próprio.** Muda as consultas de dia por batida e a carga em lote. Cruza o PAR na região de `dia_das_batidas` e `dia_do_turno_da_batida`. `app/ponto/turnos.py:2951–3020,3113–3116` · `app/docs/RELATO.md:3` |

##

[truncado em 20000 de 21772 caracteres]

## 26/11 10:22 · sessao 0cbbfb568f2f · DIA-DA-BATIDA-JUIZ-UNICO · verificador · origem=Code

1. **nota | O achado 1 da F1 está curado na origem do mapa por PK.** A casca chama `_carregar_dias_das_batidas([cid], ini, fim)`. O lote chama a mesma função com todos os IDs, reaproveita `_ata` e guarda o resultado. Ambos passam por `_turnos_do_colab_puro` → `parear_turnos` → `_dia_das_batidas_puro`. O adaptador unitário consulta esse mapa por PK. A assinatura perdeu `turnos=` e a varredura dos chamadores Python, incluindo os aliases da API, encontrou apenas chamadas com colaborador e datas na casca, ou com colaborador no lote. `app/ponto/turnos.py:2951`, `app/ponto/turnos.py:2975`, `app/ponto/turnos.py:2991`, `app/ponto/turnos.py:3056`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3157`, `app/api/views.py:1444`, `app/api/views.py:2042`, `app/colaboradores/services/pos_vinculo.py:43`, `app/ponto/services/espelho.py:465`, `app/core/services/painel_op.py:330`, `app/ferias/management/commands/listar_ferias_com_batida.py:54`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`

2. **nota | O histórico é inteiro e o pad seleciona as chaves.** A consulta usa `batidas_apuraveis(None)` sem datas, filtra pelos IDs e ordena por colaborador, timestamp e PK. Retratada fica fora e pendência de aprovação continua apurável. Para cada colaborador com alguma PK apresentada, os limites do pareamento são a véspera da primeira marca e o dia civil da última. O lote consulta células numa faixa agregada e devolve a cada colaborador seu recorte histórico. Com os mesmos fatos e fuso, `ini/fim` só determina quais PKs saem. O pad civil é `[ini−2, fim+1]`, inclusivo nas duas pontas. Dentro do pareador, o recorte por **instante** do segmento inclui a meia-noite de `seg_ini−2` e exclui a de `seg_fim+2`. O mapa começa com o dia civil e põe o `data_turno` nas chaves reclamadas. `app/ponto/turnos.py:45`, `app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1980`, `app/ponto/turnos.py:1988`, `app/ponto/turnos.py:2924`, `app/ponto/turnos.py:2941`, `app/ponto/turnos.py:3020`, `app/ponto/turnos.py:3026`, `app/ponto/turnos.py:3040`, `app/ponto/turnos.py:3056`, `app/ponto/turnos.py:3003`

3. **nota | Há mudança além dos três casos do RED.** Reproduzi em memória, com o pareador da cópia e carga simulada, um vínculo 19h–07h sem DNA, com E às 02h e S às 07h **ambas em 25/10**. Para `[25/10, 25/10]`, a F1 pareia as duas, descarta o turno de 24/10 pelo recorte e devolve **25/10** para as duas PKs. A F2 devolve **24/10**. Também reproduzi uma mudança causada pela borda direita: E em 25/10 às 05h e S em 26/10 às 06h, no mesmo tipo de vínculo. Para `[24/10, 24/10]`, a F1 vê a E como turno aberto de **24/10**. A F2 carrega a S, fecha o par e publica **25/10** para a E, pois a saída excede o envelope da jornada da véspera. A S fica fora das chaves apresentadas nessa janela. `app/ponto/turnos.py:229`, `app/ponto/turnos.py:299`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1492`, `app/ponto/turnos.py:1497`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3027`, `app/ponto/turnos.py:3056`

4. **bloqueia | A resposta ainda diverge da autoridade registrada.** `_T1` aponta para `turnos_do_colab`, que conserva a carga recortada. No próprio RED das órfãs, reproduzi a última S com `Turno.data_turno = 24/10` em `turnos_do_colab(24/10, 26/10)` e **25/10** no mapa F2 para a mesma PK e janela. É o mesmo pareador com cargas que produzem respostas diferentes. O espelho usa o mapa para marcas e cartões, mas usa os turnos recortados para `datas_saida_orfa`. O painel também cruza o mapa histórico com `dias_encerrados` calculado sobre turnos da janela. `app/core/juizes.py:216`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:3056`, `app/ponto/turnos.py:3142`, `app/ponto/turnos.py:3148`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:66`, `app/ponto/services/espelho.py:465`, `app/ponto/services/espelho.py:589`, `app/ponto/services/espelho.py:628`, `app/core/services/painel_op.py:330`

5. **bloqueia | O signal do cartório e o evento do fechamento ainda escolhem pelo dia civil.** `_dias_batida` entrega `(D−1, D)` ao cartório no `post_save` e no `post_delete`. Para a S de 25/10 do RED de 24h10, esse receiver pede 24 e 25/10, enquanto o mapa atribui a S a **23/10**. O receiver do núcleo pede outro recorte, `D−2..D+1`. Já o evento de fechamento recebe o timestamp na criação e na retratação, converte para a data civil e agenda uma única competência. Com corte em 24/10, a jornada de 23/10 e a saída de 25/10 caem em competências distintas, e esse evento agenda a da saída. Efeito final nas células e no gravado em produção [nao verificado]. `app/ponto/signals.py:23`, `app/ponto/signals.py:26`, `app/ponto/signals.py:92`, `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:61`, `app/ponto/registro_batida.py:176`, `app/ponto/registro_batida.py:301`, `app/ponto/services/fechamento.py:1593`, `app/ponto/services/fechamento.py:1619`, `app/ponto/services/fechamento.py:1833`, `app/ponto/janelas.py:24`

6. **nota | Escritores e leitores.**
   - **Mapa `{pk: dia}`.** `_carregar_dias_das_batidas` monta a projeção civil de entrada e `_dia_das_batidas_puro` publica o mapa em memória. Leem a adesão reativa, o espelho, as duas rotas da API, o painel, o diagnóstico de férias, o vigia de plantio e o adaptador unitário. O signal da adesão chega ao mapa pelo serviço pós-vínculo. O PDF lê as linhas do espelho. `app/ponto/turnos.py:3001`, `app/ponto/turnos.py:3026`, `app/colaboradores/services/pos_vinculo.py:43`, `app/ponto/signals.py:234`, `app/ponto/services/espelho.py:589`, `app/api/views.py:1461`, `app/api/views.py:2046`, `app/core/services/painel_op.py:330`, `app/ferias/management/commands/listar_ferias_com_batida.py:54`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`, `app/ponto/turnos.py:2991`, `app/relatorios/pdf_espelho.py:596`
   - **`Turno.data_turno` e `TurnoMaterializado.data_turno`.** `_mk` publica o dia calculado por `_data_do_turno`. O mapa copia esse dia, o espelho e a API também leem turnos diretamente, e o núcleo copia o campo para `TurnoMaterializado` via `recompute_turnos`, a porta registrada. Painel e reconciliador leem a materialização. No fechamento mensal, o motor monta outro mapa, por instante da entrada e depois por identidade do período, e o fechamento passa `resultado.dia_da_jornada` à lavratura. `app/ponto/turnos.py:285`, `app/ponto/turnos.py:312`, `app/ponto/turnos.py:3009`, `app/ponto/services/espelho.py:628`, `app/api/views.py:1447`, `app/ponto/nucleo.py:47`, `app/ponto/nucleo.py:59`, `app/core/portas.py:118`, `app/core/services/painel_op.py:222`, `app/chamados/reconciliador.py:97`, `app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:884`, `app/ponto/motor_calculo_v2.py:2961`, `app/ponto/services/fechamento.py:1241`
   - **Outros campos chamados `data_turno`.** Os dois kernels da API gravam a **data civil** no `contexto_json['data_turno']` de `batida_fora_escala_leve`, e o catálogo lê essa chave como dia do chamado. A regularização também usa o dia civil da entrada em `Justificativa.data_turno`, lido pelo fechamento. O instante da marca está em `Batida.timestamp`. Na adesão, o emissor grava `periodo` e `data_inicio`, e o pós-vínculo compara os valores do mapa aos dois limites inclusivos de `periodo`. `app/api/views.py:717`, `app/api/views_core.py:711`, `app/chamados/catalogo/modulos.py:168`, `app/chamados/catalogo/modulos.py:219`, `app/ponto/services/regularizacao.py:73`, `app/ponto/services/regularizacao.py:91`, `app/ponto/services/fechamento.py:680`, `app/ponto/models.py:23`, `app/chamados/services/adesao.py:86`, `app/colaboradores/services/pos_vinculo.py:38`

7. **fila | A adesão continua com duas regras de evidência.** O fluxo reativo exige dia do mapa dentro do período do pai quando há dois limites. O cron aceita qualquer batida apurável do colaborador. Ambos passam por `transicionar_com_pauta` → `pai.reconciliar` → `_gravar_estado`. A F2 escreve o mapa em memória e o fechamento mantém a ordem **Colaborador → Disputa → Chamado → Pergunta**. A evidência é consultada antes da trava do gate. Comportamento sob retratação concorrente [nao verificado]. `app/colaboradores/services/pos_vinculo.py:37`, `app/colaboradores/services/pos_vinculo.py:49`, `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/chamados/services/adesao.py:143`, `app/chamados/services/fecho_runtime.py:157`, `app/chamados/models.py:670`, `app/chamados/models.py:777`, `app/chamados/models.py:2939`, `app/chamados/models.py:2948`, `app/chamados/models.py:2961`, `app/chamados/models.py:2965`

8. **nota | Relógio e mutantes.** A carga F2 calcula os limites pelos timestamps das marcas. Há uma leitura de relógio herdada em `periodos_do_dia` → `intervalo_cadastrado`, que chama `date.today()` sem `dia`. Ela alimenta intervalo e perguntas, enquanto a projeção de papéis usa as bordas e luzes da ata. O cartório também lê o relógio no julgamento disparado pelo signal. Nos testes em memória, com a carga simulada e os métodos F1 reconstituídos do diff, os três casos F2 passaram. **`borda`** falhou na igualdade entre janelas nos dois REDs, **`lote`** na igualdade dos mapas nos dois, **`civil`** no dia esperado da saída de 24h10, e **`sem_pad`** na igualdade do conjunto de PKs, com três chaves a mais. Execução com Django e banco [nao verificado]. `app/ponto/turnos.py:3030`, `app/ponto/turnos.py:1631`, `app/ponto/turnos.py:1682`, `app/ponto/juiz_batida.py:129`, `app/ponto/juiz_batida.py:274`, `app/ponto/juiz_batida.py:328`, `app/ponto/services/cartorio.py:2084`, `app/ponto/services/cartorio.py:2105`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:53`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:54`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:58`, `app/ponto/tests/test_dia_das_batidas_estavel_na_janela.py:72`

9. **fila | Menos consultas por chamada, mais histórico relido nos laços.** Pela leitura do código, a casca F2 faz **4–5 consultas** com PK apresentada, ou **1** com o pad vazio, ante **5–8** na F1. São batidas, células, candidatos, vínculo da ata e eventualmente geradoras faltantes. Conte uma busca pelos pais e uma carga completa **por pai com período**, inclusive para períodos iguais. O signal repete isso a cada `save` de `Batida`, e o pós-vínculo chama a adesão depois do laço diário. Há outras multiplicações por dia/ausência no diagnóstico de férias, por marca na emissão de disputa e por marca na foto E4B, sob trava do colaborador. Na foto, **B** chamadas que leem **B** marcas cada já dão volume quadrático só na leitura das marcas. O painel e o vigia de plantio reutilizam uma carga por lote e por colaborador, respectivamente. SQL executado, latência e frequência em produção [nao verificado]. `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1889`, `app/ponto/turnos.py:1903`, `app/ponto/turnos.py:1914`, `app/ponto/turnos.py:3020`, `app/ponto/turnos.py:3040`, `app/ponto/turnos.py:3045`, `app/escala/alimentacao.py:148`, `app/colaboradores/services/pos_vinculo.py:37`, `app/colaboradores/services/pos_vinculo.py:95`, `app/ponto/signals.py:206`, `app/ponto/signals.py:234`, `app/ferias/management/commands/listar_ferias_com_batida.py:43`, `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/foto_e4b.py:225`, `app/chamados/services/foto_e4b.py:250`, `app/chamados/services/foto_e4b.py:282`, `app/core/services/painel_op.py:327`, `app/ponto/management/commands/vigia_plantio_overnight.py:82`

10. **nota | A catraca e o R6 batem com a cópia.** O B1 está em `_carregar_dias_das_batidas:3026` e `dia_do_turno_da_batida:2989`, com identidade e linhas registradas. Rodei em memória os **35 testes** da catraca, com **9 pareadores e 81 derivações**, os **10 testes** do contrato de projeção civil e o selo R6. Todos passaram. O R6 da cópia publica **18/20 verdes, 20 declaradas**, e a célula de juiz único segue vermelha pelo registro de dívida. Os quatro arquivos do diff batem com os blobs de destino do anexo. Hash do commit HEAD, suíte com banco e contagens F2 das competências 10 e 11 [nao verificado]. `app/core/tests/test_medida_turno_do_par.py:108`, `app/core/tests/test_medida_turno_do_par.py:238`, `app/core/tests/test_medida_turno_do_par.py:338`, `app/core/tests/test_medida_turno_do_par.py:546`, `app/core/tests/test_medida_turno_do_par.py:562`, `app/ponto/tests/test_contract_no_batida_date.py:650`, `app/core/tests/test_selo_r6_frase_do_juiz.py:20`, `app/core/placar_estrutural.py:443`, `app/core/contratos_estruturais.py:225`

## 26/11 10:21 · sessao d7fbb66af7a6 · DIA-DA-BATIDA-JUIZ-UNICO · sonda · origem=Code

```text
rc_sombra=125
```

No dado fornecido, o Docker falhou ao montar `/app/staticfiles` antes de iniciar a sonda. O envelope executa o script dentro do contêiner e propaga o código de saída. [bin/sonda_leitura.sh:235](/home/ronald/codex_ws/a4d3675c7a187493c006eb291ee40580a3ea7a77/bin/sonda_leitura.sh:235)

Não repeti a execução porque o envelope cria um arquivo temporário e acrescenta uma linha ao log. [bin/sonda_leitura.sh:216](/home/ronald/codex_ws/a4d3675c7a187493c006eb291ee40580a3ea7a77/bin/sonda_leitura.sh:216) [bin/sonda_leitura.sh:248](/home/ronald/codex_ws/a4d3675c7a187493c006eb291ee40580a3ea7a77/bin/sonda_leitura.sh:248)

`universo_colabs`, `pks_na_comp`, `pks_com_dia_diferente`, `colabs_afetados`, `delta`, `top_colabs` e `exemplos` da F2: [nao verificado]. Se o número novo é 0: [nao verificado].

## 26/11 10:18 · sessao 9f2e3f3e9784 · E1-PREVISAO-24H · verificador · origem=Code

1. **bloqueia | Férias ainda contam a falta recusada.** `faltas_periodo` continua contando direto as ausências aprovadas de tipo `'falta'`, sem consultar o mapa da previsão. No caso novo de cinco válidas e uma inválida, a consulta conta seis, e `aplicar_art130` resolve 24 dias onde o teste exige 30. A sincronização do período também consome esse resultado e pode gravá-lo em `dias_direito`. `app/ferias/models.py:52–57,143–150`, `app/ferias/services.py:382–399,405–412`, `app/ferias/tests.py:53–78`.

2. **nota | Todas as linhas alteradas no motor estão no bloco do limite diário do `MotorComercial`.** Inventário do hunk fornecido. `app/ponto/motor_calculo_v2.py:3404–3413`.

   - **Base 3404–3407:** removidos os comentários sobre o limite por marcos e a inicialização pelo padrão de 480 min. **Base 3408–3414:** removida a seleção de `_md_m2` pelo vínculo ou template. **Base 3415–3418:** removidas a chamada aos marcos e a captura da exceção. **Base 3419–3427:** removidos a guarda de meia-noite e o cálculo do span menos a primeira pausa. **Base 3428:** substituído o comentário sobre intrajornada. Todas eram linhas do limite diário. `diff.diff`, hunk `-3401,31 +3401,15`.
   - **Cópia 3404–3408:** comentários sobre a fonte e a medição. **3409:** o limite passa a ler `_jornada_do_dia(dia)`. **3410–3411:** se o previsto é zero, pula a apuração de excedente desse dia. **3412:** comentário sobre a intrajornada. A subtração de `3413` é contexto preservado. O reset por período já ocorre dentro do motor, antes de recalcular o dia. O fechamento lê os totais resultantes, e os hunks de espelho e calendário mexem na palavra. `app/ponto/motor_calculo_v2.py:3348–3358,3404–3427`, `app/ponto/services/fechamento.py:948–953`, `app/ponto/services/espelho.py:889–892`, `app/colaboradores/services/calendario.py:750–756`.

3. **bloqueia | A leitura da grade tem um ramo que escapa ao aval literal.** Com resposta da grade, a cadeia é `MotorComercial → _jornada_do_dia → _previsto_do_dia → minutos_previstos_do_dia → previsao_invalida_do_vinculo → previsao_do_dia_valida`. A decisão de validade continua no juiz registrado. Porém, colaborador ausente, linha não encontrada ou exceção na consulta devolvem `None`, e `_jornada_do_dia` passa ao `self.jornada_minutos` do template. A porta e o fallback já existiam, mas o hunk os torna alcançáveis no limite diário comercial. O fechamento fornece `colaborador_id`, o que cobre a ausência do argumento. Ocorrências dos demais ramos na frota [nao verificado]. `app/core/juizes.py:213–217`, `app/ponto/motor_calculo_v2.py:810,1244–1294,3409`, `app/escala/utils.py:407–432,2472–2512`, `app/ponto/services/fechamento.py:306–311`.

4. **nota | Quem produz e quem lê o previsto e a HE.** A porta de célula grava DNA, `trabalha` e geradora. O fechamento alimenta o motor com células em lote. Espelho e PDF também alimentam folgas. O motor atribui o colaborador e memoiza o previsto por dia. Um acerto do memo custa zero consultas nessa leitura. Em dia ainda não memoizado, há uma consulta de colaborador e uma de vínculos, mais as consultas que a falta de alimentação exigir. O fechamento fornece células, mas nesse ponto não fornece folgas nem a lista de vínculos à função do previsto. Custo incremental medido [nao verificado]. `app/ponto/portas/celula.py:293–306,655–659`, `app/ponto/services/fechamento.py:777–781`, `app/ponto/services/espelho.py:1169–1170`, `app/relatorios/pdf_espelho.py:405–407`, `app/ponto/motor_calculo_v2.py:848,1249–1277,2296`, `app/escala/utils.py:2354,2395–2399,2472–2485`.
   
   `MotorComercial` escreve `PeriodoCalculo.minutos_extra_50/100` por período. O split noturno, o banco e o DSR leem esses minutos. `ResultadoMes.horas_extra_50/100` são propriedades que somam `periodos` e dividem por 60. Leitores diretos incluem o fechamento, a sonda, a autorização de HE e o diagnóstico de extrato. O fechamento persiste as horas mensais e entrega os períodos à porta `dia_pago.lavrar`, que grava as horas diárias. TXT, cartão, relatórios e ranking leem essas projeções. A restauração histórica de HE também passa pela porta `restaurar_fechamento`. `app/ponto/motor_calculo_v2.py:324–344,1024–1038,3374–3381,3420–3427,3448–3459,3491–3505`, `app/ponto/services/fechamento.py:948–953,1173–1225,1237–1242`, `app/scripts/sonda_e1_comercial.py:82–86`, `app/ponto/services/autorizacao_he_periodo.py:46–48`, `app/relatorios/management/commands/diff_extrato_parcial.py:68–74`, `app/ponto/services/dia_pago.py:180–186,360`, `app/folha/export.py:427–455`, `app/relatorios/cartao_pela_celula.py:94–95,177–180`, `app/relatorios/views.py:529–530`, `app/folha/services/ranking_he.py:139–150`, `app/ponto/management/commands/aplicar_09_he_intra.py:137–152`.

5. **nota | BUG-HE-INTRA-DOBRADA, no ramo do limite diário.** O comercial grava o suprimido em `minutos_intrajornada_indenizada`. `_limite_com_suprimido` acrescenta ao teto `min(suprimido indenizado, intervalo declarado)` antes de subtrair o teto do trabalhado. Subir o teto reduz a HE. A indenização continua na rubrica própria do fechamento, do `DiaPago` e do TXT. Com 660 min previstos, 720 min trabalhados e 60 min suprimidos e declarados, saem zero HE e 60 min de intra. O helper também é chamado nos ramos de 12x36, turno partido e comercial por período. `app/ponto/motor_calculo_v2.py:1324–1352,1781–1819,2859,3163,3261–3275,3304,3413`, `app/ponto/services/fechamento.py:1037`, `app/ponto/services/dia_pago.py:186,225`, `app/folha/export.py:455`, `app/ponto/tests/test_he_intra_dobrada.py:77–104`.

6. **fila | Bordas de unidade e data.** A grade desconta **todas** as pausas, enquanto `intervalo_declarado_do_dia`, que limita quanto volta ao teto, lê só a primeira. A troca torna essa diferença relevante para o comercial com duas pausas. O vigia usa o mesmo valor da primeira pausa, portanto esse selo não distingue os dois valores. A dobra de feriado grava HE100 e sai antes do novo limite, inclusive antes da guarda de zero. Há ainda leitores por data civil da entrada no banco semanal, no DSR e no crédito de atraso, enquanto o teto diário e a sonda usam o dia atribuído à jornada. Efeito dessas bordas na frota [nao verificado]. `app/escala/utils.py:2434–2443,2507–2540`, `app/folha/porta_export.py:564–571`, `app/ponto/motor_calculo_v2.py:3342–3350,3370–3396,3409–3413,3448–3452,3491–3499`, `app/ponto/services/credito_parcial.py:34–49`, `app/scripts/sonda_e1_comercial.py:64–68`.

7. **nota | Quem escreve e quem lê a falta e `sem_celula`.** As portas de ausência criam, editam e decidem o registro. `Ausencia.save` deriva `minutos` quando uma falta aprovada chega sem valor. O filtro novo lê `versao_para` pela competência da empresa e cruza as datas com `dias_de_previsao_invalida`. Esse mapa só percorre células existentes elegíveis e delega a validade ao juiz. Em setembro, `versao_para` devolve regra 1, e o filtro exige regra 4. `app/ponto/services/ausencia.py:262,614,1057`, `app/ponto/models.py:878–897`, `app/escala/utils.py:456–498`, `app/ponto/services/cartorio.py:160,276–295`.
   
   - **Minutos e datas:** `folha.export` expande e rateia a ausência, depois filtra. O mapa de minutos alimenta `FechamentoMensal.horas_falta` e `DiaPago`; o de datas alimenta TXT, gates de exportação e a lista de faltas decididas do cartão. `app/folha/export.py:63–70,95–145,148–192,747–749,1020`, `app/ponto/services/fechamento.py:902–904,1036,1241`, `app/relatorios/cartao_pela_celula.py:373–374`.
   - **Overlay e palavra:** `_marcar_ausencias` escreve `tipo_dia`, `falta_aprovada` e `ausencia_id`. A palavra passa pela cobertura filtrada no espelho e no calendário, por `do_dia → rotulo_de → palavra_do_dia`, e chega ao PDF e à API. O valor da frase ainda vem de `Ausencia.minutos` do registro inteiro, enquanto a folha rateia por dia. `app/escala/utils.py:1365–1399`, `app/ponto/services/dia_decidido.py:254–256,442–451,494–497,554–566`, `app/ponto/services/espelho.py:334–346,889–892`, `app/colaboradores/services/calendario.py:570–571,751–760`, `app/relatorios/pdf_espelho.py:923–924`, `app/api/views.py:1394`.
   - **Assiduidade e crédito:** o comando lê a data inicial dos registros de ausência, filtra as faltas aprovadas recusadas e grava `ScoreColaborador`, lido pelas telas de inteligência. `creditos_por_dia` usa o mesmo mapa para créditos de ausências pagas parciais. Seus leitores incluem **o desconto de atraso do fechamento e do `DiaPago`**, além do subtotal do PDF. O teste novo cobre o mapa e o subtotal de uma declaração, sem aferir o desconto do fechamento. `app/inteligencia/management/commands/calcular_scores.py:7–21,98–119`, `app/inteligencia/views.py:328–352`, `app/ponto/services/credito_parcial.py:15–31`, `app/ponto/services/fechamento.py:1027–1035,1241–1242`, `app/ponto/services/dia_pago.py:189–193`, `app/ponto/services/efeito_ausencia.py:46,60`, `app/relatorios/pdf_espelho.py:336,479`, `app/escala/tests/test_e1_v5_leitores.py:46–53`.
   - **`sem_celula`:** `_dias_sem_decisao` produz o motivo quando falta célula em dia passado com vínculo, dentro das guardas da porta. Também usa o motivo para célula existente sem veredito. Leem-no a classificação do export, a prévia, Fora da Folha e o vigia de pausa. O teste fixa o caso de cadastro inválido sem célula, com mapa vazio, retenção `sem_celula` e os 480 min ainda no mapa de falta. `app/folha/export.py:975,1212–1220,1267–1287,1310–1322`, `app/folha/previa.py:170–171`, `app/folha/fora_da_folha.py:120–121`, `app/folha/services/vigia_pausa.py:125`, `app/escala/tests/test_e1_v5_leitores.py:82–99`.

8. **bloqueia | REDs, mutantes e ordem da trava.** Confirmei por importação local, sem banco, que o módulo escolhido pelo executor falha com `NameError: DATABASES is not defined` antes dos testes. O comando documentado também chama a suíte direto, sem a trava da L-074 para o banco compartilhado. Os asserts comerciais agora exigem `MotorComercial`, cobrem as duas batidas inválidas e preservam o contraexemplo válido com 2 h de HE50 e 1 h de HE100. Há asserts para `falta_aprovada`, transição, rateio e regra 1. Mordida executada na base, mutantes detectados e suíte inteira verde [nao verificado]. `app/scripts/test_e1_v5_variantes.py:1–6,30–73,118–125`, `app/config/settings_test.py:2–4`, `app/docs/LEIS.md:101`, `bin/suite.sh:343–373`, `app/escala/tests/test_e1_v4_juiz_unico.py:152–195,224–241,312–379`, `app/escala/tests/test_e1_v5_leitores.py:55–79`.

9. **nota | Segundo escritor e relógio.** Os hunks de produção acrescentam leituras e projeções em memória. Os campos persistidos continuam chegando pelas portas citadas. A sonda captura `agora` na entrada e o congela nas duas passadas, com a transação `REPEATABLE READ, READ ONLY` configurada antes das consultas. O filtro por competência recebe a data, sem relógio próprio. A retenção já tinha o default de `localdate()` quando o chamador omite `hoje`. `app/escala/utils.py:486–498`, `app/ponto/services/cartorio.py:276–295`, `app/ponto/services/fechamento.py:1164–1227`, `app/folha/export.py:1238–1246`, `app/scripts/sonda_e1_comercial.py:89–103`.

10. **bloqueia | A sonda mede o motor real. O zero informado pertence ao recorte dela.** As duas passadas chamam `recalcular_fechamento_mes(..., somente_leitura=True)`. A segunda enxerta só o bloco do limite em memória sobre a base. A sonda usa as propriedades reais de `ResultadoMes` e o juiz de dono do E6. Ela compara HE50/HE100 dos `periodos` comerciais na competência 11, por dia de jornada. O denominador inclui dias em que a fatia comercial responde mesmo sem período. A classificação chama `previsao_invalida_do_vinculo` e trata todo `None` como válido. Isso inclui casos sem previsão a julgar, e sua lista de recusados pode incluir template inválido sem célula, que fica fora do mapa de retenção V3. Assim, o código comprova o recorte de **HE diária bruta do motor**, sem comprovar equivalência exata com a população de dias efetivamente válidos do aval nem com as rubricas finais do fechamento. `app/scripts/sonda_e1_comercial.py:27–43,51–86,97–135`, `app/escala/alimentacao.py:411–424`, `app/escala/utils.py:391,407–432,473–483`, `app/ponto/services/fechamento.py:948–996`, `app/ponto/management/commands/e6_oraculo.py:182–195`.
    
    O dado fornecido informa `dias_validos=5314` e `validos_com_HE_diferente=0`. A cópia cita a medição no comentário do hunk. A saída por rubrica dos inválidos, a execução da sonda de intermitentes, o vínculo dos resultados ao hash e a publicação antes do pouso estão [nao verificado]. A sonda comercial exige o bloco antigo para reproduzir a comparação, e o procedimento exige suíte inteira verde antes do Opus final. **Veredito do pré-verificador: PAREI.** `app/ponto/motor_calculo_v2.py:3404–3408`, `app/scripts/sonda_e1_comercial.py:27–37,126–135`, `app/scripts/sonda_e1_intermitente.py:66–107`, `CLAUDE.md:392–402`.

## 26/11 10:17 · sessao d2b358f477f5 · R1-REMEDE-2411 · leitura · origem=Code

## 1. Dias ESTRUTURA que saíram, entraram e ficaram

| Comp. | Saíram | Entraram | Ficaram |
|---|---|---|---|
| **10** | **0** | **2** — col348 11/10, col382 21/09 | **26**, listados por classe abaixo. `antes_c10.txt:12–37`, `depois_c10.txt:12–39` |
| **11** | **3** — col334 16/11, col950 11/11, col1044 13/11 | **4** — col200 03/11, col382 11/11, col922 29/10, col1010 05/11 | **34**, listados por classe abaixo. `antes_c11.txt:12–48`, `depois_c11.txt:12–49` |

A chave é `colab + dia`. As datas são de 2026. A ausência de uma chave na lista ESTRUTURA deixa em aberto se ela bateu, mudou de dono ou deixou de ser comparada. O comando tem caminhos distintos para esses resultados. `app/ponto/management/commands/e6_oraculo.py:331–381`

## 2. Atribuição por entrada e saída

A cópia veio sem metadados `.git`. A verificação via `git show` ficou **[nao verificado]**. Comparei as versões locais disponíveis do código e usei os registros versionados para identificar os pousos. `pousos.txt:2–7`, `pousos.txt:12–14`, `app/docs/RELATO.md:13`, `:39–44`, `:75`, `:143`, `:157`

| Comp. | Movimento | Colab e dia | Atribuição |
|---|---|---|---|
| 10 | Entrou | **col348 11/10** | **Não sei.** O depois mostra espelho `0`, E6 `665` e `folga`. A documentação da A-TELA registra que a obra seguiu sem o caso do col348. A entrada desse dia por código ou dado está **[nao verificado]**. `depois_c10.txt:24`, `app/docs/RELATO.md:273`, `app/ponto/services/cartorio.py:1118–1126` |
| 10 | Entrou | **col382 21/09** | **Não sei.** O delta de `+389` min coincide com o caso de atribuição da madrugada de 21/09 ao plantão de 20/09, já registrado na O76 e na O215. Isso identifica a família do caso. O efeito de um pouso ou do apply da 10 sobre a entrada está **[nao verificado]**. `depois_c10.txt:26`, `app/docs/BACKLOG.md:1494`, `:1598`, `:2014` |
| 11 | Saiu | **col334 16/11** | **Não sei.** O antes mostra espelho `209`, E6 `591–683` e `folga`. O resultado desse dia na segunda sombra está **[nao verificado]**. `antes_c11.txt:34`, `depois_c11.txt:12–49` |
| 11 | Saiu | **col950 11/11** | **Não sei.** O antes mostra `480 × 240`. O registro identifica col950 como colaborador de teste. A razão da saída e o resultado posterior estão **[nao verificado]**. `antes_c11.txt:44`, `depois_c11.txt:12–49`, `app/docs/RELATO.md:271` |
| 11 | Saiu | **col1044 13/11** | **Não sei.** O antes mostra `204 × 135–339`, com delta `−69`. O resultado desse dia na segunda sombra está **[nao verificado]**. `antes_c11.txt:48`, `depois_c11.txt:12–49` |
| 11 | Entrou | **col200 03/11** | **Não sei.** O depois mostra `420 × 598–720`, com oito batidas. O registro da NOTURNO-A menciona um par civil de 03/11 cujo dia do juiz é **02/11**. A ligação desse par com a entrada de **03/11**, ou com a COMP11-RELAVRA, está **[nao verificado]**. `depois_c11.txt:23`, `app/docs/RELATO.md:189`, `app/docs/BACKLOG.md:2304` |
| 11 | Entrou | **col382 11/11** | **Não sei.** O depois mostra `437 × 854–949`. A relavra posterior à NOTURNO-A inclui o col382, mas o registro disponível vai até o nível do colaborador. O efeito em **11/11** está **[nao verificado]**. `depois_c11.txt:36`, `app/docs/BACKLOG.md:2303`, `app/docs/RELATO.md:6070–6076` |
| 11 | Entrou | **col922 29/10** | **NOTURNO-A é a explicação sustentada para o par desse dia.** Ao ancorar os marcos no dia do turno, a L-084 libera o par cru. O diff documenta **+174,6 min** no motor para a chave exata e o anexo mede **−175 min** entre piso E6 e espelho. O registro também aponta a divergência entre 29/10 no juiz e 27/10 no mapa do motor. A transição de dono no R1 está **[nao verificado]**. `depois_c11.txt:43`, `app/ponto/motor_calculo_v2.py:1542–1588`, `app/docs/RELATO.md:194`, `:205` |
| 11 | Entrou | **col1010 05/11** | **NOTURNO-A é a explicação sustentada para o par desse dia.** O teste da chave exata põe o marco de saída em D+1 e aplica a L-084 ao par de madrugada, que passa a contar cru. O diff documenta **+294,8 min** no motor e o anexo mede **−295 min** entre piso E6 e espelho. A transição de dono no R1 está **[nao verificado]**. `depois_c11.txt:49`, `app/ponto/janela_he.py:41–64`, `app/ponto/tests/test_noturno_v4_janela_dia_do_turno.py:93–107`, `app/docs/RELATO.md:205` |

Os dois deltas da NOTURNO-A são de **minutos trabalhados do motor**. O E6 prioriza `minutos_realizados`, que o espelho calcula pelos turnos do juiz. O comando usa `pago_h` quando esse campo falta e também lê a lista de Cadastro × Realidade do motor para atribuir o dono. Os anexos não trazem a fonte do valor nem o dono anterior dos dias que entraram, por isso a correspondência dos minutos sustenta o mecanismo sem fechar a transição. `app/ponto/services/espelho.py:267–294`, `:689–690`, `:801–830`, `app/ponto/management/commands/e6_oraculo.py:162–172`, `:182–195`, `:263–269`

- **A-TELA v18** mudou a contagem de protestos e carimbos e a ordem entre a escrita de log e a aplicação da precedência. **A4** mudou a palavra de espera da ausência e a oferta de anexo na API. Efeito em uma das nove chaves **[nao verificado]**. `app/ponto/services/cartorio.py:1118–1129`, `:1925–1934`, `:2107–2134`, `app/ponto/services/dia_decidido.py:261–272`, `:474–479`, `app/api/views.py:1061`
- **PARCIAL-BORDA v2** mudou o julgamento de batidas na borda da ausência parcial pela direção. O caso com mudança registrado na prova é col274 04/10. **PONTA-PEQUENA** mudou o prazo de HE no vigia, que percorre hoje e ontem. Efeito de qualquer um dos dois nas nove chaves **[nao verificado]**. `app/ponto/turnos.py:2663–2694`, `app/escala/utils.py:1271–1284`, `app/docs/RELATO.md:13242–13250`, `app/ponto/services/vigia_de_hora.py:22–30`, `:87–124`
- **COMP11-RELAVRA** tem apply de frota registrado em 24/11, das 23:40 às 23:45. O `recalcular_fechamento_mes` chama a lavra de `DiaPago`. O registro agregado e os anexos deixam **[nao verificado]** o vínculo do apply com cada chave, assim como eventual mudança de batida ou cadastro entre as sombras. `app/docs/BACKLOG.md:2304`, `app/docs/RELATO.md:173–174`, `app/ponto/services/fechamento.py:1224`, `app/ponto/services/dia_pago.py:359–360`, `antes_c11.txt:12–48`, `depois_c11.txt:12–49`

## 3. Dias que ficaram ou entraram, por classe e obra

**`+` indica entrada.** As classes abaixo são as do anexo. O E6 classifica pelos minutos antes de arredondar o delta, por isso um `−10` impresso pode aparecer em `diverge_10_60`. O sufixo `/FORA` indica a posição do espelho em relação à faixa do oráculo. `app/ponto/management/commands/e6_oraculo.py:362–372`, `:393–404`

| Classe | Comp. 10 | Comp. 11 |
|---|---|---|
| **`e6_zero_esp_trabalho`** — E6 zero, espelho positivo | **6** — col37 05/10, 09/10; col218 15/10; col382 30/09; col515 09/10, 11/10. `depois_c10.txt:12–15`, `:27`, `:31–32` | **13** — col37 14/11; col110 12/11; col113 30/10, 01/11; col200 29/10, 02/11; col218 14/11; col296 02/11; col302 20/11; col382 10/11; col516 23/10; col727 10/11; col841 16/11. `depois_c11.txt:12–39` |
| **`esp_zero_e6_trabalho`** — espelho zero, E6 positivo | **3** — col129 12/10; **+col348 11/10**; col868 22/09. `depois_c10.txt:14`, `:24`, `:38` | **1** — col996 28/10. `depois_c11.txt:47` |
| **`diverge_10_60`** — diferença acima de 10 até 60 min | **12** — col252 23/09, 24/09, 25/09, 28/09, 01/10, 02/10, 06/10, 09/10; col369 12/10; col451 23/09, 25/09; col864 12/10. `depois_c10.txt:16–23`, `:25`, `:28–29`, `:37` | **10** — col114 08/11, 15/11; col252 09/11, 10/11, 11/11, 13/11, 17/11, 18/11, 19/11; col884 08/11. `depois_c11.txt:18–19`, `:26–32`, `:41` |
| **`diverge_acima_60`** — diferença acima de 60 min | **7** — **+col382 21/09**; col478 25/09; col723 08/10; col746 24/09; col841 20/10; col854 04/10; col876 21/09. `depois_c10.txt:26`, `:30`, `:33–36`, `:39` | **13** — col112 21/10, 22/10; col130 29/10; **+col200 03/11**; col220 10/11; **+col382 11/11**; col874 22/10; **+col922 29/10**; col922 01/11, 04/11; col989 23/10; col996 10/11; **+col1010 05/11**. `depois_c11.txt:14–15`, `:20`, `:23`, `:25`, `:36`, `:40`, `:43–46`, `:48–49` |
| **`diverge_acima_60/FORA`** | **0**. `depois_c10.txt:12–39` | **1** — col909 11/11, com `CORTE_do_oraculo`. `depois_c11.txt:42` |

**Obras donas e lacunas por classe**

- **`e6_zero_esp_trabalho`** — **MOTOR-LE-O-TURNO** assume **12 dos 13 dias da comp 11**. O BACKLOG enumera 11 chaves em que E6 e pareador dão vazio e o espelho tem minutos, mais col841 16/11 como caso de chave do dia da jornada. **Col302 20/11 e os seis dias da comp 10 ficam sem obra dona confirmada para a divergência R1.** A **R1-A-i-FILTRO-DATA-CIVIL** cita col218 15/10 e os mesmos 105 min, mas a intervenção registrada é no filtro do motor. `app/docs/BACKLOG.md:2234`, `:1830`, `depois_c10.txt:12–32`, `depois_c11.txt:34`
- **`esp_zero_e6_trabalho`** — **O54 ESPELHO-VERDADE, E1-sem-previsão**, acompanha o contador. **E6-CAUDA-1** fechou o defeito histórico que lia `None` como zero. **Os quatro dias atuais ficam sem obra dona confirmada por chave.** A fila do intermitente cita col348, sem identificar a causa dos 665 min de 11/10. `app/docs/BACKLOG.md:1469`, `:1303`, `:2272`, `app/core/espelho_verdade.py:60–61`, `app/ponto/management/commands/e6_oraculo.py:263–269`
- **`diverge_10_60`** — **Sem obra dona da classe inteira.** **R1-ESTRUTURA-91** registra as assinaturas `+46` e `−60` para investigação por família. Para **col252**, a família documentada é pausa curta abaixo do piso com intervalo pré-assinalado, em **ORACULO-E6-43-DEFEITOS**, registrada no BACKLOG por **EV-NA-BRECHA-1611**. O RELATO explicita 09, 10, 11, 13 e 17/11 no censo de intervalo curto. A extensão dessa causa às outras datas do col252 está **[nao verificado]**. **Col114, col369, col451, col864 e col884 ficam sem obra dona confirmada para essas chaves.** `app/docs/BACKLOG.md:1896`, `:823`, `app/docs/GRADE-MESTRA.md:303`, `app/docs/RELATO.md:2390`, `:3096–3097`
- **`diverge_acima_60`** — **E6-CAUDA-2** registrou a classe e a redirecionou ao **PLACAR-ESTRUTURAL**. A classe agrega causas diferentes. **O76 / O215** documentam a atribuição de dia de **col382 21/09**. **ATA-X-TURNO-PAUSA** nomeia **col841 20/10**. **NOTURNO-A-JANELA-NO-DIA-DO-TURNO** é dona do mecanismo de janela nos pares de **col922 29/10** e **col1010 05/11**. **EV-NA-BRECHA-1611 / O54 E3-geometria** tem o diagnóstico de soma versus exibição para **col922 01/11 e 04/11**, registrado como a confirmar. **Os demais dias da classe ficam sem obra dona confirmada.** `app/docs/BACKLOG.md:1301–1302`, `:1494`, `:1598`, `:1859`, `:1989`, `:823`, `:1469`, `app/docs/RELATO.md:205`, `:3097`
- **`diverge_acima_60/FORA`** — **ORACULO-E6-43-DEFEITOS**, família **corte por vão**, cobre o tipo de defeito assinalado em **col909 11/11**. O código marca o corte que deixa um trecho ímpar. O anexo identifica esse corte, e a correção do delta de `−586` após tratá-lo está **[nao verificado]**. `depois_c11.txt:42`, `app/docs/BACKLOG.md:823`, `app/docs/GRADE-MESTRA.md:303`, `app/ponto/calculador/nucleo.py:215–225`

## 4. AUTORIDADE por dono

Cada célula mostra **antes → depois (delta)**. As horas são as publicadas pelos anexos. `antes_c10.txt:6–11`, `depois_c10.txt:6–11`, `antes_c11.txt:6–11`, `depois_c11.txt:6–11` [nao verificado]

| Comp. | Dono | Dias-colab | Horas | Colabs | Fonte dos anexos |
|---|---|---:|---:|---:|---|
| 10 | ESTRUTURA | 26 → 28 (**+2**) | 88,4 → 105,9 (+17,5) | 16 → 17 (+1) | `antes_c10.txt:6`, `depois_c10.txt:6` |
| 10 | CADASTRO | 75 → 74 (**−1**) | 295,0 → 288,5 (−6,5) | 31 → 30 (−1) | `antes_c10.txt:7`, `depois_c10.txt:7` |
| 10 | BATIDA | 217 → 210 (**−7**) | 1.021,0 → 992,6 (−28,4) | 117 → 119 (+2) | `antes_c10.txt:8`, `depois_c10.txt:8` |
| 10 | **SOMA** | **318 → 312 (−6)** | | | `antes_c10.txt:9–11`, `depois_c10.txt:9–11` |
| 11 | ESTRUTURA | 37 → 38 (**+1**) | 152,9 → 159,1 (+6,2) | 25 → 23 (−2) | `antes_c11.txt:6`, `depois_c11.txt:6` |
| 11 | CADASTRO | 79 → 77 (**−2**) | 315,3 → 305,6 (−9,7) | 37 → 33 (−4) | `antes_c11.txt:7`, `depois_c11.txt:7` |
| 11 | BATIDA | 275 → 254 (**−21**) | 1.473,0 → 1.401,7 (−71,3) | 129 → 126 (−3) | `antes_c11.txt:8`, `depois_c11.txt:8` |
| 11 | **SOMA** | **391 → 369 (−22)** | | | `antes_c11.txt:9–11`, `depois_c11.txt:9–11` |

Nos quatro anexos, a soma por dono confere com `AUTORIDADE SOMA`, com a soma das quatro classes e com `CSV_POR_DONO`. A contagem nominal ESTRUTURA fecha em **26 + 2 = 28** na comp 10 e **37 − 3 + 4 = 38** na comp 11. `antes_c10.txt:1–37`, `depois_c10.txt:1–39`, `antes_c11.txt:1–48`, `depois_c11.txt:1–49`

## 26/11 10:16 · sessao 6d378f7647cc · DIA-DA-BATIDA-JUIZ-UNICO · sonda · origem=Code

```text
rc_sombra=125
```

O Docker falhou ao montar `/app/logs` antes de iniciar o script. O envelope cria arquivos temporários, então a regra de somente leitura impediu uma nova execução. [bin/sonda_leitura.sh:203](bin/sonda_leitura.sh#L203) · [bin/sonda_leitura.sh:216](bin/sonda_leitura.sh#L216) · [bin/sonda_leitura.sh:235](bin/sonda_leitura.sh#L235)

`universo_colabs`, `pks_na_comp`, `pks_com_dia_diferente`, `colabs_afetados`, `delta`, `top_colabs` e `exemplos`: [nao verificado]

Se o novo `pks_com_dia_diferente` é `0`: [nao verificado]

## 26/11 10:16 · sessao cf3d0dd25532 · O30-F3-CONSTRUIR · verificador · origem=Code

1. **bloqueia | O30-F3 c3.** A porta passou a travar antes de decidir e a reler antes de rederivar. Ainda há divergência entre quem grava e quem serve o anexo em cadeia legada, uma irmã aberta consegue trocar o comprovante compartilhado por outra já aprovada, e o selo aceita formas indiretas de gravação que deveria acusar. `app/ponto/services/ausencia.py:283–305,312–314,1161–1183,1228–1237,1919–1939`, `app/ponto/views.py:2144–2154`, `app/ponto/tests/test_atestado_f3_contrato.py:65–107`.

2. **nota | C1 procedente na c2, curado nos dois caminhos apontados.** `decidir` trava a raiz e relê a linha antes da transição, mesmo quando a aprovação em grupo itera pela filha mais recente. O H06 faz a leitura inicial e chama a mesma trava antes de conferir a identidade. `of=('self',)` restringe a trava a `Ausencia`, inclusive onde o `ordering` inclui `colaborador__nome`. Os REDs registraram `[raiz, filha, raiz]` para o grupo e `[raiz, filha]` para H06. Os dois mutantes falharam. `app/ponto/services/ausencia.py:294–299,1228–1237`, `app/ponto/views.py:2786–2815`, `app/ponto/models.py:843`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:135–150`, `app/ponto/tests/test_atestado_f3_contrato.py:401–434`, `app/ponto/tests/mutantes_atestado_f3.py:140–149`.

3. **bloqueia | C2 procedente, cura parcial.** Criar grava a raiz real no `INSERT`. Reabrir e anexar achatam a referência da linha tratada. A entrega por ID, o drawer e o agrupamento de aprovação e rejeição ainda seguem um elo. Reproduzi em memória `C → B → A` com arquivo só em A. O serviço resolveu A e a entrega por C retornou 404. O lote parte de A, pois seleciona somente referências nulas. Os três mutantes morderam, mas o RED que compara os bytes da entrega e do ZIP só serve C **depois** de achatar a FK. A sonda fornecida informa 5.218 ausências, duas referências, zero cadeias de profundidade ≥ 2 e zero raízes com duas ou mais filhas em espera. Confirmação em produção [nao verificado]. `app/ponto/services/ausencia.py:283–305,559–576,645–663,1171–1183,1456–1474`, `app/ponto/views.py:2100–2109,2144–2154,2786–2790,2861–2866`, `app/ponto/services/dia_decidido.py:624–630`, `app/ponto/tests/test_atestado_f3_contrato.py:436–487`, `app/ponto/tests/mutantes_atestado_f3.py:150–157`.

4. **nota | C3 procedente, curado na origem quanto à instância velha.** `rederivar_fim` relê sob trava antes de calcular e salvar. O comando usa o retorno da porta e pula o registro que já ganhou fim, foi rejeitado ou deixou de ter fim derivável. A cerca inclui o último dia proposto e consulta o K6 registrado pela composição existente de janela da empresa e TXT vigente. Sem releitura, o RED gravou `07/09, 180 min` em vez de `28/09, 420 min`. Cercar só o início deixou o `save` ocorrer no segundo mutante. A cerca consulta o TXT naquele momento. Serialização com uma exportação concorrente [nao verificado]. A ordem legada do `save` ainda calcula minutos de falta aprovada com `minutos=0` antes de preencher o fim, caso já caracterizado em teste próprio. `app/ponto/services/ausencia.py:1919–1952`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29–53`, `app/core/juizes.py:340–349`, `app/chamados/services/vigia_fio.py:814–836`, `app/folha/export.py:580–594`, `app/folha/views.py:297–302`, `app/ponto/models.py:898–932`, `app/ponto/tests/test_atestado_f3_contrato.py:523–541`, `app/ponto/tests/mutantes_atestado_f3.py:158–163`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:618–634`.

5. **bloqueia | C4 procedente, cura parcial.** A troca na **linha solicitada** recém-aprovada retorna 409 após a releitura sob trava. A primeira entrega na aprovada sem arquivo preserva o status. O anexo e o reenvio com arquivo na raiz liberam as irmãs em espera por `mudar_estado`. Os cinco mutantes morderam. Resta o caso que reproduzi em memória com a porta real e ORM simulado: raiz aberta com arquivo, uma filha aprovada e outra aberta. O upload próprio pela aberta substitui o arquivo que a aprovada também lê. A aprovação por IDs pode decidir uma filha isoladamente, e as guardas do upload consultam os estados da linha solicitada e da raiz. `app/api/views.py:3093–3101,3126–3132`, `app/ponto/services/ausencia.py:312–314,468–476,1118–1132,1161–1183`, `app/ponto/views.py:2146–2154,2392–2403`, `app/ponto/tests/test_atestado_f3_contrato.py:571–628`, `app/ponto/tests/mutantes_atestado_f3.py:164–172`.

6. **bloqueia | C5 procedente, curado para os exemplos do v2.** `campos = None`, alias de `None` e parâmetro sem atribuição são acusados. Os dois mutantes do `save` falharam no selo. Confirmei dois casos que ainda dão **zero achados**: `campos = ['status']; campos.append('atestado_cid'); ausencia.save(update_fields=campos)` e um parâmetro `campos` que recebe `['status']` só dentro de um `if`. O selo olha atribuições anteriores pela posição no fonte. A mutação da lista e o caminho em que o `if` não roda escapam. Isso é lacuna de prova. O censo da porta passou na cópia. `app/ponto/tests/test_atestado_f3_contrato.py:65–107,162–184,232–261`, `app/ponto/tests/mutantes_atestado_f3.py:173–176`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:29–57`.

7. **nota | C6 procedente na c2, curado no lote.** A view lê `timezone.localdate()` uma vez, preenche mês e ano ausentes ou vazios a partir da mesma data e repassa os inteiros. O serviço usa o par explícito ou recebe `de` e `até`. Os dois mutantes morderam, um pelo argumento errado na virada do ano e outro pela leitura indevida do relógio. `app/ponto/views_atestado.py:49–67,103–117`, `app/ponto/services/atestados_lote.py:33–45`, `app/ponto/tests/test_atestado_f3_contrato.py:655–688`, `app/ponto/tests/mutantes_atestado_f3.py:177–185`.

8. **bloqueia | Classes abertas ou ampliadas pelo delta.** A raiz transitiva virou fonte para as portas, mas a entrega ainda lê o elo imediato. Há outra divergência concreta no comando de rejulgamento: ele decide se chama a porta com `bool(x.documento)` e pode pular a filha cuja raiz já tem arquivo. A cerca de C3 também passou a calcular `fim_proposto` pela função da porta e deixar `save` derivar o fim de novo. Os dois cálculos coincidem no recorte atual da cura, mas são dois pontos para responder qual fim será gravado. O lote delega a janela e o histórico, e K6 é reutilizado. O `TIPOS_EXIGEM_DOC` cru do gate continua na fila. `app/ponto/services/ausencia.py:198–210,283–291,790–809,874–893,1922–1939`, `app/ponto/models.py:915–932`, `app/ponto/views.py:2144–2154`, `app/ponto/management/commands/rejulgar_status_por_tipo.py:25–37`, `app/ponto/services/atestados_lote.py:27–74`, `app/core/juizes.py:259–264,340–349`.

9. **fila | Restante do checklist.** O relógio saiu do serviço do lote. A criação com referência acrescentou uma segunda chamada possível a `localdate()` para recalcular o prazo sob trava. A reabertura ainda lê o próprio relógio. A trava nova segue raiz → filha → demais irmãs, com `of=('self',)`. Fora desse caminho, o cron de prazo ainda lê a fila e chama `mudar_estado` sem reler sob trava. O censo de produção da porta passou e o delta mantém os `save` e `update` de `Ausencia` na porta. Fora do fluxo operacional, `SET_NULL` altera a FK na exclusão, a migração 0057 escreveu status e a máscara da sombra reescreve `documento` na cópia. `app/ponto/services/ausencia.py:294–305,562–564,645–655,1118–1132,1323–1338,1483–1485`, `app/ponto/models.py:815–819`, `app/ponto/migrations/0057_a4_backfill_pendente.py:35–43,58–65`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:29–57`, `bin/sombra_regras_pessoais.py:56–65`, `bin/sombra_mascara_pessoal.py:16–26`.

10. **nota | Censo de referência e anexo.**
    - **`documento_referencia` / raiz.** Escrevem `criar_ausencia` no `INSERT` e `_fixar_raiz_documento`, chamado no anexo, na liberação das irmãs e na reabertura. A exclusão da referência aciona `SET_NULL`. Leem a resolução/trava da porta, edição, anexo, rejulgamento e gate, além de drawer, entrega protegida, grupos de decisão, listagem, histórico e lote. `app/ponto/services/ausencia.py:283–305,559–569,645–656,729–772,790–809,862–892,1118–1132,1161–1173,1456–1465`, `app/ponto/models.py:815–819`, `app/ponto/views.py:2100–2109,2144–2154,2786–2790,2861–2866`, `app/ponto/views_atestado.py:29–31`, `app/templates/ponto/ausencias.html:168–171`, `app/ponto/services/dia_decidido.py:624–630`, `app/ponto/services/atestados_lote.py:48–56`.
    - **`documento`.** Criar grava o arquivo próprio na raiz nova ou encaminha para `anexar_documento` se houver referência. Anexar e `_reabrir` gravam na raiz, inclusive quando a entrada é `reverter_rejeicao(documento=...)`. Leem as guardas e o estado inicial, drawer, entrega por ID, mídia por caminho, histórico e lote. Fila do app, abertura do chamado, cartão do dia e troca de plantão ainda consultam o campo próprio. O CSV consome o indicador, o PDF lê imagem ou monta link protegido, e o ZIP lê os bytes. `app/ponto/services/ausencia.py:559–577,645–666,908–935,1049–1057,1164–1183,1431–1435,1463–1475`, `app/ponto/views.py:2144–2159`, `app/core/media_protegida.py:18–23`, `app/templates/ponto/partials/painel_ausencia.html:68–95`, `app/ponto/services/dia_decidido.py:546–555,671–685`, `app/ponto/services/atestados_lote.py:134–149,173–193,218–225,247–269`, `app/api/views.py:1053–1067`, `app/ponto/portas/celula.py:719–746`.

11. **nota | Censo de fim, minutos e estado.**
    - **`data_fim`.** Criar e editar fornecem o fim. `Ausencia.save` completa ou valida o valor. `mudar_estado` e `rederivar_fim` persistem a derivação. H06 e o curador leem a linha e delegam a escrita. Leem também cobertura e vigência, sinais do cartório, rateio da folha, lote, histórico e relatório anual. `app/ponto/services/ausencia.py:418–420,565–569,735–768,1093–1101,1922–1939`, `app/ponto/models.py:912–932`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137–169`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29–52`, `app/ponto/turnos.py:2540–2552,2822–2867,3207–3231`, `app/ponto/signals.py:115–128,143–147`, `app/folha/export.py:113–143`, `app/ponto/services/atestados_lote.py:48–60,140–149`, `app/ponto/services/dia_decidido.py:678–685`, `app/relatorios/views.py:275–292`.
    - **`minutos`.** Criar e editar recebem a duração. O modelo deriva o valor da falta aprovada sem minutos. Transição e rederivação persistem o resultado. Leem H06, juiz de parcial, rateio e crédito da folha, fechamento/DiaPago, API, admin, histórico e CSV do lote. `app/ponto/services/ausencia.py:565–567,763–769,1093–1101,1939`, `app/ponto/models.py:898–911`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:146–169`, `app/ponto/turnos.py:2602–2619`, `app/folha/export.py:113–143`, `app/ponto/services/credito_parcial.py:15–29`, `app/ponto/services/fechamento.py:893–895,1224–1230`, `app/api/views.py:1870–1880`, `app/ponto/admin.py:38–53`, `app/ponto/services/dia_decidido.py:570–583`, `app/ponto/services/atestados_lote.py:141–149`.
    - **`status`.** Criar grava o inicial ou o informado. `mudar_estado` escreve as transições de decisão, anexo, reabertura, prazo e rejulgamento. A rejeição de férias derivadas tem escrita própria dentro da mesma porta. Leem as guardas das portas, comandos de cura e H06, views de decisão, API, admin, histórico, lote, cobertura da folha e cobrança, e relatório anual. H06 e E4b ainda leem os campos concretos em bloco. `app/ponto/services/ausencia.py:562–575,790–809,1068–1111,1118–1132,1164–1170,1240–1245,1323–1338,1456–1485,1648–1667,1925–1927`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29–34`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:93–94,137–150,164–174`, `app/ponto/views.py:2392–2403,2777–2790`, `app/api/views.py:1870–1879`, `app/ponto/admin.py:45–46`, `app/ponto/services/dia_decidido.py:570–583`, `app/ponto/services/atestados_lote.py:48–60,140–149,207–210`, `app/ponto/turnos.py:2528–2552`, `app/relatorios/views.py:275–292`, `app/chamados/services/foto_e4b.py:247–252,284–288`.

12. **fila | Mês, ano e semântica dos campos.** No lote, o GET fornece `mes` e `ano`, a view preenche os vazios e o formulário valida. O serviço e `janela_fechamento` leem o par como **mês e ano do fim da competência**. O valor padrão vem do mês civil da data local. Assim, com corte 21, em 25/11 a opção padrão é 11, embora a competência que contém esse dia seja 12. `de` e `até` prevalecem quando ambos chegam e o recorte é inclusivo. `data_fim` é uma data inclusiva e `NULL` tem os sentidos legados de dia único ou período aberto conforme o tipo. `minutos` é duração inteira, com zero usado como sinal de dia inteiro na entrada e com falta aprovada derivada da grade. O período parcial que cruza meia-noite pertence ao dia de início do turno. O CSV formata `decidido_em` sem conversão local explícita. `app/ponto/views_atestado.py:49–67,103–120`, `app/ponto/services/atestados_lote.py:33–60,145–149`, `app/templates/relatorios/atestados_lote.html:6–24,31–37`, `app/ponto/janelas.py:10–33,54–63`, `app/ponto/models.py:750–771,898–932`, `app/ponto/turnos.py:2851–2867`, `app/ponto/services/ausencia.py:213–219,1193–1195`, `app/ponto/services/dia_decidido.py:558–567`.

13. **bloqueia | Entrega tardia é regra nova do endpoint próprio.** O brief da c3 pede explicitamente a exceção como decisão contestável. O registro escrito anterior da porta própria manda devolver 409 para ausência decidida e atribui o anexo ao DP. A view do DP já registra que o atestado pode chegar depois da aprovação, e o reenvio do mesmo fato já aceitava preencher arquivo faltante. Isso explica os precedentes. Não achei lei ou corte anterior que estenda essa permissão ao **endpoint próprio** depois da aprovação. A alteração passa a aceitar a primeira entrega em linha aprovada sem janela de prazo e sem reabrir a decisão. `app/docs/RELATO.md:7112`, `app/ponto/views.py:2166–2186`, `app/ponto/services/ausencia.py:1164–1178,1323–1338`, `app/api/views.py:3082–3089,3126–3132`, `app/docs/CORTES.md:348`.

14. **fila | Passivos e alcance da prova.** Permanecem o nome do arquivo na trilha e na ficha admin, o storage compartilhado, a vigência aberta anterior ao recorte, o gate cru, o JSON E4b, o fuso de `decidido_em` e o prazo de reabrir. A máscara da sombra segue pendente para a sessão. Hoje marca o caminho do arquivo e deixa os seis campos estruturados fora da máscara e do padrão do selo. Rodei 35 testes F3 sem banco, os dois selos estruturais de ausência e os 16 mutantes novos, todos com a falha esperada nos alvos. Hash Git, REDs contra o commit c2 original, concorrência PostgreSQL, integração e tempo do PDF real com 20 fotos [nao verificado]. `app/ponto/services/ausencia.py:887–892,1179–1181,1483–1485`, `app/ponto/admin.py:38–53`, `app/ponto/models.py:777–780`, `app/core/media_protegida.py:18–23,50–56`, `app/ponto/services/atestados_lote.py:48–60,149`, `app/ponto/turnos.py:3207–3226`, `app/chamados/services/foto_e4b.py:247–252,284–288`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:355–373`, `bin/sombra_regras_pessoais.py:48–78,160–164`, `app/ponto/tests/test_atestado_f3_contrato.py:115–184,232–688`, `app/ponto/tests/test_chokepoint_ausencia_gate.py:51–87`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:296–318`, `app/ponto/tests/mutantes_atestado_f3.py:140–185,268–296`, `app/ponto/tests/test_atestado_f3.py:330–351`.

## 26/11 10:13 · sessao 2246d4b83412 · DIA-DA-BATIDA-JUIZ-UNICO · construtor · origem=Code

A cura está em `ponto/turnos.py:3013`. A chamada avulsa e `CargaDaJanela` alimentam o mesmo pareador com o histórico apurável do colaborador. O pad da janela pedida seleciona as PKs apresentadas. O adaptador unitário continua consultando o mesmo mapa por PK. `ponto/turnos.py:1923`, `ponto/turnos.py:2951`, `ponto/turnos.py:2983`, `ponto/turnos.py:3154`

Usei o histórico completo porque uma sequência de saídas órfãs pode propagar a decisão da véspera por vários dias. Retirei o parâmetro `turnos=` da chamada avulsa, pois ele admitia turnos já recortados pelo chamador. A catraca de projeções civis perdeu os dois lookups removidos e passou a registrar a projeção civil no carregador. **Zero `except` novos**, incluindo os testes. `ponto/turnos.py:1253`, `ponto/turnos.py:1295`, `ponto/turnos.py:2951`, `ponto/turnos.py:3026`, `ponto/tests/test_contract_no_batida_date.py:472`, `ponto/tests/test_dia_das_batidas_estavel_na_janela.py:51`

### RED → GREEN em memória

| Caso | Base | Cura |
|---|---|---|
| Par de 24h10 sem marcos | A saída recebia **25/10** nas duas janelas curtas e **23/10** nas demais. | **23/10** nas quatro janelas e na consulta unitária. `ponto/tests/test_dia_das_batidas_estavel_na_janela.py:61` |
| Série de saídas órfãs | A última saída recebia **24/10** em `D−1..D+1` e **25/10** nas demais. | **25/10** nas quatro janelas e na consulta unitária. `ponto/tests/test_dia_das_batidas_estavel_na_janela.py:66` |
| Virada comum | **24/10** nas janelas do controle. | Conservou **24/10**, inclusive para a saída apresentada no pad à direita. `ponto/tests/test_dia_das_batidas_estavel_na_janela.py:74` |

Os três casos passaram com o pareador real e a carga simulada em memória. A paridade compara o **mapa inteiro** do lote com o da casca. Restaurar a casca F1 derrubou os dois REDs, restaurar o lote F1 derrubou a paridade nos dois, publicar só o dia civil derrubou o par de 24h10 e remover o recorte vazou três PKs no caso das órfãs. Django com banco e a suíte do Code ficam [nao verificado]. `ponto/tests/test_dia_das_batidas_estavel_na_janela.py:51`, `ponto/tests/test_dia_das_batidas_estavel_na_janela.py:72`

### Comandos para o Code

`BASE_TURNOS` aponta para `ponto/turnos.py` da cópia em `8837e24587cf521da533c2d579db3fd1f7c0ad41`. Os mutantes abaixo atuam em memória. `ponto/turnos.py:2951`, `ponto/turnos.py:3001`, `ponto/turnos.py:3013`, `ponto/turnos.py:3154`

```bash
modulo=ponto.tests.test_dia_das_batidas_estavel_na_janela
classe="$modulo.DiaDaBatidaEstavelNaJanelaTest"
par="$classe.test_par_de_24h10_sem_marcos_tem_um_dia_so"
orfas="$classe.test_serie_de_saidas_orfas_tem_um_dia_so"

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test "$modulo"

mutante() {
    MUTANTE="$1" BASE_TURNOS="${BASE_TURNOS:?}" PYTHONDONTWRITEBYTECODE=1 \
        python3 -B - "${@:2}" <<'PY'
import inspect
import os
import runpy
import sys

import django
from django.core.management import execute_from_command_line

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.saas")
django.setup()
from ponto import turnos

modo = os.environ["MUTANTE"]
if modo in {"borda", "lote"}:
    base = runpy.run_path(os.environ["BASE_TURNOS"])
    nome = {"borda": "dia_das_batidas", "lote": "CargaDaJanela"}[modo]
    setattr(turnos, nome, base[nome])
elif modo == "civil":
    turnos._dia_das_batidas_puro = lambda turnos, dias_civis: dict(dias_civis)
elif modo == "sem_pad":
    fonte = inspect.getsource(turnos._carregar_dias_das_batidas)
    alvo = "if pad_ini <= dia <= pad_fim"
    assert fonte.count(alvo) == 1
    exec(fonte.replace(alvo, "if True"), vars(turnos))
else:
    raise ValueError(modo)

execute_from_command_line(["manage.py", "test", *sys.argv[1:]])
PY
}

mutante borda "$par" "$orfas"
mutante lote "$par" "$orfas"
mutante civil "$par"
mutante sem_pad "$orfas"
```

### Catraca e custo

- **Catraca protegida:** duas asserções falharam na verificação estática. `REGISTRO` precisa trocar `_dia_das_batidas_puro` por `_carregar_dias_das_batidas` no sítio B1. Em `LINHAS_REGISTRADAS`, `ponto/turnos.py:3020` → `ponto/turnos.py:3026` para esse sítio, e `ponto/turnos.py:2998` → `ponto/turnos.py:2989` para `dia_do_turno_da_batida`. Os totais continuam em 9 pareadores e 81 derivações. O arquivo protegido ficou fora da edição. `core/tests/test_medida_turno_do_par.py:108`, `core/tests/test_medida_turno_do_par.py:556`, `core/tests/test_medida_turno_do_par.py:563`
- **Casca no signal:** zero consultas a mais por chamada, pela leitura do código. A F1 fazia 5–8. A cura faz **4–5** com PK apresentada, ou **1** quando o recorte está vazio. São batidas, células, candidatos, vínculo da ata e uma leitura condicional de geradoras. A carga já fornece DNA e última batida. O signal continua chamando por pai. `ponto/turnos.py:3020`, `ponto/turnos.py:3033`, `ponto/turnos.py:3040`, `ponto/turnos.py:3054`, `escala/alimentacao.py:147`, `colaboradores/services/pos_vinculo.py:37`, `ponto/signals.py:232`
- **Lote:** a primeira consulta do mapa acrescenta à construção do lote 3–4 leituras históricas, ou 1 se nenhum colaborador tiver PK apresentada. As consultas seguintes ao mapa reutilizam a carga. O histórico inteiro aumenta o volume potencial de leitura por pai. Latência, volume real e contagem SQL executada ficam [nao verificado]. `ponto/turnos.py:3020`, `ponto/turnos.py:3040`, `ponto/turnos.py:3154`

### Sonda para o envelope

A sonda usa as funções reais da base e da cura sobre o mesmo snapshot. Conta uma PK na competência se o dia civil ou o dia publicado por uma das versões estiver nela. A posição usa o dia civil, distinguindo as bordas internas do pad. Execução na sombra e contagens ficam [nao verificado]. `ponto/janelas.py:54`, `ponto/turnos.py:1834`, `ponto/turnos.py:2921`, `ponto/turnos.py:2951`

```python
import os
import runpy
from collections import Counter, defaultdict

from django.db import connection, transaction

from colaboradores.models import Colaborador, Empresa
from ponto.janelas import janela_fechamento
from ponto.turnos import batidas_apuraveis, data_local, dia_das_batidas, janela_com_pad

base = runpy.run_path(os.environ["BASE_TURNOS"])["dia_das_batidas"]
meses = (10, 11)
contagens = {mes: Counter() for mes in meses}
colabs_afetados = {mes: set() for mes in meses}
deltas = {mes: Counter() for mes in meses}
amostras = {mes: [] for mes in meses}
pks_afetadas = set()


def posicao(dia, ini, fim):
    if dia < ini:
        return "pad_ini"
    if dia == ini:
        return "borda_ini"
    if dia < fim:
        return "miolo"
    if dia == fim:
        return "borda_fim"
    return "pad_fim"


with transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
    janelas = {
        (empresa.pk, mes): janela_fechamento(mes, 2026, empresa)
        for empresa in Empresa.objects.only("pk", "dia_inicio_competencia")
        for mes in meses
    }
    if janelas:
        pads = [janela_com_pad(*janela) for janela in janelas.values()]
        menor = min(inicio for inicio, _ in pads)
        maior = max(fim for _, fim in pads)
        civis = defaultdict(dict)
        for cid, pk, ts in (
            batidas_apuraveis(None)
            .filter(timestamp__date__range=(menor, maior))
            .order_by()
            .values_list("colaborador_id", "pk", "timestamp")
            .iterator()
        ):
            civis[cid][pk] = data_local(ts)

        colabs = (
            Colaborador.objects.filter(pk__in=civis)
            .only("pk", "empresa_id").order_by("pk")
        )
        for numero, colab in enumerate(colabs, 1):
            alias = f"col{numero}"
            for mes in meses:
                ini, fim = janelas[colab.empresa_id, mes]
                pad_ini, pad_fim = janela_com_pad(ini, fim)
                dias = civis[colab.pk]
                if not any(pad_ini <= dia <= pad_fim for dia in dias.values()):
                    continue
                antes = base(colab, ini, fim)
                depois = dia_das_batidas(colab, ini, fim)
                assert antes.keys() == depois.keys(), (mes, alias, "universo de PKs divergente")

                tem_pk = False
                for pk, dia_base in sorted(antes.items()):
                    dia_cura, civil = depois[pk], dias[pk]
                    if not any(ini <= d <= fim for d in (civil, dia_base, dia_cura)):
                        continue
                    tem_pk = True
                    contagens[mes]["pks_na_comp"] += 1
                    if dia_base == dia_cura:
                        continue
                    contagens[mes]["pks_com_dia_diferente"] += 1
                    colabs_afetados[mes].add(alias)
                    pks_afetadas.add(pk)
                    pos = posicao(civil, ini, fim)
                    deltas[mes][(dia_cura - dia_base).days, pos] += 1
                    if len(amostras[mes]) < 15:
                        amostras[mes].append((alias, pk, civil, dia_base, dia_cura, pos))
                contagens[mes]["universo_colabs"] += int(tem_pk)

for mes in meses:
    c = contagens[mes]
    print(
        f"comp={mes:02d}/2026 universo_colabs={c['universo_colabs']} "
        f"pks_na_comp={c['pks_na_comp']} "
        f"pks_com_dia_diferente={c['pks_com_dia_diferente']} "
        f"colabs_afetados={len(colabs_afetados[mes])}"
    )
    print("delta_dias x posicao (cura - base)", dict(sorted(deltas[mes].items())))
    for col, pk, civil, antes, depois, pos in amostras[mes]:
        print(f"{col} pk={pk} civil={civil} base={antes} cura={depois} pos={pos}")

print("total_pks_distintas_com_dia_diferente", len(pks_afetadas))
print("total_colabs_distintos_afetados", len(set.union(*colabs_afetados.values())))
```

Ficaram fora da F2 o selo R6, a regra de fechamento por estoque no cron e o tratamento de períodos inválidos na adesão. A certificação cabe ao Code e ao opus final [nao verificado]. `core/tests/test_selo_r6_frase_do_juiz.py:20`, `chamados/services/adesao.py:143`, `colaboradores/services/pos_vinculo.py:38`

## 26/11 10:09 · sessao c9043d84b0a1 · R1-REMEDE-2611 · sonda · origem=Code

No trecho fornecido, o único retorno exibido é `rc_sombra=0`. A execução em produção está [nao verificado].

```text
sonda_leitura: script.py [c1b407550bdf8a63] no cpuset 4-7, lendo sombra como leitor (LEITURA)

AUTORIDADE {'dia_sem_lavratura_no_espelho': 9203, 'dias_comparados': 7815, 'bate_ate_10min': 7446, 'dia_batida_impar': 411, 'diverge_acima_60': 279, 'dono_BATIDA': 254, 'impar_que_DIVERGE': 261, 'impar_no_conjunto_do_dia': 257, 'impar_dentro_da_faixa': 218, 'dia_isento_ou_ausencia': 1708, 'e6_zero_esp_trabalho': 27, 'dono_CADASTRO': 77, 'esp_zero_e6_trabalho': 41, 'impar_FORA_da_faixa': 43, 'dia_sem_trabalho_ambos': 150, 'impar_que_BATE': 150, 'dono_ESTRUTURA': 38, 'esp_desconta_janela_declarada': 282, 'diverge_10_60': 22, 'impar_SO_POR_CORTE_do_oraculo': 4}
AUTORIDADE erros no espelho: 0
AUTORIDADE BATE (ate 10 min): 95.3% de 7815 dias comparados
AUTORIDADE dos que divergem: 218 dentro da faixa piso..teto (indecidivel sem DNA), 43 FORA da faixa (contraditorio)
AUTORIDADE paridade de quem: 257 com contagem impar no conjunto do dia (falta OU sobra batida), 4 partidos SO pelo corte do proprio oraculo
AUTORIDADE ESTRUTURA    38 dia-colab     159.1 h   23 colabs   o sistema se contradiz ou calcula errado DADO o cadastro -- fila 1
AUTORIDADE CADASTRO     77 dia-colab     305.6 h   33 colabs   o DNA nao descreve as batidas -- lista do admin, nao codigo
AUTORIDADE BATIDA      254 dia-colab    1401.7 h  126 colabs   falta ou sobra batida -- lista do admin, nao codigo
AUTORIDADE SOMA        369   <- tem de ser igual a soma das classes (369)
AUTORIDADE colabs com divergencia: 160
CSV_POR_DONO {'BATIDA': 254, 'CADASTRO': 77, 'ESTRUTURA': 38} TOTAL 369
CSV_PARIDADE {('IMPAR', 'BATIDA'): 205, ('par', 'CADASTRO'): 22, ('IMPAR', 'CADASTRO'): 55, ('par', 'ESTRUTURA'): 37, ('par', 'BATIDA'): 49, ('IMPAR', 'ESTRUTURA'): 1}
CAB emp|colab|dia|dono_da_divergencia|esp_min|piso_min|teto_min|dif|classe|paridade|dono_da_paridade|veredito
ESTRUTURA 4|37|2026-11-14|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
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
ESTRUTURA 2|200|2026-11-03|ESTRUTURA|420|598|720|178|diverge_acima_60|par|5|trabalhou |n_batidas=8
ESTRUTURA 2|218|2026-11-14|ESTRUTURA|103|0|0|-103|e6_zero_esp_trabalho|par|3|em_aberto |n_batidas=0
ESTRUTURA 2|220|2026-11-10|ESTRUTURA|337|430|486|93|diverge_acima_60|par|1|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-09|ESTRUTURA|551|597|610|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-10|ESTRUTURA|551|598|611|47|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-11|ESTRUTURA|550|598|611|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-13|ESTRUTURA|551|597|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-17|ESTRUTURA|553|598|612|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-18|ESTRUTURA|550|596|610|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-11-19|ESTRUTURA|554|599|613|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|296|2026-11-02|ESTRUTURA|345|0|0|-345|e6_zero_esp_trabalho|par|0|trabalhou |n_batidas=0
ESTRUTURA 2|302|2026-11-20|ESTRUTURA|296|0|0|-296|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|382|2026-11-10|ESTRUTURA|418|0|0|-418|e6_zero_esp_trabalho|par|3|trabalhou |n_batidas=0
ESTRUTURA 2|382|2026-11-11|ESTRUTURA|437|854|949|417|diverge_acima_60|par|5|trabalhou |n_batidas=8
ESTRUTURA 2|516|2026-10-23|ESTRUTURA|361|0|0|-361|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|727|2026-11-10|ESTRUTURA|307|0|0|-307|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|841|2026-11-16|ESTRUTURA|178|0|0|-178|e6_zero_esp_trabalho|par|2|folga |n_batidas=0
ESTRUTURA 2|874|2026-10-22|ESTRUTURA|667|383|383|-284|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 2|884|2026-11-08|ESTRUTURA|679|699|739|20|diverge_10_60|par|4|rejeitado |n_batidas=4
ESTRUTURA 3|909|2026-11-11|ESTRUTURA|831|245|374|-586|diverge_acima_60/FORA|IMPAR|CORTE_do_oraculo|em_aberto |n_batidas=4
ESTRUTURA 2|922|2026-10-29|ESTRUTURA|577|402|478|-175|diverge_acima_60|par|3|trabalhou |n_batidas=4
ESTRUTURA 2|922|2026-11-01|ESTRUTURA|1382|68|68|-1314|diverge_acima_60|par|4|trabalhou |n_batidas=2
ESTRUTURA 2|922|2026-11-04|ESTRUTURA|676|458|458|-218|diverge_acima_60|par|3|trabalhou |n_batidas=2
ESTRUTURA 2|989|2026-10-23|ESTRUTURA|721|567|567|-154|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|996|2026-10-28|ESTRUTURA|0|580|580|580|esp_zero_e6_trabalho|par|1|folga |n_batidas=2
ESTRUTURA 3|996|2026-11-10|ESTRUTURA|443|1026|1116|583|diverge_acima_60|par|5|em_aberto |n_batidas=6
ESTRUTURA 2|1010|2026-11-05|ESTRUTURA|982|687|747|-295|diverge_acima_60|par|3|trabalhou |n_batidas=4
FIM_LISTA
rc_sombra=0
```

## 26/11 10:02 · sessao c70107422e81 · R1-REMEDE-2611 · sonda · origem=Code

No trecho fornecido, não há `rc_sombra` diferente de 0. A última linha registra `rc_sombra=0`. A execução em produção está [nao verificado].

```text
sonda_leitura: script.py [3474291c37ec5740] no cpuset 4-7, lendo sombra como leitor (LEITURA)

AUTORIDADE {'dia_sem_lavratura_no_espelho': 10686, 'dias_comparados': 7544, 'bate_ate_10min': 7232, 'dia_isento_ou_ausencia': 1882, 'dia_batida_impar': 354, 'impar_que_BATE': 148, 'esp_zero_e6_trabalho': 36, 'dono_BATIDA': 210, 'diverge_acima_60': 226, 'impar_que_DIVERGE': 206, 'impar_no_conjunto_do_dia': 205, 'impar_FORA_da_faixa': 34, 'impar_dentro_da_faixa': 172, 'dono_CADASTRO': 74, 'e6_zero_esp_trabalho': 20, 'dia_sem_trabalho_ambos': 151, 'esp_desconta_janela_declarada': 257, 'dono_ESTRUTURA': 28, 'diverge_10_60': 30, 'impar_SO_POR_CORTE_do_oraculo': 1}
AUTORIDADE erros no espelho: 0
AUTORIDADE BATE (ate 10 min): 95.9% de 7544 dias comparados
AUTORIDADE dos que divergem: 172 dentro da faixa piso..teto (indecidivel sem DNA), 34 FORA da faixa (contraditorio)
AUTORIDADE paridade de quem: 205 com contagem impar no conjunto do dia (falta OU sobra batida), 1 partidos SO pelo corte do proprio oraculo
AUTORIDADE ESTRUTURA    28 dia-colab     105.9 h   17 colabs   o sistema se contradiz ou calcula errado DADO o cadastro -- fila 1
AUTORIDADE CADASTRO     74 dia-colab     288.5 h   30 colabs   o DNA nao descreve as batidas -- lista do admin, nao codigo
AUTORIDADE BATIDA      210 dia-colab     992.6 h  119 colabs   falta ou sobra batida -- lista do admin, nao codigo
AUTORIDADE SOMA        312   <- tem de ser igual a soma das classes (312)
AUTORIDADE colabs com divergencia: 148
CSV_POR_DONO {'BATIDA': 210, 'CADASTRO': 74, 'ESTRUTURA': 28} TOTAL 312
CSV_PARIDADE {('par', 'BATIDA'): 43, ('IMPAR', 'BATIDA'): 167, ('IMPAR', 'CADASTRO'): 39, ('par', 'CADASTRO'): 35, ('par', 'ESTRUTURA'): 28}
CAB emp|colab|dia|dono_da_divergencia|esp_min|piso_min|teto_min|dif|classe|paridade|dono_da_paridade|veredito
ESTRUTURA 4|37|2026-10-05|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 4|37|2026-10-09|ESTRUTURA|241|0|0|-241|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|129|2026-10-12|ESTRUTURA|0|779|779|779|esp_zero_e6_trabalho|par|2|em_aberto |n_batidas=2
ESTRUTURA 2|218|2026-10-15|ESTRUTURA|105|0|0|-105|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|252|2026-09-23|ESTRUTURA|548|594|608|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-09-24|ESTRUTURA|552|598|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-09-25|ESTRUTURA|556|604|616|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-09-28|ESTRUTURA|535|581|595|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-01|ESTRUTURA|545|590|605|45|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-02|ESTRUTURA|545|593|606|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-06|ESTRUTURA|551|597|611|46|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|252|2026-10-09|ESTRUTURA|544|592|604|48|diverge_10_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|348|2026-10-11|ESTRUTURA|0|665|665|665|esp_zero_e6_trabalho|par|1|folga |n_batidas=2
ESTRUTURA 2|369|2026-10-12|ESTRUTURA|476|416|476|-60|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 2|382|2026-09-21|ESTRUTURA|403|792|956|389|diverge_acima_60|par|5|trabalhou |n_batidas=8
ESTRUTURA 2|382|2026-09-30|ESTRUTURA|364|0|0|-364|e6_zero_esp_trabalho|par|2|trabalhou |n_batidas=0
ESTRUTURA 2|451|2026-09-23|ESTRUTURA|707|718|718|11|diverge_10_60|par|2|em_aberto |n_batidas=2
ESTRUTURA 2|451|2026-09-25|ESTRUTURA|705|718|718|13|diverge_10_60|par|2|em_aberto |n_batidas=2
ESTRUTURA 2|478|2026-09-25|ESTRUTURA|719|8|8|-711|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 3|515|2026-10-09|ESTRUTURA|244|0|0|-244|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 3|515|2026-10-11|ESTRUTURA|245|0|0|-245|e6_zero_esp_trabalho|par|0|em_aberto |n_batidas=0
ESTRUTURA 2|723|2026-10-08|ESTRUTURA|440|345|500|-95|diverge_acima_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 3|746|2026-09-24|ESTRUTURA|289|193|350|-96|diverge_acima_60|par|4|trabalhou |n_batidas=4
ESTRUTURA 2|841|2026-10-20|ESTRUTURA|681|522|701|-159|diverge_acima_60|par|4|em_aberto |n_batidas=4
ESTRUTURA 2|854|2026-10-04|ESTRUTURA|718|7|7|-711|diverge_acima_60|par|2|trabalhou |n_batidas=2
ESTRUTURA 2|864|2026-10-12|ESTRUTURA|509|449|509|-60|diverge_10_60|par|4|feriado |n_batidas=4
ESTRUTURA 3|868|2026-09-22|ESTRUTURA|0|679|733|679|esp_zero_e6_trabalho|par|1|em_aberto |n_batidas=4
ESTRUTURA 3|876|2026-09-21|ESTRUTURA|550|665|722|115|diverge_acima_60|par|1|em_aberto |n_batidas=4
FIM_LISTA
rc_sombra=0
```

## 26/11 09:35 · sessao 59b1208b1fda · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · construtor · origem=Code

Implementei a c4d nos quatro arquivos da obra. No caso comum de K2b, o fechamento voltou a **0,69 h de HE100** e **4,69 h de extras**. A lavra mantém as horas diárias e guarda em `detalhe` as parcelas originais em minutos, na ordem dos pares. A soma mensal das novas lavras usa esses minutos e arredonda uma vez. As demais rubricas voltam ao `SUM` do banco. `ponto/services/fechamento.py:967–986` · `ponto/services/dia_pago.py:174–204,365–369,627–669`

| Caso | Resultado em memória |
|---|---|
| **K2b** | Com 10,0 e 31,1 min de HE100 e 120 min de HE50 em cada dia, a c4b e a c4d deram **0,69 / 4,69 h**. A c4c e o mutante de reconversão deram **0,68 / 4,68 h**. Na c4d, a soma da nova lavra também deu **0,69 h**. `ponto/tests/test_feriado_sem_limite_do_suprimido.py:389–430` · `ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:59–72,116–117` |
| **K2c** | Com `10 × 0,10 + 0,445 h`, o teste com `SUM` simulado passou na c4b e na c4d sob ambas as ordens. A c4c e o mutante falharam com **1,44 h** onde o teste exigia **1,45 h**. `ponto/tests/test_feriado_sem_limite_do_suprimido.py:450–475` · `ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:73–83,118–119` |
| **K2d** | Três feriados de **30,2 min** deram **1,51 h** em cada uma das três HE100. O mutante que arredonda cada dia só na soma mensal da lavra deu **1,50 h**. O RED deste caso é contra o mutante. `ponto/tests/test_feriado_sem_limite_do_suprimido.py:432–447` · `ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:84–87` |

Comandos para o ambiente da suíte Code. O runner aplica cada mutação em memória. `ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:95–145` · `config/settings/ci.py:6–18`

```bash
feriado() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci \
    python3 -B -m ponto.tests.mutantes_feriado_sem_limite_do_suprimido "$1"
}

feriado base-k2b
feriado red-k2b
feriado k2b-reconverte-horas

feriado base-k2c
feriado red-k2c
feriado k2c-soma-python-sem-ordem

feriado k2d-arredonda-cada-dia-na-soma

feriado k2-soma-em-horas
feriado k2-tolerancia-he100
feriado mensal-le-par
feriado lavra-le-par
```

**Decisão para revisão.** Usei o JSON existente do DiaPago para registrar os minutos. A consulta soma lavras históricas e do oráculo em horas, a unidade em que foram gravadas. Para as novas lavras, ela também considera alterações nas colunas de horas, de modo que o teste de K2 continue acusando a diferença de **0,01 h**. `ponto/models.py:376–379` · `ponto/services/dia_pago.py:365–369,627–669` · `ponto/tests/test_feriado_sem_limite_do_suprimido.py:374–387`

**Catraca.** A medição em memória encontrou estas três mudanças de linha. O registro fica para a sessão. `core/tests/test_medida_turno_do_par.py:287,323,329,562–566`

| Entrada | Linha registrada → medida |
|---|---:|
| `folha/porta_export.py::medir` | 586 → 594 |
| `ponto/services/dia_pago.py::_dia_de` | 132 → 135 |
| `ponto/services/fechamento.py::_so_a_data` | 1592 → 1593 |

**Fora da c4d.** K1 segue aberto no turno partido. O vigia por data civil, o cálculo do oráculo, o filtro 12x36 do TXT e a diferença do G0.6 entre split e relógio ficam na fila. `ponto/motor_calculo_v2.py:3092–3129` · `folha/porta_export.py:590–603` · `ponto/calculador/regras.py:679–688` · `folha/export.py:357–380,420–423` · `ponto/tests/test_feriado_sem_limite_do_suprimido.py:318–321`

A contagem de `except` pelo AST ficou em **1→1** no `dia_pago`, **9→9** no `fechamento` e **0→0** nos dois arquivos de teste. Execução da suíte Django/PostgreSQL, ordem física do `SUM` no banco e efeito em produção **[nao verificado]**. A certificação fica com o Code e o Opus final **[nao verificado]**. [nao verificado]

## 26/11 09:31 · sessao aa6b041a94d3 · O30-F3-CONSTRUIR · construtor · origem=Code

O30-F3 c3 entregue na cópia sobre `4c7ac2c77`. Rodei 52 testes sem banco, incluindo os selos estruturais. Os seis grupos de RED falharam contra a c2 e os 16 mutantes novos falharam nos testes correspondentes. O diff final tem zero cláusulas `except` adicionadas ou removidas. `ponto/tests/test_atestado_f3_contrato.py:116`, `ponto/tests/mutantes_atestado_f3.py:18`, `ponto/tests/mutantes_atestado_f3.py:51`, `ponto/tests/mutantes_atestado_f3.py:139`

### Curas e provas

Para executar cada RED ou mutante, substitua `<seletor>` pelo nome da tabela. O resultado esperado é `rc=1` com falha no teste indicado. `ponto/tests/mutantes_atestado_f3.py:299`

```bash
PYTHONDONTWRITEBYTECODE=1 GIT_OPTIONAL_LOCKS=0 python3 -m ponto.tests.mutantes_atestado_f3 <seletor>
```

| Item | Cura e o que o RED pega | RED contra c2 | Mutantes |
|---|---|---|---|
| C1 | Com a filha mais recente na frente, a porta trava raiz → filha. O H06 usa a mesma ordem antes de conferir a identidade. `ponto/services/ausencia.py:1211`, `ponto/management/commands/rederivar_minutos_h06_comp11.py:125`, `ponto/views.py:2813`, `ponto/tests/test_atestado_f3_contrato.py:401` | `red_c2_c1` | `c1_decidir_sem_trava`, `c1_h06_trava_filha` |
| C2 | Criar com referência a uma filha grava a raiz. Reabrir corrige a FK legada, e o teste compara os bytes da rota protegida com os do ZIP. `ponto/services/ausencia.py:283`, `ponto/services/ausencia.py:559`, `ponto/services/ausencia.py:1438`, `ponto/tests/test_atestado_f3_contrato.py:436` | `red_c2_c2` | `c2_criar_grava_filha`, `c2_reabrir_grava_referencia_imediata`, `c2_reabrir_nao_achata` |
| C3 | Com a data movida pelo DP, a c2 gravava fim em 7/9 e 180 minutos. A releitura sob trava resulta em 28/9 e 420 minutos. A cerca usa o juiz registrado, inclusive na competência do fim proposto. `ponto/services/ausencia.py:1909`, `ponto/management/commands/curar_ausencia_sem_fim.py:48`, `core/juizes.py:349`, `ponto/tests/test_atestado_f3_contrato.py:523` | `red_c2_c3` | `c3_cura_sem_releitura`, `c3_cerca_so_no_inicio` |
| C4 | A aprovação concorrente barra a troca sob trava. A primeira entrega continua aceita e todas as irmãs em espera passam pela transição, inclusive no reenvio com anexo já existente. `api/views.py:1842`, `api/views.py:3129`, `ponto/services/ausencia.py:1118`, `ponto/tests/test_atestado_f3_contrato.py:571`, `ponto/tests/test_anexar_ausencia_colab.py:65` | `red_c2_c4` | `c4_api_sem_guarda`, `c4_barra_primeira_entrega`, `c4_ignora_irmas`, `c4_ignora_irma_legada`, `c4_reenvio_nao_libera_irmas` |
| C5 | O selo acusa `update_fields` indireto com valor `None` ou desconhecido e aceita a lista restritiva conhecida. `ponto/tests/test_atestado_f3_contrato.py:65`, `ponto/tests/test_atestado_f3_contrato.py:232` | `red_c2_c5` | `c5_save_indireto_none`, `c5_save_indireto_desconhecido` |
| C6 | Uma leitura de `localdate()` preenche mês e ano vazios na borda. O serviço recebe os dois valores explícitos ou o período livre. `ponto/views_atestado.py:103`, `ponto/services/atestados_lote.py:33`, `ponto/tests/test_atestado_f3_contrato.py:655` | `red_c2_c6` | `c6_borda_deixa_mes_ano_vazios`, `c6_lote_le_relogio` |

**Decisão contestável da sessão.** O primeiro arquivo em uma ausência aprovada sem comprovante conta como entrega tardia. `ponto/services/ausencia.py:1164`

**Fora da volta**

- A rota protegida lê um salto da FK. Uma cadeia legada ainda sem passagem pela porta de gravação conserva essa limitação. A sonda fornecida registra zero cadeias desse tipo `[nao verificado]`. `ponto/views.py:2147`, `ponto/services/ausencia.py:302`
- Nome do arquivo na auditoria e na ficha admin, storage sem namespace por tenant e máscara da sombra a cargo da sessão. `ponto/services/ausencia.py:1179`, `ponto/admin.py:50`, `core/media_protegida.py:53`, `bin/sombra_regras_pessoais.py:48`
- Vigência de afastamento aberto antes do recorte, `TIPOS_EXIGEM_DOC` cru no gate, JSON de reversão E4b, fuso de `decidido_em` e prazo da reabertura. `ponto/services/atestados_lote.py:55`, `ponto/services/ausencia.py:887`, `chamados/services/foto_e4b.py:251`, `ponto/services/atestados_lote.py:149`, `ponto/services/ausencia.py:1484`
- Integração com PostgreSQL, oito mutantes de integração e tempo do PDF real com 20 fotos `[nao verificado]`. A certificação fica com o Code e o opus final. `ponto/tests/mutantes_atestado_f3.py:325`, `ponto/tests/test_atestado_f3.py:331`

## 26/11 09:30 · sessao 96d4d81b3845 · CLEAR-NAO-PARA-A-NOITE · verificador · origem=Code

**Veredito final: reprovado.** O diff implementa a janela e o recado de AVISO, mas deixa três lacunas no fluxo: um disparo de dia pode atravessar a abertura da janela sem receber a nova instrução, o retorno de Ronald pode perder o pedido de `PAREI` no mesmo ciclo, e o selo aceita a repetição do `siga` que deveria sair uma vez. [hook_stop_fila1.py:1068](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1068) [hook_stop_fila1.py:1135](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1135) [test_orcamento_contexto.sh:543](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/tests/test_orcamento_contexto.sh:543)

Os quatro hashes de conteúdo conferem com o lado novo do `diff.txt`. A identidade Git do commit e do pai, o selo completo e o comportamento em produção ficam **[nao verificado]**. Reproduzi as transições e os contraexemplos indicados abaixo em memória. O selo completo cria e apaga arquivos. [test_orcamento_contexto.sh:32](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/tests/test_orcamento_contexto.sh:32)

### 1. Aval literal

| Item | Parecer |
|---|---|
| Janela 23:00–08:00 **ou** 30 min sem resposta | **Procedente para o contrato, parcial para o aval literal.** A janela local inclui 23:00 e exclui 08:00. Fora dela, a ausência exige **mais de** 30 min. Mede tempo desde a última fala classificada, sem verificar se ela responde a uma pergunta. [orcamento_contexto.py:139](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/orcamento_contexto.py:139) [orcamento_contexto.py:152](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/orcamento_contexto.py:152) |
| `AVISO: CLEAR devido desde HH:MM` no topo e fila segue | **Procedente como recado, parcial na transição.** Pede o AVISO ao disparar na janela, ao detectar o primeiro handoff elegível ou ao ver um `PAREI` ativo pela primeira vez na janela. O hook devolve instrução à sessão. O cumprimento no RELATO fica **[nao verificado]**. [hook_stop_fila1.py:1069](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1069) [hook_stop_fila1.py:1138](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1138) [hook_stop_fila1.py:1185](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1185) |
| Handoff antes de **cada** marco | **Não procede como cobertura de cada marco.** A instrução aparece no disparo novo na janela. As duas entradas posteriores na conduta noturna pedem o AVISO, sem repetir a instrução do handoff. `aviso_visto` limita o recado de fim de marco a uma vez por ciclo. O 7b já manda gerar o handoff ao fechar marco, e o `PreCompact` automático já chama o script. [hook_stop_fila1.py:1076](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1076) [hook_stop_fila1.py:1138](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1138) [hook_stop_fila1.py:1188](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1188) [CLAUDE.md:343](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/CLAUDE.md:343) [.claude/settings.json:25](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/.claude/settings.json:25) |
| DIFF/apply em curso impede ato novo | **Não procede em todos os caminhos.** Um disparo inicial fica pendente quando `_guarda_clear` detecta a trava. Com disparo ativo, tanto a troca direta de `PAREI` quanto o fim de marco noturno podem devolver `siga a fila` antes de consultar a guarda. Confirmei em memória o segundo caminho com DIFF/apply em curso. [hook_stop_fila1.py:936](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:936) [hook_stop_fila1.py:1070](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1070) [hook_stop_fila1.py:1138](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1138) [hook_stop_fila1.py:1169](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1169) |
| Ronald presente segue como hoje | **Procedente em ciclo diurno novo. Não procede em todo retorno no mesmo ciclo.** O disparo de dia ainda pede handoff e `PAREI`. No retorno, o pedido depende do novo limite de handoff e de `fim_visto`, que sobrevive à janela. [hook_stop_fila1.py:1148](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1148) [hook_stop_fila1.py:1192](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/bin/hook_stop_fila1.py:1192) |
| Linha no 7b, registro em `PROMPTS.md` e config do Code | **Não procede para a entrega documental. Procedente para a proibição de mexer na config.** O `diff.txt` fornecido só altera os quatro arquivos de código e selo. O 7b tem a regra anterior de handoff e compact, e a cópia de `PROMPTS.md` termina sem o registro deste aval. [CLAUDE.md:343](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/CLAUDE.md:343) [app/docs/PROMPTS.md:3005](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/app/docs/PROMPTS.md:3005) [.claude/settings.json:14](/home/ronald/codex_ws/e1f1d9e710998e21e0301dda5cd7c34635145fd9/.claude/settings.json:14) |

### 2. Escritores, leitores e significado

| Campo | Quem escreve e quem lê |
|---|---|
| `disparo.noite_clear_visto` | O Stop lê em `bin/hook_stop_fila1.py:1072` e grava `True` na linha 1073. Significa que já pediu a troca de um `PAREI` na janela neste ciclo. |
| `disparo.aviso_visto` | O Stop lê em `bin/hook_stop_fila1.py:1138` e grava `True` na linha 1141. Significa que já **pediu** o AVISO no fim de marco. O disparo inicial e a troca direta de `PAREI` deixam esse campo intacto. `bin/hook_stop_fila1.py:1072`, `bin/hook_stop_fila1.py:1185`. |
| `disparo.handoff_min` | Já era gravado quando o CLEAR era adiado pela guarda, em `bin/hook_stop_fila1.py:1101`, e lido no `PAREI` e no fim de marco diurno, linhas 1085 e 1148. O diff acrescenta escrita e leitura no fim de marco noturno, linhas 1141 e 1139. **Procedente que o significado se mantém.** É um limite inferior inclusivo, em segundos Unix. Os dois escritores gravam o instante do **Stop**. Os leitores comparam o `st_mtime` do handoff com `max(hora, handoff_min)`. Mudou o evento que pode avançar o limite. `bin/hook_stop_fila1.py:929`, `bin/hook_stop_fila1.py:999`. |
| `CLEAR-NOITE` | O Stop enfileira o evento na troca de `PAREI`, no fim de marco e no disparo inicial. O mesmo escritor persiste a trilha após salvar o estado. O disjuntor lê e conta. `bin/hook_stop_fila1.py:1074`, `bin/hook_stop_fila1.py:1142`, `bin/hook_stop_fila1.py:1186`, `bin/hook_stop_fila1.py:887`, `bin/hook_stop_fila1.py:918`, `bin/disjuntor.py:2083`, `bin/disjuntor.py:2108`. |
| `d['clear_noite']` | `disjuntor.ler` produz em `bin/disjuntor.py:2149`. `disjuntor.linha` lê em `bin/disjuntor.py:2381`, chamado pelo ESTADO em `bin/gerar_estado.py:298` e conferido pelo selo em `bin/tests/test_orcamento_contexto.sh:527`. **Procedente a ambiguidade do zero.** Trilha ilegível e ausência de eventos podem render o mesmo `0`. Até três recados de um ciclo podem entrar no contador, inclusive de dia por ausência. O contador mede eventos emitidos, e o rótulo diz “avisos”. `bin/disjuntor.py:2080`, `bin/disjuntor.py:2114`, `bin/hook_stop_fila1.py:1074`, `bin/hook_stop_fila1.py:1142`, `bin/hook_stop_fila1.py:1186`. |
| `HOOK_STOP_HORA_LOCAL` | O selo fornece a string em `bin/tests/test_orcamento_contexto.sh:115` e `bin/tests/test_orcamento_contexto.sh:454`. O Stop lê apenas com `HOOK_STOP_RAIZ`, em `bin/hook_stop_fila1.py:28` e `bin/hook_stop_fila1.py:1067`. O avaliador converte para hora inteira em `bin/orcamento_contexto.py:143`. Sobrescreve a hora usada na janela. O `HH:MM` do AVISO continua vindo do epoch do disparo no fuso local do processo. `bin/hook_stop_fila1.py:989`, `bin/hook_stop_fila1.py:1069`. |

**Fuso e instante.** A ausência compara epochs. Timestamp ISO com `Z` tem UTC explícito, e timestamp sem offset herda o fuso local. A trilha grava data e hora locais sem offset, e o ESTADO as reconverte no fuso do leitor. O mesmo fuso entre os processos de produção fica **[nao verificado]**. O filtro do contador tem só o limite inferior de 24 h, por isso também aceita linha datada no futuro. `bin/orcamento_contexto.py:84`, `bin/orcamento_contexto.py:152`, `bin/hook_stop_fila1.py:887`, `bin/disjuntor.py:2074`, `bin/disjuntor.py:2091`.

### 3. `_fala_humana` e transcript real

**Linha real classificada errado: [nao verificado].** A fixture real raspada tem registros `user` sem `message.content`, então o leitor retorna sem fala humana nela. O texto humano usado pelo selo é acrescentado sinteticamente. `bin/tests/fixtures/orcamento_transcript.jsonl:1`, `bin/tests/test_orcamento_contexto.sh:8`, `bin/tests/test_orcamento_contexto.sh:72`, `bin/orcamento_contexto.py:107`.

| Forma da entrada | Comportamento demonstrável |
|---|---|
| Imagem do usuário | **Falso ausente** para imagem sem texto. Imagem com bloco de texto não vazio conta. `bin/orcamento_contexto.py:108`. |
| Slash | `<command-name>/clear</command-name>` passa como humano se for `user` sem as flags de exclusão. `<local-command…>` é descartado. A primeira forma aparece no leitor de marcos da própria cópia. `bin/orcamento_contexto.py:94`, `bin/orcamento_contexto.py:105`, `bin/orcamento_contexto.py:335`. |
| Resposta a `AskUserQuestion` | **Falso ausente se vier em `tool_result`**, mesmo com texto humano. O formato da resposta real fica **[nao verificado]**. `bin/orcamento_contexto.py:108`. |
| Eco de bash `!` | Um `user` sem flags cujo conteúdo começa com `<bash-stdout>` passa como humano. A linha real e suas flags ficam **[nao verificado]**. `bin/orcamento_contexto.py:94`, `bin/orcamento_contexto.py:105`, `bin/orcamento_contexto.py:112`. |
| Enfileirada e removida | O `enqueue` continua marcando presença após `remove`. O selo afirma exatamente esse resultado. Um `enqueue` com string vazia também conta. `bin/orcamento_contexto.py:102`, `bin/orcamento_contexto.py:134`, `bin/tests/test_orcamento_contexto.sh:494`, `bin/tests/test_orcamento_contexto.sh:504`. |
| Sidechain | **Não procede para `user` marcado. Procede para `enqueue` marcado.** O ramo da fila roda antes do filtro de sidechain, meta e resumo. `bin/orcamento_contexto.py:102`, `bin/orcamento_contexto.py:105`. |

### 4. Transições Stop a Stop

- **Noite → volta, com fim de marco noturno.** Stop 1 arma e pede handoff e AVISO. Stop 2 vê handoff elegível, pede AVISO e grava `handoff_min = agora`. Stop 3, já fora da janela, com o mesmo handoff, cai no caminho normal. Stop 4, com handoff que atinge o novo limite e `fim_visto` vazio, pede `PAREI`. Stop 5 libera especificamente pelo CLEAR se o topo tiver a linha, o handoff continuar elegível e a guarda estiver livre. `bin/hook_stop_fila1.py:1185`, `bin/hook_stop_fila1.py:1138`, `bin/hook_stop_fila1.py:1148`, `bin/hook_stop_fila1.py:1085`, `bin/hook_stop_fila1.py:1115`.
- **Dia → noite, com disparo ativo e topo neutro.** Stop 1 diurno arma e pede o rito de dia. Stop 2 na janela, sem handoff elegível, retorna `None`: não anuncia a troca de conduta. Stop 3 com handoff elegível pede AVISO uma vez, sem a guarda de DIFF/apply. Stops seguintes ficam no caminho normal do Stop. `bin/hook_stop_fila1.py:1192`, `bin/hook_stop_fila1.py:1135`, `bin/hook_stop_fila1.py:1155`, `bin/hook_stop_fila1.py:1436`.
- **Dia → noite → volta, com `PAREI` já pedido de dia.** Stop 1 de dia grava `fim_visto`. Primeiro Stop na janela com `PAREI` ativo pede a troca e grava `noite_clear_visto`. O seguinte, se o topo continuar em `PAREI`, retorna `None`. Com a troca feita, na volta o `fim_visto` antigo impede novo pedido diurno, mesmo com handoff novo. A pendência pode permanecer até clear/compact ou até o topo voltar a `PAREI` por outra via. Se a linha antiga ficar no topo e já houver `clear_parei_desde`, o tempo da noite entra na expiração de 10 min. `bin/hook_stop_fila1.py:1025`, `bin/hook_stop_fila1.py:1070`, `bin/hook_stop_fila1.py:1115`, `bin/hook_stop_fila1.py:1148`, `bin/orcamento_contexto.py:56`.

**Não procede laço das duas flags nem Stop permanentemente bloqueado no fluxo nominal. Procede pendência sem novo pedido.** No segundo Stop com `PAREI` noturno, o orçamento não dá `CLEAR-LIBERA`, mas o hook pode encerrar o turno pelo `ALLOW` comum da fila. Em falha contida, há ainda liberação específica pelo par topo + handoff recente, sem consulta à janela. `bin/hook_stop_fila1.py:1072`, `bin/hook_stop_fila1.py:1138`, `bin/hook_stop_fila1.py:1304`, `bin/hook_stop_fila1.py:1424`, `bin/hook_stop_fila1.py:1443`, `bin/hook_stop_fila1.py:1213`.

### 5. Exceções e contrato item 3

- **Procedente a contenção.** De dia, `OSError` ao abrir ou ler o transcript vira `None` e a máquina segue com Ronald presente. Uma `TypeError` ao juntar um bloco com `text` numérico, por exemplo, chega ao `_orcamento_contido`, que registra `FALHA` e aplica o fallback existente. **Não procede** escape de uma nova exceção ordinária pelo caminho do Stop. `bin/orcamento_contexto.py:111`, `bin/orcamento_contexto.py:148`, `bin/hook_stop_fila1.py:1197`, `bin/hook_stop_fila1.py:1399`.
- **Não procede a garantia literal de “ilegível = não sei” em toda entrada.** A janela horária decide antes de abrir o arquivo. De dia, o leitor pula JSON ou timestamp inválido. Reproduzi uma linha `user` truncada após uma linha datada de 40 min atrás: o leitor declarou ausência pela cauda. Uma cauda sem fala com datas recentes ou sem data útil retorna `None`. `bin/orcamento_contexto.py:126`, `bin/orcamento_contexto.py:143`, `bin/orcamento_contexto.py:158`.

### 6. Mordida do bloco 7

Os mutantes abaixo são avaliações por assert. A repetição do `siga` e o retorno diurno vazio foram reproduzidos em memória. `bin/tests/test_orcamento_contexto.sh:513`, `bin/tests/test_orcamento_contexto.sh:539`.

| Caso | Mutante que cai e lacuna que passa |
|---|---|
| **(a) Fala e janela** | Excluir o `enqueue` válido, aceitar qualquer uma das linhas sintéticas recentes excluídas ou desligar a janela por ausência derruba os asserts. Aceitar 08:00 como noite passa, pois o assert só pede resultado diferente de `None`. Rejeitar apenas o `user` digitado de 50 min também passa: o `enqueue` de 40 min continua vencendo. `bin/tests/test_orcamento_contexto.sh:493`, `bin/tests/test_orcamento_contexto.sh:504`, `bin/tests/test_orcamento_contexto.sh:509`. |
| **(b) Dia presente** | Forçar a rota noturna ao meio-dia cai. **Saída vazia passa** neste caso, assim como sumir com o pedido diurno. O zero diurno anunciado no comentário também fica sem assert: `_zera` apaga a trilha antes da medição. `bin/tests/test_orcamento_contexto.sh:106`, `bin/tests/test_orcamento_contexto.sh:513`, `bin/tests/test_orcamento_contexto.sh:516`. |
| **(c) Disparo e contador** | Suprimir o recado noturno, o AVISO, o evento, a contagem ou a renderização derruba ao menos um assert. Remover as instruções de handoff e DIFF/apply passa. Um contador fixo em `1` ou sem corte de 24 h passa nesta amostra. `bin/tests/test_orcamento_contexto.sh:516`, `bin/tests/test_orcamento_contexto.sh:530`. |
| **(d) Marco e volta** | Suprimir o recado de fim noturno, omitir a atualização de `handoff_min` ou impedir o pedido diurno com handoff novo cai. Retirar a guarda `aviso_visto` passa, pois só há um Stop de fim de marco à noite. O Stop intermediário aceita saída vazia, e o assert noturno só exige o prefixo do recado. `bin/tests/test_orcamento_contexto.sh:532`, `bin/tests/test_orcamento_contexto.sh:535`, `bin/tests/test_orcamento_contexto.sh:537`. |
| **(e) `PAREI` noturno** | Suprimir a primeira troca, devolver `CLEAR-LIBERA` à noite nesse cenário ou impedir a liberação diurna cai. **Trocar `if not disp.get('noite_clear_visto')` por `if True` passa.** No segundo Stop, tanto o `siga` repetido quanto um `ALLOW` comum convertido pelo helper em `siga-aviso` satisfazem `[ -n "$_s" ]`. `bin/hook_stop_fila1.py:1072`, `bin/tests/test_orcamento_contexto.sh:543`, `bin/tests/test_orcamento_contexto.sh:548`, `bin/tests/_saida_do_stop.sh:13`. |
| **(f) Ausência de dia** | Remover a ausência diurna ou usar a última linha datada em vez da última fala cai: o teste põe um turno recente do assistente depois da fala antiga. Saída vazia falha. `bin/tests/test_orcamento_contexto.sh:550`, `bin/tests/test_orcamento_contexto.sh:558`. |

Há também regressão demonstrável em `test_disjuntor.sh`: ele usa a fixture raspada sem acrescentar fala recente e sem fixar a hora. Com a fixture antiga, a ausência abre a janela de dia e o intervalo horário abre à noite. `c1_par` e `c1_fim_de_marco` ainda esperam a rota diurna. `bin/tests/test_disjuntor.sh:37`, `bin/tests/test_disjuntor.sh:80`, `bin/tests/test_disjuntor.sh:92`, `bin/tests/test_disjuntor.sh:117`, `bin/tests/test_disjuntor.sh:125`, `bin/orcamento_contexto.py:158`.

### 7. Classes de reprovação

- **Relógio fora da borda — não procede para a nova decisão.** O Stop fornece `agora`. A trilha usa o relógio próprio já existente, de modo que a hora gravada é a do append e o AVISO usa a do disparo. `bin/hook_stop_fila1.py:999`, `bin/hook_stop_fila1.py:1068`, `bin/hook_stop_fila1.py:887`.
- **Segundo escritor fora da porta — não procede.** Os novos campos são gravados pelo Stop sob o `flock` existente. `CLEAR-NOITE` usa a mesma trilha após a gravação do estado. O ESTADO lê e formata. `bin/hook_stop_fila1.py:972`, `bin/hook_stop_fila1.py:903`, `bin/hook_stop_fila1.py:918`, `bin/disjuntor.py:2083`.
- **Derivação paralela — procedente.** Já existia `disjuntor.ultima_humana` para a Escada. O novo leitor decide o mesmo filtro básico com critérios divergentes: o anterior só procura `user` e exclui texto iniciado por `<`, enquanto o novo aceita `enqueue` e exclui apenas dois prefixos. O disjuntor também abre a mesma trilha duas vezes, para `ATENDIDO` e `CLEAR-NOITE`. `bin/disjuntor.py:1342`, `bin/disjuntor.py:1378`, `bin/orcamento_contexto.py:94`, `bin/orcamento_contexto.py:102`, `bin/disjuntor.py:2081`.
- **Selo sem mordida — procedente nos pontos apontados.** O dia passa vazio em (b), o `siga` repetido passa em (e), e o corte de 08:00 passa inclusivo em (a). `bin/tests/test_orcamento_contexto.sh:509`, `bin/tests/test_orcamento_contexto.sh:514`, `bin/tests/test_orcamento_contexto.sh:544`.
- **Campo com dois significados — não procede para `handoff_min`. Procede para o zero de `clear_noite`.** O primeiro continua sendo limite inferior de `mtime`. O segundo representa tanto zero eventos lidos quanto falha de leitura. `bin/hook_stop_fila1.py:1085`, `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1141`, `bin/disjuntor.py:2080`, `bin/disjuntor.py:2114`.

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

_8319 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
