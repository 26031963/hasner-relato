# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 16:45** · publicado em **12/11/2026 16:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 16:45 · sessao bab0c9032b66 · - · - · origem=Ronald (tmux)

Mapa de escrita/leitura dos campos do C6, verificado apenas no código:

| Campos | Quem escreve | Quem lê na decisão |
|---|---|---|
| `validada_em` | `_carimbar_validacao`: `app/chamados/services/validacao.py:134`, salva em `:137`; devolução limpa em `:199`; `PerguntaDisputa.carimbar(resposta='validar')` também atribui em `app/chamados/models.py:3136` | Projeção temporal: `app/chamados/juizes.py:1375`; morte imediata: `:1391` |
| `resposta_colab`, `respondida_em` | Resposta humana: `app/chamados/services/disputa_emissao.py:1467`; resposta automática: `:1652`; recuperação de carimbo pela trilha: `:1674`; `carimbar` também preenche `respondida_em`: `app/chamados/models.py:3133` | Projeção: `app/chamados/juizes.py:1375`; ramo respondida: `:1407`; comparação com luz: `:1346` |
| `materializada_em`, `via_resolucao` | Atribuição conjunta `_escrever_carimbo`: `app/chamados/models.py:3098`; portas persistem em `:3153` e `:3191`; limpeza em lote: `:3265`; mudança apenas da via por fato no espelho: `:3276` | Projeção: `app/chamados/juizes.py:1376`; precedência de via: `:1402`, `:1428`; marco materializado: `:1434`; sem carimbo: `:1466`; testemunha externa: `:1473` |
| `motivo`, `tipo_resposta`, `celula` | Emissor passa motivo/tipo: `app/chamados/services/disputa_emissao.py:444`; `_nascer` monta e salva a pergunta, incluindo célula e demais campos: `app/chamados/models.py:3015` | Motivo e célula para identificar marco: `app/chamados/juizes.py:941`; tipo para comparar resposta/luz: `:1332`; testemunha por tipo: `:706` |
| `celula.veredito`, `celula.veredito_via` | Porta `lavrar_veredito`: `app/ponto/portas/celula.py:974`, atribui em `:1035` e persiste em `:1040` | Decisão explícita do admin: `app/chamados/juizes.py:1405`; acusação do dia: `:1438`; concordância do dia: `:1365` |
| `celula.dna.marcos` | Nascimento: `app/ponto/portas/celula.py:260`; regeneração: `:272`; edição: `:604`; reversão: `:786`; geração de período: `:857`, `:865`, `:875` | Identificação das lâmpadas: `app/chamados/juizes.py:948`; comparação resposta/luz: `:1338` |
| `celula.ata.lampadas` (`hora`, `tipo`, `acesa`, `luz`, `pendente`) | Cartório prepara e entrega a ata: `app/ponto/services/cartorio.py:1851`; `_ata_ate_o_teto` marca `pendente=True`: `:915`; porta persiste ata: `app/ponto/portas/celula.py:1033` | Marco aceso: `app/chamados/juizes.py:965`; luz para comparar resposta: `:1344`; `marcos_pendentes`: `app/ponto/services/lastro.py:148`, chamado pelo C6 em `app/chamados/juizes.py:1452` |

Não afirmar escritor único literal de resposta/validação: há caminhos explícitos adicionais nas referências acima. Para carimbo, `_escrever_carimbo` concentra a atribuição conjunta, mas existem limpeza em lote e mudanças isoladas de via explicitadas acima. `app/chamados/models.py:3098`, `app/chamados/models.py:3265`, `app/chamados/models.py:3276`.

O último degrau delega a `_pergunta_materializada_de_fato`, que lê também testemunhas externas de batida/turno; sua investigação completa excede os campos diretos mapeados aqui. `app/chamados/juizes.py:662`, `app/chamados/juizes.py:708`, `app/chamados/juizes.py:1473`.

## 12/11 16:45 · sessao b1de5a0cb52e · - · - · origem=Ronald (tmux)

`medir()` usa a janela de `janela_fechamento(mes, ano, Empresa)` e seleciona perguntas por `data_evento` cru entre os extremos, sem filtro `conversa=False` — `app/chamados/services/competencia_trancada.py:87`, `:107-109`. A tranca usa `_dia_de_turno_da_pergunta`, que devolve `None` para conversa e `data_evento` para pergunta de fato — `app/chamados/juizes.py:334-338`, `:991-992`. Portanto há uma diferença demonstrável de universo para conversas; não há linha de produção fornecida que prove ocorrência dela neste caso [não verificado].

Para pergunta de fato, ambos usam o corte cadastrado da empresa: `janela_fechamento` chama `periodo_apuracao(..., corte_da_empresa(...))`, e `janela_atual` chama `periodo_apuracao_atual(..., corte_da_empresa(...))` — `app/ponto/janelas.py:54-63`. O juiz extrai mês/ano do fim da janela que contém o dia e consulta a tranca — `app/chamados/juizes.py:1062-1070`.

O toque humano posterior é lei documentada, não hipótese: OPÇÃO (A), menos `(colaborador,dia)` tocado pelo administrador depois da tranca, em `app/docs/CORTES.md:83`, repetida em `:272`. O código efetua essa exceção com `out[i] = not tocou` — `app/chamados/juizes.py:1071-1079`. Isso não reabre o PF: a reabertura do período escreve `reaberto_em` em outra porta — `app/ponto/services/fechamento.py:1432-1441`.

**Limitação material do contador da cópia:** `vivas_apesar_da_tranca` recebe perguntas vivas somente no ramo em que chamado destino ou pai está no conjunto `resgatados` — `app/chamados/services/competencia_trancada.py:119-125`. Fora desse ramo, o código apenas acrescenta as perguntas mortas a `perguntas_lidas_trancadas`; não acrescenta a viva a `vivas_apesar_da_tranca` — `:130-134`. Assim, o contador de `medir()` pode dar zero sem esgotar todas as vivas selecionadas, por inspeção do fluxo — `:119-134`. **Não se pode atribuir essa limitação à sonda raspada:** sem fonte de `sonda_vivas08.py`, não sabemos se ela chama `medir()` ou faz censo próprio [não verificado].

Mapa de campos:

| Campo | Escrita | Leitura pertinente |
|---|---|---|
| `PerguntaDisputa.data_evento` | Ponto de atribuição `fio.gravar_dia_do_fato`, `app/chamados/services/fio.py:791-802`; nascimento chama-o e salva em `app/chamados/models.py:3014-3018`. Portas: pergunta de fato recebe dia do turno em `:2777-2791`; conversa recebe `localdate()` em `:2795-2803`. | `medir`: `app/chamados/services/competencia_trancada.py:108`; tranca pelo discriminador: `app/chamados/juizes.py:338`, `:991-992`. |
| `PerguntaDisputa.conversa` | `_nascer` recebe a porta e insere em `app/chamados/models.py:3015`; `abrir` passa `False` em `:2790`, `abrir_conversa` passa `True` em `:2802`. | `_dia_de_turno_da_pergunta`: `app/chamados/juizes.py:338`. |
| `Empresa.dia_inicio_competencia` | Form lê/valida em `app/colaboradores/views.py:496-499`, repassa para atualizar em `:520-523`, criar em `:531-538`; portas persistem em `app/colaboradores/services/estrutura.py:12-20`. | `corte_da_empresa`: `app/ponto/janelas.py:42-51`; ambas as janelas: `:57`, `:63`. |
| PF `empresa_id`, `mes`, `ano` | `trancar_periodo.get_or_create`: `app/ponto/services/fechamento.py:1408-1410`. | `periodo_trancado`: `:1470`; frota: `app/chamados/services/competencia_trancada.py:312-313`. |
| PF `fechado_em` | Inserção automática: `app/ponto/models.py:926`; retranca: `app/ponto/services/fechamento.py:1415`, salva `:1419`. | Exceção resgatado: `app/chamados/services/competencia_trancada.py:105`; juiz do toque: `app/chamados/juizes.py:1070`. |
| PF `reaberto_em` | Retranca limpa: `app/ponto/services/fechamento.py:1417`; reabertura grava: `:1439`, salva `:1441`. | Tranca viva: `:1470`; frota: `app/chamados/services/competencia_trancada.py:312`. `ativo` não é autoridade da tranca — `app/ponto/services/fechamento.py:1464-1470`. |
| Chamado `resgatado_em` | `arquivo.levantar` guarda o primeiro carimbo e salva: `app/chamados/services/arquivo.py:197-210`. A documentação da porta diz que pode representar mão humana **ou do colaborador** — `:180-184`; portanto não prova sozinho toque de admin. | `medir` compara com PF `fechado_em`: `app/chamados/services/competencia_trancada.py:105-106`. |

O dado enviado contém listas vazias em todos os 15 PF e `total_erros=0`, mas não traz nenhum PK ou campo individual das quatro perguntas históricas citadas no aval; atribuir degrau concreto, conversa ou toque humano a qualquer delas seria [não verificado].

## 12/11 16:45 · sessao 08d6f68761e0 · - · - · origem=Ronald (tmux)

A exceção de toque humano depois da tranca é **lei existente**, documentada em `app/docs/CORTES.md:83` e registrada como predicado C20 em `app/core/juizes.py:156`. Não reabre o `PeriodoFechado`: deixa aquele `(colaborador, dia)` fora do silêncio da tranca, por `out[i] = not tocou` em `app/chamados/juizes.py:1079`.

O caminho é:

- `_pergunta_viva_calc` consulta a tranca antes da resposta e retorna `False` se ela calar a pergunta: `app/chamados/juizes.py:1395`.
- `dia_da_pergunta_trancado` usa `_dia_de_turno_da_pergunta`, explicitamente distinguindo a conversa cujo `data_evento` é dia civil: `app/chamados/juizes.py:989`.
- `dias_trancados` exige dia, colaborador e empresa; resolve a janela pela empresa, consulta o PF da competência e passa `pf.fechado_em` ao toque: `app/chamados/juizes.py:1024`, `app/chamados/juizes.py:1062`, `app/chamados/juizes.py:1070`.
- O toque reconhecido vem de `LogAuditoria`: usuário não nulo e diferente do sistema; timestamp **estritamente posterior** à tranca: `app/chamados/juizes.py:1252`. Não lê `CelulaDia.editada_em` ou `veredito_em` para esse predicado: os três conjuntos de campos consultados estão em `app/chamados/juizes.py:1261`, `app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1310`.

| Campos usados | Escritor | Leitor |
|---|---|---|
| PF `empresa_id`, `mes`, `ano`, `fechado_por`; nascimento | `trancar_periodo`, `app/ponto/services/fechamento.py:1408` | Busca por empresa/competência e `reaberto_em IS NULL`, `app/ponto/services/fechamento.py:1470` |
| PF `fechado_em` | `auto_now_add`, `app/ponto/models.py:926`; nova tranca após reabertura, `app/ponto/services/fechamento.py:1415` | Limite do toque, `app/chamados/juizes.py:1070` |
| PF `reaberto_em` | Limpa ao trancar novamente, `app/ponto/services/fechamento.py:1417`; grava ao reabrir, `app/ponto/services/fechamento.py:1439` | Decide tranca viva, `app/ponto/services/fechamento.py:1470` |
| Log `usuario`, `acao`, `modelo`, `objeto_id`, `descricao` | Escritor comum `core.observ.evento`, `app/core/observ.py:134`; adaptador `registrar_log`, `app/core/auditoria.py:22` | Identificação de humano, ação e alvo, `app/chamados/juizes.py:1252`, `app/chamados/juizes.py:1261`, `app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1310` |
| Log `criado_em` | `auto_now_add`, `app/core/models.py:307` | Corte estrito contra PF, `app/chamados/juizes.py:1268`, `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1315` |
| Ação `cobrar_dia_manual`, alvo colaborador e `dia=AAAA-MM-DD` na descrição | `app/chamados/services/cobrar_dia.py:39` | `app/chamados/juizes.py:1261` |
| Ação `veredito_celula`, alvo célula | `app/ponto/services/veredito_celula.py:356` | Resolve célula por `(colaborador_id,data)`, `app/chamados/juizes.py:1276`; consulta ação, `app/chamados/juizes.py:1287` |
| Ação `desarquivar_chamado`, alvo chamado | `app/chamados/views_arquivar.py:77` → `app/chamados/services/arquivo.py:211` → trilha `app/chamados/services/arquivo.py:137` | Liga chamado ao colaborador e célula, `app/chamados/juizes.py:1300`; consulta ação, `app/chamados/juizes.py:1310` |

Os testes codificam: toque humano posterior mantém viva (`app/chamados/tests/test_c7_red_minimos.py:391`); sistema/usuário nulo não contam (`app/chamados/tests/test_c7_red_minimos.py:399`); toque anterior não conta (`app/chamados/tests/test_c7_red_minimos.py:408`). Esses testes foram lidos; execução nesta auditoria [não verificado].

O dado fornecido contém zero VIVAs e nenhum campo individual de pergunta; atribuir algum degrau ou toque às quatro históricas permanece [não verificado].

## 12/11 16:44 · sessao fcbb067fed4f · - · - · origem=Ronald (tmux)

Censo primário de (i): **60 sítios por arquivo/linha**, fundindo leitura/escrita na mesma linha e incluindo derivados da grade e cópia para contexto de chamado. Esse total deriva de 70 ocorrências AST literais, exclui cinco homônimos, funde seis duplicidades e acrescenta `orfas=` em `app/escala/services/leitor_celula.py:420`.

Homônimos excluídos:

- `app/chamados/reconciliador.py:1340`, `:1377`: contador de eventos.
- `app/core/management/commands/gerar_diagrama.py:783`, `:854`: perguntas sem juiz.
- `app/ponto/portas/celula.py:1132`: campo numérico de `FuroDiario`.

(ii), produção Python: `_orfas_da_ata` definido em `app/escala/services/leitor_celula.py:180`, chamado em `:420`; transporte de `orfas_grade` em `app/ponto/services/espelho.py:689`, `:692`, `:844`. Templates, por grep e sem AST: `app/templates/ponto/espelho.html:151`, `:240`. Função pública `orfas_da_ata` e campo `orfas_instantes` não foram encontrados no levantamento textual de `app/` [não verificado quanto a construções dinâmicas].

Complemento de aliases/transportes, fora dos 60 primários:

