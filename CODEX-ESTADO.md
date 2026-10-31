# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 10:40** · publicado em **31/10/2026 10:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 10:40 · sessao 275443782328 · - · - · origem=Ronald (tmux)

Mapa de linhagem, somente por inspeção de código.

| Campo | Quem produz/escreve | Quem lê no caminho dos quatro sítios |
|---|---|---|
| `cel.trabalha` | Campo triestado: `app/escala/models.py:1654`. Geração/regeneração obtêm `valor` de `eh_dia_trabalho_calculado`: `app/ponto/portas/celula.py:243`, `:590`; escrevem em `:247`, `:260`, `:595`, `:612`. Fora da vigência vira `False`: `:231`; reversão deriva do DNA anterior: `:532`. | Juiz único lê em `app/chamados/juizes.py:408`; os quatro emissores o chamam em `app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/materializacao.py:160`, `app/ponto/services/cluster_espurio.py:26`, `app/ponto/management/commands/detectar_intervalo_ausente.py:49`. |
| `ata.tipo_dia` | Grade normal produz trabalho/folga em `app/escala/utils.py:727`, `:752`; grade por turno em `:1743`, `:1749`; overlays escrevem folga/falta/ausência em `:1118`, `:1174`, `:1180`. Cartório copia para ata em `app/ponto/services/cartorio.py:566`. | Juiz lê em `app/chamados/juizes.py:408`; leitor genérico lê em `app/escala/services/leitor_celula.py:344`; fábrica também o consulta transitivamente em `app/chamados/services/declaracao_texto.py:90`, `:95`. |
| `ata.sem_ancora` | Grade normal: `work is None` em `app/escala/utils.py:729`, saída em `:752`; grade por turno deriva `eh_dia_trabalho(...) is None` em `:1701`; ramo intermitente deriva falta de âncora de batidas em `:1456`, repassa em `:1514`. Cartório copia/coage em `app/ponto/services/cartorio.py:569`. | Nenhum dos quatro emissores mantém leitura direta após a cura; leitor genérico repassa em `app/escala/services/leitor_celula.py:362`. |
| `ata.lampadas` | Produtor é `lampadas_do_dia` em `app/ponto/services/cartorio.py:427`, com `append` em `:500`, `:504`, `:506`, `:543`, `:554`; `ata_do_dia` chama-o em `:564` e inclui resultado em `:566`. Dispensa modifica luzes em `:614`; teto temporal modifica em `:711`. | Diretos: `app/chamados/services/materializacao.py:158`, `app/ponto/services/cluster_espurio.py:29`, `app/ponto/management/commands/detectar_intervalo_ausente.py:62`. Fábrica: `app/chamados/services/fabrica_da_celula.py:49` → `app/chamados/services/disputa_emissao.py:827` → `app/chamados/services/declaracao_texto.py:82` → `app/escala/services/leitor_celula.py:93`. |

Os três campos da ata persistem pela mesma porta: `CelulaDia.lavrar_veredito` delega em `app/escala/models.py:1781`; a porta recebe/copia a ata em `app/ponto/portas/celula.py:697`, atribui em `:711` ou `:725` e salva por `salvar_e_emitir` em `:732`, cujo `save` e evento estão no mesmo `atomic` em `:639`. O julgamento em lote usa `save=False` e entrega os campos a `carimbar`: `app/ponto/services/cartorio.py:1520`, `:1535`.

Há atribuições de ata em memória fora da lavra, mas sem nova derivação de lâmpadas: `app/escala/utils.py:1826` copia a célula e chama o mesmo `lampadas_do_dia` em `:1832`; `app/ponto/portas/celula.py:678` também copia para leitura. `carimbar_precedencia` só modifica a chave `precedencia`, preservando o restante: `app/ponto/portas/celula.py:774`.

Cuidado com a equivalência declarada no comentário do diff: `sem_ancora` não é universalmente apenas `trabalha=None`; no ramo intermitente vem de `bool(_locs_i) and _anc_ix is None`: `app/escala/utils.py:1456`.

Outros predicados próprios relevantes:

- `app/chamados/services/declaracao_texto.py:95`: só `tipo_dia == 'trabalho'` tem marcos cobrados; a fábrica chega aqui por `app/chamados/services/fabrica_da_celula.py:49`. Para `trabalha=None` com tipo ausente, juiz libera (`app/chamados/juizes.py:408`), leitor deriva `indefinido` (`app/escala/services/leitor_celula.py:345`) e essa cláusula cala os marcos (`app/chamados/services/declaracao_texto.py:95`).
- `app/ponto/services/dia_do_colab.py:121`: `not cel.trabalha` descreve dia sem trabalho, incluindo `None`.
- `app/relatorios/dia_da_frota.py:47`: `not cel.trabalha` conta folga, incluindo `None`.
- `app/ponto/services/passivo_s84.py:96`: `TRABALHO if cel.trabalha else FOLGA`, incluindo `None` em folga.
- `app/ponto/marcos.py:155`, `:159`: veto próprio lê `trabalha`/veredito e veta `False`, sem ler `tipo_dia`.
- `app/ponto/services/flip_auto.py:208`, `:212`: decide folga/âncora com `is False` e `is not True`.
- Recortes ORM que excluem `None`: `app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/services/fila_por_causa.py:192`, `app/ponto/services/vigia_entrada.py:79`, `:120`, `app/ponto/services/vigia_de_hora.py:21`.

Leitores adicionais dos campos incluem `app/ponto/juiz_batida.py:160`, `app/chamados/juizes.py:457`, `:888`, `:1008`, `app/ponto/services/lastro.py:89`, `app/ponto/worklist.py:22`, `app/folha/previa.py:84`, `app/folha/services/sem_furo.py:22`, `app/pautas/services.py:414`, `app/relatorios/cartao_pela_celula.py:289`; leitores de `sem_ancora` derivado incluem `app/folha/causas.py:96`, `app/relatorios/furos.py:157`, `:218`, `:237`, `app/inteligencia/calculadores.py:48`, `:143`, `app/ponto/services/fechamento.py:1018`, `app/ponto/supra_juiz.py:214`.

## 31/10 10:40 · sessao 98123c160ec5 · - · - · origem=Ronald (tmux)

**Mordida dos testes — previsão estática; execução e RED efetivo [não verificado].** [nao verificado]

| Reversão isolada | Resultado antigo para fixture limbo | Assert que a rejeita |
|---|---|---|
| Fábrica | `[]`, pois `None is not True` | `app/chamados/tests/test_f1c4_3_emissores.py:70`; guarda atual `app/chamados/services/fabrica_da_celula.py:80` |
| Materialização | `False`, pois pula a única célula | `app/chamados/tests/test_f1c4_3_emissores.py:86`; loop/retorno `app/chamados/services/materializacao.py:156`, `:169` |
| Cluster | `[]`, pois pula a única célula | `app/chamados/tests/test_f1c4_3_emissores.py:101`; loop/retorno `app/ponto/services/cluster_espurio.py:23`, `:34` |
| Intervalo | `[]`, pois pula a única célula | `app/chamados/tests/test_f1c4_3_emissores.py:117`; loop/retorno `app/ponto/management/commands/detectar_intervalo_ausente.py:46`, `:86` |

A reversão parcial para `dia_sem_trabalho(cel) or ata.get('sem_ancora')` também é rejeitada nos sítios cluster/intervalo: a fixture grava `sem_ancora=True` quando `trabalha=None`, e os asserts exigem o dia retornado. `app/chamados/tests/test_f1c4_3_emissores.py:55`, `:101`, `:117`.

Há controles positivos `True/'trabalho'` nos quatro helpers; os controles de folga usam **None/'folga'**, que a guarda velha já descartava. Logo estes controles não isolam a nova exclusão de **True/'folga'**. `app/chamados/tests/test_f1c4_3_emissores.py:73`, `:76`, `:89`, `:92`, `:104`, `:107`, `:120`, `:123`.

Os novos testes não incluem `False`, tipo ausente/vazio, `True + sem_ancora=True`, nem verificam a chamada ao juiz mediante spy/patch: chamam funções reais e verificam somente resultados. Um clone equivalente do predicado poderia satisfazer esses asserts. `app/chamados/tests/test_f1c4_3_emissores.py:54`, `:64`, `:79`, `:95`, `:110`.

**Escopo real:** fábrica retorna motivos, e só `perguntar_pela_celula` chama o escritor; o teste chama apenas `motivos_a_perguntar`. Materialização testa uma segunda opinião de paridade. Cluster testa detecção, não `reconciliar_colab`. Intervalo já tem emissão aposentada: reconcilia legado e depois executa `continue`. `app/chamados/services/fabrica_da_celula.py:111`, `:146`, `:151`; `app/chamados/services/materializacao.py:128`, `:438`, `:818`; `app/ponto/services/cluster_espurio.py:69`, `:86`; `app/ponto/management/commands/detectar_intervalo_ausente.py:147`, `:154`.

**Imports/query/chokepoint:**

- Os quatro novos imports são internos às funções, não imports novos de módulo; o juiz apenas lê `cel.ata` e `cel.trabalha`, sem ORM nem relógio em seu corpo. `app/chamados/services/fabrica_da_celula.py:78`; `app/chamados/services/materializacao.py:141`; `app/ponto/services/cluster_espurio.py:20`; `app/ponto/management/commands/detectar_intervalo_ausente.py:37`; `app/chamados/juizes.py:407`.
- Há dependência circular **preexistente** entre `chamados.juizes` e `chamados.models`: juiz importa models no topo; models reexporta juiz e materialização ao final. Os imports locais evitam introduzir a aresta durante definição dos quatro módulos; ausência de erro em execução [não verificado]. `app/chamados/juizes.py:6`; `app/chamados/models.py:3135`, `:3140`.
- O caminho quente existe: signal síncrono da batida chama julgamento; cartório chama cluster e fábrica. `app/ponto/signals.py:85`, `:93`, `:131`, `:135`; `app/ponto/services/cartorio.py:793`, `:797`, `:798`, `:800`.
- Embora o juiz não acrescente consulta explícita, a mudança passa células antes puladas para consultas e escritas posteriores: fábrica consulta perguntas e pode gerar perguntas; cluster consulta dedup e abre chamados. Portanto “delta total zero na batida” não está demonstrado. `app/chamados/services/fabrica_da_celula.py:103`, `:111`, `:151`; `app/ponto/services/cluster_espurio.py:56`, `:62`, `:65`, `:86`.
- O selo existente cobra tetos e quantidade de `on_commit`; tetos dos cenários de batida com furos são 191/241 quando fecham e 134/194 quando ficam. As respectivas fixtures criam `trabalha=True`; não demonstram o custo do novo universo limbo. `app/ponto/tests/test_selo_chokepoint_escrita.py:157`, `:158`, `:163`, `:164`, `:212`, `:218`, `:340`, `:380`.
- Os novos testes não medem consultas ou imports: o arquivo inteiro contém fixtures e asserts de retorno. O teto após a alteração, custo real de produção e import completo da aplicação estão [não verificado]. `app/chamados/tests/test_f1c4_3_emissores.py:21`, `:123`.

Nenhum comando de teste foi executado e nenhum arquivo foi escrito. [não verificado] [nao verificado]

## 31/10 10:19 · sessao 026ee072896a · E3-STATUS · censo · origem=Code

Censo estático da cópia fornecida. Estado do banco, execução dos testes e correspondência com um commit Git: **[não verificado]**. [nao verificado]

**1. Cadastro × realidade — L-084**

A lista completa **fica em memória no motor e no resultado; o fechamento não a persiste**. `dias_cadastro_x_realidade` é atributo do `@dataclass ResultadoMes`, nasce vazia a cada cálculo e recebe a lista acumulada no retorno. O comando de diff confirma essa ausência de persistência e captura as instâncias do motor para obter o relato. Referências: `app/ponto/motor_calculo_v2.py:218`, `app/ponto/motor_calculo_v2.py:239`, `app/ponto/motor_calculo_v2.py:2221`, `app/ponto/motor_calculo_v2.py:2657`, `app/ponto/management/commands/diff_reclassificar_partido.py:169`, `app/ponto/management/commands/diff_reclassificar_partido.py:188`.

Os escritores encontrados são:

- `_janela_he`: acrescenta a acusação `l084_do_dia`, deduplicada por data — `app/ponto/motor_calculo_v2.py:1552`, `app/ponto/motor_calculo_v2.py:1560`.
- `_acusa_cadastro_x_realidade`: acrescenta as pontas fora da janela, chamada para entrada e saída — `app/ponto/motor_calculo_v2.py:1608`, `app/ponto/motor_calculo_v2.py:1648`, `app/ponto/motor_calculo_v2.py:1686`.
- `aplicar_tolerancia`: acrescenta o relato quando o cadastro não descreve as duas pontas — `app/ponto/motor_calculo_v2.py:1926`, `app/ponto/motor_calculo_v2.py:1930`.
- O fechamento concatena os relatos das fatias no resultado composto, ainda em memória — `app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:278`.

Os leitores atuais encontrados são o espelho, que traduz o relato em pendências e expõe o subconjunto acima do teto; o oráculo de dono, que extrai as datas; e os diagnósticos/diffs. Referências: `app/ponto/services/espelho.py:751`, `app/ponto/services/espelho.py:1263`, `app/ponto/management/commands/e6_oraculo.py:150`, `app/ponto/management/commands/e6_oraculo.py:178`, `app/ponto/management/commands/pautas_discordantes.py:102`, `app/ponto/management/commands/diff_janela_he.py:70`, `app/ponto/management/commands/diff_reclassificar_partido.py:188`.