| Arquivo e função | Linhas e operação |
|---|---|
| `app/escala/utils.py::_match_marcos` | `621`, `662`: construção HH:MM; `624`, `665`: retorno das listas |
| `app/escala/utils.py::marco_ocupado_pela_batida` | `427`: recebe lista pelo desempacotamento; `_orfas` descartada |
| `app/escala/utils.py::montar_grade_prevista` | `776`: desempacota lista |
| `app/escala/utils.py::montar_grade_prevista_periodo_por_turno` | `1438`, `1567`: mapa e listas vazias; `1650`: desempacota; `1651`: copia para set; `1675`: membership HH:MM; `1701`: desempacota; `1722`, `1733`: append HH:MM; `1758`: mapa recebe lista; `1970`, `1996`: aliases; `1997`: teste None; `2006`: membership; `2032`: filtra órfãs do turno e compara lista; `2039`: remove pelo HH:MM |
| `app/colaboradores/services/calendario.py::contexto_calendario` | `297`: mapa vazio; `622`: extrai coleção em `_ho`; `626`: membership HH:MM |
| `app/escala/orfa_classe.py::classificar_orfas` | `59`: teste vazio; `66`, `71`: mapas HH:MM→classe; `74`: ordenação e chamada do agrupador |
| `app/ponto/propositor.py::dia_degenerado` | `42`, `59`, `61`: presença; `53`: valida hora; `55`: converte e ordena minutos |
| `app/ponto/services/cartorio.py::lampadas_do_dia` | `599`: parâmetro `orfas`; `628`: converte órfãs para minutos em `_reais` |
| `app/ponto/services/cluster_espurio.py::_leitura_da_celula` | `33`: quantidade; `34`: minutos ordenados; `36`: retorna coleção |
| `app/ponto/services/cluster_espurio.py::dias_com_cluster` | `49`: chama leitor; `51`: transporta lista recebida |
| `app/ponto/services/cluster_espurio.py::reconciliar_colab` | `193`: desempacota; `210`: texto por join |
| `app/ponto/services/vigia_pareador_celula.py::censo` | `78`: mapa transporta lista/set; `79`: desempacotamento; `109`: extrai órfãs civis; `111`: membership; `112`: publica caso |
| `app/relatorios/cartao_pela_celula.py::orfas_e_isencao_da_ata` | `285`: mapas vazios; `293`: retorna mapa de órfãs |
| `app/relatorios/pdf_espelho.py::_coletar_dados_espelho` | `472`: set vazio; `513`: passa set global ao predicado; `544`: mapa vazio; `607`: extrai `_orf`; `609`: membership HH:MM |
| `app/ponto/supra_juiz.py::classificar_dia` | `516`, `517`: testes de presença; `520`: texto da coleção por join |

Cópias de ata/dia inteiros confirmadas, para complemento opaco:

- `app/escala/services/leitor_celula.py::grade_da_celula`: `393`, `395`, `418` copiam `base`; `418` também transporta ata completa no envelope de insumos.
- `app/escala/utils.py::_montar_grade_prevista_periodo_legacy`: `1043`, `**sd`, seguido de sobrescrita explícita de `orfas` na mesma linha.
- `app/ponto/portas/celula.py::celula_como_o_julgamento_deixa`: `943`, cópia de célula; `945`, cópia da ata.
- `app/ponto/portas/celula.py::lavrar_veredito`: `995`, cópia; `1000`, comparação integral; `1006`, `1033`, atribuição.
- `app/ponto/portas/celula.py::carimbar_precedencia`: `1083`, cópia integral; `1085`, atribuição.
- `app/ponto/services/cartorio.py::ata_do_dia`: `772`, retorno integral.
- `app/ponto/services/cartorio.py::dispensar_por_ausencia`: `803`, `813`, retorno integral.
- `app/ponto/services/cartorio.py::_dia_ate_o_teto`: `904`, cópia integral do dia.
- `app/ponto/services/cartorio.py::_ata_ate_o_teto`: `910`, retorno; `917`, cópia.
- `app/ponto/services/cartorio.py::_julgar_colab_corpo`: `1251`, `1293`, `1436`, `1437`, `1440`, `1851`, `1853`, composição, cache e passagem de atas.
- `app/ponto/services/cartorio.py::ata_deste_julgamento`: `57`, leitura de ata integral no cache; `_tl.atas` limpo em `1124`.
- `app/chamados/juizes.py::celula_deste_julgamento`: `422`, import com alias do leitor de ata integral; `424`, transporte para projeção.
- `app/ponto/supra_juiz.py::com_insumo_dia_sem_trabalho`: `62`, cópia do dia e envelope de ata integral.

Esses transportes conservam o valor HH:MM já sem data; as conversões que efetivamente retiram data/fuso estão na construção das listas, por exemplo `app/escala/utils.py:621`, `:662`, `:799`, `:1832`, `:1834`. O leitor aceita strings sem validar formato e usa `'?'` como fallback em `app/escala/services/leitor_celula.py:187`, `:190`; conteúdo real das atas [não verificado].

## 12/11 16:44 · sessao b4426e614e40 · - · - · origem=Ronald (tmux)

O censo AST dos acessos literais a `orfas`, `orfas_grade` e `_orfas_da_ata` revela que a mudança alcança também classificadores, vigias e propositor, além dos cinco caminhos nomeados pelo aval. Os sítios concretos seguem abaixo. (`app/escala/orfa_classe.py:58`; `app/ponto/propositor.py:39`; `app/ponto/services/vigia_pareador_celula.py:74`)

**Grade, dry-run e reconciliação**

| Sítio atual | Comportamento e perda de lastro | Encaixe proposto |
|---|---|---|
| `app/escala/utils.py:621`, `:662` | `_match_marcos` recebe datetimes em `locs`/`B`, mas devolve órfãs por `strftime('%H:%M')`. | Devolver diretamente `list[datetime]` aware; o cartório serializa ISO. Não acrescentar coletor/lista paralela. |
| `app/escala/utils.py:683` | Builder single monta `locs` com `timezone.localtime(b.timestamp)`. | Conservar esse objeto até a lavra. |
| `app/escala/utils.py:799`, `:803` | Sobras fora dos envelopes viram HH:MM e são ordenadas como strings. | Acrescentar instante original e ordenar cronologicamente. |
| `app/escala/utils.py:1650`, `:1651`, `:1675` | Dry-run copia `consumed`, obtém órfãs HH:MM, cria set e reconhece candidato por `strftime('%H:%M') in _orfset`. | Set dos próprios datetimes retornados por `_match_marcos`; `locs[k][0] in _orfset`. |
| `app/escala/utils.py:1722`, `:1733` | Ponta fora da régua e intervalo excedente são promovidos a órfã copiando `_cc['hora']`. | Preservar instante em `_celula_do_marco`, antes dessa promoção; hoje a função só guarda hora, delta, status e tipo. (`app/escala/utils.py:314`, `:326`) |
| `app/escala/utils.py:1832`, `:1834`, `:1838` | Conservação por turno novamente produz HH:MM e ordena string. | Usar instantes de `locs`, inclusive segundos/microssegundos. |
| `app/escala/utils.py:1887`, `:1895` | Há **ata sintética em memória**, usada para perguntar ao juiz do par, além da ata persistida. | A ata sintética também precisa de `orfas` ISO, para que `periodos_do_dia` use o mesmo leitor único. A cópia não é salva. (`app/escala/utils.py:1886`) |
| `app/escala/utils.py:1968`, `:1973` | Reconciliação escolhe a lista do dia do turno; para saída noturna, a lista de `data_turno + 1`, pelo juiz `cross_meianoite`. | A seleção da célula permanece a que o juiz já dá; não serve para fabricar instante. |
| `app/escala/utils.py:2005`, `:2006`, `:2039` | Borda é reconhecida e removida por HH:MM. | Procurar/remover **uma ocorrência do instante exato** `_inst_t`. |
| `app/escala/utils.py:2028`, `:2032`, `:2034` | Exceção de entrada também monta horas do turno, intersecta órfãs por hora e ordena por string. | Conjunto de timestamps do turno; interseção por instante; comparar instante preservado da célula com `_inst_t`. A guarda atual que exclui noturno continua existindo até alteração explícita de sua regra. (`app/escala/utils.py:2027`) |

As limpezas atuais devem limpar a única lista nova, sem sincronização com lista auxiliar: clip `app/escala/utils.py:962`, `:966`; folga `:1153`; ausência/falta/justificativa `:1218`, `:1253`, `:1258`. O transporte legacy permanece em `:1043`, e o adaptador de realizado devolve a lista em `:1110`. Hoje esse adaptador descarta órfãs quando faltam marcos, devido ao retorno antecipado; o caso sem marcos do diff é uma especificação útil. (`app/escala/utils.py:1084`)

**Todos os outros leitores diretos da ata encontrados**

| Leitor | O que faz hoje | Migração necessária |
|---|---|---|
| `app/ponto/juiz_batida.py:161` | Converte cada órfã em string e publica no resultado de `periodos_do_dia`. | Usar `orfas_da_ata(celula)` e preservar `None`; não transformar desconhecimento em `[]`. Resultado publica órfãs em `:164`. |
| `app/chamados/juizes.py:510` | Testa existência por lista ou `n_orfas`; prende pelo juiz da régua em `:516`. | Eliminar acesso direto à lista. Existência pode continuar pela contagem lavrada quando conhecida, mesmo quando a identidade é desconhecida. |
| `app/chamados/services/materializacao.py:181` | Filtra órfãs para HH:MM legível, mistura com luzes e entrega ao propositor; o plantio também vira HH:MM em `:182`. | Ler instantes pela porta; adaptar a atribuição para coordenadas temporais que conservem a data real; `None` impede absolvição por marcas incompletas. Não datar órfãs pela célula. |
| `app/ponto/services/cluster_espurio.py:29`, `:31` | Exige lista; corta HH:MM em minutos em `:34`; reconhece intervalo ≤15 min em `:35`. | Leitor único; `None` mantém “não medido”; ordenar instantes e subtrair datetimes. A API já distingue `None`, `()` e assinatura. (`:15`–`:20`) |
| `app/ponto/services/esmeril_espelho.py:125` | A5 acumula recorrência em faixas de 30 min e emite quando ≥3 ocorrências. (`:126`–`:139`) | Instantes pela porta; pertença a faixas aware da **data real do instante**, preservando as mesmas 48 faixas e o mesmo limiar; agregar somente a categoria da faixa entre dias. HH:MM é rótulo final em `:139`. Não substituir a regra de recorrência por igualdade de instantes. A5 é explicitamente “recorrente no mesmo horário”. (`:19`) |
| `app/ponto/services/vigia_pareador_celula.py:74` | Carrega listas por célula; encontra data civil por limites de instantes em `:82`–`:87`, mas reconhece a borda por HH:MM em `:110`–`:111`. | Carregar células/representações aceitas pela porta e comparar `b.timestamp in instantes`; registrar “não medido” para legado, sem fingir universo vazio. |
| `app/ponto/management/commands/tripwire_celulas.py:264` | Peneira circular HH:MM em `:165`–`:177`; confirmação ainda reconhece órfã por HH:MM em `:195`, apesar de conferir distância por instante. | Porta única; passar o instante da órfã à peneira/confirmação e comparar diretamente com marcos absolutos. Não procurar uma batida qualquer com a mesma hora. |
| `app/relatorios/cartao_pela_celula.py:288` | `orfas_e_isencao_da_ata` devolve set de strings por célula. | Porta única e sets de instantes, mantendo desconhecimento. Nenhum chamador fora dos testes foi encontrado pelo nome; completude de chamadas dinâmicas: [não verificado]. |
| `bin/snapshot_o118_col221.py:32` | Conta células com lista não vazia para imprimir “ANTES”. | Ferramenta também deve deixar de ler diretamente a lista; não foi executada. Ela escreve snapshot em `:30`. |

O contexto de chamado recebe cópia de órfãs em `app/ponto/services/cluster_espurio.py:211`, e a descrição concatena suas strings em `:210`; esse transporte precisa manter ISO no dado e formatar HH:MM somente na descrição. Não é outro escritor de `CelulaDia.ata`. (`app/ponto/services/cluster_espurio.py:196`, `:211`)

**Leitores de projeção `dia['orfas']`**

| Grupo | Sítios | Tratamento |
|---|---|---|
| Identidade na tela calendário | `app/colaboradores/services/calendario.py:380`, `:381` → comparação `:625`, `:626` → marca `b.orfa` `:627` | Projeção carrega instantes; marcar pela igualdade com `b.timestamp`; guardar desconhecimento separado do booleano conhecido. |
| Classificação de órfãs | `app/escala/orfa_classe.py:58` | Chave do resultado deve ser instante; rajada deixa de usar `_minutos` `:31`, `:44`, `:74` e passa a diferenças entre instantes. |
| Propositor | `app/ponto/propositor.py:39` | Hoje valida HH:MM em `:52`–`:54` e calcula rajada por minutos de relógio em `:55`–`:57`; migrar cálculos temporais conservando data e manter o bucket não avaliável para `None`. Esse bucket já é contrato explícito. (`:18`–`:21`, `:44`–`:50`) |
| Command do propositor | `app/ponto/management/commands/propor_reorganizacao.py:104` | Hoje filtra HH:MM e chama `atribuir_global` em `:107`; não pode descartar instantes porque `_h_ok` espera tamanho cinco. Preservar identidade no adaptador temporal; desconhecido vai ao bucket já contado em `:78`–`:79`, `:123`–`:126`. |
| Ausência parcial | `app/ponto/turnos.py:2667` | `horas_reais_do_dia` lê órfã como minuto do próprio dia; a reconstrução pela linha é explícita em `:2649`. Substituir por deslocamento calculado do instante real em relação à origem temporal declarada do turno, preservando dia seguinte; legado gera desconhecimento, que o juiz já expressa como `nao_sei`. (`:2616`–`:2643`) |
| Existência/contagem | `app/folha/causas.py:43`, `:100`; `app/ponto/status_dia.py:188`; `app/escala/services/furos_diarios.py:22`; `app/ponto/services/bordas_realizado.py:49`; `app/ponto/services/simular_dia.py:12`; `app/ponto/management/commands/detectar_par_relampago.py:140` | Não precisam converter instantes para horas. Precisam distinguir lista conhecida, ausência conhecida e identidade desconhecida; preservar contagem lavrada quando o consumidor pergunta somente quantidade. |
| Suprajuiz | `app/ponto/supra_juiz.py:379`, `:514`, `:520` | Existência/contagem e descrição; formatar só descrição. Não deixar `None` de legado ativar o recount por batidas em `:521`, nem virar falso em `bool`. |

A grade atual **não publica** `n_orfas` no resultado do leitor; publica `n_ok`, `n_casadas` e `orfas`. Para conservar perguntas de quantidade sem confundir legado com zero, o pré-desenho deve prever transportar a contagem já lavrada, separada da disponibilidade de identidades; isso não cria outra lista de órfãs. (`app/escala/services/leitor_celula.py:417`–`:424`; lavra da contagem: `app/ponto/services/cartorio.py:742`)

`chamados/reconciliador.py:1377` e `core/management/commands/gerar_diagrama.py:854` usam homônimo `orfas` para perguntas/autoridades, respectivamente; não são leitores da órfã de batida. (`app/chamados/reconciliador.py:1319`; `app/core/management/commands/gerar_diagrama.py:783`)

**Autoridades e fronteiras**

O juiz do marco já é `_match_marcos`, e o juiz da pertença ao turno já é `turnos_do_colab`; a mudança de representação não exige outro juiz. (`app/core/juizes.py:208`, `:212`; `app/docs/JUIZES.md:20`, `:27`) A cadeia absoluta já tem escritor único `encadear_marcos`, inclusive virada de meia-noite. (`app/escala/utils.py:330`–`:340`)

Servem como especificação do diff: conservação das promoções de célula a órfã, dry-run com instante, retirada de uma única ocorrência na reconciliação, órfãs sem marcos e ignorância explícita do legado; a implementação paralela não serve como base. [não verificado na cópia: classificação do anexo fornecido]

REDs a exigir: mesma HH:MM em datas diferentes; órfãs no mesmo minuto com segundos distintos; noturno com órfã antes/depois da meia-noite; mesmo instante com offsets diferentes; legado HH:MM/count-only/ata ausente; lista vazia explícita; promoção por régua; dry-run sem consumir a passagem definitiva; remoção preservando outras ocorrências; `None` atravessando todos os consumidores sem `or []`. São propostas de prova, não testes executados. [não verificado] [nao verificado]

Perguntas de lei restantes neste recorte: **0**; A5 pode preservar a recorrência já declarada por pertença a faixas temporais aware, sem casar identidades por HH:MM e sem fabricar datas. (`app/ponto/services/esmeril_espelho.py:19`, `:125`–`:139`)

## 12/11 16:43 · sessao 483dbe76f6e0 · - · - · origem=Ronald (tmux)

Censo de (iii), com identificação de funções por AST e inspeção das operações; a lista abaixo usa **uma linha física por sítio**, inclui projeção/parse que alimenta casamento e exclui mero render final. A exaustividade de análise dinâmica/reflexiva é [não verificado].

```python
{
  "app/escala/orfa_classe.py": [32,44,74],
  "app/ponto/services/cluster_espurio.py": [34,35],
  "app/ponto/services/cartorio.py": [623,628,666,667],
  "app/ponto/propositor.py": [
    26,27,31,55,57,94,98,99,107,122,123,124,
    135,136,155,156,176,177,249,250,273,276,
    289,297,315,340,342,343,345,351
  ],
  "app/ponto/turnos.py": [2611,2640,2641,2667],
  "app/ponto/management/commands/tripwire_celulas.py": [
    129,165,174,177,192,195,280,285
  ],
  "app/ponto/services/vigia_pareador_celula.py": [110,111],
  "app/chamados/services/materializacao.py": [177,178,182],
  "app/ponto/management/commands/propor_reorganizacao.py": [93,94,95,107],
  "app/chamados/detector_anomalias.py": [213,214],
  "app/ponto/motor_calculo_v2.py": [3069],
  "app/escala/servico_jornada.py": [455,456],
  "app/colaboradores/services/calendario.py": [625,626],
  "app/relatorios/pdf_espelho.py": [178,609,716,720,960,963,964],
  "app/escala/utils.py": [429,431,1675,2005,2006,2028,2032,2034,2039],
  "app/ponto/services/flip_auto.py": [155,161,162,219,221,222,232,233,238],
  "app/escala/regua_defesa.py": [415,418,419,433,436,438,455,456]
}
```

Funções e circulação:

| Arquivo | Funções correspondentes | Valor / perda da data |
|---|---|---|
| `escala/orfa_classe.py` | `_minutos`, `_runs_de_rajada`, `classificar_orfas` | HH:MM → minutos; data já ausente. `:32`, `:44`, `:74` |
| `ponto/services/cluster_espurio.py` | `_leitura_da_celula` | HH:MM → minutos → diferença ≤15; data ausente. `:34`, `:35` |
| `ponto/services/cartorio.py` | `lampadas_do_dia._min`, `lampadas_do_dia` | Órfas HH:MM → `_reais` → proximidade de marco ≤30 min; data ausente. `:623`, `:628`, `:666`, `:667` |
| `ponto/propositor.py` | `_tem_hora`, `_min`, `dia_degenerado`, `atribuir_global_turno`, `atribuir_global`, `_tem_empate`, `plantio_preenche_furo`, `primeiro_furo_posicional`, `dia_posicional_completo` | HH:MM/minutos; a DP casa marcas e classifica sobras; coletor elimina data de timestamp em `:345`; reconstrução +1440 em `:107` não recupera data original. |
| `ponto/turnos.py` | `_minutos_do_dia`, `periodo_da_parcial_no_dia`, `horas_reais_do_dia` | HH:MM → minutos; órfa recebe deslocamento da própria hora sem data, conforme `:2649` e implementação `:2667`; comparação com parcial em `:2641`. |
| `tripwire_celulas.py` | `_hora_hhmm`, `sobra_na_borda`, `confirmada_por_instante`, `medir_sobra_com_tipo_trocado` | HH:MM → `time`; casamento circular via `perto_do_marco` em `:177`; datetime → HH:MM em `:192`, `:195`, `:280`. `:195` também exige distância por instante. |
| `vigia_pareador_celula.py` | `censo` | Timestamp → HH:MM → pertença à lista de órfas civis; perde data no valor comparado. `:110`, `:111` |
| `chamados/services/materializacao.py` | `_plantio_preenche_furo_na_data` | Marcas incluem `ata['orfas']` em `:181`; plantio timestamp → HH:MM e DP em `:182`. |
| `propor_reorganizacao.py` | `Command.handle` | Validação HH:MM `:93`–`:95`; órfas entram em `marcas` `:104`, comparadas pela DP chamada em `:107`. |
| `chamados/detector_anomalias.py` | `detectar_batida_fora_escala` | Candidata timestamp → HH:MM → igualdade com célula; ausência de casamento produz `batida_fora_escala`. `:213`, `:214`, `:218` |
| `motor_calculo_v2.py` | `calcular_mes` | Entrada candidata datetime → HH:MM → igualdade com célula retornada pelo matcher. `:3062`, `:3069` |
| `escala/servico_jornada.py` | `classificar_falta` | Timestamp → HH:MM → pareamento posicional. `:455`, `:456` |
| `colaboradores/services/calendario.py` | `contexto_calendario` | Timestamp → HH:MM → pertença; marca `b.orfa=True`. `:625`, `:626`, `:627` |
| `relatorios/pdf_espelho.py` | `_entrada_espuria`, `_coletar_dados_espelho`, `_build_story` | Timestamp → HH:MM; igualdade/pertença de órfa `:178`, `:609`, `:964`; filtro textual de anomalias usa somente hora `:716`, `:720`. |
| `escala/utils.py` | `marco_ocupado_pela_batida`, `montar_grade_prevista_periodo_por_turno` | Timestamp → HH:MM; igualdade `:431`; pertença órfa `:1675`, `:2006`, `:2032`; ordenação de horas `:2034`; remoção por hora `:2039`. |
| `ponto/services/flip_auto.py` | `_mn`, `_dist_turno`, `etiquetas_pela_celula` | Timestamp → HH:MM `:232`; DP gera sobra sem slot `:233`, `:236`; distância circular em HH:MM `:238`, `:161`, `:162`. |
| `escala/regua_defesa.py` | `distancia_do_marco`, `_min_do_dia_f`, `perto_do_marco` | datetime/time → minutos do dia `:436`, `:438`; distância circular `:418`, `:419`; caminho de órfa é chamado pelo tripwire `:177`. |

Todas são LEITURA de identidade/tempo, exceto mutações de classificação/coleções: `calendario.py:627` escreve `b.orfa`, `pdf_espelho.py:609` escreve `_b.orfa`, `utils.py:2039` remove órfa por HH:MM; atribuições locais e produções de listas escrevem valores intermediários nos sítios indicados.

Produção pura de órfas, separável dos casamentos acima:

```python
{"app/escala/utils.py": [326,621,662,799,1832,1834]}
```

`_celula_do_marco:326` produz HH:MM que pode ser promovido a órfa em `:1722`/`:1733`; `_match_marcos:621` e `:662` eliminam a data diretamente; conservação faz o mesmo em `montar_grade_prevista:799` e no builder por turno `:1832`/`:1834`.

Casamento de **marco**, que determina promoção de batida a órfa, merece inventário separado: `app/escala/utils.py:1719` e `:1724`, com promoção em `:1722` e `:1733`. São datetime de marco → HH:MM para chave da régua; não comparam diretamente identidade de órfa.

Não classificar como perda da data o mapa `papel_por_minuto_da_ata`: seu normalizador preserva a data, embora retire fuso/segundos em `app/ponto/turnos.py:1604`; a mesma chave datada identifica candidatas/órfas em `:1616`, `:1644`, `:1804`.

HTML/JS por busca textual, **não AST**: transporte/render de `orfas_grade` em `app/templates/ponto/espelho.html:151` e `:240`; render de `b.orfa` em `app/templates/colaboradores/partials/_calendario_grade.html:121`. Resultado JS relevante às chaves/funções pedidas: nenhum encontrado pela busca textual [não verificado quanto a nomes computados].

## 12/11 16:43 · sessao 842846aab3c0 · - · - · origem=Ronald (tmux)

Censo da fonte e escritores, somente leitura.

- A lavra atual é `app/ponto/services/cartorio.py:730::ata_do_dia`; grava `n_orfas=len(...)` em `:742` e `'orfas': [str(h) ...]` em `:745`. O campo persistido é JSON, sem validação de formato na declaração: `app/escala/models.py:1778`.
- O conteúdo produtivo chega como **strings HH:MM**, sem data, segundos ou offset: `_match_marcos` perde essas partes em `app/escala/utils.py:621,:662`; sobras fora do envelope em `:799,:1832,:1834`; promoção de célula a órfã em `:1722,:1733`. Antes da perda, são datetimes aware derivados de `timezone.localtime(b.timestamp)` em `:683,:1323`; configuração padrão `America/Sao_Paulo` e `USE_TZ=True` em `app/config/settings/base.py:93,:95`. O fuso efetivamente ativo em uma execução de produção é [não verificado].
- Portanto trocar apenas o `str(h)` da ata por outra serialização não recupera o instante; é necessário preservar o datetime desde os pontos acima, inclusive nas promoções, e serializar `isoformat()` **no mesmo campo `orfas`**. O instante original da célula casada também se perde hoje em `_celula_do_marco`, `app/escala/utils.py:326`; a promoção precisa conservá-lo, em vez de reutilizar `hora` (`:1722,:1733`).

**Todos os escritores produtivos da ata identificados pelo AST e leitura:**

| Escritor | Efeito e fonte |
|---|---|
| `cartorio.ata_do_dia` | Constrói a ata, incluindo órfãs: `app/ponto/services/cartorio.py:738,:745`. |
| `lavrar_veredito` | Atribui ata inteira em `app/ponto/portas/celula.py:1006,:1033`; preserva apenas chaves declaradas de outra operação em `:997` — atualmente `precedencia`, `:104`. |
| `carimbar_precedencia` | Copia a ata, troca `precedencia`, atribui a ata inteira e salva: `app/ponto/portas/celula.py:1083,:1084,:1085,:1087`; transporta órfãs sem interpretá-las. |
| `escala.utils` — memória | Cria ata sintética com órfãs em `app/escala/utils.py:1887,:1895` numa cópia explicitamente não salva (`:1886`). Deve migrar junto para não apresentar outra forma de ata ao leitor único. |
| `celula_como_o_julgamento_deixa` — memória | Copia ata inteira para projeção, `app/ponto/portas/celula.py:945`; cópia sem persistência declarada em `:933,:934`. |
| Cartório — memória | `SimpleNamespace(ata=...)` com ata do julgamento em `app/ponto/services/cartorio.py:1436`; publicação transitória da mesma ata em `:1440`. |

A fachada do modelo apenas encaminha: `app/escala/models.py:1819,:1827`. O cartório chama a porta com `save=False` nos ramos “nunca bateu” e normal (`app/ponto/services/cartorio.py:1292,:1853`); `carimbar(...,lavrados=_cv)` efetiva a gravação em `:1314,:1873`, via `app/ponto/portas/celula.py:1072::salvar_e_emitir`, cujo `cel.save(update_fields=campos)` está em `:905`, junto com emissão do evento (`:904,:907`).

`dispensar_por_ausencia` e `_ata_ate_o_teto` modificam lâmpadas/`n_missing`, sem escrever órfãs (`app/ponto/services/cartorio.py:805,:812,:913,:917`). A reversão de regeneração grava DNA/trabalho/carimbos, sem ata (`app/ponto/portas/celula.py:786,:794`).

AST executado em memória sobre 3.559 arquivos Python da cópia, buscando atribuições `.ata`, subscritos `['ata']`, keywords `ata=`, `setattr(...,'ata',...)`, `update` e `bulk_update` com ata; nenhum `update(ata=...)`/`bulk_update(...,'ata')` produtivo adicional apareceu [não verificado: instrumento não salvo, saída da execução disponível no histórico]. Censo de sintaxe não prova ausência de metaprogramação arbitrária [não verificado].

Snapshots **leem** a ata e escrevem arquivo externo; não são escritores da CelulaDia: `bin/snapshot_o118_col221.py:18,:30`; `app/ponto/management/commands/celula_veredito_velho.py:108,:112,:113`. O primeiro descreve reversão em texto (`bin/snapshot_o118_col221.py:27`), sem implementá-la nesse arquivo; o segundo relavra pelo cartório (`app/ponto/management/commands/celula_veredito_velho.py:144`) e desfaz o ensaio por rollback (`:156,:157`).

**Leitor importante dentro da própria fonte:** `lampadas_do_dia` recebe órfãs em `app/ponto/services/cartorio.py:599`. O parser `_min` fatia HH:MM (`:621,:623`) e alimenta `_reais` com órfãs em `:628`. O ramo `j is None` procura proximidade de até 30 minutos (`:666`) e acende lâmpada com luz HH:MM (`:672,:673`), sem retirar a batida da lista de órfãs; o próprio comentário registra a obrigação “acesa e órfã disjuntas” (`:702,:703`). Esse consumidor precisa da migração por instante e do juiz geométrico existente; passar ISO diretamente para `_min` é incorreto (`:623`).

**Autoridades para a implementação:** `_match_marcos` já responde qual marco previsto a batida ocupa (`app/core/juizes.py:212`, `app/docs/JUIZES.md:27`); turno/data_turno é `turnos_do_colab` (`app/core/juizes.py:208`); marcos congelados são `EscalaColaborador.marcos_do_dia` (`:213`); `CelulaDia` só pode ser escrita pela porta (`app/core/portas.py:107,:112`). A fonte única já é padrão registrado (`app/docs/GLOSSARIO.md:11`); não precisa nascer outro juiz.

**Avisos enviados ao pai para o censo de leitores:** leitura direta também em `app/ponto/juiz_batida.py:161`; `cluster_espurio` fatia HH:MM em `app/ponto/services/cluster_espurio.py:29,:31,:34`; `app/ponto/turnos.py:2667` converte órfãs a minutos relativos, com pressuposto de data da linha explícito em `:2649`. Esses sítios entram na migração, embora não sejam escritores.

## 12/11 16:43 · sessao ffcae5163e6e · - · - · origem=Ronald (tmux)

HTML/JS — busca textual, **não AST**:

| Arquivo:linha | Sítio | Operação/tipo | Perda da data |
|---|---|---|---|
| `app/templates/ponto/espelho.html:151` | calendário: `{% for o in d.orfas_grade %}` | LEITURA, HH:MM str, render | Já chega sem data |
| `app/templates/ponto/espelho.html:240` | detalhe: `{% for o in dia.orfas_grade %}` | LEITURA, HH:MM str, render em `:241` | Já chega sem data |
| `app/templates/colaboradores/partials/_calendario_grade.html:121` | `{% if b.orfa %}`, render `b.timestamp\|date:"H:i"` | LEITURA de classificação bool; datetime → texto no render | Texto sem data; objeto conserva datetime |

A busca integral de `*.html`/`*.js` pelos nomes `orfas_grade`, `_orfas_da_ata`, `orfas_da_ata`, `orfas_instantes`, `\borfas\b`, `\borfa\b` encontrou somente os sítios acima e o texto explicativo de ranking, sem leitura de órfã, em `app/templates/relatorios/ranking_furos.html:42`.

Falsos positivos examinados, fora do casamento de órfã:

- Confirmação de resposta `time`/`datetime-local`: `app/templates/chamados/meu_atendimento.html:252`, `:253`, `:255`.
- Filtro pelo horário inicial cadastrado da escala: `app/static/js/hasner-filter.js:24`, `:30`.
- Endereço `tipo:id`, não hora: `app/static/js/live-cell.js:36`.
- Conversão de horário da agenda: `app/templates/colaboradores/partials/agenda_edit_dia.html:255`.
- Exibição do relógio e confirmação local de batida salva: `app/templates/ponto/bater_ponto.html:317`, `:547`, `:599`; `app/templates/ponto/bater_ponto_v2.html:164`, `:336`, `:349`.