Também existem `Censo.cxr` e `Rubricas.detalhe['cadastro_x_realidade']`, em memória. Esse detalhe **não é copiado para `DiaPago.detalhe`**: o lavrador lê das rubricas apenas `valores` e `nao_decide`. Referências: `app/ponto/calculador/periodo.py:286`, `app/ponto/calculador/periodo.py:790`, `app/ponto/calculador/regras.py:857`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:329`, `app/ponto/services/dia_pago.py:343`.

Há persistências relacionadas, mas de outras estruturas: a tela Cadastro × Realidade lê assinaturas do esmeril, gravadas em `MetricaSnapshot.detalhe`; a C9 guarda por item o `dono` derivado do oráculo. Nenhuma dessas escritas conserva a lista completa do motor com todos os relatos L-084. Referências: `app/escala/services/cadastro_realidade.py:635`, `app/ponto/services/esmeril_espelho.py:324`, `app/ponto/services/esmeril_espelho.py:330`, `app/ponto/services/teto_na_lista.py:86`, `app/ponto/services/teto_na_lista.py:113`.

**2. Feriado no caminho do status**

`status_da_lavra` é pura: recebe fatos e não consulta calendário diretamente. Entretanto, o caminho **combina grade/célula com consulta dinâmica à precedência A2**; não depende exclusivamente do DNA. Referências: [app/ponto/status_dia.py:57](/home/ronald/codex_ws/ae0502e59fd6dc6cc647154e864643a7158a30b3/app/ponto/status_dia.py:57), `app/ponto/status_dia.py:122`.

Os ramos encontrados são:

- **Grade que alimenta o status:** o cartório monta a grade; ela consulta `eh_dia_trabalho` e usa marcos do DNA. Havendo célula, `eh_dia_trabalho` devolve `cel.trabalha`; sem célula, calcula novamente. Nesse fallback, trabalho pode virar folga por feriado se `trabalha_em_feriado=False` e o ciclo for `6x1`, `5x2` ou `semanal`. O builder legacy consulta o mesmo método. Referências: `app/ponto/services/cartorio.py:931`, `app/escala/utils.py:1548`, `app/escala/utils.py:1550`, `app/escala/utils.py:725`, `app/escala/models.py:32`, `app/escala/models.py:1071`, `app/escala/models.py:1073`, `app/escala/models.py:1340`, `app/escala/models.py:1358`, `app/escala/models.py:1362`.

- **Consulta explícita dos fatos do status:** somente quando `veredito == 'indefinida'`, `fatos_do_status` chama `fatos_do_dia` e `folga_humana_ou_feriado`. A2 consulta os feriados aplicáveis, testa a data e aplica precedência. Se o vínculo trabalha em feriado e o dia é previsto, o feriado fica apenas no pagamento; caso contrário, participa como candidato à natureza do dia. Referências: `app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:126`, `app/ponto/precedencia.py:209`, `app/ponto/precedencia.py:245`, `app/ponto/precedencia.py:299`, `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:335`, `app/ponto/precedencia.py:349`.

- **Fonte territorial:** essas consultas usam `core.models.Feriado`, com alcance nacional, estadual ou municipal. O lugar é `colaborador.posto.municipio`; sem município, só o nacional alcança. A2 também consulta o nome do feriado. Referências: `app/core/feriados.py:27`, `app/core/feriados.py:60`, `app/core/feriados.py:62`, `app/core/feriados.py:64`, `app/core/feriados.py:91`, `app/core/feriados.py:106`, `app/ponto/precedencia.py:364`.

- **Depois do julgamento unitário:** `julgar_celula` também chama `aplicar_precedencia_na_celula`, que consulta A2 e carimba a precedência na ata. Essa chamada acontece depois do julgamento e não recalcula o status. Referências: `app/ponto/services/cartorio.py:1674`, `app/ponto/services/cartorio.py:1675`, `app/ponto/services/cartorio.py:1718`, `app/ponto/services/cartorio.py:1723`.

O resultado de `indefinida + folga_humana` é `folga` sem batida ou `folga_trabalhada` com batida; sem natureza humana vencedora, é `escala_suspeita`. Nos demais vereditos, a classificação lê `tipo_dia`/previsto da grade. Decisão do admin tem prioridade própria. Referências: `app/ponto/status_dia.py:83`, `app/ponto/status_dia.py:88`, `app/ponto/status_dia.py:93`, `app/ponto/status_dia.py:102`, `app/ponto/portas/celula.py:694`.

**3. `isento_ponto`, hash e rejulgamento**

A isenção é lida nestes pontos da lavra:

- A grade consulta `core.constants.isento_ponto(colab)` e aplica o preenchimento presumido dos marcos — `app/escala/utils.py:865`, `app/escala/utils.py:872`, `app/escala/utils.py:1790`.
- O cartório passa `c.isento_ponto` ao classificador nos dois ramos; o classificador retorna sem acusações para isento — `app/ponto/services/cartorio.py:1029`, `app/ponto/services/cartorio.py:1212`, `app/ponto/supra_juiz.py:195`.
- O cartório passa a flag aos fatos do status no ramo normal, no nunca-bateu e na complementação D5 — `app/ponto/services/cartorio.py:734`, `app/ponto/services/cartorio.py:1046`, `app/ponto/services/cartorio.py:1563`.
- Os fatos excluem marcos presumidos `isencao` da contagem de batidas; a regra usa a isenção na cobertura parcial, no `ok` sem batida e no rótulo — `app/ponto/status_dia.py:85`, `app/ponto/status_dia.py:95`, `app/ponto/status_dia.py:107`, `app/ponto/status_dia.py:130`.

Hoje, `impressao_insumos` calcula SHA-256 das partes concatenadas com `|`, contendo:

- Batidas: `pk:tipo:timestamp.isoformat()`;
- Status da cobertura;
- Chamados: `pk:status_local`;
- Somente `dna_versao` e `marcos` do DNA;
- Condicionalmente: `veto`, `teto`, versão `regra`, `enc`, quantidade de `fio` e cobertura parcial, inclusive seu estado de período quando fornecido.

Referências: [app/ponto/services/cartorio.py:324](/home/ronald/codex_ws/ae0502e59fd6dc6cc647154e864643a7158a30b3/app/ponto/services/cartorio.py:324), `app/ponto/services/cartorio.py:333`, `app/ponto/services/cartorio.py:339`, `app/ponto/services/cartorio.py:344`, `app/ponto/services/cartorio.py:352`, `app/ponto/services/cartorio.py:362`, `app/ponto/services/cartorio.py:377`, `app/ponto/services/cartorio.py:385`, `app/ponto/services/cartorio.py:390`.

**`isento_ponto`, feriado e `CelulaDia.trabalha` não entram diretamente no hash.** Há testes `expectedFailure` específicos para essas lacunas. A versão `regra` representa versão do código, não a régua CCT. Referências: `app/ponto/tests/test_d3_impressao_cobre_insumos.py:167`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:201`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:213`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:255`.

O hash persistido é `CelulaDia.impressao`. O cartório compara-o ao hash atual nos dois ramos: igualdade, julgamento anterior e veredito permitem pular; divergência conduz ao julgamento normal. Com `apply_`, lavra veredito/status/ata e grava o novo hash e carimbos. Isso não regenera o DNA. Referências: `app/escala/models.py:1713`, `app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1559`, `app/ponto/services/cartorio.py:1579`, `app/ponto/portas/celula.py:800`.

Na operação D5, hash divergente **não provoca rejulgamento naquela execução**: a célula entra em `status_fila_cartorio`. Hash em dia e status ausente permitem completar só o status. Referências: `app/ponto/services/cartorio.py:719`, `app/ponto/services/cartorio.py:729`, `app/ponto/services/cartorio.py:1158`.

A porta `carimbar` também lê o hash para evitar gravação repetida; `celula_veredito_velho` o lê para snapshot diagnóstico. Referências: `app/ponto/portas/celula.py:796`, `app/ponto/management/commands/celula_veredito_velho.py:110`.

Os **goldens literais** estão em `test_o219_versao_da_regra.py`: cinco casos — vazio, uma batida, duas batidas + chamado, veto e teto. Dois testes exigem os valores exatos, inclusive com `regra=None`; outro preserva o golden fora do horizonte. Referências: `app/ponto/tests/test_o219_versao_da_regra.py:39`, `app/ponto/tests/test_o219_versao_da_regra.py:47`, `app/ponto/tests/test_o219_versao_da_regra.py:94`, `app/ponto/tests/test_o219_versao_da_regra.py:99`, `app/ponto/tests/test_o219_versao_da_regra.py:141`.

**4. Competência exportada**

A recusa fica no **command `lavrar_status_competencia`**: `if competencia_entregue(emp.pk, mes, ano)`, imprime `ENTREGUE` e retorna antes de chamar `julgar_colab(... completar_status=True)`. Referências: [app/ponto/management/commands/lavrar_status_competencia.py:40](/home/ronald/codex_ws/ae0502e59fd6dc6cc647154e864643a7158a30b3/app/ponto/management/commands/lavrar_status_competencia.py:40), `app/ponto/management/commands/lavrar_status_competencia.py:55`.

O predicado exige **`ExportacaoDominio` vigente e não invalidada para empresa/mês/ano**. Vigente é o registro mais recente por `(gerado_em, pk)`; não basta existir uma exportação histórica. Referências: `app/folha/export.py:569`, `app/folha/export.py:572`, `app/folha/models.py:209`, `app/folha/models.py:220`.

Essa guarda não é universal no serviço genérico: `julgar_colab` e `julgar_celula` não consultam `competencia_entregue` nessas entradas. O teste D5 verifica especificamente a recusa do command. Referências: `app/ponto/services/cartorio.py:739`, `app/ponto/services/cartorio.py:776`, `app/ponto/services/cartorio.py:1700`, `app/ponto/services/cartorio.py:1715`, `app/ponto/tests/test_e3_status_lavrado.py:287`.

Outros consumidores diretos do predicado:

| Consumidor | Uso e referência |
|---|---|
| `competencia_fechada_para_celula` | Compõe entrega **OU** holerite publicado — `app/folha/export.py:599`. |
| `txt_vigente_x_gravado` | Obtém o TXT vigente — `app/folha/export.py:636`. |
| Preview de exportação | Lê o registro da entrega — `app/folha/views.py:354`. |
| Ranking HE | Define carimbo lavrado — `app/folha/services/ranking_he.py:89`. |
| Esmeril | Limita a busca retroativa — `app/ponto/services/esmeril_espelho.py:58`. |
| Marcos da competência | Define `exportada` — `app/ponto/services/fechamento.py:1505`. |
| HE pendente lavrado | Seleciona competência anterior ainda não entregue — `app/ponto/services/he_pendente_lavrado.py:223`. |
| Vigia do fio | Consulta entrega da competência do dia — `app/chamados/services/vigia_fio.py:787`. |

O gerador usa indiretamente o predicado composto `competencia_fechada_para_celula`; essa proteção de geração/regeneração é distinta da guarda do command D5. Referências: `app/ponto/management/commands/gerar_celulas.py:173`, `app/ponto/management/commands/gerar_celulas.py:203`.

## 31/10 10:17 · sessao 2e5aa3a351d1 · - · - · origem=Ronald (tmux)

Questão 2 — caminho verificado, sem execução de testes: [nao verificado]

- **Não é exclusivamente DNA:** `status_da_lavra` é pura, recebe `tipo_dia`, batida, previsto, isenção, veredito e `folga_humana`; `fatos_do_status` extrai `tipo_dia`/minutos/batidas da grade, mas consulta a lei A2 **quando `veredito == 'indefinida'`**. Referências: `app/ponto/status_dia.py:57`, `:122`, `:125`, `:126`, `:130`, `:133`.

- **Ramo direto de feriado na composição dos fatos:** `fatos_do_status → fatos_do_dia → fatos_do_periodo`. A2 busca município do posto e feriados aplicáveis; pergunta `data in feriados`; usa `CelulaDia.trabalha` para o previsto (fallback aritmético se ausente); mantém trabalho quando `feriado and previsto is True and ec.trabalha_em_feriado`; nos demais casos inclui feriado e decide pela precedência. Referências: `app/ponto/precedencia.py:209`, `:239`, `:242`, `:245`, `:299`, `:305`, `:316`, `:322`, `:325`, `:330`, `:335`, `:349`. `folga_humana_ou_feriado` retorna verdadeiro quando a faixa vencedora é `feriado`: `app/ponto/precedencia.py:189`.

- **Efeito no status:** `indefinida + folga_humana` resulta em `folga` sem batida e `folga_trabalhada` com batida; sem esse fato resulta em `escala_suspeita`. Ausência integral e falta sem batida são avaliadas antes. Para veredito diferente de `indefinida`, a consulta A2 não ocorre; `tipo_dia='folga'` produz `folga` ou `folga_trabalhada`. Referências: `app/ponto/status_dia.py:83`, `:85`, `:88`, `:90`, `:91`, `:93`, `:102`.

- **Ramo upstream de feriado na grade:** cartório monta a grade (`app/ponto/services/cartorio.py:931`); o builder pergunta `eh_dia_trabalho` e usa marcos congelados (`app/escala/utils.py:1548`, `:1550`; leitura de DNA em `:84`, `:103`). `eh_dia_trabalho` devolve `cel.trabalha` quando existe célula; **só sem célula** chama cálculo vivo (`app/escala/models.py:1066`, `:1071`, `:1073`). Nesse fallback, `base is True` pode virar folga por feriado (`:1340`): exige vínculo que não trabalha em feriado e ciclo `6x1/5x2/semanal`, e consulta `core.feriados.eh_feriado` pelo município do posto (`:1357`, `:1358`, `:1360`, `:1362`, `:1363`; lista de ciclos em `:32`). A grade escreve `tipo_dia='folga'` ou `'trabalho'`: `app/escala/utils.py:1743`, `:1749`. O dispatcher também tem legacy: `app/escala/utils.py:2026`, `:2029`; legacy pergunta o mesmo método `eh_dia_trabalho`: `:725`.

- **Fonte do calendário de feriados:** consulta `core.models.Feriado`, nacional para todos, estadual pela UF e municipal por UF+município; município vem de `colaborador.posto.municipio`. Sem município, apenas nacional passa pelos predicados. Referências: `app/core/feriados.py:27`, `:29`, `:57`, `:60`, `:62`, `:64`, `:70`, `:82`, `:83`, `:91`, `:95`, `:106`, `:113`, `:114`. A2 também consulta o nome do feriado para o seu resultado: `app/ponto/precedencia.py:364` → `app/core/feriados.py:98`.