**Spec reprovada — lista de acréscimos, separada da cópia.** Nas referências seguintes, `+N` é o início do hunk **do anexo**, não linha da cópia. A ancoragem por hunk é necessária porque o anexo não existe como arquivo local [não verificado].

| Arquivo no anexo / hunk | Novos sítios, transportes ou operações |
|---|---|
| `escala/services/leitor_celula.py`, `+177` | Parâmetro `orfas_instantes` em `_orfas_da_ata`; novo `ata.get('orfas_instantes')`; validação de lista/tamanho; `orfas_instantes.extend(...)` acoplado por `zip(horas, instantes)`. HH:MM e ISO str seguem em listas paralelas. A leitura `ata.get('orfas')` é modificada, não adicionada. |
| mesmo, `+393` | Novo literal `'orfas_instantes': []` na base de dia. |
| mesmo, `+421` | Novo coletor `_orfas_instantes=[]`; chamada `_orfas_da_ata(..., orfas_instantes=...)`; novo campo de saída `orfas_instantes=...`. |
| `escala/utils.py`, `+320` | `_celula_do_marco`: novo `'instante': instante.isoformat()`; mantém `'hora': instante.strftime('%H:%M')`. |
| mesmo, `+584` | `_match_marcos`: novo parâmetro `orfas_instantes`. |
| mesmo, `+622` | Nova lista `_sobras` de datetimes; novo `extend(t.isoformat()...)`; mantém geração HH:MM separada. |
| mesmo, `+667` | Novo coletor de `B[j].isoformat()` para órfãs. |
| mesmo, `+768` | `montar_grade_prevista`: novo campo vazio `orfas_instantes` em folga. |
| mesmo, `+782` | Novo coletor e argumento para `_match_marcos`. |
| mesmo, `+796` | Novo campo `orfas_instantes` no dia de trabalho. |
| mesmo, `+808` | Novo append ISO para sobra externa; nova leitura conjunta via `zip(_d['orfas'], _d['orfas_instantes'])`; ordenação pelo primeiro componente, **HH:MM**; reescrita sincronizada das duas listas. |
| mesmo, `+975` | `clipar_grade_janela`: duas novas escritas zerando `orfas_instantes`, em futuro e fora da janela. |
| mesmo, `+1021` | `_montar_grade_prevista_periodo_legacy`: novo campo vazio em segmento sem escala. |
| mesmo, `+1050` | Mesmo builder: novo campo vazio em dia sem dados. |
| mesmo, `+1100` | `montar_realizado_grade`: leitura antecipada `orfas = list(gd.get('orfas') or [])` e retorno dessas órfãs sem marcos. A leitura substitui a leitura antiga no retorno final; muda também o fluxo. |
| mesmo, `+1173` | `_aplicar_folgas_foto`: nova escrita zerando `orfas_instantes`. |
| mesmo, `+1252` | `_marcar_ausencias`: nova escrita zerando `orfas_instantes` em falta coberta. |
| mesmo, `+1296` | Mesmo overlay: duas novas escritas zerando `orfas_instantes`, em ausência integral e justificativa. |
| mesmo, `+1482` | `montar_grade_prevista_periodo_por_turno`: novo mapa `instantes_orfas_por_data`. |
| mesmo, `+1613` | Novo armazenamento de lista vazia nesse mapa. |
| mesmo, `+1696` | Novo coletor `_orf2_instantes`; argumento para `_match_marcos`; novo set de datetimes via `fromisoformat`. Substitui o set de HH:MM. |
| mesmo, `+1723` | Nova condição de candidatura `locs[k][0] in _orfset`, datetime, substituindo membership HH:MM. |
| mesmo, `+1749` | Novo coletor `orfas_instantes` e argumento para `_match_marcos`. |
| mesmo, `+1773` | Novo append de `_cc['instante']` quando célula vira órfã. |
| mesmo, `+1785` | Segundo novo append de `_cc['instante']`, excedente de intervalo. |
| mesmo, `+1811` | Novo armazenamento do coletor em `instantes_orfas_por_data[d]`. |
| mesmo, `+1855` | Novo campo vazio em dia de folga. |
| mesmo, `+1864` | Novo campo preenchido do mapa em dia de trabalho. |
| mesmo, `+1887` | Dois novos appends ISO na conservação de sobras; nova leitura `zip` e reescrita sincronizada das duas listas, ainda ordenadas por HH:MM. |
| mesmo, `+2064` | Nova leitura `dias_out[...].get('orfas_instantes')`; localização por `fromisoformat(_iso) == _inst_t`; mantém `_hhmm_t = _inst_t.strftime('%H:%M')` mais abaixo. |
| mesmo, `+2101` | Nova mutação `orfas_instantes.pop(_ix_orfa)` e remoção posicional no alias `_lista_orf`; substitui remoção pelo valor HH:MM. |
| `ponto/services/cartorio.py`, `+737` | `ata_do_dia`: nova escrita do campo da ata `'orfas_instantes'` e nova leitura `dia.get('orfas_instantes')`, ISO str. Mantém escrita HH:MM em `ata['orfas']`. |
| `ponto/services/espelho.py`, `+494` | `_grade_builder`: nova escrita `dia_grade['orfas_instantes']=[]`, descartando os instantes não lavrados. |
| mesmo, `+857` | `montar_dias`: nova leitura de `_gd.get('orfas_instantes')`, nova publicação `'orfas_instantes_grade'`. |
| `relatorios/pdf_espelho.py`, `+171` | Nova função `_orfas_publicadas`: novas leituras `dia.get('orfas_grade')` e `dia.get('orfas_instantes_grade')`; validação de tamanho/ISO/fuso; retorna `set[datetime]`, vazio ou `None`. `_entrada_espuria` passa a receber e comparar instantes. |
| mesmo, `+485` | Novas chamadas `_orfas_publicadas(d)` por dia; novo set `_orfas_instantes` filtrado por timestamps de batidas da tela. |
| mesmo, `+534` | Chamada de `_entrada_espuria` alterada para novo conjunto de instantes. |
| mesmo, `+584` | Nova escrita `orfas_sem_instantes`; nova comparação `_b.timestamp in _orfas_instantes` para escrever `_b.orfa`. |
| mesmo, `+694` | **Novo casamento textual ainda truncado:** conjunto `_espurias_datadas` com `strftime('%d/%m %H:%M')`; filtro por substring no texto da anomalia. Preserva dia/mês; perde ano, segundos e offset. Não compara datetime. |
| mesmo, `+896` | Nova leitura `dia.get('orfas_sem_instantes')` para render “não sei”. |
| mesmo, `+941` | Novo conjunto `_espurias_instantes` de `b.timestamp`; leitura `pp.entrada` e membership por datetime, substituindo HH:MM. |

Diferença material entre anexo e cópia: o PDF atual ainda remonta grades e lê `orfas` em `app/relatorios/pdf_espelho.py:472`, `:477`, `:544`, `:552`; o hunk `+485` do anexo pressupõe um contexto antigo já baseado na tela. Logo, não afirmar aplicabilidade do patch ao HEAD [não verificado].

O texto HH:MM continua sendo a fonte atual em `app/escala/services/leitor_celula.py:187` e `app/ponto/services/cartorio.py:745`; a spec adiciona transporte paralelo de ISO nos hunks indicados, sem substituir a chave `orfas`.

## 12/11 16:43 · sessao 2746476993e6 · - · - · origem=Ronald (tmux)

Censo AST executado em memória com `python3 -B` (sem gerar arquivos); no escopo de tela/PDF, os sítios atuais e a migração proposta são os seguintes. [não verificado: execução não persistiu relatório]

| Sítio atual | Comportamento hoje | Pré-desenho |
|---|---|---|
| `app/escala/services/leitor_celula.py:180`, `:187`, `:190` | `_orfas_da_ata(ata)` entrega strings da ata ou `'?' * n_orfas`. | Substituir pela porta pública `orfas_da_ata(celula) -> list[datetime] \| None`; todo elemento exige ISO completo e offset; HH:MM, valor inválido ou ausência de lastro retorna `None`, preservando `[]` como zero conhecido. Proposta ancorada no sítio a substituir. |
| `app/escala/services/leitor_celula.py:420` | `grade_da_celula` chama o leitor privado e publica `dia['orfas']`. | Chamar a porta com `cel`; transportar a mesma lista de datetimes/`None`, sem recompor pela data do dia. |
| `app/escala/utils.py:1084`, `:1110` | `montar_realizado_grade` devolve `[], []` sem marcos; no caminho normal faz `list(gd.get('orfas') or [])`. | Não descartar órfãs em dia sem marcos; preservar `None` explicitamente. O ajuste equivalente no diff serve como caso/spec, sem sua dupla lista. |
| `app/ponto/services/espelho.py:519`, `:689`, `:844` | Tela lê grade da célula, passa pelo adaptador e escreve `orfas_grade`. | `orfas_grade` pode conservar o nome, mudando seu tipo para lista aware/`None`; nenhuma lista paralela de instantes. |
| `app/ponto/services/espelho.py:489`, `:530`, `:537`, `:560` | Trecho visual anterior à competência ou ata agregada passa pelo builder antigo. | A degradação de geometria não autoriza publicar órfãs reconstruídas como lavradas: consultar o mesmo leitor da célula e manter `None` se a ata não sabe. |
| `app/templates/ponto/espelho.html:151`, `:240` | Render imprime a string `o` diretamente; órfãs estão dentro do ramo que exige `realizado_grade`. | Render final `date:"H:i"` sobre instante e ramo explícito de `None`; desenhar órfã mesmo quando o dia não tem células realizadas. |
| `app/colaboradores/services/calendario.py:380`, `:381`, `:622`, `:625` | Calendário guarda set de órfãs em mapa por `(dia, mês)` e compara com `b.timestamp.strftime('%H:%M')`, escrevendo `b.orfa=True` em `:627`. | Mapa por data completa para localizar a resposta, mas pertencimento por `b.timestamp` aware; `None` não vira set vazio nem `False`. |
| `app/templates/colaboradores/partials/_calendario_grade.html:121` | Lê `b.orfa` e desenha timestamp com `date:"H:i"`. | Manter HH:MM apenas nesse render; exibir estado desconhecido sem afirmar classificação negativa. |

PDF atual difere da base mostrada no diff: importa `grade_espelho_janela` como `_grade` (`app/relatorios/pdf_espelho.py:193`), remonta o builder para obter órfãs duas vezes (`:475`, `:543`) e só depois consulta a tela (`:598`). Logo, não aplicar o hunk do anexo por transplante; reescrever o caminho atual para consumir a resposta da tela oriunda do leitor único (`app/relatorios/pdf_espelho.py:475`, `:543`, `:598`; desenho do anexo: [não verificado no filesystem]).

Todos os comparadores do PDF que precisam migrar:

- União de HH:MM de toda a janela em `app/relatorios/pdf_espelho.py:472`, `:477`; `_entrada_espuria` compara entrada por HH:MM em `:178`, chamado em `:513`. Trocar por pertencimento do datetime aware e resultado triestado; desconhecido não pode virar “entrada não espúria”. Proposta nos sítios citados.
- Atribuição da resposta à linha por `data_fim - índice` em `app/relatorios/pdf_espelho.py:550`, `:551`, `:552`, seguida de `b.orfa` por HH:MM em `:609`. Consumir a resposta já publicada pela tela e comparar timestamp real; a data localiza a célula, não fabrica o instante. Proposta nos sítios citados.
- Supressão de anomalia por substring HH:MM em `app/relatorios/pdf_espelho.py:716`, `:719`, `:720`. **Também precisa sair**; o diff apenas adiciona `%d/%m`, mantendo identidade textual sem ano/segundos/offset. A fonte atual já perde o instante ao fabricar mensagens em `app/ponto/adaptador_motor.py:33`, `:35`, `:40`, `:42`; para supressão exata, transportar a identidade estruturada do fato desde esse adaptador e renderizar a mensagem no fim. Não reconstruir instante parseando texto. Proposta fundamentada nos pontos de perda citados.
- Supressão dos alertas de turno por HH:MM em `app/relatorios/pdf_espelho.py:960`, `:963`, `:964`. Trocar o conjunto por `b.timestamp` e testar `pp.entrada` diretamente; `None` precisa chegar ao render como desconhecido. Proposta nos sítios citados.
- O HH:MM final legítimo fica em `app/relatorios/pdf_espelho.py:881` e `:885`; `b.orfa` controla marca e legenda em `:876`, `:877`, `:1024`. O campo é escrito pelo coletor em `:609`.

Leitor direto adicional do escopo PDF: `relatorios.cartao_pela_celula.orfas_e_isencao_da_ata` lê `ata.get('orfas')` em `app/relatorios/cartao_pela_celula.py:288`. A busca de referência encontrou somente seu teste (`app/relatorios/tests/test_cartao_pela_celula.py:159`, `:166`) além da definição (`app/relatorios/cartao_pela_celula.py:277`); candidato à remoção pela L-111, não à criação de outro parser (`CLAUDE.md:191`).

Casos/spec aproveitáveis: órfã sem marcos não pode sumir (`app/escala/utils.py:1084`); somente órfã sem `realizado_grade` precisa aparecer no template (`app/templates/ponto/espelho.html:151`, `:231`, `:240`); mesmo HH:MM em dias distintos deve marcar somente o instante correspondente, atingindo os quatro comparadores do PDF acima; ata HH:MM antiga deve devolver desconhecido na tela, calendário e PDF (`app/escala/services/leitor_celula.py:187`; proposta [não verificado]).

Autoridades preservadas: grade/match e cluster guard estão em `app/docs/JUIZES.md:27`; geometria de turno em `:20`; encadeamento cross-midnight em `:24`. A mudança de transporte/leitura não deve criar novo julgamento nesses consumidores (`CLAUDE.md:20`, `:21`).

## 12/11 16:39 · sessao a061d30af7be · - · - · origem=Ronald (tmux)

Recenso na árvore após a cura, com `ast.parse` e inspeção dos operandos e consumidores, sem editar arquivos.

| Sítio | Classe | Interpretação |
|---|---|---|
| `relatorios/pdf_espelho.py:698` e `:702` | **IDENTIDADE — residual** | O conjunto `DD/MM HH:MM` alimenta substring que elimina anomalia. Ainda perde segundos. |
| `escala/utils.py:1760` | EXIBIÇÃO/projeção | Chave `(tipo, HH:MM do marco previsto)` localiza coluna da régua. Não compara a hora da batida. |
| `escala/utils.py:1772` | EXIBIÇÃO/projeção com promoção | A ausência do **marco previsto** na régua promove a célula correspondente; hora e instante são transportados juntos em `:1775-1776`. Não identifica a órfã pela sua hora. |
| `escala/utils.py:1778` | EXIBIÇÃO/projeção com promoção | Marco interno fora da régua ocupa slot `I`; se faltam slots, a célula inteira vira órfã, conservando o instante em `:1787-1788`. |
| `escala/utils.py:1797`, `:1800` | EXIBIÇÃO/projeção | Chave prevista seleciona e deduplica colunas das pontas. |
| `escala/utils.py:1801` | EXIBIÇÃO/projeção | `.index(_key4)` retorna a coluna do marco previsto. |
| `escala/utils.py:1809-1810` | EXIBIÇÃO/projeção | `in` e `.index` mapeiam cada marco previsto à coluna. A pertença da **batida** ao turno é decidida depois por instante em `:2104`. |
| `escala/utils.py:1897` | ORDENAÇÃO | Ordena pares hora/instante pela hora de exibição; conserva os pares em `:1898-1899`. |
| `escala/utils.py:626`, `:669` | EXIBIÇÃO | HH:MM serializado em paralelo com os instantes em `:628`, `:671`; identificação usa índices e datetimes. |
| `escala/services/leitor_celula.py:287`, `:302-303` | EXIBIÇÃO/projeção | Chaves de marcos previstos remapeiam células na régua. |
| `escala/services/leitor_celula.py:347-350` | EXIBIÇÃO/projeção | Deduplicação de colunas previstas por `(tipo, HH:MM)`. |
| `escala/services/leitor_celula.py:25`, `:30-35`, `:121` | EXIBIÇÃO/status | Truncamento para calcular desvio da luz em relação ao previsto; não decide identidade ou pertença de órfã. |
| `relatorios/pdf_espelho.py:714-715`, `:863`, `:867`, `:889`, `:939`, `:1030`, `:1077` | EXIBIÇÃO | Formata escala, batidas, procedência e emissão. Os filtros usam `b.orfa`, decidido em `:590`. |