- **Porta/admin:** `_status_alvo` prioriza decisão do admin e chama `status_do_admin`, sem avaliar feriado; para cartório chama `status_da_lavra` com os fatos. Referências: `app/ponto/portas/celula.py:693`, `:694`, `:695`, `:698`; mapeamento admin em `app/ponto/status_dia.py:39`, `:51`.

- **Testes que documentam a distinção:** teste puro mostra feriado sobre célula de trabalho vencendo `indefinida` (`app/ponto/tests/test_e3_status_lavrado.py:110`, `:111`); teste de costura assegura A2 chamada para `indefinida`, sem nova chamada para `concorde` (`:134`, `:135`, `:137`, `:138`); integração cria feriado nacional e exige `(indefinida, folga)` (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:74`, `:76`, `:79`). Testes apenas lidos; resultado de execução [não verificado].

- **Limite relacionado ao hash:** há teste `expectedFailure` que registra que feriado novo não move a impressão, porque não entra diretamente nem pelo DNA congelado: `app/ponto/tests/test_d3_impressao_cobre_insumos.py:201`, `:203`, `:206`.

## 31/10 10:17 · sessao a200f0ac5780 · - · - · origem=Ronald (tmux)

**Q1 — a lista completa da L-084 fica em memória; não encontrei persistência dela em campo/tabela.** Ela é atributo do `@dataclass ResultadoMes`, não modelo ORM; nasce vazia por cálculo e é entregue no resultado. Referências: `app/ponto/motor_calculo_v2.py:218`, `app/ponto/motor_calculo_v2.py:239`, `app/ponto/motor_calculo_v2.py:2221`, `app/ponto/motor_calculo_v2.py:2657`. O comentário e a captura de instâncias no comando de diff explicitam que `recalcular_fechamento_mes` não a persiste: `app/ponto/management/commands/diff_reclassificar_partido.py:169`, `app/ponto/management/commands/diff_reclassificar_partido.py:188`.

Quem escreve na lista do motor:

- `_janela_he` acusa a L-084 do dia, com deduplicação por data e `causa='l084_do_dia'`: `app/ponto/motor_calculo_v2.py:1544`, `app/ponto/motor_calculo_v2.py:1552`, `app/ponto/motor_calculo_v2.py:1560`.
- `_acusa_cadastro_x_realidade` faz `append` das pontas fora da janela; os chamadores acusam chegada e saída: `app/ponto/motor_calculo_v2.py:1593`, `app/ponto/motor_calculo_v2.py:1608`, `app/ponto/motor_calculo_v2.py:1648`, `app/ponto/motor_calculo_v2.py:1686`.
- `aplicar_tolerancia` acusa as duas pontas distantes, incluindo o dia do juiz e deltas, e devolve `(0, 0)`: `app/ponto/motor_calculo_v2.py:1926`, `app/ponto/motor_calculo_v2.py:1930`, `app/ponto/motor_calculo_v2.py:1935`, `app/ponto/motor_calculo_v2.py:1943`.
- O fechamento apenas concatena as listas das fatias no resultado composto: `app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:278`.

Quem lê hoje:

- O espelho traduz o relato do motor em pendências de HE e expõe um subconjunto das chegadas acima do teto: `app/ponto/services/espelho.py:751`, `app/ponto/services/espelho.py:1263`.
- `e6_oraculo.Insumos.cxr()` transforma a lista em conjunto de datas; `dono()` passa a pertinência da data ao juiz de dono: `app/ponto/management/commands/e6_oraculo.py:150`, `app/ponto/management/commands/e6_oraculo.py:156`, `app/ponto/management/commands/e6_oraculo.py:178`.
- Os comandos de diagnóstico/diff contam ou capturam a lista: `app/ponto/management/commands/diff_janela_he.py:70`, `app/ponto/management/commands/diff_reclassificar_partido.py:188`. `pautas_discordantes` também chama `Ins...cxr()`: `app/ponto/management/commands/pautas_discordantes.py:102`.

Há equivalentes **também em memória** no calculador: `Censo.cxr` recebe as acusações de pontualidade (`app/ponto/calculador/periodo.py:286`, `app/ponto/calculador/periodo.py:790`); `Rubricas.detalhe['cadastro_x_realidade']` recebe a lista (`app/ponto/calculador/regras.py:857`). Apesar do JSONField `DiaPago.detalhe` existir (`app/ponto/models.py:378`), o lavrador de `DiaPago` copia apenas `Rubricas.valores` e `Rubricas.nao_decide`, não `Rubricas.detalhe`: `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:329`, `app/ponto/services/dia_pago.py:339`. Portanto esse caminho não persiste a CxR.

**A tela “Cadastro x Realidade” não é consumidora direta da lista completa do motor.** Sua `lista()` lê assinaturas do esmeril por `ler_lavra()` e combina fontes de cadastro: `app/escala/services/cadastro_realidade.py:560`, `app/escala/services/cadastro_realidade.py:635`, `app/escala/services/cadastro_realidade.py:639`. O esmeril grava outra estrutura em `MetricaSnapshot.detalhe` e lê o snapshot mais recente: `app/ponto/services/esmeril_espelho.py:324`, `app/ponto/services/esmeril_espelho.py:330`, `app/ponto/services/esmeril_espelho.py:345`.

Existem persistências **derivadas**, sem guardar a lista completa: a C9 do teto armazena por item o `dono` obtido de `Ins...dono(d)` (`app/ponto/services/teto_na_lista.py:86`) e grava o documento em `MetricaSnapshot.detalhe` (`app/ponto/services/teto_na_lista.py:113`). Isso conserva classificação do juiz para certos dias, não a lista `dias_cadastro_x_realidade` com todos os relatos.

Cuidado com comentário envelhecido em `app/ponto/calculador/periodo.py:281`: ele diz que o campo de produção carrega “só causas de janela de HE”, mas o código atual também escreve a acusação de `aplicar_tolerancia` e a causa `l084_do_dia`: `app/ponto/motor_calculo_v2.py:1930`, `app/ponto/motor_calculo_v2.py:1561`.

## 31/10 10:17 · sessao ee46650b7bc1 · - · - · origem=Ronald (tmux)

4. A recusa fica no **command** `lavrar_status_competencia`: após contar as células, faz `if competencia_entregue(emp.pk, mes, ano)`, imprime `ENTREGUE` e retorna. Só depois dessa guarda chama `julgar_colab(... completar_status=True)`. Referências: `app/ponto/management/commands/lavrar_status_competencia.py:39`, `:40`, `:41`, `:42`, `:55`.

O predicado é **existe ExportacaoDominio vigente e não invalidada para empresa+mês+ano**: filtra a tripla, passa por `anotar_vigencia` e `so_vigentes`, rejeita o vigente com `invalidada_em` preenchido e retorna booleano; `registro=True` devolve o registro ou `None`. Referência: `app/folha/export.py:569–575`. O vigente é o maior `(gerado_em, pk)` de cada tripla, não simplesmente qualquer exportação existente: `app/folha/models.py:209–225`; `so_vigentes` lê `e_vigente`: `app/folha/models.py:192–200`.

Essa guarda **não está no serviço genérico do cartório**: `julgar_colab` entra em `_julgar_colab_corpo` e trata `completar_status` sem consultar `competencia_entregue` (`app/ponto/services/cartorio.py:739–778`); a própria documentação adverte que uma chamada com janela ampla/`--forcar` alcançaria competência exportada (`app/ponto/services/cartorio.py:756–759`). `julgar_celula` guarda futuro, inexistência e turno ainda aberto e depois chama `julgar_colab`, sem guarda de exportação nessa entrada (`app/ponto/services/cartorio.py:1700–1717`).

O teste específico do bloqueio é `test_D5_competencia_entregue_nao_se_toca`: simula `competencia_entregue=True`, executa o lote com `--apply`, exige `ENTREGUE` e que nenhum `status_dia` tenha sido preenchido (`app/ponto/tests/test_e3_status_lavrado.py:287–294`). Os testes do predicado cobrem sem TXT, vigente, vigente revogado, substituição e empate de carimbo (`app/folha/tests/test_k6_competencia_entregue.py:60–91`). Execução desses testes: [não verificado].

Outros consumidores diretos de `competencia_entregue` encontrados:

- `competencia_fechada_para_celula`: compõe entrega **OU** holerite do colaborador com lote publicado; `app/folha/export.py:598–609`. O gerador lê esse composto em `_competencias_exportadas` e `fonte_da_lavra`: `app/ponto/management/commands/gerar_celulas.py:173`, `:203`.
- `txt_vigente_x_gravado`: obtém o TXT vigente para comparar com o gravado atual; `app/folha/export.py:636`, `:648–658`.
- Preview da exportação: mostra quem/quando exportou; `app/folha/views.py:354`, `:360–364`.
- Ranking HE `_carimbo`: devolve `lavrado` quando todas as empresas representadas foram entregues; `app/folha/services/ranking_he.py:77–91`.
- `esmeril_espelho.janela`: encerra a busca retroativa na primeira competência entregue; `app/ponto/services/esmeril_espelho.py:55–63`.
- `marcos_da_competencia`: calcula o marco `exportada`; `app/ponto/services/fechamento.py:1500–1512`.
- `he_pendente_lavrado.competencias_a_lavrar`: inclui a anterior apenas enquanto não entregue; `app/ponto/services/he_pendente_lavrado.py:220–225`.
- `vigia_fio.competencia_do_dia_entregue`: converte o dia na competência pela janela da empresa e consulta o predicado, com memo opcional; `app/chamados/services/vigia_fio.py:787–797`. Seus leitores são o censo por dono (`app/chamados/services/vigia_fio.py:769`), a cerca do reconciliador (`app/chamados/reconciliador.py:715–720`), a seleção de perguntas vivas no pós-estado do chamado (`app/chamados/models.py:332–350`) e a guarda da costura para destino terminal (`app/chamados/services/fio.py:765–771`).

## 31/10 09:43 · sessao 60db60690637 · DISJUNTOR-B3-TOPO · verificador · origem=Code

O delta corrige os exemplos procedentes da triagem, mas **não garante equivalência entre a identificação no contador e no topo**: os reconhecedores continuam diferentes (`bin/disjuntor.py:84`, `bin/disjuntor.py:475`, `bin/disjuntor.py:494`).

1. **Resposta aos procedentes**

| Procedente | Resultado no código atual |
|---|---|
| Branco depois da primeira linha encerrava o bloco | **Resolvido:** branco agora apenas avança; não encerra (`bin/hook_stop_fila1.py:433`). |
| `O9 (C)` e `O9 NOTURNO-FOO` calavam B3 de `O9` | **Resolvido nesses exemplos:** cada token incorpora sua cauda pela `_identidade`; a comparação exige pertinência exata ao conjunto (`bin/disjuntor.py:488`, `bin/disjuntor.py:505`, `bin/hook_stop_fila1.py:417`). |
| Falta de caso para lookbehind | **Coberto:** testa `PRE-OBRA-X` e `PRE_OBRA-X`; há mutante retirando lookbehind (`bin/tests/test_disjuntor.sh:349`, `bin/tests/test_disjuntor.sh:648`, `bin/tests/test_disjuntor.sh:779`). |
| Falta de caso para DESENHO e ID em linhas cruzadas | **Coberto:** cenário cruzado e mutante correspondente (`bin/tests/test_disjuntor.sh:351`, `bin/tests/test_disjuntor.sh:781`). |
| Falta de positivo na primeira linha | **Coberto:** pergunta na linha 1; mutante restringe a leitura à linha 2 (`bin/tests/test_disjuntor.sh:345`, `bin/tests/test_disjuntor.sh:782`). |
| Falta de cobertura para leitura parcial após `OSError` | **Coberto:** preserva o trecho lido, verifica também `_linha_do_topo` e testa erro antes de qualquer linha; há mutante (`bin/hook_stop_fila1.py:443`, `bin/tests/test_disjuntor.sh:665`, `bin/tests/test_disjuntor.sh:788`). |
| Positivo aceitava apenas ausência do recado | **Resolvido no selo:** exige JSON com `decision=block`, motivo iniciado por `siga: `, ausência do recado e nenhuma nova `FALHA`; há mutante que lança exceção (`bin/tests/test_disjuntor.sh:337`, `bin/tests/test_disjuntor.sh:791`). |

2. **Onde a identidade ainda falha**

**A composição usa a mesma função; o reconhecimento não é equivalente.** O contador chama `_identidade` em `bin/disjuntor.py:482`; o topo chama em `bin/disjuntor.py:506`. Entretanto, o contador aceita markdown com `_`, enquanto o lookbehind do topo impede um ID imediatamente precedido por `_` (`bin/disjuntor.py:84`, `bin/disjuntor.py:494`).

Contraexemplos confirmados em memória:

| Entrada | Contador | Topo |
|---|---|---|
| Prompt `Leitura final da obra _OBRA-X_`; pergunta `PERGUNTA DE DESENHO _OBRA-X_` | Produz `OBRA-X` | Produz conjunto vazio; **cobra pergunta já escrita nessa forma** (`bin/disjuntor.py:84`, `bin/disjuntor.py:494`, `bin/hook_stop_fila1.py:417`). |
| Prompt `obra O9a`; pergunta `DESENHO O9a`, para contador disparado de `O9` | Rejeita `O9a`: captura com `re.I`, depois reprova no `fullmatch` | Extrai `O9`; **cala seu recado** (`bin/disjuntor.py:84`, `bin/disjuntor.py:475`, `bin/disjuntor.py:494`, `bin/hook_stop_fila1.py:417`). |

Portanto, compartilhar `_identidade` não demonstra a promessa de “mesmas bordas”: há tanto rejeição indevida quanto aceitação divergente (`bin/disjuntor.py:491`, `bin/disjuntor.py:494`). Ocorrência desses exemplos em produção: **[nao verificado]**.

3. **Quem escreve, quem lê e a primeira linha**

Nos caminhos auditados, o construtor recebe a instrução de escrever a pergunta; o PreToolUse apenas emite essa instrução, e o Stop consulta o topo (`bin/disjuntor.py:941`, `bin/hook_stop_fila1.py:1049`). A escrita efetiva pelo construtor em produção é **[nao verificado]**.

O contador recebe a identidade de `_obra_de`, acumula por chave de obra e persiste pelo `_com_estado` em `logs/sessao/disjuntor_estado.json`; Stop e PreToolUse chamam a varredura (`bin/disjuntor.py:54`, `bin/disjuntor.py:521`, `bin/disjuntor.py:556`, `bin/disjuntor.py:244`, `bin/hook_stop_fila1.py:1050`, `bin/disjuntor.py:940`).

**O delta não muda qual primeira linha é devolvida para um mesmo conteúdo:** `_linha_do_topo` continua retornando `b[0]`; pular brancos passa a encontrar a segunda linha útil, preservando a primeira capturada (`bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:433`, `bin/hook_stop_fila1.py:439`). O ramo atual do teto libera sem escrever PAREI no RELATO (`bin/hook_stop_fila1.py:1287`, `bin/hook_stop_fila1.py:1295`).

**No selo há reescrita de arquivos de fixture**, inclusive colocando a pergunta na primeira linha: `_topo` sobrescreve o RELATO de teste; o cenário com branco também o sobrescreve diretamente (`bin/tests/test_disjuntor.sh:72`, `bin/tests/test_disjuntor.sh:345`, `bin/tests/test_disjuntor.sh:347`).

Os leitores diretos são:

| Função lida | Chamadores |
|---|---|
| `_bloco_do_topo` | `_linha_do_topo`; `_disjuntor_contido`, para recados B3 (`bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:1049`). |
| `_linha_do_topo` | `_esperando_com_prova`; `_orcamento_travado`; `_orcamento_contido`, na recuperação de erro; `_disjuntor_contido`, para B4; `_parei_do_em_curso` (`bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:909`, `bin/hook_stop_fila1.py:1026`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1106`). |