Verificação dos sítios de identidade após a cura:

- Dry-run compara `datetime` em `escala/utils.py:1702`, `:1726`.
- Localizador da órfã compara ISO reconstruído com instante em `escala/utils.py:2068-2069`.
- Guarda usa timestamps do turno em `escala/utils.py:2092`; interseção por instante em `:2093-2095`; instante da célula em `:2100-2102`; unicidade em `:2103`; pertença e ordem em `:2104`.
- Leitor mantém pareamento posicional em `escala/services/leitor_celula.py:191-193`.
- PDF identifica órfãs e períodos por timestamp em `relatorios/pdf_espelho.py:195`, `:500`, `:590`, `:944-947`.

O achado de identidade reduzida remanescente neste escopo é o filtro textual do cabeçalho do PDF (`relatorios/pdf_espelho.py:698-702`).

## 12/11 16:36 · sessao a061d30af7be · - · - · origem=Ronald (tmux)

Censo preliminar feito com `ast.parse`, sem editar arquivos.

| Achado | Classe | Razão |
|---|---|---|
| `utils.py:2093,2097,2099` | IDENTIDADE | A redução para HH:MM decide pertença/unicidade de órfã e pertença da célula ao turno. |
| `pdf_espelho.py:698-702` | IDENTIDADE | Conjunto e substring sem segundos eliminam anomalias de outra batida no mesmo minuto. |
| `utils.py:1760` | EXIBIÇÃO/projeção | Chave `(tipo, HH:MM previsto)` alinha célula na coluna do marco. |
| `utils.py:1772,1778` | EXIBIÇÃO/projeção | Compara o marco previsto com a régua; quando promove uma célula, conserva seu instante (`utils.py:1776,1788`). |
| `utils.py:1797-1801` | EXIBIÇÃO/projeção | Identifica e deduplica colunas de marcos previstos; não compara órfãs/batidas. |
| `utils.py:1809-1810` | EXIBIÇÃO/projeção | `.index` localiza coluna do marco previsto. |
| `utils.py:1897` | ORDENAÇÃO | Ordena pares hora/instante, conservando o pareamento (`utils.py:1898-1899`). |
| `leitor_celula.py:287,302-303` | EXIBIÇÃO/projeção | Remapeia células por coluna prevista. |
| `leitor_celula.py:347-350` | EXIBIÇÃO/projeção | Deduplica colunas previstas da régua. |
| `leitor_celula.py:24-35,121` | EXIBIÇÃO/status | Calcula desvio, sem decidir identidade/orfandade. |
| `utils.py:626,669` | EXIBIÇÃO | Serializa HH:MM junto dos respectivos instantes (`utils.py:628,671`). |
| `pdf_espelho.py:863,867,939` | EXIBIÇÃO | Formata batidas cuja identidade já foi estabelecida por timestamp (`pdf_espelho.py:590`). |

Os caminhos já datados usam `datetime`: dry-run (`utils.py:1702,1726`), localizador da reconciliação (`utils.py:2068-2069`), órfãs publicadas/PDF (`pdf_espelho.py:195,500,590,944-947`) e pareamento posicional do leitor (`leitor_celula.py:191-193`). [nao verificado]

A célula de `_celula_do_marco` já carrega `instante.isoformat()` com offset (`utils.py:327-328`).

Aguardo a cura para repetir o censo na árvore final.

## 12/11 16:09 · sessao 7d019a0204b9 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**1. NAO PROCEDE a cura dos cinco achados como classe.** Quatro correções estão presentes; a reconciliação corrigiu a localização e a remoção, mas conservou outra decisão por HH:MM. (`app/escala/utils.py:2068`; `app/escala/utils.py:2097`)

| Achado | Situação após c19 |
|---|---|
| Reconciliação remove a órfã errada | Agora localiza pelo instante completo e remove a mesma posição nas duas listas. Porém, a guarda ainda associa órfãs e células ao turno por minuto. (`app/escala/utils.py:2068`; `app/escala/utils.py:2105`; `app/escala/utils.py:2097`; `app/escala/utils.py:2099`) |
| Dry-run usa conjunto de horas | Corrigido para conjunto de `datetime`. (`app/escala/utils.py:1702`; `app/escala/utils.py:1726`) |
| Contador sem chamador | Apagado, com lápide. (`app/ponto/services/cartorio.py:721`) |
| Leitor desalinha hora e instante | Filtra ambos pelo mesmo par posicional; lastro ausente/desalinhado permanece vazio. (`app/escala/services/leitor_celula.py:191`; `app/escala/services/leitor_celula.py:193`) |
| Adaptador perde órfãs sem marcos | Agora retorna as horas mesmo com `marcos=[]`. (`app/escala/utils.py:1105`; `app/escala/utils.py:1107`) |

**A guarda [nome]/ATA-X-TURNO decide pertença, não somente ordem:** `_hh_turno` reduz as batidas do turno a minutos; o filtro de `_orf_t` usa esses minutos para decidir se a entrada é a única órfã daquele turno; a célula posterior também é associada ao turno por sua hora. Somente `_ck['hora'] > _hhmm_t` é ordenação. (`app/escala/utils.py:2093`; `app/escala/utils.py:2097`; `app/escala/utils.py:2099`)

Contraexemplo no laço real em memória: turno não noturno `13:41 → 17:14:20`, marcos `11:00/16:00`, célula posterior ocupada por `17:14:20`; órfãs `13:41` e uma batida alheia ao turno às `17:14:10`. A entrada fica missing; deslocar somente a órfã alheia para `17:15:10` permite preencher a entrada. A decisão muda pela colisão de minuto; reprodução integral desse universo em produção **[nao verificado]**. (`app/escala/utils.py:2097`; `app/escala/utils.py:2101`; `app/escala/utils.py:2107`)

Nos outros três arquivos, não encontrei identidade de órfã **somente por HH:MM**: leitor e espelho transportam; o casamento central do PDF compara instantes. O filtro textual do PDF conserva data, mas perde segundos — tratado no item 6. (`app/escala/services/leitor_celula.py:193`; `app/ponto/services/espelho.py:860`; `app/relatorios/pdf_espelho.py:195`; `app/relatorios/pdf_espelho.py:698`)

**2. PROCEDE: o item 5 alcança um leitor que toma decisões e pode mudar número no PDF.** Há dois chamadores de produção: espelho e calendário. O calendário descarta o segundo retorno e já lê diretamente `dia['orfas']`; portanto, esse chamador não ganha órfãs pelo novo retorno. (`app/ponto/services/espelho.py:490`; `app/ponto/services/espelho.py:704`; `app/colaboradores/services/calendario.py:377`; `app/colaboradores/services/calendario.py:380`)

- **Tela do espelho:** publica as horas em `orfas_grade`, mas não passa automaticamente a desenhá-las em roxo quando `realizado_grade=[]`: os dois desenhos de órfãs estão dentro do ramo de realizado não vazio; o ramo alternativo mostra batidas cruas. (`app/ponto/services/espelho.py:859`; `app/templates/ponto/espelho.html:151`; `app/templates/ponto/espelho.html:231`; `app/templates/ponto/espelho.html:240`; `app/templates/ponto/espelho.html:243`)
- **PDF com lastro válido:** passa a reconhecer a batida como órfã, desenhá-la em cinza com asterisco e excluir os alertas do período que ela abriu. (`app/relatorios/pdf_espelho.py:494`; `app/relatorios/pdf_espelho.py:590`; `app/relatorios/pdf_espelho.py:858`; `app/relatorios/pdf_espelho.py:947`)
- **PDF sem lastro:** passa a declarar “Orfas: nao sei (ata sem instantes)”. (`app/relatorios/pdf_espelho.py:180`; `app/relatorios/pdf_espelho.py:587`; `app/relatorios/pdf_espelho.py:899`)
- **Número informativo:** uma entrada agora reconhecida como órfã deixa de contar em `turnos_abertos`; o dia também pode sair de `datas_turno_aberto`. A marca ainda alimenta a supressão de anomalias do cabeçalho. (`app/relatorios/pdf_espelho.py:537`; `app/relatorios/pdf_espelho.py:598`; `app/relatorios/pdf_espelho.py:702`)

O adaptador não grava ata. Não encontrei caminho do **segundo retorno alterado** até emissão de cobrança, chamado, pergunta ou cálculo financeiro: o calendário o descarta e o espelho o publica para seus consumidores. Os leitores diretos da ata continuam recebendo a lista anterior. (`app/escala/utils.py:1105`; `app/escala/utils.py:1132`; `app/colaboradores/services/calendario.py:377`; `app/ponto/services/espelho.py:859`; `app/chamados/juizes.py:510`)

Folga com batida, dia sem escala e isento só são alcançados por essa alteração quando chegam ao adaptador com órfãs e **marcos recebidos vazios**. Sem escala que satisfaça o gate do espelho, o adaptador nem é instalado; isenção não apaga as órfãs existentes. Quantidade de casos reais atingidos e impacto monetário **[nao verificado]**. (`app/escala/utils.py:1103`; `app/escala/utils.py:1107`; `app/ponto/services/espelho.py:488`; `app/ponto/services/espelho.py:707`; `app/escala/services/leitor_celula.py:214`)

**3. PROCEDE a cura do dry-run.** O conjunto recebe a serialização dos próprios instantes locais de `locs`, sem truncamento de segundos ou microssegundos. `fromisoformat` reconstrói o valor comparado; a identidade do objeto `tzinfo` não é requisito para essa comparação. (`app/escala/utils.py:1370`; `app/escala/utils.py:671`; `app/escala/utils.py:1702`; `app/escala/utils.py:1726`)

Não há saída normal de `_match_marcos` que devolva horas órfãs sem preencher o coletor fornecido: tanto a saída antecipada quanto a saída DP preenchem horas e instantes antes do retorno. No dry-run, `derivar=False`, sem `turno`, evita a derivação de papéis e segue pela DP; não esvazia o coletor por si. (`app/escala/utils.py:601`; `app/escala/utils.py:626`; `app/escala/utils.py:628`; `app/escala/utils.py:631`; `app/escala/utils.py:669`; `app/escala/utils.py:671`; `app/escala/utils.py:674`)

**4. NAO PROCEDE a igualdade universal de tamanho em todo escritor; o pareamento existe na cadeia com lastro completo.** (`app/escala/services/leitor_celula.py:191`; `app/ponto/services/espelho.py:503`)

| Dado | Quem escreve | Quem lê |
|---|---|---|
| `ata['orfas']` | Conteúdo por `ata_do_dia`; persistência pela porta. (`app/ponto/services/cartorio.py:739`; `app/ponto/portas/celula.py:995`; `app/ponto/portas/celula.py:1033`) | Leitor da célula, juiz de batida, cluster, vigia, esmeril, juiz de chamados, materialização, tripwire e helper do cartão. (`app/escala/services/leitor_celula.py:187`; `app/ponto/juiz_batida.py:161`; `app/ponto/services/cluster_espurio.py:31`; `app/ponto/services/vigia_pareador_celula.py:74`; `app/ponto/services/esmeril_espelho.py:125`; `app/chamados/juizes.py:510`; `app/chamados/services/materializacao.py:180`; `app/ponto/management/commands/tripwire_celulas.py:264`; `app/relatorios/cartao_pela_celula.py:288`) |
| `ata['orfas_instantes']` | Conteúdo por `ata_do_dia`; porta transporta a ata inteira. (`app/ponto/services/cartorio.py:742`; `app/ponto/portas/celula.py:995`) | `_orfas_da_ata`; o contador anterior foi retirado. (`app/escala/services/leitor_celula.py:190`; `app/ponto/services/cartorio.py:721`) |
| `dia['orfas_instantes']`, builder | `_match_marcos`, builders, promoção de células, conservação de sobras, overlays e reconciliação. (`app/escala/utils.py:628`; `app/escala/utils.py:671`; `app/escala/utils.py:799`; `app/escala/utils.py:1776`; `app/escala/utils.py:1788`; `app/escala/utils.py:1868`; `app/escala/utils.py:1890`; `app/escala/utils.py:2105`) | Sort pareado, reconciliação e cartório. (`app/escala/utils.py:815`; `app/escala/utils.py:1897`; `app/escala/utils.py:2068`; `app/ponto/services/cartorio.py:742`) |
| `dia['orfas_instantes']`, transportado | Leitor da ata; degradação do espelho esvazia deliberadamente. (`app/escala/services/leitor_celula.py:193`; `app/escala/services/leitor_celula.py:428`; `app/ponto/services/espelho.py:503`) | Espelho publica `orfas_instantes_grade`; PDF consome. (`app/ponto/services/espelho.py:860`; `app/relatorios/pdf_espelho.py:177`) |
| Retorno de `montar_realizado_grade` | O próprio adaptador copia as horas, conservando sua ordem. (`app/escala/utils.py:1105`; `app/escala/utils.py:1132`) | Espelho e calendário, conforme item 2. (`app/ponto/services/espelho.py:704`; `app/colaboradores/services/calendario.py:377`) |

Nos builders, append, promoção e remoção acompanham os dois membros; o sort é por HH:MM, conservando pares, e os overlays limpam ambos. (`app/escala/utils.py:810`; `app/escala/utils.py:815`; `app/escala/utils.py:977`; `app/escala/utils.py:1175`; `app/escala/utils.py:1254`; `app/escala/utils.py:1298`; `app/escala/utils.py:1304`; `app/escala/utils.py:1775`; `app/escala/utils.py:1787`; `app/escala/utils.py:2105`)

As exceções são explícitas: ata antiga/desalinhada e degradação podem ter horas com `instantes=[]`; há ata parcial somente em memória, sem instantes. Além disso, `ata_do_dia` copia as listas separadamente e não valida tamanho/conteúdo de uma entrada arbitrária. (`app/escala/services/leitor_celula.py:191`; `app/escala/services/leitor_celula.py:195`; `app/ponto/services/espelho.py:503`; `app/escala/utils.py:1948`; `app/escala/utils.py:1957`; `app/ponto/services/cartorio.py:739`; `app/ponto/services/cartorio.py:742`)

**5. NAO PROCEDE a suficiência dos testes para certificar a classe inteira.** Reproduzidos em memória: c18 **13**, c19 **9**, identidade datada **7** verdes; **14 mutantes mortos**, por falhas de asserção. São recortes do código, sem runner Django nem certificação dos bytes PDF. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:1`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:336`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:217`; `app/relatorios/tests/test_pdf_identidade_datada.py:6`)

O mutante `reconciliacao_minuto` morde o retorno ao HH:MM **no localizador `_ix_orfa`**. Entretanto, os testes fornecem marco igual ao instante alvo e `_abs_t=[alvo]`: a distância é zero, `_mais_perto=[]`, e a guarda residual nunca executa. Portanto, não certificam sua pertença/unicidade por instante. (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:25`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:53`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:59`; `app/escala/utils.py:2089`; `app/escala/utils.py:2092`)

Persistem identidade reduzida na reconciliação e decisão própria de supressão no PDF. Não identifiquei no delta novo escritor fora da porta, leitura nova de relógio ou aquisição nova de trava; concorrência em execução **[nao verificado]**. (`app/escala/utils.py:2097`; `app/relatorios/pdf_espelho.py:702`; `app/ponto/services/cartorio.py:742`; `app/escala/services/leitor_celula.py:193`; `app/escala/utils.py:1700`)

**6. NAO PROCEDE integralmente a triagem como quitação da classe.** Procedem os cinco encaminhamentos e a remoção do contador pela L-111; o item 1, contudo, ficou incompleto pela guarda descrita acima. A prova de frota, reversão, relavratura da competência 11 e DIFF monetário zero **[nao verificado]**. (`app/ponto/services/cartorio.py:721`; `CLAUDE.md:191`; `app/escala/utils.py:2097`)

Quanto a `_espurias_datadas`, procede que ele conserva dia/mês. Não procede tratá-lo como simples formatação: ele elimina anomalias por substring de `DD/MM HH:MM`; a própria origem da string já perdeu segundos. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `app/ponto/adaptador_motor.py:41`)

Contraexemplo do filtro real: órfã `10/11 03:00:10` e anomalia de outra batida `10/11 03:00:20` produzem a mesma chave textual, permitindo apagar a segunda anomalia. Ocorrência em produção **[nao verificado]**. O selo aceita expressamente identidades por data/minuto, portanto seu verde não resolve essa colisão. (`app/relatorios/pdf_espelho.py:702`; `app/ponto/adaptador_motor.py:42`; `app/relatorios/tests/test_pdf_identidade_datada.py:301`; `app/relatorios/tests/test_pdf_identidade_datada.py:302`)

**Procede remeter o descarte ao opus**, como determina a regra; isso não equivale a declarar o achado curado. O alcance adicional do aval `RESPOSTAS-1211-1230`, citado somente pela triagem fornecida, **[nao verificado]**. (`CLAUDE.md:375`; `CLAUDE.md:379`)

**Fecho: NAO LIBERA para suite + opus nesta volta: resta decisão de pertença de órfã por HH:MM na reconciliação, fora da cobertura dos REDs apresentados.** (`app/escala/utils.py:2097`; `app/escala/utils.py:2099`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:59`)

## 12/11 16:07 · sessao 76402e02c93d · FIO-E0-V4-E3-LEITORES · verificador · origem=Code

**1. NAO PROCEDE — os avais ainda não estão integralmente realizados.**

Permanecem pelo menos três respondedores nomeados no aval:

- Relatório: decide “aguarda colaborador/supervisão” pelo carimbo cru (`app/chamados/relatorios_disputa.py:21`).
- Cobrança: seleciona somente `respondida_em__isnull=True` (`app/chamados/services/cobranca.py:229`).
- Cartão: anuncia “Resposta enviada” e “Aguardando a supervisão” apenas pela presença do carimbo (`app/templates/chamados/partials/_card_atendimento.html:94`, `:99`, `:101`).

A catraca admite explicitamente pendências, fixa `TETO = 35` e verifica apenas que esse número não aumentou; portanto, não demonstra “respondedores vivos esperado 0”. Ela ainda exige encontrar cobrança e relatório no universo (`app/chamados/tests/test_e3_catraca_respondedores.py:7`, `:16`, `:39`, `:44`).

**Quanto ao b4:** não identifiquei pressuposto remanescente do filtro transplantado nos sítios examinados. O painel exclui somente silêncio futuro; a lista do colaborador continua exigindo silêncio nulo; o teste conserva silêncio vencido pertencendo à fila, com verbo `historico` (`app/chamados/services/painel.py:190`; `app/chamados/services/janela_colab.py:73`; `app/chamados/tests/test_e3_portas_tarefa.py:95`). O “#4” restante em `test_e3_bloqueantes` trata do relógio do C6, não do silêncio vencido (`app/chamados/tests/test_e3_bloqueantes.py:3`).

Igualdade literal com MAIN e repetição da sonda C2: **[nao verificado]**.

**2. PROCEDE — a recusa está nos dois escritores apontados; não existe guarda universal de persistência.**

Ambos consultam a mesma porta, que chama `respondida_sem_texto` sobre o par solicitado; o predicado significa carimbo não nulo e texto vazio após `strip()` (`app/chamados/services/fio.py:1578`; `app/chamados/juizes.py:1510`, `:1518`).

| Escritor/caminho | Tratamento |
|---|---|
| `_nascer` | Recusa antes da trava e dos ramos criar/reusar/costurar (`app/chamados/models.py:2861`, `:2864`, `:2866`). |
| `responder_automaticamente` | Recusa antes das atribuições e do `save`; texto previamente existente mantém o no-op (`app/chamados/services/disputa_emissao.py:1647`, `:1655`, `:1657`). |
| `responder_pergunta_disputa_core` | Não chama a porta nova; rejeita texto vazio antes de escrever o par (`app/chamados/services/disputa_emissao.py:1432`, `:1467`, `:1508`). |
| `carimbar_resposta_pela_trilha` | Não chama a porta nova; exige texto existente antes de carimbar (`app/chamados/services/disputa_emissao.py:1675`, `:1679`). |
| `PerguntaDisputa.carimbar` | Só acrescenta carimbo quando `texto_vazio` é falso (`app/chamados/models.py:3155`, `:3157`, `:3177`). |
| `reabrir_para_correcao` | Limpa carimbo e texto por `update` ou `descarimbar_em_lote` (`app/chamados/services/disputa_emissao.py:1593`, `:1630`, `:1632`). |
| `ressuscitar_perguntas` | Limpa carimbo pelo lote (`app/chamados/models.py:1567`, `:3289`). |
| `_desfazer_em_memoria` | Restaura o carimbo anterior somente na instância (`app/chamados/models.py:3184`, `:3188`). |

**Existe escape estrutural:** `PerguntaDisputa.save` não valida esse par, e `descarimbar_em_lote(**outros)` aceita campos adicionais sem essa guarda (`app/chamados/models.py:3438`, `:3445`, `:3277`, `:3291`). A fixture demonstra criação crua do estado ilegal por `objects.create` (`app/chamados/tests/test_e3_10_cura_no_escritor.py:39`).

Outro escritor produtivo que efetivamente gere carimbo sem texto por esses escapes, inclusive admin/comando/signal: **[nao verificado]**. Os outros escritores identificados acima têm guardas próprias; não seria correto apresentá-los como novas fontes comprovadas do estado ilegal (`app/chamados/services/disputa_emissao.py:1432`, `:1675`; `app/chamados/models.py:3155`).

**3. NAO PROCEDE — os leitores ainda não compartilham inteiramente o significado.**

| Campo/valor | Quem produz | Quem lê |
|---|---|---|
| Retorno de `tarefa_do_dia` | Corpo da tarefa e publicação de portas, em memória (`app/chamados/services/tarefa_do_dia.py:184`, `:525`, `:540`). | Folha, vigia, beco, pauta e instrumento de sombra (`app/folha/fora_da_folha.py:74`; `app/chamados/services/vigia_fio.py:638`; `app/escala/services/cadastro_realidade.py:497`; `app/ponto/services/pautas_dos_discordantes.py:80`; `app/chamados/management/commands/diff_e3_sombra.py:128`). |
| `respondida_em` / `resposta_colab` | Escritores censados na pergunta 2 (`app/chamados/services/disputa_emissao.py:1467`, `:1657`; `app/chamados/models.py:3157`). | Crédito admin, C7, aviso do fio, API, pendências, relatório, cobrança e template (`app/chamados/services/raiox.py:143`; `app/chamados/models.py:1849`; `app/chamados/services/fio.py:174`; `app/api/views.py:1175`; `app/chamados/services/pergunta_no_app.py:71`; `app/chamados/relatorios_disputa.py:21`; `app/chamados/services/cobranca.py:229`; `app/templates/chamados/partials/_card_atendimento.html:94`). |
| `respostas_sem_veredito` | C7 seleciona não validada, não materializada, com resposta e fora de `respondida_sem_texto`, respeitando tranca (`app/chamados/models.py:1847`). | `pode_fechar` e porta de fechamento (`app/chamados/models.py:1860`, `:1872`). |
| Motivo/razão `respondida_sem_texto` | A exceção usa o par solicitado; a tarefa usa o texto da última decisão RESPOSTA (`app/chamados/models.py:2325`; `app/chamados/services/fio.py:1585`; `app/chamados/services/tarefa_do_dia.py:135`). | C7, aviso do cartão e diagnóstico do vigia (`app/chamados/models.py:1851`; `app/chamados/services/fio.py:174`; `app/chamados/services/vigia_fio.py:679`). |
| Papel do vigia | Vem de `fio`, cuja precedência é colaborador → admin → sistema (`app/chamados/services/tarefa_do_dia.py:552`). | Vigia repassa `fio` e conta exigência de admin por igualdade com `'admin'` (`app/chamados/services/vigia_fio.py:650`). |
| `fio_vivo` | Redução `any(pergunta_viva)` no universo recebido (`app/chamados/juizes.py:1494`, `:1503`). | C7 e aviso do colaborador (`app/chamados/models.py:1860`; `app/chamados/services/fio.py:174`). |
| `pode_fechar` | C7 exige simultaneamente lista de respostas sem veredito vazia e ausência de fio vivo (`app/chamados/models.py:1860`). | Explicador, agora alimentado com a disputa inteira (`app/chamados/services/explicador.py:139`; `app/chamados/services/fio.py:333`). |

**Tempo:** o dia de entrada é o dia do turno; `data_civil` vem do instante local do marco. A janela é inclusiva, e decisões/passivo/efeitos aceitam igualdade com `agora` (`app/chamados/services/tarefa_do_dia.py:105`, `:184`, `:519`, `:393`, `:401`, `:412`). O retorno não representa reconstrução histórica integral: identidade, cadastro, fila/status e tranca usam estado atual, conforme contrato declarado (`app/chamados/services/tarefa_do_dia.py:352`).

**Divergência concreta:** carimbo com texto vazio vira `cobrar` no crédito admin, mas continua fora das pendências do app, fora dos calados e classificado como aguardando supervisão no relatório (`app/chamados/services/raiox.py:144`; `app/chamados/services/pergunta_no_app.py:71`; `app/chamados/services/cobranca.py:229`; `app/chamados/relatorios_disputa.py:21`).

**4. PROCEDE — há classes de reprovação presentes.**

- **Derivação paralela remanescente:** o relatório declara `PRONTA_FECHAR` quando não há perguntas vivas, sem consultar a outra metade do C7, respostas sem veredito (`app/chamados/relatorios_disputa.py:18`; `app/chamados/models.py:1860`). O próprio teste novo do explicador demonstra cenário com C6 morto e C7 recusando fechamento (`app/chamados/tests/test_explicador_disputa.py:150`).
- **Relógio fora da borda:** `censo_alcance` exige `agora`, mas, omitido `hoje`, lê `timezone.localdate()`. Competência e teto usam esse dia, enquanto a tarefa usa `agora`; uma consulta histórica pode misturar relógios (`app/chamados/services/vigia_fio.py:542`, `:567`, `:584`, `:637`, `:653`). `furo_sem_canal.medir` também mantém fallback interno a `timezone.now()` (`app/chamados/services/furo_sem_canal.py:43`, `:56`).
- **Selo insuficiente para o resultado exigido:** teto 35 não prova morte dos cinco respondedores, e o detector exclui tuplas e templates (`app/chamados/tests/test_e3_catraca_respondedores.py:7`, `:10`, `:16`, `:39`).

No trajeto corrigido do explicador, ele agora consulta o C7 registrado; a guarda nova do nascimento executa antes da trava (`app/chamados/services/explicador.py:139`; `app/chamados/models.py:2864`). Inversão de trava ou pouso antes do pré-requisito: **[nao verificado]**.

**5. PROCEDE — existe mutante plausível sem RED correspondente.**

Mutante: executar a guarda de `_nascer` somente sob `if conversa:`. Isso deixaria `abrir` factual sem recusa, preservando o caminho exercitado pelos novos testes, que chamam exclusivamente `abrir_conversa` (`app/chamados/models.py:2845`, `:2864`; `app/chamados/tests/test_e3_10_cura_no_escritor.py:85`, `:98`, `:105`).

Deveria morder um RED por `PerguntaDisputa.abrir`, com dia válido, carimbo e texto vazio, incluindo nascimento e reuso/costura, exigindo motivo `respondida_sem_texto` e nenhuma alteração — o contrato promete recusa em todos esses ramos (`app/chamados/models.py:2861`). Sobrevivência executada contra todos os testes do diff: **[nao verificado]**.

**6. NAO SEI — não é possível provar invariância individual do passivo com o raspado fornecido.**

Há mudanças visíveis: API e web alteram a possibilidade de solicitar correção; a fila admin reclassifica carimbo sem texto para cobrança do colaborador (`app/api/views.py:1175`; `app/chamados/services/fio.py:361`; `app/chamados/services/raiox.py:143`). Os testes exigem expressamente essas mudanças (`app/chamados/tests/test_e3_leitores_fio_disputa.py:156`; `app/chamados/tests/test_creditos_perguntas_invariantes.py:94`).

Esses caminhos estão identificados no código como item 12 e cura do crédito; sua existência não demonstra um apply de limpeza do passivo (`app/chamados/services/fio.py:164`; `app/chamados/services/raiox.py:139`). A lista de pendências continua usando o carimbo cru, e o badge conta essa lista (`app/chamados/services/pergunta_no_app.py:71`, `:91`).

Provar que nenhuma vista dos 142/16 muda, ou reconciliar esse recorte com os 143/17 citados no aval: **[nao verificado]**.

**Fecho: NAO LIBERA para suíte + opus.** O bloqueio verificável é a entrega incompleta dos respondedores e do selo exigido, além do relógio ainda independente no vigia (`app/chamados/relatorios_disputa.py:21`; `app/chamados/services/cobranca.py:229`; `app/templates/chamados/partials/_card_atendimento.html:94`; `app/chamados/tests/test_e3_catraca_respondedores.py:39`; `app/chamados/services/vigia_fio.py:567`).

## 12/11 16:07 · sessao 95b777412492 · - · - · origem=Ronald (tmux)

1. **NAO PROCEDE a cura integral como classe.** O localizador e a remoção da órfã passaram a usar o instante completo e a mesma posição nas duas listas (`app/escala/utils.py:2068`; `app/escala/utils.py:2069`; `app/escala/utils.py:2105`; `app/escala/utils.py:2106`). Mas a guarda [nome]/ATA-X-TURNO ainda:
   - constrói um conjunto dos minutos das batidas do turno (`app/escala/utils.py:2093`);
   - identifica quais órfãs pertencem ao turno por pertença nesse conjunto, decidindo se existe uma única órfã do turno (`app/escala/utils.py:2097`);
   - identifica se a célula real pertence ao turno por minuto (`app/escala/utils.py:2099`).

   Portanto, não é apenas ordem: somente `_ck['hora'] > _hhmm_t` é comparação de ordem. As duas pertenças anteriores decidem identidade/associação (`app/escala/utils.py:2097`; `app/escala/utils.py:2099`).

2. **Contraexemplo executado no laço real, sem arquivo:** turno não noturno com entrada `13:41` e saída `17:14:20`; marcos `11:00`/`16:00`; célula posterior com instante `17:14:20`; órfã alvo `13:41` e outra órfã, fora de `_t.batidas`, às `17:14:10`. A entrada permanece missing; mover exclusivamente a órfã alheia para `17:15:10` faz a mesma entrada ocupar o marco. Isso decorre do filtro por minuto citado; reprodução integral do caso pelo builder/banco **[nao verificado]** (`app/escala/utils.py:2093`; `app/escala/utils.py:2097`; `app/escala/utils.py:2101`; `app/escala/utils.py:2107`).

3. **PROCEDE a cura do dry-run.** `locs` recebe `timezone.localtime`; o coletor serializa os próprios objetos de `locs` com `isoformat`; o set os reconstrói por `fromisoformat` e compara `datetime`, preservando segundos/microssegundos (`app/escala/utils.py:1370`; `app/escala/utils.py:671`; `app/escala/utils.py:1702`; `app/escala/utils.py:1726`). Executei o `_match_marcos` real com `03:00:20.654321-03:00`: o coletor preservou `.654321`, e o round-trip comparou igual **[nao verificado: ensaio em memória sem log persistido]**.

   Não há saída normal retornando horas sem preencher o coletor fornecido: a saída antecipada escreve ambos antes de retornar; a saída DP também (`app/escala/utils.py:626`; `app/escala/utils.py:628`; `app/escala/utils.py:631`; `app/escala/utils.py:669`; `app/escala/utils.py:671`; `app/escala/utils.py:674`). `derivar=False` apenas evita a derivação de papéis, seguindo para a DP (`app/escala/utils.py:601`; `app/escala/utils.py:632`).

4. **Os REDs não mordem a guarda residual.** O helper c19 fornece marco alvo igual ao instante da batida e `_abs_t=[alvo]`; assim `_d_alvo=0` e `_mais_perto=[]`, sem entrar na guarda (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:53`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:59`; `app/escala/utils.py:2089`; `app/escala/utils.py:2092`). O mutante oficial `reconciliacao_minuto` troca apenas `_ix_orfa`, cobrindo o localizador corrigido, não a guarda (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:25`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:28`).

   Rodei os **29 testes c18+c19+identidade_datada** com dois mutantes adicionais em memória: desligar integralmente a guarda de `2092`; ou substituir seu corpo por `_mais_perto=[]`, liberando toda reconciliação nessa situação. **Ambos sobreviveram: 29 testes verdes em cada execução** **[nao verificado: saída observada em memória, sem log persistido]**. O sítio de produção afetado é `app/escala/utils.py:2092`.

5. No PDF, o casamento central usa instantes completos e recusa ISO sem offset; o filtro `_espurias_datadas` continua reduzindo para `DD/MM HH:MM` e suprimindo anomalia por substring (`app/relatorios/pdf_espelho.py:183`; `app/relatorios/pdf_espelho.py:186`; `app/relatorios/pdf_espelho.py:195`; `app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`). Ele tem data, mas permanece decisão com identidade reduzida; o descarte por escopo depende do aval literal fornecido pelo usuário **[nao verificado além dos anexos]**.

Há também comentário desatualizado afirmando que o casamento da reconciliação é por HH:MM, apesar do localizador novo (`app/escala/utils.py:1996`).

## 12/11 16:07 · sessao 6b8954ecf85b · - · - · origem=Ronald (tmux)

**Q5 — NAO PROCEDE a cura integral da classe.** Os testes mordem a regressão do lookup por minuto, mas não exercitam a guarda [nome]/ATA-X-TURNO: o helper entrega marco e batida no mesmo instante, com `_abs_t=[alvo]`; assim, `_mais_perto=[]` e o ramo residual por HH:MM não executa. (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:53`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:59`; `app/escala/utils.py:2089`; `app/escala/utils.py:2092`)

**Ensaios reproduzidos:** c18 13 verdes, c19 9 verdes, identidade datada 7 verdes: 29 testes, todos rc 0; 9 mutantes c18 e 5 c19 mortos com falhas de asserção, nenhum dependendo de erro de infraestrutura. São recortes em memória, sem inicialização Django ou renderização dos bytes PDF. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:1`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:336`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:217`; `app/relatorios/tests/test_pdf_identidade_datada.py:1`)