4. **Casos, mutantes e lacunas**

Os procedentes têm casos e mutantes correspondentes, conforme a tabela acima; branco e cauda estão ligados aos mutantes específicos em `bin/tests/test_disjuntor.sh:783`, `bin/tests/test_disjuntor.sh:789` e `bin/tests/test_disjuntor.sh:790`.

Executei em memória o corpo Python de `b3_topo`: a base passa; os mutantes declarados `sem-lookbehind`, `parcial-vira-none`, `sem-cauda` e `regex-paralela` caem, respectivamente, nos casos 7, 10, 2 e 3 (`bin/tests/test_disjuntor.sh:648`, `bin/tests/test_disjuntor.sh:666`, `bin/tests/test_disjuntor.sh:643`, `bin/tests/test_disjuntor.sh:644`). A queda dos demais no selo integral é **[nao verificado]**.

**Nem todo ramo novo está protegido.** Mutar `_identidade` de `if cauda and '-' not in oid` para `if cauda` faz `obra OBRA-X (C)` virar `OBRA-X (C)`, contrariando o comportamento atual; mesmo assim, `b3_topo` passa em memória porque falta o caso de ID hifenado com cauda válida (`bin/disjuntor.py:488`, `bin/tests/test_disjuntor.sh:642`). Sobrevivência ao selo completo: **[nao verificado]**.

Também falta caso para normalização de cauda minúscula: remover `.upper()` não é distinguido pelos exemplos de cauda usados, todos já maiúsculos (`bin/disjuntor.py:488`, `bin/tests/test_disjuntor.sh:384`, `bin/tests/test_disjuntor.sh:642`, `bin/tests/test_disjuntor.sh:645`). Sobrevivência ao selo completo: **[nao verificado]**.

Não executei o selo integral: ele cria, altera e remove arquivos, contrariando a restrição desta verificação (`bin/tests/test_disjuntor.sh:37`, `bin/tests/test_disjuntor.sh:49`, `bin/tests/test_disjuntor.sh:697`, `bin/tests/test_disjuntor.sh:706`).

## 31/10 09:42 · sessao fbd435b3bfa8 · E3-STATUS · verificador · origem=Code

**1. Sim: o delta resolve o achado A por inspeção do código.** O cartório obtém `_par_da_lavra` pela função da própria porta e passa seu veredito efetivo a `fatos_do_status`. Assim, `indefinida` derivada dos códigos ativa a consulta A2 que antes era omitida. (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1564`; `app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:126`.)

**2. Quem produz, quem lê e igualdade com a lavra:**

| Chamador | Quem produz o argumento | É o veredito gravado/preservado? |
|---|---|---|
| Lavra normal | `classificar_dia` produz os códigos; o cartório define `_vd`; **`veredito_que_a_lavra_escreve`** resolve o par efetivo, incluindo preservação de decisão admin. (`app/ponto/services/cartorio.py:1207`, `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1245`, `app/ponto/services/cartorio.py:1252`; `app/ponto/portas/celula.py:655`.) | **Sim, no fluxo sequencial inspecionado:** o adaptador recebe `_par_da_lavra[0]`; a porta usa novamente a mesma função com `_vd/cods`. (`app/ponto/services/cartorio.py:1559`, `app/ponto/services/cartorio.py:1564`; `app/ponto/portas/celula.py:745`.) |
| Nunca-bateu | `classificar_dia` → `derivar_veredito`; o cartório substitui por `nunca_bateu` quando falta cobertura integral. (`app/ponto/services/cartorio.py:1026`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1041`.) | **Sim para célula sem decisão admin:** `_vd_nb` alimenta ambos. **Com admin, pode diferir:** o adaptador recebe `_vd_nb`, mas a porta preserva `cel.veredito` e calcula o status humano, ignorando esses fatos. (`app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:1047`; `app/ponto/portas/celula.py:739`, `app/ponto/portas/celula.py:694`.) |
| `_completar_status` | Lê **`cel.veredito`**, escrito pela lavra anterior; passa-o à porta e ao adaptador. (`app/ponto/services/cartorio.py:733`, `app/ponto/services/cartorio.py:735`; `app/ponto/portas/celula.py:770`.) | **Sim:** seus chamadores exigem veredito preenchido para fornecer um dia elegível, evitando derivação alternativa por valor vazio. (`app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1002`, `app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1159`.) |

O **leitor do argumento do adaptador** é `fatos_do_status`: usa-o para decidir consultar A2 e não o inclui no dicionário retornado. O **leitor que decide o status** é `status_da_lavra`, chamado pela porta com seu próprio `v` efetivo; esse mesmo `v` é atribuído à célula. Não identifiquei, nesses fluxos sequenciais, status decidido com outro veredito; a ressalva admin acima muda o argumento do adaptador, mas seus fatos são ignorados. (`app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:133`; `app/ponto/portas/celula.py:694`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:752`, `app/ponto/portas/celula.py:770`.)

**3. O teste novo morde a reversão, por análise estática, e atravessa a implementação real.** Chama `julgar_colab(... apply_=True, forcar=True)` e relê a célula persistida; usa o classificador real e a porta real. Há congelamento do relógio, sem mock dessas funções no teste. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:29`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:65`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:66`; `app/ponto/services/cartorio.py:767`; `app/escala/models.py:1794`.)

Os dois RED exigem `PREVISTO_COM_DIA_INDEFINIDO` e `('indefinida', 'folga')`. Com esses códigos, `_vd=None`; reverter a linha para `_vd` deixa `folga_humana=False`, enquanto a porta continua derivando `indefinida`. A regra então retorna `escala_suspeita`, contrariando ambos RED; o controle permanece compatível. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:78`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:79`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:86`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:87`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:72`; `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1221`; `app/escala/models.py:1583`; `app/ponto/status_dia.py:121`, `app/ponto/status_dia.py:91`.)

**4. Nenhuma das cinco classes aparece como defeito novo deste delta:**

| Classe | Avaliação |
|---|---|
| Segundo juiz | Reutiliza a autoridade da lavra e consulta A2 pelo adaptador existente; A2 está registrada. (`app/ponto/services/cartorio.py:1252`; `app/ponto/portas/celula.py:745`; `app/ponto/status_dia.py:126`; `app/core/juizes.py:161`.) |
| Segundo escritor fora da porta | Guarda apenas o par local/transitório; veredito e status continuam escritos na porta. (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1253`; `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:770`.) |
| Relógio fora da borda | Os trechos alterados não acrescentam leitura do relógio; a lavra recebe `agora`. (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1559`, `app/ponto/services/cartorio.py:1564`.) |
| Derivação paralela | Calcular novamente pela **mesma função** mantém uma autoridade; o delta não copia a regra de derivação. (`app/ponto/services/cartorio.py:1252`; `app/ponto/portas/celula.py:745`.) |
| Selo/RED sem mordida | O selo AST continua aceitando qualquer `fatos_do_status=f`; o teste novo cobre semanticamente a reversão específica. (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:287`; `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:79`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:87`.) |

Execução GREEN, execução do mutante, efeitos concorrentes, ocorrência/quantidade em produção e confirmação independente do hash Git: **[não verificado]**.

## 31/10 09:42 · sessao 2d7c58b7082a · - · - · origem=Ronald (tmux)

As duas rotas chamam `_identidade`: contador em `bin/disjuntor.py:482`; topo em `bin/disjuntor.py:506`; composição em `bin/disjuntor.py:488`. Porém, isso não torna os reconhecedores equivalentes: o contador aceita `_` como markdown e usa `re.I`, seguido de `fullmatch`; o topo usa lookbehind sem `re.I` (`bin/disjuntor.py:84`, `bin/disjuntor.py:475`, `bin/disjuntor.py:494`).

Contraexemplos confirmados com Python `-B`, somente funções puras:

- `_obra_de('Leitura final da obra _OBRA-X_') == 'OBRA-X'`, mas `obras_da_linha('PERGUNTA DE DESENHO _OBRA-X_') == set()`; a pergunta é rejeitada. O `_` é permitido no contador e proibido imediatamente antes do ID no topo (`bin/disjuntor.py:84`, `bin/disjuntor.py:494`, `bin/hook_stop_fila1.py:417`).
- `_obra_de('obra O9a') is None`, enquanto `obras_da_linha('DESENHO O9a') == {'O9'}`; a pergunta cala B3 de `O9`. O contador captura `O9a` com `re.I` e rejeita no `fullmatch`; o topo captura somente `O9`, sem borda direita (`bin/disjuntor.py:84`, `bin/disjuntor.py:475`, `bin/disjuntor.py:494`).

Não encontrei identidade válida emitida pelo contador cujo texto cru, sem markdown/contexto adicional, não faça roundtrip pelo topo; demonstração exaustiva: [nao verificado].

Mutante plausível: remover `.upper()` de `_identidade`. O selo usa somente caudas já maiúsculas, `(C)` e `NOTURNO-FOO`; portanto não verifica a normalização de `(c)` nem de sufixo minúsculo (`bin/disjuntor.py:488`, `bin/tests/test_disjuntor.sh:384`, `bin/tests/test_disjuntor.sh:642`, `bin/tests/test_disjuntor.sh:645`). Hoje `P('O9 (C)', ['DESENHO O9 (c)'])` é verdadeiro e esse mutante o tornaria falso; sobrevivência no selo completo: [nao verificado].

## 31/10 09:42 · sessao 661999cfe410 · - · - · origem=Ronald (tmux)

No delta auditado, nenhuma das cinco classes se sustenta como defeito novo, por inspeção estática; execução dos testes e mutante **[não verificado]**. [nao verificado]

| Classe | Parecer e prova |
|---|---|
| Segundo juiz registrado | **Não.** A costura pede o veredito à autoridade já usada pela lavra: `app/ponto/services/cartorio.py:1252`; `app/ponto/portas/celula.py:745`. A2 continua chamada pelo adaptador (`app/ponto/status_dia.py:126`) e registrada em `app/core/juizes.py:161`. |
| Segundo escritor fora da porta | **Não.** O delta guarda uma tupla local e no mapa transitório (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1253`); a porta escreve status e veredito (`app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:770`). |
| Relógio fora da borda | **Não acrescenta leitura.** A lavra recebe `agora` (`app/ponto/services/cartorio.py:1559`); o teste fixa o relógio (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:29`). As leituras em `julgar_celula` são preexistentes ao delta fornecido (`app/ponto/services/cartorio.py:1691`, `app/ponto/services/cartorio.py:1712`, `app/ponto/services/cartorio.py:1715`). |
| Derivação paralela da fonte única | **Não.** O resultado de `veredito_que_a_lavra_escreve` alimenta os fatos (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1564`); a porta chama a mesma regra (`app/ponto/portas/celula.py:745`). Continua existindo o predicado inline da tela, anterior ao delta (`app/colaboradores/services/calendario.py:801`), com migração E4 explicitada (`app/ponto/precedencia.py:185`). |
| Selo/RED sem mordida | **O selo AST continua limitado:** exige apenas presença do argumento (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:166`) e aceita `fatos_do_status=f` (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:287`). **O teste novo tem mordida estática sobre a reversão:** usa julgamento real com apply e releitura da célula (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:65`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:66`), exigindo `indefinida/folga` em ambos RED (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:79`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:87`). Reverter a `_vd` suprime A2, pois `_vd` é `None` nesse cenário com códigos (`app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1221`), e o adaptador só pergunta com `indefinida` (`app/ponto/status_dia.py:122`); sem esse fato, a regra retorna `escala_suspeita` (`app/ponto/status_dia.py:91`). |