O mutante `reconciliacao_minuto` reverte somente `_ix_orfa`; falhou em três subtests: instante ausente e mesmo minuto com offsets BRT/UTC. O mutante `dryrun_minuto` falhou nos dois testes, incluindo aquele que chama `_match_marcos` real. (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:25`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:64`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:75`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:121`)

**Q6 — NAO PROCEDE chamar o descarte de cura ou ausência de decisão.** `_espurias_datadas` conserva dia/mês, mas decide supressão de anomalias por substring de minuto. O formato da anomalia também perdeu segundos na origem. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `app/ponto/adaptador_motor.py:39`)

Executei o filtro real em memória com órfã `10/11 03:00:10`, batida não órfã `10/11 03:00:20` e anomalia `Entrada sem saída em 10/11 03:00.`: a anomalia foi removida. Esse é contraexemplo da colisão textual; ocorrência desse universo pelo pareador ou em produção **[nao verificado]**. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `app/ponto/adaptador_motor.py:41`)

O selo atual aceita explicitamente `(data, HH:MM)` e `DD/MM HH:MM`; o RED S2 cobre datas/mês diferentes, não segundos diferentes no mesmo minuto. (`app/relatorios/tests/test_pdf_identidade_datada.py:247`; `app/relatorios/tests/test_pdf_identidade_datada.py:301`)

**Procede encaminhar o descarte ao opus**, pois essa é a regra expressa; não procede tratá-lo como certificação de que o leitor não decide identidade reduzida. O escopo do verificador inclui leitores dos valores tocados, mesmo fora do diff. (`CLAUDE.md:375`; `CLAUDE.md:379`)

A remoção do contador com lapide procede pela L-111; a sonda de frota/reversão/DIFF monetário zero continua **[nao verificado]**. (`app/ponto/services/cartorio.py:721`; `app/docs/LEIS.md:137`)

## 12/11 16:06 · sessao 4d31fddac01f · - · - · origem=Ronald (tmux)

Q2 — **PROCEDE, com efeito adicional em número informativo do PDF.**

- Há dois chamadores de produção: `montar_dias`, com alias `montar_realizado`, em `app/ponto/services/espelho.py:490`, `app/ponto/services/espelho.py:704`; e calendário, com alias `_montar_real_grade`, em `app/colaboradores/services/calendario.py:333`, `app/colaboradores/services/calendario.py:377`.
- No calendário, o segundo retorno é descartado; as órfãs já eram lidas diretamente do dia. Portanto esse chamador não ganha nova informação por causa do item 5. (`app/colaboradores/services/calendario.py:377`; `app/colaboradores/services/calendario.py:380`)
- No espelho, sem marcos continua `real=[]`, mas agora `_orfas_grade` conserva as horas. (`app/escala/utils.py:1105`; `app/escala/utils.py:1107`; `app/ponto/services/espelho.py:704`; `app/ponto/services/espelho.py:859`)
- **A TELA não passa automaticamente a desenhar órfãs roxas nesse caso:** tanto o cartão quanto o minicalendário só percorrem `orfas_grade` dentro do ramo `realizado_grade` verdadeiro; com `real=[]`, mostram as batidas cruas pelo ramo alternativo. (`app/templates/ponto/espelho.html:151`; `app/templates/ponto/espelho.html:231`; `app/templates/ponto/espelho.html:240`; `app/templates/ponto/espelho.html:243`)
- **O PDF passa a ver e decidir com essa órfã.** Com lastro válido, `_orfas_publicadas` devolve os timestamps, o coletor passa a marcar `b.orfa`, e a batida vira `*HH:MM` cinza. Sem lastro, passa a declarar “Orfas: nao sei (ata sem instantes)”. (`app/relatorios/pdf_espelho.py:174`; `app/relatorios/pdf_espelho.py:494`; `app/relatorios/pdf_espelho.py:587`; `app/relatorios/pdf_espelho.py:590`; `app/relatorios/pdf_espelho.py:858`; `app/relatorios/pdf_espelho.py:867`; `app/relatorios/pdf_espelho.py:899`)
- **Não se pode afirmar “nenhum número muda”:** se a entrada de período aberto agora for reconhecida como órfã, deixa de integrar `resumo['turnos_abertos']`; também deixa de integrar a lista `datas_turno_aberto`. (`app/relatorios/pdf_espelho.py:191`; `app/relatorios/pdf_espelho.py:535`; `app/relatorios/pdf_espelho.py:537`; `app/relatorios/pdf_espelho.py:598`)
- A marca também entra no filtro de anomalias do cabeçalho. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`)
- A função só copia e retorna listas: não escreve a ata. Os consumidores financeiros do espelho leem realizado/rubricas por outras fontes; o segundo retorno é somente publicado em `orfas_grade`. (`app/escala/utils.py:1097`; `app/escala/utils.py:1132`; `app/ponto/services/espelho.py:798`; `app/ponto/services/espelho.py:826`; `app/ponto/services/espelho.py:859`)
- Não encontrei caminho do segundo retorno até emissão de cobrança/chamado/pergunta: o calendário descarta-o, e o espelho só o publica para desenho/PDF. Já os consumidores diretos de `dia['orfas']`/`ata['orfas']` continuam vendo a lista que já viam. (`app/colaboradores/services/calendario.py:377`; `app/ponto/services/espelho.py:704`; `app/ponto/services/espelho.py:859`)
- Folga/dia sem escala/isento só mudam por esse item se chegarem ao adaptador com órfãs e marcos vazios; a função não distingue esses tipos. Há gate de vínculo no espelho: se não satisfizer a condição de escala, o adaptador nem é instalado e a saída continua vazia. Isenção não limpa órfãs. (`app/escala/utils.py:1103`; `app/escala/utils.py:1107`; `app/ponto/services/espelho.py:488`; `app/ponto/services/espelho.py:707`; `app/escala/services/leitor_celula.py:214`; `app/escala/services/leitor_celula.py:223`)

Q4 — **PROCEDE para a cadeia normal; NÃO PROCEDE a afirmação universal de tamanho igual em todo escritor.**

| Dado | Escritores | Leitores diretos |
|---|---|---|
| `ata['orfas']` persistida | Produtor `ata_do_dia`; porta copia `_nova` e salva. (`app/ponto/services/cartorio.py:739`; `app/ponto/portas/celula.py:995`; `app/ponto/portas/celula.py:1006`; `app/ponto/portas/celula.py:1033`) | Leitor da célula (`app/escala/services/leitor_celula.py:187`); esmeril (`app/ponto/services/esmeril_espelho.py:125`); vigia pareador (`app/ponto/services/vigia_pareador_celula.py:74`); cluster (`app/ponto/services/cluster_espurio.py:29`); juiz batida (`app/ponto/juiz_batida.py:161`); juiz chamados (`app/chamados/juizes.py:510`); materialização (`app/chamados/services/materializacao.py:180`); tripwire (`app/ponto/management/commands/tripwire_celulas.py:264`); helper `orfas_e_isencao_da_ata` (`app/relatorios/cartao_pela_celula.py:288`). |
| `ata['orfas_instantes']` | Produtor `ata_do_dia`, cópia independente da lista recebida; porta transporta a ata inteira. (`app/ponto/services/cartorio.py:742`; `app/ponto/portas/celula.py:995`) | `_orfas_da_ata`. O contador foi apagado. (`app/escala/services/leitor_celula.py:190`; `app/ponto/services/cartorio.py:721`) |
| `dia['orfas_instantes']` builder | Coletor `_match_marcos` em ambos retornos; builders; conservação de sobras; promoção de célula para órfã; resets; reconciliação. (`app/escala/utils.py:628`; `app/escala/utils.py:671`; `app/escala/utils.py:799`; `app/escala/utils.py:811`; `app/escala/utils.py:1776`; `app/escala/utils.py:1788`; `app/escala/utils.py:1868`; `app/escala/utils.py:1890`; `app/escala/utils.py:1893`; `app/escala/utils.py:2105`) | Sort pareado; reconciliação; cartório. (`app/escala/utils.py:815`; `app/escala/utils.py:1897`; `app/escala/utils.py:2068`; `app/ponto/services/cartorio.py:742`) |
| `dia['orfas_instantes']` lido | Base vazia; leitor preenche junto com o filtro das horas. Degradação do espelho esvazia deliberadamente. (`app/escala/services/leitor_celula.py:395`; `app/escala/services/leitor_celula.py:193`; `app/escala/services/leitor_celula.py:428`; `app/ponto/services/espelho.py:503`) | Publicação como `orfas_instantes_grade`, depois `_orfas_publicadas`. (`app/ponto/services/espelho.py:860`; `app/relatorios/pdf_espelho.py:177`) |
| Retorno `montar_realizado_grade` | Somente o adaptador: cópia das horas na mesma ordem, `real=[]` quando não há marcos. (`app/escala/utils.py:1105`; `app/escala/utils.py:1107`; `app/escala/utils.py:1132`) | Os dois chamadores descritos acima. (`app/ponto/services/espelho.py:704`; `app/colaboradores/services/calendario.py:377`) |

Os escritores normais acrescentam e removem os dois membros juntos; o sort é **por HH:MM preservando pares**, não por instante absoluto. Os overlays limpam ambos; o legacy conserva o dicionário por `**sd`. (`app/escala/utils.py:810`; `app/escala/utils.py:815`; `app/escala/utils.py:977`; `app/escala/utils.py:1062`; `app/escala/utils.py:1175`; `app/escala/utils.py:1254`; `app/escala/utils.py:1298`; `app/escala/utils.py:1304`; `app/escala/utils.py:1775`; `app/escala/utils.py:1787`; `app/escala/utils.py:1897`; `app/escala/utils.py:2105`)

As exceções são explícitas: ata antiga/desalinhada produz horas e `instantes=[]`; degradação do builder conserva horas e apaga instantes; existe ata parcial **em memória** com apenas lâmpadas e órfãs, sem instantes. O cartório copia as duas listas sem validar comprimento ou conteúdo, portanto não impõe pareamento a uma entrada arbitrária. (`app/escala/services/leitor_celula.py:191`; `app/escala/services/leitor_celula.py:195`; `app/ponto/services/espelho.py:503`; `app/escala/utils.py:1948`; `app/escala/utils.py:1957`; `app/ponto/services/cartorio.py:739`; `app/ponto/services/cartorio.py:742`)

`carimbar_precedencia` pode persistir outra chave na ata, preservando as listas existentes; não é produtor do conteúdo novo. (`app/ponto/portas/celula.py:1083`; `app/ponto/portas/celula.py:1087`)

## 12/11 16:06 · sessao 9404cc491001 · - · - · origem=Ronald (tmux)

Achados para perguntas 3–5:

- **Relógio fora da borda presente:** `censo_alcance(..., *, agora)` ainda executa `hoje = hoje or timezone.localdate()`; `hoje` escolhe competências e teto civil enquanto a tarefa recebe `agora`. Omitir `hoje` com `agora` histórico mistura dois relógios (`app/chamados/services/vigia_fio.py:542`, `:567`, `:584`, `:637`, `:640`, `:653`). `furo_sem_canal.medir` também mantém fallback interno a `timezone.now()` (`app/chamados/services/furo_sem_canal.py:43`, `:56`).

- **Mutante plausível sobrevivente ao diff:** restringir a guarda nova de `_nascer` a `if conversa:` deixa `abrir` datada sem proteção; a guarda real atende ambos hoje, mas os testes novos de nascimento chamam exclusivamente `abrir_conversa` (`app/chamados/models.py:2796`, `:2809`, `:2845`, `:2864`; `app/chamados/tests/test_e3_10_cura_no_escritor.py:85`, `:98`, `:105`). Sobrevivência executada: **[nao verificado]**. Deve morder um RED chamando `PerguntaDisputa.abrir` com dia válido, `respondida_em=INSTANTE`, texto vazio, incluindo reuso/costura, exigindo motivo correto e nenhuma alteração (`app/chamados/models.py:2861`).

- **Catraca insuficiente para o aval:** teto é 35, teste exige somente `<=35`, e o próprio selo exige encontrar respondedores antigos; não demonstra “respondedores vivos esperado 0” (`app/chamados/tests/test_e3_catraca_respondedores.py:6`, `:16`, `:37`, `:44`). O detector não cobre tuplas nem templates, declarado nele (`app/chamados/tests/test_e3_catraca_respondedores.py:10`).

- **C7 corrigido sem juiz duplicado nesse trajeto:** `pode_fechar` compõe ausência de respostas sem veredito e ausência de fio vivo; explicador agora lê C7; fio fornece universo da disputa inteira, voltando à consulta integral para disputa fechada (`app/chamados/models.py:1847`, `:1854`; `app/chamados/services/explicador.py:139`; `app/chamados/services/fio.py:333`). `fio_vivo` significa somente existência de pergunta viva no universo fornecido pelo chamador, portanto não equivale a `pode_fechar` (`app/chamados/juizes.py:1494`).

Censo semântico:

| Valor | Escritor/autoridade | Leitores relevantes |
|---|---|---|
| Retorno `tarefa_do_dia` | Dict inicial e ações; projeção de portas dentro do mesmo juiz (`app/chamados/services/tarefa_do_dia.py:184`, `:243`, `:540`) | Porta é mera projeção; vigia, folha, beco e pauta leem-na (`app/chamados/services/tarefa_do_dia.py:559`; `app/chamados/services/vigia_fio.py:637`; `app/folha/fora_da_folha.py:67`; `app/escala/services/cadastro_realidade.py:499`; `app/ponto/services/pautas_dos_discordantes.py:80`) |
| Dia do turno / dia civil | Entrada `dia_do_turno`; civil do marco vem do teto convertido ao fuso local (`app/chamados/services/tarefa_do_dia.py:94`, `:105`, `:184`) | Janela da carga é inclusiva (`app/chamados/services/tarefa_do_dia.py:519`) |
| Instante | Decisões, passivo e efeitos aceitam igualdade `<= agora`; identidade, cadastro, fila/status e tranca continuam atuais por contrato explícito (`app/chamados/services/tarefa_do_dia.py:352`, `:393`, `:401`, `:412`) | Não se pode interpretar esse retorno como reconstrução histórica integral (`app/chamados/services/tarefa_do_dia.py:357`) |
| `fio` / papel do vigia | Precedência colab → admin → sistema no juiz (`app/chamados/services/tarefa_do_dia.py:552`) | Vigia repassa fio e conta admin por igualdade, sem traduzir verbo (`app/chamados/services/vigia_fio.py:645`) |
| `respondida_sem_texto` | Carimbo não nulo + `texto_vazio` (`app/chamados/juizes.py:1510`, `:1515`) | Guarda, C7 e aviso do cartão (`app/chamados/services/fio.py:1578`; `app/chamados/models.py:1851`; `app/chamados/services/fio.py:174`) |
| Razão `'respondida_sem_texto'` da tarefa | Última decisão RESPOSTA com `ato_texto` vazio (`app/chamados/services/tarefa_do_dia.py:135`) | É lida no retorno da tarefa; difere em insumo do predicado sobre projeção atual por desenho de sequência/instante (`app/chamados/services/tarefa_do_dia.py:118`, `:393`) |

O campo `data_evento` ainda possui dois significados declarados, separados por `conversa`: turno em `abrir`, civil em `abrir_conversa`; C19 exclui conversas da carga (`app/chamados/models.py:2803`, `:2814`; `app/chamados/services/tarefa_do_dia.py:375`). Não é mudança introduzida nos hunks do diff fornecido **[nao verificado por Git: cópia sem .git]**.

## 12/11 16:05 · sessao d2f5aa522f8b · - · - · origem=Ronald (tmux)

1. **NAO PROCEDE** para cumprimento integral: três respondedores nomeados seguem ativos — relatório decide pelo carimbo em `app/chamados/relatorios_disputa.py:21`, cobrança filtra carimbo nulo em `app/chamados/services/cobranca.py:229`, e template declara resposta enviada pelo carimbo em `app/templates/chamados/partials/_card_atendimento.html:94`. A catraca admite pendências, permite teto 35 e verifica apenas `<= TETO`: `app/chamados/tests/test_e3_catraca_respondedores.py:7`, `:16`, `:39`.

   **Não identifiquei pressuposto b4 nos sítios examinados.** O painel conserva exclusão somente de silêncio futuro (`app/chamados/services/painel.py:190`), seu contador idem (`:216`), e a lista colaborador continua exigindo silêncio nulo (`app/chamados/services/janela_colab.py:73`). O teste de silêncio vencido conserva a diferença antiga entre pertencimento à fila e verbo histórico (`app/chamados/tests/test_e3_portas_tarefa.py:95`, `:98`). Comparação literal com MAIN: **[nao verificado]**, pois a cópia não contém repositório Git.

6. **NAO SEI** quanto à invariância concreta dos 142/16: o raspado não fornece as vistas individuais **[nao verificado]**. O código muda a flag de correção da API (`app/api/views.py:1175`) e as flags web (`app/chamados/services/fio.py:361`); o teste exige expressamente flag falsa para pergunta carimbada sem texto (`app/chamados/tests/test_e3_leitores_fio_disputa.py:156`). A fila admin passa a atribuir esse estado ao colaborador (`app/chamados/services/raiox.py:143`, `:144`), com teste que exige `cobrar` (`app/chamados/tests/test_creditos_perguntas_invariantes.py:98`).

   Esses caminhos correspondem aos itens 12/#8 dos avais anexos **[nao verificado — anexo sem numeração]**; não demonstram aplicação sobre o passivo. A lista de pendências permanece separada pelo carimbo cru e continua classificando carimbada sem texto como histórico (`app/chamados/services/pergunta_no_app.py:71`, `:74`); seu badge conta essa lista (`:91`). Logo, a migração de leitores permanece incompleta (`app/chamados/tests/test_e3_catraca_respondedores.py:8`).

**Recomendação: NAO LIBERA**, pelo requisito verificavelmente aberto de eliminar os respondedores nomeados (`app/chamados/relatorios_disputa.py:21`; `app/chamados/services/cobranca.py:229`; `app/templates/chamados/partials/_card_atendimento.html:94`).

## 12/11 16:05 · sessao 57dfd31229d6 · - · - · origem=Ronald (tmux)

**Q2 — PROCEDE nos dois escritores curados; não há guarda universal no modelo.**

| Escritor | Tratamento do par |
|---|---|
| `_nascer` | Recusa antes da trava e antes dos ramos criar/reusar/costurar: `app/chamados/models.py:2863-2865`; inserção em `:3039-3042`. |
| `responder_automaticamente` | Recusa antes da atribuição e do save: `app/chamados/services/disputa_emissao.py:1651-1660`. Texto previamente existente produz no-op antes da guarda: `:1647-1648`, coberto em `app/chamados/tests/test_e3_10_cura_no_escritor.py:73-78`. |
| `responder_pergunta_disputa_core` | Escritor fora dessa porta, com guarda própria de texto não vazio: `app/chamados/services/disputa_emissao.py:1432-1434`; escreve ambos em `:1467-1468`, salva em `:1508`. |
| `carimbar_resposta_pela_trilha` | Escritor fora dessa porta, exige texto existente: `app/chamados/services/disputa_emissao.py:1675`; carimba/salva em `:1679-1681`. Comando consumidor: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67-68`. |
| `PerguntaDisputa.carimbar` | Escritor fora dessa porta; só põe `respondida_em` com `texto_vazio` falso: `app/chamados/models.py:3155-3158`; save em `:3177`. |
| `reabrir_para_correcao` | Limpa ambos: `app/chamados/services/disputa_emissao.py:1593`, `:1630-1632`. |
| `ressuscitar_perguntas` | Limpa `respondida_em` via lote: `app/chamados/models.py:1567`; update efetivo em `:3289-3291`. |
| `_desfazer_em_memoria` | Restauração em memória do carimbo anterior, sem save próprio: `app/chamados/models.py:3184-3191`. |
| `_universo_da_ressuscitacao` | Sondador em cópia, sem persistir: `app/chamados/models.py:1495-1509`. |