Entre cálculo do par e lavra, o corpo não reatribui `_vd`, `cods`, `cel.veredito` ou `cel.veredito_via`; as entradas da lavra continuam `_vd/cods` (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1559`). Existem chamadas intermediárias a reconciliação, ressuscitação e emissão (`app/ponto/services/cartorio.py:1281`, `app/ponto/services/cartorio.py:1415`, `app/ponto/services/cartorio.py:1508`); a ausência de efeitos transitivos em todos os sinais/concorrência **[não verificado]**.

## 31/10 09:41 · sessao 3f3aa9587f73 · - · - · origem=Ronald (tmux)

Os procedentes têm cenário e mutante correspondente:

| Achado | Caso | Mutante |
|---|---|---|
| Branco entre linhas úteis | `bin/tests/test_disjuntor.sh:347` | `bin/tests/test_disjuntor.sh:783` |
| Cauda distingue obra | `bin/tests/test_disjuntor.sh:642`–`:647` | `bin/tests/test_disjuntor.sh:789`–`:790` |
| Lookbehind | `bin/tests/test_disjuntor.sh:648` | `bin/tests/test_disjuntor.sh:779` |
| DESENHO e ID em linhas cruzadas | `bin/tests/test_disjuntor.sh:351` | `bin/tests/test_disjuntor.sh:781` |
| Positivo na primeira linha | `bin/tests/test_disjuntor.sh:345` | `bin/tests/test_disjuntor.sh:782` |
| Leitura parcial após OSError | `bin/tests/test_disjuntor.sh:665`–`:669` | `bin/tests/test_disjuntor.sh:788` |
| Exceção não vale como positivo | `_b3_cala` verifica JSON, recado e FALHA, `bin/tests/test_disjuntor.sh:337`–`:341` | `bin/tests/test_disjuntor.sh:791` |

Executei em memória o heredoc exato de `b3_topo`: base passa; os mutantes `sem-lookbehind`, `parcial-vira-none`, `sem-cauda` e `regex-paralela` caem respectivamente nos casos 7, 10, 2 e 3 (`bin/tests/test_disjuntor.sh:643`, `:644`, `:648`, `:666`, `:779`, `:788`–`:790`). Os 13 mutantes B3-topo têm âncora única e substituição sintaticamente válida (`bin/tests/test_disjuntor.sh:775`–`:794`).

**Há mutante plausível sobrevivente ao novo `b3_topo`:** trocar `if cauda and '-' not in oid` por `if cauda` em `bin/disjuntor.py:488`. Verificado em memória: `b3_topo` continua rc=0, mas `_obra_de('obra OBRA-X (C)')` muda de `OBRA-X` para `OBRA-X (C)`; os casos não incluem ID hifenado com cauda válida (`bin/tests/test_disjuntor.sh:642`–`:650`). Sobrevivência ao selo completo: **[nao verificado]**.

Não executei o selo integral: cria/remove árvore e grava mutantes (`bin/tests/test_disjuntor.sh:37`–`:49`, `:697`–`:706`).

## 31/10 09:41 · sessao 0cd79261025c · - · - · origem=Ronald (tmux)

| Chamador | Quem produz o argumento de `fatos_do_status(veredito=...)` | Relação com a célula |
|---|---|---|
| Lavra normal | `classificar_dia` produz `cods`; `_vd` começa `None`, podendo virar `cobrado`; `veredito_que_a_lavra_escreve` resolve `_par_da_lavra`, cujo primeiro elemento vai ao adaptador. (`app/ponto/services/cartorio.py:1207`, `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1245`, `app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1564`.) | Mesma autoridade e mesmas entradas da lavra: esta chama novamente `veredito_que_a_lavra_escreve`; sua regra preserva admin ou resolve `veredito or derivar_veredito(codigos)`. (`app/ponto/services/cartorio.py:1559`; `app/ponto/portas/celula.py:655`, `app/ponto/portas/celula.py:658`, `app/ponto/portas/celula.py:745`.) |
| Nunca-bateu | `classificar_dia` produz `_cods_nb`; `derivar_veredito` traduz; o cartório substitui por `nunca_bateu` quando falta cobertura integral. (`app/ponto/services/cartorio.py:1026`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1032`, `app/ponto/services/cartorio.py:1041`.) | O mesmo `_vd_nb` explícito alimenta lavra e adaptador. **Ressalva admin:** o adaptador ainda recebe `_vd_nb`, mas a porta preserva o veredito humano e deriva o status por `status_do_admin`, ignorando os fatos recebidos. (`app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:1047`; `app/ponto/portas/celula.py:738`, `app/ponto/portas/celula.py:739`, `app/ponto/portas/celula.py:694`.) |
| `_completar_status` | Lê `cel.veredito`, resultado de uma lavra anterior; passa esse valor tanto à porta quanto ao adaptador. (`app/ponto/services/cartorio.py:733`, `app/ponto/services/cartorio.py:735`; escritor: `app/ponto/portas/celula.py:770`.) | Os chamadores só fornecem `dia` elegível quando existe veredito preenchido; portanto, nesse fluxo não ocorre fallback de um veredito vazio para a derivação pelos protestos. (`app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1002`, `app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1159`, `app/ponto/services/cartorio.py:729`.) |

O argumento `veredito` do adaptador **apenas controla a consulta A2**: ele não integra o dicionário retornado. O leitor que decide o status, `status_da_lavra`, recebe da própria porta o `v` efetivo; esse mesmo `v` é atribuído a `cel.veredito`. Assim, nos caminhos sequenciais inspecionados, não aparece status decidido com um veredito diferente do gravado/preservado. A ressalva do ramo nunca-bateu/admin é diferença no argumento do adaptador, sem divergência do status, porque o caminho admin ignora esses fatos. (`app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:133`; `app/ponto/portas/celula.py:694`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:739`, `app/ponto/portas/celula.py:752`, `app/ponto/portas/celula.py:770`.)

Concorrência, valores históricos concretos e ocorrência em produção: **[não verificado]**.

## 31/10 09:41 · sessao 5e906c1afdbc · - · - · origem=Ronald (tmux)

**Morde a reversão por análise estática.** Os dois RED exigem o código `PREVISTO_COM_DIA_INDEFINIDO` e a tupla `('indefinida', 'folga')`; o controle exige `('indefinida', 'escala_suspeita')`. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:71`, `:72`, `:78`, `:79`, `:86`, `:87`.)

Com aquele código, `_vd` permanece `None`, pois só muda para `cobrado` quando não há códigos. Reverter a chamada para `_vd` deixa `folga_humana=False`; a porta continua derivando `indefinida` pelos códigos e entrega esse veredito à regra de status, que devolve `escala_suspeita`. Logo, os dois RED contradizem a saída do mutante; o controle permanece compatível. (`app/ponto/services/cartorio.py:1220`, `:1221`, `:1245`, `:1564`; `app/ponto/status_dia.py:121`, `:122`, `:88`, `:91`; `app/ponto/portas/celula.py:658`, `:745`, `:698`; `app/escala/models.py:1583`, `:1597`.)

**Exercita a implementação real.** `_lavrar` cria a célula, chama `julgar_colab(... apply_=True, forcar=True)` e relê a persistência com `refresh_from_db`; `julgar_colab` importa o classificador real e chama o corpo, que monta a grade real. O método de modelo delega à porta; a porta chama `status_da_lavra`. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:61`, `:65`, `:66`; `app/ponto/services/cartorio.py:767`, `:776`, `:931`; `app/escala/models.py:1794`; `app/ponto/portas/celula.py:698`.)

Não há mock de julgamento, A2, adaptador ou regra no teste; há congelamento do relógio com `freezegun`. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:17`, `:29`, `:32`, `:57`, `:74`, `:82`.)

Fixture coerente com o caminho: vínculo termina sete dias antes, célula mantém `trabalha=True` e geradora vencida, batida anterior garante adesão; a autoridade recusa a geradora fora da vigência, e o supra acusa cadastro sem exigir previsto positivo. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:49`, `:53`, `:62`; `app/ponto/services/cartorio.py:956`, `:964`; `app/escala/alimentacao.py:250`; `app/ponto/supra_juiz.py:168`, `:405`.)

Execução GREEN e execução do mutante: **[não verificado]**.

## 31/10 09:27 · sessao f29f24b0d110 · DISJUNTOR-B3-TOPO · verificador · origem=Code

**REPROVADO.** Há caminhos que liberam o Stop antes da cobrança B3 e uma colisão entre IDs admitidos pelo próprio contador (`bin/hook_stop_fila1.py:1224`, `bin/hook_stop_fila1.py:1231`, `bin/hook_stop_fila1.py:1298`; `bin/disjuntor.py:483`).

**(1) Onde o desenho falha**

- **ESPERANDO verdadeiro contorna a cobrança.** A ordem relevante é PAREI → ESPERANDO com prova → teto → B2 → B3/recados. Portanto, pergunta ausente ou na terceira linha não é cobrada quando uma saída anterior libera. `_libera()` encerra o processo (`bin/hook_stop_fila1.py:1224`, `:1229`, `:1282`, `:1293`, `:1298`, `:133`).
- **O selo não exercita essa espera verdadeira.** Os quatro casos novos usam `pid=1`; esse PID está sempre entre os ancestrais e é recusado como prova. Eles chegam à cobrança B3 pelo caminho “ESPERANDO sem prova” (`bin/tests/test_disjuntor.sh:335`, `:337`, `:339`, `:341`; `bin/hook_stop_fila1.py:477`, `:529`).
- **IDs hifenados semelhantes estão protegidos, mas IDs com cauda não.** A regex rejeita `PRE-OBRA-X` e `OBRA-X-OUTRA` para `OBRA-X`. Contudo, aceita `DESENHO O9 (C)` ou `DESENHO O9 NOTURNO-FOO` como pergunta de `O9`, embora `_obra_de()` produza três identidades distintas. Confirmei esses resultados em memória (`bin/hook_stop_fila1.py:414`–`:415`; `bin/disjuntor.py:81`–`:85`, `:483`–`:484`).
- **“Duas primeiras úteis” tem uma restrição adicional:** depois da primeira linha capturada, uma linha vazia encerra o bloco. `ESPERANDO`, branco, pergunta resulta em somente uma linha lida; pelo contrato literal de duas linhas úteis, a segunda pergunta deveria contar (`bin/hook_stop_fila1.py:430`–`:436`).
- **A validação é apenas lexical:** exige a substring maiúscula `DESENHO` e o ID na mesma linha. Aceita `REDESENHO OBRA-X` e `OBRA-X NAO TEM PERGUNTA DE DESENHO`, sem verificar QUAL PADRÃO/POR QUÊ (`bin/hook_stop_fila1.py:415`, `:1048`–`:1050`).

**(2) Quem escreve e quem lê**

| Dado | Escritor | Leitor |
|---|---|---|
| Pergunta no topo | O construtor, conforme instrução emitida pelo PreToolUse; no selo, `_topo()` grava a fixture (`bin/disjuntor.py:919`–`:921`; `bin/tests/test_disjuntor.sh:72`). | B3 lê `_bloco_do_topo()` e aplica `_pergunta_b3_escrita()` (`bin/hook_stop_fila1.py:1045`–`:1047`). |
| Primeira linha ESPERANDO/PAREI | Declaração manual; o teto atual apenas registra/libera, tendo abandonado a escrita automática de PAREI (`bin/hook_stop_fila1.py:1283`–`:1291`). | Ambos usam `_linha_do_topo()`, derivada do mesmo bloco (`bin/hook_stop_fila1.py:407`, `:652`, `:1102`). |
| Contador B3 | `varrer_b3()` acumula agentes reprovados por obra, deduplica e dispara no limiar; `_com_estado()` persiste em `logs/sessao/disjuntor_estado.json` (`bin/disjuntor.py:54`, `:533`–`:550`, `:242`–`:245`). | Stop e PreToolUse chamam a varredura; `b3_disparados()` consulta o estado (`bin/hook_stop_fila1.py:1046`; `bin/disjuntor.py:918`, `:566`). |
| Rearme B3 | CLI chama `rearma_b3()`, preserva agentes antigos em `rearmado` e zera contagem/disparo (`bin/disjuntor.py:1138`, `:560`–`:563`). | A varredura ignora esses agentes antigos (`bin/disjuntor.py:535`). |

Escrever a pergunta **cala o recado**, sem rearmar o B3: o quarto verificador continua negado pelo PreToolUse, que decide pela chave da obra sem consultar o topo (`bin/hook_stop_fila1.py:1047`; `bin/disjuntor.py:915`–`:922`).

`_rteto()` restaura outro contador, `logs/hook_stop_fila1.json`; ele não restaura o estado B3 (`bin/tests/test_disjuntor.sh:333`–`:334`; `bin/hook_stop_fila1.py:35`; `bin/disjuntor.py:54`).

**(3) O selo morde cada ramo novo? Não.**

| Comportamento | Cobertura existente |
|---|---|
| Aceitar segunda linha | Caso `:335`–`:336`; mutante `:715`. |
| Rejeitar terceira linha | Caso `:337`–`:338`; mutante `:718`. |
| Rejeitar `OBRA-X-OUTRA` | Caso `:339`–`:340`; mutante `:716`. |
| Exigir `DESENHO` | Caso `:341`–`:342`; mutante `:717`. |

Todas as referências dessa tabela são de `bin/tests/test_disjuntor.sh`.

Há mutantes que preservam os resultados desses casos:

- Retirar **somente o lookbehind** da regex aceita `PRE-OBRA-X`; o selo testa apenas a fronteira direita (`bin/hook_stop_fila1.py:414`; `bin/tests/test_disjuntor.sh:339`).
- Separar os testes em `any(DESENHO)` e `any(id)` aceita `['DESENHO OBRA-Y', 'OBRA-X']`; falta esse caso cruzado (`bin/hook_stop_fila1.py:415`; `bin/tests/test_disjuntor.sh:335`–`:342`).
- Limitar B3 à última linha lida perde a pergunta válida na primeira linha; todos os positivos novos a colocam na segunda (`bin/hook_stop_fila1.py:1045`; `bin/tests/test_disjuntor.sh:335`–`:342`).
- Reverter `return out or None` para `return None` perde a leitura parcial após `OSError`. Esse ramo novo não tem cenário nem mutante declarado; também afeta ESPERANDO/PAREI pelo leitor compartilhado (`bin/hook_stop_fila1.py:439`, `:407`; `bin/tests/test_disjuntor.sh:72`, `:715`–`:718`).

Além disso, o positivo exige somente ausência do recado: uma exceção convertida em `FALHA + None` pode satisfazê-lo. Falta conferir a decisão esperada e ausência de falha (`bin/tests/test_disjuntor.sh:336`; `bin/hook_stop_fila1.py:1057`–`:1062`).

Validei funções isoladas em memória. Não executei o selo, pois ele cria, altera e apaga arquivos; sobrevivência no selo completo: **[nao verificado]** (`bin/tests/test_disjuntor.sh:37`–`:49`, `:637`–`:646`).

## 31/10 09:25 · sessao 47865606b8e9 · - · - · origem=Ronald (tmux)

O selo cobre quatro comportamentos: pergunta na 2ª linha, rejeição na 3ª, rejeição de `OBRA-X-OUTRA` e rejeição de prosa sem `DESENHO`; os respectivos mutantes estão declarados. `bin/tests/test_disjuntor.sh:335`, `:337`, `:339`, `:341`, `:715`, `:716`, `:717`, `:718`.

Há lacunas concretas:

- **Fronteira esquerda:** remover somente `(?<![\w-])` mantém os resultados dos quatro casos atuais, mas aceita `DESENHO PREFIXO-OBRA-X`; o teste existente exercita somente a fronteira direita com `OBRA-X-OUTRA`. `bin/hook_stop_fila1.py:414`; `bin/tests/test_disjuntor.sh:339`.
- **DESENHO e ID na mesma linha:** trocar a expressão por dois `any` independentes mantém os casos atuais, mas aceita `['DESENHO OBRA-Y', 'OBRA-X']`. `bin/hook_stop_fila1.py:415`; `bin/tests/test_disjuntor.sh:335`.
- **Pergunta na primeira linha:** limitar somente o B3 à última linha do bloco passa pelos quatro exemplos atuais, todos com `ESPERANDO` na primeira, mas perde pergunta na primeira com texto neutro na segunda. `bin/hook_stop_fila1.py:1045`; `bin/tests/test_disjuntor.sh:335`, `:337`, `:339`, `:341`.
- **OSError após leitura parcial:** voltar `return out or None` para `return None` desfaz um ramo novo sem cenário correspondente; `_topo` sempre escreve arquivo regular completo. A mudança também afeta PAREI e ESPERANDO porque usam `_linha_do_topo`. `bin/hook_stop_fila1.py:439`, `:407`, `:652`, `:1102`; `bin/tests/test_disjuntor.sh:72`.
- **Exceção confundida com sucesso:** o positivo apenas procura ausência de B3; uma exceção exclusiva do reconhecimento da pergunta vira `FALHA + None` e satisfaz essa asserção. O helper de autorização positiva existe, mas esse caso não o usa. `bin/tests/test_disjuntor.sh:336`, `:91`; `bin/hook_stop_fila1.py:1057`.

Verifiquei os dois primeiros mutantes e o erro parcial executando funções extraídas por AST apenas em memória. Sobrevivência no selo completo: **[nao verificado]**. Executar o selo criaria e apagaria arquivos temporários, contrariando a restrição de leitura. `bin/tests/test_disjuntor.sh:37`, `:39`, `:49`, `:637`, `:646`.

## 31/10 09:25 · sessao 2142652f636b · - · - · origem=Ronald (tmux)

Mapa de produtores e consumidores:

| Dado | Escreve | Lê |
|---|---|---|
| Pergunta de DESENHO no topo | O construtor é o produtor exigido pela mensagem do PreToolUse; não é escrita automaticamente por esse ramo (`bin/disjuntor.py:919-922`). No selo, `_topo` escreve o RELATO fictício (`bin/tests/test_disjuntor.sh:72`). | `_bloco_do_topo` abre RELATO e captura até duas linhas (`bin/hook_stop_fila1.py:428-437`); B3 consulta esse bloco e chama `_pergunta_b3_escrita` (`bin/hook_stop_fila1.py:1045-1047`). |
| Primeira linha: ESPERANDO/PAREI | A declaração do topo é humana; o hook deixou de produzir `PAREI: hook-teto`, conforme comentário do ramo atual (`bin/hook_stop_fila1.py:1283-1291`). | `_linha_do_topo` retorna a primeira linha do mesmo bloco (`bin/hook_stop_fila1.py:407-408`); ESPERANDO lê em `:652-655`; PAREI lê em `:1102-1105`. |
| Contador B3 | `varrer_b3` coleta agentes reprovados (`bin/disjuntor.py:517-525`), acumula por obra e deduplica por `aid` (`:533-538`), marca disparado no limiar (`:540-548`). `_com_estado` grava o JSON atomicamente (`:242-245`), cujo caminho é `logs/sessao/disjuntor_estado.json` (`:54`). | Stop chama `varrer_b3` (`bin/hook_stop_fila1.py:1046`); PreToolUse também varre antes de decidir (`bin/disjuntor.py:915-918`); `b3_disparados` consulta o estado persistido (`:566-567`). |
| Rearme | CLI `--rearma-b3` (`bin/disjuntor.py:1138-1141`) chama `rearma_b3`, arquiva os IDs já contados em `rearmado` e zera lista/disparo (`:553-563`). | A próxima varredura ignora IDs rearmados (`bin/disjuntor.py:535-536`). |

A pergunta escrita cala o recado do Stop, mas não rearma o contador nem permite o quarto verificador: o Stop consulta o topo (`bin/hook_stop_fila1.py:1045-1050`), enquanto PreToolUse nega pela presença da chave da obra na varredura, sem consultar RELATO (`bin/disjuntor.py:915-922`).

Identidade da obra:

- No topo, as duas fronteiras `(?<![\w-])` e `(?![\w-])` impedem que `OBRA-X-OUTRA`, `OUTRA-OBRA-X` ou `_OBRA-X` satisfaçam `OBRA-X` (`bin/hook_stop_fila1.py:414-415`).
- O contador usa a obra extraída como chave exata; a gramática reconhece IDs hifenados inteiros (`bin/disjuntor.py:81-85`, `:470-485`) e PreToolUse usa pertinência exata da chave (`:918`).
- Há um resíduo fora dessa gramática: `obra OBRA-X_OUTRA` é extraída como `OBRA-X`, porque a regex externa não exige fronteira à direita e o `fullmatch` valida apenas o grupo já capturado (`bin/disjuntor.py:84-85`, `:475`). Reprodução feita em memória, sem importar o módulo inteiro ou escrever arquivos; `_` não integra a gramática declarada de IDs (`:81`). 

Os escritores automáticos de alarmes preservam o topo: `no_relato` insere na seção de alarmes ou antes da primeira seção `##`, e depois grava o arquivo (`app/core/esteira_vigia.py:863-884`). Outros leitores possuem janelas diferentes: handoff percorre até a primeira seção (`bin/handoff_sessao.sh:94-108`); alarme de ociosidade procura `PAREI:` nas primeiras 40 linhas físicas (`bin/alarme_sessao_ociosa.py:119-127`).

## 31/10 09:25 · sessao 9083f468370d · - · - · origem=Ronald (tmux)

1. **A cobrança B3 pode ser pulada no caminho real do Stop.** `main()` libera PAREI em `bin/hook_stop_fila1.py:1224` e ESPERANDO com prova em `bin/hook_stop_fila1.py:1229`; só acrescenta B3 ao recado em `bin/hook_stop_fila1.py:1298`. Portanto pergunta ausente ou na terceira linha não produz cobrança quando ESPERANDO tem prova; teto e B2 também podem liberar antes da varredura (`bin/hook_stop_fila1.py:1282`, `bin/hook_stop_fila1.py:1293`).

2. **O selo não cobre ESPERANDO com prova.** Seus quatro casos usam `pid=1` (`bin/tests/test_disjuntor.sh:335`, `bin/tests/test_disjuntor.sh:337`, `bin/tests/test_disjuntor.sh:339`, `bin/tests/test_disjuntor.sh:341`); PID 1 pertence sempre ao conjunto de ancestrais (`bin/hook_stop_fila1.py:477`) e é recusado (`bin/hook_stop_fila1.py:529`). Logo o cenário central “espera real + pergunta fora das duas linhas” permanece descoberto nesses testes (`bin/hook_stop_fila1.py:1230`, `bin/tests/test_disjuntor.sh:335`).

3. **“Duas primeiras úteis” significa duas linhas contíguas após começar.** Branco ou cabeçalho `# RELATO` depois da primeira linha encerra a leitura (`bin/hook_stop_fila1.py:431`). Assim ESPERANDO, branco, pergunta não aceita a pergunta embora ela seja a segunda não vazia; confirmei com execução apenas em memória das funções extraídas por AST (`bin/hook_stop_fila1.py:430`, `bin/hook_stop_fila1.py:435`). Se o contrato exige contiguidade, essa restrição precisa constar dele [nao verificado].

4. **Prefixo e sufixo semelhantes estão corretamente recusados no helper atual.** As duas fronteiras `(?<![\w-])` e `(?![\w-])` recusam tanto `PRE-OBRA-X` quanto `OBRA-X-OUTRA` para obra `OBRA-X` (`bin/hook_stop_fila1.py:414`). O caso novo do selo testa somente `OBRA-X-OUTRA`, portanto cobre a fronteira à direita; não cobre isoladamente a fronteira à esquerda (`bin/tests/test_disjuntor.sh:339`).

5. **Ainda não se verifica que há pergunta de desenho.** A aceitação exige apenas substring maiúscula `DESENHO` e ID inteiro na mesma linha; aceita, por exemplo, `OBRA-X NAO TEM PERGUNTA DE DESENHO` e `REDESENHO OBRA-X`, confirmado em memória (`bin/hook_stop_fila1.py:415`). A mensagem exige QUAL PADRÃO e POR QUE, mas o predicado não verifica esses elementos (`bin/hook_stop_fila1.py:1048`, `bin/hook_stop_fila1.py:415`).

6. **A leitura parcial preserva somente `OSError`.** Depois de ler uma linha, `OSError` devolve o trecho já obtido (`bin/hook_stop_fila1.py:438`); exceção diferente escapa ao helper e é convertida em ausência de recado pelo invólucro (`bin/hook_stop_fila1.py:1057`, `bin/hook_stop_fila1.py:1062`). O leitor abre UTF-8 sem política de substituição (`bin/hook_stop_fila1.py:428`).

Não executei o selo, pois ele escreve o RELATO de teste e restaura/remove o contador (`bin/tests/test_disjuntor.sh:334`, `bin/tests/test_disjuntor.sh:335`).

## 31/10 09:05 · sessao b14d140bc3b4 · E3-STATUS · verificador · origem=Code

**O delta ainda deixa uma falha central: no julgamento normal, `folga_humana` fica sempre `False`, inclusive quando a porta acaba lavrando `indefinida`.** O adaptador recebe `_vd`, que nesse ramo só pode ser `None` ou `cobrado`; a porta deriva o veredito efetivo depois, pelos códigos. Portanto, a exceção nova de feriado/folga humana não funciona nesse caminho. (`app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1245`, `app/ponto/services/cartorio.py:1558`; `app/ponto/status_dia.py:122`; `app/ponto/portas/celula.py:658`, `app/ponto/portas/celula.py:698`.)

Os anexos fornecidos não contêm linhas de células de produção: quantidade de dias afetados e ocorrência dos cenários abaixo em produção **[não verificado]**.

**1. Matriz de saída e divergências**

A tupla solicitada precisa também de **`previsto_min`**. Exemplo alcançável pelo código: ausência parcial sobre folga ou sobre trabalho, sem batida, não isento, `nunca_bateu`, `folga_humana=False`: com previsto zero sai `folga`; com previsto positivo sai `irregular`. A grade permite ausência parcial sobre folga, e o ramo sem adesão força `nunca_bateu` fora da cobertura integral. (`app/escala/utils.py:1170`, `app/escala/utils.py:1190`, `app/escala/utils.py:1743`, `app/escala/utils.py:1751`; `app/ponto/services/cartorio.py:1032`; `app/ponto/status_dia.py:85`, `app/ponto/status_dia.py:93`, `app/ponto/status_dia.py:99`.)

Na tabela, **a primeira linha aplicável vence**. `B` = tem batida; `C` = cobertura parcial; `I` = isento; `H` = folga humana; `N` = previsto diferente de zero. `A` = `{furo, discordante, cobrado, nunca_bateu, fato_em_ausencia}`. Essa partição cobre todas as entradas da função, incluindo todas as alcançáveis. (`app/ponto/status_dia.py:42`, `app/ponto/status_dia.py:83`.)

| B | Tipo da grade | Condição adicional | Status | Prova |
|---|---|---|---|---|
| 0 | `falta` | qualquer | `falta` | `app/ponto/status_dia.py:83` |
| 0 | `ausencia` | não `(C e não I e V ∈ A∪{indefinida})` | `ausencia` | `app/ponto/status_dia.py:85` |
| qualquer | qualquer | `V=indefinida`, H=1 | B=0: `folga`; B=1: `folga_trabalhada` | `app/ponto/status_dia.py:88` |
| qualquer | qualquer | `V=indefinida`, H=0 | `escala_suspeita` | `app/ponto/status_dia.py:91` |
| 0 | qualquer | tipo=`folga` ou N=0 | `folga` | `app/ponto/status_dia.py:93` |
| 0 | qualquer | I=1 | `ok`, rótulo “isento de ponto” | `app/ponto/status_dia.py:95` |
| 0 | qualquer | V=`coberta` | `ausencia` | `app/ponto/status_dia.py:97` |
| 0 | qualquer | V∈A | `irregular` | `app/ponto/status_dia.py:99` |
| 0 | qualquer | restante | `sem_certidao` | `app/ponto/status_dia.py:101` |
| 1 | qualquer | tipo=`folga` ou V=`fato_sem_previsao` | `folga_trabalhada` | `app/ponto/status_dia.py:102` |
| 1 | qualquer | V∈A | `irregular` | `app/ponto/status_dia.py:104` |
| 1 | qualquer | V∈{`concorde`,`coberta`,`cobranca_indevida`} | `ok`, rótulo de isento se I=1 | `app/ponto/status_dia.py:106` |
| 1 | qualquer | restante | `sem_certidao` | `app/ponto/status_dia.py:108` |