A porta compartilha o predicado: `app/chamados/services/fio.py:1578-1588` → `app/chamados/juizes.py:1515-1518`. O único consumidor produtivo encontrado do automático fornece texto constante não vazio: `app/api/foto_recebida.py:25`, `:48`.

**Escape estrutural:** `PerguntaDisputa.save` só deriva identidade e abre atomic; não valida HX-PAR-RESPOSTA (`app/chamados/models.py:3438-3448`). `descarimbar_em_lote(**outros)` também aceita campos adicionais sem validar esse par (`app/chamados/models.py:3277-3291`). São capacidades de contornar a guarda; escritor produtivo concreto que as use para gerar novo carimbo sem texto, além dos dois curados, **[nao verificado]**. A própria fixture cria estado ilegal por `objects.create`, explicitamente fora das portas (`app/chamados/tests/test_e3_10_cura_no_escritor.py:39-42`).

O bulk produtivo localizado altera apenas lavra: `app/chamados/veredito_lavrado.py:237`, `:250`. Os receivers da pergunta fecham beco ou emitem evento, sem atribuir o par nesses corpos (`app/chamados/signals.py:229-259`).

**Q5 — mutante plausível sem mordida no teste novo:** condicionar a guarda de `_nascer` a `if conversa:`. O caminho factual `abrir` ficaria sem recusa, embora ambos os caminhos hoje convirjam para `_nascer` (`app/chamados/models.py:2809`, `:2821`, `:2840`). Os testes novos do nascimento só exercitam `abrir_conversa` (`app/chamados/tests/test_e3_10_cura_no_escritor.py:85-110`). O teste expand extrai apenas os nós de save/registrar, sem executar a guarda (`app/chamados/tests/test_e4_expand.py:260-279`).

Deveria morder um RED chamando **`PerguntaDisputa.abrir` factual**, com carimbo e texto vazio, inclusive reuso de fato existente, afirmando recusa e nenhuma alteração. Sobrevivência executada contra todos os testes do diff **[nao verificado]**. [nao verificado]

## 12/11 15:58 · sessao 89593e35bd52 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura entregue para certificação: **29 testes locais verdes, 14 mutantes mortos por asserção e Ruff limpo nos cinco Python tocados**. HEAD continua na base, sem commit. ([testes](docs/lastro_c19_execucao.txt:1040), [mutantes](docs/lastro_c19_execucao.txt:1086), [Ruff](docs/lastro_c19_execucao.txt:1045), [HEAD](docs/lastro_c19_execucao.txt:1053))

| Item | RED que mordeu antes da cura |
|---|---|
| 1 | `test_reconciliacao_mesmo_minuto_remove_so_instante_alvo`: célula preenchida e órfã remanescente conferidas. [RED](docs/lastro_c19_execucao.txt:96) |
| 2 | `test_dryrun_nao_orfa_no_mesmo_minuto_nao_vira_intervalo`. Também há cobertura com matcher real. [RED](docs/lastro_c19_execucao.txt:159), [matcher real](ponto/tests/test_lastro_c19_orfas_instantes.py:121) |
| 3 | `test_contador_sem_chamador_foi_apagado`: contador removido com lápide de uma linha. [RED](docs/lastro_c19_execucao.txt:193), [lápide](ponto/services/cartorio.py:721) |
| 4 | `test_leitor_filtra_par_hora_instante_junto`: filtra o par; ata antiga conserva instantes vazios. [RED](docs/lastro_c19_execucao.txt:213), [cura](escala/services/leitor_celula.py:187) |
| 5 | **Procede.** `test_sem_marcos_publica_orfas_da_ata_e_pdf_declara_ignorancia`: adaptador conserva horas sem marcos; PDF mantém “não sei” para ata antiga. [RED](docs/lastro_c19_execucao.txt:268), [cura](escala/utils.py:1105) |

Comandos para repetir os testes e os cinco mutantes C19: [nao verificado]

```bash
c19() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B \
    -m ponto.tests.test_lastro_c19_orfas_instantes "$@" -v
}

c19 ReconciliacaoC19Test
c19 DryRunC19Test
c19 ContadorC19Test
c19 LeitorC19Test.test_leitor_filtra_par_hora_instante_junto
c19 LeitorC19Test.test_sem_marcos_publica_orfas_da_ata_e_pdf_declara_ignorancia

c19 --mutante reconciliacao_minuto ReconciliacaoC19Test
c19 --mutante dryrun_minuto DryRunC19Test
c19 --mutante contador_volta ContadorC19Test
c19 --mutante leitor_desalinhado LeitorC19Test
c19 --mutante sem_marcos_descarta LeitorC19Test
```

Os comandos executados antes de cada cura, os nove mutantes C18 restantes e o comando exato do Ruff estão no [registro de entrega](docs/lastro_c19_execucao.txt:1065).

Arquivos::funções alterados:

- `escala/utils.py::montar_grade_prevista_periodo_por_turno` — comparação por instante no [dry-run](escala/utils.py:1699) e na [reconciliação](escala/utils.py:2067).
- `escala/utils.py::montar_realizado_grade` — [órfãs independem dos marcos](escala/utils.py:1097).
- `escala/services/leitor_celula.py::_orfas_da_ata` e `::grade_da_celula` — [filtragem pareada](escala/services/leitor_celula.py:180), [transporte](escala/services/leitor_celula.py:424).
- `ponto/services/cartorio.py::contar_celulas_orfas_sem_instantes` — [apagada](ponto/services/cartorio.py:721).
- Testes C18 ajustados e C19 acrescentados: [lista de funções e remoções](docs/lastro_c19_execucao.txt:1095).

| Campos | Quem escreve | Quem lê |
|---|---|---|
| Grade `orfas` / `orfas_instantes` | Matcher, builders e reconciliação: [mapa completo](docs/lastro_c19_execucao.txt:1107) | [Cartório](ponto/services/cartorio.py:737), [adaptador](escala/utils.py:1105), [espelho](ponto/services/espelho.py:860) |
| Ata `orfas` / `orfas_instantes` | [ata_do_dia](ponto/services/cartorio.py:739); porta persiste | [leitor](escala/services/leitor_celula.py:187) |
| Grade lida | [grade_da_celula](escala/services/leitor_celula.py:427) | [adaptador/espelho](ponto/services/espelho.py:704) |
| `orfas_grade` / `orfas_instantes_grade` | [montar_dias](ponto/services/espelho.py:859) | [PDF](relatorios/pdf_espelho.py:176), [tela](templates/ponto/espelho.html:240) |

Certificação Code/Opus, dados de produção, relavratura, DIFF monetário e bytes do PDF: **[nao verificado]**.

_4342 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