Os filtros de alcance são importantes:

- **Normal:** H=0 sempre, pelo defeito de `_vd` descrito acima. O veredito efetivo já está disponível pela autoridade da porta em `_tl.vereditos`, mas esse resultado não alimenta `fatos_do_status`. (`app/ponto/services/cartorio.py:1249`, `app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1560`.)
- **D5:** usa `cel.veredito`; pode obter H=1 para `indefinida`, somente com `--apply`, status nulo e carimbo considerado atualizado. Status já preenchido é ignorado. (`app/ponto/services/cartorio.py:726`, `app/ponto/services/cartorio.py:729`, `app/ponto/services/cartorio.py:733`.)
- **Nunca bateu:** deriva o veredito, mas força `nunca_bateu` quando não há cobertura integral. (`app/ponto/services/cartorio.py:997`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1041`.)
- **Isento:** o juiz retorna sem códigos; `isento + indefinida` não nasce desse julgamento fresco. Uma célula histórica que já tenha `indefinida` pode seguir outro caminho no D5. (`app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:735`.)
- **Admin:** seus vereditos passam por outra regra: `trabalhou→ok`, `faltou→falta`, `justificado→ausencia`; a porta preserva a decisão humana. (`app/ponto/status_dia.py:39`; `app/ponto/portas/celula.py:694`, `app/ponto/portas/celula.py:738`.)

`indefinida` também inclui `FASE_NAO_DECLARADA_COM_PREVISTO` e `PARCIAL_SEM_PERIODO`, além de `PREVISTO_COM_DIA_INDEFINIDO`; pode existir com batida. (`app/escala/models.py:1583`; `app/ponto/supra_juiz.py:350`.)

As divergências demonstráveis são:

| Situação | Lavra | A2/calendário/ADR |
|---|---|---|
| Normal, `indefinida`, A2 reconheceria feriado/folga humana | H permanece falso; em geral `escala_suspeita` | Calendário suprime trabalho pelo feriado/folga humana; contradiz a costura prometida pelo ADR. (`app/ponto/status_dia.py:88`; `app/colaboradores/services/calendario.py:112`; `app/docs/ADR/0002-celula-e-o-dia.md:55`.) |
| Parcial, não isento, sem batida, veredito acusador | `irregular`, `escala_suspeita` ou até `folga` se previsto zero | Calendário retorna ausência/falta imediatamente pela faixa A2, sem receber `cobertura_parcial`. O supra sustenta cobrar o restante, mas a igualdade com o calendário não existe. (`app/ponto/status_dia.py:85`, `app/ponto/status_dia.py:93`; `app/colaboradores/services/calendario.py:109`; `app/ponto/supra_juiz.py:279`.) |
| Ausência aprovada `folga_compensatoria`/`feriado_folga`, sem batida | Grade chama `ausencia`; lavra retorna `ausencia` antes de consultar H | A2 classifica como faixa `folga`; calendário retorna `folga`. Com batida, a lavra pode ainda produzir `irregular` onde o calendário produz `folga_trabalhada`. (`app/escala/utils.py:1173`, `app/escala/utils.py:1180`; `app/ponto/precedencia.py:62`; `app/ponto/status_dia.py:85`, `app/ponto/status_dia.py:104`; `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:146`.) |
| Feriado supressor, grade ainda `trabalho`, veredito diferente de `indefinida` | A nova consulta é dispensada; lavra pode produzir `irregular`/`ok` | Calendário considera feriado para qualquer veredito. A exceção `trabalha_em_feriado` deve ser respeitada: nesse caso A2 pode manter trabalho. (`app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:99`, `app/ponto/status_dia.py:106`; `app/colaboradores/services/calendario.py:112`; `app/ponto/precedencia.py:330`.) |
| Folga só de escala, `indefinida` | `escala_suspeita` | Calendário preserva `folga`/`folga_trabalhada` contra o mapa de veredito quando reconhece que não é trabalho. Essa diferença já vinha da regra anterior ao delta. (`app/ponto/status_dia.py:91`; `app/colaboradores/services/calendario.py:134`, `app/colaboradores/services/calendario.py:146`, `app/colaboradores/services/calendario.py:168`.) |
| Trabalho previsto, não isento, sem batida, `nunca_bateu` | `irregular` | Calendário mapeia para `falta`, quando nenhuma precedência anterior o absorve. Também é diferença anterior ao delta. (`app/ponto/status_dia.py:42`, `app/ponto/status_dia.py:99`; `app/colaboradores/services/calendario.py:56`, `app/colaboradores/services/calendario.py:168`.) |
| Trabalho previsto, não isento, sem batida, `concorde` | `sem_certidao` | Calendário bloqueia a sobreposição por `ok` e conserva sua classificação anterior, conforme cobertura e demais fatos. (`app/ponto/status_dia.py:101`; `app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:161`.) |

A comparação com o calendário exige mais dados que a tupla: faixa A2, efeito da ausência, trabalho reconhecido pela célula, futuro, turno aberto e cobertura também participam. Portanto, não há uma coluna única de “status do calendário” determinada pelos seis valores pedidos. (`app/colaboradores/services/calendario.py:109`, `app/colaboradores/services/calendario.py:119`, `app/colaboradores/services/calendario.py:144`, `app/colaboradores/services/calendario.py:799`.)

**2. Escritores e leitores**

| Valor | Quem produz/escreve | Quem lê no caminho auditado |
|---|---|---|
| `folga_humana` da lavra | Adaptador inicia falso e deriva pela A2 somente quando recebe `indefinida`; é valor em memória. (`app/ponto/status_dia.py:121`, `app/ponto/status_dia.py:126`, `app/ponto/status_dia.py:136`.) | `status_da_lavra`, somente no ramo `indefinida`. (`app/ponto/status_dia.py:89`.) |
| `folga_humana` da tela | Outra expressão, inline. (`app/colaboradores/services/calendario.py:801`.) | `_classificar`, condicionada à faixa `folga`. (`app/colaboradores/services/calendario.py:116`.) |
| `cobertura_parcial` | Grade, pela resposta de `cobre_parte_do_dia`. (`app/escala/utils.py:1138`, `app/escala/utils.py:1150`, `app/escala/utils.py:1190`.) | Supra, cartório, impressão e adaptador/status. (`app/ponto/supra_juiz.py:243`, `app/ponto/supra_juiz.py:279`; `app/ponto/services/cartorio.py:991`, `app/ponto/services/cartorio.py:1144`, `app/ponto/services/cartorio.py:1153`; `app/ponto/status_dia.py:86`, `app/ponto/status_dia.py:136`.) |
| `tipo_dia` da grade | Base `trabalho/folga`; overlays de FolgaDia e ausência aprovada escrevem `folga/falta/ausencia`. (`app/escala/utils.py:1743`, `app/escala/utils.py:1749`, `app/escala/utils.py:1118`, `app/escala/utils.py:1174`, `app/escala/utils.py:1180`.) | Supra, cartório e adaptador/status. (`app/ponto/supra_juiz.py:197`; `app/ponto/services/cartorio.py:997`; `app/ponto/status_dia.py:133`, `app/ponto/status_dia.py:83`.) |
| `veredito=indefinida` | Supra produz códigos; `derivar_veredito` os traduz; porta resolve o veredito efetivo e o grava. (`app/ponto/supra_juiz.py:305`, `app/ponto/supra_juiz.py:312`, `app/ponto/supra_juiz.py:357`; `app/escala/models.py:1583`; `app/ponto/portas/celula.py:658`, `app/ponto/portas/celula.py:770`.) | Adaptador decide consultar A2; regra decide o status; calendário tem seu próprio mapa. (`app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:88`; `app/colaboradores/services/calendario.py:56`.) |
| A2 `faixa` e `tipo_do_dia` | `fatos_do_periodo` monta candidatos, ordena a precedência e escreve o vencedor; sem jurisdição escreve `None`. `fatos_do_dia` apenas delega. (`app/ponto/precedencia.py:209`, `app/ponto/precedencia.py:332`, `app/ponto/precedencia.py:344`, `app/ponto/precedencia.py:349`, `app/ponto/precedencia.py:356`.) | Helper novo, calendário, texto de supressão e projeção da ata. (`app/ponto/precedencia.py:188`, `app/ponto/precedencia.py:192`, `app/ponto/precedencia.py:378`; `app/colaboradores/services/calendario.py:793`; `app/ponto/services/cartorio.py:1672`.) |
| A2 `fatos.folga_lancada` | A2 consulta FolgaDia, filtra calendário de vínculo alheio e escreve o booleano. (`app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:313`, `app/ponto/precedencia.py:365`.) | Helper, calendário e regra de repouso para férias. (`app/ponto/precedencia.py:191`; `app/colaboradores/services/calendario.py:801`; `app/ferias/situacao.py:97`.) |
| `status_dia/status_rotulo` | `_aplicar_status`, dentro da porta; persistência pelo mesmo mecanismo da lavra. (`app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:773`.) | Juiz D4 lê a coluna após tratar encerramento. Calendário continua na regra antiga até E4. (`app/ponto/status_dia.py:158`, `app/ponto/status_dia.py:163`; `app/docs/ADR/0002-celula-e-o-dia.md:73`.) |

Outros consumidores de **`faixa`** incluem reavaliação de feriado, semana do posto, veto de emissão, previsão e diagnóstico. (`app/chamados/services/feriado_reavaliar.py:36`; `app/escala/services/semana_do_posto.py:177`; `app/ponto/management/commands/detectar_ausencias.py:616`; `app/ponto/services/abriu_nao_bateu.py:123`; `app/ponto/services/dia_do_colab.py:130`.)

Outros consumidores de **`tipo_do_dia`** incluem folha sem furo, vigias e palavra do dia. O cartório também persiste uma projeção A2 em `ata['precedencia']`, pela porta. (`app/folha/services/sem_furo.py:26`; `app/ponto/services/faltas_de_hoje.py:61`; `app/ponto/services/deslogados.py:166`; `app/ponto/services/dia_decidido.py:370`; `app/ponto/services/cartorio.py:1671`; `app/ponto/portas/celula.py:819`.)

**3. O helper responde à mesma pergunta do inline?**

**Para o mesmo dict A2, equivale ao efeito combinado das duas condições do calendário**, isto é:

`faixa=feriado OU (faixa=folga E (folga_lancada OU subtipo de folga))`.

Não equivale ao booleano inline isolado: feriado sem FolgaDia dá helper verdadeiro e booleano inline falso, mas o calendário trata feriado separadamente. (`app/ponto/precedencia.py:187`; `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:801`.)

**A alimentação, porém, não é igual.** O caminho novo chama `fatos_do_dia` sem parâmetros e consulta FolgaDia de todos os vínculos da pessoa. Quando o vínculo ativo cobre a janela, o calendário injeta apenas suas datas de folga, transformadas em `True`. (`app/ponto/status_dia.py:126`; `app/ponto/precedencia.py:209`, `app/ponto/precedencia.py:234`; `app/colaboradores/services/calendario.py:593`, `app/colaboradores/services/calendario.py:604`.)

Há dois contraexemplos por construção; ocorrência em produção **[não verificado]**:

| Mesmo colaborador/data | A2 sem parâmetros | A2 alimentada pelo calendário |
|---|---|---|
| Folga **manual** no vínculo antigo B; ativo A cobre a janela; célula de trabalho por A; sem ausência/feriado | Encontra a folga de B e a preserva: H verdadeiro | Só recebeu datas de A; pode responder trabalho/H falso. (`app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:313`, `app/ponto/precedencia.py:337`; `app/colaboradores/services/calendario.py:607`; `app/escala/alimentacao.py:66`.) |
| Folga **de calendário** no ativo A; célula gerada pelo vínculo B | Descarta a folga de A quando o vínculo selecionado é B | Recebe `True`, que perdeu origem e vínculo; a verificação de propriedade não consegue descartá-la. Pode responder folga/H verdadeiro. (`app/ponto/precedencia.py:305`, `app/ponto/precedencia.py:313`; `app/colaboradores/services/calendario.py:607`.) |

Mesmo com insumos iguais, o status final pode diferir: calendário preserva `aberto` antes de `folga_trabalhada`; o escritor não recebe turno aberto. O D4 trata o relógio separadamente, conforme o ADR. (`app/colaboradores/services/calendario.py:144`; `app/ponto/status_dia.py:90`, `app/ponto/status_dia.py:158`; `app/docs/ADR/0002-celula-e-o-dia.md:21`.)

**4. Checklist**

| Item | Resultado |
|---|---|
| Segundo juiz | O helper projeta fatos já julgados pela A2; não escolhe vínculo nem reordena precedência. Contudo, duplica o predicado que permanece inline no calendário. A2 e status D4 estão registrados; a coexistência dos leitores está declarada até E4. (`app/ponto/precedencia.py:187`; `app/colaboradores/services/calendario.py:801`; `app/core/juizes.py:160`, `app/core/juizes.py:173`; `app/docs/ADR/0002-celula-e-o-dia.md:73`.) |
| Segundo escritor fora da porta | Não aparece no delta: os três sítios passam fatos; status continua escrito na porta. (`app/ponto/services/cartorio.py:733`, `app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:1558`; `app/ponto/portas/celula.py:705`.) |
| Relógio fora da borda | O delta não acrescenta leitura. D5 captura um instante e deriva `hoje`. Já `julgar_celula` lê `localdate()` independentemente de `agora`, além de outros `now()`; é anterior ao delta fornecido. (`app/ponto/management/commands/lavrar_status_competencia.py:43`; `app/ponto/services/cartorio.py:1687`, `app/ponto/services/cartorio.py:1708`, `app/ponto/services/cartorio.py:1711`.) |
| Derivação paralela | Existe no predicado de folga e nas alimentações diferentes descritas acima. Além disso, a costura consulta o override `_vd`, enquanto a porta resolve o veredito efetivo. (`app/ponto/precedencia.py:191`; `app/colaboradores/services/calendario.py:607`, `app/colaboradores/services/calendario.py:801`; `app/ponto/services/cartorio.py:1560`; `app/ponto/portas/celula.py:658`.) |
| Selo/RED sem mordida | O selo de escritor tem casos contra atribuição/save, update e bulk_update indevidos. O selo de costura verifica apenas a presença de `fatos_do_status=`: aceita qualquer `f`. O teste novo chama diretamente o adaptador com `indefinida` e A2 mockada; não atravessa `_vd=None → porta deriva indefinida`. (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:259`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:166`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:284`; `app/ponto/tests/test_e3_status_lavrado.py:133`.) |
| Ordem de trava | Há atomicidade de save+evento, mas isso não prova serialização da leitura dos insumos até a escrita. Nenhuma aquisição explícita de trava de linha aparece nesses caminhos; inversão/contenda efetiva **[não verificado]**. (`app/ponto/portas/celula.py:639`; `app/ponto/management/commands/lavrar_status_competencia.py:49`; `app/ponto/services/cartorio.py:776`.) |
| Impressão e reaplicação | O novo fato consultado não entra no hash; os pulos por impressão antecedem o adaptador. D5 ignora status já preenchido, portanto não é mecanismo de correção dos status anteriores à r3. A promessa de proteção pelo mesmo hash não cobre diretamente a nova leitura; o ADR já admite a lacuna de isenção. (`app/ponto/services/cartorio.py:333`, `app/ponto/services/cartorio.py:385`, `app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:726`; `app/docs/ADR/0002-celula-e-o-dia.md:59`.) |

O **custo incremental** da nova costura é:

- **Cartório normal:** zero consultas A2 adicionais, justamente porque `_vd` nunca ativa o ramo novo. Isso não significa custo total zero: `julgar_celula` já consulta A2 posteriormente para carimbar precedência. (`app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1560`, `app/ponto/services/cartorio.py:1670`, `app/ponto/services/cartorio.py:1719`.)
- **D5 dry:** nenhuma consulta nova dessa costura. **D5 apply:** uma chamada A2 por célula elegível com `indefinida`, sem reaproveitamento em lote. (`app/ponto/services/cartorio.py:733`; `app/ponto/status_dia.py:122`.)
- **Por chamada A2:** cinco consultas estruturais básicas — ausências, FolgaDia, feriados, células e vínculos — mais foto de folgas se houver vínculos. Podem somar consulta de geradora ausente, nome do feriado e acessos lazy de localização. Número observado e duração **[não verificado]**. (`app/ponto/precedencia.py:227`, `app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:245`, `app/ponto/precedencia.py:271`, `app/ponto/precedencia.py:275`, `app/ponto/precedencia.py:289`; `app/escala/alimentacao.py:123`; `app/core/feriados.py:100`, `app/core/feriados.py:113`.)

O contrato do ADR exige lote por competência, igualdade da tela com a folha e precedência preservada; os desvios acima impedem considerar essas propriedades demonstradas pela r3. (`app/docs/ADR/0002-celula-e-o-dia.md:24`, `app/docs/ADR/0002-celula-e-o-dia.md:29`, `app/docs/ADR/0002-celula-e-o-dia.md:51`.)

## 31/10 09:04 · sessao a00cb56b707a · - · - · origem=Ronald (tmux)

Censo adicional dos leitores A2, delimitado a acessos de produção aos campos solicitados:

| Campo | Leitores e uso |
|---|---|
| `faixa` | Classificação do calendário: `app/colaboradores/services/calendario.py:107`, montagem `:793`; novo helper: `app/ponto/precedencia.py:188`; texto de supressão `:378`; carimbo de ata: `app/ponto/services/cartorio.py:1673`; chamados espúrios: `app/chamados/services/feriado_reavaliar.py:36`; buracos da semana: `app/escala/services/semana_do_posto.py:177`; veto de feriado: `app/ponto/management/commands/detectar_ausencias.py:616`; filtro de previsão: `app/ponto/services/abriu_nao_bateu.py:123`; diagnóstico: `app/ponto/services/dia_do_colab.py:130`. |
| `tipo_do_dia` | Calendário, inclusive efeito desconta: `app/colaboradores/services/calendario.py:794`, `:798`, `:802`; helper: `app/ponto/precedencia.py:192`; texto de supressão `:378`; ata: `app/ponto/services/cartorio.py:1672`; folha sem furo: `app/folha/services/sem_furo.py:26`; vigias: `app/ponto/services/faltas_de_hoje.py:61`, `app/ponto/services/deslogados.py:166`; palavra do dia: `app/ponto/services/dia_decidido.py:370`; diagnóstico: `app/ponto/services/dia_do_colab.py:86`. |
| `fatos.folga_lancada` | Calendário: `app/colaboradores/services/calendario.py:801`; helper: `app/ponto/precedencia.py:191`; repouso para aviso de férias CLT §3: `app/ferias/situacao.py:97`. O último consulta os fatos subjacentes mesmo com férias vencendo, conforme a própria regra explica em `app/ferias/situacao.py:82`. |

Os produtores continuam os já indicados: sem jurisdição, A2 escreve `None` em `app/ponto/precedencia.py:344` e `:346`; com vencedor escreve `tipo_do_dia`, `faixa` e booleano `folga_lancada` em `:356`, `:357`, `:365`. O cartório persiste uma projeção `tipo_do_dia/faixa/...` na ata pela porta em `app/ponto/services/cartorio.py:1671`; a porta escreve `ata['precedencia']` em `app/ponto/portas/celula.py:819`.

O espelho transporta o dict A2 para o tradutor da palavra, sem recalcular esses campos: carrega o período em `app/ponto/services/espelho.py:659`, transporta em `:881` e entrega a `dia_decidido.do_dia` em `:351`; o acesso efetivo a `tipo_do_dia` está em `app/ponto/services/dia_decidido.py:370`.

Há dois valores de memória chamados `folga_humana`: o adaptador novo escreve em `app/ponto/status_dia.py:126` e devolve em `:136`, consumido pela regra em `:89`; o calendário escreve sua própria expressão inline em `app/colaboradores/services/calendario.py:801`, consumida em `:116`. Isso identifica as duas derivações sem confundi-las com uma coluna persistente.

## 31/10 09:03 · sessao a00cb56b707a · - · - · origem=Ronald (tmux)

Evidências consolidadas, todas da cópia local:

- **Escritor persistente:** o delta acrescenta argumentos aos três chamadores; a aplicação e escrita de `status_dia/status_rotulo` continuam na porta, em `app/ponto/portas/celula.py:686`, `:698`, `:701`, `:705`. A persistência do save e evento é atômica em `app/ponto/portas/celula.py:634`, `:639`. A delegação do modelo permanece em `app/escala/models.py:1787`.
- **Falha central da costura:** no ramo normal, `_vd` recebe somente `None` ou `'cobrado'`; o adaptador recebe esse valor, enquanto a porta pode derivar `'indefinida'` pelos códigos. Logo o ramo normal não consulta a nova A2, mesmo quando o status finalmente é julgado com veredito `'indefinida'`. `app/ponto/services/cartorio.py:1220`, `:1245`, `:1558`, `:1560`; `app/ponto/portas/celula.py:658`, `:745`; `app/ponto/status_dia.py:122`.
- **D5 efetivamente consulta A2:** passa `cel.veredito`; somente em apply, status nulo e carimbo considerado em dia. Não existe cache/lote para essa nova consulta. `app/ponto/services/cartorio.py:726`, `:729`, `:733`, `:735`.
- **Ramo nunca bateu:** deriva pelos códigos, mas fora da cobertura integral força `'nunca_bateu'`; a própria grade define cobertura integral como ausência/falta sem parcial. `app/ponto/services/cartorio.py:997`, `:1031`, `:1032`, `:1041`, `:1047`. Caso efetivo de nova consulta A2 nesse ramo: [não verificado].

Custo incremental da A2 por chamada efetiva do D5:

| Consulta | Prova |
|---|---|
| Ausências | `app/ponto/precedencia.py:227` |
| FolgaDia do dia | `app/ponto/precedencia.py:236` |
| Feriados | `app/ponto/precedencia.py:245`; `app/core/feriados.py:82` |
| Células | `app/ponto/precedencia.py:271`; `app/escala/alimentacao.py:43` |
| Vínculos candidatos | `app/ponto/precedencia.py:275`; `app/escala/alimentacao.py:116` |
| Foto de folgas, se há vínculos | `app/ponto/precedencia.py:289`; `app/escala/alimentacao.py:66` |
| Extra se geradora falta entre candidatos | `app/escala/alimentacao.py:123` |
| Extra para nome, se feriado | `app/ponto/precedencia.py:364`; `app/core/feriados.py:100` |
| Possíveis acessos lazy de posto/município | `app/ponto/precedencia.py:243`; `app/core/feriados.py:113` |

São **cinco consultas estruturais básicas, seis com vínculos, mais extras condicionais**; o número observado e duração são [não verificado]. O normal paga **zero consultas incrementais da nova costura**, por causa de `_vd`; `julgar_celula` já consulta A2 separadamente após o julgamento, para carimbar precedência. `app/ponto/services/cartorio.py:1220`, `:1560`, `:1670`, `:1717`.

Mapa de valores:

| Valor | Escritor/origem | Leitores principais |
|---|---|---|
| `folga_humana` | Local do adaptador, derivado do helper A2: `app/ponto/status_dia.py:121`, `:126`, `:136`; helper `app/ponto/precedencia.py:179` | `app/ponto/status_dia.py:89` |
| `cobertura_parcial` | Grade, via juiz `cobre_parte_do_dia`: `app/escala/utils.py:1137`, `:1150`, `:1190` | `app/ponto/supra_juiz.py:243`, `:279`; `app/ponto/services/cartorio.py:991`, `:1144`; `app/ponto/status_dia.py:136`, `:86` |
| `tipo_dia` | Base trabalho/folga: `app/escala/utils.py:1743`, `:1749`; overlay folga `:1118`; falta/ausência `:1174`, `:1180` | `app/ponto/supra_juiz.py:279`; adaptador `app/ponto/status_dia.py:133`; regra `:83` |
| `veredito='indefinida'` | Mapa de três códigos: `app/escala/models.py:1583`, `:1592`; códigos no supra, inclusive parcial sem período: `app/ponto/supra_juiz.py:305`, `:312`, `:324`, `:357` | Porta deriva/grava: `app/ponto/portas/celula.py:658`, `:770`; adaptador `app/ponto/status_dia.py:122`; status `:88` |
| A2 `faixa`, `tipo_do_dia`, `fatos.folga_lancada` | `app/ponto/precedencia.py:344`, `:346`, `:350`, `:356`, `:357`, `:365`; FolgaDia de outro calendário é filtrada em `:313` | Novo helper `app/ponto/precedencia.py:188`, `:191`; ata `app/ponto/services/cartorio.py:1672`, `:1673` |

Checklist adicional:

- **Registro:** A2 e status estão registrados em `app/core/juizes.py:160`, `:173`; a autoridade nova continua sendo `status_do_dia_lavrado`, não `status_da_lavra`, conforme declaração em `app/docs/ADR/0002-celula-e-o-dia.md:63`. O helper é projeção dos fatos já julgados, mas conserva derivação paralela à expressão inline do calendário até E4, como o próprio docstring admite. `app/ponto/precedencia.py:184`.
- **Selo tem mordida para escritor:** planta atribuição/save, update e bulk_update indevidos de status. `app/ponto/tests/test_selo_veredito_so_pela_porta.py:259`.
- **Selo da costura não morde o defeito:** verifica apenas presença da keyword `fatos_do_status`, aceita um valor arbitrário `f`, não verifica `colab/data/veredito` nem identidade com veredito efetivo. `app/ponto/tests/test_selo_veredito_so_pela_porta.py:156`, `:166`, `:284`.
- **Testes r3 não atravessam a costura normal:** chamam diretamente o adaptador com `'indefinida'` e A2 mockada; não detectam `_vd=None` chegando ao adaptador enquanto a porta deriva `'indefinida'`. `app/ponto/tests/test_e3_status_lavrado.py:133`, `:135`, `:137`.
- **Relógio D5:** uma leitura na borda, com `hoje` derivado dela. `app/ponto/management/commands/lavrar_status_competencia.py:43`. `_completar_status` não passa `agora`, mas o caminho que só muda status preserva `veredito_em`. `app/ponto/services/cartorio.py:733`; `app/ponto/portas/celula.py:753`, `:757`.
- **Relógio legado da unidade:** `julgar_celula` lê `localdate()` independentemente do `agora` injetado e pode voltar a ler `now()`. Isso antecede o delta dado. `app/ponto/services/cartorio.py:1687`, `:1708`, `:1711`.
- **Travas:** não há aquisição explícita de trava de linha no D5, em `julgar_colab` ou na lavra; há atomic no save+evento. Ordem/contenda efetiva de locks: [não verificado]. Trechos: `app/ponto/management/commands/lavrar_status_competencia.py:49`, `:55`; `app/ponto/services/cartorio.py:739`, `:776`; `app/ponto/portas/celula.py:639`.
- **Impressão não inclui o novo fato:** lista de insumos codificados está em `app/ponto/services/cartorio.py:333`, `:339`, `:344`, `:352`, `:362`, `:377`, `:385`; `folga_humana` não participa. Pulo por impressão vem antes do adaptador, e D5 ignora status já preenchido. `app/ponto/services/cartorio.py:1157`, `:1161`, `:726`. A promessa do ADR de proteção pelo mesmo hash merece contestação; ele já admite explicitamente a lacuna de isenção. `app/docs/ADR/0002-celula-e-o-dia.md:59`.

_941 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
